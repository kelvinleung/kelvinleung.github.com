---
title: 荔枝直播小结（上）
date: 2016-08-27 11:24:36
tags: 荔枝
---

一周的忙碌终于过去了，周末想睡个懒觉又被快递吵醒，真是日了狗的……

好吧，其实我6点就醒了一次，开了电脑看了一下后台的数据，然后又关上电脑睡去。这些天基本都是这个状况，项目刚上线，生怕会出什么问题，担心昨天的数据太差，如果真的太差的话，这个刚诞生的“宠儿”也许就会被永久的抛弃了吧。

Anyway，总算是挤出了时间来给直播做个小结：

## 旧直播规划

其实直播从05年底就已经有在规划了，当时由于我们和湖南广电（就是传说中的芒果啦）那边有一些资源上的合作，就想在广州和湖南设置荔枝自己的直播室，邀请芒果台的一些大咖过来开直播的节目，以及一些芒果的节目，以音频直播的形式在荔枝播出。

这是最初我们对于直播的定位，是“荔枝”的直播，而不是“人人都是播客”的直播。

从提出要做直播，到迭代完成第一个版本推出去，其中还拖了不少的时间。在我们做完荔枝3.0（智能推荐）大改版之后，我们把直播的功能正式提上了日程。

由于我们是在自己的直播室里做直播，因此对主播端的功能能简即简，重点放在了听众端，也就是客户端的功能设计上。经过讨论我们确定了要做的主功能点有三个：

1. 直播模块：能够自由配置，方便运营，显示直播预告及节目信息
2. 直播间的收听：基础功能，收听、暂停、重连等
3. 直播间的互动：发送、查看弹幕

然而当时正值16年农历新年前，节前大概还能开一到两个迭代，而上面三个直播的功能估计年前是完成不了的，因此只能砍开来做，先实现基本功能，让直播的整个流程至少能跑起来。多次讨论后我们决定，第一期先上线模块及基础的收听功能，互动这一块能赶就赶，不行的话等春节回来后再上。

更悲催的是，由于当时 iOS 还落后了安卓一个迭代，因此我们不得不把 iOS 的直播功能安排得更后，节前只争取上一个安卓的版本。与此同时，我们在广州的直播间也进行了装修及录音设备的购置，一切都按部就班地进行着。

## 旧直播上线

非常感谢各位码农的汗水，直播的第一版在一砍再砍之后，终于赶在春节前出来了，虽然只有安卓版，虽然只能听不能互动，但毕竟是出来了。

考虑到没有互动的直播缺乏爆点，而直播的节目是我们发起的，在第一版上线之后，我们并没有马上安排内容去运营，而是希望能等互动功能上了之后再推。因此我们可以说是“悄无声息”地上线直播的功能，没有任何的宣传，界面上也不可见，用户完全不知道有这个功能。

然而，接下来的迭代的确是把互动给加上了，但却并不是加在直播之上……

## 互动转型

春节放假前的15年荔枝年会上，厂长提出了两大目标：

1. 每日的弹幕数量达到一百万条
2. 为一个主播创造一百万的收入

以上两大目标的提出，标志着荔枝正式向互动及营收的方向转型。只有互动率提高了，才能有更大的营收可能。

于是我们在节前最后一个迭代上线了弹幕的功能，当然这里的弹幕并不是直播的弹幕，而是录播节目的弹幕，其实就是节目评论的另一种展现形式。运营上，也是往弹幕互动上面发力，成立了产品运营团队，负责策划各种功能、活动来刺激用户发送弹幕。

接着产品运营团队更是调整成了互动娱乐团队，除了继续激励用户的互动，同时也在营收方面开始发力。一期推出了弹幕皮肤的功能，但效果不太理想，用户的付费意欲并不高。二期又尝试在虚拟道具上下功能，推出了给主播赠送荔枝的功能。

如果说第一期弹幕皮肤是一次尝试，那么第二期的送荔枝就应该要吸取经验，谨慎多了才对。在规划阶段我们进行过多次讨论，当时争论的点是送荔枝究竟是给“什么”送？是送主播，还是送内容（节目）？Teddy（虚拟道具项目的 PM）始终坚持，这一功能的定位是“打赏”，是对于“主播”的打赏，是因为对于主播的认同，才产生了赠送的动机。然而我并不太认同这样的定位，对于这个需求、整个付费的场景，我持怀疑的态度。

除了定位，场景的问题，功能的设计上，我同样产生很大的怀疑，尤其是其中最大争议的一点：

* *自己能给自己送荔枝？*

这一设定直接导致荔枝榜（以赠送荔枝的数量排名）沦为了各种微商、成功学刷量换曝光的最佳场所。最初希望通过设置榜单，来鼓励用户为自己喜爱的主播送荔枝打榜的构思，并没有得到实现。整个荔枝榜都是这些恶心的内容，并且荔枝榜还被放到了首页最显眼的位置，抢占了内容运营的位置，实在不见得是一件好事。我想不明白的是，既然设计了榜单的功能，肯定是需要考虑恶意刷榜的防范机制的，然而实际出来的却并没有，看不懂。

无论如果，只要是有人刷榜，我们就有得分成。先不管这种模式是否良性，毕竟也是有了一定的营收。因此往后所有的资源都往这一块业务上倾斜，优先级也调到了最优。而另一边，直播功能基本上已经被所有人遗忘，正式成为了“弃儿”。

哦对了，iOS 的直播功能拖到5月底才上线，即便是上齐了两个平台，整个直播功能也没有正式运营过。整个运营团队似乎从来就不把这当作一回事，只约了《坏蛋调频》来试水了一次，效果并不理想（当时只有安卓版本）。之后还做了几次商业性质（广告）的直播，像 TCL 手机的发布会什么的，效果就不说了。

以上就是对旧版直播的一个回顾，从它艰难的诞生，一直不受重视，到最终被彻底遗忘，甚至在用户看来，荔枝就像从来不曾有过直播这个功能一样。

下半部分的内容将会对新版直播前两期的迭代，同样进行一次回顾，留到有时间再码吧，大致内容如下：

1. 新直播规划
2. 新直播一期上线
3. 新直播运营
4. 新直播二期上线
5. 新直播的中期规划