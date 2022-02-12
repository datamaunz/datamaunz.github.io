---
layout: post
title: Setup PostgreSQL
subtitle: Notes from Datacamp Course
categories: Databases
tags: [Relational Databases, Intro]
---

## Prerequisites

- Basic understanding of SQL
- For Setup on a Mac see [here](https://youtu.be/wTqosS71Dc4), [here](https://medium.com/@xueyingli66/start-using-postgresql-with-terminal-on-mac-787ab643c817), [here](https://medium.com/@viviennediegoencarnacion/getting-started-with-postgresql-on-mac-e6a5f48ee399) and [here](https://towardsdatascience.com/postgresql-basics-to-get-you-up-and-running-macos-f0a14d51aed0)


If you are a Mac user, you can install Postgresql via Homebrew. Open your Terminal and check whether Homebrew is already installed.

```shell
brew --version
```

If Homebrew is already installed you can update it by exectuing the following command.

```shell
brew update
```
If you have now Homebrew on your machine yet, you can install it by running:

```shell
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
```

Once Homebrew ist installed or updated run:

```shell
brew install postgresql
```

You can turn PostgreSQL on or off by using the following comands:

```shell
brew services start postgresql
brew services stop postgresql
```

once you ahve started PostgreSQL, enter the PostgreSQL terminal by typing:

```shell
psql postgres
```