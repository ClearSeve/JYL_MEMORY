# XML

```xml
<?xml version="1.0" encoding="utf-8"?>
<abc xmlns="*************"> //abc及其子元素属于名称空间，名称空间一般是一个地址
  <a></a>
  <x:b></ x:b>
</abc>
```

## 创建

```xml
<?xml version="1.0" encoding="utf-8" standalone="yes"?>
<rootNodeName>
  <NodeA>
    <NodeAa>aa</NodeAa>
  </NodeA>
  <NodeB attr="bb">
    <!--我是注释-->
  </NodeB>
</rootNodeName>
```

```
XmlDocument doc = new XmlDocument();
XmlDeclaration docDec = doc.CreateXmlDeclaratio("1.0", "utf-8", "yes");
doc.AppendChild(docDec);
XmlElement root = doc.CreateElement("rootNodeName");/创建根节点
doc.AppendChild(root);
XmlElement nodeA = doc.CreateElement("NodeA");//创建点
XmlElement nodeAa = doc.CreateElement("NodeAa");
nodeAa.InnerText = "aa";
nodeA.AppendChild(nodeAa);
root.AppendChild(nodeA);
XmlElement nodeB = doc.CreateElement("NodeB");//创建点
XmlAttribute attr =  doc.CreateAttribute("attr");
attr.Value = "bb";
nodeB.Attributes.Append(attr);
nodeB.AppendChild(doc.CreateComment("我是注释"));//插注释
root.AppendChild(nodeB);
```

## 读取

```
XmlDocument doc = new XmlDocument();　  
doc.Load("D:\\1.xml");

//获取节点
XmlNodeList nodeList =  doc.SelectNodes("//Object");//获取所有Object节点【xpath】
XmlNode root = doc.SelectSingleNode("root");//获取单个节点

string s = root.SelectSingleNode("a").InnerText; //获取节点a内的值
string ss = root.InnerText;  //会将根节点中所有节点的的值合并为一个字符串  

//获取节点属性
rootNode.Attributes["属性名"].Value;


//遍历根节点下的所有节点
if (rootNode.HasChildNodes)
{
    foreach (XmlNode node in rootNode)
    {
           string str = node.InnerText;
    }
}
```


## xmlns
```
对于带xmlns的节点，需要特殊处理

<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <startup>
    <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.6"/>
  </startup>
  <runtime>
    <assemblyBinding xmlns="urn:schemas-microsoft-com:asm.v1">
      <probing />
    </assemblyBinding>
  </runtime>
</configuration>


mlDocument docNode = new XmlDocument();
docNode.Load(xmlpathname);

XmlNamespaceManager xnm = new XmlNamespaceManager(docNode.NameTable);
xnm.AddNamespace("x", "urn:schemas-microsoft-com:asm.v1");//添加xml中节点上xmlns的名称空间
XmlNode node = docNode.SelectSingleNode("/configuration/runtime/x:assemblyBinding/x:probing", xnm);//节点带xmlns，子节点也必须带
```