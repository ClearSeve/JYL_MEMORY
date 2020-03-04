# pdfsharp

## 合并文件

```
void Combin(List<string> files,string outpathName)
{
    PdfDocument outputDocument = new PdfDocument() { PageLayout  = PdfPageLayout.TwoColumnLeft };
    foreach (var file in files)
    {
        PdfDocument inputDocument = PdfReader.Open(file, PdfDocumentOpenMode.Import);
        foreach (var page in inputDocument.Pages) outputDocument.AddPage(page);
    }
    outputDocument.Save(outpathName);
}
```

## 创建文件

```
void GenPdf()
{
    PdfDocument document = new PdfDocument();
    PdfPage page = document.AddPage();
    XGraphics gfx = XGraphics.FromPdfPage(page);
    XFont font = new XFont("Verdana", 20, XFontStyle.Bold);
    gfx.DrawString("Hello, World!", font, XBrushes.Black,
      new XRect(0, 0, page.Width, page.Height),
      XStringFormat.Center);
    string filename = "HelloWorld.pdf";
    document.Save(filename);
    Process.Start(filename);
}
```
