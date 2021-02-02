# Hello this world.
获取网页内容的固定格式
 <dependencies>
      <dependency>
        <groupId>com.squareup.okhttp3</groupId>    
        <artifactId>okhttp</artifactId>    
        <version>4.1.0</version>    

import java.io.IOException;
import okhttp3.Call;
import okhttp3.OkHttpClient;
import okhttp3.Request;

public class GetPage {

  /**
   * 根据输入的url，读取页面内容并返回
   */
  public String getContent(String url) {
   OkHttpClient okHttpClient=new OkHttpClient();
   Request request=new Request.Builder().url(url).build();
   Call call=okHttpClient.newCall(request);
    // 返回结果字符串
    String result = null;
    try{
      result=call.execute().body().string();
    }
catch(IOException e){
  System.out.println("request"+url+"error,");
  e.printStackTrace();
}
    return result;
  }

  public static void main(String[] args) {
    GetPage getPage=new GetPage();
    String url ="https://ipservice.3g.163.com/ip" ;//可变内容
    String content =getPage.getContent(url) ;
  
    System.out.println("API调用结果");
    System.out.println("Call"+url+"content="+content);
  }
}
POST表单数据
package com.youkeda.test.http;

import java.io.IOException;
import java.util.Map;
import java.util.HashMap;
import okhttp3.Call;
import okhttp3.OkHttpClient;
import okhttp3.Request;
import okhttp3.FormBody;
import okhttp3.FormBody.Builder;

public class FormPoster {

  /**
   * 向指定的 url 提交数据
   */
  public String postContent(String url, Map<String, String> formData) {
    // okHttpClient 实例
    OkHttpClient okHttpClient = new OkHttpClient();

    //post方式提交的数据
    Builder builder = new FormBody.Builder();
    // 放入表单数据
    for (String key : formData.keySet()) {
      builder.add(key, formData.get(key));
    }
    // 构建 FormBody 对象
    FormBody formBody = builder.build();
    // 指定 post 方式提交FormBody
    Request request = new Request.Builder().url(url).post(formBody).build();

    // 使用client去请求
    Call call = okHttpClient.newCall(request);
    // 返回结果字符串
    String result = null;
    try {
      // 获得返回结果
      result = call.execute().body().string();
    } catch (IOException e) {
      // 抓取异常
      System.out.println("request " + url + " error . ");
      e.printStackTrace();
    }
    return result;
  }

  public static void main(String[] args) {
    String url = "https://gitee.com/login";
    Map<String, String> formData = new HashMap();
    formData.put("user[login]", "17177466748");

    FormPoster poster = new FormPoster();
    String content = poster.postContent(url, formData);
  
    System.out.println("API调用结果");
    System.out.println(content);
  }
}



























POST JSON数据
<dependencies>

      <dependency>
        <groupId>com.squareup.okhttp3</groupId>
        <artifactId>okhttp</artifactId>
        <version>4.1.0</version>
      </dependency>

      <!-- JSON 操作库 -->
      <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>fastjson</artifactId>
        <version>1.2.62</version>
      </dependency>

    </dependencies>

</project>


package com.youkeda.test.http;

import com.alibaba.fastjson.JSON;
import java.io.IOException;
import java.util.HashMap;
import java.util.Map;
import okhttp3.Call;
import okhttp3.MediaType;
import okhttp3.OkHttpClient;
import okhttp3.Request;
import okhttp3.RequestBody;

public class JsonPoster {

  // 定义提交数据的类型
  public static final MediaType JSON_TYPE = MediaType.parse("application/json; charset=utf-8");

  /**
   * 向指定的 url 提交数据，以 json 的方式
   */
  public String postContent(String url, Map<String, String> datas) {
    // okHttpClient 实例
    OkHttpClient okHttpClient = new OkHttpClient();

    // 数据对象转换成 json 格式字符串
    String param = JSON.toJSONString(datas);
    //post方式提交的数据
    RequestBody requestBody = RequestBody.create(JSON_TYPE, param);
    Request request = new Request.Builder().url(url).post(requestBody).build();

    // 使用client去请求
    Call call = okHttpClient.newCall(request);
    // 返回结果字符串
    String result = null;
    try {
      // 获得返回结果
      result = call.execute().body().string();
    } catch (IOException e) {
      // 抓取异常
      System.out.println("request " + url + " error . ");
      e.printStackTrace();
    }
    return result;
  }

  public static void main(String[] args) {
    String url = "https://www.fastmock.site/mock/3d95acf3f26358ef032d8a23bfdead99/api/posts";
    Map<String, String> datas = new HashMap();
    datas.put("name", "张飞");
    datas.put("secondName", "翼德");

    JsonPoster poster = new JsonPoster();
    String content = poster.postContent(url, datas);

    System.out.println("API调用结果");
    System.out.println(content);
  }
}






RESPONSE网页

<dependencies>

      <dependency>
        <groupId>com.squareup.okhttp3</groupId>
        <artifactId>okhttp</artifactId>
        <version>4.1.0</version>
      </dependency>

    </dependencies>

</project>




package com.youkeda.test.http;

import java.io.IOException;
import okhttp3.Call;
import okhttp3.OkHttpClient;
import okhttp3.Request;
import okhttp3.Response;

public class GetPage {

  /**
   * 根据输入的url，读取页面内容并返回
   */
  public String getContent(String url) {
    // okHttpClient 实例
    OkHttpClient okHttpClient = new OkHttpClient();
    // 定义一个request
    Request request = new Request.Builder().url(url).build();
    // 使用client去请求
    Call call = okHttpClient.newCall(request);
    // 返回结果字符串
    String result = null;
    try {
      // 执行请求
    Response rep = call.execute();  
      // 获取响应状态码
    int code = rep.code();  
      System.out.println("状态码：" + code);
      // 获取响应内容
    result = rep.body().string();  
    } catch (IOException e) {
      // 抓取异常
      System.out.println("request " + url + " error . ");
      e.printStackTrace();
    }
    return result;
  }

  public static void main(String[] args) {
    String url = "https://www.ustc.edu.cn/";
    GetPage getPage = new GetPage();
    String content = getPage.getContent(url);
  
    System.out.println("请求结果字符串长度：" + content.length());
  }
}























RESPONSE非文本文件

<dependencies>

      <dependency>
        <groupId>com.squareup.okhttp3</groupId>
        <artifactId>okhttp</artifactId>
        <version>4.1.0</version>
      </dependency>

    </dependencies>

</project>


import java.io.IOException;
import okhttp3.Call;
import okhttp3.OkHttpClient;
import okhttp3.Request;
import okhttp3.Response;

public class ImageAsker {

  /**
   * 根据输入的url，读取页面内容并返回
   */
  public void getImage(String url) {
    // okHttpClient 实例
    OkHttpClient okHttpClient = new OkHttpClient();
    // 定义一个request
    Request request = new Request.Builder().url(url).build();
    try {
      // 执行请求
      Response response = okHttpClient.newCall(request).execute();
      byte[] bytes = response.body().bytes();  
      System.out.println("图片大小为：" + bytes.length + " 字节");
    } catch (IOException e) {
      // 抓取异常
      System.out.println("request " + url + " error . ");
      e.printStackTrace();
    }
  }

  public static void main(String[] args) {
    String url = "https://style.youkeda.com/img/ham/course/py2/douban.png";
    ImageAsker asker = new ImageAsker();
    asker.getImage(url);
  }
}





































RESPONSE-JSON

  <dependencies>

      <dependency>
        <groupId>com.squareup.okhttp3</groupId>
        <artifactId>okhttp</artifactId>
        <version>4.1.0</version>
      </dependency>

      <!-- JSON 操作库 -->
      <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>fastjson</artifactId>
        <version>1.2.62</version>
      </dependency>

    </dependencies>

</project>

import com.alibaba.fastjson.JSON;
import java.io.IOException;
import java.util.HashMap;
import java.util.Map;
import okhttp3.Call;
import okhttp3.OkHttpClient;
import okhttp3.Request;
import okhttp3.Response;

public class ApiAsker {

  /**
   * 根据输入的url，读取页面内容并返回
   */
  public String getContent(String url) {
    // okHttpClient 实例
    OkHttpClient okHttpClient = new OkHttpClient();
    // 定义一个request
    Request request = new Request.Builder().url(url).build();
    // 使用client去请求
    Call call = okHttpClient.newCall(request);
    // 返回结果字符串
    String result = null;
    try {
      // 执行请求
      Response response = call.execute();
      // 获取响应内容
      result = response.body().string();
    } catch (IOException e) {
      // 抓取异常
      System.out.println("request " + url + " error . ");
      e.printStackTrace();
    }
    return result;
  }

  public static void main(String[] args) {
    String url = "http://ip-api.com/json/";
    ApiAsker asker = new ApiAsker();
    String content = asker.getContent(url);

    System.out.println("查询结果文本：" + content);
    Map contentObj = JSON.parseObject(content, Map.class);
    System.out.println("JSON 格式字符串转换为 Map 对象");
  }
}



















解析JSON对象

<dependencies>

      <dependency>
        <groupId>com.squareup.okhttp3</groupId>
        <artifactId>okhttp</artifactId>
        <version>4.1.0</version>
      </dependency>

      <!-- JSON 操作库 -->
      <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>fastjson</artifactId>
        <version>1.2.62</version>
      </dependency>

    </dependencies>

</project>

import com.alibaba.fastjson.JSON;
import java.io.IOException;
import java.util.HashMap;
import java.util.Map;
import okhttp3.Call;
import okhttp3.OkHttpClient;
import okhttp3.Request;
import okhttp3.Response;

public class ApiAsker {

  /**
   * 根据输入的url，读取页面内容并返回
   */
  public String getContent(String url) {
    // okHttpClient 实例
    OkHttpClient okHttpClient = new OkHttpClient();
    // 定义一个request
    Request request = new Request.Builder().url(url).build();
    // 使用client去请求
    Call call = okHttpClient.newCall(request);
    // 返回结果字符串
    String result = null;
    try {
      // 执行请求
      Response response = call.execute();
      // 获取响应内容
      result = response.body().string();
    } catch (IOException e) {
      // 抓取异常
      System.out.println("request " + url + " error . ");
      e.printStackTrace();
    }
    return result;
  }

  public static void main(String[] args) {
    String url = "http://ip-api.com/json/";
    ApiAsker asker = new ApiAsker();
    String content = asker.getContent(url);

    System.out.println("查询结果文本：" + content);
    Map contentObj = JSON.parseObject(content, Map.class);
    System.out.println("当前 IP 为：" + contentObj.get("query"));
  }
}



















4.1User-Agent

import java.io.IOException;
import java.util.HashMap;
import java.util.Map;
import okhttp3.Call;
import okhttp3.OkHttpClient;
import okhttp3.Request;
import okhttp3.Response;

public class ApiAsker {

  /**
   * 根据输入的url，读取页面内容并返回
   */
  public String getContent(String url) {
    // okHttpClient 实例
    OkHttpClient okHttpClient = new OkHttpClient();
    // 定义一个request
    Request request = new Request.Builder()
        .url(url)
        // 添加一个 header 信息
        .addHeader("User-Agent", "Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/535.1 (KHTML, like Gecko) Chrome/14.0.835.163 Safari/535.1")    
        .build();
    // 返回结果字符串
    String result = null;
    try {
      // 执行请求
      Response response = okHttpClient.newCall(request).execute();
      // 获取响应内容
      result = response.body().string();
    } catch (IOException e) {
      // 抓取异常
      System.out.println("request " + url + " error . ");
      e.printStackTrace();
    }
    return result;
  }

  public static void main(String[] args) {
    String url = "https://www.fastmock.site/mock/3d95acf3f26358ef032d8a23bfdead99/api/service/getIpInfo.php?ip=117.89.35.58&format=json";
    ApiAsker asker = new ApiAsker();
    String content = asker.getContent(url);

    System.out.println("查询结果文本：" + content);
  }
}
4.2Reference

import java.io.IOException;
import java.util.HashMap;
import java.util.Map;
import okhttp3.Call;
import okhttp3.OkHttpClient;
import okhttp3.Request;
import okhttp3.Response;

public class ImageAsker {

  /**
   * 根据输入的url，读取页面内容并返回
   */
  public void getContent(String url) {
    // okHttpClient 实例
    OkHttpClient okHttpClient = new OkHttpClient();
    // 定义一个request
    Request request = new Request.Builder()
        .url(url)
        .addHeader("Referer", "https://unreach.yuque.com")
        .build();
    try {
      // 执行请求
      Response response = okHttpClient.newCall(request).execute();
      int code = response.code();  
      System.out.println("状态码：" + code);
    } catch (IOException e) {
      // 抓取异常
      System.out.println("request " + url + " error . ");
      e.printStackTrace();
    }
  }

  public static void main(String[] args) {
    String url = "https://cdn.nlark.com/yuque/0/2019/png/93870/1571386626984-2462f7f9-d397-4e50-91e4-e4b688dd3410.png";
    ImageAsker asker = new ImageAsker();
    asker.getContent(url);
  }
}

4.3HOST
import java.io.IOException;
import java.util.HashMap;
import java.util.Map;
import java.util.List;
import okhttp3.Call;
import okhttp3.OkHttpClient;
import okhttp3.Request;
import okhttp3.Response;

public class GetPage {

  /**
   * 根据输入的url，读取页面内容并返回
   */
  public String getContent(String url) {
    // okHttpClient 实例
    OkHttpClient okHttpClient = new OkHttpClient();
    // 定义一个request
    Request request = new Request.Builder()
        .url(url)
        .addHeader("User-Agent", "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_1) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/78.0.3904.108 Safari/537.36")
        .addHeader("Referer", "https://www.douban.com/")
        .addHeader("Host", "www.douban.com")
        .build();
    // 返回结果字符串
    String result = null;
    try {
      // 执行请求
      Response response = okHttpClient.newCall(request).execute();
      // 获取响应内容
      result = response.body().string();
    } catch (IOException e) {
      // 抓取异常
      System.out.println("request " + url + " error . ");
      e.printStackTrace();
    }
    return result;
  }

  public static void main(String[] args) {
    String url = "https://www.douban.com/search?source=suggest&q=小丑";
    GetPage asker = new GetPage();
    String content = asker.getContent(url);

    System.out.println("页面size = " + content.length());
  }
}

5.1．1写入文本文件
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.youkeda.course</groupId>
    <artifactId>app</artifactId>
    <version>1.0-SNAPSHOT</version>

    <properties>
        <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
        <java.version>1.8</java.version>
        <resource.delimiter>@</resource.delimiter>
        <maven.compiler.source>${java.version}</maven.compiler.source>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <maven.compiler.target>${java.version}</maven.compiler.target>
    </properties>


   


</project>



import java.io.File;
import java.io.FileWriter;
import java.io.IOException;

public class FileTest {

  public static void main(String[] args) {
    try {
      String content = "Hello World";

      File file = new File("foo.txt");  

      FileWriter fileWritter = new FileWriter(file.getName());  
      fileWritter.write(content);  

      fileWritter.close();  

      System.out.println("finish");
    } catch (IOException e) {
      e.printStackTrace();
    }
  }
}
5.1.2写入二进制文件
<dependencies>

      <dependency>
        <groupId>com.squareup.okhttp3</groupId>
        <artifactId>okhttp</artifactId>
        <version>4.1.0</version>
      </dependency>

    </dependencies>

</project>
import java.io.IOException;
import okhttp3.Call;
import okhttp3.OkHttpClient;
import okhttp3.Request;
import okhttp3.Response;
import java.io.File;
import java.io.FileOutputStream;


public class ExcelAsker {

  /**
   * 根据输入的url，读取页面内容并返回
   */
  public byte[] getFile(String url) {
    // okHttpClient 实例
    OkHttpClient okHttpClient = new OkHttpClient();
    // 定义一个request
    Request request = new Request.Builder().url(url).build();
    byte[] bytes = null;
    try {
      // 执行请求
      Response response = okHttpClient.newCall(request).execute();
      bytes = response.body().bytes();
    } catch (IOException e) {
      // 抓取异常
      System.out.println("request " + url + " error . ");
      e.printStackTrace();
    }

    return bytes;
  }

  public static void main(String[] args) {
    String url = "https://style.youkeda.com/img/ham/course/py2/china-city-list.xlsx";
    ExcelAsker asker = new ExcelAsker();
    byte[] data = asker.getFile(url);

    try {
      // 文件对象
      File file = new File("china-city-list.xlsx");  

      // 写文件
      FileOutputStream fos = new FileOutputStream(file);  
      fos.write(data);  

      // 必须刷新并关闭
      fos.flush();  
      fos.close();  
    } catch (IOException e) {
      e.printStackTrace();
    }
  }
}
5.2下载图片
import java.io.IOException;
import java.io.File;
import java.io.FileOutputStream;
import okhttp3.Call;
import okhttp3.OkHttpClient;
import okhttp3.Request;
import okhttp3.Response;

public class ImageAsker {

  /**
   * 根据输入的url，读取页面内容并返回
   */
  public byte[] getImage(String url) {
    // okHttpClient 实例
    OkHttpClient okHttpClient = new OkHttpClient();
    // 定义一个request
    Request request = new Request.Builder().url(url).build();
    byte[] bytes = null;
    try {
      // 执行请求
      Response response = okHttpClient.newCall(request).execute();
      bytes = response.body().bytes();
      System.out.println("图片大小为： " + bytes.length + " 字节");
    } catch (IOException e) {
      // 抓取异常
      System.out.println("request " + url + " error . ");
      e.printStackTrace();
    }

    return bytes;
  }

  public static void main(String[] args) {
    String url = "https://n.sinaimg.cn/news/1_img/dfic/72f96829/125/w1024h701/20191209/e270-iknhexh8551480.jpg";
    ImageAsker asker = new ImageAsker();
    byte[] data = asker.getImage(url);

    try {
      File file = new File("e270-iknhexh8551480.jpg");

      FileOutputStream fos = new FileOutputStream(file);
      fos.write(data);

      fos.flush();
      fos.close();

      System.out.println("下载成功");
    } catch (IOException e) {
      e.printStackTrace();
    }
  }
}
5.3.1解析EXCEL

<dependencies>

      <dependency>
        <groupId>com.squareup.okhttp3</groupId>
        <artifactId>okhttp</artifactId>
        <version>4.1.0</version>
      </dependency>

      <!-- JSON 操作库 -->
      <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>fastjson</artifactId>
        <version>1.2.62</version>
      </dependency>

      <!-- excel 操作库 -->
      <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>easyexcel</artifactId>
        <version>2.1.6</version>
      </dependency>

    </dependencies>

</project>

import com.alibaba.excel.EasyExcel;
import com.alibaba.fastjson.JSON;
import java.io.File;
import java.io.FileOutputStream;
import java.io.IOException;
import java.util.List;
import java.util.Map;
import okhttp3.OkHttpClient;
import okhttp3.Request;
import okhttp3.Response;

public class ExcelAsker {

  /**
   * 根据输入的url，读取页面内容并返回
   */
  public byte[] getFile(String url) {
    // okHttpClient 实例
    OkHttpClient okHttpClient = new OkHttpClient();
    // 定义一个request
    Request request = new Request.Builder().url(url).build();
    byte[] bytes = null;
    try {
      // 执行请求
      Response response = okHttpClient.newCall(request).execute();
      bytes = response.body().bytes();
    } catch (IOException e) {
      // 抓取异常
      System.out.println("request " + url + " error . ");
      e.printStackTrace();
    }

    return bytes;
  }

  public static void main(String[] args) {
    String url = "https://style.youkeda.com/img/ham/course/py2/xzq_201907.xlsx";
    ExcelAsker asker = new ExcelAsker();
    byte[] data = asker.getFile(url);

    try {
      File file = new File("xzq_201907.xlsx");

      FileOutputStream fos = new FileOutputStream(file);
      fos.write(data);

      fos.flush();
      fos.close();

      System.out.println("下载成功");
    } catch (IOException e) {
      e.printStackTrace();
    }

    List<Map<Integer, String>> sheetDatas = EasyExcel.read("xzq_201907.xlsx").sheet(0).doReadSync();
    for (Map<Integer, String> rowData : sheetDatas) {
      for (Integer index : rowData.keySet()) {
        System.out.print("列号：" + index + "，列值：" + rowData.get(index) + "。");    
      }  
      System.out.println("");  
    }
  }
}
5.3.2自定义
public class DemoData {

  private String code1;
  private String city1;
  private String code2;
  private String city2;
  private String code3;
  private String city3;

  public String getCode1() {
    return code1;
  }

  public void setCode1(String code1) {
    this.code1 = code1;
  }

  public String getCity1() {
    return city1;
  }

  public void setCity1(String city1) {
    this.city1 = city1;
  }

  public String getCode2() {
    return code2;
  }

  public void setCode2(String code2) {
    this.code2 = code2;
  }

  public String getCity2() {
    return city2;
  }

  public void setCity2(String city2) {
    this.city2 = city2;
  }

  public String getCode3() {
    return code3;
  }

  public void setCode3(String code3) {
    this.code3 = code3;
  }

  public String getCity3() {
    return city3;
  }

  public void setCity3(String city3) {
    this.city3 = city3;
  }
}

import com.alibaba.excel.EasyExcel;
import com.alibaba.fastjson.JSON;
import java.io.File;
import java.io.FileOutputStream;
import java.io.IOException;
import java.util.List;
import java.util.Map;
import okhttp3.OkHttpClient;
import okhttp3.Request;
import okhttp3.Response;

public class ExcelAsker {

  /**
   * 根据输入的url，读取页面内容并返回
   */
  public byte[] getFile(String url) {
    // okHttpClient 实例
    OkHttpClient okHttpClient = new OkHttpClient();
    // 定义一个request
    Request request = new Request.Builder().url(url).build();
    byte[] bytes = null;
    try {
      // 执行请求
      Response response = okHttpClient.newCall(request).execute();
      bytes = response.body().bytes();
    } catch (IOException e) {
      // 抓取异常
      System.out.println("request " + url + " error . ");
      e.printStackTrace();
    }

    return bytes;
  }

  public static void main(String[] args) {
    String url = "https://style.youkeda.com/img/ham/course/py2/xzq_201907.xlsx";
    ExcelAsker asker = new ExcelAsker();
    byte[] data = asker.getFile(url);

    try {
      File file = new File("xzq_201907.xlsx");

      FileOutputStream fos = new FileOutputStream(file);
      fos.write(data);

      fos.flush();
      fos.close();

      System.out.println("下载成功");
    } catch (IOException e) {
      e.printStackTrace();
    }

    List<DemoData> sheetDatas = EasyExcel.read("xzq_201907.xlsx").head(DemoData.class).sheet(0).doReadSync();
    for (DemoData rowData : sheetDatas) {
      System.out.println(JSON.toJSONString(rowData));
    }

  }
}
6.1cookie
import java.io.IOException;
import okhttp3.Call;
import okhttp3.OkHttpClient;
import okhttp3.Request;
import okhttp3.Response;

public class PageAsker {

  /**
   * 根据输入的url，读取页面内容并返回
   */
  public String getContent(String url) {
    // okHttpClient 实例
    OkHttpClient okHttpClient = new OkHttpClient();
    // 定义一个request
    Request request = new Request.Builder()
        .url(url)
        .addHeader("User-Agent", "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_1) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/78.0.3904.108 Safari/537.36")
        .addHeader("Referer", "https://www.douban.com")
        .addHeader("Host", "www.douban.com")
        .addHeader("Cookie", ReadFileTool.readContent("cookie.txt"))
        .build();
    // 返回结果字符串
    String result = null;
    try {
      // 执行请求
      Response response = okHttpClient.newCall(request).execute();
      result = response.body().string();
    } catch (IOException e) {
      System.out.println("request " + url + " error . ");
      e.printStackTrace();
    }
    return result;
  }

  public static void main(String[] args) {
    String url = "https://www.douban.com/mine";
    PageAsker asker = new PageAsker();
    String content = asker.getContent(url);

    System.out.println(content);
  }
}



系统提供的工具类，用于内容
import java.io.FileReader;
import java.io.IOException;

/**
 * 读文件工具类
 */
public class ReadFileTool {

  /**
   * 读取指定文件的内容
   */
  public static String readContent(String fileName) {
    FileReader fr = null;
    StringBuilder strBuilder = new StringBuilder();
    try {
      fr = new FileReader(fileName);
      int ch = 0;
      while ((ch = fr.read()) != -1) {
        strBuilder.append((char) ch);
      }
    } catch (IOException e) {
      e.printStackTrace();
    } finally {
      try {
        if (fr != null) {
          fr.close();
        }
      } catch (IOException e) {
        e.printStackTrace();
      }
    }

    return strBuilder.toString();
  }
}
6.2session
import java.io.IOException;
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
import okhttp3.Cookie;
import okhttp3.CookieJar;
import okhttp3.FormBody;
import okhttp3.FormBody.Builder;
import okhttp3.HttpUrl;
import okhttp3.OkHttpClient;
import okhttp3.Request;

public class PageLoginer {

  // 用 CookieJar 实现 cookie 的存储，便于登录后请求其它 URL 可以复用
  private static final OkHttpClient okHttpClient = new OkHttpClient.Builder()
      .cookieJar(new CookieJar() {
        private final HashMap<String, List<Cookie>> cookieStore = new HashMap<>();

        @Override
        public void saveFromResponse(HttpUrl url, List<Cookie> cookies) {
          cookieStore.put("mtime.com", cookies);
          System.out.println("[saveFromResponse]url.host()=" + url.host());
        }

        @Override
        public List<Cookie> loadForRequest(HttpUrl url) {
          System.out.println("[loadForRequest]url.host()=" + url.host());
          List<Cookie> cookies = cookieStore.get("mtime.com");
          return cookies != null ? cookies : new ArrayList<>();
        }
      })
      .build();

  public String postContent(String url, Map<String, String> formData) {
    //post方式提交的数据
    Builder builder = new FormBody.Builder();
    // 放入表单数据
    for (String key : formData.keySet()) {
      builder.add(key, formData.get(key));
    }
    // 构建 FormBody 对象
    FormBody formBody = builder.build();
    // 指定 post 方式提交FormBody
    Request request = new Request.Builder()
        .url(url)
        .post(formBody)
        .addHeader("User-Agent",
            "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_1) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/78.0.3904.108 Safari/537.36")
        .addHeader("Referer",
            "http://www.mtime.com/")
        .addHeader("Host", "front-gateway.mtime.com")
        .build();

    // 返回结果字符串
    String result = null;
    try {
      result = okHttpClient.newCall(request).execute().body().string();
    } catch (IOException e) {
      System.out.println("request " + url + " error . ");
      e.printStackTrace();
    }
    return result;
  }

  public static void main(String[] args) {
    // 登录页面 url
    String url = "https://front-gateway.mtime.com/user/user/login.api";
    // 登录表单数据
    Map<String, String> formData = new HashMap();
    formData.put("name", "13777467803");
    formData.put("password", "a357c62a83bb0f441e0fecd5aef50f99");

    PageLoginer asker = new PageLoginer();
    String content = asker.postContent(url, formData);

    System.out.println(content);
  }
}

6.3复用SESSION
import java.io.IOException;
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
import okhttp3.Cookie;
import okhttp3.CookieJar;
import okhttp3.FormBody;
import okhttp3.FormBody.Builder;
import okhttp3.HttpUrl;
import okhttp3.OkHttpClient;
import okhttp3.Request;

public class PageLoginer {

  // 用 CookieJar 实现 cookie 的存储，便于登录后请求其它 URL 可以复用
  private static final OkHttpClient okHttpClient = new OkHttpClient.Builder()
      .cookieJar(new CookieJar() {
        private final HashMap<String, List<Cookie>> cookieStore = new HashMap<>();

        @Override
        public void saveFromResponse(HttpUrl url, List<Cookie> cookies) {
          cookieStore.put("mtime.com", cookies);
          //System.out.println("[saveFromResponse]url.host()=" + url.host());
        }

        @Override
        public List<Cookie> loadForRequest(HttpUrl url) {
          //System.out.println("[loadForRequest]url.host()=" + url.host());
          List<Cookie> cookies = cookieStore.get("mtime.com");
          return cookies != null ? cookies : new ArrayList<>();
        }
      })
      .build();

  /**
   * 向指定的 URL 提交数据，并返回提交后的结果
   */
  public String postContent(String url, Map<String, String> formData) {
    //post方式提交的数据
    Builder builder = new FormBody.Builder();
    // 放入表单数据
    for (String key : formData.keySet()) {
      builder.add(key, formData.get(key));
    }
    // 构建 FormBody 对象
    FormBody formBody = builder.build();
    // 指定 post 方式提交FormBody
    Request request = new Request.Builder()
        .url(url)
        .post(formBody)
        .addHeader("User-Agent",
            "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_1) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/78.0.3904.108 Safari/537.36")
        .addHeader("Referer",
            "http://www.mtime.com/")
        .addHeader("Host", "front-gateway.mtime.com")
        .build();

    return doExcute(request);
  }

  /**
   * 根据输入的url，读取页面内容并返回
   */
  public String getContent(String url) {
    // 定义一个request
    Request request = new Request.Builder()
        .url(url)
        .addHeader("User-Agent",
            "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_1) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/78.0.3904.108 Safari/537.36")
        .addHeader("Host", "my.mtime.com")
        .build();

    return doExcute(request);
  }

  private String doExcute(Request request) {
    // 返回结果字符串
    String result = null;
    try {
      result = okHttpClient.newCall(request).execute().body().string();
    } catch (IOException e) {
      // 抓取异常
      System.out.println("request " + request.url() + " error . ");
      e.printStackTrace();
    }

    return result;
  }

  public static void main(String[] args) {
    PageLoginer asker = new PageLoginer();

    // 完成登录
    String url = "https://front-gateway.mtime.com/user/user/login.api";
    // 登录表单数据
    Map<String, String> formData = new HashMap();
    formData.put("name", "13777467803");
    formData.put("password", "aa787bc9cc97ba5d27cc042ecffe1489");


    String content = asker.postContent(url, formData);
    System.out.println("login result:");
    System.out.println(content);

    // 请求个人设置页面
    String myUrl = "http://my.mtime.com/personal/personInfo";
    String myContent = asker.getContent(myUrl);
    System.out.println("personInfo result:");
    System.out.println(myContent);
    
  }
}
7.2JAVA MAIL邮件发送
  <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
        <java.version>1.8</java.version>
        <resource.delimiter>@</resource.delimiter>
        <maven.compiler.source>${java.version}</maven.compiler.source>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <maven.compiler.target>${java.version}</maven.compiler.target>
    </properties>

    <dependencies>
        <dependency>
            <groupId>javax.mail</groupId>      
            <artifactId>mail</artifactId>
            <version>1.4</version>
        </dependency>
    </dependencies>

</project>

import java.security.Security;
import java.util.Properties;
import javax.mail.Authenticator;
import javax.mail.Message;
import javax.mail.PasswordAuthentication;
import javax.mail.Session;
import javax.mail.Transport;
import javax.mail.internet.InternetAddress;
import javax.mail.internet.MimeMessage;
import com.sun.net.ssl.internal.ssl.Provider;

public class Mail {

  public static void main(String[] args) {
    try {
      // 设置SSL连接、邮件环境
      Security.addProvider(new Provider());
      final String SSL_FACTORY = "javax.net.ssl.SSLSocketFactory";
      Properties props = System.getProperties();
      props.setProperty("mail.smtp.host", "smtp.qq.com");
      props.setProperty("mail.smtp.socketFactory.class", SSL_FACTORY);
      props.setProperty("mail.smtp.socketFactory.fallback", "false");
      props.setProperty("mail.smtp.port", "465");
      props.setProperty("mail.smtp.socketFactory.port", "465");
      props.setProperty("mail.smtp.auth", "true");

      // 建立邮件会话
      Session session = Session.getDefaultInstance(props, new Authenticator() {
        // 身份认证
        protected PasswordAuthentication getPasswordAuthentication() {
          return new PasswordAuthentication("xxxx@qq.com", "xxxx"); // 账户 授权码
        }
      });

      // 建立邮件对象
      MimeMessage message = new MimeMessage(session);
      // 设置邮件的发件人、收件人、主题
      // 附带发件人名字
      message.setFrom(new InternetAddress("xxxx@qq.com"));
      message.setRecipients(Message.RecipientType.TO, "xxxx@qq.com");
      message.setSubject("hello");
      // 文本部分
      message.setContent("hello world", "text/html;charset=UTF-8");
      message.saveChanges();
      // 发送邮件
      Transport.send(message);
      // 打印成功信息
      System.out.println("发送成功");
    } catch (Exception e) {
      e.printStackTrace();
    }
  }
}

爬取歌单2.4示例



import com.alibaba.fastjson.JSON;
import com.youkeda.music.model.Artist;
import com.youkeda.music.model.Song;
import com.youkeda.music.service.SongCrawlerService;
import java.util.HashMap;
import java.util.Map;

/**
 * 音乐抓取服务的实现
 */
public class SongCrawlerServiceImpl implements SongCrawlerService {

  // 歌单数据仓库
  private Map<String, Artist> artists;

  private void init() {
    artists = new HashMap<>();
  }

  @Override
  public void start(String artistId) {
    // 参数判断，未输入参数则直接返回
    if (artistId == null || artistId.equals("")) {
      return;  
    }
    
    // 执行初始化
    init();
    
    // 取得整体数据对象
    Map returnData = getArtistObj(artistId);
    // 构建填充了属性的 Artist 实例
    Artist artist = buildArtist(returnData);
    
  }

  @Override
  public Artist getArtist(String artistId) {
    return null;
  }

  @Override
  public Song getSong(String artistId, String songId) {
    return null;
  }

  private Map getArtistObj(String artistId) {
    // 构建歌单url
    String aUrl = "http://neteaseapi.youkeda.com:3000/artists?id=" + artistId;
    // 调用 okhttp3 获取返回数据
    String content = getPageContentSync(aUrl);
    // 反序列化成 Map 对象
    Map returnData = JSON.parseObject(content, Map.class);
    
    return returnData;
  }

  private Artist buildArtist(Map returnData) {
    // 从 Map 对象中取得 歌单 数据。歌单也是一个子 Map 对象。
    Map artistData = (Map) returnData.get("artist");
    Artist artist = new Artist();
    artist.setId(artistData.get("id").toString());
    
    return artist;
  }


  /**
   * 仅演示
   */
  private String getPageContentSync(String url) {
    String result = null;

    return result;
  }
}


3.2服务设计（示例）
import com.alibaba.fastjson.JSON;
import com.youkeda.music.model.Artist;
import com.youkeda.music.model.Song;
import com.youkeda.music.service.SongCrawlerService;
import java.io.IOException;
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
import okhttp3.Call;
import okhttp3.OkHttpClient;
import okhttp3.Request;

/**
 * 音乐抓取服务的实现
 */
public class SongCrawlerServiceImpl implements SongCrawlerService {

  private static final String ARTIEST_API_PREFIX = "http://neteaseapi.youkeda.com:3000/artists?id=";

  // okHttpClient 实例
  private OkHttpClient okHttpClient;

  // 歌单数据仓库
  private Map<String, Artist> artists;

  private void init() {
    //1. 构建 okHttpClient 实例
    okHttpClient = new OkHttpClient();
    artists = new HashMap<>();
  }

  @Override
  public void start(String artistId) {
    // 参数判断，未输入参数则直接返回
    if (artistId == null || artistId.equals("")) {
      return;
    }

    // 执行初始化
    init();

    // 初始化歌曲及歌单
    initArtistHotSongs(artistId);
  }

  @Override
  public Artist getArtist(String artistId) {
    return artists.get(artistId);
  }

  @Override
  public Song getSong(String artistId, String songId) {
    Artist artist = artists.get(artistId);
    List<Song> songs = artist.getSongList();

    if (songs == null) {
      return null;
    }

    for (Song song : songs) {
      if (song.getId().equals(songId)) {
        return song;
      }
    }
    return null;
  }

  @SuppressWarnings("unchecked")
  private Map getArtistObj(String artistId) {
    // 构建歌单url
    String aUrl = ARTIEST_API_PREFIX + artistId;
    // 调用 okhttp3 获取返回数据
    String content = getPageContentSync(aUrl);
    // 反序列化成 Map 对象
    Map returnData = JSON.parseObject(content, Map.class);

    return returnData;
  }

  @SuppressWarnings("unchecked")
  private Artist buildArtist(Map returnData) {
    // 从 Map 对象中取得 歌单 数据。歌单也是一个子 Map 对象。
    Map artistData = (Map) returnData.get("artist");
    Artist artist = new Artist();
    artist.setId(artistData.get("id").toString());
    if (artistData.get("picUrl") != null) {
      artist.setPicUrl(artistData.get("picUrl").toString());
    }
    artist.setBriefDesc(artistData.get("briefDesc").toString());
    artist.setImg1v1Url(artistData.get("img1v1Url").toString());
    artist.setName(artistData.get("name").toString());
    artist.setAlias((List) artistData.get("alias"));
    return artist;
  }

  private List<Song> buildSongs(Map returnData) {
    // 从 Map 对象中取得一组 歌曲 数据
    List songsData = (List) returnData.get("hotSongs");
    List<Song> songs = new ArrayList<>();

    for (int i = 0; i < songsData.size(); i++) {
      Map songData = (Map) songsData.get(i);
      Song songObj = new Song();
      songObj.setId(songData.get("id").toString());
      songObj.setName(songData.get("name").toString());

      songs.add(songObj);
    }

    return songs;
  }

  /**
   * 根据输入的url，读取页面内容并返回
   */
  private String getPageContentSync(String url) {
    //2.定义一个request
    Request request = new Request.Builder().url(url).build();
    //3.使用client去请求
    Call call = okHttpClient.newCall(request);
    String result = null;
    try {
      //4.获得返回结果
      result = call.execute().body().string();
      System.out.println("call " + url + " , content's size=" + result.length());
    } catch (IOException e) {
      System.out.println("request " + url + " error . ");
      e.printStackTrace();
    }

    return result;
  }private void initArtistHotSongs(String artistId) {
    // 取得整体数据对象。
    Map returnData = getArtistObj(artistId);
    // 构建填充了属性的 Artist 实例
    Artist artist = buildArtist(returnData);
    // 构建一组填充了属性的 Song 实例
    List<Song> songs = buildSongs(returnData);
    // 歌曲填入歌单
    artist.setSongList(songs);
    // 存入本地
    artists.put(artist.getId(), artist);
    
  }

生成自己的SSH：  ssh-keygen -t rsa -b 4096 -C “3220880480@qq.com”
查看生成的SSH：cat ~/.ssh/id_rsa.pub
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQDSEN8jEHwvKZg53274PJ4PBuIRJO02B5WynyWtvvH8IVMv36dyirJuLhtRWjf0XGkG7PMcq7mo38azz1Y9p7jIlGrBbdK5MauwS2DjdPqLOEsjNy/85GEcMJ6U7dL9LjyWMlj/S1DCJWPxrmfsJylP8U6ZmWWIYBI0vk/o9AI3rOvAmpXJ3RvM3N2HiIzt19XTnfwYutd+qvm4oNGGh3x80Pj9hGUjMuCpVwFrq8h8bM186XtDeXjufJDjHvSui8QYdgM8gHaiuFO2+C7Nwb8cGdx6E8w2u8DahJuveuP1CumurWP4b3EGAJzy7BuDJk0Q4sYUB+SiaEZlus3oHEFomVZimbLoznV8mry28wlCX5OJIlvfsn8f6Un17ZRDU5ky8BhymNVEka4IvQi5u1Rm+FvEYE2VH3dD7gOHeWo+V6rs1b5sGWoQeKA7hPvi6sQ52ykfklZNdV3aD53eHsUZWaoEp29vHScxsGBOekZyjODmjmbTPMmGU+yX7l4p6cy0mr3BznthVm6gx10vE36676qF6DSSsIJnBpeknpQUIcFdH/4R5vASVWcFW7He4BM7rUBp1OcIEL7LqJm4KgZ1a5PltAqWbc9FqrxZmQ2CQ8vSC6KjOOLWod/RTrl/HIOXivYsY9jsazh9PKg/aM8bu4vRaukTRP3PmHK7wbx7vQ== your_email@example.com

git config --global user.name "zhangyu"
git config --global user.email "3220880480@qq.com"
下载仓库地址
 git@github.com:xshiguangdaoliux/GitHubxshiguangdaoliux.github.io.git

Git clone git@github.com:xshiguangdaoliux/GitHubxshiguangdaoliux.github.io.git

常见的LINUX命令
查看当前目录下的文件： ls -a
查看当前目录下的文件，包括隐藏文件：ls -a
在当前目录下创建文件：touch 文件
在当前目录下创建文件夹： mkdir 文件夹
进入某个文件夹：cd 文件目录
删除当前目录下的指定文件; rm -rf 文件

Git 提交三步走：
1.git add
2.Git commmit
3.Git push

具体为：
1.  git add -A
2.  git commit -m "本次提交的修改的备注"
3.（第一次提交）  git push origin main
4.（第2-n此提交） git push


2.4绑定本地仓库到GITHUB
1.在GITHUB创建一个同名的空仓库
2.在本地也创建同名的文件夹
3.使用git init   命令将这个文件夹初始化为一个git仓库
4.git remote -v 查看当前的git仓库有没有绑定的远程仓库（空白即无绑定）
5.git remote add origin 仓库地址        (绑定远程仓库)

3.3Git分支&Git ignore
1.创建分支  git branch dev    （全部以分支名字为dev示例）
2.切换到新分支  git checkout dev
通常我们会把这2个命令合并使用    git checkout -b dev

分支的应用&Git ignore
进入博客文件夹 cd blog 打开里面的  .gitignore文件夹，（如果没有，自己创建）
看到里面内容：
.DS_Store
Thumbs.db
db.json
*.log
node_modules/
public/
.deploy*/

如果没有public/    就自己加

提交到新分支  先把它变成git仓库（）只需要绑定Git地址，不需要提交
绑定成功后我们处于main分支，需要切换到dev新分支，然后在dev分支完成代码提交






























3.1初认识Hexo
1.安装Hexo(课程环境已经帮我们安装好了)
2.创建我们的博客文件夹  hexo init blog   (可以把blog自定义为想起的名字)
3.安装发布工具 首先进入博客目录 cd blog,在terminal中输入  
npm install hexo-deployer-git --save （注意，每当创建新的博客文件夹，就需要重新安装）
进入博客文件夹。找到  _config.yml  这个文件夹，打开，输入配置：
theme: landscape

deploy:

  type: git

  repository: 


  branch: main
(theme:后面有空格，同理，type 和P对齐
)
然后可以将博客文件夹发布到线，替换我们原来的内容。


Hexo 提交
Hexo提交三步走
1.  hexo clean
2.  hexo generate
3.  Hexo deploy

3.2Hexo 的应用
1.创建博客 hexo new 博客名（会产生一个新的同名文件，在source/-posts内，找到并编辑）
2.发布博客（三步走）
跟换主题
在博客根目录中使用命令
先 cd blog
git clone https://gitee.com/xiaoyebing/hexo-theme-next.git themes/next
然后打开-config.yml文件，将配置中theme: landscape 改为theme: next
然后发布博客（三步走）
音乐实战3.3参考
import com.alibaba.fastjson.JSON;
import com.youkeda.music.model.Artist;
import com.youkeda.music.model.Song;
import com.youkeda.music.service.SongCrawlerService;
import java.io.IOException;
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

public class SongCrawlerServiceImpl implements SongCrawlerService {

  private static final String ARTIEST_API_PREFIX = "http://neteaseapi.youkeda.com:3000/artists?id=";
  private static final String S_D_API_PREFIX = "http://neteaseapi.youkeda.com:3000/song/detail?ids=";
  private Map<String, Artist> artists;

  @Override
  public void start(String artistId) {
  }

  @Override
  public Artist getArtist(String artistId) {
    return artists.get(artistId);
  }

  private Map getSourceDataObj(String prefix, String postfix) {
    // 构建歌单url
    String aUrl = prefix + postfix;

    return null;//这一行仅演示，不要照抄哦
  }

  private void initArtistHotSongs(String artistId) {
    // 取得整体数据对象。
    getSourceDataObj(ARTIEST_API_PREFIX, artistId);
  }

  private void assembleSongDetail(String artistId) {
    Artist artist = getArtist(artistId);
    List<String> songIds = new ArrayList<>();
    List<Song> songs = artist.getSongList();
    for (Song song : songs) {
      songIds.add(song.getId());
    }

    String sIdsParam = String.join(",", songIds);
    // 抓取结果
    Map songsDetailObj = getSourceDataObj(S_D_API_PREFIX, sIdsParam);
    // 原始数据中的 songs 是歌曲列表
    List<Map> sourceSongs = (List<Map>) songsDetailObj.get("songs");
    // 临时的 Map
    Map<String, Map> sourceSongsMap = new HashMap<>();
    // 遍历歌曲列表
    for (Map songSourceData : sourceSongs) {
      String sId = songSourceData.get("id").toString();
      // 原始歌曲数据对象放入一个临时的 Map 中
      sourceSongsMap.put(sId, songSourceData);
    }
    
    // 再次遍历歌单中的歌曲，填入详情数据
    for (Song song : songs) {
      String sId = song.getId();
      // 从临时的Map中取得对应的歌曲源数据，使用id直接获取，比较方便
      Map songSourceData = sourceSongsMap.get(sId);
    }
    
  }
}


音乐实战3.4参考
public class SongCrawlerServiceImpl implements SongCrawlerService {

  private static final String ARTIEST_API_PREFIX = "http://neteaseapi.youkeda.com:3000/artists?id=";
  private static final String S_D_API_PREFIX = "http://neteaseapi.youkeda.com:3000/song/detail?ids=";
  // 歌曲评论 API
  private static final String S_C_API_PREFIX = "http://neteaseapi.youkeda.com:3000/comment/music?id=";
  private Map<String, Artist> artists;

  @Override
  public void start(String artistId) {
  }

  @Override
  public Artist getArtist(String artistId) {
    return artists.get(artistId);
  }

  private Map getSourceDataObj(String prefix, String postfix) {
    // 构建歌单url
    String aUrl = prefix + postfix;

    return null;//这一行仅演示，不要照抄哦
  }

  private void initArtistHotSongs(String artistId) {
    // 取得整体数据对象。
    getSourceDataObj(ARTIEST_API_PREFIX, artistId);
  }

  private void assembleSongDetail(String artistId) {
    Artist artist = getArtist(artistId);
    List<String> songIds = new ArrayList<>();
    List<Song> songs = artist.getSongList();
    for (Song song : songs) {
      songIds.add(song.getId());
    }

    String sIdsParam = String.join(",", songIds);
    // 抓取结果
    Map songsDetailObj = getSourceDataObj(S_D_API_PREFIX, sIdsParam);
    // 原始数据中的 songs 是歌曲列表
    List<Map> sourceSongs = (List<Map>) songsDetailObj.get("songs");
    
    // 遍历歌单中的歌曲，填入详情数据
    for (Song song : songs) {
      // 仅演示，具体代码省略
    }
  }

  private void assembleSongComment(String artistId) {
    Artist artist = getArtist(artistId);
    List<Song> songs = artist.getSongList();
    for (Song song : songs) {
      String sIdsParam = song.getId() + "&limit=5";
      // 抓取结果
      Map songsCommentObj = getSourceDataObj(S_C_API_PREFIX, sIdsParam);
      
    }
  }

  private void assembleSongUrl(String artistId) {

  }
}
