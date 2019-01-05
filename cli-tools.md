<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [awk](#awk)
  - [Show Only Column 1 and Column 4 Of Output](#show-only-column-1-and-column-4-of-output)
- [column](#column)
  - [Sort Output Into Columns Based Upon Delimeter](#sort-output-into-columns-based-upon-delimeter)
- [mount](#mount)
  - [Better Than Linking](#better-than-linking)
- [nl](#nl)
  - [Line Numbers Skipping Empty Lines](#line-numbers-skipping-empty-lines)
- [slop](#slop)
  - [Use The Mouse To Find Coordinates Of A Region](#use-the-mouse-to-find-coordinates-of-a-region)
  - [A Short Script To Record A Region Of Your Screen](#a-short-script-to-record-a-region-of-your-screen)
- [sort](#sort)
  - [Sort Lines In A File](#sort-lines-in-a-file)
- [xdotool](#xdotool)
  - [Find Coordinates Of Mouse On Screen](#find-coordinates-of-mouse-on-screen)
- [yes](#yes)
  - [Automatically Approve Confirmations](#automatically-approve-confirmations)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## awk
### Show Only Column 1 and Column 4 Of Output
~~~
lsblk -l | awk '{print $1, $4}'
~~~
## column
### Sort Output Into Columns Based Upon Delimeter
~~~
cat help.txt
start: Start development mode.
stop: Stop development mode.
compile: Compile the binary.

column -t -s ':' help.txt
start     Start development mode.
stop      Stop development mode.
compile   Compile the binary.
~~~
## mount
### Better Than Linking
Serving a website from your home folder with conventional servers means messing
with permissions.  Try this idea from [Azer Koçulu][1]
~~~
mount --bind ~/my-website /var/www
~~~
## nl
### Line Numbers Skipping Empty Lines
`cat -n` or `less -n` will count blank lines.  Instead use
~~~
nl foobat.txt
~~~
[Azer Koçulu][1]
## slop
### Use The Mouse To Find Coordinates Of A Region
~~~
slop
~~~
### A Short Script To Record A Region Of Your Screen
Written by [Azer Koçulu][1]
~~~
#!/bin/bash
slop=$(slop -f "%x %y %w %h %g %i") || exit 1
read -r X Y W H G ID < <(echo $slop)
ffmpeg -f x11grab -s "$W"x"$H" -i :0.0+$X,$Y -f alsa -i pulse ~/screenshots/`date +%Y-%m-%d.%H:%M:%S`.mp4
~~~
## sort
### Sort Lines In A File
~~~
sort foobar.txt
~~~
## xdotool
### Find Coordinates Of Mouse On Screen
~~~
xdotool getmouselocation --shell
~~~
## yes
### Automatically Approve Confirmations
This is a good one to look out for in third party scripts for safety, `grep yes
install_script.sh`
~~~
yes | pacman -S cool-program
~~~


[1]: (https://azer.bike/journal/10-linux-commands-that-will-save-your-time/) "Azer Koçulu"
