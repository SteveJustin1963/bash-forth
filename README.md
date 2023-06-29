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

Certainly! Here's an explanation of how the program works in point form:

1. The program defines a variable `file-string` to store a string.
2. The word `set-file-string` takes a string input and stores it in the `file-string` variable.
3. The word `grep-string` takes a pattern input and searches for that pattern within the `file-string` variable.
4. If a match is found, it displays "Match found: " followed by the matched portion of the string.
5. If no match is found, it displays "No match found."
6. The word `grep` is the main entry point for the program. It sets the string using `set-file-string` and performs the search using `grep-string`.
7. The `main` word provides an example usage of the program.
8. It displays a hardcoded string "Hello, World!" and a hardcoded pattern "World".
9. It then calls `grep` to search for the pattern within the stored string.
10. The program output shows whether a match is found or not.

So, when you run the program, it will display the hardcoded string and pattern, perform the search, and show the result indicating whether a match is found or not.

