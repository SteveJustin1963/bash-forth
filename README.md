# tec-bash-forth
bash in forth


## grep
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
