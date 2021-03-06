
Mosaic API
=========

## Activity

#### POST Activity

Insert new activity

POST `http://localhost/mosaic/v1/activity`

`curl -H "Content-Type: application/json" -X POST http://localhost/mosaic/v1/activity -d {Activity JSON}`

POST body `{Activity JSON}` example as shown in [Standard activity](../README.md). Make sure to use header: `Content-Type: application/json`. 

For post photo activity, refer to [Upload photo](photo.md)

#### GET Activity (deprecated)

**This endpoint is deprecated. Use home activity to get user home activities.**

Get list of all public activity

GET `http://localhost/mosaic/v1/activity`

## User Activity

#### User header 

If request sent with header `Movreak-Uid: MovreakUserId`, Mosaic will use `MovreakUserId` as reference to current active user.

#### GET home Activity

Get list of home activity

GET `http://localhost/mosaic/v1/home/activity`

With paging

`curl http://localhost/mosaic/v1/home/activity?limit=10&offset=2`

With `Movreak-Uid`

`curl -H "Movreak-Uid: MovreakUserId" http://localhost/mosaic/v1/home/activity?limit=10&offset=2`

`Movreak-Uid` header rules : 

* If header `Movreak-Uid` is set : return current user activities with all his/her (public, follower) followings activities and (public, follower, friend) friends activities
* If not set : return public activities (for user that is not logged in yet)

#### GET user activity

Get list of user activity

GET `http://localhost/mosaic/v1/user/USERID/activity`

With paging

`curl http://localhost/mosaic/v1/user/USERID/activity?limit=10&offset=2`

With `Movreak-Uid`

`curl -H "Movreak-Uid: MovreakUserId" http://localhost/mosaic/v1/user/USERID/activity?limit=10&offset=2`

`Movreak-Uid` header rules : 

* If `Movreak-Uid` is equal to `USERID` : return all (public, private, friend, follower) current active user activities
* If `Movreak-Uid` is equal not to `USERID` : return `USERID` activities based on : 
  * if `Movreak-Uid` is following `USERID` : return `USERID` public and follower activities
  * if `Movreak-Uid` is friend with `USERID` : return `USERID` public, friend, follower activities
  * else : return `USERID` public activities

#### GET user specific activity (based on object type either from `target` or `object` in activity)

GET `http://localhost/mosaic/v1/user/USERID/activity/objectType`

With paging

`curl http://localhost/mosaic/v1/user/USERID/activity/OBJECTTYPE?limit=10&offset=2`

`Movreak-Uid` rules above are applied.

## User Data

#### GET user data

GET `http://localhost/mosaic/v1/user/id`

#### GET followings

GET `http://localhost/mosaic/v1/user/id/following`

#### GET followers

GET `http://localhost/mosaic/v1/user/id/follower`

#### GET friends (mutual following-follower)

GET `http://localhost/mosaic/v1/user/id/friends`

#### Check if user a follows user b

GET `http://localhost/mosaic/v1/user/USER:A:ID/follows/USER:B:ID`

## Database

#### DROP database (Will remove all data) 

Use header `Client-Id`.

`curl -H "Client-Id: SOMETOKEN" -H "Content-Length: 0" -X POST http://localhost/mosaic/v1/drop/activity`

`curl -H "Client-Id: SOMETOKEN" -H "Content-Length: 0" -X POST http://localhost/mosaic/v1/drop/user`