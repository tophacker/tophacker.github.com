---
layout: post
title: Linux 
category : linux
tagline: "Supporting tagline"
tags : [shell]
---
{% include JB/setup %}

## detect net:

#!/bin/sh

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




<script>
  (function(i,s,o,g,r,a,m){i['GoogleAnalyticsObject']=r;i[r]=i[r]||function(){
  (i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),
  m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;m.parentNode.insertBefore(a,m)
  })(window,document,'script','//www.google-analytics.com/analytics.js','ga');

  ga('create', 'UA-39534509-1', 'tophacker.github.io');
  ga('send', 'pageview');

</script>





