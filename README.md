<div class="se-preview-section-delimiter"></div>






<div class="se-preview-section-delimiter"></div>

## 接收用户数据服务端接口需求
@[需求提出者：尹洪冰  文档作者： 赵垠兰]





<div class="se-preview-section-delimiter"></div>

### 目录

[toc]





<div class="se-preview-section-delimiter"></div>

### 修订记录

|版本|时间|作者|文档信息|
|:--|:--|:--|:--|
|1.0|2015-03-02|赵垠兰|根据SDC的同事的需求描述输出接口需求文档|



----------






<div class="se-preview-section-delimiter"></div>

### 项目背景

广告业务点击率（广告业务主要是针对网盟类似于google的广告）与用户行为分析（如用户流失分析）均需要从客户端接收用户数据。目前SA并没有提供很方便的接收工具，虽然有cache，但是其流程复杂，可用性低，兄弟部门的同事有一定抵触倾向，当前各工作室的解决方案是各自写客户端和搭建服务器以接收来自客户端的信息，最终rsync给SDC。 此现象导致人力资源的浪费（重复劳动）、服务器资源利用率低、维护成本高等问题。故希望提供统一的客户端与服务端以提高资源的利用率。本文重点描述服务端的需求。





<div class="se-preview-section-delimiter"></div>

### 可行性分析

10分钟200万的数据，约为每秒3000的访问量，在可接受的范围内。





<div class="se-preview-section-delimiter"></div>

### 功能需求

提供数据接口接收来自客户端的数据并存储





<div class="se-preview-section-delimiter"></div>

#### 接口字段

1. **时间**，格式如 [2014-12-01 10:00:00 +0800] ， 包括时区，”+0800”就表示时区信息，必填
     生成该格式时间的方式： ```date '+%Y-%m-%d %H:%M:%S %z'```
3. **项目代号**，必填
4. **日志来源**，如网盟、产品， 必填
5. **日志类型**，如网盟的xxx操作、产品的xxx的操作
6.  其他字段，允许用户上传多个字段，不作限制，如下所示
	```
	http请求的样式为http://bias.netease.com/mmad/click?s=L0dF2POv%2BZIAnaihFlMdg%2BoNRvE%3D&project_id=10003624&
	monitor_type=1&time= 1423640943&ip=123.58.111.110&device=ipad3&os_ver=8.0.1&
mac=null%idfa=B081DD6A-DADFDAF-FDAFDAF-1DFAFDAF2343  
	```





<div class="se-preview-section-delimiter"></div>

#### 存储

[时间][项目][来源][类型]{其他字段的json} 
如果接口的必填字段客户端没有提供，则将该请求以json的格式存储于其他表，7天定时删除
