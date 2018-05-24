### Introduction
**PHP** is an <a id="top">acronym</a> which stands for: *Hypertext Preprocessor*, and is 
mostly used for back-end web application related tasks such as parsing/processing data, 
saving to/reading from a cookie, a session or file, and communicating with a database. However, 
the PHP_CLI_SAPI (command-line server application interface) allows you to interact with 
the PHP shell inside your terminal, much the same as Python and Ruby's interactive interpreters. 
But you can also run a PHP Shell script as a standalone executable. This guide shows you exactly how its done.

The following script is pretty simple. It asks for your name and reads the what you enter from STDIN: standard input.


    #!/usr/bin/php -q
    <?php 
	 Always begin with a shebang! -q: quiet, no headers!
    // Define STDIN in case it's not already defined by PHP for some reason 

	 if (!defined("STDIN")) 
	   define("STDIN", fopen('php://stdin','r'));
 
	 echo "Hello! What is your name (enter below):\n";
     /* reads one line from STDIN and escapes ( \ ) any 
       funny chars (#'!$%?) for passing cmds/exploits
       also Read up to 80 characters or a newline */   
	 $strName = escapeshellcmd(trim(fread(STDIN, 80)));
	 #Send the name entered to STDOUT with echo
	 echo "Hello " , $strName , "\n";
	 ?> 

























Return to <a href="#top">Top</a>.