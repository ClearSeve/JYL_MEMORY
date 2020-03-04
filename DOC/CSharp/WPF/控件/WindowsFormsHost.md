# WindowsFormsHost

```
集成winform窗口需要将winform程序生成为dll，且Form1类设置为public
需要引用WindowsFormsIntegration、System.Windows.Forms

Form1 form = new Form1();
form.FormBorderStyle = System.Windows.Forms.FormBorderStyle.None;
form.TopLevel = false;
wfhObj.Child = form;
```