#### [Ruby IO](https://thoughtbot.com/blog/io-in-ruby)  

## IO 

IO is a terms that covers the ways that a computer interacts with the world, Screen, Keyboard, files, sockets are all forms of IO, data are sent to and from those programs as a stream of characters/bytes. 

Unix-likes systems treat all those devices as files, under `/dev` directory in file system. 
IO streams located under `/dev/fd` files there are given a number known as file descriptor. 

the operating system provides three streams by default  
- **standard input**:  reading from keyboard
- **standard output:** writing to the terminal 
- **standard error:**  writing to the terminal 


