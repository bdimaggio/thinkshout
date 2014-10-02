---
title: Getting started with Behat
layout: post
author: david
published: false
featured: true
short: |
  Learn everything you need to know to start using behat today.
tags:
- behat
- testing
- BDD
- Behavioral testing
date: 2014-10-03 16:00:00
---

## Situation
Suppose you have built a site, it works great, the client loves it, you launch it, and the client still loves it. Yay! Now life goes on, and six months later, the client comes back to you saying they see a red box when they are logged in, with a message about security updates. You look and see that Drupal core, ctools, rules, views, commerce, date, and a handful of other modules have updates availalbe. Some are security updates, and others are bugfix/feature updates.

So you want to updates this code to resolve security issues and improve the functionality of the site. But how can you be sure that these code updates will not hurt or break any of the existing functionality? You could revisit all of your feature work from 6 months to a year ago, and confirm that those features still work as intended. But that can be time consuming and disrupt your other work, just reminding yourself what all that functionality was.

So how do you make updates, whether updating contrib code or doing new custom work, with confidence that you're not breaking essential funcionality, without wasting countless hours doing a bunch of manual testing?

## A better solution: automated testing.

Let a machine do it for you. There are several categories of automated testing:

- Unit testing. Tests that a small piece of code, a function, behaves as expected.
- Integration testing. These combine several Unit tests in logical groups, to ensure that they work together properly.
- System testing. This tests the system as a whole, and is mainly code oriented, but starts to touch how real people would use the system.
- Behavioral testing. Acceptance testing. Customer testing. This involves clickthroughs, user behavior. This is what we are mainly interested in, and what I am talking about today. You will also hear this referred to as BDD or Behavior Driven Development.

## Enter behat

Behat is an automated testing system. Its strength is in behavioral testing, so fits perfectly in our use case.

Behat test are written in plain English phrases, combined into human readable scenarios. (This comes from inspiration by Ruby's Cucumber project and Gherkin syntax.) This is probably the most appealing aspect of Behat. Most tests are understandable by anyone, whether developer, project manager, or business owner.

Behat is the core framework used for running tests. It is capabable of testing several types of systems: terminal commands, REST APIs, etc. To enable behat to test web pages, you need to add Mink and a browser emulator to the mix. Mink functions as the connector between Behat and browser emulators, and provides a consistent testing API.

There are several commonly used browser emulators. Some, like Goutte, are very fast but don't support JavaScript. Others, like Selenium and Firefox, are full-featured browsers but will run more slowly.

So when you hear people talking about behat, they're usually talking about all three components: behat, mink, and browser emulators.

## Why behat versus others?

Mainly becuase of popularity, which comes mainly from its human readability. There are certainly other contenders with their certain strengths, but we're focusing on behat today because it is a popular PHP based testing framework, its tests are written as human readable scenarios, can be easily extended by writing additional PHP methods, and as you'll see soon, getting set up is not too difficult.

## Business use

Even though this all seems like a good thing, it does take some time to write tests, set up a testing environment, and determine what the best tests are. We need to allocate time to do this, and it shouldn't just be a surprise at the end of the project. Automated testing should be considered in several phases of a web project. Whe writing custom code, it is a good practice to write unit tests, and time should be allocated for that. When devleoping custom features for a site, behaviroal tests should be written to accompany them, and again, time should be allocated. It's good if clients know at the beginning of a project that test writing is part of the development process, and test running is part of deployment.

Things that are measured always get more attention than things that just happen. Clients should have a large say in what is measured and tested. As a result, project managers can gain a better insight into priorities of the client and project. By making behavior tests something that is intentionally done, project stakeholders must clarify and prioritize the most important aspects of the site.

## Run tests

Let's use the scenario of ensuring that the user login experience is correct. This will verify that the site is up & running, that valid users can log in, and that invalid credentials will not work. Here's a test run, using a local development site:

![behat test run](/assets/images/blog/behat-test-run.png)

And it only takes a few seconds to run.

If you run this test after a code update and find that the test fails, you know immediately that something must be fixed before it can be deployed to the production environment.

## Write tests

Behat tests are written in "Feature" files. They're just text files with a .feature extension on the name, instead of .txt or .php. They are usually placed in a "features" directory inside your behat directory. More on that in the next section.

In the test run above, I was in my project's behat directory, and ran `bin/behat features/loginout.feature`. That launches behat and tells it to run the tests that are in loginout.feature. Here are the entire contents of that file:

```
Feature: Log in and out of the site.
  In order to maintain an account
    As a site visitor
    I need to log in and out of the site.

Scenario: Logs in to the site
  Given I am on "/"
  When I follow "Log In"
    And I fill in "Username" with "admin"
    And I fill in "Password" with "test"
    And I press "Log in"
  Then I should see "Log out"
    And I should see "My account"

Scenario: Logs out of the site
  Given I am on "/"
  When I follow "Log In"
    And I fill in "Username" with "admin"
    And I fill in "Password" with "test"
    And I press "Log in"
    And I follow "Log out"
  Then I should see "Log in"
    And I should not see "My account"

Scenario: Attempts login with wrong credentials.
  Given I am on "/"
  When I follow "Log In"
    And I fill in "Username" with "badusername"
    And I fill in "Password" with "boguspass"
    And I press "Log in"
  Then I should see "Sorry, unrecognized username or password."
    And I should not see "My account"
```

Indentation is only for readability, and has not impact on how the tests are run.

Now let's look each line and see what each is doing. The first few lines are essentially comments.

```Feature: Log in and out of the site.```

^ Name of the feature.

```In order to maintain an account```

 ^ Benefit.

```As a site visitor```

^ Role.

```I need to log in and out of the site.```

^ Feature itself.

Behat tests are written in the form of scnearios, and they comprise the rest of the feature file.

```Scenario: Logs in to the site```

^ Description of the first scenario.

```Given I am on "/"```

^ The context. This is the first line that is actually executed. In this case, it will load "/" (the home page) in a browser.

This (a "Given") as well as the next things ("When" and "Then") are each called a "Step".

```
When I follow "Log In"
  And I fill in "Username" with "admin"
  And I fill in "Password" with "test"
  And I press "Log in"
```

^ The events that need to happen. `When` kicks it off. `And` adds more events. If behat is unable to do any of these events, the test will fail. `I follow "Log In"` looks for a link with the text "Log In" and clicks it. `I fill in "Username" with "admin"` looks for a field with the label of "Username" and types "admin" into it. `I press "Log in"` looks for button with the text "Log in" and presses it. Pro tip: `follow` is for clicking links, and `press` is for buttons on forms.

```
Then I should see "Log out"
  And I should see "My account"
```

^ The desired outcome. `Then` starts it, and `And` adds more outcomes. These are the actual tests that need to pass. Other testing frameworks often call these "assertions". `I should see "Log out"` looks for the text "Log out" anywhere on the page.

The other two scenarios follow the same format, as well as using `not` to ensure that certain things do not happen.

That's the quick walkthrough of writing scenarios, but you can dig deeper at http://docs.behat.org/en/v2.5/quick_intro.html#define-your-feature and http://docs.behat.org/en/v2.5/guides/1.gherkin.html and find out about other aspects like [Scenario Outlines](http://docs.behat.org/en/v2.5/guides/1.gherkin.html#scenario-outlines), [Backgrounds](http://docs.behat.org/en/v2.5/guides/1.gherkin.html#backgrounds) and Multiline Arguments.

## Get set up

I've looked at several resources from behat.org and elsewhere, and ended up just having to piece things together to get something that will work. I've consolidated those notes to ease the setup in the future. [Behat Installation and Use](https://github.com/thinkshout/ts_recipes/blob/master/behat/README.md).

There are a number of dependencies, so the easiest way to handle them all is to let composer do it for you. So install composer if you haven't already. On a mac, using homebrew works great: `brew install composer`.

Make a behat directory, either for a project you're working on, or in a generic location. Copy this [composer.json](https://github.com/thinkshout/ts_recipes/blob/master/behat/composer.json) file into it. Run `composer install`, which might take a while. It's installing behat, mink, several mink extensions, and webdriver which is for selenium. Then run `bin/behat` to make sure that behat is actually available and doing something. You should see something like `No scenarios`.

Install selenium. This part is optional, if you don't need to test javascript. Download the latest version of [selenium-server-standalone](http://selenium-release.storage.googleapis.com/index.html). You'll also need firefox and a java runtime installed. If you get output from `java -version` you should be good.

In your behat directory, add a features folder if there's not one already, and add a something.feature file to it. You can use this [loginout.feature](https://github.com/thinkshout/ts_recipes/blob/master/behat/features/loginout.feature) as an example.

The last thing you need is a behat.yml file in your behat directory. Use this [behat.yml](https://github.com/thinkshout/ts_recipes/blob/master/behat/behat.yml) as an example, replacing the domain with the site you want to test. Also remove the selenium2 line if you're not using it.

At this point, running `bin/behat` in your behat directory should run any tests located in the features directory.

Hopefully that gets you started on your road to readable automated testing. The best resources I've found are on the [behat site](http://behat.org). You'll probably be redirected to something like http://docs.behat.org/en/v2.5/. Please leave a comment with your successes or other suggestions. Thanks for reading, and good luck!