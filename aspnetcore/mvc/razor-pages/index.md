---
title: ASP.NET Core Razor sayfalarında giriş
author: Rick-Anderson
description: Daha kolay ve MVC kullanmaktan daha üretken ASP.NET Core Razor sayfalarında kodlama sayfa odaklı senaryoları nasıl kolaylaştırdığını öğrenin.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 5/12/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: mvc/razor-pages/index
ms.openlocfilehash: d515e1354546e95553a010fa7a2143f0e529d68b
ms.sourcegitcommit: 63fb07fb3f71b32daf2c9466e132f2e7cc617163
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/10/2018
ms.locfileid: "35252457"
---
# <a name="introduction-to-razor-pages-in-aspnet-core"></a><span data-ttu-id="ae4b9-103">ASP.NET Core Razor sayfalarında giriş</span><span class="sxs-lookup"><span data-stu-id="ae4b9-103">Introduction to Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="ae4b9-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Ryan Nowak](https://github.com/rynowak)</span><span class="sxs-lookup"><span data-stu-id="ae4b9-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Ryan Nowak](https://github.com/rynowak)</span></span>

<span data-ttu-id="ae4b9-105">Razor sayfalarının yeni bir sayfa odaklı senaryoları daha kolay ve daha üretken kodlama yapar ASP.NET Core MVC durumuyla olur.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-105">Razor Pages is a new aspect of ASP.NET Core MVC that makes coding page-focused scenarios easier and more productive.</span></span>

<span data-ttu-id="ae4b9-106">Model-View-Controller yaklaşım kullanan bir öğretici için arıyorsanız bkz [ASP.NET Core MVC ile çalışmaya başlama](xref:tutorials/first-mvc-app/start-mvc).</span><span class="sxs-lookup"><span data-stu-id="ae4b9-106">If you're looking for a tutorial that uses the Model-View-Controller approach, see [Get started with ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span></span>

<span data-ttu-id="ae4b9-107">Bu belge Razor sayfalarının tanıtılmaktadır.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-107">This document provides an introduction to Razor Pages.</span></span> <span data-ttu-id="ae4b9-108">Adım adım öğretici değil.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-108">It's not a step by step tutorial.</span></span> <span data-ttu-id="ae4b9-109">Bazı çok Gelişmiş bölümleri bulamazsanız, bakın [Razor sayfalarının ile çalışmaya başlama](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="ae4b9-109">If you find some of the sections too advanced, see [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span></span> <span data-ttu-id="ae4b9-110">ASP.NET Core genel bakış için bkz: [ASP.NET Core giriş](xref:index).</span><span class="sxs-lookup"><span data-stu-id="ae4b9-110">For an overview of ASP.NET Core, see the [Introduction to ASP.NET Core](xref:index).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ae4b9-111">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="ae4b9-111">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs.md)]

<a name="rpvs17"></a>

## <a name="creating-a-razor-pages-project"></a><span data-ttu-id="ae4b9-112">Razor sayfalarının proje oluşturma</span><span class="sxs-lookup"><span data-stu-id="ae4b9-112">Creating a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ae4b9-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ae4b9-113">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="ae4b9-114">Bkz: [Razor sayfalarının ile çalışmaya başlama](xref:tutorials/razor-pages/razor-pages-start) Razor sayfalarının oluşturma konusunda ayrıntılı yönergeler için Proje Visual Studio kullanarak.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-114">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) for detailed instructions on how to create a Razor Pages project using Visual Studio.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="ae4b9-115">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ae4b9-115">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="ae4b9-116">Çalıştırma `dotnet new webapp` komut satırından.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-116">Run `dotnet new webapp` from the command line.</span></span>

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="ae4b9-118">Çalıştırma `dotnet new razor` komut satırından.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-118">Run `dotnet new razor` from the command line.</span></span>

::: moniker-end

<span data-ttu-id="ae4b9-119">Oluşturulan açmak *.csproj* Visual Studio dosyasından Mac için</span><span class="sxs-lookup"><span data-stu-id="ae4b9-119">Open the generated *.csproj* file from Visual Studio for Mac.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ae4b9-120">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ae4b9-120">Visual Studio Code</span></span>](#tab/visual-studio-code) 

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="ae4b9-121">Çalıştırma `dotnet new webapp` komut satırından.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-121">Run `dotnet new webapp` from the command line.</span></span>

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="ae4b9-123">Çalıştırma `dotnet new razor` komut satırından.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-123">Run `dotnet new razor` from the command line.</span></span>

::: moniker-end

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="ae4b9-124">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="ae4b9-124">.NET Core CLI</span></span>](#tab/netcore-cli) 

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="ae4b9-125">Çalıştırma `dotnet new webapp` komut satırından.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-125">Run `dotnet new webapp` from the command line.</span></span>

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="ae4b9-127">Çalıştırma `dotnet new razor` komut satırından.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-127">Run `dotnet new razor` from the command line.</span></span>

::: moniker-end

---

## <a name="razor-pages"></a><span data-ttu-id="ae4b9-128">Razor sayfalarının</span><span class="sxs-lookup"><span data-stu-id="ae4b9-128">Razor Pages</span></span>

<span data-ttu-id="ae4b9-129">Razor sayfalarının etkinleşir *haline*:</span><span class="sxs-lookup"><span data-stu-id="ae4b9-129">Razor Pages is enabled in *Startup.cs*:</span></span>

[!code-cs[](index/sample/RazorPagesIntro/Startup.cs?name=snippet_Startup)]

<span data-ttu-id="ae4b9-130">Temel bir sayfa göz önünde bulundurun: <a name="OnGet"></a></span><span class="sxs-lookup"><span data-stu-id="ae4b9-130">Consider a basic page: <a name="OnGet"></a></span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index.cshtml)]

<span data-ttu-id="ae4b9-131">Önceki kod çok Razor görünüm dosyası gibi görünüyor.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-131">The preceding code looks a lot like a Razor view file.</span></span> <span data-ttu-id="ae4b9-132">Neler farklı kılan unsurdur `@page` yönergesi.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-132">What makes it different is the `@page` directive.</span></span> <span data-ttu-id="ae4b9-133">`@page` Dosya, isteklerini doğrudan bir denetleyici geçmeden işleme anlamına gelir ve MVC eylemi - içine yapar.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-133">`@page` makes the file into an MVC action - which means that it handles requests directly, without going through a controller.</span></span> <span data-ttu-id="ae4b9-134">`@page` ilk Razor yönergesi bir sayfa üzerinde olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-134">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="ae4b9-135">`@page` diğer Razor yapıları davranışını etkiler.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-135">`@page` affects the behavior of other Razor constructs.</span></span>

<span data-ttu-id="ae4b9-136">Benzer bir sayfa, kullanarak bir `PageModel` sınıfında, aşağıdaki iki dosyada gösterilir.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-136">A similar page, using a `PageModel` class, is shown in the following two files.</span></span> <span data-ttu-id="ae4b9-137">*Pages/Index2.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="ae4b9-137">The *Pages/Index2.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index2.cshtml)]

<span data-ttu-id="ae4b9-138">*Pages/Index2.cshtml.cs* sayfa modeli:</span><span class="sxs-lookup"><span data-stu-id="ae4b9-138">The *Pages/Index2.cshtml.cs* page model:</span></span>

[!code-cs[](index/sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]

<span data-ttu-id="ae4b9-139">Kural tarafından `PageModel` sınıf dosyası Razor sayfa dosyası ile aynı ada sahip *.cs* eklenir.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-139">By convention, the `PageModel` class file has the same name as the Razor Page file with *.cs* appended.</span></span> <span data-ttu-id="ae4b9-140">Örneğin, önceki Razor sayfasıdır *Pages/Index2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-140">For example, the previous Razor Page is *Pages/Index2.cshtml*.</span></span> <span data-ttu-id="ae4b9-141">Dosyayı içeren `PageModel` sınıfı adlandırılan *Pages/Index2.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-141">The file containing the `PageModel` class is named *Pages/Index2.cshtml.cs*.</span></span>

<span data-ttu-id="ae4b9-142">URL yollarını sayfalara ilişkilendirmelerini dosya sisteminde sayfanın konuma göre belirlenir.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-142">The associations of URL paths to pages are determined by the page's location in the file system.</span></span> <span data-ttu-id="ae4b9-143">Aşağıdaki tablo Razor sayfasının yolunu ve eşleşen URL gösterir:</span><span class="sxs-lookup"><span data-stu-id="ae4b9-143">The following table shows a Razor Page path and the matching URL:</span></span>

| <span data-ttu-id="ae4b9-144">Dosya adı ve yolu</span><span class="sxs-lookup"><span data-stu-id="ae4b9-144">File name and path</span></span>               | <span data-ttu-id="ae4b9-145">URL eşleştirme</span><span class="sxs-lookup"><span data-stu-id="ae4b9-145">matching URL</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="ae4b9-146">*/Pages/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="ae4b9-146">*/Pages/Index.cshtml*</span></span> | <span data-ttu-id="ae4b9-147">`/` Veya `/Index`</span><span class="sxs-lookup"><span data-stu-id="ae4b9-147">`/` or `/Index`</span></span> |
| <span data-ttu-id="ae4b9-148">*/Pages/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="ae4b9-148">*/Pages/Contact.cshtml*</span></span> | `/Contact` |
| <span data-ttu-id="ae4b9-149">*/Pages/Store/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="ae4b9-149">*/Pages/Store/Contact.cshtml*</span></span> | `/Store/Contact` |
| <span data-ttu-id="ae4b9-150">*/Pages/Store/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="ae4b9-150">*/Pages/Store/Index.cshtml*</span></span> | <span data-ttu-id="ae4b9-151">`/Store` Veya `/Store/Index`</span><span class="sxs-lookup"><span data-stu-id="ae4b9-151">`/Store` or `/Store/Index`</span></span> |

<span data-ttu-id="ae4b9-152">Notlar:</span><span class="sxs-lookup"><span data-stu-id="ae4b9-152">Notes:</span></span>

* <span data-ttu-id="ae4b9-153">Razor sayfalarının dosyalarında çalışma zamanı arar *sayfaları* varsayılan klasör.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-153">The runtime looks for Razor Pages files in the *Pages* folder by default.</span></span>
* <span data-ttu-id="ae4b9-154">`Index` bir URL bir sayfa içermediğinde varsayılan sayfasıdır.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-154">`Index` is the default page when a URL doesn't include a page.</span></span>

## <a name="writing-a-basic-form"></a><span data-ttu-id="ae4b9-155">Temel form yazma</span><span class="sxs-lookup"><span data-stu-id="ae4b9-155">Writing a basic form</span></span>

<span data-ttu-id="ae4b9-156">Razor sayfalarının web tarayıcılarıyla birlikte kolay bir uygulama oluştururken uygulamak için kullanılan ortak desenler olmak için tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-156">Razor Pages is designed to make common patterns used with web browsers easy to implement when building an app.</span></span> <span data-ttu-id="ae4b9-157">[Model bağlama](xref:mvc/models/model-binding), [etiket Yardımcıları](xref:mvc/views/tag-helpers/intro)ve HTML Yardımcıları tüm *yalnızca iş* Razor sayfasını sınıfında tanımlanan özelliklere sahip.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-157">[Model binding](xref:mvc/models/model-binding), [Tag Helpers](xref:mvc/views/tag-helpers/intro), and HTML helpers all *just work* with the properties defined in a Razor Page class.</span></span> <span data-ttu-id="ae4b9-158">"Bize başvurun" form için temel bir uygulayan bir sayfa göz önünde bulundurun `Contact` modeli:</span><span class="sxs-lookup"><span data-stu-id="ae4b9-158">Consider a page that implements a basic "contact us" form for the `Contact` model:</span></span>

<span data-ttu-id="ae4b9-159">Bu belgede, örnekleri için `DbContext` içinde başlatılan [haline](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) dosya.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-159">For the samples in this document, the `DbContext` is initialized in the [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) file.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Startup.cs?highlight=15-16)]

<span data-ttu-id="ae4b9-160">Veri modeli:</span><span class="sxs-lookup"><span data-stu-id="ae4b9-160">The data model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Data/Customer.cs)]

<span data-ttu-id="ae4b9-161">Db bağlamı:</span><span class="sxs-lookup"><span data-stu-id="ae4b9-161">The db context:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Data/AppDbContext.cs)]

<span data-ttu-id="ae4b9-162">*Pages/Create.cshtml* dosyasını görüntüle:</span><span class="sxs-lookup"><span data-stu-id="ae4b9-162">The *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml)]

<span data-ttu-id="ae4b9-163">*Pages/Create.cshtml.cs* sayfa modeli:</span><span class="sxs-lookup"><span data-stu-id="ae4b9-163">The *Pages/Create.cshtml.cs* page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_ALL)]

<span data-ttu-id="ae4b9-164">Kural tarafından `PageModel` sınıfı çağrıldığında `<PageName>Model` ve sayfa aynı ad.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-164">By convention, the `PageModel` class is called `<PageName>Model` and is in the same namespace as the page.</span></span>

<span data-ttu-id="ae4b9-165">`PageModel` Sınıfı, bir sayfa mantığı ayrılması kendi sunudan olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-165">The `PageModel` class allows separation of the logic of a page from its presentation.</span></span> <span data-ttu-id="ae4b9-166">Sayfaya gönderilen istekleri ve sayfayı oluşturmak için kullanılan verileri için sayfa işleyiciler tanımlar.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-166">It defines page handlers for requests sent to the page and the data used to render the page.</span></span> <span data-ttu-id="ae4b9-167">Bu ayrım aracılığıyla sayfa bağımlılıklar yönetmenize olanak sağlayan [bağımlılık ekleme](xref:fundamentals/dependency-injection) ve [birim testi](xref:test/razor-pages-tests) sayfaları.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-167">This separation allows you to manage page dependencies through [dependency injection](xref:fundamentals/dependency-injection) and to [unit test](xref:test/razor-pages-tests) the pages.</span></span>

<span data-ttu-id="ae4b9-168">Sayfasına sahip bir `OnPostAsync` *işleyici yöntemi*, üzerinde çalıştığı `POST` ister (kullanıcı formu gönderdiğinde).</span><span class="sxs-lookup"><span data-stu-id="ae4b9-168">The page has an `OnPostAsync` *handler method*, which runs on `POST` requests (when a user posts the form).</span></span> <span data-ttu-id="ae4b9-169">Herhangi bir HTTP fiil için işleyici yöntemleri ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-169">You can add handler methods for any HTTP verb.</span></span> <span data-ttu-id="ae4b9-170">En yaygın işleyicileri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="ae4b9-170">The most common handlers are:</span></span>

* <span data-ttu-id="ae4b9-171">`OnGet` sayfa için gereken durumu başlatılamadı.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-171">`OnGet` to initialize state needed for the page.</span></span> <span data-ttu-id="ae4b9-172">[OnGet](#OnGet) örnek.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-172">[OnGet](#OnGet) sample.</span></span>
* <span data-ttu-id="ae4b9-173">`OnPost` Form Gönderme işlemek için.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-173">`OnPost` to handle form submissions.</span></span>

<span data-ttu-id="ae4b9-174">`Async` Adlandırma soneki isteğe bağlıdır ancak genellikle kurala göre zaman uyumsuz işlevleri için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-174">The `Async` naming suffix is optional but is often used by convention for asynchronous functions.</span></span> <span data-ttu-id="ae4b9-175">`OnPostAsync` Önceki örnekte kod ne, normalde bir denetleyicisi yazarsınız için benzer görünür.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-175">The `OnPostAsync` code in the preceding example looks similar to what you would normally write in a controller.</span></span> <span data-ttu-id="ae4b9-176">Önceki kod Razor sayfalar için normaldir.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-176">The preceding code is typical for Razor Pages.</span></span> <span data-ttu-id="ae4b9-177">MVC elemanlar çoğu ister [model bağlama](xref:mvc/models/model-binding), [doğrulama](xref:mvc/models/validation), ve eylem sonuçlarını paylaşıldığı.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-177">Most of the MVC primitives like [model binding](xref:mvc/models/model-binding), [validation](xref:mvc/models/validation), and action results are shared.</span></span>  <!-- Review: Ryan, can we get a list of what is shared and what isn't? -->

<span data-ttu-id="ae4b9-178">Önceki `OnPostAsync` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="ae4b9-178">The previous `OnPostAsync` method:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="ae4b9-179">Temel akışı `OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="ae4b9-179">The basic flow of `OnPostAsync`:</span></span>

<span data-ttu-id="ae4b9-180">Doğrulama hataları denetleyin.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-180">Check for validation errors.</span></span>

*  <span data-ttu-id="ae4b9-181">Herhangi bir hata varsa, verileri Kaydet ve yeniden yönlendirme.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-181">If there are no errors, save the data and redirect.</span></span>
*  <span data-ttu-id="ae4b9-182">Hatalar varsa, doğrulama iletileri sayfasıyla yeniden gösterir.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-182">If there are errors, show the page again with validation messages.</span></span> <span data-ttu-id="ae4b9-183">İstemci tarafı doğrulama geleneksel ASP.NET Core MVC uygulamaları için aynıdır.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-183">Client-side validation is identical to traditional ASP.NET Core MVC applications.</span></span> <span data-ttu-id="ae4b9-184">Çoğu durumda, doğrulama hataları istemcide algılanabilir ve hiçbir zaman sunucuya gönderildi.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-184">In many cases, validation errors would be detected on the client, and never submitted to the server.</span></span>

<span data-ttu-id="ae4b9-185">Veri başarıyla girildiğinde `OnPostAsync` işleyici yöntem çağrılarını `RedirectToPage` bir örneğini döndürmek için yardımcı yöntem `RedirectToPageResult`.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-185">When the data is entered successfully, the `OnPostAsync` handler method calls the `RedirectToPage` helper method to return an instance of `RedirectToPageResult`.</span></span> <span data-ttu-id="ae4b9-186">`RedirectToPage` Yeni eylem sonucu, benzer olduğunu `RedirectToAction` veya `RedirectToRoute`, ancak özelleştirilmiş sayfalar için.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-186">`RedirectToPage` is a new action result, similar to `RedirectToAction` or `RedirectToRoute`, but customized for pages.</span></span> <span data-ttu-id="ae4b9-187">İsteğe bağlı olarak önceki örnekte kök dizin sayfasına yönlendiren (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="ae4b9-187">In the preceding sample, it redirects to the root Index page (`/Index`).</span></span> <span data-ttu-id="ae4b9-188">`RedirectToPage` içinde ayrıntılı [sayfaları için URL oluşturma](#url_gen) bölümü.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-188">`RedirectToPage` is detailed in the [URL generation for Pages](#url_gen) section.</span></span>

<span data-ttu-id="ae4b9-189">Teslim edilen formu (yani sunucuya geçirilir) doğrulama hataları olduğunda`OnPostAsync` işleyici yöntem çağrılarını `Page` yardımcı yöntemi.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-189">When the submitted form has validation errors (that are passed to the server), the`OnPostAsync` handler method calls the `Page` helper method.</span></span> <span data-ttu-id="ae4b9-190">`Page` örneğini döndürür `PageResult`.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-190">`Page` returns an instance of `PageResult`.</span></span> <span data-ttu-id="ae4b9-191">Döndürme `Page` nasıl denetleyicileri Eylemler döndürmek için benzer `View`.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-191">Returning `Page` is similar to how actions in controllers return `View`.</span></span> <span data-ttu-id="ae4b9-192">`PageResult` Varsayılan değer <!-- Review  --> dönüş türü için bir işleyici yöntemi.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-192">`PageResult` is the default <!-- Review  --> return type for a handler method.</span></span> <span data-ttu-id="ae4b9-193">Döndüren bir işleyici yöntemi `void` sayfasını işler.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-193">A handler method that returns `void` renders the page.</span></span>

<span data-ttu-id="ae4b9-194">`Customer` Özelliğini kullanan `[BindProperty]` model bağlama için kabul özniteliği.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-194">The `Customer` property uses `[BindProperty]` attribute to opt in to model binding.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_PageModel&highlight=10-11)]

<span data-ttu-id="ae4b9-195">Razor sayfalarının varsayılan olarak, GET olmayan fiiller yalnızca özelliklerle bağlayın.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-195">Razor Pages, by default, bind properties only with non-GET verbs.</span></span> <span data-ttu-id="ae4b9-196">Bağlama özellikleri için kod yazmak zorunda miktarını azaltır.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-196">Binding to properties can reduce the amount of code you have to write.</span></span> <span data-ttu-id="ae4b9-197">Bağlama form alanları oluşturmak için aynı özelliği kullanarak kod azaltır (`<input asp-for="Customer.Name" />`) ve giriş kabul edin.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-197">Binding reduces code by using the same property to render form fields (`<input asp-for="Customer.Name" />`) and accept the input.</span></span>

> [!NOTE]
> <span data-ttu-id="ae4b9-198">Güvenlik nedenleriyle, sayfa model özellikleri GET isteği veri bağlaması kabul gerekir.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-198">For security reasons, you must opt in to binding GET request data to page model properties.</span></span> <span data-ttu-id="ae4b9-199">Kullanıcı girişi özelliklere eşleme önce doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-199">Verify user input before mapping it to properties.</span></span> <span data-ttu-id="ae4b9-200">Bu davranış seçim sorgu dizesi veya rota değerlerine dayanan senaryoları adresleme yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-200">Opting in to this behavior is useful when addressing scenarios which rely on query string or route values.</span></span>
>
> <span data-ttu-id="ae4b9-201">Özellik GET isteklerinde bağlamak için ayarlanmış `[BindProperty]` özniteliğin `SupportsGet` özelliğine `true`: `[BindProperty(SupportsGet = true)]`</span><span class="sxs-lookup"><span data-stu-id="ae4b9-201">To bind a property on GET requests, set the `[BindProperty]` attribute's `SupportsGet` property to `true`: `[BindProperty(SupportsGet = true)]`</span></span>

<span data-ttu-id="ae4b9-202">Giriş sayfası (*Index.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="ae4b9-202">The home page (*Index.cshtml*):</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml)]

<span data-ttu-id="ae4b9-203">Arka plan kod *Index.cshtml.cs* dosyası:</span><span class="sxs-lookup"><span data-stu-id="ae4b9-203">The code behind *Index.cshtml.cs* file:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs)]

<span data-ttu-id="ae4b9-204">*Index.cshtml* dosyası her kişi için bir düzenleme bağlantısı oluşturmak için aşağıdaki biçimlendirme içerir:</span><span class="sxs-lookup"><span data-stu-id="ae4b9-204">The *Index.cshtml* file contains the following markup to create an edit link for each contact:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=21)]

<span data-ttu-id="ae4b9-205">[Yer işareti etiketi yardımcı](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) kullanılan `asp-route-{value}` düzenleme sayfasını bir bağlantı oluşturmak için öznitelik.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-205">The [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) used the `asp-route-{value}` attribute to generate a link to the Edit page.</span></span> <span data-ttu-id="ae4b9-206">Rota verilerini kişiyle bağlantıyı içeren kimliği</span><span class="sxs-lookup"><span data-stu-id="ae4b9-206">The link contains route data with the contact ID.</span></span> <span data-ttu-id="ae4b9-207">Örneğin, `http://localhost:5000/Edit/1`.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-207">For example, `http://localhost:5000/Edit/1`.</span></span>

<span data-ttu-id="ae4b9-208">*Pages/Edit.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="ae4b9-208">The *Pages/Edit.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Edit.cshtml?highlight=1)]

<span data-ttu-id="ae4b9-209">İlk satır içerir `@page "{id:int}"` yönergesi.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-209">The first line contains the `@page "{id:int}"` directive.</span></span> <span data-ttu-id="ae4b9-210">Yönlendirme kısıtlaması`"{id:int}"` içeren sayfa isteklerini kabul etmek için sayfayı söyler `int` rota verileri.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-210">The routing constraint`"{id:int}"` tells the page to accept requests to the page that contain `int` route data.</span></span> <span data-ttu-id="ae4b9-211">Sayfa için bir istek dönüştürülebilir rota verileri içermiyorsa, bir `int`, çalışma zamanı (bulunamadı) HTTP 404 hatası döndürür.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-211">If a request to the page doesn't contain route data that can be converted to an `int`, the runtime returns an HTTP 404 (not found) error.</span></span> <span data-ttu-id="ae4b9-212">Kimliği isteğe bağlı yapmak için ekleme `?` rota kısıtlaması için:</span><span class="sxs-lookup"><span data-stu-id="ae4b9-212">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="ae4b9-213">*Pages/Edit.cshtml.cs* dosyası:</span><span class="sxs-lookup"><span data-stu-id="ae4b9-213">The *Pages/Edit.cshtml.cs* file:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Edit.cshtml.cs)]

<span data-ttu-id="ae4b9-214">*Index.cshtml* dosya de Sil düğmesini her müşteri irtibat için oluşturmak için biçimlendirme içerir:</span><span class="sxs-lookup"><span data-stu-id="ae4b9-214">The *Index.cshtml* file also contains markup to create a delete button for each customer contact:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=22-23)]

<span data-ttu-id="ae4b9-215">Sil düğmesini HTML olarak işlendiğinde, `formaction` parametreleri içerir:</span><span class="sxs-lookup"><span data-stu-id="ae4b9-215">When the delete button is rendered in HTML, its `formaction` includes parameters for:</span></span>

* <span data-ttu-id="ae4b9-216">Müşteri tarafından belirtilen kimliği başvurun `asp-route-id` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-216">The customer contact ID specified by the `asp-route-id` attribute.</span></span>
* <span data-ttu-id="ae4b9-217">`handler` Tarafından belirtilen `asp-page-handler` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-217">The `handler` specified by the `asp-page-handler` attribute.</span></span>

<span data-ttu-id="ae4b9-218">Bir müşteri ile işlenen Sil düğmesini bir örneği burada verilmiştir Kimliğini başvurun `1`:</span><span class="sxs-lookup"><span data-stu-id="ae4b9-218">Here is an example of a rendered delete button with a customer contact ID of `1`:</span></span>

```html
<button type="submit" formaction="/?id=1&amp;handler=delete">delete</button>
```

<span data-ttu-id="ae4b9-219">Düğme seçildiğinde, bir form `POST` isteği sunucuya gönderilir.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-219">When the button is selected, a form `POST` request is sent to the server.</span></span> <span data-ttu-id="ae4b9-220">Kurala göre işleyicisi yönteminin adı seçili değerine göre `handler` parametre düzeni göre `OnPost[handler]Async`.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-220">By convention, the name of the handler method is selected based the value of the `handler` parameter according to the scheme `OnPost[handler]Async`.</span></span>

<span data-ttu-id="ae4b9-221">Çünkü `handler` olan `delete` Bu örnekte, `OnPostDeleteAsync` işleyici yöntemi kullanılır işleme `POST` isteği.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-221">Because the `handler` is `delete` in this example, the `OnPostDeleteAsync` handler method is used to process the `POST` request.</span></span> <span data-ttu-id="ae4b9-222">Varsa `asp-page-handler` gibi farklı bir değere ayarlanmış `remove`, ada sahip bir sayfa işleyici yöntemi `OnPostRemoveAsync` seçilir.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-222">If the `asp-page-handler` is set to a different value, such as `remove`, a page handler method with the name `OnPostRemoveAsync` is selected.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs?range=26-37)]

<span data-ttu-id="ae4b9-223">`OnPostDeleteAsync` Yöntemi:</span><span class="sxs-lookup"><span data-stu-id="ae4b9-223">The `OnPostDeleteAsync` method:</span></span>

* <span data-ttu-id="ae4b9-224">Kabul `id` Sorgu dizesinden.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-224">Accepts the `id` from the query string.</span></span>
* <span data-ttu-id="ae4b9-225">Müşteri irtibat ile veritabanını sorgular `FindAsync`.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-225">Queries the database for the customer contact with `FindAsync`.</span></span>
* <span data-ttu-id="ae4b9-226">Müşteri irtibat bulunursa, müşteri kişiler listesinden kaldırıldı.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-226">If the customer contact is found, they're removed from the list of customer contacts.</span></span> <span data-ttu-id="ae4b9-227">Veritabanı güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-227">The database is updated.</span></span>
* <span data-ttu-id="ae4b9-228">Çağrıları `RedirectToPage` kök dizin sayfasına yeniden yönlendirmek için (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="ae4b9-228">Calls `RedirectToPage` to redirect to the root Index page (`/Index`).</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="mark-page-properties-required"></a><span data-ttu-id="ae4b9-229">Gerekli işareti sayfa özellikleri</span><span class="sxs-lookup"><span data-stu-id="ae4b9-229">Mark page properties required</span></span>

<span data-ttu-id="ae4b9-230">Özellikleri bir `PageModel` ile donatılmış [gerekli](/dotnet/api/system.componentmodel.dataannotations.requiredattribute) özniteliği:</span><span class="sxs-lookup"><span data-stu-id="ae4b9-230">Properties on a `PageModel` can be decorated with the [Required](/dotnet/api/system.componentmodel.dataannotations.requiredattribute) attribute:</span></span>

<span data-ttu-id="ae4b9-231">[!code-cs[](index/sample/Create.cshtml.cs?highlight=3,15-16)]</span><span class="sxs-lookup"><span data-stu-id="ae4b9-231">[!code-cs[](index/sample/Create.cshtml.cs?highlight=3,15-16)]</span></span>

<span data-ttu-id="ae4b9-232">Bkz: [Model doğrulama](xref:mvc/models/validation) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-232">See [Model validation](xref:mvc/models/validation) for more information.</span></span>

## <a name="manage-head-requests-with-the-onget-handler"></a><span data-ttu-id="ae4b9-233">HEAD isteklerini OnGet işleyici ile yönetme</span><span class="sxs-lookup"><span data-stu-id="ae4b9-233">Manage HEAD requests with the OnGet handler</span></span>

<span data-ttu-id="ae4b9-234">Normalde, HEAD işleyici oluşturulur ve HEAD isteklerini çağrılır:</span><span class="sxs-lookup"><span data-stu-id="ae4b9-234">Ordinarily, a HEAD handler is created and called for HEAD requests:</span></span>

```csharp
public void OnHead()
{
    HttpContext.Response.Headers.Add("HandledBy", "Handled by OnHead!");
}
```

<span data-ttu-id="ae4b9-235">Hiçbir HEAD işleyici varsa (`OnHead`) olan tanımlı, Razor sayfalarının geri alma sayfası işleyicisi çağırma için döner (`OnGet`) ASP.NET Core 2.1 veya üzeri.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-235">If no HEAD handler (`OnHead`) is defined, Razor Pages falls back to calling the GET page handler (`OnGet`) in ASP.NET Core 2.1 or later.</span></span> <span data-ttu-id="ae4b9-236">Bu davranışı ile için kabul [SetCompatibilityVersion yöntemi](xref:fundamentals/startup#setcompatibilityversion-for-aspnet-core-mvc) içinde `Startup.Configure` ASP.NET Core 2.1 2.x için:</span><span class="sxs-lookup"><span data-stu-id="ae4b9-236">Opt in to this behavior with the [SetCompatibilityVersion method](xref:fundamentals/startup#setcompatibilityversion-for-aspnet-core-mvc) in `Startup.Configure` for ASP.NET Core 2.1 to 2.x:</span></span>

```csharp
services.AddMvc()
    .SetCompatibilityVersion(Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_1);
```

<span data-ttu-id="ae4b9-237">`SetCompatibilityVersion` Razor sayfalarının seçeneği etkin ayarlar `AllowMappingHeadRequestsToGetHandler` için `true`.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-237">`SetCompatibilityVersion` effectively sets the Razor Pages option `AllowMappingHeadRequestsToGetHandler` to `true`.</span></span>

<span data-ttu-id="ae4b9-238">Tüm 2.1 davranışlarla içine kullanmama yerine `SetCompatibilityVersion`, siz açıkça belirli davranışları katılımı.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-238">Rather than opting into all 2.1 behaviors with `SetCompatibilityVersion`, you can explicitly opt-in to specific behaviors.</span></span> <span data-ttu-id="ae4b9-239">Aşağıdaki kod içine GET işleyici eşlemesi HEAD isteklerini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-239">The following code opts into the mapping HEAD requests to the GET handler.</span></span>


```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        options.AllowMappingHeadRequestsToGetHandler = true;
    });
```
::: moniker-end

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a><span data-ttu-id="ae4b9-240">XSRF/CSRF ve Razor sayfaları</span><span class="sxs-lookup"><span data-stu-id="ae4b9-240">XSRF/CSRF and Razor Pages</span></span>

<span data-ttu-id="ae4b9-241">Herhangi bir kod yazmak zorunda değilsiniz [antiforgery doğrulama](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="ae4b9-241">You don't have to write any code for [antiforgery validation](xref:security/anti-request-forgery).</span></span> <span data-ttu-id="ae4b9-242">Otomatik olarak antiforgery belirteci oluşturma ve doğrulama Razor sayfalarında dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-242">Antiforgery token generation and validation are automatically included in Razor Pages.</span></span>

<a name="layout"></a>
## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a><span data-ttu-id="ae4b9-243">Düzenleri, kısmi, şablonları ve etiket Yardımcıları Razor Pages ile kullanma</span><span class="sxs-lookup"><span data-stu-id="ae4b9-243">Using Layouts, partials, templates, and Tag Helpers with Razor Pages</span></span>

<span data-ttu-id="ae4b9-244">Sayfaları Razor görüntüleme altyapısı tüm özellikleri çalışır.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-244">Pages work with all the capabilities of the Razor view engine.</span></span> <span data-ttu-id="ae4b9-245">Düzenleri, kısmi, şablonları, etiket Yardımcıları *_ViewStart.cshtml*, *_viewımports.cshtml* iş için geleneksel Razor görünümleri yaparlar aynı şekilde.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-245">Layouts, partials, templates, Tag Helpers, *_ViewStart.cshtml*, *_ViewImports.cshtml* work in the same way they do for conventional Razor views.</span></span>

<span data-ttu-id="ae4b9-246">Şimdi bu sayfa bu özelliklerden bazıları yararlanarak declutter.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-246">Let's declutter this page by taking advantage of some of those capabilities.</span></span>

<span data-ttu-id="ae4b9-247">Ekleme bir [düzen sayfası](xref:mvc/views/layout) için *Pages/_Layout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="ae4b9-247">Add a [layout page](xref:mvc/views/layout) to *Pages/_Layout.cshtml*:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_LayoutSimple.cshtml)]

<span data-ttu-id="ae4b9-248">[Düzeni](xref:mvc/views/layout):</span><span class="sxs-lookup"><span data-stu-id="ae4b9-248">The [Layout](xref:mvc/views/layout):</span></span>

* <span data-ttu-id="ae4b9-249">(Sayfa düzeni dışında çevrilir sürece) her sayfanın düzenini denetler.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-249">Controls the layout of each page (unless the page opts out of layout).</span></span>
* <span data-ttu-id="ae4b9-250">JavaScript ve stil sayfalarını gibi HTML yapıları alır.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-250">Imports HTML structures such as JavaScript and stylesheets.</span></span>

<span data-ttu-id="ae4b9-251">Bkz: [düzen sayfası](xref:mvc/views/layout) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-251">See [layout page](xref:mvc/views/layout) for more information.</span></span>

<span data-ttu-id="ae4b9-252">[Düzeni](xref:mvc/views/layout#specifying-a-layout) özelliği ayarlanmış *Pages/_ViewStart.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="ae4b9-252">The [Layout](xref:mvc/views/layout#specifying-a-layout) property is set in *Pages/_ViewStart.cshtml*:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]

<span data-ttu-id="ae4b9-253">Düzen bulunduğu *sayfaları* klasör.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-253">The layout is in the *Pages* folder.</span></span> <span data-ttu-id="ae4b9-254">Sayfaları başka görünümlerini (düzenleri, şablonlar, kısmi), geçerli sayfa ile aynı klasörde başlangıç hiyerarşik olarak arayın.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-254">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="ae4b9-255">Bir düzende *sayfaları* klasörü altında herhangi bir Razor sayfadan kullanılabilir *sayfaları* klasör.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-255">A layout in the *Pages* folder can be used from any Razor page under the *Pages* folder.</span></span>

<span data-ttu-id="ae4b9-256">Öneririz **değil** Düzen dosyası içine *görünümler/paylaşılan* klasör.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-256">We recommend you **not** put the layout file in the *Views/Shared* folder.</span></span> <span data-ttu-id="ae4b9-257">*Görünümler/paylaşılan* bir MVC görünümleri deseni.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-257">*Views/Shared* is an MVC views pattern.</span></span> <span data-ttu-id="ae4b9-258">Razor sayfalarının klasör hiyerarşisi, yol kuralları yararlanmayı yöneliktir.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-258">Razor Pages are meant to rely on folder hierarchy, not path conventions.</span></span>

<span data-ttu-id="ae4b9-259">Bir Razor sayfa görünümü aramadan içerir *sayfaları* klasör.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-259">View search from a Razor Page includes the *Pages* folder.</span></span> <span data-ttu-id="ae4b9-260">Düzenleri, şablonları ve MVC denetleyicileri ve geleneksel Razor görünümleri ile kullanmakta olduğunuz kısmi *yalnızca iş*.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-260">The layouts, templates, and partials you're using with MVC controllers and conventional Razor views *just work*.</span></span>

<span data-ttu-id="ae4b9-261">Ekleme bir *Pages/_ViewImports.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="ae4b9-261">Add a *Pages/_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]

<span data-ttu-id="ae4b9-262">`@namespace` Daha sonra öğreticide açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-262">`@namespace` is explained later in the tutorial.</span></span> <span data-ttu-id="ae4b9-263">`@addTagHelper` Yönergesi getirir [yerleşik etiket Yardımcıları](xref:mvc/views/tag-helpers/builtin-th/Index) tüm sayfalar için *sayfaları* klasör.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-263">The `@addTagHelper` directive brings in the [built-in Tag Helpers](xref:mvc/views/tag-helpers/builtin-th/Index) to all the pages in the *Pages* folder.</span></span>

<a name="namespace"></a>

<span data-ttu-id="ae4b9-264">Zaman `@namespace` yönergesi açıkça bir sayfada kullanılır:</span><span class="sxs-lookup"><span data-stu-id="ae4b9-264">When the `@namespace` directive is used explicitly on a page:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]

<span data-ttu-id="ae4b9-265">Yönergesi sayfa için ad alanı ayarlar.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-265">The directive sets the namespace for the page.</span></span> <span data-ttu-id="ae4b9-266">`@model` Yönergesi olmayan ad alanı içerecek şekilde gerekir.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-266">The `@model` directive doesn't need to include the namespace.</span></span>

<span data-ttu-id="ae4b9-267">Zaman `@namespace` yönergesi bulunduğu *_viewımports.cshtml*, belirtilen ad alanı içe aktaran sayfasındaki oluşturulan ad alanı öneki sağlayan `@namespace` yönergesi.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-267">When the `@namespace` directive is contained in *_ViewImports.cshtml*, the specified namespace supplies the prefix for the generated namespace in the Page that imports the `@namespace` directive.</span></span> <span data-ttu-id="ae4b9-268">Oluşturulan ad alanı (soneki bölümü) kalanı içeren klasör arasında nokta ayrılmış göreli yol olup *_viewımports.cshtml* ve sayfayı içeren klasör.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-268">The rest of the generated namespace (the suffix portion) is the dot-separated relative path between the folder containing *_ViewImports.cshtml* and the folder containing the page.</span></span>

<span data-ttu-id="ae4b9-269">Örneğin, dosyanın arkasındaki kod *Pages/Customers/Edit.cshtml.cs* ad alanını açıkça ayarlar:</span><span class="sxs-lookup"><span data-stu-id="ae4b9-269">For example, the code behind file *Pages/Customers/Edit.cshtml.cs* explicitly sets the namespace:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=snippet_namespace)]

<span data-ttu-id="ae4b9-270">*Pages/_ViewImports.cshtml* dosyasını ayarlar aşağıdaki ad alanı:</span><span class="sxs-lookup"><span data-stu-id="ae4b9-270">The *Pages/_ViewImports.cshtml* file sets the following namespace:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]

<span data-ttu-id="ae4b9-271">Oluşturulan ad alanı için *Pages/Customers/Edit.cshtml* Razor sayfasını aynıdır dosyanın arkasındaki kod.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-271">The generated namespace for the *Pages/Customers/Edit.cshtml* Razor Page is the same as the code behind file.</span></span> <span data-ttu-id="ae4b9-272">`@namespace` Yönergesi tasarlandığı bir proje ve sayfaları üretilen kod için C# sınıfları eklenmiş şekilde *yalnızca iş* eklemek zorunda kalmadan bir `@using` dosyanın arkasındaki kod için yönerge.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-272">The `@namespace` directive was designed so the C# classes added to a project and pages-generated code *just work* without having to add an `@using` directive for the code behind file.</span></span>

<span data-ttu-id="ae4b9-273">`@namespace` *Geleneksel Razor görünümleri ile de çalışır.*</span><span class="sxs-lookup"><span data-stu-id="ae4b9-273">`@namespace` *also works with conventional Razor views.*</span></span>

<span data-ttu-id="ae4b9-274">Özgün *Pages/Create.cshtml* dosyasını görüntüle:</span><span class="sxs-lookup"><span data-stu-id="ae4b9-274">The original *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml?highlight=2)]

<span data-ttu-id="ae4b9-275">Güncelleştirilmiş *Pages/Create.cshtml* dosyasını görüntüle:</span><span class="sxs-lookup"><span data-stu-id="ae4b9-275">The updated *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/Create.cshtml?highlight=2)]

<span data-ttu-id="ae4b9-276">[Razor sayfalarının başlangıç projesi](#rpvs17) içeren *Pages/_ValidationScriptsPartial.cshtml*, istemci tarafı doğrulama kanca oluşturur.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-276">The [Razor Pages starter project](#rpvs17) contains the *Pages/_ValidationScriptsPartial.cshtml*, which hooks up client-side validation.</span></span>

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a><span data-ttu-id="ae4b9-277">Sayfaları için URL oluşturma</span><span class="sxs-lookup"><span data-stu-id="ae4b9-277">URL generation for Pages</span></span>

<span data-ttu-id="ae4b9-278">`Create` Sayfasında, daha önce kullandığı gösterilen `RedirectToPage`:</span><span class="sxs-lookup"><span data-stu-id="ae4b9-278">The `Create` page, shown previously, uses `RedirectToPage`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=10)]

<span data-ttu-id="ae4b9-279">Uygulama, aşağıdaki dosya/klasör yapısını sahiptir:</span><span class="sxs-lookup"><span data-stu-id="ae4b9-279">The app has the following file/folder structure:</span></span>

* <span data-ttu-id="ae4b9-280">*/ Sayfaları*</span><span class="sxs-lookup"><span data-stu-id="ae4b9-280">*/Pages*</span></span>

  * <span data-ttu-id="ae4b9-281">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="ae4b9-281">*Index.cshtml*</span></span>
  * <span data-ttu-id="ae4b9-282">*/ Müşteriler*</span><span class="sxs-lookup"><span data-stu-id="ae4b9-282">*/Customers*</span></span>

    * <span data-ttu-id="ae4b9-283">*Create.cshtml*</span><span class="sxs-lookup"><span data-stu-id="ae4b9-283">*Create.cshtml*</span></span>
    * <span data-ttu-id="ae4b9-284">*Edit.cshtml*</span><span class="sxs-lookup"><span data-stu-id="ae4b9-284">*Edit.cshtml*</span></span>
    * <span data-ttu-id="ae4b9-285">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="ae4b9-285">*Index.cshtml*</span></span>

<span data-ttu-id="ae4b9-286">*Pages/Customers/Create.cshtml* ve *Pages/Customers/Edit.cshtml* sayfaları yeniden yönlendirmek için *Pages/Index.cshtml* başarı sonra.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-286">The *Pages/Customers/Create.cshtml* and *Pages/Customers/Edit.cshtml* pages redirect to *Pages/Index.cshtml* after success.</span></span> <span data-ttu-id="ae4b9-287">Dize `/Index` önceki sayfaya erişmek için URI bir parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-287">The string `/Index` is part of the URI to access the preceding page.</span></span> <span data-ttu-id="ae4b9-288">Dize `/Index` için URI oluşturmak için kullanılan *Pages/Index.cshtml* sayfası.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-288">The string `/Index` can be used to generate URIs to the *Pages/Index.cshtml* page.</span></span> <span data-ttu-id="ae4b9-289">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="ae4b9-289">For example:</span></span>

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">My Index Page</a>`
* `RedirectToPage("/Index")`

<span data-ttu-id="ae4b9-290">Sayfa adı sayfasına kökünden yoludur */sayfaları* başında dahil olmak üzere klasörü `/` (örneğin, `/Index`).</span><span class="sxs-lookup"><span data-stu-id="ae4b9-290">The page name is the path to the page from the root */Pages* folder including a leading `/` (for example, `/Index`).</span></span> <span data-ttu-id="ae4b9-291">Önceki URL nesil örnekleri bir URL cmdlet'e kod Gelişmiş Seçenekler ve işlevsel özellikler sunar.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-291">The preceding URL generation samples offer enhanced options and functional capabilities over hardcoding a URL.</span></span> <span data-ttu-id="ae4b9-292">URL oluşturma kullanan [yönlendirme](xref:mvc/controllers/routing) ve oluşturmak ve yol hedef yolunda nasıl tanımlandığına göre parametreleri kodlayın.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-292">URL generation uses [routing](xref:mvc/controllers/routing) and can generate and encode parameters according to how the route is defined in the destination path.</span></span>

<span data-ttu-id="ae4b9-293">Göreli adlar sayfaları için URL oluşturmayı destekler.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-293">URL generation for pages supports relative names.</span></span> <span data-ttu-id="ae4b9-294">Aşağıdaki tabloda hangi dizin sayfası ile farklı seçildiğini gösterir `RedirectToPage` parametrelerinden *Pages/Customers/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="ae4b9-294">The following table shows which Index page is selected with different `RedirectToPage` parameters from *Pages/Customers/Create.cshtml*:</span></span>

| <span data-ttu-id="ae4b9-295">RedirectToPage(x)</span><span class="sxs-lookup"><span data-stu-id="ae4b9-295">RedirectToPage(x)</span></span>| <span data-ttu-id="ae4b9-296">Sayfa</span><span class="sxs-lookup"><span data-stu-id="ae4b9-296">Page</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="ae4b9-297">RedirectToPage("/Index")</span><span class="sxs-lookup"><span data-stu-id="ae4b9-297">RedirectToPage("/Index")</span></span> | <span data-ttu-id="ae4b9-298">*Sayfa/dizini*</span><span class="sxs-lookup"><span data-stu-id="ae4b9-298">*Pages/Index*</span></span> |
| <span data-ttu-id="ae4b9-299">RedirectToPage("./Index");</span><span class="sxs-lookup"><span data-stu-id="ae4b9-299">RedirectToPage("./Index");</span></span> | <span data-ttu-id="ae4b9-300">*Müşteriler/sayfalar/dizini*</span><span class="sxs-lookup"><span data-stu-id="ae4b9-300">*Pages/Customers/Index*</span></span> |
| <span data-ttu-id="ae4b9-301">RedirectToPage(".. / Dizin")</span><span class="sxs-lookup"><span data-stu-id="ae4b9-301">RedirectToPage("../Index")</span></span> | <span data-ttu-id="ae4b9-302">*Sayfa/dizini*</span><span class="sxs-lookup"><span data-stu-id="ae4b9-302">*Pages/Index*</span></span> |
| <span data-ttu-id="ae4b9-303">RedirectToPage("Index")</span><span class="sxs-lookup"><span data-stu-id="ae4b9-303">RedirectToPage("Index")</span></span>  | <span data-ttu-id="ae4b9-304">*Müşteriler/sayfalar/dizini*</span><span class="sxs-lookup"><span data-stu-id="ae4b9-304">*Pages/Customers/Index*</span></span> |

<span data-ttu-id="ae4b9-305">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, ve `RedirectToPage("../Index")` olan <em>göreli adlar</em>.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-305">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, and `RedirectToPage("../Index")`  are <em>relative names</em>.</span></span> <span data-ttu-id="ae4b9-306">`RedirectToPage` Parametresi <em>birleştirilmiş</em> ile hedef sayfanın adını işlem için geçerli sayfasının yolu.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-306">The `RedirectToPage` parameter is <em>combined</em> with the path of the current page to compute the name of the destination page.</span></span>  <!-- Review: Original had The provided string is combined with the page name of the current page to compute the name of the destination page.  page name, not page path -->

<span data-ttu-id="ae4b9-307">Göreli adı bağlama karmaşık bir yapıyı siteleriyle oluştururken yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-307">Relative name linking is useful when building sites with a complex structure.</span></span> <span data-ttu-id="ae4b9-308">Bir klasördeki sayfaları arasında bağlamak için göreli adları kullanıyorsa, bu klasörü yeniden adlandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-308">If you use relative names to link between pages in a folder, you can rename that folder.</span></span> <span data-ttu-id="ae4b9-309">Tüm bağlantılar hala çalışır, (bunlar klasör adı eklemediniz çünkü).</span><span class="sxs-lookup"><span data-stu-id="ae4b9-309">All the links still work (because they didn't include the folder name).</span></span>

::: moniker range=">= aspnetcore-2.1"
## <a name="viewdata-attribute"></a><span data-ttu-id="ae4b9-310">ViewData özniteliği</span><span class="sxs-lookup"><span data-stu-id="ae4b9-310">ViewData attribute</span></span>

<span data-ttu-id="ae4b9-311">Veri içeren bir sayfa geçirilebilir [ViewDataAttribute](/dotnet/api/microsoft.aspnetcore.mvc.viewdataattribute).</span><span class="sxs-lookup"><span data-stu-id="ae4b9-311">Data can be passed to a page with [ViewDataAttribute](/dotnet/api/microsoft.aspnetcore.mvc.viewdataattribute).</span></span> <span data-ttu-id="ae4b9-312">Denetleyicileri veya Razor sayfasını modelleri özelliklerini donatılmış ile `[ViewData]` depolanır ve aşağıdaki konumdan yüklendi değerlerine sahip [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary).</span><span class="sxs-lookup"><span data-stu-id="ae4b9-312">Properties on controllers or Razor Page models decorated with `[ViewData]` have their values stored and loaded from the [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary).</span></span>

<span data-ttu-id="ae4b9-313">Aşağıdaki örnekte, `AboutModel` içeren bir `Title` özelliği donatılmış ile `[ViewData]`.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-313">In the following example, the `AboutModel` contains a `Title` property decorated with `[ViewData]`.</span></span> <span data-ttu-id="ae4b9-314">`Title` Özelliği hakkında sayfa başlık ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="ae4b9-314">The `Title` property is set to the title of the About page:</span></span>

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

<span data-ttu-id="ae4b9-315">Erişim hakkında sayfasının `Title` özelliği model özelliği olarak:</span><span class="sxs-lookup"><span data-stu-id="ae4b9-315">In the About page, access the `Title` property as a model property:</span></span>

```cshtml
<h1>@Model.Title</h1>
```

<span data-ttu-id="ae4b9-316">Düzende başlığı ViewData sözlükten okunur:</span><span class="sxs-lookup"><span data-stu-id="ae4b9-316">In the layout, the title is read from the ViewData dictionary:</span></span>

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"] - WebApplication</title>
    ...
```
::: moniker-end

## <a name="tempdata"></a><span data-ttu-id="ae4b9-317">TempData</span><span class="sxs-lookup"><span data-stu-id="ae4b9-317">TempData</span></span>

<span data-ttu-id="ae4b9-318">ASP.NET Core sunan [TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) özelliği bir [denetleyicisi](/dotnet/api/microsoft.aspnetcore.mvc.controller).</span><span class="sxs-lookup"><span data-stu-id="ae4b9-318">ASP.NET Core exposes the [TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) property on a [controller](/dotnet/api/microsoft.aspnetcore.mvc.controller).</span></span> <span data-ttu-id="ae4b9-319">Bu özellik, dosyayı okuma kadar verileri depolar.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-319">This property stores data until it's read.</span></span> <span data-ttu-id="ae4b9-320">`Keep` Ve `Peek` yöntemleri, verileri silme olmadan incelemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-320">The `Keep` and `Peek` methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="ae4b9-321">`TempData` birden çok tek bir istek için veri gerektiğinde yeniden yönlendirme için yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-321">`TempData` is  useful for redirection, when data is needed for more than a single request.</span></span>

<span data-ttu-id="ae4b9-322">`[TempData]` Özniteliği ASP.NET Core 2. 0 ' yenidir ve denetleyicileri ve sayfaları desteklenir.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-322">The `[TempData]` attribute is new in ASP.NET Core 2.0 and is supported on controllers and pages.</span></span>

<span data-ttu-id="ae4b9-323">Aşağıdaki kodu değerini ayarlar `Message` kullanarak `TempData`:</span><span class="sxs-lookup"><span data-stu-id="ae4b9-323">The following code sets the value of `Message` using `TempData`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,25&name=snippet_Temp)]

<span data-ttu-id="ae4b9-324">Aşağıdaki biçimlendirmede *Pages/Customers/Index.cshtml* dosyasını değerini görüntüler `Message` kullanarak `TempData`.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-324">The following markup in the *Pages/Customers/Index.cshtml* file displays the value of `Message` using `TempData`.</span></span>

```cshtml
<h3>Msg: @Model.Message</h3>
```

<span data-ttu-id="ae4b9-325">*Pages/Customers/Index.cshtml.cs* sayfa modeli uygular `[TempData]` özniteliğini `Message` özelliği.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-325">The *Pages/Customers/Index.cshtml.cs* page model applies the `[TempData]` attribute to the `Message` property.</span></span>

```cs
[TempData]
public string Message { get; set; }
```

<span data-ttu-id="ae4b9-326">Bkz: [TempData](xref:fundamentals/app-state#tempdata) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-326">See [TempData](xref:fundamentals/app-state#tempdata) for more information.</span></span>

<a name="mhpp"></a>
## <a name="multiple-handlers-per-page"></a><span data-ttu-id="ae4b9-327">Sayfa başına birden çok işleyicileri</span><span class="sxs-lookup"><span data-stu-id="ae4b9-327">Multiple handlers per page</span></span>

<span data-ttu-id="ae4b9-328">İki kullanarak işleyicileri sayfa için aşağıdaki sayfayı biçimlendirmeleri oluşturur `asp-page-handler` etiket Yardımcısı:</span><span class="sxs-lookup"><span data-stu-id="ae4b9-328">The following page generates markup for two page handlers using the `asp-page-handler` Tag Helper:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

<!-- Review: the FormActionTagHelper applies to all <form /> elements on a Razor page, even when there's no `asp-` attribute   -->

<span data-ttu-id="ae4b9-329">Önceki örnekte form iki düğmeleri, her kullanarak gönderme sahip `FormActionTagHelper` farklı bir URL'ye göndermek için.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-329">The form in the preceding example has two submit buttons, each using the `FormActionTagHelper` to submit to a different URL.</span></span> <span data-ttu-id="ae4b9-330">`asp-page-handler` Özniteliği için bir yardımcı olan `asp-page`.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-330">The `asp-page-handler` attribute is a companion to `asp-page`.</span></span> <span data-ttu-id="ae4b9-331">`asp-page-handler` bir sayfa tarafından tanımlanan işleyici yöntemlerin her biri için gönderme URL'leri oluşturur.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-331">`asp-page-handler` generates URLs that submit to each of the handler methods defined by a page.</span></span> <span data-ttu-id="ae4b9-332">`asp-page` Örnek geçerli sayfasına bağlantılandırma çünkü belirtilmedi.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-332">`asp-page` isn't specified because the sample is linking to the current page.</span></span>

<span data-ttu-id="ae4b9-333">Sayfa modeli:</span><span class="sxs-lookup"><span data-stu-id="ae4b9-333">The page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]

<span data-ttu-id="ae4b9-334">Önceki kod kullanan *işleyici yöntemleri adlı*.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-334">The preceding code uses *named handler methods*.</span></span> <span data-ttu-id="ae4b9-335">Adlandırılmış işleyici yöntemleri adından sonra metin gerçekleştirerek oluşturulur `On<HTTP Verb>` ve önce `Async` (varsa).</span><span class="sxs-lookup"><span data-stu-id="ae4b9-335">Named handler methods are created by taking the text in the name after `On<HTTP Verb>` and before `Async` (if present).</span></span> <span data-ttu-id="ae4b9-336">Önceki örnekte OnPost sayfa yöntemlerdir**JoinList**zaman uyumsuz ve OnPost**JoinListUC**zaman uyumsuz.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-336">In the preceding example, the page methods are OnPost**JoinList**Async and OnPost**JoinListUC**Async.</span></span> <span data-ttu-id="ae4b9-337">İle *OnPost* ve *zaman uyumsuz* kaldırıldı, işleyici adlardır `JoinList` ve `JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-337">With *OnPost* and *Async* removed, the handler names are `JoinList` and `JoinListUC`.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?range=12-13)]

<span data-ttu-id="ae4b9-338">Yukarıdaki kullanan kod, gönderildiği URL yolunu `OnPostJoinListAsync` olan `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-338">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span></span> <span data-ttu-id="ae4b9-339">Gönderildiği URL yolunu `OnPostJoinListUCAsync` olan `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-339">The URL path that submits to `OnPostJoinListUCAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span></span>

## <a name="customizing-routing"></a><span data-ttu-id="ae4b9-340">Yönlendirme özelleştirme</span><span class="sxs-lookup"><span data-stu-id="ae4b9-340">Customizing Routing</span></span>

<span data-ttu-id="ae4b9-341">Sorgu dizesi değiştirebileceğiniz `?handler=JoinList` yol kesimi URL'yi `/JoinList` rota şablonu belirterek `@page "{handler?}"`.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-341">You can change the query string `?handler=JoinList` in the URL to a route segment `/JoinList` by specifying the route template `@page "{handler?}"`.</span></span>

<span data-ttu-id="ae4b9-342">Sorgu dizesi hoşlanmıyorsanız `?handler=JoinList` URL'de işleyicisi adı URL'nin yol kısmı koymak için rota değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-342">If you don't like the query string `?handler=JoinList` in the URL, you can change the route to put the handler name in the path portion of the URL.</span></span> <span data-ttu-id="ae4b9-343">Bir rota şablonu sonra çift tırnak içine ekleyerek rota özelleştirebilirsiniz `@page` yönergesi.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-343">You can customize the route by adding a route template enclosed in double quotes after the `@page` directive.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]

<span data-ttu-id="ae4b9-344">Yukarıdaki kullanan kod, gönderildiği URL yolunu `OnPostJoinListAsync` olan `http://localhost:5000/Customers/CreateFATH/JoinList`.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-344">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `http://localhost:5000/Customers/CreateFATH/JoinList`.</span></span> <span data-ttu-id="ae4b9-345">Gönderildiği URL yolunu `OnPostJoinListUCAsync` olan `http://localhost:5000/Customers/CreateFATH/JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-345">The URL path that submits to `OnPostJoinListUCAsync` is `http://localhost:5000/Customers/CreateFATH/JoinListUC`.</span></span>

<span data-ttu-id="ae4b9-346">`?` Aşağıdaki `handler` rota parametresinin isteğe bağlı olduğu anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-346">The `?` following `handler` means the route parameter is optional.</span></span>

<span data-ttu-id="ae4b9-347">Kullanabileceğiniz `@page` kesimleri ve parametreleri bir sayfanın varsayılan yol için eklenecek.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-347">You can use `@page` to append segments and parameters to a page's default route.</span></span> <span data-ttu-id="ae4b9-348">Sayfanın yolu değiştirmek için bir mutlak ya da sanal yolu kullanarak (gibi `"~/Some/Other/Path"`) desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-348">Using an absolute or virtual path to change the page's route (like `"~/Some/Other/Path"`) isn't supported.</span></span>

## <a name="configuration-and-settings"></a><span data-ttu-id="ae4b9-349">Yapılandırma ve ayarları</span><span class="sxs-lookup"><span data-stu-id="ae4b9-349">Configuration and settings</span></span>

<span data-ttu-id="ae4b9-350">Gelişmiş seçenekleri yapılandırmak için genişletme yöntemi kullanmak `AddRazorPagesOptions` MVC oluşturucu üzerinde:</span><span class="sxs-lookup"><span data-stu-id="ae4b9-350">To configure advanced options, use the extension method `AddRazorPagesOptions` on the MVC builder:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/StartupAdvanced.cs?name=snippet_1)]

<span data-ttu-id="ae4b9-351">Şu anda kullanabileceğiniz `RazorPagesOptions` sayfaları için kök dizini ayarlayın veya sayfaları için uygulama modeli kuralları eklemek için.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-351">Currently you can use the `RazorPagesOptions` to set the root directory for pages, or add application model conventions for pages.</span></span> <span data-ttu-id="ae4b9-352">Gelecekte bu şekilde daha fazla genişletilebilirlik etkinleştirme.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-352">We'll enable more extensibility this way in the future.</span></span>

<span data-ttu-id="ae4b9-353">Görünümleri derleneceği bkz [Razor görünüm derleme](xref:mvc/views/view-compilation) .</span><span class="sxs-lookup"><span data-stu-id="ae4b9-353">To precompile views, see [Razor view compilation](xref:mvc/views/view-compilation) .</span></span>

<span data-ttu-id="ae4b9-354">[İndirme veya görüntüleme örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/index/sample).</span><span class="sxs-lookup"><span data-stu-id="ae4b9-354">[Download or view sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/index/sample).</span></span>

<span data-ttu-id="ae4b9-355">Bkz: [Razor sayfalarının ile çalışmaya başlama](xref:tutorials/razor-pages/razor-pages-start), ilgili bu girişi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-355">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start), which builds on this introduction.</span></span>

### <a name="specify-that-razor-pages-are-at-the-content-root"></a><span data-ttu-id="ae4b9-356">Razor sayfalarının içerik kök dizininde olduğunu belirtin</span><span class="sxs-lookup"><span data-stu-id="ae4b9-356">Specify that Razor Pages are at the content root</span></span>

<span data-ttu-id="ae4b9-357">Razor sayfalarının olsunlar varsayılan olarak, */sayfaları* dizin.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-357">By default, Razor Pages are rooted in the */Pages* directory.</span></span> <span data-ttu-id="ae4b9-358">Ekleme [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) için [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) Razor sayfalarınızı içerik kök dizininde olduğunu belirtmek için ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) uygulamasının:</span><span class="sxs-lookup"><span data-stu-id="ae4b9-358">Add [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at the content root ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) of the app:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesAtContentRoot();
```

### <a name="specify-that-razor-pages-are-at-a-custom-root-directory"></a><span data-ttu-id="ae4b9-359">Razor sayfalarının özel kök dizininde olduğunu belirtin</span><span class="sxs-lookup"><span data-stu-id="ae4b9-359">Specify that Razor Pages are at a custom root directory</span></span>

<span data-ttu-id="ae4b9-360">Ekleme [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) için [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) Razor sayfalarınızı uygulamasında özel kök dizininde olduğunu belirtmek için (göreli bir yol belirtin):</span><span class="sxs-lookup"><span data-stu-id="ae4b9-360">Add [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at a custom root directory in the app (provide a relative path):</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesRoot("/path/to/razor/pages");
```

## <a name="see-also"></a><span data-ttu-id="ae4b9-361">Ayrıca bkz.</span><span class="sxs-lookup"><span data-stu-id="ae4b9-361">See also</span></span>

* [<span data-ttu-id="ae4b9-362">ASP.NET Core giriş</span><span class="sxs-lookup"><span data-stu-id="ae4b9-362">Introduction to ASP.NET Core</span></span>](xref:index)
* [<span data-ttu-id="ae4b9-363">Razor söz dizimi</span><span class="sxs-lookup"><span data-stu-id="ae4b9-363">Razor syntax</span></span>](xref:mvc/views/razor)
* [<span data-ttu-id="ae4b9-364">Razor Sayfaları kullanmaya başlama</span><span class="sxs-lookup"><span data-stu-id="ae4b9-364">Get started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="ae4b9-365">Razor sayfalarının yetkilendirme kuralları</span><span class="sxs-lookup"><span data-stu-id="ae4b9-365">Razor Pages authorization conventions</span></span>](xref:security/authorization/razor-pages-authorization)
* [<span data-ttu-id="ae4b9-366">Razor sayfalarının özel yolu ve sayfayı model sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="ae4b9-366">Razor Pages custom route and page model providers</span></span>](xref:mvc/razor-pages/razor-pages-conventions)
* [<span data-ttu-id="ae4b9-367">Razor sayfalarının birim testleri</span><span class="sxs-lookup"><span data-stu-id="ae4b9-367">Razor Pages unit tests</span></span>](xref:test/razor-pages-tests)
