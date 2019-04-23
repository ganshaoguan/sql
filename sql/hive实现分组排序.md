```sql
select
   *
from 
  (
    select
      a.p_month,
      a.invite_code,
      a.user_num,
      row_number() over(partition by a.p_month order by a.user_num desc) as rank_num
    from 
    (
      select
        substring(p_date,1,6) as p_month,
        invite_code,
        count(distinct uid) as user_num
      from  table  
      where p_date >='20170701'
      group by substring(p_date,1,6),invite_code
       ) a 
    ) b 
where b.rank_num <= 10
limit 100
```