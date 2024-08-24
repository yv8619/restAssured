## why rest assured ?
Its a java library for api automation testing. its an open source tool.
As it a java library it Can be easily integrated with testng, maven, junit, allure report.
It has the ability to verify http responses.
rest assured follows bdd approach which makes readabiltiy easy.

## How do you add Rest Assured to your project?
To add Rest Assured to your project, you'll need to include the Rest Assured library in your project's dependencies.
<dependency>
      <groupId>io.rest-assured</groupId>
      <artifactId>rest-assured</artifactId>
      <version>5.5.0</version>
      <scope>test</scope>
</dependency>

## Explain the main components of a Rest Assured test ?
- Base URI and Base Path
- RequestSpecification, ResponseSpecification
- http methods
- response
- assertion

## How do you perform a simple GET request using Rest Assured?
 Response response = given().baseUri("http://localhost:4001").when().get("/posts/1").then().statusCode(200);
 response.prettyPrint();
 response;
 reponse.then().body("userId", equalTo(1));


## what is static import in rest assured ?
static imports are used to improve the readability and simplicity of your test code.Rest Assured makes extensive 
use of static methods to build HTTP requests and assert responses. 

import static io.restassured.RestAssured.*;
public class ApiTest {
    @Test
    public void testGetUser() {
        given().baseUri("https").
        when().get("/users/1").
        then().statusCode(200);
    }
}

## How do you validate JSON responses in Rest Assured?
You can validate specific fields in the JSON response using the body method with matchers.
- response.then().statusCode(200);
- response.then().header("Content-Type", "application/json; charset=utf-8");
- response.then().body("userId", equalTo(1));

## How can you handle cookies in Rest Assured ?
You can extract cookies from a response using the getCookies method.
Map<String, String> cookies = response.getCookies();

## How do you handle query parameters in a Rest Assured request?
By using the queryParam or queryParams methods. These methods allow you to add single or multiple query parameters to your request.
Map<String, Object> queryParams = new HashMap<>();
- queryParams.put("userId", 1);
- queryParams.put("id", 1);

## How can you handle JSON response validation in Rest Assured when the expected JSON structure is complex and 
contains nested elements?
By using a combination of JSONPath expressions and the built-in matchers provided by Rest Assured. 

## How can you handle response caching in Rest Assured?
Key HTTP Headers for Caching
Cache-Control: Controls caching mechanisms in browsers and intermediate caches.
Expires: Provides a date/time after which the response is considered stale.
ETag: Provides a unique identifier for a specific version of a resource.
Last-Modified: Indicates the date and time at which the resource was last modified.

## how to set ssl cert in rest assured ?
By default, Rest Assured trusts all certificates, including self-signed certificates. This can be useful for 
testing environments but is not recommended for production.

## what are the authentications available ?
- basic authetication
- form based
- OAuth1
- OAuth2

## how to check response time in API ?
response.getTime();
response.getTimeIn(TimeUnit.SECONDS);

## What is the difference between given() and when() in Rest Assured?
given() is used to set up the request, while when() is used to send the request.

## How do you perform a POST request with a JSON payload in Rest Assured ?



## how to perform schema validation ?
If you want to validate that a JSON response conforms to a Json Schema you can use the json-schema-validator module
get("/products").then().assertThat().body(matchesJsonSchemaInClasspath("products-schema.json"));

<dependency>
      <groupId>io.rest-assured</groupId>
      <artifactId>json-schema-validator</artifactId>
      <version>5.5.0</version>
      <scope>test</scope>
</dependency>

## Explain the concept of Request Specification and Response Specification in Rest Assured ?
A RequestSpecification allows you to define common configurations for your HTTP requests, such as base URI,
headers, authentication, and other settings. Once defined, you can reuse this specification across multiple requests.

RequestSpecification requestSpec = given().
                                   baseUri("https://api.example.com").
                                   header("Content-Type", "application/json").
                                   auth().basic("username", "password");
Use in test - 
 given()
            .spec(requestSpec)
            .body("{\"key\": \"value\"}")
            .when()
            .post("/endpoint")
            .then()
            .statusCode(201);

A ResponseSpecification allows you to define common validations and checks for your HTTP responses. 
This is useful for applying the same response assertions across multiple tests.

ResponseSpecification responseSpec = expect().statusCode(200).header("Content-Type", "application/json")
                                          .body("status", equalTo("success"));

Use in test - 
 given()
            .baseUri("https://api.example.com")
            .when()
            .get("/endpoint")
            .then()
            .spec(responseSpec);


## Explain the purpose of the ‘extract().jsonPath()’ method in Rest Assured ?
The extract().jsonPath() method in Rest Assured is used to extract data from the JSON response of an HTTP request. 
This method allows you to parse the JSON response and extract specific values, which can then be used for further 
validation, processing, or in subsequent requests.

JsonPath jsonPath = given()
            .when()
            .get("/users/123")
            .then()
            .statusCode(200)
            .extract()
            .jsonPath();

        // Extract specific values from the JSON response
        int id = jsonPath.getInt("id");
        String name = jsonPath.getString("name");

## how to pass multiple headers ?
1) either use
Headers headers = new Headers();
2) using map
Map<String,Object> headers = new HashMap<>(MANDATORY_HEADERS);

## how to set multi-value headers ?
for multi vaue headers, rest assured creates 2 entries ( key1 value1, key1 value2). here key is duplicated with diff values. since map do not allow
duplicate entries we cannot use Map interface to set headers. Instead we need to use like this - 
Header header1 = new Header(key1, value1);
Header header1 = new Header(key1, value2);
Headers headers = new Headers(header1,header2);

## how to assert response header ?
To assert response header we need to use it under Then statement.
then().assertThat().statusCode(200).header(key,value);

## how to extract headers ?
Headers headers = given() ...   then().extract().headers();   
s.o.u.t(header.getValue("X-Journey-Id");

## what is Request specification ?
 allows you to predefine various aspects of a request, such as headers, query parameters, body content, cookies, authentication, and more. 
By using a RequestSpecification, you can reuse configurations across multiple tests, ensuring consistency and reducing redundancy in your test code.

## how to extract a specific field from josn repsonse ?
There are various ways like using jackson library, gson, rest assureds JsonPath().
String logErrorId = response.getBody().jsonBath().getString("logErrorid");

## how to do swagger compliance assertion ?
Using json schema validator library.
response().then().body(matchesJsonSchemaInClasspath("/src/test/resources/swagger.yaml"));

## how to send query param ?
By using param method or queryParam method.      
given().baseUri().queryParam("key","value")
given().baseUri().queryParam("key","value1","value2","value3")       // multi value query param.

Map<String,String> queryparam = new HashMap<>();
queryparam.put("key","value1")

## How to log response in Rest Assured only in the case of an error.
User ifError() method. It Logs everything only if an error occurs.
 RestAssured.given().baseUri("http://localhost:4001").contentType(ContentType.JSON).log().all().when().get("/users").
                 then().log().ifError().assertThat().statusCode(200);

## How to mask header information ?
By using the Blacklistheader method.
given().config(config().logConfig(logConfig().blacklistHeader("apikey")))

## How to check if a specific field is present or not in the JSON response?
when()
    .get("/author/123")
.then()
    .body("$", hasKey("lastname"))
    .body("$", not(hasKey("age")));
 

## how to Pass value from one API to another API ?
- store the response of first api using extract() method
Response ptyToken = given()
            .contentType("application/json")
            .body("{ \"username\": \"user\", \"password\": \"pass\" }")
            .when()
            .post("https://api.example.com/auth")
            .then()
            .statusCode(200)
            .extract().response();

- Step 2: Extract the token from the response
        String token = ptyToken.jsonPath().getString("token");
        
- Step 3: Use the extracted token in the second API call
        given()
            .header("Authorization", "Bearer " + token)
            .when()
            .get("https://api.example.com/protected-resource")
            .then()
            .statusCode(200)
            .body("data.id", equalTo(123));
    }
}














