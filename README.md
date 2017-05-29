
*Goal: To practice/document regex syntax and patterns - Lesson from Gordon Zhu @watchandcode.com.*

## What is a Regular Expression?
 1. A concise way to look for patterns in strings.
 2. Creating a regular expression:  
  ```
   var regex = / *Pattern goes here* / *flags goes here*;
   var regex = new RegExp(/*pattern*/,*flags*); 
  ```
__Notes/Highlights:__ 
1. If pattern does not match, it'll return a null.
2. metacharacters:
   * x|y Pipe matches x OR y. 
   * . Matches the preceding subexpression zero or more times.
   * [xyz] A 'set' or 'character set'. Matches any one of the enclosed characters or 'range' in a set - [a-z] is same as [abcdefgh...z]
     * can combine ranges in sets and ranges with individual characters in sets.
   * [^xyz] A carat within a set matches the opposite of the enclosed range (if a-z, will match on anything except lowercase letters).
   * {} is known as a quantifier - which is used to look for consecutive matches of any length. 
     * {min, max}
     * {1,20} - 1 letter up to 20 times.
     * {1,} - matches 1 or more ad infinitum... (shortcut is known as +)
     * {n} - matches 'n' consecutive digits (i.e [0-9]{3} matches '123' in '12345')
   * x+ or []+ matches the preceding item 1 or more times. (same as {1,})
   * \d is a shortcut for digits ([0-9]) that makes the expression a little shorter.
   * $& - references the whole matched string.
3. Accepted flags/options: 
   * g (global - find all matches rather than just stopping at the first.)
   * i (ignore case)
   * m (multiline; match the beginning or end of each line)
   * u (unicode; treat pattern as a sequence of unicode code points)
   * y (sticky; matches only from the index indicated by the lastIndex property of this regular expression in the target string (and does      not attempt to match from any later indexes)).
4. Relevant methods:
   * .match()
   * .replace() - will look for the pattern and will replace with the value passed as the second argument.
   * .split()
   * .search()


### Exercise 1: Grading Passwords 
1. #### Get number of total characters (any kind)
```
1. 'g'.match(/g/); // will match 'g' with a length of 1.
2. 'g'.match(/r/); // null
3. 'gg'.match(/g/); // will only match on first occurrence, length of 1.
4. 'gg'.match(/g/g); // will match all results of 'g'. (ie. 'gggg' - will match 4 individual g's with a length of 4.)
5. 'ag'.match(/g/g); // expect 1 g, length of 1
6. 'ag'.match(/a|g/g);//match 'a' OR 'g' - without pipe, will match specific pattern of 'ag' (a followed by g) - length of 2.
7. 'abcdefg'.match(/a|b|c|d|e|f|g/g); // will return a length of 7 (a,b,c,d,e,f,g)

// above examples can be substituted with a metacharacter and will fulfill our goal:
1. 'abcdefg'.match(/./g); // will return a length of 7 (a,b,c,d,e,f,g)

// confirm all characters, case insensitive, numbers and special characters work.
2. 'abcdefgABC123#% '.match(/./g); // will match on ALL characters and return a length of 16
```
2. #### Get number of lowercase letters
```
// count number of lowercase characters using a 'set'.
// a-z is range in set []
'abCD12%&'.match(/[a-z]/g); // expect a,b
```
3. #### Get number of uppercase letters
```
// A-Z is range in set []
'abAB'.match(/[A-Z]/g); // expect only uppercase letters A,B
```
4. #### Get number of digits
```
// 0-9 is range in set [] (can combine ranges in sets and ranges with individual characters in sets.)
'1abA3b1'.match(/[0-9]/g); // expect only digits to be returned - [1,3,1]
```
5. #### Get number of special characters (anything not a letter or digit)
```
// we want to match anything that is NOT a lowercase/uppercase letter or digit in the set.
// ''.match(/[a-zA-Z0-9]/g); -- would match all lowercase/uppercase letters or digits.

// we would want the OPPOSITE.
'aA1!@#$%^&*()-'.match(/[^a-zA-Z0-9]/g); // a carat within a set says get the opposite. - expect !@#$%^&*()- in set
```
6. #### Get a series of consecutive letters. 
```
// current solution will only match single characters.
'DONT!be*a^framework-chaser'.match(/[a-zA-Z]/g); 

// we want to match consecutive characters, so we need to use quantifiers.
'DONT!be*a^framework-chaser'.match(/[a-zA-Z]+/g); // can also add case insensitive flag (i) ie. .match(/[a-z]+/gi)

```

### Exercise 2: Phone Number Obfuscator
*Transform a number like '555-666-7777' to 'XXX-XXX-7777'*
1. #### Find all numbers in the range [0-9]
```
'343-555-9999'.match(/[0-9]/g) // matches: ["3", "4", "3", "5", "5", "5", "9", "9", "9", "9"]
```
2. #### Use a quantifier to match 3 consecutive digits and hyphen at end of each set.
```
'343-555-9999'.match(/[0-9{3}-]/g) // matches: ["343-", "555-"]
```
3. #### Use a quantifier to match 4 consecutive digits.
```
'343-555-9999'.match(/[0-9{3}-[0-9]{3}-[0-9]{4}/g) // ["343-555-9999"]
```
4. #### Capture the 4 consecutive digits
```
// to capture a group - add parenthesis around the set.
// reference to the set is with $n - n being numerically sequential (1,2,3)
'343-555-9999'.match(/[0-9{3}-[0-9]{3}-([0-9]{4})/g) // ["343-555-9999"]
```
5. #### Replace the first 6 digits with 'XXX-XXX-'
```
// \d - shorthand for digits. (works the same as [0-9])
// $1 captures the first group in the regular expression wrapped in parenthesis.
var num = '343-555-9999'.replace(/[\d]{3}-[\d]{3}-([\d]{4})/g, 'XXX-XXX-$1'); // "XXX-XXX-9999"

```
