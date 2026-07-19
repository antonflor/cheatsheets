# sed Quick Reference

> **Applies to:** POSIX sed concepts with common GNU and BSD implementations
> **Last reviewed:** 2026-07-18

`sed` is a stream editor for selecting, replacing, deleting, inserting, and transforming text in files or pipelines.

## Basic model

```text
sed [options] '<command>' <file>
```

By default, sed prints each processed line after applying the script. Use `-n` when output should occur only through explicit `p` commands.

## Common operations

Print selected lines:

```bash
sed -n '1,10p' <file>
sed -n '/<pattern>/p' <file>
```

Replace the first match on each line:

```bash
sed 's/<old>/<new>/' <file>
```

Replace every match on each line:

```bash
sed 's/<old>/<new>/g' <file>
```

Delete lines:

```bash
sed '<line-number>d' <file>
sed '/<pattern>/d' <file>
```

Insert or append text:

```bash
sed '<line-number>i\
<text>' <file>

sed '<line-number>a\
<text>' <file>
```

Use another delimiter when paths contain many slashes:

```bash
sed 's|/old/path|/new/path|g' <file>
```

## Multiple commands and script files

```bash
sed -e '<command-1>' -e '<command-2>' <file>
sed -f <script.sed> <file>
```

## In-place editing

> [!DANGER]
> In-place syntax differs between GNU and BSD/macOS sed, and a bad expression can damage every matching file. Preview output first and create a backup.

GNU sed example with a backup suffix:

```bash
sed -i.bak 's/<old>/<new>/g' <file>
```

BSD/macOS sed example with a backup suffix:

```bash
sed -i '.bak' 's/<old>/<new>/g' <file>
```

Verify the result before removing backup files.

## Useful addressing

```bash
sed -n '<start>,<end>p' <file>
sed '/<start-pattern>/,/<end-pattern>/p' <file>
sed '<first>~<step>p' <file>  # GNU extension
```

## Troubleshooting and portability

- Quote sed programs to prevent shell expansion.
- Confirm whether GNU or BSD sed is installed before using `-i`.
- Check line endings when patterns appear correct but do not match.
- Use a different delimiter for paths and URLs.
- Test with `printf` or a copy of the file before modifying production configuration.
- Prefer a dedicated parser when editing structured formats such as JSON, YAML, XML, or TOML.
