---
layout: default
title: Execute Painless stored script
parent: Script APIs
nav_order: 2
---

## Execute Painless stored script

Runs a stored script written in the Painless language. 

OpenSearch provides several ways to run a script; the following sections show how to run a script by passing script information in the request body of a `GET <index>/_search` request.

### Request fields

| Field | Data Type | Description | 
:--- | :--- | :---
| query | Object | A filter that specifies documents to process. |
| script_fields | Object | Fields to include in output. | 
| script | Object | ID of the script that produces a value for a field. |

#### Sample request

The following request runs the stored script that was created in [Create or update stored script]({{site.url}}{{site.baseurl}}/api-reference/script-apis/create-stored-script/). The script sums the ratings for each book and displays the sum in the `total_ratings` field in the output.

* The script's target is the `books` index.

* The `"match_all": {}` property value is an empty object indicating to process each document in the index.

* The `total_ratings` field value is the result of the `my-first-script` execution. See  [Create or update stored script]({{site.url}}{{site.baseurl}}/api-reference/script-apis/create-stored-script/).

````json
GET books/_search
{
   "query": {
    "match_all": {}
  },
  "script_fields": {
    "total_ratings": {
      "script": {
        "id": "my-first-script" 
      }
    }
  }
}
````

#### Sample response

The `GET books/_search` request returns the following fields:

````json
{
  "took" : 2,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 3,
      "relation" : "eq"
    },
    "max_score" : 1.0,
    "hits" : [
      {
        "_index" : "books",
        "_id" : "1",
        "_score" : 1.0,
        "fields" : {
          "total_ratings" : [
            12
          ]
        }
      },
      {
        "_index" : "books",
        "_id" : "2",
        "_score" : 1.0,
        "fields" : {
          "total_ratings" : [
            15
          ]
        }
      },
      {
        "_index" : "books",
        "_id" : "3",
        "_score" : 1.0,
        "fields" : {
          "total_ratings" : [
            8
          ]
        }
      }
    ]
  }
}
````

### Response fields

| Field | Data Type | Description | 
:--- | :--- | :---
| took | Integer | How long the operation took in milliseconds. |
| timed_out | Boolean | Whether the operation timed out. |
| _shards | Object | Total number of shards processed and also the total number of successful, skipped, and not processed. |
| hits | Object | Contains high-level information about the documents processed and an array of `hits` objects. See [Hits object](#hits-object). | 

#### Hits object

| Field | Data Type | Description | 
:--- | :--- | :---
| total | Object | Total number of documents processed and their relationship to the `match` request field. |
| max_score | Double | Highest relevance score returned from all the hits. |
| hits | Array | Information about each document that was processed. See [Document object](#Document-object). |

#### Document object

| Field | Data Type | Description | 
:--- | :--- | :---
| _index | String | Index that contains the document. |
| _id | String | Document ID. |
| _score | Float | Document's relevance score. |
| fields | Object | Fields and their value returned from the script. |