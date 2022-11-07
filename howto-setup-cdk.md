# How to setup and deploy CDK environment

Here we will setup and deploy CDK environment in user folder, without impacting the OS wide system packages. 
This do not require `sudo` (root permission) and have the advantage to not impact other users.

## Install nodejs in custom folder 
This rely on `nvm` that will install and let you choose which version of nodejs you want to use.
```
mkdir -p /data/ls/hitri0/nvm

curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.2/install.sh | NVM_DIR="/data/ls/hitri0/nvm" bash
```
Open a new terminal. List and install the desired version:
```
$ nvm list-remote
[...]
       v16.17.1   (LTS: Gallium)
       v16.18.0   (Latest LTS: Gallium)
        v17.0.0
        v17.0.1
[...]

$ nvm install  v16.18.0
$ node -v
```
See also: https://github.com/nvm-sh/nvm#intro

## Install CDK cli in a custom folder
Activate the right node version (this require `nvm` installed as above):
```
nvm use v16
```
Do the install of cdk cli:
```
npm install -g aws-cdk
```

## Install CDK python library in a virtual venv
Craete the virtual venv:
```
python3 -v venv /path/to/venv
source /path/to/venv/bin/activate
pip install --upgrade pip
```
Install the CDK python library into the virtual venv
```
pip install aws-cdk-lib
```

## Configure your AWS credential
cdk cli rely in configuration in `~/.aws` It's the same configuration that aws cli use. 
```
aws configure
```

Or if you want to install manually, edit ~/.aws/credentials` as :
```
[default]
aws_access_key_id = XXXXXX
aws_secret_access_key = yyyyyyyy

[ssam]
aws_access_key_id = ZZZZZ
aws_secret_access_key = zzzzzz
```

Having multiple profile allow you choose which one to use when deploying with cdk

## Bootstrapping
Before you can deploy in a region, you need to run bootstrap at least once:
```bash
cdk bootstrap 

# Or specific region:
cdk bootstrap accountNumber/us-west-2
```
