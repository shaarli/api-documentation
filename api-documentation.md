# Shaarli API Documentation

> Note: using [this markdown template](https://gist.github.com/iros/3426278).

## **List links**

Retrieve a list of links ordered by creation date.

* **URL**:  `?do=api[TODO]`
* **Method:** `GET`
*  **URL Params**

   **Optional:**
 
    * `offset=[numeric]`
      Offset from which to start listing links (default: 0).
    * `limit=[numeric]`
      Number of links to retrieve (default ?) or `all`.
      

* **Body Params:** none

* **Success Response:**
* **Content:** 
    ```json
    [
      { 
        "todo": "link obj"
      }
    ]
    ```
 
* **Error Response:** todo

* **Sample Call:**

```javascript
$.ajax({
  url: "?do=api[??]]&offset=60&limit=20",
  type : "GET",
  success : function(r) {
    console.log(r);
  }
});
```
