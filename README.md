# MD5 WASM

MD5 WASM is an asynchronous MD5 calculator, optimized for large files.&nbsp;
It is called using a Promise-style syntax, with 'then' and 'catch'.&nbsp;
WebAssembly is seamlessly used to calculate MD5 values for files above a certain size threshold.

## Raison d'être &nbsp; (Reason for being)

### Brief Background

Our MD5 hashing was initially performed using this wonderfully simple utility:&nbsp; 
https://www.npmjs.com/package/md5&nbsp;&nbsp;
It works server-side and client-side and was really easy to use.&nbsp; 
However, being inline, it would hang our server for the duration of the calculation, a problem when processing large files.&nbsp;
Our application sees large files fairly frequently, so we built a utility which is *much* faster and runs async.&nbsp;
That is this.

### Highlights

&#9679; Fastest JavaScript MD5 utility&nbsp;&nbsp;(uses WebAssembly for large files)&nbsp;   
&#9679; Serverside (NodeJS) or clientside (browser)&nbsp;   
&#9679; Uses Promise syntax for async processing&nbsp;   
&#9679; *Only* accepts Buffer or Uint8Array as input;&nbsp; _no_ _Strings_!&nbsp;   
&#9679; Up to 256MByte file size&nbsp;   


## Javascript Calls And Parameters

### Usage example

	data      = largeArrayBufferFromSomewhere;                    // Get the data any which way you can

	md5WASM(data)                                                 // Our function
	    .then(function(hash){ console.log(hash) })
	    .catch(function(err){ console.log(err) })

## Loading MD5-WASM

At less than 32K, the code file does not justify minification.&nbsp;
It is all-inclusive and has no external dependencies.&nbsp;

### HTML tag

	<script type="text/javascript" src="path/md5-wasm.js"></script>

You will find the function at *window.md5WASM*

### In NodeJS

	md5WASM      = require("md5-wasm");

## Performance and Benchmarks

The MD5 algorithm, by definition, runs with 32 bit integers.&nbsp;
Javascript uses floating point numbers but performs bitwise operations in 32 bit numbers, converting formats in between.&nbsp;
This means there is a lot of time consumed converting number formats -- in theory.&nbsp;
WebAssembly supports 32 bit integers in native form.&nbsp;
It seemed to the author that a WebAssembly implementation would be very fast; so we did it in WebAssembly.&nbsp;

For large files (20Mbytes or more) this implementation hashes the files about 20 times as fast as the utility we were previously using (https://www.npmjs.com/package/md5).&nbsp;
At some point, we will happily perform a bit more benchmarking but this is good enough for now.

