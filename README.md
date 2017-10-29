## What you should know? 
- JS
- HTML
- CSS

please refer to the following courses on lynda.com : 
- Learning ES6
- ES6: the right parts 
- The Good Parts of JavaScript and the Web

## Transpiling ES6
 
<b>When to use what, in my opinion?</b>

-npm scripts: very large projects or a lot of projects that share common tasks; A lot of developers contributing to these projects with different backgrounds; Companies working on different projects;

-Webpack: very straightforward website projects; Quick delivery time; Low amount of developers;

-Gulp: straightforward / less straightforward projects; for teams that have a shared knowledge for one of those task runners; single, one-off projects;

### Transpiling with Webpack (Webpack + Babel)
Check the installation guide of babel and webpack here : https://babeljs.io/docs/setup#installation

Webpack is a build tool that helps us load all of our dependencies, CSS, images, node modules and more, and babel and webpack work really well together, and can help automate the process of performing these conversions.

Webpack is a build tool that helps us load all of our dependencies, CSS, images, node modules and more, and babel and webpack work really well together, and can help automate the process of performing these conversions.

```
cd my-project 
npm init
```
Now the package.json file is our module documentation, so it's going to contain information about our project, as well as all of our dependencies. The first of these dependencies that we're going to install with our terminal is going to be webpack itself.

```
sudo npm install -g webpack 
```

So now that this is installed, we also want to install babel. 
```
sudo npm install --save-dev babel-loader babel-core
```
So babel is going to help us process that es6 and turn it into es5.  

Create an env preset, which enables transforms for ES2015+: 
```
sudo npm install babel-preset-env --save-dev
```


Next create a .babelrc config file 
```
vim .babelrc
```
Input the following in .babelrc: 
```javascript
{
    "presets": ["env"]
  }
```

Next create a webpack config file

```
vim webpack.config.js
```
Input the following in webpack.config.js: 
```javascript
module.exports = {
	entry: './script.js',
	output: {filename: 'bundle.js'},
	module: {
		loaders: [
			{test: /\.js?/, loader: 'babel-loader', exclude: /node_modules/}
		]
	}
};
```
Once we give that a save, we can switch back over to our terminal, where we'll run webpack.

```
webpack
```
The job is done! 
Webpack is transforming script.js (ES6) to bundle.js (ES5). So you must create a script.js file before running webpack, and in this script.js file you are free to write some ES6. 

### Transpiling with Gulp (gulp + babel) 
Gulp is a command line task runner utilizing Node.js platform. It runs custom defined repetitious tasks and manages process automation.

The transpilation process is the same as with webpack. Check babel's documentation to install it with gulp: https://babeljs.io/docs/setup#installation
https://gulpjs.com/

installation: 
```  
sudo npm install -g gulp
sudo npm install gulp-cli -g

Then install gulp at the package level 
sudo npm install --save-dev gulp 
sudo npm install --save-dev gulp-babel
sudo npm install --save-dev gulp-sourcemaps
sudo npm install --save-dev gulp-concat
```

Usage:
create a gulp file: 
```
touch gulpfile.js
```
INSIDE gulp file: 
```javascript 
var gulp = require("gulp");
var sourcemaps = require("gulp-sourcemaps");
var babel = require("gulp-babel");
var concat = require("gulp-concat");

gulp.task("default", function () {
  return gulp.src("src/**/*.js")
    .pipe(babel())
    .pipe(gulp.dest("dist"));
});

gulp.task("map", function () {
  return gulp.src("src/**/*.js")
    .pipe(sourcemaps.init())
    .pipe(babel())
    .pipe(concat("all.js"))
    .pipe(sourcemaps.write("."))
    .pipe(gulp.dest("dist"));
});
```

finally run gulp or gulp map and you're done
```
gulp
gulp map
```
### Transpiling with NPM scripts (NPM scripts + babel) Preferred method!
install babel-cli locally 
```
sudo snpm install --save-dev babel-cli
```

Usage: 
Instead of running Babel directly from the command line we’re going to put our commands in npm scripts which will use our local version.

Simply add a "scripts" field to your package.json and put the babel command inside there as build.
```javascript
package.json: 

  {
    "name": "my-project",
    "version": "1.0.0",
+   "scripts": {
    "build-js": "babel ./src/**/*.js --watch --out-dir ./dist --source-maps" 
+   },
    "devDependencies": {
      "babel-cli": "^6.0.0"
    }
  }
```
Nb: Check the cli usage documentation : https://babeljs.io/docs/usage/cli/

Now from our terminal we can run:
```
npm run build-js
```
Now whenever you make changes to js files the script will keep "watching" your changes and transpile them. 


## ES6 Synthax
### Let Keyword
When dealing with scope in JavaScript, we have a new keyword, let, that we can use to declare variables. 

We use the let keyword to create block scoping in JavaScript in locations where we weren't able to do so before. 

My recommandations: 
- Only use let when needed! for instance in for loops and if conditions. Using var in the global scope is much better than let for a simple reason that it will confuse the programmer that is reading your code 


Example1: 
```javascript
//without let
var x = 10; 
if (x) { 
    var x = 5;
}
console.log(x); 
//OUTPUT: 5

//with let
var x = 10; 
if (x) { 
    let x = 5;
}
console.log(x); 
//OUTPUT: 10
```

Example2: 
```javascript
<head>
	<style type="text/css">
		section > div {
			height: 100px;
			width: 100px;
			background-color: red;
			float: left;
			margin: 3px;
			cursor: pointer;
		}
    </style>
</head>

<body>
	<header>
		<h1>Click on a box</h1>
	</header>
	<section></section>
</body>	

//Without let: 
		for(var i= 0; i<45; i++){
			var div = document.createElement('div');
			div.onclick = function() {
				alert("you clicked on a box #" + i);
			};
			document.getElementsByTagName('section')[0].appendChild(div);
        }
//OUTPUT: all div will alert 45

//With let:
		for(let i= 0; i<45; i++){
			var div = document.createElement('div');
			div.onclick = function() {
				alert("you clicked on a box #" + i);
			};
			document.getElementsByTagName('section')[0].appendChild(div);
		}        
//OUTPUT: the problem is fixed! 
```

### Const Keyword
Much like the let keyword, we can use const as an alternative when declaring variables. The const keyword is short for constant and allows us to set constant variables that shouldn't be reassigned.

My recommendations: 
- Note: Let and Const are both variable scope declarations. The difference between let and const is that const declares a "constant" which is by definition a variable that CAN'T BE RE-ASSIGNED, but its value can still change though. 

So if you want to declare a variable which its value don't change, you don't need to use const. You simply use the following: 
```javascript
var x = Object.freeze([1,2,3]); 
// so now the values of the array can't be changed! 
// you don't need the const keyword here! 
```

Example: 
```javascript
const name = "fred"; 

// you can't reassign the constant variable "name" another value! So you can't write name="something else" or var name = "something else" or let name = "something else" 
```

### Template Strings
Template strings allow you to tap into the functionality of template languages to format your JavaScript code with variables.

Example: 
```javascript
function createEmail(firstName, purchasePrice) {
			var shipping = 5.95;
			console.log(
				`Hi ${firstName}, Thanks for buying from us!
					Total: $${purchasePrice}
					Shipping: $${shipping}
					Grand Total: $${purchasePrice + shipping};
				`	
				);
		}

		createEmail("Gina", 100);

//OUTPUT: Template strings allow you to tap into the functionality of template languages to format your JavaScript code with variables.
```

### Spread Operator
The spread operator can turn the elements of an array into arguments of a function call, or into elements of an array literal. 

Example: 
```javascript
//Without Spread operator
		var cats = ["Tabby", "Siamese", "Persian"];
		var dogs = ["Golden Retriever", "Pug", "Schnauzer"];

		var animals = ["Whale", "Giraffe", cats, "Snake", dogs, "Coyote"];

		console.log(animals);

/*
if we console log animals, we should see these items, but notice here we see whale, giraffe, array three snake, array three coyote. What we're doing here is we're seeing the dogs and cats appear as a subarray, an array within an array. If we want to add the array elements to the new array, we can use the spread operator. Instead of just using the variable name, I'm going to have each of these variables preceded by three dots.
*/

//With spread operator 
		var cats = ["Tabby", "Siamese", "Persian"];
		var dogs = ["Golden Retriever", "Pug", "Schnauzer"];

		var animals = ["Whale", "Giraffe", ...cats, "Snake", ...dogs, "Coyote"];

		console.log(animals);

```

### Maps
The map is an object that holds key value pairs, so what's the difference between a map and a regular object? Well, in a map, any value may be used as either a key or a value. So, why might you want to use a map? Well, you might want to use one if you want to use something other than a string as a key. In a map, a function can be a key, an array can be a key, a number could be a key.

We also could use a map to utilize key value pairs that are always the same values. Finally, another good reason to use a map is if we need to iterate in order. Unlike objects, the map object iterates its elements in insertion order.

A Map is basically an array of arrays. 

Example: 
```javascript

		var course = new Map();
		course.set('react', {description: 'ui'});
		course.set('jest', {description: 'testing'});

		console.log(course);
		console.log(course.get('react'));

		var details = new Map([
			[new Date(), 'today'],
			['items', [1, 2]]
		]);

		console.log(details.size);

		details.forEach(function(item) {
			console.log(item);
        });
```

### Sets 
Set objects are collections of values. They can be of any type, but each value must be unique.

Example1: 
```javascript

		var books = new Set();
		books.add('Pride and Prejudice');
		books.add('War and Peace')
			 .add('Oliver Twist');

		console.log(books);
		console.log('how many books?', books.size);
		console.log('has Oliver Twist?', books.has('Oliver Twist'));
		books.delete('Oliver Twist');
		console.log('has Oliver Twist still?', books.has('Oliver Twist'));

```


Example2: 
```javascript

		var data = [4,2,4,4,2,5,1,6,7,5,6,8,2,7];
		var set = new Set(data);
		console.log('data.length', data.length);
		console.log('set.size', set.size);


```
So, let's take a look at this in our browser. We should see that data.length is 14, but that set.size is seven. So, what we're doing here in our set is, when we create a set using this data, even though it starts out with 14 items, by the time it is turned into a set, all of the duplicates are removed.


### For of loop 
For-of is a new loop in ES6 that we can use to loop over iterable objects like arrays, strings, maps, and sets. 

Example: 
```javascript
for (let letter of 'JavaScript') {
			console.log(letter);
		}

		var topics = ['JavaScript', 'Node', 'React'];

		for (let topic of topics) {
			console.log(topic);
		}

		var topics = new Map();
		topics.set('HTML', '/class/html');
		topics.set('CSS', '/class/css');
		topics.set('JavaScript', '/class/javascript');
		topics.set('Node', '/class/node');

		for (let topic of topics.keys()) {
			console.log(topic, "is the course name");
		}

		for (let topic of topics.values()) {
			console.log("The course description can be found at", topic);
		}

		for (let course of topics.entries()) {
			console.log(course);
		}

```


## ES6 Functions and Objects 
### Default function parameters 
One of the easiest to implement features of ES6 is using default parameters in your functions. 
Example: 
```javascript
		function haveFun(activityName="hiking", time=3) {
			console.log(`Today I will go ${activityName} for ${time} hours.`)
		}
        haveFun("biking", 5);
        haveFun(); //will console log default parameters 
        haveFun("cycling"); //will console log default parameter for time which is 3 and cycling 
```

### Enhancing object literals 
ES6 enhancements give us ways of shortening and simplifying object literals.

Example: 
```javascript
//In ES5
//join is going to take the elements of the array and join them together as a string. So the value that I pass into the join method will be the character or the word that joins the array items together. 
        var cat = { 
            meow: function(times) { 
                console.log(Array(times+1).join("meow")); 
            }
        }
 		cat.meow(3);       

//In ES6:

//All right, so the first thing I want to do here is I'm going to remove the function keyword as well as the colon. We no longer need this in ES6, so quite a bit simpler already. 

//So, repeat() is a new ES6 method, and it's going to construct and return a new string. The number of times it's repeated is defined by the value that's passed to it.
		var cat = {
			meow(times) {
				console.log("meow".repeat(times));
			}
		};

		cat.meow(3);
```

### Arrow functions
Arrow functions, sometimes called fat arrow functions, have an abbreviated syntax for working with functions. 

Arrow functions provide us with some syntactic sugar. 

It's also some sort of lambda function if you're familiar with the term. 

My recommendation: 
- Never use arrow functions everywhere! It will confuse you and sometimes it won't work. Why? Because the nature of an arrow function is as follows: the arrow function ONLY accepts an expression and NOT A STATEMENT. Please refer the internet to understand the difference between an expression and a statement in JS. 
- In my opinion the only case where arrow functions should be used is to resolve the "this" problem in JS. Actually arrow functions were created to specifically solve this issue. 

Example1: 
```javascript
        //form1: standard function
        function func1(param1) {
            console.log(param1); 
        }
        func1("hello form1"); 

        //form2: using an anonymous function
        var func1 = function(param1) { 
            console.log(param1);
        }
        func1("hello form2"); 

        //form3: using an Arrow function (ES6)
        //subform1: 
        var func1 = (param1) => {
            console.log(param1); 
        } 
        func1("hello form3 subform1 with the curly braces"); 
		
		//subform2
        var func1 = (param1) => console.log(param1);
        func1("hello form3 subform2 without the curly braces"); 
```

We reviewed how arrow functions can make our code more readable. In addition, arrow functions can help us deal with the scope of the 'this' keyword in our JavaScript code. Let's take a look at an example of some old ways that we dealt with the scope of this, and how arrow functions can make this much easier. 

Example2: Resolving the this scope with arrow functions
```javascript
//Problem: 
var person = { 
    first: "Doug", 
    actions: ['bike','hike', 'ski', 'surf'], 
    printActions: function() { 
        this.actions.forEach(function(action) { 
            var str = this.first + " likes to " + action; 
            console.log (str); 
        }); 
    }
};
person.printActions();
//Solution with _this variable:
var person = { 
    first: "Doug", 
    actions: ['bike','hike', 'ski', 'surf'], 
    printActions: function() { 
        var _this = this; 
        this.actions.forEach(function(action) { 
            var str = this.first + " likes to " + action; 
            console.log (str); 
        }); 
    }
};
person.printActions();

//Solution with .bind(this):

var person = { 
    first: "Doug", 
    actions: ['bike','hike', 'ski', 'surf'], 
    printActions: function() { 
        this.actions.forEach(function(action) { 
            var str = this.first + " likes to " + action; 
            console.log (str); 
        }.bind(this)); 
    }
};
person.printActions();

//Solution with arrow functions: 
		var person = {
			first: "Doug",
			actions: ['bike', 'hike', 'ski', 'surf'],
			printActions() {
				this.actions.forEach(action => {
					var str = this.first + " likes to " + action;
					console.log(str);
				});
			}
		};
		person.printActions();

```


### Destructuring assignment 
Destructuring Assignment gives us an easy way to extract data from arrays and objects and assign them to variables.

Example1: 
```javascript
		var vacation = {
			destination: "Chile",
			travelers: 2,
			activity: "skiing", 
			cost: 4000
		};

		function vacationMarketing({destination, activity}) {
			return `Come to ${destination} and do some ${activity}`
		}

        console.log(vacationMarketing(vacation));
```

Example2: 
```javascript
        var [first,,,fourth,] = ["one", "two", "three", "four", "five"]; 
        console.log(fourth); 
```

### Generators
Generators are a new type of function that allow us to pause functions in the middle of execution, to be resumed later. You know you're looking at a generator when you see an asterisk immediately following the function keyword. Sometimes you'll see people use the asterisk right before the function name. We hit pause within our function by using the new yield keyword. And this can be used multiple times within the same function. 

Remember, each time we use a yield statement, we need to restart the function. And to do this, we're going to use the next function. So the next function is going to jump to the next yield.

Example: 
```javascript
function* eachItem(arr) {
			for(var i=0; i< arr.length; i++) {
				yield arr[i];
			}
		}

		var letters = eachItem(["a", "b", "c", "d", "e", "f", "g"]);

		var abcs = setInterval(function(){
			var letter = letters.next();
			if(letter.done) {
				clearInterval(abcs);
				console.log("Now I know my ABC's");
			} else {
				console.log(letter.value);
			}
		},500);
```            

### Symbols 
The building blocks of all of our JavaScript applications are JavaScript primitives. Primitives in JavaScript are types, like numbers, strings, arrays, and objects. Symbols are a new primitive type in ES6. Symbols are often used as unique identifiers because they won't conflict with object keys that are strings. Symbols also help us to create customized iteration 

Example: 
```javascript
/*
So, here in our file, we're going to create a new variable called id and we're going to set this equal to a symbol. So symbol is going to be a capital S and we're going to create it by using this factory function. Factory functions are simply functions that create things, in this case, a symbol: Symbol(). 
*/
		const id = Symbol();
		const courseInfo = {
			title: "ES6",
			topics: ["babel", "syntax", "functions", "classes"],
			id: "js-course"
		};

		courseInfo[id] = 41284;
		console.log(courseInfo);

/*
So this becomes particularly interesting if we add another field here, another key, called id to our courseInfo object. So let's go ahead and do this. I'm going to give this an id, we're going to say js-course. So, if another developer comes in and adds this id to the courseInfo object, we would typically find some sort of conflict behavior happen here, but this is why the symbol is pretty cool.

If we refresh this, we're going to see that we have title, topics, id, js-course, and the symbol is still intact. So there's no naming conflict here, and we don't have any problems overriding any data. We can also use the symbol type to create our own iterators,
*/
```

### Iterators
Processing each of the items in a collection is a very common programming task.

So iteration in nothing new, but the ability to examine, define, and customize iteration behavior is new with ES6. With ES6 we have two new protocols to take advantage of.

The iterable protocol which allows JavaScript objects to define or customize their own iteration behavior and the iterator protocol, a standard way to produce a sequence of values from a collection. An object is an iterator when it understands how to access each item in a collection. It also keeps track of where it is currently positioned in the collection. 

Example: 
```javascript
ar title = 'ES6';
		console.log(typeof title[Symbol.iterator]);

		var iterateIt = title[Symbol.iterator]();
		console.log(iterateIt.next());
		console.log(iterateIt.next());
		console.log(iterateIt.next());
		console.log(iterateIt.next());

		function tableReady(arr) {
			var nextIndex = 0;
			return {
				next() {
					if(nextIndex < arr.length) {
						return {value: arr.shift(), done: false}
					} else {
						return {done: true}
					}
				}
			}
		}

		var waitingList = ['Sarah', 'Heather', 'Anna', 'Meagan'];
		var iterateList = tableReady(waitingList);

		console.log(`${iterateList.next().value}, your table is ready`);
		console.log(`${iterateList.next().value}, your table is ready`);
		console.log(`${iterateList.next().value}, your table is ready`);
		console.log(`${iterateList.next().value}, your table is ready`);
		console.log(`Is this finished? ${iterateList.next().done}`);


/*

 Symbol.iterator is a method that returns the default iterator for an object. Or a factory of iterators. So the type of title symbol iterator is going to be different. It's going to be a function.

So what I'm going to do here is create a function called tableReady. tableReady is going to take in an array and it's going to have a variable nextIndex which will start out being set to zero.

Next up we're going to return a new definition for the next function. So we'll say Next and we're going to define some custom behavior here. So we'll say if nextIndex is less than the length of the array, array.length, we then will return an object where value is set to array.shift which just pops off that first item in the array and it will also return done is false.

 */


```

## Asynchronous Features
A Synchronous function is a function that does not return until the work is completed or has failed.

Asynchronous functions return immediately. You call it, it comes right back, there is almost no passage of time. 

Check the following [Understanding Callback Functions] (http://javascriptissexy.com/understand-javascript-callback-functions-and-use-them/) 	

### Promises
Promises have emerged in the S6 to help us deal with asynchronous behavior in JavaScript. When something is asynchronous it means that some sort of waiting is going on. There's a delay between when we ask for something and when we receive it. 