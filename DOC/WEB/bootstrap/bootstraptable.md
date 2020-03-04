# bootstrap-table

## 中文编码

bootstraptable刷新函数参数包含中文时，接收需要解码

String name = new String(userName.getBytes("iso-8859-1"),"utf-8");

## 前台

```
 <link rel="stylesheet" type="text/css" href="bootstrap.min.css" />
 <link rel="stylesheet" type="text/css" href="bootstrap-table.min.css">
 <script src="jquery.min.js"></script>
 <script src="bootstrap.min.js"></script>
 <!-- 一个页面中js引用多次会出错 -->
 <script src="bootstrap-table.min.js"></script>
 <script src="bootstrap-table-zh-CN.min.js"></script>


<table id="myTable" class="table table-hover"></table>


function queryParamsFun(params)
{
        var id = $("#barrelDevidQuery").val();
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
    toolbar: "#toolbar",
    striped: true,                       // 是否显示行间隔色
    uniqueId: "id",               //每一行的唯一标识，一般为主键列
    sortable: false,                     //是否启用排序
    sortOrder: "asc",                   //排序方式
    pagination: true,                   //是否显示分页             
    sidePagination: "server",           //分页方式：client客户端分页，server服务端分页（*）
    pageNumber:1,                       //初始化加载第一页，默认第一页
    pageSize: 5,                       //每页的记录行数
    pageList: [5, 10, 25, 50, 100],        //可供选择的每页的行数
    strictSearch: true,
    showColumns: true,                  //是否显示选择列按钮
    showRefresh: true,                  //是否显示刷新按钮
    minimumCountColumns: 2,             //最少允许的列数
    clickToSelect: true,                //是否启用点击选中行
    height: 500,                        //行高，如果没有设置height属性，表格自动根据记录条数觉得表格高度
    showToggle:true,                    //是否显示详细视图和列表视图的切换按钮
    cardView: true,                    //是否显示详细视图
    detailView: false,                   //是否显示父子表
    columns:[
    {
            field: 'none',
            title: '操作',
            width: 120,
            align: 'center',
            valign: 'middle',
            formatter: actionFormatter,
        },
	{title: 'IP',field: 'ip',visible:true}, 
	{title: 'port',field: 'port'}, 	
	{title: 'ID',field: 'id'},
	]
});


  $("#quaryBtn").click(function() {
  	$("#myTable").bootstrapTable('refresh',queryParamsFun);
});



function actionFormatter(value, row, index) {
//在数据刷新时调用，将数值固定写入到了result
//value:当前单元格的field
//row:当前行的数据
//index:当前行索引
	var val = JSON.stringify(row).replace(/\"/g,"'");
    var result = "";
    result += "<a href='javascript:;' class='btn btn-xs blue' onclick=\"EditView(" + value + "," + val + ")\" title='编辑'><span class='glyphicon glyphicon-pencil'></span></a>";
    return result;
}
function EditView(value,row){}
```

## 后端

### 实体类

```
public class Page implements Serializable {
   private static final long serialVersionUID = 1L;
   private Integer total; //bootstrap-table要求服务器返回的json数据包含：total和rows
   private List rows = new ArrayList();
   public Integer getTotal() {return total;}
   public void setTotal(Integer total) {this.total = total;}
   public List getRows() {return rows;}
   public void setRows(List rows) {this.rows = rows;}
}
```

### contoller

```
@RequestMapping(value = "/GetPageData", method = RequestMethod.POST)
public @ResponseBody Page getBarrelByPage(@RequestParam("page") Integer startPage, @RequestParam("limit") Integer limit, @RequestParam(id")String Id) {
   return service.getByPage(startPage, limit, Id);
}
```

### service

```
public Page getByPage(Integer startPage, Integer limit,String ID)
{
   int start = (startPage - 1) * limit;
   Integer total = mapper.getcount(ID);
   List<SaveData> rows = mapper.getllimit(start, limit, devID);
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
