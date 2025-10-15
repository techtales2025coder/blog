# Sync/Async api code


```java
import java.util.concurrent.CompletableFuture;
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;


public class SyncAndAsyncProcess {
   /**
    * Make http client call synchronously
    */
   public static HttpResponse<String> getSyncApiResponse() {
       System.out.println("Sync - starting api call");
       HttpClient httpClient = HttpClient.newHttpClient();
       HttpRequest httpRequest = HttpRequest.newBuilder()
               .uri(URI.create("https://api.weather.gov/alerts?limit=2"))
               .build();


       try {
           // Sync method
           HttpResponse<String> response = httpClient.send(httpRequest, HttpResponse.BodyHandlers.ofString());
           System.out.println("Sync Status Code: " + response.statusCode());
           System.out.println("Sync Response Body: === ");
       } catch (Exception e) {
           System.out.println("Exception occured: " + e);
       }
       return null;
   }


   /**
    * Make http client call asynchronously
    */
   public static CompletableFuture<HttpResponse<String>> getAsyncApiResponse() {
       System.out.println("Async - starting api call");
       HttpClient httpClient = HttpClient.newHttpClient();
       HttpRequest httpRequest = HttpRequest.newBuilder()
               .uri(URI.create("https://api.weather.gov/alerts?limit=2"))
               .build();


       // Async method
       CompletableFuture<HttpResponse<String>> response = httpClient.sendAsync(httpRequest,
               HttpResponse.BodyHandlers.ofString());
       return response;
   }


   public static void syncTrigger() {
       System.out.println("Sync - triggering api call");
       System.out.println(getSyncApiResponse());
       System.out.println("Sync - completed api call");
   }


   public static void asyncTrigger() {
       System.out.println("Async - triggering api call");
       System.out.println(getAsyncApiResponse()
               .thenAccept(res -> {
                   System.out.println("Async Status Code: " + res.statusCode());
                   System.out.println("Async Response Body: ===");
               }).join());
       System.out.println("Async - completed api call");
   }


   public static void main(String[] args) {
       System.out.println("==========Sync==========");
       syncTrigger();
       System.out.println("==========Async==========");
       asyncTrigger();
   }
}
```

