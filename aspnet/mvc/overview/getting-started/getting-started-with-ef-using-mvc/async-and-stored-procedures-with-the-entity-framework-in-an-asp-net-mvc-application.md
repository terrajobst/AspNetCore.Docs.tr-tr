---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application
title: 'Öğretici: Bir ASP.NET MVC uygulamasında EF ile zaman uyumsuz ve saklı yordamlar kullanma'
description: Bu öğreticide, zaman uyumsuz programlama modeli uygulamak ve saklı yordamlar kullanma konusunda bilgi almak nasıl görürsünüz.
author: tdykstra
ms.author: riande
ms.date: 01/18/2019
ms.topic: tutorial
ms.assetid: 27d110fc-d1b7-4628-a763-26f1e6087549
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 9041167af076d80ebf294e054ffe51293d11e888
ms.sourcegitcommit: d5223cf6a2cf80b4f5dc54169b0e376d493d2d3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54889879"
---
# <a name="tutorial-use-async-and-stored-procedures-with-ef-in-an-aspnet-mvc-app"></a><span data-ttu-id="fa583-103">Öğretici: Bir ASP.NET MVC uygulamasında EF ile zaman uyumsuz ve saklı yordamlar kullanma</span><span class="sxs-lookup"><span data-stu-id="fa583-103">Tutorial: Use async and stored procedures with EF in an ASP.NET MVC App</span></span>

<span data-ttu-id="fa583-104">Önceki öğreticilerde okumasına ve güncelleştirmesine eşzamanlı programlama modeli kullanarak verileri öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="fa583-104">In earlier tutorials you learned how to read and update data using the synchronous programming model.</span></span> <span data-ttu-id="fa583-105">Bu öğreticide, zaman uyumsuz programlama modeli nasıl göreceksiniz.</span><span class="sxs-lookup"><span data-stu-id="fa583-105">In this tutorial you see how to implement the asynchronous programming model.</span></span> <span data-ttu-id="fa583-106">Zaman uyumsuz kod, uygulamanın daha iyi server kaynakları kullanmayı kolaylaştırdığından daha iyi performans yardımcı olabilir.</span><span class="sxs-lookup"><span data-stu-id="fa583-106">Asynchronous code can help an application perform better because it makes better use of server resources.</span></span>

<span data-ttu-id="fa583-107">Bu öğreticide ekleme, güncelleştirme ve silme işlemleri bir varlıkta saklı yordamlar kullanma de görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fa583-107">In this tutorial you also see how to use stored procedures for insert, update, and delete operations on an entity.</span></span>

<span data-ttu-id="fa583-108">Son olarak, uygulamayı azure'a tüm dağıttıysanız bu yana ilk kez uyguladık veritabanı değişikliklerinin yanı sıra yeniden dağıtın.</span><span class="sxs-lookup"><span data-stu-id="fa583-108">Finally, you redeploy the application to Azure, along with all of the database changes that you've implemented since the first time you deployed.</span></span>

<span data-ttu-id="fa583-109">Aşağıdaki çizimler ile çalışma sayfaları bazılarını göstermektedir.</span><span class="sxs-lookup"><span data-stu-id="fa583-109">The following illustrations show some of the pages that you'll work with.</span></span>

![Departmanlar Sayfası](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Bölüm oluşturma](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

<span data-ttu-id="fa583-112">Bu öğreticide şunları yaptınız:</span><span class="sxs-lookup"><span data-stu-id="fa583-112">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="fa583-113">Zaman uyumsuz kod hakkında bilgi edinin</span><span class="sxs-lookup"><span data-stu-id="fa583-113">Learn about asynchronous code</span></span>
> * <span data-ttu-id="fa583-114">Bir bölüm denetleyicisi oluşturun</span><span class="sxs-lookup"><span data-stu-id="fa583-114">Create a Department controller</span></span>
> * <span data-ttu-id="fa583-115">Saklı yordamlar kullanma</span><span class="sxs-lookup"><span data-stu-id="fa583-115">Use stored procedures</span></span>
> * <span data-ttu-id="fa583-116">Azure’a dağıtma</span><span class="sxs-lookup"><span data-stu-id="fa583-116">Deploy to Azure</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fa583-117">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="fa583-117">Prerequisites</span></span>

* [<span data-ttu-id="fa583-118">İlgili Verileri Güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="fa583-118">Updating Related Data</span></span>](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="why-use-asynchronous-code"></a><span data-ttu-id="fa583-119">Zaman uyumsuz kodu neden kullanın</span><span class="sxs-lookup"><span data-stu-id="fa583-119">Why use asynchronous code</span></span>

<span data-ttu-id="fa583-120">Sınırlı sayıda iş parçacığı kullanılabilir bir web sunucusuna sahip ve yüksek yük durumlarda tüm kullanılabilir iş parçacıklarının kullanımda olabilir.</span><span class="sxs-lookup"><span data-stu-id="fa583-120">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="fa583-121">Bu durum oluştuğunda, sunucunun iş parçacıklarının serbest bırakılana kadar yeni istekleri işleyemiyor.</span><span class="sxs-lookup"><span data-stu-id="fa583-121">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="fa583-122">G/ç tamamlanması bekleniyor çünkü bunlar herhangi bir iş gerçekten yapmamanız sırasında eş zamanlı kod ile birçok iş parçacığı bağlanması.</span><span class="sxs-lookup"><span data-stu-id="fa583-122">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="fa583-123">İşlemi tamamlamak, g/ç için beklerken zaman uyumsuz kod ile diğer istekleri işlemek için kullanılacak sunucuyu için kendi iş parçacığı serbest bırakılır.</span><span class="sxs-lookup"><span data-stu-id="fa583-123">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="fa583-124">Sonuç olarak, zaman uyumsuz kod, sunucu kaynaklarını daha verimli bir şekilde kullanmasına ve sunucu gecikmeler olmadan daha fazla trafik işlemek için etkin olmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="fa583-124">As a result, asynchronous code enables server resources to be use more efficiently, and the server is enabled to handle more traffic without delays.</span></span>

<span data-ttu-id="fa583-125">.NET önceki sürümlerinde, yazma ve zaman uyumsuz kod test karmaşık, hata yapmaya açık ve hata ayıklamak sabit.</span><span class="sxs-lookup"><span data-stu-id="fa583-125">In earlier versions of .NET, writing and testing asynchronous code was complex, error prone, and hard to debug.</span></span> <span data-ttu-id="fa583-126">.NET 4.5 içinde yazma, test ve zaman uyumsuz kod hata ayıklama için bir nedeniniz yoksa zaman uyumsuz kod genellikle yazmalısınız çok daha kolay olur.</span><span class="sxs-lookup"><span data-stu-id="fa583-126">In .NET 4.5, writing, testing, and debugging asynchronous code is so much easier that you should generally write asynchronous code unless you have a reason not to.</span></span> <span data-ttu-id="fa583-127">Zaman uyumsuz kod az miktarda bir ek yükü sağladıkları gerektirmez, ancak olası performans artışını uğradığı performans düşüşünü yüksek trafik durumlar için göz ardı edilebilir, çalışırken, düşük trafiğe durumlar için önemli.</span><span class="sxs-lookup"><span data-stu-id="fa583-127">Asynchronous code does introduce a small amount of overhead, but for low traffic situations the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="fa583-128">Zaman uyumsuz programlama hakkında daha fazla bilgi için bkz. [çağrıları engellemekten kaçınacak şekilde kullanım .NET 4.5'ın zaman uyumsuz Destek](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices.md#async).</span><span class="sxs-lookup"><span data-stu-id="fa583-128">For more information about asynchronous programming, see [Use .NET 4.5's async support to avoid blocking calls](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices.md#async).</span></span>

## <a name="create-department-controller"></a><span data-ttu-id="fa583-129">Departman denetleyicisi oluşturma</span><span class="sxs-lookup"><span data-stu-id="fa583-129">Create Department controller</span></span>

<span data-ttu-id="fa583-130">Bu süre dışında önceki denetleyicileri yaptığınız gibi seçin bir bölümü denetleyicisi oluşturmak **zaman uyumsuz denetleyici eylemlerini kullanmak** onay kutusu.</span><span class="sxs-lookup"><span data-stu-id="fa583-130">Create a Department controller the same way you did the earlier controllers, except this time select the **Use async controller actions** check box.</span></span>

<span data-ttu-id="fa583-131">Ne zaman uyumlu koda eklenmiş olan aşağıdaki önemli özelliklerle Göster `Index` yöntemini zaman uyumsuz sağlamak için:</span><span class="sxs-lookup"><span data-stu-id="fa583-131">The following highlights show what was added to the synchronous code for the `Index` method to make it asynchronous:</span></span>

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=1,4)]

<span data-ttu-id="fa583-132">Zaman uyumsuz olarak yürütmek Entity Framework veritabanı sorgusu etkinleştirmek için dört değişiklikleri uygulandı:</span><span class="sxs-lookup"><span data-stu-id="fa583-132">Four changes were applied to enable the Entity Framework database query to execute asynchronously:</span></span>

- <span data-ttu-id="fa583-133">Yöntem ile işaretlenmiş `async` derleyiciye geri çağırmaları yöntem gövdesini bölümleri için oluşturulacak ve otomatik olarak oluşturmak için anahtar sözcüğü `Task<ActionResult>` döndürülen nesne.</span><span class="sxs-lookup"><span data-stu-id="fa583-133">The method is marked with the `async` keyword, which tells the compiler to generate callbacks for parts of the method body and to automatically create the `Task<ActionResult>` object that is returned.</span></span>
- <span data-ttu-id="fa583-134">Dönüş türü değiştirildi `ActionResult` için `Task<ActionResult>`.</span><span class="sxs-lookup"><span data-stu-id="fa583-134">The return type was changed from `ActionResult` to `Task<ActionResult>`.</span></span> <span data-ttu-id="fa583-135">`Task<T>` Türünü temsil eden bir sonuç türü ile devam eden çalışma `T`.</span><span class="sxs-lookup"><span data-stu-id="fa583-135">The `Task<T>` type represents ongoing work with a result of type `T`.</span></span>
- <span data-ttu-id="fa583-136">`await` Anahtar sözcüğü, web hizmeti çağrısı uygulandı.</span><span class="sxs-lookup"><span data-stu-id="fa583-136">The `await` keyword was applied to the web service call.</span></span> <span data-ttu-id="fa583-137">Derleyici bu anahtar sözcük gördüğünde arka planda bu yöntemin iki bölüme ayırır.</span><span class="sxs-lookup"><span data-stu-id="fa583-137">When the compiler sees this keyword, behind the scenes it splits the method into two parts.</span></span> <span data-ttu-id="fa583-138">İlk bölüm ile zaman uyumsuz olarak başlatıldığında işlemi sonlandırır.</span><span class="sxs-lookup"><span data-stu-id="fa583-138">The first part ends with the operation that is started asynchronously.</span></span> <span data-ttu-id="fa583-139">İkinci bölümü, işlemi tamamlandıktan sonra çağrılan bir geri çağırma yöntemi yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="fa583-139">The second part is put into a callback method that is called when the operation completes.</span></span>
- <span data-ttu-id="fa583-140">Zaman uyumsuz sürümü `ToList` genişletme yöntemi çağrıldı.</span><span class="sxs-lookup"><span data-stu-id="fa583-140">The asynchronous version of the `ToList` extension method was called.</span></span>

<span data-ttu-id="fa583-141">Neden olduğu `departments.ToList` deyimi değiştirilmiş ancak `departments = db.Departments` deyimi?</span><span class="sxs-lookup"><span data-stu-id="fa583-141">Why is the `departments.ToList` statement modified but not the `departments = db.Departments` statement?</span></span> <span data-ttu-id="fa583-142">Sorguları veya veritabanına gönderilecek komutları neden deyimleri zaman uyumsuz olarak yürütülür nedenidir.</span><span class="sxs-lookup"><span data-stu-id="fa583-142">The reason is that only statements that cause queries or commands to be sent to the database are executed asynchronously.</span></span> <span data-ttu-id="fa583-143">`departments = db.Departments` Sorgu deyimi ayarlar ancak sorgu kadar yürütülmez `ToList` yöntemi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="fa583-143">The `departments = db.Departments` statement sets up a query but the query is not executed until the `ToList` method is called.</span></span> <span data-ttu-id="fa583-144">Bu nedenle, yalnızca `ToList` yöntemi zaman uyumsuz olarak yürütülür.</span><span class="sxs-lookup"><span data-stu-id="fa583-144">Therefore, only the `ToList` method is executed asynchronously.</span></span>

<span data-ttu-id="fa583-145">İçinde `Details` yöntemi ve `HttpGet` `Edit` ve `Delete` yöntemleri `Find` , zaman uyumsuz olarak yürütülen yöntemi, bu nedenle veritabanına gönderilmek üzere bir sorgu neden olan bir yöntemdir:</span><span class="sxs-lookup"><span data-stu-id="fa583-145">In the `Details` method and the `HttpGet` `Edit` and `Delete` methods, the `Find` method is the one that causes a query to be sent to the database, so that's the method that gets executed asynchronously:</span></span>

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs?highlight=7)]

<span data-ttu-id="fa583-146">İçinde `Create`, `HttpPost Edit`, ve `DeleteConfirmed` yöntemleri olduğu `SaveChanges` yürütülecek bir komut neden olan yöntem çağrısı, gibi deyimler `db.Departments.Add(department)` yalnızca neden varlıkları bellekte değiştirilecek.</span><span class="sxs-lookup"><span data-stu-id="fa583-146">In the `Create`, `HttpPost Edit`, and `DeleteConfirmed` methods, it is the `SaveChanges` method call that causes a command to be executed, not statements such as `db.Departments.Add(department)` which only cause entities in memory to be modified.</span></span>

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs?highlight=6)]

<span data-ttu-id="fa583-147">Açık *Views\Department\Index.cshtml*, şablonu kodu aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="fa583-147">Open *Views\Department\Index.cshtml*, and replace the template code with the following code:</span></span>

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml?highlight=3,5,20-22,36-38)]

<span data-ttu-id="fa583-148">Bu kod başlığı dizinden Departmanlar için değişiklikleri yönetici adı sağa taşır ve yöneticinin tam adı sağlar.</span><span class="sxs-lookup"><span data-stu-id="fa583-148">This code changes the title from Index to Departments, moves the Administrator name to the right, and provides the full name of the administrator.</span></span>

<span data-ttu-id="fa583-149">Oluşturma, silme, Ayrıntılar ve düzenleme görünümleri, başlığını değiştirme `InstructorID` "Yönetici" değiştirdiğiniz departmanı ad alanı "Departman" için kursu görünümlerinde aynı şekilde alanı.</span><span class="sxs-lookup"><span data-stu-id="fa583-149">In the Create, Delete, Details, and Edit views, change the caption for the `InstructorID` field to "Administrator" the same way you changed the department name field to "Department" in the Course views.</span></span>

<span data-ttu-id="fa583-150">Oluşturma ve düzenleme görünümleri aşağıdaki kodu kullanın:</span><span class="sxs-lookup"><span data-stu-id="fa583-150">In the Create and Edit views use the following code:</span></span>

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml)]

<span data-ttu-id="fa583-151">Silme ve ayrıntıları görünümlerinde aşağıdaki kodu kullanın:</span><span class="sxs-lookup"><span data-stu-id="fa583-151">In the Delete and Details views use the following code:</span></span>

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml)]

<span data-ttu-id="fa583-152">Uygulamayı çalıştırın ve **Departmanlar** sekmesi.</span><span class="sxs-lookup"><span data-stu-id="fa583-152">Run the application, and click the **Departments** tab.</span></span>

<span data-ttu-id="fa583-153">Her şey aynı diğer denetleyicilerin olduğu gibi çalışır, ancak bu denetleyicide tüm SQL sorguları zaman uyumsuz olarak yürütülen.</span><span class="sxs-lookup"><span data-stu-id="fa583-153">Everything works the same as in the other controllers, but in this controller all of the SQL queries are executing asynchronously.</span></span>

<span data-ttu-id="fa583-154">Zaman uyumsuz programlama Entity Framework ile kullandığınızda dikkat edilecek bazı noktalar:</span><span class="sxs-lookup"><span data-stu-id="fa583-154">Some things to be aware of when you are using asynchronous programming with the Entity Framework:</span></span>

- <span data-ttu-id="fa583-155">Zaman uyumsuz kod, iş parçacığı güvenli değildir.</span><span class="sxs-lookup"><span data-stu-id="fa583-155">The async code is not thread safe.</span></span> <span data-ttu-id="fa583-156">Diğer bir deyişle, diğer bir deyişle, aynı bağlam örneğini kullanarak işlemi paralel olarak birden çok işlem yapmak çalışmayın.</span><span class="sxs-lookup"><span data-stu-id="fa583-156">In other words, in other words, don't try to do multiple operations in parallel using the same context instance.</span></span>
- <span data-ttu-id="fa583-157">Zaman uyumsuz kodun performans avantajlarından yararlanmak istiyorsanız herhangi bir kitaplığı paketleri emin olun (örneğin, disk belleği) kullanıyorsanız, bunlar sorgular veritabanına neden herhangi bir Entity Framework yöntem çağırırsanız zaman uyumsuz de kullanın.</span><span class="sxs-lookup"><span data-stu-id="fa583-157">If you want to take advantage of the performance benefits of async code, make sure that any library packages that you're using (such as for paging), also use async if they call any Entity Framework methods that cause queries to be sent to the database.</span></span>

## <a name="use-stored-procedures"></a><span data-ttu-id="fa583-158">Saklı yordamlar kullanma</span><span class="sxs-lookup"><span data-stu-id="fa583-158">Use stored procedures</span></span>

<span data-ttu-id="fa583-159">Bazı geliştiriciler ve Dba'lar veritabanı erişimi için saklı yordamlar'ı kullanmayı tercih eder.</span><span class="sxs-lookup"><span data-stu-id="fa583-159">Some developers and DBAs prefer to use stored procedures for database access.</span></span> <span data-ttu-id="fa583-160">Entity Framework'ün önceki sürümlerinde bir saklı yordam tarafından kullanarak verileri alabilir [ham bir SQL sorgusu yürütme](advanced-entity-framework-scenarios-for-an-mvc-web-application.md), ancak depolanan yordamları güncelleştirme işlemleri için kullanılacak EF toplamasını olamaz.</span><span class="sxs-lookup"><span data-stu-id="fa583-160">In earlier versions of Entity Framework you can retrieve data using a stored procedure by [executing a raw SQL query](advanced-entity-framework-scenarios-for-an-mvc-web-application.md), but you can't instruct EF to use stored procedures for update operations.</span></span> <span data-ttu-id="fa583-161">EF 6'da Code First saklı yordamları kullanmak için yapılandırmak kolaydır.</span><span class="sxs-lookup"><span data-stu-id="fa583-161">In EF 6 it's easy to configure Code First to use stored procedures.</span></span>

1. <span data-ttu-id="fa583-162">İçinde *DAL\SchoolContext.cs*, vurgulanmış kodu ekleyin `OnModelCreating` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="fa583-162">In *DAL\SchoolContext.cs*, add the highlighted code to the `OnModelCreating` method.</span></span>

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=9)]

    <span data-ttu-id="fa583-163">Bu kod, INSERT saklı yordamlara Entity Framework bildirir. güncelleştirme ve silme işlemleri üzerinde `Department` varlık.</span><span class="sxs-lookup"><span data-stu-id="fa583-163">This code instructs Entity Framework to use stored procedures for insert, update, and delete operations on the `Department` entity.</span></span>
2. <span data-ttu-id="fa583-164">Paket yönetme konsolunda aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="fa583-164">In Package Manage Console, enter the following command:</span></span>

    `add-migration DepartmentSP`

    <span data-ttu-id="fa583-165">Açık *geçişler\&lt; zaman damgası&gt;\_DepartmentSP.cs* kodu görmek için `Up` oluşturan yöntemine INSERT, Update ve Delete saklı yordamlar:</span><span class="sxs-lookup"><span data-stu-id="fa583-165">Open *Migrations\&lt;timestamp&gt;\_DepartmentSP.cs* to see the code in the `Up` method that creates Insert, Update, and Delete stored procedures:</span></span>

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs?highlight=3-4,26-27,42-43)]
3. <span data-ttu-id="fa583-166">Paket yönetme konsolunda aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="fa583-166">In Package Manage Console, enter the following command:</span></span>

     `update-database`
4. <span data-ttu-id="fa583-167">Uygulamayı hata ayıklama modunda çalıştırabilir, tıklayın **Departmanlar** sekmesine ve ardından **Yeni Oluştur**.</span><span class="sxs-lookup"><span data-stu-id="fa583-167">Run the application in debug mode, click the **Departments** tab, and then click **Create New**.</span></span>
5. <span data-ttu-id="fa583-168">Yeni bir bölüm için veri girin ve ardından **Oluştur**.</span><span class="sxs-lookup"><span data-stu-id="fa583-168">Enter data for a new department, and then click **Create**.</span></span>

6. <span data-ttu-id="fa583-169">Visual Studio'da günlüklerine bakın **çıkış** yeni bölüm satır eklemek için bir saklı yordam kullanıldığını görmek için pencere.</span><span class="sxs-lookup"><span data-stu-id="fa583-169">In Visual Studio, look at the logs in the **Output** window to see that a stored procedure was used to insert the new Department row.</span></span>

     ![Bölüm Ekle SP](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

<span data-ttu-id="fa583-171">Kod öncelikle varsayılan depolanmış yordam adları oluşturur.</span><span class="sxs-lookup"><span data-stu-id="fa583-171">Code First creates default stored procedure names.</span></span> <span data-ttu-id="fa583-172">Varolan bir veritabanını kullanıyorsanız, zaten veritabanında tanımlı saklı yordamları kullanmak için depolanmış yordam adları özelleştirmek gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="fa583-172">If you are using an existing database, you might need to customize the stored procedure names in order to use stored procedures already defined in the database.</span></span> <span data-ttu-id="fa583-173">Bunu nasıl yapacağınız hakkında daha fazla bilgi için bkz. [Entity Framework kod ilk Insert/Update/Delete saklı yordamlar](https://msdn.microsoft.com/data/dn468673).</span><span class="sxs-lookup"><span data-stu-id="fa583-173">For information about how to do that, see [Entity Framework Code First Insert/Update/Delete Stored Procedures](https://msdn.microsoft.com/data/dn468673).</span></span>

<span data-ttu-id="fa583-174">Saklı yordamları ne oluşturulan özelleştirmek istiyorsanız, geçişler için iskele kurulan kodu düzenleyebileceğiniz `Up` saklı yordamı oluşturan yöntemi.</span><span class="sxs-lookup"><span data-stu-id="fa583-174">If you want to customize what generated stored procedures do, you can edit the scaffolded code for the migrations `Up` method that creates the stored procedure.</span></span> <span data-ttu-id="fa583-175">Böylece geçiş çalıştırılır ve geçişleri çalıştırdığında, otomatik dağıtımdan sonra üretim ortamında, üretim veritabanına uygulanır değişikliklerinizi her yansıtılır.</span><span class="sxs-lookup"><span data-stu-id="fa583-175">That way your changes are reflected whenever that migration is run and will be applied to your production database when migrations runs automatically in production after deployment.</span></span>

<span data-ttu-id="fa583-176">Önceki bir geçiş içinde oluşturulan varolan bir saklı yordam değiştirmek istiyorsanız, boş bir geçiş oluşturmak için Add-geçiş komutunu kullanın ve çağıran kodu el ile yazma [AlterStoredProcedure](https://msdn.microsoft.com/library/system.data.entity.migrations.dbmigration.alterstoredprocedure.aspx) yöntemi .</span><span class="sxs-lookup"><span data-stu-id="fa583-176">If you want to change an existing stored procedure that was created in a previous migration, you can use the Add-Migration command to generate a blank migration, and then manually write code that calls the [AlterStoredProcedure](https://msdn.microsoft.com/library/system.data.entity.migrations.dbmigration.alterstoredprocedure.aspx) method.</span></span>

## <a name="deploy-to-azure"></a><span data-ttu-id="fa583-177">Azure’a dağıtma</span><span class="sxs-lookup"><span data-stu-id="fa583-177">Deploy to Azure</span></span>

<span data-ttu-id="fa583-178">Bu bölümde isteğe bağlı tamamlamış olmanız gerekir **uygulamasını Azure'a dağıtma** konusundaki [geçişleri ve dağıtımı](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md) öğretici serisinin bu.</span><span class="sxs-lookup"><span data-stu-id="fa583-178">This section requires you to have completed the optional **Deploying the app to Azure** section in the [Migrations and Deployment](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md) tutorial of this series.</span></span> <span data-ttu-id="fa583-179">Veritabanı yerel projenizdeki silerek çözümlenen geçişleri hatasız tamamlanırsa, bu bölümü atlayın.</span><span class="sxs-lookup"><span data-stu-id="fa583-179">If you had migrations errors that you resolved by deleting the database in your local project, skip this section.</span></span>

1. <span data-ttu-id="fa583-180">Visual Studio'da projeye sağ **Çözüm Gezgini** seçip **Yayımla** bağlam menüsünden.</span><span class="sxs-lookup"><span data-stu-id="fa583-180">In Visual Studio, right-click the project in **Solution Explorer** and select **Publish** from the context menu.</span></span>
2. <span data-ttu-id="fa583-181">Tıklayın **yayımlama**.</span><span class="sxs-lookup"><span data-stu-id="fa583-181">Click **Publish**.</span></span>

    <span data-ttu-id="fa583-182">Visual Studio uygulamayı azure'a dağıtır ve uygulama Azure'da çalışan varsayılan tarayıcınızda açılır.</span><span class="sxs-lookup"><span data-stu-id="fa583-182">Visual Studio deploys the application to Azure, and the application opens in your default browser, running in Azure.</span></span>
3. <span data-ttu-id="fa583-183">Bunu doğrulamak için uygulamayı test etme çalışmaktadır.</span><span class="sxs-lookup"><span data-stu-id="fa583-183">Test the application to verify it's working.</span></span>

    <span data-ttu-id="fa583-184">Bir sayfa, ilk kez çalıştırdığınızda, veritabanına erişir, Entity Framework tüm geçişlerde çalıştırır `Up` veritabanı geçerli bir veri modeli ile güncel duruma getirmek için gerekli yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="fa583-184">The first time you run a page that accesses the database, the Entity Framework runs all of the migrations `Up` methods required to bring the database up to date with the current data model.</span></span> <span data-ttu-id="fa583-185">Artık tüm Bu öğreticide eklediğiniz departman sayfaları dahil olmak üzere dağıtılan en son ne zaman beri eklenmiş web sayfaları kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fa583-185">You can now use all of the web pages that you added since the last time you deployed, including the Department pages that you added in this tutorial.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="fa583-186">Kodu alma</span><span class="sxs-lookup"><span data-stu-id="fa583-186">Get the code</span></span>

[<span data-ttu-id="fa583-187">Projeyi yükle</span><span class="sxs-lookup"><span data-stu-id="fa583-187">Download the Completed Project</span></span>](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a><span data-ttu-id="fa583-188">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="fa583-188">Additional resources</span></span>

<span data-ttu-id="fa583-189">Entity Framework diğer kaynakların bağlantılarını bulunabilir [ASP.NET veri erişimi - önerilen kaynaklar](../../../../whitepapers/aspnet-data-access-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="fa583-189">Links to other Entity Framework resources can be found in the [ASP.NET Data Access - Recommended Resources](../../../../whitepapers/aspnet-data-access-content-map.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="fa583-190">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="fa583-190">Next steps</span></span>

<span data-ttu-id="fa583-191">Bu öğreticide şunları yaptınız:</span><span class="sxs-lookup"><span data-stu-id="fa583-191">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="fa583-192">Zaman uyumsuz kod hakkında bilgi edindiniz</span><span class="sxs-lookup"><span data-stu-id="fa583-192">Learned about asynchronous code</span></span>
> * <span data-ttu-id="fa583-193">Bir bölüm denetleyicisi oluşturuldu</span><span class="sxs-lookup"><span data-stu-id="fa583-193">Created a Department controller</span></span>
> * <span data-ttu-id="fa583-194">Saklı yordamlar kullanılır.</span><span class="sxs-lookup"><span data-stu-id="fa583-194">Used stored procedures</span></span>
> * <span data-ttu-id="fa583-195">Azure'a dağıtılan</span><span class="sxs-lookup"><span data-stu-id="fa583-195">Deployed to Azure</span></span>

<span data-ttu-id="fa583-196">Birden çok kullanıcı aynı anda aynı varlık güncelleştirdiğinizde çakışmalarını işleme hakkında bilgi edinmek için sonraki makaleye ilerleyin.</span><span class="sxs-lookup"><span data-stu-id="fa583-196">Advance to the next article to learn how to handle conflicts when multiple users update the same entity at the same time.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="fa583-197">Eşzamanlılığı işleme</span><span class="sxs-lookup"><span data-stu-id="fa583-197">Handling concurrency</span></span>](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
