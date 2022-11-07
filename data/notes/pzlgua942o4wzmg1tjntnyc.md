
This document collects basic syntax and best practices for writing
BASH scripts.

## Starting scripts

It is good practice to start BASH scripts with the following lines:

```bash
#!/bin/bash

set -euo pipefail
```
### Set

The command `set` allows you to change shell options. The most useful flags are:

-e
: Stop execution when an error is encountered.
You can "catch" an error to prevent the shell from exiting by appending `|| true` after a failing command.

-o pipefail
: Catch errors in pipes. Return zero exit status only if all commands in a pipeline succeed.

-u
: Error out when shell encounters undefined variables by their values.

-x
: Debug mode. Prints every line and output, substituting variables

-f
: Disable file globbing.

## A general template for BASH scripts

This template contains the most common boilerplate code for a CLI application:

```bash

```bash

#!/usr/bin/env bash

set -Eeuo pipefail

cd "$(dirname "${BASH_SOURCE[0]}")" >/dev/null 2>&1

trap cleanup SIGINT SIGTERM ERR EXIT

usage() {
  cat <<EOF
Usage: $(basename "$0") [-h] [-v] [-f] -p param_value arg1 [arg2...]
Script description here.
Available options:
-h, --help      Print this help and exit
-v, --verbose   Print script debug info
-f, --flag      Some flag description
-p, --param     Some param description
EOF
  exit
}

cleanup() {
  trap - SIGINT SIGTERM ERR EXIT
  # script cleanup here
}

setup_colors() {
  if [[ -t 2 ]] && [[ -z "${NO_COLOR-}" ]] && [[ "${TERM-}" != "dumb" ]]; then
    NOCOLOR='\033[0m' RED='\033[0;31m' GREEN='\033[0;32m' ORANGE='\033[0;33m' BLUE='\033[0;34m' PURPLE='\033[0;35m' CYAN='\033[0;36m' YELLOW='\033[1;33m'
  else
    NOCOLOR='' RED='' GREEN='' ORANGE='' BLUE='' PURPLE='' CYAN='' YELLOW=''
  fi
}

msg() {
  echo >&2 -e "${1-}"
}

die() {
  local msg=$1
  local code=${2-1} # default exit status 1
  msg "$msg"
  exit "$code"
}

parse_params() {
  # default values of variables set from params
  flag=0
  param=''

  while :; do
    case "${1-}" in
    -h | --help)
      usage
      ;;
    -v | --verbose)
      set -x
      ;;
    --no-color)
      NO_COLOR=1
      ;;
    -f | --flag) # example flag
      flag=1
      ;;
    -p | --param) # example named parameter
      param="${2-}"
      shift
      ;;
    -?*)
      die "Unknown option: $1"
      ;;
    *)
      break
      ;;
    esac
    shift
  done

  args=("$@")

  # check required params and arguments
  [[ -z "${param-}" ]] && die "Missing required parameter: param"
  [[ ${#args[@]} -eq 0 ]] && die "Missing script arguments"

  return 0
}

parse_params "$@"
setup_colors

# script logic here

msg "${RED}Read parameters:${NOCOLOR}"
msg "- flag: ${flag}"
msg "- param: ${param}"
msg "- arguments: ${args[*]-}"

```



