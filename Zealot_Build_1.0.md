# Zealot Build 1.0

The analyst workstation is a collections of tools used to collect and analyze data.  This workstationi can work as part of the overall Open Operations Center system or standalone.

This system is built with the intent to collect, analyze and correlate information across all data domains.

Functions for this software bulid include the following modes:

### Analyst Workstation
    Integration with security operations services for event triage.

### IR Workstation
    Incident Response and Forensics

### Sensor Workstation
    Data Collection and Processing

-------------------------------------


## Install CENTOS07

http://isoredirect.centos.org/centos/7/isos/x86_64/CentOS-7-x86_64-DVD-1804.iso

- Choose Creative/Development Workstation Build
- Install OS on SSD drive
- Set Root PW
- Update repos and system after install is complete

### PCAP Storage Folders
Make directories for the elasticsearch indicies as well as the moloch pcaps to use in later configurations.

    sudo mkdir /home/moloch_pcaps
    
    sudo mkdir /home/elastic_indicies


## Instal Elasticsearch 
*https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.4.1.tar.gz*
>tar -xzvf "elasticsearch=6.4.1.tar.gz"
>sudo rpm -i elasticsearch-6.4.1.rpm

_assign elasticsearch an ip that can be accessable from a vritual machine on a bridged connection to dump endpoint collections into_

### Elasticsearch settings
    sudo nano /etc/elasticsearch/elasticsearch.yml
 - change: hosted ip address -> 172.16.xx.1
 - change: data path -> /home/elastic_indicies
 Add firewall exception.
 
    sudo firewall-cmd --permanent --add-port=9200/tcp
    sudo firewall-cmd --reload
 
## Install Kibana


### Kibana Settings


## Install Packetbeat
### Packetbeat Settings
    sudo nano /etc/packetbeat/packetbeat.yml
- Change elasticsearch output [localhost:9200] -> [172.16.xx.1:9200]
- Uncomment and set to 'True' -> xpack.monitoring.enabled: True

## Install Filebeat



## Install VMware Workstation Pro 

**Any virtualization software will work, but VMware Workstation is what is tested**

*Install according to the VMware proceedures for CentOS07*

    https://www.vmware.com/products/workstation-pro.html

- Advise not signing up for checking for updates every time the software is started
- Advise not checking the "send VMware info to make this product better box"

'''
Licensing:  Generally if you are working in a large organization there is some sort of enterprise VMware deployment.  With many of the versions of vSphere come obligatory sets of Workstation Pro, get with your   purchasing manager to login to the my.vmware.com account and find the workstation licenses that no one is using! 

If you are an individual or don't have that resource available, I recommend using the Vmware Advantage Program 
    https://www.vmug.com/Join/VMUG-Advantage-Membership
for 200 dollars you get access to all the enterprise software for a year.
'''

### Create Windows 10 Virtual Machine
Start vmware workstation

> sudo vmware

Create a virtual machine with the following minimum specs:

OS|CPU|RAM|Disk|NIC
--|---|---|----|---
Win10x64|4|4GB|100GB

Options
- Store virtual disk as single file
- save as /home/vms/Win10-64-AW1
- Install VMware Tools

### Install Endpoint Collection Tool
- Because most endpoints are windows, this is done on the windows box

###

## Install Moloch

Download Moloch project from: https://files.molo.ch/builds/centos-7/moloch-1.5.3-1.x86_64.rpm

Install the RPM

> sudo yum install moloch-1.5.3-1.x86_64.rpm

After instalation navigate to the * /data/mooch * folder.

Start the elasticsearch instance:

> sudo systemctl start elasticsearch

And verify the node is up and running on the vmnet 8 interface.

> curl http://172.16.xx.1:9200

Follow the steps in the README file to setup using the aobve address for the elasticsearch node and the physical ethernet nic as the collection interface.

Modify the /data/moloch/etc/config.ini file:

> nano /data/moloch/etc/config.ini

 - pcap storage -> /home/moloch_pcaps
 - elastic ip -> http://172.16.xx.1:9200
 
 Download the latest intel files here:

 https://updates.maxmind.com/app/update_secure?edition_id=GeoLite2-ASN
 https://updates.maxmind.com/app/update_secure?edition_id=GeoLite2-Country
 https://raw.githubusercontent.com/wireshark/wireshark/master/manuf
 
 
 gunzip mmdb files and copy files into /data/moloch/etc folder
 
 >gunzip GeoLite2-ASN.mmdb.gz .
 >gunzip GeoLite2-ASN.mmdb.gz .
 
 * if no other files are in your download dictory----otherwise do one by one *
 >sudo mv /Downloads/* /data/moloch/etc/
 

After all of the steps have been followed start up the two moloch services:

> sudo systemctl start molochcapture
> sudo systemctl start molochviewer

/ You can verify they started with <sudo systemctl status servicename> and then navigating to http://localhost:8005
    












