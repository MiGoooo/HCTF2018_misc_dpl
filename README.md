# HCTF2018_misc_dpl(24solves)
writeup of difficult_programming_language

这是我第一次参与CTF的出题，出的不是很好，看到有些师傅说过于脑洞和套路化了，在这里先向各位师傅道个歉orz

这题我给了一个usb流量包，用tshark命令可以将Leftover Capture Date域中的usb协议数据提取出来

>tshark -r difficult_programming_language.pcap -T fields -e usb.capdata > usbdata.txt

提取出数据后我们通过[usb协议文档](https://www.usb.org/sites/default/files/documents/hut1_12v2.pdf)可以查找到值和键位的关系，编写脚本后得到结果

>D'\`;M?!\mZ4j8hgSvt2bN);^]+7jiE3Ve0A@Q=|;)sxwYXtsl2pongOe+LKa'e^]\a\`_X|V[Tx;:VONSRQJn1MFKJCBfFE>&<`@9!=<5Y9y7654-,P0/o-,%I)ih&%$#z@xw|{ts9wvXWm3~

联系题目 `difficult programming language` ,可以搜索到这是一段`malbolge`语言的代码，找一个[在线编译网站](https://zb3.me/malbolge-tools/)跑一下即可得到flag.

>hctf{m4lb0lGe}

附上脚本
```
#!/usr/bin/env python

import sys
import os

normalkeys = { 0x04:"a",  0x05:"b",  0x06:"c", 0x07:"d", 0x08:"e", 0x09:"f", 0x0A:"g",  0x0B:"h", 0x0C:"i",  0x0D:"j", 0x0E:"k", 0x0F:"l", 0x10:"m", 0x11:"n",0x12:"o",  0x13:"p", 0x14:"q", 0x15:"r", 0x16:"s", 0x17:"t", 0x18:"u",0x19:"v", 0x1A:"w", 0x1B:"x", 0x1C:"y", 0x1D:"z",0x1E:"1", 0x1F:"2", 0x20:"3", 0x21:"4", 0x22:"5",  0x23:"6", 0x24:"7", 0x25:"8", 0x26:"9", 0x27:"0", 0x28:"\n", 0x2a:"[DEL]",  0X2B:"    ", 0x2C:" ",  0x2D:"-", 0x2E:"=", 0x2F:"[",  0x30:"]",  0x31:"\\", 0x32:"`", 0x33:";",  0x34:"'",0x35:"`", 0x36:",",  0x37:"." , 0x38:"/"}

shiftkeys = { 0x04:"A",  0x05:"B",  0x06:"C", 0x07:"D", 0x08:"E", 0x09:"F", 0x0A:"G",  0x0B:"H", 0x0C:"I",  0x0D:"J", 0x0E:"K", 0x0F:"L", 0x10:"M", 0x11:"N",0x12:"O",  0x13:"P", 0x14:"Q", 0x15:"R", 0x16:"S", 0x17:"T", 0x18:"U",0x19:"V", 0x1A:"W", 0x1B:"X", 0x1C:"Y", 0x1D:"Z", 0x1E:"!", 0x1F:"@", 0x20:"#", 0x21:"$", 0x22:"%",  0x23:"^", 0x24:"&", 0x25:"*", 0x26:"(", 0x27:")", 0x28:"\n", 0x2a:"[DEL]",  0X2B:"    ", 0x2C:" ",  0x2D:"_", 0x2E:"+", 0x2F:"{",  0x30:"}",  0x31:"|", 0x32:"~", 0x33:":",  0x34:"\"",0x35:"~", 0x36:"<",  0x37:">", 0x38:"?" }

nums = []
shift_press = []
keys = open('usbdata.txt')
for line in keys:
    shift_press.append(line[1])
    nums.append(int(line[6:8],16))
keys.close()

output = ""
m = 0
for n in nums:
    if n == 0 :
        m += 1
        continue
    if n in shiftkeys:
        if shift_press[m] == '2' : #shift is pressed
            output += shiftkeys[n]
            m += 1 
        elif shift_press[m] == '0' :
            output += normalkeys[n]
            m += 1
print 'output :\n' + output
```