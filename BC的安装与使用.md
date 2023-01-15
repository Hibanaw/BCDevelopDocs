Borland C++ 是 Borland 公司在 MSDOS 下的 C/C++ 开发工具。由于历史发展进程，BC 是一个16位 DOS 应用。在现代操作系统中，你需要一个 DOSBOX 来模拟 DOS 系统。

## 安装

### 对于 HUST AIA 学生

你可能从老师或者学长那里得到了一个完整的压缩包，其中包含 DOSBOX 和 BC 本体，请检查确保其结构如下。

```
BC31/
├── DISK_C/
│   ├── BORLANDC/
│   └── ...
├── dosbox/
│   ├── BC31.conf
│   ├── dosbox.exe
│   └── ...
└── Borland C++ 3.1.vbs
```

**请不要修改这些文件之间的相对位置**。你需要让文件目录**保持这个结构**，将其**完整地**解压到你需要的位置。而后，运行 `Borland C++ 3.1.vbs` ，这个脚本会帮助你打开 DOSBOX，并导入配置文件，而后你就会看到 BC 的界面了。

### 配置 DOSBOX

#### 模拟C盘

DOSBOX 是是一个模拟器，需要一个 `disk_c` 目录来模拟 DOS 系统的C盘。你需要将BC的完整安装目录 `BORLANDC` 放置在 `disc_c` 下，文件夹之后会被映射到 DOS 系统的 `C:\BORLANDC`。

#### 直接启动 BC
此时，如果你直接打开 DOSBOX，你会发现其处于 shell 模式。你需要挂载C盘，设置 `path` 环境变量，设置当前目录，并启动BC。

这里以 `disk_c` 位于当前目录下，进入 DOS 后需要切换到 `C:\PRJ\`目录为例。请依次输入如下命令。

```
mount C ..\DISK_C
path C:\BORLANDC\BIN
C:
cd C:\PRJ
BC
```

#### 配置文件
DOSBOX 作为模拟器，所有对模拟相关的配置，存储在一个 `.conf` 文件内。

DOSBOX 内有一个 `dosbox.conf` 文件，里面储存了默认配置和相关注释。我们不要直接修改这个文件，而是将其复制并重命名，这里为 `bc31.conf` 为例。

##### 修改分辨率

默认情况下，DOSBOX 中的 BC 运行在 $640\times 480$ 的窗口中。在高分屏盛行的今天阅读起来会存在困难。

配置文件中 `[SDL]` 一项存在 `fullresolution` 和 `windowresolution` 参数，可以修改 DOSBOX 的默认分辨率。需注意，你需要更改 `output` 为 `opengl` 模式才可使其生效。

这里以窗口大小 $1920\times 1080$，全屏大小 $2560\times 1600$ 为例，我需要修改配置如下。
```
fullresolution=2560x1600
windowresolution=1920x1080
output=opengl
```

##### 配置默认启动命令

默认命令位于 `[autoexec]` 项下，这些命令会在 DOSBOX 启动后直接运行，减少重复性工作。与上方[直接启动 BC](#直接启动-bc) 相同，你可以添加如下内容。

```
mount C ..\DISK_C
path C:\BORLANDC\BIN
C:
cd C:\PRJ
BC
exit
```

#### 使用脚本启动
你可以使用各种类型的 script 通过 shell 来运行 DOSBOX 并导入配置文件。这里以脚本位于 `dosbox` 文件夹上一级目录，配置文件为`bc31.conf`，不显示终端为例。

##### VBS

```vb
Set WshShell = CreateObject("Wscript.Shell")
WshShell.CurrentDirectory="dosbox"
WshShell.Run "dosbox.exe -conf bc31.conf -noconsole"
```

##### CMD

```shell
cd dosbox
.\dosbox.exe -conf bc31.conf -noconsole
```
## 使用

