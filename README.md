# Broadbot - An opensource and modular SIEM.

## Welcome to Broadbot (Alpha)!

Built on the Elastic Stack (https://elastic.co/), Broadbot was designed with simplicity in mind. We want to give users insight to any malicious activity happening on their local machine and/or network, without needing the knowledge or spending the time putting everything together themselves.

Broadbot is an opensource offering of Invinsec's SIEMaaS. Please visit our website for a full product breakdown.


<img src="/images/ss1.JPG?raw=true" width="600"/> <img src="/images/ss2.JPG?raw=true" width="600"/> 




Broadbot 1.0.0 (ALPHA) ships with a module that allows you to instantly start monitoring software and hardware firewalls. All the hard work is done for you, you simply need to run our installer and you'll be good to go.

As of version 1.0.0 (ALPHA), the firewall module supports the following devices (provided they are configured to produce or send logs):
 
* Unix IPTables
* PFSense
* Windows Firewall
* Cisco ASA
* Juniper Netscreen
* Fortigate

## Installation Instructions

Please note: This installer requires access to the Internet to download packages. An offline version may be provided in beta or other official releases.

### Microsoft Windows


### Centos/Debian:


`curl --silent https://github.com/Invinsec/broadbot/releases/download/v1.0/BB-Unix.tar.gz | tar xvz && bash BB-Unix-Install.sh`


That's pretty much it! To start using the included dashboards, point your browser to http://x.x.x.x:5601 or http://hostname:5601 (as instructed at the end of the installer).

Broadbot ships with one 'Firewall' Dashboard, which is what you are greated with upon accessing the above URL.

The installer also ships with a predefined configuration that is expecting firewalls logs in `/var/log/firewalltype.log`. This can be changed if required from `/etc/logstash/firewalls.conf`

![LGConfig1](/images/ss3.JPG?raw=true)

Also included in the configuration is an option to receive such logs via syslog. This is 100% configurable. You may assign any port or protocol that you wish. Any TCP port specified can also support TLS encryption, however certificate generation is not in scope of this release. This is also configurable from from `/etc/logstash/firewalls.conf`. Simple uncomment the port stansaz and comment the file stanzas.

![LGConfig2](/images/ss4.JPG?raw=true)

Any changes made to this file require a restart of logstash:

`systemctl restart logstash` or `service logstash restart` for older systems.

### Points to Note:

**IPTables**

If you are using IPTables on the Broadbot host itself, or shipping logs from another host, the message within the IPTables log for any block filter must contain the word 'Dropped'. Otherwise, Broadbot will mark these packets with a 'pass' action.

*Example of a block action:*

`Apr 23 07:42:55 Server kernel: IPTables-Inbound-Dropped: IN=eth0 OUT= MAC=04:01:e7:93:12:01:5c:45:27:79:03:30:08:00 SRC=1.2.3.4 DST=5.6.7.8 LEN=44 TOS=0x00 PREC=0x00 TTL=56 ID=63836 PROTO=TCP SPT=43517 DPT=81 WINDOW=22997 RES=0x00 SYN URGP=0`

*Examples of a pass action:*

`Apr 24 07:09:50 Server kernel: IPTables-Outbound: IN= OUT=tun0 SRC=1.2.3.4 DST=5.6.7.8 LEN=60 TOS=0x00 PREC=0x00 TTL=64 ID=22977 DF PROTO=TCP SPT=56620 DPT=5141 WINDOW=29200 RES=0x00 SYN URGP=0`

`Apr 24 07:09:54 Server kernel: IPTables-Inbound: IN=tun0 OUT= MAC= SRC=1.2.3.4 DST=5.6.7.8 LEN=60 TOS=0x00 PREC=0x00 TTL=64 ID=120 DF PROTO=TCP SPT=34616 DPT=10050 WINDOW=29200 RES=0x00 SYN URGP=0`

### Troubleshooting:

* My Dashboard is Blank!

1st and foremost, make sure you are sending some logs! Confirm you have logs in the predefined directories, or that your syslog devices are indeed sending logs.

If you've confirmed this, and your dashboard is still blank - we need to confirm that logs are being ingested correctly.

Navigate to the 'Discover' tab - this will display log messages in a textual and tabular form.

![Discover](/images/ss5.jpg?raw=true)

If you see that there are indeed log messages, then we may need to refresh the field lists. Since Broadbot is built on elasticsearch, a predefined set of feilds is needed for it to work. When new data flows in, elasticsearch needs to be made aware that these fields are now available.

Navigate to the 'Management' tab and click on 'Index Patterns'

![Management1](/images/ss6.JPG?raw=true)

Select the 'broadbot-*' index pattern and click on the 'refresh feilds' button as show below:

![Management2](/images/ss7.JPG?raw=true)

Head back to your Dashboard and hit the search button on the top right. Your dashboard should now be populated.

