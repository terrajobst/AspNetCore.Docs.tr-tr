---
title: ASP.NET MVC ASP.NET Core MVC geçirme
author: ardalis
description: ASP.NET MVC projesinde ASP.NET Core MVC geçirme başlayacağınızı öğrenin.
manager: wpickett
ms.author: riande
ms.date: 03/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/mvc
ms.openlocfilehash: b8c913c0a6f47a1c993d508f9baae54981327957
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2018
ms.locfileid: "33851033"
---
# <a name="migrate-from-aspnet-mvc-to-aspnet-core-mvc"></a><span data-ttu-id="c1975-103">ASP.NET MVC ASP.NET Core MVC geçirme</span><span class="sxs-lookup"><span data-stu-id="c1975-103">Migrate from ASP.NET MVC to ASP.NET Core MVC</span></span>

<span data-ttu-id="c1975-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), [Steve Smith](https://ardalis.com/), ve [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="c1975-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), [Steve Smith](https://ardalis.com/), and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="c1975-105">Bu makalede, ASP.NET MVC projesinde geçirme başlamak gösterilmiştir [ASP.NET Core MVC](../mvc/overview.md).</span><span class="sxs-lookup"><span data-stu-id="c1975-105">This article shows how to get started migrating an ASP.NET MVC project to [ASP.NET Core MVC](../mvc/overview.md).</span></span> <span data-ttu-id="c1975-106">İşlem sırasında çoğunu ASP.NET MVC değişen vurgular.</span><span class="sxs-lookup"><span data-stu-id="c1975-106">In the process, it highlights many of the things that have changed from ASP.NET MVC.</span></span> <span data-ttu-id="c1975-107">ASP.NET MVC geçiş birden çok adım bir işlemdir ve bu makalede, ilk Kurulum, temel denetleyicileri ve görünümler, statik içerik ve istemci tarafı bağımlılıkları yer almaktadır.</span><span class="sxs-lookup"><span data-stu-id="c1975-107">Migrating from ASP.NET MVC is a multiple step process and this article covers the initial setup, basic controllers and views, static content, and client-side dependencies.</span></span> <span data-ttu-id="c1975-108">Diğer makaleler geçirme yapılandırması ve kimlik kodu birçok ASP.NET MVC projelerinde bulunamadı kapsar.</span><span class="sxs-lookup"><span data-stu-id="c1975-108">Additional articles cover migrating configuration and identity code found in many ASP.NET MVC projects.</span></span>

> [!NOTE]
> <span data-ttu-id="c1975-109">Sürüm numaraları örneklerdeki geçerli olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="c1975-109">The version numbers in the samples might not be current.</span></span> <span data-ttu-id="c1975-110">Buna göre projelerinizi güncelleştirmeniz gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="c1975-110">You may need to update your projects accordingly.</span></span>

## <a name="create-the-starter-aspnet-mvc-project"></a><span data-ttu-id="c1975-111">ASP.NET MVC projesinin starter oluşturma</span><span class="sxs-lookup"><span data-stu-id="c1975-111">Create the starter ASP.NET MVC project</span></span>

<span data-ttu-id="c1975-112">Yükseltme göstermek için bir ASP.NET MVC uygulaması oluşturarak başlayacağız.</span><span class="sxs-lookup"><span data-stu-id="c1975-112">To demonstrate the upgrade, we'll start by creating a ASP.NET MVC app.</span></span> <span data-ttu-id="c1975-113">Adlı oluşturun *WebApp1* ad alanı sonraki adımda oluşturuyoruz ASP.NET Core proje eşleşecek şekilde.</span><span class="sxs-lookup"><span data-stu-id="c1975-113">Create it with the name *WebApp1* so the namespace matches the ASP.NET Core project we create in the next step.</span></span>

![Visual Studio yeni proje iletişim kutusu](mvc/_static/new-project.png)

![Yeni Web uygulaması iletişim kutusu: ASP.NET şablonları panelinde seçili MVC proje şablonu](mvc/_static/new-project-select-mvc-template.png)

<span data-ttu-id="c1975-116">*İsteğe bağlı:* çözümden adını değiştirmek *WebApp1* için *Mvc5*.</span><span class="sxs-lookup"><span data-stu-id="c1975-116">*Optional:* Change the name of the Solution from *WebApp1* to *Mvc5*.</span></span> <span data-ttu-id="c1975-117">Visual Studio yeni çözüm adını görüntüler (*Mvc5*), hangi kolaylaştırır sonraki proje bu projeden bildirmek.</span><span class="sxs-lookup"><span data-stu-id="c1975-117">Visual Studio displays the new solution name (*Mvc5*), which makes it easier to tell this project from the next project.</span></span>

## <a name="create-the-aspnet-core-project"></a><span data-ttu-id="c1975-118">ASP.NET Core projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="c1975-118">Create the ASP.NET Core project</span></span>

<span data-ttu-id="c1975-119">Yeni bir *boş* ASP.NET Core web uygulaması önceki projesi olarak aynı ada sahip (*WebApp1*) ad alanları iki proje ile eşleşecek şekilde.</span><span class="sxs-lookup"><span data-stu-id="c1975-119">Create a new *empty* ASP.NET Core web app with the same name as the previous project (*WebApp1*) so the namespaces in the two projects match.</span></span> <span data-ttu-id="c1975-120">Aynı ad alanına sahip iki projeler arasında kodu kopyalamak kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="c1975-120">Having the same namespace makes it easier to copy code between the two projects.</span></span> <span data-ttu-id="c1975-121">Aynı adı kullanmak üzere önceki proje daha farklı bir dizinde bu projeyi oluşturmak zorunda kalırsınız.</span><span class="sxs-lookup"><span data-stu-id="c1975-121">You'll have to create this project in a different directory than the previous project to use the same name.</span></span>

![Yeni Proje iletişim kutusu](mvc/_static/new_core.png)

![Yeni ASP.NET Web uygulaması iletişim kutusu: ASP.NET Core şablonları panelinde seçili boş proje şablonu](mvc/_static/new-project-select-empty-aspnet5-template.png)

* <span data-ttu-id="c1975-124">*İsteğe bağlı:* kullanarak yeni bir ASP.NET Core uygulama oluştur *Web uygulaması* proje şablonu.</span><span class="sxs-lookup"><span data-stu-id="c1975-124">*Optional:* Create a new ASP.NET Core app using the *Web Application* project template.</span></span> <span data-ttu-id="c1975-125">Proje adı *WebApp1*, bir kimlik doğrulama seçeneğini seçip **tek tek kullanıcı hesaplarını**.</span><span class="sxs-lookup"><span data-stu-id="c1975-125">Name the project *WebApp1*, and select an authentication option of **Individual User Accounts**.</span></span> <span data-ttu-id="c1975-126">Bu uygulama yeniden adlandırma *FullAspNetCore*.</span><span class="sxs-lookup"><span data-stu-id="c1975-126">Rename this app to *FullAspNetCore*.</span></span> <span data-ttu-id="c1975-127">Size zaman proje kazandırır dönüştürmede oluşturuluyor.</span><span class="sxs-lookup"><span data-stu-id="c1975-127">Creating this project saves you time in the conversion.</span></span> <span data-ttu-id="c1975-128">Sonuç görmek için veya dönüştürme projeye kodu kopyalamak için ' şablon oluşturulan kod bakabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c1975-128">You can look at the template-generated code to see the end result or to copy code to the conversion project.</span></span> <span data-ttu-id="c1975-129">Şablon tarafından oluşturulan proje ile karşılaştırmak için dönüştürme adım sıkışan gerektiğinde de yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="c1975-129">It's also helpful when you get stuck on a conversion step to compare with the template-generated project.</span></span>

## <a name="configure-the-site-to-use-mvc"></a><span data-ttu-id="c1975-130">Siteyi MVC kullanacak şekilde yapılandırma</span><span class="sxs-lookup"><span data-stu-id="c1975-130">Configure the site to use MVC</span></span>

* <span data-ttu-id="c1975-131">.NET Core hedeflerken ASP.NET Core metapackage adlı projeye eklenen `Microsoft.AspNetCore.All` varsayılan olarak.</span><span class="sxs-lookup"><span data-stu-id="c1975-131">When targeting .NET Core, the ASP.NET Core metapackage is added to the project, called `Microsoft.AspNetCore.All` by default.</span></span> <span data-ttu-id="c1975-132">Paketleri gibi bu pakette `Microsoft.AspNetCore.Mvc` ve `Microsoft.AspNetCore.StaticFiles`.</span><span class="sxs-lookup"><span data-stu-id="c1975-132">This package contains packages like `Microsoft.AspNetCore.Mvc` and `Microsoft.AspNetCore.StaticFiles`.</span></span> <span data-ttu-id="c1975-133">.NET Framework'ü hedefleme, paket referanslarını ayrı ayrı \*.csproj dosyasında listelenen gerekir.</span><span class="sxs-lookup"><span data-stu-id="c1975-133">If targeting .NET Framework, package references need to be listed individually in the \*.csproj file.</span></span>

<span data-ttu-id="c1975-134">`Microsoft.AspNetCore.Mvc` ASP.NET Core MVC çerçevedir.</span><span class="sxs-lookup"><span data-stu-id="c1975-134">`Microsoft.AspNetCore.Mvc` is the ASP.NET Core MVC framework.</span></span> <span data-ttu-id="c1975-135">`Microsoft.AspNetCore.StaticFiles` statik dosya işleyici olur.</span><span class="sxs-lookup"><span data-stu-id="c1975-135">`Microsoft.AspNetCore.StaticFiles` is the static file handler.</span></span> <span data-ttu-id="c1975-136">ASP.NET çekirdeği çalışma zamanı modüler ve açıkça statik dosyaları işleme için kabul gerekir (bkz [statik dosyaları](xref:fundamentals/static-files)).</span><span class="sxs-lookup"><span data-stu-id="c1975-136">The ASP.NET Core runtime is modular, and you must explicitly opt in to serve static files (see [Static files](xref:fundamentals/static-files)).</span></span>

* <span data-ttu-id="c1975-137">Açık *haline* dosya ve kodu aşağıdaki ile eşleşecek şekilde değiştirin:</span><span class="sxs-lookup"><span data-stu-id="c1975-137">Open the *Startup.cs* file and change the code to match the following:</span></span>

  [!code-csharp[](mvc/sample/Startup.cs?highlight=13,26-31)]

<span data-ttu-id="c1975-138">`UseStaticFiles` Genişletme yöntemi statik dosya işleyici ekler.</span><span class="sxs-lookup"><span data-stu-id="c1975-138">The `UseStaticFiles` extension method adds the static file handler.</span></span> <span data-ttu-id="c1975-139">Daha önce belirtildiği gibi ASP.NET çalışma zamanı modüler ve açıkça statik dosyaları işleme için kabul gerekir.</span><span class="sxs-lookup"><span data-stu-id="c1975-139">As mentioned previously, the ASP.NET runtime is modular, and you must explicitly opt in to serve static files.</span></span> <span data-ttu-id="c1975-140">`UseMvc` Genişletme yöntemi ekler yönlendirme.</span><span class="sxs-lookup"><span data-stu-id="c1975-140">The `UseMvc` extension method adds routing.</span></span> <span data-ttu-id="c1975-141">Daha fazla bilgi için bkz: [uygulama başlangıç](xref:fundamentals/startup) ve [yönlendirme](xref:fundamentals/routing).</span><span class="sxs-lookup"><span data-stu-id="c1975-141">For more information, see [Application Startup](xref:fundamentals/startup) and [Routing](xref:fundamentals/routing).</span></span>

## <a name="add-a-controller-and-view"></a><span data-ttu-id="c1975-142">Bir denetleyici ve Görünüm Ekle</span><span class="sxs-lookup"><span data-stu-id="c1975-142">Add a controller and view</span></span>

<span data-ttu-id="c1975-143">Bu bölümde, bir en az denetleyici ve görünüm ASP.NET MVC denetleyicisi yer tutucular olarak hizmet verecek ve sonraki bölümde geçirmek görünümleri ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="c1975-143">In this section, you'll add a minimal controller and view to serve as placeholders for the ASP.NET MVC controller and views you'll migrate in the next section.</span></span>

* <span data-ttu-id="c1975-144">Ekleme bir *denetleyicileri* klasör.</span><span class="sxs-lookup"><span data-stu-id="c1975-144">Add a *Controllers* folder.</span></span>

* <span data-ttu-id="c1975-145">Ekleme bir **denetleyici sınıfı** adlı *HomeController.cs* için *denetleyicileri* klasör.</span><span class="sxs-lookup"><span data-stu-id="c1975-145">Add a **Controller Class** named *HomeController.cs* to the *Controllers* folder.</span></span>

![Yeni öğe iletişim ekleyin](mvc/_static/add_mvc_ctl.png)

* <span data-ttu-id="c1975-147">Ekleme bir *görünümleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="c1975-147">Add a *Views* folder.</span></span>

* <span data-ttu-id="c1975-148">Ekleme bir *görünümler/giriş* klasör.</span><span class="sxs-lookup"><span data-stu-id="c1975-148">Add a *Views/Home* folder.</span></span>

* <span data-ttu-id="c1975-149">Ekleme bir **Razor Görünüm** adlı *Index.cshtml* için *görünümler/giriş* klasör.</span><span class="sxs-lookup"><span data-stu-id="c1975-149">Add a **Razor View** named *Index.cshtml* to the *Views/Home* folder.</span></span>

![Yeni öğe iletişim ekleyin](mvc/_static/view.png)

<span data-ttu-id="c1975-151">Proje yapısı aşağıda gösterilmiştir:</span><span class="sxs-lookup"><span data-stu-id="c1975-151">The project structure is shown below:</span></span>

![Dosya ve klasörleri WebApp1 gösteren Çözüm Gezgini](mvc/_static/project-structure-controller-view.png)

<span data-ttu-id="c1975-153">Değiştir *Views/Home/Index.cshtml* aşağıdaki dosyasıyla:</span><span class="sxs-lookup"><span data-stu-id="c1975-153">Replace the contents of the *Views/Home/Index.cshtml* file with the following:</span></span>

```html
<h1>Hello world!</h1>
```

<span data-ttu-id="c1975-154">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="c1975-154">Run the app.</span></span>

![Web uygulaması Microsoft Edge'de Aç](mvc/_static/hello-world.png)

<span data-ttu-id="c1975-156">Bkz: [denetleyicileri](xref:mvc/controllers/actions) ve [görünümleri](xref:mvc/views/overview) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="c1975-156">See [Controllers](xref:mvc/controllers/actions) and [Views](xref:mvc/views/overview) for more information.</span></span>

<span data-ttu-id="c1975-157">En az bir çalışma ASP.NET Core proje sahibiz, biz işlevselliği ASP.NET MVC projesinden geçiş başlatabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c1975-157">Now that we have a minimal working ASP.NET Core project, we can start migrating functionality from the ASP.NET MVC project.</span></span> <span data-ttu-id="c1975-158">Aşağıdaki taşımak ihtiyacımız var:</span><span class="sxs-lookup"><span data-stu-id="c1975-158">We need to move the following:</span></span>

* <span data-ttu-id="c1975-159">istemci-tarafı içeriği (CSS, yazı tipleri ve komut dosyaları)</span><span class="sxs-lookup"><span data-stu-id="c1975-159">client-side content (CSS, fonts, and scripts)</span></span>

* <span data-ttu-id="c1975-160">denetleyiciler</span><span class="sxs-lookup"><span data-stu-id="c1975-160">controllers</span></span>

* <span data-ttu-id="c1975-161">görünümler</span><span class="sxs-lookup"><span data-stu-id="c1975-161">views</span></span>

* <span data-ttu-id="c1975-162">modeller</span><span class="sxs-lookup"><span data-stu-id="c1975-162">models</span></span>

* <span data-ttu-id="c1975-163">Paketleme</span><span class="sxs-lookup"><span data-stu-id="c1975-163">bundling</span></span>

* <span data-ttu-id="c1975-164">filtreler</span><span class="sxs-lookup"><span data-stu-id="c1975-164">filters</span></span>

* <span data-ttu-id="c1975-165">Giriş/Çıkış günlüğü, kimlik (Bu, sonraki öğreticide gerçekleştirilir.)</span><span class="sxs-lookup"><span data-stu-id="c1975-165">Log in/out, Identity (This is done in the next tutorial.)</span></span>

## <a name="controllers-and-views"></a><span data-ttu-id="c1975-166">Denetleyicileri ve görünümler</span><span class="sxs-lookup"><span data-stu-id="c1975-166">Controllers and views</span></span>

* <span data-ttu-id="c1975-167">Yöntemlerin her biri ASP.NET MVC kopyalama `HomeController` yeni `HomeController`.</span><span class="sxs-lookup"><span data-stu-id="c1975-167">Copy each of the methods from the ASP.NET MVC `HomeController` to the new `HomeController`.</span></span> <span data-ttu-id="c1975-168">ASP.NET MVC uygulamasında yerleşik şablonun denetleyici eylem yönteminin dönüş türü olduğunu unutmayın [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); ASP.NET mvc'de çekirdek, dönüş eylem yöntemleri `IActionResult` yerine.</span><span class="sxs-lookup"><span data-stu-id="c1975-168">Note that in ASP.NET MVC, the built-in template's controller action method return type is [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); in ASP.NET Core MVC, the action methods return `IActionResult` instead.</span></span> <span data-ttu-id="c1975-169">`ActionResult` uygulayan `IActionResult`, bu yüzden, eylem yöntemleri dönüş türü değiştirmenize gerek yoktur.</span><span class="sxs-lookup"><span data-stu-id="c1975-169">`ActionResult` implements `IActionResult`, so there's no need to change the return type of your action methods.</span></span>

* <span data-ttu-id="c1975-170">Kopya *About.cshtml*, *Contact.cshtml*, ve *Index.cshtml* ASP.NET Core proje için ASP.NET MVC projesindeki Razor görünüm dosyaları.</span><span class="sxs-lookup"><span data-stu-id="c1975-170">Copy the *About.cshtml*, *Contact.cshtml*, and *Index.cshtml* Razor view files from the ASP.NET MVC project to the ASP.NET Core project.</span></span>

* <span data-ttu-id="c1975-171">ASP.NET Core uygulamayı çalıştırın ve her yöntemi sınayın.</span><span class="sxs-lookup"><span data-stu-id="c1975-171">Run the ASP.NET Core app and test each method.</span></span> <span data-ttu-id="c1975-172">İşlenmiş görünüm yalnızca görünüm dosyalarının içeriği içeren böylece biz düzeni dosyasını veya stilleri henüz, geçirilen henüz.</span><span class="sxs-lookup"><span data-stu-id="c1975-172">We haven't migrated the layout file or styles yet, so the rendered views only contain the content in the view files.</span></span> <span data-ttu-id="c1975-173">Düzen oluşturulan dosyanın bağlantılarını olmayacaktır `About` ve `Contact` tarayıcıdan çağırma gerekecek şekilde görünümler (Değiştir **4492** projenizde kullanılan bağlantı noktası numarası ile).</span><span class="sxs-lookup"><span data-stu-id="c1975-173">You won't have the layout file generated links for the `About` and `Contact` views, so you'll have to invoke them from the browser (replace **4492** with the port number used in your project).</span></span>

  * `http://localhost:4492/home/about`

  * `http://localhost:4492/home/contact`

![Kişi sayfası](mvc/_static/contact-page.png)

<span data-ttu-id="c1975-175">Stil ve menü öğelerini eksikliği unutmayın.</span><span class="sxs-lookup"><span data-stu-id="c1975-175">Note the lack of styling and menu items.</span></span> <span data-ttu-id="c1975-176">Biz, sonraki bölümde düzeltmesi.</span><span class="sxs-lookup"><span data-stu-id="c1975-176">We'll fix that in the next section.</span></span>

## <a name="static-content"></a><span data-ttu-id="c1975-177">Statik içerik</span><span class="sxs-lookup"><span data-stu-id="c1975-177">Static content</span></span>

<span data-ttu-id="c1975-178">ASP.NET MVC önceki sürümlerinde, statik içerik web projesi kökünden barındırılan ve sunucu tarafı dosyalarıyla intermixed.</span><span class="sxs-lookup"><span data-stu-id="c1975-178">In previous versions of ASP.NET MVC, static content was hosted from the root of the web project and was intermixed with server-side files.</span></span> <span data-ttu-id="c1975-179">ASP.NET çekirdek statik içerik içinde barındırılan *wwwroot* klasör.</span><span class="sxs-lookup"><span data-stu-id="c1975-179">In ASP.NET Core, static content is hosted in the *wwwroot* folder.</span></span> <span data-ttu-id="c1975-180">Statik içerik eski ASP.NET MVC uygulamanıza kopyalamak istersiniz *wwwroot* ASP.NET Core projenizdeki klasöre.</span><span class="sxs-lookup"><span data-stu-id="c1975-180">You'll want to copy the static content from your old ASP.NET MVC app to the *wwwroot* folder in your ASP.NET Core project.</span></span> <span data-ttu-id="c1975-181">Bu örnek dönüştürmede:</span><span class="sxs-lookup"><span data-stu-id="c1975-181">In this sample conversion:</span></span>

* <span data-ttu-id="c1975-182">Kopya *favicon.ico* eski MVC proje dosyasından *wwwroot* ASP.NET Core proje klasöründe.</span><span class="sxs-lookup"><span data-stu-id="c1975-182">Copy the *favicon.ico* file from the old MVC project to the *wwwroot* folder in the ASP.NET Core project.</span></span>

<span data-ttu-id="c1975-183">ASP.NET MVC eski proje kullanır [önyükleme](https://getbootstrap.com/) önyükleme dosyaları stil ve depoları *içerik* ve *betikleri* klasörler.</span><span class="sxs-lookup"><span data-stu-id="c1975-183">The old ASP.NET MVC project uses [Bootstrap](https://getbootstrap.com/) for its styling and stores the Bootstrap files in the *Content* and *Scripts* folders.</span></span> <span data-ttu-id="c1975-184">Önyükleme düzeni dosyasında eski ASP.NET MVC projesinin oluşturulan, şablonu başvuruyor (*Views/Shared/_Layout.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="c1975-184">The template, which generated the old ASP.NET MVC project, references Bootstrap in the layout file (*Views/Shared/_Layout.cshtml*).</span></span> <span data-ttu-id="c1975-185">Kopyalamak *bootstrap.js* ve *bootstrap.css* ASP.NET MVC dosyalarından proje için *wwwroot* yeni proje klasöründe.</span><span class="sxs-lookup"><span data-stu-id="c1975-185">You could copy the *bootstrap.js* and *bootstrap.css* files from the ASP.NET MVC project to the *wwwroot* folder in the new project.</span></span> <span data-ttu-id="c1975-186">Bunun yerine, sonraki bölümde CDN'ler kullanarak önyükleme desteği (ve diğer istemci-tarafı kitaplıklar) ekleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="c1975-186">Instead, we'll add support for Bootstrap (and other client-side libraries) using CDNs in the next section.</span></span>

## <a name="migrate-the-layout-file"></a><span data-ttu-id="c1975-187">Düzen dosyasını geçirme</span><span class="sxs-lookup"><span data-stu-id="c1975-187">Migrate the layout file</span></span>

* <span data-ttu-id="c1975-188">Kopya *_ViewStart.cshtml* eski ASP.NET MVC projesinin dosyadan *görünümleri* ASP.NET Core projenin klasörüne *görünümleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="c1975-188">Copy the *_ViewStart.cshtml* file from the old ASP.NET MVC project's *Views* folder into the ASP.NET Core project's *Views* folder.</span></span> <span data-ttu-id="c1975-189">*_ViewStart.cshtml* dosya ASP.NET Core MVC'de değişmemiştir.</span><span class="sxs-lookup"><span data-stu-id="c1975-189">The *_ViewStart.cshtml* file has not changed in ASP.NET Core MVC.</span></span>

* <span data-ttu-id="c1975-190">Oluşturma bir *görünümler/paylaşılan* klasör.</span><span class="sxs-lookup"><span data-stu-id="c1975-190">Create a *Views/Shared* folder.</span></span>

* <span data-ttu-id="c1975-191">*İsteğe bağlı:* kopya *_viewımports.cshtml* gelen *FullAspNetCore* MVC projesinin *görünümleri* ASP.NET Core projenin klasörüne  *Görünümler* klasör.</span><span class="sxs-lookup"><span data-stu-id="c1975-191">*Optional:* Copy *_ViewImports.cshtml* from the *FullAspNetCore* MVC project's *Views* folder into the ASP.NET Core project's *Views* folder.</span></span> <span data-ttu-id="c1975-192">Tüm ad alanı bildirimini kaldırın *_viewımports.cshtml* dosya.</span><span class="sxs-lookup"><span data-stu-id="c1975-192">Remove any namespace declaration in the *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="c1975-193">*_Viewımports.cshtml* dosya ad alanları için tüm görünüm dosyaları sağlar ve getirir [etiket Yardımcıları](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="c1975-193">The *_ViewImports.cshtml* file provides namespaces for all the view files and brings in [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="c1975-194">Etiket Yardımcıları yeni düzen dosyasında kullanılır.</span><span class="sxs-lookup"><span data-stu-id="c1975-194">Tag Helpers are used in the new layout file.</span></span> <span data-ttu-id="c1975-195">*_Viewımports.cshtml* dosya ASP.NET Core için yenidir.</span><span class="sxs-lookup"><span data-stu-id="c1975-195">The *_ViewImports.cshtml* file is new for ASP.NET Core.</span></span>

* <span data-ttu-id="c1975-196">Kopya *_Layout.cshtml* eski ASP.NET MVC projesinin dosyadan *görünümler/paylaşılan* ASP.NET Core projenin klasörüne *görünümler/paylaşılan* klasör.</span><span class="sxs-lookup"><span data-stu-id="c1975-196">Copy the *_Layout.cshtml* file from the old ASP.NET MVC project's *Views/Shared* folder into the ASP.NET Core project's *Views/Shared* folder.</span></span>

<span data-ttu-id="c1975-197">Açık *_Layout.cshtml* dosya ve (tamamlanan kodu aşağıda gösterilmektedir) aşağıdaki değişiklikleri yapın:</span><span class="sxs-lookup"><span data-stu-id="c1975-197">Open *_Layout.cshtml* file and make the following changes (the completed code is shown below):</span></span>

* <span data-ttu-id="c1975-198">Değiştir `@Styles.Render("~/Content/css")` ile bir `<link>` yüklemek için öğesi *bootstrap.css* (aşağıya bakın).</span><span class="sxs-lookup"><span data-stu-id="c1975-198">Replace `@Styles.Render("~/Content/css")` with a `<link>` element to load *bootstrap.css* (see below).</span></span>

* <span data-ttu-id="c1975-199">Kaldırma `@Scripts.Render("~/bundles/modernizr")`.</span><span class="sxs-lookup"><span data-stu-id="c1975-199">Remove `@Scripts.Render("~/bundles/modernizr")`.</span></span>

* <span data-ttu-id="c1975-200">Out açıklama `@Html.Partial("_LoginPartial")` satır (satırıyla çevreleyen `@*...*@`).</span><span class="sxs-lookup"><span data-stu-id="c1975-200">Comment out the `@Html.Partial("_LoginPartial")` line (surround the line with `@*...*@`).</span></span> <span data-ttu-id="c1975-201">Sonraki öğreticide kendisine getireceğiz.</span><span class="sxs-lookup"><span data-stu-id="c1975-201">We'll return to it in a future tutorial.</span></span>

* <span data-ttu-id="c1975-202">Değiştir `@Scripts.Render("~/bundles/jquery")` ile bir `<script>` öğesi (aşağıya bakın).</span><span class="sxs-lookup"><span data-stu-id="c1975-202">Replace `@Scripts.Render("~/bundles/jquery")` with a `<script>` element (see below).</span></span>

* <span data-ttu-id="c1975-203">Değiştir `@Scripts.Render("~/bundles/bootstrap")` ile bir `<script>` öğesi (aşağıya bakın).</span><span class="sxs-lookup"><span data-stu-id="c1975-203">Replace `@Scripts.Render("~/bundles/bootstrap")` with a `<script>` element (see below).</span></span>

<span data-ttu-id="c1975-204">Değiştirme biçimlendirme önyükleme CSS eklemek için:</span><span class="sxs-lookup"><span data-stu-id="c1975-204">The replacement markup for Bootstrap CSS inclusion:</span></span>

```html
<link rel="stylesheet"
    href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css"
    integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u"
    crossorigin="anonymous">
```

<span data-ttu-id="c1975-205">JQuery ve önyükleme JavaScript ekleme için değiştirme biçimlendirme:</span><span class="sxs-lookup"><span data-stu-id="c1975-205">The replacement markup for jQuery and Bootstrap JavaScript inclusion:</span></span>

```html
<script src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js"
    integrity="sha384-Tc5IQib027qvyjSMfHjOMaLkfuWVxZxUPnCJA7l2mCWNIpG9mGCD8wGNIcPD7Txa" crossorigin="anonymous"></script>
```

<span data-ttu-id="c1975-206">Güncelleştirilmiş *_Layout.cshtml* dosya aşağıda gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="c1975-206">The updated *_Layout.cshtml* file is shown below:</span></span>

[!code-cshtml[](mvc/sample/Views/Shared/_Layout.cshtml?highlight=7-10,29,41-44)]

<span data-ttu-id="c1975-207">Site tarayıcıda görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="c1975-207">View the site in the browser.</span></span> <span data-ttu-id="c1975-208">Artık doğru yerde beklenen stilleri ile yüklenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="c1975-208">It should now load correctly, with the expected styles in place.</span></span>

* <span data-ttu-id="c1975-209">*İsteğe bağlı:* yeni düzen dosyasını kullanmayı denemek isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c1975-209">*Optional:* You might want to try using the new layout file.</span></span> <span data-ttu-id="c1975-210">Bu proje için Düzen dosyasından kopyalayabilirsiniz *FullAspNetCore* projesi.</span><span class="sxs-lookup"><span data-stu-id="c1975-210">For this project you can copy the layout file from the *FullAspNetCore* project.</span></span> <span data-ttu-id="c1975-211">Yeni düzen dosyasını kullanan [etiket Yardımcıları](xref:mvc/views/tag-helpers/intro) ve diğer geliştirmeler sahiptir.</span><span class="sxs-lookup"><span data-stu-id="c1975-211">The new layout file uses [Tag Helpers](xref:mvc/views/tag-helpers/intro) and has other improvements.</span></span>

## <a name="configure-bundling-and-minification"></a><span data-ttu-id="c1975-212">Paketleme ve küçültme yapılandırın</span><span class="sxs-lookup"><span data-stu-id="c1975-212">Configure bundling and minification</span></span>

<span data-ttu-id="c1975-213">Paketleme ve küçültme yapılandırma hakkında daha fazla bilgi için bkz: [paketleme ve küçültme](../client-side/bundling-and-minification.md).</span><span class="sxs-lookup"><span data-stu-id="c1975-213">For information about how to configure bundling and minification, see [Bundling and Minification](../client-side/bundling-and-minification.md).</span></span>

## <a name="solve-http-500-errors"></a><span data-ttu-id="c1975-214">HTTP 500 hataları çözme</span><span class="sxs-lookup"><span data-stu-id="c1975-214">Solve HTTP 500 errors</span></span>

<span data-ttu-id="c1975-215">Sorunun kaynağını hakkında hiçbir bilgi içeren bir HTTP 500 hata iletisini neden olan birçok sorunları vardır.</span><span class="sxs-lookup"><span data-stu-id="c1975-215">There are many problems that can cause a HTTP 500 error message that contain no information on the source of the problem.</span></span> <span data-ttu-id="c1975-216">Örneğin, varsa *Views/_ViewImports.cshtml* dosya içeriyorsa, projede mevcut olmayan bir ad alanı, bir HTTP 500 hata iletisi alırsınız.</span><span class="sxs-lookup"><span data-stu-id="c1975-216">For example, if the *Views/_ViewImports.cshtml* file contains a namespace that doesn't exist in your project, you'll get a HTTP 500 error.</span></span> <span data-ttu-id="c1975-217">Varsayılan olarak ASP.NET Core uygulamaları `UseDeveloperExceptionPage` uzantı eklenir `IApplicationBuilder` ve yapılandırma olduğunda yürütülen *geliştirme*.</span><span class="sxs-lookup"><span data-stu-id="c1975-217">By default in ASP.NET Core apps, the `UseDeveloperExceptionPage` extension is added to the `IApplicationBuilder` and executed when the configuration is *Development*.</span></span> <span data-ttu-id="c1975-218">Bu aşağıdaki kodda ayrıntılı olarak açıklanmıştır:</span><span class="sxs-lookup"><span data-stu-id="c1975-218">This is detailed in the following code:</span></span>

[!code-csharp[](mvc/sample/Startup.cs?highlight=19-22)]

<span data-ttu-id="c1975-219">ASP.NET Core HTTP 500 hata yanıtları bir web uygulaması işlenmemiş özel durumlardan dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="c1975-219">ASP.NET Core converts unhandled exceptions in a web app into HTTP 500 error responses.</span></span> <span data-ttu-id="c1975-220">Normalde, hata ayrıntıları sunucu ile ilgili olası hassas bilgilerin açığa çıkmasını önlemek için bu yanıtları dahil değildir.</span><span class="sxs-lookup"><span data-stu-id="c1975-220">Normally, error details aren't included in these responses to prevent disclosure of potentially sensitive information about the server.</span></span> <span data-ttu-id="c1975-221">Bkz: **Geliştirici özel durum sayfasını kullanarak** içinde [işleme hataları](../fundamentals/error-handling.md) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="c1975-221">See **Using the Developer Exception Page** in [Handle errors](../fundamentals/error-handling.md) for more information.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c1975-222">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="c1975-222">Additional resources</span></span>

* [<span data-ttu-id="c1975-223">İstemci tarafı geliştirme</span><span class="sxs-lookup"><span data-stu-id="c1975-223">Client-side development</span></span>](xref:client-side/index)
* [<span data-ttu-id="c1975-224">Etiket Yardımcıları</span><span class="sxs-lookup"><span data-stu-id="c1975-224">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
