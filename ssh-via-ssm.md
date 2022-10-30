# Bastion or not Bastion ?

This about the pro and cons of using a bastion/jump host in order to access to an instance in a private subnet

# Context

**With Bastion/hump host:**
* VPC with private and public subnet
* A bastion/jump host instance is running in your public subnet
* An instance that your want to connect to in private subnet
* A S3 path dedicated to hold allowed ssh key. 
* A script on your instance and bastion that periodically update the authorized ssh key list. 

You ssh into the bastion/jump host that is public facing then ssh into your instance. 

Your `~/.ssh/config` look like:
```
Host awsBastion 
    Hostname ec2-13-211-5-42.ap-southeast-2.compute.amazonaws.com
    User ubuntu   
    IdentityFile /key/to/bastion.pem

Host *.ap-southeast-2.compute.internal
    User ubuntu
    ProxyCommand ssh -l ubuntu awsBastion -W %h:%p
    IdentityFile /key/to/instance.pem
```

To connect, you run:
```
ssh xxxx.ap-southeast-2.compute.internal
```

**Without Bastion/Jump host:**
* VPC with private and public subnet
* An instance that your want to connect to in private subnet

You ask AWS Sesion-Manager to estalish a ssh tunnel from your instance to your local machine. Then you ssh into your instance via the tunnel.

Your `~/.ssh/config` look like:
```
Host i-* 
    User ubuntu
    ProxyCommand sh -c "aws ssm start-session --target %h --document-name AWS-StartSSHSession --parameters 'portNumber=%p'"
    IdentityFile /key/to/instance.pem
```

To connect, you run:
```
AWS_PROFILE=aws-cli-profile ssh i-014322b06bb5e5f76
```

# Pro using SSM (without bastion/jump host)
* More secure: 
  * you do not have a bastion/jump host exposed to internet
  * minor: your instance can have a Security-group that is fully closed: not inbound port is needed. Not even port 22. 
  But your instance is already in a private subnet so this do not add much more security
* More access trace and log: every time a ssh connection is opened, a SSM `StartSession` is created and will be logged the CloudTrail, 
  with the IAM user (and the access key) attached to it. Using bastion, only the syslog in the bastion will have trace, all under ubuntu user. 
* No bastion instance to manage. 
* No S3 folder containing individuals ssh key (and the permission around it)
* No custom script to download and update authorized keys

## Different way to controll who is allowed to ssh into your private instance
The list of user allowed to access to your private instance is defined by :
* In the case **with bastion**: the list of people allowed to drop their ssh key into the S3 ssh key folder.
* In the case **with ssm**: the list of IAM user with the permission``ssm:StartSession` and having access to the ssh key embedded to your instance.

# Cons using SSM
* Your local device, from which you want to ssh to your instance, needs : 
  * ```aws cli``` installed.
  * ```Session Manager plugin``` : see https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager-working-with-install-plugin.html
