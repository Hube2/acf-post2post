# Post 2 Post for ACF

Creates two way (bidirectional) relationships in Advanced Custom Fields

This plugin will provide no functionality if ACF is not installed and active.

This plugin does not create a new type of field or any admin interface. This plugin when used as explained below makes the
existing ACF Relationship and Post Object fields work bi-directionaly, automatically updating the relationship field on
the other end of the relationship.

A couple of months after I created this plugin the developer posted a tutorial on how to do this using a filter.
That example basically does the same thing except it seems to require the fields to have the same key as well
as the same name where this plugin will let you mix fields as long as they are of a type that allows a relationship
and they have the same name.

[Questions? Bugs? Comments?](https://github.com/Hube2/acf-post2post/issues)

## How to use

* Create a relationship or post object field.
* The field must be at the top level. It cannot be a subfield of a repeater or a flexible content field.
* The field name must be the same on all posts. In other words if you want to have different post types be related then you must add a field with the same field name on both post types.

When you add a post to a relationship or post object field and the same field name appears on the post added to the relationship then the relationship field on the related post will be updated to include a relationship to the post being edited.

If a post is removed from a relationship then the post being removed will also be updated to remove the relationship to the post being edited.

## Post Object Fields

If a post object field is being used

* If it allows multiple values then it will work the same way that relationship fields work.
* If it does not allow multiple values and the related post already contains a value see *Overwrite Settings*

## Overwrite Settings

If the field in a related post, whether it is a post object field that only allows 1 value or a relationship field that has a maximum number of related posts, if the field in the related post already has the maximum number of values allowed then, by default, a new value will not be added. You can override this default by specifying overwrite settings.

How to add overwrite settings
```
add_filter('acf-post2post/overwrite-settings', 'my_overwrite_settings');
function my_overwrite_settings($settings) {
  $settings['field_name'] = array(
	  'overwrite' => true,
		'type' => 'first'
	);
	return $settings;
}
```
Each element of the $settings array is an array. The index of the array is the field that you want to
specify settings for. Each field can have 2 arguments.  
`overwrite` = true/false or 1/0. If set to true
or 1 then new values will overwrite older values. The default value of this setting is false.  
`type` = 'first' or 'last'. Which of the existing values should be removed, the first one added or the last. The default value is 'first'.  
after a value is removed from the existing list the new value is added to the end of the list.

## Field Exeptions

You can disable automatic bidirectional relationships for specific field keys using the filter
```
// field_XXXXXXXX = the field key of the field
// you want to disable bidirectional relationships for
add_filter('acf/post2post/update_relationships/key=field_XXXXXXXX', '__return_false');
```

## After update hooks
There are two actions that can be used after a post is updated and passes a single post ID. Please make sure you see the subtle difference in these two hooks.

The first is run after each related post is updated
```
add_action('acf/post2post/relationship_updated', 'my_post_updated_action');
function my_post_updated_action($post_id) {
  // $post_id == the post ID that was updated
  // do something after the related post is updated
}
```

The second is run after all posts are updated and passes an array of post IDs.
```
add_action('acf/post2post/relationships_updated', 'my_post_updated_action');
function my_post_updated_action($posts) {
  // $posts == and array of post IDs that were updated
	// do something to all posts after update
	foreach ($posts as $post_id) {
	  // do something to post
	}
}
```

## So why do this?

I did not actually create this plugin to make it easier on the person that's managing a site, although that is an added side benifit. The main reason for implementing some way to make relationship and post object fields biderectional is that doing reverse relationship queries for ACF is a huge PITA. I completely understand why this is not built into ACF. If it was built in then Elliot would need to deal with relationship fields in repeaters and
flex fields and nested repeaters. And then there is the problem of relationships with different post types and fields with different names. It would be a deep dark rabbit hole from which I don't think he'd return. There are too many parameters and possibilities. On the other hand, filters and actions can be created with a finite number of options to do the work that's needed.

#### Automatic Updates
Github updater support has been removed. This plugin has been published to WordPress.Org here
https://wordpress.org/plugins/post-2-post-for-acf/. If you are having problems updating please
try installing from there. 

