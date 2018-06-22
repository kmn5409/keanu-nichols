

```python
import nltk, re, pprint
from nltk import word_tokenize
import os,json
from nltk.corpus import stopwords
from nltk.tokenize import sent_tokenize 
import nltk.data
from nltk.sentiment.vader import SentimentIntensityAnalyzer
from nltk import sentiment
from nltk import word_tokenize
%matplotlib inline
#nltk.download('punkt')
#nltk.download('stopwords')
#pip install twython
#nltk.download('vader_lexicon')
```


```python
def read_json(p):
    k = j = 0
    y=""
    for line in p:
        if(p[j:j+9] == "\"origin\":" or p[j:j+11] == "\"unixfrom\":"):
            k+=1
        y+=line
        j+=1
        if(k==2 and line == "}"):
            break
    return y,j
```


```python
link = "https://lists.opendaylight.org/pipermail/"
archives = ["aalldp-dev","archetypes-dev"]
path = "/augur/data/"

if "notebooks" in os.getcwd():
    os.chdir("..")

for i in range(len(archives)):
    place = os.getcwd() + path + 'opendaylight-' + archives[i]
    name = os.getcwd() + path + archives[i]
    f = open(place + '.json','r')
    x = f.read()
    #print(x,"\n\n\n")
    temp = json.dumps(x)
    f.close()
    data,j = read_json(x)
    di = json.loads(data)
    #print(repr((di['data']['body']['plain'])))
    while(j<len(x)):
        raw = di['data']['body']['plain']
        #print(raw)
        # Next, we initialize VADER so we can use it within our Python script
        sid = SentimentIntensityAnalyzer()

        # We will also initialize our 'english.pickle' function and give it a short name

        tokenizer = nltk.data.load('tokenizers/punkt/english.pickle')
        message_text = raw
        sentences = tokenizer.tokenize(message_text)
        for sentence in sentences:
            scores = sid.polarity_scores(sentence)
            #print(scores)
            print(sentence)
            for key in sorted(scores):
                    print('{0}: {1}, '.format(key, scores[key]), end='')
            print("\n\n")
        #print("\n\n","-"*70,"\n\n")
        data,r= read_json(x[j:])
        j+=r
        if(j==len(x)):
            break
        print("*"*40,"NEW MESSAGE","*"*40,"\n\n")
        di = json.loads(data)
    print("\n\n\n")
    
```

    Hi Brian Kaczynski <kaczynskib at avaya.com>, Dennis Flynn <drflynn at avaya.com>,
    
    Please reply to this email to indicate that you are still an active committer on this project.
    compound: 0.6124, neg: 0.0, neu: 0.839, pos: 0.161, 
    
    
    Best Regards,
    An Ho
    compound: 0.6369, neg: 0.0, neu: 0.417, pos: 0.583, 
    
    
    **************************************** NEW MESSAGE **************************************** 
    
    
    Hi AALLDP Team,
    
    
    
    1.
    compound: 0.0, neg: 0.0, neu: 1.0, pos: 0.0, 
    
    
    Does your project have any plans to be archived?
    compound: 0.0, neg: 0.0, neu: 1.0, pos: 0.0, 
    
    
    If your project has no plans for any future active development, project committers can vote to move a project to the Archived State by a Termination Review according to the OpenDaylight Project Lifecycle and Releases document [1].
    compound: 0.128, neg: 0.058, neu: 0.871, pos: 0.071, 
    
    
    2.
    compound: 0.0, neg: 0.0, neu: 1.0, pos: 0.0, 
    
    
    If you projects plans to remain active, does it intend to participate in the Boron Simultaneous Release as documented in the Boron Release Plan [2]?
    compound: 0.4019, neg: 0.0, neu: 0.899, pos: 0.101, 
    
    
    If not, could someone please publicly decline to participate with a message like so: "The <PROJECT_NAME> project is formally declining to participate in the Boron SR".
    compound: 0.1376, neg: 0.071, neu: 0.838, pos: 0.091, 
    
    
    There are no project related activity on Gerrit.
    compound: -0.296, neg: 0.239, neu: 0.761, pos: 0.0, 
    
    
    Best Regards,
    
    An Ho
    
    
    
    [1] https://www.opendaylight.org/project-lifecycle-releases
    
    [2] https://wiki.opendaylight.org/view/Simultaneous_Release:Boron_Release_Plan
    
    
    -------------- next part --------------
    An HTML attachment was scrubbed...
    URL: <http://lists.opendaylight.org/pipermail/aalldp-dev/attachments/20160324/6e137881/attachment.html>
    compound: 0.7506, neg: 0.0, neu: 0.726, pos: 0.274, 
    
    
    
    
    
    
    This is just a test.
    compound: 0.0, neg: 0.0, neu: 1.0, pos: 0.0, 
    
    
    -- 
    Andrew J Grimberg
    Lead, IT Release Engineering
    The Linux Foundation
    
    -------------- next part --------------
    A non-text attachment was scrubbed...
    Name: signature.asc
    Type: application/pgp-signature
    Size: 488 bytes
    Desc: OpenPGP digital signature
    URL: <http://lists.opendaylight.org/pipermail/archetypes-dev/attachments/20180417/126c0b23/attachment-0001.sig>
    compound: 0.296, neg: 0.0, neu: 0.932, pos: 0.068, 
    
    
    **************************************** NEW MESSAGE **************************************** 
    
    
    Hello archetypians,
    
    Just to let you all know that
    https://lists.opendaylight.org/pipermail/archetypes-dev/ works now (thanks
    Andy!).
    compound: 0.0, neg: 0.0, neu: 1.0, pos: 0.0, 
    
    
    Just in case anyone in the future every looks back at the first message on
    this list, here is the background to how this all started:
    
    * https://wiki.opendaylight.org/view/Archetypes
    
    * https://lists.opendaylight.org/pipermail/tsc/2018-April/009333.html
    
    * https://jira.linuxfoundation.org/browse/RELENG-854 and its linked issues
    
    Tx,
    M.
    --
    Michael Vorburger, Red Hat
    vorburger at redhat.com | IRC: vorburger @freenode | ~ = http://vorburger.ch
    -------------- next part --------------
    An HTML attachment was scrubbed...
    URL: <http://lists.opendaylight.org/pipermail/archetypes-dev/attachments/20180418/b55eba01/attachment.html>
    compound: 0.5719, neg: 0.0, neu: 0.923, pos: 0.077, 
    
    
    
    
    
    

