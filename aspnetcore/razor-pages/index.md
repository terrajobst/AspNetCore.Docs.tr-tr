---
title: ASP.NET Core Razor Pages giriş
author: Rick-Anderson
description: Nasıl ASP.NET Core Razor sayfalar kodlama sayfa odaklı senaryolar daha kolay ve MVC kullanmaktan daha üretken hale getirdiğini öğrenin.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 10/07/2019
uid: razor-pages/index
ms.openlocfilehash: 61b1c3a17b378524c8fea9004b615c2d3d480135
ms.sourcegitcommit: 3d082bd46e9e00a3297ea0314582b1ed2abfa830
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/07/2019
ms.locfileid: "72007465"
---
# <a name="introduction-to-razor-pages-in-aspnet-core"></a><span data-ttu-id="021e2-103">ASP.NET Core Razor Pages giriş</span><span class="sxs-lookup"><span data-stu-id="021e2-103">Introduction to Razor Pages in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="021e2-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Ryan şimdi ak](https://github.com/rynowak)</span><span class="sxs-lookup"><span data-stu-id="021e2-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Ryan Nowak](https://github.com/rynowak)</span></span>

<span data-ttu-id="021e2-105">Razor Pages, kodlama sayfasına odaklanmış senaryolar denetleyicileri ve görünümleri kullanmaktan daha kolay ve daha üretken hale getirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="021e2-105">Razor Pages can make coding page-focused scenarios easier and more productive than using controllers and views.</span></span>

<span data-ttu-id="021e2-106">Model-View-Controller yaklaşımını kullanan bir öğretici arıyorsanız, bkz. [ASP.NET Core MVC ile çalışmaya başlama](xref:tutorials/first-mvc-app/start-mvc).</span><span class="sxs-lookup"><span data-stu-id="021e2-106">If you're looking for a tutorial that uses the Model-View-Controller approach, see [Get started with ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span></span>

<span data-ttu-id="021e2-107">Bu belge Razor Pages bir giriş sağlar.</span><span class="sxs-lookup"><span data-stu-id="021e2-107">This document provides an introduction to Razor Pages.</span></span> <span data-ttu-id="021e2-108">Adım adım öğretici değildir.</span><span class="sxs-lookup"><span data-stu-id="021e2-108">It's not a step by step tutorial.</span></span> <span data-ttu-id="021e2-109">Bölümlerden bazılarını çok gelişmiş bir şekilde bulursanız, bkz. [Razor Pages kullanmaya başlama](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="021e2-109">If you find some of the sections too advanced, see [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span></span> <span data-ttu-id="021e2-110">ASP.NET Core genel bir bakış için bkz. [ASP.NET Core giriş](xref:index).</span><span class="sxs-lookup"><span data-stu-id="021e2-110">For an overview of ASP.NET Core, see the [Introduction to ASP.NET Core](xref:index).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="021e2-111">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="021e2-111">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="021e2-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="021e2-112">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="021e2-113">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="021e2-113">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="021e2-114">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="021e2-114">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

<a name="rpvs17"></a>

## <a name="create-a-razor-pages-project"></a><span data-ttu-id="021e2-115">Razor Pages projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="021e2-115">Create a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="021e2-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="021e2-116">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="021e2-117">Razor Pages projesi oluşturma hakkında ayrıntılı yönergeler için bkz. [Razor Pages kullanmaya başlama](xref:tutorials/razor-pages/razor-pages-start) .</span><span class="sxs-lookup"><span data-stu-id="021e2-117">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) for detailed instructions on how to create a Razor Pages project.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="021e2-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="021e2-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="021e2-119">Komut satırından `dotnet new webapp` komutunu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="021e2-119">Run `dotnet new webapp` from the command line.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="021e2-120">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="021e2-120">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="021e2-121">Komut satırından `dotnet new webapp` komutunu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="021e2-121">Run `dotnet new webapp` from the command line.</span></span>

<span data-ttu-id="021e2-122">Oluşturulan *. csproj* dosyasını Mac için Visual Studio açın.</span><span class="sxs-lookup"><span data-stu-id="021e2-122">Open the generated *.csproj* file from Visual Studio for Mac.</span></span>

---

## <a name="razor-pages"></a><span data-ttu-id="021e2-123">Razor Pages</span><span class="sxs-lookup"><span data-stu-id="021e2-123">Razor Pages</span></span>

<span data-ttu-id="021e2-124">Razor Pages, *Startup.cs*'de etkinleştirilmiştir:</span><span class="sxs-lookup"><span data-stu-id="021e2-124">Razor Pages is enabled in *Startup.cs*:</span></span>

[!code-cs[](index/3.0sample/RazorPagesIntro/Startup.cs?name=snippet_Startup&highlight=12)]

<span data-ttu-id="021e2-125">Temel bir sayfa düşünün:<a name="OnGet"></a></span><span class="sxs-lookup"><span data-stu-id="021e2-125">Consider a basic page: <a name="OnGet"></a></span></span>

[!code-cshtml[](index/3.0sample/RazorPagesIntro/Pages/Index.cshtml?highlight=1)]

<span data-ttu-id="021e2-126">Yukarıdaki kod, denetleyiciler ve görünümlerle ASP.NET Core bir uygulamada kullanılan [Razor görünüm dosyası](xref:tutorials/first-mvc-app/adding-view) gibi bir çok şey arar.</span><span class="sxs-lookup"><span data-stu-id="021e2-126">The preceding code looks a lot like a [Razor view file](xref:tutorials/first-mvc-app/adding-view) used in an ASP.NET Core app with controllers and views.</span></span> <span data-ttu-id="021e2-127">Bunu farklı kılan [@page](xref:mvc/views/razor#page) yönergedir.</span><span class="sxs-lookup"><span data-stu-id="021e2-127">What makes it different is the [@page](xref:mvc/views/razor#page) directive.</span></span> <span data-ttu-id="021e2-128">`@page`, dosyayı bir MVC eylemine dönüştürür. Bu, bir denetleyiciden geçmeden istekleri doğrudan işlediği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="021e2-128">`@page` makes the file into an MVC action - which means that it handles requests directly, without going through a controller.</span></span> <span data-ttu-id="021e2-129">`@page` bir sayfada ilk Razor yönergesi olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="021e2-129">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="021e2-130">`@page`, diğer [Razor](xref:mvc/views/razor) yapıları davranışını etkiler.</span><span class="sxs-lookup"><span data-stu-id="021e2-130">`@page` affects the behavior of other [Razor](xref:mvc/views/razor) constructs.</span></span> <span data-ttu-id="021e2-131">Razor Pages dosya adlarında *. cshtml* soneki vardır.</span><span class="sxs-lookup"><span data-stu-id="021e2-131">Razor Pages file names have a *.cshtml* suffix.</span></span>

<span data-ttu-id="021e2-132">@No__t-0 sınıfı kullanan benzer bir sayfa aşağıdaki iki dosyada gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="021e2-132">A similar page, using a `PageModel` class, is shown in the following two files.</span></span> <span data-ttu-id="021e2-133">*Pages/Index2. cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="021e2-133">The *Pages/Index2.cshtml* file:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesIntro/Pages/Index2.cshtml)]

<span data-ttu-id="021e2-134">*Pages/Index2. cshtml. cs* sayfa modeli:</span><span class="sxs-lookup"><span data-stu-id="021e2-134">The *Pages/Index2.cshtml.cs* page model:</span></span>

[!code-cs[](index/3.0sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]

<span data-ttu-id="021e2-135">Kurala göre `PageModel` sınıf dosyası, *. cs* eklenmiş Razor sayfası dosyasıyla aynı ada sahiptir.</span><span class="sxs-lookup"><span data-stu-id="021e2-135">By convention, the `PageModel` class file has the same name as the Razor Page file with *.cs* appended.</span></span> <span data-ttu-id="021e2-136">Örneğin, önceki Razor sayfası *Pages/Index2. cshtml*' dir.</span><span class="sxs-lookup"><span data-stu-id="021e2-136">For example, the previous Razor Page is *Pages/Index2.cshtml*.</span></span> <span data-ttu-id="021e2-137">@No__t-0 sınıfını içeren dosya *sayfa/Index2. cshtml. cs*olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="021e2-137">The file containing the `PageModel` class is named *Pages/Index2.cshtml.cs*.</span></span>

<span data-ttu-id="021e2-138">URL yollarının sayfalara olan ilişkilendirmeleri, sayfanın dosya sistemindeki konumuna göre belirlenir.</span><span class="sxs-lookup"><span data-stu-id="021e2-138">The associations of URL paths to pages are determined by the page's location in the file system.</span></span> <span data-ttu-id="021e2-139">Aşağıdaki tabloda bir Razor sayfa yolu ve eşleşen URL gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="021e2-139">The following table shows a Razor Page path and the matching URL:</span></span>

| <span data-ttu-id="021e2-140">Dosya adı ve yolu</span><span class="sxs-lookup"><span data-stu-id="021e2-140">File name and path</span></span>               | <span data-ttu-id="021e2-141">eşleşen URL</span><span class="sxs-lookup"><span data-stu-id="021e2-141">matching URL</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="021e2-142">*/Pages/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="021e2-142">*/Pages/Index.cshtml*</span></span> | <span data-ttu-id="021e2-143">`/` veya `/Index`</span><span class="sxs-lookup"><span data-stu-id="021e2-143">`/` or `/Index`</span></span> |
| <span data-ttu-id="021e2-144">*/Pages/Contact.exe*</span><span class="sxs-lookup"><span data-stu-id="021e2-144">*/Pages/Contact.cshtml*</span></span> | `/Contact` |
| <span data-ttu-id="021e2-145">*/Pages/Store/Contact.exe*</span><span class="sxs-lookup"><span data-stu-id="021e2-145">*/Pages/Store/Contact.cshtml*</span></span> | `/Store/Contact` |
| <span data-ttu-id="021e2-146">*/Pages/Store/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="021e2-146">*/Pages/Store/Index.cshtml*</span></span> | <span data-ttu-id="021e2-147">`/Store` veya `/Store/Index`</span><span class="sxs-lookup"><span data-stu-id="021e2-147">`/Store` or `/Store/Index`</span></span> |

<span data-ttu-id="021e2-148">Notlar:</span><span class="sxs-lookup"><span data-stu-id="021e2-148">Notes:</span></span>

* <span data-ttu-id="021e2-149">Çalışma zamanı, *Sayfalar* klasöründeki Razor Pages dosyaları varsayılan olarak arar.</span><span class="sxs-lookup"><span data-stu-id="021e2-149">The runtime looks for Razor Pages files in the *Pages* folder by default.</span></span>
* <span data-ttu-id="021e2-150">URL, bir sayfa içermiyorsa varsayılan sayfa olan `Index` ' dır.</span><span class="sxs-lookup"><span data-stu-id="021e2-150">`Index` is the default page when a URL doesn't include a page.</span></span>

## <a name="write-a-basic-form"></a><span data-ttu-id="021e2-151">Temel form yazma</span><span class="sxs-lookup"><span data-stu-id="021e2-151">Write a basic form</span></span>

<span data-ttu-id="021e2-152">Razor Pages, Web tarayıcıları ile kullanılan ortak desenleri bir uygulama oluştururken kolayca uygulanması için tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="021e2-152">Razor Pages is designed to make common patterns used with web browsers easy to implement when building an app.</span></span> <span data-ttu-id="021e2-153">[Model bağlama](xref:mvc/models/model-binding), [ETIKET yardımcıları](xref:mvc/views/tag-helpers/intro)ve HTML Yardımcıları hepsi, Razor sayfası sınıfında tanımlanan özelliklerle *çalışır* .</span><span class="sxs-lookup"><span data-stu-id="021e2-153">[Model binding](xref:mvc/models/model-binding), [Tag Helpers](xref:mvc/views/tag-helpers/intro), and HTML helpers all *just work* with the properties defined in a Razor Page class.</span></span> <span data-ttu-id="021e2-154">@No__t-0 modeli için temel bir "bize başvurun" formu uygulayan bir sayfa düşünün:</span><span class="sxs-lookup"><span data-stu-id="021e2-154">Consider a page that implements a basic "contact us" form for the `Contact` model:</span></span>

<span data-ttu-id="021e2-155">Bu belgedeki örneklerde, `DbContext` [Startup.cs](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/razor-pages/index/3.0sample/RazorPagesContacts/Startup.cs#L23-L24) dosyasında başlatılır.</span><span class="sxs-lookup"><span data-stu-id="021e2-155">For the samples in this document, the `DbContext` is initialized in the [Startup.cs](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/razor-pages/index/3.0sample/RazorPagesContacts/Startup.cs#L23-L24) file.</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Startup.cs?name=snippet)]

<span data-ttu-id="021e2-156">Veri modeli:</span><span class="sxs-lookup"><span data-stu-id="021e2-156">The data model:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Models/Customer.cs)]

<span data-ttu-id="021e2-157">DB bağlamı:</span><span class="sxs-lookup"><span data-stu-id="021e2-157">The db context:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Data/CustomerDbContext.cs)]

<span data-ttu-id="021e2-158">*Pages/Create. cshtml* görünüm dosyası:</span><span class="sxs-lookup"><span data-stu-id="021e2-158">The *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml)]

<span data-ttu-id="021e2-159">*Pages/Create. cshtml. cs* sayfa modeli:</span><span class="sxs-lookup"><span data-stu-id="021e2-159">The *Pages/Create.cshtml.cs* page model:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml.cs?name=snippet_ALL)]

<span data-ttu-id="021e2-160">Kurala göre `PageModel` sınıfı `<PageName>Model` olarak adlandırılır ve sayfayla aynı ad alanında yer alan.</span><span class="sxs-lookup"><span data-stu-id="021e2-160">By convention, the `PageModel` class is called `<PageName>Model` and is in the same namespace as the page.</span></span>

<span data-ttu-id="021e2-161">@No__t-0 sınıfı, bir sayfanın mantığının sunumuna ayrılmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="021e2-161">The `PageModel` class allows separation of the logic of a page from its presentation.</span></span> <span data-ttu-id="021e2-162">Sayfaya gönderilen istekler için sayfa işleyicilerini ve sayfayı işlemek için kullanılan verileri tanımlar.</span><span class="sxs-lookup"><span data-stu-id="021e2-162">It defines page handlers for requests sent to the page and the data used to render the page.</span></span> <span data-ttu-id="021e2-163">Bu ayrım şunları sağlar:</span><span class="sxs-lookup"><span data-stu-id="021e2-163">This separation allows:</span></span>

* <span data-ttu-id="021e2-164">[Bağımlılık ekleme](xref:fundamentals/dependency-injection)yoluyla sayfa bağımlılıklarını yönetme.</span><span class="sxs-lookup"><span data-stu-id="021e2-164">Managing of page dependencies through [dependency injection](xref:fundamentals/dependency-injection).</span></span>
* [<span data-ttu-id="021e2-165">Birim testi</span><span class="sxs-lookup"><span data-stu-id="021e2-165">Unit testing</span></span>](xref:test/razor-pages-tests)

<span data-ttu-id="021e2-166">Sayfada, @no__t 2 isteklerinde çalışan (bir Kullanıcı formu gönderdiğinde) `OnPostAsync` *işleyicisi yöntemi*vardır.</span><span class="sxs-lookup"><span data-stu-id="021e2-166">The page has an `OnPostAsync` *handler method*, which runs on `POST` requests (when a user posts the form).</span></span> <span data-ttu-id="021e2-167">Herhangi bir HTTP fiili için işleyici metotları eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="021e2-167">Handler methods for any HTTP verb can be added.</span></span> <span data-ttu-id="021e2-168">En yaygın işleyiciler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="021e2-168">The most common handlers are:</span></span>

* <span data-ttu-id="021e2-169">sayfa için gereken durumu başlatmak için `OnGet`.</span><span class="sxs-lookup"><span data-stu-id="021e2-169">`OnGet` to initialize state needed for the page.</span></span> <span data-ttu-id="021e2-170">Yukarıdaki kodda `OnGet` yöntemi *CreateModel. cshtml* Razor sayfasını görüntüler.</span><span class="sxs-lookup"><span data-stu-id="021e2-170">In the preceding code, the `OnGet` method displays the *CreateModel.cshtml* Razor Page.</span></span>
* <span data-ttu-id="021e2-171">form gönderilerini işlemek için `OnPost`.</span><span class="sxs-lookup"><span data-stu-id="021e2-171">`OnPost` to handle form submissions.</span></span>

<span data-ttu-id="021e2-172">@No__t-0 adlandırma son eki isteğe bağlıdır, ancak genellikle zaman uyumsuz işlevler için kural tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="021e2-172">The `Async` naming suffix is optional but is often used by convention for asynchronous functions.</span></span> <span data-ttu-id="021e2-173">Yukarıdaki kod Razor Pages için tipik bir davranıştır.</span><span class="sxs-lookup"><span data-stu-id="021e2-173">The preceding code is typical for Razor Pages.</span></span>

<span data-ttu-id="021e2-174">Denetleyicileri ve görünümleri kullanarak ASP.NET uygulamaları hakkında bilginiz varsa:</span><span class="sxs-lookup"><span data-stu-id="021e2-174">If you're familiar with ASP.NET apps using controllers and views:</span></span>

* <span data-ttu-id="021e2-175">Yukarıdaki örnekteki `OnPostAsync` kodu, tipik denetleyici koduna benzer şekilde görünür.</span><span class="sxs-lookup"><span data-stu-id="021e2-175">The `OnPostAsync` code in the preceding example looks similar to typical controller code.</span></span>
* <span data-ttu-id="021e2-176">[Model bağlama](xref:mvc/models/model-binding), [doğrulama](xref:mvc/models/validation)ve eylem sonuçları gibi mvc temel elemanlarının çoğu denetleyiciler ve Razor Pages aynı şekilde çalışır.</span><span class="sxs-lookup"><span data-stu-id="021e2-176">Most of the MVC primitives like [model binding](xref:mvc/models/model-binding), [validation](xref:mvc/models/validation), and action results work the same with Controllers and Razor Pages.</span></span> 

<span data-ttu-id="021e2-177">Önceki `OnPostAsync` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="021e2-177">The previous `OnPostAsync` method:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="021e2-178">@No__t temel akışı-0:</span><span class="sxs-lookup"><span data-stu-id="021e2-178">The basic flow of `OnPostAsync`:</span></span>

<span data-ttu-id="021e2-179">Doğrulama hatalarını kontrol edin.</span><span class="sxs-lookup"><span data-stu-id="021e2-179">Check for validation errors.</span></span>

* <span data-ttu-id="021e2-180">Hata yoksa, verileri kaydedin ve yeniden yönlendirin.</span><span class="sxs-lookup"><span data-stu-id="021e2-180">If there are no errors, save the data and redirect.</span></span>
* <span data-ttu-id="021e2-181">Hatalar varsa, doğrulama iletileriyle sayfayı yeniden görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="021e2-181">If there are errors, show the page again with validation messages.</span></span> <span data-ttu-id="021e2-182">Çoğu durumda, istemci üzerinde doğrulama hataları algılanır ve sunucuya hiçbir zaman gönderilmez.</span><span class="sxs-lookup"><span data-stu-id="021e2-182">In many cases, validation errors would be detected on the client, and never submitted to the server.</span></span>

<span data-ttu-id="021e2-183">*Pages/Create. cshtml* görünüm dosyası:</span><span class="sxs-lookup"><span data-stu-id="021e2-183">The *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml)]

<span data-ttu-id="021e2-184">Sayfalardan işlenmiş HTML */Create. cshtml*:</span><span class="sxs-lookup"><span data-stu-id="021e2-184">The rendered HTML from *Pages/Create.cshtml*:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create4.html)]

<span data-ttu-id="021e2-185">Önceki kodda, formu deftere nakletme:</span><span class="sxs-lookup"><span data-stu-id="021e2-185">In the previous code, posting the form:</span></span>

* <span data-ttu-id="021e2-186">Geçerli verilerle:</span><span class="sxs-lookup"><span data-stu-id="021e2-186">With valid data:</span></span>

  * <span data-ttu-id="021e2-187">@No__t-0 işleyicisi yöntemi <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel.RedirectToPage*> yardımcı yöntemini çağırır.</span><span class="sxs-lookup"><span data-stu-id="021e2-187">The `OnPostAsync` handler method calls the <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel.RedirectToPage*> helper method.</span></span> <span data-ttu-id="021e2-188">`RedirectToPage` <xref:Microsoft.AspNetCore.Mvc.RedirectToPageResult> ' in bir örneğini döndürür.</span><span class="sxs-lookup"><span data-stu-id="021e2-188">`RedirectToPage` returns an instance of <xref:Microsoft.AspNetCore.Mvc.RedirectToPageResult>.</span></span> <span data-ttu-id="021e2-189">`RedirectToPage`:</span><span class="sxs-lookup"><span data-stu-id="021e2-189">`RedirectToPage`:</span></span>

    * <span data-ttu-id="021e2-190">Bir eylem sonucudur.</span><span class="sxs-lookup"><span data-stu-id="021e2-190">Is an action result.</span></span>
    * <span data-ttu-id="021e2-191">@No__t-0 veya `RedirectToRoute` ' e benzerdir (denetleyiciler ve görünümlerde kullanılır).</span><span class="sxs-lookup"><span data-stu-id="021e2-191">Is similar to `RedirectToAction` or `RedirectToRoute` (used in controllers and views).</span></span>
    * <span data-ttu-id="021e2-192">Sayfalar için özelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="021e2-192">Is customized for pages.</span></span> <span data-ttu-id="021e2-193">Önceki örnekte, kök dizin sayfasına (`/Index`) yeniden yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="021e2-193">In the preceding sample, it redirects to the root Index page (`/Index`).</span></span> <span data-ttu-id="021e2-194">`RedirectToPage`, [Sayfalar Için URL oluşturma](#url_gen) bölümünde ayrıntılı olarak açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="021e2-194">`RedirectToPage` is detailed in the [URL generation for Pages](#url_gen) section.</span></span>

* <span data-ttu-id="021e2-195">Sunucuya geçirilen doğrulama hatalarıyla birlikte:</span><span class="sxs-lookup"><span data-stu-id="021e2-195">With validation errors that are passed to the server:</span></span>

  * <span data-ttu-id="021e2-196">@No__t-0 işleyicisi yöntemi <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageBase.Page*> yardımcı yöntemini çağırır.</span><span class="sxs-lookup"><span data-stu-id="021e2-196">The `OnPostAsync` handler method calls the <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageBase.Page*> helper method.</span></span> <span data-ttu-id="021e2-197">`Page` <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageResult> ' in bir örneğini döndürür.</span><span class="sxs-lookup"><span data-stu-id="021e2-197">`Page` returns an instance of <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageResult>.</span></span> <span data-ttu-id="021e2-198">@No__t-0 döndürme, denetleyicilerde bulunan eylemlerin `View` ' i döndürme biçimine benzer.</span><span class="sxs-lookup"><span data-stu-id="021e2-198">Returning `Page` is similar to how actions in controllers return `View`.</span></span> <span data-ttu-id="021e2-199">`PageResult`, bir işleyici yöntemi için varsayılan dönüş türüdür.</span><span class="sxs-lookup"><span data-stu-id="021e2-199">`PageResult` is the default return type for a handler method.</span></span> <span data-ttu-id="021e2-200">@No__t-0 döndüren bir işleyici yöntemi sayfayı işler.</span><span class="sxs-lookup"><span data-stu-id="021e2-200">A handler method that returns `void` renders the page.</span></span>
  * <span data-ttu-id="021e2-201">Yukarıdaki örnekte, formun hiçbir değer olmadan nakledilmesi [ModelState ile sonuçlanır. IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) yanlış döndürüyor.</span><span class="sxs-lookup"><span data-stu-id="021e2-201">In the preceding example, posting the form with no value results in [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) returning false.</span></span> <span data-ttu-id="021e2-202">Bu örnekte, istemcide hiçbir doğrulama hatası gösterilmezler.</span><span class="sxs-lookup"><span data-stu-id="021e2-202">In this sample, no validation errors are displayed on the client.</span></span> <span data-ttu-id="021e2-203">Doğrulama hatası teslim etme bu belgenin ilerleyen bölümlerinde ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="021e2-203">Validation error handing is covered later in this document.</span></span>

  [!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=3-6)]

* <span data-ttu-id="021e2-204">İstemci tarafı doğrulaması tarafından algılanan doğrulama hatalarıyla birlikte:</span><span class="sxs-lookup"><span data-stu-id="021e2-204">With validation errors detected by client side validation:</span></span>

  * <span data-ttu-id="021e2-205">Veriler sunucuya **nakledilmedi.**</span><span class="sxs-lookup"><span data-stu-id="021e2-205">Data is **not** posted to the server.</span></span>
  * <span data-ttu-id="021e2-206">İstemci tarafı doğrulaması bu belgenin ilerleyen kısımlarında açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="021e2-206">Client-side validation is explained later in this document.</span></span>

<span data-ttu-id="021e2-207">@No__t-0 özelliği, model bağlamasını kabul etmek için [`[BindProperty]`](xref:Microsoft.AspNetCore.Mvc.BindPropertyAttribute) özniteliğini kullanır:</span><span class="sxs-lookup"><span data-stu-id="021e2-207">The `Customer` property uses [`[BindProperty]`](xref:Microsoft.AspNetCore.Mvc.BindPropertyAttribute) attribute to opt in to model binding:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml.cs?name=snippet_PageModel&highlight=15-16)]

<span data-ttu-id="021e2-208">`[BindProperty]`, istemci tarafından **değiştirilmemesi gereken özellikler** içeren modellerde kullanılmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="021e2-208">`[BindProperty]` should **not** be used on models containing properties that should not be changed by the client.</span></span> <span data-ttu-id="021e2-209">Daha fazla bilgi için bkz. fazla [nakil](xref:data/ef-rp/crud#overposting).</span><span class="sxs-lookup"><span data-stu-id="021e2-209">For more information, see [Overposting](xref:data/ef-rp/crud#overposting).</span></span>

<span data-ttu-id="021e2-210">Razor Pages, varsayılan olarak, özellikleri yalnızca @no__t olmayan-0 olan fiiller ile bağlayın.</span><span class="sxs-lookup"><span data-stu-id="021e2-210">Razor Pages, by default, bind properties only with non-`GET` verbs.</span></span> <span data-ttu-id="021e2-211">Özelliklere bağlama, HTTP verilerini model türüne dönüştürmek için kod yazma ihtiyacını ortadan kaldırır.</span><span class="sxs-lookup"><span data-stu-id="021e2-211">Binding to properties removes the need to writing code to convert HTTP data to the model type.</span></span> <span data-ttu-id="021e2-212">Bağlama, form alanlarını işlemek için aynı özelliği kullanarak kodu azaltır (`<input asp-for="Customer.Name">`) ve girişi kabul eder.</span><span class="sxs-lookup"><span data-stu-id="021e2-212">Binding reduces code by using the same property to render form fields (`<input asp-for="Customer.Name">`) and accept the input.</span></span>

[!INCLUDE[](~/includes/bind-get.md)]

<span data-ttu-id="021e2-213">*Sayfalar/oluşturma. cshtml* görünüm dosyası gözden geçiriliyor:</span><span class="sxs-lookup"><span data-stu-id="021e2-213">Reviewing the *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml?highlight=3,9)]

* <span data-ttu-id="021e2-214">Yukarıdaki kodda, `<input asp-for="Customer.Name" />` [giriş etiketi Yardımcısı](xref:mvc/views/working-with-forms#the-input-tag-helper) , HTML `<input>` öğesini `Customer.Name` model ifadesine bağlar.</span><span class="sxs-lookup"><span data-stu-id="021e2-214">In the preceding code, the [input tag helper](xref:mvc/views/working-with-forms#the-input-tag-helper) `<input asp-for="Customer.Name" />` binds the HTML `<input>` element to the `Customer.Name` model expression.</span></span>
* <span data-ttu-id="021e2-215">[`@addTagHelper`](xref:mvc/views/tag-helpers/intro#addtaghelper-makes-tag-helpers-available) etiket yardımcılarını kullanılabilir hale getirir.</span><span class="sxs-lookup"><span data-stu-id="021e2-215">[`@addTagHelper`](xref:mvc/views/tag-helpers/intro#addtaghelper-makes-tag-helpers-available) makes Tag Helpers available.</span></span>

### <a name="the-home-page"></a><span data-ttu-id="021e2-216">Giriş sayfası</span><span class="sxs-lookup"><span data-stu-id="021e2-216">The home page</span></span>

<span data-ttu-id="021e2-217">*Index. cshtml* giriş sayfasıdır:</span><span class="sxs-lookup"><span data-stu-id="021e2-217">*Index.cshtml* is the home page:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Index.cshtml)]

<span data-ttu-id="021e2-218">İlişkili `PageModel` sınıfı (*Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="021e2-218">The associated `PageModel` class (*Index.cshtml.cs*):</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Index.cshtml.cs?name=snippet)]

<span data-ttu-id="021e2-219">*Index. cshtml* dosyası aşağıdaki biçimlendirmeyi içerir:</span><span class="sxs-lookup"><span data-stu-id="021e2-219">The *Index.cshtml* file contains the following markup:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Index.cshtml?range=21)]

<span data-ttu-id="021e2-220">@No__t-0 [bağlantı etiketi Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) , düzenleme sayfasına bir bağlantı oluşturmak için `asp-route-{value}` özniteliğini kullandı.</span><span class="sxs-lookup"><span data-stu-id="021e2-220">The `<a /a>` [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) used the `asp-route-{value}` attribute to generate a link to the Edit page.</span></span> <span data-ttu-id="021e2-221">Bağlantı, iletişim KIMLIĞINE sahip rota verileri içerir.</span><span class="sxs-lookup"><span data-stu-id="021e2-221">The link contains route data with the contact ID.</span></span> <span data-ttu-id="021e2-222">Örneğin, `https://localhost:5001/Edit/1`.</span><span class="sxs-lookup"><span data-stu-id="021e2-222">For example, `https://localhost:5001/Edit/1`.</span></span> <span data-ttu-id="021e2-223">[Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro), Razor dosyalarında HTML öğelerinin oluşturulmasına ve işlenmesine sunucu tarafı kodun katılmasını etkinleştir.</span><span class="sxs-lookup"><span data-stu-id="021e2-223">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span>

<span data-ttu-id="021e2-224">*Index. cshtml* dosyası her müşteri için bir silme düğmesi oluşturmak için biçimlendirme içerir:</span><span class="sxs-lookup"><span data-stu-id="021e2-224">The *Index.cshtml* file contains markup to create a delete button for each customer contact:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Index.cshtml?range=22-23)]

<span data-ttu-id="021e2-225">İşlenmiş HTML:</span><span class="sxs-lookup"><span data-stu-id="021e2-225">The rendered HTML:</span></span>

```HTML
<button type="submit" formaction="/Customers?id=1&amp;handler=delete">delete</button>
```

<span data-ttu-id="021e2-226">Sil düğmesi HTML 'de işlendiğinde, bu nesnenin [biçimlendirme](https://developer.mozilla.org/docs/Web/HTML/Element/button#attr-formaction) parametreleri içerir:</span><span class="sxs-lookup"><span data-stu-id="021e2-226">When the delete button is rendered in HTML, its [formaction](https://developer.mozilla.org/docs/Web/HTML/Element/button#attr-formaction) includes parameters for:</span></span>

* <span data-ttu-id="021e2-227">@No__t-0 özniteliğiyle belirtilen müşteri iletişim KIMLIĞI.</span><span class="sxs-lookup"><span data-stu-id="021e2-227">The customer contact ID, specified by the `asp-route-id` attribute.</span></span>
* <span data-ttu-id="021e2-228">@No__t-1 özniteliğiyle belirtilen `handler`.</span><span class="sxs-lookup"><span data-stu-id="021e2-228">The `handler`, specified by the `asp-page-handler` attribute.</span></span>

<span data-ttu-id="021e2-229">Düğme seçildiğinde, sunucuya `POST` isteği gönderilir.</span><span class="sxs-lookup"><span data-stu-id="021e2-229">When the button is selected, a form `POST` request is sent to the server.</span></span> <span data-ttu-id="021e2-230">Kural gereği, işleyici yönteminin adı, `OnPost[handler]Async` düzenine göre `handler` parametresinin değerine göre seçilir.</span><span class="sxs-lookup"><span data-stu-id="021e2-230">By convention, the name of the handler method is selected based on the value of the `handler` parameter according to the scheme `OnPost[handler]Async`.</span></span>

<span data-ttu-id="021e2-231">Bu örnekte `handler` `delete` olduğundan, `POST` isteğini işlemek için `OnPostDeleteAsync` işleyici yöntemi kullanılır.</span><span class="sxs-lookup"><span data-stu-id="021e2-231">Because the `handler` is `delete` in this example, the `OnPostDeleteAsync` handler method is used to process the `POST` request.</span></span> <span data-ttu-id="021e2-232">@No__t-0, `remove` gibi farklı bir değere ayarlanmışsa, `OnPostRemoveAsync` adlı bir işleyici yöntemi seçilidir.</span><span class="sxs-lookup"><span data-stu-id="021e2-232">If the `asp-page-handler` is set to a different value, such as `remove`, a handler method with the name `OnPostRemoveAsync` is selected.</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Index.cshtml.cs?name=snippet2)]

<span data-ttu-id="021e2-233">`OnPostDeleteAsync` Yöntemi:</span><span class="sxs-lookup"><span data-stu-id="021e2-233">The `OnPostDeleteAsync` method:</span></span>

* <span data-ttu-id="021e2-234">Sorgu dizesinden `id` alır.</span><span class="sxs-lookup"><span data-stu-id="021e2-234">Gets the `id` from the query string.</span></span>
* <span data-ttu-id="021e2-235">@No__t-0 ile müşteri iletişim için veritabanını sorgular.</span><span class="sxs-lookup"><span data-stu-id="021e2-235">Queries the database for the customer contact with `FindAsync`.</span></span>
* <span data-ttu-id="021e2-236">Müşteri ilgili kişisi bulunursa, kaldırılır ve veritabanı güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="021e2-236">If the customer contact is found, it's removed and the database is updated.</span></span>
* <span data-ttu-id="021e2-237">Kök dizin sayfasına yeniden yönlendirmek için <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel.RedirectToPage*> çağırır (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="021e2-237">Calls <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel.RedirectToPage*> to redirect to the root Index page (`/Index`).</span></span>

### <a name="the-editcshtml-file"></a><span data-ttu-id="021e2-238">Edit. cshtml dosyası</span><span class="sxs-lookup"><span data-stu-id="021e2-238">The Edit.cshtml file</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Edit.cshtml?highlight=1)]

<span data-ttu-id="021e2-239">İlk satır `@page "{id:int}"` yönergesini içerir.</span><span class="sxs-lookup"><span data-stu-id="021e2-239">The first line contains the `@page "{id:int}"` directive.</span></span> <span data-ttu-id="021e2-240">{No__t-0 Yönlendirme kısıtlaması, sayfaya `int` rota verileri içeren sayfaya istekleri kabul etmesini söyler.</span><span class="sxs-lookup"><span data-stu-id="021e2-240">The routing constraint`"{id:int}"` tells the page to accept requests to the page that contain `int` route data.</span></span> <span data-ttu-id="021e2-241">Sayfaya yapılan bir istek bir `int` ' a dönüştürülebileceği rota verileri içermiyorsa, çalışma zamanı bir HTTP 404 (bulunamadı) hatası döndürür.</span><span class="sxs-lookup"><span data-stu-id="021e2-241">If a request to the page doesn't contain route data that can be converted to an `int`, the runtime returns an HTTP 404 (not found) error.</span></span> <span data-ttu-id="021e2-242">KIMLIĞI isteğe bağlı yapmak için, yol kısıtlamasına `?` ekleyin:</span><span class="sxs-lookup"><span data-stu-id="021e2-242">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="021e2-243">*Edit.cshtml.cs* dosyası:</span><span class="sxs-lookup"><span data-stu-id="021e2-243">The *Edit.cshtml.cs* file:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Edit.cshtml.cs?name=snippet)]

## <a name="validation"></a><span data-ttu-id="021e2-244">Doğrulama</span><span class="sxs-lookup"><span data-stu-id="021e2-244">Validation</span></span>

<span data-ttu-id="021e2-245">Doğrulama kuralları:</span><span class="sxs-lookup"><span data-stu-id="021e2-245">Validation rules:</span></span>

* <span data-ttu-id="021e2-246">Model sınıfında bildirimli olarak belirtilir.</span><span class="sxs-lookup"><span data-stu-id="021e2-246">Are declaratively specified in the model class.</span></span>
* <span data-ttu-id="021e2-247">Uygulamada her yerde zorlanır.</span><span class="sxs-lookup"><span data-stu-id="021e2-247">Are enforced everywhere in the app.</span></span>

<span data-ttu-id="021e2-248">@No__t-0 ad alanı, bir sınıfa veya özelliğe bildirimli olarak uygulanan bir yerleşik doğrulama öznitelikleri kümesi sağlar.</span><span class="sxs-lookup"><span data-stu-id="021e2-248">The <xref:System.ComponentModel.DataAnnotations> namespace provides a set of built-in validation attributes that are applied declaratively to a class or property.</span></span> <span data-ttu-id="021e2-249">Veri açıklamaları, biçimlendirme ile yardım eden [`[DataType]`](xref:System.ComponentModel.DataAnnotations.DataTypeAttribute) gibi biçimlendirme özniteliklerini de içerir ve herhangi bir doğrulama sağlamaz.</span><span class="sxs-lookup"><span data-stu-id="021e2-249">DataAnnotations also contains formatting attributes like [`[DataType]`](xref:System.ComponentModel.DataAnnotations.DataTypeAttribute) that help with formatting and don't provide any validation.</span></span>

<span data-ttu-id="021e2-250">@No__t-0 modelini göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="021e2-250">Consider the `Customer` model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Data/Customer.cs)]

<span data-ttu-id="021e2-251">Aşağıdaki *Create. cshtml* görünüm dosyasını kullanarak:</span><span class="sxs-lookup"><span data-stu-id="021e2-251">Using the following *Create.cshtml* view file:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create3.cshtml?highlight=3,8-9,15-99)]

<span data-ttu-id="021e2-252">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="021e2-252">The preceding code:</span></span>

* <span data-ttu-id="021e2-253">JQuery ve jQuery doğrulama betikleri içerir.</span><span class="sxs-lookup"><span data-stu-id="021e2-253">Includes jQuery and jQuery validation scripts.</span></span>
* <span data-ttu-id="021e2-254">Etkinleştirmek için `<div />` ve `<span />` [Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro) kullanır:</span><span class="sxs-lookup"><span data-stu-id="021e2-254">Uses the `<div />` and `<span />` [Tag Helpers](xref:mvc/views/tag-helpers/intro) to enable:</span></span>

  * <span data-ttu-id="021e2-255">İstemci tarafı doğrulama.</span><span class="sxs-lookup"><span data-stu-id="021e2-255">Client-side validation.</span></span>
  * <span data-ttu-id="021e2-256">Doğrulama hatası işleme.</span><span class="sxs-lookup"><span data-stu-id="021e2-256">Validation error rendering.</span></span>

* <span data-ttu-id="021e2-257">Aşağıdaki HTML 'yi oluşturur:</span><span class="sxs-lookup"><span data-stu-id="021e2-257">Generates the following HTML:</span></span>

  [!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create5.html)]

<span data-ttu-id="021e2-258">Create formunu ad değeri olmadan göndermek "ad alanı gereklidir" hata iletisini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="021e2-258">Posting the Create form without a name value displays the error message "The Name field is required."</span></span> <span data-ttu-id="021e2-259">formunda.</span><span class="sxs-lookup"><span data-stu-id="021e2-259">on the form.</span></span> <span data-ttu-id="021e2-260">İstemcide JavaScript etkinse tarayıcı, sunucuya göndermeden hatayı görüntüler.</span><span class="sxs-lookup"><span data-stu-id="021e2-260">If JavaScript is enabled on the client, the browser displays the error without posting to the server.</span></span>

<span data-ttu-id="021e2-261">@No__t-0 özniteliği işlenmiş HTML üzerinde `data-val-length-max="10"` oluşturur.</span><span class="sxs-lookup"><span data-stu-id="021e2-261">The `[StringLength(10)]` attribute generates `data-val-length-max="10"` on the rendered HTML.</span></span> <span data-ttu-id="021e2-262">`data-val-length-max`, tarayıcıların belirtilen uzunluk üst sınırından daha fazlasını girmesini engeller.</span><span class="sxs-lookup"><span data-stu-id="021e2-262">`data-val-length-max` prevents browsers from entering more than the maximum length specified.</span></span> <span data-ttu-id="021e2-263">Gönderiyi düzenlemek ve yeniden oynatmak için [Fiddler](https://www.telerik.com/fiddler) gibi bir araç kullanılıyorsa:</span><span class="sxs-lookup"><span data-stu-id="021e2-263">If a tool such as [Fiddler](https://www.telerik.com/fiddler) is used to edit and replay the post:</span></span>

* <span data-ttu-id="021e2-264">, Adı 10 ' dan daha uzun.</span><span class="sxs-lookup"><span data-stu-id="021e2-264">With the name longer than 10.</span></span>
* <span data-ttu-id="021e2-265">"Alan adı, en fazla 10 uzunluğunda bir dize olmalıdır" hata iletisi.</span><span class="sxs-lookup"><span data-stu-id="021e2-265">The error message "The field Name must be a string with a maximum length of 10."</span></span> <span data-ttu-id="021e2-266">döndürülür.</span><span class="sxs-lookup"><span data-stu-id="021e2-266">is returned.</span></span>

<span data-ttu-id="021e2-267">Aşağıdaki `Movie` modelini göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="021e2-267">Consider the following `Movie` model:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Models/MovieDateRatingDA.cs?name=snippet1)]

<span data-ttu-id="021e2-268">Doğrulama öznitelikleri, uygulanan model özellikleri üzerinde zorlamak için davranışı belirtir:</span><span class="sxs-lookup"><span data-stu-id="021e2-268">The validation attributes specify behavior to enforce on the model properties they're applied to:</span></span>

* <span data-ttu-id="021e2-269">@No__t-0 ve `MinimumLength` öznitelikleri bir özelliğin bir değere sahip olması gerektiğini belirtir, ancak hiçbir şey, kullanıcının bu doğrulamayı karşılamak için boşluk girmesini engeller.</span><span class="sxs-lookup"><span data-stu-id="021e2-269">The `Required` and `MinimumLength` attributes indicate that a property must have a value, but nothing prevents a user from entering white space to satisfy this validation.</span></span>
* <span data-ttu-id="021e2-270">@No__t-0 özniteliği, hangi karakterlerin girişi yapabileceğini sınırlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="021e2-270">The `RegularExpression` attribute is used to limit what characters can be input.</span></span> <span data-ttu-id="021e2-271">Yukarıdaki kodda, "tarz":</span><span class="sxs-lookup"><span data-stu-id="021e2-271">In the preceding code, "Genre":</span></span>

  * <span data-ttu-id="021e2-272">Yalnızca harfler kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="021e2-272">Must only use letters.</span></span>
  * <span data-ttu-id="021e2-273">İlk harfin büyük harfle olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="021e2-273">The first letter is required to be uppercase.</span></span> <span data-ttu-id="021e2-274">Boşluk, sayı ve özel karakterlere izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="021e2-274">White space, numbers, and special characters are not allowed.</span></span>

* <span data-ttu-id="021e2-275">@No__t-0 "derecelendirmesi":</span><span class="sxs-lookup"><span data-stu-id="021e2-275">The `RegularExpression` "Rating":</span></span>

  * <span data-ttu-id="021e2-276">İlk karakterin büyük harf olmasını gerektirir.</span><span class="sxs-lookup"><span data-stu-id="021e2-276">Requires that the first character be an uppercase letter.</span></span>
  * <span data-ttu-id="021e2-277">Sonraki boşlukların içindeki özel karakter ve sayılara izin verir.</span><span class="sxs-lookup"><span data-stu-id="021e2-277">Allows special characters and numbers in subsequent spaces.</span></span> <span data-ttu-id="021e2-278">"PG-13" bir derecelendirme için geçerlidir, ancak bir "tarz" için başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="021e2-278">"PG-13" is valid for a rating, but fails for a "Genre".</span></span>

* <span data-ttu-id="021e2-279">@No__t-0 özniteliği, bir değeri belirtilen bir Aralık içinde kısıtlar.</span><span class="sxs-lookup"><span data-stu-id="021e2-279">The `Range` attribute constrains a value to within a specified range.</span></span>
* <span data-ttu-id="021e2-280">@No__t-0 özniteliği bir dize özelliğinin en büyük uzunluğunu ve isteğe bağlı olarak en düşük uzunluğunu ayarlar.</span><span class="sxs-lookup"><span data-stu-id="021e2-280">The `StringLength` attribute sets the maximum length of a string property, and optionally its minimum length.</span></span>
* <span data-ttu-id="021e2-281">Değer türleri (örneğin `decimal`, `int`, `float`, `DateTime`), doğal olarak gereklidir ve `[Required]` özniteliğine gerek kalmaz.</span><span class="sxs-lookup"><span data-stu-id="021e2-281">Value types (such as `decimal`, `int`, `float`, `DateTime`) are inherently required and don't need the `[Required]` attribute.</span></span>

<span data-ttu-id="021e2-282">@No__t-0 modelinin Oluştur sayfası, geçersiz değerlere sahip hataları gösterir:</span><span class="sxs-lookup"><span data-stu-id="021e2-282">The Create page for the `Movie` model shows displays errors with invalid values:</span></span>

![Birden çok jQuery istemci tarafı doğrulama hatası içeren film görünümü formu](~/tutorials/razor-pages/validation/_static/val.png)

<span data-ttu-id="021e2-284">Daha fazla bilgi için bkz.</span><span class="sxs-lookup"><span data-stu-id="021e2-284">For more information, see:</span></span>

* [<span data-ttu-id="021e2-285">Film uygulamasına doğrulama ekleme</span><span class="sxs-lookup"><span data-stu-id="021e2-285">Add validation to the Movie app</span></span>](xref:tutorials/razor-pages/validation)
* <span data-ttu-id="021e2-286">[ASP.NET Core 'de model doğrulaması](xref:mvc/models/validation).</span><span class="sxs-lookup"><span data-stu-id="021e2-286">[Model validation in ASP.NET Core](xref:mvc/models/validation).</span></span>

## <a name="handle-head-requests-with-an-onget-handler-fallback"></a><span data-ttu-id="021e2-287">OnGet işleyicisi geri dönüşü ile tanıtıcı HEAD istekleri</span><span class="sxs-lookup"><span data-stu-id="021e2-287">Handle HEAD requests with an OnGet handler fallback</span></span>

<span data-ttu-id="021e2-288">`HEAD` istekleri belirli bir kaynağın üst bilgilerini almaya izin verir.</span><span class="sxs-lookup"><span data-stu-id="021e2-288">`HEAD` requests allow retrieving the headers for a specific resource.</span></span> <span data-ttu-id="021e2-289">@No__t-0 isteklerinin aksine, `HEAD` istekleri yanıt gövdesi döndürmez.</span><span class="sxs-lookup"><span data-stu-id="021e2-289">Unlike `GET` requests, `HEAD` requests don't return a response body.</span></span>

<span data-ttu-id="021e2-290">Normalde, `HEAD` istekleri için `OnHead` işleyicisi oluşturulur ve çağırılır:</span><span class="sxs-lookup"><span data-stu-id="021e2-290">Ordinarily, an `OnHead` handler is created and called for `HEAD` requests:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Privacy.cshtml.cs?name=snippet)]

<span data-ttu-id="021e2-291">Razor Pages, `OnHead` işleyicisi tanımlanmamışsa `OnGet` işleyicisini çağırmaya geri döner.</span><span class="sxs-lookup"><span data-stu-id="021e2-291">Razor Pages falls back to calling the `OnGet` handler if no `OnHead` handler is defined.</span></span>

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a><span data-ttu-id="021e2-292">XSRF/CSRF ve Razor Pages</span><span class="sxs-lookup"><span data-stu-id="021e2-292">XSRF/CSRF and Razor Pages</span></span>

<span data-ttu-id="021e2-293">Razor Pages, [Antiforgery doğrulaması](xref:security/anti-request-forgery)tarafından korunur.</span><span class="sxs-lookup"><span data-stu-id="021e2-293">Razor Pages are protected by [Antiforgery validation](xref:security/anti-request-forgery).</span></span> <span data-ttu-id="021e2-294">[Formtaghelper](xref:mvc/views/working-with-forms#the-form-tag-helper) , antiforgery belirteçlerini HTML form öğelerine çıkartır.</span><span class="sxs-lookup"><span data-stu-id="021e2-294">The [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) injects antiforgery tokens into HTML form elements.</span></span>

<a name="layout"></a>

## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a><span data-ttu-id="021e2-295">Razor Pages ile düzenleri, partileri, şablonları ve etiket yardımcılarını kullanma</span><span class="sxs-lookup"><span data-stu-id="021e2-295">Using Layouts, partials, templates, and Tag Helpers with Razor Pages</span></span>

<span data-ttu-id="021e2-296">Sayfalar, Razor görünüm altyapısının tüm özellikleri ile çalışır.</span><span class="sxs-lookup"><span data-stu-id="021e2-296">Pages work with all the capabilities of the Razor view engine.</span></span> <span data-ttu-id="021e2-297">Düzenler, partıals, şablonlar, etiket yardımcıları, *_Viewstart. cshtml*ve *_Viewwimports. cshtml* geleneksel Razor görünümlerinde oldukları gibi çalışır.</span><span class="sxs-lookup"><span data-stu-id="021e2-297">Layouts, partials, templates, Tag Helpers, *_ViewStart.cshtml*, and *_ViewImports.cshtml* work in the same way they do for conventional Razor views.</span></span>

<span data-ttu-id="021e2-298">Bu özelliklerden bazılarının avantajlarından yararlanarak bu sayfayı declutter edelim.</span><span class="sxs-lookup"><span data-stu-id="021e2-298">Let's declutter this page by taking advantage of some of those capabilities.</span></span>

<span data-ttu-id="021e2-299">*Pages/Shared/_Layout. cshtml*öğesine bir [Düzen sayfası](xref:mvc/views/layout) ekleyin:</span><span class="sxs-lookup"><span data-stu-id="021e2-299">Add a [layout page](xref:mvc/views/layout) to *Pages/Shared/_Layout.cshtml*:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Shared/_Layout2.cshtml?hightlight=12)]

<span data-ttu-id="021e2-300">[Düzen](xref:mvc/views/layout):</span><span class="sxs-lookup"><span data-stu-id="021e2-300">The [Layout](xref:mvc/views/layout):</span></span>

* <span data-ttu-id="021e2-301">Her sayfanın yerleşimini denetler (sayfa düzen dışında değilse).</span><span class="sxs-lookup"><span data-stu-id="021e2-301">Controls the layout of each page (unless the page opts out of layout).</span></span>
* <span data-ttu-id="021e2-302">JavaScript ve stil sayfaları gibi HTML yapılarını içeri aktarır.</span><span class="sxs-lookup"><span data-stu-id="021e2-302">Imports HTML structures such as JavaScript and stylesheets.</span></span>
* <span data-ttu-id="021e2-303">Razor sayfasının içerikleri `@RenderBody()` çağrıldığında işlenir.</span><span class="sxs-lookup"><span data-stu-id="021e2-303">The contents of the Razor page are rendered where `@RenderBody()` is called.</span></span>

<span data-ttu-id="021e2-304">Daha fazla bilgi için bkz. [Düzen sayfası](xref:mvc/views/layout).</span><span class="sxs-lookup"><span data-stu-id="021e2-304">For more information, see [layout page](xref:mvc/views/layout).</span></span>

<span data-ttu-id="021e2-305">[Layout](xref:mvc/views/layout#specifying-a-layout) özelliği *Pages/_viewstart. cshtml*içinde ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="021e2-305">The [Layout](xref:mvc/views/layout#specifying-a-layout) property is set in *Pages/_ViewStart.cshtml*:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]

<span data-ttu-id="021e2-306">Düzen *Sayfalar/paylaşılan* klasöründedir.</span><span class="sxs-lookup"><span data-stu-id="021e2-306">The layout is in the *Pages/Shared* folder.</span></span> <span data-ttu-id="021e2-307">Sayfalar, geçerli sayfayla aynı klasörden başlayarak diğer görünümleri (düzenler, şablonlar, parals) hiyerarşik olarak arar.</span><span class="sxs-lookup"><span data-stu-id="021e2-307">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="021e2-308">*Sayfalar/paylaşılan* klasördeki bir düzen, *Sayfalar* klasörü altındaki herhangi bir Razor sayfasından kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="021e2-308">A layout in the *Pages/Shared* folder can be used from any Razor page under the *Pages* folder.</span></span>

<span data-ttu-id="021e2-309">Düzen dosyası *Sayfalar/paylaşılan* klasörüne gitmelidir.</span><span class="sxs-lookup"><span data-stu-id="021e2-309">The layout file should go in the *Pages/Shared* folder.</span></span>

<span data-ttu-id="021e2-310">Düzen dosyasını *Görünümler/paylaşılan* klasöre **yerleştirmenizi öneririz** .</span><span class="sxs-lookup"><span data-stu-id="021e2-310">We recommend you **not** put the layout file in the *Views/Shared* folder.</span></span> <span data-ttu-id="021e2-311">*Görünümler/paylaşılan* bir MVC görünümleri modelidir.</span><span class="sxs-lookup"><span data-stu-id="021e2-311">*Views/Shared* is an MVC views pattern.</span></span> <span data-ttu-id="021e2-312">Razor Pages, yol kurallarını değil klasör hiyerarşisine güvenmektir.</span><span class="sxs-lookup"><span data-stu-id="021e2-312">Razor Pages are meant to rely on folder hierarchy, not path conventions.</span></span>

<span data-ttu-id="021e2-313">Bir Razor sayfasından arama görüntüleme, *Sayfalar* klasörünü içerir.</span><span class="sxs-lookup"><span data-stu-id="021e2-313">View search from a Razor Page includes the *Pages* folder.</span></span> <span data-ttu-id="021e2-314">MVC denetleyicileri ve geleneksel Razor görünümleriyle kullanılan düzenler, şablonlar ve partilar *yalnızca çalışır*.</span><span class="sxs-lookup"><span data-stu-id="021e2-314">The layouts, templates, and partials used with MVC controllers and conventional Razor views *just work*.</span></span>

<span data-ttu-id="021e2-315">*Pages/_Viewwimports. cshtml* dosyası ekleyin:</span><span class="sxs-lookup"><span data-stu-id="021e2-315">Add a *Pages/_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]

<span data-ttu-id="021e2-316">`@namespace` daha sonra öğreticide açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="021e2-316">`@namespace` is explained later in the tutorial.</span></span> <span data-ttu-id="021e2-317">@No__t-0 yönergesi, [yerleşik etiket yardımcılarını](xref:mvc/views/tag-helpers/builtin-th/Index) *Sayfalar* klasöründeki tüm sayfalara getirir.</span><span class="sxs-lookup"><span data-stu-id="021e2-317">The `@addTagHelper` directive brings in the [built-in Tag Helpers](xref:mvc/views/tag-helpers/builtin-th/Index) to all the pages in the *Pages* folder.</span></span>

<a name="namespace"></a>

<span data-ttu-id="021e2-318">Bir sayfada ayarlanan `@namespace` yönergesi:</span><span class="sxs-lookup"><span data-stu-id="021e2-318">The `@namespace` directive set on a page:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]

<span data-ttu-id="021e2-319">@No__t-0 yönergesi sayfanın ad alanını ayarlar.</span><span class="sxs-lookup"><span data-stu-id="021e2-319">The `@namespace` directive sets the namespace for the page.</span></span> <span data-ttu-id="021e2-320">@No__t-0 yönergesinin ad alanını içermesi gerekmez.</span><span class="sxs-lookup"><span data-stu-id="021e2-320">The `@model` directive doesn't need to include the namespace.</span></span>

<span data-ttu-id="021e2-321">@No__t-0 yönergesi *_Viewwimports. cshtml*içinde yer aldığında, belirtilen ad alanı, `@namespace` yönergesini Içeri aktaran sayfada oluşturulan ad alanı için ön ek sağlar.</span><span class="sxs-lookup"><span data-stu-id="021e2-321">When the `@namespace` directive is contained in *_ViewImports.cshtml*, the specified namespace supplies the prefix for the generated namespace in the Page that imports the `@namespace` directive.</span></span> <span data-ttu-id="021e2-322">Oluşturulan ad alanının geri kalanı (sonek bölümü), *_Viewwimports. cshtml* dosyasını ve sayfayı içeren klasörü içeren, noktayla ayrılmış göreli yoldur.</span><span class="sxs-lookup"><span data-stu-id="021e2-322">The rest of the generated namespace (the suffix portion) is the dot-separated relative path between the folder containing *_ViewImports.cshtml* and the folder containing the page.</span></span>

<span data-ttu-id="021e2-323">Örneğin, `PageModel` sınıf *sayfaları/müşteriler/Edit. cshtml. cs* açıkça ad alanını ayarlar:</span><span class="sxs-lookup"><span data-stu-id="021e2-323">For example, the `PageModel` class *Pages/Customers/Edit.cshtml.cs* explicitly sets the namespace:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=snippet_namespace)]

<span data-ttu-id="021e2-324">*Pages/_Viewwimports. cshtml* dosyası aşağıdaki ad alanını ayarlar:</span><span class="sxs-lookup"><span data-stu-id="021e2-324">The *Pages/_ViewImports.cshtml* file sets the following namespace:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]

<span data-ttu-id="021e2-325">*Pages/Customers/Edit. cshtml* Razor sayfasının oluşturulan ad alanı `PageModel` sınıfıyla aynıdır.</span><span class="sxs-lookup"><span data-stu-id="021e2-325">The generated namespace for the *Pages/Customers/Edit.cshtml* Razor Page is the same as the `PageModel` class.</span></span>

<span data-ttu-id="021e2-326">`@namespace` *geleneksel Razor görünümleriyle de kullanılabilir.*</span><span class="sxs-lookup"><span data-stu-id="021e2-326">`@namespace` *also works with conventional Razor views.*</span></span>

<span data-ttu-id="021e2-327">*Pages/Create. cshtml* görünüm dosyasını göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="021e2-327">Consider the *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create3.cshtml?highlight=2-3)]

<span data-ttu-id="021e2-328">Güncelleştirilmiş *Sayfalar/Create. cshtml* görünüm dosyası *_Viewwimports. cshtml* ve önceki düzen dosyası:</span><span class="sxs-lookup"><span data-stu-id="021e2-328">The updated *Pages/Create.cshtml* view file with *_ViewImports.cshtml* and the preceding layout file:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create4.cshtml?highlight=2)]

<span data-ttu-id="021e2-329">Yukarıdaki kodda, *_Viewwimports. cshtml* ad alanını ve etiket yardımcıları içeri aktardı.</span><span class="sxs-lookup"><span data-stu-id="021e2-329">In the preceding code, the *_ViewImports.cshtml* imported the namespace and Tag Helpers.</span></span> <span data-ttu-id="021e2-330">Düzen dosyası JavaScript dosyalarını içeri aktardı.</span><span class="sxs-lookup"><span data-stu-id="021e2-330">The layout file imported the JavaScript files.</span></span>

<span data-ttu-id="021e2-331">[Razor Pages Başlatıcı projesi](#rpvs17) , istemci tarafı doğrulamayı bağlayan *sayfaları/_ValidationScriptsPartial. cshtml*'yi içerir.</span><span class="sxs-lookup"><span data-stu-id="021e2-331">The [Razor Pages starter project](#rpvs17) contains the *Pages/_ValidationScriptsPartial.cshtml*, which hooks up client-side validation.</span></span>

<span data-ttu-id="021e2-332">Kısmi görünümler hakkında daha fazla bilgi için bkz. <xref:mvc/views/partial>.</span><span class="sxs-lookup"><span data-stu-id="021e2-332">For more information on partial views, see <xref:mvc/views/partial>.</span></span>

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a><span data-ttu-id="021e2-333">Sayfalar için URL oluşturma</span><span class="sxs-lookup"><span data-stu-id="021e2-333">URL generation for Pages</span></span>

<span data-ttu-id="021e2-334">Daha önce gösterilen `Create` sayfası `RedirectToPage` kullanır:</span><span class="sxs-lookup"><span data-stu-id="021e2-334">The `Create` page, shown previously, uses `RedirectToPage`:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml.cs?name=snippet_PageModel&highlight=28)]

<span data-ttu-id="021e2-335">Uygulama aşağıdaki dosya/klasör yapısına sahiptir:</span><span class="sxs-lookup"><span data-stu-id="021e2-335">The app has the following file/folder structure:</span></span>

* <span data-ttu-id="021e2-336">*/Pages*</span><span class="sxs-lookup"><span data-stu-id="021e2-336">*/Pages*</span></span>

  * <span data-ttu-id="021e2-337">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="021e2-337">*Index.cshtml*</span></span>
  * <span data-ttu-id="021e2-338">*Gizlilik. cshtml*</span><span class="sxs-lookup"><span data-stu-id="021e2-338">*Privacy.cshtml*</span></span>
  * <span data-ttu-id="021e2-339">*/Customers*</span><span class="sxs-lookup"><span data-stu-id="021e2-339">*/Customers*</span></span>

    * <span data-ttu-id="021e2-340">*. Cshtml oluştur*</span><span class="sxs-lookup"><span data-stu-id="021e2-340">*Create.cshtml*</span></span>
    * <span data-ttu-id="021e2-341">*Edit.cshtml*</span><span class="sxs-lookup"><span data-stu-id="021e2-341">*Edit.cshtml*</span></span>
    * <span data-ttu-id="021e2-342">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="021e2-342">*Index.cshtml*</span></span>

<span data-ttu-id="021e2-343">*Pages/Customers/Create. cshtml* ve *Pages/Customers/Edit. cshtml* sayfaları, başarılı olduktan sonra *sayfaları/müşterileri/Index. cshtml* 'ye yeniden yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="021e2-343">The *Pages/Customers/Create.cshtml* and *Pages/Customers/Edit.cshtml* pages redirect to *Pages/Customers/Index.cshtml* after success.</span></span> <span data-ttu-id="021e2-344">@No__t-0 dizesi, önceki sayfaya erişmek için kullanılan göreli bir sayfa adıdır.</span><span class="sxs-lookup"><span data-stu-id="021e2-344">The string `./Index` is a relative page name used to access the preceding page.</span></span> <span data-ttu-id="021e2-345">*Pages/Customers/Index. cshtml* sayfasının URL 'leri oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="021e2-345">It is used to generate URLs to the *Pages/Customers/Index.cshtml* page.</span></span> <span data-ttu-id="021e2-346">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="021e2-346">For example:</span></span>

* `Url.Page("./Index", ...)`
* `<a asp-page="./Index">Customers Index Page</a>`
* `RedirectToPage("./Index")`

<span data-ttu-id="021e2-347">@No__t-0 mutlak sayfa adı, *Sayfalar/Index. cshtml* sayfasına URL 'ler oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="021e2-347">The absolute page name `/Index` is used to generate URLs to the *Pages/Index.cshtml* page.</span></span> <span data-ttu-id="021e2-348">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="021e2-348">For example:</span></span>

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">Home Index Page</a>`
* `RedirectToPage("/Index")`

<span data-ttu-id="021e2-349">Sayfa adı, kök */Pages* klasöründeki, önde gelen `/` (örneğin, `/Index`) içeren sayfanın yoludur.</span><span class="sxs-lookup"><span data-stu-id="021e2-349">The page name is the path to the page from the root */Pages* folder including a leading `/` (for example, `/Index`).</span></span> <span data-ttu-id="021e2-350">Önceki URL oluşturma örnekleri, bir URL 'YI sabit kodlamadan gelişmiş seçenekler ve işlevsel yetenekler sunar.</span><span class="sxs-lookup"><span data-stu-id="021e2-350">The preceding URL generation samples offer enhanced options and functional capabilities over hard-coding a URL.</span></span> <span data-ttu-id="021e2-351">URL oluşturma [yönlendirme](xref:mvc/controllers/routing) kullanır ve yolun hedef yolda nasıl tanımlandığınıza göre parametreleri oluşturabilir ve kodlayabilir.</span><span class="sxs-lookup"><span data-stu-id="021e2-351">URL generation uses [routing](xref:mvc/controllers/routing) and can generate and encode parameters according to how the route is defined in the destination path.</span></span>

<span data-ttu-id="021e2-352">Sayfalar için URL oluşturma göreli adları destekler.</span><span class="sxs-lookup"><span data-stu-id="021e2-352">URL generation for pages supports relative names.</span></span> <span data-ttu-id="021e2-353">Aşağıdaki tabloda, *sayfalarda/müşteriler/Create. cshtml*'de farklı `RedirectToPage` parametreleri kullanılarak hangi dizin sayfasının seçildiği gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="021e2-353">The following table shows which Index page is selected using different `RedirectToPage` parameters in *Pages/Customers/Create.cshtml*.</span></span>

| <span data-ttu-id="021e2-354">RedirectToPage (x)</span><span class="sxs-lookup"><span data-stu-id="021e2-354">RedirectToPage(x)</span></span>| <span data-ttu-id="021e2-355">Sayfasında</span><span class="sxs-lookup"><span data-stu-id="021e2-355">Page</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="021e2-356">RedirectToPage ("/Index")</span><span class="sxs-lookup"><span data-stu-id="021e2-356">RedirectToPage("/Index")</span></span> | <span data-ttu-id="021e2-357">*Sayfa/dizin*</span><span class="sxs-lookup"><span data-stu-id="021e2-357">*Pages/Index*</span></span> |
| <span data-ttu-id="021e2-358">RedirectToPage ("./Index");</span><span class="sxs-lookup"><span data-stu-id="021e2-358">RedirectToPage("./Index");</span></span> | <span data-ttu-id="021e2-359">*Sayfalar/müşteriler/Dizin*</span><span class="sxs-lookup"><span data-stu-id="021e2-359">*Pages/Customers/Index*</span></span> |
| <span data-ttu-id="021e2-360">RedirectToPage (".. /İndex ")</span><span class="sxs-lookup"><span data-stu-id="021e2-360">RedirectToPage("../Index")</span></span> | <span data-ttu-id="021e2-361">*Sayfa/dizin*</span><span class="sxs-lookup"><span data-stu-id="021e2-361">*Pages/Index*</span></span> |
| <span data-ttu-id="021e2-362">RedirectToPage ("Dizin")</span><span class="sxs-lookup"><span data-stu-id="021e2-362">RedirectToPage("Index")</span></span>  | <span data-ttu-id="021e2-363">*Sayfalar/müşteriler/Dizin*</span><span class="sxs-lookup"><span data-stu-id="021e2-363">*Pages/Customers/Index*</span></span> |

<!-- Test via ~/razor-pages/index/3.0sample/RazorPagesContacts/Pages/Customers/Details.cshtml.cs -->

<span data-ttu-id="021e2-364">`RedirectToPage("Index")`, `RedirectToPage("./Index")` ve `RedirectToPage("../Index")` *göreli adlardır*.</span><span class="sxs-lookup"><span data-stu-id="021e2-364">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, and `RedirectToPage("../Index")` are *relative names*.</span></span> <span data-ttu-id="021e2-365">@No__t-0 parametresi, hedef sayfanın adını hesaplamak için geçerli sayfanın yoluyla *birleştirilir* .</span><span class="sxs-lookup"><span data-stu-id="021e2-365">The `RedirectToPage` parameter is *combined* with the path of the current page to compute the name of the destination page.</span></span>

<span data-ttu-id="021e2-366">Karmaşık bir yapıya sahip siteler oluştururken göreli ad bağlama yararlı olur.</span><span class="sxs-lookup"><span data-stu-id="021e2-366">Relative name linking is useful when building sites with a complex structure.</span></span> <span data-ttu-id="021e2-367">Bir klasördeki sayfalar arasında bağlantı için göreli adlar kullanıldığında:</span><span class="sxs-lookup"><span data-stu-id="021e2-367">When relative names are used to link between pages in a folder:</span></span>

* <span data-ttu-id="021e2-368">Bir klasörü yeniden adlandırmak, göreli bağlantıları bozmaz.</span><span class="sxs-lookup"><span data-stu-id="021e2-368">Renaming a folder doesn't break the relative links.</span></span>
* <span data-ttu-id="021e2-369">Klasör adını içermediği için bağlantılar kopuk değildir.</span><span class="sxs-lookup"><span data-stu-id="021e2-369">Links are not broken because they don't include the folder name.</span></span>

<span data-ttu-id="021e2-370">Farklı bir [alandaki](xref:mvc/controllers/areas)bir sayfaya yeniden yönlendirmek için alanını belirtin:</span><span class="sxs-lookup"><span data-stu-id="021e2-370">To redirect to a page in a different [Area](xref:mvc/controllers/areas), specify the area:</span></span>

```csharp
RedirectToPage("/Index", new { area = "Services" });
```

<span data-ttu-id="021e2-371">Daha fazla bilgi için bkz. <xref:mvc/controllers/areas> ve <xref:razor-pages/razor-pages-conventions>.</span><span class="sxs-lookup"><span data-stu-id="021e2-371">For more information, see <xref:mvc/controllers/areas> and <xref:razor-pages/razor-pages-conventions>.</span></span>

## <a name="viewdata-attribute"></a><span data-ttu-id="021e2-372">ViewData özniteliği</span><span class="sxs-lookup"><span data-stu-id="021e2-372">ViewData attribute</span></span>

<span data-ttu-id="021e2-373">Veriler, <xref:Microsoft.AspNetCore.Mvc.ViewDataAttribute> ile bir sayfaya geçirilebilir.</span><span class="sxs-lookup"><span data-stu-id="021e2-373">Data can be passed to a page with <xref:Microsoft.AspNetCore.Mvc.ViewDataAttribute>.</span></span> <span data-ttu-id="021e2-374">@No__t-0 özniteliğiyle birlikte bulunan özellikler, değerleri <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.ViewDataDictionary> ' den depolanır ve yüklenir.</span><span class="sxs-lookup"><span data-stu-id="021e2-374">Properties with the `[ViewData]` attribute have their values stored and loaded from the <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.ViewDataDictionary>.</span></span>

<span data-ttu-id="021e2-375">Aşağıdaki örnekte `AboutModel`, `Title` özelliğine `[ViewData]` özniteliğini uygular:</span><span class="sxs-lookup"><span data-stu-id="021e2-375">In the following example, the `AboutModel` applies the `[ViewData]` attribute to the `Title` property:</span></span>

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

<span data-ttu-id="021e2-376">Hakkında sayfasında, `Title` özelliğine model özelliği olarak erişin:</span><span class="sxs-lookup"><span data-stu-id="021e2-376">In the About page, access the `Title` property as a model property:</span></span>

```cshtml
<h1>@Model.Title</h1>
```

<span data-ttu-id="021e2-377">Mizanpajda, başlık ViewData sözlüğünden okundu:</span><span class="sxs-lookup"><span data-stu-id="021e2-377">In the layout, the title is read from the ViewData dictionary:</span></span>

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"] - WebApplication</title>
    ...
```

## <a name="tempdata"></a><span data-ttu-id="021e2-378">TempData</span><span class="sxs-lookup"><span data-stu-id="021e2-378">TempData</span></span>

<span data-ttu-id="021e2-379">ASP.NET Core, @no__t gösterir.</span><span class="sxs-lookup"><span data-stu-id="021e2-379">ASP.NET Core exposes the <xref:Microsoft.AspNetCore.Mvc.Controller.TempData>.</span></span> <span data-ttu-id="021e2-380">Bu özellik, okunana kadar verileri depolar.</span><span class="sxs-lookup"><span data-stu-id="021e2-380">This property stores data until it's read.</span></span> <span data-ttu-id="021e2-381">@No__t-0 ve <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.TempDataDictionary.Peek*> yöntemleri, silme yapılmadan verileri incelemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="021e2-381">The <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.TempDataDictionary.Keep*> and <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.TempDataDictionary.Peek*> methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="021e2-382">`TempData`, tek bir istekten daha fazla veri gerektiğinde yeniden yönlendirme için kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="021e2-382">`TempData` is useful for redirection, when data is needed for more than a single request.</span></span>

<span data-ttu-id="021e2-383">Aşağıdaki kod, `TempData` kullanarak `Message` değerini ayarlar:</span><span class="sxs-lookup"><span data-stu-id="021e2-383">The following code sets the value of `Message` using `TempData`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,25&name=snippet_Temp)]

<span data-ttu-id="021e2-384">*Pages/Customers/Index. cshtml* dosyasında aşağıdaki biçimlendirme `Message` değerini `TempData` kullanarak görüntüler.</span><span class="sxs-lookup"><span data-stu-id="021e2-384">The following markup in the *Pages/Customers/Index.cshtml* file displays the value of `Message` using `TempData`.</span></span>

```cshtml
<h3>Msg: @Model.Message</h3>
```

<span data-ttu-id="021e2-385">*Pages/Customers/Index. cshtml. cs* sayfa modeli, `[TempData]` özniteliğini `Message` özelliğine uygular.</span><span class="sxs-lookup"><span data-stu-id="021e2-385">The *Pages/Customers/Index.cshtml.cs* page model applies the `[TempData]` attribute to the `Message` property.</span></span>

```cs
[TempData]
public string Message { get; set; }
```

<span data-ttu-id="021e2-386">Daha fazla bilgi için bkz. [TempData](xref:fundamentals/app-state#tempdata).</span><span class="sxs-lookup"><span data-stu-id="021e2-386">For more information, see [TempData](xref:fundamentals/app-state#tempdata).</span></span>

<a name="mhpp"></a>

## <a name="multiple-handlers-per-page"></a><span data-ttu-id="021e2-387">Sayfa başına birden çok işleyici</span><span class="sxs-lookup"><span data-stu-id="021e2-387">Multiple handlers per page</span></span>

<span data-ttu-id="021e2-388">Aşağıdaki sayfa `asp-page-handler` etiketi Yardımcısını kullanarak iki işleyici için biçimlendirme oluşturur:</span><span class="sxs-lookup"><span data-stu-id="021e2-388">The following page generates markup for two handlers using the `asp-page-handler` Tag Helper:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

<span data-ttu-id="021e2-389">Yukarıdaki örnekteki formda, her biri farklı bir URL 'ye göndermek için `FormActionTagHelper` kullanan iki gönderme düğmesi vardır.</span><span class="sxs-lookup"><span data-stu-id="021e2-389">The form in the preceding example has two submit buttons, each using the `FormActionTagHelper` to submit to a different URL.</span></span> <span data-ttu-id="021e2-390">@No__t-0 özniteliği, `asp-page` ' e yönelik bir yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="021e2-390">The `asp-page-handler` attribute is a companion to `asp-page`.</span></span> <span data-ttu-id="021e2-391">`asp-page-handler`, bir sayfa tarafından tanımlanan her bir işleyici yöntemini gönderen URL 'Ler oluşturur.</span><span class="sxs-lookup"><span data-stu-id="021e2-391">`asp-page-handler` generates URLs that submit to each of the handler methods defined by a page.</span></span> <span data-ttu-id="021e2-392">örnek geçerli sayfaya bağlandığından `asp-page` belirtilmedi.</span><span class="sxs-lookup"><span data-stu-id="021e2-392">`asp-page` isn't specified because the sample is linking to the current page.</span></span>

<span data-ttu-id="021e2-393">Sayfa modeli:</span><span class="sxs-lookup"><span data-stu-id="021e2-393">The page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]

<span data-ttu-id="021e2-394">Yukarıdaki kod, *adlandırılmış işleyici yöntemlerini*kullanır.</span><span class="sxs-lookup"><span data-stu-id="021e2-394">The preceding code uses *named handler methods*.</span></span> <span data-ttu-id="021e2-395">Adlandırılmış işleyici yöntemleri, `On<HTTP Verb>` ' dan sonra ve `Async` ' den (varsa) önce, ad içindeki metin alınarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="021e2-395">Named handler methods are created by taking the text in the name after `On<HTTP Verb>` and before `Async` (if present).</span></span> <span data-ttu-id="021e2-396">Yukarıdaki örnekte, Page metotları OnPost**Joinlist**Async ve onpost**Joinlıstuc**Async ' dir.</span><span class="sxs-lookup"><span data-stu-id="021e2-396">In the preceding example, the page methods are OnPost**JoinList**Async and OnPost**JoinListUC**Async.</span></span> <span data-ttu-id="021e2-397">*Onpost* ve *Async* kaldırılmış olarak, işleyici adları `JoinList` ve `JoinListUC` ' dir.</span><span class="sxs-lookup"><span data-stu-id="021e2-397">With *OnPost* and *Async* removed, the handler names are `JoinList` and `JoinListUC`.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?range=12-13)]

<span data-ttu-id="021e2-398">Yukarıdaki kodu kullanarak, `OnPostJoinListAsync` ' a gönderen URL yolu `https://localhost:5001/Customers/CreateFATH?handler=JoinList` ' dir.</span><span class="sxs-lookup"><span data-stu-id="021e2-398">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `https://localhost:5001/Customers/CreateFATH?handler=JoinList`.</span></span> <span data-ttu-id="021e2-399">@No__t-0 ' a gönderen URL yolu `https://localhost:5001/Customers/CreateFATH?handler=JoinListUC` ' dir.</span><span class="sxs-lookup"><span data-stu-id="021e2-399">The URL path that submits to `OnPostJoinListUCAsync` is `https://localhost:5001/Customers/CreateFATH?handler=JoinListUC`.</span></span>

## <a name="custom-routes"></a><span data-ttu-id="021e2-400">Özel yollar</span><span class="sxs-lookup"><span data-stu-id="021e2-400">Custom routes</span></span>

<span data-ttu-id="021e2-401">@No__t-0 yönergesini kullanarak şunları yapın:</span><span class="sxs-lookup"><span data-stu-id="021e2-401">Use the `@page` directive to:</span></span>

* <span data-ttu-id="021e2-402">Sayfaya özel bir yol belirtin.</span><span class="sxs-lookup"><span data-stu-id="021e2-402">Specify a custom route to a page.</span></span> <span data-ttu-id="021e2-403">Örneğin, hakkında sayfasının yolu `@page "/Some/Other/Path"` ile `/Some/Other/Path` olarak ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="021e2-403">For example, the route to the About page can be set to `/Some/Other/Path` with `@page "/Some/Other/Path"`.</span></span>
* <span data-ttu-id="021e2-404">Kesimleri bir sayfanın varsayılan yoluna ekleyin.</span><span class="sxs-lookup"><span data-stu-id="021e2-404">Append segments to a page's default route.</span></span> <span data-ttu-id="021e2-405">Örneğin, bir "öğe" segmenti sayfanın varsayılan yoluna `@page "item"` ile eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="021e2-405">For example, an "item" segment can be added to a page's default route with `@page "item"`.</span></span>
* <span data-ttu-id="021e2-406">Bir sayfanın varsayılan yoluna parametreleri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="021e2-406">Append parameters to a page's default route.</span></span> <span data-ttu-id="021e2-407">Örneğin, `id` olan bir ID parametresi, `@page "{id}"` içeren bir sayfa için gerekli olabilir.</span><span class="sxs-lookup"><span data-stu-id="021e2-407">For example, an ID parameter, `id`, can be required for a page with `@page "{id}"`.</span></span>

<span data-ttu-id="021e2-408">Yolun başındaki bir tilde (`~`) tarafından atanan kök göreli bir yol desteklenir.</span><span class="sxs-lookup"><span data-stu-id="021e2-408">A root-relative path designated by a tilde (`~`) at the beginning of the path is supported.</span></span> <span data-ttu-id="021e2-409">Örneğin, `@page "~/Some/Other/Path"` `@page "/Some/Other/Path"` ' dir.</span><span class="sxs-lookup"><span data-stu-id="021e2-409">For example, `@page "~/Some/Other/Path"` is the same as `@page "/Some/Other/Path"`.</span></span>

<span data-ttu-id="021e2-410">URL 'deki `?handler=JoinList` sorgu dizesini, `@page "{handler?}"` yol şablonunu belirterek, `/JoinList` bir yol kesimine değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="021e2-410">You can change the query string `?handler=JoinList` in the URL to a route segment `/JoinList` by specifying the route template `@page "{handler?}"`.</span></span>

<span data-ttu-id="021e2-411">URL 'de `?handler=JoinList` sorgu dizesini beğenmezseniz, bu yolu URL 'nin yol bölümüne işleyici adını koymak için değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="021e2-411">If you don't like the query string `?handler=JoinList` in the URL, you can change the route to put the handler name in the path portion of the URL.</span></span> <span data-ttu-id="021e2-412">@No__t-0 yönergesinden sonra çift tırnak içine alınmış bir rota şablonu ekleyerek yolu özelleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="021e2-412">You can customize the route by adding a route template enclosed in double quotes after the `@page` directive.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]

<span data-ttu-id="021e2-413">Yukarıdaki kodu kullanarak, `OnPostJoinListAsync` ' a gönderen URL yolu `https://localhost:5001/Customers/CreateFATH/JoinList` ' dir.</span><span class="sxs-lookup"><span data-stu-id="021e2-413">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `https://localhost:5001/Customers/CreateFATH/JoinList`.</span></span> <span data-ttu-id="021e2-414">@No__t-0 ' a gönderen URL yolu `https://localhost:5001/Customers/CreateFATH/JoinListUC` ' dir.</span><span class="sxs-lookup"><span data-stu-id="021e2-414">The URL path that submits to `OnPostJoinListUCAsync` is `https://localhost:5001/Customers/CreateFATH/JoinListUC`.</span></span>

<span data-ttu-id="021e2-415">@No__t-0 `handler`, Route parametresinin isteğe bağlı olduğu anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="021e2-415">The `?` following `handler` means the route parameter is optional.</span></span>

## <a name="advanced-configuration-and-settings"></a><span data-ttu-id="021e2-416">Gelişmiş yapılandırma ve ayarlar</span><span class="sxs-lookup"><span data-stu-id="021e2-416">Advanced configuration and settings</span></span>

<span data-ttu-id="021e2-417">Aşağıdaki bölümlerdeki yapılandırma ve ayarlar çoğu uygulama için gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="021e2-417">The configuration and settings in following sections is not required by most apps.</span></span>

<span data-ttu-id="021e2-418">Gelişmiş seçenekleri yapılandırmak için uzantı yöntemini kullanın <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>:</span><span class="sxs-lookup"><span data-stu-id="021e2-418">To configure advanced options, use the extension method <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/StartupRPoptions.cs?name=snippet)]

<span data-ttu-id="021e2-419">Sayfalar için kök dizini ayarlamak için <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions> kullanın veya sayfalar için uygulama modeli kuralları ekleyin.</span><span class="sxs-lookup"><span data-stu-id="021e2-419">Use the <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions> to set the root directory for pages, or add application model conventions for pages.</span></span> <span data-ttu-id="021e2-420">Kurallar hakkında daha fazla bilgi için bkz. [Razor Pages yetkilendirme kuralları](xref:security/authorization/razor-pages-authorization).</span><span class="sxs-lookup"><span data-stu-id="021e2-420">For more information on conventions, see [Razor Pages authorization conventions](xref:security/authorization/razor-pages-authorization).</span></span>

<span data-ttu-id="021e2-421">Görünümleri önceden derlemek için bkz. [Razor görünüm derlemesi](xref:mvc/views/view-compilation).</span><span class="sxs-lookup"><span data-stu-id="021e2-421">To precompile views, see [Razor view compilation](xref:mvc/views/view-compilation).</span></span>

### <a name="specify-that-razor-pages-are-at-the-content-root"></a><span data-ttu-id="021e2-422">Razor Pages içerik kökünde olduğunu belirtin</span><span class="sxs-lookup"><span data-stu-id="021e2-422">Specify that Razor Pages are at the content root</span></span>

<span data-ttu-id="021e2-423">Varsayılan olarak, Razor Pages */Pages* dizininde kök olarak depolanır.</span><span class="sxs-lookup"><span data-stu-id="021e2-423">By default, Razor Pages are rooted in the */Pages* directory.</span></span> <span data-ttu-id="021e2-424">Razor Pages uygulamanın (<xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootPath>) [içerik kökünde](xref:fundamentals/index#content-root) olduğunu belirtmek için <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.WithRazorPagesAtContentRoot*> ekleyin:</span><span class="sxs-lookup"><span data-stu-id="021e2-424">Add <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.WithRazorPagesAtContentRoot*> to specify that your Razor Pages are at the [content root](xref:fundamentals/index#content-root) (<xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootPath>) of the app:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/StartupWithRazorPagesAtContentRoot.cs?name=snippet)]

### <a name="specify-that-razor-pages-are-at-a-custom-root-directory"></a><span data-ttu-id="021e2-425">Razor Pages özel kök dizinde olduğunu belirtin</span><span class="sxs-lookup"><span data-stu-id="021e2-425">Specify that Razor Pages are at a custom root directory</span></span>

<span data-ttu-id="021e2-426">Razor Pages uygulamada bir özel kök dizinde olduğunu belirtmek için @no__t ekleyin-0 (göreli bir yol sağlayın):</span><span class="sxs-lookup"><span data-stu-id="021e2-426">Add <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcCoreBuilderExtensions.WithRazorPagesRoot*> to specify that Razor Pages are at a custom root directory in the app (provide a relative path):</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/StartupWithRazorPagesRoot.cs?name=snippet)]

## <a name="additional-resources"></a><span data-ttu-id="021e2-427">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="021e2-427">Additional resources</span></span>

* <span data-ttu-id="021e2-428">Bkz. [Razor Pages kullanmaya başlama](xref:tutorials/razor-pages/razor-pages-start), bu giriş hakkında derleme</span><span class="sxs-lookup"><span data-stu-id="021e2-428">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start), which builds on this introduction</span></span>
* [<span data-ttu-id="021e2-429">Örnek kodu indirme veya görüntüleme</span><span class="sxs-lookup"><span data-stu-id="021e2-429">Download or view sample code</span></span>](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/index/3.0sample)
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

<span data-ttu-id="021e2-430">By [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Ryan şimdi ak](https://github.com/rynowak)</span><span class="sxs-lookup"><span data-stu-id="021e2-430">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Ryan Nowak](https://github.com/rynowak)</span></span>

<span data-ttu-id="021e2-431">Razor Pages, kod odaklı senaryoları daha kolay ve daha üretken hale getiren ASP.NET Core MVC 'nin yeni bir yönüdür.</span><span class="sxs-lookup"><span data-stu-id="021e2-431">Razor Pages is a new aspect of ASP.NET Core MVC that makes coding page-focused scenarios easier and more productive.</span></span>

<span data-ttu-id="021e2-432">Model-View-Controller yaklaşımını kullanan bir öğretici arıyorsanız, bkz. [ASP.NET Core MVC ile çalışmaya başlama](xref:tutorials/first-mvc-app/start-mvc).</span><span class="sxs-lookup"><span data-stu-id="021e2-432">If you're looking for a tutorial that uses the Model-View-Controller approach, see [Get started with ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span></span>

<span data-ttu-id="021e2-433">Bu belge Razor Pages bir giriş sağlar.</span><span class="sxs-lookup"><span data-stu-id="021e2-433">This document provides an introduction to Razor Pages.</span></span> <span data-ttu-id="021e2-434">Adım adım öğretici değildir.</span><span class="sxs-lookup"><span data-stu-id="021e2-434">It's not a step by step tutorial.</span></span> <span data-ttu-id="021e2-435">Bölümlerden bazılarını çok gelişmiş bir şekilde bulursanız, bkz. [Razor Pages kullanmaya başlama](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="021e2-435">If you find some of the sections too advanced, see [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span></span> <span data-ttu-id="021e2-436">ASP.NET Core genel bir bakış için bkz. [ASP.NET Core giriş](xref:index).</span><span class="sxs-lookup"><span data-stu-id="021e2-436">For an overview of ASP.NET Core, see the [Introduction to ASP.NET Core](xref:index).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="021e2-437">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="021e2-437">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="021e2-438">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="021e2-438">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="021e2-439">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="021e2-439">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="021e2-440">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="021e2-440">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

<a name="rpvs17"></a>

## <a name="create-a-razor-pages-project"></a><span data-ttu-id="021e2-441">Razor Pages projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="021e2-441">Create a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="021e2-442">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="021e2-442">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="021e2-443">Razor Pages projesi oluşturma hakkında ayrıntılı yönergeler için bkz. [Razor Pages kullanmaya başlama](xref:tutorials/razor-pages/razor-pages-start) .</span><span class="sxs-lookup"><span data-stu-id="021e2-443">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) for detailed instructions on how to create a Razor Pages project.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="021e2-444">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="021e2-444">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="021e2-445">Komut satırından `dotnet new webapp` komutunu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="021e2-445">Run `dotnet new webapp` from the command line.</span></span>

<span data-ttu-id="021e2-446">Oluşturulan *. csproj* dosyasını Mac için Visual Studio açın.</span><span class="sxs-lookup"><span data-stu-id="021e2-446">Open the generated *.csproj* file from Visual Studio for Mac.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="021e2-447">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="021e2-447">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="021e2-448">Komut satırından `dotnet new webapp` komutunu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="021e2-448">Run `dotnet new webapp` from the command line.</span></span>

---

## <a name="razor-pages"></a><span data-ttu-id="021e2-449">Razor Pages</span><span class="sxs-lookup"><span data-stu-id="021e2-449">Razor Pages</span></span>

<span data-ttu-id="021e2-450">Razor Pages, *Startup.cs*'de etkinleştirilmiştir:</span><span class="sxs-lookup"><span data-stu-id="021e2-450">Razor Pages is enabled in *Startup.cs*:</span></span>

[!code-cs[](index/sample/RazorPagesIntro/Startup.cs?name=snippet_Startup)]

<span data-ttu-id="021e2-451">Temel bir sayfa düşünün:<a name="OnGet"></a></span><span class="sxs-lookup"><span data-stu-id="021e2-451">Consider a basic page: <a name="OnGet"></a></span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index.cshtml)]

<span data-ttu-id="021e2-452">Yukarıdaki kod, denetleyiciler ve görünümlerle ASP.NET Core bir uygulamada kullanılan [Razor görünüm dosyası](xref:tutorials/first-mvc-app/adding-view) gibi bir çok şey arar.</span><span class="sxs-lookup"><span data-stu-id="021e2-452">The preceding code looks a lot like a [Razor view file](xref:tutorials/first-mvc-app/adding-view) used in an ASP.NET Core app with controllers and views.</span></span> <span data-ttu-id="021e2-453">Bu, farklı kılan `@page` yönergedir.</span><span class="sxs-lookup"><span data-stu-id="021e2-453">What makes it different is the `@page` directive.</span></span> <span data-ttu-id="021e2-454">`@page`, dosyayı bir MVC eylemine dönüştürür. Bu, bir denetleyiciden geçmeden istekleri doğrudan işlediği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="021e2-454">`@page` makes the file into an MVC action - which means that it handles requests directly, without going through a controller.</span></span> <span data-ttu-id="021e2-455">`@page` bir sayfada ilk Razor yönergesi olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="021e2-455">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="021e2-456">`@page`, diğer Razor yapıları davranışını etkiler.</span><span class="sxs-lookup"><span data-stu-id="021e2-456">`@page` affects the behavior of other Razor constructs.</span></span>

<span data-ttu-id="021e2-457">@No__t-0 sınıfı kullanan benzer bir sayfa aşağıdaki iki dosyada gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="021e2-457">A similar page, using a `PageModel` class, is shown in the following two files.</span></span> <span data-ttu-id="021e2-458">*Pages/Index2. cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="021e2-458">The *Pages/Index2.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index2.cshtml)]

<span data-ttu-id="021e2-459">*Pages/Index2. cshtml. cs* sayfa modeli:</span><span class="sxs-lookup"><span data-stu-id="021e2-459">The *Pages/Index2.cshtml.cs* page model:</span></span>

[!code-cs[](index/sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]

<span data-ttu-id="021e2-460">Kurala göre `PageModel` sınıf dosyası, *. cs* eklenmiş Razor sayfası dosyasıyla aynı ada sahiptir.</span><span class="sxs-lookup"><span data-stu-id="021e2-460">By convention, the `PageModel` class file has the same name as the Razor Page file with *.cs* appended.</span></span> <span data-ttu-id="021e2-461">Örneğin, önceki Razor sayfası *Pages/Index2. cshtml*' dir.</span><span class="sxs-lookup"><span data-stu-id="021e2-461">For example, the previous Razor Page is *Pages/Index2.cshtml*.</span></span> <span data-ttu-id="021e2-462">@No__t-0 sınıfını içeren dosya *sayfa/Index2. cshtml. cs*olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="021e2-462">The file containing the `PageModel` class is named *Pages/Index2.cshtml.cs*.</span></span>

<span data-ttu-id="021e2-463">URL yollarının sayfalara olan ilişkilendirmeleri, sayfanın dosya sistemindeki konumuna göre belirlenir.</span><span class="sxs-lookup"><span data-stu-id="021e2-463">The associations of URL paths to pages are determined by the page's location in the file system.</span></span> <span data-ttu-id="021e2-464">Aşağıdaki tabloda bir Razor sayfa yolu ve eşleşen URL gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="021e2-464">The following table shows a Razor Page path and the matching URL:</span></span>

| <span data-ttu-id="021e2-465">Dosya adı ve yolu</span><span class="sxs-lookup"><span data-stu-id="021e2-465">File name and path</span></span>               | <span data-ttu-id="021e2-466">eşleşen URL</span><span class="sxs-lookup"><span data-stu-id="021e2-466">matching URL</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="021e2-467">*/Pages/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="021e2-467">*/Pages/Index.cshtml*</span></span> | <span data-ttu-id="021e2-468">`/` veya `/Index`</span><span class="sxs-lookup"><span data-stu-id="021e2-468">`/` or `/Index`</span></span> |
| <span data-ttu-id="021e2-469">*/Pages/Contact.exe*</span><span class="sxs-lookup"><span data-stu-id="021e2-469">*/Pages/Contact.cshtml*</span></span> | `/Contact` |
| <span data-ttu-id="021e2-470">*/Pages/Store/Contact.exe*</span><span class="sxs-lookup"><span data-stu-id="021e2-470">*/Pages/Store/Contact.cshtml*</span></span> | `/Store/Contact` |
| <span data-ttu-id="021e2-471">*/Pages/Store/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="021e2-471">*/Pages/Store/Index.cshtml*</span></span> | <span data-ttu-id="021e2-472">`/Store` veya `/Store/Index`</span><span class="sxs-lookup"><span data-stu-id="021e2-472">`/Store` or `/Store/Index`</span></span> |

<span data-ttu-id="021e2-473">Notlar:</span><span class="sxs-lookup"><span data-stu-id="021e2-473">Notes:</span></span>

* <span data-ttu-id="021e2-474">Çalışma zamanı, *Sayfalar* klasöründeki Razor Pages dosyaları varsayılan olarak arar.</span><span class="sxs-lookup"><span data-stu-id="021e2-474">The runtime looks for Razor Pages files in the *Pages* folder by default.</span></span>
* <span data-ttu-id="021e2-475">URL, bir sayfa içermiyorsa varsayılan sayfa olan `Index` ' dır.</span><span class="sxs-lookup"><span data-stu-id="021e2-475">`Index` is the default page when a URL doesn't include a page.</span></span>

## <a name="write-a-basic-form"></a><span data-ttu-id="021e2-476">Temel form yazma</span><span class="sxs-lookup"><span data-stu-id="021e2-476">Write a basic form</span></span>

<span data-ttu-id="021e2-477">Razor Pages, Web tarayıcıları ile kullanılan ortak desenleri bir uygulama oluştururken kolayca uygulanması için tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="021e2-477">Razor Pages is designed to make common patterns used with web browsers easy to implement when building an app.</span></span> <span data-ttu-id="021e2-478">[Model bağlama](xref:mvc/models/model-binding), [ETIKET yardımcıları](xref:mvc/views/tag-helpers/intro)ve HTML Yardımcıları hepsi, Razor sayfası sınıfında tanımlanan özelliklerle *çalışır* .</span><span class="sxs-lookup"><span data-stu-id="021e2-478">[Model binding](xref:mvc/models/model-binding), [Tag Helpers](xref:mvc/views/tag-helpers/intro), and HTML helpers all *just work* with the properties defined in a Razor Page class.</span></span> <span data-ttu-id="021e2-479">@No__t-0 modeli için temel bir "bize başvurun" formu uygulayan bir sayfa düşünün:</span><span class="sxs-lookup"><span data-stu-id="021e2-479">Consider a page that implements a basic "contact us" form for the `Contact` model:</span></span>

<span data-ttu-id="021e2-480">Bu belgedeki örneklerde, `DbContext` [Startup.cs](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) dosyasında başlatılır.</span><span class="sxs-lookup"><span data-stu-id="021e2-480">For the samples in this document, the `DbContext` is initialized in the [Startup.cs](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) file.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Startup.cs?highlight=15-16)]

<span data-ttu-id="021e2-481">Veri modeli:</span><span class="sxs-lookup"><span data-stu-id="021e2-481">The data model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Data/Customer.cs)]

<span data-ttu-id="021e2-482">DB bağlamı:</span><span class="sxs-lookup"><span data-stu-id="021e2-482">The db context:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Data/AppDbContext.cs)]

<span data-ttu-id="021e2-483">*Pages/Create. cshtml* görünüm dosyası:</span><span class="sxs-lookup"><span data-stu-id="021e2-483">The *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml)]

<span data-ttu-id="021e2-484">*Pages/Create. cshtml. cs* sayfa modeli:</span><span class="sxs-lookup"><span data-stu-id="021e2-484">The *Pages/Create.cshtml.cs* page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_ALL)]

<span data-ttu-id="021e2-485">Kurala göre `PageModel` sınıfı `<PageName>Model` olarak adlandırılır ve sayfayla aynı ad alanında yer alan.</span><span class="sxs-lookup"><span data-stu-id="021e2-485">By convention, the `PageModel` class is called `<PageName>Model` and is in the same namespace as the page.</span></span>

<span data-ttu-id="021e2-486">@No__t-0 sınıfı, bir sayfanın mantığının sunumuna ayrılmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="021e2-486">The `PageModel` class allows separation of the logic of a page from its presentation.</span></span> <span data-ttu-id="021e2-487">Sayfaya gönderilen istekler için sayfa işleyicilerini ve sayfayı işlemek için kullanılan verileri tanımlar.</span><span class="sxs-lookup"><span data-stu-id="021e2-487">It defines page handlers for requests sent to the page and the data used to render the page.</span></span> <span data-ttu-id="021e2-488">Bu ayrım şunları sağlar:</span><span class="sxs-lookup"><span data-stu-id="021e2-488">This separation allows:</span></span>

* <span data-ttu-id="021e2-489">[Bağımlılık ekleme](xref:fundamentals/dependency-injection)yoluyla sayfa bağımlılıklarını yönetme.</span><span class="sxs-lookup"><span data-stu-id="021e2-489">Managing of page dependencies through [dependency injection](xref:fundamentals/dependency-injection).</span></span>
* <span data-ttu-id="021e2-490">Sayfaların [birim testi](xref:test/razor-pages-tests) .</span><span class="sxs-lookup"><span data-stu-id="021e2-490">[Unit testing](xref:test/razor-pages-tests) the pages.</span></span>

<span data-ttu-id="021e2-491">Sayfada, @no__t 2 isteklerinde çalışan (bir Kullanıcı formu gönderdiğinde) `OnPostAsync` *işleyicisi yöntemi*vardır.</span><span class="sxs-lookup"><span data-stu-id="021e2-491">The page has an `OnPostAsync` *handler method*, which runs on `POST` requests (when a user posts the form).</span></span> <span data-ttu-id="021e2-492">Herhangi bir HTTP fiili için işleyici yöntemleri ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="021e2-492">You can add handler methods for any HTTP verb.</span></span> <span data-ttu-id="021e2-493">En yaygın işleyiciler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="021e2-493">The most common handlers are:</span></span>

* <span data-ttu-id="021e2-494">sayfa için gereken durumu başlatmak için `OnGet`.</span><span class="sxs-lookup"><span data-stu-id="021e2-494">`OnGet` to initialize state needed for the page.</span></span> <span data-ttu-id="021e2-495">[OnGet](#OnGet) örneği.</span><span class="sxs-lookup"><span data-stu-id="021e2-495">[OnGet](#OnGet) sample.</span></span>
* <span data-ttu-id="021e2-496">form gönderilerini işlemek için `OnPost`.</span><span class="sxs-lookup"><span data-stu-id="021e2-496">`OnPost` to handle form submissions.</span></span>

<span data-ttu-id="021e2-497">@No__t-0 adlandırma son eki isteğe bağlıdır, ancak genellikle zaman uyumsuz işlevler için kural tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="021e2-497">The `Async` naming suffix is optional but is often used by convention for asynchronous functions.</span></span> <span data-ttu-id="021e2-498">Yukarıdaki kod Razor Pages için tipik bir davranıştır.</span><span class="sxs-lookup"><span data-stu-id="021e2-498">The preceding code is typical for Razor Pages.</span></span>

<span data-ttu-id="021e2-499">Denetleyicileri ve görünümleri kullanarak ASP.NET uygulamaları hakkında bilginiz varsa:</span><span class="sxs-lookup"><span data-stu-id="021e2-499">If you're familiar with ASP.NET apps using controllers and views:</span></span>

* <span data-ttu-id="021e2-500">Yukarıdaki örnekteki `OnPostAsync` kodu, tipik denetleyici koduna benzer şekilde görünür.</span><span class="sxs-lookup"><span data-stu-id="021e2-500">The `OnPostAsync` code in the preceding example looks similar to typical controller code.</span></span>
* <span data-ttu-id="021e2-501">[Model bağlama](xref:mvc/models/model-binding), [doğrulama](xref:mvc/models/validation), [doğrulama](xref:mvc/models/validation)ve eylem sonuçları gibi mvc temel elemanlarının çoğu paylaşılır.</span><span class="sxs-lookup"><span data-stu-id="021e2-501">Most of the MVC primitives like [model binding](xref:mvc/models/model-binding), [validation](xref:mvc/models/validation), [Validation](xref:mvc/models/validation),  and action results are shared.</span></span>

<span data-ttu-id="021e2-502">Önceki `OnPostAsync` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="021e2-502">The previous `OnPostAsync` method:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="021e2-503">@No__t temel akışı-0:</span><span class="sxs-lookup"><span data-stu-id="021e2-503">The basic flow of `OnPostAsync`:</span></span>

<span data-ttu-id="021e2-504">Doğrulama hatalarını kontrol edin.</span><span class="sxs-lookup"><span data-stu-id="021e2-504">Check for validation errors.</span></span>

* <span data-ttu-id="021e2-505">Hata yoksa, verileri kaydedin ve yeniden yönlendirin.</span><span class="sxs-lookup"><span data-stu-id="021e2-505">If there are no errors, save the data and redirect.</span></span>
* <span data-ttu-id="021e2-506">Hatalar varsa, doğrulama iletileriyle sayfayı yeniden görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="021e2-506">If there are errors, show the page again with validation messages.</span></span> <span data-ttu-id="021e2-507">İstemci tarafı doğrulaması geleneksel ASP.NET Core MVC uygulamalarıyla aynıdır.</span><span class="sxs-lookup"><span data-stu-id="021e2-507">Client-side validation is identical to traditional ASP.NET Core MVC applications.</span></span> <span data-ttu-id="021e2-508">Çoğu durumda, istemci üzerinde doğrulama hataları algılanır ve sunucuya hiçbir zaman gönderilmez.</span><span class="sxs-lookup"><span data-stu-id="021e2-508">In many cases, validation errors would be detected on the client, and never submitted to the server.</span></span>

<span data-ttu-id="021e2-509">Veriler başarıyla girildiğinde, `OnPostAsync` işleyicisi yöntemi bir `RedirectToPageResult` örneğini döndürmek için `RedirectToPage` yardımcı yöntemini çağırır.</span><span class="sxs-lookup"><span data-stu-id="021e2-509">When the data is entered successfully, the `OnPostAsync` handler method calls the `RedirectToPage` helper method to return an instance of `RedirectToPageResult`.</span></span> <span data-ttu-id="021e2-510">`RedirectToPage`, `RedirectToAction` veya `RedirectToRoute` ' ye benzer ancak sayfalar için özelleştirilen yeni bir eylem sonucudur.</span><span class="sxs-lookup"><span data-stu-id="021e2-510">`RedirectToPage` is a new action result, similar to `RedirectToAction` or `RedirectToRoute`, but customized for pages.</span></span> <span data-ttu-id="021e2-511">Önceki örnekte, kök dizin sayfasına (`/Index`) yeniden yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="021e2-511">In the preceding sample, it redirects to the root Index page (`/Index`).</span></span> <span data-ttu-id="021e2-512">`RedirectToPage`, [Sayfalar Için URL oluşturma](#url_gen) bölümünde ayrıntılı olarak açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="021e2-512">`RedirectToPage` is detailed in the [URL generation for Pages](#url_gen) section.</span></span>

<span data-ttu-id="021e2-513">Gönderilen formda doğrulama hataları olduğunda (sunucuya geçilen), @ no__t-0 işleyici yöntemi `Page` yardımcı yöntemini çağırır.</span><span class="sxs-lookup"><span data-stu-id="021e2-513">When the submitted form has validation errors (that are passed to the server), the`OnPostAsync` handler method calls the `Page` helper method.</span></span> <span data-ttu-id="021e2-514">`Page` `PageResult` ' in bir örneğini döndürür.</span><span class="sxs-lookup"><span data-stu-id="021e2-514">`Page` returns an instance of `PageResult`.</span></span> <span data-ttu-id="021e2-515">@No__t-0 döndürme, denetleyicilerde bulunan eylemlerin `View` ' i döndürme biçimine benzer.</span><span class="sxs-lookup"><span data-stu-id="021e2-515">Returning `Page` is similar to how actions in controllers return `View`.</span></span> <span data-ttu-id="021e2-516">`PageResult`, bir işleyici yöntemi için varsayılan dönüş türüdür.</span><span class="sxs-lookup"><span data-stu-id="021e2-516">`PageResult` is the default return type for a handler method.</span></span> <span data-ttu-id="021e2-517">@No__t-0 döndüren bir işleyici yöntemi sayfayı işler.</span><span class="sxs-lookup"><span data-stu-id="021e2-517">A handler method that returns `void` renders the page.</span></span>

<span data-ttu-id="021e2-518">@No__t-0 özelliği, model bağlamasını kabul etmek için `[BindProperty]` özniteliğini kullanır.</span><span class="sxs-lookup"><span data-stu-id="021e2-518">The `Customer` property uses `[BindProperty]` attribute to opt in to model binding.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_PageModel&highlight=10-11)]

<span data-ttu-id="021e2-519">Razor Pages, varsayılan olarak, özellikleri yalnızca @no__t olmayan-0 olan fiiller ile bağlayın.</span><span class="sxs-lookup"><span data-stu-id="021e2-519">Razor Pages, by default, bind properties only with non-`GET` verbs.</span></span> <span data-ttu-id="021e2-520">Özelliklere bağlama, yazmanız gerektiğini kodun miktarını azaltabilir.</span><span class="sxs-lookup"><span data-stu-id="021e2-520">Binding to properties can reduce the amount of code you have to write.</span></span> <span data-ttu-id="021e2-521">Bağlama, form alanlarını işlemek için aynı özelliği kullanarak kodu azaltır (`<input asp-for="Customer.Name">`) ve girişi kabul eder.</span><span class="sxs-lookup"><span data-stu-id="021e2-521">Binding reduces code by using the same property to render form fields (`<input asp-for="Customer.Name">`) and accept the input.</span></span>

[!INCLUDE[](~/includes/bind-get.md)]

<span data-ttu-id="021e2-522">Giriş sayfası (*Index. cshtml*):</span><span class="sxs-lookup"><span data-stu-id="021e2-522">The home page (*Index.cshtml*):</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml)]

<span data-ttu-id="021e2-523">İlişkili `PageModel` sınıfı (*Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="021e2-523">The associated `PageModel` class (*Index.cshtml.cs*):</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs)]

<span data-ttu-id="021e2-524">*Index. cshtml* dosyası her kişi için bir düzenleme bağlantısı oluşturmak üzere aşağıdaki biçimlendirmeyi içerir:</span><span class="sxs-lookup"><span data-stu-id="021e2-524">The *Index.cshtml* file contains the following markup to create an edit link for each contact:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=21)]

<span data-ttu-id="021e2-525">@No__t-0 [bağlantı etiketi Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) , düzenleme sayfasına bir bağlantı oluşturmak için `asp-route-{value}` özniteliğini kullandı.</span><span class="sxs-lookup"><span data-stu-id="021e2-525">The `<a asp-page="./Edit" asp-route-id="@contact.Id">Edit</a>` [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) used the `asp-route-{value}` attribute to generate a link to the Edit page.</span></span> <span data-ttu-id="021e2-526">Bağlantı, iletişim KIMLIĞINE sahip rota verileri içerir.</span><span class="sxs-lookup"><span data-stu-id="021e2-526">The link contains route data with the contact ID.</span></span> <span data-ttu-id="021e2-527">Örneğin, `https://localhost:5001/Edit/1`.</span><span class="sxs-lookup"><span data-stu-id="021e2-527">For example, `https://localhost:5001/Edit/1`.</span></span> <span data-ttu-id="021e2-528">[Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro), Razor dosyalarında HTML öğelerinin oluşturulmasına ve işlenmesine sunucu tarafı kodun katılmasını etkinleştir.</span><span class="sxs-lookup"><span data-stu-id="021e2-528">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span> <span data-ttu-id="021e2-529">Etiket Yardımcıları @no__t tarafından etkinleştirilir-0</span><span class="sxs-lookup"><span data-stu-id="021e2-529">Tag Helpers are enabled by `@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers`</span></span>

<span data-ttu-id="021e2-530">*Pages/Edit. cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="021e2-530">The *Pages/Edit.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Edit.cshtml?highlight=1)]

<span data-ttu-id="021e2-531">İlk satır `@page "{id:int}"` yönergesini içerir.</span><span class="sxs-lookup"><span data-stu-id="021e2-531">The first line contains the `@page "{id:int}"` directive.</span></span> <span data-ttu-id="021e2-532">{No__t-0 Yönlendirme kısıtlaması, sayfaya `int` rota verileri içeren sayfaya istekleri kabul etmesini söyler.</span><span class="sxs-lookup"><span data-stu-id="021e2-532">The routing constraint`"{id:int}"` tells the page to accept requests to the page that contain `int` route data.</span></span> <span data-ttu-id="021e2-533">Sayfaya yapılan bir istek bir `int` ' a dönüştürülebileceği rota verileri içermiyorsa, çalışma zamanı bir HTTP 404 (bulunamadı) hatası döndürür.</span><span class="sxs-lookup"><span data-stu-id="021e2-533">If a request to the page doesn't contain route data that can be converted to an `int`, the runtime returns an HTTP 404 (not found) error.</span></span> <span data-ttu-id="021e2-534">KIMLIĞI isteğe bağlı yapmak için, yol kısıtlamasına `?` ekleyin:</span><span class="sxs-lookup"><span data-stu-id="021e2-534">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="021e2-535">*Pages/Edit. cshtml. cs* dosyası:</span><span class="sxs-lookup"><span data-stu-id="021e2-535">The *Pages/Edit.cshtml.cs* file:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Edit.cshtml.cs)]

<span data-ttu-id="021e2-536">*Index. cshtml* dosyası, her müşteri kişisi için bir silme düğmesi oluşturmak için de biçimlendirme içerir:</span><span class="sxs-lookup"><span data-stu-id="021e2-536">The *Index.cshtml* file also contains markup to create a delete button for each customer contact:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=22-23)]

<span data-ttu-id="021e2-537">Sil düğmesi HTML 'de işlendiğinde, `formaction` ' ın parametreleri şunları içerir:</span><span class="sxs-lookup"><span data-stu-id="021e2-537">When the delete button is rendered in HTML, its `formaction` includes parameters for:</span></span>

* <span data-ttu-id="021e2-538">@No__t-0 özniteliği tarafından belirtilen müşteri iletişim KIMLIĞI.</span><span class="sxs-lookup"><span data-stu-id="021e2-538">The customer contact ID specified by the `asp-route-id` attribute.</span></span>
* <span data-ttu-id="021e2-539">@No__t-1 özniteliği tarafından belirtilen `handler`.</span><span class="sxs-lookup"><span data-stu-id="021e2-539">The `handler` specified by the `asp-page-handler` attribute.</span></span>

<span data-ttu-id="021e2-540">Müşteri irtibat KIMLIĞI `1` olan işlenmiş silme düğmesine bir örnek aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="021e2-540">Here is an example of a rendered delete button with a customer contact ID of `1`:</span></span>

```html
<button type="submit" formaction="/?id=1&amp;handler=delete">delete</button>
```

<span data-ttu-id="021e2-541">Düğme seçildiğinde, sunucuya `POST` isteği gönderilir.</span><span class="sxs-lookup"><span data-stu-id="021e2-541">When the button is selected, a form `POST` request is sent to the server.</span></span> <span data-ttu-id="021e2-542">Kural gereği, işleyici yönteminin adı, `OnPost[handler]Async` düzenine göre `handler` parametresinin değerine göre seçilir.</span><span class="sxs-lookup"><span data-stu-id="021e2-542">By convention, the name of the handler method is selected based on the value of the `handler` parameter according to the scheme `OnPost[handler]Async`.</span></span>

<span data-ttu-id="021e2-543">Bu örnekte `handler` `delete` olduğundan, `POST` isteğini işlemek için `OnPostDeleteAsync` işleyici yöntemi kullanılır.</span><span class="sxs-lookup"><span data-stu-id="021e2-543">Because the `handler` is `delete` in this example, the `OnPostDeleteAsync` handler method is used to process the `POST` request.</span></span> <span data-ttu-id="021e2-544">@No__t-0, `remove` gibi farklı bir değere ayarlanmışsa, `OnPostRemoveAsync` adlı bir işleyici yöntemi seçilidir.</span><span class="sxs-lookup"><span data-stu-id="021e2-544">If the `asp-page-handler` is set to a different value, such as `remove`, a handler method with the name `OnPostRemoveAsync` is selected.</span></span> <span data-ttu-id="021e2-545">Aşağıdaki kod `OnPostDeleteAsync` işleyicisini gösterir:</span><span class="sxs-lookup"><span data-stu-id="021e2-545">The following code shows the `OnPostDeleteAsync` handler:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs?range=26-37)]

<span data-ttu-id="021e2-546">`OnPostDeleteAsync` Yöntemi:</span><span class="sxs-lookup"><span data-stu-id="021e2-546">The `OnPostDeleteAsync` method:</span></span>

* <span data-ttu-id="021e2-547">Sorgu dizesinden `id` kabul eder.</span><span class="sxs-lookup"><span data-stu-id="021e2-547">Accepts the `id` from the query string.</span></span> <span data-ttu-id="021e2-548">*Index. cshtml* sayfa yönergesi yönlendirme kısıtlaması içeriyorsa `"{id:int?}"`, `id` rota verilerinden gelir.</span><span class="sxs-lookup"><span data-stu-id="021e2-548">If the *Index.cshtml* page directive contained routing constraint `"{id:int?}"`, `id` would come from route data.</span></span> <span data-ttu-id="021e2-549">@No__t-0 ' a ait rota verileri, URI 'de `https://localhost:5001/Customers/2` gibi belirtilir.</span><span class="sxs-lookup"><span data-stu-id="021e2-549">The route data for `id` is specified in the URI such as `https://localhost:5001/Customers/2`.</span></span>
* <span data-ttu-id="021e2-550">@No__t-0 ile müşteri iletişim için veritabanını sorgular.</span><span class="sxs-lookup"><span data-stu-id="021e2-550">Queries the database for the customer contact with `FindAsync`.</span></span>
* <span data-ttu-id="021e2-551">Müşteri ilgili kişisi bulunursa, bunlar müşteri kişileri listesinden kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="021e2-551">If the customer contact is found, they're removed from the list of customer contacts.</span></span> <span data-ttu-id="021e2-552">Veritabanı güncelleştirildi.</span><span class="sxs-lookup"><span data-stu-id="021e2-552">The database is updated.</span></span>
* <span data-ttu-id="021e2-553">Kök dizin sayfasına yeniden yönlendirmek için `RedirectToPage` çağırır (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="021e2-553">Calls `RedirectToPage` to redirect to the root Index page (`/Index`).</span></span>

## <a name="mark-page-properties-as-required"></a><span data-ttu-id="021e2-554">Sayfa özelliklerini gerektiği gibi işaretle</span><span class="sxs-lookup"><span data-stu-id="021e2-554">Mark page properties as required</span></span>

<span data-ttu-id="021e2-555">@No__t-0 ' a ait özellikler [gerekli](/dotnet/api/system.componentmodel.dataannotations.requiredattribute) öznitelikle birlikte kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="021e2-555">Properties on a `PageModel` can be decorated with the [Required](/dotnet/api/system.componentmodel.dataannotations.requiredattribute) attribute:</span></span>

[!code-cs[](index/sample/Create.cshtml.cs?highlight=3,15-16)]

<span data-ttu-id="021e2-556">Daha fazla bilgi için bkz. [model doğrulaması](xref:mvc/models/validation).</span><span class="sxs-lookup"><span data-stu-id="021e2-556">For more information, see [Model validation](xref:mvc/models/validation).</span></span>

## <a name="handle-head-requests-with-an-onget-handler-fallback"></a><span data-ttu-id="021e2-557">OnGet işleyicisi geri dönüşü ile tanıtıcı HEAD istekleri</span><span class="sxs-lookup"><span data-stu-id="021e2-557">Handle HEAD requests with an OnGet handler fallback</span></span>

<span data-ttu-id="021e2-558">`HEAD` istekleri belirli bir kaynağın üst bilgilerini almanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="021e2-558">`HEAD` requests allow you to retrieve the headers for a specific resource.</span></span> <span data-ttu-id="021e2-559">@No__t-0 isteklerinin aksine, `HEAD` istekleri yanıt gövdesi döndürmez.</span><span class="sxs-lookup"><span data-stu-id="021e2-559">Unlike `GET` requests, `HEAD` requests don't return a response body.</span></span>

<span data-ttu-id="021e2-560">Normalde, `HEAD` istekleri için `OnHead` işleyicisi oluşturulur ve çağırılır:</span><span class="sxs-lookup"><span data-stu-id="021e2-560">Ordinarily, an `OnHead` handler is created and called for `HEAD` requests:</span></span> 

```csharp
public void OnHead()
{
    HttpContext.Response.Headers.Add("HandledBy", "Handled by OnHead!");
}
```

<span data-ttu-id="021e2-561">ASP.NET Core 2,1 veya sonraki bir sürümde, hiçbir `OnHead` işleyicisi tanımlanmamışsa Razor Pages `OnGet` işleyicisini çağırmaya geri döner.</span><span class="sxs-lookup"><span data-stu-id="021e2-561">In ASP.NET Core 2.1 or later, Razor Pages falls back to calling the `OnGet` handler if no `OnHead` handler is defined.</span></span> <span data-ttu-id="021e2-562">Bu davranış, `Startup.ConfigureServices` ' de [Setcompatibilityversion](xref:mvc/compatibility-version) çağrısıyla etkinleştirilir:</span><span class="sxs-lookup"><span data-stu-id="021e2-562">This behavior is enabled by the call to [SetCompatibilityVersion](xref:mvc/compatibility-version) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddMvc()
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_1);
```

<span data-ttu-id="021e2-563">Varsayılan Şablonlar, ASP.NET Core 2,1 ve 2,2 ' de `SetCompatibilityVersion` çağrısını üretir.</span><span class="sxs-lookup"><span data-stu-id="021e2-563">The default templates generate the `SetCompatibilityVersion` call in ASP.NET Core 2.1 and 2.2.</span></span> <span data-ttu-id="021e2-564">`SetCompatibilityVersion` `AllowMappingHeadRequestsToGetHandler` Razor Pages seçeneğini `true` ' ye etkin şekilde ayarlar.</span><span class="sxs-lookup"><span data-stu-id="021e2-564">`SetCompatibilityVersion` effectively sets the Razor Pages option `AllowMappingHeadRequestsToGetHandler` to `true`.</span></span>

<span data-ttu-id="021e2-565">@No__t-0 ile tüm davranışlardan çıkmak yerine, açıkça *belirli* davranışları kabul edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="021e2-565">Rather than opting in to all behaviors with `SetCompatibilityVersion`, you can explicitly opt in to *specific* behaviors.</span></span> <span data-ttu-id="021e2-566">Aşağıdaki kod, `HEAD` isteklerinin `OnGet` işleyicisine eşlenmesine izin vermek için ' de kullanılır:</span><span class="sxs-lookup"><span data-stu-id="021e2-566">The following code opts in to allowing `HEAD` requests to be mapped to the `OnGet` handler:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        options.AllowMappingHeadRequestsToGetHandler = true;
    });
```

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a><span data-ttu-id="021e2-567">XSRF/CSRF ve Razor Pages</span><span class="sxs-lookup"><span data-stu-id="021e2-567">XSRF/CSRF and Razor Pages</span></span>

<span data-ttu-id="021e2-568">[Antiforgery doğrulaması](xref:security/anti-request-forgery)için herhangi bir kod yazmanız gerekmez.</span><span class="sxs-lookup"><span data-stu-id="021e2-568">You don't have to write any code for [antiforgery validation](xref:security/anti-request-forgery).</span></span> <span data-ttu-id="021e2-569">Antiforgery belirteci oluşturma ve doğrulama, Razor Pages otomatik olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="021e2-569">Antiforgery token generation and validation are automatically included in Razor Pages.</span></span>

<a name="layout"></a>

## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a><span data-ttu-id="021e2-570">Razor Pages ile düzenleri, partileri, şablonları ve etiket yardımcılarını kullanma</span><span class="sxs-lookup"><span data-stu-id="021e2-570">Using Layouts, partials, templates, and Tag Helpers with Razor Pages</span></span>

<span data-ttu-id="021e2-571">Sayfalar, Razor görünüm altyapısının tüm özellikleri ile çalışır.</span><span class="sxs-lookup"><span data-stu-id="021e2-571">Pages work with all the capabilities of the Razor view engine.</span></span> <span data-ttu-id="021e2-572">Düzenler, partıals, şablonlar, etiket yardımcıları, *_Viewstart. cshtml*, *_viewwimports. cshtml* geleneksel Razor görünümlerinde oldukları gibi çalışır.</span><span class="sxs-lookup"><span data-stu-id="021e2-572">Layouts, partials, templates, Tag Helpers, *_ViewStart.cshtml*, *_ViewImports.cshtml* work in the same way they do for conventional Razor views.</span></span>

<span data-ttu-id="021e2-573">Bu özelliklerden bazılarının avantajlarından yararlanarak bu sayfayı declutter edelim.</span><span class="sxs-lookup"><span data-stu-id="021e2-573">Let's declutter this page by taking advantage of some of those capabilities.</span></span>

<span data-ttu-id="021e2-574">*Pages/Shared/_Layout. cshtml*öğesine bir [Düzen sayfası](xref:mvc/views/layout) ekleyin:</span><span class="sxs-lookup"><span data-stu-id="021e2-574">Add a [layout page](xref:mvc/views/layout) to *Pages/Shared/_Layout.cshtml*:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_LayoutSimple.cshtml)]

<span data-ttu-id="021e2-575">[Düzen](xref:mvc/views/layout):</span><span class="sxs-lookup"><span data-stu-id="021e2-575">The [Layout](xref:mvc/views/layout):</span></span>

* <span data-ttu-id="021e2-576">Her sayfanın yerleşimini denetler (sayfa düzen dışında değilse).</span><span class="sxs-lookup"><span data-stu-id="021e2-576">Controls the layout of each page (unless the page opts out of layout).</span></span>
* <span data-ttu-id="021e2-577">JavaScript ve stil sayfaları gibi HTML yapılarını içeri aktarır.</span><span class="sxs-lookup"><span data-stu-id="021e2-577">Imports HTML structures such as JavaScript and stylesheets.</span></span>

<span data-ttu-id="021e2-578">Daha fazla bilgi için bkz. [Düzen sayfası](xref:mvc/views/layout) .</span><span class="sxs-lookup"><span data-stu-id="021e2-578">See [layout page](xref:mvc/views/layout) for more information.</span></span>

<span data-ttu-id="021e2-579">[Layout](xref:mvc/views/layout#specifying-a-layout) özelliği *Pages/_viewstart. cshtml*içinde ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="021e2-579">The [Layout](xref:mvc/views/layout#specifying-a-layout) property is set in *Pages/_ViewStart.cshtml*:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]

<span data-ttu-id="021e2-580">Düzen *Sayfalar/paylaşılan* klasöründedir.</span><span class="sxs-lookup"><span data-stu-id="021e2-580">The layout is in the *Pages/Shared* folder.</span></span> <span data-ttu-id="021e2-581">Sayfalar, geçerli sayfayla aynı klasörden başlayarak diğer görünümleri (düzenler, şablonlar, parals) hiyerarşik olarak arar.</span><span class="sxs-lookup"><span data-stu-id="021e2-581">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="021e2-582">*Sayfalar/paylaşılan* klasördeki bir düzen, *Sayfalar* klasörü altındaki herhangi bir Razor sayfasından kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="021e2-582">A layout in the *Pages/Shared* folder can be used from any Razor page under the *Pages* folder.</span></span>

<span data-ttu-id="021e2-583">Düzen dosyası *Sayfalar/paylaşılan* klasörüne gitmelidir.</span><span class="sxs-lookup"><span data-stu-id="021e2-583">The layout file should go in the *Pages/Shared* folder.</span></span>

<span data-ttu-id="021e2-584">Düzen dosyasını *Görünümler/paylaşılan* klasöre **yerleştirmenizi öneririz** .</span><span class="sxs-lookup"><span data-stu-id="021e2-584">We recommend you **not** put the layout file in the *Views/Shared* folder.</span></span> <span data-ttu-id="021e2-585">*Görünümler/paylaşılan* bir MVC görünümleri modelidir.</span><span class="sxs-lookup"><span data-stu-id="021e2-585">*Views/Shared* is an MVC views pattern.</span></span> <span data-ttu-id="021e2-586">Razor Pages, yol kurallarını değil klasör hiyerarşisine güvenmektir.</span><span class="sxs-lookup"><span data-stu-id="021e2-586">Razor Pages are meant to rely on folder hierarchy, not path conventions.</span></span>

<span data-ttu-id="021e2-587">Bir Razor sayfasından arama görüntüleme, *Sayfalar* klasörünü içerir.</span><span class="sxs-lookup"><span data-stu-id="021e2-587">View search from a Razor Page includes the *Pages* folder.</span></span> <span data-ttu-id="021e2-588">MVC denetleyicileri ve geleneksel Razor görünümleriyle kullandığınız düzenler, şablonlar ve parals işlemleri *yalnızca çalışır*.</span><span class="sxs-lookup"><span data-stu-id="021e2-588">The layouts, templates, and partials you're using with MVC controllers and conventional Razor views *just work*.</span></span>

<span data-ttu-id="021e2-589">*Pages/_Viewwimports. cshtml* dosyası ekleyin:</span><span class="sxs-lookup"><span data-stu-id="021e2-589">Add a *Pages/_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]

<span data-ttu-id="021e2-590">`@namespace` daha sonra öğreticide açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="021e2-590">`@namespace` is explained later in the tutorial.</span></span> <span data-ttu-id="021e2-591">@No__t-0 yönergesi, [yerleşik etiket yardımcılarını](xref:mvc/views/tag-helpers/builtin-th/Index) *Sayfalar* klasöründeki tüm sayfalara getirir.</span><span class="sxs-lookup"><span data-stu-id="021e2-591">The `@addTagHelper` directive brings in the [built-in Tag Helpers](xref:mvc/views/tag-helpers/builtin-th/Index) to all the pages in the *Pages* folder.</span></span>

<a name="namespace"></a>

<span data-ttu-id="021e2-592">@No__t-0 yönergesi açıkça bir sayfada kullanıldığında:</span><span class="sxs-lookup"><span data-stu-id="021e2-592">When the `@namespace` directive is used explicitly on a page:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]

<span data-ttu-id="021e2-593">Yönergesi sayfanın ad alanını ayarlar.</span><span class="sxs-lookup"><span data-stu-id="021e2-593">The directive sets the namespace for the page.</span></span> <span data-ttu-id="021e2-594">@No__t-0 yönergesinin ad alanını içermesi gerekmez.</span><span class="sxs-lookup"><span data-stu-id="021e2-594">The `@model` directive doesn't need to include the namespace.</span></span>

<span data-ttu-id="021e2-595">@No__t-0 yönergesi *_Viewwimports. cshtml*içinde yer aldığında, belirtilen ad alanı, `@namespace` yönergesini Içeri aktaran sayfada oluşturulan ad alanı için ön ek sağlar.</span><span class="sxs-lookup"><span data-stu-id="021e2-595">When the `@namespace` directive is contained in *_ViewImports.cshtml*, the specified namespace supplies the prefix for the generated namespace in the Page that imports the `@namespace` directive.</span></span> <span data-ttu-id="021e2-596">Oluşturulan ad alanının geri kalanı (sonek bölümü), *_Viewwimports. cshtml* dosyasını ve sayfayı içeren klasörü içeren, noktayla ayrılmış göreli yoldur.</span><span class="sxs-lookup"><span data-stu-id="021e2-596">The rest of the generated namespace (the suffix portion) is the dot-separated relative path between the folder containing *_ViewImports.cshtml* and the folder containing the page.</span></span>

<span data-ttu-id="021e2-597">Örneğin, `PageModel` sınıf *sayfaları/müşteriler/Edit. cshtml. cs* açıkça ad alanını ayarlar:</span><span class="sxs-lookup"><span data-stu-id="021e2-597">For example, the `PageModel` class *Pages/Customers/Edit.cshtml.cs* explicitly sets the namespace:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=snippet_namespace)]

<span data-ttu-id="021e2-598">*Pages/_Viewwimports. cshtml* dosyası aşağıdaki ad alanını ayarlar:</span><span class="sxs-lookup"><span data-stu-id="021e2-598">The *Pages/_ViewImports.cshtml* file sets the following namespace:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]

<span data-ttu-id="021e2-599">*Pages/Customers/Edit. cshtml* Razor sayfasının oluşturulan ad alanı `PageModel` sınıfıyla aynıdır.</span><span class="sxs-lookup"><span data-stu-id="021e2-599">The generated namespace for the *Pages/Customers/Edit.cshtml* Razor Page is the same as the `PageModel` class.</span></span>

<span data-ttu-id="021e2-600">`@namespace` *geleneksel Razor görünümleriyle de kullanılabilir.*</span><span class="sxs-lookup"><span data-stu-id="021e2-600">`@namespace` *also works with conventional Razor views.*</span></span>

<span data-ttu-id="021e2-601">Özgün *Sayfalar/Create. cshtml* görünüm dosyası:</span><span class="sxs-lookup"><span data-stu-id="021e2-601">The original *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml?highlight=2)]

<span data-ttu-id="021e2-602">Güncelleştirilmiş *Sayfalar/Create. cshtml* görünüm dosyası:</span><span class="sxs-lookup"><span data-stu-id="021e2-602">The updated *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/Create.cshtml?highlight=2)]

<span data-ttu-id="021e2-603">[Razor Pages Başlatıcı projesi](#rpvs17) , istemci tarafı doğrulamayı bağlayan *sayfaları/_ValidationScriptsPartial. cshtml*'yi içerir.</span><span class="sxs-lookup"><span data-stu-id="021e2-603">The [Razor Pages starter project](#rpvs17) contains the *Pages/_ValidationScriptsPartial.cshtml*, which hooks up client-side validation.</span></span>

<span data-ttu-id="021e2-604">Kısmi görünümler hakkında daha fazla bilgi için bkz. <xref:mvc/views/partial>.</span><span class="sxs-lookup"><span data-stu-id="021e2-604">For more information on partial views, see <xref:mvc/views/partial>.</span></span>

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a><span data-ttu-id="021e2-605">Sayfalar için URL oluşturma</span><span class="sxs-lookup"><span data-stu-id="021e2-605">URL generation for Pages</span></span>

<span data-ttu-id="021e2-606">Daha önce gösterilen `Create` sayfası `RedirectToPage` kullanır:</span><span class="sxs-lookup"><span data-stu-id="021e2-606">The `Create` page, shown previously, uses `RedirectToPage`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=10)]

<span data-ttu-id="021e2-607">Uygulama aşağıdaki dosya/klasör yapısına sahiptir:</span><span class="sxs-lookup"><span data-stu-id="021e2-607">The app has the following file/folder structure:</span></span>

* <span data-ttu-id="021e2-608">*/Pages*</span><span class="sxs-lookup"><span data-stu-id="021e2-608">*/Pages*</span></span>

  * <span data-ttu-id="021e2-609">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="021e2-609">*Index.cshtml*</span></span>
  * <span data-ttu-id="021e2-610">*/Customers*</span><span class="sxs-lookup"><span data-stu-id="021e2-610">*/Customers*</span></span>

    * <span data-ttu-id="021e2-611">*. Cshtml oluştur*</span><span class="sxs-lookup"><span data-stu-id="021e2-611">*Create.cshtml*</span></span>
    * <span data-ttu-id="021e2-612">*Edit.cshtml*</span><span class="sxs-lookup"><span data-stu-id="021e2-612">*Edit.cshtml*</span></span>
    * <span data-ttu-id="021e2-613">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="021e2-613">*Index.cshtml*</span></span>

<span data-ttu-id="021e2-614">*Pages/Customers/Create. cshtml* ve *Pages/Customers/Edit. cshtml* sayfaları, başarılı olduktan sonra *Pages/Index. cshtml* dosyasına yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="021e2-614">The *Pages/Customers/Create.cshtml* and *Pages/Customers/Edit.cshtml* pages redirect to *Pages/Index.cshtml* after success.</span></span> <span data-ttu-id="021e2-615">@No__t-0 dizesi, önceki sayfaya erişmek için URI 'nin bir parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="021e2-615">The string `/Index` is part of the URI to access the preceding page.</span></span> <span data-ttu-id="021e2-616">@No__t-0 dizesi, *Sayfalar/Index. cshtml* sayfasına URI 'ler oluşturmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="021e2-616">The string `/Index` can be used to generate URIs to the *Pages/Index.cshtml* page.</span></span> <span data-ttu-id="021e2-617">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="021e2-617">For example:</span></span>

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">My Index Page</a>`
* `RedirectToPage("/Index")`

<span data-ttu-id="021e2-618">Sayfa adı, kök */Pages* klasöründeki, önde gelen `/` (örneğin, `/Index`) içeren sayfanın yoludur.</span><span class="sxs-lookup"><span data-stu-id="021e2-618">The page name is the path to the page from the root */Pages* folder including a leading `/` (for example, `/Index`).</span></span> <span data-ttu-id="021e2-619">Önceki URL oluşturma örnekleri bir URL 'YI kodlamadan gelişmiş seçenekler ve işlevsel yetenekler sunar.</span><span class="sxs-lookup"><span data-stu-id="021e2-619">The preceding URL generation samples offer enhanced options and functional capabilities over hardcoding a URL.</span></span> <span data-ttu-id="021e2-620">URL oluşturma [yönlendirme](xref:mvc/controllers/routing) kullanır ve yolun hedef yolda nasıl tanımlandığınıza göre parametreleri oluşturabilir ve kodlayabilir.</span><span class="sxs-lookup"><span data-stu-id="021e2-620">URL generation uses [routing](xref:mvc/controllers/routing) and can generate and encode parameters according to how the route is defined in the destination path.</span></span>

<span data-ttu-id="021e2-621">Sayfalar için URL oluşturma göreli adları destekler.</span><span class="sxs-lookup"><span data-stu-id="021e2-621">URL generation for pages supports relative names.</span></span> <span data-ttu-id="021e2-622">Aşağıdaki tabloda, *sayfa/müşteri/oluşturma. cshtml*'den farklı `RedirectToPage` parametreleriyle hangi dizin sayfasının seçildiği gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="021e2-622">The following table shows which Index page is selected with different `RedirectToPage` parameters from *Pages/Customers/Create.cshtml*:</span></span>

| <span data-ttu-id="021e2-623">RedirectToPage (x)</span><span class="sxs-lookup"><span data-stu-id="021e2-623">RedirectToPage(x)</span></span>| <span data-ttu-id="021e2-624">Sayfasında</span><span class="sxs-lookup"><span data-stu-id="021e2-624">Page</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="021e2-625">RedirectToPage ("/Index")</span><span class="sxs-lookup"><span data-stu-id="021e2-625">RedirectToPage("/Index")</span></span> | <span data-ttu-id="021e2-626">*Sayfa/dizin*</span><span class="sxs-lookup"><span data-stu-id="021e2-626">*Pages/Index*</span></span> |
| <span data-ttu-id="021e2-627">RedirectToPage ("./Index");</span><span class="sxs-lookup"><span data-stu-id="021e2-627">RedirectToPage("./Index");</span></span> | <span data-ttu-id="021e2-628">*Sayfalar/müşteriler/Dizin*</span><span class="sxs-lookup"><span data-stu-id="021e2-628">*Pages/Customers/Index*</span></span> |
| <span data-ttu-id="021e2-629">RedirectToPage (".. /İndex ")</span><span class="sxs-lookup"><span data-stu-id="021e2-629">RedirectToPage("../Index")</span></span> | <span data-ttu-id="021e2-630">*Sayfa/dizin*</span><span class="sxs-lookup"><span data-stu-id="021e2-630">*Pages/Index*</span></span> |
| <span data-ttu-id="021e2-631">RedirectToPage ("Dizin")</span><span class="sxs-lookup"><span data-stu-id="021e2-631">RedirectToPage("Index")</span></span>  | <span data-ttu-id="021e2-632">*Sayfalar/müşteriler/Dizin*</span><span class="sxs-lookup"><span data-stu-id="021e2-632">*Pages/Customers/Index*</span></span> |

<span data-ttu-id="021e2-633">`RedirectToPage("Index")`, `RedirectToPage("./Index")` ve `RedirectToPage("../Index")` , *göreli adlardır*.</span><span class="sxs-lookup"><span data-stu-id="021e2-633">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, and `RedirectToPage("../Index")`  are *relative names*.</span></span> <span data-ttu-id="021e2-634">@No__t-0 parametresi, hedef sayfanın adını hesaplamak için geçerli sayfanın yoluyla *birleştirilir* .</span><span class="sxs-lookup"><span data-stu-id="021e2-634">The `RedirectToPage` parameter is *combined* with the path of the current page to compute the name of the destination page.</span></span>  <!-- Review: Original had The provided string is combined with the page name of the current page to compute the name of the destination page.  page name, not page path -->

<span data-ttu-id="021e2-635">Karmaşık bir yapıya sahip siteler oluştururken göreli ad bağlama yararlı olur.</span><span class="sxs-lookup"><span data-stu-id="021e2-635">Relative name linking is useful when building sites with a complex structure.</span></span> <span data-ttu-id="021e2-636">Bir klasördeki sayfalar arasında bağlantı sağlamak için göreli adlar kullanırsanız, bu klasörü yeniden adlandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="021e2-636">If you use relative names to link between pages in a folder, you can rename that folder.</span></span> <span data-ttu-id="021e2-637">Tüm bağlantılar hala çalışır (klasör adını içermediği için).</span><span class="sxs-lookup"><span data-stu-id="021e2-637">All the links still work (because they didn't include the folder name).</span></span>

<span data-ttu-id="021e2-638">Farklı bir [alandaki](xref:mvc/controllers/areas)bir sayfaya yeniden yönlendirmek için alanını belirtin:</span><span class="sxs-lookup"><span data-stu-id="021e2-638">To redirect to a page in a different [Area](xref:mvc/controllers/areas), specify the area:</span></span>

```csharp
RedirectToPage("/Index", new { area = "Services" });
```

<span data-ttu-id="021e2-639">Daha fazla bilgi için bkz. <xref:mvc/controllers/areas>.</span><span class="sxs-lookup"><span data-stu-id="021e2-639">For more information, see <xref:mvc/controllers/areas>.</span></span>

## <a name="viewdata-attribute"></a><span data-ttu-id="021e2-640">ViewData özniteliği</span><span class="sxs-lookup"><span data-stu-id="021e2-640">ViewData attribute</span></span>

<span data-ttu-id="021e2-641">Veri, [Viewdataattribute](/dotnet/api/microsoft.aspnetcore.mvc.viewdataattribute)içeren bir sayfaya geçirilebilir.</span><span class="sxs-lookup"><span data-stu-id="021e2-641">Data can be passed to a page with [ViewDataAttribute](/dotnet/api/microsoft.aspnetcore.mvc.viewdataattribute).</span></span> <span data-ttu-id="021e2-642">@No__t-0 ile donatılmış denetleyicilerde veya Razor sayfa modellerindeki özelliklerin değerleri, [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary)'den depolanır ve yüklenir.</span><span class="sxs-lookup"><span data-stu-id="021e2-642">Properties on controllers or Razor Page models decorated with `[ViewData]` have their values stored and loaded from the [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary).</span></span>

<span data-ttu-id="021e2-643">Aşağıdaki örnekte, `AboutModel` `[ViewData]` ile donatılmış bir `Title` özelliği içerir.</span><span class="sxs-lookup"><span data-stu-id="021e2-643">In the following example, the `AboutModel` contains a `Title` property decorated with `[ViewData]`.</span></span> <span data-ttu-id="021e2-644">@No__t-0 özelliği hakkında sayfasının başlığına ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="021e2-644">The `Title` property is set to the title of the About page:</span></span>

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

<span data-ttu-id="021e2-645">Hakkında sayfasında, `Title` özelliğine model özelliği olarak erişin:</span><span class="sxs-lookup"><span data-stu-id="021e2-645">In the About page, access the `Title` property as a model property:</span></span>

```cshtml
<h1>@Model.Title</h1>
```

<span data-ttu-id="021e2-646">Mizanpajda, başlık ViewData sözlüğünden okundu:</span><span class="sxs-lookup"><span data-stu-id="021e2-646">In the layout, the title is read from the ViewData dictionary:</span></span>

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"] - WebApplication</title>
    ...
```

## <a name="tempdata"></a><span data-ttu-id="021e2-647">TempData</span><span class="sxs-lookup"><span data-stu-id="021e2-647">TempData</span></span>

<span data-ttu-id="021e2-648">ASP.NET Core bir [denetleyicide](/dotnet/api/microsoft.aspnetcore.mvc.controller) [TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) özelliğini kullanıma sunar.</span><span class="sxs-lookup"><span data-stu-id="021e2-648">ASP.NET Core exposes the [TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) property on a [controller](/dotnet/api/microsoft.aspnetcore.mvc.controller).</span></span> <span data-ttu-id="021e2-649">Bu özellik, okunana kadar verileri depolar.</span><span class="sxs-lookup"><span data-stu-id="021e2-649">This property stores data until it's read.</span></span> <span data-ttu-id="021e2-650">@No__t-0 ve `Peek` yöntemleri, silme yapılmadan verileri incelemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="021e2-650">The `Keep` and `Peek` methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="021e2-651">`TempData`, tek bir istekten daha fazla veri gerektiğinde yeniden yönlendirme için kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="021e2-651">`TempData` is  useful for redirection, when data is needed for more than a single request.</span></span>

<span data-ttu-id="021e2-652">Aşağıdaki kod, `TempData` kullanarak `Message` değerini ayarlar:</span><span class="sxs-lookup"><span data-stu-id="021e2-652">The following code sets the value of `Message` using `TempData`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,25&name=snippet_Temp)]

<span data-ttu-id="021e2-653">*Pages/Customers/Index. cshtml* dosyasında aşağıdaki biçimlendirme `Message` değerini `TempData` kullanarak görüntüler.</span><span class="sxs-lookup"><span data-stu-id="021e2-653">The following markup in the *Pages/Customers/Index.cshtml* file displays the value of `Message` using `TempData`.</span></span>

```cshtml
<h3>Msg: @Model.Message</h3>
```

<span data-ttu-id="021e2-654">*Pages/Customers/Index. cshtml. cs* sayfa modeli, `[TempData]` özniteliğini `Message` özelliğine uygular.</span><span class="sxs-lookup"><span data-stu-id="021e2-654">The *Pages/Customers/Index.cshtml.cs* page model applies the `[TempData]` attribute to the `Message` property.</span></span>

```cs
[TempData]
public string Message { get; set; }
```

<span data-ttu-id="021e2-655">Daha fazla bilgi için bkz. [TempData](xref:fundamentals/app-state#tempdata) .</span><span class="sxs-lookup"><span data-stu-id="021e2-655">For more information, see [TempData](xref:fundamentals/app-state#tempdata) .</span></span>

<a name="mhpp"></a>

## <a name="multiple-handlers-per-page"></a><span data-ttu-id="021e2-656">Sayfa başına birden çok işleyici</span><span class="sxs-lookup"><span data-stu-id="021e2-656">Multiple handlers per page</span></span>

<span data-ttu-id="021e2-657">Aşağıdaki sayfa `asp-page-handler` etiketi Yardımcısını kullanarak iki işleyici için biçimlendirme oluşturur:</span><span class="sxs-lookup"><span data-stu-id="021e2-657">The following page generates markup for two handlers using the `asp-page-handler` Tag Helper:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

<!-- Review: the FormActionTagHelper applies to all <form /> elements on a Razor page, even when there's no `asp-` attribute   -->

<span data-ttu-id="021e2-658">Yukarıdaki örnekteki formda, her biri farklı bir URL 'ye göndermek için `FormActionTagHelper` kullanan iki gönderme düğmesi vardır.</span><span class="sxs-lookup"><span data-stu-id="021e2-658">The form in the preceding example has two submit buttons, each using the `FormActionTagHelper` to submit to a different URL.</span></span> <span data-ttu-id="021e2-659">@No__t-0 özniteliği, `asp-page` ' e yönelik bir yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="021e2-659">The `asp-page-handler` attribute is a companion to `asp-page`.</span></span> <span data-ttu-id="021e2-660">`asp-page-handler`, bir sayfa tarafından tanımlanan her bir işleyici yöntemini gönderen URL 'Ler oluşturur.</span><span class="sxs-lookup"><span data-stu-id="021e2-660">`asp-page-handler` generates URLs that submit to each of the handler methods defined by a page.</span></span> <span data-ttu-id="021e2-661">örnek geçerli sayfaya bağlandığından `asp-page` belirtilmedi.</span><span class="sxs-lookup"><span data-stu-id="021e2-661">`asp-page` isn't specified because the sample is linking to the current page.</span></span>

<span data-ttu-id="021e2-662">Sayfa modeli:</span><span class="sxs-lookup"><span data-stu-id="021e2-662">The page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]

<span data-ttu-id="021e2-663">Yukarıdaki kod, *adlandırılmış işleyici yöntemlerini*kullanır.</span><span class="sxs-lookup"><span data-stu-id="021e2-663">The preceding code uses *named handler methods*.</span></span> <span data-ttu-id="021e2-664">Adlandırılmış işleyici yöntemleri, `On<HTTP Verb>` ' dan sonra ve `Async` ' den (varsa) önce, ad içindeki metin alınarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="021e2-664">Named handler methods are created by taking the text in the name after `On<HTTP Verb>` and before `Async` (if present).</span></span> <span data-ttu-id="021e2-665">Yukarıdaki örnekte, Page metotları OnPost**Joinlist**Async ve onpost**Joinlıstuc**Async ' dir.</span><span class="sxs-lookup"><span data-stu-id="021e2-665">In the preceding example, the page methods are OnPost**JoinList**Async and OnPost**JoinListUC**Async.</span></span> <span data-ttu-id="021e2-666">*Onpost* ve *Async* kaldırılmış olarak, işleyici adları `JoinList` ve `JoinListUC` ' dir.</span><span class="sxs-lookup"><span data-stu-id="021e2-666">With *OnPost* and *Async* removed, the handler names are `JoinList` and `JoinListUC`.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?range=12-13)]

<span data-ttu-id="021e2-667">Yukarıdaki kodu kullanarak, `OnPostJoinListAsync` ' a gönderen URL yolu `https://localhost:5001/Customers/CreateFATH?handler=JoinList` ' dir.</span><span class="sxs-lookup"><span data-stu-id="021e2-667">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `https://localhost:5001/Customers/CreateFATH?handler=JoinList`.</span></span> <span data-ttu-id="021e2-668">@No__t-0 ' a gönderen URL yolu `https://localhost:5001/Customers/CreateFATH?handler=JoinListUC` ' dir.</span><span class="sxs-lookup"><span data-stu-id="021e2-668">The URL path that submits to `OnPostJoinListUCAsync` is `https://localhost:5001/Customers/CreateFATH?handler=JoinListUC`.</span></span>

## <a name="custom-routes"></a><span data-ttu-id="021e2-669">Özel yollar</span><span class="sxs-lookup"><span data-stu-id="021e2-669">Custom routes</span></span>

<span data-ttu-id="021e2-670">@No__t-0 yönergesini kullanarak şunları yapın:</span><span class="sxs-lookup"><span data-stu-id="021e2-670">Use the `@page` directive to:</span></span>

* <span data-ttu-id="021e2-671">Sayfaya özel bir yol belirtin.</span><span class="sxs-lookup"><span data-stu-id="021e2-671">Specify a custom route to a page.</span></span> <span data-ttu-id="021e2-672">Örneğin, hakkında sayfasının yolu `@page "/Some/Other/Path"` ile `/Some/Other/Path` olarak ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="021e2-672">For example, the route to the About page can be set to `/Some/Other/Path` with `@page "/Some/Other/Path"`.</span></span>
* <span data-ttu-id="021e2-673">Kesimleri bir sayfanın varsayılan yoluna ekleyin.</span><span class="sxs-lookup"><span data-stu-id="021e2-673">Append segments to a page's default route.</span></span> <span data-ttu-id="021e2-674">Örneğin, bir "öğe" segmenti sayfanın varsayılan yoluna `@page "item"` ile eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="021e2-674">For example, an "item" segment can be added to a page's default route with `@page "item"`.</span></span>
* <span data-ttu-id="021e2-675">Bir sayfanın varsayılan yoluna parametreleri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="021e2-675">Append parameters to a page's default route.</span></span> <span data-ttu-id="021e2-676">Örneğin, `id` olan bir ID parametresi, `@page "{id}"` içeren bir sayfa için gerekli olabilir.</span><span class="sxs-lookup"><span data-stu-id="021e2-676">For example, an ID parameter, `id`, can be required for a page with `@page "{id}"`.</span></span>

<span data-ttu-id="021e2-677">Yolun başındaki bir tilde (`~`) tarafından atanan kök göreli bir yol desteklenir.</span><span class="sxs-lookup"><span data-stu-id="021e2-677">A root-relative path designated by a tilde (`~`) at the beginning of the path is supported.</span></span> <span data-ttu-id="021e2-678">Örneğin, `@page "~/Some/Other/Path"` `@page "/Some/Other/Path"` ' dir.</span><span class="sxs-lookup"><span data-stu-id="021e2-678">For example, `@page "~/Some/Other/Path"` is the same as `@page "/Some/Other/Path"`.</span></span>

<span data-ttu-id="021e2-679">URL 'deki `?handler=JoinList` sorgu dizesini, `@page "{handler?}"` yol şablonunu belirterek, `/JoinList` bir yol kesimine değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="021e2-679">You can change the query string `?handler=JoinList` in the URL to a route segment `/JoinList` by specifying the route template `@page "{handler?}"`.</span></span>

<span data-ttu-id="021e2-680">URL 'de `?handler=JoinList` sorgu dizesini beğenmezseniz, bu yolu URL 'nin yol bölümüne işleyici adını koymak için değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="021e2-680">If you don't like the query string `?handler=JoinList` in the URL, you can change the route to put the handler name in the path portion of the URL.</span></span> <span data-ttu-id="021e2-681">@No__t-0 yönergesinden sonra çift tırnak içine alınmış bir rota şablonu ekleyerek yolu özelleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="021e2-681">You can customize the route by adding a route template enclosed in double quotes after the `@page` directive.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]

<span data-ttu-id="021e2-682">Yukarıdaki kodu kullanarak, `OnPostJoinListAsync` ' a gönderen URL yolu `https://localhost:5001/Customers/CreateFATH/JoinList` ' dir.</span><span class="sxs-lookup"><span data-stu-id="021e2-682">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `https://localhost:5001/Customers/CreateFATH/JoinList`.</span></span> <span data-ttu-id="021e2-683">@No__t-0 ' a gönderen URL yolu `https://localhost:5001/Customers/CreateFATH/JoinListUC` ' dir.</span><span class="sxs-lookup"><span data-stu-id="021e2-683">The URL path that submits to `OnPostJoinListUCAsync` is `https://localhost:5001/Customers/CreateFATH/JoinListUC`.</span></span>

<span data-ttu-id="021e2-684">@No__t-0 `handler`, Route parametresinin isteğe bağlı olduğu anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="021e2-684">The `?` following `handler` means the route parameter is optional.</span></span>

## <a name="configuration-and-settings"></a><span data-ttu-id="021e2-685">Yapılandırma ve ayarlar</span><span class="sxs-lookup"><span data-stu-id="021e2-685">Configuration and settings</span></span>

<span data-ttu-id="021e2-686">Gelişmiş seçenekleri yapılandırmak için, MVC Oluşturucu 'da-0 @no__t genişletme yöntemini kullanın:</span><span class="sxs-lookup"><span data-stu-id="021e2-686">To configure advanced options, use the extension method `AddRazorPagesOptions` on the MVC builder:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/StartupAdvanced.cs?name=snippet_1)]

<span data-ttu-id="021e2-687">Şu anda, sayfalar için kök dizini ayarlamak üzere `RazorPagesOptions` ' ı kullanabilir veya sayfalar için uygulama modeli kuralları ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="021e2-687">Currently you can use the `RazorPagesOptions` to set the root directory for pages, or add application model conventions for pages.</span></span> <span data-ttu-id="021e2-688">Gelecekte bu şekilde daha fazla genişletilebilirlik etkinleştireceğiz.</span><span class="sxs-lookup"><span data-stu-id="021e2-688">We'll enable more extensibility this way in the future.</span></span>

<span data-ttu-id="021e2-689">Görünümleri önceden derlemek için bkz. [Razor görünüm derlemesi](xref:mvc/views/view-compilation) .</span><span class="sxs-lookup"><span data-stu-id="021e2-689">To precompile views, see [Razor view compilation](xref:mvc/views/view-compilation) .</span></span>

<span data-ttu-id="021e2-690">[Örnek kodu indirin veya görüntüleyin](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/index/sample).</span><span class="sxs-lookup"><span data-stu-id="021e2-690">[Download or view sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/index/sample).</span></span>

<span data-ttu-id="021e2-691">Bu giriş hakkında bilgi için bkz. [Razor Pages kullanmaya başlama](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="021e2-691">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start), which builds on this introduction.</span></span>

### <a name="specify-that-razor-pages-are-at-the-content-root"></a><span data-ttu-id="021e2-692">Razor Pages içerik kökünde olduğunu belirtin</span><span class="sxs-lookup"><span data-stu-id="021e2-692">Specify that Razor Pages are at the content root</span></span>

<span data-ttu-id="021e2-693">Varsayılan olarak, Razor Pages */Pages* dizininde kök olarak depolanır.</span><span class="sxs-lookup"><span data-stu-id="021e2-693">By default, Razor Pages are rooted in the */Pages* directory.</span></span> <span data-ttu-id="021e2-694">Razor Pages, uygulamanın [içerik kökünde](xref:fundamentals/index#content-root) ([contentrootpath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) olduğunu belirtmek Için [addmvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) 'ye [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) ekleyin:</span><span class="sxs-lookup"><span data-stu-id="021e2-694">Add [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at the [content root](xref:fundamentals/index#content-root) ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) of the app:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesAtContentRoot();
```

### <a name="specify-that-razor-pages-are-at-a-custom-root-directory"></a><span data-ttu-id="021e2-695">Razor Pages özel kök dizinde olduğunu belirtin</span><span class="sxs-lookup"><span data-stu-id="021e2-695">Specify that Razor Pages are at a custom root directory</span></span>

<span data-ttu-id="021e2-696">Razor Pages uygulamadaki özel bir kök dizinde olduğunu belirtmek için [Addmvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) 'ye [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) ekleyin (göreli bir yol sağlayın):</span><span class="sxs-lookup"><span data-stu-id="021e2-696">Add [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at a custom root directory in the app (provide a relative path):</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesRoot("/path/to/razor/pages");
```

## <a name="additional-resources"></a><span data-ttu-id="021e2-697">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="021e2-697">Additional resources</span></span>

* <xref:index>
* <xref:mvc/views/razor>
* <xref:mvc/controllers/areas>
* <xref:tutorials/razor-pages/razor-pages-start>
* <xref:security/authorization/razor-pages-authorization>
* <xref:razor-pages/razor-pages-conventions>
* <xref:test/razor-pages-tests>
* <xref:mvc/views/partial>

::: moniker-end
