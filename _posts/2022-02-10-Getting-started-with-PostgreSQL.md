---
layout: post 
title: Getting started with PostgreSQL
subtitle: Notes from reading the guide
categories: PostgreSQL
tags: [PostgreSQL, Intro]
---

## Intro

These are notes from reading the [documentation](https://www.postgresql.org/docs/) of PostgreSQL.

## Basics

- PostgreSQL uses a client/server model. A session consists of:

1. a server process managing the database files, accepting connections to the database and performing database actions on behalf of clients. The server program is called `postgres`
2. the user's client application wanting to perform database operations

- client and server can be on different hosts
    - in this case, they communicate via a TCP/IP network connection
- PostgreSQL servers can handle multiple concurrent connections from clients (based on *forks*)


[Setup Postgres on Mac](https://postgresapp.com/) following these steps

### Download

[Download](https://postgresapp.com/downloads.html) the app

### Create new server

Open the app and click initialize to create a new server

### Install command line tool

```Bash
sudo mkdir -p /etc/paths.d &&
echo /Applications/Postgres.app/Contents/Versions/latest/bin | sudo tee /etc/paths.d/postgresapp
```

## Connect to database

```

```


## install postgres

```Terminal
brew install postgres
```

```Terminal
rm -rf /usr/local/var/postgres
```

```Terminal
postgres -V
```

```Terminal
pg_ctl -D /usr/local/var/postgres start
```

```Terminal
initdb /usr/local/var/postgres start
```














