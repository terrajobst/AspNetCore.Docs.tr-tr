---
title: ASP.NET Core Razor Pages giriş
author: Rick-Anderson
description: Nasıl ASP.NET Core Razor sayfalar kodlama sayfa odaklı senaryolar daha kolay ve MVC kullanmaktan daha üretken hale getirdiğini öğrenin.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 02/12/2020
uid: razor-pages/index
ms.openlocfilehash: 42ffb0d4d2e49663dd53ffeee5d9fa2a931ee5b7
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78667582"
---
# <a name="introduction-to-razor-pages-in-aspnet-core"></a><span data-ttu-id="9d1cc-103">ASP.NET Core Razor Pages giriş</span><span class="sxs-lookup"><span data-stu-id="9d1cc-103">Introduction to Razor Pages in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="9d1cc-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Ryan şimdi ak](https://github.com/rynowak)</span><span class="sxs-lookup"><span data-stu-id="9d1cc-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Ryan Nowak](https://github.com/rynowak)</span></span>

<span data-ttu-id="9d1cc-105">Razor Pages, kodlama sayfasına odaklanmış senaryolar denetleyicileri ve görünümleri kullanmaktan daha kolay ve daha üretken hale getirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-105">Razor Pages can make coding page-focused scenarios easier and more productive than using controllers and views.</span></span>

<span data-ttu-id="9d1cc-106">Model-View-Controller yaklaşımını kullanan bir öğretici arıyorsanız, bkz. [ASP.NET Core MVC ile çalışmaya başlama](xref:tutorials/first-mvc-app/start-mvc).</span><span class="sxs-lookup"><span data-stu-id="9d1cc-106">If you're looking for a tutorial that uses the Model-View-Controller approach, see [Get started with ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span></span>

<span data-ttu-id="9d1cc-107">Bu belge Razor Pages bir giriş sağlar.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-107">This document provides an introduction to Razor Pages.</span></span> <span data-ttu-id="9d1cc-108">Adım adım öğretici değildir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-108">It's not a step by step tutorial.</span></span> <span data-ttu-id="9d1cc-109">Bölümlerden bazılarını çok gelişmiş bir şekilde bulursanız, bkz. [Razor Pages kullanmaya başlama](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="9d1cc-109">If you find some of the sections too advanced, see [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span></span> <span data-ttu-id="9d1cc-110">ASP.NET Core genel bir bakış için bkz. [ASP.NET Core giriş](xref:index).</span><span class="sxs-lookup"><span data-stu-id="9d1cc-110">For an overview of ASP.NET Core, see the [Introduction to ASP.NET Core](xref:index).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9d1cc-111">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="9d1cc-111">Prerequisites</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="9d1cc-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9d1cc-112">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="9d1cc-113">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9d1cc-113">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="9d1cc-114">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9d1cc-114">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

<a name="rpvs17"></a>

## <a name="create-a-razor-pages-project"></a><span data-ttu-id="9d1cc-115">Razor Pages projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="9d1cc-115">Create a Razor Pages project</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="9d1cc-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9d1cc-116">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="9d1cc-117">Razor Pages projesi oluşturma hakkında ayrıntılı yönergeler için bkz. [Razor Pages kullanmaya başlama](xref:tutorials/razor-pages/razor-pages-start) .</span><span class="sxs-lookup"><span data-stu-id="9d1cc-117">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) for detailed instructions on how to create a Razor Pages project.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="9d1cc-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9d1cc-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="9d1cc-119">Komut satırından `dotnet new webapp` çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-119">Run `dotnet new webapp` from the command line.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="9d1cc-120">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9d1cc-120">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="9d1cc-121">Komut satırından `dotnet new webapp` çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-121">Run `dotnet new webapp` from the command line.</span></span>

<span data-ttu-id="9d1cc-122">Oluşturulan *. csproj* dosyasını Mac için Visual Studio açın.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-122">Open the generated *.csproj* file from Visual Studio for Mac.</span></span>

---

## <a name="razor-pages"></a><span data-ttu-id="9d1cc-123">Razor Pages</span><span class="sxs-lookup"><span data-stu-id="9d1cc-123">Razor Pages</span></span>

<span data-ttu-id="9d1cc-124">Razor Pages, *Startup.cs*'de etkinleştirilmiştir:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-124">Razor Pages is enabled in *Startup.cs*:</span></span>

[!code-cs[](index/3.0sample/RazorPagesIntro/Startup.cs?name=snippet_Startup&highlight=12,36)]

<span data-ttu-id="9d1cc-125">Temel bir sayfa düşünün:<a name="OnGet"></a></span><span class="sxs-lookup"><span data-stu-id="9d1cc-125">Consider a basic page: <a name="OnGet"></a></span></span>

[!code-cshtml[](index/3.0sample/RazorPagesIntro/Pages/Index.cshtml?highlight=1)]

<span data-ttu-id="9d1cc-126">Yukarıdaki kod, denetleyiciler ve görünümlerle ASP.NET Core bir uygulamada kullanılan [Razor görünüm dosyası](xref:tutorials/first-mvc-app/adding-view) gibi bir çok şey arar.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-126">The preceding code looks a lot like a [Razor view file](xref:tutorials/first-mvc-app/adding-view) used in an ASP.NET Core app with controllers and views.</span></span> <span data-ttu-id="9d1cc-127">Bu, farklı kılan [`@page`](xref:mvc/views/razor#page) yönergedir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-127">What makes it different is the [`@page`](xref:mvc/views/razor#page) directive.</span></span> <span data-ttu-id="9d1cc-128">`@page`, dosyayı bir denetleyiciye geçmeden doğrudan istekleri işlediği anlamına gelen bir MVC eylemine sahip olur.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-128">`@page` makes the file into an MVC action - which means that it handles requests directly, without going through a controller.</span></span> <span data-ttu-id="9d1cc-129">`@page` sayfadaki ilk Razor yönergesi olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-129">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="9d1cc-130">`@page` diğer [Razor](xref:mvc/views/razor) yapıları davranışını etkiler.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-130">`@page` affects the behavior of other [Razor](xref:mvc/views/razor) constructs.</span></span> <span data-ttu-id="9d1cc-131">Razor Pages dosya adlarında *. cshtml* soneki vardır.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-131">Razor Pages file names have a *.cshtml* suffix.</span></span>

<span data-ttu-id="9d1cc-132">`PageModel` sınıfı kullanan benzer bir sayfa aşağıdaki iki dosyada gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-132">A similar page, using a `PageModel` class, is shown in the following two files.</span></span> <span data-ttu-id="9d1cc-133">*Pages/Index2. cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-133">The *Pages/Index2.cshtml* file:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesIntro/Pages/Index2.cshtml)]

<span data-ttu-id="9d1cc-134">*Pages/Index2. cshtml. cs* sayfa modeli:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-134">The *Pages/Index2.cshtml.cs* page model:</span></span>

[!code-cs[](index/3.0sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]

<span data-ttu-id="9d1cc-135">Kurala göre `PageModel` sınıf dosyası, *. cs* eklenmiş Razor sayfası dosyasıyla aynı ada sahiptir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-135">By convention, the `PageModel` class file has the same name as the Razor Page file with *.cs* appended.</span></span> <span data-ttu-id="9d1cc-136">Örneğin, önceki Razor sayfası *Pages/Index2. cshtml*' dir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-136">For example, the previous Razor Page is *Pages/Index2.cshtml*.</span></span> <span data-ttu-id="9d1cc-137">`PageModel` sınıfını içeren dosya *sayfa/Index2. cshtml. cs*olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-137">The file containing the `PageModel` class is named *Pages/Index2.cshtml.cs*.</span></span>

<span data-ttu-id="9d1cc-138">URL yollarının sayfalara olan ilişkilendirmeleri, sayfanın dosya sistemindeki konumuna göre belirlenir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-138">The associations of URL paths to pages are determined by the page's location in the file system.</span></span> <span data-ttu-id="9d1cc-139">Aşağıdaki tabloda bir Razor sayfa yolu ve eşleşen URL gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-139">The following table shows a Razor Page path and the matching URL:</span></span>

| <span data-ttu-id="9d1cc-140">Dosya adı ve yolu</span><span class="sxs-lookup"><span data-stu-id="9d1cc-140">File name and path</span></span>               | <span data-ttu-id="9d1cc-141">eşleşen URL</span><span class="sxs-lookup"><span data-stu-id="9d1cc-141">matching URL</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="9d1cc-142">*/Pages/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="9d1cc-142">*/Pages/Index.cshtml*</span></span> | <span data-ttu-id="9d1cc-143">`/` veya `/Index`</span><span class="sxs-lookup"><span data-stu-id="9d1cc-143">`/` or `/Index`</span></span> |
| <span data-ttu-id="9d1cc-144">*/Pages/Contact.exe*</span><span class="sxs-lookup"><span data-stu-id="9d1cc-144">*/Pages/Contact.cshtml*</span></span> | `/Contact` |
| <span data-ttu-id="9d1cc-145">*/Pages/Store/Contact.exe*</span><span class="sxs-lookup"><span data-stu-id="9d1cc-145">*/Pages/Store/Contact.cshtml*</span></span> | `/Store/Contact` |
| <span data-ttu-id="9d1cc-146">*/Pages/Store/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="9d1cc-146">*/Pages/Store/Index.cshtml*</span></span> | <span data-ttu-id="9d1cc-147">`/Store` veya `/Store/Index`</span><span class="sxs-lookup"><span data-stu-id="9d1cc-147">`/Store` or `/Store/Index`</span></span> |

<span data-ttu-id="9d1cc-148">Notlar:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-148">Notes:</span></span>

* <span data-ttu-id="9d1cc-149">Çalışma zamanı, *Sayfalar* klasöründeki Razor Pages dosyaları varsayılan olarak arar.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-149">The runtime looks for Razor Pages files in the *Pages* folder by default.</span></span>
* <span data-ttu-id="9d1cc-150">`Index`, URL bir sayfa içermiyorsa varsayılan sayfasıdır.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-150">`Index` is the default page when a URL doesn't include a page.</span></span>

## <a name="write-a-basic-form"></a><span data-ttu-id="9d1cc-151">Temel form yazma</span><span class="sxs-lookup"><span data-stu-id="9d1cc-151">Write a basic form</span></span>

<span data-ttu-id="9d1cc-152">Razor Pages, Web tarayıcıları ile kullanılan ortak desenleri bir uygulama oluştururken kolayca uygulanması için tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-152">Razor Pages is designed to make common patterns used with web browsers easy to implement when building an app.</span></span> <span data-ttu-id="9d1cc-153">[Model bağlama](xref:mvc/models/model-binding), [ETIKET yardımcıları](xref:mvc/views/tag-helpers/intro)ve HTML Yardımcıları hepsi, Razor sayfası sınıfında tanımlanan özelliklerle *çalışır* .</span><span class="sxs-lookup"><span data-stu-id="9d1cc-153">[Model binding](xref:mvc/models/model-binding), [Tag Helpers](xref:mvc/views/tag-helpers/intro), and HTML helpers all *just work* with the properties defined in a Razor Page class.</span></span> <span data-ttu-id="9d1cc-154">`Contact` modeli için temel bir "bize başvurun" formu uygulayan bir sayfa düşünün:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-154">Consider a page that implements a basic "contact us" form for the `Contact` model:</span></span>

<span data-ttu-id="9d1cc-155">Bu belgedeki örnekler için `DbContext`, [Startup.cs](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/razor-pages/index/3.0sample/RazorPagesContacts/Startup.cs#L23-L24) dosyasında başlatılır.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-155">For the samples in this document, the `DbContext` is initialized in the [Startup.cs](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/razor-pages/index/3.0sample/RazorPagesContacts/Startup.cs#L23-L24) file.</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Startup.cs?name=snippet)]

<span data-ttu-id="9d1cc-156">Veri modeli:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-156">The data model:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Models/Customer.cs)]

<span data-ttu-id="9d1cc-157">DB bağlamı:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-157">The db context:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Data/CustomerDbContext.cs)]

<span data-ttu-id="9d1cc-158">*Pages/Create. cshtml* görünüm dosyası:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-158">The *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml)]

<span data-ttu-id="9d1cc-159">*Pages/Create. cshtml. cs* sayfa modeli:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-159">The *Pages/Create.cshtml.cs* page model:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml.cs?name=snippet_ALL)]

<span data-ttu-id="9d1cc-160">Kurala göre `PageModel` sınıfı `<PageName>Model` olarak adlandırılır ve sayfayla aynı ad alanında yer alan.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-160">By convention, the `PageModel` class is called `<PageName>Model` and is in the same namespace as the page.</span></span>

<span data-ttu-id="9d1cc-161">`PageModel` sınıfı, bir sayfa mantığının sunumundaki ayırmayı sağlar.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-161">The `PageModel` class allows separation of the logic of a page from its presentation.</span></span> <span data-ttu-id="9d1cc-162">Sayfaya gönderilen istekler için sayfa işleyicilerini ve sayfayı işlemek için kullanılan verileri tanımlar.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-162">It defines page handlers for requests sent to the page and the data used to render the page.</span></span> <span data-ttu-id="9d1cc-163">Bu ayrım şunları sağlar:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-163">This separation allows:</span></span>

* <span data-ttu-id="9d1cc-164">[Bağımlılık ekleme](xref:fundamentals/dependency-injection)yoluyla sayfa bağımlılıklarını yönetme.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-164">Managing of page dependencies through [dependency injection](xref:fundamentals/dependency-injection).</span></span>
* [<span data-ttu-id="9d1cc-165">Birim testi</span><span class="sxs-lookup"><span data-stu-id="9d1cc-165">Unit testing</span></span>](xref:test/razor-pages-tests)

<span data-ttu-id="9d1cc-166">Sayfada, `POST` isteklerinde çalışan bir `OnPostAsync` *işleyicisi yöntemi*vardır (bir Kullanıcı formu gönderdiğinde).</span><span class="sxs-lookup"><span data-stu-id="9d1cc-166">The page has an `OnPostAsync` *handler method*, which runs on `POST` requests (when a user posts the form).</span></span> <span data-ttu-id="9d1cc-167">Herhangi bir HTTP fiili için işleyici metotları eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-167">Handler methods for any HTTP verb can be added.</span></span> <span data-ttu-id="9d1cc-168">En yaygın işleyiciler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-168">The most common handlers are:</span></span>

* <span data-ttu-id="9d1cc-169">Sayfanın başlatılması için gereken durum `OnGet`.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-169">`OnGet` to initialize state needed for the page.</span></span> <span data-ttu-id="9d1cc-170">Yukarıdaki kodda `OnGet` yöntemi *CreateModel. cshtml* Razor sayfasını görüntüler.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-170">In the preceding code, the `OnGet` method displays the *CreateModel.cshtml* Razor Page.</span></span>
* <span data-ttu-id="9d1cc-171">form gönderilerini işlemek için `OnPost`.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-171">`OnPost` to handle form submissions.</span></span>

<span data-ttu-id="9d1cc-172">`Async` adlandırma son eki isteğe bağlıdır, ancak genellikle zaman uyumsuz işlevler için kural tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-172">The `Async` naming suffix is optional but is often used by convention for asynchronous functions.</span></span> <span data-ttu-id="9d1cc-173">Yukarıdaki kod Razor Pages için tipik bir davranıştır.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-173">The preceding code is typical for Razor Pages.</span></span>

<span data-ttu-id="9d1cc-174">Denetleyicileri ve görünümleri kullanarak ASP.NET uygulamaları hakkında bilginiz varsa:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-174">If you're familiar with ASP.NET apps using controllers and views:</span></span>

* <span data-ttu-id="9d1cc-175">Yukarıdaki örnekteki `OnPostAsync` kodu, tipik denetleyici koduna benzer şekilde görünür.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-175">The `OnPostAsync` code in the preceding example looks similar to typical controller code.</span></span>
* <span data-ttu-id="9d1cc-176">[Model bağlama](xref:mvc/models/model-binding), [doğrulama](xref:mvc/models/validation)ve eylem sonuçları gibi mvc temel elemanlarının çoğu denetleyiciler ve Razor Pages aynı şekilde çalışır.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-176">Most of the MVC primitives like [model binding](xref:mvc/models/model-binding), [validation](xref:mvc/models/validation), and action results work the same with Controllers and Razor Pages.</span></span> 

<span data-ttu-id="9d1cc-177">Önceki `OnPostAsync` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-177">The previous `OnPostAsync` method:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="9d1cc-178">`OnPostAsync`temel akışı:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-178">The basic flow of `OnPostAsync`:</span></span>

<span data-ttu-id="9d1cc-179">Doğrulama hatalarını kontrol edin.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-179">Check for validation errors.</span></span>

* <span data-ttu-id="9d1cc-180">Hata yoksa, verileri kaydedin ve yeniden yönlendirin.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-180">If there are no errors, save the data and redirect.</span></span>
* <span data-ttu-id="9d1cc-181">Hatalar varsa, doğrulama iletileriyle sayfayı yeniden görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-181">If there are errors, show the page again with validation messages.</span></span> <span data-ttu-id="9d1cc-182">Çoğu durumda, istemci üzerinde doğrulama hataları algılanır ve sunucuya hiçbir zaman gönderilmez.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-182">In many cases, validation errors would be detected on the client, and never submitted to the server.</span></span>

<span data-ttu-id="9d1cc-183">*Pages/Create. cshtml* görünüm dosyası:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-183">The *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml)]

<span data-ttu-id="9d1cc-184">Sayfalardan işlenmiş HTML */Create. cshtml*:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-184">The rendered HTML from *Pages/Create.cshtml*:</span></span>

[!code-html[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create4.html)]

<span data-ttu-id="9d1cc-185">Önceki kodda, formu deftere nakletme:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-185">In the previous code, posting the form:</span></span>

* <span data-ttu-id="9d1cc-186">Geçerli verilerle:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-186">With valid data:</span></span>

  * <span data-ttu-id="9d1cc-187">`OnPostAsync` Handler yöntemi <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel.RedirectToPage*> yardımcı yöntemini çağırır.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-187">The `OnPostAsync` handler method calls the <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel.RedirectToPage*> helper method.</span></span> <span data-ttu-id="9d1cc-188">`RedirectToPage`, <xref:Microsoft.AspNetCore.Mvc.RedirectToPageResult>örneğini döndürür.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-188">`RedirectToPage` returns an instance of <xref:Microsoft.AspNetCore.Mvc.RedirectToPageResult>.</span></span> <span data-ttu-id="9d1cc-189">`RedirectToPage`:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-189">`RedirectToPage`:</span></span>

    * <span data-ttu-id="9d1cc-190">Bir eylem sonucudur.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-190">Is an action result.</span></span>
    * <span data-ttu-id="9d1cc-191">`RedirectToAction` veya `RedirectToRoute` benzerdir (denetleyiciler ve görünümlerde kullanılır).</span><span class="sxs-lookup"><span data-stu-id="9d1cc-191">Is similar to `RedirectToAction` or `RedirectToRoute` (used in controllers and views).</span></span>
    * <span data-ttu-id="9d1cc-192">Sayfalar için özelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-192">Is customized for pages.</span></span> <span data-ttu-id="9d1cc-193">Önceki örnekte, kök dizin sayfasına (`/Index`) yeniden yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-193">In the preceding sample, it redirects to the root Index page (`/Index`).</span></span> <span data-ttu-id="9d1cc-194">`RedirectToPage`, [Sayfalar Için URL oluşturma](#url_gen) bölümünde ayrıntılı olarak açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-194">`RedirectToPage` is detailed in the [URL generation for Pages](#url_gen) section.</span></span>

* <span data-ttu-id="9d1cc-195">Sunucuya geçirilen doğrulama hatalarıyla birlikte:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-195">With validation errors that are passed to the server:</span></span>

  * <span data-ttu-id="9d1cc-196">`OnPostAsync` Handler yöntemi <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageBase.Page*> yardımcı yöntemini çağırır.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-196">The `OnPostAsync` handler method calls the <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageBase.Page*> helper method.</span></span> <span data-ttu-id="9d1cc-197">`Page`, <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageResult>örneğini döndürür.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-197">`Page` returns an instance of <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageResult>.</span></span> <span data-ttu-id="9d1cc-198">`Page` döndürmek, denetleyicilerde eylemlerin `View`nasıl dönüşlerine benzer.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-198">Returning `Page` is similar to how actions in controllers return `View`.</span></span> <span data-ttu-id="9d1cc-199">`PageResult`, bir işleyici yöntemi için varsayılan dönüş türüdür.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-199">`PageResult` is the default return type for a handler method.</span></span> <span data-ttu-id="9d1cc-200">`void` döndüren bir işleyici yöntemi sayfayı işler.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-200">A handler method that returns `void` renders the page.</span></span>
  * <span data-ttu-id="9d1cc-201">Yukarıdaki örnekte, formun hiçbir değer olmadan nakledilmesi [ModelState ile sonuçlanır. IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) yanlış döndürüyor.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-201">In the preceding example, posting the form with no value results in [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) returning false.</span></span> <span data-ttu-id="9d1cc-202">Bu örnekte, istemcide hiçbir doğrulama hatası gösterilmezler.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-202">In this sample, no validation errors are displayed on the client.</span></span> <span data-ttu-id="9d1cc-203">Doğrulama hatası teslim etme bu belgenin ilerleyen bölümlerinde ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-203">Validation error handing is covered later in this document.</span></span>

  [!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=3-6)]

* <span data-ttu-id="9d1cc-204">İstemci tarafı doğrulaması tarafından algılanan doğrulama hatalarıyla birlikte:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-204">With validation errors detected by client side validation:</span></span>

  * <span data-ttu-id="9d1cc-205">Veriler sunucuya **nakledilmedi.**</span><span class="sxs-lookup"><span data-stu-id="9d1cc-205">Data is **not** posted to the server.</span></span>
  * <span data-ttu-id="9d1cc-206">İstemci tarafı doğrulaması bu belgenin ilerleyen kısımlarında açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-206">Client-side validation is explained later in this document.</span></span>

<span data-ttu-id="9d1cc-207">`Customer` özelliği, model bağlamasını kabul etmek için [`[BindProperty]`](xref:Microsoft.AspNetCore.Mvc.BindPropertyAttribute) özniteliğini kullanır:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-207">The `Customer` property uses [`[BindProperty]`](xref:Microsoft.AspNetCore.Mvc.BindPropertyAttribute) attribute to opt in to model binding:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml.cs?name=snippet_PageModel&highlight=15-16)]

<span data-ttu-id="9d1cc-208">`[BindProperty]`, istemci tarafından değiştirilmemesi gereken özellikler içeren **modellerde kullanılmamalıdır.**</span><span class="sxs-lookup"><span data-stu-id="9d1cc-208">`[BindProperty]` should **not** be used on models containing properties that should not be changed by the client.</span></span> <span data-ttu-id="9d1cc-209">Daha fazla bilgi için bkz. fazla [nakil](xref:data/ef-rp/crud#overposting).</span><span class="sxs-lookup"><span data-stu-id="9d1cc-209">For more information, see [Overposting](xref:data/ef-rp/crud#overposting).</span></span>

<span data-ttu-id="9d1cc-210">Razor Pages, varsayılan olarak, özellikleri yalnızca`GET` olmayan fiiller ile bağlayın.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-210">Razor Pages, by default, bind properties only with non-`GET` verbs.</span></span> <span data-ttu-id="9d1cc-211">Özelliklere bağlama, HTTP verilerini model türüne dönüştürmek için kod yazma ihtiyacını ortadan kaldırır.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-211">Binding to properties removes the need to writing code to convert HTTP data to the model type.</span></span> <span data-ttu-id="9d1cc-212">Bağlama, form alanlarını işlemek için aynı özelliği kullanarak kodu azaltır (`<input asp-for="Customer.Name">`) ve girişi kabul eder.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-212">Binding reduces code by using the same property to render form fields (`<input asp-for="Customer.Name">`) and accept the input.</span></span>

[!INCLUDE[](~/includes/bind-get.md)]

<span data-ttu-id="9d1cc-213">*Sayfalar/oluşturma. cshtml* görünüm dosyası gözden geçiriliyor:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-213">Reviewing the *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml?highlight=3,9)]

* <span data-ttu-id="9d1cc-214">Yukarıdaki kodda, [giriş etiketi yardımcısı](xref:mvc/views/working-with-forms#the-input-tag-helper) `<input asp-for="Customer.Name" />` HTML `<input>` öğesini `Customer.Name` model ifadesine bağlar.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-214">In the preceding code, the [input tag helper](xref:mvc/views/working-with-forms#the-input-tag-helper) `<input asp-for="Customer.Name" />` binds the HTML `<input>` element to the `Customer.Name` model expression.</span></span>
* <span data-ttu-id="9d1cc-215">[`@addTagHelper`](xref:mvc/views/tag-helpers/intro#addtaghelper-makes-tag-helpers-available) etiket yardımcılarını kullanılabilir hale getirir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-215">[`@addTagHelper`](xref:mvc/views/tag-helpers/intro#addtaghelper-makes-tag-helpers-available) makes Tag Helpers available.</span></span>

### <a name="the-home-page"></a><span data-ttu-id="9d1cc-216">Giriş sayfası</span><span class="sxs-lookup"><span data-stu-id="9d1cc-216">The home page</span></span>

<span data-ttu-id="9d1cc-217">*Index. cshtml* giriş sayfasıdır:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-217">*Index.cshtml* is the home page:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Index.cshtml)]

<span data-ttu-id="9d1cc-218">İlişkili `PageModel` sınıfı (*Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="9d1cc-218">The associated `PageModel` class (*Index.cshtml.cs*):</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Index.cshtml.cs?name=snippet)]

<span data-ttu-id="9d1cc-219">*Index. cshtml* dosyası aşağıdaki biçimlendirmeyi içerir:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-219">The *Index.cshtml* file contains the following markup:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Index.cshtml?range=21)]

<span data-ttu-id="9d1cc-220">`<a /a>` [tutturucu etiketi Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) , düzenleme sayfasına bir bağlantı oluşturmak için `asp-route-{value}` özniteliğini kullandı.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-220">The `<a /a>` [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) used the `asp-route-{value}` attribute to generate a link to the Edit page.</span></span> <span data-ttu-id="9d1cc-221">Bağlantı, iletişim KIMLIĞINE sahip rota verileri içerir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-221">The link contains route data with the contact ID.</span></span> <span data-ttu-id="9d1cc-222">Örneğin, `https://localhost:5001/Edit/1`.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-222">For example, `https://localhost:5001/Edit/1`.</span></span> <span data-ttu-id="9d1cc-223">[Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro), Razor dosyalarında HTML öğelerinin oluşturulmasına ve işlenmesine sunucu tarafı kodun katılmasını etkinleştir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-223">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span>

<span data-ttu-id="9d1cc-224">*Index. cshtml* dosyası her müşteri için bir silme düğmesi oluşturmak için biçimlendirme içerir:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-224">The *Index.cshtml* file contains markup to create a delete button for each customer contact:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Index.cshtml?range=22-23)]

<span data-ttu-id="9d1cc-225">İşlenmiş HTML:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-225">The rendered HTML:</span></span>

```html
<button type="submit" formaction="/Customers?id=1&amp;handler=delete">delete</button>
```

<span data-ttu-id="9d1cc-226">Sil düğmesi HTML 'de işlendiğinde, bu nesnenin [biçimlendirme](https://developer.mozilla.org/docs/Web/HTML/Element/button#attr-formaction) parametreleri içerir:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-226">When the delete button is rendered in HTML, its [formaction](https://developer.mozilla.org/docs/Web/HTML/Element/button#attr-formaction) includes parameters for:</span></span>

* <span data-ttu-id="9d1cc-227">`asp-route-id` özniteliğiyle belirtilen müşteri iletişim KIMLIĞI.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-227">The customer contact ID, specified by the `asp-route-id` attribute.</span></span>
* <span data-ttu-id="9d1cc-228">`asp-page-handler` özniteliğiyle belirtilen `handler`.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-228">The `handler`, specified by the `asp-page-handler` attribute.</span></span>

<span data-ttu-id="9d1cc-229">Düğme seçildiğinde, sunucuya bir form `POST` isteği gönderilir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-229">When the button is selected, a form `POST` request is sent to the server.</span></span> <span data-ttu-id="9d1cc-230">Kurala göre, işleyici yönteminin adı, düzen `OnPost[handler]Async`göre `handler` parametresinin değerine göre seçilir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-230">By convention, the name of the handler method is selected based on the value of the `handler` parameter according to the scheme `OnPost[handler]Async`.</span></span>

<span data-ttu-id="9d1cc-231">Bu örnekte `handler` `delete` olduğundan, `OnPostDeleteAsync` Handler yöntemi `POST` isteğini işlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-231">Because the `handler` is `delete` in this example, the `OnPostDeleteAsync` handler method is used to process the `POST` request.</span></span> <span data-ttu-id="9d1cc-232">`asp-page-handler`, `remove`gibi farklı bir değere ayarlandıysa `OnPostRemoveAsync` ada sahip bir işleyici yöntemi seçilidir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-232">If the `asp-page-handler` is set to a different value, such as `remove`, a handler method with the name `OnPostRemoveAsync` is selected.</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Index.cshtml.cs?name=snippet2)]

<span data-ttu-id="9d1cc-233">`OnPostDeleteAsync` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-233">The `OnPostDeleteAsync` method:</span></span>

* <span data-ttu-id="9d1cc-234">Sorgu dizesinden `id` alır.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-234">Gets the `id` from the query string.</span></span>
* <span data-ttu-id="9d1cc-235">`FindAsync`ile müşteri iletişim için veritabanını sorgular.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-235">Queries the database for the customer contact with `FindAsync`.</span></span>
* <span data-ttu-id="9d1cc-236">Müşteri ilgili kişisi bulunursa, kaldırılır ve veritabanı güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-236">If the customer contact is found, it's removed and the database is updated.</span></span>
* <span data-ttu-id="9d1cc-237">Kök dizin sayfasına yeniden yönlendirmek için <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel.RedirectToPage*> çağırır (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="9d1cc-237">Calls <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel.RedirectToPage*> to redirect to the root Index page (`/Index`).</span></span>

### <a name="the-editcshtml-file"></a><span data-ttu-id="9d1cc-238">Edit. cshtml dosyası</span><span class="sxs-lookup"><span data-stu-id="9d1cc-238">The Edit.cshtml file</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Edit.cshtml?highlight=1)]

<span data-ttu-id="9d1cc-239">İlk satır `@page "{id:int}"` yönergesini içerir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-239">The first line contains the `@page "{id:int}"` directive.</span></span> <span data-ttu-id="9d1cc-240">Yönlendirme kısıtlaması`"{id:int}"`, sayfaya istekleri `int` yönlendirme verileri içeren sayfaya kabul etmesini söyler.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-240">The routing constraint`"{id:int}"` tells the page to accept requests to the page that contain `int` route data.</span></span> <span data-ttu-id="9d1cc-241">Sayfaya yapılan bir istek bir `int`dönüştürülebildiği rota verileri içermiyorsa, çalışma zamanı bir HTTP 404 (bulunamadı) hatası döndürür.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-241">If a request to the page doesn't contain route data that can be converted to an `int`, the runtime returns an HTTP 404 (not found) error.</span></span> <span data-ttu-id="9d1cc-242">KIMLIĞI isteğe bağlı yapmak için `?` yol kısıtlamasına ekleyin:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-242">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="9d1cc-243">*Edit.cshtml.cs* dosyası:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-243">The *Edit.cshtml.cs* file:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Edit.cshtml.cs?name=snippet)]

## <a name="validation"></a><span data-ttu-id="9d1cc-244">Doğrulama</span><span class="sxs-lookup"><span data-stu-id="9d1cc-244">Validation</span></span>

<span data-ttu-id="9d1cc-245">Doğrulama kuralları:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-245">Validation rules:</span></span>

* <span data-ttu-id="9d1cc-246">Model sınıfında bildirimli olarak belirtilir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-246">Are declaratively specified in the model class.</span></span>
* <span data-ttu-id="9d1cc-247">Uygulamada her yerde zorlanır.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-247">Are enforced everywhere in the app.</span></span>

<span data-ttu-id="9d1cc-248"><xref:System.ComponentModel.DataAnnotations> ad alanı, bir sınıfa veya özelliğe bildirimli olarak uygulanan bir yerleşik doğrulama öznitelikleri kümesi sağlar.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-248">The <xref:System.ComponentModel.DataAnnotations> namespace provides a set of built-in validation attributes that are applied declaratively to a class or property.</span></span> <span data-ttu-id="9d1cc-249">Veri açıklamaları, biçimlendirme ile yardım eden [`[DataType]`](xref:System.ComponentModel.DataAnnotations.DataTypeAttribute) gibi biçimlendirme özniteliklerini de içerir ve herhangi bir doğrulama sağlamaz.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-249">DataAnnotations also contains formatting attributes like [`[DataType]`](xref:System.ComponentModel.DataAnnotations.DataTypeAttribute) that help with formatting and don't provide any validation.</span></span>

<span data-ttu-id="9d1cc-250">`Customer` modelini göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-250">Consider the `Customer` model:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Models/Customer.cs)]

<span data-ttu-id="9d1cc-251">Aşağıdaki *Create. cshtml* görünüm dosyasını kullanarak:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-251">Using the following *Create.cshtml* view file:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create3.cshtml?highlight=3,8-9,15-99)]

<span data-ttu-id="9d1cc-252">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-252">The preceding code:</span></span>

* <span data-ttu-id="9d1cc-253">JQuery ve jQuery doğrulama betikleri içerir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-253">Includes jQuery and jQuery validation scripts.</span></span>
* <span data-ttu-id="9d1cc-254">Etkinleştirmek için `<div />` ve `<span />` [etiketi yardımcıları](xref:mvc/views/tag-helpers/intro) kullanır:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-254">Uses the `<div />` and `<span />` [Tag Helpers](xref:mvc/views/tag-helpers/intro) to enable:</span></span>

  * <span data-ttu-id="9d1cc-255">İstemci tarafı doğrulama.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-255">Client-side validation.</span></span>
  * <span data-ttu-id="9d1cc-256">Doğrulama hatası işleme.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-256">Validation error rendering.</span></span>

* <span data-ttu-id="9d1cc-257">Aşağıdaki HTML 'yi oluşturur:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-257">Generates the following HTML:</span></span>

  [!code-html[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create5.html)]

<span data-ttu-id="9d1cc-258">Create formunu ad değeri olmadan göndermek "ad alanı gereklidir" hata iletisini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-258">Posting the Create form without a name value displays the error message "The Name field is required."</span></span> <span data-ttu-id="9d1cc-259">formunda.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-259">on the form.</span></span> <span data-ttu-id="9d1cc-260">İstemcide JavaScript etkinse tarayıcı, sunucuya göndermeden hatayı görüntüler.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-260">If JavaScript is enabled on the client, the browser displays the error without posting to the server.</span></span>

<span data-ttu-id="9d1cc-261">`[StringLength(10)]` özniteliği işlenmiş HTML üzerinde `data-val-length-max="10"` oluşturur.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-261">The `[StringLength(10)]` attribute generates `data-val-length-max="10"` on the rendered HTML.</span></span> <span data-ttu-id="9d1cc-262">`data-val-length-max`, tarayıcıların belirtilen uzunluk üst sınırından daha fazlasını girmesini engeller.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-262">`data-val-length-max` prevents browsers from entering more than the maximum length specified.</span></span> <span data-ttu-id="9d1cc-263">Gönderiyi düzenlemek ve yeniden oynatmak için [Fiddler](https://www.telerik.com/fiddler) gibi bir araç kullanılıyorsa:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-263">If a tool such as [Fiddler](https://www.telerik.com/fiddler) is used to edit and replay the post:</span></span>

* <span data-ttu-id="9d1cc-264">, Adı 10 ' dan daha uzun.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-264">With the name longer than 10.</span></span>
* <span data-ttu-id="9d1cc-265">"Alan adı, en fazla 10 uzunluğunda bir dize olmalıdır" hata iletisi.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-265">The error message "The field Name must be a string with a maximum length of 10."</span></span> <span data-ttu-id="9d1cc-266">döndürülür.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-266">is returned.</span></span>

<span data-ttu-id="9d1cc-267">Aşağıdaki `Movie` modelini göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-267">Consider the following `Movie` model:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Models/MovieDateRatingDA.cs?name=snippet1)]

<span data-ttu-id="9d1cc-268">Doğrulama öznitelikleri, uygulanan model özellikleri üzerinde zorlamak için davranışı belirtir:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-268">The validation attributes specify behavior to enforce on the model properties they're applied to:</span></span>

* <span data-ttu-id="9d1cc-269">`Required` ve `MinimumLength` öznitelikleri bir özelliğin bir değere sahip olması gerektiğini belirtir, ancak hiçbir şey, kullanıcının bu doğrulamayı karşılamak için boşluk girmesini engeller.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-269">The `Required` and `MinimumLength` attributes indicate that a property must have a value, but nothing prevents a user from entering white space to satisfy this validation.</span></span>
* <span data-ttu-id="9d1cc-270">`RegularExpression` özniteliği, hangi karakterlerin giriş yapabileceğini sınırlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-270">The `RegularExpression` attribute is used to limit what characters can be input.</span></span> <span data-ttu-id="9d1cc-271">Yukarıdaki kodda, "tarz":</span><span class="sxs-lookup"><span data-stu-id="9d1cc-271">In the preceding code, "Genre":</span></span>

  * <span data-ttu-id="9d1cc-272">Yalnızca harfler kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-272">Must only use letters.</span></span>
  * <span data-ttu-id="9d1cc-273">İlk harfin büyük harfle olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-273">The first letter is required to be uppercase.</span></span> <span data-ttu-id="9d1cc-274">Boşluk, sayı ve özel karakterlere izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-274">White space, numbers, and special characters are not allowed.</span></span>

* <span data-ttu-id="9d1cc-275">`RegularExpression` "derecelendirmesi":</span><span class="sxs-lookup"><span data-stu-id="9d1cc-275">The `RegularExpression` "Rating":</span></span>

  * <span data-ttu-id="9d1cc-276">İlk karakterin büyük harf olmasını gerektirir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-276">Requires that the first character be an uppercase letter.</span></span>
  * <span data-ttu-id="9d1cc-277">Sonraki boşlukların içindeki özel karakter ve sayılara izin verir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-277">Allows special characters and numbers in subsequent spaces.</span></span> <span data-ttu-id="9d1cc-278">"PG-13" bir derecelendirme için geçerlidir, ancak bir "tarz" için başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-278">"PG-13" is valid for a rating, but fails for a "Genre".</span></span>

* <span data-ttu-id="9d1cc-279">`Range` özniteliği bir değeri belirtilen bir Aralık içinde kısıtlar.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-279">The `Range` attribute constrains a value to within a specified range.</span></span>
* <span data-ttu-id="9d1cc-280">`StringLength` özniteliği, bir dize özelliğinin en büyük uzunluğunu ve isteğe bağlı olarak en düşük uzunluğunu ayarlar.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-280">The `StringLength` attribute sets the maximum length of a string property, and optionally its minimum length.</span></span>
* <span data-ttu-id="9d1cc-281">Değer türleri (örneğin `decimal`, `int`, `float`, `DateTime`), doğal olarak gereklidir ve `[Required]` özniteliğine gerek kalmaz.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-281">Value types (such as `decimal`, `int`, `float`, `DateTime`) are inherently required and don't need the `[Required]` attribute.</span></span>

<span data-ttu-id="9d1cc-282">`Movie` modeli için Oluştur sayfasında, geçersiz değerlere sahip hatalar görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-282">The Create page for the `Movie` model shows displays errors with invalid values:</span></span>

![Birden çok jQuery istemci tarafı doğrulama hatası içeren film görünümü formu](~/tutorials/razor-pages/validation/_static/val.png)

<span data-ttu-id="9d1cc-284">Daha fazla bilgi için bkz.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-284">For more information, see:</span></span>

* [<span data-ttu-id="9d1cc-285">Film uygulamasına doğrulama ekleme</span><span class="sxs-lookup"><span data-stu-id="9d1cc-285">Add validation to the Movie app</span></span>](xref:tutorials/razor-pages/validation)
* <span data-ttu-id="9d1cc-286">[ASP.NET Core 'de model doğrulaması](xref:mvc/models/validation).</span><span class="sxs-lookup"><span data-stu-id="9d1cc-286">[Model validation in ASP.NET Core](xref:mvc/models/validation).</span></span>

## <a name="handle-head-requests-with-an-onget-handler-fallback"></a><span data-ttu-id="9d1cc-287">OnGet işleyicisi geri dönüşü ile tanıtıcı HEAD istekleri</span><span class="sxs-lookup"><span data-stu-id="9d1cc-287">Handle HEAD requests with an OnGet handler fallback</span></span>

<span data-ttu-id="9d1cc-288">`HEAD` istekleri belirli bir kaynağın üst bilgilerini almaya izin verir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-288">`HEAD` requests allow retrieving the headers for a specific resource.</span></span> <span data-ttu-id="9d1cc-289">`GET` isteklerinin aksine `HEAD` istekleri bir yanıt gövdesi döndürmez.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-289">Unlike `GET` requests, `HEAD` requests don't return a response body.</span></span>

<span data-ttu-id="9d1cc-290">Normalde, `HEAD` istekleri için `OnHead` işleyicisi oluşturulur ve çağırılır:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-290">Ordinarily, an `OnHead` handler is created and called for `HEAD` requests:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Privacy.cshtml.cs?name=snippet)]

<span data-ttu-id="9d1cc-291">Razor Pages, `OnHead` işleyicisi tanımlanmamışsa `OnGet` işleyicisini çağırmaya geri döner.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-291">Razor Pages falls back to calling the `OnGet` handler if no `OnHead` handler is defined.</span></span>

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a><span data-ttu-id="9d1cc-292">XSRF/CSRF ve Razor Pages</span><span class="sxs-lookup"><span data-stu-id="9d1cc-292">XSRF/CSRF and Razor Pages</span></span>

<span data-ttu-id="9d1cc-293">Razor Pages, [Antiforgery doğrulaması](xref:security/anti-request-forgery)tarafından korunur.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-293">Razor Pages are protected by [Antiforgery validation](xref:security/anti-request-forgery).</span></span> <span data-ttu-id="9d1cc-294">[Formtaghelper](xref:mvc/views/working-with-forms#the-form-tag-helper) , antiforgery belirteçlerini HTML form öğelerine çıkartır.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-294">The [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) injects antiforgery tokens into HTML form elements.</span></span>

<a name="layout"></a>

## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a><span data-ttu-id="9d1cc-295">Razor Pages ile düzenleri, partileri, şablonları ve etiket yardımcılarını kullanma</span><span class="sxs-lookup"><span data-stu-id="9d1cc-295">Using Layouts, partials, templates, and Tag Helpers with Razor Pages</span></span>

<span data-ttu-id="9d1cc-296">Sayfalar, Razor görünüm altyapısının tüm özellikleri ile çalışır.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-296">Pages work with all the capabilities of the Razor view engine.</span></span> <span data-ttu-id="9d1cc-297">Düzenler, partıals, şablonlar, etiket yardımcıları, *_ViewStart. cshtml*ve *_ViewImports. cshtml* geleneksel Razor görünümlerinde oldukları gibi çalışır.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-297">Layouts, partials, templates, Tag Helpers, *_ViewStart.cshtml*, and *_ViewImports.cshtml* work in the same way they do for conventional Razor views.</span></span>

<span data-ttu-id="9d1cc-298">Bu özelliklerden bazılarının avantajlarından yararlanarak bu sayfayı declutter edelim.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-298">Let's declutter this page by taking advantage of some of those capabilities.</span></span>

<span data-ttu-id="9d1cc-299">*Sayfa/paylaşılan/_Layout. cshtml*'ye bir [Düzen sayfası](xref:mvc/views/layout) ekleyin:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-299">Add a [layout page](xref:mvc/views/layout) to *Pages/Shared/_Layout.cshtml*:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Shared/_Layout2.cshtml?hightlight=12)]

<span data-ttu-id="9d1cc-300">[Düzen](xref:mvc/views/layout):</span><span class="sxs-lookup"><span data-stu-id="9d1cc-300">The [Layout](xref:mvc/views/layout):</span></span>

* <span data-ttu-id="9d1cc-301">Her sayfanın yerleşimini denetler (sayfa düzen dışında değilse).</span><span class="sxs-lookup"><span data-stu-id="9d1cc-301">Controls the layout of each page (unless the page opts out of layout).</span></span>
* <span data-ttu-id="9d1cc-302">JavaScript ve stil sayfaları gibi HTML yapılarını içeri aktarır.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-302">Imports HTML structures such as JavaScript and stylesheets.</span></span>
* <span data-ttu-id="9d1cc-303">Razor sayfasının içerikleri `@RenderBody()` her çağrıldığında işlenir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-303">The contents of the Razor page are rendered where `@RenderBody()` is called.</span></span>

<span data-ttu-id="9d1cc-304">Daha fazla bilgi için bkz. [Düzen sayfası](xref:mvc/views/layout).</span><span class="sxs-lookup"><span data-stu-id="9d1cc-304">For more information, see [layout page](xref:mvc/views/layout).</span></span>

<span data-ttu-id="9d1cc-305">[Layout](xref:mvc/views/layout#specifying-a-layout) özelliği *Pages/_ViewStart. cshtml*' de ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-305">The [Layout](xref:mvc/views/layout#specifying-a-layout) property is set in *Pages/_ViewStart.cshtml*:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]

<span data-ttu-id="9d1cc-306">Düzen *Sayfalar/paylaşılan* klasöründedir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-306">The layout is in the *Pages/Shared* folder.</span></span> <span data-ttu-id="9d1cc-307">Sayfalar, geçerli sayfayla aynı klasörden başlayarak diğer görünümleri (düzenler, şablonlar, parals) hiyerarşik olarak arar.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-307">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="9d1cc-308">*Sayfalar/paylaşılan* klasördeki bir düzen, *Sayfalar* klasörü altındaki herhangi bir Razor sayfasından kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-308">A layout in the *Pages/Shared* folder can be used from any Razor page under the *Pages* folder.</span></span>

<span data-ttu-id="9d1cc-309">Düzen dosyası *Sayfalar/paylaşılan* klasörüne gitmelidir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-309">The layout file should go in the *Pages/Shared* folder.</span></span>

<span data-ttu-id="9d1cc-310">Düzen dosyasını *Görünümler/paylaşılan* klasöre **yerleştirmenizi öneririz** .</span><span class="sxs-lookup"><span data-stu-id="9d1cc-310">We recommend you **not** put the layout file in the *Views/Shared* folder.</span></span> <span data-ttu-id="9d1cc-311">*Görünümler/paylaşılan* bir MVC görünümleri modelidir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-311">*Views/Shared* is an MVC views pattern.</span></span> <span data-ttu-id="9d1cc-312">Razor Pages, yol kurallarını değil klasör hiyerarşisine güvenmektir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-312">Razor Pages are meant to rely on folder hierarchy, not path conventions.</span></span>

<span data-ttu-id="9d1cc-313">Bir Razor sayfasından arama görüntüleme, *Sayfalar* klasörünü içerir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-313">View search from a Razor Page includes the *Pages* folder.</span></span> <span data-ttu-id="9d1cc-314">MVC denetleyicileri ve geleneksel Razor görünümleriyle kullanılan düzenler, şablonlar ve partilar *yalnızca çalışır*.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-314">The layouts, templates, and partials used with MVC controllers and conventional Razor views *just work*.</span></span>

<span data-ttu-id="9d1cc-315">Bir *Pages/_ViewImports. cshtml* dosyası ekleyin:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-315">Add a *Pages/_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]

<span data-ttu-id="9d1cc-316">`@namespace` öğreticide daha sonra açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-316">`@namespace` is explained later in the tutorial.</span></span> <span data-ttu-id="9d1cc-317">`@addTagHelper` yönergesi, [yerleşik etiket yardımcılarını](xref:mvc/views/tag-helpers/builtin-th/Index) *Sayfalar* klasöründeki tüm sayfalara getirir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-317">The `@addTagHelper` directive brings in the [built-in Tag Helpers](xref:mvc/views/tag-helpers/builtin-th/Index) to all the pages in the *Pages* folder.</span></span>

<a name="namespace"></a>

<span data-ttu-id="9d1cc-318">Bir sayfada `@namespace` yönerge kümesi:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-318">The `@namespace` directive set on a page:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]

<span data-ttu-id="9d1cc-319">`@namespace` yönergesi sayfanın ad alanını ayarlar.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-319">The `@namespace` directive sets the namespace for the page.</span></span> <span data-ttu-id="9d1cc-320">`@model` yönergesinin ad alanını içermesi gerekmez.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-320">The `@model` directive doesn't need to include the namespace.</span></span>

<span data-ttu-id="9d1cc-321">`@namespace` yönergesi *_ViewImports. cshtml*içinde yer aldığında, belirtilen ad alanı `@namespace` yönergesini Içeri aktaran sayfada oluşturulan ad alanı için ön ek sağlar.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-321">When the `@namespace` directive is contained in *_ViewImports.cshtml*, the specified namespace supplies the prefix for the generated namespace in the Page that imports the `@namespace` directive.</span></span> <span data-ttu-id="9d1cc-322">Oluşturulan ad alanı (sonek bölümü) geri kalanı, *_ViewImports. cshtml* içeren klasör ve sayfayı içeren klasör arasındaki noktayla ayrılmış göreli yoldur.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-322">The rest of the generated namespace (the suffix portion) is the dot-separated relative path between the folder containing *_ViewImports.cshtml* and the folder containing the page.</span></span>

<span data-ttu-id="9d1cc-323">Örneğin, `PageModel` Class *Pages/Customers/Edit. cshtml. cs* , ad alanını açıkça ayarlar:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-323">For example, the `PageModel` class *Pages/Customers/Edit.cshtml.cs* explicitly sets the namespace:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=snippet_namespace)]

<span data-ttu-id="9d1cc-324">*Pages/_ViewImports. cshtml* dosyası aşağıdaki ad alanını ayarlar:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-324">The *Pages/_ViewImports.cshtml* file sets the following namespace:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]

<span data-ttu-id="9d1cc-325">*Pages/Customers/Edit. cshtml* Razor sayfasının oluşturulan ad alanı `PageModel` sınıfıyla aynıdır.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-325">The generated namespace for the *Pages/Customers/Edit.cshtml* Razor Page is the same as the `PageModel` class.</span></span>

<span data-ttu-id="9d1cc-326">`@namespace` *Ayrıca geleneksel Razor görünümleriyle birlikte kullanılabilir.*</span><span class="sxs-lookup"><span data-stu-id="9d1cc-326">`@namespace` *also works with conventional Razor views.*</span></span>

<span data-ttu-id="9d1cc-327">*Pages/Create. cshtml* görünüm dosyasını göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-327">Consider the *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create3.cshtml?highlight=2-3)]

<span data-ttu-id="9d1cc-328">Güncelleştirilmiş *sayfalar/oluşturma. cshtml* görünüm dosyası *_ViewImports. cshtml* ve önceki düzen dosyası:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-328">The updated *Pages/Create.cshtml* view file with *_ViewImports.cshtml* and the preceding layout file:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create4.cshtml?highlight=2)]

<span data-ttu-id="9d1cc-329">Yukarıdaki kodda *_ViewImports. cshtml* ad alanı ve etiket yardımcıları içeri aktardı.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-329">In the preceding code, the *_ViewImports.cshtml* imported the namespace and Tag Helpers.</span></span> <span data-ttu-id="9d1cc-330">Düzen dosyası JavaScript dosyalarını içeri aktardı.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-330">The layout file imported the JavaScript files.</span></span>

<span data-ttu-id="9d1cc-331">[Razor Pages Starter projesi](#rpvs17) , istemci tarafı doğrulamayı bağlayan *sayfaları/_ValidationScriptsPartial. cshtml*'yi içerir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-331">The [Razor Pages starter project](#rpvs17) contains the *Pages/_ValidationScriptsPartial.cshtml*, which hooks up client-side validation.</span></span>

<span data-ttu-id="9d1cc-332">Kısmi görünümler hakkında daha fazla bilgi için bkz. <xref:mvc/views/partial>.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-332">For more information on partial views, see <xref:mvc/views/partial>.</span></span>

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a><span data-ttu-id="9d1cc-333">Sayfalar için URL oluşturma</span><span class="sxs-lookup"><span data-stu-id="9d1cc-333">URL generation for Pages</span></span>

<span data-ttu-id="9d1cc-334">Daha önce gösterilen `Create` sayfası `RedirectToPage`kullanır:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-334">The `Create` page, shown previously, uses `RedirectToPage`:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml.cs?name=snippet_PageModel&highlight=28)]

<span data-ttu-id="9d1cc-335">Uygulama aşağıdaki dosya/klasör yapısına sahiptir:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-335">The app has the following file/folder structure:</span></span>

* <span data-ttu-id="9d1cc-336">*/Pages*</span><span class="sxs-lookup"><span data-stu-id="9d1cc-336">*/Pages*</span></span>

  * <span data-ttu-id="9d1cc-337">*Index. cshtml*</span><span class="sxs-lookup"><span data-stu-id="9d1cc-337">*Index.cshtml*</span></span>
  * <span data-ttu-id="9d1cc-338">*Gizlilik. cshtml*</span><span class="sxs-lookup"><span data-stu-id="9d1cc-338">*Privacy.cshtml*</span></span>
  * <span data-ttu-id="9d1cc-339">*/Customers*</span><span class="sxs-lookup"><span data-stu-id="9d1cc-339">*/Customers*</span></span>

    * <span data-ttu-id="9d1cc-340">*. Cshtml oluştur*</span><span class="sxs-lookup"><span data-stu-id="9d1cc-340">*Create.cshtml*</span></span>
    * <span data-ttu-id="9d1cc-341">*Edit. cshtml*</span><span class="sxs-lookup"><span data-stu-id="9d1cc-341">*Edit.cshtml*</span></span>
    * <span data-ttu-id="9d1cc-342">*Index. cshtml*</span><span class="sxs-lookup"><span data-stu-id="9d1cc-342">*Index.cshtml*</span></span>

<span data-ttu-id="9d1cc-343">*Pages/Customers/Create. cshtml* ve *Pages/Customers/Edit. cshtml* sayfaları, başarılı olduktan sonra *sayfaları/müşterileri/Index. cshtml* 'ye yeniden yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-343">The *Pages/Customers/Create.cshtml* and *Pages/Customers/Edit.cshtml* pages redirect to *Pages/Customers/Index.cshtml* after success.</span></span> <span data-ttu-id="9d1cc-344">Dize `./Index`, önceki sayfaya erişmek için kullanılan göreli bir sayfa adıdır.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-344">The string `./Index` is a relative page name used to access the preceding page.</span></span> <span data-ttu-id="9d1cc-345">*Pages/Customers/Index. cshtml* sayfasının URL 'leri oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-345">It is used to generate URLs to the *Pages/Customers/Index.cshtml* page.</span></span> <span data-ttu-id="9d1cc-346">Örnek:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-346">For example:</span></span>

* `Url.Page("./Index", ...)`
* `<a asp-page="./Index">Customers Index Page</a>`
* `RedirectToPage("./Index")`

<span data-ttu-id="9d1cc-347">`/Index` mutlak sayfa adı, *Sayfalar/Index. cshtml* sayfasına URL 'ler oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-347">The absolute page name `/Index` is used to generate URLs to the *Pages/Index.cshtml* page.</span></span> <span data-ttu-id="9d1cc-348">Örnek:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-348">For example:</span></span>

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">Home Index Page</a>`
* `RedirectToPage("/Index")`

<span data-ttu-id="9d1cc-349">Sayfa adı, kök */Pages* klasöründeki, önde gelen `/` (örneğin, `/Index`) içeren sayfanın yoludur.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-349">The page name is the path to the page from the root */Pages* folder including a leading `/` (for example, `/Index`).</span></span> <span data-ttu-id="9d1cc-350">Önceki URL oluşturma örnekleri, bir URL 'YI sabit kodlamadan gelişmiş seçenekler ve işlevsel yetenekler sunar.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-350">The preceding URL generation samples offer enhanced options and functional capabilities over hard-coding a URL.</span></span> <span data-ttu-id="9d1cc-351">URL oluşturma [yönlendirme](xref:mvc/controllers/routing) kullanır ve yolun hedef yolda nasıl tanımlandığınıza göre parametreleri oluşturabilir ve kodlayabilir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-351">URL generation uses [routing](xref:mvc/controllers/routing) and can generate and encode parameters according to how the route is defined in the destination path.</span></span>

<span data-ttu-id="9d1cc-352">Sayfalar için URL oluşturma göreli adları destekler.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-352">URL generation for pages supports relative names.</span></span> <span data-ttu-id="9d1cc-353">Aşağıdaki tabloda, *sayfalarda/müşteriler/Create. cshtml*'de farklı `RedirectToPage` parametreleri kullanılarak hangi dizin sayfasının seçildiği gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-353">The following table shows which Index page is selected using different `RedirectToPage` parameters in *Pages/Customers/Create.cshtml*.</span></span>

| <span data-ttu-id="9d1cc-354">RedirectToPage (x)</span><span class="sxs-lookup"><span data-stu-id="9d1cc-354">RedirectToPage(x)</span></span>| <span data-ttu-id="9d1cc-355">Sayfasında</span><span class="sxs-lookup"><span data-stu-id="9d1cc-355">Page</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="9d1cc-356">RedirectToPage ("/Index")</span><span class="sxs-lookup"><span data-stu-id="9d1cc-356">RedirectToPage("/Index")</span></span> | <span data-ttu-id="9d1cc-357">*Sayfa/dizin*</span><span class="sxs-lookup"><span data-stu-id="9d1cc-357">*Pages/Index*</span></span> |
| <span data-ttu-id="9d1cc-358">RedirectToPage ("./Index");</span><span class="sxs-lookup"><span data-stu-id="9d1cc-358">RedirectToPage("./Index");</span></span> | <span data-ttu-id="9d1cc-359">*Sayfalar/müşteriler/Dizin*</span><span class="sxs-lookup"><span data-stu-id="9d1cc-359">*Pages/Customers/Index*</span></span> |
| <span data-ttu-id="9d1cc-360">RedirectToPage (".. /İndex ")</span><span class="sxs-lookup"><span data-stu-id="9d1cc-360">RedirectToPage("../Index")</span></span> | <span data-ttu-id="9d1cc-361">*Sayfa/dizin*</span><span class="sxs-lookup"><span data-stu-id="9d1cc-361">*Pages/Index*</span></span> |
| <span data-ttu-id="9d1cc-362">RedirectToPage ("Dizin")</span><span class="sxs-lookup"><span data-stu-id="9d1cc-362">RedirectToPage("Index")</span></span>  | <span data-ttu-id="9d1cc-363">*Sayfalar/müşteriler/Dizin*</span><span class="sxs-lookup"><span data-stu-id="9d1cc-363">*Pages/Customers/Index*</span></span> |

<!-- Test via ~/razor-pages/index/3.0sample/RazorPagesContacts/Pages/Customers/Details.cshtml.cs -->

<span data-ttu-id="9d1cc-364">`RedirectToPage("Index")`, `RedirectToPage("./Index")`ve `RedirectToPage("../Index")` *göreli adlardır*.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-364">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, and `RedirectToPage("../Index")` are *relative names*.</span></span> <span data-ttu-id="9d1cc-365">`RedirectToPage` parametresi, hedef sayfanın adını hesaplamak için geçerli sayfanın yoluyla *birleştirilir* .</span><span class="sxs-lookup"><span data-stu-id="9d1cc-365">The `RedirectToPage` parameter is *combined* with the path of the current page to compute the name of the destination page.</span></span>

<span data-ttu-id="9d1cc-366">Karmaşık bir yapıya sahip siteler oluştururken göreli ad bağlama yararlı olur.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-366">Relative name linking is useful when building sites with a complex structure.</span></span> <span data-ttu-id="9d1cc-367">Bir klasördeki sayfalar arasında bağlantı için göreli adlar kullanıldığında:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-367">When relative names are used to link between pages in a folder:</span></span>

* <span data-ttu-id="9d1cc-368">Bir klasörü yeniden adlandırmak, göreli bağlantıları bozmaz.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-368">Renaming a folder doesn't break the relative links.</span></span>
* <span data-ttu-id="9d1cc-369">Klasör adını içermediği için bağlantılar kopuk değildir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-369">Links are not broken because they don't include the folder name.</span></span>

<span data-ttu-id="9d1cc-370">Farklı bir [alandaki](xref:mvc/controllers/areas)bir sayfaya yeniden yönlendirmek için alanını belirtin:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-370">To redirect to a page in a different [Area](xref:mvc/controllers/areas), specify the area:</span></span>

```csharp
RedirectToPage("/Index", new { area = "Services" });
```

<span data-ttu-id="9d1cc-371">Daha fazla bilgi için <xref:mvc/controllers/areas> ve <xref:razor-pages/razor-pages-conventions> bölümlerine bakın.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-371">For more information, see <xref:mvc/controllers/areas> and <xref:razor-pages/razor-pages-conventions>.</span></span>

## <a name="viewdata-attribute"></a><span data-ttu-id="9d1cc-372">ViewData özniteliği</span><span class="sxs-lookup"><span data-stu-id="9d1cc-372">ViewData attribute</span></span>

<span data-ttu-id="9d1cc-373">Veriler, <xref:Microsoft.AspNetCore.Mvc.ViewDataAttribute>bir sayfaya geçirilebilir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-373">Data can be passed to a page with <xref:Microsoft.AspNetCore.Mvc.ViewDataAttribute>.</span></span> <span data-ttu-id="9d1cc-374">`[ViewData]` özniteliğiyle birlikte bulunan özellikler, <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.ViewDataDictionary>değerlerinin depolandığı ve yüklendiği değerlerdir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-374">Properties with the `[ViewData]` attribute have their values stored and loaded from the <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.ViewDataDictionary>.</span></span>

<span data-ttu-id="9d1cc-375">Aşağıdaki örnekte `AboutModel`, `Title` özelliğine `[ViewData]` özniteliğini uygular:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-375">In the following example, the `AboutModel` applies the `[ViewData]` attribute to the `Title` property:</span></span>

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

<span data-ttu-id="9d1cc-376">Hakkında sayfasında, `Title` özelliğine model özelliği olarak erişin:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-376">In the About page, access the `Title` property as a model property:</span></span>

```cshtml
<h1>@Model.Title</h1>
```

<span data-ttu-id="9d1cc-377">Mizanpajda, başlık ViewData sözlüğünden okundu:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-377">In the layout, the title is read from the ViewData dictionary:</span></span>

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"] - WebApplication</title>
    ...
```

## <a name="tempdata"></a><span data-ttu-id="9d1cc-378">TempData</span><span class="sxs-lookup"><span data-stu-id="9d1cc-378">TempData</span></span>

<span data-ttu-id="9d1cc-379">ASP.NET Core <xref:Microsoft.AspNetCore.Mvc.Controller.TempData>kullanıma sunar.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-379">ASP.NET Core exposes the <xref:Microsoft.AspNetCore.Mvc.Controller.TempData>.</span></span> <span data-ttu-id="9d1cc-380">Bu özellik, okunana kadar verileri depolar.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-380">This property stores data until it's read.</span></span> <span data-ttu-id="9d1cc-381"><xref:Microsoft.AspNetCore.Mvc.ViewFeatures.TempDataDictionary.Keep*> ve <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.TempDataDictionary.Peek*> yöntemleri silinmeden verileri incelemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-381">The <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.TempDataDictionary.Keep*> and <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.TempDataDictionary.Peek*> methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="9d1cc-382">`TempData`, bir tek istekten daha fazla veri gerektiğinde yeniden yönlendirme için kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-382">`TempData` is useful for redirection, when data is needed for more than a single request.</span></span>

<span data-ttu-id="9d1cc-383">Aşağıdaki kod, `TempData`kullanarak `Message` değerini ayarlar:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-383">The following code sets the value of `Message` using `TempData`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,25&name=snippet_Temp)]

<span data-ttu-id="9d1cc-384">*Pages/Customers/Index. cshtml* dosyasında aşağıdaki biçimlendirme `TempData`kullanarak `Message` değerini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-384">The following markup in the *Pages/Customers/Index.cshtml* file displays the value of `Message` using `TempData`.</span></span>

```cshtml
<h3>Msg: @Model.Message</h3>
```

<span data-ttu-id="9d1cc-385">*Pages/Customers/Index. cshtml. cs* sayfa modeli, `[TempData]` özniteliğini `Message` özelliğine uygular.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-385">The *Pages/Customers/Index.cshtml.cs* page model applies the `[TempData]` attribute to the `Message` property.</span></span>

```csharp
[TempData]
public string Message { get; set; }
```

<span data-ttu-id="9d1cc-386">Daha fazla bilgi için bkz. [TempData](xref:fundamentals/app-state#tempdata).</span><span class="sxs-lookup"><span data-stu-id="9d1cc-386">For more information, see [TempData](xref:fundamentals/app-state#tempdata).</span></span>

<a name="mhpp"></a>

## <a name="multiple-handlers-per-page"></a><span data-ttu-id="9d1cc-387">Sayfa başına birden çok işleyici</span><span class="sxs-lookup"><span data-stu-id="9d1cc-387">Multiple handlers per page</span></span>

<span data-ttu-id="9d1cc-388">Aşağıdaki sayfa `asp-page-handler` etiketi Yardımcısını kullanarak iki işleyici için biçimlendirme oluşturur:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-388">The following page generates markup for two handlers using the `asp-page-handler` Tag Helper:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

<span data-ttu-id="9d1cc-389">Yukarıdaki örnekteki formda, her biri farklı bir URL 'ye göndermek için `FormActionTagHelper` kullanan iki gönderme düğmesi vardır.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-389">The form in the preceding example has two submit buttons, each using the `FormActionTagHelper` to submit to a different URL.</span></span> <span data-ttu-id="9d1cc-390">`asp-page-handler` özniteliği `asp-page`bir yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-390">The `asp-page-handler` attribute is a companion to `asp-page`.</span></span> <span data-ttu-id="9d1cc-391">`asp-page-handler`, bir sayfa tarafından tanımlanan her bir işleyici yöntemini gönderen URL 'Ler oluşturur.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-391">`asp-page-handler` generates URLs that submit to each of the handler methods defined by a page.</span></span> <span data-ttu-id="9d1cc-392">örnek geçerli sayfaya bağlandığından `asp-page` belirtilmedi.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-392">`asp-page` isn't specified because the sample is linking to the current page.</span></span>

<span data-ttu-id="9d1cc-393">Sayfa modeli:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-393">The page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]

<span data-ttu-id="9d1cc-394">Yukarıdaki kod, *adlandırılmış işleyici yöntemlerini*kullanır.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-394">The preceding code uses *named handler methods*.</span></span> <span data-ttu-id="9d1cc-395">Adlandırılmış işleyici yöntemleri, `On<HTTP Verb>` sonra ve `Async` önce (varsa), ad içindeki metin alınarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-395">Named handler methods are created by taking the text in the name after `On<HTTP Verb>` and before `Async` (if present).</span></span> <span data-ttu-id="9d1cc-396">Yukarıdaki örnekte, Page metotları OnPost**Joinlist**Async ve onpost**Joinlıstuc**Async ' dir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-396">In the preceding example, the page methods are OnPost**JoinList**Async and OnPost**JoinListUC**Async.</span></span> <span data-ttu-id="9d1cc-397">*Onpost* Ile *zaman uyumsuz* olarak kaldırıldığında, işleyici adları `JoinList` ve `JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-397">With *OnPost* and *Async* removed, the handler names are `JoinList` and `JoinListUC`.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?range=12-13)]

<span data-ttu-id="9d1cc-398">Yukarıdaki kodu kullanarak, `OnPostJoinListAsync` ' a gönderen URL yolu `https://localhost:5001/Customers/CreateFATH?handler=JoinList`.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-398">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `https://localhost:5001/Customers/CreateFATH?handler=JoinList`.</span></span> <span data-ttu-id="9d1cc-399">`OnPostJoinListUCAsync` ' a gönderen URL yolu `https://localhost:5001/Customers/CreateFATH?handler=JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-399">The URL path that submits to `OnPostJoinListUCAsync` is `https://localhost:5001/Customers/CreateFATH?handler=JoinListUC`.</span></span>

## <a name="custom-routes"></a><span data-ttu-id="9d1cc-400">Özel yollar</span><span class="sxs-lookup"><span data-stu-id="9d1cc-400">Custom routes</span></span>

<span data-ttu-id="9d1cc-401">`@page` yönergesini kullanarak şunları yapın:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-401">Use the `@page` directive to:</span></span>

* <span data-ttu-id="9d1cc-402">Sayfaya özel bir yol belirtin.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-402">Specify a custom route to a page.</span></span> <span data-ttu-id="9d1cc-403">Örneğin, hakkında sayfasına olan yol `@page "/Some/Other/Path"``/Some/Other/Path` olarak ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-403">For example, the route to the About page can be set to `/Some/Other/Path` with `@page "/Some/Other/Path"`.</span></span>
* <span data-ttu-id="9d1cc-404">Kesimleri bir sayfanın varsayılan yoluna ekleyin.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-404">Append segments to a page's default route.</span></span> <span data-ttu-id="9d1cc-405">Örneğin, bir "öğe" segmenti sayfanın varsayılan yoluna `@page "item"`eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-405">For example, an "item" segment can be added to a page's default route with `@page "item"`.</span></span>
* <span data-ttu-id="9d1cc-406">Bir sayfanın varsayılan yoluna parametreleri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-406">Append parameters to a page's default route.</span></span> <span data-ttu-id="9d1cc-407">Örneğin, `@page "{id}"`bir sayfa için `id`bir ID parametresi gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-407">For example, an ID parameter, `id`, can be required for a page with `@page "{id}"`.</span></span>

<span data-ttu-id="9d1cc-408">Yolun başındaki bir tilde (`~`) tarafından atanan kök göreli bir yol desteklenir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-408">A root-relative path designated by a tilde (`~`) at the beginning of the path is supported.</span></span> <span data-ttu-id="9d1cc-409">Örneğin, `@page "~/Some/Other/Path"` `@page "/Some/Other/Path"`aynıdır.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-409">For example, `@page "~/Some/Other/Path"` is the same as `@page "/Some/Other/Path"`.</span></span>

<span data-ttu-id="9d1cc-410">Sorgu dizesini URL 'de `?handler=JoinList` beğenmezseniz, URL 'nin yol bölümüne işleyici adını koymak için yolu değiştirin.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-410">If you don't like the query string `?handler=JoinList` in the URL, change the route to put the handler name in the path portion of the URL.</span></span> <span data-ttu-id="9d1cc-411">Yol, `@page` yönergesinin ardından çift tırnak içine alınmış bir rota şablonu eklenerek özelleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-411">The route can be customized by adding a route template enclosed in double quotes after the `@page` directive.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]

<span data-ttu-id="9d1cc-412">Yukarıdaki kodu kullanarak, `OnPostJoinListAsync` ' a gönderen URL yolu `https://localhost:5001/Customers/CreateFATH/JoinList`.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-412">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `https://localhost:5001/Customers/CreateFATH/JoinList`.</span></span> <span data-ttu-id="9d1cc-413">`OnPostJoinListUCAsync` ' a gönderen URL yolu `https://localhost:5001/Customers/CreateFATH/JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-413">The URL path that submits to `OnPostJoinListUCAsync` is `https://localhost:5001/Customers/CreateFATH/JoinListUC`.</span></span>

<span data-ttu-id="9d1cc-414">Aşağıdaki `handler` `?` yol parametresinin isteğe bağlı olduğu anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-414">The `?` following `handler` means the route parameter is optional.</span></span>

## <a name="advanced-configuration-and-settings"></a><span data-ttu-id="9d1cc-415">Gelişmiş yapılandırma ve ayarlar</span><span class="sxs-lookup"><span data-stu-id="9d1cc-415">Advanced configuration and settings</span></span>

<span data-ttu-id="9d1cc-416">Aşağıdaki bölümlerdeki yapılandırma ve ayarlar çoğu uygulama için gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-416">The configuration and settings in following sections is not required by most apps.</span></span>

<span data-ttu-id="9d1cc-417">Gelişmiş seçenekleri yapılandırmak için <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>genişletme yöntemini kullanın:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-417">To configure advanced options, use the extension method <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/StartupRPoptions.cs?name=snippet)]

<span data-ttu-id="9d1cc-418">Sayfaların kök dizinini ayarlamak için <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions> kullanın veya sayfalar için uygulama modeli kuralları ekleyin.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-418">Use the <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions> to set the root directory for pages, or add application model conventions for pages.</span></span> <span data-ttu-id="9d1cc-419">Kurallar hakkında daha fazla bilgi için bkz. [Razor Pages yetkilendirme kuralları](xref:security/authorization/razor-pages-authorization).</span><span class="sxs-lookup"><span data-stu-id="9d1cc-419">For more information on conventions, see [Razor Pages authorization conventions](xref:security/authorization/razor-pages-authorization).</span></span>

<span data-ttu-id="9d1cc-420">Görünümleri önceden derlemek için bkz. [Razor görünüm derlemesi](xref:mvc/views/view-compilation).</span><span class="sxs-lookup"><span data-stu-id="9d1cc-420">To precompile views, see [Razor view compilation](xref:mvc/views/view-compilation).</span></span>

### <a name="specify-that-razor-pages-are-at-the-content-root"></a><span data-ttu-id="9d1cc-421">Razor Pages içerik kökünde olduğunu belirtin</span><span class="sxs-lookup"><span data-stu-id="9d1cc-421">Specify that Razor Pages are at the content root</span></span>

<span data-ttu-id="9d1cc-422">Varsayılan olarak, Razor Pages */Pages* dizininde kök olarak depolanır.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-422">By default, Razor Pages are rooted in the */Pages* directory.</span></span> <span data-ttu-id="9d1cc-423">Razor Pages uygulamanın (<xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootPath>) [içerik kökünde](xref:fundamentals/index#content-root) olduğunu belirtmek için <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.WithRazorPagesAtContentRoot*> ekleyin:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-423">Add <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.WithRazorPagesAtContentRoot*> to specify that your Razor Pages are at the [content root](xref:fundamentals/index#content-root) (<xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootPath>) of the app:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/StartupWithRazorPagesAtContentRoot.cs?name=snippet)]

### <a name="specify-that-razor-pages-are-at-a-custom-root-directory"></a><span data-ttu-id="9d1cc-424">Razor Pages özel kök dizinde olduğunu belirtin</span><span class="sxs-lookup"><span data-stu-id="9d1cc-424">Specify that Razor Pages are at a custom root directory</span></span>

<span data-ttu-id="9d1cc-425">Razor Pages uygulamada bir özel kök dizinde olduğunu belirtmek için <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcCoreBuilderExtensions.WithRazorPagesRoot*> ekleyin (göreli bir yol sağlayın):</span><span class="sxs-lookup"><span data-stu-id="9d1cc-425">Add <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcCoreBuilderExtensions.WithRazorPagesRoot*> to specify that Razor Pages are at a custom root directory in the app (provide a relative path):</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/StartupWithRazorPagesRoot.cs?name=snippet)]

## <a name="additional-resources"></a><span data-ttu-id="9d1cc-426">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="9d1cc-426">Additional resources</span></span>

* <span data-ttu-id="9d1cc-427">Bkz. [Razor Pages kullanmaya başlama](xref:tutorials/razor-pages/razor-pages-start), bu giriş hakkında derleme</span><span class="sxs-lookup"><span data-stu-id="9d1cc-427">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start), which builds on this introduction</span></span>
* [<span data-ttu-id="9d1cc-428">Örnek kodu indirme veya görüntüleme</span><span class="sxs-lookup"><span data-stu-id="9d1cc-428">Download or view sample code</span></span>](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/index/3.0sample)
* <xref:index>
* <xref:mvc/views/razor>
* <xref:mvc/controllers/areas>
* <xref:tutorials/razor-pages/razor-pages-start>
* <xref:security/authorization/razor-pages-authorization>
* <xref:razor-pages/razor-pages-conventions>
* <xref:test/razor-pages-tests>
* <xref:mvc/views/partial>
* <xref:blazor/integrate-components>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="9d1cc-429">By [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Ryan şimdi ak](https://github.com/rynowak)</span><span class="sxs-lookup"><span data-stu-id="9d1cc-429">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Ryan Nowak](https://github.com/rynowak)</span></span>

<span data-ttu-id="9d1cc-430">Razor Pages, kod odaklı senaryoları daha kolay ve daha üretken hale getiren ASP.NET Core MVC 'nin yeni bir yönüdür.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-430">Razor Pages is a new aspect of ASP.NET Core MVC that makes coding page-focused scenarios easier and more productive.</span></span>

<span data-ttu-id="9d1cc-431">Model-View-Controller yaklaşımını kullanan bir öğretici arıyorsanız, bkz. [ASP.NET Core MVC ile çalışmaya başlama](xref:tutorials/first-mvc-app/start-mvc).</span><span class="sxs-lookup"><span data-stu-id="9d1cc-431">If you're looking for a tutorial that uses the Model-View-Controller approach, see [Get started with ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span></span>

<span data-ttu-id="9d1cc-432">Bu belge Razor Pages bir giriş sağlar.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-432">This document provides an introduction to Razor Pages.</span></span> <span data-ttu-id="9d1cc-433">Adım adım öğretici değildir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-433">It's not a step by step tutorial.</span></span> <span data-ttu-id="9d1cc-434">Bölümlerden bazılarını çok gelişmiş bir şekilde bulursanız, bkz. [Razor Pages kullanmaya başlama](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="9d1cc-434">If you find some of the sections too advanced, see [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span></span> <span data-ttu-id="9d1cc-435">ASP.NET Core genel bir bakış için bkz. [ASP.NET Core giriş](xref:index).</span><span class="sxs-lookup"><span data-stu-id="9d1cc-435">For an overview of ASP.NET Core, see the [Introduction to ASP.NET Core](xref:index).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9d1cc-436">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="9d1cc-436">Prerequisites</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="9d1cc-437">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9d1cc-437">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="9d1cc-438">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9d1cc-438">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="9d1cc-439">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9d1cc-439">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

<a name="rpvs17"></a>

## <a name="create-a-razor-pages-project"></a><span data-ttu-id="9d1cc-440">Razor Pages projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="9d1cc-440">Create a Razor Pages project</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="9d1cc-441">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9d1cc-441">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="9d1cc-442">Razor Pages projesi oluşturma hakkında ayrıntılı yönergeler için bkz. [Razor Pages kullanmaya başlama](xref:tutorials/razor-pages/razor-pages-start) .</span><span class="sxs-lookup"><span data-stu-id="9d1cc-442">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) for detailed instructions on how to create a Razor Pages project.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="9d1cc-443">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9d1cc-443">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="9d1cc-444">Komut satırından `dotnet new webapp` çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-444">Run `dotnet new webapp` from the command line.</span></span>

<span data-ttu-id="9d1cc-445">Oluşturulan *. csproj* dosyasını Mac için Visual Studio açın.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-445">Open the generated *.csproj* file from Visual Studio for Mac.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="9d1cc-446">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9d1cc-446">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="9d1cc-447">Komut satırından `dotnet new webapp` çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-447">Run `dotnet new webapp` from the command line.</span></span>

---

## <a name="razor-pages"></a><span data-ttu-id="9d1cc-448">Razor Pages</span><span class="sxs-lookup"><span data-stu-id="9d1cc-448">Razor Pages</span></span>

<span data-ttu-id="9d1cc-449">Razor Pages, *Startup.cs*'de etkinleştirilmiştir:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-449">Razor Pages is enabled in *Startup.cs*:</span></span>

[!code-cs[](index/sample/RazorPagesIntro/Startup.cs?name=snippet_Startup)]

<span data-ttu-id="9d1cc-450">Temel bir sayfa düşünün:<a name="OnGet"></a></span><span class="sxs-lookup"><span data-stu-id="9d1cc-450">Consider a basic page: <a name="OnGet"></a></span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index.cshtml)]

<span data-ttu-id="9d1cc-451">Yukarıdaki kod, denetleyiciler ve görünümlerle ASP.NET Core bir uygulamada kullanılan [Razor görünüm dosyası](xref:tutorials/first-mvc-app/adding-view) gibi bir çok şey arar.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-451">The preceding code looks a lot like a [Razor view file](xref:tutorials/first-mvc-app/adding-view) used in an ASP.NET Core app with controllers and views.</span></span> <span data-ttu-id="9d1cc-452">Bu, farklı kılan `@page` yönergedir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-452">What makes it different is the `@page` directive.</span></span> <span data-ttu-id="9d1cc-453">`@page`, dosyayı bir denetleyiciye geçmeden doğrudan istekleri işlediği anlamına gelen bir MVC eylemine sahip olur.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-453">`@page` makes the file into an MVC action - which means that it handles requests directly, without going through a controller.</span></span> <span data-ttu-id="9d1cc-454">`@page` sayfadaki ilk Razor yönergesi olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-454">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="9d1cc-455">`@page` diğer Razor yapıları davranışını etkiler.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-455">`@page` affects the behavior of other Razor constructs.</span></span>

<span data-ttu-id="9d1cc-456">`PageModel` sınıfı kullanan benzer bir sayfa aşağıdaki iki dosyada gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-456">A similar page, using a `PageModel` class, is shown in the following two files.</span></span> <span data-ttu-id="9d1cc-457">*Pages/Index2. cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-457">The *Pages/Index2.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index2.cshtml)]

<span data-ttu-id="9d1cc-458">*Pages/Index2. cshtml. cs* sayfa modeli:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-458">The *Pages/Index2.cshtml.cs* page model:</span></span>

[!code-cs[](index/sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]

<span data-ttu-id="9d1cc-459">Kurala göre `PageModel` sınıf dosyası, *. cs* eklenmiş Razor sayfası dosyasıyla aynı ada sahiptir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-459">By convention, the `PageModel` class file has the same name as the Razor Page file with *.cs* appended.</span></span> <span data-ttu-id="9d1cc-460">Örneğin, önceki Razor sayfası *Pages/Index2. cshtml*' dir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-460">For example, the previous Razor Page is *Pages/Index2.cshtml*.</span></span> <span data-ttu-id="9d1cc-461">`PageModel` sınıfını içeren dosya *sayfa/Index2. cshtml. cs*olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-461">The file containing the `PageModel` class is named *Pages/Index2.cshtml.cs*.</span></span>

<span data-ttu-id="9d1cc-462">URL yollarının sayfalara olan ilişkilendirmeleri, sayfanın dosya sistemindeki konumuna göre belirlenir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-462">The associations of URL paths to pages are determined by the page's location in the file system.</span></span> <span data-ttu-id="9d1cc-463">Aşağıdaki tabloda bir Razor sayfa yolu ve eşleşen URL gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-463">The following table shows a Razor Page path and the matching URL:</span></span>

| <span data-ttu-id="9d1cc-464">Dosya adı ve yolu</span><span class="sxs-lookup"><span data-stu-id="9d1cc-464">File name and path</span></span>               | <span data-ttu-id="9d1cc-465">eşleşen URL</span><span class="sxs-lookup"><span data-stu-id="9d1cc-465">matching URL</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="9d1cc-466">*/Pages/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="9d1cc-466">*/Pages/Index.cshtml*</span></span> | <span data-ttu-id="9d1cc-467">`/` veya `/Index`</span><span class="sxs-lookup"><span data-stu-id="9d1cc-467">`/` or `/Index`</span></span> |
| <span data-ttu-id="9d1cc-468">*/Pages/Contact.exe*</span><span class="sxs-lookup"><span data-stu-id="9d1cc-468">*/Pages/Contact.cshtml*</span></span> | `/Contact` |
| <span data-ttu-id="9d1cc-469">*/Pages/Store/Contact.exe*</span><span class="sxs-lookup"><span data-stu-id="9d1cc-469">*/Pages/Store/Contact.cshtml*</span></span> | `/Store/Contact` |
| <span data-ttu-id="9d1cc-470">*/Pages/Store/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="9d1cc-470">*/Pages/Store/Index.cshtml*</span></span> | <span data-ttu-id="9d1cc-471">`/Store` veya `/Store/Index`</span><span class="sxs-lookup"><span data-stu-id="9d1cc-471">`/Store` or `/Store/Index`</span></span> |

<span data-ttu-id="9d1cc-472">Notlar:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-472">Notes:</span></span>

* <span data-ttu-id="9d1cc-473">Çalışma zamanı, *Sayfalar* klasöründeki Razor Pages dosyaları varsayılan olarak arar.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-473">The runtime looks for Razor Pages files in the *Pages* folder by default.</span></span>
* <span data-ttu-id="9d1cc-474">`Index`, URL bir sayfa içermiyorsa varsayılan sayfasıdır.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-474">`Index` is the default page when a URL doesn't include a page.</span></span>

## <a name="write-a-basic-form"></a><span data-ttu-id="9d1cc-475">Temel form yazma</span><span class="sxs-lookup"><span data-stu-id="9d1cc-475">Write a basic form</span></span>

<span data-ttu-id="9d1cc-476">Razor Pages, Web tarayıcıları ile kullanılan ortak desenleri bir uygulama oluştururken kolayca uygulanması için tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-476">Razor Pages is designed to make common patterns used with web browsers easy to implement when building an app.</span></span> <span data-ttu-id="9d1cc-477">[Model bağlama](xref:mvc/models/model-binding), [ETIKET yardımcıları](xref:mvc/views/tag-helpers/intro)ve HTML Yardımcıları hepsi, Razor sayfası sınıfında tanımlanan özelliklerle *çalışır* .</span><span class="sxs-lookup"><span data-stu-id="9d1cc-477">[Model binding](xref:mvc/models/model-binding), [Tag Helpers](xref:mvc/views/tag-helpers/intro), and HTML helpers all *just work* with the properties defined in a Razor Page class.</span></span> <span data-ttu-id="9d1cc-478">`Contact` modeli için temel bir "bize başvurun" formu uygulayan bir sayfa düşünün:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-478">Consider a page that implements a basic "contact us" form for the `Contact` model:</span></span>

<span data-ttu-id="9d1cc-479">Bu belgedeki örnekler için `DbContext`, [Startup.cs](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) dosyasında başlatılır.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-479">For the samples in this document, the `DbContext` is initialized in the [Startup.cs](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) file.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Startup.cs?highlight=15-16)]

<span data-ttu-id="9d1cc-480">Veri modeli:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-480">The data model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Data/Customer.cs)]

<span data-ttu-id="9d1cc-481">DB bağlamı:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-481">The db context:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Data/AppDbContext.cs)]

<span data-ttu-id="9d1cc-482">*Pages/Create. cshtml* görünüm dosyası:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-482">The *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml)]

<span data-ttu-id="9d1cc-483">*Pages/Create. cshtml. cs* sayfa modeli:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-483">The *Pages/Create.cshtml.cs* page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_ALL)]

<span data-ttu-id="9d1cc-484">Kurala göre `PageModel` sınıfı `<PageName>Model` olarak adlandırılır ve sayfayla aynı ad alanında yer alan.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-484">By convention, the `PageModel` class is called `<PageName>Model` and is in the same namespace as the page.</span></span>

<span data-ttu-id="9d1cc-485">`PageModel` sınıfı, bir sayfa mantığının sunumundaki ayırmayı sağlar.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-485">The `PageModel` class allows separation of the logic of a page from its presentation.</span></span> <span data-ttu-id="9d1cc-486">Sayfaya gönderilen istekler için sayfa işleyicilerini ve sayfayı işlemek için kullanılan verileri tanımlar.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-486">It defines page handlers for requests sent to the page and the data used to render the page.</span></span> <span data-ttu-id="9d1cc-487">Bu ayrım şunları sağlar:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-487">This separation allows:</span></span>

* <span data-ttu-id="9d1cc-488">[Bağımlılık ekleme](xref:fundamentals/dependency-injection)yoluyla sayfa bağımlılıklarını yönetme.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-488">Managing of page dependencies through [dependency injection](xref:fundamentals/dependency-injection).</span></span>
* <span data-ttu-id="9d1cc-489">Sayfaların [birim testi](xref:test/razor-pages-tests) .</span><span class="sxs-lookup"><span data-stu-id="9d1cc-489">[Unit testing](xref:test/razor-pages-tests) the pages.</span></span>

<span data-ttu-id="9d1cc-490">Sayfada, `POST` isteklerinde çalışan bir `OnPostAsync` *işleyicisi yöntemi*vardır (bir Kullanıcı formu gönderdiğinde).</span><span class="sxs-lookup"><span data-stu-id="9d1cc-490">The page has an `OnPostAsync` *handler method*, which runs on `POST` requests (when a user posts the form).</span></span> <span data-ttu-id="9d1cc-491">Herhangi bir HTTP fiili için işleyici yöntemleri ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-491">You can add handler methods for any HTTP verb.</span></span> <span data-ttu-id="9d1cc-492">En yaygın işleyiciler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-492">The most common handlers are:</span></span>

* <span data-ttu-id="9d1cc-493">Sayfanın başlatılması için gereken durum `OnGet`.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-493">`OnGet` to initialize state needed for the page.</span></span> <span data-ttu-id="9d1cc-494">[OnGet](#OnGet) örneği.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-494">[OnGet](#OnGet) sample.</span></span>
* <span data-ttu-id="9d1cc-495">form gönderilerini işlemek için `OnPost`.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-495">`OnPost` to handle form submissions.</span></span>

<span data-ttu-id="9d1cc-496">`Async` adlandırma son eki isteğe bağlıdır, ancak genellikle zaman uyumsuz işlevler için kural tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-496">The `Async` naming suffix is optional but is often used by convention for asynchronous functions.</span></span> <span data-ttu-id="9d1cc-497">Yukarıdaki kod Razor Pages için tipik bir davranıştır.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-497">The preceding code is typical for Razor Pages.</span></span>

<span data-ttu-id="9d1cc-498">Denetleyicileri ve görünümleri kullanarak ASP.NET uygulamaları hakkında bilginiz varsa:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-498">If you're familiar with ASP.NET apps using controllers and views:</span></span>

* <span data-ttu-id="9d1cc-499">Yukarıdaki örnekteki `OnPostAsync` kodu, tipik denetleyici koduna benzer şekilde görünür.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-499">The `OnPostAsync` code in the preceding example looks similar to typical controller code.</span></span>
* <span data-ttu-id="9d1cc-500">[Model bağlama](xref:mvc/models/model-binding), [doğrulama](xref:mvc/models/validation), [doğrulama](xref:mvc/models/validation)ve eylem sonuçları gibi mvc temel elemanlarının çoğu paylaşılır.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-500">Most of the MVC primitives like [model binding](xref:mvc/models/model-binding), [validation](xref:mvc/models/validation), [Validation](xref:mvc/models/validation),  and action results are shared.</span></span>

<span data-ttu-id="9d1cc-501">Önceki `OnPostAsync` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-501">The previous `OnPostAsync` method:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="9d1cc-502">`OnPostAsync`temel akışı:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-502">The basic flow of `OnPostAsync`:</span></span>

<span data-ttu-id="9d1cc-503">Doğrulama hatalarını kontrol edin.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-503">Check for validation errors.</span></span>

* <span data-ttu-id="9d1cc-504">Hata yoksa, verileri kaydedin ve yeniden yönlendirin.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-504">If there are no errors, save the data and redirect.</span></span>
* <span data-ttu-id="9d1cc-505">Hatalar varsa, doğrulama iletileriyle sayfayı yeniden görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-505">If there are errors, show the page again with validation messages.</span></span> <span data-ttu-id="9d1cc-506">İstemci tarafı doğrulaması geleneksel ASP.NET Core MVC uygulamalarıyla aynıdır.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-506">Client-side validation is identical to traditional ASP.NET Core MVC applications.</span></span> <span data-ttu-id="9d1cc-507">Çoğu durumda, istemci üzerinde doğrulama hataları algılanır ve sunucuya hiçbir zaman gönderilmez.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-507">In many cases, validation errors would be detected on the client, and never submitted to the server.</span></span>

<span data-ttu-id="9d1cc-508">Veriler başarıyla girildiğinde, `OnPostAsync` Handler yöntemi bir `RedirectToPageResult`örneğini döndürmek için `RedirectToPage` yardımcı yöntemini çağırır.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-508">When the data is entered successfully, the `OnPostAsync` handler method calls the `RedirectToPage` helper method to return an instance of `RedirectToPageResult`.</span></span> <span data-ttu-id="9d1cc-509">`RedirectToPage`, `RedirectToAction` veya `RedirectToRoute`benzer ancak sayfalar için özelleştirilen yeni bir eylem sonucudur.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-509">`RedirectToPage` is a new action result, similar to `RedirectToAction` or `RedirectToRoute`, but customized for pages.</span></span> <span data-ttu-id="9d1cc-510">Önceki örnekte, kök dizin sayfasına (`/Index`) yeniden yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-510">In the preceding sample, it redirects to the root Index page (`/Index`).</span></span> <span data-ttu-id="9d1cc-511">`RedirectToPage`, [Sayfalar Için URL oluşturma](#url_gen) bölümünde ayrıntılı olarak açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-511">`RedirectToPage` is detailed in the [URL generation for Pages](#url_gen) section.</span></span>

<span data-ttu-id="9d1cc-512">Gönderilen formda doğrulama hataları olduğunda (sunucuya geçirilen)`OnPostAsync` işleyicisi yöntemi `Page` yardımcı yöntemini çağırır.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-512">When the submitted form has validation errors (that are passed to the server), the`OnPostAsync` handler method calls the `Page` helper method.</span></span> <span data-ttu-id="9d1cc-513">`Page`, `PageResult`örneğini döndürür.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-513">`Page` returns an instance of `PageResult`.</span></span> <span data-ttu-id="9d1cc-514">`Page` döndürmek, denetleyicilerde eylemlerin `View`nasıl dönüşlerine benzer.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-514">Returning `Page` is similar to how actions in controllers return `View`.</span></span> <span data-ttu-id="9d1cc-515">`PageResult`, bir işleyici yöntemi için varsayılan dönüş türüdür.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-515">`PageResult` is the default return type for a handler method.</span></span> <span data-ttu-id="9d1cc-516">`void` döndüren bir işleyici yöntemi sayfayı işler.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-516">A handler method that returns `void` renders the page.</span></span>

<span data-ttu-id="9d1cc-517">`Customer` özelliği, model bağlamasını kabul etmek için `[BindProperty]` özniteliğini kullanır.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-517">The `Customer` property uses `[BindProperty]` attribute to opt in to model binding.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_PageModel&highlight=10-11)]

<span data-ttu-id="9d1cc-518">Razor Pages, varsayılan olarak, özellikleri yalnızca`GET` olmayan fiiller ile bağlayın.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-518">Razor Pages, by default, bind properties only with non-`GET` verbs.</span></span> <span data-ttu-id="9d1cc-519">Özelliklere bağlama, yazmanız gerektiğini kodun miktarını azaltabilir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-519">Binding to properties can reduce the amount of code you have to write.</span></span> <span data-ttu-id="9d1cc-520">Bağlama, form alanlarını işlemek için aynı özelliği kullanarak kodu azaltır (`<input asp-for="Customer.Name">`) ve girişi kabul eder.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-520">Binding reduces code by using the same property to render form fields (`<input asp-for="Customer.Name">`) and accept the input.</span></span>

[!INCLUDE[](~/includes/bind-get.md)]

<span data-ttu-id="9d1cc-521">Giriş sayfası (*Index. cshtml*):</span><span class="sxs-lookup"><span data-stu-id="9d1cc-521">The home page (*Index.cshtml*):</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml)]

<span data-ttu-id="9d1cc-522">İlişkili `PageModel` sınıfı (*Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="9d1cc-522">The associated `PageModel` class (*Index.cshtml.cs*):</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs)]

<span data-ttu-id="9d1cc-523">*Index. cshtml* dosyası her kişi için bir düzenleme bağlantısı oluşturmak üzere aşağıdaki biçimlendirmeyi içerir:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-523">The *Index.cshtml* file contains the following markup to create an edit link for each contact:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=21)]

<span data-ttu-id="9d1cc-524">`<a asp-page="./Edit" asp-route-id="@contact.Id">Edit</a>` [tutturucu etiketi Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) , düzenleme sayfasına bir bağlantı oluşturmak için `asp-route-{value}` özniteliğini kullandı.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-524">The `<a asp-page="./Edit" asp-route-id="@contact.Id">Edit</a>` [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) used the `asp-route-{value}` attribute to generate a link to the Edit page.</span></span> <span data-ttu-id="9d1cc-525">Bağlantı, iletişim KIMLIĞINE sahip rota verileri içerir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-525">The link contains route data with the contact ID.</span></span> <span data-ttu-id="9d1cc-526">Örneğin, `https://localhost:5001/Edit/1`.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-526">For example, `https://localhost:5001/Edit/1`.</span></span> <span data-ttu-id="9d1cc-527">[Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro), Razor dosyalarında HTML öğelerinin oluşturulmasına ve işlenmesine sunucu tarafı kodun katılmasını etkinleştir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-527">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span> <span data-ttu-id="9d1cc-528">Etiket Yardımcıları `@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` tarafından etkinleştirilir</span><span class="sxs-lookup"><span data-stu-id="9d1cc-528">Tag Helpers are enabled by `@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers`</span></span>

<span data-ttu-id="9d1cc-529">*Pages/Edit. cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-529">The *Pages/Edit.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Edit.cshtml?highlight=1)]

<span data-ttu-id="9d1cc-530">İlk satır `@page "{id:int}"` yönergesini içerir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-530">The first line contains the `@page "{id:int}"` directive.</span></span> <span data-ttu-id="9d1cc-531">Yönlendirme kısıtlaması`"{id:int}"`, sayfaya istekleri `int` yönlendirme verileri içeren sayfaya kabul etmesini söyler.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-531">The routing constraint`"{id:int}"` tells the page to accept requests to the page that contain `int` route data.</span></span> <span data-ttu-id="9d1cc-532">Sayfaya yapılan bir istek bir `int`dönüştürülebildiği rota verileri içermiyorsa, çalışma zamanı bir HTTP 404 (bulunamadı) hatası döndürür.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-532">If a request to the page doesn't contain route data that can be converted to an `int`, the runtime returns an HTTP 404 (not found) error.</span></span> <span data-ttu-id="9d1cc-533">KIMLIĞI isteğe bağlı yapmak için `?` yol kısıtlamasına ekleyin:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-533">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="9d1cc-534">*Pages/Edit. cshtml. cs* dosyası:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-534">The *Pages/Edit.cshtml.cs* file:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Edit.cshtml.cs)]

<span data-ttu-id="9d1cc-535">*Index. cshtml* dosyası, her müşteri kişisi için bir silme düğmesi oluşturmak için de biçimlendirme içerir:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-535">The *Index.cshtml* file also contains markup to create a delete button for each customer contact:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=22-23)]

<span data-ttu-id="9d1cc-536">Sil düğmesi HTML 'de işlendiğinde, `formaction` şunlar için parametreler içerir:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-536">When the delete button is rendered in HTML, its `formaction` includes parameters for:</span></span>

* <span data-ttu-id="9d1cc-537">`asp-route-id` özniteliği tarafından belirtilen müşteri iletişim KIMLIĞI.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-537">The customer contact ID specified by the `asp-route-id` attribute.</span></span>
* <span data-ttu-id="9d1cc-538">`asp-page-handler` özniteliği tarafından belirtilen `handler`.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-538">The `handler` specified by the `asp-page-handler` attribute.</span></span>

<span data-ttu-id="9d1cc-539">Müşteri irtibat KIMLIĞI `1`olan işlenmiş silme düğmesine bir örnek aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-539">Here is an example of a rendered delete button with a customer contact ID of `1`:</span></span>

```html
<button type="submit" formaction="/?id=1&amp;handler=delete">delete</button>
```

<span data-ttu-id="9d1cc-540">Düğme seçildiğinde, sunucuya bir form `POST` isteği gönderilir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-540">When the button is selected, a form `POST` request is sent to the server.</span></span> <span data-ttu-id="9d1cc-541">Kurala göre, işleyici yönteminin adı, düzen `OnPost[handler]Async`göre `handler` parametresinin değerine göre seçilir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-541">By convention, the name of the handler method is selected based on the value of the `handler` parameter according to the scheme `OnPost[handler]Async`.</span></span>

<span data-ttu-id="9d1cc-542">Bu örnekte `handler` `delete` olduğundan, `OnPostDeleteAsync` Handler yöntemi `POST` isteğini işlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-542">Because the `handler` is `delete` in this example, the `OnPostDeleteAsync` handler method is used to process the `POST` request.</span></span> <span data-ttu-id="9d1cc-543">`asp-page-handler`, `remove`gibi farklı bir değere ayarlandıysa `OnPostRemoveAsync` ada sahip bir işleyici yöntemi seçilidir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-543">If the `asp-page-handler` is set to a different value, such as `remove`, a handler method with the name `OnPostRemoveAsync` is selected.</span></span> <span data-ttu-id="9d1cc-544">Aşağıdaki kod `OnPostDeleteAsync` işleyicisini gösterir:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-544">The following code shows the `OnPostDeleteAsync` handler:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs?range=26-37)]

<span data-ttu-id="9d1cc-545">`OnPostDeleteAsync` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-545">The `OnPostDeleteAsync` method:</span></span>

* <span data-ttu-id="9d1cc-546">Sorgu dizesinden `id` kabul eder.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-546">Accepts the `id` from the query string.</span></span> <span data-ttu-id="9d1cc-547">*Index. cshtml* sayfa yönergesi yönlendirme kısıtlaması `"{id:int?}"`içeriyorsa, `id` rota verilerinden gelir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-547">If the *Index.cshtml* page directive contained routing constraint `"{id:int?}"`, `id` would come from route data.</span></span> <span data-ttu-id="9d1cc-548">`id` için rota verileri, URI 'de `https://localhost:5001/Customers/2`gibi belirtilir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-548">The route data for `id` is specified in the URI such as `https://localhost:5001/Customers/2`.</span></span>
* <span data-ttu-id="9d1cc-549">`FindAsync`ile müşteri iletişim için veritabanını sorgular.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-549">Queries the database for the customer contact with `FindAsync`.</span></span>
* <span data-ttu-id="9d1cc-550">Müşteri ilgili kişisi bulunursa, bunlar müşteri kişileri listesinden kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-550">If the customer contact is found, they're removed from the list of customer contacts.</span></span> <span data-ttu-id="9d1cc-551">Veritabanı güncelleştirildi.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-551">The database is updated.</span></span>
* <span data-ttu-id="9d1cc-552">Kök dizin sayfasına yeniden yönlendirmek için `RedirectToPage` çağırır (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="9d1cc-552">Calls `RedirectToPage` to redirect to the root Index page (`/Index`).</span></span>

## <a name="mark-page-properties-as-required"></a><span data-ttu-id="9d1cc-553">Sayfa özelliklerini gerektiği gibi işaretle</span><span class="sxs-lookup"><span data-stu-id="9d1cc-553">Mark page properties as required</span></span>

<span data-ttu-id="9d1cc-554">`PageModel` Özellikler [gerekli](/dotnet/api/system.componentmodel.dataannotations.requiredattribute) öznitelikle işaretlenebilir:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-554">Properties on a `PageModel` can be marked with the [Required](/dotnet/api/system.componentmodel.dataannotations.requiredattribute) attribute:</span></span>

[!code-cs[](index/sample/Create.cshtml.cs?highlight=3,15-16)]

<span data-ttu-id="9d1cc-555">Daha fazla bilgi için bkz. [model doğrulaması](xref:mvc/models/validation).</span><span class="sxs-lookup"><span data-stu-id="9d1cc-555">For more information, see [Model validation](xref:mvc/models/validation).</span></span>

## <a name="handle-head-requests-with-an-onget-handler-fallback"></a><span data-ttu-id="9d1cc-556">OnGet işleyicisi geri dönüşü ile tanıtıcı HEAD istekleri</span><span class="sxs-lookup"><span data-stu-id="9d1cc-556">Handle HEAD requests with an OnGet handler fallback</span></span>

<span data-ttu-id="9d1cc-557">`HEAD` istekleri belirli bir kaynak için üst bilgileri almanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-557">`HEAD` requests allow you to retrieve the headers for a specific resource.</span></span> <span data-ttu-id="9d1cc-558">`GET` isteklerinin aksine `HEAD` istekleri bir yanıt gövdesi döndürmez.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-558">Unlike `GET` requests, `HEAD` requests don't return a response body.</span></span>

<span data-ttu-id="9d1cc-559">Normalde, `HEAD` istekleri için `OnHead` işleyicisi oluşturulur ve çağırılır:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-559">Ordinarily, an `OnHead` handler is created and called for `HEAD` requests:</span></span> 

```csharp
public void OnHead()
{
    HttpContext.Response.Headers.Add("HandledBy", "Handled by OnHead!");
}
```

<span data-ttu-id="9d1cc-560">ASP.NET Core 2,1 veya sonraki sürümlerde Razor Pages, `OnHead` işleyici tanımlanmadığında `OnGet` işleyicisini çağırmaya geri döner.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-560">In ASP.NET Core 2.1 or later, Razor Pages falls back to calling the `OnGet` handler if no `OnHead` handler is defined.</span></span> <span data-ttu-id="9d1cc-561">Bu davranış, `Startup.ConfigureServices`[Setcompatibilityversion](xref:mvc/compatibility-version) çağrısıyla etkinleştirilir:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-561">This behavior is enabled by the call to [SetCompatibilityVersion](xref:mvc/compatibility-version) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddMvc()
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_1);
```

<span data-ttu-id="9d1cc-562">Varsayılan Şablonlar, ASP.NET Core 2,1 ve 2,2 ' de `SetCompatibilityVersion` çağrısını üretir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-562">The default templates generate the `SetCompatibilityVersion` call in ASP.NET Core 2.1 and 2.2.</span></span> <span data-ttu-id="9d1cc-563">`SetCompatibilityVersion` `AllowMappingHeadRequestsToGetHandler` Razor Pages seçeneğini `true`için etkin şekilde ayarlar.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-563">`SetCompatibilityVersion` effectively sets the Razor Pages option `AllowMappingHeadRequestsToGetHandler` to `true`.</span></span>

<span data-ttu-id="9d1cc-564">`SetCompatibilityVersion`tüm davranışlardan çıkmak yerine, açıkça *belirli* davranışları kabul edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-564">Rather than opting in to all behaviors with `SetCompatibilityVersion`, you can explicitly opt in to *specific* behaviors.</span></span> <span data-ttu-id="9d1cc-565">Aşağıdaki kod, `HEAD` isteklerinin `OnGet` işleyicisine eşlenmesine izin vermek için ' de kullanılır:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-565">The following code opts in to allowing `HEAD` requests to be mapped to the `OnGet` handler:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        options.AllowMappingHeadRequestsToGetHandler = true;
    });
```

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a><span data-ttu-id="9d1cc-566">XSRF/CSRF ve Razor Pages</span><span class="sxs-lookup"><span data-stu-id="9d1cc-566">XSRF/CSRF and Razor Pages</span></span>

<span data-ttu-id="9d1cc-567">[Antiforgery doğrulaması](xref:security/anti-request-forgery)için herhangi bir kod yazmanız gerekmez.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-567">You don't have to write any code for [antiforgery validation](xref:security/anti-request-forgery).</span></span> <span data-ttu-id="9d1cc-568">Antiforgery belirteci oluşturma ve doğrulama, Razor Pages otomatik olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-568">Antiforgery token generation and validation are automatically included in Razor Pages.</span></span>

<a name="layout"></a>

## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a><span data-ttu-id="9d1cc-569">Razor Pages ile düzenleri, partileri, şablonları ve etiket yardımcılarını kullanma</span><span class="sxs-lookup"><span data-stu-id="9d1cc-569">Using Layouts, partials, templates, and Tag Helpers with Razor Pages</span></span>

<span data-ttu-id="9d1cc-570">Sayfalar, Razor görünüm altyapısının tüm özellikleri ile çalışır.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-570">Pages work with all the capabilities of the Razor view engine.</span></span> <span data-ttu-id="9d1cc-571">Düzenler, partıals, şablonlar, etiket yardımcıları, *_ViewStart. cshtml*, *_ViewImports. cshtml* geleneksel Razor görünümlerinde oldukları gibi çalışır.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-571">Layouts, partials, templates, Tag Helpers, *_ViewStart.cshtml*, *_ViewImports.cshtml* work in the same way they do for conventional Razor views.</span></span>

<span data-ttu-id="9d1cc-572">Bu özelliklerden bazılarının avantajlarından yararlanarak bu sayfayı declutter edelim.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-572">Let's declutter this page by taking advantage of some of those capabilities.</span></span>

<span data-ttu-id="9d1cc-573">*Sayfa/paylaşılan/_Layout. cshtml*'ye bir [Düzen sayfası](xref:mvc/views/layout) ekleyin:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-573">Add a [layout page](xref:mvc/views/layout) to *Pages/Shared/_Layout.cshtml*:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_LayoutSimple.cshtml)]

<span data-ttu-id="9d1cc-574">[Düzen](xref:mvc/views/layout):</span><span class="sxs-lookup"><span data-stu-id="9d1cc-574">The [Layout](xref:mvc/views/layout):</span></span>

* <span data-ttu-id="9d1cc-575">Her sayfanın yerleşimini denetler (sayfa düzen dışında değilse).</span><span class="sxs-lookup"><span data-stu-id="9d1cc-575">Controls the layout of each page (unless the page opts out of layout).</span></span>
* <span data-ttu-id="9d1cc-576">JavaScript ve stil sayfaları gibi HTML yapılarını içeri aktarır.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-576">Imports HTML structures such as JavaScript and stylesheets.</span></span>

<span data-ttu-id="9d1cc-577">Daha fazla bilgi için bkz. [Düzen sayfası](xref:mvc/views/layout) .</span><span class="sxs-lookup"><span data-stu-id="9d1cc-577">See [layout page](xref:mvc/views/layout) for more information.</span></span>

<span data-ttu-id="9d1cc-578">[Layout](xref:mvc/views/layout#specifying-a-layout) özelliği *Pages/_ViewStart. cshtml*' de ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-578">The [Layout](xref:mvc/views/layout#specifying-a-layout) property is set in *Pages/_ViewStart.cshtml*:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]

<span data-ttu-id="9d1cc-579">Düzen *Sayfalar/paylaşılan* klasöründedir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-579">The layout is in the *Pages/Shared* folder.</span></span> <span data-ttu-id="9d1cc-580">Sayfalar, geçerli sayfayla aynı klasörden başlayarak diğer görünümleri (düzenler, şablonlar, parals) hiyerarşik olarak arar.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-580">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="9d1cc-581">*Sayfalar/paylaşılan* klasördeki bir düzen, *Sayfalar* klasörü altındaki herhangi bir Razor sayfasından kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-581">A layout in the *Pages/Shared* folder can be used from any Razor page under the *Pages* folder.</span></span>

<span data-ttu-id="9d1cc-582">Düzen dosyası *Sayfalar/paylaşılan* klasörüne gitmelidir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-582">The layout file should go in the *Pages/Shared* folder.</span></span>

<span data-ttu-id="9d1cc-583">Düzen dosyasını *Görünümler/paylaşılan* klasöre **yerleştirmenizi öneririz** .</span><span class="sxs-lookup"><span data-stu-id="9d1cc-583">We recommend you **not** put the layout file in the *Views/Shared* folder.</span></span> <span data-ttu-id="9d1cc-584">*Görünümler/paylaşılan* bir MVC görünümleri modelidir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-584">*Views/Shared* is an MVC views pattern.</span></span> <span data-ttu-id="9d1cc-585">Razor Pages, yol kurallarını değil klasör hiyerarşisine güvenmektir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-585">Razor Pages are meant to rely on folder hierarchy, not path conventions.</span></span>

<span data-ttu-id="9d1cc-586">Bir Razor sayfasından arama görüntüleme, *Sayfalar* klasörünü içerir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-586">View search from a Razor Page includes the *Pages* folder.</span></span> <span data-ttu-id="9d1cc-587">MVC denetleyicileri ve geleneksel Razor görünümleriyle kullandığınız düzenler, şablonlar ve parals işlemleri *yalnızca çalışır*.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-587">The layouts, templates, and partials you're using with MVC controllers and conventional Razor views *just work*.</span></span>

<span data-ttu-id="9d1cc-588">Bir *Pages/_ViewImports. cshtml* dosyası ekleyin:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-588">Add a *Pages/_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]

<span data-ttu-id="9d1cc-589">`@namespace` öğreticide daha sonra açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-589">`@namespace` is explained later in the tutorial.</span></span> <span data-ttu-id="9d1cc-590">`@addTagHelper` yönergesi, [yerleşik etiket yardımcılarını](xref:mvc/views/tag-helpers/builtin-th/Index) *Sayfalar* klasöründeki tüm sayfalara getirir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-590">The `@addTagHelper` directive brings in the [built-in Tag Helpers](xref:mvc/views/tag-helpers/builtin-th/Index) to all the pages in the *Pages* folder.</span></span>

<a name="namespace"></a>

<span data-ttu-id="9d1cc-591">`@namespace` yönergesi açıkça bir sayfada kullanıldığında:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-591">When the `@namespace` directive is used explicitly on a page:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]

<span data-ttu-id="9d1cc-592">Yönergesi sayfanın ad alanını ayarlar.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-592">The directive sets the namespace for the page.</span></span> <span data-ttu-id="9d1cc-593">`@model` yönergesinin ad alanını içermesi gerekmez.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-593">The `@model` directive doesn't need to include the namespace.</span></span>

<span data-ttu-id="9d1cc-594">`@namespace` yönergesi *_ViewImports. cshtml*içinde yer aldığında, belirtilen ad alanı `@namespace` yönergesini Içeri aktaran sayfada oluşturulan ad alanı için ön ek sağlar.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-594">When the `@namespace` directive is contained in *_ViewImports.cshtml*, the specified namespace supplies the prefix for the generated namespace in the Page that imports the `@namespace` directive.</span></span> <span data-ttu-id="9d1cc-595">Oluşturulan ad alanı (sonek bölümü) geri kalanı, *_ViewImports. cshtml* içeren klasör ve sayfayı içeren klasör arasındaki noktayla ayrılmış göreli yoldur.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-595">The rest of the generated namespace (the suffix portion) is the dot-separated relative path between the folder containing *_ViewImports.cshtml* and the folder containing the page.</span></span>

<span data-ttu-id="9d1cc-596">Örneğin, `PageModel` Class *Pages/Customers/Edit. cshtml. cs* , ad alanını açıkça ayarlar:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-596">For example, the `PageModel` class *Pages/Customers/Edit.cshtml.cs* explicitly sets the namespace:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=snippet_namespace)]

<span data-ttu-id="9d1cc-597">*Pages/_ViewImports. cshtml* dosyası aşağıdaki ad alanını ayarlar:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-597">The *Pages/_ViewImports.cshtml* file sets the following namespace:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]

<span data-ttu-id="9d1cc-598">*Pages/Customers/Edit. cshtml* Razor sayfasının oluşturulan ad alanı `PageModel` sınıfıyla aynıdır.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-598">The generated namespace for the *Pages/Customers/Edit.cshtml* Razor Page is the same as the `PageModel` class.</span></span>

<span data-ttu-id="9d1cc-599">`@namespace` *Ayrıca geleneksel Razor görünümleriyle birlikte kullanılabilir.*</span><span class="sxs-lookup"><span data-stu-id="9d1cc-599">`@namespace` *also works with conventional Razor views.*</span></span>

<span data-ttu-id="9d1cc-600">Özgün *Sayfalar/Create. cshtml* görünüm dosyası:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-600">The original *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml?highlight=2)]

<span data-ttu-id="9d1cc-601">Güncelleştirilmiş *Sayfalar/Create. cshtml* görünüm dosyası:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-601">The updated *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/Create.cshtml?highlight=2)]

<span data-ttu-id="9d1cc-602">[Razor Pages Starter projesi](#rpvs17) , istemci tarafı doğrulamayı bağlayan *sayfaları/_ValidationScriptsPartial. cshtml*'yi içerir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-602">The [Razor Pages starter project](#rpvs17) contains the *Pages/_ValidationScriptsPartial.cshtml*, which hooks up client-side validation.</span></span>

<span data-ttu-id="9d1cc-603">Kısmi görünümler hakkında daha fazla bilgi için bkz. <xref:mvc/views/partial>.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-603">For more information on partial views, see <xref:mvc/views/partial>.</span></span>

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a><span data-ttu-id="9d1cc-604">Sayfalar için URL oluşturma</span><span class="sxs-lookup"><span data-stu-id="9d1cc-604">URL generation for Pages</span></span>

<span data-ttu-id="9d1cc-605">Daha önce gösterilen `Create` sayfası `RedirectToPage`kullanır:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-605">The `Create` page, shown previously, uses `RedirectToPage`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=10)]

<span data-ttu-id="9d1cc-606">Uygulama aşağıdaki dosya/klasör yapısına sahiptir:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-606">The app has the following file/folder structure:</span></span>

* <span data-ttu-id="9d1cc-607">*/Pages*</span><span class="sxs-lookup"><span data-stu-id="9d1cc-607">*/Pages*</span></span>

  * <span data-ttu-id="9d1cc-608">*Index. cshtml*</span><span class="sxs-lookup"><span data-stu-id="9d1cc-608">*Index.cshtml*</span></span>
  * <span data-ttu-id="9d1cc-609">*/Customers*</span><span class="sxs-lookup"><span data-stu-id="9d1cc-609">*/Customers*</span></span>

    * <span data-ttu-id="9d1cc-610">*. Cshtml oluştur*</span><span class="sxs-lookup"><span data-stu-id="9d1cc-610">*Create.cshtml*</span></span>
    * <span data-ttu-id="9d1cc-611">*Edit. cshtml*</span><span class="sxs-lookup"><span data-stu-id="9d1cc-611">*Edit.cshtml*</span></span>
    * <span data-ttu-id="9d1cc-612">*Index. cshtml*</span><span class="sxs-lookup"><span data-stu-id="9d1cc-612">*Index.cshtml*</span></span>

<span data-ttu-id="9d1cc-613">*Pages/Customers/Create. cshtml* ve *Pages/Customers/Edit. cshtml* sayfaları, başarılı olduktan sonra *Pages/Index. cshtml* dosyasına yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-613">The *Pages/Customers/Create.cshtml* and *Pages/Customers/Edit.cshtml* pages redirect to *Pages/Index.cshtml* after success.</span></span> <span data-ttu-id="9d1cc-614">`/Index` dize, önceki sayfaya erişmek için URI 'nin bir parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-614">The string `/Index` is part of the URI to access the preceding page.</span></span> <span data-ttu-id="9d1cc-615">`/Index` dize */Index. cshtml* sayfasına URI oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-615">The string `/Index` can be used to generate URIs to the *Pages/Index.cshtml* page.</span></span> <span data-ttu-id="9d1cc-616">Örnek:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-616">For example:</span></span>

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">My Index Page</a>`
* `RedirectToPage("/Index")`

<span data-ttu-id="9d1cc-617">Sayfa adı, kök */Pages* klasöründeki, önde gelen `/` (örneğin, `/Index`) içeren sayfanın yoludur.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-617">The page name is the path to the page from the root */Pages* folder including a leading `/` (for example, `/Index`).</span></span> <span data-ttu-id="9d1cc-618">Önceki URL oluşturma örnekleri bir URL 'YI kodlamadan gelişmiş seçenekler ve işlevsel yetenekler sunar.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-618">The preceding URL generation samples offer enhanced options and functional capabilities over hardcoding a URL.</span></span> <span data-ttu-id="9d1cc-619">URL oluşturma [yönlendirme](xref:mvc/controllers/routing) kullanır ve yolun hedef yolda nasıl tanımlandığınıza göre parametreleri oluşturabilir ve kodlayabilir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-619">URL generation uses [routing](xref:mvc/controllers/routing) and can generate and encode parameters according to how the route is defined in the destination path.</span></span>

<span data-ttu-id="9d1cc-620">Sayfalar için URL oluşturma göreli adları destekler.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-620">URL generation for pages supports relative names.</span></span> <span data-ttu-id="9d1cc-621">Aşağıdaki tabloda, *sayfa/müşteri/oluşturma. cshtml*'den farklı `RedirectToPage` parametrelerle hangi dizin sayfasının seçildiği gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-621">The following table shows which Index page is selected with different `RedirectToPage` parameters from *Pages/Customers/Create.cshtml*:</span></span>

| <span data-ttu-id="9d1cc-622">RedirectToPage (x)</span><span class="sxs-lookup"><span data-stu-id="9d1cc-622">RedirectToPage(x)</span></span>| <span data-ttu-id="9d1cc-623">Sayfasında</span><span class="sxs-lookup"><span data-stu-id="9d1cc-623">Page</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="9d1cc-624">RedirectToPage ("/Index")</span><span class="sxs-lookup"><span data-stu-id="9d1cc-624">RedirectToPage("/Index")</span></span> | <span data-ttu-id="9d1cc-625">*Sayfa/dizin*</span><span class="sxs-lookup"><span data-stu-id="9d1cc-625">*Pages/Index*</span></span> |
| <span data-ttu-id="9d1cc-626">RedirectToPage ("./Index");</span><span class="sxs-lookup"><span data-stu-id="9d1cc-626">RedirectToPage("./Index");</span></span> | <span data-ttu-id="9d1cc-627">*Sayfalar/müşteriler/Dizin*</span><span class="sxs-lookup"><span data-stu-id="9d1cc-627">*Pages/Customers/Index*</span></span> |
| <span data-ttu-id="9d1cc-628">RedirectToPage (".. /İndex ")</span><span class="sxs-lookup"><span data-stu-id="9d1cc-628">RedirectToPage("../Index")</span></span> | <span data-ttu-id="9d1cc-629">*Sayfa/dizin*</span><span class="sxs-lookup"><span data-stu-id="9d1cc-629">*Pages/Index*</span></span> |
| <span data-ttu-id="9d1cc-630">RedirectToPage ("Dizin")</span><span class="sxs-lookup"><span data-stu-id="9d1cc-630">RedirectToPage("Index")</span></span>  | <span data-ttu-id="9d1cc-631">*Sayfalar/müşteriler/Dizin*</span><span class="sxs-lookup"><span data-stu-id="9d1cc-631">*Pages/Customers/Index*</span></span> |

<span data-ttu-id="9d1cc-632">`RedirectToPage("Index")`, `RedirectToPage("./Index")`ve `RedirectToPage("../Index")` *göreli adlardır*.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-632">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, and `RedirectToPage("../Index")`  are *relative names*.</span></span> <span data-ttu-id="9d1cc-633">`RedirectToPage` parametresi, hedef sayfanın adını hesaplamak için geçerli sayfanın yoluyla *birleştirilir* .</span><span class="sxs-lookup"><span data-stu-id="9d1cc-633">The `RedirectToPage` parameter is *combined* with the path of the current page to compute the name of the destination page.</span></span>  <!-- Review: Original had The provided string is combined with the page name of the current page to compute the name of the destination page.  page name, not page path -->

<span data-ttu-id="9d1cc-634">Karmaşık bir yapıya sahip siteler oluştururken göreli ad bağlama yararlı olur.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-634">Relative name linking is useful when building sites with a complex structure.</span></span> <span data-ttu-id="9d1cc-635">Bir klasördeki sayfalar arasında bağlantı sağlamak için göreli adlar kullanırsanız, bu klasörü yeniden adlandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-635">If you use relative names to link between pages in a folder, you can rename that folder.</span></span> <span data-ttu-id="9d1cc-636">Tüm bağlantılar hala çalışır (klasör adını içermediği için).</span><span class="sxs-lookup"><span data-stu-id="9d1cc-636">All the links still work (because they didn't include the folder name).</span></span>

<span data-ttu-id="9d1cc-637">Farklı bir [alandaki](xref:mvc/controllers/areas)bir sayfaya yeniden yönlendirmek için alanını belirtin:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-637">To redirect to a page in a different [Area](xref:mvc/controllers/areas), specify the area:</span></span>

```csharp
RedirectToPage("/Index", new { area = "Services" });
```

<span data-ttu-id="9d1cc-638">Daha fazla bilgi için bkz. <xref:mvc/controllers/areas>.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-638">For more information, see <xref:mvc/controllers/areas>.</span></span>

## <a name="viewdata-attribute"></a><span data-ttu-id="9d1cc-639">ViewData özniteliği</span><span class="sxs-lookup"><span data-stu-id="9d1cc-639">ViewData attribute</span></span>

<span data-ttu-id="9d1cc-640">Veri, [Viewdataattribute](/dotnet/api/microsoft.aspnetcore.mvc.viewdataattribute)içeren bir sayfaya geçirilebilir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-640">Data can be passed to a page with [ViewDataAttribute](/dotnet/api/microsoft.aspnetcore.mvc.viewdataattribute).</span></span> <span data-ttu-id="9d1cc-641">`[ViewData]` özniteliği olan denetleyicilerde veya Razor sayfa modellerinde bulunan özelliklerin değerleri, [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary)'den depolanır ve yüklenir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-641">Properties on controllers or Razor Page models with the `[ViewData]` attribute have their values stored and loaded from the [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary).</span></span>

<span data-ttu-id="9d1cc-642">Aşağıdaki örnekte `AboutModel`, `[ViewData]`ile işaretlenmiş bir `Title` özelliği içerir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-642">In the following example, the `AboutModel` contains a `Title` property marked with `[ViewData]`.</span></span> <span data-ttu-id="9d1cc-643">`Title` özelliği, hakkında sayfasının başlığına ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-643">The `Title` property is set to the title of the About page:</span></span>

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

<span data-ttu-id="9d1cc-644">Hakkında sayfasında, `Title` özelliğine model özelliği olarak erişin:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-644">In the About page, access the `Title` property as a model property:</span></span>

```cshtml
<h1>@Model.Title</h1>
```

<span data-ttu-id="9d1cc-645">Mizanpajda, başlık ViewData sözlüğünden okundu:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-645">In the layout, the title is read from the ViewData dictionary:</span></span>

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"] - WebApplication</title>
    ...
```

## <a name="tempdata"></a><span data-ttu-id="9d1cc-646">TempData</span><span class="sxs-lookup"><span data-stu-id="9d1cc-646">TempData</span></span>

<span data-ttu-id="9d1cc-647">ASP.NET Core bir [denetleyicide](/dotnet/api/microsoft.aspnetcore.mvc.controller) [TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) özelliğini kullanıma sunar.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-647">ASP.NET Core exposes the [TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) property on a [controller](/dotnet/api/microsoft.aspnetcore.mvc.controller).</span></span> <span data-ttu-id="9d1cc-648">Bu özellik, okunana kadar verileri depolar.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-648">This property stores data until it's read.</span></span> <span data-ttu-id="9d1cc-649">`Keep` ve `Peek` yöntemleri silinmeden verileri incelemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-649">The `Keep` and `Peek` methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="9d1cc-650">`TempData`, bir tek istekten daha fazla veri gerektiğinde yeniden yönlendirme için kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-650">`TempData` is  useful for redirection, when data is needed for more than a single request.</span></span>

<span data-ttu-id="9d1cc-651">Aşağıdaki kod, `TempData`kullanarak `Message` değerini ayarlar:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-651">The following code sets the value of `Message` using `TempData`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,25&name=snippet_Temp)]

<span data-ttu-id="9d1cc-652">*Pages/Customers/Index. cshtml* dosyasında aşağıdaki biçimlendirme `TempData`kullanarak `Message` değerini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-652">The following markup in the *Pages/Customers/Index.cshtml* file displays the value of `Message` using `TempData`.</span></span>

```cshtml
<h3>Msg: @Model.Message</h3>
```

<span data-ttu-id="9d1cc-653">*Pages/Customers/Index. cshtml. cs* sayfa modeli, `[TempData]` özniteliğini `Message` özelliğine uygular.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-653">The *Pages/Customers/Index.cshtml.cs* page model applies the `[TempData]` attribute to the `Message` property.</span></span>

```csharp
[TempData]
public string Message { get; set; }
```

<span data-ttu-id="9d1cc-654">Daha fazla bilgi için bkz. [TempData](xref:fundamentals/app-state#tempdata) .</span><span class="sxs-lookup"><span data-stu-id="9d1cc-654">For more information, see [TempData](xref:fundamentals/app-state#tempdata) .</span></span>

<a name="mhpp"></a>

## <a name="multiple-handlers-per-page"></a><span data-ttu-id="9d1cc-655">Sayfa başına birden çok işleyici</span><span class="sxs-lookup"><span data-stu-id="9d1cc-655">Multiple handlers per page</span></span>

<span data-ttu-id="9d1cc-656">Aşağıdaki sayfa `asp-page-handler` etiketi Yardımcısını kullanarak iki işleyici için biçimlendirme oluşturur:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-656">The following page generates markup for two handlers using the `asp-page-handler` Tag Helper:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

<!-- Review: the FormActionTagHelper applies to all <form /> elements on a Razor page, even when there's no `asp-` attribute   -->

<span data-ttu-id="9d1cc-657">Yukarıdaki örnekteki formda, her biri farklı bir URL 'ye göndermek için `FormActionTagHelper` kullanan iki gönderme düğmesi vardır.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-657">The form in the preceding example has two submit buttons, each using the `FormActionTagHelper` to submit to a different URL.</span></span> <span data-ttu-id="9d1cc-658">`asp-page-handler` özniteliği `asp-page`bir yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-658">The `asp-page-handler` attribute is a companion to `asp-page`.</span></span> <span data-ttu-id="9d1cc-659">`asp-page-handler`, bir sayfa tarafından tanımlanan her bir işleyici yöntemini gönderen URL 'Ler oluşturur.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-659">`asp-page-handler` generates URLs that submit to each of the handler methods defined by a page.</span></span> <span data-ttu-id="9d1cc-660">örnek geçerli sayfaya bağlandığından `asp-page` belirtilmedi.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-660">`asp-page` isn't specified because the sample is linking to the current page.</span></span>

<span data-ttu-id="9d1cc-661">Sayfa modeli:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-661">The page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]

<span data-ttu-id="9d1cc-662">Yukarıdaki kod, *adlandırılmış işleyici yöntemlerini*kullanır.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-662">The preceding code uses *named handler methods*.</span></span> <span data-ttu-id="9d1cc-663">Adlandırılmış işleyici yöntemleri, `On<HTTP Verb>` sonra ve `Async` önce (varsa), ad içindeki metin alınarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-663">Named handler methods are created by taking the text in the name after `On<HTTP Verb>` and before `Async` (if present).</span></span> <span data-ttu-id="9d1cc-664">Yukarıdaki örnekte, Page metotları OnPost**Joinlist**Async ve onpost**Joinlıstuc**Async ' dir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-664">In the preceding example, the page methods are OnPost**JoinList**Async and OnPost**JoinListUC**Async.</span></span> <span data-ttu-id="9d1cc-665">*Onpost* Ile *zaman uyumsuz* olarak kaldırıldığında, işleyici adları `JoinList` ve `JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-665">With *OnPost* and *Async* removed, the handler names are `JoinList` and `JoinListUC`.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?range=12-13)]

<span data-ttu-id="9d1cc-666">Yukarıdaki kodu kullanarak, `OnPostJoinListAsync` ' a gönderen URL yolu `https://localhost:5001/Customers/CreateFATH?handler=JoinList`.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-666">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `https://localhost:5001/Customers/CreateFATH?handler=JoinList`.</span></span> <span data-ttu-id="9d1cc-667">`OnPostJoinListUCAsync` ' a gönderen URL yolu `https://localhost:5001/Customers/CreateFATH?handler=JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-667">The URL path that submits to `OnPostJoinListUCAsync` is `https://localhost:5001/Customers/CreateFATH?handler=JoinListUC`.</span></span>

## <a name="custom-routes"></a><span data-ttu-id="9d1cc-668">Özel yollar</span><span class="sxs-lookup"><span data-stu-id="9d1cc-668">Custom routes</span></span>

<span data-ttu-id="9d1cc-669">`@page` yönergesini kullanarak şunları yapın:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-669">Use the `@page` directive to:</span></span>

* <span data-ttu-id="9d1cc-670">Sayfaya özel bir yol belirtin.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-670">Specify a custom route to a page.</span></span> <span data-ttu-id="9d1cc-671">Örneğin, hakkında sayfasına olan yol `@page "/Some/Other/Path"``/Some/Other/Path` olarak ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-671">For example, the route to the About page can be set to `/Some/Other/Path` with `@page "/Some/Other/Path"`.</span></span>
* <span data-ttu-id="9d1cc-672">Kesimleri bir sayfanın varsayılan yoluna ekleyin.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-672">Append segments to a page's default route.</span></span> <span data-ttu-id="9d1cc-673">Örneğin, bir "öğe" segmenti sayfanın varsayılan yoluna `@page "item"`eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-673">For example, an "item" segment can be added to a page's default route with `@page "item"`.</span></span>
* <span data-ttu-id="9d1cc-674">Bir sayfanın varsayılan yoluna parametreleri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-674">Append parameters to a page's default route.</span></span> <span data-ttu-id="9d1cc-675">Örneğin, `@page "{id}"`bir sayfa için `id`bir ID parametresi gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-675">For example, an ID parameter, `id`, can be required for a page with `@page "{id}"`.</span></span>

<span data-ttu-id="9d1cc-676">Yolun başındaki bir tilde (`~`) tarafından atanan kök göreli bir yol desteklenir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-676">A root-relative path designated by a tilde (`~`) at the beginning of the path is supported.</span></span> <span data-ttu-id="9d1cc-677">Örneğin, `@page "~/Some/Other/Path"` `@page "/Some/Other/Path"`aynıdır.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-677">For example, `@page "~/Some/Other/Path"` is the same as `@page "/Some/Other/Path"`.</span></span>

<span data-ttu-id="9d1cc-678">Sorgu dizesini URL 'de `?handler=JoinList` beğenmezseniz, URL 'nin yol bölümüne işleyici adını koymak için yolu değiştirin.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-678">If you don't like the query string `?handler=JoinList` in the URL, change the route to put the handler name in the path portion of the URL.</span></span> <span data-ttu-id="9d1cc-679">Yol, `@page` yönergesinin ardından çift tırnak içine alınmış bir rota şablonu eklenerek özelleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-679">The route can be customized by adding a route template enclosed in double quotes after the `@page` directive.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]

<span data-ttu-id="9d1cc-680">Yukarıdaki kodu kullanarak, `OnPostJoinListAsync` ' a gönderen URL yolu `https://localhost:5001/Customers/CreateFATH/JoinList`.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-680">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `https://localhost:5001/Customers/CreateFATH/JoinList`.</span></span> <span data-ttu-id="9d1cc-681">`OnPostJoinListUCAsync` ' a gönderen URL yolu `https://localhost:5001/Customers/CreateFATH/JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-681">The URL path that submits to `OnPostJoinListUCAsync` is `https://localhost:5001/Customers/CreateFATH/JoinListUC`.</span></span>

<span data-ttu-id="9d1cc-682">Aşağıdaki `handler` `?` yol parametresinin isteğe bağlı olduğu anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-682">The `?` following `handler` means the route parameter is optional.</span></span>

## <a name="configuration-and-settings"></a><span data-ttu-id="9d1cc-683">Yapılandırma ve ayarlar</span><span class="sxs-lookup"><span data-stu-id="9d1cc-683">Configuration and settings</span></span>

<span data-ttu-id="9d1cc-684">Gelişmiş seçenekleri yapılandırmak için, MVC Oluşturucu 'da `AddRazorPagesOptions` genişletme yöntemini kullanın:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-684">To configure advanced options, use the extension method `AddRazorPagesOptions` on the MVC builder:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/StartupAdvanced.cs?name=snippet_1)]

<span data-ttu-id="9d1cc-685">Şu anda, sayfalar için kök dizini ayarlamak veya sayfalar için uygulama modeli kuralları eklemek üzere `RazorPagesOptions` kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-685">Currently you can use the `RazorPagesOptions` to set the root directory for pages, or add application model conventions for pages.</span></span> <span data-ttu-id="9d1cc-686">Gelecekte bu şekilde daha fazla genişletilebilirlik etkinleştireceğiz.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-686">We'll enable more extensibility this way in the future.</span></span>

<span data-ttu-id="9d1cc-687">Görünümleri önceden derlemek için bkz. [Razor görünüm derlemesi](xref:mvc/views/view-compilation) .</span><span class="sxs-lookup"><span data-stu-id="9d1cc-687">To precompile views, see [Razor view compilation](xref:mvc/views/view-compilation) .</span></span>

<span data-ttu-id="9d1cc-688">[Örnek kodu indirin veya görüntüleyin](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/index/sample).</span><span class="sxs-lookup"><span data-stu-id="9d1cc-688">[Download or view sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/index/sample).</span></span>

<span data-ttu-id="9d1cc-689">Bu giriş hakkında bilgi için bkz. [Razor Pages kullanmaya başlama](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="9d1cc-689">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start), which builds on this introduction.</span></span>

### <a name="specify-that-razor-pages-are-at-the-content-root"></a><span data-ttu-id="9d1cc-690">Razor Pages içerik kökünde olduğunu belirtin</span><span class="sxs-lookup"><span data-stu-id="9d1cc-690">Specify that Razor Pages are at the content root</span></span>

<span data-ttu-id="9d1cc-691">Varsayılan olarak, Razor Pages */Pages* dizininde kök olarak depolanır.</span><span class="sxs-lookup"><span data-stu-id="9d1cc-691">By default, Razor Pages are rooted in the */Pages* directory.</span></span> <span data-ttu-id="9d1cc-692">Razor Pages, uygulamanın [içerik kökünde](xref:fundamentals/index#content-root) ([contentrootpath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) olduğunu belirtmek Için [addmvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) 'ye [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) ekleyin:</span><span class="sxs-lookup"><span data-stu-id="9d1cc-692">Add [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at the [content root](xref:fundamentals/index#content-root) ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) of the app:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesAtContentRoot();
```

### <a name="specify-that-razor-pages-are-at-a-custom-root-directory"></a><span data-ttu-id="9d1cc-693">Razor Pages özel kök dizinde olduğunu belirtin</span><span class="sxs-lookup"><span data-stu-id="9d1cc-693">Specify that Razor Pages are at a custom root directory</span></span>

<span data-ttu-id="9d1cc-694">Razor Pages uygulamadaki özel bir kök dizinde olduğunu belirtmek için [Addmvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) 'ye [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) ekleyin (göreli bir yol sağlayın):</span><span class="sxs-lookup"><span data-stu-id="9d1cc-694">Add [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at a custom root directory in the app (provide a relative path):</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesRoot("/path/to/razor/pages");
```

## <a name="additional-resources"></a><span data-ttu-id="9d1cc-695">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="9d1cc-695">Additional resources</span></span>

* <xref:index>
* <xref:mvc/views/razor>
* <xref:mvc/controllers/areas>
* <xref:tutorials/razor-pages/razor-pages-start>
* <xref:security/authorization/razor-pages-authorization>
* <xref:razor-pages/razor-pages-conventions>
* <xref:test/razor-pages-tests>
* <xref:mvc/views/partial>

::: moniker-end
