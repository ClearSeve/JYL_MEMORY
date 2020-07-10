# thymeleaf

## 变量获取
```
<script th:inline="javascript">
    /*<![CDATA[*/
    var cid = /*[[${cid}]]*/;
    /*]]>*/
</script>
```

## 元素显示

```
<div th:if="${param.show}">
    show or hide
</div>
```

## 表单跳转
```
<form th:action="@{/logout}" method="post">
    <input type="submit" value="Sign Out"/>
</form>
```