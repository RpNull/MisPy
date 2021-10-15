# MispPy
Query FireEye API for MISP ingestion.
Basic pythonic implementation for querying the FireEye Intelligence api for automated MISP injestion.


## Leverages:

  [FireEye Intel APIv3](https://api.intelligence.fireeye.com/docs#introduction-intel-apiv3)\
  [MISP](https://www.misp-project.org/documentation/)

## Requires:
  Python3.9 or newer\
  [pymisp](https://pymisp.readthedocs.io/en/latest/)\
  python-dotenv

`pip3 install python-dotenv`  
`pip3 install pymisp`


## Usage:
Add required variables to your .env file, in the same directory as FirePy.\
The APP_NAME variable is used to identify Api activety by FireEye.
```
PUB=FireEyePublicKeyHere
PRIV=FireEyePrivateKeyHere
OUTPATH=/Path/To/Output/Here/
APP_NAME=OrganizationalNameHere
```
Add required MISP variables to keys.py
```
#!/usr/bin/env python
# -*- coding: utf-8 -*-

misp_url = 'https:///'
misp_key = 'Your MISP auth key' # The MISP auth key can be found on the MISP web interface under the automation section
misp_verifycert = True
```


Execute the python file MisPy.py

`python3 MisPy.py`

Output will be directed to your MISP instance as unpublished events. You can use PyMisp to automate publishing them. Replace x/y with the event ID range you wish to publish\
```
misp=ExpandedPyMISP(misp_url, misp_key, misp_verifycert)
try:
  for i in range(x,y):
    misp.publish(i)
except Exception as e:
  print(e)
```

## Limitations:

50,000 queries per day, 1000 queries per second. Reference comments in the python file for individual endpoint limitations. All lengths are set to the maximum by default.

Recommended to run as schtask or cronjob. Edit to your paths to update events at 2:30am daily via cron.
```
#!/bin/bash

line="30 2 * * * /path/to/python3 /path/to/MisPy.py"
(crontab -u $(whoami) -l; echo "$line" ) | crontab -u $(whoami) -
```
```
apt-get install python-pip3;pip3 install python-dotenv pymisp;cd /opt && git clone https://github.com/RpNull/MisPy;useradd MisPy -s /bin/bash;chmod -R 770 /opt/MisPy;chown -R MisPy:MisPy /opt/MisPy;mkdir /var/log/MisPy && touch /var/log/MisPy;chmod -R 770 /var/log/MisPy;chown -R MisPy:MisPy /var/log/MisPy && su MisPy;line="30 2 * * * /usr/bin/python3 /opt/MisPy/MisPy.py 1";(crontab -u $(whoami) -l; echo "$line" ) | crontab -u $(whoami) -
```

