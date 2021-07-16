
# Postfix 教学文档
> 参考地址：https://wiki.centos.org/zh/HowTos/postfix  

### 1. 引言  

这篇文章的对像是希望设置一台基本邮件服务器的初学者。拥有基本系统管理知识会有帮助，而能够安装软件及编辑配置文件是必须的。这篇文章是针对 CentOS 5 所撰写的，但亦应该适用於更早版本。其后版本也许会有差异。

一台邮件服务器的设置可以有很多不同的实例和组合（多至不能在此尽录），因此这篇文章为你作了一些基本的选择，例如我们将会采用的软件（postfix 及 dovecot）。其它选项则需要用户更改，例如你的网络地址及域名。虚拟网域或用户等高级选项已超越了这篇文章的范畴，因而不会在这里处理。

这篇文章采用 postfix 作为邮件传输代理（MTA）。dovecot 是用来容让用户通过 imap 或 pop 协议来访问他们的邮件。我们会假设域名是 example.com，它应该由读者更改，这可以是给一台正式邮件服务器用的真实域名，或者只是供内部邮件服务器用的虚构域名。我们假设实体的邮件服务器（主机）是 mail.example.com，并且位於 192.168.0.1 这个私人 IP 地址（这应该按读者的需要作出修改）。这台邮件服务器将会通过标准的系统帐户来提供邮件帐户，而用户将会利用他们的系统帐户及口令来访问他们的邮件。我们会假设有一位用户名叫 John Smith，它拥有一个名为 john 的系统帐户。

### 2. 安装  

我们首先要做的事情就是安装所需的软件。最简单的做法就是在命令行上采用 yum：


yum install postfix dovecot system-switch-mail system-switch-mail-gnome
yum 应该会自动解决任何依赖性的需要。dovecot 依赖 mysql 及 perl，因此它们若未被安装在系统上，现在很可能便会。

另外，我们可省略 'system-switch-mail' 及 'system-switch-mail-gnome' 的安装和删除缺省的 'sendmail' MTA，这样便会令 'postfix' 成为我们系统的缺省 MTA。


yum install postfix dovecot
yum remove sendmail
请留意 CentOS 5 的缺省 MTA 是 sendmail。要是你没有把 postfix 改为缺省的 MTA，更新 postfix 时也许会把 sendmail 恢复为缺省的 MTA。

### 3. 设置  

接下来我们需要设置邮件服务器的各部份。

#### 3.1. Postfix  

postfix 的配置文件是存储在 /etc/postfix 之内。postfix 的两个主要配置文件是 master.cf 及 main.cf，虽然我们在这里只会处理 main.cf。我们会首先在 main.cf 配置文件内作出添加及修改。以下内容需要被添加、编辑或解除注释：

```
myhostname = mail.example.com
mydomain = example.com
myorigin = $mydomain
inet_interfaces = all
mydestination = $myhostname, localhost.$mydomain, localhost, $mydomain
#这里的网络注意下，如果不行就配置成0.0.0.0/0
mynetworks = 192.168.0.0/24, 127.0.0.0/8
relay_domains =
home_mailbox = Maildir/
```
注：每行文字应该放置行首，而且前面不应有空格或定格字符。以空格或定格字符起首的行会被视为上一行的延续，假如上一行是注释（#），接著的那一行亦会被同样看待。此外，请避免采用内置的注释。

现在让我们查看每个设置来理解我们刚才所做的事情：

myhostname：这是系统的主机名称（例如：系统名叫 mail 或 mail.example.com）。

mydomain：这是邮件服务器的域名（它可以是真实或虚构的域名）。

myorigin：这是本地邮寄的电邮发放及投递时用的域名。

inet_interfaces：它设置 postfix 可以用来接收邮件的网络界面。它们最少要包括 localhost 及 本地网域。

mydestination：这是可投递的网域（换言之，这台服务器是寄往这些网域的邮件的最终目的地）。

mynetworks：这是获信任可以通过服务器来发放或转发邮件的 IP 地址。当来自这些 IP 地址以外的用户尝试通过服务器发放邮件时，便会被拒绝。

relay_domains：这是本系统会把邮件转寄到的网域的清单。通过将它设置为空白，我们确保这台邮件服务器不会成为未受信任的网络的公开转发站。我们推荐读者在这里测试他们的系统并非一个公开的转发站：http://www.abuse.net/relay.html

home_mailbox：它设置信箱相对用户根目录的路径，以及指定要采用的信箱格式。postfix 同时支持 Maildir 及 mbox 格式，而我们鼓励读者阅读有关每个格式的优点。然而，在这篇文章内我们选用了 Maildir 格式（一个尾随的斜线代表 Maildir 格式。要指定 mbox 格式，读者需要采用 home_mailbox = Mailbox）。

#### 3.2. Dovecot  

dovecot 的配置文件位於 /etc/dovecot.conf。以下内容需要被添加、编辑或解除注释：

```
protocols = imap imaps pop3 pop3s
mail_location = maildir:~/Maildir
pop3_uidl_format = %08Xu%08Xv
# Required on x86_64 kernels
login_process_size = 64
```
让我们再次查看每个选项：

protocols：它指定可供用户访问邮件的协议。dovecot 支持 imap(s) 及 pop3(s) ，你可应用部份或所有协议。

mail_location：它指定每位用户的信箱格式及位置。在这里我们采用 maildir 格式，而且每位用户的信箱是放在 ~/Maildir。配置文件内已经提供了 mbox 格式的样例。

pop3_uidl_format：这是用来修正 Outlook 2003 通过 pop3 访问信箱时出现的问题，因此加入它是很合理的（详情请参阅配置文件内的备注）。

login_process_size：CentOS 5.1 的发行注记声称「当 x86_64 内核上的 dovecot 组件升级至 CentOS 5.1 后，/etc/dovecot.conf 内必须加入 login_process_size = 64 这个参数」。32 位元的安装并不受影响，及不需要这个设置。

注：假如你通过 imap 或 pop3 与 dovecot 连接时出现任何问题，请查阅 dovecot.conf 配置文件内的 IMAP specific settings 及 POP3 specific settings 部份有关权宜之计。它所备有的选项多数是关乎旧电邮客户端，以及针对微软 Outlook 和 Outlook Express 的权宜之计。

关于 dovecot 及 C6 的备注：CentOS 6 把配置文件转到 /etc/dovecot/dovecot.conf。Dovecot 在不必对配置文件作出任何修改的情况下已经能引导及自动聆听 pop3(s) 和 imap(s) 端口上的连接。你很可能要作出修改来迎合你的环境。

#### 3.3. 创建用户的信箱  

接著我们须要在每位用户的根目录内置立一个信箱及设置适的权限，因此以 john 这个用户为例：

```
mkdir /home/john/Maildir
chmod -R 700 /home/john/Maildir 
```
#### 3.4. 别名  

我们快要完成了。我们已经为以 john 登录的 John Smith 用户创建了一个邮件帐户。他的电邮地址是 john@example.com 。然而，John 或许会希望通过 jsmith@example.com （或者其它别名）接收邮件。我们可以利用系统的别名文件（postfix 缺省使用 /etc/aliases）为 John 设置一个别名来达至这个目的。我们亦可以为其它用户加入别名，譬如我们可以在 /etc/aliases 加入以下内容，将 root 的电邮转发给 John：

```
# 要接收 root 的电邮的人
root:           john
# 用户别名
jsmith:         john
j.smith:        john
```
如果你在 postfix 运作时编辑别名文件来为用户设立新的别名，你必须执行 newaliases 这个指令来重建别名数据库。

### 4. 引导服务器
现在我们已经准备好引导新的邮件服务器。首先我们要告诉我们的系统以 postfix 取代缺省的 sendmail 作为 MTA。要这样做，执行 system-switch-mail 这个指令并选择 postfix 作为 MTA。这样便会安装 postfix 服务并将它设置在 runlevel 3、4、及 5 下自动引导。然后我们亦须设置 dovecot 在 runlevel 3、4、及 5 下自动引导，并引导这两个服务：

```
chkconfig --level 345 dovecot on
/etc/init.d/dovecot start
/etc/init.d/postfix start
```
此刻一切应该已在运作。你的邮件服务器不论是发送及接收内部的电邮，或是发送对外的电邮都应该没有问题。要在你的网域上接收外来的电邮，你必须在你的网域的 DNS 内设置 MX 记录（最理想是通过你的 ISP 额外设置一个 PTR rDNS 将你的网域映射至你的 IP 地址）。切勿忘记在你的 Linux 防火墙上根据你所执行的服务打开端口（SMTP 25；POP3 110；IMAP 143；IMAPS 993；POP3S 995），并在所有路由器上为这些端口启用转接功能。

如果你对 postfix 的 main.cf 配置文件作出任何改动，你可以选择重新引导 postfix 服务，或执行 postfix reload 这个指令来更新这些改动。

### 5. 总结
postfix 是一个非常强劲及多用途的邮件传输代理。在这篇文章内我们看见如何利用 postfix 及 dovecot 为一个网域设立一台应用系统帐户的基本邮件服务器。我们并未认真地探究 postfix 系统的性能，但盼望这里提供了一个基础让新用户能够创建在其上。

现在我们鼓励读者阅读相关的 postfix 规限指南。

### 6. 连结
我们鼓励读者阅读 postfix 网站上收藏的大量 postfix 文件，及所包含的众多样例设置：

http://www.postfix.org/documentation.html

测试邮件服务器是否被设成公开转发站：

http://www.abuse.net/relay.html

### 7. 注意事项
postfix+dovecot的用户名和密码是系统的用户名和密码，不能关闭系统的密码验证。

### 8. 使用遇到的问题
postfix日志报错NOQUEUE：reject：RCPT from unknown  
解决办法：  
```
vim /etc/dovecot/main.cf
#这里的网络注意下，如果不行就配置成0.0.0.0/0  
mynetworks值修改成：  
mynetworks = 0.0.0.0/0  
```
    