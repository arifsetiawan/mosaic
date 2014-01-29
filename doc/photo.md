
Post Photo
=========

POST `http://localhost/mosaic/v1/activity`

Mosaic support upload image as activity using `Content-Type: multipart/form-data`.
Note that standard insert activity operation is using `Content-Type: application/json`.

Current implementation will :

* Register user to Jepret
* Upload image to share destination using Jepret 

Since posting photo is implemented using `multipart`, then post request should follow `multipart/form-data` format.

```
curl -F 'file=@C:\\Users\\ArifSetiawan\\Downloads\\Mixed\\rocket.jpg' -F 'activity={"title":"Helmi post new photo","actor":{"objectType":"user","id":"user:id:123","displayName":"Kang Helmi","username":"kanghelmi","image":{"url":"http://example.org/user/thumbnail.jpg","width":250,"height":250},"credentials":{"provider":"twitter","token":"250539439-LUx5znSqMkHeahWzUVTEJtRJQunSJg2Ay0IEhbSE","tokenParam":"jWHIZr0XplXLo4XtkAo2eZNiBc5HlkkaST56dsK9B8"}},"verb":"post","object":{"objectType":"photo","id":"photo:id:123","description":"Some action on PVJ!","share":"twitter"},"target":{},"privacy":"public"}' 'http://localhost:3003/mosaic/v1/activity'
```

Two fields are required :

* `file` is containing binary data of image
* `activity` is stringified JSON of activity containing all necessary parameters to perform login and upload with Jepret

```
{
  "title": "Helmi post new photo",
  "actor": {
    "objectType": "user",
    "id": "user:id:123",
    "displayName": "Kang Helmi",
    "username": "kanghelmi",
    "image": {
      "url": "http://example.org/user/thumbnail.jpg",
      "width": 250,
      "height": 250
    },
    "credentials": {
      "provider" : "twitter",
      "token" : "250539439-LUx5znSqMkHeahWzUVTEJtRJQunSJg2Ay0IEhbSE",
      "tokenParam" : "jWHIZr0XplXLo4XtkAo2eZNiBc5HlkkaST56dsK9B8"
    }
  },
  "verb": "post",
  "object" : {
    "objectType": "photo",
    "id": "photo:id:123",
    "description": "Some action on PVJ!",
    "share": "twitter"
  },
  "target": {},
  "privacy":"public"
}
```

Required activity field

* `actor.credentials`
* `object.objectType` equal to `photo`
* `object.description`
* `object.share` should equal to `actor.credentials.provider`

### Return body

Upon succesfull upload, activity will contain image url and credentials is removed.

```
{
  "status": "OK",
  "message": "New activity added",
  "data": [
    {
      "title": "Helmi post new photo",
      "actor": {
        "objectType": "user",
        "id": "user:id:123",
        "displayName": "Kang Helmi",
        "image": {
          "url": "http://example.org/user/thumbnail.jpg",
          "width": 250,
          "height": 250
        }
      },
      "verb": "post",
      "object": {
        "objectType": "photo",
        "id": "photo:id:123",
        "description": "Some action on PVJ!",
        "share": "twitter",
        "image": {
          "url": "http://jepretsupport.blob.core.windows.net/pulcher/528990fcf46bee0006000001/245383260987e59acf54bfd50ff3247a.jpg"
        }
      },
      "target": {},
      "privacy": "public",
      "published": "2014-01-29T08:50:52.403Z",
      "_id": "52e8c0ec4062ecbc1c5ee191"
    }
  ]
}
```