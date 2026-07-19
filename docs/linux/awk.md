# awk Quick Reference

> **Applies to:** POSIX awk concepts with common modern awk implementations
> **Last reviewed:** 2026-07-18

`awk` processes text as records and fields. It is especially useful for filtering rows, selecting columns, aggregating values, and transforming command output.

## Basic model

```text
awk 'pattern { action }' <file>
```

Useful built-in variables:

| Variable | Meaning |
|---|---|
| `$0` | Entire current record |
| `$1`, `$2`, ... | Individual fields |
| `NF` | Number of fields in the current record |
| `NR` | Current record number across all input |
| `FNR` | Current record number within the current file |
| `FS` | Input field separator |
| `OFS` | Output field separator |

## Common examples

```bash
awk '{ print }' <file>
awk '{ print $1, $3 }' <file>
awk 'NF > 3' <file>
awk '/<pattern>/ { print NR, $0 }' <file>
awk '{ total += $1 } END { print total }' <file>
awk -F: '{ print $1 }' /etc/passwd
awk -v value="$SHELL_VALUE" '{ print value, $0 }' <file>
```

Group and aggregate by a field:

```bash
awk '{ totals[$1] += $2 } END { for (key in totals) print key, totals[key] }' <file>
```

Set input and output separators:

```bash
awk 'BEGIN { FS=","; OFS="\t" } { print $1, $2, $3 }' <file.csv>
```

## Conditions and loops

```bash
awk '$1 >= 5 && $1 <= 10 { print }' <file>
awk '{ for (i = 1; i <= NF; i++) print i, $i }' <file>
awk '{ if ($1 > 10) print $1; else print $0 }' <file>
```

## Substitution and splitting

```bash
awk '{ gsub(/<pattern>/, "<replacement>"); print }' <file>
awk '{ count = split($0, parts, ":"); print parts[1], count }' <file>
```

## Safety and portability

- Quote awk programs with single quotes in the shell.
- Use `-v` to pass shell values rather than constructing executable awk source.
- Test against representative input before overwriting files.
- POSIX awk, `gawk`, `mawk`, and BSD awk have implementation-specific extensions.
- For complex logic, place the program in a file and run `awk -f <script.awk> <input>`.

## Troubleshooting

```bash
awk --version 2>/dev/null || awk -W version
printf '%s\n' '<sample line>' | awk '{ print NF, $0 }'
```

Check the actual delimiter, quoting, line endings, locale, and whether empty fields are significant when output differs from expectations.
