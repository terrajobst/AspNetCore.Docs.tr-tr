---
uid: mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
title: Glimpse ile ASP.NET MVC uygulamanızın hatalarını ayıklama ve profil | Microsoft Docs
author: Rick-Anderson
description: Glimpse olduğu bir başarısız ayrıntılı performans sağlayan açık kaynak NuGet paketlerini ailesi büyüyen, hata ayıklama ve tanılama bilgilerini ASP.NET bir...
ms.author: riande
ms.date: 03/26/2015
ms.assetid: c205805f-efdd-4fa7-9616-f26eab180611
msc.legacyurl: /mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
msc.type: authoredcontent
ms.openlocfilehash: f5d174ff6823d654a24dcb2c90f10a3cbd24f1e7
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754021"
---
<a name="profile-and-debug-your-aspnet-mvc-app-with-glimpse"></a>Glimpse ile ASP.NET MVC uygulamanızın hatalarını ayıklama ve profil
====================
Tarafından [Rick Anderson](https://github.com/Rick-Anderson)

> Glimpse bir başarısız ve ayrıntılı performans sağlayan açık kaynak NuGet paketlerini ailesi büyüyen, hata ayıklama ve ASP.NET uygulamaları için tanılama bilgileri içindir. Önemsiz yüklemek için basit, son derece hızlı ve her sayfanın altında temel performans ölçümlerini görüntüler. Sunucuda neler olduğunu öğrenmek, ihtiyacınız olduğunda uygulamanıza detaya olanak tanır. Glimpse Azure test ortamına dahil olmak üzere, geliştirme döngüsü boyunca kullanmanızı öneririz çok değerli bilgiler sağlar. Sırada [Fiddler](http://www.telerik.com/fiddler) ve [F-12 geliştirme araçları](https://msdn.microsoft.com/library/ie/gg589512(v=vs.85).aspx) sağlayan bir istemci tarafı görünüm Glimpse sunucudan ayrıntılı bir görünüm sağlar. Bu öğretici bir bakışta ASP.NET MVC ve EF paketleri kullanarak odaklanmak, ancak diğer birçok paketleri kullanılabilir. Mümkün olduğunda miyim uygun bağlayacaksınız [Glimpse docs](http://getglimpse.com/Docs/) korunmasına yardımcı olan. Glimpse açık kaynak bir proje, kaynak kodu ve belgeleri için çok katkıda bulunabilir.


- [Glimpse yükleme](#ig)
- [Glimpse localhost için etkinleştirme](#eg)
- [Zaman Çizelgesi sekmesi](#Time)
- [Model Bağlamaları](#mb)
- [Yollar](#route)
- [Glimpse Azure'da kullanma](#da)
- [Ek Kaynaklar](#addRes)

<a id="ig"></a>
## <a name="installing-glimpse"></a>Glimpse yükleme

NuGet Paket Yöneticisi konsolundan veya Glimpse yükleyebilirsiniz **NuGet paketlerini Yönet** Konsolu. Bu Tanıtım için Mvc5 ve EF6 paketleri yükleyeceğim:

![Glimpse NuGet Dlg ' yükleme](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image1.png)

Arama *Glimpse.EF*

![NuGet yüklemesi dlg gelen Glimpse.EF](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image2.png)

Seçerek **yüklü paketleri**, yüklü bir bakışta bağımlı modülleri görebilirsiniz:

![DLg Glimpse paketleri yüklü](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image3.png)

Aşağıdaki komutlar, Paket Yöneticisi konsolundan Glimpse MVC5 ve EF6 modüllerini yükleyin:

[!code-console[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample1.cmd)]

<a id="eg"></a>
## <a name="enable-glimpse-for-localhost"></a>Glimpse localhost için etkinleştirme

Gidin http://localhost:&lt; bağlantı noktası #&gt;tıklayın ve /glimpse.axd <strong>Glimpse açma</strong> düğmesi.

![Glimpse axd sayfası](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image4.png)

Gösterilen, Sık Kullanılanlar çubuğu varsa sürükleyin ve Glimpse düğmeleri bırakın ve bunları bookmarklets ekleyin:

![Glimpse boookmarklets ile IE](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image5.png)

Şimdi uygulamanıza gidin ve **Heads yukarı görünen** (baş üstü) sayfanın alt kısmında gösterilir.

![Kişi Yöneticisi sayfasıyla baş üstü](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image6.png)

[Glimpse baş üstü sayfa](http://getglimpse.com/Docs/Heads-up-Display) yukarıda gösterilen zamanlama bilgilerini ayrıntıları. Örtük performans verilerini baş üstü görüntüler için test döngüsü geçmeden önce bir sorun hemen - bildirebilir. Tıklayarak &quot;g&quot; Glimpse paneli sağ alt köşeye taşır:

![Glimpse paneli](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image7.png)

Yukarıdaki görüntüde [yürütme sekmesi](http://getglimpse.com/Docs/Execution-Tab) seçildiğinde, işlem hattı, filtreleri ve eylemleri zamanlama ayrıntılarını gösterir. Gördüğünüz my [İzle Durdur filtre Zamanlayıcı](http://www.nuget.org/packages/StopWatch/) işlem hattının 6 aşamasından başlayıp. My hafif Zamanlayıcı sağlarken yararlı veri profili/zamanlama, yetkilendirme için harcanan ve görünüm işlemeyle her zaman isabetsizlikleri. Konumundaki my Zamanlayıcısı okuyabilirsiniz [profil ve ASP.NET MVC uygulamanızı azure'a tüm saat](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx). [Sekmeleri](http://getglimpse.com/Docs/Tabs) sayfası, her bir sekmede ayrıntılı bilgi için bağlantılar sağlar.

<a id="Time"></a>
## <a name="the-timeline-tab"></a>Zaman Çizelgesi sekmesi

Tom Dykstra'ait değiştirilen bekleyen [EF 6/MVC 5 Öğreticisi](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) Eğitmenler denetleyiciye aşağıdaki kodla değiştirin:

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample2.cs?highlight=1,20-31)]

Yukarıdaki kod bana sorgu dizesine geçmenizi sağlar (`eager`) eager veya açık denetim için verileri yükleniyor. Aşağıdaki görüntüde, açık yükleme kullanılır ve zamanlama sayfası yüklenmiş her kaydı gösterir `Index` eylem yöntemi:

![açık yükleme](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image8.png)

Aşağıdaki kodda eager belirtilir ve sonra her kayıt getirilen `Index` görünümü çağrılır:

![eager belirtilir](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image9.png)

Ayrıntılı zamanlama bilgileri almak için zaman diliminin gelerek:

![ayrıntılı zamanlama görmek için üzerine gelme](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image10.png)

<a id="mb"></a>
## <a name="model-binding"></a>Model bağlama

[Model bağlama sekmesini](http://getglimpse.com/Docs/Model-Binding-Tab) çok sayıda form değişkenlerinizi nasıl bağlı ve beklediğiniz gibi neden bazı bağlı olmayan anlamanıza yardımcı olacak bilgiler sağlar. Gösterir aşağıdaki resim **?** simge bir özelliğin glimpse Yardım sayfasını açmak için tıklayabilirsiniz.

![Model bağlama görünümü işlediğine göz atın](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image11.png)

<a id="route"></a>
## <a name="routes"></a>Yollar

 Glimpse yollar sekmesi, hata ayıklama ve yönlendirme anlamanıza yardımcı olabilir. Aşağıdaki görüntüde, ürün yol seçilir (ve yeşil bir bakışta kuralı gösterir). ![Seçili ürün adı](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png) rota kısıtlamaları, alanlar ve veri belirteçleri de görüntülenir. Bkz: [Glimpse yollar](http://getglimpse.com/Docs/Routes-Tab) ve [öznitelik yönlendirme ASP.NET MVC 5'te](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx) daha fazla bilgi için. 

<a id="da"></a>
## <a name="using-glimpse-on-azure"></a>Glimpse Azure'da kullanma

Glimpse varsayılan güvenlik ilkesini yalnızca yerel ana bilgisayardan görüntülenecek Glimpse verileri sağlar. Uzak bir sunucuya (örneğin, bir web uygulamasını azure'da) bu verileri görüntülemek için bu güvenlik ilkesini değiştirebilirsiniz. Azure üzerinde test ortamları için sonuna kadar vurgulanan işareti ekleme *web.confg* Glimpse etkinleştirmek için dosya:

[!code-xml[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample3.xml?highlight=2-6)]

Bu değişiklik yalnızca, herhangi bir kullanıcı, uzak bir siteye Glimpse verilerinizi görebilirsiniz. Bu yayımlama profilini (örneğin, Azure test proifle.) kullandığınızda, yalnızca bir uygulanan dağıtıldığını için yukarıda işaretleme için bir yayımlama profili eklemeyi düşünün Glimpse verileri kısıtlamak için ekleyeceğiz `canViewGlimpseData` rol ve Glimpse verilerini görüntülemek için bu rol yalnızca kullanıcıların.

Açıklamayı Kaldır *GlimpseSecurityPolicy.cs* dosya ve değiştirme [IPrincipal](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx) çağırmanıza `Administrator` için `canViewGlimpseData` rolü:

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample4.cs?highlight=6)]

> [!WARNING]
> Güvenlik - Glimpse tarafından sağlanan zengin veriler uygulamanızın güvenlik geçmesine neden olabilir. Microsoft, üretim uygulamalarında kullanmak için bir bakışta güvenlik denetimi yapmamış.


Rol ekleme hakkında daha fazla bilgi için bkz. benim [bir üyelik, OAuth ve SQL veritabanı ile güvenli bir ASP.NET MVC 5 web uygulamasını Azure'a dağıtma](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) öğretici.

<a id="addRes"></a>
## <a name="additional-resources"></a>Ek Kaynaklar

- [Azure'da bir üyelik, OAuth ve SQL veritabanı ile güvenli bir ASP.NET MVC 5 uygulaması dağıtma](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)
- [Glimpse yapılandırma](http://getglimpse.com/Docs/Configuration) -Doc sayfa sekmeleri, çalışma zamanı İlkesi, günlüğe kaydetme ve daha fazla yapılandırma.
