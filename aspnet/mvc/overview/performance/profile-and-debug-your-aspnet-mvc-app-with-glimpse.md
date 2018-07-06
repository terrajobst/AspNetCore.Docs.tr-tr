---
uid: mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
title: Glimpse ile ASP.NET MVC uygulamanızın hatalarını ayıklama ve profil | Microsoft Docs
author: Rick-Anderson
description: Glimpse olduğu bir başarısız ayrıntılı performans sağlayan açık kaynak NuGet paketlerini ailesi büyüyen, hata ayıklama ve tanılama bilgilerini ASP.NET bir...
ms.author: aspnetcontent
ms.date: 03/26/2015
ms.assetid: c205805f-efdd-4fa7-9616-f26eab180611
msc.legacyurl: /mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
msc.type: authoredcontent
ms.openlocfilehash: 4d044217d6c33594b39747a765165b8702338027
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808547"
---
<a name="profile-and-debug-your-aspnet-mvc-app-with-glimpse"></a><span data-ttu-id="449c3-103">Glimpse ile ASP.NET MVC uygulamanızın hatalarını ayıklama ve profil</span><span class="sxs-lookup"><span data-stu-id="449c3-103">Profile and debug your ASP.NET MVC app with Glimpse</span></span>
====================
<span data-ttu-id="449c3-104">Tarafından [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="449c3-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="449c3-105">Glimpse bir başarısız ve ayrıntılı performans sağlayan açık kaynak NuGet paketlerini ailesi büyüyen, hata ayıklama ve ASP.NET uygulamaları için tanılama bilgileri içindir.</span><span class="sxs-lookup"><span data-stu-id="449c3-105">Glimpse is a thriving and growing family of open source NuGet packages that provides detailed performance, debugging and diagnostic information for ASP.NET apps.</span></span> <span data-ttu-id="449c3-106">Önemsiz yüklemek için basit, son derece hızlı ve her sayfanın altında temel performans ölçümlerini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="449c3-106">It's trivial to install, lightweight, ultra-fast, and displays key performance metrics at the bottom of every page.</span></span> <span data-ttu-id="449c3-107">Sunucuda neler olduğunu öğrenmek, ihtiyacınız olduğunda uygulamanıza detaya olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="449c3-107">It allows you to drill down into your app when you need to find out what's going on at the server.</span></span> <span data-ttu-id="449c3-108">Glimpse Azure test ortamına dahil olmak üzere, geliştirme döngüsü boyunca kullanmanızı öneririz çok değerli bilgiler sağlar.</span><span class="sxs-lookup"><span data-stu-id="449c3-108">Glimpse provides so much valuable information we recommend you use it throughout your development cycle, including your Azure test environment.</span></span> <span data-ttu-id="449c3-109">Sırada [Fiddler](http://www.telerik.com/fiddler) ve [F-12 geliştirme araçları](https://msdn.microsoft.com/library/ie/gg589512(v=vs.85).aspx) sağlayan bir istemci tarafı görünüm Glimpse sunucudan ayrıntılı bir görünüm sağlar.</span><span class="sxs-lookup"><span data-stu-id="449c3-109">While [Fiddler](http://www.telerik.com/fiddler) and the [F-12 development tools](https://msdn.microsoft.com/library/ie/gg589512(v=vs.85).aspx) provide a client side view, Glimpse provides a detailed view from the server.</span></span> <span data-ttu-id="449c3-110">Bu öğretici bir bakışta ASP.NET MVC ve EF paketleri kullanarak odaklanmak, ancak diğer birçok paketleri kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="449c3-110">This tutorial will focus on using the Glimpse ASP.NET MVC and EF packages, but many other packages are available.</span></span> <span data-ttu-id="449c3-111">Mümkün olduğunda miyim uygun bağlayacaksınız [Glimpse docs](http://getglimpse.com/Docs/) korunmasına yardımcı olan.</span><span class="sxs-lookup"><span data-stu-id="449c3-111">Where possible I will link to the appropriate [Glimpse docs](http://getglimpse.com/Docs/) which I help maintain.</span></span> <span data-ttu-id="449c3-112">Glimpse açık kaynak bir proje, kaynak kodu ve belgeleri için çok katkıda bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="449c3-112">Glimpse is an open source project, you too can contribute to the source code and the docs.</span></span>


- [<span data-ttu-id="449c3-113">Glimpse yükleme</span><span class="sxs-lookup"><span data-stu-id="449c3-113">Installing Glimpse</span></span>](#ig)
- [<span data-ttu-id="449c3-114">Glimpse localhost için etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="449c3-114">Enable Glimpse for localhost</span></span>](#eg)
- [<span data-ttu-id="449c3-115">Zaman Çizelgesi sekmesi</span><span class="sxs-lookup"><span data-stu-id="449c3-115">The Timeline tab</span></span>](#Time)
- [<span data-ttu-id="449c3-116">Model Bağlamaları</span><span class="sxs-lookup"><span data-stu-id="449c3-116">Model Binding</span></span>](#mb)
- [<span data-ttu-id="449c3-117">Yollar</span><span class="sxs-lookup"><span data-stu-id="449c3-117">Routes</span></span>](#route)
- [<span data-ttu-id="449c3-118">Glimpse Azure'da kullanma</span><span class="sxs-lookup"><span data-stu-id="449c3-118">Using Glimpse on Azure</span></span>](#da)
- [<span data-ttu-id="449c3-119">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="449c3-119">Additional Resources</span></span>](#addRes)

<a id="ig"></a>
## <a name="installing-glimpse"></a><span data-ttu-id="449c3-120">Glimpse yükleme</span><span class="sxs-lookup"><span data-stu-id="449c3-120">Installing Glimpse</span></span>

<span data-ttu-id="449c3-121">NuGet Paket Yöneticisi konsolundan veya Glimpse yükleyebilirsiniz **NuGet paketlerini Yönet** Konsolu.</span><span class="sxs-lookup"><span data-stu-id="449c3-121">You can install Glimpse from the NuGet package manager console or from the **Manage NuGet Packages** console.</span></span> <span data-ttu-id="449c3-122">Bu Tanıtım için Mvc5 ve EF6 paketleri yükleyeceğim:</span><span class="sxs-lookup"><span data-stu-id="449c3-122">For this demo, I'll install the Mvc5 and EF6 packages:</span></span>

![Glimpse NuGet Dlg ' yükleme](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image1.png)

<span data-ttu-id="449c3-124">Arama *Glimpse.EF*</span><span class="sxs-lookup"><span data-stu-id="449c3-124">Search for *Glimpse.EF*</span></span>

![NuGet yüklemesi dlg gelen Glimpse.EF](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image2.png)

<span data-ttu-id="449c3-126">Seçerek **yüklü paketleri**, yüklü bir bakışta bağımlı modülleri görebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="449c3-126">By selecting **Installed packages**, you can see the Glimpse dependent modules installed:</span></span>

![DLg Glimpse paketleri yüklü](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image3.png)

<span data-ttu-id="449c3-128">Aşağıdaki komutlar, Paket Yöneticisi konsolundan Glimpse MVC5 ve EF6 modüllerini yükleyin:</span><span class="sxs-lookup"><span data-stu-id="449c3-128">The following commands install Glimpse MVC5 and EF6 modules from the package manager console:</span></span>

[!code-console[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample1.cmd)]

<a id="eg"></a>
## <a name="enable-glimpse-for-localhost"></a><span data-ttu-id="449c3-129">Glimpse localhost için etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="449c3-129">Enable Glimpse for localhost</span></span>

<span data-ttu-id="449c3-130">Gidin http://localhost:&lt; bağlantı noktası #&gt;tıklayın ve /glimpse.axd <strong>Glimpse açma</strong> düğmesi.</span><span class="sxs-lookup"><span data-stu-id="449c3-130">Navigate to http://localhost:&lt;port #&gt;/glimpse.axd and click the <strong>Turn Glimpse On</strong> button.</span></span>

![Glimpse axd sayfası](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image4.png)

<span data-ttu-id="449c3-132">Gösterilen, Sık Kullanılanlar çubuğu varsa sürükleyin ve Glimpse düğmeleri bırakın ve bunları bookmarklets ekleyin:</span><span class="sxs-lookup"><span data-stu-id="449c3-132">If you have your favorites bar displayed, you can drag and drop the Glimpse buttons and add them as bookmarklets:</span></span>

![Glimpse boookmarklets ile IE](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image5.png)

<span data-ttu-id="449c3-134">Şimdi uygulamanıza gidin ve **Heads yukarı görünen** (baş üstü) sayfanın alt kısmında gösterilir.</span><span class="sxs-lookup"><span data-stu-id="449c3-134">You can now navigate your app, and the **Heads Up Display** (HUD) is shown at the bottom of the page.</span></span>

![Kişi Yöneticisi sayfasıyla baş üstü](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image6.png)

<span data-ttu-id="449c3-136">[Glimpse baş üstü sayfa](http://getglimpse.com/Docs/Heads-up-Display) yukarıda gösterilen zamanlama bilgilerini ayrıntıları.</span><span class="sxs-lookup"><span data-stu-id="449c3-136">The [Glimpse HUD page](http://getglimpse.com/Docs/Heads-up-Display) details the timing information shown above.</span></span> <span data-ttu-id="449c3-137">Örtük performans verilerini baş üstü görüntüler için test döngüsü geçmeden önce bir sorun hemen - bildirebilir.</span><span class="sxs-lookup"><span data-stu-id="449c3-137">The unobtrusive performance data the HUD displays can notify you of a problem immediately - before you get to the test cycle.</span></span> <span data-ttu-id="449c3-138">Tıklayarak &quot;g&quot; Glimpse paneli sağ alt köşeye taşır:</span><span class="sxs-lookup"><span data-stu-id="449c3-138">Clicking on the &quot;g&quot; in the lower right corner brings up the Glimpse panel:</span></span>

![Glimpse paneli](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image7.png)

<span data-ttu-id="449c3-140">Yukarıdaki görüntüde [yürütme sekmesi](http://getglimpse.com/Docs/Execution-Tab) seçildiğinde, işlem hattı, filtreleri ve eylemleri zamanlama ayrıntılarını gösterir.</span><span class="sxs-lookup"><span data-stu-id="449c3-140">In the image above, the [Execution tab](http://getglimpse.com/Docs/Execution-Tab) is selected, which shows timing details of the actions and filters in the pipeline.</span></span> <span data-ttu-id="449c3-141">Gördüğünüz my [İzle Durdur filtre Zamanlayıcı](http://www.nuget.org/packages/StopWatch/) işlem hattının 6 aşamasından başlayıp.</span><span class="sxs-lookup"><span data-stu-id="449c3-141">You can see my [Stop Watch filter timer](http://www.nuget.org/packages/StopWatch/) start at stage 6 of the pipeline.</span></span> <span data-ttu-id="449c3-142">My hafif Zamanlayıcı sağlarken yararlı veri profili/zamanlama, yetkilendirme için harcanan ve görünüm işlemeyle her zaman isabetsizlikleri.</span><span class="sxs-lookup"><span data-stu-id="449c3-142">While my light weight timer can provide useful profile/timing data, it misses all the time spent in authorization and rendering the view.</span></span> <span data-ttu-id="449c3-143">Konumundaki my Zamanlayıcısı okuyabilirsiniz [profil ve ASP.NET MVC uygulamanızı azure'a tüm saat](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx).</span><span class="sxs-lookup"><span data-stu-id="449c3-143">You can read about my timer at [Profile and Time your ASP.NET MVC app all the way to Azure](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx).</span></span> <span data-ttu-id="449c3-144">[Sekmeleri](http://getglimpse.com/Docs/Tabs) sayfası, her bir sekmede ayrıntılı bilgi için bağlantılar sağlar.</span><span class="sxs-lookup"><span data-stu-id="449c3-144">The [Tabs](http://getglimpse.com/Docs/Tabs) page provides links to detailed information on each tab.</span></span>

<a id="Time"></a>
## <a name="the-timeline-tab"></a><span data-ttu-id="449c3-145">Zaman Çizelgesi sekmesi</span><span class="sxs-lookup"><span data-stu-id="449c3-145">The Timeline tab</span></span>

<span data-ttu-id="449c3-146">Tom Dykstra'ait değiştirilen bekleyen [EF 6/MVC 5 Öğreticisi](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) Eğitmenler denetleyiciye aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="449c3-146">I've modified Tom Dykstra's outstanding [EF 6/MVC 5 tutorial](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) with the following code change to the instructors controller:</span></span>

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample2.cs?highlight=1,20-31)]

<span data-ttu-id="449c3-147">Yukarıdaki kod bana sorgu dizesine geçmenizi sağlar (`eager`) eager veya açık denetim için verileri yükleniyor.</span><span class="sxs-lookup"><span data-stu-id="449c3-147">The code above allows me to pass in query string (`eager`) to control eager or explicit loading of data.</span></span> <span data-ttu-id="449c3-148">Aşağıdaki görüntüde, açık yükleme kullanılır ve zamanlama sayfası yüklenmiş her kaydı gösterir `Index` eylem yöntemi:</span><span class="sxs-lookup"><span data-stu-id="449c3-148">In the image below, explicit loading is used and the timing page shows each enrollment loaded in the `Index` action method:</span></span>

![açık yükleme](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image8.png)

<span data-ttu-id="449c3-150">Aşağıdaki kodda eager belirtilir ve sonra her kayıt getirilen `Index` görünümü çağrılır:</span><span class="sxs-lookup"><span data-stu-id="449c3-150">In the following code, eager is specified, and each enrollment is fetched after the `Index` view is called:</span></span>

![eager belirtilir](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image9.png)

<span data-ttu-id="449c3-152">Ayrıntılı zamanlama bilgileri almak için zaman diliminin gelerek:</span><span class="sxs-lookup"><span data-stu-id="449c3-152">You can hover over a time segment to get detailed timing information:</span></span>

![ayrıntılı zamanlama görmek için üzerine gelme](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image10.png)

<a id="mb"></a>
## <a name="model-binding"></a><span data-ttu-id="449c3-154">Model bağlama</span><span class="sxs-lookup"><span data-stu-id="449c3-154">Model Binding</span></span>

<span data-ttu-id="449c3-155">[Model bağlama sekmesini](http://getglimpse.com/Docs/Model-Binding-Tab) çok sayıda form değişkenlerinizi nasıl bağlı ve beklediğiniz gibi neden bazı bağlı olmayan anlamanıza yardımcı olacak bilgiler sağlar.</span><span class="sxs-lookup"><span data-stu-id="449c3-155">The [model binding tab](http://getglimpse.com/Docs/Model-Binding-Tab) provides a wealth of information to help you understand how your form variables are bound and why some are not bound as would expect.</span></span> <span data-ttu-id="449c3-156">Gösterir aşağıdaki resim **?**</span><span class="sxs-lookup"><span data-stu-id="449c3-156">The image below shows the **?**</span></span> <span data-ttu-id="449c3-157">simge bir özelliğin glimpse Yardım sayfasını açmak için tıklayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="449c3-157">icon, which you can click on to bring up the glimpse help page for that feature.</span></span>

![Model bağlama görünümü işlediğine göz atın](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image11.png)

<a id="route"></a>
## <a name="routes"></a><span data-ttu-id="449c3-159">Yollar</span><span class="sxs-lookup"><span data-stu-id="449c3-159">Routes</span></span>

 <span data-ttu-id="449c3-160">Glimpse yollar sekmesi, hata ayıklama ve yönlendirme anlamanıza yardımcı olabilir.</span><span class="sxs-lookup"><span data-stu-id="449c3-160">The Glimpse Routes tab will can help you debug and understand routing.</span></span> <span data-ttu-id="449c3-161">Aşağıdaki görüntüde, ürün yol seçilir (ve yeşil bir bakışta kuralı gösterir).</span><span class="sxs-lookup"><span data-stu-id="449c3-161">In the image below, the product route is selected (and it shows in green, a Glimpse convention).</span></span> <span data-ttu-id="449c3-162">![Seçili ürün adı](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png) rota kısıtlamaları, alanlar ve veri belirteçleri de görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="449c3-162">![product name selected](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png) Route constraints, Areas and data tokens are also displayed.</span></span> <span data-ttu-id="449c3-163">Bkz: [Glimpse yollar](http://getglimpse.com/Docs/Routes-Tab) ve [öznitelik yönlendirme ASP.NET MVC 5'te](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="449c3-163">See [Glimpse Routes](http://getglimpse.com/Docs/Routes-Tab) and [Attribute Routing in ASP.NET MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx) for more information.</span></span> 

<a id="da"></a>
## <a name="using-glimpse-on-azure"></a><span data-ttu-id="449c3-164">Glimpse Azure'da kullanma</span><span class="sxs-lookup"><span data-stu-id="449c3-164">Using Glimpse on Azure</span></span>

<span data-ttu-id="449c3-165">Glimpse varsayılan güvenlik ilkesini yalnızca yerel ana bilgisayardan görüntülenecek Glimpse verileri sağlar.</span><span class="sxs-lookup"><span data-stu-id="449c3-165">The Glimpse default security policy only allows Glimpse data to be displayed from local host.</span></span> <span data-ttu-id="449c3-166">Uzak bir sunucuya (örneğin, bir web uygulamasını azure'da) bu verileri görüntülemek için bu güvenlik ilkesini değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="449c3-166">You can change this security policy so you can view this data on a remote server (such as a web app on Azure).</span></span> <span data-ttu-id="449c3-167">Azure üzerinde test ortamları için sonuna kadar vurgulanan işareti ekleme *web.confg* Glimpse etkinleştirmek için dosya:</span><span class="sxs-lookup"><span data-stu-id="449c3-167">For test environments on Azure, add the highlighted mark up to the bottom of the *web.confg* file to enable Glimpse:</span></span>

[!code-xml[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample3.xml?highlight=2-6)]

<span data-ttu-id="449c3-168">Bu değişiklik yalnızca, herhangi bir kullanıcı, uzak bir siteye Glimpse verilerinizi görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="449c3-168">With this change alone, any user can see your Glimpse data on a remote site.</span></span> <span data-ttu-id="449c3-169">Bu yayımlama profilini (örneğin, Azure test proifle.) kullandığınızda, yalnızca bir uygulanan dağıtıldığını için yukarıda işaretleme için bir yayımlama profili eklemeyi düşünün Glimpse verileri kısıtlamak için ekleyeceğiz `canViewGlimpseData` rol ve Glimpse verilerini görüntülemek için bu rol yalnızca kullanıcıların.</span><span class="sxs-lookup"><span data-stu-id="449c3-169">Consider adding the markup above to a publish profile so it's only deployed an applyed when you use that publish profile (for example, your Azure test proifle.) To restrict Glimpse data, we will add the `canViewGlimpseData` role and only allow users in this role to view Glimpse data.</span></span>

<span data-ttu-id="449c3-170">Açıklamayı Kaldır *GlimpseSecurityPolicy.cs* dosya ve değiştirme [IPrincipal](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx) çağırmanıza `Administrator` için `canViewGlimpseData` rolü:</span><span class="sxs-lookup"><span data-stu-id="449c3-170">Remove the comments from the *GlimpseSecurityPolicy.cs* file and change the [IsInRole](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx) call from `Administrator` to the `canViewGlimpseData` role:</span></span>

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample4.cs?highlight=6)]

> [!WARNING]
> <span data-ttu-id="449c3-171">Güvenlik - Glimpse tarafından sağlanan zengin veriler uygulamanızın güvenlik geçmesine neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="449c3-171">Security - The rich data provided by Glimpse could expose the security of your app.</span></span> <span data-ttu-id="449c3-172">Microsoft, üretim uygulamalarında kullanmak için bir bakışta güvenlik denetimi yapmamış.</span><span class="sxs-lookup"><span data-stu-id="449c3-172">Microsoft has not performed a security audit of Glimpse for use on productions apps.</span></span>


<span data-ttu-id="449c3-173">Rol ekleme hakkında daha fazla bilgi için bkz. benim [bir üyelik, OAuth ve SQL veritabanı ile güvenli bir ASP.NET MVC 5 web uygulamasını Azure'a dağıtma](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) öğretici.</span><span class="sxs-lookup"><span data-stu-id="449c3-173">For information on adding roles, see my [Deploy a Secure ASP.NET MVC 5 web app with Membership, OAuth, and SQL Database to Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) tutorial.</span></span>

<a id="addRes"></a>
## <a name="additional-resources"></a><span data-ttu-id="449c3-174">Ek Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="449c3-174">Additional Resources</span></span>

- [<span data-ttu-id="449c3-175">Azure'da bir üyelik, OAuth ve SQL veritabanı ile güvenli bir ASP.NET MVC 5 uygulaması dağıtma</span><span class="sxs-lookup"><span data-stu-id="449c3-175">Deploy a Secure ASP.NET MVC 5 app with Membership, OAuth, and SQL Database to Azure</span></span>](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)
- <span data-ttu-id="449c3-176">[Glimpse yapılandırma](http://getglimpse.com/Docs/Configuration) -Doc sayfa sekmeleri, çalışma zamanı İlkesi, günlüğe kaydetme ve daha fazla yapılandırma.</span><span class="sxs-lookup"><span data-stu-id="449c3-176">[Glimpse Configuration](http://getglimpse.com/Docs/Configuration) - Doc page on configuring tabs, runtime policy, logging and more.</span></span>
