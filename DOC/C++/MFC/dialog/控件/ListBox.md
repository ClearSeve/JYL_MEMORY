# List Box

```
绑Control类型
样式的选择里，可设置单个或者多个的选择，无法通过属性添加数据
listbox.SetItemHeight(index,30);//设置列表高度
if(LB_ERR==m_list.FindString(-1,cstring)){
 //搜索判断类表中所有项目名称，如果没有名称cstring则执行
     int nIndex=m_list.AddString(cstring);
     CString   *pPath=new  CString;
         //为list添加附加数据（比如添加地址，后面可以取出）
        //如果添加的数字是数字，就可以直接加入。。不用这个方式
     *pPath=m_strFilePath;
      m_list.SetItemData(nIndex,(DWORD)pPath);
}  
找到列表框IDC_LIST的LBN_DBLCLK（双击消息），然后进入相应的处理函数取出pPath：
int  nSel=m_list.GetCurSel();
if(LB_ERR==nSel) return;
else{
   CString  *pPath=(CString *)m_list.GetItemData(nSel);
}
new出的pPath需要考虑释放，使用m_list.GetCount()统计出数量，在for循环中delete掉从GetItemData中拿出的附加数据
int  count=m_list.GetCount();
for(int i=0;i<count;i++){
  CString *pPath=(CString *)m_list.GetItemData(i);
  delete  pPath;}
m_list.GetText(nSel,str);可获得列表框文本
m_list.DeleteString(nSel); 将列表框中某行删除
```
