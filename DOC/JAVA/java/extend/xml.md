# xml

```
<dependency>
    <groupId>jdom</groupId>
    <artifactId>jdom</artifactId>
    <version>1.1</version>
</dependency>


import java.io.*;
import org.jdom.*;
import org.jdom.input.*;
  
SAXBuilder builder = new SAXBuilder();  
Document doc = builder.build(new File("cfg.xml"));
Element root = doc.getRootElement();
String Ip = root.getChild("Ip").getValue();
String port = root.getChild("port").getText();
```
