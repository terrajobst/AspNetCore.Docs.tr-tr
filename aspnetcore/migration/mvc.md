---
title: "ASP.NET MVC ASP.NET Core MVC geçirme"
author: ardalis
description: 
ms.author: riande
manager: wpickett
ms.date: 03/07/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/mvc
ms.openlocfilehash: e3220fb32900aac42cf96497964936ad5b375a86
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
# <a name="migrating-from-aspnet-mvc-to-aspnet-core-mvc"></a><span data-ttu-id="6c522-102">ASP.NET MVC ASP.NET Core MVC geçirme</span><span class="sxs-lookup"><span data-stu-id="6c522-102">Migrating From ASP.NET MVC to ASP.NET Core MVC</span></span>

<span data-ttu-id="6c522-103">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), [Steve Smith](https://ardalis.com/), ve [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="6c522-103">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), [Steve Smith](https://ardalis.com/), and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="6c522-104">Bu makalede, ASP.NET MVC projesinde geçirme başlamak gösterilmiştir [ASP.NET Core MVC](../mvc/overview.md).</span><span class="sxs-lookup"><span data-stu-id="6c522-104">This article shows how to get started migrating an ASP.NET MVC project to [ASP.NET Core MVC](../mvc/overview.md).</span></span> <span data-ttu-id="6c522-105">İşlem sırasında çoğunu ASP.NET MVC değişen vurgular.</span><span class="sxs-lookup"><span data-stu-id="6c522-105">In the process, it highlights many of the things that have changed from ASP.NET MVC.</span></span> <span data-ttu-id="6c522-106">ASP.NET MVC geçiş birden çok adım bir işlemdir ve bu makalede, ilk Kurulum, temel denetleyicileri ve görünümler, statik içerik ve istemci tarafı bağımlılıkları yer almaktadır.</span><span class="sxs-lookup"><span data-stu-id="6c522-106">Migrating from ASP.NET MVC is a multiple step process and this article covers the initial setup, basic controllers and views, static content, and client-side dependencies.</span></span> <span data-ttu-id="6c522-107">Diğer makaleler geçirme yapılandırması ve kimlik kodu birçok ASP.NET MVC projelerinde bulunamadı kapsar.</span><span class="sxs-lookup"><span data-stu-id="6c522-107">Additional articles cover migrating configuration and identity code found in many ASP.NET MVC projects.</span></span>

> [!NOTE]
> <span data-ttu-id="6c522-108">Sürüm numaraları örneklerdeki geçerli olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="6c522-108">The version numbers in the samples might not be current.</span></span> <span data-ttu-id="6c522-109">Buna göre projelerinizi güncelleştirmeniz gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="6c522-109">You may need to update your projects accordingly.</span></span>

## <a name="create-the-starter-aspnet-mvc-project"></a><span data-ttu-id="6c522-110">ASP.NET MVC projesinin starter oluşturma</span><span class="sxs-lookup"><span data-stu-id="6c522-110">Create the starter ASP.NET MVC project</span></span>

<span data-ttu-id="6c522-111">Yükseltme göstermek için bir ASP.NET MVC uygulaması oluşturarak başlayacağız.</span><span class="sxs-lookup"><span data-stu-id="6c522-111">To demonstrate the upgrade, we'll start by creating a ASP.NET MVC app.</span></span> <span data-ttu-id="6c522-112">Adlı oluşturun *WebApp1* ad alanı sonraki adımda oluşturuyoruz ASP.NET Core proje eşleşecek şekilde.</span><span class="sxs-lookup"><span data-stu-id="6c522-112">Create it with the name *WebApp1* so the namespace will match the ASP.NET Core project we create in the next step.</span></span>

![Visual Studio yeni proje iletişim kutusu](mvc/_static/new-project.png)

![Yeni Web uygulaması iletişim kutusu: ASP.NET şablonları panelinde seçili MVC proje şablonu](mvc/_static/new-project-select-mvc-template.png)

<span data-ttu-id="6c522-115">*İsteğe bağlı:* çözümden adını değiştirmek *WebApp1* için *Mvc5*.</span><span class="sxs-lookup"><span data-stu-id="6c522-115">*Optional:* Change the name of the Solution from *WebApp1* to *Mvc5*.</span></span> <span data-ttu-id="6c522-116">Visual Studio yeni çözüm adı görüntülenir (*Mvc5*), hangi kolaylaştırır sonraki proje bu projeden söyleyin.</span><span class="sxs-lookup"><span data-stu-id="6c522-116">Visual Studio will display the new solution name (*Mvc5*), which will make it easier to tell this project from the next project.</span></span>

## <a name="create-the-aspnet-core-project"></a><span data-ttu-id="6c522-117">ASP.NET Core projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="6c522-117">Create the ASP.NET Core project</span></span>

<span data-ttu-id="6c522-118">Yeni bir *boş* ASP.NET Core web uygulaması önceki projesi olarak aynı ada sahip (*WebApp1*) ad alanları iki proje ile eşleşecek şekilde.</span><span class="sxs-lookup"><span data-stu-id="6c522-118">Create a new *empty* ASP.NET Core web app with the same name as the previous project (*WebApp1*) so the namespaces in the two projects match.</span></span> <span data-ttu-id="6c522-119">Aynı ad alanına sahip iki projeler arasında kodu kopyalamak kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="6c522-119">Having the same namespace makes it easier to copy code between the two projects.</span></span> <span data-ttu-id="6c522-120">Aynı adı kullanmak üzere önceki proje daha farklı bir dizinde bu projeyi oluşturmak zorunda kalırsınız.</span><span class="sxs-lookup"><span data-stu-id="6c522-120">You'll have to create this project in a different directory than the previous project to use the same name.</span></span>

![Yeni Proje iletişim kutusu](mvc/_static/new_core.png)

![Yeni ASP.NET Web uygulaması iletişim kutusu: ASP.NET Core şablonları panelinde seçili boş proje şablonu](mvc/_static/new-project-select-empty-aspnet5-template.png)

* <span data-ttu-id="6c522-123">*İsteğe bağlı:* kullanarak yeni bir ASP.NET Core uygulama oluştur *Web uygulaması* proje şablonu.</span><span class="sxs-lookup"><span data-stu-id="6c522-123">*Optional:* Create a new ASP.NET Core app using the *Web Application* project template.</span></span> <span data-ttu-id="6c522-124">Proje adı *WebApp1*, bir kimlik doğrulama seçeneğini seçip **tek tek kullanıcı hesaplarını**.</span><span class="sxs-lookup"><span data-stu-id="6c522-124">Name the project *WebApp1*, and select an authentication option of **Individual User Accounts**.</span></span> <span data-ttu-id="6c522-125">Bu uygulama yeniden adlandırma *FullAspNetCore*.</span><span class="sxs-lookup"><span data-stu-id="6c522-125">Rename this app to *FullAspNetCore*.</span></span> <span data-ttu-id="6c522-126">Bu proje zaman kazanabilirsiniz dönüştürmede oluşturuluyor.</span><span class="sxs-lookup"><span data-stu-id="6c522-126">Creating this project will save you time in the conversion.</span></span> <span data-ttu-id="6c522-127">Sonuç görmek için veya dönüştürme projeye kodu kopyalamak için ' şablon oluşturulan kod bakabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6c522-127">You can look at the template-generated code to see the end result or to copy code to the conversion project.</span></span> <span data-ttu-id="6c522-128">Şablon tarafından oluşturulan proje ile karşılaştırmak için dönüştürme adım sıkışan gerektiğinde de yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="6c522-128">It's also helpful when you get stuck on a conversion step to compare with the template-generated project.</span></span>

## <a name="configure-the-site-to-use-mvc"></a><span data-ttu-id="6c522-129">Siteyi MVC kullanacak şekilde yapılandırma</span><span class="sxs-lookup"><span data-stu-id="6c522-129">Configure the site to use MVC</span></span>

* <span data-ttu-id="6c522-130">Yükleme `Microsoft.AspNetCore.Mvc` ve `Microsoft.AspNetCore.StaticFiles` NuGet paketleri.</span><span class="sxs-lookup"><span data-stu-id="6c522-130">Install the `Microsoft.AspNetCore.Mvc` and `Microsoft.AspNetCore.StaticFiles` NuGet packages.</span></span>

  <span data-ttu-id="6c522-131">`Microsoft.AspNetCore.Mvc`ASP.NET Core MVC çerçevedir.</span><span class="sxs-lookup"><span data-stu-id="6c522-131">`Microsoft.AspNetCore.Mvc` is the ASP.NET Core MVC framework.</span></span> <span data-ttu-id="6c522-132">`Microsoft.AspNetCore.StaticFiles`statik dosya işleyici olur.</span><span class="sxs-lookup"><span data-stu-id="6c522-132">`Microsoft.AspNetCore.StaticFiles` is the static file handler.</span></span> <span data-ttu-id="6c522-133">ASP.NET çalışma zamanı modüler ve açıkça statik dosyaları işleme için kabul gerekir (bkz [statik dosyaları ile çalışma](../fundamentals/static-files.md)).</span><span class="sxs-lookup"><span data-stu-id="6c522-133">The ASP.NET runtime is modular, and you must explicitly opt in to serve static files (see [Working with Static Files](../fundamentals/static-files.md)).</span></span>

* <span data-ttu-id="6c522-134">Açık *.csproj* dosyası ('nde projeye sağ **Çözüm Gezgini** seçip **Düzenle WebApp1.csproj**) ve ekleme bir `PrepareForPublish` hedef:</span><span class="sxs-lookup"><span data-stu-id="6c522-134">Open the *.csproj* file (right-click the project in **Solution Explorer** and select **Edit WebApp1.csproj**) and add a `PrepareForPublish` target:</span></span>

  [!code-xml[Main](mvc/sample/WebApp1.csproj?range=21-23)]

  <span data-ttu-id="6c522-135">`PrepareForPublish` Hedef istemci-tarafı kitaplıkları Bower aracılığıyla alınırken için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="6c522-135">The `PrepareForPublish` target is needed for acquiring client-side libraries via Bower.</span></span> <span data-ttu-id="6c522-136">Biz hakkında daha sonra konuşun.</span><span class="sxs-lookup"><span data-stu-id="6c522-136">We'll talk about that later.</span></span>

* <span data-ttu-id="6c522-137">Açık *haline* dosya ve kodu aşağıdaki ile eşleşecek şekilde değiştirin:</span><span class="sxs-lookup"><span data-stu-id="6c522-137">Open the *Startup.cs* file and change the code to match the following:</span></span>

  [!code-csharp[Main](mvc/sample/Startup.cs?highlight=14,27-34)]

  <span data-ttu-id="6c522-138">`UseStaticFiles` Genişletme yöntemi statik dosya işleyici ekler.</span><span class="sxs-lookup"><span data-stu-id="6c522-138">The `UseStaticFiles` extension method adds the static file handler.</span></span> <span data-ttu-id="6c522-139">Daha önce belirtildiği gibi ASP.NET çalışma zamanı modüler ve açıkça statik dosyaları işleme için kabul gerekir.</span><span class="sxs-lookup"><span data-stu-id="6c522-139">As mentioned previously, the ASP.NET runtime is modular, and you must explicitly opt in to serve static files.</span></span> <span data-ttu-id="6c522-140">`UseMvc` Genişletme yöntemi ekler yönlendirme.</span><span class="sxs-lookup"><span data-stu-id="6c522-140">The `UseMvc` extension method adds routing.</span></span> <span data-ttu-id="6c522-141">Daha fazla bilgi için bkz: [uygulama başlangıç](../fundamentals/startup.md) ve [yönlendirme](../fundamentals/routing.md).</span><span class="sxs-lookup"><span data-stu-id="6c522-141">For more information, see [Application Startup](../fundamentals/startup.md) and [Routing](../fundamentals/routing.md).</span></span>

## <a name="add-a-controller-and-view"></a><span data-ttu-id="6c522-142">Bir denetleyici ve Görünüm Ekle</span><span class="sxs-lookup"><span data-stu-id="6c522-142">Add a controller and view</span></span>

<span data-ttu-id="6c522-143">Bu bölümde, bir en az denetleyici ve görünüm ASP.NET MVC denetleyicisi yer tutucular olarak hizmet verecek ve sonraki bölümde geçirmek görünümleri ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="6c522-143">In this section, you'll add a minimal controller and view to serve as placeholders for the ASP.NET MVC controller and views you'll migrate in the next section.</span></span>

* <span data-ttu-id="6c522-144">Ekleme bir *denetleyicileri* klasör.</span><span class="sxs-lookup"><span data-stu-id="6c522-144">Add a *Controllers* folder.</span></span>

* <span data-ttu-id="6c522-145">Ekleme bir **MVC denetleyicisi sınıfı** adıyla *HomeController.cs* için *denetleyicileri* klasör.</span><span class="sxs-lookup"><span data-stu-id="6c522-145">Add an **MVC controller class** with the name *HomeController.cs* to the *Controllers* folder.</span></span>

![Yeni öğe iletişim ekleyin](mvc/_static/add_mvc_ctl.png)

* <span data-ttu-id="6c522-147">Ekleme bir *görünümleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="6c522-147">Add a *Views* folder.</span></span>

* <span data-ttu-id="6c522-148">Ekleme bir *görünümler/giriş* klasör.</span><span class="sxs-lookup"><span data-stu-id="6c522-148">Add a *Views/Home* folder.</span></span>

* <span data-ttu-id="6c522-149">Ekleme bir *Index.cshtml* MVC görünüm sayfası *görünümler/giriş* klasör.</span><span class="sxs-lookup"><span data-stu-id="6c522-149">Add an *Index.cshtml* MVC view page to the *Views/Home* folder.</span></span>

![Yeni öğe iletişim ekleyin](mvc/_static/view.png)

<span data-ttu-id="6c522-151">Proje yapısı aşağıda gösterilmiştir:</span><span class="sxs-lookup"><span data-stu-id="6c522-151">The project structure is shown below:</span></span>

![Dosya ve klasörleri WebApp1 gösteren Çözüm Gezgini](mvc/_static/project-structure-controller-view.png)

<span data-ttu-id="6c522-153">Değiştir *Views/Home/Index.cshtml* aşağıdaki dosyasıyla:</span><span class="sxs-lookup"><span data-stu-id="6c522-153">Replace the contents of the *Views/Home/Index.cshtml* file with the following:</span></span>

```html
<h1>Hello world!</h1>
```

<span data-ttu-id="6c522-154">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="6c522-154">Run the app.</span></span>

![Web uygulaması Microsoft Edge'de Aç](mvc/_static/hello-world.png)

<span data-ttu-id="6c522-156">Bkz: [denetleyicileri](../mvc/controllers/index.md) ve [görünümleri](../mvc/views/index.md) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="6c522-156">See [Controllers](../mvc/controllers/index.md) and [Views](../mvc/views/index.md) for more information.</span></span>

<span data-ttu-id="6c522-157">En az bir çalışma ASP.NET Core proje sahibiz, biz işlevselliği ASP.NET MVC projesinden geçiş başlatabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6c522-157">Now that we have a minimal working ASP.NET Core project, we can start migrating functionality from the ASP.NET MVC project.</span></span> <span data-ttu-id="6c522-158">Aşağıdaki taşımanız gerekebilir:</span><span class="sxs-lookup"><span data-stu-id="6c522-158">We will need to move the following:</span></span>

* <span data-ttu-id="6c522-159">istemci-tarafı içeriği (CSS, yazı tipleri ve komut dosyaları)</span><span class="sxs-lookup"><span data-stu-id="6c522-159">client-side content (CSS, fonts, and scripts)</span></span>

* <span data-ttu-id="6c522-160">denetleyiciler</span><span class="sxs-lookup"><span data-stu-id="6c522-160">controllers</span></span>

* <span data-ttu-id="6c522-161">görünümler</span><span class="sxs-lookup"><span data-stu-id="6c522-161">views</span></span>

* <span data-ttu-id="6c522-162">modeller</span><span class="sxs-lookup"><span data-stu-id="6c522-162">models</span></span>

* <span data-ttu-id="6c522-163">Paketleme</span><span class="sxs-lookup"><span data-stu-id="6c522-163">bundling</span></span>

* <span data-ttu-id="6c522-164">filtreler</span><span class="sxs-lookup"><span data-stu-id="6c522-164">filters</span></span>

* <span data-ttu-id="6c522-165">Giriş/Çıkış günlüğü, kimlik (Bu sonraki öğreticide yapılır.)</span><span class="sxs-lookup"><span data-stu-id="6c522-165">Log in/out, identity (This will be done in the next tutorial.)</span></span>

## <a name="controllers-and-views"></a><span data-ttu-id="6c522-166">Denetleyicileri ve görünümler</span><span class="sxs-lookup"><span data-stu-id="6c522-166">Controllers and views</span></span>

* <span data-ttu-id="6c522-167">Yöntemlerin her biri ASP.NET MVC kopyalama `HomeController` yeni `HomeController`.</span><span class="sxs-lookup"><span data-stu-id="6c522-167">Copy each of the methods from the ASP.NET MVC `HomeController` to the new `HomeController`.</span></span> <span data-ttu-id="6c522-168">ASP.NET MVC uygulamasında yerleşik şablonun denetleyici eylem yönteminin dönüş türü olduğunu unutmayın [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); ASP.NET mvc'de çekirdek, dönüş eylem yöntemleri `IActionResult` yerine.</span><span class="sxs-lookup"><span data-stu-id="6c522-168">Note that in ASP.NET MVC, the built-in template's controller action method return type is [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); in ASP.NET Core MVC, the action methods return `IActionResult` instead.</span></span> <span data-ttu-id="6c522-169">`ActionResult`uygulayan `IActionResult`, bu yüzden, eylem yöntemleri dönüş türü değiştirmenize gerek yoktur.</span><span class="sxs-lookup"><span data-stu-id="6c522-169">`ActionResult` implements `IActionResult`, so there's no need to change the return type of your action methods.</span></span>

* <span data-ttu-id="6c522-170">Kopya *About.cshtml*, *Contact.cshtml*, ve *Index.cshtml* ASP.NET Core proje için ASP.NET MVC projesindeki Razor görünüm dosyaları.</span><span class="sxs-lookup"><span data-stu-id="6c522-170">Copy the *About.cshtml*, *Contact.cshtml*, and *Index.cshtml* Razor view files from the ASP.NET MVC project to the ASP.NET Core project.</span></span>

* <span data-ttu-id="6c522-171">ASP.NET Core uygulamayı çalıştırın ve her yöntemi sınayın.</span><span class="sxs-lookup"><span data-stu-id="6c522-171">Run the ASP.NET Core app and test each method.</span></span> <span data-ttu-id="6c522-172">İşlenmiş görünüm yalnızca görünüm dosyalarının içeriği içerecek şekilde biz düzeni dosyasını veya stiller henüz, geçirilen henüz.</span><span class="sxs-lookup"><span data-stu-id="6c522-172">We haven't migrated the layout file or styles yet, so the rendered views will only contain the content in the view files.</span></span> <span data-ttu-id="6c522-173">Düzen oluşturulan dosyanın bağlantılarını olmayacaktır `About` ve `Contact` tarayıcıdan çağırma gerekecek şekilde görünümler (Değiştir **4492** projenizde kullanılan bağlantı noktası numarası ile).</span><span class="sxs-lookup"><span data-stu-id="6c522-173">You won't have the layout file generated links for the `About` and `Contact` views, so you'll have to invoke them from the browser (replace **4492** with the port number used in your project).</span></span>

  * `http://localhost:4492/home/about`

  * `http://localhost:4492/home/contact`

![Kişi sayfası](mvc/_static/contact-page.png)

<span data-ttu-id="6c522-175">Stil ve menü öğelerini eksikliği unutmayın.</span><span class="sxs-lookup"><span data-stu-id="6c522-175">Note the lack of styling and menu items.</span></span> <span data-ttu-id="6c522-176">Biz, sonraki bölümde düzeltmesi.</span><span class="sxs-lookup"><span data-stu-id="6c522-176">We'll fix that in the next section.</span></span>

## <a name="static-content"></a><span data-ttu-id="6c522-177">Statik içerik</span><span class="sxs-lookup"><span data-stu-id="6c522-177">Static content</span></span>

<span data-ttu-id="6c522-178">ASP.NET MVC önceki sürümlerinde, statik içerik web projesi kökünden barındırılan ve sunucu tarafı dosyalarıyla intermixed.</span><span class="sxs-lookup"><span data-stu-id="6c522-178">In previous versions of  ASP.NET MVC, static content was hosted from the root of the web project and was intermixed with server-side files.</span></span> <span data-ttu-id="6c522-179">ASP.NET çekirdek statik içerik içinde barındırılan *wwwroot* klasör.</span><span class="sxs-lookup"><span data-stu-id="6c522-179">In ASP.NET Core, static content is hosted in the *wwwroot* folder.</span></span> <span data-ttu-id="6c522-180">Statik içerik eski ASP.NET MVC uygulamanıza kopyalamak istersiniz *wwwroot* ASP.NET Core projenizdeki klasöre.</span><span class="sxs-lookup"><span data-stu-id="6c522-180">You'll want to copy the static content from your old ASP.NET MVC app to the *wwwroot* folder in your ASP.NET Core project.</span></span> <span data-ttu-id="6c522-181">Bu örnek dönüştürmede:</span><span class="sxs-lookup"><span data-stu-id="6c522-181">In this sample conversion:</span></span>

* <span data-ttu-id="6c522-182">Kopya *favicon.ico* eski MVC proje dosyasından *wwwroot* ASP.NET Core proje klasöründe.</span><span class="sxs-lookup"><span data-stu-id="6c522-182">Copy the *favicon.ico* file from the old MVC project to the *wwwroot* folder in the ASP.NET Core project.</span></span>

<span data-ttu-id="6c522-183">ASP.NET MVC eski proje kullanır [önyükleme](http://getbootstrap.com/) önyükleme dosyaları stil ve depoları *içerik* ve *betikleri* klasörler.</span><span class="sxs-lookup"><span data-stu-id="6c522-183">The old ASP.NET MVC project uses [Bootstrap](http://getbootstrap.com/) for its styling and stores the Bootstrap files in the *Content* and *Scripts* folders.</span></span> <span data-ttu-id="6c522-184">Önyükleme düzeni dosyasında eski ASP.NET MVC projesinin oluşturulan, şablonu başvuruyor (*Views/Shared/_Layout.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="6c522-184">The template, which generated the old ASP.NET MVC project, references Bootstrap in the layout file (*Views/Shared/_Layout.cshtml*).</span></span> <span data-ttu-id="6c522-185">Kopyalamak *bootstrap.js* ve *bootstrap.css* ASP.NET MVC dosyalarından proje için *wwwroot* değil klasörünü yeni proje, ancak bu yaklaşımı kullanın ASP.NET Core istemci-tarafı bağımlılıkları yönetmek için geliştirilmiş mekanizması.</span><span class="sxs-lookup"><span data-stu-id="6c522-185">You could copy the *bootstrap.js* and *bootstrap.css* files from the ASP.NET MVC project to the *wwwroot* folder in the new project, but that approach doesn't use the improved mechanism for managing client-side dependencies in ASP.NET Core.</span></span>

<span data-ttu-id="6c522-186">Yeni projede kullanarak önyükleme (ve diğer istemci-tarafı kitaplıklar) desteği ekleyeceğiz [Bower](https://bower.io/):</span><span class="sxs-lookup"><span data-stu-id="6c522-186">In the new project, we'll add support for Bootstrap (and other client-side libraries) using [Bower](https://bower.io/):</span></span>

* <span data-ttu-id="6c522-187">Ekle bir [Bower](https://bower.io/) yapılandırma dosyası adlı *bower.json* proje kök (projeye sağ tıklayın ve ardından **Ekle > Yeni Öğe > Bower yapılandırma dosyası**).</span><span class="sxs-lookup"><span data-stu-id="6c522-187">Add a [Bower](https://bower.io/) configuration file named *bower.json* to the project root (Right-click on the project, and then **Add > New Item > Bower Configuration File**).</span></span> <span data-ttu-id="6c522-188">Ekleme [önyükleme](http://getbootstrap.com/) ve [jQuery](https://jquery.com/) dosyasına (aşağıda vurgulanan satırlar bakın).</span><span class="sxs-lookup"><span data-stu-id="6c522-188">Add [Bootstrap](http://getbootstrap.com/) and [jQuery](https://jquery.com/) to the file (see the highlighted lines below).</span></span>

  [!code-json[Main](mvc/sample/bower.json?highlight=5-6)]

<span data-ttu-id="6c522-189">Dosyayı kaydettikten sonra Bower otomatik olarak bağımlılıkları indirir *wwwroot/lib* klasör.</span><span class="sxs-lookup"><span data-stu-id="6c522-189">Upon saving the file, Bower will automatically download the dependencies to the *wwwroot/lib* folder.</span></span> <span data-ttu-id="6c522-190">Kullanabileceğiniz **arama Çözüm Gezgini** kutusunu varlıklar yolunu bulmak için:</span><span class="sxs-lookup"><span data-stu-id="6c522-190">You can use the **Search Solution Explorer** box to find the path of the assets:</span></span>

![Çözüm Gezgini arama sonuçlarında gösterilen jquery varlıklar](mvc/_static/search.png)

<span data-ttu-id="6c522-192">Bkz: [istemci-tarafı paketlerle yönetmek Bower](../client-side/bower.md) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="6c522-192">See [Manage Client-Side Packages with Bower](../client-side/bower.md) for more information.</span></span>

<a name="migrate-layout-file"></a>

## <a name="migrate-the-layout-file"></a><span data-ttu-id="6c522-193">Düzen dosyasını geçirme</span><span class="sxs-lookup"><span data-stu-id="6c522-193">Migrate the layout file</span></span>

* <span data-ttu-id="6c522-194">Kopya *_ViewStart.cshtml* eski ASP.NET MVC projesinin dosyadan *görünümleri* ASP.NET Core projenin klasörüne *görünümleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="6c522-194">Copy the *_ViewStart.cshtml* file from the old ASP.NET MVC project's *Views* folder into the ASP.NET Core project's *Views* folder.</span></span> <span data-ttu-id="6c522-195">*_ViewStart.cshtml* dosya ASP.NET Core MVC'de değişmemiştir.</span><span class="sxs-lookup"><span data-stu-id="6c522-195">The *_ViewStart.cshtml* file has not changed in ASP.NET Core MVC.</span></span>

* <span data-ttu-id="6c522-196">Oluşturma bir *görünümler/paylaşılan* klasör.</span><span class="sxs-lookup"><span data-stu-id="6c522-196">Create a *Views/Shared* folder.</span></span>

* <span data-ttu-id="6c522-197">*İsteğe bağlı:* kopya *_viewımports.cshtml* gelen *FullAspNetCore* MVC projesinin *görünümleri* ASP.NET Core projenin klasörüne*Görünümleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="6c522-197">*Optional:* Copy *_ViewImports.cshtml* from the *FullAspNetCore* MVC project's *Views* folder into the ASP.NET Core project's *Views* folder.</span></span> <span data-ttu-id="6c522-198">Tüm ad alanı bildirimini kaldırın *_viewımports.cshtml* dosya.</span><span class="sxs-lookup"><span data-stu-id="6c522-198">Remove any namespace declaration in the *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="6c522-199">*_Viewımports.cshtml* dosya ad alanları için tüm görünüm dosyaları sağlar ve getirir [etiket Yardımcıları](../mvc/views/tag-helpers/index.md).</span><span class="sxs-lookup"><span data-stu-id="6c522-199">The *_ViewImports.cshtml* file provides namespaces for all the view files and brings in [Tag Helpers](../mvc/views/tag-helpers/index.md).</span></span> <span data-ttu-id="6c522-200">Etiket Yardımcıları yeni düzen dosyasında kullanılır.</span><span class="sxs-lookup"><span data-stu-id="6c522-200">Tag Helpers are used in the new layout file.</span></span> <span data-ttu-id="6c522-201">*_Viewımports.cshtml* dosya ASP.NET Core için yenidir.</span><span class="sxs-lookup"><span data-stu-id="6c522-201">The *_ViewImports.cshtml* file is new for ASP.NET Core.</span></span>

* <span data-ttu-id="6c522-202">Kopya *_Layout.cshtml* eski ASP.NET MVC projesinin dosyadan *görünümler/paylaşılan* ASP.NET Core projenin klasörüne *görünümler/paylaşılan* klasör.</span><span class="sxs-lookup"><span data-stu-id="6c522-202">Copy the *_Layout.cshtml* file from the old ASP.NET MVC project's *Views/Shared* folder into the ASP.NET Core project's *Views/Shared* folder.</span></span>

<span data-ttu-id="6c522-203">Açık *_Layout.cshtml* dosya ve (tamamlanan kodu aşağıda gösterilmektedir) aşağıdaki değişiklikleri yapın:</span><span class="sxs-lookup"><span data-stu-id="6c522-203">Open *_Layout.cshtml* file and make the following changes (the completed code is shown below):</span></span>

   * <span data-ttu-id="6c522-204">Değiştir `@Styles.Render("~/Content/css")` ile bir `<link>` yüklemek için öğesi *bootstrap.css* (aşağıya bakın).</span><span class="sxs-lookup"><span data-stu-id="6c522-204">Replace `@Styles.Render("~/Content/css")` with a `<link>` element to load *bootstrap.css* (see below).</span></span>

   * <span data-ttu-id="6c522-205">Kaldırma `@Scripts.Render("~/bundles/modernizr")`.</span><span class="sxs-lookup"><span data-stu-id="6c522-205">Remove `@Scripts.Render("~/bundles/modernizr")`.</span></span>

   * <span data-ttu-id="6c522-206">Out açıklama `@Html.Partial("_LoginPartial")` satır (satırıyla çevreleyen `@*...*@`).</span><span class="sxs-lookup"><span data-stu-id="6c522-206">Comment out the `@Html.Partial("_LoginPartial")` line (surround the line with `@*...*@`).</span></span> <span data-ttu-id="6c522-207">Sonraki öğreticide kendisine getireceğiz.</span><span class="sxs-lookup"><span data-stu-id="6c522-207">We'll return to it in a future tutorial.</span></span>

   * <span data-ttu-id="6c522-208">Değiştir `@Scripts.Render("~/bundles/jquery")` ile bir `<script>` öğesi (aşağıya bakın).</span><span class="sxs-lookup"><span data-stu-id="6c522-208">Replace `@Scripts.Render("~/bundles/jquery")` with a `<script>` element (see below).</span></span>

   * <span data-ttu-id="6c522-209">Değiştir `@Scripts.Render("~/bundles/bootstrap")` ile bir `<script>` öğesi (aşağıya bakın)...</span><span class="sxs-lookup"><span data-stu-id="6c522-209">Replace `@Scripts.Render("~/bundles/bootstrap")` with a `<script>` element (see below)..</span></span>

<span data-ttu-id="6c522-210">Değiştirme CSS Bağlantısı:</span><span class="sxs-lookup"><span data-stu-id="6c522-210">The replacement CSS link:</span></span>

```html
<link rel="stylesheet" href="~/lib/bootstrap/dist/css/bootstrap.css" />
```

<span data-ttu-id="6c522-211">Değiştirme komut dosyası etiketi:</span><span class="sxs-lookup"><span data-stu-id="6c522-211">The replacement script tags:</span></span>

```html
<script src="~/lib/jquery/dist/jquery.js"></script>
<script src="~/lib/bootstrap/dist/js/bootstrap.js"></script>
```

<span data-ttu-id="6c522-212">Güncelleştirilmiş *_Layout.cshtml* dosya aşağıda gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="6c522-212">The updated *_Layout.cshtml* file is shown below:</span></span>

[!code-html[Main](mvc/sample/Views/Shared/_Layout.cshtml?highlight=7,27,39-40)]

<span data-ttu-id="6c522-213">Site tarayıcıda görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="6c522-213">View the site in the browser.</span></span> <span data-ttu-id="6c522-214">Artık doğru yerde beklenen stilleri ile yüklenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="6c522-214">It should now load correctly, with the expected styles in place.</span></span>

* <span data-ttu-id="6c522-215">*İsteğe bağlı:* yeni düzen dosyasını kullanmayı denemek isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6c522-215">*Optional:* You might want to try using the new layout file.</span></span> <span data-ttu-id="6c522-216">Bu proje için Düzen dosyasından kopyalayabilirsiniz *FullAspNetCore* projesi.</span><span class="sxs-lookup"><span data-stu-id="6c522-216">For this project you can copy the layout file from the *FullAspNetCore* project.</span></span> <span data-ttu-id="6c522-217">Yeni düzen dosyasını kullanan [etiket Yardımcıları](../mvc/views/tag-helpers/index.md) ve diğer geliştirmeler sahiptir.</span><span class="sxs-lookup"><span data-stu-id="6c522-217">The new layout file uses [Tag Helpers](../mvc/views/tag-helpers/index.md) and has other improvements.</span></span>

## <a name="configure-bundling--minification"></a><span data-ttu-id="6c522-218">Paketleme yapılandırmak & küçültme</span><span class="sxs-lookup"><span data-stu-id="6c522-218">Configure Bundling & Minification</span></span>

<span data-ttu-id="6c522-219">Paketleme ve küçültme yapılandırma hakkında daha fazla bilgi için bkz: [paketleme ve küçültme](../client-side/bundling-and-minification.md).</span><span class="sxs-lookup"><span data-stu-id="6c522-219">For information about how to configure bundling and minification, see [Bundling and Minification](../client-side/bundling-and-minification.md).</span></span>

## <a name="solving-http-500-errors"></a><span data-ttu-id="6c522-220">HTTP 500 hataları çözme</span><span class="sxs-lookup"><span data-stu-id="6c522-220">Solving HTTP 500 errors</span></span>

<span data-ttu-id="6c522-221">Sorunun kaynağını hakkında hiçbir bilgi içeren bir HTTP 500 hata iletisini neden olan birçok sorunları vardır.</span><span class="sxs-lookup"><span data-stu-id="6c522-221">There are many problems that can cause a HTTP 500 error message that contain no information on the source of the problem.</span></span> <span data-ttu-id="6c522-222">Örneğin, varsa *Views/_ViewImports.cshtml* dosya içeriyorsa, projede mevcut olmayan bir ad alanı, bir HTTP 500 hata iletisi alırsınız.</span><span class="sxs-lookup"><span data-stu-id="6c522-222">For example, if the *Views/_ViewImports.cshtml* file contains a namespace that doesn't exist in your project, you'll get a HTTP 500 error.</span></span> <span data-ttu-id="6c522-223">Ayrıntılı hata iletisini almak için aşağıdaki kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="6c522-223">To get a detailed error message, add the following code:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
{
    if (env.IsDevelopment())
    {
         app.UseDeveloperExceptionPage();
    }

    app.UseStaticFiles();

    app.UseMvc(routes =>
    {
        routes.MapRoute(
            name: "default",
            template: "{controller=Home}/{action=Index}/{id?}");
    });
}
```

<span data-ttu-id="6c522-224">Bkz: **Geliştirici özel durum sayfasını kullanarak** içinde [işleme hatası](../fundamentals/error-handling.md) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="6c522-224">See **Using the Developer Exception Page** in [Error Handling](../fundamentals/error-handling.md) for more information.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6c522-225">Ek Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="6c522-225">Additional Resources</span></span>

* [<span data-ttu-id="6c522-226">İstemci-tarafı geliştirme</span><span class="sxs-lookup"><span data-stu-id="6c522-226">Client-Side Development</span></span>](../client-side/index.md)

* [<span data-ttu-id="6c522-227">Etiket Yardımcıları</span><span class="sxs-lookup"><span data-stu-id="6c522-227">Tag Helpers</span></span>](../mvc/views/tag-helpers/index.md)
