
- Installation



- ### Start Jenkins[](https://www.jenkins.io/doc/book/installing/linux/#start-jenkins)

You can enable the Jenkins service to start at boot with the command:

```
sudo systemctl enable jenkins
```

You can start the Jenkins service with the command:

```
sudo systemctl start jenkins
```

You can check the status of the Jenkins service using the command:

```
sudo systemctl status jenkins
```

If everything has been set up correctly, you should see output similar to this:

```
Loaded: loaded (/lib/systemd/system/jenkins.service; enabled; vendor preset: enabled)
Active: active (running) since Tue 2018-11-13 16:19:01 +03; 4min 57s ago
```

|     |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| --- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|     | If you have a firewall installed, you must add Jenkins as an exception. You must change `YOURPORT` in the script below to the port you want to use. Port `8080` is the most common.<br><br>```<br>YOURPORT=8080<br>PERM="--permanent"<br>SERV="$PERM --service=jenkins"<br><br>firewall-cmd $PERM --new-service=jenkins<br>firewall-cmd $SERV --set-short="Jenkins ports"<br>firewall-cmd $SERV --set-description="Jenkins port exceptions"<br>firewall-cmd $SERV --add-port=$YOURPORT/tcp<br>firewall-cmd $PERM --add-service=jenkins<br>firewall-cmd --zone=public --add-service=http --permanent<br>firewall-cmd --reload<br>``` |
|     |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
