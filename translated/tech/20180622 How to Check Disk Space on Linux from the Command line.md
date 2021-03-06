如何使用命令行检查 Linux 上的磁盘空间
========

>通过使用 `df` 命令和 `du` 命令查看 Linux 系统上挂载的驱动器的空间使用情况

![](https://camo.githubusercontent.com/9e87938753101d1aad089c55f3793b6f0ce8158f/68747470733a2f2f7777772e6c696e75782e636f6d2f73697465732f6c636f6d2f66696c65732f7374796c65732f72656e64657265645f66696c652f7075626c69632f6469736b73706163652d6d61696e2e6a70673f69746f6b3d74394f7878633958)

-----------------------------

*** 快速提问： ***你的驱动器剩余多少剩余空间？一点点还是很多？接下来的提问是：你知道如何找出这些剩余空间吗？如果你使用的是 GUI 桌面（ 例如 GNOME，KDE，Mate，Pantheon 等 ），则任务可能非常简单。但是，当你要在一个没有 GUI 桌面的服务器上查询剩余空间，你该如何去做呢？你是否要为这个任务安装相应的软件工具？答案是绝对不是。在 Linux 中，具备查找驱动器上的剩余磁盘空间的所有工具。事实上，有两个非常容易使用的工具。

在本文中，我将演示这些工具。我将使用 Elementary OS（ LCTT译注：Elementary OS 是基于 Ubuntu 精心打磨美化的桌面 Linux 发行版 ），它还包括一个 GUI 选项，但我们将限制自己仅使用命令行。好消息是这些命令行工具随时可用于每个 Linux 发行版。在我的测试系统中，连接了许多的驱动器（ 内部的和外部的 ）。使用的命令与连接驱动器的位置无关，仅仅与驱动器是否已经挂载好并且对操作系统可见。

话虽如此，让我们来试试这些工具。

### df

`df` 命令是我第一次用于在 Linux 上查询驱动器空间的工具，时间可以追溯到20世纪90年代。它的使用和报告结果非常简单。直到今天，`df` 还是我执行此任务的首选命令。此命令有几个选项开关，对于基本的报告，你实际上只需要一个选项。该命令是 `df -H` 。`-H` 选项开关用于将df的报告结果以人类可读的格式进行显示。`df -H` 的输出包括：已经使用了的空间量，可用空间，空间使用的百分比，以及每个磁盘连接到系统的挂载点（ 图 1 ）。

![](https://camo.githubusercontent.com/3e52d8b2ba349ecc8517b32080f18ec8216ca63c/68747470733a2f2f7777772e6c696e75782e636f6d2f73697465732f6c636f6d2f66696c65732f7374796c65732f72656e64657265645f66696c652f7075626c69632f6469736b73706163655f312e6a70673f69746f6b3d614a6138415a414d)

图 1：Elementary OS 系统上 `df -H` 命令的输出结果

如果你的驱动器列表非常长并且你只想查看单个驱动器上使用的空间，该怎么办？有了 `df`，就可以做到。我们来看一下位于 `/dev/sda1` 的主驱动器已经使用了多少空间。为此，执行如下命令：
```
    df -H /dev/sda1
```
输出将限于该驱动器（ 图 2 ）。
![](https://camo.githubusercontent.com/4bb588e30a52dff9b588b14e489eb5ffaae98862/68747470733a2f2f7777772e6c696e75782e636f6d2f73697465732f6c636f6d2f66696c65732f7374796c65732f72656e64657265645f66696c652f7075626c69632f6469736b73706163655f322e6a70673f69746f6b3d5f504171336b7843)

图 2：一个单独驱动器空间情况

你还可以限制 `df` 命令结果报告中显示指定的字段。可用的字段包括：

- source — 文件系统的来源（ LCTT译注：通常为一个设备，如 `/dev/sda1` ）
- size — 块总数
- used — 驱动器已使用的空间
- avail — 可以使用的剩余空间
- pcent — 驱动器已经使用的空间占驱动器总空间的百分比
- target —驱动器的挂载点

让我们显示所有驱动器的输出，仅显示 `size` ，`used` ，`avail` 字段。对此的命令是：
```
    df -H --output=size,used,avail
```
该命令的输出非常简单（ 图 3 ）。
![](https://camo.githubusercontent.com/57dc803d72d6927b31e02b16e9cf695fec6b3a13/68747470733a2f2f7777772e6c696e75782e636f6d2f73697465732f6c636f6d2f66696c65732f7374796c65732f72656e64657265645f66696c652f7075626c69632f6469736b73706163655f332e6a70673f69746f6b3d35316d38492d5675)

图 3：显示我们驱动器的指定输出

这里唯一需要注意的是我们不知道输出的来源，因此，我们要把来源加入命令中：
```
    df -H --output=source,size,used,avail
```
现在输出的信息更加全面有意义（ 图 4 ）。
![](https://camo.githubusercontent.com/e30919f4cce4655d1eee89c635b83fcf7e73e44e/68747470733a2f2f7777772e6c696e75782e636f6d2f73697465732f6c636f6d2f66696c65732f7374796c65732f72656e64657265645f66696c652f7075626c69632f6469736b73706163655f342e6a70673f69746f6b3d5375776775654e33)

图 4：我们现在知道了磁盘使用情况的来源


### du

我们的下一个命令是 `du` 。 正如您所料，这代表磁盘使用情况（ disk usage ）。 `du` 命令与 `df` 命令完全不同，因为它报告目录而不是驱动器的空间使用情况。 因此，您需要知道要检查的目录的名称。 假设我的计算机上有一个包含虚拟机文件的目录。 那个目录是 `/media/jack/HALEY/VIRTUALBOX` 。 如果我想知道该特定目录使用了多少空间，我将运行如下命令：
```
    du -h /media/jack/HALEY/VIRTUALBOX
```
上面命令的输出将显示目录中每个文件占用的空间（ 图 5 ）。
![](https://camo.githubusercontent.com/7f7cc19851dfe98abaa782431c924e5a3d2061f7/68747470733a2f2f7777772e6c696e75782e636f6d2f73697465732f6c636f6d2f66696c65732f7374796c65732f72656e64657265645f66696c652f7075626c69632f6469736b73706163655f352e6a70673f69746f6b3d5866533473375a71)

图 5 在特定目录上运行 `du` 命令的输出

到目前为止，这个命令并没有那么有用。如果我们想知道特定目录的总使用量怎么办？幸运的是，`du` 可以处理这项任务。对于同一目录，命令将是：
```
    du -sh /media/jack/HALEY/VIRTUALBOX/
```
现在我们知道了上述目录使用存储空间的总和（ 图 6 ）。
![](https://camo.githubusercontent.com/13cc1575d0612367b86ada9250cc03adb84272c7/68747470733a2f2f7777772e6c696e75782e636f6d2f73697465732f6c636f6d2f66696c65732f7374796c65732f72656e64657265645f66696c652f7075626c69632f6469736b73706163655f362e6a70673f69746f6b3d7237317149437947)

图 6：我的虚拟机文件使用存储空间的总和是 559GB

您还可以使用此命令查看父项的所有子目录使用了多少空间，如下所示：
```
    du -h /media/jack/HALEY
```
此命令的输出见（ 图 7 ），是一个用于查看各子目录占用的驱动器空间的好方法。
![](https://camo.githubusercontent.com/a59213db964bdeb8680e1b91f03fb6e631a58d8f/68747470733a2f2f7777772e6c696e75782e636f6d2f73697465732f6c636f6d2f66696c65732f7374796c65732f72656e64657265645f66696c652f7075626c69632f6469736b73706163655f372e6a70673f69746f6b3d5074446534713579)

图 7：子目录的存储空间使用情况

`du` 命令也是一个很好的工具，用于查看使用系统磁盘空间最多的目录列表。执行此任务的方法是将 `du` 命令的输出通过管道传递给另外两个命令：`sort` 和 `head` 。下面的命令用于找出驱动器上占用存储空间最大的前10各目录：
```
    du -a /media/jack | sort -n -r |head -n 10
```
输出将以从大到小的顺序列出这些目录( 图 8 )。
![](https://camo.githubusercontent.com/4ddae52f2bd56f9a9c161e82f095e0671133855e/68747470733a2f2f7777772e6c696e75782e636f6d2f73697465732f6c636f6d2f66696c65732f7374796c65732f72656e64657265645f66696c652f7075626c69632f6469736b73706163655f382e6a70673f69746f6b3d7639453153466343)

图 8：使用驱动器空间最多的 10 个目录

### 没有你想像的那么难

查看 Linux 系统上挂载的驱动器的空间使用情况非常简单。只要你将你的驱动器挂载在 Linux 系统上，使用 `df` 命令或 `du` 命令在报告必要信息方面都会非常出色。使用 `df` 命令，您可以快速查看磁盘上总的空间使用量，使用 `du` 命令，可以查看特定目录的空间使用情况。对于每一个 Linux 系统的管理员来说，这两个命令的结合使用是必须掌握的。

而且，如果你不需要使用 `du` 或 `df` 命令查看驱动器空间的使用情况，我最近介绍了查看 Linux 上内存使用情况的方法。总之，这些技巧将大力帮助你成功管理 Linux 服务器。

通过 Linux Foundation 和 edX 免费提供的 “ Linux 简介 ” 课程，了解更多有关 Linux 的信息。

--------

via: https://www.linux.com/learn/intro-to-linux/2018/6how-check-disk-space-linux-command-line

作者：Jack Wallen 选题：lujun9972 译者：SunWave 校对：校对者ID

本文由 LCTT 原创编译，Linux中国 荣誉推出

























