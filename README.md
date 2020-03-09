# GAB-sentiment-analysis
import requests
import collections
import json

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
