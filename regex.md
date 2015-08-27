### Matching a Username
```
/^[a-z0-9_-]{3,16}$/
```

Description:

We begin by telling the parser to find the beginning of the string (^), followed by any lowercase letter (a-z), number (0-9), an underscore, or a hyphen. Next, {3,16} makes sure that are at least 3 of those characters, but no more than 16. Finally, we want the end of the string ($).

String that matches:

my-us3r_n4m3

String that doesn't match:

th1s1s-wayt00_l0ngt0beausername (too long)


### Matching a Password
```
/^[a-z0-9_-]{6,18}$/
```

Pattern:

/^[a-z0-9_-]{6,18}$/
Description:

Matching a password is very similar to matching a username. The only difference is that instead of 3 to 16 letters, numbers, underscores, or hyphens, we want 6 to 18 of them ({6,18}).

String that matches:

myp4ssw0rd

String that doesn't match:

mypa$$w0rd (contains a dollar sign)


### Matching a Hex Value

Pattern:
```
/^#?([a-f0-9]{6}|[a-f0-9]{3})$/
```
Description:

We begin by telling the parser to find the beginning of the string (^). Next, a number sign is optional because it is followed a question mark. The question mark tells the parser that the preceding character — in this case a number sign — is optional, but to be "greedy" and capture it if it's there. Next, inside the first group (first group of parentheses), we can have two different situations. The first is any lowercase letter between a and f or a number six times. The vertical bar tells us that we can also have three lowercase letters between a and f or numbers instead. Finally, we want the end of the string ($).

The reason that I put the six character before is that parser will capture a hex value like #ffffff. If I had reversed it so that the three characters came first, the parser would only pick up #fff and not the other three f's.

String that matches:

**#a3c113**

String that doesn't match:

**#4d82h4 (contains the letter h)**