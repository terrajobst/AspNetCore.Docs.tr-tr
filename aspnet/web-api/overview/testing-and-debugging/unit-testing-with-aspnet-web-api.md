---
uid: web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
title: Birim testi ASP.NET Web API 2 | Microsoft Docs
author: tfitzmac
description: Bu kılavuzu ve uygulama, Web API 2 uygulama için basit birim testleri oluşturma işlemini göstermektedir. Bu öğreticide, bir birim test proj içerecek şekilde gösterilmektedir...
ms.author: riande
ms.date: 06/05/2014
ms.assetid: bf20f78d-ff91-48be-abd1-88e23dcc70ba
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 0a3f9ddde725995359de85c5e0783106f5de02bd
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753909"
---
<a name="unit-testing-aspnet-web-api-2"></a><span data-ttu-id="f90cc-104">Birim testi ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="f90cc-104">Unit Testing ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="f90cc-105">tarafından [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="f90cc-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[<span data-ttu-id="f90cc-106">Projeyi yükle</span><span class="sxs-lookup"><span data-stu-id="f90cc-106">Download Completed Project</span></span>](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11)

> <span data-ttu-id="f90cc-107">Bu kılavuzu ve uygulama, Web API 2 uygulama için basit birim testleri oluşturma işlemini göstermektedir.</span><span class="sxs-lookup"><span data-stu-id="f90cc-107">This guidance and application demonstrate how to create simple unit tests for your Web API 2 application.</span></span> <span data-ttu-id="f90cc-108">Bu öğreticide, bir denetleyici yönteminden döndürülen değerleri kontrol test yöntemler yazmak ve birim testi projesi içeren çözümünüze gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="f90cc-108">This tutorial shows how to include a unit test project in your solution, and write test methods that check the returned values from a controller method.</span></span>
> 
> <span data-ttu-id="f90cc-109">Bu öğreticide, ASP.NET Web API ile ilgili temel kavramlar hakkında bilgi sahibi olduğunuz varsayılır.</span><span class="sxs-lookup"><span data-stu-id="f90cc-109">This tutorial assumes you are familiar with the basic concepts of ASP.NET Web API.</span></span> <span data-ttu-id="f90cc-110">Giriş niteliğindeki bir eğitim için bkz. [ASP.NET Web API 2 ile çalışmaya başlama](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="f90cc-110">For an introductory tutorial, see [Getting Started with ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span></span>
> 
> <span data-ttu-id="f90cc-111">Birim testleri bu konuda, basit veri senaryoları için kasıtlı olarak sınırlıdır.</span><span class="sxs-lookup"><span data-stu-id="f90cc-111">The unit tests in this topic are intentionally limited to simple data scenarios.</span></span> <span data-ttu-id="f90cc-112">Birim testi daha gelişmiş veri senaryoları için bkz. [sahte Entity Framework, birim testi ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).</span><span class="sxs-lookup"><span data-stu-id="f90cc-112">For unit testing more advanced data scenarios, see [Mocking Entity Framework when Unit Testing ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="f90cc-113">Bu öğreticide kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="f90cc-113">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="f90cc-114">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="f90cc-114">Visual Studio 2017</span></span>](https://www.visualstudio.com/vs/)
> - <span data-ttu-id="f90cc-115">Web API 2</span><span class="sxs-lookup"><span data-stu-id="f90cc-115">Web API 2</span></span>


## <a name="in-this-topic"></a><span data-ttu-id="f90cc-116">Bu konuda</span><span class="sxs-lookup"><span data-stu-id="f90cc-116">In this topic</span></span>

<span data-ttu-id="f90cc-117">Bu konu aşağıdaki bölümleri içermektedir:</span><span class="sxs-lookup"><span data-stu-id="f90cc-117">This topic contains the following sections:</span></span>

- [<span data-ttu-id="f90cc-118">Önkoşulları</span><span class="sxs-lookup"><span data-stu-id="f90cc-118">Prerequisites</span></span>](#prereqs)
- [<span data-ttu-id="f90cc-119">Kodu indir</span><span class="sxs-lookup"><span data-stu-id="f90cc-119">Download code</span></span>](#download)
- [<span data-ttu-id="f90cc-120">Uygulama ile birim testi projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="f90cc-120">Create application with unit test project</span></span>](#appwithunittest)

    - [<span data-ttu-id="f90cc-121">Uygulama oluştururken, birim testi projesi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="f90cc-121">Add unit test project when creating the application</span></span>](#whencreate)
    - [<span data-ttu-id="f90cc-122">Birim testi projesi varolan bir uygulamaya ekleme</span><span class="sxs-lookup"><span data-stu-id="f90cc-122">Add unit test project to an existing application</span></span>](#addtoexisting)
- [<span data-ttu-id="f90cc-123">Web API 2 uygulama ayarlama</span><span class="sxs-lookup"><span data-stu-id="f90cc-123">Set up the Web API 2 application</span></span>](#setupproject)
- [<span data-ttu-id="f90cc-124">Test projesinde NuGet paketlerini yükleme</span><span class="sxs-lookup"><span data-stu-id="f90cc-124">Install NuGet packages in test project</span></span>](#testpackages)
- [<span data-ttu-id="f90cc-125">Testleri oluşturma</span><span class="sxs-lookup"><span data-stu-id="f90cc-125">Create tests</span></span>](#tests)
- [<span data-ttu-id="f90cc-126">Testleri çalıştırın</span><span class="sxs-lookup"><span data-stu-id="f90cc-126">Run tests</span></span>](#runtests)

<a id="prereqs"></a>
## <a name="prerequisites"></a><span data-ttu-id="f90cc-127">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="f90cc-127">Prerequisites</span></span>

<span data-ttu-id="f90cc-128">Visual Studio 2017 Community, Professional veya Enterprise edition</span><span class="sxs-lookup"><span data-stu-id="f90cc-128">Visual Studio 2017 Community, Professional or Enterprise edition</span></span>

<a id="download"></a>
## <a name="download-code"></a><span data-ttu-id="f90cc-129">Kodu indir</span><span class="sxs-lookup"><span data-stu-id="f90cc-129">Download code</span></span>

<span data-ttu-id="f90cc-130">İndirme [projeyi](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11).</span><span class="sxs-lookup"><span data-stu-id="f90cc-130">Download the [completed project](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11).</span></span> <span data-ttu-id="f90cc-131">Birim testi kodu ve için bu konunun indirilebilir proje içerir [sahte Entity Framework, ASP.NET Web API birim testi](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md) konu.</span><span class="sxs-lookup"><span data-stu-id="f90cc-131">The downloadable project includes unit test code for this topic and for the [Mocking Entity Framework when Unit Testing ASP.NET Web API](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md) topic.</span></span>

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a><span data-ttu-id="f90cc-132">Uygulama ile birim testi projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="f90cc-132">Create application with unit test project</span></span>

<span data-ttu-id="f90cc-133">Uygulamanızı oluştururken bir birim test projesi oluşturun veya mevcut bir uygulamaya bir birim test projesi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="f90cc-133">You can either create a unit test project when creating your application or add a unit test project to an existing application.</span></span> <span data-ttu-id="f90cc-134">Bu öğreticide, bir birim test projesi oluşturmak için her iki yöntem de gösterilir.</span><span class="sxs-lookup"><span data-stu-id="f90cc-134">This tutorial shows both methods for creating a unit test project.</span></span> <span data-ttu-id="f90cc-135">Bu öğreticiyi uygulamak için her iki yöntemle kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f90cc-135">To follow this tutorial, you can use either approach.</span></span>

<a id="whencreate"></a>
### <a name="add-unit-test-project-when-creating-the-application"></a><span data-ttu-id="f90cc-136">Uygulama oluştururken, birim testi projesi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="f90cc-136">Add unit test project when creating the application</span></span>

<span data-ttu-id="f90cc-137">Adlı yeni bir ASP.NET Web uygulaması oluşturma **StoreApp**.</span><span class="sxs-lookup"><span data-stu-id="f90cc-137">Create a new ASP.NET Web Application named **StoreApp**.</span></span>

![Proje oluşturma](unit-testing-with-aspnet-web-api/_static/image1.png)

<span data-ttu-id="f90cc-139">Yeni ASP.NET projesi Windows'da seçin **boş** şablon klasörleri ekleyin ve Web API'si için başvuru çekirdek.</span><span class="sxs-lookup"><span data-stu-id="f90cc-139">In the New ASP.NET Project windows, select the **Empty** template and add folders and core references for Web API.</span></span> <span data-ttu-id="f90cc-140">Seçin **birim testleri ekleme** seçeneği.</span><span class="sxs-lookup"><span data-stu-id="f90cc-140">Select the **Add unit tests** option.</span></span> <span data-ttu-id="f90cc-141">Birim test projesi otomatik olarak adlandırılır **StoreApp.Tests**.</span><span class="sxs-lookup"><span data-stu-id="f90cc-141">The unit test project is automatically named **StoreApp.Tests**.</span></span> <span data-ttu-id="f90cc-142">Bu ad tutabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f90cc-142">You can keep this name.</span></span>

![Birim testi projesi oluşturma](unit-testing-with-aspnet-web-api/_static/image2.png)

<span data-ttu-id="f90cc-144">Uygulamayı oluşturduktan sonra iki proje içeren görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="f90cc-144">After creating the application, you will see it contains two projects.</span></span>

![iki proje](unit-testing-with-aspnet-web-api/_static/image3.png)

<a id="addtoexisting"></a>
### <a name="add-unit-test-project-to-an-existing-application"></a><span data-ttu-id="f90cc-146">Birim testi projesi varolan bir uygulamaya ekleme</span><span class="sxs-lookup"><span data-stu-id="f90cc-146">Add unit test project to an existing application</span></span>

<span data-ttu-id="f90cc-147">Uygulamanızı oluştururken birim test projesi oluşturmadıysanız herhangi bir zamanda ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f90cc-147">If you did not create the unit test project when you created your application, you can add one at any time.</span></span> <span data-ttu-id="f90cc-148">Örneğin, zaten sahip olduğunuz StoreApp adlı bir uygulama ve birim testleri eklemek istediğiniz varsayalım.</span><span class="sxs-lookup"><span data-stu-id="f90cc-148">For example, suppose you already have an application named StoreApp, and you want to add unit tests.</span></span> <span data-ttu-id="f90cc-149">Birim testi projesi eklemek için çözümü sağ tıklatın ve seçin **Ekle** ve **yeni proje**.</span><span class="sxs-lookup"><span data-stu-id="f90cc-149">To add a unit test project, right-click your solution and select **Add** and **New Project**.</span></span>

![çözüme yeni proje Ekle](unit-testing-with-aspnet-web-api/_static/image4.png)

<span data-ttu-id="f90cc-151">Seçin **Test** sol bölmesinde, seçip **birim testi projesi** proje türü.</span><span class="sxs-lookup"><span data-stu-id="f90cc-151">Select **Test** in the left pane, and select **Unit Test Project** for the project type.</span></span> <span data-ttu-id="f90cc-152">Projeyi adlandırın **StoreApp.Tests**.</span><span class="sxs-lookup"><span data-stu-id="f90cc-152">Name the project **StoreApp.Tests**.</span></span>

![Birim testi projesi ekleme](unit-testing-with-aspnet-web-api/_static/image5.png)

<span data-ttu-id="f90cc-154">Birim test projesi çözümünüze görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="f90cc-154">You will see the unit test project in your solution.</span></span>

<span data-ttu-id="f90cc-155">Birim test projesinde özgün proje için bir proje başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="f90cc-155">In the unit test project, add a project reference to the original project.</span></span>

<a id="setupproject"></a>
## <a name="set-up-the-web-api-2-application"></a><span data-ttu-id="f90cc-156">Web API 2 uygulama ayarlama</span><span class="sxs-lookup"><span data-stu-id="f90cc-156">Set up the Web API 2 application</span></span>

<span data-ttu-id="f90cc-157">StoreApp projeniz için sınıf dosyası ekleyin **modelleri** adlı klasöre **Product.cs**.</span><span class="sxs-lookup"><span data-stu-id="f90cc-157">In your StoreApp project, add a class file to the **Models** folder named **Product.cs**.</span></span> <span data-ttu-id="f90cc-158">Dosyanın içeriğini aşağıdaki kodla değiştirin.</span><span class="sxs-lookup"><span data-stu-id="f90cc-158">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="f90cc-159">Çözümü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="f90cc-159">Build the solution.</span></span>

<span data-ttu-id="f90cc-160">Denetleyicileri klasörüne sağ tıklayıp **Ekle** ve **yeni iskele kurulmuş öğe**.</span><span class="sxs-lookup"><span data-stu-id="f90cc-160">Right-click the Controllers folder and select **Add** and **New Scaffolded Item**.</span></span> <span data-ttu-id="f90cc-161">Seçin **Web API 2 denetleyici - boş**.</span><span class="sxs-lookup"><span data-stu-id="f90cc-161">Select **Web API 2 Controller - Empty**.</span></span>

![Yeni denetleyici ekleyin](unit-testing-with-aspnet-web-api/_static/image6.png)

<span data-ttu-id="f90cc-163">Denetleyici adı kümesine **SimpleProductController**, tıklatıp **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="f90cc-163">Set the controller name to **SimpleProductController**, and click **Add**.</span></span>

![Denetleyici belirtin](unit-testing-with-aspnet-web-api/_static/image7.png)

<span data-ttu-id="f90cc-165">Varolan kodu aşağıdaki kodla değiştirin.</span><span class="sxs-lookup"><span data-stu-id="f90cc-165">Replace the existing code with the following code.</span></span> <span data-ttu-id="f90cc-166">Bu örneği basitleştirmek amacıyla verileri bir veritabanı yerine bir liste içinde depolanır.</span><span class="sxs-lookup"><span data-stu-id="f90cc-166">To simplify this example, the data is stored in a list rather than a database.</span></span> <span data-ttu-id="f90cc-167">Bu sınıfta tanımlanan liste üretim verileri temsil eder.</span><span class="sxs-lookup"><span data-stu-id="f90cc-167">The list defined in this class represents the production data.</span></span> <span data-ttu-id="f90cc-168">Denetleyici ürün nesnelerin bir listesini bir parametre olarak alan bir oluşturucu içerdiğine dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="f90cc-168">Notice that the controller includes a constructor that takes as a parameter a list of Product objects.</span></span> <span data-ttu-id="f90cc-169">Bu oluşturucu, test verileri geçirmenizi sağlar, birim testi.</span><span class="sxs-lookup"><span data-stu-id="f90cc-169">This constructor enables you to pass test data when unit testing.</span></span> <span data-ttu-id="f90cc-170">İki denetleyici de içeren **zaman uyumsuz** birim testi zaman uyumsuz yöntemleri göstermek için yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="f90cc-170">The controller also includes two **async** methods to illustrate unit testing asynchronous methods.</span></span> <span data-ttu-id="f90cc-171">Bu zaman uyumsuz yöntemler çağrılarak uygulandığına **Task.FromResult** gereksiz kod, ancak genellikle yöntemleri en aza indirmek için kaynak kullanımı yoğun işlemleri içerir.</span><span class="sxs-lookup"><span data-stu-id="f90cc-171">These async methods were implemented by calling **Task.FromResult** to minimize extraneous code, but normally the methods would include resource-intensive operations.</span></span>

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="f90cc-172">Örneğini GetProduct yöntemi döndürür **Ihttpactionresult** arabirimi.</span><span class="sxs-lookup"><span data-stu-id="f90cc-172">The GetProduct method returns an instance of the **IHttpActionResult** interface.</span></span> <span data-ttu-id="f90cc-173">Web API 2'deki yeni özelliklerin Ihttpactionresult biridir ve birim testi geliştirmenin kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="f90cc-173">IHttpActionResult is one of the new features in Web API 2, and it simplifies unit test development.</span></span> <span data-ttu-id="f90cc-174">Ihttpactionresult arabirimi uygulayan sınıflar bulunur [System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx) ad alanı.</span><span class="sxs-lookup"><span data-stu-id="f90cc-174">Classes that implement the IHttpActionResult interface are found in the [System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx) namespace.</span></span> <span data-ttu-id="f90cc-175">Bu sınıfların bir eylem istek olası yanıtlar temsil eder ve bunlar için HTTP durum kodları karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="f90cc-175">These classes represent possible responses from an action request, and they correspond to HTTP status codes.</span></span>

<span data-ttu-id="f90cc-176">Çözümü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="f90cc-176">Build the solution.</span></span>

<span data-ttu-id="f90cc-177">Şimdi test projesini ayarlarsınız hazırsınız.</span><span class="sxs-lookup"><span data-stu-id="f90cc-177">You are now ready to set up the test project.</span></span>

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a><span data-ttu-id="f90cc-178">Test projesinde NuGet paketlerini yükleme</span><span class="sxs-lookup"><span data-stu-id="f90cc-178">Install NuGet packages in test project</span></span>

<span data-ttu-id="f90cc-179">Bir uygulama oluşturmak için boş şablonu kullandığınızda, birim testi projesi (StoreApp.Tests) yüklü herhangi bir NuGet paketinin içermez.</span><span class="sxs-lookup"><span data-stu-id="f90cc-179">When you use the Empty template to create an application, the unit test project (StoreApp.Tests) does not include any installed NuGet packages.</span></span> <span data-ttu-id="f90cc-180">Web API şablonu gibi diğer şablonları, birim test projesinde NuGet paketlerinden bazıları içerir.</span><span class="sxs-lookup"><span data-stu-id="f90cc-180">Other templates, such as the Web API template, include some NuGet packages in the unit test project.</span></span> <span data-ttu-id="f90cc-181">Bu öğreticide, Microsoft ASP.NET Web API 2 Çekirdek paketini test projesine eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="f90cc-181">For this tutorial, you must include the Microsoft ASP.NET Web API 2 Core package to the test project.</span></span>

<span data-ttu-id="f90cc-182">StoreApp.Tests projeye sağ tıklayıp **NuGet paketlerini Yönet**.</span><span class="sxs-lookup"><span data-stu-id="f90cc-182">Right-click the StoreApp.Tests project and select **Manage NuGet Packages**.</span></span> <span data-ttu-id="f90cc-183">Paketler bu projeye eklemek için StoreApp.Tests proje seçmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="f90cc-183">You must select the StoreApp.Tests project to add the packages to that project.</span></span>

![paketlerini yönetme](unit-testing-with-aspnet-web-api/_static/image8.png)

<span data-ttu-id="f90cc-185">Bulun ve Microsoft ASP.NET Web API 2 Çekirdek paketini yükleyin.</span><span class="sxs-lookup"><span data-stu-id="f90cc-185">Find and install Microsoft ASP.NET Web API 2 Core package.</span></span>

![Web API çekirdek paketini yükle](unit-testing-with-aspnet-web-api/_static/image9.png)

<span data-ttu-id="f90cc-187">NuGet paketlerini Yönet penceresini kapatın.</span><span class="sxs-lookup"><span data-stu-id="f90cc-187">Close the Manage NuGet Packages window.</span></span>

<a id="tests"></a>
## <a name="create-tests"></a><span data-ttu-id="f90cc-188">Testleri oluşturma</span><span class="sxs-lookup"><span data-stu-id="f90cc-188">Create tests</span></span>

<span data-ttu-id="f90cc-189">Varsayılan olarak, test projenize UnitTest1.cs adlı boş bir test dosyası içerir.</span><span class="sxs-lookup"><span data-stu-id="f90cc-189">By default, your test project includes an empty test file named UnitTest1.cs.</span></span> <span data-ttu-id="f90cc-190">Bu dosya, test yöntemleri oluşturduğunuzda kullandığınız öznitelikleri gösterir.</span><span class="sxs-lookup"><span data-stu-id="f90cc-190">This file shows the attributes you use to create test methods.</span></span> <span data-ttu-id="f90cc-191">Birim testleriniz için için bu dosyayı kullanabilir veya kendi dosyanızı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="f90cc-191">For your unit tests, you can either use this file or create your own file.</span></span>

![UnitTest1](unit-testing-with-aspnet-web-api/_static/image10.png)

<span data-ttu-id="f90cc-193">Bu öğreticide, kendi test sınıfı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="f90cc-193">For this tutorial, you will create your own test class.</span></span> <span data-ttu-id="f90cc-194">UnitTest1.cs dosyasını silebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f90cc-194">You can delete the UnitTest1.cs file.</span></span> <span data-ttu-id="f90cc-195">Adlı bir sınıf ekleyin **TestSimpleProductController.cs**, kodu aşağıdaki kodla değiştirin.</span><span class="sxs-lookup"><span data-stu-id="f90cc-195">Add a class named **TestSimpleProductController.cs**, and replace the code with the following code.</span></span>

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample3.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a><span data-ttu-id="f90cc-196">Testleri çalıştırma</span><span class="sxs-lookup"><span data-stu-id="f90cc-196">Run tests</span></span>

<span data-ttu-id="f90cc-197">Testleri çalıştırmak artık hazırsınız.</span><span class="sxs-lookup"><span data-stu-id="f90cc-197">You are now ready to run the tests.</span></span> <span data-ttu-id="f90cc-198">Tüm ile işaretlenmiş yöntem **TestMethod** özniteliği test edilmiş.</span><span class="sxs-lookup"><span data-stu-id="f90cc-198">All of the method that are marked with the **TestMethod** attribute will be tested.</span></span> <span data-ttu-id="f90cc-199">Gelen **Test** menü öğesi, testleri çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="f90cc-199">From the **Test** menu item, run the tests.</span></span>

![testleri çalıştırma](unit-testing-with-aspnet-web-api/_static/image11.png)

<span data-ttu-id="f90cc-201">Açık **Test Gezgini** penceresinde ve test sonuçlarını dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="f90cc-201">Open the **Test Explorer** window, and notice the results of the tests.</span></span>

![test sonuçları](unit-testing-with-aspnet-web-api/_static/image12.png)

## <a name="summary"></a><span data-ttu-id="f90cc-203">Özet</span><span class="sxs-lookup"><span data-stu-id="f90cc-203">Summary</span></span>

<span data-ttu-id="f90cc-204">Bu öğreticiyi tamamladınız.</span><span class="sxs-lookup"><span data-stu-id="f90cc-204">You have completed this tutorial.</span></span> <span data-ttu-id="f90cc-205">Bu öğreticide verileri kasıtlı olarak birim testi koşullar odaklanmak için Basitleştirilmiş.</span><span class="sxs-lookup"><span data-stu-id="f90cc-205">The data in this tutorial was intentionally simplified to focus on unit testing conditions.</span></span> <span data-ttu-id="f90cc-206">Birim testi daha gelişmiş veri senaryoları için bkz. [sahte Entity Framework, birim testi ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).</span><span class="sxs-lookup"><span data-stu-id="f90cc-206">For unit testing more advanced data scenarios, see [Mocking Entity Framework when Unit Testing ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).</span></span>
