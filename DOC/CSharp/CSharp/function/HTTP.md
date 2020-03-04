# http

## GET请求

```
HttpWebRequest req = (HttpWebRequest)HttpWebRequest.Create(reqstStr);
req.Method = "GET";
req.Headers["diyKey"] = "val";//自定义报头中的key-val
using (WebResponse wr = req.GetResponse())
{
      Stream responseStream = wr.GetResponseStream();
      StreamReader streamReader = new StreamReader(responseStream, Encoding.UTF8);
      string retString = streamReader.ReadToEnd();
}
```

## POST请求

```
string param = string.Format("userName={0}&passWord={1}", user, password);
byte[] bs = Encoding.ASCII.GetBytes(param);

HttpWebRequest req = (HttpWebRequest)HttpWebRequest.Create(reqstStr);
req.Method = "POST";
req.KeepAlive = false;
req.ContentType = "application/x-www-form-urlencoded";
req.ContentLength = bs.Length;
using (Stream reqStream = req.GetRequestStream())
{
     reqStream.Write(bs, 0, bs.Length);
}


using (WebResponse wr = req.GetResponse())
{
      Stream responseStream = wr.GetResponseStream();
      XmlDocument doc = new XmlDocument();
      doc.Load(responseStream);
}
```

## HTTP服务

```
class HttpSer
{
    HttpListener httpPostRequest = new HttpListener();
    public void Start()
    {
        httpPostRequest.Prefixes.Add("http://127.0.0.1:8080/abc/");
        httpPostRequest.Start();

        Thread ThrednHttpPostRequest = new Thread(httpPostRequestHandle);
        ThrednHttpPostRequest.IsBackground = true;
        ThrednHttpPostRequest.Start();    
    }
    void httpPostRequestHandle()
    {
        while (true)
        {
            HttpListenerContext requestContext = httpPostRequest.GetContext();

            Thread threadsub = new Thread(() =>
            {
                HttpListenerContext request = (HttpListenerContext)requestContext;
                string requestMethod = request.Request.HttpMethod;

                if (requestMethod == "GET")
                {
                    string val = request.Request.QueryString["user"];
string requestKeyVal = request.Request.Headers["diyKey"];//获取自定义报头中的key-val
                }
                else
                {
                    string requestDatType = request.Request.ContentType;
                    Stream SourceStream = request.Request.InputStream;
                    if (requestDatType == "text/plain")
                    {
                        StreamReader streamReader = new StreamReader(SourceStream, Encoding.UTF8);
                        string retString = streamReader.ReadToEnd();
                    }
                    else if(requestDatType == "image/jpeg")
                    {
                        int requestDatLen = (int)request.Request.ContentLength64;

                        BinaryReader br = new BinaryReader(SourceStream);
                        byte[] img = new byte[requestDatLen];
                        int revLen = 0;
                        while (revLen < requestDatLen)
                        {
                            int read = br.Read(img, revLen, requestDatLen - revLen);
                            revLen += read;
                        }
                    }
                }


                request.Response.StatusCode = 200;
                request.Response.Headers.Add("Access-Control-Allow-Origin", "*");
                request.Response.ContentType = "application/json";
                request.Response.ContentEncoding = Encoding.UTF8;
                byte[] buffer = System.Text.Encoding.UTF8.GetBytes("success");
                request.Response.ContentLength64 = buffer.Length;
                var output = request.Response.OutputStream;
                output.Write(buffer, 0, buffer.Length);
                output.Close();
            });
            threadsub.IsBackground = true;
            threadsub.Start();
        }
    }
}  
```
