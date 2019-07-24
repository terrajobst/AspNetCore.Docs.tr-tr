---
title: ASP.NET Core Razor Pages giriş
author: Rick-Anderson
description: Nasıl ASP.NET Core Razor sayfalar kodlama sayfa odaklı senaryolar daha kolay ve MVC kullanmaktan daha üretken hale getirdiğini öğrenin.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 04/06/2019
uid: razor-pages/index
ms.openlocfilehash: 406e89c96ea63493091d0287077e244faee5f730
ms.sourcegitcommit: b40613c603d6f0cc71f3232c16df61550907f550
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68308013"
---
# <a name="introduction-to-razor-pages-in-aspnet-core"></a><span data-ttu-id="8d2a2-103">ASP.NET Core Razor Pages giriş</span><span class="sxs-lookup"><span data-stu-id="8d2a2-103">Introduction to Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="8d2a2-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Ryan şimdi ak](https://github.com/rynowak)</span><span class="sxs-lookup"><span data-stu-id="8d2a2-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Ryan Nowak](https://github.com/rynowak)</span></span>

<span data-ttu-id="8d2a2-105">Razor Pages, kod odaklı senaryoları daha kolay ve daha üretken hale getiren ASP.NET Core MVC 'nin yeni bir yönüdür.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-105">Razor Pages is a new aspect of ASP.NET Core MVC that makes coding page-focused scenarios easier and more productive.</span></span>

<span data-ttu-id="8d2a2-106">Model-View-Controller yaklaşımını kullanan bir öğretici arıyorsanız, bkz. [ASP.NET Core MVC ile çalışmaya başlama](xref:tutorials/first-mvc-app/start-mvc).</span><span class="sxs-lookup"><span data-stu-id="8d2a2-106">If you're looking for a tutorial that uses the Model-View-Controller approach, see [Get started with ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span></span>

<span data-ttu-id="8d2a2-107">Bu belge Razor Pages bir giriş sağlar.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-107">This document provides an introduction to Razor Pages.</span></span> <span data-ttu-id="8d2a2-108">Adım adım öğretici değildir.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-108">It's not a step by step tutorial.</span></span> <span data-ttu-id="8d2a2-109">Bölümlerden bazılarını çok gelişmiş bir şekilde bulursanız, bkz. [Razor Pages kullanmaya başlama](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="8d2a2-109">If you find some of the sections too advanced, see [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span></span> <span data-ttu-id="8d2a2-110">ASP.NET Core genel bir bakış için bkz. [ASP.NET Core giriş](xref:index).</span><span class="sxs-lookup"><span data-stu-id="8d2a2-110">For an overview of ASP.NET Core, see the [Introduction to ASP.NET Core](xref:index).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8d2a2-111">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="8d2a2-111">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8d2a2-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8d2a2-112">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="8d2a2-113">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8d2a2-113">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="8d2a2-114">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8d2a2-114">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

<a name="rpvs17"></a>

## <a name="create-a-razor-pages-project"></a><span data-ttu-id="8d2a2-115">Razor Pages projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="8d2a2-115">Create a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8d2a2-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8d2a2-116">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="8d2a2-117">Razor Pages projesi oluşturma hakkında ayrıntılı yönergeler için bkz. [Razor Pages kullanmaya başlama](xref:tutorials/razor-pages/razor-pages-start) .</span><span class="sxs-lookup"><span data-stu-id="8d2a2-117">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) for detailed instructions on how to create a Razor Pages project.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="8d2a2-118">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8d2a2-118">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="8d2a2-119">Komut `dotnet new webapp` satırından çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-119">Run `dotnet new webapp` from the command line.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="8d2a2-120">Komut `dotnet new razor` satırından çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-120">Run `dotnet new razor` from the command line.</span></span>

::: moniker-end

<span data-ttu-id="8d2a2-121">Oluşturulan *. csproj* dosyasını Mac için Visual Studio açın.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-121">Open the generated *.csproj* file from Visual Studio for Mac.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="8d2a2-122">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8d2a2-122">Visual Studio Code</span></span>](#tab/visual-studio-code)

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="8d2a2-123">Komut `dotnet new webapp` satırından çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-123">Run `dotnet new webapp` from the command line.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="8d2a2-124">Komut `dotnet new razor` satırından çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-124">Run `dotnet new razor` from the command line.</span></span>

::: moniker-end

---

## <a name="razor-pages"></a><span data-ttu-id="8d2a2-125">Razor Pages</span><span class="sxs-lookup"><span data-stu-id="8d2a2-125">Razor Pages</span></span>

<span data-ttu-id="8d2a2-126">Razor Pages, *Startup.cs*'de etkinleştirilmiştir:</span><span class="sxs-lookup"><span data-stu-id="8d2a2-126">Razor Pages is enabled in *Startup.cs*:</span></span>

[!code-cs[](index/sample/RazorPagesIntro/Startup.cs?name=snippet_Startup)]

<span data-ttu-id="8d2a2-127">Temel bir sayfa düşünün:<a name="OnGet"></a></span><span class="sxs-lookup"><span data-stu-id="8d2a2-127">Consider a basic page: <a name="OnGet"></a></span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index.cshtml)]

<span data-ttu-id="8d2a2-128">Yukarıdaki kod, denetleyiciler ve görünümlerle ASP.NET Core bir uygulamada kullanılan [Razor görünüm dosyası](xref:tutorials/first-mvc-app/adding-view) gibi bir çok şey arar.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-128">The preceding code looks a lot like a [Razor view file](xref:tutorials/first-mvc-app/adding-view) used in an ASP.NET Core app with controllers and views.</span></span> <span data-ttu-id="8d2a2-129">Bu, `@page` farklı kılan yönergedir.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-129">What makes it different is the `@page` directive.</span></span> <span data-ttu-id="8d2a2-130">`@page`dosyayı bir MVC eylemine dönüştürür. Bu, bir denetleyiciden geçmeden istekleri doğrudan işlediği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-130">`@page` makes the file into an MVC action - which means that it handles requests directly, without going through a controller.</span></span> <span data-ttu-id="8d2a2-131">`@page`sayfada ilk Razor yönergesi olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-131">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="8d2a2-132">`@page`diğer Razor yapıları davranışını etkiler.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-132">`@page` affects the behavior of other Razor constructs.</span></span>

<span data-ttu-id="8d2a2-133">Bir `PageModel` sınıf kullanan benzer bir sayfa aşağıdaki iki dosyada gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-133">A similar page, using a `PageModel` class, is shown in the following two files.</span></span> <span data-ttu-id="8d2a2-134">*Pages/Index2. cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="8d2a2-134">The *Pages/Index2.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index2.cshtml)]

<span data-ttu-id="8d2a2-135">*Pages/Index2. cshtml. cs* sayfa modeli:</span><span class="sxs-lookup"><span data-stu-id="8d2a2-135">The *Pages/Index2.cshtml.cs* page model:</span></span>

[!code-cs[](index/sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]

<span data-ttu-id="8d2a2-136">Kurala göre, `PageModel` sınıf dosyası *. cs* eklenmiş Razor sayfası dosyasıyla aynı ada sahiptir.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-136">By convention, the `PageModel` class file has the same name as the Razor Page file with *.cs* appended.</span></span> <span data-ttu-id="8d2a2-137">Örneğin, önceki Razor sayfası *Pages/Index2. cshtml*' dir.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-137">For example, the previous Razor Page is *Pages/Index2.cshtml*.</span></span> <span data-ttu-id="8d2a2-138">`PageModel` Sınıfını içeren dosya *sayfa/Index2. cshtml. cs*olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-138">The file containing the `PageModel` class is named *Pages/Index2.cshtml.cs*.</span></span>

<span data-ttu-id="8d2a2-139">URL yollarının sayfalara olan ilişkilendirmeleri, sayfanın dosya sistemindeki konumuna göre belirlenir.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-139">The associations of URL paths to pages are determined by the page's location in the file system.</span></span> <span data-ttu-id="8d2a2-140">Aşağıdaki tabloda bir Razor sayfa yolu ve eşleşen URL gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="8d2a2-140">The following table shows a Razor Page path and the matching URL:</span></span>

| <span data-ttu-id="8d2a2-141">Dosya adı ve yolu</span><span class="sxs-lookup"><span data-stu-id="8d2a2-141">File name and path</span></span>               | <span data-ttu-id="8d2a2-142">eşleşen URL</span><span class="sxs-lookup"><span data-stu-id="8d2a2-142">matching URL</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="8d2a2-143">*/Pages/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="8d2a2-143">*/Pages/Index.cshtml*</span></span> | <span data-ttu-id="8d2a2-144">`/` veya `/Index`</span><span class="sxs-lookup"><span data-stu-id="8d2a2-144">`/` or `/Index`</span></span> |
| <span data-ttu-id="8d2a2-145">*/Pages/Contact.exe*</span><span class="sxs-lookup"><span data-stu-id="8d2a2-145">*/Pages/Contact.cshtml*</span></span> | `/Contact` |
| <span data-ttu-id="8d2a2-146">*/Pages/Store/Contact.exe*</span><span class="sxs-lookup"><span data-stu-id="8d2a2-146">*/Pages/Store/Contact.cshtml*</span></span> | `/Store/Contact` |
| <span data-ttu-id="8d2a2-147">*/Pages/Store/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="8d2a2-147">*/Pages/Store/Index.cshtml*</span></span> | <span data-ttu-id="8d2a2-148">`/Store` veya `/Store/Index`</span><span class="sxs-lookup"><span data-stu-id="8d2a2-148">`/Store` or `/Store/Index`</span></span> |

<span data-ttu-id="8d2a2-149">Notlar:</span><span class="sxs-lookup"><span data-stu-id="8d2a2-149">Notes:</span></span>

* <span data-ttu-id="8d2a2-150">Çalışma zamanı, *Sayfalar* klasöründeki Razor Pages dosyaları varsayılan olarak arar.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-150">The runtime looks for Razor Pages files in the *Pages* folder by default.</span></span>
* <span data-ttu-id="8d2a2-151">`Index`, URL bir sayfa içermiyorsa varsayılan sayfasıdır.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-151">`Index` is the default page when a URL doesn't include a page.</span></span>

## <a name="write-a-basic-form"></a><span data-ttu-id="8d2a2-152">Temel form yazma</span><span class="sxs-lookup"><span data-stu-id="8d2a2-152">Write a basic form</span></span>

<span data-ttu-id="8d2a2-153">Razor Pages, Web tarayıcıları ile kullanılan ortak desenleri bir uygulama oluştururken kolayca uygulanması için tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-153">Razor Pages is designed to make common patterns used with web browsers easy to implement when building an app.</span></span> <span data-ttu-id="8d2a2-154">[Model bağlama](xref:mvc/models/model-binding), [ETIKET yardımcıları](xref:mvc/views/tag-helpers/intro)ve HTML Yardımcıları hepsi, Razor sayfası sınıfında tanımlanan özelliklerle *çalışır* .</span><span class="sxs-lookup"><span data-stu-id="8d2a2-154">[Model binding](xref:mvc/models/model-binding), [Tag Helpers](xref:mvc/views/tag-helpers/intro), and HTML helpers all *just work* with the properties defined in a Razor Page class.</span></span> <span data-ttu-id="8d2a2-155">`Contact` Model için temel bir "bize başvurun" formu uygulayan bir sayfa düşünün:</span><span class="sxs-lookup"><span data-stu-id="8d2a2-155">Consider a page that implements a basic "contact us" form for the `Contact` model:</span></span>

<span data-ttu-id="8d2a2-156">Bu belgedeki `DbContext` örnekler için, [Startup.cs](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) dosyasında başlatılır.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-156">For the samples in this document, the `DbContext` is initialized in the [Startup.cs](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) file.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Startup.cs?highlight=15-16)]

<span data-ttu-id="8d2a2-157">Veri modeli:</span><span class="sxs-lookup"><span data-stu-id="8d2a2-157">The data model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Data/Customer.cs)]

<span data-ttu-id="8d2a2-158">DB bağlamı:</span><span class="sxs-lookup"><span data-stu-id="8d2a2-158">The db context:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Data/AppDbContext.cs)]

<span data-ttu-id="8d2a2-159">*Pages/Create. cshtml* görünüm dosyası:</span><span class="sxs-lookup"><span data-stu-id="8d2a2-159">The *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml)]

<span data-ttu-id="8d2a2-160">*Pages/Create. cshtml. cs* sayfa modeli:</span><span class="sxs-lookup"><span data-stu-id="8d2a2-160">The *Pages/Create.cshtml.cs* page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_ALL)]

<span data-ttu-id="8d2a2-161">Kuralına göre, `PageModel` sınıfı çağrılır `<PageName>Model` ve sayfayla aynı ad alanında bulunur.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-161">By convention, the `PageModel` class is called `<PageName>Model` and is in the same namespace as the page.</span></span>

<span data-ttu-id="8d2a2-162">`PageModel` Sınıfı, bir sayfanın mantığının sunumuna ayrılmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-162">The `PageModel` class allows separation of the logic of a page from its presentation.</span></span> <span data-ttu-id="8d2a2-163">Sayfaya gönderilen istekler için sayfa işleyicilerini ve sayfayı işlemek için kullanılan verileri tanımlar.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-163">It defines page handlers for requests sent to the page and the data used to render the page.</span></span> <span data-ttu-id="8d2a2-164">Bu ayrım, [bağımlılık ekleme](xref:fundamentals/dependency-injection) ve sayfaların [birim testi](xref:test/razor-pages-tests) aracılığıyla sayfa bağımlılıklarını yönetmenizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-164">This separation allows you to manage page dependencies through [dependency injection](xref:fundamentals/dependency-injection) and to [unit test](xref:test/razor-pages-tests) the pages.</span></span>

<span data-ttu-id="8d2a2-165">Sayfada, istekler üzerinde `OnPostAsync` `POST` çalışan bir *işleyici yöntemi*vardır (bir Kullanıcı formu gönderdiğinde).</span><span class="sxs-lookup"><span data-stu-id="8d2a2-165">The page has an `OnPostAsync` *handler method*, which runs on `POST` requests (when a user posts the form).</span></span> <span data-ttu-id="8d2a2-166">Herhangi bir HTTP fiili için işleyici yöntemleri ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-166">You can add handler methods for any HTTP verb.</span></span> <span data-ttu-id="8d2a2-167">En yaygın işleyiciler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="8d2a2-167">The most common handlers are:</span></span>

* <span data-ttu-id="8d2a2-168">`OnGet`sayfa için gereken durumu başlatmak için.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-168">`OnGet` to initialize state needed for the page.</span></span> <span data-ttu-id="8d2a2-169">[OnGet](#OnGet) örneği.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-169">[OnGet](#OnGet) sample.</span></span>
* <span data-ttu-id="8d2a2-170">`OnPost`form gönderilerini işlemek için.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-170">`OnPost` to handle form submissions.</span></span>

<span data-ttu-id="8d2a2-171">`Async` Adlandırma son eki isteğe bağlıdır, ancak genellikle zaman uyumsuz işlevler için kural tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-171">The `Async` naming suffix is optional but is often used by convention for asynchronous functions.</span></span> <span data-ttu-id="8d2a2-172">Yukarıdaki `OnPostAsync` örnekteki kod, normalde bir denetleyicide yazdıklarınız ile benzer şekilde görünür.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-172">The `OnPostAsync` code in the preceding example looks similar to what you would normally write in a controller.</span></span> <span data-ttu-id="8d2a2-173">Yukarıdaki kod Razor Pages için tipik bir davranıştır.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-173">The preceding code is typical for Razor Pages.</span></span> <span data-ttu-id="8d2a2-174">[Model bağlama](xref:mvc/models/model-binding), [doğrulama](xref:mvc/models/validation)ve eylem sonuçları gibi mvc temel elemanlarının çoğu paylaşılır.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-174">Most of the MVC primitives like [model binding](xref:mvc/models/model-binding), [validation](xref:mvc/models/validation), and action results are shared.</span></span>  <!-- Review: Ryan, can we get a list of what is shared and what isn't? -->

<span data-ttu-id="8d2a2-175">Önceki `OnPostAsync` Yöntem:</span><span class="sxs-lookup"><span data-stu-id="8d2a2-175">The previous `OnPostAsync` method:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="8d2a2-176">Temel akışı `OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="8d2a2-176">The basic flow of `OnPostAsync`:</span></span>

<span data-ttu-id="8d2a2-177">Doğrulama hatalarını kontrol edin.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-177">Check for validation errors.</span></span>

* <span data-ttu-id="8d2a2-178">Hata yoksa, verileri kaydedin ve yeniden yönlendirin.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-178">If there are no errors, save the data and redirect.</span></span>
* <span data-ttu-id="8d2a2-179">Hatalar varsa, doğrulama iletileriyle sayfayı yeniden görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-179">If there are errors, show the page again with validation messages.</span></span> <span data-ttu-id="8d2a2-180">İstemci tarafı doğrulaması geleneksel ASP.NET Core MVC uygulamalarıyla aynıdır.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-180">Client-side validation is identical to traditional ASP.NET Core MVC applications.</span></span> <span data-ttu-id="8d2a2-181">Çoğu durumda, istemci üzerinde doğrulama hataları algılanır ve sunucuya hiçbir zaman gönderilmez.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-181">In many cases, validation errors would be detected on the client, and never submitted to the server.</span></span>

<span data-ttu-id="8d2a2-182">Veriler başarıyla girildiğinde, `OnPostAsync` işleyici yöntemi bir örneğini döndürmek `RedirectToPageResult`için `RedirectToPage` yardımcı yöntemini çağırır.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-182">When the data is entered successfully, the `OnPostAsync` handler method calls the `RedirectToPage` helper method to return an instance of `RedirectToPageResult`.</span></span> <span data-ttu-id="8d2a2-183">`RedirectToPage`, `RedirectToAction` veya`RedirectToRoute`' a benzer ancak sayfalara özelleştirilmiş yeni bir eylem sonucudur.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-183">`RedirectToPage` is a new action result, similar to `RedirectToAction` or `RedirectToRoute`, but customized for pages.</span></span> <span data-ttu-id="8d2a2-184">Yukarıdaki örnekte, kök dizin sayfasına (`/Index`) yeniden yönlendiriliyor.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-184">In the preceding sample, it redirects to the root Index page (`/Index`).</span></span> <span data-ttu-id="8d2a2-185">`RedirectToPage`, [Sayfalar Için URL oluşturma](#url_gen) bölümünde ayrıntılı olarak açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-185">`RedirectToPage` is detailed in the [URL generation for Pages](#url_gen) section.</span></span>

<span data-ttu-id="8d2a2-186">Gönderilen formda doğrulama hataları olduğunda (sunucuya geçirilen),`OnPostAsync` işleyici yöntemi `Page` yardımcı yöntemini çağırır.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-186">When the submitted form has validation errors (that are passed to the server), the`OnPostAsync` handler method calls the `Page` helper method.</span></span> <span data-ttu-id="8d2a2-187">`Page`bir örneğini `PageResult`döndürür.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-187">`Page` returns an instance of `PageResult`.</span></span> <span data-ttu-id="8d2a2-188">Döndürme `Page` , denetleyicilerde eylemlerin nasıl dönüşlerine `View`benzer.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-188">Returning `Page` is similar to how actions in controllers return `View`.</span></span> <span data-ttu-id="8d2a2-189">`PageResult`Varsayılan değer</span><span class="sxs-lookup"><span data-stu-id="8d2a2-189">`PageResult` is the default</span></span> <!-- Review  --> <span data-ttu-id="8d2a2-190">işleyici yöntemi için dönüş türü.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-190">return type for a handler method.</span></span> <span data-ttu-id="8d2a2-191">Döndüren `void` bir işleyici yöntemi sayfayı işler.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-191">A handler method that returns `void` renders the page.</span></span>

<span data-ttu-id="8d2a2-192">Özelliği model bağlamayı kabul etmek için özniteliğini kullanır `[BindProperty]`. `Customer`</span><span class="sxs-lookup"><span data-stu-id="8d2a2-192">The `Customer` property uses `[BindProperty]` attribute to opt in to model binding.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_PageModel&highlight=10-11)]

<span data-ttu-id="8d2a2-193">Razor Pages, varsayılan olarak yalnızca`GET` fiiller olmayan özellikleri bağlayın.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-193">Razor Pages, by default, bind properties only with non-`GET` verbs.</span></span> <span data-ttu-id="8d2a2-194">Özelliklere bağlama, yazmanız gerektiğini kodun miktarını azaltabilir.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-194">Binding to properties can reduce the amount of code you have to write.</span></span> <span data-ttu-id="8d2a2-195">Bağlama, form alanlarını işlemek için aynı özelliği kullanarak kodu azaltır (`<input asp-for="Customer.Name">`) ve girişi kabul eder.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-195">Binding reduces code by using the same property to render form fields (`<input asp-for="Customer.Name">`) and accept the input.</span></span>

[!INCLUDE[](~/includes/bind-get.md)]

<span data-ttu-id="8d2a2-196">Giriş sayfası (*Index. cshtml*):</span><span class="sxs-lookup"><span data-stu-id="8d2a2-196">The home page (*Index.cshtml*):</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml)]

<span data-ttu-id="8d2a2-197">İlişkili `PageModel` Sınıf (*Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="8d2a2-197">The associated `PageModel` class (*Index.cshtml.cs*):</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs)]

<span data-ttu-id="8d2a2-198">*Index. cshtml* dosyası her kişi için bir düzenleme bağlantısı oluşturmak üzere aşağıdaki biçimlendirmeyi içerir:</span><span class="sxs-lookup"><span data-stu-id="8d2a2-198">The *Index.cshtml* file contains the following markup to create an edit link for each contact:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=21)]

<span data-ttu-id="8d2a2-199">[Tutturucu etiketi Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) , düzenleme sayfasına `asp-route-{value}` bir bağlantı oluşturmak için özniteliğini kullandı.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-199">The [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) used the `asp-route-{value}` attribute to generate a link to the Edit page.</span></span> <span data-ttu-id="8d2a2-200">Bağlantı, iletişim KIMLIĞINE sahip rota verileri içerir.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-200">The link contains route data with the contact ID.</span></span> <span data-ttu-id="8d2a2-201">Örneğin: `http://localhost:5000/Edit/1`.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-201">For example, `http://localhost:5000/Edit/1`.</span></span> <span data-ttu-id="8d2a2-202">Bir alanı belirtmek için özniteliğinikullanın.`asp-area`</span><span class="sxs-lookup"><span data-stu-id="8d2a2-202">Use the `asp-area` attribute to specify an area.</span></span> <span data-ttu-id="8d2a2-203">Daha fazla bilgi için bkz. <xref:mvc/controllers/areas>.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-203">For more information, see <xref:mvc/controllers/areas>.</span></span>

<span data-ttu-id="8d2a2-204">*Pages/Edit. cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="8d2a2-204">The *Pages/Edit.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Edit.cshtml?highlight=1)]

<span data-ttu-id="8d2a2-205">İlk satır `@page "{id:int}"` yönergesini içerir.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-205">The first line contains the `@page "{id:int}"` directive.</span></span> <span data-ttu-id="8d2a2-206">Yönlendirme kısıtlaması`"{id:int}"` , sayfada `int` yönlendirme verileri içeren sayfaya istekleri kabul etmesini söyler.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-206">The routing constraint`"{id:int}"` tells the page to accept requests to the page that contain `int` route data.</span></span> <span data-ttu-id="8d2a2-207">Sayfaya yapılan bir istek öğesine `int`dönüştürülebileceği rota verileri içermiyorsa, çalışma zamanı bir HTTP 404 (bulunamadı) hatası döndürür.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-207">If a request to the page doesn't contain route data that can be converted to an `int`, the runtime returns an HTTP 404 (not found) error.</span></span> <span data-ttu-id="8d2a2-208">Kimliği isteğe bağlı yapmak için yol kısıtlamasına `?` ekleyin:</span><span class="sxs-lookup"><span data-stu-id="8d2a2-208">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="8d2a2-209">*Pages/Edit. cshtml. cs* dosyası:</span><span class="sxs-lookup"><span data-stu-id="8d2a2-209">The *Pages/Edit.cshtml.cs* file:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Edit.cshtml.cs)]

<span data-ttu-id="8d2a2-210">*Index. cshtml* dosyası, her müşteri kişisi için bir silme düğmesi oluşturmak için de biçimlendirme içerir:</span><span class="sxs-lookup"><span data-stu-id="8d2a2-210">The *Index.cshtml* file also contains markup to create a delete button for each customer contact:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=22-23)]

<span data-ttu-id="8d2a2-211">Sil düğmesi HTML biçiminde işlendiğinde, `formaction` için parametreleri içerir:</span><span class="sxs-lookup"><span data-stu-id="8d2a2-211">When the delete button is rendered in HTML, its `formaction` includes parameters for:</span></span>

* <span data-ttu-id="8d2a2-212">`asp-route-id` Özniteliği tarafından belirtilen müşteri iletişim kimliği.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-212">The customer contact ID specified by the `asp-route-id` attribute.</span></span>
* <span data-ttu-id="8d2a2-213">Özniteliği tarafından belirtilen `handler` `asp-page-handler` .</span><span class="sxs-lookup"><span data-stu-id="8d2a2-213">The `handler` specified by the `asp-page-handler` attribute.</span></span>

<span data-ttu-id="8d2a2-214">Aşağıda, müşteri irtibat KIMLIĞIYLE `1`birlikte işlenmiş bir Delete düğmesine örnek verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="8d2a2-214">Here is an example of a rendered delete button with a customer contact ID of `1`:</span></span>

```html
<button type="submit" formaction="/?id=1&amp;handler=delete">delete</button>
```

<span data-ttu-id="8d2a2-215">Düğme seçildiğinde, sunucuya bir form `POST` isteği gönderilir.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-215">When the button is selected, a form `POST` request is sent to the server.</span></span> <span data-ttu-id="8d2a2-216">Kurala göre, işleyici yönteminin adı, şemaya `handler` `OnPost[handler]Async`göre parametrenin değerine göre seçilir.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-216">By convention, the name of the handler method is selected based on the value of the `handler` parameter according to the scheme `OnPost[handler]Async`.</span></span>

<span data-ttu-id="8d2a2-217">Bu örnekte olduğundan ,`POST` isteği işlemek için işleyiciyöntemikullanılır`OnPostDeleteAsync`. `handler` `delete`</span><span class="sxs-lookup"><span data-stu-id="8d2a2-217">Because the `handler` is `delete` in this example, the `OnPostDeleteAsync` handler method is used to process the `POST` request.</span></span> <span data-ttu-id="8d2a2-218">, Gibi farklı bir değere `remove`ayarlandıysa, adında `OnPostRemoveAsync` bir işleyici yöntemi seçilir. `asp-page-handler`</span><span class="sxs-lookup"><span data-stu-id="8d2a2-218">If the `asp-page-handler` is set to a different value, such as `remove`, a handler method with the name `OnPostRemoveAsync` is selected.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs?range=26-37)]

<span data-ttu-id="8d2a2-219">`OnPostDeleteAsync` Yöntemi:</span><span class="sxs-lookup"><span data-stu-id="8d2a2-219">The `OnPostDeleteAsync` method:</span></span>

* <span data-ttu-id="8d2a2-220">`id` Sorgu dizesinden öğesini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-220">Accepts the `id` from the query string.</span></span>
* <span data-ttu-id="8d2a2-221">Müşteri iletişim `FindAsync`için veritabanını sorgular.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-221">Queries the database for the customer contact with `FindAsync`.</span></span>
* <span data-ttu-id="8d2a2-222">Müşteri ilgili kişisi bulunursa, bunlar müşteri kişileri listesinden kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-222">If the customer contact is found, they're removed from the list of customer contacts.</span></span> <span data-ttu-id="8d2a2-223">Veritabanı güncelleştirildi.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-223">The database is updated.</span></span>
* <span data-ttu-id="8d2a2-224">Kök `RedirectToPage` dizin sayfasına (`/Index`) yeniden yönlendirmek için çağrılar.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-224">Calls `RedirectToPage` to redirect to the root Index page (`/Index`).</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="mark-page-properties-as-required"></a><span data-ttu-id="8d2a2-225">Sayfa özelliklerini gerektiği gibi işaretle</span><span class="sxs-lookup"><span data-stu-id="8d2a2-225">Mark page properties as required</span></span>

<span data-ttu-id="8d2a2-226">Bir `PageModel` üzerindeki Özellikler [gerekli](/dotnet/api/system.componentmodel.dataannotations.requiredattribute) öznitelikle birlikte kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="8d2a2-226">Properties on a `PageModel` can be decorated with the [Required](/dotnet/api/system.componentmodel.dataannotations.requiredattribute) attribute:</span></span>

[!code-cs[](index/sample/Create.cshtml.cs?highlight=3,15-16)]

<span data-ttu-id="8d2a2-227">Daha fazla bilgi için bkz. [model doğrulaması](xref:mvc/models/validation).</span><span class="sxs-lookup"><span data-stu-id="8d2a2-227">For more information, see [Model validation](xref:mvc/models/validation).</span></span>

## <a name="handle-head-requests-with-an-onget-handler-fallback"></a><span data-ttu-id="8d2a2-228">OnGet işleyicisi geri dönüşü ile tanıtıcı HEAD istekleri</span><span class="sxs-lookup"><span data-stu-id="8d2a2-228">Handle HEAD requests with an OnGet handler fallback</span></span>

<span data-ttu-id="8d2a2-229">`HEAD`istekleri belirli bir kaynak için üstbilgileri almanıza izin verir.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-229">`HEAD` requests allow you to retrieve the headers for a specific resource.</span></span> <span data-ttu-id="8d2a2-230">İsteklerin aksine `GET`isteklerbiryanıtgövdesi döndürmez.`HEAD`</span><span class="sxs-lookup"><span data-stu-id="8d2a2-230">Unlike `GET` requests, `HEAD` requests don't return a response body.</span></span>

<span data-ttu-id="8d2a2-231">Normalde, istekler `OnHead` için `HEAD` bir işleyici oluşturulur ve çağırılır:</span><span class="sxs-lookup"><span data-stu-id="8d2a2-231">Ordinarily, an `OnHead` handler is created and called for `HEAD` requests:</span></span> 

```csharp
public void OnHead()
{
    HttpContext.Response.Headers.Add("HandledBy", "Handled by OnHead!");
}
```

<span data-ttu-id="8d2a2-232">ASP.NET Core 2,1 veya sonraki bir sürümde, hiçbir `OnGet` `OnHead` işleyici tanımlanmazsa, Razor Pages işleyiciyi çağırmaya geri döner.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-232">In ASP.NET Core 2.1 or later, Razor Pages falls back to calling the `OnGet` handler if no `OnHead` handler is defined.</span></span> <span data-ttu-id="8d2a2-233">Bu davranış, içindeki `Startup.ConfigureServices` [setcompatibilityversion](xref:mvc/compatibility-version) çağrısıyla etkinleştirilir:</span><span class="sxs-lookup"><span data-stu-id="8d2a2-233">This behavior is enabled by the call to [SetCompatibilityVersion](xref:mvc/compatibility-version) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddMvc()
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_1);
```

<span data-ttu-id="8d2a2-234">Varsayılan Şablonlar ASP.NET Core 2,1 ve `SetCompatibilityVersion` 2,2 ' de çağrıyı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-234">The default templates generate the `SetCompatibilityVersion` call in ASP.NET Core 2.1 and 2.2.</span></span> <span data-ttu-id="8d2a2-235">`SetCompatibilityVersion`Razor Pages seçeneğini `AllowMappingHeadRequestsToGetHandler` etkin bir şekilde `true`ayarlar.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-235">`SetCompatibilityVersion` effectively sets the Razor Pages option `AllowMappingHeadRequestsToGetHandler` to `true`.</span></span>

<span data-ttu-id="8d2a2-236">İle `SetCompatibilityVersion`tüm davranışlardan çıkmak yerine, açıkça *belirli* davranışları kabul edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-236">Rather than opting in to all behaviors with `SetCompatibilityVersion`, you can explicitly opt in to *specific* behaviors.</span></span> <span data-ttu-id="8d2a2-237">Aşağıdaki kod, isteklerin `HEAD` `OnGet` işleyiciye eşlenmesine izin vermek için ' de kullanılır:</span><span class="sxs-lookup"><span data-stu-id="8d2a2-237">The following code opts in to allowing `HEAD` requests to be mapped to the `OnGet` handler:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        options.AllowMappingHeadRequestsToGetHandler = true;
    });
```

::: moniker-end

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a><span data-ttu-id="8d2a2-238">XSRF/CSRF ve Razor Pages</span><span class="sxs-lookup"><span data-stu-id="8d2a2-238">XSRF/CSRF and Razor Pages</span></span>

<span data-ttu-id="8d2a2-239">[Antiforgery doğrulaması](xref:security/anti-request-forgery)için herhangi bir kod yazmanız gerekmez.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-239">You don't have to write any code for [antiforgery validation](xref:security/anti-request-forgery).</span></span> <span data-ttu-id="8d2a2-240">Antiforgery belirteci oluşturma ve doğrulama, Razor Pages otomatik olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-240">Antiforgery token generation and validation are automatically included in Razor Pages.</span></span>

<a name="layout"></a>

## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a><span data-ttu-id="8d2a2-241">Razor Pages ile düzenleri, partileri, şablonları ve etiket yardımcılarını kullanma</span><span class="sxs-lookup"><span data-stu-id="8d2a2-241">Using Layouts, partials, templates, and Tag Helpers with Razor Pages</span></span>

<span data-ttu-id="8d2a2-242">Sayfalar, Razor görünüm altyapısının tüm özellikleri ile çalışır.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-242">Pages work with all the capabilities of the Razor view engine.</span></span> <span data-ttu-id="8d2a2-243">Düzenler, partıals, şablonlar, etiket yardımcıları, *_Viewstart. cshtml*, *_viewwimports. cshtml* geleneksel Razor görünümlerinde oldukları gibi çalışır.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-243">Layouts, partials, templates, Tag Helpers, *_ViewStart.cshtml*, *_ViewImports.cshtml* work in the same way they do for conventional Razor views.</span></span>

<span data-ttu-id="8d2a2-244">Bu özelliklerden bazılarının avantajlarından yararlanarak bu sayfayı declutter edelim.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-244">Let's declutter this page by taking advantage of some of those capabilities.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="8d2a2-245">*Pages/Shared/_Layout. cshtml*öğesine bir [Düzen sayfası](xref:mvc/views/layout) ekleyin:</span><span class="sxs-lookup"><span data-stu-id="8d2a2-245">Add a [layout page](xref:mvc/views/layout) to *Pages/Shared/_Layout.cshtml*:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="8d2a2-246">Sayfalara [Düzen sayfası](xref:mvc/views/layout) Ekle */_Layout. cshtml*:</span><span class="sxs-lookup"><span data-stu-id="8d2a2-246">Add a [layout page](xref:mvc/views/layout) to *Pages/_Layout.cshtml*:</span></span>

::: moniker-end

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_LayoutSimple.cshtml)]

<span data-ttu-id="8d2a2-247">[Düzen](xref:mvc/views/layout):</span><span class="sxs-lookup"><span data-stu-id="8d2a2-247">The [Layout](xref:mvc/views/layout):</span></span>

* <span data-ttu-id="8d2a2-248">Her sayfanın yerleşimini denetler (sayfa düzen dışında değilse).</span><span class="sxs-lookup"><span data-stu-id="8d2a2-248">Controls the layout of each page (unless the page opts out of layout).</span></span>
* <span data-ttu-id="8d2a2-249">JavaScript ve stil sayfaları gibi HTML yapılarını içeri aktarır.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-249">Imports HTML structures such as JavaScript and stylesheets.</span></span>

<span data-ttu-id="8d2a2-250">Daha fazla bilgi için bkz. [Düzen sayfası](xref:mvc/views/layout) .</span><span class="sxs-lookup"><span data-stu-id="8d2a2-250">See [layout page](xref:mvc/views/layout) for more information.</span></span>

<span data-ttu-id="8d2a2-251">[Layout](xref:mvc/views/layout#specifying-a-layout) özelliği *Pages/_viewstart. cshtml*içinde ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="8d2a2-251">The [Layout](xref:mvc/views/layout#specifying-a-layout) property is set in *Pages/_ViewStart.cshtml*:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="8d2a2-252">Düzen *Sayfalar/paylaşılan* klasöründedir.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-252">The layout is in the *Pages/Shared* folder.</span></span> <span data-ttu-id="8d2a2-253">Sayfalar, geçerli sayfayla aynı klasörden başlayarak diğer görünümleri (düzenler, şablonlar, parals) hiyerarşik olarak arar.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-253">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="8d2a2-254">*Sayfalar/paylaşılan* klasördeki bir düzen, *Sayfalar* klasörü altındaki herhangi bir Razor sayfasından kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-254">A layout in the *Pages/Shared* folder can be used from any Razor page under the *Pages* folder.</span></span>

<span data-ttu-id="8d2a2-255">Düzen dosyası *Sayfalar/paylaşılan* klasörüne gitmelidir.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-255">The layout file should go in the *Pages/Shared* folder.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="8d2a2-256">Düzen *Sayfalar* klasöründedir.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-256">The layout is in the *Pages* folder.</span></span> <span data-ttu-id="8d2a2-257">Sayfalar, geçerli sayfayla aynı klasörden başlayarak diğer görünümleri (düzenler, şablonlar, parals) hiyerarşik olarak arar.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-257">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="8d2a2-258">*Sayfalar* klasöründeki bir düzen, *Sayfalar* klasörü altındaki herhangi bir Razor sayfasından kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-258">A layout in the *Pages* folder can be used from any Razor page under the *Pages* folder.</span></span>

::: moniker-end

<span data-ttu-id="8d2a2-259">Düzen dosyasını *Görünümler/paylaşılan* klasöre **yerleştirmenizi öneririz** .</span><span class="sxs-lookup"><span data-stu-id="8d2a2-259">We recommend you **not** put the layout file in the *Views/Shared* folder.</span></span> <span data-ttu-id="8d2a2-260">*Görünümler/paylaşılan* bir MVC görünümleri modelidir.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-260">*Views/Shared* is an MVC views pattern.</span></span> <span data-ttu-id="8d2a2-261">Razor Pages, yol kurallarını değil klasör hiyerarşisine güvenmektir.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-261">Razor Pages are meant to rely on folder hierarchy, not path conventions.</span></span>

<span data-ttu-id="8d2a2-262">Bir Razor sayfasından arama görüntüleme, *Sayfalar* klasörünü içerir.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-262">View search from a Razor Page includes the *Pages* folder.</span></span> <span data-ttu-id="8d2a2-263">MVC denetleyicileri ve geleneksel Razor görünümleriyle kullandığınız düzenler, şablonlar ve parals işlemleri *yalnızca çalışır*.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-263">The layouts, templates, and partials you're using with MVC controllers and conventional Razor views *just work*.</span></span>

<span data-ttu-id="8d2a2-264">*Pages/_Viewwimports. cshtml* dosyası ekleyin:</span><span class="sxs-lookup"><span data-stu-id="8d2a2-264">Add a *Pages/_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]

<span data-ttu-id="8d2a2-265">`@namespace`, Öğreticinin ilerleyen kısımlarında açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-265">`@namespace` is explained later in the tutorial.</span></span> <span data-ttu-id="8d2a2-266">Yönergesi, [yerleşik etiket yardımcılarını](xref:mvc/views/tag-helpers/builtin-th/Index) sayfalar klasöründeki tüm sayfalara getirir.  `@addTagHelper`</span><span class="sxs-lookup"><span data-stu-id="8d2a2-266">The `@addTagHelper` directive brings in the [built-in Tag Helpers](xref:mvc/views/tag-helpers/builtin-th/Index) to all the pages in the *Pages* folder.</span></span>

<a name="namespace"></a>

<span data-ttu-id="8d2a2-267">`@namespace` Yönerge bir sayfada açıkça kullanıldığında:</span><span class="sxs-lookup"><span data-stu-id="8d2a2-267">When the `@namespace` directive is used explicitly on a page:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]

<span data-ttu-id="8d2a2-268">Yönergesi sayfanın ad alanını ayarlar.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-268">The directive sets the namespace for the page.</span></span> <span data-ttu-id="8d2a2-269">`@model` Yönergesinin ad alanını içermesi gerekmez.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-269">The `@model` directive doesn't need to include the namespace.</span></span>

<span data-ttu-id="8d2a2-270">Yönerge _viewwimports *. cshtml*içinde yer aldığında, belirtilen ad alanı, `@namespace` yönergeyi içeri aktaran sayfada oluşturulan ad alanı için ön ek sağlar. `@namespace`</span><span class="sxs-lookup"><span data-stu-id="8d2a2-270">When the `@namespace` directive is contained in *_ViewImports.cshtml*, the specified namespace supplies the prefix for the generated namespace in the Page that imports the `@namespace` directive.</span></span> <span data-ttu-id="8d2a2-271">Oluşturulan ad alanının geri kalanı (sonek bölümü), *_Viewwimports. cshtml* dosyasını ve sayfayı içeren klasörü içeren, noktayla ayrılmış göreli yoldur.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-271">The rest of the generated namespace (the suffix portion) is the dot-separated relative path between the folder containing *_ViewImports.cshtml* and the folder containing the page.</span></span>

<span data-ttu-id="8d2a2-272">Örneğin, `PageModel` *Pages/Customers/Edit. cshtml. cs* sınıfı, ad alanını açıkça ayarlar:</span><span class="sxs-lookup"><span data-stu-id="8d2a2-272">For example, the `PageModel` class *Pages/Customers/Edit.cshtml.cs* explicitly sets the namespace:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=snippet_namespace)]

<span data-ttu-id="8d2a2-273">*Pages/_Viewwimports. cshtml* dosyası aşağıdaki ad alanını ayarlar:</span><span class="sxs-lookup"><span data-stu-id="8d2a2-273">The *Pages/_ViewImports.cshtml* file sets the following namespace:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]

<span data-ttu-id="8d2a2-274">*Pages/Customers/Edit. cshtml* Razor sayfasının oluşturulan ad alanı `PageModel` sınıfıyla aynıdır.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-274">The generated namespace for the *Pages/Customers/Edit.cshtml* Razor Page is the same as the `PageModel` class.</span></span>

<span data-ttu-id="8d2a2-275">`@namespace`*Ayrıca geleneksel Razor görünümleriyle birlikte da geçerlidir.*</span><span class="sxs-lookup"><span data-stu-id="8d2a2-275">`@namespace` *also works with conventional Razor views.*</span></span>

<span data-ttu-id="8d2a2-276">Özgün *Sayfalar/Create. cshtml* görünüm dosyası:</span><span class="sxs-lookup"><span data-stu-id="8d2a2-276">The original *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml?highlight=2)]

<span data-ttu-id="8d2a2-277">Güncelleştirilmiş *Sayfalar/Create. cshtml* görünüm dosyası:</span><span class="sxs-lookup"><span data-stu-id="8d2a2-277">The updated *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/Create.cshtml?highlight=2)]

<span data-ttu-id="8d2a2-278">[Razor Pages Başlatıcı projesi](#rpvs17) , istemci tarafı doğrulamayı bağlayan *sayfaları/_ValidationScriptsPartial. cshtml*'yi içerir.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-278">The [Razor Pages starter project](#rpvs17) contains the *Pages/_ValidationScriptsPartial.cshtml*, which hooks up client-side validation.</span></span>

<span data-ttu-id="8d2a2-279">Kısmi görünümler hakkında daha fazla bilgi için bkz <xref:mvc/views/partial>.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-279">For more information on partial views, see <xref:mvc/views/partial>.</span></span>

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a><span data-ttu-id="8d2a2-280">Sayfalar için URL oluşturma</span><span class="sxs-lookup"><span data-stu-id="8d2a2-280">URL generation for Pages</span></span>

<span data-ttu-id="8d2a2-281">Daha önce gösterilen `RedirectToPage` `Create` sayfa şunları kullanır:</span><span class="sxs-lookup"><span data-stu-id="8d2a2-281">The `Create` page, shown previously, uses `RedirectToPage`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=10)]

<span data-ttu-id="8d2a2-282">Uygulama aşağıdaki dosya/klasör yapısına sahiptir:</span><span class="sxs-lookup"><span data-stu-id="8d2a2-282">The app has the following file/folder structure:</span></span>

* <span data-ttu-id="8d2a2-283">*/Pages*</span><span class="sxs-lookup"><span data-stu-id="8d2a2-283">*/Pages*</span></span>

  * <span data-ttu-id="8d2a2-284">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="8d2a2-284">*Index.cshtml*</span></span>
  * <span data-ttu-id="8d2a2-285">*/Customers*</span><span class="sxs-lookup"><span data-stu-id="8d2a2-285">*/Customers*</span></span>

    * <span data-ttu-id="8d2a2-286">*. Cshtml oluştur*</span><span class="sxs-lookup"><span data-stu-id="8d2a2-286">*Create.cshtml*</span></span>
    * <span data-ttu-id="8d2a2-287">*Edit.cshtml*</span><span class="sxs-lookup"><span data-stu-id="8d2a2-287">*Edit.cshtml*</span></span>
    * <span data-ttu-id="8d2a2-288">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="8d2a2-288">*Index.cshtml*</span></span>

<span data-ttu-id="8d2a2-289">*Pages/Customers/Create. cshtml* ve *Pages/Customers/Edit. cshtml* sayfaları, başarılı olduktan sonra *Pages/Index. cshtml* dosyasına yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-289">The *Pages/Customers/Create.cshtml* and *Pages/Customers/Edit.cshtml* pages redirect to *Pages/Index.cshtml* after success.</span></span> <span data-ttu-id="8d2a2-290">Dize `/Index` , önceki sayfaya erişmek için URI 'nin bir parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-290">The string `/Index` is part of the URI to access the preceding page.</span></span> <span data-ttu-id="8d2a2-291">Dize `/Index` , *Sayfalar/Index. cshtml* sayfasına URI 'ler oluşturmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-291">The string `/Index` can be used to generate URIs to the *Pages/Index.cshtml* page.</span></span> <span data-ttu-id="8d2a2-292">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="8d2a2-292">For example:</span></span>

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">My Index Page</a>`
* `RedirectToPage("/Index")`

<span data-ttu-id="8d2a2-293">Sayfa adı, kök */Pages* klasöründeki sayfanın başında `/` (örneğin, `/Index`) bir yoldur.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-293">The page name is the path to the page from the root */Pages* folder including a leading `/` (for example, `/Index`).</span></span> <span data-ttu-id="8d2a2-294">Önceki URL oluşturma örnekleri bir URL 'YI kodlamadan gelişmiş seçenekler ve işlevsel yetenekler sunar.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-294">The preceding URL generation samples offer enhanced options and functional capabilities over hardcoding a URL.</span></span> <span data-ttu-id="8d2a2-295">URL oluşturma [yönlendirme](xref:mvc/controllers/routing) kullanır ve yolun hedef yolda nasıl tanımlandığınıza göre parametreleri oluşturabilir ve kodlayabilir.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-295">URL generation uses [routing](xref:mvc/controllers/routing) and can generate and encode parameters according to how the route is defined in the destination path.</span></span>

<span data-ttu-id="8d2a2-296">Sayfalar için URL oluşturma göreli adları destekler.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-296">URL generation for pages supports relative names.</span></span> <span data-ttu-id="8d2a2-297">Aşağıdaki tabloda, *sayfa/müşteri/oluşturma. cshtml*'den `RedirectToPage` farklı parametrelerle hangi dizin sayfasının seçildiği gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="8d2a2-297">The following table shows which Index page is selected with different `RedirectToPage` parameters from *Pages/Customers/Create.cshtml*:</span></span>

| <span data-ttu-id="8d2a2-298">RedirectToPage (x)</span><span class="sxs-lookup"><span data-stu-id="8d2a2-298">RedirectToPage(x)</span></span>| <span data-ttu-id="8d2a2-299">Sayfasında</span><span class="sxs-lookup"><span data-stu-id="8d2a2-299">Page</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="8d2a2-300">RedirectToPage ("/Index")</span><span class="sxs-lookup"><span data-stu-id="8d2a2-300">RedirectToPage("/Index")</span></span> | <span data-ttu-id="8d2a2-301">*Sayfa/dizin*</span><span class="sxs-lookup"><span data-stu-id="8d2a2-301">*Pages/Index*</span></span> |
| <span data-ttu-id="8d2a2-302">RedirectToPage ("./Index");</span><span class="sxs-lookup"><span data-stu-id="8d2a2-302">RedirectToPage("./Index");</span></span> | <span data-ttu-id="8d2a2-303">*Sayfalar/müşteriler/Dizin*</span><span class="sxs-lookup"><span data-stu-id="8d2a2-303">*Pages/Customers/Index*</span></span> |
| <span data-ttu-id="8d2a2-304">RedirectToPage (". /İndex ")</span><span class="sxs-lookup"><span data-stu-id="8d2a2-304">RedirectToPage("../Index")</span></span> | <span data-ttu-id="8d2a2-305">*Sayfa/dizin*</span><span class="sxs-lookup"><span data-stu-id="8d2a2-305">*Pages/Index*</span></span> |
| <span data-ttu-id="8d2a2-306">RedirectToPage ("Dizin")</span><span class="sxs-lookup"><span data-stu-id="8d2a2-306">RedirectToPage("Index")</span></span>  | <span data-ttu-id="8d2a2-307">*Sayfalar/müşteriler/Dizin*</span><span class="sxs-lookup"><span data-stu-id="8d2a2-307">*Pages/Customers/Index*</span></span> |

<span data-ttu-id="8d2a2-308">`RedirectToPage("Index")`, `RedirectToPage("./Index")` ve `RedirectToPage("./Index")` , *göreli adlardır*.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-308">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, and `RedirectToPage("../Index")`  are *relative names*.</span></span> <span data-ttu-id="8d2a2-309">Parametresi, hedef sayfanın adını hesaplamak için geçerli sayfanın yoluyla *birleştirilir.* `RedirectToPage`</span><span class="sxs-lookup"><span data-stu-id="8d2a2-309">The `RedirectToPage` parameter is *combined* with the path of the current page to compute the name of the destination page.</span></span>  <!-- Review: Original had The provided string is combined with the page name of the current page to compute the name of the destination page.  page name, not page path -->

<span data-ttu-id="8d2a2-310">Karmaşık bir yapıya sahip siteler oluştururken göreli ad bağlama yararlı olur.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-310">Relative name linking is useful when building sites with a complex structure.</span></span> <span data-ttu-id="8d2a2-311">Bir klasördeki sayfalar arasında bağlantı sağlamak için göreli adlar kullanırsanız, bu klasörü yeniden adlandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-311">If you use relative names to link between pages in a folder, you can rename that folder.</span></span> <span data-ttu-id="8d2a2-312">Tüm bağlantılar hala çalışır (klasör adını içermediği için).</span><span class="sxs-lookup"><span data-stu-id="8d2a2-312">All the links still work (because they didn't include the folder name).</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="8d2a2-313">Farklı bir [alandaki](xref:mvc/controllers/areas)bir sayfaya yeniden yönlendirmek için alanını belirtin:</span><span class="sxs-lookup"><span data-stu-id="8d2a2-313">To redirect to a page in a different [Area](xref:mvc/controllers/areas), specify the area:</span></span>

```csharp
RedirectToPage("/Index", new { area = "Services" });
```

<span data-ttu-id="8d2a2-314">Daha fazla bilgi için bkz. <xref:mvc/controllers/areas>.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-314">For more information, see <xref:mvc/controllers/areas>.</span></span>

## <a name="viewdata-attribute"></a><span data-ttu-id="8d2a2-315">ViewData özniteliği</span><span class="sxs-lookup"><span data-stu-id="8d2a2-315">ViewData attribute</span></span>

<span data-ttu-id="8d2a2-316">Veri, [Viewdataattribute](/dotnet/api/microsoft.aspnetcore.mvc.viewdataattribute)içeren bir sayfaya geçirilebilir.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-316">Data can be passed to a page with [ViewDataAttribute](/dotnet/api/microsoft.aspnetcore.mvc.viewdataattribute).</span></span> <span data-ttu-id="8d2a2-317">Denetleyiciler veya Razor sayfa modelleriyle birlikte `[ViewData]` düzenlenmiş özellikler, değerlerini, [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary)'den saklı ve yüklenmiş olarak alır.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-317">Properties on controllers or Razor Page models decorated with `[ViewData]` have their values stored and loaded from the [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary).</span></span>

<span data-ttu-id="8d2a2-318">Aşağıdaki örnekte `AboutModel` , ile `[ViewData]`donatılmış bir `Title` özelliği içerir.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-318">In the following example, the `AboutModel` contains a `Title` property decorated with `[ViewData]`.</span></span> <span data-ttu-id="8d2a2-319">`Title` Özelliği hakkında sayfasının başlığına ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="8d2a2-319">The `Title` property is set to the title of the About page:</span></span>

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

<span data-ttu-id="8d2a2-320">Hakkında sayfasında, `Title` özelliğe model özelliği olarak erişin:</span><span class="sxs-lookup"><span data-stu-id="8d2a2-320">In the About page, access the `Title` property as a model property:</span></span>

```cshtml
<h1>@Model.Title</h1>
```

<span data-ttu-id="8d2a2-321">Mizanpajda, başlık ViewData sözlüğünden okundu:</span><span class="sxs-lookup"><span data-stu-id="8d2a2-321">In the layout, the title is read from the ViewData dictionary:</span></span>

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"] - WebApplication</title>
    ...
```

::: moniker-end

## <a name="tempdata"></a><span data-ttu-id="8d2a2-322">TempData</span><span class="sxs-lookup"><span data-stu-id="8d2a2-322">TempData</span></span>

<span data-ttu-id="8d2a2-323">ASP.NET Core bir [denetleyicide](/dotnet/api/microsoft.aspnetcore.mvc.controller) [TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) özelliğini kullanıma sunar.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-323">ASP.NET Core exposes the [TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) property on a [controller](/dotnet/api/microsoft.aspnetcore.mvc.controller).</span></span> <span data-ttu-id="8d2a2-324">Bu özellik, okunana kadar verileri depolar.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-324">This property stores data until it's read.</span></span> <span data-ttu-id="8d2a2-325">`Keep` Ve`Peek` yöntemleri silmeden verileri incelemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-325">The `Keep` and `Peek` methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="8d2a2-326">`TempData`, bir tek istekten daha fazla veri gerektiğinde yeniden yönlendirme için yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-326">`TempData` is  useful for redirection, when data is needed for more than a single request.</span></span>

<span data-ttu-id="8d2a2-327">`[TempData]` Özniteliği ASP.NET Core 2,0 ' de yenidir ve denetleyicilerde ve sayfalarda desteklenir.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-327">The `[TempData]` attribute is new in ASP.NET Core 2.0 and is supported on controllers and pages.</span></span>

<span data-ttu-id="8d2a2-328">Aşağıdaki kod, şunu `Message` kullanarak `TempData`değerini ayarlar:</span><span class="sxs-lookup"><span data-stu-id="8d2a2-328">The following code sets the value of `Message` using `TempData`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,25&name=snippet_Temp)]

<span data-ttu-id="8d2a2-329">*Pages/Customers/Index. cshtml* dosyasında aşağıdaki biçimlendirme, `Message` using `TempData`değerini gösterir.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-329">The following markup in the *Pages/Customers/Index.cshtml* file displays the value of `Message` using `TempData`.</span></span>

```cshtml
<h3>Msg: @Model.Message</h3>
```

<span data-ttu-id="8d2a2-330">*Pages/Customers/Index. cshtml. cs* sayfa modeli, `[TempData]` `Message` özelliğine özniteliğini uygular.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-330">The *Pages/Customers/Index.cshtml.cs* page model applies the `[TempData]` attribute to the `Message` property.</span></span>

```cs
[TempData]
public string Message { get; set; }
```

<span data-ttu-id="8d2a2-331">Daha fazla bilgi için bkz. [TempData](xref:fundamentals/app-state#tempdata) .</span><span class="sxs-lookup"><span data-stu-id="8d2a2-331">For more information, see [TempData](xref:fundamentals/app-state#tempdata) .</span></span>

<a name="mhpp"></a>

## <a name="multiple-handlers-per-page"></a><span data-ttu-id="8d2a2-332">Sayfa başına birden çok işleyici</span><span class="sxs-lookup"><span data-stu-id="8d2a2-332">Multiple handlers per page</span></span>

<span data-ttu-id="8d2a2-333">Aşağıdaki sayfa, `asp-page-handler` etiket Yardımcısını kullanarak iki işleyici için biçimlendirme oluşturur:</span><span class="sxs-lookup"><span data-stu-id="8d2a2-333">The following page generates markup for two handlers using the `asp-page-handler` Tag Helper:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

<!-- Review: the FormActionTagHelper applies to all <form /> elements on a Razor page, even when there's no `asp-` attribute   -->

<span data-ttu-id="8d2a2-334">Yukarıdaki örnekteki formda, her biri farklı bir URL 'ye göndermek `FormActionTagHelper` için kullanan iki gönderme düğmesi vardır.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-334">The form in the preceding example has two submit buttons, each using the `FormActionTagHelper` to submit to a different URL.</span></span> <span data-ttu-id="8d2a2-335">Özniteliği, için `asp-page`bir yardımcı ' dir. `asp-page-handler`</span><span class="sxs-lookup"><span data-stu-id="8d2a2-335">The `asp-page-handler` attribute is a companion to `asp-page`.</span></span> <span data-ttu-id="8d2a2-336">`asp-page-handler`bir sayfa tarafından tanımlanan her bir işleyici yöntemini gönderen URL 'Ler oluşturur.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-336">`asp-page-handler` generates URLs that submit to each of the handler methods defined by a page.</span></span> <span data-ttu-id="8d2a2-337">`asp-page`örnek geçerli sayfaya bağlandığından belirtilmedi.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-337">`asp-page` isn't specified because the sample is linking to the current page.</span></span>

<span data-ttu-id="8d2a2-338">Sayfa modeli:</span><span class="sxs-lookup"><span data-stu-id="8d2a2-338">The page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]

<span data-ttu-id="8d2a2-339">Yukarıdaki kod, *adlandırılmış işleyici yöntemlerini*kullanır.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-339">The preceding code uses *named handler methods*.</span></span> <span data-ttu-id="8d2a2-340">Adlandırılmış işleyici yöntemleri, `On<HTTP Verb>` ve öncesinde `Async` (varsa) ad içindeki metin alınarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-340">Named handler methods are created by taking the text in the name after `On<HTTP Verb>` and before `Async` (if present).</span></span> <span data-ttu-id="8d2a2-341">Yukarıdaki örnekte, Page metotları OnPost**Joinlist**Async ve onpost**Joinlıstuc**Async ' dir.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-341">In the preceding example, the page methods are OnPost**JoinList**Async and OnPost**JoinListUC**Async.</span></span> <span data-ttu-id="8d2a2-342">*Onpost* ile *zaman uyumsuz* olarak kaldırıldığında, işleyici adları ve `JoinList` `JoinListUC`' dir.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-342">With *OnPost* and *Async* removed, the handler names are `JoinList` and `JoinListUC`.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?range=12-13)]

<span data-ttu-id="8d2a2-343">Önceki kodu kullanarak, ' a gönderen `OnPostJoinListAsync` `http://localhost:5000/Customers/CreateFATH?handler=JoinList`URL yolu.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-343">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span></span> <span data-ttu-id="8d2a2-344">' A gönderen `OnPostJoinListUCAsync` `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`URL yolu.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-344">The URL path that submits to `OnPostJoinListUCAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span></span>

## <a name="custom-routes"></a><span data-ttu-id="8d2a2-345">Özel yollar</span><span class="sxs-lookup"><span data-stu-id="8d2a2-345">Custom routes</span></span>

<span data-ttu-id="8d2a2-346">`@page` İçin yönergesini kullanın:</span><span class="sxs-lookup"><span data-stu-id="8d2a2-346">Use the `@page` directive to:</span></span>

* <span data-ttu-id="8d2a2-347">Sayfaya özel bir yol belirtin.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-347">Specify a custom route to a page.</span></span> <span data-ttu-id="8d2a2-348">Örneğin, hakkında sayfasına olan yol ile `/Some/Other/Path` `@page "/Some/Other/Path"`öğesine ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-348">For example, the route to the About page can be set to `/Some/Other/Path` with `@page "/Some/Other/Path"`.</span></span>
* <span data-ttu-id="8d2a2-349">Kesimleri bir sayfanın varsayılan yoluna ekleyin.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-349">Append segments to a page's default route.</span></span> <span data-ttu-id="8d2a2-350">Örneğin, bir "öğe" segmenti sayfanın varsayılan rotasına `@page "item"`eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-350">For example, an "item" segment can be added to a page's default route with `@page "item"`.</span></span>
* <span data-ttu-id="8d2a2-351">Bir sayfanın varsayılan yoluna parametreleri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-351">Append parameters to a page's default route.</span></span> <span data-ttu-id="8d2a2-352">Örneğin, bir ID parametresi `id`, içeren `@page "{id}"`bir sayfa için gerekli olabilir.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-352">For example, an ID parameter, `id`, can be required for a page with `@page "{id}"`.</span></span>

<span data-ttu-id="8d2a2-353">Yolun başındaki bir tilde (`~`) tarafından belirlenen kök göreli bir yol desteklenir.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-353">A root-relative path designated by a tilde (`~`) at the beginning of the path is supported.</span></span> <span data-ttu-id="8d2a2-354">Örneğin, `@page "~/Some/Other/Path"` ile `@page "/Some/Other/Path"`aynıdır.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-354">For example, `@page "~/Some/Other/Path"` is the same as `@page "/Some/Other/Path"`.</span></span>

<span data-ttu-id="8d2a2-355">Yol şablonunu `?handler=JoinList` `/JoinList` belirterek,URL'dekisorgudizesinibirrota`@page "{handler?}"`segmentine dönüştürebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-355">You can change the query string `?handler=JoinList` in the URL to a route segment `/JoinList` by specifying the route template `@page "{handler?}"`.</span></span>

<span data-ttu-id="8d2a2-356">URL 'de sorgu dizesini `?handler=JoinList` beğenmezseniz, yolu URL 'nin yol bölümüne koymak için yolu değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-356">If you don't like the query string `?handler=JoinList` in the URL, you can change the route to put the handler name in the path portion of the URL.</span></span> <span data-ttu-id="8d2a2-357">`@page` Yönergeden sonra çift tırnak içine alınmış bir rota şablonu ekleyerek yolu özelleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-357">You can customize the route by adding a route template enclosed in double quotes after the `@page` directive.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]

<span data-ttu-id="8d2a2-358">Önceki kodu kullanarak, ' a gönderen `OnPostJoinListAsync` `http://localhost:5000/Customers/CreateFATH/JoinList`URL yolu.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-358">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `http://localhost:5000/Customers/CreateFATH/JoinList`.</span></span> <span data-ttu-id="8d2a2-359">' A gönderen `OnPostJoinListUCAsync` `http://localhost:5000/Customers/CreateFATH/JoinListUC`URL yolu.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-359">The URL path that submits to `OnPostJoinListUCAsync` is `http://localhost:5000/Customers/CreateFATH/JoinListUC`.</span></span>

<span data-ttu-id="8d2a2-360">`?` Aşağıda`handler` yol parametresinin isteğe bağlı olduğu anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-360">The `?` following `handler` means the route parameter is optional.</span></span>

## <a name="configuration-and-settings"></a><span data-ttu-id="8d2a2-361">Yapılandırma ve ayarlar</span><span class="sxs-lookup"><span data-stu-id="8d2a2-361">Configuration and settings</span></span>

<span data-ttu-id="8d2a2-362">Gelişmiş seçenekleri yapılandırmak için, MVC Oluşturucu 'da genişletme `AddRazorPagesOptions` yöntemini kullanın:</span><span class="sxs-lookup"><span data-stu-id="8d2a2-362">To configure advanced options, use the extension method `AddRazorPagesOptions` on the MVC builder:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/StartupAdvanced.cs?name=snippet_1)]

<span data-ttu-id="8d2a2-363">Şu anda ' nı, `RazorPagesOptions` sayfalar için kök dizini ayarlamak veya sayfalar için uygulama modeli kuralları eklemek için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-363">Currently you can use the `RazorPagesOptions` to set the root directory for pages, or add application model conventions for pages.</span></span> <span data-ttu-id="8d2a2-364">Gelecekte bu şekilde daha fazla genişletilebilirlik etkinleştireceğiz.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-364">We'll enable more extensibility this way in the future.</span></span>

<span data-ttu-id="8d2a2-365">Görünümleri önceden derlemek için bkz. [Razor görünüm derlemesi](xref:mvc/views/view-compilation) .</span><span class="sxs-lookup"><span data-stu-id="8d2a2-365">To precompile views, see [Razor view compilation](xref:mvc/views/view-compilation) .</span></span>

<span data-ttu-id="8d2a2-366">[Örnek kodu indirin veya görüntüleyin](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/index/sample).</span><span class="sxs-lookup"><span data-stu-id="8d2a2-366">[Download or view sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/index/sample).</span></span>

<span data-ttu-id="8d2a2-367">Bu giriş hakkında bilgi için bkz. [Razor Pages kullanmaya başlama](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="8d2a2-367">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start), which builds on this introduction.</span></span>

### <a name="specify-that-razor-pages-are-at-the-content-root"></a><span data-ttu-id="8d2a2-368">Razor Pages içerik kökünde olduğunu belirtin</span><span class="sxs-lookup"><span data-stu-id="8d2a2-368">Specify that Razor Pages are at the content root</span></span>

<span data-ttu-id="8d2a2-369">Varsayılan olarak, Razor Pages */Pages* dizininde kök olarak depolanır.</span><span class="sxs-lookup"><span data-stu-id="8d2a2-369">By default, Razor Pages are rooted in the */Pages* directory.</span></span> <span data-ttu-id="8d2a2-370">Razor Pages, uygulamanın içerik kökünde ([Contentrootpath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) olduğunu belirtmek Için [Addmvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) 'ye [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) ekleyin:</span><span class="sxs-lookup"><span data-stu-id="8d2a2-370">Add [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at the content root ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) of the app:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesAtContentRoot();
```

### <a name="specify-that-razor-pages-are-at-a-custom-root-directory"></a><span data-ttu-id="8d2a2-371">Razor Pages özel kök dizinde olduğunu belirtin</span><span class="sxs-lookup"><span data-stu-id="8d2a2-371">Specify that Razor Pages are at a custom root directory</span></span>

<span data-ttu-id="8d2a2-372">Razor Pages uygulamadaki özel bir kök dizinde olduğunu belirtmek için [Addmvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) 'ye [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) ekleyin (göreli bir yol sağlayın):</span><span class="sxs-lookup"><span data-stu-id="8d2a2-372">Add [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at a custom root directory in the app (provide a relative path):</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesRoot("/path/to/razor/pages");
```

## <a name="additional-resources"></a><span data-ttu-id="8d2a2-373">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="8d2a2-373">Additional resources</span></span>

* <xref:index>
* <xref:mvc/views/razor>
* <xref:mvc/controllers/areas>
* <xref:tutorials/razor-pages/razor-pages-start>
* <xref:security/authorization/razor-pages-authorization>
* <xref:razor-pages/razor-pages-conventions>
* <xref:test/razor-pages-tests>
* <xref:mvc/views/partial>
