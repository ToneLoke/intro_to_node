#Intro to Node

###Objectives

- Learn the history of node
- Blocking vs. non-blocking
- Use file system module
- Require and build modules

Node is the most popular implementation of server side JavaScript. 

- JavaScript 1995 by Brendan Eich at Netscape 

- JavaScript used to have a reputation for lacking in performance and only used by amateur

- Due to the overall improvement in browsers and computer, JavaScript becomes suitable for just about any general purpose computer task


####Facts:

	- Node.jsCreated by Ryan Dahl
	- Built on top of Google's V8 JavaScript engine	
	- Used for highly scalable network application
	- Managed by Joyent
	- Over 77,000 third-party modules have been published to npm

###Simple server

	var http = require('http');
	http.createServer(function (req, res) {
	  res.writeHead(200, {'Content-Type': 'text/plain'});
	  res.end('Hello World\n');
	}).listen(1337, '127.0.0.1');
	console.log('Server running at http://127.0.0.1:1337/');

###MEAN-Stack

Before this, we were using RAP (Rails, Angular, Postgres) for our need.

MEAN-stack is *Mongo, Express, AngularJS, NodeJS*

###Check out the JavaScript usage

[Githut Statistic](http://githut.info/)


###I/O: Problem with modern web

Eventually everyone has to execute some I/O. 

	Input, Output

￼I/O is the slowest part of any system for the modern computers

On servers that have to respond to hundred of thousands requests, I/O becomes a crippling performance problem.

###Steps of IO

<img src="http://everypageispageone.com/wp-content/uploads/2011/12/dbreport.png" />

1. A request comes in to the web server. The client has requested every car in our
database that is “red.”

2. A second remote server houses our corporate database. Our web server creates a
database query and sends it off to the remote server housing our database.

3. The database engine runs the query and assembles the results.

4. The database server responds to our web server with the results.

5. The web server receives the query results and turns them into JSON so that the
results are easier to process.

6. The server reads from the local file system to find an appropriate HTML document to
display the results.

7. The server responds to the request and returns HTML with all the red cars in our database
￼

###Blocking

I/O activity that blocks
other processing from happen in a running program is known as blocking I/O.

###JavaScript is non-blocking

Rather than having to solve the I/O issues after the language was created, it has as simple solution already built into the core: **Callbacks**


###Callbacks

A callback is a function that is executed when I/O operations finish or produce an error. There is a delay waiting for I/O to finish, but other logic can execute freely during this time. **This is all built into JavaScript's initial design**

###Examining how to web browser works

Event loop = Watches the stack and callback queues to see if it can push into the stack

Stack = Where the code gets run

Callback(task) queue = Where the callbacks waits

Web apis = running the code behind the scenes

Render queue = renders the browser every 16 milliseconds. It has a higher priority than callback queue. 

During the synchronous queue, the render queue is blocked. Synchronous code blocks the render queue

	console.log('hi');
	
	setTimeout(function(){
		console.log('there');
	}, 5000);
	
	console.log('JS')


###Real world case study

<img src="java.png" />

1. The Node.js web server could handle double the number of requests per second
compared to the Java server. This is especially impressive because the Java web
server was using five cores and the Node.js server was using one.

2. There was a 35% decrease in the response times for the same page. This amounts to
about 200ms time savings for the user, a noticeable amount.

###Using the Node

When typing in `node` in the terminal, you launch Node's REPL (Read-Eval-Print-Loop)

	> 10+5
	> console.log('Hello you cool cats')
	> var x = 5
	> var http = require('http')
	> http


###Dealing with File System in Node

	$ echo "You are cool" >> text.txt
	$ touch filesystem.js

	// filesystem.js
	var fs = require('fs');
	
	fs.readFile('text.txt', 'utf-8', function(error, data){
	  if(error){
	    throw error;
	  }
	  console.log(data);
	});

###Add to our file

	// filesystem.js
	
	fs.appendFile('text.txt', 'Writing more content', function(error){
	
	  if(error){
	    return console.error(error);
	  }
	
	  console.log('Added new data to file');
	
	});
	
	// Other content

###Creating another file

	// Other content

	fs.writeFile('anotherFile.txt', 'I am learning node!', function(error){

	  if(error){
	    return error;
	  }
	
	  console.log('Wrote another file');
	
	});

###What is npm?

	npm is the package manager for Node.js

###npm commands

`npm install` will install add modules listed in the package.json in the current directory

`npm install -g [module name]` will install the module globally

`npm install --save [module name]` will install the module locally

`npm search` is a quick way to query the npm registry without leaving the terminal.

Try:

	$ npm search colors

It will bring up all the modules with colors in the name

`npm init` will create an package.json for you

Run: 

	$ npm init

Click enter thru the steps. You will get a file that looks something like this:

	{
	  "name": "node_intro",
	  "version": "1.0.0",
	  "description": "Testing out node",
	  "main": "filesystem.js",
	  "scripts": {
	    "test": "echo \"Error: no test specified\" && exit 1"
	  },
	  "author": "Stanley Yang",
	  "license": "ISC"
	}

- **name**: is the name of the module. Usually, it shouldn't have node or js in it. Avoid non-URL-safe character since it is used to create a URL when it's on the npm registry
- version: version of package: **major.minor.path**
- scripts: Provides addition npm commands running from this directory. Try running `npm test`
- dependencies: List of all modules that the current module or app needs to function. We can state version numbers.

###require()

The require function is Node specific, meaning we can't use it in the web browsers.

When you require code, you execute the JavaScript code that maes up the specific module. 

When we require('http) , Node will try the following:

1. Check to see if the module named is a core module.

2. Check in the current directory’s node_modules folder.

3. Move up one directory and look inside the node_modules folder if present.

4. Repeat until the root directory is reached.

5. Check in the global directory.

6. Throw an error because the module could not be found.

In our directory, now run:

	$ npm install colors --save

Add the following:

	var colors = require('colors');

And append colors to all console.log

	console.log(‘Hello world’.green);

This will color all of your console.logs

###Writing our own module

	$ mkdir math
	$ touch index.js

In `index.js`

	// exports object exports functions from a module
	exports.add = add;
	exports.multiply = multiply;
	exports.divide = divide;
	
	function add(number1, number2){
	  return parseInt(number1, 10) + parseInt(number2, 10);
	}
	
	function multiply(number1, number2){
	  return parseInt(number1, 10) * parseInt(number2, 10);
	}
	
	function divide(number1, number2){
	  return parseInt(number1, 10) / parseInt(number2, 10);

Now try the code in the local directory

	$ touch test.js

In `test.js`

	var math = require('./index');
	
	console.log(math.add(3, 5)); // expect 8
	console.log(math.multiply(4, 5)); // expect 20
	console.log(math.divide(20, 5)); // expect 4

When the index is required the first time, it will be cached. 

###npm link

npm link is a 2 step process.

1. Move index.js into a new folder named math-module and run `npm init`. From within the math-module directory, run `npm link`

2. go back to `test.js`. We will run `npm link [module name]`

