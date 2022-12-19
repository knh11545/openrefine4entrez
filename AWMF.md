Search AWMF Guideline Register
==============================================================================

The (as of fall 2022) new [Guideline Register](https://register.awmf.org/) of the German medical societies' federation [AWMF](https://www.awmf.org/) has a novel search interface. At the time of writing this text it says that it is in beta state. 

The search capabilities are as yet limited (from an expert searchers point of view). Boolean Operators are not currently supported, nor are brackets (to be used in combination). Boolean operators supposedly are a thing to come in the future. There is no wildcard (such as \*) for truncation but according to a message "truncation ist done automatically". In my experience it is more a kind of linguistic lemmatization that is applied. Nouns and adjectives may ntot be not treated to be the same, according to my limited experience. See examples below.

## Search terms


An example of a list of search terms, each term on its own line:

```

search_term
Kieferorthopädie
kieferorthopädisch
Malokklusion

```

Start a new project in OpenRefine. Paste the search terms from the clipboard or load a text file. The column with the search terms must be named `search_term`. I found it easiest to add the column name as the first line and then choose TSV file format.


## Search using API

The website is using an API that is requested by Ajax calls. We use that API to get nicely formatted JSON. URL pattern: <https://awmf-leitlinien.dev.howto.health/api/dev/search?sorting=relevance&limit=20&offset=0&api_key=MkI5Y1VIOEJ0ZGpoelNBVXRNM1E6WVFld0pBUF9RLVdJa012UHVPTmRQUQ==&lang=de?keywords=search_term>

OpenRefine code to carry out the searches:

Beware: We just fetch the first 50 records for each search (`limit` parameter in the query URL). You may need to adjust this value if result counts (see below) are higher than the limit.

```json

[
  {
    "op": "core/column-addition-by-fetching-urls",
    "engineConfig": {
      "facets": [],
      "mode": "row-based"
    },
    "baseColumnName": "search_term",
    "urlExpression": "grel:\"https://awmf-leitlinien.dev.howto.health/api/dev/search?sorting=relevance&limit=50&offset=0&api_key=MkI5Y1VIOEJ0ZGpoelNBVXRNM1E6WVFld0pBUF9RLVdJa012UHVPTmRQUQ==&lang=de&keywords=\" + value",
    "onError": "set-to-blank",
    "newColumnName": "search_result_json",
    "columnInsertIndex": 1,
    "delay": 5000,
    "cacheResponses": true,
    "httpHeadersJson": [
      {
        "name": "authorization",
        "value": ""
      },
      {
        "name": "user-agent",
        "value": "OpenRefine 3.6.1 [5fc8883]"
      },
      {
        "name": "accept",
        "value": "*/*"
      }
    ],
    "description": "Create column search_result_json at index 1 by fetching URLs based on column search_term using expression grel:\"https://awmf-leitlinien.dev.howto.health/api/dev/search?sorting=relevance&limit=50&offset=0&api_key=MkI5Y1VIOEJ0ZGpoelNBVXRNM1E6WVFld0pBUF9RLVdJa012UHVPTmRQUQ==&lang=de&keywords=\" + value"
  }
]

```

Extract the result count for each search:


```json

[
  {
    "op": "core/column-addition",
    "engineConfig": {
      "facets": [],
      "mode": "row-based"
    },
    "baseColumnName": "search_result_json",
    "expression": "grel:value.parseJson().meta.get(\"hits\")",
    "onError": "set-to-blank",
    "newColumnName": "result_count",
    "columnInsertIndex": 2,
    "description": "Create column result_count at index 2 based on column search_result_json using expression grel:value.parseJson().meta.get(\"hits\")"
  }
]

```


## Export search history

At this stage it is convenient to export a search history from OpenRefine, e.g. search terms and their record counts. 

Use the Custom Tabular Exporter to export to a tsv-file:

![Export options: Content](media/AWMF/export_search_history_01.png)
![Export options: Download](media/AWMF/export_search_history_02.png)


## Extract record data

We just extract some minimal data about the guidelines.

```json



```


