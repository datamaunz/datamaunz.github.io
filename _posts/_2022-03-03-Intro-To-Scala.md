--- 
layout: post 
title: Introduction to Scala
subtitle: Notes from David Venturi's DataCamp Course
categories: Scala
tags: [Scala, Intro]
---

## General

- Scala fuese object-oriented and functional programming concepts
- This makes it so scalable
- every value is an object, and every operation is a method call

```S
val sumA = 2 + 4
```

Behind the scenes, Scala rewrites it like that:

```S
val sumA = 2.+(4)
```

1. Functions are first-class values
2. Opeartions of a program should map input values to output values rather than change data in place (i.e. functions should not have side effects)

## Why Scala is awesome

- concise
- high-level
- advanced static types systems
- compatible (runs primarily on JVM)

## Get started

Check out this [online playground](https://scastie.scala-lang.org/pEBYc5VMT02wAGaDrfLnyw) to experiment with Scala

### Installation

[Install Scala](https://docs.scala-lang.org/getting-started/index.html) on your computer.

```Terminal
brew install coursier/formulas/coursier && cs setup
```

Alternatively, if you don't use Homebrew

```Terminal
curl -fL https://github.com/coursier/launchers/raw/master/cs-x86_64-apple-darwin.gz | gzip -d > cs && chmod +x cs && (xattr -d com.apple.quarantine cs || true) && ./cs setup
```

Now, install [Java8](https://www.oracle.com/java/technologies/javase-jdk8-downloads.html), [11](https://www.oracle.com/java/technologies/javase-jdk11-downloads.html), or [8/11](https://adoptopenjdk.net/) (in case it is not installed yet).

Install [sbt](https://www.scala-sbt.org/download.html)

### Create a new Scala project

To create a new Scala project with `sbt`:

1. `cd` to an empty folder
2. Run the command 

`sbt new scala/scala3.g8` to create a `Scala 3` project, or `sbt new scala/hello-world.g8` to create a `Scala 2` project. This pulls a project template from GitHub. It will also create a target folder, which you can ignore.

3. When prompted, name the applciation `hello-world`. This will create a project called *hello-world*
4. Let's take a look at what just got generated:

```
- hello-world
    - project (sbt uses this for its own files)
        - build.properties
    - build.sbt (sbt's build definition file)
    - src
        - main
            - scala (all of your Scala code goes here)
                - Main.scala (Entry point of program) <-- this is all we need for now
```




you need a scala interpreter


