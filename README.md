# 🏆 Hellopikari - Tutorial for Pikari

This repo contains two files:
- this README contains step-by-step instructions for *[Pikari](https://github.com/olliNiinivaara/Pikari/)* [Backend-as-a-Service](https://www.cloudflare.com/learning/serverless/glossary/backend-as-a-service-baas/) Server user interface.
- *[index.html](https://github.com/olliNiinivaara/Hellopikari/raw/master/index.html)* is a simple application for Pikari that is used as an example in this tutorial.

## Starting Pikari from command line
1. Install [Pikari](https://github.com/olliNiinivaara/Pikari/) first, if you don't have it yet.
1. Open shell, and in the directory where *Pikari* is installed, give following command:
    - Linux, macOS: ./pikari
    - Windows: pikari
1. Take note that admin password is printed
1. With a javascript-enabled web browser, open the url <http://127.0.0.1:8080>

## Stopping Pikari
Press Ctrl-C in the shell or send SIGINT if running as a service

## Optional command line parameters
Synopsis: **pikari** \[**-password** _password_\] \[**-port** _port_\]

\[-password _password_\]  
Password for administering Pikari. If no password is given, a random password is generated and printed to stdout when Pikari is started.

\[-port _port_\]
By default Pikari runs at port 8080. you can change this with port parameter.

## The index
When Pikari first starts, it creates _index.html_ to it's directory and serves it as home page. Index asks user name and lists all subdirectories as installed applications, including _Admin_ application, which is created for managing other Pikari apps. You can change _index.html_ and admin files as you wish (The original(s) will be recreated if you delete file(s)).
At first, _Admin_ is the only available application. To install another application, open _Admin_ by clicking it and enter admin password.

## Installing an application
1. In Admin, Select Upload new application from...Git repository
1. Enter server directory to upload to: _Hello Pikari_
1. Enter URL to upload from: https://github.com/olliNiinivaara/Hellopikari.git
1. Press proceed

You can also upload local directory to Pikari with *Local directory* -button. You can even log on to your server and manually create and update apps and files, but then new apps become visible only after Pikari is restarted.

## Using applications
You can select an application from index at http://127.0.0.1:8080/ or open it directly at http://127.0.0.1:8080/appdir where appdir is the directory of the application. User name can be submitted to application with parameter _user_. For example, if you installed *Hello Pikari*, you can now open <http://127.0.0.1:8080/Hello%20Pikari?user=tutee>

## Basic functionality

### Automated database persistence
In *Hello Pikari* application, write some text to input field and close and restart the *Pikari* server. Reload the page. Text will reappear. The data is saved to a [sqlite](https://www.sqlite.org/) database called *data.db* in the *Hello Pikari* directory. Notice how the file modification time changes as you write text to input field. The database content can be inspected with a tool such as [DB Browser for SQLite](https://sqlitebrowser.org/). When your data schema changes, check the _Delete existing data_ checkbox when updating.

### Web socket -based data sync between all on-line users
Open *Hello Pikari* in two or more browser windows and notice how the input text is kept in sync while you do modifications in any client.

### Static file serving
If an application does not have an *index.html* file, directory contents are listed *as is* and can be browsed and selected.

## Administrator GUI
In Admin, select Edit... button (the button with pen icon) for some application. An update form appears that lets you to:
- Update the application (replaces all files with new ones)
- Disable the application (it is then hidden from Index and cannot be used (but the static files remain accessible))
- Delete existing data
- Delete the whole application

## Developing an application

### The front-end javascript API *pikari.js* 
*Pikari* API is contained in file *pikari.js*. You can modify it and if deleted, the original will be recreated. The file contains JSDoc comments and the resulting API documentation is [here](http://htmlpreview.github.io/?https://github.com/olliNiinivaara/Pikari/blob/master/doc/pikari_API.html).

### Writing code
Study the *Hello Pikari* [source code](https://github.com/olliNiinivaara/Hellopikari/blob/master/index.html) and notice how easily the application is created. Open your local *index.html* file in some text editor, do some changes to it (like change the title) and reload the page. The changes are immediately effective, which makes developing a breeze. Tip: When you change the application served by pikari, you may need to
[hard reload](https://en.wikipedia.org/wiki/Wikipedia:Bypass_your_cache) the browser page.

### No security
While you can do [evolutionary](https://en.wikipedia.org/wiki/Software_prototyping#Evolutionary_prototyping) front-end prototyping with *Pikari*, the *Pikari* back-end itself is strictly [throw-away](https://en.wikipedia.org/wiki/Software_prototyping#Throwaway_prototyping) grade. It namely lacks all security - all users could bypass your UI and then read, modify, mess up and delete any data. If you publish a prototype to Internet, always use a HTTPS+WSS reverse proxy and instruct testers to enter only fake data. The web socket upgrade request is sent
to path */ws* which you must hail. The configuration for [NGINX](https://www.nginx.com/) (say) should be something like this:

```nginx
upstream pikariws {
  server 127.0.0.1:8080;
  keepalive 100;
}
server {
  location /pikari/ {
    proxy_pass http://127.0.0.1:8080/;
  }
  location /pikari/ws {
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "Upgrade";
    proxy_http_version 1.1;
    proxy_connect_timeout 8h;
    proxy_send_timeout 8h;
    proxy_read_timeout 8h;
    proxy_pass http://pikariws/ws;
  }
```

## There is more
There are more examples listed at [*Pikari* web site](https://github.com/olliNiinivaara/Pikari/). But best way to learn is to start writing your own prototype with *Pikari*!

## Try on-line

This application is testable on-line *as-is* at <https://verkkoyhteys.fi/pikari/hello>. Don't write too fast or you'll receive *429 Too Many Requests* ;-)