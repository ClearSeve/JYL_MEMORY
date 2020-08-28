# poi

```
<dependency>
  <groupId>org.apache.poi</groupId>
  <artifactId>poi</artifactId>
  <version>4.1.2</version>
</dependency>
<dependency>
  <groupId>org.apache.poi</groupId>
  <artifactId>poi-ooxml</artifactId>
  <version>4.1.2</version>
</dependency>
```


## 创建
```
Workbook workbook = new XSSFWorkbook();
// xls
//Workbook workbook = new HSSFWorkbook(); 
```

## 打开
```
FileInputStream ifs = new FileInputStream(pathName);
Workbook workbook = new XSSFWorkbook(ifs); 
//xls
//Workbook workbook = new HSSFWorkbook(ifs); 
ifs.close();
```

## 保存workbook
```
FileOutputStream outputStream = new FileOutputStream(pathName);
workbook.write(outputStream);
workbook.close();
outputStream.close();
```


## Sheet页
```
Sheet sheet = workbook.createSheet("sheet");
Sheet sheet = workbook.getSheet("sheet");
```

## 写入单元格
```
Row row = sheet.createRow(1);
int col = 0;
Cell cell = row.createCell(col);
cell.setCellValue("adf");
sheet.autoSizeColumn(col);
```

## sheet页样式

+ 单元格宽高
```
sheet1.setColumnWidth(col, 9000);
sheet.autoSizeColumn(col);
row.setHeight((short)500);
```

+ 合并单元格
```
int startRow = 0;
int endRow = 0;
int startCol = 0;
int endCol = 5;
CellRangeAddress region = new CellRangeAddress(startRow, endRow, startCol, endCol);
sheet.addMergedRegion(region);
```



## 单元格样式
+ 设置
```
CellStyle style = workbook.createCellStyle();
cell.setCellStyle(style);
```

+ 对齐
```
style.setAlignment(HorizontalAlignment.CENTER);
style.setVerticalAlignment(VerticalAlignment.CENTER);
```

+ 单元格颜色
```
style.setFillForegroundColor(IndexedColors.LIGHT_YELLOW.getIndex());
style.setFillPattern(FillPatternType.SOLID_FOREGROUND);

//底部边框
style.setBorderBottom(BorderStyle.THIN);
style.setBottomBorderColor(IndexedColors.BLACK.index);
```

+ 字体
```
Font font = workbook.createFont();
font.setFontName("宋体");
font.setFontHeightInPoints((short)28);
font.setItalic(false);
font.setBold(true);
font.setColor(IndexedColors.RED.index);
style.setFont(font);
```  