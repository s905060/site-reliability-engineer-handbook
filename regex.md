# Regular expression metacharacters

| Operator	| Effect |
| --        | --     |
|.	| Matches any single character.|
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

