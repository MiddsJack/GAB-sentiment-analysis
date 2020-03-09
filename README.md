# GAB-sentiment-analysis
As part of my dissertation im attempting to conduct an analysis of posts from GAB surrounding certain dates. 
Here is the dataset https://files.pushshift.io/gab/
Here is the documentation provided for the API https://docs.google.com/document/d/1aN7DZxluW4Ty884AwnNB-Nzk0FHOsqK4DIEo8hG4e4I/edit
Ive had some success using the URI search but cant seem to convert the fies into a usable CSV or to get the python snippet working. When i try to run it in jupyter it throws an error at sys.exit(r.content)
import requests
import collections
import json
import sys

def query_es(term):

    base_url = 'https://gab.pushshift.io/search'
    headers = {'Content-type': 'application/json'}
    q = collections.defaultdict(lambda : collections.defaultdict(dict))
    q['query']['match']['body'] = term
    q['sort']['created_at'] = 'desc'
    q['size'] = 100
    r = requests.get(base_url, headers=headers, data=q)
    if r.status_code == 200:
        return r.json()
    else:
       sys.exit(r.content)

result = query_es('president|trump')

if 'hits' in result:
    for obj in result['hits']['hits']:
        post = obj['_source']
        print(post)
        
#This is the code provided in the documentation.
