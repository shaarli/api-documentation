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
