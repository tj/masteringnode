
# File System

 To work with the filesystem, node provides the 'fs' module. The commands follow the POSIX operations, with most methods supporting an asynchronous and synchronous method call. We will look at how to use both and then establish which is the better option.
 
## Working with the filesystem

 Lets start with a basic example of working with the filesystem, this example creates a directory, it then creates a file in it. Once the file has been created the contents of the file are written to console:
    
    var fs = require('fs');

    fs.mkdir('./helloDir',0777, function (err) {
      if (err) throw err;

      fs.writeFile('./helloDir/message.txt', 'Hello Node', function (err) {
        if (err) throw err;
        console.log('file created with contents:');

        fs.readFile('./helloDir/message.txt','UTF-8' ,function (err, data) {
          if (err) throw err;
          console.log(data);
        });
      });
    });
  
  As evident in the example above, each callback is placed in the previous callback - this is what is referred to as chainable callbacks. When using asynchronous methods this pattern should be used, as there is no guarantee that the operations will be completed in the order that they are created. This could lead to unpredictable behavior.
 
  The example can be rewritten to use a synchronous approach:

    fs.mkdirSync('./helloDirSync',0777);
    fs.writeFileSync('./helloDirSync/message.txt', 'Hello Node');
    var data = fs.readFileSync('./helloDirSync/message.txt','UTF-8');
    console.log('file created with contents:');
    console.log(data);

  It is better to use the asynchronous approach on servers with a high load, as the synchronous methods will cause the whole process to halt and wait for the operation to complete. This will block any incoming connections and other events.

## File information
  
  The fs.Stats object contains information about a particular file or directory. This can be used to determine what type of object we
  are working with. In this example we are getting all the file objects in a directory and displaying whether they are a file or a
  directory object.

    var fs = require('fs');

    fs.readdir('/etc/', function (err, files) {
      if (err) throw err;

      files.forEach( function (file) {
        fs.stat('/etc/' + file, function (err, stats) {
          if (err) throw err;

          if (stats.isFile()) {
            console.log("%s is file", file);
          }
          else if (stats.isDirectory ()) {
          console.log("%s is a directory", file);
          }    
        console.log('stats:  %s',JSON.stringify(stats));
        });
      });
    });

 
## Watching files

  The fs.watchfile monitors a file and will fire the event whenever the file is changed.

    var fs = require('fs');

    fs.watchFile('./testFile.txt', function (curr, prev) {
      console.log('the current mtime is: ' + curr.mtime);
      console.log('the previous mtime was: ' + prev.mtime);
    });

    fs.writeFile('./testFile.txt', "changed", function (err) {
      if (err) throw err;

      console.log("file write complete");   
    });

  A file can also be unwatched using the fs.unwatchFile method call. This is used once monitoring of a file is no longer required.


## Nodejs Docs for further reading

  The node api [docs](http://nodejs.org/api.html#file-system-106) are very detailed and list all the possible filesystem commands
  available when working with Nodejs.


  


  

    
