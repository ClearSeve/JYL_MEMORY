# ListView

## 字符串数据

```
String[] strArr = new String[] { "AA", "BB","CC"};//或者list
ArrayAdapter<String> adapter = new ArrayAdapter<String>(getActivity(), android.R.layout.simple_list_item_1, strArr);
lv.setAdapter(adapter); //主窗口直接用this，Fragment用getActivity()


lv.setOnItemClickListener(new AdapterView.OnItemClickListener() {
    @Override
    public void onItemClick(AdapterView<?> parent, View view, final int position, long id) {

    }
});
```

## DataGrid

N行2列的datagrid

### layout

创建dgmodel.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="horizontal" >
    <TextView
        android:id="@+id/tv1"
        android:gravity="center"
        android:layout_weight="1"
        android:layout_width="0dp"
        android:layout_height="50dp"
        android:text="" />
    <TextView
        android:id="@+id/tv2"
        android:gravity="center"
        android:layout_weight="1"
        android:layout_width="0dp"
        android:layout_height="50dp"
        android:text="" />
</LinearLayout>
```


### MyAdapter.java

```
package com.jyl.netassist;

import java.util.ArrayList;
import java.util.List;
import android.content.Context;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.BaseAdapter;
import android.widget.TextView;


public class MyAdapter extends BaseAdapter{
    int itemCount = 2;
    protected Context context;
    protected LayoutInflater inflater;
    protected int resource;
    protected ArrayList<String> list;
    public MyAdapter(Context context, int resource, ArrayList<String> list){
        inflater = LayoutInflater.from(context);
        this.context = context;
        this.resource = resource;
        if(list==null){
            this.list=new ArrayList<>();
        }else{
            this.list = list;
        }
    }
    @Override
    public int getCount() {
        if(list.size()%itemCount>0) {
            return list.size()/itemCount+1;
        } else {
            return list.size()/itemCount;
        }
    }
    @Override
    public Object getItem(int position) {
        return list.get(position);
    }
    @Override
    public long getItemId(int position) {
        return position;
    }
    @Override
    public View getView(int position, View convertView, ViewGroup parent) {
        ViewHolder vh = null;
        if (convertView == null ) {
            convertView = inflater.inflate(resource, null);
            vh = new ViewHolder();
            vh.tv1=(TextView)convertView.findViewById(R.id.tv1);
            vh.tv2=(TextView)convertView.findViewById(R.id.tv2);
            convertView.setTag(vh);
        }else {
            vh = (ViewHolder)convertView.getTag();
        }
        int distance =  list.size() - position*itemCount;
        int cellCount = distance >= itemCount? itemCount:distance;
        final List<String> itemList = list.subList(position*itemCount,position*itemCount+cellCount);
        if (itemList.size() >0) {
            vh.tv1.setText(itemList.get(0));
            if (itemList.size() >1) vh.tv2.setText(itemList.get(1));
            else vh.tv2.setText("");
        }
        return convertView;
    }
    public static class ViewHolder{
        TextView tv1;
        TextView tv2;
    }
}
```

### 使用

```
ArrayList<String> listData = new ArrayList<String>();
MyAdapter adapter = new MyAdapter(context,R.layout.dgmodel,listData);
lvhead.setAdapter(adapter);
```
