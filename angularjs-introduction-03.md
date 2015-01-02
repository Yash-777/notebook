AngularJS Introduction : Part Three
===================================

### [Prev: Controllers & Markup](https://github.com/scottoffen/ps-notes/blob/master/angularjs-introduction-02.md) ###

# Services #

A service is a worker object that performs some sort of (typically reusable) business logic. While it is not accessed "over the wire" (i.e. a web service), it may be used to perform operations that do, such as make AJAX calls.  Services are generally stateless, although it is not unusual for services to cache data that is accessed frequently.

Services facilitate reusablility and ease of maintenance. You don't want to put all of your logic inside of a controller as that would be difficult to maintain and would violate the [single responsibility principle](https://github.com/scottoffen/ps-notes/blob/master/solid-introduction.md#single-responsibility-principle) (SRP).

Services can be injected into your controllers - and other services - that need them. This also makes your code more testable, because you can inject your services in as you need them. Angular is unaware of your services until you register them, but once register, it can easily be injected as needed.

>The ubiquitous `$scope` object is an example of a service, and a built-in one at that!

## Creating Services ##

Services are created by calling the `factory` method on the module you want that service to be available in, then it can be injected by referencing it.

### EventData.js ###

```javascript
eventsApp.factory('eventData', function ()
{
    var service =
    {
        event :
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
                    duration    : 1,
                    level       : 'Advanced',
                    abstract    : 'In this session you will learn the ins and outs of directives!',
                    upVoteCount : 0
                },
                {
                    name        : 'Scopes For Fun And Profit',
                    creatorName : 'John Doe',
                    duration    : 2,
                    level       : 'Introductory',
                    abstract    : 'This session will take a closer look at scopes. Learn what they do, how they do it, and how to get them to do it for you!',
                    upVoteCount : 0
                },
                {
                    name        : 'Well Behaved Controllers',
                    creatorName : 'Jane Doe',
                    duration    : 4,
                    level       : 'Intermediate',
                    abstract    : 'Controllers are the beginning of everything Angular does. Learn how to craft controllers that will win the respect of your friends and neighbors.',
                    upVoteCount : 0
                }
            ]
        }
    };

    return service;
});
```

### EventController.js ###

```javascript
'use strict';

eventsApp.controller('EventController', function ($scope, eventData)
{
    $scope.snippet = '<span style="color: red;">hi there</span>';
    $scope.boolValue = true;
    $scope.mystyle = {color:'red'};
    $scope.myclass = 'blue';
    $scope.event = eventData.event;

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

>Notice that only built-in services begin with the `$`. Since `eventData` is a custom service, we omit the `$`. 

## Commonly Used Built-In Services ###

In addition to building your own services, Angular has many built-in services to choose from.

### $http and $q ###

The [`$http`](https://docs.angularjs.org/api/ng/service/$http) service is a core Angular service that facilitates communication with the remote HTTP servers via the browser's XMLHttpRequest object or via JSONP. The service takes a single argument — a configuration object — that is used to generate an HTTP request and returns a [promise](https://promisesaplus.com/) with two `$http` specific methods: success and error.

The `$http` API is based on the deferred/promise APIs exposed by the [`$q`](https://docs.angularjs.org/api/ng/service/$q) service. While for simple usage patterns this doesn't matter much, for advanced usage it is important to familiarize yourself with these APIs and the guarantees they provide.


**EventData.js**

```javascript
eventsApp.factory('eventData', function ($http, $q)
{
    var service =
    {
        getEvent : function ()
        {
            var deferred = $q.defer();

            $http({method: 'GET', url: '/data/event/1'})
            .success(function (data, status, headers, config)
            {
                deferred.resolve(data);
            })
            .error(function (data, status, headers, config)
            {
                deferred.reject(status);
            });

            return deferred.promise;
        }
    };

    return service;
});
```

**EventController.js**

```javascript
...
    $scope.event = eventData.getEvent().then(
        function (event)
        {
            $scope.event = event;
        },
        function (statusCode)
        {
            console.log(statusCode);
        }
    );
...
```

### $resource ###

[`$resource`](https://docs.angularjs.org/api/ngResource/service/$resource) is a factory which creates a resource object that lets you interact with RESTful server-side data sources. The returned resource object has action methods which provide high-level behaviors without the need to interact with the low level `$http` service.

**app.js**

```javascript
var eventsApp = angular.module('eventsApp', ['ngSanitize', 'ngResource']);
```

**EventDetails.html & NewEvent.html**

```html
<script src="/lib/angular/angular-sanitize.js"></script>
<script src="/lib/angular/angular-resource.js"></script>
```

**EventData.js**

```javascript
eventsApp.factory('eventData', function ($resource)
{
	var resource = $resource('/data/event/:id', {id:'@id'});

    var service =
    {
        getEvent : function (eventId)
        {
            return resource.get({id:eventId});
        },

		save : function (event)
		{
			return resource.save(event);
		}
    };

    return service;
});
```

**EventController.js**

Without a promise, you can bind the data directly to the property.

```javascript
	$scope.event = eventData.getEvent(1);
```

Or, you can use the `$promise` *property* of the resource returned.

```javascript
	eventData.getEvent(1)
	.$promise.then(
		function (event)
		{
			console.log(event);
			$scope.event = event;
		},
		function (response)
		{
			console.log(response);
		}
	);
```

**EditEventController.js**

```javascript
eventsApp.controller('EditEventController', function EditEventController ($scope, eventData)
{
    $scope.saveEvent = function (event, newEventForm)
    {
        if (newEventForm.$valid)
        {
            eventData.save(event)
                .$promise.then(
                function (response)
                {
                    console.log('success', response);
                },
                function (response)
                {
                    console.log('failure', response);
                }
            );
        }
    };

    $scope.cancelEdit = function ()
    {
        window.location = "/EventDetails.html";
    };
});
```

**Actions**

`$resource` comes with several actions you can use: `get`, `save`,  `query` (same as get but expects array not object), `remove` and `delete`.  You can also create custom actions. See the documentation for more information on that. 

### $anchorScroll ###



### $cacheFactory ###



### $compile ###



### $parse ###



### $locale ###



### $timeout ###



### $exceptionHandler ###



### $filter ###



### $cookieStore ###



## Less Commonly Used Built-In Services ##

### $interpolate ###



### $log ###



### $rootScope ###



### $window ###



### $document ###



### $rootElement ###


## Other Built-In Services ##

- `$route`, `$routeParams` and `$location` are discussed in the section on [Routing](https://github.com/scottoffen/ps-notes/blob/master/angularjs-introduction-04.md)
- `$httpBackend` and `$controller` are discussed in the section on [Testing](https://github.com/scottoffen/ps-notes/blob/master/angularjs-introduction-06.md)

### [Next: Routing](https://github.com/scottoffen/ps-notes/blob/master/angularjs-introduction-04.md) ###
