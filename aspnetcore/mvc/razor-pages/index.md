---
title: "ASP.NET Core Razor sayfalarında giriş"
author: Rick-Anderson
description: "Daha kolay ve MVC kullanmaktan daha üretken ASP.NET Core Razor sayfalarında kodlama sayfa odaklı senaryoları nasıl kolaylaştırdığını öğrenin."
manager: wpickett
ms.author: riande
ms.date: 09/12/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: mvc/razor-pages/index
ms.openlocfilehash: cb80c38fd0284d5153aebfe7bb515722623a4a34
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/15/2018
---
# <a name="introduction-to-razor-pages-in-aspnet-core"></a><span data-ttu-id="b030e-103">ASP.NET Core Razor sayfalarında giriş</span><span class="sxs-lookup"><span data-stu-id="b030e-103">Introduction to Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="b030e-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Ryan Nowak](https://github.com/rynowak)</span><span class="sxs-lookup"><span data-stu-id="b030e-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Ryan Nowak](https://github.com/rynowak)</span></span>

<span data-ttu-id="b030e-105">Razor sayfalarının sayfa odaklı senaryoları daha kolay ve daha üretken kodlama yapar ASP.NET Core MVC yeni özelliğidir.</span><span class="sxs-lookup"><span data-stu-id="b030e-105">Razor Pages is a new feature of ASP.NET Core MVC that makes coding page-focused scenarios easier and more productive.</span></span>

<span data-ttu-id="b030e-106">Model-View-Controller yaklaşım kullanan bir öğretici için arıyorsanız bkz [ASP.NET Core MVC ile çalışmaya başlama](xref:tutorials/first-mvc-app/start-mvc).</span><span class="sxs-lookup"><span data-stu-id="b030e-106">If you're looking for a tutorial that uses the Model-View-Controller approach, see [Get started with ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span></span>

<span data-ttu-id="b030e-107">Bu belge Razor sayfalarının tanıtılmaktadır.</span><span class="sxs-lookup"><span data-stu-id="b030e-107">This document provides an introduction to Razor Pages.</span></span> <span data-ttu-id="b030e-108">Adım adım öğretici değil.</span><span class="sxs-lookup"><span data-stu-id="b030e-108">It's not a step by step tutorial.</span></span> <span data-ttu-id="b030e-109">Bazı çok Gelişmiş bölümleri bulamazsanız, bakın [Razor sayfalarının ile çalışmaya başlama](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="b030e-109">If you find some of the sections too advanced, see [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span></span> <span data-ttu-id="b030e-110">ASP.NET Core genel bakış için bkz: [ASP.NET Core giriş](xref:index).</span><span class="sxs-lookup"><span data-stu-id="b030e-110">For an overview of ASP.NET Core, see the [Introduction to ASP.NET Core](xref:index).</span></span>

<a name="prerequisites"></a>

## <a name="aspnet-core-20-prerequisites"></a><span data-ttu-id="b030e-111">ASP.NET Core 2.0 önkoşulları</span><span class="sxs-lookup"><span data-stu-id="b030e-111">ASP.NET Core 2.0 prerequisites</span></span>

<span data-ttu-id="b030e-112">Yükleme [.NET Core](https://www.microsoft.com/net/core) 2.0.0 veya sonraki bir sürümü.</span><span class="sxs-lookup"><span data-stu-id="b030e-112">Install [.NET Core](https://www.microsoft.com/net/core) 2.0.0 or later.</span></span>

<span data-ttu-id="b030e-113">Visual Studio kullanıyorsanız, yükleme [Visual Studio](https://www.visualstudio.com/vs/) 2017 15.3 veya aşağıdaki iş yükleri ile sonraki bir sürümü:</span><span class="sxs-lookup"><span data-stu-id="b030e-113">If you're using Visual Studio, install [Visual Studio](https://www.visualstudio.com/vs/) 2017 version 15.3 or later with the following workloads:</span></span>

* <span data-ttu-id="b030e-114">**ASP.NET ve web geliştirme**</span><span class="sxs-lookup"><span data-stu-id="b030e-114">**ASP.NET and web development**</span></span>
* <span data-ttu-id="b030e-115">**.NET core platformlar arası geliştirme**</span><span class="sxs-lookup"><span data-stu-id="b030e-115">**.NET Core cross-platform development**</span></span>

<a name="rpvs17"></a>

## <a name="creating-a-razor-pages-project"></a><span data-ttu-id="b030e-116">Razor sayfalarının proje oluşturma</span><span class="sxs-lookup"><span data-stu-id="b030e-116">Creating a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b030e-117">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b030e-117">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="b030e-118">Bkz: [Razor sayfalarının ile çalışmaya başlama](xref:tutorials/razor-pages/razor-pages-start) Razor sayfalarının oluşturma konusunda ayrıntılı yönergeler için Proje Visual Studio kullanarak.</span><span class="sxs-lookup"><span data-stu-id="b030e-118">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) for detailed instructions on how to create a Razor Pages project using Visual Studio.</span></span>

#   <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="b030e-119">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b030e-119">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="b030e-120">Çalıştırma `dotnet new razor` komut satırından.</span><span class="sxs-lookup"><span data-stu-id="b030e-120">Run `dotnet new razor` from the command line.</span></span>

<span data-ttu-id="b030e-121">Oluşturulan açmak *.csproj* Visual Studio dosyasından Mac için</span><span class="sxs-lookup"><span data-stu-id="b030e-121">Open the generated *.csproj* file from Visual Studio for Mac.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b030e-122">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b030e-122">Visual Studio Code</span></span>](#tab/visual-studio-code) 

<span data-ttu-id="b030e-123">Çalıştırma `dotnet new razor` komut satırından.</span><span class="sxs-lookup"><span data-stu-id="b030e-123">Run `dotnet new razor` from the command line.</span></span>

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="b030e-124">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="b030e-124">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="b030e-125">Çalıştırma `dotnet new razor` komut satırından.</span><span class="sxs-lookup"><span data-stu-id="b030e-125">Run `dotnet new razor` from the command line.</span></span>

---

## <a name="razor-pages"></a><span data-ttu-id="b030e-126">Razor sayfalarının</span><span class="sxs-lookup"><span data-stu-id="b030e-126">Razor Pages</span></span>

<span data-ttu-id="b030e-127">Razor sayfalarının etkinleşir *haline*:</span><span class="sxs-lookup"><span data-stu-id="b030e-127">Razor Pages is enabled in *Startup.cs*:</span></span>

[!code-cs[](index/sample/RazorPagesIntro/Startup.cs?name=snippet_Startup)]

<span data-ttu-id="b030e-128">Temel bir sayfa göz önünde bulundurun: <a name="OnGet"></a></span><span class="sxs-lookup"><span data-stu-id="b030e-128">Consider a basic page: <a name="OnGet"></a></span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index.cshtml)]

<span data-ttu-id="b030e-129">Önceki kod çok Razor görünüm dosyası gibi görünüyor.</span><span class="sxs-lookup"><span data-stu-id="b030e-129">The preceding code looks a lot like a Razor view file.</span></span> <span data-ttu-id="b030e-130">Neler farklı kılan unsurdur `@page` yönergesi.</span><span class="sxs-lookup"><span data-stu-id="b030e-130">What makes it different is the `@page` directive.</span></span> <span data-ttu-id="b030e-131">`@page` Dosya, isteklerini doğrudan bir denetleyici geçmeden işleme anlamına gelir ve MVC eylemi - içine yapar.</span><span class="sxs-lookup"><span data-stu-id="b030e-131">`@page` makes the file into an MVC action - which means that it handles requests directly, without going through a controller.</span></span> <span data-ttu-id="b030e-132">`@page` ilk Razor yönergesi bir sayfa üzerinde olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="b030e-132">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="b030e-133">`@page` diğer Razor yapıları davranışını etkiler.</span><span class="sxs-lookup"><span data-stu-id="b030e-133">`@page` affects the behavior of other Razor constructs.</span></span>

<span data-ttu-id="b030e-134">Benzer bir sayfa, kullanarak bir `PageModel` sınıfında, aşağıdaki iki dosyada gösterilir.</span><span class="sxs-lookup"><span data-stu-id="b030e-134">A similar page, using a `PageModel` class, is shown in the following two files.</span></span> <span data-ttu-id="b030e-135">*Pages/Index2.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="b030e-135">The *Pages/Index2.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index2.cshtml)]

<span data-ttu-id="b030e-136">*Pages/Index2.cshtml.cs* sayfa modeli:</span><span class="sxs-lookup"><span data-stu-id="b030e-136">The *Pages/Index2.cshtml.cs* page model:</span></span>

[!code-cs[](index/sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]

<span data-ttu-id="b030e-137">Kural tarafından `PageModel` sınıf dosyası Razor sayfa dosyası ile aynı ada sahip *.cs* eklenir.</span><span class="sxs-lookup"><span data-stu-id="b030e-137">By convention, the `PageModel` class file has the same name as the Razor Page file with *.cs* appended.</span></span> <span data-ttu-id="b030e-138">Örneğin, önceki Razor sayfasıdır *Pages/Index2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="b030e-138">For example, the previous Razor Page is *Pages/Index2.cshtml*.</span></span> <span data-ttu-id="b030e-139">Dosyayı içeren `PageModel` sınıfı adlandırılan *Pages/Index2.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="b030e-139">The file containing the `PageModel` class is named *Pages/Index2.cshtml.cs*.</span></span>

<span data-ttu-id="b030e-140">URL yollarını sayfalara ilişkilendirmelerini dosya sisteminde sayfanın konuma göre belirlenir.</span><span class="sxs-lookup"><span data-stu-id="b030e-140">The associations of URL paths to pages are determined by the page's location in the file system.</span></span> <span data-ttu-id="b030e-141">Aşağıdaki tablo Razor sayfasının yolunu ve eşleşen URL gösterir:</span><span class="sxs-lookup"><span data-stu-id="b030e-141">The following table shows a Razor Page path and the matching URL:</span></span>

| <span data-ttu-id="b030e-142">Dosya adı ve yolu</span><span class="sxs-lookup"><span data-stu-id="b030e-142">File name and path</span></span>               | <span data-ttu-id="b030e-143">URL eşleştirme</span><span class="sxs-lookup"><span data-stu-id="b030e-143">matching URL</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="b030e-144">*/Pages/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="b030e-144">*/Pages/Index.cshtml*</span></span> | <span data-ttu-id="b030e-145">`/` Veya `/Index`</span><span class="sxs-lookup"><span data-stu-id="b030e-145">`/` or `/Index`</span></span> |
| <span data-ttu-id="b030e-146">*/Pages/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="b030e-146">*/Pages/Contact.cshtml*</span></span> | `/Contact` |
| <span data-ttu-id="b030e-147">*/Pages/Store/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="b030e-147">*/Pages/Store/Contact.cshtml*</span></span> | `/Store/Contact` |
| <span data-ttu-id="b030e-148">*/Pages/Store/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="b030e-148">*/Pages/Store/Index.cshtml*</span></span> | <span data-ttu-id="b030e-149">`/Store` Veya `/Store/Index`</span><span class="sxs-lookup"><span data-stu-id="b030e-149">`/Store` or `/Store/Index`</span></span> |

<span data-ttu-id="b030e-150">Notlar:</span><span class="sxs-lookup"><span data-stu-id="b030e-150">Notes:</span></span>

* <span data-ttu-id="b030e-151">Razor sayfalarının dosyalarında çalışma zamanı arar *sayfaları* varsayılan klasör.</span><span class="sxs-lookup"><span data-stu-id="b030e-151">The runtime looks for Razor Pages files in the *Pages* folder by default.</span></span>
* <span data-ttu-id="b030e-152">`Index` bir URL bir sayfa içermediğinde varsayılan sayfasıdır.</span><span class="sxs-lookup"><span data-stu-id="b030e-152">`Index` is the default page when a URL doesn't include a page.</span></span>

## <a name="writing-a-basic-form"></a><span data-ttu-id="b030e-153">Temel form yazma</span><span class="sxs-lookup"><span data-stu-id="b030e-153">Writing a basic form</span></span>

<span data-ttu-id="b030e-154">Razor sayfalarının özellikleri ortak desenler web tarayıcılarıyla birlikte kullanılan kolay hale getirmek üzere tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="b030e-154">Razor Pages features are designed to make common patterns used with web browsers easy.</span></span> <span data-ttu-id="b030e-155">[Model bağlama](xref:mvc/models/model-binding), [etiket Yardımcıları](xref:mvc/views/tag-helpers/intro)ve HTML Yardımcıları tüm *yalnızca iş* Razor sayfasını sınıfında tanımlanan özelliklere sahip.</span><span class="sxs-lookup"><span data-stu-id="b030e-155">[Model binding](xref:mvc/models/model-binding), [Tag Helpers](xref:mvc/views/tag-helpers/intro), and HTML helpers all *just work* with the properties defined in a Razor Page class.</span></span> <span data-ttu-id="b030e-156">"Bize başvurun" form için temel bir uygulayan bir sayfa göz önünde bulundurun `Contact` modeli:</span><span class="sxs-lookup"><span data-stu-id="b030e-156">Consider a page that implements a basic "contact us" form for the `Contact` model:</span></span>

<span data-ttu-id="b030e-157">Bu belgede, örnekleri için `DbContext` içinde başlatılan [haline](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) dosya.</span><span class="sxs-lookup"><span data-stu-id="b030e-157">For the samples in this document, the `DbContext` is initialized in the [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) file.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Startup.cs?highlight=15-16)]

<span data-ttu-id="b030e-158">Veri modeli:</span><span class="sxs-lookup"><span data-stu-id="b030e-158">The data model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Data/Customer.cs)]

<span data-ttu-id="b030e-159">Db bağlamı:</span><span class="sxs-lookup"><span data-stu-id="b030e-159">The db context:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Data/AppDbContext.cs)]

<span data-ttu-id="b030e-160">*Pages/Create.cshtml* dosyasını görüntüle:</span><span class="sxs-lookup"><span data-stu-id="b030e-160">The *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml)]

<span data-ttu-id="b030e-161">*Pages/Create.cshtml.cs* sayfa modeli:</span><span class="sxs-lookup"><span data-stu-id="b030e-161">The *Pages/Create.cshtml.cs* page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_ALL)]

<span data-ttu-id="b030e-162">Kural tarafından `PageModel` sınıfı çağrıldığında `<PageName>Model` ve sayfa aynı ad.</span><span class="sxs-lookup"><span data-stu-id="b030e-162">By convention, the `PageModel` class is called `<PageName>Model` and is in the same namespace as the page.</span></span>

<span data-ttu-id="b030e-163">`PageModel` Sınıfı, bir sayfa mantığı ayrılması kendi sunudan olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="b030e-163">The `PageModel` class allows separation of the logic of a page from its presentation.</span></span> <span data-ttu-id="b030e-164">Sayfaya gönderilen istekleri ve sayfayı oluşturmak için kullanılan verileri için sayfa işleyiciler tanımlar.</span><span class="sxs-lookup"><span data-stu-id="b030e-164">It defines page handlers for requests sent to the page and the data used to render the page.</span></span> <span data-ttu-id="b030e-165">Bu ayrım aracılığıyla sayfa bağımlılıklar yönetmenize olanak sağlayan [bağımlılık ekleme](xref:fundamentals/dependency-injection) ve [birim testi](xref:testing/razor-pages-testing) sayfaları.</span><span class="sxs-lookup"><span data-stu-id="b030e-165">This separation allows you to manage page dependencies through [dependency injection](xref:fundamentals/dependency-injection) and to [unit test](xref:testing/razor-pages-testing) the pages.</span></span>

<span data-ttu-id="b030e-166">Sayfasına sahip bir `OnPostAsync` *işleyici yöntemi*, üzerinde çalıştığı `POST` ister (kullanıcı formu gönderdiğinde).</span><span class="sxs-lookup"><span data-stu-id="b030e-166">The page has an `OnPostAsync` *handler method*, which runs on `POST` requests (when a user posts the form).</span></span> <span data-ttu-id="b030e-167">Herhangi bir HTTP fiil için işleyici yöntemleri ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b030e-167">You can add handler methods for any HTTP verb.</span></span> <span data-ttu-id="b030e-168">En yaygın işleyicileri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="b030e-168">The most common handlers are:</span></span>

* <span data-ttu-id="b030e-169">`OnGet` sayfa için gereken durumu başlatılamadı.</span><span class="sxs-lookup"><span data-stu-id="b030e-169">`OnGet` to initialize state needed for the page.</span></span> <span data-ttu-id="b030e-170">[OnGet](#OnGet) örnek.</span><span class="sxs-lookup"><span data-stu-id="b030e-170">[OnGet](#OnGet) sample.</span></span>
* <span data-ttu-id="b030e-171">`OnPost` Form Gönderme işlemek için.</span><span class="sxs-lookup"><span data-stu-id="b030e-171">`OnPost` to handle form submissions.</span></span>

<span data-ttu-id="b030e-172">`Async` Adlandırma soneki isteğe bağlıdır ancak genellikle kurala göre zaman uyumsuz işlevleri için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b030e-172">The `Async` naming suffix is optional but is often used by convention for asynchronous functions.</span></span> <span data-ttu-id="b030e-173">`OnPostAsync` Önceki örnekte kod ne, normalde bir denetleyicisi yazarsınız için benzer görünür.</span><span class="sxs-lookup"><span data-stu-id="b030e-173">The `OnPostAsync` code in the preceding example looks similar to what you would normally write in a controller.</span></span> <span data-ttu-id="b030e-174">Önceki kod Razor sayfalar için normaldir.</span><span class="sxs-lookup"><span data-stu-id="b030e-174">The preceding code is typical for Razor Pages.</span></span> <span data-ttu-id="b030e-175">MVC elemanlar çoğu ister [model bağlama](xref:mvc/models/model-binding), [doğrulama](xref:mvc/models/validation), ve eylem sonuçlarını paylaşıldığı.</span><span class="sxs-lookup"><span data-stu-id="b030e-175">Most of the MVC primitives like [model binding](xref:mvc/models/model-binding), [validation](xref:mvc/models/validation), and action results are shared.</span></span>  <!-- Review: Ryan, can we get a list of what is shared and what isn't? -->

<span data-ttu-id="b030e-176">Önceki `OnPostAsync` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="b030e-176">The previous `OnPostAsync` method:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="b030e-177">Temel akışı `OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="b030e-177">The basic flow of `OnPostAsync`:</span></span>

<span data-ttu-id="b030e-178">Doğrulama hataları denetleyin.</span><span class="sxs-lookup"><span data-stu-id="b030e-178">Check for validation errors.</span></span>

*  <span data-ttu-id="b030e-179">Herhangi bir hata varsa, verileri Kaydet ve yeniden yönlendirme.</span><span class="sxs-lookup"><span data-stu-id="b030e-179">If there are no errors, save the data and redirect.</span></span>
*  <span data-ttu-id="b030e-180">Hatalar varsa, doğrulama iletileri sayfasıyla yeniden gösterir.</span><span class="sxs-lookup"><span data-stu-id="b030e-180">If there are errors, show the page again with validation messages.</span></span> <span data-ttu-id="b030e-181">İstemci tarafı doğrulama geleneksel ASP.NET Core MVC uygulamaları için aynıdır.</span><span class="sxs-lookup"><span data-stu-id="b030e-181">Client-side validation is identical to traditional ASP.NET Core MVC applications.</span></span> <span data-ttu-id="b030e-182">Çoğu durumda, doğrulama hataları istemcide algılanabilir ve hiçbir zaman sunucuya gönderildi.</span><span class="sxs-lookup"><span data-stu-id="b030e-182">In many cases, validation errors would be detected on the client, and never submitted to the server.</span></span>

<span data-ttu-id="b030e-183">Veri başarıyla girildiğinde `OnPostAsync` işleyici yöntem çağrılarını `RedirectToPage` bir örneğini döndürmek için yardımcı yöntem `RedirectToPageResult`.</span><span class="sxs-lookup"><span data-stu-id="b030e-183">When the data is entered successfully, the `OnPostAsync` handler method calls the `RedirectToPage` helper method to return an instance of `RedirectToPageResult`.</span></span> <span data-ttu-id="b030e-184">`RedirectToPage` Yeni eylem sonucu, benzer olduğunu `RedirectToAction` veya `RedirectToRoute`, ancak özelleştirilmiş sayfalar için.</span><span class="sxs-lookup"><span data-stu-id="b030e-184">`RedirectToPage` is a new action result, similar to `RedirectToAction` or `RedirectToRoute`, but customized for pages.</span></span> <span data-ttu-id="b030e-185">İsteğe bağlı olarak önceki örnekte kök dizin sayfasına yönlendiren (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="b030e-185">In the preceding sample, it redirects to the root Index page (`/Index`).</span></span> <span data-ttu-id="b030e-186">`RedirectToPage` içinde ayrıntılı [sayfaları için URL oluşturma](#url_gen) bölümü.</span><span class="sxs-lookup"><span data-stu-id="b030e-186">`RedirectToPage` is detailed in the [URL generation for Pages](#url_gen) section.</span></span>

<span data-ttu-id="b030e-187">Teslim edilen formu (yani sunucuya geçirilir) doğrulama hataları olduğunda`OnPostAsync` işleyici yöntem çağrılarını `Page` yardımcı yöntemi.</span><span class="sxs-lookup"><span data-stu-id="b030e-187">When the submitted form has validation errors (that are passed to the server), the`OnPostAsync` handler method calls the `Page` helper method.</span></span> <span data-ttu-id="b030e-188">`Page` örneğini döndürür `PageResult`.</span><span class="sxs-lookup"><span data-stu-id="b030e-188">`Page` returns an instance of `PageResult`.</span></span> <span data-ttu-id="b030e-189">Döndürme `Page` nasıl denetleyicileri Eylemler döndürmek için benzer `View`.</span><span class="sxs-lookup"><span data-stu-id="b030e-189">Returning `Page` is similar to how actions in controllers return `View`.</span></span> <span data-ttu-id="b030e-190">`PageResult` Varsayılan değer <!-- Review  --> dönüş türü için bir işleyici yöntemi.</span><span class="sxs-lookup"><span data-stu-id="b030e-190">`PageResult` is the default <!-- Review  --> return type for a handler method.</span></span> <span data-ttu-id="b030e-191">Döndüren bir işleyici yöntemi `void` sayfasını işler.</span><span class="sxs-lookup"><span data-stu-id="b030e-191">A handler method that returns `void` renders the page.</span></span>

<span data-ttu-id="b030e-192">`Customer` Özelliğini kullanan `[BindProperty]` model bağlama için kabul özniteliği.</span><span class="sxs-lookup"><span data-stu-id="b030e-192">The `Customer` property uses `[BindProperty]` attribute to opt in to model binding.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_PageModel&highlight=10-11)]

<span data-ttu-id="b030e-193">Razor sayfalarının varsayılan olarak, GET olmayan fiiller yalnızca özelliklerle bağlayın.</span><span class="sxs-lookup"><span data-stu-id="b030e-193">Razor Pages, by default, bind properties only with non-GET verbs.</span></span> <span data-ttu-id="b030e-194">Bağlama özellikleri için kod yazmak zorunda miktarını azaltır.</span><span class="sxs-lookup"><span data-stu-id="b030e-194">Binding to properties can reduce the amount of code you have to write.</span></span> <span data-ttu-id="b030e-195">Bağlama form alanları oluşturmak için aynı özelliği kullanarak kod azaltır (`<input asp-for="Customer.Name" />`) ve giriş kabul edin.</span><span class="sxs-lookup"><span data-stu-id="b030e-195">Binding reduces code by using the same property to render form fields (`<input asp-for="Customer.Name" />`) and accept the input.</span></span>

> [!NOTE]
> <span data-ttu-id="b030e-196">Güvenlik nedenleriyle, sayfa model özellikleri GET isteği veri bağlaması kabul gerekir.</span><span class="sxs-lookup"><span data-stu-id="b030e-196">For security reasons, you must opt in to binding GET request data to page model properties.</span></span> <span data-ttu-id="b030e-197">Kullanıcı girişi özelliklere eşleme önce doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="b030e-197">Verify user input before mapping it to properties.</span></span> <span data-ttu-id="b030e-198">Bu davranış seçim sorgu dizesi veya rota değerlerine dayanan özellikleri oluştururken yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="b030e-198">Opting in to this behavior is useful when building features which rely on query string or route values.</span></span>
>
> <span data-ttu-id="b030e-199">Özellik GET isteklerinde bağlamak için ayarlanmış `[BindProperty]` özniteliğin `SupportsGet` özelliğine `true`: `[BindProperty(SupportsGet = true)]`</span><span class="sxs-lookup"><span data-stu-id="b030e-199">To bind a property on GET requests, set the `[BindProperty]` attribute's `SupportsGet` property to `true`: `[BindProperty(SupportsGet = true)]`</span></span>

<span data-ttu-id="b030e-200">Giriş sayfası (*Index.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="b030e-200">The home page (*Index.cshtml*):</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml)]

<span data-ttu-id="b030e-201">Arka plan kod *Index.cshtml.cs* dosyası:</span><span class="sxs-lookup"><span data-stu-id="b030e-201">The code behind *Index.cshtml.cs* file:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs)]

<span data-ttu-id="b030e-202">*Index.cshtml* dosyası her kişi için bir düzenleme bağlantısı oluşturmak için aşağıdaki biçimlendirme içerir:</span><span class="sxs-lookup"><span data-stu-id="b030e-202">The *Index.cshtml* file contains the following markup to create an edit link for each contact:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=21)]

<span data-ttu-id="b030e-203">[Yer işareti etiketi yardımcı](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) kullanılan `asp-route-{value}` düzenleme sayfasını bir bağlantı oluşturmak için öznitelik.</span><span class="sxs-lookup"><span data-stu-id="b030e-203">The [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) used the `asp-route-{value}` attribute to generate a link to the Edit page.</span></span> <span data-ttu-id="b030e-204">Rota verilerini kişiyle bağlantıyı içeren kimliği</span><span class="sxs-lookup"><span data-stu-id="b030e-204">The link contains route data with the contact ID.</span></span> <span data-ttu-id="b030e-205">Örneğin, `http://localhost:5000/Edit/1`.</span><span class="sxs-lookup"><span data-stu-id="b030e-205">For example, `http://localhost:5000/Edit/1`.</span></span>

<span data-ttu-id="b030e-206">*Pages/Edit.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="b030e-206">The *Pages/Edit.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Edit.cshtml?highlight=1)]

<span data-ttu-id="b030e-207">İlk satır içerir `@page "{id:int}"` yönergesi.</span><span class="sxs-lookup"><span data-stu-id="b030e-207">The first line contains the `@page "{id:int}"` directive.</span></span> <span data-ttu-id="b030e-208">Yönlendirme kısıtlaması`"{id:int}"` içeren sayfa isteklerini kabul etmek için sayfayı söyler `int` rota verileri.</span><span class="sxs-lookup"><span data-stu-id="b030e-208">The routing constraint`"{id:int}"` tells the page to accept requests to the page that contain `int` route data.</span></span> <span data-ttu-id="b030e-209">Sayfa için bir istek dönüştürülebilir rota verileri içermiyorsa, bir `int`, çalışma zamanı (bulunamadı) HTTP 404 hatası döndürür.</span><span class="sxs-lookup"><span data-stu-id="b030e-209">If a request to the page doesn't contain route data that can be converted to an `int`, the runtime returns an HTTP 404 (not found) error.</span></span>

<span data-ttu-id="b030e-210">*Pages/Edit.cshtml.cs* dosyası:</span><span class="sxs-lookup"><span data-stu-id="b030e-210">The *Pages/Edit.cshtml.cs* file:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Edit.cshtml.cs)]

<span data-ttu-id="b030e-211">*Index.cshtml* dosya de Sil düğmesini her müşteri irtibat için oluşturmak için biçimlendirme içerir:</span><span class="sxs-lookup"><span data-stu-id="b030e-211">The *Index.cshtml* file also contains markup to create a delete button for each customer contact:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=22-23)]

<span data-ttu-id="b030e-212">Sil düğmesini HTML olarak işlendiğinde, `formaction` parametreleri içerir:</span><span class="sxs-lookup"><span data-stu-id="b030e-212">When the delete button is rendered in HTML, its `formaction` includes parameters for:</span></span>

* <span data-ttu-id="b030e-213">Müşteri tarafından belirtilen kimliği başvurun `asp-route-id` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="b030e-213">The customer contact ID specified by the `asp-route-id` attribute.</span></span>
* <span data-ttu-id="b030e-214">`handler` Tarafından belirtilen `asp-page-handler` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="b030e-214">The `handler` specified by the `asp-page-handler` attribute.</span></span>

<span data-ttu-id="b030e-215">Bir müşteri ile işlenen Sil düğmesini bir örneği burada verilmiştir Kimliğini başvurun `1`:</span><span class="sxs-lookup"><span data-stu-id="b030e-215">Here is an example of a rendered delete button with a customer contact ID of `1`:</span></span>

```html
<button type="submit" formaction="/?id=1&amp;handler=delete">delete</button>
```

<span data-ttu-id="b030e-216">Düğme seçildiğinde, bir form `POST` isteği sunucuya gönderilir.</span><span class="sxs-lookup"><span data-stu-id="b030e-216">When the button is selected, a form `POST` request is sent to the server.</span></span> <span data-ttu-id="b030e-217">Kurala göre işleyicisi yönteminin adı seçili değerine göre `handler` parametre düzeni göre `OnPost[handler]Async`.</span><span class="sxs-lookup"><span data-stu-id="b030e-217">By convention, the name of the handler method is selected based the value of the `handler` parameter according to the scheme `OnPost[handler]Async`.</span></span>

<span data-ttu-id="b030e-218">Çünkü `handler` olan `delete` Bu örnekte, `OnPostDeleteAsync` işleyici yöntemi kullanılır işleme `POST` isteği.</span><span class="sxs-lookup"><span data-stu-id="b030e-218">Because the `handler` is `delete` in this example, the `OnPostDeleteAsync` handler method is used to process the `POST` request.</span></span> <span data-ttu-id="b030e-219">Varsa `asp-page-handler` gibi farklı bir değere ayarlanmış `remove`, ada sahip bir sayfa işleyici yöntemi `OnPostRemoveAsync` seçilir.</span><span class="sxs-lookup"><span data-stu-id="b030e-219">If the `asp-page-handler` is set to a different value, such as `remove`, a page handler method with the name `OnPostRemoveAsync` is selected.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs?range=26-37)]

<span data-ttu-id="b030e-220">`OnPostDeleteAsync` Yöntemi:</span><span class="sxs-lookup"><span data-stu-id="b030e-220">The `OnPostDeleteAsync` method:</span></span>

* <span data-ttu-id="b030e-221">Kabul `id` Sorgu dizesinden.</span><span class="sxs-lookup"><span data-stu-id="b030e-221">Accepts the `id` from the query string.</span></span>
* <span data-ttu-id="b030e-222">Müşteri irtibat ile veritabanını sorgular `FindAsync`.</span><span class="sxs-lookup"><span data-stu-id="b030e-222">Queries the database for the customer contact with `FindAsync`.</span></span>
* <span data-ttu-id="b030e-223">Müşteri irtibat bulunursa, müşteri kişiler listesinden kaldırıldı.</span><span class="sxs-lookup"><span data-stu-id="b030e-223">If the customer contact is found, they're removed from the list of customer contacts.</span></span> <span data-ttu-id="b030e-224">Veritabanı güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="b030e-224">The database is updated.</span></span>
* <span data-ttu-id="b030e-225">Çağrıları `RedirectToPage` kök dizin sayfasına yeniden yönlendirmek için (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="b030e-225">Calls `RedirectToPage` to redirect to the root Index page (`/Index`).</span></span>

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a><span data-ttu-id="b030e-226">XSRF/CSRF ve Razor sayfaları</span><span class="sxs-lookup"><span data-stu-id="b030e-226">XSRF/CSRF and Razor Pages</span></span>

<span data-ttu-id="b030e-227">Herhangi bir kod yazmak zorunda değilsiniz [antiforgery doğrulama](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="b030e-227">You don't have to write any code for [antiforgery validation](xref:security/anti-request-forgery).</span></span> <span data-ttu-id="b030e-228">Otomatik olarak antiforgery belirteci oluşturma ve doğrulama Razor sayfalarında dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="b030e-228">Antiforgery token generation and validation are automatically included in Razor Pages.</span></span>

<a name="layout"></a>
## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a><span data-ttu-id="b030e-229">Düzenleri, kısmi, şablonları ve etiket Yardımcıları Razor Pages ile kullanma</span><span class="sxs-lookup"><span data-stu-id="b030e-229">Using Layouts, partials, templates, and Tag Helpers with Razor Pages</span></span>

<span data-ttu-id="b030e-230">Sayfaları tüm özellikleri Razor görüntüleme altyapısı ile çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="b030e-230">Pages work with all the features of the Razor view engine.</span></span> <span data-ttu-id="b030e-231">Düzenleri, kısmi, şablonları, etiket Yardımcıları *_ViewStart.cshtml*, *_viewımports.cshtml* iş için geleneksel Razor görünümleri yaparlar aynı şekilde.</span><span class="sxs-lookup"><span data-stu-id="b030e-231">Layouts, partials, templates, Tag Helpers, *_ViewStart.cshtml*, *_ViewImports.cshtml* work in the same way they do for conventional Razor views.</span></span>

<span data-ttu-id="b030e-232">Şimdi bu sayfa bu özelliklerden bazıları yararlanarak declutter.</span><span class="sxs-lookup"><span data-stu-id="b030e-232">Let's declutter this page by taking advantage of some of those features.</span></span>

<span data-ttu-id="b030e-233">Ekleme bir [düzen sayfası](xref:mvc/views/layout) için *Pages/_Layout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="b030e-233">Add a [layout page](xref:mvc/views/layout) to *Pages/_Layout.cshtml*:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_LayoutSimple.cshtml)]

<span data-ttu-id="b030e-234">[Düzeni](xref:mvc/views/layout):</span><span class="sxs-lookup"><span data-stu-id="b030e-234">The [Layout](xref:mvc/views/layout):</span></span>

* <span data-ttu-id="b030e-235">(Sayfa düzeni dışında çevrilir sürece) her sayfanın düzenini denetler.</span><span class="sxs-lookup"><span data-stu-id="b030e-235">Controls the layout of each page (unless the page opts out of layout).</span></span>
* <span data-ttu-id="b030e-236">JavaScript ve stil sayfalarını gibi HTML yapıları alır.</span><span class="sxs-lookup"><span data-stu-id="b030e-236">Imports HTML structures such as JavaScript and stylesheets.</span></span>

<span data-ttu-id="b030e-237">Bkz: [düzen sayfası](xref:mvc/views/layout) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="b030e-237">See [layout page](xref:mvc/views/layout) for more information.</span></span>

<span data-ttu-id="b030e-238">[Düzeni](xref:mvc/views/layout#specifying-a-layout) özelliği ayarlanmış *Pages/_ViewStart.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="b030e-238">The [Layout](xref:mvc/views/layout#specifying-a-layout) property is set in *Pages/_ViewStart.cshtml*:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]

<span data-ttu-id="b030e-239">**Not:** düzeni bulunduğu *sayfaları* klasör.</span><span class="sxs-lookup"><span data-stu-id="b030e-239">**Note:** The layout is in the *Pages* folder.</span></span> <span data-ttu-id="b030e-240">Sayfaları başka görünümlerini (düzenleri, şablonlar, kısmi), geçerli sayfa ile aynı klasörde başlangıç hiyerarşik olarak arayın.</span><span class="sxs-lookup"><span data-stu-id="b030e-240">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="b030e-241">Bir düzende *sayfaları* klasörü altında herhangi bir Razor sayfadan kullanılabilir *sayfaları* klasör.</span><span class="sxs-lookup"><span data-stu-id="b030e-241">A layout in the *Pages* folder can be used from any Razor page under the *Pages* folder.</span></span>

<span data-ttu-id="b030e-242">Öneririz **değil** Düzen dosyası içine *görünümler/paylaşılan* klasör.</span><span class="sxs-lookup"><span data-stu-id="b030e-242">We recommend you **not** put the layout file in the *Views/Shared* folder.</span></span> <span data-ttu-id="b030e-243">*Görünümler/paylaşılan* bir MVC görünümleri deseni.</span><span class="sxs-lookup"><span data-stu-id="b030e-243">*Views/Shared* is an MVC views pattern.</span></span> <span data-ttu-id="b030e-244">Razor sayfalarının klasör hiyerarşisi, yol kuralları yararlanmayı yöneliktir.</span><span class="sxs-lookup"><span data-stu-id="b030e-244">Razor Pages are meant to rely on folder hierarchy, not path conventions.</span></span>

<span data-ttu-id="b030e-245">Bir Razor sayfa görünümü aramadan içerir *sayfaları* klasör.</span><span class="sxs-lookup"><span data-stu-id="b030e-245">View search from a Razor Page includes the *Pages* folder.</span></span> <span data-ttu-id="b030e-246">Düzenleri, şablonları ve MVC denetleyicileri ve geleneksel Razor görünümleri ile kullanmakta olduğunuz kısmi *yalnızca iş*.</span><span class="sxs-lookup"><span data-stu-id="b030e-246">The layouts, templates, and partials you're using with MVC controllers and conventional Razor views *just work*.</span></span>

<span data-ttu-id="b030e-247">Ekleme bir *Pages/_ViewImports.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="b030e-247">Add a *Pages/_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]

<span data-ttu-id="b030e-248">`@namespace` Daha sonra öğreticide açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="b030e-248">`@namespace` is explained later in the tutorial.</span></span> <span data-ttu-id="b030e-249">`@addTagHelper` Yönergesi getirir [yerleşik etiket Yardımcıları](xref:mvc/views/tag-helpers/builtin-th/Index) tüm sayfalar için *sayfaları* klasör.</span><span class="sxs-lookup"><span data-stu-id="b030e-249">The `@addTagHelper` directive brings in the [built-in Tag Helpers](xref:mvc/views/tag-helpers/builtin-th/Index) to all the pages in the *Pages* folder.</span></span>

<a name="namespace"></a>

<span data-ttu-id="b030e-250">Zaman `@namespace` yönergesi açıkça bir sayfada kullanılır:</span><span class="sxs-lookup"><span data-stu-id="b030e-250">When the `@namespace` directive is used explicitly on a page:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]

<span data-ttu-id="b030e-251">Yönergesi sayfa için ad alanı ayarlar.</span><span class="sxs-lookup"><span data-stu-id="b030e-251">The directive sets the namespace for the page.</span></span> <span data-ttu-id="b030e-252">`@model` Yönergesi olmayan ad alanı içerecek şekilde gerekir.</span><span class="sxs-lookup"><span data-stu-id="b030e-252">The `@model` directive doesn't need to include the namespace.</span></span>

<span data-ttu-id="b030e-253">Zaman `@namespace` yönergesi bulunduğu *_viewımports.cshtml*, belirtilen ad alanı içe aktaran sayfasındaki oluşturulan ad alanı öneki sağlayan `@namespace` yönergesi.</span><span class="sxs-lookup"><span data-stu-id="b030e-253">When the `@namespace` directive is contained in *_ViewImports.cshtml*, the specified namespace supplies the prefix for the generated namespace in the Page that imports the `@namespace` directive.</span></span> <span data-ttu-id="b030e-254">Oluşturulan ad alanı (soneki bölümü) kalanı içeren klasör arasında nokta ayrılmış göreli yol olup *_viewımports.cshtml* ve sayfayı içeren klasör.</span><span class="sxs-lookup"><span data-stu-id="b030e-254">The rest of the generated namespace (the suffix portion) is the dot-separated relative path between the folder containing *_ViewImports.cshtml* and the folder containing the page.</span></span>

<span data-ttu-id="b030e-255">Örneğin, dosyanın arkasındaki kod *Pages/Customers/Edit.cshtml.cs* ad alanını açıkça ayarlar:</span><span class="sxs-lookup"><span data-stu-id="b030e-255">For example, the code behind file *Pages/Customers/Edit.cshtml.cs* explicitly sets the namespace:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=snippet_namespace)]

<span data-ttu-id="b030e-256">*Pages/_ViewImports.cshtml* dosyasını ayarlar aşağıdaki ad alanı:</span><span class="sxs-lookup"><span data-stu-id="b030e-256">The *Pages/_ViewImports.cshtml* file sets the following namespace:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]

<span data-ttu-id="b030e-257">Oluşturulan ad alanı için *Pages/Customers/Edit.cshtml* Razor sayfasını aynıdır dosyanın arkasındaki kod.</span><span class="sxs-lookup"><span data-stu-id="b030e-257">The generated namespace for the *Pages/Customers/Edit.cshtml* Razor Page is the same as the code behind file.</span></span> <span data-ttu-id="b030e-258">`@namespace` Yönergesi tasarlandığı bir proje ve sayfaları üretilen kod için C# sınıfları eklenmiş şekilde *yalnızca iş* eklemek zorunda kalmadan bir `@using` dosyanın arkasındaki kod için yönerge.</span><span class="sxs-lookup"><span data-stu-id="b030e-258">The `@namespace` directive was designed so the C# classes added to a project and pages-generated code *just work* without having to add an `@using` directive for the code behind file.</span></span>

<span data-ttu-id="b030e-259">**Not:** `@namespace` geleneksel Razor görünümleri ile de çalışır.</span><span class="sxs-lookup"><span data-stu-id="b030e-259">**Note:** `@namespace` also works with conventional Razor views.</span></span>

<span data-ttu-id="b030e-260">Özgün *Pages/Create.cshtml* dosyasını görüntüle:</span><span class="sxs-lookup"><span data-stu-id="b030e-260">The original *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml?highlight=2)]

<span data-ttu-id="b030e-261">Güncelleştirilmiş *Pages/Create.cshtml* dosyasını görüntüle:</span><span class="sxs-lookup"><span data-stu-id="b030e-261">The updated *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/Create.cshtml?highlight=2)]

<span data-ttu-id="b030e-262">[Razor sayfalarının başlangıç projesi](#rpvs17) içeren *Pages/_ValidationScriptsPartial.cshtml*, istemci tarafı doğrulama kanca oluşturur.</span><span class="sxs-lookup"><span data-stu-id="b030e-262">The [Razor Pages starter project](#rpvs17) contains the *Pages/_ValidationScriptsPartial.cshtml*, which hooks up client-side validation.</span></span>

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a><span data-ttu-id="b030e-263">Sayfaları için URL oluşturma</span><span class="sxs-lookup"><span data-stu-id="b030e-263">URL generation for Pages</span></span>

<span data-ttu-id="b030e-264">`Create` Sayfasında, daha önce kullandığı gösterilen `RedirectToPage`:</span><span class="sxs-lookup"><span data-stu-id="b030e-264">The `Create` page, shown previously, uses `RedirectToPage`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=10)]

<span data-ttu-id="b030e-265">Uygulama, aşağıdaki dosya/klasör yapısını sahiptir:</span><span class="sxs-lookup"><span data-stu-id="b030e-265">The app has the following file/folder structure:</span></span>

* <span data-ttu-id="b030e-266">*/ Sayfaları*</span><span class="sxs-lookup"><span data-stu-id="b030e-266">*/Pages*</span></span>

  * <span data-ttu-id="b030e-267">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="b030e-267">*Index.cshtml*</span></span>
  * <span data-ttu-id="b030e-268">*/ Müşteri*</span><span class="sxs-lookup"><span data-stu-id="b030e-268">*/Customer*</span></span>

    * <span data-ttu-id="b030e-269">*Create.cshtml*</span><span class="sxs-lookup"><span data-stu-id="b030e-269">*Create.cshtml*</span></span>
    * <span data-ttu-id="b030e-270">*Edit.cshtml*</span><span class="sxs-lookup"><span data-stu-id="b030e-270">*Edit.cshtml*</span></span>
    * <span data-ttu-id="b030e-271">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="b030e-271">*Index.cshtml*</span></span>

<span data-ttu-id="b030e-272">*Pages/Customers/Create.cshtml* ve *Pages/Customers/Edit.cshtml* sayfaları yeniden yönlendirmek için *Pages/Index.cshtml* başarı sonra.</span><span class="sxs-lookup"><span data-stu-id="b030e-272">The *Pages/Customers/Create.cshtml* and *Pages/Customers/Edit.cshtml* pages redirect to *Pages/Index.cshtml* after success.</span></span> <span data-ttu-id="b030e-273">Dize `/Index` önceki sayfaya erişmek için URI bir parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="b030e-273">The string `/Index` is part of the URI to access the preceding page.</span></span> <span data-ttu-id="b030e-274">Dize `/Index` için URI oluşturmak için kullanılan *Pages/Index.cshtml* sayfası.</span><span class="sxs-lookup"><span data-stu-id="b030e-274">The string `/Index` can be used to generate URIs to the *Pages/Index.cshtml* page.</span></span> <span data-ttu-id="b030e-275">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="b030e-275">For example:</span></span>

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">My Index Page</a>`
* `RedirectToPage("/Index")`

<span data-ttu-id="b030e-276">Sayfa adı sayfasına kökünden yoludur */sayfaları* klasörü (başında dahil olmak üzere `/`, örneğin `/Index`).</span><span class="sxs-lookup"><span data-stu-id="b030e-276">The page name is the path to the page from the root */Pages* folder (including a leading `/`, for example `/Index`).</span></span> <span data-ttu-id="b030e-277">Önceki URL nesil yalnızca cmdlet'e kod bir URL daha çok daha zengin örneklerdir.</span><span class="sxs-lookup"><span data-stu-id="b030e-277">The preceding URL generation samples are much more feature rich than just hardcoding a URL.</span></span> <span data-ttu-id="b030e-278">URL oluşturma kullanan [yönlendirme](xref:mvc/controllers/routing) ve oluşturmak ve yol hedef yolunda nasıl tanımlandığına göre parametreleri kodlayın.</span><span class="sxs-lookup"><span data-stu-id="b030e-278">URL generation uses [routing](xref:mvc/controllers/routing) and can generate and encode parameters according to how the route is defined in the destination path.</span></span>

<span data-ttu-id="b030e-279">Göreli adlar sayfaları için URL oluşturmayı destekler.</span><span class="sxs-lookup"><span data-stu-id="b030e-279">URL generation for pages supports relative names.</span></span> <span data-ttu-id="b030e-280">Aşağıdaki tabloda hangi dizin sayfası ile farklı seçildiğini gösterir `RedirectToPage` parametrelerinden *Pages/Customers/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="b030e-280">The following table shows which Index page is selected with different `RedirectToPage` parameters from *Pages/Customers/Create.cshtml*:</span></span>

| <span data-ttu-id="b030e-281">RedirectToPage(x)</span><span class="sxs-lookup"><span data-stu-id="b030e-281">RedirectToPage(x)</span></span>| <span data-ttu-id="b030e-282">Sayfa</span><span class="sxs-lookup"><span data-stu-id="b030e-282">Page</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="b030e-283">RedirectToPage("/Index")</span><span class="sxs-lookup"><span data-stu-id="b030e-283">RedirectToPage("/Index")</span></span> | <span data-ttu-id="b030e-284">*Sayfa/dizini*</span><span class="sxs-lookup"><span data-stu-id="b030e-284">*Pages/Index*</span></span> |
| <span data-ttu-id="b030e-285">RedirectToPage("./Index");</span><span class="sxs-lookup"><span data-stu-id="b030e-285">RedirectToPage("./Index");</span></span> | <span data-ttu-id="b030e-286">*Müşteriler/sayfalar/dizini*</span><span class="sxs-lookup"><span data-stu-id="b030e-286">*Pages/Customers/Index*</span></span> |
| <span data-ttu-id="b030e-287">RedirectToPage(".. / Dizin")</span><span class="sxs-lookup"><span data-stu-id="b030e-287">RedirectToPage("../Index")</span></span> | <span data-ttu-id="b030e-288">*Sayfa/dizini*</span><span class="sxs-lookup"><span data-stu-id="b030e-288">*Pages/Index*</span></span> |
| <span data-ttu-id="b030e-289">RedirectToPage("Index")</span><span class="sxs-lookup"><span data-stu-id="b030e-289">RedirectToPage("Index")</span></span>  | <span data-ttu-id="b030e-290">*Müşteriler/sayfalar/dizini*</span><span class="sxs-lookup"><span data-stu-id="b030e-290">*Pages/Customers/Index*</span></span> |

<span data-ttu-id="b030e-291">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, ve `RedirectToPage("../Index")` olan *göreli adlar*.</span><span class="sxs-lookup"><span data-stu-id="b030e-291">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, and `RedirectToPage("../Index")`  are *relative names*.</span></span> <span data-ttu-id="b030e-292">`RedirectToPage` Parametresi *birleştirilmiş* ile hedef sayfanın adını işlem için geçerli sayfasının yolu.</span><span class="sxs-lookup"><span data-stu-id="b030e-292">The `RedirectToPage` parameter is *combined* with the path of the current page to compute the name of the destination page.</span></span>  <!-- Review: Original had The provided string is combined with the page name of the current page to compute the name of the destination page. -- page name, not page path -->

<span data-ttu-id="b030e-293">Göreli adı bağlama karmaşık bir yapıyı siteleriyle oluştururken yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="b030e-293">Relative name linking is useful when building sites with a complex structure.</span></span> <span data-ttu-id="b030e-294">Bir klasördeki sayfaları arasında bağlamak için göreli adları kullanıyorsa, bu klasörü yeniden adlandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b030e-294">If you use relative names to link between pages in a folder, you can rename that folder.</span></span> <span data-ttu-id="b030e-295">Tüm bağlantılar hala çalışır, (bunlar klasör adı eklemediniz çünkü).</span><span class="sxs-lookup"><span data-stu-id="b030e-295">All the links still work (because they didn't include the folder name).</span></span>

## <a name="tempdata"></a><span data-ttu-id="b030e-296">TempData</span><span class="sxs-lookup"><span data-stu-id="b030e-296">TempData</span></span>

<span data-ttu-id="b030e-297">ASP.NET Core sunan [TempData](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) özelliği bir [denetleyicisi](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller).</span><span class="sxs-lookup"><span data-stu-id="b030e-297">ASP.NET Core exposes the [TempData](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) property on a [controller](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller).</span></span> <span data-ttu-id="b030e-298">Bu özellik, dosyayı okuma kadar verileri depolar.</span><span class="sxs-lookup"><span data-stu-id="b030e-298">This property stores data until it's read.</span></span> <span data-ttu-id="b030e-299">`Keep` Ve `Peek` yöntemleri, verileri silme olmadan incelemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="b030e-299">The `Keep` and `Peek` methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="b030e-300">`TempData` birden çok tek bir istek için veri gerektiğinde yeniden yönlendirme için yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="b030e-300">`TempData` is  useful for redirection, when data is needed for more than a single request.</span></span>

<span data-ttu-id="b030e-301">`[TempData]` Özniteliği ASP.NET Core 2. 0 ' yenidir ve denetleyicileri ve sayfaları desteklenir.</span><span class="sxs-lookup"><span data-stu-id="b030e-301">The `[TempData]` attribute is new in ASP.NET Core 2.0 and is supported on controllers and pages.</span></span>

<span data-ttu-id="b030e-302">Aşağıdaki kodu değerini ayarlar `Message` kullanarak `TempData`:</span><span class="sxs-lookup"><span data-stu-id="b030e-302">The following code sets the value of `Message` using `TempData`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,25&name=snippet_Temp)]

<span data-ttu-id="b030e-303">Aşağıdaki biçimlendirmede *Pages/Customers/Index.cshtml* dosyasını değerini görüntüler `Message` kullanarak `TempData`.</span><span class="sxs-lookup"><span data-stu-id="b030e-303">The following markup in the *Pages/Customers/Index.cshtml* file displays the value of `Message` using `TempData`.</span></span>

```cshtml
<h3>Msg: @Model.Message</h3>
```

<span data-ttu-id="b030e-304">*Pages/Customers/Index.cshtml.cs* sayfa modeli uygular `[TempData]` özniteliğini `Message` özelliği.</span><span class="sxs-lookup"><span data-stu-id="b030e-304">The *Pages/Customers/Index.cshtml.cs* page model applies the `[TempData]` attribute to the `Message` property.</span></span>

```cs
[TempData]
public string Message { get; set; }
```

<span data-ttu-id="b030e-305">Bkz: [TempData](xref:fundamentals/app-state#temp) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="b030e-305">See [TempData](xref:fundamentals/app-state#temp) for more information.</span></span>

<a name="mhpp"></a>
## <a name="multiple-handlers-per-page"></a><span data-ttu-id="b030e-306">Sayfa başına birden çok işleyicileri</span><span class="sxs-lookup"><span data-stu-id="b030e-306">Multiple handlers per page</span></span>

<span data-ttu-id="b030e-307">İki kullanarak işleyicileri sayfa için aşağıdaki sayfayı biçimlendirmeleri oluşturur `asp-page-handler` etiket Yardımcısı:</span><span class="sxs-lookup"><span data-stu-id="b030e-307">The following page generates markup for two page handlers using the `asp-page-handler` Tag Helper:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

<!-- Review: the FormActionTagHelper applies to all <form /> elements on a Razor page, even when there's no `asp-` attribute   -->

<span data-ttu-id="b030e-308">Önceki örnekte form iki düğmeleri, her kullanarak gönderme sahip `FormActionTagHelper` farklı bir URL'ye göndermek için.</span><span class="sxs-lookup"><span data-stu-id="b030e-308">The form in the preceding example has two submit buttons, each using the `FormActionTagHelper` to submit to a different URL.</span></span> <span data-ttu-id="b030e-309">`asp-page-handler` Özniteliği için bir yardımcı olan `asp-page`.</span><span class="sxs-lookup"><span data-stu-id="b030e-309">The `asp-page-handler` attribute is a companion to `asp-page`.</span></span> <span data-ttu-id="b030e-310">`asp-page-handler` bir sayfa tarafından tanımlanan işleyici yöntemlerin her biri için gönderme URL'leri oluşturur.</span><span class="sxs-lookup"><span data-stu-id="b030e-310">`asp-page-handler` generates URLs that submit to each of the handler methods defined by a page.</span></span> <span data-ttu-id="b030e-311">`asp-page` Örnek geçerli sayfasına bağlantılandırma çünkü belirtilmedi.</span><span class="sxs-lookup"><span data-stu-id="b030e-311">`asp-page` isn't specified because the sample is linking to the current page.</span></span>

<span data-ttu-id="b030e-312">Sayfa modeli:</span><span class="sxs-lookup"><span data-stu-id="b030e-312">The page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]

<span data-ttu-id="b030e-313">Önceki kod kullanan *işleyici yöntemleri adlı*.</span><span class="sxs-lookup"><span data-stu-id="b030e-313">The preceding code uses *named handler methods*.</span></span> <span data-ttu-id="b030e-314">Adlandırılmış işleyici yöntemleri adından sonra metin gerçekleştirerek oluşturulur `On<HTTP Verb>` ve önce `Async` (varsa).</span><span class="sxs-lookup"><span data-stu-id="b030e-314">Named handler methods are created by taking the text in the name after `On<HTTP Verb>` and before `Async` (if present).</span></span> <span data-ttu-id="b030e-315">Önceki örnekte OnPost sayfa yöntemlerdir**JoinList**zaman uyumsuz ve OnPost**JoinListUC**zaman uyumsuz.</span><span class="sxs-lookup"><span data-stu-id="b030e-315">In the preceding example, the page methods are OnPost**JoinList**Async and OnPost**JoinListUC**Async.</span></span> <span data-ttu-id="b030e-316">İle *OnPost* ve *zaman uyumsuz* kaldırıldı, işleyici adlardır `JoinList` ve `JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="b030e-316">With *OnPost* and *Async* removed, the handler names are `JoinList` and `JoinListUC`.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?range=12-13)]

<span data-ttu-id="b030e-317">Yukarıdaki kullanan kod, gönderildiği URL yolunu `OnPostJoinListAsync` olan `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span><span class="sxs-lookup"><span data-stu-id="b030e-317">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span></span> <span data-ttu-id="b030e-318">Gönderildiği URL yolunu `OnPostJoinListUCAsync` olan `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="b030e-318">The URL path that submits to `OnPostJoinListUCAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span></span>

## <a name="customizing-routing"></a><span data-ttu-id="b030e-319">Yönlendirme özelleştirme</span><span class="sxs-lookup"><span data-stu-id="b030e-319">Customizing Routing</span></span>

<span data-ttu-id="b030e-320">Sorgu dizesi hoşlanmıyorsanız `?handler=JoinList` URL'de işleyicisi adı URL'nin yol kısmı koymak için rota değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b030e-320">If you don't like the query string `?handler=JoinList` in the URL, you can change the route to put the handler name in the path portion of the URL.</span></span> <span data-ttu-id="b030e-321">Bir rota şablonu sonra çift tırnak içine ekleyerek rota özelleştirebilirsiniz `@page` yönergesi.</span><span class="sxs-lookup"><span data-stu-id="b030e-321">You can customize the route by adding a route template enclosed in double quotes after the `@page` directive.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]

<span data-ttu-id="b030e-322">Önceki yol işleyicisi adı ve URL yolunu sorgu dizesi yerine koyar.</span><span class="sxs-lookup"><span data-stu-id="b030e-322">The preceding route puts the handler name in the URL path instead of the query string.</span></span> <span data-ttu-id="b030e-323">`?` Aşağıdaki `handler` rota parametresinin isteğe bağlı olduğu anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="b030e-323">The `?` following `handler` means the route parameter is optional.</span></span>

<span data-ttu-id="b030e-324">Kullanabileceğiniz `@page` ek kesimleri ve parametreleri bir sayfanın rotaya eklemek için.</span><span class="sxs-lookup"><span data-stu-id="b030e-324">You can use `@page` to add additional segments and parameters to a page's route.</span></span> <span data-ttu-id="b030e-325">Ne olursa olsun sahip **eklenmiş** sayfasının varsayılan yol için.</span><span class="sxs-lookup"><span data-stu-id="b030e-325">Whatever's there's **appended** to the default route of the page.</span></span> <span data-ttu-id="b030e-326">Sayfanın yolu değiştirmek için bir mutlak ya da sanal yolu kullanarak (gibi `"~/Some/Other/Path"`) desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="b030e-326">Using an absolute or virtual path to change the page's route (like `"~/Some/Other/Path"`) isn't supported.</span></span>

## <a name="configuration-and-settings"></a><span data-ttu-id="b030e-327">Yapılandırma ve ayarları</span><span class="sxs-lookup"><span data-stu-id="b030e-327">Configuration and settings</span></span>

<span data-ttu-id="b030e-328">Gelişmiş seçenekleri yapılandırmak için genişletme yöntemi kullanmak `AddRazorPagesOptions` MVC oluşturucu üzerinde:</span><span class="sxs-lookup"><span data-stu-id="b030e-328">To configure advanced options, use the extension method `AddRazorPagesOptions` on the MVC builder:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/StartupAdvanced.cs?name=snippet_1)]

<span data-ttu-id="b030e-329">Şu anda kullanabileceğiniz `RazorPagesOptions` sayfaları için kök dizini ayarlayın veya sayfaları için uygulama modeli kuralları eklemek için.</span><span class="sxs-lookup"><span data-stu-id="b030e-329">Currently you can use the `RazorPagesOptions` to set the root directory for pages, or add application model conventions for pages.</span></span> <span data-ttu-id="b030e-330">Gelecekte bu şekilde daha fazla genişletilebilirlik etkinleştirme.</span><span class="sxs-lookup"><span data-stu-id="b030e-330">We'll enable more extensibility this way in the future.</span></span>

<span data-ttu-id="b030e-331">Görünümleri derleneceği bkz [Razor görünüm derleme](xref:mvc/views/view-compilation) .</span><span class="sxs-lookup"><span data-stu-id="b030e-331">To precompile views, see [Razor view compilation](xref:mvc/views/view-compilation) .</span></span>

<span data-ttu-id="b030e-332">[İndirme veya görüntüleme örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/index/sample).</span><span class="sxs-lookup"><span data-stu-id="b030e-332">[Download or view sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/index/sample).</span></span>

<span data-ttu-id="b030e-333">Bkz: [Razor sayfalarının ile çalışmaya başlama](xref:tutorials/razor-pages/razor-pages-start), ilgili bu girişi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="b030e-333">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start), which builds on this introduction.</span></span>

### <a name="specify-that-razor-pages-are-at-the-content-root"></a><span data-ttu-id="b030e-334">Razor sayfalarının içerik kök dizininde olduğunu belirtin</span><span class="sxs-lookup"><span data-stu-id="b030e-334">Specify that Razor Pages are at the content root</span></span>

<span data-ttu-id="b030e-335">Razor sayfalarının olsunlar varsayılan olarak, */sayfaları* dizin.</span><span class="sxs-lookup"><span data-stu-id="b030e-335">By default, Razor Pages are rooted in the */Pages* directory.</span></span> <span data-ttu-id="b030e-336">Ekleme [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) için [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) Razor sayfalarınızı içerik kök dizininde olduğunu belirtmek için ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) uygulamasının:</span><span class="sxs-lookup"><span data-stu-id="b030e-336">Add [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at the content root ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) of the app:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesAtContentRoot();
```

### <a name="specify-that-razor-pages-are-at-a-custom-root-directory"></a><span data-ttu-id="b030e-337">Razor sayfalarının özel kök dizininde olduğunu belirtin</span><span class="sxs-lookup"><span data-stu-id="b030e-337">Specify that Razor Pages are at a custom root directory</span></span>

<span data-ttu-id="b030e-338">Ekleme [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) için [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) Razor sayfalarınızı uygulamasında özel kök dizininde olduğunu belirtmek için (göreli bir yol belirtin):</span><span class="sxs-lookup"><span data-stu-id="b030e-338">Add [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at a custom root directory in the app (provide a relative path):</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesRoot("/path/to/razor/pages");
```

## <a name="see-also"></a><span data-ttu-id="b030e-339">Ayrıca bkz.</span><span class="sxs-lookup"><span data-stu-id="b030e-339">See also</span></span>

* [<span data-ttu-id="b030e-340">ASP.NET Core giriş</span><span class="sxs-lookup"><span data-stu-id="b030e-340">Introduction to ASP.NET Core</span></span>](xref:index)
* [<span data-ttu-id="b030e-341">Razor Sayfaları kullanmaya başlama</span><span class="sxs-lookup"><span data-stu-id="b030e-341">Get started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="b030e-342">Razor sayfalarının yetkilendirme kuralları</span><span class="sxs-lookup"><span data-stu-id="b030e-342">Razor Pages authorization conventions</span></span>](xref:security/authorization/razor-pages-authorization)
* [<span data-ttu-id="b030e-343">Razor sayfalarının özel yolu ve sayfayı model sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="b030e-343">Razor Pages custom route and page model providers</span></span>](xref:mvc/razor-pages/razor-pages-convention-features)
* [<span data-ttu-id="b030e-344">Razor sayfalarının birim ve tümleştirme sınaması</span><span class="sxs-lookup"><span data-stu-id="b030e-344">Razor Pages unit and integration testing</span></span>](xref:testing/razor-pages-testing)
