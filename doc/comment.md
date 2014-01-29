
Mosaic Comment
=========

Comment is an `activity` to `post comment` another `activity`

User activity : 

```
{
  "_id" : ObjectId("52e7b1ced623ae0c195ab7ef"),
  "actor" : {
    "objectType" : "user",
    "id" : "user:id:123",
    "displayName" : "Kang Helmi",
    "image" : {
      "url" : "http://example.org/user/thumbnail.jpg",
      "width" : 250,
      "height" : 250
    }
  },
  "object" : {
    "objectType" : "review",
    "id" : "review:id:123",
    "content" : "Great movie!",
  },
  "privacy" : "public",
  "published" : ISODate("2014-01-28T13:34:06.374Z"),
  "target" : {
    "objectType" : "movie",
    "id" : "movie:id:123",
    "title" : "The Hobbit",
    "image" : {
      "url" : "http://example.org/album/thumbnail.jpg",
      "width" : 250,
      "height" : 250
    },
  },
  "title" : "Helmi post new review for movie The Hobbit",
  "verb" : "post"
}
```

Comment activity :

```
{
  "title": "Bayu post a comment for Helmi's review",
  "actor": {
    "objectType": "user",
    "id": "user:id:234",
    "displayName": "Bayu",
    "image": {
      "url": "http://example.org/user/thumbnail.jpg",
      "width": 250,
      "height": 250
    }
  },
  "verb": "post",
  "object": {
    "objectType": "comment",
    "id": "comment:id:123",
    "content":"I agree!"
  },
  "target: {
    "objectType": "activity",
    "id": "52e7b1ced623ae0c195ab7ef"
  }
  "privacy":"public"
}
``` 

Then `comment` is stored as activity and its id is referenced in `target activity`

```
{
  "_id" : ObjectId("52e7b1ced623ae0c195ab7ef"),
  "actor" : {
    "objectType" : "user",
    "id" : "user:id:123",
    "displayName" : "Kang Helmi",
    "image" : {
      "url" : "http://example.org/user/thumbnail.jpg",
      "width" : 250,
      "height" : 250
    }
  },
  "comments" : [ObjectId("52e7b50d102cd6d80d688cff")],
  "object" : {
    "objectType" : "review",
    "id" : "review:id:123",
    "content" : "Great movie!",
  },
  "privacy" : "public",
  "published" : ISODate("2014-01-28T13:34:06.374Z"),
  "target" : {
    "objectType" : "movie",
    "id" : "movie:id:123",
    "title" : "The Hobbit",
    "image" : {
      "url" : "http://example.org/album/thumbnail.jpg",
      "width" : 250,
      "height" : 250
    },
  },
  "title" : "Helmi post new review for movie The Hobbit",
  "verb" : "post"
}
``` 

### Requirements

* `object.objectType` equal to `comment`
* `target.objectType` equal to `activity`  
* `target.id` equal to target `activity` `_id`

### Query

Comment will not appear in first level activity query (comment is an response to another activity).

Recursive query is performed to expand comments from `_id` to actual data. Since its recursive, a comment can contain another comment.  
