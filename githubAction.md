## Github Action 使用教程说明
 - Fork [此仓库项目](https://github.com/lxk0301/scripts) > 点击右上角 fork 按钮即可，[再不会可看此图](icon/fork.png)

 - 然后参考 github@ruicky 写的特别详细的小白教程[@ruicky 教程](https://ruicky.me/2020/06/05/jd-sign/) (**注：此 [@ruicky 教程](https://ruicky.me/2020/06/05/jd-sign/) 里面获取 ck 的方法不对。参考下面两种获取京东 cookie 的方式才对。**)

   

### 注意几个地方就行



####  **fork 后必须修改一下文件，才能执行定时任务**
  - 比如修改一下`README.md`文件 (enter 键回车)，再提交   
  - 不知怎么修改 README.md 文件的看[这个图](icon/action3.png);
    



#### 京东 Cookie

  - Secret 新增`JD_COOKIE`，填入 cookie 信息，多账号的 cookie， 使用`&`或者换行隔开 (两种方法)
  
  - 方式已一：`&`号隔开示例 (注：后面的英文引号`;`不可缺失)
    如 `账号一 cookie&账号二 cookie&账号三 cookie`，再多账号就依次类推即可
    ```
    pt_key=xxx1;pt_pin=xxx1;&pt_key=xxx2;pt_pin=xxx2;&pt_key=xxx3;pt_pin=xxx3;
    ```
  - 方式二：按`Enter`键换行隔开示例 (这里给下三个账号的示例)
    ```
    pt_key=bbbbbbbbbbbbbb;pt_pin=aaaaaaa;
    pt_key=cccccccc;pt_pin=dddddddd;
    pt_key=eeeeeeeee;pt_pin=ffffffff;
    ```
  - 京东 cookie 获取看这里
    - [浏览器获取京东 cookie 教程](/backUp/GetJdCookie.md) 或者 [插件获取京东 cookie 教程](/backUp/GetJdCookie2.md)
    - IOS 代理软件 (Surge, Quantumult X, Loon) 等用户有使用过 BoxJs 的，可在 BoxJs 里面提取京东 cookie(打开 BoxJs -> 底部中间的 `应用` -> NobyDa 脚本订阅 -> 京东 (多合一签到) -> 点击会话右上方的三个点点 -> 修改会话 -> 全选复制即可)，再不会看此[图文教程](icon/jd8.png)
    
    
#### Action 里面 cron 时间

  - 此时间是国际标准时间，与北京时间不同，github action 写 16 点才表示北京时间 0 点，具体可参考下面两个链接写 cron

  -  [参考链接一](https://datetime360.com/cn/utc-beijing-time/) ， [参考链接二](http://www.timebie.com/cn/universalbeijing.php)

  - 根据使用经验发现 github action 会有延迟现象，一般会延迟 15 分钟左右吧。比如 action 设置`北京时间 16:00`运行，action 其实要`16:15 左右`才会执行脚本的。



#### 如何查看 Action 运行情况

   - [查看运行状态](icon/action1.png)
    
   - [查看运行日志](icon/action2.png)  



#### 如何禁用单个或者多个脚本 (Action)

   - 操作步骤[看此图](icon/disable-action.jpg) 



#### Fork 后 Action 未运行

  > 是因为`/.github/workflows/`路径里面的`.yml`后缀文件里面的 cron 时间未到，如需立马看到效果

  - 方法：在自己仓库，手动点击仓库的右上角`star 图标按钮`即可，稍后就能看到运行 
  - 注：之后如果想单独运行某一个脚本 (此处的前提条件是执行过上面的方法)，手动点击 Run workflow [根据此图片示例操作](https://user-images.githubusercontent.com/21308593/93980945-e28ab000-fdb1-11ea-977c-c50705e79ac3.png) ，再次点一下`Actions`图标即可看到效果 (或者等待 10 秒左右也可) 



#### 自动同步 Fork 后的代码

  > 此部分内容由 tg@wukongdada 和 tg@goukey 提供

  - 方案 A - 强制远程分支覆盖自己的分支 (**新手推荐使用**)
  
      1. 参考 tg@wukongdada 这篇教程 [保持自己 github 的 forks 自动和上游仓库同步的教程](/backUp/gitSync.md) ， 安装[pull 插件](https://github.com/apps/pull) 并确认此项目已在 pull 插件的作用下（参考 @twukongdada 这篇教程文中 1-d）
      
      2. 确保.github/pull.yml 文件正常存在，yml 内上游作者填写正确 (此项目已填好，无需更改)。
      
      3. 确保 pull.yml 里面是`mergeMethod: hardreset`(默认就是`hardreset`)。
      
      4. ENJOY！上游更改三小时左右就会自动发起同步。
    ```
    # 方案 A 可参考这里
    version: "1"
    rules:                      # Array of rules
      - base: master            # Required. Target branch
        upstream: lxk0301:master    # Required. Must be in the same fork network.
        mergeMethod: hardreset  # Optional, one of [none, merge, squash, rebase, hardreset], Default: none.
        mergeUnstable: true    # Optional, merge pull request even when the mergeable_state is not clean. Default: false
    ```
  - 方案 B - 保留自己仓库已修改过的文件 (**需修改脚本或者提 PR 的使用**)
    
    > 上游变动后 pull 插件会自动发起 pr，但如果有冲突需要自行**手动**确认。
    > 如果上游更新涉及 workflow 里的文件内容改动，需要自行**手动**确认。
    
    1. 参考 tg@wukongdada 这篇教程 [保持自己 github 的 forks 自动和上游仓库同步的教程](/backUp/gitSync.md) ， 安装[pull 插件](https://github.com/apps/pull) 并确认此项目已在 pull 插件的作用下（参考 @twukongdada 这篇教程文中 1-d）
    2. 确保.github/pull.yml 文件正常存在，yml 内上游作者填写正确 (此项目已填好，无需更改)。
    3. 将 pull.yml 里面的`mergeMethod: hardreset`修改为`mergeMethod: merge`保存。
    4. ENJOY！上游更改三小时左右就会自动发起同步。
    ```
    # 方案 B 可参考这里
    version: "1"
    rules:                      # Array of rules
      - base: master            # Required. Target branch
        upstream: lxk0301:master    # Required. Must be in the same fork network.
        mergeMethod: merge  # Optional, one of [none, merge, squash, rebase, hardreset], Default: none.
        mergeUnstable: true    # Optional, merge pull request even when the mergeable_state is not clean. Default: false
    ```
  - 方案 C - 利用 github-action 定时 cron 更新同步 (**新手推荐使用**)
  
    > 效果和方案 A 一样（即：强制更新覆盖）
    
    新建 secret，`Name`为`PAT`，填写的`Value`值需要去申请 Personal access tokens，申请教程[看此处](https://www.jianshu.com/p/bb82b3ad1d11) 记得勾选`repo`权限就行



#### 下方提供使用到的 **Secrets 全集合**

| Name                    |   归属   | 属性   | 说明                                                         |
| :---------------------: | :----------: | --------- | ------------------------------------------------------------ |
| `JD_COOKIE`             |   京东   | 必须   | 京东 cookie，多个账号的 cookie 使用`&`隔开或者换行。具体获取参考[浏览器获取京东 cookie 教程](/backUp/GetJdCookie.md) 或者 [插件获取京东 cookie 教程](/backUp/GetJdCookie2.md) |
| `JD_BEAN_STOP`          |   京东   | 非必须   | 自定义延迟签到，单位毫秒。默认分批并发无延迟。延迟作用于每个签到接口，如填入延迟则切换顺序签到 (耗时较长)，如需填写建议输入数字`1` |
| `JD_DEBUG`              |   脚本打印 log   | 非必须   | 运行脚本时，是否显示 log，默认显示。改成 false 表示不显示，注重隐私的人可以在设置 secret -> `Name:JD_DEBUG,Value:false` |
| `PUSH_KEY`              |   微信推送   | 非必须 | cookie 失效推送[server 酱的微信通知](http://sc.ftqq.com/3.version) |
| `BARK_PUSH`             |   BARK 推送   | 非必须 | cookie 失效推送 BARK 这个 APP，填写内容是 app 提供的`设备码`，例如：https://api.day.app/123 ，那么此处的设备码就是`123`，再不懂看 [这个图](icon/bark.jpg) |
| `BARK_SOUND`            |   BARK 推送   | 非必须 | bark 推送声音设置，例如`choo`，具体值请在`bark`-`推送铃声`-`查看所有铃声` |
| `TG_BOT_TOKEN`          |   telegram 推送   | 非必须 | tg 推送，填写自己申请[@BotFather](https://t.me/BotFather)的 Token，如`10xxx4:AAFcqxxxxgER5uw` , [具体教程](https://github.com/lxk0301/scripts/pull/37#issuecomment-692415594) |
| `TG_USER_ID`            |   telegram 推送   | 非必须 | tg 推送，填写[@getuseridbot](https://t.me/getuseridbot)中获取到的纯数字 ID, [具体教程](https://github.com/lxk0301/scripts/pull/37#issuecomment-692415594) |
| `DD_BOT_TOKEN`          |   钉钉推送   | 非必须 | 钉钉推送[官方文档](https://ding-doc.dingtalk.com/doc#/serverapi2/qf2nxq) ，只需`https://oapi.dingtalk.com/robot/send?access_token=XXX` 等于符号后面的 XXX， 注：如果钉钉推送只填写`DD_BOT_TOKEN`，那么安全设置需勾选`自定义关键词`，内容输入输入`账号`即可，其他安全设置不要勾选 |
| `DD_BOT_SECRET`         |   钉钉推送   | 非必须 | 密钥，机器人安全设置页面，加签一栏下面显示的 SEC 开头的字符串 ，注：填写了`DD_BOT_TOKEN`和`DD_BOT_SECRET`，钉钉机器人安全设置只需勾选`加签`即可，其他选项不要勾选，再不懂看 [这个图](icon/DD_bot.png) |
| `IGOT_PUSH_KEY`         |   iGot 推送   | 非必须 | iGot 聚合推送，支持多方式推送，确保消息可达。 [参考文档](https://wahao.github.io/Bark-MP-helper ) |
| `PET_NOTIFY_CONTROL`    | 东东萌宠推送开关  | 非必须 | 控制京东萌宠是否静默运行，`false`为否 (发送推送通知消息),`true`为是 (即：不发送推送通知消息)              |
| `FRUIT_NOTIFY_CONTROL`  | 东东农场推送开关  | 非必须 | 控制京东农场是否静默运行，`false`为否 (发送推送通知消息),`true`为是 (即：不发送推送通知消息)              |
| `JD_JOY_REWARD_NOTIFY`  | 宠汪汪兑换京豆推送开关  | 非必须 | 控制 jd_joy_reward.js 脚本是否静默运行，`false`为否 (发送推送通知消息),`true`为是 (即：不发送推送通知消息) 
| `JD_818_SHAREID_NOTIFY`  | 京东 818 互助码通知开关  | 非必须 | 控制 jd_818.js 脚本是否在获取互助码后通知，`true`为是 (发送推送通知消息),`false`为否 (即：不发送推送通知消息)              |
| `JOY_FEED_COUNT`        | 宠汪汪喂食数量  | 非必须 | 控制 jd_joy_feedPets.js 脚本喂食数量  ，可以填的数字 10,20,40,80 ，其他数字不可。|
| `JOY_HELP_FEED`         | 宠汪汪帮好友喂食  | 非必须 | 控制 jd_joy_steal.js 脚本是否给好友喂食，`false`为否，`true`为是 (给好友喂食)              |
| `JOY_RUN_FLAG`          | 宠汪汪参加双人赛跑  | 非必须 | 控制 jd_joy.js 脚本是否参加双人赛跑，`false`为否，`true`为是，脚本默认是`true`              |
| `MARKET_COIN_TO_BEANS`  | 京小超兑换京豆数量  | 非必须 | 控制 jd_blueCoin.js 兑换京豆数量，可输入值为`20`或者`1000`的数字或者其他商品的名称，例如`碧浪洗衣凝珠`              |
| `MARKET_REWARD_NOTIFY`  | 京小超兑换奖品推送开关  | 非必须 | 控制 jd_blueCoin.js 兑换奖品成功后是否静默运行，`false`为否 (发送推送通知消息),`true`为是 (即：不发送推送通知消息)             |
| `SUPERMARKET_UPGRADE`   |  京小超自动升级  | 非必须 | 自动升级，顺序：解锁升级商品、升级货架，`true`表示自动升级，`false`表示关闭自动升级 |
| `BUSINESS_CIRCLE_JUMP`  |  京小超自动更换商圈  | 非必须 | 小于对方 300 热力值自动更换商圈队伍，`true`表示运行，`false`表示禁止 |
| `SUPERMARKET_LOTTERY`   |  京小超抽奖  | 非必须 | 每天运行脚本是否使用金币去抽奖，`true`表示抽奖，`false`表示不抽奖 |
| `FRUIT_BEAN_CARD`       |  农场使用水滴换豆卡  | 非必须 | 农场使用水滴换豆卡 (如果出现限时活动时 100g 水换 20 豆，此时比浇水划算，推荐换豆),`true`表示换豆 (不浇水),`false`表示不换豆 (继续浇水)，脚本默认是浇水 |
| `UN_SUBSCRIBES`         |  jd_unsubscribe.js  | 非必须 | 共四个参数，换行隔开。四个参数分别表示`取关商品数量`,`取关店铺数量`,`遇到此商品不再进行取关`, `遇到此店铺不再进行取关`，[具体使用往下看](#取关店铺secret的说明)|
| `FruitShareCodes`       |  东东农场互助码  | 非必须 | 填写规则请看 [jdFruitShareCodes.js](/jdFruitShareCodes.js) 或见下方[互助码的填写规则](#互助码的填写规则) |
| `PETSHARECODES`         |  东东萌宠互助码  | 非必须 | 填写规则请看 [jdPetShareCodes.js](/jdPetShareCodes.js) 或见下方[互助码的填写规则](#互助码的填写规则) |
| `PLANT_BEAN_SHARECODES` |  种豆得豆互助码  | 非必须 | 填写规则请看 [jdPlantBeanShareCodes.js](/jdPlantBeanShareCodes.js) 或见下方[互助码的填写规则](#互助码的填写规则) |
| `SUPERMARKET_SHARECODES`|  京小超商圈互助码  | 非必须 | 填写规则请看 [jdSuperMarketShareCodes.js](/jdSuperMarketShareCodes.js) 或见下方[互助码的填写规则](#互助码的填写规则) |



##### 互助码的填写规则

  > 互助码如何获取：运行相应脚本后，在日志里面可以找到。(如何查看日志上面有写，详见 如何查看 action 运行情况)

同一个京东账号的好友互助码用 @隔开，不同京东账号互助码用&或者换行隔开，下面给一个文字示例和具体互助码示例说明

两个账号各两个互助码的文字示例：

  ```
京东账号 1 的 shareCode1@京东账号 1 的 shareCode2&京东账号 2 的 shareCode1@京东账号 2 的 shareCode2
  ```

 两个账号各两个互助码的真实示例：
  ``` 
0a74407df5df4fa99672a037eec61f7e@dbb21614667246fabcfd9685b6f448f3&6fbd26cc27ac44d6a7fed34092453f77@61ff5c624949454aa88561f2cd721bf6&6fbd26cc27ac44d6a7fed34092453f77@61ff5c624949454aa88561f2cd721bf6
  ```



#### 取关店铺 secret 的说明

 > secret 依次是`取关商品数`,`取关店铺数`,`遇到此商品不再进行取关`,`遇到此店铺不再进行取关`

例如我要取关 `100`个商品，`100`个店铺，商品遇到商品关键字 `iPhone12` 停止取关，店铺遇到 `Apple 京东自营旗舰店` 不再取关
则使用`换行`或者`&`隔开即可，
下面给出换行隔开示例：

 ```
100
100
iPhone12
Apple 京东自营旗舰店
 ```

下面给出`&`符号隔开示例：
 ```
100&100&iPhone12&Apple 京东自营旗舰店
 ```

#### 关于脚本推送通知 (微信 server 酱推送通知，bark app 推送通知，telegram 机器人推送通知，钉钉机器人推送通知，iGot 聚合推送)

  > 如果你填写了上面五种推送通知方式中的某一个通知所需 secret，那么脚本通知情况如下：

  > 目前默认只有 jd_fruit.js,jd_pet.js,jd_bean_sign.js,jd_818.js 四个脚本每次运行后都通知

  ```
jd_plantBean.js 是每周一收集京豆后通知一次，
jd_joy_reward.js 是每次兑换到了京豆通知一次，
jd_blueCoin.js 是每次兑换到了奖品通知一次，
jd_818.js 是每次获取新的互助码会通知一次，以帮助您快速上车，
其余的脚本平常运行都是不通知，只有在京东 cookie 失效后，才会推送通知    
  ```
  - 如果填写了推送通知所需的 secret 后，运行上面有通知的脚本，还没收到通知的话，请自行查看 action 运行日志 (如何查看日志教程请看上面的`如何查看 action 运行情况`)，里面会推送通知发送失败的 log


​    
##### 参考文献
[GitHub Actions 手动触发方式进化史](https://p3terx.com/archives/github-actions-manual-trigger.html)
    
[GitHub Actions 入门教程](https://p3terx.com/archives/github-actions-started-tutorial.html)

[github@ruicky 教程](https://ruicky.me/2020/06/05/jd-sign/)
