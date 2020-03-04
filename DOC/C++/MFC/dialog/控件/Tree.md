# Tree

## 操作说明

```
风格设置为has Buttons和Lines at root

HTREEITEM item; //存储插入数据后返回的节点句柄
item=m_tree.InsertItem(“节点”); //只有一个数据代表插入节点数据
item=m_tree.InsertItem(“数据”,item); //代表在指代的节点下插入数据

m_tree.SetItemData(item,1234);//加入附加数据，需要数字，可以用(DWORD)pName加入一个指向堆对象的CString

m_tree.DeleteAllItems();//删除所有节点

m_tree.SelectItem(item);

HTREEITEM   hChildItem   =   m_tree.GetChildItem(item);   树控件如果没有底层目录了，值就为空
```

## 取出附加数据

```
为树控件增加NM_CLICK消息处理

HTREEITEM   hSelectItemOld   =   m_tree.GetSelectedItem();
CPoint   point;   
GetCursorPos(&point);   
m_tree.ScreenToClient(&point); 
	
UINT   flag  =   TVHT_ONITEM ;   
HTREEITEM   hItem   =   m_tree.HitTest(point);   
if(hItem   !=   NULL  &&  hSelectItemOld   !=   hItem)   
m_tree.SelectItem(hItem); 
HTREEITEM childTree=  m_tree.GetChildItem(hItem);
	
if (!childTree)
{
   //附加数据是字符串时
	CString *str=(CString*)m_tree.GetItemData(hItem);	
	//附加数据是数字时
   int  i=(int)m_tree.GetItemData(hItem);

}
```

## 循环插入实例

### 数据

```
有如下向量，每个数据带有两个CString的结构，且内部数据是按照根相同的数据连续放置。
#include <vector>
using namespace std;
typedef struct Data
{
	CString type;  
	CString name;   
}Data;
typedef vector<Data>  VectorData;


VectorData  v;


Data d1;
d1.type="蔬菜";
d1.name="白菜";
v.push_back(d1);

Data d2;
d2.type="蔬菜";
d2.name="土豆";
v.push_back(d2);

Data d3;
d3.type="水果";
d3.name="葡萄";
v.push_back(d3);

Data d4;
d4.type="零食";
d4.name="方便面";
v.push_back(d4);
```

### 插入树

```
HTREEITEM  root; //指代根节点，用来判断是否需要插入新的根   
CString rootRepeate=_T(""); //用来存储每次根节点数据，用来判断数据是否和刚才插入的根相同
for(int i=0;i<v.size();i++)  //循环加入数据
{	
    //取出数据
	Data data=v[i];
	CString name=data.name;
	CString type=data.type;
	/////////为树插入节点和数据
	if (type!=rootRepeate)  //type是每次取出的代表根的数据，如果和上次节点不同，就插入新的节点，如果相同，就在上次节点后继续插入（需要取出的数据按规律排）
	{
		root=m_tree.InsertItem(type); //如果插入新根节点，就将root指向新的节点
	}
	HTREEITEM leaf=m_tree.InsertItem(name,root); //在根节点下插入数据
	
	CString *pName=new CString;
	*pName=name;
	m_tree.SetItemData(leaf,(DWORD)pName);//附加数据，如果是数字，可以直接写
	rootRepeate=type;
}
```