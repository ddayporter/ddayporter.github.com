---
layout: post
title: "Jasmine: A Tutorial for Beginners"
description: "A tutorial for past me and other newbs in web development"
category: tutorial
tags: [jasmine]
---
{% include JB/setup %}

_As a current student at General Assembly, I'm starting this blog to help synthesize what I'm learning in class. Hopefully this will be just the first of many posts from me on this and other web development topics. Please feel free to leave comments with suggestions or corrections!_

## Getting Started with Test Driven Development (TDD)

Testing is good practice on small projects but becomes essential for large projects and when working on teams (which is all the time, basically). Some quick bullets on why you should test:

* **Bug detection** - Your tests will find bugs you would never anticipate

* **Code quality** - Writing code that makes the tests pass keeps things clean and efficient

* **Documentation** - Good tests read like a book and help other developers looking at your code (and future you, because you'll forget) figure out what the heck you were trying to do.

* **Ultimately, Time** - I say "ultimately" time, because for a new developer, writing tests seems to take more time than they're worth. _Can't I just test it myself once I've got some working code?_

  No. The point of writing tests first is to keep you from wasting time on code you don't need. Just write the code to make the tests pass and you'll end up saving time in the end.

### Jasmine, So Hot Right Now

There are many testing frameworks but this post will focus on Jasmine testing for JavaScript, because it's popular, so you'll most likely be using it on a team you may join, and also because it's preferred by AngularJS - we'll come back to testing in Angular in a future post.

### Setting up

Have a project you're about to start? Feel free to follow along with your own idea. If not, I've developed a simple app just for test purposes.

Open up your terminal (I use iTerm because it's more fun, maybe a blog post coming soon on that), and create your project folder and change directories into it.

    $ mkdir jasmine-resolutions
    $ cd jasmine-resolutions

Now we need to install Jasmine - you can actually run the following code anywhere because it's being installed globally but we're already in our project folder so that's where we're running it.

    $ npm install -g jasmine-node

_Having trouble with npm? This tutorial assumes you've got Node.js installed on your machine. If not, head over to [Node.js](https://nodejs.org/en/) and then check out [this page](http://www.sitepoint.com/beginners-guide-node-package-manager/) for help getting started_

You might have guessed the app we're creating has something to do with resolutions. It's New Year's Eve so it seemed appropriate to create an app that tracks New Years Resolutions, everyone's favorite thing to hate.

Before we write any code though, we're going to write tests. Start by creating a spec directory in your project folder

    $ mkdir spec


and inside that folder, create a spec file for your resolution model.

    $ touch spec/resolution-spec.js


Now for our first win. Run this:

    $ jasmine-node spec


and you should see output something like this:

    $ jasmine-node spec


    Finished in 0.001 seconds
    0 tests, 0 assertions, 0 failures, 0 skipped


Hooray! Something happened. So now that we're set up, let's get to the tests. What should our resolutions model do? In your ```resolution-spec.js``` file, write out some specifications in plain English. If you've got your own app you're working on, come up with some basic functionality for the model you're building out.

    // Each resolution should have a title that summarizes the goal
    // There should be a description of the goal in more detail
    // A resolution should have a deadline

### Describe your model

Now that we've focused our idea, let's put it in terms Jasmine can understand. The main component of a Jasmine test suite is a ```describe``` function, which takes a string and a function as its arguments. The string is a description of what is being tested.

    describe("a resolution", function(){

    });


This by itself won't be recognized as a test. If you run ```$ jasmine-node spec --verbose``` you will find that it prints out "a resolution" and then notes 0 tests. To be a test, you need to add an ```it``` function, which also takes a string and a function as arguments. Replace your plain English statements with ```it``` statements as below:

    describe("a resolution", function(){

      it("should have a title", function(){

      });

      it("should have a description", function(){

      });

      it("should have a deadline", function(){

      });

    });


Now run jasmine again and you'll get this:

    a resolution - 3 ms
      should have a title - 3 ms
      should have a description - 1 ms
      should have a deadline - 0 ms

    Finished in 0.008 seconds
    1 test, 0 assertions, 0 failures, 0 skipped


It recognizes the tests, but since you have set no expectations, there's nothing to fail.

### Ready, set, expect
**Expectations**

From the [Jasmine docs](http://jasmine.github.io/2.0/introduction.html):

    "An expectation in Jasmine is an assertion that is either true or false. A spec with all true expectations is a passing spec. A spec with one or more false expectations is a failing spec."

Basically, we want to write our tests in a way that will evaluate as true only if the code is working properly.

**Matchers**

    "Expectations are built with the function expect which takes a value, called the actual. It is chained with a Matcher function, which takes the expected value."

You can find the complete list of matcher functions in the [Jasmine docs](http://jasmine.github.io/2.0/introduction.html), but here are a few examples:

    'toBe' compares with ===
    'toBeDefined' compares against `undefined`
    'toContain' is for finding an item in an array
    'toMatch' is for regular expressions (regex)

### Pulling it all together

Let's complete a test by adding our first expectation.

    describe("a resolution", function(){

      it("should have a title", function(){
        var learnJasmine = new Resolution({title: "Learn Jasmine"});
        expect( learnJasmine.title ).toBeDefined();
      });

      it("should have a description", function(){

      });

      it("should have a deadline", function(){

      });

    });

Run jasmine again and what do you think will happen?

    a resolution - 6 ms
        *should have a title - 5 ms*
        should have a description - 0 ms
        should have a deadline - 0 ms

    Failures:

      1) a resolution should have a title
        Message:
          ReferenceError: Resolution is not defined
        Stacktrace:
          ReferenceError: Resolution is not defined
          at null.<anonymous> (/Users/rachel/blog/tutorials/jasmine-resolutions/spec/resolution-spec.js:6:28)

    Finished in 0.012 seconds
    3 tests, 1 assertion, 1 failure, 0 skipped

Oh no! 1 failure! But this is a good thing. We know we want our resolution to have a title and now this error message has told us exactly what we need to do: define Resolution.

In your main project folder, add the new file ```resolution.js```, and then in your spec file, let it know this file exists by requiring it:

    // in /spec/resolution-spec.js

    var Resolution = require("../resolution");

Then define Resolution:

    // in /resolution.js

    function Resolution(info) {
      this.title = info.title;
    }

    module.exports = Resolution;

Make sure both files are saved and then run jasmine again. What do you get?

    a resolution - 4 ms
      should have a title - 4 ms
      should have a description - 0 ms
      should have a deadline - 0 ms

    Finished in 0.01 seconds
    3 tests, 1 assertion, 0 failures, 0 skipped

High five! You've got one working test. Can you make the second test pass? Did it feel a little redundant typing out the new Resolution code again?

### DRY It Up

You can keep your code DRY by using a ```beforeEach``` function inside your ```describe``` block, before your ```it``` statements.

    beforeEach(function(){
      learnJasmine = new Resolution({
        title: "Learn Jasmine",
        description: "Read through Jasmine docs, find more tutorials, and practice with next project",
        deadline: "January 31, 2016"
      });
    });


### Fun with Regex

Let's get a little fancy with the third test - doesn't it make sense to have a deadline in date format? How would you write a test to validate that?

In JavaScript, the Date constructor can take in a variety of date formats and spit out a long string of the full date and time. Try it out by running node in your terminal window.

    $ node
    var date = new Date("3/24/2015")
    date
    Tue Mar 24 2015 20:00:00 GMT-0400 (EDT)

By tacking on ```.toDateString()``` you can get a shorter output that will always follow the pattern of "Tue Mar 24 2015".

Let's write some regex in our test to make sure our dates always match this format (I'll be back with more on regex soon, but in the meantime you can check out [this resource](http://www.regular-expressions.info/) to learn more).

    describe("a resolution", function(){
      var learnJasmine;
      var isDateString = new RegExp("[A-Z][a-z]{2}\\s[A-Z][a-z]{2}\\s[0-9]{2}\\s[0-9]{4}")

      beforeEach(function(){
        learnJasmine = new Resolution({
          title: "Learn Jasmine",
          description: "Read through Jasmine docs and practice with next project",
          deadline: "January 6, 2016"
        });
      });

      it("should have a title", function(){
        expect( learnJasmine.title ).toBeDefined();
      });

      it("should have a description", function(){
        expect( learnJasmine.description ).toBeDefined();
      });

      it("should have a deadline in Date String format", function(){
        expect( learnJasmine.deadline.match(isDateString)).toBeTruthy();
      });

    });

If you run jasmine now your 3rd test will fail.

    a resolution - 30 ms
        should have a title - 4 ms
        should have a description - 1 ms
        *should have a deadline in Date String format - 24 ms*

    Failures:

    1) a resolution should have a deadline in Date String format
      Message:
        TypeError: Cannot read property 'match' of undefined
      Stacktrace:
        TypeError: Cannot read property 'match' of undefined
      at null.<anonymous> (/Users/rachel/blog/tutorials/jasmine-resolutions/spec/resolution-spec.js:25:34)

    Finished in 0.037 seconds
    3 tests, 3 assertions, 1 failure, 0 skipped

However, we know from the error message that we need to define learnJasmine.deadline. So let's go update the ```resolution.js``` file.

    function Resolution(info) {
      this.title = info.title;
      this.description = info.description;
      this.deadline = new Date(info.deadline).toDateString();
    }

    module.exports = Resolution;

Now when you run jasmine, you should have three tests that pass.

### Learn more

This tutorial gives a very basic introduction to testing with Jasmine. Here are some resources for further reading:

* [Jasmine Docs](http://jasmine.github.io/2.0/introduction.html)
* [Getting Started with Node.js and Jasmine](https://semaphoreci.com/community/tutorials/getting-started-with-node-js-and-jasmine)
* [Testing in AngularJS](https://docs.angularjs.org/guide/unit-testing)
