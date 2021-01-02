# bootstrap-table

## 引入
```
 <link rel="stylesheet" type="text/css" href="bootstrap.min.css" />
 <script src="jquery.min.js"></script>
 <script src="bootstrap.min.js"></script>
 <!-- 一个页面中js引用多次会出错 -->

<link href="https://cdn.bootcss.com/bootstrap-table/1.11.1/bootstrap-table.min.css" rel="stylesheet">
<script src="https://cdn.bootcss.com/bootstrap-table/1.11.1/bootstrap-table.min.js"></script>
<script src="https://cdn.bootcss.com/bootstrap-table/1.11.1/locale/bootstrap-table-zh-CN.min.js"></script>
```

## json数据表
```
<div>
   <table data-toggle="table" data-url="data.json">
      <thead>
       <tr>
           <th data-field="id">序号</th>
           <th data-field="name">名称</th>
       </tr>
     </thead>
   </table>
</div>
```


## 中文编码

bootstraptable刷新函数参数包含中文时，接收需要解码

String name = new String(userName.getBytes("iso-8859-1"),"utf-8");

## 页面

```
前台分页对应json
[
  {
    "id":1,
    "name":"张三",
    "age":22
  },
  ...
]
```
```
后台分页对应json
{
   "total":20,
   "rows":[
        {
          "id":1,
          "name":"张三",
        },
       ...
    ]
}
```


```
<div id="toolbar">
   <button id="quaryBtn" class="btn btn-success">查询</button>
</div>
<table id="myTable" class="table table-hover"></table>

function queryParamsFun(params)
{
   var id = 10;
   var temp = {
       limit: params.pageSize,
       page: params.pageNumber,
       id:id,
   };
   return temp;
}

/bootstrap-table要求服务器返回的json数据包含：total和rows
$('#myTable).bootstrapTable({
    url: 'GetPageData',
    method: "POST",
    queryParamsType: "",
    queryParams: queryParamsFun,
    contentType: "application/x-www-form-urlencoded",
    dataType:'json',
    toolbar: "#toolbar",  //使用html中div的id为toolbar
    striped: false,                       // 是否显示行间隔色
    uniqueId: "id",               //每一行的唯一标识，一般为主键列
    sortable: false,                     //是否启用排序
    sortOrder: "asc",                   //排序方式
    pagination: true,                   //是否显示分页             
    sidePagination: "server",           //分页方式：client客户端分页，server服务端分页（*）
    pageNumber:1,                       //初始化加载第一页，默认第一页
    pageSize: 5,                       //每页的记录行数
    pageList: [5, 10, 25, 50, 100],        //可供选择的每页的行数
    strictSearch: true,
    minimumCountColumns: 2,             //最少允许的列数
    clickToSelect: true,                //是否启用点击选中行
    height: 500,                        //行高，如果没有设置height属性，表格自动根据记录条数觉得表格高度
    showToggle:true,                    //是否显示详细视图和列表视图的切换按钮
    cardView: false,                    //是否显示详细视图,详细视图将每一行做成卡片
    detailView: false,                   //是否显示父子表

    //隐藏工具栏
    showSearch: false,
    showRefresh: false,
    showToggle: false,
    showColumns: false,
    
    columns:[
         {
            field: 'none',
            title: '操作',
            width: 20,
            align: 'center',
            valign: 'middle',
            formatter: actionFormatter,//定义操作栏按钮html字符串返回
        },
	     {title: 'ID',field: 'id',visible:true}, 
	     {title: '名称',field: 'name'}, 	
	]
});


$("#quaryBtn").click(function() {
  	$("#myTable").bootstrapTable('refresh',queryParamsFun);
});



function BtnFun(value,row){}
function actionFormatter(value, row, index) {
   //value:当前单元格的field
   //row:当前行的数据
   //index:当前行索引
   //参数可选择

    var valuestr = JSON.stringify(value).replace(/"/g, "'");
	 var rowstr = JSON.stringify(row).replace(/"/g, "'");
    var result = "<a href='javascript:;' class='btn btn-xs blue' onclick=\"BtnFun(" + valuestr + "," + rowstr + ")'title='编辑'><span class='glyphicon glyphicon-pencil'></span></a>";
    return result;
}
```

## 后端

### 实体类

```
public class Page implements Serializable {
   private Integer total; //bootstrap-table要求服务器返回的json数据包含：total和rows
   private List rows = new ArrayList();
}
```

### contoller

```
@RequestMapping(value = "/GetPageData", method = RequestMethod.POST)
public @ResponseBody Page getBarrelByPage(@RequestParam("page") Integer startPage, @RequestParam("limit") Integer limit, @RequestParam(id")String Id) {
   int start = (startPage - 1) * limit;
   Integer total = mapper.getcount(ID);
   List<Data> rows = mapper.getllimit(start, limit, devID);
   Page page = new Page();
   page.setTotal(total);
   page.setRows(rows);
   return page;
}
```

### mapper

```
<sql id="limit_sql">
	<where>
		<if test="id != null and id  != ''">
			<![CDATA[
				TempTableName.id=#{id}
			]]>
		</if>
	</where>
</sql>
<select id="getcount" parameterType="map" resultType="int">
	<![CDATA[
		select count(id) from TableName TempTableName
	]]>
	<include refid="limit_sql" />
</select>
<select id="getllimit" parameterType="map" resultType="map">
	<![CDATA[
		select * from TableName TempTableName
	]]>
	<include refid="limit_sql" />
	<![CDATA[
		ORDER BY id DESC LIMIT #{start}, #{limit}
	]]>
</select>
```
