#### Translate(tr)

The tr (translate) command in Unix/Linux is used for translating or deleting characters from the standard input.

#### tr command 
```
tr [OPTIONS] SET1 [SET2]

SET1: Specifies the characters to be replaced or removed.
SET2: Specifies the characters to replace SET1. If omitted, characters from SET1 are removed or squeezed.

```
#### tr command common options
```
-d: Delete characters in SET1 from the input.
-s: Squeeze (replace consecutive repeated characters with a single occurrence).
-c: Complement the set of characters in SET1.
-t: Truncate SET1 to the length of SET2.
```

#### tr command basic example

1) Translating Characters
Example: Change lowercase letters to uppercase.
```
echo "hello world" | tr 'a-z' 'A-Z'
```

2) Deleting Characters
Example: Remove digits from a string.

```
echo "My phone number is 123-456-7890" | tr -d '0-9'
```

3) Squeezing Repeated Characters
Example: Replace multiple spaces with a single space.
```
echo "This    is  a   sentence." | tr -s ' '
```

#### Example - Replace character r to R
```
nixmin@DESKTOP-:~$ cat /etc/passwd | tr 'r' 'R'
Root:x:0:0:Root:/Root:/bin/bash
daemon:x:1:1:daemon:/usR/sbin:/usR/sbin/nologin
```

#### Example - Replace spaces to underscore
```
nixmin@DESKTOP:~$ tr ' ' '_' < /etc/passwd
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
```

#### Example - translate lowercase to uppercase
```
nixmin@DESKTOP-GINCH92:~/linux-shell-scripting/5_AWK$ cat /etc/passwd | tr [:lower:] [:upper:]
ROOT:X:0:0:ROOT:/ROOT:/BIN/BASH
DAEMON:X:1:1:DAEMON:/USR/SBIN:/USR/SBIN/NOLOGIN
BIN:X:2:2:BIN:/BIN:/USR/SBIN/NOLOGIN

```

#### Example - Delete 
```
nixmin@DESKTOP:~$ cat data.txt | tr -d 'Amit'
Rohit Amit
nixmin@DESKTOP:~$
Roh
```





























#### Real-World Example - Cleaning and Normalizing a CSV file

Imagine you have a CSV file, data.csv, with customer information. The file may contain messy data, such as:

Mixed case letters.
Spaces around fields.
Non-printable characters.
Commas that need to be replaced with semicolons for further processing.

The goal is to clean and normalize this file by performing the following steps:

Convert all text to lowercase.
Remove spaces around fields.
Replace commas with semicolons.
Remove any non-printable characters.

Sample CSV Data (data.csv)
```
Name, Age , Email  ,Phone Number
John Doe , 29, john.doe@Example.com, +1-800-555-1234
Jane SMITH, 34,  JANE.SMITH@example.COM  ,  +1-800-555-5678
```

Shell Script to Clean and Normalize the CSV File

```
#!/bin/bash

# Input and output files
input_file="data.csv"
output_file="cleaned_data.csv"

# Check if the input file exists
if [[ ! -f $input_file ]]; then
    echo "Input file not found!"
    exit 1
fi

# Clean and normalize the data
cat "$input_file" |
# Step 1: Convert all text to lowercase
tr 'A-Z' 'a-z' |
# Step 2: Remove spaces around fields (use awk for trimming and reformatting the CSV)
awk -F, '{
    for (i=1; i<=NF; i++) {
        gsub(/^ +| +$/, "", $i);  # Remove leading and trailing spaces
    }
    print $0
}' |
# Step 3: Replace commas with semicolons
tr ',' ';' |
# Step 4: Remove non-printable characters
tr -cd '\11\12\15\40-\176' > "$output_file"

echo "Data cleaned and saved to $output_file"
```

Explanation

1) Convert All Text to Lowercase:
```
tr 'A-Z' 'a-z'
```
Converts uppercase letters to lowercase for consistency.

2) Remove Spaces Around Fields:
```
awk -F, '{ 
    for (i=1; i<=NF; i++) { 
        gsub(/^ +| +$/, "", $i); 
    } 
    print $0 
}'
```
Uses awk to split the fields by commas, then uses gsub to remove leading and trailing spaces from each field.


3) Replace Commas with Semicolons:
```
tr ',' ';'
```

Replaces commas with semicolons to format the CSV for systems that require semicolons as delimiters.

4) Remove Non-Printable Characters:
```
tr -cd '\11\12\15\40-\176'
```
Keeps only printable ASCII characters and common whitespace characters like tabs and newlines.


