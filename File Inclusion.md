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
Using ffuf we can find php files that web application uses in some cases we might need to have a larger directory file, but for this module small directory file is enough. <code>ffuf -w Desktop/directory-list-2.3-small.txt:FUZZ -u http://94.237.59.180:42908/FUZZ.php</code> , after running command we can see that it found two .php files index and configure.

Once we have a list of potential PHP files we want to read, we can start disclosing their sources with the base64 PHP filter. We can try to read source code of config.php using the base64 filter, by specifying convert.base64-encode for the read parameter and config for the resource parameter, using this code <code>php://filter/read=convert.base64-encode/resource=</code> our final URL looks like this: <code>http://94.237.59.180:42908/index.php?language=php://filter/read=convert.base64-encode/resource=configure</code>
To copy full encoded text we can use View Page Source

![Screenshot 2024-11-16 185901](https://github.com/user-attachments/assets/e03e9000-db00-4771-bfaa-f8f683c3e621)

Using <code> echo "encoded text" | base64 -d</code> we can decode text and find flag.

![Screenshot 2024-11-16 190353](https://github.com/user-attachments/assets/f15de0a4-40f5-49fe-9631-0d25be69e09e)

<h1><ins>PHP Wrappers</ins></h1>
<h2>Task: Try to gain RCE using one of the PHP wrappers and read the flag at /</h2>

