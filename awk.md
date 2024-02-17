### AWK Cheat Sheet

#### Introduction to AWK

AWK is a powerful text processing language, ideal for manipulating data files, extracting and formatting data, and producing reports.

- **Purpose**: Text processing, data extraction, and reporting.

#### AWK Commands

**Print All Lines**

- `awk '{print}' file.txt`
- Prints all lines in `file.txt`.

**Print Specific Field**

- `awk '{print $2}' file.txt`
- Prints the second field of each line in `file.txt`.

**Sum a Column**

- `awk '{sum += $1} END {print sum}' file.txt`
- Sums up the numbers in the first column of `file.txt`.

**Print Lines Matching a Pattern**

- `awk '/pattern/ {print}' file.txt`
- Prints lines that match 'pattern'.

**Print Lines with More than N Fields**

- `awk 'NF > N' file.txt`
- Prints lines with more than N fields.

**Print Line Number with Each Line**

- `awk '{print NR, $0}' file.txt`
- Prints each line preceded by its line number.

**Print Last Field of Each Line**

- `awk '{print $NF}' file.txt`
- Prints the last field of each line.

**Replace or Substitute Text**

- `awk '{gsub(/pattern/, "replacement"); print}' file.txt`
- Replaces 'pattern' with 'replacement' in `file.txt`.

**Print Lines Where a Field Matches a Pattern**

- `awk '$N ~ /pattern/' file.txt`
- Prints lines where the Nth field matches 'pattern'.

**Split a Line into Array**

- `awk '{split($0, array, ":"); print array[1]}' file.txt`
- Splits each line by ':' and prints the first element.

#### Complex Data Processing

**Pass Shell Variable to AWK**

- `awk -v var="$shell_var" '{print var, $0}' file.txt`
- Passes a shell variable `shell_var` to AWK.

**Perform Arithmetic Operations**

- `awk '{print $1 * $2}' file.txt`
- Multiplies the first and second fields.

**Print Lines If a Field is Within a Range**

- `awk '$1 > 5 && $1 < 10' file.txt`
- Prints lines if the first field is between 5 and 10.

**Conditional Statements**

- `awk '{if ($1 > 10) print $1; else print $0}' file.txt`
- Prints the first field if it's greater than 10, otherwise prints the whole line.

**Loop Through Fields in a Line**

- `awk '{for (i=1; i<=NF; i++) print $i}' file.txt`
- Prints each field of a line on a new line.

#### Advanced Text Processing

**Print Lines Whose Field Matches a Regular Expression**

- `awk '$1 ~ /^[a-zA-Z]+$/' file.txt`
- Prints lines where the first field contains only letters.

**Aggregate Data Based on a Field**

- `awk '{arr[$1] += $2} END {for (i in arr) print i, arr[i]}' file.txt`
- Sums the second field grouped by the first field.

**Print Every Nth Line**

- `awk 'NR % N == 0' file.txt`
- Prints every Nth line.

**Print Lines Longer Than N Characters**

- `awk 'length($0) > N' file.txt`
- Prints lines longer than N characters.

**Convert CSV to Tab-Delimited**

- `awk 'BEGIN {FS=","; OFS="\t"} {print}' file.csv`
- Converts a CSV file to tab-delimited format.

#### Tips for Using AWK

- **Regular Expressions**: Mastering regular expressions enhances AWK's power.
- **Script Files**: For complex operations, write AWK commands in a script file.
- **Testing**: Test AWK commands on sample data before applying to important files.
