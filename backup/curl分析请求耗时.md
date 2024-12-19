## 与耗时相关的参数
以下是一些常见的 curl --write-out 即：curl -w 参数及其解释：
```
%{http_code}：输出HTTP状态码。
%{time_starttransfer}：从开始到服务器开始发送数据的时间（秒）。等同于 从请求开始到第一个字节开始传输的时间
%{time_connect}：从开始到建立TCP连接的时间（秒）。
%{time_appconnect}：从开始到SSL/SSH等应用层握手完成的时间（秒）。
%{time_pretransfer}：从开始到准备发送请求的时间（秒）。
%{time_redirect}：重定向所花费的总时间（秒）。
%{time_total}：完成请求所花费的总时间（秒）。
%{size_download}：下载的字节数。
%{size_upload}：上传的字节数。
%{url_effective}：请求的最终URL（可能包含重定向）。
%{response_rate}：数据传输速率（字节/秒）。
%{ssl_verify_result}：SSL证书验证结果。
%{filename_effective}：输出文件的名称（如果使用了-O选项）。
%{method}：请求使用的HTTP方法。
%{response_header}：输出整个响应头。
```

核心参数再理解：

**time_namelookup**：DNS 域名解析的时候，就是把域名转换成 ip 地址的过程
**time_connect**：TCP 连接建立的时间，就是三次握手的时间
**time_appconnect**：SSL/SSH 等上层协议建立连接的时间，比如 connect/handshake 的时间
**time_pretransfer**：是从 curl 开始执行到请求数据开始传输给服务器之前的准备时间。（还没正式发送请求体给服务器）
**time_starttransfer**：从请求开始到第一个字节开始传输的时间（服务器处理完毕，开始给客户端发响应）
**time_total**：这次请求花费的全部时间

**以上参数全部都是从开始请求那一刻开始算起。所以要算单独一个阶段，都要减去前面无关的。**

## 使用示例
可以将这些参数组合起来，创建一个自定义的输出格式。例如：
```
curl -w "HTTP Status: %{http_code}\n" -w "Total Time: %{time_total} seconds\n" http://example.com/
```

我们先看看一个简单的请求，没有重定向，也没有 SSL 协议的时间：
```
curl -o /dev/null -w "@curl-format.txt" -s -L "http://cizixs.com"

结果：

time_namelookup:  0.012
       time_connect:  0.227
    time_appconnect:  0.000
      time_redirect:  0.000
   time_pretransfer:  0.227
 time_starttransfer:  0.443
                    ----------
         time_total:  0.867
```

可以看到这次请求各个步骤的时间都打印出来了，每个数字的单位都是秒（seconds），这样可以分析哪一步比较耗时，方便定位问题。这个命令各个参数的意义：
```
-w：从文件中读取要打印信息的格式
-o /dev/null：把响应的内容丢弃，因为我们这里并不关心它，只关心请求的耗时情况
-s：不要打印进度条
从这个输出，我们可以算出各个步骤的时间：
DNS 查询：12ms
TCP 连接时间：pretransfter(227) - namelookup(12) = 215ms
发送请求体+服务器处理时间：starttransfter(443) - pretransfer(227) = 216ms
内容传输时间：total(867) - starttransfer(443) = 424ms
```

来个比较复杂的，访问某度首页，带有中间有重定向和 SSL 协议：

```
curl -w "@curl-format.txt" -o /dev/null -s -L "https://baidu.com"/

结果：

    time_namelookup:  0.012
       time_connect:  0.018
    time_appconnect:  0.328
      time_redirect:  0.356
   time_pretransfer:  0.018
 time_starttransfer:  0.027
                    ----------
         time_total:  0.384
```

可以看到 time_appconnect 和 time_redirect 都不是 0 了，其中 SSL 协议处理时间为 328-18=310ms。而且 pretransfer 和 starttransfer 的时间都缩短了，这是重定向之后请求的时间。（有重定向以后，需要考虑的比较多，实际拨测中，不要选取可能重定向的）

## shell脚本使用指引
根据curl的-w参数功能，可以写脚本用来分析http请求耗时，以及tls是否超时，然后告警出来，是可行的拨测监控方式。
定时方式，可以结合crontab，或者使用更加靠谱的python定时任务包：crond。

shell脚本示例：
```
#!/bin/bash
echo '----------------------本次拨测开始----------------------'
# 需要检测的url列表 从文件获取  不同机器路径可能不一样
DOMAIN_URL_FILE="domain.txt"
# 获取本机ip
GETIP=`ip a | grep -oP '(?<=inet\s)\d+(\.\d+){3}' | grep -v '127.0.0.1' | head -n 1`
# 设置超时的时间间隔
TIME_OUT=1.0
echo "拨测机器ip：$GETIP"
# 机器主机名
MACHINE=`hostname`
echo "机器名：$MACHINE"
# 当前时间 也就是发送告警时设置的告警时间 它比实际告警出来的时间要更早一些
ADATE=$(date +'%Y-%m-%d %H:%M:%S')
echo "本次拨测时间：$ADATE"
echo "超时设置：$TIME_OUT 秒"
# 生成随机IP函数 不发送随机ip 蓝鲸告警会收敛过滤
function generate_random_ip()
{
    local ip1=$((RANDOM % 256))
    local ip2=$((RANDOM % 256))
    local ip3=$((RANDOM % 256))
    local ip4=$((RANDOM % 256))
    echo "${ip1}.${ip2}.${ip3}.${ip4}"
}
# 发送tls超时告警到蓝鲸
alarm()
{
    curl -d '{
    "ip": "'$(generate_random_ip)'",
    "source_time": "'"${ADATE}"'",
    "alarm_type": "tls告警",
    "level": "fatal",
    "alarm_name": "tls连接超时告警",
    "alarm_content": "curl拨测告警明细:  '$MACHINE' - '$GETIP' 访问url：'$3' 握手时长超过'$TIME_OUT'秒触发告警。具体数据：tcp建立连接耗时'$2'秒，tls建立连接耗时'$1'秒，客户端发送请求之前总耗时'$7'秒，客户端开始发送请求+服务器处理完请求后开始返回内容总耗时'$4'秒，服务器响应内容传输总耗时'$5'秒，本次http请求总的访问耗时'$6'秒，请核实确认tls/ssl相关。",
    "action": "firing"
    }' https://appops.haday.cn/o/cw_uac_saas/alarm/collect/event/api/34da4ffa-4158-41c9-a0c4-c4380f67c520/ -H 'X-Secret:AncecVal59WdMDr8KP0BivbJQLym4yu4'  -k
    echo "告警已发送"
}
# 计算tls与tcp握手时长，如果二者之差大于超时时间，则发送告警
# 注意  tls和tcp时间可能获取不到，比如请求失败或者超时 这些场景将不告警
print_tcp_tls_time()
{
    cat $DOMAIN_URL_FILE | while read line
    do
        echo "拨测URL：$line"
        res=`curl -w "%{time_connect},%{time_appconnect},%{time_namelookup},%{time_total},%{url_effective},%{http_code},%{time_redirect},%{time_pretransfer},%{time_starttransfer},%{size_download},%{speed_download}\n" -so /dev/null -L $line`
        echo "拨测结果：$res"
        url_effective=`echo $res | cut -d "," -f 5`
        echo "最终访问的url-url_effective: $url_effective"
        http_code=`echo $res | cut -d "," -f 6`
        echo "HTTP状态码-http_code: $http_code"
        size_download=`echo $res | cut -d "," -f 10`
        echo "下载大小-size_download: $size_download bytes"
        speed_download=`echo $res | cut -d "," -f 11`
        echo "下载速率-speed_download: $speed_download bytes/s"
        echo "耗时分析开始。============================================="
        ns_time=`echo $res | cut -d "," -f 3 | bc`
        echo "开始请求到DNS解析完成时间-time_namelookup: $ns_time"
        tcp_time=`echo $res | cut -d "," -f 1 | bc`
        echo "从开始请求到建立tcp连接完成时间-time_connect: $tcp_time"
        # tcp建立时间  不包含dns解析
        only_tcp_time=$(echo "$tcp_time - $ns_time" | bc)
        echo "tcp建立时间  不包含dns解析-only_tcp_time: $only_tcp_time"
        tls_time=`echo $res | cut -d "," -f 2 | bc`
        echo "从开始请求到建立tls连接完成时间-time_appconnect: $tls_time"
        # tls连接建立时间 不包含dns解析 不包含tcp连接
        only_tls_time=$(echo "$tls_time - $tcp_time" | bc)
        echo "tls连接建立时间 不包含dns解析 不包含tcp连接-only_tls_time: $only_tls_time"
        time_redirect=`echo $res | cut -d "," -f 7 | bc`
        echo "重定向耗时-time_redirect: $time_redirect"
        time_pretransfer=`echo $res | cut -d "," -f 8 | bc`
        echo "传输前准备时间-time_pretransfer: $time_pretransfer"
        time_starttransfer=`echo $res | cut -d "," -f 9 | bc`
        echo "开始传输时间-time_starttransfer: $time_starttransfer"
        time_total=`echo $res | cut -d "," -f 4 | bc`
        echo "----------总的耗时-也等于下面三项耗时之和---------: $time_total"
        echo "发送请求体给服务器之前，耗时：$time_pretransfer"
        # 请求体发送给服务器，并且服务器处理完毕开始返回第一个字节的时间
        server_time=$(echo "$time_starttransfer - $time_pretransfer " | bc)
        echo "发送请求体 + 服务器处理时间-server_time: $server_time"
        # 内容传输时间 服务器开始返回第一个字节 到全部传输完毕的耗时
        content_trans_time=$(echo "$time_total - $time_starttransfer " | bc)
        echo "内容传输时间-content_trans_time: $content_trans_time"
        echo "耗时分析结束。=============================================="
        time_out=`echo $TIME_OUT|bc`
        tls_timeout=$(echo "$tls_time - $tcp_time - $time_out" | bc)
        if [ $(echo "$tls_timeout > 0" | bc) -eq 1 ]; then
            echo "tls连接建立耗时，减去设置的超时timeout，结果 > 0 超时了 异常，发送告警"
            # 此处alarm函数会获取后面的参数列表 $1 $2 $3等
            alarm $only_tls_time $only_tcp_time $line $server_time $content_trans_time $time_total $time_pretransfer
        else
            echo "tls连接建立耗时，减去设置的超时timeout，结果 < 0 没超时 正常"
        fi
    done
}
print_tcp_tls_time
echo '----------------------本次拨测结束----------------------'
```

crontab脚本定时：
```
* */5  *  *  *  sh /app/tls_alarm.sh 2>&1 >> /app/log.txt

crontab这种方式实际运行中不稳定，尤其是涉及频繁修改执行间隔，可能有bug，运行有时不生效，所以改成python的定时包。

nohup python cron_sh.py > /dev/null 2>&1 &

同样也是依赖系统的crond服务，关闭crond后就无法工作。所以要确保crond正常工作。

sudo systemctl stop crond
sudo systemctl start crond
sudo systemctl status crond
```

python定时包crond使用示例：
```
import subprocess
from datetime import datetime
from apscheduler.schedulers.blocking import BlockingScheduler
def run_script():
    # 获取当前日期并格式化为YYYY-MM-DD
    current_date = datetime.now().strftime('%Y-%m-%d')
    # 创建日志文件名，每天一个
    log_filename = f'./logs/{current_date}.log'
    with open(log_filename, 'a') as log_file:
        try:
            # 使用subprocess.Popen执行shell脚本
            process = subprocess.Popen(['./tls.sh'], stdout=subprocess.PIPE, stderr=subprocess.PIPE)
            stdout, stderr = process.communicate()
            # 获取当前时间戳
            current_time = datetime.now().strftime('%Y-%m-%d %H:%M:%S')
            # 写入脚本输出到log文件
            log_file.write(f'[{current_time}] Output:\n')
            if stdout:
                log_file.write(stdout.decode() + '\n')
            if stderr:
                log_file.write(f'[{current_time}] Error:\n')
                log_file.write(stderr.decode() + '\n')
        except Exception as e:
            # 获取当前时间戳
            current_time = datetime.now().strftime('%Y-%m-%d %H:%M:%S')
            # 写入异常信息到log文件
            log_file.write(f'[{current_time}] An error occurred: {e}\n')
# 创建一个调度器实例
scheduler = BlockingScheduler()
# 每分钟执行一次
scheduler.add_job(run_script, 'interval', minutes=1)
# 启动调度器
scheduler.start()  # 这行代码在实际环境中会启动调度器，但在这里不会执行
```

## 参考
- https://blog.csdn.net/zzhongcy/article/details/23372535