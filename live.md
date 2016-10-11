### 登录流程
* 客户端->展游sdk -- 得到登录session ->uc服务器 - 得到uctoken ->直播服务器
### 服务器url
* uc服务器 http://42.62.78.20/uc.live.com/index.php
* 直播服务器 http://42.62.78.20:9501/
* 支付服务器 http://42.62.78.20/pay.live.com/index.php
### No.1接口说明
* 请求uc服务器得到uctoken
### No.1请求地址
http://42.62.78.20/uc.live.com/index.php?c=userzc&m=login
### No.1请求参数
```sh
  var param = {
    uId  : "展游sdk uid",
    session : "登录session",
    authkey : "",
    channel : "默认填1",
    mac : "mac地址",
    idfa : "idfa",
    android_id : "android 设备码"s,
    isbind : "账号是否绑定",
    appid : "展游sdk 分配的id"
  }
```
### No.1返回值
```sh
  var re = {
    status  : "1", //-1 参数错误，-2验证失败，1 登录成功
    zcid : "42341312", //展游uid
    uid : "42343", //直播服务uid
    token : "xxxxxxxxxxx" //uctoken 用这个去登录直播服务器
  }
```
### No.2接口说明
使用No.1接口得到的uctoken登录直播服务器，拿到直播服务器的token，拿到这个token，后续服务才能继续请求，并且每个请求都必须带上这个token
### No.2请求地址
http://42.62.78.20:9501/?c=login&m=user_token
### No.2请求参数
```sh
  var param = {
    uctoken  : "UISzgOPdnsUPNhBi5hBxhg", //No.1 得到的 uctoken
  }
```
### No.2返回值
```sh
  var re = {
    status  : "1", //-1 没传token，-2验证失败，-3注册云信账号失败，-4登录失败，-5刷新云信token失败，1 登录成功
    token : "LW6A7onv3A11jqg8SxxF3w", //直播服务器token，后续每个请求都要带上这个token
    uid : "4904739", //直播服务uid
    nimtoken : "07cc12512e1687d2263e7f9db7326091" //云信token
  }
```
### No.3接口说明
使用No.2得到的token获取用户信息
### No.3请求地址
http://42.62.78.20:9501/?c=member&m=get_member_info
### No.3请求参数
```sh
  var param = {
    token_uid  : "4904739", //直播服务uid
    token : "LW6A7onv3A11jqg8SxxF3w", //直播服务器token
  }
  ```
### No.3返回值
```sh
  var re = {
    "status": 1,//只有这一个状态
    "info": {//用户基础信息
        "uid": "6560324",//直播id
        "nickname": "nowhy",//昵称
        "level": 5,//等级
        "exp": 453,//当前经验
        "sex": "0",//性别，1男2女
        "birth": "0",//生日，0代表未设置过
        "job": "",//职业
        "marriage": "0",//婚姻状况，0单身，1已婚，2未婚
        "from": "",//来自哪里
        "bill": "0",//获得的夜票
        "diamond": 7240,//当前钻石
        "all_diamond": 35480,//总共获得过的钻石
        "icon": "http://qzapp.qlogo.cn/qzapp/1105640374/F39CE33CD3210424E364961A744579D3/100", //玩家头像
        "causerie": "",//个性签名
        "status": "0",//账号状态 0正常 2禁言 4封号 5直播隐藏 6禁播 7已审核 10管理员
        "my_care": 4,//我关注的人
        "care_me": 2,//关注我的人
        "levelup_exp": 2200,//升级所需经验
        "auth": 0,//1管理员  2普通认证 
        "icon_small": "http://qzapp.qlogo.cn/qzapp/1105640374/F39CE33CD3210424E364961A744579D3/100",//头像小图
        "icon_middle": "http://qzapp.qlogo.cn/qzapp/1105640374/F39CE33CD3210424E364961A744579D3/100",//头像中图
        "share_url": "http://42.62.78.20/share.live.com/#!/index/6560324/"//直播分享地址
    },
    "wechat_url": "http://42.62.78.20/www.liveweb.com/wechat/?token=RqkpvtcGsba2Acv33Q3hRQ&uid=123" //点击微信红包跳转的webview
    "notice": [//热门列表顶部的公告 列表
        {
            "id": "3", //公告id
            "title": "我是标题"
            "img": "http://img.hb.aicdn.com/c1bb7f3f54c8856f2463cbd9891a400d2b58e2c85b121-Y7SI0r_fw658",//公告图片
            "url": "http://42.62.78.20/live/document.html" //公告跳转地址
        }
    ],
    "room_msg": [//进入直播房间提示的内容
        {
            "id": "4",
            "info": "我们提倡绿色直播，封面和直播内容包含吸烟、低俗、引诱、暴露等都将会被封停账号，网警24小时在线巡查哦！"
        },
        {
            "id": "5",
            "info": "分享也能随机获得1-8个钻石哦，分享越多钻石也越多，而且还能为主播挣到夜币哦~"
        }
    ],
    "gift": [ //礼物列表
        {
            "id": "1", //礼物id
            "name": "甜甜圈", //礼物名称
            "price": "1",//礼物价格
            "type": "1",//礼物类型
            "res": "",//礼物资源图片
            "addexp": "10",//礼物增加经验
            "status": "1"//礼物状态  是否启用
        },
        {
            "id": "2",
            "name": "肥皂",
            "price": "1",
            "type": "1",
            "res": "",
            "addexp": "10",
            "status": "1"
        }
    ],
    "product": [//夜票兑换钻石的订单列表
        {
            "id": "1",//订单id
            "bill": "240",//兑换需要夜票
            "diamond": "100"//兑换获得钻石
        },
        {
            "id": "2",
            "bill": "1030",
            "diamond": "500"
        }
}
```
### No.4接口说明
创建直播房间，得到推流，拉流地址 以及 聊天室房间号
### No.4请求地址
http://42.62.78.20:9501/?c=live&m=start_live
### No.4请求参数
```sh
  var param = {
    token : "LW6A7onv3A11jqg8SxxF3w", //直播服务器token
    name : "首次直播"  //直播标题
  }
  ```
### No.4返回值
```sh
  var re = {
    "status":1, //-1请输入直播标题，-2频道创建失败，-3聊天室房间创建失败，-4记录数据失败，-5直播等级不足，-6禁止直播 1创建成功
    "level": 1,//允许直播的等级
    "op": 0, //2房主,1管理员,0普通用户
    "info":{
        "name":"4904739#xxxxxssss#363", //直播房间名称
        "title":"", //直播话题
        "pushurl":"rtmp://pc3fe87c5.live.126.net/live/cd668d343c9245c3bacccf796a8aa44c?wsSecret=22c16eb151aaa048fb75a6dcd0f32533&wsTime=1468414408", //推流地址
        "httpurl":"http://vc3fe87c5.live.126.net/live/cd668d343c9245c3bacccf796a8aa44c.flv",    //http 拉流地址
        "hlsurl":"http://pullhlsc3fe87c5.live.126.net/live/cd668d343c9245c3bacccf796a8aa44c/playlist.m3u8", //hls 拉流地址
        "rtmpurl":"rtmp://vc3fe87c5.live.126.net/live/cd668d343c9245c3bacccf796a8aa44c", //rtmp拉流地址
        "cid":"cd668d343c9245c3bacccf796a8aa44c", //直播频道房间id
        "uid":"4904739",    //创建者id
        "status":0, //直播状态，忽略
        "dateline":1468414408,   //创建时间戳
        "roomid":3607393    //聊天室房间id
    }
}
```
### No.5接口说明
直播列表，只返回200条，客户端每隔1分钟重新调用一次，下拉刷新的时候也调用一次。
### No.5请求地址
http://42.62.78.20:9501/?c=live&m=get_live_list
### No.5请求参数
```sh
  var param = {
    token : "LW6A7onv3A11jqg8SxxF3w", //直播服务器token
    t:1//0为热度列表，1为最新列表,2待审核,3隐藏,4特殊隐藏
  }
  ```
### No.5返回值
```sh
  var re = {
    "status": 1,//-1 直播列表为空，-2用户信息获取失败
    "list": [
        {
            "id": "1",//记录id，没啥作用，唯一值
            "name": "4904739#xxxxx#860",//房间名称
            "title": "",//房间话题
            "pushurl": "rtmp://pc3fe87c5.live.126.net/live/550aee3d69c342a9999ec54cad3f0d50?wsSecret=3ad3d8adab7cdb03bce65277dc5241ba&wsTime=1468403691",//推流地址
            "httpurl": "http://vc3fe87c5.live.126.net/live/550aee3d69c342a9999ec54cad3f0d50.flv",//http 拉流地址
            "hlsurl": "http://pullhlsc3fe87c5.live.126.net/live/550aee3d69c342a9999ec54cad3f0d50/playlist.m3u8", //hls 拉流地址
            "rtmpurl": "rtmp://vc3fe87c5.live.126.net/live/550aee3d69c342a9999ec54cad3f0d50", //rtmp 拉流地址
            "cid": "550aee3d69c342a9999ec54cad3f0d50", //房间id
            "roomid": "3609065", //聊天室id
            "uid": "4904739", //创建者id
            "filename": "", //忽略该字段
            "status": "0", //直播状态 
            "dateline": "4294967295", //直播创建时间戳
            "nickname": "春海蓝", //创建者名称
            "level": "1", //创建者等级
            "icon": "http://cdn.bdwsw.zhanchenggame.com/liveimg/icon/girl8.jpg", //创建者头像
            "birth": "0", //创建者生日，0代表未设置过
            "job": "", //创建者职业，空代表为设置过
            "sex": "0", //性别，1男，2女，0 未设置过
            "causerie": "", //个性签名
            "marriage": "0", //婚姻状况，0单身，1已婚，2未婚
            "from": "",  //来自哪里
            "online": "10", //当前观看人数 
            "auth": 0, //1管理员  2普通认证 
        }
    ]
}
```
### No.6接口说明
关闭直播间 主播使用
### No.6请求地址
http://42.62.78.20:9501/?c=live&m=end_live
### No.6请求参数
```sh
  var param = {
    token : "LW6A7onv3A11jqg8SxxF3w", //直播服务器token
  }
  ```
### No.6返回值
```sh
  var re = {
    "status": 1,
    "care_me" : 10, //新增粉丝数
    "bill" : 231, //本次直播获得夜票
    "change": { //更新的数据
        "member": { //更新用户数据
            "level": "3", //等级
            "exp": "19",//当前经验值
            "levelup_exp": 150//当前等级总经验
        }
    }
}
```
### No.7接口说明
进入直播间，该接口返回初始的在线人数和主播获得的映票（暂时这么称呼），后续获得的奖励通过聊天室推送过去。
### No.7请求地址
http://42.62.78.20:9501/?c=live&m=look
### No.7请求参数
```sh
  var param = {
    token : "LW6A7onv3A11jqg8SxxF3w", //直播服务器token
    uid : "4904855", //房间创建者uid
    id : "12", //房间记录id
    roomid : "聊天室id" //用户获取在线人数 --字段已废弃 可不传
  }
  ```
### No.7返回值
```sh
var re = {
    "status": 1,//-1 参数不对,-2请求聊天室令牌失败，1成功,-3直播间已关闭（返回此状态时增加look_num字段，表示历史观看人数）
    "online": 20,//在线人数
    "bill": 0,//映票
    "friend": 1,//0未关注，1已关注
    "blacklist": 1, //1已拉黑，0未拉黑
    "op": 0, //2房主,1管理员,0普通用户
    "role" : -1, // 2 普通等级（忽略） -1 黑名单（忽略） -2 禁言用户  0 无状态
    "share_url": "http://share.live.com/sss/sdfasdf" //直播分享地址
    "is_top": 1,//1已置顶 0未置顶
    "is_rec": 0,//1已推荐 0未推荐
    "is_pass": 1,//1已审核 0未审核
    "hide_bill": 1,//1隐藏夜币 0显示
    "hide_online": 1,//1隐藏在线人数 0显示人数
    "look": { //房间列表，默认20个，其余的在滑动的时候请求 接口NO.12 获得
        "0": {
            "uid": "4904938",//观看者uid
            "nickname": "亦凌香",//观看者昵称
            "level": 3,//等级
            "exp": 18,//经验
            "sex": "0",//性别
            "birth": "0",//生日
            "job": "",//职业
            "marriage": "0",//婚姻状况，0单身，1已婚，2未婚
            "from": "",//来自哪里
            "bill": "0",//映票
            "diamond": "10000",//剩余钻石
            "all_diamond": "0",//总共获得钻石
            "img": "http://cdn.bdwsw.zhanchenggame.com/liveimg/icon/boy6.jpg",//头像地址
            "causerie": "",//签名
            "care_me": 0,//关注我的人
            "my_care": 0,//我关注的人,
            "auth": 0,//1管理员  2普通认证 
        }
    },
    "send_msg": [//要发送的消息 ，列表形式
        {
            "online": 4 //当前在线人数
        }
    ],
}
```
### No.8接口说明
接口已弃用

聊天室发送消息
### No.8请求地址
http://42.62.78.20:9501/?c=chat&m=send_msg
### No.8请求参数
```sh
  var param = {
    token : "LW6A7onv3A11jqg8SxxF3w", //直播服务器token
    roomid : "3622193", //聊天室房间id
    msg : "sadfasdfsadfasdfsadfa", //发送的聊天信息
  }
  ```
### No.8返回值
```sh
  var re = {
    "status": 1,//-1 发送失败，1成功
    "info": {
        "time": "1468556168492",//发送时间
        "fromAvator": "http://cdn.bdwsw.zhanchenggame.com/liveimg/icon/girl8.jpg",//发送者头像
        "msgid_client": "3622193_4904739_3073_121608",//消息id
        "fromClientType": "REST",//
        "attach": "aaaaa",//发送的内容
        "roomId": "3622193",//聊天室房间id
        "fromAccount": "4904739",//发送者uid
        "fromNick": "春海蓝",//发送者昵称
        "type": "0",//消息类型，类型为100的时候，ext字段不为空，这个后续再定义
        "ext": ""//扩展字段，json格式返回，默认为空
    }
}
```
type 字段详解： 0: 表示文本消息， 1: 表示图片， 2: 表示语音， 3: 表示视频， 4: 表示地理位置信息， 6: 表示文件， 10: 表示Tips消息， 100: 自定义消息类型

### No.9接口说明
设置玩家状态
### No.9请求地址
http://42.62.78.20:9501/?c=chat&m=set_member_role
### No.9请求参数
```sh
  var param = {
    token : "LW6A7onv3A11jqg8SxxF3w", //直播服务器token
    roomid : "3622193", //聊天室房间id
    uid : "306", //被处理的uid
    opt : 1, //1 设为管理员，2设置普通等级用户，-1设为黑名单，-2设为禁言用户
    optvalue : 1, //1设置，0取消设置
  }
  ```
### No.9返回值
```sh
  var re = {
    "status": 1,//-1 参数错误，-2无法设置超级管理员权限，-3查询房间信息失败，-4只有房主才能设置管理员，-5只有房主或者管理员 才能设置禁言，1成功
}
```
### No.10接口说明
发送礼物
发送成功过后 映票数量会通过消息的形式推送给客户端，消息类型100 ext字段为json对象{bill:映票数量}
### No.10请求地址
http://42.62.78.20:9501/?c=gift&m=send_gift
### No.10请求参数
```sh
  var param = {
    token : "LW6A7onv3A11jqg8SxxF3w", //直播服务器token
    id : "72", //直播列表id or type等于1的时候  此字段传对方uid
    giftid : "1", //礼物id
    num : "1", //礼物数量
    type : "1",//按uid送礼物
  }
  ```
### No.10返回值
```sh
  var re = {
    "status": 1,//-1 参数错误，-2 房间不存在，-3礼物信息不存在，-4拥有钻石不足，-5扣钻石失败，-6加经验失败，-7操作失败，-8 不能自己给自己发礼物，1成功
    "send_msg": [//要发送的消息
        {
            "bill": 1000//映票数量
        },
        {
            "gift": 210//礼物id
        }
    ],
    "change": {//更新信息
        "member": {//用户信息
            "diamond": 9900,//剩余钻石
            "level": 2,//当前等级
            "exp": 70//当前经验
        }
    }
}
```
### No.11接口说明
关闭直播间
### No.11请求地址
http://42.62.78.20:9501/?c=live&m=out_live
### No.11请求参数
```sh
  var param = {
    token : "LW6A7onv3A11jqg8SxxF3w", //直播服务器token
  }
  ```
### No.11返回值
```sh
  var re = {
    "status": 1,
    "change": { //更新的数据
        "member": { //更新用户数据
            "level": "3", //等级
            "exp": "19",//当前经验值
            "levelup_exp": 150//当前等级总经验
        }
    },
    "send_msg": [//要发送的消息 ，列表形式
        {
            "online": 3 //当前在线人数
        }
    ],
}
```
### No.12接口说明
获取直播间其他观看人列表
### No.12请求地址
http://42.62.78.20:9501/?c=live&m=get_look_member
### No.12请求参数
```sh
  var param = {
    token : "LW6A7onv3A11jqg8SxxF3w", //直播服务器token
    id : 1, //直播列表id
    p : 1, //获取页数
  }
  ```
### No.12返回值
```sh
  var re = {
    "status": 1,
    "list": { //房间列表，每页20个
        "0": {
            "uid": "4904938",//观看者uid
            "nickname": "亦凌香",//观看者昵称
            "level": 3,//等级
            "exp": 18,//经验
            "sex": "0",//性别
            "birth": "0",//生日
            "job": "",//职业
            "marriage": "0",//婚姻状况，0单身，1已婚，2未婚
            "from": "",//来自哪里
            "bill": "0",//映票
            "diamond": "10000",//剩余钻石
            "all_diamond": "0",//总共获得钻石
            "img": "http://cdn.bdwsw.zhanchenggame.com/liveimg/icon/boy6.jpg",//头像地址
            "causerie": "",//签名
            "care_me": 0,//关注我的人
            "my_care": 0//我关注的人
        }
    }
}
```
### No.13接口说明
获取直播间贡献列表排行
### No.13请求地址
http://42.62.78.20:9501/?c=member&m=get_top_list
### No.13请求参数
```sh
  var param = {
    token : "LW6A7onv3A11jqg8SxxF3w", //直播服务器token
    uid : 1, //查看哪个用户的列表
    p : 1, //获取页数
  }
  ```
### No.13返回值
```sh
  var re = {
    "status": 1,
    "list": [ //贡献列表，按排名显示，每页20个，滑动时页数+1 再次获取数据，如果每页不足20个了，就不能在下拉刷新了
        {
            "uid": "4904938",//观看者uid
            "nickname": "亦凌香",//观看者昵称
            "level": 3,//等级
            "exp": 18,//经验
            "sex": "0",//性别
            "birth": "0",//生日
            "job": "",//职业
            "marriage": "0",//婚姻状况，0单身，1已婚，2未婚
            "from": "",//来自哪里
            "bill": "0",//映票
            "diamond": "10000",//剩余钻石
            "all_diamond": "0",//总共获得钻石
            "img": "http://cdn.bdwsw.zhanchenggame.com/liveimg/icon/boy6.jpg",//头像地址
            "causerie": "",//签名
            "send": "5500",//贡献了多少钻石
            "care_me": 0,//关注我的人
            "my_care": 0//我关注的人
        }
    ]
}
```
### No.14接口说明
关注或者取消关注 or 拉黑
### No.14请求地址
http://42.62.78.20:9501/?c=friend&m=care
### No.14请求参数
```sh
  var param = {
    token : "LW6A7onv3A11jqg8SxxF3w", //直播服务器token
    uid : 1, //要关注或者取消关注的人
    t : 1, //1关注 0 取消关注 2拉黑
  }
  ```
### No.14返回值
```sh
  var re = {
    "status": 1,//-1玩家信息错误，-2操作失败，-3已关注/已取关,-4更新缓存失败
    "change": {//要更新的用户信息
        "member": {
            "my_care": 0
        }
    }
}
```
### No.15接口说明
关注的主播 直播列表
### No.15请求地址
http://42.62.78.20:9501/?c=live&m=get_friend_live
### No.15请求参数
```sh
  var param = {
    token : "LW6A7onv3A11jqg8SxxF3w", //直播服务器token
  }
  ```
### No.15返回值
```sh
  var re = {
    "status": 1,//1 成功
    "list": [ //如果没有 则返回空列表
        {
            "id": "1",//记录id，没啥作用，唯一值
            "name": "4904739#xxxxx#860",//房间名称
            "title": "",//房间话题
            "pushurl": "rtmp://pc3fe87c5.live.126.net/live/550aee3d69c342a9999ec54cad3f0d50?wsSecret=3ad3d8adab7cdb03bce65277dc5241ba&wsTime=1468403691",//推流地址
            "httpurl": "http://vc3fe87c5.live.126.net/live/550aee3d69c342a9999ec54cad3f0d50.flv",//http 拉流地址
            "hlsurl": "http://pullhlsc3fe87c5.live.126.net/live/550aee3d69c342a9999ec54cad3f0d50/playlist.m3u8", //hls 拉流地址
            "rtmpurl": "rtmp://vc3fe87c5.live.126.net/live/550aee3d69c342a9999ec54cad3f0d50", //rtmp 拉流地址
            "cid": "550aee3d69c342a9999ec54cad3f0d50", //房间id
            "roomid": "3609065", //聊天室id
            "uid": "4904739", //创建者id
            "filename": "", //忽略该字段
            "status": "0", //直播状态 
            "dateline": "4294967295", //直播创建时间戳
            "nickname": "春海蓝", //创建者名称
            "level": "1", //创建者等级
            "icon": "http://cdn.bdwsw.zhanchenggame.com/liveimg/icon/girl8.jpg", //创建者头像
            "birth": "0", //创建者生日，0代表未设置过
            "job": "", //创建者职业，空代表为设置过
            "sex": "0", //性别，1男，2女，0 未设置过
            "causerie": "", //个性签名
            "marriage": "0", //婚姻状况，0单身，1已婚，2未婚
            "from": "",  //来自哪里
            "online": "10" //当前观看人数 
        }
    ]
}
```
### No.16接口说明
查询玩家的资料信息（资料页）
### No.16请求地址
http://42.62.78.20:9501/?c=member&m=get_player_info
### No.16请求参数
``` sh
  var param = {
    token  : "IeXBqNnikDNpBV-owFyQmA",//固定参数
    uid : "306"//要查看的用户uid
  }
```
### No.16返回值
```sh
    var re = {
        status: 1, //-1参数错误，-2用户信息错误，1 成功
        info: {
            "uid": "4904739",   //用户id
            "nickname": "春海蓝",  //用户昵称
            "level": "1",   //用户等级
            "exp": "0",     //用户经验值
            "sex": "0",     //用户性别
            "birth": "0",   //生日，0表示还没有设置过
            "diamond": "0",     //当前拥有钻石
            "all_diamond": "0",     //累计获得钻石
            "img": "http://cdn.bdwsw.zhanchenggame.com/liveimg/icon/girl8.jpg", //头像地址
            "my_care": 0,   //我关注的人 数量
            "care_me": 0,   //我的粉丝  数量
            "live": 0,   //直播数量
            "friend": 1 //0未关注，1已关注
            "blacklist": 1 //1已拉黑，0未拉黑
            "role" : -1, // 2 普通等级（忽略） -1 黑名单（忽略） -2 禁言用户  0 无状态
            "auth": 0,//1管理员  2普通认证 
        },
        "live": {//如果正在直播返回下列信息，否则为空
            "id": "2773",
            "name": "4904965#在一起#653",
            "title": "",
            "pushurl": "rtmp://pc3fe87c5.live.126.net/live/f8e902cc80944735b8608cadfd80dd2c?wsSecret=b58514b0e35f9172112dbbba45c66e66&wsTime=1471528455",
            "httpurl": "http://vc3fe87c5.live.126.net/live/f8e902cc80944735b8608cadfd80dd2c.flv",
            "hlsurl": "http://pullhlsc3fe87c5.live.126.net/live/f8e902cc80944735b8608cadfd80dd2c/playlist.m3u8",
            "rtmpurl": "rtmp://vc3fe87c5.live.126.net/live/f8e902cc80944735b8608cadfd80dd2c",
            "cid": "f8e902cc80944735b8608cadfd80dd2c",
            "roomid": "3883101",
            "dateline": "1471528449"
        }
    }
```
### No.17接口说明
查看 我关注的人 / 关注我的人
### No.17请求地址
http://42.62.78.20:9501/?c=friend&m=get_friend_list
### No.17请求参数
``` sh
  var param = {
    token  : "IeXBqNnikDNpBV-owFyQmA",//固定参数
    p : 1，//查看的页数，每页20条，如果返回的列表内不足20条  则没有下一页了，反之可页数递增获取列表
    t : 1, //1 查看我关注的人 ， 2查看关注我的人
    uid : "306"// 要查看的用户uid，如果是查看自己的 可以不用传
  }
```
### No.17返回值
```sh
    var re = {
    "status": 1,//-1 列表为空  1获取成功
    "list": [
        {
            "uid": "4904951", //玩家uid
            "fuid": "4904934",//关注人的uid  -- 字段已废弃
            "status": "1",//忽略该字段
            "online": 0,//忽略该字段
            "nickname": "王芷珍",//玩家昵称
            "level": 2,//玩家等级
            "icon_small": "http://cdn.bdwsw.zhanchenggame.com/liveimg/icon/09eff8ec24d9bf4d02acc9c010ae7769788_96.jpg",//小图
            "icon_middle": "http://cdn.bdwsw.zhanchenggame.com/liveimg/icon/09eff8ec24d9bf4d02acc9c010ae7769788_200.jpg",//中图
            "icon": "http://cdn.bdwsw.zhanchenggame.com/liveimg/icon/09eff8ec24d9bf4d02acc9c010ae7769788.jpg"//大图
            "birth": "0",//玩家生日
            "job": "",//玩家职业
            "sex": "0",//玩家性别
            "causerie": "",//玩家签名
            "marriage": "0",//玩家婚姻状况
            "auth": 0,//1管理员  2普通认证 
            "from": "",//玩家居住地
            "friend": 1, //1已关注，0未关注
            "blacklist": 1 //1已拉黑，0未拉黑
        },
        {
            "uid": "4904949",
            "fuid": "4904934",
            "status": "1",
            "online": 0,
            "nickname": "又飞雪",
            "level": 1,
            "icon": "http://cdn.bdwsw.zhanchenggame.com/liveimg/icon/boy12.jpg",
            "birth": "0",
            "job": "",
            "sex": "0",
            "causerie": "",
            "marriage": "0",
            "from": ""
        }
    ]
}
```
### No.18接口说明
修改用户资料
### No.18请求地址
http://42.62.78.20:9501/?c=member&m=update_info
### No.18请求参数
``` sh
  var param = {
    token  : "IeXBqNnikDNpBV-owFyQmA",//固定参数
    type : 1，//修改那一项（1昵称，2性别，3签名，4生日，5居住地，6职业，7婚姻状况）
    ----------下面的参数根据上面的type类型传值-------------
    nickname : "要修改的姓名"， //最长16个字符 or 8个汉字
    sex : 1, //0未知，1男，2女
    causerie : '我的签名' //最长64个字符 or 32个汉字
    birth : '633888000'//生日 时间戳
    from : '1,3' //这个前端用列表表示  后端只存key
    job : '无业游民' //最长16个字符 or 8个汉字
    marriage : '0' //0保密，1单身，2恋爱，3已婚，4同性
  }
```
### No.18返回值
```sh
    var re = {
    "status": 1,// 1修改成功，-1参数错误，-2参数太短，-3参数太长，-4修改失败
}
```
### No.19接口说明
* 修改头像
* 注意url和其他的不一样
### No.19请求地址
http://update.bdwsw.zhanchenggame.com/index.php?c=liveimg&m=upload
### No.19请求参数
``` sh
  var param = {
    token  : "IeXBqNnikDNpBV-owFyQmA",//固定参数
    userfile : "" //图片data
  }
```
### No.18返回值
```sh
    var re = {
    "status": 1,//1修改成功，-1 参数错误，-2 修改失败
    "icon_small": "http://cdn.bdwsw.zhanchenggame.com/liveimg/icon/09eff8ec24d9bf4d02acc9c010ae7769788_96.jpg",//小图
    "icon_middle": "http://cdn.bdwsw.zhanchenggame.com/liveimg/icon/09eff8ec24d9bf4d02acc9c010ae7769788_200.jpg",//中图
    "icon": "http://cdn.bdwsw.zhanchenggame.com/liveimg/icon/09eff8ec24d9bf4d02acc9c010ae7769788.jpg"//大图
}
```
### No.20接口说明
* 搜索功能
### No.20请求地址
http://42.62.78.20:9501/?c=member&m=search
### No.19请求参数
``` sh
  var param = {
    token  : "IeXBqNnikDNpBV-owFyQmA",//固定参数
    str : "" //搜索的内容
    p : 1//搜索的页数 默认20条，当前搜索页不足20条时，则没有下一页
  }
```
### No.18返回值
```sh
    var re = {
    "status": 1,//只会返回成功状态，但是数据为空时，list返回空列表
    "list": [//用户信息，和其他接口一致
        {
            "uid": "306",
            "friend": 0,
            "blacklist": 1 //1已拉黑，0未拉黑
            "icon_small": "http://cdn.bdwsw.zhanchenggame.com/liveimg/icon/c9ee4086869aff8e5d6d23a9b0e0d080360_96.jpg",
            "icon_middle": "http://cdn.bdwsw.zhanchenggame.com/liveimg/icon/c9ee4086869aff8e5d6d23a9b0e0d080360_200.jpg",
            "icon": "http://cdn.bdwsw.zhanchenggame.com/liveimg/icon/c9ee4086869aff8e5d6d23a9b0e0d080360.jpg",
            "online": 0,
            "nickname": "醉忆霜",
            "level": "7",
            "birth": "0",
            "job": "",
            "sex": "0",
            "causerie": "",
            "marriage": "0",
            "from": ""
        }
    ]
}
```
### No.21接口说明
查看 我关注的人 / 关注我的人
### No.21请求地址
http://42.62.78.20:9501/?c=friend&m=get_blacklist
### No.21请求参数
``` sh
  var param = {
    token  : "IeXBqNnikDNpBV-owFyQmA",//固定参数
    p : 1，//查看的页数，每页20条，如果返回的列表内不足20条  则没有下一页了，反之可页数递增获取列表
  }
```
### No.21返回值
```sh
    var re = {
    "status": 1,//  1获取成功
    "list": [
        {
            "uid": "4904951", //玩家uid
            "fuid": "4904934",//关注人的uid  -- 字段已废弃
            "status": "1",//忽略该字段
            "online": 0,//忽略该字段
            "nickname": "王芷珍",//玩家昵称
            "level": 2,//玩家等级
            "icon_small": "http://cdn.bdwsw.zhanchenggame.com/liveimg/icon/09eff8ec24d9bf4d02acc9c010ae7769788_96.jpg",//小图
            "icon_middle": "http://cdn.bdwsw.zhanchenggame.com/liveimg/icon/09eff8ec24d9bf4d02acc9c010ae7769788_200.jpg",//中图
            "icon": "http://cdn.bdwsw.zhanchenggame.com/liveimg/icon/09eff8ec24d9bf4d02acc9c010ae7769788.jpg"//大图
            "birth": "0",//玩家生日
            "job": "",//玩家职业
            "sex": "0",//玩家性别
            "causerie": "",//玩家签名
            "marriage": "0",//玩家婚姻状况
            "from": "",//玩家居住地
            "friend": 1 //1已关注，0未关注
        }
    ]
}
```
### No.22接口说明
获得我关注的所有人uid
### No.22请求地址
http://42.62.78.20:9501/?c=friend&m=get_my_friend_list
### No.22请求参数
``` sh
  var param = {
    token  : "IeXBqNnikDNpBV-owFyQmA",//固定参数
  }
```
### No.22返回值
```sh
    var re = {
    "status": 1,//  1获取成功
    "list": ["4904953","306"]//uid列表
}
```
### No.23接口说明
* 创建支付订单
* 注意 此接口地址与其他地址不同
* 充值回调地址 http://42.62.78.20/pay.live.com/index.php?c=payzc&m=confirm_zcpay
### No.23请求地址
http://42.62.78.20/pay.live.com/index.php?c=payzc&m=create_prepay_order
### No.23请求参数
``` sh
  var param = {
    token  : "IeXBqNnikDNpBV-owFyQmA",//固定参数
    id : 1, //订单列表id
    type : 0 //默认为0
    appid : 28 //sdk appid
  }
```
### No.23返回值
```sh
    var re = {
    "status": 1, //-1 获取订单信息失败，-2创建订单失败，-3同步订单失败，1创建成功
    "amount": "6", //订单金额
    "product_name": "60 Ingots",//订单名称
    "product_id": 1, //订单列表id
    "app_order_id": "2661160806143643362306" //用于支付的订单id
}
```
### No.24接口说明
* 检查订单状态
### No.24请求地址
http://42.62.78.20:9501/?c=recharge&m=get_order_status
### No.24请求参数
``` sh
  var param = {
    token  : "IeXBqNnikDNpBV-owFyQmA",//固定参数
    order_id : 2661160806143643362306, //用于支付的订单id
  }
```
### No.24返回值
```sh
    var re = {
    "status": 1,//-1 参数缺失，-2订单信息不存在，-3订单未完成，-4充值信息异常，log没有添加成功，1充值成功
    "amount": "6", //充值支付金额
    "diamond": "60", //获得钻石
    "first": "0", //是否首次充值
    "change": { //充值后更新用户本地信息
        "member": {
            "diamond": 10004480,
            "all_diamond": 60031620
        }
    }
}
```
### No.25接口说明
* 举报
### No.25请求地址
http://42.62.78.20:9501/?c=member&m=report
### No.25请求参数
``` sh
  var param = {
    token  : "IeXBqNnikDNpBV-owFyQmA",//固定参数
    uid : 306, //要举报的uid
    str : '涉黄'// 默认为空，举报的原因
  }
```
### No.25返回值
```sh
    var re = {
    "status": 1,//-1 参数缺失，1举报成功
}
```
### No.26接口说明
* 发送喇叭
### No.26请求地址
http://42.62.78.20:9501/?c=chat&m=send_horn
### No.26请求参数
``` sh
  var param = {
    token  : "IeXBqNnikDNpBV-owFyQmA",//固定参数
  }
```
### No.26返回值
```sh
    var re = {
    "status": 1,//-1钻石不足，-2扣除失败，-3扣除失败，1成功
    "change": {//要更新的用户信息
        "member": {
            "diamond": 10009242,
            "all_diamond": 10009976
        }
    }
}
```
### No.27接口说明
* 手机注册 初始化用户信息
### No.27请求地址
http://42.62.78.20:9501/?c=member&m=reg_info
### No.27请求参数
``` sh
  var param = {
    token  : "IeXBqNnikDNpBV-owFyQmA",//固定参数
    nickname : "要修改的姓名"， //最长16个字符 or 8个汉字
    sex : 1, //0未知，1男，2女
    causerie : '我的签名' //最长64个字符 or 32个汉字
    birth : '633888000'//生日 时间戳
    from : '1,3' //这个前端用列表表示  后端只存key
    job : '无业游民' //最长16个字符 or 8个汉字
    marriage : '0' //0保密，1单身，2恋爱，3已婚，4同性
  }
```
### No.27返回值
```sh
    var re = {
    "status": 1, //-1 已经设置过用户信息，-2昵称太短，-3昵称太长，-4签名太长，-5工作信息太长，-6修改信息为空，-7 修改失败， 1成功
    "member": { //用户所有信息
        "uid": "4904964",
        "nickname": "哈哈",
        "level": "1",
        "exp": "0",
        "sex": "1",
        "birth": "12345",
        "job": "324",
        "marriage": "0",
        "from": "",
        "bill": "0",
        "diamond": "0",
        "all_diamond": "0",
        "icon": "",
        "causerie": "",
        "status": "0",
        "statustime": "0",
        "look": "",
        "channel": "0",
        "regchannel": "0",
        "jointime": "1470735890",
        "dateline": 1471959945,
        "my_care": 0,
        "care_me": 0
    }
}
```
### No.28接口说明
* 初始化直播房间，在start_live 接口前请求
### No.28请求地址
http://42.62.78.20:9501/?c=live&m=create_live
### No.28请求参数
``` sh
  var param = {
    token  : "IeXBqNnikDNpBV-owFyQmA",//固定参数
  }
```
### No.28返回值
```sh
    var re = {
        "status": 1//1初始化成功，-1等级不够无法直播，-2已被禁止直播，-3初始化失败，-4创建房间失败
    }
```

### No.29接口说明
* 启动应用时请求的第一个接口，获得启动界面大图、登录提示，和当前版本审核状态
### No.29请求地址
http://42.62.78.20:9501/?c=liveinit&m=check_version
### No.29请求参数
``` sh
  var param = {
    ver  : 20160101,//当前客户端版本号
  }
```
### No.29返回值
```sh
    var re = {
    "status": 1,//只有这1个状态
    "isreview": 0,//0待审核 审核中，1审核通过
    "new_client": {//客户端更新信息 
        "status": 1,//1有更新 ，0没有更新 
        "ver": "2",//客户端版本号 
        "desc": "1、优化体验",//更新详解 
        "update": "1",//1强制更新 ，0非强制 
        "url": "http://xxxx.pkg"//更新地址 
    }
    "openimg": [ //启动页面大图，这是个数组，有可能有多个
        "http://img.hb.aicdn.com/a922530c1cba9c92c4eff6ea4be7936e55430d4e2e618-egPFIs_fw658"
    ],
    "login_msg": [ //进入页面之后弹的提示
        "这个是登录过后弹出的alter，这是后端写的内容"
    ]
}
```
### No.30接口说明
* 管理员账号所属菜单  置顶、取消置顶
### No.30请求地址
http://42.62.78.20:9501/?c=alive&m=live_top&d=admin
### No.30请求参数
``` sh
  var param = {
    "token"  : 'qCxHTPsD1k_XJxo43eQenQ',//用户token
    "id" : '4321', //要置顶的直播id
    "t" : 1, //1置顶 0取消置顶
  }
```
### No.30返回值
```sh
    var re = {
    "status": 1,//1 操作成功，0已被置顶或取消置顶，-10没有权限
}
```
### No.31接口说明
* 管理员账号所属菜单  关闭直播
### No.31请求地址
http://42.62.78.20:9501/?c=alive&m=live_close&d=admin
### No.31请求参数
``` sh
  var param = {
    "token"  : 'qCxHTPsD1k_XJxo43eQenQ',//用户token
    "id" : '4321', //要关闭的直播id
  }
```
### No.31返回值
```sh
    var re = {
    "status": 1,//1 操作成功，0已被关闭，-10没有权限
}
```
### No.32接口说明
* 管理员账号所属菜单  禁播、隐藏、通过审核、封号
### No.32请求地址
http://42.62.78.20:9501/?c=user&m=update_player_info&d=admin
### No.32请求参数
``` sh
  var param = {
    "token"  : 'qCxHTPsD1k_XJxo43eQenQ',//用户token
    "uid" : '4321234', //要处理的uid
    "status": 4,//0解除， 2禁言，4封号，5隐藏，6禁播，7通过审核，8特殊隐藏分组
  }
```
### No.32返回值
```sh
    var re = {
    "status": 1,//1 操作成功，0已是当前状态，-10没有权限
}
```
### No.33接口说明
* 夜票兑换钻石
### No.33请求地址
http://42.62.78.20:9501/?c=recharge&m=change_diamond
### No.33请求参数
``` sh
  var param = {
    "token"  : 'qCxHTPsD1k_XJxo43eQenQ',//用户token
    "id" : '1', //兑换钻石的订单id  在No.3 接口返回兑换比例的订单列表
  }
```
### No.33返回值
```sh
    var re = {
    "status": 1,//1兑换成功，-1订单信息错误，-2夜票不足，-3兑换失败，-4扣除夜票失败，-5添加钻石失败，-6兑换失败
    "change": {//兑换后更新的member信息
        "member": {
            "bill": 5460,
            "diamond": 10009795,
            "all_diamond": 70067199
        }
    }
}
```
### No.34接口说明
* 微信红包/兑换钻石  兑换记录
### No.34请求地址
http://42.62.78.20:9501/?c=recharge&m=get_change_log
### No.34请求参数
``` sh
  var param = {
    "token"  : 'qCxHTPsD1k_XJxo43eQenQ',//用户token
    "p" : 1//当前页数， 每页展示20条，如果当前页数不足20条，则没有下一页
  }
```
### No.34返回值
```sh
    var re = {
    "status": 1,//获取成功
    "change_amount": 0, //累计兑换金额
    "list": [//兑换列表
        {
            "id": "9", //兑换记录id
            "uid": "4904739",//兑换者uid
            "type": "1",//兑换类型（1兑换钻石，2微信红包）
            "order_id": "3",//兑换的订单id
            "order_bill": "2020",//兑换消耗夜票
            "get": "1000",//兑换获得（type为1是钻石，type为2是人民币）
            "own_bill": "1930",//兑换后剩余夜票
            "own_diamond": "10011495",//兑换后剩余钻石
            "dateline": "2016.09.11 19:14:43"//兑换时间
        },
        {
            "id": "8",
            "uid": "4904739",
            "type": "1",
            "order_id": "2",
            "order_bill": "1030",
            "get": "500",
            "own_bill": "3950",
            "own_diamond": "10010495",
            "dateline": "2016.09.11 19:14:40"
        }
    ]
}
```
### No.35接口说明
* 获取支付的订单列表
* 注意请求的服务器不同
### No.35请求地址
http://42.62.78.20/pay.live.com/index.php?c=ios&m=get_product_list
### No.35请求参数
``` sh
  var param = {
    "version"  : '123',//客户端唯一版本号
  }
```
### No.35返回值
```sh
    var re = {
    "status": 1,//只有1一个状态
    "list": [//订单列表
        {
            "orderid": "1",//订单列表id
            "buyid": "com.zhanle.showtime.app.buy1",//ios 订单id
            "name": "60钻石",//订单名称
            "channel": "1",//忽略
            "amount": "6",//订单要求金额
            "gold": "60"//充值可获得钻石
        },
        {
            "orderid": "2",
            "buyid": "com.zhanle.showtime.app.buy2",
            "name": "300钻石",
            "channel": "1",
            "amount": "30",
            "gold": "300"
        },
        {
            "orderid": "3",
            "buyid": "com.zhanle.showtime.app.buy3",
            "name": "980钻石",
            "channel": "1",
            "amount": "68",
            "gold": "680"
        },
        {
            "orderid": "4",
            "buyid": "com.zhanle.showtime.app.buy4",
            "name": "1980钻石",
            "channel": "1",
            "amount": "198",
            "gold": "1980"
        },
        {
            "orderid": "5",
            "buyid": "com.zhanle.showtime.app.buy5",
            "name": "3280钻石",
            "channel": "1",
            "amount": "328",
            "gold": "3280"
        },
        {
            "orderid": "6",
            "buyid": "com.zhanle.showtime.app.buy6",
            "name": "6480钻石",
            "channel": "1",
            "amount": "648",
            "gold": "6480"
        }
    ]
}
```
### No.36接口说明
* 管理员账号所属菜单  推荐、取消推荐
### No.36请求地址
http://42.62.78.20:9501/?c=alive&m=live_recommended&d=admin
### No.36请求参数
``` sh
  var param = {
    "token"  : 'qCxHTPsD1k_XJxo43eQenQ',//用户token
    "id" : '4321', //要推荐的直播id
    "t" : 1, //1推荐 0取消推荐
  }
```
### No.36返回值
```sh
    var re = {
    "status": 1,//1 操作成功，0已被推荐或取消推荐，-10没有权限
}
```
### No.37接口说明
* 管理员发送警告 
### No.37请求地址
http://42.62.78.20:9501/?c=user&m=send_msg_to_user&d=admin
### No.37请求参数
``` sh
  var param = {
    "token"  : 'qCxHTPsD1k_XJxo43eQenQ',//用户token
    "uid" : '4321', //要警告的uid
  }
```
### No.37返回值
```sh
    var re = {
    "status": 1,//1 操作成功，-1发送失败 ，-10没有权限
}
```
### No.38接口说明
* 歌曲搜索
### No.38请求地址
http://42.62.78.20:9501/?c=music&m=search
### No.38请求参数
``` sh
  var param = {
    "token"  : 'qCxHTPsD1k_XJxo43eQenQ',//用户token
    "str" : 'a', //要搜索的歌名，拼音 首字母都行 
    "p" :   1,//页数
  }
```
### No.38返回值
```sh
    var re = {
    "status": 1,//-1 参数错误  1搜索成功
    "music": [//歌曲列表
        {
            "id": 26017,//歌曲id
            "lyrics": "http://lyric.banshenggua.cn/lrcx/17/217/26017.lrcx",//歌词文件
            "filepath": "http://banzoucdn.banshenggua.cn/banzou/v2/2011/07/15/1914588716.mkv.3.mp3",//歌曲文件
            "name": "爱就爱",//歌曲名称
            "singer": "张靓颖",//演唱者
            "size": 2570304,//文件大小
            "type": "mp3"//文件类型
        }
    ],
    "pn": 10,//当前页数
    "rn": 20,//每页显示条数
    "hit": 2001//总条数
}
```
### No.39 接口说明
* 实名认证
### No.39请求地址
http://42.62.78.20:9501/?c=member&m=certification
### No.39请求参数
```sh
  var param{
	"uid" :	4904656,//用户uid
    "token" :	"wi6tLQ4CEMC_va-4vZ02iw",//用户token
    "name"	:	"哇haha",//真实姓名,长度不大于20
    "phone"	:	15662137729,//用户手机
    "id_number"	:	"460003199909010050",//身份证号码
    "img"	:	"http://cdn.bdwsw.zhanchenggame.com/liveimg/icon/girl11.jpg",//手持身份证照片地址
    "auth_name"	:	0,	//认证名称
  }
```
### No.39 返回值
```
var re={
	"status": 1,// 1成功,-1参数错误,-2认证过了,-3名字为空,-4名字过长,-5手机号不正确,-6身份证号吗不对,-7手持身份证证件照不能为空,-8插入失败,-9更新失败
}
```
