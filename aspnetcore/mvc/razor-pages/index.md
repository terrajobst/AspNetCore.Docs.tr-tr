---
title: "ASP.NET Core Razor sayfalarında giriş"
author: Rick-Anderson
description: "Bu belge sayfası odaklı senaryoları geliştirme kolaylığı için ASP.NET Core Razor sayfalarını kullanarak genel bir bakış sağlar."
keywords: "ASP.NET Core, Razor sayfaları"
ms.author: riande
manager: wpickett
ms.date: 09/12/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/razor-pages/index
ms.openlocfilehash: a66b5ea32c2090b9944cd61f90f7fe011a823e82
ms.sourcegitcommit: 3511552becb081fb860a23d6c9b6c4efcab74577
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/07/2017
---
# <a name="introduction-to-razor-pages-in-aspnet-core"></a><span data-ttu-id="cb730-104">ASP.NET Core Razor sayfalarında giriş</span><span class="sxs-lookup"><span data-stu-id="cb730-104">Introduction to Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="cb730-105">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Ryan Nowak](https://github.com/rynowak)</span><span class="sxs-lookup"><span data-stu-id="cb730-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Ryan Nowak](https://github.com/rynowak)</span></span>

<span data-ttu-id="cb730-106">Razor sayfalarının sayfa odaklı senaryoları daha kolay ve daha üretken kodlama yapar ASP.NET Core MVC yeni özelliğidir.</span><span class="sxs-lookup"><span data-stu-id="cb730-106">Razor Pages is a new feature of ASP.NET Core MVC that makes coding page-focused scenarios easier and more productive.</span></span>

<span data-ttu-id="cb730-107">Model-View-Controller yaklaşım kullanan bir öğretici için arıyorsanız bkz [ASP.NET Core MVC'ye Başlarken](xref:tutorials/first-mvc-app/start-mvc).</span><span class="sxs-lookup"><span data-stu-id="cb730-107">If you're looking for a tutorial that uses the Model-View-Controller approach, see [Getting started with ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span></span>

<span data-ttu-id="cb730-108">Bu belge Razor sayfalarının tanıtılmaktadır.</span><span class="sxs-lookup"><span data-stu-id="cb730-108">This document provides an introduction to Razor Pages.</span></span> <span data-ttu-id="cb730-109">Adım adım öğretici değil.</span><span class="sxs-lookup"><span data-stu-id="cb730-109">It's not a step by step tutorial.</span></span> <span data-ttu-id="cb730-110">Bazı bölümleri izleyin zor görürseniz, bkz: [Razor sayfalarının ile çalışmaya başlama](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="cb730-110">If you find some of the sections difficult to follow, see [Getting started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span></span>

<a name="prerequisites"></a>

## <a name="aspnet-core-20-prerequisites"></a><span data-ttu-id="cb730-111">ASP.NET Core 2.0 önkoşulları</span><span class="sxs-lookup"><span data-stu-id="cb730-111">ASP.NET Core 2.0 prerequisites</span></span>

<span data-ttu-id="cb730-112">Yükleme [.NET Core](https://www.microsoft.com/net/core) 2.0.0 veya sonraki bir sürümü.</span><span class="sxs-lookup"><span data-stu-id="cb730-112">Install [.NET Core](https://www.microsoft.com/net/core) 2.0.0 or later.</span></span>

<span data-ttu-id="cb730-113">Visual Studio kullanıyorsanız, yükleme [Visual Studio](https://www.visualstudio.com/vs/) 2017 15.3 veya aşağıdaki iş yükleri ile sonraki bir sürümü:</span><span class="sxs-lookup"><span data-stu-id="cb730-113">If you're using Visual Studio, install [Visual Studio](https://www.visualstudio.com/vs/) 2017 version 15.3 or later with the following workloads:</span></span>

* <span data-ttu-id="cb730-114">**ASP.NET ve web geliştirme**</span><span class="sxs-lookup"><span data-stu-id="cb730-114">**ASP.NET and web development**</span></span>
* <span data-ttu-id="cb730-115">**.NET core platformlar arası geliştirme**</span><span class="sxs-lookup"><span data-stu-id="cb730-115">**.NET Core cross-platform development**</span></span>

<a name="rpvs17"></a>

## <a name="creating-a-razor-pages-project"></a><span data-ttu-id="cb730-116">Razor sayfalarının proje oluşturma</span><span class="sxs-lookup"><span data-stu-id="cb730-116">Creating a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="cb730-117">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cb730-117">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="cb730-118">Bkz: [Razor sayfalarının ile çalışmaya başlama](xref:tutorials/razor-pages/razor-pages-start) Razor sayfalarının oluşturma konusunda ayrıntılı yönergeler için Proje Visual Studio kullanarak.</span><span class="sxs-lookup"><span data-stu-id="cb730-118">See [Getting started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) for detailed instructions on how to create a Razor Pages project using Visual Studio.</span></span>

#   <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="cb730-119">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cb730-119">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="cb730-120">Çalıştırma `dotnet new razor` komut satırından.</span><span class="sxs-lookup"><span data-stu-id="cb730-120">Run `dotnet new razor` from the command line.</span></span>

<span data-ttu-id="cb730-121">Oluşturulan açmak *.csproj* Visual Studio dosyasından Mac için</span><span class="sxs-lookup"><span data-stu-id="cb730-121">Open the generated *.csproj* file from Visual Studio for Mac.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="cb730-122">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="cb730-122">Visual Studio Code</span></span>](#tab/visual-studio-code) 

<span data-ttu-id="cb730-123">Çalıştırma `dotnet new razor` komut satırından.</span><span class="sxs-lookup"><span data-stu-id="cb730-123">Run `dotnet new razor` from the command line.</span></span>

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="cb730-124">.NET core CLI</span><span class="sxs-lookup"><span data-stu-id="cb730-124">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="cb730-125">Çalıştırma `dotnet new razor` komut satırından.</span><span class="sxs-lookup"><span data-stu-id="cb730-125">Run `dotnet new razor` from the command line.</span></span>

---

## <a name="razor-pages"></a><span data-ttu-id="cb730-126">Razor sayfalarının</span><span class="sxs-lookup"><span data-stu-id="cb730-126">Razor Pages</span></span>

<span data-ttu-id="cb730-127">Razor sayfalarının etkinleşir *haline*:</span><span class="sxs-lookup"><span data-stu-id="cb730-127">Razor Pages is enabled in *Startup.cs*:</span></span>

[!code-cs[main](index/sample/RazorPagesIntro/Startup.cs?name=snippet_Startup)]

<span data-ttu-id="cb730-128">Temel bir sayfa göz önünde bulundurun:<a name="OnGet"></a></span><span class="sxs-lookup"><span data-stu-id="cb730-128">Consider a basic page: <a name="OnGet"></a></span></span>

[!code-cshtml[main](index/sample/RazorPagesIntro/Pages/Index.cshtml)]

<span data-ttu-id="cb730-129">Önceki kod çok Razor görünüm dosyası gibi görünüyor.</span><span class="sxs-lookup"><span data-stu-id="cb730-129">The preceding code looks a lot like a Razor view file.</span></span> <span data-ttu-id="cb730-130">Neler farklı kılan unsurdur `@page` yönergesi.</span><span class="sxs-lookup"><span data-stu-id="cb730-130">What makes it different is the `@page` directive.</span></span> <span data-ttu-id="cb730-131">`@page`Dosya, isteklerini doğrudan bir denetleyici geçmeden işleme anlamına gelir ve MVC eylemi - içine yapar.</span><span class="sxs-lookup"><span data-stu-id="cb730-131">`@page` makes the file into an MVC action - which means that it handles requests directly, without going through a controller.</span></span> <span data-ttu-id="cb730-132">`@page`ilk Razor yönergesi bir sayfa üzerinde olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="cb730-132">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="cb730-133">`@page`diğer Razor yapıları davranışını etkiler.</span><span class="sxs-lookup"><span data-stu-id="cb730-133">`@page` affects the behavior of other Razor constructs.</span></span>

<span data-ttu-id="cb730-134">Benzer bir sayfa, kullanarak bir `PageModel` sınıfında, aşağıdaki iki dosyada gösterilir.</span><span class="sxs-lookup"><span data-stu-id="cb730-134">A similar page, using a `PageModel` class, is shown in the following two files.</span></span> <span data-ttu-id="cb730-135">*Pages/Index2.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="cb730-135">The *Pages/Index2.cshtml* file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesIntro/Pages/Index2.cshtml)]

<span data-ttu-id="cb730-136">*Pages/Index2.cshtml.cs* "arka plan kod" dosyası:</span><span class="sxs-lookup"><span data-stu-id="cb730-136">The *Pages/Index2.cshtml.cs* "code-behind" file:</span></span>

[!code-cs[main](index/sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]

<span data-ttu-id="cb730-137">Kural tarafından `PageModel` sınıf dosyası Razor sayfa dosyası ile aynı ada sahip *.cs* eklenir.</span><span class="sxs-lookup"><span data-stu-id="cb730-137">By convention, the `PageModel` class file has the same name as the Razor Page file with *.cs* appended.</span></span> <span data-ttu-id="cb730-138">Örneğin, önceki Razor sayfasıdır *Pages/Index2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="cb730-138">For example, the previous Razor Page is *Pages/Index2.cshtml*.</span></span> <span data-ttu-id="cb730-139">Dosyayı içeren `PageModel` sınıfı adlandırılan *Pages/Index2.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="cb730-139">The file containing the `PageModel` class is named *Pages/Index2.cshtml.cs*.</span></span>

<span data-ttu-id="cb730-140">URL yollarını sayfalara ilişkilendirmelerini dosya sisteminde sayfanın konuma göre belirlenir.</span><span class="sxs-lookup"><span data-stu-id="cb730-140">The associations of URL paths to pages are determined by the page's location in the file system.</span></span> <span data-ttu-id="cb730-141">Aşağıdaki tablo Razor sayfasının yolunu ve eşleşen URL gösterir:</span><span class="sxs-lookup"><span data-stu-id="cb730-141">The following table shows a Razor Page path and the matching URL:</span></span>

| <span data-ttu-id="cb730-142">Dosya adı ve yolu</span><span class="sxs-lookup"><span data-stu-id="cb730-142">File name and path</span></span>               | <span data-ttu-id="cb730-143">URL eşleştirme</span><span class="sxs-lookup"><span data-stu-id="cb730-143">matching URL</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="cb730-144">*/Pages/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="cb730-144">*/Pages/Index.cshtml*</span></span> | <span data-ttu-id="cb730-145">`/`veya`/Index`</span><span class="sxs-lookup"><span data-stu-id="cb730-145">`/` or `/Index`</span></span> |
| <span data-ttu-id="cb730-146">*/Pages/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="cb730-146">*/Pages/Contact.cshtml*</span></span> | `/Contact` |
| <span data-ttu-id="cb730-147">*/Pages/Store/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="cb730-147">*/Pages/Store/Contact.cshtml*</span></span> | `/Store/Contact` |
| <span data-ttu-id="cb730-148">*/Pages/Store/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="cb730-148">*/Pages/Store/Index.cshtml*</span></span> | <span data-ttu-id="cb730-149">`/Store`veya`/Store/Index`</span><span class="sxs-lookup"><span data-stu-id="cb730-149">`/Store` or `/Store/Index`</span></span> |

<span data-ttu-id="cb730-150">Notlar:</span><span class="sxs-lookup"><span data-stu-id="cb730-150">Notes:</span></span>

* <span data-ttu-id="cb730-151">Razor sayfalarının dosyalarında çalışma zamanı arar *sayfaları* varsayılan klasör.</span><span class="sxs-lookup"><span data-stu-id="cb730-151">The runtime looks for Razor Pages files in the *Pages* folder by default.</span></span>
* <span data-ttu-id="cb730-152">`Index`bir URL bir sayfa içermediğinde varsayılan sayfasıdır.</span><span class="sxs-lookup"><span data-stu-id="cb730-152">`Index` is the default page when a URL doesn't include a page.</span></span>

## <a name="writing-a-basic-form"></a><span data-ttu-id="cb730-153">Temel form yazma</span><span class="sxs-lookup"><span data-stu-id="cb730-153">Writing a basic form</span></span>

<span data-ttu-id="cb730-154">Razor sayfalarının özellikleri ortak desenler web tarayıcılarıyla birlikte kullanılan kolay hale getirmek üzere tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="cb730-154">Razor Pages features are designed to make common patterns used with web browsers easy.</span></span> <span data-ttu-id="cb730-155">[Model bağlama](xref:mvc/models/model-binding), [etiket Yardımcıları](xref:mvc/views/tag-helpers/intro)ve HTML Yardımcıları tüm *yalnızca iş* Razor sayfasını sınıfında tanımlanan özelliklere sahip.</span><span class="sxs-lookup"><span data-stu-id="cb730-155">[Model binding](xref:mvc/models/model-binding), [Tag Helpers](xref:mvc/views/tag-helpers/intro), and HTML helpers all *just work* with the properties defined in a Razor Page class.</span></span> <span data-ttu-id="cb730-156">"Bize başvurun" form için temel bir uygulayan bir sayfa göz önünde bulundurun `Contact` modeli:</span><span class="sxs-lookup"><span data-stu-id="cb730-156">Consider a page that implements a basic "contact us" form for the `Contact` model:</span></span>

<span data-ttu-id="cb730-157">Bu belgede, örnekleri için `DbContext` içinde başlatılan [haline](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) dosya.</span><span class="sxs-lookup"><span data-stu-id="cb730-157">For the samples in this document, the `DbContext` is initialized in the [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) file.</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Startup.cs?highlight=15-16)]

<span data-ttu-id="cb730-158">Veri modeli:</span><span class="sxs-lookup"><span data-stu-id="cb730-158">The data model:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Data/Customer.cs)]

<span data-ttu-id="cb730-159">Db bağlamı:</span><span class="sxs-lookup"><span data-stu-id="cb730-159">The db context:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Data/AppDbContext.cs)]

<span data-ttu-id="cb730-160">*Pages/Create.cshtml* dosyasını görüntüle:</span><span class="sxs-lookup"><span data-stu-id="cb730-160">The *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Create.cshtml)]

<span data-ttu-id="cb730-161">*Pages/Create.cshtml.cs* görünümü için arka plan kod dosyası:</span><span class="sxs-lookup"><span data-stu-id="cb730-161">The *Pages/Create.cshtml.cs* code-behind file for the view:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_ALL)]

<span data-ttu-id="cb730-162">Kural tarafından `PageModel` sınıfı çağrıldığında `<PageName>Model` ve sayfa aynı ad.</span><span class="sxs-lookup"><span data-stu-id="cb730-162">By convention, the `PageModel` class is called `<PageName>Model` and is in the same namespace as the page.</span></span>

<span data-ttu-id="cb730-163">`PageModel` Sınıfı, bir sayfa mantığı ayrılması kendi sunudan olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="cb730-163">The `PageModel` class allows separation of the logic of a page from its presentation.</span></span> <span data-ttu-id="cb730-164">Sayfaya gönderilen istekleri ve sayfayı oluşturmak için kullanılan verileri için sayfa işleyiciler tanımlar.</span><span class="sxs-lookup"><span data-stu-id="cb730-164">It defines page handlers for requests sent to the page and the data used to render the page.</span></span> <span data-ttu-id="cb730-165">Bu ayrım aracılığıyla sayfa bağımlılıklar yönetmenize olanak sağlayan [bağımlılık ekleme](xref:fundamentals/dependency-injection) ve [birim testi](xref:testing/razor-pages-testing) sayfaları.</span><span class="sxs-lookup"><span data-stu-id="cb730-165">This separation allows you to manage page dependencies through [dependency injection](xref:fundamentals/dependency-injection) and to [unit test](xref:testing/razor-pages-testing) the pages.</span></span>

<span data-ttu-id="cb730-166">Sayfasına sahip bir `OnPostAsync` *işleyici yöntemi*, üzerinde çalıştığı `POST` ister (kullanıcı formu gönderdiğinde).</span><span class="sxs-lookup"><span data-stu-id="cb730-166">The page has an `OnPostAsync` *handler method*, which runs on `POST` requests (when a user posts the form).</span></span> <span data-ttu-id="cb730-167">Herhangi bir HTTP fiil için işleyici yöntemleri ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="cb730-167">You can add handler methods for any HTTP verb.</span></span> <span data-ttu-id="cb730-168">En yaygın işleyicileri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="cb730-168">The most common handlers are:</span></span>

* <span data-ttu-id="cb730-169">`OnGet`sayfa için gereken durumu başlatılamadı.</span><span class="sxs-lookup"><span data-stu-id="cb730-169">`OnGet` to initialize state needed for the page.</span></span> <span data-ttu-id="cb730-170">[OnGet](#OnGet) örnek.</span><span class="sxs-lookup"><span data-stu-id="cb730-170">[OnGet](#OnGet) sample.</span></span>
* <span data-ttu-id="cb730-171">`OnPost`Form Gönderme işlemek için.</span><span class="sxs-lookup"><span data-stu-id="cb730-171">`OnPost` to handle form submissions.</span></span>

<span data-ttu-id="cb730-172">`Async` Adlandırma soneki isteğe bağlıdır ancak genellikle kurala göre zaman uyumsuz işlevleri için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="cb730-172">The `Async` naming suffix is optional but is often used by convention for asynchronous functions.</span></span> <span data-ttu-id="cb730-173">`OnPostAsync` Önceki örnekte kod ne, normalde bir denetleyicisi yazarsınız için benzer görünür.</span><span class="sxs-lookup"><span data-stu-id="cb730-173">The `OnPostAsync` code in the preceding example looks similar to what you would normally write in a controller.</span></span> <span data-ttu-id="cb730-174">Önceki kod Razor sayfalar için normaldir.</span><span class="sxs-lookup"><span data-stu-id="cb730-174">The preceding code is typical for Razor Pages.</span></span> <span data-ttu-id="cb730-175">MVC elemanlar çoğu ister [model bağlama](xref:mvc/models/model-binding), [doğrulama](xref:mvc/models/validation), ve eylem sonuçlarını paylaşıldığı.</span><span class="sxs-lookup"><span data-stu-id="cb730-175">Most of the MVC primitives like [model binding](xref:mvc/models/model-binding), [validation](xref:mvc/models/validation), and action results are shared.</span></span>  <!-- Review: Ryan, can we get a list of what is shared and what isn't? -->

<span data-ttu-id="cb730-176">Önceki `OnPostAsync` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="cb730-176">The previous `OnPostAsync` method:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="cb730-177">Temel akışı `OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="cb730-177">The basic flow of `OnPostAsync`:</span></span>

<span data-ttu-id="cb730-178">Doğrulama hataları denetleyin.</span><span class="sxs-lookup"><span data-stu-id="cb730-178">Check for validation errors.</span></span>

*  <span data-ttu-id="cb730-179">Herhangi bir hata varsa, verileri Kaydet ve yeniden yönlendirme.</span><span class="sxs-lookup"><span data-stu-id="cb730-179">If there are no errors, save the data and redirect.</span></span>
*  <span data-ttu-id="cb730-180">Hatalar varsa, doğrulama iletileri sayfasıyla yeniden gösterir.</span><span class="sxs-lookup"><span data-stu-id="cb730-180">If there are errors, show the page again with validation messages.</span></span> <span data-ttu-id="cb730-181">İstemci tarafı doğrulama geleneksel ASP.NET Core MVC uygulamaları için aynıdır.</span><span class="sxs-lookup"><span data-stu-id="cb730-181">Client-side validation is identical to traditional ASP.NET Core MVC applications.</span></span> <span data-ttu-id="cb730-182">Çoğu durumda, doğrulama hataları istemcide algılanabilir ve hiçbir zaman sunucuya gönderildi.</span><span class="sxs-lookup"><span data-stu-id="cb730-182">In many cases, validation errors would be detected on the client, and never submitted to the server.</span></span>

<span data-ttu-id="cb730-183">Veri başarıyla girildiğinde `OnPostAsync` işleyici yöntem çağrılarını `RedirectToPage` bir örneğini döndürmek için yardımcı yöntem `RedirectToPageResult`.</span><span class="sxs-lookup"><span data-stu-id="cb730-183">When the data is entered successfully, the `OnPostAsync` handler method calls the `RedirectToPage` helper method to return an instance of `RedirectToPageResult`.</span></span> <span data-ttu-id="cb730-184">`RedirectToPage`Yeni eylem sonucu, benzer olduğunu `RedirectToAction` veya `RedirectToRoute`, ancak özelleştirilmiş sayfalar için.</span><span class="sxs-lookup"><span data-stu-id="cb730-184">`RedirectToPage` is a new action result, similar to `RedirectToAction` or `RedirectToRoute`, but customized for pages.</span></span> <span data-ttu-id="cb730-185">İsteğe bağlı olarak önceki örnekte kök dizin sayfasına yönlendiren (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="cb730-185">In the preceding sample, it redirects to the root Index page (`/Index`).</span></span> <span data-ttu-id="cb730-186">`RedirectToPage`içinde ayrıntılı [sayfaları için URL oluşturma](#url_gen) bölümü.</span><span class="sxs-lookup"><span data-stu-id="cb730-186">`RedirectToPage` is detailed in the [URL generation for Pages](#url_gen) section.</span></span>

<span data-ttu-id="cb730-187">Teslim edilen formu (yani sunucuya geçirilir) doğrulama hataları olduğunda`OnPostAsync` işleyici yöntem çağrılarını `Page` yardımcı yöntemi.</span><span class="sxs-lookup"><span data-stu-id="cb730-187">When the submitted form has validation errors (that are passed to the server), the`OnPostAsync` handler method calls the `Page` helper method.</span></span> <span data-ttu-id="cb730-188">`Page`örneğini döndürür `PageResult`.</span><span class="sxs-lookup"><span data-stu-id="cb730-188">`Page` returns an instance of `PageResult`.</span></span> <span data-ttu-id="cb730-189">Döndürme `Page` nasıl denetleyicileri Eylemler döndürmek için benzer `View`.</span><span class="sxs-lookup"><span data-stu-id="cb730-189">Returning `Page` is similar to how actions in controllers return `View`.</span></span> <span data-ttu-id="cb730-190">`PageResult`Varsayılan değer <!-- Review  --> dönüş türü için bir işleyici yöntemi.</span><span class="sxs-lookup"><span data-stu-id="cb730-190">`PageResult` is the default <!-- Review  --> return type for a handler method.</span></span> <span data-ttu-id="cb730-191">Döndüren bir işleyici yöntemi `void` sayfasını işler.</span><span class="sxs-lookup"><span data-stu-id="cb730-191">A handler method that returns `void` renders the page.</span></span>

<span data-ttu-id="cb730-192">`Customer` Özelliğini kullanan `[BindProperty]` model bağlama için kabul özniteliği.</span><span class="sxs-lookup"><span data-stu-id="cb730-192">The `Customer` property uses `[BindProperty]` attribute to opt in to model binding.</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_PageModel&highlight=10-11)]

<span data-ttu-id="cb730-193">Razor sayfalarının varsayılan olarak, GET olmayan fiiller yalnızca özelliklerle bağlayın.</span><span class="sxs-lookup"><span data-stu-id="cb730-193">Razor Pages, by default, bind properties only with non-GET verbs.</span></span> <span data-ttu-id="cb730-194">Bağlama özellikleri için kod yazmak zorunda miktarını azaltır.</span><span class="sxs-lookup"><span data-stu-id="cb730-194">Binding to properties can reduce the amount of code you have to write.</span></span> <span data-ttu-id="cb730-195">Bağlama form alanları oluşturmak için aynı özelliği kullanarak kod azaltır (`<input asp-for="Customer.Name" />`) ve giriş kabul edin.</span><span class="sxs-lookup"><span data-stu-id="cb730-195">Binding reduces code by using the same property to render form fields (`<input asp-for="Customer.Name" />`) and accept the input.</span></span>

<span data-ttu-id="cb730-196">Giriş sayfası (*Index.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="cb730-196">The home page (*Index.cshtml*):</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Index.cshtml)]

<span data-ttu-id="cb730-197">Arka plan kod *Index.cshtml.cs* dosyası:</span><span class="sxs-lookup"><span data-stu-id="cb730-197">The code behind *Index.cshtml.cs* file:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs)]

<span data-ttu-id="cb730-198">*Index.cshtml* dosyası her kişi için bir düzenleme bağlantısı oluşturmak için aşağıdaki biçimlendirme içerir:</span><span class="sxs-lookup"><span data-stu-id="cb730-198">The *Index.cshtml* file contains the following markup to create an edit link for each contact:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=21)]

<span data-ttu-id="cb730-199">[Yer işareti etiketi yardımcı](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) kullanılan [asp - rota-{value}](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper#route) düzenleme sayfasını bir bağlantı oluşturmak için öznitelik.</span><span class="sxs-lookup"><span data-stu-id="cb730-199">The [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) used the [asp-route-{value}](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper#route) attribute to generate a link to the Edit page.</span></span> <span data-ttu-id="cb730-200">Rota verilerini kişiyle bağlantıyı içeren kimliği</span><span class="sxs-lookup"><span data-stu-id="cb730-200">The link contains route data with the contact ID.</span></span> <span data-ttu-id="cb730-201">Örneğin, `http://localhost:5000/Edit/1`.</span><span class="sxs-lookup"><span data-stu-id="cb730-201">For example, `http://localhost:5000/Edit/1`.</span></span>

<span data-ttu-id="cb730-202">*Pages/Edit.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="cb730-202">The *Pages/Edit.cshtml* file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Edit.cshtml?highlight=1)]

<span data-ttu-id="cb730-203">İlk satır içerir `@page "{id:int}"` yönergesi.</span><span class="sxs-lookup"><span data-stu-id="cb730-203">The first line contains the `@page "{id:int}"` directive.</span></span> <span data-ttu-id="cb730-204">Yönlendirme kısıtlaması`"{id:int}"` içeren sayfa isteklerini kabul etmek için sayfayı söyler `int` rota verileri.</span><span class="sxs-lookup"><span data-stu-id="cb730-204">The routing constraint`"{id:int}"` tells the page to accept requests to the page that contain `int` route data.</span></span> <span data-ttu-id="cb730-205">Sayfa için bir istek dönüştürülebilir rota verileri içermiyorsa, bir `int`, çalışma zamanı (bulunamadı) HTTP 404 hatası döndürür.</span><span class="sxs-lookup"><span data-stu-id="cb730-205">If a request to the page doesn't contain route data that can be converted to an `int`, the runtime returns an HTTP 404 (not found) error.</span></span>

<span data-ttu-id="cb730-206">*Pages/Edit.cshtml.cs* dosyası:</span><span class="sxs-lookup"><span data-stu-id="cb730-206">The *Pages/Edit.cshtml.cs* file:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Edit.cshtml.cs)]

<span data-ttu-id="cb730-207">*Index.cshtml* dosya de Sil düğmesini her müşteri irtibat için oluşturmak için biçimlendirme içerir:</span><span class="sxs-lookup"><span data-stu-id="cb730-207">The *Index.cshtml* file also contains markup to create a delete button for each customer contact:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=22-23)]

<span data-ttu-id="cb730-208">Sil düğmesini HTML olarak işlendiğinde, `formaction` parametreleri içerir:</span><span class="sxs-lookup"><span data-stu-id="cb730-208">When the delete button is rendered in HTML, its `formaction` includes parameters for:</span></span>

* <span data-ttu-id="cb730-209">Müşteri tarafından belirtilen kimliği başvurun `asp-route-id` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="cb730-209">The customer contact ID specified by the `asp-route-id` attribute.</span></span>
* <span data-ttu-id="cb730-210">`handler` Tarafından belirtilen `asp-page-handler` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="cb730-210">The `handler` specified by the `asp-page-handler` attribute.</span></span>

<span data-ttu-id="cb730-211">Bir müşteri ile işlenen Sil düğmesini bir örneği burada verilmiştir Kimliğini başvurun `1`:</span><span class="sxs-lookup"><span data-stu-id="cb730-211">Here is an example of a rendered delete button with a customer contact ID of `1`:</span></span>

```html
<button type="submit" formaction="/?id=1&amp;handler=delete">delete</button>
```

<span data-ttu-id="cb730-212">Düğme seçildiğinde, bir form `POST` isteği sunucuya gönderilir.</span><span class="sxs-lookup"><span data-stu-id="cb730-212">When the button is selected, a form `POST` request is sent to the server.</span></span> <span data-ttu-id="cb730-213">Kurala göre işleyicisi yönteminin adı seçili değerine göre `handler` parametre düzeni göre `OnPost[handler]Async`.</span><span class="sxs-lookup"><span data-stu-id="cb730-213">By convention, the name of the handler method is selected based the value of the `handler` parameter according to the scheme `OnPost[handler]Async`.</span></span>

<span data-ttu-id="cb730-214">Çünkü `handler` olan `delete` Bu örnekte, `OnPostDeleteAsync` işleyici yöntemi kullanılır işleme `POST` isteği.</span><span class="sxs-lookup"><span data-stu-id="cb730-214">Because the `handler` is `delete` in this example, the `OnPostDeleteAsync` handler method is used to process the `POST` request.</span></span> <span data-ttu-id="cb730-215">Varsa `asp-page-handler` gibi farklı bir değere ayarlanmış `remove`, ada sahip bir sayfa işleyici yöntemi `OnPostRemoveAsync` seçilir.</span><span class="sxs-lookup"><span data-stu-id="cb730-215">If the `asp-page-handler` is set to a different value, such as `remove`, a page handler method with the name `OnPostRemoveAsync` is selected.</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs?range=26-37)]

<span data-ttu-id="cb730-216">`OnPostDeleteAsync` Yöntemi:</span><span class="sxs-lookup"><span data-stu-id="cb730-216">The `OnPostDeleteAsync` method:</span></span>

* <span data-ttu-id="cb730-217">Kabul `id` Sorgu dizesinden.</span><span class="sxs-lookup"><span data-stu-id="cb730-217">Accepts the `id` from the query string.</span></span>
* <span data-ttu-id="cb730-218">Müşteri irtibat ile veritabanını sorgular `FindAsync`.</span><span class="sxs-lookup"><span data-stu-id="cb730-218">Queries the database for the customer contact with `FindAsync`.</span></span>
* <span data-ttu-id="cb730-219">Müşteri irtibat bulunursa, müşteri kişiler listesinden kaldırıldı.</span><span class="sxs-lookup"><span data-stu-id="cb730-219">If the customer contact is found, they're removed from the list of customer contacts.</span></span> <span data-ttu-id="cb730-220">Veritabanı güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="cb730-220">The database is updated.</span></span>
* <span data-ttu-id="cb730-221">Çağrıları `RedirectToPage` kök dizin sayfasına yeniden yönlendirmek için (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="cb730-221">Calls `RedirectToPage` to redirect to the root Index page (`/Index`).</span></span>

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a><span data-ttu-id="cb730-222">XSRF/CSRF ve Razor sayfaları</span><span class="sxs-lookup"><span data-stu-id="cb730-222">XSRF/CSRF and Razor Pages</span></span>

<span data-ttu-id="cb730-223">Herhangi bir kod yazmak zorunda değilsiniz [antiforgery doğrulama](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="cb730-223">You don't have to write any code for [antiforgery validation](xref:security/anti-request-forgery).</span></span> <span data-ttu-id="cb730-224">Otomatik olarak antiforgery belirteci oluşturma ve doğrulama Razor sayfalarında dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="cb730-224">Antiforgery token generation and validation are automatically included in Razor Pages.</span></span>

<a name="layout"></a>
## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a><span data-ttu-id="cb730-225">Düzenleri, kısmi, şablonları ve etiket Yardımcıları Razor Pages ile kullanma</span><span class="sxs-lookup"><span data-stu-id="cb730-225">Using Layouts, partials, templates, and Tag Helpers with Razor Pages</span></span>

<span data-ttu-id="cb730-226">Sayfaları tüm özellikleri Razor görüntüleme altyapısı ile çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="cb730-226">Pages work with all the features of the Razor view engine.</span></span> <span data-ttu-id="cb730-227">Düzenleri, kısmi, şablonları, etiket Yardımcıları *_ViewStart.cshtml*, *_viewımports.cshtml* iş için geleneksel Razor görünümleri yaparlar aynı şekilde.</span><span class="sxs-lookup"><span data-stu-id="cb730-227">Layouts, partials, templates, Tag Helpers, *_ViewStart.cshtml*, *_ViewImports.cshtml* work in the same way they do for conventional Razor views.</span></span>

<span data-ttu-id="cb730-228">Şimdi bu sayfa bu özelliklerden bazıları yararlanarak declutter.</span><span class="sxs-lookup"><span data-stu-id="cb730-228">Let's declutter this page by taking advantage of some of those features.</span></span>

<span data-ttu-id="cb730-229">Ekleme bir [düzen sayfası](xref:mvc/views/layout) için *Pages/_Layout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="cb730-229">Add a [layout page](xref:mvc/views/layout) to *Pages/_Layout.cshtml*:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_LayoutSimple.cshtml)]

<span data-ttu-id="cb730-230">[Düzeni](xref:mvc/views/layout):</span><span class="sxs-lookup"><span data-stu-id="cb730-230">The [Layout](xref:mvc/views/layout):</span></span>

* <span data-ttu-id="cb730-231">(Sayfa düzeni dışında çevrilir sürece) her sayfanın düzenini denetler.</span><span class="sxs-lookup"><span data-stu-id="cb730-231">Controls the layout of each page (unless the page opts out of layout).</span></span>
* <span data-ttu-id="cb730-232">JavaScript ve stil sayfalarını gibi HTML yapıları alır.</span><span class="sxs-lookup"><span data-stu-id="cb730-232">Imports HTML structures such as JavaScript and stylesheets.</span></span>

<span data-ttu-id="cb730-233">Bkz: [düzen sayfası](xref:mvc/views/layout) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="cb730-233">See [layout page](xref:mvc/views/layout) for more information.</span></span>

<span data-ttu-id="cb730-234">[Düzeni](xref:mvc/views/layout#specifying-a-layout) özelliği ayarlanmış *Pages/_ViewStart.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="cb730-234">The [Layout](xref:mvc/views/layout#specifying-a-layout) property is set in *Pages/_ViewStart.cshtml*:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]

<span data-ttu-id="cb730-235">**Not:** düzeni bulunduğu *sayfaları* klasör.</span><span class="sxs-lookup"><span data-stu-id="cb730-235">**Note:** The layout is in the *Pages* folder.</span></span> <span data-ttu-id="cb730-236">Sayfaları başka görünümlerini (düzenleri, şablonlar, kısmi), geçerli sayfa ile aynı klasörde başlangıç hiyerarşik olarak arayın.</span><span class="sxs-lookup"><span data-stu-id="cb730-236">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="cb730-237">Bir düzende *sayfaları* klasörü altında herhangi bir Razor sayfadan kullanılabilir *sayfaları* klasör.</span><span class="sxs-lookup"><span data-stu-id="cb730-237">A layout in the *Pages* folder can be used from any Razor page under the *Pages* folder.</span></span>

<span data-ttu-id="cb730-238">Öneririz **değil** Düzen dosyası içine *görünümler/paylaşılan* klasör.</span><span class="sxs-lookup"><span data-stu-id="cb730-238">We recommend you **not** put the layout file in the *Views/Shared* folder.</span></span> <span data-ttu-id="cb730-239">*Görünümler/paylaşılan* bir MVC görünümleri deseni.</span><span class="sxs-lookup"><span data-stu-id="cb730-239">*Views/Shared* is an MVC views pattern.</span></span> <span data-ttu-id="cb730-240">Razor sayfalarının klasör hiyerarşisi, yol kuralları yararlanmayı yöneliktir.</span><span class="sxs-lookup"><span data-stu-id="cb730-240">Razor Pages are meant to rely on folder hierarchy, not path conventions.</span></span>

<span data-ttu-id="cb730-241">Bir Razor sayfa görünümü aramadan içerir *sayfaları* klasör.</span><span class="sxs-lookup"><span data-stu-id="cb730-241">View search from a Razor Page includes the *Pages* folder.</span></span> <span data-ttu-id="cb730-242">Düzenleri, şablonları ve MVC denetleyicileri ve geleneksel Razor görünümleri ile kullanmakta olduğunuz kısmi *yalnızca iş*.</span><span class="sxs-lookup"><span data-stu-id="cb730-242">The layouts, templates, and partials you're using with MVC controllers and conventional Razor views *just work*.</span></span>

<span data-ttu-id="cb730-243">Ekleme bir *Pages/_ViewImports.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="cb730-243">Add a *Pages/_ViewImports.cshtml* file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]

<span data-ttu-id="cb730-244">`@namespace`Daha sonra öğreticide açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="cb730-244">`@namespace` is explained later in the tutorial.</span></span> <span data-ttu-id="cb730-245">`@addTagHelper` Yönergesi getirir [yerleşik etiket Yardımcıları](xref:mvc/views/tag-helpers/builtin-th/Index) tüm sayfalar için *sayfaları* klasör.</span><span class="sxs-lookup"><span data-stu-id="cb730-245">The `@addTagHelper` directive brings in the [built-in Tag Helpers](xref:mvc/views/tag-helpers/builtin-th/Index) to all the pages in the *Pages* folder.</span></span>

<a name="namespace"></a>

<span data-ttu-id="cb730-246">Zaman `@namespace` yönergesi açıkça bir sayfada kullanılır:</span><span class="sxs-lookup"><span data-stu-id="cb730-246">When the `@namespace` directive is used explicitly on a page:</span></span>

[!code-cshtml[main](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]

<span data-ttu-id="cb730-247">Yönergesi sayfa için ad alanı ayarlar.</span><span class="sxs-lookup"><span data-stu-id="cb730-247">The directive sets the namespace for the page.</span></span> <span data-ttu-id="cb730-248">`@model` Yönergesi olmayan ad alanı içerecek şekilde gerekir.</span><span class="sxs-lookup"><span data-stu-id="cb730-248">The `@model` directive doesn't need to include the namespace.</span></span>

<span data-ttu-id="cb730-249">Zaman `@namespace` yönergesi bulunduğu *_viewımports.cshtml*, belirtilen ad alanı içe aktaran sayfasındaki oluşturulan ad alanı öneki sağlayan `@namespace` yönergesi.</span><span class="sxs-lookup"><span data-stu-id="cb730-249">When the `@namespace` directive is contained in *_ViewImports.cshtml*, the specified namespace supplies the prefix for the generated namespace in the Page that imports the `@namespace` directive.</span></span> <span data-ttu-id="cb730-250">Oluşturulan ad alanı (soneki bölümü) kalanı içeren klasör arasında nokta ayrılmış göreli yol olup *_viewımports.cshtml* ve sayfayı içeren klasör.</span><span class="sxs-lookup"><span data-stu-id="cb730-250">The rest of the generated namespace (the suffix portion) is the dot-separated relative path between the folder containing *_ViewImports.cshtml* and the folder containing the page.</span></span>

<span data-ttu-id="cb730-251">Örneğin, dosyanın arkasındaki kod *Pages/Customers/Edit.cshtml.cs* ad alanını açıkça ayarlar:</span><span class="sxs-lookup"><span data-stu-id="cb730-251">For example, the code behind file *Pages/Customers/Edit.cshtml.cs* explicitly sets the namespace:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=snippet_namespace)]

<span data-ttu-id="cb730-252">*Pages/_ViewImports.cshtml* dosyasını ayarlar aşağıdaki ad alanı:</span><span class="sxs-lookup"><span data-stu-id="cb730-252">The *Pages/_ViewImports.cshtml* file sets the following namespace:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]

<span data-ttu-id="cb730-253">Oluşturulan ad alanı için *Pages/Customers/Edit.cshtml* Razor sayfasını aynıdır dosyanın arkasındaki kod.</span><span class="sxs-lookup"><span data-stu-id="cb730-253">The generated namespace for the *Pages/Customers/Edit.cshtml* Razor Page is the same as the code behind file.</span></span> <span data-ttu-id="cb730-254">`@namespace` Yönergesi tasarlandığı bir proje ve sayfaları üretilen kod için C# sınıfları eklenmiş şekilde *yalnızca iş* eklemek zorunda kalmadan bir `@using` dosyanın arkasındaki kod için yönerge.</span><span class="sxs-lookup"><span data-stu-id="cb730-254">The `@namespace` directive was designed so the C# classes added to a project and pages-generated code *just work* without having to add an `@using` directive for the code behind file.</span></span>

<span data-ttu-id="cb730-255">**Not:** `@namespace` geleneksel Razor görünümleri ile de çalışır.</span><span class="sxs-lookup"><span data-stu-id="cb730-255">**Note:** `@namespace` also works with conventional Razor views.</span></span>

<span data-ttu-id="cb730-256">Özgün *Pages/Create.cshtml* dosyasını görüntüle:</span><span class="sxs-lookup"><span data-stu-id="cb730-256">The original *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Create.cshtml?highlight=2)]

<span data-ttu-id="cb730-257">Güncelleştirilmiş *Pages/Create.cshtml* dosyasını görüntüle:</span><span class="sxs-lookup"><span data-stu-id="cb730-257">The updated *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/Create.cshtml?highlight=2)]

<span data-ttu-id="cb730-258">[Razor sayfalarının başlangıç projesi](#rpvs17) içeren *Pages/_ValidationScriptsPartial.cshtml*, istemci tarafı doğrulama kanca oluşturur.</span><span class="sxs-lookup"><span data-stu-id="cb730-258">The [Razor Pages starter project](#rpvs17) contains the *Pages/_ValidationScriptsPartial.cshtml*, which hooks up client-side validation.</span></span>

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a><span data-ttu-id="cb730-259">Sayfaları için URL oluşturma</span><span class="sxs-lookup"><span data-stu-id="cb730-259">URL generation for Pages</span></span>

<span data-ttu-id="cb730-260">`Create` Sayfasında, daha önce kullandığı gösterilen `RedirectToPage`:</span><span class="sxs-lookup"><span data-stu-id="cb730-260">The `Create` page, shown previously, uses `RedirectToPage`:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=10)]

<span data-ttu-id="cb730-261">Uygulama, aşağıdaki dosya/klasör yapısını sahiptir:</span><span class="sxs-lookup"><span data-stu-id="cb730-261">The app has the following file/folder structure:</span></span>

* <span data-ttu-id="cb730-262">*/ Sayfaları*</span><span class="sxs-lookup"><span data-stu-id="cb730-262">*/Pages*</span></span>

  * <span data-ttu-id="cb730-263">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="cb730-263">*Index.cshtml*</span></span>
  * <span data-ttu-id="cb730-264">*/ Müşteri*</span><span class="sxs-lookup"><span data-stu-id="cb730-264">*/Customer*</span></span>

    * <span data-ttu-id="cb730-265">*Create.cshtml*</span><span class="sxs-lookup"><span data-stu-id="cb730-265">*Create.cshtml*</span></span>
    * <span data-ttu-id="cb730-266">*Edit.cshtml*</span><span class="sxs-lookup"><span data-stu-id="cb730-266">*Edit.cshtml*</span></span>
    * <span data-ttu-id="cb730-267">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="cb730-267">*Index.cshtml*</span></span>

<span data-ttu-id="cb730-268">*Pages/Customers/Create.cshtml* ve *Pages/Customers/Edit.cshtml* sayfaları yeniden yönlendirmek için *Pages/Index.cshtml* başarı sonra.</span><span class="sxs-lookup"><span data-stu-id="cb730-268">The *Pages/Customers/Create.cshtml* and *Pages/Customers/Edit.cshtml* pages redirect to *Pages/Index.cshtml* after success.</span></span> <span data-ttu-id="cb730-269">Dize `/Index` önceki sayfaya erişmek için URI bir parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="cb730-269">The string `/Index` is part of the URI to access the preceding page.</span></span> <span data-ttu-id="cb730-270">Dize `/Index` için URI oluşturmak için kullanılan *Pages/Index.cshtml* sayfası.</span><span class="sxs-lookup"><span data-stu-id="cb730-270">The string `/Index` can be used to generate URIs to the *Pages/Index.cshtml* page.</span></span> <span data-ttu-id="cb730-271">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="cb730-271">For example:</span></span>

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">My Index Page</a>`
* `RedirectToPage("/Index")`

<span data-ttu-id="cb730-272">Sayfa adı sayfasına kökünden yoludur */sayfaları* klasörü (başında dahil olmak üzere `/`, örneğin `/Index`).</span><span class="sxs-lookup"><span data-stu-id="cb730-272">The page name is the path to the page from the root */Pages* folder (including a leading `/`, for example `/Index`).</span></span> <span data-ttu-id="cb730-273">Önceki URL nesil yalnızca cmdlet'e kod bir URL daha çok daha zengin örneklerdir.</span><span class="sxs-lookup"><span data-stu-id="cb730-273">The preceding URL generation samples are much more feature rich than just hardcoding a URL.</span></span> <span data-ttu-id="cb730-274">URL oluşturma kullanan [yönlendirme](xref:mvc/controllers/routing) ve oluşturmak ve yol hedef yolunda nasıl tanımlandığına göre parametreleri kodlayın.</span><span class="sxs-lookup"><span data-stu-id="cb730-274">URL generation uses [routing](xref:mvc/controllers/routing) and can generate and encode parameters according to how the route is defined in the destination path.</span></span>

<span data-ttu-id="cb730-275">Göreli adlar sayfaları için URL oluşturmayı destekler.</span><span class="sxs-lookup"><span data-stu-id="cb730-275">URL generation for pages supports relative names.</span></span> <span data-ttu-id="cb730-276">Aşağıdaki tabloda hangi dizin sayfası ile farklı seçildiğini gösterir `RedirectToPage` parametrelerinden *Pages/Customers/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="cb730-276">The following table shows which Index page is selected with different `RedirectToPage` parameters from *Pages/Customers/Create.cshtml*:</span></span>

| <span data-ttu-id="cb730-277">RedirectToPage(x)</span><span class="sxs-lookup"><span data-stu-id="cb730-277">RedirectToPage(x)</span></span>| <span data-ttu-id="cb730-278">Sayfa</span><span class="sxs-lookup"><span data-stu-id="cb730-278">Page</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="cb730-279">RedirectToPage("/Index")</span><span class="sxs-lookup"><span data-stu-id="cb730-279">RedirectToPage("/Index")</span></span> | <span data-ttu-id="cb730-280">*Sayfa/dizini*</span><span class="sxs-lookup"><span data-stu-id="cb730-280">*Pages/Index*</span></span> |
| <span data-ttu-id="cb730-281">RedirectToPage("./Index");</span><span class="sxs-lookup"><span data-stu-id="cb730-281">RedirectToPage("./Index");</span></span> | <span data-ttu-id="cb730-282">*Müşteriler/sayfalar/dizini*</span><span class="sxs-lookup"><span data-stu-id="cb730-282">*Pages/Customers/Index*</span></span> |
| <span data-ttu-id="cb730-283">RedirectToPage(".. / Dizin")</span><span class="sxs-lookup"><span data-stu-id="cb730-283">RedirectToPage("../Index")</span></span> | <span data-ttu-id="cb730-284">*Sayfa/dizini*</span><span class="sxs-lookup"><span data-stu-id="cb730-284">*Pages/Index*</span></span> |
| <span data-ttu-id="cb730-285">RedirectToPage("Index")</span><span class="sxs-lookup"><span data-stu-id="cb730-285">RedirectToPage("Index")</span></span>  | <span data-ttu-id="cb730-286">*Müşteriler/sayfalar/dizini*</span><span class="sxs-lookup"><span data-stu-id="cb730-286">*Pages/Customers/Index*</span></span> |

<span data-ttu-id="cb730-287">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, ve `RedirectToPage("../Index")` olan *göreli adlar*.</span><span class="sxs-lookup"><span data-stu-id="cb730-287">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, and `RedirectToPage("../Index")`  are *relative names*.</span></span> <span data-ttu-id="cb730-288">`RedirectToPage` Parametresi *birleştirilmiş* ile hedef sayfanın adını işlem için geçerli sayfasının yolu.</span><span class="sxs-lookup"><span data-stu-id="cb730-288">The `RedirectToPage` parameter is *combined* with the path of the current page to compute the name of the destination page.</span></span>  <!-- Review: Original had The provided string is combined with the page name of the current page to compute the name of the destination page. -- page name, not page path -->

<span data-ttu-id="cb730-289">Göreli adı bağlama karmaşık bir yapıyı siteleriyle oluştururken yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="cb730-289">Relative name linking is useful when building sites with a complex structure.</span></span> <span data-ttu-id="cb730-290">Bir klasördeki sayfaları arasında bağlamak için göreli adları kullanıyorsa, bu klasörü yeniden adlandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="cb730-290">If you use relative names to link between pages in a folder, you can rename that folder.</span></span> <span data-ttu-id="cb730-291">Tüm bağlantılar hala çalışır, (bunlar klasör adı eklemediniz çünkü).</span><span class="sxs-lookup"><span data-stu-id="cb730-291">All the links still work (because they didn't include the folder name).</span></span>

## <a name="tempdata"></a><span data-ttu-id="cb730-292">TempData</span><span class="sxs-lookup"><span data-stu-id="cb730-292">TempData</span></span>

<span data-ttu-id="cb730-293">ASP.NET Core sunan [TempData](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) özelliği bir [denetleyicisi](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller).</span><span class="sxs-lookup"><span data-stu-id="cb730-293">ASP.NET Core exposes the [TempData](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) property on a [controller](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller).</span></span> <span data-ttu-id="cb730-294">Bu özellik, dosyayı okuma kadar verileri depolar.</span><span class="sxs-lookup"><span data-stu-id="cb730-294">This property stores data until it is read.</span></span> <span data-ttu-id="cb730-295">`Keep` Ve `Peek` yöntemleri, verileri silme olmadan incelemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="cb730-295">The `Keep` and `Peek` methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="cb730-296">`TempData`birden çok tek bir istek için veri gerektiğinde yeniden yönlendirme için yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="cb730-296">`TempData` is  useful for redirection, when data is needed for more than a single request.</span></span>

<span data-ttu-id="cb730-297">`[TempData]` Özniteliği ASP.NET Core 2. 0 ' yenidir ve denetleyicileri ve sayfaları desteklenir.</span><span class="sxs-lookup"><span data-stu-id="cb730-297">The `[TempData]` attribute is new in ASP.NET Core 2.0 and is supported on controllers and pages.</span></span>

<span data-ttu-id="cb730-298">Aşağıdaki kodu değerini ayarlar `Message` kullanarak `TempData`:</span><span class="sxs-lookup"><span data-stu-id="cb730-298">The following code sets the value of `Message` using `TempData`:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,25&name=snippet_Temp)]

<span data-ttu-id="cb730-299">Aşağıdaki biçimlendirmede *Pages/Customers/Index.cshtml* dosyasını değerini görüntüler `Message` kullanarak `TempData`.</span><span class="sxs-lookup"><span data-stu-id="cb730-299">The following markup in the *Pages/Customers/Index.cshtml* file displays the value of `Message` using `TempData`.</span></span>

```cshtml
<h3>Msg: @Model.Message</h3>
```

<span data-ttu-id="cb730-300">*Pages/Customers/Index.cshtml.cs* arka plan kod dosyasının geçerli `[TempData]` özniteliğini `Message` özelliği.</span><span class="sxs-lookup"><span data-stu-id="cb730-300">The *Pages/Customers/Index.cshtml.cs* code-behind file applies the `[TempData]` attribute to the `Message` property.</span></span>

```cs
[TempData]
public string Message { get; set; }
```

<span data-ttu-id="cb730-301">Bkz: [TempData](xref:fundamentals/app-state#temp) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="cb730-301">See [TempData](xref:fundamentals/app-state#temp) for more information.</span></span>

<a name="mhpp"></a>
## <a name="multiple-handlers-per-page"></a><span data-ttu-id="cb730-302">Sayfa başına birden çok işleyicileri</span><span class="sxs-lookup"><span data-stu-id="cb730-302">Multiple handlers per page</span></span>

<span data-ttu-id="cb730-303">İki kullanarak işleyicileri sayfa için aşağıdaki sayfayı biçimlendirmeleri oluşturur `asp-page-handler` etiket Yardımcısı:</span><span class="sxs-lookup"><span data-stu-id="cb730-303">The following page generates markup for two page handlers using the `asp-page-handler` Tag Helper:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

<!-- Review: the FormActionTagHelper applies to all <form /> elements on a Razor page, even when there is no `asp-` attribute   -->

<span data-ttu-id="cb730-304">Önceki örnekte form iki düğmeleri, her kullanarak gönderme sahip `FormActionTagHelper` farklı bir URL'ye göndermek için.</span><span class="sxs-lookup"><span data-stu-id="cb730-304">The form in the preceding example has two submit buttons, each using the `FormActionTagHelper` to submit to a different URL.</span></span> <span data-ttu-id="cb730-305">`asp-page-handler` Özniteliği için bir yardımcı olan `asp-page`.</span><span class="sxs-lookup"><span data-stu-id="cb730-305">The `asp-page-handler` attribute is a companion to `asp-page`.</span></span> <span data-ttu-id="cb730-306">`asp-page-handler`bir sayfa tarafından tanımlanan işleyici yöntemlerin her biri için gönderme URL'leri oluşturur.</span><span class="sxs-lookup"><span data-stu-id="cb730-306">`asp-page-handler` generates URLs that submit to each of the handler methods defined by a page.</span></span> <span data-ttu-id="cb730-307">`asp-page`Örnek geçerli sayfasına bağlantılandırma çünkü belirtilmedi.</span><span class="sxs-lookup"><span data-stu-id="cb730-307">`asp-page` is not specified because the sample is linking to the current page.</span></span>

<span data-ttu-id="cb730-308">Arka plan kod dosyası:</span><span class="sxs-lookup"><span data-stu-id="cb730-308">The code-behind file:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]

<span data-ttu-id="cb730-309">Önceki kod kullanan *işleyici yöntemleri adlı*.</span><span class="sxs-lookup"><span data-stu-id="cb730-309">The preceding code uses *named handler methods*.</span></span> <span data-ttu-id="cb730-310">Adlandırılmış işleyici yöntemleri adından sonra metin gerçekleştirerek oluşturulur `On<HTTP Verb>` ve önce `Async` (varsa).</span><span class="sxs-lookup"><span data-stu-id="cb730-310">Named handler methods are created by taking the text in the name after `On<HTTP Verb>` and before `Async` (if present).</span></span> <span data-ttu-id="cb730-311">Önceki örnekte OnPost sayfa yöntemlerdir**JoinList**zaman uyumsuz ve OnPost**JoinListUC**zaman uyumsuz.</span><span class="sxs-lookup"><span data-stu-id="cb730-311">In the preceding example, the page methods are OnPost**JoinList**Async and OnPost**JoinListUC**Async.</span></span> <span data-ttu-id="cb730-312">İle *OnPost* ve *zaman uyumsuz* kaldırıldı, işleyici adlardır `JoinList` ve `JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="cb730-312">With *OnPost* and *Async* removed, the handler names are `JoinList` and `JoinListUC`.</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?range=12-13)]

<span data-ttu-id="cb730-313">Yukarıdaki kullanan kod, gönderildiği URL yolunu `OnPostJoinListAsync` olan `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span><span class="sxs-lookup"><span data-stu-id="cb730-313">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span></span> <span data-ttu-id="cb730-314">Gönderildiği URL yolunu `OnPostJoinListUCAsync` olan `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="cb730-314">The URL path that submits to `OnPostJoinListUCAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span></span>

## <a name="customizing-routing"></a><span data-ttu-id="cb730-315">Yönlendirme özelleştirme</span><span class="sxs-lookup"><span data-stu-id="cb730-315">Customizing Routing</span></span>

<span data-ttu-id="cb730-316">Sorgu dizesi hoşlanmıyorsanız `?handler=JoinList` URL'de işleyicisi adı URL'nin yol kısmı koymak için rota değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="cb730-316">If you don't like the query string `?handler=JoinList` in the URL, you can change the route to put the handler name in the path portion of the URL.</span></span> <span data-ttu-id="cb730-317">Bir rota şablonu sonra çift tırnak içine ekleyerek rota özelleştirebilirsiniz `@page` yönergesi.</span><span class="sxs-lookup"><span data-stu-id="cb730-317">You can customize the route by adding a route template enclosed in double quotes after the `@page` directive.</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]

<span data-ttu-id="cb730-318">Önceki yol işleyicisi adı ve URL yolunu sorgu dizesi yerine koyar.</span><span class="sxs-lookup"><span data-stu-id="cb730-318">The preceding route puts the handler name in the URL path instead of the query string.</span></span> <span data-ttu-id="cb730-319">`?` Aşağıdaki `handler` rota parametresinin isteğe bağlı olduğu anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="cb730-319">The `?` following `handler` means the route parameter is optional.</span></span>

<span data-ttu-id="cb730-320">Kullanabileceğiniz `@page` ek kesimleri ve parametreleri bir sayfanın rotaya eklemek için.</span><span class="sxs-lookup"><span data-stu-id="cb730-320">You can use `@page` to add additional segments and parameters to a page's route.</span></span> <span data-ttu-id="cb730-321">Ne olursa olsun olan **eklenmiş** sayfasının varsayılan yol için.</span><span class="sxs-lookup"><span data-stu-id="cb730-321">Whatever's there is **appended** to the default route of the page.</span></span> <span data-ttu-id="cb730-322">Sayfanın yolu değiştirmek için bir mutlak ya da sanal yolu kullanarak (gibi `"~/Some/Other/Path"`) desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="cb730-322">Using an absolute or virtual path to change the page's route (like `"~/Some/Other/Path"`) is not supported.</span></span>

## <a name="configuration-and-settings"></a><span data-ttu-id="cb730-323">Yapılandırma ve ayarları</span><span class="sxs-lookup"><span data-stu-id="cb730-323">Configuration and settings</span></span>

<span data-ttu-id="cb730-324">Gelişmiş seçenekleri yapılandırmak için genişletme yöntemi kullanmak `AddRazorPagesOptions` MVC oluşturucu üzerinde:</span><span class="sxs-lookup"><span data-stu-id="cb730-324">To configure advanced options, use the extension method `AddRazorPagesOptions` on the MVC builder:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/StartupAdvanced.cs?name=snippet_1)]

<span data-ttu-id="cb730-325">Şu anda kullanabileceğiniz `RazorPagesOptions` sayfaları için kök dizini ayarlayın veya sayfaları için uygulama modeli kuralları eklemek için.</span><span class="sxs-lookup"><span data-stu-id="cb730-325">Currently you can use the `RazorPagesOptions` to set the root directory for pages, or add application model conventions for pages.</span></span> <span data-ttu-id="cb730-326">Gelecekte bu şekilde daha fazla genişletilebilirlik etkinleştirme.</span><span class="sxs-lookup"><span data-stu-id="cb730-326">We'll enable more extensibility this way in the future.</span></span>

<span data-ttu-id="cb730-327">Görünümleri derleneceği bkz [Razor görünüm derleme](xref:mvc/views/view-compilation) .</span><span class="sxs-lookup"><span data-stu-id="cb730-327">To precompile views, see [Razor view compilation](xref:mvc/views/view-compilation) .</span></span>

<span data-ttu-id="cb730-328">[İndirme veya görüntüleme örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/index/sample).</span><span class="sxs-lookup"><span data-stu-id="cb730-328">[Download or view sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/index/sample).</span></span>

<span data-ttu-id="cb730-329">Bkz: [ASP.NET Core Razor sayfalarında ile çalışmaya başlama](xref:tutorials/razor-pages/razor-pages-start), ilgili bu girişi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="cb730-329">See [Getting started with Razor Pages in ASP.NET Core](xref:tutorials/razor-pages/razor-pages-start), which builds on this introduction.</span></span>

### <a name="specify-that-razor-pages-are-at-the-content-root"></a><span data-ttu-id="cb730-330">Razor sayfalarının içerik kök dizininde olduğunu belirtin</span><span class="sxs-lookup"><span data-stu-id="cb730-330">Specify that Razor Pages are at the content root</span></span>

<span data-ttu-id="cb730-331">Razor sayfalarının olsunlar varsayılan olarak, */sayfaları* dizin.</span><span class="sxs-lookup"><span data-stu-id="cb730-331">By default, Razor Pages are rooted in the */Pages* directory.</span></span> <span data-ttu-id="cb730-332">Ekleme [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) için [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) Razor sayfalarınızı içerik kök dizininde olduğunu belirtmek için ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) uygulamasının:</span><span class="sxs-lookup"><span data-stu-id="cb730-332">Add [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at the content root ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) of the app:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesAtContentRoot();
```

### <a name="specify-that-razor-pages-are-at-a-custom-root-directory"></a><span data-ttu-id="cb730-333">Razor sayfalarının özel kök dizininde olduğunu belirtin</span><span class="sxs-lookup"><span data-stu-id="cb730-333">Specify that Razor Pages are at a custom root directory</span></span>

<span data-ttu-id="cb730-334">Ekleme [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) için [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) Razor sayfalarınızı uygulamasında özel kök dizininde olduğunu belirtmek için (göreli bir yol belirtin):</span><span class="sxs-lookup"><span data-stu-id="cb730-334">Add [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at a custom root directory in the app (provide a relative path):</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesRoot("/path/to/razor/pages");
```

## <a name="see-also"></a><span data-ttu-id="cb730-335">Ayrıca bkz.</span><span class="sxs-lookup"><span data-stu-id="cb730-335">See also</span></span>

* [<span data-ttu-id="cb730-336">Razor sayfalarının ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="cb730-336">Getting started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="cb730-337">Razor sayfalarının yetkilendirme kuralları</span><span class="sxs-lookup"><span data-stu-id="cb730-337">Razor Pages authorization conventions</span></span>](xref:security/authorization/razor-pages-authorization)
* [<span data-ttu-id="cb730-338">Razor sayfalarının özel yolu ve sayfayı model sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="cb730-338">Razor Pages custom route and page model providers</span></span>](xref:mvc/razor-pages/razor-pages-convention-features)
* [<span data-ttu-id="cb730-339">Razor sayfalarının birim ve tümleştirme sınaması</span><span class="sxs-lookup"><span data-stu-id="cb730-339">Razor Pages unit and integration testing</span></span>](xref:testing/razor-pages-testing)
