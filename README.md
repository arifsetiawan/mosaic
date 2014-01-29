
Mosaic
=========

Simple Node.js based social graph and activity stream API server

## Functionalities

* user activity stream (insert and get)
* maintain user following followers

## API

* [Mosaic API](https://github.com/arifsetiawan/mosaic/blob/master/doc/api.md)
* [Comments](https://github.com/arifsetiawan/mosaic/blob/master/doc/comment.md)
* [Upload photo](https://github.com/arifsetiawan/mosaic/blob/master/doc/photo.md)

## Activity Stream

Mosaic do not handle the details of the activity stream. It simply just doing insert and get on them. It defines some necessary field to make some query easier. 

Mosaic follows [Activity Streams definition of activity](http://activitystrea.ms/specs/json/1.0/)

### Standard activity item

Required field in activity are : `title`, `actor`, `verb`, `object`, `target`, `privacy`.
Refer to [Activity Serialization](http://activitystrea.ms/specs/json/1.0/#activity) for details about each field.

`actor`, `object` and `target` MUST have id field. Application are free to add other fields. 

User empty json object if `target` is not exists


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

* `folllow` and `unfollow`. Use to set following and follower lists. This will affect what activity specific user can see

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

### Privacy

* `public` will be shown to all users
* `private` will be available only to user who do the activity
* `friend` will be available to user who do the activity and his/her followers

### Mandatory Fields

Current implementation do not enforce some fields that should be existing in activity JSON body required for certain operations. Please read the documentation to make sure all fields are provided.
