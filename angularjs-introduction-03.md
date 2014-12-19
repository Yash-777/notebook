AngularJS Introduction : Part Three
===================================

### [Prev: Controllers & Markup](https://github.com/scottoffen/ps-notes/blob/master/angularjs-introduction-02.md) ###

# Services #

A service is a worker object that performs some sort of (typically reusable) business logic. While it is not accessed "over the wire" (i.e. a web service), it may be used to perform operations that do, such as make AJAX calls.  Services are generally stateless, although it is not unusually for services to cache data that is accessed frequently.

Services facilitate reusablility and ease of maintenance. You don't want to put all of your logic inside of a controller as that would be difficult to maintain and would violate the [single responsibility principle](https://github.com/scottoffen/ps-notes/blob/master/solid-introduction.md#single-responsibility-principle) (SRP).

Services can be injected into your controllers - and other services - that need them. This also makes your code more testable, because you can inject your services in as you need them. Angular is unaware of your services until you register them, but once register, it can easily be injected as needed.

>The ubiquitous `$scope` object is an example of a service, and a built-in one at that!

## Creating Services ##

Services are created by calling the `factory` method on the module you want that service to be available in, then it can be injected by referencing in.

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

## Commonly Used Built In Services ###

In addition to building your own services, Angular has many built-in services to choose from.

### $http and $q ###



### $resource ###



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

- `$route`, `$routeParams` and `$location` are discussed in the section on Routing
- `$httpBackend` and `$controller` are discussed in the section on Testing.

### [Next: Routing](https://github.com/scottoffen/ps-notes/blob/master/angularjs-introduction-04.md) ###
