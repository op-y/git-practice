# Github的SSH连接配置
一般情况下，不做任何配置就进行连接github库（git clone）的操作，会出现如下的语句：
```
Warning: Permanently added the RSA host key for IP address '192.30.252.129' to the list of known hosts.
Permission denied (publickey).
fatal: Could not read from remote repository.）
```
这是因为没有配置ssh。
所以请移步到 Hunter同学的博客，按照他的博文[《Github之SSH连接配置》](http://www.linmuxi.com/2016/02/24/github-config-ssh/)，进行github 的SSH连接配置，祝你成功！