## 【更新日志-2024-09-24】
- 绿联新系统大换血，用就得抛弃旧系统的思维
- 【重要】更新绿联新系统的个人文件夹的绝对路径说明，看下面，官方相册文件夹无法自定义存放在哪个存储池，只能把整个个人文件夹设置到其他存储池才可以修改
- 绿联新系统的compose和群晖一样，都需要提前建立好对应的共享文件夹，文件夹才可以执行成功，如果用portainer等第三方的部署，就只需要创建好共享文件夹即可
- 绿联新系统compose不像群晖那样子选择保存路径后，路径之下现有的compose文件不会提示调用，体验略差

#### 【更新日志-2024-08-27】
- 速更新增fnOS的支持，其实就是把模板稍微改改替换一下就完事了，新NAS系统简单体验了一下，还挺不错的
- [飞牛OS](https://www.fnnas.com)的第一个用户的权限（也是管理员账号）：uid=1000(用户的名字) gid=1001(Users) groups=1001(Users),1000(Administrators)

#### 【更新日志-2024-07-15】
- 上传适用于威联通的compose模板，味精测试，但应该冇问题，仅支持QuTScloud，QTS5.0版本及其以上，Container Station(docker)的路径可能和以前的qnap不太一样，冇求证过
- 威联通的第一个用户的权限：uid=1000(用户的名字) gid=100(everyone) groups=100(everyone),0(administrators)

#### 【更新日志-2024-07-10】
- 提供logo图，可用于unRAID，CasaOS的图标显示，也可以用于Sun-Panel当中使用，其余同理
- logo源于 [xushier/HD-Icons](https://github.com/xushier/HD-Icons)、[walkxcode/dashboard-icons](https://github.com/walkxcode/dashboard-icons)、casaos和unraid商店、有些是从网上找的
- [整合包-2024-07-10](https://www.123pan.com/s/YuAUVv-eW1nA.html)

##### 【更新日志-2024-07-05】
- 来自casaos的项目，YAML Generator 生成器：https://play.cuse.eu.org/generate
- 通过“在线docker面板”生成对应的compose模板，666666，流下没有技术的泪水
- 项目来源：[CasaOS-AppStore-Play](https://github.com/Cp0204/CasaOS-AppStore-Play)
- 另外推举一个cli转compose的网站：https://www.composerize.com
- 123盘的docker离线镜像好久没更新了，这里附上docker加速源项目：[cmliu/CF-Workers-docker.io](https://github.com/cmliu/CF-Workers-docker.io) ，推荐自建，只需要域名+cloudflare即可

##### 【更新日志-2024-06-25】
- 上传一些针对于群晖和新绿联的预设写好的模板，默认存储池均为/volume1，部分内容还需要自己稍微修改，或者增添，注意看注释
- 群晖用户请预先创建好对应的路径的文件夹，绿联用户请预先创建好共享文件夹
- unRAID用户不需要预先创建文件夹，对于unRAID的模板，linux原生系统都可以直接用

##### 【更新日志-2024-06-17】
- 上传一些模板，初步摸索github怎么用T_T
- docker国内墙了，对于一些小白无法下载镜像，这里提供个人拉取导出的镜像文件，拉取时间是6月8日，保证是当天最新，但不保证一定有你想要的镜像，部分镜像在后续有所更新，偶尔更新的频率
- https://www.123pan.com/s/YuAUVv-Qp1nA.html 提取码:fgee

## 【一堆肺话】
- 分享compose模板，方便新人，老手快速部署docker容器；因unraid模板而想写，但unraid模板只能用于unraid上，对于其他nas系统并不通用，而compose模板通用性很好

- 本人模板强烈建议使用portainer进行编写，其他工具自己执衫
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
💡OMV的docker配置文件存放路径
💡OMV的数据存放路径开头

(待补充...)
```

```shell
💡CasaOS的docker配置文件存放路径
/DATA/AppData/aaa
💡CasaOS的数据存放路径开头
/DATA/bbb
```

```shell
💡TrueNAS的docker配置文件存放路径
/mnt/共享文件夹/docker/aaa
💡TrueNAS的数据存放路径开头
/mnt/共享文件夹/bbb

共享文件夹需要自己设置好
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
```

```shell
💡绿联新系统(基于debian)的docker配置文件存放路径
/volume*/docker/aaa
💡绿联新系统(基于debian)的数据存放路径开头
/volume*/bbb

建议把创建好一个名为docker的共享文件夹

💡绿联新系统（基于debian）的个人文件夹存放路径（系统默认）
/volume*/@home/xxx/yyy
另一个写法：/home/xxx/yyy

xxx为用户名，包括管理员和普通用户，yyy为在此之下所创建的文件夹，其中包括了每一个用户的相册文件夹“Photos”
```

```shell
💡极空间系统的docker配置文件存放路径
/tmp/zfsv3/sata*/手机号码/data/docker/aaa
/tmp/zfsv3/nvme*/手机号码/data/docker/aaa
/tmp/zfsv3/sata*/手机号码+字母/data/docker/aaa
/tmp/zfsv3/nvme*/手机号码+字母/data/docker/aaa

💡极空间系统的数据存放路径开头
/tmp/zfsv3/sata*/手机号码/data/bbb
/tmp/zfsv3/nvme*/手机号码/data/bbb
/tmp/zfsv3/sata*/手机号码+字母/data/bbb
/tmp/zfsv3/nvme*/手机号码+字母/data/bbb

"nvme*"和"sata*"根据自己实际情况修改，*为数字；"手机号码"为个人手机号码
如果你有第二台极空间，并且用同一个手机号绑定注册，则为需要在手机号码后添加上a-z的字母
例如：1688888888，1688888888a，1688888888b，1688888888c，这样类推
```

```shell
💡fnOS的docker配置文件存放路径
/vol*/用户id/docker/aaa
💡fnOS的数据存放路径开头
/vol*/用户id/bbb

*代表第几个存储池
用户id，如其意思，第一个用户为即1000，创建第二个用户则是1001，第三个是1002，如此类推

第一个用户的第一个存储池的共享文件夹路径是/vol1/1000
第一个用户的第二个存储池的共享文件夹路径是/vol2/1000
第三个用户的第二个存储池的共享文件夹路径是/vol2/1002
...
类推
```

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
(记得把"/nvme*/手机号码+字母"替换掉)
```shell
docker run -d -p 9595:9000 --name=portainer-zh --restart=no -v /var/run/docker.sock:/var/run/docker.sock -v /tmp/zfsv3/nvme*/手机号码+字母/data/docker/portainer-zh:/data 6053537/portainer-ce

```

# 【portainer 食用方法】
:white_check_mark: 视频教程：
https://www.bilibili.com/video/BV1TCgNeLEtu

:white_check_mark: 图文教程：

- 1、使用文本编辑器先把自己的docker文件路径，存储池路径记录下来（见上说明）

- 2、下载模板，按照模板内的注释修改内容，请务必确认好配置单无误

- 3、这里拿汉化版作为演示便于理解，官方版一样的位置的，部署好portainer（见上）并打开，点击docker，左侧的堆栈（stack），右侧的新增（Add）

![image](https://github.com/FrozenGEE/compose/blob/main/.portainer/portainer-01-03.png)
![image](https://github.com/FrozenGEE/compose/blob/main/.portainer/portainer-04.png)
![image](https://github.com/FrozenGEE/compose/blob/main/.portainer/portainer-05.png)
![image](https://github.com/FrozenGEE/compose/blob/main/.portainer/portainer-06.png)
![image](https://github.com/FrozenGEE/compose/blob/main/.portainer/portainer-07.png)

- 4、命名stack的名字，把预先写好的compose内容复制到文本框中，点击部署，等待完成，如果有报错，看右上交报文提示，寻找网友帮助

![image](https://github.com/FrozenGEE/compose/blob/main/.portainer/portainer-08-10.png)

- 5、后续修改更新 & 更新镜像

![image](https://github.com/FrozenGEE/compose/blob/main/.portainer/portainer-11.png)
![image](https://github.com/FrozenGEE/compose/blob/main/.portainer/portainer-12.png)

- 8、未映射卷 & 残留卷

![image](https://github.com/FrozenGEE/compose/blob/main/.portainer/portainer-13.png)
![image](https://github.com/FrozenGEE/compose/blob/main/.portainer/portainer-14.png)
![image](https://github.com/FrozenGEE/compose/blob/main/.portainer/portainer-15.png)

- 9、镜像导出 & 镜像导入 & 镜像删除

![image](https://github.com/FrozenGEE/compose/blob/main/.portainer/portainer-16.png)
![image](https://github.com/FrozenGEE/compose/blob/main/.portainer/portainer-17.png)
![image](https://github.com/FrozenGEE/compose/blob/main/.portainer/portainer-18.png)

# 【logo 食用方法】
- unRAID：先创建一个名为"LOGO"的共享文件夹，然后把图标的压缩包解压到目录内，路径为 /mnt/user/LOGO/LOGO.png 这样的格式即可，提供的compose均预设好，也可以选择使用网址访问
 
![image](https://github.com/FrozenGEE/compose/blob/main/.logo/unraid-01.png)
![image](https://github.com/FrozenGEE/compose/blob/main/.logo/unraid-02.png)
![image](https://github.com/FrozenGEE/compose/blob/main/.logo/unraid-03.png)
![image](https://github.com/FrozenGEE/compose/blob/main/.logo/unraid-04.png)

- CasaOS：使用图床，然后将图片上传后，在logo的地址栏上输入logo的访问地址即可
  
![image](https://github.com/FrozenGEE/compose/blob/main/.logo/casaos-01.png)
![image](https://github.com/FrozenGEE/compose/blob/main/.logo/casaos-02.png)
![image](https://github.com/FrozenGEE/compose/blob/main/.logo/casaos-03.png)

- Sun-Panel：先在nas中找个地方存放好logo图标，将"/app/conf/uploads"该路径映射，映射到logo文件夹上，具体请看compose的注释，然后在图标编辑的时候，输入"/uploads/LOGO.PNG"即可
  
![image](https://github.com/FrozenGEE/compose/blob/main/.logo/sun-panel-01.png)
![image](https://github.com/FrozenGEE/compose/blob/main/.logo/sun-panel-02.png)
