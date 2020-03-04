# Fragment

不包含view，没有findViewById函数  

需要在onCreateView中通过view调用findViewById函数  
```
@Override
public View onCreateView(LayoutInflater inflater, ViewGroup container,
                Bundle savedInstanceState) {
    View vv =  inflater.inflate(R.layout.****, container, false);

｝
```