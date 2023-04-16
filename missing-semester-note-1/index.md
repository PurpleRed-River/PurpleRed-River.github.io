# 计算机教育中缺失的一课 笔记一




# 前言

2020 MIT公开课:The Missing Semester of Your CS Education(计算机教育中缺失的一课)

&gt; 官方 github 仓库:https://github.com/missing-semester/missing-semester
&gt;
&gt; 2020 讲义:https://missing.csail.mit.edu/
&gt;
&gt; 2020 中文讲义:https://missing-semester-cn.github.io/
&gt;
&gt; 2019 讲义:https://missing.csail.mit.edu/2019/

感谢课程制作者和 [刘黑黑a](https://space.bilibili.com/518734451) 的翻译上传。

我的笔记基本是课程内容和讲义的二次整理，方便自己查看。

# Shell 工具和脚本

Shell 基于**空格**分割命令并进行解析。如果传递的参数中包含空格，使用单引号，双引号，或使用**转义符号 **`\`** **。
在 Shell 中执行 `date` 或 `echo` 时，Shell 通过**环境变量**来找到程序并执行。也就是说 Shell 是一个编程环境，它实际具备变量、条件、循环和函数等。

&gt; Shell 查看环境变量 `echo $PATH`。
&gt; 查看程序路径 `which echo`。

Shell 中的路径是一组被分割的目录，在 Linux 和 macOS 上使用 / 分割，而在Windows上是 \。

&gt; `pwd` 获取当前目录路径。
&gt; `cd` 切换目录，`.` 当前目录， `..` 上级目录。

&gt; `ls` 打印当前目录下的文件
&gt; `mv` 用于重命名或移动文件
&gt; `cp` 拷贝文件
&gt; `mkdir` 新建文件夹

在 Shell 中，程序有两个主要的“流”：它们的输入流和输出流。
最简单的重定向是 `&lt;` 和 `&gt;`。还可以使用 `&gt;&gt;` 来向一个文件追加内容，使用管道 `|` 操作符，我们能够更好的利用文件重定向。
在 Bash 中为变量赋值的语法是 `foo=bar`，访问变量中存储的数值，其语法为 `$foo`。 
Bash 中的字符串通过 `&#39;` 和 `&#34;` 分隔符来定义，但是它们的含义并不相同。

&gt; `&#39;` 定义的字符串为原义字符串，其中的**变量不会被转义**
&gt; `&#34;` 定义的字符串会将变量值进行替换。

Bash 支持 `if`, `case`, `while` 和 `for` 这些控制流关键字。
Bash 支持**函数**，它可以接受参数并基于参数进行操作。与其他脚本语言不同的是，bash使用了很多特殊的变量来表示参数、错误代码和相关变量。下面是列举来其中一些变量，更完整的列表可以参考 [高级 Bash 脚本编写指南：第三章特殊字符](https://www.tldp.org/LDP/abs/html/special-chars.html)。

&gt; `$0` - 脚本名
&gt; `$1` 到 `$9` - 脚本的参数。 `$1` 是第一个参数，依此类推。
&gt; `$@` - 所有参数
&gt; `$#` - 参数个数
&gt; `$?` - 前一个命令的返回值
&gt; `$$` - 当前脚本的进程识别码
&gt; `!!` - 完整的上一条命令，包括参数。常见应用：当你因为权限不足执行命令失败时，可以使用 sudo !!再尝试一次。
&gt; `$_` - 上一条命令的最后一个参数。如果你正在使用的是交互式 shell，你可以通过按下 Esc 之后键入 . 来获取这个值。

命令通常使用 `STDOUT` 来返回**输出值**，使用 `STDERR` 来返回错误及**错误码**。 返回码或退出状态是脚本/命令之间交流执行状态的方式。返回值0表示正常执行，其他所有非0的返回值都表示有错误发生。
退出码可以搭配 `&amp;&amp;`（与操作符）和 `||`（或操作符）使用，用来进行条件判断，决定是否执行其他程序。同一行的多个命令可以用 `;` 分隔。程序 `true` 的返回码永远是 `0`，`false` 的返回码永远是 `1`。
另一个常见的模式是以变量的形式获取一个命令的输出，这可以通过**命令替换**（command substitution）实现。 `$(CMD)` 这样的方式来执行CMD 这个命令时，它的输出结果会替换掉 `$(CMD)`。
在 `Bash` 中进行比较时，尽量使用双方括号 `[[ ]]` 而不是单方括号 `[ ]`

&gt; 通配符 - 当你想要利用通配符进行匹配时，你可以分别使用 `?` 和 `*` 来匹配**一个**或**任意个字符**。
&gt; 花括号 `{}` - 当你有一系列的指令，其中包含一段公共子串时，可以用花括号来自动展开这些命令。

shell函数和脚本有如下一些不同点：

&gt; 函数只能与shell使用相同的语言，脚本可以使用任意语言。因此在脚本中包含 shebang 是很重要的。
&gt; 函数仅在定义时被加载，脚本会在每次被执行时加载。这让函数的加载比脚本略快一些，但每次修改函数定义，都要重新加载一次。
&gt; 函数会在当前的shell环境中执行，脚本会在单独的进程中执行。因此，函数可以对环境变量进行更改，比如改变当前工作目录，脚本则不行。脚本需要使用 export 将环境变量导出，并将值传递给环境变量。
&gt; 与其他程序语言一样，函数可以提高代码模块性、代码复用性并创建清晰性的结构。shell脚本中往往也会包含它们自己的函数定义。

## 命令行基本工具

#####  [shellcheck](https://github.com/koalaman/shellcheck) 定位sh/bash脚本中的错误。

#####  [TLDR pages](https://tldr.sh/) 它提供了一些命令案例，帮助快速找到正确的选项。

#####  [find](https://man7.org/linux/man-pages/man1/find.1.html) 它是 shell 上用于查找文件的绝佳工具。

```bash
# 查找所有名称为src的文件夹
find . -name src -type d
# 查找所有文件夹路径中包含test的python文件
find . -path &#39;*/test/*.py&#39; -type f
# 查找前一天修改的所有文件
find . -mtime -1
# 查找所有大小在500k至10M的tar.gz文件
find . -size &#43;500k -size -10M -name &#39;*.tar.gz&#39;
# 删除全部扩展名为.tmp 的文件
find . -name &#39;*.tmp&#39; -exec rm {} \;
# 查找全部的 PNG 文件并将其转换为 JPG
find . -name &#39;*.png&#39; -exec convert {} {}.jpg \;
```

##### [fd](https://github.com/sharkdp/fd) 就是一个更简单、更快速、更友好的查找程序，它可以用来作为find的替代品。输出着色、默认支持正则匹配、支持unicode并且我认为它的语法更符合直觉。

##### [locate](https://man7.org/linux/man-pages/man1/locate.1.html) 使用一个由 updatedb负责更新的数据库，通过编译索引或建立数据库的方式来实现更加快速地搜索。在大多数系统中 updatedb 都会通过 cron 每日更新。find 和类似的工具可以通过别的属性比如文件大小、修改时间或是权限来查找文件，locate则只能通过文件名。

##### grep 有很多选项，这也使它成为一个非常全能的工具。其中经常使用的有 -C ：获取查找结果的上下文（Context）；-v 将对结果进行反选（Invert），也就是输出不匹配的结果。举例来说， grep -C 5 会输出匹配结果前后五行。当需要搜索大量文件的时候，使用 -R 会递归地进入子目录并搜索所有的文本文件。

#####  [ack](https://beyondgrep.com/), [ag](https://github.com/ggreer/the_silver_searcher) 和 [rg](https://github.com/BurntSushi/ripgrep) 都是 grep 的替代品，比较常用的是 ripgrep (rg) ，速度快，而且用法符合直觉。

```bash
# 查找所有使用了 requests 库的文件
rg -t py &#39;import requests&#39;
# 查找所有没有写 shebang 的文件（包含隐藏文件）
rg -u --files-without-match &#34;^#!&#34;
# 查找所有的foo字符串，并打印其之后的5行
rg foo -A 5
# 打印匹配的统计信息（匹配的行和文件的数量）
rg --stats PATTERN
```

##### history 命令允许您以程序员的方式来访问shell中输入的历史命令。

对于大多数的 Shell 来说，您可以使用 `Ctrl&#43;R` 对命令历史记录进行回溯搜索。敲 `Ctrl&#43;R` 后您可以输入子串来进行匹配，查找历史命令行。zsh 支持直接方向键上、下，同时 zsh 还支持基于历史的自动补全，非常方便。

&gt; 你可以修改 shell history 的行为，例如，如果在命令的开头加上一个空格，它就不会被加进 Shell 记录中。当你输入包含密码或是其他敏感信息的命令时会用到这一特性。 为此你需要在 `.bashrc` 中添加 `HISTCONTROL=ignorespace` 或者向 `.zshrc` 添加 `setopt HIST_IGNORE_SPACE`。 如果你不小心忘了在前面加空格，可以通过编辑。`bash_history` 或 `.zhistory` 来手动地从历史记录中移除那一项。

##### fzf 是一个通用对模糊查找工具，它可以和很多命令一起使用。

##### [fasd](https://github.com/clvv/fasd)和 [autojump](https://github.com/wting/autojump) 这两个工具来查找最常用或最近使用的文件和目录。

Fasd 基于 frecency 对文件和文件排序，也就是说它会同时针对频率（frequency）和时效（recency）进行排序。默认情况下，fasd使用命令 z 帮助我们快速切换到最常访问的目录。
还有一些更复杂的工具可以用来概览目录结构，例如 [tree](https://linux.die.net/man/1/tree), [broot](https://github.com/Canop/broot) 或更加完整的文件管理器，例如 [nnn](https://github.com/jarun/nnn) 或 [ranger](https://github.com/ranger/ranger)。

#### [xargs](https://www.runoob.com/linux/linux-comm-xargs.html) 

命令传递参数的一个过滤器，也是组合多个命令的一个工具。它能够捕获一个命令的输出，然后传递给另外一个命令。有时候您要利用数据整理技术从一长串列表里找出你所需要安装或移除的东西。我们之前讨论的相关技术配合 xargs 即可实现。之所以能用到这个命令，关键是由于很多命令不支持|管道来传递参数，而日常工作中有有这个必要，所以就有了 xargs 命令。
命令格式 `somecommand |xargs -item  command`

## 数据分析相关工具

#### less 文件分页器

 `less` 为我们创建来一个文件分页器，使我们可以通过翻页的方式浏览较长的文本。

#### sed 流编辑器

`sed` 是一个基于文本编辑器 ed 构建的”流编辑器” 。在 `sed` 中，可以利用一些简短的命令来修改文件，而不是直接操作文件的内容。最常用的是 `s` 替换命令。`sed` 可以搭配其他命令，完成一些数据的处理。例如 `cat server.log | grep sshd | grep &#34;Disconnected from&#34; | sed &#39;s/.*Disconnected from //&#39;` 通过 `grep` 过滤最后使用 `sed` 匹配正则表达式完成了一次输出。
`sed` 还可以做很多各种各样有趣的事情，例如文本注入：(使用 `i` 命令)，打印特定的行 (使用 `p` 命令)，基于索引选择特定行等等。
关于 `sed` 的更多使用，可以通过 [Sed 备忘清单](https://wangchujiang.com/reference/docs/sed.html) 了解。

#### 正则表达式（RegEX）

正则表达式非常常见也非常有用，值得您花些时间去理解它。

&gt; `.` 除换行符之外的”任意单个字符”
&gt; `*` 匹配前面字符零次或多次
&gt; `&#43;` 匹配前面字符一次或多次
&gt; `[abc]` 匹配 a, b 和 c 中的任意一个
&gt; `(RX1|RX2)` 任何能够匹配RX1 或 RX2的结果
&gt; `^` 行首
&gt; `$` 行尾

`sed` 使用正则表达式有些时候是比较奇怪的，它需要你在这些模式前添加 `\` 才能使其具有特殊含义。或者，您也可以添加 `-E` 选项来支持这些匹配。
正则表达式是出了名的难以写对，但是它仍然会是您强大的常备工具之一。我在此不准备写更多关于正则表达式的用例。下面列出来一些非常有用的网站。

##### [正则表达式在线调试工具regex debugger](https://regex101.com/)

##### [RegExp Example 正则实例大全](https://wangchujiang.com/regexp-example/)

##### [RegEX 备忘清单](https://wangchujiang.com/reference/docs/regex.html)

#### awk 用于文本处理的编程语言

&gt; Awk、sed与grep，俗称Linux下的三剑客，它们之间有很多相似点，但是同样也各有各的特色，相似的地方是它们都可以匹配文本，其中sed和awk还可以用于文本编辑，而grep则不具备这个功用。sed是一种非交互式且面向字符流的编辑器（a &#34;non-interactive&#34; stream-oriented editor），而awk则是一门模式匹配的编程语言，因为它的主要功能是用于匹配文本并处理，同时它有一些编程语言才有的语法，例如函数、分支循环语句、变量等等，当然比起我们常见的编程语言，Awk相对比较简单。

语法 `awk [选项参数] &#39;script&#39; var=value file(s)` 或 `awk [选项参数] -f scriptfile var=value file(s)`，例子：每行按空格或TAB分割，输出文本中的1、4项 `awk &#39;{print $1,$4}&#39; log.txt`。
在代码块中，`$0` 表示整行的内容，`$1` 到 `$n` 为一行中的 n 个区域，区域的分割基于 awk 的域分隔符（默认是空格，可以通过 `-F` 来修改）。
更多内容可以看 [菜鸟教程](https://www.runoob.com/linux/linux-comm-awk.html) 和 [Awk 备忘清单](https://wangchujiang.com/reference/docs/awk.html)。

#### [sort 数据排序](https://www.runoob.com/linux/linux-comm-sort.html)、[uniq 次数统计](https://www.runoob.com/linux/linux-comm-uniq.html)、[head 查看开头](https://www.runoob.com/linux/linux-comm-head.html)、[tail 查看内容](https://www.runoob.com/linux/linux-comm-tail.html)、[paste 合并列](https://www.runoob.com/linux/linux-comm-paste.html)、[bc 计算器](https://www.runoob.com/linux/linux-comm-bc.html)

```bash
ssh myserver journalctl
 | grep sshd # 筛选
 | grep &#34;Disconnected from&#34;
 | sed -E &#39;s/.*Disconnected from (invalid |authenticating )?user (.*) [^ ]&#43; port [0-9]&#43;( \[preauth\])?$/\2/&#39;
 | sort | uniq -c 
 # sort 会对其输入数据进行排序。
 # uniq -c 会把连续出现的行折叠为一行并使用出现次数作为前缀。
 # 这句最终是按照出现次数排序，过滤出最常出现的用户名
 | sort -nk1,1 | tail -n10
 # sort -n 会按照数字顺序对输入进行排序
 # 默认情况下是按照字典序排序 -k1,1 则表示“仅基于以空格分割的第一列进行排序”
 # n 部分表示“仅排序到第n个部分”，默认情况是到行尾。
 # tail -n10 表示从尾部显示十行，也就是次数多的
 | awk &#39;{print $2}&#39; | paste -sd,
 # awk 对于每一行文本，打印其第二个部分，也就是用户名。
 # paste 以 &#39;,&#39; 分隔，将用户名放到一行
```

```bash
| paste -sd&#43; | bc -l
# paste 将一列数字用 &#39;&#43;&#39; 分隔放到一行，然后 bc 执行
```

#### R语言

R 也是一种编程语言，它非常适合被用来进行**数据分析**和**绘制图表**。这里我们不会讲的特别详细， 您只需要知道summary 可以打印某个向量的统计结果。我们将输入的一系列数据存放在一个向量后，利用R语言就可以得到我们想要的统计数据。

#### gnuplot 二维曲线绘图工具

#### 整理二进制数据

虽然到目前为止我们的讨论都是基于文本数据，但对于二进制文件其实同样有用。例如我们可以用 ffmpeg 从相机中捕获一张图片，将其转换成灰度图后通过SSH将压缩后的文件发送到远端服务器，并在那里解压、存档并显示。

```bash
ffmpeg -loglevel panic -i /dev/video0 -frames 1 -f image2 -
 | convert - -colorspace gray -
 | gzip
 | ssh mymachine &#39;gzip -d | tee copy.jpg | env DISPLAY=:0 feh -&#39;
```

# 编辑器（Vim）

写作和写代码其实是两项非常不同的活动。当我们编程的时候，会经常在文件间进行切换、阅读、浏览和修改代码，而不是连续编写一大段的文字。因此代码编辑器和文本编辑器是很不同的两种工具。

## Vim 的哲学——多模态编辑

多模态编辑，插入文字、操纵文字有不同模式，**Vim 接口本身就是一种程序语言可以进行编程，键入操作（以及他们的助记名） 本身是命令，这些命令可以组合使用。这使得移动和编辑更加高效。**
Vim 会维护一系列打开的文件，称为**“缓存”**。一个 Vim 会话包含一系列**标签页**，每个标签页包含 一系列**窗口（分隔面板）**。每个窗口显示一个缓存。但值得注意的是，“缓存”和“窗口”之间不是对应关系，窗口只是其中一个视角，我们可以打开多个窗口显示同一个缓存。

| **模式**                       | **作用**                     | **备注**                     |
| ------------------------------ | ---------------------------- | ---------------------------- |
| **正常模式（默认）**           | 在文件中四处移动光标进行修改 | `&lt;ESC&gt;` 回到正常模式         |
| **插入模式**                   | 插入文本                     | `i` 进入                     |
| **替换模式**                   | 替换文本                     | `R` 进入                     |
| **可视化模式（一般，行，块）** | 选中文本块                   | `v` 一般、`V` 行、`&lt;C-v&gt;` 块 |
| **命令模式**                   | 用于执行命令                 | `:` 进入                     |

在不同模式下，按键的含义也不同，一般当前的模式会显示在左下角。

## Vim 命令

除了以下课程上提到的操作之外我推荐 [**Vim 备忘清单**](https://wangchujiang.com/reference/docs/vim.html)

#### 命令行模式

&gt; `:q` 		退出（关闭窗口）
&gt; `:w` 		保存（写）
&gt; `:wq` 		保存然后退出
&gt; `:e {文件名}` 	打开要编辑的文件
&gt; `:ls` 		显示打开的缓存
&gt; `:help {标题}` 	打开帮助文档
&gt; `:help :w` 	打开 `:w` 命令的帮助文档
&gt; `:help w` 	打开 `w` 移动的帮助文档

#### 移动

&gt; **基本移动： **`h``j``k``l` （左， 下， 上， 右）
&gt; **词：** `w` （下一个词）， `b` （词初）， `e` （词尾）
&gt; **行：** `0` （行初）， `^` （第一个非空格字符）， `$` （行尾）
&gt; **屏幕：** `H` （屏幕首行）， `M` （屏幕中间）， `L` （屏幕底部）
&gt; **翻页：** `Ctrl-u` （上翻）， `Ctrl-d` （下翻）
&gt; **文件：** `gg` （文件头）， `G` （文件尾）
&gt; **行数：** `:{行数}&lt;CR&gt;` 或者 `{行数}G` ({行数}为行数)
&gt; **杂项：** `%` （找到配对，比如括号或者 /* */ 之类的注释对）
&gt; **查找：** `f{字符}`， `t{字符}`， `F{字符}`， `T{字符}` 查找/到 向前/向后 在本行的{字符} 
&gt;
&gt; - `,`/`;` 用于导航匹配
&gt;
&gt; **搜索: **`/{正则表达式}`, `n`/`N` 用于导航匹配

#### 编辑

&gt; `i` 进入插入模式
&gt; `O`/`o` 在之上/之下插入行
&gt; `d{移动命令}` 删除 {移动命令}
&gt;
&gt; - 例如，`dw` 删除词, `d$` 删除到行尾, `d0` 删除到行头。
&gt;
&gt; `c{移动命令}` 改变 {移动命令}
&gt;
&gt; - 例如，`cw` 改变词
&gt; - 比如 `d{移动命令}` 再 `i`
&gt;
&gt; `x` 删除字符（等同于 `dl`）
&gt; `s` 替换字符（等同于 `xi`）
&gt; 可视化模式 &#43; 操作
&gt;
&gt; - 选中文字, `d` 删除 或者 `c` 改变
&gt;
&gt; `u` 撤销, `&lt;C-r&gt;` 重做
&gt; `y` 复制 / “yank” （其他一些命令比如 `d` 也会复制）
&gt; `p` 粘贴
&gt; `~` 改变字符的大小写

#### 可视化模式下的选择

&gt; 可视化：`v`
&gt; 可视化行：`V`
&gt; 可视化块：`Ctrl&#43;v`

使用移动命令选中

#### 计数使用

所有你需要用鼠标做的事， 你现在都可以用键盘：采用编辑命令和移动命令的组合来完成。 这就是 Vim 的界面开始看起来像一个程序语言的时候。Vim 的编辑命令也被称为 **“动词”**， 因为动词可以施动于名词。下面是一些例子，通过结合使用，来指定操作多次。

&gt; `3w` 向前移动三个词
&gt; `5j` 向下移动5行
&gt; `7dw` 删除7个词

#### 使用修饰语

你可以用修饰语改变“名词”的意义。
修饰语有 `i`，表示**“内部”**或者**“在内”**，和 `a`， 表示**“周围”**。

&gt; `ci(` 改变当前括号内的内容
&gt; `ci[` 改变当前方括号内的内容
&gt; `da&#39;` 删除一个单引号字符串， 包括周围的单引号

#### 进阶操作：搜索与替换

`:s` （替换）命令（[文档](https://vim.fandom.com/wiki/Search_and_replace)）。

&gt; `%s/foo/bar/g` 	在整个文件中将 foo 全局替换成 bar
&gt; `%s/[._]((._))/\1/g` 	将有命名的 Markdown 链接替换成简单 URLs

#### 进阶操作：多窗口

&gt; 用 `:sp`/`:vsp` 来分割窗口
&gt; 同一个缓存可以在多个窗口中显示。

#### 进阶操作：宏

&gt; `q{字符}` 	来开始在寄存器`{字符}`中录制宏
&gt; `q` 	停止录制
&gt; `@{字符}` 	重放宏，宏的执行遇错误会停止
&gt; `{计数}@{字符}` 执行一个宏{计数}次
&gt; `@@`	重复上一个宏

## 配置文件与扩展插件

### 自定义你的 Vim —— 从配置文件开始

Vim 由一个位于 `~/.vimrc` 的文本配置文件，如果没有就新建一个即可。Vim 能够被重度自定义，花时间探索自定义选项是值得的。可以参考其他人的在 GitHub 上共享的配置文件。例如下面是三位授课人的配置文件，但尽量在阅读后加入自己用的到的部分。不要直接复制。

&gt; [Anish](https://github.com/anishathalye/dotfiles/blob/master/vimrc) 的配置文件
&gt; [Jon](https://github.com/jonhoo/configs/blob/master/editor/.config/nvim/init.vim) (uses [neovim](https://neovim.io/)) 的配置文件
&gt; [Jose](https://github.com/JJGO/dotfiles/blob/master/vim/.vimrc) 的配置文件

下面提供一个文档详细的基本设置，你可以用它当作你的初始设置。我们推荐使用这个设置因为它修复了一些 Vim 默认设置奇怪行为。将其复制粘贴保存为 `~/.vimrc`。

```
&#34; Vimscript 中的注释以 `&#34;` 开头。

&#34; 如果您在 Vim 中打开此文件，它将为您突出显示语法。

&#34; Vim 是基于 Vi 的。设置 `noknown` 会切换默认的 Vi 兼容模式并启用有用的 Vim 功能。
&#34; 对于名为 &#39;~/.vimrc&#39; 的文件来说，这个配置选项不是必需的，因为 Vim 会自动输入 noknown
&#34; 如果该文件存在，则我们将其包含在此处，以防万一该配置文件以其他方式加载
&#34; （例如保存为 `foo`，然后 Vim 以 `vim -u foo` 启动）。
set nocompatible

&#34; 打开语法高亮。
syntax on

&#34; 禁用默认的 Vim 启动消息。
set shortmess&#43;=I

&#34; 显示行号。
set number

&#34; 这将启用相对行编号模式。同时启用 number 和relativenumber 后，当前行显示真实行号，
&#34; 而所有其他行（上方和下方）均相对于当前行进行编号。这很有用，因为您可以知道，
&#34; 一目了然，需要多少次才能向上或向下跳转到特定行，通过 {count}k 向上或通过 {count}j 向下。
set relativenumber

&#34; 即使只打开一个窗口，也始终在底部显示状态行。
set laststatus=2

&#34; 默认情况下，退格键的行为有点不直观。
&#34; 例如，默认情况下，您无法在用“i”设置的插入点之前退格。
&#34; 此配置使退格键的行为更加合理，因为您可以在任何东西上退格。
set backspace=indent,eol,start

&#34; 默认情况下，Vim 不允许您隐藏具有未保存更改的缓冲区（即具有未在任何窗口中显示的缓冲区）。
&#34; 这是为了防止您 &#34; 忘记未保存的更改然后退出，例如 通过`:qa!`。
&#34; 我们发现隐藏缓冲区足以帮助禁用此保护。 有关详细信息，请参阅“:helphidden”。
set hidden

&#34; 当要搜索的字符串中的所有字符均为小写时，此设置使搜索不区分大小写。
&#34; 但是，如果包含任何大写字母，则搜索将区分大小写。这使搜索更加方便。
set ignorecase
set smartcase

&#34; 在键入时启用搜索，而不是等到按 Enter 键。
set incsearch

&#34; 取消绑定一些无用/烦人的默认按键绑定。
&#34; &#39;Q&#39; 在正常模式下进入 Ex 模式。你几乎永远不会想要这个。
nmap Q &lt;Nop&gt; &#34; &#39;Q&#39; in normal mode enters Ex mode. You almost never want this.

&#34; 禁用声音铃声，因为它很烦人。
set noerrorbells visualbell t_vb=

&#34; 启用鼠标支持。您应该避免过度依赖它，但有时它很方便。
set mouse&#43;=a

&#34; 尽量避免使用方向键进行移动等坏习惯。这并不是唯一可能的坏习惯。
&#34; 例如，按住 h/j/k/l 键进行移动，而不是使用更高效的移动命令，也是一种可能的坏习惯。
&#34; 前者可以通过 .vimrc 强制执行，而我们不知道如何防止后者。
&#34; 在正常模式下执行此操作...
nnoremap &lt;Left&gt;  :echoe &#34;Use h&#34;&lt;CR&gt;
nnoremap &lt;Right&gt; :echoe &#34;Use l&#34;&lt;CR&gt;
nnoremap &lt;Up&gt;    :echoe &#34;Use k&#34;&lt;CR&gt;
nnoremap &lt;Down&gt;  :echoe &#34;Use j&#34;&lt;CR&gt;
&#34; ...并在插入模式下执行此操作
inoremap &lt;Left&gt;  &lt;ESC&gt;:echoe &#34;Use h&#34;&lt;CR&gt;
inoremap &lt;Right&gt; &lt;ESC&gt;:echoe &#34;Use l&#34;&lt;CR&gt;
inoremap &lt;Up&gt;    &lt;ESC&gt;:echoe &#34;Use k&#34;&lt;CR&gt;
inoremap &lt;Down&gt;  &lt;ESC&gt;:echoe &#34;Use j&#34;&lt;CR&gt;
```

### 扩展你的 Vim —— 插件的使用

Vim 从 8.0 开始有了内置的插件管理器。只需要创建一个 `~/.vim/pack/vendor/start/` 的文件夹，然后把插件放到这里即可。至于插件的选择，可以到 Vim Awesome 去浏览并尝试一下。

&gt; [Vim Awesome](https://vimawesome.com/)

同时，授课讲师们也推荐了一些插件：

&gt; [ctrlp.vim](https://github.com/ctrlpvim/ctrlp.vim)	模糊文件查找
&gt; [ack.vim](https://github.com/mileszs/ack.vim)	代码搜索
&gt; [nerdtree](https://github.com/scrooloose/nerdtree)	文件浏览器
&gt; [vim-easymotion](https://github.com/easymotion/vim-easymotion)	魔术操作

或者去网路上查找一些博客、文章、回答，关键词是 &#34;best Vim plugins&#34;，当然之前去大佬的配置文件中看看也不失为一个好办法。

### 万物皆可 Vim —— 其他程式的 Vim 模式

#### Shell

&gt; Bash 用户，用 `set -o vi`。
&gt; Zsh：`bindkey -v`。
&gt; Fish 用 `fish_vi_key_bindings`。

另外，不管利用什么 shell，你可以 `export EDITOR=vim`。 这是一个用来决定当一个程序需要启动编辑时启动哪个的环境变量。 例如，git 会使用这个编辑器来编辑 commit 信息。

#### 浏览器

甚至有 Vim 的网页浏览快捷键 [browsers](http://vim.wikia.com/wiki/Vim_key_bindings_for_web_browsers)

&gt; Google Chrome 的 [Vimium](https://chrome.google.com/webstore/detail/vimium/dbepggeogbaibhgnhhndojpepiihcmeb?hl=en)
&gt; Firefox 的 [Tridactyl](https://github.com/tridactyl/tridactyl)

[这个列表](https://reversed.top/2016-08-13/big-list-of-vim-like-software) 中列举了支持类 vim 键位绑定的软件

## 扩展资料

&gt; `vimtutor` 是一个 Vim 安装时自带的教程
&gt; [Vim Adventures](https://vim-adventures.com/) 是一个学习使用 Vim 的游戏
&gt; [Vim Tips Wiki](http://vim.wikia.com/wiki/Vim_Tips_Wiki)
&gt; [Vim Advent Calendar](https://vimways.org/2019/) 有很多 Vim 小技巧
&gt; [Vim Golf](http://www.vimgolf.com/) 是用 Vim 的用户界面作为程序语言的 [code golf](https://en.wikipedia.org/wiki/Code_golf)
&gt; [Vi/Vim Stack Exchange](https://vi.stackexchange.com/)
&gt; [Vim Screencasts](http://vimcasts.org/)
&gt; [Practical Vim](https://pragprog.com/titles/dnvim2/)（书籍）


---

> 作者: 度绛河  
> URL: https://purplered-river.github.io/missing-semester-note-1/  

