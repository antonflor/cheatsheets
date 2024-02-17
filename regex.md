### Regular Expressions (Regex) Cheat Sheet

#### Introduction to Regex


Regular expressions are sequences of characters that define a search pattern, mainly for use in pattern matching with strings.

- **Purpose**: Text search, text manipulation, input validation.
- **Common Uses**: String parsing, data validation, search and replace in text.


#### Basic Syntax


- **Literal Characters**: Match themselves (e.g., `abc` matches "abc").
- **Dot (.)**: Matches any single character except newline.
- **Anchors**:
  - `^`: Start of a string.
  - `$`: End of a string.


#### Character Classes


- **[abc]**: Matches any one of the characters a, b, or c.
- **[^abc]**: Matches any character not in the set.
- **[a-z]**: Matches any character from a to z.
- **[A-Z]**: Matches any character from A to Z.
- **[0-9]**: Matches any digit.
- **\d**: Matches any digit (equivalent to [0-9]).
- **\D**: Matches any non-digit.
- **\w**: Matches any word character (alphanumeric plus underscore).
- **\W**: Matches any non-word character.
- **\s**: Matches any whitespace character.
- **\S**: Matches any non-whitespace character.


#### Quantifiers


- **\***: Zero or more of the preceding element.
- **+**: One or more of the preceding element.
- **?**: Zero or one of the preceding element (marks it optional).
- **{n}**: Exactly n occurrences of the preceding element.
- **{n,}**: n or more occurrences of the preceding element.
- **{n,m}**: Between n and m occurrences of the preceding element.


#### Grouping and Alternation


- **(abc)**: Groups multiple tokens together and remembers the matched text.
- **a|b**: Matches either a or b.
- **(?:abc)**: Groups multiple tokens together without remembering the matched text.


#### Special Characters


- **\**: Escapes a special character.
- **\n**: Newline.
- **\t**: Tab.


#### Lookahead and Lookbehind


- **(?=...)**: Positive lookahead (succeeds if the pattern inside the lookahead can be matched next).
- **(?!...)**: Negative lookahead.
- **(?<=...)**: Positive lookbehind.
- **(?<!...)**: Negative lookbehind.


#### Common Regex Patterns


- **Email Address**: `\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z|a-z]{2,}\b`
- **URL**: `https?://(?:www\.)?[-a-zA-Z0-9@:%._\+~#=]{2,256}\.[a-z]{2,6}\b([-a-zA-Z0-9@:%_\+.~#?&//=]*)`
- **IP Address**: `\b\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}\b`


#### Tips for Using Regex


- **Testing**: Use online tools like [regex101.com](https://regex101.com) for testing and debugging regex patterns.
- **Readability**: Complex regex can be hard to read, so document or break down complex patterns.
- **Specificity**: Be as specific as possible to avoid unexpected matches.
