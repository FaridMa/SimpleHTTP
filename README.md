# SimpleHTTP

[![CircleCI](https://circleci.com/gh/ffmammadov/SimpleHTTP/tree/master.svg?style=svg)](https://circleci.com/gh/ffmammadov/SimpleHTTP/tree/master) 
[![Dependabot Status](https://api.dependabot.com/badges/status?host=github&repo=ffmammadov/SimpleHTTP)](https://dependabot.com)
[![codecov](https://codecov.io/gh/ffmammadov/SimpleHTTP/branch/master/graph/badge.svg)](https://codecov.io/gh/ffmammadov/SimpleHTTP)
[![Quality Gate Status](https://sonarcloud.io/api/project_badges/measure?project=ffmammadov_SimpleHTTP&metric=alert_status)](https://sonarcloud.io/dashboard?id=ffmammadov_SimpleHTTP)
[![Code Smells](https://sonarcloud.io/api/project_badges/measure?project=ffmammadov_SimpleHTTP&metric=code_smells)](https://sonarcloud.io/dashboard?id=ffmammadov_SimpleHTTP)
[![Bugs](https://sonarcloud.io/api/project_badges/measure?project=ffmammadov_SimpleHTTP&metric=bugs)](https://sonarcloud.io/dashboard?id=ffmammadov_SimpleHTTP)

SimpleHTTP provides you most commonly used HTTP actions. You can open connection, send GET/POST request, get response as String. 

## Getting Started 

These instructions will help you get the app and use it in your code.

### Maven install

For add dependency to your maven project add the following code to your pom.xml

```xml
    <repositories>
        <repository>
            <id>ffmammadov-mvn-repo</id>
            <name>Project common</name>
            <url>https://github.com/ffmammadov/mvn-repo/raw/master</url>
        </repository>
    </repositories>
    <dependencies>
        <dependency>
            <groupId>com.github.ffmammadov</groupId>
            <artifactId>SimpleHTTP</artifactId>
            <version>2.0.0</version>
        </dependency>
    </dependencies>
```

check latest version at [this URL](https://github.com/FaridMa/mvn-repo/tree/master/com/github/ffmammadov/SimpleHTTP).

### Download jar 

.jar can be reached at the following url:
 
* [https://github.com/faridma/mvn-repo/raw/master/com/github/ffmammadov/SimpleHTTP/2.0.0/SimpleHTTP-2.0.0.jar](https://github.com/faridma/mvn-repo/raw/master/com/github/ffmammadov/SimpleHTTP/2.0.0/SimpleHTTP-2.0.0.jar)
where 2.0.0 is specific version;

## Usage

SimpleHTTP is used for basic HTTP operations with java. 

### Send HTTP GET Request

```java
public class Main {

    public static void main(String[] args) throws IOException {
        String url = "https://httpbin.org/user-agent";
        
        //Get HTTPS connection
        HttpsURLConnection c = HttpConnection.getSecureConnection(url, HttpMethod.GET);
        
        //Add needed request headers
        c.addRequestProperty("Accept", "*/*");
        
        //Get response code 
        int responseCode = c.getResponseCode();
        
        //Get returned response String
        String response = HttpConnection.getResponse(c, responseCode, StandardCharsets.UTF_8);
        
        //Check response code and handle data depending on your app logic
        switch (responseCode) {
            case 200:
                System.out.println(response);
                break;
            default:
                System.out.println("ERROR");

        }
    }
}
```

### Send HTTP POST Request

```java
public class Main {

    public static void main(String[] args) throws IOException {
        String url = "https://httpbin.org/post";
        
        //Get HTTPS connection, when method setted POST, doOutput is setted true;
        HttpsURLConnection c = HttpConnection.getSecureConnection(url, HttpMethod.POST);
        
        //Add needed request headers
        c.addRequestProperty("Content-Type",  MediaType.APPLICATION_JSON);
        
        //Add Basic Authorization Header
        HttpConnection.addBasicAuthorization(c, "FaridMa", "qwerty");
        
        String request = "{\n"
                + "	\"data\": \"some data\",\n"
                + "	\"id\": \"1234567890\"\n"
                + "}";
        
        //Sending some JSON to the endpoint
        HttpConnection.sendPost(c, request);
        
        //Get response code 
        int responseCode = c.getResponseCode();
        
        //Get returned response String
        String response = HttpConnection.getResponse(c, responseCode, StandardCharsets.UTF_8);
        
        //Check response code and handle data depending on your app logic
        switch (responseCode) {
            case 200:
                System.out.println(response);
                break;
            default:
                System.out.println("ERROR");

        }
    }
}
```
### Send HTTP multipart/form-data Request

```java
public class Main {

    public static void main(String[] args) throws IOException {
        String url = "https://httpbin.org/post";

        //Get HTTPS connection, when method setted POST, doOutput is setted true;
        HttpsURLConnection c = HttpConnection.getSecureConnection(url, HttpMethod.POST);

        //Create Multipart object to generate  multipart/form-data request
        MultiPart multiPart = new MultiPart(c);
        //Init multipart request by adding request Header Content-Type: multipart/form-data
        multiPart.init();
        //add field part to the request
        multiPart.addFieldPart("formPartOne", "some value for part one");
        //add file part to the request
        multiPart.addFilePart("formFile", new File(Main.class.getClassLoader().getResource("croco.jpg").getPath()), "image/jpg");

        //Get formed request String
        String request = multiPart.finish();

        //Sending some JSON to the endpoint
        HttpConnection.sendPost(c, request);

        //Get response code 
        int responseCode = c.getResponseCode();

        //Get returned response String
        String response = HttpConnection.getResponse(c, responseCode, StandardCharsets.UTF_8);

        //Check response code and handle data depending on your app logic
        switch (responseCode) {
            case 200:
                System.out.println(response);
                break;
            default:
                System.out.println("ERROR");

        }
    }

}
```
