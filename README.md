# A Lighthouse Style Guide: PHP

*Its a hard knock life being a PHP developer...*

## Table of Contents

1. [Composer](#1-composer)
2. [Style Guide](#2-style-guide)
3. [Linting](#3-linting)
4. [Testing](#4-testing)

## 1. Composer

[Composer](https://getcomposer.org/) is **the** dependency manager for PHP, what that means is we use Composer to include any PHP dependencies for our projects.

Use the following command to install the `composer` command globally:

`curl -sSL https://getcomposer.org/installer | php && mv composer.phar /usr/local/bin/composer`

Most projects will have a `composer.json` file which contains the list of project dependencies, to install these dependencies on a projects you can run the following command:

`composer install`

**N.B.** if you are using Docker you will need to run the `composer` command from within the Docker container.

### Global dependencies

Like other dependency managers you can install dependencies globally meaning they are installed to your system and not just as a project dependency. This is particually usefull if a dependency has a command or executable that we want to use on the command line. To make these commands accessible on the command line you need to add the global composer `bin` folder to your [PATH](https://ss64.com/nt/path.html).

Adding the following line to your [`.bash_profile`](http://apple.stackexchange.com/questions/51036/what-is-the-difference-between-bash-profile-and-bashrc) or [`.bashrc`](http://apple.stackexchange.com/questions/51036/what-is-the-difference-between-bash-profile-and-bashrc) will accomplish this:

`export PATH=$PATH:~/.composer/vendor/bin`

### A note on `composer update`

`composer update` is a dangerous command and shouldn't be used unless you know what you are doing. If used wrongly you can accidentally fetch versions of your projects dependencies that don't work with your code base.

The best thing to do is read their [getting started](https://getcomposer.org/doc/01-basic-usage.md) guide and learn everything you need to about composer!

## 2. Style Guide

The good people over at [PHP-FIG](http://www.php-fig.org/) have spent a good amount of time (probably too long) hashing out various standards on of which is a coding style guide. We have chosen to use this style guide for a few reasons:

* Widely adopted by many popular libaries
* Didn't have to write it our selves
* We agree (mostly) with it

The style guide is [PSR-2](https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-2-coding-style-guide.md) (very catchy I know) and the best thing to do is use their [readme](https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-2-coding-style-guide.md) as a reference.

A key thing to take into account with this style guide is the difference between "**MUST**" and "**SHOULD**", for example:

> Code MUST use 4 spaces for indenting, not tabs.

There is no budging on this, we **must** all use 4 spaces for indenting!

Whereas:

> Method names SHOULD NOT be prefixed with a single underscore to indicate protected or private visibility.

We can budge on this, infact we prefer that visibility is indicated with a single underscore!

## 3. Linting

Linting is probably the best tool a programmer has and you **must** use it (see what I did there).

### `php -l`

This method of linting is built in to PHP and will do a syntax check, so if you've forgotten that trailing semi-colon it will let you know! It can be run using the following command:

`php -l YourAwesomePhpClass.php`

This is cumbersome so you should probably install a package for your favourite text editor to take care of this.

#### Sublime Text

Follow these [instructions](https://packagecontrol.io/packages/SublimeLinter-php) to get setup with `php -l` in Sublime Text.

###  `phpcs`

[PHP_CodeSniffer](https://github.com/squizlabs/PHP_CodeSniffer) takes the pain out of learning all the style guide rules, using this handy command we can programmatically check out PHP files against [PSR-2](https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-2-coding-style-guide.md).

You can install the `phpcs` command using the following command:

`composer global require "squizlabs/php_codesniffer=*"`

You can then lint a given file with the following command:

`phpcs --standard=PSR2 YourAwesomePhpClass.php`

**Again** this is pretty cubersome so you should probably install a package for your favourite text editor to take care of this.

#### Sublime Text

Follow these [instructions](https://packagecontrol.io/packages/SublimeLinter-phpcs) to get setup with `phpcs` in Sublime Text.

### `phpmd`

[PHP Mess Detector](https://phpmd.org/) is an awsome linter that looks into the complexity of your code as well as giving you instruction on some best practice. You don't need to use this but it is a handy tool and we couldn't recommend it highly enough!

## 4. Testing

This section should really just be called [`phpunit`](https://phpunit.de/), we use `phpunit` to run all our PHP tests and why would you use [anything else](https://www.google.co.uk/webhp?sourceid=chrome-instant&rlz=1C5CHFA_enGB726GB726&ion=1&espv=2&ie=UTF-8#q=alternatives+to+phpunit)!

### Testing Strategy

When to write tests and when to not bother is a tough one but here are our rules of thumb:

* Have you just refreshed your browser over and over with a `var_dump` to work out what is going on?
* Have you just refactored a large bit of your code base and you're not sure its still working?
* Are you having a hard time working out where to start or how to break down the problem?
* Can you remember every line of code you've written and how a new line or code might affect them?

If any of the above is true you should write a test, but we know its tougher than that!

### Test Driven Development (TDD)

TDD is hard initially, takes ages and you feel like it would be quicker to just write the code then maybe write a test afterwards when its working. This is kind of true but also quite a shallow approach to solving a problem, you've dived in head first potentially not thought about all the cases and then maybe coded up a solution that doesn't fully work. This is probably more of a waste of time and when you add up all the extra time patching old code you'll probably land somewhere near the same amount of time spent if you had written it with TDD.

#### TDD Lifecycle

1. Write the test
2. Run the test - This **should** fail
3. Code!
4. Run the test - This **should** pass
5. Refactor
6. Run the test - This **should** pass

The hardest thing is probably coming up with the *things* you want to test, here are a few examples:

* What are the inputs?
* What are the outputs?
* Are there any exceptions?

If you are still confused read other peoples test! Go and check out the tests from your [favourite framework](https://github.com/cakephp/cakephp/tree/master/tests/TestCase) and compare them to the functions they are testing, the pennies will start to drop.

#### In Aid of Writing Tough Code

When a problem is tough, and you don't know where to start, using TDD will help you get on the road to Codedem. By working out what your inputs are (and testing them) and what your ouputs are (and test them) the middle will just fill its self in! And if all else fails you can get one of your [fellow programmers](http://wearelighthouse.com/team/christy/) to write the tests and you just make them pass, one way or another you'll get there!

#### A Big Caveat

TDD is not going to feel natural to start with but you should still be writing test just maybe after you've written the code. This **is** fine and maybe one day, with a little help from your [friends](http://wearelighthouse.com/team/), you will love the TDD way!