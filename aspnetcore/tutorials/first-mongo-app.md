---
title: MongoDB ile ASP.NET Core ile web API'si oluşturma
author: prkhandelwal
description: Bu öğreticide bir ASP.NET Core web API'sini kullanarak bir MongoDB NoSQL veritabanı oluşturma işlemini gösterir.
ms.author: scaddie
ms.custom: mvc, seodec18
ms.date: 07/10/2019
uid: tutorials/first-mongo-app
ms.openlocfilehash: c64f6e69a19e294a18cc72c860af0a03ef70d444
ms.sourcegitcommit: 357a7120632b20465801c093e4e5bd4a315496a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2019
ms.locfileid: "67649187"
---
# <a name="create-a-web-api-with-aspnet-core-and-mongodb"></a><span data-ttu-id="c9ec2-103">MongoDB ile ASP.NET Core ile web API'si oluşturma</span><span class="sxs-lookup"><span data-stu-id="c9ec2-103">Create a web API with ASP.NET Core and MongoDB</span></span>

<span data-ttu-id="c9ec2-104">Tarafından [Pratik Khandelwal](https://twitter.com/K2Prk) ve [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="c9ec2-104">By [Pratik Khandelwal](https://twitter.com/K2Prk) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="c9ec2-105">Bu öğreticide web API'si temel oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemleri gerçekleştiren oluşturur bir [MongoDB](https://www.mongodb.com/what-is-mongodb) NoSQL veritabanı.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-105">This tutorial creates a web API that performs Create, Read, Update, and Delete (CRUD) operations on a [MongoDB](https://www.mongodb.com/what-is-mongodb) NoSQL database.</span></span>

<span data-ttu-id="c9ec2-106">Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:</span><span class="sxs-lookup"><span data-stu-id="c9ec2-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c9ec2-107">MongoDB yapılandırın</span><span class="sxs-lookup"><span data-stu-id="c9ec2-107">Configure MongoDB</span></span>
> * <span data-ttu-id="c9ec2-108">MongoDB veritabanı oluşturma</span><span class="sxs-lookup"><span data-stu-id="c9ec2-108">Create a MongoDB database</span></span>
> * <span data-ttu-id="c9ec2-109">MongoDB koleksiyonu ve şema tanımlayın</span><span class="sxs-lookup"><span data-stu-id="c9ec2-109">Define a MongoDB collection and schema</span></span>
> * <span data-ttu-id="c9ec2-110">Bir web API'sini MongoDB CRUD işlemleri gerçekleştirme</span><span class="sxs-lookup"><span data-stu-id="c9ec2-110">Perform MongoDB CRUD operations from a web API</span></span>
> * <span data-ttu-id="c9ec2-111">JSON seri hale getirme özelleştirmek</span><span class="sxs-lookup"><span data-stu-id="c9ec2-111">Customize JSON serialization</span></span>

<span data-ttu-id="c9ec2-112">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-mongo-app/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c9ec2-112">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-mongo-app/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c9ec2-113">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="c9ec2-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c9ec2-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c9ec2-114">Visual Studio</span></span>](#tab/visual-studio)

* [<span data-ttu-id="c9ec2-115">.NET core SDK 2.2 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="c9ec2-115">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="c9ec2-116">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) ile **ASP.NET ve web geliştirme** iş yükü</span><span class="sxs-lookup"><span data-stu-id="c9ec2-116">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="c9ec2-117">MongoDB</span><span class="sxs-lookup"><span data-stu-id="c9ec2-117">MongoDB</span></span>](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c9ec2-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c9ec2-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="c9ec2-119">.NET core SDK 2.2 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="c9ec2-119">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="c9ec2-120">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c9ec2-120">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="c9ec2-121">Visual Studio Code için C#</span><span class="sxs-lookup"><span data-stu-id="c9ec2-121">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [<span data-ttu-id="c9ec2-122">MongoDB</span><span class="sxs-lookup"><span data-stu-id="c9ec2-122">MongoDB</span></span>](https://docs.mongodb.com/manual/administration/install-community/)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="c9ec2-123">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c9ec2-123">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* [<span data-ttu-id="c9ec2-124">.NET core SDK 2.2 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="c9ec2-124">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="c9ec2-125">Mac 7,7 veya sonraki bir sürümü için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c9ec2-125">Visual Studio for Mac version 7.7 or later</span></span>](https://visualstudio.microsoft.com/downloads/)
* [<span data-ttu-id="c9ec2-126">MongoDB</span><span class="sxs-lookup"><span data-stu-id="c9ec2-126">MongoDB</span></span>](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-os-x/)

---

## <a name="configure-mongodb"></a><span data-ttu-id="c9ec2-127">MongoDB yapılandırın</span><span class="sxs-lookup"><span data-stu-id="c9ec2-127">Configure MongoDB</span></span>

<span data-ttu-id="c9ec2-128">Windows kullanıyorsanız, MongoDB yüklü *C:\\Program dosyaları\\MongoDB* varsayılan olarak.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-128">If using Windows, MongoDB is installed at *C:\\Program Files\\MongoDB* by default.</span></span> <span data-ttu-id="c9ec2-129">Ekleme *C:\\Program dosyaları\\MongoDB\\sunucu\\\<version_number >\\bin* için `Path` ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-129">Add *C:\\Program Files\\MongoDB\\Server\\\<version_number>\\bin* to the `Path` environment variable.</span></span> <span data-ttu-id="c9ec2-130">Bu değişiklik yerden MongoDB erişim sağlar, geliştirme makinenizde.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-130">This change enables MongoDB access from anywhere on your development machine.</span></span>

<span data-ttu-id="c9ec2-131">Mongo kabuğunu veritabanı oluşturma, koleksiyonları yapın ve belgeleri depolamak için aşağıdaki adımları kullanın.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-131">Use the mongo Shell in the following steps to create a database, make collections, and store documents.</span></span> <span data-ttu-id="c9ec2-132">Mongo Kabuğu komutları hakkında daha fazla bilgi için bkz. [mongo kabuğunu çalışma](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell).</span><span class="sxs-lookup"><span data-stu-id="c9ec2-132">For more information on mongo Shell commands, see [Working with the mongo Shell](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell).</span></span>

1. <span data-ttu-id="c9ec2-133">Geliştirme makinenizde verilerin depolanması için bir dizin seçin.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-133">Choose a directory on your development machine for storing the data.</span></span> <span data-ttu-id="c9ec2-134">Örneğin, *C:\\BooksData* Windows üzerinde.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-134">For example, *C:\\BooksData* on Windows.</span></span> <span data-ttu-id="c9ec2-135">Yoksa dizini oluşturun.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-135">Create the directory if it doesn't exist.</span></span> <span data-ttu-id="c9ec2-136">Mongo kabuğunu yeni dizinleri oluşturmaz.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-136">The mongo Shell doesn't create new directories.</span></span>
1. <span data-ttu-id="c9ec2-137">Bir komut kabuğunu açın.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-137">Open a command shell.</span></span> <span data-ttu-id="c9ec2-138">Varsayılan bağlantı noktası 27017 mongodb'ye bağlanmak için aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-138">Run the following command to connect to MongoDB on default port 27017.</span></span> <span data-ttu-id="c9ec2-139">Değiştirmeyi unutmayın `<data_directory_path>` önceki adımda seçtiğiniz dizini.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-139">Remember to replace `<data_directory_path>` with the directory you chose in the previous step.</span></span>

    ```console
    mongod --dbpath <data_directory_path>
    ```

1. <span data-ttu-id="c9ec2-140">Başka bir komut kabuğu örneği açın.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-140">Open another command shell instance.</span></span> <span data-ttu-id="c9ec2-141">Aşağıdaki komutu çalıştırarak varsayılan test veritabanı'na bağlanma:</span><span class="sxs-lookup"><span data-stu-id="c9ec2-141">Connect to the default test database by running the following command:</span></span>

    ```console
    mongo
    ```

1. <span data-ttu-id="c9ec2-142">Bir komut kabuğu'nda aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="c9ec2-142">Run the following in a command shell:</span></span>

    ```console
    use BookstoreDb
    ```

    <span data-ttu-id="c9ec2-143">Adlı bir veritabanı zaten mevcut olmayan halinde *BookstoreDb* oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-143">If it doesn't already exist, a database named *BookstoreDb* is created.</span></span> <span data-ttu-id="c9ec2-144">Veritabanı mevcut değilse, bağlantı işlemleri için açılır.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-144">If the database does exist, its connection is opened for transactions.</span></span>

1. <span data-ttu-id="c9ec2-145">Oluşturma bir `Books` koleksiyon aşağıdaki komutu kullanarak:</span><span class="sxs-lookup"><span data-stu-id="c9ec2-145">Create a `Books` collection using following command:</span></span>

    ```console
    db.createCollection('Books')
    ```

    <span data-ttu-id="c9ec2-146">Aşağıdaki sonucu görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="c9ec2-146">The following result is displayed:</span></span>

    ```console
    { "ok" : 1 }
    ```

1. <span data-ttu-id="c9ec2-147">İçin bir şema tanımlayabilir `Books` toplama ve ekleme iki belge aşağıdaki komutu kullanarak:</span><span class="sxs-lookup"><span data-stu-id="c9ec2-147">Define a schema for the `Books` collection and insert two documents using the following command:</span></span>

    ```console
    db.Books.insertMany([{'Name':'Design Patterns','Price':54.93,'Category':'Computers','Author':'Ralph Johnson'}, {'Name':'Clean Code','Price':43.15,'Category':'Computers','Author':'Robert C. Martin'}])
    ```

    <span data-ttu-id="c9ec2-148">Aşağıdaki sonucu görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="c9ec2-148">The following result is displayed:</span></span>

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
  > <span data-ttu-id="c9ec2-149">Kimliğin gösterilen bu makalede, bu örneği çalıştırdığınızda kimlikleri eşleşmez.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-149">The ID's shown in this article will not match the IDs when you run this sample.</span></span>

1. <span data-ttu-id="c9ec2-150">Aşağıdaki komutu kullanarak veritabanında belgelerini görüntüleyin:</span><span class="sxs-lookup"><span data-stu-id="c9ec2-150">View the documents in the database using the following command:</span></span>

    ```console
    db.Books.find({}).pretty()
    ```

    <span data-ttu-id="c9ec2-151">Aşağıdaki sonucu görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="c9ec2-151">The following result is displayed:</span></span>

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

    <span data-ttu-id="c9ec2-152">Şema girmiş ekler `_id` türünün özelliği `ObjectId` her belge için.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-152">The schema adds an autogenerated `_id` property of type `ObjectId` for each document.</span></span>

<span data-ttu-id="c9ec2-153">Veritabanı hazırdır.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-153">The database is ready.</span></span> <span data-ttu-id="c9ec2-154">ASP.NET Core web API'si oluşturmaya başlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-154">You can start creating the ASP.NET Core web API.</span></span>

## <a name="create-the-aspnet-core-web-api-project"></a><span data-ttu-id="c9ec2-155">ASP.NET Core web API projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="c9ec2-155">Create the ASP.NET Core web API project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c9ec2-156">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c9ec2-156">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="c9ec2-157">Git **dosya** > **yeni** > **proje**.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-157">Go to **File** > **New** > **Project**.</span></span>
1. <span data-ttu-id="c9ec2-158">Seçin **ASP.NET Core Web uygulaması** proje türü ve seçin **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-158">Select the **ASP.NET Core Web Application** project type, and select **Next**.</span></span>
1. <span data-ttu-id="c9ec2-159">Projeyi adlandırın *BooksApi*seçip **Oluştur**.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-159">Name the project *BooksApi*, and select **Create**.</span></span>
1. <span data-ttu-id="c9ec2-160">Seçin **.NET Core** hedef çerçeve ve **ASP.NET Core 2.2**.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-160">Select the **.NET Core** target framework and **ASP.NET Core 2.2**.</span></span> <span data-ttu-id="c9ec2-161">Seçin **API** proje şablonu, belirleyin **Oluştur**.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-161">Select the **API** project template, and select **Create**.</span></span>
1. <span data-ttu-id="c9ec2-162">Ziyaret [NuGet Galerisi: Mongodb](https://www.nuget.org/packages/MongoDB.Driver/) için MongoDB .NET sürücüsü en son kararlı sürümünü belirlemek için.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-162">Visit the [NuGet Gallery: MongoDB.Driver](https://www.nuget.org/packages/MongoDB.Driver/) to determine the latest stable version of the .NET driver for MongoDB.</span></span> <span data-ttu-id="c9ec2-163">İçinde **Paket Yöneticisi Konsolu** penceresinde proje kök dizinine gidin.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-163">In the **Package Manager Console** window, navigate to the project root.</span></span> <span data-ttu-id="c9ec2-164">MongoDB için .NET sürücüsünü yüklemek için aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="c9ec2-164">Run the following command to install the .NET driver for MongoDB:</span></span>

    ```powershell
    Install-Package MongoDB.Driver -Version {VERSION}
    ```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c9ec2-165">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c9ec2-165">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="c9ec2-166">Bir komut kabuğu'nda aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="c9ec2-166">Run the following commands in a command shell:</span></span>

    ```console
    dotnet new webapi -o BooksApi
    code BooksApi
    ```

    <span data-ttu-id="c9ec2-167">.NET Core'u hedefleyen yeni bir ASP.NET Core web API projesi oluşturulur ve Visual Studio Code'da açılır.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-167">A new ASP.NET Core web API project targeting .NET Core is generated and opened in Visual Studio Code.</span></span>

1. <span data-ttu-id="c9ec2-168">Durum çubuğunun OmniSharp sonra soran bir iletişim kutusu yangın simgesi yeşile **gerekli varlıkları oluşturun ve hata ayıklama 'BooksApi' eksik. Bunları eklensin mi?** .</span><span class="sxs-lookup"><span data-stu-id="c9ec2-168">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'BooksApi'. Add them?**.</span></span> <span data-ttu-id="c9ec2-169">Seçin **Evet**.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-169">Select **Yes**.</span></span>
1. <span data-ttu-id="c9ec2-170">Ziyaret [NuGet Galerisi: Mongodb](https://www.nuget.org/packages/MongoDB.Driver/) için MongoDB .NET sürücüsü en son kararlı sürümünü belirlemek için.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-170">Visit the [NuGet Gallery: MongoDB.Driver](https://www.nuget.org/packages/MongoDB.Driver/) to determine the latest stable version of the .NET driver for MongoDB.</span></span> <span data-ttu-id="c9ec2-171">Açık **tümleşik Terminalini** ve proje kök dizinine gidin.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-171">Open **Integrated Terminal** and navigate to the project root.</span></span> <span data-ttu-id="c9ec2-172">MongoDB için .NET sürücüsünü yüklemek için aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="c9ec2-172">Run the following command to install the .NET driver for MongoDB:</span></span>

    ```console
    dotnet add BooksApi.csproj package MongoDB.Driver -v {VERSION}
    ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="c9ec2-173">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c9ec2-173">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="c9ec2-174">Git **dosya** > **yeni çözüm** >  **.NET Core** > **uygulama**.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-174">Go to **File** > **New Solution** > **.NET Core** > **App**.</span></span>
1. <span data-ttu-id="c9ec2-175">Seçin **ASP.NET Core Web API'si** C# proje şablonu, belirleyin **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-175">Select the **ASP.NET Core Web API** C# project template, and select **Next**.</span></span>
1. <span data-ttu-id="c9ec2-176">Seçin **.NET Core 2.2** gelen **hedef Framework'ü** seçin ve açılır listede **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-176">Select **.NET Core 2.2** from the **Target Framework** drop-down list, and select **Next**.</span></span>
1. <span data-ttu-id="c9ec2-177">Girin *BooksApi* için **proje adı**seçip **Oluştur**.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-177">Enter *BooksApi* for the **Project Name**, and select **Create**.</span></span>
1. <span data-ttu-id="c9ec2-178">İçinde **çözüm** paneli, projenin sağ **bağımlılıkları** düğümünü seçip alt **paketleri Ekle**.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-178">In the **Solution** pad, right-click the project's **Dependencies** node and select **Add Packages**.</span></span>
1. <span data-ttu-id="c9ec2-179">Girin *MongoDB.Driver* arama kutusunda *MongoDB.Driver* paketini bulun ve seçin **Paketi Ekle**.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-179">Enter *MongoDB.Driver* in the search box, select the *MongoDB.Driver* package, and select **Add Package**.</span></span>
1. <span data-ttu-id="c9ec2-180">Seçin **kabul** düğmesine **lisans kabulü** iletişim.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-180">Select the **Accept** button in the **License Acceptance** dialog.</span></span>

---

## <a name="add-an-entity-model"></a><span data-ttu-id="c9ec2-181">Varlık modeli ekleme</span><span class="sxs-lookup"><span data-stu-id="c9ec2-181">Add an entity model</span></span>

1. <span data-ttu-id="c9ec2-182">Ekleme bir *modelleri* proje kök dizini.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-182">Add a *Models* directory to the project root.</span></span>
1. <span data-ttu-id="c9ec2-183">Ekleme bir `Book` sınıfının *modelleri* aşağıdaki kod ile dizin:</span><span class="sxs-lookup"><span data-stu-id="c9ec2-183">Add a `Book` class to the *Models* directory with the following code:</span></span>

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

    <span data-ttu-id="c9ec2-184">Önceki sınıfında `Id` özelliği:</span><span class="sxs-lookup"><span data-stu-id="c9ec2-184">In the preceding class, the `Id` property:</span></span>
    
    * <span data-ttu-id="c9ec2-185">Ortak dil çalışma zamanı (CLR) nesnesi için MongoDB koleksiyonu eşlemek için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-185">Is required for mapping the Common Language Runtime (CLR) object to the MongoDB collection.</span></span>
    * <span data-ttu-id="c9ec2-186">İle açıklanıyor [[BsonId]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonIdAttribute.htm) belgenin birincil anahtarı olarak bu özellik belirlemek için.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-186">Is annotated with [[BsonId]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonIdAttribute.htm) to designate this property as the document's primary key.</span></span>
    * <span data-ttu-id="c9ec2-187">İle açıklanıyor [[BsonRepresentation(BsonType.ObjectId)]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonRepresentationAttribute.htm) parametre türü olarak geçirerek izin vermek için `string` yerine bir [objectID](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_ObjectId.htm) yapısı.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-187">Is annotated with [[BsonRepresentation(BsonType.ObjectId)]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonRepresentationAttribute.htm) to allow passing the parameter as type `string` instead of an [ObjectId](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_ObjectId.htm) structure.</span></span> <span data-ttu-id="c9ec2-188">Mongo işleme dönüştürme `string` için `ObjectId`.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-188">Mongo handles the conversion from `string` to `ObjectId`.</span></span>
    
    <span data-ttu-id="c9ec2-189">`BookName` Özelliği ile ek açıklamalı [[BsonElement]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonElementAttribute.htm) özniteliği.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-189">The `BookName` property is annotated with the [[BsonElement]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonElementAttribute.htm) attribute.</span></span> <span data-ttu-id="c9ec2-190">Özniteliğin değerini `Name` özellik adında MongoDB koleksiyonu temsil eder.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-190">The attribute's value of `Name` represents the property name in the MongoDB collection.</span></span>

## <a name="add-a-configuration-model"></a><span data-ttu-id="c9ec2-191">Yapılandırma modeli ekleme</span><span class="sxs-lookup"><span data-stu-id="c9ec2-191">Add a configuration model</span></span>

1. <span data-ttu-id="c9ec2-192">Aşağıdaki veritabanı yapılandırma değerlerini eklemek *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="c9ec2-192">Add the following database configuration values to *appsettings.json*:</span></span>

    [!code-json[](first-mongo-app/sample/BooksApi/appsettings.json?highlight=2-6)]

1. <span data-ttu-id="c9ec2-193">Ekleme bir *BookstoreDatabaseSettings.cs* dosyasını *modelleri* aşağıdaki kod ile dizin:</span><span class="sxs-lookup"><span data-stu-id="c9ec2-193">Add a *BookstoreDatabaseSettings.cs* file to the *Models* directory with the following code:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Models/BookstoreDatabaseSettings.cs)]

    <span data-ttu-id="c9ec2-194">Önceki `BookstoreDatabaseSettings` depolamak için kullanılan sınıf *appsettings.json* dosyanın `BookstoreDatabaseSettings` özellik değerleri.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-194">The preceding `BookstoreDatabaseSettings` class is used to store the *appsettings.json* file's `BookstoreDatabaseSettings` property values.</span></span> <span data-ttu-id="c9ec2-195">JSON ve C# özellik adları adlı aynı şekilde eşleme işlemini kolaylaştırmak için.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-195">The JSON and C# property names are named identically to ease the mapping process.</span></span>

1. <span data-ttu-id="c9ec2-196">Aşağıdaki vurgulanmış kodu ekleyin `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="c9ec2-196">Add the following highlighted code to `Startup.ConfigureServices`:</span></span>

    [!code-csharp[](first-mongo-app/sample_snapshot/BooksApi/Startup.ConfigureServices.AddDbSettings.cs?highlight=3-7)]

    <span data-ttu-id="c9ec2-197">Yukarıdaki kodda:</span><span class="sxs-lookup"><span data-stu-id="c9ec2-197">In the preceding code:</span></span>

    * <span data-ttu-id="c9ec2-198">Yapılandırma örneğine *appsettings.json* dosyanın `BookstoreDatabaseSettings` bölümüne bağlar, bağımlılık ekleme (dı) kapsayıcısında kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-198">The configuration instance to which the *appsettings.json* file's `BookstoreDatabaseSettings` section binds is registered in the Dependency Injection (DI) container.</span></span> <span data-ttu-id="c9ec2-199">Örneğin, bir `BookstoreDatabaseSettings` nesnenin `ConnectionString` özelliği ile doldurulur `BookstoreDatabaseSettings:ConnectionString` özelliğinde *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-199">For example, a `BookstoreDatabaseSettings` object's `ConnectionString` property is populated with the `BookstoreDatabaseSettings:ConnectionString` property in *appsettings.json*.</span></span>
    * <span data-ttu-id="c9ec2-200">`IBookstoreDatabaseSettings` Arabirimi ile tek DI kayıtlı [hizmet ömrü](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="c9ec2-200">The `IBookstoreDatabaseSettings` interface is registered in DI with a singleton [service lifetime](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="c9ec2-201">Arabirim örneğinin eklenen, çözümler bir `BookstoreDatabaseSettings` nesne.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-201">When injected, the interface instance resolves to a `BookstoreDatabaseSettings` object.</span></span>

1. <span data-ttu-id="c9ec2-202">Üstüne aşağıdaki kodu ekleyin *Startup.cs* çözümlenecek `BookstoreDatabaseSettings` ve `IBookstoreDatabaseSettings` başvuruları:</span><span class="sxs-lookup"><span data-stu-id="c9ec2-202">Add the following code to the top of *Startup.cs* to resolve the `BookstoreDatabaseSettings` and `IBookstoreDatabaseSettings` references:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Startup.cs?name=snippet_UsingBooksApiModels)]

## <a name="add-a-crud-operations-service"></a><span data-ttu-id="c9ec2-203">CRUD işlemleri hizmet ekleme</span><span class="sxs-lookup"><span data-stu-id="c9ec2-203">Add a CRUD operations service</span></span>

1. <span data-ttu-id="c9ec2-204">Ekleme bir *Hizmetleri* proje kök dizini.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-204">Add a *Services* directory to the project root.</span></span>
1. <span data-ttu-id="c9ec2-205">Ekleme bir `BookService` sınıfının *Hizmetleri* aşağıdaki kod ile dizin:</span><span class="sxs-lookup"><span data-stu-id="c9ec2-205">Add a `BookService` class to the *Services* directory with the following code:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Services/BookService.cs?name=snippet_BookServiceClass)]

    <span data-ttu-id="c9ec2-206">Önceki kodda, bir `IBookstoreDatabaseSettings` Oluşturucu ekleme örneği DI alınır.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-206">In the preceding code, an `IBookstoreDatabaseSettings` instance is retrieved from DI via constructor injection.</span></span> <span data-ttu-id="c9ec2-207">Bu tekniği erişim sağlayan *appsettings.json* eklenmiştir yapılandırma değerlerini [yapılandırma modeli ekleme](#add-a-configuration-model) bölümü.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-207">This technique provides access to the *appsettings.json* configuration values that were added in the [Add a configuration model](#add-a-configuration-model) section.</span></span>

1. <span data-ttu-id="c9ec2-208">Aşağıdaki vurgulanmış kodu ekleyin `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="c9ec2-208">Add the following highlighted code to `Startup.ConfigureServices`:</span></span>

    [!code-csharp[](first-mongo-app/sample_snapshot/BooksApi/Startup.ConfigureServices.AddSingletonService.cs?highlight=9)]

    <span data-ttu-id="c9ec2-209">Önceki kodda, `BookService` sınıfı Oluşturucu ekleme sınıfları tüketen desteklemek için DI ile kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-209">In the preceding code, the `BookService` class is registered with DI to support constructor injection in consuming classes.</span></span> <span data-ttu-id="c9ec2-210">Singleton hizmet ömrü uygundur çünkü `BookService` doğrudan bağımlılık vereceğine `MongoClient`.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-210">The singleton service lifetime is most appropriate because `BookService` takes a direct dependency on `MongoClient`.</span></span> <span data-ttu-id="c9ec2-211">Resmi başına [Mongo istemci yeniden yönergeleri](https://mongodb.github.io/mongo-csharp-driver/2.8/reference/driver/connecting/#re-use), `MongoClient` DI tekil hizmet ömrü ile kayıtlı olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-211">Per the official [Mongo Client reuse guidelines](https://mongodb.github.io/mongo-csharp-driver/2.8/reference/driver/connecting/#re-use), `MongoClient` should be registered in DI with a singleton service lifetime.</span></span>

1. <span data-ttu-id="c9ec2-212">Üstüne aşağıdaki kodu ekleyin *Startup.cs* çözümlenecek `BookService` başvurusu:</span><span class="sxs-lookup"><span data-stu-id="c9ec2-212">Add the following code to the top of *Startup.cs* to resolve the `BookService` reference:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Startup.cs?name=snippet_UsingBooksApiServices)]

<span data-ttu-id="c9ec2-213">`BookService` Sınıfını kullanan aşağıdaki `MongoDB.Driver` veritabanında CRUD işlemleri gerçekleştirmek için üyeleri:</span><span class="sxs-lookup"><span data-stu-id="c9ec2-213">The `BookService` class uses the following `MongoDB.Driver` members to perform CRUD operations against the database:</span></span>

* <span data-ttu-id="c9ec2-214">[MongoClient](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoClient.htm) &ndash; veritabanı işlemleri gerçekleştirmek için kullanılan bir sunucuyu okur.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-214">[MongoClient](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoClient.htm) &ndash; Reads the server instance for performing database operations.</span></span> <span data-ttu-id="c9ec2-215">Bu sınıfın oluşturucusu, MongoDB bağlantı dizesini sağlanır:</span><span class="sxs-lookup"><span data-stu-id="c9ec2-215">The constructor of this class is provided the MongoDB connection string:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Services/BookService.cs?name=snippet_BookServiceConstructor&highlight=3)]

* <span data-ttu-id="c9ec2-216">[IMongoDatabase](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_IMongoDatabase.htm) &ndash; işlemleri gerçekleştirmek için kullanılan Mongo veritabanını temsil eder.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-216">[IMongoDatabase](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_IMongoDatabase.htm) &ndash; Represents the Mongo database for performing operations.</span></span> <span data-ttu-id="c9ec2-217">Bu öğreticide genel [belirtilmiş\<TDocument > (koleksiyon)](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoDatabase_GetCollection__1.htm) yöntemi belirli bir koleksiyondaki verileri erişim elde etmek için arabirim.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-217">This tutorial uses the generic [GetCollection\<TDocument>(collection)](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoDatabase_GetCollection__1.htm) method on the interface to gain access to data in a specific collection.</span></span> <span data-ttu-id="c9ec2-218">Bu yöntemi çağrıldıktan sonra koleksiyonu karşı bir CRUD işlemleri gerçekleştirin.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-218">Perform CRUD operations against the collection after this method is called.</span></span> <span data-ttu-id="c9ec2-219">İçinde `GetCollection<TDocument>(collection)` yöntem çağrısı:</span><span class="sxs-lookup"><span data-stu-id="c9ec2-219">In the `GetCollection<TDocument>(collection)` method call:</span></span>
  * <span data-ttu-id="c9ec2-220">`collection` Koleksiyon adını temsil eder.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-220">`collection` represents the collection name.</span></span>
  * <span data-ttu-id="c9ec2-221">`TDocument` Bir koleksiyonda depolanan CLR nesne türünü temsil eder.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-221">`TDocument` represents the CLR object type stored in the collection.</span></span>

<span data-ttu-id="c9ec2-222">`GetCollection<TDocument>(collection)` döndürür bir [MongoCollection](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoCollection.htm) koleksiyonu temsil eden nesne.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-222">`GetCollection<TDocument>(collection)` returns a [MongoCollection](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoCollection.htm) object representing the collection.</span></span> <span data-ttu-id="c9ec2-223">Bu öğreticide, aşağıdaki yöntemlerden koleksiyonunda çağrılır:</span><span class="sxs-lookup"><span data-stu-id="c9ec2-223">In this tutorial, the following methods are invoked on the collection:</span></span>

* <span data-ttu-id="c9ec2-224">[DeleteOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_DeleteOne.htm) &ndash; sağlanan arama ölçütleriyle eşleşen tek bir belge siler.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-224">[DeleteOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_DeleteOne.htm) &ndash; Deletes a single document matching the provided search criteria.</span></span>
* <span data-ttu-id="c9ec2-225">[Bulma\<TDocument >](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollectionExtensions_Find__1_1.htm) &ndash; sağlanan arama ölçütleriyle eşleşen koleksiyondaki tüm belgeleri döndürür.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-225">[Find\<TDocument>](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollectionExtensions_Find__1_1.htm) &ndash; Returns all documents in the collection matching the provided search criteria.</span></span>
* <span data-ttu-id="c9ec2-226">[InsertOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_InsertOne.htm) &ndash; belirtilen nesne koleksiyonunda yeni bir belge olarak ekler.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-226">[InsertOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_InsertOne.htm) &ndash; Inserts the provided object as a new document in the collection.</span></span>
* <span data-ttu-id="c9ec2-227">[ReplaceOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_ReplaceOne.htm) &ndash; sağlanan nesne ile sağlanan arama ölçütleriyle eşleşen tek bir belge değiştirir.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-227">[ReplaceOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_ReplaceOne.htm) &ndash; Replaces the single document matching the provided search criteria with the provided object.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="c9ec2-228">Denetleyici ekleme</span><span class="sxs-lookup"><span data-stu-id="c9ec2-228">Add a controller</span></span>

<span data-ttu-id="c9ec2-229">Ekleme bir `BooksController` sınıfının *denetleyicileri* aşağıdaki kod ile dizin:</span><span class="sxs-lookup"><span data-stu-id="c9ec2-229">Add a `BooksController` class to the *Controllers* directory with the following code:</span></span>

[!code-csharp[](first-mongo-app/sample/BooksApi/Controllers/BooksController.cs)]

<span data-ttu-id="c9ec2-230">Önceki web API denetleyicisi:</span><span class="sxs-lookup"><span data-stu-id="c9ec2-230">The preceding web API controller:</span></span>

* <span data-ttu-id="c9ec2-231">Kullanan `BookService` CRUD işlemleri gerçekleştirmek için sınıf.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-231">Uses the `BookService` class to perform CRUD operations.</span></span>
* <span data-ttu-id="c9ec2-232">GET, POST, PUT ve DELETE HTTP isteklerini desteklemek için eylem yöntemleri içerir.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-232">Contains action methods to support GET, POST, PUT, and DELETE HTTP requests.</span></span>
* <span data-ttu-id="c9ec2-233">Çağrıları <xref:System.Web.Http.ApiController.CreatedAtRoute*> içinde `Create` döndürülecek eylem yöntemi bir [HTTP 201](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) yanıt.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-233">Calls <xref:System.Web.Http.ApiController.CreatedAtRoute*> in the `Create` action method to return an [HTTP 201](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) response.</span></span> <span data-ttu-id="c9ec2-234">Durum kodu 201 sunucuda yeni bir kaynak oluşturan bir HTTP POST yöntemi için standart yanıttır.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-234">Status code 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span> <span data-ttu-id="c9ec2-235">`CreatedAtRoute` Ayrıca ekler bir `Location` yanıt üst bilgisi.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-235">`CreatedAtRoute` also adds a `Location` header to the response.</span></span> <span data-ttu-id="c9ec2-236">`Location` Üst bilgisi, yeni oluşturulan kitap URI'sini belirtir.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-236">The `Location` header specifies the URI of the newly created book.</span></span>

## <a name="test-the-web-api"></a><span data-ttu-id="c9ec2-237">Web API'sini test etme</span><span class="sxs-lookup"><span data-stu-id="c9ec2-237">Test the web API</span></span>

1. <span data-ttu-id="c9ec2-238">Uygulamayı derleyin ve çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-238">Build and run the app.</span></span>

1. <span data-ttu-id="c9ec2-239">Gidin `http://localhost:<port>/api/books` test denetleyicisi için parametresiz `Get` eylem yöntemi.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-239">Navigate to `http://localhost:<port>/api/books` to test the controller's parameterless `Get` action method.</span></span> <span data-ttu-id="c9ec2-240">Aşağıdaki JSON yanıtı gösterilir:</span><span class="sxs-lookup"><span data-stu-id="c9ec2-240">The following JSON response is displayed:</span></span>

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

1. <span data-ttu-id="c9ec2-241">Gidin `http://localhost:<port>/api/books/{id here}` test denetleyicisi için aşırı yüklenmiş `Get` eylem yöntemi.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-241">Navigate to `http://localhost:<port>/api/books/{id here}` to test the controller's overloaded `Get` action method.</span></span> <span data-ttu-id="c9ec2-242">Aşağıdaki JSON yanıtı gösterilir:</span><span class="sxs-lookup"><span data-stu-id="c9ec2-242">The following JSON response is displayed:</span></span>

    ```json
    {
      "id":"{ID}",
      "bookName":"Clean Code",
      "price":43.15,
      "category":"Computers",
      "author":"Robert C. Martin"
    }
    ```

## <a name="configure-json-serialization-options"></a><span data-ttu-id="c9ec2-243">JSON seri hale getirme seçenekleri yapılandırın</span><span class="sxs-lookup"><span data-stu-id="c9ec2-243">Configure JSON serialization options</span></span>

<span data-ttu-id="c9ec2-244">Döndürülen JSON yanıtları değiştirmek için iki ayrıntı [web API'si Test](#test-the-web-api) bölümü:</span><span class="sxs-lookup"><span data-stu-id="c9ec2-244">There are two details to change about the JSON responses returned in the [Test the web API](#test-the-web-api) section:</span></span>

* <span data-ttu-id="c9ec2-245">Özellik adlarını ortası büyük olan varsayılan büyük/küçük harf Pascal eşleşecek şekilde değiştirilmesi gereken CLR nesnenin özellik adları büyük/küçük harfleri.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-245">The property names' default camel casing should be changed to match the Pascal casing of the CLR object's property names.</span></span>
* <span data-ttu-id="c9ec2-246">`bookName` Özelliği olarak döndürülmesi `Name`.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-246">The `bookName` property should be returned as `Name`.</span></span>

<span data-ttu-id="c9ec2-247">Yukarıdaki gereksinimleri karşılamak için aşağıdaki değişiklikleri yapın:</span><span class="sxs-lookup"><span data-stu-id="c9ec2-247">To satisfy the preceding requirements, make the following changes:</span></span>

1. <span data-ttu-id="c9ec2-248">İçinde `Startup.ConfigureServices`, aşağıdaki vurgulanmış kodu için zincir `AddMvc` yöntem çağrısı:</span><span class="sxs-lookup"><span data-stu-id="c9ec2-248">In `Startup.ConfigureServices`, chain the following highlighted code on to the `AddMvc` method call:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Startup.cs?name=snippet_ConfigureServices&highlight=12)]

    <span data-ttu-id="c9ec2-249">Önceki değişiklikle, web API'SİNİN özellik adlarını JSON yanıtı eşleşme karşılık gelen özellik adlarını CLR nesne türü seri hale.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-249">With the preceding change, property names in the web API's serialized JSON response match their corresponding property names in the CLR object type.</span></span> <span data-ttu-id="c9ec2-250">Örneğin, `Book` sınıfın `Author` serileştiren özelliğini olarak `Author`.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-250">For example, the `Book` class's `Author` property serializes as `Author`.</span></span>

1. <span data-ttu-id="c9ec2-251">İçinde *Models/Book.cs*, açıklama `BookName` aşağıdaki özellik [[Item]](https://www.newtonsoft.com/json/help/html/T_Newtonsoft_Json_JsonPropertyAttribute.htm) özniteliği:</span><span class="sxs-lookup"><span data-stu-id="c9ec2-251">In *Models/Book.cs*, annotate the `BookName` property with the following [[JsonProperty]](https://www.newtonsoft.com/json/help/html/T_Newtonsoft_Json_JsonPropertyAttribute.htm) attribute:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Models/Book.cs?name=snippet_BookNameProperty&highlight=2)]

    <span data-ttu-id="c9ec2-252">`[JsonProperty]` Özniteliğin değerini `Name` seri hale getirilmiş JSON yanıtı web API'SİNİN name özelliği temsil eder.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-252">The `[JsonProperty]` attribute's value of `Name` represents the property name in the web API's serialized JSON response.</span></span>

1. <span data-ttu-id="c9ec2-253">Üstüne aşağıdaki kodu ekleyin *Models/Book.cs* çözümlenecek `[JsonProperty]` öznitelik başvurusu:</span><span class="sxs-lookup"><span data-stu-id="c9ec2-253">Add the following code to the top of *Models/Book.cs* to resolve the `[JsonProperty]` attribute reference:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Models/Book.cs?name=snippet_NewtonsoftJsonImport)]

1. <span data-ttu-id="c9ec2-254">Tanımlı adımları yineleyin [web API'si Test](#test-the-web-api) bölümü.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-254">Repeat the steps defined in the [Test the web API](#test-the-web-api) section.</span></span> <span data-ttu-id="c9ec2-255">JSON özellik adları fark dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="c9ec2-255">Notice the difference in JSON property names.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c9ec2-256">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="c9ec2-256">Next steps</span></span>

<span data-ttu-id="c9ec2-257">ASP.NET Core web API'leri oluşturmaya daha fazla bilgi için aşağıdaki kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="c9ec2-257">For more information on building ASP.NET Core web APIs, see the following resources:</span></span>

* [<span data-ttu-id="c9ec2-258">Bu makalenin YouTube sürümü</span><span class="sxs-lookup"><span data-stu-id="c9ec2-258">YouTube version of this article</span></span>](https://www.youtube.com/watch?v=7uJt_sOenyo&feature=youtu.be)
* <xref:web-api/index>
* <xref:web-api/action-return-types>
