---
title: MongoDB ile ASP.NET Core ile web API'si oluşturma
author: prkhandelwal
description: Bu öğreticide bir ASP.NET Core web API'sini kullanarak bir MongoDB NoSQL veritabanı oluşturma işlemini gösterir.
ms.author: scaddie
ms.custom: mvc, seodec18
ms.date: 06/10/2019
uid: tutorials/first-mongo-app
ms.openlocfilehash: 426b4c0dee290153b9b1bf83deec14fa728183cb
ms.sourcegitcommit: f5762967df3be8b8c868229e679301f2f7954679
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67048086"
---
# <a name="create-a-web-api-with-aspnet-core-and-mongodb"></a>MongoDB ile ASP.NET Core ile web API'si oluşturma

Tarafından [Pratik Khandelwal](https://twitter.com/K2Prk) ve [Scott Addie](https://twitter.com/Scott_Addie)

Bu öğreticide web API'si temel oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemleri gerçekleştiren oluşturur bir [MongoDB](https://www.mongodb.com/what-is-mongodb) NoSQL veritabanı.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * MongoDB yapılandırın
> * MongoDB veritabanı oluşturma
> * MongoDB koleksiyonu ve şema tanımlayın
> * Bir web API'sini MongoDB CRUD işlemleri gerçekleştirme
> * JSON seri hale getirme özelleştirmek

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-mongo-app/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Önkoşullar

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* [.NET core SDK 2.2 veya üzeri](https://www.microsoft.com/net/download/all)
* [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) ile **ASP.NET ve web geliştirme** iş yükü
* [MongoDB](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* [.NET core SDK 2.2 veya üzeri](https://www.microsoft.com/net/download/all)
* [Visual Studio Code](https://code.visualstudio.com/download)
* [Visual Studio Code için C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [MongoDB](https://docs.mongodb.com/manual/administration/install-community/)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

* [.NET core SDK 2.2 veya üzeri](https://www.microsoft.com/net/download/all)
* [Mac 7,7 veya sonraki bir sürümü için Visual Studio](https://visualstudio.microsoft.com/downloads/)
* [MongoDB](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-os-x/)

---

## <a name="configure-mongodb"></a>MongoDB yapılandırın

Windows kullanıyorsanız, MongoDB yüklü *C:\\Program dosyaları\\MongoDB* varsayılan olarak. Ekleme *C:\\Program dosyaları\\MongoDB\\sunucu\\\<version_number >\\bin* için `Path` ortam değişkeni. Bu değişiklik yerden MongoDB erişim sağlar, geliştirme makinenizde.

Mongo kabuğunu veritabanı oluşturma, koleksiyonları yapın ve belgeleri depolamak için aşağıdaki adımları kullanın. Mongo Kabuğu komutları hakkında daha fazla bilgi için bkz. [mongo kabuğunu çalışma](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell).

1. Geliştirme makinenizde verilerin depolanması için bir dizin seçin. Örneğin, *C:\\BooksData* Windows üzerinde. Yoksa dizini oluşturun. Mongo kabuğunu yeni dizinleri oluşturmaz.
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

1. Git **dosya** > **yeni** > **proje**.
1. Seçin **ASP.NET Core Web uygulaması** proje türü ve seçin **sonraki**.
1. Projeyi adlandırın *BooksApi*seçip **Oluştur**.
1. Seçin **.NET Core** hedef çerçeve ve **ASP.NET Core 2.2**. Seçin **API** proje şablonu, belirleyin **Oluştur**.
1. Ziyaret [NuGet Galerisi: Mongodb](https://www.nuget.org/packages/MongoDB.Driver/) için MongoDB .NET sürücüsü en son kararlı sürümünü belirlemek için. İçinde **Paket Yöneticisi Konsolu** penceresinde proje kök dizinine gidin. MongoDB için .NET sürücüsünü yüklemek için aşağıdaki komutu çalıştırın:

    ```powershell
    Install-Package MongoDB.Driver -Version {VERSION}
    ```

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

1. Bir komut kabuğu'nda aşağıdaki komutları çalıştırın:

    ```console
    dotnet new webapi -o BooksApi
    code BooksApi
    ```

    .NET Core'u hedefleyen yeni bir ASP.NET Core web API projesi oluşturulur ve Visual Studio Code'da açılır.

1. Durum çubuğunun OmniSharp sonra soran bir iletişim kutusu yangın simgesi yeşile **gerekli varlıkları oluşturun ve hata ayıklama 'BooksApi' eksik. Bunları eklensin mi?** . Seçin **Evet**.
1. Ziyaret [NuGet Galerisi: Mongodb](https://www.nuget.org/packages/MongoDB.Driver/) için MongoDB .NET sürücüsü en son kararlı sürümünü belirlemek için. Açık **tümleşik Terminalini** ve proje kök dizinine gidin. MongoDB için .NET sürücüsünü yüklemek için aşağıdaki komutu çalıştırın:

    ```console
    dotnet add BooksApi.csproj package MongoDB.Driver -v {VERSION}
    ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

1. Git **dosya** > **yeni çözüm** >  **.NET Core** > **uygulama**.
1. Seçin **ASP.NET Core Web API'si** C# proje şablonu, belirleyin **sonraki**.
1. Seçin **.NET Core 2.2** gelen **hedef Framework'ü** seçin ve açılır listede **sonraki**.
1. Girin *BooksApi* için **proje adı**seçip **Oluştur**.
1. İçinde **çözüm** paneli, projenin sağ **bağımlılıkları** düğümünü seçip alt **paketleri Ekle**.
1. Girin *MongoDB.Driver* arama kutusunda *MongoDB.Driver* paketini bulun ve seçin **Paketi Ekle**.
1. Seçin **kabul** düğmesine **lisans kabulü** iletişim.

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

    Önceki sınıfında `Id` özelliği:
    
    * Ortak dil çalışma zamanı (CLR) nesnesi için MongoDB koleksiyonu eşlemek için gereklidir.
    * İle açıklanıyor [[BsonId]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonIdAttribute.htm) belgenin birincil anahtarı olarak bu özellik belirlemek için.
    * İle açıklanıyor [[BsonRepresentation(BsonType.ObjectId)]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonRepresentationAttribute.htm) parametre türü olarak geçirerek izin vermek için `string` yerine bir [objectID](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_ObjectId.htm) yapısı. Mongo işleme dönüştürme `string` için `ObjectId`.
    
    `BookName` Özelliği ile ek açıklamalı [[BsonElement]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonElementAttribute.htm) özniteliği. Özniteliğin değerini `Name` özellik adında MongoDB koleksiyonu temsil eder.

## <a name="add-a-configuration-model"></a>Yapılandırma modeli ekleme

1. Aşağıdaki veritabanı yapılandırma değerlerini eklemek *appsettings.json*:

    [!code-json[](first-mongo-app/sample/BooksApi/appsettings.json?highlight=2-6)]

1. Ekleme bir *BookstoreDatabaseSettings.cs* dosyasını *modelleri* aşağıdaki kod ile dizin:

    [!code-csharp[](first-mongo-app/sample/BooksApi/Models/BookstoreDatabaseSettings.cs)]

    Önceki `BookstoreDatabaseSettings` depolamak için kullanılan sınıf *appsettings.json* dosyanın `BookstoreDatabaseSettings` özellik değerleri. JSON ve C# özellik adları adlı aynı şekilde eşleme işlemini kolaylaştırmak için.

1. Aşağıdaki vurgulanmış kodu ekleyin `Startup.ConfigureServices`:

    [!code-csharp[](first-mongo-app/sample_snapshot/BooksApi/Startup.ConfigureServices.AddDbSettings.cs?highlight=3-7)]

    Yukarıdaki kodda:

    * Yapılandırma örneğine *appsettings.json* dosyanın `BookstoreDatabaseSettings` bölümüne bağlar, bağımlılık ekleme (dı) kapsayıcısında kaydedilir. Örneğin, bir `BookstoreDatabaseSettings` nesnenin `ConnectionString` özelliği ile doldurulur `BookstoreDatabaseSettings:ConnectionString` özelliğinde *appsettings.json*.
    * `IBookstoreDatabaseSettings` Arabirimi ile tek DI kayıtlı [hizmet ömrü](xref:fundamentals/dependency-injection#service-lifetimes). Arabirim örneğinin eklenen, çözümler bir `BookstoreDatabaseSettings` nesne.

1. Üstüne aşağıdaki kodu ekleyin *Startup.cs* çözümlenecek `BookstoreDatabaseSettings` ve `IBookstoreDatabaseSettings` başvuruları:

    [!code-csharp[](first-mongo-app/sample/BooksApi/Startup.cs?name=snippet_UsingBooksApiModels)]

## <a name="add-a-crud-operations-service"></a>CRUD işlemleri hizmet ekleme

1. Ekleme bir *Hizmetleri* proje kök dizini.
1. Ekleme bir `BookService` sınıfının *Hizmetleri* aşağıdaki kod ile dizin:

    [!code-csharp[](first-mongo-app/sample/BooksApi/Services/BookService.cs?name=snippet_BookServiceClass)]

    Önceki kodda, bir `IBookstoreDatabaseSettings` Oluşturucu ekleme örneği DI alınır. Bu tekniği erişim sağlayan *appsettings.json* eklenmiştir yapılandırma değerlerini [yapılandırma modeli ekleme](#add-a-configuration-model) bölümü.

1. Aşağıdaki vurgulanmış kodu ekleyin `Startup.ConfigureServices`:

    [!code-csharp[](first-mongo-app/sample_snapshot/BooksApi/Startup.ConfigureServices.AddSingletonService.cs?highlight=9)]

    Önceki kodda, `BookService` sınıfı Oluşturucu ekleme sınıfları tüketen desteklemek için DI ile kaydedilir. Singleton hizmet ömrü uygundur çünkü `BookService` doğrudan bağımlılık vereceğine `MongoClient`. Resmi başına [Mongo istemci yeniden yönergeleri](https://mongodb.github.io/mongo-csharp-driver/2.8/reference/driver/connecting/#re-use), `MongoClient` DI tekil hizmet ömrü ile kayıtlı olması gerekir.

1. Üstüne aşağıdaki kodu ekleyin *Startup.cs* çözümlenecek `BookService` başvurusu:

    [!code-csharp[](first-mongo-app/sample/BooksApi/Startup.cs?name=snippet_UsingBooksApiServices)]

`BookService` Sınıfını kullanan aşağıdaki `MongoDB.Driver` veritabanında CRUD işlemleri gerçekleştirmek için üyeleri:

* [MongoClient](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoClient.htm) &ndash; veritabanı işlemleri gerçekleştirmek için kullanılan bir sunucuyu okur. Bu sınıfın oluşturucusu, MongoDB bağlantı dizesini sağlanır:

    [!code-csharp[](first-mongo-app/sample/BooksApi/Services/BookService.cs?name=snippet_BookServiceConstructor&highlight=3)]

* [IMongoDatabase](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_IMongoDatabase.htm) &ndash; işlemleri gerçekleştirmek için kullanılan Mongo veritabanını temsil eder. Bu öğreticide genel [belirtilmiş\<TDocument > (koleksiyon)](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoDatabase_GetCollection__1.htm) yöntemi belirli bir koleksiyondaki verileri erişim elde etmek için arabirim. Bu yöntemi çağrıldıktan sonra koleksiyonu karşı bir CRUD işlemleri gerçekleştirin. İçinde `GetCollection<TDocument>(collection)` yöntem çağrısı:
  * `collection` Koleksiyon adını temsil eder.
  * `TDocument` Bir koleksiyonda depolanan CLR nesne türünü temsil eder.

`GetCollection<TDocument>(collection)` döndürür bir [MongoCollection](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoCollection.htm) koleksiyonu temsil eden nesne. Bu öğreticide, aşağıdaki yöntemlerden koleksiyonunda çağrılır:

* [DeleteOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_DeleteOne.htm) &ndash; sağlanan arama ölçütleriyle eşleşen tek bir belge siler.
* [Bulma\<TDocument >](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollectionExtensions_Find__1_1.htm) &ndash; sağlanan arama ölçütleriyle eşleşen koleksiyondaki tüm belgeleri döndürür.
* [InsertOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_InsertOne.htm) &ndash; belirtilen nesne koleksiyonunda yeni bir belge olarak ekler.
* [ReplaceOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_ReplaceOne.htm) &ndash; sağlanan nesne ile sağlanan arama ölçütleriyle eşleşen tek bir belge değiştirir.

## <a name="add-a-controller"></a>Denetleyici ekleme

Ekleme bir `BooksController` sınıfının *denetleyicileri* aşağıdaki kod ile dizin:

[!code-csharp[](first-mongo-app/sample/BooksApi/Controllers/BooksController.cs)]

Önceki web API denetleyicisi:

* Kullanan `BookService` CRUD işlemleri gerçekleştirmek için sınıf.
* GET, POST, PUT ve DELETE HTTP isteklerini desteklemek için eylem yöntemleri içerir.
* Çağrıları <xref:System.Web.Http.ApiController.CreatedAtRoute*> içinde `Create` döndürülecek eylem yöntemi bir [HTTP 201](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) yanıt. Durum kodu 201 sunucuda yeni bir kaynak oluşturan bir HTTP POST yöntemi için standart yanıttır. `CreatedAtRoute` Ayrıca ekler bir `Location` yanıt üst bilgisi. `Location` Üst bilgisi, yeni oluşturulan kitap URI'sini belirtir.

## <a name="test-the-web-api"></a>Web API'sini test etme

1. Uygulamayı derleyin ve çalıştırın.

1. Gidin `http://localhost:<port>/api/books` test denetleyicisi için parametresiz `Get` eylem yöntemi. Aşağıdaki JSON yanıtı gösterilir:

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

1. Gidin `http://localhost:<port>/api/books/5bfd996f7b8e48dc15ff215e` test denetleyicisi için aşırı yüklenmiş `Get` eylem yöntemi. Aşağıdaki JSON yanıtı gösterilir:

    ```json
    {
      "id":"5bfd996f7b8e48dc15ff215e",
      "bookName":"Clean Code",
      "price":43.15,
      "category":"Computers",
      "author":"Robert C. Martin"
    }
    ```

## <a name="configure-json-serialization-options"></a>JSON seri hale getirme seçenekleri yapılandırın

Döndürülen JSON yanıtları değiştirmek için iki ayrıntı [web API'si Test](#test-the-web-api) bölümü:

* Özellik adlarını ortası büyük olan varsayılan büyük/küçük harf Pascal eşleşecek şekilde değiştirilmesi gereken CLR nesnenin özellik adları büyük/küçük harfleri.
* `bookName` Özelliği olarak döndürülmesi `Name`.

Yukarıdaki gereksinimleri karşılamak için aşağıdaki değişiklikleri yapın:

1. İçinde `Startup.ConfigureServices`, aşağıdaki vurgulanmış kodu için zincir `AddMvc` yöntem çağrısı:

    [!code-csharp[](first-mongo-app/sample/BooksApi/Startup.cs?name=snippet_ConfigureServices&highlight=12)]

    Önceki değişiklikle, web API'SİNİN özellik adlarını JSON yanıtı eşleşme karşılık gelen özellik adlarını CLR nesne türü seri hale. Örneğin, `Book` sınıfın `Author` serileştiren özelliğini olarak `Author`.

1. İçinde *Models/Book.cs*, açıklama `BookName` aşağıdaki özellik [[Item]](https://www.newtonsoft.com/json/help/html/T_Newtonsoft_Json_JsonPropertyAttribute.htm) özniteliği:

    [!code-csharp[](first-mongo-app/sample/BooksApi/Models/Book.cs?name=snippet_BookNameProperty&highlight=2)]

    `[JsonProperty]` Özniteliğin değerini `Name` seri hale getirilmiş JSON yanıtı web API'SİNİN name özelliği temsil eder.

1. Üstüne aşağıdaki kodu ekleyin *Models/Book.cs* çözümlenecek `[JsonProperty]` öznitelik başvurusu:

    [!code-csharp[](first-mongo-app/sample/BooksApi/Models/Book.cs?name=snippet_NewtonsoftJsonImport)]

1. Tanımlı adımları yineleyin [web API'si Test](#test-the-web-api) bölümü. JSON özellik adları fark dikkat edin.

## <a name="next-steps"></a>Sonraki adımlar

ASP.NET Core web API'leri oluşturmaya daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Bu makalenin YouTube sürümü](https://www.youtube.com/watch?v=7uJt_sOenyo&feature=youtu.be)
* <xref:web-api/index>
* <xref:web-api/action-return-types>
