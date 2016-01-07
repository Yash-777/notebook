Unit Testing with Node.js
=========================

*See the [course repo on GitHub](https://github.com/joeeames/UnitTestingNodeCourse) for updates.*

Mocha & Chai
Sinon
Gulp
Grunt

# [Mocha](http://mochajs.org) and [Chai](http://chaijs.com) #

```
$ npm install mocha chai

$ npm install mocha -g
```

## Usage ##

Require in the library, make `expect` easier to use.

```javascript
var chai = require('chai');
var expect = chai.expect;
chai.should();
```

That last line makes it so every JavaScript object now has a new property called [`should`](http://chaijs.com/guide/styles/#should).

### Test Suites ###

Create a test suite with `describe`.

```javascript
describe('describe tests in this suite', function ()
{
	// tests go here
});
```

Test suites can be nested to create multiple levels of grouping.

```javascript
describe('smoke test', function ()
{
	describe('component one', function ()
	{
		// tests go here
	});

	describe('component two', function ()
	{
		// tests go here
	});
});
```

### Individual Test ###

Create a test with `it()`.

```javascript
it('should do something', function ()
{
	// what it should do
});
```

### Assertions ###

Assert using [`should`](http://chaijs.com/guide/styles/#should).

```javascript
it('should statement', function ()
{
	methodUnderTest(4).should.be.true;
});
```

Assert using [`expect()`](http://chaijs.com/guide/styles/#expect).

```javascript
it('should statement', function ()
{
	expect(methodUnderTest(4)).to.be.true;
});
```

### Test Execution ###

When run without any parameters, mocha will look for a `test.js` file and run those tests or a `test` folder and run all files it finds in there.

```
$ mocha
```

Or you can specify which tests to run by passing a space separated list of files to be executed.

```
$ mocha [name of test file or files]
```

You can also tell mocha to run the tests each time they change.

```
$ mocha -w
```

### Setup and Teardown ###

Create methods to setup and tear down your test.

```javascript
beforeEach(function(){...});
afterEach(function(){...});
```

### Specifying Tests To Execute ###

```javascript
it.skip(...); // skips this test
xit.skip(...); // skips this test

describe.skip(...); //skips this suite
xdescribe.skip(...); //skips this suite

describe.only(...); //only runs this suite
it.only(...); // only runs this test
```
