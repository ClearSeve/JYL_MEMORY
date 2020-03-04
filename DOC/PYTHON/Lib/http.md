# http

```
from urllib.parse import urlsplit, parse_qs  
from http.server import HTTPServer, BaseHTTPRequestHandler  
  
class ProHeader(BaseHTTPRequestHandler):  
    def do_GET(self):  
        url = urlsplit(self.path)  
        qs = parse_qs(url.query)
        rsp = "get"  
        rsp = rsp.encode("gb2312")
        self.send_response(200)
        self.send_header("Content-type", "text/html; charset=gb2312")
        self.send_header("Content-Length", str(len(rsp)))
        self.end_headers()
        self.wfile.write(rsp)  
    def do_POST(self):  
        datas = self.rfile.read(int(self.headers['content-length']))

if __name__ == '__main__':  
    httpd = HTTPServer(('', 1234), ProHeader)  
    httpd.serve_forever()
```
