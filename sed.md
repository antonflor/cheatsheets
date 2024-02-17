### SED (Stream Editor) Cheat Sheet

#### Introduction to SED

`sed` is a stream editor used to perform basic text transformations on an input stream (a file or input from a pipeline). It's non-interactive, meaning it processes text without user interaction.

- **Function**: Text manipulation and transformation in scripts and command line.
- **Use Cases**: Search, find and replace, insertion, deletion, and more complex text processing.

#### Basic SED Operations

- **Displaying Text**

```
sed 'p' filename  # Prints all lines from the file
```

- **Find and Replace**

```
sed 's/original/replacement/' filename  # Replaces first occurrence of 'original' with 'replacement' in each line
```

```
sed 's/original/replacement/g' filename  # Replaces all occurrences in each line
```

- **Deleting Lines**

```
sed 'nd' filename  # Deletes line number n
```

```
sed '/pattern/d' filename  # Deletes lines matching the pattern
```

- **Inserting and Appending Text**

```
sed 'n i text' filename  # Inserts 'text' before line n
```
```
sed 'n a text' filename  # Appends 'text' after line n
```


#### Advanced SED Featuresmarkdown
- **In-Place Editing**

```
sed -i 's/original/replacement/' filename  # Edits file in place
```

- **Regular Expressions**

  - `sed` supports regular expressions, providing powerful pattern matching capabilities.

- **Multiline Transformations**

  - `sed` can perform operations over multiple lines.

- **Script Files**

  - Complex `sed` commands can be written in a script file and executed.

```
sed -f script.sed filename
```

#### Special Characters in SED

- **& (Ampersand)**: Represents the matched string.

```
  sed 's/pattern/& replacement/' filename  # Appends 'replacement' to the matched 'pattern'
```

- Escaping Characters

  : Use a backslash to escape special characters.


```
  sed 's/\/path\/to\/file/\/new\/path/' filename  # Escaping slashes
```

#### SED Command Line Options

- **-e**: Allows multiple commands to be specified.

```
  sed -e 'command1' -e 'command2' filename
```

- **-n**: Suppresses automatic printing; used with 'p' to print specific lines.


```
  sed -n 'p' filename
```

- **-i**: Edits files in place (use with caution).

#### Common Use Casesmarkdown
- **File Editing**: Automate editing of configuration files or scripts.
- **Data Processing**: Process and transform data files or command outputs.
- **Text Extraction**: Extract specific lines or patterns from text.

#### Tips and Best Practices

- **Backup Files**: When using `-i`, create a backup to prevent data loss.
- **Testing**: Test `sed` commands on sample data before applying to important files.
- **Regular Expression Knowledge**: Understanding regular expressions enhances `sed`'s utility.
