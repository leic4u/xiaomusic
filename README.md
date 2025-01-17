# xiaomusic
[![GitHub License](https://img.shields.io/github/license/hanxi/xiaomusic)](https://github.com/hanxi/xiaomusic)
[![Docker Image Version](https://img.shields.io/docker/v/hanxi/xiaomusic?sort=semver&label=docker%20image)](https://hub.docker.com/r/hanxi/xiaomusic)
[![Docker Pulls](https://img.shields.io/docker/pulls/hanxi/xiaomusic)](https://hub.docker.com/r/hanxi/xiaomusic)
[![PyPI - Version](https://img.shields.io/pypi/v/xiaomusic)](https://pypi.org/project/xiaomusic/)
[![PyPI - Downloads](https://img.shields.io/pypi/dm/xiaomusic)](https://pypi.org/project/xiaomusic/)
[![Python Version from PEP 621 TOML](https://img.shields.io/python/required-version-toml?tomlFilePath=https%3A%2F%2Fraw.githubusercontent.com%2Fhanxi%2Fxiaomusic%2Fmain%2Fpyproject.toml)](https://pypi.org/project/xiaomusic/)
[![GitHub Release](https://img.shields.io/github/v/release/hanxi/xiaomusic)](https://github.com/hanxi/xiaomusic/releases)



使用小爱音箱播放音乐，音乐使用 yt-dlp 下载。

<https://github.com/hanxi/xiaomusic>

## 最简配置运行

已经支持在 web 页面配置其他参数，docker compose 配置如下：

```yaml
services:
  xiaomusic:
    image: hanxi/xiaomusic
    container_name: xiaomusic
    restart: unless-stopped
    ports:
      - 8090:8090
    volumes:
      - ./music:/app/music
    environment:
      MI_USER: '小米账号'
      MI_PASS: '小米密码'
      XIAOMUSIC_VERBOSE: 'true'
      XIAOMUSIC_HOSTNAME: 'docker 主机 ip'
```

对应的 docker 启动命令如下:

```yaml
docker run -e MI_USER='小米账号' \
    -e MI_PASS='小米密码' \
    -e XIAOMUSIC_VERBOSE='true' \
    -e XIAOMUSIC_HOSTNAME='docker 主机 ip' \
    -p 8090:8090 \
    -v ./music:/app/music \
    hanxi/xiaomusic
```

启动成功后，在 web 页面可以配置 MI_DID, MI_HARDWARE, XIAOMUSIC_SEARCH, XIAOMUSIC_PROXY 参数。

### ✨ 修改8090端口

如果需要修改 8090 端口为其他端口，比如 5678，需要这样配，3个数字都需要是 5678 。见 <https://github.com/hanxi/xiaomusic/issues/19>

```yaml
services:
  xiaomusic:
    image: hanxi/xiaomusic
    container_name: xiaomusic
    restart: unless-stopped
    ports:
      - 5678:5678
    volumes:
      - ./music:/app/music
    environment:
      MI_USER: '小米账号'
      MI_PASS: '小米密码'
      XIAOMUSIC_VERBOSE: 'true'
      XIAOMUSIC_HOSTNAME: 'docker 主机 ip'
      XIAOMUSIC_PORT: 5678
```

其中 XIAOMUSIC_VERBOSE 设置为 'true' 时表示开启 debug 日志，遇到问题可以去 web 设置页面底部【下载日志文件】按钮，然后搜索一下日志文件内容确保里面没有账号密码信息后(有就删除这些敏感信息)，然后在提 issues 反馈问题时把下载的日志文件带上。

## pip 方式安装运行

```shell
> pip install xiaomusic
> xiaomusic --help
 __  __  _                   __  __                 _
 \ \/ / (_)   __ _    ___   |  \/  |  _   _   ___  (_)   ___
  \  /  | |  / _` |  / _ \  | |\/| | | | | | / __| | |  / __|
  /  \  | | | (_| | | (_) | | |  | | | |_| | \__ \ | | | (__
 /_/\_\ |_|  \__,_|  \___/  |_|  |_|  \__,_| |___/ |_|  \___|
          XiaoMusic v0.1.81 by: github.com/hanxi

usage: xiaomusic.py [-h] [--hardware HARDWARE] [--account ACCOUNT]
                    [--password PASSWORD] [--cookie COOKIE] [--verbose]
                    [--config CONFIG] [--ffmpeg_location FFMPEG_LOCATION]

options:
  -h, --help            show this help message and exit
  --hardware HARDWARE   小爱 hardware
  --account ACCOUNT     xiaomi account
  --password PASSWORD   xiaomi password
  --cookie COOKIE       xiaomi cookie
  --verbose             show info
  --config CONFIG       config file path
  --ffmpeg_location FFMPEG_LOCATION
                        ffmpeg bin path
> xiaomusic --config config.json
```

其中 `config.json` 文件可以参考 `config-example.json` 文件配置。见 <https://github.com/hanxi/xiaomusic/issues/94>

## 开发环境运行

- 使用 install_dependencies.sh 下载依赖
- 使用 pdm 安装环境
- 参考 [xiaogpt](https://github.com/yihong0618/xiaogpt) 设置好环境变量

```shell
export MI_USER="xxxxx"
export MI_PASS="xxxx"
export MI_DID=00000
export XIAOMUSIC_SEARCH='bilisearch:'
```

然后启动即可。默认监听了端口 8090 , 使用其他端口自行修改。

```shell
pdm run xiaomusic.py
````

### 支持口令

- **播放歌曲**
- **播放歌曲**+歌名 比如：播放歌曲周杰伦晴天
- 下一首
- 单曲循环
- 全部循环
- 随机播放
- 关机
- 停止播放
- 刷新列表
- 播放列表+列表名 比如：播放列表其他

> 隐藏玩法: 对小爱同学说播放歌曲小猪佩奇的故事，会播放小猪佩奇的故事。

## 已测试支持的设备

```txt
- L06A
- L07A
- S12
- S12A
- LX5A
- LX05
- L16A
- L17A
- LX06
- LX01
- L05B
- L05C
````

型号与产品名称对照可以在这里查询 <https://home.miot-spec.com/s/xiaomi.wifispeaker>

> 如果你的设备支持播放，请反馈给我添加到支持列表里，谢谢。

## 支持音乐格式

- mp3
- flac
- wav
- ape
- ogg

> 本地音乐会搜索目录下上面格式的文件，下载的歌曲是 mp3 格式的。
> 已知 L05B L05C 不支持 flac 格式。

## 在 Docker 里使用

```shell
docker run -e MI_USER='your-xiaomi-account' \
    -e MI_PASS='your-xiaomi-password' \
    -e MI_DID='your-xiaomi-speaker-mid' \
    -e MI_HARDWARE='L07A' \
    -e XIAOMUSIC_PROXY='proxy-for-yt-dlp' \
    -e XIAOMUSIC_HOSTNAME=192.168.2.5 \
    -e XIAOMUSIC_SEARCH='bilisearch:' \
    -p 8090:8090 \
    -v ./music:/app/music hanxi/xiaomusic
```

- XIAOMUSIC_SEARCH 可以配置为 'bilisearch:' 表示歌曲从哔哩哔哩下载;
    - 配置为 'ytsearch:' 表示歌曲从 youtube 下载。
- XIAOMUSIC_PROXY 用于配置代理，默认为空;
    - 当 XIAOMUSIC_SEARCH 配置为 'ytsearch:' 时在国内需要用到。
- MI_HARDWARE 是小米音箱的型号，默认为'L07A'
- 注意端口必须映射为与容器内一致， XIAOMUSIC_HOSTNAME 需要设置为宿主机的 IP 地址，否则小爱无法正常播放。
- 可以把 /app/music 目录映射到本地，用于保存下载的歌曲。

XIAOMUSIC_PROXY 参数格式参考 yt-dlp 文档说明:
```
Use the specified HTTP/HTTPS/SOCKS proxy. To
enable SOCKS proxy, specify a proper scheme,
e.g. socks5://user:pass@127.0.0.1:1080/.
Pass in an empty string (--proxy "") for
direct connection
```

见 <https://github.com/hanxi/xiaomusic/issues/2> 和 <https://github.com/hanxi/xiaomusic/issues/11>

### 本地编译Docker Image

```shell
docker build -t xiaomusic .
```

### docker compose 示例

使用哔哩哔哩下载歌曲:

```yaml
version: '3'

services:
  xiaomusic:
    image: hanxi/xiaomusic
    container_name: xiaomusic
    restart: unless-stopped
    ports:
      - 8090:8090
    volumes:
      - ./music:/app/music
    environment:
      MI_USER: '小米账号'
      MI_PASS: '小米密码'
      MI_DID: 00000
      MI_HARDWARE: 'L07A'
      XIAOMUSIC_SEARCH: 'bilisearch:'
      XIAOMUSIC_HOSTNAME: '192.168.2.5'
```


使用 youtobe 下载歌曲:

```yaml
version: '3'

services:
  xiaomusic:
    image: hanxi/xiaomusic
    container_name: xiaomusic
    restart: unless-stopped
    ports:
      - 8090:8090
    volumes:
      - ./music:/app/music
    environment:
      MI_USER: '小米账号'
      MI_PASS: '小米密码'
      MI_DID: 00000
      MI_HARDWARE: 'L07A'
      XIAOMUSIC_SEARCH: 'ytsearch:'
      XIAOMUSIC_PROXY: 'http://192.168.2.5:8080'
      XIAOMUSIC_HOSTNAME: '192.168.2.5'
```

如果想让 setting.json 文件不存储到 music 目录，可以这样配，下面的示例会把 setting.json 文件放到容器的 /app/conf 目录且映射到本地的 ./conf 目录：

```yaml
services:
  xiaomusic:
    image: hanxi/xiaomusic
    container_name: xiaomusic
    restart: unless-stopped
    ports:
      - 8090:8090
    volumes:
      - ./music:/app/music
      - ./conf:/app/conf
    environment:
      MI_USER: '小米账号'
      MI_PASS: '小米密码'
      XIAOMUSIC_HOSTNAME: 'docker 主机 ip'
      XIAOMUSIC_CONF_PATH: '/app/conf'
```


## 简易的控制面板

浏览器进入 <http://192.168.2.5:8090>

- ip 是 XIAOMUSIC_HOSTNAME 设置的
- 8090 是默认端口
- 支持功能
    - 显示正在播放的歌曲
    - 模糊搜索本地歌曲
    - 播放列表
    - 删除歌曲
    - 设置页面
    - 配置网络歌单
    - 日志文件下载

采用新的设置页面之后，必须在启动前配置的环境变量只剩下:
- MI_USER
- MI_PASS
- XIAOMUSIC_HOSTNAME

其他的这些可以在网页里配置:
- MI_DID
- MI_HARDWARE
- XIAOMUSIC_SEARCH
- XIAOMUSIC_PROXY

## 网络歌单功能

可以配置一个 json 格式的歌单，支持电台和歌曲，也可以直接用别人分享的链接，同时配备了 m3u 文件格式转换工具，可以很方便的把 m3u 电台文件转换成网络歌单格式的 json 文件，具体用法见  <https://github.com/hanxi/xiaomusic/issues/78>

> 欢迎有想法的朋友们制作更多的歌单转换工具。

## 更多其他可选配置

- XIAOMUSIC_ACTIVE_CMD 环境变量，用于唤醒口令，配置成'play,random_play'，在非播放状态下，只有这两个指令（播放歌曲和随机播放）可以触发，触发后，xiaomusic进入playing状态，其他指令则可以正常触发。具体见 <https://github.com/hanxi/xiaomusic/pull/43>
- XIAOMUSIC_EXCLUDE_DIRS 配置歌曲目录里需要忽略的目录
- XIAOMUSIC_MUSIC_PATH_DEPTH 配置歌曲目录搜索深度，具体见 <https://github.com/hanxi/xiaomusic/issues/76>
- XIAOMUSIC_DISABLE_HTTPAUTH 配置成 false 表示开启密码访问web控制台，具体见 <https://github.com/hanxi/xiaomusic/issues/47>
- XIAOMUSIC_HTTPAUTH_USERNAME 配置 web 控制台用户
- XIAOMUSIC_HTTPAUTH_PASSWORD 配置 web 控制台密码
- XIAOMUSIC_CONF_PATH 用来存放配置文件的目录，记得把目录映射到主机，默认情况会把配置存放在music目录，具体见 <https://github.com/hanxi/xiaomusic/issues/74>
- XIAOMUSIC_DISABLE_DOWNLOAD 设为 true 时关闭下载功能，见 <https://github.com/hanxi/xiaomusic/issues/82>
- XIAOMUSIC_USE_MUSIC_API 设为 true 时使用 player_play_music 接口播放音乐，用于兼容不能播放的型号
- XIAOMUSIC_KEYWORDS_PLAY 用来播放歌曲的口令前缀，默认是 "播放歌曲,放歌曲" ，可以用英文逗号分割配置多个
- XIAOMUSIC_KEYWORDS_STOP 用来关机的口令，默认是 "关机,暂停,停止" ，可以用英文逗号分割配置多个。
- XIAOMUSIC_KEYWORDS_PLAYLOCAL 用来播放本地歌曲的口令前缀，本地找不到时不会下载歌曲，默认是 "播放本地歌曲,本地播放歌曲" ，可以用英文逗号分割配置多个。
- XIAOMUSIC_ENABLE_FUZZY_MATCH 设为 true 时开启模糊匹配（默认），设为 false 时关闭模糊匹配，支持模糊匹配歌名和歌单名。 具体见 <https://github.com/hanxi/xiaomusic/issues/52>
- XIAOMUSIC_FUZZY_MATCH_CUTOFF 设置模糊搜索匹配的最低相似度阈值（默认0.6，可以配0到1直接的小数），越小越模糊，越大越精准。具体见 <https://github.com/hanxi/xiaomusic/issues/52>

## 讨论区

- [点击链接加入QQ频道【xiaomusic】](https://pd.qq.com/s/e2jybz0ss)
- [点击链接加入群聊【xiaomusic】 604526973](http://qm.qq.com/cgi-bin/qm/qr?_wv=1027&k=13St5PLVcTxYlWTAs_iAawazjtdD1l-a&authKey=dJWEpaT2fDBDpdUUOWj%2FLt6NS1ePBfShDfz7a6seNURi05VvVnAGQzXF%2FM%2F5HgIm&noverify=0&group_code=604526973)
- <https://github.com/hanxi/xiaomusic/issues>
- [微信群二维码](https://github.com/hanxi/xiaomusic/issues/86)

## 感谢

- [xiaomi](https://www.mi.com/)
- [PDM](https://pdm.fming.dev/latest/)
- [xiaogpt](https://github.com/yihong0618/xiaogpt)
- [MiService](https://github.com/yihong0618/MiService)
- [yt-dlp](https://github.com/yt-dlp/yt-dlp)
- [NAS部署教程](https://post.m.smzdm.com/p/avpe7n99/)
- [群晖部署教程](https://post.m.smzdm.com/p/a7px7dol/)
- [QNAS部署教程](https://post.smzdm.com/p/a5xz5x63/)

## Star History

[![Star History Chart](https://api.star-history.com/svg?repos=hanxi/xiaomusic&type=Date)](https://star-history.com/#hanxi/xiaomusic&Date)

## 赞赏

- 爱发电 <https://afdian.net/a/imhanxi>
- 点个 Star ⭐
- 谢谢 ❤️
