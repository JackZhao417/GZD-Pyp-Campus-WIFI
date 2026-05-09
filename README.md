# 🎓 GZD-Pyp-Campus-WIFI
> 广州多所高效多设备验证绕过技术原理，支持同一账号无限制多设备同时在线
---
##技术  
本人只是学习零散技术并整合测试，然后通过恩山论坛里面的大佬指引下刷好了固件，在此感谢所有开源技术的大佬们
## ✨ 项目背景
我是真被这个设备验证搞烦了，最多只能一台电脑一个手机登陆同一个账号，以至于我无论如何都要攻破这一难关，在经历了九九八十一难后，我发现我一开始的方向就错了，原来是那样的简单。

## 🚀 核心功能
- 🔓 绕过校园网多设备登录限制，实现多台设备登陆同一个wifi
## 📦 快速开始
### 设备要求
本人测试设备是redmi ac 2100以及xiaomi ac 2100（老生常谈的ac2100），需要自己刷入breed不死鸟，这个我不提供教程，因为这个就是个鬼门关。
### 部署步骤
1.进入breed界面。按住路由器上的reset按钮，一般成功进入后路由器会闪蓝灯，然后用网线与电脑连接，在浏览器输入192.168.1.1进入breed页面

2.在breed页面上点击固件更新，然后在固件那里选择文件<img width="3102" height="1189" alt="校园网技术项目GitHub页面搭建方案" src="https://github.com/user-attachments/assets/208be4a8-2d7f-40c8-8908-b3b0dcfdd784" />
3.然后刷入initramfs-kernel文件，等待重启（务必耐心等待哦）
4.疯狂刷新192.168.1.1页面（又或者是其他ip地址，自己ipconfig一下），直到出现这个页面（一般是没有密码直接登陆的，又或者密码是root，我也忘记了。进入后先重新设置一下root密码，随便简单一点）
<img width="1889" height="913" alt="image" src="https://github.com/user-attachments/assets/b1db8981-174a-4143-a7f8-f7397c8ddcfb" />
5.根据箭头步骤刷入squashfs-sysupgrade固件，<img width="1920" height="911" alt="image" src="https://github.com/user-attachments/assets/b2448e22-fb4c-456f-918e-709ac9d3d0c4" />
6.然后就是反检测设置了，修改NTP时间同步服务器。添加4个：  
ntp1.aliyun.com  
time1.cloud.tencent.com  
stdtime.gov.hk  
pool.ntp.org

然后保存并应用<img width="1920" height="911" alt="image" src="https://github.com/user-attachments/assets/986bda7d-9681-4015-a6b2-e8187f40bf11" />
7.然后就是防火墙规则设置了，直接就是全部复制粘贴：
#防时钟偏移检测
iptables -t nat -N ntp_force_local
iptables -t nat -I PREROUTING -p udp --dport 123 -j ntp_force_local
iptables -t nat -A ntp_force_local -d 0.0.0.0/8 -j RETURN
iptables -t nat -A ntp_force_local -d 127.0.0.0/8 -j RETURN
iptables -t nat -A ntp_force_local -d 192.168.0.0/16 -j RETURN
iptables -t nat -A ntp_force_local -s 192.168.0.0/16 -j DNAT --to-destination 192.168.1.1
#修改 TTL 值
iptables -t mangle -A POSTROUTING -j TTL --ttl-set 128 
然后保存
<img width="1920" height="911" alt="image" src="https://github.com/user-attachments/assets/818bc48c-05a0-45a5-ac4e-0b3b5a3704a4" />
8.配置ua2f（UA3F其实更稳定，期望未来能看到有同学出UA3F的教程）
输入用户名和密码（一般都是root）
<img width="1920" height="911" alt="image" src="https://github.com/user-attachments/assets/6debcc6e-5552-4797-a377-4bab69fc5e0f" />
然后依次输入  

uci set ua2f.enabled.enabled=1  

uci set ua2f.firewall.handle_fw=1  

uci set ua2f.firewall.handle_intranet=1  

uci commit ua2f  

service ua2f start  

最后就是
输入service看ua2f是否正常运行

<img width="1920" height="911" alt="image" src="https://github.com/user-attachments/assets/d85814c4-a07a-47d1-bbc4-9204d24eca9c" />
至此部署完毕，然后可以开启无线功能爽快地多设备连接啦（经过反复测试，只开启5g频段 wifi反而更加稳定）  
<img width="1920" height="911" alt="image" src="https://github.com/user-attachments/assets/d6b695b9-4a30-4786-bbb6-6f7c894b031e" />
注意：强烈建议更换一个名字，让别人以为你这是手机热点（因为你也不想被发现然后校园网来个大整顿更新吧），然后其他没特别注明的默认不要改噢，可能会烧机
<img width="1920" height="911" alt="image" src="https://github.com/user-attachments/assets/ce799251-0968-4c8a-82a4-e9f6573ef5bc" />
<img width="1920" height="911" alt="image" src="https://github.com/user-attachments/assets/ee12eb7f-69e3-417a-a5cb-8252a40af185" />
弄好了一定要保存并应用
<img width="1920" height="911" alt="image" src="https://github.com/user-attachments/assets/98dfaa70-c0c3-4ea7-8b44-f4bd54a9be51" />

⚠️ 免责声明
本项目仅供学习交流使用，使用过程中请遵守学校相关网络管理规定，由此产生的一切后果由使用者自行承担。  

⭐ 如果这个项目帮到了你，欢迎点个 Star 支持！
