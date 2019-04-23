```sql
select
  ip.province,
  round(sum(case when score.ad_id = '_read_news_reward' then score.score else 0 end)/100,2) as 领取新闻红包金额,
  round(sum(case when score.ad_id = '_read_news_ad_reward' then score.score else 0 end)/100,2) as 领取广告红包金额
from 
  (
  select
    regexp_extract(ip,'(\\d+\\.\\d+\\.\\d+)\\.(\\d+)',1) as ips,
    ad_id,
    score
  from tb_z_score_log
  where p_date = '20180509'
    and action_type = '0'
    and ad_type = '68'
    and ad_id in ('_read_news_reward','_read_news_ad_reward')
    ) score 
left join 
  (
  
  select
    distinct
    regexp_extract(ip,'(\\d+\\.\\d+\\.\\d+)\\.(\\d+)',1) as ips,
    regexp_extract(province,'(.*?)[省|市]{0,1}',1) as province
  from fdm_z_ip_log
    ) ip on score.ips = ip.ips
group by ip.province
order by 领取广告红包金额 desc
limit 100   
```