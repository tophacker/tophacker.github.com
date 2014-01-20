---
layout: post
title: 2014-01-20 Shell scripts
category : linux
tagline: "Supporting tagline"
tags : [shell]
---
{% include JB/setup %}

The following scripts are all of bash scripts, and test passed on ubuntu12.04.

## to sort a file with text block format 

	#!/bin/bash
	# To sort a file content according to the first line of each text block, two text blocks listed as below:
	#bb
	#22
	#two,two
	#
	#aa
	#11
	#one,one
	#
	# Author: lst
	# Date: 2013-12-23
	echo "Input file $1 content:"
	cat $1 
	echo "Output file $2 content:"
	
	cat $1 | awk -v RS="" '{gsub("\n","@");print}'|sort|awk -v ORS="\n\n" '{gsub("@","\n");print}' |tee $2
	
## 9 multiple 9

	#!/bin/bash
	for (( i=1;i<=9;i++ ))
	do
		for (( j=1;j<=i;j++ ))
		do
			let "tmp = i * j"
			echo -n "$j*$i=$tmp "
		done
		echo ""
	done
	

## select color

	#!/bin/bash
	echo "What's your favorite color ?"
	PS3="Please input select number : "
	TMOUT=5
	select color in "Red" "Green" "Blue"
	do
		break
	done
	if [ -z "$color" ]
	then
		echo "Your selected nothing!"
	else
		echo "Your selected $color"
	fi

## run seconds

	#!/bin/bash
	echo $SECONDS
	sleep 10
	echo $SECONDS

## timed read

	#!/bin/bash
	echo "What's your name?"
	TMOUT=5
	read name
	if [ -z "$name" ]
	then
		name="(no answer)"
	fi
	
	echo "Hello, $name"

## html converter

	#!/bin/bash
	# author: lst
	# date: 2014-01-18
	cat << MAY
	<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.0 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
	<HTML>
	<HEAD>
	<meta http-equiv="content-type" content="text/html;charset=utf-8">
	<TITLE>
	Transform TEXT file to HTML file
	</TITLE>
	</HEAD>
	<BODY>
	<TABLE>
	MAY
	
	sed -e 's/:/<\/TD><TD>/g' -e 's/^/<TR><TD>/g' -e 's//<\/TD><\/TR>/g'
	
	cat << MAY
	</TABLE>
	</BODY>
	</HTML>
	MAY

## top n

	#!/bin/bash
	# author: lst
	# date: 2014-01-18
	
	n=$1
	cat $2 |
	tr -cs "[a-z][A-Z]" "[\012*]" | # one word per line
	tr A-Z a-z |                    # lowercase for each word
	sort |                          
	uniq -c |                       # count for each word
	sort -k1nr -k2 |                # first order by count, then order by alphabetic 
	head -n "$n"                    # output the first $n lines

## verification code generator

	#!/bin/bash
	# author: lst
	# date: 2014-01-18
	
	len=6
	i=1
	all=(0 1 2 3 4 5 6 7 8 9 a b c d e f g h i j k l m n o p q r s t u v w x y z A B C D E F G H I J K L M N O P Q R S T U V W X Y Z)
	n_all=${#all[@]}
	
	while [ "$i" -le "$len" ]
	do
		verifi_code[$i]=${all[$((RANDOM%n_all))]}
		let "i=i+1"
	done
	
	for j in ${verifi_code[@]}
	do
		echo -n $j
	done
	echo

## output ascii

	#!/bin/bash
	# author: lst
	# date: 2014-01-18
	echo "Please input the start of a range (decimal number):"
	read first
	
	echo "Please input the end of a range (decimal number):"
	read last
	
	echo "Decimal    Hex     Character"
	echo "-------    ----    ---------"
	
	for (( i=first; i <= last; i++ ))
	do
		echo $i | awk '{printf("%3d        %2x      %c\n", $1, $1, $1)}'
	done

## stack simulator

	#!/bin/bash
	# author: lst
	# date: 2014-01-18
	
	MAXTOP=50
	TOP=$MAXTOP
	TMP=
	
	declare -a STACK
	
	push()
	{
		if [ -z "$1" ]
		then
			return
		fi
	
		until [ $# -eq 0 ]
		do
			let TOP=TOP-1
			STACK[$TOP]=$1
			shift
		done
	
		return
	}
	
	pop()
	{
		TMP=
	
		if [ "$TOP" -eq "$MAXTOP" ]
		then
			return
		fi
	
		TMP=${STACK[$TOP]}
		unset STACK[$TOP]
		let TOP=TOP+1
	
		return
	}
	
	status()
	{
		echo "================Begin==================="
		for i in ${STACK[@]}
		do
			echo $i
		done
		echo
	
		echo "STACK pointer = $TOP"
		echo "Just popped: $TMP from the STACK"
		echo "=================End===================="
	
	}
	
	while :
	do
	
		echo "Select operation type:"
		select action in "Push" "Pop" "Exit"
		do
			break
		done
		
		case $action in
			Push)
				echo "Input stack elements:"
				read node
				push $node
				status
				;;
			Pop)
				pop
				status
				;;
			Exit)
				echo "Bye bye !"
				exit 0
				;;
			*)
				echo "Invalid operation type!"
				exit 1;;
		esac
		
	done

	
		
<script>
  (function(i,s,o,g,r,a,m){i['GoogleAnalyticsObject']=r;i[r]=i[r]||function(){
  (i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),
  m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;m.parentNode.insertBefore(a,m)
  })(window,document,'script','//www.google-analytics.com/analytics.js','ga');

  ga('create', 'UA-39534509-1', 'tophacker.github.io');
  ga('send', 'pageview');

</script>

