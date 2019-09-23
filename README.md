# 🏆 Hellopikari

## Installation
1. *This application requires the [Pikari server-side web application framework](https://github.com/olliNiinivaara/Pikari/)*. Install it first, if you don't have it.
2. Create a subdirectory named *Hellopikari* under the directory where pikari is installed.
3. Download and save *index.html* to *Hellopikari* folder by clicking [this link](https://github.com/olliNiinivaara/Hellopikari/index.html).

## Running
1. Open shell, and in the directory where *pikari* is installed, give following command:
- Linux: ./pikari -appdir Hellopikari
2. With a javascript-enabled web browser, open the url [http.//127.0.0.1:8080]http.//127.0.0.1:8080
3. When you are done, press Ctrl-C  in the shell

## Lessons

### The *pikari.toml* configuration file
If file *pikari.toml* does not exist in current working directory, *pikari* creates it there when launched. *Pikari.toml* file allows you to configure *Pikari*. Available options are explained in [Pikari user guide](https://verkkoyhteys.fi/pikariserver.html).

### The *pikari.js* front-end javascript API
If file *pikari.js* does not exist in current working directory, *pikari* creates it there when launched. *Pikari.js* contains the *pikari API*. The file contains JSDoc comments and the resulting API document is [here](https://verkkoyhteys.fi/pikarijs.html).

### *Pikari* saves data
In *Pikarihello* application, write some text to input field and reload the page. Text will reappear.

### *Pikari* saves data to disk
In *Pikarihello* application, write some text to input field, close (Ctrl-c) the *Pikari* server and restart it. Reload the page. Text will reappear. The data is saved to a [sqlite](https://www.sqlite.org/) database called data.db in the *Hellopikari* directory. Notify how the file modification time changes as you write text to input field. The database content can be inspected with a tool such as [DB Browser for SQLite](https://sqlitebrowser.org/).

### Pikari uses HTTP POST for locking and Websockets for any other communication
Open browser's web developer tool, start inspecting network traffic and reload Pikarihello. *Index.html*, *pikari.js* and *favicon.ico* are loaded first. Then a permanent web socket connection is opened, with user name in url query string. At last a web socket start message is sent to which the server responds with a start message containing all data and list of all on-line users. All on-line users are also informed about the new user with a sign-in message. Every modification of the input field then follows this protocol:
1. Client sends a *setlocks* HTTP POST message to server which acquires an exclusive write lock for the user
2. Server sends a *lock* ws message to all clients that tells which locks are currently held by which user
3. Client sends a *commit* ws message to server which contains the modified data
4. Server sends a *change* ws message to all clients which contains the modified data
5. Server sends a *lock* ws message to all clients that tells which locks are currently held by which user

### *Pikari* syncs data between all on-line users
Open *Hellopikari* in two or more browser windows and notice how the input text is kept in sync while you do modifications in any application.

### Writing prototypes with Pikari is surprisingly easy
Study the *Hellopikari* [source code](https://github.com/olliNiinivaara/Hellopikari/blob/master/index.html) and notice how easily all this can be achieved. Open your local *index.html*, do some changes to it (like change the title) and reload the page. The changes are immediately effective, which makes developing a breeze. You can even change the *pikari.js*, if deemed necessary (and get the original back just by deleting it).

### Only for protoyping
While you can do evolutionary UI prototyping with *Pikari*, the *Pikari* server itself is only suited for throw-away prototyping. It namely lacks all security - every user can read, modify, mess up and delete any data. If you publish a prototype to Internet, always start *Pikari* with a password, use a HTTPS reverse proxy and only handle fake data.

### There is more
There are more examples listed at [*Pikari* web site](https://github.com/olliNiinivaara/Pikari/). But best way to learn is to start writing your own prototype with *Pikari*!