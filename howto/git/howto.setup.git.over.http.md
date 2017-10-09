How to setup git over HTTP
==========================

# Overview
## 1. using HTTP vs. using SSH for git

## 1. This is a placeholder

## 1. git-http-backend

While using git over HTTP, there are 2 components to git-over-http: git and apache.
These two are __connected through a script__ with the name of git-http-backend.

# Installation steps

Start out by installing git and apache2 using the package manager of your distribution.
Add the modules needed by apache to enable git-over-http. These are cgi, alias and env.


```shell
$ git --version
git version 1.9.1

$ git help http-backend
```


### install and enable 3 modules

```shell
$ a2enmod cgi alias env
```

# Reference
## URL
- __[git-scm](https://git-scm.com/docs/git-http-backend)__ - git-http-backend - Server side implementation of Git over HTTP.
- __[stackoverflow](https://stackoverflow.com/questions/26734933/how-to-set-up-git-over-http)__ - How to set up git over http?
