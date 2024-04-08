# SEL
Simple Elastic Language provides a query language to quickly explore and analyze complex datasets of images on Elasticsearch.  
  
The project is split into two sub projects:  
- [SEL](https://github.com/SimpleElasticLanguage/sel), which is the library  
- [SEL Server](https://github.com/SimpleElasticLanguage/server), unlock quick usage by connecting directly to ES.  


## Versions
Two first digits of SEL version match Elasticsearch version and then it's the inner SEL version, eg 7.17.1 works with ES 7.17, v1 of SEL for this version of ES


## Full documentation
[SEL doc](https://SimpleElasticLanguage.github.io/sel) - Containing [Big queries' examples](https://SimpleElasticLanguage.github.io/sel/query_guide.html#big-examples) and all the query synthax  
[SEL Server doc](https://SimpleElasticLanguage.github.io/server/)  


## Compagny
SEL was initially developed for Heuritech in 2016 and used by everybody inside the compagny since that time, to explore, analyse and make reports on their own dataset of images.  


## Quickstart
SEL is using index schema to generate queries.  
Be aware it will request ES schema at any query generation.  

#### Add as dependency
```
sel @ git+https://github.com/SimpleElasticLanguage/sel.git@v7.17.1
```

#### SEL as ES interface
```
from elasticsearch import Elasticsearch
from sel.sel import SEL

es = Elasticsearch(hosts="http://elasticsearch")
sel = SEL(es)
sel.search("my_index", {"query": "category = person"})
```

#### SEL as ES query generator
```
from elasticsearch import Elasticsearch
from sel.sel import SEL

es = Elasticsearch(hosts="http://elasticsearch")
sel = SEL(es)
sel.generate_query({"query": "category = cat"}, index="my_index")["elastic_query"]
```

#### SEL as offline ES query generator
```
from sel.sel import SEL

sel = SEL(None)
sel.generate_query({"query": "supercategory = animal"}, schema=my_index_schema)["elastic_query"]
```

#### SEL as API (SEL Server)
See [SEL Server](https://github.com/SimpleElasticLanguage/server) for API usage


#### Dataset MS COCO 2017 Colorized
You need to get a dataset to test the service.

This dataset has been generated from the official MS COCO 2017, without the person keypoints, using the convertor.py, and colors has been added by kmeans.py  
```
git clone https://github.com/SimpleElasticLanguage/datasets.git
cp datasets/datasets/ms_coco_2017/ms_coco_2017_colorized_head_10k.ndjson .
cp datasets/datasets/ms_coco_2017/schemas/schema_es_7.json .
```
  
or, to fetching the full dataset (123k images):  
```
wget http://simpleelasticlanguage.com/datasets/ms_coco_2017/ms_coco_2017_colorized.ndjson
wget http://simpleelasticlanguage.com/datasets/ms_coco_2017/schemas/schema_es_7.json
```

First time you need to insert some data.  
```
./scripts/elastic.py ms_coco_2017_colorized_head_10k.ndjson schema_es_7.json ms_coco_2017 --http-auth user:pwd -v
```

  
## Makefile rules  
  
 - **docker** - Build SEL docker
 - **docker-test** - Build SEL test docker
 - **lint** - Lint the code
 - **tests** - Run all tests
 - **upshell** - Up a shell into the docker, useful to run only few tests.  
 - **down-tests** - Down tests, in case of failed tests
 - **install-sphinx** - Install Sphinx and dependencies to generate documentation.  
 - **doc** - Generate the documentation in `docs/build/html/`  
 - **clean** - Clean all `__pycache__`


## Known issues

### Elasticsearch

#### Max virtual memory

Fail to start with the following error
```
[1]: max virtual memory areas vm.max_map_count [65530] is too low, increase to at least [262144]
```

Execute the following command
```
sysctl -w vm.max_map_count=262144
```

#### AccessDeniedException

Fail to start with the following error
```
Caused by: java.nio.file.AccessDeniedException: /usr/share/elasticsearch/data/nodes
```

Execute the following command
```
chown -R 1000:root /usr/share/elasticsearch/data
```