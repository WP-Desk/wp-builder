[![Latest Stable Version](https://poser.pugx.org/wpdesk/wp-builder/v/stable)](https://packagist.org/packages/wpdesk/wp-builder)
[![Total Downloads](https://poser.pugx.org/wpdesk/wp-builder/downloads)](https://packagist.org/packages/wpdesk/wp-builder)
[![Latest Unstable Version](https://poser.pugx.org/wpdesk/wp-builder/v/unstable)](https://packagist.org/packages/wpdesk/wp-builder)
[![License](https://poser.pugx.org/wpdesk/wp-builder/license)](https://packagist.org/packages/wpdesk/wp-builder)

# WP Builder

`wpdesk/wp-builder` provides small base classes and interfaces for building WordPress plugins.

## Requirements

PHP 7.4+.

## Installation via Composer

Install with Composer:

```bash
composer require wpdesk/wp-builder
```

## Example usage

Basic example for a plugin bootstrap file:

```php
<?php

use WPDesk\PluginBuilder\Plugin\AbstractPlugin;
use WPDesk\PluginBuilder\Plugin\Hookable;
use WPDesk\PluginBuilder\Plugin\HookableCollection;
use WPDesk\PluginBuilder\Plugin\HookableParent;

class ContentExtender implements Hookable {

	public function hooks() {
		add_filter( 'the_content', [ $this, 'append_sample_text_to_content' ] );
	}

	public function append_sample_text_to_content( $content ) {
		return $content . ' Sample text';
	}
}

class ExamplePlugin extends AbstractPlugin implements HookableCollection {

	use HookableParent;

	protected function hooks() {
		parent::hooks();
		$this->add_hookable( new ContentExtender() );
		$this->hooks_on_hookable_objects();
	}
}

$plugin_info = new WPDesk_Plugin_Info();
$plugin_info->set_plugin_file_name( __FILE__ );
$plugin_info->set_plugin_dir( __DIR__ );
$plugin_info->set_plugin_url( plugin_dir_url( __FILE__ ) );
$plugin_info->set_plugin_name( 'Example Plugin' );
$plugin_info->set_text_domain( 'example-plugin' );

$plugin = new ExamplePlugin( $plugin_info );
$plugin->init();

```
