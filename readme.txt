===  ACF Post-2-Post ===
Contributors: Hube2
Tags: acf, advanced custom fields, add on, bidirectional, 2 way, two way, relationship
Requires at least: 4.0
Tested up to: 5.5
Stable tag: 1.5.0
License: GPLv2 or later
License URI: http://www.gnu.org/licenses/gpl-2.0.html

Automatic Two Way (Bidirectional) Relationships with ACF5


== Description ==

***This is an add on plugin for Advanced Custom Fields (ACF) Version 5.***

***This plugin will not work with ACF Version 4.***

This plugin will not provide any functionality if ACF5 is not installed.***

This plugin does not create a new type of field or any admin interface. This plugin when used as 
explained below makes the existing ACF Relationship and Post Object fields work bi-directionaly, 
automatically updating the relationship field on the other end of the relationship.

For more information see [Other Notes](https://wordpress.org/plugins/post-2-post-for-acf/)

== Installation ==

Install like any other plugin

== Screenshots ==

None

== Frequently Asked Questions ==

Nothing yet

== Other Notes ==

== Github Repository ==

This plugin is also on GitHub 
[https://github.com/Hube2/acf-post2post](https://github.com/Hube2/acf-post2post)

== How To Use ==
* Create a relationship or post object field.
* The field must be at the top level. It cannot be a subfield of a repeater or a flexible content field.
* The field name must be the same on all posts. In other words if you want to have different post types be related then you must add a field with the same field name on both post types.

When you add a post to a relationship or post object field and the same field name appears on the post added to the relationship then the relationship field on the related post will be updated to include a relationship to the post being edited.

If a post is removed from a relationship then the post being removed will also be updated to remove the relationship to the post being edited.

== Post Object Fields ==
If a post object field is being used

* If it allows multiple values then it will work the same way that relationship fields work.
* If it does not allow multiple values and the related post already contains a value see Overwrite Settings

== Overwrite Settings ==
If the field in a related post, whether it is a post object field that only allows 1 value or a relationship field that has a maximum number of related posts, if the field in the related post already has the maximum number of values allowed, by default, a new value will not be added. You can override this default by specifying overwrite settings.

How to add overwrite settings
`
add_filter('acf-post2post/overwrite-settings', 'my_overwrite_settings');
function my_overwrite_settings($settings) {
  $settings['field_name'] = array(
      'overwrite' => true,
        'type' => 'first'
  );
  return $settings;
}
`

Each element of the $settings array is an array. The index of the array is the field that you want to specify settings for. Each field can have 2 arguments.

* overwrite: true/false or 1/0. If set to true or 1 then new values will overwrite older values. The default value of this setting is false.
* type: 'first' or 'last'. Which of the existing values should be removed, the first one added or the last. The default value is 'first'.

after a value is removed from the existing list the new value is added to the end of the list.



== Field Exeptions ==

You can disable automatic bidirectional relationships for specific field keys using the filter
`
// field_XXXXXXXX = the field key of the field
// you want to disable bidirectional relationships for
add_filter('acf/post2post/update_relationships/key=field_XXXXXXXX', '__return_false');
`


== After update hooks ==
There are two actions that can be used after a post is updated and passes a single post ID. Please make sure you see the subtle difference in these two hooks.

The first is run after each related post is updated
`
add_action('acf/post2post/relationship_updated', 'my_post_updated_action');
function my_post_updated_action($post_id) {
  // $post_id == the post ID that was updated
  // do something after the related post is updated
}
`

The second is run after all posts are updated and passes an array of post IDs.
`
add_action('acf/post2post/relationships_updated', 'my_post_updated_action');
function my_post_updated_action($posts) {
  // $posts == and array of post IDs that were updated
	// do something to all posts after update
	foreach ($posts as $post_id) {
	  // do something to post
	}
}
`

== Changelog ==

= 1.5.0 =
* Added filter to allow specifying to append or prepend new relationship (can supercede overwrite setting) 

= 1.4.1 =
* Corrected logic error in overwrite
* add context to action hooks

= 1.4.0 =
* added actions after updates to related posts to allow 3rd party integrations

= 1.3.2 =
* removed donation nag

= 1.3.1 =
* Corrected bug in 1.3.0 that prevented all fields from updating correctly

= 1.3.0 =
* added filter to allow disabling bidirectional relationships on fields by field key

= 1.2.8 =
* changed from plugins_loaded to after_setup_theme for checking if ACF >= 5 is installed to allow for ACF being installed in themes

= 1.2.7 =
* replace php array_walk() w/array_map() to correct issue with str/int conversion of IDs

= 1.2.6 =
* corrected serialization of post IDs as strings instead of integers to allow correct ACF meta_key value searching of serialized ID values useing `LIKE "{ID}"`

= 1.2.5 =
* plugin disabled if ACF5 not installed
* plugin deactivated if ACF5 not installed

= 1.2.4 =
* removed github updater support

= 1.2.3 =
* initial release to WordPress.org

