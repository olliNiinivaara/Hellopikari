# 🏆 Hellopikari

## Installation
1. *This application requires the [Pikari server-side web application framework](https://github.com/olliNiinivaara/Pikari/)*. Install it first, if you don't have it.
2. Create a subdirectory named *Hellopikari* under the directory where *pikari* is installed.
3. Save *[index.html](https://raw.githubusercontent.com/olliNiinivaara/Hellopikari/master/index.html)* to *Hellopikari* folder.

## Running
1. Open shell, and in the directory where *pikari* is installed, give following command:
- Linux:
  ./pikari -appdir Hellopikari
2. With a javascript-enabled web browser, open the url <http://127.0.0.1:8080>
3. When you are done, press Ctrl-C in the shell

## Lessons

### Pikari is a web server
Pikari serves all static assets at appdir directory given as command line parameter. Create a file called *test.txt* to *Hellopikari* directory and save some text into it. Open url <http://127.0.0.1:8080/test.txt>. The file is served.

### The front-end javascript API is in *pikari.js* 
If file *pikari.js* does not exist in *current working directory*, *pikari* creates it there when launched. *Pikari.js* contains the *pikari API*. The file contains JSDoc comments and the resulting API documentation is [here](http://htmlpreview.github.io/?https://github.com/olliNiinivaara/Pikari/blob/master/doc/pikari_API.html).

### *Pikari* saves data to disk
In *Pikarihello* application, write some text to input field and close and restart the *Pikari* server. Reload the page. Text will reappear. The data is saved to a [sqlite](https://www.sqlite.org/) database called *data.db* in the *Hellopikari* directory. Notice how the file modification time changes as you write text to input field. The database content can be inspected with a tool such as [DB Browser for SQLite](https://sqlitebrowser.org/). When your data schema changes, just delete old *data.db*.

### *Pikari* syncs data between all on-line users
Open *[Hellopikari](http://127.0.0.1:8080/)* in two or more browser windows and notice how the input text is kept in sync while you do modifications in any application.

### Writing prototypes with *Pikari* is surprisingly easy
Study the *Hellopikari* [source code](https://github.com/olliNiinivaara/Hellopikari/blob/master/index.html) and notice how easily all this can be achieved. Open your local *index.html* in some text editor, do some changes to it (like change the title) and reload the page. The changes are immediately effective, which makes developing a breeze. You can even change the *pikari.js*, if deemed necessary (and get the original back just by deleting it).

### Pikari can be configured with *pikari.toml*
If file *pikari.toml* does not exist in *current working directory*, *pikari* creates it there when launched. *Pikari.toml* file allows you to configure *Pikari*. Available options are explained in it and further notes are available in [server manual](https://github.com/olliNiinivaara/Pikari/blob/master/doc/pikari_man.md).

### Only for prototyping
While you can do [evolutionary](https://en.wikipedia.org/wiki/Software_prototyping#Evolutionary_prototyping) front-end prototyping with *Pikari*, the *Pikari* back-end itself is strictly [throw-away](https://en.wikipedia.org/wiki/Software_prototyping#Throwaway_prototyping) grade. It namely lacks all security - everyone is potentially able to bypass your UI and then read, modify, mess up and delete any data. If you publish a prototype to Internet, always start *Pikari* with a password, use a HTTPS+WSS reverse proxy and instruct testers to enter only fake data.

### There is more
There are more examples listed at [*Pikari* web site](https://github.com/olliNiinivaara/Pikari/). But best way to learn is to start writing your own prototype with *Pikari*!