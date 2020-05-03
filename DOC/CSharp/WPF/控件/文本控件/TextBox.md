# TextBox


## 限制输入小数
```
void XX_PreviewTextInput(object sender, TextCompositionEventArgs e)
{
    TextBox tb = sender as TextBox;
    if (null == tb) return;

    Regex reg = new Regex(@"[^0-9.-]+");
    e.Handled = reg.IsMatch(e.Text);
    string inputBehind = tb.Text + e.Text;
    if (!e.Handled)
        e.Handled = inputBehind.IndexOf(".") == inputBehind.LastIndexOf(".") ? false : true;
}
```

## double绑定

```
int m_pointAfterDataCount = 2;//保留小数位数
string val;
public string VAL
{
    get { return val; }
    set
    {
        try
        {
            double dat = Convert.ToDouble(value);
            int pointPos = value.LastIndexOf('.');
            if (m_pointAfterDataCount > 0 && pointPos > 0)
            {
                string A = value.Substring(0, pointPos);
                string B = value.Substring(pointPos + 1);
                if (B.Length > m_pointAfterDataCount) B = B.Substring(0, m_pointAfterDataCount);
                val = A + "." + B;
            }
            else
            {
                val = value;
            }
        }
        catch (Exception ex)
        {
            val = "";
        }
        OnPropertyChanged("VAL");
    }
}
```
