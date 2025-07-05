<font style="color:rgb(64, 64, 64);">在移动应用测试领域，ADB 是连接开发机与终端设备的桥梁，掌握它意味着掌控了测试自动化的底层钥匙。</font>

### <font style="color:rgb(64, 64, 64);">一、ADB 基础：移动测试工程师的必备工具</font>
**<font style="color:rgb(64, 64, 64);">ADB（Android Debug Bridge）</font>**<font style="color:rgb(64, 64, 64);"> </font><font style="color:rgb(64, 64, 64);">是 Android SDK 中一个强大的命令行工具，它建立了 PC 与 Android 设备（或模拟器）之间的通信桥梁。对于测试工程师而言，ADB 是进行深度测试、问题排查、自动化脚本开发的</font>**<font style="color:rgb(64, 64, 64);">核心基础设施</font>**<font style="color:rgb(64, 64, 64);">。</font>

**<font style="color:rgb(64, 64, 64);">典型应用场景</font>**<font style="color:rgb(64, 64, 64);">：</font>

+ <font style="color:rgb(64, 64, 64);">应用安装/卸载/调试</font>
+ <font style="color:rgb(64, 64, 64);">设备日志实时抓取与分析</font>
+ <font style="color:rgb(64, 64, 64);">文件双向传输与管理</font>
+ <font style="color:rgb(64, 64, 64);">设备信息深度获取</font>
+ <font style="color:rgb(64, 64, 64);">自动化测试脚本执行基础</font>
+ <font style="color:rgb(64, 64, 64);">性能数据采集（CPU、内存等）</font>

### <font style="color:rgb(64, 64, 64);">二、环境搭建：快速配置 ADB 工作环境</font>
**<font style="color:rgb(64, 64, 64);">1.安装 Android SDK Platform-Tools</font>**<font style="color:rgb(64, 64, 64);">：</font>

```plain
# Mac/Linux (使用 Homebrew)
brew install android-platform-tools

# Windows 用户从官网下载 ZIP 并配置环境变量：
https://developer.android.com/studio/releases/platform-tools
```

**<font style="color:rgb(64, 64, 64);">2.配置环境变量（关键步骤）</font>**<font style="color:rgb(64, 64, 64);">：</font>

```plain
# Linux/Mac 在 ~/.bashrc 或 ~/.zshrc 中添加：
export PATH=$PATH:/path/to/platform-tools

# Windows：系统属性 -> 高级 -> 环境变量 -> Path 添加路径
```

**<font style="color:rgb(64, 64, 64);">3.连接设备与授权</font>**<font style="color:rgb(64, 64, 64);">：</font>

```plain
adb devices  # 查看设备列表
# 首次连接时设备端需点击"允许USB调试"
```

**<font style="color:rgb(64, 64, 64);">验证安装</font>**<font style="color:rgb(64, 64, 64);">：</font>

1. <font style="color:rgb(82, 82, 82);background-color:rgb(250, 250, 250);">bash</font><font style="color:rgb(82, 82, 82);background-color:rgba(0, 0, 0, 0);">复制</font><font style="color:rgb(82, 82, 82);background-color:rgba(0, 0, 0, 0);">下载</font><font style="color:rgb(73, 73, 73);background-color:rgb(250, 250, 250);">adb --version  # 输出版本信息即成功</font>

### <font style="color:rgb(64, 64, 64);">三、ADB 核心命令手册（测试场景精选）</font>
| **<font style="color:rgb(64, 64, 64);">命令类别</font>** | **<font style="color:rgb(64, 64, 64);">命令示例</font>** | **<font style="color:rgb(64, 64, 64);">测试应用场景</font>** |
| --- | --- | --- |
| **<font style="color:rgb(64, 64, 64);">设备管理</font>** | `**<font style="color:rgb(64, 64, 64);background-color:rgb(236, 236, 236);">adb devices -l</font>**` | <font style="color:rgb(64, 64, 64);">获取设备详细型号信息</font> |
| | `**<font style="color:rgb(64, 64, 64);background-color:rgb(236, 236, 236);">adb reboot</font>**` | <font style="color:rgb(64, 64, 64);">设备重启（异常恢复）</font> |
| **<font style="color:rgb(64, 64, 64);">应用管理</font>** | `**<font style="color:rgb(64, 64, 64);background-color:rgb(236, 236, 236);">adb install -t app.apk</font>**` | <font style="color:rgb(64, 64, 64);">强制安装测试包（兼容性测试）</font> |
| | `**<font style="color:rgb(64, 64, 64);background-color:rgb(236, 236, 236);">adb uninstall com.example</font>**` | <font style="color:rgb(64, 64, 64);">彻底卸载应用（清理环境）</font> |
| **<font style="color:rgb(64, 64, 64);">文件操作</font>** | `**<font style="color:rgb(64, 64, 64);background-color:rgb(236, 236, 236);">adb push local /sdcard/</font>**` | <font style="color:rgb(64, 64, 64);">推送测试数据到设备</font> |
| | `**<font style="color:rgb(64, 64, 64);background-color:rgb(236, 236, 236);">adb pull /sdcard/logs ./</font>**` | <font style="color:rgb(64, 64, 64);">拉取设备日志进行分析</font> |
| **<font style="color:rgb(64, 64, 64);">日志抓取</font>** | `**<font style="color:rgb(64, 64, 64);background-color:rgb(236, 236, 236);">adb logcat -v time > log.txt</font>**` | <font style="color:rgb(64, 64, 64);">带时间戳记录完整日志</font> |
| | `**<font style="color:rgb(64, 64, 64);background-color:rgb(236, 236, 236);">adb logcat *:E</font>**` | <font style="color:rgb(64, 64, 64);">仅抓取错误日志（快速定位崩溃）</font> |
| **<font style="color:rgb(64, 64, 64);">系统操作</font>** | `**<font style="color:rgb(64, 64, 64);background-color:rgb(236, 236, 236);">adb shell input tap 500 500</font>**` | <font style="color:rgb(64, 64, 64);">模拟屏幕点击（基础自动化）</font> |
| | `**<font style="color:rgb(64, 64, 64);background-color:rgb(236, 236, 236);">adb shell am start -n com.example/.MainActivity</font>**` | <font style="color:rgb(64, 64, 64);">启动特定Activity</font> |
| **<font style="color:rgb(64, 64, 64);">信息获取</font>** | `**<font style="color:rgb(64, 64, 64);background-color:rgb(236, 236, 236);">adb shell dumpsys battery</font>**` | <font style="color:rgb(64, 64, 64);">获取电池状态（功耗测试）</font> |
| | `**<font style="color:rgb(64, 64, 64);background-color:rgb(236, 236, 236);">adb shell getprop ro.build.version.release</font>**` | <font style="color:rgb(64, 64, 64);">获取系统版本号</font> |


### <font style="color:rgb(64, 64, 64);">四、测试实战：ADB 在移动测试中的高阶应用</font>
#### <font style="color:rgb(64, 64, 64);">1. 精准日志过滤（定位崩溃）</font>
<font style="color:rgb(73, 73, 73);background-color:rgb(250, 250, 250);">adb logcat --pid=$(adb shell pidof -s com.example.app) | grep "FATAL"</font>

<font style="color:rgb(73, 73, 73);background-color:rgb(250, 250, 250);"></font>

+ <font style="color:rgb(64, 64, 64);">通过</font><font style="color:rgb(64, 64, 64);"> </font>`**<font style="color:rgb(64, 64, 64);background-color:rgb(236, 236, 236);">pidof</font>**`<font style="color:rgb(64, 64, 64);"> </font><font style="color:rgb(64, 64, 64);">获取目标进程 PID</font>
+ <font style="color:rgb(64, 64, 64);">过滤致命错误日志，快速定位崩溃点</font>

#### <font style="color:rgb(64, 64, 64);">2. 多设备并行操作</font>
```plain
# 指定设备执行命令
adb -s emulator-5554 install app.apk
adb -s 5EF4S18B30004522 uninstall com.test
```

+ <font style="color:rgb(64, 64, 64);">在同时连接多台设备时精确控制目标设备</font>

#### <font style="color:rgb(64, 64, 64);">3. 自动化 Monkey 压力测试</font>
<font style="color:rgb(73, 73, 73);background-color:rgb(250, 250, 250);">adb shell monkey -p com.example.app --throttle 100 -s 123 --ignore-crashes 10000</font>

<font style="color:rgb(73, 73, 73);background-color:rgb(250, 250, 250);"></font>

+ `**<font style="color:rgb(64, 64, 64);background-color:rgb(236, 236, 236);">-p</font>**`<font style="color:rgb(64, 64, 64);">：指定测试包名</font>
+ `**<font style="color:rgb(64, 64, 64);background-color:rgb(236, 236, 236);">--throttle</font>**`<font style="color:rgb(64, 64, 64);">：事件延迟（ms）</font>
+ `**<font style="color:rgb(64, 64, 64);background-color:rgb(236, 236, 236);">-s</font>**`<font style="color:rgb(64, 64, 64);">：随机种子（复现问题）</font>
+ `**<font style="color:rgb(64, 64, 64);background-color:rgb(236, 236, 236);">--ignore-crashes</font>**`<font style="color:rgb(64, 64, 64);">：崩溃后继续执行</font>

#### <font style="color:rgb(64, 64, 64);">4. 屏幕录制与性能分析</font>
```plain
adb shell screenrecord /sdcard/bug.mp4 &  # 后台录制
adb shell top -d 1 | grep com.example > perf.log  # 监控性能
adb pull /sdcard/bug.mp4  # 获取录制文件
```

+ <font style="color:rgb(64, 64, 64);">录制用户操作视频</font>
+ <font style="color:rgb(64, 64, 64);">实时记录 CPU/内存占用</font>
+ <font style="color:rgb(64, 64, 64);">复现性能问题与界面异常</font>

#### <font style="color:rgb(64, 64, 64);">5. 深度权限管理（Android 6.0+）</font>
```plain
adb shell pm grant com.example android.permission.ACCESS_FINE_LOCATION
adb shell pm revoke com.example android.permission.CAMERA
```

+ <font style="color:rgb(64, 64, 64);">动态授予/撤销运行时权限</font>
+ <font style="color:rgb(64, 64, 64);">验证权限逻辑是否正常</font>

### <font style="color:rgb(64, 64, 64);">五、调试技巧：常见问题解决方案</font>
**<font style="color:rgb(64, 64, 64);">1.设备未授权</font>**<font style="color:rgb(64, 64, 64);">：</font>

    - <font style="color:rgb(64, 64, 64);">检查 USB 调试是否开启</font>
    - <font style="color:rgb(64, 64, 64);">重新插拔数据线并确认授权弹窗</font>
    - <font style="color:rgb(64, 64, 64);">执行</font><font style="color:rgb(64, 64, 64);"> </font>`**<font style="color:rgb(64, 64, 64);background-color:rgb(236, 236, 236);">adb kill-server && adb start-server</font>**`

**<font style="color:rgb(64, 64, 64);">2.端口占用冲突</font>**<font style="color:rgb(64, 64, 64);">：</font>

1. <font style="color:rgb(82, 82, 82);background-color:rgb(250, 250, 250);">bash</font><font style="color:rgb(82, 82, 82);background-color:rgba(0, 0, 0, 0);">复制</font><font style="color:rgb(82, 82, 82);background-color:rgba(0, 0, 0, 0);">下载</font>

```plain
netstat -ano | findstr 5037  # Windows 查找占用进程
adb -P 5038 start-server    # 指定新端口
```

**<font style="color:rgb(64, 64, 64);">3.文件传输权限不足</font>**<font style="color:rgb(64, 64, 64);">：</font>

```plain
adb root          # 获取 root 权限（需设备支持）
adb disable-verity # 关闭验证（开发设备）
adb remount       # 重新挂载系统分区为可写
```

### <font style="color:rgb(64, 64, 64);">六、ADB 与现代测试框架集成</font>
<font style="color:rgb(64, 64, 64);">ADB 作为底层引擎，可与主流测试框架无缝集成：</font>

**<font style="color:rgb(64, 64, 64);">1.Appium</font>**<font style="color:rgb(64, 64, 64);">：通过 </font>`**<font style="color:rgb(64, 64, 64);background-color:rgb(236, 236, 236);">adb</font>**`<font style="color:rgb(64, 64, 64);"> 驱动实现跨平台自动化</font>

```plain
// Java 示例：使用 ADB 获取设备信息
String udid = driver.getCapabilities().getCapability("udid").toString();
String androidVersion = executeCommand("adb -s " + udid + " shell getprop ro.build.version.release");
```

**<font style="color:rgb(64, 64, 64);">2.Python + ADB</font>**<font style="color:rgb(64, 64, 64);">：快速构建轻量自动化脚本</font>

```plain
import subprocess
def tap(x, y, udid):
    subprocess.run(f"adb -s {udid} shell input tap {x} {y}", shell=True)
```

**<font style="color:rgb(64, 64, 64);">3.持续集成（Jenkins）</font>**<font style="color:rgb(64, 64, 64);">：自动安装 APK 并运行测试</font>

```plain
pipeline {
  agent any
  stages {
    stage('Install APK') {
      steps {
        sh 'adb -s emulator-5554 install app/build/outputs/apk/debug/app-debug.apk'
      }
    }
  }
}
```

---

**<font style="color:rgb(64, 64, 64);">ADB 命令速查表（测试工程师版）</font>**

```plain
# 性能监控
adb shell dumpsys meminfo <package>  # 内存详情
adb shell cat /proc/cpuinfo          # CPU信息
adb shell dumpsys gfxinfo <package>  # 渲染性能

# 快速调试
adb shell settings put global window_animation_scale 0  # 关闭动画（加速测试）
adb shell wm size 1080x1920         # 修改分辨率
adb shell setprop debug.layout true  # 显示布局边界

# 高效截图/录屏
adb exec-out screencap -p > screen.png  # 直接输出到文件
adb shell screenrecord --bit-rate 4000000 /sdcard/demo.mp4  # 高质量录制
```

**<font style="color:rgb(64, 64, 64);">最佳实践提示</font>**<font style="color:rgb(64, 64, 64);">：将常用 ADB 操作封装成 Shell 脚本或 Python 函数库，可提升日常测试效率 50% 以上。例如创建</font><font style="color:rgb(64, 64, 64);"> </font>`**<font style="color:rgb(64, 64, 64);background-color:rgb(236, 236, 236);">device_utils.sh</font>**`<font style="color:rgb(64, 64, 64);"> </font><font style="color:rgb(64, 64, 64);">包含日志清理、应用安装、性能快照等一键操作。</font>

<font style="color:rgb(64, 64, 64);">掌握 ADB 不仅提升测试效率，更能深入理解 Android 系统运行机制。随着测试任务复杂度提升，这些底层技能将成为你解决疑难杂症的终极武器。</font>

