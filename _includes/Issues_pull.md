

```python
from perceval.backends.core.github import GitHub
from datetime import date
import datetime
import dateutil.tz
from dateutil.relativedelta import relativedelta
import matplotlib.pyplot as plt
```


```python
# Url for the git repo to analyze
#repo_url = 'https://github.com/chaoss/grimoirelab-perceval'
repo_url = 'CTC'
# Directory for letting Perceval clone the git repo
#repo_dir = 'CTC'
token = 'Insert_github_token_here'#I have my own token
own = 'nodejs'
# ElasticSearch instance (url)
#repo = 'grimoirelab-perceval'

# Create the 'commits' index in ElasticSearch
# Create a Git object, pointing to repo_url, using repo_dir for cloning
repo = GitHub(owner=own,repository=repo_url,api_token=token)
```
