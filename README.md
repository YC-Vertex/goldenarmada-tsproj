# goldenarmada-tsproj

Private repo of team Golden Armada in THU AI Project

---

## 20190514 03:00 - zyc:

### debug，修修补补

改了各种小bug，其中比较重要的包括enemy队列的更新方式、捡物品的策略，并且增加了跑图的稳定性（寻路基本没问题，但是city图在其它情况下有可能会卡）

在线的平台炸了，所以没太多机会实战测试，不过反正昨天的ai被今天的吊着打

捡物品和打人的策略基本成型了，下一步任务大概是加入职业特色，同时把city图（和其它图）的寻路再完善一下

---

## 20190513 02:30 - zyc:

### 把新东西整合到比赛逻辑里

主要是把寻路功能整合到比赛逻辑里，然后改了一些捡东西、打人的策略

各种零零碎碎的，不太好解释，写不动了

---

## 20190512 00:45 - zms, zyc:

### 加入寻路功能

rt，寻路函数为`GetMoveAngle(cur_pos, tar_pos)`

基本功能都实现了，不过要考虑一下如何和主函数中的其它指令衔接一下

下一步任务是四个人合作，写Radio和职业特色（主要就是群发Radio和救人）

Radio主要功能就是四个人协同，分别看四个方向（视角++），见到人集火这样

下周四收代码，所以比较紧张，加油

---

## 20190509 16:00 - zyc:

### 加入GAMEPLAYMODE

主要是用来调试用，测试的东西直接放在主函数`if (MODE == TEST) { ... }`里面就可以了，不需要改外面的东西了

---

## 20190508 01:00 - zyc:

### 整理地图、建图

地图带标注图片见`map_*.jpg`，地图带结点标注见`map_*_node.jpg`，原文件见`map_*.sldprt`

地图结点位置和邻接矩阵定义见`0_map.txt`

共有9种地形，分别是`beach` `city` `farmland` `grass` `hill` `pool` `roadA` `roadB` `forest`，其中：

- `beach` `city` `hill`：障碍物复杂，建图建结点寻路

- `farmland` `pool`：有少量挡路障碍物（分别为1个和3个），直接走，判断是否与已知障碍物有交点；如果需要建图叫我我再来弄

- `grass` `roadA` `roadB` `forest`：没有树以外的挡路障碍物，直接横跨，需要加上如果没有移动则改变移动角度的逻辑；其中`forest`我连地图都懒的画了

这一版先大致这样，有bug的话直接改了开branch，如果要改路线以达到更不容易被发现等战术效果，下一个版本再说（甩锅

---

## 20190412 20:30 - zyc:

### merge

把dxr上一个branch merge到master里了，angleDT设置为35.0

关于输出文件f的部分，每次重新打开文件会覆盖，所以我还是用的原来版本

我最开始是直接手改文件名的（每次要改AI\_VOCATION和文件名两个地方），然后`sprintf()`是我直接在注释里加的，没有编译过，所以可能有问题

实在不行就手改文件名吧（233

---

## 20190412 12:10 - dxr：

### 少量的Debug工作

1. 将未定义/已注释的函数的声明、调用部分注释掉，这样才能通过编译。。。

2. VS不让在声明全局变量时调用函数，所以我把对输出文件f的定义挪到了程序开头（并修改了sprintf函数的语法233）

3. 在函数vLoseHp中，定义了被调用的变量angleDT。目前我将该参数设为了0.0.

---

## 20190411 - zyc: 

主要负责把框架写好了，但是逻辑用的都是最简单的逻辑

目前主要工作是改逻辑，然后调参数、测试什么的

### 关于Debug信息：

简单说一下，现在的debug信息从文件输出，文件会在platform/playback底下

把所有debug信息都注释掉了（因为昨天晚上传到网站上），如果要改回来，直接搜索`Debug Info`，然后把注释一个一个去掉

zrk今天去群里确认了一下，.pb文件是用来可视化debug的，不是用来看的，我之前理解错了

### 关于主函数逻辑框架：

主函数里三个主要部分：（240行、250行、300行）

1. 对战场的分析（上次讨论说可删，但是我怕之后要处理声音什么的还是要用）里面基本没东西

2. 确定ai的行为和参数，写了几个函数，写策略的时候直接改函数就可以，如果要改主函数里面的东西或者要其它接口，记得说一声

3. 按照行为和参数执行，这个基本确定好不用再改了，其它的就是一些跳伞操作（前面）和后处理（后面）

### 关于程序这么长我该去哪找：

- 一些标注：可以直接搜索`Temporary` `Unfinished` `Unused` `Debug Info`

	分别代表暂时的逻辑（简单逻辑或者不确定好不好用的）、没写完的代码（可能会编译不过的）、没用到的代码、Debug输出信息

- 一些函数和全局变量的分类：`item queue` `ai behavior` `ai decision` `ai evaluation` `ai audio` `ai sight`

	声明和定义附近都会有这些分类作为注释

	分别代表物品的优先级队列、ai的行为（主要是结构体定义）、ai的决策（各种函数）、ai对局势的判断（没用）、ai对听觉（暂时没用，之后写radio在这部分）、ai的视觉（处理敌人也在这部分）

- 一些命名规则：全局常量、函数、struct、enum以`v`开头，全局变量以`ai`开头

- 一些索引：`item`声明(50 ~ 80)，`ai`声明(80 ~ 160)，主程序(160 ~ 380)，通用函数(390 ~ 410)，`item`定义(420 ~ 670)，`ai`定义(680 ~ 1100)


### 关于现在可以调用什么：

- 物品：所有的武器/药品，不要从bag里找，以weapon为例，直接`aiWeaponCase[...]`（记得先用`aiWeaponCase.size() != 0`检查一下）

	如果要有选择的用物品，调用`vFilterWeapon(...)`（定义自找），如果找到了会把`vFilterWeaponFlag = true`

	在使用的时候自动检查如果`flag == true`就用`aiFilterWeaponCase[0]`，如果是`false`就用`aiWeaponCase[0]`

- 行为：详见`struct vAiBehavior`定义，移动、转身、射击的时候需要两个angle，嗑药、发消息的时候需要两个int类

	全局变量`aiBehavior`里面存当前的行为（作为决策和执行的接口），前几帧的行为用`aiPrevAct[...]`得到

- 敌人：向量`aiEnemy`里面存敌人的id（按优先级高到低），要某个敌人的信息用`aiKV[id]`得到敌人的info struct

### 关于这两天打架的一些结果：

昨天晚上的版本和电脑ai打了几把，故意调的降落点大概在50~100格左右，应该是三局全胜，其中一局剩一人，两局无伤

半夜扔到平台上打了几把，当前ai强度大概是：游客 > Haibara > 我们 ≈ yyr，后面的好像跑毒都跑不好

交火感觉基本没有发生过，好像还是跑毒算法优先

最新这个因为加了ai decision的几个函数所以还没测试，但是逻辑和昨天晚上的是一样的

### 关于这两天Debug的一些结果：

- `shoot()`和`move()`函数的参数是相对于`info.self.view\_angle`的相对角度，`info.self.move\_angle`好像并没有卵用

	`aiBehavior`结构体里面存的也都是相对于`view\_angle`的角度

	所以如果得到的是绝对角度的话，记得减去`info.self.view\_angle`再对`aiBehavior`进行赋值

	比如`aiKV[id].rel\_polar\_pos.angle`、`info.items[i].polar\_pos.angle`都是相对角度，直接当`aiBehavior`里面参数就可以

	而自己算出来的毒圈中心角度是绝对角度，需要减去`info.self.view\_angle`

- 用xy坐标算角度的时候建议用`atan2(dy, dx)`，注意参数顺序不要反

	并且得到的是弧度制，记得`*180/M_PI`

	我在通用函数里面也给了计算距离和角度的函数`vCalcDist()`和`vCalcAngle()`

- 还有不要写`double == double`这些东西应该不用多说

### 关于实现一些逻辑的初级想法：

避障可以把周围（1 chunk？）的障碍物四个角的坐标扩大大概2.5格左右，作为结点然后建图算最短路什么的

### 后：

反正大致就是框架写好了，然后不用直接改主函数，可以分成子任务了（改函数）

个人感觉目前主要方向大概是：写跑毒避障算法、改主函数的逻辑顺序和策略、做通讯、加入更多职业特征

大家各自接一下锅，有空就写一点

（偷偷：其实挺希望初赛前几直接保个决赛的，期中就不用肝代码了23333

---
