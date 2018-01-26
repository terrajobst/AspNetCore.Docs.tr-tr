---
title: "ASP.NET Core Razor sayfalarında giriş"
author: Rick-Anderson
description: "Razor sayfalarında ASP.NET Core Öğreticisi. MVC çekirdek, ASP.NET Core içerir 2.x, web geliştirme ve Visual Studio 2017 giriş. Bu belge sayfası odaklı senaryoları geliştirme kolaylığı için ASP.NET Core Razor sayfalarını kullanarak genel bir bakış sağlar."
ms.author: riande
manager: wpickett
ms.date: 09/12/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/razor-pages/index
ms.openlocfilehash: a08c1b59c7be3a27fc11e6737a1cb4b4208f2901
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
# <a name="introduction-to-razor-pages-in-aspnet-core"></a><span data-ttu-id="1d041-105">ASP.NET Core Razor sayfalarında giriş</span><span class="sxs-lookup"><span data-stu-id="1d041-105">Introduction to Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="1d041-106">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Ryan Nowak](https://github.com/rynowak)</span><span class="sxs-lookup"><span data-stu-id="1d041-106">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Ryan Nowak](https://github.com/rynowak)</span></span>

<span data-ttu-id="1d041-107">Razor sayfalarının sayfa odaklı senaryoları daha kolay ve daha üretken kodlama yapar ASP.NET Core MVC yeni özelliğidir.</span><span class="sxs-lookup"><span data-stu-id="1d041-107">Razor Pages is a new feature of ASP.NET Core MVC that makes coding page-focused scenarios easier and more productive.</span></span>

<span data-ttu-id="1d041-108">Model-View-Controller yaklaşım kullanan bir öğretici için arıyorsanız bkz [ASP.NET Core MVC'ye Başlarken](xref:tutorials/first-mvc-app/start-mvc).</span><span class="sxs-lookup"><span data-stu-id="1d041-108">If you're looking for a tutorial that uses the Model-View-Controller approach, see [Getting started with ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span></span>

<span data-ttu-id="1d041-109">Bu belge Razor sayfalarının tanıtılmaktadır.</span><span class="sxs-lookup"><span data-stu-id="1d041-109">This document provides an introduction to Razor Pages.</span></span> <span data-ttu-id="1d041-110">Adım adım öğretici değil.</span><span class="sxs-lookup"><span data-stu-id="1d041-110">It's not a step by step tutorial.</span></span> <span data-ttu-id="1d041-111">Bazı bölümleri izleyin zor görürseniz, bkz: [Razor sayfalarının ile çalışmaya başlama](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="1d041-111">If you find some of the sections difficult to follow, see [Getting started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span></span>

<a name="prerequisites"></a>

## <a name="aspnet-core-20-prerequisites"></a><span data-ttu-id="1d041-112">ASP.NET Core 2.0 önkoşulları</span><span class="sxs-lookup"><span data-stu-id="1d041-112">ASP.NET Core 2.0 prerequisites</span></span>

<span data-ttu-id="1d041-113">Yükleme [.NET Core](https://www.microsoft.com/net/core) 2.0.0 veya sonraki bir sürümü.</span><span class="sxs-lookup"><span data-stu-id="1d041-113">Install [.NET Core](https://www.microsoft.com/net/core) 2.0.0 or later.</span></span>

<span data-ttu-id="1d041-114">Visual Studio kullanıyorsanız, yükleme [Visual Studio](https://www.visualstudio.com/vs/) 2017 15.3 veya aşağıdaki iş yükleri ile sonraki bir sürümü:</span><span class="sxs-lookup"><span data-stu-id="1d041-114">If you're using Visual Studio, install [Visual Studio](https://www.visualstudio.com/vs/) 2017 version 15.3 or later with the following workloads:</span></span>

* <span data-ttu-id="1d041-115">**ASP.NET ve web geliştirme**</span><span class="sxs-lookup"><span data-stu-id="1d041-115">**ASP.NET and web development**</span></span>
* <span data-ttu-id="1d041-116">**.NET core platformlar arası geliştirme**</span><span class="sxs-lookup"><span data-stu-id="1d041-116">**.NET Core cross-platform development**</span></span>

<a name="rpvs17"></a>

## <a name="creating-a-razor-pages-project"></a><span data-ttu-id="1d041-117">Razor sayfalarının proje oluşturma</span><span class="sxs-lookup"><span data-stu-id="1d041-117">Creating a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1d041-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1d041-118">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="1d041-119">Bkz: [Razor sayfalarının ile çalışmaya başlama](xref:tutorials/razor-pages/razor-pages-start) Razor sayfalarının oluşturma konusunda ayrıntılı yönergeler için Proje Visual Studio kullanarak.</span><span class="sxs-lookup"><span data-stu-id="1d041-119">See [Getting started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) for detailed instructions on how to create a Razor Pages project using Visual Studio.</span></span>

#   <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="1d041-120">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1d041-120">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="1d041-121">Çalıştırma `dotnet new razor` komut satırından.</span><span class="sxs-lookup"><span data-stu-id="1d041-121">Run `dotnet new razor` from the command line.</span></span>

<span data-ttu-id="1d041-122">Oluşturulan açmak *.csproj* Visual Studio dosyasından Mac için</span><span class="sxs-lookup"><span data-stu-id="1d041-122">Open the generated *.csproj* file from Visual Studio for Mac.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1d041-123">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1d041-123">Visual Studio Code</span></span>](#tab/visual-studio-code) 

<span data-ttu-id="1d041-124">Çalıştırma `dotnet new razor` komut satırından.</span><span class="sxs-lookup"><span data-stu-id="1d041-124">Run `dotnet new razor` from the command line.</span></span>

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="1d041-125">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="1d041-125">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="1d041-126">Çalıştırma `dotnet new razor` komut satırından.</span><span class="sxs-lookup"><span data-stu-id="1d041-126">Run `dotnet new razor` from the command line.</span></span>

---

## <a name="razor-pages"></a><span data-ttu-id="1d041-127">Razor sayfalarının</span><span class="sxs-lookup"><span data-stu-id="1d041-127">Razor Pages</span></span>

<span data-ttu-id="1d041-128">Razor sayfalarının etkinleşir *haline*:</span><span class="sxs-lookup"><span data-stu-id="1d041-128">Razor Pages is enabled in *Startup.cs*:</span></span>

[!code-cs[main](index/sample/RazorPagesIntro/Startup.cs?name=snippet_Startup)]

<span data-ttu-id="1d041-129">Temel bir sayfa göz önünde bulundurun:<a name="OnGet"></a></span><span class="sxs-lookup"><span data-stu-id="1d041-129">Consider a basic page: <a name="OnGet"></a></span></span>

[!code-cshtml[main](index/sample/RazorPagesIntro/Pages/Index.cshtml)]

<span data-ttu-id="1d041-130">Önceki kod çok Razor görünüm dosyası gibi görünüyor.</span><span class="sxs-lookup"><span data-stu-id="1d041-130">The preceding code looks a lot like a Razor view file.</span></span> <span data-ttu-id="1d041-131">Neler farklı kılan unsurdur `@page` yönergesi.</span><span class="sxs-lookup"><span data-stu-id="1d041-131">What makes it different is the `@page` directive.</span></span> <span data-ttu-id="1d041-132">`@page`Dosya, isteklerini doğrudan bir denetleyici geçmeden işleme anlamına gelir ve MVC eylemi - içine yapar.</span><span class="sxs-lookup"><span data-stu-id="1d041-132">`@page` makes the file into an MVC action - which means that it handles requests directly, without going through a controller.</span></span> <span data-ttu-id="1d041-133">`@page`ilk Razor yönergesi bir sayfa üzerinde olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="1d041-133">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="1d041-134">`@page`diğer Razor yapıları davranışını etkiler.</span><span class="sxs-lookup"><span data-stu-id="1d041-134">`@page` affects the behavior of other Razor constructs.</span></span>

<span data-ttu-id="1d041-135">Benzer bir sayfa, kullanarak bir `PageModel` sınıfında, aşağıdaki iki dosyada gösterilir.</span><span class="sxs-lookup"><span data-stu-id="1d041-135">A similar page, using a `PageModel` class, is shown in the following two files.</span></span> <span data-ttu-id="1d041-136">*Pages/Index2.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="1d041-136">The *Pages/Index2.cshtml* file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesIntro/Pages/Index2.cshtml)]

<span data-ttu-id="1d041-137">*Pages/Index2.cshtml.cs* "arka plan kod" dosyası:</span><span class="sxs-lookup"><span data-stu-id="1d041-137">The *Pages/Index2.cshtml.cs* "code-behind" file:</span></span>

[!code-cs[main](index/sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]

<span data-ttu-id="1d041-138">Kural tarafından `PageModel` sınıf dosyası Razor sayfa dosyası ile aynı ada sahip *.cs* eklenir.</span><span class="sxs-lookup"><span data-stu-id="1d041-138">By convention, the `PageModel` class file has the same name as the Razor Page file with *.cs* appended.</span></span> <span data-ttu-id="1d041-139">Örneğin, önceki Razor sayfasıdır *Pages/Index2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="1d041-139">For example, the previous Razor Page is *Pages/Index2.cshtml*.</span></span> <span data-ttu-id="1d041-140">Dosyayı içeren `PageModel` sınıfı adlandırılan *Pages/Index2.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="1d041-140">The file containing the `PageModel` class is named *Pages/Index2.cshtml.cs*.</span></span>

<span data-ttu-id="1d041-141">URL yollarını sayfalara ilişkilendirmelerini dosya sisteminde sayfanın konuma göre belirlenir.</span><span class="sxs-lookup"><span data-stu-id="1d041-141">The associations of URL paths to pages are determined by the page's location in the file system.</span></span> <span data-ttu-id="1d041-142">Aşağıdaki tablo Razor sayfasının yolunu ve eşleşen URL gösterir:</span><span class="sxs-lookup"><span data-stu-id="1d041-142">The following table shows a Razor Page path and the matching URL:</span></span>

| <span data-ttu-id="1d041-143">Dosya adı ve yolu</span><span class="sxs-lookup"><span data-stu-id="1d041-143">File name and path</span></span>               | <span data-ttu-id="1d041-144">URL eşleştirme</span><span class="sxs-lookup"><span data-stu-id="1d041-144">matching URL</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="1d041-145">*/Pages/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="1d041-145">*/Pages/Index.cshtml*</span></span> | <span data-ttu-id="1d041-146">`/`veya`/Index`</span><span class="sxs-lookup"><span data-stu-id="1d041-146">`/` or `/Index`</span></span> |
| <span data-ttu-id="1d041-147">*/Pages/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="1d041-147">*/Pages/Contact.cshtml*</span></span> | `/Contact` |
| <span data-ttu-id="1d041-148">*/Pages/Store/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="1d041-148">*/Pages/Store/Contact.cshtml*</span></span> | `/Store/Contact` |
| <span data-ttu-id="1d041-149">*/Pages/Store/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="1d041-149">*/Pages/Store/Index.cshtml*</span></span> | <span data-ttu-id="1d041-150">`/Store`veya`/Store/Index`</span><span class="sxs-lookup"><span data-stu-id="1d041-150">`/Store` or `/Store/Index`</span></span> |

<span data-ttu-id="1d041-151">Notlar:</span><span class="sxs-lookup"><span data-stu-id="1d041-151">Notes:</span></span>

* <span data-ttu-id="1d041-152">Razor sayfalarının dosyalarında çalışma zamanı arar *sayfaları* varsayılan klasör.</span><span class="sxs-lookup"><span data-stu-id="1d041-152">The runtime looks for Razor Pages files in the *Pages* folder by default.</span></span>
* <span data-ttu-id="1d041-153">`Index`bir URL bir sayfa içermediğinde varsayılan sayfasıdır.</span><span class="sxs-lookup"><span data-stu-id="1d041-153">`Index` is the default page when a URL doesn't include a page.</span></span>

## <a name="writing-a-basic-form"></a><span data-ttu-id="1d041-154">Temel form yazma</span><span class="sxs-lookup"><span data-stu-id="1d041-154">Writing a basic form</span></span>

<span data-ttu-id="1d041-155">Razor sayfalarının özellikleri ortak desenler web tarayıcılarıyla birlikte kullanılan kolay hale getirmek üzere tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="1d041-155">Razor Pages features are designed to make common patterns used with web browsers easy.</span></span> <span data-ttu-id="1d041-156">[Model bağlama](xref:mvc/models/model-binding), [etiket Yardımcıları](xref:mvc/views/tag-helpers/intro)ve HTML Yardımcıları tüm *yalnızca iş* Razor sayfasını sınıfında tanımlanan özelliklere sahip.</span><span class="sxs-lookup"><span data-stu-id="1d041-156">[Model binding](xref:mvc/models/model-binding), [Tag Helpers](xref:mvc/views/tag-helpers/intro), and HTML helpers all *just work* with the properties defined in a Razor Page class.</span></span> <span data-ttu-id="1d041-157">"Bize başvurun" form için temel bir uygulayan bir sayfa göz önünde bulundurun `Contact` modeli:</span><span class="sxs-lookup"><span data-stu-id="1d041-157">Consider a page that implements a basic "contact us" form for the `Contact` model:</span></span>

<span data-ttu-id="1d041-158">Bu belgede, örnekleri için `DbContext` içinde başlatılan [haline](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) dosya.</span><span class="sxs-lookup"><span data-stu-id="1d041-158">For the samples in this document, the `DbContext` is initialized in the [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) file.</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Startup.cs?highlight=15-16)]

<span data-ttu-id="1d041-159">Veri modeli:</span><span class="sxs-lookup"><span data-stu-id="1d041-159">The data model:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Data/Customer.cs)]

<span data-ttu-id="1d041-160">Db bağlamı:</span><span class="sxs-lookup"><span data-stu-id="1d041-160">The db context:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Data/AppDbContext.cs)]

<span data-ttu-id="1d041-161">*Pages/Create.cshtml* dosyasını görüntüle:</span><span class="sxs-lookup"><span data-stu-id="1d041-161">The *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Create.cshtml)]

<span data-ttu-id="1d041-162">*Pages/Create.cshtml.cs* görünümü için arka plan kod dosyası:</span><span class="sxs-lookup"><span data-stu-id="1d041-162">The *Pages/Create.cshtml.cs* code-behind file for the view:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_ALL)]

<span data-ttu-id="1d041-163">Kural tarafından `PageModel` sınıfı çağrıldığında `<PageName>Model` ve sayfa aynı ad.</span><span class="sxs-lookup"><span data-stu-id="1d041-163">By convention, the `PageModel` class is called `<PageName>Model` and is in the same namespace as the page.</span></span>

<span data-ttu-id="1d041-164">`PageModel` Sınıfı, bir sayfa mantığı ayrılması kendi sunudan olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="1d041-164">The `PageModel` class allows separation of the logic of a page from its presentation.</span></span> <span data-ttu-id="1d041-165">Sayfaya gönderilen istekleri ve sayfayı oluşturmak için kullanılan verileri için sayfa işleyiciler tanımlar.</span><span class="sxs-lookup"><span data-stu-id="1d041-165">It defines page handlers for requests sent to the page and the data used to render the page.</span></span> <span data-ttu-id="1d041-166">Bu ayrım aracılığıyla sayfa bağımlılıklar yönetmenize olanak sağlayan [bağımlılık ekleme](xref:fundamentals/dependency-injection) ve [birim testi](xref:testing/razor-pages-testing) sayfaları.</span><span class="sxs-lookup"><span data-stu-id="1d041-166">This separation allows you to manage page dependencies through [dependency injection](xref:fundamentals/dependency-injection) and to [unit test](xref:testing/razor-pages-testing) the pages.</span></span>

<span data-ttu-id="1d041-167">Sayfasına sahip bir `OnPostAsync` *işleyici yöntemi*, üzerinde çalıştığı `POST` ister (kullanıcı formu gönderdiğinde).</span><span class="sxs-lookup"><span data-stu-id="1d041-167">The page has an `OnPostAsync` *handler method*, which runs on `POST` requests (when a user posts the form).</span></span> <span data-ttu-id="1d041-168">Herhangi bir HTTP fiil için işleyici yöntemleri ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1d041-168">You can add handler methods for any HTTP verb.</span></span> <span data-ttu-id="1d041-169">En yaygın işleyicileri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="1d041-169">The most common handlers are:</span></span>

* <span data-ttu-id="1d041-170">`OnGet`sayfa için gereken durumu başlatılamadı.</span><span class="sxs-lookup"><span data-stu-id="1d041-170">`OnGet` to initialize state needed for the page.</span></span> <span data-ttu-id="1d041-171">[OnGet](#OnGet) örnek.</span><span class="sxs-lookup"><span data-stu-id="1d041-171">[OnGet](#OnGet) sample.</span></span>
* <span data-ttu-id="1d041-172">`OnPost`Form Gönderme işlemek için.</span><span class="sxs-lookup"><span data-stu-id="1d041-172">`OnPost` to handle form submissions.</span></span>

<span data-ttu-id="1d041-173">`Async` Adlandırma soneki isteğe bağlıdır ancak genellikle kurala göre zaman uyumsuz işlevleri için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1d041-173">The `Async` naming suffix is optional but is often used by convention for asynchronous functions.</span></span> <span data-ttu-id="1d041-174">`OnPostAsync` Önceki örnekte kod ne, normalde bir denetleyicisi yazarsınız için benzer görünür.</span><span class="sxs-lookup"><span data-stu-id="1d041-174">The `OnPostAsync` code in the preceding example looks similar to what you would normally write in a controller.</span></span> <span data-ttu-id="1d041-175">Önceki kod Razor sayfalar için normaldir.</span><span class="sxs-lookup"><span data-stu-id="1d041-175">The preceding code is typical for Razor Pages.</span></span> <span data-ttu-id="1d041-176">MVC elemanlar çoğu ister [model bağlama](xref:mvc/models/model-binding), [doğrulama](xref:mvc/models/validation), ve eylem sonuçlarını paylaşıldığı.</span><span class="sxs-lookup"><span data-stu-id="1d041-176">Most of the MVC primitives like [model binding](xref:mvc/models/model-binding), [validation](xref:mvc/models/validation), and action results are shared.</span></span>  <!-- Review: Ryan, can we get a list of what is shared and what isn't? -->

<span data-ttu-id="1d041-177">Önceki `OnPostAsync` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="1d041-177">The previous `OnPostAsync` method:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="1d041-178">Temel akışı `OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="1d041-178">The basic flow of `OnPostAsync`:</span></span>

<span data-ttu-id="1d041-179">Doğrulama hataları denetleyin.</span><span class="sxs-lookup"><span data-stu-id="1d041-179">Check for validation errors.</span></span>

*  <span data-ttu-id="1d041-180">Herhangi bir hata varsa, verileri Kaydet ve yeniden yönlendirme.</span><span class="sxs-lookup"><span data-stu-id="1d041-180">If there are no errors, save the data and redirect.</span></span>
*  <span data-ttu-id="1d041-181">Hatalar varsa, doğrulama iletileri sayfasıyla yeniden gösterir.</span><span class="sxs-lookup"><span data-stu-id="1d041-181">If there are errors, show the page again with validation messages.</span></span> <span data-ttu-id="1d041-182">İstemci tarafı doğrulama geleneksel ASP.NET Core MVC uygulamaları için aynıdır.</span><span class="sxs-lookup"><span data-stu-id="1d041-182">Client-side validation is identical to traditional ASP.NET Core MVC applications.</span></span> <span data-ttu-id="1d041-183">Çoğu durumda, doğrulama hataları istemcide algılanabilir ve hiçbir zaman sunucuya gönderildi.</span><span class="sxs-lookup"><span data-stu-id="1d041-183">In many cases, validation errors would be detected on the client, and never submitted to the server.</span></span>

<span data-ttu-id="1d041-184">Veri başarıyla girildiğinde `OnPostAsync` işleyici yöntem çağrılarını `RedirectToPage` bir örneğini döndürmek için yardımcı yöntem `RedirectToPageResult`.</span><span class="sxs-lookup"><span data-stu-id="1d041-184">When the data is entered successfully, the `OnPostAsync` handler method calls the `RedirectToPage` helper method to return an instance of `RedirectToPageResult`.</span></span> <span data-ttu-id="1d041-185">`RedirectToPage`Yeni eylem sonucu, benzer olduğunu `RedirectToAction` veya `RedirectToRoute`, ancak özelleştirilmiş sayfalar için.</span><span class="sxs-lookup"><span data-stu-id="1d041-185">`RedirectToPage` is a new action result, similar to `RedirectToAction` or `RedirectToRoute`, but customized for pages.</span></span> <span data-ttu-id="1d041-186">İsteğe bağlı olarak önceki örnekte kök dizin sayfasına yönlendiren (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="1d041-186">In the preceding sample, it redirects to the root Index page (`/Index`).</span></span> <span data-ttu-id="1d041-187">`RedirectToPage`içinde ayrıntılı [sayfaları için URL oluşturma](#url_gen) bölümü.</span><span class="sxs-lookup"><span data-stu-id="1d041-187">`RedirectToPage` is detailed in the [URL generation for Pages](#url_gen) section.</span></span>

<span data-ttu-id="1d041-188">Teslim edilen formu (yani sunucuya geçirilir) doğrulama hataları olduğunda`OnPostAsync` işleyici yöntem çağrılarını `Page` yardımcı yöntemi.</span><span class="sxs-lookup"><span data-stu-id="1d041-188">When the submitted form has validation errors (that are passed to the server), the`OnPostAsync` handler method calls the `Page` helper method.</span></span> <span data-ttu-id="1d041-189">`Page`örneğini döndürür `PageResult`.</span><span class="sxs-lookup"><span data-stu-id="1d041-189">`Page` returns an instance of `PageResult`.</span></span> <span data-ttu-id="1d041-190">Döndürme `Page` nasıl denetleyicileri Eylemler döndürmek için benzer `View`.</span><span class="sxs-lookup"><span data-stu-id="1d041-190">Returning `Page` is similar to how actions in controllers return `View`.</span></span> <span data-ttu-id="1d041-191">`PageResult`Varsayılan değer <!-- Review  --> dönüş türü için bir işleyici yöntemi.</span><span class="sxs-lookup"><span data-stu-id="1d041-191">`PageResult` is the default <!-- Review  --> return type for a handler method.</span></span> <span data-ttu-id="1d041-192">Döndüren bir işleyici yöntemi `void` sayfasını işler.</span><span class="sxs-lookup"><span data-stu-id="1d041-192">A handler method that returns `void` renders the page.</span></span>

<span data-ttu-id="1d041-193">`Customer` Özelliğini kullanan `[BindProperty]` model bağlama için kabul özniteliği.</span><span class="sxs-lookup"><span data-stu-id="1d041-193">The `Customer` property uses `[BindProperty]` attribute to opt in to model binding.</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_PageModel&highlight=10-11)]

<span data-ttu-id="1d041-194">Razor sayfalarının varsayılan olarak, GET olmayan fiiller yalnızca özelliklerle bağlayın.</span><span class="sxs-lookup"><span data-stu-id="1d041-194">Razor Pages, by default, bind properties only with non-GET verbs.</span></span> <span data-ttu-id="1d041-195">Bağlama özellikleri için kod yazmak zorunda miktarını azaltır.</span><span class="sxs-lookup"><span data-stu-id="1d041-195">Binding to properties can reduce the amount of code you have to write.</span></span> <span data-ttu-id="1d041-196">Bağlama form alanları oluşturmak için aynı özelliği kullanarak kod azaltır (`<input asp-for="Customer.Name" />`) ve giriş kabul edin.</span><span class="sxs-lookup"><span data-stu-id="1d041-196">Binding reduces code by using the same property to render form fields (`<input asp-for="Customer.Name" />`) and accept the input.</span></span>

<span data-ttu-id="1d041-197">Giriş sayfası (*Index.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="1d041-197">The home page (*Index.cshtml*):</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Index.cshtml)]

<span data-ttu-id="1d041-198">Arka plan kod *Index.cshtml.cs* dosyası:</span><span class="sxs-lookup"><span data-stu-id="1d041-198">The code behind *Index.cshtml.cs* file:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs)]

<span data-ttu-id="1d041-199">*Index.cshtml* dosyası her kişi için bir düzenleme bağlantısı oluşturmak için aşağıdaki biçimlendirme içerir:</span><span class="sxs-lookup"><span data-stu-id="1d041-199">The *Index.cshtml* file contains the following markup to create an edit link for each contact:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=21)]

<span data-ttu-id="1d041-200">[Yer işareti etiketi yardımcı](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) kullanılan `asp-route-{value}` düzenleme sayfasını bir bağlantı oluşturmak için öznitelik.</span><span class="sxs-lookup"><span data-stu-id="1d041-200">The [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) used the `asp-route-{value}` attribute to generate a link to the Edit page.</span></span> <span data-ttu-id="1d041-201">Rota verilerini kişiyle bağlantıyı içeren kimliği</span><span class="sxs-lookup"><span data-stu-id="1d041-201">The link contains route data with the contact ID.</span></span> <span data-ttu-id="1d041-202">Örneğin, `http://localhost:5000/Edit/1`.</span><span class="sxs-lookup"><span data-stu-id="1d041-202">For example, `http://localhost:5000/Edit/1`.</span></span>

<span data-ttu-id="1d041-203">*Pages/Edit.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="1d041-203">The *Pages/Edit.cshtml* file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Edit.cshtml?highlight=1)]

<span data-ttu-id="1d041-204">İlk satır içerir `@page "{id:int}"` yönergesi.</span><span class="sxs-lookup"><span data-stu-id="1d041-204">The first line contains the `@page "{id:int}"` directive.</span></span> <span data-ttu-id="1d041-205">Yönlendirme kısıtlaması`"{id:int}"` içeren sayfa isteklerini kabul etmek için sayfayı söyler `int` rota verileri.</span><span class="sxs-lookup"><span data-stu-id="1d041-205">The routing constraint`"{id:int}"` tells the page to accept requests to the page that contain `int` route data.</span></span> <span data-ttu-id="1d041-206">Sayfa için bir istek dönüştürülebilir rota verileri içermiyorsa, bir `int`, çalışma zamanı (bulunamadı) HTTP 404 hatası döndürür.</span><span class="sxs-lookup"><span data-stu-id="1d041-206">If a request to the page doesn't contain route data that can be converted to an `int`, the runtime returns an HTTP 404 (not found) error.</span></span>

<span data-ttu-id="1d041-207">*Pages/Edit.cshtml.cs* dosyası:</span><span class="sxs-lookup"><span data-stu-id="1d041-207">The *Pages/Edit.cshtml.cs* file:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Edit.cshtml.cs)]

<span data-ttu-id="1d041-208">*Index.cshtml* dosya de Sil düğmesini her müşteri irtibat için oluşturmak için biçimlendirme içerir:</span><span class="sxs-lookup"><span data-stu-id="1d041-208">The *Index.cshtml* file also contains markup to create a delete button for each customer contact:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=22-23)]

<span data-ttu-id="1d041-209">Sil düğmesini HTML olarak işlendiğinde, `formaction` parametreleri içerir:</span><span class="sxs-lookup"><span data-stu-id="1d041-209">When the delete button is rendered in HTML, its `formaction` includes parameters for:</span></span>

* <span data-ttu-id="1d041-210">Müşteri tarafından belirtilen kimliği başvurun `asp-route-id` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="1d041-210">The customer contact ID specified by the `asp-route-id` attribute.</span></span>
* <span data-ttu-id="1d041-211">`handler` Tarafından belirtilen `asp-page-handler` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="1d041-211">The `handler` specified by the `asp-page-handler` attribute.</span></span>

<span data-ttu-id="1d041-212">Bir müşteri ile işlenen Sil düğmesini bir örneği burada verilmiştir Kimliğini başvurun `1`:</span><span class="sxs-lookup"><span data-stu-id="1d041-212">Here is an example of a rendered delete button with a customer contact ID of `1`:</span></span>

```html
<button type="submit" formaction="/?id=1&amp;handler=delete">delete</button>
```

<span data-ttu-id="1d041-213">Düğme seçildiğinde, bir form `POST` isteği sunucuya gönderilir.</span><span class="sxs-lookup"><span data-stu-id="1d041-213">When the button is selected, a form `POST` request is sent to the server.</span></span> <span data-ttu-id="1d041-214">Kurala göre işleyicisi yönteminin adı seçili değerine göre `handler` parametre düzeni göre `OnPost[handler]Async`.</span><span class="sxs-lookup"><span data-stu-id="1d041-214">By convention, the name of the handler method is selected based the value of the `handler` parameter according to the scheme `OnPost[handler]Async`.</span></span>

<span data-ttu-id="1d041-215">Çünkü `handler` olan `delete` Bu örnekte, `OnPostDeleteAsync` işleyici yöntemi kullanılır işleme `POST` isteği.</span><span class="sxs-lookup"><span data-stu-id="1d041-215">Because the `handler` is `delete` in this example, the `OnPostDeleteAsync` handler method is used to process the `POST` request.</span></span> <span data-ttu-id="1d041-216">Varsa `asp-page-handler` gibi farklı bir değere ayarlanmış `remove`, ada sahip bir sayfa işleyici yöntemi `OnPostRemoveAsync` seçilir.</span><span class="sxs-lookup"><span data-stu-id="1d041-216">If the `asp-page-handler` is set to a different value, such as `remove`, a page handler method with the name `OnPostRemoveAsync` is selected.</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs?range=26-37)]

<span data-ttu-id="1d041-217">`OnPostDeleteAsync` Yöntemi:</span><span class="sxs-lookup"><span data-stu-id="1d041-217">The `OnPostDeleteAsync` method:</span></span>

* <span data-ttu-id="1d041-218">Kabul `id` Sorgu dizesinden.</span><span class="sxs-lookup"><span data-stu-id="1d041-218">Accepts the `id` from the query string.</span></span>
* <span data-ttu-id="1d041-219">Müşteri irtibat ile veritabanını sorgular `FindAsync`.</span><span class="sxs-lookup"><span data-stu-id="1d041-219">Queries the database for the customer contact with `FindAsync`.</span></span>
* <span data-ttu-id="1d041-220">Müşteri irtibat bulunursa, müşteri kişiler listesinden kaldırıldı.</span><span class="sxs-lookup"><span data-stu-id="1d041-220">If the customer contact is found, they're removed from the list of customer contacts.</span></span> <span data-ttu-id="1d041-221">Veritabanı güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="1d041-221">The database is updated.</span></span>
* <span data-ttu-id="1d041-222">Çağrıları `RedirectToPage` kök dizin sayfasına yeniden yönlendirmek için (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="1d041-222">Calls `RedirectToPage` to redirect to the root Index page (`/Index`).</span></span>

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a><span data-ttu-id="1d041-223">XSRF/CSRF ve Razor sayfaları</span><span class="sxs-lookup"><span data-stu-id="1d041-223">XSRF/CSRF and Razor Pages</span></span>

<span data-ttu-id="1d041-224">Herhangi bir kod yazmak zorunda değilsiniz [antiforgery doğrulama](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="1d041-224">You don't have to write any code for [antiforgery validation](xref:security/anti-request-forgery).</span></span> <span data-ttu-id="1d041-225">Otomatik olarak antiforgery belirteci oluşturma ve doğrulama Razor sayfalarında dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="1d041-225">Antiforgery token generation and validation are automatically included in Razor Pages.</span></span>

<a name="layout"></a>
## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a><span data-ttu-id="1d041-226">Düzenleri, kısmi, şablonları ve etiket Yardımcıları Razor Pages ile kullanma</span><span class="sxs-lookup"><span data-stu-id="1d041-226">Using Layouts, partials, templates, and Tag Helpers with Razor Pages</span></span>

<span data-ttu-id="1d041-227">Sayfaları tüm özellikleri Razor görüntüleme altyapısı ile çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="1d041-227">Pages work with all the features of the Razor view engine.</span></span> <span data-ttu-id="1d041-228">Düzenleri, kısmi, şablonları, etiket Yardımcıları *_ViewStart.cshtml*, *_viewımports.cshtml* iş için geleneksel Razor görünümleri yaparlar aynı şekilde.</span><span class="sxs-lookup"><span data-stu-id="1d041-228">Layouts, partials, templates, Tag Helpers, *_ViewStart.cshtml*, *_ViewImports.cshtml* work in the same way they do for conventional Razor views.</span></span>

<span data-ttu-id="1d041-229">Şimdi bu sayfa bu özelliklerden bazıları yararlanarak declutter.</span><span class="sxs-lookup"><span data-stu-id="1d041-229">Let's declutter this page by taking advantage of some of those features.</span></span>

<span data-ttu-id="1d041-230">Ekleme bir [düzen sayfası](xref:mvc/views/layout) için *Pages/_Layout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="1d041-230">Add a [layout page](xref:mvc/views/layout) to *Pages/_Layout.cshtml*:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_LayoutSimple.cshtml)]

<span data-ttu-id="1d041-231">[Düzeni](xref:mvc/views/layout):</span><span class="sxs-lookup"><span data-stu-id="1d041-231">The [Layout](xref:mvc/views/layout):</span></span>

* <span data-ttu-id="1d041-232">(Sayfa düzeni dışında çevrilir sürece) her sayfanın düzenini denetler.</span><span class="sxs-lookup"><span data-stu-id="1d041-232">Controls the layout of each page (unless the page opts out of layout).</span></span>
* <span data-ttu-id="1d041-233">JavaScript ve stil sayfalarını gibi HTML yapıları alır.</span><span class="sxs-lookup"><span data-stu-id="1d041-233">Imports HTML structures such as JavaScript and stylesheets.</span></span>

<span data-ttu-id="1d041-234">Bkz: [düzen sayfası](xref:mvc/views/layout) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="1d041-234">See [layout page](xref:mvc/views/layout) for more information.</span></span>

<span data-ttu-id="1d041-235">[Düzeni](xref:mvc/views/layout#specifying-a-layout) özelliği ayarlanmış *Pages/_ViewStart.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="1d041-235">The [Layout](xref:mvc/views/layout#specifying-a-layout) property is set in *Pages/_ViewStart.cshtml*:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]

<span data-ttu-id="1d041-236">**Not:** düzeni bulunduğu *sayfaları* klasör.</span><span class="sxs-lookup"><span data-stu-id="1d041-236">**Note:** The layout is in the *Pages* folder.</span></span> <span data-ttu-id="1d041-237">Sayfaları başka görünümlerini (düzenleri, şablonlar, kısmi), geçerli sayfa ile aynı klasörde başlangıç hiyerarşik olarak arayın.</span><span class="sxs-lookup"><span data-stu-id="1d041-237">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="1d041-238">Bir düzende *sayfaları* klasörü altında herhangi bir Razor sayfadan kullanılabilir *sayfaları* klasör.</span><span class="sxs-lookup"><span data-stu-id="1d041-238">A layout in the *Pages* folder can be used from any Razor page under the *Pages* folder.</span></span>

<span data-ttu-id="1d041-239">Öneririz **değil** Düzen dosyası içine *görünümler/paylaşılan* klasör.</span><span class="sxs-lookup"><span data-stu-id="1d041-239">We recommend you **not** put the layout file in the *Views/Shared* folder.</span></span> <span data-ttu-id="1d041-240">*Görünümler/paylaşılan* bir MVC görünümleri deseni.</span><span class="sxs-lookup"><span data-stu-id="1d041-240">*Views/Shared* is an MVC views pattern.</span></span> <span data-ttu-id="1d041-241">Razor sayfalarının klasör hiyerarşisi, yol kuralları yararlanmayı yöneliktir.</span><span class="sxs-lookup"><span data-stu-id="1d041-241">Razor Pages are meant to rely on folder hierarchy, not path conventions.</span></span>

<span data-ttu-id="1d041-242">Bir Razor sayfa görünümü aramadan içerir *sayfaları* klasör.</span><span class="sxs-lookup"><span data-stu-id="1d041-242">View search from a Razor Page includes the *Pages* folder.</span></span> <span data-ttu-id="1d041-243">Düzenleri, şablonları ve MVC denetleyicileri ve geleneksel Razor görünümleri ile kullanmakta olduğunuz kısmi *yalnızca iş*.</span><span class="sxs-lookup"><span data-stu-id="1d041-243">The layouts, templates, and partials you're using with MVC controllers and conventional Razor views *just work*.</span></span>

<span data-ttu-id="1d041-244">Ekleme bir *Pages/_ViewImports.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="1d041-244">Add a *Pages/_ViewImports.cshtml* file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]

<span data-ttu-id="1d041-245">`@namespace`Daha sonra öğreticide açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="1d041-245">`@namespace` is explained later in the tutorial.</span></span> <span data-ttu-id="1d041-246">`@addTagHelper` Yönergesi getirir [yerleşik etiket Yardımcıları](xref:mvc/views/tag-helpers/builtin-th/Index) tüm sayfalar için *sayfaları* klasör.</span><span class="sxs-lookup"><span data-stu-id="1d041-246">The `@addTagHelper` directive brings in the [built-in Tag Helpers](xref:mvc/views/tag-helpers/builtin-th/Index) to all the pages in the *Pages* folder.</span></span>

<a name="namespace"></a>

<span data-ttu-id="1d041-247">Zaman `@namespace` yönergesi açıkça bir sayfada kullanılır:</span><span class="sxs-lookup"><span data-stu-id="1d041-247">When the `@namespace` directive is used explicitly on a page:</span></span>

[!code-cshtml[main](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]

<span data-ttu-id="1d041-248">Yönergesi sayfa için ad alanı ayarlar.</span><span class="sxs-lookup"><span data-stu-id="1d041-248">The directive sets the namespace for the page.</span></span> <span data-ttu-id="1d041-249">`@model` Yönergesi olmayan ad alanı içerecek şekilde gerekir.</span><span class="sxs-lookup"><span data-stu-id="1d041-249">The `@model` directive doesn't need to include the namespace.</span></span>

<span data-ttu-id="1d041-250">Zaman `@namespace` yönergesi bulunduğu *_viewımports.cshtml*, belirtilen ad alanı içe aktaran sayfasındaki oluşturulan ad alanı öneki sağlayan `@namespace` yönergesi.</span><span class="sxs-lookup"><span data-stu-id="1d041-250">When the `@namespace` directive is contained in *_ViewImports.cshtml*, the specified namespace supplies the prefix for the generated namespace in the Page that imports the `@namespace` directive.</span></span> <span data-ttu-id="1d041-251">Oluşturulan ad alanı (soneki bölümü) kalanı içeren klasör arasında nokta ayrılmış göreli yol olup *_viewımports.cshtml* ve sayfayı içeren klasör.</span><span class="sxs-lookup"><span data-stu-id="1d041-251">The rest of the generated namespace (the suffix portion) is the dot-separated relative path between the folder containing *_ViewImports.cshtml* and the folder containing the page.</span></span>

<span data-ttu-id="1d041-252">Örneğin, dosyanın arkasındaki kod *Pages/Customers/Edit.cshtml.cs* ad alanını açıkça ayarlar:</span><span class="sxs-lookup"><span data-stu-id="1d041-252">For example, the code behind file *Pages/Customers/Edit.cshtml.cs* explicitly sets the namespace:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=snippet_namespace)]

<span data-ttu-id="1d041-253">*Pages/_ViewImports.cshtml* dosyasını ayarlar aşağıdaki ad alanı:</span><span class="sxs-lookup"><span data-stu-id="1d041-253">The *Pages/_ViewImports.cshtml* file sets the following namespace:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]

<span data-ttu-id="1d041-254">Oluşturulan ad alanı için *Pages/Customers/Edit.cshtml* Razor sayfasını aynıdır dosyanın arkasındaki kod.</span><span class="sxs-lookup"><span data-stu-id="1d041-254">The generated namespace for the *Pages/Customers/Edit.cshtml* Razor Page is the same as the code behind file.</span></span> <span data-ttu-id="1d041-255">`@namespace` Yönergesi tasarlandığı bir proje ve sayfaları üretilen kod için C# sınıfları eklenmiş şekilde *yalnızca iş* eklemek zorunda kalmadan bir `@using` dosyanın arkasındaki kod için yönerge.</span><span class="sxs-lookup"><span data-stu-id="1d041-255">The `@namespace` directive was designed so the C# classes added to a project and pages-generated code *just work* without having to add an `@using` directive for the code behind file.</span></span>

<span data-ttu-id="1d041-256">**Not:** `@namespace` geleneksel Razor görünümleri ile de çalışır.</span><span class="sxs-lookup"><span data-stu-id="1d041-256">**Note:** `@namespace` also works with conventional Razor views.</span></span>

<span data-ttu-id="1d041-257">Özgün *Pages/Create.cshtml* dosyasını görüntüle:</span><span class="sxs-lookup"><span data-stu-id="1d041-257">The original *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Create.cshtml?highlight=2)]

<span data-ttu-id="1d041-258">Güncelleştirilmiş *Pages/Create.cshtml* dosyasını görüntüle:</span><span class="sxs-lookup"><span data-stu-id="1d041-258">The updated *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/Create.cshtml?highlight=2)]

<span data-ttu-id="1d041-259">[Razor sayfalarının başlangıç projesi](#rpvs17) içeren *Pages/_ValidationScriptsPartial.cshtml*, istemci tarafı doğrulama kanca oluşturur.</span><span class="sxs-lookup"><span data-stu-id="1d041-259">The [Razor Pages starter project](#rpvs17) contains the *Pages/_ValidationScriptsPartial.cshtml*, which hooks up client-side validation.</span></span>

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a><span data-ttu-id="1d041-260">Sayfaları için URL oluşturma</span><span class="sxs-lookup"><span data-stu-id="1d041-260">URL generation for Pages</span></span>

<span data-ttu-id="1d041-261">`Create` Sayfasında, daha önce kullandığı gösterilen `RedirectToPage`:</span><span class="sxs-lookup"><span data-stu-id="1d041-261">The `Create` page, shown previously, uses `RedirectToPage`:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=10)]

<span data-ttu-id="1d041-262">Uygulama, aşağıdaki dosya/klasör yapısını sahiptir:</span><span class="sxs-lookup"><span data-stu-id="1d041-262">The app has the following file/folder structure:</span></span>

* <span data-ttu-id="1d041-263">*/ Sayfaları*</span><span class="sxs-lookup"><span data-stu-id="1d041-263">*/Pages*</span></span>

  * <span data-ttu-id="1d041-264">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="1d041-264">*Index.cshtml*</span></span>
  * <span data-ttu-id="1d041-265">*/ Müşteri*</span><span class="sxs-lookup"><span data-stu-id="1d041-265">*/Customer*</span></span>

    * <span data-ttu-id="1d041-266">*Create.cshtml*</span><span class="sxs-lookup"><span data-stu-id="1d041-266">*Create.cshtml*</span></span>
    * <span data-ttu-id="1d041-267">*Edit.cshtml*</span><span class="sxs-lookup"><span data-stu-id="1d041-267">*Edit.cshtml*</span></span>
    * <span data-ttu-id="1d041-268">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="1d041-268">*Index.cshtml*</span></span>

<span data-ttu-id="1d041-269">*Pages/Customers/Create.cshtml* ve *Pages/Customers/Edit.cshtml* sayfaları yeniden yönlendirmek için *Pages/Index.cshtml* başarı sonra.</span><span class="sxs-lookup"><span data-stu-id="1d041-269">The *Pages/Customers/Create.cshtml* and *Pages/Customers/Edit.cshtml* pages redirect to *Pages/Index.cshtml* after success.</span></span> <span data-ttu-id="1d041-270">Dize `/Index` önceki sayfaya erişmek için URI bir parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="1d041-270">The string `/Index` is part of the URI to access the preceding page.</span></span> <span data-ttu-id="1d041-271">Dize `/Index` için URI oluşturmak için kullanılan *Pages/Index.cshtml* sayfası.</span><span class="sxs-lookup"><span data-stu-id="1d041-271">The string `/Index` can be used to generate URIs to the *Pages/Index.cshtml* page.</span></span> <span data-ttu-id="1d041-272">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="1d041-272">For example:</span></span>

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">My Index Page</a>`
* `RedirectToPage("/Index")`

<span data-ttu-id="1d041-273">Sayfa adı sayfasına kökünden yoludur */sayfaları* klasörü (başında dahil olmak üzere `/`, örneğin `/Index`).</span><span class="sxs-lookup"><span data-stu-id="1d041-273">The page name is the path to the page from the root */Pages* folder (including a leading `/`, for example `/Index`).</span></span> <span data-ttu-id="1d041-274">Önceki URL nesil yalnızca cmdlet'e kod bir URL daha çok daha zengin örneklerdir.</span><span class="sxs-lookup"><span data-stu-id="1d041-274">The preceding URL generation samples are much more feature rich than just hardcoding a URL.</span></span> <span data-ttu-id="1d041-275">URL oluşturma kullanan [yönlendirme](xref:mvc/controllers/routing) ve oluşturmak ve yol hedef yolunda nasıl tanımlandığına göre parametreleri kodlayın.</span><span class="sxs-lookup"><span data-stu-id="1d041-275">URL generation uses [routing](xref:mvc/controllers/routing) and can generate and encode parameters according to how the route is defined in the destination path.</span></span>

<span data-ttu-id="1d041-276">Göreli adlar sayfaları için URL oluşturmayı destekler.</span><span class="sxs-lookup"><span data-stu-id="1d041-276">URL generation for pages supports relative names.</span></span> <span data-ttu-id="1d041-277">Aşağıdaki tabloda hangi dizin sayfası ile farklı seçildiğini gösterir `RedirectToPage` parametrelerinden *Pages/Customers/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="1d041-277">The following table shows which Index page is selected with different `RedirectToPage` parameters from *Pages/Customers/Create.cshtml*:</span></span>

| <span data-ttu-id="1d041-278">RedirectToPage(x)</span><span class="sxs-lookup"><span data-stu-id="1d041-278">RedirectToPage(x)</span></span>| <span data-ttu-id="1d041-279">Sayfa</span><span class="sxs-lookup"><span data-stu-id="1d041-279">Page</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="1d041-280">RedirectToPage("/Index")</span><span class="sxs-lookup"><span data-stu-id="1d041-280">RedirectToPage("/Index")</span></span> | <span data-ttu-id="1d041-281">*Sayfa/dizini*</span><span class="sxs-lookup"><span data-stu-id="1d041-281">*Pages/Index*</span></span> |
| <span data-ttu-id="1d041-282">RedirectToPage("./Index");</span><span class="sxs-lookup"><span data-stu-id="1d041-282">RedirectToPage("./Index");</span></span> | <span data-ttu-id="1d041-283">*Müşteriler/sayfalar/dizini*</span><span class="sxs-lookup"><span data-stu-id="1d041-283">*Pages/Customers/Index*</span></span> |
| <span data-ttu-id="1d041-284">RedirectToPage(".. / Dizin")</span><span class="sxs-lookup"><span data-stu-id="1d041-284">RedirectToPage("../Index")</span></span> | <span data-ttu-id="1d041-285">*Sayfa/dizini*</span><span class="sxs-lookup"><span data-stu-id="1d041-285">*Pages/Index*</span></span> |
| <span data-ttu-id="1d041-286">RedirectToPage("Index")</span><span class="sxs-lookup"><span data-stu-id="1d041-286">RedirectToPage("Index")</span></span>  | <span data-ttu-id="1d041-287">*Müşteriler/sayfalar/dizini*</span><span class="sxs-lookup"><span data-stu-id="1d041-287">*Pages/Customers/Index*</span></span> |

<span data-ttu-id="1d041-288">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, ve `RedirectToPage("../Index")` olan *göreli adlar*.</span><span class="sxs-lookup"><span data-stu-id="1d041-288">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, and `RedirectToPage("../Index")`  are *relative names*.</span></span> <span data-ttu-id="1d041-289">`RedirectToPage` Parametresi *birleştirilmiş* ile hedef sayfanın adını işlem için geçerli sayfasının yolu.</span><span class="sxs-lookup"><span data-stu-id="1d041-289">The `RedirectToPage` parameter is *combined* with the path of the current page to compute the name of the destination page.</span></span>  <!-- Review: Original had The provided string is combined with the page name of the current page to compute the name of the destination page. -- page name, not page path -->

<span data-ttu-id="1d041-290">Göreli adı bağlama karmaşık bir yapıyı siteleriyle oluştururken yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="1d041-290">Relative name linking is useful when building sites with a complex structure.</span></span> <span data-ttu-id="1d041-291">Bir klasördeki sayfaları arasında bağlamak için göreli adları kullanıyorsa, bu klasörü yeniden adlandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1d041-291">If you use relative names to link between pages in a folder, you can rename that folder.</span></span> <span data-ttu-id="1d041-292">Tüm bağlantılar hala çalışır, (bunlar klasör adı eklemediniz çünkü).</span><span class="sxs-lookup"><span data-stu-id="1d041-292">All the links still work (because they didn't include the folder name).</span></span>

## <a name="tempdata"></a><span data-ttu-id="1d041-293">TempData</span><span class="sxs-lookup"><span data-stu-id="1d041-293">TempData</span></span>

<span data-ttu-id="1d041-294">ASP.NET Core sunan [TempData](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) özelliği bir [denetleyicisi](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller).</span><span class="sxs-lookup"><span data-stu-id="1d041-294">ASP.NET Core exposes the [TempData](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) property on a [controller](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller).</span></span> <span data-ttu-id="1d041-295">Bu özellik, dosyayı okuma kadar verileri depolar.</span><span class="sxs-lookup"><span data-stu-id="1d041-295">This property stores data until it's read.</span></span> <span data-ttu-id="1d041-296">`Keep` Ve `Peek` yöntemleri, verileri silme olmadan incelemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="1d041-296">The `Keep` and `Peek` methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="1d041-297">`TempData`birden çok tek bir istek için veri gerektiğinde yeniden yönlendirme için yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="1d041-297">`TempData` is  useful for redirection, when data is needed for more than a single request.</span></span>

<span data-ttu-id="1d041-298">`[TempData]` Özniteliği ASP.NET Core 2. 0 ' yenidir ve denetleyicileri ve sayfaları desteklenir.</span><span class="sxs-lookup"><span data-stu-id="1d041-298">The `[TempData]` attribute is new in ASP.NET Core 2.0 and is supported on controllers and pages.</span></span>

<span data-ttu-id="1d041-299">Aşağıdaki kodu değerini ayarlar `Message` kullanarak `TempData`:</span><span class="sxs-lookup"><span data-stu-id="1d041-299">The following code sets the value of `Message` using `TempData`:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,25&name=snippet_Temp)]

<span data-ttu-id="1d041-300">Aşağıdaki biçimlendirmede *Pages/Customers/Index.cshtml* dosyasını değerini görüntüler `Message` kullanarak `TempData`.</span><span class="sxs-lookup"><span data-stu-id="1d041-300">The following markup in the *Pages/Customers/Index.cshtml* file displays the value of `Message` using `TempData`.</span></span>

```cshtml
<h3>Msg: @Model.Message</h3>
```

<span data-ttu-id="1d041-301">*Pages/Customers/Index.cshtml.cs* arka plan kod dosyasının geçerli `[TempData]` özniteliğini `Message` özelliği.</span><span class="sxs-lookup"><span data-stu-id="1d041-301">The *Pages/Customers/Index.cshtml.cs* code-behind file applies the `[TempData]` attribute to the `Message` property.</span></span>

```cs
[TempData]
public string Message { get; set; }
```

<span data-ttu-id="1d041-302">Bkz: [TempData](xref:fundamentals/app-state#temp) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="1d041-302">See [TempData](xref:fundamentals/app-state#temp) for more information.</span></span>

<a name="mhpp"></a>
## <a name="multiple-handlers-per-page"></a><span data-ttu-id="1d041-303">Sayfa başına birden çok işleyicileri</span><span class="sxs-lookup"><span data-stu-id="1d041-303">Multiple handlers per page</span></span>

<span data-ttu-id="1d041-304">İki kullanarak işleyicileri sayfa için aşağıdaki sayfayı biçimlendirmeleri oluşturur `asp-page-handler` etiket Yardımcısı:</span><span class="sxs-lookup"><span data-stu-id="1d041-304">The following page generates markup for two page handlers using the `asp-page-handler` Tag Helper:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

<!-- Review: the FormActionTagHelper applies to all <form /> elements on a Razor page, even when there's no `asp-` attribute   -->

<span data-ttu-id="1d041-305">Önceki örnekte form iki düğmeleri, her kullanarak gönderme sahip `FormActionTagHelper` farklı bir URL'ye göndermek için.</span><span class="sxs-lookup"><span data-stu-id="1d041-305">The form in the preceding example has two submit buttons, each using the `FormActionTagHelper` to submit to a different URL.</span></span> <span data-ttu-id="1d041-306">`asp-page-handler` Özniteliği için bir yardımcı olan `asp-page`.</span><span class="sxs-lookup"><span data-stu-id="1d041-306">The `asp-page-handler` attribute is a companion to `asp-page`.</span></span> <span data-ttu-id="1d041-307">`asp-page-handler`bir sayfa tarafından tanımlanan işleyici yöntemlerin her biri için gönderme URL'leri oluşturur.</span><span class="sxs-lookup"><span data-stu-id="1d041-307">`asp-page-handler` generates URLs that submit to each of the handler methods defined by a page.</span></span> <span data-ttu-id="1d041-308">`asp-page`Örnek geçerli sayfasına bağlantılandırma çünkü belirtilmedi.</span><span class="sxs-lookup"><span data-stu-id="1d041-308">`asp-page` isn't specified because the sample is linking to the current page.</span></span>

<span data-ttu-id="1d041-309">Arka plan kod dosyası:</span><span class="sxs-lookup"><span data-stu-id="1d041-309">The code-behind file:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]

<span data-ttu-id="1d041-310">Önceki kod kullanan *işleyici yöntemleri adlı*.</span><span class="sxs-lookup"><span data-stu-id="1d041-310">The preceding code uses *named handler methods*.</span></span> <span data-ttu-id="1d041-311">Adlandırılmış işleyici yöntemleri adından sonra metin gerçekleştirerek oluşturulur `On<HTTP Verb>` ve önce `Async` (varsa).</span><span class="sxs-lookup"><span data-stu-id="1d041-311">Named handler methods are created by taking the text in the name after `On<HTTP Verb>` and before `Async` (if present).</span></span> <span data-ttu-id="1d041-312">Önceki örnekte OnPost sayfa yöntemlerdir**JoinList**zaman uyumsuz ve OnPost**JoinListUC**zaman uyumsuz.</span><span class="sxs-lookup"><span data-stu-id="1d041-312">In the preceding example, the page methods are OnPost**JoinList**Async and OnPost**JoinListUC**Async.</span></span> <span data-ttu-id="1d041-313">İle *OnPost* ve *zaman uyumsuz* kaldırıldı, işleyici adlardır `JoinList` ve `JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="1d041-313">With *OnPost* and *Async* removed, the handler names are `JoinList` and `JoinListUC`.</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?range=12-13)]

<span data-ttu-id="1d041-314">Yukarıdaki kullanan kod, gönderildiği URL yolunu `OnPostJoinListAsync` olan `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span><span class="sxs-lookup"><span data-stu-id="1d041-314">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span></span> <span data-ttu-id="1d041-315">Gönderildiği URL yolunu `OnPostJoinListUCAsync` olan `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="1d041-315">The URL path that submits to `OnPostJoinListUCAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span></span>

## <a name="customizing-routing"></a><span data-ttu-id="1d041-316">Yönlendirme özelleştirme</span><span class="sxs-lookup"><span data-stu-id="1d041-316">Customizing Routing</span></span>

<span data-ttu-id="1d041-317">Sorgu dizesi hoşlanmıyorsanız `?handler=JoinList` URL'de işleyicisi adı URL'nin yol kısmı koymak için rota değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1d041-317">If you don't like the query string `?handler=JoinList` in the URL, you can change the route to put the handler name in the path portion of the URL.</span></span> <span data-ttu-id="1d041-318">Bir rota şablonu sonra çift tırnak içine ekleyerek rota özelleştirebilirsiniz `@page` yönergesi.</span><span class="sxs-lookup"><span data-stu-id="1d041-318">You can customize the route by adding a route template enclosed in double quotes after the `@page` directive.</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]

<span data-ttu-id="1d041-319">Önceki yol işleyicisi adı ve URL yolunu sorgu dizesi yerine koyar.</span><span class="sxs-lookup"><span data-stu-id="1d041-319">The preceding route puts the handler name in the URL path instead of the query string.</span></span> <span data-ttu-id="1d041-320">`?` Aşağıdaki `handler` rota parametresinin isteğe bağlı olduğu anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="1d041-320">The `?` following `handler` means the route parameter is optional.</span></span>

<span data-ttu-id="1d041-321">Kullanabileceğiniz `@page` ek kesimleri ve parametreleri bir sayfanın rotaya eklemek için.</span><span class="sxs-lookup"><span data-stu-id="1d041-321">You can use `@page` to add additional segments and parameters to a page's route.</span></span> <span data-ttu-id="1d041-322">Ne olursa olsun sahip **eklenmiş** sayfasının varsayılan yol için.</span><span class="sxs-lookup"><span data-stu-id="1d041-322">Whatever's there's **appended** to the default route of the page.</span></span> <span data-ttu-id="1d041-323">Sayfanın yolu değiştirmek için bir mutlak ya da sanal yolu kullanarak (gibi `"~/Some/Other/Path"`) desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="1d041-323">Using an absolute or virtual path to change the page's route (like `"~/Some/Other/Path"`) isn't supported.</span></span>

## <a name="configuration-and-settings"></a><span data-ttu-id="1d041-324">Yapılandırma ve ayarları</span><span class="sxs-lookup"><span data-stu-id="1d041-324">Configuration and settings</span></span>

<span data-ttu-id="1d041-325">Gelişmiş seçenekleri yapılandırmak için genişletme yöntemi kullanmak `AddRazorPagesOptions` MVC oluşturucu üzerinde:</span><span class="sxs-lookup"><span data-stu-id="1d041-325">To configure advanced options, use the extension method `AddRazorPagesOptions` on the MVC builder:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/StartupAdvanced.cs?name=snippet_1)]

<span data-ttu-id="1d041-326">Şu anda kullanabileceğiniz `RazorPagesOptions` sayfaları için kök dizini ayarlayın veya sayfaları için uygulama modeli kuralları eklemek için.</span><span class="sxs-lookup"><span data-stu-id="1d041-326">Currently you can use the `RazorPagesOptions` to set the root directory for pages, or add application model conventions for pages.</span></span> <span data-ttu-id="1d041-327">Gelecekte bu şekilde daha fazla genişletilebilirlik etkinleştirme.</span><span class="sxs-lookup"><span data-stu-id="1d041-327">We'll enable more extensibility this way in the future.</span></span>

<span data-ttu-id="1d041-328">Görünümleri derleneceği bkz [Razor görünüm derleme](xref:mvc/views/view-compilation) .</span><span class="sxs-lookup"><span data-stu-id="1d041-328">To precompile views, see [Razor view compilation](xref:mvc/views/view-compilation) .</span></span>

<span data-ttu-id="1d041-329">[İndirme veya görüntüleme örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/index/sample).</span><span class="sxs-lookup"><span data-stu-id="1d041-329">[Download or view sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/index/sample).</span></span>

<span data-ttu-id="1d041-330">Bkz: [ASP.NET Core Razor sayfalarında ile çalışmaya başlama](xref:tutorials/razor-pages/razor-pages-start), ilgili bu girişi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="1d041-330">See [Getting started with Razor Pages in ASP.NET Core](xref:tutorials/razor-pages/razor-pages-start), which builds on this introduction.</span></span>

### <a name="specify-that-razor-pages-are-at-the-content-root"></a><span data-ttu-id="1d041-331">Razor sayfalarının içerik kök dizininde olduğunu belirtin</span><span class="sxs-lookup"><span data-stu-id="1d041-331">Specify that Razor Pages are at the content root</span></span>

<span data-ttu-id="1d041-332">Razor sayfalarının olsunlar varsayılan olarak, */sayfaları* dizin.</span><span class="sxs-lookup"><span data-stu-id="1d041-332">By default, Razor Pages are rooted in the */Pages* directory.</span></span> <span data-ttu-id="1d041-333">Ekleme [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) için [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) Razor sayfalarınızı içerik kök dizininde olduğunu belirtmek için ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) uygulamasının:</span><span class="sxs-lookup"><span data-stu-id="1d041-333">Add [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at the content root ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) of the app:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesAtContentRoot();
```

### <a name="specify-that-razor-pages-are-at-a-custom-root-directory"></a><span data-ttu-id="1d041-334">Razor sayfalarının özel kök dizininde olduğunu belirtin</span><span class="sxs-lookup"><span data-stu-id="1d041-334">Specify that Razor Pages are at a custom root directory</span></span>

<span data-ttu-id="1d041-335">Ekleme [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) için [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) Razor sayfalarınızı uygulamasında özel kök dizininde olduğunu belirtmek için (göreli bir yol belirtin):</span><span class="sxs-lookup"><span data-stu-id="1d041-335">Add [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at a custom root directory in the app (provide a relative path):</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesRoot("/path/to/razor/pages");
```

## <a name="see-also"></a><span data-ttu-id="1d041-336">Ayrıca bkz.</span><span class="sxs-lookup"><span data-stu-id="1d041-336">See also</span></span>

* [<span data-ttu-id="1d041-337">Razor Sayfaları kullanmaya başlama</span><span class="sxs-lookup"><span data-stu-id="1d041-337">Getting started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="1d041-338">Razor sayfalarının yetkilendirme kuralları</span><span class="sxs-lookup"><span data-stu-id="1d041-338">Razor Pages authorization conventions</span></span>](xref:security/authorization/razor-pages-authorization)
* [<span data-ttu-id="1d041-339">Razor sayfalarının özel yolu ve sayfayı model sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="1d041-339">Razor Pages custom route and page model providers</span></span>](xref:mvc/razor-pages/razor-pages-convention-features)
* [<span data-ttu-id="1d041-340">Razor sayfalarının birim ve tümleştirme sınaması</span><span class="sxs-lookup"><span data-stu-id="1d041-340">Razor Pages unit and integration testing</span></span>](xref:testing/razor-pages-testing)
