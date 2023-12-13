
## Resources 

## Review 

#### [Ruby IO From thoughtbot](https://thoughtbot.com/blog/io-in-ruby)   
#### [File IO The Bastards Book of Ruby](http://ruby.bastardsbook.com/chapters/io/) 



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
file1 = File.new('note.md') # create file and open it for read 

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
f1 = File.open('note.md', 'r+') # read/write mode starts at the beginig of the file 
f1 = File.open('note.md', 'w+') # truncate file if it exists or create a new one 
f1 = File.open('note.md', 'a+') # append open file W/R start at the end of the file

# Locate bytes by position 
f1 = File.open('notes.md') 
f1.seek(5) # return the first 5 character starts from 0 the beging of the file
str = f1.gets

f1.seek(30) 
pos = f1.pos  # 30 
f1.seek(20, IO::SEEK_CUR) # seek 20 charcter from the pos of the file 30 
pos2 = f1.pos # 30 + 20 

f1.rewind # rest the file pointer at the begining 
```

### Reading Lines 
#### readlines Vs readline  
- readlines: draw all the content of the file parse it as an array, splited by line break 
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
the `Dir` class provides a way to interact with directories in the file system. It includes methods for listing the contents of a directory, creating directories, and performing various operations related to file paths.

#### Dir methods and Use cases
```ruby  
entries = Dir.entries('dir paths') # list all files in the specified dir 
files = Dir.glob('/notes/*.md') # return an array of all md files in the dir 
Dir.mkdir(dir_name, mode="permision") # create dir with specified permision 
Dir.delete(dir_name) 
Dir.chdir(dir_name) { } # change the current dir for the block 
Dir.pwd # return the current working dir 
Dir.exists?(dir_name) 

# Use cases 
# list files only in dir 
files = Dir.entries('/home/elaraby').reject { |f| File.directory?(f) } 

# iterate over files in a dir 
Dir.foreach('/home/elaraby') do |f| 
	puts "file name is: #{f}" 
end 

# change the current dir and list all the files in it 
Dir.chdir('/mnt/c/books/engineering') do 
	md_files = Dir.glob('*.md') 
	puts "notes: #{md_files}"
end 

# create directory if it is not exists? 
Dir.mkdir('/notes') unless Dir.exists?(/notes) 

# Count files in dir and subdirs 
Dir.glob('/mnt/c/books/Engineering/**/*').length
Dir.glob('./**/*').length # count files in current and subdir

# Find the top 10 largest files in a dir 
Dir.glob('./**/*').sort_by { |f| File.size(f) }.reverse.take(10). each do |fname| 
	puts "#{fname}\t#{File.size(fname)}"
end  

```

#### Files Stats 
```ruby 
# Statistics about files 
hash = Dir.glob('./**/*').inject({}) do |hsh, fname| 
	file_ext = File.extname(fname).downcase[1..-1] # extract the extinsion name 
	hsh[file_ext] ||= [0, 0] # for each file extension create [count, size] if not exists 
	hsh[file_ext][0] += 1  # increase count by one 
	hsh[file_ext][1] = File.size(fname)  
	hsh
end 

# wriet stat to file 

File.open('file_stats.txt', 'w') do |f| 
	hahs.each do |ext, (count, total_szie)| 
		line = "#{ext}\t#{count}\t#{total_size}" 
		f.puts line 
		pust line 
	end
end 
```


##### Exercise: Read a text file and create a Google Chart
[Exercie](http://ruby.bastardsbook.com/chapters/io/#:~:text=Exercise%3A%20Read%20a%20text%20file%20and%20create%20a%20Google%20Chart)



## FileUtils 

## StringIO  
## SocketIO   

