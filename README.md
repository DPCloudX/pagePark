#### PagePark

I wrote  this simple HTTP server to park domains I've bought but not yet used.

It's written in JavaScript and runs in Node.js.

Each domain is in its own folder. The content for that domain is in the folder. I went a little wild with content types, it can serve Markdown docs, or run JS code. And of course HTML, text files, images, movies, etc. 

Yet it's still very simple. Which is the point. ;-)

It's 90 percent of what all web servers do, so if you learn how to run <a href="http://pagepark.io/">PagePark</a>, you're learning how to run a web server. A real one you can use to host your sites. And it's easy to hack the code if you want to.

#### How to

1. Create a folder to host your website. 

2. Copy all the files from the downloaded <a href="https://github.com/scripting/pagepark/archive/master.zip">folder</a> into that folder.

3. npm install

4. node pagepark.js

#### Screen shot

Here's a <a href="http://scripting.com/2015/01/04/pageParkFolderScreenShot.png">screen shot</a> of my PagePark server folder. 

#### How it works

1. PagePark will automatically create a *prefs* sub-folder and a *domains* sub-folder. 

2. Add your web content under domains. Each folder's name is the name of a domain. The contents within the folder are what we serve. <a href="http://scripting.com/2015/01/04/pageParkFolderScreenShot.png">Screen shot</a>.

3. Serves all major media types including audio and video. Files whose names end with .md are passed through the built-in Markdown processor. Files ending with .js are interpreted as scripts. The text they return is what we serve. Here's an <a href="https://gist.github.com/scripting/2b5e237a105b6cb96287">example</a> of a script that I have <a href="http://lucky.wtf/badass/butt.js">running</a> on one of my servers.

4. The prefs folder contains a file of settings you can change, prefs.json. These include the port that the server runs on and the name of the index file (see below).

5. stats.json contains information generated by the server including the number of times the server has started, how many hits it's received (all time and today), and hits by domain.

6. mdTemplate.txt is the template we use to serve Markdown text. You can edit this file to provide a common template for all your Markdown documents.

7. If a request comes in for a folder, we scan the folder for a file whose name begins with *index* and serve the first one we find. So the index file can be HTML, Markdown or a script, or any other type PagePark can serve. 

8. If you want to run PagePark from a folder different from the one that contains the app, set the *pageparkFolderPath* environment variable to point to that folder. 

9. There are three special endpoints on all domains: /version, /now and /status that return the version of PagePark that's running, the time on the server and the stats and prefs. 

#### Port 1339

The first time you run PagePark it will open on port 1339. You can change this by editing prefs.json in the prefs folder. 

This means if you want to access a page on your site, the URL will be of the form:

http://myserver.com:1339/somepage.html

The normal port for HTTP is 80. That would have been the natural default, however a lot of Unix servers require the app to be running in supervisor mode in order for it to open on port 80. You can do this by launching PagePark this way:

sudo node pagepark.js

I made the default 1339 because I wanted it to work "out of the box" for first-time users.

#### Example pages

http://noderunner.org/ -- simple home page

http://lucky.wtf/ -- images

http://lucky.wtf/test.md -- markdown page

http://lucky.wtf/badass/ -- index file in a sub-directory

http://lucky.wtf/badass/butt.js -- a page implemented in a script

http://pagepark.io/ -- the home page for this product, served by the product

http://karass.co/nosuchfile.html -- file not found

http://pagepark.io/version -- the version of PagePark that's running on the server

http://pagepark.io/now -- the time on the server

#### JavaScript sample code

I've iterated over the code to try to make it good sample code for JavaScript projects. 

I wanted to make code that could be used for people who are just getting started with Node, to help make the process easier.

There will always be more work to do here. ;-)

#### Updates

##### v0.63 6/27/15 by DW

If you add ?format=json to a request for an OPML file, you'll get a JSON representation of the outline. This is useful for single-page JavaScript apps that want to use the outlineBrowser toolkit to diplay the contents. An example of this is <a href="http://davewiner.com/">davewiner.com</a>. 

I also figured out how to write a JS page that's served by PagePark that returns the JSON representation of any OPML file. Here's a <a href="https://gist.github.com/scripting/7b95bfea614245a47f38">gist</a> containing the code. And a <a href="http://pagepark.io/getjsonopml.js?url=http://hosting.opml.org/dave/states.opml">call</a> to that page. It's extra-nice that this feature could be added without modifying PagePark. It's starting to become a real platform! :-)

In both cases, the CORS header allows access from anywhere. 

And in both cases <a href="http://fargo.io/docs/fargo/includes.html">include</a> nodes are resolved.

##### v0.62 6/25/15 by DW

Code cleanup and factoring in <a href="https://github.com/scripting/pagepark/blob/master/lib/opml.js">opml.js</a>.

##### v0.61 6/24/15 by DW

Files with the extension .opml are now  rendered as outlines. 

There's a <a href="http://fargo.io/code/pagepark/defaultopmltemplate.txt">template</a> for outlines, it uses the new <a href="https://github.com/scripting/outlineBrowser">outlineBrowser</a> toolkit. Here's an <a href="http://montauk.scripting.com/test.opml">example</a> of an OPML file rendered through the new template. 

If you want the raw OPML text from the server, you can do it one of two ways:

1. Add ?format=opml at the end of the url. <a href="http://montauk.scripting.com/test.opml?format=opml">Example</a>.

2. If you're making the request in a program, you can add an Accept header with the value: text/x-opml, following a convention established by the OPML Editor. 

As with scripts and markdown files, you can turn the feature off in config.json. 

##### v0.60 5/26/15 by DW

When delegating requests, pass redirects back to the client, don't follow them. This was necessary so that that OAuth dance with Twitter in nodeStorage would work. 

There's a new <a href="http://scripting.com/listings/pagepark.html">structured listing</a> of the source code of PagePark, linked to from the <a href="https://github.com/scripting/pagepark/blob/master/pagepark.js">flat listing</a> (above). This makes it easy to see how the code is organized. It gets pretty deeply nested! The outline view makes that manageable. I use the OPML Editor on a Mac to edit PagePark. 

##### v0.59 5/23/15 by DW

You can now delegate requests to apps running on other ports on your server machine. 

There's a new optional section of the prefs. json file where you specify these mappings. It's called domainMap. It's a set of name-values, where the name is the domain, and the value is the port the requests are mapped to. 

Here's an <a href="https://gist.github.com/scripting/af9df0198db5daa3c982">example</a> of a prefs.json that has a domainMap specified. It maps twitter.radio3.io to the process running on port 5342, twitter.happyfriends.camp to 5338, and any request on judgment.club to the process on port 5351. 

The name matches the end of the HOST header for the request, so a request for judy.judgment.club will map, as will renee.judgment.club and judgment.club.

##### v0.57 5/11/15 by DW

PagePark has pre-defined pages, /now, /version and /status, whose values are returned by PagePark itself. It used to be that they took precedence, so if a site defines pages with those names, the internal ones would be served instead. Now we only serve them if the site didn't define it. 

The <i>urlSiteContents</i> feature now transmits search params. It still will only forward GET calls. This needs to be updated in a future version. 

PagePark now supports wildcards. Suppose you want to serve all the names from mydomain.org with a wildcard. Create a sub-folder of the domains folder with the name *.mydomain.org. If a request comes in for a sub-domain of mydomain.org that doesn't have its own folder, we'll route it through that folder. You can combine this feature with the urlSiteContents feature, or script-implemented pages. 

We also set the <a href="http://stackoverflow.com/questions/19084340/real-life-usage-of-the-x-forwarded-host-header">X-Forwarded-Host</a> and <a href="https://en.wikipedia.org/wiki/X-Forwarded-For">X-Forwarded-For</a> headers on urlSiteContents requests. 

##### v0.56 5/5/15 by DW

New prefs and config values that allow you to disable processing of scripts and Markdown files. By setting the values in prefs.json, you control all domains on the server. And by adding the values to config.json, in the folder the site is served from, you can turn them off selectively by site. I needed to turn off script processing for .js files served from <a href="https://github.com/scripting/river4">River4</a>, to make it possible to serve a full river from PagePark. 

##### v0.55 4/26/15 by DW

With this release you can serve domains whose content is stored elsewhere on the web. 

There's a new optional element of config.json, urlSiteContents. Its value is the URL of a directory on the web that stores the content of the domain. 

As an experiment, I mapped the domain my.this.how to point to a folder in my public Dropbox folder. Here's how I did it.

1. First I created a new sub-folder of my public Dropbox folder called pageParkDemo. I put one file in that folder, index.html.

2. I used the web interface of my domain registrar to point my.this.how at the IP address of my PagePark server.

3. On my PagePark server, I created a sub-folder of the domains folder called my.this.how.

4. I created a <a href="https://gist.github.com/scripting/f06e32acdee685510211">config.json file</a> in that folder, pointing to the pageParkDemo folder in my Dropbox folder.

To test the setup, I just went to <a href="http://my.this.how/">my.this.how</a>, where I saw "this is just a demo".

Just for fun I put a <a href="http://my.this.how/saul.png">picture</a> in that folder, to see if it works.

Obviously it can get a lot more elaborate, and you can store the content anywhere, not just in a Dropbox folder. The key is that the content be accessible over the web. 

##### v0.54 2/18/15 by DW

A new feature for pages implemented as scripts. If the script returns the value <i>undefined</i> PagePark will not return a value to the HTTP client, it assumes that the script will do this. 

To make it possible for a script page to return a value to the client, there's a new built-in function httpReturn. It takes two parameters, the value, a string, and the type, a MIME type. For example you might have a script that makes an HTTP request and returns a value based on the result to the caller.

Here's an <a href="https://gist.github.com/scripting/a3a8232d193ea88e04ba">example script</a> that illustrates.

##### v0.51 1/18/15 by DW

Created utils.js in the lib folder, and require it in pagepark.js.

New feature: If there's a file called config.json in a domain folder, we read it on every request, and values in that file can change the behavior of the server. The first feature allows you to do a whole-site redirect. Useful if you want to have several names map to the same content. Here's an <a href="https://gist.github.com/scripting/27be2d8be40577ad0fdf">example</a> of the config.json file that maps a domain to <a href="http://nodestorage.io/">nodestorage.io</a>.

##### v0.48 1/8/15 by DW

The default port the server boots up on is now 1339. Previously it was 80, which is the standard port for HTTP, but on many OSes this requires PagePark to be running in supervisor mode. I added docs above to explain this. 

Changed package.json so that only *request* and *marked* were listed as dependencies. Apparently the others are included in Node without having to list them. 

Instead of keeping our own MIME type table, we use the Node *mime* package, which is also included as a dependency in the package.json file.

##### v0.47 1/7/15 by DW

Added a package.json file to the repository.

Fixed first-time startup problem creating prefs.json and stats.json. 

Also, we now make sure the *domains* folder exists at startup.

Fixed a problem in handling requests if you specified a different folder for PagePark to serve from. 

#### Questions, comments?

Please post a note on the <a href="https://groups.google.com/forum/#!forum/server-snacks">Server Snacks</a> mail list. 

