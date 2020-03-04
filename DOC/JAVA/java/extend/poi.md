# poi

```
import java.io.FileOutputStream;  
import org.apache.poi.hssf.usermodel.HSSFCell;  
import org.apache.poi.hssf.usermodel.HSSFCellStyle;  
import org.apache.poi.hssf.usermodel.HSSFRow;  
import org.apache.poi.hssf.usermodel.HSSFSheet;  
import org.apache.poi.hssf.usermodel.HSSFWorkbook;  


/创建一个webbook，对应一个Excel文件  
HSSFWorkbook wb = new HSSFWorkbook();  

//格式
HSSFCellStyle style = wb.createCellStyle();  

//在webbook中添加一个sheet,对应Excel文件中的sheet  
HSSFSheet sheet = wb.createSheet("表一");  


//在sheet中第1行第一列加入数据xx
HSSFRow row = sheet.createRow(0);  
HSSFCell cell = row.createCell(0);  
cell.setCellValue("xxx");  
cell.setCellStyle(style); //设置单元格格式


//保存文件
try  
{  
    FileOutputStream fout = new FileOutputStream("D:/abc.xls");  
    wb.write(fout);  
    fout.close();  
}  
catch (Exception e)  
{  
    e.printStackTrace();  
}  
```
