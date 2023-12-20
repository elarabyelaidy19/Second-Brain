when creating an object in ruby we get the memory address of that object back, each object has a unique identifier we can get with `obj.object_id`. 
### Operating System Heap Vs Ruby Heap 

Operating System has a large memory space called `system heap`  which use to store data, inside this memory space ruby allocate memory space for it's own called `ruby heap` , it can shrink or grow on demand. 

#### Ruby Heap 
ruby heap is made up of small units called `Pages` each page of size `16kb` that use to store ruby objects or references to ruby object inside system heap if it exceeds the size of a  page slot which is `40 byte`, each page is made of small memory units called slots, each of size `40 byte`, one the ruby heap is out of memory it can asks the system heap for memory in form of increments of pages. 

occupied slots has ruby internal C representation of ruby object called `RVALUE` and that's where the object or their reference live. 

![[heap.png]]

## [How Ruby GC Works](https://blog.peterzhu.ca/notes-on-ruby-gc/) 


## [Detailed Explanation]([Blog | Jemma Issroff | Garbage collection](https://jemma.dev/))


## [Optimizing Memory](https://reintech.io/blog/optimizing-memory-management-in-ruby) 



