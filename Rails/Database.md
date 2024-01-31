
# Active Record 
- be aware of what you send to activerecord because it build an object for each row fetched for the database, so fetch what you need only. 
- 

```ruby 
Person.all.joins(:role).where(roles: {billable: true})  
Person.joins(:role).merge(Role.billable)
Role.where(billable: true) 

def slef.billable 
	where(billable: true)  
end 


def slef.billable 
	joins(:role).merge(Role.billable)
end 

Person.select { |p| p.role.billable? } # N+1 
Person.includes(:role).select { |p| p.role.billable? } # prevent N+1
```



## Migrations 
- add self-referential association 
```ruby 
add_refernces :pople, :manager, foreign_key: {to_table: :people}  
```