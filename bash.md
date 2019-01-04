<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [History](#history)
  - [Allow Multiple Terminal Sessions To Write To History](#allow-multiple-terminal-sessions-to-write-to-history)
  - [Run Previous Command With Sudo](#run-previous-command-with-sudo)
  - [Run All Previous Arguments With New Command](#run-all-previous-arguments-with-new-command)
  - [Search History](#search-history)
  - [Show Most Used Commands In History](#show-most-used-commands-in-history)
- [Navigation](#navigation)
  - [Return To Previous Directory](#return-to-previous-directory)
  - [Quick Navigation With $CDPATH](#quick-navigation-with-cdpath)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## History
### Allow Multiple Terminal Sessions To Write To History
~~~
shopt -s histappend
~~~
### Run Previous Command With Sudo
~~~
sudo !!
~~~
### Run All Previous Arguments With New Command
~~~
touch file1 file2 file3 file4
chmod 777 !*
chmod 777 file1 file2 file3 file4
~~~
### Search History
~~~
history | grep <some command>
~~~
or
~~~
ctrl + r
~~~
### Show Most Used Commands In History
~~~
history | awk 'BEGIN {FS="[ \t]+|\\|"} {print $3}' | sort | uniq -c | sort -nr | head
~~~
## Navigation
### Return To Previous Directory
~~~
cd -
~~~
### Quick Navigation With $CDPATH
Any top level directory in $CDPATH can be jumped to using just the directory
name.  It works the same way as $PATH, but specifically for 'cd'
~~~
export CDPATH='~:/var/log:/etc'
~~~

