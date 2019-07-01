---
title: "Migrating to Ghost"
date: "2018-10-01"
meta_title: "How to Migrate to Ghost from other platforms - Docs"
meta_description: "A detailed guide for migrating to the Ghost publishing platform from WordPress, Tumblr, Medium, Blogger, and other CMS."
keywords:
    - import
    - export
    - migration
    - wordpress
    - tumblr
    - medium
    - drupal
    - blogger
    - migrating
---

Migrations between platforms are difficult, so we've compiled a list of resources about migrating to Ghost

Ghost imports from existing blogs via a custom JSON data format, described below. The import and export tools can be found on the 'labs' page in Ghost settings. The importer will accept either a JSON file, or a zip file containing a JSON file and the related images.



## Existing Plugins

### Official

**WordPress** users can try the [Ghost WordPress Plugin](http://wordpress.org/plugins/ghost/) to export their data - however this is currently limited to Ghost 1.0. TLDR: You'll need to export content from WordPress, import it into a Ghost 1.0 site, then upgrade that Ghost 1.0 site to the latest version of Ghost. Not ideal! We're working on it.

### Non-official
* **WordPress** users can also try the [wp2ghost](https://github.com/jonhoo/wp2ghost) command-line tool to convert their WordPress export files to Ghost-compatible JSON.
* **Jekyll** users can try the [Jekyll to Ghost Plugin](https://github.com/mekomlusa/Jekyll-to-Ghost)
* **Blogger** users can try [Blogger2Ghost.NET](https://github.com/jessehouwing/Blogger2Ghost)
* **Open-source geeks** may consider importing READMEs from all of their Github repositories as regular posts using [gh2ost](https://github.com/RReverser/gh2ost) tool.
* **Tumblr** users can try [Tumblr to Ghost](https://github.com/jpadilla/tumblr-to-ghost)
* **Brave people** can try [Medium to Ghost](https://github.com/ageitgey/medium_to_ghost)

### Importing the JSON

Once you've generated the JSON go to the Labs Menu (`/ghost/settings/labs/`) on your blog to access the import form.

### Importing Images

Ghost can import images within the provided zip. The images need to be located in a folder called `content/images`.


---


## Rolling your own

If no export tools exist for your current blogging system you'll need to create one that generates a JSON file as described here. There is a full example at the end of this file. Please note that your final JSON file should have no comments in the final format. Those are only included here for readability and explanatory purposes.

### JSON file structure

First and foremost, your JSON file must contain valid JSON. You can test your file is valid using the [JSONLint](http://jsonlint.com/) online tool.

The file structure can optionally be wrapped in:

```json
{
  "db": [...contents here...]
}
```

Both with and without are valid Ghost JSON files. But you must include a `meta` and a `data` object.

### The meta object

```json
"meta": {
    "exported_on":1408552443891,
    "version":"2.14.0"
}
```

The `meta` block expects two keys, `exported_on` and `version`. `exported_on` should be an epoch timestamp in milliseconds, version should be the Ghost version the import is valid for.

### The data block

Ghost's JSON format mirrors the underlying database structure, rather than the API, as it allows you to override fields that the API would ignore.

```json
"data": {
  "posts": [{...}, ...],
  "tags": [],
  "users": [],
  "posts_tags": [],
  "posts_authors": [],
  "roles_users": []
}
```

The data block contains all of the individual post, tag, and user resources that you want to import into your site, as well as the relationships between all of these resources. Each item that you include should be an array of objects.

Relationships can be defined between posts and tags, posts and users (authors) and between users and their roles.

IDs inside the file are relative to the file only, so if you have a `post` with `id: 1` and a `posts_tags` object which references `post_id: 1`, then those two things will be linked, but they do not relate to the `post` with `id: 1` in your database.


### Posts

The only strictly required field when importing posts is the title. Ghost will automatically generate slugs and set every other field to the default or empty.

To import a valid post with content, published at a specific time, the bare minimum fields to provide are title, mobiledoc, status and published:

```json
{
    "title": "my blog post title",
    "mobiledoc": "{\"version\":\"0.3.1\",\"atoms\":[],\"cards\":[],\"markups\":[],\"sections\":[[1,\"p\",[[0,[],0,\"You're live, nice!\"]]]]}",
    "status": "published",
    "published_at":  1283780649000
}
```

The `status` field defaults to `draft`. Set it to `published` and set `published_at` to a past millisecond timestamp in order to import an already published post with the correct date. You can also set `status` to `scheduled` and set `published_at` to a future date to create a post that will be scheduled in future.

Mobiledoc is the data format used internally by Ghost to represent your content. See the section on [mobiledoc](#mobiledoc) below for more details on converting content.


### Users

Your import file must include both any users that you want to import and any users that you need to reference in the import file in relation to other resources.

Users with an email address that matches the email address of a existing user will not be duplicated or updated. Users with an unmatched email address will be imported, but their account will be locked so that they must reset their password to login.

#### Linking objects to users

To link your resources to a user, you would specify a user id relative to the user's id in the import file. So, if you have a user with `id: 2` in your file, and a post with `published_by: 2` that post will be set to be published by that user.

All users with unmatched email addresses are imported first. Once this is complete, the importer uses the relative ID from the import file to look up which user to link, and then uses the email address to find the correct user in the database. Therefore if you want to link objects to a user that is already in the database you can specify the bare minimum information for that user so that it can be mapped correctly:

```json
"users": [{
   id: 35,
   name: "A User",
   email: "user@example.com"
}]
```

If the importer is unable to find a reference for the user inside the import file, it will default to the Owner.

#### User Roles

All users are given the role of `author` by default. If you want to specify different roles, you can do so by providing a `roles_users` object, much like the `posts_tags` object. Please note that Ghost doesn't yet support importing roles, so in this case the `role_id` is always relative to the `id` in the database, rather than in the file.

## Example

```json
{
    "meta":{
        // epoch time in milliseconds
        "exported_on":  1388805572000,
        "version":      "2.14.0"
    },
    "data":{
        "posts": [
            {
                "id": 1,
                "title":         "my blog post title",
                "slug":          "my-blog-post-title", // Optional, will be generated by Ghost if missing
                // mobiledoc is used to represent your content
                "mobiledoc":     "{\"version\":\"0.3.1\",\"atoms\":[],\"cards\":[],\"markups\":[],\"sections\":[[1,\"p\",[[0,[],0,\"You're live, nice!\"]]]]}",
                "feature_image": null,
                "featured":      0, // boolean indicating featured status
                "page":          0, // boolean indicating if this is a page or post
                "status":        "published", // or draft
                "published_at":  1283780649000, // epoch time in milliseconds
                "published_by":  1, // the first user created has an id of 1
                "meta_title":    null,
                "meta_description":null,
                "author_id":     1, // the first user created has an id of 1
                "created_at":    1283780649000, // epoch time in milliseconds
                "created_by":    1, // the first user created has an id of 1
                "updated_at":    1286958624000, // epoch time in milliseconds
                "updated_by":    1 // the first user created has an id of 1
            }
        ],
        "tags": [
            {
                "id":           5,
                "name":         "Colorado Ho!",
                "slug":         "colorado-ho", // Optional, will be generated by Ghost if missing
                "description":  ""
            }
        ],
        "posts_tags": [
            {"tag_id": 5, "post_id": 1},
        ],
        "users": [
            {
                "id":           3,
                "name":         "Jo Bloggs",
                "slug":         "jo-blogs", // Optional, will be generated by Ghost if missing
                "email":        "jo@example.com",
                "profile_image": null,
                "cover_image":   null,
                "bio":           null,
                "website":       null,
                "location":      null,
                "accessibility": null,
                "meta_title":    null,
                "meta_description": null,
                "created_at":    1283780649000, // epoch time in millis
                "created_by":    1,
                "updated_at":    1286958624000, // epoch time in millis
                "updated_by":    1
            }
        ],
        // Provide this if you want different roles to the default "Author" role
        "roles_users": [
            {
                "user_id": 2,
                "role_id": ObjectId // This must reference the id from your database
            }
        ]
    }
}
```

## Mobiledoc

Mobiledoc is a standardised JSON-based document storage format, which forms the heart of publishing with Ghost. In order to import content into Ghost, it must first be converted to mobiledoc.

Although mobiledoc is a JSON format, the mobiledoc field in the import file should be serialised into a string, which can be done by calling `JSON.stringify()`.

Ghost's importer is not able to accept other formats, such as HTML or markdown. Instead, there are tools available for converting from these formats into mobiledoc prior to importing.

### Converting HTML

The easiest way to convert HTML to mobiledoc is to generate a Ghost JSON file with `html` fields containing your content for each post instead of `mobiledoc`.
You can then use Ghost's standalone migration tool to convert the `html` field to a `mobiledoc` field.

1. Requires Node.js v10 installed locally
2. `npm install @tryghost/migrate -g` - install the migration tooling
3. `migrate json html /path/to/your/import.json` - will convert the HTML fields in your JSON file
4. The tool will output a path to a converted JSON file - use this to import your content
5. Run `npm uninstall @tryghost/migrate -g` to cleanup

This should work well for most semantic HTML, and result in a series of populated cards in the editor, making it easy to update your content in future.

#### Mobiledoc HTML card

If your html consists of tables or other non-semantic markdown, you may have a better experience wrapping the HTML in a single HTML card:

```JavaScript
mobiledoc = JSON.stringify({
    version: '0.3.1',
    markups: [],
    atoms: [],
    cards: [['html', {cardName: 'html', html: '<p>HTML goes here</p>'}]],
    sections: [[10, 0]]
});
```


### Converting Markdown

There are two approaches for converting markdown to mobiledoc. The first is to convert your markdown content to HTML first using the tool of your choice, and then follow the steps above for converting HTML to mobiledoc.

 The second is to wrap your markdown content in a single markdown card:

```JavaScript
mobiledoc = JSON.stringify({
    version: '0.3.1',
    markups: [],
    atoms: [],
    cards: [['markdown', {cardName: 'markdown', markdown: 'markdown content goes here...'}]],
    sections: [[10, 0]]
});
```

## Troubleshooting

There are a number of ways in which an import file may trigger an **Error** or a **Warning** particularly if the JSON file was created by a third-party tool.

### Error

When Ghost detects an error, the import will be cancelled, no information is imported and there should be a suitable error message to help you debug the problem. This also means there may be multiple errors in your import file but you will only see a single error during the import process.

An Error or warning should contain a relevant message on why the import was not successful along with a the relevant JSON entry that caused the issue where applicable.

An example of an error would be a user that contains a null email field:

![Import Failed](../../../images/api/import-failed.png)


### Warning

The import may generate multiple warnings. These are issues that may want to know about but are not significant enough to prevent the data being imported. Examples of this include a duplicate user (either duplicated in your JSON file or matching an existing user) or a post linked to an unknown user.

![Import Warnings](../../../images/api/import-warnings.png)
