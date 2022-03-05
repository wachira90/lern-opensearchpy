# lern-opensearchpy
lerning opensearchpy

## install 
````
pip install opensearch-py
````


## example
````
#!python
from opensearchpy import OpenSearch

#ca_certs_path = '/full/path/to/cluster-ca-certificate.pem'

#hosts = ["xxx.xxx.xxx.xxx", "xxx.xxx.xxx.xxx", "xxx.xxx.xxx.xxx"]
hosts = ["172.16.1.5"]
port = 9200
#auth = ("icelasticsearch", "<Password>")
#auth = ("", "")

# client = OpenSearch(
#     hosts = hosts,
#     port = port,
#     http_auth = auth,
#     use_ssl = True,
#     verify_certs = True,
#     ca_certs = ca_certs_path
# )

client = OpenSearch(
    hosts = hosts,
    port = port,
    use_ssl = False,
    verify_certs = False,
)

# Testing
# Create an index with non-default settings.
index_name = 'python-test-index'
index_body = {
  'settings': {
    'index': {
      'number_of_shards': 4
    }
  }
}

response = client.indices.create(index_name, body=index_body)
print('\nCreating index:')
print('We get response:', response)

# Add a document to the index created before.
document = {
  'title': 'Moneyball',
  'director': 'Bennett Miller',
  'year': '2011'
}
id = '1'

response = client.index(
    index = index_name,
    body = document,
    id = id,
    refresh = True
)

print('\nAdding document:')
print('We get response:', response)

# Search for the document.
q = 'miller'
query = {
  'size': 5,
  'query': {
    'multi_match': {
      'query': q,
      'fields': ['title^2', 'director']
    }
  }
}

response = client.search(
    body = query,
    index = index_name
)
print('\nSearch results:')
print('We get response:', response)

# Delete the document.
response = client.delete(
    index = index_name,
    id = id
)

print('\nDeleting document:')
print('We get response:', response)

# Delete the index.
response = client.indices.delete(
    index = index_name
)

print('\nDeleting index:')
print('We get response:', response)
````  

more info

https://github.com/opensearch-project/opensearch-py
