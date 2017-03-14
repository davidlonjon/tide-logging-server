# Tide Logging Server

## Description

This repo will provision a [Graylogw](https://www.graylog.org/) server which will capture log events/messages from the Tide services.

## Dependencies

- Docker: [https://docs.docker.com/engine/installation/](https://docs.docker.com/engine/installation/)

## How to run locally

* Clone the repo:

```
$ git clone git@github.com:davidlonjon/tide-logging-server.git
```

* Create a new `.env` file from the `.env.dist` file and edit the settings:

```
GRAYLOG_PASSWORD_SECRET=some_password_pepper
GRAYLOG_ROOT_PASSWORD_SHA2=some_sha2_encrypted_password
GRAYLOG_WEB_ENDPOINT_URI=http://localhost:9000/api
```

To generate a SHA2 encrypted password you can run the following command on your local:

```
$ echo -n yourpassword | shasum -a 256
```

Launch the Graylog2 server using the docker compose command

```
$ docker-compose up -d
```

Now you should be able to access the admin console of the Graylog2 server at [http://localhost:9000/api](http://localhost:9000/api)

To interact with the Graylog2 server to send log events to it using PHP for example, you will be able to do it using your host machine IP on port `12201` as the graylog IP address and port.

## How to run on production

TBC


## How to send a log event to the Graylog server using php:

You can find and how to on Jeremy Cook blog post at [https://jeremycook.ca/2012/10/02/turbocharging-your-logs/](https://jeremycook.ca/2012/10/02/turbocharging-your-logs/).

In a nutshell, you'll need:

- Composer PHP package manager: [https://getcomposer.org/](https://getcomposer.org/)
- Monolog PHP library: [https://github.com/Seldaek/monolog](https://github.com/Seldaek/monolog)
- A GELF PHP library for Graylog2: [https://github.com/mlehner/gelf-php](https://github.com/mlehner/gelf-php)

Code sample:

```
<?php

require "vendor/autoload.php";
use Monolog\Logger;
use Monolog\Handler\GelfHandler;
use Gelf\MessagePublisher;

$log = new Logger('Test');
$log->pushHandler(new GelfHandler(new MessagePublisher('IP OR DOMAIN NAME OF YOUR GRAYLOG2 SERVER')));

$log->addWarning('Test warning message');
$log->addError('Test error message');
$log->addInfo('Test info message');
$log->addDebug('Test debug message');
```
