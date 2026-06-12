# NOT Toolbox Resource Document
## 一、使用方式
### 1. URL构建
#### (1). 主URL
https://raw.githubusercontent.com/HOE-Team/not-toolbox-resource/refs/heads/main
#### (2). 追加
**Windows**
- Windows Winget:  
  `/windows/packages-windows-winget.json`
- Windows Scoop:  
  `/windows/packages-windows-scoop.json`
- Windows Chocolatey:  
  `/windows/packages-windows-chocolatey.json`  

**Linux**
- DNF:  
  `/linux/packages-linux-dnf.json`
- Pacman:  
  `/linux/packages-linux-pacman.json`
- APT:  
  `/linux/packages-linux-apt.json`
- Nix: 暂无
- Zypper: 暂无
- Emerge: 暂无

## 二、变量
| 字段 | 类型 | 作用 | 使用场景 |
|------|------|------|----------|
| name | String | 工具显示名称，在UI中展示给用户的工具名 | ToolsScreen 中列表显示、搜索匹配、CommonPackages 中作为缓存key（小写化后） |
| description | String? | 工具描述，中文说明该工具的用途 | UI中鼠标悬停或列表项副标题显示 |
| url | String? | 官网/下载链接 | 用户点击"官网"按钮时打开浏览器跳转 |
| category | String? | 分类标签，用于工具分组 | getPackagesByCategory() 按分类分组展示，getCategories() 获取所有分类列表 |
| isProprietarySoftware | Boolean | 是否专有软件，默认false | UI中标记专有软件（如显示"专有"标签），合并时取逻辑或 |
| licenseUrl | String? | 许可证文件链接 | 开源工具显示许可证详情链接 |
| eulaUrl | String? | 最终用户许可协议链接 | 专有软件显示EULA链接（与licenseUrl互斥） |
| licenseType | String? | 许可证类型标识，如 GPL-2.0、MIT、Apache-2.0 | UI中显示许可证类型标签 |
| wingetName | String? | Winget包管理器中的包名 | getPackageNameForManager(WINGET) 返回此值，用于 winget install --silent |
| scoopName | String? | Scoop包管理器中的包名 | getPackageNameForManager(SCOOP) 返回此值，用于 scoop install |
| chocolateyName | String? | Chocolatey包管理器中的包名 | getPackageNameForManager(CHOCOLATEY) 返回此值，用于 choco install -y |
| aptName | String? | APT包管理器中的包名 | getPackageNameForManager(APT) 返回此值，用于 sudo apt-get install -y |
| dnfName | String? | DNF包管理器中的包名 | getPackageNameForManager(DNF) 返回此值，用于 sudo dnf install -y |
| pacmanName | String? | Pacman包管理器中的包名 | getPackageNameForManager(PACMAN) 返回此值，用于 sudo pacman -S --noconfirm |
| zypperName | String? | Zypper包管理器中的包名 | 代码中已定义但JSON文件尚未创建 |
| emergeName | String? | Emerge包管理器中的包名 | 代码中已定义但JSON文件尚未创建 |
| nixName | String? | Nix包管理器中的包名 | 代码中已定义但JSON文件尚未创建 |

## 三、使用示例
模拟的演示环境：ArchLinux x86-64、Python3

```python
#!/usr/bin/env python3
import json, subprocess
from urllib.request import urlopen

# 从RAW拉取
url = "https://raw.githubusercontent.com/HOE-Team/not-toolbox-resource/refs/heads/main/linux/packages-linux-pacman.json"
with urlopen(url, timeout=10) as resp:
    data = json.loads(resp.read().decode('utf-8'))
# 调用pacman安装第一个包
target = data[0]
pkg_name = target.get('pacmanName') or target['name']
cmd = f"sudo pacman -S --noconfirm {pkg_name}"

# 执行
subprocess.run(cmd.split())
```

## 四、许可协议
本仓库基于MIT协议开放，你可以在保留原作者署名的前提下，自由使用本仓库内的所有JSON文件，包括但不限于复制、修改、合并、发布、分发、再许可及商业用途。  
版权所有 ©2026 HOE Team.保留所有权利。
