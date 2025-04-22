## Flex Regex vs. Standard Regex

Regular expressions in Flex (a lexical analyzer generator) have some important differences from standard regex implementations (like those found in JavaScript, Python, or PCRE). Understanding these differences is crucial when writing a scanner:

| Feature | Flex Regex | Standard Regex | Example |
|---------|------------|----------------|---------|
| **Start Conditions** | Supports states with `%s`, `%x` and `BEGIN` | No equivalent concept | Flex: `<STRING>\"  { BEGIN INITIAL; }` |
| **Definition Substitution** | Supports named pattern definitions | No built-in support | Flex: `DIGIT [0-9]` then `{DIGIT}+` |
| **End-of-File Handling** | Special `<<EOF>>` pattern | No direct equivalent | Flex: `<<EOF>> { /* EOF action */ }` |
| **Quoted Strings** | Double quotes mean literal text | Must escape special chars | Flex: `"+"` vs Standard: `\+` |
| **Trailing Context** | `/` operator for following context | Uses lookahead assertions | Flex: `"abc"/("def")` vs Standard: `/abc(?=def)/` |
| **POSIX Classes** | Full support for `[[:alpha:]]` etc. | Limited support | Flex: `[[:digit:]]+` vs Standard: `/\d+/` |
| **State-Based Matching** | Exclusive/inclusive states with `%x`/`%s` | No equivalent | Flex: `<COMMENT>` patterns only match in COMMENT state |
| **Pattern Priority** | Longest match wins, then earliest rule | Various prioritization rules | Flex: `"=="` takes precedence over `"="` when both match |
| **No Capturing Groups** | No support for `()` as capturing groups | Full support for captures | Standard: `/(abc)def/` - captures "abc" |
| **No Look-Behind** | Not supported | Supported in many engines | Standard: `/(?<=\$)\d+/` matches# Regular Expressions Cheat Sheet

## Important Note on Regex Matching

When discussing regular expressions, it's crucial to understand the difference between **full matching** and **partial matching**:

- **Partial matching**: A regex pattern matches any part of a string. For example, the pattern `cat` partially matches "concatenate" because "cat" appears somewhere in the string.

- **Full matching**: A regex pattern matches the entire string from beginning to end. For example, the pattern `cat` fully matches only the string "cat" and nothing else.

In most regex implementations, patterns perform partial matching by default. To force full matching, you typically need to use anchors:
- `^` at the beginning of the pattern (matches start of string/line)
- ` at the end of the pattern (matches end of string/line)

So to fully match only "cat", you would use the pattern `^cat.

Examples to illustrate the difference:

| Pattern | String | Partial Match? | Full Match? |
|---------|--------|----------------|-------------|
| `cat` | "cat" | Yes | Yes |
| `cat` | "concatenate" | Yes | No |
| `cat` | "bobcat" | Yes | No |
| `^cat` | "cat" | Yes | Yes |
| `^cat` | "concatenate" | No | No |
| `^cat` | "bobcat" | No | No |
| `cat | "cat" | Yes | Yes |
| `cat | "concatenate" | No | No |
| `cat | "bobcat" | Yes | No |
| `^cat | "cat" | Yes | Yes |
| `^cat | "cats" | No | No |

In this cheat sheet, the examples provided in the "Example Matches" column demonstrate cases where the pattern completely matches the indicated portion of the text.

## Basic Expressions

| Pattern | Matches                | Example Matches   | Non-Matches   |
| ------- | ---------------------- | ----------------- | ------------- |
| `a`     | The character 'a'      | "a" in "apple"    | "b", "A"      |
| `x`     | The character 'x'      | "x" in "fox"      | "y", "X"      |
| `abc`   | The exact string 'abc' | "abc" in "abcdef" | "abd", "ABC"  |
| `123`   | The exact string '123' | "123" in "12345"  | "124", "1234" |
| `\n`    | A newline character    | Line break        | Space, tab    |

*A string matches itself*

## Character Classes

| Pattern | Matches | Example Matches | Non-Matches |
|---------|---------|-----------------|-------------|
| `[abc]` | Any one of the characters 'a', 'b', or 'c' | "a" in "apple", "b" in "bee", "c" in "cat" | "d", "A" |
| `[123]` | Any one of the characters '1', '2', or '3' | "1" in "123", "2" in "2022", "3" in "30" | "4", "0" |
| `.` | Any character except newline | "a", "B", "3", "*" | newline |

## Character Ranges

| Pattern | Matches | Example Matches | Non-Matches |
|---------|---------|-----------------|-------------|
| `[a-z]` | Any lowercase letter | "a" in "apple", "z" in "zebra" | "A", "0", "_" |
| `[A-Z]` | Any uppercase letter | "A" in "Apple", "Z" in "ZERO" | "a", "0", "_" |
| `[0-9]` | Any digit | "1" in "123", "9" in "9th" | "a", "A", "_" |
| `[ak-nx]` | 'a', 'k', 'l', 'm', 'n', or 'x' | "a" in "apple", "m" in "mom" | "b", "j", "o" |
| `[a-zA-Z]` | Any letter (uppercase or lowercase) | "a" in "apple", "Z" in "ZERO" | "0", "_", "." |
| `[a-zA-Z0-9]` | Any letter or digit | "a" in "apple", "Z" in "ZERO", "7" in "7th" | "_", ".", "!" |

## Negated Character Classes

| Pattern  | Matches                                       | Example Matches                   | Non-Matches |
| -------- | --------------------------------------------- | --------------------------------- | ----------- |
| `[^A-Z]` | Any character that is NOT an uppercase letter | "a" in "apple", "1" in "123", "*" | "A", "Z"    |
| `[^\n]`  | Any character that is NOT a newline           | "a", "B", "3", "*"                | newline     |

## Operators

| Pattern | Matches                                                       | Example Matches                        | Non-Matches              |
| ------- | ------------------------------------------------------------- | -------------------------------------- | ------------------------ |
| `+`     | 1 or more repetitions of the preceding pattern                | "aaa" in "aaabc" (using `a+`)          | "" (empty string)        |
| `*`     | 0 or more repetitions of the preceding pattern                | "", "a", "aaa" (using `a*`)            | N/A (matches everything) |
| `?`     | 0 or 1 repetition of the preceding pattern (optional)         | "", "a" (using `a?`)                   | "aa"                     |
| `{n}`   | Exactly n repetitions                                         | "aaa" (using `a{3}`)                   | "aa", "aaaa"             |
| `{n,m}` | Between n and m repetitions (inclusive)                       | "aa", "aaa", "aaaa" (using `a{2,4}`)   | "a", "aaaaa"             |
| `{n,}`  | n or more repetitions                                         | "aaa", "aaaa", "aaaaa" (using `a{3,}`) | "a", "aa"                |
| `\|`    | Alternation - matches either expression on left OR right side | "cat", "dog" (using `cat\|dog`)        | "rat", "mouse"           |

## Examples of Operators

| Pattern                 | Matches                                                  | Example Matches             | Non-Matches            |
| ----------------------- | -------------------------------------------------------- | --------------------------- | ---------------------- |
| `[a-z]+`                | Any word written in lowercase letters (1 or more)        | "apple", "zebra", "hello"   | "", "Apple", "123"     |
| `[0-9]+`                | Any positive integer number (possibly with preceding 0s) | "123", "0", "01234"         | "", "abc", "12.3"      |
| `[a-z]*`                | Zero or more lowercase letters                           | "", "a", "hello"            | "Hello", "123"         |
| `[a-z]?`                | Zero or one lowercase letter                             | "", "a", "z"                | "ab", "ABC"            |
| `[A-Z]{4}`              | Exactly 4 uppercase letters                              | "ABCD", "NASA"              | "ABC", "ABCDE", "abcd" |
| `[A-Z]{2,4}`            | 2, 3, or 4 uppercase letters                             | "AB", "ABC", "ABCD"         | "A", "ABCDE", "abc"    |
| `[1-9][0-9]{2,}`        | Any positive integer with 3 or more digits               | "100", "1234", "9999"       | "12", "099", "abc"     |
| `cat\|dog`              | Either "cat" or "dog"                                    | "cat", "dog"                | "cats", "dogs", "rat"  |
| `(jpg\|jpeg\|png\|gif)` | Any common image file extension                          | "jpg", "jpeg", "png", "gif" | "pdf", "tiff", "jpe"   |

## Pattern Matching in Context

| Pattern | Matches | Example Matches | Non-Matches |
|---------|---------|-----------------|-------------|
| `^[0-9]` | A digit at the beginning of a line | "1" in "123\nabc" | "a" in "a123", "1" in "a\n123" |
| `[0-9]$` | A digit at the end of a line | "3" in "123\nabc" | "3" in "123a", "c" in "abc\n123" |
| `[0-9]/[0-9]` | A digit followed by '/' then another digit | "1/2" in "ratio is 1/2" | "1 / 2", "1.2" |

## Common Patterns

| Pattern | Matches | Example Matches | Non-Matches |
|---------|---------|-----------------|-------------|
| `-[1-9][0-9]*` | Any negative integer | "-1", "-123", "-9876" | "0", "123", "-0", "+1" |
| `-?[1-9][0-9]*` | Any non-zero integer (positive or negative) | "1", "-1", "123", "-456" | "0", "01", "-0" |
| `[a-zA-Z][a-zA-Z0-9_]*` | Valid identifier | "x", "var1", "my_variable", "AbC123" | "1var", "_var", "my-var" |
| `yes\|no\|maybe` | One of these three specific options | "yes", "no", "maybe" | "Yes", "nope", "perhaps" |
| `(Sun\|Mon\|Tue\|Wed\|Thu\|Fri\|Sat)` | Any day of the week abbreviation | "Sun", "Mon", "Tue" | "Sunday", "sun", "Monday" |

## Using Regex in Flex

```c
%%
[0-9]+       return tNUMBER;  /* caught a number */
"+"          return tADD;     /* caught an addition */
"-"          return tSUB;     /* caught a subtraction */
[ \t\n]+     /* eat up whitespace */
.            { return yytext[0]; }  /* pass other characters */
%%
```

## Literal Text Matching in Flex

There are two ways to match literal text in flex:

1. **Using quotes** - Match the exact characters between quotes:
   ```c
   "while"      return tWHILE;   /* matches the keyword "while" */
   "=="         return tEQUALITY; /* matches the equality operator */
   "a+b"        return tEXPR;    /* matches literal "a+b", not regex */
   ```

2. **Escaping special characters** - Escape each special regex character with a backslash:
   ```c
   while        return tWHILE;   /* same as "while" */
   ==           return tEQUALITY; /* ERROR - this is not valid regex */
   a\+b         return tEXPR;    /* same as "a+b" */
   ```

When to use quotes:
- For strings containing regex special characters (`+`, `*`, `.`, `[`, `]`, `|`, etc.)
- For multi-character operators (`==`, `!=`, `<=`, etc.)
- For clarity when matching exact text sequences

Example comparing both approaches to match C++ array notation:
```c
"a[i+1]"     return tARRAY_ACCESS;  /* Clean, readable */
a\[i\+1\]    return tARRAY_ACCESS;  /* More error-prone */
```

For simple keywords without special characters, either approach works:
```c
"if"         return tIF;  /* With quotes */
if           return tIF;  /* Without quotes - same result */
```

## Building a More Complex Pattern

To match a valid identifier (starts with a letter, followed by letters, digits, or underscores):
```c
[a-zA-Z][a-zA-Z0-9_]*
```
Example matches: "variable", "x1", "myVar_123"

To match a valid email address (simplified version):
```c
[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}
```
Example matches: "user@example.com", "john.doe123@mail.university.edu"

To match a date in DD.MM.YYYY format:
```c
[0-9]{2}\.[0-9]{2}\.[0-9]{4}
```
Example matches: "01.01.2025", "31.12.2024", "25.02.2023"

## Special Notes

1. Inside square brackets `[]`, the meaning of some characters changes:
   - `-` inside brackets denotes a range (e.g., `[a-z]` matches any lowercase letter)
   - `-` outside brackets is just the minus character (e.g., `a-z` matches the literal string "a-z")

2. To use a literal special character, escape it with backslash:
   - `\.` matches a literal period (e.g., "example\.com" matches "example.com" but not "exampleXcom")
   - `\+` matches a literal plus sign (e.g., "1\+1" matches "1+1" but not "11")
   - `\\` matches a literal backslash (e.g., "C:\\Program" matches "C:\Program")

3. The caret `^` has different meanings:
   - At the start of a pattern: matches beginning of line (e.g., `^Hello` matches "Hello" at the start of a line)
   - Inside brackets as first character: negates the character class (e.g., `[^0-9]` matches any non-digit)

4. Using quotes in flex:
   - Double quotes (`"keyword"`) tell flex to match the exact text literally
   - This is particularly useful for text containing regex special characters
   - Without quotes, you would need to escape each special character with a backslash
