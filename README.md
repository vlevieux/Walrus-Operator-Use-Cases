# Walrus Operator Use Cases

Walrus Operator (aka Assignement expression) has been added in **Python 3.8**.

> There is new syntax := that assigns values to variables as part of a larger expression. It is affectionately known as “the walrus operator” due to its resemblance to the eyes and tusks of a walrus. [Source: What's New In Python 3.8](https://docs.python.org/3/whatsnew/3.8.html)

## Avoid double calls

### Reuse a value from if statement
```python
if (n:= len(foo)) > 10:
    print(f"foo is too long. Size of foo : {n}")
    
# OR

group = match.group(1) if (match := re.match(data)) else None
```
*Instead of*

```python
if (len(foo)) > 10:
    print(f"foo is too long. Size of foo : {len(foo)}")
```
It is important to note that Walrus operator does not introduce a new scope.

```python
if (n:= len(foo)) > 10:
    print(f"foo is too long.")
print(f"Size of foo : {n}")
```

### Reuse a value in a list

```python
foo = [y:= f(x), y**2, y**3]
```

*Instead of*

```python
foo = [f(x), f(x)**2, f(x)**3]
```

### Reuse a value from a condition in a list comprehension

```python
results = [ y for x in data if (y:=f(x))]
```
*Instead of*

```python
results = []
for x in data:
    result = f(x)
    if result:
        results.append(result)
```

## Avoid nested if statements

```python
if y:= re.match("str1", x):
    print(f"Match the 1st string: {y.match(0)}")
elif y:= re.match("str2", x):
    print(f"Match the 2nd string: {y.match(0)}")
    
# OR USING SHORT CIRCUITING

if y := (re.match('str1', x) or re.match('str2', x)):
    print(f'match: {y.group(0)}')
```
[More info about short-circuiting](https://stackoverflow.com/a/14892812/9515831)

*Instead of*
```python
y = re.match("str1", x)
if y:
    print(f"Match the 1st string: {y.match(0)}")
else:
    y = re.match("str2", x)
    if y:
        print(f"Match the 2nd string: {y.match(0)}")
```

## Read within while-loops (or Do-loops)

```python
while (block := f.read(256)) != '':
    process(block)

# OR

while (command := input("> ")) != "quit":
    print("You entered:", command)
```

*Instead of*

```python
while True:
    stuff()
    if fail_condition:
        break

# OR

stuff()
while not fail_condition():
    stuff()
```
## Use a subtotal value as you go

```python
values = 1, 2, 3, 4
total = 0
[total := total + value for value in values]
# [1, 3, 6, 10]
```

## Use it as a key & as a value in dict declaration

```python
names = "str1 str2", "str3 str4"
{ (n := name.lower()): n.split() for name in names}
# {'str1 str2': ['str1', 'str2'], 'str3 str4': ['str3', 'str4']}
```

## Filter data

```python
filtered_data = [y for x in data if (y := f(x)) is not None]
```
*Instead of*
```python
filtered_data = list(filter(lambda y: y is not None, map(f, data)))
```

# Warning

You should not use Walrus operator in a with statement.

```python
# INVALID, SHOULD BE AVOIDED
with (f := open('file.txt')):
    for l in f:
        print(f)
```

*Keep doing*

```python
with open('file.txt') as f:
    for l in f:
        print(f)
```

**Stay Zen**
> Beautiful is better than ugly.
> Explicit is better than implicit.
> Simple is better than complex.
> [Source: The Zen of Python](https://www.python.org/dev/peps/pep-0020/)

Please read the [PEP 572](https://www.python.org/dev/peps/pep-0572/) for more information about what is valid/invalid.

# How to contribute

To add a use case, please create an issue with the *Propose a Use Case* template. It would be discussed and added.

## Sources:
* [Dustin Ingram - PEP 572: The Walrus Operator - PyCon 2019](https://www.youtube.com/watch?v=6uAvHOKofws)
* [What's Coming in 3 8 Assignment Expressions & More! - Adam Forsyth](https://www.youtube.com/watch?v=OtdQN24Z5MA)
* [“:=” syntax and assignment expressions: what and why?](https://stackoverflow.com/questions/50297704/syntax-and-assignment-expressions-what-and-why)
* [With assignment expressions in Python 3.8, why do we need to use `as` in `with`?](https://stackoverflow.com/questions/51385511/with-assignment-expressions-in-python-3-8-why-do-we-need-to-use-as-in-with)
