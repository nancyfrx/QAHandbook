<font style="color:rgb(64, 64, 64);">本文解析压测中 </font>**<font style="color:rgb(64, 64, 64);">8种TPS曲线形态</font>**<font style="color:rgb(64, 64, 64);"> 背后的秘密，提供 </font>**<font style="color:rgb(64, 64, 64);">“看图识病”</font>**<font style="color:rgb(64, 64, 64);"> 的诊断方法论，并附赠匹配不同趋势的 </font>**<font style="color:rgb(64, 64, 64);">调优工具包</font>**<font style="color:rgb(64, 64, 64);">。</font>

---

### <font style="color:rgb(64, 64, 64);">一、TPS曲线的核心价值：系统健康的“心电图”</font>
#### <font style="color:rgb(64, 64, 64);">1.1 压测过程三阶段</font>
![](https://cdn.nlark.com/yuque/0/2025/png/538409/1751078218711-e8aee047-c72b-46d7-b865-1fac140d3b27.png)

#### <font style="color:rgb(64, 64, 64);">1.2 关键观测维度</font>
| **<font style="color:rgb(64, 64, 64);">曲线特征</font>** | **<font style="color:rgb(64, 64, 64);">健康信号</font>** | **<font style="color:rgb(64, 64, 64);">危险信号</font>** |
| --- | --- | --- |
| <font style="color:rgb(64, 64, 64);">上升斜率</font> | <font style="color:rgb(64, 64, 64);">线性陡峭</font> | <font style="color:rgb(64, 64, 64);">平缓/波动</font> |
| <font style="color:rgb(64, 64, 64);">稳定平台</font> | <font style="color:rgb(64, 64, 64);">平坦高位</font> | <font style="color:rgb(64, 64, 64);">锯齿状/下降</font> |
| <font style="color:rgb(64, 64, 64);">衰退形态</font> | <font style="color:rgb(64, 64, 64);">缓慢下降</font> | <font style="color:rgb(64, 64, 64);">断崖式下跌</font> |


---

### <font style="color:rgb(64, 64, 64);">二、8大经典TPS曲线解析与根因定位</font>
![](https://cdn.nlark.com/yuque/0/2025/png/538409/1751078976620-e53dd984-e20a-435c-aa31-f3224e3419dc.png)



#### <font style="color:rgb(64, 64, 64);">2.1 理想型：阶梯式上升→高位平稳</font>
```plain
TPS
  |     /¯¯¯¯¯¯¯¯
  |   ↗
  | ↗
0 +--------------> 时间
```

**<font style="color:rgb(64, 64, 64);">特征</font>**<font style="color:rgb(64, 64, 64);">：</font>

+ <font style="color:rgb(64, 64, 64);">爬坡期短（<30秒）</font>
+ <font style="color:rgb(64, 64, 64);">稳定期波动<5%  
</font>**<font style="color:rgb(64, 64, 64);">诊断</font>**<font style="color:rgb(64, 64, 64);">：</font>
+ <font style="color:rgb(64, 64, 64);">✅</font><font style="color:rgb(64, 64, 64);"> 系统设计良好</font>
+ <font style="color:rgb(64, 64, 64);">⚠️</font><font style="color:rgb(64, 64, 64);"> 需验证资源余量：</font>`**<font style="color:rgb(64, 64, 64);background-color:rgb(236, 236, 236);">free -m | awk 'NR==2{printf "内存余量:%.1fG\n", $7/1024}'</font>**`

---

#### <font style="color:rgb(64, 64, 64);">2.2 早衰型：快速上升→急速下跌</font>
```plain
TPS
  |   ▲    
  |  ↗ ↘  
  | ↗   ↘
0 +------╳---> 时间
```

**<font style="color:rgb(64, 64, 64);">根因链</font>**<font style="color:rgb(64, 64, 64);">：</font>

![](https://cdn.nlark.com/yuque/0/2025/png/538409/1751078310463-3af20e5f-e01f-478b-8a58-5f689aaf83d4.png)

**<font style="color:rgb(64, 64, 64);">定位工具</font>**<font style="color:rgb(64, 64, 64);">：</font>

```plain
# 检查连接池
watch -n 1 'curl http://localhost:8080/actuator/metrics/hikaricp.connections.active | jq .value'
```

---

#### <font style="color:rgb(64, 64, 64);">2.3 锯齿型：周期性波动</font>
```plain
TPS
  |  /¯\/¯\/¯\
  | /  \/  \/  \
0 +--------------> 时间
```

**<font style="color:rgb(64, 64, 64);">周期模式库</font>**<font style="color:rgb(64, 64, 64);">：</font>

| **<font style="color:rgb(64, 64, 64);">周期</font>** | **<font style="color:rgb(64, 64, 64);">可能原因</font>** | **<font style="color:rgb(64, 64, 64);">验证方式</font>** |
| --- | --- | --- |
| <font style="color:rgb(64, 64, 64);">10-30秒</font> | <font style="color:rgb(64, 64, 64);">Young GC</font> | `**<font style="color:rgb(64, 64, 64);background-color:rgb(236, 236, 236);">jstat -gcutil 1s</font>**` |
| <font style="color:rgb(64, 64, 64);">1-5分钟</font> | <font style="color:rgb(64, 64, 64);">缓存刷新</font> | <font style="color:rgb(64, 64, 64);">检查Redis TTL配置</font> |
| <font style="color:rgb(64, 64, 64);">无规律</font> | <font style="color:rgb(64, 64, 64);">网络抖动</font> | `**<font style="color:rgb(64, 64, 64);background-color:rgb(236, 236, 236);">mtr -r -c 60 目标IP</font>**` |


---

#### <font style="color:rgb(64, 64, 64);">2.4 平台跳跃型：多段稳定平台</font>
```plain
TPS
  |   ______
  |  ↗      ↘______
  | ↗              ↘
0 +-----------------> 时间
```

**<font style="color:rgb(64, 64, 64);">典型场景</font>**<font style="color:rgb(64, 64, 64);">：</font>

+ <font style="color:rgb(64, 64, 64);">弹性扩容触发</font>
+ <font style="color:rgb(64, 64, 64);">负载均衡策略切换  
</font>**<font style="color:rgb(64, 64, 64);">诊断步骤</font>**<font style="color:rgb(64, 64, 64);">：</font>
1. <font style="color:rgb(64, 64, 64);">检查K8s事件：</font>`**<font style="color:rgb(64, 64, 64);background-color:rgb(236, 236, 236);">kubectl get events --field-selector reason=ScalingReplicaSet</font>**`
2. <font style="color:rgb(64, 64, 64);">分析Nginx upstream变化：</font>`**<font style="color:rgb(64, 64, 64);background-color:rgb(236, 236, 236);">tail -f /var/log/nginx/upstream.log</font>**`

---

#### <font style="color:rgb(64, 64, 64);">2.5 长尾爬坡型：缓慢上升</font>
```plain
TPS
  |       ↗
  |    ↗
  | ↗
0 +--------------> 时间 (>5分钟)
```

**<font style="color:rgb(64, 64, 64);">根因四象限</font>**<font style="color:rgb(64, 64, 64);">：</font>

![](https://cdn.nlark.com/yuque/0/2025/png/538409/1751078416955-ad378ebd-e0e8-422c-9614-18b047eea9d2.png)

**<font style="color:rgb(64, 64, 64);">优化方案</font>**<font style="color:rgb(64, 64, 64);">：</font>

+ <font style="color:rgb(64, 64, 64);">预加载缓存：</font>`**<font style="color:rgb(64, 64, 64);background-color:rgb(236, 236, 236);">@PostConstruct public void preloadCache()</font>**`
+ <font style="color:rgb(64, 64, 64);">增加线程池：</font>`**<font style="color:rgb(64, 64, 64);background-color:rgb(236, 236, 236);">ExecutorService pool = Executors.newWorkStealingPool(64)</font>**`

---

#### <font style="color:rgb(64, 64, 64);">2.6 毛刺型：高位稳定但偶发骤降</font>
```plain
TPS
  |   ______↓______
  |  |      ↑     |
0 +-----------------> 时间
```

**<font style="color:rgb(64, 64, 64);">毛刺根因树</font>**<font style="color:rgb(64, 64, 64);">：</font>

![](https://cdn.nlark.com/yuque/0/2025/png/538409/1751078436849-d6e0ba07-f7df-45ec-9008-02691e3bc205.png)

---

#### <font style="color:rgb(64, 64, 64);">2.7 崩溃型：断崖式下跌</font>
```plain
TPS
  |   ▲ 
  |  ↗ ↘ 
  | ↗   ╳_________
0 +---------------> 时间
```

**<font style="color:rgb(64, 64, 64);">崩溃检查清单</font>**<font style="color:rgb(64, 64, 64);">：</font>

1. <font style="color:rgb(64, 64, 64);">进程状态：</font>`**<font style="color:rgb(64, 64, 64);background-color:rgb(236, 236, 236);">ps aux | grep -v grep | grep <应用名></font>**`
2. <font style="color:rgb(64, 64, 64);">系统日志：</font>`**<font style="color:rgb(64, 64, 64);background-color:rgb(236, 236, 236);">journalctl --since "5 min ago" | grep oom</font>**`
3. <font style="color:rgb(64, 64, 64);">线程快照：</font>`**<font style="color:rgb(64, 64, 64);background-color:rgb(236, 236, 236);">jstack -l <PID> > thread_dump.log</font>**`
4. <font style="color:rgb(64, 64, 64);">网络连接：</font>`**<font style="color:rgb(64, 64, 64);background-color:rgb(236, 236, 236);">ss -s | grep TCP</font>**`

---

#### <font style="color:rgb(64, 64, 64);">2.8 过山车型：剧烈震荡</font>
```plain
TPS
  |  /¯\    /¯\  
  | /   \__/   \ 
0 +--------------> 时间
```

**<font style="color:rgb(64, 64, 64);">自激振荡原理</font>**<font style="color:rgb(64, 64, 64);">：</font>

![](https://cdn.nlark.com/yuque/0/2025/png/538409/1751078471222-b68f4f23-cda5-4d1e-959c-c301faf48a43.png)

**<font style="color:rgb(64, 64, 64);">破解方案</font>**<font style="color:rgb(64, 64, 64);">：</font>

+ <font style="color:rgb(64, 64, 64);">固定线程池大小</font>
+ <font style="color:rgb(64, 64, 64);">添加队列缓冲：</font>`**<font style="color:rgb(64, 64, 64);background-color:rgb(236, 236, 236);">new ArrayBlockingQueue(5000)</font>**`

---

### <font style="color:rgb(64, 64, 64);">三、诊断工具箱：趋势匹配的观测利器</font>
#### <font style="color:rgb(64, 64, 64);">3.1 曲线类型识别脚本</font>
```plain
# 基于斜率变化的曲线分类
def classify_tps_curve(data):
    slopes = np.diff(data)
    # 计算波动率
    volatility = np.std(slopes) / np.mean(data[:-1])
    
    if volatility < 0.05:
        return "理想型"
    elif np.any(data == 0):
        return "崩溃型"
    elif volatility > 0.3:
        return "过山车型" if np.mean(slopes) > 0 else "锯齿型"
```

#### <font style="color:rgb(64, 64, 64);">3.2 分层诊断工具矩阵</font>
| **<font style="color:rgb(64, 64, 64);">模式</font>** | **<font style="color:rgb(64, 64, 64);">首选工具</font>** | **<font style="color:rgb(64, 64, 64);">备选工具</font>** | **<font style="color:rgb(64, 64, 64);">关键指标</font>** |
| --- | --- | --- | --- |
| <font style="color:rgb(64, 64, 64);">早衰型</font> | `**<font style="color:rgb(64, 64, 64);background-color:rgb(236, 236, 236);">jstack + jmap</font>**` | <font style="color:rgb(64, 64, 64);">Arthas</font> | <font style="color:rgb(64, 64, 64);">BLOCKED线程数/OOM错误</font> |
| <font style="color:rgb(64, 64, 64);">锯齿型</font> | `**<font style="color:rgb(64, 64, 64);background-color:rgb(236, 236, 236);">jstat + GC日志</font>**` | <font style="color:rgb(64, 64, 64);">Prometheus GC仪表盘</font> | <font style="color:rgb(64, 64, 64);">Young GC频率/GCCause</font> |
| <font style="color:rgb(64, 64, 64);">平台跳跃型</font> | <font style="color:rgb(64, 64, 64);">K8s事件日志</font> | <font style="color:rgb(64, 64, 64);">LB访问日志</font> | <font style="color:rgb(64, 64, 64);">实例数变化时间点</font> |
| <font style="color:rgb(64, 64, 64);">长尾爬坡型</font> | <font style="color:rgb(64, 64, 64);">Redis监控</font> | <font style="color:rgb(64, 64, 64);">连接池指标</font> | <font style="color:rgb(64, 64, 64);">缓存命中率/连接初始化时间</font> |
| <font style="color:rgb(64, 64, 64);">毛刺型</font> | `**<font style="color:rgb(64, 64, 64);background-color:rgb(236, 236, 236);">nstat + tcpdump</font>**` | <font style="color:rgb(64, 64, 64);">Wireshark</font> | <font style="color:rgb(64, 64, 64);">TcpRetransSegs/丢包率</font> |
| <font style="color:rgb(64, 64, 64);">崩溃型</font> | <font style="color:rgb(64, 64, 64);">系统日志 + dmesg</font> | <font style="color:rgb(64, 64, 64);">进程监控</font> | <font style="color:rgb(64, 64, 64);">OOM Killer记录</font> |
| <font style="color:rgb(64, 64, 64);">过山车型</font> | <font style="color:rgb(64, 64, 64);">线程池监控</font> | <font style="color:rgb(64, 64, 64);">队列监控</font> | <font style="color:rgb(64, 64, 64);">队列长度波动/拒绝次数</font> |




---

### <font style="color:rgb(64, 64, 64);">四、调优实战：电商平台过山车曲线治理</font>
#### <font style="color:rgb(64, 64, 64);">4.1 压测曲线对比</font>
```plain
优化前：
  TPS:  _/¯¯\__/¯¯\__/¯¯\_

优化后：
  TPS:  /¯¯¯¯¯¯¯¯¯¯¯¯\
```

#### <font style="color:rgb(64, 64, 64);">4.2 核心优化点</font>
![](https://cdn.nlark.com/yuque/0/2025/png/538409/1751078618945-7022092d-de53-4442-8b3b-80ac29ad8ef0.png)

#### <font style="color:rgb(64, 64, 64);">4.3 线程池参数模板</font>
```plain
ThreadPoolExecutor executor = new ThreadPoolExecutor(
    50, // 核心线程 (固定值)
    50, // 最大线程 (与核心相同)
    0,  // 非核心线程立即回收
    TimeUnit.MILLISECONDS,
    new ArrayBlockingQueue<>(2000), // 有限队列
    new ThreadPoolExecutor.AbortPolicy() // 拒绝策略
);
```

#### <font style="color:rgb(64, 64, 64);">4.4 效果数据</font>
| **<font style="color:rgb(64, 64, 64);">指标</font>** | **<font style="color:rgb(64, 64, 64);">优化前</font>** | **<font style="color:rgb(64, 64, 64);">优化后</font>** | **<font style="color:rgb(64, 64, 64);">提升</font>** |
| --- | --- | --- | --- |
| <font style="color:rgb(64, 64, 64);">TPS波动率</font> | <font style="color:rgb(64, 64, 64);">63%</font> | <font style="color:rgb(64, 64, 64);">8%</font> | <font style="color:rgb(64, 64, 64);">87%↓</font> |
| <font style="color:rgb(64, 64, 64);">平均响应时间</font> | <font style="color:rgb(64, 64, 64);">340ms</font> | <font style="color:rgb(64, 64, 64);">210ms</font> | <font style="color:rgb(64, 64, 64);">38%↓</font> |
| <font style="color:rgb(64, 64, 64);">错误率</font> | <font style="color:rgb(64, 64, 64);">12%</font> | <font style="color:rgb(64, 64, 64);">0.3%</font> | <font style="color:rgb(64, 64, 64);">97%↓</font> |


---

### <font style="color:rgb(64, 64, 64);">五、趋势预测：基于AI的压测曲线治理</font>
#### <font style="color:rgb(64, 64, 64);">5.1 智能分析框架</font>
![](https://cdn.nlark.com/yuque/0/2025/png/538409/1751078524545-6f462dc5-73b7-429d-892c-13fd831ef51b.png)

#### <font style="color:rgb(64, 64, 64);">5.2 预测模型输入特征</font>
```plain
features = [
    'tps_slope',         # TPS变化率
    'cpu_util',          # CPU使用率
    'mem_usage',         # 内存使用量
    'gc_pause_ratio',    # GC暂停占比
    'db_conn_wait'       # 数据库连接等待
]
```

#### <font style="color:rgb(64, 64, 64);">5.3 调优建议输出</font>
```plain
{
  "curve_type": "锯齿型",
  "confidence": 0.92,
  "root_cause": "Young GC频繁触发",
  "solutions": [
    "调整JVM参数: -XX:NewSize=512m",
    "优化对象分配: 减少短生命期对象"
  ]
}
```

---

### <font style="color:rgb(64, 64, 64);">六、黄金法则：压测曲线分析三步法</font>
**<font style="color:rgb(64, 64, 64);">1.形态识别</font>**

![](https://cdn.nlark.com/yuque/0/2025/png/538409/1751078547143-57bf0744-04ab-4e98-a2d3-2a11f2fb11c8.png)

**<font style="color:rgb(64, 64, 64);">2.分层下钻</font>**

```plain
应用层：线程栈/JVM监控
中间件：慢SQL/Redis大Key
系统层：网络丢包/IO等待
```

**<font style="color:rgb(64, 64, 64);">3.调优验证</font>**

    - <font style="color:rgb(64, 64, 64);">单点优化 → 局部重测</font>
    - <font style="color:rgb(64, 64, 64);">架构改造 → 全链路压测</font>
    - <font style="color:rgb(64, 64, 64);">建立基线 → 监控告警</font>

**<font style="color:rgb(64, 64, 64);"></font>**

**<font style="color:rgb(64, 64, 64);">原则</font>**<font style="color:rgb(64, 64, 64);">：</font>

1. **<font style="color:rgb(64, 64, 64);">从现象倒推</font>**<font style="color:rgb(64, 64, 64);">：先识别曲线模式再针对性排查</font>
2. **<font style="color:rgb(64, 64, 64);">分层验证</font>**<font style="color:rgb(64, 64, 64);">：应用→中间件→系统→网络层层下钻</font>
3. **<font style="color:rgb(64, 64, 64);">时序对齐</font>**<font style="color:rgb(64, 64, 64);">：将TPS波动与系统事件时间轴对齐</font>
4. **<font style="color:rgb(64, 64, 64);">最小化复现</font>**<font style="color:rgb(64, 64, 64);">：在预发环境注入故障验证猜想</font>

<font style="color:rgb(64, 64, 64);">  
</font>

