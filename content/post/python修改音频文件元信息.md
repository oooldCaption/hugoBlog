---
title: "Python修改音频文件元信息"
date: 2022-12-03T21:44:50+08:00
draft: false
tags: ['巧篆',Python]
categories: ['软件开发']
---


事情是这样的 

.......... 

巴拉巴拉巴拉巴拉巴拉巴拉

巴拉巴拉巴拉巴拉巴拉巴拉

巴拉巴拉巴拉巴拉巴拉巴拉

..........

我下载了一个 MP3 版本的<**天龙八部**>想着上下班通勤的时候听, 结果导入到本地播放器(EverPlayer)的时候发现播放顺序是乱的. 

这样的话可能剧情上就会断断续续, 听起来有点前言不搭后语的, 毕竟不是按照播讲顺序听的.

本来我想应该是因为文件名读取的问题,  因为文件名格式并不是按照` 01 , 02,  03.... 33, 34`这种顺序排列的, 所以写了一个小脚本, 用来序列化文件名, 保证文件名是 从 `1,2,3....32,33,34` 这样的逻辑顺序. 然后重新新导入到播放器, 发现还是不对. 

认真打量了一眼播放器的页面信息, 发现排序根据音频文件的元数据信息进行排序的. 找到了问题的所在, 我们就可以解决这个问题啦. 

###  为什么要修改音频文件的元数据标签

举个🌰:

我现在有一个 mp3 文件, 文件名叫做, `001 青衫磊落险峰行1.mp3`,

在电脑中, 我们可以查看文件的详细信息, 如图所示: 
可以看到, 在红色方框中显示标题是 vx 听书, 但是文件名却是带序列号的文件名, 因为有些音频播放器会读取 标题 , 来当做歌曲名, 而不是将文件名作为歌曲名的读取对象

![1678871232.png](https://img.52smile.vip/1678871232.png)

我们可以按照以下的方式来读取 `mp3` 文件的 `标签`  或者 `元数据`

```
from mutagen.id3 import ID3, TIT1, TIT2

name = "001青衫磊落险峰行1.mp3"
def read_mp3():
    audio = ID3(name)
    for tag in audio.keys():
        print(tag + ": " + str(audio.get(tag)))

read_mp3()

```

运行之后你会在控制台看到如下输出结果, 结果中包含云数据标签的 **标签名** `(eg:TIT2)` 和 **标签对应的值** `(eg: vx听书)`

```
TIT2: vx听书
TPE1: 唯一拼课微信：****
TALB: 唯一拼课微信：****
TCOM: 唯一拼课微信：****
TPE2: 唯一拼课微信：****
COMM::eng: 唯一拼课微信：****
COMM:ID3v1 Comment:eng: ****
APIC:: APIC(encoding=<Encoding.LATIN1: 0>, mime='image/jpeg', type=<PictureType.COVER_FRONT: 3>, desc='', data=b'...')

```

我们要做的就是 去弄明白 这些标签名 对应的是什么意思, 这个你可以在相关第三方的 api 文档中得到答案,  我在下面将 mp3 类型的文件做了一下统计和整理, 列表如下: 

### MP3 格式标签属性列表

| 标签   | 含义                                                         | 
|-------|-------------------------------------------------------------|
| APIC  | 封面图像，包括图片类型、描述、二进制数据等。                  |
| COMM  | 评论，包括语言、评论文本等。                                   |
| TALB  | 专辑名称。                                                   |
| TBPM  | 每分钟节拍数。                                                |
| TCOM  | 作曲家。                                                      |
| TCON  | 音频类型，如流行、摇滚、古典等。                                |
| TCOP  | 版权信息。                                                    |
| TDAT  | 日期。                                                        |
| TDRC  | 录制日期。                                                    |
| TENC  | 编码器。                                                      |
| TEXT  | 作词家。                                                      |
| TFLT  | 文件类型。                                                    |
| TIME  | 时间。                                                        |
| TIT1  | 内容组描述。                                                  |
| TIT2  | 歌曲名称。                                                    |
| TIT3  | 副标题。                                                      |
| TLEN  | 音频长度。                                                    |
| TMED  | 媒体类型。                                                    |
| TOAL  | 原始专辑/电影/节目名称。                                       |
| TOFN  | 原始文件名。                                                  |
| TOLY  | 原始歌词作者。                                                |
| TOPE  | 原始表演者。                                                  |
| TORY  | 原始发行年份。                                                |
| TOWN  | 文件所有者。                                                  |
| TPE1  | 艺术家。                                                      |
| TPE2  | 乐队。                                                        |
| TPE3  | 指挥者/演奏团体。                                              |
| TPE4  | 翻译/修改者。                                                 |
| TPOS  | 作品集部分。                                                  |
| TPUB  | 出版者。                                                      |
| TRCK  | 曲目号。                                                      |
| TRDA  | 录制日期。                                                    |
| TRSN  | Internet电台名称。                                            |
| TRSO  | Internet电台所有者。                                          |
| TSIZ  | 音频大小。                                                    |
| TSRC  | 国际标准录音编码(ISRC)。                                       |
| TSSE  | 编码器设置。                                                  |
| TYER  | 年份。                                                        |

### 修改音频文件标签
看完上面那个 长长的表格, 我们就会知道自己有哪些属性是想要修改的, 比如想要在播放的时候, 文件播放序列是从 01 -> 10 的正序进行播放, 我们就可以修改音频文件的 `TIT2` 属性为 为你想要播放的序号, 比如修改如下 :

```
001青衫磊落险峰行1.MP3  TIT2 --->  01
001青衫磊落险峰行2.MP3  TIT2 --->  02
001青衫磊落险峰行3.MP3  TIT2 --->  03
```
在代码里面可以使用如下方式进行修改:


```
from mutagen.id3 import ID3, TIT1, TIT2

name = "001青衫磊落险峰行1.mp3"
def mod_mp3():
    audio = ID3(name)
    audio["TIT2"] = TIT2(encoding=3, text=["01:青衫磊落险峰行1"]);
    audio.save()
mod_mp3()

```

通过这种方式, 我们可以修改音频文件所有的原信息, 比如 我给 mp3 添加一个专辑图

### 给 mp3 添加一张图片
下面的代码提供了一个修改 mp3 文件元数据标签的实例, 修改了 标题, 副标题, 作曲, 以及封面图:

![1678927491.png](https://img.52smile.vip/1678927491.png)

```

def mod_tag():
    audio = ID3(name)
    with open("cover.png", "rb") as f:
        image_data = BytesIO(f.read())

    apic = APIC(
        encoding=3,  # UTF-8
        mime='image/png',
        type=3,  # cover front
        desc=u'Cover',
        data=image_data.getvalue()
    )
    #修改标题/专辑/作者信息
    audio['TABL'] = TALB(encoding=3, text="天龙八部")
    audio['TIT2'] = TIT2(encodings = 3, text='青衫磊落险峰行')
    audio['TIT3'] = TIT3(encodings = 3, text='01')
    audio['TCOM'] = TCOM(encodings = 3, text='金庸')
    # 修改专辑封面图
    audio['APIC'] = apic
    audio.save()


```


### 修改 FLAC 格式

```
from mutagen.flac import FLAC

# 打开FLAC文件
audio = FLAC("example.flac")

# 获取元数据信息
title = audio.get("title", "Unknown Title")
artist = audio.get("artist", "Unknown Artist")
album = audio.get("album", "Unknown Album")
bitrate = audio.info.bitrate
length = audio.info.length

# 打印元数据信息
print("Title:", title)
print("Artist:", artist)
print("Album:", album)
print("Bitrate:", bitrate)
print("Length:", length)

```

### 修改 OGG 格式 

```
from mutagen.oggvorbis import OggVorbis

# 打开OGG文件
audio = OggVorbis("example.ogg")

# 获取元数据信息
title = audio.get("title", "Unknown Title")
artist = audio.get("artist", "Unknown Artist")
album = audio.get("album", "Unknown Album")
bitrate = audio.info.bitrate
length = audio.info.length

# 打印元数据信息
print("Title:", title)
print("Artist:", artist)
print("Album:", album)
print("Bitrate:", bitrate)
print("Length:", length)


```

### 修改 WAV 格式
```
from mutagen.oggvorbis import OggVorbis

# 打开OGG文件
audio = OggVorbis("example.ogg")

# 获取元数据信息
title = audio.get("title", "Unknown Title")
artist = audio.get("artist", "Unknown Artist")
album = audio.get("album", "Unknown Album")
bitrate = audio.info.bitrate
length = audio.info.length

# 打印元数据信息
print("Title:", title)
print("Artist:", artist)
print("Album:", album)
print("Bitrate:", bitrate)
print("Length:", length)

```

