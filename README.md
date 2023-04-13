Updated Fork for Seplos BMS - Exprimental, only with HAOS usage.

1. Install SSH & Web Terminal, Mosquitto Broker

- Install SSH & Web Terminal => username: root, password: => "yourstrongpasswordonlyfortemporraryusage"
- RUN SSH & Web Terminal
- RUN Mosquitto Broker (127.0.0.1 for local usage, define login / password, do not use local user / root login)
- Set-up Mosquitto Broker add-on

2. SSH:
```
$ cd /config
$ mkdir .ssh
$ ssh-keygen -t rsa -b 2048 (enter only, no names)
$ ssh-copy-id root@127.0.0.1 (use your defined temporrary password)
$ cp /root/.ssh/* .ssh/
$ ls .ssh (output shall be: authorized_keys id_rsa id_rsa.pub known_hosts)
```
3. SSH & Web Terminal Config Update

- With file editor, open config/.ssh/id_rsa.pub
- Copypaste input of id_rsa.pub to SSH Web Terminal Config:
```
authorized_keys:
  - >-
<pastehere>
```
- For password, let password: ""
- Restart
```
$ ssh root@127.0.0.1 (for testing purpose. If login ok w/o pwd, continue)
$ exit
```
4. Terminal SSH
```
$ cd /share
$ git clone https://github.com/TheHexaMaster/seplos_update.git
$ chmod 700 ./seplos_update/query_seplos_ha.sh ./seplos_update/run_bms_query_ha.sh
```
5. Edit config

- edit the script share/seplos_update/query_seplos_ha.sh and set the COM port that you use (Ex. DEV=/dev/ttyUSB0)
- edit the file config.ini share/seplos_update/config.ini and set the parameters with your MQTT server information:

6. Create a shell command in config.yaml
```
shell_command:
    seplos_query: ssh -o UserKnownHostsFile=/config/.ssh/known_hosts -o StrictHostKeyChecking=no -i /config/.ssh/id_rsa root@127.0.0.1 "cd /share/seplos_update;nohup /share/seplos_update/run_bms_query_ha.sh &"
```
7. Create Automation:
```
alias: Seplos BMS Autoupdate 10S
description: ""
trigger:
  - platform: time_pattern
    seconds: "10"
  - platform: time_pattern
    seconds: "20"
  - platform: time_pattern
    seconds: "30"
  - platform: time_pattern
    seconds: "40"
  - platform: time_pattern
    seconds: "50"
  - platform: time_pattern
    seconds: "0"
condition: []
action:
  - service: shell_command.seplos_query
    data: {}
mode: single
```
8. Use included configuration.yaml to create sensors and lovelace.yaml to create lovelace desktop
9. All credits to byte4geek - he deserves a donation for an excellent job! 
