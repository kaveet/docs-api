---
title: "Content API"
date: "2019-01-09"
meta_title: "Ghost Content API Documentation"
meta_description: "Ghost's RESTful Content API delivers published content to the world and can be accessed by any client to render a website. Read more on Ghost Docs."
keywords:
    - "headless cms"
    - "javascript"
    - "ghost api"
---

Ghost's RESTful Content API delivers published content to the world and can be accessed in a read-only manner by any client to render in a website, app or other embedded media.

Access control is managed via an API key, and even the most complex filters are made simple with our SDK. The Content API is designed to be fully cachable, meaning you can fetch data as often as you like without limitation.


---


## API Clients

### JavaScript Client Library

We've developed an [API client for JavaScript](https://docs.ghost.org/api/javascript/), that will allow you to quickly and easily interact with the Content API.
The client is an advanced wrapper on top of our REST API - everything that can be done with the Content API can be done using the client, with no need to deal with the details of authentication or the request & response format.

### Handlebars

You can upgrade your Ghost theme to use the v2 Content API by specifying the `ghost-api` version in the `engines` field of your package.json. See the [handlebars reference](/api/handlebars-themes/packagejson/) for an example.


---


## Authentication

### Url

`https://{admin_domain}`

Your admin domain can be different to your main domain.
Using the correct domain and protocol are critical to getting consistent behaviour, particularly when dealing with CORS in the browser.
All Ghost(Pro) blogs have a `*.ghost.io domain` as their admin domain and require https.

### Path & Version

`/ghost/api/{version}/content/`

Each API is prefixed with the same path, followed by a specific version. Version strings are required and always start with `v`. The [api versioning](https://docs.ghost.org/faq/api-versioning/) guide explains the current available versions and stability index.

### Key

`?key={key}`

Content API keys are provided via a query parameter in the url. These keys are safe for use in browsers and other insecure environments, as they only ever provide access to public data. Sites in private mode should consider where they share any keys they create.

The Content API URL and key can be obtained by creating a new `Custom Integration` under the **Integrations** screen in Ghost Admin.

![Get a Ghost Content API key](/images/apikey.png)


### Working Example

```bash:title=cURL example
# Real endpoint - copy and paste to see!
curl "https://demo.ghost.io/ghost/api/v2/content/posts/?key=22444f78447824223cefc48062"
```

---

## Endpoints

The Content API provides access to Posts, Pages, Tags, Authors and Settings. All endpoints return JSON.

<table class="table">
<tbody>
<tr>
  <th>Verb</th>
  <th>Path</th>
  <th>Method</th>
</tr>
<tr>
  <td>GET</td>
  <td width="300"><a href="#posts">/posts/</a></td>
  <td>Browse posts</td>
</tr>
<tr>
  <td>GET</td>
  <td width="300"><a href="#posts">/posts/{id}/</td>
  <td>Read a post by ID</td>
</tr>
<tr>
  <td>GET</td>
  <td width="300"><a href="#posts">/posts/slug/{slug}/</td>
  <td>Read a post by slug</td>
</tr>
<tr>
  <td>GET</td>
  <td width="300"><a href="#authors">/authors/</td>
  <td>Browse authors</td>
</tr>
<tr>
  <td>GET</td>
  <td width="300"><a href="#authors">/authors/{id}/</td>
  <td>Read an author by ID</td>
</tr>
<tr>
  <td>GET</td>
  <td width="300"><a href="#authors">/authors/slug/{slug}/</td>
  <td>Read a author by slug</td>
</tr>
<tr>
  <td>GET</td>
  <td width="300"><a href="#tags">/tags/</td>
  <td>Browse tags</td>
</tr>
<tr>
  <td>GET</td>
  <td width="300"><a href="#tags">/tags/{id}/</td>
  <td>Read a tag by ID</td>
</tr>
<tr>
  <td>GET</td>
  <td width="300"><a href="#tags">/tags/slug/{slug}/</td>
  <td>Read a tag by slug</td>
</tr>
<tr>
  <td>GET</td>
  <td width="300"><a href="#pages">/pages/</td>
  <td>Browse pages</td>
</tr>
<tr>
  <td>GET</td>
  <td width="300"><a href="#pages">/pages/{id}/</td>
  <td>Read a page by ID</td>
</tr>
<tr>
  <td>GET</td>
  <td width="300"><a href="#pages">/pages/slug/{slug}/</td>
  <td>Read a page by slug</td>
</tr>
<tr>
  <td>GET</td>
  <td width="300"><a href="#settings">/settings/</td>
  <td>Browse settings</td>
</tr>
</tbody>
</table>

The Content API supports two types of request: Browse and Read.
Browse endpoints allow you to fetch lists of resources, whereas Read endpoints allow you to fetch a single resource.


---


## Resources

The API will always return valid JSON in the same structure:

```json
{
    "resource_type": [{
        ...
    }],
    "meta": {}
}
```

- `resource_type`: will always match the resource name in the URL. All resources are returned wrapped in an array, with the exception of `/site/` and `/settings/`.
- `meta`: contains [pagination](/api/content/#pagination) information for browse requests.


### Posts

Posts are the [primary resource](/concepts/posts/) in a Ghost site. Using the posts endpoint it is possible to get lists of posts filtered by various criteria.

```JavaScript
GET /content/posts/
GET /content/posts/{id}/
GET /content/posts/slug/{slug}/
```

By default, posts are returned in reverse chronological order by published date when fetching more than one.

The most common gotcha when fetching posts from the Content API is not using the [include](#include) parameter to request related data such as tags and authors.
By default, the response for a post will not include these:

`embed:api/v2/content/demo/posts.json`

Posts allow you to include `authors` and `tags` using `?include=authors,tags`, which will add an `authors` and `tags` array to the response, as well as both a `primary_author` and `primary_tag` object.

#### Working Example

```bash:title=cURL example
# Real endpoint - copy and paste to see!
curl "https://demo.ghost.io/ghost/api/v2/content/posts/?key=22444f78447824223cefc48062&include=tags,authors"
```

Returns:

`embed:api/v2/content/demo/posts-with-tags-authors.json`

### Pages

Pages are static resources that are not included in channels or collections on the Ghost front-end.
The API will only return pages that were created as [resources](/concepts/pages/) and will not contain routes created with [dynamic routing](/api/handlebars-themes/routing/).


```JavaScript
GET /content/pages/
GET /content/pages/{id}/
GET /content/pages/slug/{slug}/
```


Pages are structured identically to posts. The response object will look the same, only the resource key will be `pages`.
By default, pages are ordered by title when fetching more than one.

### Tags

Tags are the [primary taxonomy](/concepts/tags/) within a Ghost site.

```JavaScript
GET /content/tags/
GET /content/tags/{id}/
GET /content/tags/slug/{slug}/
```

By default, internal tags are always included, use `filter=visibility:"public"` to limit the response directly or use the [tags helper](/api/helpers/#tags) to handle filtering and outputting the response.

Tags that are not associated with a post are not returned. You can supply `include=count.posts` to retrieve the number of posts associated with a tag.

`embed:api/v2/content/demo/tags.json`

By default, tags are ordered by name when fetching more than one.

### Authors

Authors are a subset of [users](/concepts/users/) who have published posts asscoiated with them.

```JavaScript
GET /content/authors/
GET /content/authors/{id}/
GET /content/authors/slug/{slug}/
```

Authors that are not associated with a post are not returned. You can supply `include=count.posts` to retrieve the number of posts associated with an author.

`embed:api/v2/content/demo/authors.json`

### Settings

Settings contains the global settings for a site.

```JavaScript
GET /content/settings/
```

The settings endpoint is a special case. You will receive a single object, rather than an array. This endpoint doesn't accept any query parameters.

`embed:api/v2/content/demo/settings.json`

---


## Parameters

Query parameters provide fine-grained control over responses. All endpoints accept `include` and `fields`. Browse endpoints additionally accept `filter`, `limit`, `page` and `order`.

The values provided as query parameters MUST be url encoded when used directly. The [client libraries](/api/javascript/) will handle this for you.

### Include

Tells the API to return additional data related to the resource you have requested. The following includes are available:

- Posts & Pages: `authors`, `tags`
- Authors: `count.posts`
- Tags: `count.posts`

Includes can be combined with a comma, e.g. `&include=authors,tags`.

For posts and pages:

- `&include=authors` will add `"authors": [{...},]` and `"primary_author": {...}`
- `&include=tags` will add `"tags": [{...},]` and `"primary_tag": {...}`

For authors and tags:

- `&include=count.posts` will add `"count": {"posts": 7}` to the response.

### Fields

Limit the fields returned in the response object. Useful for optimising queries, but does not play well with include.

E.g. for posts `&fields=title,url` would return:

```json
{"posts": [{
    "id": "5b7ada404f87d200b5b1f9c8",
    "title": "Welcome to Ghost",
    "url": "https://demo.ghost.io/welcome/"
}]}
```

### Formats

(Posts and Pages only)

By default, only `html` is returned, however each post and page in Ghost has 2 available formats: `html` and `plaintext`.

- `&formats=html,plaintext` will additionally return the plaintext format.


### Filter

(Browse requests only)

Apply fine-grained filters to target specific data.

- `&filter=featured:true` on posts, would return only those marked featured.
- `&filter=tag:getting-started` on posts, would return those with the tag slug that matches `getting-started`.

The possibilities are extensive! Query strings are explained in detail in the [filtering](#filtering) section.

### Limit

(Browse requests only)

By default, only 15 records are returned at once.

- `&limit=5` would return only 5 records.
- `&limit=all` will return all records - use carefully!

### Page

(Browse requests only)

By default, the first 15 records are returned.

- `&page=2` will return the second set of 15 records.

### Order

(Browse requests only)

Different resources have a different default sort order:

- Posts: `published_at DESC` (newest post first)
- Pages: `title ASC` (alphabetically by title)
- Tags: `name ASC` (alphabetically by name)
- Authors: `name ASC` (alphabetically by name)

The syntax for modifying this follows SQL order by syntax:

- `&order=published_at%20asc` would return posts with the newest post last


---


## Filtering

Ghost uses a query language called NQL to allow filtering API results. You can filter any field or included field using matches, greater/less than or negation, as well as combining with and/or. NQL doesn't yet support 'like' or partial matches.

Filter strings must be URL encoded. The [{{get}}](/api/handlebars-themes/helpers/get/) helper and [client libraries](/api/javascript) handle this for you.

At it's most simple, filtering works the same as in GMail, GitHub or Slack - you provide a field and a value, separated by a colon.

### Syntax Reference

#### Filter Expressions

A **filter expression** is a string which provides the **property**, **operator** and **value** in the form <strong>property:<em>operator</em>value</strong>:
- **property** - a path representing the field to filter on
- **:** - separator between **property** and an **operator**-**value** expression
- **operator** (optional) - how to compare values (`:` on its own is roughly `=`)
- **value** - the value to match against

#### Property

Matches: `[a-zA-Z_][a-zA-Z0-9_.]`
-   can contain only alpha-numeric characters and `_`
-   cannot contain whitespace
-   must start with a letter
-   supports `.` separated paths, E.g. `authors.slug` or `posts.count`
-   is always lowercase, but accepts and converts uppercase

#### Value

Can be one of the following
-   **null**
-   **true**
-   **false**
-   a **_number_** (integer)
-   a **literal**
    -   Any character string which follows these rules:
    -   Cannot start with `-` but may contain it
    -   Cannot contain any of these symbols: `'"+,()><=[]` unless they are escaped
    -   Cannot contain whitespace
-  a **string**
    -  `'` string here `'` Any character except a single or double quote surrounded by single quotes
    -   Single or Double quote *_MUST *_be escaped*
    -   Can contain whitespace
    -   A string can contain a date any format that can be understood by `new Date()`

#### Operators

* `-` - not
* `>` - greater than
* `>=` - greater than or equals
* `<` - less than
* `<=` - less than or equals
* `[` value, value, ... `]` - "in" group, can be negated with `-`

#### Combinations

* `+` - represents and
* `,` - represents or
* `(` filter expression `)` - overrides operator precedence

#### Strings vs Literals

Most of the time, there's no need to put quotes around strings when building filters in Ghost.
If you filter based on slugs, slugs are always compatible with literals.
However, in some cases you may need to use a string that contains one of the other characters used in the filter syntax, e.g. dates & times contain`:`. Use single-quotes for these.


---


## Pagination

All browse endpoints are paginated, returning 15 records by default. You can use the [page](#page) and [limit](#limit) parameters to move through the pages of records. The response object contains a `meta.pagination` key with information on the current location within the records:

```json
"meta":{
    "pagination":{
      "page":1,
      "limit":2,
      "pages":1,
      "total":1,
      "next":null,
      "prev":null
    }
  }
```


---


## Errors

The Content API will generate errors for the following cases:

- Status 400: Badly formed queries e.g. filter parameters that are not correctly encoded
- Status 401: Authentication failures e.g. unrecognised keys
- Status 404: Unknown resources e.g. data which is not public
- Status 500: Server errors e.g. where something has gone

Errors are also formatted in JSON, as an array of error objects.
The HTTP status code of the response along with the `errorType` property indicate the type of error.

The `message` field is designed to provide clarity on what exactly has gone wrong.

```json
{"errors": [{
    "message": "Unknown Content API Key",
    "errorType": "UnauthorizedError"
}]}
```

---


## Versioning

The v2 Content API is **stable** as of **Ghost 2.10.0**. See the [stability index](https://docs.ghost.org/faq/api-versioning/) for full details of the API versions.
You can disable the v0.1 Public API in the labs section of your admin panel.
