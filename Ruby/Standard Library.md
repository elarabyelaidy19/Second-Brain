
# Zlib  
[Zlib](https://ruby-doc.org/stdlib/libdoc/zlib/rdoc/Zlib.html) is a compression library in Ruby that provides interfaces for the zlib and gzip compression and decompression algorithms. It is part of the Ruby Standard Library. 

### Key Features of Zlib:
- **Gzip Compression:**
    - Zlib supports gzip compression, a widely used compression format for files and data on the web.
- **Zlib Compression:**
    - Zlib provides the zlib compression library, which is a lower-level interface for compression and decompression.
- **In-Memory Compression:**
    - Zlib allows you to perform in-memory compression and decompression, which is useful for scenarios where you don't want to write compressed data to disk.

## Usage 
```ruby 
require 'zlib' 
# compress in zlib format 
data = "May I can be smaller" 
compressed_data = Zlib::Deflate.deflate(data) 
decompressed_dat = Zlib::Inflate.inflate(data) 

# gzib format 
compressed_string = StringIO.new
compressed_dat = Zlib::GzibWriter.wrap(compressed_string) { |gz| gz.write(data) } 
puts compressed_string # print compressed data 

decompressed_data = Zlib::GzibReader.new(compressed_data).read 


```

### **When to Use Zlib and Gzip in Ruby:**

- **Zlib:** Use Zlib when you need a general-purpose compression library for in-memory compression and decompression tasks.
- **Gzip:** Use Gzip when you are dealing with compressing and decompressing files, especially in the context of Unix-like systems.