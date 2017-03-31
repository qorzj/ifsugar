## Overview
#### motivation
The purpose of ifsugar is to make python source code be ++easier understood by human++.

#### Install
ifsugar is only supported by ++python3.5 or above++, because the matrix multiplication operator: `a @ b` is not available in lower python version.

`pip3 install ifsugar`

#### Example
```
from ifsugar import _if, _try, _times
# or you can: from ifsugar import *

if __name__ == "__main__":
        if "test _if":
        x = 42
        x @= 24 @_if([1, 2, 3])
        assert x == 24
        x @= 42 @_if({})
        assert x == 24

    if "test _try":
        x = None
        with _try:
            x = 3 / 0
        assert x is None
        with "successful code" @_try:
            y = -9
        assert y == -9

    if "test _times":
        n = []
        m = list(n.append(0) or len(n) for _ in 5 @_times)
        assert m == [1, 2, 3, 4, 5]
        for _ in 3 @_times:
            m.pop()
            n.pop()

        assert m == [1, 2] and n == [0, 0]
        
```

## Sugar: _if
```
x @= A @_if(B)
```
is equivalent to `x = A if B else x` or `if B: x = A`
#### Motivation
When reading a source code, it is so important to know ++what you are going to do++ **BEFORE** ++how to do++.

That is why [Ruby supports](https://www.tutorialspoint.com/ruby/ruby_if_else.htm) `code if condition` and `code unless conditional` syntax. For example:
```
#!/usr/bin/ruby
$var =  1
print "1 -- Value is set\n" if $var
print "2 -- Value is set\n" unless $var
```

Since `unless` is not intuitive (my personal opinion), ifsugar doesn't adopt `_unless`

#### Limitation
both `x` and `A` in `x @= A @_if(B)` cannot be Matrix type.

## Sugar: _try
```
with _try:
    ...code...
    
```
is equivalent to
```
try:
    ...code...
except:
    pass
    
```

#### _try with comment
```
with "I am comment" @_try:
    ...code...
    
```
is equivalent to
```
if "I am comment":
    try:
        ...code...
    except:
        pass
    
```

## Sugar: _times
```
for _ in n @_times:
    ...code...
```
is equivalent to
```
import itertools
for _ in itertools.repeat(None, n):
    ...code...
```

i.e. `n @_times` is equivalent to `itertools.repeat(None, n)`
