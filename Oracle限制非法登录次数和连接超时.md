# Oracle限制非法登录次数和连接超时自动退出

首先检查default profile设置：

`
SQL> select * from dba_profiles where profile = 'DEFAULT';
`

结果：

    DEFAULT    CONNECT_TIME                 KERNEL        UNLIMITED

    DEFAULT    FAILED_LOGIN_ATTEMPTS        PASSWORD      UNLIMITED

    DEFAULT    PASSWORD_LOCK_TIME           PASSWORD      UNLIMITED

FAILED_LOGIN_ATTEMPTS、PASSWORD_LOCK_TIME、CONNECT_TIME、IDLE_TIME都应该是UNLIMITED，也就是默认无限制。

修改：

`
SQL> ALTER SYSTEM set RESOURCE_LIMIT=true SCOPE=both;
`

`
SQL> ALTER PROFILE DEFAULT LIMIT FAILED_LOGIN_ATTEMPTS 5 PASSWORD_LOCK_TIME 1 CONNECT_TIME 60;
`

FAILED_LOGIN_ATTEMPTS(登录尝试失败次数)，改为5次，PASSWORD_LOCK_TIME(账号锁定时间)改为1天。
CONNECT_TIME(session连接时间)改为60分钟。

## 参考

[How to get the Values Assigned by Default to a Profile in Oracle Database
](https://www.thegeekdiary.com/how-to-get-the-values-assigned-by-default-to-a-profile-in-oracle-database/)

[Terminating Oracle connections with connect_time, idle_time, expire_time and inbound_connect_timeout](http://www.dba-oracle.com/t_connect_time_idle_expire_timeout.htm)

[Oracle Password Security](http://www.dba-oracle.com/t_password_security.htm#:~:text=failed_login_attempts%20%2D%20This%20is%20the%20number,the%20password_life_time%20limit%20is%20exceeded.)
