when creating an object in ruby we get the memory address of that object back, each object has a unique identifier we can get with `obj.object_id`. 
### Operating System Heap Vs Ruby Heap 

Operating System has a large memory space called `system heap`  which use to store data, inside this memory space ruby allocate memory space for it's own called `ruby heap` , it can shrink or grow on demand. 

#### Ruby Heap 
ruby heap is made up of small units called `Pages` each page of size `16kb` that use to store ruby objects or references to ruby object inside system heap if it exceeds the size of a  page slot which is `40 byte`, each page is made of small memory units called slots, each of size `40 byte`, one the ruby heap is out of memory it can asks the system heap for memory in form of increments of pages. 

occupied slots has ruby internal C representation of ruby object called `RVALUE` and that's where the object or their reference live. 

![[heap.png]]

### Garbage's Collection 

Ruby use the Tri-color-mark-sweep Algorithm to allocate and deallocate, managing ruby heap. 
The algorithm contain of two phases, mark phase in this phase the GC marks all slots which contains accessible RVALUE, in the sweeping the GC clear all not marked slots. the ruby GC follow the STOP THE WORLD mechanism during the GC the ruby code does not get executed. 



![[GC.png]]




#### RVALUE:
- The Ruby program uses heap memory for dynamic memory allocation, with each slot comprising a 40-byte RVALUE, including a flag, Klass pointer, and object-specific fields.
- For example, for a Class object, it stores the pointer to an extension object and for a String, it stores its content.

####  Heap Pages:
- 40-byte slots are organized into 16kb memory region containers known as heap pages.
- Each heap page has 408-409 contiguous slots and is initially filled with special RVALUE type T_NONE representing empty slots.

#### Freelist:
- When the heap page is initialized, a pointer called freelist pointer is set to the address of the first slot, creating a LinkedList of empty slots.
- Ruby uses the freelist to allocate objects, keeping the object allocation operation constant in time by returning addresses of empty slots.

#### Garbage collection:
- Ruby uses the Mark-Sweep-Compact garbage collection algorithm, with phases including Marking, Sweeping, and Compaction.
- During Marking, objects and their children are marked as alive, followed by Sweeping which reclaims unmarked objects and Compaction which moves objects within the heap page to reduce memory usage.
- The garbage collector iterates over all pages, finding all slots which are not marked. Where applicable, the garbage collector then adds the unmarked slots to each page’s freelist

### Compaction step:
- Compaction uses the Compact and Free cursors to move objects within the heap page, reducing memory usage and improving write performance.
- This involves 2 steps: Compact step and Update reference step to update pointers to objects after compaction. 
- As of Ruby 3.0, if auto-compaction is enabled, compaction will actually happen as part of the sweeping phase.

#### Memory Management:
- Ruby's memory management and garbage collection mechanism were explored in this blog, providing insights into object allocation, heap pages, and the Mark-Sweep-Compact algorithm.
- The next blog will focus on Variable Width Allocation in Ruby.

#### Marked Bitmap 
The marked bitmap is a data structure used to keep track of the marked status of objects in the memory. As the collector visits each reachable object, it marks the corresponding entry in the marked bitmap. The collector can efficiently iterate through the memory space, relying on the marked bitmap to identify live objects. After the sweeping phase, the marked bitmap may be reset or cleared to prepare for the next garbage collection cycle.

#### Glossary 
- **Eden page**: A page which contains slots with RVALUES, might also have empty slots.
- **Tomb page**: A page which contains only empty slots, it may returned back to system heap.
- **Free list**: A linked list per Heap Page tracking empty slots.


### Generational GC 

# Generational Garbage Collection

Generational garbage collection (GC) is a strategy designed to take advantage of the observation that most objects in a program become unreachable shortly after allocation. This strategy divides the heap into different generations or age groups, each employing distinct collection strategies.

Generational garbage collection is based on the weak generational hypothesis, which states that most objects die young, and those that don't tend to live for a long time.

## Rails Example
Rails example to justify this hypothesis to ourselves. To generate a webpage for a client request, the Rails application will create many new Ruby objects. Once the page has been returned to the client, all of these objects are no longer needed and their space in memory can be reclaimed.

## Major and Minor GCs
Ruby's Generational GC consists of major and minor garbage collections, with minor GCs focusing on young objects and happening more frequently than major GCs. Young objects become old after surviving three garbage collections.
## Triggers for Minor vs Major GCs
Minor GCs are triggered when the Ruby Heap is full, while major GCs may also be triggered if the old object limit is crossed or manually using `GC.start`.

## Write Barriers and Remembered Set
Write barriers and remembered sets are mechanisms employed by Ruby's garbage collector to manage references between old and new objects. Write barriers ensure that all objects that are not young and not in the remembered set will not have any young references.

## Young Generation
- Newly allocated objects are placed in the young generation.
- The young generation is collected more frequently because many short-lived objects become unreachable quickly.
- Collection in the young generation is typically faster and involves copying live objects to a new space, leaving the old space mostly empty.
## Survivor Space
- Objects that survive one or more young generation collections are promoted to a "survivor space" within the young generation.
- The survivor space helps identify objects that have a longer lifespan but are not yet considered long-lived.
## Old Generation
- Objects that have survived multiple young generation collections are eventually promoted to the old generation.
- The old generation is collected less frequently because long-lived objects tend to persist for a more extended period.
- Collection in the old generation is typically more heavyweight and may involve different algorithms, such as mark-and-sweep or mark-and-compact.
##  Advantages
- **Faster Collection in Young Generation:** Collecting the young generation frequently is quick, as most objects become unreachable quickly. Techniques like copying or scavenging are employed.
- **Less Frequent Collection in Old Generation:** Reducing the frequency of collection in the old generation improves overall efficiency.
## Generational Hypothesis
- The generational hypothesis posits that most objects die young. By focusing on collecting the young generation frequently, generational garbage collection can quickly identify and collect short-lived objects, minimizing the impact on long-lived objects in the old generation.

Generational garbage collection is a widely adopted strategy in modern garbage collectors, including those used in Java (e.g., the JVM's Garbage-First Garbage Collector) and some implementations of Ruby, such as the Ruby MRI (Matz's Ruby Implementation) with its "RGenGC" (Restricted Generational Garbage Collection) introduced in Ruby 2.1.




## ObjectSpace 

`ObjectSpace` in Ruby provides a way to interact with the Ruby runtime's object allocation system. It allows you to enumerate and manipulate living objects within the Ruby process.

```ruby 
# enumeration of all living object of a specific class or all classes 
objects = ObjectSpace.each_object { |obj| puts obj } 
# count objects 
count = ObjectSpace.count_objects # return a hash of counts of all objects 
count[:TOTAL] # count total objects 
count[:T_STRING] # count of string objects

## Trace object Allocatio 
ObjectSpace.trace_object_allocations do 
	obj = Object.new 
	puts "allocated #{obj}" 
end

## Object Refernces provide the set of object that is reachable form a specifc object 
obj1 = Object.new 
obj2 = Object.new 
obj1.instance_varaiables_ser(:@ref, obj2)
references = ObjectSpace.reachable_objects_from(obj1) 
puts "referncse of obj1 #{refernces}" 

## memory size of an object 
ObejectSpace.memsize_of(obj)

```

## Resources 
### [How Ruby GC Works](https://blog.peterzhu.ca/notes-on-ruby-gc/) 

### [Detailed Explaination]([Blog | Jemma Issroff | Garbage collection](https://jemma.dev/))

### [Optimizing Memory](https://reintech.io/blog/optimizing-memory-management-in-ruby) 



