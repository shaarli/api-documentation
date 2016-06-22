# Shaarli API Documentation

> Note: using [this markdown template](https://gist.github.com/iros/3426278).

## **GET links**

Retrieve a list of links ordered by creation date.

* **URL**: Endpoint discussion in #3.
* **Method:** `GET`
* **Path Parameters:** none
* **URL Parameters**

   **Optional:**
 
    * `offset=[numeric]`
      Offset from which to start listing links (default: 0).
    * `limit=[numeric]`
      Number of links to retrieve (default 20) or `all`.
    * `searchterm=[string]`
      Search terms across all links fields.
    * `searchtags=[string]`
      Search for specifics tags. Tag list should be separated by a `+` delimitor.
* **Body Parameters:** none

* **Success Response:** `200`
  **Content:** 
    ```json
    [
      { 
        "id": "linkID",
        "url": "Shaared URL",
        "title": "link title",
        "description": "link description",
        "tags": [
          "shaarli",
          "php",
          "api"
        ],
        "private": true,
        "created": "2016-06-15T19:23:14+0200",
        "updated": "not implemented yet"
      },
      {
        <...>
      }
    ]
    ```
  **Notes**: 
    * Dates use ISO8601 format.
 
* **Error Response:**
  - `400`: Invalid parameters  
    Content:
    ```json
    { "message": "Offset should be an integer." }
    ```
  - `401`: Invalid token/authentication.

* **Sample Call:**

```javascript
$.ajax({
  url: "[See #3]&offset=60&limit=30",
  type : "GET",
  success : function(r) {
    console.log(r);
  }
});
```

## **GET link**

Retrieve a single link by its ID.

* **URL**: Endpoint discussion in #7.
* **Method:** `GET`
* **Path Parameters:**
   
    **Required:**

      * `[string]`
        Link ID.

* **URL Parameters:** none
* **Body Parameters:** none

* **Success Response:** `200`
  **Content:** 
    ```json
    [
      { 
        "id": "linkID",
        "url": "Shaared URL",
        "title": "link title",
        "description": "link description",
        "tags": [
          "shaarli",
          "php",
          "api"
        ],
        "private": true,
        "created": "2016-06-15T19:23:14+0200",
        "updated": "not implemented yet"
      }
    ]
    ```
  **Notes**: 
    * Dates use ISO8601 format.
 
* **Error Response:**
  - `401`: Invalid token/authentication.
  - `404`: Link not found
    Content:
    ```json
    { "message": "Link ID 'foobar' does not exist." }
    ```
* **Sample Call:**

```javascript
$.ajax({
  url: "[See #7]/20160615_180000",
  type : "GET",
  success : function(r) {
    console.log(r);
  }
});
```

## **PUT link**

Edit an existing link by its ID.

* **URL**: Endpoint discussion in #7.
* **Method:** `PUT`
* **Path Parameters:** 

* **URL Parameters:** none
* **Body Parameters:**

  **Required:**
    * **url** (string URL format): Shaared URL.
    * **title** (string): Link title.  

  **Optional:**
    * **description** (string): Text describing the link.
    * **tags** (array): list of tags associated to the link.
    * **private** (boolean): set the link as private (default depending on user configuration).

  **Example:** 
    ```json
    { 
      "url": "http://ShaaredURL.com",
      "title": "link title",
      "description": "link description",
      "tags": [
        "shaarli",
        "php",
        "api"
      ],
      "private": true
    }
    ```

* **Success Response:** `200`

  **Content:** edited link
    ```json
    { 
      "id": "linkID",
      "url": "http://ShaaredURL.com",
      "title": "link title",
      "description": "link description",
      "tags": [
        "shaarli",
        "php",
        "api"
      ],
      "private": true,
      "created": "2016-06-15T19:23:14+0200",
      "updated": "not implemented yet"
    }
    ```
 
* **Error Response:**
  - `400`: Invalid parameters

    **Content:**
    ```json
    { "message": "URL is required." }
    ```
  - `401`: Invalid token/authentication.
  - `404`: Link ID not found.
    Content:
    ```json
    { "message": "Link ID 'foobar' does not exist." }
    ```

* **Sample Call:**

```javascript
var formData = { 
      "url": "http://ShaaredURL.com",
      "title": "link title",
      "description": "link description",
      "tags": [
        "shaarli",
        "php",
        "api"
      ],
      "private": true,
    };
$.ajax({
  url: "[See #7]",
  type : "PUT",
  data : formData,
  success : function(r) {
    console.log(r);
  }
});
```

## **POST link**

Create a new link.

* **URL**: Endpoint discussion in #7.
* **Method:** `POST`
* **Path Parameters:** none
* **URL Parameters:** none
* **Body Parameters:**

  **Required:**
    * **url** (string URL format): Shaared URL.
    * **title** (string): Link title.  

  **Optional:**
    * **description** (string): Text describing the link.
    * **tags** (array): list of tags associated to the link.
    * **private** (boolean): set the link as private (default depending on user configuration).

  **Example:** 
    ```json
    { 
      "url": "http://ShaaredURL.com",
      "title": "link title",
      "description": "link description",
      "tags": [
        "shaarli",
        "php",
        "api"
      ],
      "private": true
    }
    ```

* **Success Response:** `200`  
  **Content:** created link
    ```json
    { 
      "id": "linkID",
      "url": "http://ShaaredURL.com",
      "title": "link title",
      "description": "link description",
      "tags": [
        "shaarli",
        "php",
        "api"
      ],
      "private": true,
      "created": "2016-06-15T19:23:14+0200",
      "updated": "not implemented yet"
    }
    ```
 
* **Error Response:**
  - `400`: Invalid parameters

    **Content:**
    ```json
    { "message": "URL is required." }
    ```
  - `401`: Invalid token/authentication.
  - `409`: Conflict. A link with this URL already exists.

    **Content:** existing link  
    ```json
    { 
      "id": "linkID",
      "url": "http://ShaaredURL.com",
      "title": "link title",
      "description": "link description",
      "tags": [
        "shaarli",
        "php",
        "api"
      ],
      "private": true,
      "created": "2016-06-15T19:23:14+0200",
      "updated": "not implemented yet"
    }
    ```
* **Sample Call:**

```javascript
var formData = { 
      "id": "linkID",
      "url": "http://ShaaredURL.com",
      "title": "link title",
      "description": "link description",
      "tags": [
        "shaarli",
        "php",
        "api"
      ],
      "private": true,
      "created": "2016-06-15T19:23:14+0200",
      "updated": "not implemented yet"
    };
$.ajax({
  url: "[See #7]",
  type : "POST",
  data : formData,
  success : function(r) {
    console.log(r);
  }
});
```

## **GET log**

Retrieve user actions history.

* **URL**: Endpoint discussion in #7.
* **Method:** `GET`
* **Path Parameters:** none
* **URL Parameters**

   **Optional:**
 
    * `offset=[numeric]`
      Offset from which to start listing the log (default: 0).
    * `limit=[numeric]`
      Number of log items to retrieve (default 20) or `all`.
      
* **Body Parameters:** none
* **Success Response:** `200`
  **Content:** 
    ```json
    [
      {
        "action": "SETTINGS_UPDATED",
        "datetime": "2016-06-15T19:23:14+0200"
      },
      { 
        "action": "CREATED",
        "link_id": "linkid",
        "datetime": "2016-06-15T19:23:14+0200"
      },
      {
        "action": "UPDATED",
        "link_id": "linkid",
        "datetime": "2016-06-15T19:23:14+0200"
      },
      {
        "action": "DELETED",
        "link_id": "linkid",
        "datetime": "2016-06-15T19:23:14+0200"
      }      
    ]
    ```
  **Notes**: 
    * Dates use ISO8601 format.
 
* **Error Response:**
  - `400`: Invalid parameters

    **Content:**
    ```json
    { "message": "Offset should be an integer." }
    ```
  - `401`: Invalid token/authentication.

* **Sample Call:**

```javascript
$.ajax({
  url: "[See #7]",
  type : "GET",
  success : function(r) {
    console.log(r);
  }
});
```
