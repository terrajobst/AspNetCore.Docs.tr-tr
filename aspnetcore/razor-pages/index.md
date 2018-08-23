---
title: ASP.NET Core Razor sayfalar giriş
author: Rick-Anderson
description: Nasıl ASP.NET Core Razor sayfalar kodlama sayfa odaklı senaryolar daha kolay ve MVC kullanmaktan daha üretken hale getirdiğini öğrenin.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 05/12/2018
uid: razor-pages/index
ms.openlocfilehash: f5549a24c5b5fe2e6b33bd55960f87a8bf86bd19
ms.sourcegitcommit: 5a2456cbf429069dc48aaa2823cde14100e4c438
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2018
ms.locfileid: "41870886"
---
# <a name="introduction-to-razor-pages-in-aspnet-core"></a><span data-ttu-id="9c374-103">ASP.NET Core Razor sayfalar giriş</span><span class="sxs-lookup"><span data-stu-id="9c374-103">Introduction to Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="9c374-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Ryan Nowak](https://github.com/rynowak)</span><span class="sxs-lookup"><span data-stu-id="9c374-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Ryan Nowak](https://github.com/rynowak)</span></span>

<span data-ttu-id="9c374-105">Razor sayfaları, ASP.NET Core MVC sayfası odaklı senaryolar daha kolay ve daha üretken kodlama sağlayan yeni bir yönü olur.</span><span class="sxs-lookup"><span data-stu-id="9c374-105">Razor Pages is a new aspect of ASP.NET Core MVC that makes coding page-focused scenarios easier and more productive.</span></span>

<span data-ttu-id="9c374-106">Model-View-Controller yaklaşım kullanan bir öğretici için arıyorsanız bkz [ASP.NET Core MVC ile çalışmaya başlama](xref:tutorials/first-mvc-app/start-mvc).</span><span class="sxs-lookup"><span data-stu-id="9c374-106">If you're looking for a tutorial that uses the Model-View-Controller approach, see [Get started with ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span></span>

<span data-ttu-id="9c374-107">Bu belge, Razor sayfaları için bir giriş sağlar.</span><span class="sxs-lookup"><span data-stu-id="9c374-107">This document provides an introduction to Razor Pages.</span></span> <span data-ttu-id="9c374-108">Bir adım adım öğretici değil.</span><span class="sxs-lookup"><span data-stu-id="9c374-108">It's not a step by step tutorial.</span></span> <span data-ttu-id="9c374-109">Bazı bölümlerinin çok Gelişmiş bulma olmadığını [Razor sayfaları kullanmaya başlama](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="9c374-109">If you find some of the sections too advanced, see [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span></span> <span data-ttu-id="9c374-110">ASP.NET Core genel bakış için bkz. [ASP.NET Core'a giriş](xref:index).</span><span class="sxs-lookup"><span data-stu-id="9c374-110">For an overview of ASP.NET Core, see the [Introduction to ASP.NET Core](xref:index).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9c374-111">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="9c374-111">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs.md)]

<a name="rpvs17"></a>

## <a name="creating-a-razor-pages-project"></a><span data-ttu-id="9c374-112">Razor sayfaları proje oluşturma</span><span class="sxs-lookup"><span data-stu-id="9c374-112">Creating a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9c374-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9c374-113">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="9c374-114">Bkz: [Razor sayfaları kullanmaya başlama](xref:tutorials/razor-pages/razor-pages-start) Visual Studio kullanarak Razor sayfaları oluşturma hakkında ayrıntılı yönergeler için proje.</span><span class="sxs-lookup"><span data-stu-id="9c374-114">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) for detailed instructions on how to create a Razor Pages project using Visual Studio.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9c374-115">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9c374-115">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="9c374-116">Çalıştırma `dotnet new webapp` komut satırından.</span><span class="sxs-lookup"><span data-stu-id="9c374-116">Run `dotnet new webapp` from the command line.</span></span>

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="9c374-117">Çalıştırma `dotnet new razor` komut satırından.</span><span class="sxs-lookup"><span data-stu-id="9c374-117">Run `dotnet new razor` from the command line.</span></span>

::: moniker-end

<span data-ttu-id="9c374-118">Oluşturulan açın *.csproj* Mac için Visual Studio'dan dosyası</span><span class="sxs-lookup"><span data-stu-id="9c374-118">Open the generated *.csproj* file from Visual Studio for Mac.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9c374-119">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9c374-119">Visual Studio Code</span></span>](#tab/visual-studio-code)

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="9c374-120">Çalıştırma `dotnet new webapp` komut satırından.</span><span class="sxs-lookup"><span data-stu-id="9c374-120">Run `dotnet new webapp` from the command line.</span></span>

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="9c374-121">Çalıştırma `dotnet new razor` komut satırından.</span><span class="sxs-lookup"><span data-stu-id="9c374-121">Run `dotnet new razor` from the command line.</span></span>

::: moniker-end

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="9c374-122">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="9c374-122">.NET Core CLI</span></span>](#tab/netcore-cli)

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="9c374-123">Çalıştırma `dotnet new webapp` komut satırından.</span><span class="sxs-lookup"><span data-stu-id="9c374-123">Run `dotnet new webapp` from the command line.</span></span>

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="9c374-124">Çalıştırma `dotnet new razor` komut satırından.</span><span class="sxs-lookup"><span data-stu-id="9c374-124">Run `dotnet new razor` from the command line.</span></span>

::: moniker-end

---

## <a name="razor-pages"></a><span data-ttu-id="9c374-125">Razor sayfaları</span><span class="sxs-lookup"><span data-stu-id="9c374-125">Razor Pages</span></span>

<span data-ttu-id="9c374-126">Razor sayfaları etkin *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="9c374-126">Razor Pages is enabled in *Startup.cs*:</span></span>

[!code-cs[](index/sample/RazorPagesIntro/Startup.cs?name=snippet_Startup)]

<span data-ttu-id="9c374-127">Temel sayfa göz önünde bulundurun: <a name="OnGet"></a></span><span class="sxs-lookup"><span data-stu-id="9c374-127">Consider a basic page: <a name="OnGet"></a></span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index.cshtml)]

<span data-ttu-id="9c374-128">Yukarıdaki kod, çok Razor görünüm dosyası gibi görünüyor.</span><span class="sxs-lookup"><span data-stu-id="9c374-128">The preceding code looks a lot like a Razor view file.</span></span> <span data-ttu-id="9c374-129">Neler farklı kılan unsurdur `@page` yönergesi.</span><span class="sxs-lookup"><span data-stu-id="9c374-129">What makes it different is the `@page` directive.</span></span> <span data-ttu-id="9c374-130">`@page` Dosya, istekleri doğrudan bir denetleyici geçmeden işleme anlamına gelir ve MVC eyleme - yapar.</span><span class="sxs-lookup"><span data-stu-id="9c374-130">`@page` makes the file into an MVC action - which means that it handles requests directly, without going through a controller.</span></span> <span data-ttu-id="9c374-131">`@page` ilk Razor yönergesi bir sayfa üzerinde olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="9c374-131">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="9c374-132">`@page` diğer Razor yapıları davranışını etkiler.</span><span class="sxs-lookup"><span data-stu-id="9c374-132">`@page` affects the behavior of other Razor constructs.</span></span>

<span data-ttu-id="9c374-133">Benzer bir sayfa, kullanarak bir `PageModel` sınıfında, aşağıdaki iki dosyada gösterilir.</span><span class="sxs-lookup"><span data-stu-id="9c374-133">A similar page, using a `PageModel` class, is shown in the following two files.</span></span> <span data-ttu-id="9c374-134">*Pages/Index2.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="9c374-134">The *Pages/Index2.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index2.cshtml)]

<span data-ttu-id="9c374-135">*Pages/Index2.cshtml.cs* sayfa modeli:</span><span class="sxs-lookup"><span data-stu-id="9c374-135">The *Pages/Index2.cshtml.cs* page model:</span></span>

[!code-cs[](index/sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]

<span data-ttu-id="9c374-136">Kural olarak, `PageModel` sınıf dosyası Razor sayfası dosyası ile aynı ada sahip *.cs* eklenir.</span><span class="sxs-lookup"><span data-stu-id="9c374-136">By convention, the `PageModel` class file has the same name as the Razor Page file with *.cs* appended.</span></span> <span data-ttu-id="9c374-137">Örneğin, önceki Razor sayfa *Pages/Index2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="9c374-137">For example, the previous Razor Page is *Pages/Index2.cshtml*.</span></span> <span data-ttu-id="9c374-138">Dosyayı içeren `PageModel` sınıf adlı *Pages/Index2.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="9c374-138">The file containing the `PageModel` class is named *Pages/Index2.cshtml.cs*.</span></span>

<span data-ttu-id="9c374-139">URL yolu sayfalara ilişkilendirmelerini dosya sisteminde sayfanın konuma göre belirlenir.</span><span class="sxs-lookup"><span data-stu-id="9c374-139">The associations of URL paths to pages are determined by the page's location in the file system.</span></span> <span data-ttu-id="9c374-140">Aşağıdaki tabloda bir Razor sayfası ile eşleşen bir URL gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="9c374-140">The following table shows a Razor Page path and the matching URL:</span></span>

| <span data-ttu-id="9c374-141">Dosya adı ve yolu</span><span class="sxs-lookup"><span data-stu-id="9c374-141">File name and path</span></span>               | <span data-ttu-id="9c374-142">URL eşleştirme</span><span class="sxs-lookup"><span data-stu-id="9c374-142">matching URL</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="9c374-143">*/Pages/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="9c374-143">*/Pages/Index.cshtml*</span></span> | <span data-ttu-id="9c374-144">`/` veya `/Index`</span><span class="sxs-lookup"><span data-stu-id="9c374-144">`/` or `/Index`</span></span> |
| <span data-ttu-id="9c374-145">*/Pages/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="9c374-145">*/Pages/Contact.cshtml*</span></span> | `/Contact` |
| <span data-ttu-id="9c374-146">*/Pages/Store/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="9c374-146">*/Pages/Store/Contact.cshtml*</span></span> | `/Store/Contact` |
| <span data-ttu-id="9c374-147">*/Pages/Store/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="9c374-147">*/Pages/Store/Index.cshtml*</span></span> | <span data-ttu-id="9c374-148">`/Store` veya `/Store/Index`</span><span class="sxs-lookup"><span data-stu-id="9c374-148">`/Store` or `/Store/Index`</span></span> |

<span data-ttu-id="9c374-149">Notlar:</span><span class="sxs-lookup"><span data-stu-id="9c374-149">Notes:</span></span>

* <span data-ttu-id="9c374-150">Razor sayfaları dosyalarında çalışma zamanı arar *sayfaları* varsayılan klasör.</span><span class="sxs-lookup"><span data-stu-id="9c374-150">The runtime looks for Razor Pages files in the *Pages* folder by default.</span></span>
* <span data-ttu-id="9c374-151">`Index` bir URL bir sayfa içermediğinde varsayılan sayfasıdır.</span><span class="sxs-lookup"><span data-stu-id="9c374-151">`Index` is the default page when a URL doesn't include a page.</span></span>

## <a name="writing-a-basic-form"></a><span data-ttu-id="9c374-152">Temel bir form yazma</span><span class="sxs-lookup"><span data-stu-id="9c374-152">Writing a basic form</span></span>

<span data-ttu-id="9c374-153">Razor sayfaları web tarayıcıları ile kolay bir uygulama oluştururken uygulamak için kullanılan ortak desenler hale getirmek için tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="9c374-153">Razor Pages is designed to make common patterns used with web browsers easy to implement when building an app.</span></span> <span data-ttu-id="9c374-154">[Model bağlama](xref:mvc/models/model-binding), [etiket Yardımcıları](xref:mvc/views/tag-helpers/intro)ve HTML yardımcılarını tüm *yalnızca iş* bir Razor sayfası sınıfta tanımlanan özelliklere sahip.</span><span class="sxs-lookup"><span data-stu-id="9c374-154">[Model binding](xref:mvc/models/model-binding), [Tag Helpers](xref:mvc/views/tag-helpers/intro), and HTML helpers all *just work* with the properties defined in a Razor Page class.</span></span> <span data-ttu-id="9c374-155">"Bize başvurun" oluşturmak için temel bir uygulayan bir sayfa göz önünde bulundurun `Contact` modeli:</span><span class="sxs-lookup"><span data-stu-id="9c374-155">Consider a page that implements a basic "contact us" form for the `Contact` model:</span></span>

<span data-ttu-id="9c374-156">Bu belgedeki örnekler için `DbContext` içinde başlatılan [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) dosya.</span><span class="sxs-lookup"><span data-stu-id="9c374-156">For the samples in this document, the `DbContext` is initialized in the [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) file.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Startup.cs?highlight=15-16)]

<span data-ttu-id="9c374-157">Veri modeli:</span><span class="sxs-lookup"><span data-stu-id="9c374-157">The data model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Data/Customer.cs)]

<span data-ttu-id="9c374-158">Db bağlamı:</span><span class="sxs-lookup"><span data-stu-id="9c374-158">The db context:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Data/AppDbContext.cs)]

<span data-ttu-id="9c374-159">*Pages/Create.cshtml* dosyasını görüntüle:</span><span class="sxs-lookup"><span data-stu-id="9c374-159">The *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml)]

<span data-ttu-id="9c374-160">*Pages/Create.cshtml.cs* sayfa modeli:</span><span class="sxs-lookup"><span data-stu-id="9c374-160">The *Pages/Create.cshtml.cs* page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_ALL)]

<span data-ttu-id="9c374-161">Kural olarak, `PageModel` sınıfı çağrıldığında `<PageName>Model` ve aynı ad alanında sayfası.</span><span class="sxs-lookup"><span data-stu-id="9c374-161">By convention, the `PageModel` class is called `<PageName>Model` and is in the same namespace as the page.</span></span>

<span data-ttu-id="9c374-162">`PageModel` Sınıf kendi sunudan sayfasının mantığının ayrımı sağlar.</span><span class="sxs-lookup"><span data-stu-id="9c374-162">The `PageModel` class allows separation of the logic of a page from its presentation.</span></span> <span data-ttu-id="9c374-163">Bu sayfa işleyicileri sayfasına gönderilen istekleri ve sayfayı oluşturmak için kullanılan verileri tanımlar.</span><span class="sxs-lookup"><span data-stu-id="9c374-163">It defines page handlers for requests sent to the page and the data used to render the page.</span></span> <span data-ttu-id="9c374-164">Bu ayırma sayfasında bağımlılıkları üzerinden yönetmenizi sağlar [bağımlılık ekleme](xref:fundamentals/dependency-injection) ve [birim testi](xref:test/razor-pages-tests) sayfaları.</span><span class="sxs-lookup"><span data-stu-id="9c374-164">This separation allows you to manage page dependencies through [dependency injection](xref:fundamentals/dependency-injection) and to [unit test](xref:test/razor-pages-tests) the pages.</span></span>

<span data-ttu-id="9c374-165">Sayfanın bir `OnPostAsync` *işleyicisi yöntemi*, üzerinde çalıştığı `POST` ister (kullanıcı formu gönderdiğinde).</span><span class="sxs-lookup"><span data-stu-id="9c374-165">The page has an `OnPostAsync` *handler method*, which runs on `POST` requests (when a user posts the form).</span></span> <span data-ttu-id="9c374-166">Herhangi bir HTTP fiil için işleyici yöntemleri ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9c374-166">You can add handler methods for any HTTP verb.</span></span> <span data-ttu-id="9c374-167">En yaygın işleyicileri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="9c374-167">The most common handlers are:</span></span>

* <span data-ttu-id="9c374-168">`OnGet` sayfa için gerekli durumu başlatılamadı.</span><span class="sxs-lookup"><span data-stu-id="9c374-168">`OnGet` to initialize state needed for the page.</span></span> <span data-ttu-id="9c374-169">[OnGet](#OnGet) örnek.</span><span class="sxs-lookup"><span data-stu-id="9c374-169">[OnGet](#OnGet) sample.</span></span>
* <span data-ttu-id="9c374-170">`OnPost` Form gönderilerini görselleştirip işlemek için.</span><span class="sxs-lookup"><span data-stu-id="9c374-170">`OnPost` to handle form submissions.</span></span>

<span data-ttu-id="9c374-171">`Async` Adlandırma soneki isteğe bağlıdır, ancak genellikle kurala göre zaman uyumsuz işlevleri için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="9c374-171">The `Async` naming suffix is optional but is often used by convention for asynchronous functions.</span></span> <span data-ttu-id="9c374-172">`OnPostAsync` Kod önceki örnekte ne, normalde bir denetleyicisi yazmalısınız için benzer görünür.</span><span class="sxs-lookup"><span data-stu-id="9c374-172">The `OnPostAsync` code in the preceding example looks similar to what you would normally write in a controller.</span></span> <span data-ttu-id="9c374-173">Yukarıdaki kod, Razor sayfaları için tipik bir durumdur.</span><span class="sxs-lookup"><span data-stu-id="9c374-173">The preceding code is typical for Razor Pages.</span></span> <span data-ttu-id="9c374-174">MVC temelleri çoğu ister [model bağlama](xref:mvc/models/model-binding), [doğrulama](xref:mvc/models/validation), ve eylem sonuçlarını paylaşılır.</span><span class="sxs-lookup"><span data-stu-id="9c374-174">Most of the MVC primitives like [model binding](xref:mvc/models/model-binding), [validation](xref:mvc/models/validation), and action results are shared.</span></span>  <!-- Review: Ryan, can we get a list of what is shared and what isn't? -->

<span data-ttu-id="9c374-175">Önceki `OnPostAsync` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="9c374-175">The previous `OnPostAsync` method:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="9c374-176">Temel akışı `OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="9c374-176">The basic flow of `OnPostAsync`:</span></span>

<span data-ttu-id="9c374-177">Doğrulama hataları denetleyin.</span><span class="sxs-lookup"><span data-stu-id="9c374-177">Check for validation errors.</span></span>

*  <span data-ttu-id="9c374-178">Herhangi bir hata varsa, verileri kaydetmek ve yönlendirin.</span><span class="sxs-lookup"><span data-stu-id="9c374-178">If there are no errors, save the data and redirect.</span></span>
*  <span data-ttu-id="9c374-179">Hatalar varsa sayfası yeniden doğrulama iletileri göster.</span><span class="sxs-lookup"><span data-stu-id="9c374-179">If there are errors, show the page again with validation messages.</span></span> <span data-ttu-id="9c374-180">İstemci tarafı doğrulama geleneksel ASP.NET Core MVC uygulamaları için aynıdır.</span><span class="sxs-lookup"><span data-stu-id="9c374-180">Client-side validation is identical to traditional ASP.NET Core MVC applications.</span></span> <span data-ttu-id="9c374-181">Çoğu durumda, doğrulama hataları istemcide algılandı ve hiçbir zaman sunucuya teslim.</span><span class="sxs-lookup"><span data-stu-id="9c374-181">In many cases, validation errors would be detected on the client, and never submitted to the server.</span></span>

<span data-ttu-id="9c374-182">Verileri başarıyla girildiğinde `OnPostAsync` işleyicisi yöntem çağrılarını `RedirectToPage` örneği döndürmek için yardımcı yöntem `RedirectToPageResult`.</span><span class="sxs-lookup"><span data-stu-id="9c374-182">When the data is entered successfully, the `OnPostAsync` handler method calls the `RedirectToPage` helper method to return an instance of `RedirectToPageResult`.</span></span> <span data-ttu-id="9c374-183">`RedirectToPage` Yeni eylem sonucu, benzer olan `RedirectToAction` veya `RedirectToRoute`, ancak özelleştirilmiş sayfalar için.</span><span class="sxs-lookup"><span data-stu-id="9c374-183">`RedirectToPage` is a new action result, similar to `RedirectToAction` or `RedirectToRoute`, but customized for pages.</span></span> <span data-ttu-id="9c374-184">İsteğe bağlı olarak önceki örnekte, kök dizin sayfasına yönlendirir (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="9c374-184">In the preceding sample, it redirects to the root Index page (`/Index`).</span></span> <span data-ttu-id="9c374-185">`RedirectToPage` içinde ayrıntılı [sayfaları için URL üretimi](#url_gen) bölümü.</span><span class="sxs-lookup"><span data-stu-id="9c374-185">`RedirectToPage` is detailed in the [URL generation for Pages](#url_gen) section.</span></span>

<span data-ttu-id="9c374-186">Gönderilen bir formu, (yani sunucuya geçirilir) doğrulama hataları olduğunda`OnPostAsync` işleyicisi yöntem çağrılarını `Page` yardımcı yöntemi.</span><span class="sxs-lookup"><span data-stu-id="9c374-186">When the submitted form has validation errors (that are passed to the server), the`OnPostAsync` handler method calls the `Page` helper method.</span></span> <span data-ttu-id="9c374-187">`Page` örneği döndürür `PageResult`.</span><span class="sxs-lookup"><span data-stu-id="9c374-187">`Page` returns an instance of `PageResult`.</span></span> <span data-ttu-id="9c374-188">Döndüren `Page` denetleyicileri eylemleri nasıl döndürmek için benzer `View`.</span><span class="sxs-lookup"><span data-stu-id="9c374-188">Returning `Page` is similar to how actions in controllers return `View`.</span></span> <span data-ttu-id="9c374-189">`PageResult` Varsayılan değer <!-- Review  --> dönüş türü için bir işleyici yöntemi.</span><span class="sxs-lookup"><span data-stu-id="9c374-189">`PageResult` is the default <!-- Review  --> return type for a handler method.</span></span> <span data-ttu-id="9c374-190">Döndürür bir işleyici yönteminin `void` sayfasını işler.</span><span class="sxs-lookup"><span data-stu-id="9c374-190">A handler method that returns `void` renders the page.</span></span>

<span data-ttu-id="9c374-191">`Customer` Özelliği kullanan `[BindProperty]` model bağlama için katılım için özniteliği.</span><span class="sxs-lookup"><span data-stu-id="9c374-191">The `Customer` property uses `[BindProperty]` attribute to opt in to model binding.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_PageModel&highlight=10-11)]

<span data-ttu-id="9c374-192">Razor sayfaları varsayılan olarak, GET olmayan fiilleri yalnızca özelliklerle bağlayın.</span><span class="sxs-lookup"><span data-stu-id="9c374-192">Razor Pages, by default, bind properties only with non-GET verbs.</span></span> <span data-ttu-id="9c374-193">Özellikleri bağlama yazmanız gereken kod miktarını azaltır.</span><span class="sxs-lookup"><span data-stu-id="9c374-193">Binding to properties can reduce the amount of code you have to write.</span></span> <span data-ttu-id="9c374-194">Form alanlarını işlemek için aynı özellik kullanarak kod azaltır bağlama (`<input asp-for="Customer.Name" />`) ve giriş kabul edin.</span><span class="sxs-lookup"><span data-stu-id="9c374-194">Binding reduces code by using the same property to render form fields (`<input asp-for="Customer.Name" />`) and accept the input.</span></span>

> [!NOTE]
> <span data-ttu-id="9c374-195">Güvenlik nedenleriyle, sayfa modeli özellikleri için GET isteği veri bağlaması kabul gerekir.</span><span class="sxs-lookup"><span data-stu-id="9c374-195">For security reasons, you must opt in to binding GET request data to page model properties.</span></span> <span data-ttu-id="9c374-196">Özellikleri için eşleme önce kullanıcı girişi doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="9c374-196">Verify user input before mapping it to properties.</span></span> <span data-ttu-id="9c374-197">Bu davranış için seçim olan sorgu dizesi veya rota değerlerini senaryoları belirtirken yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="9c374-197">Opting in to this behavior is useful when addressing scenarios which rely on query string or route values.</span></span>
>
> <span data-ttu-id="9c374-198">Bir özellik üzerinde GET istekleri bağlamak için ayarlanmış `[BindProperty]` özniteliğin `SupportsGet` özelliğini `true`: `[BindProperty(SupportsGet = true)]`</span><span class="sxs-lookup"><span data-stu-id="9c374-198">To bind a property on GET requests, set the `[BindProperty]` attribute's `SupportsGet` property to `true`: `[BindProperty(SupportsGet = true)]`</span></span>

<span data-ttu-id="9c374-199">Giriş sayfası (*Index.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="9c374-199">The home page (*Index.cshtml*):</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml)]

<span data-ttu-id="9c374-200">İlişkili `PageModel` sınıfı (*Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="9c374-200">The associated `PageModel` class (*Index.cshtml.cs*):</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs)]

<span data-ttu-id="9c374-201">*Index.cshtml* dosyası her kişi için bir düzenleme bağlantısı oluşturmak için aşağıdaki biçimlendirme içerir:</span><span class="sxs-lookup"><span data-stu-id="9c374-201">The *Index.cshtml* file contains the following markup to create an edit link for each contact:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=21)]

<span data-ttu-id="9c374-202">[Yer işareti etiketi Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) kullanılan `asp-route-{value}` düzenleme sayfasına giden bir bağlantı oluşturmak için öznitelik.</span><span class="sxs-lookup"><span data-stu-id="9c374-202">The [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) used the `asp-route-{value}` attribute to generate a link to the Edit page.</span></span> <span data-ttu-id="9c374-203">Rota verilerini kişiyle bağlantısını içeren kimliği</span><span class="sxs-lookup"><span data-stu-id="9c374-203">The link contains route data with the contact ID.</span></span> <span data-ttu-id="9c374-204">Örneğin, `http://localhost:5000/Edit/1`.</span><span class="sxs-lookup"><span data-stu-id="9c374-204">For example, `http://localhost:5000/Edit/1`.</span></span>

<span data-ttu-id="9c374-205">*Pages/Edit.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="9c374-205">The *Pages/Edit.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Edit.cshtml?highlight=1)]

<span data-ttu-id="9c374-206">İlk satırında `@page "{id:int}"` yönergesi.</span><span class="sxs-lookup"><span data-stu-id="9c374-206">The first line contains the `@page "{id:int}"` directive.</span></span> <span data-ttu-id="9c374-207">Yönlendirme kısıtlaması`"{id:int}"` sayfasına içeren isteklerini kabul etmek için sayfayı söyler `int` rota verileri.</span><span class="sxs-lookup"><span data-stu-id="9c374-207">The routing constraint`"{id:int}"` tells the page to accept requests to the page that contain `int` route data.</span></span> <span data-ttu-id="9c374-208">Sayfasına bir istek dönüştürülebilir rota verilerini içermiyorsa, bir `int`, çalışma zamanı HTTP 404 (bulunamadı) hatası döndürür.</span><span class="sxs-lookup"><span data-stu-id="9c374-208">If a request to the page doesn't contain route data that can be converted to an `int`, the runtime returns an HTTP 404 (not found) error.</span></span> <span data-ttu-id="9c374-209">Kimliği isteğe bağlı yapmak için URL'ye `?` rota kısıtlaması için:</span><span class="sxs-lookup"><span data-stu-id="9c374-209">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="9c374-210">*Pages/Edit.cshtml.cs* dosyası:</span><span class="sxs-lookup"><span data-stu-id="9c374-210">The *Pages/Edit.cshtml.cs* file:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Edit.cshtml.cs)]

<span data-ttu-id="9c374-211">*Index.cshtml* dosyası ayrıca Sil düğmesini her müşteri iletişim bilgileri için oluşturulacak işaretleme içerir:</span><span class="sxs-lookup"><span data-stu-id="9c374-211">The *Index.cshtml* file also contains markup to create a delete button for each customer contact:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=22-23)]

<span data-ttu-id="9c374-212">Sil düğmesini HTML işlendiğinde, `formaction` parametreleri içerir:</span><span class="sxs-lookup"><span data-stu-id="9c374-212">When the delete button is rendered in HTML, its `formaction` includes parameters for:</span></span>

* <span data-ttu-id="9c374-213">Müşteri Kimliği tarafından belirtilen iletişime `asp-route-id` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="9c374-213">The customer contact ID specified by the `asp-route-id` attribute.</span></span>
* <span data-ttu-id="9c374-214">`handler` Tarafından belirtilen `asp-page-handler` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="9c374-214">The `handler` specified by the `asp-page-handler` attribute.</span></span>

<span data-ttu-id="9c374-215">İşte bir müşteri bir işlenen Sil düğmesini bir örnek kimliği başvurun `1`:</span><span class="sxs-lookup"><span data-stu-id="9c374-215">Here is an example of a rendered delete button with a customer contact ID of `1`:</span></span>

```html
<button type="submit" formaction="/?id=1&amp;handler=delete">delete</button>
```

<span data-ttu-id="9c374-216">Düğme seçildiğinde, bir form `POST` isteği sunucuya gönderilir.</span><span class="sxs-lookup"><span data-stu-id="9c374-216">When the button is selected, a form `POST` request is sent to the server.</span></span> <span data-ttu-id="9c374-217">Kural gereği, işleyici yönteminin adı seçili değerine göre `handler` parametre düzeni göre `OnPost[handler]Async`.</span><span class="sxs-lookup"><span data-stu-id="9c374-217">By convention, the name of the handler method is selected based the value of the `handler` parameter according to the scheme `OnPost[handler]Async`.</span></span>

<span data-ttu-id="9c374-218">Çünkü `handler` olduğu `delete` Bu örnekte, `OnPostDeleteAsync` işleyicisi yöntemi kullanılır işleme `POST` isteği.</span><span class="sxs-lookup"><span data-stu-id="9c374-218">Because the `handler` is `delete` in this example, the `OnPostDeleteAsync` handler method is used to process the `POST` request.</span></span> <span data-ttu-id="9c374-219">Varsa `asp-page-handler` gibi farklı bir değere ayarlanmış `remove`, ada sahip bir sayfa işleyicisi yöntemi `OnPostRemoveAsync` seçilir.</span><span class="sxs-lookup"><span data-stu-id="9c374-219">If the `asp-page-handler` is set to a different value, such as `remove`, a page handler method with the name `OnPostRemoveAsync` is selected.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs?range=26-37)]

<span data-ttu-id="9c374-220">`OnPostDeleteAsync` Yöntemi:</span><span class="sxs-lookup"><span data-stu-id="9c374-220">The `OnPostDeleteAsync` method:</span></span>

* <span data-ttu-id="9c374-221">Kabul `id` Sorgu dizesinden.</span><span class="sxs-lookup"><span data-stu-id="9c374-221">Accepts the `id` from the query string.</span></span>
* <span data-ttu-id="9c374-222">Veritabanı ile müşteri iletişim bilgileri için sorgular `FindAsync`.</span><span class="sxs-lookup"><span data-stu-id="9c374-222">Queries the database for the customer contact with `FindAsync`.</span></span>
* <span data-ttu-id="9c374-223">Müşteri irtibat bulunursa, müşteri kişiler listesinden kaldırıldı.</span><span class="sxs-lookup"><span data-stu-id="9c374-223">If the customer contact is found, they're removed from the list of customer contacts.</span></span> <span data-ttu-id="9c374-224">Veritabanı güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="9c374-224">The database is updated.</span></span>
* <span data-ttu-id="9c374-225">Çağrıları `RedirectToPage` kök dizin sayfasına yeniden yönlendirmek için (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="9c374-225">Calls `RedirectToPage` to redirect to the root Index page (`/Index`).</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="mark-page-properties-required"></a><span data-ttu-id="9c374-226">Gerekli işareti sayfa özellikleri</span><span class="sxs-lookup"><span data-stu-id="9c374-226">Mark page properties required</span></span>

<span data-ttu-id="9c374-227">Özellikleri bir `PageModel` ile donatılmış [gerekli](/dotnet/api/system.componentmodel.dataannotations.requiredattribute) özniteliği:</span><span class="sxs-lookup"><span data-stu-id="9c374-227">Properties on a `PageModel` can be decorated with the [Required](/dotnet/api/system.componentmodel.dataannotations.requiredattribute) attribute:</span></span>

[!code-cs[](index/sample/Create.cshtml.cs?highlight=3,15-16)]

<span data-ttu-id="9c374-228">Bkz: [Model doğrulama](xref:mvc/models/validation) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="9c374-228">See [Model validation](xref:mvc/models/validation) for more information.</span></span>

## <a name="manage-head-requests-with-the-onget-handler"></a><span data-ttu-id="9c374-229">HEAD isteklerini OnGet işleyici ile yönetme</span><span class="sxs-lookup"><span data-stu-id="9c374-229">Manage HEAD requests with the OnGet handler</span></span>

<span data-ttu-id="9c374-230">Normalde, bir baş işleyici oluşturulur ve HEAD isteklerini çağrılır:</span><span class="sxs-lookup"><span data-stu-id="9c374-230">Ordinarily, a HEAD handler is created and called for HEAD requests:</span></span>

```csharp
public void OnHead()
{
    HttpContext.Response.Headers.Add("HandledBy", "Handled by OnHead!");
}
```

<span data-ttu-id="9c374-231">Hiçbir baş işleyici (`OnHead`) olan tanımlanan, Razor sayfaları geri alma sayfası işleyicisi çağırma için döner (`OnGet`) ASP.NET Core 2.1 veya üzeri.</span><span class="sxs-lookup"><span data-stu-id="9c374-231">If no HEAD handler (`OnHead`) is defined, Razor Pages falls back to calling the GET page handler (`OnGet`) in ASP.NET Core 2.1 or later.</span></span> <span data-ttu-id="9c374-232">Bu davranışı ile kabul etmek [SetCompatibilityVersion yöntemi](xref:mvc/compatibility-version) içinde `Startup.Configure` 2.x için ASP.NET Core 2.1 için:</span><span class="sxs-lookup"><span data-stu-id="9c374-232">Opt in to this behavior with the [SetCompatibilityVersion method](xref:mvc/compatibility-version) in `Startup.Configure` for ASP.NET Core 2.1 to 2.x:</span></span>

```csharp
services.AddMvc()
    .SetCompatibilityVersion(Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_1);
```

<span data-ttu-id="9c374-233">`SetCompatibilityVersion` Razor sayfaları seçeneği etkili bir şekilde ayarlar `AllowMappingHeadRequestsToGetHandler` için `true`.</span><span class="sxs-lookup"><span data-stu-id="9c374-233">`SetCompatibilityVersion` effectively sets the Razor Pages option `AllowMappingHeadRequestsToGetHandler` to `true`.</span></span>

<span data-ttu-id="9c374-234">Tüm 2.1 davranışlarla içine edilmesiyle yerine `SetCompatibilityVersion`, siz açıkça özel davranışları için katılımı.</span><span class="sxs-lookup"><span data-stu-id="9c374-234">Rather than opting into all 2.1 behaviors with `SetCompatibilityVersion`, you can explicitly opt-in to specific behaviors.</span></span> <span data-ttu-id="9c374-235">Aşağıdaki kod içine GET işleyicisine eşleme HEAD isteklerini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="9c374-235">The following code opts into the mapping HEAD requests to the GET handler.</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        options.AllowMappingHeadRequestsToGetHandler = true;
    });
```

::: moniker-end

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a><span data-ttu-id="9c374-236">XSRF/CSRF ve Razor sayfaları</span><span class="sxs-lookup"><span data-stu-id="9c374-236">XSRF/CSRF and Razor Pages</span></span>

<span data-ttu-id="9c374-237">Herhangi bir kod yazmanıza gerek kalmaz [antiforgery doğrulama](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="9c374-237">You don't have to write any code for [antiforgery validation](xref:security/anti-request-forgery).</span></span> <span data-ttu-id="9c374-238">Otomatik olarak antiforgery belirteç oluşturma ve doğrulama Razor sayfaları dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="9c374-238">Antiforgery token generation and validation are automatically included in Razor Pages.</span></span>

<a name="layout"></a>
## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a><span data-ttu-id="9c374-239">Razor sayfaları kullanmaya düzenleri, kısmi, şablonları ve etiket yardımcılarını kullanma</span><span class="sxs-lookup"><span data-stu-id="9c374-239">Using Layouts, partials, templates, and Tag Helpers with Razor Pages</span></span>

<span data-ttu-id="9c374-240">Sayfaları Razor görüntüleme motorunu tüm özellikleriyle çalışma.</span><span class="sxs-lookup"><span data-stu-id="9c374-240">Pages work with all the capabilities of the Razor view engine.</span></span> <span data-ttu-id="9c374-241">Düzenleri, kısmi, şablonlar, etiket Yardımcıları *_ViewStart.cshtml*, *_viewımports.cshtml* geleneksel Razor görünümleri yaptıkları aynı şekilde çalışır.</span><span class="sxs-lookup"><span data-stu-id="9c374-241">Layouts, partials, templates, Tag Helpers, *_ViewStart.cshtml*, *_ViewImports.cshtml* work in the same way they do for conventional Razor views.</span></span>

<span data-ttu-id="9c374-242">Şimdi bu sayfa, bu özelliklerden bazılarını avantajlarından yararlanarak declutter.</span><span class="sxs-lookup"><span data-stu-id="9c374-242">Let's declutter this page by taking advantage of some of those capabilities.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="9c374-243">Ekleme bir [düzen sayfası](xref:mvc/views/layout) için *Pages/Shared/_Layout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="9c374-243">Add a [layout page](xref:mvc/views/layout) to *Pages/Shared/_Layout.cshtml*:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="9c374-244">Ekleme bir [düzen sayfası](xref:mvc/views/layout) için *Pages/_Layout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="9c374-244">Add a [layout page](xref:mvc/views/layout) to *Pages/_Layout.cshtml*:</span></span>

::: moniker-end

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_LayoutSimple.cshtml)]

<span data-ttu-id="9c374-245">[Düzen](xref:mvc/views/layout):</span><span class="sxs-lookup"><span data-stu-id="9c374-245">The [Layout](xref:mvc/views/layout):</span></span>

* <span data-ttu-id="9c374-246">(Sayfa düzeni dışında bölgedeyse sürece) her sayfasının düzenini denetler.</span><span class="sxs-lookup"><span data-stu-id="9c374-246">Controls the layout of each page (unless the page opts out of layout).</span></span>
* <span data-ttu-id="9c374-247">JavaScript ve stil sayfalarını gibi HTML yapıları içeri aktarır.</span><span class="sxs-lookup"><span data-stu-id="9c374-247">Imports HTML structures such as JavaScript and stylesheets.</span></span>

<span data-ttu-id="9c374-248">Bkz: [düzen sayfası](xref:mvc/views/layout) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="9c374-248">See [layout page](xref:mvc/views/layout) for more information.</span></span>

<span data-ttu-id="9c374-249">[Düzen](xref:mvc/views/layout#specifying-a-layout) özelliği ayarlandığında *Pages/_ViewStart.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="9c374-249">The [Layout](xref:mvc/views/layout#specifying-a-layout) property is set in *Pages/_ViewStart.cshtml*:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="9c374-250">Düzen bulunduğu *sayfaları/paylaşılan* klasör.</span><span class="sxs-lookup"><span data-stu-id="9c374-250">The layout is in the *Pages/Shared* folder.</span></span> <span data-ttu-id="9c374-251">Geçerli sayfa ile aynı klasörde başlangıç sayfaları başka görünümlerini (düzenleri, şablonlar, kısmi) hiyerarşik olarak arayın.</span><span class="sxs-lookup"><span data-stu-id="9c374-251">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="9c374-252">Bir düzende *sayfaları/paylaşılan* klasörü altında herhangi bir Razor sayfadan kullanılabilir *sayfaları* klasör.</span><span class="sxs-lookup"><span data-stu-id="9c374-252">A layout in the *Pages/Shared* folder can be used from any Razor page under the *Pages* folder.</span></span>

<span data-ttu-id="9c374-253">Düzen dosyası gitmesi gereken *sayfaları/paylaşılan* klasör.</span><span class="sxs-lookup"><span data-stu-id="9c374-253">The layout file should go in the *Pages/Shared* folder.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="9c374-254">Düzen bulunduğu *sayfaları* klasör.</span><span class="sxs-lookup"><span data-stu-id="9c374-254">The layout is in the *Pages* folder.</span></span> <span data-ttu-id="9c374-255">Geçerli sayfa ile aynı klasörde başlangıç sayfaları başka görünümlerini (düzenleri, şablonlar, kısmi) hiyerarşik olarak arayın.</span><span class="sxs-lookup"><span data-stu-id="9c374-255">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="9c374-256">Bir düzende *sayfaları* klasörü altında herhangi bir Razor sayfadan kullanılabilir *sayfaları* klasör.</span><span class="sxs-lookup"><span data-stu-id="9c374-256">A layout in the *Pages* folder can be used from any Razor page under the *Pages* folder.</span></span>

::: moniker-end

<span data-ttu-id="9c374-257">Öneririz **değil** Düzen dosyası koymak *görünümler/paylaşılan* klasör.</span><span class="sxs-lookup"><span data-stu-id="9c374-257">We recommend you **not** put the layout file in the *Views/Shared* folder.</span></span> <span data-ttu-id="9c374-258">*Görünümler/paylaşılan* bir MVC görünümleri modelidir.</span><span class="sxs-lookup"><span data-stu-id="9c374-258">*Views/Shared* is an MVC views pattern.</span></span> <span data-ttu-id="9c374-259">Razor sayfaları klasör hiyerarşisi, yol kuralları yararlanmayı yöneliktir.</span><span class="sxs-lookup"><span data-stu-id="9c374-259">Razor Pages are meant to rely on folder hierarchy, not path conventions.</span></span>

<span data-ttu-id="9c374-260">Bir Razor sayfası görünümü aramadan içerir *sayfaları* klasör.</span><span class="sxs-lookup"><span data-stu-id="9c374-260">View search from a Razor Page includes the *Pages* folder.</span></span> <span data-ttu-id="9c374-261">Düzenleri, şablonları ve geleneksel Razor görünümleri ile MVC denetleyicileri ile kullanmakta olduğunuz kısmi *yalnızca iş*.</span><span class="sxs-lookup"><span data-stu-id="9c374-261">The layouts, templates, and partials you're using with MVC controllers and conventional Razor views *just work*.</span></span>

<span data-ttu-id="9c374-262">Ekleme bir *Pages/_ViewImports.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="9c374-262">Add a *Pages/_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]

<span data-ttu-id="9c374-263">`@namespace` öğreticinin ilerleyen bölümlerinde açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="9c374-263">`@namespace` is explained later in the tutorial.</span></span> <span data-ttu-id="9c374-264">`@addTagHelper` Yönergesi getirdiği [yerleşik etiket Yardımcıları](xref:mvc/views/tag-helpers/builtin-th/Index) tüm sayfalara *sayfaları* klasör.</span><span class="sxs-lookup"><span data-stu-id="9c374-264">The `@addTagHelper` directive brings in the [built-in Tag Helpers](xref:mvc/views/tag-helpers/builtin-th/Index) to all the pages in the *Pages* folder.</span></span>

<a name="namespace"></a>

<span data-ttu-id="9c374-265">Zaman `@namespace` yönergesi bir sayfada açıkça kullanılır:</span><span class="sxs-lookup"><span data-stu-id="9c374-265">When the `@namespace` directive is used explicitly on a page:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]

<span data-ttu-id="9c374-266">Yönergesi, ad alanı sayfası için ayarlar.</span><span class="sxs-lookup"><span data-stu-id="9c374-266">The directive sets the namespace for the page.</span></span> <span data-ttu-id="9c374-267">`@model` Yönergesi ad alanını katmak gerekmez.</span><span class="sxs-lookup"><span data-stu-id="9c374-267">The `@model` directive doesn't need to include the namespace.</span></span>

<span data-ttu-id="9c374-268">Zaman `@namespace` yönergesi kapsanıyorsa *_viewımports.cshtml*, belirtilen ad alanı içe aktaran sayfasında oluşturulan ad alanı öneki sağlayan `@namespace` yönergesi.</span><span class="sxs-lookup"><span data-stu-id="9c374-268">When the `@namespace` directive is contained in *_ViewImports.cshtml*, the specified namespace supplies the prefix for the generated namespace in the Page that imports the `@namespace` directive.</span></span> <span data-ttu-id="9c374-269">Oluşturulan ad alanı (sonek kısmı) geri kalanı içeren klasörü arasında noktayla ayrılmış göreli yoludur *_viewımports.cshtml* ve sayfayı içeren klasör.</span><span class="sxs-lookup"><span data-stu-id="9c374-269">The rest of the generated namespace (the suffix portion) is the dot-separated relative path between the folder containing *_ViewImports.cshtml* and the folder containing the page.</span></span>

<span data-ttu-id="9c374-270">Örneğin, `PageModel` sınıfı *Pages/Customers/Edit.cshtml.cs* ad alanını açıkça ayarlar:</span><span class="sxs-lookup"><span data-stu-id="9c374-270">For example, the `PageModel` class *Pages/Customers/Edit.cshtml.cs* explicitly sets the namespace:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=snippet_namespace)]

<span data-ttu-id="9c374-271">*Pages/_ViewImports.cshtml* dosyasını ayarlar şu ad alanı:</span><span class="sxs-lookup"><span data-stu-id="9c374-271">The *Pages/_ViewImports.cshtml* file sets the following namespace:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]

<span data-ttu-id="9c374-272">Oluşturulan ad alanı için *Pages/Customers/Edit.cshtml* Razor sayfası aynıdır `PageModel` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="9c374-272">The generated namespace for the *Pages/Customers/Edit.cshtml* Razor Page is the same as the `PageModel` class.</span></span>

<span data-ttu-id="9c374-273">`@namespace` *Geleneksel Razor görünümleri ile de çalışır.*</span><span class="sxs-lookup"><span data-stu-id="9c374-273">`@namespace` *also works with conventional Razor views.*</span></span>

<span data-ttu-id="9c374-274">Özgün *Pages/Create.cshtml* dosyasını görüntüle:</span><span class="sxs-lookup"><span data-stu-id="9c374-274">The original *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml?highlight=2)]

<span data-ttu-id="9c374-275">Güncelleştirilmiş *Pages/Create.cshtml* dosyasını görüntüle:</span><span class="sxs-lookup"><span data-stu-id="9c374-275">The updated *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/Create.cshtml?highlight=2)]

<span data-ttu-id="9c374-276">[Razor sayfaları başlangıç projesini](#rpvs17) içeren *Pages/_ValidationScriptsPartial.cshtml*, istemci tarafı doğrulama kancaları.</span><span class="sxs-lookup"><span data-stu-id="9c374-276">The [Razor Pages starter project](#rpvs17) contains the *Pages/_ValidationScriptsPartial.cshtml*, which hooks up client-side validation.</span></span>

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a><span data-ttu-id="9c374-277">Sayfaları için URL üretimi</span><span class="sxs-lookup"><span data-stu-id="9c374-277">URL generation for Pages</span></span>

<span data-ttu-id="9c374-278">`Create` Sayfasında, daha önce kullandığı gösterilen `RedirectToPage`:</span><span class="sxs-lookup"><span data-stu-id="9c374-278">The `Create` page, shown previously, uses `RedirectToPage`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=10)]

<span data-ttu-id="9c374-279">Uygulama, aşağıdaki dosya/klasör yapısına sahiptir:</span><span class="sxs-lookup"><span data-stu-id="9c374-279">The app has the following file/folder structure:</span></span>

* <span data-ttu-id="9c374-280">*/ Sayfaları*</span><span class="sxs-lookup"><span data-stu-id="9c374-280">*/Pages*</span></span>

  * <span data-ttu-id="9c374-281">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="9c374-281">*Index.cshtml*</span></span>
  * <span data-ttu-id="9c374-282">*/ Müşteriler*</span><span class="sxs-lookup"><span data-stu-id="9c374-282">*/Customers*</span></span>

    * <span data-ttu-id="9c374-283">*Create.cshtml*</span><span class="sxs-lookup"><span data-stu-id="9c374-283">*Create.cshtml*</span></span>
    * <span data-ttu-id="9c374-284">*Edit.cshtml*</span><span class="sxs-lookup"><span data-stu-id="9c374-284">*Edit.cshtml*</span></span>
    * <span data-ttu-id="9c374-285">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="9c374-285">*Index.cshtml*</span></span>

<span data-ttu-id="9c374-286">*Pages/Customers/Create.cshtml* ve *Pages/Customers/Edit.cshtml* yeniden yönlendirme sayfası *Pages/Index.cshtml* başarılı olduktan sonra.</span><span class="sxs-lookup"><span data-stu-id="9c374-286">The *Pages/Customers/Create.cshtml* and *Pages/Customers/Edit.cshtml* pages redirect to *Pages/Index.cshtml* after success.</span></span> <span data-ttu-id="9c374-287">Dize `/Index` önceki sayfaya erişmek için URI'ın bir parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="9c374-287">The string `/Index` is part of the URI to access the preceding page.</span></span> <span data-ttu-id="9c374-288">Dize `/Index` için URI oluşturmak için kullanılan *Pages/Index.cshtml* sayfası.</span><span class="sxs-lookup"><span data-stu-id="9c374-288">The string `/Index` can be used to generate URIs to the *Pages/Index.cshtml* page.</span></span> <span data-ttu-id="9c374-289">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="9c374-289">For example:</span></span>

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">My Index Page</a>`
* `RedirectToPage("/Index")`

<span data-ttu-id="9c374-290">Sayfa adı sayfasına kökünden yoludur */sayfaları* önde gelen dahil olmak üzere klasörü `/` (örneğin, `/Index`).</span><span class="sxs-lookup"><span data-stu-id="9c374-290">The page name is the path to the page from the root */Pages* folder including a leading `/` (for example, `/Index`).</span></span> <span data-ttu-id="9c374-291">Önceki URL oluşturma örnekleri, runbook'a kod bir URL Gelişmiş Seçenekler ve işlevsel yetenekleri sunar.</span><span class="sxs-lookup"><span data-stu-id="9c374-291">The preceding URL generation samples offer enhanced options and functional capabilities over hardcoding a URL.</span></span> <span data-ttu-id="9c374-292">URL üretimi kullanan [yönlendirme](xref:mvc/controllers/routing) ve oluşturmak ve parametreleri hedef yolda bir rota nasıl tanımlandığını göre kodlayın.</span><span class="sxs-lookup"><span data-stu-id="9c374-292">URL generation uses [routing](xref:mvc/controllers/routing) and can generate and encode parameters according to how the route is defined in the destination path.</span></span>

<span data-ttu-id="9c374-293">URL üretimi sayfaları için göreli adlarını destekler.</span><span class="sxs-lookup"><span data-stu-id="9c374-293">URL generation for pages supports relative names.</span></span> <span data-ttu-id="9c374-294">Aşağıdaki tabloda, hangi dizin sayfası ile farklı seçili olduğunu gösterir `RedirectToPage` parametrelerinden *Pages/Customers/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="9c374-294">The following table shows which Index page is selected with different `RedirectToPage` parameters from *Pages/Customers/Create.cshtml*:</span></span>

| <span data-ttu-id="9c374-295">RedirectToPage(x)</span><span class="sxs-lookup"><span data-stu-id="9c374-295">RedirectToPage(x)</span></span>| <span data-ttu-id="9c374-296">Sayfa</span><span class="sxs-lookup"><span data-stu-id="9c374-296">Page</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="9c374-297">RedirectToPage("/Index")</span><span class="sxs-lookup"><span data-stu-id="9c374-297">RedirectToPage("/Index")</span></span> | <span data-ttu-id="9c374-298">*Sayfalar/dizin*</span><span class="sxs-lookup"><span data-stu-id="9c374-298">*Pages/Index*</span></span> |
| <span data-ttu-id="9c374-299">RedirectToPage("./Index");</span><span class="sxs-lookup"><span data-stu-id="9c374-299">RedirectToPage("./Index");</span></span> | <span data-ttu-id="9c374-300">*Müşteriler/sayfalar/dizin*</span><span class="sxs-lookup"><span data-stu-id="9c374-300">*Pages/Customers/Index*</span></span> |
| <span data-ttu-id="9c374-301">RedirectToPage(".. / Dizin")</span><span class="sxs-lookup"><span data-stu-id="9c374-301">RedirectToPage("../Index")</span></span> | <span data-ttu-id="9c374-302">*Sayfalar/dizin*</span><span class="sxs-lookup"><span data-stu-id="9c374-302">*Pages/Index*</span></span> |
| <span data-ttu-id="9c374-303">RedirectToPage("Index")</span><span class="sxs-lookup"><span data-stu-id="9c374-303">RedirectToPage("Index")</span></span>  | <span data-ttu-id="9c374-304">*Müşteriler/sayfalar/dizin*</span><span class="sxs-lookup"><span data-stu-id="9c374-304">*Pages/Customers/Index*</span></span> |

<span data-ttu-id="9c374-305">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, ve `RedirectToPage("../Index")` olan <em>göreli adlar</em>.</span><span class="sxs-lookup"><span data-stu-id="9c374-305">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, and `RedirectToPage("../Index")`  are <em>relative names</em>.</span></span> <span data-ttu-id="9c374-306">`RedirectToPage` Parametresi <em>birleştirilmiş</em> ile hedef sayfanın adını işlem için geçerli sayfasının yolu.</span><span class="sxs-lookup"><span data-stu-id="9c374-306">The `RedirectToPage` parameter is <em>combined</em> with the path of the current page to compute the name of the destination page.</span></span>  <!-- Review: Original had The provided string is combined with the page name of the current page to compute the name of the destination page.  page name, not page path -->

<span data-ttu-id="9c374-307">Göreli adı bağlama siteleri karmaşık bir yapısı ile derleme yaparken yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="9c374-307">Relative name linking is useful when building sites with a complex structure.</span></span> <span data-ttu-id="9c374-308">Bir klasördeki sayfalar arasında bağlamak için göreli adları kullanıyorsa, bu klasörü yeniden adlandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9c374-308">If you use relative names to link between pages in a folder, you can rename that folder.</span></span> <span data-ttu-id="9c374-309">(Bunlar klasör adı olmadığından) tüm bağlantıların hala çalıştığından.</span><span class="sxs-lookup"><span data-stu-id="9c374-309">All the links still work (because they didn't include the folder name).</span></span>

::: moniker range=">= aspnetcore-2.1"
## <a name="viewdata-attribute"></a><span data-ttu-id="9c374-310">ViewData özniteliği</span><span class="sxs-lookup"><span data-stu-id="9c374-310">ViewData attribute</span></span>

<span data-ttu-id="9c374-311">Veri içeren bir sayfa geçilebilir [ViewDataAttribute](/dotnet/api/microsoft.aspnetcore.mvc.viewdataattribute).</span><span class="sxs-lookup"><span data-stu-id="9c374-311">Data can be passed to a page with [ViewDataAttribute](/dotnet/api/microsoft.aspnetcore.mvc.viewdataattribute).</span></span> <span data-ttu-id="9c374-312">Denetleyicilerde veya modellerde Razor sayfası özelliklerini düzenlenmiş ile `[ViewData]` depolanır ve gelen yüklenen değerlerine sahip [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary).</span><span class="sxs-lookup"><span data-stu-id="9c374-312">Properties on controllers or Razor Page models decorated with `[ViewData]` have their values stored and loaded from the [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary).</span></span>

<span data-ttu-id="9c374-313">Aşağıdaki örnekte, `AboutModel` içeren bir `Title` özelliği düzenlenmiş ile `[ViewData]`.</span><span class="sxs-lookup"><span data-stu-id="9c374-313">In the following example, the `AboutModel` contains a `Title` property decorated with `[ViewData]`.</span></span> <span data-ttu-id="9c374-314">`Title` Özelliği hakkında sayfasının başlığı ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="9c374-314">The `Title` property is set to the title of the About page:</span></span>

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

<span data-ttu-id="9c374-315">Hakkında sayfasında erişim `Title` özelliği model özelliği olarak:</span><span class="sxs-lookup"><span data-stu-id="9c374-315">In the About page, access the `Title` property as a model property:</span></span>

```cshtml
<h1>@Model.Title</h1>
```

<span data-ttu-id="9c374-316">Düzende dışında ViewData sözlükten başlığını okuyun:</span><span class="sxs-lookup"><span data-stu-id="9c374-316">In the layout, the title is read from the ViewData dictionary:</span></span>

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"] - WebApplication</title>
    ...
```
::: moniker-end

## <a name="tempdata"></a><span data-ttu-id="9c374-317">TempData</span><span class="sxs-lookup"><span data-stu-id="9c374-317">TempData</span></span>

<span data-ttu-id="9c374-318">ASP.NET Core sunan [TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) özelliği bir [denetleyicisi](/dotnet/api/microsoft.aspnetcore.mvc.controller).</span><span class="sxs-lookup"><span data-stu-id="9c374-318">ASP.NET Core exposes the [TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) property on a [controller](/dotnet/api/microsoft.aspnetcore.mvc.controller).</span></span> <span data-ttu-id="9c374-319">Bu özellik, okunur kadar verileri depolar.</span><span class="sxs-lookup"><span data-stu-id="9c374-319">This property stores data until it's read.</span></span> <span data-ttu-id="9c374-320">`Keep` Ve `Peek` yöntemleri silme olmadan verileri incelemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="9c374-320">The `Keep` and `Peek` methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="9c374-321">`TempData` için yeniden yönlendirme, birden çok tek bir istek verileri gerektiğinde yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="9c374-321">`TempData` is  useful for redirection, when data is needed for more than a single request.</span></span>

<span data-ttu-id="9c374-322">`[TempData]` Özniteliği ASP.NET Core 2.0 sürümünde yenidir ve denetleyicileri ve sayfaları desteklenmektedir.</span><span class="sxs-lookup"><span data-stu-id="9c374-322">The `[TempData]` attribute is new in ASP.NET Core 2.0 and is supported on controllers and pages.</span></span>

<span data-ttu-id="9c374-323">Aşağıdaki kodu değerini ayarlar `Message` kullanarak `TempData`:</span><span class="sxs-lookup"><span data-stu-id="9c374-323">The following code sets the value of `Message` using `TempData`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,25&name=snippet_Temp)]

<span data-ttu-id="9c374-324">Aşağıdaki biçimlendirmede *Pages/Customers/Index.cshtml* dosya değerini görüntülediğine `Message` kullanarak `TempData`.</span><span class="sxs-lookup"><span data-stu-id="9c374-324">The following markup in the *Pages/Customers/Index.cshtml* file displays the value of `Message` using `TempData`.</span></span>

```cshtml
<h3>Msg: @Model.Message</h3>
```

<span data-ttu-id="9c374-325">*Pages/Customers/Index.cshtml.cs* sayfa modeli uygular `[TempData]` özniteliğini `Message` özelliği.</span><span class="sxs-lookup"><span data-stu-id="9c374-325">The *Pages/Customers/Index.cshtml.cs* page model applies the `[TempData]` attribute to the `Message` property.</span></span>

```cs
[TempData]
public string Message { get; set; }
```

<span data-ttu-id="9c374-326">Bkz: [TempData](xref:fundamentals/app-state#tempdata) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="9c374-326">See [TempData](xref:fundamentals/app-state#tempdata) for more information.</span></span>

<a name="mhpp"></a>
## <a name="multiple-handlers-per-page"></a><span data-ttu-id="9c374-327">Sayfa başına birden çok işleyicileri</span><span class="sxs-lookup"><span data-stu-id="9c374-327">Multiple handlers per page</span></span>

<span data-ttu-id="9c374-328">İki sayfa kullanarak işleyicileri için şu sayfaya biçimlendirmeleri oluşturur `asp-page-handler` etiketi Yardımcısı:</span><span class="sxs-lookup"><span data-stu-id="9c374-328">The following page generates markup for two page handlers using the `asp-page-handler` Tag Helper:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

<!-- Review: the FormActionTagHelper applies to all <form /> elements on a Razor page, even when there's no `asp-` attribute   -->

<span data-ttu-id="9c374-329">Önceki örnekte iki düğmeleri kullanarak her gönderme formundadır `FormActionTagHelper` için farklı bir URL göndermek için.</span><span class="sxs-lookup"><span data-stu-id="9c374-329">The form in the preceding example has two submit buttons, each using the `FormActionTagHelper` to submit to a different URL.</span></span> <span data-ttu-id="9c374-330">`asp-page-handler` Özniteliktir na yardımcı olarak `asp-page`.</span><span class="sxs-lookup"><span data-stu-id="9c374-330">The `asp-page-handler` attribute is a companion to `asp-page`.</span></span> <span data-ttu-id="9c374-331">`asp-page-handler` bir sayfa tarafından tanımlanan işleyici yöntemlerin her biri için gönderme URL oluşturur.</span><span class="sxs-lookup"><span data-stu-id="9c374-331">`asp-page-handler` generates URLs that submit to each of the handler methods defined by a page.</span></span> <span data-ttu-id="9c374-332">`asp-page` örnek, geçerli sayfa için bağlama için belirtilmemiş.</span><span class="sxs-lookup"><span data-stu-id="9c374-332">`asp-page` isn't specified because the sample is linking to the current page.</span></span>

<span data-ttu-id="9c374-333">Sayfa modeli:</span><span class="sxs-lookup"><span data-stu-id="9c374-333">The page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]

<span data-ttu-id="9c374-334">Önceki kod *işleyici yöntemleri adlı*.</span><span class="sxs-lookup"><span data-stu-id="9c374-334">The preceding code uses *named handler methods*.</span></span> <span data-ttu-id="9c374-335">Adlandırılmış işleyici yöntemleri adında sonra metin alarak oluşturulur `On<HTTP Verb>` ve önce `Async` (varsa).</span><span class="sxs-lookup"><span data-stu-id="9c374-335">Named handler methods are created by taking the text in the name after `On<HTTP Verb>` and before `Async` (if present).</span></span> <span data-ttu-id="9c374-336">Önceki örnekte OnPost sayfa yöntemlerdir**JoinList**zaman uyumsuz ve OnPost**JoinListUC**zaman uyumsuz.</span><span class="sxs-lookup"><span data-stu-id="9c374-336">In the preceding example, the page methods are OnPost**JoinList**Async and OnPost**JoinListUC**Async.</span></span> <span data-ttu-id="9c374-337">İle *OnPost* ve *zaman uyumsuz* kaldırıldıysa, işleyici adları olan `JoinList` ve `JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="9c374-337">With *OnPost* and *Async* removed, the handler names are `JoinList` and `JoinListUC`.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?range=12-13)]

<span data-ttu-id="9c374-338">Kod önceki kullanarak, gönderildiği URL yolu `OnPostJoinListAsync` olduğu `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span><span class="sxs-lookup"><span data-stu-id="9c374-338">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span></span> <span data-ttu-id="9c374-339">Gönderildiği URL yolu `OnPostJoinListUCAsync` olduğu `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="9c374-339">The URL path that submits to `OnPostJoinListUCAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span></span>

## <a name="custom-routes"></a><span data-ttu-id="9c374-340">Özel yollar</span><span class="sxs-lookup"><span data-stu-id="9c374-340">Custom routes</span></span>

<span data-ttu-id="9c374-341">Kullanım `@page` yönergesini:</span><span class="sxs-lookup"><span data-stu-id="9c374-341">Use the `@page` directive to:</span></span>

* <span data-ttu-id="9c374-342">Bir sayfa için özel bir yol belirtin.</span><span class="sxs-lookup"><span data-stu-id="9c374-342">Specify a custom route to a page.</span></span> <span data-ttu-id="9c374-343">Rota hakkında sayfasına gibi ayarlanabilir `/Some/Other/Path` ile `@page "/Some/Other/Path"`.</span><span class="sxs-lookup"><span data-stu-id="9c374-343">For example, the route to the About page can be set to `/Some/Other/Path` with `@page "/Some/Other/Path"`.</span></span>
* <span data-ttu-id="9c374-344">Bir sayfanın varsayılan yol kesimleri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="9c374-344">Append segments to a page's default route.</span></span> <span data-ttu-id="9c374-345">Örneğin, bir "Item" segment ile bir sayfanın varsayılan rota eklenebilir `@page "item"`.</span><span class="sxs-lookup"><span data-stu-id="9c374-345">For example, an "item" segment can be added to a page's default route with `@page "item"`.</span></span>
* <span data-ttu-id="9c374-346">Bir sayfanın varsayılan rota için parametreleri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="9c374-346">Append parameters to a page's default route.</span></span> <span data-ttu-id="9c374-347">Örneğin, bir kimliği parametre `id`, içeren bir sayfa için gerekli olabilir `@page "{id}"`.</span><span class="sxs-lookup"><span data-stu-id="9c374-347">For example, an ID parameter, `id`, can be required for a page with `@page "{id}"`.</span></span>

<span data-ttu-id="9c374-348">Bir tilde işareti tarafından atanan bir köküne göreli yol (`~`) yolunun başında desteklenir.</span><span class="sxs-lookup"><span data-stu-id="9c374-348">A root-relative path designated by a tilde (`~`) at the beginning of the path is supported.</span></span> <span data-ttu-id="9c374-349">Örneğin, `@page "~/Some/Other/Path"` aynı `@page "/Some/Other/Path"`.</span><span class="sxs-lookup"><span data-stu-id="9c374-349">For example, `@page "~/Some/Other/Path"` is the same as `@page "/Some/Other/Path"`.</span></span>

<span data-ttu-id="9c374-350">Sorgu dizesi değiştirebilirsiniz `?handler=JoinList` yol kesimi URL'yi `/JoinList` rota şablonu belirterek `@page "{handler?}"`.</span><span class="sxs-lookup"><span data-stu-id="9c374-350">You can change the query string `?handler=JoinList` in the URL to a route segment `/JoinList` by specifying the route template `@page "{handler?}"`.</span></span>

<span data-ttu-id="9c374-351">Sorgu dizesi beğenmezseniz `?handler=JoinList` URL'de, yol işleyicisi adı URL'nin yol kısmı put değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9c374-351">If you don't like the query string `?handler=JoinList` in the URL, you can change the route to put the handler name in the path portion of the URL.</span></span> <span data-ttu-id="9c374-352">Rota sonra çift tırnak içine alınmış bir rota şablonu ekleyerek özelleştirebilirsiniz `@page` yönergesi.</span><span class="sxs-lookup"><span data-stu-id="9c374-352">You can customize the route by adding a route template enclosed in double quotes after the `@page` directive.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]

<span data-ttu-id="9c374-353">Kod önceki kullanarak, gönderildiği URL yolu `OnPostJoinListAsync` olduğu `http://localhost:5000/Customers/CreateFATH/JoinList`.</span><span class="sxs-lookup"><span data-stu-id="9c374-353">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `http://localhost:5000/Customers/CreateFATH/JoinList`.</span></span> <span data-ttu-id="9c374-354">Gönderildiği URL yolu `OnPostJoinListUCAsync` olduğu `http://localhost:5000/Customers/CreateFATH/JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="9c374-354">The URL path that submits to `OnPostJoinListUCAsync` is `http://localhost:5000/Customers/CreateFATH/JoinListUC`.</span></span>

<span data-ttu-id="9c374-355">`?` Aşağıdaki `handler` rota parametresinin isteğe bağlı olduğu anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="9c374-355">The `?` following `handler` means the route parameter is optional.</span></span>

## <a name="configuration-and-settings"></a><span data-ttu-id="9c374-356">Yapılandırma ve ayarlar</span><span class="sxs-lookup"><span data-stu-id="9c374-356">Configuration and settings</span></span>

<span data-ttu-id="9c374-357">Gelişmiş seçeneklerini yapılandırmak için genişletme yöntemini kullanmak `AddRazorPagesOptions` MVC oluşturucu üzerinde:</span><span class="sxs-lookup"><span data-stu-id="9c374-357">To configure advanced options, use the extension method `AddRazorPagesOptions` on the MVC builder:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/StartupAdvanced.cs?name=snippet_1)]

<span data-ttu-id="9c374-358">Şu anda kullanabileceğiniz `RazorPagesOptions` sayfa için kök dizine ayarlayın veya uygulama sayfaları için model kuralları ekleyin.</span><span class="sxs-lookup"><span data-stu-id="9c374-358">Currently you can use the `RazorPagesOptions` to set the root directory for pages, or add application model conventions for pages.</span></span> <span data-ttu-id="9c374-359">Daha fazla genişletilebilirlik bu şekilde gelecekte etkinleştiririz.</span><span class="sxs-lookup"><span data-stu-id="9c374-359">We'll enable more extensibility this way in the future.</span></span>

<span data-ttu-id="9c374-360">Görünümlerinizi önceden derleme için bkz: [Razor görünüm derlemesi](xref:mvc/views/view-compilation) .</span><span class="sxs-lookup"><span data-stu-id="9c374-360">To precompile views, see [Razor view compilation](xref:mvc/views/view-compilation) .</span></span>

<span data-ttu-id="9c374-361">[İndirme veya görüntüleme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/index/sample).</span><span class="sxs-lookup"><span data-stu-id="9c374-361">[Download or view sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/index/sample).</span></span>

<span data-ttu-id="9c374-362">Bkz: [Razor sayfaları kullanmaya başlama](xref:tutorials/razor-pages/razor-pages-start), ilgili bu girişi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="9c374-362">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start), which builds on this introduction.</span></span>

### <a name="specify-that-razor-pages-are-at-the-content-root"></a><span data-ttu-id="9c374-363">Razor sayfaları içerik kök dizininde olduğunu belirtin</span><span class="sxs-lookup"><span data-stu-id="9c374-363">Specify that Razor Pages are at the content root</span></span>

<span data-ttu-id="9c374-364">Varsayılan olarak, Razor sayfaları, köklü olmayan */sayfaları* dizin.</span><span class="sxs-lookup"><span data-stu-id="9c374-364">By default, Razor Pages are rooted in the */Pages* directory.</span></span> <span data-ttu-id="9c374-365">Ekleme [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) için [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) , Razor sayfaları içerik kök dizininde olduğunu belirtmek için ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) uygulama:</span><span class="sxs-lookup"><span data-stu-id="9c374-365">Add [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at the content root ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) of the app:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesAtContentRoot();
```

### <a name="specify-that-razor-pages-are-at-a-custom-root-directory"></a><span data-ttu-id="9c374-366">Razor sayfaları özel kök dizininde olduğunu belirtin</span><span class="sxs-lookup"><span data-stu-id="9c374-366">Specify that Razor Pages are at a custom root directory</span></span>

<span data-ttu-id="9c374-367">Ekleme [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) için [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) , Razor sayfaları bir uygulamadaki özel kök dizininde olduğunu belirtmek için (göreli bir yol belirtin):</span><span class="sxs-lookup"><span data-stu-id="9c374-367">Add [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at a custom root directory in the app (provide a relative path):</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesRoot("/path/to/razor/pages");
```

## <a name="see-also"></a><span data-ttu-id="9c374-368">Ayrıca bkz.</span><span class="sxs-lookup"><span data-stu-id="9c374-368">See also</span></span>

* [<span data-ttu-id="9c374-369">ASP.NET Core'a giriş</span><span class="sxs-lookup"><span data-stu-id="9c374-369">Introduction to ASP.NET Core</span></span>](xref:index)
* [<span data-ttu-id="9c374-370">Razor söz dizimi</span><span class="sxs-lookup"><span data-stu-id="9c374-370">Razor syntax</span></span>](xref:mvc/views/razor)
* [<span data-ttu-id="9c374-371">Razor Sayfaları kullanmaya başlama</span><span class="sxs-lookup"><span data-stu-id="9c374-371">Get started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="9c374-372">Razor sayfaları yetkilendirme kuralları</span><span class="sxs-lookup"><span data-stu-id="9c374-372">Razor Pages authorization conventions</span></span>](xref:security/authorization/razor-pages-authorization)
* [<span data-ttu-id="9c374-373">Razor sayfaları özel rota ve sayfa modeli sağlayıcılar</span><span class="sxs-lookup"><span data-stu-id="9c374-373">Razor Pages custom route and page model providers</span></span>](xref:razor-pages/razor-pages-conventions)
* [<span data-ttu-id="9c374-374">Razor Sayfaları birim testleri</span><span class="sxs-lookup"><span data-stu-id="9c374-374">Razor Pages unit tests</span></span>](xref:test/razor-pages-tests)
