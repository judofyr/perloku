Perloku
=======

Deploy Perl applications in seconds.

## Step 1

Write an app:

```perl
#!/usr/bin/env perl
use Mojolicious::Lite;

get '/' => sub {
  my $self = shift;
  $self->render('index');
};

app->start;
__DATA__

@@ index.html.ep
% layout 'default';
% title 'Welcome';
Welcome to the Mojolicious real-time web framework!

@@ layouts/default.html.ep
<!DOCTYPE html>
<html>
  <head><title><%= title %></title></head>
  <body><%= content %></body>
</html>
```

## Step 2 (optional)

Create a Makefile.PL with your dependencies:

```perl
use strict;
use warnings;

use ExtUtils::MakeMaker;

WriteMakefile(
  NAME         => 'app.pl',
  VERSION      => '1.0',
  AUTHOR       => 'Magnus Holm <judofyr@gmail.com>',
  EXE_FILES    => ['app.pl'],
  PREREQ_PM    => {'Mojolicious' => '2.0'},
  test         => {TESTS => 't/*.t'}
);
```

## Step 3

Create an executable file called Perloku which runs a server on the port
given as an enviroment variable:

```sh
#!/bin/sh
./app.pl daemon --listen http://*:$PORT
```

Remember to `chmod +x Perloku`!

## Step 4

Deploy:

```sh
git init
git add .
git commit -m "Initial version"
heroku create -s cedar --buildpack http://github.com/judofyr/perloku.git
git push heroku master
```

