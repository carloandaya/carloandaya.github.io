---
layout: post
title:  "Install Jekyll on Ubuntu 22.04 WSL"
date:   2023-05-15 12:00:00 -0800
categories: web
---

install [rvm](https://github.com/rvm/ubuntu_rvm)

follow these instructions to [install openssl 1 and install ruby 2.7.4](https://stackoverflow.com/questions/74882078/rvm-error-when-installing-ruby-using-wsl2-on-ubuntu-error-running-rvm-make)

now you can install jekyll and bundler

`gem install jekyll bundler`

you will have to 

`gem update --system`

before you can navigate to the jekyll project and 

`bundle exec jekyll serve`

on each new session you will have to start with

`rvm use 2.7.4`
