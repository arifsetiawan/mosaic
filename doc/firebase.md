
Firebase support
=========

Mosaic now support firebase. Sign up to [firebase](https://www.firebase.com/) to create new app.

Every user logged in will have unique url using his/her `Movreak-Uid` with following formats

```
https://mysuperfirebaseapp.firebaseio.com/userid/notifications
```

## Firebase rules

Currently firebase support direct notification actions with notification target is specified in Activity Stream. Mosaic will update user ref child when: 

* `object.objectType` is user then the user with `object.id` will be notified
* `target.objectType` is user then the user with `target.id` will be notified
* If both `target` and `object` exists with type of user, Mosaic will update both

Refer to [PMJ reference](pmj.md) for PMJ styled activity example.

You have to explicitly tell Mosaic to send notification to either `object` or `target` using `notify` field in the activity data. 

Currently, firebase integration do not support [post photo activity](photo.md)

## Examples

### 1. John like Mary

```
  {
    "notify": true,
    "title": "John like Mary", 
    "actor": {
      "objectType": "user",
      "id": "user:id:123",
      "displayName": "John",
      "other key": "other value",
      "profilePage": "http://example.org/profile/user:id:123",
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
      "profilePage": "http://example.org/profile/user:id:234",
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

Mosaic will add new child to `object.id` firebase url (which user with id `object.id` will listen when logged in)

```
https://mysuperfirebaseapp.firebaseio.com/user:id:234/notifications
```

With body

```
  {
    "read" : false
    "title": "John like Mary",
    "actor": {
      "objectType": "user",
      "id": "user:id:123",
      "displayName": "John",
      "other key": "other value",
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

`read` is used to flag if this notification is read.

#### Reconstructing activity text

Application need to be able to construct activity text in html format using informations in activity stream data. Using actor-verb-object-target format or in passive form.

In John page. text would be : [You](http://example.org/profile/user:id:123) like [Mary](http://example.org/profile/user:id:234)

In Mary page. text would be : [John](http://example.org/profile/user:id:123) likes [you](http://example.org/profile/user:id:234)

### 2. John post message to Mary

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
    "targetPre": "to" 
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
    "privacy":"public"
  }
```

In John page. text would be : [You](http://example.org/profile/user:id:123) send [message](http://example.org/message/message:id:234) to [Mary](http://example.org/profile/user:id:234)

In Mary page. text would be : [John](http://example.org/profile/user:id:123) send [message](http://example.org/message/message:id:234) to [you](http://example.org/profile/user:id:234)

#### Passive activity text

In some cases, it is desirable to show the text in passive form: **John send message to you** into **You received a message from John** 

Note that, it is **up to you** to add whatever fields necessary to construct the sentence. 
