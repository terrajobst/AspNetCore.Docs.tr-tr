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
# <a name="create-a-web-api-with-aspnet-core-and-mongodb"></a><span data-ttu-id="34c9f-103">MongoDB ile ASP.NET Core ile web API'si oluşturma</span><span class="sxs-lookup"><span data-stu-id="34c9f-103">Create a web API with ASP.NET Core and MongoDB</span></span>

<span data-ttu-id="34c9f-104">Tarafından [Pratik Khandelwal](https://twitter.com/K2Prk) ve [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="34c9f-104">By [Pratik Khandelwal](https://twitter.com/K2Prk) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="34c9f-105">Bu öğreticide web API'si temel oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemleri gerçekleştiren oluşturur bir [MongoDB](https://www.mongodb.com/what-is-mongodb) NoSQL veritabanı.</span><span class="sxs-lookup"><span data-stu-id="34c9f-105">This tutorial creates a web API that performs Create, Read, Update, and Delete (CRUD) operations on a [MongoDB](https://www.mongodb.com/what-is-mongodb) NoSQL database.</span></span>

<span data-ttu-id="34c9f-106">Bu öğreticide, şunların nasıl yapılır:</span><span class="sxs-lookup"><span data-stu-id="34c9f-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="34c9f-107">MongoDB yapılandırın</span><span class="sxs-lookup"><span data-stu-id="34c9f-107">Configure MongoDB</span></span>
> * <span data-ttu-id="34c9f-108">MongoDB veritabanı oluşturma</span><span class="sxs-lookup"><span data-stu-id="34c9f-108">Create a MongoDB database</span></span>
> * <span data-ttu-id="34c9f-109">MongoDB koleksiyonu ve şema tanımlayın</span><span class="sxs-lookup"><span data-stu-id="34c9f-109">Define a MongoDB collection and schema</span></span>
> * <span data-ttu-id="34c9f-110">Bir web API'sini MongoDB CRUD işlemleri gerçekleştirme</span><span class="sxs-lookup"><span data-stu-id="34c9f-110">Perform MongoDB CRUD operations from a web API</span></span>

<span data-ttu-id="34c9f-111">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-mongo-app/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="34c9f-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-mongo-app/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="34c9f-112">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="34c9f-112">Prerequisites</span></span>

* [<span data-ttu-id="34c9f-113">.NET core SDK 2.1 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="34c9f-113">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="34c9f-114">MongoDB</span><span class="sxs-lookup"><span data-stu-id="34c9f-114">MongoDB</span></span>](https://docs.mongodb.com/manual/administration/install-community/)
* <span data-ttu-id="34c9f-115">[Visual Studio 2017](https://www.visualstudio.com/downloads/) 15.7.3 sürümü veya üzeri aşağıdaki iş yükleri ile:</span><span class="sxs-lookup"><span data-stu-id="34c9f-115">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.7.3 or later with the following workloads:</span></span>
  * <span data-ttu-id="34c9f-116">**.NET core platformlar arası geliştirme**</span><span class="sxs-lookup"><span data-stu-id="34c9f-116">**.NET Core cross-platform development**</span></span>
  * <span data-ttu-id="34c9f-117">**ASP.NET ve web geliştirme**</span><span class="sxs-lookup"><span data-stu-id="34c9f-117">**ASP.NET and web development**</span></span>

## <a name="configure-mongodb"></a><span data-ttu-id="34c9f-118">MongoDB yapılandırın</span><span class="sxs-lookup"><span data-stu-id="34c9f-118">Configure MongoDB</span></span>

<span data-ttu-id="34c9f-119">Windows kullanıyorsanız, MongoDB yüklü *C:\Program Files\MongoDB* varsayılan olarak.</span><span class="sxs-lookup"><span data-stu-id="34c9f-119">If using Windows, MongoDB is installed at *C:\Program Files\MongoDB* by default.</span></span> <span data-ttu-id="34c9f-120">Ekleme *C:\Program Files\MongoDB\Server\<version_number > \bin* için `Path` ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="34c9f-120">Add *C:\Program Files\MongoDB\Server\<version_number>\bin* to the `Path` environment variable.</span></span> <span data-ttu-id="34c9f-121">Bu değişiklik yerden MongoDB erişim sağlar, geliştirme makinenizde.</span><span class="sxs-lookup"><span data-stu-id="34c9f-121">This change enables MongoDB access from anywhere on your development machine.</span></span>

<span data-ttu-id="34c9f-122">Mongo kabuğunu veritabanı oluşturma, koleksiyonları yapın ve belgeleri depolamak için aşağıdaki adımları kullanın.</span><span class="sxs-lookup"><span data-stu-id="34c9f-122">Use the mongo Shell in the following steps to create a database, make collections, and store documents.</span></span> <span data-ttu-id="34c9f-123">Mongo Kabuğu komutları hakkında daha fazla bilgi için bkz. [mongo kabuğunu çalışma](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell).</span><span class="sxs-lookup"><span data-stu-id="34c9f-123">For more information on mongo Shell commands, see [Working with the mongo Shell](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell).</span></span>

1. <span data-ttu-id="34c9f-124">Geliştirme makinenizde verilerin depolanması için bir dizin seçin.</span><span class="sxs-lookup"><span data-stu-id="34c9f-124">Choose a directory on your development machine for storing the data.</span></span> <span data-ttu-id="34c9f-125">Örneğin, *C:\BooksData* Windows üzerinde.</span><span class="sxs-lookup"><span data-stu-id="34c9f-125">For example, *C:\BooksData* on Windows.</span></span> <span data-ttu-id="34c9f-126">Yoksa dizini oluşturun.</span><span class="sxs-lookup"><span data-stu-id="34c9f-126">Create the directory if it doesn't exist.</span></span> <span data-ttu-id="34c9f-127">Mongo kabuğunu yeni dizinleri oluşturmaz.</span><span class="sxs-lookup"><span data-stu-id="34c9f-127">The mongo Shell doesn't create new directories.</span></span>
1. <span data-ttu-id="34c9f-128">Bir komut kabuğunu açın.</span><span class="sxs-lookup"><span data-stu-id="34c9f-128">Open a command shell.</span></span> <span data-ttu-id="34c9f-129">Varsayılan bağlantı noktası 27017 mongodb'ye bağlanmak için aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="34c9f-129">Run the following command to connect to MongoDB on default port 27017.</span></span> <span data-ttu-id="34c9f-130">Değiştirmeyi unutmayın `<data_directory_path>` önceki adımda seçtiğiniz dizini.</span><span class="sxs-lookup"><span data-stu-id="34c9f-130">Remember to replace `<data_directory_path>` with the directory you chose in the previous step.</span></span>

    ```console
    mongod --dbpath <data_directory_path>
    ```

1. <span data-ttu-id="34c9f-131">Başka bir komut kabuğu örneği açın.</span><span class="sxs-lookup"><span data-stu-id="34c9f-131">Open another command shell instance.</span></span> <span data-ttu-id="34c9f-132">Aşağıdaki komutu çalıştırarak varsayılan test veritabanı'na bağlanma:</span><span class="sxs-lookup"><span data-stu-id="34c9f-132">Connect to the default test database by running the following command:</span></span>

    ```console
    mongo
    ```

1. <span data-ttu-id="34c9f-133">Bir komut kabuğu'nda aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="34c9f-133">Run the following in a command shell:</span></span>

    ```console
    use BookstoreDb
    ```

    <span data-ttu-id="34c9f-134">Adlı bir veritabanı zaten mevcut olmayan halinde *BookstoreDb* oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="34c9f-134">If it doesn't already exist, a database named *BookstoreDb* is created.</span></span> <span data-ttu-id="34c9f-135">Veritabanı mevcut değilse, bağlantı işlemleri için açılır.</span><span class="sxs-lookup"><span data-stu-id="34c9f-135">If the database does exist, its connection is opened for transactions.</span></span>

1. <span data-ttu-id="34c9f-136">Oluşturma bir `Books` koleksiyon aşağıdaki komutu kullanarak:</span><span class="sxs-lookup"><span data-stu-id="34c9f-136">Create a `Books` collection using following command:</span></span>

    ```console
    db.createCollection('Books')
    ```

    <span data-ttu-id="34c9f-137">Aşağıdaki sonucu görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="34c9f-137">The following result is displayed:</span></span>

    ```console
    { "ok" : 1 }
    ```

1. <span data-ttu-id="34c9f-138">İçin bir şema tanımlayabilir `Books` toplama ve ekleme iki belge aşağıdaki komutu kullanarak:</span><span class="sxs-lookup"><span data-stu-id="34c9f-138">Define a schema for the `Books` collection and insert two documents using the following command:</span></span>

    ```console
    db.Books.insertMany([{'Name':'Design Patterns','Price':54.93,'Category':'Computers','Author':'Ralph Johnson'}, {'Name':'Clean Code','Price':43.15,'Category':'Computers','Author':'Robert C. Martin'}])
    ```

    <span data-ttu-id="34c9f-139">Aşağıdaki sonucu görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="34c9f-139">The following result is displayed:</span></span>

    ```console
    {
      "acknowledged" : true,
      "insertedIds" : [
        ObjectId("5bfd996f7b8e48dc15ff215d"),
        ObjectId("5bfd996f7b8e48dc15ff215e")
      ]
    }
    ```

1. <span data-ttu-id="34c9f-140">Aşağıdaki komutu kullanarak veritabanında belgelerini görüntüleyin:</span><span class="sxs-lookup"><span data-stu-id="34c9f-140">View the documents in the database using the following command:</span></span>

    ```console
    db.Books.find({}).pretty()
    ```

    <span data-ttu-id="34c9f-141">Aşağıdaki sonucu görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="34c9f-141">The following result is displayed:</span></span>

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

    <span data-ttu-id="34c9f-142">Şema girmiş ekler `_id` türünün özelliği `ObjectId` her belge için.</span><span class="sxs-lookup"><span data-stu-id="34c9f-142">The schema adds an autogenerated `_id` property of type `ObjectId` for each document.</span></span>

<span data-ttu-id="34c9f-143">Veritabanı hazırdır.</span><span class="sxs-lookup"><span data-stu-id="34c9f-143">The database is ready.</span></span> <span data-ttu-id="34c9f-144">ASP.NET Core web API'si oluşturmaya başlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="34c9f-144">You can start creating the ASP.NET Core web API.</span></span>

## <a name="create-the-aspnet-core-web-api-project"></a><span data-ttu-id="34c9f-145">ASP.NET Core web API projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="34c9f-145">Create the ASP.NET Core web API project</span></span>

1. <span data-ttu-id="34c9f-146">Visual Studio'da Git **dosya** > **yeni** > **proje**.</span><span class="sxs-lookup"><span data-stu-id="34c9f-146">In Visual Studio, go to **File** > **New** > **Project**.</span></span>
1. <span data-ttu-id="34c9f-147">Seçin **ASP.NET Core Web uygulaması**, projeyi adlandırın *BookMongo*, tıklatıp **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="34c9f-147">Select **ASP.NET Core Web Application**, name the project *BookMongo*, and click **OK**.</span></span>
1. <span data-ttu-id="34c9f-148">Seçin **.NET Core** hedef çerçeve ve **ASP.NET Core 2.1**.</span><span class="sxs-lookup"><span data-stu-id="34c9f-148">Select the **.NET Core** target framework and **ASP.NET Core 2.1**.</span></span> <span data-ttu-id="34c9f-149">Seçin **API** proje şablonu ve tıklayın **Tamam**:</span><span class="sxs-lookup"><span data-stu-id="34c9f-149">Select the **API** project template, and click **OK**:</span></span>
1. <span data-ttu-id="34c9f-150">İçinde **Paket Yöneticisi Konsolu** penceresinde proje kök dizinine gidin.</span><span class="sxs-lookup"><span data-stu-id="34c9f-150">In the **Package Manager Console** window, navigate to the project root.</span></span> <span data-ttu-id="34c9f-151">MongoDB için .NET sürücüsünü yüklemek için aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="34c9f-151">Run the following command to install the .NET driver for MongoDB:</span></span>

    ```powershell
    Install-Package MongoDB.Driver -Version 2.7.2
    ```

## <a name="add-a-model"></a><span data-ttu-id="34c9f-152">Model ekleme</span><span class="sxs-lookup"><span data-stu-id="34c9f-152">Add a model</span></span>

1. <span data-ttu-id="34c9f-153">Ekleme bir *modelleri* proje kök klasör.</span><span class="sxs-lookup"><span data-stu-id="34c9f-153">Add a *Models* folder to the project root.</span></span>
1. <span data-ttu-id="34c9f-154">Ekleme bir `Book` sınıfının *modelleri* aşağıdaki kodla klasörü:</span><span class="sxs-lookup"><span data-stu-id="34c9f-154">Add a `Book` class to the *Models* folder with the following code:</span></span>

    [!code-csharp[](first-mongo-app/sample/BookstoreAPI/Models/Book.cs)]

<span data-ttu-id="34c9f-155">Önceki sınıfında `Id` özelliği ortak dil çalışma zamanı (CLR) nesnesi için MongoDB koleksiyonu eşlemek için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="34c9f-155">In the preceding class, the `Id` property is required for mapping the Common Language Runtime (CLR) object to the MongoDB collection.</span></span> <span data-ttu-id="34c9f-156">Sınıftaki diğer özellikler ile donatılmış `[BsonElement]` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="34c9f-156">Other properties in the class are decorated with the `[BsonElement]` attribute.</span></span> <span data-ttu-id="34c9f-157">Özniteliğin değeri, özellik adı, MongoDB koleksiyonu temsil eder.</span><span class="sxs-lookup"><span data-stu-id="34c9f-157">The attribute's value represents the property name in the MongoDB collection.</span></span>

## <a name="add-a-crud-operations-class"></a><span data-ttu-id="34c9f-158">CRUD işlemleri sınıfı Ekle</span><span class="sxs-lookup"><span data-stu-id="34c9f-158">Add a CRUD operations class</span></span>

1. <span data-ttu-id="34c9f-159">Ekleme bir *Hizmetleri* proje kök klasör.</span><span class="sxs-lookup"><span data-stu-id="34c9f-159">Add a *Services* folder to the project root.</span></span>
1. <span data-ttu-id="34c9f-160">Ekleme bir `BookService` sınıfının *Hizmetleri* aşağıdaki kodla klasörü:</span><span class="sxs-lookup"><span data-stu-id="34c9f-160">Add a `BookService` class to the *Services* folder with the following code:</span></span>

    [!code-csharp[](first-mongo-app/sample/BookstoreAPI/Services/BookService.cs?name=snippet_BookServiceClass)]

1. <span data-ttu-id="34c9f-161">MongoDB bağlantı dizesi Ekle *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="34c9f-161">Add the MongoDB connection string to *appsettings.json*:</span></span>

    [!code-csharp[](first-mongo-app/sample/BookstoreAPI/appsettings.json?highlight=2-4)]

    <span data-ttu-id="34c9f-162">Önceki `BookstoreDb` özelliğine erişilirse `BookService` sınıf oluşturucusu.</span><span class="sxs-lookup"><span data-stu-id="34c9f-162">The preceding `BookstoreDb` property is accessed in the `BookService` class constructor.</span></span>

1. <span data-ttu-id="34c9f-163">İçinde `Startup.ConfigureServices`, kayıt `BookService` bağımlılık ekleme sistemiyle sınıfı:</span><span class="sxs-lookup"><span data-stu-id="34c9f-163">In `Startup.ConfigureServices`, register the `BookService` class with the Dependency Injection system:</span></span>

    [!code-csharp[](first-mongo-app/sample/BookstoreAPI/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

    <span data-ttu-id="34c9f-164">Önceki hizmet kaydı sınıfları tüketen yapıcı eklemeyi desteklemek gereklidir.</span><span class="sxs-lookup"><span data-stu-id="34c9f-164">The preceding service registration is necessary to support constructor injection in consuming classes.</span></span>

<span data-ttu-id="34c9f-165">`BookService` Sınıfını kullanan aşağıdaki `MongoDB.Driver` veritabanında CRUD işlemleri gerçekleştirmek için üyeleri:</span><span class="sxs-lookup"><span data-stu-id="34c9f-165">The `BookService` class uses the following `MongoDB.Driver` members to perform CRUD operations against the database:</span></span>

* <span data-ttu-id="34c9f-166">`MongoClient` &ndash; Veritabanı işlemleri gerçekleştirmek için kullanılan bir sunucuyu okur.</span><span class="sxs-lookup"><span data-stu-id="34c9f-166">`MongoClient` &ndash; Reads the server instance for performing database operations.</span></span> <span data-ttu-id="34c9f-167">Bu sınıfın oluşturucusu, MongoDB bağlantı dizesini sağlanır:</span><span class="sxs-lookup"><span data-stu-id="34c9f-167">The constructor of this class is provided the MongoDB connection string:</span></span>

    [!code-csharp[](first-mongo-app/sample/BookstoreAPI/Services/BookService.cs?name=snippet_BookServiceConstructor&highlight=3)]

* <span data-ttu-id="34c9f-168">`IMongoDatabase` &ndash; Mongo veritabanı işlemleri gerçekleştirmek için temsil eder.</span><span class="sxs-lookup"><span data-stu-id="34c9f-168">`IMongoDatabase` &ndash; Represents the Mongo database for performing operations.</span></span> <span data-ttu-id="34c9f-169">Bu öğreticide genel `GetCollection<T>(collection)` yöntemi belirli bir koleksiyondaki verileri erişim elde etmek için arabirim.</span><span class="sxs-lookup"><span data-stu-id="34c9f-169">This tutorial uses the generic `GetCollection<T>(collection)` method on the interface to gain access to data in a specific collection.</span></span> <span data-ttu-id="34c9f-170">Bu yöntemi çağrıldıktan sonra koleksiyonunda CRUD işlemleri gerçekleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="34c9f-170">CRUD operations can be performed against the collection after this method is called.</span></span> <span data-ttu-id="34c9f-171">İçinde `GetCollection<T>(collection)` yöntem çağrısı:</span><span class="sxs-lookup"><span data-stu-id="34c9f-171">In the `GetCollection<T>(collection)` method call:</span></span>
  * <span data-ttu-id="34c9f-172">`collection` Koleksiyon adını temsil eder.</span><span class="sxs-lookup"><span data-stu-id="34c9f-172">`collection` represents the collection name.</span></span>
  * <span data-ttu-id="34c9f-173">`T` Bir koleksiyonda depolanan CLR nesne türünü temsil eder.</span><span class="sxs-lookup"><span data-stu-id="34c9f-173">`T` represents the CLR object type stored in the collection.</span></span>

<span data-ttu-id="34c9f-174">`GetCollection<T>(collection)` döndürür bir `MongoCollection` koleksiyonu temsil eden nesne.</span><span class="sxs-lookup"><span data-stu-id="34c9f-174">`GetCollection<T>(collection)` returns a `MongoCollection` object representing the collection.</span></span> <span data-ttu-id="34c9f-175">Bu öğreticide, aşağıdaki yöntemlerden koleksiyonunda çağrılır:</span><span class="sxs-lookup"><span data-stu-id="34c9f-175">In this tutorial, the following methods are invoked on the collection:</span></span>

* <span data-ttu-id="34c9f-176">`Find<T>` &ndash; Sağlanan arama ölçütleriyle eşleşen koleksiyondaki tüm belgeleri döndürür.</span><span class="sxs-lookup"><span data-stu-id="34c9f-176">`Find<T>` &ndash; Returns all documents in the collection matching the provided search criteria.</span></span>
* <span data-ttu-id="34c9f-177">`InsertOne` &ndash; Belirtilen nesne koleksiyonunda yeni bir belge olarak ekler.</span><span class="sxs-lookup"><span data-stu-id="34c9f-177">`InsertOne` &ndash; Inserts the provided object as a new document in the collection.</span></span>
* <span data-ttu-id="34c9f-178">`ReplaceOne` &ndash; Sağlanan nesne ile sağlanan arama ölçütleriyle eşleşen tek bir belge değiştirir.</span><span class="sxs-lookup"><span data-stu-id="34c9f-178">`ReplaceOne` &ndash; Replaces the single document matching the provided search criteria with the provided object.</span></span>
* <span data-ttu-id="34c9f-179">`DeleteOne` &ndash; Belirtilen arama ölçütleriyle eşleşen tek bir belge siler.</span><span class="sxs-lookup"><span data-stu-id="34c9f-179">`DeleteOne` &ndash; Deletes a single document matching the provided search criteria.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="34c9f-180">Denetleyici ekleme</span><span class="sxs-lookup"><span data-stu-id="34c9f-180">Add a controller</span></span>

1. <span data-ttu-id="34c9f-181">Sağ *denetleyicileri* klasöründe **Çözüm Gezgini**.</span><span class="sxs-lookup"><span data-stu-id="34c9f-181">Right-click the *Controllers* folder in **Solution Explorer**.</span></span> <span data-ttu-id="34c9f-182">Seçin **ekleme** > **denetleyicisi**.</span><span class="sxs-lookup"><span data-stu-id="34c9f-182">Select **Add** > **Controller**.</span></span>
1. <span data-ttu-id="34c9f-183">Seçin **API denetleyici - boş** öğe şablonu ve tıklayın **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="34c9f-183">Choose the **API Controller - Empty** item template, and click **Add**.</span></span>
1. <span data-ttu-id="34c9f-184">Girin *BooksController* içinde **Denetleyici adı** metin kutusu seçeneğine tıklayıp **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="34c9f-184">Enter *BooksController* in the **Controller name** text box, and click **Add**.</span></span>
1. <span data-ttu-id="34c9f-185">Aşağıdaki kodu ekleyin *BooksController.cs*:</span><span class="sxs-lookup"><span data-stu-id="34c9f-185">Add the following code to *BooksController.cs*:</span></span>

    [!code-csharp[](first-mongo-app/sample/BookstoreAPI/Controllers/BooksController.cs)]

    <span data-ttu-id="34c9f-186">Önceki web API denetleyicisi:</span><span class="sxs-lookup"><span data-stu-id="34c9f-186">The preceding web API controller:</span></span>

    * <span data-ttu-id="34c9f-187">Kullanan `BookService` CRUD işlemleri gerçekleştirmek için sınıf.</span><span class="sxs-lookup"><span data-stu-id="34c9f-187">Uses the `BookService` class to perform CRUD operations.</span></span>
    * <span data-ttu-id="34c9f-188">GET, POST, PUT ve DELETE HTTP isteklerini desteklemek için eylem yöntemleri içerir.</span><span class="sxs-lookup"><span data-stu-id="34c9f-188">Contains action methods to support GET, POST, PUT, and DELETE HTTP requests.</span></span>
1. <span data-ttu-id="34c9f-189">Derleme ve uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="34c9f-189">Build and run the app.</span></span>
1. <span data-ttu-id="34c9f-190">Gidin `http://localhost:<port>/api/books` tarayıcınızda.</span><span class="sxs-lookup"><span data-stu-id="34c9f-190">Navigate to `http://localhost:<port>/api/books` in your browser.</span></span> <span data-ttu-id="34c9f-191">Aşağıdaki JSON yanıtı gösterilir:</span><span class="sxs-lookup"><span data-stu-id="34c9f-191">The following JSON response is displayed:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="34c9f-192">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="34c9f-192">Next steps</span></span>

<span data-ttu-id="34c9f-193">ASP.NET Core web API'leri oluşturmaya daha fazla bilgi için aşağıdaki kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="34c9f-193">For more information on building ASP.NET Core web APIs, see the following resources:</span></span>

* <xref:web-api/index>
* <xref:web-api/action-return-types>
