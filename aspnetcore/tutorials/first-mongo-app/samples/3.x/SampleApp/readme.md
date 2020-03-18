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
ms.openlocfilehash: 09d73e25667822b8748a00cc76ad6d4f0e5fe290
ms.sourcegitcommit: d64ef143c64ee4fdade8f9ea0b753b16752c5998
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/18/2020
ms.locfileid: "79511411"
---
# <a name="create-a-web-api-with-aspnet-core-and-mongodb"></a><span data-ttu-id="24e90-102">MongoDB ile ASP.NET Core ile web API'si oluşturma</span><span class="sxs-lookup"><span data-stu-id="24e90-102">Create a web API with ASP.NET Core and MongoDB</span></span>

<span data-ttu-id="24e90-103">Bu öğretici, bir [MongoDB](https://www.mongodb.com/what-is-mongodb) NoSQL veritabanında oluşturma, okuma, güncelleştirme ve SILME (CRUD) işlemlerini gerçekleştiren BIR Web API 'si oluşturur.</span><span class="sxs-lookup"><span data-stu-id="24e90-103">This tutorial creates a web API that performs Create, Read, Update, and Delete (CRUD) operations on a [MongoDB](https://www.mongodb.com/what-is-mongodb) NoSQL database.</span></span>

<span data-ttu-id="24e90-104">Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:</span><span class="sxs-lookup"><span data-stu-id="24e90-104">In this tutorial, you learn how to:</span></span>

* <span data-ttu-id="24e90-105">MongoDB yapılandırın</span><span class="sxs-lookup"><span data-stu-id="24e90-105">Configure MongoDB</span></span>
* <span data-ttu-id="24e90-106">MongoDB veritabanı oluşturma</span><span class="sxs-lookup"><span data-stu-id="24e90-106">Create a MongoDB database</span></span>
* <span data-ttu-id="24e90-107">MongoDB koleksiyonu ve şema tanımlayın</span><span class="sxs-lookup"><span data-stu-id="24e90-107">Define a MongoDB collection and schema</span></span>
* <span data-ttu-id="24e90-108">Bir web API'sini MongoDB CRUD işlemleri gerçekleştirme</span><span class="sxs-lookup"><span data-stu-id="24e90-108">Perform MongoDB CRUD operations from a web API</span></span>
* <span data-ttu-id="24e90-109">JSON serileştirmesini özelleştirme</span><span class="sxs-lookup"><span data-stu-id="24e90-109">Customize JSON serialization</span></span>

## <a name="prerequisites"></a><span data-ttu-id="24e90-110">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="24e90-110">Prerequisites</span></span>

* [<span data-ttu-id="24e90-111">.NET Core SDK 3.0 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="24e90-111">.NET Core SDK 3.0 or later</span></span>](https://dotnet.microsoft.com/download/dotnet-core)
* <span data-ttu-id="24e90-112">**ASP.net ve Web geliştirme** iş yüküyle [Visual Studio 2019 Preview](https://visualstudio.microsoft.com/thank-you-downloading-visual-studio/?sku=community&ch=pre&rel=16&utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019preview)</span><span class="sxs-lookup"><span data-stu-id="24e90-112">[Visual Studio 2019 Preview](https://visualstudio.microsoft.com/thank-you-downloading-visual-studio/?sku=community&ch=pre&rel=16&utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019preview) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="24e90-113">MongoDB</span><span class="sxs-lookup"><span data-stu-id="24e90-113">MongoDB</span></span>](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/)

## <a name="configure-mongodb"></a><span data-ttu-id="24e90-114">MongoDB yapılandırın</span><span class="sxs-lookup"><span data-stu-id="24e90-114">Configure MongoDB</span></span>

<span data-ttu-id="24e90-115">Windows kullanıyorsanız MongoDB, varsayılan olarak *mongodb\\C:\\Program Files* 'a yüklenir.</span><span class="sxs-lookup"><span data-stu-id="24e90-115">If using Windows, MongoDB is installed at *C:\\Program Files\\MongoDB* by default.</span></span> <span data-ttu-id="24e90-116">*\\Program Files\\MongoDB\\Server\\* \<Version_Number >\\`Path`) ortam değişkenine ekleyin.</span><span class="sxs-lookup"><span data-stu-id="24e90-116">Add *C:\\Program Files\\MongoDB\\Server\\\<version_number>\\bin* to the `Path` environment variable.</span></span> <span data-ttu-id="24e90-117">Bu değişiklik yerden MongoDB erişim sağlar, geliştirme makinenizde.</span><span class="sxs-lookup"><span data-stu-id="24e90-117">This change enables MongoDB access from anywhere on your development machine.</span></span>

<span data-ttu-id="24e90-118">Mongo kabuğunu veritabanı oluşturma, koleksiyonları yapın ve belgeleri depolamak için aşağıdaki adımları kullanın.</span><span class="sxs-lookup"><span data-stu-id="24e90-118">Use the mongo Shell in the following steps to create a database, make collections, and store documents.</span></span> <span data-ttu-id="24e90-119">Mongo kabuğu komutları hakkında daha fazla bilgi için bkz. [Mongo kabuğu Ile çalışma](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell).</span><span class="sxs-lookup"><span data-stu-id="24e90-119">For more information on mongo Shell commands, see [Working with the mongo Shell](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell).</span></span>

1. <span data-ttu-id="24e90-120">Geliştirme makinenizde verilerin depolanması için bir dizin seçin.</span><span class="sxs-lookup"><span data-stu-id="24e90-120">Choose a directory on your development machine for storing the data.</span></span> <span data-ttu-id="24e90-121">Örneğin, *C: Windows üzerinde\\BooksData* .</span><span class="sxs-lookup"><span data-stu-id="24e90-121">For example, *C:\\BooksData* on Windows.</span></span> <span data-ttu-id="24e90-122">Yoksa dizini oluşturun.</span><span class="sxs-lookup"><span data-stu-id="24e90-122">Create the directory if it doesn't exist.</span></span> <span data-ttu-id="24e90-123">Mongo kabuğunu yeni dizinleri oluşturmaz.</span><span class="sxs-lookup"><span data-stu-id="24e90-123">The mongo Shell doesn't create new directories.</span></span>
1. <span data-ttu-id="24e90-124">Bir komut kabuğunu açın.</span><span class="sxs-lookup"><span data-stu-id="24e90-124">Open a command shell.</span></span> <span data-ttu-id="24e90-125">Varsayılan bağlantı noktası 27017 mongodb'ye bağlanmak için aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="24e90-125">Run the following command to connect to MongoDB on default port 27017.</span></span> <span data-ttu-id="24e90-126">`<data_directory_path>`, önceki adımda seçtiğiniz dizinle değiştirmeyi unutmayın.</span><span class="sxs-lookup"><span data-stu-id="24e90-126">Remember to replace `<data_directory_path>` with the directory you chose in the previous step.</span></span>

    ```console
    mongod --dbpath <data_directory_path>
    ```

1. <span data-ttu-id="24e90-127">Başka bir komut kabuğu örneği açın.</span><span class="sxs-lookup"><span data-stu-id="24e90-127">Open another command shell instance.</span></span> <span data-ttu-id="24e90-128">Aşağıdaki komutu çalıştırarak varsayılan test veritabanı'na bağlanma:</span><span class="sxs-lookup"><span data-stu-id="24e90-128">Connect to the default test database by running the following command:</span></span>

    ```console
    mongo
    ```

1. <span data-ttu-id="24e90-129">Bir komut kabuğu'nda aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="24e90-129">Run the following in a command shell:</span></span>

    ```console
    use BookstoreDb
    ```

    <span data-ttu-id="24e90-130">Zaten mevcut değilse, *BookstoreDb* adlı bir veritabanı oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="24e90-130">If it doesn't already exist, a database named *BookstoreDb* is created.</span></span> <span data-ttu-id="24e90-131">Veritabanı mevcut değilse, bağlantı işlemleri için açılır.</span><span class="sxs-lookup"><span data-stu-id="24e90-131">If the database does exist, its connection is opened for transactions.</span></span>

1. <span data-ttu-id="24e90-132">Aşağıdaki komutu kullanarak bir `Books` koleksiyonu oluşturun:</span><span class="sxs-lookup"><span data-stu-id="24e90-132">Create a `Books` collection using following command:</span></span>

    ```console
    db.createCollection('Books')
    ```

    <span data-ttu-id="24e90-133">Aşağıdaki sonucu görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="24e90-133">The following result is displayed:</span></span>

    ```console
    { "ok" : 1 }
    ```

1. <span data-ttu-id="24e90-134">`Books` koleksiyonu için bir şema tanımlayın ve aşağıdaki komutu kullanarak iki belge ekleyin:</span><span class="sxs-lookup"><span data-stu-id="24e90-134">Define a schema for the `Books` collection and insert two documents using the following command:</span></span>

    ```console
    db.Books.insertMany([{'Name':'Design Patterns','Price':54.93,'Category':'Computers','Author':'Ralph Johnson'}, {'Name':'Clean Code','Price':43.15,'Category':'Computers','Author':'Robert C. Martin'}])
    ```

    <span data-ttu-id="24e90-135">Aşağıdaki sonucu görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="24e90-135">The following result is displayed:</span></span>

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
  > <span data-ttu-id="24e90-136">Bu makalede gösterilen KIMLIK, bu örneği çalıştırdığınızda kimliklerle eşleşmeyecektir.</span><span class="sxs-lookup"><span data-stu-id="24e90-136">The ID's shown in this article will not match the IDs when you run this sample.</span></span>

1. <span data-ttu-id="24e90-137">Aşağıdaki komutu kullanarak veritabanında belgelerini görüntüleyin:</span><span class="sxs-lookup"><span data-stu-id="24e90-137">View the documents in the database using the following command:</span></span>

    ```console
    db.Books.find({}).pretty()
    ```

    <span data-ttu-id="24e90-138">Aşağıdaki sonucu görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="24e90-138">The following result is displayed:</span></span>

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

    <span data-ttu-id="24e90-139">Şema her belge için `ObjectId` türünde bir otomatik olarak `_id` özelliği ekler.</span><span class="sxs-lookup"><span data-stu-id="24e90-139">The schema adds an autogenerated `_id` property of type `ObjectId` for each document.</span></span>

<span data-ttu-id="24e90-140">Veritabanı hazırdır.</span><span class="sxs-lookup"><span data-stu-id="24e90-140">The database is ready.</span></span> <span data-ttu-id="24e90-141">ASP.NET Core web API'si oluşturmaya başlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="24e90-141">You can start creating the ASP.NET Core web API.</span></span>

## <a name="create-the-aspnet-core-web-api-project"></a><span data-ttu-id="24e90-142">ASP.NET Core web API projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="24e90-142">Create the ASP.NET Core web API project</span></span>

1. <span data-ttu-id="24e90-143">**Dosya** > **Yeni** > **projesi**' ne gidin.</span><span class="sxs-lookup"><span data-stu-id="24e90-143">Go to **File** > **New** > **Project**.</span></span>
1. <span data-ttu-id="24e90-144">**ASP.NET Core Web uygulaması** proje türünü seçin ve **İleri**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="24e90-144">Select the **ASP.NET Core Web Application** project type, and select **Next**.</span></span>
1. <span data-ttu-id="24e90-145">Projeyi *Booksapı*olarak adlandırın ve **Oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="24e90-145">Name the project *BooksApi*, and select **Create**.</span></span>
1. <span data-ttu-id="24e90-146">**.NET Core** hedef çerçevesini ve **3,0 ASP.NET Core**seçin.</span><span class="sxs-lookup"><span data-stu-id="24e90-146">Select the **.NET Core** target framework and **ASP.NET Core 3.0**.</span></span> <span data-ttu-id="24e90-147">**API** proje şablonunu seçin ve **Oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="24e90-147">Select the **API** project template, and select **Create**.</span></span>
1. <span data-ttu-id="24e90-148">MongoDB için .NET sürücüsünün en son kararlı sürümünü öğrenmek üzere [NuGet galerisini ziyaret edin: MongoDB. Driver](https://www.nuget.org/packages/MongoDB.Driver/) .</span><span class="sxs-lookup"><span data-stu-id="24e90-148">Visit the [NuGet Gallery: MongoDB.Driver](https://www.nuget.org/packages/MongoDB.Driver/) to determine the latest stable version of the .NET driver for MongoDB.</span></span> <span data-ttu-id="24e90-149">**Paket Yöneticisi konsol** penceresinde, proje köküne gidin.</span><span class="sxs-lookup"><span data-stu-id="24e90-149">In the **Package Manager Console** window, navigate to the project root.</span></span> <span data-ttu-id="24e90-150">MongoDB için .NET sürücüsünü yüklemek için aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="24e90-150">Run the following command to install the .NET driver for MongoDB:</span></span>

    ```powershell
    Install-Package MongoDB.Driver -Version {VERSION}
    ```

## <a name="add-an-entity-model"></a><span data-ttu-id="24e90-151">Varlık modeli ekleme</span><span class="sxs-lookup"><span data-stu-id="24e90-151">Add an entity model</span></span>

1. <span data-ttu-id="24e90-152">Proje köküne *modeller* dizini ekleyin.</span><span class="sxs-lookup"><span data-stu-id="24e90-152">Add a *Models* directory to the project root.</span></span>
1. <span data-ttu-id="24e90-153">*Modeller* dizinine aşağıdaki kodla bir `Book` sınıfı ekleyin:</span><span class="sxs-lookup"><span data-stu-id="24e90-153">Add a `Book` class to the *Models* directory with the following code:</span></span>

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

    <span data-ttu-id="24e90-154">Önceki sınıfta `Id` özelliği:</span><span class="sxs-lookup"><span data-stu-id="24e90-154">In the preceding class, the `Id` property:</span></span>

    * <span data-ttu-id="24e90-155">Ortak dil çalışma zamanı (CLR) nesnesini MongoDB koleksiyonuna eşlemek için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="24e90-155">Is required for mapping the Common Language Runtime (CLR) object to the MongoDB collection.</span></span>
    * <span data-ttu-id="24e90-156">, Bu özelliği belgenin birincil anahtarı olarak belirlemek için [`[BsonId]`](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonIdAttribute.htm) ile açıklama eklenir.</span><span class="sxs-lookup"><span data-stu-id="24e90-156">Is annotated with [`[BsonId]`](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonIdAttribute.htm) to designate this property as the document's primary key.</span></span>
    * <span data-ttu-id="24e90-157">, Parametreyi [ObjectID](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_ObjectId.htm) yapısı yerine tür `string` olarak geçirmeyi sağlayan [`[BsonRepresentation(BsonType.ObjectId)]`](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonRepresentationAttribute.htm) ile açıklama eklenir.</span><span class="sxs-lookup"><span data-stu-id="24e90-157">Is annotated with [`[BsonRepresentation(BsonType.ObjectId)]`](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonRepresentationAttribute.htm) to allow passing the parameter as type `string` instead of an [ObjectId](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_ObjectId.htm) structure.</span></span> <span data-ttu-id="24e90-158">Mongo, `string` dönüştürmeyi `ObjectId`olarak işler.</span><span class="sxs-lookup"><span data-stu-id="24e90-158">Mongo handles the conversion from `string` to `ObjectId`.</span></span>

    <span data-ttu-id="24e90-159">`BookName` özelliğine [`[BsonElement]`](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonElementAttribute.htm) özniteliğiyle açıklama eklenir.</span><span class="sxs-lookup"><span data-stu-id="24e90-159">The `BookName` property is annotated with the [`[BsonElement]`](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonElementAttribute.htm) attribute.</span></span> <span data-ttu-id="24e90-160">Özniteliğin `Name` değeri, MongoDB koleksiyonundaki özellik adını temsil eder.</span><span class="sxs-lookup"><span data-stu-id="24e90-160">The attribute's value of `Name` represents the property name in the MongoDB collection.</span></span>

## <a name="add-a-configuration-model"></a><span data-ttu-id="24e90-161">Yapılandırma modeli ekleme</span><span class="sxs-lookup"><span data-stu-id="24e90-161">Add a configuration model</span></span>

1. <span data-ttu-id="24e90-162">Aşağıdaki veritabanı yapılandırma değerlerini *appSettings. JSON*öğesine ekleyin:</span><span class="sxs-lookup"><span data-stu-id="24e90-162">Add the following database configuration values to *appsettings.json*:</span></span>

    ```javascript
    {
      "BookstoreDatabaseSettings": {
        "BooksCollectionName": "Books",
        "ConnectionString": "mongodb://localhost:27017",
        "DatabaseName": "BookstoreDb"
      },

    ```

1. <span data-ttu-id="24e90-163">*Modeller* dizinine aşağıdaki kodla bir *BookstoreDatabaseSettings.cs* dosyası ekleyin:</span><span class="sxs-lookup"><span data-stu-id="24e90-163">Add a *BookstoreDatabaseSettings.cs* file to the *Models* directory with the following code:</span></span>

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

    <span data-ttu-id="24e90-164">Yukarıdaki `BookstoreDatabaseSettings` sınıfı, *appSettings. JSON* dosyasının `BookstoreDatabaseSettings` özellik değerlerini depolamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="24e90-164">The preceding `BookstoreDatabaseSettings` class is used to store the *appsettings.json* file's `BookstoreDatabaseSettings` property values.</span></span> <span data-ttu-id="24e90-165">JSON ve C# Özellik adları, eşleme sürecini kolaylaştırmak için aynı şekilde adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="24e90-165">The JSON and C# property names are named identically to ease the mapping process.</span></span>

1. <span data-ttu-id="24e90-166">Aşağıdaki Vurgulanan kodu `Startup.ConfigureServices`ekleyin:</span><span class="sxs-lookup"><span data-stu-id="24e90-166">Add the following highlighted code to `Startup.ConfigureServices`:</span></span>

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

    <span data-ttu-id="24e90-167">Önceki kodda:</span><span class="sxs-lookup"><span data-stu-id="24e90-167">In the preceding code:</span></span>

    * <span data-ttu-id="24e90-168">*AppSettings. JSON* dosyasının `BookstoreDatabaseSettings` bölüm bağlandığı yapılandırma örneği, bağımlılık ekleme (dı) kapsayıcısına kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="24e90-168">The configuration instance to which the *appsettings.json* file's `BookstoreDatabaseSettings` section binds is registered in the Dependency Injection (DI) container.</span></span> <span data-ttu-id="24e90-169">Örneğin, bir `BookstoreDatabaseSettings` nesnesinin `ConnectionString` özelliği *appSettings. JSON*içindeki `BookstoreDatabaseSettings:ConnectionString` özelliği ile doldurulur.</span><span class="sxs-lookup"><span data-stu-id="24e90-169">For example, a `BookstoreDatabaseSettings` object's `ConnectionString` property is populated with the `BookstoreDatabaseSettings:ConnectionString` property in *appsettings.json*.</span></span>
    * <span data-ttu-id="24e90-170">`IBookstoreDatabaseSettings` arabirimi, tek bir [hizmet ömrü](xref:fundamentals/dependency-injection#service-lifetimes)ile dı 'ye kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="24e90-170">The `IBookstoreDatabaseSettings` interface is registered in DI with a singleton [service lifetime](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="24e90-171">Eklerken, arabirim örneği bir `BookstoreDatabaseSettings` nesnesine çözümlenir.</span><span class="sxs-lookup"><span data-stu-id="24e90-171">When injected, the interface instance resolves to a `BookstoreDatabaseSettings` object.</span></span>

1. <span data-ttu-id="24e90-172">`BookstoreDatabaseSettings` ve `IBookstoreDatabaseSettings` başvurularını çözümlemek için aşağıdaki kodu en *üst kısmına ekleyin* :</span><span class="sxs-lookup"><span data-stu-id="24e90-172">Add the following code to the top of *Startup.cs* to resolve the `BookstoreDatabaseSettings` and `IBookstoreDatabaseSettings` references:</span></span>

    ```csharp
    using BooksApi.Models;
    ```

## <a name="add-a-crud-operations-service"></a><span data-ttu-id="24e90-173">CRUD işlemleri hizmeti ekleme</span><span class="sxs-lookup"><span data-stu-id="24e90-173">Add a CRUD operations service</span></span>

1. <span data-ttu-id="24e90-174">Proje köküne bir *Hizmetler* dizini ekleyin.</span><span class="sxs-lookup"><span data-stu-id="24e90-174">Add a *Services* directory to the project root.</span></span>
1. <span data-ttu-id="24e90-175">*Hizmetler* dizinine aşağıdaki kodla bir `BookService` sınıfı ekleyin:</span><span class="sxs-lookup"><span data-stu-id="24e90-175">Add a `BookService` class to the *Services* directory with the following code:</span></span>

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

    <span data-ttu-id="24e90-176">Yukarıdaki kodda, Oluşturucu ekleme yoluyla dı 'den bir `IBookstoreDatabaseSettings` örneği alınır.</span><span class="sxs-lookup"><span data-stu-id="24e90-176">In the preceding code, an `IBookstoreDatabaseSettings` instance is retrieved from DI via constructor injection.</span></span> <span data-ttu-id="24e90-177">Bu teknik, [yapılandırma modeli ekleme](#add-a-configuration-model) bölümüne eklenen *appSettings. JSON* yapılandırma değerlerine erişim sağlar.</span><span class="sxs-lookup"><span data-stu-id="24e90-177">This technique provides access to the *appsettings.json* configuration values that were added in the [Add a configuration model](#add-a-configuration-model) section.</span></span>

1. <span data-ttu-id="24e90-178">Aşağıdaki Vurgulanan kodu `Startup.ConfigureServices`ekleyin:</span><span class="sxs-lookup"><span data-stu-id="24e90-178">Add the following highlighted code to `Startup.ConfigureServices`:</span></span>

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

    <span data-ttu-id="24e90-179">Önceki kodda, `BookService` sınıfı, tüketen sınıflarda Oluşturucu ekleme işlemini desteklemek için DI ile kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="24e90-179">In the preceding code, the `BookService` class is registered with DI to support constructor injection in consuming classes.</span></span> <span data-ttu-id="24e90-180">`BookService`, `MongoClient`doğrudan bağımlılığı sağladığından, tek hizmet ömrü en uygundur.</span><span class="sxs-lookup"><span data-stu-id="24e90-180">The singleton service lifetime is most appropriate because `BookService` takes a direct dependency on `MongoClient`.</span></span> <span data-ttu-id="24e90-181">Resmi [Mongo istemci yeniden kullanım yönergeleri](https://mongodb.github.io/mongo-csharp-driver/2.8/reference/driver/connecting/#re-use)temelinde `MongoClient`, tek bir hizmet ömrü ile birlikte kaydedilmelidir.</span><span class="sxs-lookup"><span data-stu-id="24e90-181">Per the official [Mongo Client reuse guidelines](https://mongodb.github.io/mongo-csharp-driver/2.8/reference/driver/connecting/#re-use), `MongoClient` should be registered in DI with a singleton service lifetime.</span></span>

1. <span data-ttu-id="24e90-182">`BookService` başvurusunu çözümlemek için aşağıdaki kodu *Startup.cs* 'un en üstüne ekleyin:</span><span class="sxs-lookup"><span data-stu-id="24e90-182">Add the following code to the top of *Startup.cs* to resolve the `BookService` reference:</span></span>


    ```csharp
    using BooksApi.Services;
    ```

<span data-ttu-id="24e90-183">`BookService` sınıfı, veritabanında CRUD işlemleri gerçekleştirmek için aşağıdaki `MongoDB.Driver` üyeleri kullanır:</span><span class="sxs-lookup"><span data-stu-id="24e90-183">The `BookService` class uses the following `MongoDB.Driver` members to perform CRUD operations against the database:</span></span>

* <span data-ttu-id="24e90-184">[Mongoclient](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoClient.htm) &ndash;, veritabanı işlemlerini gerçekleştirmek için sunucu örneğini okur.</span><span class="sxs-lookup"><span data-stu-id="24e90-184">[MongoClient](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoClient.htm) &ndash; Reads the server instance for performing database operations.</span></span> <span data-ttu-id="24e90-185">Bu sınıfın oluşturucusu, MongoDB bağlantı dizesini sağlanır:</span><span class="sxs-lookup"><span data-stu-id="24e90-185">The constructor of this class is provided the MongoDB connection string:</span></span>

    ```csharp
    public BookService(IBookstoreDatabaseSettings settings)
    {
        var client = new MongoClient(settings.ConnectionString);
        var database = client.GetDatabase(settings.DatabaseName);

        _books = database.GetCollection<Book>(settings.BooksCollectionName);
    }
    ```

* <span data-ttu-id="24e90-186">[Imongodatabase](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_IMongoDatabase.htm) &ndash; işlemleri gerçekleştirmek Için Mongo veritabanını temsil eder.</span><span class="sxs-lookup"><span data-stu-id="24e90-186">[IMongoDatabase](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_IMongoDatabase.htm) &ndash; Represents the Mongo database for performing operations.</span></span> <span data-ttu-id="24e90-187">Bu öğretici, belirli bir koleksiyondaki verilere erişim kazanmak için arabirimindeki genel [GetCollection\<TDocument > (koleksiyon)](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoDatabase_GetCollection__1.htm) yöntemini kullanır.</span><span class="sxs-lookup"><span data-stu-id="24e90-187">This tutorial uses the generic [GetCollection\<TDocument>(collection)](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoDatabase_GetCollection__1.htm) method on the interface to gain access to data in a specific collection.</span></span> <span data-ttu-id="24e90-188">Bu yöntem çağrıldıktan sonra, koleksiyonda CRUD işlemleri gerçekleştirin.</span><span class="sxs-lookup"><span data-stu-id="24e90-188">Perform CRUD operations against the collection after this method is called.</span></span> <span data-ttu-id="24e90-189">`GetCollection<TDocument>(collection)` Yöntem çağrısında:</span><span class="sxs-lookup"><span data-stu-id="24e90-189">In the `GetCollection<TDocument>(collection)` method call:</span></span>
  * <span data-ttu-id="24e90-190">`collection`, koleksiyon adını temsil eder.</span><span class="sxs-lookup"><span data-stu-id="24e90-190">`collection` represents the collection name.</span></span>
  * <span data-ttu-id="24e90-191">`TDocument`, koleksiyonda depolanan CLR nesne türünü temsil eder.</span><span class="sxs-lookup"><span data-stu-id="24e90-191">`TDocument` represents the CLR object type stored in the collection.</span></span>

<span data-ttu-id="24e90-192">`GetCollection<TDocument>(collection)`, koleksiyonu temsil eden bir [Mongocollection](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoCollection.htm) nesnesi döndürür.</span><span class="sxs-lookup"><span data-stu-id="24e90-192">`GetCollection<TDocument>(collection)` returns a [MongoCollection](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoCollection.htm) object representing the collection.</span></span> <span data-ttu-id="24e90-193">Bu öğreticide, aşağıdaki yöntemlerden koleksiyonunda çağrılır:</span><span class="sxs-lookup"><span data-stu-id="24e90-193">In this tutorial, the following methods are invoked on the collection:</span></span>

* <span data-ttu-id="24e90-194">[Deleteone](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_DeleteOne.htm) &ndash;, belirtilen arama ölçütleriyle eşleşen tek bir belgeyi siler.</span><span class="sxs-lookup"><span data-stu-id="24e90-194">[DeleteOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_DeleteOne.htm) &ndash; Deletes a single document matching the provided search criteria.</span></span>
* <span data-ttu-id="24e90-195">[\<TDocument > bul](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollectionExtensions_Find__1_1.htm) &ndash;, koleksiyonda belirtilen arama ölçütleriyle eşleşen tüm belgeleri döndürür.</span><span class="sxs-lookup"><span data-stu-id="24e90-195">[Find\<TDocument>](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollectionExtensions_Find__1_1.htm) &ndash; Returns all documents in the collection matching the provided search criteria.</span></span>
* <span data-ttu-id="24e90-196">[Insertone](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_InsertOne.htm) &ndash;, belirtilen nesneyi koleksiyondaki yeni bir belge olarak ekler.</span><span class="sxs-lookup"><span data-stu-id="24e90-196">[InsertOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_InsertOne.htm) &ndash; Inserts the provided object as a new document in the collection.</span></span>
* <span data-ttu-id="24e90-197">[Replaceone](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_ReplaceOne.htm) &ndash;, belirtilen arama ölçütleriyle eşleşen tek belgeyi, belirtilen nesneyle değiştirir.</span><span class="sxs-lookup"><span data-stu-id="24e90-197">[ReplaceOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_ReplaceOne.htm) &ndash; Replaces the single document matching the provided search criteria with the provided object.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="24e90-198">Denetleyici ekleme</span><span class="sxs-lookup"><span data-stu-id="24e90-198">Add a controller</span></span>

<span data-ttu-id="24e90-199">*Controllers* dizinine aşağıdaki kodla bir `BooksController` sınıfı ekleyin:</span><span class="sxs-lookup"><span data-stu-id="24e90-199">Add a `BooksController` class to the *Controllers* directory with the following code:</span></span>

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

<span data-ttu-id="24e90-200">Önceki web API denetleyicisi:</span><span class="sxs-lookup"><span data-stu-id="24e90-200">The preceding web API controller:</span></span>

* <span data-ttu-id="24e90-201">CRUD işlemleri gerçekleştirmek için `BookService` sınıfını kullanır.</span><span class="sxs-lookup"><span data-stu-id="24e90-201">Uses the `BookService` class to perform CRUD operations.</span></span>
* <span data-ttu-id="24e90-202">GET, POST, PUT ve DELETE HTTP isteklerini desteklemek için eylem yöntemleri içerir.</span><span class="sxs-lookup"><span data-stu-id="24e90-202">Contains action methods to support GET, POST, PUT, and DELETE HTTP requests.</span></span>
* <span data-ttu-id="24e90-203">Bir [HTTP 201](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) yanıtı döndürmek için `Create` eylemi yönteminde <xref:System.Web.Http.ApiController.CreatedAtRoute*> çağırır.</span><span class="sxs-lookup"><span data-stu-id="24e90-203">Calls <xref:System.Web.Http.ApiController.CreatedAtRoute*> in the `Create` action method to return an [HTTP 201](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) response.</span></span> <span data-ttu-id="24e90-204">Durum kodu 201, sunucuda yeni bir kaynak oluşturan HTTP POST yöntemi için standart yanıttır.</span><span class="sxs-lookup"><span data-stu-id="24e90-204">Status code 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span> <span data-ttu-id="24e90-205">`CreatedAtRoute` Ayrıca yanıta bir `Location` üst bilgisi ekler.</span><span class="sxs-lookup"><span data-stu-id="24e90-205">`CreatedAtRoute` also adds a `Location` header to the response.</span></span> <span data-ttu-id="24e90-206">`Location` üstbilgisi yeni oluşturulan kitabın URI 'sini belirtir.</span><span class="sxs-lookup"><span data-stu-id="24e90-206">The `Location` header specifies the URI of the newly created book.</span></span>

## <a name="test-the-web-api"></a><span data-ttu-id="24e90-207">Web API 'sini test etme</span><span class="sxs-lookup"><span data-stu-id="24e90-207">Test the web API</span></span>

1. <span data-ttu-id="24e90-208">Uygulamayı derleyin ve çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="24e90-208">Build and run the app.</span></span>

1. <span data-ttu-id="24e90-209">Denetleyicinin parametresiz `Get` eylem yöntemini sınamak için `http://localhost:<port>/api/books` gidin.</span><span class="sxs-lookup"><span data-stu-id="24e90-209">Navigate to `http://localhost:<port>/api/books` to test the controller's parameterless `Get` action method.</span></span> <span data-ttu-id="24e90-210">Aşağıdaki JSON yanıtı gösterilir:</span><span class="sxs-lookup"><span data-stu-id="24e90-210">The following JSON response is displayed:</span></span>

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

1. <span data-ttu-id="24e90-211">Denetleyicinin aşırı yüklenmiş `Get` eylem yöntemini sınamak için `http://localhost:<port>/api/books/{id here}` gidin.</span><span class="sxs-lookup"><span data-stu-id="24e90-211">Navigate to `http://localhost:<port>/api/books/{id here}` to test the controller's overloaded `Get` action method.</span></span> <span data-ttu-id="24e90-212">Aşağıdaki JSON yanıtı gösterilir:</span><span class="sxs-lookup"><span data-stu-id="24e90-212">The following JSON response is displayed:</span></span>

    ```json
    {
      "id":"{ID}",
      "bookName":"Clean Code",
      "price":43.15,
      "category":"Computers",
      "author":"Robert C. Martin"
    }
    ```

## <a name="configure-json-serialization-options"></a><span data-ttu-id="24e90-213">JSON serileştirme seçeneklerini yapılandırma</span><span class="sxs-lookup"><span data-stu-id="24e90-213">Configure JSON serialization options</span></span>

<span data-ttu-id="24e90-214">[Web API 'Sini test](#test-the-web-api) etme bölümünde döndürülen JSON yanıtları hakkında iki ayrıntı vardır:</span><span class="sxs-lookup"><span data-stu-id="24e90-214">There are two details to change about the JSON responses returned in the [Test the web API](#test-the-web-api) section:</span></span>

* <span data-ttu-id="24e90-215">' Varsayılan ortası büyük/küçük harf özelliği, CLR nesnesinin özellik adlarının Pascal büyük küçük harfleriyle eşleşecek şekilde değiştirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="24e90-215">The property names' default camel casing should be changed to match the Pascal casing of the CLR object's property names.</span></span>
* <span data-ttu-id="24e90-216">`bookName` özelliği `Name`olarak döndürülmelidir.</span><span class="sxs-lookup"><span data-stu-id="24e90-216">The `bookName` property should be returned as `Name`.</span></span>

<span data-ttu-id="24e90-217">Önceki gereksinimleri karşılamak için aşağıdaki değişiklikleri yapın:</span><span class="sxs-lookup"><span data-stu-id="24e90-217">To satisfy the preceding requirements, make the following changes:</span></span>

1. <span data-ttu-id="24e90-218">JSON.NET, ASP.NET paylaşılan çerçevesinden kaldırılmıştır.</span><span class="sxs-lookup"><span data-stu-id="24e90-218">JSON.NET has been removed from ASP.NET shared framework.</span></span> <span data-ttu-id="24e90-219">[Microsoft. AspNetCore. Mvc. NewtonsoftJson](https://nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson)öğesine bir paket başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="24e90-219">Add a package reference to [Microsoft.AspNetCore.Mvc.NewtonsoftJson](https://nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson).</span></span>

1. <span data-ttu-id="24e90-220">`Startup.ConfigureServices`' de, aşağıdaki vurgulanmış kodu `AddMvc` yöntemi çağrısına zincirle:</span><span class="sxs-lookup"><span data-stu-id="24e90-220">In `Startup.ConfigureServices`, chain the following highlighted code on to the `AddMvc` method call:</span></span>

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

    <span data-ttu-id="24e90-221">Önceki değişiklik ile, Web API 'sinin seri hale getirilmiş JSON yanıtındaki Özellik adları CLR nesne türündeki ilgili özellik adlarıyla eşleşir.</span><span class="sxs-lookup"><span data-stu-id="24e90-221">With the preceding change, property names in the web API's serialized JSON response match their corresponding property names in the CLR object type.</span></span> <span data-ttu-id="24e90-222">Örneğin, `Book` sınıfının `Author` özelliği `Author`olarak serileştirir.</span><span class="sxs-lookup"><span data-stu-id="24e90-222">For example, the `Book` class's `Author` property serializes as `Author`.</span></span>

1. <span data-ttu-id="24e90-223">*Modeller/Book. cs*' de, `BookName` özelliğine aşağıdaki [`[JsonProperty]`](https://www.newtonsoft.com/json/help/html/T_Newtonsoft_Json_JsonPropertyAttribute.htm) özniteliğiyle açıklama ekleyin:</span><span class="sxs-lookup"><span data-stu-id="24e90-223">In *Models/Book.cs*, annotate the `BookName` property with the following [`[JsonProperty]`](https://www.newtonsoft.com/json/help/html/T_Newtonsoft_Json_JsonPropertyAttribute.htm) attribute:</span></span>

    ```csharp
    [BsonElement("Name")]
    [JsonProperty("Name")]
    public string BookName { get; set; }
    ```

    <span data-ttu-id="24e90-224">`[JsonProperty]` özniteliğinin değeri `Name`, Web API 'sinin serileştirilmiş JSON yanıtında özellik adını temsil eder.</span><span class="sxs-lookup"><span data-stu-id="24e90-224">The `[JsonProperty]` attribute's value of `Name` represents the property name in the web API's serialized JSON response.</span></span>

1. <span data-ttu-id="24e90-225">`[JsonProperty]` öznitelik başvurusunu çözümlemek için *modeller/Book. cs* ' nin en üstüne aşağıdaki kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="24e90-225">Add the following code to the top of *Models/Book.cs* to resolve the `[JsonProperty]` attribute reference:</span></span>

    ```csharp
    using Newtonsoft.Json;
    ```

1. <span data-ttu-id="24e90-226">[Web API 'Sini test](#test-the-web-api) etme bölümünde tanımlanan adımları yineleyin.</span><span class="sxs-lookup"><span data-stu-id="24e90-226">Repeat the steps defined in the [Test the web API](#test-the-web-api) section.</span></span> <span data-ttu-id="24e90-227">JSON Özellik adlarındaki farka dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="24e90-227">Notice the difference in JSON property names.</span></span>

## <a name="next-steps"></a><span data-ttu-id="24e90-228">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="24e90-228">Next steps</span></span>

<span data-ttu-id="24e90-229">ASP.NET Core web API'leri oluşturmaya daha fazla bilgi için aşağıdaki kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="24e90-229">For more information on building ASP.NET Core web APIs, see the following resources:</span></span>

* [<span data-ttu-id="24e90-230">Bu makalenin YouTube sürümü</span><span class="sxs-lookup"><span data-stu-id="24e90-230">YouTube version of this article</span></span>](https://www.youtube.com/watch?v=7uJt_sOenyo&feature=youtu.be)
* [<span data-ttu-id="24e90-231">ASP.NET Core ile Web API 'Leri oluşturma</span><span class="sxs-lookup"><span data-stu-id="24e90-231">Create web APIs with ASP.NET Core</span></span>](https://docs.microsoft.com/aspnet/core/web-api/index?view=aspnetcore-3.0)
