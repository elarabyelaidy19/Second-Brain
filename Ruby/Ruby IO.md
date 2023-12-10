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

writing Wikipedia front page to a local file  
```ruby 
require 'rest-client' 

wiki_url = "http://en.wikipedia.org/" 
wiki_file = "wiki_local.html" 
File.open(wiki_file, 'w') do |f| 
	f.puts(RestClient.get(wiki_url))
end 

```

### Reading from a file 
```ruby 
file = File.open('file.txt', 'r') # open the file in read mode 
content = file.read # load the entire file content, when reading again on the same it will starts from where the previous read ends 
puts content 
content = file.read 
puts content # will out "" 
```

### Reading Lines 
#### readlines Vs readline  
- readlines: draw all the content of the file parse it as an array, splitted by line break 
- readline: read singular line at a time, each read ops move the file handle forward in the file. if keep calling readline hit end of file, you will get and `end of file error` 
- using readline is more efficient because you will not load the entire file into memory at once. 
```ruby 
require 'open-uri'
url = "http://ruby.bastardsbook.com/files/fundamentals/hamlet.txt"
hamlet = "hamlet.txt"

File.open(hamlet, 'w') do |f|
    f.puts(URI.open(url).read)
end

File.open(hamlet, 'r') do |f|
    f.readlines.each_with_index do |line, idx|
        puts line if idx % 42 == 41
    end
end

```
### Closing File 
File write operations don't happen instantaneously, as disk access is bound by at least the laws of physics. As data queues up to be written, [it sits in a memory buffer before actually being written to the hard disk](http://en.wikipedia.org/wiki/Disk_buffer "Disk buffer - Wikipedia, the free encyclopedia").  

"flush" is good practice in programming because it frees up memory for the rest of your program and (ideally) ensures that that file is available for other processes to access.  

when  pass a block into `File.open`. At the end of the block, the file is automatically closed:

```ruby  
datafile = File.open("sample.txt", "r")
lines = datafile.readlines         
datafile.close
lines.each{ |line| puts line }    
```

### File Existing and Properties 

```ruby 
File.exists?('filename') # check whether the file exits? or not 
dir_name = 'notes' 
Dir.mkdir(dirname) unless File.exists?(dir_name) 
File.open("#{dir_name}/new_note.md", 'w') { |f| f.puts "hello world" }
```

## Dir 


## StringIO 
## SocketIO  


