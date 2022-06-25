# grep

`grep` is a linux command line program that searches FILES for substrings that match a PATTERN|PATTERNS (capitalization from `man grep`).

## General Command Forms

This supports basic pattern matching (ie without "extended" regex)
```bash
user@host:~/.../working_directory$ grep [OPTION(S)] PATTERNS [FILE(S)]
```

If you need to use an "extended" regex pattern (ie where characters ?, +, {, }, |, (, and ) are treated as special metacharacters), use the `-e` flag before your pattern(s)
```bash
user@host:~/.../working_directory\$ grep [OPTION(S)] -e PATTERNS [FILE(S)]
```

```bash
user@host:~/.../working_directory\$ grep [–bcEFilnqsvx] [–e PATTERNS | –f PATTERNFILE | PATTERNS] [FILE ...]
```

## Cookbook

### Search all subdirectories of a directory for files containing a substring

Use the `-r` flag to **recursively** search subdirectories
```bash
user@host:~/.../working_directory\$ grep -r -e "string to search for" *
```

### Recursively search subdirectories for files with a specific extension containing a substring

Use the `--include` flag to specify the file extensions to search
```bash
user@host:~/.../working_directory\$ grep -r -e "string to search for" --include=*.ipynb *
```

If you want to include multiple file extensions in the search, put them in brackets (comma-separated and without spaces)
```bash
user@host:~/.../working_directory\$ grep -r -e "string to search for" --include=*.{ipynb,py} *
```

### Recursively search files with specific extensions while excluding directories matching a pattern

```bash
user@host:~/.../working_directory\$ grep -r -e "string to search for" --include=*.{ipynb,py} --exclude-dir=*.ipynb_checkpoints *
```

## Troubleshooting

Are you trying to match a substring that contains any of these characters?
* ^ $ . \[ \] \ \* \{ \} \( ? | + \\

These are special characters and have to be **escaped** (meaning: put a backslash in front of the special char, eg \$)