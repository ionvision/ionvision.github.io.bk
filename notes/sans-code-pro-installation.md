---
title: Install Adobe Source Code Pro font on Ubuntu
layout: post
---

Adobe Source Code Pro font is nice!
=========

##Introduction
`Adobe Source Code Pro` is a set of monospaced OpenType fonts designed for coding environments. It's open source and totally free. It can be downloaded [here](http://store1.adobe.com/cfusion/store/html/index.cfm?event=displayFontPackage&code=1960).

---------------

##Installation

[#Reference#](http://askubuntu.com/questions/193072/how-to-use-the-new-adobe-source-code-pro-font)

1.Download the archive from here. You can do it also using wget: Open a terminal (ctrl-alt-t or press the win key and type "terminal") and type

```bash
wget http://downloads.sourceforge.net/project/sourcecodepro.adobe/SourceCodePro_FontsOnly-1.017.zip
```
---------------

2.Unzip the archive (you can use Nautilus for that, or use the following command).

```bash
unzip SourceCodePro_FontsOnly-1.017.zip
```

---------------

3.Create a directory in your home directory called ".fonts" (either go to home in Nautilus and create a new folder, or type the following from the terminal)

```bash
mkdir -p ~/.fonts
```

If you already have that directory, don't worry.

---------------

4.Move the Open Type fonts (*.otf) to the newly created .fonts directory. In command line, that would be

```bash
cp SourceCodePro_FontsOnly-1.017/OTF/*.otf ~/.fonts/
```
---------------

5.If you haven't done it yet, open a terminal, and type
```
fc-cache -f -v
```

Your font is now ready to use and the applications should be able to see it.

---------------

##Note

All in one script for those who simply want to copy/paste the answer

```bash
#!/bin/bash
mkdir /tmp/adodefont
cd /tmp/adodefont
wget http://downloads.sourceforge.net/project/sourcecodepro.adobe/SourceCodePro_FontsOnly-1.017.zip
unzip SourceCodePro_FontsOnly-1.017.zip
mkdir -p ~/.fonts
cp SourceCodePro_FontsOnly-1.017/OTF/*.otf ~/.fonts
fc-cache -f -v
```
If you want to install system wide instead of per user, copy the files to `/usr/local/share/fonts/`
instead of `~/.fonts/`.
