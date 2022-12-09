# Python Argument parser

```python
#!/usrbin/env ptyhon3 

import argparse

if __name__ == "__main__":
    parser=argparse.ArgumentParser(description="Tool for triggering AMI build",formatter_class=argparse.RawTextHelpFormatter)
    
    parser.add_argument("arn",help="ARN of the ImageBuilder Pipeline")
    parser.add_argument("--force",help="Run the without requiring confirmation",default=False,action='store_true')
    parser.add_argument("--number",default=900,help="Optional number. Default %(default)d")
    parser.add_argument("--string",default="/a/path",help="Optional String. Default %(default)s")
    parser.add_argument("--replace", help="Replace pattern and value. Can be use multiple time",
                        nargs=2,metavar=("pattern","replaceWith"),action='append')
    
    
def main(foo,barr):
    [...]
    

if __name__ == "__main__":
    args=parser.parse_args()
    main(args.arn,args.force)
```

Output:
```
./amiBuid.py
usage: amiBuid.py [-h] [--force] [--number NUMBER] [--string STRING] arn
amiBuid.py: error: the following arguments are required: arn
```

```
./amiBuid.py -h
usage: amiBuid.py [-h] [--force] [--number NUMBER] [--string STRING] arn

Tool for triggering AMI build

positional arguments:
  arn              ARN of the ImageBuilder Pipeline

optional arguments:
  -h, --help       show this help message and exit
  --force          Run the wihtout requiring confirmation
  --number NUMBER  Optional number. Default 900
  --string STRING  Optional String. Default /a/path
```
