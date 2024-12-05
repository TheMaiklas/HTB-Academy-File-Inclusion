![image](https://github.com/user-attachments/assets/8dfa9abc-67ad-4154-bfd6-836fd20c22c5)<h1><ins>Directory Fuzzing</ins></h1>
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

<h1><ins>Skills Assessment - Web Fuzzing</ins></h1>
<h2>Task: Try to create the 'ids.txt' wordlist, identify the accepted value with a fuzzing scan, and then use it in a 'POST' request with 'curl' to collect the flag. What is the content of the flag?</h2>
![Screenshot 2024-12-04 214040](https://github.com/user-attachments/assets/089e982d-a731-418a-b3cd-f22f9bf691b1)

<h1><ins>Skills Assessment - Web Fuzzing</ins></h1>
<h2>Task: Run a sub-domain/vhost fuzzing scan on '*.academy.htb' for the IP shown above. What are all the sub-domains you can identify? (Only write the sub-domain name)</h2>
Using we host fuzzing with filtering <code>ffuf -w Desktop/DirectoryTraversalLists/subdomains-top1million-5000.txt:FUZZ -u http://academy.htb:34252/ -H 'Host: FUZZ.academy.htb' -fs 985</code> we find different sub-domains

![Screenshot 2024-12-05 172512](https://github.com/user-attachments/assets/b17d2e69-5d93-4ffe-a110-dd1ee7acc2e5)

<h2>Task: Before you run your page fuzzing scan, you should first run an extension fuzzing scan. What are the different extensions accepted by the domains?</h2>
Before we start extension fuzzing we have to add sub-domains to hosts file

![Screenshot 2024-12-05 180343](https://github.com/user-attachments/assets/ed26b4a6-3f43-4414-ad13-a6c3193fd75d)

Then after fuzzing faculty domain using <code>ffuf -w Desktop/DirectoryTraversalLists/web-extensions.txt:FUZZ -u http://faculty.academy.htb:34252/indexFUZZ</code> we see file extensions used

![Screenshot 2024-12-05 180610](https://github.com/user-attachments/assets/a0b5a764-57bd-45de-b192-69ef25f89306)

<h2>Task: One of the pages you will identify should say 'You don't have access!'. What is the full page URL?</h2>
Fist we need to Fuzz directories in faculty sub-domain

![Screenshot 2024-12-05 182812](https://github.com/user-attachments/assets/a9ab039a-a35f-429e-bd8d-1e5b0f8bb746)

Then knwoing extensions we found before we ffuz files with those extensions after checking multiple pages with different extensions we find our page.

![Screenshot 2024-12-05 185328](https://github.com/user-attachments/assets/b25a8187-4855-4fb2-bf65-e807ce7bd4ef)

<h2>Task: In the page from the previous question, you should be able to find multiple parameters that are accepted by the page. What are they?</h2>
Using <code>ffuf -w Desktop/DirectoryTraversalLists/burp-parameter-names.txt:FUZZ -u http://faculty.academy.htb:34252/courses/linux-security.php7 -X POST -d 'FUZZ=key' -H 'Content-Type: application/x-www-form-urlencoded'</code> we find 2 parameters

![Screenshot 2024-12-05 190755](https://github.com/user-attachments/assets/fb2799c0-fefa-4851-9e0f-c6a2d43da2f1)

<h2>Task: Try fuzzing the parameters you identified for working values. One of them should return a flag. What is the content of the flag?</h2>
Knowing parameter usernames we can try to find username for this we need to get most common username list then we change our command <code>ffuf -w Desktop/DirectoryTraversalLists/names.txt:FUZZ -u http://faculty.academy.htb:45544/courses/linux-security.php7 -X POST -d 'username=FUZZ' -H 'Content-Type: application/x-www-form-urlencoded' -fs 781</code> which gives us username harry then we use curl to get post results of the page using user harry <code>curl http://admin.academy.htb:PORT/admin/admin.php -X POST -d 'id=key' -H 'Content-Type: application/x-www-form-urlencoded'</code>

![Screenshot 2024-12-05 192437](https://github.com/user-attachments/assets/36f2e848-af1c-4dd1-8b09-3f59699a62a7)

