# Yeoman
Yeoman is a generic scaffolding system allowing the creation of any kind of app.
Yeoman is language agnostic. It can generate projects in any language (Web, Java, Python, C#, etc.)
Yeoman by itself doesn’t make any decisions. Every decision is made by *generators* which are basically plugins in the Yeoman environment.



## Getting started

`yo` is the Yeoman command line utility allowing the creation of projects utilizing scaffolding templates (referred to as generators). Yo and the generators used are installed using [npm](http://npmjs.org/).



### Installing yo and some generators

First thing is to install `yo` using `npm`:

```
npm install -g yo
```



### Basic scaffolding

We’ll use `generator-goncy-react` in our examples below.

To scaffold a new project, run:

```
yo goncy-react
```

Most generators will ask a series of questions to customize the new project.

