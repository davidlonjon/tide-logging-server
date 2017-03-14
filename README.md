# Tide Graylog Server

## Description

This repo will provision a [Graylog](https://www.graylog.org/) server which will capture log events from the Tide services.

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

Launch the graylog server using docker compose (in daemon mode)

```
$ docker-compose up -d
```

Now you should be able to access the admin console of the Graylog server at [http://localhost:9000/api](http://localhost:9000/api)

To interact with the Graylog server to send log events to it using PHP for example, you will be able to do it using your host machine IP on port `12201` as the graylog IP address and port.

## How to run on production

TBC
