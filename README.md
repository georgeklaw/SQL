select z.pin,count(z.pin) as pincount
from [pin] z
where z.created_time >= (now()-1)
group by pin
having count(z.pin)>=3 and count(z.pin)>(select 3* avg(a.count) as avg
from
(select count(*) as count, b.created_time
from
[pin] b
where b.pin in  (
select x.pin
from [pin] x
where x.created_time between now()-1 and now()-8 and x.pin in (
select q.pin
from [pin] q
where q.created_time >= (now() -1)
group by pin
having count(q.pin)>=3
)
group by pin
)
group by b.created_time
)
as a)
