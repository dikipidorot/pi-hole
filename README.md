##  【更新日志-2024-11-10】
- 重新整理 群晖&新绿联&华硕 的compose，他们三的路径都是一样的，完全通用，好事
- 因为本人没有华硕NAS的设备，compose 味精测试，但应该冇问题，一些细节后续再去慢慢修
- 华硕的第一个用户的权限：uid=1000(用户名) gid=100(users) groups=100(users),999(administrators)
- 新增华硕NAS的路径和端口说明，端口注意如果模板有冲突，自行修改

[历史更新内容](https://github.com/FrozenGEE/compose/blob/main/WHAT'S_OLD.md)

## 【一堆肺话】
- 分享compose模板，方便新人，老手快速部署docker容器；因unraid模板而想写，但unraid模板只能用于unraid上，对于其他nas系统并不通用，而compose模板通用性很好
- 本人模板强烈建议使用portainer进行编写，其他工具自己执衫，其实大同小于的，不用担心太难
- 部分nas需要SSH才可以部署portainer，因为需要有权限访问docker的核心，但这一些nas的webui上无法选中路径，所以需要使用ssh命令进行部署，部署命令看文档后面
- 模板上很多变量可能很不简化（不小心故意的），还有大量注释，方便理解参数的用途，以及有用的信息，并且废话有点多.....
- 模板很多都是以外链接，host网络模式为主的，个人偏向于能自定义容器内部端口就自定义，然后host网络模式
- 而外链接是指针对于数据库的调用，我看了官方教程compose之类的教程后而个人修改的，因为个人不喜欢创建太多的docker容器，数据库要是为每一个docker容器单独创建，就会产生大量的数据库容器
- 大量的容器部署了，对于除了unraid和casaos以外的不带webui管理面板的nas系统，倒是无妨，毕竟看不见为净，但是我尽量能外链接就外链接，所以有些带有数据库管理的docker容器需要自己先提前在数据库容器中创建好对应的子数据库和子账号密码，再去compose中设置好（注意看注释说明）
- 后续也会搞一份专门的内置关联，啊.....开局就挖坑了，GBF了，还有好多没写π_π
- 并且暂时分享通用模板，后续分享个人认为合适的现成compose参数配置模板，直接搞上去即可使用，做法类似casaos商店那样一键部署（但还是要自己稍微检查修改）

## 【docker的一些小知识】
- docker的更新其实就是把当前部署好的docker容器先卸载了，然后使用相同的配置单，再去部署一个新的容器，如果你仔细观察可以发现，docker容器的ID每一次更新都会变化

- 所以利用这个特性，只要你做好路径映射，把必要的东西映射到nas的实际路径上，如果你的nas要转移，docker部署的容器，你只要把映射出来的东西都打包到新的nas上，按照原来的配置单，稍微修改对应实际情况，就可以完美恢复（portainer可以在左侧“volume”/“卷”中看到哪些没有映射，并且可以删除残留卷）
- 这也是我个人用群晖，比起套件，更加优先用docker的原因，而且我也很推荐这么做

## 【NAS的路径说明】
- 模板上会有很多【这里替换为你的docker数据目录】这些文字，一般每一个docker镜像都会有自己配置数据文件夹存在，用于存放容器自己的配置文件，所以很建议集中存放，见上说明，以下内容仅供参考，你只要理解了，其实都能知道这是什么意思，部分路径待后续补充，本人遗忘了。
> aaa代表docker容器的配置文件目录，例如emby的"/config"映射为/xxx/emby，ddnsgo的"/root"映射为/xxx/ddns-go，alist的"/opt/data/alist"映射为/xxx/alist等等，用于专门存放他们的配置文件

> bbb代表某个文件夹的名字，或者数据存放的目录，例如媒体库，相册，数据下载目录等

>  *代表第几个存储池，根据实际情况来写，后续更新针对nas专用的适配模板将会默认都用/volume1，请根据实际情况修改

```shell
💡unRAID的docker配置文件存放路径
/mnt/user/appdata/aaa
💡unRAID的数据存放路径开头
/mnt/user/共享文件夹/bbb

熟悉unRAID的玩家会知道另一个绝对路径写法
/mnt/disk*/共享文件夹/bbb
```

```shell
💡群晖的docker配置文件存放路径
/volume*/docker/aaa
💡群晖的数据存放路径开头
/volume*/共享文件夹/bbb
```

```shell
💡威联通的docker配置文件存放路径
/share/Container/aaa
💡威联通的数据存放路径开头
/share/共享文件夹/bbb

💡另一种写法
/share/CACHEDEV*_DATA/共享文件夹/bbb
对于这两种写法，模板以第一种写法为主，因为创建好第一个存储池，会预设创建好部分文件夹，因此模板将会以这些文件夹作为预设的模板，
```

```shell
💡TrueNAS的docker配置文件存放路径
/mnt/共享文件夹/docker/aaa
💡TrueNAS的数据存放路径开头
/mnt/共享文件夹/bbb

共享文件夹需要自己设置好
```

```shell
💡铁威马的docker配置文件存放路径
/Volume*/User/docker/aaa
💡铁威马的数据存放路径开头
/Volume*/User/bbb
```

```shell
💡华硕(华芸)的docker配置文件存放路径
/volume*/Docker/aaa
💡华硕(华芸)的数据存放路径开头
/Volume*/共享文件夹/bbb

```

```shell
💡CasaOS的docker配置文件存放路径
/DATA/AppData/aaa
💡CasaOS的数据存放路径开头
/DATA/共享文件夹/bbb
```

```shell
💡fnOS的docker配置文件存放路径
/vol*/用户id/docker/aaa
💡fnOS的数据存放路径开头
/vol*/用户id/bbb

用户id，如其意思，第一个用户为即1000，创建第二个用户则是1001，第三个是1002，如此类推

第一个用户的第一个存储池的共享文件夹路径是/vol1/1000
第一个用户的第二个存储池的共享文件夹路径是/vol2/1000
第三个用户的第二个存储池的共享文件夹路径是/vol2/1002
...
类推
```

```shell
💡万由的docker配置文件存放路径

💡万由的数据存放路径开头

(待补充...)
```

```shell
💡绿联旧系统(基于op)的docker配置文件存放路径
/mnt/media_rw/存储池序列号/.ugreen_nas/用户ID/docker/aaa
另一种写法
/mnt/dm-*/.ugreen_nas/用户ID/docker/aaa
💡绿联旧系统(基于op)的数据存放路径开头
/mnt/media_rw/存储池序列号/.ugreen_nas/用户ID/文件夹名字/bbb
另一种写法
/mnt/dm-*/.ugreen_nas/用户ID/data/bbb

第一种写法是根据webui上创建docker后使用portainer查看所得知的，一般推荐这种，只是相对来说查找比较麻烦，所以更推荐另一种写法
另一种写法中dm-* 中的*代表第几个存储池，从0开始算起，但实际你自己是怎么设置的请视情况修改

由于每个人的存储池序列号和用户ID不一样，因为无法写出现成模板，[全通用]的模板可以使用
没什么值得保留的就趁早上新系统吧
```

```shell
💡绿联新系统(基于debian)的docker配置文件存放路径
/volume*/docker/aaa
💡绿联新系统(基于debian)的数据存放路径开头
/volume*/共享文件夹/bbb
务必把创建好一个名为docker的共享文件夹

💡绿联新系统（基于debian）的个人文件夹存放路径（系统默认）
/volume*/@home/xxx/yyy
另一个写法：/home/xxx/yyy
xxx为用户名，包括管理员和普通用户，yyy为在此之下所创建的文件夹，其中包括了每一个用户的相册文件夹“Photos”
```

```shell
💡极空间系统的docker配置文件存放路径
/tmp/zfsv3/sata$/手机号码/data/docker/aaa
/tmp/zfsv3/nvmel$/手机号码/data/docker/aaa
/tmp/zfsv3/sata$/手机号码+字母/data/docker/aaa
/tmp/zfsv3/nvmel$/手机号码+字母/data/docker/aaa

💡极空间系统的数据存放路径开头
/tmp/zfsv3/sata$/手机号码/data/bbb
/tmp/zfsv3/nvmel$/手机号码/data/bbb
/tmp/zfsv3/sata$/手机号码+字母/data/bbb
/tmp/zfsv3/nvmel$/手机号码+字母/data/bbb

"nvme*"和"sata*"根据自己实际情况修改，$为数字；"手机号码"为个人手机号码，到处都是手机号，真他妈恶心
如果你有第二台极空间，并且用同一个手机号绑定注册，则为需要在手机号码后添加上a-z的字母
例如：1688888888，1688888888a，1688888888b，1688888888c，这样类推

由于每个人的硬盘类别不一样，手机号也是个人隐私，因为无法写出现成模板，[全通用]的模板可以使用
```

## 【NAS默认端口说明】
只列举WebUI，WebDAV，SSH

💡debian/ubentu/unRAID/CasaOS/OMV/TrueNAS等众多linux系统的WebUI http和https端口为80和443，ssh端口为22
| NAS/端口 | WebUI| WebDAV | SSH | 其他 |
| :----: | :----: | :----: | :----: | :----: |
| unRAID | 80/443 | 无 | 22 |
| TrueNAS | 80/443 | 待补充 | 22 |
| OMV | 80/443 | 无 | 22 |
| CASAOS/ZimaOS | 80/443 | 无 | 22 |
| 群晖 | 5000/5001 | 5005/5006 | 22 |
| 威联通 | 8080/5443 | 5005/5006 | 22 |
| 铁威马 | 8181 | 5005/5006 | 9222 |
| 华硕 | 8000/8001 | 9800/9802 | 22 |
| 飞牛OS | 8000 | 5005/5006 | 22 |
| 新绿联 | 5000/5001 | 5005/5006 | 22 |
| 旧绿联 | 9999 | 待补充 | 922 |
| 极空间 | 5055/5056 | 5005/5006 | 可自定义<br>最低10000 | 8050 |

## 【各nas portainer 部署】
- 汉化版镜像：6053537/portainer-ce
- 官方镜像：portainer/portainer-ce

> 请见上说明根据自己实际情况修改，以下以中文版为例，如果9000端口冲突，就把冒号前面的9000修改掉

> 使用教程👇
- https://article.juejin.cn/post/7127050260710424589
- https://juejin.cn/post/7345379917821411347
- https://zhuanlan.zhihu.com/p/617947859

- 以下命令，请按照实际修改，均为以第一个存储池/volume1的共享文件夹/docker，作为docker的数据存放路径

### unraid商店就有模板，推荐直接安装部署即可，要用中文版把存储库名字替换一下就可以用汉化版即可。

### 群晖7.2开始自带compose编写，不用装，但你也可以安装套件或者用群晖compose/cli来安装，但请预先在/volume1/docker上创建好一个portainer-zh的文件夹，群晖不会自动创建不存在的文件夹
### 群晖，绿联，华硕，铁威马自带的套件都支持compose编写，unRAID有插件支持但是交互体验个人觉得不好用，CasaOS自带导入compose后转换为自己的模板，也可以导出
```shell
docker run -d -p 9000:9000 --name=portainer-zh --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v /volume1/docker/portainer-zh:/data 6053537/portainer-ce
```

### 威联通
```shell
docker run -d -p 9000:9000 --name=portainer-zh --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v /share/container-station-data/portainer-zh:/data 6053537/portainer-ce
```

### casaos，商店中就可以一键安装，但非汉化版
```shell
docker run -d -p 9000:9000 --name=portainer-zh --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v /DATA/AppData/portainer-zh:/data 6053537/portainer-ce
```

### debian/ubuntu/armbian(其实随便自定义路径都可以，这里写和casaos一样)
```shell
docker run -d -p 9000:9000 --name=portainer-zh --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v /DATA/AppData/portainer-zh:/data 6053537/portainer-ce
```

### 绿联旧系统
(记得把"用户ID"替换掉)
```shell
docker run -d -p 9000:9000 --name=portainer-zh --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v /mnt/dm-0/.ugreen_nas/用户ID/docker/portainer-zh:/data 6053537/portainer-ce

```

### 绿联新系统(和群晖一样)
```shell
docker run -d -p 9000:9000 --name=portainer-zh --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v /volume1/docker/portainer-zh:/data 6053537/portainer-ce
```

### 极空间docker用的是魔改的portainer，官方没有开放ssh，但可以通过一些途径获取到，然而本人的Z2pro（rk3568）使用portainer会和极空间的魔改portainer冲突，x86的根据网友表示不会，所以这里修改portainer的启动参数为不自启动，以免有问题
[2024-09-28更新] 现在arm也不会冲突重启了，命令行修改为自启动
(记得把"/nvmel$/手机号码+字母"替换掉)
```shell
docker run -d -p 9000:9000 --name=portainer-zh --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v /tmp/zfsv3/nvmel*/手机号码+字母/data/docker/portainer-zh:/data 6053537/portainer-ce

```

# 【portainer 食用方法】
:white_check_mark: 视频教程：
https://www.bilibili.com/video/BV1TCgNeLEtu

:white_check_mark: 图文教程：

- 1、使用文本编辑器先把自己的docker文件路径，存储池路径记录下来（见上说明）

- 2、下载模板，按照模板内的注释修改内容，请务必确认好配置单无误

- 3、这里拿汉化版作为演示便于理解，官方版一样的位置的，部署好portainer（见上）并打开，点击docker，左侧的堆栈（stack），右侧的新增（Add）

![image](https://github.com/FrozenGEE/compose/blob/main/.images/00.Portainer/Portainer-01-03.png)
![image](https://github.com/FrozenGEE/compose/blob/main/.images/00.Portainer/Portainer-04.png)
![image](https://github.com/FrozenGEE/compose/blob/main/.images/00.Portainer/Portainer-05.png)
![image](https://github.com/FrozenGEE/compose/blob/main/.images/00.Portainer/Portainer-06.png)
![image](https://github.com/FrozenGEE/compose/blob/main/.images/00.Portainer/Portainer-07.png)

- 4、命名stack的名字，把预先写好的compose内容复制到文本框中，点击部署，等待完成，如果有报错，看右上交报文提示，寻找网友帮助

![image](https://github.com/FrozenGEE/compose/blob/main/.images/00.Portainer/Portainer-08-10.png)

- 5、后续修改更新 & 更新镜像

![image](https://github.com/FrozenGEE/compose/blob/main/.images/00.Portainer/Portainer-11.png)
![image](https://github.com/FrozenGEE/compose/blob/main/.images/00.Portainer/Portainer-12.png)

- 8、未映射卷 & 残留卷

![image](https://github.com/FrozenGEE/compose/blob/main/.images/00.Portainer/Portainer-13.png)
![image](https://github.com/FrozenGEE/compose/blob/main/.images/00.Portainer/Portainer-14.png)
![image](https://github.com/FrozenGEE/compose/blob/main/.images/00.Portainer/Portainer-15.png)

- 9、镜像导出 & 镜像导入 & 镜像删除

![image](https://github.com/FrozenGEE/compose/blob/main/.images/00.Portainer/Portainer-16.png)
![image](https://github.com/FrozenGEE/compose/blob/main/.images/00.Portainer/Portainer-17.png)
![image](https://github.com/FrozenGEE/compose/blob/main/.images/00.Portainer/Portainer-18.png)

# 【logo 食用方法】
- unRAID：先创建一个名为"LOGO"的共享文件夹，然后把图标的压缩包解压到目录内，路径为 /mnt/user/LOGO/LOGO.png 这样的格式即可，提供的compose均预设好，也可以选择使用网址访问
 
![image](https://github.com/FrozenGEE/compose/blob/main/.images/01.unRAID/unRAID-01.png)
![image](https://github.com/FrozenGEE/compose/blob/main/.images/01.unRAID/unRAID-02.png)
![image](https://github.com/FrozenGEE/compose/blob/main/.images/01.unRAID/unRAID-03.png)
![image](https://github.com/FrozenGEE/compose/blob/main/.images/01.unRAID/unRAID-04.png)

- CasaOS：使用图床，然后将图片上传后，在logo的地址栏上输入logo的访问地址即可
  
![image](https://github.com/FrozenGEE/compose/blob/main/.images/02.CasaOS/CasaOS-01.png)
![image](https://github.com/FrozenGEE/compose/blob/main/.images/02.CasaOS/CasaOS-02.png)
![image](https://github.com/FrozenGEE/compose/blob/main/.images/02.CasaOS/CasaOS-03.png)

- Sun-Panel：先在nas中找个地方存放好logo图标，将"/app/conf/uploads"该路径映射，映射到logo文件夹上，具体请看compose的注释，然后在图标编辑的时候，输入"/uploads/LOGO.PNG"即可
  
![image](https://github.com/FrozenGEE/compose/blob/main/.images/04.Sun-Panel/Sun-Panel-01.png)
![image](https://github.com/FrozenGEE/compose/blob/main/.images/04.Sun-Panel/Sun-Panel-02.png)
