
## grep

### Line context

-A NUM
: Print n lines of trailing context after each match.

-B NUM
: Print n lines of leading context before each match.

### Inline context
Limit the context to N characters before and after the match:

```bash
grep -o -E ".{0,10}wantedText.{0,10}" input_file
```


## curl

Pronounced "See URL". Curl is useful for sending HTTP/HTTPS requests.

Common flags:

-X
: method. e.g. GET, POST, PUT, DELETE, etc.

-H:
headers

-d
document to send in request body


## Piping utils

### Tee

Named after a plumbing "T" junction, tee allows us to pipe the output of a command to be redirected to two different commands, or to an output file and the screen.

Examples:

```bash
# 1. redirect output to screen (stdout) and a file
ls -la | tee output.txt

# 2. redirect output to a file and another command
ls -la |  tee output1.txt | head 

# 3. redirect output to two different files
ls -la |  tee output1.txt | awk '{print $1}' > output2.txt

# It can be useful for creating backup files
cat myfile.txt | tee myfile.bak | sed...
```


### Sponge

Part of the `moreutils` package.

Sponge delays a file's modification, by creating a temporary buffer that allows using the same file as both input and output of a pipe.

For example:

```bash
cat myfile.txt | awk '{print $1}'> myfile.txt
```

does not work, because "myfile.txt" gets overwritten before awk is finished processing it. The normal way around it is to write the output to a temporary file, then replace the original file with the temporary.

On the other hand, we could use sponge:

```bash
cat myfile.txt | awk `{print $1}` | sponge > myfile.txt
```