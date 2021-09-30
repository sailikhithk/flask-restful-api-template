# Saved Searches Flask Resty API

# Introduction

Saved Search API was built from the grounds with a Flask API that makes it easy for developers and sysadmins to monitor saved and recent searches.

This project shows one of the possible ways to implement RESTful API server for saved and recent searches.

There are implemented with a models having search criteria, search type and last updated date.

Main libraries used:

1. Flask-WTForms - for form validation.
2. Flask-RESTful - restful API library.
3. Flask-Script - provides support for writing external scripts.
4. Flask-SQLAlchemy - adds support for SQLAlchemy ORM.

## Project structure:

```
.
├── README.md
├── db_scripts
│   ├── SchemaScript.sql
│   ├── SeedData.sql
├── logging
│   ├── logs
│   │   ├── logging.ini
│   │
├── search
│   ├── __init__.py
│   ├── models.py
│   ├── readenv.py
│   ├── routes.py
│   ├── schemas.py
│   ├── settings.py
│   └── views.py
│
├── search
│   ├── __init__.py
│   └── test_search.py
│
├── __init__.py
├── .env
├── .flake8
├── .gitattributes
├── .gitignore
├── .pre-commit-config.yaml
├── pyvenv.cfg
├── Dockerfile
├── sonar-project.properties
├── sonarcloud.properties
└── requirements.txt
```

- endpoints - holds all endpoints.
- app.py - Flask application initialization.
- settings.py - all global app settings.
- manage.py - script for managing application (migrations, server execution, etc.)

## Use Cases

There are many reasons to use the Saved Search API. The most common use case is to gather report information for a given search and save it, so that you can retrive saved and .

The history of a saved search contains the past and current instances (jobs) of the search.

When you create a saved search, at a minimum you need to provide a search query and a name for the search. You can also specify additional properties for the saved search at this time by providing a dictionary of key-value pairs for the properties. Or, modify properties after you have created the saved search.

## Running

1. Clone repository.
2. pip install requirements.txt
3. Run following commands:
   1. python manage.py db init
   2. python manage.py db migrate
   3. python manage.py db upgrade
4. Start server by running python manage.py runserver

## Responses

Many API endpoints return the JSON representation of the resources created or edited. However, if an invalid request is submitted, or some other error occurs, our API returns a JSON response in the following format:

```javascript
{
  "message" : string,
  "success" : bool,
  "data"    : string
}
```

The `message` attribute contains a message commonly used to indicate errors or, in the case of deleting a resource, success that the resource was properly deleted.

The `success` attribute describes if the transaction was successful or not.

The `data` attribute contains any other metadata associated with the response. This will be an escaped string containing JSON data.

## Status Codes

Gophish returns the following status codes in its API:

| Status Code | Description             |
| :---------- | :---------------------- |
| 200         | `OK`                    |
| 201         | `CREATED`               |
| 400         | `BAD REQUEST`           |
| 404         | `NOT FOUND`             |
| 500         | `INTERNAL SERVER ERROR` |

## Get Saved Searches

- `GET /api/search/` will return all saved_searches.

```json
{
  "id": 1,
  "name": "Smith Smith",
  "todos": []
}
```

**Status Codes**

- `200 OK` will be returned

By default an array of entry ids is returned in the correct order. The theory is that you will already have some or all of the entries and the ones that you don't yet have can be picked up using `GET /api/search/?appuserid=1&limit=10`.

## Get Saved Search

- `GET /api/search/?appuserid=1&limit=10`
  shall return only specific searches

RESPONSE

```json
{
  "id": 1,
  "name": "Smith Smith",
  "todos": []
}
```

| Parameter               | Required | Example                                                                                                         |
| ----------------------- | -------- | --------------------------------------------------------------------------------------------------------------- |
| `include_entries: bool` | false    | `GET /v2/saved_searches/1.json?include_entries=true` will return entry objects instead of an array of entry_ids |
| `page: number`          | false    | `GET /v2/saved_searches.json?page=2` will get page two of the search results                                    |

**Status Codes**

- `200 OK` will be returned if found
- `403 Forbidden` will be returned if the user does not own this saved search

## Create Saved Search

- `POST /api/search/?appuserid=4&searchtype=saved` will create a new saved search with the specified parameters

**Request**

```json
{
  "data": {
    "appuserid": 4,
    "createdby": 0,
    "name": "e60f7662c6174021ba6feee",
    "searchtype": "saved",
    "searchcriteria": {
      "reviewer": "G Smith",
      "patientid": "123456"
    },
    "updatedby": 0
  }
}
```

**Response**

```json
{
  "id": 1,
  "name": "John John",
  "todos": []
}
```

| Parameter       | Required |
| --------------- | -------- |
| `name: string`  | true     |
| `query: string` | true     |

**Status Codes**

- `201 Created` will be returned if creating the saved search is successful, the `Location` header will have the URL to the newly created saved search

## Delete Saved Search

`DELETE /api/search/14` will delete the saved search with and id of `14`

**Response**

```json
{
  "id": 3,
  "name": "Tom Tom",
  "todos": []
}
```

**Status Codes**

- `204 No Content` will be returned if the request was successful
- `403 Forbidden` will be returned if the user does not own this saved search

## Rename Saved Search

Updating a saved search can be used to set a custom title for a feed.

`PATCH /api/search/update/2` will update the saved search with and id of `2`

**Request**

```json
{
  "data": {
    "id": 2,
    "createdby": 0,
    "name": "SearchSep2133",
    "updatedby": 0,
    "createddate": "2021-09-18",
    "updateddate": "2021-09-18"
  }
}
```

**Response**

```json
{
  "data": {
    "id": 2,
    "createdby": 0,
    "name": "SearchSep2133",
    "updatedby": 0,
    "createddate": "2021-09-18",
    "updateddate": "2021-09-18"
  }
}
```

**Status Codes**

- `200 OK` will be returned if the request was successful
- `403 Forbidden` will be returned if the user does not own this saved search

**PATCH Alternative**

Some proxies block or filter PATCH requests. A POST alternative is available for these cases:

`POST /v2/saved_searches/1/update.json` will update the saved search with and id of `1`

### Swagger API Documentation

------------- TO DO ---------

### POST-Man API requests

------------------ TO DO --------
