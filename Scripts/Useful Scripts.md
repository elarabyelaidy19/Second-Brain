 - count numbers of line of codes in a code base   

```shell 
find . -name '*.rb' | xargs wc -l
```

- list process that takes specific port, and then kill it using PID 
	```shell 
	lsof -i :3000 
	kill -9 PID	
	```

- run rails on different port `
```shell 
rails s -p PortNumber
```

- remove all local branches except main 
```shell 
git branch | grep -v 'main' | xargs git branch -D
```


