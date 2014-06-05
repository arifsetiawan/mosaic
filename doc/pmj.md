
PMJ Styled Mosaic
=========

## Uid header

Mosaic do not maintain user data, as such, the client must submit necessary information to establish current user who want to interact with Mosaci. In [API documentation](api.md), we have `Movreak-Uid`. For pmj we will use header `Pmj-Uid`. Do replace all `Movreak-Uid` references into `Pmj-Uid` when implementing Mosaic client.

Note that this Uid must be the same as `id` in `actor` field of the Activity Stream. 

## Privacy

By default, all PMJ activity are **private** to the actor and object/target of the activity.

## Some of the pmj activity

* John like Mary profile
* John favorite Mary
* John post message to Mary
* John viewed Mary profile
* others

### Common verbs

* like
* favorite
* send
* view
* post

## Example activity 

We will use convention that **Mary** and **Mary profile** to be the same object since Mary is represented by her profile in pmj website.

Note that you can add any field in `actor`, `object` or `target`. Mosaic will just store them. Its up to you how to use those fields. 

#### 1. John like Mary profile

```
  {
    "title": "John like Mary profile",
    "actor": {
      "objectType": "user",
      "id": "user:id:123",
      "displayName": "John",
      "other key": "other value",
      "url": "http://example.org/profile/user:id:123",
      "image": {
        "url": "http://example.org/user/thumbnail.jpg",
        "width": 250,
        "height": 250
      }
    },
    "verb": "like",
    "object": {
      "objectType": "user",
      "id": "user:id:234",
      "displayName": "Mary",
      "other key": "other value",
      "url": "http://example.org/profile/user:id:234",
      "image": {
        "url": "http://example.org/album/thumbnail.jpg",
        "width": 250,
        "height": 250
      },
    },
    "target" : {},
    "privacy":"private"
  }
```

**John favorite Mary** and **John view Mary profile** will have similar activity data with different verb.

#### 2. John post message to Mary

Here John (actor) post (verb) message (object) to Mary (target)

```
  {
    "title": "John post message to Mary",
    "actor": {
      "objectType": "user",
      "id": "user:id:123",
      "displayName": "John",
      "other key": "other value",
      "url": "http://example.org/profile/user:id:123",
      "image": {
        "url": "http://example.org/user/thumbnail.jpg",
        "width": 250,
        "height": 250
      }
    },
    "verb": "send",
    "object": {
      "objectType": "message",
      "id": "message:id:234",
      "content": "Hi, apa kabar?",
      "other key": "other value",
      "url": "http://example.org/message/message:id:234"
    },
    "target" : {
      "objectType": "user",
      "id": "user:id:234",
      "other key": "other value",
      "displayName": "Mary",
      "url": "http://example.org/profile/user:id:234",
      "image": {
        "url": "http://example.org/album/thumbnail.jpg",
        "width": 250,
        "height": 250
      }
    },
    "privacy":"private"
  }
```
