DNS域名解析：把域名解析成IP地址。

如果用户的浏览器缓存中没有，浏览器会查找操作系统系统中是否有这个域名对应的DNS解析结果。
在操作系统中，也有一个域名解析的过程，在windows中通过C:\Windows\System\drivers\etc\hosts
文件来设置，你可以将任何域名解析到任何能够可以访问的地址。所以把hosts文件设置成只读。访问这个文件被
轻易修改，导致域名被劫持。

如果在本机仍然无法完成域名解析，就会真正请求域名服务器来解析这个域名了。
如何知道域名服务器呢？我们的网络配置中都会有DNS服务器地址这一项。这个地址就用于此，操作系统会把
这个域名发送给这里设置的LDNS，就是本地区的域名服务器。这个DNS通常都提供给你本地互联网接入的一个
DNS解析服务。

如果你在学校接入互联网，那么你的DNS服务器就在学校，如果你在一个小区接入互联网的，那么这个DNS就是提供给你
的互联网的应用提供商，既电信或者联通，通常所说的SPA，那么这个DNS通常也会在你所在城市的角落，通常不会很远。
在window下，可以通过ipconfig /all进行查看

如果LDNS还是没有命中，就直接到Root Server域名服务器请求解析。

根域名服务器返回给本地域名服务器一个所查询域的主域名服务器（gTLD Server）地址。gTLD是国际顶级域名服务器，如果.com .cn .org等。全球只有13台。

本地域名服务器（Local DNS Server）再向上一步返回的gTLD服务器发送请求。

接受请求的gTLD服务器查找并返回此域名对应的Name Server域名服务器的地址，这个Name Server通常就是你注册的域名服务器，例如你在某个域名服务提供商申请的域名，这个域名解析任务就由这个域名提供商的服务器来完成。

Name Server域名服务器会查询存储的域名和IP映射关系表，在正常情况下都根据域名得到目标IP记录，连同一个TTL值返回给
DNS Server域名服务器。

返回该域名对应的IP和TTL值，Local DNS Server会缓存这个域名和IP对应关系，缓存时间由TTL控制。

把解析的结果返回给用户，用户根据TTL值缓存在本地系统缓存中，域名解析结束。

在实际的过程中，可能Name Server有多级，或者有GTM来负载均衡控制。

![image](https://user-images.githubusercontent.com/97614802/184052746-6f29c0da-91b8-4913-b11b-7a8b64511ef8.png)

##### 几种域名解析方式

* A记录，A代表的Address，用来指定域名对应的IP地址，如将item.taobao.com指定到115.238.23.xxx，将switch.taobao.com指定到121.14.24.xxx。A记录可以将多个域名解析一个IP地址，但是不能将一个域名解析到多个IP地址。

* MX记录，表示的是Mail Exchange，就是可以将某个域名下的邮件服务器指定自己的Mail Server，如taobao.com的域名的A
记录IP地址是115.234.23.xxx,如果将MX记录设置为115.234.23.xxx，既xxx@taobao.com的邮件路由，DNS会将邮件发送到
115.234.23.xxx所在的服务器，而正常通过Web请求的话，仍然解析到A记录的的IP地址。

* CNAME记录，全称是Canonical Name（别名解析）。所谓的别名解析就是可以为一个域名设置一个或者多个别名。如将taobao.com解析到xulingbo.net,将srcfan.com解析到xulingbo.net。其中xulingbo.net分别是taobao.com和srcfan.com的别名。

* NS记录，为某个域名指定DNS解析服务器， 也就是这个域名有指定的IP地址的DNS服务器去解析。

* TXT记录，为某个主机名或者域名设置说明
