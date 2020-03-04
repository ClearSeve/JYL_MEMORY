# request

```
<dependency>
    <groupId>org.apache.httpcomponents</groupId>
    <artifactId>httpclient</artifactId>
    <version>4.5.6</version>
</dependency>


import java.util.Map;
import java.util.List;
import java.util.ArrayList;
import java.net.URI;
import org.apache.http.HttpEntity;
import org.apache.http.NameValuePair;
import org.apache.http.client.config.RequestConfig;
import org.apache.http.client.entity.UrlEncodedFormEntity;
import org.apache.http.client.methods.CloseableHttpResponse;
import org.apache.http.client.methods.HttpGet;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.client.methods.HttpUriRequest;
import org.apache.http.impl.client.CloseableHttpClient;
import org.apache.http.impl.client.HttpClientBuilder;
import org.apache.http.message.BasicNameValuePair;
import org.apache.http.util.EntityUtils;
import org.apache.http.util.EntityUtils;



byte[] sendGetMsg(String url)throws Exception
{
    HttpGet httpGet = new HttpGet();
    httpGet.setURI(new URI(url));
    httpGet.setHeader("Content-Type", "text/html;charset=UTF-8");
    return sendHTTP(httpGet);
}
byte[] sendPostMsg(String url, Map<String, Object> params)throws Exception
{
    HttpPost httpPost = new HttpPost(url);

    List<NameValuePair> pairs = null;
    if (params != null && !params.isEmpty()) {
        pairs = new ArrayList<NameValuePair>(params.size());
        for (String key : params.keySet()) {
            pairs.add(new BasicNameValuePair(key, params.get(key).toString()));
        }
    }
    if (pairs != null && pairs.size() > 0) {
        httpPost.setEntity(new UrlEncodedFormEntity(pairs));
    }
    httpPost.setHeader("Content-Type", "application/x-www-form-urlencoded");
    return sendHTTP(httpPost);
}
byte[] sendPostMsg(String url, String json)throws Exception
{
    HttpPost httpPost = new HttpPost(url);
     StringEntity entity = new StringEntity(json,"UTF-8");
    entity.setContentEncoding("UTF-8");
    entity.setContentType("application/json");
    httpPost.setEntity(entity);
    return sendHTTP(httpPost);
}
byte[] sendHTTP(HttpUriRequest httpObj) throws Exception {
    RequestConfig config = RequestConfig.custom().setConnectTimeout(5000).setSocketTimeout(3000).build();
    CloseableHttpClient client = HttpClientBuilder.create().setDefaultRequestConfig(config).build();
    CloseableHttpResponse response = client.execute(httpObj);
    int statusCode = response.getStatusLine().getStatusCode();
    if (statusCode != 200) {
        httpObj.abort();
        throw new RuntimeException("HttpClient,error status code :" + statusCode);
    }
    HttpEntity entity = response.getEntity();
    if (entity != null) {
        return EntityUtils.toByteArray(entity);
    } else {
        return null;
    }
}



String url = "http://127.0.0.1:8080/";
//byte[] body = sendGetMsg(url);

Map<String, Object> params = new HashMap<String, Object>();
params.put("IntVal", 12);
params.put("StrVal", "ss");
byte[] body = sendPostMsg(url,params);

String retstr = new String(body, "UTF-8");
```
