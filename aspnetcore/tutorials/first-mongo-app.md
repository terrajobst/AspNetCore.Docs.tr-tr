---
title: MongoDB ile ASP.NET Core ile Web API oluşturma
author: pratik-khandelwal
description: Bu öğreticide bir ASP.NET Core web API'si kullanarak bir MongoDB NoSQL veritabanı oluşturma gösterilmektedir.
ms.author: scaddie
ms.custom: mvc
ms.date: 11/26/2018
uid: tutorials/first-mongo-app
ms.openlocfilehash: c4e00eeb2c4aecde9c70c6902e21d06853be7696
ms.sourcegitcommit: e7fafb153b9de7595c2558a0133f8d1c33a3bddb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2018
ms.locfileid: "52458499"
---
# <a name="create-a-web-api-with-aspnet-core-and-mongodb"></a>MongoDB ile ASP.NET Core ile web API'si oluşturma

Tarafından [Pratik Khandelwal](https://twitter.com/K2Prk) ve [Scott Addie](https://twitter.com/Scott_Addie)

Bu öğreticide web API'si temel oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemleri gerçekleştiren oluşturur bir [MongoDB](https://www.mongodb.com/what-is-mongodb) NoSQL veritabanı.

Bu öğreticide, şunların nasıl yapılır:

> [!div class="checklist"]
> * MongoDB yapılandırın
> * MongoDB veritabanı oluşturma
> * MongoDB koleksiyonu ve şema tanımlayın
> * Bir web API'sini MongoDB CRUD işlemleri gerçekleştirme

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-mongo-app/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Önkoşullar

* [.NET core SDK 2.1 veya üzeri](https://www.microsoft.com/net/download/all)
* [MongoDB](https://docs.mongodb.com/manual/administration/install-community/)
* [Visual Studio 2017](https://www.visualstudio.com/downloads/) 15.7.3 sürümü veya üzeri aşağıdaki iş yükleri ile:
  * **.NET core platformlar arası geliştirme**
  * **ASP.NET ve web geliştirme**

## <a name="configure-mongodb"></a>MongoDB yapılandırın

Windows kullanıyorsanız, MongoDB yüklü *C:\Program Files\MongoDB* varsayılan olarak. Ekleme *C:\Program Files\MongoDB\Server\<version_number > \bin* için `Path` ortam değişkeni. Bu değişiklik yerden MongoDB erişim sağlar, geliştirme makinenizde.

Mongo kabuğunu veritabanı oluşturma, koleksiyonları yapın ve belgeleri depolamak için aşağıdaki adımları kullanın. Mongo Kabuğu komutları hakkında daha fazla bilgi için bkz. [mongo kabuğunu çalışma](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell).

1. Geliştirme makinenizde verilerin depolanması için bir dizin seçin. Örneğin, *C:\BooksData* Windows üzerinde. Yoksa dizini oluşturun. Mongo kabuğunu yeni dizinleri oluşturmaz.
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

1. Visual Studio'da Git **dosya** > **yeni** > **proje**.
1. Seçin **ASP.NET Core Web uygulaması**, projeyi adlandırın *BookMongo*, tıklatıp **Tamam**.
1. Seçin **.NET Core** hedef çerçeve ve **ASP.NET Core 2.1**. Seçin **API** proje şablonu ve tıklayın **Tamam**:
1. İçinde **Paket Yöneticisi Konsolu** penceresinde proje kök dizinine gidin. MongoDB için .NET sürücüsünü yüklemek için aşağıdaki komutu çalıştırın:

    ```powershell
    Install-Package MongoDB.Driver -Version 2.7.2
    ```

## <a name="add-a-model"></a>Model ekleme

1. Ekleme bir *modelleri* proje kök klasör.
1. Ekleme bir `Book` sınıfının *modelleri* aşağıdaki kodla klasörü:

    [!code-csharp[](first-mongo-app/sample/BookstoreAPI/Models/Book.cs)]

Önceki sınıfında `Id` özelliği ortak dil çalışma zamanı (CLR) nesnesi için MongoDB koleksiyonu eşlemek için gereklidir. Sınıftaki diğer özellikler ile donatılmış `[BsonElement]` özniteliği. Özniteliğin değeri, özellik adı, MongoDB koleksiyonu temsil eder.

## <a name="add-a-crud-operations-class"></a>CRUD işlemleri sınıfı Ekle

1. Ekleme bir *Hizmetleri* proje kök klasör.
1. Ekleme bir `BookService` sınıfının *Hizmetleri* aşağıdaki kodla klasörü:

    [!code-csharp[](first-mongo-app/sample/BookstoreAPI/Services/BookService.cs?name=snippet_BookServiceClass)]

1. MongoDB bağlantı dizesi Ekle *appsettings.json*:

    [!code-csharp[](first-mongo-app/sample/BookstoreAPI/appsettings.json?highlight=2-4)]

    Önceki `BookstoreDb` özelliğine erişilirse `BookService` sınıf oluşturucusu.

1. İçinde `Startup.ConfigureServices`, kayıt `BookService` bağımlılık ekleme sistemiyle sınıfı:

    [!code-csharp[](first-mongo-app/sample/BookstoreAPI/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

    Önceki hizmet kaydı sınıfları tüketen yapıcı eklemeyi desteklemek gereklidir.

`BookService` Sınıfını kullanan aşağıdaki `MongoDB.Driver` veritabanında CRUD işlemleri gerçekleştirmek için üyeleri:

* `MongoClient` &ndash; Veritabanı işlemleri gerçekleştirmek için kullanılan bir sunucuyu okur. Bu sınıfın oluşturucusu, MongoDB bağlantı dizesini sağlanır:

    [!code-csharp[](first-mongo-app/sample/BookstoreAPI/Services/BookService.cs?name=snippet_BookServiceConstructor&highlight=3)]

* `IMongoDatabase` &ndash; Mongo veritabanı işlemleri gerçekleştirmek için temsil eder. Bu öğreticide genel `GetCollection<T>(collection)` yöntemi belirli bir koleksiyondaki verileri erişim elde etmek için arabirim. Bu yöntemi çağrıldıktan sonra koleksiyonunda CRUD işlemleri gerçekleştirilebilir. İçinde `GetCollection<T>(collection)` yöntem çağrısı:
  * `collection` Koleksiyon adını temsil eder.
  * `T` Bir koleksiyonda depolanan CLR nesne türünü temsil eder.

`GetCollection<T>(collection)` döndürür bir `MongoCollection` koleksiyonu temsil eden nesne. Bu öğreticide, aşağıdaki yöntemlerden koleksiyonunda çağrılır:

* `Find<T>` &ndash; Sağlanan arama ölçütleriyle eşleşen koleksiyondaki tüm belgeleri döndürür.
* `InsertOne` &ndash; Belirtilen nesne koleksiyonunda yeni bir belge olarak ekler.
* `ReplaceOne` &ndash; Sağlanan nesne ile sağlanan arama ölçütleriyle eşleşen tek bir belge değiştirir.
* `DeleteOne` &ndash; Belirtilen arama ölçütleriyle eşleşen tek bir belge siler.

## <a name="add-a-controller"></a>Denetleyici ekleme

1. Sağ *denetleyicileri* klasöründe **Çözüm Gezgini**. Seçin **ekleme** > **denetleyicisi**.
1. Seçin **API denetleyici - boş** öğe şablonu ve tıklayın **Ekle**.
1. Girin *BooksController* içinde **Denetleyici adı** metin kutusu seçeneğine tıklayıp **Ekle**.
1. Aşağıdaki kodu ekleyin *BooksController.cs*:

    [!code-csharp[](first-mongo-app/sample/BookstoreAPI/Controllers/BooksController.cs)]

    Önceki web API denetleyicisi:

    * Kullanan `BookService` CRUD işlemleri gerçekleştirmek için sınıf.
    * GET, POST, PUT ve DELETE HTTP isteklerini desteklemek için eylem yöntemleri içerir.
1. Derleme ve uygulamayı çalıştırın.
1. Gidin `http://localhost:<port>/api/books` tarayıcınızda. Aşağıdaki JSON yanıtı gösterilir:

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

## <a name="next-steps"></a>Sonraki adımlar

ASP.NET Core web API'leri oluşturmaya daha fazla bilgi için aşağıdaki kaynaklara bakın:

* <xref:web-api/index>
* <xref:web-api/action-return-types>
