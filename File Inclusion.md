Local File Inclusion (LFI)

Task: Using the file inclusion find the name of a user on the system that starts with "b".

To exploit LFI, we use directory traversal techniques to access sensitive files. By injecting ../ into the file path, we can navigate out of the intended directory and access system files, such as /etc/passwd. The /etc/passwd file is a standard Linux file that contains information about system users. Each line in the file corresponds to a user, and their usernames are the first field on each line.

![Screenshot 2024-11-15 211516](https://github.com/user-attachments/assets/d8bed4c5-9929-4d5b-bb24-0974766b87bd)

Since Text output is to long we can view page source to see full text which gives us user on the system that starts with "b".

![Screenshot 2024-11-15 211552](https://github.com/user-attachments/assets/a3ac51a4-57a0-4a98-8266-73a97d51e4b7)



Task: Submit the contents of the flag.txt file located in the /usr/share/flags directory.

Using same method and changing directory with file name get our flag.

![Screenshot 2024-11-15 211948](https://github.com/user-attachments/assets/36fd1584-3202-41c5-99b3-fa6c83676c7d)
