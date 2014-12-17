AngularJS Introduction
====================

*Simple and Elegant MVC Web Applications*

- [AngularJS](http://www.angularjs.org)
- [Developer Guide](https://docs.angularjs.org/guide)
- [API Reference](https://docs.angularjs.org/api)

# Introduction #

[Course Update Site](https://github.com/joeeames/AngularFundamentalsFiles)

Angular is an [*opinionated*](https://gettingreal.37signals.com/ch04_Make_Opinionated_Software.php) MV* framework, such as Knockout or Backbone.

### What is MV*? ###

**M**odel is where you store the data and state of your application

**V**iew is where you render the information to and receive input from the user.

__\*__ is anything else, such as a controller, presenter or ViewModel. Angular uses a controller.

### Features ###

- Handles AJAX communication with server
- Shows data on the page
- Updates the data (model) automatically
- Routes from one view to another (SPAs)
- Extends HTML by providing it's own elements and properties (called directives) to allow you to "teach HTML new tricks".
- [Web Components](http://www.w3.org/TR/components-intro/)
- [Object.observe](http://wiki.ecmascript.org/doku.php?id=harmony:observe)

## Angular Architecture ##

- Two Way Binding
- Dirty Checking
- Dependency Injection (Inversion of Control)

### Angular Components ###

```
+------------------+       +------------------+       +------------------+
|    Services      | <---> |    Controllers   | <---> | Views/Directives |
+------------------+       +------------------+       +------------------+
```

- **Controller** : The central component in an angular application
- **Services**: Communicate  with the server, contain complex business logic

## [Angular Seed](https://github.com/angular/angular-seed) ##

A zip file that acts as a starting point for an angular application.  It contains basic organization, and is geared towards a small project. It also contains a simple node server.

Download and extract the zip, then run `npm install`, `npm test` and `npm start`.

# Controllers & Markup #

Go to the [course github site](https://github.com/joeeames/AngularFundamentalsFiles) and download and extract the DemoApp.zip file. Update the version of Angular used in that app, if you'd like.

## Controllers and Scope ##

The controllers responsibility is to create a scope object. The scope object is how we communicate with the view. The scope is used to expose the model to the view - however, the scope **is not** the model. The model is the data we put into the scope.

```
+------------------+       +------------------+       +------------------+
|   Controllers    | <---> |      Scope       | <---> |       View       |
+------------------+       +------------------+       +------------------+
```

The scope is what ties our controller to our view, but the scope is NOT the model.

## Markup & Binding ##

**EventDetails.html**

```html
<!doctype html>
<html lang="en" ng-app="eventsApp">
    <head>
        <meta charset="utf-8">
        <title>Event Registration</title>
        <link rel="stylesheet" href="/css/bootstrap.min.css">
        <link rel="stylesheet" href="/css/app.css">
    </head>
    <body>

        <div class="container">

            <div class="navbar">
                <div class="navbar-inner">
                    <ul class="nav">

                    </ul>
                </div>
            </div>

            <div ng-controller="EventController">
                <img ng-src="{{event.imgUrl}}" alt="{{event.name}}" style="width: 200px;">
                <div class="row">
                    <div class="spann11">
                        <h2>{{event.name}}</h2>
                    </div>
                </div>
                <div class="row">
                    <div class="span3">
                        <div><strong>Date: </strong>{{event.date}}</div>
                        <div><strong>Time: </strong>{{event.time}}</div>
                    </div>
                    <div class="span4">
                        <strong>Address:</strong><br>
                        {{event.location.address}}<br>
                        {{event.location.city}}, {{event.location.province}}
                    </div>
                </div>

                <hr>

                <h3>Sessions</h3>
                <ul class="thumbnails">
                    <li ng-repeat="session in event.sessions">
                        <div class="row session">
                            <div class="span0 well votingWidget">
                                <div class="votingButton" ng-click="upVoteSession(session)">
                                    <i class="icon-chevron-up icon-white"></i>
                                </div>
                                <div class="badge badge-inverse">
                                    <div>{{session.upVoteCount}}</div>
                                </div>
                                <div class="votingButton" ng-click="downVoteSession(session)">
                                    <i class="icon-chevron-down icon-white"></i>
                                </div>
                            </div>

                            <div class="well span9">
                                <h4>{{session.name}}</h4>
                                <h6 style="margin-top: -10px">{{session.creatorName}}</h6>
                                <span>Duration: {{session.duration}}</span><br>
                                <span>Level: {{session.level}}</span><br><br>

                                <p>{{session.abstract}}</p>
                            </div>
                        </div>
                    </li>
                </div>
            </div>
        </div>

        <script src="/lib/jquery.min.js"></script>
        <script src="/lib/underscore-1.4.4.min.js"></script>
        <script src="/lib/bootstrap.min.js"></script>
        <script src="/lib/angular/angular.js"></script>
        <script src="/js/app.js"></script>
        <script src="/js/controllers/EventController.js"></script>
    </body>
</html>
```

**EventController.js**

```javascript
'use strict';

eventsApp.controller('EventController', function ($scope)
{
    $scope.event =
    {
        name : 'Angular Boot Camp',
        date : '1/1/2015',
        time : '10:30 am',
        location :
        {
            address  : 'Google Headquarters',
            city     : 'Mountain View',
            province : 'CA'
        },
        imgUrl : '/img/angularjs-logo.png',
        sessions :
        [
            {
                name        : 'Directives Masterclass',
                creatorName : 'Bob Smith',
                duration    : '1 hr',
                level       : 'Advanced',
                abstract    : 'In this session you will learn the ins and outs of directives!',
                upVoteCount : 0
            },
            {
                name        : 'Scopes For Fun And Profit',
                creatorName : 'John Doe',
                duration    : '30 min',
                level       : 'Introductory',
                abstract    : 'This session will take a closer look at scopes. Learn what they do, how they do it, and how to get them to do it for you!',
                upVoteCount : 0
            },
            {
                name        : 'Well Behaved Controllers',
                creatorName : 'Jane Doe',
                duration    : '2 hrs',
                level       : 'Intermediate',
                abstract    : 'Controllers are the beginning of everything Angular does. Learn how to craft controllers that will win the respect of your friends and neighbors.',
                upVoteCount : 0
            }
        ]
    };

    $scope.upVoteSession = function (session)
    {
        session.upVoteCount++;
    };

    $scope.downVoteSession = function (session)
    {
        session.upVoteCount--;
    };
});
```

### Built-In Directives ###

Directives are a way to give HTML new functionality. 

**Four Way to Specify Directives**
1. As a tag itself `<ng-form />`
2. As an attribute of a tag `<div ng-form>`
3. As a class `<div class="ng-form">`
4. Inside an HTML comment (no code samples for this)

Not all directives can be written in all four ways. Check the documentation for the directive you want to use.

**Event Directives**
1. `ngClick`
2. `ngDblClick`
3. `ngMousedown`
4. `ngMouseenter`
5. `ngMouseleave`
6. `ngMousemove`
7. `ngMouseover`
8. `ngMouseup`
9. `ngChange` (requires `ngModel`)

**Other Directives**
1. `ngApp`
2. `ngBind` : Single bindings
3. `ngBindTemplate` : Multiple bindings
4. `ngBindHtml`
5. ~~`ngBindHtmlUnsafe`~~
6. `ngHide`
7. `ngShow`
8. `ngCloak` : Avoid a flash of unbound html
9. `ngStyle`
10. `ngClass`
11. `ngClassEven`
12. `ngClassOdd`
13. `ngDisabled` : add or remove the attributed based on the true/false of the value assigned
14. `ngChecked`
14. `ngMultiple`
14. `ngReadonly`
14. `ngSelected`
15. `ngForm` : nest forms
16. `ngSubmit`
14. `ngHref` : bind the href property on an anchor tag
14. `ngSrc` : bind the src property on a image tag
14. `ngNonBindable` : don't parse this

**HTML Injection Workaround**

Given that the `ngBindHtmlUnsafe` directive has been deprecated, here is how you inject HTML using Angular (via [StackOverflow](http://stackoverflow.com/questions/19415394/with-ng-bind-html-unsafe-removed-how-do-i-inject-html)) and [`$sce`](https://docs.angularjs.org/api/ng/service/$sce).

```javascript
angular.module('myApp').filter('to_trusted', ['$sce', function($sce)
{
	return function(text)
	{
		return $sce.trustAsHtml(text);
	};
}]);
```

Usage:

```javascript
<div ng-bind-html="data.html | to_trusted"></div>
```

**IE Restrictions**

If your site needs to support older versions of IE, you will need to do two things to make Angular work.

1. Polyfill JSON.stringify
2. Avoid using custom tag name directives

**Expressions**

Expressions are code snippets in Angular that support a subset of JavaScript.

```javascript
{{ 3 * 10 }

{{ 'hello' + 'world' }}

{{ [1,2,3][2] }}
```

## Filters ##

Filters are a way to tell Angular that you want to modify something for output.

1. Formatting
2. Sorting Datasets
3. Filtering Datasets

```javascript
{{ expression | filter }}
```

### Built In filters ###

1. `uppercase`
2. `lowercase`
3. `number:[decimal]`
4. `currency`
5. `date:'[medium|mediumDate]'`
6. `json` (mostly for debugging)
7. `orderBy`
8. `limitTo`
9. `filter`

### Two Way Binding ###

## Validation ##









# Services #


# Routing #


# Directives #


# Testing #
