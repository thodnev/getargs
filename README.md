# getargs

Yup, I know about all that full-blown *argparse*, *getopt* and *optparse* modules pre-included in standard lib.

This one is really lightweight and simple.

```python
'''
A simple command-line arguments parser

Generally accepts the next arguments patterns:
  --mykey=myvalue    =>    {'mykey': 'myvalue', ...}
  --someswitch       =>    {'someswitch': True, ...}
  switch1 switch2    =>    {None: ['switch1', 'switch2'], ...}

Where -- and = are KEY and SEP module attributes respectively, could
be set to something completely different.
'''

import sys

# See module description above
KEY = '--'
SEP = '='

def getargs(argv=sys.argv[1:]):
    # --file=somefile_with=in_name is error
    resdict = {None: []}
    for arg in argv:
        if arg.startswith(KEY):
            split = arg[len(KEY):].split(SEP)
            key, value = split[0], split[1:]
            if len(value)>1:
                raise ValueError('Too many "{}" in argument: {}'
                                 .format(SEP, arg))
            resdict[key] = True if not value else value[0]
        else:
            resdict[None].append(arg)
            
    if not resdict[None]:
        del resdict[None]
        
    return resdict


# Self-test
if __name__ == '__main__':
    inpargs = ['--food=spam --ba=boon this is empty',
               '--this=one --without=empty',
               #'--this=one=generates=error=too=many==='
               ]
    for inp in inpargs:
        print(getargs(inp.split()) )
```
