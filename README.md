# regex
practice/document syntax and patterns - Lesson from Gordon Zhu @watchandcode.com. Plan is to build toward a password generator.

## Some definitions:
What is a Regular Expression?
  // A concise way to look for patterns in strings.
  
## Regex Form: 
var regex = / *Pattern goes here* / *flags goes here*; 
var regex = new RegExp(/pattern/flags);

## Additional Notes: 
1. If pattern does not match, it'll return a null. 
2. Accepted flags/options: 
  -- g (global - find all matches rather than just stopping at the first.)
  -- i (ignore case)
  -- m (multiline; match the beginning or end of each line)
  -- u (unicode; treat pattern as a sequence of unicode code points)
  -- y (sticky; matches only from the index indicated by the lastIndex property of this regular expression in the target string (and does          not attempt to match from any later indexes)).
3. Relevant methods:
  -- .match()
  -- .replace()


### Examples:
1. 'g'.match(/g/);
2. 'g'.match(/r/); // null
3. 'gg'.match(/g/); // only will match on first occurrence.
4. 'gg'.match(/g/g); // will match all results of 'g'. (ie. 'gggg' - will match 4 individual g's with a length of 4.)
5. 'ag'.match(/g/g); // expect 1 g
6. 'ag'.match(/a|g/g);//match 'a' OR 'g' - without pipe, will match specific pattern of 'ag' (a followed by g).
7. 'abcdefg'.match(/a|b|c|d|e|f|g/g); // will return a length of 7 (a,b,c,d,e,f,g)

#### Above examples can be substituted with a metacharacter:
1. 'abcdefg'.match(/./g); // will return a length of 7 (a,b,c,d,e,f,g)
