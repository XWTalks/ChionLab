---
title: ThunderHack开发笔记
date: 2016-01-20 20:32:38
tags:
- python
- crawler
- hack
categories:
- 开发笔记
- Python
---

关于ThunderHack
--------------
ThunderHack是一个运行于服务端的Python脚本，模拟用户操作迅雷离线平台，同时运行web server，提供网页端UI和API。该工具由维护者提供大容量离线空间，用户只需直接提交源下载地址（magnet/ed2k/http/https/ftp等），可获取迅雷离线服务器的下载地址，实现免费高速下载。
https://github.com/Chion82/ThunderHack.git

CLI版（推荐）
----------
依赖：Python2.7/3.X, aria2
```
$ sudo pip install xunleishare
$ xunleishare [-o OUTPUT_SHELL] DOWNLOAD_LINK
$ ./xunlei_output
```
https://github.com/Chion82/xunlei_share.git

网页端
-----
http://syslab.ddns.us/thunder_hack

开发笔记
-------
为什么会突然想造这个轮子？一次登陆迅雷离线网页端顺便F12抓的包，发现网页版的API参数无加密且不复杂，可通过程序模拟请求。

### 抓包

从迅雷离线网页端抓取的几个关键API如下：

1. BT、magnet查询API
    ```
    GET: http://dynamic.cloud.vip.xunlei.com/interface/url_query?callback=queryUrl&u=magnet%3A%3Fxt%3Durn%3Abtih%3AMPGNCXWJJZ4XMDQRQB2D6XE4I3U7D74K%26dn%3D%255BMGRT%2526Mabors%2BSub%255DGekijouban%2BAno%2BHi%2BMita%2BHana%2Bno%2BNamae%2Bo%2BBokutachi%2Bwa%2BMada%2BShiranai.%2528BDrip%2B1920x1080%2Bx264%2BFLAC%2BAAC%2BGB%2526BIG5%2526JP%2529%26tr%3Dhttp%253A%252F%252F208.67.16.113%253A8000%252Fannounce%26tr%3Dudp%253A%252F%252F208.67.16.113%253A8000%252Fannounce%26tr%3Dhttp%253A%252F%252Ftracker.openbittorrent.com%253A80%252Fannounce%26tr%3Dhttp%253A%252F%252Ftracker.publicbt.com%253A80%252Fannounce%26tr%3Dhttp%253A%252F%252Ftracker.prq.to%252Fannounce%26tr%3Dhttp%253A%252F%252Ftracker.ktxp.com%253A7070%252Fannounce%26tr%3Dhttp%253A%252F%252Ftracker.ktxp.com%253A6868%252Fannounce%26tr%3Dhttp%253A%252F%252Fopen.nyaatorrents.info%253A6544%252Fannounce%26tr%3Dhttp%253A%252F%252Fbtfile.sdo.com%253A6961%252Fannounce%26tr%3Dhttp%253A%252F%252F121.14.98.151%253A9090%252Fannounce&interfrom=task&random=14394337045561054423.7983869016&tcache=1439433717265
    =>
    queryUrl(1,'MPGNCXWJJZ4XMDQRQB2D6XE4I3U7D74K','6229928848','[MGRT&Mabors Sub]Gekijouban Ano Hi Mita Hana no Namae o Bokutachi wa Mada Shiranai.(BDrip 1920x1080 x264 FLAC AAC GB&BIG5&JP)','0',new Array('Scans\\/Scan_00.png','Scans\\/Scan_01.png','Scans\\/Scan_02.png','Scans\\/Scan_03.png','Scans\\/Scan_04.png','Scans\\/Scan_05.png','Scans\\/Scan_06.png','Scans\\/Scan_07.png','Scans\\/Scan_08.png','Scans\\/Scan_09.png','Scans\\/Scan_10.png','Scans\\/Scan_11.png','Scans\\/Scan_12.png','Scans\\/Scan_13.png','Scans\\/Scan_14.png','Scans\\/Scan_15.png','Scans\\/Scan_16.png','[MGRT&amp;Mabors Sub]Gekijouban Ano Hi Mita Hana no Namae o Bokutachi wa Mada Shiranai. - Audio 5.1.flac','[MGRT&amp;Mabors Sub]Gekijouban Ano Hi Mita Hana no Namae o Bokutachi wa Mada Shiranai. - Menu 01(BDrip 1920x1080 x264 AAC).mkv','[MGRT&amp;Mabors Sub]Gekijouban Ano Hi Mita Hana no Namae o Bokutachi wa Mada Shiranai. - Menu 02(BDrip 1920x1080 x264 AAC).mkv','[MGRT&amp;Mabors Sub]Gekijouban Ano Hi Mita Hana no Namae o Bokutachi wa Mada Shiranai. - NCOP(BDrip 1920x1080 x264 AAC).mkv','[MGRT&amp;Mabors Sub]Gekijouban Ano Hi Mita Hana no Namae o Bokutachi wa Mada Shiranai. - SP 01(BDrip 1920x1080 x264 AAC).mkv','[MGRT&amp;Mabors Sub]Gekijouban Ano Hi Mita Hana no Namae o Bokutachi wa Mada Shiranai. - SP 02(BDrip 1920x1080 x264 AAC).mkv','[MGRT&amp;Mabors Sub]Gekijouban Ano Hi Mita Hana no Namae o Bokutachi wa Mada Shiranai.(BDrip 1920x1080 x264 FLAC AAC).big5.ass','[MGRT&amp;Mabors Sub]Gekijouban Ano Hi Mita Hana no Namae o Bokutachi wa Mada Shiranai.(BDrip 1920x1080 x264 FLAC AAC).gb.ass','[MGRT&amp;Mabors Sub]Gekijouban Ano Hi Mita Hana no Namae o Bokutachi wa Mada Shiranai.(BDrip 1920x1080 x264 FLAC AAC).mkv','[MGRT][13.08.21] 劇場版「あの日見た花の名前を僕達はまだ知らない。」主題歌 - サークルゲーム【期間生産限定アニメ盤】 歌：Galileo Galilei (ALAC+cue+log+png+DVDISO).rar','[MGRT][13.08.28] 劇場版「あの日見た花の名前を僕達はまだ知らない。」オリジナル・サウンドトラック  音楽：REMEDIOS (2ALAC+cue+log+png).rar','COMPILATION MUSIC.rar','Fonts.rar'),new Array('3.90M','4.32M','4.38M','3.63M','4.29M','2.82M','1.94M','3.70M','1.98M','3.17M','3.87M','3.99M','2.59M','2.41M','2.56M','1.63M','22.7M','2.20G','9.03M','2.68M','20.9M','139M','35.4M','142K','142K','2.30G','311M','416M','379M','17.7M'),new Array('4090243','4536547','4598479','3810200','4500046','2964511','2039023','3882558','2077791','3329777','4067333','4189214','2723186','2529606','2693052','1711549','23876871','2363024548','9474807','2818296','22001218','146039473','37178662','146284','146266','2469742194','326404175','436642897','397730513','18579515'),new Array('0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','1','1','1','1','1','1','1','1','1','1','1','1','1'),new Array('RAR','RAR','RAR','RAR','RAR','RAR','RAR','RAR','RAR','RAR','RAR','RAR','RAR','RAR','RAR','RAR','RAR','RAR','RMVB','RMVB','RMVB','RMVB','RMVB','RAR','RAR','RMVB','RAR','RAR','RAR','RAR'),new Array('0','1','2','3','4','5','6','7','8','9','10','11','12','13','14','15','16','17','18','19','20','21','22','23','24','25','26','27','28','29'),new Array('0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0'),'14394337045561054423.7983869016','0')
    ```
2. 提交BT、magnet任务到离线空间
    ```
    POST: http://dynamic.cloud.vip.xunlei.com/interface/bt_task_commit?callback=jsonp1439433353636&t=Thu%20Aug%2013%202015%2010:42:05%20GMT+0800%20(HKT)
    Post field:
    uid:208115253
    btname:[MGRT&Mabors Sub]Gekijouban Ano Hi Mita Hana no Namae o Bokutachi wa Mada Shiranai.(BDrip 1920x1080 x264 FLAC AAC GB&BIG5&JP)
    cid:MPGNCXWJJZ4XMDQRQB2D6XE4I3U7D74K
    goldbean:0
    silverbean:0
    tsize:6229928848
    findex:17_18_19_20_21_22_23_24_25_26_27_28_29_
    size:2363024548_9474807_2818296_22001218_146039473_37178662_146284_146266_2469742194_326404175_436642897_397730513_18579515_
    o_taskid:0
    o_page:task
    class_id:0
    interfrom:task
    verify_code:
    =>
    jsonp1439433353636({"id":"1033432253338880","avail_space":"2249634561012829","time":0.42569899559021,"progress":1})
    ```
3. 提交普通任务(http/ed2k等)
    ```
    GET :http://dynamic.cloud.vip.xunlei.com/interface/task_commit?callback=ret_task&uid=208115253&cid=6269CC18A9B66728C1E5A67ACA7523A424513D76&gcid=B06FE736DADEDBA4364348C779E9CC7DC859F17D&size=1073741824&goldbean=0&silverbean=0&t=Wu.Xia.Setup.V1.1.3.2.Full.Client.part02.rar&url=http%3A%2F%2Fgdl.lixian.vip.xunlei.com%2FWu.Xia.Setup.V1.1.3.2.Full.Client.part02.rar%3Ffid%3DYmnMGKm2ZyjB5aZ6ynUjpCRRPXYAAABAAAAAALBv5zba3tukNkNIx3npzH3IWfF9%26mid%3D666%26threshold%3D150%26tid%3D0AAE0146A6116A4BE5B84EE4F9A3D51B%26srcid%3D6%26verno%3D1%26g%3DB06FE736DADEDBA4364348C779E9CC7DC859F17D%26ui%3D208115253%26s%3D1073741824%26pk%3Dkuaichuan%26ak%3D8%3A0%3A999%3A0%26e%3D1439520556%26ms%3D51200%26ci%3D%26ck%3DE3DAEF800B933F3167A0197646D3B4A0%26at%3D1A3E485B19E9EB13EB8C3F11DD7B599F%26hy%3D1%26n%3D0C32B5BC89281E2A5300B4E7A16F29352E54EEA1FF7351427509ACBC922D16616E11EEE2B0330B34324BB2F3A3417F0400&type=0&o_page=history&o_taskid=0&class_id=0&database=undefined&interfrom=task&verify_code=&time=Thu%20Aug%2013%202015%2010:52:54%20GMT+0800%20(HKT)&noCacheIE=1439434374477
    =>
    ret_task(1,'1033437575386368','0.2315080165863')
    ```
4. 翻页查询任务
    ```
    GET: http://dynamic.cloud.vip.xunlei.com/interface/showtask_unfresh?callback=jsonp1439433877220&t=Thu%20Aug%2013%202015%2010:44:37%20GMT+0800%20(HKT)&type_id=4&page=1&tasknum=30&p=1&interfrom=task
    =>
    queryCid('6269CC18A9B66728C1E5A67ACA7523A424513D76', 'B06FE736DADEDBA4364348C779E9CC7DC859F17D', '1073741824','2249633487271005', 'Wu.Xia.Setup.V1.1.3.2.Full.Client.part02.rar', '0','0', 0,'1439434366089947731.337838443','rar','0')
    ```
5. 列出BT、magnet任务的文件列表
    ```
    GET: http://dynamic.cloud.vip.xunlei.com/interface/fill_bt_list?callback=fill_bt_list&tid=1033432253338880&infoid=63CCD15EC94E79760E1180743F5C9C46E9F1FF8A&g_net=1&p=1&uid=208115253&interfrom=task&noCacheIE=1439433922810
    =>
    fill_bt_list({"Result":{"Tid":"1033432253338880","Infoid":"63CCD15EC94E79760E1180743F5C9C46E9F1FF8A","btnum":"13","btpernum":30,"now_page":1,"Record":[{"id":0,"title":"[MGRT&amp;Mabors Sub]Gekijouban Ano Hi Mita Hana no Namae o Bokutachi wa Mada Shiranai. - Audio 5.1.flac","download_status":"2","cid":"99C94A8BDC84DCEAFD7356F232557F27FAF81E43","size":"2.20G","percent":100,"taskid":"1033432253404480","icon":"RAR","livetime":"365\u5929","downurl":"http:\/\/gdl.lixian.vip.xunlei.com\/download?fid=mclKi9yE3Or9c1byMlV\/J\/r4HkOk5NiMAAAAAM0QeeQUdvGKWARXXwfMCh0lt934&mid=666&threshold=150&tid=493E66D0ECA261B41AA97E8ECE0561AA&srcid=4&verno=1&g=CD1079E41476F18A5804575F07CC0A1D25B7DDF8&scn=t18&i=63CCD15EC94E79760E1180743F5C9C46E9F1FF8A&t=6&ui=208115253&ti=1033432253404480&s=2363024548&m=0&n=013A7CA30D54264D61035E962C205375623C768134696A6F7503508A7F416E6F202958C4126974612029508A3E206E6F202F50893E65206F20235E8F2A746163680811933E204D61640011B7376972616E0058CA7F2D20417505588B7F352E312E075D853C00000000&ih=63CCD15EC94E79760E1180743F5C9C46E9F1FF8A&fi=17&pi=1033432253338880&ff=0&co=B07951D481443FF3B125AF546C501605&cm=1&pk=lixian&ak=1:0:6:4&e=1447209695&ms=10485760&ck=601902F2D83C9A97ED4E6FAC83FA4010&at=F2191321811F9920CE166300279ABA1B","vod":"0","cdn":[],"format_img":"other","filesize":"2363024548","verify":"ca609506d44b624fdd0c117da1f843d0","url":"bt:\/\/63CCD15EC94E79760E1180743F5C9C46E9F1FF8A\/17","openformat":"","ext":"flac","dirtitle":"[MGRT&amp;Mabors Sub]Gekijouban Ano Hi Mita Hana no Namae o Bokutachi wa Mada Shiranai. - Audio 5.1.flac","is_blocked":0},{"id":1,"title":"[MGRT&amp;Mabors Sub]Gekijouban Ano Hi Mita Hana no Namae o Bokutachi wa Mada Shiranai. - Menu 01(BDrip 1920x1080 x264 AAC).mkv","download_status":"2","cid":"8B88BA5C635F9E8606A1059AC594A5B62FC5898D","size":"9.03M","percent":100,"taskid":"1033432253470016","icon":"RMVB","livetime":"365\u5929","downurl":"http:\/\/gdl.lixian.vip.xunlei.com\/download?fid=i4i6XGNfnoYGoQWaxZSlti\/FiY33kpAAAAAAALqMOWwoV6AQ5zopMFal0RP5VQpE&mid=666&threshold=150&tid=B917B3233B55EAA7E6293CEB10D4DA53&srcid=4&verno=1&g=BA8C396C2857A010E73A293056A5D113F9550A44&scn=u2&i=63CCD15EC94E79760E1180743F5C9C46E9F1FF8A&t=6&ui=208115253&ti=1033432253470016&s=9474807&m=0&n=013A7CA30D54264D61035E962C205375623C768134696A6F7503508A7F416E6F202958C4126974612029508A3E206E6F202F50893E65206F20235E8F2A746163680811933E204D61640011B7376972616E0058CA7F2D204D650F44C46F312842441358947F313932301900D467302078325705C41E4143292E0C5A925F00000000&ih=63CCD15EC94E79760E1180743F5C9C46E9F1FF8A&fi=18&pi=1033432253338880&ff=0&co=8046D92536CB73B6ACA5CDD2E6647C1C&cm=1&pk=lixian&ak=1:0:6:4&e=1447209695&ms=10485760&ck=601902F2D83C9A97ED4E6FAC83FA4010&at=63500B46C3C5A833E454AFE1824E19C4","vod":"1","cdn":[],"format_img":"video","filesize":"9474807","verify":"f87c5821789304dce494c4430ae5717f","url":"bt:\/\/63CCD15EC94E79760E1180743F5C9C46E9F1FF8A\/18","openformat":"movie","ext":"mkv","dirtitle":"[MGRT&amp;Mabors Sub]Gekijouban Ano Hi Mita Hana no Namae o Bokutachi wa Mada Shiranai. - Menu 01(BDrip 1920x1080 x264 AAC).mkv","is_blocked":0},{"id":2,"title":"[MGRT&amp;Mabors Sub]Gekijouban Ano Hi Mita Hana no Namae o Bokutachi wa Mada Shiranai. - Menu 02(BDrip 1920x1080 x264 AAC).mkv","download_status":"2","cid":"1B9D7D66B87C8B809EE1F7F6844C5E237C1FF5DF","size":"2.68M","percent":100,"taskid":"1033432253535552","icon":"RMVB","livetime":"365\u5929","downurl":"http:\/\/gdl.lixian.vip.xunlei.com\/download?fid=G519Zrh8i4Ce4ff2hExeI3wf9d\/4ACsAAAAAAODuVHjC9g9eAQ0WVSutnyDl\/wZ\/&mid=666&threshold=150&tid=8449710F1FB9C13B236A932BD0C55BFC&srcid=4&verno=1&g=E0EE5478C2F60F5E010D16552BAD9F20E5FF067F&scn=u1&i=63CCD15EC94E79760E1180743F5C9C46E9F1FF8A&t=6&ui=208115253&ti=1033432253535552&s=2818296&m=0&n=013A7CA30D54264D61035E962C205375623C768134696A6F7503508A7F416E6F202958C4126974612029508A3E206E6F202F50893E65206F20235E8F2A746163680811933E204D61640011B7376972616E0058CA7F2D204D650F44C46F322842441358947F313932301900D467302078325705C41E4143292E0C5A925F00000000&ih=63CCD15EC94E79760E1180743F5C9C46E9F1FF8A&fi=19&pi=1033432253338880&ff=0&co=0B4508291E892F59357B13FFECFC36CA&cm=1&pk=lixian&ak=1:0:6:4&e=1447209695&ms=10485760&ck=601902F2D83C9A97ED4E6FAC83FA4010&at=614C54E08D0B8BF4975CDAD173051D7F","vod":"1","cdn":[],"format_img":"video","filesize":"2818296","verify":"f6a67416abd94e12789c8975abb3af3c","url":"bt:\/\/63CCD15EC94E79760E1180743F5C9C46E9F1FF8A\/19","openformat":"movie","ext":"mkv","dirtitle":"[MGRT&amp;Mabors Sub]Gekijouban Ano Hi Mita Hana no Namae o Bokutachi wa Mada Shiranai. - Menu 02(BDrip 1920x1080 x264 AAC).mkv","is_blocked":0},{"id":3,"title":"[MGRT&amp;Mabors Sub]Gekijouban Ano Hi Mita Hana no Namae o Bokutachi wa Mada Shiranai. - NCOP(BDrip 1920x1080 x264 AAC).mkv","download_status":"2","cid":"D9688AA52D48BB24C6CEB4847C17E2FBBEDD6963","size":"20.9M","percent":100,"taskid":"1033432253601088","icon":"RMVB","livetime":"365\u5929","downurl":"http:\/\/gdl.lixian.vip.xunlei.com\/download?fid=2WiKpS1IuyTGzrSEfBfi+77daWNCtk8BAAAAAGDI6XLMe8VwWyjM2sYD8ZBPeXFp&mid=666&threshold=150&tid=B4071273C65DB771E13977D953D099AC&srcid=4&verno=1&g=60C8E972CC7BC5705B28CCDAC603F1904F797169&scn=u2&i=63CCD15EC94E79760E1180743F5C9C46E9F1FF8A&t=6&ui=208115253&ti=1033432253601088&s=22001218&m=0&n=013A7CA30D54264D61035E962C205375623C768134696A6F7503508A7F416E6F202958C4126974612029508A3E206E6F202F50893E65206F20235E8F2A746163680811933E204D61640011B7376972616E0058CA7F2D204E432E61CC1D447269704100DD6D307831305901C427323634202070A7762E6D6B76&ih=63CCD15EC94E79760E1180743F5C9C46E9F1FF8A&fi=20&pi=1033432253338880&ff=0&co=89D0B0F90BDB5952CFE0FBE8288D2DDC&cm=1&pk=lixian&ak=1:0:6:4&e=1447209695&ms=10485760&ck=601902F2D83C9A97ED4E6FAC83FA4010&at=1B78AFF7AE5A243001937C0530A8ADB7","vod":"1","cdn":[],"format_img":"video","filesize":"22001218","verify":"ba0671b7f5bac7ef4a80559408ff5257","url":"bt:\/\/63CCD15EC94E79760E1180743F5C9C46E9F1FF8A\/20","openformat":"movie","ext":"mkv","dirtitle":"[MGRT&amp;Mabors Sub]Gekijouban Ano Hi Mita Hana no Namae o Bokutachi wa Mada Shiranai. - NCOP(BDrip 1920x1080 x264 AAC).mkv","is_blocked":0},{"id":4,"title":"[MGRT&amp;Mabors Sub]Gekijouban Ano Hi Mita Hana no Namae o Bokutachi wa Mada Shiranai. - SP 01(BDrip 1920x1080 x264 AAC).mkv","download_status":"2","cid":"A03B272727AB66B4E6B1146810DDC637DF259AEE","size":"139M","percent":100,"taskid":"1033432253666624","icon":"RMVB","livetime":"365\u5929","downurl":"http:\/\/gdl.lixian.vip.xunlei.com\/download?fid=oDsnJyerZrTmsRRoEN3GN98lmu6xYrQIAAAAAGLpp9cenPuTuu2gf2ScgFMP9sH\/&mid=666&threshold=150&tid=CE317301A02824D3A70B07E0A3837C98&srcid=4&verno=1&g=62E9A7D71E9CFB93BAEDA07F649C80530FF6C1FF&scn=t36&i=63CCD15EC94E79760E1180743F5C9C46E9F1FF8A&t=6&ui=208115253&ti=1033432253666624&s=146039473&m=0&n=013A7CA30D54264D61035E962C205375623C768134696A6F7503508A7F416E6F202958C4126974612029508A3E206E6F202F50893E65206F20235E8F2A746163680811933E204D61640011B7376972616E0058CA7F2D2053504101D577424472691111D566323078315109D47F783236344170A51C292E6D6B1731E45F00000000&ih=63CCD15EC94E79760E1180743F5C9C46E9F1FF8A&fi=21&pi=1033432253338880&ff=0&co=F9C243ABFCA7B155387966DEA5F30B94&cm=1&pk=lixian&ak=1:0:6:4&e=1447209695&ms=10485760&ck=601902F2D83C9A97ED4E6FAC83FA4010&at=E31070775C737855FF303FC85840FCEB","vod":"1","cdn":[],"format_img":"video","filesize":"146039473","verify":"ba279489d6b6ee5f1dd19920bd6b83eb","url":"bt:\/\/63CCD15EC94E79760E1180743F5C9C46E9F1FF8A\/21","openformat":"movie","ext":"mkv","dirtitle":"[MGRT&amp;Mabors Sub]Gekijouban Ano Hi Mita Hana no Namae o Bokutachi wa Mada Shiranai. - SP 01(BDrip 1920x1080 x264 AAC).mkv","is_blocked":0},{"id":5,"title":"[MGRT&amp;Mabors Sub]Gekijouban Ano Hi Mita Hana no Namae o Bokutachi wa Mada Shiranai. - SP 02(BDrip 1920x1080 x264 AAC).mkv","download_status":"2","cid":"B29B36F826CB965DB158A32082D7532D27FF906D","size":"35.4M","percent":100,"taskid":"1033432253732160","icon":"RMVB","livetime":"365\u5929","downurl":"http:\/\/gdl.lixian.vip.xunlei.com\/download?fid=sps2+CbLll2xWKMggtdTLSf\/kG0mTTcCAAAAAL\/iLScbZ7YtJhoaD4OTklAS\/Mlr&mid=666&threshold=150&tid=3D42AABF47FD03964F44D060361AFF9B&srcid=4&verno=1&g=BFE22D271B67B62D261A1A0F8393925012FCC96B&scn=u2&i=63CCD15EC94E79760E1180743F5C9C46E9F1FF8A&t=6&ui=208115253&ti=1033432253732160&s=37178662&m=0&n=013A7CA30D54264D61035E962C205375623C768134696A6F7503508A7F416E6F202958C4126974612029508A3E206E6F202F50893E65206F20235E8F2A746163680811933E204D61640011B7376972616E0058CA7F2D2053504101D677424472691111D566323078315109D47F783236344170A51C292E6D6B1731E45F00000000&ih=63CCD15EC94E79760E1180743F5C9C46E9F1FF8A&fi=22&pi=1033432253338880&ff=0&co=889A413CD4946B4AADC203633FB8C90F&cm=1&pk=lixian&ak=1:0:6:4&e=1447209695&ms=10485760&ck=601902F2D83C9A97ED4E6FAC83FA4010&at=1857D940BD6E1F36910AD27BDF4C1586","vod":"1","cdn":[],"format_img":"video","filesize":"37178662","verify":"498d1349691a6f7ac403b77e063969c2","url":"bt:\/\/63CCD15EC94E79760E1180743F5C9C46E9F1FF8A\/22","openformat":"movie","ext":"mkv","dirtitle":"[MGRT&amp;Mabors Sub]Gekijouban Ano Hi Mita Hana no Namae o Bokutachi wa Mada Shiranai. - SP 02(BDrip 1920x1080 x264 AAC).mkv","is_blocked":0},{"id":6,"title":"[MGRT&amp;Mabors Sub]Gekijouban Ano Hi Mita Hana no Namae o Bokutachi wa Mada Shiranai.(BDrip 1920x1080 x264 FLAC AAC).big5.ass","download_status":"2","cid":"58040A73363D851B245503D2942F8DB4D69CAD5F","size":"142K","percent":100,"taskid":"1033432253797696","icon":"RAR","livetime":"365\u5929","downurl":"http:\/\/gdl.lixian.vip.xunlei.com\/download?fid=WAQKczY9hRskVQPSlC+NtNacrV9sOwIAAAAAAPkWnP+FfMcK8unvPXNu3uGFMcYF&mid=666&threshold=150&tid=779661D8252B842B5C5980E551F510AE&srcid=4&verno=1&g=F9169CFF857CC70AF2E9EF3D736EDEE18531C605&scn=u1&i=63CCD15EC94E79760E1180743F5C9C46E9F1FF8A&t=6&ui=208115253&ti=1033432253797696&s=146284&m=0&n=013A7CA30D54264D61035E962C205375623C768134696A6F7503508A7F416E6F202958C4126974612029508A3E206E6F202F50893E65206F20235E8F2A746163680811933E204D61640011B7376972616E0058CA77424472691111D566323078315109D47F783236344177A81E432041412218CA3D6967352E0042975F00000000&ih=63CCD15EC94E79760E1180743F5C9C46E9F1FF8A&fi=23&pi=1033432253338880&ff=0&co=D1601A52F60DA5C28F156B4C36BAADC9&cm=1&pk=lixian&ak=1:0:6:4&e=1447209695&ms=10485760&ck=601902F2D83C9A97ED4E6FAC83FA4010&at=BD2805F1A23E8A3E97F118C54D4AF218","vod":"0","cdn":[],"format_img":"other","filesize":"146284","verify":"9b12510b53c14701a1a932e3f4efc5c2","url":"bt:\/\/63CCD15EC94E79760E1180743F5C9C46E9F1FF8A\/23","openformat":"","ext":"ass","dirtitle":"[MGRT&amp;Mabors Sub]Gekijouban Ano Hi Mita Hana no Namae o Bokutachi wa Mada Shiranai.(BDrip 1920x1080 x264 FLAC AAC).big5.ass","is_blocked":0},{"id":7,"title":"[MGRT&amp;Mabors Sub]Gekijouban Ano Hi Mita Hana no Namae o Bokutachi wa Mada Shiranai.(BDrip 1920x1080 x264 FLAC AAC).gb.ass","download_status":"2","cid":"B6C5245A9303571FEC5F448504FA2A14D7EFF5A6","size":"142K","percent":100,"taskid":"1033432253863232","icon":"RAR","livetime":"365\u5929","downurl":"http:\/\/gdl.lixian.vip.xunlei.com\/download?fid=tsUkWpMDVx\/sX0SFBPoqFNfv9aZaOwIAAAAAAPqH9eNTUTK05pyR5gRKcTSOziAj&mid=666&threshold=150&tid=A92E865C2B13D776FDEEC88A1A78C26B&srcid=4&verno=1&g=FA87F5E3535132B4E69C91E6044A71348ECE2023&scn=c14&i=63CCD15EC94E79760E1180743F5C9C46E9F1FF8A&t=6&ui=208115253&ti=1033432253863232&s=146266&m=0&n=013A7CA30D54264D61035E962C205375623C768134696A6F7503508A7F416E6F202958C4126974612029508A3E206E6F202F50893E65206F20235E8F2A746163680811933E204D61640011B7376972616E0058CA77424472691111D566323078315109D47F783236344177A81E432041412218CA38622E61731231E45F00000000&ih=63CCD15EC94E79760E1180743F5C9C46E9F1FF8A&fi=24&pi=1033432253338880&ff=0&co=7B9E3B8DECBE1272C5AF0F374152C178&cm=1&pk=lixian&ak=1:0:6:4&e=1447209695&ms=10485760&ck=601902F2D83C9A97ED4E6FAC83FA4010&at=549524CF2A45282C5E3B6942EBEA8C83","vod":"0","cdn":[],"format_img":"other","filesize":"146266","verify":"37e0ce7c27c96aa71cf359f6a571e0e0","url":"bt:\/\/63CCD15EC94E79760E1180743F5C9C46E9F1FF8A\/24","openformat":"","ext":"ass","dirtitle":"[MGRT&amp;Mabors Sub]Gekijouban Ano Hi Mita Hana no Namae o Bokutachi wa Mada Shiranai.(BDrip 1920x1080 x264 FLAC AAC).gb.ass","is_blocked":0},{"id":8,"title":"[MGRT&amp;Mabors Sub]Gekijouban Ano Hi Mita Hana no Namae o Bokutachi wa Mada Shiranai.(BDrip 1920x1080 x264 FLAC AAC).mkv","download_status":"2","cid":"BDE1E4C2E445E03AFE12365C342C425116340423","size":"2.30G","percent":100,"taskid":"1033432253928768","icon":"RMVB","livetime":"365\u5929","downurl":"http:\/\/gdl.lixian.vip.xunlei.com\/download?fid=veHkwuRF4Dr+EjZcNCxCURY0BCNyRjWTAAAAACMhW\/VIu9RrN1HfSQ3yhtpbeBdO&mid=666&threshold=150&tid=4FE3E9C81AAD65CFE1889B80C14E7351&srcid=4&verno=1&g=23215BF548BBD46B3751DF490DF286DA5B78174E&scn=u2&i=63CCD15EC94E79760E1180743F5C9C46E9F1FF8A&t=6&ui=208115253&ti=1033432253928768&s=2469742194&m=0&n=013A7CA30D54264D61035E962C205375623C768134696A6F7503508A7F416E6F202958C4126974612029508A3E206E6F202F50893E65206F20235E8F2A746163680811933E204D61640011B7376972616E0058CA77424472691111D566323078315109D47F783236344177A81E432041412218CA326B760000&ih=63CCD15EC94E79760E1180743F5C9C46E9F1FF8A&fi=25&pi=1033432253338880&ff=0&co=4B093BF041312F70C0327DB4D9D745E3&cm=1&pk=lixian&ak=1:0:6:4&e=1447209695&ms=10485760&ck=601902F2D83C9A97ED4E6FAC83FA4010&at=FCA839D230AF7BF499A48287C6FE6361","vod":"1","cdn":[],"format_img":"video","filesize":"2469742194","verify":"8e99bf5f12b3aae8371ecaa5e2294cae","url":"bt:\/\/63CCD15EC94E79760E1180743F5C9C46E9F1FF8A\/25","openformat":"movie","ext":"mkv","dirtitle":"[MGRT&amp;Mabors Sub]Gekijouban Ano Hi Mita Hana no Namae o Bokutachi wa Mada Shiranai.(BDrip 1920x1080 x264 FLAC AAC).mkv","is_blocked":0},{"id":9,"title":"[MGRT][13.08.21] \u5287\u5834\u7248\u300c\u3042\u306e\u65e5\u898b\u305f\u82b1\u306e\u540d\u524d\u3092\u50d5\u9054\u306f\u307e\u3060\u77e5\u3089\u306a\u3044\u3002\u300d\u4e3b\u984c\u6b4c - \u30b5\u30fc\u30af\u30eb\u30b2\u30fc\u30e0\u3010\u671f\u9593\u751f\u7523\u9650\u5b9a\u30a2\u30cb\u30e1\u76e4\u3011 \u6b4c\uff1aGalileo Galilei (ALAC+cue+log+png+DVDISO).rar","download_status":"2","cid":"C0466CBD1B0D6B7A68D7D0381BE33FFAA40268FD","size":"311M","percent":100,"taskid":"1033432253994304","icon":"RAR","livetime":"365\u5929","downurl":"http:\/\/gdl.lixian.vip.xunlei.com\/download?fid=wEZsvRsNa3po19A4G+M\/+qQCaP1PiHQTAAAAAAyccZr4L96XOemFf+gPmdBnVNkD&mid=666&threshold=150&tid=1718B0786966239C73B33F3A785E6EDB&srcid=4&verno=1&g=0C9C719AF82FDE9739E9857FE80F99D06754D903&scn=u2&i=63CCD15EC94E79760E1180743F5C9C46E9F1FF8A&t=6&ui=208115253&ti=1033432253994304&s=326404175&m=0&n=013A7CA30D545D5B31521FD4672E32315D41B545D7F6B0E6A1D99546FBCEC8D5D2EB955BE4A8A4CEC39AF654FBF28357DF3E952BFBDEA4C0D6CB950DFBCAA4A4A1C2905D89F7EE7DB88911C97FA5B5A960C49E41B4A5B2A960C4D145E1C6DAE967A8CB4A3DCFDEB6A8C4934194A5E1B150C08EC4E7E8A3BA47005D8D33656F2047005D8D3365692028207DA51C2B6375654A5D8B382B706E674A75B21B49534F294F43852D00000000&ih=63CCD15EC94E79760E1180743F5C9C46E9F1FF8A&fi=26&pi=1033432253338880&ff=0&co=B3328ED373BAC2FF1024FC7FA827030F&cm=1&pk=lixian&ak=1:0:6:4&e=1447209695&ms=10485760&ck=601902F2D83C9A97ED4E6FAC83FA4010&at=B8D80E30FE9B3484F034C1817B6184FE","vod":"0","cdn":[],"format_img":"rar","filesize":"326404175","verify":"baf437596d3fc8579a37c83b3cf221c3","url":"bt:\/\/63CCD15EC94E79760E1180743F5C9C46E9F1FF8A\/26","openformat":"rar","ext":"rar","dirtitle":"[MGRT][13.08.21] \u5287\u5834\u7248\u300c\u3042\u306e\u65e5\u898b\u305f\u82b1\u306e\u540d\u524d\u3092\u50d5\u9054\u306f\u307e\u3060\u77e5\u3089\u306a\u3044\u3002\u300d\u4e3b\u984c\u6b4c - \u30b5\u30fc\u30af\u30eb\u30b2\u30fc\u30e0\u3010\u671f\u9593\u751f\u7523\u9650\u5b9a\u30a2\u30cb\u30e1\u76e4\u3011 \u6b4c\uff1aGalileo Galilei (ALAC+cue+log+png+DVDISO).rar","is_blocked":0},{"id":10,"title":"[MGRT][13.08.28] \u5287\u5834\u7248\u300c\u3042\u306e\u65e5\u898b\u305f\u82b1\u306e\u540d\u524d\u3092\u50d5\u9054\u306f\u307e\u3060\u77e5\u3089\u306a\u3044\u3002\u300d\u30aa\u30ea\u30b8\u30ca\u30eb\u30fb\u30b5\u30a6\u30f3\u30c9\u30c8\u30e9\u30c3\u30af  \u97f3\u697d\uff1aREMEDIOS (2ALAC+cue+log+png).rar","download_status":"2","cid":"CAF507BFEE8196790C8EE2792FEBB3E5F388AD8E","size":"416M","percent":100,"taskid":"1033432254059840","icon":"RAR","livetime":"365\u5929","downurl":"http:\/\/gdl.lixian.vip.xunlei.com\/download?fid=yvUHv+6BlnkMjuJ5L+uz5fOIrY5RpAYaAAAAAAoMmWGcbjxRxE0lO1jOQuDQ0NjK&mid=666&threshold=150&tid=9688E18D6FEFC21A44AE91EBADF72319&srcid=4&verno=1&g=0A0C99619C6E3C51C44D253B58CE42E0D0D0D8CA&scn=u2&i=63CCD15EC94E79760E1180743F5C9C46E9F1FF8A&t=6&ui=208115253&ti=1033432254059840&s=436642897&m=0&n=013A7CA30D545D5B31521FD4672E32385D41B545D7F6B0E6A1D99546FBCEC8D5D2EB955BE4A8A4CEC39AF654FBF28357DF3E952BFBDEA4C0D6CB950DFBCAA4A4A1C2905DFAAAA5EAA5D9942EFAEB8139A7589451FAA6A5F3A5A8942CFAE9A5C3A5CE11C48DF49853A3DB63A1124544494F3211CC6D414C41434A52913A2B6C6F674A418A38292E72611331E45F00000000&ih=63CCD15EC94E79760E1180743F5C9C46E9F1FF8A&fi=27&pi=1033432253338880&ff=0&co=BA5C3D395BA72264D88E28AA2F830C15&cm=1&pk=lixian&ak=1:0:6:4&e=1447209695&ms=10485760&ck=601902F2D83C9A97ED4E6FAC83FA4010&at=55AFB0FBED521F5190BEEB103739B7F0","vod":"0","cdn":[],"format_img":"rar","filesize":"436642897","verify":"21f40dbe89d38e1f73048e25542313c7","url":"bt:\/\/63CCD15EC94E79760E1180743F5C9C46E9F1FF8A\/27","openformat":"rar","ext":"rar","dirtitle":"[MGRT][13.08.28] \u5287\u5834\u7248\u300c\u3042\u306e\u65e5\u898b\u305f\u82b1\u306e\u540d\u524d\u3092\u50d5\u9054\u306f\u307e\u3060\u77e5\u3089\u306a\u3044\u3002\u300d\u30aa\u30ea\u30b8\u30ca\u30eb\u30fb\u30b5\u30a6\u30f3\u30c9\u30c8\u30e9\u30c3\u30af  \u97f3\u697d\uff1aREMEDIOS (2ALAC+cue+log+png).rar","is_blocked":0},{"id":11,"title":"COMPILATION MUSIC.rar","download_status":"2","cid":"E2E717DA7A121A9ADE4EC07F942D0629F619CA48","size":"379M","percent":100,"taskid":"1033432254125376","icon":"RAR","livetime":"365\u5929","downurl":"http:\/\/gdl.lixian.vip.xunlei.com\/download?fid=4ucX2noSGpreTsB\/lC0GKfYZykjR4rQXAAAAAPcL77QMfZKZQ3\/UjmC\/3PfYfwT4&mid=666&threshold=150&tid=140792B8B9FDBD3D9883D38634532C7F&srcid=4&verno=1&g=F70BEFB40C7D9299437FD48E60BFDCF7D87F04F8&scn=u2&i=63CCD15EC94E79760E1180743F5C9C46E9F1FF8A&t=6&ui=208115253&ti=1033432254125376&s=397730513&m=0&n=01227EA90F494C4154287EAA7F4D555349221F963E72000000&ih=63CCD15EC94E79760E1180743F5C9C46E9F1FF8A&fi=28&pi=1033432253338880&ff=0&co=9191E0BE47AF5E75280AF8D3C45725CB&cm=1&pk=lixian&ak=1:0:6:4&e=1447209695&ms=10485760&ck=601902F2D83C9A97ED4E6FAC83FA4010&at=C8A9FBFE2116F272CE722B5D25AAC2CF","vod":"0","cdn":[],"format_img":"rar","filesize":"397730513","verify":"00f7b749c39934c8aa438ed10b4ce115","url":"bt:\/\/63CCD15EC94E79760E1180743F5C9C46E9F1FF8A\/28","openformat":"rar","ext":"rar","dirtitle":"COMPILATION MUSIC.rar","is_blocked":0},{"id":12,"title":"Fonts.rar","download_status":"2","cid":"75F5BA6F476120B3663BB96F460D39C337111440","size":"17.7M","percent":100,"taskid":"1033432254190912","icon":"RAR","livetime":"365\u5929","downurl":"http:\/\/gdl.lixian.vip.xunlei.com\/download?fid=dfW6b0dhILNmO7lvRg05wzcRFEA7gBsBAAAAAHnPF0NBEx0oMYs70Hu9Wqg\/a0Q+&mid=666&threshold=150&tid=CE4019E6D03E3A16451051D09D531008&srcid=4&verno=1&g=79CF174341131D28318B3BD07BBD5AA83F6B443E&scn=u2&i=63CCD15EC94E79760E1180743F5C9C46E9F1FF8A&t=6&ui=208115253&ti=1033432254190912&s=18579515&m=0&n=01275E8A2B732E72611331E45F00000000&ih=63CCD15EC94E79760E1180743F5C9C46E9F1FF8A&fi=29&pi=1033432253338880&ff=0&co=B3789EBC09FF1DFE5557FDEFBCE51DDC&cm=1&pk=lixian&ak=1:0:6:4&e=1447209695&ms=10485760&ck=601902F2D83C9A97ED4E6FAC83FA4010&at=2D70E9E97F5E0CC55DCDC8B6905B50CB","vod":"0","cdn":[],"format_img":"rar","filesize":"18579515","verify":"a3d729967d842fed18c7b36308339b25","url":"bt:\/\/63CCD15EC94E79760E1180743F5C9C46E9F1FF8A\/29","openformat":"rar","ext":"rar","dirtitle":"Fonts.rar","is_blocked":0}]}})
    ```
更多XHR示例可在github项目的xhr-sample目录中找到。

### 关于迅雷离线的登录验证方式
一开始我未找到模拟登录迅雷离线的方法，是通过人工登录获取cookie的方式实现的。后来我将登录页面的JS beautify之后，找到了登录API及相应加密算法。用户密码加密基于RSA。我用nodejs+express实现了一个加密密码的API，关键代码如下：
``` javascript
app.get('/api/crypto/rsa_encrypt.do', function(req, res, next){
    var kn = req.query.check_n || '123';
    var ke = req.query.check_e || '123';
    var captcha = req.query.captcha || '123';
    var pwd = req.query.pwd || '123';
    var rsa = new RSAKey();
    rsa.setPublic(b64tohex(kn), b64tohex(ke));
    var code = captcha.toUpperCase();
    var encrypted_pwd = hex2b64(rsa.encrypt(md5(pwd) + code));
    res.json({'encrypted_pwd':encrypted_pwd})
});
```

### 离线服务器的验证
服务端脚本获取到离线下载地址后，需要弄清楚如何让用户能直接用这个链接下载。当然不能直接将服务器通过模拟登录获取到的cookie直接返回给用户。经过测试，发现用户只需要在cookie中提供正确的gdriveid的值，离线服务器就允许用户下载，因此服务器脚本需要返回该值。
要实现此功能，服务端脚本关键代码如下：
``` python
#从文件中读入模拟请求所需cookie
cookie = str(open('cookie.txt','rb').read()).replace('\n','')
...
#从cookie中提取gdriveid字段
app.config['thunder_gdriveid'] = re.search(r'gdriveid=(\w+?);', cookie).group(1)
#用于返回gdriveid的API
@app.route('/api/gdriveid', methods=['GET'])
def API_get_gdriveid():
	return jsonify({'gdriveid':app.config['thunder_gdriveid']})
```
用户只需在Aria2请求中带上此cookie即可。在用户端生成的Aria2命令如下：
```
aria2c -c -s10 -x10 --header "Cookie: gdriveid=XXXXX;" -o "filename.XXX" "http://gdl.lixian.vip.xunlei.com/download?..."
```

### 服务端实现
使用Flask框架实现了API server，写了个简易的网页版前端。在API抓到并分析清楚后已没有多大难度，用requests实现模拟操作即可。有以下几点需要注意：  
* 虽尚不明确迅雷离线网页端的登录过期时间是多少，但实验发现若服务器脚本长时间不进行重新登录，模拟请求失败率升高，因此使用cron实现每天定时重新模拟登录一次，获取新的cookie。
* 服务器需要拥有国内IP，若使用境外VPS做服务器，向迅雷服务器发出的请求很容易被reset。原因未明。
* 受服务器网络状况影响，单次请求可能耗时较长，为了提高性能，用gunicorn、uwsgi等容器来跑web server。

### CLI客户端
对于linux和mac，使用CLI更为方便。目前已上线CLI客户端，python实现，已上传pypi，通过pip可快速安装。使用详情见 https://github.com/Chion82/xunlei_share.git
