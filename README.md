# Wordpress-Tips-and-Trick

## Starter Themes
1. [underscores.me](https://underscores.me/) 
2. [Sage](https://roots.io/sage/)
## Plugins
## CPT
## TAXONOMY
## METABOX
in metabox we have 3 important things to remember :
1. Register metabox
2. Callback to show custom field in administrator page
3. Register Save function for metabox


__1. Register metabox__
This is how to add meta box
```PHP
// add metabox
function example_add_metabox()
{
      add_meta_box(
        'unique_id', // ID 
        'Box Title',  // Box title
        'callback_for_html',  // Content callback
        'post_type' // Post type
      );
}
add_action('add_meta_boxes', 'example_add_metabox');
```

__2. Callback to show custom field in administrator page__
 
 the callback function name must be registered in "add_meta_box" before.
 
```PHP
// this is callback with name "callback_for_html" like our example above
function callback_for_html($post)
{
    // this is how to get registered custom_metabox_field;
    // don't forget to add true in the of function get_post_meta to return single value
    $custom_meta_box_field = get_post_meta($post->ID, 'custom_metabox_field', true);
    ?>
    <p>
      <label>Custom metabox field</label>
      <input name="custom_metabox_field" class="widefat" value="<?php echo $custom_metabox_field; ?>"/>
    </p>
    <?php
}
```

__3. Register Save function for metabox__

this another important things about metabox, we must register what field in callback function must be saved after form submited.

```PHP
function example_save_metabox($post_id)
{
    // if you use "update_post_meta" when data exists it will be updated and created new data when does'nt exists
    update_post_meta( $post_id, 'custom_metabox_field', $_POST['custom_metabox_field']);
    
    // if you use "insert_post_meta" it will be added new data even data exists 
    // insert_post_meta( $post_id, 'custom_metabox_field, $_POST['custom_metabox_field']);
}
add_action('save_post', 'example_save_metabox');
```
