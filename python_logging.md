# Python log facility

```python
import logging,sys
logging.basicConfig(format='%(asctime)s | %(module)s.py : %(message)s', 
                    stream=sys.stdout, 
                    level=logging.INFO,
                    datefmt='%Y-%m-%d %H:%M:%S')
LOGGER = logging.getLogger(__name__)

# [...]

LOGGER.info(f"blalbalba {foobar} ...")
```
