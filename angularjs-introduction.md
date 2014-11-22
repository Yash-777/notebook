Angular.js
==========

angularjs.com

plnkr.co : Launch the Editor

Add the script to the head, then add the `ng-app` application directive to a section (html/body/div) on the page.

```html
<div ng-app>
	This area is controlled by Angular!
</div>
```

BTW: `ng` is short for Angular.

You can only have one `ng-app` directive per page.

Interpolation is done using double curly-braces:

	{{ stuff here }}

## Controllers ##

Controllers are responsible for building a model. A model contains the data we need to work with. Controllers will do whatever is needed to get that data - including service calls. Data binding is used to show the model in a view (html).

### Controller Basics ###

The controller directive in Angular is `ng-controller`. In this example, `MainController` is the name of the controller that will be used.

```html
<div ng-app>
	<div ng-controller="MainController">
		<!-- This is the View -->
		<h1>{{message}}</h1>
	</div>
</div>
```

We create this controller by writing a function that will be invoked by Angular.

```javascript
// This is the Controller
var MainController = function ($scope)
{
	// This is the Model
	$scope.message = "Hello";
};
```

Notice that the name of the function matches the controller name specified in the HTML.  The controller function can take `$scope` as a parameter.

> Much like jQuery, the $ at the beginning of the variable name indicates that it is a component provided by Angular.

The attach the model to `$scope` - but `$scope` is not our model, but things that we attach to `$scope` are going to be the model. We use binding function to attache the model to the view.

### Controller Capabilities ###

- **Multiple Controllers:** In larger applications, you will almost always have multiple controllers.

- **Complex Objects:** Instead of just using `{{message}}`, you can reference more complex objects, like a *stock* object with a *symbol* property: `{{stock.symbol}}`

- **Nested Controllers :** You can nest controllers inside of other controllers.

- **Set HTML Attributes:** You can use data binding to set HTML attributes like `<img src="{{chart.source}}">`

```html
<div ng-app>
	<div ng-controller="MainController">
		<h1>{{message}}</h1>

		<div>
			<div>First Name: {{person.firstName}}</div>
			<div>Last Name: {{person.lastName}}</div>
			<img src="{{person.imgsrc}}">
		</div>
	</div>
</div>
```

```javascript
// This is the Controller
var MainController = function ($scope)
{
	var person =
	{
		firstName : 'Scott',
		lastName  : 'Offen',
		imgsrc    : 'http://images.scottoffen.com/profile.jpg'
	};

	$scope.message = "Hello";
	$scope.person  = person;
};
```

