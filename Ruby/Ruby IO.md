#### [Ruby IO](https://thoughtbot.com/blog/io-in-ruby)  

## IO 

IO is a terms that covers the ways that a computer interacts with the world, Screen, Keyboard, files, sockets are all forms of IO, data are sent to and from those programs as a stream of characters/bytes. 

Unix-likes systems treat all those devices as files, under `/dev` directory in file system. 
IO streams located under `/dev/fd` files there are given a number known as file descriptor. 

the operating system provides three streams by default  
- **standard input**:  reading from keyboard, it's read-only 
- **standard output:** writing to the terminal, write-only.
- **standard error:**  writing to the terminal, write-only.  

- to create IO object you need a file descriptor 
```ruby 
io = IO.new(1) # creating IO object attaching it with fd 1(stdout) 
io.puts "hello world"

```
- to create IO to other streams/files you need a file descriptor first 
```ruby 
fd  = IO.sysopen('/dev/null', '+w') # create a fd to a stream 
stream = IO.new(fd) # create IO object with attached fd 
stream.puts "hello world" # write to the stdout 
stream.close # after you finish you should close stream to release system resources 

```


IO class in ruby is the parent of all IO classes 
## FileIO

The File class supplies the basics methods of manipulate files. 

```ruby 
fname = "file.txt" # create file
somefile = File.open(fname, 'w') # open the file with write mode, using w on an existing file will erase the content, to append on the file, use "a" as the second arg
somefile = File.open(fname, 'a') # append to the existing file
somefile.puts("hello world")  # write to the file, you can also use write which does not include newline at the end 
somefile.close # close the file to prevent any further operations on the file.

```


## StringIO 
## SocketIO  


