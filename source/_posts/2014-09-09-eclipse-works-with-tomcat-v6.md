---
layout: post
title: 'Eclipse works with Tomcat v6.'
date: 2014-09-09 14:48
comments: true
categories:
---
此篇為設定 Tomcat 和 Eclipse 的一些紀錄。

<!--more-->

## **前提**
平台: Windows 8
IDE: [Eclispe for JavaEE](http://www.eclipse.org/downloads/packages/eclipse-ide-java-ee-developers/keplersr2)
安裝好 JRE ( Java Runtime Environment ) & JDK ( Java Software Development Kits )

## **Tomcat 6 安裝與設定**
Server Runtime Environment: [Tomcat 6](http://tomcat.apache.org/download-60.cgi)
請直接下載 .exe 安裝檔比較方便，下一步下一步安裝完成後
打開 Eclipse，點選 Window => Preferences 下的 Server 中的 Runtime Environments
再點選 Add，由於我們安裝的是 Tomcat 6，所以請選擇 Tomcat v6.0，接者只要選擇我們安裝 Tomcat 所選擇的安裝目錄(預設是在 C:\Program Files\Apache Software Foundation\Tomcat 6.0) 即可完成設定。

    **小記錄:**
    Maven project 的 Local Repository 預設是
    Windows 在 => C:\Documents and Settings\<username>\.m2
    OSX / Unix 在跟目錄下也就是 ~/.m2之下
    這裡面會放許多 Projects 會用到的 Dependecies
