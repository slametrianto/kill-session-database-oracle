# kill-session-database-oracle

select
   (select username from v$session where sid=a.sid) blocker,
   a.sid,
   ' is blocking ',
   (select username from v$session where sid=b.sid) blockee,
   b.sid
from
   v$lock a,
   v$lock b
where
   a.block = 1
and
   b.request > 0
and
   a.id1 = b.id1
and
   a.id2 = b.id2;
   
   
  # kill user
  select inst_id,sid,serial# from gv$session where username='SCOTT';
  
  alter system kill session '130,620,@1';
  
  
  => Detecting blocking locks in Oracle RAC database
  
select s1.inst_id||':'||s1.sid||':'||s1.serial# BLOCKING_INST_SESS_SER#,' IS BLOCKING ' ACTION,s2.inst_id||':'||s2.sid||':'||s2.serial# BLOCKING_INST_SESS_SER#,round(s1.last_call_et/60) for_minutes 
from gv$lock l1, gv$lock l2, gv$session s1,gv$session s2
where l1.block >0 and l2.request > 0
and l1.id1 = l2.id1    
and l1.id2 = l2.id2
and l1.sid = s1.sid
and l1.inst_id = s1.inst_id
and l2.sid = s2.sid
and l2.inst_id = s2.inst_id
order by l1.inst_id;
