
Mosaic API
=========

## Activity

#### POST Activity

Insert new activity

POST `http://localhost/mosaic/v1/activity`

`curl -H "Content-Type: application/json" -X POST http://localhost/mosaic/v1/activity -d {Activity JSON}`

POST body `{Activity JSON}` example as shown in Standard activity item above. Make sure to use header: `Content-Type: application/json`. 

#### GET Activity

Get list of all public activity

GET `http://localhost/mosaic/v1/activity`

With paging

`curl http://localhost/mosaic/v1/activity?limit=10&offset=2`


## User

#### GET user activity

Get list of all user activity (including from users that he/she follow if it's privacy set to friend)

GET `http://localhost/mosaic/v1/user/id/activity`

With paging

`curl http://localhost/mosaic/v1/user/USERID/activity?limit=10&offset=2`

With paging and show only users activity (not from his following)

`curl http://localhost/mosaic/v1/user/USERID/activity?limit=10&offset=2&self=true`

#### GET user specific activity (based on object type either from `target` or `object` in activity)

GET `http://localhost/mosaic/v1/user/id/activity/objectType`

With paging

`curl http://localhost/mosaic/v1/user/USERID/activity/OBJECTTYPE?limit=10&offset=2`

With paging and show only users activity (not from his following)

`curl http://localhost/mosaic/v1/user/USERID/activity/OBJECTTYPE?limit=10&offset=2&self=true`

#### GET followings

GET `http://localhost/mosaic/v1/user/id/following`

#### GET followers

GET `http://localhost/mosaic/v1/user/id/follower`


## Database

#### DROP database (Will remove all data) 

Use header `Client-Id`.

`curl -H "Client-Id: SOMETOKEN" -H "Content-Length: 0" -X POST http://localhost/mosaic/v1/drop/activity`

`curl -H "Client-Id: SOMETOKEN" -H "Content-Length: 0" -X POST http://localhost/mosaic/v1/drop/user`