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

<h1><ins>Sub-domain Fuzzing</ins></h1>
<h2>Task: Try running a sub-domain fuzzing test on 'inlanefreight.com' to find a customer sub-domain portal. What is the full domain of it?</h2>
After running FFuf using <code>ffuf -w /opt/useful/seclists/Discovery/DNS/subdomains-top1million-5000.txt:FUZZ -u https://FUZZ.inlanefreight.com/</code> which finds us sub-domain named customer making fulldomain name customer.inlanefreight.com

![Screenshot 2024-12-04 201647](https://github.com/user-attachments/assets/bfba5a1f-b5cc-48ad-820c-4d13724e61a7)

<h1><ins>Filtering Results</ins></h1>
<h2>Task: Try running a VHost fuzzing scan on 'academy.htb', and see what other VHosts you get. What other VHosts did you get?</h2>
For this task first we need to add ip provided in htb as academy.htb to <code>/etc/hosts</code> then after fuzzing using <code>ffuf -w /opt/useful/seclists/Discovery/DNS/subdomains-top1million-5000.txt:FUZZ -u http://academy.htb:PORT/ -H 'Host: FUZZ.academy.htb' -fs 900</code> last part -fs 900 bit size might be different so you need to adjust it, we find to vHosts.

![Screenshot 2024-12-04 204437](https://github.com/user-attachments/assets/8a440c2b-21ff-4b48-8995-58dd310d8e55)

<h1><ins>Parameter Fuzzing - GET</ins></h1>
<h2>Task: Using what you learned in this section, run a parameter fuzzing scan on this page. what is the parameter accepted by this webpage?</h2>
Before we start to host list we have to add subdomains in order to proceed

![Screenshot 2024-12-04 212355](https://github.com/user-attachments/assets/d905f161-6444-45c6-875d-f22bf7d718fd)

Then using <code>ffuf -w Desktop/DirectoryTraversalLists/burp-parameter-names.txt:FUZZ -u http://admin.academy.htb:42829/admin/admin.php?FUZZ=key -fs 798</code> we find the parameters used.

![Screenshot 2024-12-04 212602](https://github.com/user-attachments/assets/5d102376-5d0d-419f-958b-98f468259c2a)

<h1><ins>Value Fuzzing</ins></h1>
<h2>Task: Try to create the 'ids.txt' wordlist, identify the accepted value with a fuzzing scan, and then use it in a 'POST' request with 'curl' to collect the flag. What is the content of the flag?</h2>
Using id scan <code>ffuf -w ids.txt:FUZZ -u http://admin.academy.htb:PORT/admin/admin.php -X POST -d 'id=FUZZ' -H 'Content-Type: application/x-www-form-urlencoded'</code> we find our id 

![Screenshot 2024-12-04 213421](https://github.com/user-attachments/assets/b526f7b5-23f8-4971-88fc-c2ee3dcfc879)

Using <code>curl http://admin.academy.htb:PORT/admin/admin.php -X POST -d 'id=key' -H 'Content-Type: application/x-www-form-urlencoded'</code> we change our id number and we find flag.

![Screenshot 2024-12-04 214040](https://github.com/user-attachments/assets/089e982d-a731-418a-b3cd-f22f9bf691b1)

<h1><ins>Skills Assessment - Web Fuzzing</ins></h1>
<h2>Task: Run a sub-domain/vhost fuzzing scan on '*.academy.htb' for the IP shown above. What are all the sub-domains you can identify? (Only write the sub-domain name)</h2>
