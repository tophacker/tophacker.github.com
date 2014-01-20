---
layout: post
title: 2013-12-05 Shell scripts 
category : linux
tagline: "Supporting tagline"
tags : [shell]
---
{% include JB/setup %}

The following scripts are all of bash scripts, and test passed on ubuntu12.04.

## detect net:
	#!/bin/sh
	# author: lst
	# date : 2013-12-05	
	num=0

	until [ "$num" -gt "1" ]
	do
        	num=`ifconfig | grep -i 'inet addr' | wc -l`
		echo 'net detecting...'
		sleep 2
	done

	sleep 2
	`stardict&`
	`thunderbird&`
	`chromium-broswer&`
	echo '\n\nnet connected!'

## whileread
	#!/bin/sh
	# whileread
	while read LINE
	do
		echo $LINE
	done < $1

## menu
	#!/bin/sh
	# menu
	# set the date, user and hostname up
	MYDATE=`date +%d/%m/%Y`
	THIS_HOST=`hostname -s`
	USER=`whoami`
	# loop forever !
	while :
	do
		# clear the screen
		tput clear
		# here documents starts here
		cat <<MAYDAY
		----------------------------------------------------------------------------------------------------------------
		User: $USER                      Host: $THIS_HOST                             Date: $MYDATE          
		----------------------------------------------------------------------------------------------------------------
		                         1: List files in current directory
				         2: Use the vi editor
				         3: See who is on the system
				         H: Help screen
				         Q: Exit Menu	 
		----------------------------------------------------------------------------------------------------------------
	MAYDAY
	
		# here document finished
		echo -e -n "\tYour Choice [1,2,3,H,Q] >"
		read CHOICE
		case $CHOICE in
			1) ls
				;;
			2) vi
				;;
			3) who
				;;
			H|h)
				#use a here document for the help screen
				cat <<MAYDAY
				This is the help screen, nothing here yet to help you!
	MAYDAY
				;;
			Q|q) exit 0
				;;
			*) echo -e "\t\007unknown user response"
				;;
		esac
		echo -e -n "\tHit the return key to continue"
		read DUMMY
	done

## heredoc
	#!/bin/sh
	cat <<MAY
	111
	222
	aaa
	bbb
	ccc
	MAY

## tr case2
	#!/bin/sh
	#tr_case2
	# convert case, using getopts
	EXT=""
	TRCASE=""
	FLAG=""
	OPT="no"
	VERBOSE="off"
	
	while getopts :luv OPTION
	do
		case $OPTION in
			l) TRCASE="lower"
				EXT=".LC"
				OPT=yes
				;;
			u) TRCASE="upper"
				EXT=".UC"
				OPT="yes"
				;;
			v) VERBOSE=on
				;;
			\?) echo "Usage: `basename $0`: -[l|u] -v file[s]"
				exit 1
				;;
		esac
	done
	
	# next argument down only please
	shift `expr $OPTIND - 1`
	# are there any arguments passed ???
	if [ "$#" = "0" ] || [ "$OPT" = "NO" ]
	then
		echo "Usage: `basename $0`: -[l|u] -v file[s]" >&2
		exit 1
	fi
	
	for LOOP in "$@"
	do
		if [ ! -f $LOOP ]
		then
			echo "`basename $0`: Error cannot find file $LOOP" >&2
			exit 1
		fi
		echo $TRCASE $LOOP
	
		case $TRCASE in
			lower) if [ "$VERBOSE" = "on" ]; then
				echo "doing.. lower on $LOOP .. newfile called $LOOP$EXT"
			fi
			cat $LOOP | tr "[A-Z]" "[a-z]" >$LOOP$EXT
			;;
		upper) if [ "$VERBOSE" = "on" ]; then
			echo "doing.. upper on $LOOP .. newfile called $LOOP$EXT"
		fi
		cat $LOOP | tr "[a-z]" "[A-Z]" >$LOOP$EXT
		;;
		esac
	done

## lockit
	#!/bin/sh
	# lockit
	# trap signals 2 3 and 15
	trap "nice_try" 2 3 15 20
	
	# get the device we are running on
	TTY=`tty`
	nice_try()
	{
		# nice_try
		echo "Nice try, the terminal stays locked"
	}
	
	# save stty settings hide characters typed in for the password
	SAVEDSTTY=`stty -g`
	stty -echo
	echo -n "Enter your password to lock $TTY :"
	read PASSWORD
	clear
	
	while :
	do
		# read from tty only !!,
		read RESPONSE < $TTY
		if [ "$RESPONSE" = "$PASSWORD" ]; then
			# password matches...unlocking
			echo "unlocking..."
			break
		fi
		# show this if the user inputs a  wrong password or hits return
		echo "wrong password and terminal is locked..."
	done
	
	# restore stty settings
	stty $SAVEDSTTY
	
## pingall
	#!/bin/sh
	# pingall
	
	# grab /etc/hosts and ping each address
	cat /etc/hosts| grep -v '^#' | while read LINE
	do
		ADDR=`awk '{print $1}'`
		for MACHINE in $ADDR
		do 
			ping -c3 $MACHINE
		done
	done
	
## del lines
	#!/bin/sh
	
	# del lines
	# script takes filename(s) and deletes all blank lines
	
	TEMP_F=/tmp/del.lines.$$
	
	usage()
	{
		# usage
		echo "Usage : `basename $0` file [file...]"
		echo "try `basename $0` -help for more info"
		exit 1
	
	}
	
	if [ $# -eq 0 ];then
		usage
	fi
	
	while [ $# -gt 0 ]
	do
		case $1 in
			-help) cat << MAYDAY
				Use the script to delete all blank lines from a text file(s)
	MAYDAY
	                        exit 0
	                        ;;
	
			*) FILE_NAME=$1
				if [ -f $1 ]; then
					sed '/^$/d' $FILE_NAME >$TEMP_F
					mv $TEMP_F $FILE_NAME
					echo "processing $1 done" 
				else
					echo "`basename $0` cannot find this file : $1"
				fi
				shift
				;;
		esac
	done

## colour scr
	#!/bin/sh
	# colour_scr
	tput init
	MYDATE=`date +%D`
	
	colour()
	{
		# format is background; foregroundm
		case $1 in
			black_green)
				echo '[40;32m' 
				;;
			black_yellow)
				echo '[40;33m'
				;;
			black_white)
				echo '[40;37m'
				;;
			black_cyan)
				echo '[40;36m'
				;;
			black_red)
				echo '[40;31m'
				;;
			red_yellow)
				echo '[41;33m'
				;;
		esac
	}
	
	xy()
	# xy
	# to call: xy row, column, "text"
	# goto xy screen co-ordinates
	{
		# _R=row,_C=column
		_R=$1
		_C=$2
		_TEXT=$3
		tput cup $_R $_C
		echo -n $_TEXT
	
	}
	
	center()
	{
		# center
		# centers a string of text across screen
		# to call: center "string" row_number
		_STR=$1
		_ROW=$2
		# crude way of getting length of string
		LEN=`echo $_STR | wc -c`
	
		COLS=`tput cols`
		HOLD_COL=`expr $COLS - $LEN`
		NEW_COL=`expr $HOLD_COL / 2`
		tput cup $_ROW $NEW_COL
		echo -n $_STR
	}
	
	tput clear
	colour red_yellow
	xy 2 3 "USER: $LOGNAME"
	colour black_cyan
	center "ADD A NEW WARP DRIVE TO A STAR SHIP" 3
	echo -e "\f\f"
	center "-----------------------------------" 4
	
	colour black_yellow
	xy 5 1 "---------------------------------------------------------------------------------"
	xy 7 1 "---------------------------------------------------------------------------------"
	xy 21 1 "---------------------------------------------------------------------------------"
	center "Star Date $MYDATE " 22
	xy 23 1 "---------------------------------------------------------------------------------"
	
	colour black_green
	xy 6 6 "Initials: "
	read INIT
	xy 8 14
	echo -n "Security Code No:         :"
	read CODE
	xy 10 14
	echo -n "Ship's Serial NO:         :"
	read SERIAL
	xy 12 14
	echo -n "Is it on the Port Side    :"
	read PORT
	
	colour red_yellow
	center "Save This Record [Y..N] : " 18
	read ans
	
	# reset to normal
	colour black_white
	
## menu2
	#!/bin/sh
	# menu2
	# Main menu script
	# ignore ctrl-c and quit interrupts
	
	trap "" 2 3 15 20
	MYDATE=`date +%d/%m/%Y`
	THIS_HOST=`hostname -s`
	
	USER=`whoami`
	
	# user level file
	USER_LEVELS=0011_menu2_priv.user.txt
	
	# hold file
	HOLD1=hold1.$$
	
	# colour function
	colour()
	{
		# format is background; foregroundm
		case $1 in
			black_green)
				echo '[40;32m' 
				;;
			black_yellow)
				echo '[40;33m'
				;;
			black_white)
				echo '[40;37m'
				;;
			black_cyan)
				echo '[40;36m'
				;;
			black_red)
				echo '[40;31m'
				;;
			red_yellow)
				echo '[41;33m'
				;;
		esac
	}
	
	# just read a single key please
	get_char()
	{
		# get_char
		# save current stty settings
		SAVEDSTTY=`stty -g`
		stty cbreak
		dd if=/dev/tty bs=1 count=1 2> /dev/null
		stty -cbreak
	
		# restore stty
		stty $SAVEDSTTY
	}
	
	# turn the cursor on or off
	cursor()
	{
		# cursor
		# turn cursor on/off
		_OPT=$1
		case $OPT in
			on) echo '[?25h'
				;;
			off) echo '[?25l'
				;;
			*) return 1
				;;
		esac
	
	}
	
	# check what privilege level the user has
	restrict()
	{
		colour red_yellow
		echo -e -n "\n\n\007Sorry you are not authorised to use this function"
		colour black_green
	}
	
	user_level()
	{
		# user level
		# read in the priv.user file
		while read LINE
		do
			case $LINE in
				# ignore comments
				\#*);;
			*) echo $LINE >>$HOLD1
				;;
		esac
	done < $USER_LEVELS
	
	FOUND=false
	while read MENU_USER PRIV
	do
		if [ "$MENU_USER" = "$USER" ];
		then
			FOUND=true
	
			case $PRIV in
				yes|YES)
					return 0
					;;
				no|NO)
					return 1
					;;
			esac
		else
			# no match found read next record
			continue
		fi
	done < $HOLD1
	
	if [ "$FOUND" = "false" ]; then
		echo "Sorry $USER you have not been authorised to use this menu"
		exit 1
	fi
	
	}
	
	# called when user selects quit
	my_exit()
	{
		# my_exit
		# called when user selects quit!
		# colour black_white
		cursor on
		rm *.$$
		tput reset
	
		exit 0
	
	}
	
	tput init
	# display their user levels on the screen
	if user_level; then
		ACCESS="Access Mode is High"
	else
		ACCESS="Access Mode is Normal"
	fi
	
	tput init
	while :
	do
		tput clear
		colour black_green
		cat <<MAYDAY
		$ACCESS
		-------------------------------------------------------------------------------------------------------
		User: $USER                     Host: $THIS_HOST                           Date: $MYDATE
		-------------------------------------------------------------------------------------------------------
		                                1 : ADD A RECORD
						2 : VIEW A RECORD
						3 : PAGE ALL RECORDS
						4 : CHANGE A RECORD
						5 : DELETE A RECORD
						P : PRINT ALL RECORDS
						H : Help ScREEN
						Q : Exit Menu
		-------------------------------------------------------------------------------------------------------
	MAYDAY
		colour black_cyan
		echo -e -n "\t Your Choice [1,2,3,4,5,P,H,Q] >"
		#read CHOICE
		CHOICE=`get_char`
		case $CHOICE in
			1) ls
				;;
			2) vi
				;;
			3) who
				;;
			4) if user_level; then
				ls -l | wc
			else
				restrict
			fi
			;;
			5) if user_level; then
				pwd
			else
				restrict
			fi
			;;
			P|p) echo -e "\n\nPrinting records......"
			;;
			H|h) 
				tput clear
				cat <<MAYDAY
				This is the help screen, nothing here yet to help you!
	MAYDAY
			;;
			Q|q) my_exit
	
			;;
			*) echo -e "\t\007Unknown user response"
				;;
		esac
		echo -e -n "\tHit the return key to continue..."
		read DUMMY
	done
	
## find string from files content, typical usage : `find path -name "*.txt" -print | xargs this_script.sh -s string`.
	#!/bin/sh
	# find string from files' content
	# script takes string and filename(s) , if found the string inside some file, then output the line number, string and file name.
	# author: lst
	# date  : 2013-11-30
	
	usage()
	{
		# usage
		echo "Usage : `basename $0` -[h] -[s string] file(s)"
		exit 1
	
	}
	
	while getopts :hs: OPTION 
	do
		case $OPTION in
			h) cat << MAYDAY
				Use the script to find those file(s) which content contains the string
				`basename $0` -[h] -[s string] file(s)
	
	MAYDAY
	                        exit 0
	                        ;;
	
			s) THE_STRING=$OPTARG
				shift
				shift
	
				;;
			\?) usage
				exit 1
				;;
		esac
	done
	
	while [ $# -gt 0 ]
	do
		if [ -f $1 ]; then
			if cat -n $1 | grep $THE_STRING; then
				echo "--->$1"
			fi
		else
			echo "`basename $0` cannot find this file : $1"
		fi
		shift
	done
	

	
<script>
  (function(i,s,o,g,r,a,m){i['GoogleAnalyticsObject']=r;i[r]=i[r]||function(){
  (i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),
  m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;m.parentNode.insertBefore(a,m)
  })(window,document,'script','//www.google-analytics.com/analytics.js','ga');

  ga('create', 'UA-39534509-1', 'tophacker.github.io');
  ga('send', 'pageview');

</script>





