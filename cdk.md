# Tag 

## Tagging all resources
This allow you to tag recursively all resource in a stack
```python
app = cdk.App()
myStack = AccessStack(app,"access-stack",env=env)

# Do the taggin
cdk.Tags.of(app).add("deployedBy",awsUser)
cdk.Tags.of(app).add("gitUrl",gitRepoName)

```

## Tag by the user who deploy it
We retrieve the username using `boto3` before populating the tags
```python
# Get the username who deploy 
awsUser = boto3.client('sts',).get_caller_identity()['Arn'].split(':')[5]

# Then tag
cdk.Tags.of(app).add("deployedBy",awsUser)
```
Note: if you are deploying using a different profile than default, do not use `cdk --profile myProfile` as this is not pass on to boto3. 
Use the environment variable `AWS_PROFILE` instead. `boto3` by default check for this variable if defined and use it to retrieve the right credentials. 

Using profile, your deployment command will look like: 
```bash
AWS_PROFILE="myProfile" cdk deploy my-stack-name
```

## Tag with a git repo url
```python
# Dynamically retrieve the git repo name
def getRepoName():
    process = subprocess.Popen(['git', 'config', '--get','remote.origin.url'], 
                           stdout=subprocess.PIPE,
                           universal_newlines=True)
    gitRepoName=process.stdout.readline().strip()
    if len(gitRepoName) < 1:
        gitRepoName="Failed to retrieve git repo url."
    
    return gitRepoName

cdk.Tags.of(app).add("gitUrl",getRepoName())
```
This requires that you are working in a folder or subfolder covered by git

# Dynamic versioning
Some resource (eg Image Builder component, Recipe ...) need a version number to be provided. 
* You hard code a version like `1.0.0`.  
* Make change to your stack and deploy/update
* If the resource need to be replaced: it will fail ! Because to replace, it will first create a new resource but will clash with the same hard coded version `1.0.0`

Work-around: dynamic version with timestamp
```python
def getDatetimeVersion(self):
    now=datetime.now()
    version=now.strftime("%Y%m%d.%H%M.%S")
    return version
```
**Cons**: everytime your do an update of your stack, a new resource (and version number) is created, even if the resource did not change !
