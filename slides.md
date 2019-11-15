Code golf

---

# Outline

1. Introduction
2. Problem 1
3. Problem 2
4. Problem 3

---

# Why golf?

1. Improve problem solving skills
2. Deepen mastery of language
3. Train very specific skills (e.g. regexes, bit twiddling)

---

# Example: Alphabet triangle

**Output**

A
AB
ABC
ABCD
ABCDE
...
ABCDEFGHIJKLMNOPQRSTUVWXYZ

---

# Example: Alphabet triangle

```python (40 bytes)

s=""
exec"s+=chr(len(s)+65);print s;"*26
```

---

# Example: Alphabet triangle

```sh (25 bytes)

eval s+={A..Z}';echo $s;'
```

---

# Example: NATO phonetic alphabet

**Input**

Alpha Bravo Charlie Delta Echo Foxtrot Golf

**Output**

ABCDEFG

---

# Example: NATO phonetic alphabet

```c (44 bytes)

c;main(i){main(write(read(0,&c,1)*i)+c<33);}
```

---

# Example: Missing number

**Input**

5               // n
2 3 1 5         // numbers from 1 to n, except 1

**Output**

4

---

# Example: Missing number

```ruby (42 bytes)

p eval "[*1..#{[*$<]*"].sum "}".tr ' ', ?-
```

---

# Example: Missing number


```
p eval "[*1..#{[*$<]*"].sum "}".tr ' ', ?-
```

---

# Your turn!

3 problems

For each problem, the best solution in python wins 5 bucks

1. Download inputs from https://github.com/xsot/FH186
2. When the time is up, create a PR with your solution

---

# Problem 1: Rock paper scissors

(original: http://golf.shinh.org/p.rb?RPS+winner)

**Input**               **Output**

Rock Rock           Rock
Rock Paper          Paper
Rock Scissors       Rock
Paper Rock          Paper
Paper Paper         Paper
Paper Scissors      Scissors
Scissors Rock       Rock
Scissors Paper      Scissors
Scissors Scissors   Scissors

---

```python

while 1:s=raw_input();print s.split()[hash(s)/6%2]
```

---

# Problem 2: Divide by power of 10

(original: http://golf.shinh.org/p.rb?Decimal+Point+Fixed)
(inspired by: https://open.kattis.com/problems/divideby100)

**Input**        **Output**

1230 0       1230
1230 1       123
1230 2       12.3
1230 3       1.23
1230 4       0.123
1230 5       0.0123
1230 6       0.00123

---

```python (92 bytes)

while 1:a,b=map(int,raw_input().split());print('%%d.%0*d'%(b,a%10**b)).strip('.0')%(a/10**b)
```

---

# Problem 3: Variables

(original: http://golf.shinh.org/p.rb?variables+fixed)

**Input**     **Output**

e=7       7=e
f=7       f=4
?=e
4=f
f=?

---

for each line
    if has '?'
        print line with '?' replaced
    else
        set variable

---

```python

while 1:
    s = raw_input()
    d = {}
    if '?' in s:
        if 'a' <= s[0] <= 'z':
            print s.replace('?', d[s[0]])
        else:
            print s.replace('?', d[s[2]])
    else:
        if 'a' <= s[0] <= 'z':
            d[s[0]] = s[2]
        else:
            d[s[2]] = s[0]
```

---

```python

d = {}
while 1:
    s = raw_input()
    if 'a' <= s[0] <= 'z':
        i = 0
        j = 2
    else:
        i = 2
        j = 0
    if '?' in s:
        print s.replace('?', d[s[i]])
    else:
        d[s[i]] = s[j]
```

---

/--
we don't actually need two indices, just derive one from the other
--/

```python

d = {}
while 1:
    s = raw_input()
    if 'a' <= s[0] <= 'z':
        i = 0
    else:
        i = 2
    if '?' in s:
        print s.replace('?', d[s[i]])
    else:
        d[s[i]] = s[2-i]
```

---

/--
if statements are bad, so let's get rid of that one
there's still another one but it's not easy to remove. we'll get back to it later
--/

```python

d = {}
while 1:
    s = raw_input()
    i = ('a' <= s[2] <= 'z') * 2
    if '?' in s:
        print s.replace('?', d[s[i]])
    else:
        d[s[i]] = s[2-i]
```

---

/--
surely there's a better way to perform the assignments
--/

```python

while 1:
    s = raw_input()
    i = ('a' <= s[2] <= 'z') * 2
    if '?' in s:
        print s.replace('?', `eval(s[i])`)
    else:
        exec s[::1-i]
```

---

/--
hey, why not just exec everywhere?
--/

```python

while 1:
    s = raw_input()
    i = ('a' <= s[2] <= 'z') * 2
    if '?' in s:
        exec "print s.replace('?', `%s`)" % s[i]
    else:
        exec s[::1-i]
```

---

/--
which lets us remove the if statement
--/

```python

while 1:
    s = raw_input()
    i = ('a' <= s[2] <= 'z') * 2
    exec [
        s[::1-i],
        "print s.replace('?', `%s`)" % s[i]
    ]['?' in s]
```

---

```python (93 bytes)

while 1:s=raw_input();i=('`'<s[2])*2;exec[s[::1-i],"print s.replace('?',`%s`)"%s[i]]['?'in s]
```

/--
can we do better? let's undo the minification
--/

---

```python

while 1:
    s = raw_input()
    i = ('`' < s[2]) * 2
    exec [
        s[::1-i],
        "print s.replace('?', `%s`)" % s[i]
    ]['?' in s]
```

/--
computing i seems wasteful, let's remove it!
--/

---

```python

while 1:
    s = raw_input()
    exec [
        max(s, s[::-1]),
        "print s.replace('?', `%s`)" % max(s)
    ]['?' in s]
```

/--
This works since: digits < equals < lowercase letters
--/

---

```python (86 bytes)

while 1:s=raw_input();exec[max(s,s[::-1]),"print s.replace('?',`%s`)"%max(s)]['?'in s]
```

---

/--
take a step back and see what we're trying to do
exec depending on case:
--/

    a=?    ->    "print ..."
    ?=a    ->    "print ..."
    a=0    ->    "a=0"
    0=a    ->    "a=0"

/-- any other way to perform this mapping? --/

---

If only we could do something like:

```python

max(s, s[::-1], x)
```

Which gives us this set of inequalities:

    a=?    ->    "?=a", "a=?" < x
    ?=a    ->    "?=a", "a=?" < x
    a=0    ->    "0=a", x     < "a=0"
    0=a    ->    "0=a", x     < "a=0"

where x = "a=..." /-- which is not what we want --/

---

/--
if we try min, we can't even get the correct string between s and s[::-1]
but wait, it's correct if we reverse it again
let's see where this takes us
--/

    min(s, s[::-1], x)[::-1]

    a=?    ->    x     < "?=a", "a=?"
    ?=a    ->    x     < "?=a", "a=?"
    a=0    ->    "0=a" < x,     "a=0"
    0=a    ->    "0=a" < x,     "a=0"

---

    min(s, s[::-1], x)[::-1]

    a=?    ->    x     < "?=a", "a=?"
    ?=a    ->    x     < "?=a", "a=?"
    a=0    ->    "0=a" < x,     "a=0"
    0=a    ->    "0=a" < x,     "a=0"

this seems to work, as long as

    "0=a" < x < "?=a"
    x[::-1] = "print ..."

---

    min(s, s[::-1], x)[::-1]

    a=?    ->    x     < "?=a", "a=?"
    ?=a    ->    x     < "?=a", "a=?"
    a=0    ->    "0=a" < x,     "a=0"
    0=a    ->    "0=a" < x,     "a=0"

this seems to work, as long as

    "0=a" < x < "?=a"
    x[::-1] = "print ..."

but

    x = ")`a`,'?'(ecalper.s tnirp" < "0=a"

---

/--
luckily there's a fix! '0' < ';' < '?' so just add a semicolon
--/
```python

while 1:
    s = raw_input()
    exec min(
        s,
        s[::-1],
        ";)`%s`,'?'(ecalper.s tnirp" % max(s)
    )[::-1]
```

/-- There's just a slight problem, can anyone spot it? --/

---

/--
doesn't work if 's' is used as a variable name
so just change to uppercase or underscore
--/

```python (83 bytes)

while 1:_=raw_input();exec min(_,_[::-1],";)`%s`,'?'(ecalper._ tnirp"%max(_))[::-1]
```

/--
81 bytes is possible, using a different (and less convoluted) strategy
--/

