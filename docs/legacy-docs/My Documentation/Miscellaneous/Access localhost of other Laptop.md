You can access localhost of other devices on same network (connected to same hotspot or wifi)

---
# Step 1: Find the IP address of the computer

You will have to find the IP address of the computer whose localhost you want to access.

### Find IP address of Windows OS

 - Press Window + R, and type ‘cmd’, Press ‘OK’. 
 - It will prompt you a command line screen
- Now Run command ipconfig | findstr /i “ipv4”

```console
IPv4 Address. . . . . . . . . . . . . . . . : 192.168.1.41
```
Note the IP address
There might be multiple IPv4 address showing some time (Don't know why) so might have to try all those (most probably the last one).

### Find IP address on Linux

- Press Ctrl + Alt + T. It will prompt you the terminal screen
- Now Run command hostname -I
- Note the IP address

```console
172.16.105.168
```

---
# Step 2: Start your localhost service

On your developer machine run localhost service, make sure to note what port number the localhost is being served on. If you are able to view application running locally on your machine via localhost.

---
# Step 3:

Now you can access the localhost of developer machine on any device’s browser navigate to 
```
http://<local IP address of developer machine>:<port number>
```
#### Example:

If I am running developer localhost on http://localhost:5173/ and my local IP Address is 192.168.1.48 than on my mobile device/secondary device browser i will navigate to
http://192.168.1.48:5173.

---
# Reference

https://www.linkedin.com/pulse/how-access-localhost-other-devices-mobile-laptop-harsh-verma/