# node-elasticsearch

Elasticsearch is a distributed, free and open search and analytics engine for all types of data, including textual, numerical, geospatial, structured, and unstructured

### What is Elasticsearch used for?

The speed and scalability of Elasticsearch and its ability to index many types of content mean that it can be used for a number of use cases:

- Application search
- Website search
- Enterprise search
- Logging and log analytics
- Infrastructure metrics and container monitoring
- Application performance monitoring
- Geospatial data analysis and visualization
- Security analytics
- Business analytics

### How does Elasticsearch work?

Raw data flows into Elasticsearch from a variety of sources, including logs, system metrics, and web applications. Data ingestion is the process by which this raw data is parsed, normalized, and enriched before it is indexed in Elasticsearch. Once indexed in Elasticsearch, users can run complex queries against their data and use aggregations to retrieve complex summaries of their data

### What is Logstash used for?

Logstash, one of the core products of the Elastic Stack, is used to aggregate and process data and send it to Elasticsearch. Logstash is an open source, server-side data processing pipeline that enables you to ingest data from multiple sources simultaneously and enrich and transform it before it is indexed into Elasticsearch

### What is Kibana used for?

Kibana is a data visualization and management tool for Elasticsearch that provides real-time histograms, line graphs, pie charts, and maps. Kibana also includes advanced applications such as Canvas, which allows users to create custom dynamic infographics based on their data, and Elastic Maps for visualizing geospatial data.

### Why use Elasticsearch?

- **Elasticsearch is fast**:
  Because Elasticsearch is built on top of Lucene, it excels at full-text search. Elasticsearch is also a near real-time search platform, meaning the latency from the time a document is indexed until it becomes searchable is very short — typically one second

- **Elasticsearch is distributed by nature**:
  The documents stored in Elasticsearch are distributed across different containers known as shards, which are duplicated to provide redundant copies of the data in case of hardware failure

- **Elasticsearch comes with a wide set of features**: In addition to its speed, scalability, and resiliency, Elasticsearch has a number of powerful built-in features that make storing and searching data even more efficient, such as data rollups and index lifecycle management.

- **The Elastic Stack simplifies data ingest, visualization, and reporting:**
  Integration with Beats and Logstash makes it easy to process data before indexing into Elasticsearch. And Kibana provides real-time visualization of Elasticsearch. APM - Application performance monitoring.

---

### Installation

1. Install elastc search with docker image
   `docker pull docker.elastic.co/elasticsearch/elasticsearch:8.5.1`

2. Create a new docker network for Elasticsearch and Kibana.
   `docker network create elastic`

3. Run docker container to start elastic search
   `docker run --name elasticsearch --net elastic -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" -t docker.elastic.co/elasticsearch/elasticsearch:8.5.1`

4. Kibana installation docker - Data visualization tool
   `docker pull docker.elastic.co/kibana/kibana:8.5.1`

5. Run kibana docker container in same network
   `docker run --name kibana --net elastic -p 5601:5601 docker.elastic.co/kibana/kibana:8.5.1`
   Use the username and password, enrollment token to open the kibana in browser.

---

### TLS security.

You can access the elastic search through the http API calls.

Each API call must be attached with .crt key which is used for TLS security and Authrorization header with username and password.

---

### Mapping -

Mapping is the outline of the documents stored in an index. Similar to database schema before inserting any data to database.

---

### Elastic search basic API's

**CREATE Index**

When createing index we can specify the following.

1. Settings for the index.
2. Mapping field in the index.
3. Index alias

**POST /customer/\_doc/1**
This request automatically creates the customer index if it doesn’t exist, adds a new document that has an ID of 1, and stores and indexes the firstname and lastname fields.

---

### Bulk Write

**PUT /customer/\_bulk**

```javascript
{ "create": { } }
{ "firstName": "Monica","lastName":"Rambeau"}

{ "create": { } }
{ "firstName": "Carol","lastName":"Danvers"}

{ "create": { } }
{ "firstName": "Wanda","lastName":"Maximoff"}

{ "create": { } }
{ "firstName": "Jennifer","lastName":"Takeda"}

```

---

### Index API

Create Index with config

```
PUT {index}
{
  "settings" : {
      "index" : {
         "number_of_shards" : 3,
         "number_of_replicas" : 2
      }
   }
}
```

Create Index Mapping

```
PUT products
{
"mappings": {
    "properties": {
      "title": { "type": "text" },
      "description": {"type": "text"},
      "qty": {"type": "integer"},
      "price": {"type": "double"}
    }
  }
}
```

Insert data to index

```
POST products/_doc/1
{
  "title": "Redmi Watch",
  "description": "Redmi fitness band to track heart beat, and blood presure",
  "qty": 10,
  "price": 5000
}
```

Automatic index and underplaying mapping can be blocked by changing elasticsearch.yml config

```
action.auto_create_index:false
index.mapper.dynamic:false
```

GET all docs

```
GET products/_search
```

GET DOC by ID

```
GET products/_doc/1
```

Paginated search result

```
GET products/_search
{
  "from": 0,
  "size": 2
}
```

Update DOC elstic search

```
GET products/_update/1
{
   "script" : {
      "source": "ctx._source.title = params.title",
      "lang": "painless",
      "params" : {
         "title" : "Samsung S20 FE 5G"
      }
   }
 }
```

Delete DOC

```
DELETE products/_doc/4
```

### Delete Index

```
DELETE /{index}
```

### Search Query

GET customer/\_search

```
{
  "query" : {
    "match" : { "firstname": "Jennifer" }
  }
}
```

Match all doc in all index which contains given title

```
GET /_all/_search?q=title:Samsung S20 FE 5G
```

GET Projection

```
GET /customer/_doc/{_id}?_source_includes={field}
```

---

### Mapping.

Mapping defines the schema to record the doc in an index.

1. Create mapping.
2. Add new field to existing mappings.
3. Get view of mappings.

### Text Analysis

Text analysis is the process of converting unstructured text, like the body of an email or a product description, into a structured format that’s optimized for search.
Elasticsearch performs text analysis when indexing or searching text fields.

1. Tokenization - Breaking text into small chunk

2. Normalization - To normalize the base term and with synonymous meaning can match

**Text analysis is performed by analyzer.**

Default analyzer is **standard analyzer**

**The building blocks of any searchengine are tokenizers, token-filters and analyzers**

**Tokenizers**
Tokenization is a process of breaking the strings into sections of strings or terms called tokens based on a certain rule.

**Token Filters**
Token filters operate on tokens produced from tokenizers and modify the tokens accordingly.
Ex: Lower case conversion

**Analyzer**
Analyzer is a combination of tokenizer and filters that can be applied to any field for analyzing in Elasticsearch. There are already built in analyzers available in Elasticsearch.

```

```
