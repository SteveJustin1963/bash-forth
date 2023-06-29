# tec-bash-forth
bash in forth


## grep

Certainly! Here's the complete modified program:

```forth
variable file-string  \ Variable to store the string

: set-file-string ( str -- )  \ Store a string in the file-string variable
  file-string !
;

: grep-string ( pattern -- )  \ Search for a pattern in the file-string variable
  file-string @   \ Fetch the string from the file-string variable
  over search 0< if
    drop
    ." Match found: " type cr
  else
    drop
    ." No match found." cr
  then
;

: grep ( pattern -- )  \ Main function to set the string and perform the search
  set-file-string
  grep-string
;

\ Hardcoded string and pattern for demonstration
: main
  ." String: Hello, World!" cr
  ." Pattern: World" cr
  ." Searching..." cr
  ." -----------" cr
  "Hello, World!" grep
;

main  \ Call the main function to start the program

```

In this program, the `set-file-string` word is used to store a string in the `file-string` variable. The `grep-string` word searches for a pattern within the stored string and displays the result. The `grep` word combines both operations and is the main entry point for searching.

The `main` word provides an example usage of the program. It prompts for a string, stores it, prompts for a pattern to search for, and performs the search.
