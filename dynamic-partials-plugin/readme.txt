DYNAMIC PARTIALS is a mini framework

that allows you to build a site with php blocks that can be inserted in the editor,
where you'll see only a placeholder.
These php blocks are partials that can include easily a js just by creating the js file.

It also includes an easy way to reload these partials whenever we want, by using
ajax. This entails that these templates and functions will need to be ready to work
with args or with $_POST variables.


# Conventions



edit config.php to set the folder for the
- php templates
- where to include the js files for the

This folder deserves a little explanation. It's already mentioned in the README.md, but here we repeat it.

# Differences between blocks (data-dynamic-templates-reload) and subtemplates (data-dynamic-subtemplates-reload)

- blocks are created to be used in the editor
- subtemplates are used only programmatically, form other blocks or parts

# Usage:

Create:
- The action php to be called and declare as ajax
- The template view php to reload
- And:
```
<form onsubmit="window.dynamicPartials.handleSubmitFormAndLoadTemplateAjax(event)"
		data-dynamic-templates-reload=""
		data-dynamic-subtemplates-reload="partial-example-subpartial"
		>

		<input type="hidden" name="action" value="add_contribution_year" />
		<input type="hidden" name="ticker" value="<?php echo esc_attr( $symbol ); ?>" />
		<?php wp_nonce_field( 'nonce_action', 'nonce', false ); ?>

		<input type="text" name="ticker" value="<?php echo esc_attr( $ticker ); ?>" />
```
** * [data-dynamic-template-reload] or data-dynamic-subtemplates-reload:
	the name of the file (wihtout .php) in the 'php-partials-path' ("/dynamic-partial-templates/blocks/")
	or you can include the whole path of the partial, eg. 'dynamic-partial-templates/sub-templates/partial-show-ticker-info'

** * input[name="action"] the ajax name in php declared in php with the hooks

** * `wp_ajax_<name_for_your_action>` and `wp_ajax_nopriv_<name_for_your_action>`
the nonce that will be validated in the php file with wp_verify_nonce( $_POST['nonce'], '<name_of_nonce_action>')

** *add the rest of inputs with the value that will be used (through $_POST) in botn
- th e php `wp_ajax_<name_for_your_action>`
- and the partial template view that will be reloaded as $args

=======
> Note: we need to restart wp scripts every time we create a new block dynamic template part with frontend js.

We want to use `shortcodes` in the editor. But shortcodes are deprecated.
We can use /patterns and create a php file which can be inserted in the editor as a Pattern.
But sometimes the template part comes with some js. Then the conception that covers it is a Block.

But in this case we don't need to make the block look like a block in the Editor, and we don't need
to use other features of the block like attributes, inspector controls or whatever. We could create
them with ACF, but I prefered to do something house made.

With this system, we can easily create a new template part in php, with js associated in the view.
No attributes, no need to use block.json ... just a php file, and, optionally, the .js file with the
same name. That js can call more js if you want to, if you need more complexity.

This folder allows you to add a new dynamic block easily, as if it where a template part,
but made in php, not just html.
