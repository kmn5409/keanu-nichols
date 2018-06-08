---
layout: post
---
Welcome to my first post! I know it's now week 4 and I'm only now starting a blog, but you know better late than never. So I have to give props to my other fellow [GSoC](https://summerofcode.withgoogle.com/) (Google Summer of Code) developer at [CHAOSS](https://chaoss.community/) (Community Health Analytics Open Source Software) [Pranjal Aswani](https://aswanipranjal.github.io/) for having an amazing layout for his website which I think is a very good structure for talking about what happens each week.

So this week's task with my mentor [Sean Goggins](http://www.seangoggins.net/) is as follows:

Goals for the week: 
1. Pull down mailing lists using Perceval
2. Develop notebooks for making sense of resulting emails
3. Explore the pros and cons of different persistence strategies using two main use cases: 
	- Reserachers trying to understand linguistic trends within and across lists
	- Community managers and researchers identifying a set of email communications containing different linguisist properties.  For example, sentiment analysis or some other linguistic marker.
4. (Stretch Goal) : Experiment with ways of filteing out old message strings from mailing lists and identifying mailing list discussion threads (some lists have them automatically, some don't)

Ongoing Tasks:
1. The development / merge process
2. Getting to the know the metrics
3. Configuring Augur for different data backends (i.e. mailing lists)

Now over the past few weeks I've been working on generally tasks 1-2 and I believe once I reach a bit futher with my mailing list task we will begin to explore goal 3 which I very interested in, it seems like it will have a bit of a challenge (Not that what I haven't been doing before wasn't challengin trust me I spent a good amount of time on it). Also I would say I'm beginning to look at goal 4 at the moment now because I have to start look at threads that are pretty long I think one of the longest threads were like 7,598,287 characters so I'm going to try to break it up. However Sean did bring up a good point that different mailing services can record threads a bit differently so I'm going to have to look at how to get around that.
					
# Goals

Goal 1:
Now this will generall be looking at two sets of files. The first one would be application.py and we will look specifically at the class Application(highlight) and the method piper(). Now as you can tell it's basically the same thing as the method ghtorrentplus (was it copying and pasting yes, yes it was). But I took me a bit to understand what it was doing before because I don't know if you can see but it actually calls the method ghtorrent inside it.

{% highlight python %}
    def piper(self):
        from augur.PiperReader import PiperMail
        if self.__piper is None:
            logger.debug('Initializing PiperMail')
            self.__piper = PiperMail(
                user=self.read_config('GHTorrentPlus', 'user', 'AUGUR_DB_USER', 'root'),
                password=self.read_config('GHTorrentPlus', 'pass', 'AUGUR_DB_PASS', 'password'),
                host=self.read_config('GHTorrentPlus', 'host', 'AUGUR_DB_HOST', '127.0.0.1'),
                port=self.read_config('GHTorrentPlus', 'port', 'AUGUR_DB_PORT', '3306'),
                dbname=self.read_config('GHTorrentPlus', 'name', 'AUGUR_DB_NAME', 'Pipermail')
            , ghtorrent=self.ghtorrent())
        return self.__piper
{% endhighlight %}

Also line 98 I had to initialize piper to be None {% highlight python %} self.__piper = None {% endhighlight %}

And then on line 125 we call the method piper() {% highlight python %} self.piper() {% endhighlight %}

Goal 2:
The question you might be asking of course is so what is this for..... Don't worry I'm getting to it you just need to give me a little more time. So we move onto the next file which is where everything actually comes together and that's the jupyter notebook. So it's a bit of stuff going on here we have thing something called perceval which is used to retreieve data from a bunch of sources. Since I'm working with mailing lists we use something called Pipermail but it has other data sources such as we all know and love github and funny enough slack which is how I communicate with Sean and everyone at Augur. So I was using the opendaylight.org/pipermail and they had a bunch of mailing lists and so to iterate through some of them I created a list of the different ones. So then we send perceval on his task and it calls the Pipermail function which is a back in perceval and it downloads all the mail boxes int a folder called archives under another folder called temp. This would take a few minutes because some of the mailing lists were pretty long. I then have to iterate throught the messages, and just output it to a file.

{% include PiperMail.md %}

Now we move onto the file PiperReader, this is called from the jupyter notebook and this first called by augurApp.piper and what this does is sent all the credentials you had to put a file called augur.config.json and that's how you connect to your sql database. Then we use temp.make() (I know my naming convention is amazing) which then go through each of the json files we created in the jupyter notebook and so what I do after is go through the files which is in the function read_json. Now I was wondering if there was a faster way to do this but to my knowledge and what I found through what I felt was a lotttttt of Googling was that because it was made up of a number of JSON objects Ijust have to iterate through the whole file and add it to a variable y and i seperated it by using a variable k which just helped me decide if the JSON object was going to close. The good thing is that I can now load this into a function called json.loads() which then allows me to select specific things easily. after we then just convert it to a pandas dataframe. My issue was that the threads in some of the messages were incredibly long so that's where I kind of cut off the thread but that's something I need to work on cause we kind want all those messages (lol). After it's as simple as using df.to_sql and upload it to the SQL database (the beauties of python).

{% highlight python %}
import json
import pandas as pd
from sys import exit
import pprint 
#import mysql.connector
from sqlalchemy import create_engine
import sqlalchemy as s
#from sqlalchemy_utils import database_exists, create_database
from augur import logger
from augur.ghtorrentplus import GHTorrentPlus
import os
import augur
#if(line[j:j+11]=="},\"unixfrom\"" or line[j:j+9] == "},\"origin\"" ):
#9359610
#1355
#Need to have pip install sqlalchemy-utils
def read_json(p):
		#print(p,"\n\n")
		k = j = 0
		y=""
		for line in p:
			#print(line,"\n\n")
			if(p[j:j+9] == "\"origin\":" or p[j:j+11] == "\"unixfrom\":"):
				k+=1
				#print(p[temp:j])
			y+=line
			j+=1
			if(k==2 and line == "}"):
				break
		#print(k)
		return y,j

def add_row(columns,df,di):
	temp = 	di['data']['body']['plain']
	words = ""
	for j in range(0,len(temp)):
		words+=temp[j]
		if(temp[j] == "\n" and j+1<len(temp)):
			if(temp[j+1] == ">" or j>10000):
				di['data']['body']['plain'] = words
				break
	li = [[di["backend_name"],di['category'],di['data']['Date'],
		      di['data']['From'],di['data']['Message-ID'],
		      di['data']['body']['plain']]]
	df1 = pd.DataFrame(li,columns=columns)
	df3 = df.append(df1)
	return df3

class PiperMail:
	def __init__(self, user, password, host, port, dbname, ghtorrent, buildMode="auto"):
		"""
		Connect to the database

		:param dbstr: The [database string](http://docs.sqlalchemy.org/en/latest/core/engines.html) to connect to the GHTorrent database
		"""
		char = "charset=utf8"
		self.DB_STR = 'mysql+pymysql://{}:{}@{}:{}/{}?{}'.format(
		    user, password, host, port, dbname,char
		)
		#print('GHTorrentPlus: Connecting to {}:{}/{}?{} as {}'.format(host, port, dbname, char,user))
		self.db = s.create_engine(self.DB_STR, poolclass=s.pool.NullPool)
		self.ghtorrent = ghtorrent

		try:
		    	# Table creation
			if (buildMode == 'rebuild') or ((not self.db.dialect.has_table(self.db.connect(), 'issue_response_time')) and buildMode == "auto"):
				logger.info("[GHTorrentPlus] Creating Issue Response Time table...")
				self.build_issue_response_time()
		except Exception as e:
		    logger.error("Could not connect to GHTorrentPlus database. Error: " + str(e))
		#print("\nHEREEEEEEEE!!!!!!!!!\n",self.temp())
		#GHTorrentPlus.temp
		#piper.temp
		#engine = s.create_engine('mysql+mysqlconnector://root:Password@localhost/Pipermail?charset=utf8')
		#if not database_exists(engine.url):
		 #   create_database(engine.url)
		#print(os.getcwd())		
	def make(self):
		#print(self.db)
		print("ugh")
		archives = ["aalldp-dev","aaa-dev","advisory-group","affinity-dev","alto-dev","archetypes-dev"]
		'''if("augur/notebooks" in os.getcwd()):
				os.chdir("..")
				print(os.getcwd())
				path = os.getcwd() + "/augur/" + "data/" 
		else:
			path = "data/"	'''
		print("Hey")
		path = "/augur/data/"
		for i in range(len(archives)):
			place = os.getcwd() + path + 'opendaylight-' + archives[i]
			name = os.getcwd() + path + archives[i]
			if(os.path.exists(name + '.csv')):
				print("File exists")
				continue
			f = open(place + '.json','r')
			x = f.read()
			temp = json.dumps(x)
			f.close()
			#print(y)
			data,j = read_json(x)
			#print(data,"\n\n")
			# decoding the JSON to dictionay
			di = json.loads(data)

			#print(di)
			# converting json dataset from dictionary to dataframe
			##print(di["data"]["body"]["plain"])
			#pprint.pprint(di)
			#Tried using Subject but sometimes they have fancy symbols that's
			#hard to upload to the database would have to decode it and upload
			#to the database and then encode it back when requesting from
			#the database
			columns = "backend_name","category","Date","From","Message-ID","Text"
			li = [[di["backend_name"],di['category'],di['data']['Date'],
					      di['data']['From'],di['data']['Message-ID'],
					      di['data']['body']['plain']]]
			df = pd.DataFrame(li,columns=columns)
			#print(len(x))
			#print(j)
			while(j<len(x)):
				data,r= read_json(x[j:])
				j+=r
				#print(j,"\n\n\n")
				if(j==len(x)):
					break
				di = json.loads(data)
				df = add_row(columns,df,di)

			df = df.reset_index(drop=True)
			df.to_csv(name + ".csv")
			#print(archives[i])
			df.to_sql(name=archives[i], con=self.db, if_exists = 'replace', index=False,)
			print("File uploaded")
		print("Finished")
	
{% endhighlight %}


Goal 3:
So I think this will happen more when I finish up organizing the database correctly because right now it's not as organized


Goal 4:
I feel like that I will be working on that this upcoming week to tackle to long thread problem


# Ongoing Tasks
Ongoing Task 1:
I think that's just generally doing stuff in GitHub which I'm learning to use more and more now


Ongoing Task 2:
I would say I was working on this at the beginning where I was just learning how to add a metric

Ongoing Task 3:
And well you know that's what mostly this blog post was about.
