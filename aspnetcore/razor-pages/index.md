---
title: ASP.NET Core Razor Pages giriş
author: Rick-Anderson
description: ASP.NET Core ' deki Razor Pages, MVC 'yi kullanmaktan daha kolay ve daha üretken hale getirmeye nasıl yardımcı olduğunu öğrenin.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 10/07/2019
uid: razor-pages/index
ms.openlocfilehash: 61e15b9b9b8f84de36621c301ecb9d33b21dff88
ms.sourcegitcommit: 16cf016035f0c9acf3ff0ad874c56f82e013d415
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73034273"
---
# <a name="introduction-to-razor-pages-in-aspnet-core"></a><span data-ttu-id="5dfd9-103">ASP.NET Core Razor Pages giriş</span><span class="sxs-lookup"><span data-stu-id="5dfd9-103">Introduction to Razor Pages in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="5dfd9-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Ryan şimdi ak](https://github.com/rynowak)</span><span class="sxs-lookup"><span data-stu-id="5dfd9-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Ryan Nowak](https://github.com/rynowak)</span></span>

<span data-ttu-id="5dfd9-105">Razor Pages, kodlama sayfasına odaklanmış senaryolar denetleyicileri ve görünümleri kullanmaktan daha kolay ve daha üretken hale getirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-105">Razor Pages can make coding page-focused scenarios easier and more productive than using controllers and views.</span></span>

<span data-ttu-id="5dfd9-106">Model-View-Controller yaklaşımını kullanan bir öğretici arıyorsanız, bkz. [ASP.NET Core MVC ile çalışmaya başlama](xref:tutorials/first-mvc-app/start-mvc).</span><span class="sxs-lookup"><span data-stu-id="5dfd9-106">If you're looking for a tutorial that uses the Model-View-Controller approach, see [Get started with ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span></span>

<span data-ttu-id="5dfd9-107">Bu belge Razor Pages bir giriş sağlar.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-107">This document provides an introduction to Razor Pages.</span></span> <span data-ttu-id="5dfd9-108">Adım adım öğretici değildir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-108">It's not a step by step tutorial.</span></span> <span data-ttu-id="5dfd9-109">Bölümlerden bazılarını çok gelişmiş bir şekilde bulursanız, bkz. [Razor Pages kullanmaya başlama](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="5dfd9-109">If you find some of the sections too advanced, see [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span></span> <span data-ttu-id="5dfd9-110">ASP.NET Core genel bir bakış için bkz. [ASP.NET Core giriş](xref:index).</span><span class="sxs-lookup"><span data-stu-id="5dfd9-110">For an overview of ASP.NET Core, see the [Introduction to ASP.NET Core](xref:index).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5dfd9-111">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="5dfd9-111">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5dfd9-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5dfd9-112">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5dfd9-113">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5dfd9-113">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="5dfd9-114">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5dfd9-114">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

<a name="rpvs17"></a>

## <a name="create-a-razor-pages-project"></a><span data-ttu-id="5dfd9-115">Razor Pages projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="5dfd9-115">Create a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5dfd9-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5dfd9-116">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="5dfd9-117">Razor Pages projesi oluşturma hakkında ayrıntılı yönergeler için bkz. [Razor Pages kullanmaya başlama](xref:tutorials/razor-pages/razor-pages-start) .</span><span class="sxs-lookup"><span data-stu-id="5dfd9-117">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) for detailed instructions on how to create a Razor Pages project.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5dfd9-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5dfd9-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="5dfd9-119">Komut satırından `dotnet new webapp` çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-119">Run `dotnet new webapp` from the command line.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="5dfd9-120">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5dfd9-120">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="5dfd9-121">Komut satırından `dotnet new webapp` çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-121">Run `dotnet new webapp` from the command line.</span></span>

<span data-ttu-id="5dfd9-122">Oluşturulan *. csproj* dosyasını Mac için Visual Studio açın.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-122">Open the generated *.csproj* file from Visual Studio for Mac.</span></span>

---

## <a name="razor-pages"></a><span data-ttu-id="5dfd9-123">Razor Pages</span><span class="sxs-lookup"><span data-stu-id="5dfd9-123">Razor Pages</span></span>

<span data-ttu-id="5dfd9-124">Razor Pages, *Startup.cs*'de etkinleştirilmiştir:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-124">Razor Pages is enabled in *Startup.cs*:</span></span>

[!code-cs[](index/3.0sample/RazorPagesIntro/Startup.cs?name=snippet_Startup&highlight=12)]

<span data-ttu-id="5dfd9-125">Temel bir sayfa düşünün:<a name="OnGet"></a></span><span class="sxs-lookup"><span data-stu-id="5dfd9-125">Consider a basic page: <a name="OnGet"></a></span></span>

[!code-cshtml[](index/3.0sample/RazorPagesIntro/Pages/Index.cshtml?highlight=1)]

<span data-ttu-id="5dfd9-126">Yukarıdaki kod, denetleyiciler ve görünümlerle ASP.NET Core bir uygulamada kullanılan [Razor görünüm dosyası](xref:tutorials/first-mvc-app/adding-view) gibi bir çok şey arar.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-126">The preceding code looks a lot like a [Razor view file](xref:tutorials/first-mvc-app/adding-view) used in an ASP.NET Core app with controllers and views.</span></span> <span data-ttu-id="5dfd9-127">Bu, farklı kılan [@page](xref:mvc/views/razor#page) yönergedir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-127">What makes it different is the [@page](xref:mvc/views/razor#page) directive.</span></span> <span data-ttu-id="5dfd9-128">`@page`, dosyayı bir denetleyiciye geçmeden doğrudan istekleri işlediği anlamına gelen bir MVC eylemine sahip olur.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-128">`@page` makes the file into an MVC action - which means that it handles requests directly, without going through a controller.</span></span> <span data-ttu-id="5dfd9-129">`@page` bir sayfada ilk Razor yönergesi olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-129">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="5dfd9-130">`@page` diğer [Razor](xref:mvc/views/razor) yapıları davranışını etkiler.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-130">`@page` affects the behavior of other [Razor](xref:mvc/views/razor) constructs.</span></span> <span data-ttu-id="5dfd9-131">Razor Pages dosya adlarında *. cshtml* soneki vardır.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-131">Razor Pages file names have a *.cshtml* suffix.</span></span>

<span data-ttu-id="5dfd9-132">`PageModel` sınıfı kullanan benzer bir sayfa aşağıdaki iki dosyada gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-132">A similar page, using a `PageModel` class, is shown in the following two files.</span></span> <span data-ttu-id="5dfd9-133">*Pages/Index2. cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-133">The *Pages/Index2.cshtml* file:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesIntro/Pages/Index2.cshtml)]

<span data-ttu-id="5dfd9-134">*Pages/Index2. cshtml. cs* sayfa modeli:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-134">The *Pages/Index2.cshtml.cs* page model:</span></span>

[!code-cs[](index/3.0sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]

<span data-ttu-id="5dfd9-135">Kurala göre `PageModel` sınıf dosyası, *. cs* eklenmiş Razor sayfası dosyasıyla aynı ada sahiptir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-135">By convention, the `PageModel` class file has the same name as the Razor Page file with *.cs* appended.</span></span> <span data-ttu-id="5dfd9-136">Örneğin, önceki Razor sayfası *Pages/Index2. cshtml*' dir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-136">For example, the previous Razor Page is *Pages/Index2.cshtml*.</span></span> <span data-ttu-id="5dfd9-137">`PageModel` sınıfını içeren dosya *sayfa/Index2. cshtml. cs*olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-137">The file containing the `PageModel` class is named *Pages/Index2.cshtml.cs*.</span></span>

<span data-ttu-id="5dfd9-138">URL yollarının sayfalara olan ilişkilendirmeleri, sayfanın dosya sistemindeki konumuna göre belirlenir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-138">The associations of URL paths to pages are determined by the page's location in the file system.</span></span> <span data-ttu-id="5dfd9-139">Aşağıdaki tabloda bir Razor sayfa yolu ve eşleşen URL gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-139">The following table shows a Razor Page path and the matching URL:</span></span>

| <span data-ttu-id="5dfd9-140">Dosya adı ve yolu</span><span class="sxs-lookup"><span data-stu-id="5dfd9-140">File name and path</span></span>               | <span data-ttu-id="5dfd9-141">eşleşen URL</span><span class="sxs-lookup"><span data-stu-id="5dfd9-141">matching URL</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="5dfd9-142">*/Pages/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="5dfd9-142">*/Pages/Index.cshtml*</span></span> | <span data-ttu-id="5dfd9-143">`/` veya `/Index`</span><span class="sxs-lookup"><span data-stu-id="5dfd9-143">`/` or `/Index`</span></span> |
| <span data-ttu-id="5dfd9-144">*/Pages/Contact.exe*</span><span class="sxs-lookup"><span data-stu-id="5dfd9-144">*/Pages/Contact.cshtml*</span></span> | `/Contact` |
| <span data-ttu-id="5dfd9-145">*/Pages/Store/Contact.exe*</span><span class="sxs-lookup"><span data-stu-id="5dfd9-145">*/Pages/Store/Contact.cshtml*</span></span> | `/Store/Contact` |
| <span data-ttu-id="5dfd9-146">*/Pages/Store/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="5dfd9-146">*/Pages/Store/Index.cshtml*</span></span> | <span data-ttu-id="5dfd9-147">`/Store` veya `/Store/Index`</span><span class="sxs-lookup"><span data-stu-id="5dfd9-147">`/Store` or `/Store/Index`</span></span> |

<span data-ttu-id="5dfd9-148">Notlar:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-148">Notes:</span></span>

* <span data-ttu-id="5dfd9-149">Çalışma zamanı, *Sayfalar* klasöründeki Razor Pages dosyaları varsayılan olarak arar.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-149">The runtime looks for Razor Pages files in the *Pages* folder by default.</span></span>
* <span data-ttu-id="5dfd9-150">`Index`, URL bir sayfa içermiyorsa varsayılan sayfasıdır.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-150">`Index` is the default page when a URL doesn't include a page.</span></span>

## <a name="write-a-basic-form"></a><span data-ttu-id="5dfd9-151">Temel form yazma</span><span class="sxs-lookup"><span data-stu-id="5dfd9-151">Write a basic form</span></span>

<span data-ttu-id="5dfd9-152">Razor Pages, Web tarayıcıları ile kullanılan ortak desenleri bir uygulama oluştururken kolayca uygulanması için tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-152">Razor Pages is designed to make common patterns used with web browsers easy to implement when building an app.</span></span> <span data-ttu-id="5dfd9-153">[Model bağlama](xref:mvc/models/model-binding), [ETIKET yardımcıları](xref:mvc/views/tag-helpers/intro)ve HTML Yardımcıları hepsi, Razor sayfası sınıfında tanımlanan özelliklerle *çalışır* .</span><span class="sxs-lookup"><span data-stu-id="5dfd9-153">[Model binding](xref:mvc/models/model-binding), [Tag Helpers](xref:mvc/views/tag-helpers/intro), and HTML helpers all *just work* with the properties defined in a Razor Page class.</span></span> <span data-ttu-id="5dfd9-154">`Contact` modeli için temel bir "bize başvurun" formu uygulayan bir sayfa düşünün:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-154">Consider a page that implements a basic "contact us" form for the `Contact` model:</span></span>

<span data-ttu-id="5dfd9-155">Bu belgedeki örnekler için `DbContext`, [Startup.cs](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/razor-pages/index/3.0sample/RazorPagesContacts/Startup.cs#L23-L24) dosyasında başlatılır.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-155">For the samples in this document, the `DbContext` is initialized in the [Startup.cs](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/razor-pages/index/3.0sample/RazorPagesContacts/Startup.cs#L23-L24) file.</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Startup.cs?name=snippet)]

<span data-ttu-id="5dfd9-156">Veri modeli:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-156">The data model:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Models/Customer.cs)]

<span data-ttu-id="5dfd9-157">DB bağlamı:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-157">The db context:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Data/CustomerDbContext.cs)]

<span data-ttu-id="5dfd9-158">*Pages/Create. cshtml* görünüm dosyası:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-158">The *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml)]

<span data-ttu-id="5dfd9-159">*Pages/Create. cshtml. cs* sayfa modeli:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-159">The *Pages/Create.cshtml.cs* page model:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml.cs?name=snippet_ALL)]

<span data-ttu-id="5dfd9-160">Kurala göre `PageModel` sınıfı `<PageName>Model` olarak adlandırılır ve sayfayla aynı ad alanında yer alan.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-160">By convention, the `PageModel` class is called `<PageName>Model` and is in the same namespace as the page.</span></span>

<span data-ttu-id="5dfd9-161">`PageModel` sınıfı, bir sayfa mantığının sunumundaki ayırmayı sağlar.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-161">The `PageModel` class allows separation of the logic of a page from its presentation.</span></span> <span data-ttu-id="5dfd9-162">Sayfaya gönderilen istekler için sayfa işleyicilerini ve sayfayı işlemek için kullanılan verileri tanımlar.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-162">It defines page handlers for requests sent to the page and the data used to render the page.</span></span> <span data-ttu-id="5dfd9-163">Bu ayrım şunları sağlar:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-163">This separation allows:</span></span>

* <span data-ttu-id="5dfd9-164">[Bağımlılık ekleme](xref:fundamentals/dependency-injection)yoluyla sayfa bağımlılıklarını yönetme.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-164">Managing of page dependencies through [dependency injection](xref:fundamentals/dependency-injection).</span></span>
* [<span data-ttu-id="5dfd9-165">Birim testi</span><span class="sxs-lookup"><span data-stu-id="5dfd9-165">Unit testing</span></span>](xref:test/razor-pages-tests)

<span data-ttu-id="5dfd9-166">Sayfada, `POST` isteklerinde çalışan bir `OnPostAsync` *işleyicisi yöntemi*vardır (bir Kullanıcı formu gönderdiğinde).</span><span class="sxs-lookup"><span data-stu-id="5dfd9-166">The page has an `OnPostAsync` *handler method*, which runs on `POST` requests (when a user posts the form).</span></span> <span data-ttu-id="5dfd9-167">Herhangi bir HTTP fiili için işleyici metotları eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-167">Handler methods for any HTTP verb can be added.</span></span> <span data-ttu-id="5dfd9-168">En yaygın işleyiciler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-168">The most common handlers are:</span></span>

* <span data-ttu-id="5dfd9-169">Sayfanın başlatılması için gereken durum `OnGet`.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-169">`OnGet` to initialize state needed for the page.</span></span> <span data-ttu-id="5dfd9-170">Yukarıdaki kodda `OnGet` yöntemi *CreateModel. cshtml* Razor sayfasını görüntüler.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-170">In the preceding code, the `OnGet` method displays the *CreateModel.cshtml* Razor Page.</span></span>
* <span data-ttu-id="5dfd9-171">form gönderilerini işlemek için `OnPost`.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-171">`OnPost` to handle form submissions.</span></span>

<span data-ttu-id="5dfd9-172">`Async` adlandırma son eki isteğe bağlıdır, ancak genellikle zaman uyumsuz işlevler için kural tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-172">The `Async` naming suffix is optional but is often used by convention for asynchronous functions.</span></span> <span data-ttu-id="5dfd9-173">Yukarıdaki kod Razor Pages için tipik bir davranıştır.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-173">The preceding code is typical for Razor Pages.</span></span>

<span data-ttu-id="5dfd9-174">Denetleyicileri ve görünümleri kullanarak ASP.NET uygulamaları hakkında bilginiz varsa:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-174">If you're familiar with ASP.NET apps using controllers and views:</span></span>

* <span data-ttu-id="5dfd9-175">Yukarıdaki örnekteki `OnPostAsync` kodu, tipik denetleyici koduna benzer şekilde görünür.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-175">The `OnPostAsync` code in the preceding example looks similar to typical controller code.</span></span>
* <span data-ttu-id="5dfd9-176">[Model bağlama](xref:mvc/models/model-binding), [doğrulama](xref:mvc/models/validation)ve eylem sonuçları gibi mvc temel elemanlarının çoğu denetleyiciler ve Razor Pages aynı şekilde çalışır.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-176">Most of the MVC primitives like [model binding](xref:mvc/models/model-binding), [validation](xref:mvc/models/validation), and action results work the same with Controllers and Razor Pages.</span></span> 

<span data-ttu-id="5dfd9-177">Önceki `OnPostAsync` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-177">The previous `OnPostAsync` method:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="5dfd9-178">`OnPostAsync`temel akışı:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-178">The basic flow of `OnPostAsync`:</span></span>

<span data-ttu-id="5dfd9-179">Doğrulama hatalarını kontrol edin.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-179">Check for validation errors.</span></span>

* <span data-ttu-id="5dfd9-180">Hata yoksa, verileri kaydedin ve yeniden yönlendirin.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-180">If there are no errors, save the data and redirect.</span></span>
* <span data-ttu-id="5dfd9-181">Hatalar varsa, doğrulama iletileriyle sayfayı yeniden görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-181">If there are errors, show the page again with validation messages.</span></span> <span data-ttu-id="5dfd9-182">Çoğu durumda, istemci üzerinde doğrulama hataları algılanır ve sunucuya hiçbir zaman gönderilmez.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-182">In many cases, validation errors would be detected on the client, and never submitted to the server.</span></span>

<span data-ttu-id="5dfd9-183">*Pages/Create. cshtml* görünüm dosyası:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-183">The *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml)]

<span data-ttu-id="5dfd9-184">Sayfalardan işlenmiş HTML */Create. cshtml*:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-184">The rendered HTML from *Pages/Create.cshtml*:</span></span>

[!code-html[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create4.html)]

<span data-ttu-id="5dfd9-185">Önceki kodda, formu deftere nakletme:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-185">In the previous code, posting the form:</span></span>

* <span data-ttu-id="5dfd9-186">Geçerli verilerle:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-186">With valid data:</span></span>

  * <span data-ttu-id="5dfd9-187">`OnPostAsync` Handler yöntemi <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel.RedirectToPage*> yardımcı yöntemini çağırır.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-187">The `OnPostAsync` handler method calls the <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel.RedirectToPage*> helper method.</span></span> <span data-ttu-id="5dfd9-188">`RedirectToPage`, <xref:Microsoft.AspNetCore.Mvc.RedirectToPageResult>örneğini döndürür.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-188">`RedirectToPage` returns an instance of <xref:Microsoft.AspNetCore.Mvc.RedirectToPageResult>.</span></span> <span data-ttu-id="5dfd9-189">`RedirectToPage`:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-189">`RedirectToPage`:</span></span>

    * <span data-ttu-id="5dfd9-190">Bir eylem sonucudur.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-190">Is an action result.</span></span>
    * <span data-ttu-id="5dfd9-191">`RedirectToAction` veya `RedirectToRoute` benzerdir (denetleyiciler ve görünümlerde kullanılır).</span><span class="sxs-lookup"><span data-stu-id="5dfd9-191">Is similar to `RedirectToAction` or `RedirectToRoute` (used in controllers and views).</span></span>
    * <span data-ttu-id="5dfd9-192">Sayfalar için özelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-192">Is customized for pages.</span></span> <span data-ttu-id="5dfd9-193">Önceki örnekte, kök dizin sayfasına (`/Index`) yeniden yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-193">In the preceding sample, it redirects to the root Index page (`/Index`).</span></span> <span data-ttu-id="5dfd9-194">`RedirectToPage`, [Sayfalar Için URL oluşturma](#url_gen) bölümünde ayrıntılı olarak açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-194">`RedirectToPage` is detailed in the [URL generation for Pages](#url_gen) section.</span></span>

* <span data-ttu-id="5dfd9-195">Sunucuya geçirilen doğrulama hatalarıyla birlikte:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-195">With validation errors that are passed to the server:</span></span>

  * <span data-ttu-id="5dfd9-196">`OnPostAsync` Handler yöntemi <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageBase.Page*> yardımcı yöntemini çağırır.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-196">The `OnPostAsync` handler method calls the <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageBase.Page*> helper method.</span></span> <span data-ttu-id="5dfd9-197">`Page`, <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageResult>örneğini döndürür.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-197">`Page` returns an instance of <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageResult>.</span></span> <span data-ttu-id="5dfd9-198">`Page` döndürmek, denetleyicilerde eylemlerin `View`nasıl dönüşlerine benzer.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-198">Returning `Page` is similar to how actions in controllers return `View`.</span></span> <span data-ttu-id="5dfd9-199">`PageResult`, bir işleyici yöntemi için varsayılan dönüş türüdür.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-199">`PageResult` is the default return type for a handler method.</span></span> <span data-ttu-id="5dfd9-200">`void` döndüren bir işleyici yöntemi sayfayı işler.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-200">A handler method that returns `void` renders the page.</span></span>
  * <span data-ttu-id="5dfd9-201">Yukarıdaki örnekte, formun hiçbir değer olmadan nakledilmesi [ModelState ile sonuçlanır. IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) yanlış döndürüyor.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-201">In the preceding example, posting the form with no value results in [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) returning false.</span></span> <span data-ttu-id="5dfd9-202">Bu örnekte, istemcide hiçbir doğrulama hatası gösterilmezler.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-202">In this sample, no validation errors are displayed on the client.</span></span> <span data-ttu-id="5dfd9-203">Doğrulama hatası teslim etme bu belgenin ilerleyen bölümlerinde ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-203">Validation error handing is covered later in this document.</span></span>

  [!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=3-6)]

* <span data-ttu-id="5dfd9-204">İstemci tarafı doğrulaması tarafından algılanan doğrulama hatalarıyla birlikte:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-204">With validation errors detected by client side validation:</span></span>

  * <span data-ttu-id="5dfd9-205">Veriler sunucuya **nakledilmedi.**</span><span class="sxs-lookup"><span data-stu-id="5dfd9-205">Data is **not** posted to the server.</span></span>
  * <span data-ttu-id="5dfd9-206">İstemci tarafı doğrulaması bu belgenin ilerleyen kısımlarında açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-206">Client-side validation is explained later in this document.</span></span>

<span data-ttu-id="5dfd9-207">`Customer` özelliği, model bağlamasını kabul etmek için [`[BindProperty]`](xref:Microsoft.AspNetCore.Mvc.BindPropertyAttribute) özniteliğini kullanır:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-207">The `Customer` property uses [`[BindProperty]`](xref:Microsoft.AspNetCore.Mvc.BindPropertyAttribute) attribute to opt in to model binding:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml.cs?name=snippet_PageModel&highlight=15-16)]

<span data-ttu-id="5dfd9-208">`[BindProperty]`, istemci tarafından değiştirilmemesi gereken özellikler içeren **modellerde kullanılmamalıdır.**</span><span class="sxs-lookup"><span data-stu-id="5dfd9-208">`[BindProperty]` should **not** be used on models containing properties that should not be changed by the client.</span></span> <span data-ttu-id="5dfd9-209">Daha fazla bilgi için bkz. fazla [nakil](xref:data/ef-rp/crud#overposting).</span><span class="sxs-lookup"><span data-stu-id="5dfd9-209">For more information, see [Overposting](xref:data/ef-rp/crud#overposting).</span></span>

<span data-ttu-id="5dfd9-210">Razor Pages, varsayılan olarak, özellikleri yalnızca`GET` olmayan fiiller ile bağlayın.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-210">Razor Pages, by default, bind properties only with non-`GET` verbs.</span></span> <span data-ttu-id="5dfd9-211">Özelliklere bağlama, HTTP verilerini model türüne dönüştürmek için kod yazma ihtiyacını ortadan kaldırır.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-211">Binding to properties removes the need to writing code to convert HTTP data to the model type.</span></span> <span data-ttu-id="5dfd9-212">Bağlama, form alanlarını işlemek için aynı özelliği kullanarak kodu azaltır (`<input asp-for="Customer.Name">`) ve girişi kabul eder.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-212">Binding reduces code by using the same property to render form fields (`<input asp-for="Customer.Name">`) and accept the input.</span></span>

[!INCLUDE[](~/includes/bind-get.md)]

<span data-ttu-id="5dfd9-213">*Sayfalar/oluşturma. cshtml* görünüm dosyası gözden geçiriliyor:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-213">Reviewing the *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml?highlight=3,9)]

* <span data-ttu-id="5dfd9-214">Yukarıdaki kodda, [giriş etiketi yardımcısı](xref:mvc/views/working-with-forms#the-input-tag-helper) `<input asp-for="Customer.Name" />` HTML `<input>` öğesini `Customer.Name` model ifadesine bağlar.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-214">In the preceding code, the [input tag helper](xref:mvc/views/working-with-forms#the-input-tag-helper) `<input asp-for="Customer.Name" />` binds the HTML `<input>` element to the `Customer.Name` model expression.</span></span>
* <span data-ttu-id="5dfd9-215">[`@addTagHelper`](xref:mvc/views/tag-helpers/intro#addtaghelper-makes-tag-helpers-available) etiket yardımcılarını kullanılabilir hale getirir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-215">[`@addTagHelper`](xref:mvc/views/tag-helpers/intro#addtaghelper-makes-tag-helpers-available) makes Tag Helpers available.</span></span>

### <a name="the-home-page"></a><span data-ttu-id="5dfd9-216">Giriş sayfası</span><span class="sxs-lookup"><span data-stu-id="5dfd9-216">The home page</span></span>

<span data-ttu-id="5dfd9-217">*Index. cshtml* giriş sayfasıdır:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-217">*Index.cshtml* is the home page:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Index.cshtml)]

<span data-ttu-id="5dfd9-218">İlişkili `PageModel` sınıfı (*Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="5dfd9-218">The associated `PageModel` class (*Index.cshtml.cs*):</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Index.cshtml.cs?name=snippet)]

<span data-ttu-id="5dfd9-219">*Index. cshtml* dosyası aşağıdaki biçimlendirmeyi içerir:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-219">The *Index.cshtml* file contains the following markup:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Index.cshtml?range=21)]

<span data-ttu-id="5dfd9-220">`<a /a>` [tutturucu etiketi Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) , düzenleme sayfasına bir bağlantı oluşturmak için `asp-route-{value}` özniteliğini kullandı.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-220">The `<a /a>` [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) used the `asp-route-{value}` attribute to generate a link to the Edit page.</span></span> <span data-ttu-id="5dfd9-221">Bağlantı, iletişim KIMLIĞINE sahip rota verileri içerir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-221">The link contains route data with the contact ID.</span></span> <span data-ttu-id="5dfd9-222">Örneğin, `https://localhost:5001/Edit/1`.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-222">For example, `https://localhost:5001/Edit/1`.</span></span> <span data-ttu-id="5dfd9-223">[Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro), Razor dosyalarında HTML öğelerinin oluşturulmasına ve işlenmesine sunucu tarafı kodun katılmasını etkinleştir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-223">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span>

<span data-ttu-id="5dfd9-224">*Index. cshtml* dosyası her müşteri için bir silme düğmesi oluşturmak için biçimlendirme içerir:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-224">The *Index.cshtml* file contains markup to create a delete button for each customer contact:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Index.cshtml?range=22-23)]

<span data-ttu-id="5dfd9-225">İşlenmiş HTML:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-225">The rendered HTML:</span></span>

```HTML
<button type="submit" formaction="/Customers?id=1&amp;handler=delete">delete</button>
```

<span data-ttu-id="5dfd9-226">Sil düğmesi HTML 'de işlendiğinde, bu nesnenin [biçimlendirme](https://developer.mozilla.org/docs/Web/HTML/Element/button#attr-formaction) parametreleri içerir:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-226">When the delete button is rendered in HTML, its [formaction](https://developer.mozilla.org/docs/Web/HTML/Element/button#attr-formaction) includes parameters for:</span></span>

* <span data-ttu-id="5dfd9-227">`asp-route-id` özniteliğiyle belirtilen müşteri iletişim KIMLIĞI.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-227">The customer contact ID, specified by the `asp-route-id` attribute.</span></span>
* <span data-ttu-id="5dfd9-228">`asp-page-handler` özniteliğiyle belirtilen `handler`.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-228">The `handler`, specified by the `asp-page-handler` attribute.</span></span>

<span data-ttu-id="5dfd9-229">Düğme seçildiğinde, sunucuya bir form `POST` isteği gönderilir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-229">When the button is selected, a form `POST` request is sent to the server.</span></span> <span data-ttu-id="5dfd9-230">Kurala göre, işleyici yönteminin adı, düzen `OnPost[handler]Async`göre `handler` parametresinin değerine göre seçilir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-230">By convention, the name of the handler method is selected based on the value of the `handler` parameter according to the scheme `OnPost[handler]Async`.</span></span>

<span data-ttu-id="5dfd9-231">Bu örnekte `handler` `delete` olduğundan, `OnPostDeleteAsync` Handler yöntemi `POST` isteğini işlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-231">Because the `handler` is `delete` in this example, the `OnPostDeleteAsync` handler method is used to process the `POST` request.</span></span> <span data-ttu-id="5dfd9-232">`asp-page-handler`, `remove`gibi farklı bir değere ayarlandıysa `OnPostRemoveAsync` ada sahip bir işleyici yöntemi seçilidir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-232">If the `asp-page-handler` is set to a different value, such as `remove`, a handler method with the name `OnPostRemoveAsync` is selected.</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Index.cshtml.cs?name=snippet2)]

<span data-ttu-id="5dfd9-233">`OnPostDeleteAsync` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-233">The `OnPostDeleteAsync` method:</span></span>

* <span data-ttu-id="5dfd9-234">Sorgu dizesinden `id` alır.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-234">Gets the `id` from the query string.</span></span>
* <span data-ttu-id="5dfd9-235">`FindAsync`ile müşteri iletişim için veritabanını sorgular.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-235">Queries the database for the customer contact with `FindAsync`.</span></span>
* <span data-ttu-id="5dfd9-236">Müşteri ilgili kişisi bulunursa, kaldırılır ve veritabanı güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-236">If the customer contact is found, it's removed and the database is updated.</span></span>
* <span data-ttu-id="5dfd9-237">Kök dizin sayfasına yeniden yönlendirmek için <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel.RedirectToPage*> çağırır (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="5dfd9-237">Calls <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel.RedirectToPage*> to redirect to the root Index page (`/Index`).</span></span>

### <a name="the-editcshtml-file"></a><span data-ttu-id="5dfd9-238">Edit. cshtml dosyası</span><span class="sxs-lookup"><span data-stu-id="5dfd9-238">The Edit.cshtml file</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Edit.cshtml?highlight=1)]

<span data-ttu-id="5dfd9-239">İlk satır `@page "{id:int}"` yönergesini içerir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-239">The first line contains the `@page "{id:int}"` directive.</span></span> <span data-ttu-id="5dfd9-240">Yönlendirme kısıtlaması`"{id:int}"`, sayfaya istekleri `int` yönlendirme verileri içeren sayfaya kabul etmesini söyler.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-240">The routing constraint`"{id:int}"` tells the page to accept requests to the page that contain `int` route data.</span></span> <span data-ttu-id="5dfd9-241">Sayfaya yapılan bir istek bir `int`dönüştürülebildiği rota verileri içermiyorsa, çalışma zamanı bir HTTP 404 (bulunamadı) hatası döndürür.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-241">If a request to the page doesn't contain route data that can be converted to an `int`, the runtime returns an HTTP 404 (not found) error.</span></span> <span data-ttu-id="5dfd9-242">KIMLIĞI isteğe bağlı yapmak için, yol kısıtlamasına `?` ekleyin:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-242">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="5dfd9-243">*Edit.cshtml.cs* dosyası:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-243">The *Edit.cshtml.cs* file:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Edit.cshtml.cs?name=snippet)]

## <a name="validation"></a><span data-ttu-id="5dfd9-244">Doğrulama</span><span class="sxs-lookup"><span data-stu-id="5dfd9-244">Validation</span></span>

<span data-ttu-id="5dfd9-245">Doğrulama kuralları:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-245">Validation rules:</span></span>

* <span data-ttu-id="5dfd9-246">Model sınıfında bildirimli olarak belirtilir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-246">Are declaratively specified in the model class.</span></span>
* <span data-ttu-id="5dfd9-247">Uygulamada her yerde zorlanır.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-247">Are enforced everywhere in the app.</span></span>

<span data-ttu-id="5dfd9-248"><xref:System.ComponentModel.DataAnnotations> ad alanı, bir sınıfa veya özelliğe bildirimli olarak uygulanan bir yerleşik doğrulama öznitelikleri kümesi sağlar.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-248">The <xref:System.ComponentModel.DataAnnotations> namespace provides a set of built-in validation attributes that are applied declaratively to a class or property.</span></span> <span data-ttu-id="5dfd9-249">Veri açıklamaları, biçimlendirme ile yardım eden [`[DataType]`](xref:System.ComponentModel.DataAnnotations.DataTypeAttribute) gibi biçimlendirme özniteliklerini de içerir ve herhangi bir doğrulama sağlamaz.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-249">DataAnnotations also contains formatting attributes like [`[DataType]`](xref:System.ComponentModel.DataAnnotations.DataTypeAttribute) that help with formatting and don't provide any validation.</span></span>

<span data-ttu-id="5dfd9-250">`Customer` modelini göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-250">Consider the `Customer` model:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Models/Customer.cs)]

<span data-ttu-id="5dfd9-251">Aşağıdaki *Create. cshtml* görünüm dosyasını kullanarak:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-251">Using the following *Create.cshtml* view file:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create3.cshtml?highlight=3,8-9,15-99)]

<span data-ttu-id="5dfd9-252">Önceki kod:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-252">The preceding code:</span></span>

* <span data-ttu-id="5dfd9-253">JQuery ve jQuery doğrulama betikleri içerir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-253">Includes jQuery and jQuery validation scripts.</span></span>
* <span data-ttu-id="5dfd9-254">Etkinleştirmek için `<div />` ve `<span />` [etiketi yardımcıları](xref:mvc/views/tag-helpers/intro) kullanır:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-254">Uses the `<div />` and `<span />` [Tag Helpers](xref:mvc/views/tag-helpers/intro) to enable:</span></span>

  * <span data-ttu-id="5dfd9-255">İstemci tarafı doğrulama.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-255">Client-side validation.</span></span>
  * <span data-ttu-id="5dfd9-256">Doğrulama hatası işleme.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-256">Validation error rendering.</span></span>

* <span data-ttu-id="5dfd9-257">Aşağıdaki HTML 'yi oluşturur:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-257">Generates the following HTML:</span></span>

  [!code-html[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create5.html)]

<span data-ttu-id="5dfd9-258">Create formunu ad değeri olmadan göndermek "ad alanı gereklidir" hata iletisini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-258">Posting the Create form without a name value displays the error message "The Name field is required."</span></span> <span data-ttu-id="5dfd9-259">formunda.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-259">on the form.</span></span> <span data-ttu-id="5dfd9-260">İstemcide JavaScript etkinse tarayıcı, sunucuya göndermeden hatayı görüntüler.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-260">If JavaScript is enabled on the client, the browser displays the error without posting to the server.</span></span>

<span data-ttu-id="5dfd9-261">`[StringLength(10)]` özniteliği işlenmiş HTML üzerinde `data-val-length-max="10"` oluşturur.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-261">The `[StringLength(10)]` attribute generates `data-val-length-max="10"` on the rendered HTML.</span></span> <span data-ttu-id="5dfd9-262">`data-val-length-max`, tarayıcıların belirtilen uzunluk üst sınırından daha fazlasını girmesini engeller.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-262">`data-val-length-max` prevents browsers from entering more than the maximum length specified.</span></span> <span data-ttu-id="5dfd9-263">Gönderiyi düzenlemek ve yeniden oynatmak için [Fiddler](https://www.telerik.com/fiddler) gibi bir araç kullanılıyorsa:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-263">If a tool such as [Fiddler](https://www.telerik.com/fiddler) is used to edit and replay the post:</span></span>

* <span data-ttu-id="5dfd9-264">, Adı 10 ' dan daha uzun.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-264">With the name longer than 10.</span></span>
* <span data-ttu-id="5dfd9-265">"Alan adı, en fazla 10 uzunluğunda bir dize olmalıdır" hata iletisi.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-265">The error message "The field Name must be a string with a maximum length of 10."</span></span> <span data-ttu-id="5dfd9-266">döndürülür.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-266">is returned.</span></span>

<span data-ttu-id="5dfd9-267">Aşağıdaki `Movie` modelini göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-267">Consider the following `Movie` model:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Models/MovieDateRatingDA.cs?name=snippet1)]

<span data-ttu-id="5dfd9-268">Doğrulama öznitelikleri, uygulanan model özellikleri üzerinde zorlamak için davranışı belirtir:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-268">The validation attributes specify behavior to enforce on the model properties they're applied to:</span></span>

* <span data-ttu-id="5dfd9-269">`Required` ve `MinimumLength` öznitelikleri bir özelliğin bir değere sahip olması gerektiğini belirtir, ancak hiçbir şey, kullanıcının bu doğrulamayı karşılamak için boşluk girmesini engeller.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-269">The `Required` and `MinimumLength` attributes indicate that a property must have a value, but nothing prevents a user from entering white space to satisfy this validation.</span></span>
* <span data-ttu-id="5dfd9-270">`RegularExpression` özniteliği, hangi karakterlerin giriş yapabileceğini sınırlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-270">The `RegularExpression` attribute is used to limit what characters can be input.</span></span> <span data-ttu-id="5dfd9-271">Yukarıdaki kodda, "tarz":</span><span class="sxs-lookup"><span data-stu-id="5dfd9-271">In the preceding code, "Genre":</span></span>

  * <span data-ttu-id="5dfd9-272">Yalnızca harfler kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-272">Must only use letters.</span></span>
  * <span data-ttu-id="5dfd9-273">İlk harfin büyük harfle olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-273">The first letter is required to be uppercase.</span></span> <span data-ttu-id="5dfd9-274">Boşluk, sayı ve özel karakterlere izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-274">White space, numbers, and special characters are not allowed.</span></span>

* <span data-ttu-id="5dfd9-275">`RegularExpression` "derecelendirmesi":</span><span class="sxs-lookup"><span data-stu-id="5dfd9-275">The `RegularExpression` "Rating":</span></span>

  * <span data-ttu-id="5dfd9-276">İlk karakterin büyük harf olmasını gerektirir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-276">Requires that the first character be an uppercase letter.</span></span>
  * <span data-ttu-id="5dfd9-277">Sonraki boşlukların içindeki özel karakter ve sayılara izin verir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-277">Allows special characters and numbers in subsequent spaces.</span></span> <span data-ttu-id="5dfd9-278">"PG-13" bir derecelendirme için geçerlidir, ancak bir "tarz" için başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-278">"PG-13" is valid for a rating, but fails for a "Genre".</span></span>

* <span data-ttu-id="5dfd9-279">`Range` özniteliği bir değeri belirtilen bir Aralık içinde kısıtlar.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-279">The `Range` attribute constrains a value to within a specified range.</span></span>
* <span data-ttu-id="5dfd9-280">`StringLength` özniteliği, bir dize özelliğinin en büyük uzunluğunu ve isteğe bağlı olarak en düşük uzunluğunu ayarlar.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-280">The `StringLength` attribute sets the maximum length of a string property, and optionally its minimum length.</span></span>
* <span data-ttu-id="5dfd9-281">Değer türleri (örneğin `decimal`, `int`, `float`, `DateTime`), doğal olarak gereklidir ve `[Required]` özniteliğine gerek kalmaz.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-281">Value types (such as `decimal`, `int`, `float`, `DateTime`) are inherently required and don't need the `[Required]` attribute.</span></span>

<span data-ttu-id="5dfd9-282">`Movie` modeli için Oluştur sayfasında, geçersiz değerlere sahip hatalar görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-282">The Create page for the `Movie` model shows displays errors with invalid values:</span></span>

![Birden çok jQuery istemci tarafı doğrulama hatası içeren film görünümü formu](~/tutorials/razor-pages/validation/_static/val.png)

<span data-ttu-id="5dfd9-284">Daha fazla bilgi için bkz.:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-284">For more information, see:</span></span>

* [<span data-ttu-id="5dfd9-285">Film uygulamasına doğrulama ekleme</span><span class="sxs-lookup"><span data-stu-id="5dfd9-285">Add validation to the Movie app</span></span>](xref:tutorials/razor-pages/validation)
* <span data-ttu-id="5dfd9-286">[ASP.NET Core 'de model doğrulaması](xref:mvc/models/validation).</span><span class="sxs-lookup"><span data-stu-id="5dfd9-286">[Model validation in ASP.NET Core](xref:mvc/models/validation).</span></span>

## <a name="handle-head-requests-with-an-onget-handler-fallback"></a><span data-ttu-id="5dfd9-287">OnGet işleyicisi geri dönüşü ile tanıtıcı HEAD istekleri</span><span class="sxs-lookup"><span data-stu-id="5dfd9-287">Handle HEAD requests with an OnGet handler fallback</span></span>

<span data-ttu-id="5dfd9-288">`HEAD` istekleri belirli bir kaynağın üst bilgilerini almaya izin verir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-288">`HEAD` requests allow retrieving the headers for a specific resource.</span></span> <span data-ttu-id="5dfd9-289">`GET` isteklerinin aksine `HEAD` istekleri bir yanıt gövdesi döndürmez.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-289">Unlike `GET` requests, `HEAD` requests don't return a response body.</span></span>

<span data-ttu-id="5dfd9-290">Normalde, `HEAD` istekleri için `OnHead` işleyicisi oluşturulur ve çağırılır:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-290">Ordinarily, an `OnHead` handler is created and called for `HEAD` requests:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Privacy.cshtml.cs?name=snippet)]

<span data-ttu-id="5dfd9-291">Razor Pages, `OnHead` işleyicisi tanımlanmamışsa `OnGet` işleyicisini çağırmaya geri döner.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-291">Razor Pages falls back to calling the `OnGet` handler if no `OnHead` handler is defined.</span></span>

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a><span data-ttu-id="5dfd9-292">XSRF/CSRF ve Razor Pages</span><span class="sxs-lookup"><span data-stu-id="5dfd9-292">XSRF/CSRF and Razor Pages</span></span>

<span data-ttu-id="5dfd9-293">Razor Pages, [Antiforgery doğrulaması](xref:security/anti-request-forgery)tarafından korunur.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-293">Razor Pages are protected by [Antiforgery validation](xref:security/anti-request-forgery).</span></span> <span data-ttu-id="5dfd9-294">[Formtaghelper](xref:mvc/views/working-with-forms#the-form-tag-helper) , antiforgery belirteçlerini HTML form öğelerine çıkartır.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-294">The [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) injects antiforgery tokens into HTML form elements.</span></span>

<a name="layout"></a>

## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a><span data-ttu-id="5dfd9-295">Razor Pages ile düzenleri, partileri, şablonları ve etiket yardımcılarını kullanma</span><span class="sxs-lookup"><span data-stu-id="5dfd9-295">Using Layouts, partials, templates, and Tag Helpers with Razor Pages</span></span>

<span data-ttu-id="5dfd9-296">Sayfalar, Razor görünüm altyapısının tüm özellikleri ile çalışır.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-296">Pages work with all the capabilities of the Razor view engine.</span></span> <span data-ttu-id="5dfd9-297">Düzenler, partıals, şablonlar, etiket yardımcıları, *_Viewstart. cshtml*ve *_Viewwimports. cshtml* geleneksel Razor görünümlerinde oldukları gibi çalışır.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-297">Layouts, partials, templates, Tag Helpers, *_ViewStart.cshtml*, and *_ViewImports.cshtml* work in the same way they do for conventional Razor views.</span></span>

<span data-ttu-id="5dfd9-298">Bu özelliklerden bazılarının avantajlarından yararlanarak bu sayfayı declutter edelim.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-298">Let's declutter this page by taking advantage of some of those capabilities.</span></span>

<span data-ttu-id="5dfd9-299">*Pages/Shared/_Layout. cshtml*öğesine bir [Düzen sayfası](xref:mvc/views/layout) ekleyin:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-299">Add a [layout page](xref:mvc/views/layout) to *Pages/Shared/_Layout.cshtml*:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Shared/_Layout2.cshtml?hightlight=12)]

<span data-ttu-id="5dfd9-300">[Düzen](xref:mvc/views/layout):</span><span class="sxs-lookup"><span data-stu-id="5dfd9-300">The [Layout](xref:mvc/views/layout):</span></span>

* <span data-ttu-id="5dfd9-301">Her sayfanın yerleşimini denetler (sayfa düzen dışında değilse).</span><span class="sxs-lookup"><span data-stu-id="5dfd9-301">Controls the layout of each page (unless the page opts out of layout).</span></span>
* <span data-ttu-id="5dfd9-302">JavaScript ve stil sayfaları gibi HTML yapılarını içeri aktarır.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-302">Imports HTML structures such as JavaScript and stylesheets.</span></span>
* <span data-ttu-id="5dfd9-303">Razor sayfasının içerikleri `@RenderBody()` her çağrıldığında işlenir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-303">The contents of the Razor page are rendered where `@RenderBody()` is called.</span></span>

<span data-ttu-id="5dfd9-304">Daha fazla bilgi için bkz. [Düzen sayfası](xref:mvc/views/layout).</span><span class="sxs-lookup"><span data-stu-id="5dfd9-304">For more information, see [layout page](xref:mvc/views/layout).</span></span>

<span data-ttu-id="5dfd9-305">[Layout](xref:mvc/views/layout#specifying-a-layout) özelliği *Pages/_viewstart. cshtml*içinde ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-305">The [Layout](xref:mvc/views/layout#specifying-a-layout) property is set in *Pages/_ViewStart.cshtml*:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]

<span data-ttu-id="5dfd9-306">Düzen *Sayfalar/paylaşılan* klasöründedir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-306">The layout is in the *Pages/Shared* folder.</span></span> <span data-ttu-id="5dfd9-307">Sayfalar, geçerli sayfayla aynı klasörden başlayarak diğer görünümleri (düzenler, şablonlar, parals) hiyerarşik olarak arar.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-307">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="5dfd9-308">*Sayfalar/paylaşılan* klasördeki bir düzen, *Sayfalar* klasörü altındaki herhangi bir Razor sayfasından kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-308">A layout in the *Pages/Shared* folder can be used from any Razor page under the *Pages* folder.</span></span>

<span data-ttu-id="5dfd9-309">Düzen dosyası *Sayfalar/paylaşılan* klasörüne gitmelidir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-309">The layout file should go in the *Pages/Shared* folder.</span></span>

<span data-ttu-id="5dfd9-310">Düzen dosyasını *Görünümler/paylaşılan* klasöre **yerleştirmenizi öneririz** .</span><span class="sxs-lookup"><span data-stu-id="5dfd9-310">We recommend you **not** put the layout file in the *Views/Shared* folder.</span></span> <span data-ttu-id="5dfd9-311">*Görünümler/paylaşılan* bir MVC görünümleri modelidir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-311">*Views/Shared* is an MVC views pattern.</span></span> <span data-ttu-id="5dfd9-312">Razor Pages, yol kurallarını değil klasör hiyerarşisine güvenmektir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-312">Razor Pages are meant to rely on folder hierarchy, not path conventions.</span></span>

<span data-ttu-id="5dfd9-313">Bir Razor sayfasından arama görüntüleme, *Sayfalar* klasörünü içerir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-313">View search from a Razor Page includes the *Pages* folder.</span></span> <span data-ttu-id="5dfd9-314">MVC denetleyicileri ve geleneksel Razor görünümleriyle kullanılan düzenler, şablonlar ve partilar *yalnızca çalışır*.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-314">The layouts, templates, and partials used with MVC controllers and conventional Razor views *just work*.</span></span>

<span data-ttu-id="5dfd9-315">*Pages/_Viewwimports. cshtml* dosyası ekleyin:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-315">Add a *Pages/_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]

<span data-ttu-id="5dfd9-316">`@namespace` öğreticide daha sonra açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-316">`@namespace` is explained later in the tutorial.</span></span> <span data-ttu-id="5dfd9-317">`@addTagHelper` yönergesi, [yerleşik etiket yardımcılarını](xref:mvc/views/tag-helpers/builtin-th/Index) *Sayfalar* klasöründeki tüm sayfalara getirir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-317">The `@addTagHelper` directive brings in the [built-in Tag Helpers](xref:mvc/views/tag-helpers/builtin-th/Index) to all the pages in the *Pages* folder.</span></span>

<a name="namespace"></a>

<span data-ttu-id="5dfd9-318">Bir sayfada `@namespace` yönerge kümesi:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-318">The `@namespace` directive set on a page:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]

<span data-ttu-id="5dfd9-319">`@namespace` yönergesi sayfanın ad alanını ayarlar.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-319">The `@namespace` directive sets the namespace for the page.</span></span> <span data-ttu-id="5dfd9-320">`@model` yönergesinin ad alanını içermesi gerekmez.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-320">The `@model` directive doesn't need to include the namespace.</span></span>

<span data-ttu-id="5dfd9-321">`@namespace` yönergesi *_Viewwimports. cshtml*içinde yer aldığında, belirtilen ad alanı, `@namespace` yönergesini Içeri aktaran sayfada oluşturulan ad alanı için ön ek sağlar.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-321">When the `@namespace` directive is contained in *_ViewImports.cshtml*, the specified namespace supplies the prefix for the generated namespace in the Page that imports the `@namespace` directive.</span></span> <span data-ttu-id="5dfd9-322">Oluşturulan ad alanının geri kalanı (sonek bölümü), *_Viewwimports. cshtml* dosyasını ve sayfayı içeren klasörü içeren, noktayla ayrılmış göreli yoldur.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-322">The rest of the generated namespace (the suffix portion) is the dot-separated relative path between the folder containing *_ViewImports.cshtml* and the folder containing the page.</span></span>

<span data-ttu-id="5dfd9-323">Örneğin, `PageModel` Class *Pages/Customers/Edit. cshtml. cs* , ad alanını açıkça ayarlar:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-323">For example, the `PageModel` class *Pages/Customers/Edit.cshtml.cs* explicitly sets the namespace:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=snippet_namespace)]

<span data-ttu-id="5dfd9-324">*Pages/_Viewwimports. cshtml* dosyası aşağıdaki ad alanını ayarlar:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-324">The *Pages/_ViewImports.cshtml* file sets the following namespace:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]

<span data-ttu-id="5dfd9-325">*Pages/Customers/Edit. cshtml* Razor sayfasının oluşturulan ad alanı `PageModel` sınıfıyla aynıdır.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-325">The generated namespace for the *Pages/Customers/Edit.cshtml* Razor Page is the same as the `PageModel` class.</span></span>

<span data-ttu-id="5dfd9-326">`@namespace` *Ayrıca geleneksel Razor görünümleriyle birlikte kullanılabilir.*</span><span class="sxs-lookup"><span data-stu-id="5dfd9-326">`@namespace` *also works with conventional Razor views.*</span></span>

<span data-ttu-id="5dfd9-327">*Pages/Create. cshtml* görünüm dosyasını göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-327">Consider the *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create3.cshtml?highlight=2-3)]

<span data-ttu-id="5dfd9-328">Güncelleştirilmiş *Sayfalar/Create. cshtml* görünüm dosyası *_Viewwimports. cshtml* ve önceki düzen dosyası:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-328">The updated *Pages/Create.cshtml* view file with *_ViewImports.cshtml* and the preceding layout file:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create4.cshtml?highlight=2)]

<span data-ttu-id="5dfd9-329">Yukarıdaki kodda, *_Viewwimports. cshtml* ad alanını ve etiket yardımcıları içeri aktardı.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-329">In the preceding code, the *_ViewImports.cshtml* imported the namespace and Tag Helpers.</span></span> <span data-ttu-id="5dfd9-330">Düzen dosyası JavaScript dosyalarını içeri aktardı.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-330">The layout file imported the JavaScript files.</span></span>

<span data-ttu-id="5dfd9-331">[Razor Pages Başlatıcı projesi](#rpvs17) , istemci tarafı doğrulamayı bağlayan *sayfaları/_ValidationScriptsPartial. cshtml*'yi içerir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-331">The [Razor Pages starter project](#rpvs17) contains the *Pages/_ValidationScriptsPartial.cshtml*, which hooks up client-side validation.</span></span>

<span data-ttu-id="5dfd9-332">Kısmi görünümler hakkında daha fazla bilgi için bkz. <xref:mvc/views/partial>.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-332">For more information on partial views, see <xref:mvc/views/partial>.</span></span>

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a><span data-ttu-id="5dfd9-333">Sayfalar için URL oluşturma</span><span class="sxs-lookup"><span data-stu-id="5dfd9-333">URL generation for Pages</span></span>

<span data-ttu-id="5dfd9-334">Daha önce gösterilen `Create` sayfası `RedirectToPage`kullanır:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-334">The `Create` page, shown previously, uses `RedirectToPage`:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml.cs?name=snippet_PageModel&highlight=28)]

<span data-ttu-id="5dfd9-335">Uygulama aşağıdaki dosya/klasör yapısına sahiptir:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-335">The app has the following file/folder structure:</span></span>

* <span data-ttu-id="5dfd9-336">*/Pages*</span><span class="sxs-lookup"><span data-stu-id="5dfd9-336">*/Pages*</span></span>

  * <span data-ttu-id="5dfd9-337">*Index. cshtml*</span><span class="sxs-lookup"><span data-stu-id="5dfd9-337">*Index.cshtml*</span></span>
  * <span data-ttu-id="5dfd9-338">*Gizlilik. cshtml*</span><span class="sxs-lookup"><span data-stu-id="5dfd9-338">*Privacy.cshtml*</span></span>
  * <span data-ttu-id="5dfd9-339">*/Customers*</span><span class="sxs-lookup"><span data-stu-id="5dfd9-339">*/Customers*</span></span>

    * <span data-ttu-id="5dfd9-340">*. Cshtml oluştur*</span><span class="sxs-lookup"><span data-stu-id="5dfd9-340">*Create.cshtml*</span></span>
    * <span data-ttu-id="5dfd9-341">*Edit. cshtml*</span><span class="sxs-lookup"><span data-stu-id="5dfd9-341">*Edit.cshtml*</span></span>
    * <span data-ttu-id="5dfd9-342">*Index. cshtml*</span><span class="sxs-lookup"><span data-stu-id="5dfd9-342">*Index.cshtml*</span></span>

<span data-ttu-id="5dfd9-343">*Pages/Customers/Create. cshtml* ve *Pages/Customers/Edit. cshtml* sayfaları, başarılı olduktan sonra *sayfaları/müşterileri/Index. cshtml* 'ye yeniden yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-343">The *Pages/Customers/Create.cshtml* and *Pages/Customers/Edit.cshtml* pages redirect to *Pages/Customers/Index.cshtml* after success.</span></span> <span data-ttu-id="5dfd9-344">Dize `./Index`, önceki sayfaya erişmek için kullanılan göreli bir sayfa adıdır.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-344">The string `./Index` is a relative page name used to access the preceding page.</span></span> <span data-ttu-id="5dfd9-345">*Pages/Customers/Index. cshtml* sayfasının URL 'leri oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-345">It is used to generate URLs to the *Pages/Customers/Index.cshtml* page.</span></span> <span data-ttu-id="5dfd9-346">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-346">For example:</span></span>

* `Url.Page("./Index", ...)`
* `<a asp-page="./Index">Customers Index Page</a>`
* `RedirectToPage("./Index")`

<span data-ttu-id="5dfd9-347">`/Index` mutlak sayfa adı, *Sayfalar/Index. cshtml* sayfasına URL 'ler oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-347">The absolute page name `/Index` is used to generate URLs to the *Pages/Index.cshtml* page.</span></span> <span data-ttu-id="5dfd9-348">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-348">For example:</span></span>

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">Home Index Page</a>`
* `RedirectToPage("/Index")`

<span data-ttu-id="5dfd9-349">Sayfa adı, kök */Pages* klasöründeki, önde gelen `/` (örneğin, `/Index`) içeren sayfanın yoludur.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-349">The page name is the path to the page from the root */Pages* folder including a leading `/` (for example, `/Index`).</span></span> <span data-ttu-id="5dfd9-350">Önceki URL oluşturma örnekleri, bir URL 'YI sabit kodlamadan gelişmiş seçenekler ve işlevsel yetenekler sunar.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-350">The preceding URL generation samples offer enhanced options and functional capabilities over hard-coding a URL.</span></span> <span data-ttu-id="5dfd9-351">URL oluşturma [yönlendirme](xref:mvc/controllers/routing) kullanır ve yolun hedef yolda nasıl tanımlandığınıza göre parametreleri oluşturabilir ve kodlayabilir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-351">URL generation uses [routing](xref:mvc/controllers/routing) and can generate and encode parameters according to how the route is defined in the destination path.</span></span>

<span data-ttu-id="5dfd9-352">Sayfalar için URL oluşturma göreli adları destekler.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-352">URL generation for pages supports relative names.</span></span> <span data-ttu-id="5dfd9-353">Aşağıdaki tabloda, *sayfalarda/müşteriler/Create. cshtml*'de farklı `RedirectToPage` parametreleri kullanılarak hangi dizin sayfasının seçildiği gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-353">The following table shows which Index page is selected using different `RedirectToPage` parameters in *Pages/Customers/Create.cshtml*.</span></span>

| <span data-ttu-id="5dfd9-354">RedirectToPage (x)</span><span class="sxs-lookup"><span data-stu-id="5dfd9-354">RedirectToPage(x)</span></span>| <span data-ttu-id="5dfd9-355">Sayfasında</span><span class="sxs-lookup"><span data-stu-id="5dfd9-355">Page</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="5dfd9-356">RedirectToPage ("/Index")</span><span class="sxs-lookup"><span data-stu-id="5dfd9-356">RedirectToPage("/Index")</span></span> | <span data-ttu-id="5dfd9-357">*Sayfa/dizin*</span><span class="sxs-lookup"><span data-stu-id="5dfd9-357">*Pages/Index*</span></span> |
| <span data-ttu-id="5dfd9-358">RedirectToPage ("./Index");</span><span class="sxs-lookup"><span data-stu-id="5dfd9-358">RedirectToPage("./Index");</span></span> | <span data-ttu-id="5dfd9-359">*Sayfalar/müşteriler/Dizin*</span><span class="sxs-lookup"><span data-stu-id="5dfd9-359">*Pages/Customers/Index*</span></span> |
| <span data-ttu-id="5dfd9-360">RedirectToPage (".. /İndex ")</span><span class="sxs-lookup"><span data-stu-id="5dfd9-360">RedirectToPage("../Index")</span></span> | <span data-ttu-id="5dfd9-361">*Sayfa/dizin*</span><span class="sxs-lookup"><span data-stu-id="5dfd9-361">*Pages/Index*</span></span> |
| <span data-ttu-id="5dfd9-362">RedirectToPage ("Dizin")</span><span class="sxs-lookup"><span data-stu-id="5dfd9-362">RedirectToPage("Index")</span></span>  | <span data-ttu-id="5dfd9-363">*Sayfalar/müşteriler/Dizin*</span><span class="sxs-lookup"><span data-stu-id="5dfd9-363">*Pages/Customers/Index*</span></span> |

<!-- Test via ~/razor-pages/index/3.0sample/RazorPagesContacts/Pages/Customers/Details.cshtml.cs -->

<span data-ttu-id="5dfd9-364">`RedirectToPage("Index")`, `RedirectToPage("./Index")`ve `RedirectToPage("../Index")` *göreli adlardır*.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-364">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, and `RedirectToPage("../Index")` are *relative names*.</span></span> <span data-ttu-id="5dfd9-365">`RedirectToPage` parametresi, hedef sayfanın adını hesaplamak için geçerli sayfanın yoluyla *birleştirilir* .</span><span class="sxs-lookup"><span data-stu-id="5dfd9-365">The `RedirectToPage` parameter is *combined* with the path of the current page to compute the name of the destination page.</span></span>

<span data-ttu-id="5dfd9-366">Karmaşık bir yapıya sahip siteler oluştururken göreli ad bağlama yararlı olur.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-366">Relative name linking is useful when building sites with a complex structure.</span></span> <span data-ttu-id="5dfd9-367">Bir klasördeki sayfalar arasında bağlantı için göreli adlar kullanıldığında:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-367">When relative names are used to link between pages in a folder:</span></span>

* <span data-ttu-id="5dfd9-368">Bir klasörü yeniden adlandırmak, göreli bağlantıları bozmaz.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-368">Renaming a folder doesn't break the relative links.</span></span>
* <span data-ttu-id="5dfd9-369">Klasör adını içermediği için bağlantılar kopuk değildir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-369">Links are not broken because they don't include the folder name.</span></span>

<span data-ttu-id="5dfd9-370">Farklı bir [alandaki](xref:mvc/controllers/areas)bir sayfaya yeniden yönlendirmek için alanını belirtin:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-370">To redirect to a page in a different [Area](xref:mvc/controllers/areas), specify the area:</span></span>

```csharp
RedirectToPage("/Index", new { area = "Services" });
```

<span data-ttu-id="5dfd9-371">Daha fazla bilgi için bkz. <xref:mvc/controllers/areas> ve <xref:razor-pages/razor-pages-conventions>.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-371">For more information, see <xref:mvc/controllers/areas> and <xref:razor-pages/razor-pages-conventions>.</span></span>

## <a name="viewdata-attribute"></a><span data-ttu-id="5dfd9-372">ViewData özniteliği</span><span class="sxs-lookup"><span data-stu-id="5dfd9-372">ViewData attribute</span></span>

<span data-ttu-id="5dfd9-373">Veriler, <xref:Microsoft.AspNetCore.Mvc.ViewDataAttribute>bir sayfaya geçirilebilir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-373">Data can be passed to a page with <xref:Microsoft.AspNetCore.Mvc.ViewDataAttribute>.</span></span> <span data-ttu-id="5dfd9-374">`[ViewData]` özniteliğiyle birlikte bulunan özellikler, <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.ViewDataDictionary>değerlerinin depolandığı ve yüklendiği değerlerdir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-374">Properties with the `[ViewData]` attribute have their values stored and loaded from the <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.ViewDataDictionary>.</span></span>

<span data-ttu-id="5dfd9-375">Aşağıdaki örnekte `AboutModel`, `Title` özelliğine `[ViewData]` özniteliğini uygular:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-375">In the following example, the `AboutModel` applies the `[ViewData]` attribute to the `Title` property:</span></span>

```csharp
public class AboutModel : PageModel
{
    [ViewData]
    public string Title { get; } = "About";

    public void OnGet()
    {
    }
}
```

<span data-ttu-id="5dfd9-376">Hakkında sayfasında, `Title` özelliğine model özelliği olarak erişin:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-376">In the About page, access the `Title` property as a model property:</span></span>

```cshtml
<h1>@Model.Title</h1>
```

<span data-ttu-id="5dfd9-377">Mizanpajda, başlık ViewData sözlüğünden okundu:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-377">In the layout, the title is read from the ViewData dictionary:</span></span>

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"] - WebApplication</title>
    ...
```

## <a name="tempdata"></a><span data-ttu-id="5dfd9-378">TempData</span><span class="sxs-lookup"><span data-stu-id="5dfd9-378">TempData</span></span>

<span data-ttu-id="5dfd9-379">ASP.NET Core <xref:Microsoft.AspNetCore.Mvc.Controller.TempData>kullanıma sunar.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-379">ASP.NET Core exposes the <xref:Microsoft.AspNetCore.Mvc.Controller.TempData>.</span></span> <span data-ttu-id="5dfd9-380">Bu özellik, okunana kadar verileri depolar.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-380">This property stores data until it's read.</span></span> <span data-ttu-id="5dfd9-381"><xref:Microsoft.AspNetCore.Mvc.ViewFeatures.TempDataDictionary.Keep*> ve <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.TempDataDictionary.Peek*> yöntemleri silinmeden verileri incelemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-381">The <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.TempDataDictionary.Keep*> and <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.TempDataDictionary.Peek*> methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="5dfd9-382">`TempData`, bir tek istekten daha fazla veri gerektiğinde yeniden yönlendirme için kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-382">`TempData` is useful for redirection, when data is needed for more than a single request.</span></span>

<span data-ttu-id="5dfd9-383">Aşağıdaki kod, `TempData`kullanarak `Message` değerini ayarlar:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-383">The following code sets the value of `Message` using `TempData`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,25&name=snippet_Temp)]

<span data-ttu-id="5dfd9-384">*Pages/Customers/Index. cshtml* dosyasında aşağıdaki biçimlendirme `TempData`kullanarak `Message` değerini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-384">The following markup in the *Pages/Customers/Index.cshtml* file displays the value of `Message` using `TempData`.</span></span>

```cshtml
<h3>Msg: @Model.Message</h3>
```

<span data-ttu-id="5dfd9-385">*Pages/Customers/Index. cshtml. cs* sayfa modeli, `[TempData]` özniteliğini `Message` özelliğine uygular.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-385">The *Pages/Customers/Index.cshtml.cs* page model applies the `[TempData]` attribute to the `Message` property.</span></span>

```cs
[TempData]
public string Message { get; set; }
```

<span data-ttu-id="5dfd9-386">Daha fazla bilgi için bkz. [TempData](xref:fundamentals/app-state#tempdata).</span><span class="sxs-lookup"><span data-stu-id="5dfd9-386">For more information, see [TempData](xref:fundamentals/app-state#tempdata).</span></span>

<a name="mhpp"></a>

## <a name="multiple-handlers-per-page"></a><span data-ttu-id="5dfd9-387">Sayfa başına birden çok işleyici</span><span class="sxs-lookup"><span data-stu-id="5dfd9-387">Multiple handlers per page</span></span>

<span data-ttu-id="5dfd9-388">Aşağıdaki sayfa `asp-page-handler` etiketi Yardımcısını kullanarak iki işleyici için biçimlendirme oluşturur:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-388">The following page generates markup for two handlers using the `asp-page-handler` Tag Helper:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

<span data-ttu-id="5dfd9-389">Yukarıdaki örnekteki formda, her biri farklı bir URL 'ye göndermek için `FormActionTagHelper` kullanan iki gönderme düğmesi vardır.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-389">The form in the preceding example has two submit buttons, each using the `FormActionTagHelper` to submit to a different URL.</span></span> <span data-ttu-id="5dfd9-390">`asp-page-handler` özniteliği `asp-page`bir yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-390">The `asp-page-handler` attribute is a companion to `asp-page`.</span></span> <span data-ttu-id="5dfd9-391">`asp-page-handler`, bir sayfa tarafından tanımlanan her bir işleyici yöntemini gönderen URL 'Ler oluşturur.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-391">`asp-page-handler` generates URLs that submit to each of the handler methods defined by a page.</span></span> <span data-ttu-id="5dfd9-392">örnek geçerli sayfaya bağlandığından `asp-page` belirtilmedi.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-392">`asp-page` isn't specified because the sample is linking to the current page.</span></span>

<span data-ttu-id="5dfd9-393">Sayfa modeli:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-393">The page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]

<span data-ttu-id="5dfd9-394">Yukarıdaki kod, *adlandırılmış işleyici yöntemlerini*kullanır.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-394">The preceding code uses *named handler methods*.</span></span> <span data-ttu-id="5dfd9-395">Adlandırılmış işleyici yöntemleri, `On<HTTP Verb>` sonra ve `Async` önce (varsa), ad içindeki metin alınarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-395">Named handler methods are created by taking the text in the name after `On<HTTP Verb>` and before `Async` (if present).</span></span> <span data-ttu-id="5dfd9-396">Yukarıdaki örnekte, Page metotları OnPost**Joinlist**Async ve onpost**Joinlıstuc**Async ' dir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-396">In the preceding example, the page methods are OnPost**JoinList**Async and OnPost**JoinListUC**Async.</span></span> <span data-ttu-id="5dfd9-397">*Onpost* Ile *zaman uyumsuz* olarak kaldırıldığında, işleyici adları `JoinList` ve `JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-397">With *OnPost* and *Async* removed, the handler names are `JoinList` and `JoinListUC`.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?range=12-13)]

<span data-ttu-id="5dfd9-398">Yukarıdaki kodu kullanarak, `OnPostJoinListAsync` ' a gönderen URL yolu `https://localhost:5001/Customers/CreateFATH?handler=JoinList`.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-398">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `https://localhost:5001/Customers/CreateFATH?handler=JoinList`.</span></span> <span data-ttu-id="5dfd9-399">`OnPostJoinListUCAsync` ' a gönderen URL yolu `https://localhost:5001/Customers/CreateFATH?handler=JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-399">The URL path that submits to `OnPostJoinListUCAsync` is `https://localhost:5001/Customers/CreateFATH?handler=JoinListUC`.</span></span>

## <a name="custom-routes"></a><span data-ttu-id="5dfd9-400">Özel yollar</span><span class="sxs-lookup"><span data-stu-id="5dfd9-400">Custom routes</span></span>

<span data-ttu-id="5dfd9-401">`@page` yönergesini kullanarak şunları yapın:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-401">Use the `@page` directive to:</span></span>

* <span data-ttu-id="5dfd9-402">Sayfaya özel bir yol belirtin.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-402">Specify a custom route to a page.</span></span> <span data-ttu-id="5dfd9-403">Örneğin, hakkında sayfasına olan yol `@page "/Some/Other/Path"``/Some/Other/Path` olarak ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-403">For example, the route to the About page can be set to `/Some/Other/Path` with `@page "/Some/Other/Path"`.</span></span>
* <span data-ttu-id="5dfd9-404">Kesimleri bir sayfanın varsayılan yoluna ekleyin.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-404">Append segments to a page's default route.</span></span> <span data-ttu-id="5dfd9-405">Örneğin, bir "öğe" segmenti sayfanın varsayılan yoluna `@page "item"`eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-405">For example, an "item" segment can be added to a page's default route with `@page "item"`.</span></span>
* <span data-ttu-id="5dfd9-406">Bir sayfanın varsayılan yoluna parametreleri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-406">Append parameters to a page's default route.</span></span> <span data-ttu-id="5dfd9-407">Örneğin, `@page "{id}"`bir sayfa için `id`bir ID parametresi gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-407">For example, an ID parameter, `id`, can be required for a page with `@page "{id}"`.</span></span>

<span data-ttu-id="5dfd9-408">Yolun başındaki bir tilde (`~`) tarafından atanan kök göreli bir yol desteklenir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-408">A root-relative path designated by a tilde (`~`) at the beginning of the path is supported.</span></span> <span data-ttu-id="5dfd9-409">Örneğin, `@page "~/Some/Other/Path"` `@page "/Some/Other/Path"`aynıdır.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-409">For example, `@page "~/Some/Other/Path"` is the same as `@page "/Some/Other/Path"`.</span></span>

<span data-ttu-id="5dfd9-410">Yol şablonu `@page "{handler?}"`belirterek `/JoinList` URL 'sindeki `?handler=JoinList` sorgu dizesini bir rota kesimine değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-410">You can change the query string `?handler=JoinList` in the URL to a route segment `/JoinList` by specifying the route template `@page "{handler?}"`.</span></span>

<span data-ttu-id="5dfd9-411">Sorgu dizesini URL 'de `?handler=JoinList` beğenmezseniz, işleyicinin yol bölümüne işleyici adını koymak için yolu değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-411">If you don't like the query string `?handler=JoinList` in the URL, you can change the route to put the handler name in the path portion of the URL.</span></span> <span data-ttu-id="5dfd9-412">Yolu, `@page` yönergesinden sonra çift tırnak içine alınmış bir rota şablonu ekleyerek özelleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-412">You can customize the route by adding a route template enclosed in double quotes after the `@page` directive.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]

<span data-ttu-id="5dfd9-413">Yukarıdaki kodu kullanarak, `OnPostJoinListAsync` ' a gönderen URL yolu `https://localhost:5001/Customers/CreateFATH/JoinList`.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-413">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `https://localhost:5001/Customers/CreateFATH/JoinList`.</span></span> <span data-ttu-id="5dfd9-414">`OnPostJoinListUCAsync` ' a gönderen URL yolu `https://localhost:5001/Customers/CreateFATH/JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-414">The URL path that submits to `OnPostJoinListUCAsync` is `https://localhost:5001/Customers/CreateFATH/JoinListUC`.</span></span>

<span data-ttu-id="5dfd9-415">Aşağıdaki `handler` `?` yol parametresinin isteğe bağlı olduğu anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-415">The `?` following `handler` means the route parameter is optional.</span></span>

## <a name="advanced-configuration-and-settings"></a><span data-ttu-id="5dfd9-416">Gelişmiş yapılandırma ve ayarlar</span><span class="sxs-lookup"><span data-stu-id="5dfd9-416">Advanced configuration and settings</span></span>

<span data-ttu-id="5dfd9-417">Aşağıdaki bölümlerdeki yapılandırma ve ayarlar çoğu uygulama için gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-417">The configuration and settings in following sections is not required by most apps.</span></span>

<span data-ttu-id="5dfd9-418">Gelişmiş seçenekleri yapılandırmak için <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>genişletme yöntemini kullanın:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-418">To configure advanced options, use the extension method <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/StartupRPoptions.cs?name=snippet)]

<span data-ttu-id="5dfd9-419">Sayfaların kök dizinini ayarlamak için <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions> kullanın veya sayfalar için uygulama modeli kuralları ekleyin.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-419">Use the <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions> to set the root directory for pages, or add application model conventions for pages.</span></span> <span data-ttu-id="5dfd9-420">Kurallar hakkında daha fazla bilgi için bkz. [Razor Pages yetkilendirme kuralları](xref:security/authorization/razor-pages-authorization).</span><span class="sxs-lookup"><span data-stu-id="5dfd9-420">For more information on conventions, see [Razor Pages authorization conventions](xref:security/authorization/razor-pages-authorization).</span></span>

<span data-ttu-id="5dfd9-421">Görünümleri önceden derlemek için bkz. [Razor görünüm derlemesi](xref:mvc/views/view-compilation).</span><span class="sxs-lookup"><span data-stu-id="5dfd9-421">To precompile views, see [Razor view compilation](xref:mvc/views/view-compilation).</span></span>

### <a name="specify-that-razor-pages-are-at-the-content-root"></a><span data-ttu-id="5dfd9-422">Razor Pages içerik kökünde olduğunu belirtin</span><span class="sxs-lookup"><span data-stu-id="5dfd9-422">Specify that Razor Pages are at the content root</span></span>

<span data-ttu-id="5dfd9-423">Varsayılan olarak, Razor Pages */Pages* dizininde kök olarak depolanır.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-423">By default, Razor Pages are rooted in the */Pages* directory.</span></span> <span data-ttu-id="5dfd9-424">Razor Pages uygulamanın (<xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootPath>) [içerik kökünde](xref:fundamentals/index#content-root) olduğunu belirtmek için <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.WithRazorPagesAtContentRoot*> ekleyin:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-424">Add <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.WithRazorPagesAtContentRoot*> to specify that your Razor Pages are at the [content root](xref:fundamentals/index#content-root) (<xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootPath>) of the app:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/StartupWithRazorPagesAtContentRoot.cs?name=snippet)]

### <a name="specify-that-razor-pages-are-at-a-custom-root-directory"></a><span data-ttu-id="5dfd9-425">Razor Pages özel kök dizinde olduğunu belirtin</span><span class="sxs-lookup"><span data-stu-id="5dfd9-425">Specify that Razor Pages are at a custom root directory</span></span>

<span data-ttu-id="5dfd9-426">Razor Pages uygulamada bir özel kök dizinde olduğunu belirtmek için <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcCoreBuilderExtensions.WithRazorPagesRoot*> ekleyin (göreli bir yol sağlayın):</span><span class="sxs-lookup"><span data-stu-id="5dfd9-426">Add <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcCoreBuilderExtensions.WithRazorPagesRoot*> to specify that Razor Pages are at a custom root directory in the app (provide a relative path):</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/StartupWithRazorPagesRoot.cs?name=snippet)]

## <a name="additional-resources"></a><span data-ttu-id="5dfd9-427">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="5dfd9-427">Additional resources</span></span>

* <span data-ttu-id="5dfd9-428">Bkz. [Razor Pages kullanmaya başlama](xref:tutorials/razor-pages/razor-pages-start), bu giriş hakkında derleme</span><span class="sxs-lookup"><span data-stu-id="5dfd9-428">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start), which builds on this introduction</span></span>
* [<span data-ttu-id="5dfd9-429">Örnek kodu indirme veya görüntüleme</span><span class="sxs-lookup"><span data-stu-id="5dfd9-429">Download or view sample code</span></span>](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/index/3.0sample)
* <xref:index>
* <xref:mvc/views/razor>
* <xref:mvc/controllers/areas>
* <xref:tutorials/razor-pages/razor-pages-start>
* <xref:security/authorization/razor-pages-authorization>
* <xref:razor-pages/razor-pages-conventions>
* <xref:test/razor-pages-tests>
* <xref:mvc/views/partial>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="5dfd9-430">By [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Ryan şimdi ak](https://github.com/rynowak)</span><span class="sxs-lookup"><span data-stu-id="5dfd9-430">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Ryan Nowak](https://github.com/rynowak)</span></span>

<span data-ttu-id="5dfd9-431">Razor Pages, kod odaklı senaryoları daha kolay ve daha üretken hale getiren ASP.NET Core MVC 'nin yeni bir yönüdür.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-431">Razor Pages is a new aspect of ASP.NET Core MVC that makes coding page-focused scenarios easier and more productive.</span></span>

<span data-ttu-id="5dfd9-432">Model-View-Controller yaklaşımını kullanan bir öğretici arıyorsanız, bkz. [ASP.NET Core MVC ile çalışmaya başlama](xref:tutorials/first-mvc-app/start-mvc).</span><span class="sxs-lookup"><span data-stu-id="5dfd9-432">If you're looking for a tutorial that uses the Model-View-Controller approach, see [Get started with ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span></span>

<span data-ttu-id="5dfd9-433">Bu belge Razor Pages bir giriş sağlar.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-433">This document provides an introduction to Razor Pages.</span></span> <span data-ttu-id="5dfd9-434">Adım adım öğretici değildir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-434">It's not a step by step tutorial.</span></span> <span data-ttu-id="5dfd9-435">Bölümlerden bazılarını çok gelişmiş bir şekilde bulursanız, bkz. [Razor Pages kullanmaya başlama](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="5dfd9-435">If you find some of the sections too advanced, see [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span></span> <span data-ttu-id="5dfd9-436">ASP.NET Core genel bir bakış için bkz. [ASP.NET Core giriş](xref:index).</span><span class="sxs-lookup"><span data-stu-id="5dfd9-436">For an overview of ASP.NET Core, see the [Introduction to ASP.NET Core](xref:index).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5dfd9-437">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="5dfd9-437">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5dfd9-438">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5dfd9-438">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5dfd9-439">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5dfd9-439">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="5dfd9-440">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5dfd9-440">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

<a name="rpvs17"></a>

## <a name="create-a-razor-pages-project"></a><span data-ttu-id="5dfd9-441">Razor Pages projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="5dfd9-441">Create a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5dfd9-442">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5dfd9-442">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="5dfd9-443">Razor Pages projesi oluşturma hakkında ayrıntılı yönergeler için bkz. [Razor Pages kullanmaya başlama](xref:tutorials/razor-pages/razor-pages-start) .</span><span class="sxs-lookup"><span data-stu-id="5dfd9-443">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) for detailed instructions on how to create a Razor Pages project.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="5dfd9-444">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5dfd9-444">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="5dfd9-445">Komut satırından `dotnet new webapp` çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-445">Run `dotnet new webapp` from the command line.</span></span>

<span data-ttu-id="5dfd9-446">Oluşturulan *. csproj* dosyasını Mac için Visual Studio açın.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-446">Open the generated *.csproj* file from Visual Studio for Mac.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5dfd9-447">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5dfd9-447">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="5dfd9-448">Komut satırından `dotnet new webapp` çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-448">Run `dotnet new webapp` from the command line.</span></span>

---

## <a name="razor-pages"></a><span data-ttu-id="5dfd9-449">Razor Pages</span><span class="sxs-lookup"><span data-stu-id="5dfd9-449">Razor Pages</span></span>

<span data-ttu-id="5dfd9-450">Razor Pages, *Startup.cs*'de etkinleştirilmiştir:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-450">Razor Pages is enabled in *Startup.cs*:</span></span>

[!code-cs[](index/sample/RazorPagesIntro/Startup.cs?name=snippet_Startup)]

<span data-ttu-id="5dfd9-451">Temel bir sayfa düşünün:<a name="OnGet"></a></span><span class="sxs-lookup"><span data-stu-id="5dfd9-451">Consider a basic page: <a name="OnGet"></a></span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index.cshtml)]

<span data-ttu-id="5dfd9-452">Yukarıdaki kod, denetleyiciler ve görünümlerle ASP.NET Core bir uygulamada kullanılan [Razor görünüm dosyası](xref:tutorials/first-mvc-app/adding-view) gibi bir çok şey arar.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-452">The preceding code looks a lot like a [Razor view file](xref:tutorials/first-mvc-app/adding-view) used in an ASP.NET Core app with controllers and views.</span></span> <span data-ttu-id="5dfd9-453">Bu, farklı kılan `@page` yönergedir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-453">What makes it different is the `@page` directive.</span></span> <span data-ttu-id="5dfd9-454">`@page`, dosyayı bir denetleyiciye geçmeden doğrudan istekleri işlediği anlamına gelen bir MVC eylemine sahip olur.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-454">`@page` makes the file into an MVC action - which means that it handles requests directly, without going through a controller.</span></span> <span data-ttu-id="5dfd9-455">`@page` bir sayfada ilk Razor yönergesi olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-455">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="5dfd9-456">`@page` diğer Razor yapıları davranışını etkiler.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-456">`@page` affects the behavior of other Razor constructs.</span></span>

<span data-ttu-id="5dfd9-457">`PageModel` sınıfı kullanan benzer bir sayfa aşağıdaki iki dosyada gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-457">A similar page, using a `PageModel` class, is shown in the following two files.</span></span> <span data-ttu-id="5dfd9-458">*Pages/Index2. cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-458">The *Pages/Index2.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index2.cshtml)]

<span data-ttu-id="5dfd9-459">*Pages/Index2. cshtml. cs* sayfa modeli:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-459">The *Pages/Index2.cshtml.cs* page model:</span></span>

[!code-cs[](index/sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]

<span data-ttu-id="5dfd9-460">Kurala göre `PageModel` sınıf dosyası, *. cs* eklenmiş Razor sayfası dosyasıyla aynı ada sahiptir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-460">By convention, the `PageModel` class file has the same name as the Razor Page file with *.cs* appended.</span></span> <span data-ttu-id="5dfd9-461">Örneğin, önceki Razor sayfası *Pages/Index2. cshtml*' dir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-461">For example, the previous Razor Page is *Pages/Index2.cshtml*.</span></span> <span data-ttu-id="5dfd9-462">`PageModel` sınıfını içeren dosya *sayfa/Index2. cshtml. cs*olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-462">The file containing the `PageModel` class is named *Pages/Index2.cshtml.cs*.</span></span>

<span data-ttu-id="5dfd9-463">URL yollarının sayfalara olan ilişkilendirmeleri, sayfanın dosya sistemindeki konumuna göre belirlenir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-463">The associations of URL paths to pages are determined by the page's location in the file system.</span></span> <span data-ttu-id="5dfd9-464">Aşağıdaki tabloda bir Razor sayfa yolu ve eşleşen URL gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-464">The following table shows a Razor Page path and the matching URL:</span></span>

| <span data-ttu-id="5dfd9-465">Dosya adı ve yolu</span><span class="sxs-lookup"><span data-stu-id="5dfd9-465">File name and path</span></span>               | <span data-ttu-id="5dfd9-466">eşleşen URL</span><span class="sxs-lookup"><span data-stu-id="5dfd9-466">matching URL</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="5dfd9-467">*/Pages/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="5dfd9-467">*/Pages/Index.cshtml*</span></span> | <span data-ttu-id="5dfd9-468">`/` veya `/Index`</span><span class="sxs-lookup"><span data-stu-id="5dfd9-468">`/` or `/Index`</span></span> |
| <span data-ttu-id="5dfd9-469">*/Pages/Contact.exe*</span><span class="sxs-lookup"><span data-stu-id="5dfd9-469">*/Pages/Contact.cshtml*</span></span> | `/Contact` |
| <span data-ttu-id="5dfd9-470">*/Pages/Store/Contact.exe*</span><span class="sxs-lookup"><span data-stu-id="5dfd9-470">*/Pages/Store/Contact.cshtml*</span></span> | `/Store/Contact` |
| <span data-ttu-id="5dfd9-471">*/Pages/Store/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="5dfd9-471">*/Pages/Store/Index.cshtml*</span></span> | <span data-ttu-id="5dfd9-472">`/Store` veya `/Store/Index`</span><span class="sxs-lookup"><span data-stu-id="5dfd9-472">`/Store` or `/Store/Index`</span></span> |

<span data-ttu-id="5dfd9-473">Notlar:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-473">Notes:</span></span>

* <span data-ttu-id="5dfd9-474">Çalışma zamanı, *Sayfalar* klasöründeki Razor Pages dosyaları varsayılan olarak arar.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-474">The runtime looks for Razor Pages files in the *Pages* folder by default.</span></span>
* <span data-ttu-id="5dfd9-475">`Index`, URL bir sayfa içermiyorsa varsayılan sayfasıdır.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-475">`Index` is the default page when a URL doesn't include a page.</span></span>

## <a name="write-a-basic-form"></a><span data-ttu-id="5dfd9-476">Temel form yazma</span><span class="sxs-lookup"><span data-stu-id="5dfd9-476">Write a basic form</span></span>

<span data-ttu-id="5dfd9-477">Razor Pages, Web tarayıcıları ile kullanılan ortak desenleri bir uygulama oluştururken kolayca uygulanması için tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-477">Razor Pages is designed to make common patterns used with web browsers easy to implement when building an app.</span></span> <span data-ttu-id="5dfd9-478">[Model bağlama](xref:mvc/models/model-binding), [ETIKET yardımcıları](xref:mvc/views/tag-helpers/intro)ve HTML Yardımcıları hepsi, Razor sayfası sınıfında tanımlanan özelliklerle *çalışır* .</span><span class="sxs-lookup"><span data-stu-id="5dfd9-478">[Model binding](xref:mvc/models/model-binding), [Tag Helpers](xref:mvc/views/tag-helpers/intro), and HTML helpers all *just work* with the properties defined in a Razor Page class.</span></span> <span data-ttu-id="5dfd9-479">`Contact` modeli için temel bir "bize başvurun" formu uygulayan bir sayfa düşünün:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-479">Consider a page that implements a basic "contact us" form for the `Contact` model:</span></span>

<span data-ttu-id="5dfd9-480">Bu belgedeki örnekler için `DbContext`, [Startup.cs](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) dosyasında başlatılır.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-480">For the samples in this document, the `DbContext` is initialized in the [Startup.cs](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) file.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Startup.cs?highlight=15-16)]

<span data-ttu-id="5dfd9-481">Veri modeli:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-481">The data model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Data/Customer.cs)]

<span data-ttu-id="5dfd9-482">DB bağlamı:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-482">The db context:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Data/AppDbContext.cs)]

<span data-ttu-id="5dfd9-483">*Pages/Create. cshtml* görünüm dosyası:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-483">The *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml)]

<span data-ttu-id="5dfd9-484">*Pages/Create. cshtml. cs* sayfa modeli:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-484">The *Pages/Create.cshtml.cs* page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_ALL)]

<span data-ttu-id="5dfd9-485">Kurala göre `PageModel` sınıfı `<PageName>Model` olarak adlandırılır ve sayfayla aynı ad alanında yer alan.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-485">By convention, the `PageModel` class is called `<PageName>Model` and is in the same namespace as the page.</span></span>

<span data-ttu-id="5dfd9-486">`PageModel` sınıfı, bir sayfa mantığının sunumundaki ayırmayı sağlar.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-486">The `PageModel` class allows separation of the logic of a page from its presentation.</span></span> <span data-ttu-id="5dfd9-487">Sayfaya gönderilen istekler için sayfa işleyicilerini ve sayfayı işlemek için kullanılan verileri tanımlar.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-487">It defines page handlers for requests sent to the page and the data used to render the page.</span></span> <span data-ttu-id="5dfd9-488">Bu ayrım şunları sağlar:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-488">This separation allows:</span></span>

* <span data-ttu-id="5dfd9-489">[Bağımlılık ekleme](xref:fundamentals/dependency-injection)yoluyla sayfa bağımlılıklarını yönetme.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-489">Managing of page dependencies through [dependency injection](xref:fundamentals/dependency-injection).</span></span>
* <span data-ttu-id="5dfd9-490">Sayfaların [birim testi](xref:test/razor-pages-tests) .</span><span class="sxs-lookup"><span data-stu-id="5dfd9-490">[Unit testing](xref:test/razor-pages-tests) the pages.</span></span>

<span data-ttu-id="5dfd9-491">Sayfada, `POST` isteklerinde çalışan bir `OnPostAsync` *işleyicisi yöntemi*vardır (bir Kullanıcı formu gönderdiğinde).</span><span class="sxs-lookup"><span data-stu-id="5dfd9-491">The page has an `OnPostAsync` *handler method*, which runs on `POST` requests (when a user posts the form).</span></span> <span data-ttu-id="5dfd9-492">Herhangi bir HTTP fiili için işleyici yöntemleri ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-492">You can add handler methods for any HTTP verb.</span></span> <span data-ttu-id="5dfd9-493">En yaygın işleyiciler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-493">The most common handlers are:</span></span>

* <span data-ttu-id="5dfd9-494">Sayfanın başlatılması için gereken durum `OnGet`.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-494">`OnGet` to initialize state needed for the page.</span></span> <span data-ttu-id="5dfd9-495">[OnGet](#OnGet) örneği.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-495">[OnGet](#OnGet) sample.</span></span>
* <span data-ttu-id="5dfd9-496">form gönderilerini işlemek için `OnPost`.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-496">`OnPost` to handle form submissions.</span></span>

<span data-ttu-id="5dfd9-497">`Async` adlandırma son eki isteğe bağlıdır, ancak genellikle zaman uyumsuz işlevler için kural tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-497">The `Async` naming suffix is optional but is often used by convention for asynchronous functions.</span></span> <span data-ttu-id="5dfd9-498">Yukarıdaki kod Razor Pages için tipik bir davranıştır.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-498">The preceding code is typical for Razor Pages.</span></span>

<span data-ttu-id="5dfd9-499">Denetleyicileri ve görünümleri kullanarak ASP.NET uygulamaları hakkında bilginiz varsa:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-499">If you're familiar with ASP.NET apps using controllers and views:</span></span>

* <span data-ttu-id="5dfd9-500">Yukarıdaki örnekteki `OnPostAsync` kodu, tipik denetleyici koduna benzer şekilde görünür.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-500">The `OnPostAsync` code in the preceding example looks similar to typical controller code.</span></span>
* <span data-ttu-id="5dfd9-501">[Model bağlama](xref:mvc/models/model-binding), [doğrulama](xref:mvc/models/validation), [doğrulama](xref:mvc/models/validation)ve eylem sonuçları gibi mvc temel elemanlarının çoğu paylaşılır.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-501">Most of the MVC primitives like [model binding](xref:mvc/models/model-binding), [validation](xref:mvc/models/validation), [Validation](xref:mvc/models/validation),  and action results are shared.</span></span>

<span data-ttu-id="5dfd9-502">Önceki `OnPostAsync` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-502">The previous `OnPostAsync` method:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="5dfd9-503">`OnPostAsync`temel akışı:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-503">The basic flow of `OnPostAsync`:</span></span>

<span data-ttu-id="5dfd9-504">Doğrulama hatalarını kontrol edin.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-504">Check for validation errors.</span></span>

* <span data-ttu-id="5dfd9-505">Hata yoksa, verileri kaydedin ve yeniden yönlendirin.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-505">If there are no errors, save the data and redirect.</span></span>
* <span data-ttu-id="5dfd9-506">Hatalar varsa, doğrulama iletileriyle sayfayı yeniden görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-506">If there are errors, show the page again with validation messages.</span></span> <span data-ttu-id="5dfd9-507">İstemci tarafı doğrulaması geleneksel ASP.NET Core MVC uygulamalarıyla aynıdır.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-507">Client-side validation is identical to traditional ASP.NET Core MVC applications.</span></span> <span data-ttu-id="5dfd9-508">Çoğu durumda, istemci üzerinde doğrulama hataları algılanır ve sunucuya hiçbir zaman gönderilmez.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-508">In many cases, validation errors would be detected on the client, and never submitted to the server.</span></span>

<span data-ttu-id="5dfd9-509">Veriler başarıyla girildiğinde, `OnPostAsync` Handler yöntemi bir `RedirectToPageResult`örneğini döndürmek için `RedirectToPage` yardımcı yöntemini çağırır.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-509">When the data is entered successfully, the `OnPostAsync` handler method calls the `RedirectToPage` helper method to return an instance of `RedirectToPageResult`.</span></span> <span data-ttu-id="5dfd9-510">`RedirectToPage`, `RedirectToAction` veya `RedirectToRoute`benzer ancak sayfalar için özelleştirilen yeni bir eylem sonucudur.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-510">`RedirectToPage` is a new action result, similar to `RedirectToAction` or `RedirectToRoute`, but customized for pages.</span></span> <span data-ttu-id="5dfd9-511">Önceki örnekte, kök dizin sayfasına (`/Index`) yeniden yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-511">In the preceding sample, it redirects to the root Index page (`/Index`).</span></span> <span data-ttu-id="5dfd9-512">`RedirectToPage`, [Sayfalar Için URL oluşturma](#url_gen) bölümünde ayrıntılı olarak açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-512">`RedirectToPage` is detailed in the [URL generation for Pages](#url_gen) section.</span></span>

<span data-ttu-id="5dfd9-513">Gönderilen formda doğrulama hataları olduğunda (sunucuya geçirilen)`OnPostAsync` işleyicisi yöntemi `Page` yardımcı yöntemini çağırır.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-513">When the submitted form has validation errors (that are passed to the server), the`OnPostAsync` handler method calls the `Page` helper method.</span></span> <span data-ttu-id="5dfd9-514">`Page`, `PageResult`örneğini döndürür.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-514">`Page` returns an instance of `PageResult`.</span></span> <span data-ttu-id="5dfd9-515">`Page` döndürmek, denetleyicilerde eylemlerin `View`nasıl dönüşlerine benzer.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-515">Returning `Page` is similar to how actions in controllers return `View`.</span></span> <span data-ttu-id="5dfd9-516">`PageResult`, bir işleyici yöntemi için varsayılan dönüş türüdür.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-516">`PageResult` is the default return type for a handler method.</span></span> <span data-ttu-id="5dfd9-517">`void` döndüren bir işleyici yöntemi sayfayı işler.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-517">A handler method that returns `void` renders the page.</span></span>

<span data-ttu-id="5dfd9-518">`Customer` özelliği, model bağlamasını kabul etmek için `[BindProperty]` özniteliğini kullanır.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-518">The `Customer` property uses `[BindProperty]` attribute to opt in to model binding.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_PageModel&highlight=10-11)]

<span data-ttu-id="5dfd9-519">Razor Pages, varsayılan olarak, özellikleri yalnızca`GET` olmayan fiiller ile bağlayın.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-519">Razor Pages, by default, bind properties only with non-`GET` verbs.</span></span> <span data-ttu-id="5dfd9-520">Özelliklere bağlama, yazmanız gerektiğini kodun miktarını azaltabilir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-520">Binding to properties can reduce the amount of code you have to write.</span></span> <span data-ttu-id="5dfd9-521">Bağlama, form alanlarını işlemek için aynı özelliği kullanarak kodu azaltır (`<input asp-for="Customer.Name">`) ve girişi kabul eder.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-521">Binding reduces code by using the same property to render form fields (`<input asp-for="Customer.Name">`) and accept the input.</span></span>

[!INCLUDE[](~/includes/bind-get.md)]

<span data-ttu-id="5dfd9-522">Giriş sayfası (*Index. cshtml*):</span><span class="sxs-lookup"><span data-stu-id="5dfd9-522">The home page (*Index.cshtml*):</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml)]

<span data-ttu-id="5dfd9-523">İlişkili `PageModel` sınıfı (*Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="5dfd9-523">The associated `PageModel` class (*Index.cshtml.cs*):</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs)]

<span data-ttu-id="5dfd9-524">*Index. cshtml* dosyası her kişi için bir düzenleme bağlantısı oluşturmak üzere aşağıdaki biçimlendirmeyi içerir:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-524">The *Index.cshtml* file contains the following markup to create an edit link for each contact:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=21)]

<span data-ttu-id="5dfd9-525">`<a asp-page="./Edit" asp-route-id="@contact.Id">Edit</a>` [tutturucu etiketi Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) , düzenleme sayfasına bir bağlantı oluşturmak için `asp-route-{value}` özniteliğini kullandı.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-525">The `<a asp-page="./Edit" asp-route-id="@contact.Id">Edit</a>` [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) used the `asp-route-{value}` attribute to generate a link to the Edit page.</span></span> <span data-ttu-id="5dfd9-526">Bağlantı, iletişim KIMLIĞINE sahip rota verileri içerir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-526">The link contains route data with the contact ID.</span></span> <span data-ttu-id="5dfd9-527">Örneğin, `https://localhost:5001/Edit/1`.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-527">For example, `https://localhost:5001/Edit/1`.</span></span> <span data-ttu-id="5dfd9-528">[Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro), Razor dosyalarında HTML öğelerinin oluşturulmasına ve işlenmesine sunucu tarafı kodun katılmasını etkinleştir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-528">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span> <span data-ttu-id="5dfd9-529">Etiket Yardımcıları `@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` tarafından etkinleştirilir</span><span class="sxs-lookup"><span data-stu-id="5dfd9-529">Tag Helpers are enabled by `@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers`</span></span>

<span data-ttu-id="5dfd9-530">*Pages/Edit. cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-530">The *Pages/Edit.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Edit.cshtml?highlight=1)]

<span data-ttu-id="5dfd9-531">İlk satır `@page "{id:int}"` yönergesini içerir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-531">The first line contains the `@page "{id:int}"` directive.</span></span> <span data-ttu-id="5dfd9-532">Yönlendirme kısıtlaması`"{id:int}"`, sayfaya istekleri `int` yönlendirme verileri içeren sayfaya kabul etmesini söyler.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-532">The routing constraint`"{id:int}"` tells the page to accept requests to the page that contain `int` route data.</span></span> <span data-ttu-id="5dfd9-533">Sayfaya yapılan bir istek bir `int`dönüştürülebildiği rota verileri içermiyorsa, çalışma zamanı bir HTTP 404 (bulunamadı) hatası döndürür.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-533">If a request to the page doesn't contain route data that can be converted to an `int`, the runtime returns an HTTP 404 (not found) error.</span></span> <span data-ttu-id="5dfd9-534">KIMLIĞI isteğe bağlı yapmak için, yol kısıtlamasına `?` ekleyin:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-534">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="5dfd9-535">*Pages/Edit. cshtml. cs* dosyası:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-535">The *Pages/Edit.cshtml.cs* file:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Edit.cshtml.cs)]

<span data-ttu-id="5dfd9-536">*Index. cshtml* dosyası, her müşteri kişisi için bir silme düğmesi oluşturmak için de biçimlendirme içerir:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-536">The *Index.cshtml* file also contains markup to create a delete button for each customer contact:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=22-23)]

<span data-ttu-id="5dfd9-537">Sil düğmesi HTML 'de işlendiğinde, `formaction` şunlar için parametreler içerir:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-537">When the delete button is rendered in HTML, its `formaction` includes parameters for:</span></span>

* <span data-ttu-id="5dfd9-538">`asp-route-id` özniteliği tarafından belirtilen müşteri iletişim KIMLIĞI.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-538">The customer contact ID specified by the `asp-route-id` attribute.</span></span>
* <span data-ttu-id="5dfd9-539">`asp-page-handler` özniteliği tarafından belirtilen `handler`.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-539">The `handler` specified by the `asp-page-handler` attribute.</span></span>

<span data-ttu-id="5dfd9-540">Müşteri irtibat KIMLIĞI `1`olan işlenmiş silme düğmesine bir örnek aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-540">Here is an example of a rendered delete button with a customer contact ID of `1`:</span></span>

```html
<button type="submit" formaction="/?id=1&amp;handler=delete">delete</button>
```

<span data-ttu-id="5dfd9-541">Düğme seçildiğinde, sunucuya bir form `POST` isteği gönderilir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-541">When the button is selected, a form `POST` request is sent to the server.</span></span> <span data-ttu-id="5dfd9-542">Kurala göre, işleyici yönteminin adı, düzen `OnPost[handler]Async`göre `handler` parametresinin değerine göre seçilir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-542">By convention, the name of the handler method is selected based on the value of the `handler` parameter according to the scheme `OnPost[handler]Async`.</span></span>

<span data-ttu-id="5dfd9-543">Bu örnekte `handler` `delete` olduğundan, `OnPostDeleteAsync` Handler yöntemi `POST` isteğini işlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-543">Because the `handler` is `delete` in this example, the `OnPostDeleteAsync` handler method is used to process the `POST` request.</span></span> <span data-ttu-id="5dfd9-544">`asp-page-handler`, `remove`gibi farklı bir değere ayarlandıysa `OnPostRemoveAsync` ada sahip bir işleyici yöntemi seçilidir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-544">If the `asp-page-handler` is set to a different value, such as `remove`, a handler method with the name `OnPostRemoveAsync` is selected.</span></span> <span data-ttu-id="5dfd9-545">Aşağıdaki kod `OnPostDeleteAsync` işleyicisini gösterir:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-545">The following code shows the `OnPostDeleteAsync` handler:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs?range=26-37)]

<span data-ttu-id="5dfd9-546">`OnPostDeleteAsync` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-546">The `OnPostDeleteAsync` method:</span></span>

* <span data-ttu-id="5dfd9-547">Sorgu dizesinden `id` kabul eder.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-547">Accepts the `id` from the query string.</span></span> <span data-ttu-id="5dfd9-548">*Index. cshtml* sayfa yönergesi yönlendirme kısıtlaması `"{id:int?}"`içeriyorsa, `id` rota verilerinden gelir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-548">If the *Index.cshtml* page directive contained routing constraint `"{id:int?}"`, `id` would come from route data.</span></span> <span data-ttu-id="5dfd9-549">`id` için rota verileri, URI 'de `https://localhost:5001/Customers/2`gibi belirtilir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-549">The route data for `id` is specified in the URI such as `https://localhost:5001/Customers/2`.</span></span>
* <span data-ttu-id="5dfd9-550">`FindAsync`ile müşteri iletişim için veritabanını sorgular.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-550">Queries the database for the customer contact with `FindAsync`.</span></span>
* <span data-ttu-id="5dfd9-551">Müşteri ilgili kişisi bulunursa, bunlar müşteri kişileri listesinden kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-551">If the customer contact is found, they're removed from the list of customer contacts.</span></span> <span data-ttu-id="5dfd9-552">Veritabanı güncelleştirildi.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-552">The database is updated.</span></span>
* <span data-ttu-id="5dfd9-553">Kök dizin sayfasına yeniden yönlendirmek için `RedirectToPage` çağırır (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="5dfd9-553">Calls `RedirectToPage` to redirect to the root Index page (`/Index`).</span></span>

## <a name="mark-page-properties-as-required"></a><span data-ttu-id="5dfd9-554">Sayfa özelliklerini gerektiği gibi işaretle</span><span class="sxs-lookup"><span data-stu-id="5dfd9-554">Mark page properties as required</span></span>

<span data-ttu-id="5dfd9-555">`PageModel` Özellikler [gerekli](/dotnet/api/system.componentmodel.dataannotations.requiredattribute) öznitelikle birlikte kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-555">Properties on a `PageModel` can be decorated with the [Required](/dotnet/api/system.componentmodel.dataannotations.requiredattribute) attribute:</span></span>

[!code-cs[](index/sample/Create.cshtml.cs?highlight=3,15-16)]

<span data-ttu-id="5dfd9-556">Daha fazla bilgi için bkz. [model doğrulaması](xref:mvc/models/validation).</span><span class="sxs-lookup"><span data-stu-id="5dfd9-556">For more information, see [Model validation](xref:mvc/models/validation).</span></span>

## <a name="handle-head-requests-with-an-onget-handler-fallback"></a><span data-ttu-id="5dfd9-557">OnGet işleyicisi geri dönüşü ile tanıtıcı HEAD istekleri</span><span class="sxs-lookup"><span data-stu-id="5dfd9-557">Handle HEAD requests with an OnGet handler fallback</span></span>

<span data-ttu-id="5dfd9-558">`HEAD` istekleri belirli bir kaynak için üst bilgileri almanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-558">`HEAD` requests allow you to retrieve the headers for a specific resource.</span></span> <span data-ttu-id="5dfd9-559">`GET` isteklerinin aksine `HEAD` istekleri bir yanıt gövdesi döndürmez.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-559">Unlike `GET` requests, `HEAD` requests don't return a response body.</span></span>

<span data-ttu-id="5dfd9-560">Normalde, `HEAD` istekleri için `OnHead` işleyicisi oluşturulur ve çağırılır:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-560">Ordinarily, an `OnHead` handler is created and called for `HEAD` requests:</span></span> 

```csharp
public void OnHead()
{
    HttpContext.Response.Headers.Add("HandledBy", "Handled by OnHead!");
}
```

<span data-ttu-id="5dfd9-561">ASP.NET Core 2,1 veya sonraki sürümlerde Razor Pages, `OnHead` işleyici tanımlanmadığında `OnGet` işleyicisini çağırmaya geri döner.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-561">In ASP.NET Core 2.1 or later, Razor Pages falls back to calling the `OnGet` handler if no `OnHead` handler is defined.</span></span> <span data-ttu-id="5dfd9-562">Bu davranış, `Startup.ConfigureServices`[Setcompatibilityversion](xref:mvc/compatibility-version) çağrısıyla etkinleştirilir:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-562">This behavior is enabled by the call to [SetCompatibilityVersion](xref:mvc/compatibility-version) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddMvc()
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_1);
```

<span data-ttu-id="5dfd9-563">Varsayılan Şablonlar, ASP.NET Core 2,1 ve 2,2 ' de `SetCompatibilityVersion` çağrısını üretir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-563">The default templates generate the `SetCompatibilityVersion` call in ASP.NET Core 2.1 and 2.2.</span></span> <span data-ttu-id="5dfd9-564">`SetCompatibilityVersion` `AllowMappingHeadRequestsToGetHandler` Razor Pages seçeneğini `true`için etkin şekilde ayarlar.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-564">`SetCompatibilityVersion` effectively sets the Razor Pages option `AllowMappingHeadRequestsToGetHandler` to `true`.</span></span>

<span data-ttu-id="5dfd9-565">`SetCompatibilityVersion`tüm davranışlardan çıkmak yerine, açıkça *belirli* davranışları kabul edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-565">Rather than opting in to all behaviors with `SetCompatibilityVersion`, you can explicitly opt in to *specific* behaviors.</span></span> <span data-ttu-id="5dfd9-566">Aşağıdaki kod, `HEAD` isteklerinin `OnGet` işleyicisine eşlenmesine izin vermek için ' de kullanılır:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-566">The following code opts in to allowing `HEAD` requests to be mapped to the `OnGet` handler:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        options.AllowMappingHeadRequestsToGetHandler = true;
    });
```

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a><span data-ttu-id="5dfd9-567">XSRF/CSRF ve Razor Pages</span><span class="sxs-lookup"><span data-stu-id="5dfd9-567">XSRF/CSRF and Razor Pages</span></span>

<span data-ttu-id="5dfd9-568">[Antiforgery doğrulaması](xref:security/anti-request-forgery)için herhangi bir kod yazmanız gerekmez.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-568">You don't have to write any code for [antiforgery validation](xref:security/anti-request-forgery).</span></span> <span data-ttu-id="5dfd9-569">Antiforgery belirteci oluşturma ve doğrulama, Razor Pages otomatik olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-569">Antiforgery token generation and validation are automatically included in Razor Pages.</span></span>

<a name="layout"></a>

## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a><span data-ttu-id="5dfd9-570">Razor Pages ile düzenleri, partileri, şablonları ve etiket yardımcılarını kullanma</span><span class="sxs-lookup"><span data-stu-id="5dfd9-570">Using Layouts, partials, templates, and Tag Helpers with Razor Pages</span></span>

<span data-ttu-id="5dfd9-571">Sayfalar, Razor görünüm altyapısının tüm özellikleri ile çalışır.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-571">Pages work with all the capabilities of the Razor view engine.</span></span> <span data-ttu-id="5dfd9-572">Düzenler, partıals, şablonlar, etiket yardımcıları, *_Viewstart. cshtml*, *_viewwimports. cshtml* geleneksel Razor görünümlerinde oldukları gibi çalışır.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-572">Layouts, partials, templates, Tag Helpers, *_ViewStart.cshtml*, *_ViewImports.cshtml* work in the same way they do for conventional Razor views.</span></span>

<span data-ttu-id="5dfd9-573">Bu özelliklerden bazılarının avantajlarından yararlanarak bu sayfayı declutter edelim.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-573">Let's declutter this page by taking advantage of some of those capabilities.</span></span>

<span data-ttu-id="5dfd9-574">*Pages/Shared/_Layout. cshtml*öğesine bir [Düzen sayfası](xref:mvc/views/layout) ekleyin:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-574">Add a [layout page](xref:mvc/views/layout) to *Pages/Shared/_Layout.cshtml*:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_LayoutSimple.cshtml)]

<span data-ttu-id="5dfd9-575">[Düzen](xref:mvc/views/layout):</span><span class="sxs-lookup"><span data-stu-id="5dfd9-575">The [Layout](xref:mvc/views/layout):</span></span>

* <span data-ttu-id="5dfd9-576">Her sayfanın yerleşimini denetler (sayfa düzen dışında değilse).</span><span class="sxs-lookup"><span data-stu-id="5dfd9-576">Controls the layout of each page (unless the page opts out of layout).</span></span>
* <span data-ttu-id="5dfd9-577">JavaScript ve stil sayfaları gibi HTML yapılarını içeri aktarır.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-577">Imports HTML structures such as JavaScript and stylesheets.</span></span>

<span data-ttu-id="5dfd9-578">Daha fazla bilgi için bkz. [Düzen sayfası](xref:mvc/views/layout) .</span><span class="sxs-lookup"><span data-stu-id="5dfd9-578">See [layout page](xref:mvc/views/layout) for more information.</span></span>

<span data-ttu-id="5dfd9-579">[Layout](xref:mvc/views/layout#specifying-a-layout) özelliği *Pages/_viewstart. cshtml*içinde ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-579">The [Layout](xref:mvc/views/layout#specifying-a-layout) property is set in *Pages/_ViewStart.cshtml*:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]

<span data-ttu-id="5dfd9-580">Düzen *Sayfalar/paylaşılan* klasöründedir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-580">The layout is in the *Pages/Shared* folder.</span></span> <span data-ttu-id="5dfd9-581">Sayfalar, geçerli sayfayla aynı klasörden başlayarak diğer görünümleri (düzenler, şablonlar, parals) hiyerarşik olarak arar.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-581">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="5dfd9-582">*Sayfalar/paylaşılan* klasördeki bir düzen, *Sayfalar* klasörü altındaki herhangi bir Razor sayfasından kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-582">A layout in the *Pages/Shared* folder can be used from any Razor page under the *Pages* folder.</span></span>

<span data-ttu-id="5dfd9-583">Düzen dosyası *Sayfalar/paylaşılan* klasörüne gitmelidir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-583">The layout file should go in the *Pages/Shared* folder.</span></span>

<span data-ttu-id="5dfd9-584">Düzen dosyasını *Görünümler/paylaşılan* klasöre **yerleştirmenizi öneririz** .</span><span class="sxs-lookup"><span data-stu-id="5dfd9-584">We recommend you **not** put the layout file in the *Views/Shared* folder.</span></span> <span data-ttu-id="5dfd9-585">*Görünümler/paylaşılan* bir MVC görünümleri modelidir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-585">*Views/Shared* is an MVC views pattern.</span></span> <span data-ttu-id="5dfd9-586">Razor Pages, yol kurallarını değil klasör hiyerarşisine güvenmektir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-586">Razor Pages are meant to rely on folder hierarchy, not path conventions.</span></span>

<span data-ttu-id="5dfd9-587">Bir Razor sayfasından arama görüntüleme, *Sayfalar* klasörünü içerir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-587">View search from a Razor Page includes the *Pages* folder.</span></span> <span data-ttu-id="5dfd9-588">MVC denetleyicileri ve geleneksel Razor görünümleriyle kullandığınız düzenler, şablonlar ve parals işlemleri *yalnızca çalışır*.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-588">The layouts, templates, and partials you're using with MVC controllers and conventional Razor views *just work*.</span></span>

<span data-ttu-id="5dfd9-589">*Pages/_Viewwimports. cshtml* dosyası ekleyin:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-589">Add a *Pages/_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]

<span data-ttu-id="5dfd9-590">`@namespace` öğreticide daha sonra açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-590">`@namespace` is explained later in the tutorial.</span></span> <span data-ttu-id="5dfd9-591">`@addTagHelper` yönergesi, [yerleşik etiket yardımcılarını](xref:mvc/views/tag-helpers/builtin-th/Index) *Sayfalar* klasöründeki tüm sayfalara getirir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-591">The `@addTagHelper` directive brings in the [built-in Tag Helpers](xref:mvc/views/tag-helpers/builtin-th/Index) to all the pages in the *Pages* folder.</span></span>

<a name="namespace"></a>

<span data-ttu-id="5dfd9-592">`@namespace` yönergesi açıkça bir sayfada kullanıldığında:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-592">When the `@namespace` directive is used explicitly on a page:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]

<span data-ttu-id="5dfd9-593">Yönergesi sayfanın ad alanını ayarlar.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-593">The directive sets the namespace for the page.</span></span> <span data-ttu-id="5dfd9-594">`@model` yönergesinin ad alanını içermesi gerekmez.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-594">The `@model` directive doesn't need to include the namespace.</span></span>

<span data-ttu-id="5dfd9-595">`@namespace` yönergesi *_Viewwimports. cshtml*içinde yer aldığında, belirtilen ad alanı, `@namespace` yönergesini Içeri aktaran sayfada oluşturulan ad alanı için ön ek sağlar.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-595">When the `@namespace` directive is contained in *_ViewImports.cshtml*, the specified namespace supplies the prefix for the generated namespace in the Page that imports the `@namespace` directive.</span></span> <span data-ttu-id="5dfd9-596">Oluşturulan ad alanının geri kalanı (sonek bölümü), *_Viewwimports. cshtml* dosyasını ve sayfayı içeren klasörü içeren, noktayla ayrılmış göreli yoldur.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-596">The rest of the generated namespace (the suffix portion) is the dot-separated relative path between the folder containing *_ViewImports.cshtml* and the folder containing the page.</span></span>

<span data-ttu-id="5dfd9-597">Örneğin, `PageModel` Class *Pages/Customers/Edit. cshtml. cs* , ad alanını açıkça ayarlar:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-597">For example, the `PageModel` class *Pages/Customers/Edit.cshtml.cs* explicitly sets the namespace:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=snippet_namespace)]

<span data-ttu-id="5dfd9-598">*Pages/_Viewwimports. cshtml* dosyası aşağıdaki ad alanını ayarlar:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-598">The *Pages/_ViewImports.cshtml* file sets the following namespace:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]

<span data-ttu-id="5dfd9-599">*Pages/Customers/Edit. cshtml* Razor sayfasının oluşturulan ad alanı `PageModel` sınıfıyla aynıdır.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-599">The generated namespace for the *Pages/Customers/Edit.cshtml* Razor Page is the same as the `PageModel` class.</span></span>

<span data-ttu-id="5dfd9-600">`@namespace` *Ayrıca geleneksel Razor görünümleriyle birlikte kullanılabilir.*</span><span class="sxs-lookup"><span data-stu-id="5dfd9-600">`@namespace` *also works with conventional Razor views.*</span></span>

<span data-ttu-id="5dfd9-601">Özgün *Sayfalar/Create. cshtml* görünüm dosyası:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-601">The original *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml?highlight=2)]

<span data-ttu-id="5dfd9-602">Güncelleştirilmiş *Sayfalar/Create. cshtml* görünüm dosyası:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-602">The updated *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/Create.cshtml?highlight=2)]

<span data-ttu-id="5dfd9-603">[Razor Pages Başlatıcı projesi](#rpvs17) , istemci tarafı doğrulamayı bağlayan *sayfaları/_ValidationScriptsPartial. cshtml*'yi içerir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-603">The [Razor Pages starter project](#rpvs17) contains the *Pages/_ValidationScriptsPartial.cshtml*, which hooks up client-side validation.</span></span>

<span data-ttu-id="5dfd9-604">Kısmi görünümler hakkında daha fazla bilgi için bkz. <xref:mvc/views/partial>.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-604">For more information on partial views, see <xref:mvc/views/partial>.</span></span>

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a><span data-ttu-id="5dfd9-605">Sayfalar için URL oluşturma</span><span class="sxs-lookup"><span data-stu-id="5dfd9-605">URL generation for Pages</span></span>

<span data-ttu-id="5dfd9-606">Daha önce gösterilen `Create` sayfası `RedirectToPage`kullanır:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-606">The `Create` page, shown previously, uses `RedirectToPage`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=10)]

<span data-ttu-id="5dfd9-607">Uygulama aşağıdaki dosya/klasör yapısına sahiptir:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-607">The app has the following file/folder structure:</span></span>

* <span data-ttu-id="5dfd9-608">*/Pages*</span><span class="sxs-lookup"><span data-stu-id="5dfd9-608">*/Pages*</span></span>

  * <span data-ttu-id="5dfd9-609">*Index. cshtml*</span><span class="sxs-lookup"><span data-stu-id="5dfd9-609">*Index.cshtml*</span></span>
  * <span data-ttu-id="5dfd9-610">*/Customers*</span><span class="sxs-lookup"><span data-stu-id="5dfd9-610">*/Customers*</span></span>

    * <span data-ttu-id="5dfd9-611">*. Cshtml oluştur*</span><span class="sxs-lookup"><span data-stu-id="5dfd9-611">*Create.cshtml*</span></span>
    * <span data-ttu-id="5dfd9-612">*Edit. cshtml*</span><span class="sxs-lookup"><span data-stu-id="5dfd9-612">*Edit.cshtml*</span></span>
    * <span data-ttu-id="5dfd9-613">*Index. cshtml*</span><span class="sxs-lookup"><span data-stu-id="5dfd9-613">*Index.cshtml*</span></span>

<span data-ttu-id="5dfd9-614">*Pages/Customers/Create. cshtml* ve *Pages/Customers/Edit. cshtml* sayfaları, başarılı olduktan sonra *Pages/Index. cshtml* dosyasına yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-614">The *Pages/Customers/Create.cshtml* and *Pages/Customers/Edit.cshtml* pages redirect to *Pages/Index.cshtml* after success.</span></span> <span data-ttu-id="5dfd9-615">`/Index` dize, önceki sayfaya erişmek için URI 'nin bir parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-615">The string `/Index` is part of the URI to access the preceding page.</span></span> <span data-ttu-id="5dfd9-616">`/Index` dize */Index. cshtml* sayfasına URI oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-616">The string `/Index` can be used to generate URIs to the *Pages/Index.cshtml* page.</span></span> <span data-ttu-id="5dfd9-617">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-617">For example:</span></span>

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">My Index Page</a>`
* `RedirectToPage("/Index")`

<span data-ttu-id="5dfd9-618">Sayfa adı, kök */Pages* klasöründeki, önde gelen `/` (örneğin, `/Index`) içeren sayfanın yoludur.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-618">The page name is the path to the page from the root */Pages* folder including a leading `/` (for example, `/Index`).</span></span> <span data-ttu-id="5dfd9-619">Önceki URL oluşturma örnekleri bir URL 'YI kodlamadan gelişmiş seçenekler ve işlevsel yetenekler sunar.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-619">The preceding URL generation samples offer enhanced options and functional capabilities over hardcoding a URL.</span></span> <span data-ttu-id="5dfd9-620">URL oluşturma [yönlendirme](xref:mvc/controllers/routing) kullanır ve yolun hedef yolda nasıl tanımlandığınıza göre parametreleri oluşturabilir ve kodlayabilir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-620">URL generation uses [routing](xref:mvc/controllers/routing) and can generate and encode parameters according to how the route is defined in the destination path.</span></span>

<span data-ttu-id="5dfd9-621">Sayfalar için URL oluşturma göreli adları destekler.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-621">URL generation for pages supports relative names.</span></span> <span data-ttu-id="5dfd9-622">Aşağıdaki tabloda, *sayfa/müşteri/oluşturma. cshtml*'den farklı `RedirectToPage` parametrelerle hangi dizin sayfasının seçildiği gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-622">The following table shows which Index page is selected with different `RedirectToPage` parameters from *Pages/Customers/Create.cshtml*:</span></span>

| <span data-ttu-id="5dfd9-623">RedirectToPage (x)</span><span class="sxs-lookup"><span data-stu-id="5dfd9-623">RedirectToPage(x)</span></span>| <span data-ttu-id="5dfd9-624">Sayfasında</span><span class="sxs-lookup"><span data-stu-id="5dfd9-624">Page</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="5dfd9-625">RedirectToPage ("/Index")</span><span class="sxs-lookup"><span data-stu-id="5dfd9-625">RedirectToPage("/Index")</span></span> | <span data-ttu-id="5dfd9-626">*Sayfa/dizin*</span><span class="sxs-lookup"><span data-stu-id="5dfd9-626">*Pages/Index*</span></span> |
| <span data-ttu-id="5dfd9-627">RedirectToPage ("./Index");</span><span class="sxs-lookup"><span data-stu-id="5dfd9-627">RedirectToPage("./Index");</span></span> | <span data-ttu-id="5dfd9-628">*Sayfalar/müşteriler/Dizin*</span><span class="sxs-lookup"><span data-stu-id="5dfd9-628">*Pages/Customers/Index*</span></span> |
| <span data-ttu-id="5dfd9-629">RedirectToPage (".. /İndex ")</span><span class="sxs-lookup"><span data-stu-id="5dfd9-629">RedirectToPage("../Index")</span></span> | <span data-ttu-id="5dfd9-630">*Sayfa/dizin*</span><span class="sxs-lookup"><span data-stu-id="5dfd9-630">*Pages/Index*</span></span> |
| <span data-ttu-id="5dfd9-631">RedirectToPage ("Dizin")</span><span class="sxs-lookup"><span data-stu-id="5dfd9-631">RedirectToPage("Index")</span></span>  | <span data-ttu-id="5dfd9-632">*Sayfalar/müşteriler/Dizin*</span><span class="sxs-lookup"><span data-stu-id="5dfd9-632">*Pages/Customers/Index*</span></span> |

<span data-ttu-id="5dfd9-633">`RedirectToPage("Index")`, `RedirectToPage("./Index")`ve `RedirectToPage("../Index")` *göreli adlardır*.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-633">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, and `RedirectToPage("../Index")`  are *relative names*.</span></span> <span data-ttu-id="5dfd9-634">`RedirectToPage` parametresi, hedef sayfanın adını hesaplamak için geçerli sayfanın yoluyla *birleştirilir* .</span><span class="sxs-lookup"><span data-stu-id="5dfd9-634">The `RedirectToPage` parameter is *combined* with the path of the current page to compute the name of the destination page.</span></span>  <!-- Review: Original had The provided string is combined with the page name of the current page to compute the name of the destination page.  page name, not page path -->

<span data-ttu-id="5dfd9-635">Karmaşık bir yapıya sahip siteler oluştururken göreli ad bağlama yararlı olur.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-635">Relative name linking is useful when building sites with a complex structure.</span></span> <span data-ttu-id="5dfd9-636">Bir klasördeki sayfalar arasında bağlantı sağlamak için göreli adlar kullanırsanız, bu klasörü yeniden adlandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-636">If you use relative names to link between pages in a folder, you can rename that folder.</span></span> <span data-ttu-id="5dfd9-637">Tüm bağlantılar hala çalışır (klasör adını içermediği için).</span><span class="sxs-lookup"><span data-stu-id="5dfd9-637">All the links still work (because they didn't include the folder name).</span></span>

<span data-ttu-id="5dfd9-638">Farklı bir [alandaki](xref:mvc/controllers/areas)bir sayfaya yeniden yönlendirmek için alanını belirtin:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-638">To redirect to a page in a different [Area](xref:mvc/controllers/areas), specify the area:</span></span>

```csharp
RedirectToPage("/Index", new { area = "Services" });
```

<span data-ttu-id="5dfd9-639">Daha fazla bilgi için bkz. <xref:mvc/controllers/areas>.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-639">For more information, see <xref:mvc/controllers/areas>.</span></span>

## <a name="viewdata-attribute"></a><span data-ttu-id="5dfd9-640">ViewData özniteliği</span><span class="sxs-lookup"><span data-stu-id="5dfd9-640">ViewData attribute</span></span>

<span data-ttu-id="5dfd9-641">Veri, [Viewdataattribute](/dotnet/api/microsoft.aspnetcore.mvc.viewdataattribute)içeren bir sayfaya geçirilebilir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-641">Data can be passed to a page with [ViewDataAttribute](/dotnet/api/microsoft.aspnetcore.mvc.viewdataattribute).</span></span> <span data-ttu-id="5dfd9-642">`[ViewData]` ve Razor sayfa modellerindeki özellikler, değerlerini, [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary)'den saklı ve yüklenmiş olarak alır.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-642">Properties on controllers or Razor Page models decorated with `[ViewData]` have their values stored and loaded from the [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary).</span></span>

<span data-ttu-id="5dfd9-643">Aşağıdaki örnekte `AboutModel`, `[ViewData]`ile donatılmış bir `Title` özelliği içerir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-643">In the following example, the `AboutModel` contains a `Title` property decorated with `[ViewData]`.</span></span> <span data-ttu-id="5dfd9-644">`Title` özelliği, hakkında sayfasının başlığına ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-644">The `Title` property is set to the title of the About page:</span></span>

```csharp
public class AboutModel : PageModel
{
    [ViewData]
    public string Title { get; } = "About";

    public void OnGet()
    {
    }
}
```

<span data-ttu-id="5dfd9-645">Hakkında sayfasında, `Title` özelliğine model özelliği olarak erişin:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-645">In the About page, access the `Title` property as a model property:</span></span>

```cshtml
<h1>@Model.Title</h1>
```

<span data-ttu-id="5dfd9-646">Mizanpajda, başlık ViewData sözlüğünden okundu:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-646">In the layout, the title is read from the ViewData dictionary:</span></span>

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"] - WebApplication</title>
    ...
```

## <a name="tempdata"></a><span data-ttu-id="5dfd9-647">TempData</span><span class="sxs-lookup"><span data-stu-id="5dfd9-647">TempData</span></span>

<span data-ttu-id="5dfd9-648">ASP.NET Core bir [denetleyicide](/dotnet/api/microsoft.aspnetcore.mvc.controller) [TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) özelliğini kullanıma sunar.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-648">ASP.NET Core exposes the [TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) property on a [controller](/dotnet/api/microsoft.aspnetcore.mvc.controller).</span></span> <span data-ttu-id="5dfd9-649">Bu özellik, okunana kadar verileri depolar.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-649">This property stores data until it's read.</span></span> <span data-ttu-id="5dfd9-650">`Keep` ve `Peek` yöntemleri silinmeden verileri incelemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-650">The `Keep` and `Peek` methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="5dfd9-651">`TempData`, bir tek istekten daha fazla veri gerektiğinde yeniden yönlendirme için kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-651">`TempData` is  useful for redirection, when data is needed for more than a single request.</span></span>

<span data-ttu-id="5dfd9-652">Aşağıdaki kod, `TempData`kullanarak `Message` değerini ayarlar:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-652">The following code sets the value of `Message` using `TempData`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,25&name=snippet_Temp)]

<span data-ttu-id="5dfd9-653">*Pages/Customers/Index. cshtml* dosyasında aşağıdaki biçimlendirme `TempData`kullanarak `Message` değerini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-653">The following markup in the *Pages/Customers/Index.cshtml* file displays the value of `Message` using `TempData`.</span></span>

```cshtml
<h3>Msg: @Model.Message</h3>
```

<span data-ttu-id="5dfd9-654">*Pages/Customers/Index. cshtml. cs* sayfa modeli, `[TempData]` özniteliğini `Message` özelliğine uygular.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-654">The *Pages/Customers/Index.cshtml.cs* page model applies the `[TempData]` attribute to the `Message` property.</span></span>

```cs
[TempData]
public string Message { get; set; }
```

<span data-ttu-id="5dfd9-655">Daha fazla bilgi için bkz. [TempData](xref:fundamentals/app-state#tempdata) .</span><span class="sxs-lookup"><span data-stu-id="5dfd9-655">For more information, see [TempData](xref:fundamentals/app-state#tempdata) .</span></span>

<a name="mhpp"></a>

## <a name="multiple-handlers-per-page"></a><span data-ttu-id="5dfd9-656">Sayfa başına birden çok işleyici</span><span class="sxs-lookup"><span data-stu-id="5dfd9-656">Multiple handlers per page</span></span>

<span data-ttu-id="5dfd9-657">Aşağıdaki sayfa `asp-page-handler` etiketi Yardımcısını kullanarak iki işleyici için biçimlendirme oluşturur:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-657">The following page generates markup for two handlers using the `asp-page-handler` Tag Helper:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

<!-- Review: the FormActionTagHelper applies to all <form /> elements on a Razor page, even when there's no `asp-` attribute   -->

<span data-ttu-id="5dfd9-658">Yukarıdaki örnekteki formda, her biri farklı bir URL 'ye göndermek için `FormActionTagHelper` kullanan iki gönderme düğmesi vardır.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-658">The form in the preceding example has two submit buttons, each using the `FormActionTagHelper` to submit to a different URL.</span></span> <span data-ttu-id="5dfd9-659">`asp-page-handler` özniteliği `asp-page`bir yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-659">The `asp-page-handler` attribute is a companion to `asp-page`.</span></span> <span data-ttu-id="5dfd9-660">`asp-page-handler`, bir sayfa tarafından tanımlanan her bir işleyici yöntemini gönderen URL 'Ler oluşturur.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-660">`asp-page-handler` generates URLs that submit to each of the handler methods defined by a page.</span></span> <span data-ttu-id="5dfd9-661">örnek geçerli sayfaya bağlandığından `asp-page` belirtilmedi.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-661">`asp-page` isn't specified because the sample is linking to the current page.</span></span>

<span data-ttu-id="5dfd9-662">Sayfa modeli:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-662">The page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]

<span data-ttu-id="5dfd9-663">Yukarıdaki kod, *adlandırılmış işleyici yöntemlerini*kullanır.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-663">The preceding code uses *named handler methods*.</span></span> <span data-ttu-id="5dfd9-664">Adlandırılmış işleyici yöntemleri, `On<HTTP Verb>` sonra ve `Async` önce (varsa), ad içindeki metin alınarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-664">Named handler methods are created by taking the text in the name after `On<HTTP Verb>` and before `Async` (if present).</span></span> <span data-ttu-id="5dfd9-665">Yukarıdaki örnekte, Page metotları OnPost**Joinlist**Async ve onpost**Joinlıstuc**Async ' dir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-665">In the preceding example, the page methods are OnPost**JoinList**Async and OnPost**JoinListUC**Async.</span></span> <span data-ttu-id="5dfd9-666">*Onpost* Ile *zaman uyumsuz* olarak kaldırıldığında, işleyici adları `JoinList` ve `JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-666">With *OnPost* and *Async* removed, the handler names are `JoinList` and `JoinListUC`.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?range=12-13)]

<span data-ttu-id="5dfd9-667">Yukarıdaki kodu kullanarak, `OnPostJoinListAsync` ' a gönderen URL yolu `https://localhost:5001/Customers/CreateFATH?handler=JoinList`.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-667">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `https://localhost:5001/Customers/CreateFATH?handler=JoinList`.</span></span> <span data-ttu-id="5dfd9-668">`OnPostJoinListUCAsync` ' a gönderen URL yolu `https://localhost:5001/Customers/CreateFATH?handler=JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-668">The URL path that submits to `OnPostJoinListUCAsync` is `https://localhost:5001/Customers/CreateFATH?handler=JoinListUC`.</span></span>

## <a name="custom-routes"></a><span data-ttu-id="5dfd9-669">Özel yollar</span><span class="sxs-lookup"><span data-stu-id="5dfd9-669">Custom routes</span></span>

<span data-ttu-id="5dfd9-670">`@page` yönergesini kullanarak şunları yapın:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-670">Use the `@page` directive to:</span></span>

* <span data-ttu-id="5dfd9-671">Sayfaya özel bir yol belirtin.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-671">Specify a custom route to a page.</span></span> <span data-ttu-id="5dfd9-672">Örneğin, hakkında sayfasına olan yol `@page "/Some/Other/Path"``/Some/Other/Path` olarak ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-672">For example, the route to the About page can be set to `/Some/Other/Path` with `@page "/Some/Other/Path"`.</span></span>
* <span data-ttu-id="5dfd9-673">Kesimleri bir sayfanın varsayılan yoluna ekleyin.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-673">Append segments to a page's default route.</span></span> <span data-ttu-id="5dfd9-674">Örneğin, bir "öğe" segmenti sayfanın varsayılan yoluna `@page "item"`eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-674">For example, an "item" segment can be added to a page's default route with `@page "item"`.</span></span>
* <span data-ttu-id="5dfd9-675">Bir sayfanın varsayılan yoluna parametreleri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-675">Append parameters to a page's default route.</span></span> <span data-ttu-id="5dfd9-676">Örneğin, `@page "{id}"`bir sayfa için `id`bir ID parametresi gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-676">For example, an ID parameter, `id`, can be required for a page with `@page "{id}"`.</span></span>

<span data-ttu-id="5dfd9-677">Yolun başındaki bir tilde (`~`) tarafından atanan kök göreli bir yol desteklenir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-677">A root-relative path designated by a tilde (`~`) at the beginning of the path is supported.</span></span> <span data-ttu-id="5dfd9-678">Örneğin, `@page "~/Some/Other/Path"` `@page "/Some/Other/Path"`aynıdır.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-678">For example, `@page "~/Some/Other/Path"` is the same as `@page "/Some/Other/Path"`.</span></span>

<span data-ttu-id="5dfd9-679">Yol şablonu `@page "{handler?}"`belirterek `/JoinList` URL 'sindeki `?handler=JoinList` sorgu dizesini bir rota kesimine değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-679">You can change the query string `?handler=JoinList` in the URL to a route segment `/JoinList` by specifying the route template `@page "{handler?}"`.</span></span>

<span data-ttu-id="5dfd9-680">Sorgu dizesini URL 'de `?handler=JoinList` beğenmezseniz, işleyicinin yol bölümüne işleyici adını koymak için yolu değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-680">If you don't like the query string `?handler=JoinList` in the URL, you can change the route to put the handler name in the path portion of the URL.</span></span> <span data-ttu-id="5dfd9-681">Yolu, `@page` yönergesinden sonra çift tırnak içine alınmış bir rota şablonu ekleyerek özelleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-681">You can customize the route by adding a route template enclosed in double quotes after the `@page` directive.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]

<span data-ttu-id="5dfd9-682">Yukarıdaki kodu kullanarak, `OnPostJoinListAsync` ' a gönderen URL yolu `https://localhost:5001/Customers/CreateFATH/JoinList`.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-682">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `https://localhost:5001/Customers/CreateFATH/JoinList`.</span></span> <span data-ttu-id="5dfd9-683">`OnPostJoinListUCAsync` ' a gönderen URL yolu `https://localhost:5001/Customers/CreateFATH/JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-683">The URL path that submits to `OnPostJoinListUCAsync` is `https://localhost:5001/Customers/CreateFATH/JoinListUC`.</span></span>

<span data-ttu-id="5dfd9-684">Aşağıdaki `handler` `?` yol parametresinin isteğe bağlı olduğu anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-684">The `?` following `handler` means the route parameter is optional.</span></span>

## <a name="configuration-and-settings"></a><span data-ttu-id="5dfd9-685">Yapılandırma ve ayarlar</span><span class="sxs-lookup"><span data-stu-id="5dfd9-685">Configuration and settings</span></span>

<span data-ttu-id="5dfd9-686">Gelişmiş seçenekleri yapılandırmak için, MVC Oluşturucu 'da `AddRazorPagesOptions` genişletme yöntemini kullanın:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-686">To configure advanced options, use the extension method `AddRazorPagesOptions` on the MVC builder:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/StartupAdvanced.cs?name=snippet_1)]

<span data-ttu-id="5dfd9-687">Şu anda, sayfalar için kök dizini ayarlamak veya sayfalar için uygulama modeli kuralları eklemek üzere `RazorPagesOptions` kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-687">Currently you can use the `RazorPagesOptions` to set the root directory for pages, or add application model conventions for pages.</span></span> <span data-ttu-id="5dfd9-688">Gelecekte bu şekilde daha fazla genişletilebilirlik etkinleştireceğiz.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-688">We'll enable more extensibility this way in the future.</span></span>

<span data-ttu-id="5dfd9-689">Görünümleri önceden derlemek için bkz. [Razor görünüm derlemesi](xref:mvc/views/view-compilation) .</span><span class="sxs-lookup"><span data-stu-id="5dfd9-689">To precompile views, see [Razor view compilation](xref:mvc/views/view-compilation) .</span></span>

<span data-ttu-id="5dfd9-690">[Örnek kodu indirin veya görüntüleyin](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/index/sample).</span><span class="sxs-lookup"><span data-stu-id="5dfd9-690">[Download or view sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/index/sample).</span></span>

<span data-ttu-id="5dfd9-691">Bu giriş hakkında bilgi için bkz. [Razor Pages kullanmaya başlama](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="5dfd9-691">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start), which builds on this introduction.</span></span>

### <a name="specify-that-razor-pages-are-at-the-content-root"></a><span data-ttu-id="5dfd9-692">Razor Pages içerik kökünde olduğunu belirtin</span><span class="sxs-lookup"><span data-stu-id="5dfd9-692">Specify that Razor Pages are at the content root</span></span>

<span data-ttu-id="5dfd9-693">Varsayılan olarak, Razor Pages */Pages* dizininde kök olarak depolanır.</span><span class="sxs-lookup"><span data-stu-id="5dfd9-693">By default, Razor Pages are rooted in the */Pages* directory.</span></span> <span data-ttu-id="5dfd9-694">Razor Pages, uygulamanın [içerik kökünde](xref:fundamentals/index#content-root) ([contentrootpath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) olduğunu belirtmek Için [addmvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) 'ye [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) ekleyin:</span><span class="sxs-lookup"><span data-stu-id="5dfd9-694">Add [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at the [content root](xref:fundamentals/index#content-root) ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) of the app:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesAtContentRoot();
```

### <a name="specify-that-razor-pages-are-at-a-custom-root-directory"></a><span data-ttu-id="5dfd9-695">Razor Pages özel kök dizinde olduğunu belirtin</span><span class="sxs-lookup"><span data-stu-id="5dfd9-695">Specify that Razor Pages are at a custom root directory</span></span>

<span data-ttu-id="5dfd9-696">Razor Pages uygulamadaki özel bir kök dizinde olduğunu belirtmek için [Addmvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) 'ye [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) ekleyin (göreli bir yol sağlayın):</span><span class="sxs-lookup"><span data-stu-id="5dfd9-696">Add [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at a custom root directory in the app (provide a relative path):</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesRoot("/path/to/razor/pages");
```

## <a name="additional-resources"></a><span data-ttu-id="5dfd9-697">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="5dfd9-697">Additional resources</span></span>

* <xref:index>
* <xref:mvc/views/razor>
* <xref:mvc/controllers/areas>
* <xref:tutorials/razor-pages/razor-pages-start>
* <xref:security/authorization/razor-pages-authorization>
* <xref:razor-pages/razor-pages-conventions>
* <xref:test/razor-pages-tests>
* <xref:mvc/views/partial>

::: moniker-end
