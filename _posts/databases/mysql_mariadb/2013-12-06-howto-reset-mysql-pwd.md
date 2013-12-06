--- 
layout: post
title: How to reset mysql password 
category : database
tagline: "Supporting tagline"
tags : [mysql, mariadb]
---
{% include JB/setup %}

## How to reset mysql password
	Do you receive error messages like the following? :
	
	
	ERROR 1045: Access denied for user: 'root@localhost' (Using 
	password: NO)
	or
	
	
	ERROR 1045: Access denied for user: 'root@localhost' (Using 
	password: YES)
	To resolve this problem ,a fast and always working way is the "Password Resetting" .
	
	How can I reset my MySQL password?
	Following this procedure, you will disable access control on the MySQL server. All connexions will have a root access. It is a good thing to unplug your server from the network or at least disable remote access.
	
	To reset your mysqld password just follow these instructions :
	
	Stop the mysql demon process using this command :
	   sudo /etc/init.d/mysql stop
	Start the mysqld demon process using the --skip-grant-tables option with this command 
	   sudo /usr/sbin/mysqld --skip-grant-tables --skip-networking &
	Because you are not checking user privs at this point, it's safest to disable networking. In Dapper, /usr/bin/mysqld... did not work. However, mysqld --skip-grant-tables did.
	
	start the mysql client process using this command 
	   mysql -u root
	from the mysql prompt execute this command to be able to change any password
	   FLUSH PRIVILEGES;
	Then reset/update your password 
	   SET PASSWORD FOR root@'localhost' = PASSWORD('password');
	If you have a mysql root account that can connect from everywhere, you should also do:
	   UPDATE mysql.user SET Password=PASSWORD('newpwd') WHERE User='root';
	Alternate Method:
	   USE mysql
	   UPDATE user SET Password = PASSWORD('newpwd')
	   WHERE Host = 'localhost' AND User = 'root';
	And if you have a root account that can access from everywhere:
	   USE mysql
	   UPDATE user SET Password = PASSWORD('newpwd')
	   WHERE Host = '%' AND User = 'root';
	For either method, once have received a message indicating a successful query (one or more rows affected), flush privileges:
	
	FLUSH PRIVILEGES;
	Then stop the mysqld process and relaunch it with the classical way:
	
	sudo /etc/init.d/mysql stop
	sudo /etc/init.d/mysql start
	When you have completed all this steps ,you can easily access to your mysql server with the password you have set in the step before. 
	

<script>
  (function(i,s,o,g,r,a,m){i['GoogleAnalyticsObject']=r;i[r]=i[r]||function(){
  (i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),
  m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;m.parentNode.insertBefore(a,m)
  })(window,document,'script','//www.google-analytics.com/analytics.js','ga');

  ga('create', 'UA-39534509-1', 'tophacker.github.io');
  ga('send', 'pageview');

</script>
