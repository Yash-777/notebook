Testing and Prototyping JavaScript Apps
=======================================

> For debugging, use [node-inspector](https://github.com/node-inspector/node-inspector)!

# Principles of Unit Testing #

## What Are Unit Tests ##

- Unit: Small sections of code
- Test: Verify functionality, increase confidence and reduce risk

### Example Unit Test ##

**./js/calculator.js**

```javascript
var Calculator = (function ()
{
	function Calculator (){};

	Calculator.prototype.add = function (operand1, operand2)
	{
		return operand1 + operand2;
	};

	return Calculator;
})();
```

**./test/calculator-tests.js**

```javascript
//Mocha

describe("Calculator", function () // Define a test suite
{
	var calculator;

	beforeEach (function () // Setup necessary state
	{
		calculator = new Calculator();
	});

	describe("Add", function () // Describe the specification
	{
		it("should return the sum of both operands", function () // Define what it should do
		{
			expect(calculator.add(1,1)).to.be(2); // Expect the desired outcome
		});
	});
});
```

**./index.html**

```html
<!doctype html>
<html>
	<head>
		<meta charset="utf-8">
		<title>Calculator Unit Tests</title>
		<link rel="stylesheet" href="./css/vendor/mocha.css">
	</head>

	<body>
		<div id="mocha"></div>
		<script src= "./js/vendor/expect.js" />
		<script src= "./js/vendor/mocha.js" />
		<script src= "./js/calculator.js" />
		<script>
			mocha.setup({ ui : "bdd", bail : false });
		</script>
		<script src= "./js/test/calculator-tests.js" />
		<script>
			mocha.ignoreLeaks();
			mocha.run();
		</script>
	</body>
</html>
```

You run the Mocha tests by opening the web page in a browser window.

### Test Driven Development ###

When adding new features to the above code, start by adding the test case.

**./test/calculator-tests.js**
```javascript
describe("Calculator", function ()
{
	...
	describe("Subtract", function ()
	{
		it("should return the difference of both operands", function ()
		{
			expect(calculator.subtract(4, 2)).to.be.(2);
		});
	});
});
```

Now when you load the web page, you shoud see the test fail. The next thing you want to do is add the functionality.

**./js/calculator.js**

```javascript
Calculator.prototype.subtract = function (operand1, operand2)
{
	return operand1 + operand2; // using a plus sign to trigger a different failure
};
```

Loading the web page this time will show a different error than before. Now let's edit the code to work.

**./js/calculator.js**

```javascript
Calculator.prototype.subtract = function (operand1, operand2)
{
	return operand1 - operand2;
};
```

And finally we see it pass.

## Unit Testing Concepts ##

- **Predictable**: Given the same input, you'll get the same output. Passing a random or frequently changing value (like today's date) would be harder to predict, so try to stick with static data.

- **Pass or Fail**: Each test should either pass or fail, and it should be easy to determine which one happened.

- **Self Documenting**: Each test should describe what it intends to do.

- **Single Responsibility**: They should only test one thing at a time. If you want to test other things, you should include them in different tests.

- **Useful Error Messages**: The error message should make it easy to see what went wrong.

- **Not Integration Tests**: Unit tests should not be testing multiple components at once, but rather only one component at a time.

## Testing Tools ##

- [**Mocha**](https://mochajs.org/): the test runner. Could also use [Jasmine](http://jasmine.github.io/).
- [**Sinon.JS**](http://sinonjs.org/): Test spies, stubs and mocks
- [**Mockjax**](https://github.com/jakerella/jquery-mockjax): Easily mock AJAX requests when using jQuery
- [**AmplifyJS**](http://amplifyjs.com/): Request support for mocking AJAX
- [**mockJSON**](https://github.com/mennovanslooten/mockJSON): A plugin for jQuery that can hijack JSON and JSONP requests and respond with randomly generated JSON data.
- [**Grunt**](http://gruntjs.com/): The JavaScript task runner, to automatically run our unit tests

>You can also integrate node-inspector into Grunt using [grunt-node-inspector](https://www.npmjs.com/package/grunt-node-inspector).

# Examples of Hard to Test Code #

- Try not to tightly couple your components
- Be aware that anything you make private will be unavailable to test
- Limit the use of singletons, otherwise you'll need to reset them
- Be careful of using too may anonymous functions
- Try not to mix various non-related concerns in your code
- Be aware of the new operator when the constructor does the work for you

## Tightly Coupled Components ##

Two or more components have a direct reference to each other. This makes it difficult if you need to test one apart from the other. This also make the code more brittle, as changes in one could potentially break another.

In the following snippet, we have a `polls` object. Both the `submit` and the `view` object reference `polls` and call it's methods. If a method name or parameters change, this will cause the other code to break.

```javascript
var polls =
{
	add     : function (poll) { /*...*/},
	getList : function (callback) { /*...*/}
};

var submit =
{
	add : function (p)
	{
		polls.add(p);
	}
};

var view =
{
	init   : function ()
	{
		polls.getList(this.render);
	},

	render : function (list)
	{
		list.foreach(/*...*/);
	}
};
```

We can resolve this by passing the polls object into the submit and view object - manually injecting the dependency. Alternatively, we could have the components communicate to each other using a message bus.

```javascript
...
var pollsBridge =
{
	add     : polls.add,
	getList : polls.getList
};

var  submit =
{
	init : function (polls)
	{
		this.polls = polls;
	},

	add : function (p)
	{
		this.polls.add(p);
	}
}

var view =
{
	init   : function (polls)
	{
		this.polls = polls;
		polls.getList(this.render);
	},

	render : function (list)
	{
		list.foreach(/*...*/);
	}
};

submit.init(pollsBridge);
...
```

## Private Parts ##

Encapsulation and data hiding are great things, but they make it harder to test things. This might be acceptable, it just depends on you.

### Revealing Module Pattern ###

This pattern creates variables and method private to the closure. In the code below, `eat` is a public method, and `chew` and `swallow` are both private methods, but `eat` can still call `chew` and `swallow`.

```javascript
var person = (function ()
{
	var chew = function ()
	{
		console.log("chew");
	};

	var swallow = function ()
	{
		console.log("swallow");
	};

	var eat = function ()
	{
		for (var i = 0, l = 10; i < l; i += 1)
		{
			chew();
		}

		swallow(); 
	};

	return { eat : eat };
})();
```

But how do we unit test the private methods? We could only do that by exposing them.

```javascript
...
	return { eat : eat, chew : chew, swallow : swallow };
...
```

## Singletons ##

This Singleton pattern is a well known and popular [GoF design pattern](https://en.wikipedia.org/wiki/Design_Patterns). You can only have one instance of an object. The usually requires you to reset the singletons state for each test. That's okay, it's just extra work that you can't forget.

```javascript
// This is the singleton, and would have to be reset for every unit test.
var data =
{
	token : null,
	users : []
};

function setToken (username, password)
{
	data.token = username + password;
}

function addUser (user)
{
	if (data.token)
	{
		data.users.push(user);
	}
}
```

You could include a reset method in the data object, or wrap the methods in an object and then inject the singleton into the object, like we did in the previous solution.

## Anonymous Functions ##

Anonymous functions are convenient and common, but it's hard to test the callback in isolation since there is no handle to reference the function. Making AJAX calls is a good example, because there isn't a way to easily test the `success` callback without actually making the AJAX call.

```javascript
$.ajax(
{
	url : "/people",
	success : function (data)
	{
		var $list = $("#list");
		$.each(data.people, function (index, person)
		{
			$("<li/>", {text : person.fullName}).appendTo($list);
		});
	}
});
```

The easiest way to resolve this is to make the success callback a reference to an available function.

```javascript
function render (data)
{
	var $list = $("#list");
	$.each(data.people, function (index, person)
	{
		$("<li/>", {text : person.fullName}).appendTo($list);
	});
}

$.ajax(
{
	url : "/people",
	success : render
});
```

## Mixed Concerns ##

Be wary of code that mixes different concerns, especially if the code doesn't go together. Testing this kind of code requires that you know about the implementation details. This code, for example, shouldn't update our model and update our DOM at the same time.

```javascript
var people =
{
	list : [],
	add  : function (person)
	{
		this.list.push(person);
		$("#numberOfPeople").html(this.list.length);
	}
};
```

Those can be split out into two different functions, and a message bus, such as jQuery custom events, can be used to trigger DOM changes.

## New Operator ##

Using the `new` operator in your application code can make it difficult to test. Try injecting dependencies or provide enough information to create the dependency.

In this code snippett, getting the current date makes it hard to unit test because it's not predictable.

```javascript
...
nextBirthday : function ()
{
	var now = new Date();
	var next = Number.MAX_VALUE;
	var person;

	$.each(this.list, function (idx, item)
	{
		/*...*/
	});

	return person;
}
...
```

This is easy to fix by allowing the function to take a date as a string to use in it's calculation.

```javascript
...
nextBirthday : function (date)
{
	var now  = (date) ? new Date(date) : new Date();
	var next = Number.MAX_VALUE;
	var person;

	$.each(this.list, function (idx, item)
	{
		/*...*/
	});

	return person;
}
...
```

# Unit Test Your Front-End App #

## [Mocha](https://mochajs.org/) ##

- Cross browser and Node Support
- Simple Async Support
- Assertion Library of Choice
- Easy CI Integration
- Higlights Slow Tests
- Hooks
- Global Variable Leak Detection

### Mocha Hooks ###

Hooks are used to setup or tear down a specific environment.

```javascript
before(function () { /* Runs once before the suite */ });

beforeEach(function () { /* Runs before each test */ });

afterEach(function () { /* Runs after each test */ });

after(function () { /* Runs once after the suite */ });
```

### [Expect](https://github.com/Automattic/expect.js) Assertion Library ###

Minimalistic BDD assertion toolkit (based on [should.js](https://github.com/shouldjs/should.js)) with browser and Node.js support.

```javascript
expect(variable).to.be(value);
expect(variable).to.eql(value);
expect(variable).to.be.a(type);
expect(variable).to.be.an(type);
expect(variable).not.to.be.an(type);
```

### Mocha Setup ###

You can change how Mocha is configured using the `.setup` method.

```javascript
mocha.setup(
{
	ui          : "bdd", //"tdd", "exports"
	reporter    : "mocha.reporters.Dot", // for terminal
	globals     : ["jQuery"], // array of acceptable globals
	timeout     : 3000, // how long in ms to wait before assuming test has failed
	bail        : false, // bail on the first test failure
	slow        : 2000, // ms to wait before test is slow
	ignoreLeaks : true, // ignore global variable leaks
	grep        : "" // string or regexp to filter tests with
});
```

### Pending Tests ###

Specification that haven't been flushed out yet.

```javascript
it("should return NaN if passed 0 arguments");
it("should return NaN if passed 1 argument");
```

## Simple Tests ##

### Exclusive Tests ###



## Asynchronous Tests ##



## Getting Started with Grunt ##



## Running Tests Automatically ##



## Visual Studio Test Runner ##



# Mocking Your Front-End App #

# Prototyping Your Front-End App #

# Integration with Front-End Frameworks #
