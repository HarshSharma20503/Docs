# Install jenkins and access it from the browser using GUI

## Steps

1. Create a new virtual machine in any cloud provider ( use ubuntu OS preferablly)
2. install according to official jenkins docs as this might get old.
3. Install Java first

    ```bash
    sudo apt install fontconfig openjdk-17-jre
    ```

4. Install Jenkins

    ```bash
    sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
      https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
    echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]" \
      https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
      /etc/apt/sources.list.d/jenkins.list > /dev/null
    sudo apt-get update
    sudo apt-get install jenkins
    ```

5. Run jenkins
You can enable the Jenkins service to start at boot with the command:

    ```bash
    sudo systemctl enable jenkins
    ```

You can start the Jenkins service with the command:

```bash
sudo systemctl start jenkins
```

You can check the status of the Jenkins service using the command:

```bash
sudo systemctl status jenkins
```

If everything has been set up correctly, you should see an output like this:

```bash
Loaded: loaded (/lib/systemd/system/jenkins.service; enabled; vendor preset: enabled)
Active: active (running) since Tue 2018-11-13 16:19:01 +03; 4min 57s ago
```
