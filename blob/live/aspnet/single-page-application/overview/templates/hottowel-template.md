---
uid: single-page-application/overview/templates/hottowel-template
title: "Sık kullanılan havlu şablon | Microsoft Docs"
author: madskristensen
description: "HotTowel şablonu"
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/09/2013
ms.topic: article
ms.assetid: 75af2e17-6ed3-4d24-8ea1-bc340027c318
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/templates/hottowel-template
msc.type: authoredcontent
ms.openlocfilehash: bfc6e2c884c422f44e8be5f4f29554ae86f7ecb6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="hot-towel-template"></a><span data-ttu-id="1428d-103">Sık kullanılan havlu şablonu</span><span class="sxs-lookup"><span data-stu-id="1428d-103">Hot Towel template</span></span>
====================
<span data-ttu-id="1428d-104">tarafından [Kristensen Mads](https://github.com/madskristensen)</span><span class="sxs-lookup"><span data-stu-id="1428d-104">by [Mads Kristensen](https://github.com/madskristensen)</span></span>

> <span data-ttu-id="1428d-105">Sık kullanılan havlu MVC şablonu tarafından John Papa yazılır</span><span class="sxs-lookup"><span data-stu-id="1428d-105">The Hot Towel MVC Template is written by John Papa</span></span>
> 
> <span data-ttu-id="1428d-106">İndirmek için hangi sürümü seçin:</span><span class="sxs-lookup"><span data-stu-id="1428d-106">Choose which version to download:</span></span>
> 
> [<span data-ttu-id="1428d-107">Visual Studio 2012 için sık kullanılan havlu MVC şablonu</span><span class="sxs-lookup"><span data-stu-id="1428d-107">Hot Towel MVC Template for Visual Studio 2012</span></span>](https://visualstudiogallery.msdn.microsoft.com/1f68fbe8-b4e9-4968-9fd3-ddc7cbc52dca)
> 
> [<span data-ttu-id="1428d-108">Visual Studio 2013 için sık kullanılan havlu MVC şablonu</span><span class="sxs-lookup"><span data-stu-id="1428d-108">Hot Towel MVC Template for Visual Studio 2013</span></span>](https://visualstudiogallery.msdn.microsoft.com/1eb8780d-d522-4dcf-bf56-56f0eab305c2)


> <span data-ttu-id="1428d-109">Sık kullanılan havlu: biri olmadan SPA gitmek istemeyeceğiniz için!</span><span class="sxs-lookup"><span data-stu-id="1428d-109">Hot Towel: Because you don't want to go to the SPA without one!</span></span>


<span data-ttu-id="1428d-110">Bir SPA oluşturmak istediğiniz ancak nerede başlatılacağını karar veremez?</span><span class="sxs-lookup"><span data-stu-id="1428d-110">Want to build a SPA but can't decide where to start?</span></span> <span data-ttu-id="1428d-111">Sık kullanılan havlu kullanın ve saniye cinsinden bir SPA ve üzerinde oluşturmak için ihtiyacınız olan araçları gerekir!</span><span class="sxs-lookup"><span data-stu-id="1428d-111">Use Hot Towel and in seconds you'll have a SPA and all the tools you need to build on it!</span></span>

<span data-ttu-id="1428d-112">Sık kullanılan havlu tek sayfa uygulama (SPA) ASP.NET ile oluşturmak için harika bir başlangıç noktası oluşturur.</span><span class="sxs-lookup"><span data-stu-id="1428d-112">Hot Towel creates a great starting point for building a Single Page Application (SPA) with ASP.NET.</span></span> <span data-ttu-id="1428d-113">Kutudan çıktığında, onu modüler yapı, kod, görünümü gezinti, veri bağlama, zengin veri yönetimi ve basit ancak Zarif stil sağlar.</span><span class="sxs-lookup"><span data-stu-id="1428d-113">Out of the box you it provides a modular structure for your code, view navigation, data binding, rich data management and simple but elegant styling.</span></span> <span data-ttu-id="1428d-114">Sık kullanılan havlu uygulamanıza, tesisat odaklanabilirsiniz bir SPA oluşturmak için gereken her şeyi sağlar.</span><span class="sxs-lookup"><span data-stu-id="1428d-114">Hot Towel provides everything you need to build a SPA, so you can focus on your app, not the plumbing.</span></span>

> <span data-ttu-id="1428d-115">Gelen bir SPA oluşturma hakkında daha fazla bilgi [John Papa'nın videoları, eğitim ve Pluralsight kurslar](http://johnpapa.net/spa?vsix).</span><span class="sxs-lookup"><span data-stu-id="1428d-115">Learn more about building a SPA from [John Papa's videos, tutorials and Pluralsight courses](http://johnpapa.net/spa?vsix).</span></span>


## <a name="application-structure"></a><span data-ttu-id="1428d-116">Uygulama yapısı</span><span class="sxs-lookup"><span data-stu-id="1428d-116">Application Structure</span></span>

<span data-ttu-id="1428d-117">Sık kullanılan havlu SPA uygulamanızı tanımlamak JavaScript ve HTML dosyaları içeren bir uygulama klasörü sağlar.</span><span class="sxs-lookup"><span data-stu-id="1428d-117">Hot Towel SPA provides an App folder which contains the JavaScript and HTML files that define your application.</span></span>

<span data-ttu-id="1428d-118">Uygulama klasörü içinde:</span><span class="sxs-lookup"><span data-stu-id="1428d-118">Inside the App folder:</span></span>

- <span data-ttu-id="1428d-119">Durandal</span><span class="sxs-lookup"><span data-stu-id="1428d-119">durandal</span></span>
- <span data-ttu-id="1428d-120">hizmetler</span><span class="sxs-lookup"><span data-stu-id="1428d-120">services</span></span>
- <span data-ttu-id="1428d-121">viewmodels</span><span class="sxs-lookup"><span data-stu-id="1428d-121">viewmodels</span></span>
- <span data-ttu-id="1428d-122">görünümler</span><span class="sxs-lookup"><span data-stu-id="1428d-122">views</span></span>

<span data-ttu-id="1428d-123">Uygulama klasörünün modülleri koleksiyonunu içerir.</span><span class="sxs-lookup"><span data-stu-id="1428d-123">The App folder contains a collection of modules.</span></span> <span data-ttu-id="1428d-124">Bu modüller işlevleri kapsülleyen ve diğer modüller üzerinde bağımlılıkları bildirin.</span><span class="sxs-lookup"><span data-stu-id="1428d-124">These modules encapsulate functionality and declare dependencies on other modules.</span></span> <span data-ttu-id="1428d-125">Görünümler klasöründe, uygulamanız için HTML ve viewmodels klasör görünümleri (ortak MVVM deseni) sunu mantığını içerir.</span><span class="sxs-lookup"><span data-stu-id="1428d-125">The views folder contains the HTML for your application and the viewmodels folder contains the presentation logic for the views (a common MVVM pattern).</span></span> <span data-ttu-id="1428d-126">Hizmetleri klasörü, uygulamanızın HTTP veri alma veya yerel depolama etkileşim gibi gerekebilir tüm ortak Hizmetleri barındırmak için idealdir.</span><span class="sxs-lookup"><span data-stu-id="1428d-126">The services folder is ideal for housing any common services that your application may need such as HTTP data retrieval or local storage interaction.</span></span> <span data-ttu-id="1428d-127">Hizmet modülleri koddan yeniden kullanmak için birden çok viewmodels için yaygın bir sorundur.</span><span class="sxs-lookup"><span data-stu-id="1428d-127">It is common for multiple viewmodels to re-use code from the service modules.</span></span>

## <a name="aspnet-mvc-server-side-application-structure"></a><span data-ttu-id="1428d-128">ASP.NET MVC sunucu tarafı uygulama yapısı</span><span class="sxs-lookup"><span data-stu-id="1428d-128">ASP.NET MVC Server Side Application Structure</span></span>

<span data-ttu-id="1428d-129">Sık kullanılan havlu tanıdık ve güçlü ASP.NET MVC yapısını oluşturur.</span><span class="sxs-lookup"><span data-stu-id="1428d-129">Hot Towel builds on the familiar and powerful ASP.NET MVC structure.</span></span>

- <span data-ttu-id="1428d-130">Uygulama\_Başlat</span><span class="sxs-lookup"><span data-stu-id="1428d-130">App\_Start</span></span>
- <span data-ttu-id="1428d-131">İçerik</span><span class="sxs-lookup"><span data-stu-id="1428d-131">Content</span></span>
- <span data-ttu-id="1428d-132">Denetleyiciler</span><span class="sxs-lookup"><span data-stu-id="1428d-132">Controllers</span></span>
- <span data-ttu-id="1428d-133">Modeller</span><span class="sxs-lookup"><span data-stu-id="1428d-133">Models</span></span>
- <span data-ttu-id="1428d-134">Komut dosyaları</span><span class="sxs-lookup"><span data-stu-id="1428d-134">Scripts</span></span>
- <span data-ttu-id="1428d-135">Görünümler</span><span class="sxs-lookup"><span data-stu-id="1428d-135">Views</span></span>

## <a name="featured-libraries"></a><span data-ttu-id="1428d-136">Öne çıkan kitaplıkları</span><span class="sxs-lookup"><span data-stu-id="1428d-136">Featured Libraries</span></span>

- <span data-ttu-id="1428d-137">ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="1428d-137">ASP.NET MVC</span></span>
- <span data-ttu-id="1428d-138">ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="1428d-138">ASP.NET Web API</span></span>
- <span data-ttu-id="1428d-139">ASP.NET Web iyileştirme - paketleme ve küçültme</span><span class="sxs-lookup"><span data-stu-id="1428d-139">ASP.NET Web Optimization - bundling and minification</span></span>
- <span data-ttu-id="1428d-140">[BREEZE.js](http://Breezejs.com) -zengin veri yönetimi</span><span class="sxs-lookup"><span data-stu-id="1428d-140">[Breeze.js](http://Breezejs.com) - rich data management</span></span>
- <span data-ttu-id="1428d-141">[Durandal.js](http://Durandaljs.com) -gezinti ve görünüm oluşturma</span><span class="sxs-lookup"><span data-stu-id="1428d-141">[Durandal.js](http://Durandaljs.com) - navigation and View composition</span></span>
- <span data-ttu-id="1428d-142">[Knockout.js](http://Knockoutjs.com) -veri bağlamaları</span><span class="sxs-lookup"><span data-stu-id="1428d-142">[Knockout.js](http://Knockoutjs.com) - data bindings</span></span>
- <span data-ttu-id="1428d-143">[Require.js](http://requirejs.org) -modülerlik AMD ve en iyi duruma getirme</span><span class="sxs-lookup"><span data-stu-id="1428d-143">[Require.js](http://requirejs.org) - Modularity with AMD and optimization</span></span>
- <span data-ttu-id="1428d-144">[Toastr.js](http://jpapa.me/c7toastr) -açılan iletiler</span><span class="sxs-lookup"><span data-stu-id="1428d-144">[Toastr.js](http://jpapa.me/c7toastr) - pop-up messages</span></span>
- <span data-ttu-id="1428d-145">[Önyükleme twitter](http://twitter.github.com/bootstrap/) - sağlam CSS stil oluşturma</span><span class="sxs-lookup"><span data-stu-id="1428d-145">[Twitter Bootstrap](http://twitter.github.com/bootstrap/) - robust CSS styling</span></span>

## <a name="installing-via-the-visual-studio-2012-hot-towel-spa-template"></a><span data-ttu-id="1428d-146">Visual Studio 2012 etkin havlu SPA şablonu yükleme</span><span class="sxs-lookup"><span data-stu-id="1428d-146">Installing via the Visual Studio 2012 Hot Towel SPA Template</span></span>

<span data-ttu-id="1428d-147">Sık kullanılan havlu Visual Studio 2012 şablon olarak yüklenebilir.</span><span class="sxs-lookup"><span data-stu-id="1428d-147">Hot Towel can be installed as a Visual Studio 2012 template.</span></span> <span data-ttu-id="1428d-148">Tıklatmanız `File`  |  `New Project` ve `ASP.NET MVC 4 Web Application`.</span><span class="sxs-lookup"><span data-stu-id="1428d-148">Just click `File` | `New Project` and choose `ASP.NET MVC 4 Web Application`.</span></span> <span data-ttu-id="1428d-149">Ardından ' etkin havlu tek sayfa uygulaması "şablonu ve çalıştırın!</span><span class="sxs-lookup"><span data-stu-id="1428d-149">Then select the 'Hot Towel Single Page Application" template and run!</span></span>

## <a name="installing-via-the-nuget-package"></a><span data-ttu-id="1428d-150">NuGet paketi yükleniyor</span><span class="sxs-lookup"><span data-stu-id="1428d-150">Installing via the NuGet Package</span></span>

<span data-ttu-id="1428d-151">Sık kullanılan havlu de varolan boş bir ASP.NET MVC projesini güçlendirir bir NuGet paketidir.</span><span class="sxs-lookup"><span data-stu-id="1428d-151">Hot Towel is also a NuGet package that augments an existing empty ASP.NET MVC project.</span></span> <span data-ttu-id="1428d-152">Yalnızca Nuget kullanarak yükleyin ve ardından çalıştırın!</span><span class="sxs-lookup"><span data-stu-id="1428d-152">Just install using Nuget and then run!</span></span>

[!code-powershell[Main](hottowel-template/samples/sample1.ps1)]

## <a name="how-do-i-build-on-hot-towel"></a><span data-ttu-id="1428d-153">Etkin havlu nasıl oluşturulacağına?</span><span class="sxs-lookup"><span data-stu-id="1428d-153">How Do I Build On Hot Towel?</span></span>

<span data-ttu-id="1428d-154">Yalnızca kod eklemeye başlayın!</span><span class="sxs-lookup"><span data-stu-id="1428d-154">Simply start adding code!</span></span>

1. <span data-ttu-id="1428d-155">Kendi sunucu tarafı kodu, tercihen Entity Framework ve Webapı (gerçekten Breeze.js ile görünmek) ekleyin</span><span class="sxs-lookup"><span data-stu-id="1428d-155">Add your own server-side code, preferably Entity Framework and WebAPI (which really shine with Breeze.js)</span></span>
2. <span data-ttu-id="1428d-156">Görünümlerine ekleme `App/views` klasörü</span><span class="sxs-lookup"><span data-stu-id="1428d-156">Add views to the `App/views` folder</span></span>
3. <span data-ttu-id="1428d-157">Viewmodels için ekleme `App/viewmodels` klasörü</span><span class="sxs-lookup"><span data-stu-id="1428d-157">Add viewmodels to the `App/viewmodels` folder</span></span>
4. <span data-ttu-id="1428d-158">HTML ve Boşaltılan veri bağlamaları yeni görünümlerinize ekleme</span><span class="sxs-lookup"><span data-stu-id="1428d-158">Add HTML and Knockout data bindings to your new views</span></span>
5. <span data-ttu-id="1428d-159">Gezinti yollar güncelleştir`shell.js`</span><span class="sxs-lookup"><span data-stu-id="1428d-159">Update the navigation routes in `shell.js`</span></span>

## <a name="walkthrough-of-the-htmljavascript"></a><span data-ttu-id="1428d-160">HTML/JavaScript gözden geçirme</span><span class="sxs-lookup"><span data-stu-id="1428d-160">Walkthrough of the HTML/JavaScript</span></span>

### <a name="viewshottowelindexcshtml"></a><span data-ttu-id="1428d-161">Views/HotTowel/index.cshtml</span><span class="sxs-lookup"><span data-stu-id="1428d-161">Views/HotTowel/index.cshtml</span></span>

<span data-ttu-id="1428d-162">Index.cshtml başlangıç rota ve görünüm MVC uygulaması için ' dir.</span><span class="sxs-lookup"><span data-stu-id="1428d-162">index.cshtml is the starting route and view for the MVC application.</span></span> <span data-ttu-id="1428d-163">Tüm standart meta etiketleri, css bağlantılar ve beklediğiniz JavaScript başvurular içerir.</span><span class="sxs-lookup"><span data-stu-id="1428d-163">It contains all the standard meta tags, css links, and JavaScript references you would expect.</span></span> <span data-ttu-id="1428d-164">Tek bir gövdesinde `<div>` istendiklerinde tüm içeriği (görünümlerinizi) yerleştirileceği olduğu.</span><span class="sxs-lookup"><span data-stu-id="1428d-164">The body contains a single `<div>` which is where all of the content (your views) will be placed when they are requested.</span></span> <span data-ttu-id="1428d-165">`@Scripts.Render` Require.js bulunan uygulama kodunun için giriş noktası çalıştırmak için kullandığı `main.js` dosya.</span><span class="sxs-lookup"><span data-stu-id="1428d-165">The `@Scripts.Render` uses Require.js to run the entrance point for the application's code, which is contained in the `main.js` file.</span></span> <span data-ttu-id="1428d-166">Karşılama ekranı uygulamanızı yüklenirken giriş ekranı oluşturma göstermek için sağlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="1428d-166">A splash screen is provided to demonstrate how to create a splash screen while your app loads.</span></span>

[!code-cshtml[Main](hottowel-template/samples/sample2.cshtml)]

### <a name="appmainjs"></a><span data-ttu-id="1428d-167">App/Main.js</span><span class="sxs-lookup"><span data-stu-id="1428d-167">App/main.js</span></span>

<span data-ttu-id="1428d-168">`main.js` Dosyası uygulamanızı yüklendikten hemen sonra çalıştırılacak kodu içerir.</span><span class="sxs-lookup"><span data-stu-id="1428d-168">The `main.js` file contains the code that will run as soon as your app is loaded.</span></span> <span data-ttu-id="1428d-169">Gezinti yollarınızı tanımlamak, görünümler, başlangıcı Ayarla ve tüm Kurulum/uygulamanızın veri hazırlama gibi önyükleme gerçekleştirmek istediğiniz budur.</span><span class="sxs-lookup"><span data-stu-id="1428d-169">This is where you want to define your navigation routes, set your start up views, and perform any setup/bootstrapping such as priming your application's data.</span></span>

<span data-ttu-id="1428d-170">`main.js` Dosyası birkaç başlangıç uygulaması başlangıç yardımcı olmak için durandal'ın modülleri tanımlar.</span><span class="sxs-lookup"><span data-stu-id="1428d-170">The `main.js` file defines several of durandal's modules to help the application kick start.</span></span> <span data-ttu-id="1428d-171">Tanımla deyimi işlevi için kullanılabilir olmaları modülleri bağımlılıkları çözmenize yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="1428d-171">The define statement helps resolve the modules dependencies so they are available for the function.</span></span> <span data-ttu-id="1428d-172">İlk hata ayıklama iletilerini uygulama konsol penceresine gerçekleştiriyor iletileri hangi olayların hakkında göndermesi etkinleştirilir.</span><span class="sxs-lookup"><span data-stu-id="1428d-172">First the debugging messages are enabled, which send messages about what events the application is performing to the console window.</span></span> <span data-ttu-id="1428d-173">App.start kod uygulamayı başlatmak için durandal framework bildirir.</span><span class="sxs-lookup"><span data-stu-id="1428d-173">The app.start code tells durandal framework to start the application.</span></span> <span data-ttu-id="1428d-174">Kuralları, böylece tüm görünümleri durandal bilir ve viewmodels sırasıyla aynı adlandırılmış klasörlerde bulunan ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="1428d-174">The conventions are set so that durandal knows all views and viewmodels are contained in the same named folders, respectively.</span></span> <span data-ttu-id="1428d-175">Son olarak, `app.setRoot` yükleri devreye `shell` önceden tanımlanmış bir kullanarak `entrance` animasyon.</span><span class="sxs-lookup"><span data-stu-id="1428d-175">Finally, the `app.setRoot` kicks loads the `shell` using a predefined `entrance` animation.</span></span>

[!code-javascript[Main](hottowel-template/samples/sample3.js)]

## <a name="views"></a><span data-ttu-id="1428d-176">Görünümler</span><span class="sxs-lookup"><span data-stu-id="1428d-176">Views</span></span>

<span data-ttu-id="1428d-177">Görünümler içinde bulunan `App/views` klasör.</span><span class="sxs-lookup"><span data-stu-id="1428d-177">Views are found in the `App/views` folder.</span></span>

### <a name="shellhtml"></a><span data-ttu-id="1428d-178">Shell.HTML</span><span class="sxs-lookup"><span data-stu-id="1428d-178">shell.html</span></span>

<span data-ttu-id="1428d-179">`shell.html` İçin HTML ana düzeni içerir.</span><span class="sxs-lookup"><span data-stu-id="1428d-179">The `shell.html` contains the master layout for your HTML.</span></span> <span data-ttu-id="1428d-180">Tüm diğer görünümlerinizi tarafında içinde herhangi bir yerde oluşacaktır, `shell` görünümü.</span><span class="sxs-lookup"><span data-stu-id="1428d-180">All of your other views will be composed somewhere in side of your `shell` view.</span></span> <span data-ttu-id="1428d-181">Sık kullanılan havlu sağlar bir `shell` üç tür bölgeleriyle: üst bilgi, içerik alanının ve altbilgi.</span><span class="sxs-lookup"><span data-stu-id="1428d-181">Hot Towel provides a `shell` with three such regions: a header, a content area, and a footer.</span></span> <span data-ttu-id="1428d-182">Bu bölgeler her ile yüklenen içeriği istendiğinde diğer görünümlere form.</span><span class="sxs-lookup"><span data-stu-id="1428d-182">Each of these regions is loaded with contents form other views when requested.</span></span>

<span data-ttu-id="1428d-183">`compose` Üstbilgi ve altbilgi için bağlamaları sabit işaret etmek için sık kullanılan havlu kodlanmış `nav` ve `footer` sırasıyla görüntüler.</span><span class="sxs-lookup"><span data-stu-id="1428d-183">The `compose` bindings for the header and footer are hard coded in Hot Towel to point to the `nav` and `footer` views, respectively.</span></span> <span data-ttu-id="1428d-184">Bölümü için oluşturma bağlama `#content` bağlı `router` modülünün etkin öğesi.</span><span class="sxs-lookup"><span data-stu-id="1428d-184">The compose binding for the section `#content` is bound to the `router` module's active item.</span></span> <span data-ttu-id="1428d-185">Diğer bir deyişle, tıkladığınızda bu alana karşılık gelen görünümdür bir gezinti bağlantısı yüklenir.</span><span class="sxs-lookup"><span data-stu-id="1428d-185">In other words, when you click a navigation link it's corresponding view is loaded in this area.</span></span>

[!code-html[Main](hottowel-template/samples/sample4.html)]

### <a name="navhtml"></a><span data-ttu-id="1428d-186">NAV.HTML</span><span class="sxs-lookup"><span data-stu-id="1428d-186">nav.html</span></span>

<span data-ttu-id="1428d-187">`nav.html` SPA Gezinti bağlantıları içeriyor.</span><span class="sxs-lookup"><span data-stu-id="1428d-187">The `nav.html` contains the navigation links for the SPA.</span></span> <span data-ttu-id="1428d-188">Bu, burada menü yapısı, örneğin yerleştirilebilir değildir.</span><span class="sxs-lookup"><span data-stu-id="1428d-188">This is where the menu structure can be placed, for example.</span></span> <span data-ttu-id="1428d-189">Bu veri bağlama (Boşaltılan kullanarak) için görülür `router` , tanımladığınız Gezinti görüntülemek için Modülü `shell.js`.</span><span class="sxs-lookup"><span data-stu-id="1428d-189">Often this is data bound (using Knockout) to the `router` module to display the navigation you defined in the `shell.js`.</span></span> <span data-ttu-id="1428d-190">Boşaltılan veri bağlama öznitelikleri ve onlara bağlar arar `shell` viewmodel Gezinti yolları görüntüler ve (Twitter Bootstrap kullanarak) progressbar göstermek için `router` modülüdür'ın başka bir (görmek bir görünümden gezinme ortasında `router.isNavigating`).</span><span class="sxs-lookup"><span data-stu-id="1428d-190">Knockout looks for the data-bind attributes and binds those to the `shell` viewmodel to display the navigation routes and to show a progressbar (using Twitter Bootstrap) if the `router` module is in the middle of navigating from one view to another (see `router.isNavigating`).</span></span>

[!code-html[Main](hottowel-template/samples/sample5.html)]

### <a name="homehtml-and-detailshtml"></a><span data-ttu-id="1428d-191">Home.HTML ve details.html</span><span class="sxs-lookup"><span data-stu-id="1428d-191">home.html and details.html</span></span>

<span data-ttu-id="1428d-192">Bu görünümler HTML için özel görünümleri içerir.</span><span class="sxs-lookup"><span data-stu-id="1428d-192">These views contain HTML for custom views.</span></span> <span data-ttu-id="1428d-193">Zaman `home` bağlamak `nav` görünümün menü tıklandığında, `home` görünümü içerik alanında yerleştirilecek `shell` görünümü.</span><span class="sxs-lookup"><span data-stu-id="1428d-193">When the `home` link in the `nav` view's menu is clicked, the `home` view will be placed in the content area of the `shell` view.</span></span> <span data-ttu-id="1428d-194">Bu görünümler, genişletilebilir veya kendi özel görünümlerinizi ile değiştirilir.</span><span class="sxs-lookup"><span data-stu-id="1428d-194">These views can be augmented or replaced with your own custom views.</span></span>

### <a name="footerhtml"></a><span data-ttu-id="1428d-195">Footer.HTML</span><span class="sxs-lookup"><span data-stu-id="1428d-195">footer.html</span></span>

<span data-ttu-id="1428d-196">`footer.html` İçeren alt kısmındaki altbilgisindeki görüntülenen HTML `shell` görünümü.</span><span class="sxs-lookup"><span data-stu-id="1428d-196">The `footer.html` contains HTML that appears in the footer, at the bottom of the `shell` view.</span></span>

## <a name="viewmodels"></a><span data-ttu-id="1428d-197">ViewModels</span><span class="sxs-lookup"><span data-stu-id="1428d-197">ViewModels</span></span>

<span data-ttu-id="1428d-198">İçinde bulunan ViewModels `App/viewmodels` klasör.</span><span class="sxs-lookup"><span data-stu-id="1428d-198">ViewModels are found in the `App/viewmodels` folder.</span></span>

### <a name="shelljs"></a><span data-ttu-id="1428d-199">Shell.js</span><span class="sxs-lookup"><span data-stu-id="1428d-199">shell.js</span></span>

<span data-ttu-id="1428d-200">`shell` Viewmodel içermektedir özelliklerini ve bağlı olan işlevler `shell` görünümü.</span><span class="sxs-lookup"><span data-stu-id="1428d-200">The `shell` viewmodel contains properties and functions that are bound to the `shell` view.</span></span> <span data-ttu-id="1428d-201">Bu menü Gezinti bağlamaları bulunduğu görülür (bkz `router.mapNav` mantığı).</span><span class="sxs-lookup"><span data-stu-id="1428d-201">Often this is where the menu navigation bindings are found (see the `router.mapNav` logic).</span></span>

[!code-javascript[Main](hottowel-template/samples/sample6.js)]

### <a name="homejs-and-detailsjs"></a><span data-ttu-id="1428d-202">Home.js ve details.js</span><span class="sxs-lookup"><span data-stu-id="1428d-202">home.js and details.js</span></span>

<span data-ttu-id="1428d-203">Bu viewmodels bağlı olan işlevler ve Özellikler içeren `home` görünümü.</span><span class="sxs-lookup"><span data-stu-id="1428d-203">These viewmodels contain the properties and functions that are bound to the `home` view.</span></span> <span data-ttu-id="1428d-204">Ayrıca görünümün sunu mantığını içerir ve veri ve görünüm arasında bir Yapıştırıcı işlevi görür.</span><span class="sxs-lookup"><span data-stu-id="1428d-204">it also contains the presentation logic for the view, and is the glue between the data and the view.</span></span>

[!code-javascript[Main](hottowel-template/samples/sample7.js)]

## <a name="services"></a><span data-ttu-id="1428d-205">Hizmetler</span><span class="sxs-lookup"><span data-stu-id="1428d-205">Services</span></span>

<span data-ttu-id="1428d-206">Hizmetleri uygulama/hizmetleri klasöründe bulunur.</span><span class="sxs-lookup"><span data-stu-id="1428d-206">Services are found in the App/services folder.</span></span> <span data-ttu-id="1428d-207">İdeal olarak, gelecekteki hizmetlerinizi uzak veri gönderme ve alma için sorumlu olan dataservice modülü gibi yerleştirilemiyor.</span><span class="sxs-lookup"><span data-stu-id="1428d-207">Ideally your future services such as a dataservice module, that is responsible for getting and posting remote data, could be placed.</span></span>

### <a name="loggerjs"></a><span data-ttu-id="1428d-208">Logger.js</span><span class="sxs-lookup"><span data-stu-id="1428d-208">logger.js</span></span>

<span data-ttu-id="1428d-209">Sık kullanılan havlu sağlayan bir `logger` Hizmetleri klasörü modülünde.</span><span class="sxs-lookup"><span data-stu-id="1428d-209">Hot Towel provides a `logger` module in the services folder.</span></span> <span data-ttu-id="1428d-210">`logger` Modülüdür günlük iletilerini konsola ve bildirimleri açılır kullanıcıya için idealdir.</span><span class="sxs-lookup"><span data-stu-id="1428d-210">The `logger` module is ideal for logging messages to the console and to the user in pop up toasts.</span></span>
