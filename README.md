# Microservices with Go

The Microservices Architecture contains

-   Broker Service
-   Authentication Service
-   Logger Service
-   Mail Service
-   Listener Service AMQP with RabbitMQ

## Frontend Test

-   The Test Page is used for test the Services in this Architecture
-   The Test Page will send an example request to the specific Service that user choose and then receive the response from that Service
-   Technologies
    -   [bootstrap](https://getbootstrap.com/docs/5.2/getting-started/introduction/)
    -   [javaScript](https://www.javascript.com) for fetching data from the services
    -   [html](https://developer.mozilla.org/en-US/docs/Learn/HTML)

## Broker Service

-   <strong>Broker Service</strong> is an entry point for redirecting the request to appropriate service and receiving the response from that service
-   Package index
    -   [github.com/go-chi/chi](https://github.com/go-chi/chi) is a lightweight, idiomatic and composable router for building Go HTTP services
        -   Installation
            ```sh
            go get -u github.com/go-chi/chi/v5
            go get -u github.com/go-chi/chi/middleware
            go get -u github.com/go-chi/cors
            ```
-   File `broker-service/broker-service.dockerfile` is the `dockerfile` for the service

## Authentication Service with PostgreSQL

-   <strong>Authentication Service</strong> is an API for determining if the credentials from the request body is matching the data in the database. All the credentials are stored in a PostgreSQL image
-   Package index
    -   [github.com/go-chi/chi](https://github.com/go-chi/chi) is a lightweight, idiomatic and composable router for building Go HTTP services
        -   Installation
            ```sh
            go get -u github.com/go-chi/chi/v5
            go get -u github.com/go-chi/chi/middleware
            go get -u github.com/go-chi/cors
            ```
    -   [github.com/jackc/pgconn](https://github.com/jackc/pgconn) is a low-level `PostgreSQL` database driver. It operates at nearly the same level as the `C` library `libpq`
        -   Installation
            ```sh
            go get -u github.com/jackc/pgconn
            ```
    -   [github.com/jackc/pgx](https://github.com/jackc/pgx) is a higher level libraries, high performance interface that exposes PostgreSQL-specific features such as `LISTEN` / `NOTIFY` and `COPY`. It also includes an adapter for the standard `database/sql` interface. The toolkit component is a related set of packages that implement `PostgreSQL` functionality such as parsing the wire protocol and type mapping between `PostgreSQL` and `Go`
        -   Installation
            ```sh
            go get -u github.com/jackc/pgx/v4
            go get -u github.com/jackc/pgx/v4/stdlib
            ```
    -   [github.com/golang/crypto/bcrypt](https://github.com/golang/crypto/blob/master/bcrypt/bcrypt.go) is used for hashing passwords
    -   [pkg.go.dev/database/sql](https://pkg.go.dev/database/sql) provides a generic interface around SQL (or SQL-like) databases. The sql package must be used in conjunction with a database driver. See [golang.org/s/sqldrivers](https://golang.org/s/sqldrivers) for a list of drivers
-   File `authentication-service/authentication-service.dockerfile` is the `dockerfile` for the service

## Logger Service with MongoDB

-   <strong>Logger Service</strong> is an API for saving logs whenever one Service receives and processes a response. All the `LogEntry` struct contains 2 fields Name and Data. The collections are stored in a MongoDB image
-   Package index
    -   [github.com/go-chi/chi](https://github.com/go-chi/chi) is a lightweight, idiomatic and composable router for building Go HTTP services
        -   Installation
            ```sh
            go get -u github.com/go-chi/chi/v5
            go get -u github.com/go-chi/chi/middleware
            go get -u github.com/go-chi/cors
            ```
    -   [github.com/mongodb/mongo-go-driver](https://github.com/mongodb/mongo-go-driver) is the `MongoDB` supported driver for `Go`
        -   Installation
            ```sh
            go get go.mongodb.org/mongo-driver/mongo
            go get go.mongodb.org/mongo-driver/mongo/options
            ```
-   File `logger-service/logger-service.dockerfile` is the `dockerfile` for the service

## Mail Service

-   <strong>Mail Service</strong> connects directly with <strong>Broker Service</strong> in the development version (you should not do that way in production). In production, <strong>Mail Service</strong> can not be connected by User, just be connected to other Service except Broker Service. The Service sends tested mails to the MailHog server.
-   Package index
    -   [github.com/go-chi/chi](https://github.com/go-chi/chi) is a lightweight, idiomatic and composable router for building Go HTTP services
        -   Installation
            ```sh
            go get -u github.com/go-chi/chi/v5
            go get -u github.com/go-chi/chi/middleware
            go get -u github.com/go-chi/cors
            ```
    -   [github.com/mailhog/MailHog](https://github.com/mailhog/MailHog) is an email testing tool for developers
        -   Overview
            -   Configure your application to use `MailHog` for SMTP delivery
            -   View messages in the web UI, or retrieve them with the JSON API
            -   Optionally release messages to real SMTP servers for delivery
        -   Installation
            ```sh
            go get github.com/mailhog/MailHog
            ```
        -   The local server of `MailHog` runs on `http://localhost:1025`, check this server for web UI and see all the tested mails in the `Inbox` section
        -   A `Docker` image for `MailHog` is configured in file `project/docker-compose.yml`
    -   [github.com/vanng822/go-premailer](github.com/vanng822/go-premailer) is an inline styling for HTML mail in golang
        -   Styling mail with both `HTML` and `plain` formats before sending
        -   Installation
            ```sh
            go get github.com/vanng822/go-premailer/premailer
            ```
    -   [github.com/xhit/go-simple-mail](https://github.com/xhit/go-simple-mail) is the best way to send emails in `Go` with SMTP Keep Alive and Timeout for Connect and Send
        -   You can find more information in the [documentation](https://pkg.go.dev/github.com/xhit/go-simple-mail/v2)
        -   Installation
            ```sh
            go get github.com/xhit/go-simple-mail/v2
            ```
-   File `mail-service/mail-service.dockerfile` is the `dockerfile` for the service

## List Service AMQP with RabbitMQ

## Docker usage

-   All the `Docker` configurations are in the `project/docker-compose.yml` file
-   All the `Docker` images control for

    -   `Linux/MacOS` are in the `project/Makefile` file
    -   `Windows` are in the `project/Makefile.windows` file

-   Build all the `Docker` images and services's binaries
    ```sh
    make up-build
    ```
-   Build one `Docker` image of one specific service
    ```sh
    make service-build
    ```
-   Pull and Start all the `Docker` images
    ```sh
    make up
    ```
-   Stop docker compose
    ```sh
    make down
    ```
-   Start the `frontend` binary listening on `http://localhost:80`
    ```sh
    make start
    ```
-   Stop the `frontend` binary listening on `http://localhost:80`
    ```sh
    make stop
    ```

## Contributor

-   Minh Tran (Me)
