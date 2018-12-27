---
title: MongoDB ile ASP.NET Core ile Web API oluşturma
author: prkhandelwal
description: Bu öğreticide bir ASP.NET Core web API'si kullanarak bir MongoDB NoSQL veritabanı oluşturma gösterilmektedir.
ms.author: scaddie
ms.custom: mvc, seodec18
ms.date: 11/29/2018
uid: tutorials/first-mongo-app
ms.openlocfilehash: bd9a36c5eb06542c820e71e937b8da10f735a0f8
ms.sourcegitcommit: 68a3081dd175d6518d1bfa31b4712bd8a2dd3864
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2018
ms.locfileid: "53577844"
---
# <a name="create-a-web-api-with-aspnet-core-and-mongodb"></a><span data-ttu-id="7c76b-103">MongoDB ile ASP.NET Core ile web API'si oluşturma</span><span class="sxs-lookup"><span data-stu-id="7c76b-103">Create a web API with ASP.NET Core and MongoDB</span></span>

<span data-ttu-id="7c76b-104">Tarafından [Pratik Khandelwal](https://twitter.com/K2Prk) ve [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="7c76b-104">By [Pratik Khandelwal](https://twitter.com/K2Prk) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="7c76b-105">Bu öğreticide web API'si temel oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemleri gerçekleştiren oluşturur bir [MongoDB](https://www.mongodb.com/what-is-mongodb) NoSQL veritabanı.</span><span class="sxs-lookup"><span data-stu-id="7c76b-105">This tutorial creates a web API that performs Create, Read, Update, and Delete (CRUD) operations on a [MongoDB](https://www.mongodb.com/what-is-mongodb) NoSQL database.</span></span>

<span data-ttu-id="7c76b-106">Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:</span><span class="sxs-lookup"><span data-stu-id="7c76b-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="7c76b-107">MongoDB yapılandırın</span><span class="sxs-lookup"><span data-stu-id="7c76b-107">Configure MongoDB</span></span>
> * <span data-ttu-id="7c76b-108">MongoDB veritabanı oluşturma</span><span class="sxs-lookup"><span data-stu-id="7c76b-108">Create a MongoDB database</span></span>
> * <span data-ttu-id="7c76b-109">MongoDB koleksiyonu ve şema tanımlayın</span><span class="sxs-lookup"><span data-stu-id="7c76b-109">Define a MongoDB collection and schema</span></span>
> * <span data-ttu-id="7c76b-110">Bir web API'sini MongoDB CRUD işlemleri gerçekleştirme</span><span class="sxs-lookup"><span data-stu-id="7c76b-110">Perform MongoDB CRUD operations from a web API</span></span>

<span data-ttu-id="7c76b-111">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-mongo-app/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7c76b-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-mongo-app/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7c76b-112">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="7c76b-112">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7c76b-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7c76b-113">Visual Studio</span></span>](#tab/visual-studio)

* [<span data-ttu-id="7c76b-114">.NET core SDK 2.2 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="7c76b-114">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="7c76b-115">[Visual Studio 2017 sürüm 15,9 veya üzeri](https://www.visualstudio.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) ile **ASP.NET ve web geliştirme** iş yükü</span><span class="sxs-lookup"><span data-stu-id="7c76b-115">[Visual Studio 2017 version 15.9 or later](https://www.visualstudio.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="7c76b-116">MongoDB</span><span class="sxs-lookup"><span data-stu-id="7c76b-116">MongoDB</span></span>](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="7c76b-117">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7c76b-117">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="7c76b-118">.NET core SDK 2.2 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="7c76b-118">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="7c76b-119">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7c76b-119">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="7c76b-120">Visual Studio Code için C#</span><span class="sxs-lookup"><span data-stu-id="7c76b-120">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [<span data-ttu-id="7c76b-121">MongoDB</span><span class="sxs-lookup"><span data-stu-id="7c76b-121">MongoDB</span></span>](https://docs.mongodb.com/manual/administration/install-community/)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="7c76b-122">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7c76b-122">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* [<span data-ttu-id="7c76b-123">.NET core SDK 2.2 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="7c76b-123">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="7c76b-124">Mac 7,7 veya sonraki bir sürümü için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7c76b-124">Visual Studio for Mac version 7.7 or later</span></span>](https://www.visualstudio.com/downloads/)
* [<span data-ttu-id="7c76b-125">MongoDB</span><span class="sxs-lookup"><span data-stu-id="7c76b-125">MongoDB</span></span>](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-os-x/)

---

## <a name="configure-mongodb"></a><span data-ttu-id="7c76b-126">MongoDB yapılandırın</span><span class="sxs-lookup"><span data-stu-id="7c76b-126">Configure MongoDB</span></span>

<span data-ttu-id="7c76b-127">Windows kullanıyorsanız, MongoDB yüklü *C:\Program Files\MongoDB* varsayılan olarak.</span><span class="sxs-lookup"><span data-stu-id="7c76b-127">If using Windows, MongoDB is installed at *C:\Program Files\MongoDB* by default.</span></span> <span data-ttu-id="7c76b-128">Ekleme *C:\Program Files\MongoDB\Server\<version_number > \bin* için `Path` ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="7c76b-128">Add *C:\Program Files\MongoDB\Server\<version_number>\bin* to the `Path` environment variable.</span></span> <span data-ttu-id="7c76b-129">Bu değişiklik yerden MongoDB erişim sağlar, geliştirme makinenizde.</span><span class="sxs-lookup"><span data-stu-id="7c76b-129">This change enables MongoDB access from anywhere on your development machine.</span></span>

<span data-ttu-id="7c76b-130">Mongo kabuğunu veritabanı oluşturma, koleksiyonları yapın ve belgeleri depolamak için aşağıdaki adımları kullanın.</span><span class="sxs-lookup"><span data-stu-id="7c76b-130">Use the mongo Shell in the following steps to create a database, make collections, and store documents.</span></span> <span data-ttu-id="7c76b-131">Mongo Kabuğu komutları hakkında daha fazla bilgi için bkz. [mongo kabuğunu çalışma](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell).</span><span class="sxs-lookup"><span data-stu-id="7c76b-131">For more information on mongo Shell commands, see [Working with the mongo Shell](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell).</span></span>

1. <span data-ttu-id="7c76b-132">Geliştirme makinenizde verilerin depolanması için bir dizin seçin.</span><span class="sxs-lookup"><span data-stu-id="7c76b-132">Choose a directory on your development machine for storing the data.</span></span> <span data-ttu-id="7c76b-133">Örneğin, *C:\BooksData* Windows üzerinde.</span><span class="sxs-lookup"><span data-stu-id="7c76b-133">For example, *C:\BooksData* on Windows.</span></span> <span data-ttu-id="7c76b-134">Yoksa dizini oluşturun.</span><span class="sxs-lookup"><span data-stu-id="7c76b-134">Create the directory if it doesn't exist.</span></span> <span data-ttu-id="7c76b-135">Mongo kabuğunu yeni dizinleri oluşturmaz.</span><span class="sxs-lookup"><span data-stu-id="7c76b-135">The mongo Shell doesn't create new directories.</span></span>
1. <span data-ttu-id="7c76b-136">Bir komut kabuğunu açın.</span><span class="sxs-lookup"><span data-stu-id="7c76b-136">Open a command shell.</span></span> <span data-ttu-id="7c76b-137">Varsayılan bağlantı noktası 27017 mongodb'ye bağlanmak için aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="7c76b-137">Run the following command to connect to MongoDB on default port 27017.</span></span> <span data-ttu-id="7c76b-138">Değiştirmeyi unutmayın `<data_directory_path>` önceki adımda seçtiğiniz dizini.</span><span class="sxs-lookup"><span data-stu-id="7c76b-138">Remember to replace `<data_directory_path>` with the directory you chose in the previous step.</span></span>

    ```console
    mongod --dbpath <data_directory_path>
    ```

1. <span data-ttu-id="7c76b-139">Başka bir komut kabuğu örneği açın.</span><span class="sxs-lookup"><span data-stu-id="7c76b-139">Open another command shell instance.</span></span> <span data-ttu-id="7c76b-140">Aşağıdaki komutu çalıştırarak varsayılan test veritabanı'na bağlanma:</span><span class="sxs-lookup"><span data-stu-id="7c76b-140">Connect to the default test database by running the following command:</span></span>

    ```console
    mongo
    ```

1. <span data-ttu-id="7c76b-141">Bir komut kabuğu'nda aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="7c76b-141">Run the following in a command shell:</span></span>

    ```console
    use BookstoreDb
    ```

    <span data-ttu-id="7c76b-142">Adlı bir veritabanı zaten mevcut olmayan halinde *BookstoreDb* oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="7c76b-142">If it doesn't already exist, a database named *BookstoreDb* is created.</span></span> <span data-ttu-id="7c76b-143">Veritabanı mevcut değilse, bağlantı işlemleri için açılır.</span><span class="sxs-lookup"><span data-stu-id="7c76b-143">If the database does exist, its connection is opened for transactions.</span></span>

1. <span data-ttu-id="7c76b-144">Oluşturma bir `Books` koleksiyon aşağıdaki komutu kullanarak:</span><span class="sxs-lookup"><span data-stu-id="7c76b-144">Create a `Books` collection using following command:</span></span>

    ```console
    db.createCollection('Books')
    ```

    <span data-ttu-id="7c76b-145">Aşağıdaki sonucu görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="7c76b-145">The following result is displayed:</span></span>

    ```console
    { "ok" : 1 }
    ```

1. <span data-ttu-id="7c76b-146">İçin bir şema tanımlayabilir `Books` toplama ve ekleme iki belge aşağıdaki komutu kullanarak:</span><span class="sxs-lookup"><span data-stu-id="7c76b-146">Define a schema for the `Books` collection and insert two documents using the following command:</span></span>

    ```console
    db.Books.insertMany([{'Name':'Design Patterns','Price':54.93,'Category':'Computers','Author':'Ralph Johnson'}, {'Name':'Clean Code','Price':43.15,'Category':'Computers','Author':'Robert C. Martin'}])
    ```

    <span data-ttu-id="7c76b-147">Aşağıdaki sonucu görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="7c76b-147">The following result is displayed:</span></span>

    ```console
    {
      "acknowledged" : true,
      "insertedIds" : [
        ObjectId("5bfd996f7b8e48dc15ff215d"),
        ObjectId("5bfd996f7b8e48dc15ff215e")
      ]
    }
    ```

1. <span data-ttu-id="7c76b-148">Aşağıdaki komutu kullanarak veritabanında belgelerini görüntüleyin:</span><span class="sxs-lookup"><span data-stu-id="7c76b-148">View the documents in the database using the following command:</span></span>

    ```console
    db.Books.find({}).pretty()
    ```

    <span data-ttu-id="7c76b-149">Aşağıdaki sonucu görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="7c76b-149">The following result is displayed:</span></span>

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

    <span data-ttu-id="7c76b-150">Şema girmiş ekler `_id` türünün özelliği `ObjectId` her belge için.</span><span class="sxs-lookup"><span data-stu-id="7c76b-150">The schema adds an autogenerated `_id` property of type `ObjectId` for each document.</span></span>

<span data-ttu-id="7c76b-151">Veritabanı hazırdır.</span><span class="sxs-lookup"><span data-stu-id="7c76b-151">The database is ready.</span></span> <span data-ttu-id="7c76b-152">ASP.NET Core web API'si oluşturmaya başlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7c76b-152">You can start creating the ASP.NET Core web API.</span></span>

## <a name="create-the-aspnet-core-web-api-project"></a><span data-ttu-id="7c76b-153">ASP.NET Core web API projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="7c76b-153">Create the ASP.NET Core web API project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7c76b-154">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7c76b-154">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="7c76b-155">Git **dosya** > **yeni** > **proje**.</span><span class="sxs-lookup"><span data-stu-id="7c76b-155">Go to **File** > **New** > **Project**.</span></span>
1. <span data-ttu-id="7c76b-156">Seçin **ASP.NET Core Web uygulaması**, projeyi adlandırın *BooksApi*, tıklatıp **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="7c76b-156">Select **ASP.NET Core Web Application**, name the project *BooksApi*, and click **OK**.</span></span>
1. <span data-ttu-id="7c76b-157">Seçin **.NET Core** hedef çerçeve ve **ASP.NET Core 2.1**.</span><span class="sxs-lookup"><span data-stu-id="7c76b-157">Select the **.NET Core** target framework and **ASP.NET Core 2.1**.</span></span> <span data-ttu-id="7c76b-158">Seçin **API** proje şablonu ve tıklayın **Tamam**:</span><span class="sxs-lookup"><span data-stu-id="7c76b-158">Select the **API** project template, and click **OK**:</span></span>
1. <span data-ttu-id="7c76b-159">İçinde **Paket Yöneticisi Konsolu** penceresinde proje kök dizinine gidin.</span><span class="sxs-lookup"><span data-stu-id="7c76b-159">In the **Package Manager Console** window, navigate to the project root.</span></span> <span data-ttu-id="7c76b-160">MongoDB için .NET sürücüsünü yüklemek için aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="7c76b-160">Run the following command to install the .NET driver for MongoDB:</span></span>

    ```powershell
    Install-Package MongoDB.Driver -Version 2.7.2
    ```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="7c76b-161">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7c76b-161">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="7c76b-162">Bir komut kabuğu'nda aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="7c76b-162">Run the following commands in a command shell:</span></span>

    ```console
    dotnet new webapi -o BooksApi
    code BooksApi
    ```

    <span data-ttu-id="7c76b-163">.NET Core'u hedefleyen yeni bir ASP.NET Core web API projesi oluşturulur ve Visual Studio Code'da açılır.</span><span class="sxs-lookup"><span data-stu-id="7c76b-163">A new ASP.NET Core web API project targeting .NET Core is generated and opened in Visual Studio Code.</span></span>

1. <span data-ttu-id="7c76b-164">Tıklayın **Evet** olduğunda *gerekli varlıkları oluşturun ve hata ayıklama 'BooksApi' eksik. Bunları eklensin mi?*  bildirim görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="7c76b-164">Click **Yes** when the *Required assets to build and debug are missing from 'BooksApi'. Add them?* notification appears.</span></span>
1. <span data-ttu-id="7c76b-165">Açık **tümleşik Terminalini** ve proje kök dizinine gidin.</span><span class="sxs-lookup"><span data-stu-id="7c76b-165">Open **Integrated Terminal** and navigate to the project root.</span></span> <span data-ttu-id="7c76b-166">MongoDB için .NET sürücüsünü yüklemek için aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="7c76b-166">Run the following command to install the .NET driver for MongoDB:</span></span>

    ```console
    dotnet add BooksApi.csproj package MongoDB.Driver -v 2.7.2
    ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="7c76b-167">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7c76b-167">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="7c76b-168">Git **dosya** > **yeni çözüm** > **.NET Core** > **uygulama**.</span><span class="sxs-lookup"><span data-stu-id="7c76b-168">Go to **File** > **New Solution** > **.NET Core** > **App**.</span></span>
1. <span data-ttu-id="7c76b-169">Seçin **ASP.NET Core Web API'si** C# proje şablonu ve tıklayın **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="7c76b-169">Select the **ASP.NET Core Web API** C# project template, and click **Next**.</span></span>
1. <span data-ttu-id="7c76b-170">Seçin **.NET Core 2.2** gelen **hedef Framework'ü** açılır listede seçeneğine tıklayıp **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="7c76b-170">Select **.NET Core 2.2** from the **Target Framework** drop-down list, and click **Next**.</span></span>
1. <span data-ttu-id="7c76b-171">Girin *BooksApi* için **proje adı**, tıklatıp **Oluştur**.</span><span class="sxs-lookup"><span data-stu-id="7c76b-171">Enter *BooksApi* for the **Project Name**, and click **Create**.</span></span>
1. <span data-ttu-id="7c76b-172">İçinde **çözüm** paneli, projenin sağ **bağımlılıkları** düğümünü seçip alt **paketleri Ekle**.</span><span class="sxs-lookup"><span data-stu-id="7c76b-172">In the **Solution** pad, right-click the project's **Dependencies** node and select **Add Packages**.</span></span>
1. <span data-ttu-id="7c76b-173">Girin *MongoDB.Driver* arama kutusunda *MongoDB.Driver* paketini ve tıklayın **Paketi Ekle**.</span><span class="sxs-lookup"><span data-stu-id="7c76b-173">Enter *MongoDB.Driver* in the search box, select the *MongoDB.Driver* package, and click **Add Package**.</span></span>
1. <span data-ttu-id="7c76b-174">Tıklayın **kabul** düğmesine **lisans kabulü** iletişim.</span><span class="sxs-lookup"><span data-stu-id="7c76b-174">Click the **Accept** button in the **License Acceptance** dialog.</span></span>

---

## <a name="add-a-model"></a><span data-ttu-id="7c76b-175">Model ekleme</span><span class="sxs-lookup"><span data-stu-id="7c76b-175">Add a model</span></span>

1. <span data-ttu-id="7c76b-176">Ekleme bir *modelleri* proje kök dizini.</span><span class="sxs-lookup"><span data-stu-id="7c76b-176">Add a *Models* directory to the project root.</span></span>
1. <span data-ttu-id="7c76b-177">Ekleme bir `Book` sınıfının *modelleri* aşağıdaki kod ile dizin:</span><span class="sxs-lookup"><span data-stu-id="7c76b-177">Add a `Book` class to the *Models* directory with the following code:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Models/Book.cs)]

<span data-ttu-id="7c76b-178">Önceki sınıfında `Id` özelliği ortak dil çalışma zamanı (CLR) nesnesi için MongoDB koleksiyonu eşlemek için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="7c76b-178">In the preceding class, the `Id` property is required for mapping the Common Language Runtime (CLR) object to the MongoDB collection.</span></span> <span data-ttu-id="7c76b-179">Sınıftaki diğer özellikler ile donatılmış `[BsonElement]` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="7c76b-179">Other properties in the class are decorated with the `[BsonElement]` attribute.</span></span> <span data-ttu-id="7c76b-180">Özniteliğin değeri, özellik adı, MongoDB koleksiyonu temsil eder.</span><span class="sxs-lookup"><span data-stu-id="7c76b-180">The attribute's value represents the property name in the MongoDB collection.</span></span>

## <a name="add-a-crud-operations-class"></a><span data-ttu-id="7c76b-181">CRUD işlemleri sınıfı Ekle</span><span class="sxs-lookup"><span data-stu-id="7c76b-181">Add a CRUD operations class</span></span>

1. <span data-ttu-id="7c76b-182">Ekleme bir *Hizmetleri* proje kök dizini.</span><span class="sxs-lookup"><span data-stu-id="7c76b-182">Add a *Services* directory to the project root.</span></span>
1. <span data-ttu-id="7c76b-183">Ekleme bir `BookService` sınıfının *Hizmetleri* aşağıdaki kod ile dizin:</span><span class="sxs-lookup"><span data-stu-id="7c76b-183">Add a `BookService` class to the *Services* directory with the following code:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Services/BookService.cs?name=snippet_BookServiceClass)]

1. <span data-ttu-id="7c76b-184">MongoDB bağlantı dizesi Ekle *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="7c76b-184">Add the MongoDB connection string to *appsettings.json*:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/appsettings.json?highlight=2-4)]

    <span data-ttu-id="7c76b-185">Önceki `BookstoreDb` özelliğine erişilirse `BookService` sınıf oluşturucusu.</span><span class="sxs-lookup"><span data-stu-id="7c76b-185">The preceding `BookstoreDb` property is accessed in the `BookService` class constructor.</span></span>

1. <span data-ttu-id="7c76b-186">İçinde `Startup.ConfigureServices`, kayıt `BookService` bağımlılık ekleme sistemiyle sınıfı:</span><span class="sxs-lookup"><span data-stu-id="7c76b-186">In `Startup.ConfigureServices`, register the `BookService` class with the Dependency Injection system:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

    <span data-ttu-id="7c76b-187">Önceki hizmet kaydı sınıfları tüketen yapıcı eklemeyi desteklemek gereklidir.</span><span class="sxs-lookup"><span data-stu-id="7c76b-187">The preceding service registration is necessary to support constructor injection in consuming classes.</span></span>

<span data-ttu-id="7c76b-188">`BookService` Sınıfını kullanan aşağıdaki `MongoDB.Driver` veritabanında CRUD işlemleri gerçekleştirmek için üyeleri:</span><span class="sxs-lookup"><span data-stu-id="7c76b-188">The `BookService` class uses the following `MongoDB.Driver` members to perform CRUD operations against the database:</span></span>

* <span data-ttu-id="7c76b-189">`MongoClient` &ndash; Veritabanı işlemleri gerçekleştirmek için kullanılan bir sunucuyu okur.</span><span class="sxs-lookup"><span data-stu-id="7c76b-189">`MongoClient` &ndash; Reads the server instance for performing database operations.</span></span> <span data-ttu-id="7c76b-190">Bu sınıfın oluşturucusu, MongoDB bağlantı dizesini sağlanır:</span><span class="sxs-lookup"><span data-stu-id="7c76b-190">The constructor of this class is provided the MongoDB connection string:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Services/BookService.cs?name=snippet_BookServiceConstructor&highlight=3)]

* <span data-ttu-id="7c76b-191">`IMongoDatabase` &ndash; Mongo veritabanı işlemleri gerçekleştirmek için temsil eder.</span><span class="sxs-lookup"><span data-stu-id="7c76b-191">`IMongoDatabase` &ndash; Represents the Mongo database for performing operations.</span></span> <span data-ttu-id="7c76b-192">Bu öğreticide genel `GetCollection<T>(collection)` yöntemi belirli bir koleksiyondaki verileri erişim elde etmek için arabirim.</span><span class="sxs-lookup"><span data-stu-id="7c76b-192">This tutorial uses the generic `GetCollection<T>(collection)` method on the interface to gain access to data in a specific collection.</span></span> <span data-ttu-id="7c76b-193">Bu yöntemi çağrıldıktan sonra koleksiyonunda CRUD işlemleri gerçekleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="7c76b-193">CRUD operations can be performed against the collection after this method is called.</span></span> <span data-ttu-id="7c76b-194">İçinde `GetCollection<T>(collection)` yöntem çağrısı:</span><span class="sxs-lookup"><span data-stu-id="7c76b-194">In the `GetCollection<T>(collection)` method call:</span></span>
  * <span data-ttu-id="7c76b-195">`collection` Koleksiyon adını temsil eder.</span><span class="sxs-lookup"><span data-stu-id="7c76b-195">`collection` represents the collection name.</span></span>
  * <span data-ttu-id="7c76b-196">`T` Bir koleksiyonda depolanan CLR nesne türünü temsil eder.</span><span class="sxs-lookup"><span data-stu-id="7c76b-196">`T` represents the CLR object type stored in the collection.</span></span>

<span data-ttu-id="7c76b-197">`GetCollection<T>(collection)` döndürür bir `MongoCollection` koleksiyonu temsil eden nesne.</span><span class="sxs-lookup"><span data-stu-id="7c76b-197">`GetCollection<T>(collection)` returns a `MongoCollection` object representing the collection.</span></span> <span data-ttu-id="7c76b-198">Bu öğreticide, aşağıdaki yöntemlerden koleksiyonunda çağrılır:</span><span class="sxs-lookup"><span data-stu-id="7c76b-198">In this tutorial, the following methods are invoked on the collection:</span></span>

* <span data-ttu-id="7c76b-199">`Find<T>` &ndash; Sağlanan arama ölçütleriyle eşleşen koleksiyondaki tüm belgeleri döndürür.</span><span class="sxs-lookup"><span data-stu-id="7c76b-199">`Find<T>` &ndash; Returns all documents in the collection matching the provided search criteria.</span></span>
* <span data-ttu-id="7c76b-200">`InsertOne` &ndash; Belirtilen nesne koleksiyonunda yeni bir belge olarak ekler.</span><span class="sxs-lookup"><span data-stu-id="7c76b-200">`InsertOne` &ndash; Inserts the provided object as a new document in the collection.</span></span>
* <span data-ttu-id="7c76b-201">`ReplaceOne` &ndash; Sağlanan nesne ile sağlanan arama ölçütleriyle eşleşen tek bir belge değiştirir.</span><span class="sxs-lookup"><span data-stu-id="7c76b-201">`ReplaceOne` &ndash; Replaces the single document matching the provided search criteria with the provided object.</span></span>
* <span data-ttu-id="7c76b-202">`DeleteOne` &ndash; Belirtilen arama ölçütleriyle eşleşen tek bir belge siler.</span><span class="sxs-lookup"><span data-stu-id="7c76b-202">`DeleteOne` &ndash; Deletes a single document matching the provided search criteria.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="7c76b-203">Denetleyici ekleme</span><span class="sxs-lookup"><span data-stu-id="7c76b-203">Add a controller</span></span>

1. <span data-ttu-id="7c76b-204">Ekleme bir `BooksController` sınıfının *denetleyicileri* aşağıdaki kod ile dizin:</span><span class="sxs-lookup"><span data-stu-id="7c76b-204">Add a `BooksController` class to the *Controllers* directory with the following code:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Controllers/BooksController.cs)]

    <span data-ttu-id="7c76b-205">Önceki web API denetleyicisi:</span><span class="sxs-lookup"><span data-stu-id="7c76b-205">The preceding web API controller:</span></span>

    * <span data-ttu-id="7c76b-206">Kullanan `BookService` CRUD işlemleri gerçekleştirmek için sınıf.</span><span class="sxs-lookup"><span data-stu-id="7c76b-206">Uses the `BookService` class to perform CRUD operations.</span></span>
    * <span data-ttu-id="7c76b-207">GET, POST, PUT ve DELETE HTTP isteklerini desteklemek için eylem yöntemleri içerir.</span><span class="sxs-lookup"><span data-stu-id="7c76b-207">Contains action methods to support GET, POST, PUT, and DELETE HTTP requests.</span></span>
1. <span data-ttu-id="7c76b-208">Uygulamayı derleyin ve çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="7c76b-208">Build and run the app.</span></span>
1. <span data-ttu-id="7c76b-209">Gidin `http://localhost:<port>/api/books` tarayıcınızda.</span><span class="sxs-lookup"><span data-stu-id="7c76b-209">Navigate to `http://localhost:<port>/api/books` in your browser.</span></span> <span data-ttu-id="7c76b-210">Aşağıdaki JSON yanıtı gösterilir:</span><span class="sxs-lookup"><span data-stu-id="7c76b-210">The following JSON response is displayed:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="7c76b-211">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="7c76b-211">Next steps</span></span>

<span data-ttu-id="7c76b-212">ASP.NET Core web API'leri oluşturmaya daha fazla bilgi için aşağıdaki kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="7c76b-212">For more information on building ASP.NET Core web APIs, see the following resources:</span></span>

* <xref:web-api/index>
* <xref:web-api/action-return-types>
