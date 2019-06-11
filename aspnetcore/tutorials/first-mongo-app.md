---
title: MongoDB ile ASP.NET Core ile web API'si oluşturma
author: prkhandelwal
description: Bu öğreticide bir ASP.NET Core web API'sini kullanarak bir MongoDB NoSQL veritabanı oluşturma işlemini gösterir.
ms.author: scaddie
ms.custom: mvc, seodec18
ms.date: 06/10/2019
uid: tutorials/first-mongo-app
ms.openlocfilehash: 5e3bdb10f0e192ba98df442959ceb68dc7c7adc5
ms.sourcegitcommit: 9691b742134563b662948b0ed63f54ef7186801e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/10/2019
ms.locfileid: "66824783"
---
# <a name="create-a-web-api-with-aspnet-core-and-mongodb"></a><span data-ttu-id="3494a-103">MongoDB ile ASP.NET Core ile web API'si oluşturma</span><span class="sxs-lookup"><span data-stu-id="3494a-103">Create a web API with ASP.NET Core and MongoDB</span></span>

<span data-ttu-id="3494a-104">Tarafından [Pratik Khandelwal](https://twitter.com/K2Prk) ve [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="3494a-104">By [Pratik Khandelwal](https://twitter.com/K2Prk) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="3494a-105">Bu öğreticide web API'si temel oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemleri gerçekleştiren oluşturur bir [MongoDB](https://www.mongodb.com/what-is-mongodb) NoSQL veritabanı.</span><span class="sxs-lookup"><span data-stu-id="3494a-105">This tutorial creates a web API that performs Create, Read, Update, and Delete (CRUD) operations on a [MongoDB](https://www.mongodb.com/what-is-mongodb) NoSQL database.</span></span>

<span data-ttu-id="3494a-106">Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:</span><span class="sxs-lookup"><span data-stu-id="3494a-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="3494a-107">MongoDB yapılandırın</span><span class="sxs-lookup"><span data-stu-id="3494a-107">Configure MongoDB</span></span>
> * <span data-ttu-id="3494a-108">MongoDB veritabanı oluşturma</span><span class="sxs-lookup"><span data-stu-id="3494a-108">Create a MongoDB database</span></span>
> * <span data-ttu-id="3494a-109">MongoDB koleksiyonu ve şema tanımlayın</span><span class="sxs-lookup"><span data-stu-id="3494a-109">Define a MongoDB collection and schema</span></span>
> * <span data-ttu-id="3494a-110">Bir web API'sini MongoDB CRUD işlemleri gerçekleştirme</span><span class="sxs-lookup"><span data-stu-id="3494a-110">Perform MongoDB CRUD operations from a web API</span></span>
> * <span data-ttu-id="3494a-111">JSON seri hale getirme özelleştirmek</span><span class="sxs-lookup"><span data-stu-id="3494a-111">Customize JSON serialization</span></span>

<span data-ttu-id="3494a-112">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-mongo-app/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="3494a-112">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-mongo-app/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3494a-113">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="3494a-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3494a-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3494a-114">Visual Studio</span></span>](#tab/visual-studio)

* [<span data-ttu-id="3494a-115">.NET core SDK 2.2 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="3494a-115">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="3494a-116">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) ile **ASP.NET ve web geliştirme** iş yükü</span><span class="sxs-lookup"><span data-stu-id="3494a-116">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="3494a-117">MongoDB</span><span class="sxs-lookup"><span data-stu-id="3494a-117">MongoDB</span></span>](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="3494a-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="3494a-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="3494a-119">.NET core SDK 2.2 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="3494a-119">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="3494a-120">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="3494a-120">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="3494a-121">Visual Studio Code için C#</span><span class="sxs-lookup"><span data-stu-id="3494a-121">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [<span data-ttu-id="3494a-122">MongoDB</span><span class="sxs-lookup"><span data-stu-id="3494a-122">MongoDB</span></span>](https://docs.mongodb.com/manual/administration/install-community/)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="3494a-123">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3494a-123">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* [<span data-ttu-id="3494a-124">.NET core SDK 2.2 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="3494a-124">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="3494a-125">Mac 7,7 veya sonraki bir sürümü için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3494a-125">Visual Studio for Mac version 7.7 or later</span></span>](https://visualstudio.microsoft.com/downloads/)
* [<span data-ttu-id="3494a-126">MongoDB</span><span class="sxs-lookup"><span data-stu-id="3494a-126">MongoDB</span></span>](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-os-x/)

---

## <a name="configure-mongodb"></a><span data-ttu-id="3494a-127">MongoDB yapılandırın</span><span class="sxs-lookup"><span data-stu-id="3494a-127">Configure MongoDB</span></span>

<span data-ttu-id="3494a-128">Windows kullanıyorsanız, MongoDB yüklü *C:\\Program dosyaları\\MongoDB* varsayılan olarak.</span><span class="sxs-lookup"><span data-stu-id="3494a-128">If using Windows, MongoDB is installed at *C:\\Program Files\\MongoDB* by default.</span></span> <span data-ttu-id="3494a-129">Ekleme *C:\\Program dosyaları\\MongoDB\\sunucu\\\<version_number >\\bin* için `Path` ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="3494a-129">Add *C:\\Program Files\\MongoDB\\Server\\\<version_number>\\bin* to the `Path` environment variable.</span></span> <span data-ttu-id="3494a-130">Bu değişiklik yerden MongoDB erişim sağlar, geliştirme makinenizde.</span><span class="sxs-lookup"><span data-stu-id="3494a-130">This change enables MongoDB access from anywhere on your development machine.</span></span>

<span data-ttu-id="3494a-131">Mongo kabuğunu veritabanı oluşturma, koleksiyonları yapın ve belgeleri depolamak için aşağıdaki adımları kullanın.</span><span class="sxs-lookup"><span data-stu-id="3494a-131">Use the mongo Shell in the following steps to create a database, make collections, and store documents.</span></span> <span data-ttu-id="3494a-132">Mongo Kabuğu komutları hakkında daha fazla bilgi için bkz. [mongo kabuğunu çalışma](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell).</span><span class="sxs-lookup"><span data-stu-id="3494a-132">For more information on mongo Shell commands, see [Working with the mongo Shell](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell).</span></span>

1. <span data-ttu-id="3494a-133">Geliştirme makinenizde verilerin depolanması için bir dizin seçin.</span><span class="sxs-lookup"><span data-stu-id="3494a-133">Choose a directory on your development machine for storing the data.</span></span> <span data-ttu-id="3494a-134">Örneğin, *C:\\BooksData* Windows üzerinde.</span><span class="sxs-lookup"><span data-stu-id="3494a-134">For example, *C:\\BooksData* on Windows.</span></span> <span data-ttu-id="3494a-135">Yoksa dizini oluşturun.</span><span class="sxs-lookup"><span data-stu-id="3494a-135">Create the directory if it doesn't exist.</span></span> <span data-ttu-id="3494a-136">Mongo kabuğunu yeni dizinleri oluşturmaz.</span><span class="sxs-lookup"><span data-stu-id="3494a-136">The mongo Shell doesn't create new directories.</span></span>
1. <span data-ttu-id="3494a-137">Bir komut kabuğunu açın.</span><span class="sxs-lookup"><span data-stu-id="3494a-137">Open a command shell.</span></span> <span data-ttu-id="3494a-138">Varsayılan bağlantı noktası 27017 mongodb'ye bağlanmak için aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="3494a-138">Run the following command to connect to MongoDB on default port 27017.</span></span> <span data-ttu-id="3494a-139">Değiştirmeyi unutmayın `<data_directory_path>` önceki adımda seçtiğiniz dizini.</span><span class="sxs-lookup"><span data-stu-id="3494a-139">Remember to replace `<data_directory_path>` with the directory you chose in the previous step.</span></span>

    ```console
    mongod --dbpath <data_directory_path>
    ```

1. <span data-ttu-id="3494a-140">Başka bir komut kabuğu örneği açın.</span><span class="sxs-lookup"><span data-stu-id="3494a-140">Open another command shell instance.</span></span> <span data-ttu-id="3494a-141">Aşağıdaki komutu çalıştırarak varsayılan test veritabanı'na bağlanma:</span><span class="sxs-lookup"><span data-stu-id="3494a-141">Connect to the default test database by running the following command:</span></span>

    ```console
    mongo
    ```

1. <span data-ttu-id="3494a-142">Bir komut kabuğu'nda aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="3494a-142">Run the following in a command shell:</span></span>

    ```console
    use BookstoreDb
    ```

    <span data-ttu-id="3494a-143">Adlı bir veritabanı zaten mevcut olmayan halinde *BookstoreDb* oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="3494a-143">If it doesn't already exist, a database named *BookstoreDb* is created.</span></span> <span data-ttu-id="3494a-144">Veritabanı mevcut değilse, bağlantı işlemleri için açılır.</span><span class="sxs-lookup"><span data-stu-id="3494a-144">If the database does exist, its connection is opened for transactions.</span></span>

1. <span data-ttu-id="3494a-145">Oluşturma bir `Books` koleksiyon aşağıdaki komutu kullanarak:</span><span class="sxs-lookup"><span data-stu-id="3494a-145">Create a `Books` collection using following command:</span></span>

    ```console
    db.createCollection('Books')
    ```

    <span data-ttu-id="3494a-146">Aşağıdaki sonucu görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="3494a-146">The following result is displayed:</span></span>

    ```console
    { "ok" : 1 }
    ```

1. <span data-ttu-id="3494a-147">İçin bir şema tanımlayabilir `Books` toplama ve ekleme iki belge aşağıdaki komutu kullanarak:</span><span class="sxs-lookup"><span data-stu-id="3494a-147">Define a schema for the `Books` collection and insert two documents using the following command:</span></span>

    ```console
    db.Books.insertMany([{'Name':'Design Patterns','Price':54.93,'Category':'Computers','Author':'Ralph Johnson'}, {'Name':'Clean Code','Price':43.15,'Category':'Computers','Author':'Robert C. Martin'}])
    ```

    <span data-ttu-id="3494a-148">Aşağıdaki sonucu görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="3494a-148">The following result is displayed:</span></span>

    ```console
    {
      "acknowledged" : true,
      "insertedIds" : [
        ObjectId("5bfd996f7b8e48dc15ff215d"),
        ObjectId("5bfd996f7b8e48dc15ff215e")
      ]
    }
    ```

1. <span data-ttu-id="3494a-149">Aşağıdaki komutu kullanarak veritabanında belgelerini görüntüleyin:</span><span class="sxs-lookup"><span data-stu-id="3494a-149">View the documents in the database using the following command:</span></span>

    ```console
    db.Books.find({}).pretty()
    ```

    <span data-ttu-id="3494a-150">Aşağıdaki sonucu görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="3494a-150">The following result is displayed:</span></span>

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

    <span data-ttu-id="3494a-151">Şema girmiş ekler `_id` türünün özelliği `ObjectId` her belge için.</span><span class="sxs-lookup"><span data-stu-id="3494a-151">The schema adds an autogenerated `_id` property of type `ObjectId` for each document.</span></span>

<span data-ttu-id="3494a-152">Veritabanı hazırdır.</span><span class="sxs-lookup"><span data-stu-id="3494a-152">The database is ready.</span></span> <span data-ttu-id="3494a-153">ASP.NET Core web API'si oluşturmaya başlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3494a-153">You can start creating the ASP.NET Core web API.</span></span>

## <a name="create-the-aspnet-core-web-api-project"></a><span data-ttu-id="3494a-154">ASP.NET Core web API projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="3494a-154">Create the ASP.NET Core web API project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3494a-155">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3494a-155">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="3494a-156">Git **dosya** > **yeni** > **proje**.</span><span class="sxs-lookup"><span data-stu-id="3494a-156">Go to **File** > **New** > **Project**.</span></span>
1. <span data-ttu-id="3494a-157">Seçin **ASP.NET Core Web uygulaması** proje türü ve seçin **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="3494a-157">Select the **ASP.NET Core Web Application** project type, and select **Next**.</span></span>
1. <span data-ttu-id="3494a-158">Projeyi adlandırın *BooksApi*seçip **Oluştur**.</span><span class="sxs-lookup"><span data-stu-id="3494a-158">Name the project *BooksApi*, and select **Create**.</span></span>
1. <span data-ttu-id="3494a-159">Seçin **.NET Core** hedef çerçeve ve **ASP.NET Core 2.2**.</span><span class="sxs-lookup"><span data-stu-id="3494a-159">Select the **.NET Core** target framework and **ASP.NET Core 2.2**.</span></span> <span data-ttu-id="3494a-160">Seçin **API** proje şablonu, belirleyin **Oluştur**.</span><span class="sxs-lookup"><span data-stu-id="3494a-160">Select the **API** project template, and select **Create**.</span></span>
1. <span data-ttu-id="3494a-161">Ziyaret [NuGet Galerisi: Mongodb](https://www.nuget.org/packages/MongoDB.Driver/) için MongoDB .NET sürücüsü en son kararlı sürümünü belirlemek için.</span><span class="sxs-lookup"><span data-stu-id="3494a-161">Visit the [NuGet Gallery: MongoDB.Driver](https://www.nuget.org/packages/MongoDB.Driver/) to determine the latest stable version of the .NET driver for MongoDB.</span></span> <span data-ttu-id="3494a-162">İçinde **Paket Yöneticisi Konsolu** penceresinde proje kök dizinine gidin.</span><span class="sxs-lookup"><span data-stu-id="3494a-162">In the **Package Manager Console** window, navigate to the project root.</span></span> <span data-ttu-id="3494a-163">MongoDB için .NET sürücüsünü yüklemek için aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="3494a-163">Run the following command to install the .NET driver for MongoDB:</span></span>

    ```powershell
    Install-Package MongoDB.Driver -Version {VERSION}
    ```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="3494a-164">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="3494a-164">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="3494a-165">Bir komut kabuğu'nda aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="3494a-165">Run the following commands in a command shell:</span></span>

    ```console
    dotnet new webapi -o BooksApi
    code BooksApi
    ```

    <span data-ttu-id="3494a-166">.NET Core'u hedefleyen yeni bir ASP.NET Core web API projesi oluşturulur ve Visual Studio Code'da açılır.</span><span class="sxs-lookup"><span data-stu-id="3494a-166">A new ASP.NET Core web API project targeting .NET Core is generated and opened in Visual Studio Code.</span></span>

1. <span data-ttu-id="3494a-167">Durum çubuğunun OmniSharp sonra soran bir iletişim kutusu yangın simgesi yeşile **gerekli varlıkları oluşturun ve hata ayıklama 'BooksApi' eksik. Bunları eklensin mi?** .</span><span class="sxs-lookup"><span data-stu-id="3494a-167">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'BooksApi'. Add them?**.</span></span> <span data-ttu-id="3494a-168">Seçin **Evet**.</span><span class="sxs-lookup"><span data-stu-id="3494a-168">Select **Yes**.</span></span>
1. <span data-ttu-id="3494a-169">Ziyaret [NuGet Galerisi: Mongodb](https://www.nuget.org/packages/MongoDB.Driver/) için MongoDB .NET sürücüsü en son kararlı sürümünü belirlemek için.</span><span class="sxs-lookup"><span data-stu-id="3494a-169">Visit the [NuGet Gallery: MongoDB.Driver](https://www.nuget.org/packages/MongoDB.Driver/) to determine the latest stable version of the .NET driver for MongoDB.</span></span> <span data-ttu-id="3494a-170">Açık **tümleşik Terminalini** ve proje kök dizinine gidin.</span><span class="sxs-lookup"><span data-stu-id="3494a-170">Open **Integrated Terminal** and navigate to the project root.</span></span> <span data-ttu-id="3494a-171">MongoDB için .NET sürücüsünü yüklemek için aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="3494a-171">Run the following command to install the .NET driver for MongoDB:</span></span>

    ```console
    dotnet add BooksApi.csproj package MongoDB.Driver -v {VERSION}
    ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="3494a-172">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3494a-172">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="3494a-173">Git **dosya** > **yeni çözüm** >  **.NET Core** > **uygulama**.</span><span class="sxs-lookup"><span data-stu-id="3494a-173">Go to **File** > **New Solution** > **.NET Core** > **App**.</span></span>
1. <span data-ttu-id="3494a-174">Seçin **ASP.NET Core Web API'si** C# proje şablonu, belirleyin **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="3494a-174">Select the **ASP.NET Core Web API** C# project template, and select **Next**.</span></span>
1. <span data-ttu-id="3494a-175">Seçin **.NET Core 2.2** gelen **hedef Framework'ü** seçin ve açılır listede **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="3494a-175">Select **.NET Core 2.2** from the **Target Framework** drop-down list, and select **Next**.</span></span>
1. <span data-ttu-id="3494a-176">Girin *BooksApi* için **proje adı**seçip **Oluştur**.</span><span class="sxs-lookup"><span data-stu-id="3494a-176">Enter *BooksApi* for the **Project Name**, and select **Create**.</span></span>
1. <span data-ttu-id="3494a-177">İçinde **çözüm** paneli, projenin sağ **bağımlılıkları** düğümünü seçip alt **paketleri Ekle**.</span><span class="sxs-lookup"><span data-stu-id="3494a-177">In the **Solution** pad, right-click the project's **Dependencies** node and select **Add Packages**.</span></span>
1. <span data-ttu-id="3494a-178">Girin *MongoDB.Driver* arama kutusunda *MongoDB.Driver* paketini bulun ve seçin **Paketi Ekle**.</span><span class="sxs-lookup"><span data-stu-id="3494a-178">Enter *MongoDB.Driver* in the search box, select the *MongoDB.Driver* package, and select **Add Package**.</span></span>
1. <span data-ttu-id="3494a-179">Seçin **kabul** düğmesine **lisans kabulü** iletişim.</span><span class="sxs-lookup"><span data-stu-id="3494a-179">Select the **Accept** button in the **License Acceptance** dialog.</span></span>

---

## <a name="add-an-entity-model"></a><span data-ttu-id="3494a-180">Varlık modeli ekleme</span><span class="sxs-lookup"><span data-stu-id="3494a-180">Add an entity model</span></span>

1. <span data-ttu-id="3494a-181">Ekleme bir *modelleri* proje kök dizini.</span><span class="sxs-lookup"><span data-stu-id="3494a-181">Add a *Models* directory to the project root.</span></span>
1. <span data-ttu-id="3494a-182">Ekleme bir `Book` sınıfının *modelleri* aşağıdaki kod ile dizin:</span><span class="sxs-lookup"><span data-stu-id="3494a-182">Add a `Book` class to the *Models* directory with the following code:</span></span>

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

    <span data-ttu-id="3494a-183">Önceki sınıfında `Id` özelliği:</span><span class="sxs-lookup"><span data-stu-id="3494a-183">In the preceding class, the `Id` property:</span></span>
    
    * <span data-ttu-id="3494a-184">Ortak dil çalışma zamanı (CLR) nesnesi için MongoDB koleksiyonu eşlemek için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="3494a-184">Is required for mapping the Common Language Runtime (CLR) object to the MongoDB collection.</span></span>
    * <span data-ttu-id="3494a-185">İle açıklanıyor [[BsonId]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonIdAttribute.htm) belgenin birincil anahtarı olarak bu özellik belirlemek için.</span><span class="sxs-lookup"><span data-stu-id="3494a-185">Is annotated with [[BsonId]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonIdAttribute.htm) to designate this property as the document's primary key.</span></span>
    * <span data-ttu-id="3494a-186">İle açıklanıyor [[BsonRepresentation(BsonType.ObjectId)]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonRepresentationAttribute.htm) parametre türü olarak geçirerek izin vermek için `string` yerine bir [objectID](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_ObjectId.htm) yapısı.</span><span class="sxs-lookup"><span data-stu-id="3494a-186">Is annotated with [[BsonRepresentation(BsonType.ObjectId)]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonRepresentationAttribute.htm) to allow passing the parameter as type `string` instead of an [ObjectId](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_ObjectId.htm) structure.</span></span> <span data-ttu-id="3494a-187">Mongo işleme dönüştürme `string` için `ObjectId`.</span><span class="sxs-lookup"><span data-stu-id="3494a-187">Mongo handles the conversion from `string` to `ObjectId`.</span></span>
    
    <span data-ttu-id="3494a-188">`BookName` Özelliği ile ek açıklamalı [[BsonElement]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonElementAttribute.htm) özniteliği.</span><span class="sxs-lookup"><span data-stu-id="3494a-188">The `BookName` property is annotated with the [[BsonElement]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonElementAttribute.htm) attribute.</span></span> <span data-ttu-id="3494a-189">Özniteliğin değerini `Name` özellik adında MongoDB koleksiyonu temsil eder.</span><span class="sxs-lookup"><span data-stu-id="3494a-189">The attribute's value of `Name` represents the property name in the MongoDB collection.</span></span>

## <a name="add-a-configuration-model"></a><span data-ttu-id="3494a-190">Yapılandırma modeli ekleme</span><span class="sxs-lookup"><span data-stu-id="3494a-190">Add a configuration model</span></span>

1. <span data-ttu-id="3494a-191">Aşağıdaki veritabanı yapılandırma değerlerini eklemek *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="3494a-191">Add the following database configuration values to *appsettings.json*:</span></span>

    [!code-json[](first-mongo-app/sample/BooksApi/appsettings.json?highlight=2-6)]

1. <span data-ttu-id="3494a-192">Ekleme bir *BookstoreDatabaseSettings.cs* dosyasını *modelleri* aşağıdaki kod ile dizin:</span><span class="sxs-lookup"><span data-stu-id="3494a-192">Add a *BookstoreDatabaseSettings.cs* file to the *Models* directory with the following code:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Models/BookstoreDatabaseSettings.cs)]

    <span data-ttu-id="3494a-193">Önceki `BookstoreDatabaseSettings` depolamak için kullanılan sınıf *appsettings.json* dosyanın `BookstoreDatabaseSettings` özellik değerleri.</span><span class="sxs-lookup"><span data-stu-id="3494a-193">The preceding `BookstoreDatabaseSettings` class is used to store the *appsettings.json* file's `BookstoreDatabaseSettings` property values.</span></span> <span data-ttu-id="3494a-194">JSON ve C# özellik adları adlı aynı şekilde eşleme işlemini kolaylaştırmak için.</span><span class="sxs-lookup"><span data-stu-id="3494a-194">The JSON and C# property names are named identically to ease the mapping process.</span></span>

1. <span data-ttu-id="3494a-195">Aşağıdaki vurgulanmış kodu ekleyin `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="3494a-195">Add the following highlighted code to `Startup.ConfigureServices`:</span></span>

    [!code-csharp[](first-mongo-app/sample_snapshot/BooksApi/Startup.ConfigureServices.AddDbSettings.cs?highlight=3-7)]

    <span data-ttu-id="3494a-196">Yukarıdaki kodda:</span><span class="sxs-lookup"><span data-stu-id="3494a-196">In the preceding code:</span></span>

    * <span data-ttu-id="3494a-197">Yapılandırma örneğine *appsettings.json* dosyanın `BookstoreDatabaseSettings` bölümüne bağlar, bağımlılık ekleme (dı) kapsayıcısında kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="3494a-197">The configuration instance to which the *appsettings.json* file's `BookstoreDatabaseSettings` section binds is registered in the Dependency Injection (DI) container.</span></span> <span data-ttu-id="3494a-198">Örneğin, bir `BookstoreDatabaseSettings` nesnenin `ConnectionString` özelliği ile doldurulur `BookstoreDatabaseSettings:ConnectionString` özelliğinde *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="3494a-198">For example, a `BookstoreDatabaseSettings` object's `ConnectionString` property is populated with the `BookstoreDatabaseSettings:ConnectionString` property in *appsettings.json*.</span></span>
    * <span data-ttu-id="3494a-199">`IBookstoreDatabaseSettings` Arabirimi ile tek DI kayıtlı [hizmet ömrü](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="3494a-199">The `IBookstoreDatabaseSettings` interface is registered in DI with a singleton [service lifetime](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="3494a-200">Arabirim örneğinin eklenen, çözümler bir `BookstoreDatabaseSettings` nesne.</span><span class="sxs-lookup"><span data-stu-id="3494a-200">When injected, the interface instance resolves to a `BookstoreDatabaseSettings` object.</span></span>

1. <span data-ttu-id="3494a-201">Üstüne aşağıdaki kodu ekleyin *Startup.cs* çözümlenecek `BookstoreDatabaseSettings` ve `IBookstoreDatabaseSettings` başvuruları:</span><span class="sxs-lookup"><span data-stu-id="3494a-201">Add the following code to the top of *Startup.cs* to resolve the `BookstoreDatabaseSettings` and `IBookstoreDatabaseSettings` references:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Startup.cs?name=snippet_UsingBooksApiModels)]

## <a name="add-a-crud-operations-service"></a><span data-ttu-id="3494a-202">CRUD işlemleri hizmet ekleme</span><span class="sxs-lookup"><span data-stu-id="3494a-202">Add a CRUD operations service</span></span>

1. <span data-ttu-id="3494a-203">Ekleme bir *Hizmetleri* proje kök dizini.</span><span class="sxs-lookup"><span data-stu-id="3494a-203">Add a *Services* directory to the project root.</span></span>
1. <span data-ttu-id="3494a-204">Ekleme bir `BookService` sınıfının *Hizmetleri* aşağıdaki kod ile dizin:</span><span class="sxs-lookup"><span data-stu-id="3494a-204">Add a `BookService` class to the *Services* directory with the following code:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Services/BookService.cs?name=snippet_BookServiceClass)]

    <span data-ttu-id="3494a-205">Önceki kodda, bir `IBookstoreDatabaseSettings` Oluşturucu ekleme örneği DI alınır.</span><span class="sxs-lookup"><span data-stu-id="3494a-205">In the preceding code, an `IBookstoreDatabaseSettings` instance is retrieved from DI via constructor injection.</span></span> <span data-ttu-id="3494a-206">Bu tekniği erişim sağlayan *appsettings.json* eklenmiştir yapılandırma değerlerini [yapılandırma modeli ekleme](#add-a-configuration-model) bölümü.</span><span class="sxs-lookup"><span data-stu-id="3494a-206">This technique provides access to the *appsettings.json* configuration values that were added in the [Add a configuration model](#add-a-configuration-model) section.</span></span>

1. <span data-ttu-id="3494a-207">Aşağıdaki vurgulanmış kodu ekleyin `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="3494a-207">Add the following highlighted code to `Startup.ConfigureServices`:</span></span>

    [!code-csharp[](first-mongo-app/sample_snapshot/BooksApi/Startup.ConfigureServices.AddSingletonService.cs?highlight=9)]

    <span data-ttu-id="3494a-208">Önceki kodda, `BookService` sınıfı Oluşturucu ekleme sınıfları tüketen desteklemek için DI ile kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="3494a-208">In the preceding code, the `BookService` class is registered with DI to support constructor injection in consuming classes.</span></span> <span data-ttu-id="3494a-209">Singleton hizmet ömrü uygundur çünkü `BookService` doğrudan bağımlılık vereceğine `MongoClient`.</span><span class="sxs-lookup"><span data-stu-id="3494a-209">The singleton service lifetime is most appropriate because `BookService` takes a direct dependency on `MongoClient`.</span></span> <span data-ttu-id="3494a-210">Resmi başına [Mongo istemci yeniden yönergeleri](https://mongodb.github.io/mongo-csharp-driver/2.8/reference/driver/connecting/#re-use), `MongoClient` DI tekil hizmet ömrü ile kayıtlı olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="3494a-210">Per the official [Mongo Client reuse guidelines](https://mongodb.github.io/mongo-csharp-driver/2.8/reference/driver/connecting/#re-use), `MongoClient` should be registered in DI with a singleton service lifetime.</span></span>

1. <span data-ttu-id="3494a-211">Üstüne aşağıdaki kodu ekleyin *Startup.cs* çözümlenecek `BookService` başvurusu:</span><span class="sxs-lookup"><span data-stu-id="3494a-211">Add the following code to the top of *Startup.cs* to resolve the `BookService` reference:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Startup.cs?name=snippet_UsingBooksApiServices)]

<span data-ttu-id="3494a-212">`BookService` Sınıfını kullanan aşağıdaki `MongoDB.Driver` veritabanında CRUD işlemleri gerçekleştirmek için üyeleri:</span><span class="sxs-lookup"><span data-stu-id="3494a-212">The `BookService` class uses the following `MongoDB.Driver` members to perform CRUD operations against the database:</span></span>

* <span data-ttu-id="3494a-213">[MongoClient](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoClient.htm) &ndash; veritabanı işlemleri gerçekleştirmek için kullanılan bir sunucuyu okur.</span><span class="sxs-lookup"><span data-stu-id="3494a-213">[MongoClient](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoClient.htm) &ndash; Reads the server instance for performing database operations.</span></span> <span data-ttu-id="3494a-214">Bu sınıfın oluşturucusu, MongoDB bağlantı dizesini sağlanır:</span><span class="sxs-lookup"><span data-stu-id="3494a-214">The constructor of this class is provided the MongoDB connection string:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Services/BookService.cs?name=snippet_BookServiceConstructor&highlight=3)]

* <span data-ttu-id="3494a-215">[IMongoDatabase](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_IMongoDatabase.htm) &ndash; işlemleri gerçekleştirmek için kullanılan Mongo veritabanını temsil eder.</span><span class="sxs-lookup"><span data-stu-id="3494a-215">[IMongoDatabase](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_IMongoDatabase.htm) &ndash; Represents the Mongo database for performing operations.</span></span> <span data-ttu-id="3494a-216">Bu öğreticide genel [belirtilmiş<TDocument>(koleksiyon)](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoDatabase_GetCollection__1.htm) yöntemi belirli bir koleksiyondaki verileri erişim elde etmek için arabirim.</span><span class="sxs-lookup"><span data-stu-id="3494a-216">This tutorial uses the generic [GetCollection<TDocument>(collection)](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoDatabase_GetCollection__1.htm) method on the interface to gain access to data in a specific collection.</span></span> <span data-ttu-id="3494a-217">Bu yöntemi çağrıldıktan sonra koleksiyonu karşı bir CRUD işlemleri gerçekleştirin.</span><span class="sxs-lookup"><span data-stu-id="3494a-217">Perform CRUD operations against the collection after this method is called.</span></span> <span data-ttu-id="3494a-218">İçinde `GetCollection<TDocument>(collection)` yöntem çağrısı:</span><span class="sxs-lookup"><span data-stu-id="3494a-218">In the `GetCollection<TDocument>(collection)` method call:</span></span>
  * <span data-ttu-id="3494a-219">`collection` Koleksiyon adını temsil eder.</span><span class="sxs-lookup"><span data-stu-id="3494a-219">`collection` represents the collection name.</span></span>
  * <span data-ttu-id="3494a-220">`TDocument` Bir koleksiyonda depolanan CLR nesne türünü temsil eder.</span><span class="sxs-lookup"><span data-stu-id="3494a-220">`TDocument` represents the CLR object type stored in the collection.</span></span>

<span data-ttu-id="3494a-221">`GetCollection<TDocument>(collection)` döndürür bir [MongoCollection](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoCollection.htm) koleksiyonu temsil eden nesne.</span><span class="sxs-lookup"><span data-stu-id="3494a-221">`GetCollection<TDocument>(collection)` returns a [MongoCollection](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoCollection.htm) object representing the collection.</span></span> <span data-ttu-id="3494a-222">Bu öğreticide, aşağıdaki yöntemlerden koleksiyonunda çağrılır:</span><span class="sxs-lookup"><span data-stu-id="3494a-222">In this tutorial, the following methods are invoked on the collection:</span></span>

* <span data-ttu-id="3494a-223">[DeleteOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_DeleteOne.htm) &ndash; sağlanan arama ölçütleriyle eşleşen tek bir belge siler.</span><span class="sxs-lookup"><span data-stu-id="3494a-223">[DeleteOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_DeleteOne.htm) &ndash; Deletes a single document matching the provided search criteria.</span></span>
* <span data-ttu-id="3494a-224">[Bulma\<TDocument >](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollectionExtensions_Find__1_1.htm) &ndash; sağlanan arama ölçütleriyle eşleşen koleksiyondaki tüm belgeleri döndürür.</span><span class="sxs-lookup"><span data-stu-id="3494a-224">[Find\<TDocument>](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollectionExtensions_Find__1_1.htm) &ndash; Returns all documents in the collection matching the provided search criteria.</span></span>
* <span data-ttu-id="3494a-225">[InsertOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_InsertOne.htm) &ndash; belirtilen nesne koleksiyonunda yeni bir belge olarak ekler.</span><span class="sxs-lookup"><span data-stu-id="3494a-225">[InsertOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_InsertOne.htm) &ndash; Inserts the provided object as a new document in the collection.</span></span>
* <span data-ttu-id="3494a-226">[ReplaceOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_ReplaceOne.htm) &ndash; sağlanan nesne ile sağlanan arama ölçütleriyle eşleşen tek bir belge değiştirir.</span><span class="sxs-lookup"><span data-stu-id="3494a-226">[ReplaceOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_ReplaceOne.htm) &ndash; Replaces the single document matching the provided search criteria with the provided object.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="3494a-227">Denetleyici ekleme</span><span class="sxs-lookup"><span data-stu-id="3494a-227">Add a controller</span></span>

<span data-ttu-id="3494a-228">Ekleme bir `BooksController` sınıfının *denetleyicileri* aşağıdaki kod ile dizin:</span><span class="sxs-lookup"><span data-stu-id="3494a-228">Add a `BooksController` class to the *Controllers* directory with the following code:</span></span>

[!code-csharp[](first-mongo-app/sample/BooksApi/Controllers/BooksController.cs)]

<span data-ttu-id="3494a-229">Önceki web API denetleyicisi:</span><span class="sxs-lookup"><span data-stu-id="3494a-229">The preceding web API controller:</span></span>

* <span data-ttu-id="3494a-230">Kullanan `BookService` CRUD işlemleri gerçekleştirmek için sınıf.</span><span class="sxs-lookup"><span data-stu-id="3494a-230">Uses the `BookService` class to perform CRUD operations.</span></span>
* <span data-ttu-id="3494a-231">GET, POST, PUT ve DELETE HTTP isteklerini desteklemek için eylem yöntemleri içerir.</span><span class="sxs-lookup"><span data-stu-id="3494a-231">Contains action methods to support GET, POST, PUT, and DELETE HTTP requests.</span></span>
* <span data-ttu-id="3494a-232">Çağrıları <xref:System.Web.Http.ApiController.CreatedAtRoute*> içinde `Create` döndürülecek eylem yöntemi bir [HTTP 201](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) yanıt.</span><span class="sxs-lookup"><span data-stu-id="3494a-232">Calls <xref:System.Web.Http.ApiController.CreatedAtRoute*> in the `Create` action method to return an [HTTP 201](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) response.</span></span> <span data-ttu-id="3494a-233">Durum kodu 201 sunucuda yeni bir kaynak oluşturan bir HTTP POST yöntemi için standart yanıttır.</span><span class="sxs-lookup"><span data-stu-id="3494a-233">Status code 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span> <span data-ttu-id="3494a-234">`CreatedAtRoute` Ayrıca ekler bir `Location` yanıt üst bilgisi.</span><span class="sxs-lookup"><span data-stu-id="3494a-234">`CreatedAtRoute` also adds a `Location` header to the response.</span></span> <span data-ttu-id="3494a-235">`Location` Üst bilgisi, yeni oluşturulan kitap URI'sini belirtir.</span><span class="sxs-lookup"><span data-stu-id="3494a-235">The `Location` header specifies the URI of the newly created book.</span></span>

## <a name="test-the-web-api"></a><span data-ttu-id="3494a-236">Web API'sini test etme</span><span class="sxs-lookup"><span data-stu-id="3494a-236">Test the web API</span></span>

1. <span data-ttu-id="3494a-237">Uygulamayı derleyin ve çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="3494a-237">Build and run the app.</span></span>

1. <span data-ttu-id="3494a-238">Gidin `http://localhost:<port>/api/books` test denetleyicisi için parametresiz `Get` eylem yöntemi.</span><span class="sxs-lookup"><span data-stu-id="3494a-238">Navigate to `http://localhost:<port>/api/books` to test the controller's parameterless `Get` action method.</span></span> <span data-ttu-id="3494a-239">Aşağıdaki JSON yanıtı gösterilir:</span><span class="sxs-lookup"><span data-stu-id="3494a-239">The following JSON response is displayed:</span></span>

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

1. <span data-ttu-id="3494a-240">Gidin `http://localhost:<port>/api/books/5bfd996f7b8e48dc15ff215e` test denetleyicisi için aşırı yüklenmiş `Get` eylem yöntemi.</span><span class="sxs-lookup"><span data-stu-id="3494a-240">Navigate to `http://localhost:<port>/api/books/5bfd996f7b8e48dc15ff215e` to test the controller's overloaded `Get` action method.</span></span> <span data-ttu-id="3494a-241">Aşağıdaki JSON yanıtı gösterilir:</span><span class="sxs-lookup"><span data-stu-id="3494a-241">The following JSON response is displayed:</span></span>

    ```json
    {
      "id":"5bfd996f7b8e48dc15ff215e",
      "bookName":"Clean Code",
      "price":43.15,
      "category":"Computers",
      "author":"Robert C. Martin"
    }
    ```

## <a name="configure-json-serialization-options"></a><span data-ttu-id="3494a-242">JSON seri hale getirme seçenekleri yapılandırın</span><span class="sxs-lookup"><span data-stu-id="3494a-242">Configure JSON serialization options</span></span>

<span data-ttu-id="3494a-243">Döndürülen JSON yanıtları değiştirmek için iki ayrıntı [web API'si Test](#test-the-web-api) bölümü:</span><span class="sxs-lookup"><span data-stu-id="3494a-243">There are two details to change about the JSON responses returned in the [Test the web API](#test-the-web-api) section:</span></span>

* <span data-ttu-id="3494a-244">Özellik adlarını ortası büyük olan varsayılan büyük/küçük harf Pascal eşleşecek şekilde değiştirilmesi gereken CLR nesnenin özellik adları büyük/küçük harfleri.</span><span class="sxs-lookup"><span data-stu-id="3494a-244">The property names' default camel casing should be changed to match the Pascal casing of the CLR object's property names.</span></span>
* <span data-ttu-id="3494a-245">`bookName` Özelliği olarak döndürülmesi `Name`.</span><span class="sxs-lookup"><span data-stu-id="3494a-245">The `bookName` property should be returned as `Name`.</span></span>

<span data-ttu-id="3494a-246">Yukarıdaki gereksinimleri karşılamak için aşağıdaki değişiklikleri yapın:</span><span class="sxs-lookup"><span data-stu-id="3494a-246">To satisfy the preceding requirements, make the following changes:</span></span>

1. <span data-ttu-id="3494a-247">İçinde `Startup.ConfigureServices`, aşağıdaki vurgulanmış kodu için zincir `AddMvc` yöntem çağrısı:</span><span class="sxs-lookup"><span data-stu-id="3494a-247">In `Startup.ConfigureServices`, chain the following highlighted code on to the `AddMvc` method call:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Startup.cs?name=snippet_ConfigureServices&highlight=12)]

    <span data-ttu-id="3494a-248">Önceki değişiklikle, web API'SİNİN özellik adlarını JSON yanıtı eşleşme karşılık gelen özellik adlarını CLR nesne türü seri hale.</span><span class="sxs-lookup"><span data-stu-id="3494a-248">With the preceding change, property names in the web API's serialized JSON response match their corresponding property names in the CLR object type.</span></span> <span data-ttu-id="3494a-249">Örneğin, `Book` sınıfın `Author` serileştiren özelliğini olarak `Author`.</span><span class="sxs-lookup"><span data-stu-id="3494a-249">For example, the `Book` class's `Author` property serializes as `Author`.</span></span>

1. <span data-ttu-id="3494a-250">İçinde *Models/Book.cs*, açıklama `BookName` aşağıdaki özellik [[Item]](https://www.newtonsoft.com/json/help/html/T_Newtonsoft_Json_JsonPropertyAttribute.htm) özniteliği:</span><span class="sxs-lookup"><span data-stu-id="3494a-250">In *Models/Book.cs*, annotate the `BookName` property with the following [[JsonProperty]](https://www.newtonsoft.com/json/help/html/T_Newtonsoft_Json_JsonPropertyAttribute.htm) attribute:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Models/Book.cs?name=snippet_BookNameProperty&highlight=2)]

    <span data-ttu-id="3494a-251">`[JsonProperty]` Özniteliğin değerini `Name` seri hale getirilmiş JSON yanıtı web API'SİNİN name özelliği temsil eder.</span><span class="sxs-lookup"><span data-stu-id="3494a-251">The `[JsonProperty]` attribute's value of `Name` represents the property name in the web API's serialized JSON response.</span></span>

1. <span data-ttu-id="3494a-252">Üstüne aşağıdaki kodu ekleyin *Models/Book.cs* çözümlenecek `[JsonProperty]` öznitelik başvurusu:</span><span class="sxs-lookup"><span data-stu-id="3494a-252">Add the following code to the top of *Models/Book.cs* to resolve the `[JsonProperty]` attribute reference:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Models/Book.cs?name=snippet_NewtonsoftJsonImport)]

1. <span data-ttu-id="3494a-253">Tanımlı adımları yineleyin [web API'si Test](#test-the-web-api) bölümü.</span><span class="sxs-lookup"><span data-stu-id="3494a-253">Repeat the steps defined in the [Test the web API](#test-the-web-api) section.</span></span> <span data-ttu-id="3494a-254">JSON özellik adları fark dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="3494a-254">Notice the difference in JSON property names.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3494a-255">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="3494a-255">Next steps</span></span>

<span data-ttu-id="3494a-256">ASP.NET Core web API'leri oluşturmaya daha fazla bilgi için aşağıdaki kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="3494a-256">For more information on building ASP.NET Core web APIs, see the following resources:</span></span>

* [<span data-ttu-id="3494a-257">Bu makalenin YouTube sürümü</span><span class="sxs-lookup"><span data-stu-id="3494a-257">YouTube version of this article</span></span>](https://www.youtube.com/watch?v=7uJt_sOenyo&feature=youtu.be)
* <xref:web-api/index>
* <xref:web-api/action-return-types>
