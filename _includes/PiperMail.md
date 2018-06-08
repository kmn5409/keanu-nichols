

```python
import augur
from augur.PiperReader import PiperMail

# import everything from githubapi.py and ghtorrent.py so we can
# just copy and paste our function later
import json
import pandas as pd
from perceval.backends.core.pipermail import Pipermail
import os, os.path

# create an Augur application so we can test our function
print(os.getcwd())
if("notebooks" not in os.getcwd()):
    os.chdir("notebooks")
augurApp = augur.Application('../augur.config.json')
print(os.getcwd())
link = "https://lists.opendaylight.org/pipermail/"
mail = ["aalldp-dev","aaa-dev","advisory-group","affinity-dev","alto-dev","archetypes-dev"]
#print(os.getcwd())
file = "opendaylight-"
if "notebooks" in os.getcwd():
    os.chdir("..")
path = "/augur/data/opendaylight-" 
for x in range(len(mail)):
    #print(link+mail[x])
    #print(os.getcwd())
    #print(os.path.join(os.getcwd() + path+'.json'))
    place = os.path.join(os.getcwd() + path + mail[x] +'.json')
    if(os.path.exists(place)):
        print(mail[x])
        print("File Exists")
        continue                   
    repo = Pipermail(url = "https://lists.opendaylight.org/pipermail/"+ mail[x] + "/",dirpath="tmp/archives"+mail[x])
    #print(repo)
    outfile = open(place,"w+")
    for message in repo.fetch():
        obj = json.dumps(message, indent=4, sort_keys=True)
        outfile.write(obj)
        outfile.write('\n')
    outfile.close()
    print("Created File",mail[x])
print("Finished downloading files")
```

    2018-06-08 15:19:46 keanu-Inspiron-5567 perceval.backends.core.pipermail[1679] INFO Looking for messages from 'https://lists.opendaylight.org/pipermail/archetypes-dev/' since 1970-01-01 00:00:00+00:00
    2018-06-08 15:19:46 keanu-Inspiron-5567 perceval.backends.core.pipermail[1679] INFO Downloading mboxes from 'https://lists.opendaylight.org/pipermail/archetypes-dev/' to since 1970-01-01 00:00:00+00:00


    /home/keanu/temp/augur7/augur
    /home/keanu/temp/augur7/augur/notebooks
    aalldp-dev
    File Exists
    aaa-dev
    File Exists
    advisory-group
    File Exists
    affinity-dev
    File Exists
    alto-dev
    File Exists


    2018-06-08 15:19:48 keanu-Inspiron-5567 perceval.backends.core.pipermail[1679] INFO 1/1 MBoxes downloaded
    2018-06-08 15:19:48 keanu-Inspiron-5567 perceval.backends.core.mbox[1679] INFO Done. 2/2 messages fetched; 0 ignored
    2018-06-08 15:19:48 keanu-Inspiron-5567 perceval.backends.core.pipermail[1679] INFO Fetch process completed


    Created File archetypes-dev
    Finished downloading files



```python
temp = augurApp.piper()
temp.make()
print(temp)
#temp.make()
#print(temp.make)
```

    ugh
    Hey
    File exists
    File exists
    File exists
    File exists
    File exists
    File uploaded
    Finished
    <augur.PiperReader.PiperMail object at 0x7f2f6d148cf8>

