### **STEP 1**   

Install the dependency packages  

```bash  
apt install php7.3 php7.3-gd php7.3-cli  
```

### **STEP 2**   

Install snmp  

```bash  
apt install snmp snmpd -y  
```

### **STEP 3**  

Edit the file  
Uncomment the line: rocommunity public localhost  

```bash  
vim /etc/snmp/snmpd.conf  
```

### **STEP 4**   

Restart the SNMP  

```bash  
Service snmp restart  
```

### **STEP 5**   

Test the SNMP service  

```bash  
snmpwalk -v1 -c public localhost  
```

### **STEP 6**   

Install the MRTG   
 
```bash   
apt install mrtg  
```

### **STEP 7**   

Make a folder in “ /var/www/html ” for mrtg files and webpages  

```bash  
mkdir /var/www/html/mrtg  
```

Create a MRTG DIR for files of hosts monitoring  

```bash  
mkdir /etc/mrtg/  
```


### **STEP 8**   

Create config file  

```bash  
localhost:  

cfgmaker public@localhost > /etc/mrtg/01-localhost.cfg  

Remote device:  

cfgmaker public@172.20.1.10 > /etc/mrtg/02-switch.cfg  
```


### **STEP 9**  

Open “ mrtg.cfg “ file for making some changes  

```bash  
vim /etc/mrtg/01-localhost.cfg  
```
Below Global Config Options In the first few lines, add  

```bash  
RunAsDaemon: Yes  

Scroll down and find   
```  

“ # for Debian  
WorkDir: /var/www/mrtg  
”  

And edit it to  

WorkDir: /var/www/html/mrtg  



### **STEP 10**  

Create a index file for mrtg Graph  

indexmaker /etc/mrtg/01-localhost.cfg > /var/www/html/mrtg/01-localhost.html  

**If an error of the type “ occurs” appears:** 

*ERROR: did not find any matching data in cfg file*  

*You need uncomment lines on file 01-localhost.html*  


### **STEP 11**   

Create a index file for mrtg Graph  

```bash  
indexmaker /etc/mrtg/01-localhost.cfg > /var/www/html/mrtg/01-localhost.html  
```   

### **STEP 12** 

Open your browse  

localhost/mrtg  


### **STEP 13**   

For each configuration file, you need to run the command below.  

```bash  
env LANG=C /usr/bin/mrtg /etc/mrtg/01-localhost.cfg  
```

### **STEP 14**  

Create a script to initialize all files and add it to RC.Local.  

```bash  
vim /usr/local/bin/mrtg.sh  
#!/bin/bash  

env LANG=C /usr/bin/mrtg /etc/mrtg/01-localhost.cfg  
env LANG=C /usr/bin/mrtg /etc/mrtg/02-mikrotik.cfg  
env LANG=C /usr/bin/mrtg /etc/mrtg/03-switch.cfg  
env LANG=C /usr/bin/mrtg /etc/mrtg/04-router.cfg  
```


add to RC.Local  

```bash  
vim /etc/rc.local  
sh /usr/local/bin/mrtg.sh  
```
OR  

```bash  
mv /usr/local/bin/mrtg.sh /etc/inid.d/  
```
```bash  
cd /etc/init.d/  
chmod 777 mrtg.sh  
update-rc.d mrtg.sh defaults  
```