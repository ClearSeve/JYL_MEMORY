tkinter
=======

```
#coding=utf-8  
import tkinter as tk  
import threading  
import tkinter.messagebox as tkMB  
import tkinter.filedialog as tkFD  
  
  
class Application(tk.Frame):  
def __init__(self, master=None):  
super().__init__(master)  
  
self.pack() #pack布局
#可以将pack全部换成grid grid(row=1) grid(row=0,column=1)
self.create_widgets()  
def create_widgets(self):  
self.L = tk.Label(self,text="txt", fg="black", bg="white")  
self.L.pack()  
  
self.E = tk.Entry(self)  
self.E.pack()  
  
self.Etxt = tk.StringVar()  
self.Etxt.set("xxx")  
self.E["textvariable"] = self.Etxt  
  
  
self.BtnRun = tk.Button(self,width=15, height=5,text = "Run",command =
self.__Run)  
self.BtnRun.pack(padx=20, side='left')  
def __Run(self):  
filePathName = tkFD.askopenfilename(filetypes=[('all files', '.\*'),
('text files', '.txt')])  
self.L['text'] = filePathName  
  
_thread = threading.Thread(target=self.__thrRun)  
_thread.setDaemon(True)  
_thread.start()  
def __thrRun(self):  
ss = self.Etxt.get()  
tkMB.showinfo("提示", ss)  
self.BtnRun['state'] = tk.NORMAL  
  
root = tk.Tk()  
root.title('title')  
root.geometry('400x200')  
root.maxsize(400, 200)  
root.minsize(400, 200)  
app = Application(master=root)  
app.mainloop()
```

设置窗口大小和位置
------------------

root.title('窗口名')

root.geometry('600x400') *\#设置了主窗口的初始大小600x400*

root.maxsize(600, 400)

root.minsize(300, 240)

设置窗口居中
------------

def center_window(root, width, height):  
screenwidth = root.winfo_screenwidth()  
screenheight = root.winfo_screenheight()  
size = '%dx%d+%d+%d' % (width, height, (screenwidth - width)/2,
(screenheight - height)/2)  
root.geometry(size)

center_window(root, 300, 240)

控件 
-----

### 按钮

属性名进行设置

tk.Button(text="运行", fg="red",command=self.fun,width = 20,height = 10)

### 文本标签

self.l1 = tk.Label(text="txt", fg="black", bg="white")

self.l1["text"]="xxx"

### 编辑框

self.editTxt = tk.Entry() *\#创建文本控件*  
self.editTxt.pack()  
  
self.contents = tk.StringVar() *\#和Entry控件绑定的变量*  
self.contents.set("this is a variable")  
self.editTxt["textvariable"] = self.contents  
  
ss = self.contents.get() *\#获取字符串*

self.editTxt.bind('\<Enter\>',self.fun1) *\#绑定Enter事件到函数 def
fun1(self, event):*

### 进度条

scale=Scale(top,from_=10,to=40,orient=HORIZONTAL,command=fun)  
scale.set(12) *\#设置起始位置*  
scale.pack(fill=X,expand=1)

def fun(ev=None):

控件可用
--------

self.BtnRun['state'] = tk.DISABLED

self.BtnRun['state'] = tk.NORMAL

Ttk
---

from tkinter.ttk import 

可以取代（from Tkinter import \*）tk中的button等控件

self.l1 = Label(text="Test", style="BW.TLabel")

文本可以让底色透明

弹出对话框
----------

```
from tkinter import  *
  
def dialog():  
win = Toplevel()  
Label(win,text="abc").pack()  
win.focus_set()  
win.grab_set()  
win.wait_window()  
  
root = Tk()  
Button(root,text="btn",command=dialog).pack()  
root.mainloop()
```

特殊对话框
----------

Lib\\tkinter\\

filedialog.py

messagebox.py

### 文件和文件夹

import tkinter.filedialog as tkFD

选择文件对话框

filePathName =tkFD.askopenfilename(filetypes=[("图像", ".jpg")])

[('all files', '.\*'), ('text files', '.txt')]

文件夹选择对话框

dir = tkFD.askdirectory()

### 消息框

import tkinter.messagebox as tkMB

tkMB.showinfo(**"提示"**,**"处理完成"**)
