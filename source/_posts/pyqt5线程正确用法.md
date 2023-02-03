---
title: pyqt5线程正确用法
date: 2023-01-31 10:31:02
tags: pyqt线程
categories: python
---
timer任务为Qthread里另一条线程，Qthread实质为管理线程的工具
<!--more-->

```python
from PyQt5.QtCore import QTimer
from PyQt5.QtCore import QThread,pyqtSignal,QObject
class Work(QObject):
    sinal = pyqtSignal()
    def __init__(self,total_page=20):
      super().__init__()

    def tim(self):
      print("timer")
      print(QThread.currentThreadId(),self.tes)
      self.time.stop()
    def dowork(self):
      self.time = QTimer()
      self.time.timeout.connect(self.tim)
      self.time.start(3)
      print(QThread.currentThreadId(),self.tes)
      self.sinal.emit()
    def test(self):
      print(QThread.currentThreadId())
      self.tes = "tset"
      

class cou(QObject):
    def __init__(self,total_page=20):
      super().__init__()
    
      self.work =  Work()
      
      self.threa = QThread()
      self.work.moveToThread(self.threa)
      
      self.threa.started.connect(self.work.dowork)
      self.work.sinal.connect(self.sinal)
    def sinal(self):
      print("sinal")
      #timer任务为Qthread里另一条线程，Qthread实质为管理线程的工具
```
      
   