# tec-bash-forth
bash in forth


## grep

basic version of the `grep` functionality found in Unix-based systems. The `grep` command is used to search text for lines that match a given regular expression. This program is a simplified version and does not support regular expressions, but it allows for text searching within files.

The three words defined in the program are `grep-file`, `grep-directory`, and `grep`:

1. `grep-file` takes a pattern and a filename as input. It attempts to open the given file and read it line by line, looking for the provided pattern. If the pattern is found in a line, it prints "Match found: " along with the line that contains the pattern. If the file can't be opened, it prints "Error: File not found".

2. `grep-directory` takes a pattern and a directory name as input. It reads all the filenames in the given directory and applies `grep-file` to each file. Essentially, it searches for the given pattern in all files within the specified directory.

3. `grep` takes a pattern and an optional directory name as input. If no directory name is provided, it just drops the top two stack items (which were duplicates of the pattern and the nonexistent directory name). If a directory name is provided, it calls `grep-directory` to search for the given pattern in all files within the specified directory.



```
: grep-file ( pattern filename -- )   \ A new function 'grep-file' that takes a pattern and a filename
  s" r" open-file if                 \ Opens the file with read permission; if successful
    begin
      dup file-status 0= until       \ Keep duplicating the top stack item and check file status until it's 0 (end of file)
      begin
        dup file-status while        \ While the file status is non-zero (not end of file)
        0 0
        begin
          over read-line while       \ Keep reading a line from the file while it's possible
          2dup swap search 0< if     \ Search for the pattern in the line; if found (0<)
            drop                     \ Drop the top stack item
            ." Match found: " type cr \ Print 'Match found: ' followed by the content of the line
          then
        repeat
        2drop close-file             \ Close the file and drop the two top stack items
      repeat
      drop
    else                             \ If the file couldn't be opened
      ." Error: File not found" cr   \ Print 'Error: File not found'
    then ;

: grep-directory ( pattern dirname -- ) \ A new function 'grep-directory' that takes a pattern and a directory name
  dir>files                           \ Convert directory to a sequence of filenames
  begin
    s" " swap strcat                  \ Concatenate the filename with a space
    dup file-status 0= until          \ Check file status until it's 0 (end of file)
    begin
      2dup grep-file                  \ Use 'grep-file' function on the pattern and filename
      2drop
    repeat
    drop ;

: grep ( pattern [dirname] -- )       \ A new function 'grep' that takes a pattern and an optional directory name
  2dup 2>r 2>r                        \ Duplicate the two top stack items and move them to the return stack
  case
    of 2drop 2drop                    \ If no directory name is provided, drop the two top stack items
    endof
    else grep-directory               \ If a directory name is provided, call 'grep-directory' function
  endcase
  2r> 2r> ;                           \ Move the two top items from the return stack to the data stack
```
