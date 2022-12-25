---
title: setupUi()函数
date: 2022-12-04 15:00:14
tags: pyqt
categories: pyqt
id: setupUi()函数
---

qt提供了工具以拖拽控件的方式的简化了Ui代码的编写，在qtcreator里设计好ui后会自动生成对应qml文件，qml会被翻译成C++代码。同样地，qml文件也可以翻译成python代码，然后pyqt里调用翻译出来的ui.py代码。<!--more-->
可以在cmd通过命令方式把qml文件翻译成python代码, 例如把Ui.ui文件翻译成Ui.py。
```bash
pyuic5 Ui.ui > Ui.py
```
Ui.ui的效果如图所示，其中只有一个登录按钮。
![](/img/login.png)
Ui.py文件内容如下：
```python
from PyQt5 import QtCore, QtGui, QtWidgets
class Ui(object):
    def setupUi(self, FormLogin):
        FormLogin.setObjectName("FormLogin")
        FormLogin.resize(400, 300)
        sizePolicy = QtWidgets.QSizePolicy(QtWidgets.QSizePolicy.Fixed, QtWidgets.QSizePolicy.Fixed)
        sizePolicy.setHorizontalStretch(0)
        sizePolicy.setVerticalStretch(0)
        sizePolicy.setHeightForWidth(FormLogin.sizePolicy().hasHeightForWidth())
        FormLogin.setSizePolicy(sizePolicy)
        FormLogin.setMinimumSize(QtCore.QSize(400, 300))
        FormLogin.setMaximumSize(QtCore.QSize(400, 300))
        self.btnLogin = QtWidgets.QPushButton(FormLogin)
        self.btnLogin.setGeometry(QtCore.QRect(110, 120, 181, 31))
        self.btnLogin.setObjectName("btnLogin")

        self.retranslateUi(FormLogin)
        QtCore.QMetaObject.connectSlotsByName(FormLogin)

    def retranslateUi(self, FormLogin):
        _translate = QtCore.QCoreApplication.translate
        FormLogin.setWindowTitle(_translate("FormLogin", "IM"))
        self.btnLogin.setText(_translate("FormLogin", "登  录"))
```
> 重点看其中 QtCore.QMetaObject.connectSlotsByName(FormLogin)函数，此函数的作用是根据槽函数的函数名称把UI控件发出的信号连接到槽函数。

以登录按钮控件举例，如果通过connectSlotsByName(FormLogin)函数把点击信号与某一函数自动链接，则此函数命名遵循一定规则。
例如函数def on_btnLogin_clicked(self)，on表示这是一个槽函数，btnLogin是登录控件对象的名字(对象名字需要唯一)，clicked表示登录控件发出的点击信号,而且需要@pyqtSlot()装饰器装饰槽函数。
完整的例子代码如下：
```python
from PyQt5.QtWidgets import QApplication
import sys
from PyQt5.QtCore import pyqtSlot
from PyQt5.QtWidgets import QWidget
from Ui import Ui
class test(QWidget,Ui):
    def __init__(self):
        super().__init__()
        self.setupUi(self)
        self.btnLogin.clicked.connect(self.lin)
    @pyqtSlot()
    def on_btnLogin_clicked(self):
        print("hello")
    def lin(self):
        print("lin")
if __name__ == '__main__':
    app = QApplication(sys.argv)
    ui = test()
    ui.show()
    app.exec_()
```
也可以手动连接信号与槽函数。
```python
self.btnLogin.clicked.connect(self.lin)
```
> 另外在qtcreator（c++）里面可以右键控件点击转到槽，可以在正确位置生成相应的槽函数，前提是Ui界面类的.ui、.h、.cpp文件名需要保持一致。qtcreateor通过相同的文件名确定槽函数的正确位置（即生成的槽函数应该位于哪个源文件）。槽函数名规则跟上述一致：“on_控件对象名_控件信号”。如下图所示：

![](/img/qt1.png)
![](/img/qt2.png)
![](/img/qt3.png)
![](/img/qt4.png)