# xSHELL

[English](README.md) | 中文

`xSHELL` 是一个用于日常 Shell 工作的小工具集合。它把一些常用但容易忘记的命令封装成固定格式：

```bash
xSHELL <功能名> [参数]
```

例如：

```bash
xSHELL ext -e tsv -m .
xSHELL find '*.fa*' -s size -r .
xSHELL top -u USER -s mem
```

## 安装

在当前目录下执行：

```bash
chmod +x xSHELL
```

然后把当前目录加入 `PATH`，或者把脚本软链接到已经在 `PATH` 里的目录。

例如：

```bash
ln -s /path/to/xSHELL/xSHELL ~/bin/xSHELL
```

如果 `~/bin` 还没有加入 `PATH`，可以在 `~/.zshrc` 或 `~/.bashrc` 中加入：

```bash
export PATH="$HOME/bin:$PATH"
```

## 查看帮助

列出所有功能：

```bash
xSHELL list
```

查看总帮助：

```bash
xSHELL help
```

查看某个功能的帮助：

```bash
xSHELL help ext
xSHELL help find
xSHELL help path
xSHELL help size
xSHELL help top
```

## 功能名含义

当前包含这些功能：

```text
ext
find
path
size
top
```

含义如下：

| 功能名 | 含义 |
| --- | --- |
| `ext` | 根据文件后缀检查子目录，例如哪些子目录有 `tsv`、`fasta`、`fa`、`vcf` 文件，哪些没有 |
| `find` | 在目录中递归查找文件或目录，并可按名称、大小、修改时间排序 |
| `path` | 保存和打印常用路径，例如常用基因组 fasta 文件路径 |
| `size` | 查看当前目录下各个子目录的占用空间 |
| `top` | 查看进程信息，例如某个用户的任务、某个 PID、按 CPU 或内存排序 |

## ext

`ext` 用于检查当前目录下的子目录，判断它们是否包含某种后缀的文件。

查看哪些子目录有 `*.tsv` 文件：

```bash
xSHELL ext -e tsv .
```

查看哪些子目录没有 `*.fasta` 文件：

```bash
xSHELL ext -e fasta -m .
```

同时显示有和没有 `*.vcf` 文件的子目录：

```bash
xSHELL ext -e vcf -b .
```

只检查每个子目录第一层，不递归检查更深层目录：

```bash
xSHELL ext -e fa -d .
```

说明：

- `-e tsv` 或 `--ext tsv` 表示检查 `*.tsv`
- `-e fasta` 或 `--ext fasta` 表示检查 `*.fasta`
- `--has` 打印包含目标文件的子目录
- `-m` 或 `--missing` 打印不包含目标文件的子目录
- `-b` 或 `--both` 同时打印两类子目录
- `-d` 或 `--direct` 只检查每个子目录第一层
- 默认会递归检查每个子目录内部

## find

`find` 用于递归查找文件或目录，并按名称、文件大小、修改时间排序。

查找所有 `*.tsv` 文件：

```bash
xSHELL find '*.tsv' .
```

查找 fasta/fa 相关文件，并按文件大小从大到小排序：

```bash
xSHELL find '*.fa*' -s size -r .
```

查找文件名中包含 `genome` 的文件，并按修改时间从新到旧排序：

```bash
xSHELL find '*genome*' -s time -r .
```

查找名为 `results` 的目录：

```bash
xSHELL find 'results' --type d .
```

常用参数：

- `-s name` 或 `--sort name` 按名称排序，默认值
- `-s size` 或 `--sort size` 按文件大小排序
- `-s time` 或 `--sort time` 按修改时间排序
- `-r` 或 `--reverse` 反向排序，例如从大到小、从新到旧
- `--type f` 只查找文件，默认值
- `--type d` 只查找目录
- `--type a` 查找所有类型

## path

`path` 用于保存和打印常用路径。适合保存经常使用但很长的基因组、注释文件、项目目录等路径。

打印已经保存的 `ref-genome` 路径：

```bash
xSHELL path ref-genome
```

默认会包含这个路径：

```text
ref-genome    /data/reference/genome.fa
```

添加或更新一个路径：

```bash
xSHELL path add ref /data/reference/genome.fa 'reference genome fasta'
```

列出所有已保存路径：

```bash
xSHELL path ls
```

删除一个路径：

```bash
xSHELL path rm ref
```

查看路径数据库位置：

```bash
xSHELL path db
```

路径数据库默认保存在：

```text
~/.config/xSHELL/paths.tsv
```

## size

`size` 用于查看当前目录下每个子目录的空间占用。

查看当前目录下所有子目录大小：

```bash
xSHELL size .
```

按大小从大到小排序：

```bash
xSHELL size -r .
```

## top

`top` 用于查看进程快照，适合快速回答这些问题：

- 某个用户正在跑哪些任务？
- 某个 PID 是什么任务？
- 哪些任务最占 CPU？
- 哪些任务最占内存？
- 某个命令，例如 `java`、`bwa`、`blast`，是谁在跑？

查看当前进程，默认按 CPU 使用率排序：

```bash
xSHELL top
```

查看某个用户的任务：

```bash
xSHELL top -u USER
```

查看某个 PID：

```bash
xSHELL top -p 1270479
```

按内存占用排序：

```bash
xSHELL top -s mem
```

按 CPU 使用率排序，并显示前 50 行：

```bash
xSHELL top -s cpu -n 50
```

筛选命令中包含 `java` 的进程：

```bash
xSHELL top -u USER -g java
```

每 5 秒刷新一次：

```bash
xSHELL top -s mem -w 5
```

常用参数：

- `-u USER` 或 `--user USER` 只看某个用户的进程
- `-p PID` 或 `--pid PID` 只看某个 PID，也支持 `PID1,PID2`
- `-g TEXT` 或 `--grep TEXT` 只看命令中包含某段文字的进程
- `-s cpu` 或 `--sort cpu` 按 CPU 使用率排序
- `-s mem` 或 `--sort mem` 按内存百分比排序
- `-s rss` 或 `--sort rss` 按实际常驻内存大小排序
- `-s time` 或 `--sort time` 按运行时间排序
- `-n N` 或 `--limit N` 显示前 N 行
- `-w SEC` 或 `--watch SEC` 每隔 SEC 秒刷新

## 推荐别名

可以把下面的内容加入 `~/.zshrc` 或 `~/.bashrc`：

```bash
alias xs='xSHELL'
alias refgenome='xSHELL path ref-genome'
```

之后可以这样使用：

```bash
xs ext -e tsv -m .
xs find '*.fa*' -s size -r .
refgenome
```
