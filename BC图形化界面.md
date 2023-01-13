
## VGA
BC的BGI默认为VGA模式，默认输出只支持 $640\times 480\times 16$。其中第三个数字是指16色，4位。显示效果十分不好，只能满足最基础的图形界面需求。

优点是其拥有`graphics.h`库的完全支持，绘图简单，编程效率和运行效率高。

其初始化方式如下。
```c

```

## SVGA
SVGA 拥有两种驱动方式，一是由BC提供的BGI驱动，二是手动实现驱动。

### BGI SVGA256
BC提供了SVGA256的BGI驱动，可以在程序内手动安装驱动，从而实现256色，即8位色彩深度。
使用 `graphics.h` 中的 `int far installuserdriver(char far *name, int huge (*detect) (void))` 函数可以安装BGI驱动。其中，第一个参数为BGI驱动的名称，即不包含后缀名的文件名。需要注意的是该文件必须位于当前目录下，如果需要安装其他目录的驱动文件，则必须提前切换路径到该目录下，可以使用函数 `int changedir(const char *dir)` 切换目录，其位于 `dir.h` 中。

第二个参数则需要根据需要使用的图形模式传入一个返回值为 `int`，内存模式为 `huge`，参数为空的函数指针。该函数的返回值表示所需模式，其对应关系如下。

```c
#define SVGA320x200x256 0	/* 320x200x256 Standard VGA */
#define SVGA640x400x256 1	/* 640x400x256 Svga/VESA */
#define SVGA640x480x256 2	/* 640x480x256 Svga/VESA */
#define SVGA800x600x256 3	/* 800x600x256 Svga/VESA */
#define SVGA1024x768x256 4	/* 1024x768x256 Svga/VESA */
#define SVGA640x350x256 5	/* 640x350x256 Svga/VESA */
#define SVGA1280x1024x256 6 /* 1280x1024x256 Svga/VESA */
```

由于我们使用的是BorlandC++，没有办法使用匿名函数等新特性，所以这个配置函数需要在外部定义。

综上，如果我需要安装位于 `C:\DRIVER` 路径下的驱动 `SVGA256.BGI`，并使用$1024\times768\times256$的显示模式，我需要进行如下设置。

```c
int huge detectSVGA256(void){
	return SVGA1024x768x256;
}

int initsvga256(void){
	int G_driver = DETECT, G_model;
	char Gr_error;
    chdir("C:\\DRIVER\\");
	installuserdriver("SVGA256", detectSVGA256);
	initgraph(&G_driver, &G_model, "");
	Gr_error = graphresult();
	if (Gr_error != grOk){
		return 0;
	}
	return 1;
}
```

接下来，你可以使用`putpixel`等函数进行绘图。


### 手动实现 SVGA256



### 手动实现 SVGA64K
需注意，SVGA64k有严重的运行效率问题，请在知悉这些问题之后使用。
