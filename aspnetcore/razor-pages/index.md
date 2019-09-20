---
title: ASP.NET Core Razor Pages giriş
author: Rick-Anderson
description: Nasıl ASP.NET Core Razor sayfalar kodlama sayfa odaklı senaryolar daha kolay ve MVC kullanmaktan daha üretken hale getirdiğini öğrenin.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 09/19/2019
uid: razor-pages/index
ms.openlocfilehash: 284fb0fa64b26cf51f822b9ef42fe9bb7247e421
ms.sourcegitcommit: e7dc89620fa02c2ff80bee1e3f77297f97616968
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71151160"
---
# <a name="introduction-to-razor-pages-in-aspnet-core"></a><span data-ttu-id="5e3fa-103">ASP.NET Core Razor Pages giriş</span><span class="sxs-lookup"><span data-stu-id="5e3fa-103">Introduction to Razor Pages in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="5e3fa-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Ryan şimdi ak](https://github.com/rynowak)</span><span class="sxs-lookup"><span data-stu-id="5e3fa-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Ryan Nowak](https://github.com/rynowak)</span></span>

<span data-ttu-id="5e3fa-105">Razor Pages, kodlama sayfasına odaklanmış senaryolar denetleyicileri ve görünümleri kullanmaktan daha kolay ve daha üretken hale getirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-105">Razor Pages can make coding page-focused scenarios easier and more productive than using controllers and views.</span></span>

<span data-ttu-id="5e3fa-106">Model-View-Controller yaklaşımını kullanan bir öğretici arıyorsanız, bkz. [ASP.NET Core MVC ile çalışmaya başlama](xref:tutorials/first-mvc-app/start-mvc).</span><span class="sxs-lookup"><span data-stu-id="5e3fa-106">If you're looking for a tutorial that uses the Model-View-Controller approach, see [Get started with ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span></span>

<span data-ttu-id="5e3fa-107">Bu belge Razor Pages bir giriş sağlar.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-107">This document provides an introduction to Razor Pages.</span></span> <span data-ttu-id="5e3fa-108">Adım adım öğretici değildir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-108">It's not a step by step tutorial.</span></span> <span data-ttu-id="5e3fa-109">Bölümlerden bazılarını çok gelişmiş bir şekilde bulursanız, bkz. [Razor Pages kullanmaya başlama](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="5e3fa-109">If you find some of the sections too advanced, see [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span></span> <span data-ttu-id="5e3fa-110">ASP.NET Core genel bir bakış için bkz. [ASP.NET Core giriş](xref:index).</span><span class="sxs-lookup"><span data-stu-id="5e3fa-110">For an overview of ASP.NET Core, see the [Introduction to ASP.NET Core](xref:index).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5e3fa-111">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="5e3fa-111">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5e3fa-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5e3fa-112">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5e3fa-113">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5e3fa-113">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="5e3fa-114">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5e3fa-114">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

<a name="rpvs17"></a>

## <a name="create-a-razor-pages-project"></a><span data-ttu-id="5e3fa-115">Razor Pages projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="5e3fa-115">Create a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5e3fa-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5e3fa-116">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="5e3fa-117">Razor Pages projesi oluşturma hakkında ayrıntılı yönergeler için bkz. [Razor Pages kullanmaya başlama](xref:tutorials/razor-pages/razor-pages-start) .</span><span class="sxs-lookup"><span data-stu-id="5e3fa-117">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) for detailed instructions on how to create a Razor Pages project.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5e3fa-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5e3fa-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="5e3fa-119">Komut `dotnet new webapp` satırından çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-119">Run `dotnet new webapp` from the command line.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="5e3fa-120">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5e3fa-120">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="5e3fa-121">Komut `dotnet new webapp` satırından çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-121">Run `dotnet new webapp` from the command line.</span></span>

<span data-ttu-id="5e3fa-122">Oluşturulan *. csproj* dosyasını Mac için Visual Studio açın.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-122">Open the generated *.csproj* file from Visual Studio for Mac.</span></span>

---

## <a name="razor-pages"></a><span data-ttu-id="5e3fa-123">Razor Pages</span><span class="sxs-lookup"><span data-stu-id="5e3fa-123">Razor Pages</span></span>

<span data-ttu-id="5e3fa-124">Razor Pages, *Startup.cs*'de etkinleştirilmiştir:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-124">Razor Pages is enabled in *Startup.cs*:</span></span>

[!code-cs[](index/3.0sample/RazorPagesIntro/Startup.cs?name=snippet_Startup&highlight=12)]

<span data-ttu-id="5e3fa-125">Temel bir sayfa düşünün:<a name="OnGet"></a></span><span class="sxs-lookup"><span data-stu-id="5e3fa-125">Consider a basic page: <a name="OnGet"></a></span></span>

[!code-cshtml[](index/3.0sample/RazorPagesIntro/Pages/Index.cshtml?highlight=1)]

<span data-ttu-id="5e3fa-126">Yukarıdaki kod, denetleyiciler ve görünümlerle ASP.NET Core bir uygulamada kullanılan [Razor görünüm dosyası](xref:tutorials/first-mvc-app/adding-view) gibi bir çok şey arar.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-126">The preceding code looks a lot like a [Razor view file](xref:tutorials/first-mvc-app/adding-view) used in an ASP.NET Core app with controllers and views.</span></span> <span data-ttu-id="5e3fa-127">Bu, [@page](xref:mvc/views/razor#page) farklı kılan yönergedir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-127">What makes it different is the [@page](xref:mvc/views/razor#page) directive.</span></span> <span data-ttu-id="5e3fa-128">`@page`dosyayı bir MVC eylemine dönüştürür. Bu, bir denetleyiciden geçmeden istekleri doğrudan işlediği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-128">`@page` makes the file into an MVC action - which means that it handles requests directly, without going through a controller.</span></span> <span data-ttu-id="5e3fa-129">`@page`sayfada ilk Razor yönergesi olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-129">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="5e3fa-130">`@page`diğer [Razor](xref:mvc/views/razor) yapıları davranışını etkiler.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-130">`@page` affects the behavior of other [Razor](xref:mvc/views/razor) constructs.</span></span> <span data-ttu-id="5e3fa-131">Razor Pages dosya adlarında *. cshtml* soneki vardır.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-131">Razor Pages file names have a *.cshtml* suffix.</span></span>

<span data-ttu-id="5e3fa-132">Bir `PageModel` sınıf kullanan benzer bir sayfa aşağıdaki iki dosyada gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-132">A similar page, using a `PageModel` class, is shown in the following two files.</span></span> <span data-ttu-id="5e3fa-133">*Pages/Index2. cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-133">The *Pages/Index2.cshtml* file:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesIntro/Pages/Index2.cshtml)]

<span data-ttu-id="5e3fa-134">*Pages/Index2. cshtml. cs* sayfa modeli:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-134">The *Pages/Index2.cshtml.cs* page model:</span></span>

[!code-cs[](index/3.0sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]

<span data-ttu-id="5e3fa-135">Kurala göre, `PageModel` sınıf dosyası *. cs* eklenmiş Razor sayfası dosyasıyla aynı ada sahiptir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-135">By convention, the `PageModel` class file has the same name as the Razor Page file with *.cs* appended.</span></span> <span data-ttu-id="5e3fa-136">Örneğin, önceki Razor sayfası *Pages/Index2. cshtml*' dir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-136">For example, the previous Razor Page is *Pages/Index2.cshtml*.</span></span> <span data-ttu-id="5e3fa-137">`PageModel` Sınıfını içeren dosya *sayfa/Index2. cshtml. cs*olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-137">The file containing the `PageModel` class is named *Pages/Index2.cshtml.cs*.</span></span>

<span data-ttu-id="5e3fa-138">URL yollarının sayfalara olan ilişkilendirmeleri, sayfanın dosya sistemindeki konumuna göre belirlenir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-138">The associations of URL paths to pages are determined by the page's location in the file system.</span></span> <span data-ttu-id="5e3fa-139">Aşağıdaki tabloda bir Razor sayfa yolu ve eşleşen URL gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-139">The following table shows a Razor Page path and the matching URL:</span></span>

| <span data-ttu-id="5e3fa-140">Dosya adı ve yolu</span><span class="sxs-lookup"><span data-stu-id="5e3fa-140">File name and path</span></span>               | <span data-ttu-id="5e3fa-141">eşleşen URL</span><span class="sxs-lookup"><span data-stu-id="5e3fa-141">matching URL</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="5e3fa-142">*/Pages/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="5e3fa-142">*/Pages/Index.cshtml*</span></span> | <span data-ttu-id="5e3fa-143">`/` veya `/Index`</span><span class="sxs-lookup"><span data-stu-id="5e3fa-143">`/` or `/Index`</span></span> |
| <span data-ttu-id="5e3fa-144">*/Pages/Contact.exe*</span><span class="sxs-lookup"><span data-stu-id="5e3fa-144">*/Pages/Contact.cshtml*</span></span> | `/Contact` |
| <span data-ttu-id="5e3fa-145">*/Pages/Store/Contact.exe*</span><span class="sxs-lookup"><span data-stu-id="5e3fa-145">*/Pages/Store/Contact.cshtml*</span></span> | `/Store/Contact` |
| <span data-ttu-id="5e3fa-146">*/Pages/Store/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="5e3fa-146">*/Pages/Store/Index.cshtml*</span></span> | <span data-ttu-id="5e3fa-147">`/Store` veya `/Store/Index`</span><span class="sxs-lookup"><span data-stu-id="5e3fa-147">`/Store` or `/Store/Index`</span></span> |

<span data-ttu-id="5e3fa-148">Notlar:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-148">Notes:</span></span>

* <span data-ttu-id="5e3fa-149">Çalışma zamanı, *Sayfalar* klasöründeki Razor Pages dosyaları varsayılan olarak arar.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-149">The runtime looks for Razor Pages files in the *Pages* folder by default.</span></span>
* <span data-ttu-id="5e3fa-150">`Index`, URL bir sayfa içermiyorsa varsayılan sayfasıdır.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-150">`Index` is the default page when a URL doesn't include a page.</span></span>

## <a name="write-a-basic-form"></a><span data-ttu-id="5e3fa-151">Temel form yazma</span><span class="sxs-lookup"><span data-stu-id="5e3fa-151">Write a basic form</span></span>

<span data-ttu-id="5e3fa-152">Razor Pages, Web tarayıcıları ile kullanılan ortak desenleri bir uygulama oluştururken kolayca uygulanması için tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-152">Razor Pages is designed to make common patterns used with web browsers easy to implement when building an app.</span></span> <span data-ttu-id="5e3fa-153">[Model bağlama](xref:mvc/models/model-binding), [ETIKET yardımcıları](xref:mvc/views/tag-helpers/intro)ve HTML Yardımcıları hepsi, Razor sayfası sınıfında tanımlanan özelliklerle *çalışır* .</span><span class="sxs-lookup"><span data-stu-id="5e3fa-153">[Model binding](xref:mvc/models/model-binding), [Tag Helpers](xref:mvc/views/tag-helpers/intro), and HTML helpers all *just work* with the properties defined in a Razor Page class.</span></span> <span data-ttu-id="5e3fa-154">`Contact` Model için temel bir "bize başvurun" formu uygulayan bir sayfa düşünün:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-154">Consider a page that implements a basic "contact us" form for the `Contact` model:</span></span>

<span data-ttu-id="5e3fa-155">Bu belgedeki `DbContext` örnekler için, [Startup.cs](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/razor-pages/index/3.0sample/RazorPagesContacts/Startup.cs#L23-L24) dosyasında başlatılır.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-155">For the samples in this document, the `DbContext` is initialized in the [Startup.cs](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/razor-pages/index/3.0sample/RazorPagesContacts/Startup.cs#L23-L24) file.</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Startup.cs?name=snippet)]

<span data-ttu-id="5e3fa-156">Veri modeli:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-156">The data model:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Models/Customer.cs)]

<span data-ttu-id="5e3fa-157">DB bağlamı:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-157">The db context:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Data/CustomerDbContext.cs)]

<span data-ttu-id="5e3fa-158">*Pages/Create. cshtml* görünüm dosyası:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-158">The *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml)]

<span data-ttu-id="5e3fa-159">*Pages/Create. cshtml. cs* sayfa modeli:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-159">The *Pages/Create.cshtml.cs* page model:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml.cs?name=snippet_ALL)]

<span data-ttu-id="5e3fa-160">Kuralına göre, `PageModel` sınıfı çağrılır `<PageName>Model` ve sayfayla aynı ad alanında bulunur.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-160">By convention, the `PageModel` class is called `<PageName>Model` and is in the same namespace as the page.</span></span>

<span data-ttu-id="5e3fa-161">`PageModel` Sınıfı, bir sayfanın mantığının sunumuna ayrılmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-161">The `PageModel` class allows separation of the logic of a page from its presentation.</span></span> <span data-ttu-id="5e3fa-162">Sayfaya gönderilen istekler için sayfa işleyicilerini ve sayfayı işlemek için kullanılan verileri tanımlar.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-162">It defines page handlers for requests sent to the page and the data used to render the page.</span></span> <span data-ttu-id="5e3fa-163">Bu ayrım şunları sağlar:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-163">This separation allows:</span></span>

* <span data-ttu-id="5e3fa-164">[Bağımlılık ekleme](xref:fundamentals/dependency-injection)yoluyla sayfa bağımlılıklarını yönetme.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-164">Managing of page dependencies through [dependency injection](xref:fundamentals/dependency-injection).</span></span>
* [<span data-ttu-id="5e3fa-165">Birim testi</span><span class="sxs-lookup"><span data-stu-id="5e3fa-165">Unit testing</span></span>](xref:test/razor-pages-tests)

<span data-ttu-id="5e3fa-166">Sayfada, istekler üzerinde `OnPostAsync` `POST` çalışan bir *işleyici yöntemi*vardır (bir Kullanıcı formu gönderdiğinde).</span><span class="sxs-lookup"><span data-stu-id="5e3fa-166">The page has an `OnPostAsync` *handler method*, which runs on `POST` requests (when a user posts the form).</span></span> <span data-ttu-id="5e3fa-167">Herhangi bir HTTP fiili için işleyici metotları eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-167">Handler methods for any HTTP verb can be added.</span></span> <span data-ttu-id="5e3fa-168">En yaygın işleyiciler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-168">The most common handlers are:</span></span>

* <span data-ttu-id="5e3fa-169">`OnGet`sayfa için gereken durumu başlatmak için.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-169">`OnGet` to initialize state needed for the page.</span></span> <span data-ttu-id="5e3fa-170">Yukarıdaki kodda, `OnGet` yöntemi *CreateModel. cshtml* Razor sayfasını görüntüler.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-170">In the preceding code, the `OnGet` method displays the *CreateModel.cshtml* Razor Page.</span></span>
* <span data-ttu-id="5e3fa-171">`OnPost`form gönderilerini işlemek için.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-171">`OnPost` to handle form submissions.</span></span>

<span data-ttu-id="5e3fa-172">`Async` Adlandırma son eki isteğe bağlıdır, ancak genellikle zaman uyumsuz işlevler için kural tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-172">The `Async` naming suffix is optional but is often used by convention for asynchronous functions.</span></span> <span data-ttu-id="5e3fa-173">Yukarıdaki kod Razor Pages için tipik bir davranıştır.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-173">The preceding code is typical for Razor Pages.</span></span>

<span data-ttu-id="5e3fa-174">Denetleyicileri ve görünümleri kullanarak ASP.NET uygulamaları hakkında bilginiz varsa:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-174">If you're familiar with ASP.NET apps using controllers and views:</span></span>

* <span data-ttu-id="5e3fa-175">Yukarıdaki `OnPostAsync` örnekteki kod, tipik denetleyici koduna benzer şekilde görünür.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-175">The `OnPostAsync` code in the preceding example looks similar to typical controller code.</span></span>
* <span data-ttu-id="5e3fa-176">[Model bağlama](xref:mvc/models/model-binding), [doğrulama](xref:mvc/models/validation)ve eylem sonuçları gibi mvc temel elemanlarının çoğu denetleyiciler ve Razor Pages aynı şekilde çalışır.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-176">Most of the MVC primitives like [model binding](xref:mvc/models/model-binding), [validation](xref:mvc/models/validation), and action results work the same with Controllers and Razor Pages.</span></span> 

<span data-ttu-id="5e3fa-177">Önceki `OnPostAsync` Yöntem:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-177">The previous `OnPostAsync` method:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="5e3fa-178">Temel akışı `OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-178">The basic flow of `OnPostAsync`:</span></span>

<span data-ttu-id="5e3fa-179">Doğrulama hatalarını kontrol edin.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-179">Check for validation errors.</span></span>

* <span data-ttu-id="5e3fa-180">Hata yoksa, verileri kaydedin ve yeniden yönlendirin.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-180">If there are no errors, save the data and redirect.</span></span>
* <span data-ttu-id="5e3fa-181">Hatalar varsa, doğrulama iletileriyle sayfayı yeniden görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-181">If there are errors, show the page again with validation messages.</span></span> <span data-ttu-id="5e3fa-182">Çoğu durumda, istemci üzerinde doğrulama hataları algılanır ve sunucuya hiçbir zaman gönderilmez.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-182">In many cases, validation errors would be detected on the client, and never submitted to the server.</span></span>

<span data-ttu-id="5e3fa-183">*Pages/Create. cshtml* görünüm dosyası:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-183">The *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml)]

<span data-ttu-id="5e3fa-184">Sayfalardan işlenmiş HTML */Create. cshtml*:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-184">The rendered HTML from *Pages/Create.cshtml*:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create4.html)]

<span data-ttu-id="5e3fa-185">Önceki kodda, formu deftere nakletme:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-185">In the previous code, posting the form:</span></span>

* <span data-ttu-id="5e3fa-186">Geçerli verilerle:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-186">With valid data:</span></span>

  * <span data-ttu-id="5e3fa-187">Handler yöntemi <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel.RedirectToPage*> yardımcı yöntemini çağırır. `OnPostAsync`</span><span class="sxs-lookup"><span data-stu-id="5e3fa-187">The `OnPostAsync` handler method calls the <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel.RedirectToPage*> helper method.</span></span> <span data-ttu-id="5e3fa-188">`RedirectToPage`bir örneğini <xref:Microsoft.AspNetCore.Mvc.RedirectToPageResult>döndürür.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-188">`RedirectToPage` returns an instance of <xref:Microsoft.AspNetCore.Mvc.RedirectToPageResult>.</span></span> <span data-ttu-id="5e3fa-189">`RedirectToPage`:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-189">`RedirectToPage`:</span></span>

    * <span data-ttu-id="5e3fa-190">Bir eylem sonucudur.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-190">Is an action result.</span></span>
    * <span data-ttu-id="5e3fa-191">, Veya `RedirectToAction` `RedirectToRoute` ile benzerdir (denetleyiciler ve görünümlerde kullanılır).</span><span class="sxs-lookup"><span data-stu-id="5e3fa-191">Is similar to `RedirectToAction` or `RedirectToRoute` (used in controllers and views).</span></span>
    * <span data-ttu-id="5e3fa-192">Sayfalar için özelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-192">Is customized for pages.</span></span> <span data-ttu-id="5e3fa-193">Yukarıdaki örnekte, kök dizin sayfasına (`/Index`) yeniden yönlendiriliyor.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-193">In the preceding sample, it redirects to the root Index page (`/Index`).</span></span> <span data-ttu-id="5e3fa-194">`RedirectToPage`, [Sayfalar Için URL oluşturma](#url_gen) bölümünde ayrıntılı olarak açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-194">`RedirectToPage` is detailed in the [URL generation for Pages](#url_gen) section.</span></span>

* <span data-ttu-id="5e3fa-195">Sunucuya geçirilen doğrulama hatalarıyla birlikte:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-195">With validation errors that are passed to the server:</span></span>

  * <span data-ttu-id="5e3fa-196">Handler yöntemi <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageBase.Page*> yardımcı yöntemini çağırır. `OnPostAsync`</span><span class="sxs-lookup"><span data-stu-id="5e3fa-196">The `OnPostAsync` handler method calls the <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageBase.Page*> helper method.</span></span> <span data-ttu-id="5e3fa-197">`Page`bir örneğini <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageResult>döndürür.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-197">`Page` returns an instance of <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageResult>.</span></span> <span data-ttu-id="5e3fa-198">Döndürme `Page` , denetleyicilerde eylemlerin nasıl dönüşlerine `View`benzer.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-198">Returning `Page` is similar to how actions in controllers return `View`.</span></span> <span data-ttu-id="5e3fa-199">`PageResult`, bir işleyici yöntemi için varsayılan dönüş türüdür.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-199">`PageResult` is the default return type for a handler method.</span></span> <span data-ttu-id="5e3fa-200">Döndüren `void` bir işleyici yöntemi sayfayı işler.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-200">A handler method that returns `void` renders the page.</span></span>
  * <span data-ttu-id="5e3fa-201">Yukarıdaki örnekte, formun hiçbir değer olmadan nakledilmesi [ModelState ile sonuçlanır. IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) yanlış döndürüyor.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-201">In the preceding example, posting the form with no value results in [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) returning false.</span></span> <span data-ttu-id="5e3fa-202">Bu örnekte, istemcide hiçbir doğrulama hatası gösterilmezler.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-202">In this sample, no validation errors are displayed on the client.</span></span> <span data-ttu-id="5e3fa-203">Doğrulama hatası teslim etme bu belgenin ilerleyen bölümlerinde ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-203">Validation error handing is covered later in this document.</span></span>

  [!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=3-6)]

* <span data-ttu-id="5e3fa-204">İstemci tarafı doğrulaması tarafından algılanan doğrulama hatalarıyla birlikte:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-204">With validation errors detected by client side validation:</span></span>

  * <span data-ttu-id="5e3fa-205">Veriler sunucuya **nakledilmedi.**</span><span class="sxs-lookup"><span data-stu-id="5e3fa-205">Data is **not** posted to the server.</span></span>
  * <span data-ttu-id="5e3fa-206">İstemci tarafı doğrulaması bu belgenin ilerleyen kısımlarında açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-206">Client-side validation is explained later in this document.</span></span>

<span data-ttu-id="5e3fa-207">Özelliği `Customer` , model [`[BindProperty]`](xref:Microsoft.AspNetCore.Mvc.BindPropertyAttribute) bağlamasını kabul etmek için özniteliğini kullanır:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-207">The `Customer` property uses [`[BindProperty]`](xref:Microsoft.AspNetCore.Mvc.BindPropertyAttribute) attribute to opt in to model binding:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml.cs?name=snippet_PageModel&highlight=15-16)]

<span data-ttu-id="5e3fa-208">`[BindProperty]`istemci tarafından değiştirilmemesi gereken özellikler içeren **modellerde kullanılmamalıdır.**</span><span class="sxs-lookup"><span data-stu-id="5e3fa-208">`[BindProperty]` should **not** be used on models containing properties that should not be changed by the client.</span></span> <span data-ttu-id="5e3fa-209">Daha fazla bilgi için bkz. fazla [nakil](xref:data/ef-rp/crud#overposting)</span><span class="sxs-lookup"><span data-stu-id="5e3fa-209">For more information, see [Overposting](xref:data/ef-rp/crud#overposting)</span></span>

<span data-ttu-id="5e3fa-210">Razor Pages, varsayılan olarak yalnızca`GET` fiiller olmayan özellikleri bağlayın.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-210">Razor Pages, by default, bind properties only with non-`GET` verbs.</span></span> <span data-ttu-id="5e3fa-211">Özelliklere bağlama, HTTP verilerini model türüne dönüştürmek için kod yazma ihtiyacını ortadan kaldırır.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-211">Binding to properties removes the need to writing code to convert HTTP data to the model type.</span></span> <span data-ttu-id="5e3fa-212">Bağlama, form alanlarını işlemek için aynı özelliği kullanarak kodu azaltır (`<input asp-for="Customer.Name">`) ve girişi kabul eder.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-212">Binding reduces code by using the same property to render form fields (`<input asp-for="Customer.Name">`) and accept the input.</span></span>

[!INCLUDE[](~/includes/bind-get.md)]

<span data-ttu-id="5e3fa-213">*Sayfalar/oluşturma. cshtml* görünüm dosyası gözden geçiriliyor:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-213">Reviewing the *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml?highlight=3,9)]

* <span data-ttu-id="5e3fa-214">Yukarıdaki kodda, [giriş etiketi Yardımcısı](xref:mvc/views/working-with-forms#the-input-tag-helper) `<input asp-for="Customer.Name" />` `Customer.Name` HTML `<input>` öğesini model ifadesine bağlar.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-214">In the preceding code, the [input tag helper](xref:mvc/views/working-with-forms#the-input-tag-helper) `<input asp-for="Customer.Name" />` binds the HTML `<input>` element to the `Customer.Name` model expression.</span></span>
* <span data-ttu-id="5e3fa-215">[`@addTagHelper`](xref:mvc/views/tag-helpers/intro#addtaghelper-makes-tag-helpers-available)Etiket Yardımcıları kullanılabilir hale getirir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-215">[`@addTagHelper`](xref:mvc/views/tag-helpers/intro#addtaghelper-makes-tag-helpers-available) makes Tag Helpers available.</span></span>

### <a name="the-home-page"></a><span data-ttu-id="5e3fa-216">Giriş sayfası</span><span class="sxs-lookup"><span data-stu-id="5e3fa-216">The home page</span></span>

<span data-ttu-id="5e3fa-217">*Index. cshtml* giriş sayfasıdır:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-217">*Index.cshtml* is the home page:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Index.cshtml)]

<span data-ttu-id="5e3fa-218">İlişkili `PageModel` Sınıf (*Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="5e3fa-218">The associated `PageModel` class (*Index.cshtml.cs*):</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Index.cshtml.cs?name=snippet)]

<span data-ttu-id="5e3fa-219">*Index. cshtml* dosyası aşağıdaki biçimlendirmeyi içerir:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-219">The *Index.cshtml* file contains the following markup:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Index.cshtml?range=21)]

<span data-ttu-id="5e3fa-220">[Tutturucu etiketi Yardımcısı,](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) düzenleme sayfasına bir bağlantı oluşturmak için özniteliğinikullandı.`asp-route-{value}` `<a /a>`</span><span class="sxs-lookup"><span data-stu-id="5e3fa-220">The `<a /a>`[Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) used the `asp-route-{value}` attribute to generate a link to the Edit page.</span></span> <span data-ttu-id="5e3fa-221">Bağlantı, iletişim KIMLIĞINE sahip rota verileri içerir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-221">The link contains route data with the contact ID.</span></span> <span data-ttu-id="5e3fa-222">Örneğin, uygulamasında yönetilen Hyper-V konakları olarak eklemek için aşağıdaki yordamı kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-222">For example, `https://localhost:5001/Edit/1`.</span></span> <span data-ttu-id="5e3fa-223">[Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro), Razor dosyalarında HTML öğelerinin oluşturulmasına ve işlenmesine sunucu tarafı kodun katılmasını etkinleştir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-223">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span>

<span data-ttu-id="5e3fa-224">*Index. cshtml* dosyası her müşteri için bir silme düğmesi oluşturmak için biçimlendirme içerir:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-224">The *Index.cshtml* file contains markup to create a delete button for each customer contact:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Index.cshtml?range=22-23)]

<span data-ttu-id="5e3fa-225">İşlenmiş HTML:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-225">The rendered HTML:</span></span>

```HTML
<button type="submit" formaction="/Customers?id=1&amp;handler=delete">delete</button>
```

<span data-ttu-id="5e3fa-226">Sil düğmesi HTML 'de işlendiğinde, bu nesnenin [biçimlendirme](https://developer.mozilla.org/docs/Web/HTML/Element/button#attr-formaction) parametreleri içerir:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-226">When the delete button is rendered in HTML, its [formaction](https://developer.mozilla.org/docs/Web/HTML/Element/button#attr-formaction) includes parameters for:</span></span>

* <span data-ttu-id="5e3fa-227">`asp-route-id` Özniteliği tarafından belirtilen müşteri iletişim kimliği.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-227">The customer contact ID, specified by the `asp-route-id` attribute.</span></span>
* <span data-ttu-id="5e3fa-228">`handler`, Özniteliği`asp-page-handler` tarafından belirtilen.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-228">The `handler`, specified by the `asp-page-handler` attribute.</span></span>

<span data-ttu-id="5e3fa-229">Düğme seçildiğinde, sunucuya bir form `POST` isteği gönderilir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-229">When the button is selected, a form `POST` request is sent to the server.</span></span> <span data-ttu-id="5e3fa-230">Kurala göre, işleyici yönteminin adı, şemaya `handler` `OnPost[handler]Async`göre parametrenin değerine göre seçilir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-230">By convention, the name of the handler method is selected based on the value of the `handler` parameter according to the scheme `OnPost[handler]Async`.</span></span>

<span data-ttu-id="5e3fa-231">Bu örnekte olduğundan ,`POST` isteği işlemek için işleyiciyöntemikullanılır`OnPostDeleteAsync`. `handler` `delete`</span><span class="sxs-lookup"><span data-stu-id="5e3fa-231">Because the `handler` is `delete` in this example, the `OnPostDeleteAsync` handler method is used to process the `POST` request.</span></span> <span data-ttu-id="5e3fa-232">, Gibi farklı bir değere `remove`ayarlandıysa, adında `OnPostRemoveAsync` bir işleyici yöntemi seçilir. `asp-page-handler`</span><span class="sxs-lookup"><span data-stu-id="5e3fa-232">If the `asp-page-handler` is set to a different value, such as `remove`, a handler method with the name `OnPostRemoveAsync` is selected.</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Index.cshtml.cs?name=snippet2)]

<span data-ttu-id="5e3fa-233">`OnPostDeleteAsync` Yöntemi:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-233">The `OnPostDeleteAsync` method:</span></span>

* <span data-ttu-id="5e3fa-234">`id` Sorgu dizesinden alır.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-234">Gets the `id` from the query string.</span></span>
* <span data-ttu-id="5e3fa-235">Müşteri iletişim `FindAsync`için veritabanını sorgular.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-235">Queries the database for the customer contact with `FindAsync`.</span></span>
* <span data-ttu-id="5e3fa-236">Müşteri ilgili kişisi bulunursa, kaldırılır ve veritabanı güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-236">If the customer contact is found, it's removed and the database is updated.</span></span>
* <span data-ttu-id="5e3fa-237">Kök <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel.RedirectToPage*> dizin sayfasına (`/Index`) yeniden yönlendirmek için çağrılar.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-237">Calls <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel.RedirectToPage*> to redirect to the root Index page (`/Index`).</span></span>

### <a name="the-editcshtml-file"></a><span data-ttu-id="5e3fa-238">Edit. cshtml dosyası</span><span class="sxs-lookup"><span data-stu-id="5e3fa-238">The Edit.cshtml file</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Edit.cshtml?highlight=1)]

<span data-ttu-id="5e3fa-239">İlk satır `@page "{id:int}"` yönergesini içerir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-239">The first line contains the `@page "{id:int}"` directive.</span></span> <span data-ttu-id="5e3fa-240">Yönlendirme kısıtlaması`"{id:int}"` , sayfada `int` yönlendirme verileri içeren sayfaya istekleri kabul etmesini söyler.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-240">The routing constraint`"{id:int}"` tells the page to accept requests to the page that contain `int` route data.</span></span> <span data-ttu-id="5e3fa-241">Sayfaya yapılan bir istek öğesine `int`dönüştürülebileceği rota verileri içermiyorsa, çalışma zamanı bir HTTP 404 (bulunamadı) hatası döndürür.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-241">If a request to the page doesn't contain route data that can be converted to an `int`, the runtime returns an HTTP 404 (not found) error.</span></span> <span data-ttu-id="5e3fa-242">Kimliği isteğe bağlı yapmak için yol kısıtlamasına `?` ekleyin:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-242">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="5e3fa-243">*Edit.cshtml.cs* dosyası:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-243">The *Edit.cshtml.cs* file:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Edit.cshtml.cs?name=snippet)]

## <a name="validation"></a><span data-ttu-id="5e3fa-244">Doğrulama</span><span class="sxs-lookup"><span data-stu-id="5e3fa-244">Validation</span></span>

<span data-ttu-id="5e3fa-245">Doğrulama kuralları:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-245">Validation rules:</span></span>

* <span data-ttu-id="5e3fa-246">Model sınıfında bildirimli olarak belirtilir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-246">Are declaratively specified in the model class.</span></span>
* <span data-ttu-id="5e3fa-247">Uygulamada her yerde zorlanır.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-247">Are enforced everywhere in the app.</span></span>

<span data-ttu-id="5e3fa-248">Ad <xref:System.ComponentModel.DataAnnotations> alanı, bir sınıfa veya özelliğe bildirimli olarak uygulanan bir yerleşik doğrulama öznitelikleri kümesi sağlar.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-248">The <xref:System.ComponentModel.DataAnnotations> namespace provides a set of built-in validation attributes that are applied declaratively to a class or property.</span></span> <span data-ttu-id="5e3fa-249">Dataaçıklamalarda, biçimlendirme ile ilgili Yardım [`[DataType]`](xref:System.ComponentModel.DataAnnotations.DataTypeAttribute) ve herhangi bir doğrulama sağlamayan gibi biçimlendirme öznitelikleri de bulunur.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-249">DataAnnotations also contains formatting attributes like [`[DataType]`](xref:System.ComponentModel.DataAnnotations.DataTypeAttribute) that help with formatting and don't provide any validation.</span></span>

<span data-ttu-id="5e3fa-250">`Customer` Modeli göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-250">Consider the `Customer` model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Data/Customer.cs)]

<span data-ttu-id="5e3fa-251">Aşağıdaki *Create. cshtml* görünüm dosyasını kullanarak:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-251">Using the following *Create.cshtml* view file:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create3.cshtml?highlight=3,8-9,15-99)]

<span data-ttu-id="5e3fa-252">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-252">The preceding code:</span></span>

* <span data-ttu-id="5e3fa-253">JQuery ve jQuery doğrulama betikleri içerir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-253">Includes jQuery and jQuery validation scripts.</span></span>
* <span data-ttu-id="5e3fa-254">' İ etkinleştirmek `<span />` için ve [Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro) kullanır: `<div />`</span><span class="sxs-lookup"><span data-stu-id="5e3fa-254">Uses the `<div />` and `<span />` [Tag Helpers](xref:mvc/views/tag-helpers/intro) to enable:</span></span>

  * <span data-ttu-id="5e3fa-255">İstemci tarafı doğrulama.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-255">Client-side validation.</span></span>
  * <span data-ttu-id="5e3fa-256">Doğrulama hatası işleme.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-256">Validation error rendering.</span></span>

* <span data-ttu-id="5e3fa-257">Aşağıdaki HTML 'yi oluşturur:[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create5.html)]</span><span class="sxs-lookup"><span data-stu-id="5e3fa-257">Generates the following HTML: [!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create5.html)]</span></span>

<span data-ttu-id="5e3fa-258">Create formunu ad değeri olmadan göndermek "ad alanı gereklidir" hata iletisini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-258">Posting the Create form without a name value displays the error message "The Name field is required."</span></span> <span data-ttu-id="5e3fa-259">formunda.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-259">on the form.</span></span> <span data-ttu-id="5e3fa-260">İstemcide JavaScript etkinse tarayıcı, sunucuya göndermeden hatayı görüntüler.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-260">If JavaScript is enabled on the client, the browser displays the error without posting to the server.</span></span>

<span data-ttu-id="5e3fa-261">Özniteliği işlenmiş html `data-val-length-max="10"` üzerinde oluşturulur. `[StringLength(10)]`</span><span class="sxs-lookup"><span data-stu-id="5e3fa-261">The `[StringLength(10)]` attribute generates `data-val-length-max="10"` on the rendered HTML.</span></span> <span data-ttu-id="5e3fa-262">`data-val-length-max`tarayıcıların belirtilen uzunluk üst sınırından fazlasını girmesini engeller.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-262">`data-val-length-max` prevents browsers from entering more than the maximum length specified.</span></span> <span data-ttu-id="5e3fa-263">Gönderiyi düzenlemek ve yeniden oynatmak için [Fiddler](https://www.telerik.com/fiddler) gibi bir araç kullanılıyorsa:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-263">If a tool such as [Fiddler](https://www.telerik.com/fiddler) is used to edit and replay the post:</span></span>

* <span data-ttu-id="5e3fa-264">, Adı 10 ' dan daha uzun.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-264">With the name longer than 10.</span></span>
* <span data-ttu-id="5e3fa-265">"Alan adı, en fazla 10 uzunluğunda bir dize olmalıdır" hata iletisi.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-265">The error message "The field Name must be a string with a maximum length of 10."</span></span> <span data-ttu-id="5e3fa-266">döndürülür.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-266">is returned.</span></span>

<span data-ttu-id="5e3fa-267">Aşağıdaki `Movie` modeli göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-267">Consider the following `Movie` model:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Models/MovieDateRatingDA.cs?name=snippet1)]

<span data-ttu-id="5e3fa-268">Doğrulama öznitelikleri, uygulanan model özellikleri üzerinde zorlamak için davranışı belirtir:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-268">The validation attributes specify behavior to enforce on the model properties they're applied to:</span></span>

* <span data-ttu-id="5e3fa-269">`Required` Ve`MinimumLength` öznitelikleri bir özelliğin bir değere sahip olması gerektiğini belirtir; ancak hiçbir şey, bir kullanıcının bu doğrulamayı karşılamak için boşluk girmesini engeller.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-269">The `Required` and `MinimumLength` attributes indicate that a property must have a value; but nothing prevents a user from entering white space to satisfy this validation.</span></span>
* <span data-ttu-id="5e3fa-270">`RegularExpression` Öznitelik, hangi karakterlerin girişi yapabileceğini sınırlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-270">The `RegularExpression` attribute is used to limit what characters can be input.</span></span> <span data-ttu-id="5e3fa-271">Yukarıdaki kodda, "tarz":</span><span class="sxs-lookup"><span data-stu-id="5e3fa-271">In the preceding code, "Genre":</span></span>

  * <span data-ttu-id="5e3fa-272">Yalnızca harfler kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-272">Must only use letters.</span></span>
  * <span data-ttu-id="5e3fa-273">İlk harfin büyük harfle olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-273">The first letter is required to be uppercase.</span></span> <span data-ttu-id="5e3fa-274">Boşluk, sayı ve özel karakterlere izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-274">White space, numbers, and special characters are not allowed.</span></span>

* <span data-ttu-id="5e3fa-275">`RegularExpression` "Derecelendirme":</span><span class="sxs-lookup"><span data-stu-id="5e3fa-275">The `RegularExpression` "Rating":</span></span>

  * <span data-ttu-id="5e3fa-276">İlk karakterin büyük harf olmasını gerektirir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-276">Requires that the first character be an uppercase letter.</span></span>
  * <span data-ttu-id="5e3fa-277">Sonraki boşlukların içindeki özel karakter ve sayılara izin verir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-277">Allows special characters and numbers in subsequent spaces.</span></span> <span data-ttu-id="5e3fa-278">"PG-13" bir derecelendirme için geçerlidir, ancak bir "tarz" için başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-278">"PG-13" is valid for a rating, but fails for a "Genre".</span></span>

* <span data-ttu-id="5e3fa-279">`Range` Özniteliği bir değeri belirtilen bir Aralık içinde kısıtlar.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-279">The `Range` attribute constrains a value to within a specified range.</span></span>
* <span data-ttu-id="5e3fa-280">`StringLength` Özniteliği bir dize özelliğinin uzunluk üst sınırını ve isteğe bağlı olarak en düşük uzunluğunu ayarlar.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-280">The `StringLength` attribute sets the maximum length of a string property, and optionally its minimum length.</span></span>
* <span data-ttu-id="5e3fa-281">Değer türleri (örneğin, `decimal`, `int`, `float` `DateTime`), doğal olarak gereklidir ve `[Required]` özniteliğe gerek kalmaz.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-281">Value types (such as `decimal`, `int`, `float`, `DateTime`) are inherently required and don't need the `[Required]` attribute.</span></span>

<span data-ttu-id="5e3fa-282">`Movie` Model için Oluştur sayfasında, geçersiz değerlere sahip hatalar görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-282">The Create page for the `Movie` model shows displays errors with invalid values:</span></span>

![Birden çok jQuery istemci tarafı doğrulama hatası içeren film görünümü formu](~/tutorials/razor-pages/validation/_static/val.png)

<span data-ttu-id="5e3fa-284">Daha fazla bilgi için bkz.:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-284">For more information, see:</span></span>

* [<span data-ttu-id="5e3fa-285">Film uygulamasına doğrulama ekleme</span><span class="sxs-lookup"><span data-stu-id="5e3fa-285">Add validation to the Movie app</span></span>](xref:tutorials/razor-pages/validation)
* <span data-ttu-id="5e3fa-286">[ASP.NET Core 'de model doğrulaması](xref:mvc/models/validation).</span><span class="sxs-lookup"><span data-stu-id="5e3fa-286">[Model validation in ASP.NET Core](xref:mvc/models/validation).</span></span>

## <a name="handle-head-requests-with-an-onget-handler-fallback"></a><span data-ttu-id="5e3fa-287">OnGet işleyicisi geri dönüşü ile tanıtıcı HEAD istekleri</span><span class="sxs-lookup"><span data-stu-id="5e3fa-287">Handle HEAD requests with an OnGet handler fallback</span></span>

<span data-ttu-id="5e3fa-288">`HEAD`istekler belirli bir kaynak için üstbilgileri almaya izin verir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-288">`HEAD` requests allow retrieving the headers for a specific resource.</span></span> <span data-ttu-id="5e3fa-289">İsteklerin aksine `GET`isteklerbiryanıtgövdesi döndürmez.`HEAD`</span><span class="sxs-lookup"><span data-stu-id="5e3fa-289">Unlike `GET` requests, `HEAD` requests don't return a response body.</span></span>

<span data-ttu-id="5e3fa-290">Normalde, istekler `OnHead` için `HEAD` bir işleyici oluşturulur ve çağırılır:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-290">Ordinarily, an `OnHead` handler is created and called for `HEAD` requests:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Privacy.cshtml.cs?name=snippet)]

<span data-ttu-id="5e3fa-291">Razor Pages, işleyici tanımlanmadığında `OnGet` `OnHead` işleyiciyi çağırmaya geri döner.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-291">Razor Pages falls back to calling the `OnGet` handler if no `OnHead` handler is defined.</span></span>

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a><span data-ttu-id="5e3fa-292">XSRF/CSRF ve Razor Pages</span><span class="sxs-lookup"><span data-stu-id="5e3fa-292">XSRF/CSRF and Razor Pages</span></span>

<span data-ttu-id="5e3fa-293">Razor Pages,[Antiforgery doğrulaması](xref:security/anti-request-forgery)tarafından korunur.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-293">Razor Pages are protected by[Antiforgery validation](xref:security/anti-request-forgery).</span></span> <span data-ttu-id="5e3fa-294">[Formtaghelper](xref:mvc/views/working-with-forms#the-form-tag-helper) , antiforgery belirteçlerini HTML form öğelerine çıkartır.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-294">The [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) injects antiforgery tokens into HTML form elements.</span></span>

<a name="layout"></a>

## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a><span data-ttu-id="5e3fa-295">Razor Pages ile düzenleri, partileri, şablonları ve etiket yardımcılarını kullanma</span><span class="sxs-lookup"><span data-stu-id="5e3fa-295">Using Layouts, partials, templates, and Tag Helpers with Razor Pages</span></span>

<span data-ttu-id="5e3fa-296">Sayfalar, Razor görünüm altyapısının tüm özellikleri ile çalışır.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-296">Pages work with all the capabilities of the Razor view engine.</span></span> <span data-ttu-id="5e3fa-297">Düzenler, partıals, şablonlar, etiket yardımcıları, *_Viewstart. cshtml*, *_viewwimports. cshtml* geleneksel Razor görünümlerinde oldukları gibi çalışır.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-297">Layouts, partials, templates, Tag Helpers, *_ViewStart.cshtml*, *_ViewImports.cshtml* work in the same way they do for conventional Razor views.</span></span>

<span data-ttu-id="5e3fa-298">Bu özelliklerden bazılarının avantajlarından yararlanarak bu sayfayı declutter edelim.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-298">Let's declutter this page by taking advantage of some of those capabilities.</span></span>

<span data-ttu-id="5e3fa-299">*Pages/Shared/_Layout. cshtml*öğesine bir [Düzen sayfası](xref:mvc/views/layout) ekleyin:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-299">Add a [layout page](xref:mvc/views/layout) to *Pages/Shared/_Layout.cshtml*:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Shared/_Layout2.cshtml?hightlight=12)]

<span data-ttu-id="5e3fa-300">[Düzen](xref:mvc/views/layout):</span><span class="sxs-lookup"><span data-stu-id="5e3fa-300">The [Layout](xref:mvc/views/layout):</span></span>

* <span data-ttu-id="5e3fa-301">Her sayfanın yerleşimini denetler (sayfa düzen dışında değilse).</span><span class="sxs-lookup"><span data-stu-id="5e3fa-301">Controls the layout of each page (unless the page opts out of layout).</span></span>
* <span data-ttu-id="5e3fa-302">JavaScript ve stil sayfaları gibi HTML yapılarını içeri aktarır.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-302">Imports HTML structures such as JavaScript and stylesheets.</span></span>
* <span data-ttu-id="5e3fa-303">Razor sayfasının içerikleri, çağrıldığında işlenir `@RenderBody()` .</span><span class="sxs-lookup"><span data-stu-id="5e3fa-303">The contents of the Razor page are rendered where `@RenderBody()` is called.</span></span>

<span data-ttu-id="5e3fa-304">Daha fazla bilgi için bkz. [Düzen sayfası](xref:mvc/views/layout).</span><span class="sxs-lookup"><span data-stu-id="5e3fa-304">For more information, see [layout page](xref:mvc/views/layout)..</span></span>

<span data-ttu-id="5e3fa-305">[Layout](xref:mvc/views/layout#specifying-a-layout) özelliği *Pages/_viewstart. cshtml*içinde ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-305">The [Layout](xref:mvc/views/layout#specifying-a-layout) property is set in *Pages/_ViewStart.cshtml*:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]

<span data-ttu-id="5e3fa-306">Düzen *Sayfalar/paylaşılan* klasöründedir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-306">The layout is in the *Pages/Shared* folder.</span></span> <span data-ttu-id="5e3fa-307">Sayfalar, geçerli sayfayla aynı klasörden başlayarak diğer görünümleri (düzenler, şablonlar, parals) hiyerarşik olarak arar.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-307">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="5e3fa-308">*Sayfalar/paylaşılan* klasördeki bir düzen, *Sayfalar* klasörü altındaki herhangi bir Razor sayfasından kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-308">A layout in the *Pages/Shared* folder can be used from any Razor page under the *Pages* folder.</span></span>

<span data-ttu-id="5e3fa-309">Düzen dosyası *Sayfalar/paylaşılan* klasörüne gitmelidir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-309">The layout file should go in the *Pages/Shared* folder.</span></span>

<span data-ttu-id="5e3fa-310">Düzen dosyasını *Görünümler/paylaşılan* klasöre **yerleştirmenizi öneririz** .</span><span class="sxs-lookup"><span data-stu-id="5e3fa-310">We recommend you **not** put the layout file in the *Views/Shared* folder.</span></span> <span data-ttu-id="5e3fa-311">*Görünümler/paylaşılan* bir MVC görünümleri modelidir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-311">*Views/Shared* is an MVC views pattern.</span></span> <span data-ttu-id="5e3fa-312">Razor Pages, yol kurallarını değil klasör hiyerarşisine güvenmektir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-312">Razor Pages are meant to rely on folder hierarchy, not path conventions.</span></span>

<span data-ttu-id="5e3fa-313">Bir Razor sayfasından arama görüntüleme, *Sayfalar* klasörünü içerir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-313">View search from a Razor Page includes the *Pages* folder.</span></span> <span data-ttu-id="5e3fa-314">MVC denetleyicileri ve geleneksel Razor görünümleriyle kullanılan düzenler, şablonlar ve partilar *yalnızca çalışır*.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-314">The layouts, templates, and partials used with MVC controllers and conventional Razor views *just work*.</span></span>

<span data-ttu-id="5e3fa-315">*Pages/_Viewwimports. cshtml* dosyası ekleyin:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-315">Add a *Pages/_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]

<span data-ttu-id="5e3fa-316">`@namespace`, Öğreticinin ilerleyen kısımlarında açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-316">`@namespace` is explained later in the tutorial.</span></span> <span data-ttu-id="5e3fa-317">Yönergesi, [yerleşik etiket yardımcılarını](xref:mvc/views/tag-helpers/builtin-th/Index) sayfalar klasöründeki tüm sayfalara getirir. `@addTagHelper`</span><span class="sxs-lookup"><span data-stu-id="5e3fa-317">The `@addTagHelper` directive brings in the [built-in Tag Helpers](xref:mvc/views/tag-helpers/builtin-th/Index) to all the pages in the *Pages* folder.</span></span>

<a name="namespace"></a>

<span data-ttu-id="5e3fa-318">Bir sayfada ayarlanan yönerge: `@namespace`</span><span class="sxs-lookup"><span data-stu-id="5e3fa-318">The `@namespace` directive set on a page:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]

<span data-ttu-id="5e3fa-319">`@namespace` Yönergesi sayfanın ad alanını ayarlar.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-319">The `@namespace` directive sets the namespace for the page.</span></span> <span data-ttu-id="5e3fa-320">`@model` Yönergesinin ad alanını içermesi gerekmez.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-320">The `@model` directive doesn't need to include the namespace.</span></span>

<span data-ttu-id="5e3fa-321">Yönerge _viewwimports *. cshtml*içinde yer aldığında, belirtilen ad alanı, `@namespace` yönergeyi içeri aktaran sayfada oluşturulan ad alanı için ön ek sağlar. `@namespace`</span><span class="sxs-lookup"><span data-stu-id="5e3fa-321">When the `@namespace` directive is contained in *_ViewImports.cshtml*, the specified namespace supplies the prefix for the generated namespace in the Page that imports the `@namespace` directive.</span></span> <span data-ttu-id="5e3fa-322">Oluşturulan ad alanının geri kalanı (sonek bölümü), *_Viewwimports. cshtml* dosyasını ve sayfayı içeren klasörü içeren, noktayla ayrılmış göreli yoldur.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-322">The rest of the generated namespace (the suffix portion) is the dot-separated relative path between the folder containing *_ViewImports.cshtml* and the folder containing the page.</span></span>

<span data-ttu-id="5e3fa-323">Örneğin, `PageModel` *Pages/Customers/Edit. cshtml. cs* sınıfı, ad alanını açıkça ayarlar:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-323">For example, the `PageModel` class *Pages/Customers/Edit.cshtml.cs* explicitly sets the namespace:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=snippet_namespace)]

<span data-ttu-id="5e3fa-324">*Pages/_Viewwimports. cshtml* dosyası aşağıdaki ad alanını ayarlar:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-324">The *Pages/_ViewImports.cshtml* file sets the following namespace:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]

<span data-ttu-id="5e3fa-325">*Pages/Customers/Edit. cshtml* Razor sayfasının oluşturulan ad alanı `PageModel` sınıfıyla aynıdır.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-325">The generated namespace for the *Pages/Customers/Edit.cshtml* Razor Page is the same as the `PageModel` class.</span></span>

<span data-ttu-id="5e3fa-326">`@namespace`*Ayrıca geleneksel Razor görünümleriyle birlikte da geçerlidir.*</span><span class="sxs-lookup"><span data-stu-id="5e3fa-326">`@namespace` *also works with conventional Razor views.*</span></span>

<span data-ttu-id="5e3fa-327">*Pages/Create. cshtml* görünüm dosyasını göz önünde bulundurun:[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create3.cshtml?highlight=2-3)]</span><span class="sxs-lookup"><span data-stu-id="5e3fa-327">Consider the *Pages/Create.cshtml* view file: [!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create3.cshtml?highlight=2-3)]</span></span>

<span data-ttu-id="5e3fa-328">Güncelleştirilmiş *Sayfalar/Create. cshtml* görünüm dosyası *_Viewwimports. cshtml* ve önceki düzen dosyası:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-328">The updated *Pages/Create.cshtml* view file with *_ViewImports.cshtml* and the preceding layout file:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create4.cshtml?highlight=2)]

<span data-ttu-id="5e3fa-329">Yukarıdaki kodda, *_Viewwimports. cshtml* ad alanını ve etiket yardımcıları içeri aktardı.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-329">In the preceding code, the *_ViewImports.cshtml* imported the namespace and Tag Helpers.</span></span> <span data-ttu-id="5e3fa-330">Düzen dosyası JavaScript dosyalarını içeri aktardı.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-330">The layout file imported the JavaScript files.</span></span>

<span data-ttu-id="5e3fa-331">[Razor Pages Başlatıcı projesi](#rpvs17) , istemci tarafı doğrulamayı bağlayan *sayfaları/_ValidationScriptsPartial. cshtml*'yi içerir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-331">The [Razor Pages starter project](#rpvs17) contains the *Pages/_ValidationScriptsPartial.cshtml*, which hooks up client-side validation.</span></span>

<span data-ttu-id="5e3fa-332">Kısmi görünümler hakkında daha fazla bilgi için bkz <xref:mvc/views/partial>.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-332">For more information on partial views, see <xref:mvc/views/partial>.</span></span>

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a><span data-ttu-id="5e3fa-333">Sayfalar için URL oluşturma</span><span class="sxs-lookup"><span data-stu-id="5e3fa-333">URL generation for Pages</span></span>

<span data-ttu-id="5e3fa-334">Daha önce gösterilen `RedirectToPage` `Create` sayfa şunları kullanır:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-334">The `Create` page, shown previously, uses `RedirectToPage`:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml.cs?name=snippet_PageModel&highlight=15-16)]

<span data-ttu-id="5e3fa-335">Uygulama aşağıdaki dosya/klasör yapısına sahiptir:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-335">The app has the following file/folder structure:</span></span>

* <span data-ttu-id="5e3fa-336">*/Pages*</span><span class="sxs-lookup"><span data-stu-id="5e3fa-336">*/Pages*</span></span>

  * <span data-ttu-id="5e3fa-337">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="5e3fa-337">*Index.cshtml*</span></span>
  * <span data-ttu-id="5e3fa-338">*Gizlilik. cshtml*</span><span class="sxs-lookup"><span data-stu-id="5e3fa-338">*Privacy.cshtml*</span></span>
  * <span data-ttu-id="5e3fa-339">*/Customers*</span><span class="sxs-lookup"><span data-stu-id="5e3fa-339">*/Customers*</span></span>

    * <span data-ttu-id="5e3fa-340">*. Cshtml oluştur*</span><span class="sxs-lookup"><span data-stu-id="5e3fa-340">*Create.cshtml*</span></span>
    * <span data-ttu-id="5e3fa-341">*Edit.cshtml*</span><span class="sxs-lookup"><span data-stu-id="5e3fa-341">*Edit.cshtml*</span></span>
    * <span data-ttu-id="5e3fa-342">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="5e3fa-342">*Index.cshtml*</span></span>

<span data-ttu-id="5e3fa-343">*Pages/Customers/Create. cshtml* ve *Pages/Customers/Edit. cshtml* sayfaları, başarılı olduktan sonra *sayfaları/müşterileri/Index. cshtml* 'ye yeniden yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-343">The *Pages/Customers/Create.cshtml* and *Pages/Customers/Edit.cshtml* pages redirect to *Pages/Customers/Index.cshtml* after success.</span></span> <span data-ttu-id="5e3fa-344">Dize `./Index` , önceki sayfaya erişmek için URI 'nin bir parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-344">The string `./Index` is part of the URI to access the preceding page.</span></span> <span data-ttu-id="5e3fa-345">Dize `./Index` , *Sayfalar/Customers/Index. cshtml* sayfasına URI 'ler oluşturmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-345">The string `./Index` can be used to generate URIs to the *Pages/Customers/Index.cshtml* page.</span></span> <span data-ttu-id="5e3fa-346">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-346">For example:</span></span>

* `Url.Page("./Index", ...)`
* `<a asp-page="/Customers/Index">Customers Index Page</a>`
* `RedirectToPage("./Index")`

<span data-ttu-id="5e3fa-347">Dize `/Index` , *Sayfalar/Index. cshtml* sayfasına URI 'ler oluşturmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-347">The string `/Index` can be used to generate URIs to the *Pages/Index.cshtml* page.</span></span> <span data-ttu-id="5e3fa-348">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-348">For example:</span></span>

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">Home Index Page</a>`
* `RedirectToPage("/Index")`

<span data-ttu-id="5e3fa-349">Sayfa adı, kök */Pages* klasöründeki sayfanın başında `/` (örneğin, `/Index`) bir yoldur.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-349">The page name is the path to the page from the root */Pages* folder including a leading `/` (for example, `/Index`).</span></span> <span data-ttu-id="5e3fa-350">Önceki URL oluşturma örnekleri, bir URL 'YI sabit kodlamadan gelişmiş seçenekler ve işlevsel yetenekler sunar.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-350">The preceding URL generation samples offer enhanced options and functional capabilities over hard-coding a URL.</span></span> <span data-ttu-id="5e3fa-351">URL oluşturma [yönlendirme](xref:mvc/controllers/routing) kullanır ve yolun hedef yolda nasıl tanımlandığınıza göre parametreleri oluşturabilir ve kodlayabilir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-351">URL generation uses [routing](xref:mvc/controllers/routing) and can generate and encode parameters according to how the route is defined in the destination path.</span></span>

<span data-ttu-id="5e3fa-352">Sayfalar için URL oluşturma göreli adları destekler.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-352">URL generation for pages supports relative names.</span></span> <span data-ttu-id="5e3fa-353">Aşağıdaki tabloda, *sayfalarda/müşteriler/Create. cshtml*'de `RedirectToPage` farklı parametreler kullanılarak hangi dizin sayfasının seçildiği gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-353">The following table shows which Index page is selected using different `RedirectToPage` parameters in *Pages/Customers/Create.cshtml*.</span></span>

| <span data-ttu-id="5e3fa-354">RedirectToPage (x)</span><span class="sxs-lookup"><span data-stu-id="5e3fa-354">RedirectToPage(x)</span></span>| <span data-ttu-id="5e3fa-355">Sayfasında</span><span class="sxs-lookup"><span data-stu-id="5e3fa-355">Page</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="5e3fa-356">RedirectToPage ("/Index")</span><span class="sxs-lookup"><span data-stu-id="5e3fa-356">RedirectToPage("/Index")</span></span> | <span data-ttu-id="5e3fa-357">*Sayfa/dizin*</span><span class="sxs-lookup"><span data-stu-id="5e3fa-357">*Pages/Index*</span></span> |
| <span data-ttu-id="5e3fa-358">RedirectToPage ("./Index");</span><span class="sxs-lookup"><span data-stu-id="5e3fa-358">RedirectToPage("./Index");</span></span> | <span data-ttu-id="5e3fa-359">*Sayfalar/müşteriler/Dizin*</span><span class="sxs-lookup"><span data-stu-id="5e3fa-359">*Pages/Customers/Index*</span></span> |
| <span data-ttu-id="5e3fa-360">RedirectToPage (". /İndex ")</span><span class="sxs-lookup"><span data-stu-id="5e3fa-360">RedirectToPage("../Index")</span></span> | <span data-ttu-id="5e3fa-361">*Sayfa/dizin*</span><span class="sxs-lookup"><span data-stu-id="5e3fa-361">*Pages/Index*</span></span> |
| <span data-ttu-id="5e3fa-362">RedirectToPage ("Dizin")</span><span class="sxs-lookup"><span data-stu-id="5e3fa-362">RedirectToPage("Index")</span></span>  | <span data-ttu-id="5e3fa-363">*Sayfalar/müşteriler/Dizin*</span><span class="sxs-lookup"><span data-stu-id="5e3fa-363">*Pages/Customers/Index*</span></span> |

<!-- Test via ~/razor-pages/index/3.0sample/RazorPagesContacts/Pages/Customers/Details.cshtml.cs -->

<span data-ttu-id="5e3fa-364">`RedirectToPage("Index")`, `RedirectToPage("./Index")` ve`RedirectToPage("./Index")` , *göreli adlardır*.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-364">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, and `RedirectToPage("../Index")` are *relative names*.</span></span> <span data-ttu-id="5e3fa-365">Parametresi, hedef sayfanın adını hesaplamak için geçerli sayfanın yoluyla *birleştirilir.* `RedirectToPage`</span><span class="sxs-lookup"><span data-stu-id="5e3fa-365">The `RedirectToPage` parameter is *combined* with the path of the current page to compute the name of the destination page.</span></span>

<span data-ttu-id="5e3fa-366">Karmaşık bir yapıya sahip siteler oluştururken göreli ad bağlama yararlı olur.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-366">Relative name linking is useful when building sites with a complex structure.</span></span> <span data-ttu-id="5e3fa-367">Bir klasördeki sayfalar arasında bağlantı için göreli adlar kullanıldığında:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-367">When relative names are used to link between pages in a folder:</span></span>

* <span data-ttu-id="5e3fa-368">Bir klasörü yeniden adlandırmak, göreli bağlantıları bozmaz.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-368">Renaming a folder doesn't break the relative links.</span></span>
* <span data-ttu-id="5e3fa-369">Klasör adını içermediği için bağlantılar kopuk değildir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-369">Links are not broken because they don't include the folder name.</span></span>

<span data-ttu-id="5e3fa-370">Farklı bir [alandaki](xref:mvc/controllers/areas)bir sayfaya yeniden yönlendirmek için alanını belirtin:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-370">To redirect to a page in a different [Area](xref:mvc/controllers/areas), specify the area:</span></span>

```csharp
RedirectToPage("/Index", new { area = "Services" });
```

<span data-ttu-id="5e3fa-371">Daha fazla bilgi için bkz. <xref:mvc/controllers/areas> ve <xref:razor-pages/razor-pages-conventions>.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-371">For more information, see <xref:mvc/controllers/areas> and <xref:razor-pages/razor-pages-conventions>.</span></span>

## <a name="viewdata-attribute"></a><span data-ttu-id="5e3fa-372">ViewData özniteliği</span><span class="sxs-lookup"><span data-stu-id="5e3fa-372">ViewData attribute</span></span>

<span data-ttu-id="5e3fa-373">Veri, ile <xref:Microsoft.AspNetCore.Mvc.ViewDataAttribute>bir sayfaya geçirilebilir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-373">Data can be passed to a page with <xref:Microsoft.AspNetCore.Mvc.ViewDataAttribute>.</span></span> <span data-ttu-id="5e3fa-374">[ViewData] özniteliğiyle birlikte bulunan özellikler, <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.ViewDataDictionary>değerlerinin depolandığı ve öğesinden yüklenmiş olması.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-374">Properties with the [ViewData] attribute have their values stored and loaded from the <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.ViewDataDictionary>.</span></span>

<span data-ttu-id="5e3fa-375">Aşağıdaki örnekte, `AboutModel` `[ViewData]` özniteliği `Title` özelliğine uygular:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-375">In the following example, the `AboutModel` applies the `[ViewData]` attribute to the `Title` property:</span></span>

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

<span data-ttu-id="5e3fa-376">Hakkında sayfasında, `Title` özelliğe model özelliği olarak erişin:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-376">In the About page, access the `Title` property as a model property:</span></span>

```cshtml
<h1>@Model.Title</h1>
```

<span data-ttu-id="5e3fa-377">Mizanpajda, başlık ViewData sözlüğünden okundu:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-377">In the layout, the title is read from the ViewData dictionary:</span></span>

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"] - WebApplication</title>
    ...
```

## <a name="tempdata"></a><span data-ttu-id="5e3fa-378">TempData</span><span class="sxs-lookup"><span data-stu-id="5e3fa-378">TempData</span></span>

<span data-ttu-id="5e3fa-379">ASP.NET Core, <xref:Microsoft.AspNetCore.Mvc.Controller.TempData>öğesini kullanıma sunar.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-379">ASP.NET Core exposes the <xref:Microsoft.AspNetCore.Mvc.Controller.TempData>.</span></span> <span data-ttu-id="5e3fa-380">Bu özellik, okunana kadar verileri depolar.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-380">This property stores data until it's read.</span></span> <span data-ttu-id="5e3fa-381"><xref:Microsoft.AspNetCore.Mvc.ViewFeatures.TempDataDictionary.Keep*> Ve<xref:Microsoft.AspNetCore.Mvc.ViewFeatures.TempDataDictionary.Peek*> yöntemleri silmeden verileri incelemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-381">The <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.TempDataDictionary.Keep*> and <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.TempDataDictionary.Peek*> methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="5e3fa-382">`TempData`, bir tek istekten daha fazla veri gerektiğinde yeniden yönlendirme için yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-382">`TempData` is useful for redirection, when data is needed for more than a single request.</span></span>

<span data-ttu-id="5e3fa-383">Aşağıdaki kod, şunu `Message` kullanarak `TempData`değerini ayarlar:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-383">The following code sets the value of `Message` using `TempData`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,25&name=snippet_Temp)]

<span data-ttu-id="5e3fa-384">*Pages/Customers/Index. cshtml* dosyasında aşağıdaki biçimlendirme, `Message` using `TempData`değerini gösterir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-384">The following markup in the *Pages/Customers/Index.cshtml* file displays the value of `Message` using `TempData`.</span></span>

```cshtml
<h3>Msg: @Model.Message</h3>
```

<span data-ttu-id="5e3fa-385">*Pages/Customers/Index. cshtml. cs* sayfa modeli, `[TempData]` `Message` özelliğine özniteliğini uygular.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-385">The *Pages/Customers/Index.cshtml.cs* page model applies the `[TempData]` attribute to the `Message` property.</span></span>

```cs
[TempData]
public string Message { get; set; }
```

<span data-ttu-id="5e3fa-386">Daha fazla bilgi için bkz. [TempData](xref:fundamentals/app-state#tempdata) .</span><span class="sxs-lookup"><span data-stu-id="5e3fa-386">For more information, see [TempData](xref:fundamentals/app-state#tempdata) .</span></span>

<a name="mhpp"></a>

## <a name="multiple-handlers-per-page"></a><span data-ttu-id="5e3fa-387">Sayfa başına birden çok işleyici</span><span class="sxs-lookup"><span data-stu-id="5e3fa-387">Multiple handlers per page</span></span>

<span data-ttu-id="5e3fa-388">Aşağıdaki sayfa, `asp-page-handler` etiket Yardımcısını kullanarak iki işleyici için biçimlendirme oluşturur:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-388">The following page generates markup for two handlers using the `asp-page-handler` Tag Helper:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

<span data-ttu-id="5e3fa-389">Yukarıdaki örnekteki formda, her biri farklı bir URL 'ye göndermek `FormActionTagHelper` için kullanan iki gönderme düğmesi vardır.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-389">The form in the preceding example has two submit buttons, each using the `FormActionTagHelper` to submit to a different URL.</span></span> <span data-ttu-id="5e3fa-390">Özniteliği, için `asp-page`bir yardımcı ' dir. `asp-page-handler`</span><span class="sxs-lookup"><span data-stu-id="5e3fa-390">The `asp-page-handler` attribute is a companion to `asp-page`.</span></span> <span data-ttu-id="5e3fa-391">`asp-page-handler`bir sayfa tarafından tanımlanan her bir işleyici yöntemini gönderen URL 'Ler oluşturur.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-391">`asp-page-handler` generates URLs that submit to each of the handler methods defined by a page.</span></span> <span data-ttu-id="5e3fa-392">`asp-page`örnek geçerli sayfaya bağlandığından belirtilmedi.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-392">`asp-page` isn't specified because the sample is linking to the current page.</span></span>

<span data-ttu-id="5e3fa-393">Sayfa modeli:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-393">The page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]

<span data-ttu-id="5e3fa-394">Yukarıdaki kod, *adlandırılmış işleyici yöntemlerini*kullanır.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-394">The preceding code uses *named handler methods*.</span></span> <span data-ttu-id="5e3fa-395">Adlandırılmış işleyici yöntemleri, `On<HTTP Verb>` ve öncesinde `Async` (varsa) ad içindeki metin alınarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-395">Named handler methods are created by taking the text in the name after `On<HTTP Verb>` and before `Async` (if present).</span></span> <span data-ttu-id="5e3fa-396">Yukarıdaki örnekte, Page metotları OnPost**Joinlist**Async ve onpost**Joinlıstuc**Async ' dir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-396">In the preceding example, the page methods are OnPost**JoinList**Async and OnPost**JoinListUC**Async.</span></span> <span data-ttu-id="5e3fa-397">*Onpost* ile *zaman uyumsuz* olarak kaldırıldığında, işleyici adları ve `JoinList` `JoinListUC`' dir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-397">With *OnPost* and *Async* removed, the handler names are `JoinList` and `JoinListUC`.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?range=12-13)]

<span data-ttu-id="5e3fa-398">Önceki kodu kullanarak, ' a gönderen `OnPostJoinListAsync` `https://localhost:5001/Customers/CreateFATH?handler=JoinList`URL yolu.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-398">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `https://localhost:5001/Customers/CreateFATH?handler=JoinList`.</span></span> <span data-ttu-id="5e3fa-399">' A gönderen `OnPostJoinListUCAsync` `https://localhost:5001/Customers/CreateFATH?handler=JoinListUC`URL yolu.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-399">The URL path that submits to `OnPostJoinListUCAsync` is `https://localhost:5001/Customers/CreateFATH?handler=JoinListUC`.</span></span>

## <a name="custom-routes"></a><span data-ttu-id="5e3fa-400">Özel yollar</span><span class="sxs-lookup"><span data-stu-id="5e3fa-400">Custom routes</span></span>

<span data-ttu-id="5e3fa-401">`@page` İçin yönergesini kullanın:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-401">Use the `@page` directive to:</span></span>

* <span data-ttu-id="5e3fa-402">Sayfaya özel bir yol belirtin.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-402">Specify a custom route to a page.</span></span> <span data-ttu-id="5e3fa-403">Örneğin, hakkında sayfasına olan yol ile `/Some/Other/Path` `@page "/Some/Other/Path"`öğesine ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-403">For example, the route to the About page can be set to `/Some/Other/Path` with `@page "/Some/Other/Path"`.</span></span>
* <span data-ttu-id="5e3fa-404">Kesimleri bir sayfanın varsayılan yoluna ekleyin.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-404">Append segments to a page's default route.</span></span> <span data-ttu-id="5e3fa-405">Örneğin, bir "öğe" segmenti sayfanın varsayılan rotasına `@page "item"`eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-405">For example, an "item" segment can be added to a page's default route with `@page "item"`.</span></span>
* <span data-ttu-id="5e3fa-406">Bir sayfanın varsayılan yoluna parametreleri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-406">Append parameters to a page's default route.</span></span> <span data-ttu-id="5e3fa-407">Örneğin, bir ID parametresi `id`, içeren `@page "{id}"`bir sayfa için gerekli olabilir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-407">For example, an ID parameter, `id`, can be required for a page with `@page "{id}"`.</span></span>

<span data-ttu-id="5e3fa-408">Yolun başındaki bir tilde (`~`) tarafından belirlenen kök göreli bir yol desteklenir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-408">A root-relative path designated by a tilde (`~`) at the beginning of the path is supported.</span></span> <span data-ttu-id="5e3fa-409">Örneğin, `@page "~/Some/Other/Path"` ile `@page "/Some/Other/Path"`aynıdır.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-409">For example, `@page "~/Some/Other/Path"` is the same as `@page "/Some/Other/Path"`.</span></span>

<span data-ttu-id="5e3fa-410">Yol şablonunu `?handler=JoinList` `/JoinList` belirterek,URL'dekisorgudizesinibirrota`@page "{handler?}"`segmentine dönüştürebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-410">You can change the query string `?handler=JoinList` in the URL to a route segment `/JoinList` by specifying the route template `@page "{handler?}"`.</span></span>

<span data-ttu-id="5e3fa-411">URL 'de sorgu dizesini `?handler=JoinList` beğenmezseniz, yolu URL 'nin yol bölümüne koymak için yolu değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-411">If you don't like the query string `?handler=JoinList` in the URL, you can change the route to put the handler name in the path portion of the URL.</span></span> <span data-ttu-id="5e3fa-412">`@page` Yönergeden sonra çift tırnak içine alınmış bir rota şablonu ekleyerek yolu özelleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-412">You can customize the route by adding a route template enclosed in double quotes after the `@page` directive.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]

<span data-ttu-id="5e3fa-413">Önceki kodu kullanarak, ' a gönderen `OnPostJoinListAsync` `https://localhost:5001/Customers/CreateFATH/JoinList`URL yolu.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-413">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `https://localhost:5001/Customers/CreateFATH/JoinList`.</span></span> <span data-ttu-id="5e3fa-414">' A gönderen `OnPostJoinListUCAsync` `https://localhost:5001/Customers/CreateFATH/JoinListUC`URL yolu.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-414">The URL path that submits to `OnPostJoinListUCAsync` is `https://localhost:5001/Customers/CreateFATH/JoinListUC`.</span></span>

<span data-ttu-id="5e3fa-415">`?` Aşağıda`handler` yol parametresinin isteğe bağlı olduğu anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-415">The `?` following `handler` means the route parameter is optional.</span></span>

## <a name="advanced-configuration-and-settings"></a><span data-ttu-id="5e3fa-416">Gelişmiş yapılandırma ve ayarlar</span><span class="sxs-lookup"><span data-stu-id="5e3fa-416">Advanced configuration and settings</span></span>

<span data-ttu-id="5e3fa-417">Aşağıdaki bölümlerdeki yapılandırma ve ayarlar çoğu uygulama için gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-417">The configuration and settings in following sections is not required by most apps.</span></span>

<span data-ttu-id="5e3fa-418">Gelişmiş seçenekleri yapılandırmak için genişletme yöntemini <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>kullanın:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-418">To configure advanced options, use the extension method <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/StartupRPoptions.cs?name=snippet)]

<span data-ttu-id="5e3fa-419">Sayfalar için kök dizini ayarlamak üzereöğesinikullanınveyasayfalariçinuygulamamodelikurallarıekleyin.<xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions></span><span class="sxs-lookup"><span data-stu-id="5e3fa-419">Use the <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions> to set the root directory for pages, or add application model conventions for pages.</span></span> <span data-ttu-id="5e3fa-420">Kurallar hakkında daha fazla bilgi için bkz. [Razor Pages yetkilendirme kuralları](xref:security/authorization/razor-pages-authorization).</span><span class="sxs-lookup"><span data-stu-id="5e3fa-420">For more information on conventions, see [Razor Pages authorization conventions](xref:security/authorization/razor-pages-authorization).</span></span>

<span data-ttu-id="5e3fa-421">Görünümleri önceden derlemek için bkz. [Razor görünüm derlemesi](xref:mvc/views/view-compilation) .</span><span class="sxs-lookup"><span data-stu-id="5e3fa-421">To precompile views, see [Razor view compilation](xref:mvc/views/view-compilation) .</span></span>

### <a name="specify-that-razor-pages-are-at-the-content-root"></a><span data-ttu-id="5e3fa-422">Razor Pages içerik kökünde olduğunu belirtin</span><span class="sxs-lookup"><span data-stu-id="5e3fa-422">Specify that Razor Pages are at the content root</span></span>

<span data-ttu-id="5e3fa-423">Varsayılan olarak, Razor Pages */Pages* dizininde kök olarak depolanır.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-423">By default, Razor Pages are rooted in the */Pages* directory.</span></span> <span data-ttu-id="5e3fa-424">Razor Pages <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.WithRazorPagesAtContentRoot*> , uygulamanın içerik kökünde (<xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootPath>) olduğunu belirtmek için ekleyin:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-424">Add <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.WithRazorPagesAtContentRoot*> to specify that your Razor Pages are at the content root (<xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootPath>) of the app:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/StartupWithRazorPagesAtContentRoot.cs?name=snippet)]

### <a name="specify-that-razor-pages-are-at-a-custom-root-directory"></a><span data-ttu-id="5e3fa-425">Razor Pages özel kök dizinde olduğunu belirtin</span><span class="sxs-lookup"><span data-stu-id="5e3fa-425">Specify that Razor Pages are at a custom root directory</span></span>

<span data-ttu-id="5e3fa-426">Razor Pages <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcCoreBuilderExtensions.WithRazorPagesRoot*> , uygulamadaki özel bir kök dizinde olduğunu belirtmek için ekleyin (göreli bir yol sağlayın):</span><span class="sxs-lookup"><span data-stu-id="5e3fa-426">Add <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcCoreBuilderExtensions.WithRazorPagesRoot*> to specify that Razor Pages are at a custom root directory in the app (provide a relative path):</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/StartupWithRazorPagesRoot.cs?name=snippet)]

## <a name="additional-resources"></a><span data-ttu-id="5e3fa-427">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="5e3fa-427">Additional resources</span></span>

* <span data-ttu-id="5e3fa-428">Bu giriş hakkında bilgi için bkz. [Razor Pages kullanmaya başlama](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="5e3fa-428">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start), which builds on this introduction.</span></span>
* <span data-ttu-id="5e3fa-429">[Örnek kodu indirin veya görüntüleyin](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/index/3.0sample).</span><span class="sxs-lookup"><span data-stu-id="5e3fa-429">[Download or view sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/index/3.0sample).</span></span>
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

<span data-ttu-id="5e3fa-430">By [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Ryan şimdi ak](https://github.com/rynowak)</span><span class="sxs-lookup"><span data-stu-id="5e3fa-430">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Ryan Nowak](https://github.com/rynowak)</span></span>

<span data-ttu-id="5e3fa-431">Razor Pages, kod odaklı senaryoları daha kolay ve daha üretken hale getiren ASP.NET Core MVC 'nin yeni bir yönüdür.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-431">Razor Pages is a new aspect of ASP.NET Core MVC that makes coding page-focused scenarios easier and more productive.</span></span>

<span data-ttu-id="5e3fa-432">Model-View-Controller yaklaşımını kullanan bir öğretici arıyorsanız, bkz. [ASP.NET Core MVC ile çalışmaya başlama](xref:tutorials/first-mvc-app/start-mvc).</span><span class="sxs-lookup"><span data-stu-id="5e3fa-432">If you're looking for a tutorial that uses the Model-View-Controller approach, see [Get started with ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span></span>

<span data-ttu-id="5e3fa-433">Bu belge Razor Pages bir giriş sağlar.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-433">This document provides an introduction to Razor Pages.</span></span> <span data-ttu-id="5e3fa-434">Adım adım öğretici değildir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-434">It's not a step by step tutorial.</span></span> <span data-ttu-id="5e3fa-435">Bölümlerden bazılarını çok gelişmiş bir şekilde bulursanız, bkz. [Razor Pages kullanmaya başlama](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="5e3fa-435">If you find some of the sections too advanced, see [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span></span> <span data-ttu-id="5e3fa-436">ASP.NET Core genel bir bakış için bkz. [ASP.NET Core giriş](xref:index).</span><span class="sxs-lookup"><span data-stu-id="5e3fa-436">For an overview of ASP.NET Core, see the [Introduction to ASP.NET Core](xref:index).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5e3fa-437">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="5e3fa-437">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5e3fa-438">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5e3fa-438">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5e3fa-439">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5e3fa-439">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="5e3fa-440">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5e3fa-440">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

<a name="rpvs17"></a>

## <a name="create-a-razor-pages-project"></a><span data-ttu-id="5e3fa-441">Razor Pages projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="5e3fa-441">Create a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5e3fa-442">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5e3fa-442">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="5e3fa-443">Razor Pages projesi oluşturma hakkında ayrıntılı yönergeler için bkz. [Razor Pages kullanmaya başlama](xref:tutorials/razor-pages/razor-pages-start) .</span><span class="sxs-lookup"><span data-stu-id="5e3fa-443">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) for detailed instructions on how to create a Razor Pages project.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="5e3fa-444">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5e3fa-444">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="5e3fa-445">Komut `dotnet new webapp` satırından çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-445">Run `dotnet new webapp` from the command line.</span></span>

<span data-ttu-id="5e3fa-446">Oluşturulan *. csproj* dosyasını Mac için Visual Studio açın.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-446">Open the generated *.csproj* file from Visual Studio for Mac.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5e3fa-447">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5e3fa-447">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="5e3fa-448">Komut `dotnet new webapp` satırından çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-448">Run `dotnet new webapp` from the command line.</span></span>

---

## <a name="razor-pages"></a><span data-ttu-id="5e3fa-449">Razor Pages</span><span class="sxs-lookup"><span data-stu-id="5e3fa-449">Razor Pages</span></span>

<span data-ttu-id="5e3fa-450">Razor Pages, *Startup.cs*'de etkinleştirilmiştir:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-450">Razor Pages is enabled in *Startup.cs*:</span></span>

[!code-cs[](index/sample/RazorPagesIntro/Startup.cs?name=snippet_Startup)]

<span data-ttu-id="5e3fa-451">Temel bir sayfa düşünün:<a name="OnGet"></a></span><span class="sxs-lookup"><span data-stu-id="5e3fa-451">Consider a basic page: <a name="OnGet"></a></span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index.cshtml)]

<span data-ttu-id="5e3fa-452">Yukarıdaki kod, denetleyiciler ve görünümlerle ASP.NET Core bir uygulamada kullanılan [Razor görünüm dosyası](xref:tutorials/first-mvc-app/adding-view) gibi bir çok şey arar.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-452">The preceding code looks a lot like a [Razor view file](xref:tutorials/first-mvc-app/adding-view) used in an ASP.NET Core app with controllers and views.</span></span> <span data-ttu-id="5e3fa-453">Bu, `@page` farklı kılan yönergedir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-453">What makes it different is the `@page` directive.</span></span> <span data-ttu-id="5e3fa-454">`@page`dosyayı bir MVC eylemine dönüştürür. Bu, bir denetleyiciden geçmeden istekleri doğrudan işlediği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-454">`@page` makes the file into an MVC action - which means that it handles requests directly, without going through a controller.</span></span> <span data-ttu-id="5e3fa-455">`@page`sayfada ilk Razor yönergesi olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-455">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="5e3fa-456">`@page`diğer Razor yapıları davranışını etkiler.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-456">`@page` affects the behavior of other Razor constructs.</span></span>

<span data-ttu-id="5e3fa-457">Bir `PageModel` sınıf kullanan benzer bir sayfa aşağıdaki iki dosyada gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-457">A similar page, using a `PageModel` class, is shown in the following two files.</span></span> <span data-ttu-id="5e3fa-458">*Pages/Index2. cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-458">The *Pages/Index2.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index2.cshtml)]

<span data-ttu-id="5e3fa-459">*Pages/Index2. cshtml. cs* sayfa modeli:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-459">The *Pages/Index2.cshtml.cs* page model:</span></span>

[!code-cs[](index/sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]

<span data-ttu-id="5e3fa-460">Kurala göre, `PageModel` sınıf dosyası *. cs* eklenmiş Razor sayfası dosyasıyla aynı ada sahiptir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-460">By convention, the `PageModel` class file has the same name as the Razor Page file with *.cs* appended.</span></span> <span data-ttu-id="5e3fa-461">Örneğin, önceki Razor sayfası *Pages/Index2. cshtml*' dir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-461">For example, the previous Razor Page is *Pages/Index2.cshtml*.</span></span> <span data-ttu-id="5e3fa-462">`PageModel` Sınıfını içeren dosya *sayfa/Index2. cshtml. cs*olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-462">The file containing the `PageModel` class is named *Pages/Index2.cshtml.cs*.</span></span>

<span data-ttu-id="5e3fa-463">URL yollarının sayfalara olan ilişkilendirmeleri, sayfanın dosya sistemindeki konumuna göre belirlenir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-463">The associations of URL paths to pages are determined by the page's location in the file system.</span></span> <span data-ttu-id="5e3fa-464">Aşağıdaki tabloda bir Razor sayfa yolu ve eşleşen URL gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-464">The following table shows a Razor Page path and the matching URL:</span></span>

| <span data-ttu-id="5e3fa-465">Dosya adı ve yolu</span><span class="sxs-lookup"><span data-stu-id="5e3fa-465">File name and path</span></span>               | <span data-ttu-id="5e3fa-466">eşleşen URL</span><span class="sxs-lookup"><span data-stu-id="5e3fa-466">matching URL</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="5e3fa-467">*/Pages/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="5e3fa-467">*/Pages/Index.cshtml*</span></span> | <span data-ttu-id="5e3fa-468">`/` veya `/Index`</span><span class="sxs-lookup"><span data-stu-id="5e3fa-468">`/` or `/Index`</span></span> |
| <span data-ttu-id="5e3fa-469">*/Pages/Contact.exe*</span><span class="sxs-lookup"><span data-stu-id="5e3fa-469">*/Pages/Contact.cshtml*</span></span> | `/Contact` |
| <span data-ttu-id="5e3fa-470">*/Pages/Store/Contact.exe*</span><span class="sxs-lookup"><span data-stu-id="5e3fa-470">*/Pages/Store/Contact.cshtml*</span></span> | `/Store/Contact` |
| <span data-ttu-id="5e3fa-471">*/Pages/Store/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="5e3fa-471">*/Pages/Store/Index.cshtml*</span></span> | <span data-ttu-id="5e3fa-472">`/Store` veya `/Store/Index`</span><span class="sxs-lookup"><span data-stu-id="5e3fa-472">`/Store` or `/Store/Index`</span></span> |

<span data-ttu-id="5e3fa-473">Notlar:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-473">Notes:</span></span>

* <span data-ttu-id="5e3fa-474">Çalışma zamanı, *Sayfalar* klasöründeki Razor Pages dosyaları varsayılan olarak arar.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-474">The runtime looks for Razor Pages files in the *Pages* folder by default.</span></span>
* <span data-ttu-id="5e3fa-475">`Index`, URL bir sayfa içermiyorsa varsayılan sayfasıdır.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-475">`Index` is the default page when a URL doesn't include a page.</span></span>

## <a name="write-a-basic-form"></a><span data-ttu-id="5e3fa-476">Temel form yazma</span><span class="sxs-lookup"><span data-stu-id="5e3fa-476">Write a basic form</span></span>

<span data-ttu-id="5e3fa-477">Razor Pages, Web tarayıcıları ile kullanılan ortak desenleri bir uygulama oluştururken kolayca uygulanması için tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-477">Razor Pages is designed to make common patterns used with web browsers easy to implement when building an app.</span></span> <span data-ttu-id="5e3fa-478">[Model bağlama](xref:mvc/models/model-binding), [ETIKET yardımcıları](xref:mvc/views/tag-helpers/intro)ve HTML Yardımcıları hepsi, Razor sayfası sınıfında tanımlanan özelliklerle *çalışır* .</span><span class="sxs-lookup"><span data-stu-id="5e3fa-478">[Model binding](xref:mvc/models/model-binding), [Tag Helpers](xref:mvc/views/tag-helpers/intro), and HTML helpers all *just work* with the properties defined in a Razor Page class.</span></span> <span data-ttu-id="5e3fa-479">`Contact` Model için temel bir "bize başvurun" formu uygulayan bir sayfa düşünün:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-479">Consider a page that implements a basic "contact us" form for the `Contact` model:</span></span>

<span data-ttu-id="5e3fa-480">Bu belgedeki `DbContext` örnekler için, [Startup.cs](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) dosyasında başlatılır.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-480">For the samples in this document, the `DbContext` is initialized in the [Startup.cs](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) file.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Startup.cs?highlight=15-16)]

<span data-ttu-id="5e3fa-481">Veri modeli:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-481">The data model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Data/Customer.cs)]

<span data-ttu-id="5e3fa-482">DB bağlamı:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-482">The db context:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Data/AppDbContext.cs)]

<span data-ttu-id="5e3fa-483">*Pages/Create. cshtml* görünüm dosyası:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-483">The *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml)]

<span data-ttu-id="5e3fa-484">*Pages/Create. cshtml. cs* sayfa modeli:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-484">The *Pages/Create.cshtml.cs* page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_ALL)]

<span data-ttu-id="5e3fa-485">Kuralına göre, `PageModel` sınıfı çağrılır `<PageName>Model` ve sayfayla aynı ad alanında bulunur.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-485">By convention, the `PageModel` class is called `<PageName>Model` and is in the same namespace as the page.</span></span>

<span data-ttu-id="5e3fa-486">`PageModel` Sınıfı, bir sayfanın mantığının sunumuna ayrılmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-486">The `PageModel` class allows separation of the logic of a page from its presentation.</span></span> <span data-ttu-id="5e3fa-487">Sayfaya gönderilen istekler için sayfa işleyicilerini ve sayfayı işlemek için kullanılan verileri tanımlar.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-487">It defines page handlers for requests sent to the page and the data used to render the page.</span></span> <span data-ttu-id="5e3fa-488">Bu ayrım şunları sağlar:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-488">This separation allows:</span></span>

* <span data-ttu-id="5e3fa-489">[Bağımlılık ekleme](xref:fundamentals/dependency-injection)yoluyla sayfa bağımlılıklarını yönetme.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-489">Managing of page dependencies through [dependency injection](xref:fundamentals/dependency-injection).</span></span>
* <span data-ttu-id="5e3fa-490">Sayfaların [birim testi](xref:test/razor-pages-tests) .</span><span class="sxs-lookup"><span data-stu-id="5e3fa-490">[Unit testing](xref:test/razor-pages-tests) the pages.</span></span>

<span data-ttu-id="5e3fa-491">Sayfada, istekler üzerinde `OnPostAsync` `POST` çalışan bir *işleyici yöntemi*vardır (bir Kullanıcı formu gönderdiğinde).</span><span class="sxs-lookup"><span data-stu-id="5e3fa-491">The page has an `OnPostAsync` *handler method*, which runs on `POST` requests (when a user posts the form).</span></span> <span data-ttu-id="5e3fa-492">Herhangi bir HTTP fiili için işleyici yöntemleri ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-492">You can add handler methods for any HTTP verb.</span></span> <span data-ttu-id="5e3fa-493">En yaygın işleyiciler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-493">The most common handlers are:</span></span>

* <span data-ttu-id="5e3fa-494">`OnGet`sayfa için gereken durumu başlatmak için.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-494">`OnGet` to initialize state needed for the page.</span></span> <span data-ttu-id="5e3fa-495">[OnGet](#OnGet) örneği.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-495">[OnGet](#OnGet) sample.</span></span>
* <span data-ttu-id="5e3fa-496">`OnPost`form gönderilerini işlemek için.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-496">`OnPost` to handle form submissions.</span></span>

<span data-ttu-id="5e3fa-497">`Async` Adlandırma son eki isteğe bağlıdır, ancak genellikle zaman uyumsuz işlevler için kural tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-497">The `Async` naming suffix is optional but is often used by convention for asynchronous functions.</span></span> <span data-ttu-id="5e3fa-498">Yukarıdaki kod Razor Pages için tipik bir davranıştır.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-498">The preceding code is typical for Razor Pages.</span></span>

<span data-ttu-id="5e3fa-499">Denetleyicileri ve görünümleri kullanarak ASP.NET uygulamaları hakkında bilginiz varsa:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-499">If you're familiar with ASP.NET apps using controllers and views:</span></span>

* <span data-ttu-id="5e3fa-500">Yukarıdaki `OnPostAsync` örnekteki kod, tipik denetleyici koduna benzer şekilde görünür.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-500">The `OnPostAsync` code in the preceding example looks similar to typical controller code.</span></span>
* <span data-ttu-id="5e3fa-501">[Model bağlama](xref:mvc/models/model-binding), [doğrulama](xref:mvc/models/validation), [doğrulama](xref:mvc/models/validation)ve eylem sonuçları gibi mvc temel elemanlarının çoğu paylaşılır.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-501">Most of the MVC primitives like [model binding](xref:mvc/models/model-binding), [validation](xref:mvc/models/validation), [Validation](xref:mvc/models/validation),  and action results are shared.</span></span>

<span data-ttu-id="5e3fa-502">Önceki `OnPostAsync` Yöntem:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-502">The previous `OnPostAsync` method:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="5e3fa-503">Temel akışı `OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-503">The basic flow of `OnPostAsync`:</span></span>

<span data-ttu-id="5e3fa-504">Doğrulama hatalarını kontrol edin.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-504">Check for validation errors.</span></span>

* <span data-ttu-id="5e3fa-505">Hata yoksa, verileri kaydedin ve yeniden yönlendirin.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-505">If there are no errors, save the data and redirect.</span></span>
* <span data-ttu-id="5e3fa-506">Hatalar varsa, doğrulama iletileriyle sayfayı yeniden görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-506">If there are errors, show the page again with validation messages.</span></span> <span data-ttu-id="5e3fa-507">İstemci tarafı doğrulaması geleneksel ASP.NET Core MVC uygulamalarıyla aynıdır.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-507">Client-side validation is identical to traditional ASP.NET Core MVC applications.</span></span> <span data-ttu-id="5e3fa-508">Çoğu durumda, istemci üzerinde doğrulama hataları algılanır ve sunucuya hiçbir zaman gönderilmez.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-508">In many cases, validation errors would be detected on the client, and never submitted to the server.</span></span>

<span data-ttu-id="5e3fa-509">Veriler başarıyla girildiğinde, `OnPostAsync` işleyici yöntemi bir örneğini döndürmek `RedirectToPageResult`için `RedirectToPage` yardımcı yöntemini çağırır.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-509">When the data is entered successfully, the `OnPostAsync` handler method calls the `RedirectToPage` helper method to return an instance of `RedirectToPageResult`.</span></span> <span data-ttu-id="5e3fa-510">`RedirectToPage`, `RedirectToAction` veya`RedirectToRoute`' a benzer ancak sayfalara özelleştirilmiş yeni bir eylem sonucudur.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-510">`RedirectToPage` is a new action result, similar to `RedirectToAction` or `RedirectToRoute`, but customized for pages.</span></span> <span data-ttu-id="5e3fa-511">Yukarıdaki örnekte, kök dizin sayfasına (`/Index`) yeniden yönlendiriliyor.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-511">In the preceding sample, it redirects to the root Index page (`/Index`).</span></span> <span data-ttu-id="5e3fa-512">`RedirectToPage`, [Sayfalar Için URL oluşturma](#url_gen) bölümünde ayrıntılı olarak açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-512">`RedirectToPage` is detailed in the [URL generation for Pages](#url_gen) section.</span></span>

<span data-ttu-id="5e3fa-513">Gönderilen formda doğrulama hataları olduğunda (sunucuya geçirilen),`OnPostAsync` işleyici yöntemi `Page` yardımcı yöntemini çağırır.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-513">When the submitted form has validation errors (that are passed to the server), the`OnPostAsync` handler method calls the `Page` helper method.</span></span> <span data-ttu-id="5e3fa-514">`Page`bir örneğini `PageResult`döndürür.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-514">`Page` returns an instance of `PageResult`.</span></span> <span data-ttu-id="5e3fa-515">Döndürme `Page` , denetleyicilerde eylemlerin nasıl dönüşlerine `View`benzer.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-515">Returning `Page` is similar to how actions in controllers return `View`.</span></span> <span data-ttu-id="5e3fa-516">`PageResult`, bir işleyici yöntemi için varsayılan dönüş türüdür.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-516">`PageResult` is the default return type for a handler method.</span></span> <span data-ttu-id="5e3fa-517">Döndüren `void` bir işleyici yöntemi sayfayı işler.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-517">A handler method that returns `void` renders the page.</span></span>

<span data-ttu-id="5e3fa-518">Özelliği model bağlamayı kabul etmek için özniteliğini kullanır `[BindProperty]`. `Customer`</span><span class="sxs-lookup"><span data-stu-id="5e3fa-518">The `Customer` property uses `[BindProperty]` attribute to opt in to model binding.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_PageModel&highlight=10-11)]

<span data-ttu-id="5e3fa-519">Razor Pages, varsayılan olarak yalnızca`GET` fiiller olmayan özellikleri bağlayın.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-519">Razor Pages, by default, bind properties only with non-`GET` verbs.</span></span> <span data-ttu-id="5e3fa-520">Özelliklere bağlama, yazmanız gerektiğini kodun miktarını azaltabilir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-520">Binding to properties can reduce the amount of code you have to write.</span></span> <span data-ttu-id="5e3fa-521">Bağlama, form alanlarını işlemek için aynı özelliği kullanarak kodu azaltır (`<input asp-for="Customer.Name">`) ve girişi kabul eder.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-521">Binding reduces code by using the same property to render form fields (`<input asp-for="Customer.Name">`) and accept the input.</span></span>

[!INCLUDE[](~/includes/bind-get.md)]

<span data-ttu-id="5e3fa-522">Giriş sayfası (*Index. cshtml*):</span><span class="sxs-lookup"><span data-stu-id="5e3fa-522">The home page (*Index.cshtml*):</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml)]

<span data-ttu-id="5e3fa-523">İlişkili `PageModel` Sınıf (*Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="5e3fa-523">The associated `PageModel` class (*Index.cshtml.cs*):</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs)]

<span data-ttu-id="5e3fa-524">*Index. cshtml* dosyası her kişi için bir düzenleme bağlantısı oluşturmak üzere aşağıdaki biçimlendirmeyi içerir:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-524">The *Index.cshtml* file contains the following markup to create an edit link for each contact:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=21)]

<span data-ttu-id="5e3fa-525">[Tutturucu etiketi Yardımcısı,](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) düzenleme sayfasına bir bağlantı oluşturmak için özniteliğinikullandı.`asp-route-{value}` `<a asp-page="./Edit" asp-route-id="@contact.Id">Edit</a>`</span><span class="sxs-lookup"><span data-stu-id="5e3fa-525">The `<a asp-page="./Edit" asp-route-id="@contact.Id">Edit</a>` [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) used the `asp-route-{value}` attribute to generate a link to the Edit page.</span></span> <span data-ttu-id="5e3fa-526">Bağlantı, iletişim KIMLIĞINE sahip rota verileri içerir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-526">The link contains route data with the contact ID.</span></span> <span data-ttu-id="5e3fa-527">Örneğin, uygulamasında yönetilen Hyper-V konakları olarak eklemek için aşağıdaki yordamı kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-527">For example, `https://localhost:5001/Edit/1`.</span></span> <span data-ttu-id="5e3fa-528">[Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro), Razor dosyalarında HTML öğelerinin oluşturulmasına ve işlenmesine sunucu tarafı kodun katılmasını etkinleştir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-528">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span> <span data-ttu-id="5e3fa-529">Etiket Yardımcıları tarafından etkinleştirilir`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers`</span><span class="sxs-lookup"><span data-stu-id="5e3fa-529">Tag Helpers are enabled by `@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers`</span></span>

<span data-ttu-id="5e3fa-530">*Pages/Edit. cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-530">The *Pages/Edit.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Edit.cshtml?highlight=1)]

<span data-ttu-id="5e3fa-531">İlk satır `@page "{id:int}"` yönergesini içerir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-531">The first line contains the `@page "{id:int}"` directive.</span></span> <span data-ttu-id="5e3fa-532">Yönlendirme kısıtlaması`"{id:int}"` , sayfada `int` yönlendirme verileri içeren sayfaya istekleri kabul etmesini söyler.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-532">The routing constraint`"{id:int}"` tells the page to accept requests to the page that contain `int` route data.</span></span> <span data-ttu-id="5e3fa-533">Sayfaya yapılan bir istek öğesine `int`dönüştürülebileceği rota verileri içermiyorsa, çalışma zamanı bir HTTP 404 (bulunamadı) hatası döndürür.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-533">If a request to the page doesn't contain route data that can be converted to an `int`, the runtime returns an HTTP 404 (not found) error.</span></span> <span data-ttu-id="5e3fa-534">Kimliği isteğe bağlı yapmak için yol kısıtlamasına `?` ekleyin:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-534">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="5e3fa-535">*Pages/Edit. cshtml. cs* dosyası:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-535">The *Pages/Edit.cshtml.cs* file:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Edit.cshtml.cs)]

<span data-ttu-id="5e3fa-536">*Index. cshtml* dosyası, her müşteri kişisi için bir silme düğmesi oluşturmak için de biçimlendirme içerir:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-536">The *Index.cshtml* file also contains markup to create a delete button for each customer contact:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=22-23)]

<span data-ttu-id="5e3fa-537">Sil düğmesi HTML biçiminde işlendiğinde, `formaction` için parametreleri içerir:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-537">When the delete button is rendered in HTML, its `formaction` includes parameters for:</span></span>

* <span data-ttu-id="5e3fa-538">`asp-route-id` Özniteliği tarafından belirtilen müşteri iletişim kimliği.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-538">The customer contact ID specified by the `asp-route-id` attribute.</span></span>
* <span data-ttu-id="5e3fa-539">Özniteliği tarafından belirtilen `handler` `asp-page-handler` .</span><span class="sxs-lookup"><span data-stu-id="5e3fa-539">The `handler` specified by the `asp-page-handler` attribute.</span></span>

<span data-ttu-id="5e3fa-540">Aşağıda, müşteri irtibat KIMLIĞIYLE `1`birlikte işlenmiş bir Delete düğmesine örnek verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-540">Here is an example of a rendered delete button with a customer contact ID of `1`:</span></span>

```html
<button type="submit" formaction="/?id=1&amp;handler=delete">delete</button>
```

<span data-ttu-id="5e3fa-541">Düğme seçildiğinde, sunucuya bir form `POST` isteği gönderilir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-541">When the button is selected, a form `POST` request is sent to the server.</span></span> <span data-ttu-id="5e3fa-542">Kurala göre, işleyici yönteminin adı, şemaya `handler` `OnPost[handler]Async`göre parametrenin değerine göre seçilir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-542">By convention, the name of the handler method is selected based on the value of the `handler` parameter according to the scheme `OnPost[handler]Async`.</span></span>

<span data-ttu-id="5e3fa-543">Bu örnekte olduğundan ,`POST` isteği işlemek için işleyiciyöntemikullanılır`OnPostDeleteAsync`. `handler` `delete`</span><span class="sxs-lookup"><span data-stu-id="5e3fa-543">Because the `handler` is `delete` in this example, the `OnPostDeleteAsync` handler method is used to process the `POST` request.</span></span> <span data-ttu-id="5e3fa-544">, Gibi farklı bir değere `remove`ayarlandıysa, adında `OnPostRemoveAsync` bir işleyici yöntemi seçilir. `asp-page-handler`</span><span class="sxs-lookup"><span data-stu-id="5e3fa-544">If the `asp-page-handler` is set to a different value, such as `remove`, a handler method with the name `OnPostRemoveAsync` is selected.</span></span> <span data-ttu-id="5e3fa-545">Aşağıdaki kod `OnPostDeleteAsync` işleyiciyi göstermektedir:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-545">The following code shows the `OnPostDeleteAsync` handler:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs?range=26-37)]

<span data-ttu-id="5e3fa-546">`OnPostDeleteAsync` Yöntemi:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-546">The `OnPostDeleteAsync` method:</span></span>

* <span data-ttu-id="5e3fa-547">`id` Sorgu dizesinden öğesini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-547">Accepts the `id` from the query string.</span></span> <span data-ttu-id="5e3fa-548">*Index. cshtml* sayfa yönergesi yönlendirme kısıtlaması `"{id:int?}"`içeriyorsa, `id` rota verilerinden gelir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-548">If the *Index.cshtml* page directive contained routing constraint `"{id:int?}"`, `id` would come from route data.</span></span> <span data-ttu-id="5e3fa-549">İçin `id` rota verileri, gibi URI `https://localhost:5001/Customers/2`'de belirtilmiştir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-549">The route data for `id` is specified in the URI such as `https://localhost:5001/Customers/2`.</span></span>
* <span data-ttu-id="5e3fa-550">Müşteri iletişim `FindAsync`için veritabanını sorgular.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-550">Queries the database for the customer contact with `FindAsync`.</span></span>
* <span data-ttu-id="5e3fa-551">Müşteri ilgili kişisi bulunursa, bunlar müşteri kişileri listesinden kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-551">If the customer contact is found, they're removed from the list of customer contacts.</span></span> <span data-ttu-id="5e3fa-552">Veritabanı güncelleştirildi.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-552">The database is updated.</span></span>
* <span data-ttu-id="5e3fa-553">Kök `RedirectToPage` dizin sayfasına (`/Index`) yeniden yönlendirmek için çağrılar.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-553">Calls `RedirectToPage` to redirect to the root Index page (`/Index`).</span></span>

## <a name="mark-page-properties-as-required"></a><span data-ttu-id="5e3fa-554">Sayfa özelliklerini gerektiği gibi işaretle</span><span class="sxs-lookup"><span data-stu-id="5e3fa-554">Mark page properties as required</span></span>

<span data-ttu-id="5e3fa-555">Bir `PageModel` üzerindeki Özellikler [gerekli](/dotnet/api/system.componentmodel.dataannotations.requiredattribute) öznitelikle birlikte kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-555">Properties on a `PageModel` can be decorated with the [Required](/dotnet/api/system.componentmodel.dataannotations.requiredattribute) attribute:</span></span>

[!code-cs[](index/sample/Create.cshtml.cs?highlight=3,15-16)]

<span data-ttu-id="5e3fa-556">Daha fazla bilgi için bkz. [model doğrulaması](xref:mvc/models/validation).</span><span class="sxs-lookup"><span data-stu-id="5e3fa-556">For more information, see [Model validation](xref:mvc/models/validation).</span></span>

## <a name="handle-head-requests-with-an-onget-handler-fallback"></a><span data-ttu-id="5e3fa-557">OnGet işleyicisi geri dönüşü ile tanıtıcı HEAD istekleri</span><span class="sxs-lookup"><span data-stu-id="5e3fa-557">Handle HEAD requests with an OnGet handler fallback</span></span>

<span data-ttu-id="5e3fa-558">`HEAD`istekleri belirli bir kaynak için üstbilgileri almanıza izin verir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-558">`HEAD` requests allow you to retrieve the headers for a specific resource.</span></span> <span data-ttu-id="5e3fa-559">İsteklerin aksine `GET`isteklerbiryanıtgövdesi döndürmez.`HEAD`</span><span class="sxs-lookup"><span data-stu-id="5e3fa-559">Unlike `GET` requests, `HEAD` requests don't return a response body.</span></span>

<span data-ttu-id="5e3fa-560">Normalde, istekler `OnHead` için `HEAD` bir işleyici oluşturulur ve çağırılır:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-560">Ordinarily, an `OnHead` handler is created and called for `HEAD` requests:</span></span> 

```csharp
public void OnHead()
{
    HttpContext.Response.Headers.Add("HandledBy", "Handled by OnHead!");
}
```

<span data-ttu-id="5e3fa-561">ASP.NET Core 2,1 veya sonraki bir sürümde, hiçbir `OnGet` `OnHead` işleyici tanımlanmazsa, Razor Pages işleyiciyi çağırmaya geri döner.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-561">In ASP.NET Core 2.1 or later, Razor Pages falls back to calling the `OnGet` handler if no `OnHead` handler is defined.</span></span> <span data-ttu-id="5e3fa-562">Bu davranış, içindeki `Startup.ConfigureServices` [setcompatibilityversion](xref:mvc/compatibility-version) çağrısıyla etkinleştirilir:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-562">This behavior is enabled by the call to [SetCompatibilityVersion](xref:mvc/compatibility-version) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddMvc()
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_1);
```

<span data-ttu-id="5e3fa-563">Varsayılan Şablonlar ASP.NET Core 2,1 ve `SetCompatibilityVersion` 2,2 ' de çağrıyı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-563">The default templates generate the `SetCompatibilityVersion` call in ASP.NET Core 2.1 and 2.2.</span></span> <span data-ttu-id="5e3fa-564">`SetCompatibilityVersion`Razor Pages seçeneğini `AllowMappingHeadRequestsToGetHandler` etkin bir şekilde `true`ayarlar.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-564">`SetCompatibilityVersion` effectively sets the Razor Pages option `AllowMappingHeadRequestsToGetHandler` to `true`.</span></span>

<span data-ttu-id="5e3fa-565">İle `SetCompatibilityVersion`tüm davranışlardan çıkmak yerine, açıkça *belirli* davranışları kabul edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-565">Rather than opting in to all behaviors with `SetCompatibilityVersion`, you can explicitly opt in to *specific* behaviors.</span></span> <span data-ttu-id="5e3fa-566">Aşağıdaki kod, isteklerin `HEAD` `OnGet` işleyiciye eşlenmesine izin vermek için ' de kullanılır:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-566">The following code opts in to allowing `HEAD` requests to be mapped to the `OnGet` handler:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        options.AllowMappingHeadRequestsToGetHandler = true;
    });
```

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a><span data-ttu-id="5e3fa-567">XSRF/CSRF ve Razor Pages</span><span class="sxs-lookup"><span data-stu-id="5e3fa-567">XSRF/CSRF and Razor Pages</span></span>

<span data-ttu-id="5e3fa-568">[Antiforgery doğrulaması](xref:security/anti-request-forgery)için herhangi bir kod yazmanız gerekmez.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-568">You don't have to write any code for [antiforgery validation](xref:security/anti-request-forgery).</span></span> <span data-ttu-id="5e3fa-569">Antiforgery belirteci oluşturma ve doğrulama, Razor Pages otomatik olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-569">Antiforgery token generation and validation are automatically included in Razor Pages.</span></span>

<a name="layout"></a>

## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a><span data-ttu-id="5e3fa-570">Razor Pages ile düzenleri, partileri, şablonları ve etiket yardımcılarını kullanma</span><span class="sxs-lookup"><span data-stu-id="5e3fa-570">Using Layouts, partials, templates, and Tag Helpers with Razor Pages</span></span>

<span data-ttu-id="5e3fa-571">Sayfalar, Razor görünüm altyapısının tüm özellikleri ile çalışır.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-571">Pages work with all the capabilities of the Razor view engine.</span></span> <span data-ttu-id="5e3fa-572">Düzenler, partıals, şablonlar, etiket yardımcıları, *_Viewstart. cshtml*, *_viewwimports. cshtml* geleneksel Razor görünümlerinde oldukları gibi çalışır.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-572">Layouts, partials, templates, Tag Helpers, *_ViewStart.cshtml*, *_ViewImports.cshtml* work in the same way they do for conventional Razor views.</span></span>

<span data-ttu-id="5e3fa-573">Bu özelliklerden bazılarının avantajlarından yararlanarak bu sayfayı declutter edelim.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-573">Let's declutter this page by taking advantage of some of those capabilities.</span></span>

<span data-ttu-id="5e3fa-574">*Pages/Shared/_Layout. cshtml*öğesine bir [Düzen sayfası](xref:mvc/views/layout) ekleyin:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-574">Add a [layout page](xref:mvc/views/layout) to *Pages/Shared/_Layout.cshtml*:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_LayoutSimple.cshtml)]

<span data-ttu-id="5e3fa-575">[Düzen](xref:mvc/views/layout):</span><span class="sxs-lookup"><span data-stu-id="5e3fa-575">The [Layout](xref:mvc/views/layout):</span></span>

* <span data-ttu-id="5e3fa-576">Her sayfanın yerleşimini denetler (sayfa düzen dışında değilse).</span><span class="sxs-lookup"><span data-stu-id="5e3fa-576">Controls the layout of each page (unless the page opts out of layout).</span></span>
* <span data-ttu-id="5e3fa-577">JavaScript ve stil sayfaları gibi HTML yapılarını içeri aktarır.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-577">Imports HTML structures such as JavaScript and stylesheets.</span></span>

<span data-ttu-id="5e3fa-578">Daha fazla bilgi için bkz. [Düzen sayfası](xref:mvc/views/layout) .</span><span class="sxs-lookup"><span data-stu-id="5e3fa-578">See [layout page](xref:mvc/views/layout) for more information.</span></span>

<span data-ttu-id="5e3fa-579">[Layout](xref:mvc/views/layout#specifying-a-layout) özelliği *Pages/_viewstart. cshtml*içinde ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-579">The [Layout](xref:mvc/views/layout#specifying-a-layout) property is set in *Pages/_ViewStart.cshtml*:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]

<span data-ttu-id="5e3fa-580">Düzen *Sayfalar/paylaşılan* klasöründedir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-580">The layout is in the *Pages/Shared* folder.</span></span> <span data-ttu-id="5e3fa-581">Sayfalar, geçerli sayfayla aynı klasörden başlayarak diğer görünümleri (düzenler, şablonlar, parals) hiyerarşik olarak arar.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-581">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="5e3fa-582">*Sayfalar/paylaşılan* klasördeki bir düzen, *Sayfalar* klasörü altındaki herhangi bir Razor sayfasından kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-582">A layout in the *Pages/Shared* folder can be used from any Razor page under the *Pages* folder.</span></span>

<span data-ttu-id="5e3fa-583">Düzen dosyası *Sayfalar/paylaşılan* klasörüne gitmelidir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-583">The layout file should go in the *Pages/Shared* folder.</span></span>

<span data-ttu-id="5e3fa-584">Düzen dosyasını *Görünümler/paylaşılan* klasöre **yerleştirmenizi öneririz** .</span><span class="sxs-lookup"><span data-stu-id="5e3fa-584">We recommend you **not** put the layout file in the *Views/Shared* folder.</span></span> <span data-ttu-id="5e3fa-585">*Görünümler/paylaşılan* bir MVC görünümleri modelidir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-585">*Views/Shared* is an MVC views pattern.</span></span> <span data-ttu-id="5e3fa-586">Razor Pages, yol kurallarını değil klasör hiyerarşisine güvenmektir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-586">Razor Pages are meant to rely on folder hierarchy, not path conventions.</span></span>

<span data-ttu-id="5e3fa-587">Bir Razor sayfasından arama görüntüleme, *Sayfalar* klasörünü içerir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-587">View search from a Razor Page includes the *Pages* folder.</span></span> <span data-ttu-id="5e3fa-588">MVC denetleyicileri ve geleneksel Razor görünümleriyle kullandığınız düzenler, şablonlar ve parals işlemleri *yalnızca çalışır*.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-588">The layouts, templates, and partials you're using with MVC controllers and conventional Razor views *just work*.</span></span>

<span data-ttu-id="5e3fa-589">*Pages/_Viewwimports. cshtml* dosyası ekleyin:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-589">Add a *Pages/_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]

<span data-ttu-id="5e3fa-590">`@namespace`, Öğreticinin ilerleyen kısımlarında açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-590">`@namespace` is explained later in the tutorial.</span></span> <span data-ttu-id="5e3fa-591">Yönergesi, [yerleşik etiket yardımcılarını](xref:mvc/views/tag-helpers/builtin-th/Index) sayfalar klasöründeki tüm sayfalara getirir. `@addTagHelper`</span><span class="sxs-lookup"><span data-stu-id="5e3fa-591">The `@addTagHelper` directive brings in the [built-in Tag Helpers](xref:mvc/views/tag-helpers/builtin-th/Index) to all the pages in the *Pages* folder.</span></span>

<a name="namespace"></a>

<span data-ttu-id="5e3fa-592">`@namespace` Yönerge bir sayfada açıkça kullanıldığında:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-592">When the `@namespace` directive is used explicitly on a page:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]

<span data-ttu-id="5e3fa-593">Yönergesi sayfanın ad alanını ayarlar.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-593">The directive sets the namespace for the page.</span></span> <span data-ttu-id="5e3fa-594">`@model` Yönergesinin ad alanını içermesi gerekmez.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-594">The `@model` directive doesn't need to include the namespace.</span></span>

<span data-ttu-id="5e3fa-595">Yönerge _viewwimports *. cshtml*içinde yer aldığında, belirtilen ad alanı, `@namespace` yönergeyi içeri aktaran sayfada oluşturulan ad alanı için ön ek sağlar. `@namespace`</span><span class="sxs-lookup"><span data-stu-id="5e3fa-595">When the `@namespace` directive is contained in *_ViewImports.cshtml*, the specified namespace supplies the prefix for the generated namespace in the Page that imports the `@namespace` directive.</span></span> <span data-ttu-id="5e3fa-596">Oluşturulan ad alanının geri kalanı (sonek bölümü), *_Viewwimports. cshtml* dosyasını ve sayfayı içeren klasörü içeren, noktayla ayrılmış göreli yoldur.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-596">The rest of the generated namespace (the suffix portion) is the dot-separated relative path between the folder containing *_ViewImports.cshtml* and the folder containing the page.</span></span>

<span data-ttu-id="5e3fa-597">Örneğin, `PageModel` *Pages/Customers/Edit. cshtml. cs* sınıfı, ad alanını açıkça ayarlar:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-597">For example, the `PageModel` class *Pages/Customers/Edit.cshtml.cs* explicitly sets the namespace:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=snippet_namespace)]

<span data-ttu-id="5e3fa-598">*Pages/_Viewwimports. cshtml* dosyası aşağıdaki ad alanını ayarlar:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-598">The *Pages/_ViewImports.cshtml* file sets the following namespace:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]

<span data-ttu-id="5e3fa-599">*Pages/Customers/Edit. cshtml* Razor sayfasının oluşturulan ad alanı `PageModel` sınıfıyla aynıdır.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-599">The generated namespace for the *Pages/Customers/Edit.cshtml* Razor Page is the same as the `PageModel` class.</span></span>

<span data-ttu-id="5e3fa-600">`@namespace`*Ayrıca geleneksel Razor görünümleriyle birlikte da geçerlidir.*</span><span class="sxs-lookup"><span data-stu-id="5e3fa-600">`@namespace` *also works with conventional Razor views.*</span></span>

<span data-ttu-id="5e3fa-601">Özgün *Sayfalar/Create. cshtml* görünüm dosyası:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-601">The original *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml?highlight=2)]

<span data-ttu-id="5e3fa-602">Güncelleştirilmiş *Sayfalar/Create. cshtml* görünüm dosyası:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-602">The updated *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/Create.cshtml?highlight=2)]

<span data-ttu-id="5e3fa-603">[Razor Pages Başlatıcı projesi](#rpvs17) , istemci tarafı doğrulamayı bağlayan *sayfaları/_ValidationScriptsPartial. cshtml*'yi içerir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-603">The [Razor Pages starter project](#rpvs17) contains the *Pages/_ValidationScriptsPartial.cshtml*, which hooks up client-side validation.</span></span>

<span data-ttu-id="5e3fa-604">Kısmi görünümler hakkında daha fazla bilgi için bkz <xref:mvc/views/partial>.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-604">For more information on partial views, see <xref:mvc/views/partial>.</span></span>

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a><span data-ttu-id="5e3fa-605">Sayfalar için URL oluşturma</span><span class="sxs-lookup"><span data-stu-id="5e3fa-605">URL generation for Pages</span></span>

<span data-ttu-id="5e3fa-606">Daha önce gösterilen `RedirectToPage` `Create` sayfa şunları kullanır:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-606">The `Create` page, shown previously, uses `RedirectToPage`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=10)]

<span data-ttu-id="5e3fa-607">Uygulama aşağıdaki dosya/klasör yapısına sahiptir:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-607">The app has the following file/folder structure:</span></span>

* <span data-ttu-id="5e3fa-608">*/Pages*</span><span class="sxs-lookup"><span data-stu-id="5e3fa-608">*/Pages*</span></span>

  * <span data-ttu-id="5e3fa-609">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="5e3fa-609">*Index.cshtml*</span></span>
  * <span data-ttu-id="5e3fa-610">*/Customers*</span><span class="sxs-lookup"><span data-stu-id="5e3fa-610">*/Customers*</span></span>

    * <span data-ttu-id="5e3fa-611">*. Cshtml oluştur*</span><span class="sxs-lookup"><span data-stu-id="5e3fa-611">*Create.cshtml*</span></span>
    * <span data-ttu-id="5e3fa-612">*Edit.cshtml*</span><span class="sxs-lookup"><span data-stu-id="5e3fa-612">*Edit.cshtml*</span></span>
    * <span data-ttu-id="5e3fa-613">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="5e3fa-613">*Index.cshtml*</span></span>

<span data-ttu-id="5e3fa-614">*Pages/Customers/Create. cshtml* ve *Pages/Customers/Edit. cshtml* sayfaları, başarılı olduktan sonra *Pages/Index. cshtml* dosyasına yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-614">The *Pages/Customers/Create.cshtml* and *Pages/Customers/Edit.cshtml* pages redirect to *Pages/Index.cshtml* after success.</span></span> <span data-ttu-id="5e3fa-615">Dize `/Index` , önceki sayfaya erişmek için URI 'nin bir parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-615">The string `/Index` is part of the URI to access the preceding page.</span></span> <span data-ttu-id="5e3fa-616">Dize `/Index` , *Sayfalar/Index. cshtml* sayfasına URI 'ler oluşturmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-616">The string `/Index` can be used to generate URIs to the *Pages/Index.cshtml* page.</span></span> <span data-ttu-id="5e3fa-617">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-617">For example:</span></span>

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">My Index Page</a>`
* `RedirectToPage("/Index")`

<span data-ttu-id="5e3fa-618">Sayfa adı, kök */Pages* klasöründeki sayfanın başında `/` (örneğin, `/Index`) bir yoldur.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-618">The page name is the path to the page from the root */Pages* folder including a leading `/` (for example, `/Index`).</span></span> <span data-ttu-id="5e3fa-619">Önceki URL oluşturma örnekleri bir URL 'YI kodlamadan gelişmiş seçenekler ve işlevsel yetenekler sunar.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-619">The preceding URL generation samples offer enhanced options and functional capabilities over hardcoding a URL.</span></span> <span data-ttu-id="5e3fa-620">URL oluşturma [yönlendirme](xref:mvc/controllers/routing) kullanır ve yolun hedef yolda nasıl tanımlandığınıza göre parametreleri oluşturabilir ve kodlayabilir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-620">URL generation uses [routing](xref:mvc/controllers/routing) and can generate and encode parameters according to how the route is defined in the destination path.</span></span>

<span data-ttu-id="5e3fa-621">Sayfalar için URL oluşturma göreli adları destekler.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-621">URL generation for pages supports relative names.</span></span> <span data-ttu-id="5e3fa-622">Aşağıdaki tabloda, *sayfa/müşteri/oluşturma. cshtml*'den `RedirectToPage` farklı parametrelerle hangi dizin sayfasının seçildiği gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-622">The following table shows which Index page is selected with different `RedirectToPage` parameters from *Pages/Customers/Create.cshtml*:</span></span>

| <span data-ttu-id="5e3fa-623">RedirectToPage (x)</span><span class="sxs-lookup"><span data-stu-id="5e3fa-623">RedirectToPage(x)</span></span>| <span data-ttu-id="5e3fa-624">Sayfasında</span><span class="sxs-lookup"><span data-stu-id="5e3fa-624">Page</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="5e3fa-625">RedirectToPage ("/Index")</span><span class="sxs-lookup"><span data-stu-id="5e3fa-625">RedirectToPage("/Index")</span></span> | <span data-ttu-id="5e3fa-626">*Sayfa/dizin*</span><span class="sxs-lookup"><span data-stu-id="5e3fa-626">*Pages/Index*</span></span> |
| <span data-ttu-id="5e3fa-627">RedirectToPage ("./Index");</span><span class="sxs-lookup"><span data-stu-id="5e3fa-627">RedirectToPage("./Index");</span></span> | <span data-ttu-id="5e3fa-628">*Sayfalar/müşteriler/Dizin*</span><span class="sxs-lookup"><span data-stu-id="5e3fa-628">*Pages/Customers/Index*</span></span> |
| <span data-ttu-id="5e3fa-629">RedirectToPage (". /İndex ")</span><span class="sxs-lookup"><span data-stu-id="5e3fa-629">RedirectToPage("../Index")</span></span> | <span data-ttu-id="5e3fa-630">*Sayfa/dizin*</span><span class="sxs-lookup"><span data-stu-id="5e3fa-630">*Pages/Index*</span></span> |
| <span data-ttu-id="5e3fa-631">RedirectToPage ("Dizin")</span><span class="sxs-lookup"><span data-stu-id="5e3fa-631">RedirectToPage("Index")</span></span>  | <span data-ttu-id="5e3fa-632">*Sayfalar/müşteriler/Dizin*</span><span class="sxs-lookup"><span data-stu-id="5e3fa-632">*Pages/Customers/Index*</span></span> |

<span data-ttu-id="5e3fa-633">`RedirectToPage("Index")`, `RedirectToPage("./Index")` ve `RedirectToPage("./Index")` , *göreli adlardır*.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-633">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, and `RedirectToPage("../Index")`  are *relative names*.</span></span> <span data-ttu-id="5e3fa-634">Parametresi, hedef sayfanın adını hesaplamak için geçerli sayfanın yoluyla *birleştirilir.* `RedirectToPage`</span><span class="sxs-lookup"><span data-stu-id="5e3fa-634">The `RedirectToPage` parameter is *combined* with the path of the current page to compute the name of the destination page.</span></span>  <!-- Review: Original had The provided string is combined with the page name of the current page to compute the name of the destination page.  page name, not page path -->

<span data-ttu-id="5e3fa-635">Karmaşık bir yapıya sahip siteler oluştururken göreli ad bağlama yararlı olur.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-635">Relative name linking is useful when building sites with a complex structure.</span></span> <span data-ttu-id="5e3fa-636">Bir klasördeki sayfalar arasında bağlantı sağlamak için göreli adlar kullanırsanız, bu klasörü yeniden adlandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-636">If you use relative names to link between pages in a folder, you can rename that folder.</span></span> <span data-ttu-id="5e3fa-637">Tüm bağlantılar hala çalışır (klasör adını içermediği için).</span><span class="sxs-lookup"><span data-stu-id="5e3fa-637">All the links still work (because they didn't include the folder name).</span></span>

<span data-ttu-id="5e3fa-638">Farklı bir [alandaki](xref:mvc/controllers/areas)bir sayfaya yeniden yönlendirmek için alanını belirtin:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-638">To redirect to a page in a different [Area](xref:mvc/controllers/areas), specify the area:</span></span>

```csharp
RedirectToPage("/Index", new { area = "Services" });
```

<span data-ttu-id="5e3fa-639">Daha fazla bilgi için bkz. <xref:mvc/controllers/areas>.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-639">For more information, see <xref:mvc/controllers/areas>.</span></span>

## <a name="viewdata-attribute"></a><span data-ttu-id="5e3fa-640">ViewData özniteliği</span><span class="sxs-lookup"><span data-stu-id="5e3fa-640">ViewData attribute</span></span>

<span data-ttu-id="5e3fa-641">Veri, [Viewdataattribute](/dotnet/api/microsoft.aspnetcore.mvc.viewdataattribute)içeren bir sayfaya geçirilebilir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-641">Data can be passed to a page with [ViewDataAttribute](/dotnet/api/microsoft.aspnetcore.mvc.viewdataattribute).</span></span> <span data-ttu-id="5e3fa-642">Denetleyiciler veya Razor sayfa modelleriyle birlikte `[ViewData]` düzenlenmiş özellikler, değerlerini, [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary)'den saklı ve yüklenmiş olarak alır.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-642">Properties on controllers or Razor Page models decorated with `[ViewData]` have their values stored and loaded from the [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary).</span></span>

<span data-ttu-id="5e3fa-643">Aşağıdaki örnekte `AboutModel` , ile `[ViewData]`donatılmış bir `Title` özelliği içerir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-643">In the following example, the `AboutModel` contains a `Title` property decorated with `[ViewData]`.</span></span> <span data-ttu-id="5e3fa-644">`Title` Özelliği hakkında sayfasının başlığına ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-644">The `Title` property is set to the title of the About page:</span></span>

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

<span data-ttu-id="5e3fa-645">Hakkında sayfasında, `Title` özelliğe model özelliği olarak erişin:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-645">In the About page, access the `Title` property as a model property:</span></span>

```cshtml
<h1>@Model.Title</h1>
```

<span data-ttu-id="5e3fa-646">Mizanpajda, başlık ViewData sözlüğünden okundu:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-646">In the layout, the title is read from the ViewData dictionary:</span></span>

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"] - WebApplication</title>
    ...
```

## <a name="tempdata"></a><span data-ttu-id="5e3fa-647">TempData</span><span class="sxs-lookup"><span data-stu-id="5e3fa-647">TempData</span></span>

<span data-ttu-id="5e3fa-648">ASP.NET Core bir [denetleyicide](/dotnet/api/microsoft.aspnetcore.mvc.controller) [TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) özelliğini kullanıma sunar.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-648">ASP.NET Core exposes the [TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) property on a [controller](/dotnet/api/microsoft.aspnetcore.mvc.controller).</span></span> <span data-ttu-id="5e3fa-649">Bu özellik, okunana kadar verileri depolar.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-649">This property stores data until it's read.</span></span> <span data-ttu-id="5e3fa-650">`Keep` Ve`Peek` yöntemleri silmeden verileri incelemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-650">The `Keep` and `Peek` methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="5e3fa-651">`TempData`, bir tek istekten daha fazla veri gerektiğinde yeniden yönlendirme için yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-651">`TempData` is  useful for redirection, when data is needed for more than a single request.</span></span>

<span data-ttu-id="5e3fa-652">Aşağıdaki kod, şunu `Message` kullanarak `TempData`değerini ayarlar:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-652">The following code sets the value of `Message` using `TempData`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,25&name=snippet_Temp)]

<span data-ttu-id="5e3fa-653">*Pages/Customers/Index. cshtml* dosyasında aşağıdaki biçimlendirme, `Message` using `TempData`değerini gösterir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-653">The following markup in the *Pages/Customers/Index.cshtml* file displays the value of `Message` using `TempData`.</span></span>

```cshtml
<h3>Msg: @Model.Message</h3>
```

<span data-ttu-id="5e3fa-654">*Pages/Customers/Index. cshtml. cs* sayfa modeli, `[TempData]` `Message` özelliğine özniteliğini uygular.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-654">The *Pages/Customers/Index.cshtml.cs* page model applies the `[TempData]` attribute to the `Message` property.</span></span>

```cs
[TempData]
public string Message { get; set; }
```

<span data-ttu-id="5e3fa-655">Daha fazla bilgi için bkz. [TempData](xref:fundamentals/app-state#tempdata) .</span><span class="sxs-lookup"><span data-stu-id="5e3fa-655">For more information, see [TempData](xref:fundamentals/app-state#tempdata) .</span></span>

<a name="mhpp"></a>

## <a name="multiple-handlers-per-page"></a><span data-ttu-id="5e3fa-656">Sayfa başına birden çok işleyici</span><span class="sxs-lookup"><span data-stu-id="5e3fa-656">Multiple handlers per page</span></span>

<span data-ttu-id="5e3fa-657">Aşağıdaki sayfa, `asp-page-handler` etiket Yardımcısını kullanarak iki işleyici için biçimlendirme oluşturur:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-657">The following page generates markup for two handlers using the `asp-page-handler` Tag Helper:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

<!-- Review: the FormActionTagHelper applies to all <form /> elements on a Razor page, even when there's no `asp-` attribute   -->

<span data-ttu-id="5e3fa-658">Yukarıdaki örnekteki formda, her biri farklı bir URL 'ye göndermek `FormActionTagHelper` için kullanan iki gönderme düğmesi vardır.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-658">The form in the preceding example has two submit buttons, each using the `FormActionTagHelper` to submit to a different URL.</span></span> <span data-ttu-id="5e3fa-659">Özniteliği, için `asp-page`bir yardımcı ' dir. `asp-page-handler`</span><span class="sxs-lookup"><span data-stu-id="5e3fa-659">The `asp-page-handler` attribute is a companion to `asp-page`.</span></span> <span data-ttu-id="5e3fa-660">`asp-page-handler`bir sayfa tarafından tanımlanan her bir işleyici yöntemini gönderen URL 'Ler oluşturur.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-660">`asp-page-handler` generates URLs that submit to each of the handler methods defined by a page.</span></span> <span data-ttu-id="5e3fa-661">`asp-page`örnek geçerli sayfaya bağlandığından belirtilmedi.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-661">`asp-page` isn't specified because the sample is linking to the current page.</span></span>

<span data-ttu-id="5e3fa-662">Sayfa modeli:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-662">The page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]

<span data-ttu-id="5e3fa-663">Yukarıdaki kod, *adlandırılmış işleyici yöntemlerini*kullanır.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-663">The preceding code uses *named handler methods*.</span></span> <span data-ttu-id="5e3fa-664">Adlandırılmış işleyici yöntemleri, `On<HTTP Verb>` ve öncesinde `Async` (varsa) ad içindeki metin alınarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-664">Named handler methods are created by taking the text in the name after `On<HTTP Verb>` and before `Async` (if present).</span></span> <span data-ttu-id="5e3fa-665">Yukarıdaki örnekte, Page metotları OnPost**Joinlist**Async ve onpost**Joinlıstuc**Async ' dir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-665">In the preceding example, the page methods are OnPost**JoinList**Async and OnPost**JoinListUC**Async.</span></span> <span data-ttu-id="5e3fa-666">*Onpost* ile *zaman uyumsuz* olarak kaldırıldığında, işleyici adları ve `JoinList` `JoinListUC`' dir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-666">With *OnPost* and *Async* removed, the handler names are `JoinList` and `JoinListUC`.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?range=12-13)]

<span data-ttu-id="5e3fa-667">Önceki kodu kullanarak, ' a gönderen `OnPostJoinListAsync` `https://localhost:5001/Customers/CreateFATH?handler=JoinList`URL yolu.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-667">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `https://localhost:5001/Customers/CreateFATH?handler=JoinList`.</span></span> <span data-ttu-id="5e3fa-668">' A gönderen `OnPostJoinListUCAsync` `https://localhost:5001/Customers/CreateFATH?handler=JoinListUC`URL yolu.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-668">The URL path that submits to `OnPostJoinListUCAsync` is `https://localhost:5001/Customers/CreateFATH?handler=JoinListUC`.</span></span>

## <a name="custom-routes"></a><span data-ttu-id="5e3fa-669">Özel yollar</span><span class="sxs-lookup"><span data-stu-id="5e3fa-669">Custom routes</span></span>

<span data-ttu-id="5e3fa-670">`@page` İçin yönergesini kullanın:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-670">Use the `@page` directive to:</span></span>

* <span data-ttu-id="5e3fa-671">Sayfaya özel bir yol belirtin.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-671">Specify a custom route to a page.</span></span> <span data-ttu-id="5e3fa-672">Örneğin, hakkında sayfasına olan yol ile `/Some/Other/Path` `@page "/Some/Other/Path"`öğesine ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-672">For example, the route to the About page can be set to `/Some/Other/Path` with `@page "/Some/Other/Path"`.</span></span>
* <span data-ttu-id="5e3fa-673">Kesimleri bir sayfanın varsayılan yoluna ekleyin.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-673">Append segments to a page's default route.</span></span> <span data-ttu-id="5e3fa-674">Örneğin, bir "öğe" segmenti sayfanın varsayılan rotasına `@page "item"`eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-674">For example, an "item" segment can be added to a page's default route with `@page "item"`.</span></span>
* <span data-ttu-id="5e3fa-675">Bir sayfanın varsayılan yoluna parametreleri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-675">Append parameters to a page's default route.</span></span> <span data-ttu-id="5e3fa-676">Örneğin, bir ID parametresi `id`, içeren `@page "{id}"`bir sayfa için gerekli olabilir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-676">For example, an ID parameter, `id`, can be required for a page with `@page "{id}"`.</span></span>

<span data-ttu-id="5e3fa-677">Yolun başındaki bir tilde (`~`) tarafından belirlenen kök göreli bir yol desteklenir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-677">A root-relative path designated by a tilde (`~`) at the beginning of the path is supported.</span></span> <span data-ttu-id="5e3fa-678">Örneğin, `@page "~/Some/Other/Path"` ile `@page "/Some/Other/Path"`aynıdır.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-678">For example, `@page "~/Some/Other/Path"` is the same as `@page "/Some/Other/Path"`.</span></span>

<span data-ttu-id="5e3fa-679">Yol şablonunu `?handler=JoinList` `/JoinList` belirterek,URL'dekisorgudizesinibirrota`@page "{handler?}"`segmentine dönüştürebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-679">You can change the query string `?handler=JoinList` in the URL to a route segment `/JoinList` by specifying the route template `@page "{handler?}"`.</span></span>

<span data-ttu-id="5e3fa-680">URL 'de sorgu dizesini `?handler=JoinList` beğenmezseniz, yolu URL 'nin yol bölümüne koymak için yolu değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-680">If you don't like the query string `?handler=JoinList` in the URL, you can change the route to put the handler name in the path portion of the URL.</span></span> <span data-ttu-id="5e3fa-681">`@page` Yönergeden sonra çift tırnak içine alınmış bir rota şablonu ekleyerek yolu özelleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-681">You can customize the route by adding a route template enclosed in double quotes after the `@page` directive.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]

<span data-ttu-id="5e3fa-682">Önceki kodu kullanarak, ' a gönderen `OnPostJoinListAsync` `https://localhost:5001/Customers/CreateFATH/JoinList`URL yolu.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-682">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `https://localhost:5001/Customers/CreateFATH/JoinList`.</span></span> <span data-ttu-id="5e3fa-683">' A gönderen `OnPostJoinListUCAsync` `https://localhost:5001/Customers/CreateFATH/JoinListUC`URL yolu.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-683">The URL path that submits to `OnPostJoinListUCAsync` is `https://localhost:5001/Customers/CreateFATH/JoinListUC`.</span></span>

<span data-ttu-id="5e3fa-684">`?` Aşağıda`handler` yol parametresinin isteğe bağlı olduğu anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-684">The `?` following `handler` means the route parameter is optional.</span></span>

## <a name="configuration-and-settings"></a><span data-ttu-id="5e3fa-685">Yapılandırma ve ayarlar</span><span class="sxs-lookup"><span data-stu-id="5e3fa-685">Configuration and settings</span></span>

<span data-ttu-id="5e3fa-686">Gelişmiş seçenekleri yapılandırmak için, MVC Oluşturucu 'da genişletme `AddRazorPagesOptions` yöntemini kullanın:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-686">To configure advanced options, use the extension method `AddRazorPagesOptions` on the MVC builder:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/StartupAdvanced.cs?name=snippet_1)]

<span data-ttu-id="5e3fa-687">Şu anda ' nı, `RazorPagesOptions` sayfalar için kök dizini ayarlamak veya sayfalar için uygulama modeli kuralları eklemek için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-687">Currently you can use the `RazorPagesOptions` to set the root directory for pages, or add application model conventions for pages.</span></span> <span data-ttu-id="5e3fa-688">Gelecekte bu şekilde daha fazla genişletilebilirlik etkinleştireceğiz.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-688">We'll enable more extensibility this way in the future.</span></span>

<span data-ttu-id="5e3fa-689">Görünümleri önceden derlemek için bkz. [Razor görünüm derlemesi](xref:mvc/views/view-compilation) .</span><span class="sxs-lookup"><span data-stu-id="5e3fa-689">To precompile views, see [Razor view compilation](xref:mvc/views/view-compilation) .</span></span>

<span data-ttu-id="5e3fa-690">[Örnek kodu indirin veya görüntüleyin](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/index/sample).</span><span class="sxs-lookup"><span data-stu-id="5e3fa-690">[Download or view sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/index/sample).</span></span>

<span data-ttu-id="5e3fa-691">Bu giriş hakkında bilgi için bkz. [Razor Pages kullanmaya başlama](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="5e3fa-691">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start), which builds on this introduction.</span></span>

### <a name="specify-that-razor-pages-are-at-the-content-root"></a><span data-ttu-id="5e3fa-692">Razor Pages içerik kökünde olduğunu belirtin</span><span class="sxs-lookup"><span data-stu-id="5e3fa-692">Specify that Razor Pages are at the content root</span></span>

<span data-ttu-id="5e3fa-693">Varsayılan olarak, Razor Pages */Pages* dizininde kök olarak depolanır.</span><span class="sxs-lookup"><span data-stu-id="5e3fa-693">By default, Razor Pages are rooted in the */Pages* directory.</span></span> <span data-ttu-id="5e3fa-694">Razor Pages, uygulamanın içerik kökünde ([Contentrootpath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) olduğunu belirtmek Için [Addmvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) 'ye [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) ekleyin:</span><span class="sxs-lookup"><span data-stu-id="5e3fa-694">Add [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at the content root ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) of the app:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesAtContentRoot();
```

### <a name="specify-that-razor-pages-are-at-a-custom-root-directory"></a><span data-ttu-id="5e3fa-695">Razor Pages özel kök dizinde olduğunu belirtin</span><span class="sxs-lookup"><span data-stu-id="5e3fa-695">Specify that Razor Pages are at a custom root directory</span></span>

<span data-ttu-id="5e3fa-696">Razor Pages uygulamadaki özel bir kök dizinde olduğunu belirtmek için [Addmvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) 'ye [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) ekleyin (göreli bir yol sağlayın):</span><span class="sxs-lookup"><span data-stu-id="5e3fa-696">Add [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at a custom root directory in the app (provide a relative path):</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesRoot("/path/to/razor/pages");
```

## <a name="additional-resources"></a><span data-ttu-id="5e3fa-697">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="5e3fa-697">Additional resources</span></span>

* <xref:index>
* <xref:mvc/views/razor>
* <xref:mvc/controllers/areas>
* <xref:tutorials/razor-pages/razor-pages-start>
* <xref:security/authorization/razor-pages-authorization>
* <xref:razor-pages/razor-pages-conventions>
* <xref:test/razor-pages-tests>
* <xref:mvc/views/partial>

::: moniker-end
