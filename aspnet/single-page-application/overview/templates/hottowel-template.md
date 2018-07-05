---
uid: single-page-application/overview/templates/hottowel-template
title: Hot Towel şablonu | Microsoft Docs
author: madskristensen
description: HotTowel şablonu
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/09/2013
ms.topic: article
ms.assetid: 75af2e17-6ed3-4d24-8ea1-bc340027c318
ms.technology: ''
msc.legacyurl: /single-page-application/overview/templates/hottowel-template
msc.type: authoredcontent
ms.openlocfilehash: de81f12f57d7f2fb7c6478bfa1f3a278ae905a39
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388558"
---
<a name="hot-towel-template"></a><span data-ttu-id="804d0-103">Hot Towel şablonu</span><span class="sxs-lookup"><span data-stu-id="804d0-103">Hot Towel template</span></span>
====================
<span data-ttu-id="804d0-104">tarafından [Mads Kristensen](https://github.com/madskristensen)</span><span class="sxs-lookup"><span data-stu-id="804d0-104">by [Mads Kristensen](https://github.com/madskristensen)</span></span>

> <span data-ttu-id="804d0-105">Hot Towel MVC şablonu tarafından John Papa yazılır</span><span class="sxs-lookup"><span data-stu-id="804d0-105">The Hot Towel MVC Template is written by John Papa</span></span>
> 
> <span data-ttu-id="804d0-106">İndirmek için hangi sürümünü seçin:</span><span class="sxs-lookup"><span data-stu-id="804d0-106">Choose which version to download:</span></span>
> 
> [<span data-ttu-id="804d0-107">Visual Studio 2012 için Hot Towel MVC şablonu</span><span class="sxs-lookup"><span data-stu-id="804d0-107">Hot Towel MVC Template for Visual Studio 2012</span></span>](https://visualstudiogallery.msdn.microsoft.com/1f68fbe8-b4e9-4968-9fd3-ddc7cbc52dca)
> 
> [<span data-ttu-id="804d0-108">Visual Studio 2013 için Hot Towel MVC şablonu</span><span class="sxs-lookup"><span data-stu-id="804d0-108">Hot Towel MVC Template for Visual Studio 2013</span></span>](https://visualstudiogallery.msdn.microsoft.com/1eb8780d-d522-4dcf-bf56-56f0eab305c2)
> 
> 
> <span data-ttu-id="804d0-109">Hot Towel: olmadan bir SPA gitmek istemediğinden!</span><span class="sxs-lookup"><span data-stu-id="804d0-109">Hot Towel: Because you don't want to go to the SPA without one!</span></span>


<span data-ttu-id="804d0-110">Nereden başlayacağınızı karar veremez ancak bir SPA oluşturmak istiyorsunuz?</span><span class="sxs-lookup"><span data-stu-id="804d0-110">Want to build a SPA but can't decide where to start?</span></span> <span data-ttu-id="804d0-111">Hot Towel kullanın ve saniyeler içinde bir SPA ve üzerinde oluşturmak için ihtiyacınız olan tüm araçları gerekir!</span><span class="sxs-lookup"><span data-stu-id="804d0-111">Use Hot Towel and in seconds you'll have a SPA and all the tools you need to build on it!</span></span>

<span data-ttu-id="804d0-112">Hot Towel tek sayfa uygulama (SPA) ASP.NET ile oluşturmak için harika bir başlangıç noktası oluşturur.</span><span class="sxs-lookup"><span data-stu-id="804d0-112">Hot Towel creates a great starting point for building a Single Page Application (SPA) with ASP.NET.</span></span> <span data-ttu-id="804d0-113">Kullanıma hazır, bunu bir modüler yapı, kod, görünümü gezinti, veri bağlama, zengin veri yönetimini ve basit ama Zarif stil sağlar.</span><span class="sxs-lookup"><span data-stu-id="804d0-113">Out of the box you it provides a modular structure for your code, view navigation, data binding, rich data management and simple but elegant styling.</span></span> <span data-ttu-id="804d0-114">Hot Towel uygulamanıza tesisat odaklanmanızı sağlayacak bir SPA oluşturmak için ihtiyacınız olan her şeyi sağlar.</span><span class="sxs-lookup"><span data-stu-id="804d0-114">Hot Towel provides everything you need to build a SPA, so you can focus on your app, not the plumbing.</span></span>

> <span data-ttu-id="804d0-115">Gelen bir SPA oluşturma hakkında daha fazla bilgi [John Papa'nın videolar, öğreticiler ve Pluralsight kurslarına](http://johnpapa.net/spa?vsix).</span><span class="sxs-lookup"><span data-stu-id="804d0-115">Learn more about building a SPA from [John Papa's videos, tutorials and Pluralsight courses](http://johnpapa.net/spa?vsix).</span></span>


## <a name="application-structure"></a><span data-ttu-id="804d0-116">Uygulama yapısı</span><span class="sxs-lookup"><span data-stu-id="804d0-116">Application Structure</span></span>

<span data-ttu-id="804d0-117">Hot Towel SPA uygulamanızı tanımlayan JavaScript ve HTML dosyaları içeren bir uygulama klasörü sağlar.</span><span class="sxs-lookup"><span data-stu-id="804d0-117">Hot Towel SPA provides an App folder which contains the JavaScript and HTML files that define your application.</span></span>

<span data-ttu-id="804d0-118">İçinde uygulama klasörü:</span><span class="sxs-lookup"><span data-stu-id="804d0-118">Inside the App folder:</span></span>

- <span data-ttu-id="804d0-119">durandal</span><span class="sxs-lookup"><span data-stu-id="804d0-119">durandal</span></span>
- <span data-ttu-id="804d0-120">hizmetler</span><span class="sxs-lookup"><span data-stu-id="804d0-120">services</span></span>
- <span data-ttu-id="804d0-121">viewmodel'lar</span><span class="sxs-lookup"><span data-stu-id="804d0-121">viewmodels</span></span>
- <span data-ttu-id="804d0-122">görünümler</span><span class="sxs-lookup"><span data-stu-id="804d0-122">views</span></span>

<span data-ttu-id="804d0-123">Uygulama klasör modülleri koleksiyonunu içerir.</span><span class="sxs-lookup"><span data-stu-id="804d0-123">The App folder contains a collection of modules.</span></span> <span data-ttu-id="804d0-124">Bu modüller işlevi kapsülleyen ve diğer modüllerin üzerinde bağımlılıkları bildirin.</span><span class="sxs-lookup"><span data-stu-id="804d0-124">These modules encapsulate functionality and declare dependencies on other modules.</span></span> <span data-ttu-id="804d0-125">Görünüm klasörü uygulamanız için bir HTML ve viewmodel'lar klasör görünümleri (ortak MVVM desen) sunu mantığını içerir.</span><span class="sxs-lookup"><span data-stu-id="804d0-125">The views folder contains the HTML for your application and the viewmodels folder contains the presentation logic for the views (a common MVVM pattern).</span></span> <span data-ttu-id="804d0-126">Hizmetleri klasörü, uygulamanızın HTTP veri alma veya yerel depolama etkileşimi gibi gerekebilir tüm ortak Hizmetleri barındırmak için idealdir.</span><span class="sxs-lookup"><span data-stu-id="804d0-126">The services folder is ideal for housing any common services that your application may need such as HTTP data retrieval or local storage interaction.</span></span> <span data-ttu-id="804d0-127">Hizmet modüllerden kod yeniden kullanımı için birden çok viewmodel'lar için yaygındır.</span><span class="sxs-lookup"><span data-stu-id="804d0-127">It is common for multiple viewmodels to re-use code from the service modules.</span></span>

## <a name="aspnet-mvc-server-side-application-structure"></a><span data-ttu-id="804d0-128">ASP.NET MVC sunucu tarafı uygulama yapısı</span><span class="sxs-lookup"><span data-stu-id="804d0-128">ASP.NET MVC Server Side Application Structure</span></span>

<span data-ttu-id="804d0-129">Hot Towel üzerinde tanıdık ve güçlü ASP.NET MVC yapısı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="804d0-129">Hot Towel builds on the familiar and powerful ASP.NET MVC structure.</span></span>

- <span data-ttu-id="804d0-130">Uygulama\_Başlat</span><span class="sxs-lookup"><span data-stu-id="804d0-130">App\_Start</span></span>
- <span data-ttu-id="804d0-131">İçerik</span><span class="sxs-lookup"><span data-stu-id="804d0-131">Content</span></span>
- <span data-ttu-id="804d0-132">Denetleyiciler</span><span class="sxs-lookup"><span data-stu-id="804d0-132">Controllers</span></span>
- <span data-ttu-id="804d0-133">Modeller</span><span class="sxs-lookup"><span data-stu-id="804d0-133">Models</span></span>
- <span data-ttu-id="804d0-134">Komut dosyaları</span><span class="sxs-lookup"><span data-stu-id="804d0-134">Scripts</span></span>
- <span data-ttu-id="804d0-135">Görünümler</span><span class="sxs-lookup"><span data-stu-id="804d0-135">Views</span></span>

## <a name="featured-libraries"></a><span data-ttu-id="804d0-136">Öne çıkan kitaplıkları</span><span class="sxs-lookup"><span data-stu-id="804d0-136">Featured Libraries</span></span>

- <span data-ttu-id="804d0-137">ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="804d0-137">ASP.NET MVC</span></span>
- <span data-ttu-id="804d0-138">ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="804d0-138">ASP.NET Web API</span></span>
- <span data-ttu-id="804d0-139">ASP.NET Web iyileştirme - paketleme ve küçültme</span><span class="sxs-lookup"><span data-stu-id="804d0-139">ASP.NET Web Optimization - bundling and minification</span></span>
- <span data-ttu-id="804d0-140">[BREEZE.js](http://Breezejs.com) -zengin veri yönetimi</span><span class="sxs-lookup"><span data-stu-id="804d0-140">[Breeze.js](http://Breezejs.com) - rich data management</span></span>
- <span data-ttu-id="804d0-141">[Durandal.js](http://Durandaljs.com) -gezinti ve görünümü oluşturma</span><span class="sxs-lookup"><span data-stu-id="804d0-141">[Durandal.js](http://Durandaljs.com) - navigation and View composition</span></span>
- <span data-ttu-id="804d0-142">[Knockout.js](http://Knockoutjs.com) -veri bağlamaları</span><span class="sxs-lookup"><span data-stu-id="804d0-142">[Knockout.js](http://Knockoutjs.com) - data bindings</span></span>
- <span data-ttu-id="804d0-143">[Require.js](http://requirejs.org) -modülerlik AMD ve en iyi duruma getirme</span><span class="sxs-lookup"><span data-stu-id="804d0-143">[Require.js](http://requirejs.org) - Modularity with AMD and optimization</span></span>
- <span data-ttu-id="804d0-144">[Toastr.js](http://jpapa.me/c7toastr) -açılan iletiler</span><span class="sxs-lookup"><span data-stu-id="804d0-144">[Toastr.js](http://jpapa.me/c7toastr) - pop-up messages</span></span>
- <span data-ttu-id="804d0-145">[Twitter Bootstrap](http://twitter.github.com/bootstrap/) - sağlam CSS stil oluşturma</span><span class="sxs-lookup"><span data-stu-id="804d0-145">[Twitter Bootstrap](http://twitter.github.com/bootstrap/) - robust CSS styling</span></span>

## <a name="installing-via-the-visual-studio-2012-hot-towel-spa-template"></a><span data-ttu-id="804d0-146">Visual Studio 2012 Hot Towel SPA şablonu yükleme</span><span class="sxs-lookup"><span data-stu-id="804d0-146">Installing via the Visual Studio 2012 Hot Towel SPA Template</span></span>

<span data-ttu-id="804d0-147">Hot Towel Visual Studio 2012 şablon olarak yüklenebilir.</span><span class="sxs-lookup"><span data-stu-id="804d0-147">Hot Towel can be installed as a Visual Studio 2012 template.</span></span> <span data-ttu-id="804d0-148">Tıklamanız yeterli `File`  |  `New Project` ve `ASP.NET MVC 4 Web Application`.</span><span class="sxs-lookup"><span data-stu-id="804d0-148">Just click `File` | `New Project` and choose `ASP.NET MVC 4 Web Application`.</span></span> <span data-ttu-id="804d0-149">Ardından ' Hot Towel tek sayfalı uygulama "şablonu ve çalıştırın!</span><span class="sxs-lookup"><span data-stu-id="804d0-149">Then select the 'Hot Towel Single Page Application" template and run!</span></span>

## <a name="installing-via-the-nuget-package"></a><span data-ttu-id="804d0-150">NuGet paketi yükleme</span><span class="sxs-lookup"><span data-stu-id="804d0-150">Installing via the NuGet Package</span></span>

<span data-ttu-id="804d0-151">Hot Towel Ayrıca var olan boş bir ASP.NET MVC projesi çoğaltan bir NuGet paketidir.</span><span class="sxs-lookup"><span data-stu-id="804d0-151">Hot Towel is also a NuGet package that augments an existing empty ASP.NET MVC project.</span></span> <span data-ttu-id="804d0-152">Nuget kullanarak yalnızca yükleyin ve ardından çalıştırın!</span><span class="sxs-lookup"><span data-stu-id="804d0-152">Just install using Nuget and then run!</span></span>

[!code-powershell[Main](hottowel-template/samples/sample1.ps1)]

## <a name="how-do-i-build-on-hot-towel"></a><span data-ttu-id="804d0-153">Hot Towel üzerinde nasıl oluşturabilirim?</span><span class="sxs-lookup"><span data-stu-id="804d0-153">How Do I Build On Hot Towel?</span></span>

<span data-ttu-id="804d0-154">Yalnızca kod ekleyerek başlayın!</span><span class="sxs-lookup"><span data-stu-id="804d0-154">Simply start adding code!</span></span>

1. <span data-ttu-id="804d0-155">Tercihen Entity Framework ve Webapı (gerçekten ile Breeze.js aydınlatmak), kendi sunucu tarafı kodu ekleyin</span><span class="sxs-lookup"><span data-stu-id="804d0-155">Add your own server-side code, preferably Entity Framework and WebAPI (which really shine with Breeze.js)</span></span>
2. <span data-ttu-id="804d0-156">Görünümlere ekleme `App/views` klasörü</span><span class="sxs-lookup"><span data-stu-id="804d0-156">Add views to the `App/views` folder</span></span>
3. <span data-ttu-id="804d0-157">Ekleme için viewmodel'lar `App/viewmodels` klasörü</span><span class="sxs-lookup"><span data-stu-id="804d0-157">Add viewmodels to the `App/viewmodels` folder</span></span>
4. <span data-ttu-id="804d0-158">HTML ve Boşaltılan veri bağlamaları yeni görünümlerinize ekleme</span><span class="sxs-lookup"><span data-stu-id="804d0-158">Add HTML and Knockout data bindings to your new views</span></span>
5. <span data-ttu-id="804d0-159">Gezinti yolları güncelleştir `shell.js`</span><span class="sxs-lookup"><span data-stu-id="804d0-159">Update the navigation routes in `shell.js`</span></span>

## <a name="walkthrough-of-the-htmljavascript"></a><span data-ttu-id="804d0-160">HTML/JavaScript gözden geçirme</span><span class="sxs-lookup"><span data-stu-id="804d0-160">Walkthrough of the HTML/JavaScript</span></span>

### <a name="viewshottowelindexcshtml"></a><span data-ttu-id="804d0-161">Views/HotTowel/index.cshtml</span><span class="sxs-lookup"><span data-stu-id="804d0-161">Views/HotTowel/index.cshtml</span></span>

<span data-ttu-id="804d0-162">Index.cshtml başlangıç rota ve MVC uygulaması için Görünüm ' dir.</span><span class="sxs-lookup"><span data-stu-id="804d0-162">index.cshtml is the starting route and view for the MVC application.</span></span> <span data-ttu-id="804d0-163">Bu, tüm standart meta etiketler, css bağlantılar ve beklediğiniz JavaScript başvurular içerir.</span><span class="sxs-lookup"><span data-stu-id="804d0-163">It contains all the standard meta tags, css links, and JavaScript references you would expect.</span></span> <span data-ttu-id="804d0-164">Tek bir gövde içeren `<div>` istendiklerinde tüm içeriği (görünümlerinizde) yerleştirileceği olduğu.</span><span class="sxs-lookup"><span data-stu-id="804d0-164">The body contains a single `<div>` which is where all of the content (your views) will be placed when they are requested.</span></span> <span data-ttu-id="804d0-165">`@Scripts.Render` Require.js giriş noktası bulunan uygulama kodunu çalıştırmak için kullandığı `main.js` dosya.</span><span class="sxs-lookup"><span data-stu-id="804d0-165">The `@Scripts.Render` uses Require.js to run the entrance point for the application's code, which is contained in the `main.js` file.</span></span> <span data-ttu-id="804d0-166">Giriş ekranı, uygulamanızı yüklenirken giriş ekranı oluşturma işlemini göstermek için sağlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="804d0-166">A splash screen is provided to demonstrate how to create a splash screen while your app loads.</span></span>

[!code-cshtml[Main](hottowel-template/samples/sample2.cshtml)]

### <a name="appmainjs"></a><span data-ttu-id="804d0-167">App/Main.js</span><span class="sxs-lookup"><span data-stu-id="804d0-167">App/main.js</span></span>

<span data-ttu-id="804d0-168">`main.js` Dosya, uygulama yüklendikten hemen sonra çalıştırılacak kodunu içerir.</span><span class="sxs-lookup"><span data-stu-id="804d0-168">The `main.js` file contains the code that will run as soon as your app is loaded.</span></span> <span data-ttu-id="804d0-169">Gezinti yollarınızı tanımlamak, görünümler, başlangıç ayarlayın ve tüm Kurulum/uygulamanızın verilerini hazırlama gibi önyüklemesi gerçekleştirmek istediğiniz budur.</span><span class="sxs-lookup"><span data-stu-id="804d0-169">This is where you want to define your navigation routes, set your start up views, and perform any setup/bootstrapping such as priming your application's data.</span></span>

<span data-ttu-id="804d0-170">`main.js` Dosyası durandal'ın modüllerinin Başlat uygulama clark'i yardımcı olmak için birkaç tanımlar.</span><span class="sxs-lookup"><span data-stu-id="804d0-170">The `main.js` file defines several of durandal's modules to help the application kick start.</span></span> <span data-ttu-id="804d0-171">Tanımlama bildirimi işlevi kullanabilmesi için modülleri bağımlılıklarını Çözümle yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="804d0-171">The define statement helps resolve the modules dependencies so they are available for the function.</span></span> <span data-ttu-id="804d0-172">İlk hata ayıklama iletilerini hangi olayların konsol penceresinde uygulamanın gerçekleştirdiği ileti göndermesi etkinleştirilir.</span><span class="sxs-lookup"><span data-stu-id="804d0-172">First the debugging messages are enabled, which send messages about what events the application is performing to the console window.</span></span> <span data-ttu-id="804d0-173">Uygulamayı başlatmak için durandal framework app.start kodu bildirir.</span><span class="sxs-lookup"><span data-stu-id="804d0-173">The app.start code tells durandal framework to start the application.</span></span> <span data-ttu-id="804d0-174">Kuralları, böylece tüm görünümlere durandal bilir ve viewmodel'lar sırasıyla aynı adlandırılmış klasörlerde bulunan ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="804d0-174">The conventions are set so that durandal knows all views and viewmodels are contained in the same named folders, respectively.</span></span> <span data-ttu-id="804d0-175">Son olarak, `app.setRoot` yükleri devreye `shell` önceden tanımlanmış bir şablon kullanarak `entrance` animasyon.</span><span class="sxs-lookup"><span data-stu-id="804d0-175">Finally, the `app.setRoot` kicks loads the `shell` using a predefined `entrance` animation.</span></span>

[!code-javascript[Main](hottowel-template/samples/sample3.js)]

## <a name="views"></a><span data-ttu-id="804d0-176">Görünümler</span><span class="sxs-lookup"><span data-stu-id="804d0-176">Views</span></span>

<span data-ttu-id="804d0-177">Görünümler içinde bulunan `App/views` klasör.</span><span class="sxs-lookup"><span data-stu-id="804d0-177">Views are found in the `App/views` folder.</span></span>

### <a name="shellhtml"></a><span data-ttu-id="804d0-178">Shell.HTML</span><span class="sxs-lookup"><span data-stu-id="804d0-178">shell.html</span></span>

<span data-ttu-id="804d0-179">`shell.html` Ana düzen için HTML içerir.</span><span class="sxs-lookup"><span data-stu-id="804d0-179">The `shell.html` contains the master layout for your HTML.</span></span> <span data-ttu-id="804d0-180">Tüm diğer görünümlerinizde yere in tarafında oluşacaktır, `shell` görünümü.</span><span class="sxs-lookup"><span data-stu-id="804d0-180">All of your other views will be composed somewhere in side of your `shell` view.</span></span> <span data-ttu-id="804d0-181">Hot Towel sağlayan bir `shell` üç bölgeleriyle: üst bilgi, bir içerik alanı ve alt bilgi.</span><span class="sxs-lookup"><span data-stu-id="804d0-181">Hot Towel provides a `shell` with three such regions: a header, a content area, and a footer.</span></span> <span data-ttu-id="804d0-182">Her biri bu bölgeler ile yüklenen içeriği form istendiğinde diğer görünümleri.</span><span class="sxs-lookup"><span data-stu-id="804d0-182">Each of these regions is loaded with contents form other views when requested.</span></span>

<span data-ttu-id="804d0-183">`compose` Üstbilgi ve altbilgi bağlamalarda sabit işaret edecek şekilde Hot Towel içinde kodlanmış `nav` ve `footer` görünümleri, sırasıyla.</span><span class="sxs-lookup"><span data-stu-id="804d0-183">The `compose` bindings for the header and footer are hard coded in Hot Towel to point to the `nav` and `footer` views, respectively.</span></span> <span data-ttu-id="804d0-184">Bölüm için oluşturma bağlama `#content` bağlı `router` modülün etkin öğeyi.</span><span class="sxs-lookup"><span data-stu-id="804d0-184">The compose binding for the section `#content` is bound to the `router` module's active item.</span></span> <span data-ttu-id="804d0-185">Diğer bir deyişle, karşılık gelen bir görünümü olan bir gezinti bağlantısı tıkladığınızda Bu alanda yüklenir.</span><span class="sxs-lookup"><span data-stu-id="804d0-185">In other words, when you click a navigation link it's corresponding view is loaded in this area.</span></span>

[!code-html[Main](hottowel-template/samples/sample4.html)]

### <a name="navhtml"></a><span data-ttu-id="804d0-186">NAV.HTML</span><span class="sxs-lookup"><span data-stu-id="804d0-186">nav.html</span></span>

<span data-ttu-id="804d0-187">`nav.html` SPA Gezinti bağlantıları içerir.</span><span class="sxs-lookup"><span data-stu-id="804d0-187">The `nav.html` contains the navigation links for the SPA.</span></span> <span data-ttu-id="804d0-188">Burada menüsü yapısı, örneğin yerleştirilebilir budur.</span><span class="sxs-lookup"><span data-stu-id="804d0-188">This is where the menu structure can be placed, for example.</span></span> <span data-ttu-id="804d0-189">Genellikle veri bağlama (Knockout kullanarak) için budur `router` modülünde, tanımladığınız Gezinti görüntülemek için `shell.js`.</span><span class="sxs-lookup"><span data-stu-id="804d0-189">Often this is data bound (using Knockout) to the `router` module to display the navigation you defined in the `shell.js`.</span></span> <span data-ttu-id="804d0-190">Knockout arar veri bağlama özniteliklerini ve bunları bağlar `shell` viewmodel Gezinti tariflerini görüntülemek ve bir progressbar (Twitter Bootstrap ile) göstermek için ise `router` modülüdür başka bir (bkz: tek bir görünümde gezinme ortasında `router.isNavigating`).</span><span class="sxs-lookup"><span data-stu-id="804d0-190">Knockout looks for the data-bind attributes and binds those to the `shell` viewmodel to display the navigation routes and to show a progressbar (using Twitter Bootstrap) if the `router` module is in the middle of navigating from one view to another (see `router.isNavigating`).</span></span>

[!code-html[Main](hottowel-template/samples/sample5.html)]

### <a name="homehtml-and-detailshtml"></a><span data-ttu-id="804d0-191">Home.HTML ve details.html</span><span class="sxs-lookup"><span data-stu-id="804d0-191">home.html and details.html</span></span>

<span data-ttu-id="804d0-192">Bu görünümler, HTML için özel görünümleri içerir.</span><span class="sxs-lookup"><span data-stu-id="804d0-192">These views contain HTML for custom views.</span></span> <span data-ttu-id="804d0-193">Zaman `home` bağlantısını `nav` Görünüm menüsünde tıklandığında `home` görünümü içerik alanında bulunan yerleştirilecek `shell` görünümü.</span><span class="sxs-lookup"><span data-stu-id="804d0-193">When the `home` link in the `nav` view's menu is clicked, the `home` view will be placed in the content area of the `shell` view.</span></span> <span data-ttu-id="804d0-194">Bu görünümler, genişletilmiş veya kendi özel görünümlerinizi ile değiştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="804d0-194">These views can be augmented or replaced with your own custom views.</span></span>

### <a name="footerhtml"></a><span data-ttu-id="804d0-195">Footer.HTML</span><span class="sxs-lookup"><span data-stu-id="804d0-195">footer.html</span></span>

<span data-ttu-id="804d0-196">`footer.html` Altbilgisindeki sayfanın alt kısmında görüntülenen HTML içeren `shell` görünümü.</span><span class="sxs-lookup"><span data-stu-id="804d0-196">The `footer.html` contains HTML that appears in the footer, at the bottom of the `shell` view.</span></span>

## <a name="viewmodels"></a><span data-ttu-id="804d0-197">Viewmodel'lar</span><span class="sxs-lookup"><span data-stu-id="804d0-197">ViewModels</span></span>

<span data-ttu-id="804d0-198">İçinde bulunan Viewmodel'lar `App/viewmodels` klasör.</span><span class="sxs-lookup"><span data-stu-id="804d0-198">ViewModels are found in the `App/viewmodels` folder.</span></span>

### <a name="shelljs"></a><span data-ttu-id="804d0-199">Shell.js</span><span class="sxs-lookup"><span data-stu-id="804d0-199">shell.js</span></span>

<span data-ttu-id="804d0-200">`shell` Viewmodel özellikleri ve bağlı işlevler içerir `shell` görünümü.</span><span class="sxs-lookup"><span data-stu-id="804d0-200">The `shell` viewmodel contains properties and functions that are bound to the `shell` view.</span></span> <span data-ttu-id="804d0-201">Genellikle bu menü Gezinti bağlamaları bulunduğu, (bkz `router.mapNav` mantıksal).</span><span class="sxs-lookup"><span data-stu-id="804d0-201">Often this is where the menu navigation bindings are found (see the `router.mapNav` logic).</span></span>

[!code-javascript[Main](hottowel-template/samples/sample6.js)]

### <a name="homejs-and-detailsjs"></a><span data-ttu-id="804d0-202">Home.js ve details.js</span><span class="sxs-lookup"><span data-stu-id="804d0-202">home.js and details.js</span></span>

<span data-ttu-id="804d0-203">Bu viewmodel'lar bağlı işlevleri ve özellikleri içeren `home` görünümü.</span><span class="sxs-lookup"><span data-stu-id="804d0-203">These viewmodels contain the properties and functions that are bound to the `home` view.</span></span> <span data-ttu-id="804d0-204">Ayrıca görünüm için sunu mantığı içeren ve veri ve görünüm arasında bir Yapıştırıcı işlevi görür.</span><span class="sxs-lookup"><span data-stu-id="804d0-204">it also contains the presentation logic for the view, and is the glue between the data and the view.</span></span>

[!code-javascript[Main](hottowel-template/samples/sample7.js)]

## <a name="services"></a><span data-ttu-id="804d0-205">Hizmetler</span><span class="sxs-lookup"><span data-stu-id="804d0-205">Services</span></span>

<span data-ttu-id="804d0-206">Hizmetleri uygulama/hizmetleri klasöründe bulunur.</span><span class="sxs-lookup"><span data-stu-id="804d0-206">Services are found in the App/services folder.</span></span> <span data-ttu-id="804d0-207">İdeal olarak, gelecekteki hizmetlerinizi alma ve uzak veriler gönderilirken sorumlu olan dataservice modülü gibi yerleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="804d0-207">Ideally your future services such as a dataservice module, that is responsible for getting and posting remote data, could be placed.</span></span>

### <a name="loggerjs"></a><span data-ttu-id="804d0-208">Logger.js</span><span class="sxs-lookup"><span data-stu-id="804d0-208">logger.js</span></span>

<span data-ttu-id="804d0-209">Hot Towel sağlayan bir `logger` Hizmetleri klasörü modülünde.</span><span class="sxs-lookup"><span data-stu-id="804d0-209">Hot Towel provides a `logger` module in the services folder.</span></span> <span data-ttu-id="804d0-210">`logger` Modülüdür günlük iletilerini konsola ve toasts açılır kullanıcıya için idealdir.</span><span class="sxs-lookup"><span data-stu-id="804d0-210">The `logger` module is ideal for logging messages to the console and to the user in pop up toasts.</span></span>
