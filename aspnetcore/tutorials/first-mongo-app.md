---
title: MongoDB ile ASP.NET Core ile Web API oluşturma
author: prkhandelwal
description: Bu öğreticide bir ASP.NET Core web API'si kullanarak bir MongoDB NoSQL veritabanı oluşturma gösterilmektedir.
ms.author: scaddie
ms.custom: mvc, seodec18
ms.date: 06/03/2019
uid: tutorials/first-mongo-app
ms.openlocfilehash: 88904f94eac2362947ea3f1fd68b708ef2fd6bea
ms.sourcegitcommit: a04eb20e81243930ec829a9db5dd5de49f669450
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/03/2019
ms.locfileid: "66470379"
---
# <a name="create-a-web-api-with-aspnet-core-and-mongodb"></a><span data-ttu-id="d8dcb-103">MongoDB ile ASP.NET Core ile web API'si oluşturma</span><span class="sxs-lookup"><span data-stu-id="d8dcb-103">Create a web API with ASP.NET Core and MongoDB</span></span>

<span data-ttu-id="d8dcb-104">Tarafından [Pratik Khandelwal](https://twitter.com/K2Prk) ve [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="d8dcb-104">By [Pratik Khandelwal](https://twitter.com/K2Prk) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="d8dcb-105">Bu öğreticide web API'si temel oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemleri gerçekleştiren oluşturur bir [MongoDB](https://www.mongodb.com/what-is-mongodb) NoSQL veritabanı.</span><span class="sxs-lookup"><span data-stu-id="d8dcb-105">This tutorial creates a web API that performs Create, Read, Update, and Delete (CRUD) operations on a [MongoDB](https://www.mongodb.com/what-is-mongodb) NoSQL database.</span></span>

<span data-ttu-id="d8dcb-106">Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:</span><span class="sxs-lookup"><span data-stu-id="d8dcb-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="d8dcb-107">MongoDB yapılandırın</span><span class="sxs-lookup"><span data-stu-id="d8dcb-107">Configure MongoDB</span></span>
> * <span data-ttu-id="d8dcb-108">MongoDB veritabanı oluşturma</span><span class="sxs-lookup"><span data-stu-id="d8dcb-108">Create a MongoDB database</span></span>
> * <span data-ttu-id="d8dcb-109">MongoDB koleksiyonu ve şema tanımlayın</span><span class="sxs-lookup"><span data-stu-id="d8dcb-109">Define a MongoDB collection and schema</span></span>
> * <span data-ttu-id="d8dcb-110">Bir web API'sini MongoDB CRUD işlemleri gerçekleştirme</span><span class="sxs-lookup"><span data-stu-id="d8dcb-110">Perform MongoDB CRUD operations from a web API</span></span>

<span data-ttu-id="d8dcb-111">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-mongo-app/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d8dcb-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-mongo-app/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d8dcb-112">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="d8dcb-112">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d8dcb-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d8dcb-113">Visual Studio</span></span>](#tab/visual-studio)

* [<span data-ttu-id="d8dcb-114">.NET core SDK 2.2 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="d8dcb-114">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="d8dcb-115">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) ile **ASP.NET ve web geliştirme** iş yükü</span><span class="sxs-lookup"><span data-stu-id="d8dcb-115">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="d8dcb-116">MongoDB</span><span class="sxs-lookup"><span data-stu-id="d8dcb-116">MongoDB</span></span>](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d8dcb-117">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d8dcb-117">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="d8dcb-118">.NET core SDK 2.2 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="d8dcb-118">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="d8dcb-119">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d8dcb-119">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="d8dcb-120">Visual Studio Code için C#</span><span class="sxs-lookup"><span data-stu-id="d8dcb-120">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [<span data-ttu-id="d8dcb-121">MongoDB</span><span class="sxs-lookup"><span data-stu-id="d8dcb-121">MongoDB</span></span>](https://docs.mongodb.com/manual/administration/install-community/)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="d8dcb-122">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d8dcb-122">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* [<span data-ttu-id="d8dcb-123">.NET core SDK 2.2 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="d8dcb-123">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="d8dcb-124">Mac 7,7 veya sonraki bir sürümü için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d8dcb-124">Visual Studio for Mac version 7.7 or later</span></span>](https://visualstudio.microsoft.com/downloads/)
* [<span data-ttu-id="d8dcb-125">MongoDB</span><span class="sxs-lookup"><span data-stu-id="d8dcb-125">MongoDB</span></span>](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-os-x/)

---

## <a name="configure-mongodb"></a><span data-ttu-id="d8dcb-126">MongoDB yapılandırın</span><span class="sxs-lookup"><span data-stu-id="d8dcb-126">Configure MongoDB</span></span>

<span data-ttu-id="d8dcb-127">Windows kullanıyorsanız, MongoDB yüklü *C:\\Program dosyaları\\MongoDB* varsayılan olarak.</span><span class="sxs-lookup"><span data-stu-id="d8dcb-127">If using Windows, MongoDB is installed at *C:\\Program Files\\MongoDB* by default.</span></span> <span data-ttu-id="d8dcb-128">Ekleme *C:\\Program dosyaları\\MongoDB\\sunucu\\\<version_number >\\bin* için `Path` ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="d8dcb-128">Add *C:\\Program Files\\MongoDB\\Server\\\<version_number>\\bin* to the `Path` environment variable.</span></span> <span data-ttu-id="d8dcb-129">Bu değişiklik yerden MongoDB erişim sağlar, geliştirme makinenizde.</span><span class="sxs-lookup"><span data-stu-id="d8dcb-129">This change enables MongoDB access from anywhere on your development machine.</span></span>

<span data-ttu-id="d8dcb-130">Mongo kabuğunu veritabanı oluşturma, koleksiyonları yapın ve belgeleri depolamak için aşağıdaki adımları kullanın.</span><span class="sxs-lookup"><span data-stu-id="d8dcb-130">Use the mongo Shell in the following steps to create a database, make collections, and store documents.</span></span> <span data-ttu-id="d8dcb-131">Mongo Kabuğu komutları hakkında daha fazla bilgi için bkz. [mongo kabuğunu çalışma](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell).</span><span class="sxs-lookup"><span data-stu-id="d8dcb-131">For more information on mongo Shell commands, see [Working with the mongo Shell](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell).</span></span>

1. <span data-ttu-id="d8dcb-132">Geliştirme makinenizde verilerin depolanması için bir dizin seçin.</span><span class="sxs-lookup"><span data-stu-id="d8dcb-132">Choose a directory on your development machine for storing the data.</span></span> <span data-ttu-id="d8dcb-133">Örneğin, *C:\\BooksData* Windows üzerinde.</span><span class="sxs-lookup"><span data-stu-id="d8dcb-133">For example, *C:\\BooksData* on Windows.</span></span> <span data-ttu-id="d8dcb-134">Yoksa dizini oluşturun.</span><span class="sxs-lookup"><span data-stu-id="d8dcb-134">Create the directory if it doesn't exist.</span></span> <span data-ttu-id="d8dcb-135">Mongo kabuğunu yeni dizinleri oluşturmaz.</span><span class="sxs-lookup"><span data-stu-id="d8dcb-135">The mongo Shell doesn't create new directories.</span></span>
1. <span data-ttu-id="d8dcb-136">Bir komut kabuğunu açın.</span><span class="sxs-lookup"><span data-stu-id="d8dcb-136">Open a command shell.</span></span> <span data-ttu-id="d8dcb-137">Varsayılan bağlantı noktası 27017 mongodb'ye bağlanmak için aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="d8dcb-137">Run the following command to connect to MongoDB on default port 27017.</span></span> <span data-ttu-id="d8dcb-138">Değiştirmeyi unutmayın `<data_directory_path>` önceki adımda seçtiğiniz dizini.</span><span class="sxs-lookup"><span data-stu-id="d8dcb-138">Remember to replace `<data_directory_path>` with the directory you chose in the previous step.</span></span>

    ```console
    mongod --dbpath <data_directory_path>
    ```

1. <span data-ttu-id="d8dcb-139">Başka bir komut kabuğu örneği açın.</span><span class="sxs-lookup"><span data-stu-id="d8dcb-139">Open another command shell instance.</span></span> <span data-ttu-id="d8dcb-140">Aşağıdaki komutu çalıştırarak varsayılan test veritabanı'na bağlanma:</span><span class="sxs-lookup"><span data-stu-id="d8dcb-140">Connect to the default test database by running the following command:</span></span>

    ```console
    mongo
    ```

1. <span data-ttu-id="d8dcb-141">Bir komut kabuğu'nda aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="d8dcb-141">Run the following in a command shell:</span></span>

    ```console
    use BookstoreDb
    ```

    <span data-ttu-id="d8dcb-142">Adlı bir veritabanı zaten mevcut olmayan halinde *BookstoreDb* oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="d8dcb-142">If it doesn't already exist, a database named *BookstoreDb* is created.</span></span> <span data-ttu-id="d8dcb-143">Veritabanı mevcut değilse, bağlantı işlemleri için açılır.</span><span class="sxs-lookup"><span data-stu-id="d8dcb-143">If the database does exist, its connection is opened for transactions.</span></span>

1. <span data-ttu-id="d8dcb-144">Oluşturma bir `Books` koleksiyon aşağıdaki komutu kullanarak:</span><span class="sxs-lookup"><span data-stu-id="d8dcb-144">Create a `Books` collection using following command:</span></span>

    ```console
    db.createCollection('Books')
    ```

    <span data-ttu-id="d8dcb-145">Aşağıdaki sonucu görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="d8dcb-145">The following result is displayed:</span></span>

    ```console
    { "ok" : 1 }
    ```

1. <span data-ttu-id="d8dcb-146">İçin bir şema tanımlayabilir `Books` toplama ve ekleme iki belge aşağıdaki komutu kullanarak:</span><span class="sxs-lookup"><span data-stu-id="d8dcb-146">Define a schema for the `Books` collection and insert two documents using the following command:</span></span>

    ```console
    db.Books.insertMany([{'Name':'Design Patterns','Price':54.93,'Category':'Computers','Author':'Ralph Johnson'}, {'Name':'Clean Code','Price':43.15,'Category':'Computers','Author':'Robert C. Martin'}])
    ```

    <span data-ttu-id="d8dcb-147">Aşağıdaki sonucu görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="d8dcb-147">The following result is displayed:</span></span>

    ```console
    {
      "acknowledged" : true,
      "insertedIds" : [
        ObjectId("5bfd996f7b8e48dc15ff215d"),
        ObjectId("5bfd996f7b8e48dc15ff215e")
      ]
    }
    ```

1. <span data-ttu-id="d8dcb-148">Aşağıdaki komutu kullanarak veritabanında belgelerini görüntüleyin:</span><span class="sxs-lookup"><span data-stu-id="d8dcb-148">View the documents in the database using the following command:</span></span>

    ```console
    db.Books.find({}).pretty()
    ```

    <span data-ttu-id="d8dcb-149">Aşağıdaki sonucu görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="d8dcb-149">The following result is displayed:</span></span>

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

    <span data-ttu-id="d8dcb-150">Şema girmiş ekler `_id` türünün özelliği `ObjectId` her belge için.</span><span class="sxs-lookup"><span data-stu-id="d8dcb-150">The schema adds an autogenerated `_id` property of type `ObjectId` for each document.</span></span>

<span data-ttu-id="d8dcb-151">Veritabanı hazırdır.</span><span class="sxs-lookup"><span data-stu-id="d8dcb-151">The database is ready.</span></span> <span data-ttu-id="d8dcb-152">ASP.NET Core web API'si oluşturmaya başlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d8dcb-152">You can start creating the ASP.NET Core web API.</span></span>

## <a name="create-the-aspnet-core-web-api-project"></a><span data-ttu-id="d8dcb-153">ASP.NET Core web API projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="d8dcb-153">Create the ASP.NET Core web API project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d8dcb-154">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d8dcb-154">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="d8dcb-155">Git **dosya** > **yeni** > **proje**.</span><span class="sxs-lookup"><span data-stu-id="d8dcb-155">Go to **File** > **New** > **Project**.</span></span>
1. <span data-ttu-id="d8dcb-156">Seçin **ASP.NET Core Web uygulaması** proje türü ve seçin **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="d8dcb-156">Select the **ASP.NET Core Web Application** project type, and select **Next**.</span></span>
1. <span data-ttu-id="d8dcb-157">Projeyi adlandırın *BooksApi*seçip **Oluştur**.</span><span class="sxs-lookup"><span data-stu-id="d8dcb-157">Name the project *BooksApi*, and select **Create**.</span></span>
1. <span data-ttu-id="d8dcb-158">Seçin **.NET Core** hedef çerçeve ve **ASP.NET Core 2.2**.</span><span class="sxs-lookup"><span data-stu-id="d8dcb-158">Select the **.NET Core** target framework and **ASP.NET Core 2.2**.</span></span> <span data-ttu-id="d8dcb-159">Seçin **API** proje şablonu, belirleyin **Oluştur**.</span><span class="sxs-lookup"><span data-stu-id="d8dcb-159">Select the **API** project template, and select **Create**.</span></span>
1. <span data-ttu-id="d8dcb-160">Ziyaret [NuGet Galerisi: Mongodb](https://www.nuget.org/packages/MongoDB.Driver/) için MongoDB .NET sürücüsü en son kararlı sürümünü belirlemek için.</span><span class="sxs-lookup"><span data-stu-id="d8dcb-160">Visit the [NuGet Gallery: MongoDB.Driver](https://www.nuget.org/packages/MongoDB.Driver/) to determine the latest stable version of the .NET driver for MongoDB.</span></span> <span data-ttu-id="d8dcb-161">İçinde **Paket Yöneticisi Konsolu** penceresinde proje kök dizinine gidin.</span><span class="sxs-lookup"><span data-stu-id="d8dcb-161">In the **Package Manager Console** window, navigate to the project root.</span></span> <span data-ttu-id="d8dcb-162">MongoDB için .NET sürücüsünü yüklemek için aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="d8dcb-162">Run the following command to install the .NET driver for MongoDB:</span></span>

    ```powershell
    Install-Package MongoDB.Driver -Version {VERSION}
    ```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d8dcb-163">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d8dcb-163">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="d8dcb-164">Bir komut kabuğu'nda aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="d8dcb-164">Run the following commands in a command shell:</span></span>

    ```console
    dotnet new webapi -o BooksApi
    code BooksApi
    ```

    <span data-ttu-id="d8dcb-165">.NET Core'u hedefleyen yeni bir ASP.NET Core web API projesi oluşturulur ve Visual Studio Code'da açılır.</span><span class="sxs-lookup"><span data-stu-id="d8dcb-165">A new ASP.NET Core web API project targeting .NET Core is generated and opened in Visual Studio Code.</span></span>

1. <span data-ttu-id="d8dcb-166">Durum çubuğunun OmniSharp sonra soran bir iletişim kutusu yangın simgesi yeşile **gerekli varlıkları oluşturun ve hata ayıklama 'BooksApi' eksik. Bunları eklensin mi?** .</span><span class="sxs-lookup"><span data-stu-id="d8dcb-166">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'BooksApi'. Add them?**.</span></span> <span data-ttu-id="d8dcb-167">Seçin **Evet**.</span><span class="sxs-lookup"><span data-stu-id="d8dcb-167">Select **Yes**.</span></span>
1. <span data-ttu-id="d8dcb-168">Ziyaret [NuGet Galerisi: Mongodb](https://www.nuget.org/packages/MongoDB.Driver/) için MongoDB .NET sürücüsü en son kararlı sürümünü belirlemek için.</span><span class="sxs-lookup"><span data-stu-id="d8dcb-168">Visit the [NuGet Gallery: MongoDB.Driver](https://www.nuget.org/packages/MongoDB.Driver/) to determine the latest stable version of the .NET driver for MongoDB.</span></span> <span data-ttu-id="d8dcb-169">Açık **tümleşik Terminalini** ve proje kök dizinine gidin.</span><span class="sxs-lookup"><span data-stu-id="d8dcb-169">Open **Integrated Terminal** and navigate to the project root.</span></span> <span data-ttu-id="d8dcb-170">MongoDB için .NET sürücüsünü yüklemek için aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="d8dcb-170">Run the following command to install the .NET driver for MongoDB:</span></span>

    ```console
    dotnet add BooksApi.csproj package MongoDB.Driver -v {VERSION}
    ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="d8dcb-171">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d8dcb-171">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="d8dcb-172">Git **dosya** > **yeni çözüm** >  **.NET Core** > **uygulama**.</span><span class="sxs-lookup"><span data-stu-id="d8dcb-172">Go to **File** > **New Solution** > **.NET Core** > **App**.</span></span>
1. <span data-ttu-id="d8dcb-173">Seçin **ASP.NET Core Web API'si** C# proje şablonu, belirleyin **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="d8dcb-173">Select the **ASP.NET Core Web API** C# project template, and select **Next**.</span></span>
1. <span data-ttu-id="d8dcb-174">Seçin **.NET Core 2.2** gelen **hedef Framework'ü** seçin ve açılır listede **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="d8dcb-174">Select **.NET Core 2.2** from the **Target Framework** drop-down list, and select **Next**.</span></span>
1. <span data-ttu-id="d8dcb-175">Girin *BooksApi* için **proje adı**seçip **Oluştur**.</span><span class="sxs-lookup"><span data-stu-id="d8dcb-175">Enter *BooksApi* for the **Project Name**, and select **Create**.</span></span>
1. <span data-ttu-id="d8dcb-176">İçinde **çözüm** paneli, projenin sağ **bağımlılıkları** düğümünü seçip alt **paketleri Ekle**.</span><span class="sxs-lookup"><span data-stu-id="d8dcb-176">In the **Solution** pad, right-click the project's **Dependencies** node and select **Add Packages**.</span></span>
1. <span data-ttu-id="d8dcb-177">Girin *MongoDB.Driver* arama kutusunda *MongoDB.Driver* paketini bulun ve seçin **Paketi Ekle**.</span><span class="sxs-lookup"><span data-stu-id="d8dcb-177">Enter *MongoDB.Driver* in the search box, select the *MongoDB.Driver* package, and select **Add Package**.</span></span>
1. <span data-ttu-id="d8dcb-178">Seçin **kabul** düğmesine **lisans kabulü** iletişim.</span><span class="sxs-lookup"><span data-stu-id="d8dcb-178">Select the **Accept** button in the **License Acceptance** dialog.</span></span>

---

## <a name="add-an-entity-model"></a><span data-ttu-id="d8dcb-179">Varlık modeli ekleme</span><span class="sxs-lookup"><span data-stu-id="d8dcb-179">Add an entity model</span></span>

1. <span data-ttu-id="d8dcb-180">Ekleme bir *modelleri* proje kök dizini.</span><span class="sxs-lookup"><span data-stu-id="d8dcb-180">Add a *Models* directory to the project root.</span></span>
1. <span data-ttu-id="d8dcb-181">Ekleme bir `Book` sınıfının *modelleri* aşağıdaki kod ile dizin:</span><span class="sxs-lookup"><span data-stu-id="d8dcb-181">Add a `Book` class to the *Models* directory with the following code:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Models/Book.cs)]

    <span data-ttu-id="d8dcb-182">Önceki sınıfında `Id` özelliği:</span><span class="sxs-lookup"><span data-stu-id="d8dcb-182">In the preceding class, the `Id` property:</span></span>
    
    * <span data-ttu-id="d8dcb-183">Ortak dil çalışma zamanı (CLR) nesnesi için MongoDB koleksiyonu eşlemek için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="d8dcb-183">Is required for mapping the Common Language Runtime (CLR) object to the MongoDB collection.</span></span>
    * <span data-ttu-id="d8dcb-184">İle açıklanıyor [[BsonId]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonIdAttribute.htm) belgenin birincil anahtarı olarak bu özellik belirlemek için.</span><span class="sxs-lookup"><span data-stu-id="d8dcb-184">Is annotated with [[BsonId]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonIdAttribute.htm) to designate this property as the document's primary key.</span></span>
    * <span data-ttu-id="d8dcb-185">İle açıklanıyor [[BsonRepresentation(BsonType.ObjectId)]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonRepresentationAttribute.htm) parametre türü olarak geçirerek izin vermek için `string` yerine bir [objectID](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_ObjectId.htm) yapısı.</span><span class="sxs-lookup"><span data-stu-id="d8dcb-185">Is annotated with [[BsonRepresentation(BsonType.ObjectId)]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonRepresentationAttribute.htm) to allow passing the parameter as type `string` instead of an [ObjectId](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_ObjectId.htm) structure.</span></span> <span data-ttu-id="d8dcb-186">Mongo işleme dönüştürme `string` için `ObjectId`.</span><span class="sxs-lookup"><span data-stu-id="d8dcb-186">Mongo handles the conversion from `string` to `ObjectId`.</span></span>
    
    <span data-ttu-id="d8dcb-187">Sınıftaki diğer özellikler ile açıklamalı olan [[BsonElement]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonElementAttribute.htm) özniteliği.</span><span class="sxs-lookup"><span data-stu-id="d8dcb-187">Other properties in the class are annotated with the [[BsonElement]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonElementAttribute.htm) attribute.</span></span> <span data-ttu-id="d8dcb-188">Özniteliğin değeri, özellik adı, MongoDB koleksiyonu temsil eder.</span><span class="sxs-lookup"><span data-stu-id="d8dcb-188">The attribute's value represents the property name in the MongoDB collection.</span></span>

## <a name="add-a-configuration-model"></a><span data-ttu-id="d8dcb-189">Yapılandırma modeli ekleme</span><span class="sxs-lookup"><span data-stu-id="d8dcb-189">Add a configuration model</span></span>

1. <span data-ttu-id="d8dcb-190">Aşağıdaki veritabanı yapılandırma değerlerini eklemek *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="d8dcb-190">Add the following database configuration values to *appsettings.json*:</span></span>

    [!code-json[](first-mongo-app/sample/BooksApi/appsettings.json?highlight=2-6)]

1. <span data-ttu-id="d8dcb-191">Ekleme bir *BookstoreDatabaseSettings.cs* dosyasını *modelleri* aşağıdaki kod ile dizin:</span><span class="sxs-lookup"><span data-stu-id="d8dcb-191">Add a *BookstoreDatabaseSettings.cs* file to the *Models* directory with the following code:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Models/BookstoreDatabaseSettings.cs)]

    <span data-ttu-id="d8dcb-192">Önceki `BookstoreDatabaseSettings` depolamak için kullanılan sınıf *appsettings.json* dosyanın `BookstoreDatabaseSettings` özellik değerleri.</span><span class="sxs-lookup"><span data-stu-id="d8dcb-192">The preceding `BookstoreDatabaseSettings` class is used to store the *appsettings.json* file's `BookstoreDatabaseSettings` property values.</span></span> <span data-ttu-id="d8dcb-193">JSON ve C# özellik adları adlı aynı şekilde eşleme işlemini kolaylaştırmak için.</span><span class="sxs-lookup"><span data-stu-id="d8dcb-193">The JSON and C# property names are named identically to ease the mapping process.</span></span>

1. <span data-ttu-id="d8dcb-194">Aşağıdaki kodu ekleyin `Startup.ConfigureServices`, çağırmadan önce `AddMvc`:</span><span class="sxs-lookup"><span data-stu-id="d8dcb-194">Add the following code to `Startup.ConfigureServices`, before the call to `AddMvc`:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Startup.cs?name=snippet_ConfigureDatabaseSettings)]

    <span data-ttu-id="d8dcb-195">Yukarıdaki kodda:</span><span class="sxs-lookup"><span data-stu-id="d8dcb-195">In the preceding code:</span></span>

    * <span data-ttu-id="d8dcb-196">Yapılandırma örneğine *appsettings.json* dosyanın `BookstoreDatabaseSettings` bölümüne bağlar, bağımlılık ekleme (dı) kapsayıcısında kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="d8dcb-196">The configuration instance to which the *appsettings.json* file's `BookstoreDatabaseSettings` section binds is registered in the Dependency Injection (DI) container.</span></span> <span data-ttu-id="d8dcb-197">Örneğin, bir `BookstoreDatabaseSettings` nesnenin `ConnectionString` özelliği ile doldurulur `BookstoreDatabaseSettings:ConnectionString` özelliğinde *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="d8dcb-197">For example, a `BookstoreDatabaseSettings` object's `ConnectionString` property is populated with the `BookstoreDatabaseSettings:ConnectionString` property in *appsettings.json*.</span></span>
    * <span data-ttu-id="d8dcb-198">`IBookstoreDatabaseSettings` Arabirimi ile tek DI kayıtlı [hizmet ömrü](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="d8dcb-198">The `IBookstoreDatabaseSettings` interface is registered in DI with a singleton [service lifetime](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="d8dcb-199">Arabirim örneğinin eklenen, çözümler bir `BookstoreDatabaseSettings` nesne.</span><span class="sxs-lookup"><span data-stu-id="d8dcb-199">When injected, the interface instance resolves to a `BookstoreDatabaseSettings` object.</span></span>

1. <span data-ttu-id="d8dcb-200">Üstüne aşağıdaki kodu ekleyin *Startup.cs* çözümlenecek `BookstoreDatabaseSettings` ve `IBookstoreDatabaseSettings` başvuruları:</span><span class="sxs-lookup"><span data-stu-id="d8dcb-200">Add the following code to the top of *Startup.cs* to resolve the `BookstoreDatabaseSettings` and `IBookstoreDatabaseSettings` references:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Startup.cs?name=snippet_UsingBooksApiModels)]

## <a name="add-a-crud-operations-service"></a><span data-ttu-id="d8dcb-201">CRUD işlemleri hizmet ekleme</span><span class="sxs-lookup"><span data-stu-id="d8dcb-201">Add a CRUD operations service</span></span>

1. <span data-ttu-id="d8dcb-202">Ekleme bir *Hizmetleri* proje kök dizini.</span><span class="sxs-lookup"><span data-stu-id="d8dcb-202">Add a *Services* directory to the project root.</span></span>
1. <span data-ttu-id="d8dcb-203">Ekleme bir `BookService` sınıfının *Hizmetleri* aşağıdaki kod ile dizin:</span><span class="sxs-lookup"><span data-stu-id="d8dcb-203">Add a `BookService` class to the *Services* directory with the following code:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Services/BookService.cs?name=snippet_BookServiceClass)]

    <span data-ttu-id="d8dcb-204">Önceki kodda, bir `IBookstoreDatabaseSettings` Oluşturucu ekleme örneği DI alınır.</span><span class="sxs-lookup"><span data-stu-id="d8dcb-204">In the preceding code, an `IBookstoreDatabaseSettings` instance is retrieved from DI via constructor injection.</span></span> <span data-ttu-id="d8dcb-205">Bu tekniği erişim sağlayan *appsettings.json* eklenmiştir yapılandırma değerlerini [yapılandırma modeli ekleme](#add-a-configuration-model) bölümü.</span><span class="sxs-lookup"><span data-stu-id="d8dcb-205">This technique provides access to the *appsettings.json* configuration values that were added in the [Add a configuration model](#add-a-configuration-model) section.</span></span>

1. <span data-ttu-id="d8dcb-206">İçinde `Startup.ConfigureServices`, kayıt `BookService` sınıfıyla dı:</span><span class="sxs-lookup"><span data-stu-id="d8dcb-206">In `Startup.ConfigureServices`, register the `BookService` class with DI:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Startup.cs?name=snippet_ConfigureServices&highlight=9)]

    <span data-ttu-id="d8dcb-207">Önceki kodda, `BookService` sınıfı Oluşturucu ekleme sınıfları tüketen desteklemek için DI ile kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="d8dcb-207">In the preceding code, the `BookService` class is registered with DI to support constructor injection in consuming classes.</span></span> <span data-ttu-id="d8dcb-208">Singleton hizmet ömrü uygundur çünkü `BookService` doğrudan bağımlılık vereceğine `MongoClient`.</span><span class="sxs-lookup"><span data-stu-id="d8dcb-208">The singleton service lifetime is most appropriate because `BookService` takes a direct dependency on `MongoClient`.</span></span> <span data-ttu-id="d8dcb-209">Resmi başına [Mongo istemci yeniden yönergeleri](https://mongodb.github.io/mongo-csharp-driver/2.8/reference/driver/connecting/#re-use), `MongoClient` DI tekil hizmet ömrü ile kayıtlı olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="d8dcb-209">Per the official [Mongo Client reuse guidelines](https://mongodb.github.io/mongo-csharp-driver/2.8/reference/driver/connecting/#re-use), `MongoClient` should be registered in DI with a singleton service lifetime.</span></span>

1. <span data-ttu-id="d8dcb-210">Üstüne aşağıdaki kodu ekleyin *Startup.cs* çözümlenecek `BookService` başvurusu:</span><span class="sxs-lookup"><span data-stu-id="d8dcb-210">Add the following code to the top of *Startup.cs* to resolve the `BookService` reference:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Startup.cs?name=snippet_UsingBooksApiServices)]

<span data-ttu-id="d8dcb-211">`BookService` Sınıfını kullanan aşağıdaki `MongoDB.Driver` veritabanında CRUD işlemleri gerçekleştirmek için üyeleri:</span><span class="sxs-lookup"><span data-stu-id="d8dcb-211">The `BookService` class uses the following `MongoDB.Driver` members to perform CRUD operations against the database:</span></span>

* <span data-ttu-id="d8dcb-212">[MongoClient](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoClient.htm) &ndash; veritabanı işlemleri gerçekleştirmek için kullanılan bir sunucuyu okur.</span><span class="sxs-lookup"><span data-stu-id="d8dcb-212">[MongoClient](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoClient.htm) &ndash; Reads the server instance for performing database operations.</span></span> <span data-ttu-id="d8dcb-213">Bu sınıfın oluşturucusu, MongoDB bağlantı dizesini sağlanır:</span><span class="sxs-lookup"><span data-stu-id="d8dcb-213">The constructor of this class is provided the MongoDB connection string:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Services/BookService.cs?name=snippet_BookServiceConstructor&highlight=3)]

* <span data-ttu-id="d8dcb-214">[IMongoDatabase](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_IMongoDatabase.htm) &ndash; işlemleri gerçekleştirmek için kullanılan Mongo veritabanını temsil eder.</span><span class="sxs-lookup"><span data-stu-id="d8dcb-214">[IMongoDatabase](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_IMongoDatabase.htm) &ndash; Represents the Mongo database for performing operations.</span></span> <span data-ttu-id="d8dcb-215">Bu öğreticide genel [belirtilmiş<TDocument>(koleksiyon)](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoDatabase_GetCollection__1.htm) yöntemi belirli bir koleksiyondaki verileri erişim elde etmek için arabirim.</span><span class="sxs-lookup"><span data-stu-id="d8dcb-215">This tutorial uses the generic [GetCollection<TDocument>(collection)](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoDatabase_GetCollection__1.htm) method on the interface to gain access to data in a specific collection.</span></span> <span data-ttu-id="d8dcb-216">Bu yöntemi çağrıldıktan sonra koleksiyonu karşı bir CRUD işlemleri gerçekleştirin.</span><span class="sxs-lookup"><span data-stu-id="d8dcb-216">Perform CRUD operations against the collection after this method is called.</span></span> <span data-ttu-id="d8dcb-217">İçinde `GetCollection<TDocument>(collection)` yöntem çağrısı:</span><span class="sxs-lookup"><span data-stu-id="d8dcb-217">In the `GetCollection<TDocument>(collection)` method call:</span></span>
  * <span data-ttu-id="d8dcb-218">`collection` Koleksiyon adını temsil eder.</span><span class="sxs-lookup"><span data-stu-id="d8dcb-218">`collection` represents the collection name.</span></span>
  * <span data-ttu-id="d8dcb-219">`TDocument` Bir koleksiyonda depolanan CLR nesne türünü temsil eder.</span><span class="sxs-lookup"><span data-stu-id="d8dcb-219">`TDocument` represents the CLR object type stored in the collection.</span></span>

<span data-ttu-id="d8dcb-220">`GetCollection<TDocument>(collection)` döndürür bir [MongoCollection](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoCollection.htm) koleksiyonu temsil eden nesne.</span><span class="sxs-lookup"><span data-stu-id="d8dcb-220">`GetCollection<TDocument>(collection)` returns a [MongoCollection](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoCollection.htm) object representing the collection.</span></span> <span data-ttu-id="d8dcb-221">Bu öğreticide, aşağıdaki yöntemlerden koleksiyonunda çağrılır:</span><span class="sxs-lookup"><span data-stu-id="d8dcb-221">In this tutorial, the following methods are invoked on the collection:</span></span>

* <span data-ttu-id="d8dcb-222">[DeleteOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_DeleteOne.htm) &ndash; sağlanan arama ölçütleriyle eşleşen tek bir belge siler.</span><span class="sxs-lookup"><span data-stu-id="d8dcb-222">[DeleteOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_DeleteOne.htm) &ndash; Deletes a single document matching the provided search criteria.</span></span>
* <span data-ttu-id="d8dcb-223">[Bulma\<TDocument >](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollectionExtensions_Find__1_1.htm) &ndash; sağlanan arama ölçütleriyle eşleşen koleksiyondaki tüm belgeleri döndürür.</span><span class="sxs-lookup"><span data-stu-id="d8dcb-223">[Find\<TDocument>](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollectionExtensions_Find__1_1.htm) &ndash; Returns all documents in the collection matching the provided search criteria.</span></span>
* <span data-ttu-id="d8dcb-224">[InsertOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_InsertOne.htm) &ndash; belirtilen nesne koleksiyonunda yeni bir belge olarak ekler.</span><span class="sxs-lookup"><span data-stu-id="d8dcb-224">[InsertOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_InsertOne.htm) &ndash; Inserts the provided object as a new document in the collection.</span></span>
* <span data-ttu-id="d8dcb-225">[ReplaceOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_ReplaceOne.htm) &ndash; sağlanan nesne ile sağlanan arama ölçütleriyle eşleşen tek bir belge değiştirir.</span><span class="sxs-lookup"><span data-stu-id="d8dcb-225">[ReplaceOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_ReplaceOne.htm) &ndash; Replaces the single document matching the provided search criteria with the provided object.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="d8dcb-226">Denetleyici ekleme</span><span class="sxs-lookup"><span data-stu-id="d8dcb-226">Add a controller</span></span>

<span data-ttu-id="d8dcb-227">Ekleme bir `BooksController` sınıfının *denetleyicileri* aşağıdaki kod ile dizin:</span><span class="sxs-lookup"><span data-stu-id="d8dcb-227">Add a `BooksController` class to the *Controllers* directory with the following code:</span></span>

[!code-csharp[](first-mongo-app/sample/BooksApi/Controllers/BooksController.cs)]

<span data-ttu-id="d8dcb-228">Önceki web API denetleyicisi:</span><span class="sxs-lookup"><span data-stu-id="d8dcb-228">The preceding web API controller:</span></span>

* <span data-ttu-id="d8dcb-229">Kullanan `BookService` CRUD işlemleri gerçekleştirmek için sınıf.</span><span class="sxs-lookup"><span data-stu-id="d8dcb-229">Uses the `BookService` class to perform CRUD operations.</span></span>
* <span data-ttu-id="d8dcb-230">GET, POST, PUT ve DELETE HTTP isteklerini desteklemek için eylem yöntemleri içerir.</span><span class="sxs-lookup"><span data-stu-id="d8dcb-230">Contains action methods to support GET, POST, PUT, and DELETE HTTP requests.</span></span>
* <span data-ttu-id="d8dcb-231">Çağrıları <xref:System.Web.Http.ApiController.CreatedAtRoute*> içinde `Create` döndürülecek eylem yöntemi bir [HTTP 201](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) yanıt.</span><span class="sxs-lookup"><span data-stu-id="d8dcb-231">Calls <xref:System.Web.Http.ApiController.CreatedAtRoute*> in the `Create` action method to return an [HTTP 201](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) response.</span></span> <span data-ttu-id="d8dcb-232">Durum kodu 201 sunucuda yeni bir kaynak oluşturan bir HTTP POST yöntemi için standart yanıttır.</span><span class="sxs-lookup"><span data-stu-id="d8dcb-232">Status code 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span> <span data-ttu-id="d8dcb-233">`CreatedAtRoute` Ayrıca ekler bir `Location` yanıt üst bilgisi.</span><span class="sxs-lookup"><span data-stu-id="d8dcb-233">`CreatedAtRoute` also adds a `Location` header to the response.</span></span> <span data-ttu-id="d8dcb-234">`Location` Üst bilgisi, yeni oluşturulan kitap URI'sini belirtir.</span><span class="sxs-lookup"><span data-stu-id="d8dcb-234">The `Location` header specifies the URI of the newly created book.</span></span>

## <a name="test-the-web-api"></a><span data-ttu-id="d8dcb-235">Web API'sini test etme</span><span class="sxs-lookup"><span data-stu-id="d8dcb-235">Test the web API</span></span>

1. <span data-ttu-id="d8dcb-236">Uygulamayı derleyin ve çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="d8dcb-236">Build and run the app.</span></span>

1. <span data-ttu-id="d8dcb-237">Gidin `http://localhost:<port>/api/books` test denetleyicisi için parametresiz `Get` eylem yöntemi.</span><span class="sxs-lookup"><span data-stu-id="d8dcb-237">Navigate to `http://localhost:<port>/api/books` to test the controller's parameterless `Get` action method.</span></span> <span data-ttu-id="d8dcb-238">Aşağıdaki JSON yanıtı gösterilir:</span><span class="sxs-lookup"><span data-stu-id="d8dcb-238">The following JSON response is displayed:</span></span>

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

1. <span data-ttu-id="d8dcb-239">Gidin `http://localhost:<port>/api/books/5bfd996f7b8e48dc15ff215e` test denetleyicisi için aşırı yüklenmiş `Get` eylem yöntemi.</span><span class="sxs-lookup"><span data-stu-id="d8dcb-239">Navigate to `http://localhost:<port>/api/books/5bfd996f7b8e48dc15ff215e` to test the controller's overloaded `Get` action method.</span></span> <span data-ttu-id="d8dcb-240">Aşağıdaki JSON yanıtı gösterilir:</span><span class="sxs-lookup"><span data-stu-id="d8dcb-240">The following JSON response is displayed:</span></span>

    ```json
    {
      "id":"5bfd996f7b8e48dc15ff215e",
      "bookName":"Clean Code",
      "price":43.15,
      "category":"Computers",
      "author":"Robert C. Martin"
    }
    ```

## <a name="next-steps"></a><span data-ttu-id="d8dcb-241">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="d8dcb-241">Next steps</span></span>

<span data-ttu-id="d8dcb-242">ASP.NET Core web API'leri oluşturmaya daha fazla bilgi için aşağıdaki kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="d8dcb-242">For more information on building ASP.NET Core web APIs, see the following resources:</span></span>

* [<span data-ttu-id="d8dcb-243">Bu makalenin YouTube sürümü</span><span class="sxs-lookup"><span data-stu-id="d8dcb-243">YouTube version of this article</span></span>](https://www.youtube.com/watch?v=7uJt_sOenyo&feature=youtu.be)
* <xref:web-api/index>
* <xref:web-api/action-return-types>
