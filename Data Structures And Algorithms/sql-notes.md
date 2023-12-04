## Coalesce  
- used to return the first non null value from list, otherwise return 0 
```sql 
select coalesce(price, 0) -- if there is a non null value return else return 0 
from orders 
```

## WITH  
- use to create temporary table resulted form query on the fly

```sql 
with t1 as ( 
    select ad_id, sum(case when action in ('Clicked') then 1 else 0) as Clicked 
    from Ads 
    group by ad_id
)
``` 


## Lead and Lag Over() 
- `Lead(column, NnextRow) Over() as alias` use to return the Nth row after current in this column 
- `Lead(column, NnextRow) Over() as alias` use to return the Nth row before current in this column
```sql 


with report as ( 
    select seat_id, free, 
    lead(free, 1) Over() as next, 
    lag(free, 1) oVer() as prev
    from cinema
)

select seat_id 
from report r
where r.free = true and (r.prev = true or r.next = true)
order by seat_ids
```