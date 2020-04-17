# EPPlus

## 创建/打开

ExcelPackage _pakage = new ExcelPackage(new FileInfo(excelFilePathName));

## 选择sheet页

 ExcelWorksheet _curSheet = _pakage.Workbook.Worksheets[1];  
 _curSheet = m_pakage.Workbook.Worksheets[sheetName];

## 创建sheet页

_curSheet = _pakage.Workbook.Worksheets.Add(sheetName);

## 保存

_pakage.Save();

## 数据操作

+ 获取行列数  
 int rowMax = _curSheet.Dimension.End.Row;  
 int colMax = _curSheet.Dimension.End.Column;
+ 获取单元格数据  
string val = (string)_curSheet.GetValue(row, col);  
if (null == val) val = "";
+ 设置单元格数据  
_curSheet.SetValue(row, col, val);

## 格式

+ 页面网格  
  _curSheet.View.ShowGridLines = false;
+ 单元格边框  
  _curSheet.Cells[row1, col1, row2, col2].Style.Border.BorderAround(OfficeOpenXml.Style.ExcelBorderStyle.Thin, System.Drawing.Color.Black);
+ 合并单元格  
  _curSheet.Cells[row1, col1, row2, col2].Merge = true;
+ 对齐方式  
  _curSheet.Cells[row, col].Style.HorizontalAlignment = ExcelHorizontalAlignment.Left
+ 字体颜色  
_curSheet.Cells[row, col].Style.Font.Color.SetColor(System.Drawing.Color.FromArgb(r, g, b));
+ 字体大小  
curSheet.Cells[row, col, row, col].Style.Font.Size = 12;
+ 单元格背景色  
 _curSheet.Cells[row, col].Style.Fill.PatternType = ExcelFillStyle.Solid;
 _curSheet.Cells[row, col].Style.Fill.BackgroundColor.SetColor(Color.FromArgb(r, g, b));
+ 文本自动换行  
  _curSheet.Cells[row1, col1, row2, col2].Style.WrapText = true;
+ 字体加粗  
  _curSheet.Cells[row1, col1, row2, col2].Style.Font.Bold = true;
+ 超链接  

```
 //路径前不能带//
 ExcelHyperLink hl = new ExcelHyperLink("dirname/dir", UriKind.Relative);
 hl.Display = "链接名";
 curSheet.Cells[row, col].Hyperlink = hl;
 curSheet.Cells[row, col].Style.Font.Color.SetColor(System.Drawing.ColorFromArgb(0, 0, 255));
 curSheet.Cells[row, col].Style.Font.UnderLine = true;

sheet页链接：
curSheet.Cells[row, col].Hyperlink = new ExcelHyperLink("sheet名!A1", "显示文字");
```

+ 单元格大小  
_curSheet.Column(col).Width = 10;
