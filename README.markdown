#mng-web - *The simple way to manage your website*
---
This is a simple but useful way to manage your static website or your ruby-generated website!

## What's mng-web?
mng-web means 'manage-web' and is a little piece of code that helps you to deploy your static website or your ruby-generated website into production in seconds.

I wrote mng-web to help myself to deploy my website ([http://dallangelo.com](http://dallangelo.com)) easily.

## Why had I to use it?
I wrote mng-web because I have a small personal website that has to be generated every 5 minutes (if you see my website dallangelo.com I'm talking about 'weather' and 'from my log').

Using mng-web I can manage all my pages easily both in production and locally on my mac.
This is why I use (and wrote) mng-web :)

## Why didn't you use rails or sinatra or [place here your favourite framework]?
First of all I love rails and Sinatra, I use them at work everyday and I'm really happy to work with them.

So why I didn't use them? As you can see dallangelo.com is a simple, small and with few visits website, so it didn't make any sense to run an app just for it, a static file is much faster and it works pretty good with also the content (the content doesn't need to be updated every second).

## Ok, explain me how it works!
You have two main directories:

- 'workdir'   -> Where the code lives, this is the directory you will edit with your favourited editor
- 'generated' -> In this directory will be stored files ready to be copied into production

Both these directories has inside some other directories (they are quite easy to understand):

- 'html'  -> Where there are html files or .rb files that generates html files (in 'generated' there isn't this directory because html files must be in the root)
- 'css'   -> Where there are css files or .rb files that generates css files
- 'js'    -> Where there are javascript files or .rb files that generates javascript files

When the script runs it looks in these directories looking for .rb files, it executes these files assuming that every .rb file will generate a file. So if the script finds in 'workdir/js' the script 'application.rb' it assumes that executing this script 'application.js' will be created. Another example, if the script finds in 'workdir/css' the file 'main.rb' it assumes that after its execution a file called 'main.css' will be available in the same directory.
The concept about html file is quite identical with the difference that in 'generated' there isn't 'html' directory. So if there is a script 'about.rb' the script executes it, assumes that it generates a 'about.html' in 'workdir/html' and copy it into 'generated' (not 'generated/html' as other examples).

This allows the script to deduce everything necessary to generate everything.
Do you have a static html that never changes? You can put this html file in the directory 'workdir/html' and the script simply will copy it to 'generated' directory.

Do you need that your 'index.html' is generated? No problem, puts into 'workdir/html' your 'index.rb' script and it will be executed and the resulting page will be copy into 'generated' directory.

## How to use it?
It's easy! First of all clone this repository with:

git clone git@github.com:simo2409/mng-web.git

then

cd mng-web

Now you have to run the script to generate directories, just a simple

rake setup:generate_directory_structure

And now you can start writing your html (or ruby) where you need (in workdir directory)!

## To do
mng-web has many things that have still to be done, this is a little list:

- minify css
- minify js
