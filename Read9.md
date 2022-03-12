# Read: 09 - WRRC and Java  

- HTTP Request Lifecycle

   1. Local Processing : Browser will then go through its own cache of recently accessed URLs, the operating system’s cache of recent queries, router’s cache, and DNS cache to extract the “scheme”/protocol, host, optional port number, resource path, and query strings that are given in the form.  
   2. Resolve an IP : If the cache lookup fails, the browser sends a UDP3 DNS request to its target DNS server. If a response isn’t received, the request will be forwarded to another server. This is because the server looks for the address associated with the requested hostname and sends a response. If it doesn’t arrive, the client has no idea how long it will take.  
   4. Establish a TCP Connection : Before the server can accept a three-way handshake, the client must first create a TCP connection. The server must be listening on a port and executing a passive open, after which the client can conduct an active open and the client-server connection will be recognized. This allows the receiver to restore the original order of received packets and the sender to restore the original order of sent packets.  
   5. Send an HTTP Request : A request is submitted, consisting of a “request line,” a request header, and a body, and it passes through a routing mechanism. The server receives the request, analyzes it, and locates the requested resource before returning an HTTP response. The server generates a response and responds to the request.  
   6. Tearing Down and Cleaning Up : After receiving the response, the client sends a TCP FIN packet, to which the server answers with an ACK, and then sends its own FIN, to which the client responds with its own ACK signal. Then there will be a timeout. The four-way handshake is what it’s called. The browser begins to process the information it has received and then exits.  

### way of performing HTTP requests in Java  

- The ***HttpUrlConnection*** class allows us to perform basic HTTP requests without the use of any additional libraries. All the classes that we need are part of the java.net package.  
- We can create an ***HttpUrlConnection*** instance using the openConnection() method of the URL class. in this code we create a connection to a given URL using GET method (The HttpUrlConnection class is used for all types of requests by setting the requestMethod attribute to one of the values: GET, POST, HEAD, OPTIONS, PUT, DELETE, TRACE):
   ```
   URL url = new URL("http://example.com");
   HttpURLConnection con = (HttpURLConnection) url.openConnection();
   con.setRequestMethod("GET");
   ```
- If we want to add parameters to a request, we have to set the doOutput property to true, then write a String of the form param1=value¶m2=value to the OutputStream of the HttpUrlConnection instance:  
   ```
   Map<String, String> parameters = new HashMap<>();
   parameters.put("param1", "val");

   con.setDoOutput(true);
   DataOutputStream out = new DataOutputStream(con.getOutputStream());
   out.writeBytes(ParameterStringBuilder.getParamsString(parameters));
   out.flush();
   out.close();
   ```
- Adding headers to a request can be achieved by using the setRequestProperty() method:  
   ```
   con.setRequestProperty("Content-Type", "application/json");
   ```
- to read the value of a header from a connection, we can use the getHeaderField() method:  
   ```
   String contentType = con.getHeaderField("Content-Type");
   ```
- HttpUrlConnection class allows setting the connect and read timeouts. These values define the interval of time to wait for the connection to the server to be established or data to be available for reading.
   To set the timeout values, we can use the setConnectTimeout() and setReadTimeout() methods, In the example, we set both timeout values to five seconds:    
   ```
   con.setConnectTimeout(5000);
   con.setReadTimeout(5000);
   ```
- Handling Cookies : The java.net package contains classes that ease working with cookies such as CookieManager and HttpCookie.  
- to read the cookies from a response, we can retrieve the value of the Set-Cookie header and parse it to a list of HttpCookie objects.
  ```
  String cookiesHeader = con.getHeaderField("Set-Cookie");
  List<HttpCookie> cookies = HttpCookie.parse(cookiesHeader);
  ```
- Next, we will add the cookies to the cookie store:  
  ```
  cookies.forEach(cookie -> cookieManager.getCookieStore().add(null, cookie));
  ```
- to add the cookies to the request, we need to set the Cookie header, after closing and reopening the connection.  
  ```
  con.disconnect();
  con = (HttpURLConnection) url.openConnection();

  con.setRequestProperty("Cookie", 
  StringUtils.join(cookieManager.getCookieStore().getCookies(), ";"));
  ```
- We can enable or disable automatically following redirects for a specific connection by using the setInstanceFollowRedirects() method with true or false parameter:  
  ```
  con.setInstanceFollowRedirects(false);
  ```
- It is also possible to enable or disable automatic redirect for all connections:  
  ```
  HttpUrlConnection.setFollowRedirects(false);
  ```  
- To execute the request, we can use the getResponseCode(), connect(), getInputStream() or getOutputStream() methods:
  ```
  int status = con.getResponseCode();
  ```
- Reading the response of the request can be done by parsing the InputStream of the HttpUrlConnection instance.
  ```
  BufferedReader in = new BufferedReader(
  new InputStreamReader(con.getInputStream()));
  String inputLine;
  StringBuffer content = new StringBuffer();
  while ((inputLine = in.readLine()) != null) {
    content.append(inputLine);
  }
  in.close();
  ```
- To close the connection, we can use the disconnect() method:  
    ```
    con.disconnect();
    ```
### References:

- [High level HTTP](https://dev.to/dangolant/things-i-brushed-up-on-this-week-the-http-request-lifecycle-)
- [java http request](https://www.baeldung.com/java-http-request)

