# CakePHP Localized plugin
[![Build Status](https://secure.travis-ci.org/cakephp/localized.svg?branch=master)](http://travis-ci.org/cakephp/localized)
[![License](https://poser.pugx.org/cakephp/localized/license.svg)](https://packagist.org/packages/cakephp/localized)

This plugin contains various localized validation classes for specific countries.

## Requirements

The master branch has the following requirements:

* CakePHP 3.1.0 or greater.
* PHP 5.4.16 or greater.

## Installation

You can install this plugin into your CakePHP application using [composer](http://getcomposer.org).

The recommended way to install composer packages is:

```
composer require --dev cakephp/localized
```

## Model validation

Localized validation classes can be used for validating model fields.

```php
<?php
App::uses('MxValidation', 'Localized.Validation');

class Post extends AppModel {

	public $validate = array(
		'postal' => array(
			'valid' => array(
				'rule' => array('postal', null, 'mx'),
				'message' => 'Must be valid mexico postal code'
			)
		)
	);
}
```

For further information on validation rules see the [cakephp documentation on validation](http://book.cakephp.org/2.0/en/models/data-validation.html)

## Using localized validations with Validation

You can also access the localized validators any time you would call `Validation` methods. After importing the validation class.

```php
if (Validation::postal($value, null, 'cz')) {
	// Do something with valid postal code
}
```

## Dynamic forwarding of validation
Using a snippet like the following could reduce the manual include/setup part when using multiple localizations:
```php
// Inside your custom validation rule validatePostal()
// $country (de, at, ...) and $value (12345, 1234, ...) given
$className = ucfirst($country) . 'Validation';
App::uses($className, 'Localized.Validation');

// Skip if we don't have that country, then only check for existence
if (!class_exists($className) || !method_exists($className, 'postal')) {
	return !empty($value);
}

try {
	$result = Validation::postal($value, null, $country);
} catch (NotImplementedException $e) {
	$result = !empty($value);
}

return $result;
```

## PO files

This plugin also houses translations for the client-facing translated strings in the core (the `cake` domain). to use these files link or copy them
into their expected location: `APP/Locale/<locale>/LC_MESSAGES/cake.po`

## LC_TIME files

This plugin also houses POSIX compliant LC_TIME files which are used for translating
time related string of LC_TIME domain. To use these files link or copy them into
their expected location: `APP/Locale/<locale>/LC_TIME`.

## Migration Guide

### Localized.Validation

The lib path for validation files changed. They should now be either in `APP/Validation/` or in `APP/PluginName/Validation/`.

You also need to adjust your App::uses() statements in your code accordingly:

`App::uses('MxValidation', 'Localized.Lib');` is now `App::uses('MxValidation', 'Localized.Validation');`

## Contributing to Localized

If you find that your country is not part of the Localized plugin, please fork the project on github.  Once you have forked the project you can commit your validator class (and any test cases).  Once you have pushed your changes back to github send a pull request, and your changes will be reviewed and merged in or feedback will be given.

### Validation methods

There are a few methods that are common to all classes, defined through the interface "ValidationInterface":

* `phone()` to check a phone number
* `postal()` to check a postal code
* `personId()` (and `ssn()` for BC) to check a country specific person ID

Please try to fit your validation rules in that naming scheme.
Apart from that you can also define further validation methods in your implementing class, of course.

## Issues with Localized

If you have issues with Localized, you can report them at https://github.com/cakephp/localized/issues
