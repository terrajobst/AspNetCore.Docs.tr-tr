---
uid: web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing
title: "ASP.NET Web API 2 özniteliği yönlendirme ile bir REST API'si oluşturma | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2013
ms.topic: article
ms.assetid: 23fc77da-2725-4434-99a0-ff872d96336b
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing
msc.type: authoredcontent
ms.openlocfilehash: c1d0b3e1644ef7f9ebb4be74c3fdf3df90cf3537
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="create-a-rest-api-with-attribute-routing-in-aspnet-web-api-2"></a><span data-ttu-id="01687-102">ASP.NET Web API 2 yönlendirme özniteliği olan bir REST API'si oluşturma</span><span class="sxs-lookup"><span data-stu-id="01687-102">Create a REST API with Attribute Routing in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="01687-103">tarafından [CAN Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="01687-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="01687-104">Web API 2 destekleyen yeni bir tür yönlendirme biri olarak adlandırılan *özniteliği yönlendirme*.</span><span class="sxs-lookup"><span data-stu-id="01687-104">Web API 2 supports a new type of routing, called *attribute routing*.</span></span> <span data-ttu-id="01687-105">Öznitelik yönlendirme genel bir bakış için bkz: [özniteliği yönlendirme Web API 2'deki](attribute-routing-in-web-api-2.md).</span><span class="sxs-lookup"><span data-stu-id="01687-105">For a general overview of attribute routing, see [Attribute Routing in Web API 2](attribute-routing-in-web-api-2.md).</span></span> <span data-ttu-id="01687-106">Bu öğreticide, öznitelik yönlendirme books koleksiyonu için bir REST API oluşturmak için kullanın.</span><span class="sxs-lookup"><span data-stu-id="01687-106">In this tutorial, you will use attribute routing to create a REST API for a collection of books.</span></span> <span data-ttu-id="01687-107">API aşağıdaki eylemleri destekler:</span><span class="sxs-lookup"><span data-stu-id="01687-107">The API will support the following actions:</span></span>

| <span data-ttu-id="01687-108">Eylem</span><span class="sxs-lookup"><span data-stu-id="01687-108">Action</span></span> | <span data-ttu-id="01687-109">Örnek URI</span><span class="sxs-lookup"><span data-stu-id="01687-109">Example URI</span></span> |
| --- | --- |
| <span data-ttu-id="01687-110">Tüm books listesini alın.</span><span class="sxs-lookup"><span data-stu-id="01687-110">Get a list of all books.</span></span> | <span data-ttu-id="01687-111">/ api/books</span><span class="sxs-lookup"><span data-stu-id="01687-111">/api/books</span></span> |
| <span data-ttu-id="01687-112">Kimliğe göre defteri alma</span><span class="sxs-lookup"><span data-stu-id="01687-112">Get a book by ID.</span></span> | <span data-ttu-id="01687-113">/api/books/1</span><span class="sxs-lookup"><span data-stu-id="01687-113">/api/books/1</span></span> |
| <span data-ttu-id="01687-114">Kitap ayrıntılarını alın.</span><span class="sxs-lookup"><span data-stu-id="01687-114">Get the details of a book.</span></span> | <span data-ttu-id="01687-115">/api/Books/1/details</span><span class="sxs-lookup"><span data-stu-id="01687-115">/api/books/1/details</span></span> |
| <span data-ttu-id="01687-116">Türe göre books listesini alın.</span><span class="sxs-lookup"><span data-stu-id="01687-116">Get a list of books by genre.</span></span> | <span data-ttu-id="01687-117">/api/Books/fantasy</span><span class="sxs-lookup"><span data-stu-id="01687-117">/api/books/fantasy</span></span> |
| <span data-ttu-id="01687-118">Yayın tarihe göre books listesini alın.</span><span class="sxs-lookup"><span data-stu-id="01687-118">Get a list of books by publication date.</span></span> | <span data-ttu-id="01687-119">/api/Books/Date/2013-02-16 /api/books/date/2013/02/16 (alternatif form)</span><span class="sxs-lookup"><span data-stu-id="01687-119">/api/books/date/2013-02-16 /api/books/date/2013/02/16 (alternate form)</span></span> |
| <span data-ttu-id="01687-120">Belirli bir yazar tarafından books listesini alın.</span><span class="sxs-lookup"><span data-stu-id="01687-120">Get a list of books by a particular author.</span></span> | <span data-ttu-id="01687-121">/api/Authors/1/Books</span><span class="sxs-lookup"><span data-stu-id="01687-121">/api/authors/1/books</span></span> |

<span data-ttu-id="01687-122">Tüm yöntemleri salt okunur (HTTP GET isteği) adı verilir.</span><span class="sxs-lookup"><span data-stu-id="01687-122">All methods are read-only (HTTP GET requests).</span></span>

<span data-ttu-id="01687-123">Entity Framework veri katmanı için kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="01687-123">For the data layer, we'll use Entity Framework.</span></span> <span data-ttu-id="01687-124">Kayıt defteri aşağıdaki alanları olacaktır:</span><span class="sxs-lookup"><span data-stu-id="01687-124">Book records will have the following fields:</span></span>

- <span data-ttu-id="01687-125">Kimlik</span><span class="sxs-lookup"><span data-stu-id="01687-125">ID</span></span>
- <span data-ttu-id="01687-126">Başlık</span><span class="sxs-lookup"><span data-stu-id="01687-126">Title</span></span>
- <span data-ttu-id="01687-127">Genre</span><span class="sxs-lookup"><span data-stu-id="01687-127">Genre</span></span>
- <span data-ttu-id="01687-128">Yayın tarihi</span><span class="sxs-lookup"><span data-stu-id="01687-128">Publication date</span></span>
- <span data-ttu-id="01687-129">Fiyat</span><span class="sxs-lookup"><span data-stu-id="01687-129">Price</span></span>
- <span data-ttu-id="01687-130">Açıklama</span><span class="sxs-lookup"><span data-stu-id="01687-130">Description</span></span>
- <span data-ttu-id="01687-131">AuthorID (yazarlar tablosuna yabancı anahtar)</span><span class="sxs-lookup"><span data-stu-id="01687-131">AuthorID (foreign key to an Authors table)</span></span>

<span data-ttu-id="01687-132">Çoğu istekler için ancak API (başlık, yazar ve Tarz) bu verilerin bir alt kümesini döndürür.</span><span class="sxs-lookup"><span data-stu-id="01687-132">For most requests, however, the API will return a subset of this data (title, author, and genre).</span></span> <span data-ttu-id="01687-133">Tam kayıt, istemci isteklerini almak için `/api/books/{id}/details`.</span><span class="sxs-lookup"><span data-stu-id="01687-133">To get the complete record, the client requests `/api/books/{id}/details`.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="01687-134">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="01687-134">Prerequisites</span></span>

<span data-ttu-id="01687-135">[Visual Studio 2017](https://www.visualstudio.com/vs/) Community, Professional veya Enterprise sürümü.</span><span class="sxs-lookup"><span data-stu-id="01687-135">[Visual Studio 2017](https://www.visualstudio.com/vs/) Community, Professional or Enterprise edition.</span></span>

## <a name="create-the-visual-studio-project"></a><span data-ttu-id="01687-136">Visual Studio projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="01687-136">Create the Visual Studio Project</span></span>

<span data-ttu-id="01687-137">Visual Studio çalıştırarak başlayın.</span><span class="sxs-lookup"><span data-stu-id="01687-137">Start by running Visual Studio.</span></span> <span data-ttu-id="01687-138">Gelen **dosya** menüsünde, select **yeni** ve ardından **proje**.</span><span class="sxs-lookup"><span data-stu-id="01687-138">From the **File** menu, select **New** and then select **Project**.</span></span>

<span data-ttu-id="01687-139">İçinde **şablonları** bölmesinde, **yüklü şablonlar** ve genişletin **Visual C#** düğümü.</span><span class="sxs-lookup"><span data-stu-id="01687-139">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="01687-140">Altında **Visual C#**seçin **Web**.</span><span class="sxs-lookup"><span data-stu-id="01687-140">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="01687-141">Proje şablonları listesinde seçin **ASP.NET MVC 4 Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="01687-141">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="01687-142">Proje adı &quot;BooksAPI&quot;.</span><span class="sxs-lookup"><span data-stu-id="01687-142">Name the project &quot;BooksAPI&quot;.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image1.png)

<span data-ttu-id="01687-143">İçinde **yeni ASP.NET projesi** iletişim kutusunda **boş** şablonu.</span><span class="sxs-lookup"><span data-stu-id="01687-143">In the **New ASP.NET Project** dialog, select the **Empty** template.</span></span> <span data-ttu-id="01687-144">"Klasörleri ekleyin ve çekirdek için başvurular" altında seçin **Web API** onay kutusu.</span><span class="sxs-lookup"><span data-stu-id="01687-144">Under "Add folders and core references for", select the **Web API** checkbox.</span></span> <span data-ttu-id="01687-145">Tıklatın **projesi oluşturma**.</span><span class="sxs-lookup"><span data-stu-id="01687-145">Click **Create Project**.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image2.png)

<span data-ttu-id="01687-146">Bu Web API işlevselliği için yapılandırılmış bir çatı projesini oluşturur.</span><span class="sxs-lookup"><span data-stu-id="01687-146">This creates a skeleton project that is configured for Web API functionality.</span></span>

### <a name="domain-models"></a><span data-ttu-id="01687-147">Etki alanı modeli</span><span class="sxs-lookup"><span data-stu-id="01687-147">Domain Models</span></span>

<span data-ttu-id="01687-148">Ardından, etki alanı modelleri için sınıfları ekleyin.</span><span class="sxs-lookup"><span data-stu-id="01687-148">Next, add classes for domain models.</span></span> <span data-ttu-id="01687-149">Çözüm Gezgini'nde modeller klasörü sağ tıklatın.</span><span class="sxs-lookup"><span data-stu-id="01687-149">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="01687-150">Seçin **Ekle**seçeneğini belirleyip **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="01687-150">Select **Add**, then select **Class**.</span></span> <span data-ttu-id="01687-151">Sınıf adını `Author`.</span><span class="sxs-lookup"><span data-stu-id="01687-151">Name the class `Author`.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image3.png)

<span data-ttu-id="01687-152">Author.cs kodu aşağıdakilerle değiştirin:</span><span class="sxs-lookup"><span data-stu-id="01687-152">Replace the code in Author.cs with the following:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample1.cs)]

<span data-ttu-id="01687-153">Şimdi adlı başka bir sınıf ekleyin `Book`.</span><span class="sxs-lookup"><span data-stu-id="01687-153">Now add another class named `Book`.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample2.cs)]

### <a name="add-a-web-api-controller"></a><span data-ttu-id="01687-154">Bir Web API denetleyicisi ekleme</span><span class="sxs-lookup"><span data-stu-id="01687-154">Add a Web API Controller</span></span>

<span data-ttu-id="01687-155">Bu adımda, veri katmanı olarak Entity Framework kullanan Web API denetleyicisi ekleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="01687-155">In this step, we'll add a Web API controller that uses Entity Framework as the data layer.</span></span>

<span data-ttu-id="01687-156">Projeyi oluşturmak için CTRL+SHIFT+B tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="01687-156">Press CTRL+SHIFT+B to build the project.</span></span> <span data-ttu-id="01687-157">Entity Framework yansıma veritabanı şeması oluşturmak derlenmiş bir bütünleştirilmiş kod gerektirdiği şekilde modellerinin özelliklerini bulmak için kullanır.</span><span class="sxs-lookup"><span data-stu-id="01687-157">Entity Framework uses reflection to discover the properties of the models, so it requires a compiled assembly to create the database schema.</span></span>

<span data-ttu-id="01687-158">Çözüm Gezgini'nde denetleyicileri klasörü sağ tıklatın.</span><span class="sxs-lookup"><span data-stu-id="01687-158">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="01687-159">Seçin **Ekle**seçeneğini belirleyip **denetleyicisi**.</span><span class="sxs-lookup"><span data-stu-id="01687-159">Select **Add**, then select **Controller**.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image4.png)

<span data-ttu-id="01687-160">İçinde **İskele Ekle** iletişim kutusunda "Web API 2 denetleyici Entity Framework kullanarak okuma/yazma eylemleri ile."</span><span class="sxs-lookup"><span data-stu-id="01687-160">In the **Add Scaffold** dialog, select "Web API 2 Controller with read/write actions, using Entity Framework."</span></span>

[![](create-a-rest-api-with-attribute-routing/_static/image6.png)](create-a-rest-api-with-attribute-routing/_static/image5.png)

<span data-ttu-id="01687-161">İçinde **denetleyici Ekle** iletişim için **Denetleyici adı**, girin &quot;BooksController&quot;.</span><span class="sxs-lookup"><span data-stu-id="01687-161">In the **Add Controller** dialog, for **Controller name**, enter &quot;BooksController&quot;.</span></span> <span data-ttu-id="01687-162">Seçin &quot;zaman uyumsuz denetleyici eylemlerini kullanmak&quot; onay kutusu.</span><span class="sxs-lookup"><span data-stu-id="01687-162">Select the &quot;Use async controller actions&quot; checkbox.</span></span> <span data-ttu-id="01687-163">İçin **Model sınıfı**seçin &quot;defteri&quot;.</span><span class="sxs-lookup"><span data-stu-id="01687-163">For **Model class**, select &quot;Book&quot;.</span></span> <span data-ttu-id="01687-164">(Görmüyorsanız `Book` sınıfı listelenen açılır listede, proje yerleşik emin olun.) Ardından "+" düğmesini tıklatın.</span><span class="sxs-lookup"><span data-stu-id="01687-164">(If you don't see the `Book` class listed in the dropdown, make sure that you built the project.) Then click the "+" button.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image7.png)

<span data-ttu-id="01687-165">Tıklatın **Ekle** içinde **yeni veri bağlamı** iletişim.</span><span class="sxs-lookup"><span data-stu-id="01687-165">Click **Add** in the **New Data Context** dialog.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image8.png)

<span data-ttu-id="01687-166">Tıklatın **Ekle** içinde **denetleyici Ekle** iletişim.</span><span class="sxs-lookup"><span data-stu-id="01687-166">Click **Add** in the **Add Controller** dialog.</span></span> <span data-ttu-id="01687-167">Yapı iskelesi adlı bir sınıf ekler `BooksController` API denetleyicisi tanımlar.</span><span class="sxs-lookup"><span data-stu-id="01687-167">The scaffolding adds a class named `BooksController` that defines the API controller.</span></span> <span data-ttu-id="01687-168">Ayrıca adlı bir sınıf ekler `BooksAPIContext` için Entity Framework veri bağlamı tanımlar modeller klasörü.</span><span class="sxs-lookup"><span data-stu-id="01687-168">It also adds a class named `BooksAPIContext` in the Models folder, which defines the data context for Entity Framework.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image9.png)

### <a name="seed-the-database"></a><span data-ttu-id="01687-169">Çekirdek veritabanı</span><span class="sxs-lookup"><span data-stu-id="01687-169">Seed the Database</span></span>

<span data-ttu-id="01687-170">Araçlar menüsünden seçin **kitaplık Paket Yöneticisi**ve ardından **Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="01687-170">From the Tools menu, select **Library Package Manager**, and then select **Package Manager Console**.</span></span>

<span data-ttu-id="01687-171">Paket Yöneticisi konsolu penceresinde aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="01687-171">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample3.ps1)]

<span data-ttu-id="01687-172">Bu komut Migrations klasörünü oluşturur ve Configuration.cs adlı yeni bir kod dosyası ekler.</span><span class="sxs-lookup"><span data-stu-id="01687-172">This command creates a Migrations folder and adds a new code file named Configuration.cs.</span></span> <span data-ttu-id="01687-173">Bu dosyayı açın ve aşağıdaki kodu ekleyin `Configuration.Seed` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="01687-173">Open this file and add the following code to the `Configuration.Seed` method.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample4.cs)]

<span data-ttu-id="01687-174">Paket Yöneticisi konsolu penceresinde aşağıdaki komutları yazın.</span><span class="sxs-lookup"><span data-stu-id="01687-174">In the Package Manager Console window, type the following commands.</span></span>

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample5.ps1)]

<span data-ttu-id="01687-175">Bu komutlar yerel bir veritabanı oluşturun ve veritabanını doldurmak için Seed yöntemi çağırır.</span><span class="sxs-lookup"><span data-stu-id="01687-175">These commands create a local database and invoke the Seed method to populate the database.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image10.png)

## <a name="add-dto-classes"></a><span data-ttu-id="01687-176">DTO sınıfları ekleme</span><span class="sxs-lookup"><span data-stu-id="01687-176">Add DTO Classes</span></span>

<span data-ttu-id="01687-177">Uygulamayı şimdi çalıştırmak ve /api/books/1 için bir GET isteği göndermek, yanıt aşağıdakine benzer durur.</span><span class="sxs-lookup"><span data-stu-id="01687-177">If you run the application now and send a GET request to /api/books/1, the response looks similar to the following.</span></span> <span data-ttu-id="01687-178">(Okunabilirlik için girinti ekledim.)</span><span class="sxs-lookup"><span data-stu-id="01687-178">(I added indentation for readability.)</span></span>

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample6.json)]

<span data-ttu-id="01687-179">Bunun yerine, bir alt kümesini alanları döndürmek için bu isteği istiyorum.</span><span class="sxs-lookup"><span data-stu-id="01687-179">Instead, I want this request to return a subset of the fields.</span></span> <span data-ttu-id="01687-180">Ayrıca, Yazar Kimliği yerine yazarın adını döndürmek için istediğim</span><span class="sxs-lookup"><span data-stu-id="01687-180">Also, I want it to return the author's name, rather than the author ID.</span></span> <span data-ttu-id="01687-181">Bunu başarmak için biz döndürmek için denetleyici yöntemi değiştireceksiniz bir *veri aktarım nesnesini* (DTO) yerine EF modeli.</span><span class="sxs-lookup"><span data-stu-id="01687-181">To accomplish this, we'll modify the controller methods to return a *data transfer object* (DTO) instead of the EF model.</span></span> <span data-ttu-id="01687-182">Bir DTO yalnızca veri taşımak için tasarlanmış bir nesnedir.</span><span class="sxs-lookup"><span data-stu-id="01687-182">A DTO is an object that is designed only to carry data.</span></span>

<span data-ttu-id="01687-183">Çözüm Gezgini'nde projeye sağ tıklayın ve seçin **Ekle** | **yeni klasör**.</span><span class="sxs-lookup"><span data-stu-id="01687-183">In Solution Explorer, right-click the project and select **Add** | **New Folder**.</span></span> <span data-ttu-id="01687-184">Klasör adı &quot;DTOs&quot;.</span><span class="sxs-lookup"><span data-stu-id="01687-184">Name the folder &quot;DTOs&quot;.</span></span> <span data-ttu-id="01687-185">Adlı bir sınıf ekleyin `BookDto` aşağıdaki tanımını içeren DTOs klasörü:</span><span class="sxs-lookup"><span data-stu-id="01687-185">Add a class named `BookDto` to the DTOs folder, with the following definition:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample7.cs)]

<span data-ttu-id="01687-186">Adlı başka bir sınıf ekleyin `BookDetailDto`.</span><span class="sxs-lookup"><span data-stu-id="01687-186">Add another class named `BookDetailDto`.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample8.cs)]

<span data-ttu-id="01687-187">Ardından, güncelleştirme `BooksController` dönmek için sınıf `BookDto` örnekleri.</span><span class="sxs-lookup"><span data-stu-id="01687-187">Next, update the `BooksController` class to return `BookDto` instances.</span></span> <span data-ttu-id="01687-188">Kullanacağız [Queryable.Select](https://msdn.microsoft.com/library/system.linq.queryable.select.aspx) yöntemi projesine `Book` için örnekleri `BookDto` örnekleri.</span><span class="sxs-lookup"><span data-stu-id="01687-188">We'll use the [Queryable.Select](https://msdn.microsoft.com/library/system.linq.queryable.select.aspx) method to project `Book` instances to `BookDto` instances.</span></span> <span data-ttu-id="01687-189">Denetleyici sınıfı için güncelleştirilmiş kod aşağıdaki gibidir.</span><span class="sxs-lookup"><span data-stu-id="01687-189">Here is the updated code for the controller class.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample9.cs)]

> [!NOTE]
> <span data-ttu-id="01687-190">I silinmiş `PutBook`, `PostBook`, ve `DeleteBook` yöntemleri, çünkü bu öğretici için gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="01687-190">I deleted the `PutBook`, `PostBook`, and `DeleteBook` methods, because they aren't needed for this tutorial.</span></span>


<span data-ttu-id="01687-191">Şimdi uygulamayı çalıştırın ve /api/books/1 istek, yanıt gövdesi aşağıdaki gibi görünmelidir:</span><span class="sxs-lookup"><span data-stu-id="01687-191">Now if you run the application and request /api/books/1, the response body should look like this:</span></span>

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample10.json)]

## <a name="add-route-attributes"></a><span data-ttu-id="01687-192">Rota öznitelikleri ekleme</span><span class="sxs-lookup"><span data-stu-id="01687-192">Add Route Attributes</span></span>

<span data-ttu-id="01687-193">Ardından, biz özniteliği yönlendirmeyi kullanmak için denetleyici dönüştürmeniz.</span><span class="sxs-lookup"><span data-stu-id="01687-193">Next, we'll convert the controller to use attribute routing.</span></span> <span data-ttu-id="01687-194">İlk olarak, ekleyin bir **routeprefix öğesi** özniteliği denetleyiciye.</span><span class="sxs-lookup"><span data-stu-id="01687-194">First, add a **RoutePrefix** attribute to the controller.</span></span> <span data-ttu-id="01687-195">Bu öznitelik bu denetleyicisinde tüm yöntemleri için ilk URI kesimleri tanımlar.</span><span class="sxs-lookup"><span data-stu-id="01687-195">This attribute defines the initial URI segments for all methods on this controller.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample11.cs?highlight=1)]

<span data-ttu-id="01687-196">Ardından ekleyin **[yol]** denetleyici eylemleri için aşağıdaki gibi öznitelikleri:</span><span class="sxs-lookup"><span data-stu-id="01687-196">Then add **[Route]** attributes to the controller actions, as follows:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample12.cs?highlight=1,7)]

<span data-ttu-id="01687-197">Her denetleyici yöntemi için rota şablonu için önekidir artı dizeyi belirtilen **rota** özniteliği.</span><span class="sxs-lookup"><span data-stu-id="01687-197">The route template for each controller method is the prefix plus the string specified in the **Route** attribute.</span></span> <span data-ttu-id="01687-198">İçin `GetBook` yöntemi, rota şablonu içerir tabanlı dizeye &quot;{kimliği: int}&quot;, URI segmenti bir tamsayı değeri içeriyorsa eşleşir.</span><span class="sxs-lookup"><span data-stu-id="01687-198">For the `GetBook` method, the route template includes the parameterized string &quot;{id:int}&quot;, which matches if the URI segment contains an integer value.</span></span>

| <span data-ttu-id="01687-199">Yöntem</span><span class="sxs-lookup"><span data-stu-id="01687-199">Method</span></span> | <span data-ttu-id="01687-200">Rota şablonu</span><span class="sxs-lookup"><span data-stu-id="01687-200">Route Template</span></span> | <span data-ttu-id="01687-201">Örnek URI</span><span class="sxs-lookup"><span data-stu-id="01687-201">Example URI</span></span> |
| --- | --- | --- |
| `GetBooks` | <span data-ttu-id="01687-202">"API/books"</span><span class="sxs-lookup"><span data-stu-id="01687-202">"api/books"</span></span> | `http://localhost/api/books` |
| `GetBook` | <span data-ttu-id="01687-203">"api/books/{id:int}"</span><span class="sxs-lookup"><span data-stu-id="01687-203">"api/books/{id:int}"</span></span> | `http://localhost/api/books/5` |

## <a name="get-book-details"></a><span data-ttu-id="01687-204">Defteri ayrıntıları alma</span><span class="sxs-lookup"><span data-stu-id="01687-204">Get Book Details</span></span>

<span data-ttu-id="01687-205">Defteri ayrıntıları almak için istemci bir GET isteği gönderir `/api/books/{id}/details`, burada *{id}* defteri kimliğidir.</span><span class="sxs-lookup"><span data-stu-id="01687-205">To get book details, the client will send a GET request to `/api/books/{id}/details`, where *{id}* is the ID of the book.</span></span>

<span data-ttu-id="01687-206">Aşağıdaki yöntemi ekleyin `BooksController` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="01687-206">Add the following method to the `BooksController` class.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample13.cs)]

<span data-ttu-id="01687-207">Eğer istenmişse `/api/books/1/details`, yanıtı şuna benzer:</span><span class="sxs-lookup"><span data-stu-id="01687-207">If you request `/api/books/1/details`, the response looks like this:</span></span>

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample14.json)]

## <a name="get-books-by-genre"></a><span data-ttu-id="01687-208">Türe göre Books Al</span><span class="sxs-lookup"><span data-stu-id="01687-208">Get Books By Genre</span></span>

<span data-ttu-id="01687-209">İçinde belirli bir tarzını books listesini almak için istemci bir GET isteği gönderir `/api/books/genre`, burada *Tarz* Tarz adıdır.</span><span class="sxs-lookup"><span data-stu-id="01687-209">To get a list of books in a specific genre, the client will send a GET request to `/api/books/genre`, where *genre* is the name of the genre.</span></span> <span data-ttu-id="01687-210">(Örneğin, `/get/books/fantasy`.)</span><span class="sxs-lookup"><span data-stu-id="01687-210">(For example, `/get/books/fantasy`.)</span></span>

<span data-ttu-id="01687-211">Aşağıdaki yöntemi ekleyin `BooksController`.</span><span class="sxs-lookup"><span data-stu-id="01687-211">Add the following method to `BooksController`.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample15.cs)]

<span data-ttu-id="01687-212">Burada size URI şablonunu {Tarz} parametre içeren bir yol tanımlıyorsanız.</span><span class="sxs-lookup"><span data-stu-id="01687-212">Here we are defining a route that contains a {genre} parameter in the URI template.</span></span> <span data-ttu-id="01687-213">Web API bu iki URI'ler ayırt etmek ve için farklı yöntemler rota mümkün olduğuna dikkat edin:</span><span class="sxs-lookup"><span data-stu-id="01687-213">Notice that Web API is able to distinguish these two URIs and route them to different methods:</span></span>

`/api/books/1`

`/api/books/fantasy`

<span data-ttu-id="01687-214">Çünkü `GetBook` yöntemi "id" segmenti bir tamsayı değeri olmalıdır kısıtlama içerir:</span><span class="sxs-lookup"><span data-stu-id="01687-214">That's because the `GetBook` method includes a constraint that the "id" segment must be an integer value:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample16.cs?highlight=1)]

<span data-ttu-id="01687-215">/Api/books/fantasy istek, yanıt şöyle görünür:</span><span class="sxs-lookup"><span data-stu-id="01687-215">If you request /api/books/fantasy, the response looks like this:</span></span>

`[ { "Title": "Midnight Rain", "Author": "Ralls, Kim", "Genre": "Fantasy" }, { "Title": "Maeve Ascendant", "Author": "Corets, Eva", "Genre": "Fantasy" }, { "Title": "The Sundered Grail", "Author": "Corets, Eva", "Genre": "Fantasy" } ]`

## <a name="get-books-by-author"></a><span data-ttu-id="01687-216">Yazar tarafından Books Al</span><span class="sxs-lookup"><span data-stu-id="01687-216">Get Books By Author</span></span>

<span data-ttu-id="01687-217">İçin belirli bir yazarın bir books listesini almak için istemci bir GET isteği gönderir `/api/authors/id/books`, burada *kimliği* Yazar kimliğidir.</span><span class="sxs-lookup"><span data-stu-id="01687-217">To get a list of a books for a particular author, the client will send a GET request to `/api/authors/id/books`, where *id* is the ID of the author.</span></span>

<span data-ttu-id="01687-218">Aşağıdaki yöntemi ekleyin `BooksController`.</span><span class="sxs-lookup"><span data-stu-id="01687-218">Add the following method to `BooksController`.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample17.cs)]

<span data-ttu-id="01687-219">Bu örnekte ilginç, çünkü &quot;books&quot; olan bir alt kaynağın kabul &quot;yazarlar&quot;.</span><span class="sxs-lookup"><span data-stu-id="01687-219">This example is interesting because &quot;books&quot; is treated a child resource of &quot;authors&quot;.</span></span> <span data-ttu-id="01687-220">Bu desen RESTful API'leri oldukça yaygındır.</span><span class="sxs-lookup"><span data-stu-id="01687-220">This pattern is quite common in RESTful APIs.</span></span>

<span data-ttu-id="01687-221">Rota şablonu içinde tilde (~) rota öneki geçersiz kılmaları **routeprefix öğesi** özniteliği.</span><span class="sxs-lookup"><span data-stu-id="01687-221">The tilde (~) in the route template overrides the route prefix in the **RoutePrefix** attribute.</span></span>

## <a name="get-books-by-publication-date"></a><span data-ttu-id="01687-222">Yayın tarihe göre Books Al</span><span class="sxs-lookup"><span data-stu-id="01687-222">Get Books By Publication Date</span></span>

<span data-ttu-id="01687-223">Yayın tarihe göre books listesini almak için istemci bir GET isteği gönderir `/api/books/date/yyyy-mm-dd`, burada *yyyy-aa-gg* tarihidir.</span><span class="sxs-lookup"><span data-stu-id="01687-223">To get a list of books by publication date, the client will send a GET request to `/api/books/date/yyyy-mm-dd`, where *yyyy-mm-dd* is the date.</span></span>

<span data-ttu-id="01687-224">Bunu yapmanın bir yolu şöyledir:</span><span class="sxs-lookup"><span data-stu-id="01687-224">Here is one way to do this:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample18.cs)]

<span data-ttu-id="01687-225">`{pubdate:datetime}` Eşleştirmek için parametre kısıtlı bir **DateTime** değeri.</span><span class="sxs-lookup"><span data-stu-id="01687-225">The `{pubdate:datetime}` parameter is constrained to match a **DateTime** value.</span></span> <span data-ttu-id="01687-226">Bu çalışır, ancak isteriz daha gerçekten daha fazla izin veren.</span><span class="sxs-lookup"><span data-stu-id="01687-226">This works, but it's actually more permissive than we'd like.</span></span> <span data-ttu-id="01687-227">Örneğin, bu URI rota eşleşir:</span><span class="sxs-lookup"><span data-stu-id="01687-227">For example, these URIs will also match the route:</span></span>

`/api/books/date/Thu, 01 May 2008`

`/api/books/date/2000-12-16T00:00:00`

<span data-ttu-id="01687-228">Bu URI izin vermekle yanlış bir şey yoktur.</span><span class="sxs-lookup"><span data-stu-id="01687-228">There's nothing wrong with allowing these URIs.</span></span> <span data-ttu-id="01687-229">Ancak, rota şablonu için bir normal ifade kısıtlaması ekleyerek belirli bir biçimde rota kısıtlayabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="01687-229">However, you can restrict the route to a particular format by adding a regular-expression constraint to the route template:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample19.cs?highlight=1)]

<span data-ttu-id="01687-230">Artık yalnızca tarih biçiminde &quot;yyyy-aa-gg&quot; ile eşleşir.</span><span class="sxs-lookup"><span data-stu-id="01687-230">Now only dates in the form &quot;yyyy-mm-dd&quot; will match.</span></span> <span data-ttu-id="01687-231">Biz regex gerçek tarih elde ettiğinizi doğrulamak için kullanmayın dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="01687-231">Notice that we don't use the regex to validate that we got a real date.</span></span> <span data-ttu-id="01687-232">Web API uygulamasına URI segmenti dönüştürmek çalıştığında işlenmiş bir **DateTime** örneği.</span><span class="sxs-lookup"><span data-stu-id="01687-232">That is handled when Web API tries to convert the URI segment into a **DateTime** instance.</span></span> <span data-ttu-id="01687-233">Geçersiz bir tarih gibi ' 2012-47-99' başarısız olur dönüştürülecek ve istemci 404 hatası alırsınız.</span><span class="sxs-lookup"><span data-stu-id="01687-233">An invalid date such as '2012-47-99' will fail to be converted, and the client will get a 404 error.</span></span>

<span data-ttu-id="01687-234">Ayrıca bir eğik çizgi ayırıcı destekleyebilir (`/api/books/date/yyyy/mm/dd`) başka bir ekleyerek **[yol]** farklı bir regex özniteliğiyle.</span><span class="sxs-lookup"><span data-stu-id="01687-234">You can also support a slash separator (`/api/books/date/yyyy/mm/dd`) by adding another **[Route]** attribute with a different regex.</span></span>

[!code-html[Main](create-a-rest-api-with-attribute-routing/samples/sample20.html)]

<span data-ttu-id="01687-235">Bir ince ama önemli burada ayrıntılı yoktur.</span><span class="sxs-lookup"><span data-stu-id="01687-235">There is a subtle but important detail here.</span></span> <span data-ttu-id="01687-236">İkinci yol şablonu olan bir joker karakter (\*) {pubdate} parametresi başlangıcında:</span><span class="sxs-lookup"><span data-stu-id="01687-236">The second route template has a wildcard character (\*) at the start of the {pubdate} parameter:</span></span>

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample21.json)]

<span data-ttu-id="01687-237">Bu, Yönlendirme altyapısı bu {pubdate} rest URI'sinin eşleşmesi gereken bildirir.</span><span class="sxs-lookup"><span data-stu-id="01687-237">This tells the routing engine that {pubdate} should match the rest of the URI.</span></span> <span data-ttu-id="01687-238">Varsayılan olarak, bir şablon parametresini tek bir URI segmenti eşleşir.</span><span class="sxs-lookup"><span data-stu-id="01687-238">By default, a template parameter matches a single URI segment.</span></span> <span data-ttu-id="01687-239">Bu durumda, {pubdate} birkaç URI kesimleri span istiyoruz:</span><span class="sxs-lookup"><span data-stu-id="01687-239">In this case, we want {pubdate} to span several URI segments:</span></span>

`/api/books/date/2013/06/17`

## <a name="controller-code"></a><span data-ttu-id="01687-240">Denetleyici kodu</span><span class="sxs-lookup"><span data-stu-id="01687-240">Controller Code</span></span>

<span data-ttu-id="01687-241">BooksController sınıfı için tam kod aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="01687-241">Here is the complete code for the BooksController class.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample22.cs)]

## <a name="summary"></a><span data-ttu-id="01687-242">Özet</span><span class="sxs-lookup"><span data-stu-id="01687-242">Summary</span></span>

<span data-ttu-id="01687-243">Öznitelik yönlendirme, daha fazla denetim ve esneklik API'nizi URI'ler tasarlarken sunar.</span><span class="sxs-lookup"><span data-stu-id="01687-243">Attribute routing gives you more control and greater flexibility when designing the URIs for your API.</span></span>
