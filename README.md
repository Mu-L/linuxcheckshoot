```shell
*********************************************************************
    __     __                      ______            
   / /    /_/____   __  __ _  __ / ____/__  __ ____ 
  / /    / // __ \ / / / /| |/_// / __ / / / // __ \
 / /___ / // / / // /_/ /_>  < / /_/ // /_/ // / / /
/_____//_//_/ /_/ \__,_//_/|_| \____/ \__,_//_/ /_/ 
                                                    

Version:5.0
Author:sun977
Mail:jiuwei977@foxmail.com
Date:2024.07.31


linuxcheck.sh 更新日志:
	2024.06.16:
		1、优化最近24h变化文件只看文件不看目录,同时排除目录/proc,/dev,/sys,/run
		2、修改了找不到高危端口的文件bug
		3、增加了检测系统环境变量的功能[.bashrc|.bash_profile|.zshrc|.viminfo等]
		4、增加了journalctl日志输出
        2024.07.17:
		1、修改了logo显示bug
	2024.07.30：
		1、优化掉ifconfig命令全部使用ip替换
	2024.x.x:
		1、支持多linux系统
		2、添加容器检查内容
		3、添加k8s检查内容
	[说明]:
		1、linuxcheck.sh 可正常使用,照常维护和更新,其更新日志记录在linuxcheck.sh中
		2、linuxGun.sh 为新开发 5.x 版本脚本,模块化分解(开发中)
		3、README.md 的变动记录的是 linuxGun.sh 设想和实现
		4、一些好的建议和更新会在 linuxGun.sh 和 linuxcheck.sh 中都体现


检查说明:
	1.首先采集原始信息保存到当前目录的 output/liuxcheck_[your-ip]_[date]/check_file 目录下
	2.将系统日志、应用日志打包并保存到当前目录的 output/liuxcheck_[your-ip]_[date]/check_file/log 目录下
	3.在检查过程中检查项的结果会输出到当前目录 output/liuxcheck_[your-ip]_[date]/check_file/checkresult.txt 文件中
	4.在检查过程中若发现存在问题则直接输出到当前目录 output/liuxcheck_[your-ip]_[date]/check_file/saveDangerResult.txt 文件中
	5.有些未检查可能存在问题的需要人工分析原始文件
	6.脚本编写环境Centos7,在实际使用过程中若发现问题可以邮件联系:jiuwei977@foxmail.com
	7.使用过程中若在windows下修改再同步到Linux下,请使用dos2unix工具进行格式转换,不然可能会报错
	8.在使用过程中必须使用root账号,不然可能导致某些项无法分析
	9.checkrules目录下存放的是一些检测规则,可以根据实际情况进行修改

如何使用:
	1.需要将本脚本上传到相应的服务器中
	2.执行 chmod +x linuxcheck.sh
	3.执行 ./linuxcheck.sh 即可运行检查

功能设计:
	1.采集系统基础环境信息
	2.将原始数据进行分析,并找出存在可疑或危险项
	3.增加基线检查的功能
	4.黑客工具检查功能
	5.所有系统通用(待定)

检查内容:
	1.系统基础信息
		1.1 IP地址信息
		1.2 系统版本信息
		1.3 发行版本信息
	2.网络连接
		2.1 ARP表项
		2.2 ARP攻击
		2.3 网络连接信息
			2.3.1 网络连接情况
			2.3.2 端口监听情况[3.端口信息]
				  2.3.2.1 TCP开放端口
				  2.3.2.2 TCP高危端口[遍历端口规则]
				  2.3.2.3 UDP开放端口
				  2.3.2.4 UDP高危端口[遍历端口规则]
			2.3.3 网络DNS
		2.4 网卡工作模式
			2.4.1 网卡混杂模式
			2.4.2 网卡监听模式
		2.5 网络路由
			2.5.1 网络路由表
			2.5.2 网络路由转发
		2.6 防火墙策略
			2.6.1 firewalld策略
			2.6.2 iptables策略
	3.端口信息[新版迁移到2.3.2--作废]
		3.1 TCP开放端口
		3.2 TCP高危端口
		3.3 UDP开放端口
		3.4 UDP高危端口
	4.系统进程[改名:3.进程分析--包含18.性能分析]
		4.1 系统进程信息
		4.2 系统进程分析
		4.3 敏感进程匹配[规则匹配]
	5.自启动项
		5.1 用户自启动项
		5.2 系统自启动项
		5.3 危险启动项分析
	6.定时任务[添加计划任务文件修改时间检查--输出所有的计划任务]
		6.1 系统定时任务收集
		6.2 系统定时任务分析
		6.3 用户定时任务收集
		6.4 用户定时任务分析
	7.系统服务
	8.关键文件检查
		8.1 hosts文件[11.历史命令]
		8.2 公钥文件
		8.3 私钥文件
		8.4 authorized_keys文件
		8.5 known_hosts文件
		8.6 tmp目录检查
		8.7 环境变量检查
		8.8 /root下隐藏文件检查
	9.用户登录情况
		9.1 正在登陆的用户
		9.2 用户信息[passwd文件]
		9.3 超级用户信息
		9.4 克隆用户信息
		9.5 可登录用户信息
		9.6 非系统用户信息
		9.7 检查shadow文件
		9.8 空口令用户
		9.9 空口令且可登录用户
		9.10 口令未加密用户
		9.11 用户组信息
			9.11.1 用户组信息
			9.11.2 特权用户组
			9.11.3 相同GID用户组
			9.11.4 相同用户组名
		9.12 sshd登陆配置[归类到文件排查中]
			9.12.1 sshd配置
			9.12.2 空口令登录
			9.12.3 root远程登录
			9.12.4 ssh协议版本
		9.13 文件权限
			9.13.1 etc文件权限
			9.13.2 shadow文件权限
			9.13.3 passwd文件权限
			9.13.4 group文件权限
			9.13.5 securetty文件权限
			9.13.6 services文件权限
			9.13.7 grub.conf文件权限
			9.13.8 xinetd.conf文件权限
			9.13.9 lilo.conf文件权限
			9.13.10 limits.conf文件权限
		9.14 文件属性
			9.14.1 passwd文件属性
			9.14.2 shadow文件属性
			9.14.3 gshadow文件属性
			9.14.4 group文件属性
		9.15 useradd和userdel时间属性
			9.15.1 useradd时间属性
			9.15.2 userdel时间属性
	10.配置策略检查(基线检查)--[基线检查放后]
		10.1 远程访问策略
			10.1.1 远程允许策略
			10.1.2 远程拒绝策略
		10.2 账号与密码策略
			10.2.1 密码有效期策略
				10.2.1.1 口令生存周期
				10.2.1.2 口令更改最小时间间隔
				10.2.1.3 口令最小长度
				10.2.1.4 口令过期时间天数
			10.2.2 密码复杂度策略
			10.2.3 密码已过期用户
			10.2.4 账号超时锁定策略
			10.2.5 grub密码策略检查
			10.2.6 lilo密码策略检查
		10.3 selinux策略
		10.4 sshd配置
			10.4.1 sshd配置
			10.4.2 空口令登录
			10.4.3 root远程登录
			10.4.4 ssh协议版本
		10.5 NIS配置策略
		10.6 Nginx配置策略
			10.6.1 原始配置
			10.6.2 可疑配置
		10.7 SNMP配置检查
	11.历史命令[优先级需要提高--关键文件检查]
		11.1 系统历史命令
			11.1.1 系统操作历史命令
			11.1.2 是否下载过脚本文件
			11.1.3 是否增加过账号
			11.1.4 是否删除过账号
			11.1.5 历史可疑命令
			11.1.6 本地下载文件
			11.1.7 yum下载记录
			11.1.8 关闭历史命令记录
		11.2 数据库历史命令
	12.可疑文件检查
		12.1 检查脚本文件
		12.2 检查webshell文件(需要第三方工具)
		12.3 检查最近变动的敏感文件
		12.4 检查最近变动的所有文件
		12.5 黑客工具检查
	13.系统文件完整性校验
	14.系统日志分析
		14.1 日志配置与打包
			14.1.1 查看日志配置
			14.1.2 日志是否存在
			14.1.3 日志审核是否开启
			14.1.4 自动打包日志
		14.2 secure日志分析
			14.2.1 成功登录
			14.2.2 登录失败
			14.2.3 窗口登陆情况
			14.2.4 新建用户与用户组
		14.3 message日志分析
			14.3.1 传输文件
			14.3.2 历史使用DNS
		14.4 cron日志分析
			14.4.1 定时下载
			14.4.2 定时执行脚本
		14.5 yum日志分析
			14.5.1 下载软件情况
			14.5.2 卸载软件情况
			14.5.3 下载可疑软件
		14.6 dmesg日志分析
			14.6.1 内核自检分析
		14.7 btmp日志分析
			14.7.1 错误登录分析
		14.8 lastlog日志分析
			14.8.1 所有用户最后一次登录分析
		14.9 wtmp 日志分析
			14.9.1 所有用户登录分析
		14.10 journalctl 日志输出
	15.内核检查
		15.1 内核信息
		15.2 异常内核
	16.安装软件(rpm)
		16.1 安装软件
		16.2 可疑软件
	17.环境变量
	18.性能分析[df|ps规并到--3.进程分析]
		18.1 磁盘使用
			18.1.1 磁盘使用情况
			18.1.2 磁盘使用过大
		18.2 CPU
			18.2.1 CPU情况
			18.2.2 占用CPU前五进程
			18.2.3 占用CPU较多资源进程
		18.3 内存
			18.3.1 内存情况
			18.3.2 占用内存前五进程
			18.3.3 占用内存占多进程
		18.4 系统运行及负载
			18.4.1 运行时间及负载情况
	19.统一结果打包
		19.1 系统原始日志统一打包
		19.2 检查脚本日志统一打包
-----------------------------------------

linuxGun.sh 和 linuxcheck.sh 区别
	1、linuxcheck.sh 是完整的 linux 系统检查脚本,程序自动执行,会一次性采集机器上全部日志信息。
	2、linuxcheck.sh 的输出的结果方便安全人员脱机检查,不必机器交互使用。
	3、linuxGun.sh 是设计之初就是交互式的 linux 系统检查脚本,需要安全人员交互执行。
	4、linuxGun.sh 是针对安全人员使用,安全人员可以自定义检查内容,可以自定义检查内容。
	5、linuxGun.sh 不自带输出功而 linuxcheck.sh 会自动输出检查结果并打包系统日志。
	6、linuxGun.sh 功能上基本完整。

linuxGun.sh 更新日志	
	2024-08-08:
		1、完善systemCheck检查函数
	2025-06-29:
		1、所有功能封装成函数由 main 函数统一调用
		2、基础功能已经完备
		3、支持单独调用一个或者模块

linuxGun.sh 概要
	系统信息排查
	  	- IP地址
	- 系统基础信息
	    - 系统版本信息
	    - 系统发行版本
	- 用户信息分析
	    - 正在登录用户
	    - 系统最后登录用户
	    - 用户信息passwd文件分析
	    - 检查可登录用户
	    - 检查超级用户(除root外)
	    - 检查克隆用户
	    - 检查非系统用户
	    - 检查空口令用户
	    - 检查空口令且可登录用户
	    - 检查口令未加密用户
	    - 用户组信息group文件分析
	    - 检查特权用户组(除root组外)
	    - 相同GID用户组
	    - 相同用户组名
	- 计划任务分析
	    - 系统计划任务
		- 用户计划任务
	- 历史命令分析
	    - 输出当前shell系统历史命令[history]
	    - 输出用系历史命令[.bash_history]
		- 是否下载过脚本文件
		- 是否通过主机下载,传输过文件
		- 是否增加,删除过账号
		- 是否执行过黑客命令
		- 其他敏感命令
		- 检查系统中所有可能的历史文件路径[补充]
		- 输出系统中所有用户的历史文件[补充]
		- 输出数据库操作历史命令
	网络链接排查
	- ARP 攻击分析
	- 网络连接分析
	- 端口信息排查
	    - TCP 端口检测
		- TCP 高危端口(自定义高危端口组)
		- UDP 端口检测
		- UDP 高危端口(自定义高危端口组)
	- DNS 信息排查
	- 网卡工作模式
	- 网络路由信息排查
	- 路由转发排查
	- 防火墙策略排查
	进程排查
	- ps进程分析
	- top进程分析
	- 规则匹配敏感进程(自定义进程组)
	文件排查
	- 系统服务排查
		- 系统服务收集
		- 系统服务分析
			- 系统自启动服务分析
			- 系统正在运行的服务分析
		- 用户服务分析
	- 敏感目录排查
		- /tmp目录
		- /root目录(隐藏文件)【隐藏文件分析】
	- 特殊文件排查
		- ssh相关文件排查
			- .ssh目录排查
			- 公钥私钥排查
			- authrized_keys文件排查
			- known_hosts文件排查
			- sshd_config文件分析
				- 所有开启的配置(不带#号)
				- 检测是否允许空口令登录
				- 检测是否允许root远程登录
				- 检测ssh协议版本
				- 检测ssh版本
		- 环境变量排查
			- 环境变量文件分析
			- env命令分析
		- hosts文件排查
		- shadow文件排查
			- shadow文件权限
			- shadow文件属性
			- gshadow文件权限
			- gshadow文件属性
		- 24小时变动文件排查
		— SUID/SGID文件排查	
	- 日志文件分析
		- message日志分析
			- ZMODEM传输文件
			- 历史使用DNS情况
		- secure日志分析
			- 登录成功记录分析
			- 登录失败记录分析(SSH爆破)
			- SSH登录成功记录分析
			- 新增用户分析
			- 新增用户组分析
		- 计划任务日志分析(cron)
		    - 定时下载文件
			- 定时执行脚本
		- yum日志分析
		    - yum下载记录
			- yum卸载记录
			- yum安装可疑工具
		- dmesg日志分析[内核自检日志]
		- btmp日志分析[错误登录日志]
		- lastlog日志分析[所有用户最后一次登录日志]
		- wtmp日志分析[所有用户登录日志]
		- journalctl工具日志分析
		   	- 最近24小时日志
		- auditd 服务状态
		- rsyslog 配置文件
	后门排查
	webshell排查
	病毒排查(挖矿)
	内存排查
	黑客工具排查
	- 黑客工具匹配(规则自定义)
	- 常见黑客痕迹排查(待完成)
	内核排查
	- 内核驱动排查
	- 可疑驱动排查(自定义可疑驱动列表)
	其他排查
	- 可疑脚本文件排查
	- 系统文件完整性校验(MD5)
	- 安装软件排查
	k8s排查
	系统性能分析
	- 磁盘使用情况
	- CPU使用情况
	- 内存使用情况
	- 系统负载情况
	- 网络流量情况
	基线检查
	- 1.账户管理
	    - 1.1 账户审查(用户和组策略) -- userInfoCheck() 需要修改成通过不通过
	    	- 系统最后登录用户
			- 用户信息passwd文件分析
			- 检查可登录用户
			- 检查超级用户(除root外)
			- 检查克隆用户
			- 检查非系统用户
			- 检查空口令用户
			- 检查空口令且可登录用户
			- 检查口令未加密用户
			- 用户组信息group文件分析
			- 检查特权用户组(除root组外)
			- 相同GID用户组
			- 相同用户组名
		- 1.2 密码策略
	    	- 密码有效期策略
				- 口令生存周期
				- 口令更改最小时间间隔
				- 口令最小长度
				- 口令过期时间天数
			- 密码复杂度策略
			- 密码已过期用户
			- 账号超时锁定策略
			- grub2密码策略检查
			- grub密码策略检查(存在版本久远-弃用)
			- lilo密码策略检查(存在版本久远-弃用)
		- 1.3 远程登录限制
	    	- 远程访问策略(基于 TCP Wrappers)
		    	- 远程允许策略
				- 远程拒绝策略
		- 1.4 认证与授权
			- SSH安全增强
				- sshd配置
				- 空口令登录
				- root远程登录
				- ssh协议版本
			- PAM策略
			- 其他认证服务策略
	- 2.文件权限及访问控制
		- 关键文件保护(文件或目录的权限及属性)
			- 文件权限策略
				- etc文件权限
				- shadow文件权限
				- passwd文件权限
				- group文件权限
				- securetty文件权限
				- services文件权限
				- grub.conf文件权限
				- xinetd.conf文件权限
				- lilo.conf文件权限(存在版本久远-弃用)
				- limits.conf文件权限
				    - core dump 关闭
			- 系统文件属性检查
				- passwd文件属性
				- shadow文件属性
				- gshadow文件属性
				- group文件属性
			- useradd 和 usedel 的时间属性
	- 3.网络配置与服务
		- 端口和服务审计
		- 防火墙配置
			- 允许服务IP端口
		- 网络参数优化
	- 4.selinux策略
	- 5.服务配置策略
		- NIS配置策略
		- SNMP配置检查
		- Nginx配置策略
	- 6.日志记录与监控
		- rsyslog服务
			- 服务开启
  			- 文件权限默认
		- audit服务
		- 日志轮转和监控
		- 实时监控和告警
	- 7.备份和恢复策略
	- 8.其他安全配置基准
	

*********************************************************************

##### 主函数二级参数使用参考
###### 主函数入口
main() {
    if [ $# -eq 0 ]; then
        usage
        exit 1
    fi

    local i=1
    while [ $i -le $# ]; do
        eval "arg=\${$i}"
        case "$arg" in
            -h|--help)
                usage
                exit 0
                ;;
            --firewall)
                ((i++))
                eval "sub_arg=\${$i}"
                case "$sub_arg" in
                    rule)
                        firewallRulesCheck
                        ;;
                    policy)
                        firewallDefaultPolicyCheck
                        ;;
                    *)
                        echo -e "${RED}[!] 未知的子选项: $sub_arg${NC}"
                        usage
                        exit 1
                        ;;
                esac
                ;;
            --baseline)
                ((i++))
                eval "sub_arg=\${$i}"
                case "$sub_arg" in
                    user)
                        baselineUserCheck
                        ;;
                    auth)
                        baselineAuthCheck
                        ;;
                    network)
                        baselineNetworkCheck
                        ;;
                    *)
                        echo -e "${RED}[!] 未知的子选项: $sub_arg${NC}"
                        usage
                        exit 1
                        ;;
                esac
                ;;
            --all)
                echo -e "${YELLOW}[+] 开始执行所有检查项:${NC}"
                firewallRulesCheck
                firewallDefaultPolicyCheck
                baselineUserCheck
                baselineAuthCheck
                baselineNetworkCheck
                echo -e "${GREEN}[+] 所有检查项已完成${NC}"
                ;;
            *)
                echo -e "${RED}[!] 未知的一级选项: $arg${NC}"
                usage
                exit 1
                ;;
        esac
        ((i++))
    done
}

###### 显示使用帮助
usage() {
    echo -e "${GREEN}LinuxGun 安全检查工具 v5.0 使用说明${NC}"
    echo -e "${GREEN}使用方法: ./\$(basename \$0) [选项] [子选项]${NC}"
    echo -e "${GREEN}可用选项及子选项:${NC}"
    echo -e "${GREEN}  --firewall rule|policy       防火墙策略检查${NC}"
    echo -e "${GREEN}  --baseline user|auth|network 基线安全检查${NC}"
    echo -e "${GREEN}  --all                        执行所有检查项${NC}"
    echo -e "${GREEN}  -h, --help                   显示帮助信息${NC}"
}

调用：
###### 检查防火墙默认策略
./linuxgun.sh --firewall policy

```

			--psinfo)
				modules+=("processInfo")
				;;
			--file)
				modules+=("fileCheck")
				;;
			--file-systemservice)
				modules+=("systemServiceCheck")
				;;
			--file-dir)
				modules+=("dirFileCheck")
				;;
			--file-keyfiles)
				modules+=("specialFileCheck")
				;;
			--file-systemlog)
				modules+=("systemLogCheck")
				;;
			--backdoor)
				modules+=("backdoorCheck")
				;;
			--webshell)
				modules+=("webshellCheck")
				;;
			--virus)
				modules+=("virusCheck")
				;;
			--memInfo)
				modules+=("memInfoCheck")
				;;
			--hackerTools)
				modules+=("hackerToolsCheck")
				;;
			--kernel)
				modules+=("kernelCheck")
				;;
			--other)
				modules+=("otherCheck")
				;;
			--k8s)
				modules+=("k8sCheck")
				;;
			--performance)
				modules+=("performanceCheck")
				;;
			--baseline)
                modules+=("baselineCheck")
                ;;
			--baseline-firewall)
				modules+=("firewallRulesCheck")
				;;
			--baseline-selinux)
				modules+=("selinuxStatusCheck")
				;;