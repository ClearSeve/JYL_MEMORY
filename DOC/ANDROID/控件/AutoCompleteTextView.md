# AutoCompleteTextView

```xml
    <AutoCompleteTextView
        android:background="@drawable/border"
        android:inputType="numberDecimal"
        android:id="@+id/datainputACTV"
        android:layout_width="100dp"
        android:layout_height="25dp"
        android:dropDownVerticalOffset="3dp"
        android:layout_weight="3"
        android:cacheColorHint="#00000000"
        android:completionThreshold="1"
        android:ems="1"
        android:gravity="center_vertical" >
        <requestFocus />
    </AutoCompleteTextView>
```

```
  ArrayList<String> list = new ArrayList<String>();
  list.add("18");
  list.add("19");
  list.add("20");
  ArrayAdapter<String>  adapter = new ArrayAdapter<String>(this.getContext(),android.R.layout.simple_dropdown_item_1line, list);
  datainputACTV.setAdapter(adapter);

   //显示提示框
  pointTempStartEt.showDropDown();
```
