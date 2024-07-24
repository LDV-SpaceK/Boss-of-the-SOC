## BOSS OF THE SOC V1

### Threat Hunting with Splunk

* Lab link: https://samsclass.info/50/proj/botsv1.htm

### What You Need for this Project

* A computer with a Web browser. 

### Purpose

* To practice threat hunting, using the Boss of the SOC (BOTS) Dataset. 

### Connection

* Go here: https://splunk.samsclass.info or here: https://splunk2.samsclass.info
* Log in as student1 with a password of student1 

![ảnh](https://github.com/user-attachments/assets/32437d9b-6b02-490e-815a-66fd257f4691)

* Go to `Search & Reporting`
* Search `index="botsv1"`
* Choose `All time`

![ảnh](https://github.com/user-attachments/assets/fc5b7d53-8200-4212-a804-e6038c4d7e12)

* It's about 955K events (before 12/3/18 3:07:21.000 PM) events matched at the time I do this lab (22/07/2024)
* Let choose 1:100 sampling

### Solution

#### Q1.1

![ảnh](https://github.com/user-attachments/assets/649c7da2-2a57-4c28-9e3c-a95517e1066a)

* the question is about web vulnerability scanner, so I check the http streams

`index="botsv1" sourcetype="stream:http"`

![ảnh](https://github.com/user-attachments/assets/fb58cc87-4a3b-4411-b7f1-be901f7d5a1f)

* check source IP, the top IP is `40.80.148.42`

![ảnh](https://github.com/user-attachments/assets/73152f63-cfa5-4738-b6ea-15c66d297cd9)

* check destination IP, the top IP is `192.168.250.70`

![ảnh](https://github.com/user-attachments/assets/46cb5ce2-e153-4eb4-bd69-63d5dbf09a63)

* filter these two IP

`index="botsv1" sourcetype="stream:http" src_ip="40.80.148.42" dest_ip="192.168.250.70"`

* check the url

![ảnh](https://github.com/user-attachments/assets/97e584ec-29f7-4c28-bad1-1fd6035144d2)

* the web `imreallynotbatman.com` has many accesses, filter it

`index="botsv1" sourcetype="stream:http" src_ip="40.80.148.42" dest_ip="192.168.250.70" imreallynotbatman.com`

![ảnh](https://github.com/user-attachments/assets/498a6157-1123-48e6-96fe-0e6d2912d2a3)

* Clearly, the web vul scanner is `Acunetix`

![ảnh](https://github.com/user-attachments/assets/70313f18-5761-4455-ba67-f2d140aa56fa)

#### Q1.2

![ảnh](https://github.com/user-attachments/assets/3eef0552-934c-4e01-a09a-25d19abd3f99)

* Server IP `192.168.250.70`

* `Reconnaissance`: Source IP `40.80.148.42` sent a request to get server status

![ảnh](https://github.com/user-attachments/assets/10cfcbcf-b34b-49ef-b5d0-92ac61ffc95c)

* `Weaponization`: `40.80.148.42` sent a payload to server

![ảnh](https://github.com/user-attachments/assets/5ac66d89-6bca-4e81-8dac-8ad125daa247)

* Payload

```
php5?%2D%64+%61%6C%6C%6F%77%5F%75%72%6C%5F%69%6E%63%6C%75%64%65%3D%6F%6E+%2D%64+%73%61%66%65%5F%6D%6F%64%65%3D%6F%66%66+%2D%64+%73%75%68%6F%73%69%6E%2E%73%69%6D%75%6C%61%74%69%6F%6E%3D%6F%6E+%2D%64+%64%69%73%61%62%6C%65%5F%66%75%6E%63%74%69%6F%6E%73%3D%22%22+%2D%64+%6F%70%65%6E%5F%62%61%73%65%64%69%72%3D%6E%6F%6E%65+%2D%64+%61%75%74%6F%5F%70%72%65%70%65%6E%64%5F%66%69%6C%65%3D%70%68%70%3A%2F%2F%69%6E%70%75%74+%2D%64+%63%67%69%2E%66%6F%72%63%65%5F%72%65%64%69%72%65%63%74%3D%30+%2D%64+%63%67%69%2E%72%65%64%69%72%65%63%74%5F%73%74%61%74%75%73%5F%65%6E%76%3D%30+%2D%6E
```

* Decode

```
php5?-d allow_url_include=on -d safe_mode=off -d suhosin.simulation=on -d disable_functions="" -d open_basedir=none -d auto_prepend_file=php://input -d cgi.force_redirect=0 -d cgi.redirect_status_env=0 -n
```

* allow to use URL in function include, require
* turn off safe mode
* turn on suhosin stimulation
* no function was disable
* accept access to every file in server
* no php.ini

* Those settings aim to an attack to server, so the attacker IP is `40.80.148.42`

![ảnh](https://github.com/user-attachments/assets/13ba1936-0e14-4cda-b54e-dcece83f3414)

#### Q1.3

![ảnh](https://github.com/user-attachments/assets/ea2e7b5b-354d-4610-8b76-c97a1090b878)

* Web server IP is `192.168.250.70`

![ảnh](https://github.com/user-attachments/assets/37fa8b52-d592-46f2-8e50-ce36586dbeaa)

#### Q1.4


