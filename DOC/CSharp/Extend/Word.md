# Word

## 安装

https://msdn.microsoft.com/en-us/library/office/cc850833.aspx  
OpenXMLSDKv2.msi  
OpenXMLSDKTool.msi  

可以不安装，添加引用即可:
C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETFramework\v4.0\WindowsBase.dll
C:\Program Files (x86)\Open XML SDK\V2.0\lib\DocumentFormat.OpenXml.dll

## 创建

```
会和System.Windows.Documents重名

using DocumentFormat.OpenXml;
using DocumentFormat.OpenXml.Wordprocessing;
using DocumentFormat.OpenXml.Packaging;

using OpenXmlParagraph = DocumentFormat.OpenXml.Wordprocessing.Paragraph;
using OpenXmlWordRun = DocumentFormat.OpenXml.Wordprocessing.Run;




WordprocessingDocument doc = WordprocessingDocument.Create(pathName, WordprocessingDocumentType.Document);
MainDocumentPart mainPart = doc.AddMainDocumentPart();
mainPart.Document = new Document();
Body body = mainPart.Document.AppendChild(new Body());

//....写入段落
OpenXmlWordRun run =  new OpenXmlWordRun(new Text(str));

OpenXmlParagraph paragraph = body.AppendChild(new OpenXmlParagraph());
paragraph.AppendChild(run);
//....写入

doc.Close();
```

## 打开

WordprocessingDocument.Open(filePath, true)

## 文字格式

```
 RunFonts fonts = new RunFonts() { EastAsia = "黑体" }; 
 FontSize size = new FontSize() { Val = "70" }; 
DocumentFormat.OpenXml.Wordprocessing.Color color = new DocumentFormat.OpenXml.Wordprocessing.Color() { ThemeColor = ThemeColorValues.Accent2};

//或使用 runProperties.Append(color);  runProperties.Append(size);
RunProperties runProperties = new RunProperties(color,size,fonts);

Text txt = new Text("word文字");
OpenXmlWordRun run = new OpenXmlWordRun(runProperties, txt);

OpenXmlParagraph paragraph = body.AppendChild(new OpenXmlParagraph());
paragraph.AppendChild(run);
```

## 插入图像

```
using A = DocumentFormat.OpenXml.Drawing;
using DW = DocumentFormat.OpenXml.Drawing.Wordprocessing;
using PIC = DocumentFormat.OpenXml.Drawing.Pictures;

public void AddPictureIntoWord(string docfilePath, string picturePath)
{
    using (WordprocessingDocument doc = WordprocessingDocument.Open(docfilePath, true))
    {
        ImagePartType imagePartType = ImagePartType.Jpeg;

        //string picType = picturePath.Split('.').Last();
        //if (!Enum.TryParse<ImagePartType>(picType, true, out imagePartType)) return;  // 通过后缀名判断图片类型, true 表示忽视大小写

        ImagePart  imagePart = doc.MainDocumentPart.AddImagePart(imagePartType);
        imagePart.FeedData(File.Open(picturePath, FileMode.Open)); // 读取图片二进制流
        AddImageToBody(doc, doc.MainDocumentPart.GetIdOfPart(imagePart));
    }
}




private void AddImageToBody(WordprocessingDocument wordDoc, string relationshipId)
{
    // Define the reference of the image.
    var element =
         new Drawing(
             new DW.Inline(
                 new DW.Extent() { Cx = 990000L, Cy = 792000L }, // 调节图片大小
                 new DW.EffectExtent()
                 {
                     LeftEdge = 0L,
                     TopEdge = 0L,
                     RightEdge = 0L,
                     BottomEdge = 0L
                 },
                 new DW.DocProperties()
                 {
                     Id = (UInt32Value)1U,
                     Name = "Picture 1"
                 },
                 new DW.NonVisualGraphicFrameDrawingProperties(
                     new A.GraphicFrameLocks() { NoChangeAspect = true }),
                 new A.Graphic(
                     new A.GraphicData(
                         new PIC.Picture(
                             new PIC.NonVisualPictureProperties(
                                 new PIC.NonVisualDrawingProperties()
                                 {
                                     Id = (UInt32Value)0U,
                                     Name = "New Bitmap Image.jpg"
                                 },
                                 new PIC.NonVisualPictureDrawingProperties()),
                             new PIC.BlipFill(
                                 new A.Blip(
                                     new A.BlipExtensionList(
                                         new A.BlipExtension()
                                         {
                                             Uri =
                                               "{28A0092B-C50C-407E-A947-70E740481C1C}"
                                         })
                                 )
                                 {
                                     Embed = relationshipId,
                                     CompressionState =
                                     A.BlipCompressionValues.Print
                                 },
                                 new A.Stretch(
                                     new A.FillRectangle())),
                             new PIC.ShapeProperties(
                                 new A.Transform2D(
                                     new A.Offset() { X = 0L, Y = 0L },
                                     new A.Extents() { Cx = 990000L, Cy = 792000L }), //与上面的对准
                                 new A.PresetGeometry(
                                     new A.AdjustValueList()
                                 ) { Preset = A.ShapeTypeValues.Rectangle }))
                     ) { Uri = "http://schemas.openxmlformats.org/drawingml/2006/picture" })
             )
             {
                 DistanceFromTop = (UInt32Value)0U,
                 DistanceFromBottom = (UInt32Value)0U,
                 DistanceFromLeft = (UInt32Value)0U,
                 DistanceFromRight = (UInt32Value)0U,
                 EditId = "50D07946"
             });
    // Append the reference to body, the element should be in a Run.
    wordDoc.MainDocumentPart.Document.Body.AppendChild(new Paragraph(new Run(element)));
}
```
