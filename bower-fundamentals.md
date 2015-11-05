Bower Fundamentals
==================

Bower is a tool that helps you manage third-party client-side libraries for your web-based project via Git.

# Bower Basics #

## Installing Bower ##

1. Install Node
2. Install Git for Windows
	- Run Git from the Windows Command Prompt
3. `npm install bower -g`

## Common Tasks ##

Run all of these inside your project directory. A `bower_components` directory will be created, and the desired libraries will be found in subfolders.

### Installing A Package ###
```
$ bower install [package]
```

### Uninstalling Packages ###

```
$ bower uninstall [package]
```

### Get A List Of Versions For A Package ###

```
$ bower info [package]
```

### Installing Specific Versions ###

```
$ bower uninstall [package]#[version]
```

### Updating Packages ###

```
$ bower update
```

>This will update all packages in your project. To only update a specific package, use `bower install [package]`.

### Listing Installed Packages ###

```
$ bower list
```

### Searching Bower Registry ###

```
$ bower search [search term]
```

> Or go to http://bower.io/search

# Bower Configuration #

All Bower configurations are held in two files.

## [bower.json](http://bower.io/docs/creating-packages/) ##
This file works much the same as Node's **`package.json`**. It keeps track of the packages installed into your project. This makes it easy to reinstall them at any time. It is also used for when you want to publish your own package by give Bower all the information it needs to install your package correctly.

You can create this file for your project easily.

```
$ bower init
```

If you have already installed some components, you may be prompted to bring those in as dependencies.

To add files to your project **and** add them to `bower.json`, the command is the same as for npm.

```
$ bower install [package] --save
$ bower uninstall [package] --save

$ bower install [package] --save-dev
$ bower uninstall [package] --save-dev
```

## .bowerrs ##
The **`.bowerrc`** file lets us customize the install directory of our packages. It should be located as a peer to the `bowers.json` file.

```javascript
{
	"directory" : "relative/path/to/folder"
}
```
