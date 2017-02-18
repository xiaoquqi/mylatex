# DR Cloud测试方案建议书

## 硬件配置要求

|硬件名称|配置|数量|注释|
|-------|--------|--------|-----|
|生产端容灾恢复一体机|双路英特尔至强E5 2630 v3系列或以上CPU<br/>32GB DDR4或以上内存<br/>至少4块2TB SATA HDD<br/>千兆网口4个<br/>万兆网口2个<br/>远程管理控制口|1台|用于模拟本地生产环境的保护|
|异地容灾恢复一体机|双路英特尔至强E5 2630 v3系列或以上CPU<br/>32GB DDR4或以上内存<br/>至少4块2TB SATA HDD<br/>千兆网口4个<br/>万兆网口2个<br/>远程管理控制口|1台|用于模拟异地生产环境的保护|

## 网络配置要求

* 两台物理机必须分别放置于两个VLAN中
* 两个VLAN通过三层地址能够互相访问

## 测试编号说明
|编号|说明|
|----------|-----------|
|DRCT0xx|安装部署相关的测试用例|
|DRCT1xx|保护相关的测试用例|
|DRCT2xx|恢复相关的测试用例|
|DRCT3xx|恢复相关的测试用例|
|DRCT5xx|平台本身测试用例|

## 测试用例
|编号|测试用例名称|说明|结果|
|------|-----------|----------|---------|
|DRCT001|DR Cloud的安装和配置|用于测试DR Cloud是否能正常安装，安装过程中不需要过多人为的介入||
|DRCT101|Linux物理机客户端的安装和注册|验证Linux Agent能够正确注册被保护的Linux物理机||
|DRCT102|Windows物理机客户端的安装和注册|验证Windows Agent能够正确注册被保护的Windows物理机||
|DRCT103|VMware虚拟机注册|能够正常添加ESXi或vCenter，发现所有虚拟机列表并且能正常注册||
|DRCT104|为物理机设定本地保护|正确将物理机加入本地保护||
|DRCT105|为虚拟机设定本地保护|能够为虚拟机正确加入保护，系统能够对虚拟机实现保护||
|DRCT106|设置远程保护|能够将本地的虚拟机正确复制到远程的DR Cloud中，在这个过程中，用户可以对数据采用压缩和加密的策略||
|DRCT107|修改快照生成间隔|能够正确修改快照生成的间隔，修改后，系统按照指定的时间间隔进行数据的保护||
|DRCT108|手动触发快照|能够手动的对被保护主机执行快照，执行后，系统立即进行快照||
|DRCT201|物理机快速恢复至KVM平台|正确将物理机恢复至KVM平台，恢复时间不超过30秒||
|DRCT202|物理机快速恢复至VMware平台|正确将物理机快速恢复至VMware平台，恢复时间不超过30秒||
|DRCT203|VMware虚拟机快速恢复至VMware平台|对VMware虚拟机可以采用快速启动的方式快速进行恢复，恢复时间不超过30秒||
|DRCT204|将远程复制的物理机快速恢复至KVM|将远程复制的物理机快速恢复至KVM平台，恢复时间不超过30秒||
|DRCT205|将远程复制的VMware虚拟机快速恢复至VMware|将远程复制的虚拟机正确恢复至VMware中，恢复时间不超过30秒||
|DRCT301|SQL Server数据库保护与恢复测试|保护SQL Server数据库，并且数据库可以正常恢复||
|DRCT302|Windows Oracle数据库保护与恢复测试|保护Oracle数据库，并且数据库可以正常恢复||
|DRCT303|Linux MySQL数据库保护与恢复测试|保护MySQL数据库，并且数据库可以正常恢复||
|DRCT304|Linux Oracle数据库保护与恢复测试|保护SQL Server数据库，并且数据库可以正常恢复||
|DRCT501|设置每页表格显示的数量|正确设置每页表格显示的数量||
|DRCT502|语言设置|正确设置系统界面的语言||

## DRCT001: DR Cloud的安装和配置

|目标|DR Cloud的安装和配置|
|-------|----------------|
|前提条件|DR Cloud ISO镜像|
|测试步骤|1、部署和软件并记录时间<br/>2、通过浏览器访问管理控制台|
|测试预期|1、在标准x86硬件上部署DR Cloud，可以部署在物理机或是虚拟机中<br/>2、不需要安装操作系统或数据库<br/>3、可以通过WEB浏览器远程访问管理控制台|
|测试结果||
|注释||

### 步骤1：部署和软件配置

1) 将ISO挂载或制作光盘，然后开机，当出现开始界面后，选择DRProphet + DRCloud

![](images/DRCT001-1.png)

2) 选择目标盘，使用tab键切换到Apply按钮并回车，开始安装过程

![](images/DRCT001-2.png)

3) 检查安装进度

![](images/DRCT001-3.png)

4) 安装完成后，按照提示将光盘取出并点击OK重启服务器

![](images/DRCT001-4.png)

5) 配置网络

当出现屏幕提示时，配置网络。注意：如果有多块网卡被检测到，将自动使用eth1网卡。

![](images/DRCT001-5.png)

6) 完成配置

选择skip，跳过federator的安装。

![](images/DRCT001-6.png)

步骤2：通过WEB访问管理控制台

### 步骤2: 打开浏览器(推荐使用Chrome或者Firefox)

* 访问DR Cloud管理面板地址：http://<server-ip>:8088
* 默认账户名：admin
* 默认密码：admin_pass

## DRCT101: Linux物理机客户端的安装和注册

|目标|安装DR Cloud Linux客户端并且在DR Cloud服务端注册|
|-------|----------------|
|前提条件|1、DR Cloud Linux客户端<br/>2、CentOS 7|
|测试步骤|1、在应用服务器安装DR Cloud Linux客户端<br/>2、在DR Cloud服务端确认是否已经注册，并且显示在列表中|
|测试预期|1、安装Linux DR Cloud不影响客户端<br/>2、成功向DR Cloud注册|
|测试结果||
|注释||

### 步骤1: 在应用服务器安装DR Cloud Linux客户端

* 下载安装包

```
wget http://<管理节点IP>/softwares/centos7/egis-agent-1.1-1.el7.x86_64.rpm
wget http://<管理节点IP>/softwares/centos7/iscsi-initiator-utils-6.2.0.873-33.el7_2.2.x86_64.rpm
wget http://<管理节点IP>/softwares/centos7/iscsi-initiator-utils-iscsiuio-6.2.0.873-33.el7_2.2.x86_64.rpm
wget http://<管理节点IP>/softwares/centos7/dattobd-$(uname -r).rpm
wget http://<管理节点IP>/softwares/centos7/partclone-0.2.89-10.x86_64.rpm
```

* 安装

```
rpm -ivh egis-agent-1.1-1.el7.x86_64.rpm \
  dattobd-$(uname -r).rpm \
  iscsi-initiator-utils-6.2.0.873-33.el7_2.2.x86_64.rpm  \
  partclone-0.2.89-10.x86_64.rpm
```

* 设置保护

修改配置文件: /etc/sysconfig/egis-agent

```
log_dir=/var/log/egis-agent
drcloud_url=管理网IP:8760
```

重新启动 egis-agent 服务，并检查服务状态，保证服务为 running 状态

```
systemctl restart egis-agent.service
systemctl status egis-agent.service
```

### 步骤2: 在DR Cloud服务端确认是否已经注册

![](images/DRCT101-1.png)

## DRCT102: Windows物理机客户端的安装和注册

|目标|安装DR Cloud Windows客户端并且在DR Cloud服务端注册|
|-------|----------------|
|前提条件|1、DR Cloud Windows客户端<br/>2、dotnet framework或更高<br/>3、开启iscsi initiator<br/>4、防火墙打开3260和5988端口|
|测试步骤|1、在应用服务器安装DR Cloud Windows客户端<br/>2、填写注册信息(包括IO限速)<br/>3、在DR Cloud服务端确认是否已经注册，并且显示在列表中<br/>4、确认Agent能够正确上报被保护的主要磁盘<br/>5、设置限速|
|测试预期|1、安装Windows DR Cloud不影响客户端<br/>2、成功向DR Cloud注册<br/>3、向DR Cloud正确上报客户端磁盘信息|
|测试结果||
|注释||

### 步骤1: 安装DR Cloud Windows代理端

1) 防火墙的设定

【开始】->【命令提示符】执行：netsh advfirewall firewall add rule name="DRP Backup Port" dir=in action=allow protocol=TCP localport=5988在防火墙中进行验证，验证方法：【开始】->【管理工具】->【高级安全Windows防火墙】

![](images/DRCT102-1.png)

在打开的界面中左侧列表中点击【入站规则】，在右侧窗口中应当显示【DRP Backup Port】，证明防火墙规则添加成功。

![](images/DRCT102-2.png)

2) Microsoft .NET Framework 4安装
打开浏览器，从容灾还原一体机上下载Microsoft .NET Framework 4安装包并且安装，下载地址：http://<管理节点IP>/softwares/windows/dotnet4.5.exe。

3) 安装保护代理

* 打开浏览器，从容灾还原一体机下载Windows保护Agent，下载地址：http://<管理节点IP>/softwares/windows/drprophet-master-agent-3.x-xxxx.exe* 运行安装，此时安装程序将安装三个相关的程序：DR Prophet Master Agent, DR Prophet Job Agent, DR Prophet Agent，采用默认安装方式，全部选择【Next】，如果出现文件目录已经存在的提示，则选择【OK】。
![](images/DRCT102-3.png)

### 步骤2: 填写注册信息(包括IO限速)

安装成功后，将显示设置窗口(如下图所示)

* 链接方式：iscsi
* 选择要保护的磁盘，把他们拖到"包含磁盘"中

![](images/DRCT102-4.png)

* 点击【Network】标签，在【Use the specified management server】中填入容灾还原一体机管理地址。* 如果需要对传输进行限速，则勾选【Throttling】，填入限速速度，默认情况下，无需限制。* 最后在【How can DRProphet server connect to this machine】中选择容灾还原一体机连接被保护服务器的IP地址。* 如果服务器原有IP地址可以直接连接，则选择【Connect to my local IP】。* 如果服务器需要其他地址转发后才能连接，则选择【Connect via a NAT address】。

![](images/DRCT102-5.png)

* 填写完成后，选择【OK】，完成客户端的配置。

### 步骤3: 确认注册成功，以及上报信息无误。

![](images/DRCT102-6.png)

## DRCT103: VMware虚拟机注册

|目标|发现VMware虚拟机并且在DR Cloud注册|
|-------|----------------|
|前提条件|1、ESXi 5.1或更高版本<br/>2、存储API功能确认开启|
|测试步骤|1、添加ESXi或者vCenter地址<br/>2、检查所有保护的虚拟机<br/>3、确认注册成功|
|测试预期|不需要代理程序就可以注册ESXi上的虚拟机|
|测试结果||
|注释||

### 步骤1: 添加ESXi或者vCenter地址

* 点击【虚拟化服务器】->【VMware】->【连接】->【创建】-> 输入要连接的Vcenter IP地址/端口/账户/密码等信息->【创建】

![](images/DRCT103-1.png)

* 创建完成后，点击【ESXI服务器】来查看此Vcenter下的所有ESXI主机，勾选ESXI主机  点击【网络设定】->【添加】， 输入要创建的网络名称/VLAN ID->【保存】

![](images/DRCT103-2.png)
### 步骤2: 检查所有保护的虚拟机

* 点击【虚拟化服务器】->【VMware】->【连接】点击添加的Vcenter主机名称，在跳转页面中会此Vcenter主机的连接详情以及其下的所有VM虚拟机

![](images/DRCT103-3.png)

* 勾选要保护的VM点击【注册】->【确定】，勾选刚注册的主机【加入保护】->【确定】

![](images/DRCT103-4.png)

![](images/DRCT103-5.png)

### 步骤3: 确认注册成功

在【未保护主机】中能够成功看到刚才加入保护的虚拟机

![](images/DRCT103-6.png)

## DRCT104: 为物理机设定本地保护

|目标|物理机本地保护的设定|
|-------|----------------|
|前提条件|DRCT102必须完成|
|测试步骤|1、通过DR Cloud管理平台设置保护<br/>2、确认保护设置成功|
|测试预期|1、成功添加保护<br/>2、所有镜像盘以thin provision方式分配|
|测试结果||
|注释||

### 步骤1: 通过DR Cloud管理平台设置保护

* 登录DRP管理页面, http://<DRP_SERVER_IP>:8088* 点击【主机保护】->【未保护主机】，勾选未保护主机点击【加入保护】选项

![](images/DRCT104-1.png)

* 选择后端存储服务器，可自动分配也可手动指定，点击【确定】选项

![](images/DRCT104-2.png)

### 步骤2: 确认保护设置成功

确认添加的主机已经成功的出现在【已保护主机中】

![](images/DRCT104-3.png)

## DRCT105：为虚拟机设定本地保护

* 登录DRP管理页面, http://<DRP_SERVER_IP>:8088* 点击【主机保护】->【未保护主机】，勾选未保护主机点击【加入保护】选项

![](images/DRCT104-1.png)

* 选择后端存储服务器，可自动分配也可手动指定，点击【确定】选项

![](images/DRCT104-2.png)

### 步骤2: 确认保护设置成功

确认添加的主机已经成功的出现在【已保护主机中】

![](images/DRCT104-3.png)

## DRCT106：设置远程复制

|目标|为本地已经保护的节点设置远程复制|
|-------|----------------|
|前提条件|DRCT104和DRCT105必须完成|
|测试步骤|1、通过DR Cloud管理平台设置复制<br/>2、确认复制设置成功<br/>3、配置复制计划|
|测试预期|1、成功设定复制<br/>2、成功配置复制计划|
|测试结果||
|注释||

### 步骤1: 通过DR Cloud管理平台设置复制

* 进入DR Cloud的【主机保护】->【远程复制】，点击创建，【主机】选择已经保护的主机，【远程服务器】直接输入远端主机的IP信息，【存储用户】输入drprophet，【压缩】选择是，【加密】选择否，点击确认

![](images/DRCT106-1.png)

### 步骤2: 确认复制设置成功

* 在列表页面中应出现一条新的记录

![](images/DRCT106-2.png)

### 步骤3: 配置复制计划

* 选择刚刚保护的主机，点击更新，设置复制的时间间隔

![](images/DRCT106-3.png)

## DRCT107: 修改快照生成间隔

|目标|修改快照生成间隔|
|-------|----------------|
|前提条件|DRCT104和DRCT105必须完成|
|测试步骤|1、修改快照计划<br/>2、确认数据复制开始时间正确|
|测试预期|1、成功修改快照计划<br/>2、正确配置快照开始时间|
|测试结果||
|注释||

### 步骤1: 修改快照计划

* 选择【已保护主机】，勾选已经保护的主机，点击【保护设置】，设置【快照间隔】、【快照配额】和【快照开始时间】

![](images/DRCT107-1.png)

### 步骤2: 确认数据复制开始时间正确

* 在列表页面中，确认快照间隔设置正确

![](images/DRCT107-1.png)

## DRCT108: 手动触发快照

|目标|手动触发快照|
|-------|----------------|
|前提条件|DRCT104和DRCT105必须完成|
|测试步骤|1、手动触发快照<br/>2、查看作业列表，确认任务是否已经执行|
|测试预期|成功触发快照|
|测试结果||
|注释||

### 步骤1: 手动触发快照

* 查看已保护主机,并勾选需要执行快照的主机，并点击创建快照

![](images/DRCT108-1.png)

### 步骤2: 查看作业列表，确认任务是否已经执行

* 查看【运维管理】->【任务】，查看任务是否已经开始执行

![](images/DRCT108-2.png)

## DRCT201: 物理机快速恢复至KVM平台

|目标|物理机快速恢复至KVM平台|
|-------|----------------|
|前提条件|1、DRCT104必须完成<br/>2、已经有快照产生|
|测试步骤|1、设定主机组<br/>2、设定恢复计划<br/>3、恢复<br/>4、查看已经恢复的资源|
|测试预期|1、成功设定主机组<br/>2、资源成功恢复<br/>3、恢复时间小于30秒|
|测试结果||
|注释||

### 步骤1: 设定主机组

* 进入DR Cloud，选择【灾难恢复】->【主机组】，点击创建，选择主机组内包含的主机，选择已经保护的物理机，点击创建

![](images/DRCT201-1.png)

* 创建成功后，返回列表页面中应显示主机组的内容

![](images/DRCT201-2.png)

### 步骤2: 设定恢复计划

* 选择【灾难恢复】->【恢复计划】，点击创建，选择刚刚创建的主机组，系统将自动检测该主机组内包含的主机是否包含快照

![](images/DRCT201-3.png)

* 点击下一步，选择基本的配置信息，选择恢复的目标主机为KVM平台主机，并选择已经创建的网络和恢复的规格

![](images/DRCT201-4.png)

* 点击下一步，选择恢复的时间点信息，最后点击创建

![](images/DRCT201-5.png)

### 步骤3: 恢复

* 在列表页面中，选择刚刚创建的恢复计划，并点击恢复，资源开始进行恢复

![](images/DRCT201-6.png)

### 步骤4: 查看已经恢复的资源

* 恢复完成后，点击【已恢复主机】

![](images/DRCT201-7.png)

## DRCT202：物理机快速恢复至VMware平台

|目标|物理机快速恢复至VMware平台|
|-------|----------------|
|前提条件|1、DRCT105必须完成<br/>2、已经有快照产生|
|测试步骤|1、设定主机组<br/>2、设定恢复计划<br/>3、恢复<br/>4、查看已经恢复的资源|
|测试预期|1、成功设定主机组<br/>2、资源成功恢复<br/>3、恢复时间小于30秒|
|测试结果||
|注释||

### 步骤1: 设定主机组

* 进入DR Cloud，选择【灾难恢复】->【主机组】，点击创建，选择主机组内包含的主机，选择已经保护的Windows物理机，点击创建

> 注意：Linux物理机暂时无法恢复至VMware平台

![](images/DRCT201-1.png)

* 创建成功后，返回列表页面中应显示主机组的内容

![](images/DRCT201-2.png)

### 步骤2: 设定恢复计划

* 选择【灾难恢复】->【恢复计划】，点击创建，选择刚刚创建的主机组，系统将自动检测该主机组内包含的主机是否包含快照

![](images/DRCT201-3.png)

* 点击下一步，选择基本的配置信息，选择恢复的目标主机为VMware的ESXi或者vCenter，并选择已经创建的网络和恢复的规格

![](images/DRCT201-4.png)

* 点击下一步，选择恢复的时间点信息，最后点击创建

![](images/DRCT201-5.png)

### 步骤3: 恢复

* 在列表页面中，选择刚刚创建的恢复计划，并点击恢复，资源开始进行恢复

![](images/DRCT201-6.png)

### 步骤4: 查看已经恢复的资源

* 恢复完成后，点击【已恢复主机】

![](images/DRCT201-7.png)

## DRCT203: VMware虚拟机快速恢复至VMware平台

|目标|VMware虚拟机快速恢复至VMware平台|
|-------|----------------|
|前提条件|1、DRCT105必须完成<br/>2、已经有快照产生|
|测试步骤|1、设定主机组<br/>2、设定恢复计划<br/>3、恢复<br/>4、查看已经恢复的资源|
|测试预期|1、成功设定主机组<br/>2、资源成功恢复<br/>3、恢复时间小于30秒|
|测试结果||
|注释||

### 步骤1: 设定主机组

* 进入DR Cloud，选择【灾难恢复】->【主机组】，点击创建，选择主机组内包含的主机，选择已经保护的VMware虚拟机，点击创建

![](images/DRCT203-1.png)

* 创建成功后，返回列表页面中应显示主机组的内容

![](images/DRCT201-2.png)

### 步骤2: 设定恢复计划

* 选择【灾难恢复】->【恢复计划】，点击创建，选择刚刚创建的主机组，系统将自动检测该主机组内包含的主机是否包含快照

![](images/DRCT201-3.png)

* 点击下一步，选择基本的配置信息，选择恢复的目标主机为VMware的ESXi或者vCenter，并选择已经创建的网络和恢复的规格

![](images/DRCT201-4.png)

* 点击下一步，选择恢复的时间点信息，最后点击创建

![](images/DRCT201-5.png)

### 步骤3: 恢复

* 在列表页面中，选择刚刚创建的恢复计划，并点击恢复，资源开始进行恢复

![](images/DRCT201-6.png)

### 步骤4: 查看已经恢复的资源

* 恢复完成后，点击【已恢复主机】

![](images/DRCT201-7.png)

## DRCT204: 将远程复制的物理机快速恢复至KVM

|目标|将远程复制的物理机快速恢复至KVM|
|-------|----------------|
|前提条件|1、DRCT104、DRCT105和DRCT106必须完成<br/>2、已经复制到远端DR Cloud|
|测试步骤|1、设定主机组<br/>2、设定恢复计划<br/>3、恢复<br/>4、查看已经恢复的资源|
|测试预期|1、成功设定主机组<br/>2、资源成功恢复<br/>3、恢复时间小于30秒|
|测试结果||
|注释||

### 步骤1: 设定主机组

* 进入DR Cloud，选择【灾难恢复】->【主机组】，点击创建，选择主机组内包含的主机，选择已经保护的VMware虚拟机，点击创建

![](images/DRCT204-1.png)

* 创建成功后，返回列表页面中应显示主机组的内容

![](images/DRCT201-2.png)

### 步骤2: 设定恢复计划

* 选择【灾难恢复】->【恢复计划】，点击创建，选择刚刚创建的主机组，系统将自动检测该主机组内包含的主机是否包含快照

![](images/DRCT201-3.png)

* 点击下一步，选择基本的配置信息，选择恢复的目标主机为VMware的ESXi或者vCenter，并选择已经创建的网络和恢复的规格

![](images/DRCT201-4.png)

* 点击下一步，选择恢复的时间点信息，最后点击创建

![](images/DRCT201-5.png)

### 步骤3: 恢复

* 在列表页面中，选择刚刚创建的恢复计划，并点击恢复，资源开始进行恢复

![](images/DRCT201-6.png)

### 步骤4: 查看已经恢复的资源

* 恢复完成后，点击【已恢复主机】

![](images/DRCT201-7.png)

## DRCT205: 将远程复制的VMware虚拟机快速恢复至VMware

|目标|将远程复制的VMware虚拟机快速恢复至VMware|
|-------|----------------|
|前提条件|1、DRCT104、DRCT105和DRCT106必须完成<br/>2、已经复制到远端DR Cloud|
|测试步骤|1、设定主机组<br/>2、设定恢复计划<br/>3、恢复<br/>4、查看已经恢复的资源|
|测试预期|1、成功设定主机组<br/>2、资源成功恢复<br/>3、恢复时间小于30秒|
|测试结果||
|注释||

### 步骤1: 设定主机组

* 进入DR Cloud，选择【灾难恢复】->【主机组】，点击创建，选择主机组内包含的主机，选择已经保护的VMware虚拟机，点击创建

![](images/DRCT205-1.png)

* 创建成功后，返回列表页面中应显示主机组的内容

![](images/DRCT201-2.png)

### 步骤2: 设定恢复计划

* 选择【灾难恢复】->【恢复计划】，点击创建，选择刚刚创建的主机组，系统将自动检测该主机组内包含的主机是否包含快照

![](images/DRCT201-3.png)

* 点击下一步，选择基本的配置信息，选择恢复的目标主机为VMware的ESXi或者vCenter，并选择已经创建的网络和恢复的规格

![](images/DRCT201-4.png)

* 点击下一步，选择恢复的时间点信息，最后点击创建

![](images/DRCT201-5.png)

### 步骤3: 恢复

* 在列表页面中，选择刚刚创建的恢复计划，并点击恢复，资源开始进行恢复

![](images/DRCT201-6.png)

### 步骤4: 查看已经恢复的资源

* 恢复完成后，点击【已恢复主机】

![](images/DRCT201-7.png)

## DRCT301: SQL Server数据库保护与恢复测试

|目标|SQL Server数据库保护与恢复测试|
|-------|----------------|
|前提条件|1、在Windows 2008 R2 64bit服务器上，正确配置和安装SQL Server 2008版本<br/>2、可以通过客户端正确访问SQL Server<br/>3、在另外一台客户端上，已经安装了Benchmark Factory压力测试工具<br/>4、参考DRCT102完成Windows的注册和保护<br/>5、参考DRCT104将Windows加入保护|
|测试步骤|1、手动执行快照<br/>2、开始压力测试<br/>3、再次执行快照<br/>4、使用快照点进行恢复<br/>5、验证恢复的系统能否正常访问SQL Server|
|测试预期|1、SQL Server能够被正确保护<br/>2、SQL Server能被正常恢复|
|测试结果||
|注释||

### 步骤1: 手动执行快照

* 查看已保护主机,并勾选需要执行快照的主机，并点击创建快照

![](images/DRCT108-1.png)

### 步骤2: 开始压力测试

* 在Benchmark Factory官方网站下载试用版本并正确安装：
> https://www.quest.com/register/54678/

* 配置连接方式

* 配置压力测试的类型

> 注意：受被测试机器性能的限制，不建议选择过高的压力测试内容，避免服务器超载

![](images/DRCT301-1.png)

* 确认压力测试开始

![](images/DRCT301-2.png)

### 步骤3: 再次执行快照

* 查看已保护主机,并勾选需要执行快照的主机，并点击创建快照

![](images/DRCT108-1.png)

### 步骤4: 使用快照点进行恢复

* 进入DR Cloud，选择【灾难恢复】->【主机组】，点击创建，选择主机组内包含的主机，选择已经保护的物理机，点击创建

![](images/DRCT201-1.png)

* 创建成功后，返回列表页面中应显示主机组的内容

![](images/DRCT201-2.png)

* 选择【灾难恢复】->【恢复计划】，点击创建，选择刚刚创建的主机组，系统将自动检测该主机组内包含的主机是否包含快照

![](images/DRCT201-3.png)

* 点击下一步，选择基本的配置信息，选择恢复的目标主机为KVM平台主机，并选择已经创建的网络和恢复的规格

![](images/DRCT201-4.png)

* 点击下一步，选择恢复的时间点信息，最后点击创建

![](images/DRCT201-5.png)

* 在列表页面中，选择刚刚创建的恢复计划，并点击恢复，资源开始进行恢复

![](images/DRCT201-6.png)

* 恢复完成后，点击【已恢复主机】

![](images/DRCT201-7.png)

### 步骤5: 验证恢复的系统能够正常访问SQL Server

## DRCT302: Windows Oracle数据库保护与恢复测试

|目标|Windows Oracle数据库保护与恢复测试|
|-------|----------------|
|前提条件|1、在Windows 2008 R2 64bit服务器上，正确配置和安装Oracle 12c版本<br/>2、可以通过客户端正确访问Oracle<br/>3、在另外一台客户端上，已经安装了Benchmark Factory压力测试工具<br/>4、参考DRCT102完成Windows的注册和保护<br/>5、参考DRCT104将Windows加入保护|
|测试步骤|1、手动执行快照<br/>2、开始压力测试<br/>3、再次执行快照<br/>4、使用快照点进行恢复<br/>5、验证恢复的系统能否正常访问Oracle|
|测试预期|1、Oracle能够被正确保护<br/>2、Oracle能被正常恢复|
|测试结果||
|注释||

### 步骤1: 手动执行快照

* 查看已保护主机,并勾选需要执行快照的主机，并点击创建快照

![](images/DRCT108-1.png)

### 步骤2: 开始压力测试

* 在Benchmark Factory官方网站下载试用版本并正确安装：
> https://www.quest.com/register/54678/

* 下载Oracle的客户端

在客户端安装Oracle客户端，访问Oracle Instance Client Downloads for Microsoft Windows(x64)页面：> http://www.oracle.com/technetwork/topics/winx64soft-089540.html

![](images/DRCT302-1.png)

Accept协议后，下载instanceclient-basic-windows.x64-12.1.0.2.0.zip。

* 配置Oracle客户端

下载完成后，将zip文件解压缩到C盘根目录中，例如将所有文件解压缩到C:\instantclient_12_1中后，配置系统环境变量，在PATH中添加这个目录。![](images/DRCT302-2.png)

* 使用Benchmark连接Oracle

![](images/DRCT302-3.png)
* 配置压力测试的类型

> 注意：受被测试机器性能的限制，不建议选择过高的压力测试内容，避免服务器超载

![](images/DRCT301-1.png)

* 确认压力测试开始

![](images/DRCT301-2.png)

### 步骤3: 再次执行快照

* 查看已保护主机,并勾选需要执行快照的主机，并点击创建快照

![](images/DRCT108-1.png)

### 步骤4: 使用快照点进行恢复

* 进入DR Cloud，选择【灾难恢复】->【主机组】，点击创建，选择主机组内包含的主机，选择已经保护的物理机，点击创建

![](images/DRCT201-1.png)

* 创建成功后，返回列表页面中应显示主机组的内容

![](images/DRCT201-2.png)

* 选择【灾难恢复】->【恢复计划】，点击创建，选择刚刚创建的主机组，系统将自动检测该主机组内包含的主机是否包含快照

![](images/DRCT201-3.png)

* 点击下一步，选择基本的配置信息，选择恢复的目标主机为KVM平台主机，并选择已经创建的网络和恢复的规格

![](images/DRCT201-4.png)

* 点击下一步，选择恢复的时间点信息，最后点击创建

![](images/DRCT201-5.png)

* 在列表页面中，选择刚刚创建的恢复计划，并点击恢复，资源开始进行恢复

![](images/DRCT201-6.png)

* 恢复完成后，点击【已恢复主机】

![](images/DRCT201-7.png)

### 步骤5: 验证恢复的系统能够正常访问Oracle

## DRCT303: Linux MySQL数据库保护与恢复测试

|目标|Linux MySQL数据库保护与恢复测试|
|-------|----------------|
|前提条件|1、CentOS7 64bit服务器上，正确配置和安装MySQL<br/>2、可以通过客户端正确访问MySQL<br/>3、在另外一台CentOS 7客户端上，已经安装了sysbench压力测试工具<br/>4、参考DRCT101完成Linux的注册和保护<br/>5、参考DRCT104将Linux加入保护|
|测试步骤|1、手动执行快照<br/>2、开始压力测试<br/>3、再次执行快照<br/>4、使用快照点进行恢复<br/>5、验证恢复的系统能否正常访问MySQL|
|测试预期|1、MySQL能够被正确保护<br/>2、MySQL能被正常恢复|
|测试结果||
|注释||

### 步骤1: 手动执行快照

* 查看已保护主机,并勾选需要执行快照的主机，并点击创建快照

![](images/DRCT108-1.png)

### 步骤2: 开始压力测试

* 在CentOS 7客户端安装sysbench

> 注意：使用Ubuntu 14.04的客户端可以直接使用sudo apt-get install -y sysbench

```
$ 　wget　http://nchc.dl.sourceforge.net/project/sysbench/sysbench/0.4.12/sysbench-0.4.12.tar.gz$　tar zxf sysbench-0.4.12.tar.gz$　cd sysbench-0.4.12$　./configure -with-mysql-includes=/usr/include/mysql \--with-mysql-libs=/var/lib/mysql$　make && make install
```

* MySQL数据库需要开放远程访问的权限

```
$ grant all privileges on *.* to ‘root’@’%’ identified by ‘Your_password’;$ flush privileges; 
```

* 在客户端使用sysbench产生压力

```
$ sysbench -test=oltp -max-time=60 -thread-stack-size=10000 -mysql-user=<mysql_user> -mysql-password=<mysql_password> -mysql-host=<mysql_ip>  -mysql-port=3306 prepare$ sysbench --test=oltp --max-time=60 --thread-stack-size=10000 --mysql-user=<mysql_user>  --mysql-password=<mysql_password> --mysql-host= <test_host_ip>  --mysql-port=3306 run
```

### 步骤3: 再次执行快照

* 查看已保护主机,并勾选需要执行快照的主机，并点击创建快照

![](images/DRCT108-1.png)

### 步骤4: 使用快照点进行恢复

* 进入DR Cloud，选择【灾难恢复】->【主机组】，点击创建，选择主机组内包含的主机，选择已经保护的物理机，点击创建

![](images/DRCT201-1.png)

* 创建成功后，返回列表页面中应显示主机组的内容

![](images/DRCT201-2.png)

* 选择【灾难恢复】->【恢复计划】，点击创建，选择刚刚创建的主机组，系统将自动检测该主机组内包含的主机是否包含快照

![](images/DRCT201-3.png)

* 点击下一步，选择基本的配置信息，选择恢复的目标主机为KVM平台主机，并选择已经创建的网络和恢复的规格

![](images/DRCT201-4.png)

* 点击下一步，选择恢复的时间点信息，最后点击创建

![](images/DRCT201-5.png)

* 在列表页面中，选择刚刚创建的恢复计划，并点击恢复，资源开始进行恢复

![](images/DRCT201-6.png)

### 步骤4: 查看已经恢复的资源

* 恢复完成后，点击【已恢复主机】

![](images/DRCT201-7.png)

### 步骤5: 验证恢复的系统能够正常访问MySQL

## DRCT304: Linux Oracle数据库保护与恢复测试

|目标|Windows Oracle数据库保护与恢复测试|
|-------|----------------|
|前提条件|1、在CentOS 7 64bit服务器上，正确配置和安装Oracle 12c版本<br/>2、可以通过客户端正确访问Oracle<br/>3、在另外一台客户端上，已经安装了Benchmark Factory压力测试工具<br/>4、参考DRCT101完成Linux的注册和保护<br/>5、参考DRCT104将Linux加入保护|
|测试步骤|1、手动执行快照<br/>2、开始压力测试<br/>3、再次执行快照<br/>4、使用快照点进行恢复<br/>5、验证恢复的系统能否正常访问Oracle|
|测试预期|1、Oracle能够被正确保护<br/>2、Oracle能被正常恢复|
|测试结果||
|注释||

### 步骤1: 手动执行快照

* 查看已保护主机,并勾选需要执行快照的主机，并点击创建快照

![](images/DRCT108-1.png)

### 步骤2: 开始压力测试

* 在Benchmark Factory官方网站下载试用版本并正确安装：
> https://www.quest.com/register/54678/

* 下载Oracle的客户端

在客户端安装Oracle客户端，访问Oracle Instance Client Downloads for Microsoft Windows(x64)页面：> http://www.oracle.com/technetwork/topics/winx64soft-089540.html

![](images/DRCT302-1.png)

Accept协议后，下载instanceclient-basic-windows.x64-12.1.0.2.0.zip。

* 配置Oracle客户端

下载完成后，将zip文件解压缩到C盘根目录中，例如将所有文件解压缩到C:\instantclient_12_1中后，配置系统环境变量，在PATH中添加这个目录。![](images/DRCT302-2.png)

* 使用Benchmark连接Oracle

![](images/DRCT302-3.png)
* 配置压力测试的类型

> 注意：受被测试机器性能的限制，不建议选择过高的压力测试内容，避免服务器超载

![](images/DRCT301-1.png)

* 确认压力测试开始

![](images/DRCT301-2.png)

### 步骤3: 再次执行快照

* 查看已保护主机,并勾选需要执行快照的主机，并点击创建快照

![](images/DRCT108-1.png)

### 步骤4: 使用快照点进行恢复

* 进入DR Cloud，选择【灾难恢复】->【主机组】，点击创建，选择主机组内包含的主机，选择已经保护的物理机，点击创建

![](images/DRCT201-1.png)

* 创建成功后，返回列表页面中应显示主机组的内容

![](images/DRCT201-2.png)

* 选择【灾难恢复】->【恢复计划】，点击创建，选择刚刚创建的主机组，系统将自动检测该主机组内包含的主机是否包含快照

![](images/DRCT201-3.png)

* 点击下一步，选择基本的配置信息，选择恢复的目标主机为KVM平台主机，并选择已经创建的网络和恢复的规格

![](images/DRCT201-4.png)

* 点击下一步，选择恢复的时间点信息，最后点击创建

![](images/DRCT201-5.png)

* 在列表页面中，选择刚刚创建的恢复计划，并点击恢复，资源开始进行恢复

![](images/DRCT201-6.png)

* 恢复完成后，点击【已恢复主机】

![](images/DRCT201-7.png)

### 步骤5: 验证恢复的系统能够正常访问Oracle

## DRCT501: 设置每页表格显示的数量

|目标|设置每页表格显示的数量|
|-------|----------------|
|前提条件||
|测试步骤|1、设置每页显示表格的数量<br/>2、查看列表页面中表格的显示数量是否改变|
|测试预期|成功设置表格每页显示的数量|
|测试结果||
|注释||

### 步骤1: 设置每页显示表格的数量

* 点击【设置】->【全局设置】，设置每页显示的数量，例如设置为5，然后点击保存

![](images/DRCT501-1.png)

### 步骤2: 查看列表页面中表格的显示数量是否改变

* 切换到【主机保护】->【已保护主机】列表页面中，查看列表页面中显示的行数是否已经改变

![](images/DRCT501-2.png)

## DRCT502: 语言设置

|目标|语言设置|
|-------|----------------|
|前提条件||
|测试步骤|1、设置DR Cloud显示语言<br/>2、查看界面显示的语言是否为英文|
|测试预期|成功设置指定语言|
|测试结果||
|注释||

### 步骤1: 设置每页显示表格的数量

* 点击【设置】->【全局设置】，设置语言为English

![](images/DRCT502-1.png)

### 步骤2: 查看界面显示的语言是否为英文

![](images/DRCT502-2.png)