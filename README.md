Perloku
=======

Deploy Perl applications in seconds.

## Step 1: App

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

## Step 2: Dependencies

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

Run Carton and generate a `carton.lock`:

```sh
carton install
git add carton.lock
git commit -m "Added carton.lock"
```

## Step 3: Server

If you're using PSGI you can simply create an `app.psgi` and Perloku will
automatically run the app using Starman.

If you're not using PSGI, you must create a
[Procfile](http://devcenter.heroku.com/articles/procfile) that tells
Heroku how to run your server:

```
web: ./app.pl daemon --listen http://*:$PORT
```

## Step 4: Deploy

```sh
heroku create -s cedar --buildpack http://github.com/judofyr/perloku.git
git push heroku master
```

Watch:

```
Counting objects: 5, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (4/4), done.
Writing objects: 100% (5/5), 808 bytes, done.
Total 5 (delta 0), reused 0 (delta 0)

-----> Heroku receiving push
-----> Fetching custom buildpack... done
-----> Perloku app detected
-----> Vendoring Perl
       Using Perl 5.14.2
-----> Installing dependencies
--> Working on /tmp/build_19tm6pb8ch1qa
Configuring /tmp/build_19tm6pb8ch1qa ... OK
==> Found dependencies: Mojolicious
--> Working on Mojolicious
Fetching http://search.cpan.org/CPAN/authors/id/T/TE/TEMPIRE/Mojolicious-2.48.tar.gz ... OK
Configuring Mojolicious-2.48 ... OK
Building Mojolicious-2.48 ... OK
Successfully installed Mojolicious-2.48
<== Installed dependencies for /tmp/build_19tm6pb8ch1qa. Finishing.
1 distribution installed
       Dependencies installed
-----> Discovering process types
       Procfile declares types   -> (none)
       Default types for Perloku -> web
-----> Compiled slug size is 12.4MB
-----> Launching...gi done, v5
       http://perloku-example.herokuapp.com deployed to Heroku

To git@heroku.com:perloku-example.git
 * [new branch]      master -> master
```

[Enjoy!](http://perloku-example.herokuapp.com)
