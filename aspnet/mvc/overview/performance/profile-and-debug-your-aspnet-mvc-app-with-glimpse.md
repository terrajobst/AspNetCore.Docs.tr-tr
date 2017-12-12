---
uid: mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
title: "Profil ve Glimpse'in ile ASP.NET MVC uygulamanızın hatalarını ayıklama | Microsoft Docs"
author: Rick-Anderson
description: "Glimpse'in olan bir başarısız ayrıntılı performans sağlayan açık kaynak NuGet paketlerini ailesi büyüyen, hata ayıklama ve tanılama bilgilerini ASP.NET bir..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/26/2015
ms.topic: article
ms.assetid: c205805f-efdd-4fa7-9616-f26eab180611
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
msc.type: authoredcontent
ms.openlocfilehash: 98b21a54ba00a8c82c3be7ba4e39d44041ed42c6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="profile-and-debug-your-aspnet-mvc-app-with-glimpse"></a><span data-ttu-id="369d7-103">Profil ve Glimpse'in ile ASP.NET MVC uygulamanızın hatalarını ayıklama</span><span class="sxs-lookup"><span data-stu-id="369d7-103">Profile and debug your ASP.NET MVC app with Glimpse</span></span>
====================
<span data-ttu-id="369d7-104">Tarafından [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="369d7-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="369d7-105">Glimpse'in bir başarısız ayrıntılı performans sağlayan açık kaynak NuGet paketlerini ailesi büyüyen, hata ayıklama ve tanılama bilgilerini ASP.NET uygulamaları için ' dir.</span><span class="sxs-lookup"><span data-stu-id="369d7-105">Glimpse is a thriving and growing family of open source NuGet packages that provides detailed performance, debugging and diagnostic information for ASP.NET apps.</span></span> <span data-ttu-id="369d7-106">Önemsiz yüklemek için basit, son derece hızlı ve temel performans ölçümlerini her sayfasının en altında görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="369d7-106">It's trivial to install, lightweight, ultra-fast, and displays key performance metrics at the bottom of every page.</span></span> <span data-ttu-id="369d7-107">Sunucuda neler olduğunu öğrenmek gerektiğinde uygulamanıza detaya imkan tanır.</span><span class="sxs-lookup"><span data-stu-id="369d7-107">It allows you to drill down into your app when you need to find out what's going on at the server.</span></span> <span data-ttu-id="369d7-108">Glimpse'in Azure test ortamınızı dahil olmak üzere, geliştirme döngüsü boyunca kullanmanızı öneririz çok değerli bilgiler sağlar.</span><span class="sxs-lookup"><span data-stu-id="369d7-108">Glimpse provides so much valuable information we recommend you use it throughout your development cycle, including your Azure test environment.</span></span> <span data-ttu-id="369d7-109">Sırada [Fiddler](http://www.telerik.com/fiddler) ve [F-12 geliştirme araçları](https://msdn.microsoft.com/en-us/library/ie/gg589512(v=vs.85).aspx) sağlayan bir istemci tarafı görünüm Glimpse'in sunucudan ayrıntılı bir görünüm sağlar.</span><span class="sxs-lookup"><span data-stu-id="369d7-109">While [Fiddler](http://www.telerik.com/fiddler) and the [F-12 development tools](https://msdn.microsoft.com/en-us/library/ie/gg589512(v=vs.85).aspx) provide a client side view, Glimpse provides a detailed view from the server.</span></span> <span data-ttu-id="369d7-110">Bu öğretici Glimpse'in ASP.NET MVC ve EF paketleri kullanarak odaklanır, ancak diğer birçok paketleri kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="369d7-110">This tutorial will focus on using the Glimpse ASP.NET MVC and EF packages, but many other packages are available.</span></span> <span data-ttu-id="369d7-111">Mümkün olduğunda ı uygun bağlayacaksınız [Glimpse'in belgeleri](http://getglimpse.com/Docs/) hangi korunmasına yardımcı.</span><span class="sxs-lookup"><span data-stu-id="369d7-111">Where possible I will link to the appropriate [Glimpse docs](http://getglimpse.com/Docs/) which I help maintain.</span></span> <span data-ttu-id="369d7-112">Glimpse'in açık kaynaklı proje, kaynak kodu ve belgeler için çok katkıda bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="369d7-112">Glimpse is an open source project, you too can contribute to the source code and the docs.</span></span>


- [<span data-ttu-id="369d7-113">Yükleme bakışta</span><span class="sxs-lookup"><span data-stu-id="369d7-113">Installing Glimpse</span></span>](#ig)
- [<span data-ttu-id="369d7-114">Localhost için Glimpse'in etkinleştir</span><span class="sxs-lookup"><span data-stu-id="369d7-114">Enable Glimpse for localhost</span></span>](#eg)
- [<span data-ttu-id="369d7-115">Zaman Çizelgesi sekmesi</span><span class="sxs-lookup"><span data-stu-id="369d7-115">The Timeline tab</span></span>](#Time)
- [<span data-ttu-id="369d7-116">Model bağlama</span><span class="sxs-lookup"><span data-stu-id="369d7-116">Model Binding</span></span>](#mb)
- [<span data-ttu-id="369d7-117">Yollar</span><span class="sxs-lookup"><span data-stu-id="369d7-117">Routes</span></span>](#route)
- [<span data-ttu-id="369d7-118">Azure üzerinde Glimpse'in kullanma</span><span class="sxs-lookup"><span data-stu-id="369d7-118">Using Glimpse on Azure</span></span>](#da)
- [<span data-ttu-id="369d7-119">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="369d7-119">Additional Resources</span></span>](#addRes)

<a id="ig"></a>
## <a name="installing-glimpse"></a><span data-ttu-id="369d7-120">Yükleme bakışta</span><span class="sxs-lookup"><span data-stu-id="369d7-120">Installing Glimpse</span></span>

<span data-ttu-id="369d7-121">NuGet Paket Yöneticisi Konsolu'ndan veya Glimpse'in yükleyebilirsiniz **NuGet paketlerini Yönet** konsol.</span><span class="sxs-lookup"><span data-stu-id="369d7-121">You can install Glimpse from the NuGet package manager console or from the **Manage NuGet Packages** console.</span></span> <span data-ttu-id="369d7-122">Bu Tanıtım için ı Mvc5 ve EF6 paketleri yükleyeceksiniz:</span><span class="sxs-lookup"><span data-stu-id="369d7-122">For this demo, I'll install the Mvc5 and EF6 packages:</span></span>

![NuGet iletişim kutusu Glimpse'in yükleyin](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image1.png)

<span data-ttu-id="369d7-124">Arama *Glimpse.EF*</span><span class="sxs-lookup"><span data-stu-id="369d7-124">Search for *Glimpse.EF*</span></span>

![Glimpse.EF gelen NuGet Yükle iletişim kutusu](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image2.png)

<span data-ttu-id="369d7-126">Seçerek **yüklü paketleri**, yüklü Glimpse'in bağımlı modülleri görebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="369d7-126">By selecting **Installed packages**, you can see the Glimpse dependent modules installed:</span></span>

![İletişim kutusu Glimpse'in paketleri yüklü](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image3.png)

<span data-ttu-id="369d7-128">Aşağıdaki komutlar Paket Yöneticisi Konsolu'ndan Glimpse'in MVC5 ve EF6 modüllerini yükleyin:</span><span class="sxs-lookup"><span data-stu-id="369d7-128">The following commands install Glimpse MVC5 and EF6 modules from the package manager console:</span></span>

[!code-console[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample1.cmd)]

<a id="eg"></a>
## <a name="enable-glimpse-for-localhost"></a><span data-ttu-id="369d7-129">Localhost için Glimpse'in etkinleştir</span><span class="sxs-lookup"><span data-stu-id="369d7-129">Enable Glimpse for localhost</span></span>

<span data-ttu-id="369d7-130">Http://localhost için gidin:&lt;bağlantı noktası #&gt;tıklayın ve /glimpse.axd **Glimpse'in Aç** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="369d7-130">Navigate to http://localhost:&lt;port #&gt;/glimpse.axd and click the **Turn Glimpse On** button.</span></span>

![Glimpse'in axd sayfası](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image4.png)

<span data-ttu-id="369d7-132">Görüntülenen, Sık Kullanılanlar çubuğuna varsa, sürükleyin ve Glimpse'in düğmeleri bırakın ve bunları bookmarklets Ekle:</span><span class="sxs-lookup"><span data-stu-id="369d7-132">If you have your favorites bar displayed, you can drag and drop the Glimpse buttons and add them as bookmarklets:</span></span>

![IE Glimpse'in boookmarklets ile](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image5.png)

<span data-ttu-id="369d7-134">Şimdi, uygulamanızın gidebilirsiniz ve **kafa yukarı görüntü** (HUD), sayfasının en altında gösterilir.</span><span class="sxs-lookup"><span data-stu-id="369d7-134">You can now navigate your app, and the **Heads Up Display** (HUD) is shown at the bottom of the page.</span></span>

![HUD ile ilgili kişi Yöneticisi sayfası](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image6.png)

<span data-ttu-id="369d7-136">[Glimpse'in HUD sayfa](http://getglimpse.com/Docs/Heads-up-Display) yukarıda gösterilen zamanlama bilgisini ayrıntıları.</span><span class="sxs-lookup"><span data-stu-id="369d7-136">The [Glimpse HUD page](http://getglimpse.com/Docs/Heads-up-Display) details the timing information shown above.</span></span> <span data-ttu-id="369d7-137">Test döngüsü ulaşmadan örtük performans verileri HUD görüntüler, bir sorunun hemen - bildirebilir.</span><span class="sxs-lookup"><span data-stu-id="369d7-137">The unobtrusive performance data the HUD displays can notify you of a problem immediately - before you get to the test cycle.</span></span> <span data-ttu-id="369d7-138">Tıklayarak &quot;g&quot; Glimpse'in bölmenin sağ alt köşedeki getirir:</span><span class="sxs-lookup"><span data-stu-id="369d7-138">Clicking on the &quot;g&quot; in the lower right corner brings up the Glimpse panel:</span></span>

![Glimpse'in paneli](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image7.png)

<span data-ttu-id="369d7-140">Yukarıdaki görüntüsündeki [yürütme sekmesi](http://getglimpse.com/Docs/Execution-Tab) seçildiğinde, ardışık düzeninde Eylemler ve filtreleri zamanlama ayrıntılarını gösterir.</span><span class="sxs-lookup"><span data-stu-id="369d7-140">In the image above, the [Execution tab](http://getglimpse.com/Docs/Execution-Tab) is selected, which shows timing details of the actions and filters in the pipeline.</span></span> <span data-ttu-id="369d7-141">Gördüğünüz my [durdurmak izleme filtresi Zamanlayıcı](http://www.nuget.org/packages/StopWatch/) ardışık 6 aşamada başlatın.</span><span class="sxs-lookup"><span data-stu-id="369d7-141">You can see my [Stop Watch filter timer](http://www.nuget.org/packages/StopWatch/) start at stage 6 of the pipeline.</span></span> <span data-ttu-id="369d7-142">My hafif Zamanlayıcı sağlayabilir ancak yararlı veri profili/zamanlama, yetkilendirme harcanan ve görünüm işlemeyle her zaman isabetsiz okuma.</span><span class="sxs-lookup"><span data-stu-id="369d7-142">While my light weight timer can provide useful profile/timing data, it misses all the time spent in authorization and rendering the view.</span></span> <span data-ttu-id="369d7-143">My Zamanlayıcısı hakkında bilgi edinebilirsiniz [profil ve ASP.NET MVC uygulamanıza tüm Azure saat](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx).</span><span class="sxs-lookup"><span data-stu-id="369d7-143">You can read about my timer at [Profile and Time your ASP.NET MVC app all the way to Azure](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx).</span></span> <span data-ttu-id="369d7-144">[Sekmeleri](http://getglimpse.com/Docs/Tabs) sayfası, her bir sekmede ayrıntılı bilgi için bağlantılar sağlar.</span><span class="sxs-lookup"><span data-stu-id="369d7-144">The [Tabs](http://getglimpse.com/Docs/Tabs) page provides links to detailed information on each tab.</span></span>

<a id="Time"></a>
## <a name="the-timeline-tab"></a><span data-ttu-id="369d7-145">Zaman Çizelgesi sekmesi</span><span class="sxs-lookup"><span data-stu-id="369d7-145">The Timeline tab</span></span>

<span data-ttu-id="369d7-146">Zel Dykstra'nın değiştiren bekleyen [EF 6/MVC 5 Öğreticisi](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) Eğitmen denetleyiciye aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="369d7-146">I've modified Tom Dykstra's outstanding [EF 6/MVC 5 tutorial](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) with the following code change to the instructors controller:</span></span>

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample2.cs?highlight=1,20-31)]

<span data-ttu-id="369d7-147">Yukarıdaki kod bana sorgu dizesine geçmenizi sağlar (`eager`) istekli veya açık denetim verileri yükleniyor.</span><span class="sxs-lookup"><span data-stu-id="369d7-147">The code above allows me to pass in query string (`eager`) to control eager or explicit loading of data.</span></span> <span data-ttu-id="369d7-148">Aşağıdaki resimde, açık yükleme kullanılır ve zamanlama sayfası yüklenmiş her kayıt gösterir `Index` eylem yöntemi:</span><span class="sxs-lookup"><span data-stu-id="369d7-148">In the image below, explicit loading is used and the timing page shows each enrollment loaded in the `Index` action method:</span></span>

![Açık yükleniyor](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image8.png)

<span data-ttu-id="369d7-150">Aşağıdaki kodda eager belirtilir ve sonra her kayıt getirilen `Index` görünüm çağrılır:</span><span class="sxs-lookup"><span data-stu-id="369d7-150">In the following code, eager is specified, and each enrollment is fetched after the `Index` view is called:</span></span>

![eager belirtilen](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image9.png)

<span data-ttu-id="369d7-152">Ayrıntılı zamanlama bilgilerini almak için zaman diliminin getirin:</span><span class="sxs-lookup"><span data-stu-id="369d7-152">You can hover over a time segment to get detailed timing information:</span></span>

![ayrıntılı zamanlama görmek için vurgulu](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image10.png)

<a id="mb"></a>
## <a name="model-binding"></a><span data-ttu-id="369d7-154">Model bağlama</span><span class="sxs-lookup"><span data-stu-id="369d7-154">Model Binding</span></span>

<span data-ttu-id="369d7-155">[Model bağlama sekmesini](http://getglimpse.com/Docs/Model-Binding-Tab) bol miktarda form değişkeni nasıl bağlı ve beklediğiniz gibi neden bazı bağlı olmayan anlamanıza yardımcı olacak bilgiler sağlar.</span><span class="sxs-lookup"><span data-stu-id="369d7-155">The [model binding tab](http://getglimpse.com/Docs/Model-Binding-Tab) provides a wealth of information to help you understand how your form variables are bound and why some are not bound as would expect.</span></span> <span data-ttu-id="369d7-156">Görüntünün gösterir aşağıda **?**</span><span class="sxs-lookup"><span data-stu-id="369d7-156">The image below shows the **?**</span></span> <span data-ttu-id="369d7-157">simge özellik glimpse'in Yardım sayfasını getirmek için tıklatabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="369d7-157">icon, which you can click on to bring up the glimpse help page for that feature.</span></span>

![Model bağlama görünümü glimpse'in](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image11.png)

<a id="route"></a>
## <a name="routes"></a><span data-ttu-id="369d7-159">Yollar</span><span class="sxs-lookup"><span data-stu-id="369d7-159">Routes</span></span>

 <span data-ttu-id="369d7-160">Glimpse'in yollar sekmesini hata ayıklama ve yönlendirme anlamanıza yardımcı olabilir.</span><span class="sxs-lookup"><span data-stu-id="369d7-160">The Glimpse Routes tab will can help you debug and understand routing.</span></span> <span data-ttu-id="369d7-161">Aşağıdaki görüntü, ürün yol seçilir (ve yeşil, bir bakışta kuralı gösterir).</span><span class="sxs-lookup"><span data-stu-id="369d7-161">In the image below, the product route is selected (and it shows in green, a Glimpse convention).</span></span> <span data-ttu-id="369d7-162">![Seçilen ürün adı](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png) rota kısıtlamaları, alanlar ve veri belirteçleri de görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="369d7-162">![product name selected](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png) Route constraints, Areas and data tokens are also displayed.</span></span> <span data-ttu-id="369d7-163">Bkz: [Glimpse'in yollar](http://getglimpse.com/Docs/Routes-Tab) ve [özniteliği yönlendirme ASP.NET MVC 5'te](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="369d7-163">See [Glimpse Routes](http://getglimpse.com/Docs/Routes-Tab) and [Attribute Routing in ASP.NET MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx) for more information.</span></span> 

<a id="da"></a>
## <a name="using-glimpse-on-azure"></a><span data-ttu-id="369d7-164">Azure üzerinde Glimpse'in kullanma</span><span class="sxs-lookup"><span data-stu-id="369d7-164">Using Glimpse on Azure</span></span>

<span data-ttu-id="369d7-165">Glimpse'in varsayılan güvenlik ilkesi, yalnızca yerel ana bilgisayardan görüntülenecek Glimpse'in verileri sağlar.</span><span class="sxs-lookup"><span data-stu-id="369d7-165">The Glimpse default security policy only allows Glimpse data to be displayed from local host.</span></span> <span data-ttu-id="369d7-166">Uzak bir sunucuda (örneğin, Azure üzerinde bir web uygulaması) bu veri görüntüleyebilmeniz için bu güvenlik ilkesini değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="369d7-166">You can change this security policy so you can view this data on a remote server (such as a web app on Azure).</span></span> <span data-ttu-id="369d7-167">Azure üzerinde test ortamları için sonuna kadar vurgulanan işareti ekleme *Web.config* dosya Glimpse'in etkinleştirmek için:</span><span class="sxs-lookup"><span data-stu-id="369d7-167">For test environments on Azure, add the highlighted mark up to the bottom of the *web.confg* file to enable Glimpse:</span></span>

[!code-xml[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample3.xml?highlight=2-6)]

<span data-ttu-id="369d7-168">Bu değişiklik tek başına, herhangi bir kullanıcı, uzak bir siteye Glimpse'in verilerinizi görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="369d7-168">With this change alone, any user can see your Glimpse data on a remote site.</span></span> <span data-ttu-id="369d7-169">Bu yayımlama profili (örneğin, Azure test proifle.) kullandığınızda, yalnızca bir uygulanan dağıtıldığını şekilde biçimlendirme yukarıda bir yayımlama profili eklemeyi düşünün Glimpse'in verileri kısıtlamak için ekleyeceğiz `canViewGlimpseData` rolü ve yalnızca kullanıcıların Glimpse'in verileri görüntülemek için bu rolde verin.</span><span class="sxs-lookup"><span data-stu-id="369d7-169">Consider adding the markup above to a publish profile so it's only deployed an applyed when you use that publish profile (for example, your Azure test proifle.) To restrict Glimpse data, we will add the `canViewGlimpseData` role and only allow users in this role to view Glimpse data.</span></span>

<span data-ttu-id="369d7-170">Açıklamayı kaldırma *GlimpseSecurityPolicy.cs* dosya ve değişiklik [IsInRole](https://msdn.microsoft.com/en-us/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx) çağırmanıza `Administrator` için `canViewGlimpseData` rol:</span><span class="sxs-lookup"><span data-stu-id="369d7-170">Remove the comments from the *GlimpseSecurityPolicy.cs* file and change the [IsInRole](https://msdn.microsoft.com/en-us/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx) call from `Administrator` to the `canViewGlimpseData` role:</span></span>

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample4.cs?highlight=6)]

> [!WARNING]
> <span data-ttu-id="369d7-171">Güvenlik - Glimpse'in tarafından sağlanan zengin verileri, uygulamanızın güvenlik geçmesine neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="369d7-171">Security - The rich data provided by Glimpse could expose the security of your app.</span></span> <span data-ttu-id="369d7-172">Microsoft, üretim uygulamalarını kullanmak için bir bakışta, güvenlik denetimini gerçekleştirmediği.</span><span class="sxs-lookup"><span data-stu-id="369d7-172">Microsoft has not performed a security audit of Glimpse for use on productions apps.</span></span>


<span data-ttu-id="369d7-173">Rol ekleme hakkında daha fazla bilgi için bkz: my [Güvenli ASP.NET MVC 5 web uygulaması üyeliği, OAuth ve SQL veritabanı ile Azure'a dağıtma](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) Öğreticisi.</span><span class="sxs-lookup"><span data-stu-id="369d7-173">For information on adding roles, see my [Deploy a Secure ASP.NET MVC 5 web app with Membership, OAuth, and SQL Database to Azure](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) tutorial.</span></span>

<a id="addRes"></a>
## <a name="additional-resources"></a><span data-ttu-id="369d7-174">Ek Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="369d7-174">Additional Resources</span></span>

- [<span data-ttu-id="369d7-175">Güvenli ASP.NET MVC 5 uygulama üyeliği, OAuth ve SQL veritabanı ile Azure'a dağıtma</span><span class="sxs-lookup"><span data-stu-id="369d7-175">Deploy a Secure ASP.NET MVC 5 app with Membership, OAuth, and SQL Database to Azure</span></span>](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)
- <span data-ttu-id="369d7-176">[Glimpse'in yapılandırma](http://getglimpse.com/Docs/Configuration) -sekmeler, çalışma zamanı İlkesi, günlüğe kaydetme ve daha fazla yapılandırma belge sayfası.</span><span class="sxs-lookup"><span data-stu-id="369d7-176">[Glimpse Configuration](http://getglimpse.com/Docs/Configuration) - Doc page on configuring tabs, runtime policy, logging and more.</span></span>
