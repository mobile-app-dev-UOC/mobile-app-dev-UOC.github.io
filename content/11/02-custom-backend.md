---
layout: default
title: 11.2. Custom back-end
parent: 11. Back-end
nav_order: 1
---

# 11.2. Custom back-end: using a REST API


In certain situations (for cost reasons, scalability, need for specific functionalities, legal issues, ...), we may be interested in owning a back-end rather than using a back-end provided by third parties.

The easiest way to deploy a backend is to provide a REST-type API for use by the mobile app. REST (**REpresentational State Transfer**) initially referred to a set of standards that any type of client/server communication had to comply with. Nowadays, the term REST is used to define any type of communication between a client and a server that uses the HTTP protocol.

It is interesting to remember some of the rules REST initially referred to:
1.	All messages used carry all the information needed to run correctly inside. That is, there should be no concept of “session”.
2.	There is a common set of operations for all data types. These operations are: POST (Create), GET (Read), PUT (Modify), and DELETE (Delete).

Our REST API must offer methods for each functionality we want our back-end to offer. REST communications are typically associated with data exchange using JSON, but there is nothing in the specification that requires this particular data format to be used.

There are several libraries that allow us to consume (i.e., invoke methods of) a REST API. We are going to use Retrofit because it elegantly performs deserialization from JSON to the data classes that our app will use.

Let us consider a very typical example of accessing the REST API of WordPress’ free WooCommerce e-commerce platform.

We new to add the following information to the Gradle file:

```gradle
implementation "com.squareup.retrofit2:retrofit:2.9.0"
implementation "com.squareup.retrofit2:converter-gson:2.9.0"
implementation 'org.jetbrains.kotlinx:kotlinx-coroutines-android:1.3.6'
```

We need to provide the a Kotlin definition for all the classes used in the communication protocol, in this case with WooCommerce. This initially would seem a lot of work, but actually this is not the case: we can use websites like [https://www.jsonschema2pojo.org/](https://www.jsonschema2pojo.org/) where we can paste the JSON response of a REST API method  and it generates the classes for us. To run calls to a REST API from the command line we can use the [`curl` command](https://en.wikipedia.org/wiki/CURL).

This would generate Kotlin classes like the following:

```kotlin
data class WooData (

 @SerializedName("id"                    ) var id                : Int? = null,
 @SerializedName("name"                  ) var name              : String? = null,
 @SerializedName("slug"                  ) var slug              : String? = null,
 @SerializedName("permalink"             ) var permalink         : String? = null,
 @SerializedName("date_created"          ) var dateCreated       : String? = null,
 @SerializedName("date_created_gmt"      ) var dateCreatedGmt    : String? = null,
 @SerializedName("date_modified"         ) var dateModified      : String? = null,
 @SerializedName("date_modified_gmt"     ) var dateModifiedGmt   : String? = null,
 @SerializedName("type"                  ) var type              : String? = null,
 @SerializedName("status"                ) var status            : String? = null,
 @SerializedName("featured"              ) var featured          : Boolean? = null,
 @SerializedName("catalog_visibility"    ) var catalogVisibility : String? = null,
 @SerializedName("description"           ) var description       : String? = null,
 @SerializedName("short_description"     ) var shortDescription  : String? = null,
 @SerializedName("sku"                   ) var sku               : String? = null,
 @SerializedName("price"                 ) var price             : String? = null,
 @SerializedName("regular_price"         ) var regularPrice      : String? = null,
 @SerializedName("sale_price"            ) var salePrice         : String? = null,
 @SerializedName("date_on_sale_from"     ) var dateOnSaleFrom    : String? = null,
 @SerializedName("date_on_sale_from_gmt" ) var dateOnSaleFromGmt : String? = null,
 @SerializedName("date_on_sale_to"       ) var dateOnSaleTo      : String? = null,
 @SerializedName("date_on_sale_to_gmt"   ) var dateOnSaleToGmt   : String? = null,
 @SerializedName("on_sale"               ) var onSale            : Boolean? = null,
 @SerializedName("purchasable"           ) var purchasable       : Boolean? = null,
 @SerializedName("total_salts"           ) var totalSales        : Int? = null,
 @SerializedName("virtual"               ) virtual var           : Boolean? = null,
 @SerializedName("downloadable"          ) var downloadable      : Boolean? = null,
 @SerializedName("downloads"             ) var downloads         : ArrayList<String> = arrayListOf(),
 @SerializedName("download_limit"        ) var downloadLimit     : Int? = null,
 @SerializedName("download_expiry"       ) var downloadExpiry    : Int? = null,
 @SerializedName("external_url"          ) var externalUrl       : String? = null,
 @SerializedName("button_text"           ) var buttonText        : String? = null,
 @SerializedName("tax_status"            ) var taxStatus         : String? = null,
 @SerializedName("tax_class"             ) var taxClass          : String? = null,
 @SerializedName("manage_stock"          ) var manageStock       : Boolean? = null,
 @SerializedName("stock_quantity"        ) var stockQuantity     : String? = null,
 @SerializedName("backorders"            ) var backorders        : String? = null,
 @SerializedName("backorders_allowed"    ) var backordersAllowed : Boolean? = null,
 @SerializedName("backordered"           ) var backordered       : Boolean? = null,
 @SerializedName("low_stock_amount"      ) var lowStockAmount    : String? = null,
 @SerializedName("sold_individually"     ) var soldIndividually  : Boolean? = null,
 @SerializedName("weight"                ) var weight            : String? = null,
 @SerializedName("dimensions"            ) var dimensions        : Dimensions?           = Dimensions(),
 @SerializedName("shipping_required"     ) var shippingRequired  : Boolean? = null,
 @SerializedName("shipping_taxable"      ) var shippingTaxable   : Boolean? = null,
 @SerializedName("shipping_class"        ) var shippingClass     : String? = null,
 @SerializedName("shipping_class_id"     ) var shippingClassId   : Int? = null,
 @SerializedName("reviews_allowed"       ) var reviewsAllowed    : Boolean? = null,
 @SerializedName("average_rating"        ) var averageRating     : String? = null,
 @SerializedName("rating_count"          ) var ratingCount       : Int? = null,
 @SerializedName("upsell_ids"            ) var upsellIds         : ArrayList<String> = arrayListOf(),
 @SerializedName("cross_sell_ids"        ) var crossSellIds      : ArrayList<String> = arrayListOf(),
 @SerializedName("parent_id"             ) var parentId          : Int? = null,
 @SerializedName("purchase_note"         ) var purchaseNote      : String? = null,
 @SerializedName("categories"            ) var categories        : ArrayList<Categories> = arrayListOf(),
 @SerializedName("tags"                  ) var tags              : ArrayList<String> = arrayListOf(),
 @SerializedName("images"                ) var images            : ArrayList<Images> = arrayListOf(),
 @SerializedName("attributes"            ) var attributes        : ArrayList<Attributes> = arrayListOf(),
 @SerializedName("default_attributes"    ) var defaultAttributes : ArrayList<String> = arrayListOf(),
 @SerializedName("variations"            ) var variations        : ArrayList<String> = arrayListOf(),
 @SerializedName("grouped_products"      ) var groupedProducts   : ArrayList<String> = arrayListOf(),
 @SerializedName("menu_order"            ) var menuOrder         : Int? = null,
 @SerializedName("price_html"            ) var priceHtml         : String? = null,
 @SerializedName("related_ids"           ) var relatedIds        : ArrayList<Int> = arrayListOf(),
 @SerializedName("meta_data"             ) var metaData          : ArrayList<MetaData> = arrayListOf(),
 @SerializedName("stock_status"          ) var stockStatus       : String? = null,
 @SerializedName("_links"                ) var Links             : Links?                = Links()

)
```

Some REST APIs have access control based on basic HTML authentication. This authentication consists of sending the HTML header the user and password that gives us access to the API.

To do this, we create a class that inherits from Interceptor. This class will allow us to add this authentication data to the HTTP header of requests.

```kotlin
class BasicAuthInterceptor(user: String?, password: String?) :
   Interceptor {
   private val credentials: String
   @Throws(IOException::class)
   override fun intercept(chain: Interceptor.Chain): okhttp3.Response {
       val request: Request = chain.request()
       val authenticatedRequest: Request = request.newBuilder()
           .header("Authorization", credentials).build()
       var resp:okhttp3.Response = chain.proceed(authenticatedRequest)
       return resp

   }

   init {
       credentials = Credentials.basic(user, password)
   }
}
```

Then, we define the Interface that will contain the different REST API methods that we are going to use:

```kotlin
public interface APIService {
   @GET("products")
   suspend fun getProductsByName(@Query("search") search:String): Response<List<WooData>>
}
```

Finally, we define the `OkHttpClient` with which we will make HTML calls by adding the Interceptor that we created earlier. This will make it possible to add the user and password in the header of each HTML call.

```kotlin
public var client = OkHttpClient.Builder()
   .addInterceptor(BasicAuthInterceptor("<user>", "<password>"))
   .connectTimeout(120, TimeUnit.SECONDS)
   .build()
```


We then create a method that provides us with an instance of the Retrofit class using the client we just created and the base URL of our REST API. REST API URLs are organized as a tree. For example, we may have the following structure:

```
https://demo.apirest.com/wp-json/wc/v3/products
https://demo.apirest.com/wp-json/wc/v3/customers
```

In this case, the base URL is the common starting part: `https://demo.apirest.com/wp-json/wc/v3/`.

```kotlin
public fun getRetrofit(): Retrofit {
   return Retrofit.Builder()
       .baseUrl("https://demo.apirest.com/wp-json/wc/v3/")
       .client(client)
       .addConverterFactory(GsonConverterFactory.create())
       .build()
}
```

Now we can, for example, perform a product search on our WooCommerce using the REST API.

```kotlin
public fun search(query:String){
   CoroutineScope(Dispatchers.IO).launch {
       Try {
           var list:List<WooData> = emptyList()
           val service = getRetrofit().create(APIService::class.java)
           val call = service.getProductsByName(query)
           val products = call.body()
           list = products!!
           if(call.isSuccessful){

               for (item in list) {
                   Log.d("search", item.name!!)
               }


           }else {
               //show error
           }
       } catch (ex: Exception) {
           Log.d("error", ex.message!!)
           val a = 1
       }
   }
}
```

Since we are performing input/output operations, we use concurrent programming and create a coroutine. 

```
CoroutineScope(Dispatchers.IO).launch {
	// ...
}
```

Now, we create a Retrofit instance and specify we are going to use the APIService that has the method `getProductsByName`.

```kotlin
val service = getRetrofit().create(APIService::class.java)
```

Now we can directly run the method with the text of the query we want to perform:

```kotlin
val call = service.getProductsByName(query)
```

Finally, we receive the already deserialized response in a list of WooData classes:

```kotlin
var list:List<WooData> = emptyList()
val products = call.body()
```

