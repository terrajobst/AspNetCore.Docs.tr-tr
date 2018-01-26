---
uid: web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
title: Birim testi ASP.NET Web API 2 | Microsoft Docs
author: tfitzmac
description: "Bu yönerge ve uygulama, Web API 2 uygulamanız için basit birim testleri oluşturmak nasıl ekleyebileceğiniz gösterilmektedir. Bu öğretici, bir birim testi proj dahil gösterilmektedir..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/05/2014
ms.topic: article
ms.assetid: bf20f78d-ff91-48be-abd1-88e23dcc70ba
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 4d6102dd81589e41894d8ecd95bf9ddd761a65bd
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="unit-testing-aspnet-web-api-2"></a><span data-ttu-id="6a71b-104">Birim testi ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="6a71b-104">Unit Testing ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="6a71b-105">tarafından [zel FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="6a71b-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[<span data-ttu-id="6a71b-106">Tamamlanan projenizi indirin</span><span class="sxs-lookup"><span data-stu-id="6a71b-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-e2867d4d)

> <span data-ttu-id="6a71b-107">Bu yönerge ve uygulama, Web API 2 uygulamanız için basit birim testleri oluşturmak nasıl ekleyebileceğiniz gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="6a71b-107">This guidance and application demonstrate how to create simple unit tests for your Web API 2 application.</span></span> <span data-ttu-id="6a71b-108">Bu öğretici, çözümünüzde birim testi projesi içerir ve bir denetleyici yönteminden döndürülen değerlerini denetleyin test yöntemleri yazma gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="6a71b-108">This tutorial shows how to include a unit test project in your solution, and write test methods that check the returned values from a controller method.</span></span>
> 
> <span data-ttu-id="6a71b-109">Bu öğreticide, ASP.NET Web API temel kavramları bildiğinizi varsayar.</span><span class="sxs-lookup"><span data-stu-id="6a71b-109">This tutorial assumes you are familiar with the basic concepts of ASP.NET Web API.</span></span> <span data-ttu-id="6a71b-110">Giriş bir öğretici için bkz: [ASP.NET Web API 2 ile çalışmaya başlama](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="6a71b-110">For an introductory tutorial, see [Getting Started with ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span></span>
> 
> <span data-ttu-id="6a71b-111">Birim testleri bu konuda, basit veri senaryoları için kasıtlı olarak sınırlıdır.</span><span class="sxs-lookup"><span data-stu-id="6a71b-111">The unit tests in this topic are intentionally limited to simple data scenarios.</span></span> <span data-ttu-id="6a71b-112">Birim testi daha gelişmiş veri senaryoları için bkz: [Mocking Entity Framework zaman birim testi ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).</span><span class="sxs-lookup"><span data-stu-id="6a71b-112">For unit testing more advanced data scenarios, see [Mocking Entity Framework when Unit Testing ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="6a71b-113">Öğreticide kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="6a71b-113">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="6a71b-114">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="6a71b-114">Visual Studio 2017</span></span>](https://www.visualstudio.com/vs/)
> - <span data-ttu-id="6a71b-115">Web API 2</span><span class="sxs-lookup"><span data-stu-id="6a71b-115">Web API 2</span></span>


## <a name="in-this-topic"></a><span data-ttu-id="6a71b-116">Bu konuda</span><span class="sxs-lookup"><span data-stu-id="6a71b-116">In this topic</span></span>

<span data-ttu-id="6a71b-117">Bu konu aşağıdaki bölümleri içermektedir:</span><span class="sxs-lookup"><span data-stu-id="6a71b-117">This topic contains the following sections:</span></span>

- [<span data-ttu-id="6a71b-118">Önkoşulları</span><span class="sxs-lookup"><span data-stu-id="6a71b-118">Prerequisites</span></span>](#prereqs)
- [<span data-ttu-id="6a71b-119">Kodu indirme</span><span class="sxs-lookup"><span data-stu-id="6a71b-119">Download code</span></span>](#download)
- [<span data-ttu-id="6a71b-120">Birim testi projesi ile uygulama oluşturma</span><span class="sxs-lookup"><span data-stu-id="6a71b-120">Create application with unit test project</span></span>](#appwithunittest)

    - [<span data-ttu-id="6a71b-121">Uygulama oluştururken birim testi projesi ekleme</span><span class="sxs-lookup"><span data-stu-id="6a71b-121">Add unit test project when creating the application</span></span>](#whencreate)
    - [<span data-ttu-id="6a71b-122">Varolan bir uygulamaya birim testi projesi ekleme</span><span class="sxs-lookup"><span data-stu-id="6a71b-122">Add unit test project to an existing application</span></span>](#addtoexisting)
- [<span data-ttu-id="6a71b-123">Web API 2 uygulama ayarlama</span><span class="sxs-lookup"><span data-stu-id="6a71b-123">Set up the Web API 2 application</span></span>](#setupproject)
- [<span data-ttu-id="6a71b-124">Test projesinde NuGet paketi yüklemesi</span><span class="sxs-lookup"><span data-stu-id="6a71b-124">Install NuGet packages in test project</span></span>](#testpackages)
- [<span data-ttu-id="6a71b-125">Testleri oluşturma</span><span class="sxs-lookup"><span data-stu-id="6a71b-125">Create tests</span></span>](#tests)
- [<span data-ttu-id="6a71b-126">Testleri çalıştırma</span><span class="sxs-lookup"><span data-stu-id="6a71b-126">Run tests</span></span>](#runtests)

<a id="prereqs"></a>
## <a name="prerequisites"></a><span data-ttu-id="6a71b-127">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="6a71b-127">Prerequisites</span></span>

<span data-ttu-id="6a71b-128">Visual Studio 2017 Community, Professional veya Enterprise edition</span><span class="sxs-lookup"><span data-stu-id="6a71b-128">Visual Studio 2017 Community, Professional or Enterprise edition</span></span>

<a id="download"></a>
## <a name="download-code"></a><span data-ttu-id="6a71b-129">Kodu indirme</span><span class="sxs-lookup"><span data-stu-id="6a71b-129">Download code</span></span>

<span data-ttu-id="6a71b-130">Karşıdan [projeyi](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11).</span><span class="sxs-lookup"><span data-stu-id="6a71b-130">Download the [completed project](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11).</span></span> <span data-ttu-id="6a71b-131">Birim testi kodu ve bu konu için indirilebilir proje içeriyor [Mocking Entity Framework zaman birim testi ASP.NET Web API](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md) konu.</span><span class="sxs-lookup"><span data-stu-id="6a71b-131">The downloadable project includes unit test code for this topic and for the [Mocking Entity Framework when Unit Testing ASP.NET Web API](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md) topic.</span></span>

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a><span data-ttu-id="6a71b-132">Birim testi projesi ile uygulama oluşturma</span><span class="sxs-lookup"><span data-stu-id="6a71b-132">Create application with unit test project</span></span>

<span data-ttu-id="6a71b-133">Uygulamanızı oluştururken, bir birim testi projesi oluşturma veya varolan bir uygulamaya birim testi projesi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="6a71b-133">You can either create a unit test project when creating your application or add a unit test project to an existing application.</span></span> <span data-ttu-id="6a71b-134">Bu öğreticide, bir birim testi projesi oluşturmak için her iki yöntemi gösterilir.</span><span class="sxs-lookup"><span data-stu-id="6a71b-134">This tutorial shows both methods for creating a unit test project.</span></span> <span data-ttu-id="6a71b-135">Bu öğreticiyi izlemek için her iki yaklaşım kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6a71b-135">To follow this tutorial, you can use either approach.</span></span>

<a id="whencreate"></a>
### <a name="add-unit-test-project-when-creating-the-application"></a><span data-ttu-id="6a71b-136">Uygulama oluştururken birim testi projesi ekleme</span><span class="sxs-lookup"><span data-stu-id="6a71b-136">Add unit test project when creating the application</span></span>

<span data-ttu-id="6a71b-137">Adlı yeni bir ASP.NET Web uygulaması oluşturma **StoreApp**.</span><span class="sxs-lookup"><span data-stu-id="6a71b-137">Create a new ASP.NET Web Application named **StoreApp**.</span></span>

![Proje oluşturma](unit-testing-with-aspnet-web-api/_static/image1.png)

<span data-ttu-id="6a71b-139">Yeni ASP.NET projesi Windows'da seçin **boş** şablon klasörleri ekleyin ve Web API başvuru çekirdek.</span><span class="sxs-lookup"><span data-stu-id="6a71b-139">In the New ASP.NET Project windows, select the **Empty** template and add folders and core references for Web API.</span></span> <span data-ttu-id="6a71b-140">Seçin **birim testleri ekleme** seçeneği.</span><span class="sxs-lookup"><span data-stu-id="6a71b-140">Select the **Add unit tests** option.</span></span> <span data-ttu-id="6a71b-141">Birim testi projesi otomatik olarak adlı **StoreApp.Tests**.</span><span class="sxs-lookup"><span data-stu-id="6a71b-141">The unit test project is automatically named **StoreApp.Tests**.</span></span> <span data-ttu-id="6a71b-142">Bu adı kullanmaya devam edebilir.</span><span class="sxs-lookup"><span data-stu-id="6a71b-142">You can keep this name.</span></span>

![Birim testi projesi oluşturma](unit-testing-with-aspnet-web-api/_static/image2.png)

<span data-ttu-id="6a71b-144">Uygulamayı oluşturduktan sonra iki proje içeren görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="6a71b-144">After creating the application, you will see it contains two projects.</span></span>

![iki proje](unit-testing-with-aspnet-web-api/_static/image3.png)

<a id="addtoexisting"></a>
### <a name="add-unit-test-project-to-an-existing-application"></a><span data-ttu-id="6a71b-146">Varolan bir uygulamaya birim testi projesi ekleme</span><span class="sxs-lookup"><span data-stu-id="6a71b-146">Add unit test project to an existing application</span></span>

<span data-ttu-id="6a71b-147">Uygulamanızı oluştururken birim testi projesi oluşturmadıysanız herhangi bir anda bir tane ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6a71b-147">If you did not create the unit test project when you created your application, you can add one at any time.</span></span> <span data-ttu-id="6a71b-148">Örneğin, StoreApp adlı bir uygulama zaten var ve birim testleri eklemek istediğiniz varsayalım.</span><span class="sxs-lookup"><span data-stu-id="6a71b-148">For example, suppose you already have an application named StoreApp, and you want to add unit tests.</span></span> <span data-ttu-id="6a71b-149">Birim testi projesi eklemek için Çözümünüze sağ tıklayın ve seçin **Ekle** ve **yeni proje**.</span><span class="sxs-lookup"><span data-stu-id="6a71b-149">To add a unit test project, right-click your solution and select **Add** and **New Project**.</span></span>

![Yeni proje çözüme ekleyin](unit-testing-with-aspnet-web-api/_static/image4.png)

<span data-ttu-id="6a71b-151">Seçin **Test** sol bölmede, seçip **birim testi projesi** proje türü için.</span><span class="sxs-lookup"><span data-stu-id="6a71b-151">Select **Test** in the left pane, and select **Unit Test Project** for the project type.</span></span> <span data-ttu-id="6a71b-152">Proje adı **StoreApp.Tests**.</span><span class="sxs-lookup"><span data-stu-id="6a71b-152">Name the project **StoreApp.Tests**.</span></span>

![Birim testi projesi ekleme](unit-testing-with-aspnet-web-api/_static/image5.png)

<span data-ttu-id="6a71b-154">Çözümünüzde birim testi projesi görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="6a71b-154">You will see the unit test project in your solution.</span></span>

<span data-ttu-id="6a71b-155">Birim testi projesi proje başvurusu özgün projeye ekleyin.</span><span class="sxs-lookup"><span data-stu-id="6a71b-155">In the unit test project, add a project reference to the original project.</span></span>

<a id="setupproject"></a>
## <a name="set-up-the-web-api-2-application"></a><span data-ttu-id="6a71b-156">Web API 2 uygulama ayarlama</span><span class="sxs-lookup"><span data-stu-id="6a71b-156">Set up the Web API 2 application</span></span>

<span data-ttu-id="6a71b-157">Bir sınıf dosyaya StoreApp projenize eklemek **modelleri** adlı klasörü **Product.cs**.</span><span class="sxs-lookup"><span data-stu-id="6a71b-157">In your StoreApp project, add a class file to the **Models** folder named **Product.cs**.</span></span> <span data-ttu-id="6a71b-158">Dosya içeriğini aşağıdaki kodla değiştirin.</span><span class="sxs-lookup"><span data-stu-id="6a71b-158">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="6a71b-159">Çözümü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="6a71b-159">Build the solution.</span></span>

<span data-ttu-id="6a71b-160">Denetleyicileri klasörüne sağ tıklayıp **Ekle** ve **yeni iskele kurulmuş öğe**.</span><span class="sxs-lookup"><span data-stu-id="6a71b-160">Right-click the Controllers folder and select **Add** and **New Scaffolded Item**.</span></span> <span data-ttu-id="6a71b-161">Seçin **Web API 2 denetleyici - boş**.</span><span class="sxs-lookup"><span data-stu-id="6a71b-161">Select **Web API 2 Controller - Empty**.</span></span>

![Yeni denetleyici ekleyin](unit-testing-with-aspnet-web-api/_static/image6.png)

<span data-ttu-id="6a71b-163">Denetleyici adı ayarlamak **SimpleProductController**, tıklatıp **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="6a71b-163">Set the controller name to **SimpleProductController**, and click **Add**.</span></span>

![Denetleyici belirtin](unit-testing-with-aspnet-web-api/_static/image7.png)

<span data-ttu-id="6a71b-165">Var olan kodu aşağıdaki kodla değiştirin.</span><span class="sxs-lookup"><span data-stu-id="6a71b-165">Replace the existing code with the following code.</span></span> <span data-ttu-id="6a71b-166">Bu örnek basitleştirmek için veriler, veritabanı yerine listesini depolanır.</span><span class="sxs-lookup"><span data-stu-id="6a71b-166">To simplify this example, the data is stored in a list rather than a database.</span></span> <span data-ttu-id="6a71b-167">Bu sınıf içinde tanımlanan liste üretim verileri temsil eder.</span><span class="sxs-lookup"><span data-stu-id="6a71b-167">The list defined in this class represents the production data.</span></span> <span data-ttu-id="6a71b-168">Denetleyicinin parametre olarak ürün nesnelerin bir listesini alan bir oluşturucu içerdiğine dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="6a71b-168">Notice that the controller includes a constructor that takes as a parameter a list of Product objects.</span></span> <span data-ttu-id="6a71b-169">Bu oluşturucu, test veri iletmek sağlar, birim testi.</span><span class="sxs-lookup"><span data-stu-id="6a71b-169">This constructor enables you to pass test data when unit testing.</span></span> <span data-ttu-id="6a71b-170">İki denetleyici ayrıca içerir **zaman uyumsuz** birim testi zaman uyumsuz yöntemleri göstermek için yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="6a71b-170">The controller also includes two **async** methods to illustrate unit testing asynchronous methods.</span></span> <span data-ttu-id="6a71b-171">Bu zaman uyumsuz yöntemleri çağırma uygulanan **Task.FromResult** yabancı kodu ancak normalde yöntemleri en aza indirmek için yoğun bir kaynak işlemlerinin içerir.</span><span class="sxs-lookup"><span data-stu-id="6a71b-171">These async methods were implemented by calling **Task.FromResult** to minimize extraneous code, but normally the methods would include resource-intensive operations.</span></span>

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="6a71b-172">GetProduct yöntemi örneğini döndürür **Ihttpactionresult** arabirimi.</span><span class="sxs-lookup"><span data-stu-id="6a71b-172">The GetProduct method returns an instance of the **IHttpActionResult** interface.</span></span> <span data-ttu-id="6a71b-173">Ihttpactionresult Web API 2'deki yeni özelliklerden biridir ve birim testi geliştirme basitleştirir.</span><span class="sxs-lookup"><span data-stu-id="6a71b-173">IHttpActionResult is one of the new features in Web API 2, and it simplifies unit test development.</span></span> <span data-ttu-id="6a71b-174">İçinde bulunan Ihttpactionresult arabirimini uygulayan sınıflar [System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx) ad alanı.</span><span class="sxs-lookup"><span data-stu-id="6a71b-174">Classes that implement the IHttpActionResult interface are found in the [System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx) namespace.</span></span> <span data-ttu-id="6a71b-175">Bu sınıfların bir eylem isteği olası yanıtlarının temsil eder ve HTTP durum kodları karşılık.</span><span class="sxs-lookup"><span data-stu-id="6a71b-175">These classes represent possible responses from an action request, and they correspond to HTTP status codes.</span></span>

<span data-ttu-id="6a71b-176">Çözümü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="6a71b-176">Build the solution.</span></span>

<span data-ttu-id="6a71b-177">Artık test projesi kurmanız hazırsınız.</span><span class="sxs-lookup"><span data-stu-id="6a71b-177">You are now ready to set up the test project.</span></span>

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a><span data-ttu-id="6a71b-178">Test projesinde NuGet paketi yüklemesi</span><span class="sxs-lookup"><span data-stu-id="6a71b-178">Install NuGet packages in test project</span></span>

<span data-ttu-id="6a71b-179">Bir uygulama oluşturmak için boş şablonunu kullandığınızda, birim testi projesi (StoreApp.Tests) yüklü herhangi bir NuGet paketinin içermez.</span><span class="sxs-lookup"><span data-stu-id="6a71b-179">When you use the Empty template to create an application, the unit test project (StoreApp.Tests) does not include any installed NuGet packages.</span></span> <span data-ttu-id="6a71b-180">Web API şablonu gibi diğer şablonları birim testi projesi bazı NuGet paketleri içerir.</span><span class="sxs-lookup"><span data-stu-id="6a71b-180">Other templates, such as the Web API template, include some NuGet packages in the unit test project.</span></span> <span data-ttu-id="6a71b-181">Bu öğretici için oluşturduğunuz test projesinin Microsoft ASP.NET Web API 2 Çekirdek paketi eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="6a71b-181">For this tutorial, you must include the Microsoft ASP.NET Web API 2 Core package to the test project.</span></span>

<span data-ttu-id="6a71b-182">StoreApp.Tests projesine sağ tıklatın ve **NuGet paketlerini Yönet**.</span><span class="sxs-lookup"><span data-stu-id="6a71b-182">Right-click the StoreApp.Tests project and select **Manage NuGet Packages**.</span></span> <span data-ttu-id="6a71b-183">Paketler bu projeye eklemek için StoreApp.Tests proje seçmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="6a71b-183">You must select the StoreApp.Tests project to add the packages to that project.</span></span>

![paketlerini yönetme](unit-testing-with-aspnet-web-api/_static/image8.png)

<span data-ttu-id="6a71b-185">Bul ve Microsoft ASP.NET Web API 2 Çekirdek paketi yükleyin.</span><span class="sxs-lookup"><span data-stu-id="6a71b-185">Find and install Microsoft ASP.NET Web API 2 Core package.</span></span>

![Web API core paketini yükle](unit-testing-with-aspnet-web-api/_static/image9.png)

<span data-ttu-id="6a71b-187">NuGet paketlerini Yönet penceresini kapatın.</span><span class="sxs-lookup"><span data-stu-id="6a71b-187">Close the Manage NuGet Packages window.</span></span>

<a id="tests"></a>
## <a name="create-tests"></a><span data-ttu-id="6a71b-188">Testleri oluşturma</span><span class="sxs-lookup"><span data-stu-id="6a71b-188">Create tests</span></span>

<span data-ttu-id="6a71b-189">Varsayılan olarak, test projenizin UnitTest1.cs adlı bir boş test dosyası içerir.</span><span class="sxs-lookup"><span data-stu-id="6a71b-189">By default, your test project includes an empty test file named UnitTest1.cs.</span></span> <span data-ttu-id="6a71b-190">Bu dosya, test yöntemleri oluşturmak için kullandığınız öznitelikleri gösterir.</span><span class="sxs-lookup"><span data-stu-id="6a71b-190">This file shows the attributes you use to create test methods.</span></span> <span data-ttu-id="6a71b-191">Birim testleri için bu dosyayı kullanabilir veya kendi dosyanızı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="6a71b-191">For your unit tests, you can either use this file or create your own file.</span></span>

![UnitTest1](unit-testing-with-aspnet-web-api/_static/image10.png)

<span data-ttu-id="6a71b-193">Bu öğretici için kendi test sınıfı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="6a71b-193">For this tutorial, you will create your own test class.</span></span> <span data-ttu-id="6a71b-194">UnitTest1.cs dosyayı silebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6a71b-194">You can delete the UnitTest1.cs file.</span></span> <span data-ttu-id="6a71b-195">Adlı bir sınıf ekleyin **TestSimpleProductController.cs**ve kodu aşağıdaki kodla değiştirin.</span><span class="sxs-lookup"><span data-stu-id="6a71b-195">Add a class named **TestSimpleProductController.cs**, and replace the code with the following code.</span></span>

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample3.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a><span data-ttu-id="6a71b-196">Testleri çalıştırma</span><span class="sxs-lookup"><span data-stu-id="6a71b-196">Run tests</span></span>

<span data-ttu-id="6a71b-197">Şimdi testleri çalıştırmak hazırsınız.</span><span class="sxs-lookup"><span data-stu-id="6a71b-197">You are now ready to run the tests.</span></span> <span data-ttu-id="6a71b-198">Tüm işaretlenir yöntemi **TestMethod** özniteliği test edilmiş.</span><span class="sxs-lookup"><span data-stu-id="6a71b-198">All of the method that are marked with the **TestMethod** attribute will be tested.</span></span> <span data-ttu-id="6a71b-199">Gelen **Test** menü öğesi, testleri çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="6a71b-199">From the **Test** menu item, run the tests.</span></span>

![testleri çalıştırma](unit-testing-with-aspnet-web-api/_static/image11.png)

<span data-ttu-id="6a71b-201">Açık **Test Gezgini** penceresinde ve test sonuçlarını dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="6a71b-201">Open the **Test Explorer** window, and notice the results of the tests.</span></span>

![test sonuçları](unit-testing-with-aspnet-web-api/_static/image12.png)

## <a name="summary"></a><span data-ttu-id="6a71b-203">Özet</span><span class="sxs-lookup"><span data-stu-id="6a71b-203">Summary</span></span>

<span data-ttu-id="6a71b-204">Bu öğretici tamamladınız.</span><span class="sxs-lookup"><span data-stu-id="6a71b-204">You have completed this tutorial.</span></span> <span data-ttu-id="6a71b-205">Bu öğreticide veri kasıtlı olarak birim testi koşullar odaklanmak Basitleştirilmiş.</span><span class="sxs-lookup"><span data-stu-id="6a71b-205">The data in this tutorial was intentionally simplified to focus on unit testing conditions.</span></span> <span data-ttu-id="6a71b-206">Birim testi daha gelişmiş veri senaryoları için bkz: [Mocking Entity Framework zaman birim testi ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).</span><span class="sxs-lookup"><span data-stu-id="6a71b-206">For unit testing more advanced data scenarios, see [Mocking Entity Framework when Unit Testing ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).</span></span>
