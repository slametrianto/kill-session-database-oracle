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
