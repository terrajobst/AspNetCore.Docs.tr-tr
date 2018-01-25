---
uid: visual-studio/overview/2013/creating-web-projects-in-visual-studio
title: "ASP.NET Web projeleri Visual Studio 2013'te oluşturma | Microsoft Docs"
author: tdykstra
description: "Bu konuda, burada güncelleştirme 3 ile Visual Studio 2013'te ASP.NET web projeleri oluşturmak için seçenekler web geliştirme c için yeni özelliklerden bazıları açıklanmaktadır..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/01/2014
ms.topic: article
ms.assetid: 61941e64-0c0d-4996-9270-cb8ccfd0cabc
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/creating-web-projects-in-visual-studio
msc.type: authoredcontent
ms.openlocfilehash: aacae7a9ccf483b21d3c6796c0411d558fa3c75b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="creating-aspnet-web-projects-in-visual-studio-2013"></a>Visual Studio 2013'te ASP.NET Web projeleri oluşturma
====================
by [Tom Dykstra](https://github.com/tdykstra)

> Bu konu Visual Studio 2013 güncelleştirme 3 ile ASP.NET web projeleri oluşturmak için kullanabileceğiniz seçenekler açıklanmaktadır
> 
> Önceki sürümlere göre Visual Studio web geliştirme için yeni özelliklerden bazıları şunlardır:
> 
> - Bu teklif projeleri oluşturmak için basit bir kullanıcı Arabirimi [desteklemek için birden çok ASP.NET çerçeveyi](#add) (Web Forms, MVC ve Web API).
> - [ASP.NET Identity](#indauth), tüm ASP.NET çerçeveler ve çalışan aynı web barındırma IIS dışında yazılım ile çalışan yeni bir ASP.NET üyelik sistemi.
> - Kullanımını [önyükleme](#bootstrap) esnek tasarım ve tema özellikleri sağlamak üzere.
> - Yalnızca MVC için gibi sunulması için kullanılan Web formları için yeni özellikler [otomatik test projesi oluşturmayı](#testproj) ve bir [Intranet site şablonu](#winauth).
> 
> Azure bulut Hizmetleri veya Azure Mobile Services için web projeleri oluşturma hakkında daha fazla bilgi için bkz: [Azure Cloud Services ve ASP.NET ile çalışmaya başlama](https://azure.microsoft.com/documentation/articles/cloud-services-dotnet-get-started/) ve [ile Azure Mobile Services .NET Skorbordu uygulaması oluşturma Arka uç](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/).


<a id="prerequisites"></a>
## <a name="prerequisites"></a>Önkoşullar

Bu makalede açıklanan [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566) ile [güncelleştirme 3'ü](https://go.microsoft.com/fwlink/?linkid=397827&amp;clcid=0x409) yüklü.

<a id="wap"></a>
## <a name="web-application-projects-versus-web-site-projects"></a>Web uygulaması projelerine web sitesi projeleri karşılaştırması

ASP.NET web projeleri iki tür arasında bir seçim sunar: *web uygulama projeleri* ve *web sitesi projeleri*. Web uygulaması projelerine yeni geliştirme öneririz ve bu makalede yalnızca web uygulaması projelerine yöneliktir. Daha fazla bilgi için bkz: [Visual Studio'da Web sitesi projeleri ile Web uygulaması projelerine](https://msdn.microsoft.com/library/dd547590(v=vs.120).aspx) MSDN sitesinden.

<a id="overview"></a>
## <a name="overview-of-web-application-project-creation"></a>Web uygulaması projesi oluşturma genel bakış

Aşağıdaki adımlar bir web projesi oluşturmayı gösterir:

1. Tıklatın **yeni proje** içinde **Başlat** sayfa veya **dosya** menüsü.
2. İçinde **yeni proje** iletişim kutusunda, tıklatın **Web** sol bölmede ve **ASP.NET Web uygulaması** Orta bölmede.

    ![Yeni Proje iletişim kutusu](creating-web-projects-in-visual-studio/_static/image1.png)

    Seçebileceğiniz **bulut** oluşturmak için sol bölmede bir [Azure bulut hizmeti](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy), [Azure mobil hizmeti](https://msdn.microsoft.com/library/windows/apps/dn629482.aspx), veya [Azure WebJob](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-webjobs). Bu konu, bu şablonları kapsamaz.
3. Sağ bölmede **projeye Application Insights Ekle** sistem durumu ve uygulamanız için kullanım izleme isterseniz onay kutusunu. Daha fazla bilgi için bkz: [web uygulamalarında performansı izleyerek](https://azure.microsoft.com/documentation/articles/app-insights-web-monitor-performance/).
4. Proje belirtin **adı**, **konumu**ve diğer seçenekleri ve ardından **Tamam**.

    **Yeni ASP.NET projesi** iletişim kutusu görüntülenir.

    ![Yeni Proje iletişim kutusu](creating-web-projects-in-visual-studio/_static/image2.png)
5. Bir şablonu'nu tıklatın.

    ![Bir şablon seçin](creating-web-projects-in-visual-studio/_static/image3.png)
6. Şablonda bulunmayan ek çerçeveler için destek eklemek istiyorsanız, uygun onay kutusuna tıklayın. (Gösterilen örnekte, MVC ve/veya Web API Web Forms projeye ekleyebilirsiniz.)

    ![Çerçeveleri ekleyin](creating-web-projects-in-visual-studio/_static/image4.png)
7. <a id="testproj"></a>Birim testi projesi eklemek istiyorsanız, **birim testleri ekleme**.

    ![Birim testleri ekleme](creating-web-projects-in-visual-studio/_static/image5.png)
8. Varsayılan olarak şablon sağladıkları daha farklı kimlik doğrulama yöntemini istiyorsanız, **kimlik doğrulamayı Değiştir**.

    ![Kimlik doğrulama düğmesi yapılandırın](creating-web-projects-in-visual-studio/_static/image6.png)

    ![Kimlik doğrulama iletişim yapılandırın](creating-web-projects-in-visual-studio/_static/image7.png)

<a id="azurenewproj"></a>
### <a name="create-a-web-app-or-virtual-machine-in-azure"></a>Bir web uygulaması veya sanal makine oluşturma

Visual Studio web uygulamalarını barındırmak için Azure services ile çalışmak kolaylaştıran özellikler içerir. Örneğin, aşağıdakilerin tümü doğru Visual Studio IDE'den yapabilirsiniz:

- Oluşturabilir ve web uygulamaları ya da uygulamanızı Internet üzerinden kullanılabilir hale sanal makineleri yönetebilirsiniz.
- Bulutta çalışan uygulama tarafından oluşturulan günlükleri görüntüleyin.
- Uygulama bulutta çalışırken uzaktan hata ayıklama modunda çalıştırın.
- Viiew ve diğer Azure hizmetleriyle SQL veritabanları gibi yönetebilirsiniz.

Yapabilecekleriniz [bir Azure hesabı oluşturma](https://www.windowsazure.com/pricing/free-trial/) , ücretsiz web uygulamaları gibi temel hizmetleri içerir ve bir MSDN abonesi misiniz yoksa şunları yapabilirsiniz [avantajları etkinleştirme](https://azure.microsoft.com/pricing/member-offers/visual-studio-subscriptions/) , size ek Azure doğru aylık KREDİLERİ Hizmetler. 

Varsayılan olarak **yeni ASP.NET projesi** iletişim kutusu, bir web uygulaması veya sanal makine için yeni bir web projesi oluşturmanıza olanak sağlar. Yeni web uygulaması veya sanal makine oluşturmak istemiyorsanız temizleyin **bulutta Barındır** onay kutusu.

![Uzak kaynaklar oluştur](creating-web-projects-in-visual-studio/_static/image8.png)

Onay kutusu resim yazısı olabilir **bulutta Barındır** veya **uzak kaynaklar Oluştur**, ve her iki durumda da etkisi aynıdır. Onay kutusunu seçili bırakın, Visual Studio varsayılan olarak Azure App Service'te bir web uygulaması oluşturur. Açılan kutu olarak değiştirmek için kullanabileceğiniz bir **sanal makine** tercih ederseniz. Azure'a zaten oturum açtınız, Azure kimlik bilgileri istenir. Oturum açtıktan sonra bir iletişim kutusu, projeniz için Visual Studio oluşturacak kaynakları yapılandırmanızı sağlar. Aşağıdaki çizimde bir web uygulaması için iletişim kutusu görüntüler; bir sanal makine oluşturmayı seçerseniz farklı seçenekler görünür.

![Azure uygulaması ayarlarını yapılandır](creating-web-projects-in-visual-studio/_static/image9.png)

Azure kaynaklarını oluşturmak için bu işlem kullanma hakkında daha fazla bilgi için bkz: [Azure ve ASP.NET ile çalışmaya başlama](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet) ve [Visual Studio ile web sitesi için bir sanal makine oluşturma](https://azure.microsoft.com/documentation/articles/virtual-machines-dotnet-create-visual-studio-powershell/).

Bu makalenin sonraki bölümlerinde, kullanılabilir şablonlar ve bunların seçenekleri hakkında daha fazla bilgi sağlar. Makale aynı zamanda şablonlarında kullanılan önyükleme, Düzen ve tema framework sunar.

<a id="vs2013"></a>
## <a name="visual-studio-2013-web-project-templates"></a>Visual Studio 2013 Web projesi şablonları

Visual Studio şablonları web projeleri oluşturmak için kullanır. Bir proje şablonu yeni projedeki dosyaları ve klasörleri oluşturun, NuGet paketi yüklemesi ve ilkel çalışan bir uygulama için örnek kod sağlar. Şablonlar, en son web standartlarını uygular ve kendi uygulamanızı oluşturma yanı sıra ASP.NET teknolojileri kullanan bir atlama size nasıl için en iyi uygulamaları Başlat göstermek için tasarlanmıştır.

Visual Studio 2013, .NET 4.5 veya .NET framework'ün sonraki sürümlerini hedefleyen projeler için web projesi şablonları için aşağıdaki seçenekleri sağlar:

- [Boş şablonu](#empty)
- [Web Forms şablonu](#wf)
- [MVC şablonu](#mvc)
- [Web API şablonu](#webapi)
- [Tek sayfa uygulaması şablonu](#spa)
- [Azure mobil hizmeti şablonu](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/)
- [Visual Studio 2012 şablonları](#vs2012)

Sağlayan bir Visual Studio uzantısı yükleyebilmek için bir [Facebook şablon](#facebook).

.NET 4'i hedefleyen projelerde oluşturma hakkında daha fazla bilgi için bkz: [Visual Studio 2012 şablonları](#vs2012) bu konuda daha sonra.

ASP.NET uygulamaları için mobil istemcilerin oluşturma hakkında daha fazla bilgi için bkz: [ASP.NET Mobil Destek](../../../mobile/index.md).

<a id="empty"></a>
### <a name="empty-template"></a>Boş şablonu

Tam minimum klasörleri ve dosyaları bir proje dosyası gibi bir ASP.NET web uygulaması için boş şablonu sağlar (*.csproj* veya. *vbproj*) ve bir *Web.config* dosya. Altındaki onay kutularını kullanarak Web Forms, MVC ve/veya Web API için destek ekleyebilirsiniz **klasörler ekleme ve çekirdek için başvurular:** etiketi.

Boş şablon için hiçbir kimlik doğrulama seçenekleri kullanılabilir. Kimlik doğrulaması işlevselliğini örnek uygulamalarda uygulanır ve boş şablonu örnek bir uygulama oluşturmaz.

<a id="wf"></a>
### <a name="web-forms-template"></a>Web Forms şablonu

Kullanıcı Arabirimi ve veri zengin web sitelerini hızlı bir şekilde izin veren aşağıdaki özellikleri derleme bir çerçeve sağlar Web Forms özelliklere erişim:

- Visual Studio'da WYSIWYG Tasarımcısı.
- HTML ve özelliklerin ve stillerin ayarlayarak özelleştirebilirsiniz işlemek sunucu denetimleri.
- Veri erişimi ve veri görüntüleme için denetimleri zengin bir listesini.
- İstediğiniz programı olaylar ortaya çıkaran bir olay modelini WPF gibi bir istemci uygulaması program.
- Otomatik koruma HTTP istekleri arasındaki durumu (veri).

Genel olarak, bir Web Forms uygulaması oluşturmak için ASP.NET MVC çerçevesi kullanarak aynı uygulaması oluşturma değerinden daha az programlama çaba gerekir. Ancak, Web Forms yalnızca hızlı uygulama geliştirme için değil. Çok sayıda karmaşık ticari uygulamaları ve Web Forms üstünde oluşturulmuş çerçeveleri vardır.

Web Forms sayfası ve sayfanın denetimleri otomatik olarak tarayıcıya gönderilen biçimlendirme çoğunu oluşturduğundan, ASP.NET MVC sunar HTML üzerinde ayrıntılı denetim türü yok. Sayfalar ve denetimler yapılandırmak için bildirim temelli modeli yazmak zorunda, ancak bazı HTML ve HTTP davranışını gizler kod miktarını en aza indirir. Örneğin, her zaman tam olarak hangi biçimlendirme denetimi tarafından oluşturulabilir belirlemek mümkün değildir.

Web Forms framework kendisini ASP.NET MVC olarak kullanıma desenleri tabanlı geliştirme uygulamalar gibi ödünç değil [teste dayalı geliştirme](http://en.wikipedia.org/wiki/Test-driven_development), [sorunları ayrılması](http://en.wikipedia.org/wiki/Separation_of_concerns), [, tersine çevirme Denetim](http://en.wikipedia.org/wiki/Inversion_of_control), ve [bağımlılık ekleme](http://en.wikipedia.org/wiki/Dependency_injection). Böylece kodu oluşturmak yazmak istiyorsanız, şunları yapabilirsiniz; ASP.NET MVC çerçevesi olduğu gibi otomatik değil yalnızca. [ASP.NET Web Forms MVP](http://webformsmvp.com/) proje, Web Forms teslim etmek için oluşturulan hızlı geliştirme koruyarak ayrılması sorunları ve Test Edilebilirlik kolaylaştıran bir yaklaşım gösterir. Microsoft SharePoint Web Forms MVP üzerinde oluşturulmuştur.

Web Forms şablonu kullanan bir örnek Web Forms uygulaması oluşturur [önyükleme](#bootstrap) esnek tasarım ve tema özellikleri sağlamak için. Aşağıdaki çizimde, giriş sayfası gösterilmektedir.

![Web Forms şablon uygulama giriş sayfası](creating-web-projects-in-visual-studio/_static/image10.png)

Web Forms hakkında daha fazla bilgi için bkz: [ASP.NET Web Forms](https://asp.net/web-forms). Web Forms şablon sizin için ne yaptığını hakkında daha fazla bilgi için bkz: [Visual Studio 2013 kullanarak temel bir Web Forms uygulaması oluşturma](https://blogs.msdn.com/b/webdev/archive/2013/12/19/building-a-basic-web-forms-application-using-visual-studio-2013.aspx).

<a id="mvc"></a>
### <a name="mvc-template"></a>MVC şablonu

ASP.NET MVC desenleri tabanlı geliştirme uygulamalar gibi kolaylaştırmak için tasarlanmış [teste dayalı geliştirme](http://en.wikipedia.org/wiki/Test-driven_development), [sorunları ayrılması](http://en.wikipedia.org/wiki/Separation_of_concerns), [denetimi tersine çevirme](http://en.wikipedia.org/wiki/Inversion_of_control), ve [bağımlılık ekleme](http://en.wikipedia.org/wiki/Dependency_injection). Sunu katmanı web uygulamasından iş mantığı katmanı ayırarak framework önerir. Uygulama modelleri (M), görünümler (V) ve denetleyiciler (C) bölerek, ASP.NET MVC, daha büyük uygulamalar karmaşıklığı yönetmek kolaylaştırabilir.

ASP.NET MVC ile daha doğrudan HTML ve Web formları'te daha HTTP ile çalışır. Örneğin, Web Forms HTTP isteklerini arasında durumunu otomatik olarak koruyabilirsiniz ancak açıkça MVC'de kodunu gerekiyor. Uygulamanızın ne yaptığını ve web ortamında biçimini tam olarak üzerinde tam denetim yararlanmanıza olanak sağlar ve MVC modeline avantajlarından birisidir. Daha fazla kod yazmak zorunda olumsuz olur.

MVC, güç geliştiricilerin uygulama gereksinimlerine framework özelleştirme yeteneği sağlayan genişletilebilir olmak üzere tasarlanmıştır. Buna ek olarak, ASP.NET MVC kaynak kodu OSI lisansı altında kullanılabilir.

MVC şablonu kullanan bir örnek MVC 5 uygulaması oluşturur [önyükleme](#bootstrap) esnek tasarım ve tema özellikleri sağlamak için. Aşağıdaki çizimde, giriş sayfası gösterilmektedir.

![MVC örnek uygulama](creating-web-projects-in-visual-studio/_static/image11.png)

MVC hakkında daha fazla bilgi için bkz: [ASP.NET MVC](https://asp.net/mvc). MVC 4 şablon seçme hakkında daha fazla bilgi için bkz: [Visual Studio 2012 şablonları](#vs2012) bu makalenin ilerisinde yer.

<a id="webapi"></a>
### <a name="web-api-template"></a>Web API şablonu

Web API şablon Web API, API yardım sayfalarına MVC tabanlı dahil olmak üzere temel bir örnek web hizmeti oluşturur.

ASP.NET Web API istemciler, tarayıcılar ve mobil cihazlar dahil olmak üzere çok çeşitli ulaşmak HTTP hizmetlerini oluşturmayı kolaylaştıran bir çerçevedir. ASP.NET Web API, .NET Framework üzerinde RESTful hizmetlerini geliştirmek için ideal bir platformdur.

Web API şablon bir örnek web hizmeti oluşturur. Aşağıdaki çizimler örnek yardım sayfalarına göstermektedir.

![Web API Yardım sayfası](creating-web-projects-in-visual-studio/_static/image12.png)

![Web API Yardım sayfası API almak için](creating-web-projects-in-visual-studio/_static/image13.png)

Web API'si hakkında daha fazla bilgi için bkz: [ASP.NET Web API](https://asp.net/web-api).

<a id="spa"></a>
### <a name="single-page-application-template"></a>Tek sayfalı uygulama şablonu

JavaScript, HTML 5 kullanan bir örnek uygulamayı tek sayfa uygulama (SPA) şablonu oluşturur ve [Çakıştırmaları](http://knockoutjs.com/) istemci ve sunucu üzerinde ASP.NET Web API.

Yalnızca kimlik doğrulaması seçeneği SPA şablonu için [tek tek kullanıcı hesaplarını](#indauth).

Aşağıdaki çizimde SPA şablon derlemeler örnek uygulamayı ilk durumunu gösterir.

![SPA örnek uygulama](creating-web-projects-in-visual-studio/_static/image14.png)

SPA şablonu kullanarak bir uygulama oluşturma hakkında daha fazla bilgi için bkz: [Web API - Dış kimlik doğrulama hizmetleri](../../../web-api/overview/security/external-authentication-services.md).

JavaScript çerçeveler Çakıştırmaları dışında kullanmak ek SPA şablonlar ve ASP.NET tek sayfa uygulamaları hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [ASP.NET tek sayfa uygulaması](../../../single-page-application/index.md).
- [VS2013 RC için SPA şablonundaki güvenlik özellikleri anlama](https://blogs.msdn.com/b/webdev/archive/2013/09/20/understanding-security-features-in-spa-template.aspx)
- [Tek sayfalı uygulamalar: ASP.NET ile Modern, yanıt veren Web uygulamaları oluşturma](https://msdn.microsoft.com/magazine/dn463786.aspx)

<a id="facebook"></a>
### <a name="facebook-template"></a>Facebook şablonu

Yükleyebileceğiniz bir [Facebook şablonu sağlar Visual Studio Uzantısı](https://go.microsoft.com/fwlink/?LinkID=509965&amp;clcid=0x409). Bu şablon, Facebook web sitesinin içinde çalışmak için tasarlanmış örnek bir uygulama oluşturur. ASP.NET MVC tabanlı ve gerçek zamanlı güncelleştirme işlevselliği için Web API'sini kullanır.

Facebook uygulamaları Facebook sitede çalışır ve Facebook'ın kimlik doğrulamasını kullanan çünkü hiçbir kimlik doğrulama seçenekleri Facebook şablonu için kullanılabilir.

ASP.NET Facebook uygulamaları hakkında daha fazla bilgi için bkz: [MVC Facebook API'si güncelleştirme](https://blogs.msdn.com/b/webdev/archive/2014/06/10/updating-the-mvc-facebook-api.aspx).

<a id="vs2012"></a>
### <a name="visual-studio-2012-templates"></a>Visual Studio 2012 şablonları

Visual Studio 2013 web projesi oluşturma iletişim kutusu, Visual Studio 2012'de kullanılabilen bazı şablonlarına erişim sağlamaz. Bu şablonlardan birini kullanmak istiyorsanız, Visual Studio yeni proje iletişim kutusunun sol bölmedeki Visual Studio 2012 düğümünü tıklatabilirsiniz.

![Visual Studio 2012 şablonları](creating-web-projects-in-visual-studio/_static/image15.png)

**Visual Studio 2012** sahip olmayan aşağıdaki web şablonlarını seçtiğiniz düğüm erişmelerine olanak tanır için şablonları varsayılan listesinde Visual Studio 2013 için:

- ASP.NET MVC 4 Web uygulaması
- ASP.NET dinamik veri varlıkları Web uygulaması
- ASP.NET AJAX Sunucu denetimi
- ASP.NET AJAX Sunucu denetimi genişletici
- ASP.NET sunucu denetimi

<a id="bootstrap"></a>
## <a name="bootstrap-in-the-visual-studio-2013-web-project-templates"></a>Visual Studio 2013 web projesi şablonları bootstrap

Visual Studio 2013 proje şablonlarını kullanın [önyükleme](http://getbootstrap.com/), Twitter tarafından oluşturulan bir düzen ve tema altyapısı. Önyükleme CSS3 düzenleri dinamik olarak farklı bir tarayıcı penceresi boyutlarına uyarlayabilirsiniz anlamına gelir esnek tasarım sağlamak için kullanır. Örneğin, bir geniş tarayıcı penceresinde Web Forms şablonu tarafından oluşturulan giriş sayfası aşağıdaki gibi görünür:

![Web Forms şablon uygulama giriş sayfası](creating-web-projects-in-visual-studio/_static/image16.png)

Pencerenin daraltmak ve yatay olarak düzenlenmiş sütunları dikey düzenlemeleri Taşı:

![Önyükleme dikey sütun düzenleme](creating-web-projects-in-visual-studio/_static/image17.png)

Bir dikey yönelimli menüsüne genişletmek için tıklatabileceği bir simge yatay üst menü dönüştürür ve pencere biraz daha dar:

![Önyükleme menüsü simgesi](creating-web-projects-in-visual-studio/_static/image18.png)

![Önyükleme dikey menüsü](creating-web-projects-in-visual-studio/_static/image19.png)

Uygulamanın görünüm değişikliği kolayca etkilemek için önyükleme'nın Tema oluşturma özelliğini de kullanabilirsiniz. Örneğin, temayı değiştirmek için aşağıdaki adımları yapabilirsiniz.

1. Tarayıcınızda, Git [http://Bootswatch.com](http://Bootswatch.com)bir tema seçin ve ardından **karşıdan**. (Bu indirir *bootstrap.min.css* CSS kodunu incelemek isterseniz, varsayılan olarak; alma *bootstrap.css* küçültülmüş sürümü yerine.)
2. İndirilen CSS dosyasının içeriğini kopyalayın.
3. Visual Studio'da yeni bir oluşturma **stil sayfası** adlı dosya *önyükleme theme.css* içinde *içerik* klasörü ve Yapıştır indirilen CSS kodunu buna kopyalayın.
4. Açık *uygulama\_Start/Bundle.config* değiştirip *bootstrap.css* için *önyükleme theme.css*.

Projeyi tekrar çalıştırın ve uygulama yeni bir görünüme sahiptir. Aşağıdaki çizimde Amelia tema etkisini gösterir:

![Önyükleme Amelia tema](creating-web-projects-in-visual-studio/_static/image20.png)

Birçok önyükleme tema kullanılabilir ücretsiz ve premium sürümlerinde. Önyükleme sunduğu çok çeşitli kullanıcı Arabirimi bileşenlerini gibi [aşağı açılan listeler](http://twitter.github.io/bootstrap/components.html#dropdowns), [düğmesini grupları](http://twitter.github.io/bootstrap/components.html#buttonGroups), ve [simgeleri](http://twitter.github.io/bootstrap/base-css.html#images). Önyükleme hakkında daha fazla bilgi için bkz: [önyükleme site](http://twitter.github.io/bootstrap/).

Visual Studio'da Web Forms designer kullanırsanız, tüm etkilerini önyükleme Temalar veya esnek düzen değişiklikleri doğru şekilde göstermiyor şekilde Tasarımcı CSS3, desteklemediğini unutmayın. Ancak, Web formları sayfaları doğru bir tarayıcı ile görüntülendiğinde görüntüler.

<a id="add"></a>
## <a name="adding-support-for-additional-frameworks"></a>Ek çerçeveleri desteği ekleme

Bir şablon seçin, şablon tarafından kullanılan framework(s) onay kutusunu otomatik olarak seçilir. Örneğin, **Web Forms** şablon **Web Forms** onay kutusu seçilidir ve işaretini kaldıramazsınız.

![Bir şablon seçin](creating-web-projects-in-visual-studio/_static/image21.png)

![Çerçeveleri ekleyin](creating-web-projects-in-visual-studio/_static/image22.png)

Proje oluşturduğunuzda o çerçevesi için destek eklemek için şablonda bulunup çerçevesi için onay kutusunu seçebilirsiniz. Örneğin, Web Forms kullanımını etkinleştirmek için *.aspx* MVC şablonu seçtiğinizde sayfaları seçin **Web Forms** onay kutusu. Veya Web Forms şablon kullanırken MVC etkinleştirmek için **MVC** onay kutusu. Bir çerçeve ekleme çalışma zamanı yanı sıra tasarım zamanı desteği sağlar. Örneğin, Web Forms projeye MVC destek eklerseniz, denetleyicileri ve görünümler iskele mümkün olacaktır.

Web Forms ve MVC bir proje ile birleştirir ve etkinleştirme [kolay URL'leri](http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx) Web formlarında olabilir bir URL birden çok olası hedefi olduğu sorunları yönlendirme beklenmeyen. Tanımlanan rotaları ilk öncelikli olur. Örneğin, bir `Home` denetleyicisi ve bir *Home.aspx* sayfasında `http://contoso.com/home` URL için yazılacak *Home.aspx* çağırırsanız `EnableFriendlyUrls` çağırmadanönceyöntemi`MapRoute`yönteminde *RouteConfig.cs*, ya da aynı URL için varsayılan görünüm gider, `Home` çağırırsanız denetleyicisi `MapRoute` önce `EnableFriendlyUrls`.

Bir çerçeve ekleme herhangi bir örnek uygulama işlevi eklemez. Eklerseniz Hayır MVC şablonu seçtiğinizde Örneğin, Web Forms desteği *Default.aspx* giriş sayfası dosyası oluşturulur. Yalnızca klasörleri, dosyaları ve framework desteklemek için gereken başvuru eklenir. Bu nedenle, örnek uygulamalarda şablonlar tarafından oluşturulan kodu tarafından gerçekleştirilen kimlik doğrulama seçenekleri çerçeveler ekleme değişmez. Örneğin, boş şablonu seçin ve ekleyin, Web Forms veya MVC destek, **Authentication Yapılandır** düğmesi hala devre dışı bırakılacak.

Aşağıdaki bölümlerde, her onay kutusu etkisini kısaca açıklanmaktadır.

### <a name="add-web-forms-support"></a>Web Forms desteği ekleme

Boş oluşturur *uygulama\_veri* ve *modelleri* klasörleri ve *Global.asax* dosya. Web Forms onay kutusunu seçerek diğer şablonlar için fark etmez şekilde Bunlar zaten boş şablonu dışındaki tüm şablonlar tarafından oluşturulur.

Web Forms şablon varsayılan olarak, ancak Web Forms kolay URL'leri otomatik olarak etkin olmayan Web Forms onay kutusunu seçerek diğer şablonlar için destek eklediğinizde kolay URL'leri etkinleştirir.

### <a name="add-mvc-support"></a>MVC desteği ekleme

MVC, Razor ve Web sayfalarını NuGet paketleri yükler, boş oluşturur *uygulama\_veri*, *denetleyicileri*, *modelleri*, ve *görünümleri*klasörleri oluşturur *uygulama\_Başlat* klasörüyle *RouteConfig.cs* dosya ve oluşturur *Global.asax* dosya.

### <a name="add-web-api-support"></a>Web API desteği ekleme

Webapı ve Newtonsoft.Json NuGet paketleri yükler, boş oluşturur *uygulama\_veri*, *denetleyicileri*, ve *modelleri* klasörleri oluşturur  *Uygulama\_Başlat* klasörüyle *WebApiConfig.cs* dosya ve oluşturur *Global.asax* dosya.

<a id="auth"></a>
## <a name="authentication-methods"></a>Kimlik doğrulama yöntemleri

Visual Studio 2013'ün Web Forms, MVC ve Web API şablonları için birden fazla kimlik doğrulama seçenekleri sunar:

- [Kimlik doğrulaması yok](#noauth)
- [Bireysel kullanıcı hesapları](#indauth) (ASP.NET Identity, önceden ASP.NET üyeliği olarak biliniyordu)
- [Kurumsal hesaplar](#orgauth) (Windows Server Active Directory veya Azure Active Directory)
- [Windows kimlik doğrulaması](#winauth) (Intranet)

![Kimlik doğrulama iletişim yapılandırın](creating-web-projects-in-visual-studio/_static/image23.png)

<a id="noauth"></a>

### <a name="no-authentication"></a>Kimlik doğrulaması yok

Seçerseniz **doğrulaması yok**, örnek uygulama günlüğü'nde için hiçbir web sayfalarını içerir, Hayır kimin oturum belirten bir kullanıcı Arabirimi, üyelik veritabanının ve bir üyelik veritabanı için bir bağlantı dizesi için hiçbir varlık sınıfları.

<a id="indauth"></a>
### <a name="individual-user-accounts"></a>Bireysel kullanıcı hesapları

Seçerseniz **tek tek kullanıcı hesaplarını**, örnek uygulamayı ASP.NET (eski adıyla ASP.NET üyelik) kimlik kullanmak için kullanıcı kimlik doğrulaması kullanacak şekilde yapılandırılır. ASP.NET Identity bir kullanıcının sitede bir kullanıcı adı ve parola oluşturarak veya Facebook, Google, Microsoft Account ve Twitter gibi sosyal sağlayıcılardan oturum açarak bir hesabı kaydetmesini sağlar. Kullanıcı profillerini ASP.NET Identity için varsayılan veri deposu üretim site için SQL Server veya Azure SQL veritabanı dağıtmak SQL Server yerel veritabanı veritabanıdır.

Visual Studio 2013'te bu özellikler aynı Visual Studio 2012 olduğu gibi ancak arka plandaki kod ASP.NET üyelik sistemini için yazılmıştır. Yeni kod temeli avantajları şunlardır:

- Yeni üyelik sistemi dayanır [OWIN](http://owin.org/) ASP.NET formları kimlik doğrulama modülü yerine. Bu, IIS'de Web Forms veya MVC kullanıyorsanız ya da Web API'sini veya SignalR kendi kendine barındırma aynı kimlik doğrulama mekanizması kullanabileceğiniz anlamına gelir.
- Yeni bir üyelik veritabanı Entity Framework Code First tarafından yönetilir ve tüm tabloları değiştirebileceğiniz varlık sınıfları tarafından gösterilir. Bu profil ile ilgili web kendi gereksinimlerine uyacak şekilde kullanıcı Arabirimi ve veritabanı şeması kolayca özelleştirebilir ve güncelleştirmelerinizi Code First Migrations kullanılarak kolayca dağıtabilirsiniz anlamına gelir.

Yeni üyelik sistemi yeni şablonlar, otomatik olarak uygulanır ve el ile .NET 4.5 hedefleyen bir projede uygulanan veya üzeri olabilir.

Çoğunlukla dış müşterileri için olan bir Internet web sitesi oluşturuyorsanız, ASP.NET Identity iyi bir seçimdir. Kuruluşunuz Active Directory kullanır veya Office 365 ve çalışanlar ve iş ortakları için bir çoklu oturum açma sağlayan bir proje oluşturmak istiyorsanız **Kurumsal hesaplar** seçeneği, daha iyi bir seçim olabilir.

Bireysel kullanıcı hesapları seçeneği hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [www.asp.net/identity](../../../identity/index.md). ASP.NET web sitesindeki belgeleri ASP.NET Identity hakkında.
- [Facebook ve Google OAuth2 ve Openıd oturum açma ile bir ASP.NET MVC 5 uygulaması oluşturma](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md). Ayrıca kullanıcı profili verilerini özelleştirmek nasıl gösterir.
- [Web API - Dış kimlik doğrulama hizmeti](../../../web-api/overview/security/external-authentication-services.md)
- [ASP.NET uygulamanızı Visual Studio 2013'te dış oturum açma ekleme](https://blogs.msdn.com/b/webdev/archive/2013/06/27/adding-external-logins-to-your-asp-net-application-in-visual-studio-2013.aspx)

<a id="orgauth"></a>
### <a name="organizational-accounts"></a>Kurumsal hesaplar

Seçerseniz **Kurumsal hesaplar**, örnek uygulamayı Azure Active Directory (Office 365 içeren Azure AD) kullanıcı hesaplarına dayalı kimlik doğrulaması için Windows Identity Foundation (WIF) kullanmak üzere yapılandırılmış veya Windows Server Active Directory. Daha fazla bilgi için bkz: [Kurumsal hesap kimlik doğrulama seçenekleri](#orgauthoptions) bu konuda daha sonra.

<a id="winauth"></a>
### <a name="windows-authentication"></a>Windows Kimlik Doğrulaması

Seçerseniz **Windows kimlik doğrulaması**, örnek uygulama için Windows kimlik doğrulaması IIS modülünü kimlik doğrulaması için kullanmak üzere yapılandırılmış. Uygulama pencerelere kaydedilir ancak olmaz kullanıcı kaydı ekleyen veya UI oturum açması yerel makine hesabı ve Active directory etki alanı ve kullanıcı Kimliğini görüntüler. Bu seçenek, Intranet web siteleri için tasarlanmıştır.

Alternatif olarak, seçerek AD kimlik doğrulaması kullanan bir Intranet sitesine oluşturabilirsiniz [şirket içi Kurumsal hesaplar seçeneği](#orgauthonprem). Şirket içi seçenek Windows Identity Foundation (WIF) yerine Windows kimlik doğrulama modülü kullanır. Şirket içi seçeneği atamak için bazı ek adımlar gerekli, ancak WIF Windows kimlik doğrulaması modülüyle birlikte bulunmayan özellikleri etkinleştirir. Örneğin, WIF ile uygulama erişimi dizin verilerini Active Directory ve sorgu yapılandırabilirsiniz.

<a id="orgauthoptions"></a>
## <a name="organizational-account-authentication-options"></a>Kurumsal hesap kimlik doğrulama seçenekleri

**Authentication Yapılandır** iletişim Azure Active Directory (Office 365 içeren Azure AD) veya Windows Server Active Directory (AD) hesabı kimlik doğrulaması için çeşitli seçenekler sağlar:

- [Bulut - tek bir kurumun](#orgauthsingle) (Azure AD veya Azure AD ile dizin tümleştirmesi kullanarak AD)
- [Bulut - çoklu kuruluş](#orgauthmulti) (Azure AD veya Azure AD ile dizin tümleştirmesi kullanarak AD)
- [Şirket içi](#orgauthonprem) (AD)

Azure AD seçeneklerden birini denemek istiyor, ancak henüz bir hesabınız yoksa [için bir Azure AD hesabı kaydolmak için buraya tıklayın](https://go.microsoft.com/fwlink/?LinkId=309942).

> [!NOTE]
> Azure AD seçeneklerden birini seçerseniz, projenizin bir veritabanı gerektirir ve Azure AD kiracınız için bir genel yönetici hesabınızda oturum açmak zorunda. İçin bir kurumsal hesap adını ve parolasını girin (örneğin, admin@contoso.onmicrosoft.com), Azure AD kiracınız için yönetici izinlerine sahip.
> 
> **Bir Microsoft hesabının kimlik bilgilerini girin yok (örneğin, contoso@hotmail.com) oturum açma iletişim kutusunda.**


<a id="orgauthsingle"></a>
### <a name="cloud---single-organization-authentication"></a>Bulut - tek kurum kimlik doğrulaması

![Tek bir kurumun kimlik doğrulaması](creating-web-projects-in-visual-studio/_static/image24.png)

Bir Azure AD içinde tanımlanan kullanıcı hesapları için kimlik doğrulamasını etkinleştirmek istiyorsanız bu seçeneği [Kiracı](https://technet.microsoft.com/library/jj573650.aspx). Örneğin, contoso.com sitesidir ve onu contoso.onmicrosoft.com kiracısında olan Contoso şirket çalışanları için kullanıma açık olacaktır. Kullanıcıların diğer kiracılardan uygulamaya erişmesine izin vermek için Azure AD yapılandırmanız mümkün olmayacaktır.

#### <a name="domain"></a>Etki Alanı

Uygulama, örneğin ayarlamak istediğiniz Azure AD etki alanını girin: `contoso.onmicrosoft.com`. Varsa bir [özel etki alanı](http://www.cloudidentity.com/blog/2013/04/14/adding-a-custom-domain-to-your-windows-azure-ad/), gibi `contoso.com` yerine `contoso.onmicrosoft.com`, buraya girebilirsiniz.

#### <a name="access-level"></a>Erişim düzeyi

Uygulama gerekiyorsa sorgu veya grafik API'sini kullanarak dizin bilgilerini güncelleştirin, seçin **çoklu oturum açma, dizin verilerini okuma** veya **çoklu oturum açma, okuma ve yazma dizin verilerini**. Aksi takdirde seçin **çoklu oturum açma**. Daha fazla bilgi için bkz: [uygulama erişim düzeyleri](https://msdn.microsoft.com/library/windowsazure/b08d91fa-6a64-4deb-92f4-f5857add9ed8#BKMK_AccessLevels) ve [sorguya Azure AD grafik API'sini kullanarak](https://msdn.microsoft.com/library/windowsazure/dn151791.aspx).

#### <a name="application-id-uri"></a>Uygulama Kimliği URI'si

Varsayılan olarak, Azure AD etki alanının önüne proje adı eklenerek şablon sizin için bir uygulama kimliği URI'sini oluşturur. Örneğin, proje adı ise `Example` ve etki alanının `contoso.onmicrosoft.com`, uygulama kimliği URI hale `https://contoso.onmicrosoft.com/Example`. Uygulama Kimliği URI'sini el ile belirtmek istiyorsanız, genişletin **diğer seçenekler** bölümünde ve metin kutusuna uygulama kimliği URI'sini girin. Uygulama Kimliği URI'si ile başlamalıdır `https://`.

Aynı uygulama kimliği URI'si projesi için Visual Studio kullanarak bir Azure AD içinde zaten sağlanmış bir uygulama varsa, varsayılan olarak, var olan uygulamaya yeni bir sağlama yerine Proje bağlanır. Bu durumda sağlanacak yeni bir uygulama istiyorsanız temizleyin **zaten aynı Kimliğe sahip bir uygulama girişi üzerine** onay kutusu.

Varsa **üzerine yaz** onay kutusu temizlendiğinde ve Visual Studio bulur aynı uygulama kimliği URI'si ile var olan bir uygulama, bir sayı giderek kullanmak için URI eklenerek yeni bir URI oluşturur. Örneğin, proje adı olduğunu varsayın `Example`, metin kutusunu boş bırakın, temizlemeniz **üzerine yaz** onay kutusunu ve Azure AD kiracısı URI ile bir uygulama zaten `https://contoso.onmicrosoft.com/Example`. Bu durumda, yeni bir uygulama kimliği URI'si gibi bir uygulama ile hazırlanacak `https://contoso.onmicrosoft.com/Example_20130619330903`.

#### <a name="provisioning-the-application-in-azure-ad"></a>Azure AD'de uygulama sağlama

Azure AD'de uygulama sağlamak veya proje varolan bir uygulamaya bağlanmak için Visual Studio genel yönetici kimlik bilgileri etki alanı için gerekir. Tıkladığınızda **Tamam** içinde **Authentication Yapılandır** iletişim kutusu, kullanıcı adı ve parola, belirtilen etki alanı için genel bir yönetici için istenir. Daha sonra tıkladığınızda **proje oluştur** içinde **yeni ASP.NET projesi** iletişim kutusunda, Visual Studio Azure AD'de uygulama sağlamasını yapar. Bu işlemin bir parçası olarak Visual Studio istemci gizli değerleri oluşturulduktan sonra bir yıl sona Web.config dosyasında katıştırır olduğunu unutmayın.

Kullanan uygulamaları oluşturma hakkında bilgi için **bulut - tek bir kurumun** kimlik doğrulaması, aşağıdaki kaynaklara bakın:

- [Azure kimlik doğrulaması](../2012/windows-azure-authentication.md)
- [Azure AD kullanarak, Web uygulamanıza oturum açma ekleme](https://msdn.microsoft.com/library/windowsazure/dn151790.aspx)
- [Azure Active Directory ile ASP.NET Uygulamaları geliştirme](../../../identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory.md)
- [Azure AD ile ASP.NET Web API güvenliğini sağlama ve Microsoft OWIN bileşenleri](https://msdn.microsoft.com/magazine/dn463788.aspx)

Öğreticiler, Visual Studio 2013 için henüz güncelleştirilmemiş; Bazı hangi öğreticileri, el ile yapmak için doğrudan otomatik Visual Studio 2013'te.

<a id="orgauthmulti"></a>
### <a name="cloud---multi-organization-authentication"></a>Bulut - çoklu kurum kimlik doğrulaması

![Birden çok kurum kimlik doğrulaması](creating-web-projects-in-visual-studio/_static/image25.png)

Birden çok Azure AD içinde tanımlanan kullanıcı hesapları için kimlik doğrulamasını etkinleştirmek istiyorsanız bu seçeneği [kiracılar](https://technet.microsoft.com/library/jj573650.aspx). Örneğin, contoso.com sitesidir ve onu contoso.onmicrosoft.com kiracısında olan Contoso şirket çalışanları ve fabrikam.onmicrosoft.com kiracısında olan Fabrikam şirket çalışanları tarafından yüklenebilir.

Girdiğiniz ayarları ve uygulama adımı sağlama benzer [tek kurum kimlik doğrulaması](#orgauthsingle).

Kullanan uygulamaları oluşturma hakkında bilgi için **bulut - çoklu kuruluş** kimlik doğrulaması, aşağıdaki kaynaklara bakın:

- [Azure Active Directory, ASP.NET kolay Web uygulama tümleştirmesi &amp; Visual Studio](https://blogs.msdn.com/b/active_directory_team_blog/archive/2013/06/26/improved-windows-azure-active-directory-integration-with-asp-net-amp-visual-studio.aspx) Active Directory ekip blogu üzerinde.
- [Azure AD ile çok Kiracılı Web uygulamaları geliştirme](https://msdn.microsoft.com/library/windowsazure/dn151789.aspx) Öğreticisi. Öğretici için Visual Studio 2013 henüz güncelleştirilmedi; öğretici, el ile yapmak için ne yönlendiren bazıları otomatik Visual Studio 2013'te.
- [Oturum açmadan önce Yukarı kendi birden çok kuruluşlar ASP.NET uygulaması ile oturum açması](http://www.cloudidentity.com/blog/2013/10/26/you-have-to-sign-up-with-your-own-multiple-organizations-asp-net-app-before-you-can-sign-in/). Blog nasıl ortak bir sorun kişiler çözümleneceği açıklanır Vittorio Bertocci tarafından çok kurum kimlik doğrulaması kullanan bir proje oluşturma sırasında karşılaşırsınız.

<a id="orgauthonprem"></a>
### <a name="on-premises-organizational-authentication"></a>Şirket içi Kurumsal kimlik doğrulama

![Şirket içi Kurumsal kimlik doğrulama](creating-web-projects-in-visual-studio/_static/image26.png)

Windows Server Active Directory (AD) tanımlanan kullanıcı hesapları için kimlik doğrulamasını etkinleştirmek istiyorsanız ve Azure AD kullanmak istemiyorsanız bu seçeneği belirleyin. Bir Intranet sitesine veya bir Internet sitesini oluşturmak için bu seçeneği kullanın. Bir Internet sitesini için Active Directory Federasyon Hizmetleri (ADFS) AD için erişim sağlamak için kullanın. Daha fazla bilgi için bkz: [ile şirket içi Kurumsal kimlik doğrulama seçeneği (ADFS) ASP.NET Visual Studio 2013'te kullanmak](http://www.cloudidentity.com/blog/2014/02/12/use-the-on-premises-organizational-authentication-option-adfs-with-asp-net-in-visual-studio-2013/).

Bir Intranet sitesine için alternatif olarak, seçebilirsiniz [Windows kimlik doğrulaması](#winauth) yerine bu seçeneği. Windows kimlik doğrulaması seçeneği için bir meta veri belgesi URL'si sağlamak zorunda değilsiniz. Ancak, Windows kimlik doğrulaması, özelliği Active Directory'de uygulama erişimi denetleme veya sorgu dizin verilerini sağlamaz.

#### <a name="on-premises-authority"></a>Şirket içi yetkili

Meta veri belgesi işaret eden bir URL girin. Meta veri belgesi yetkilinin koordinatlarını içerir. Uygulama, oturum açma web akışı sürücü için bu koordinatları kullanacak.

#### <a name="application-id-uri"></a>Uygulama Kimliği URI'si

AD bu uygulamayı tanımlamak ya da Visual Studio oluşturmak izin vermek için boş bırakın kullanabileceği benzersiz bir URI sağlayın.

<a id="nextsteps"></a>
## <a name="next-steps"></a>Sonraki adımlar

Bu belge, Visual Studio 2013'te yeni bir ASP.NET web projesi oluşturmak için bazı temel Yardım sağlamıştır. Visual Studio web geliştirme için kullanma hakkında daha fazla bilgi için bkz: [https://www.asp.net/visual-studio/](../../index.md).
