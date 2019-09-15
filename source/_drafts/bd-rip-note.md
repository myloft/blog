---
title: BDMV 压制流程笔记
date: 2019-07-22 10:20:45
updated: 2019-07-27 08:59:45
tags: BDrip
---
　　最近加入了 BD 压制组，接触了动画压制。与想象中复杂的操作不同，仰赖一代代大佬的努力，压制工作的自动话程度已经非常高了，安装好需要的工具，将 BDMV 拖动到脚本上就可以全自动压制了。压制过程中 CPU 全核占有率达到了 100%，温度也达到了 99℃，趁这个时间读一读脚本，了解下压制的流程（可能存在很多问题）。
　　由于用的是自动化脚本，首先脚本遍历 BDMV 路径，为找到的 m2ts 文件创建相应的 vpy 脚本。
　　首先 import 所需要的库，包括 VapourSynth 和 mawen1250's VapourSynth functions 以及预处理脚本。
``` python
import vapoursynth as vs
import mvsfunc as mvf
import default
```
　　接着初始化 VapourSynth，并载入所需要处理的 m2ts 文件。
``` python
source = "00000.m2ts"
src=core.lsmas.LWLibavSource("00000.m2ts", threads=1)
```
　　随后就是进行视频预处理，转换为 10 bit 进行去色带和降噪。
``` python
src=default.Denoise(src)
res=default.Deband(src).fmtc.bitdepth(bits=10)
```
123