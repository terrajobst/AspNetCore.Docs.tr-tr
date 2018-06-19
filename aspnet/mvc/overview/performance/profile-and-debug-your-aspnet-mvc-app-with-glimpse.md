---
uid: mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
title: Profil ve Glimpse'in ile ASP.NET MVC uygulamanızın hatalarını ayıklama | Microsoft Docs
author: Rick-Anderson
description: Glimpse'in olan bir başarısız ayrıntılı performans sağlayan açık kaynak NuGet paketlerini ailesi büyüyen, hata ayıklama ve tanılama bilgilerini ASP.NET bir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/26/2015
ms.topic: article
ms.assetid: c205805f-efdd-4fa7-9616-f26eab180611
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
msc.type: authoredcontent
ms.openlocfilehash: 6ac23256c57116de81c7bf690d5ce743301c75ce
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872981"
---
<a name="profile-and-debug-your-aspnet-mvc-app-with-glimpse"></a>Profil ve Glimpse'in ile ASP.NET MVC uygulamanızın hatalarını ayıklama
====================
tarafından [Rick Anderson](https://github.com/Rick-Anderson)

> Glimpse'in bir başarısız ayrıntılı performans sağlayan açık kaynak NuGet paketlerini ailesi büyüyen, hata ayıklama ve tanılama bilgilerini ASP.NET uygulamaları için ' dir. Önemsiz yüklemek için basit, son derece hızlı ve temel performans ölçümlerini her sayfasının en altında görüntülenir. Sunucuda neler olduğunu öğrenmek gerektiğinde uygulamanıza detaya imkan tanır. Glimpse'in Azure test ortamınızı dahil olmak üzere, geliştirme döngüsü boyunca kullanmanızı öneririz çok değerli bilgiler sağlar. Sırada [Fiddler](http://www.telerik.com/fiddler) ve [F-12 geliştirme araçları](https://msdn.microsoft.com/library/ie/gg589512(v=vs.85).aspx) sağlayan bir istemci tarafı görünüm Glimpse'in sunucudan ayrıntılı bir görünüm sağlar. Bu öğretici Glimpse'in ASP.NET MVC ve EF paketleri kullanarak odaklanır, ancak diğer birçok paketleri kullanılabilir. Mümkün olduğunda ı uygun bağlayacaksınız [Glimpse'in belgeleri](http://getglimpse.com/Docs/) hangi korunmasına yardımcı. Glimpse'in açık kaynaklı proje, kaynak kodu ve belgeler için çok katkıda bulunabilir.


- [Yükleme bakışta](#ig)
- [Localhost için Glimpse'in etkinleştir](#eg)
- [Zaman Çizelgesi sekmesi](#Time)
- [Model Bağlamaları](#mb)
- [Yollar](#route)
- [Azure üzerinde Glimpse'in kullanma](#da)
- [Ek kaynaklar](#addRes)

<a id="ig"></a>
## <a name="installing-glimpse"></a>Yükleme bakışta

NuGet Paket Yöneticisi Konsolu'ndan veya Glimpse'in yükleyebilirsiniz **NuGet paketlerini Yönet** konsol. Bu Tanıtım için ı Mvc5 ve EF6 paketleri yükleyeceksiniz:

![NuGet iletişim kutusu Glimpse'in yükleyin](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image1.png)

Arama *Glimpse.EF*

![Glimpse.EF gelen NuGet Yükle iletişim kutusu](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image2.png)

Seçerek **yüklü paketleri**, yüklü Glimpse'in bağımlı modülleri görebilirsiniz:

![İletişim kutusu Glimpse'in paketleri yüklü](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image3.png)

Aşağıdaki komutlar Paket Yöneticisi Konsolu'ndan Glimpse'in MVC5 ve EF6 modüllerini yükleyin:

[!code-console[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample1.cmd)]

<a id="eg"></a>
## <a name="enable-glimpse-for-localhost"></a>Localhost için Glimpse'in etkinleştir

Gidin http://localhost: &lt;bağlantı noktası #&gt;tıklayın ve /glimpse.axd <strong>Glimpse'in Aç</strong> düğmesi.

![Glimpse'in axd sayfası](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image4.png)

Görüntülenen, Sık Kullanılanlar çubuğuna varsa, sürükleyin ve Glimpse'in düğmeleri bırakın ve bunları bookmarklets Ekle:

![IE Glimpse'in boookmarklets ile](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image5.png)

Şimdi, uygulamanızın gidebilirsiniz ve **kafa yukarı görüntü** (HUD), sayfasının en altında gösterilir.

![HUD ile ilgili kişi Yöneticisi sayfası](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image6.png)

[Glimpse'in HUD sayfa](http://getglimpse.com/Docs/Heads-up-Display) yukarıda gösterilen zamanlama bilgisini ayrıntıları. Test döngüsü ulaşmadan örtük performans verileri HUD görüntüler, bir sorunun hemen - bildirebilir. Tıklayarak &quot;g&quot; Glimpse'in bölmenin sağ alt köşedeki getirir:

![Glimpse'in paneli](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image7.png)

Yukarıdaki görüntüsündeki [yürütme sekmesi](http://getglimpse.com/Docs/Execution-Tab) seçildiğinde, ardışık düzeninde Eylemler ve filtreleri zamanlama ayrıntılarını gösterir. Gördüğünüz my [durdurmak izleme filtresi Zamanlayıcı](http://www.nuget.org/packages/StopWatch/) ardışık 6 aşamada başlatın. My hafif Zamanlayıcı sağlayabilir ancak yararlı veri profili/zamanlama, yetkilendirme harcanan ve görünüm işlemeyle her zaman isabetsiz okuma. My Zamanlayıcısı hakkında bilgi edinebilirsiniz [profil ve ASP.NET MVC uygulamanıza tüm Azure saat](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx). [Sekmeleri](http://getglimpse.com/Docs/Tabs) sayfası, her bir sekmede ayrıntılı bilgi için bağlantılar sağlar.

<a id="Time"></a>
## <a name="the-timeline-tab"></a>Zaman Çizelgesi sekmesi

Zel Dykstra'nın değiştiren bekleyen [EF 6/MVC 5 Öğreticisi](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) Eğitmen denetleyiciye aşağıdaki kodla değiştirin:

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample2.cs?highlight=1,20-31)]

Yukarıdaki kod bana sorgu dizesine geçmenizi sağlar (`eager`) istekli veya açık denetim verileri yükleniyor. Aşağıdaki resimde, açık yükleme kullanılır ve zamanlama sayfası yüklenmiş her kayıt gösterir `Index` eylem yöntemi:

![Açık yükleniyor](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image8.png)

Aşağıdaki kodda eager belirtilir ve sonra her kayıt getirilen `Index` görünüm çağrılır:

![eager belirtilen](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image9.png)

Ayrıntılı zamanlama bilgilerini almak için zaman diliminin getirin:

![ayrıntılı zamanlama görmek için vurgulu](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image10.png)

<a id="mb"></a>
## <a name="model-binding"></a>Model Binding

[Model bağlama sekmesini](http://getglimpse.com/Docs/Model-Binding-Tab) bol miktarda form değişkeni nasıl bağlı ve beklediğiniz gibi neden bazı bağlı olmayan anlamanıza yardımcı olacak bilgiler sağlar. Görüntünün gösterir aşağıda **?** simge özellik glimpse'in Yardım sayfasını getirmek için tıklatabilirsiniz.

![Model bağlama görünümü glimpse'in](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image11.png)

<a id="route"></a>
## <a name="routes"></a>Yollar

 Glimpse'in yollar sekmesini hata ayıklama ve yönlendirme anlamanıza yardımcı olabilir. Aşağıdaki görüntü, ürün yol seçilir (ve yeşil, bir bakışta kuralı gösterir). ![Seçilen ürün adı](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png) rota kısıtlamaları, alanlar ve veri belirteçleri de görüntülenir. Bkz: [Glimpse'in yollar](http://getglimpse.com/Docs/Routes-Tab) ve [özniteliği yönlendirme ASP.NET MVC 5'te](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx) daha fazla bilgi için. 

<a id="da"></a>
## <a name="using-glimpse-on-azure"></a>Azure üzerinde Glimpse'in kullanma

Glimpse'in varsayılan güvenlik ilkesi, yalnızca yerel ana bilgisayardan görüntülenecek Glimpse'in verileri sağlar. Uzak bir sunucuda (örneğin, Azure üzerinde bir web uygulaması) bu veri görüntüleyebilmeniz için bu güvenlik ilkesini değiştirebilirsiniz. Azure üzerinde test ortamları için sonuna kadar vurgulanan işareti ekleme *Web.config* dosya Glimpse'in etkinleştirmek için:

[!code-xml[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample3.xml?highlight=2-6)]

Bu değişiklik tek başına, herhangi bir kullanıcı, uzak bir siteye Glimpse'in verilerinizi görebilirsiniz. Bu yayımlama profili (örneğin, Azure test proifle.) kullandığınızda, yalnızca bir uygulanan dağıtıldığını şekilde biçimlendirme yukarıda bir yayımlama profili eklemeyi düşünün Glimpse'in verileri kısıtlamak için ekleyeceğiz `canViewGlimpseData` rolü ve yalnızca kullanıcıların Glimpse'in verileri görüntülemek için bu rolde verin.

Açıklamayı kaldırma *GlimpseSecurityPolicy.cs* dosya ve değişiklik [IsInRole](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx) çağırmanıza `Administrator` için `canViewGlimpseData` rol:

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample4.cs?highlight=6)]

> [!WARNING]
> Güvenlik - Glimpse'in tarafından sağlanan zengin verileri, uygulamanızın güvenlik geçmesine neden olabilir. Microsoft, üretim uygulamalarını kullanmak için bir bakışta, güvenlik denetimini gerçekleştirmediği.


Rol ekleme hakkında daha fazla bilgi için bkz: my [Güvenli ASP.NET MVC 5 web uygulaması üyeliği, OAuth ve SQL veritabanı ile Azure'a dağıtma](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) Öğreticisi.

<a id="addRes"></a>
## <a name="additional-resources"></a>Ek Kaynaklar

- [Güvenli ASP.NET MVC 5 uygulama üyeliği, OAuth ve SQL veritabanı ile Azure'a dağıtma](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)
- [Glimpse'in yapılandırma](http://getglimpse.com/Docs/Configuration) -sekmeler, çalışma zamanı İlkesi, günlüğe kaydetme ve daha fazla yapılandırma belge sayfası.
