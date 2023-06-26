# Shaarli API Documentation

[Shaarli](https://github.com/shaarli/Shaarli)'s REST API.

This documentation is written according to [API Blueprint specification](https://github.com/apiaryio/api-blueprint).

 * HTML version: https://shaarli.github.io/api-documentation/
 * Git repository: https://github.com/shaarli/api-documentation

All endpoints described in this documentation must be prefixed with `/api/v1/`.  
Example: `GET http://shaarli.domain.tld/api/v1/info`

> Note: all requests reaching described services must include a valid JWT token in the HTTP header. The signature must use SHA512 encryption algorithm.
> 
> The header must have use the following syntax:
> 
>     Authorization: Bearer <token>
> 
> Resources:
>   * [jwt.io](https://jwt.io) and associated libraries
>   * [Python Client](https://github.com/shaarli/python-shaarli-client)
>   * [PHP token generation example](https://gist.github.com/ArthurHoaro/0d1bdbbaeb42c52470c745f8ac6e44f1)

# Group Links

Links

## Links Collection [/links{?offset,limit,searchterm,searchtags,visibility}]

### List All Links [GET]

Retrieve a list of links ordered by creation date, eventually filtered with parameters.

An empty array will be returned if no link is found with the filters provided. 

+ Parameters
    * offset: 40 (number, optional) -  Offset from which to start listing links (default: 0)
    * limit: 25 (number, optional) - Number of links to retrieve (default 20) or `all`.  
    Note: using `all` can be very resource consuming with a big database, use it carefully.
    * searchterm: shaarli+api (string, optional) - Search terms across all links fields (url encoded string)
    * searchtags: rest+http (string, optional) - Search for specifics tags. Tag list should be separated by a `+` delimitor. Set it to `false` to retrieve only untagged links.
    * visibility: `private` (string, optional) - Filter links by visibility. 
    Allowed values: `all`, `private`, `public` (default: `all`).

+ Response 200
  
    + Attributes (array[Link])

+ Response 400

    + Attributes (Error400)

+ Response 401

    + Attributes (Error401)
 
### Create a New Link [POST]

Create a new Link with provided request data.

Note that the shaared URL must not exists. If none is provided, a note will be created (self linked shaare).

+ Request (application/json)

    + Attributes (LinkRequest)

+ Response 201

  The new link has been created, sending a redirection to the new link in `Location` header.

  + Headers

            Location: /links/345

  + Attributes (Link)

+ Response 400

    + Attributes (Error400)
    
+ Response 401

    + Attributes (Error401)

+ Response 409

    Conflict: another link with the same URL has been found. The existing link is  returned in the body of the response.

    + Attributes (Link)

## Link [/links/{linkID}]

+ Parameters
    + linkID: `349` (number, required) - Link identifier

### View Link details [GET]

+ Response 200

      + Attributes (Link)
    
+ Response 401

    + Attributes (Error401)

+ Response 404

    + Attributes (Error404)

### Update a link [PUT]

Update an existing link with provided request data. Keep in mind that all link's fields will be updated.

+ Request (application/json)

    + Attributes (LinkRequest)

+ Response 200

  The existing link has been updated.

  + Attributes (Link)

+ Response 400

    + Attributes (Error400)
    
+ Response 401

    + Attributes (Error401)

+ Response 404

    + Attributes (Error404)

+ Response 409

  Conflict. Given URL already exists for another link, which is returned in the response body.

  + Attributes (Link)

### Delete a link [DELETE]

Delete a link.

+ Response 204

  The existing link has been deleted.

   + Body
 
+ Response 401

    + Attributes (Error401)

+ Response 404

    + Attributes (Error404)


## Tags Collection [/tags{?offset,limit,visibility}]

### List All Tags [GET]

Retrieve a list of tags ordered by usage. Provided and returned tag names are case insensitive,
meaning that the occurrences count for `Tag` and `tag` are added.

An empty array will be returned if no tag is found with the filters provided. 

+ Parameters
    * offset: 40 (number, optional) -  Offset from which to start listing tags (default: 0)
    * limit: 25 (number, optional) - Number of tags to retrieve (default `all`).
    * visibility: `private` (string, optional) - Filter links by visibility. 
    Allowed values: `all`, `private`, `public` (default: `all`).

+ Response 200
  
    + Attributes (array[Tag])

+ Response 400

    + Attributes (Error400)

+ Response 401

    + Attributes (Error401)

## Tag [/tags/{tagName}]

+ Parameters
    + tagName: `tutorial` (string, required) - Tag name

### Get a tag [GET]

Retrieve a single tag details. Provided and returned tag names are case insensitive,
meaning that the occurrences count for `Tag` and `tag` are added.

+ Response 200

      + Attributes (Tag)
    
+ Response 401

    + Attributes (Error401)

+ Response 404

    + Attributes (Error404)

### Update a tag [PUT]

Rename a tag from the given name. If the new name you provide matches an existing tag, they will be merged.

Provided tag name is case sensitive.

+ Request (application/json)

    + Attributes (TagRequest)

+ Response 200

  The existing tag has been updated.

  + Attributes (Tag)

+ Response 400

    + Attributes (Error400)
    
+ Response 401

    + Attributes (Error401)

+ Response 404

    + Attributes (Error404)

### Delete a tag [DELETE]

Delete a tag from every link where it is used.

Provided tag name is case sensitive.

+ Response 204

  The existing tag has been deleted.

   + Body
 
+ Response 401

    + Attributes (Error401)

+ Response 404

    + Attributes (Error404)

## History [/history{?since,offset,limit}]

### Get last user actions [GET]

Retrieve the last actions made by the user, even in the web version, including:

  - `CREATED`: A new link has been created.
  - `UPDATED`: An existing link has been updated.
  - `DELETED`: A link has been deleted.
  - `SETTINGS`: Shaarli settings have been updated.

> This service can be useful to maintain a local database.

+ Parameters

    + since: `2015-05-05T12:30:00%2B03:00` (string, optional) - Get all event since this datetime (format ISO ISO8601). *Note: the `+` should be encoded to `%2B`*.
    + offset: 40 (number, optional) -  Offset from which to start listing links (default: 0).
    + limit: 25 (number, optional) - Number of links to retrieve (default 20) or `all`.

+ Response 200

    + Attributes (array[History])

+ Response 400

    + Attributes (Error400)
    
+ Response 401

    + Attributes (Error401)

## Instance information [/info]

### Get instance info [GET]

Return a list of information and settings for the Shaarli instance.

+ Response 401

    + Attributes (Error401)

+ Response 200

    + Attribute (Info)

## Data Structures

### Link
+ id: 345 (number)             - Link identifier
+ url: http://foo.bar (string) - Link URL
+ shorturl: 1H3Srg (string)    - Link permalink short URL
+ title: Link title (string)   - Link  title
+ description (string)         - Link description
+ tags: foo, bar (array[string]) - List of tags associated with the link
+ private: false (boolean)     - This flag is set to true when a link is private
+ created: `2015-05-05T12:30:00+03:00` (string) - Creation datetime in ISO8601 format
+ updated: `2015-05-06T14:30:00+03:00` (string) - Link last update date in ISO8601 format

### LinkRequest
+ url: http://foo.bar (string, optional)   - Link URL
+ title: Link title (string, optional)     - Link  title
+ description (string, optional)           - Link description
+ tags: foo, bar (array[string], optional) - List of tags associated with the link
+ private: false (boolean, optional)       - This flag is set to true when a link is private
+ created: `2015-05-05T12:30:00+03:00` (string) - Creation datetime in ISO8601 format
+ updated: `2015-05-06T14:30:00+03:00` (string) - Link last update date in ISO8601 format

### History
+ event: CREATED (string, required) - An event code matching a user action.
+ datetime: `2015-05-05T12:30:00+03:00` (string, required) - Event datetime in ISO8601 format
+ id: `123` (number, optional) - Identifier of the logged event (e.g.: created link ID). 

### Info
+ global_counter: 654 (number, required) - Number of links stored in this Shaarli instance (private and public)
+ private_counter: 123 (number, required) - Number of private links stored
+ settings (Settings)

### Settings
+ title: My links (string) - Shaarli's instance title.
+ header_link: https://foo.bar/shaarli (string) - Link to the homepage.
+ timezone: Europe/Paris (string) - Shaarli's instance timezone.
+ enabled_plugins: qrcode, markdown (array[string]) - List of enabled plugins.
+ default_private_links: true (boolean) - Check the private checkbox by default for every new link.
+ tags_separator: " " (string) - String delimitator for tags, as configured on the Shaarli instance.

### Tag
+ name: Tutorial (string, required) - Name of the tag
+ occurences: 47 (number, optional) - Number of links using this tag

### TagRequest
+ name: music (string, required) - Name of the tag

### Error400
+ code: 400 (number)
+ message: Invalid parameters (string)

### Error401
+ code: 401 (number)
+ message: Not authorized (string)

### Error404
+ code: 404 (number)
+ message: Not found (string)
