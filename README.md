![CLEVER DATA GIT REPO](https://raw.githubusercontent.com/LiCongMingDeShujuku/git-resources/master/0-clever-data-github.png "李聪明的数据库")

# 如何使用TSQL搭建SQL数据库邮件
#### How To Configure SQL Database Mail With Only TSQL
**发布-日期: 2015年7月1日 (评论)**

![#](images/how-to-configure-sql-database-mail-with-only-tsql-a.png?raw=true "#")

## Contents

- [中文](#中文)
- [English](#English)
- [SQL Logic](#Logic)
- [Build Info](#Build-Info)
- [Author](#Author)
- [License](#License) 


## 中文
这里有一些快速的sql逻辑，可以用来搭建SQL数据库邮件，也就是sp_send_dbmail进程。


## English
Here’s some quick sql logic you can use to configure the SQL Database Mail aka sp_send_dbmail process.

---
## Logic
```SQL
-- configure sql database mail with the sql logic below. 
-- 通过下面的sql逻辑来搭建 sql database mail。
-- enable sql database mail profile 
-- 启用sql数据库邮件资料
exec master..sp_configure 'show advanced options',1
reconfigure;
exec master..sp_configure 'Database Mail XPs',1
reconfigure;
go
 
-- creating a profile创建一个资料
execute msdb.dbo.sysmail_add_profile_sp
    @profile_name       = 'SQLDatabaseMailProfile'
,   @description        = 'SQLDatabaseMail' ;
 
-- create a mail account.创建一个邮箱账号
execute msdb.dbo.sysmail_add_account_sp
    @account_name       = 'EmailAddressYouWantToAppearHere'
-- 你想要在此处显示的电子邮件地址&lt;
-- Does not have to be a real email aaddress. 
-- 不需要真实的邮件地址 
-- Parameters are required for process, but are not needed to work. 
-- 此过程需要参数，但不需要可执行。
,   @email_address      = 'EmailAddressYouWantToAppearHere'
-- 你想要在此处显示的电子邮件地址&lt;
-- Does not have to be a real email aaddress. 
-- 不需要是真实的邮件地址  
-- Parameters are required for process, but are not needed to work.
-- 此过程需要参数，但不需要可执行。

,   @mailserver_name    = 'MySMTPServerNameGoesHere' 
--, @port               = MyPort 					--optional
--, @enable_ssl         = 1 						--optional
--, @username           ='SQLDatabaseMailProfile' 	--optional
--, @password           ='MyPassword' 				--optional
 
-- adding the account to the profile  - 将帐户添加到资料中
execute msdb.dbo.sysmail_add_profileaccount_sp
    @profile_name       = 'SQLDatabaseMailProfile'
,   @account_name       = 'sqldatabasemail@mydomain.com'
,   @sequence_number    = 1 ;
 
-- grant access to 授予访问权限 DatabaseMailUserRole
execute msdb.dbo.sysmail_add_principalprofile_sp
    @profile_name       = 'SQLDatabaseMailProfile'
,   @principal_id       = 0
,   @is_default         = 1 ;
 
-- send email. 发送邮件
EXEC msdb.dbo.sp_send_dbmail
    @profile_name       = 'SQLDatabaseMailProfile'
,   @recipients         = 'MyEmailAddress'  &lt;
-- Distribution group always preferred here.
,   @subject            = 'Database Mail Subject...'
,   @body               = 'Database Mail Body...';
 
 
-- check to see if mail is being qued up to be sent.  检查邮件是否排队等待发送。
-- several seconds after you see email sent to your inbox. 
-- 看到发送到收件箱的电子邮件几秒钟后。

select * from msdb..sysmail_allitems 


```



[![WorksEveryTime](https://forthebadge.com/images/badges/60-percent-of-the-time-works-every-time.svg)](https://shitday.de/)

## Build-Info

| Build Quality | Build History |
|--|--|
|<table><tr><td>[![Build-Status](https://ci.appveyor.com/api/projects/status/pjxh5g91jpbh7t84?svg?style=flat-square)](#)</td></tr><tr><td>[![Coverage](https://coveralls.io/repos/github/tygerbytes/ResourceFitness/badge.svg?style=flat-square)](#)</td></tr><tr><td>[![Nuget](https://img.shields.io/nuget/v/TW.Resfit.Core.svg?style=flat-square)](#)</td></tr></table>|<table><tr><td>[![Build history](https://buildstats.info/appveyor/chart/tygerbytes/resourcefitness)](#)</td></tr></table>|

## Author

- **李聪明的数据库 Lee's Clever Data**
- **Mike的数据库宝典 Mikes Database Collection**
- **李聪明的数据库** "Lee Songming"

[![Gist](https://img.shields.io/badge/Gist-李聪明的数据库-<COLOR>.svg)](https://gist.github.com/congmingshuju)
[![Twitter](https://img.shields.io/badge/Twitter-mike的数据库宝典-<COLOR>.svg)](https://twitter.com/mikesdatawork?lang=en)
[![Wordpress](https://img.shields.io/badge/Wordpress-mike的数据库宝典-<COLOR>.svg)](https://mikesdatawork.wordpress.com/)

---
## License
[![LicenseCCSA](https://img.shields.io/badge/License-CreativeCommonsSA-<COLOR>.svg)](https://creativecommons.org/share-your-work/licensing-types-examples/)

![Lee Songming](https://raw.githubusercontent.com/LiCongMingDeShujuku/git-resources/master/1-clever-data-github.png "李聪明的数据库")

