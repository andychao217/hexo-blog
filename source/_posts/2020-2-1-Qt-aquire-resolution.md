---
layout: post
title: Qt获取分辨率
date: 2020-02-01 12:00:00
categories:
    - C++
    - Qt
tags:
    - C++
    - Qt
---

```C++
void GLWidget::getScreenInfo()  //得到当前计算机的屏幕分辨率
{
    QDesktopWidget* desktopWidget = QApplication::desktop();
    QRect screenRect = desktopWidget->screenGeometry();
    g_nActScreenW = screenRect.width();
    g_nActScreenH = screenRect.height();
}
```

```C++
    //得到当前控件相对于屏幕的坐标
    widgetPos = ui->startDateTimeValue->mapToGlobal(ui->startDateTimeValue->pos());
    widgetW = widgetPos.x();
    widgetH = widgetPos.y();
    
    //控件相对于软件窗口的坐标
    ui->startDateTimeValue->pos()
```
