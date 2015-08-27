# Regular expression metacharacters

| Operator	| Effect
| --        | --     
|.	| Matches any single character.
|?	| The preceding item is optional and will be matched, at most, once.|
|*	| The preceding item will be matched zero or more times.
|+	| The preceding item will be matched one or more times.
|{N}| The preceding item is matched exactly N times.
|{N,}| The preceding item is matched N or more times.
|{N,M} | The preceding item is matched at least N times, but not more than M times.
|-	| represents the range if it's not first or last in a list or the ending point of a range in a list.
|^	| Matches the empty string at the beginning of a line; also represents the characters not in the range of a list.
|$	| Matches the empty string at the end of a line.
|\b	| Matches the empty string at the edge of a word.
|\B	| Matches the empty string provided it's not at the edge of a word.
|\<	| Match the empty string at the beginning of word.
|\>	| Match the empty string at the end of word.
|?(list) | Matches zero or one occurrence of the given patterns.
|*(list) | Matches zero or more occurrences of the given patterns.
|+(list) | Matches one or more occurrences of the given patterns.
|@(list) | Matches one of the given patterns.
|!(list) | Matches anything except one of the given patterns.

* ^ – Match only if the Following is at the Beginning of the Line
* $ – Match only if the Previous is at the End of the Line
* ? – Match the Previous 0 or 1 times (makes it optional)
* . – Match any Single Character
* * – Match the Previous 0 or More Times
* + – Match the Previous 1 or More Times
* (x) – Matches x and Remembers the match to be called using $1
* (?:x) – Matches x but does Not remember the match
* x(?=y) – Matches x Only if it is followed by y
* x(?!y) – Matches x only if it is NOT followed by y
* x|y – Matches x or y
* [xyz] – Matches any character enclosed in the Brackets. 
Note: This matches X, or Y, or Z; it doesn’t match the string “xyz”.
* [^xyz] – Matches any character Except for those listed in the Brackets.
* \ – “Escape” special characters. Thus \? literally matches * ? not the Syntax Rule of “previous is option”.
* \d \w \s – Matches and Digit (number), Alphabetic Character (letter) or White Space (space bar) respectively.
* \D \W \S – Matches everything But a Digit, Character, or White Space respectively.