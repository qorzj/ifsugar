## Overview
#### motivation
The purpose of ifsugar is to make python source code be ++easier understood by human++.

#### Install
ifsugar is only supported by ++python3.5 or above++, because the matrix multiplication operator: `a @ b` is not available in lower python version.

`pip3 install ifsugar`

#### Example
```
from ifsugar import _nil, _if, _get, _try, _times
if __name__ == "__main__":
    if "test _if":
        x = 42
        x @= 24 @_if([1, 2, 3])
        assert x == 24
        x @= 42 @_if({})
        assert x == 24

    if "test _try & _get":
        del x
        with _try:
            x = 3 / 0
        x = _get(lambda: x)
        assert x is _nil
        with "successful code" @_try:
            y = -9
        x = _get(lambda: abs(y))
        assert x == 9

    if "test _times":
        n = []
        foo = lambda x: x.append(0) or str(len(x))
        assert ''.join(5 @_times(lambda: foo(n))) == '12345'
```

## Sugar: _if
`x = A @_if(B)` is equivalent to `x = A if B else x` or `if B: x = A`
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
In `x = A @_if(B)`, both `x` and `A` cannot be Matrix type.

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

## Sugar: _get & _nil
```
x = _get(func)
```
is equivalent to:
```
try:
    x = func()
except:
    x = _nil
```

## Sugar: _times
```
n @_times(func)
```
is equivalent to
```
(func() for _ in range(n))
```
