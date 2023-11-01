# SEL
Simple Elastic Language offer an easy way to query ElasticSearch for every body even no-tech people and even on a big, complex and nested schema.  
  
The project is split into two sub projects:  
- [SEL](https://github.com/SimpleElasticLanguage/sel), which is the library  
- [SEL Server](https://github.com/SimpleElasticLanguage/server), unlock quick usage by connecting directly to ES.  


## Versions
Two first digits of SEL version match Elasticsearch version and then it's the inner SEL version, eg 2.4.1 works with ES 2.4, v1 of SEL for this version of ES


## Full documentation
[SEL doc](https://simpleelasticlanguage.github.io/sel)  
[SEL Server doc](https://simpleelasticlanguage.github.io/server/)  


## Compagny
SEL was initially developed for Heuritech in 2016 and used by everybody inside the compagny tech and no-tech people since that time to explore internal data, generate reports and analysis.


## Quickstart
SEL is using index schema to generate queries.  
Be aware it will request ES schema at any query generation.  

#### How to add as dependency
```
sel @ git+https://github.com/SimpleElasticLanguage/sel.git
```

### SEL as ES interface
```
from elasticsearch import Elasticsearch
from sel.sel import SEL

es = Elasticsearch(hosts="http://elasticsearch")
sel = SEL(es)
sel.search("my_index", "label = bag")
```

### SEL as ES query generator
```
from elasticsearch import Elasticsearch
from sel.sel import SEL

es = Elasticsearch(hosts="http://elasticsearch")
sel = SEL(es)
sel.generate_query("label = bag", index="my_index")["elastic_query"]
```

### SEL as offline ES query generator
```
from sel.sel import SEL

sel = SEL(None)
sel.generate_query("label = bag", schema=my_index_schema)["elastic_query"]
```

### SEL as API (SEL Server)
See [SEL Server](https://github.com/SimpleElasticLanguage/server) for API usage
  
## Makefile rules  
  
 - **docker** - Build SEL docker
 - **docker-test** - Build SEL test docker
 - **lint** - Lint the code
 - **tests** - Run all tests
 - **upshell** - Up a shell into the docker, useful to run only few tests.  
 - **install-sphinx** - Install Sphinx and dependencies to generate documentation.  
 - **doc** - Generate the documentation in `docs/build/html/`  
 - **clean** - Clean all `__pycache__`