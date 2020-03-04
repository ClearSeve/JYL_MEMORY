# webBrowser

## 打开网页
wb.Navigate(new Uri(address));
wb.NavigateToString("<html><body></body></html > ");

## 跳转
if (wb.CanGoBack){wb.GoBack();}
if (wb.CanGoForward){wb.GoForward();}


## 导航到新的页面时：
wb.Navigating

## 导航之后，在下载web页面之前
wb.Navigated

## web页面下载完成时
wb.LoadCompleted
