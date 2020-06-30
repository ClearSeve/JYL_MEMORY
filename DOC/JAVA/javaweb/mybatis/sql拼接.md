# sql拼接

```
使用<![CDATA[ ……]]>  可以在其中包含小于号
    

<select id="函数名"  resultType="输出参数类型"  parameterType="输入参数类型" >
        <![CDATA[  查询语句   ]]> 
</select>

#{}将传入的参数当成一个字符串，会给传入的参数加一个双引号,能够很大程度上防止sql注入
${}将传入的参数直接显示生成在sql中，不会添加引号


<sql id="limit_sql">
     <where>
		<if test="id != null and  id != ''">
			TempTableName.id=#{id} 
		</if>
    </where>
</sql>
<select id="getlimit" parameterType="map" resultType="map">
	    select * from  TempTableName
		<include refid="limit_sql" />
		ORDER BY  id DESC LIMIT #{start}, #{limit}
</select>
```

```
大于：&gt;

小于：&lt;

大于等于：&gt;=

小于等于：&lt;=
```