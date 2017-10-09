How to setup git over HTTP
==========================

# using HTTP vs. using SSH for git
## 1

## 2 

## 3 git-http-backend

    While using git over HTTP, there are 2 components to git-over-http: git and apache.
    These two are __connected through a script__ with the name of git-http-backend.

# Installation steps

    Start out by installing git and apache2 using the package manager of your distribution.
    Add the modules needed by apache to enable git-over-http. These are cgi, alias and env
    $ a2enmod cgi alias env

# Reference

