Go to the Compute engine service
In the navbar you will see the metadata option
in the ssh option paste the public key that you will generate

1. Generate the ssh key
```bash 
ssh-keygen -t rsa -f ~/.ssh/<name of the ssh key> -C <username>
```

2. Copy the public key
```bash
pbcopy < ~/.ssh/<name of the ssh key>.pub
```

3. Paste this public key in the metadata section ssh key and then save it.

now to connect with your terminal write the following command

```bash
ssh -i ~/.ssh/<name of the ssh key> <username>@<external/public ip address>
```


If you had already running instance and later you added the ssh key in metadata then you might have to add the user

```bash
sudo adduser <username>
```
