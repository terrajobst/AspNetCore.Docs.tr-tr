---
title: ASP.NET Core Razor sayfalar giriş
author: Rick-Anderson
description: Nasıl ASP.NET Core Razor sayfalar kodlama sayfa odaklı senaryolar daha kolay ve MVC kullanmaktan daha üretken hale getirdiğini öğrenin.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 05/12/2018
uid: razor-pages/index
---
# <a name="introduction-to-razor-pages-in-aspnet-core"></a><span data-ttu-id="f5432-103">ASP.NET Core Razor sayfalar giriş</span><span class="sxs-lookup"><span data-stu-id="f5432-103">Introduction to Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="f5432-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Ryan Nowak](https://github.com/rynowak)</span><span class="sxs-lookup"><span data-stu-id="f5432-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Ryan Nowak](https://github.com/rynowak)</span></span>

<span data-ttu-id="f5432-105">Razor sayfaları, ASP.NET Core MVC sayfası odaklı senaryolar daha kolay ve daha üretken kodlama sağlayan yeni bir yönü olur.</span><span class="sxs-lookup"><span data-stu-id="f5432-105">Razor Pages is a new aspect of ASP.NET Core MVC that makes coding page-focused scenarios easier and more productive.</span></span>

<span data-ttu-id="f5432-106">Model-View-Controller yaklaşım kullanan bir öğretici için arıyorsanız bkz [ASP.NET Core MVC ile çalışmaya başlama](xref:tutorials/first-mvc-app/start-mvc).</span><span class="sxs-lookup"><span data-stu-id="f5432-106">If you're looking for a tutorial that uses the Model-View-Controller approach, see [Get started with ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span></span>

<span data-ttu-id="f5432-107">Bu belge, Razor sayfaları için bir giriş sağlar.</span><span class="sxs-lookup"><span data-stu-id="f5432-107">This document provides an introduction to Razor Pages.</span></span> <span data-ttu-id="f5432-108">Bir adım adım öğretici değil.</span><span class="sxs-lookup"><span data-stu-id="f5432-108">It's not a step by step tutorial.</span></span> <span data-ttu-id="f5432-109">Bazı bölümlerinin çok Gelişmiş bulma olmadığını [Razor sayfaları kullanmaya başlama](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="f5432-109">If you find some of the sections too advanced, see [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span></span> <span data-ttu-id="f5432-110">ASP.NET Core genel bakış için bkz. [ASP.NET Core'a giriş](xref:index).</span><span class="sxs-lookup"><span data-stu-id="f5432-110">For an overview of ASP.NET Core, see the [Introduction to ASP.NET Core](xref:index).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f5432-111">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="f5432-111">Prerequisites</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

<a name="rpvs17"></a>

## <a name="create-a-razor-pages-project"></a><span data-ttu-id="f5432-112">Razor sayfaları proje oluşturma</span><span class="sxs-lookup"><span data-stu-id="f5432-112">Create a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f5432-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f5432-113">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="f5432-114">Bkz: [Razor sayfaları kullanmaya başlama](xref:tutorials/razor-pages/razor-pages-start) Razor sayfaları proje oluşturma konusunda ayrıntılı yönergeler için.</span><span class="sxs-lookup"><span data-stu-id="f5432-114">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) for detailed instructions on how to create a Razor Pages project.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f5432-115">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f5432-115">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="f5432-116">Çalıştırma `dotnet new webapp` komut satırından.</span><span class="sxs-lookup"><span data-stu-id="f5432-116">Run `dotnet new webapp` from the command line.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="f5432-117">Çalıştırma `dotnet new razor` komut satırından.</span><span class="sxs-lookup"><span data-stu-id="f5432-117">Run `dotnet new razor` from the command line.</span></span>

::: moniker-end

<span data-ttu-id="f5432-118">Oluşturulan açın *.csproj* Mac için Visual Studio'dan dosyası</span><span class="sxs-lookup"><span data-stu-id="f5432-118">Open the generated *.csproj* file from Visual Studio for Mac.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f5432-119">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f5432-119">Visual Studio Code</span></span>](#tab/visual-studio-code)

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="f5432-120">Çalıştırma `dotnet new webapp` komut satırından.</span><span class="sxs-lookup"><span data-stu-id="f5432-120">Run `dotnet new webapp` from the command line.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="f5432-121">Çalıştırma `dotnet new razor` komut satırından.</span><span class="sxs-lookup"><span data-stu-id="f5432-121">Run `dotnet new razor` from the command line.</span></span>

::: moniker-end

---

## <a name="razor-pages"></a><span data-ttu-id="f5432-122">Razor Pages</span><span class="sxs-lookup"><span data-stu-id="f5432-122">Razor Pages</span></span>

<span data-ttu-id="f5432-123">Razor sayfaları etkin *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="f5432-123">Razor Pages is enabled in *Startup.cs*:</span></span>

[!code-cs[](index/sample/RazorPagesIntro/Startup.cs?name=snippet_Startup)]

<span data-ttu-id="f5432-124">Temel sayfa göz önünde bulundurun: <a name="OnGet"></a></span><span class="sxs-lookup"><span data-stu-id="f5432-124">Consider a basic page: <a name="OnGet"></a></span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index.cshtml)]

<span data-ttu-id="f5432-125">Yukarıdaki kod, çok Razor görünüm dosyası gibi görünüyor.</span><span class="sxs-lookup"><span data-stu-id="f5432-125">The preceding code looks a lot like a Razor view file.</span></span> <span data-ttu-id="f5432-126">Neler farklı kılan unsurdur `@page` yönergesi.</span><span class="sxs-lookup"><span data-stu-id="f5432-126">What makes it different is the `@page` directive.</span></span> <span data-ttu-id="f5432-127">`@page` Dosya, istekleri doğrudan bir denetleyici geçmeden işleme anlamına gelir ve MVC eyleme - yapar.</span><span class="sxs-lookup"><span data-stu-id="f5432-127">`@page` makes the file into an MVC action - which means that it handles requests directly, without going through a controller.</span></span> <span data-ttu-id="f5432-128">`@page` ilk Razor yönergesi bir sayfa üzerinde olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="f5432-128">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="f5432-129">`@page` diğer Razor yapıları davranışını etkiler.</span><span class="sxs-lookup"><span data-stu-id="f5432-129">`@page` affects the behavior of other Razor constructs.</span></span>

<span data-ttu-id="f5432-130">Benzer bir sayfa, kullanarak bir `PageModel` sınıfında, aşağıdaki iki dosyada gösterilir.</span><span class="sxs-lookup"><span data-stu-id="f5432-130">A similar page, using a `PageModel` class, is shown in the following two files.</span></span> <span data-ttu-id="f5432-131">*Pages/Index2.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="f5432-131">The *Pages/Index2.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index2.cshtml)]

<span data-ttu-id="f5432-132">*Pages/Index2.cshtml.cs* sayfa modeli:</span><span class="sxs-lookup"><span data-stu-id="f5432-132">The *Pages/Index2.cshtml.cs* page model:</span></span>

[!code-cs[](index/sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]

<span data-ttu-id="f5432-133">Kural olarak, `PageModel` sınıf dosyası Razor sayfası dosyası ile aynı ada sahip *.cs* eklenir.</span><span class="sxs-lookup"><span data-stu-id="f5432-133">By convention, the `PageModel` class file has the same name as the Razor Page file with *.cs* appended.</span></span> <span data-ttu-id="f5432-134">Örneğin, önceki Razor sayfa *Pages/Index2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="f5432-134">For example, the previous Razor Page is *Pages/Index2.cshtml*.</span></span> <span data-ttu-id="f5432-135">Dosyayı içeren `PageModel` sınıf adlı *Pages/Index2.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="f5432-135">The file containing the `PageModel` class is named *Pages/Index2.cshtml.cs*.</span></span>

<span data-ttu-id="f5432-136">URL yolu sayfalara ilişkilendirmelerini dosya sisteminde sayfanın konuma göre belirlenir.</span><span class="sxs-lookup"><span data-stu-id="f5432-136">The associations of URL paths to pages are determined by the page's location in the file system.</span></span> <span data-ttu-id="f5432-137">Aşağıdaki tabloda bir Razor sayfası ile eşleşen bir URL gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="f5432-137">The following table shows a Razor Page path and the matching URL:</span></span>

| <span data-ttu-id="f5432-138">Dosya adı ve yolu</span><span class="sxs-lookup"><span data-stu-id="f5432-138">File name and path</span></span>               | <span data-ttu-id="f5432-139">URL eşleştirme</span><span class="sxs-lookup"><span data-stu-id="f5432-139">matching URL</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="f5432-140">*/Pages/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="f5432-140">*/Pages/Index.cshtml*</span></span> | <span data-ttu-id="f5432-141">`/` veya `/Index`</span><span class="sxs-lookup"><span data-stu-id="f5432-141">`/` or `/Index`</span></span> |
| <span data-ttu-id="f5432-142">*/Pages/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="f5432-142">*/Pages/Contact.cshtml*</span></span> | `/Contact` |
| <span data-ttu-id="f5432-143">*/Pages/Store/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="f5432-143">*/Pages/Store/Contact.cshtml*</span></span> | `/Store/Contact` |
| <span data-ttu-id="f5432-144">*/Pages/Store/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="f5432-144">*/Pages/Store/Index.cshtml*</span></span> | <span data-ttu-id="f5432-145">`/Store` veya `/Store/Index`</span><span class="sxs-lookup"><span data-stu-id="f5432-145">`/Store` or `/Store/Index`</span></span> |

<span data-ttu-id="f5432-146">Notlar:</span><span class="sxs-lookup"><span data-stu-id="f5432-146">Notes:</span></span>

* <span data-ttu-id="f5432-147">Razor sayfaları dosyalarında çalışma zamanı arar *sayfaları* varsayılan klasör.</span><span class="sxs-lookup"><span data-stu-id="f5432-147">The runtime looks for Razor Pages files in the *Pages* folder by default.</span></span>
* <span data-ttu-id="f5432-148">`Index` bir URL bir sayfa içermediğinde varsayılan sayfasıdır.</span><span class="sxs-lookup"><span data-stu-id="f5432-148">`Index` is the default page when a URL doesn't include a page.</span></span>

## <a name="write-a-basic-form"></a><span data-ttu-id="f5432-149">Temel bir form yazma</span><span class="sxs-lookup"><span data-stu-id="f5432-149">Write a basic form</span></span>

<span data-ttu-id="f5432-150">Razor sayfaları web tarayıcıları ile kolay bir uygulama oluştururken uygulamak için kullanılan ortak desenler hale getirmek için tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="f5432-150">Razor Pages is designed to make common patterns used with web browsers easy to implement when building an app.</span></span> <span data-ttu-id="f5432-151">[Model bağlama](xref:mvc/models/model-binding), [etiket Yardımcıları](xref:mvc/views/tag-helpers/intro)ve HTML yardımcılarını tüm *yalnızca iş* bir Razor sayfası sınıfta tanımlanan özelliklere sahip.</span><span class="sxs-lookup"><span data-stu-id="f5432-151">[Model binding](xref:mvc/models/model-binding), [Tag Helpers](xref:mvc/views/tag-helpers/intro), and HTML helpers all *just work* with the properties defined in a Razor Page class.</span></span> <span data-ttu-id="f5432-152">"Bize başvurun" oluşturmak için temel bir uygulayan bir sayfa göz önünde bulundurun `Contact` modeli:</span><span class="sxs-lookup"><span data-stu-id="f5432-152">Consider a page that implements a basic "contact us" form for the `Contact` model:</span></span>

<span data-ttu-id="f5432-153">Bu belgedeki örnekler için `DbContext` içinde başlatılan [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) dosya.</span><span class="sxs-lookup"><span data-stu-id="f5432-153">For the samples in this document, the `DbContext` is initialized in the [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) file.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Startup.cs?highlight=15-16)]

<span data-ttu-id="f5432-154">Veri modeli:</span><span class="sxs-lookup"><span data-stu-id="f5432-154">The data model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Data/Customer.cs)]

<span data-ttu-id="f5432-155">Db bağlamı:</span><span class="sxs-lookup"><span data-stu-id="f5432-155">The db context:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Data/AppDbContext.cs)]

<span data-ttu-id="f5432-156">*Pages/Create.cshtml* dosyasını görüntüle:</span><span class="sxs-lookup"><span data-stu-id="f5432-156">The *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml)]

<span data-ttu-id="f5432-157">*Pages/Create.cshtml.cs* sayfa modeli:</span><span class="sxs-lookup"><span data-stu-id="f5432-157">The *Pages/Create.cshtml.cs* page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_ALL)]

<span data-ttu-id="f5432-158">Kural olarak, `PageModel` sınıfı çağrıldığında `<PageName>Model` ve aynı ad alanında sayfası.</span><span class="sxs-lookup"><span data-stu-id="f5432-158">By convention, the `PageModel` class is called `<PageName>Model` and is in the same namespace as the page.</span></span>

<span data-ttu-id="f5432-159">`PageModel` Sınıf kendi sunudan sayfasının mantığının ayrımı sağlar.</span><span class="sxs-lookup"><span data-stu-id="f5432-159">The `PageModel` class allows separation of the logic of a page from its presentation.</span></span> <span data-ttu-id="f5432-160">Bu sayfa işleyicileri sayfasına gönderilen istekleri ve sayfayı oluşturmak için kullanılan verileri tanımlar.</span><span class="sxs-lookup"><span data-stu-id="f5432-160">It defines page handlers for requests sent to the page and the data used to render the page.</span></span> <span data-ttu-id="f5432-161">Bu ayırma sayfasında bağımlılıkları üzerinden yönetmenizi sağlar [bağımlılık ekleme](xref:fundamentals/dependency-injection) ve [birim testi](xref:test/razor-pages-tests) sayfaları.</span><span class="sxs-lookup"><span data-stu-id="f5432-161">This separation allows you to manage page dependencies through [dependency injection](xref:fundamentals/dependency-injection) and to [unit test](xref:test/razor-pages-tests) the pages.</span></span>

<span data-ttu-id="f5432-162">Sayfanın bir `OnPostAsync` *işleyicisi yöntemi*, üzerinde çalıştığı `POST` ister (kullanıcı formu gönderdiğinde).</span><span class="sxs-lookup"><span data-stu-id="f5432-162">The page has an `OnPostAsync` *handler method*, which runs on `POST` requests (when a user posts the form).</span></span> <span data-ttu-id="f5432-163">Herhangi bir HTTP fiil için işleyici yöntemleri ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f5432-163">You can add handler methods for any HTTP verb.</span></span> <span data-ttu-id="f5432-164">En yaygın işleyicileri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="f5432-164">The most common handlers are:</span></span>

* <span data-ttu-id="f5432-165">`OnGet` sayfa için gerekli durumu başlatılamadı.</span><span class="sxs-lookup"><span data-stu-id="f5432-165">`OnGet` to initialize state needed for the page.</span></span> <span data-ttu-id="f5432-166">[OnGet](#OnGet) örnek.</span><span class="sxs-lookup"><span data-stu-id="f5432-166">[OnGet](#OnGet) sample.</span></span>
* <span data-ttu-id="f5432-167">`OnPost` Form gönderilerini görselleştirip işlemek için.</span><span class="sxs-lookup"><span data-stu-id="f5432-167">`OnPost` to handle form submissions.</span></span>

<span data-ttu-id="f5432-168">`Async` Adlandırma soneki isteğe bağlıdır, ancak genellikle kurala göre zaman uyumsuz işlevleri için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="f5432-168">The `Async` naming suffix is optional but is often used by convention for asynchronous functions.</span></span> <span data-ttu-id="f5432-169">`OnPostAsync` Kod önceki örnekte ne, normalde bir denetleyicisi yazmalısınız için benzer görünür.</span><span class="sxs-lookup"><span data-stu-id="f5432-169">The `OnPostAsync` code in the preceding example looks similar to what you would normally write in a controller.</span></span> <span data-ttu-id="f5432-170">Yukarıdaki kod, Razor sayfaları için tipik bir durumdur.</span><span class="sxs-lookup"><span data-stu-id="f5432-170">The preceding code is typical for Razor Pages.</span></span> <span data-ttu-id="f5432-171">MVC temelleri çoğu ister [model bağlama](xref:mvc/models/model-binding), [doğrulama](xref:mvc/models/validation), ve eylem sonuçlarını paylaşılır.</span><span class="sxs-lookup"><span data-stu-id="f5432-171">Most of the MVC primitives like [model binding](xref:mvc/models/model-binding), [validation](xref:mvc/models/validation), and action results are shared.</span></span>  <!-- Review: Ryan, can we get a list of what is shared and what isn't? -->

<span data-ttu-id="f5432-172">Önceki `OnPostAsync` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="f5432-172">The previous `OnPostAsync` method:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="f5432-173">Temel akışı `OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="f5432-173">The basic flow of `OnPostAsync`:</span></span>

<span data-ttu-id="f5432-174">Doğrulama hataları denetleyin.</span><span class="sxs-lookup"><span data-stu-id="f5432-174">Check for validation errors.</span></span>

* <span data-ttu-id="f5432-175">Herhangi bir hata varsa, verileri kaydetmek ve yönlendirin.</span><span class="sxs-lookup"><span data-stu-id="f5432-175">If there are no errors, save the data and redirect.</span></span>
* <span data-ttu-id="f5432-176">Hatalar varsa sayfası yeniden doğrulama iletileri göster.</span><span class="sxs-lookup"><span data-stu-id="f5432-176">If there are errors, show the page again with validation messages.</span></span> <span data-ttu-id="f5432-177">İstemci tarafı doğrulama geleneksel ASP.NET Core MVC uygulamaları için aynıdır.</span><span class="sxs-lookup"><span data-stu-id="f5432-177">Client-side validation is identical to traditional ASP.NET Core MVC applications.</span></span> <span data-ttu-id="f5432-178">Çoğu durumda, doğrulama hataları istemcide algılandı ve hiçbir zaman sunucuya teslim.</span><span class="sxs-lookup"><span data-stu-id="f5432-178">In many cases, validation errors would be detected on the client, and never submitted to the server.</span></span>

<span data-ttu-id="f5432-179">Verileri başarıyla girildiğinde `OnPostAsync` işleyicisi yöntem çağrılarını `RedirectToPage` örneği döndürmek için yardımcı yöntem `RedirectToPageResult`.</span><span class="sxs-lookup"><span data-stu-id="f5432-179">When the data is entered successfully, the `OnPostAsync` handler method calls the `RedirectToPage` helper method to return an instance of `RedirectToPageResult`.</span></span> <span data-ttu-id="f5432-180">`RedirectToPage` Yeni eylem sonucu, benzer olan `RedirectToAction` veya `RedirectToRoute`, ancak özelleştirilmiş sayfalar için.</span><span class="sxs-lookup"><span data-stu-id="f5432-180">`RedirectToPage` is a new action result, similar to `RedirectToAction` or `RedirectToRoute`, but customized for pages.</span></span> <span data-ttu-id="f5432-181">İsteğe bağlı olarak önceki örnekte, kök dizin sayfasına yönlendirir (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="f5432-181">In the preceding sample, it redirects to the root Index page (`/Index`).</span></span> <span data-ttu-id="f5432-182">`RedirectToPage` içinde ayrıntılı [sayfaları için URL üretimi](#url_gen) bölümü.</span><span class="sxs-lookup"><span data-stu-id="f5432-182">`RedirectToPage` is detailed in the [URL generation for Pages](#url_gen) section.</span></span>

<span data-ttu-id="f5432-183">Gönderilen bir formu, (yani sunucuya geçirilir) doğrulama hataları olduğunda`OnPostAsync` işleyicisi yöntem çağrılarını `Page` yardımcı yöntemi.</span><span class="sxs-lookup"><span data-stu-id="f5432-183">When the submitted form has validation errors (that are passed to the server), the`OnPostAsync` handler method calls the `Page` helper method.</span></span> <span data-ttu-id="f5432-184">`Page` örneği döndürür `PageResult`.</span><span class="sxs-lookup"><span data-stu-id="f5432-184">`Page` returns an instance of `PageResult`.</span></span> <span data-ttu-id="f5432-185">Döndüren `Page` denetleyicileri eylemleri nasıl döndürmek için benzer `View`.</span><span class="sxs-lookup"><span data-stu-id="f5432-185">Returning `Page` is similar to how actions in controllers return `View`.</span></span> <span data-ttu-id="f5432-186">`PageResult` Varsayılan değer</span><span class="sxs-lookup"><span data-stu-id="f5432-186">`PageResult` is the default</span></span> <!-- Review  --> <span data-ttu-id="f5432-187">dönüş türü için bir işleyici yöntemi.</span><span class="sxs-lookup"><span data-stu-id="f5432-187">return type for a handler method.</span></span> <span data-ttu-id="f5432-188">Döndürür bir işleyici yönteminin `void` sayfasını işler.</span><span class="sxs-lookup"><span data-stu-id="f5432-188">A handler method that returns `void` renders the page.</span></span>

<span data-ttu-id="f5432-189">`Customer` Özelliği kullanan `[BindProperty]` model bağlama için katılım için özniteliği.</span><span class="sxs-lookup"><span data-stu-id="f5432-189">The `Customer` property uses `[BindProperty]` attribute to opt in to model binding.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_PageModel&highlight=10-11)]

<span data-ttu-id="f5432-190">Razor sayfaları varsayılan olarak, GET olmayan fiilleri yalnızca özelliklerle bağlayın.</span><span class="sxs-lookup"><span data-stu-id="f5432-190">Razor Pages, by default, bind properties only with non-GET verbs.</span></span> <span data-ttu-id="f5432-191">Özellikleri bağlama yazmanız gereken kod miktarını azaltır.</span><span class="sxs-lookup"><span data-stu-id="f5432-191">Binding to properties can reduce the amount of code you have to write.</span></span> <span data-ttu-id="f5432-192">Form alanlarını işlemek için aynı özellik kullanarak kod azaltır bağlama (`<input asp-for="Customer.Name" />`) ve giriş kabul edin.</span><span class="sxs-lookup"><span data-stu-id="f5432-192">Binding reduces code by using the same property to render form fields (`<input asp-for="Customer.Name" />`) and accept the input.</span></span>

[!INCLUDE[](~/includes/bind-get.md)]

<span data-ttu-id="f5432-193">Giriş sayfası (*Index.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="f5432-193">The home page (*Index.cshtml*):</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml)]

<span data-ttu-id="f5432-194">İlişkili `PageModel` sınıfı (*Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="f5432-194">The associated `PageModel` class (*Index.cshtml.cs*):</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs)]

<span data-ttu-id="f5432-195">*Index.cshtml* dosyası her kişi için bir düzenleme bağlantısı oluşturmak için aşağıdaki biçimlendirme içerir:</span><span class="sxs-lookup"><span data-stu-id="f5432-195">The *Index.cshtml* file contains the following markup to create an edit link for each contact:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=21)]

<span data-ttu-id="f5432-196">[Yer işareti etiketi Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) kullanılan `asp-route-{value}` düzenleme sayfasına giden bir bağlantı oluşturmak için öznitelik.</span><span class="sxs-lookup"><span data-stu-id="f5432-196">The [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) used the `asp-route-{value}` attribute to generate a link to the Edit page.</span></span> <span data-ttu-id="f5432-197">Rota verilerini kişiyle bağlantısını içeren kimliği</span><span class="sxs-lookup"><span data-stu-id="f5432-197">The link contains route data with the contact ID.</span></span> <span data-ttu-id="f5432-198">Örneğin: `http://localhost:5000/Edit/1`</span><span class="sxs-lookup"><span data-stu-id="f5432-198">For example, `http://localhost:5000/Edit/1`.</span></span> <span data-ttu-id="f5432-199">Kullanım `asp-area` bir alanı belirtmek için özniteliği.</span><span class="sxs-lookup"><span data-stu-id="f5432-199">Use the `asp-area` attribute to specify an area.</span></span> <span data-ttu-id="f5432-200">Daha fazla bilgi için bkz. <xref:mvc/controllers/areas>.</span><span class="sxs-lookup"><span data-stu-id="f5432-200">For more information, see <xref:mvc/controllers/areas>.</span></span>

<span data-ttu-id="f5432-201">*Pages/Edit.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="f5432-201">The *Pages/Edit.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Edit.cshtml?highlight=1)]

<span data-ttu-id="f5432-202">İlk satırında `@page "{id:int}"` yönergesi.</span><span class="sxs-lookup"><span data-stu-id="f5432-202">The first line contains the `@page "{id:int}"` directive.</span></span> <span data-ttu-id="f5432-203">Yönlendirme kısıtlaması`"{id:int}"` sayfasına içeren isteklerini kabul etmek için sayfayı söyler `int` rota verileri.</span><span class="sxs-lookup"><span data-stu-id="f5432-203">The routing constraint`"{id:int}"` tells the page to accept requests to the page that contain `int` route data.</span></span> <span data-ttu-id="f5432-204">Sayfasına bir istek dönüştürülebilir rota verilerini içermiyorsa, bir `int`, çalışma zamanı HTTP 404 (bulunamadı) hatası döndürür.</span><span class="sxs-lookup"><span data-stu-id="f5432-204">If a request to the page doesn't contain route data that can be converted to an `int`, the runtime returns an HTTP 404 (not found) error.</span></span> <span data-ttu-id="f5432-205">Kimliği isteğe bağlı yapmak için URL'ye `?` rota kısıtlaması için:</span><span class="sxs-lookup"><span data-stu-id="f5432-205">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="f5432-206">*Pages/Edit.cshtml.cs* dosyası:</span><span class="sxs-lookup"><span data-stu-id="f5432-206">The *Pages/Edit.cshtml.cs* file:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Edit.cshtml.cs)]

<span data-ttu-id="f5432-207">*Index.cshtml* dosyası ayrıca Sil düğmesini her müşteri iletişim bilgileri için oluşturulacak işaretleme içerir:</span><span class="sxs-lookup"><span data-stu-id="f5432-207">The *Index.cshtml* file also contains markup to create a delete button for each customer contact:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=22-23)]

<span data-ttu-id="f5432-208">Sil düğmesini HTML işlendiğinde, `formaction` parametreleri içerir:</span><span class="sxs-lookup"><span data-stu-id="f5432-208">When the delete button is rendered in HTML, its `formaction` includes parameters for:</span></span>

* <span data-ttu-id="f5432-209">Müşteri Kimliği tarafından belirtilen iletişime `asp-route-id` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="f5432-209">The customer contact ID specified by the `asp-route-id` attribute.</span></span>
* <span data-ttu-id="f5432-210">`handler` Tarafından belirtilen `asp-page-handler` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="f5432-210">The `handler` specified by the `asp-page-handler` attribute.</span></span>

<span data-ttu-id="f5432-211">İşte bir müşteri bir işlenen Sil düğmesini bir örnek kimliği başvurun `1`:</span><span class="sxs-lookup"><span data-stu-id="f5432-211">Here is an example of a rendered delete button with a customer contact ID of `1`:</span></span>

```html
<button type="submit" formaction="/?id=1&amp;handler=delete">delete</button>
```

<span data-ttu-id="f5432-212">Düğme seçildiğinde, bir form `POST` isteği sunucuya gönderilir.</span><span class="sxs-lookup"><span data-stu-id="f5432-212">When the button is selected, a form `POST` request is sent to the server.</span></span> <span data-ttu-id="f5432-213">Kural gereği, işleyici yönteminin adı değerine göre seçilen `handler` parametre düzeni göre `OnPost[handler]Async`.</span><span class="sxs-lookup"><span data-stu-id="f5432-213">By convention, the name of the handler method is selected based on the value of the `handler` parameter according to the scheme `OnPost[handler]Async`.</span></span>

<span data-ttu-id="f5432-214">Çünkü `handler` olduğu `delete` Bu örnekte, `OnPostDeleteAsync` işleyicisi yöntemi kullanılır işleme `POST` isteği.</span><span class="sxs-lookup"><span data-stu-id="f5432-214">Because the `handler` is `delete` in this example, the `OnPostDeleteAsync` handler method is used to process the `POST` request.</span></span> <span data-ttu-id="f5432-215">Varsa `asp-page-handler` gibi farklı bir değere ayarlanmış `remove`, ada sahip bir sayfa işleyicisi yöntemi `OnPostRemoveAsync` seçilir.</span><span class="sxs-lookup"><span data-stu-id="f5432-215">If the `asp-page-handler` is set to a different value, such as `remove`, a page handler method with the name `OnPostRemoveAsync` is selected.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs?range=26-37)]

<span data-ttu-id="f5432-216">`OnPostDeleteAsync` Yöntemi:</span><span class="sxs-lookup"><span data-stu-id="f5432-216">The `OnPostDeleteAsync` method:</span></span>

* <span data-ttu-id="f5432-217">Kabul `id` Sorgu dizesinden.</span><span class="sxs-lookup"><span data-stu-id="f5432-217">Accepts the `id` from the query string.</span></span>
* <span data-ttu-id="f5432-218">Veritabanı ile müşteri iletişim bilgileri için sorgular `FindAsync`.</span><span class="sxs-lookup"><span data-stu-id="f5432-218">Queries the database for the customer contact with `FindAsync`.</span></span>
* <span data-ttu-id="f5432-219">Müşteri irtibat bulunursa, müşteri kişiler listesinden kaldırıldı.</span><span class="sxs-lookup"><span data-stu-id="f5432-219">If the customer contact is found, they're removed from the list of customer contacts.</span></span> <span data-ttu-id="f5432-220">Veritabanı güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="f5432-220">The database is updated.</span></span>
* <span data-ttu-id="f5432-221">Çağrıları `RedirectToPage` kök dizin sayfasına yeniden yönlendirmek için (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="f5432-221">Calls `RedirectToPage` to redirect to the root Index page (`/Index`).</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="mark-page-properties-as-required"></a><span data-ttu-id="f5432-222">Sayfa özelliklerini gerektiği gibi işaretleme</span><span class="sxs-lookup"><span data-stu-id="f5432-222">Mark page properties as required</span></span>

<span data-ttu-id="f5432-223">Özellikleri bir `PageModel` ile donatılmış [gerekli](/dotnet/api/system.componentmodel.dataannotations.requiredattribute) özniteliği:</span><span class="sxs-lookup"><span data-stu-id="f5432-223">Properties on a `PageModel` can be decorated with the [Required](/dotnet/api/system.componentmodel.dataannotations.requiredattribute) attribute:</span></span>

[!code-cs[](index/sample/Create.cshtml.cs?highlight=3,15-16)]

<span data-ttu-id="f5432-224">Daha fazla bilgi için [Model doğrulama](xref:mvc/models/validation).</span><span class="sxs-lookup"><span data-stu-id="f5432-224">For more information, see [Model validation](xref:mvc/models/validation).</span></span>

## <a name="manage-head-requests-with-the-onget-handler"></a><span data-ttu-id="f5432-225">HEAD isteklerini OnGet işleyici ile yönetme</span><span class="sxs-lookup"><span data-stu-id="f5432-225">Manage HEAD requests with the OnGet handler</span></span>

<span data-ttu-id="f5432-226">HEAD isteklerini belirli bir kaynak için üstbilgiler almanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="f5432-226">HEAD requests allow you to retrieve the headers for a specific resource.</span></span> <span data-ttu-id="f5432-227">GET istekleri, yanıt gövdesi HEAD isteklerini döndürmeyin.</span><span class="sxs-lookup"><span data-stu-id="f5432-227">Unlike GET requests, HEAD requests don't return a response body.</span></span>

<span data-ttu-id="f5432-228">Normalde, bir baş işleyici oluşturulur ve HEAD isteklerini çağrılır:</span><span class="sxs-lookup"><span data-stu-id="f5432-228">Ordinarily, a HEAD handler is created and called for HEAD requests:</span></span> 

```csharp
public void OnHead()
{
    HttpContext.Response.Headers.Add("HandledBy", "Handled by OnHead!");
}
```

<span data-ttu-id="f5432-229">Hiçbir baş işleyici (`OnHead`) olan tanımlanan, Razor sayfaları geri alma sayfası işleyicisi çağırma için döner (`OnGet`) ASP.NET Core 2.1 veya üzeri.</span><span class="sxs-lookup"><span data-stu-id="f5432-229">If no HEAD handler (`OnHead`) is defined, Razor Pages falls back to calling the GET page handler (`OnGet`) in ASP.NET Core 2.1 or later.</span></span> <span data-ttu-id="f5432-230">ASP.NET Core 2.1 ve 2.2 ile bu davranış oluşur [SetCompatibilityVersion](xref:mvc/compatibility-version) içinde `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="f5432-230">In ASP.NET Core 2.1 and 2.2, this behavior occurs with the [SetCompatibilityVersion](xref:mvc/compatibility-version) in `Startup.Configure`:</span></span>

```csharp
services.AddMvc()
    .SetCompatibilityVersion(Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_1);
```

<span data-ttu-id="f5432-231">Varsayılan şablon oluşturma `SetCompatibilityVersion` 2.2 ve ASP.NET Core 2.1 ile çağırın.</span><span class="sxs-lookup"><span data-stu-id="f5432-231">The default templates generate the `SetCompatibilityVersion` call in ASP.NET Core 2.1 and 2.2.</span></span>

<span data-ttu-id="f5432-232">`SetCompatibilityVersion` Razor sayfaları seçeneği etkili bir şekilde ayarlar `AllowMappingHeadRequestsToGetHandler` için `true`.</span><span class="sxs-lookup"><span data-stu-id="f5432-232">`SetCompatibilityVersion` effectively sets the Razor Pages option `AllowMappingHeadRequestsToGetHandler` to `true`.</span></span>

<span data-ttu-id="f5432-233">Tüm 2.1 davranışlarla içine edilmesiyle yerine `SetCompatibilityVersion`, siz açıkça özel davranışları için katılımı.</span><span class="sxs-lookup"><span data-stu-id="f5432-233">Rather than opting into all 2.1 behaviors with `SetCompatibilityVersion`, you can explicitly opt-in to specific behaviors.</span></span> <span data-ttu-id="f5432-234">Aşağıdaki kod içine GET işleyicisine eşleme HEAD isteklerini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="f5432-234">The following code opts into the mapping HEAD requests to the GET handler.</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        options.AllowMappingHeadRequestsToGetHandler = true;
    });
```

::: moniker-end

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a><span data-ttu-id="f5432-235">XSRF/CSRF ve Razor sayfaları</span><span class="sxs-lookup"><span data-stu-id="f5432-235">XSRF/CSRF and Razor Pages</span></span>

<span data-ttu-id="f5432-236">Herhangi bir kod yazmanıza gerek kalmaz [antiforgery doğrulama](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="f5432-236">You don't have to write any code for [antiforgery validation](xref:security/anti-request-forgery).</span></span> <span data-ttu-id="f5432-237">Otomatik olarak antiforgery belirteç oluşturma ve doğrulama Razor sayfaları dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="f5432-237">Antiforgery token generation and validation are automatically included in Razor Pages.</span></span>

<a name="layout"></a>

## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a><span data-ttu-id="f5432-238">Razor sayfaları kullanmaya düzenleri, kısmi, şablonları ve etiket yardımcılarını kullanma</span><span class="sxs-lookup"><span data-stu-id="f5432-238">Using Layouts, partials, templates, and Tag Helpers with Razor Pages</span></span>

<span data-ttu-id="f5432-239">Sayfaları Razor görüntüleme motorunu tüm özellikleriyle çalışma.</span><span class="sxs-lookup"><span data-stu-id="f5432-239">Pages work with all the capabilities of the Razor view engine.</span></span> <span data-ttu-id="f5432-240">Düzenleri, kısmi, şablonlar, etiket Yardımcıları *_ViewStart.cshtml*, *_viewımports.cshtml* geleneksel Razor görünümleri yaptıkları aynı şekilde çalışır.</span><span class="sxs-lookup"><span data-stu-id="f5432-240">Layouts, partials, templates, Tag Helpers, *_ViewStart.cshtml*, *_ViewImports.cshtml* work in the same way they do for conventional Razor views.</span></span>

<span data-ttu-id="f5432-241">Şimdi bu sayfa, bu özelliklerden bazılarını avantajlarından yararlanarak declutter.</span><span class="sxs-lookup"><span data-stu-id="f5432-241">Let's declutter this page by taking advantage of some of those capabilities.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="f5432-242">Ekleme bir [düzen sayfası](xref:mvc/views/layout) için *Pages/Shared/_Layout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="f5432-242">Add a [layout page](xref:mvc/views/layout) to *Pages/Shared/_Layout.cshtml*:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="f5432-243">Ekleme bir [düzen sayfası](xref:mvc/views/layout) için *Pages/_Layout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="f5432-243">Add a [layout page](xref:mvc/views/layout) to *Pages/_Layout.cshtml*:</span></span>

::: moniker-end

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_LayoutSimple.cshtml)]

<span data-ttu-id="f5432-244">[Düzen](xref:mvc/views/layout):</span><span class="sxs-lookup"><span data-stu-id="f5432-244">The [Layout](xref:mvc/views/layout):</span></span>

* <span data-ttu-id="f5432-245">(Sayfa düzeni dışında bölgedeyse sürece) her sayfasının düzenini denetler.</span><span class="sxs-lookup"><span data-stu-id="f5432-245">Controls the layout of each page (unless the page opts out of layout).</span></span>
* <span data-ttu-id="f5432-246">JavaScript ve stil sayfalarını gibi HTML yapıları içeri aktarır.</span><span class="sxs-lookup"><span data-stu-id="f5432-246">Imports HTML structures such as JavaScript and stylesheets.</span></span>

<span data-ttu-id="f5432-247">Bkz: [düzen sayfası](xref:mvc/views/layout) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="f5432-247">See [layout page](xref:mvc/views/layout) for more information.</span></span>

<span data-ttu-id="f5432-248">[Düzen](xref:mvc/views/layout#specifying-a-layout) özelliği ayarlandığında *Pages/_ViewStart.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="f5432-248">The [Layout](xref:mvc/views/layout#specifying-a-layout) property is set in *Pages/_ViewStart.cshtml*:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="f5432-249">Düzen bulunduğu *sayfaları/paylaşılan* klasör.</span><span class="sxs-lookup"><span data-stu-id="f5432-249">The layout is in the *Pages/Shared* folder.</span></span> <span data-ttu-id="f5432-250">Geçerli sayfa ile aynı klasörde başlangıç sayfaları başka görünümlerini (düzenleri, şablonlar, kısmi) hiyerarşik olarak arayın.</span><span class="sxs-lookup"><span data-stu-id="f5432-250">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="f5432-251">Bir düzende *sayfaları/paylaşılan* klasörü altında herhangi bir Razor sayfadan kullanılabilir *sayfaları* klasör.</span><span class="sxs-lookup"><span data-stu-id="f5432-251">A layout in the *Pages/Shared* folder can be used from any Razor page under the *Pages* folder.</span></span>

<span data-ttu-id="f5432-252">Düzen dosyası gitmesi gereken *sayfaları/paylaşılan* klasör.</span><span class="sxs-lookup"><span data-stu-id="f5432-252">The layout file should go in the *Pages/Shared* folder.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="f5432-253">Düzen bulunduğu *sayfaları* klasör.</span><span class="sxs-lookup"><span data-stu-id="f5432-253">The layout is in the *Pages* folder.</span></span> <span data-ttu-id="f5432-254">Geçerli sayfa ile aynı klasörde başlangıç sayfaları başka görünümlerini (düzenleri, şablonlar, kısmi) hiyerarşik olarak arayın.</span><span class="sxs-lookup"><span data-stu-id="f5432-254">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="f5432-255">Bir düzende *sayfaları* klasörü altında herhangi bir Razor sayfadan kullanılabilir *sayfaları* klasör.</span><span class="sxs-lookup"><span data-stu-id="f5432-255">A layout in the *Pages* folder can be used from any Razor page under the *Pages* folder.</span></span>

::: moniker-end

<span data-ttu-id="f5432-256">Öneririz **değil** Düzen dosyası koymak *görünümler/paylaşılan* klasör.</span><span class="sxs-lookup"><span data-stu-id="f5432-256">We recommend you **not** put the layout file in the *Views/Shared* folder.</span></span> <span data-ttu-id="f5432-257">*Görünümler/paylaşılan* bir MVC görünümleri modelidir.</span><span class="sxs-lookup"><span data-stu-id="f5432-257">*Views/Shared* is an MVC views pattern.</span></span> <span data-ttu-id="f5432-258">Razor sayfaları klasör hiyerarşisi, yol kuralları yararlanmayı yöneliktir.</span><span class="sxs-lookup"><span data-stu-id="f5432-258">Razor Pages are meant to rely on folder hierarchy, not path conventions.</span></span>

<span data-ttu-id="f5432-259">Bir Razor sayfası görünümü aramadan içerir *sayfaları* klasör.</span><span class="sxs-lookup"><span data-stu-id="f5432-259">View search from a Razor Page includes the *Pages* folder.</span></span> <span data-ttu-id="f5432-260">Düzenleri, şablonları ve geleneksel Razor görünümleri ile MVC denetleyicileri ile kullanmakta olduğunuz kısmi *yalnızca iş*.</span><span class="sxs-lookup"><span data-stu-id="f5432-260">The layouts, templates, and partials you're using with MVC controllers and conventional Razor views *just work*.</span></span>

<span data-ttu-id="f5432-261">Ekleme bir *Pages/_ViewImports.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="f5432-261">Add a *Pages/_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]

<span data-ttu-id="f5432-262">`@namespace` öğreticinin ilerleyen bölümlerinde açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="f5432-262">`@namespace` is explained later in the tutorial.</span></span> <span data-ttu-id="f5432-263">`@addTagHelper` Yönergesi getirdiği [yerleşik etiket Yardımcıları](xref:mvc/views/tag-helpers/builtin-th/Index) tüm sayfalara *sayfaları* klasör.</span><span class="sxs-lookup"><span data-stu-id="f5432-263">The `@addTagHelper` directive brings in the [built-in Tag Helpers](xref:mvc/views/tag-helpers/builtin-th/Index) to all the pages in the *Pages* folder.</span></span>

<a name="namespace"></a>

<span data-ttu-id="f5432-264">Zaman `@namespace` yönergesi bir sayfada açıkça kullanılır:</span><span class="sxs-lookup"><span data-stu-id="f5432-264">When the `@namespace` directive is used explicitly on a page:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]

<span data-ttu-id="f5432-265">Yönergesi, ad alanı sayfası için ayarlar.</span><span class="sxs-lookup"><span data-stu-id="f5432-265">The directive sets the namespace for the page.</span></span> <span data-ttu-id="f5432-266">`@model` Yönergesi ad alanını katmak gerekmez.</span><span class="sxs-lookup"><span data-stu-id="f5432-266">The `@model` directive doesn't need to include the namespace.</span></span>

<span data-ttu-id="f5432-267">Zaman `@namespace` yönergesi kapsanıyorsa *_viewımports.cshtml*, belirtilen ad alanı içe aktaran sayfasında oluşturulan ad alanı öneki sağlayan `@namespace` yönergesi.</span><span class="sxs-lookup"><span data-stu-id="f5432-267">When the `@namespace` directive is contained in *_ViewImports.cshtml*, the specified namespace supplies the prefix for the generated namespace in the Page that imports the `@namespace` directive.</span></span> <span data-ttu-id="f5432-268">Oluşturulan ad alanı (sonek kısmı) geri kalanı içeren klasörü arasında noktayla ayrılmış göreli yoludur *_viewımports.cshtml* ve sayfayı içeren klasör.</span><span class="sxs-lookup"><span data-stu-id="f5432-268">The rest of the generated namespace (the suffix portion) is the dot-separated relative path between the folder containing *_ViewImports.cshtml* and the folder containing the page.</span></span>

<span data-ttu-id="f5432-269">Örneğin, `PageModel` sınıfı *Pages/Customers/Edit.cshtml.cs* ad alanını açıkça ayarlar:</span><span class="sxs-lookup"><span data-stu-id="f5432-269">For example, the `PageModel` class *Pages/Customers/Edit.cshtml.cs* explicitly sets the namespace:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=snippet_namespace)]

<span data-ttu-id="f5432-270">*Pages/_ViewImports.cshtml* dosyasını ayarlar şu ad alanı:</span><span class="sxs-lookup"><span data-stu-id="f5432-270">The *Pages/_ViewImports.cshtml* file sets the following namespace:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]

<span data-ttu-id="f5432-271">Oluşturulan ad alanı için *Pages/Customers/Edit.cshtml* Razor sayfası aynıdır `PageModel` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="f5432-271">The generated namespace for the *Pages/Customers/Edit.cshtml* Razor Page is the same as the `PageModel` class.</span></span>

<span data-ttu-id="f5432-272">`@namespace` *Geleneksel Razor görünümleri ile de çalışır.*</span><span class="sxs-lookup"><span data-stu-id="f5432-272">`@namespace` *also works with conventional Razor views.*</span></span>

<span data-ttu-id="f5432-273">Özgün *Pages/Create.cshtml* dosyasını görüntüle:</span><span class="sxs-lookup"><span data-stu-id="f5432-273">The original *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml?highlight=2)]

<span data-ttu-id="f5432-274">Güncelleştirilmiş *Pages/Create.cshtml* dosyasını görüntüle:</span><span class="sxs-lookup"><span data-stu-id="f5432-274">The updated *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/Create.cshtml?highlight=2)]

<span data-ttu-id="f5432-275">[Razor sayfaları başlangıç projesini](#rpvs17) içeren *Pages/_ValidationScriptsPartial.cshtml*, istemci tarafı doğrulama kancaları.</span><span class="sxs-lookup"><span data-stu-id="f5432-275">The [Razor Pages starter project](#rpvs17) contains the *Pages/_ValidationScriptsPartial.cshtml*, which hooks up client-side validation.</span></span>

<span data-ttu-id="f5432-276">Kısmi görünümler hakkında daha fazla bilgi için bkz. <xref:mvc/views/partial>.</span><span class="sxs-lookup"><span data-stu-id="f5432-276">For more information on partial views, see <xref:mvc/views/partial>.</span></span>

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a><span data-ttu-id="f5432-277">Sayfaları için URL üretimi</span><span class="sxs-lookup"><span data-stu-id="f5432-277">URL generation for Pages</span></span>

<span data-ttu-id="f5432-278">`Create` Sayfasında, daha önce kullandığı gösterilen `RedirectToPage`:</span><span class="sxs-lookup"><span data-stu-id="f5432-278">The `Create` page, shown previously, uses `RedirectToPage`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=10)]

<span data-ttu-id="f5432-279">Uygulama, aşağıdaki dosya/klasör yapısına sahiptir:</span><span class="sxs-lookup"><span data-stu-id="f5432-279">The app has the following file/folder structure:</span></span>

* <span data-ttu-id="f5432-280">*/ Sayfaları*</span><span class="sxs-lookup"><span data-stu-id="f5432-280">*/Pages*</span></span>

  * <span data-ttu-id="f5432-281">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="f5432-281">*Index.cshtml*</span></span>
  * <span data-ttu-id="f5432-282">*/ Müşteriler*</span><span class="sxs-lookup"><span data-stu-id="f5432-282">*/Customers*</span></span>

    * <span data-ttu-id="f5432-283">*Create.cshtml*</span><span class="sxs-lookup"><span data-stu-id="f5432-283">*Create.cshtml*</span></span>
    * <span data-ttu-id="f5432-284">*Edit.cshtml*</span><span class="sxs-lookup"><span data-stu-id="f5432-284">*Edit.cshtml*</span></span>
    * <span data-ttu-id="f5432-285">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="f5432-285">*Index.cshtml*</span></span>

<span data-ttu-id="f5432-286">*Pages/Customers/Create.cshtml* ve *Pages/Customers/Edit.cshtml* yeniden yönlendirme sayfası *Pages/Index.cshtml* başarılı olduktan sonra.</span><span class="sxs-lookup"><span data-stu-id="f5432-286">The *Pages/Customers/Create.cshtml* and *Pages/Customers/Edit.cshtml* pages redirect to *Pages/Index.cshtml* after success.</span></span> <span data-ttu-id="f5432-287">Dize `/Index` önceki sayfaya erişmek için URI'ın bir parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="f5432-287">The string `/Index` is part of the URI to access the preceding page.</span></span> <span data-ttu-id="f5432-288">Dize `/Index` için URI oluşturmak için kullanılan *Pages/Index.cshtml* sayfası.</span><span class="sxs-lookup"><span data-stu-id="f5432-288">The string `/Index` can be used to generate URIs to the *Pages/Index.cshtml* page.</span></span> <span data-ttu-id="f5432-289">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="f5432-289">For example:</span></span>

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">My Index Page</a>`
* `RedirectToPage("/Index")`

<span data-ttu-id="f5432-290">Sayfa adı sayfasına kökünden yoludur */sayfaları* önde gelen dahil olmak üzere klasörü `/` (örneğin, `/Index`).</span><span class="sxs-lookup"><span data-stu-id="f5432-290">The page name is the path to the page from the root */Pages* folder including a leading `/` (for example, `/Index`).</span></span> <span data-ttu-id="f5432-291">Önceki URL oluşturma örnekleri, runbook'a kod bir URL Gelişmiş Seçenekler ve işlevsel yetenekleri sunar.</span><span class="sxs-lookup"><span data-stu-id="f5432-291">The preceding URL generation samples offer enhanced options and functional capabilities over hardcoding a URL.</span></span> <span data-ttu-id="f5432-292">URL üretimi kullanan [yönlendirme](xref:mvc/controllers/routing) ve oluşturmak ve parametreleri hedef yolda bir rota nasıl tanımlandığını göre kodlayın.</span><span class="sxs-lookup"><span data-stu-id="f5432-292">URL generation uses [routing](xref:mvc/controllers/routing) and can generate and encode parameters according to how the route is defined in the destination path.</span></span>

<span data-ttu-id="f5432-293">URL üretimi sayfaları için göreli adlarını destekler.</span><span class="sxs-lookup"><span data-stu-id="f5432-293">URL generation for pages supports relative names.</span></span> <span data-ttu-id="f5432-294">Aşağıdaki tabloda, hangi dizin sayfası ile farklı seçili olduğunu gösterir `RedirectToPage` parametrelerinden *Pages/Customers/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="f5432-294">The following table shows which Index page is selected with different `RedirectToPage` parameters from *Pages/Customers/Create.cshtml*:</span></span>

| <span data-ttu-id="f5432-295">RedirectToPage(x)</span><span class="sxs-lookup"><span data-stu-id="f5432-295">RedirectToPage(x)</span></span>| <span data-ttu-id="f5432-296">Sayfa</span><span class="sxs-lookup"><span data-stu-id="f5432-296">Page</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="f5432-297">RedirectToPage("/Index")</span><span class="sxs-lookup"><span data-stu-id="f5432-297">RedirectToPage("/Index")</span></span> | <span data-ttu-id="f5432-298">*Sayfalar/dizin*</span><span class="sxs-lookup"><span data-stu-id="f5432-298">*Pages/Index*</span></span> |
| <span data-ttu-id="f5432-299">RedirectToPage("./Index");</span><span class="sxs-lookup"><span data-stu-id="f5432-299">RedirectToPage("./Index");</span></span> | <span data-ttu-id="f5432-300">*Müşteriler/sayfalar/dizin*</span><span class="sxs-lookup"><span data-stu-id="f5432-300">*Pages/Customers/Index*</span></span> |
| <span data-ttu-id="f5432-301">RedirectToPage(".. / Dizin")</span><span class="sxs-lookup"><span data-stu-id="f5432-301">RedirectToPage("../Index")</span></span> | <span data-ttu-id="f5432-302">*Sayfalar/dizin*</span><span class="sxs-lookup"><span data-stu-id="f5432-302">*Pages/Index*</span></span> |
| <span data-ttu-id="f5432-303">RedirectToPage("Index")</span><span class="sxs-lookup"><span data-stu-id="f5432-303">RedirectToPage("Index")</span></span>  | <span data-ttu-id="f5432-304">*Müşteriler/sayfalar/dizin*</span><span class="sxs-lookup"><span data-stu-id="f5432-304">*Pages/Customers/Index*</span></span> |

<span data-ttu-id="f5432-305">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, ve `RedirectToPage("../Index")` olan *göreli adlar*.</span><span class="sxs-lookup"><span data-stu-id="f5432-305">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, and `RedirectToPage("../Index")`  are *relative names*.</span></span> <span data-ttu-id="f5432-306">`RedirectToPage` Parametresi *birleştirilmiş* ile hedef sayfanın adını işlem için geçerli sayfasının yolu.</span><span class="sxs-lookup"><span data-stu-id="f5432-306">The `RedirectToPage` parameter is *combined* with the path of the current page to compute the name of the destination page.</span></span>  <!-- Review: Original had The provided string is combined with the page name of the current page to compute the name of the destination page.  page name, not page path -->

<span data-ttu-id="f5432-307">Göreli adı bağlama siteleri karmaşık bir yapısı ile derleme yaparken yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="f5432-307">Relative name linking is useful when building sites with a complex structure.</span></span> <span data-ttu-id="f5432-308">Bir klasördeki sayfalar arasında bağlamak için göreli adları kullanıyorsa, bu klasörü yeniden adlandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f5432-308">If you use relative names to link between pages in a folder, you can rename that folder.</span></span> <span data-ttu-id="f5432-309">(Bunlar klasör adı olmadığından) tüm bağlantıların hala çalıştığından.</span><span class="sxs-lookup"><span data-stu-id="f5432-309">All the links still work (because they didn't include the folder name).</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="f5432-310">Farklı bir sayfasına yeniden yönlendirmek için [alan](xref:mvc/controllers/areas), alan belirtin:</span><span class="sxs-lookup"><span data-stu-id="f5432-310">To redirect to a page in a different [Area](xref:mvc/controllers/areas), specify the area:</span></span>

```csharp
RedirectToPage("/Index", new { area = "Services" });
```

<span data-ttu-id="f5432-311">Daha fazla bilgi için bkz. <xref:mvc/controllers/areas>.</span><span class="sxs-lookup"><span data-stu-id="f5432-311">For more information, see <xref:mvc/controllers/areas>.</span></span>

## <a name="viewdata-attribute"></a><span data-ttu-id="f5432-312">ViewData özniteliği</span><span class="sxs-lookup"><span data-stu-id="f5432-312">ViewData attribute</span></span>

<span data-ttu-id="f5432-313">Veri içeren bir sayfa geçilebilir [ViewDataAttribute](/dotnet/api/microsoft.aspnetcore.mvc.viewdataattribute).</span><span class="sxs-lookup"><span data-stu-id="f5432-313">Data can be passed to a page with [ViewDataAttribute](/dotnet/api/microsoft.aspnetcore.mvc.viewdataattribute).</span></span> <span data-ttu-id="f5432-314">Denetleyicilerde veya modellerde Razor sayfası özelliklerini düzenlenmiş ile `[ViewData]` depolanır ve gelen yüklenen değerlerine sahip [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary).</span><span class="sxs-lookup"><span data-stu-id="f5432-314">Properties on controllers or Razor Page models decorated with `[ViewData]` have their values stored and loaded from the [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary).</span></span>

<span data-ttu-id="f5432-315">Aşağıdaki örnekte, `AboutModel` içeren bir `Title` özelliği düzenlenmiş ile `[ViewData]`.</span><span class="sxs-lookup"><span data-stu-id="f5432-315">In the following example, the `AboutModel` contains a `Title` property decorated with `[ViewData]`.</span></span> <span data-ttu-id="f5432-316">`Title` Özelliği hakkında sayfasının başlığı ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="f5432-316">The `Title` property is set to the title of the About page:</span></span>

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

<span data-ttu-id="f5432-317">Hakkında sayfasında erişim `Title` özelliği model özelliği olarak:</span><span class="sxs-lookup"><span data-stu-id="f5432-317">In the About page, access the `Title` property as a model property:</span></span>

```cshtml
<h1>@Model.Title</h1>
```

<span data-ttu-id="f5432-318">Düzende dışında ViewData sözlükten başlığını okuyun:</span><span class="sxs-lookup"><span data-stu-id="f5432-318">In the layout, the title is read from the ViewData dictionary:</span></span>

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"] - WebApplication</title>
    ...
```

::: moniker-end

## <a name="tempdata"></a><span data-ttu-id="f5432-319">TempData</span><span class="sxs-lookup"><span data-stu-id="f5432-319">TempData</span></span>

<span data-ttu-id="f5432-320">ASP.NET Core sunan [TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) özelliği bir [denetleyicisi](/dotnet/api/microsoft.aspnetcore.mvc.controller).</span><span class="sxs-lookup"><span data-stu-id="f5432-320">ASP.NET Core exposes the [TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) property on a [controller](/dotnet/api/microsoft.aspnetcore.mvc.controller).</span></span> <span data-ttu-id="f5432-321">Bu özellik, okunur kadar verileri depolar.</span><span class="sxs-lookup"><span data-stu-id="f5432-321">This property stores data until it's read.</span></span> <span data-ttu-id="f5432-322">`Keep` Ve `Peek` yöntemleri silme olmadan verileri incelemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="f5432-322">The `Keep` and `Peek` methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="f5432-323">`TempData` için yeniden yönlendirme, birden çok tek bir istek verileri gerektiğinde yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="f5432-323">`TempData` is  useful for redirection, when data is needed for more than a single request.</span></span>

<span data-ttu-id="f5432-324">`[TempData]` Özniteliği ASP.NET Core 2.0 sürümünde yenidir ve denetleyicileri ve sayfaları desteklenmektedir.</span><span class="sxs-lookup"><span data-stu-id="f5432-324">The `[TempData]` attribute is new in ASP.NET Core 2.0 and is supported on controllers and pages.</span></span>

<span data-ttu-id="f5432-325">Aşağıdaki kodu değerini ayarlar `Message` kullanarak `TempData`:</span><span class="sxs-lookup"><span data-stu-id="f5432-325">The following code sets the value of `Message` using `TempData`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,25&name=snippet_Temp)]

<span data-ttu-id="f5432-326">Aşağıdaki biçimlendirmede *Pages/Customers/Index.cshtml* dosya değerini görüntülediğine `Message` kullanarak `TempData`.</span><span class="sxs-lookup"><span data-stu-id="f5432-326">The following markup in the *Pages/Customers/Index.cshtml* file displays the value of `Message` using `TempData`.</span></span>

```cshtml
<h3>Msg: @Model.Message</h3>
```

<span data-ttu-id="f5432-327">*Pages/Customers/Index.cshtml.cs* sayfa modeli uygular `[TempData]` özniteliğini `Message` özelliği.</span><span class="sxs-lookup"><span data-stu-id="f5432-327">The *Pages/Customers/Index.cshtml.cs* page model applies the `[TempData]` attribute to the `Message` property.</span></span>

```cs
[TempData]
public string Message { get; set; }
```

<span data-ttu-id="f5432-328">Daha fazla bilgi için [TempData](xref:fundamentals/app-state#tempdata) .</span><span class="sxs-lookup"><span data-stu-id="f5432-328">For more information, see [TempData](xref:fundamentals/app-state#tempdata) .</span></span>

<a name="mhpp"></a>

## <a name="multiple-handlers-per-page"></a><span data-ttu-id="f5432-329">Sayfa başına birden çok işleyicileri</span><span class="sxs-lookup"><span data-stu-id="f5432-329">Multiple handlers per page</span></span>

<span data-ttu-id="f5432-330">İki sayfa kullanarak işleyicileri için şu sayfaya biçimlendirmeleri oluşturur `asp-page-handler` etiketi Yardımcısı:</span><span class="sxs-lookup"><span data-stu-id="f5432-330">The following page generates markup for two page handlers using the `asp-page-handler` Tag Helper:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

<!-- Review: the FormActionTagHelper applies to all <form /> elements on a Razor page, even when there's no `asp-` attribute   -->

<span data-ttu-id="f5432-331">Önceki örnekte iki düğmeleri kullanarak her gönderme formundadır `FormActionTagHelper` için farklı bir URL göndermek için.</span><span class="sxs-lookup"><span data-stu-id="f5432-331">The form in the preceding example has two submit buttons, each using the `FormActionTagHelper` to submit to a different URL.</span></span> <span data-ttu-id="f5432-332">`asp-page-handler` Özniteliktir na yardımcı olarak `asp-page`.</span><span class="sxs-lookup"><span data-stu-id="f5432-332">The `asp-page-handler` attribute is a companion to `asp-page`.</span></span> <span data-ttu-id="f5432-333">`asp-page-handler` bir sayfa tarafından tanımlanan işleyici yöntemlerin her biri için gönderme URL oluşturur.</span><span class="sxs-lookup"><span data-stu-id="f5432-333">`asp-page-handler` generates URLs that submit to each of the handler methods defined by a page.</span></span> <span data-ttu-id="f5432-334">`asp-page` örnek, geçerli sayfa için bağlama için belirtilmemiş.</span><span class="sxs-lookup"><span data-stu-id="f5432-334">`asp-page` isn't specified because the sample is linking to the current page.</span></span>

<span data-ttu-id="f5432-335">Sayfa modeli:</span><span class="sxs-lookup"><span data-stu-id="f5432-335">The page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]

<span data-ttu-id="f5432-336">Önceki kod *işleyici yöntemleri adlı*.</span><span class="sxs-lookup"><span data-stu-id="f5432-336">The preceding code uses *named handler methods*.</span></span> <span data-ttu-id="f5432-337">Adlandırılmış işleyici yöntemleri adında sonra metin alarak oluşturulur `On<HTTP Verb>` ve önce `Async` (varsa).</span><span class="sxs-lookup"><span data-stu-id="f5432-337">Named handler methods are created by taking the text in the name after `On<HTTP Verb>` and before `Async` (if present).</span></span> <span data-ttu-id="f5432-338">Önceki örnekte OnPost sayfa yöntemlerdir**JoinList**zaman uyumsuz ve OnPost**JoinListUC**zaman uyumsuz.</span><span class="sxs-lookup"><span data-stu-id="f5432-338">In the preceding example, the page methods are OnPost**JoinList**Async and OnPost**JoinListUC**Async.</span></span> <span data-ttu-id="f5432-339">İle *OnPost* ve *zaman uyumsuz* kaldırıldıysa, işleyici adları olan `JoinList` ve `JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="f5432-339">With *OnPost* and *Async* removed, the handler names are `JoinList` and `JoinListUC`.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?range=12-13)]

<span data-ttu-id="f5432-340">Kod önceki kullanarak, gönderildiği URL yolu `OnPostJoinListAsync` olduğu `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span><span class="sxs-lookup"><span data-stu-id="f5432-340">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span></span> <span data-ttu-id="f5432-341">Gönderildiği URL yolu `OnPostJoinListUCAsync` olduğu `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="f5432-341">The URL path that submits to `OnPostJoinListUCAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span></span>

## <a name="custom-routes"></a><span data-ttu-id="f5432-342">Özel yollar</span><span class="sxs-lookup"><span data-stu-id="f5432-342">Custom routes</span></span>

<span data-ttu-id="f5432-343">Kullanım `@page` yönergesini:</span><span class="sxs-lookup"><span data-stu-id="f5432-343">Use the `@page` directive to:</span></span>

* <span data-ttu-id="f5432-344">Bir sayfa için özel bir yol belirtin.</span><span class="sxs-lookup"><span data-stu-id="f5432-344">Specify a custom route to a page.</span></span> <span data-ttu-id="f5432-345">Rota hakkında sayfasına gibi ayarlanabilir `/Some/Other/Path` ile `@page "/Some/Other/Path"`.</span><span class="sxs-lookup"><span data-stu-id="f5432-345">For example, the route to the About page can be set to `/Some/Other/Path` with `@page "/Some/Other/Path"`.</span></span>
* <span data-ttu-id="f5432-346">Bir sayfanın varsayılan yol kesimleri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="f5432-346">Append segments to a page's default route.</span></span> <span data-ttu-id="f5432-347">Örneğin, bir "Item" segment ile bir sayfanın varsayılan rota eklenebilir `@page "item"`.</span><span class="sxs-lookup"><span data-stu-id="f5432-347">For example, an "item" segment can be added to a page's default route with `@page "item"`.</span></span>
* <span data-ttu-id="f5432-348">Bir sayfanın varsayılan rota için parametreleri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="f5432-348">Append parameters to a page's default route.</span></span> <span data-ttu-id="f5432-349">Örneğin, bir kimliği parametre `id`, içeren bir sayfa için gerekli olabilir `@page "{id}"`.</span><span class="sxs-lookup"><span data-stu-id="f5432-349">For example, an ID parameter, `id`, can be required for a page with `@page "{id}"`.</span></span>

<span data-ttu-id="f5432-350">Bir tilde işareti tarafından atanan bir köküne göreli yol (`~`) yolunun başında desteklenir.</span><span class="sxs-lookup"><span data-stu-id="f5432-350">A root-relative path designated by a tilde (`~`) at the beginning of the path is supported.</span></span> <span data-ttu-id="f5432-351">Örneğin, `@page "~/Some/Other/Path"` aynı `@page "/Some/Other/Path"`.</span><span class="sxs-lookup"><span data-stu-id="f5432-351">For example, `@page "~/Some/Other/Path"` is the same as `@page "/Some/Other/Path"`.</span></span>

<span data-ttu-id="f5432-352">Sorgu dizesi değiştirebilirsiniz `?handler=JoinList` yol kesimi URL'yi `/JoinList` rota şablonu belirterek `@page "{handler?}"`.</span><span class="sxs-lookup"><span data-stu-id="f5432-352">You can change the query string `?handler=JoinList` in the URL to a route segment `/JoinList` by specifying the route template `@page "{handler?}"`.</span></span>

<span data-ttu-id="f5432-353">Sorgu dizesi beğenmezseniz `?handler=JoinList` URL'de, yol işleyicisi adı URL'nin yol kısmı put değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f5432-353">If you don't like the query string `?handler=JoinList` in the URL, you can change the route to put the handler name in the path portion of the URL.</span></span> <span data-ttu-id="f5432-354">Rota sonra çift tırnak içine alınmış bir rota şablonu ekleyerek özelleştirebilirsiniz `@page` yönergesi.</span><span class="sxs-lookup"><span data-stu-id="f5432-354">You can customize the route by adding a route template enclosed in double quotes after the `@page` directive.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]

<span data-ttu-id="f5432-355">Kod önceki kullanarak, gönderildiği URL yolu `OnPostJoinListAsync` olduğu `http://localhost:5000/Customers/CreateFATH/JoinList`.</span><span class="sxs-lookup"><span data-stu-id="f5432-355">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `http://localhost:5000/Customers/CreateFATH/JoinList`.</span></span> <span data-ttu-id="f5432-356">Gönderildiği URL yolu `OnPostJoinListUCAsync` olduğu `http://localhost:5000/Customers/CreateFATH/JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="f5432-356">The URL path that submits to `OnPostJoinListUCAsync` is `http://localhost:5000/Customers/CreateFATH/JoinListUC`.</span></span>

<span data-ttu-id="f5432-357">`?` Aşağıdaki `handler` rota parametresinin isteğe bağlı olduğu anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="f5432-357">The `?` following `handler` means the route parameter is optional.</span></span>

## <a name="configuration-and-settings"></a><span data-ttu-id="f5432-358">Yapılandırma ve ayarlar</span><span class="sxs-lookup"><span data-stu-id="f5432-358">Configuration and settings</span></span>

<span data-ttu-id="f5432-359">Gelişmiş seçeneklerini yapılandırmak için genişletme yöntemini kullanmak `AddRazorPagesOptions` MVC oluşturucu üzerinde:</span><span class="sxs-lookup"><span data-stu-id="f5432-359">To configure advanced options, use the extension method `AddRazorPagesOptions` on the MVC builder:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/StartupAdvanced.cs?name=snippet_1)]

<span data-ttu-id="f5432-360">Şu anda kullanabileceğiniz `RazorPagesOptions` sayfa için kök dizine ayarlayın veya uygulama sayfaları için model kuralları ekleyin.</span><span class="sxs-lookup"><span data-stu-id="f5432-360">Currently you can use the `RazorPagesOptions` to set the root directory for pages, or add application model conventions for pages.</span></span> <span data-ttu-id="f5432-361">Daha fazla genişletilebilirlik bu şekilde gelecekte etkinleştiririz.</span><span class="sxs-lookup"><span data-stu-id="f5432-361">We'll enable more extensibility this way in the future.</span></span>

<span data-ttu-id="f5432-362">Görünümlerinizi önceden derleme için bkz: [Razor görünüm derlemesi](xref:mvc/views/view-compilation) .</span><span class="sxs-lookup"><span data-stu-id="f5432-362">To precompile views, see [Razor view compilation](xref:mvc/views/view-compilation) .</span></span>

<span data-ttu-id="f5432-363">[İndirme veya görüntüleme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/index/sample).</span><span class="sxs-lookup"><span data-stu-id="f5432-363">[Download or view sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/index/sample).</span></span>

<span data-ttu-id="f5432-364">Bkz: [Razor sayfaları kullanmaya başlama](xref:tutorials/razor-pages/razor-pages-start), ilgili bu girişi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="f5432-364">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start), which builds on this introduction.</span></span>

### <a name="specify-that-razor-pages-are-at-the-content-root"></a><span data-ttu-id="f5432-365">Razor sayfaları içerik kök dizininde olduğunu belirtin</span><span class="sxs-lookup"><span data-stu-id="f5432-365">Specify that Razor Pages are at the content root</span></span>

<span data-ttu-id="f5432-366">Varsayılan olarak, Razor sayfaları, köklü olmayan */sayfaları* dizin.</span><span class="sxs-lookup"><span data-stu-id="f5432-366">By default, Razor Pages are rooted in the */Pages* directory.</span></span> <span data-ttu-id="f5432-367">Ekleme [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) için [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) , Razor sayfaları içerik kök dizininde olduğunu belirtmek için ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) uygulama:</span><span class="sxs-lookup"><span data-stu-id="f5432-367">Add [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at the content root ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) of the app:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesAtContentRoot();
```

### <a name="specify-that-razor-pages-are-at-a-custom-root-directory"></a><span data-ttu-id="f5432-368">Razor sayfaları özel kök dizininde olduğunu belirtin</span><span class="sxs-lookup"><span data-stu-id="f5432-368">Specify that Razor Pages are at a custom root directory</span></span>

<span data-ttu-id="f5432-369">Ekleme [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) için [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) , Razor sayfaları bir uygulamadaki özel kök dizininde olduğunu belirtmek için (göreli bir yol belirtin):</span><span class="sxs-lookup"><span data-stu-id="f5432-369">Add [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at a custom root directory in the app (provide a relative path):</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesRoot("/path/to/razor/pages");
```

## <a name="additional-resources"></a><span data-ttu-id="f5432-370">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="f5432-370">Additional resources</span></span>

* <xref:index>
* <xref:mvc/views/razor>
* <xref:mvc/controllers/areas>
* <xref:tutorials/razor-pages/razor-pages-start>
* <xref:security/authorization/razor-pages-authorization>
* <xref:razor-pages/razor-pages-conventions>
* <xref:test/razor-pages-tests>
* <xref:mvc/views/partial>
