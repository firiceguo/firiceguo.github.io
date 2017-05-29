---
layout: post
title: "Elementary OS 探索 —— Conky"
date: 2016-03-11
tags: ElementaryOS
categories: ElementaryOS
thumbnail:  linux
comments: true
description: My Conky in Elementary OS
toc: true
---

# Elementary OS 探索 —— Conky

## 0. 关于 Conky

[Conky](https://github.com/brndnmtthws/conky) 是一个可高度定制的桌面环境的监视软件。可以看出，所谓“高度定制”，就是从大小到颜色到形状到功能全部交给用户自己定制。

总而言之，就是没有点艺术细胞弄出来的会很丑且看起来不和谐。Bingo！我没有艺术细胞，所以就只能使用拿来主义了。

我使用的主题叫做 [Harmattan](http://zagortenay333.deviantart.com/art/Conky-Harmattan-426662366) ,在此盗用作者的一张图：
![conky]({{ site.baseurl | prepend:site.url}}/images/conky-harmattan.png){: .center-image }*Conky-harmattan by zagortenay333*

看起来总体效果还是不错的，下面就开始折腾了。

## 1. 安装

[这里](https://github.com/zagortenay333/Harmattan#harmattan-sunny-umbrella-cloud-snowflake-snowman)是项目主页，各种说明也在这里。

按照说明，确保安装 `conky` ，`curl` 和 `jq`，这些可以直接通过apt安装。

- 首先下载 `zip` 包，解压缩到任意目录。
- 将 `.harmattan-assets` 目录移动或者复制到 `~` 目录中。
- 在 `.harmattan-themes` 目录里有各种不同的 `.conkyrc` 文件，这些文件是不同主题的 conky 配置文件，选择好一个拷贝到 `~` 目录里就可以了。
- 命令行运行 `conky` 即可看到效果


## 2. 修改配置文件

这个是最初版本的配置文件的效果：

![conky-0]({{ site.baseurl | prepend:site.url}}/images/conky-before-edit.png){: .center-image }*修改配置文件之前的状态*


可以看出来，是有很多问题的，比如周围错位，内部错位，没有天气信息什么的，我改了下配置文件，效果好多了：

![conky-final]({{ site.baseurl | prepend:site.url}}/images/conky-after-edit.png){: .center-image }*修改配置文件之后的状态*

作者用的是 [OpenWeatherMap](http://openweathermap.org/) 的天气源，我们需要去上面注册一个用户，之后会给你一个 `API key` ，把那个 key 粘贴到 `template6 ""` ，城市代码在[这里](http://openweathermap.org/help/city_list.txt)找到之后粘贴到 `template7 ""` 就行了。
配置文件如下：

{% highlight bash %}
#====================================
#   Conky Settings
#====================================
background yes
update_interval 1
double_buffer yes
no_buffers yes
imlib_cache_size 10

draw_shades no
draw_outline no
draw_borders no
draw_graph_borders no
default_graph_size 26 80
show_graph_scale no
show_graph_range no


#====================================
#   Window Specifications
#====================================
gap_x 1050
gap_y 70
minimum_size 268 620
maximum_width 268
own_window yes
own_window_type normal  # other options are: override/dock/desktop/panel
own_window_transparent no
own_window_hints undecorate,sticky,skip_taskbar,skip_pager,below
border_inner_margin 0
border_outer_margin 0
#alignment middle_middle
own_window_argb_visual yes
own_window_argb_value 220


#====================================
#   Text Settings
#====================================
use_xft yes
xftalpha 0
xftfont Droid Sans:size=8
text_buffer_size 256
override_utf8_locale yes

short_units yes
short_units yes
pad_percents 2
top_name_width 7


#====================================
#   Color Scheme
#====================================
default_color DCDCDC
color1 DCDCDC
color2 DCDCDC
color3 DCDCDC
color4 F9F9F9
color5 D64937
color6 888888
color7 484848
color8 2D2D2D


#====================================
#   API Key
#====================================
template6 ""


#====================================
#   City ID
#====================================
template7 ""


#====================================
#   Temp Unit (default, metric, imperial)
#====================================
template8 metric


#====================================
#   Locale (e.g. "es_ES.UTF-8")
#   Leave empty for default
#   Leave the quotes
#====================================
template9 ""


###################################################
###################################################


TEXT
#----------------------------------------
#   CURL
#----------------------------------------
\
\
${execi 300 l=${template9}; l=${l%%_*}; curl -s "api.openweathermap.org/data/2.5/forecast/daily?APPID=${template6}&id=${template7}&cnt=5&units=${template8}&lang=$l" -o ~/.cache/forecast.json}\
${execi 300 l=${template9}; l=${l%%_*}; curl -s "api.openweathermap.org/data/2.5/weather?APPID=${template6}&id=${template7}&cnt=5&units=${template8}&lang=$l" -o ~/.cache/weather.json}\
\
\
#----------------------------------------
#   Misc Images
#----------------------------------------
\
\
${image ~/.harmattan-assets/misc/Numix/God-Mode/top-bg.png -p 0,0 -s 268x81}\
${image ~/.harmattan-assets/misc/Numix/God-Mode/bottom-bg.png -p 0,473 -s 268x119}\
${image ~/.harmattan-assets/misc/Numix/God-Mode/border.png -p 0,81 -s 268x86}\
${image ~/.harmattan-assets/misc/Numix/God-Mode/fav-color.png -p 0,81 -s 268x86}\
${image ~/.harmattan-assets/misc/Numix/God-Mode/bg-1.png -p 0,167 -s 268x96}\
${image ~/.harmattan-assets/misc/Numix/God-Mode/bg-2.png -p 0,263 -s 268x105}\
${image ~/.harmattan-assets/misc/Numix/God-Mode/bg-3.png -p 0,368 -s 268x251}\
${image ~/.harmattan-assets/misc/Numix/God-Mode/bg-4.png -p 0,478 -s 268x94}\
${image ~/.harmattan-assets/misc/Numix/God-Mode/separator-v.png -p 95,185 -s 1x72}\
${image ~/.harmattan-assets/misc/Numix/God-Mode/separator-v.png -p 172,185 -s 1x72}\
${image ~/.harmattan-assets/misc/Numix/God-Mode/separator-h.png -p 33,360 -s 202x1}\
${image ~/.harmattan-assets/misc/Numix/God-Mode/separator-h.png -p 33,260 -s 202x1}\
\
\
#----------------------------------------
#   Day names
#----------------------------------------
\
\
${color3}${voffset 172}${alignc 77}${execi 300 LANG=${template9} LC_TIME=${template9} date +%^a}${color}
${color3}${voffset -13}${alignc}${execi 300 LANG=${template9} LC_TIME=${template9} date -d +1day +%^a}${color}
${color3}${voffset -13}${alignc -77}${execi 300 LANG=${template9} LC_TIME=${template9} date -d +2day +%^a}${color}
\
\
#----------------------------------------
#   Temperatures
#----------------------------------------
\
\
${color2}${voffset 51}${alignc 77}${execi 300 jq -r .list[0].temp.min ~/.cache/forecast.json | awk '{print int($1+0.5)}' # round num}°/${execi 300 jq -r .list[0].temp.max ~/.cache/forecast.json | awk '{print int($1+0.5)}' # round num}°${color}
${color2}${voffset -13}${alignc}${execi 300 jq -r .list[1].temp.min ~/.cache/forecast.json | awk '{print int($1+0.5)}' # round num}°/${execi 300 jq -r .list[1].temp.max ~/.cache/forecast.json | awk '{print int($1+0.5)}' # round num}°${color}
${color2}${voffset -13}${alignc -77}${execi 300 jq -r .list[2].temp.min ~/.cache/forecast.json | awk '{print int($1+0.5)}' # round num}°/${execi 300 jq -r .list[2].temp.max ~/.cache/forecast.json | awk '{print int($1+0.5)}' # round num}°${color}
\
\
#----------------------------------------
#   Conditions
#----------------------------------------
\
\
${goto 36}${voffset -172}${font Droid Sans :size=36}${color4}${execi 300 jq -r .main.temp ~/.cache/weather.json | awk '{print int($1+0.5)}' # round num}°${font}${color}
${goto 46}${voffset 14}${font Droid Sans :size=12}${color4}${execi 300 jq -r .weather[0].description ~/.cache/weather.json | sed 's|\<.|\U&|g'}${font}${color}
${color1}${alignr 52}${voffset -73}${execi 300 jq -r .main.pressure ~/.cache/weather.json | awk '{print int($1+0.5)}' # round num} hPa
${color1}${alignr 52}${voffset 7}${execi 300 jq -r .main.humidity ~/.cache/weather.json | awk '{print int($1+0.5)}' # round num} %${color}
${color1}${alignr 52}${voffset 7}${execi 300 jq -r .wind.speed ~/.cache/weather.json | awk '{print int($1+0.5)}' # round num}${if_match "$template8" == "metric"} m/s${else}${if_match "$template8" == "default"} m/s${else}${if_match "$template8" == "imperial"} mi/h${endif}${endif}${endif}${color}
\
\
#----------------------------------------
#   Clock and Date
#----------------------------------------
\
\
${voffset -117}${font Droid Sans Mono :size=22}${alignc}${color2}${time %H:%M}${font}${color}
${voffset 4}${font Droid Sans :size=10}${alignc}${color6}${execi 300 LANG=${template9} LC_TIME=${template9} date +"%A, %B %-d"}${font}${color}
\
\
#----------------------------------------
#   Cpu, memory, uptime, and load graph
#----------------------------------------
\
\
${voffset 294}${goto 40}${color5}Cpu:${color}
${voffset 4}${goto 40}${color5}Mem:${color}
${voffset 4}${goto 40}${color5}Uptime:${color}
${voffset -47}${alignr 39}${color6}${cpu cpu0}%${color}
${voffset 4}${alignr 39}${color6}${memperc}%${color}
${voffset 4}${alignr 39}${color6}${uptime_short}${color}
${voffset -47}${alignc}${color2}${cpubar 5,36}${color}
${voffset 4}${alignc}${color2}${membar 5,36}${color}
${voffset 29}${goto 40}${loadgraph 26,190 D64937 D64937 -l}
\
\
#----------------------------------------
#   Processes
#----------------------------------------
\
\
${voffset 26}${goto 40}${color5}${top_mem name 1}${color}
${voffset 4}${goto 40}${color5}${top_mem name 2}${color}
${voffset 4}${goto 40}${color5}${top_mem name 3}${color}
${voffset 4}${goto 40}${color5}${top_mem name 4}${color}
${voffset 4}${goto 40}${color5}${top_mem name 5}${color}
${voffset -81}${alignc}${color2}${top_mem mem 1}%${color}
${voffset 4}${alignc}${color2}${top_mem mem 2}%${color}
${voffset 4}${alignc}${color2}${top_mem mem 3}%${color}
${voffset 4}${alignc}${color2}${top_mem mem 4}%${color}
${voffset 4}${alignc}${color2}${top_mem mem 5}%${color}
${voffset -81}${alignr 39}${color6}${top_mem mem_res 1}${color}
${voffset 4}${alignr 39}${color6}${top_mem mem_res 2}${color}
${voffset 4}${alignr 39}${color6}${top_mem mem_res 3}${color}
${voffset 4}${alignr 39}${color6}${top_mem mem_res 4}${color}
${voffset 4}${alignr 39}${color6}${top_mem mem_res 5}${color}
${voffset -104}${goto 40}${color1}Proc${color}
${voffset -13}${alignc}${color1}Mem%${color}
${voffset -13}${alignr 39}${color1}Mem${color}
\
\
#----------------------------------------
#   Network
#----------------------------------------
\
\
${if_existing /proc/net/route ppp0}
${voffset -227}${goto 40}${color5}Up: ${color2}${upspeed ppp0}${color5}${goto 150}Down: ${color2}${downspeed ppp0}
${voffset 10}${goto 40}${upspeedgraph ppp0 26,80 d64937 d64937}${goto 150}${downspeedgraph ppp0 26,80 d64937 d64937}
${voffset 9}${goto 40}${color5}Sent: ${color2}${totalup ppp0}${color5}${goto 150}Received: ${color2}${totaldown ppp0}
${else}
${if_existing /proc/net/route ppp1}
${voffset -240}${goto 40}${color5}Up: ${color2}${upspeed ppp1}${color5}${goto 150}Down: ${color2}${downspeed ppp1}
${voffset 10}${goto 40}${upspeedgraph ppp1 26,80 d64937 d64937}${goto 150}${downspeedgraph ppp1 26,80 d64937 d64937}
${voffset 9}${goto 40}${color5}Sent: ${color2}${totalup ppp1}${color5}${goto 150}Received: ${color2}${totaldown ppp1}
${else}
${if_existing /proc/net/route wlp2s1}
${voffset -253}${goto 40}${color5}Up: ${color2}${upspeed wlp2s1}${color5}${goto 150}Down: ${color2}${downspeed wlp2s1}
${voffset 10}${goto 40}${upspeedgraph wlp2s1 26,80 d64937 d64937}${goto 150}${downspeedgraph wlp2s1 26,80 d64937 d64937}
${voffset 9}${goto 40}${color5}Sent: ${color2}${totalup wlp2s1}${color5}${goto 150}Received: ${color2}${totaldown wlp2s1}
${else}
${if_existing /proc/net/route wlp2s0}
${voffset -266}${goto 40}${color5}Up: ${color2}${upspeed wlp2s0}${color5}${goto 150}Down: ${color2}${downspeed wlp2s0}
${voffset 10}${goto 40}${upspeedgraph wlp2s0 26,80 d64937 d64937}${goto 150}${downspeedgraph wlp2s0 26,80 d64937 d64937}
${voffset 9}${goto 40}${color5}Sent: ${color2}${totalup wlp2s0}${color5}${goto 150}Received: ${color2}${totaldown wlp2s0}
${else}
${if_existing /proc/net/route wlan0}
${voffset -279}${goto 40}${color5}Up: ${color2}${upspeed wlan0}${color5}${goto 150}Down: ${color2}${downspeed wlan0}
${voffset 8}${goto 40}${upspeedgraph wlan0 26,80 d64937 d64937}${goto 150}${downspeedgraph wlan0 26,80 d64937 d64937}
${voffset 9}${goto 40}${color5}Sent: ${color2}${totalup wlan0}${color5}${goto 150}Received: ${color2}${totaldown wlan0}
${else}
${if_existing /proc/net/route wlan1}
${voffset -292}${goto 40}${color5}Up: ${color2}${upspeed wlan1}${color5}${goto 150}Down: ${color2}${downspeed wlan1}
${voffset 10}${goto 40}${upspeedgraph wlan1 26,80 d64937 d64937}${goto 150}${downspeedgraph wlan1 26,80 d64937 d64937}
${voffset 9}${goto 40}${color5}Sent: ${color2}${totalup wlan1}${color5}${goto 150}Received: ${color2}${totaldown wlan1}
${else}
${if_existing /proc/net/route eth1}
${voffset -305}${goto 40}${color5}Up: ${color2}${upspeed eth1}${color5}${goto 150}Down: ${color2}${downspeed eth1}
${voffset 10}${goto 40}${upspeedgraph eth1 26,80 d64937 d64937}${goto 150}${downspeedgraph eth1 26,80 d64937 d64937}
${voffset 9}${goto 40}${color5}Sent: ${color2}${totalup eth1}${color5}${goto 150}Received: ${color2}${totaldown eth1}
${else}
${if_existing /proc/net/route eth0}
${voffset -318}${goto 40}${color5}Up: ${color2}${upspeed eth0}${color5}${goto 150}Down: ${color2}${downspeed eth0}
${voffset 10}${goto 40}${upspeedgraph eth0 26,80 d64937 d64937}${goto 150}${downspeedgraph eth0 26,80 d64937 d64937}
${voffset 9}${goto 40}${color5}Sent: ${color2}${totalup eth0}${color5}${goto 150}Received: ${color2}${totaldown eth0}
${else}
${if_existing /proc/net/route enp0s0}
${voffset -331}${goto 40}${color5}Up: ${color2}${upspeed enp0s0}${color5}${goto 150}Down: ${color2}${downspeed enp0s0}
${voffset 10}${goto 40}${upspeedgraph enp0s0 26,80 d64937 d64937}${goto 150}${downspeedgraph enp0s0 26,80 d64937 d64937}
${voffset 9}${goto 40}${color5}Sent: ${color2}${totalup enp0s0}${color5}${goto 150}Received: ${color2}${totaldown enp0s0}
${else}
${if_existing /proc/net/route enp0s1}
${voffset -344}${goto 40}${color5}Up: ${color2}${upspeed enp0s1}${color5}${goto 150}Down: ${color2}${downspeed enp0s1}
${voffset 10}${goto 40}${upspeedgraph enp0s1 26,80 d64937 d64937}${goto 150}${downspeedgraph enp0s1 26,80 d64937 d64937}
${voffset 9}${goto 40}${color5}Sent: ${color2}${totalup enp0s1}${color5}${goto 150}Received: ${color2}${totaldown enp0s1}
${else}
${voffset -311}${goto 40}${color5}Network disconnected${color}
${image ~/.harmattan-assets/misc/Numix/God-Mode/offline.png -p 44,284 -s 16x16}
${endif}${endif}${endif}${endif}${endif}${endif}${endif}${endif}${endif}${endif}
\
\
#----------------------------------------
#   Weather icons
#----------------------------------------
\
\
${image ~/.harmattan-assets/misc/Numix/God-Mode/pressure.png -p 224,95 -s 16x16}\
${image ~/.harmattan-assets/misc/Numix/God-Mode/humidity.png -p 224,115 -s 16x16}\
${image ~/.harmattan-assets/misc/Numix/God-Mode/wind-2.png -p 224,136 -s 16x16}\
${execi 300 cp -f ~/.harmattan-assets/icons/#dcdcdc__32/$(jq .list[0].weather[0].id ~/.cache/forecast.json).png ~/.cache/weather-1.png}${image ~/.cache/weather-1.png -p 41,207 -s 32x32}\
${execi 300 cp -f ~/.harmattan-assets/icons/#dcdcdc__32/$(jq .list[1].weather[0].id ~/.cache/forecast.json).png ~/.cache/weather-2.png}${image ~/.cache/weather-2.png -p 119,207 -s 32x32}\
${execi 300 cp -f ~/.harmattan-assets/icons/#dcdcdc__32/$(jq .list[2].weather[0].id ~/.cache/forecast.json).png ~/.cache/weather-3.png}${image ~/.cache/weather-3.png -p 195,207 -s 32x32}${font}${voffset -120}\
{% endhighlight bash %}
