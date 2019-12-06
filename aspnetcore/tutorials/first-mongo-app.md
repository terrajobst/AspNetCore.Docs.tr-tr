---
title: MongoDB ile ASP.NET Core ile web API'si oluşturma
author: prkhandelwal
description: Bu öğreticide, MongoDB NoSQL veritabanı kullanarak ASP.NET Core Web API 'sinin nasıl oluşturulacağı gösterilmektedir.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc, seodec18
ms.date: 08/17/2019
uid: tutorials/first-mongo-app
ms.openlocfilehash: 1425abbfc7bce6bdc445f4e41d9e004405c96e13
ms.sourcegitcommit: c0b72b344dadea835b0e7943c52463f13ab98dd1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2019
ms.locfileid: "74880341"
---
# <a name="create-a-web-api-with-aspnet-core-and-mongodb"></a>MongoDB ile ASP.NET Core ile web API'si oluşturma

Tarafından [Pratik Khandelwal](https://twitter.com/K2Prk) ve [Scott Addie](https://twitter.com/Scott_Addie)

::: moniker range=">= aspnetcore-3.0"

Bu öğreticide web API'si temel oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemleri gerçekleştiren oluşturur bir [MongoDB](https://www.mongodb.com/what-is-mongodb) NoSQL veritabanı.

Bu öğreticide şunların nasıl yapıladığını öğreneceksiniz:

> [!div class="checklist"]
> * MongoDB yapılandırın
> * MongoDB veritabanı oluşturma
> * MongoDB koleksiyonu ve şema tanımlayın
> * Bir web API'sini MongoDB CRUD işlemleri gerçekleştirme
> * JSON serileştirmesini özelleştirme

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-mongo-app/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Prerequisites

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* [.NET Core SDK 3.0 veya üzeri](https://www.microsoft.com/net/download/all)
* **ASP.net ve Web geliştirme** iş yüküyle [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019)
* [MongoDB](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* [.NET Core SDK 3.0 veya üzeri](https://www.microsoft.com/net/download/all)
* [Visual Studio Code](https://code.visualstudio.com/download)
* [Visual Studio Code için C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [MongoDB](https://docs.mongodb.com/manual/administration/install-community/)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

* [.NET Core SDK 3.0 veya üzeri](https://www.microsoft.com/net/download/all)
* [Mac 7,7 veya sonraki bir sürümü için Visual Studio](https://visualstudio.microsoft.com/downloads/)
* [MongoDB](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-os-x/)

---

## <a name="configure-mongodb"></a>MongoDB yapılandırın

Windows kullanıyorsanız MongoDB, varsayılan olarak *mongodb\\C:\\Program Files* 'a yüklenir. *\\Program Files\\MongoDB\\Server\\* \<Version_Number >\\`Path`) ortam değişkenine ekleyin. Bu değişiklik yerden MongoDB erişim sağlar, geliştirme makinenizde.

Mongo kabuğunu veritabanı oluşturma, koleksiyonları yapın ve belgeleri depolamak için aşağıdaki adımları kullanın. Mongo Kabuğu komutları hakkında daha fazla bilgi için bkz. [mongo kabuğunu çalışma](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell).

1. Geliştirme makinenizde verilerin depolanması için bir dizin seçin. Örneğin, *C: Windows üzerinde\\BooksData* . Yoksa dizini oluşturun. Mongo kabuğunu yeni dizinleri oluşturmaz.
1. Bir komut kabuğunu açın. Varsayılan bağlantı noktası 27017 mongodb'ye bağlanmak için aşağıdaki komutu çalıştırın. Değiştirmeyi unutmayın `<data_directory_path>` önceki adımda seçtiğiniz dizini.

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

   Adlı bir veritabanı zaten mevcut olmayan halinde *BookstoreDb* oluşturulur. Veritabanı mevcut değilse, bağlantı işlemleri için açılır.

1. Oluşturma bir `Books` koleksiyon aşağıdaki komutu kullanarak:

   ```console
   db.createCollection('Books')
   ```

   Aşağıdaki sonucu görüntülenir:

   ```console
   { "ok" : 1 }
   ```

1. İçin bir şema tanımlayabilir `Books` toplama ve ekleme iki belge aşağıdaki komutu kullanarak:

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

   Şema girmiş ekler `_id` türünün özelliği `ObjectId` her belge için.

Veritabanı hazırdır. ASP.NET Core web API'si oluşturmaya başlayabilirsiniz.

## <a name="create-the-aspnet-core-web-api-project"></a>ASP.NET Core web API projesi oluşturma

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. **Dosya** > **Yeni** > **projesi**' ne gidin.
1. **ASP.NET Core Web uygulaması** proje türünü seçin ve **İleri**' yi seçin.
1. Projeyi *Booksapı*olarak adlandırın ve **Oluştur**' u seçin.
1. **.NET Core** hedef çerçevesini ve **3,0 ASP.NET Core**seçin. **API** proje şablonunu seçin ve **Oluştur**' u seçin.
1. MongoDB için .NET sürücüsünün en son kararlı sürümünü öğrenmek üzere [NuGet galerisini ziyaret edin: MongoDB. Driver](https://www.nuget.org/packages/MongoDB.Driver/) . İçinde **Paket Yöneticisi Konsolu** penceresinde proje kök dizinine gidin. MongoDB için .NET sürücüsünü yüklemek için aşağıdaki komutu çalıştırın:

   ```powershell
   Install-Package MongoDB.Driver -Version {VERSION}
   ```

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

1. Bir komut kabuğu'nda aşağıdaki komutları çalıştırın:

   ```dotnetcli
   dotnet new webapi -o BooksApi
   code BooksApi
   ```

   .NET Core'u hedefleyen yeni bir ASP.NET Core web API projesi oluşturulur ve Visual Studio Code'da açılır.

1. Durum çubuğunun omnisharp Yangın simgesi yeşil ' i etkinleştirdikten sonra, **gerekli varlıkların derleme ve hata ayıklama için ' booksapı ' içinde eksik bir iletişim kutusu yok. Bunları ekleyin mi?** . **Evet**’i seçin.
1. MongoDB için .NET sürücüsünün en son kararlı sürümünü öğrenmek üzere [NuGet galerisini ziyaret edin: MongoDB. Driver](https://www.nuget.org/packages/MongoDB.Driver/) . Açık **tümleşik Terminalini** ve proje kök dizinine gidin. MongoDB için .NET sürücüsünü yüklemek için aşağıdaki komutu çalıştırın:

   ```dotnetcli
   dotnet add BooksApi.csproj package MongoDB.Driver -v {VERSION}
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

1. > **yeni çözüm** > **.NET Core** > **App** **dosyasına** gidin.
1. **ASP.NET Core Web API** C# proje şablonunu seçin ve **İleri**' yi seçin.
1. **Hedef çerçeve** açılır listesinden **.NET Core 3,0** ' i seçin ve **İleri**' yi seçin.
1. **Proje adı**Için *booksapı* girin ve **Oluştur**' u seçin.
1. İçinde **çözüm** paneli, projenin sağ **bağımlılıkları** düğümünü seçip alt **paketleri Ekle**.
1. Arama kutusuna *MongoDB. Driver* girin, *MongoDB. Driver* paketini seçin ve **paket Ekle**' yi seçin.
1. **Lisans kabulü** Iletişim kutusunda **kabul et** düğmesini seçin.

---

## <a name="add-an-entity-model"></a>Varlık modeli ekleme

1. Ekleme bir *modelleri* proje kök dizini.
1. Ekleme bir `Book` sınıfının *modelleri* aşağıdaki kod ile dizin:

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

   [!code-json[](first-mongo-app/samples/3.x/SampleApp/appsettings.json?highlight=2-6)]

1. *Modeller* dizinine aşağıdaki kodla bir *BookstoreDatabaseSettings.cs* dosyası ekleyin:

   [!code-csharp[](first-mongo-app/samples/3.x/SampleApp/Models/BookstoreDatabaseSettings.cs)]

   Yukarıdaki `BookstoreDatabaseSettings` sınıfı, *appSettings. JSON* dosyasının `BookstoreDatabaseSettings` özellik değerlerini depolamak için kullanılır. JSON ve C# Özellik adları, eşleme sürecini kolaylaştırmak için aynı şekilde adlandırılır.

1. Aşağıdaki Vurgulanan kodu `Startup.ConfigureServices`ekleyin:

   [!code-csharp[](first-mongo-app/samples_snapshot/3.x/SampleApp/Startup.ConfigureServices.AddDbSettings.cs?highlight=3-7)]

   Yukarıdaki kodda:

   * *AppSettings. JSON* dosyasının `BookstoreDatabaseSettings` bölüm bağlandığı yapılandırma örneği, bağımlılık ekleme (dı) kapsayıcısına kaydedilir. Örneğin, bir `BookstoreDatabaseSettings` nesnesinin `ConnectionString` özelliği *appSettings. JSON*içindeki `BookstoreDatabaseSettings:ConnectionString` özelliği ile doldurulur.
   * `IBookstoreDatabaseSettings` arabirimi, tek bir [hizmet ömrü](xref:fundamentals/dependency-injection#service-lifetimes)ile dı 'ye kaydedilir. Eklerken, arabirim örneği bir `BookstoreDatabaseSettings` nesnesine çözümlenir.

1. `BookstoreDatabaseSettings` ve `IBookstoreDatabaseSettings` başvurularını çözümlemek için aşağıdaki kodu en *üst kısmına ekleyin* :

   [!code-csharp[](first-mongo-app/samples/3.x/SampleApp/Startup.cs?name=snippet_UsingBooksApiModels)]

## <a name="add-a-crud-operations-service"></a>CRUD işlemleri hizmeti ekleme

1. Ekleme bir *Hizmetleri* proje kök dizini.
1. Ekleme bir `BookService` sınıfının *Hizmetleri* aşağıdaki kod ile dizin:

   [!code-csharp[](first-mongo-app/samples/3.x/SampleApp/Services/BookService.cs?name=snippet_BookServiceClass)]

   Yukarıdaki kodda, Oluşturucu ekleme yoluyla dı 'den bir `IBookstoreDatabaseSettings` örneği alınır. Bu teknik, [yapılandırma modeli ekleme](#add-a-configuration-model) bölümüne eklenen *appSettings. JSON* yapılandırma değerlerine erişim sağlar.

1. Aşağıdaki Vurgulanan kodu `Startup.ConfigureServices`ekleyin:

   [!code-csharp[](first-mongo-app/samples_snapshot/3.x/SampleApp/Startup.ConfigureServices.AddSingletonService.cs?highlight=9)]

   Önceki kodda, `BookService` sınıfı, tüketen sınıflarda Oluşturucu ekleme işlemini desteklemek için DI ile kaydedilir. `BookService`, `MongoClient`doğrudan bağımlılığı sağladığından, tek hizmet ömrü en uygundur. Resmi [Mongo istemci yeniden kullanım yönergeleri](https://mongodb.github.io/mongo-csharp-driver/2.8/reference/driver/connecting/#re-use)temelinde `MongoClient`, tek bir hizmet ömrü ile birlikte kaydedilmelidir.

1. `BookService` başvurusunu çözümlemek için aşağıdaki kodu *Startup.cs* 'un en üstüne ekleyin:

   [!code-csharp[](first-mongo-app/samples/3.x/SampleApp/Startup.cs?name=snippet_UsingBooksApiServices)]

`BookService` Sınıfını kullanan aşağıdaki `MongoDB.Driver` veritabanında CRUD işlemleri gerçekleştirmek için üyeleri:

* [Mongoclient](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoClient.htm) &ndash;, veritabanı işlemlerini gerçekleştirmek için sunucu örneğini okur. Bu sınıfın oluşturucusu, MongoDB bağlantı dizesini sağlanır:

  [!code-csharp[](first-mongo-app/samples/3.x/SampleApp/Services/BookService.cs?name=snippet_BookServiceConstructor&highlight=3)]

* [Imongodatabase](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_IMongoDatabase.htm) &ndash; işlemleri gerçekleştirmek Için Mongo veritabanını temsil eder. Bu öğretici, belirli bir koleksiyondaki verilere erişim kazanmak için arabirimindeki genel [GetCollection\<TDocument > (koleksiyon)](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoDatabase_GetCollection__1.htm) yöntemini kullanır. Bu yöntem çağrıldıktan sonra, koleksiyonda CRUD işlemleri gerçekleştirin. İçinde `GetCollection<TDocument>(collection)` yöntem çağrısı:

  * `collection` Koleksiyon adını temsil eder.
  * `TDocument` Bir koleksiyonda depolanan CLR nesne türünü temsil eder.

`GetCollection<TDocument>(collection)`, koleksiyonu temsil eden bir [Mongocollection](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoCollection.htm) nesnesi döndürür. Bu öğreticide, aşağıdaki yöntemlerden koleksiyonunda çağrılır:

* [Deleteone](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_DeleteOne.htm) &ndash;, belirtilen arama ölçütleriyle eşleşen tek bir belgeyi siler.
* [\<TDocument > bul](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollectionExtensions_Find__1_1.htm) &ndash;, koleksiyonda belirtilen arama ölçütleriyle eşleşen tüm belgeleri döndürür.
* [Insertone](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_InsertOne.htm) &ndash;, belirtilen nesneyi koleksiyondaki yeni bir belge olarak ekler.
* [Replaceone](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_ReplaceOne.htm) &ndash;, belirtilen arama ölçütleriyle eşleşen tek belgeyi, belirtilen nesneyle değiştirir.

## <a name="add-a-controller"></a>Denetleyici ekleme

Ekleme bir `BooksController` sınıfının *denetleyicileri* aşağıdaki kod ile dizin:

[!code-csharp[](first-mongo-app/samples/3.x/SampleApp/Controllers/BooksController.cs)]

Önceki web API denetleyicisi:

* Kullanan `BookService` CRUD işlemleri gerçekleştirmek için sınıf.
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

   [!code-csharp[](first-mongo-app/samples/3.x/SampleApp/Startup.cs?name=snippet_ConfigureServices&highlight=12)]

   Önceki değişiklik ile, Web API 'sinin seri hale getirilmiş JSON yanıtındaki Özellik adları CLR nesne türündeki ilgili özellik adlarıyla eşleşir. Örneğin, `Book` sınıfının `Author` özelliği `Author`olarak serileştirir.

1. *Modeller/Book. cs*' de, `BookName` özelliğine aşağıdaki [`[JsonProperty]`](https://www.newtonsoft.com/json/help/html/T_Newtonsoft_Json_JsonPropertyAttribute.htm) özniteliğiyle açıklama ekleyin:

   [!code-csharp[](first-mongo-app/samples/3.x/SampleApp/Models/Book.cs?name=snippet_BookNameProperty&highlight=2)]

   `[JsonProperty]` özniteliğinin değeri `Name`, Web API 'sinin serileştirilmiş JSON yanıtında özellik adını temsil eder.

1. `[JsonProperty]` öznitelik başvurusunu çözümlemek için *modeller/Book. cs* ' nin en üstüne aşağıdaki kodu ekleyin:

   [!code-csharp[](first-mongo-app/samples/3.x/SampleApp/Models/Book.cs?name=snippet_NewtonsoftJsonImport)]

1. [Web API 'Sini test](#test-the-web-api) etme bölümünde tanımlanan adımları yineleyin. JSON Özellik adlarındaki farka dikkat edin.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Bu öğreticide web API'si temel oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemleri gerçekleştiren oluşturur bir [MongoDB](https://www.mongodb.com/what-is-mongodb) NoSQL veritabanı.

Bu öğreticide şunların nasıl yapıladığını öğreneceksiniz:

> [!div class="checklist"]
> * MongoDB yapılandırın
> * MongoDB veritabanı oluşturma
> * MongoDB koleksiyonu ve şema tanımlayın
> * Bir web API'sini MongoDB CRUD işlemleri gerçekleştirme
> * JSON serileştirmesini özelleştirme

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-mongo-app/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Prerequisites

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* [.NET Core SDK 2,2](https://www.microsoft.com/net/download/all)
* **ASP.net ve Web geliştirme** iş yüküyle [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019)
* [MongoDB](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* [.NET Core SDK 2,2](https://www.microsoft.com/net/download/all)
* [Visual Studio Code](https://code.visualstudio.com/download)
* [Visual Studio Code için C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [MongoDB](https://docs.mongodb.com/manual/administration/install-community/)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

* [.NET Core SDK 2,2](https://www.microsoft.com/net/download/all)
* [Mac 7,7 veya sonraki bir sürümü için Visual Studio](https://visualstudio.microsoft.com/downloads/)
* [MongoDB](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-os-x/)

---

## <a name="configure-mongodb"></a>MongoDB yapılandırın

Windows kullanıyorsanız MongoDB, varsayılan olarak *mongodb\\C:\\Program Files* 'a yüklenir. *\\Program Files\\MongoDB\\Server\\* \<Version_Number >\\`Path`) ortam değişkenine ekleyin. Bu değişiklik yerden MongoDB erişim sağlar, geliştirme makinenizde.

Mongo kabuğunu veritabanı oluşturma, koleksiyonları yapın ve belgeleri depolamak için aşağıdaki adımları kullanın. Mongo Kabuğu komutları hakkında daha fazla bilgi için bkz. [mongo kabuğunu çalışma](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell).

1. Geliştirme makinenizde verilerin depolanması için bir dizin seçin. Örneğin, *C: Windows üzerinde\\BooksData* . Yoksa dizini oluşturun. Mongo kabuğunu yeni dizinleri oluşturmaz.
1. Bir komut kabuğunu açın. Varsayılan bağlantı noktası 27017 mongodb'ye bağlanmak için aşağıdaki komutu çalıştırın. Değiştirmeyi unutmayın `<data_directory_path>` önceki adımda seçtiğiniz dizini.

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

   Adlı bir veritabanı zaten mevcut olmayan halinde *BookstoreDb* oluşturulur. Veritabanı mevcut değilse, bağlantı işlemleri için açılır.

1. Oluşturma bir `Books` koleksiyon aşağıdaki komutu kullanarak:

   ```console
   db.createCollection('Books')
   ```

   Aşağıdaki sonucu görüntülenir:

   ```console
   { "ok" : 1 }
   ```

1. İçin bir şema tanımlayabilir `Books` toplama ve ekleme iki belge aşağıdaki komutu kullanarak:

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

   Şema girmiş ekler `_id` türünün özelliği `ObjectId` her belge için.

Veritabanı hazırdır. ASP.NET Core web API'si oluşturmaya başlayabilirsiniz.

## <a name="create-the-aspnet-core-web-api-project"></a>ASP.NET Core web API projesi oluşturma

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. **Dosya** > **Yeni** > **projesi**' ne gidin.
1. **ASP.NET Core Web uygulaması** proje türünü seçin ve **İleri**' yi seçin.
1. Projeyi *Booksapı*olarak adlandırın ve **Oluştur**' u seçin.
1. **.NET Core** hedef çerçevesini ve **2,2 ASP.NET Core**seçin. **API** proje şablonunu seçin ve **Oluştur**' u seçin.
1. MongoDB için .NET sürücüsünün en son kararlı sürümünü öğrenmek üzere [NuGet galerisini ziyaret edin: MongoDB. Driver](https://www.nuget.org/packages/MongoDB.Driver/) . İçinde **Paket Yöneticisi Konsolu** penceresinde proje kök dizinine gidin. MongoDB için .NET sürücüsünü yüklemek için aşağıdaki komutu çalıştırın:

   ```powershell
   Install-Package MongoDB.Driver -Version {VERSION}
   ```

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

1. Bir komut kabuğu'nda aşağıdaki komutları çalıştırın:

   ```dotnetcli
   dotnet new webapi -o BooksApi
   code BooksApi
   ```

   .NET Core'u hedefleyen yeni bir ASP.NET Core web API projesi oluşturulur ve Visual Studio Code'da açılır.

1. Durum çubuğunun omnisharp Yangın simgesi yeşil ' i etkinleştirdikten sonra, **gerekli varlıkların derleme ve hata ayıklama için ' booksapı ' içinde eksik bir iletişim kutusu yok. Bunları ekleyin mi?** . **Evet**’i seçin.
1. MongoDB için .NET sürücüsünün en son kararlı sürümünü öğrenmek üzere [NuGet galerisini ziyaret edin: MongoDB. Driver](https://www.nuget.org/packages/MongoDB.Driver/) . Açık **tümleşik Terminalini** ve proje kök dizinine gidin. MongoDB için .NET sürücüsünü yüklemek için aşağıdaki komutu çalıştırın:

   ```dotnetcli
   dotnet add BooksApi.csproj package MongoDB.Driver -v {VERSION}
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

1. > **yeni çözüm** > **.NET Core** > **App** **dosyasına** gidin.
1. **ASP.NET Core Web API** C# proje şablonunu seçin ve **İleri**' yi seçin.
1. **Hedef çerçeve** açılır listesinden **.NET Core 2,2** ' i seçin ve **İleri**' yi seçin.
1. **Proje adı**Için *booksapı* girin ve **Oluştur**' u seçin.
1. İçinde **çözüm** paneli, projenin sağ **bağımlılıkları** düğümünü seçip alt **paketleri Ekle**.
1. Arama kutusuna *MongoDB. Driver* girin, *MongoDB. Driver* paketini seçin ve **paket Ekle**' yi seçin.
1. **Lisans kabulü** Iletişim kutusunda **kabul et** düğmesini seçin.

---

## <a name="add-an-entity-model"></a>Varlık modeli ekleme

1. Ekleme bir *modelleri* proje kök dizini.
1. Ekleme bir `Book` sınıfının *modelleri* aşağıdaki kod ile dizin:

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

   [!code-json[](first-mongo-app/samples/2.x/SampleApp/appsettings.json?highlight=2-6)]

1. *Modeller* dizinine aşağıdaki kodla bir *BookstoreDatabaseSettings.cs* dosyası ekleyin:

   [!code-csharp[](first-mongo-app/samples/2.x/SampleApp/Models/BookstoreDatabaseSettings.cs)]

   Yukarıdaki `BookstoreDatabaseSettings` sınıfı, *appSettings. JSON* dosyasının `BookstoreDatabaseSettings` özellik değerlerini depolamak için kullanılır. JSON ve C# Özellik adları, eşleme sürecini kolaylaştırmak için aynı şekilde adlandırılır.

1. Aşağıdaki Vurgulanan kodu `Startup.ConfigureServices`ekleyin:

   [!code-csharp[](first-mongo-app/samples_snapshot/2.x/SampleApp/Startup.ConfigureServices.AddDbSettings.cs?highlight=3-7)]

   Yukarıdaki kodda:

   * *AppSettings. JSON* dosyasının `BookstoreDatabaseSettings` bölüm bağlandığı yapılandırma örneği, bağımlılık ekleme (dı) kapsayıcısına kaydedilir. Örneğin, bir `BookstoreDatabaseSettings` nesnesinin `ConnectionString` özelliği *appSettings. JSON*içindeki `BookstoreDatabaseSettings:ConnectionString` özelliği ile doldurulur.
   * `IBookstoreDatabaseSettings` arabirimi, tek bir [hizmet ömrü](xref:fundamentals/dependency-injection#service-lifetimes)ile dı 'ye kaydedilir. Eklerken, arabirim örneği bir `BookstoreDatabaseSettings` nesnesine çözümlenir.

1. `BookstoreDatabaseSettings` ve `IBookstoreDatabaseSettings` başvurularını çözümlemek için aşağıdaki kodu en *üst kısmına ekleyin* :

   [!code-csharp[](first-mongo-app/samples/2.x/SampleApp/Startup.cs?name=snippet_UsingBooksApiModels)]

## <a name="add-a-crud-operations-service"></a>CRUD işlemleri hizmeti ekleme

1. Ekleme bir *Hizmetleri* proje kök dizini.
1. Ekleme bir `BookService` sınıfının *Hizmetleri* aşağıdaki kod ile dizin:

   [!code-csharp[](first-mongo-app/samples/2.x/SampleApp/Services/BookService.cs?name=snippet_BookServiceClass)]

   Yukarıdaki kodda, Oluşturucu ekleme yoluyla dı 'den bir `IBookstoreDatabaseSettings` örneği alınır. Bu teknik, [yapılandırma modeli ekleme](#add-a-configuration-model) bölümüne eklenen *appSettings. JSON* yapılandırma değerlerine erişim sağlar.

1. Aşağıdaki Vurgulanan kodu `Startup.ConfigureServices`ekleyin:

   [!code-csharp[](first-mongo-app/samples_snapshot/2.x/SampleApp/Startup.ConfigureServices.AddSingletonService.cs?highlight=9)]

   Önceki kodda, `BookService` sınıfı, tüketen sınıflarda Oluşturucu ekleme işlemini desteklemek için DI ile kaydedilir. `BookService`, `MongoClient`doğrudan bağımlılığı sağladığından, tek hizmet ömrü en uygundur. Resmi [Mongo istemci yeniden kullanım yönergeleri](https://mongodb.github.io/mongo-csharp-driver/2.8/reference/driver/connecting/#re-use)temelinde `MongoClient`, tek bir hizmet ömrü ile birlikte kaydedilmelidir.

1. `BookService` başvurusunu çözümlemek için aşağıdaki kodu *Startup.cs* 'un en üstüne ekleyin:

   [!code-csharp[](first-mongo-app/samples/2.x/SampleApp/Startup.cs?name=snippet_UsingBooksApiServices)]

`BookService` Sınıfını kullanan aşağıdaki `MongoDB.Driver` veritabanında CRUD işlemleri gerçekleştirmek için üyeleri:

* [Mongoclient](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoClient.htm) &ndash;, veritabanı işlemlerini gerçekleştirmek için sunucu örneğini okur. Bu sınıfın oluşturucusu, MongoDB bağlantı dizesini sağlanır:

  [!code-csharp[](first-mongo-app/samples/2.x/SampleApp/Services/BookService.cs?name=snippet_BookServiceConstructor&highlight=3)]

* [Imongodatabase](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_IMongoDatabase.htm) &ndash; işlemleri gerçekleştirmek Için Mongo veritabanını temsil eder. Bu öğretici, belirli bir koleksiyondaki verilere erişim kazanmak için arabirimindeki genel [GetCollection\<TDocument > (koleksiyon)](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoDatabase_GetCollection__1.htm) yöntemini kullanır. Bu yöntem çağrıldıktan sonra, koleksiyonda CRUD işlemleri gerçekleştirin. İçinde `GetCollection<TDocument>(collection)` yöntem çağrısı:

  * `collection` Koleksiyon adını temsil eder.
  * `TDocument` Bir koleksiyonda depolanan CLR nesne türünü temsil eder.

`GetCollection<TDocument>(collection)`, koleksiyonu temsil eden bir [Mongocollection](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoCollection.htm) nesnesi döndürür. Bu öğreticide, aşağıdaki yöntemlerden koleksiyonunda çağrılır:

* [Deleteone](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_DeleteOne.htm) &ndash;, belirtilen arama ölçütleriyle eşleşen tek bir belgeyi siler.
* [\<TDocument > bul](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollectionExtensions_Find__1_1.htm) &ndash;, koleksiyonda belirtilen arama ölçütleriyle eşleşen tüm belgeleri döndürür.
* [Insertone](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_InsertOne.htm) &ndash;, belirtilen nesneyi koleksiyondaki yeni bir belge olarak ekler.
* [Replaceone](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_ReplaceOne.htm) &ndash;, belirtilen arama ölçütleriyle eşleşen tek belgeyi, belirtilen nesneyle değiştirir.

## <a name="add-a-controller"></a>Denetleyici ekleme

Ekleme bir `BooksController` sınıfının *denetleyicileri* aşağıdaki kod ile dizin:

[!code-csharp[](first-mongo-app/samples/2.x/SampleApp/Controllers/BooksController.cs)]

Önceki web API denetleyicisi:

* Kullanan `BookService` CRUD işlemleri gerçekleştirmek için sınıf.
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

1. `Startup.ConfigureServices`' de, aşağıdaki vurgulanmış kodu `AddMvc` yöntemi çağrısına zincirle:

   [!code-csharp[](first-mongo-app/samples/2.x/SampleApp/Startup.cs?name=snippet_ConfigureServices&highlight=12)]

   Önceki değişiklik ile, Web API 'sinin seri hale getirilmiş JSON yanıtındaki Özellik adları CLR nesne türündeki ilgili özellik adlarıyla eşleşir. Örneğin, `Book` sınıfının `Author` özelliği `Author`olarak serileştirir.

1. *Modeller/Book. cs*' de, `BookName` özelliğine aşağıdaki [`[JsonProperty]`](https://www.newtonsoft.com/json/help/html/T_Newtonsoft_Json_JsonPropertyAttribute.htm) özniteliğiyle açıklama ekleyin:

   [!code-csharp[](first-mongo-app/samples/2.x/SampleApp/Models/Book.cs?name=snippet_BookNameProperty&highlight=2)]

   `[JsonProperty]` özniteliğinin değeri `Name`, Web API 'sinin serileştirilmiş JSON yanıtında özellik adını temsil eder.

1. `[JsonProperty]` öznitelik başvurusunu çözümlemek için *modeller/Book. cs* ' nin en üstüne aşağıdaki kodu ekleyin:

   [!code-csharp[](first-mongo-app/samples/2.x/SampleApp/Models/Book.cs?name=snippet_NewtonsoftJsonImport)]

1. [Web API 'Sini test](#test-the-web-api) etme bölümünde tanımlanan adımları yineleyin. JSON Özellik adlarındaki farka dikkat edin.

::: moniker-end

## <a name="add-authentication-support-to-a-web-api"></a>Web API 'sine kimlik doğrulama desteği ekleme

[!INCLUDE[](~/includes/IdentityServer4.md)]

## <a name="next-steps"></a>Sonraki adımlar

ASP.NET Core web API'leri oluşturmaya daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Bu makalenin YouTube sürümü](https://www.youtube.com/watch?v=7uJt_sOenyo&feature=youtu.be)
* <xref:web-api/index>
* <xref:web-api/action-return-types>
