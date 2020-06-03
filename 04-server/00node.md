# Intro to Node.js

![https://upload.wikimedia.org/wikipedia/commons/thumb/d/d9/Node.js_logo.svg/590px-Node.js_logo.svg.png](https://upload.wikimedia.org/wikipedia/commons/thumb/d/d9/Node.js_logo.svg/590px-Node.js_logo.svg.png)

---

### Objectives
- Explain what Node.js is & why it exists
- Run a simple javascript file from node.js

---

## What is Node.js?

![](http://jce-il.github.io/ASOSMA/firefox-ios/general.jpg)

![](https://i.imgur.com/Qgz5eFD.png)

The makers of Node.js took javascript (which normally only runs in the browser) and made it available in your computer (on the server side). They took Google's V8 JavaScript Engine and gave it the ability to run directly inside the computer, not just in the browser.


#### Interactive Node

If you simply type node in terminal, you will launch Node's REPL (Read-Eval-Print-Loop) interactive utility. It works similar to the chrome dev tools console. Let's test it:

```js
node

> 10 + 5
// 15

> const a = [ 1, 2, 3];
// undefined

> a.forEach(function(v) {
... console.log(v);
... });
// 1
// 2
// 3

> process
// [an object with a long list of properties that comes together with node]
```

Press control-c twice to exit REPL.

---

#### Executing node javascript- creating a command line program

Write and execute some code in a file!

In your working directory:

`mkdir first-node`
`cd first-node`
`touch main.js`
`sublime main.js`

Paste into the file:
```
console.log("hello");
```

`node main.js`

---

## [Process](https://nodejs.org/api/process.html#process_process)

#### We are changing the running environment of the JS code from the browser to the "server" (your computer).

In node, "Process" refers to one CPU execution environment. It is the basic, top-level environment context of your running code.

You can see this by opening the activity monitor application on your mac. (Or Task Manager on a windows computer)

The `process` object is a `global` that provides information about, and control over, the current Node.js process.

As a global, it is always available to Node.js applications without using `require()`. Two most commonly used `process` property are `process.argv` and `process.env`.

#### `process == window`

In the browser this running context is one browser tab- it is represented by the global `window` object.

#### process.argv
The `process.argv` property returns an array containing the command line arguments passed when the Node.js process was launched. The first element will be [process.execPath](https://nodejs.org/api/process.html#process_process_execpath).

The second element will be the path to the JavaScript file being executed. The remaining elements will be any additional command line arguments.

For example, assuming we're launching this node project:
```
$ node process-args.js one two=three four
```  
the output will be:
```
0: /usr/local/bin/node
1: /Users/mjr/work/node/process-args.js
2: one
3: two=three
4: four
```

---

### Unix environment variables

When we write apps, we have certain global "environment" variables that are different for each context or **installed environment**. -The enviroment that you are running the app in.

Examples:

- a variable for whether or not the app is running on your mac computer, or on a server, or on a "cloud" virtual server.
- a variable that holds the API credentials for accessing a Google API - (one for testing and one for "production")
- a variable that hold the location of a certain configuration file or any disk location: `/Application/Google\ Chrome` etc.

#### process.env

The process.env property returns an object containing the user environment. Typically, the object looks like this.

```
{
  TERM: 'xterm-256color',
  SHELL: '/usr/local/bin/bash',
  USER: 'maciej',
  PATH: '~/.bin/:/usr/bin:/bin:/usr/sbin:/sbin:/usr/local/bin',
  PWD: '/Users/maciej',
  EDITOR: 'vim',
  SHLVL: '1',
  HOME: '/Users/maciej',
  LOGNAME: 'maciej',
  _: '/usr/local/bin/node'
}
```

Practically, updating the `env` property will allow programs to run different coding flow depending on different development environment.


For example:
```
// given the process.env.NODE_ENV === 'development'

// setting two different database url for different environment

if(process.env.NODE_ENV === 'development') {
  api_key = "test-key"
} else {
  api_key = "LK987KLJJ0989KJHK$#SDD";
}
```

---

### Pairing Exercises:

##### Setup:

1. Use the `node` command to open the REPL and do some basic math.

1. Write a basic node program that console.logs something
```
touch index.js
```
Write something like:
```
console.log("hello");
```
Run it:
```
node index.js
```

##### Exercise 1:

1. `console.log` the value of `process.argv` in your program

##### Exercise 2:

1. Write a basic node program that takes user input and `console.log`s it back out to the user:


Run this on the command line:
```
node index.js pizza
```

Look at `process.argv` - it's an array. How do you access `pizza` ?

Console log just that part, it should look like:
```
pizza
```

In your code, in order to get the command argument (pizza), you will need:
```
process.argv[2]
```

What happens when you run this:
```
node index.js noodles
```

##### Exercise 3:

What is the value of `process.argv[0]` and `process.argv[1]`?

1. Add the ability to put a second argument for your command line program. `console.log` that as well.

1. Add the ability to add an unlimited amount of extra arguments to your program. `console.log` each one

1. Create a nodejs command line program that takes 2 arguments and adds them together.

##### Further:

1. Create a nodejs command line program that takes arguments and adds them all together.

1. Create a nodejs command line program that takes arguments and averages them out.

1. Create a nodejs command line program that takes arguments, adds a random number and averages them out.
