# Introduction

This framework is intended to simply making ivr applications using Asterisk's
AGI, providing a nice level of abstraction over what an IVR should look like
from a developers' perspective.

# Documentation

# Installing
Add this library to your [Composer](https://packagist.org/) configuration. In
composer.json:
```json
  "require": {
    "kstych/php-agi": "*"
  }
```

# Quickstart

You can start by *doc/examples/quickstart* for a very basic example. You'll need something like this in your dialplan:

    [default]
    exten => 1,1,AGI(/path/to/PAGI/doc/examples/quickstart/run.sh,a,b,c,d)
    exten => 1,n,Hangup

# Testing IVR applications

A mocked pagi client is included to easily test your ivr applications. See
**doc/examples/mock** to see an example of how to use it.

# Features

## Nodes

Simple Call Flow Nodes are available (see **doc/examples/node/example.php**). Using
nodes will let you simplify how you build and test your ivr applications. Nodes
are an abstraction layer above the pagi client, and support:

 * Prompts mixing sound files, playing numbers/digits/datetime's.
 * Cancel and End Of Input digits.
 * Validator callbacks for inputs, can optionally specify 1 or more sound files
 to play when the validation fails.
 * Callbacks for invalid and valid inputs.
 * Optional sound when no input.
 * Maximum valid input attempts.
 * Optional sound when maximum attempts has been reached.
 * Expecting at least/at most/exactly N digits per input.
 * Timeout between digits in more-than-1 digit inputs.
 * Timeout per input attempt.
 * Retry Attempts for valid inputs.
 * And much more!

The NodeController will let you control the call flow of your application, by
registering nodes and actions based on node results. Thus, you can jump from
one node to the other on cancel or complete inputs, hangup the call, execute a
callback, etc. For an example, see doc/examples/nodecontroller/example.php

## AutoDial

CallFiles are supported. You can also schedule a call in the future.

## Fax

Sending and receiving faxes is supported using spandsp (applications SendFax
and ReceiveFax).

## Available Facades

 * PAGI\Client\CDR: Provided to access cdr variables.
 * PAGI\Client\ChannelVariables: Provided to access channel variables and asterisk
environment variables.
 * PAGI\Client\CallerID: Provided to access caller id variables.
 * PAGI\Client\Result: Provided to wrap up the result for agi commands.
 * PAGI\CallSpool\CallFile: Call file facade.
 * PAGI\CallSpool\CallSpool: Call spool facade.
 * PAGI\Logger\Asterisk: Provides access to asterisk logger (see logger.conf in
your asterisk installation).

## Results

For every operation, a Result is provided. Some operations decorate this
Result to add functionality, like PlayResult, ReadResult, etc. For example,
a stream file will return a PlayResult, which decorates a ReadResult which
in turn, decorated a Result.

  * PAGI\Client\DialResult
  * PAGI\Client\ExecResult
  * PAGI\Client\ReadResult
  * PAGI\Client\PlayResult
  * PAGI\Client\FaxResult

## Debugging, logging

You can optionally set a [PSR-3](http://www.php-fig.org/psr/psr-3/) compatible logger:
```php
$pagi->setLogger($logger);
```

By default, the client will use the [NullLogger](http://www.php-fig.org/psr/psr-3/#1-4-helper-classes-and-interfaces).


LICENSE
=======
Copyright 2026 Siddharth Upmanyu <siddharth@kstych.com>
Copyright 2011 Marcelo Gornstein <marcelog@gmail.com>

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

