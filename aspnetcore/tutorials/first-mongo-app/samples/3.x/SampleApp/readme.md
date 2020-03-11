---
page_type: sample
description: Bu öğreticide, MongoDB NoSQL veritabanı kullanarak ASP.NET Core Web API 'sinin nasıl oluşturulacağı gösterilmektedir.
languages:
- csharp
products:
- dotnet-core
- aspnet-core
- vs
urlFragment: aspnetcore-webapi-mongodb
ms.openlocfilehash: 01f9cf237dcf2a9b95c181c2cb87ef9f59102244
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78663501"
---
# <a name="create-a-web-api-with-aspnet-core-and-mongodb"></a>MongoDB ile ASP.NET Core ile web API'si oluşturma

Bu öğretici, bir [MongoDB](https://www.mongodb.com/what-is-mongodb) NoSQL veritabanında oluşturma, okuma, güncelleştirme ve SILME (CRUD) işlemlerini gerçekleştiren BIR Web API 'si oluşturur.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

* MongoDB yapılandırın
* MongoDB veritabanı oluşturma
* MongoDB koleksiyonu ve şema tanımlayın
* Bir web API'sini MongoDB CRUD işlemleri gerçekleştirme
* JSON serileştirmesini özelleştirme

## <a name="prerequisites"></a>Önkoşullar

* [.NET Core SDK 3.0 veya üzeri](https://www.microsoft.com/net/download/all)
* **ASP.net ve Web geliştirme** iş yüküyle [Visual Studio 2019 Preview](https://visualstudio.microsoft.com/thank-you-downloading-visual-studio/?sku=community&ch=pre&rel=16&utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019preview)
* [MongoDB](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/)

## <a name="configure-mongodb"></a>MongoDB yapılandırın

Windows kullanıyorsanız MongoDB, varsayılan olarak *mongodb\\C:\\Program Files* 'a yüklenir. *\\Program Files\\MongoDB\\Server\\* \<Version_Number >\\`Path`) ortam değişkenine ekleyin. Bu değişiklik yerden MongoDB erişim sağlar, geliştirme makinenizde.

Mongo kabuğunu veritabanı oluşturma, koleksiyonları yapın ve belgeleri depolamak için aşağıdaki adımları kullanın. Mongo kabuğu komutları hakkında daha fazla bilgi için bkz. [Mongo kabuğu Ile çalışma](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell).

1. Geliştirme makinenizde verilerin depolanması için bir dizin seçin. Örneğin, *C: Windows üzerinde\\BooksData* . Yoksa dizini oluşturun. Mongo kabuğunu yeni dizinleri oluşturmaz.
1. Bir komut kabuğunu açın. Varsayılan bağlantı noktası 27017 mongodb'ye bağlanmak için aşağıdaki komutu çalıştırın. `<data_directory_path>`, önceki adımda seçtiğiniz dizinle değiştirmeyi unutmayın.

    ```console
    mongod --dbpath <data_directory_path>
    ```

1. Başka bir komut kabuğu örneği açın. Aşağıdaki komutu çalıştırarak varsayılan test veritabanı'na bağlanma:

    ```console
    mongo
    ```

1. Bir komut kabuğu'nda aşağıdaki komutu çalıştırın:

    ```console
    use BookstoreDb
    ```

    Zaten mevcut değilse, *BookstoreDb* adlı bir veritabanı oluşturulur. Veritabanı mevcut değilse, bağlantı işlemleri için açılır.

1. Aşağıdaki komutu kullanarak bir `Books` koleksiyonu oluşturun:

    ```console
    db.createCollection('Books')
    ```

    Aşağıdaki sonucu görüntülenir:

    ```console
    { "ok" : 1 }
    ```

1. `Books` koleksiyonu için bir şema tanımlayın ve aşağıdaki komutu kullanarak iki belge ekleyin:

    ```console
    db.Books.insertMany([{'Name':'Design Patterns','Price':54.93,'Category':'Computers','Author':'Ralph Johnson'}, {'Name':'Clean Code','Price':43.15,'Category':'Computers','Author':'Robert C. Martin'}])
    ```

    Aşağıdaki sonucu görüntülenir:

    ```console
    {
      "acknowledged" : true,
      "insertedIds" : [
        ObjectId("5bfd996f7b8e48dc15ff215d"),
        ObjectId("5bfd996f7b8e48dc15ff215e")
      ]
    }
    ```

  > [!NOTE]
  > Bu makalede gösterilen KIMLIK, bu örneği çalıştırdığınızda kimliklerle eşleşmeyecektir.

1. Aşağıdaki komutu kullanarak veritabanında belgelerini görüntüleyin:

    ```console
    db.Books.find({}).pretty()
    ```

    Aşağıdaki sonucu görüntülenir:

    ```console
    {
      "_id" : ObjectId("5bfd996f7b8e48dc15ff215d"),
      "Name" : "Design Patterns",
      "Price" : 54.93,
      "Category" : "Computers",
      "Author" : "Ralph Johnson"
    }
    {
      "_id" : ObjectId("5bfd996f7b8e48dc15ff215e"),
      "Name" : "Clean Code",
      "Price" : 43.15,
      "Category" : "Computers",
      "Author" : "Robert C. Martin"
    }
    ```

    Şema her belge için `ObjectId` türünde bir otomatik olarak `_id` özelliği ekler.

Veritabanı hazırdır. ASP.NET Core web API'si oluşturmaya başlayabilirsiniz.

## <a name="create-the-aspnet-core-web-api-project"></a>ASP.NET Core web API projesi oluşturma

1. **Dosya** > **Yeni** > **projesi**' ne gidin.
1. **ASP.NET Core Web uygulaması** proje türünü seçin ve **İleri**' yi seçin.
1. Projeyi *Booksapı*olarak adlandırın ve **Oluştur**' u seçin.
1. **.NET Core** hedef çerçevesini ve **3,0 ASP.NET Core**seçin. **API** proje şablonunu seçin ve **Oluştur**' u seçin.
1. MongoDB için .NET sürücüsünün en son kararlı sürümünü öğrenmek üzere [NuGet galerisini ziyaret edin: MongoDB. Driver](https://www.nuget.org/packages/MongoDB.Driver/) . **Paket Yöneticisi konsol** penceresinde, proje köküne gidin. MongoDB için .NET sürücüsünü yüklemek için aşağıdaki komutu çalıştırın:

    ```powershell
    Install-Package MongoDB.Driver -Version {VERSION}
    ```

## <a name="add-an-entity-model"></a>Varlık modeli ekleme

1. Proje köküne *modeller* dizini ekleyin.
1. *Modeller* dizinine aşağıdaki kodla bir `Book` sınıfı ekleyin:

    ```csharp
    using MongoDB.Bson;
    using MongoDB.Bson.Serialization.Attributes;

    namespace BooksApi.Models
    {
        public class Book
        {
            [BsonId]
            [BsonRepresentation(BsonType.ObjectId)]
            public string Id { get; set; }

            [BsonElement("Name")]
            public string BookName { get; set; }

            public decimal Price { get; set; }

            public string Category { get; set; }

            public string Author { get; set; }
        }
    }
    ```

    Önceki sınıfta `Id` özelliği:

    * Ortak dil çalışma zamanı (CLR) nesnesini MongoDB koleksiyonuna eşlemek için gereklidir.
    * , Bu özelliği belgenin birincil anahtarı olarak belirlemek için [`[BsonId]`](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonIdAttribute.htm) ile açıklama eklenir.
    * , Parametreyi [ObjectID](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_ObjectId.htm) yapısı yerine tür `string` olarak geçirmeyi sağlayan [`[BsonRepresentation(BsonType.ObjectId)]`](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonRepresentationAttribute.htm) ile açıklama eklenir. Mongo, `string` dönüştürmeyi `ObjectId`olarak işler.

    `BookName` özelliğine [`[BsonElement]`](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonElementAttribute.htm) özniteliğiyle açıklama eklenir. Özniteliğin `Name` değeri, MongoDB koleksiyonundaki özellik adını temsil eder.

## <a name="add-a-configuration-model"></a>Yapılandırma modeli ekleme

1. Aşağıdaki veritabanı yapılandırma değerlerini *appSettings. JSON*öğesine ekleyin:

    ```javascript
    {
      "BookstoreDatabaseSettings": {
        "BooksCollectionName": "Books",
        "ConnectionString": "mongodb://localhost:27017",
        "DatabaseName": "BookstoreDb"
      },

    ```

1. *Modeller* dizinine aşağıdaki kodla bir *BookstoreDatabaseSettings.cs* dosyası ekleyin:

    ```csharp
    namespace BooksApi.Models
    {
        public class BookstoreDatabaseSettings : IBookstoreDatabaseSettings
        {
            public string BooksCollectionName { get; set; }
            public string ConnectionString { get; set; }
            public string DatabaseName { get; set; }
        }

        public interface IBookstoreDatabaseSettings
        {
            string BooksCollectionName { get; set; }
            string ConnectionString { get; set; }
            string DatabaseName { get; set; }
        }
    }
    ```

    Yukarıdaki `BookstoreDatabaseSettings` sınıfı, *appSettings. JSON* dosyasının `BookstoreDatabaseSettings` özellik değerlerini depolamak için kullanılır. JSON ve C# Özellik adları, eşleme sürecini kolaylaştırmak için aynı şekilde adlandırılır.

1. Aşağıdaki Vurgulanan kodu `Startup.ConfigureServices`ekleyin:

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.Configure<BookstoreDatabaseSettings>(
            Configuration.GetSection(nameof(BookstoreDatabaseSettings)));

        services.AddSingleton<IBookstoreDatabaseSettings>(sp =>
            sp.GetRequiredService<IOptions<BookstoreDatabaseSettings>>().Value);

        services.AddControllers();
    }
    ```

    Önceki kodda:

    * *AppSettings. JSON* dosyasının `BookstoreDatabaseSettings` bölüm bağlandığı yapılandırma örneği, bağımlılık ekleme (dı) kapsayıcısına kaydedilir. Örneğin, bir `BookstoreDatabaseSettings` nesnesinin `ConnectionString` özelliği *appSettings. JSON*içindeki `BookstoreDatabaseSettings:ConnectionString` özelliği ile doldurulur.
    * `IBookstoreDatabaseSettings` arabirimi, tek bir [hizmet ömrü](xref:fundamentals/dependency-injection#service-lifetimes)ile dı 'ye kaydedilir. Eklerken, arabirim örneği bir `BookstoreDatabaseSettings` nesnesine çözümlenir.

1. `BookstoreDatabaseSettings` ve `IBookstoreDatabaseSettings` başvurularını çözümlemek için aşağıdaki kodu en *üst kısmına ekleyin* :

    ```csharp
    using BooksApi.Models;
    ```

## <a name="add-a-crud-operations-service"></a>CRUD işlemleri hizmeti ekleme

1. Proje köküne bir *Hizmetler* dizini ekleyin.
1. *Hizmetler* dizinine aşağıdaki kodla bir `BookService` sınıfı ekleyin:

    ```csharp
    using BooksApi.Models;
    using MongoDB.Driver;
    using System.Collections.Generic;
    using System.Linq;

    namespace BooksApi.Services
    {
        public class BookService
        {
            private readonly IMongoCollection<Book> _books;

            public BookService(IBookstoreDatabaseSettings settings)
            {
                var client = new MongoClient(settings.ConnectionString);
                var database = client.GetDatabase(settings.DatabaseName);

                _books = database.GetCollection<Book>(settings.BooksCollectionName);
            }

            public List<Book> Get() =>
                _books.Find(book => true).ToList();

            public Book Get(string id) =>
                _books.Find<Book>(book => book.Id == id).FirstOrDefault();

            public Book Create(Book book)
            {
                _books.InsertOne(book);
                return book;
            }

            public void Update(string id, Book bookIn) =>
                _books.ReplaceOne(book => book.Id == id, bookIn);

            public void Remove(Book bookIn) =>
                _books.DeleteOne(book => book.Id == bookIn.Id);

            public void Remove(string id) =>
                _books.DeleteOne(book => book.Id == id);
        }
    }
    ```

    Yukarıdaki kodda, Oluşturucu ekleme yoluyla dı 'den bir `IBookstoreDatabaseSettings` örneği alınır. Bu teknik, [yapılandırma modeli ekleme](#add-a-configuration-model) bölümüne eklenen *appSettings. JSON* yapılandırma değerlerine erişim sağlar.

1. Aşağıdaki Vurgulanan kodu `Startup.ConfigureServices`ekleyin:

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.Configure<BookstoreDatabaseSettings>(
            Configuration.GetSection(nameof(BookstoreDatabaseSettings)));

        services.AddSingleton<IBookstoreDatabaseSettings>(sp =>
            sp.GetRequiredService<IOptions<BookstoreDatabaseSettings>>().Value);

        services.AddSingleton<BookService>();

        services.AddControllers();
    }
    ```

    Önceki kodda, `BookService` sınıfı, tüketen sınıflarda Oluşturucu ekleme işlemini desteklemek için DI ile kaydedilir. `BookService`, `MongoClient`doğrudan bağımlılığı sağladığından, tek hizmet ömrü en uygundur. Resmi [Mongo istemci yeniden kullanım yönergeleri](https://mongodb.github.io/mongo-csharp-driver/2.8/reference/driver/connecting/#re-use)temelinde `MongoClient`, tek bir hizmet ömrü ile birlikte kaydedilmelidir.

1. `BookService` başvurusunu çözümlemek için aşağıdaki kodu *Startup.cs* 'un en üstüne ekleyin:


    ```csharp
    using BooksApi.Services;
    ```

`BookService` sınıfı, veritabanında CRUD işlemleri gerçekleştirmek için aşağıdaki `MongoDB.Driver` üyeleri kullanır:

* [Mongoclient](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoClient.htm) &ndash;, veritabanı işlemlerini gerçekleştirmek için sunucu örneğini okur. Bu sınıfın oluşturucusu, MongoDB bağlantı dizesini sağlanır:

    ```csharp
    public BookService(IBookstoreDatabaseSettings settings)
    {
        var client = new MongoClient(settings.ConnectionString);
        var database = client.GetDatabase(settings.DatabaseName);

        _books = database.GetCollection<Book>(settings.BooksCollectionName);
    }
    ```

* [Imongodatabase](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_IMongoDatabase.htm) &ndash; işlemleri gerçekleştirmek Için Mongo veritabanını temsil eder. Bu öğretici, belirli bir koleksiyondaki verilere erişim kazanmak için arabirimindeki genel [GetCollection\<TDocument > (koleksiyon)](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoDatabase_GetCollection__1.htm) yöntemini kullanır. Bu yöntem çağrıldıktan sonra, koleksiyonda CRUD işlemleri gerçekleştirin. `GetCollection<TDocument>(collection)` Yöntem çağrısında:
  * `collection`, koleksiyon adını temsil eder.
  * `TDocument`, koleksiyonda depolanan CLR nesne türünü temsil eder.

`GetCollection<TDocument>(collection)`, koleksiyonu temsil eden bir [Mongocollection](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoCollection.htm) nesnesi döndürür. Bu öğreticide, aşağıdaki yöntemlerden koleksiyonunda çağrılır:

* [Deleteone](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_DeleteOne.htm) &ndash;, belirtilen arama ölçütleriyle eşleşen tek bir belgeyi siler.
* [\<TDocument > bul](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollectionExtensions_Find__1_1.htm) &ndash;, koleksiyonda belirtilen arama ölçütleriyle eşleşen tüm belgeleri döndürür.
* [Insertone](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_InsertOne.htm) &ndash;, belirtilen nesneyi koleksiyondaki yeni bir belge olarak ekler.
* [Replaceone](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_ReplaceOne.htm) &ndash;, belirtilen arama ölçütleriyle eşleşen tek belgeyi, belirtilen nesneyle değiştirir.

## <a name="add-a-controller"></a>Denetleyici ekleme

*Controllers* dizinine aşağıdaki kodla bir `BooksController` sınıfı ekleyin:

```csharp
using BooksApi.Models;
using BooksApi.Services;
using Microsoft.AspNetCore.Mvc;
using System.Collections.Generic;

namespace BooksApi.Controllers
{
    [Route("api/[controller]")]
    [ApiController]
    public class BooksController : ControllerBase
    {
        private readonly BookService _bookService;

        public BooksController(BookService bookService)
        {
            _bookService = bookService;
        }

        [HttpGet]
        public ActionResult<List<Book>> Get() =>
            _bookService.Get();

        [HttpGet("{id:length(24)}", Name = "GetBook")]
        public ActionResult<Book> Get(string id)
        {
            var book = _bookService.Get(id);

            if (book == null)
            {
                return NotFound();
            }

            return book;
        }

        [HttpPost]
        public ActionResult<Book> Create(Book book)
        {
            _bookService.Create(book);

            return CreatedAtRoute("GetBook", new { id = book.Id.ToString() }, book);
        }

        [HttpPut("{id:length(24)}")]
        public IActionResult Update(string id, Book bookIn)
        {
            var book = _bookService.Get(id);

            if (book == null)
            {
                return NotFound();
            }

            _bookService.Update(id, bookIn);

            return NoContent();
        }

        [HttpDelete("{id:length(24)}")]
        public IActionResult Delete(string id)
        {
            var book = _bookService.Get(id);

            if (book == null)
            {
                return NotFound();
            }

            _bookService.Remove(book.Id);

            return NoContent();
        }
    }
}
```

Önceki web API denetleyicisi:

* CRUD işlemleri gerçekleştirmek için `BookService` sınıfını kullanır.
* GET, POST, PUT ve DELETE HTTP isteklerini desteklemek için eylem yöntemleri içerir.
* Bir [HTTP 201](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) yanıtı döndürmek için `Create` eylemi yönteminde <xref:System.Web.Http.ApiController.CreatedAtRoute*> çağırır. Durum kodu 201, sunucuda yeni bir kaynak oluşturan HTTP POST yöntemi için standart yanıttır. `CreatedAtRoute` Ayrıca yanıta bir `Location` üst bilgisi ekler. `Location` üstbilgisi yeni oluşturulan kitabın URI 'sini belirtir.

## <a name="test-the-web-api"></a>Web API 'sini test etme

1. Uygulamayı derleyin ve çalıştırın.

1. Denetleyicinin parametresiz `Get` eylem yöntemini sınamak için `http://localhost:<port>/api/books` gidin. Aşağıdaki JSON yanıtı gösterilir:

    ```json
    [
      {
        "id":"5bfd996f7b8e48dc15ff215d",
        "bookName":"Design Patterns",
        "price":54.93,
        "category":"Computers",
        "author":"Ralph Johnson"
      },
      {
        "id":"5bfd996f7b8e48dc15ff215e",
        "bookName":"Clean Code",
        "price":43.15,
        "category":"Computers",
        "author":"Robert C. Martin"
      }
    ]
    ```

1. Denetleyicinin aşırı yüklenmiş `Get` eylem yöntemini sınamak için `http://localhost:<port>/api/books/{id here}` gidin. Aşağıdaki JSON yanıtı gösterilir:

    ```json
    {
      "id":"{ID}",
      "bookName":"Clean Code",
      "price":43.15,
      "category":"Computers",
      "author":"Robert C. Martin"
    }
    ```

## <a name="configure-json-serialization-options"></a>JSON serileştirme seçeneklerini yapılandırma

[Web API 'Sini test](#test-the-web-api) etme bölümünde döndürülen JSON yanıtları hakkında iki ayrıntı vardır:

* ' Varsayılan ortası büyük/küçük harf özelliği, CLR nesnesinin özellik adlarının Pascal büyük küçük harfleriyle eşleşecek şekilde değiştirilmelidir.
* `bookName` özelliği `Name`olarak döndürülmelidir.

Önceki gereksinimleri karşılamak için aşağıdaki değişiklikleri yapın:

1. JSON.NET, ASP.NET paylaşılan çerçevesinden kaldırılmıştır. [Microsoft. AspNetCore. Mvc. NewtonsoftJson](https://nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson)öğesine bir paket başvurusu ekleyin.

1. `Startup.ConfigureServices`' de, aşağıdaki vurgulanmış kodu `AddMvc` yöntemi çağrısına zincirle:

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.Configure<BookstoreDatabaseSettings>(
            Configuration.GetSection(nameof(BookstoreDatabaseSettings)));

        services.AddSingleton<IBookstoreDatabaseSettings>(sp =>
            sp.GetRequiredService<IOptions<BookstoreDatabaseSettings>>().Value);

        services.AddSingleton<BookService>();

        services.AddControllers()
            .AddNewtonsoftJson(options => options.UseMemberCasing());
    }
    ```

    Önceki değişiklik ile, Web API 'sinin seri hale getirilmiş JSON yanıtındaki Özellik adları CLR nesne türündeki ilgili özellik adlarıyla eşleşir. Örneğin, `Book` sınıfının `Author` özelliği `Author`olarak serileştirir.

1. *Modeller/Book. cs*' de, `BookName` özelliğine aşağıdaki [`[JsonProperty]`](https://www.newtonsoft.com/json/help/html/T_Newtonsoft_Json_JsonPropertyAttribute.htm) özniteliğiyle açıklama ekleyin:

    ```csharp
    [BsonElement("Name")]
    [JsonProperty("Name")]
    public string BookName { get; set; }
    ```

    `[JsonProperty]` özniteliğinin değeri `Name`, Web API 'sinin serileştirilmiş JSON yanıtında özellik adını temsil eder.

1. `[JsonProperty]` öznitelik başvurusunu çözümlemek için *modeller/Book. cs* ' nin en üstüne aşağıdaki kodu ekleyin:

    ```csharp
    using Newtonsoft.Json;
    ```

1. [Web API 'Sini test](#test-the-web-api) etme bölümünde tanımlanan adımları yineleyin. JSON Özellik adlarındaki farka dikkat edin.

## <a name="next-steps"></a>Sonraki adımlar

ASP.NET Core web API'leri oluşturmaya daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Bu makalenin YouTube sürümü](https://www.youtube.com/watch?v=7uJt_sOenyo&feature=youtu.be)
* [ASP.NET Core ile Web API 'Leri oluşturma](https://docs.microsoft.com/aspnet/core/web-api/index?view=aspnetcore-3.0)
