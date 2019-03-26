l5-stomp-queue
==============

[![Latest Stable Version](https://poser.pugx.org/mayconbordin/l5-stomp-queue/v/stable)](https://packagist.org/packages/mayconbordin/l5-stomp-queue) [![Total Downloads](https://poser.pugx.org/mayconbordin/l5-stomp-queue/downloads)](https://packagist.org/packages/mayconbordin/l5-stomp-queue) [![Latest Unstable Version](https://poser.pugx.org/mayconbordin/l5-stomp-queue/v/unstable)](https://packagist.org/packages/mayconbordin/l5-stomp-queue) [![License](https://poser.pugx.org/mayconbordin/l5-stomp-queue/license)](https://packagist.org/packages/mayconbordin/l5-stomp-queue)

STOMP Queue and Broadcaster Driver for Laravel 5.

## Installation

In order to install l5-stomp-queue, just add

```json
"mayconbordin/l5-stomp-queue": "dev-master"
```
	
to your composer.json. Then run `composer install` or `composer update`.

Add the Service Provider to the `providers` array in `config/app.php`:
	
```php
'providers' => array(
    ...
    'Mayconbordin\L5StompQueue\StompServiceProvider',
)
```

And add the driver configuration to the `connections` array in `config/queue.php`:

```php
'connections' => array(
    'stomp' => [
        'driver'     => 'stomp',
        'broker_url' => 'tcp://localhost:61613',
        'queue'      => 'default',
        'system'     => 'activemq'
    ]
)
```

And for the broadcaster add the same configuration to the `connections` array in `config/broadcasting.php`:

```php
'connections' => array(
    'stomp' => [
        'driver'     => 'stomp',
        'broker_url' => 'tcp://localhost:61613',
        'queue'      => 'default',
        'system'     => 'activemq'
    ]
)
```


## Configuration Options

### `queue`

The name of the queue.

### `system`

The name of the system that implements the Stomp protocol. Default: `null`.

This value is used for setting custom headers (not defined in the protocol). In the case of ActiveMQ, it will set the 
`AMQ_SCHEDULED_DELAY` (see [docs](http://activemq.apache.org/nms/stomp-delayed-and-scheduled-message-feature.html))
header in order to give support for the `later` method, defined at `Illuminate\Contracts\Queue`.
 
### `sync`
 
Whether the driver should be synchronous or not when sending messages. Default: `false`.
 
### `prefetchSize`
 
The number of messages that will be streamed to the consumer at any point in time. Applicable only to ActiveMQ. Default: `1`.
 
For more information see the [ActiveMQ documentation](http://activemq.apache.org/what-is-the-prefetch-limit-for.html).
 
### `clientId`
 
Used for durable topic subscriptions. It will set the `activemq.subcriptionName` property. See [documentation](http://activemq.apache.org/stomp.html#Stomp-ActiveMQextensionstoStomp)
for more information.

### `username` and `password`

Used for connecting to the Stomp server.

## Mediacom Maestro port (for incorporation with versions of Laravel 5.3 or greater).

### Current LTS (as of March 2019) is 5.5.

This was copied and slightly modified from https://github.com/mayconbordin/l5-stomp-queue.  The reason for this is because a 
couple of the dependencies are out of date for versions of Laravel beyond version 5.2.  Attempted Laravel upgrades beyond 5.2 using Composer 
will result in critical error messages because of the obsolete versions of the sub dependencies 'mayconbordin/l5-stomp-queue' insists upon. 
The current LTS version of Laravel is 5.5. The Maestro UI portion depends on Laravel 5.x.  Security has required that Maestro UI update its 
Laravel version to a version which continues to have security support, which is the reason for the overall upgrade.

Maestro UI, up until the Laravel 5.5 upgrade had listed the original 'mayconbordin/l5-stomp-queue' package from Github as a 
dependency.  But its own internal dependencies had conflicts with even Laravel 5.3, nevermind 5.5. This is because of one of its own
Symfony dependencies. The dependency, "symfony/process" and also its "phpunit/phpunit" dependency were both incompatible with 
either the dependencies of the newer versions of Laravel or their respective dependencies.  The changes here simply updated certain
of the dependencies to allow for newer versions which are Laravel 5.5+ compatible.  But this port IS still compatible with Laravel
version 5.2.  But this updated dependency is simply much more flexible and future proof.


## Mediacom Maestro installation of this fork:

### `Composer:`

Rather than merely being a standard dependency portion of the "require" and/or "require-dev" array(s), this can be installed using the repositories
property of the composer.json object.  It will look like the following:

    "repositories": [
        {
            "type": "git",
            "url": "https://github.com/kschipul-mediacomcc/l5-stomp-queue.git"
        }
    ],
    "require": {
        ...,
        "mayconbordin/l5-stomp-queue": "dev-master",
    }


After your composer.json is updated with the definitions in the above manner, you may then using the command line enter the following command:

'composer update'


This will update your project dependencies accordingly.
