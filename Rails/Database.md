
# Active Record 
- be aware of what you send to activerecord because it build an object for each row fetched for the database, so fetch what you need only. 
- 

```ruby 
Person.all.joins(:role).where(roles: {billable: true})  
Role.where(billable: true) 

def slef.billable 
	where(billable: true)  
end 

```