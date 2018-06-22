{% highlight python linenos %}
# create an Augur application so we can test our function
Piper = augurApp.piper()
print(os.getcwd())
print(perceval.__path__)
print(os.getcwd())
link = "https://lists.opendaylight.org/pipermail/"
#mail = ["aalldp-dev","alto-dev","archetypes-dev"]
#mail = ["aalldp-dev","alto-dev","archetypes-dev","dev"]
#mail = ["aalldp-dev","archetypes-dev","alto-dev"]
mail = ["aalldp-dev","archetypes-dev"]
mail_check = {key:False for key in mail}
print(mail_check)
#print(os.getcwd())
file = "opendaylight-"
if "notebooks" in os.getcwd():
    os.chdir("..")
path = "/augur/data/opendaylight-" 
for x in range(len(mail)):
    #print(link+mail[x])
    if(mail[x] not in df1['project'].values ):
        #print(os.getcwd())
        #print(os.path.join(os.getcwd() + path+'.json'))
        place = os.path.join(os.getcwd() + path + mail[x] +'.json')           
        repo = Pipermail(url = "https://lists.opendaylight.org/pipermail/"+ mail[x] + "/",dirpath="tmp/archives_"+mail[x])
        #print(repo)
        outfile = open(place,"w+")
        for message in repo.fetch():
            obj = json.dumps(message, indent=4, sort_keys=True)
            outfile.write(obj)
            outfile.write('\n')
        outfile.close()
        mail_check[mail[x]] = 'new'
        print("Created File",mail[x])
    else:
        last_updatedSQL = s.sql.text("""SELECT last_message_date FROM 
        mailing_list_jobs WHERE project = """ +  "'" + mail[x] + "'")
        last_updated_df = pd.read_sql(last_updatedSQL, connect.db)
        time = (last_updated_df['last_message_date'])  
        time = time.astype(object)
        place = os.path.join(os.getcwd() + path + 'temp_' + mail[x] +'.json')       
        repo = Pipermail(url = "https://lists.opendaylight.org/pipermail/"+ mail[x] + "/",dirpath="tmp/archives_"+mail[x])
        outfile = open(place,"w+")
        print(type(time[0]))
        for message in repo.fetch(from_date=time[0]):
            mess_check = Piper.convert_date(message['data']['Date'])
            #mess_check = Piper.convert_date("Thu, 24 Mar 2019 20:37:11 +0000")
            if (mess_check > time[0]):
                obj = json.dumps(message, indent=4, sort_keys=True)
                outfile.write(obj)
                outfile.write('\n')
                print("Updated messages downloaded")
                mail_check[mail[x]] = 'update'
        outfile.close()
        print("Checking to see for updated messages")
print(mail_check)
print("Finished downloading files")
{% endhighlight %}

    2018-06-21 15:10:43 keanu-Inspiron-5567 perceval.backends.core.pipermail[21549] INFO Looking for messages from 'https://lists.opendaylight.org/pipermail/aalldp-dev/' since 1970-01-01 00:00:00+00:00
    2018-06-21 15:10:43 keanu-Inspiron-5567 perceval.backends.core.pipermail[21549] INFO Downloading mboxes from 'https://lists.opendaylight.org/pipermail/aalldp-dev/' to since 1970-01-01 00:00:00+00:00


    /home/keanu/temp/augur_push/augur4/augur/notebooks
    _NamespacePath(['/home/keanu/anaconda3/envs/augur/lib/python3.6/site-packages/perceval'])
    /home/keanu/temp/augur_push/augur4/augur/notebooks
    {'aalldp-dev': False, 'archetypes-dev': False}


    2018-06-21 15:10:45 keanu-Inspiron-5567 perceval.backends.core.pipermail[21549] INFO 1/1 MBoxes downloaded
    2018-06-21 15:10:45 keanu-Inspiron-5567 perceval.backends.core.mbox[21549] INFO Done. 2/2 messages fetched; 0 ignored
    2018-06-21 15:10:45 keanu-Inspiron-5567 perceval.backends.core.pipermail[21549] INFO Fetch process completed
    2018-06-21 15:10:45 keanu-Inspiron-5567 perceval.backends.core.pipermail[21549] INFO Looking for messages from 'https://lists.opendaylight.org/pipermail/archetypes-dev/' since 1970-01-01 00:00:00+00:00
    2018-06-21 15:10:45 keanu-Inspiron-5567 perceval.backends.core.pipermail[21549] INFO Downloading mboxes from 'https://lists.opendaylight.org/pipermail/archetypes-dev/' to since 1970-01-01 00:00:00+00:00


    Created File aalldp-dev


    2018-06-21 15:10:47 keanu-Inspiron-5567 perceval.backends.core.pipermail[21549] INFO 1/1 MBoxes downloaded
    2018-06-21 15:10:47 keanu-Inspiron-5567 perceval.backends.core.mbox[21549] INFO Done. 2/2 messages fetched; 0 ignored
    2018-06-21 15:10:47 keanu-Inspiron-5567 perceval.backends.core.pipermail[21549] INFO Fetch process completed


    Created File archetypes-dev
    {'aalldp-dev': 'new', 'archetypes-dev': 'new'}
    Finished downloading files



