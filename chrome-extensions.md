Chrome Extensions
=================

A useful way to add functionality to the Chrome browser using HTML, CSS and JavaScript.

## Introduction ##

Using the model Google provides for extending the capabilities of Chrome, we can add custom functionality to Chrome in all kinds of useful way - including changing some of the default behaviors. Extensions are not the same a plugins, which run outside the context of the browser, whereas extensions run within the browser itself and conform to a specific set of API and capabilities exposed by Chrome.

>You can see and manage both extensions and plugins by typing `chrome://extensions` or `chrome://plugins` into the address bar in Chrome.

### How They Work ###

1. Developer Creates HTML, CSS and JavaScript files - just like a regular website
2. Files are packaged with a manifest into zipped file with a .CRX
3. The extension is deployed into the Chrome web store (or another location user can install it from)
4. Users install it, and Chrome uses the manifest file to know how to integrate with the browser

### What Extensions Can Do ###

- Interact with web pages or servers
- Control browser features
- Add UI to Chrome
- Create options pages
- Modify loaded pages
- Add to context menu (right click)
- Even load DLLs (DANGEROUS)

### Access The Chrome APIs ###

The [Google Developers](http://developer.google.com) site has a [page for extensions](https://developer.chrome.com/extensions) with extensive details about the [`chrome.*` API](https://developer.chrome.com/extensions/api_index).

### Anatomy Of A Chrome Extension ###

Chrome extensions can be broken down into three main parts: background pages, UI pages and content scripts.

- **Background Pages**
An HTML page that runs in the background and contains the JavaScript to control the capabilities of the extension when it is active from the Chrome UI. They can be persistent (always running and open) or event (registered for a specific event and only loaded when needed) pages. Background pages can be memory hogs!

- **UI Pages**
HTML pages that expose the UI for the extension, most commonly displayed when you click the extensions icon in the toolbar. Browser actions can have a pop-up to display this code or open a new tab or a window. UI pages can also invoke functions on a background page.

- **Content Scripts**
Content scripts are scripts that are executed when certain pages load in the browser and are used to inject JavaScript into a page to modify its behavior or user interface.

### Extension Manifest ###

At a minimum, the extension will need a [manifest file](https://developer.chrome.com/extensions/manifest); which is just a JSON formatted file that describes your extension. The minimum required fields are `manifest_version`, `name` and `version`.

The following code shows the supported manifest fields for extensions.

```javascript
{
	// Required
	"manifest_version": 2,
	"name": "My Extension",
	"version": "versionString",

	// Recommended
	"default_locale": "en",
	"description": "A plain text description",
	"icons": {...},

	// Pick one (or none), but not both. The content is the same for
	// both a browser action and a page action.
	"browser_action":
	{
		"default_icon":                 // optional
		{
			"19": "images/icon19.png",  // optional
			"38": "images/icon38.png"   // optional
		},
		"default_title": "Google Mail", // optional; shown in tooltip
		"default_popup": "popup.html"   // optional
	},

	"page_action":
	{
		"default_icon":                 // optional
		{
			"19": "images/icon19.png",  // optional
			"38": "images/icon38.png"   // optional
		},
		"default_title": "Google Mail", // optional; shown in tooltip
		"default_popup": "popup.html"   // optional
	},

	// Optional
	"author": ...,
	"automation": ...,
	"background":
	{
		// Recommended
		"persistent": false
	},
	"background_page": ...,
	"chrome_settings_overrides": {...},
	"chrome_ui_overrides":
	{
		"bookmarks_ui":
		{
			"remove_bookmark_shortcut": true,
			"remove_button": true
		}
	},
	"chrome_url_overrides": {...},
	"commands": {...},
	"content_pack": ...,
	"content_scripts": [{...}],
	"content_security_policy": "policyString",
	"converted_from_user_script": ...,
	"current_locale": ...,
	"devtools_page": "devtools.html",
	"externally_connectable":
	{
		"matches": ["*://*.example.com/*"]
	},
	"file_browser_handlers": [...],
	"homepage_url": "http://path/to/homepage",
	"import": ...,
	"incognito": "spanning or split",
	"input_components": ...,
	"key": "publicKey",
	"minimum_chrome_version": "versionString",
	"nacl_modules": [...],
	"oauth2": ...,
	"offline_enabled": true,
	"omnibox":
	{
		"keyword": "aString"
	},
	"optional_permissions": ["tabs"],
	"options_page": "options.html",
	"options_ui":
	{
		"chrome_style": true,
		"page": "options.html"
	},
	"permissions": ["tabs"],
	"platforms": ...,
	"plugins": [...],
	"requirements": {...},
	"sandbox": [...],
	"script_badge": ...,
	"short_name": "Short Name",
	"signature": ...,
	"spellcheck": ...,
	"storage":
	{
		"managed_schema": "schema.json"
	},
	"system_indicator": ...,
	"tts_engine": {...},
	"update_url": "http://path/to/updateInfo.xml",
	"web_accessible_resources": [...]
}
```

## Simple "Hello, World" Extension ##

This is a simple example extension that just shows how the pieces of an extension fit together - it isn't representative of what a real Chrome extension is likely to look like. It doesn't interact with the Chrome APIs or implemening much functionality. It's value lies in understanding the basics before delving into these broader topics.

Since extensions are really just a directory with everything your extension needs in it, create a directory for this example called `HelloWorld`. Inside this directory, create the following files.

**manifest.json**

```javascript
{
	"manifest_version" : 2,

	"name"        : "Hello World",
	"version"     : "1.0",
	"description" : "The description of the extension.",

	"browser_action" :
	{
		"default_icon":
		{
			"19": "img/icon19.png",
			"38": "img/icon38.png"
		},
		"default_title": "Hello World Title",
		"default_popup": "popup.html"
	}
}
```

**popup.html**

We can include one or more HTML files to use for the UI of our extension. These are just regular HTML file with one exception: **you can't use inline JavaScript**. All your JavaScript has to be in external files. Our manifest says an HTML file called `popup.html` will be our entry point, so here it is:

```html
<!doctype html>
<html lang=en>
	<head>
		<meta charset=utf-8>
		<title>Hello, World</title>

		<link rel="stylesheet" href="css/popup.css">

		<script src="js/jquery.min.js"></script>
		<script src="js/popup.js"></script>
	</head>
	<body>

		<h1 id="greeting">Hi!</h1>
		<input id="name" type="text">
	</body>
</html>
```

**popup.js**

Since our HTML file is referencing some external scripts, go ahead and create the `js` folder in your extenstion directory. Download [jQuery](http://jquery.com/download/) there, and then add this content to the `popup.js` file:

```javascript
$(document).ready(function ()
{
	$('#name').keyup(function ()
	{
		$('#greeting').text('Hi, ' + $('#name').val() + '!');
	});
});
```

**popup.css**

Just for the sake of completeness, we're going to add some CSS to our example. While we can do this inside the HTML file (unlike JavaScript), it's cleaner to put that in it's own file.

```css
h1
{
	color: blue;
}
```

**icon19.png and icon38.png**

Use your preferred image editor to create these. They can look however you want, as long as their dimensions are 19x19 and 38x38, respectively. Put these in a subfolder named `img` - which matches the path we specified in our manifest file.

Feel free to use mine, listed here for your convinience:

| File Name  | Icon |
|------------|------|
| icon19.png | ![](https://raw.githubusercontent.com/scottoffen/ps-notes/master/images/icon19.png) |
| icon38.png | ![](https://raw.githubusercontent.com/scottoffen/ps-notes/master/images/icon38.png) |

### Using Developer Mode ###

While developing our extension, we're going to want to test it. Put Chrome in developer mode by going to `chrome://extensions` and checking the box at the top labeled *Developer mode* - which should be unchecked by default.

![](https://raw.githubusercontent.com/scottoffen/ps-notes/master/images/developer-mode.png)

Once checked, a button bar becomes visible. Click the *Load unpacked extension...* button and browse to the folder the extension is located in. The icon should become visible in our browser bar, and when clicked will show our pop-up. As we continue to make changes to our extension files, the developer-mode plugin will automatically refresh for us.

>We can launch the Chrome developer tool on our extension by right-clicking on inside the pop-up and selecting the *Inspect element* option. This will open the developer tools in a seperate window that will be specific to our extension, not the open browser tab.

## Browser Action Extension ##

This extension will use the Chrome storage APIs for storing and syncing data, demonstrate how to use the browser action badge, have an options page, and use and event page that runs in the background to register for events.



## Page Action Extension ##



## Debugging and Deployment ##

