
# Composer Assets Plugin

Composer plugin for installing assets.

<a href="https://www.patreon.com/bePatron?u=9680759"><img src="https://c5.patreon.com/external/logo/become_a_patron_button.png" alt="Become a Patron!" height="35"></a>


## Installation

Use [Composer](http://getcomposer.org/):

```
composer require pdima88/composer-assets-plugin
```

Library requires PHP 5.4.0 or later.


## Commands

* `composer refresh-assets` - refresh files in `assets` directory


## Assets configuration

### Packages

* `assets-files` in section `extra`
	* `true` - symlinks whole package directory
	* file path - symlinks one file or directory
	* list of file paths - symlinks files/directories

Example:

``` json
{
	"extra": {
		"assets-files": [
			"static/plugin.js",
			"static/plugin.css",
			"static/icons.png"
		]
	}
}
```

* `static/plugin.js` - symlinks file to `assets/org/package/plugin.js`
* `static/plugin.css` - symlinks file to `assets/org/package/plugin.css`
* `static/icons.png` - symlinks file to `assets/org/package/icons.png`

By default, assets files stored in `assets-dir` specified in root package composer config. Package name used as path, for example, for package 
`package-owner`/`package-name` following path will be used by default: `assets-dir`/`package-owner`/`package-name`.
You can specify different default path for package with option `assets-target` in section `extra`:

* `assets-target` target path to copy assets files, optional
    * if relative path specified it will be related to `assets-dir`
    * if absolute path specified it will be related to parent of `vendor-dir`

Example:

``` json
{
    "extra": {
        "assets-target": "package1",
        "assets-files": [
               ... some assets files ...
        ]
    }
}

Relative path specified, assets files will be copied to `assets-dir`/package1

``` json
{
    "extra": {
        "assets-target": "/templates/default/js",
        "assets-files": [
               ... some assets files ...
        ]
    }
}

Absolute path specified, assets files will be copied to `project-dir`/templates/default/js, where `project-dir` is parent directory of composer `vendor-dir`
IMPORTANT: Assets files will not be cleaned on package update or removing, if default absolute path for `assets-target` is used. 
If you need automatic cleanup of assets files for this package you must redefine `assets-target` for this package in root package config.

### Root package

* `assets-dir` - directory for installing of assets, default `assets`, relative to `vendor-dir`
* `assets-directory` - alias for `assets-dir`
* `assets-files` - list of asset files in incompatible packages, it overrides `assets-files` from installed packages
* `assets-strategy` - install strategy for assets
	* `auto` - select strategy by platform (default value)
	* `copy` - copy all assets, default strategy on Windows
	* `symlink` - create relative symlinks, default strategy on non-Windows platforms
* `assets-target` - target directory for specific packages, relative to `vendor-dir`, must be out of `assets-dir`

Example:

``` json
{
	"config": {
		"assets-dir": "public",
		"assets-files": {
			"org/package": true,
			"org/package2": "js/calendar.js",
			"org/package3": [
				"static/plugin.js",
				"static/plugin.css",
				"static/icons.png"
			]
		},
		"assets-target": {
			"ckeditor/ckeditor": "admin/wysiwyg"
		}
	}
}
```

* `org/package` - symlinks whole package directory to `public/org/package`
* `org/package2` - symlinks file `js/calendar.js` to `public/org/package2/calendar.js`
* `org/package3`
	* `static/plugin.js` - symlinks file to `public/org/package3/plugin.js`
	* `static/plugin.css` - symlinks file to `public/org/package3/plugin.css`
	* `static/icons.png` - symlinks file to `public/org/package3/icons.png`
* `ckeditor/ckeditor` - symlinks files to `admin/wysiwyg`


## Default mapping

Plugin provides default mapping for selected incompatible packages. You can override this mapping in your `composer.json`.

List of packages with default mapping:

* `ckeditor/ckeditor`
* `components/jquery`
* `nette/forms`
* `o5/grido`


## Where find supported packages?

Some libraries and packages support Composer by default. For other exists shim-repositories:

* https://github.com/components
* https://github.com/frontpack

Always you can search packages on [Packagist](https://packagist.org/).

------------------------------

License: [New BSD License](license.md)
<br>Author: Jan Pecha, https://www.janpecha.cz/
