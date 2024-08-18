# O2. Build a Credential Harvester

Now that you have targets in hand, it's time to craft your attack. You'll need some way to harvest your victims' credentials, and sometimes the most straightforward approach is the best one - how about a credential harvester?
You're more likely to trick The Lucky Lion's employees if your harvester looks realistic - best to start with finding their real employee sign-in page and see if you can mimic it. Maybe start by looking at the Lucky Lion home page you found while looking for phishing targets?

## Objective
	• Clone the sign-up page & host on different domain to build a credential harvester. Once you do, submit the credentials "admin" and "password" to receive the flag.
### Tools Required
	• Web Browser
	• Any Website Cloning Tool like HTTrack or goclone
	• Static Server Utility like serve or nginx
### Additional Resources
	• https://www.crowdstrike.com/cybersecurity-101/cyberattacks/credential-harvesting
https://target-httpd.chals.io/casino/homepage.html

From <https://target.ctfd.io/challenges#O2.%20Build%20a%20Credential%20Harvester-23> 


# Solution:

There are different ways to approach this challenge. The easiest for me was to use my Kali Linux VM. Kali has pre-installed many cybersecurity tools, including nginx and go. To confirm that you have these tools, use the find command to locate the directory where they are found.

In order to clone the sign-up page, I navigated through the Lucky Lion webpage and clicked the login. This took me to the login page. I copied this URL and used the goclone command to make a clone of this webpage.

```
sudo ./goclone https://target-httpd.chals.io/casino/employee-login.html
```
![1](https://github.com/user-attachments/assets/94710c5b-d2f5-489a-bb77-761cfe123d12)

To make it easier, I moved the cloned sign-up page into the ` /var/www/html/ directory. ` The static server I used was nginx, because it was already available on my Kali machine and was easy to set up. I followed two different tutorials to complete this challenge. The following steps were done using the  [first tutorial](https://ubuntu.com/tutorials/install-and-configure-nginx#2-installing-nginx).


![2](https://github.com/user-attachments/assets/dfa7cd17-50c5-42bf-a18a-ac65126554c1)


I created a configuration file ` (notaclone.com) ` in the ` /etc/nginx/sites-available directory. ` To write and save the file, you will need sudo privileges.

![3](https://github.com/user-attachments/assets/f4bc7305-c0ca-4472-a66d-487986af257f)

In the configuration file, I copied the one listed in the tutorial. I made changes so that the port was set to 8080. I changed the directory after root to point to the file of the cloned webpage. I kept the server name the same as the configuration file: notaclone.com.

![4](https://github.com/user-attachments/assets/2dc72cd3-8abc-453c-808b-f9201383d540)

After creating the configuration file, I switched to the [tutorial](https://medium.com/@deltarfd/how-to-set-up-nginx-on-ubuntu-server-fc392c88fb59) by Delta Rahmat Fajar Delviansyah. I linked my notaclone.com site to the ` /etc/nginx/sites-enabled ` directory.  After linking it, I checked that the configuration of nginx was correct before restarting it. My static server was not working, so I went through the tutorials again and realized that I had not updated my DNS records. notaclone.com was not being recognized!

![5](https://github.com/user-attachments/assets/45c0b866-5bdb-403d-954c-ac8895a552b0)

![6](https://github.com/user-attachments/assets/02ccde48-5925-42b1-a657-daeea19494fa)


After updating the ` /etc/hosts ` file, the page was able to load! I was able to obtain the flag after entering the credentials.

![7](https://github.com/user-attachments/assets/c53e5519-14f8-48b3-bad7-378402f65068)

![8](https://github.com/user-attachments/assets/a46a7de8-ae2d-42be-b945-6fd28bc132ae)


## Lessons Learned: 

Kali is an awesome VM to use! When in doubt, unlink the default configuration file and check your DNS records! 

