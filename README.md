# Post 2 Post for ACF

Creates two way relationships in Advanced Custom Fields

This plugin will provide no functionality if ACF is not installed and active.

## How to use

* Create a relationship or post object field.
* The field must be at the top level. It cannot be a subfield of a repeater or a flexible content field.
* The field name must be the same on all posts, post types and field groups to be used. In other words if you want to have different post types be related then you must add the same type of field with the same field name on both post types.

When you add a post to a relationship or post object field and the same field name appears on the post added to the relationship then the relationship field on the related post will be updated to include a relationship to the post being edited.

If a post is removed from a relationship then the post being removed will also be updated to remove the relationship to the post being edited.

## Post Object Fields

If a post object field is being used

* If it allows multiple values then it will work the same way that relationship fields work.
* If it does not allow multiple values and the related post already contains a value that value will not be overwritten by the new value. 
