### Introduction
**PHP** is an <a id="top">acronym</a> which stands for: *Hypertext Preprocessor*, and is 
mostly used for back-end web application related tasks such as parsing/processing data, 
saving to/reading from a cookie, a session or file, and communicating with a database. However, 
the PHP_CLI_SAPI (command-line server application interface) allows you to interact with 
the PHP shell inside your terminal, much the same as Python and Ruby's interactive interpreters. 
But you can also run a PHP Shell script as a standalone executable. This guide shows you exactly how its done.

**read_stdin.php** The following script is pretty strait-forward. It asks for your name and reads whatever 
you enter from STDIN: standard input.

    #!/usr/bin/php -q
    <?php 
	 // Always begin with a shebang! -q: quiet, no headers!
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

**remote_mkdir_sftp.php** This script uses *SSH* and *SFTP* (Secure File Transfer protocol) to 
create a directory on a remote machine.

    #!/usr/bin/php
    <?php
    //create an SSH connection to a remote host, use tcp port: 22
    
    $connection = ssh2_connect('192.168.56.102', 22);
 		
 	 //setup Authorization credentials '@': suppress any error msgs

    @ssh2_auth_password($connection, 'User_Name', 'password');
    
    //Assign SFTP variable as handler using ssh2_sftp function
    
    $sftp = ssh2_sftp($connection);
    
    /* $x retains an expression which makes a dir using handle: $sftp, 
    | and the new dir path/name. This is all tied into a ternary 
    | condition '?': if/else. If expression is true $x will be assigned
    | the first string, else if false $x = Dir not created right */
    
    $x = ssh2_sftp_mkdir($sftp, '/home/remote/user/New_Dir') ? 
	    "Directory Created Successfully" : "Dir not created right"; 
	    
    //Now show us the contents of $x, insert a newline before and after it please: \n
    echo ("\n$x\n");
?>
























Return to <a href="#top">Top</a>.