#!/bin/bash

version="0.1.20"
author="Robert Tulke, rt@debian.sh"

## extract basename
file=$(basename "$0" )
ext=${file###*.}

IAM=${0##*/}    # basename of this script; used in diagnostics
Usage="Usage: $IAM [-thv] [-n <sec>] \"command\""

## dispaly a date title
display_title () {
  echo -e "Every $num,0s: \033[500C\033[31D[ $(date) ]"
  echo -e ""
}

## check timer arguements are given
refresh_timer () {

  ## get $argt1 and $argt2
  num=$1
  cmd=$($2)

  ## integer check if param is a number
  if [ "$num" -eq "$num" ] 2>/dev/null;  then
    echo "is a number"
  else
    echo "Option -n <..> must be an integer" >&2
    exit 1
  fi

  ## test if -t = 1
  if [[ -n "$t" ]]; then
    while (true); do
      clear
      echo "$cmd"
      sleep $num
    done
  else ## if -t = ""
    while (true); do
      clear
      display_title
      echo "$cmd"
      sleep $num
    done
  fi
}


display_version () {
  echo "$version"
}

display_help () {
  echo "-t,     turns off showing the header"
  echo "-h,     print a summary of the options"
  echo "-n <s>, seconds to wait between updates"
  echo "-v,     print the version number"
}


# Parse options...
while getopts ":tn:hv" opt
do  sc=0    # set extra arg shift count default (0 for no option arguments or
            # one option argument, 1 for two option-arguments, 2 for three
            # option arguments, ...)
    case $opt in
    (v) # option with No option arguments
        display_version >&2
        ;;
    (h) # option with No option arguments
        display_help >&2
        ;;
    (t) # option with No option arguments
        # we set a flag
        t=1
        echo "Missing option: -n" >&2
        echo "$Usage" >&2
        ;;
    (n) # option with Two option arguments
        if [ $# -lt $OPTIND ] ; then
          echo "Option -n argument missing" >&2
          echo "$Usage" >&2
          exit 3
        fi
        argt1=$OPTARG
        eval argt2=\$$OPTIND
        refresh_timer $argt1 $argt2
        sc=1
        ;;
    (\?)echo "Unkown option: -$OPTARG" >&2
        echo "$Usage" >&2
        # I would also exit in this case...
        ;;
    (:) echo "Option -$OPTARG argument(s) missing" >&2
        echo "$Usage" >&2
        exit 1
        ;;
    esac
    if [ $OPTIND != 1 ] # This test fails only if multiple options are stacked
                        # after a single -.
    then    shift $((OPTIND - 1 + sc))
            OPTIND=1
    fi
done

# Print operands...
shift $((OPTIND - 1))   # This shift will be a no-op unless -- ended the options
while [ $# -gt 0 ]
do  echo "Unkown option: $1"
    echo "$Usage" >&2
    shift
done
