# Regular Expressions Quick Reference

> **Applies to:** General regular-expression concepts; exact syntax varies by engine
> **Last reviewed:** 2026-07-18

Regular expressions describe text patterns for searching, validation, extraction, and replacement. Engines differ significantly, so confirm whether the tool uses POSIX basic, POSIX extended, PCRE, JavaScript, Python, .NET, RE2, or another syntax.

## Core tokens

| Pattern | Meaning |
|---|---|
| `abc` | Literal text |
| `.` | Any character except newline in many default modes |
| `^` | Start of string or line |
| `$` | End of string or line |
| `[abc]` | One character from the set |
| `[^abc]` | One character not in the set |
| `[a-z]` | Character range |
| `*` | Zero or more repetitions |
| `+` | One or more repetitions |
| `?` | Zero or one repetition |
| `{n}` | Exactly `n` repetitions |
| `{n,m}` | Between `n` and `m` repetitions |
| `(...)` | Capturing group in many engines |
| `(?:...)` | Noncapturing group where supported |
| `a|b` | Alternation |
| `\` | Escape the next metacharacter |

## Common shorthand classes

These are common in Perl-compatible engines but are not portable to every tool:

| Pattern | Typical meaning |
|---|---|
| `\d` | Digit |
| `\D` | Nondigit |
| `\w` | Word character |
| `\W` | Nonword character |
| `\s` | Whitespace |
| `\S` | Nonwhitespace |

POSIX tools may prefer classes such as `[[:digit:]]`, `[[:alnum:]]`, and `[[:space:]]`.

## Greedy and lazy matching

Quantifiers are greedy by default in many engines:

```text
.*
```

Lazy forms such as `.*?` are supported by many PCRE-style engines but not by traditional POSIX grep, sed, or awk regular expressions.

## Lookaround

Where supported:

| Pattern | Meaning |
|---|---|
| `(?=...)` | Positive lookahead |
| `(?!...)` | Negative lookahead |
| `(?<=...)` | Positive lookbehind |
| `(?<!...)` | Negative lookbehind |

RE2-based tools and some other engines intentionally do not support lookaround or backreferences.

## Practical examples

Basic IPv4-shaped text, without validating numeric ranges:

```regex
\b[0-9]{1,3}(?:\.[0-9]{1,3}){3}\b
```

Simple key-value line:

```regex
^([A-Za-z_][A-Za-z0-9_]*)=(.*)$
```

Whitespace-only line:

```regex
^[[:space:]]*$
```

## Safety and correctness

- Avoid presenting a short regex as complete email, URL, IP, or security validation unless it truly implements the required grammar.
- Anchor validation patterns with `^` and `$` or engine-specific full-match functions.
- Escape untrusted input before inserting it into a generated pattern.
- Be aware of catastrophic backtracking in engines that use backtracking evaluation.
- Prefer parsing libraries for structured formats and protocol identifiers.
- Test normal, boundary, malformed, and adversarial inputs.

## Troubleshooting

When a pattern behaves differently than expected, verify:

1. the regex engine;
2. basic versus extended mode;
3. shell quoting;
4. multiline and dot-all flags;
5. Unicode and locale behavior;
6. whether the API searches, matches from the start, or requires a full match.
