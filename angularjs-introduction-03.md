AngularJS Introduction : Part Three
===================================

### [Prev: Controllers & Markup](https://github.com/scottoffen/ps-notes/blob/master/angularjs-introduction-02.md) ###

# Services #

A service is a worker object that performs some sort of (typically reusable) business logic. While it is not accessed "over the wire" (i.e. a web service), it may be used to perform operations that do, such as make AJAX calls.  Services are generally stateless, although it is not unusually for services to cache data that is accessed frequently.

Services facilitate reusablility and ease of maintenance. You don't want to put all of your logic inside of a controller as that would be difficult to maintain and would violate the [single responsibility principle](https://github.com/scottoffen/ps-notes/blob/master/solid-introduction.md#single-responsibility-principle) (SRP).

### [Next: Routing](https://github.com/scottoffen/ps-notes/blob/master/angularjs-introduction-04.md) ###
