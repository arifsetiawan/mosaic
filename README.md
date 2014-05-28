
Mosaic
=========

Simple Node.js based social graph and activity stream API server

## Functionalities

* user activity stream (insert and get)
* maintain user following followers

## API

* [Mosaic API](doc/api.md)
* [Comments](doc/comment.md)
* [Upload photo](doc/photo.md)
* [Activity check](doc/activitycheck.md)
* [Firebase](doc/firebase.md)

**IMPORTANT**

Read [here first for PMJ styled Mosaic documentation](doc/pmj.md)

## Activity Stream

Mosaic do not handle the details of the activity stream. It simply just doing insert and get on them. It defines some necessary field to make some queries easier. 

Mosaic loosely follows [Activity Streams definition of activity](http://tools.ietf.org/id/draft-snell-activitystreams-05.html)

### Standard activity item

Refer to [Activity Serialization](http://tools.ietf.org/id/draft-snell-activitystreams-05.html#rfc.section.3.5) for details about each fields in activity stream.

Check in [Activity check](doc/activitycheck.md) for fields that required for specific activity verb and object.

```
  {
    "title": "Helmi post new review for movie The Hobbit",
    "actor": {
      "objectType": "user",
      "id": "user:id:123",
      "displayName": "Kang Helmi",
      "other key": "other value"
      "image": {
        "url": "http://example.org/user/thumbnail.jpg",
        "width": 250,
        "height": 250
      }
    },
    "verb": "post",
    "object" : {
      "objectType": "review",
      "id": "review:id:123",
      "content" : "Great movie!"
      "other key": "other value"
    },
    "target": {
      "objectType": "movie",
      "id": "movie:id:123",
      "title": "The Hobbit",
      "image": {
        "url": "http://example.org/album/thumbnail.jpg",
        "width": 250,
        "height": 250
      }
      "other key": "other value"
    },
    "privacy":"public"
  }

```

```
  {
    "title": "Helmi follow Andri",
    "actor": {
      "objectType": "user",
      "id": "user:id:123",
      "displayName": "Kang Helmi",
      "other key": "other value",
      "image": {
        "url": "http://example.org/user/thumbnail.jpg",
        "width": 250,
        "height": 250
      }
    },
    "verb": "follow",
    "object": {
      "objectType": "user",
      "id": "user:id:234",
      "displayName": "Andri",
      "image": {
        "url": "http://example.org/album/thumbnail.jpg",
        "width": 250,
        "height": 250
      },
      "other key": "other value"
    },
    "target" : {},
    "privacy":"public"
  }
```

### Verb

Applications are free to use any verbs such as: post, follow, unfollow, plan, watch, etc.

Mosaic is handling specific verbs such as:

* `follow` and `unfollow`. Use to set following and follower lists. This will affect what activity specific user can see

### Title Example Breakdown

`Helmi post new review for movie The Hobbit`

* actor : Helmi
* action : post
* object : review
* target : movie the Hobbit

`Helmi follow Andri`

* actor : Helmi
* action : follow
* object : Andri
* target : None

### Privacy options

* `public` will be shown to all users. Default privacy setting.
* `private` will be available only to user who do the activity
* `follower` will be available to user who do the activity and his/her followers
* `friend` will be available to user who do the activity and his/her mutual following-followers

Every `friend` is a `follower` but not every `follower` is a `friend`

### Required Fields

Current implementation enforce some fields that should be existing in activity JSON body required for certain operations. Please refer to [Activity check](doc/activitycheck.md) to make sure all required fields are provided.
