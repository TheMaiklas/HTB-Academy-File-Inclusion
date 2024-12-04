<h1><ins>Directory Fuzzing</ins></h1>
<h2>Task: In addition to the directory we found above, there is another directory that can be found. What is it?</h2>
After running <code>ffuf -w <SNIP> -u http://SERVER_IP:PORT/FUZZ</code> we find two direcories blog and forum.

![Screenshot 2024-12-04 175952](https://github.com/user-attachments/assets/a5f4d801-9bcd-4e82-8566-b3d3b93b764c)

<h1><ins>Page Fuzzing</ins></h1>
<h2>Task: Try to use what you learned in this section to fuzz the '/blog' directory and find all pages. One of them should contain a flag. What is the flag?</h2>
After running <code>ffuf -w /opt/useful/seclists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://SERVER_IP:PORT/blog/FUZZ.php</code> we find different pages, using home page we get flag.

![Screenshot 2024-12-04 183222](https://github.com/user-attachments/assets/785a74e3-ba99-4aa6-83df-c6beac33d193)

<h1><ins>Page Fuzzing</ins></h1>
<h2>Task: Try to use what you learned in this section to fuzz the '/blog' directory and find all pages. One of them should contain a flag. What is the flag?</h2>
Using <code>ffuf -w /opt/useful/seclists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://SERVER_IP:PORT/FUZZ -recursion -recursion-depth 1 -e .php -v</code> we find path were flag file is located.

![Screenshot 2024-12-04 184449](https://github.com/user-attachments/assets/0a6a73b0-16af-475c-8de3-870b3208d985)
