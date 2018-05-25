### Introduction
**PHP** is an <a id="top">acronym</a> which stands for: *Hypertext Preprocessor*, and is 
mostly used for back-end web application related tasks such as parsing/processing data, 
saving to/reading from a cookie, a session or file, and communicating with a database. However, 
the PHP_CLI_SAPI (command-line server application interface) allows you to interact with 
the PHP shell inside your terminal, much the same as Python and Ruby's interactive interpreters. 
But you can also run a PHP Shell script as a standalone executable. This guide shows you exactly how its done.

**read_stdin.php** The following script is short and simple. It asks for your name and reads whatever 
you enter from STDIN: standard input.

    #!/usr/bin/php -q
    <?php 
	 // Always begin with a shebang! -q: quiet, no headers!
    # Define STDIN in case it's not already defined by PHP for some reason 

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

**mkdir_remote_host_sftp.php** This script uses *SSH* and *SFTP* (Secure File Transfer protocol) to 
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

**scp_2_remote_host.php** This script is cool to use after you've created a new directory on a remote machine.
It employs scp: (Secure copy protocol) to send a file to a remote host.

    #!/usr/bin/php
    <?php
    //Make an SSH connection to a remote host, use tcp port: 22 remote host

    $connection = ssh2_connect('10.10.43.107', 22);
 		
 	 // setup Auth credentials, @ to suppress error messages
    @ssh2_auth_password($connection, 'User_Name', 'password');

    /* $do_copy contains an expression which employs the ssh2_scp_send function
    | using connection handle as 1st argument and the local path/file_name
    | to copy as the 2nd argument. 3rd arg: remote path/file_name, and
    | the 4th argument sets the permissions on the newly copied file. If the
    | expression returns: True, assign 1st message to $do_copy, else 2nd msg */
    
    $do_copy = ssh2_scp_send ($connection, '/home/local/file/fatpiggy.txt',
	 		     '/home/remote/file/fatpiggy.txt', 0644) ? 
			     "Your file was Successfully Sent & Copied :-D" :
			     "FILE! Was not sent right Mr. Peter."; 
    
    # Show us the message.

    echo "\n$do_copy\n";
    ?>





















Return to <a href="#top">Top</a>.