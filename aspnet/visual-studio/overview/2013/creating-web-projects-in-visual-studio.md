---
uid: visual-studio/overview/2013/creating-web-projects-in-visual-studio
title: Visual Studio 2013'te ASP.NET Web projeleri oluşturma | Microsoft Docs
author: tdykstra
description: Bu konuda, burada güncelleştirme 3 ile Visual Studio 2013 ASP.NET web projeleri oluşturmak için seçenekler web geliştirme c yeni özelliklerinden bazıları açıklanmaktadır...
ms.author: aspnetcontent
ms.date: 12/01/2014
ms.assetid: 61941e64-0c0d-4996-9270-cb8ccfd0cabc
msc.legacyurl: /visual-studio/overview/2013/creating-web-projects-in-visual-studio
msc.type: authoredcontent
ms.openlocfilehash: b57492a51f65e7ca861a7c354ded6ab170a92488
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37814526"
---
<a name="creating-aspnet-web-projects-in-visual-studio-2013"></a>Visual Studio 2013'te ASP.NET Web projeleri oluşturma
====================
tarafından [Tom Dykstra](https://github.com/tdykstra)

> Bu konuda, Visual Studio 2013 güncelleştirme 3 ile ASP.NET web projeleri oluşturmak için seçenekler açıklanmaktadır.
> 
> Web geliştirme Visual Studio'nun önceki sürümlere göre yeni özelliklerinden bazıları şunlardır:
> 
> - Bu teklifi oluşturmak için basit bir kullanıcı Arabirimi projeleri [desteklemek için birden çok ASP.NET çerçeve](#add) (Web formları, MVC ve Web API'si).
> - [ASP.NET Identity](#indauth), tüm ASP.NET çerçeveleri ve çalışan aynı web barındırma yazılımı dışında IIS ile çalışan yeni bir ASP.NET üyelik sistemi.
> - Kullanımını [önyükleme](#bootstrap) duyarlı tasarım ve tema özellikler sağlamak için.
> - Yalnızca MVC için gibi sunmak için kullanılan Web formları için yeni özellikler [otomatik test projesi oluşturmayı](#testproj) ve [Intranet site şablonunu](#winauth).
> 
> Azure Cloud Services veya Azure mobil hizmetler için web projeleri oluşturma hakkında daha fazla bilgi için bkz: [Azure Cloud Services ve ASP.NET ile çalışmaya başlama](https://azure.microsoft.com/documentation/articles/cloud-services-dotnet-get-started/) ve [Azure mobil hizmetler .NET ile puan tablosu uygulaması oluşturma Arka uç](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/).


<a id="prerequisites"></a>
## <a name="prerequisites"></a>Önkoşullar

Bu makalede açıklanan [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566) ile [güncelleştirme 3'ü](https://go.microsoft.com/fwlink/?linkid=397827&amp;clcid=0x409) yüklü.

<a id="wap"></a>
## <a name="web-application-projects-versus-web-site-projects"></a>Web sitesi projeleri ve Web Uygulama projeleri

ASP.NET web projeleri iki tür arasında bir seçim sunar: *web uygulama projeleri* ve *web sitesi projeleri*. Bu makale yalnızca web uygulama projeleri için geçerlidir ve yeni geliştirme web uygulaması projelerinde öneririz. Daha fazla bilgi için [Web Application Projects versus Web sitesi projeleri Visual Studio'da](https://msdn.microsoft.com/library/dd547590(v=vs.120).aspx) MSDN sitesinden.

<a id="overview"></a>
## <a name="overview-of-web-application-project-creation"></a>Web uygulaması projesi oluşturma genel bakış

Aşağıdaki adımlar, bir web projesi oluşturma işlemini gösterir:

1. Tıklayın **yeni proje** içinde **Başlat** sayfası veya **dosya** menüsü.
2. İçinde **yeni proje** iletişim kutusunda, tıklayın **Web** sol bölmede ve **ASP.NET Web uygulaması** orta bölmesinde.

    ![Yeni Proje iletişim kutusu](creating-web-projects-in-visual-studio/_static/image1.png)

    Seçebileceğiniz **bulut** oluşturmak için sol bölmede bir [Azure bulut hizmeti](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy), [Azure mobil hizmeti](https://msdn.microsoft.com/library/windows/apps/dn629482.aspx), veya [Azure WebJob](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-webjobs). Bu konuda, bu şablonların ele alınmamıştır.
3. Sağ bölmede **projeye Application Insights Ekle** sistem durumu ve uygulamanız için kullanımı izleme istiyorsanız kutuyu. Daha fazla bilgi için [web uygulamalarının performansını izleme](https://azure.microsoft.com/documentation/articles/app-insights-web-monitor-performance/).
4. Proje belirtin **adı**, **konumu**ve diğer seçenekleri ve ardından **Tamam**.

    **Yeni ASP.NET projesi** iletişim kutusu görüntülenir.

    ![Yeni Proje iletişim kutusu](creating-web-projects-in-visual-studio/_static/image2.png)
5. Bir şablon seçin.

    ![Bir şablon seçin](creating-web-projects-in-visual-studio/_static/image3.png)
6. Şablonda bulunmayan ek çerçeveler için destek eklemek istiyorsanız, uygun onay kutusuna tıklayın. (Gösterilen örnekte, MVC ve/veya Web API'sini bir Web Forms projesine ekleyebilirsiniz.)

    ![Çerçeveleri ekleyin](creating-web-projects-in-visual-studio/_static/image4.png)
7. <a id="testproj"></a>Birim testi projesi eklemek istiyorsanız, tıklayın **birim testleri ekleme**.

    ![Birim testleri ekleme](creating-web-projects-in-visual-studio/_static/image5.png)
8. Şablon varsayılan olarak sağlanan korumanın daha çok farklı kimlik doğrulama yöntemi isterseniz **kimlik doğrulamayı Değiştir**.

    ![Kimlik doğrulaması düğmesi yapılandırın](creating-web-projects-in-visual-studio/_static/image6.png)

    ![Kimlik doğrulaması iletişim kutusunu yapılandırma](creating-web-projects-in-visual-studio/_static/image7.png)

<a id="azurenewproj"></a>
### <a name="create-a-web-app-or-virtual-machine-in-azure"></a>Azure'da bir web uygulaması ya da sanal makineyi oluşturma

Visual Studio, web uygulamalarını barındırmak için Azure hizmetleriyle çalışmak kolaylaştıran özellikler içerir. Örneğin, aşağıdakilerin tümü doğrudan Visual Studio IDE'den yapabilirsiniz:

- Oluşturun ve web uygulamalarını ya da sanal makinelerin uygulamanızı Internet üzerinden kullanılabilir hale getirme yönetin.
- Bulutta çalışırken uygulama tarafından oluşturulan günlükleri görüntüleyin.
- Uygulama bulutta çalışırken uzaktan hata ayıklama modunda çalıştırabilir.
- Viiew ve SQL veritabanları gibi diğer Azure Hizmetleri yönetin.

Yapabilecekleriniz [bir Azure hesabı oluşturun](https://www.windowsazure.com/pricing/free-trial/) ücretsiz web apps gibi temel hizmetleri içeren ve MSDN abonesiyseniz yapabilecekleriniz [Avantajlarınızı etkinleştirin](https://azure.microsoft.com/pricing/member-offers/visual-studio-subscriptions/) , size ek Azure hizmetlerinde aylık KREDİLERİ Hizmetler. 

Varsayılan olarak **yeni ASP.NET projesi** iletişim kutusu, bir web uygulaması veya sanal makine için yeni bir web projesi oluşturmanıza olanak sağlar. Yeni web uygulaması ya da sanal makineyi oluşturmak istemiyorsanız, Temizle **bulutta Barındır** onay kutusu.

![Uzak kaynaklar oluştur](creating-web-projects-in-visual-studio/_static/image8.png)

Onay kutusu resim yazısı olabilir **bulutta Barındır** veya **uzak kaynaklar Oluştur**, ve her iki durumda da etkisi aynıdır. Onay kutusunu seçili bırakın, Visual Studio varsayılan olarak Azure App Service'te bir web uygulaması oluşturur. Açılan kutu olarak değiştirmek için kullanabileceğiniz bir **sanal makine** tercih ederseniz. Azure'a henüz oturum açmadıysanız, Azure kimlik bilgileri istenir. Oturum açtıktan sonra bir iletişim kutusu, Visual Studio projeniz için oluşturacak kaynakları yapılandırmanızı sağlar. Aşağıdaki çizimde bir web uygulaması için iletişim kutusu gösterir. bir sanal makine oluşturmak isterseniz farklı seçenekler görüntülenir.

![Azure uygulama ayarlarını yapılandırma](creating-web-projects-in-visual-studio/_static/image9.png)

Bu işlem, Azure kaynaklarını oluşturmak için kullanma hakkında daha fazla bilgi için bkz. [Azure ve ASP.NET ile çalışmaya başlama](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet) ve [Visual Studio'yla bir web sitesi için bir sanal makine oluşturma](https://azure.microsoft.com/documentation/articles/virtual-machines-dotnet-create-visual-studio-powershell/).

Bu makalenin geri kalanında kullanılabilir şablonları ve bunların seçenekleri hakkında daha fazla bilgi sağlar. Makaleyi şablonlarında kullanılan önyükleme, Düzen ve tema framework da tanıtılmaktadır.

<a id="vs2013"></a>
## <a name="visual-studio-2013-web-project-templates"></a>Visual Studio 2013 Web projesi şablonları

Visual Studio, web projeleri oluşturmak için şablonlar kullanır. Bir proje şablonu, dosya ve klasörleri yeni projede oluşturabilir, NuGet paketlerini yükleme ve ilkel çalışan bir uygulama için örnek kodlar sağladık. Şablonları en son web standartlarını uygular ve yanı sıra ASP.NET teknolojilerini kullanan bir atlama vermek en iyi uygulamaları kendi uygulamanızı oluşturmaya başlamak göstermek için tasarlanmıştır.

Visual Studio 2013, .NET 4.5 veya .NET framework'ün sonraki sürümlerini hedefleyen projeler için web projesi şablonları için aşağıdaki seçenekleri sağlar:

- [Boş şablon](#empty)
- [Web Forms şablonu](#wf)
- [MVC şablonu](#mvc)
- [Web API şablonu](#webapi)
- [Tek sayfalı uygulama şablonu](#spa)
- [Azure mobil hizmeti şablonu](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/)
- [Visual Studio 2012 şablonları](#vs2012)

Sağlayan Visual Studio uzantısı da yükleyebileceğiniz bir [Facebook şablon](#facebook).

.NET 4'ü hedefleyen projeler oluşturma hakkında daha fazla bilgi için bkz: [Visual Studio 2012 şablonları](#vs2012) bu konuda.

Mobil istemciler için ASP.NET uygulamaları oluşturma hakkında daha fazla bilgi için bkz: [ASP.NET Mobil desteği](../../../mobile/index.md).

<a id="empty"></a>
### <a name="empty-template"></a>Boş şablon

Çıplak minimum klasörleri ve dosyaları bir proje dosyası gibi bir ASP.NET web uygulaması için boş şablon sağlar (*.csproj* veya. *vbproj*) ve bir *Web.config* dosya. Altındaki onay kutularını kullanarak Web formları, MVC ve/veya Web API'si için destek ekleyebilirsiniz **klasörleri ekleyin ve çekirdek başvuruları:** etiketi.

Boş şablon için hiçbir kimlik doğrulama seçenekleri kullanılabilir. Örnek uygulamalarda kimlik doğrulama işlevi uygulanır ve boş şablon, örnek bir uygulama oluşturulmaz.

<a id="wf"></a>
### <a name="web-forms-template"></a>Web Forms şablonu

Web formları framework UI ve verileri zengin web sitelerini hızlı bir şekilde izin veren aşağıdaki özellikleri derleme sağlar özelliklere erişim:

- Visual Studio WYSIWYG tasarımcıda.
- HTML ve özellikleri ve stilleri ayarlayarak özelleştirebilirsiniz işleme sunucu denetimleri.
- Veri erişimi ve verileri görüntüleme denetimleri zengin bir listesini.
- İstediğiniz programı olayları ortaya koyan bir olay modeli WPF gibi bir istemci uygulama programlama.
- Otomatik koruma durumu (veriler) HTTP istekleri arasında.

Genel olarak, bir Web Forms uygulaması oluşturmak için ASP.NET MVC Çerçevesi'ni kullanarak uygulamanın aynısını oluşturmaktan programlama daha az çaba gerekir. Ancak, Web Forms yalnızca hızlı uygulama geliştirme için değil. Birçok karmaşık ticari uygulamalar ve Web Forms üzerinde oluşturulmuş çerçeveleri vardır.

Web Forms sayfası ve sayfadaki denetimleri tarayıcıya gönderilen biçimlendirmeyi çoğunu otomatik olarak oluşturmak için ASP.NET MVC sunan HTML üzerinde ayrıntılı denetim türü yok. Sayfalar ve denetimler yapılandırmak için bildirime dayalı modeli yazmanız gerekir, ancak bazı HTML ve HTTP davranışını gizler kod miktarını en aza indirir. Örneğin, her zaman tam olarak hangi biçimlendirme bir denetim tarafından oluşturulmuş olabilir belirlemek mümkün değildir.

Web formları framework kendisini ASP.NET MVC olarak kolayca için desen tabanlı geliştirme yöntemleri gibi kiralamak değil [test odaklı geliştirme](http://en.wikipedia.org/wiki/Test-driven_development), [görev ayrımı nettir](http://en.wikipedia.org/wiki/Separation_of_concerns), [, tersine çevirme Denetim](http://en.wikipedia.org/wiki/Inversion_of_control), ve [bağımlılık ekleme](http://en.wikipedia.org/wiki/Dependency_injection). Bu sayede kod factored yazmak istiyorsanız, aşağıdakileri yapabilirsiniz; ASP.NET MVC çerçevesi içinde olduğu gibi otomatik değildir yeterlidir. [ASP.NET Web Forms MVP](http://webformsmvp.com/) proje, Web Forms sunmak üzere tasarlanmış hızlı geliştirme sürdürürken ayrımı ayrılması ve Test Edilebilirlik kolaylaştıran bir yaklaşımı gösterir. Microsoft SharePoint Web Forms MVP yerleşik olarak bulunur.

Web Forms şablonu kullanan bir örnek Web Forms uygulaması oluşturur [önyükleme](#bootstrap) duyarlı tasarım ve Tema oluşturma özellikleri sağlamak için. Giriş sayfası aşağıda gösterilmektedir.

![Web Forms şablonu uygulama ana sayfası](creating-web-projects-in-visual-studio/_static/image10.png)

Web Forms hakkında daha fazla bilgi için bkz: [ASP.NET Web Forms](https://asp.net/web-forms). Web Forms şablon sizin için neler yaptığı hakkında daha fazla bilgi için bkz: [Visual Studio 2013'ü kullanarak temel bir Web Forms uygulaması oluşturma](https://blogs.msdn.com/b/webdev/archive/2013/12/19/building-a-basic-web-forms-application-using-visual-studio-2013.aspx).

<a id="mvc"></a>
### <a name="mvc-template"></a>MVC şablonu

ASP.NET MVC, geliştirme desen tabanlı uygulamalar gibi kolaylaştırmak için tasarlanmış [test odaklı geliştirme](http://en.wikipedia.org/wiki/Test-driven_development), [görev ayrımı nettir](http://en.wikipedia.org/wiki/Separation_of_concerns), [denetimi tersine çevirme](http://en.wikipedia.org/wiki/Inversion_of_control), ve [bağımlılık ekleme](http://en.wikipedia.org/wiki/Dependency_injection). Framework, iş mantığı katmanı kendi sunu katmanına bir web uygulamasından ayırarak teşvik eder. Uygulama (M) modelleri, görünümleri (V) ve denetleyicileri (C) bölerek, ASP.NET MVC, daha büyük uygulamaların karmaşıklığı yönetmek kolaylaştırabilir.

ASP.NET MVC ile doğrudan HTML ve fazla HTTP Web Forms ile çalışır. Örneğin, Web Forms, HTTP istekleri arasında durum otomatik olarak koruyabilir, ancak açıkça MVC'de kodunu zorunda. MVC modelin avantajı, tam olarak, uygulamanızın ne yaptığını ve web ortamında nasıl davrandığını üzerinden denetimini ele geçirmesine olanak sağlayan. Olumsuz yönüyse, daha fazla kod yazmak zorunda olduğunuz ' dir.

MVC power geliştiriciler framework kendi uygulama gereksinimlerinize göre özelleştirme olanağı tanıyacak Genişletilebilir olmak üzere tasarlanmıştır. Ayrıca, ASP.NET MVC kaynak kodunu bir OSI lisansı altında sunulur.

MVC şablonu kullanan bir örnek MVC 5 uygulaması oluşturur [önyükleme](#bootstrap) duyarlı tasarım ve Tema oluşturma özellikleri sağlamak için. Giriş sayfası aşağıda gösterilmektedir.

![MVC örnek uygulaması](creating-web-projects-in-visual-studio/_static/image11.png)

MVC hakkında daha fazla bilgi için bkz: [ASP.NET MVC](https://asp.net/mvc). MVC 4 şablon seçme hakkında daha fazla bilgi için bkz: [Visual Studio 2012 şablonları](#vs2012) bu makalenin ilerleyen bölümlerinde.

<a id="webapi"></a>
### <a name="web-api-template"></a>Web API şablonu

Web API şablonu MVC tabanlı API Yardım sayfaları dahil olmak üzere Web API'si, temel bir örnek web hizmeti oluşturur.

ASP.NET Web API istemciler, tarayıcılar ve mobil cihazlar dahil olmak üzere geniş bir yelpazede ulaşan HTTP hizmetlerini oluşturmayı kolaylaştıran bir çerçevedir. ASP.NET Web API'si, .NET Framework üzerinde RESTful hizmetleri oluşturmak için ideal bir platformdur.

Web API şablonu bir örnek web hizmeti oluşturur. Aşağıdaki çizimler örnek Yardım sayfalarını gösterir.

![Web API Yardım sayfası](creating-web-projects-in-visual-studio/_static/image12.png)

![Web API Yardım sayfası API'si almak için](creating-web-projects-in-visual-studio/_static/image13.png)

Web API'si hakkında daha fazla bilgi için bkz. [ASP.NET Web API](https://asp.net/web-api).

<a id="spa"></a>
### <a name="single-page-application-template"></a>Tek sayfalı uygulama şablonu

JavaScript, HTML 5 kullanan bir örnek uygulamanın tek sayfa uygulama (SPA) şablon oluşturur ve [KnockoutJS](http://knockoutjs.com/) istemci ve sunucu üzerinde ASP.NET Web API.

SPA şablon için tek kimlik doğrulaması seçeneği [bireysel kullanıcı hesapları](#indauth).

Aşağıdaki çizim SPA şablonu oluşturan örnek uygulamanın ilk durumunu gösterir.

![SPA örnek uygulaması](creating-web-projects-in-visual-studio/_static/image14.png)

SPA şablonu kullanarak bir uygulama oluşturma hakkında daha fazla bilgi için bkz: [Web API'si - Dış kimlik doğrulama hizmetleri](../../../web-api/overview/security/external-authentication-services.md).

KnockoutJS dışında yeni JavaScript çerçevesi kullanan ek SPA şablonlarını ve ASP.NET tek sayfalık uygulamalar hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [ASP.NET tek sayfa uygulaması](../../../single-page-application/index.md).
- [VS2013 RC için güvenlik özellikleri SPA şablondaki anlama](https://blogs.msdn.com/b/webdev/archive/2013/09/20/understanding-security-features-in-spa-template.aspx)
- [Tek sayfa uygulamaları: ASP.NET ile Modern, hızlı yanıt veren Web uygulamaları oluşturun](https://msdn.microsoft.com/magazine/dn463786.aspx)

<a id="facebook"></a>
### <a name="facebook-template"></a>Facebook şablonu

Yükleyebileceğiniz bir [Facebook şablonu sağlayan Visual Studio Uzantısı](https://go.microsoft.com/fwlink/?LinkID=509965&amp;clcid=0x409). Bu şablon, Facebook web sitesinin içinde çalışacak şekilde tasarlanmıştır örnek bir uygulama oluşturur. ASP.NET MVC tabanlı ve Web API'si için gerçek zamanlı güncelleştirme işlevini kullanır.

Facebook uygulamaları Facebook sitede çalıştırın ve Facebook'ın kimlik doğrulama kullanan Facebook şablon için hiçbir kimlik doğrulama seçenekleri kullanılabilir.

ASP.NET Facebook uygulamaları hakkında daha fazla bilgi için bkz. [MVC Facebook API güncelleştiriliyor](https://blogs.msdn.com/b/webdev/archive/2014/06/10/updating-the-mvc-facebook-api.aspx).

<a id="vs2012"></a>
### <a name="visual-studio-2012-templates"></a>Visual Studio 2012 şablonları

Visual Studio 2013 web proje oluşturma iletişim kutusu, Visual Studio 2012'de kullanılabilen bazı şablonlarına erişim sağlamaz. Bu şablonlardan birini kullanmak istiyorsanız, Visual Studio yeni proje iletişim kutusunun sol bölmesinde Visual Studio 2012 düğümünde tıklayabilirsiniz.

![Visual Studio 2012 şablonları](creating-web-projects-in-visual-studio/_static/image15.png)

**Visual Studio 2012** sahip olmadığınız aşağıdaki web şablonları seçtiğiniz düğümü sağlar erişmek için varsayılan şablonları listesinde Visual Studio 2013 için:

- ASP.NET MVC 4 Web uygulaması
- ASP.NET dinamik veri varlıkları Web uygulaması
- ASP.NET AJAX Sunucu denetimi
- ASP.NET AJAX Sunucu denetim Genişleticisi
- ASP.NET sunucu denetimi

<a id="bootstrap"></a>
## <a name="bootstrap-in-the-visual-studio-2013-web-project-templates"></a>Visual Studio 2013 web proje şablonlarında önyükleme

Visual Studio 2013 proje şablonlarını kullanma [önyükleme](http://getbootstrap.com/), Twitter tarafından oluşturulan bir düzen ve Tema oluşturma çerçevesi. Önyükleme CSS3 düzenleri farklı bir tarayıcı penceresi boyutları için dinamik olarak uyum sağlayabilen anlamına gelir esnek tasarım sağlamak için kullanır. Örneğin, bir geniş tarayıcı penceresinde Web Forms şablonu tarafından oluşturulan giriş sayfası aşağıdaki gibi görünür:

![Web Forms şablonu uygulama ana sayfası](creating-web-projects-in-visual-studio/_static/image16.png)

Pencerenin daraltmak ve yatay olarak düzenlenmiş sütun dikey düzenlemeleri Taşı:

![Önyükleme dikey sütun düzenleme](creating-web-projects-in-visual-studio/_static/image17.png)

Pencere biraz daha fazla daraltmak ve yatay üst menü dikey yönlendirilmiş bir menüye genişletmek için tıklayabileceği simge dönüştürür:

![Önyükleme menüsü simgesi](creating-web-projects-in-visual-studio/_static/image18.png)

![Dikey hizalama önyükleme menüsü](creating-web-projects-in-visual-studio/_static/image19.png)

Bir değişiklik uygulama görünümü sunmalarına kolayca etkilemek için önyükleme'nın Tema oluşturma özelliğini de kullanabilirsiniz. Örneğin, temasını değiştirmek için aşağıdaki adımları yapabilirsiniz.

1. Tarayıcınızda, Git [ http://Bootswatch.com ](http://Bootswatch.com)bir tema seçin ve ardından **indirme**. (Bu indirir *bootstrap.min.css* CSS kodunu incelemek isterseniz, varsayılan olarak; alma *bootstrap.css* küçültülmüş sürümü yerine.)
2. İndirilen CSS dosyasının içeriğini kopyalayın.
3. Visual Studio'da yeni bir oluşturma **stil sayfası** adlı dosya *önyükleme-theme.css* içinde *içerik* klasörü ve Yapıştır indirilen CSS kodunu buna.
4. Açık *uygulama\_Start/Bundle.config* değiştirip *bootstrap.css* için *önyükleme-theme.css*.

Projeyi yeniden çalıştırın ve uygulamayı yeni bir görünüme sahiptir. Aşağıdaki çizim Amelia tema etkisini gösterir:

![Önyükleme Amelia tema](creating-web-projects-in-visual-studio/_static/image20.png)

Birçok önyükleme tema kullanılabilir ücretsiz ve premium sürümleri. Önyükleme sunduğu birçok farklı kullanıcı Arabirimi bileşenleri gibi [açılan listeler](http://twitter.github.io/bootstrap/components.html#dropdowns), [düğme grupları](http://twitter.github.io/bootstrap/components.html#buttonGroups), ve [simgeler](http://twitter.github.io/bootstrap/base-css.html#images). Önyükleme hakkında daha fazla bilgi için bkz: [önyükleme site](http://twitter.github.io/bootstrap/).

Visual Studio'da Web formları tasarımcısını kullanırsanız, tüm etkilerini önyükleme Temalar veya esnek düzen değişiklikleri doğru bir şekilde göstermiyor şekilde CSS3, Tasarımcı desteklemediğini aklınızda bulundurun. Ancak, bir tarayıcı ile görüntülendiğinde Web formları sayfaları doğru şekilde görüntülenir.

<a id="add"></a>
## <a name="adding-support-for-additional-frameworks"></a>Ek çerçeveler için destek ekleme

Bir şablon seçtiğinizde şablon tarafından kullanılan çerçeveleri için onay kutusu otomatik olarak seçilir. Örneğin, **Web Forms** şablon **Web Forms** onay kutusu seçilidir ve işaretini kaldıramazsınız.

![Bir şablon seçin](creating-web-projects-in-visual-studio/_static/image21.png)

![Çerçeveleri ekleyin](creating-web-projects-in-visual-studio/_static/image22.png)

Şablon, proje oluşturulduğunda bu çerçevesi için destek eklemek için bulunmayan bir çerçeve için onay kutusunu seçebilirsiniz. Örneğin, Web Forms kullanımını etkinleştirmek için *.aspx* MVC şablonu seçtiğinizde sayfaları seçin **Web Forms** onay kutusu. Veya Web Forms şablonu kullanılırken MVC etkinleştirmek için tıklayın **MVC** onay kutusu. Bir çerçeve eklemek, çalışma zamanı yanı sıra, tasarım zamanı desteği sağlar. Örneğin, bir Web Forms projesine MVC desteği eklerseniz, denetleyicileri ve görünümleri iskelesini mümkün olacaktır.

WebForms ve MVC bir projede birleştirin ve etkinleştirme [kolay URL'leri](http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx) Web formlarında olabilir beklenmeyen yönlendirme sorunlarının olası birden çok hedefe sahip olduğu bir URL. Tanımlanan rotaları ilk olarak öncelik kazanır. Örneğin, bir `Home` denetleyicisi ve bir *Home.aspx* sayfasında `http://contoso.com/home` URL'si için yazılacak *Home.aspx* çağırırsanız `EnableFriendlyUrls` çağırmadanönceyöntemi`MapRoute`yönteminde *RouteConfig.cs*, ya da varsayılan görünümünü aynı URL'ye gidin, `Home` çağırırsanız denetleyicisi `MapRoute` önce `EnableFriendlyUrls`.

Bir çerçeve ekleyerek herhangi bir örnek uygulama işlevi eklemez. Eklerseniz Hayır MVC şablonu seçtiğinizde, Web Forms desteği *Default.aspx* giriş sayfası dosyası oluşturulur. Yalnızca klasörleri, dosyaları ve framework desteklemek için gerekli başvuruları eklenir. Bu nedenle, çerçeveleri eklemek şablonları tarafından oluşturulan örnek uygulamalar içindeki kod tarafından uygulanan kimlik doğrulama seçenekleri değiştirmez. Örneğin, boş şablonu seçin ve ekleyin, Web Forms veya MVC destek, **kimlik doğrulamasını yapılandırma** düğmesi yine de devre dışı bırakılacak.

Aşağıdaki bölümlerde, her onay kutusu etkisini kısaca açıklanmaktadır.

### <a name="add-web-forms-support"></a>Web Forms desteği eklendi

Boş oluşturur *uygulama\_veri* ve *modelleri* klasörleri ve *Global.asax* dosya. Diğer şablonlar için nasıl yönelttiğiniz fark etmez Web Forms onay kutusunu seçerek bu nedenle bunlar zaten boş şablonu dışındaki tüm şablonları tarafından oluşturulur.

Varsayılan olarak, ancak Web Forms kolay URL'leri otomatik olarak etkin Web Forms onay kutusunu seçerek diğer şablonlar için destek eklediğinizde, Web Forms şablonu kolay URL'leri etkinleştirir.

### <a name="add-mvc-support"></a>MVC desteği eklendi

MVC Razor ve Web sayfalarında NuGet paketleri yükler, boş oluşturur *uygulama\_veri*, *denetleyicileri*, *modelleri*, ve *görünümleri*klasörleri oluşturur *uygulama\_Başlat* klasörle *RouteConfig.cs* dosya ve oluşturur *Global.asax* dosya.

### <a name="add-web-api-support"></a>Web API desteği eklendi

Webapı ve Newtonsoft.Json NuGet paketleri yükler, boş oluşturur *uygulama\_veri*, *denetleyicileri*, ve *modelleri* klasörleri oluşturur  *Uygulama\_Başlat* klasörle *WebApiConfig.cs* dosya ve oluşturur *Global.asax* dosya.

<a id="auth"></a>
## <a name="authentication-methods"></a>Kimlik doğrulama yöntemleri

Visual Studio 2013, Web Forms, MVC ve Web API şablonları için birkaç kimlik doğrulama seçenekleri sunar:

- [Kimlik doğrulama yok](#noauth)
- [Bireysel kullanıcı hesapları](#indauth) (ASP.NET Identity, önceden ASP.NET üyeliği olarak biliniyordu)
- [Kurumsal hesaplar](#orgauth) (Windows Server Active Directory veya Azure Active Directory)
- [Windows kimlik doğrulaması](#winauth) (Intranet)

![Kimlik doğrulaması iletişim kutusunu yapılandırma](creating-web-projects-in-visual-studio/_static/image23.png)

<a id="noauth"></a>

### <a name="no-authentication"></a>Kimlik doğrulama yok

Seçerseniz **kimlik doğrulaması yok**Hayır açan gösteren bir kullanıcı Arabirimi, bir üyelik veritabanı ve bir üyelik veritabanı için bağlantı dizesi için hiçbir varlık sınıfları, örnek uygulamanın oturum açma için hiçbir web sayfalarını içerir.

<a id="indauth"></a>
### <a name="individual-user-accounts"></a>Bireysel kullanıcı hesapları

Seçerseniz **bireysel kullanıcı hesapları**, örnek uygulamayı ASP.NET kimliği (eski adıyla ASP.NET üyeliği olarak da bilinir) kullanmak için kullanıcı kimlik doğrulaması için yapılandırılacak. ASP.NET Identity bir hesap adı ve parola sitesinde oluşturarak veya Facebook, Google, Microsoft Account veya Twitter gibi sosyal sağlayıcılardan oturum açarak kaydedin açmasına sağlar. Varsayılan veri ASP.NET ıdentity'de kullanıcı profilleri için üretim sitesi için SQL Server veya Azure SQL veritabanı için dağıtabileceğiniz bir SQL Server LocalDB veritabanına deposudur.

Visual Studio 2013'te bu özellikleri gibi Visual Studio 2012 aynıdır, ancak arka plandaki kod ASP.NET üyelik sistemini yeniden yazıldı. Yeni kod tabanının avantajları şunları içerir:

- Yeni üyelik sistemini dayanır [OWIN](http://owin.org/) ASP.NET formları kimlik doğrulamasını modül yerine. Bu IIS içinde Web Forms veya MVC kullanıyorsanız veya Web API'si ve SignalR Self barındırıyorsanız aynı kimlik doğrulama mekanizması kullanabileceğiniz anlamına gelir.
- Yeni bir üyelik veritabanı Entity Framework Code First tarafından yönetilir ve tüm tabloları değiştirebileceğiniz varlık sınıfları tarafından temsil edilir. Bunun anlamı kolayca veritabanı şemasını ve profil ile ilgili web UI kendi gereksinimlerinize uyacak şekilde özelleştirebilirsiniz ve Code First Migrations'ı kullanarak yaptığınız güncelleştirmeler kolayca dağıtabilirsiniz.

Yeni üyelik sistemini yeni şablonlar, otomatik olarak uygulanır ve el ile .NET 4.5 hedefleyen bir projeye uygulanan veya sonraki bir sürümü olabilir.

ASP.NET Identity yönetebilmek dış müşteriler için olan bir Internet web sitesi oluşturuyorsanız iyi bir seçimdir. Kuruluşunuzun Active Directory kullanıyorsa ya da Office 365 ve çalışanların ve iş ortakları için bir çoklu oturum açma sağlayan bir proje oluşturmak istiyorsanız **Kurumsal hesaplar** seçeneği, daha iyi bir seçenek olabilir.

Bireysel kullanıcı hesapları seçeneği hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [www.ASP.NET/identity](../../../identity/index.md). ASP.NET web sitesinde ASP.NET kimliği hakkında belgeler.
- [Facebook ve Google OAuth2 ve Openıd oturum açma ile bir ASP.NET MVC 5 uygulaması oluşturma](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md). Ayrıca kullanıcı profili verilerini özelleştirme işlemini gösterir.
- [Web API'si - Dış kimlik doğrulama hizmeti](../../../web-api/overview/security/external-authentication-services.md)
- [ASP.NET uygulamanızı Visual Studio 2013'teki dış oturum açma ekleme](https://blogs.msdn.com/b/webdev/archive/2013/06/27/adding-external-logins-to-your-asp-net-application-in-visual-studio-2013.aspx)

<a id="orgauth"></a>
### <a name="organizational-accounts"></a>Kurumsal hesaplar

Seçerseniz **Kurumsal hesaplar**, örnek uygulamayı Azure Active Directory'de (Office 365 içeren Azure AD) kullanıcı hesaplarını temel kimlik doğrulaması için Windows Identity Foundation (WIF) kullanmak için yapılandırılmış veya Windows Server Active Directory. Daha fazla bilgi için [Kurumsal hesap kimlik doğrulama seçenekleri](#orgauthoptions) bu konuda.

<a id="winauth"></a>
### <a name="windows-authentication"></a>Windows Kimlik Doğrulaması

Seçerseniz **Windows kimlik doğrulaması**, örnek uygulama için Windows kimlik doğrulaması IIS modülünü kimlik doğrulaması için kullanmak üzere yapılandırılmış. Uygulamayı Windows kaydedilir ancak olmaz kullanıcı kayıt ekleyen veya UI oturum bir yerel makine hesabı ve Active directory etki alanı ve kullanıcı Kimliğini görüntüler. Bu seçenek, Intranet web siteleri için tasarlanmıştır.

Alternatif olarak, seçerek AD kimlik doğrulaması kullanan bir Intranet siteye oluşturabilirsiniz [şirket içi Kurumsal hesaplar seçeneğinde](#orgauthonprem). Şirket içi seçeneği, Windows Identity Foundation (WIF) yerine Windows kimlik doğrulama modülü kullanır. Şirket içi seçeneği ayarlamak için bazı ek adımlar gerekli, ancak WIF Windows kimlik doğrulaması modülüyle birlikte kullanılamayan özellikleri etkinleştirir. Örneğin, WIF ile Active Directory ve sorgu dizin verilerini uygulama erişimini yapılandırabilirsiniz.

<a id="orgauthoptions"></a>
## <a name="organizational-account-authentication-options"></a>Kurumsal hesap kimlik doğrulama seçenekleri

**Kimlik doğrulamasını yapılandırma** iletişim size Azure Active Directory (Office 365 içeren Azure AD) veya Windows Server Active Directory (AD) hesabı kimlik doğrulaması için çeşitli seçenekler sunar:

- [Bulut - tek kurum](#orgauthsingle) (Azure AD veya Azure AD ile dizin tümleştirmesi kullanılarak AD)
- [Bulut - birden çok kuruluş](#orgauthmulti) (Azure AD veya Azure AD ile dizin tümleştirmesi kullanılarak AD)
- [Şirket içi](#orgauthonprem) (AD)

Azure AD seçeneklerden birini denemek istiyor, ancak henüz bir hesabınız yok mu [bir Azure AD hesap için kaydolmak için buraya tıklayın](https://go.microsoft.com/fwlink/?LinkId=309942).

> [!NOTE]
> Azure AD seçeneklerden birini seçerseniz, projenize bir veritabanı gerektirir ve Azure AD kiracınız için genel yönetici hesabı için oturum açmanız gerekmemektedir. Bir kuruluş hesabına adını ve parolasını girin (örneğin, admin@contoso.onmicrosoft.com), Azure AD kiracınız için yönetici izinlerine sahip.
> 
> **Bir Microsoft hesabı için kimlik bilgilerini girin yoktur (örneğin, contoso@hotmail.com) oturum açma iletişim kutusunda.**


<a id="orgauthsingle"></a>
### <a name="cloud---single-organization-authentication"></a>Bulut - tek kurum kimlik doğrulaması

![Tek kurum kimlik doğrulaması](creating-web-projects-in-visual-studio/_static/image24.png)

Bir Azure AD'de tanımlanan kullanıcı hesapları için kimlik doğrulamasını etkinleştirmek istiyorsanız bu seçeneği [Kiracı](https://technet.microsoft.com/library/jj573650.aspx). Örneğin, contoso.com sitesidir ve bunu contoso.onmicrosoft.com kiracıya katıldıktan çalışanlara Contoso şirketinin kullanımına sunulacak. Uygulamaya erişmek için diğer kiracılardan kullanıcıların Azure AD yapılandırmanız mümkün olmayacaktır.

#### <a name="domain"></a>Etki Alanı

Örneğin uygulama, ayarlamak istediğiniz Azure AD etki alanını girin: `contoso.onmicrosoft.com`. Varsa bir [özel etki alanı](http://www.cloudidentity.com/blog/2013/04/14/adding-a-custom-domain-to-your-windows-azure-ad/), gibi `contoso.com` yerine `contoso.onmicrosoft.com`, burada girebilirsiniz.

#### <a name="access-level"></a>Erişim düzeyi

Uygulamanın gerekiyorsa sorgulama veya Graph API'sini kullanarak dizin bilgilerini güncelleştirme, seçin **çoklu oturum açma, dizin verilerini okuma** veya **çoklu oturum açma, okuma ve yazma dizin verilerini**. Aksi takdirde seçin **çoklu oturum açma**. Daha fazla bilgi için [uygulama erişim düzeyleri](https://msdn.microsoft.com/library/windowsazure/b08d91fa-6a64-4deb-92f4-f5857add9ed8#BKMK_AccessLevels) ve [sorguya Azure AD Graph API'sini kullanarak](https://msdn.microsoft.com/library/windowsazure/dn151791.aspx).

#### <a name="application-id-uri"></a>Uygulama Kimliği URI'si

Şablon, varsayılan olarak, bir ' % s'uygulama kimliği URI'si, Azure AD etki alanı için proje adı eklenerek oluşturur. Örneğin, proje adı ise `Example` ve etki alanı `contoso.onmicrosoft.com`, uygulama kimliği URI'si olur `https://contoso.onmicrosoft.com/Example`. Uygulama Kimliği URI'sini el ile belirtmek istiyorsanız, genişletme **diğer seçenekler** bölümünde ve uygulama kimliği URI'si metin kutusuna girin. Uygulama Kimliği URI'si ile başlamalıdır `https://`.

Azure AD'de zaten sağlanmış bir uygulama aynı uygulama kimliği URI'si Visual Studio projesi için kullandığı bir varsa varsayılan olarak, projeye yeni bir sağlama yerine var olan uygulamanın bağlanır. Bu durumda sağlanacak yeni bir uygulama istiyorsanız, temizleyin **zaten aynı Kimliğe sahip bir uygulama girdisi üzerine** onay kutusu.

Varsa **üzerine yaz** onay kutusu işaretli değilse ve Visual Studio bulur aynı uygulama kimliği URI'si ile var olan bir uygulama, bir sayı giderek kullanılacak URI ekleyerek yeni bir URI'ya oluşturur. Örneğin, proje adı varsayalım `Example`metin kutusunu boş bırakın, işaretini **üzerine yaz** onay kutusunu ve Azure AD kiracısını URI ile bir uygulama zaten `https://contoso.onmicrosoft.com/Example`. Bu durumda, yeni bir uygulama kimliği URI'si gibi bir uygulama ile sağlanacak `https://contoso.onmicrosoft.com/Example_20130619330903`.

#### <a name="provisioning-the-application-in-azure-ad"></a>Azure AD'de uygulama sağlama

Azure ad uygulaması sağlayın ya da projeyi varolan bir uygulamaya bağlanmak için Visual Studio etki alanı için bir genel yönetici kimlik bilgileriniz gerekir. Tıkladığınızda **Tamam** içinde **kimlik doğrulamasını yapılandırma** iletişim kutusunda, kullanıcı adını ve belirttiğiniz etki alanı için genel yönetici parolası istenir. Daha sonra ' a tıkladığınızda **proje oluştur** içinde **yeni ASP.NET projesi** iletişim kutusunda, Visual Studio, Azure AD'de bir uygulama sağlar. Bu işlemin bir parçası olarak Visual Studio istemci gizli değerlerini Web.config dosyasındaki bir yıl oluşturulduktan sonra süresi dolacak eklediğini göz önünde bulundurun.

Kullanan uygulamalar oluşturma hakkında bilgi için **bulut - tek kurum** kimlik doğrulaması, aşağıdaki kaynaklara bakın:

- [Azure kimlik doğrulaması](../2012/windows-azure-authentication.md)
- [Azure AD kullanarak Web uygulamanıza oturum açma ekleme](https://msdn.microsoft.com/library/windowsazure/dn151790.aspx)
- [Azure Active Directory ile ASP.NET Uygulamaları geliştirme](../../../identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory.md)
- [Azure AD ile ASP.NET Web API güvenliğini sağlama ve Microsoft OWIN bileşenleri](https://msdn.microsoft.com/magazine/dn463788.aspx)

Öğreticiler, Visual Studio 2013 için henüz güncelleştirilmemiş; Bazı hangi öğreticileri, el ile yapmak için doğrudan otomatik olarak Visual Studio 2013'te.

<a id="orgauthmulti"></a>
### <a name="cloud---multi-organization-authentication"></a>Bulut - birden çok kurum kimlik doğrulaması

![Birden çok kurum kimlik doğrulaması](creating-web-projects-in-visual-studio/_static/image25.png)

Birden çok Azure AD'de tanımlanan kullanıcı hesapları için kimlik doğrulamasını etkinleştirmek istiyorsanız bu seçeneği [kiracılar](https://technet.microsoft.com/library/jj573650.aspx). Örneğin, contoso.com sitesidir ve bunu contoso.onmicrosoft.com kiracıda olan Contoso şirketi çalışanları ve Fabrikam.onmicrosoft.com adresli kiracıya katıldıktan çalışanlarının Fabrikam şirketin kullanımına sunulacak.

Girdiğiniz ayarları ve adım sağlama uygulama benzer [tek kurum kimlik doğrulaması](#orgauthsingle).

Kullanan uygulamalar oluşturma hakkında bilgi için **bulut - birden çok kuruluş** kimlik doğrulaması, aşağıdaki kaynaklara bakın:

- [Azure Active Directory, ASP.NET kolay Web uygulaması tümleştirme &amp; Visual Studio](https://blogs.msdn.com/b/active_directory_team_blog/archive/2013/06/26/improved-windows-azure-active-directory-integration-with-asp-net-amp-visual-studio.aspx) Active Directory ekibi blogu.
- [Azure AD ile çok Kiracılı Web uygulamaları geliştirme](https://msdn.microsoft.com/library/windowsazure/dn151789.aspx) öğretici. Bu öğreticide Visual Studio 2013 için henüz güncelleştirilmedi; Bazı öğretici sizi el ile yapmak için ne yönlendiren otomatik olarak Visual Studio 2013'te.
- [Oturum açmadan önce Yukarı kendi birden çok kuruluşların ASP.NET uygulaması ile oturum açmak kullandığınız](http://www.cloudidentity.com/blog/2013/10/26/you-have-to-sign-up-with-your-own-multiple-organizations-asp-net-app-before-you-can-sign-in/). Açıklayan nasıl çözümleneceğini ortak bir sorunu kişiler Vittorio Bertocci'nin tarafından blog çok kurum kimlik doğrulaması kullanan bir proje oluşturma sırasında karşılaşırsınız.

<a id="orgauthonprem"></a>
### <a name="on-premises-organizational-authentication"></a>Şirket içi Kurumsal kimlik doğrulaması

![Şirket içi Kurumsal kimlik doğrulaması](creating-web-projects-in-visual-studio/_static/image26.png)

Windows Server Active Directory (AD) tanımlanan kullanıcı hesapları için kimlik doğrulamasını etkinleştirmek istediğiniz ve Azure AD kullanmak istemiyorsanız bu seçeneği belirleyin. Bir Intranet sitesine veya Internet sitesi oluşturmak için bu seçeneği kullanabilirsiniz. Bir Internet sitesi için Active Directory Federasyon Hizmetleri (ADFS) erişim için AD sağlamak için kullanın. Daha fazla bilgi için [ile şirket içi Kurumsal kimlik doğrulama seçeneği (ADFS) ASP.NET Visual Studio 2013'te kullanmak](http://www.cloudidentity.com/blog/2014/02/12/use-the-on-premises-organizational-authentication-option-adfs-with-asp-net-in-visual-studio-2013/).

Bir Intranet siteye için alternatif olarak, seçebilirsiniz [Windows kimlik doğrulaması](#winauth) yerine bu seçeneği. Windows kimlik doğrulaması seçeneği için bir meta veri belgesinin URL'sini sağlamanız gerekmez. Ancak, Windows kimlik doğrulaması, özelliği Active Directory'de uygulama erişimi denetlemek veya dizin veri vermez.

#### <a name="on-premises-authority"></a>Şirket içi yetkili

Meta veri belgesi için işaret eden bir URL girin. Meta veri belgesi yetkilisi içerir. Uygulamanın, oturum açma web akışını desteklemek üzere bu koordinatları kullanır.

#### <a name="application-id-uri"></a>Uygulama Kimliği URI'si

AD bu uygulamayı tanımlamak ya da Visual Studio oluşturmak izin vermek için boş bırakın kullanabileceği benzersiz bir URI sağlayın.

<a id="nextsteps"></a>
## <a name="next-steps"></a>Sonraki adımlar

Bu belge, Visual Studio 2013'te yeni bir ASP.NET web projesi oluşturmak için bazı temel Yardım sağlamıştır. Web geliştirme için Visual Studio için kullanma hakkında daha fazla bilgi için bkz. [ https://www.asp.net/visual-studio/ ](../../index.md).
