#Timely - Lightweight timing of JavaScript functions

When developing in JavaScript you often find yourself in situations where you want to measure the time it takes for a function to execute. Profilers can find this out but if you just want to measure the time for a function or two you might want to try Timely. Timely doesn't affect the parameters or return values of the functions that it measures. Instead it *decorates*  the existing functions with a timing functionality that is completely transparent to the callers.

##Installation

Either...

	npm install timely

...or clone/download Timely from github.

##Usage (in Node)

	var timely = require('timely'),

		// Synchronous function (Fibonacci)
		fib = function(n) {
			if (n <= 2) return n;
			return fib(n - 1) + fib(n - 2);
		},

		// Asynchronous function
		wait = function(n, cb) {
			setTimeout(function() {
				cb(n);
			}, 100);
		},

		// Create a timed verion of the Synchronous funciton
		fibT = timely(fib),
		// Create a timed verion of the Asynchronous function
		waitT = timely.async(wait),
		
		// Result of Synchronous function
		resultSync;

	// Call Synchronous function
	resultSync = fibT(35);
	//Output results of Synchronous function. fibT.time contains the time used for the last call
	console.log('fib(35) = ' + resultSync + ', time: ' + fibT.time + 'ms');

	// Call Asynchronous function
	waitT(42, function(resultAsync) {
		//Output results of Asynchronous function. fibT.time contains the time used for the last call
		console.log('wait(42) = ' + resultAsync + ', time: ' + waitT.time + 'ms');
	});

Outputs:

	fib(35) = 14930352, time: 201ms
	wait(42) = 42, time: 101ms

##Demo
To run Node demo:

    node demo.js
    
To run Browser demo:

    Open demo.html in your favorite browser

##API
* [timely](#timely)
* [timely.async](#timely.async)

##Functions

<a name="timely"/>
### timely( func )
  
Returns a decorated version of a synchronous function func that measures its execution time. After the function has returned the measured time is available in the returned function's property "time".

__Arguments__

    func     {Function} Function to be timed

---------------------------------------

<a name="timely.async"/>
### timely.async( func )
  
Returns a decorated version of an asynchronous function func that measures its execution time. After the callback has been called the measured time is available in the returned function's property "time". It is assumed that the callback is the last parameter of the function func.

__Arguments__

    func     {Function} Function to be timed

---------------------------------------

##Tests
Run tests with nodeunit:

     nodeunit test
     
##License 
(MIT License)

Copyright (c) 2012 Aron Kornhall

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

