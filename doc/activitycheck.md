
Mosaic Activity check
=====================

Mosaic impose some required fields to process activities. Mosaic will return error if the requirements are not met.

## Required Activity fields

* `title` - String summarized the activity.
* `actor` - Describes one or more entities that either peformed or are expected to perform the activity. Actor of the activity (JSON object)
* `verb` - Identifies the type of activity. String describing the action. Note that `verb` must be string.
* `object` - Describes the primary object of the activity. Object of the activity (JSON object)

### `Target` field

`target` Describes the target of the activity. The precise meaning of the activity's target is dependent on the activities "verb", but will often be the object the English preposition "to".

`target` field is required only for some activities type, such as `post comment`

## Optional fields

* `privacy` - Activity privacy level. If not defined, `privacy` will be public  

### Privacy options

* `public` will be shown to all users. Default privacy setting.
* `private` will be available only to user who do the activity
* `follower` will be available to user who do the activity and his/her followers
* `friend` will be available to user who do the activity and his/her mutual following-followers

## Required Actor fields

* `id` - Must be string
* `objectType` - Must be string

## Required Object fields

* `id` - Must be string
* `objectType` - Must be string

## Post comment activity

Post comment required `target` field in activity

`target.id` is required and must be string
`target.objectType` is required and must be string

`target.id` must be 24 length string Mongodb _id (for example: '52ea6cbb4a94b10c1ee86369')

## Post photo activity

Post photo is using Jepret platform.

`activity.actor` must contain `credentials`.

`credentials` must contain

* `provider`
* `token`
* `tokenParam`. Used for twitter. Use empty string for facebook

`activity.object` must contain both `share` and `description`
