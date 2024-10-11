# 硬盘的选择

在Nas搭建过程中，一直都有小伙伴咨询：NAS硬盘该选多大？选什么牌子？什么型号？要不要组Raid？等等，下面就把常见几个问题的个人见解整理如下：

### 一、需要什么类型的硬盘？

通常情况下我们说的NAS硬盘都是指3.5英寸NAS专用机械硬盘，最具有代表性的就是希捷酷狼、酷狼Pro，其他还有企业级硬盘（如希捷银河）、监控盘（如希捷酷鹰）以及普通硬盘（如希捷酷鱼）。从**可靠性**上来排名的话，**企业级>NAS专用>监控>普通**，不同系列的硬盘在工作负载、核心功能、保修政策上也会有所差异。

基本上机械硬盘的转速都是在5400RPM-7200RPM之间，更高的读写性能意味更高的转速、更大的噪音。企业盘虽拥有最高的可靠性，最高的稳定性，但也有最高的发热量，最高的噪音。NAS硬盘则是都相对做了优化，可靠性降低一点，稳定性降低一些，发热量降低一些，噪音降低一些。NAS硬盘Pro系列则是优化力度加大，尽量保证拥有类似企业盘的可靠性、稳定性、性能的情况下，把发热、噪音以及NAS场景应用优化再提高一些。

建议一般用户选择NAS专用硬盘即可，而对性能有较高要求且不介意稍高的噪音影响可以选择企业级硬盘。当然这个建议还是需要结合本身的预算来选择，在预算不高的情况下，选着监控盘、普通硬盘凑合用也不失为一个可行方案。

### 二、推荐什么品牌的硬盘？

就我个人而言，使用的硬盘不多，而且也没有可靠的售后数据查询途径及数据，只能用一些大佬总结的内容供小伙伴参考：

**可靠程度**：西数>=希捷>东芝

**售后方便性**：希捷>西数>东芝

**声音**：西数>希捷>东芝

**总结**：希捷、西数基本差不多，对声音有要求选东芝

### 三、需要多大的硬盘？

在选购NAS硬盘的时候首先要搞清楚存储需求：

- 普通家用——存储家庭成员手机、PC等各端设备的数据、照片
- 影音玩家——保存各种影音资源；
- 自媒体工作者——视频剪辑生产力工具；

对应后两类的场景需求，NAS硬盘的容量要求是很高的，硬盘容量越大越好，个人建议提高单盘位承接的容量密度，最好选择单盘16T或以上，具体盘位数量根据自身应用情况选择。

对于第一类的普通家用用户来说，容量需求会小很多。以Nas·U的自身使用情况来说，家庭成员的各类数据、照片 + 个人工作资料备份，8T的硬盘容量基本能满足。

### 四、SAS 盘可以用？

SAS硬盘支持更高级的SCSI指令集，能提供更高的数据传输速率和更好的并发处理能力。此外，SAS硬盘还支持双向全双工模式，为同时发生的读写操作提供了两路活动通道，**SAS硬盘的性能优于SATA硬盘。**

同时，SAS硬盘满足企业级应用，在设计上更注重可靠性和耐用性，它们通常采用更高质量的组件和更严格的测试标准，**可靠性和耐用性更高**。

但是，SAS 盘一般都是服务器上用的，市面上能买到的 SAS 盘也基本都是服务器上拆下来的。而二手硬盘市场水太深，部分商家以次充好，存在很多矿盘、清零盘，**不建议新手购买**。

另外，需要用SAS硬盘，也要考虑NAS设备是否满足所需条件：

1. 需要硬盘笼背板支持，目前市面上大部分常见的带硬盘背板的 NAS 机箱都支持 SAS 盘，具体可以查询机箱产品详情页或者咨询商家。
2. 需要主板支持，大部分的服务器主板通常情况下是原生带SAS接口的，但消费级主板普遍支持的都是 SATA 接口，要在该类主板上用SAS盘就需要用到阵列卡（直通卡）。

### 五、是否需要SSD缓存盘？

1. 大文件的读写对于普通机械硬盘来说能达到200MB/s的水平，对于千兆、2.5G网络，其瓶颈在于网络带宽限制，如果你平常就存储大文件，或者作为影音服务器，单纯增加缓存意义不大。
2. 但如果你有搭建Docker跑各类服务，小文件读写会比较频繁，增加SSD缓存也不失为提高NAS读写性能的方式。
3. 而对于万兆网络用户来说，网络带宽已经不是限制数据读写的瓶颈，而且该类用户一般有在线剪辑、在线游戏存储读取等需求，可以用两块M2固态组成 Raid1 作为读写缓存，来提高读写速度，或者通过多块盘组成 Raid 阵列（如 raid5）来提高读写速度 。

### 六、要不要组Raid？

还有一个比较现实的问题，就是我们如何来保护NAS硬盘中的数据，千万分之一的概率硬盘突然毫无征兆的暴毙，数据遗失的概率就是100%。不能将鸡蛋放在同一个篮子里就是同样道理。

常见的数据保护措施有**组Raid**、**异地备份**（含云端备份）以及**冷备份**三种形式；

1. 选用组Raid阵列，就意味着至少要占用一个硬盘做冗余，对于盘位本来不多的用户来说使用效率并不高。而且通常组Raid的硬盘是同一批次采购应用的，当其中一个硬盘出现问题时，那就意味着其他硬盘也存在同样隐患。服务器厂商的常规做法是数据恢复迁移后，更换所有疑似有隐患的硬盘，但对于普通消费者来说这个成本就难以接受了。个人觉得组Raid的方式并不适合普通用户作为数据保护的首选措施。诚然如上一节提及组Raid可以提升读写速率，但前提是具备万兆网络环境，不然看不到效果。
2. 在异地（如老家，父母家等）用一台设备定时备份，如果说你本来就已经有设备，这个方法还是比较靠谱；但如果说要重新配置一台新设备，那成本相对高了，也不是Nas·U所推荐的方式；异地备份的模式我建议可以考虑将核心数据上传到Onedrive、阿里云盘等进行云端备份，即使本地数据有损坏也不会造成太大损失。
3. 冷备方式也是比较可行的，定期使用移动硬盘定期备份核心数据就好了。

综上来说，不要把你的重要数据完全依靠硬盘的可靠性，而在于保护措施的多样性。我个人建议：**①、不要把组Raid作为普通用户保护数据的首选；②、理顺存储数据的重要层级；③、核心数据定时云端备份、冷备份；**；④、边缘数据（如下载的电影、多年用不上的资料）用普通Basic模式存储即可。

另外，还有极端点做法是小伙伴经常会听到的“321”原则，即3个备份副本，2种不同存储介质，1个离线备份，但个人觉得有点过犹不及，也不是我推崇的数据保护措施。