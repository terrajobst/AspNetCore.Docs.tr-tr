---
uid: visual-studio/overview/2013/release-notes
title: ASP.NET ve Web Araçları Visual Studio 2013 sürüm notları | Microsoft Docs
author: microsoft
description: Bu belgede, Visual Studio 2013 için ASP.NET and Web Tools sürümü açıklanmaktadır.
ms.author: riande
ms.date: 10/17/2013
ms.assetid: 08815768-2702-42ae-ae85-0a59934a11d1
msc.legacyurl: /visual-studio/overview/2013/release-notes
msc.type: authoredcontent
ms.openlocfilehash: 43878bc101ef97e8bbb6c150f4125707da7660c9
ms.sourcegitcommit: c43a6f1fe72d7c2db4b5815fd532f2b45d964e07
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2018
ms.locfileid: "50244963"
---
<a name="aspnet-and-web-tools-for-visual-studio-2013-release-notes"></a>ASP.NET ve Web Araçları Visual Studio 2013 sürüm notları
====================
tarafından [Microsoft](https://github.com/microsoft)

> Bu belgede, Visual Studio 2013 için ASP.NET and Web Tools sürümü açıklanmaktadır.


## <a name="contents"></a>İçindekiler

- [Yükleme notları](#TOC1)
- [Belgeler](#TOC2)
- [Yazılım gereksinimleri](#TOC4)

### <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-2013"></a>ASP.NET ve Web Araçları Visual Studio 2013 için yeni özellikler

- [One ASP.NET](#TOC6)
- [Yeni Web projesi deneyimi](#newproj)
- [ASP.NET iskeleti oluşturma](#scaffold)
- [Tarayıcı Bağlantısı](#browser-link)
- [Visual Studio Web Düzenleyicisi geliştirmeleri](#web-editor)
- [Visual Studio'da Azure App Service Web Apps desteği](#waws)
- [Web yayımlama geliştirmeleri](#publish)
- [NuGet 2.7](#nuget)
- [ASP.NET Web formları](#TOC9)
- [ASP.NET MVC 5](#TOC10)
- [ASP.NET Web API 2](#TOC11)
- [ASP.NET SignalR](#TOC13)
- [ASP.NET kimlik](#TOC8)
- [Microsoft OWIN bileşenleri](#TOC7)
- [Entity Framework 6](#ef6)
- [ASP.NET Razor 3](#TOC14)
- [ASP.NET uygulama askıya alma](#TOC15)
- [Bilinen sorunlar ve yeni değişiklikler](#knownissues)

<a id="TOC1"></a>
## <a name="installation-notes"></a>Yükleme notları

ASP.NET ve Web Araçları Visual Studio 2013 için ana yükleyicide paketlenir ve indirilebilir [burada](https://www.asp.net/downloads).

<a id="TOC2"></a>
## <a name="documentation"></a>Belgeler

Öğreticiler ve diğer bilgileri ASP.NET ve Web Araçları Visual Studio 2013 için sunulan [ASP.NET web sitesi](https://www.asp.net/).

<a id="TOC4"></a>
## <a name="software-requirements"></a>Yazılım Gereksinimleri

ASP.NET ve Web Araçları, Visual Studio 2013 gerektirir.

<a id="TOC5"></a>
## <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-2013"></a>ASP.NET ve Web Araçları Visual Studio 2013 için yeni özellikler

Aşağıdaki bölümlerde sürümünde sunulan özellikler açıklanmaktadır.

<a id="TOC6"></a>
## <a name="one-aspnet"></a>Tek ASP.NET

Visual Studio 2013'ün yayımlanmasıyla birlikte, biz, böylece kolayca karıştırın ve eşleştirin ihtiyacınız olanları ASP.NET teknolojilerini kullanarak deneyimini birleştirme bilgilerinizin yönlendirdik. Örneğin, MVC kullanarak bir proje başlatın ve kolayca projeye Web formları sayfaları ekleyin, daha sonra veya bir Web Forms projesinde Web API'leri iskelesini. Bir ASP.NET tüm, ASP.NET'te sevdiğiniz şeyleri bir geliştirici olarak kolaylaştırarak hakkında ' dir. Seçtiğiniz hangi teknolojisi ne olursa olsun, bir ASP.NET arka plandaki güvenilen çerçevesinin derlemekte olduğunuz çalışacağından emin olabilir.

<a id="newproj"></a>
## <a name="new-web-project-experience"></a>Yeni Web projesi deneyimi

Visual Studio 2013'te yeni web projeleri oluşturma deneyimini geliştirdik. İçinde **yeni ASP.NET Web projesi** iletişim, istediğiniz herhangi bir birleşimini teknolojileri (Web formları, MVC, Web API'si) yapılandırma, kimlik doğrulama seçeneklerini yapılandırmak ve birim testi projesi ekleyin proje türü seçebilirsiniz.

![Yeni ASP.NET projesi](release-notes/_static/image1.png)

Yeni iletişim kutusu şablonları birçoğu için varsayılan kimlik doğrulama seçenekleri değiştirmenize olanak tanır. Örneğin, bir ASP.NET Web formları projesi oluşturduğunuzda, aşağıdaki seçeneklerden herhangi birini seçebilirsiniz:

- Kimlik doğrulama yok
- Bireysel kullanıcı hesapları (ASP.NET üyeliği veya sosyal sağlayıcılar oturum açma)
- Kurumsal hesaplar (Active Directory'de bir Internet uygulaması)
- Windows kimlik doğrulaması (Active Directory'de bir intranet uygulaması)

![Kimlik doğrulama seçenekleri](release-notes/_static/image2.png)

Web projeleri oluşturmak için yeni süreci hakkında daha fazla bilgi için bkz. [Visual Studio 2013'te ASP.NET Web projeleri oluşturma](creating-web-projects-in-visual-studio.md). Yeni kimlik doğrulama seçenekleri hakkında daha fazla bilgi için bkz. [ASP.NET Identity](#TOC8) bu belgenin devamındaki.

<a id="scaffold"></a>
## <a name="aspnet-scaffolding"></a>ASP.NET iskeleti oluşturma

ASP.NET iskeleti oluşturma bir ASP.NET Web uygulamaları için kod oluşturma çerçevedir. Bu, ortak kod projenize bir veri modeli ile etkileşim eklemek kolaylaştırır.

Visual Studio'nun önceki sürümlerinde, ASP.NET MVC projeleri için yapı iskelesi sınırlıydı. Visual Studio 2013 ile Web Forms dahil olmak üzere tüm ASP.NET proje için yapı iskelesi artık kullanabilirsiniz. Visual Studio 2013'ün oluşturma sayfaları şu anda Web formları projesi desteklemiyor, ancak yapı iskelesi Web Forms ile projeye MVC bağımlılıkları ekleyerek kullanmaya devam edebilirsiniz. İçin Web formları sayfaları oluşturmak için destek, gelecek güncelleştirmelerden birinde eklenecektir.

Yapı iskelesi kullanırken, gerekli tüm bağımlılıkların projede yüklü olduğundan emin olun. Bir ASP.NET Web formları projesi ile başlayın ve ardından bir Web API denetleyicisi eklemek için yapı iskelesi kullanın, örneğin, başvurular ve gerekli NuGet paketlerini projenize otomatik olarak eklenir.

MVC yapı iskelesi Web Forms projesine eklemek için Ekle bir **yeni iskele kurulmuş öğe** seçip **MVC 5 bağımlılıkları** iletişim penceresinde. MVC yapı iskelesini oluşturmak için iki seçenek vardır; Minimal ve tam. En az seçerseniz, yalnızca NuGet paketleri ve ASP.NET MVC için başvuruları projenize eklenir. Tam seçeneği seçerseniz bir MVC projesi için gerekli içerik dosyalarının yanı sıra en az bağımlılıklar eklenir.

Zaman uyumsuz denetleyicilerinin yapı iskelesini oluşturmak için destek, Entity Framework 6 yeni zaman uyumsuz özelliklerini kullanır.

Daha fazla bilgi ve eğitimler için bkz. [ASP.NET yapı İskelesi genel bakış](aspnet-scaffolding-overview.md).

<a id="browser-link"></a>
## <a name="browser-link--signalr-channel-between-browser-and-visual-studio"></a>Tarayıcı bağlantısı-tarayıcı ve Visual Studio arasında SignalR kanalı

Yeni [tarayıcı bağlantısı](using-browser-link.md) birden çok tarayıcı Visual Studio'ya bağlanın ve araç çubuğundaki bir düğmeye tıklayarak yenileme özelliği sağlar. Birden çok tarayıcı, mobil öykünücüleri dahil olmak üzere, geliştirme siteye bağlanmak ve yenileme için yenileme tüm tarayıcıların tümü aynı anda tıklayın. Tarayıcı bağlantısı, ayrıca geliştiricilerin, tarayıcı bağlantısı uzantıları yazmak için bir API sunar.

![](release-notes/_static/image3.png)

Tarayıcı bağlantısı API'sini yararlanmak geliştiricilerinin etkinleştirerek, Visual Studio ile bağlı olduğu tüm tarayıcılar arasında sınırları aştığında çok Gelişmiş senaryolar oluşturmak mümkün olur. Web Essentials, Visual Studio ve tarayıcının geliştirici araçları, uzak mobil öykünücüleri ve çok daha fazlasını denetleme arasında tümleşik bir deneyim oluşturmak için API avantajlarından yararlanır.

<a id="web-editor"></a>
## <a name="visual-studio-web-editor-enhancements"></a>Visual Studio Web Düzenleyicisi geliştirmeleri

Visual Studio 2013, web uygulamalarında Razor dosyaları ve HTML dosyaları için yeni bir HTML düzenleyicisi içerir. Yeni HTML Düzenleyici üzerinde HTML5 tabanlı tek bir birleştirilmiş şema sağlar. Otomatik küme ayracı tamamlama ve jQuery kullanıcı Arabirimi IntelliSense öznitelik, öznitelik IntelliSense gruplandırma, kimlik ve sınıf adı IntelliSense ve daha iyi performans, biçimlendirme dahil olmak üzere diğer iyileştirmeler AngularJS sahiptir ve akıllı etiketler.

Aşağıdaki ekran görüntüsünde, HTML düzenleyicide IntelliSense önyükleme özniteliği kullanmayı gösterir.

![HTML düzenleyicisinde IntelliSense](release-notes/_static/image4.png)

Visual Studio 2013'de her iki CoffeeScript ve yerleşik düzenleyicileri daha az gelir. LESS Düzenleyicisi harika özelliklerle CSS Düzenleyicisi'nden gelir ve daha az işlemdeki tüm belgelerde değişkenleri ve mixin'ler belirli IntelliSense sahip @import zinciri.

<a id="waws"></a>
## <a name="azure-app-service-web-apps-support-in-visual-studio"></a>Visual Studio'da Azure App Service Web Apps desteği

Visual Studio 2013'te .NET 2.2 için Azure SDK'sı ile kullanabileceğiniz **Sunucu Gezgini** doğrudan uzak web uygulamalarınızla etkileşmek için. Azure hesabınızda oturum açın, yeni web uygulamaları oluşturun, uygulama yapılandırma, gerçek zamanlı günlükleri ve daha fazlasını görüntüleyin. SDK 2.2 hemen sonra gelen yayımlanan, azure'da uzaktan hata ayıklama modunda çalıştırmak mümkün olacaktır. .NET için Azure SDK'ın geçerli sürümüne yüklediğinizde, Azure App Service Web Apps için yeni özelliklerin çoğu Visual Studio 2012'de de çalışır.

Daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Azure App Service'te ASP.NET web uygulaması oluşturma](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/)
- [Visual Studio kullanarak Azure App Service'te bir web uygulaması sorunlarını giderme](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/)

<a id="publish"></a>
## <a name="web-publish-enhancements"></a>Web yayımlama geliştirmeleri

Visual Studio 2013 yeni ve geliştirilmiş Web yayımlama özellikleri içerir. Birkaç tanesi aşağıda verilmiştir:

- Kolayca [Web.config dosyası şifreleme otomatikleştirmek](https://go.microsoft.com/fwlink/?LinkId=325529). (Bu bağlantı ve aşağıdaki iki sonlarında, 10/17 gün kadar kullanılabilir olmayabilir MSDN'de belgelerin üzerine gelin.)
- Kolayca [bir uygulamayı çevrimdışı dağıtımı sırasında alma otomatikleştirmek](https://go.microsoft.com/fwlink/?LinkId=325530).
- Web dağıtımı için yapılandırma [son değiştirilme tarihi yerine dosya sağlama toplamı kullanın](https://go.microsoft.com/fwlink/?LinkId=325531) hangi dosyaların sunucusuna kopyalanması belirlemek için.
- Hızlı bir şekilde tek tek seçilen dosyaları (Web.config dahil), FTP kullanıyorsanız veya dosya sistemi yayımladığınızda yöntemleri yanı sıra Web dağıtımı ile yayımlayın.

ASP.NET web dağıtımı hakkında daha fazla bilgi için bkz. [ASP.NET sitesi](https://go.microsoft.com/fwlink/?LinkId=322027).

<a id="nuget"></a>
## <a name="nuget-27"></a>NuGet 2.7

NuGet 2.7 adresinde ayrıntılı olarak açıklanan yeni özellikleri zengin bir dizi içeren [NuGet 2.7 Sürüm Notları](http://docs.nuget.org/docs/release-notes/nuget-2.7).

Bu sürümü NuGet paketleri indirmek NuGet paket geri yükleme özelliği için açık izin vermeniz gereken de kaldırır. Onay (ve ilişkili onay NuGet Tercihleri iletişim kutusuna) artık NuGet yükleyerek verilir. Şimdi paket geri yükleme, yalnızca varsayılan olarak çalışır.

<a id="TOC9"></a>
## <a name="aspnet-web-forms"></a>ASP.NET Web Forms

### <a name="one-aspnet"></a>Tek ASP.NET

Web Forms proje şablonları yeni bir ASP.NET deneyimiyle sorunsuzca tümleştirin. MVC ve Web API'si, Web Forms projesine destekler ve bir ASP.NET projesi oluşturma Sihirbazı'nı kullanarak kimlik doğrulaması yapılandırabilirsiniz ekleyebilirsiniz. Daha fazla bilgi için [Visual Studio 2013'te ASP.NET Web projeleri oluşturma](creating-web-projects-in-visual-studio.md).

### <a name="aspnet-identity"></a>ASP.NET Kimlik

Web Forms proje şablonları, yeni ASP.NET Identity framework destekler. Ayrıca, şablonları artık bir Web Forms Intranet projesi oluşturulmasını destekler. Daha fazla bilgi için [kimlik doğrulama yöntemleri](creating-web-projects-in-visual-studio.md#auth) içinde **Visual Studio 2013'te ASP.NET Web projeleri oluşturma**.

### <a name="bootstrap"></a>Önyükleme

Web Forms şablonlarında kullanmak [önyükleme](http://twitter.github.io/bootstrap/) bir zarif ve hızlı yanıt veren kolayca özelleştirebileceğiniz görünüm sağlamak için. Daha fazla bilgi için [Bootstrap Visual Studio 2013 web proje şablonlarında](creating-web-projects-in-visual-studio.md#bootstrap).

<a id="TOC10"></a>
## <a name="aspnet-mvc-5"></a>ASP.NET MVC 5

### <a name="one-aspnet"></a>Tek ASP.NET

Web MVC proje şablonları yeni bir ASP.NET deneyimiyle sorunsuzca tümleştirin. MVC projenizi özelleştirmek ve kimlik doğrulaması kullanarak bir ASP.NET projesi oluşturma Sihirbazı'nı yapılandırın. ASP.NET MVC 5 için giriş niteliğindeki bir eğitim şu yolda bulunabilir: [ASP.NET MVC 5 ile çalışmaya başlama](../../../mvc/overview/getting-started/introduction/getting-started.md).

MVC 5 MVC 4 projelerini yükseltme hakkında daha fazla bilgi için bkz: [bir ASP.NET MVC 4 ve Web API projelerini ASP.NET MVC 5 ve Web API 2'ye yükseltme](../../../mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md).

### <a name="aspnet-identity"></a>ASP.NET Kimlik

MVC proje şablonları, kimlik ve kimlik yönetimi için ASP.NET Identity kullanacak şekilde güncelleştirildi. Facebook ve Google kimlik doğrulama ve yeni üyelik API'si gösteren bir öğretici bulabilirsiniz [Facebook ve Google OAuth2 ve Openıd oturum açma ile ASP.NET MVC 5 uygulaması oluşturma](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) ve [kimlik doğrulaması ile ASP.NET MVC uygulaması oluşturma ve SQL veritabanı ve Azure App Service'e dağıtma](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/).

### <a name="bootstrap"></a>Önyükleme

MVC proje şablonu kullanmak için güncelleştirilmiş [önyükleme](http://getbootstrap.com/) bir zarif ve hızlı yanıt veren kolayca özelleştirebileceğiniz görünüm sağlamak için. Daha fazla bilgi için [Bootstrap Visual Studio 2013 web proje şablonlarında](creating-web-projects-in-visual-studio.md#bootstrap).

### <a name="authentication-filters"></a>Kimlik doğrulama filtreleri

Kimlik doğrulama filtreleri olan yeni bir ASP.NET MVC, ASP.NET MVC ardışık düzende yetkilendirme filtreleri önce çalışan ve kimlik doğrulama mantığı eylem başına, belirtmenizi sağlar bir filtrede tür başına denetleyici, veya genel olarak tüm denetleyicileri için. Kimlik doğrulama filtreleri, kimlik bilgileri isteği işlemek ve karşılık gelen sorumlu sağlayın. Kimlik doğrulama filtreleri kimlik doğrulama sınaması, yetkisiz istekleri için yanıt de ekleyebilirsiniz.

### <a name="filter-overrides"></a>Filtresi geçersiz kılmaları

Artık, bir geçersiz kılma filtresi belirterek bir belirtilen eylem yöntemini veya denetleyicileri hangi Filtreleri Uygula geçersiz kılabilirsiniz. Geçersiz kılma filtreler bir belirtilen kapsamın (eylem veya denetleyici) çalıştırılmamalıdır filtre türleri kümesi belirtin. Bu, dünya çapında geçerli, ancak belirli eylemler veya denetleyicileri uygulanmasından bazı genel filtreleri sonra Dışla filtreleri yapılandırmanıza olanak sağlar.

### <a name="attribute-routing"></a>Öznitelik yönlendirme

ASP.NET MVC destekleyen bir katkı Tim McCall, yazarı tarafından sağlanan öznitelik yönlendirme [ http://attributerouting.net ](http://attributerouting.net). Öznitelik yönlendirme ile eylemlerin ve denetleyicilerin açıklama ekleyerek yollarınızı belirtebilirsiniz.

<a id="TOC11"></a>
## <a name="aspnet-web-api-2"></a>ASP.NET Web API 2

### <a name="attribute-routing"></a>Öznitelik yönlendirme

ASP.NET Web API artık destekleyen bir katkı Tim McCall, yazarı tarafından sağlanan öznitelik yönlendirme [ http://attributerouting.net ](http://attributerouting.net). Öznitelik yönlendirme ile eylemlerin ve denetleyicilerin böyle açıklama ekleyerek, Web API yolları belirtebilirsiniz:

[!code-csharp[Main](release-notes/samples/sample1.cs)]

Öznitelik yönlendirme, web API'nize bir URI'leri üzerinde daha fazla denetim sağlar. Örneğin, tek bir API denetleyicisi kullanarak bir kaynak hiyerarşi kolayca tanımlayabilirsiniz:

[!code-csharp[Main](release-notes/samples/sample2.cs)]

De öznitelik yönlendirme, isteğe bağlı parametreler, varsayılan değerleri ve rota kısıtlamaları belirtmek için kullanışlı bir söz dizimi sağlar:

[!code-csharp[Main](release-notes/samples/sample3.cs)]

Öznitelik yönlendirme hakkında daha fazla bilgi için bkz. [Web API 2'de öznitelik yönlendirme](../../../web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2.md).

### <a name="oauth-20"></a>OAuth 2.0

Web API ve tek sayfalı uygulama proje şablonları, OAuth 2.0 kullanarak Yetkilendirme artık desteklenmektedir. OAuth 2.0 istemci korumalı kaynaklara erişimi yetkilendirmek için bir çerçevedir. Bu istemciler tarayıcılar ve mobil cihazlar dahil olmak üzere birçok istemci için çalışır.

OAuth 2.0 desteğiyle Microsoft OWIN bileşenleri tarafından sağlanan için taşıyıcı kimlik doğrulaması ve yetkilendirme sunucusu rolünü uygulama yeni güvenlik ara yazılım üzerinde temel alır. Alternatif olarak, istemciler, Azure Active Directory veya ADFS Windows Server 2012 R2 gibi bir kuruluş yetkilendirme sunucusu kullanarak yetkilendirilebilir.

### <a name="odata-improvements"></a>OData geliştirmeleri

**$Batch ve $value $select, $ desteği genişletin**

$Expand, ASP.NET Web API Odata'da $select için tam destek sunuyor ve $value. $Batch değişiklik kümesini işleme ve toplu işleme istek için de kullanabilirsiniz.

$Select ve $expand seçeneklerini genişletin bir OData uç noktadan döndürülen verinin şeklini değiştirmek sağlar. Daha fazla bilgi için [Introducing $select ve $expand genişletin Web API OData desteği](../../../web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value.md).

**Gelişmiş genişletilebilirlik**

OData biçimlendiricilerini genişletilebilir. Atom girişi meta verileri eklemek, adlandırılmış stream ve medya bağlantı giriş destekler, örnek ek açıklamalarının ekleyin ve bağlantıları nasıl oluşturulacağını özelleştirin.

**Türü olmayan destek**

OData hizmetlerini artık kendi varlık türleri için CLR Türleri tanımlamak gerek kalmadan da oluşturabilirsiniz. Bunun yerine, OData denetleyicilerinizi alabilir veya dönüş örneklerini **IEdmObject**, serileştirme/seri durumdan OData biçimlendiricilerini olan.

**Mevcut bir modeli yeniden kullanma**

Var olan bir varlık veri modeli (EDM) zaten varsa, artık onu doğrudan yeni bir tane oluşturmak zorunda kalmadan yerine tekrar kullanabilirsiniz. Örneğin, Entity Framework kullanıyorsanız EF derlemeleri EDM kullanabilirsiniz.

### <a name="request-batching"></a>Toplu işleme istek

İstek toplu işlem, ağ trafiğini azaltmak ve daha sık iletişim kuran kullanıcı arabiriminde daha az bir düzgün sağlamak için tek bir HTTP POST isteği, birden fazla işlemi birleştirir. ASP.NET Web API isteği toplu işleme için çeşitli stratejileri artık destekler:

- Bir OData hizmeti $batch uç noktasını kullanın.
- Birden çok isteği tek bir MIME çok bölümlü istek paketi.
- Özel bir toplu işlem biçimini kullanın.

Toplu işleme istek etkinleştirmek için bir yol toplu işleyici ile Web API yapılandırmanıza ekleyin:

[!code-csharp[Main](release-notes/samples/sample4.cs)]

De denetleyebilirsiniz olmadığını ister veya sıralı olarak veya herhangi bir sırayla yürütülür.

### <a name="portable-aspnet-web-api-client"></a>Taşınabilir bir ASP.NET Web API istemcisi

ASP.NET Web API istemcisi artık, Windows Store ve Windows Phone 8 uygulamalarında çalışan taşınabilir sınıf kitaplıkları oluşturmak için de kullanabilirsiniz. Ayrıca, istemci ve sunucu arasında paylaşılan taşınabilir biçimlendiricileri oluşturabilirsiniz.

### <a name="improved-testability"></a>Gelişmiş Test Edilebilirlik

Web API 2 yaptığı API denetleyicilerinizi test birimine çok daha kolaydır. Yalnızca istek iletisi ve yapılandırma ile bir API denetleyicisi oluşturmak ve test etmek istediğiniz eylem yöntemini çağırın. Sahte kolaydır **UrlHelper** bağlama oluşturmayı gerçekleştiren eylem yöntemleri için bir sınıf.

### <a name="ihttpactionresult"></a>IHttpActionResult

Şimdi, Web API'si eylem yöntemleri sonucunu yalıtılacak Ihttpactionresult uygulayabilirsiniz. Bir Web API'sini eylem yönteminden döndürülen bir Ihttpactionresult sonuç yanıt iletisi oluşturmak için ASP.NET Web API çalışma zamanı tarafından yürütülür. Bir Ihttpactionresult birim basitleştirmek için bir Web API'sini eylemden döndürülebilir Web API uygulamanız, test etme. Bir dizi Ihttpactionresult uygulamaları belirli durum kodlarıyla döndürmek için sonuçları dahil olmak üzere hazır sağlanan kolaylık sağlamak için içerik veya içerik anlaşması yanıtları biçimlendirilmiş.

### <a name="httprequestcontext"></a>HttpRequestContext

Yeni **HttpRequestContext** isteğe bağlıdır, ancak istekte hemen kullanılamıyorsa, herhangi bir durumu izler. Örneğin, kullanabileceğiniz **HttpRequestContext** istemci sertifikası istekle ilişkili sorumlusu rota verilerini almak için **UrlHelper** ve sanal yol kökünü. Kolayca oluşturabileceğiniz bir **HttpRequestContext** için birim testi amacıyla.

Asıl isteğinin güvenmek yerine isteği ile aktarılan çünkü **Thread.CurrentPrincipal**, Web API işlem hattında olsa asıl isteğinin yaşam süresi kullanıma sunuldu.

### <a name="cors"></a>CORS

Brock Allen alanından başka bir önemli katkı sayesinde ASP.NET artık tam olarak çapraz kaynak isteği Paylaşımı (CORS) destekler.

Tarayıcı Güvenliği, bir web sayfası, başka bir etki alanına AJAX istekleri yapmasını engeller. [CORS](http://www.w3.org/TR/cors/) gevşek bir aynı çıkış noktası ilkesi izin veren bir W3C standardıdır. CORS kullanarak, bir sunucu açıkça bazı çıkış noktaları arası istekleri izin verirken diğerlerini.

Web API 2 CORS denetim öncesi isteği otomatik olarak işlenmesi dahil olmak üzere, artık desteklemektedir. Daha fazla bilgi için [ASP.NET Web API çıkış noktaları arası istekleri etkinleştirme](../../../web-api/overview/security/enabling-cross-origin-requests-in-web-api.md).

### <a name="authentication-filters"></a>Kimlik doğrulama filtreleri

Kimlik doğrulama filtreleri olan yeni bir ASP.NET Web API, ASP.NET Web API ardışık düzeninde yetkilendirme filtreleri önce çalışan ve kimlik doğrulama mantığı eylem başına, belirtmenizi sağlar bir filtrede tür başına denetleyici, veya genel olarak tüm denetleyicileri için. Kimlik doğrulama filtreleri, kimlik bilgileri isteği işlemek ve karşılık gelen sorumlu sağlayın. Kimlik doğrulama filtreleri kimlik doğrulama sınaması, yetkisiz istekleri için yanıt de ekleyebilirsiniz.

### <a name="filter-overrides"></a>Filtre geçersiz kılma

Artık, bir geçersiz kılma filtresi belirterek bir belirtilen eylem yöntemini veya denetleyici, hangi filtreler uygulamak geçersiz kılabilirsiniz. Geçersiz kılma filtreler bir belirtilen kapsamın (eylem veya denetleyici) çalıştırmamalısınız filtre türleri kümesi belirtin. Bu, genel filtreleri ekleyin, ancak bazı özel eylemler veya denetleyicileri hariç sağlar.

### <a name="owin-integration"></a>OWIN tümleştirme

ASP.NET Web API artık tam olarak OWIN destekler ve OWIN uyumlu ana bilgisayarda çalıştırılabilir. Ayrıca yer alan bir **HostAuthenticationFilter** OWIN kimlik doğrulama sistemi ile tümleştirme sağlar.

OWIN tümleştirmesiyle, Web API'si SignalR gibi diğer OWIN ara yazılımı yanı sıra kendi işleminizde Self barındırabilirsiniz. Daha fazla bilgi için [Self-Host ASP.NET Web API kullanımı OWIN](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).

<a id="TOC13"></a>
## <a name="aspnet-signalr-20"></a>ASP.NET SignalR 2.0

Aşağıdaki bölümlerde, SignalR 2.0 özellikleri açıklanmaktadır.

- [OWIN üzerinde oluşturulmuş](#builtonowin)
- [MapHubs ve MapConnection MapSignalR sunulmuştur](#MapSignalR)
- [Etki alanları arası destek](#crossdomain)
- [MonoTouch ve MonoDroid iOS ve Android desteği](#mobile)
- [Taşınabilir .NET istemcisi](#portable)
- [Yeni paket barındırma](#selfhost)
- [Geriye dönük olarak uyumlu sunucu desteği](#backwardcompat)
- [.NET 4.0 için sunucu desteği kaldırıldı](#remove40)
- [İstemciler ve grupların listesini ileti gönderme](#messagelist)
- [Belirli bir kullanıcıya bir ileti gönderme](#sendtouser)
- [Daha iyi hata işleme desteği](#errorhandling)
- [Daha kolay birim test hub'ları](#unittesting)
- [JavaScript hata işleme](#javascripterror)

Mevcut bir 1.x proje SignalR 2.0 sürümüne yükseltmek nasıl bir örnek için bkz [yükseltme bir SignalR 1.x proje](../../../signalr/overview/releases/upgrading-signalr-1x-projects-to-20.md).

<a id="builtonowin"></a>
### <a name="built-on-owin"></a>OWIN üzerinde oluşturulmuş

SignalR 2.0 tamamen açık yerleşik [OWIN (.NET için açık Web arabirimi)](http://owin.org/). Bu değişiklik, Kurulum işlemi için SignalR web barındırılan ve şirket içinde barındırılan SignalR uygulamalar arasında çok daha tutarlı hale getirir, ancak bir dizi API değişiklik de gerekli.

<a id="MapSignalR"></a>

### <a name="maphubs-and-mapconnection-are-now-mapsignalr"></a>MapHubs ve MapConnection MapSignalR sunulmuştur

OWIN standartlarıyla uyumluluğu sağlamak için bu yöntemleri için yeniden adlandırıldığı `MapSignalR`. `MapSignalR` Tüm hub'ları eşleme parametreleri olmadan çağırılır (olarak `MapHubs` sürümünde vermiyor 1.x); bireysel eşlemek için **PersistentConnection** nesneleri, tür parametresi ve bağlantı için URL uzantısına bağlantı türü belirtin ilk bağımsız değişken.

`MapSignalR` Yöntemi Owın başlangıç sınıfı çağrılır. Visual Studio 2013'ün Owın başlangıç sınıfı için yeni bir şablonu içerir. Bu şablonu kullanmak için aşağıdakileri yapın:

1. Projeye sağ tıklayın
2. Seçin **ekleme**, **yeni öğe...**
3. Seçin **Owın başlangıç sınıfı**. Yeni bir sınıf adı **Startup.cs**.

İçinde bir **Web uygulaması,** Owın başlangıç sınıfı içeren `MapSignalR` aşağıda gösterildiği gibi bir giriş Web.Config dosyasındaki uygulama ayarları düğümünde kullanarak Owın'ın başlatma işlemi yöntemi daha sonra eklenen.

İçinde bir **uygulama barındırabileceğiniz**, başlangıç sınıfı türü parametresi olarak geçirilir `WebApp.Start` yöntemi.

**Hub'ları ve SignalR bağlantıları eşleme 1.x (bir web uygulaması genel uygulama dosyasından):** 

[!code-csharp[Main](release-notes/samples/sample5.cs)]

**Eşleme hub'ları ve SignalR 2. 0 ' (dosyadan bir Owın başlangıç sınıfı) bağlantılar:** 

[!code-csharp[Main](release-notes/samples/sample6.cs)]

İçinde bir **uygulama barındırabileceğiniz**, başlangıç sınıfı türü parametresi olarak geçirilir `WebApp.Start` aşağıda gösterildiği gibi yöntemi.

[!code-csharp[Main](release-notes/samples/sample7.cs)]

<a id="crossdomain"></a>

### <a name="cross-domain-support"></a>Etki alanları arası destek

SignalR 1.x, etki alanı istekleri çapraz tek bir EnableCrossDomain tarafından kontrol bayrağını. Bu bayrak hem JSONP hem de CORS istekleri denetlenir. Daha fazla esneklik için tüm CORS desteği SignalR sunucu bileşeninden kaldırıldı (JavaScript lients kullanmaya devam CORS normalde tarayıcı onu desteklediğini algılanırsa), ve yeni OWIN ara yazılımı yapılan bu senaryoları desteklemek kullanılabilir.

SignalR 2. 0 ', (eski tarayıcılarda etki alanları arası istekleri desteklemek için), istemcide, JSONP gereklidir, ayarlayarak açıkça etkinleştirilmesi gerekecektir `EnableJSONP` üzerinde `HubConfiguration` nesnesini `true`, aşağıda gösterildiği gibi. CORS az güvenli olsa da JSONP varsayılan olarak devre dışıdır.

SignalR 2.0 sürümünde yeni bir CORS ara yazılım eklemek için Ekle `Microsoft.Owin.Cors` kitaplık projesi ve çağrı `UseCors` bölümünde gösterildiği gibi SignalR ara yazılım önce.

**Microsoft.Owin.Cors projenize ekleme**: Bu kitaplığını yüklemek için Paket Yöneticisi konsolunda aşağıdaki komutu çalıştırın:

[!code-powershell[Main](release-notes/samples/sample8.ps1)]

Bu komut 2.0.0 ekleyeceksiniz projenize paketin sürümü.

**UseCors çağırma**

Aşağıdaki kod parçacıkları etki alanları arası bağlantıları SignalR öğesinde kullanılmasını göstermek 1.x ve 2.0.

**Etki alanları arası istek uygulama içinde SignalR 1.x (genel uygulama dosyadan)**

[!code-csharp[Main](release-notes/samples/sample9.cs)]

**Uygulama etki alanları arası isteklerde SignalR 2. 0 (C# kod dosyası)**

Aşağıdaki kod bir SignalR 2.0 projesinde CORS veya JSONP etkinleştirme gösterir. Bu kod örneği kullanan `Map` ve `RunSignalR` yerine `MapSignalR`, böylece CORS desteği gerektiren SignalR istekleri için CORS ara yazılımı çalıştırır (yerine tüm trafiği yolda belirtilen `MapSignalR`.) `Map` uygulamanın tamamı yerine belirli bir URL ön eki için çalıştırması gereken diğer tüm ara yazılımı için de kullanılabilir.

[!code-csharp[Main](release-notes/samples/sample10.cs)]

<a id="mobile"></a>

### <a name="ios-and-android-support-via-monotouch-and-monodroid"></a>MonoTouch ve MonoDroid iOS ve Android desteği

İOS ve Android istemciler MonoTouch ve MonoDroid bileşenlerini kullanma için destek eklenmiştir [Xamarin Kitaplığı](https://xamarin.com/). Bunların nasıl kullanılacağı hakkında daha fazla bilgi için bkz. [kullanarak Xamarin bileşenleri](https://github.com/SignalR/SignalR/wiki/Building-Mono.Mobile.sln). Bu bileşenler kullanıma sunulacak [Xamarin Store](https://store.xamarin.com/) SignalR RTW sürüm ne zaman kullanılabilir.

<a id="portable"></a> ### Taşınabilir .NET istemcisi

Daha iyi platformlar arası geliştirme, Silverlight, WinRT kolaylaştırmak ve Windows Phone istemcileri aşağıdaki platformları destekler, tek bir taşınabilir .NET istemci ile değiştirilmiştir:

- NET 4.5
- Silverlight 5
- WinRT (Windows Store uygulamaları için .NET)
- Windows Phone 8

<a id="selfhost"></a>

### <a name="new-self-host-package"></a>Yeni paket barındırma

Şirket içinde SignalR barındırma (bir işlemde barındırılan SignalR uygulamalarını diğer uygulama yerine veya bir web sunucusunda barındırılan) kullanmaya başlama daha kolay hale getirmek için bir NuGet paketi artık yok. SignalR ile oluşturulan ve barındırılan bir projeyi yükseltmesine 1.x, Microsoft.AspNet.SignalR.Owin paketini kaldırın ve Microsoft.AspNet.SignalR.SelfHost paketi ekleyin. Barındırılan paket ile çalışmaya başlama hakkında daha fazla bilgi için bkz: [öğretici: SignalR barındırma](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).

<a id="backwardcompat"></a>

### <a name="backward-compatible-server-support"></a>Geriye dönük olarak uyumlu sunucu desteği

Önceki sürümlerinde SignalR, SignalR paket istemcide kullanılan ve aynı olması için gerekli sunucu sürümleri. Güncelleştirilecek zor olurdu kalın istemci uygulamaları desteklemek için yeni bir sunucu sürümü eski bir istemciyi kullanarak SignalR 2.0 artık desteklemektedir. **Not: SignalR 2.0 ile yeni istemcilerden daha eski sürümleriyle oluşturulmuş sunucularını desteklemez.**

<a id="remove40"></a>

### <a name="removed-server-support-for-net-40"></a>.NET 4.0 için sunucu desteği kaldırıldı

SignalR 2.0, .NET 4.0 ile sunucu birlikte çalışabilirlik için destek bıraktı. .NET 4.5, SignalR 2.0 sunucularıyla kullanılmalıdır. SignalR 2.0 için .NET 4.0 istemci hala yoktur.

<a id="messagelist"></a>

### <a name="sending-a-message-to-a-list-of-clients-and-groups"></a>İstemciler ve grupların listesini ileti gönderme

SignalR 2. 0'da, istemci ve Grup kimliklerinin bir listesini kullanarak ileti göndermek mümkündür. Aşağıdaki kod parçacıkları, bunun nasıl yapılacağı gösterilmektedir.

**İstemcileri ve PersistentConnection kullanarak grupları listesine ileti gönderme**

[!code-csharp[Main](release-notes/samples/sample11.cs)]

**İstemcileri ve hub'ı kullanarak grupları listesine ileti gönderme**

[!code-csharp[Main](release-notes/samples/sample12.cs)]

<a id="sendtouser"></a>

### <a name="sending-a-message-to-a-specific-user"></a>Belirli bir kullanıcıya bir ileti gönderme

Bu özellik, USERID belirtmek kullanıcılara bir Irequest IUserIdProvider yeni arabirimi aracılığıyla göre:

**IUserIdProvider arabirimi**

[!code-csharp[Main](release-notes/samples/sample13.cs)]

Varsayılan olarak kullanıcının IPrincipal.Identity.Name kullanıcı adı olarak kullanan bir uygulama olacaktır.

Hub'ları, bu kullanıcılara yeni bir API aracılığıyla iletileri göndermek mümkün olacaktır:

**' % S ' Clients.User API kullanma**

[!code-csharp[Main](release-notes/samples/sample14.cs)]

<a id="errorhandling"></a>

### <a name="better-error-handling-support"></a>Daha iyi hata işleme desteği

Kullanıcılar artık throw **HubException** herhangi bir hub çağrısının öğesinden. Oluşturucusuna **HubException** bir dize iletisi ve bir nesne gerçekleştirebileceğiniz ek hata verileri. SignalR, özel durum otomatik olarak serileştirmek ve burada, reddetme/hub yöntemi çağrısının başarısız kullanılacak istemciye göndermek.

**Ayrıntılı hubs özel durumları Göster** ayarı sahip hiçbir seçtiğiniz **HubException** veya değil; istemciye gönderilen her zaman gönderildiğini.

**Sunucu tarafı kodu gösteren bir HubException istemciye gönderme**

[!code-csharp[Main](release-notes/samples/sample15.cs)]

**Sunucudan gönderilen bir HubException yanıtlama gösteren JavaScript istemci kodu**

[!code-html[Main](release-notes/samples/sample16.html)]

**.NET istemci kodu gösteren sunucudan gönderilen bir HubException yanıtlama**

[!code-csharp[Main](release-notes/samples/sample17.cs)]

<a id="unittesting"></a>

### <a name="easier-unit-testing-of-hubs"></a>Daha kolay birim test hub'ları

SignalR 2.0 içerir adlı bir arabirim `IHubCallerConnectionContext` sahte istemci tarafı çağrılarını oluşturmayı kolaylaştırması hub'ları üzerinde. Popüler test harnesses ile bu arabirimi kullanılarak aşağıdaki kod parçacıkları göstermek [xUnit.net](https://github.com/xunit/xunit) ve [moq](https://code.google.com/p/moq/).

**Birim SignalR ile xUnit.net testi**

[!code-csharp[Main](release-notes/samples/sample18.cs)]

**Birim SignalR ile moq testi**

[!code-csharp[Main](release-notes/samples/sample19.cs)]

<a id="javascripterror"></a>

### <a name="javascript-error-handling"></a>JavaScript hata işleme

SignalR 2. 0'da, tüm JavaScript hata işleme geri çağırmaları ham dizeler yerine JavaScript hata nesneleri döndürür. Bu SignalR hata İşleyicileriniz için daha zengin bilgi akışına olanak sağlar. İç özel durumu alabilirsiniz `source` hata özelliğidir.

**JavaScript istemci Start.Fail özel durumu işleyen bir kodu**

[!code-javascript[Main](release-notes/samples/sample20.js)]

<a id="TOC8"></a>
## <a name="aspnet-identity"></a>ASP.NET Kimlik

### <a name="new-aspnet-membership-system"></a>Yeni ASP.NET üyelik sistemi

ASP.NET, ASP.NET uygulamaları için yeni üyelik sistemini kimliğidir. ASP.NET Identity kullanıcı özel profil verileri uygulama verileriyle tümleştirmeyi kolaylaştırır. ASP.NET Identity kullanıcı profilleri için Kalıcılık modeli uygulamanızda seçmenize olanak sağlar. SQL Server veritabanı veya Azure depolama tabloları gibi NoSQL veri deposu dahil olmak üzere başka bir veri deposuna verileri depolayabilirsiniz. Daha fazla bilgi için [bireysel kullanıcı hesapları](creating-web-projects-in-visual-studio.md#indauth) içinde **Visual Studio 2013'te ASP.NET Web projeleri oluşturma**.

### <a name="claims-based-authentication"></a>Beyana dayalı kimlik doğrulama

ASP.NET, şimdi burada kullanıcının kimliğini güvenilen veren gelen talepler kümesi olarak temsil edilen talep tabanlı kimlik doğrulaması, destekler. Kullanıcılar, kimliği doğrulanmış bir kullanıcı adı ve parolayı bir uygulamanın veritabanında tutulan veya sosyal kimlik sağlayıcılarını kullanarak olabilir (örneğin: Microsoft Accounts, Facebook, Google, Twitter), veya Azure Active Directory aracılığıyla Kurumsal hesaplarını kullanan veya Active Directory Federasyon Hizmetleri (ADFS).

### <a name="integration-with-azure-active-directory-and-windows-server-active-directory"></a>Azure Active Directory ve Windows Server Active Directory ile tümleştirme

Artık Azure Active Directory veya Windows Server Active Directory (AD) için kimlik doğrulaması kullanan ASP.NET projeleri oluşturabilirsiniz. Daha fazla bilgi için [Kurumsal hesaplar](creating-web-projects-in-visual-studio.md#orgauth) içinde **Visual Studio 2013'te ASP.NET Web projeleri oluşturma**.

### <a name="owin-integration"></a>OWIN tümleştirme

ASP.NET kimlik doğrulaması, artık tüm OWIN tabanlı ana bilgisayarda kullanılan OWIN ara yazılımı dayanır. OWIN hakkında daha fazla bilgi için aşağıdakilere bakın [Microsoft OWIN bileşenleri](#TOC7) bölümü.

<a id="TOC7"></a>
## <a name="microsoft-owin-components"></a>Microsoft OWIN bileşenleri

[.NET için açık Web arabirimi](http://owin.org/) (OWIN) .NET web sunucuları ve web uygulaması arasında bir Özet tanımlar. OWIN sunucusu web uygulamasından web uygulamaları konak belirsiz hale ayırır. Örneğin, bir OWIN tabanlı web uygulamasının IIS'deki konak veya özel bir işlemde kendiliğinden konak.

Microsoft OWIN bileşenleri (Katana proje olarak da bilinir) sunulan değişiklikler, yeni sunucu ve ana bileşenleri, yeni Yardımcısı kitaplıkları ve ara yazılım ve yeni kimlik doğrulaması ara yazılımı içerir.

OWIN ve Katana hakkında daha fazla bilgi için bkz: [OWIN ve Katana yenilikler](../../../aspnet/overview/owin-and-katana/index.md).

**Not: [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) uygulamalar, IIS Klasik modda çalışamaz; tümleşik modunda çalıştırılmalıdır.**

**Not: [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) uygulamalarını tam güvende çalıştırılması gerekir.**

### <a name="new-servers-and-hosts"></a>Yeni sunucular ve konakları

Bu sürümle birlikte, yeni bileşenler barındırma senaryolarını etkinleştirmek için eklendi. Bu bileşenleri aşağıdaki NuGet paketlerini içerir:

- **Microsoft.Owin.Host.HttpListener**. Kullanan bir OWIN sunucusu sağlar **HttpListener** HTTP isteklerini dinlemek ve bunları OWIN ardışık düzenine yönlendirir.
- **Microsoft.Owin.Hosting** bir OWIN ardışık düzenine bir konsol uygulaması veya Windows hizmeti gibi özel bir işlemde barındırılan isteyen geliştiriciler için bir kitaplık sağlar.
- **OwinHost**. Sarmalayan bir tek başına yürütülebilir dosya sağlar `Microsoft.Owin.Hosting` ve bir OWIN ardışık düzenini özel bir Windows uygulaması yazmak zorunda kalmadan Self barındırmanıza olanak tanır.

Ayrıca, `Microsoft.Owin.Host.SystemWeb` paket artık derleyiciye sağlamak bir ara yazılım sağlar **SystemWeb** belirli ASP.NET ardışık düzen aşamaları sırasında ara yazılımın çağrılması gerektiğini belirten bir sunucu. Bu özellik, erken ASP.NET işlem hattı çalışması gereken kimlik doğrulaması ara yazılımı için özellikle yararlıdır.

### <a name="helper-libraries-and-middleware"></a>Yardımcısı kitaplıkları ve ara yazılım

OWIN belirtiminden yalnızca işlevi ve türü tanımlarını kullanarak OWIN bileşenlerini yazabilirsiniz, ancak yeni `Microsoft.Owin` paket soyutlama daha kullanıcı dostu kümesi sağlar. Bu paketi çeşitli önceki paketler birleştirir (örneğin, `Owin.Extensions`, `Owin.Types`) sonra kolayca diğer OWIN bileşenleri tarafından kullanılabilecek bir tek ve iyi yapılandırılmış nesne modelini. Aslında, Microsoft OWIN bileşenleri çoğunu artık kullanın Bu paket.

> [!NOTE]
> [OWIN](http://www.owin.org) uygulamalar, IIS Klasik modda çalışamaz; tümleşik modunda çalıştırılmalıdır.

> [!NOTE]
> [OWIN](http://www.owin.org) uygulamalarını tam güvende çalıştırılması gerekir.

Bu sürüm ayrıca hataları araştırmanıza yardımcı olması için hata sayfası ara yazılımını yanı sıra çalışan bir OWIN uygulama doğrulamak için bir ara yazılım içeren Microsoft.Owin.Diagnostics paketini içerir.

### <a name="authentication-components"></a>Kimlik doğrulama bileşenleri

Aşağıdaki kimlik doğrulama bileşenleri kullanılabilir.

- **Microsoft.Owin.Security.ActiveDirectory**. Şirket içi veya bulut tabanlı dizin hizmetlerini kullanarak kimlik doğrulamasını etkinleştirir.
- **Microsoft.Owin.Security.Cookies** tanımlama bilgilerini kullanarak kimlik doğrulamayı etkinleştirir. Bu paket daha önce adlı `Microsoft.Owin.Security.Forms`.
- **Microsoft.Owin.Security.Facebook** Facebook'ın OAuth tabanlı hizmetini kullanarak kimlik doğrulamayı etkinleştirir.
- **Microsoft.Owin.Security.Google** Google Openıd tabanlı hizmetini kullanarak kimlik doğrulamayı etkinleştirir.
- **Microsoft.Owin.Security.Jwt** JWT belirteçleri kullanarak kimlik doğrulamayı etkinleştirir.
- **Microsoft.Owin.Security.MicrosoftAccount** Microsoft hesaplarını kullanarak kimlik doğrulamayı etkinleştirir.
- **Microsoft.Owin.Security.OAuth**. Taşıyıcı belirteç kimlik doğrulaması için bir OAuth yetkilendirme sunucusu yanı sıra ara yazılım sağlar.
- **Microsoft.Owin.Security.Twitter** Twitter'ın OAuth tabanlı hizmetini kullanarak kimlik doğrulamayı etkinleştirir.

Bu sürüm ayrıca içerir `Microsoft.Owin.Cors` çıkış noktaları arası HTTP isteklerini işlemek için bir ara yazılım içeren paket.

> [!NOTE]
> Visual Studio 2013'ün son sürümde JWT imzalamak için destek kaldırılmıştır.

<a id="ef6"></a>
## <a name="entity-framework-6"></a>Entity Framework 6

Yeni özellikler ve Entity Framework 6 diğer değişiklikler listesi için bkz. [Entity Framework sürüm geçmişi](https://msdn.com/data/jj574253).

<a id="TOC14"></a>
## <a name="aspnet-razor-3"></a>ASP.NET Razor 3

ASP.NET Razor 3, aşağıdaki yeni özellikler içerir:

- İçin sekmesinde düzenleme desteği. Daha önce **belgeyi Biçimlendir** komutu, otomatik girintileme ve biçimlendirme Visual Studio'da otomatik çalışmadığı doğru kullanırken **sekmeleri tut** seçeneği. Bu değişiklik, Visual Studio için sekmesinde biçimlendirme Razor kodu biçimlendirme düzeltir.
- Bağlantılar oluşturulurken URL yeniden yazma kuralları için destek.
- Saydam güvenlik özniteliğine kaldırılması.
  > [!NOTE]
  > Bu bölünmesi farklıdır ve Razor 2 MVC5 veya MVC5 karşı derlenen derlemeler ile uyumlu olduğu sürece Razor 3 MVC4 ve önceki sürümleri uyumsuz hale getiriyor.

Yayın öncesi sürümlerinden Visual Studio 2013'te düzeltilen razor 3 sorunlara bulunabilir [burada](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Resolved%7cClosed&type=All&priority=All&release=All%7cv5.0%2bPreview%7cv5.0%2bRC%7cv5.0%2bRTM&assignedTo=All&component=Web%2bPages%252fRazor&reasonClosed=Fixed&sortField=LastUpdatedDate&sortDirection=Descending&page=0).

<a id="TOC15"></a>
## <a name="aspnet-app-suspend"></a>ASP.NET uygulama askıya alma

ASP.NET uygulama askıya alma çok sayıda ASP.NET siteleri tek bir makinede barındırma için ekonomik bir model ve kullanıcı deneyimini önemli ölçüde değişir .NET Framework 4.5.1 oyunun kurallarını değiştiren bir özelliğidir. Daha fazla bilgi için [ASP.NET uygulama askıya alma – hızlı yanıt veren paylaşılan .NET web barındırma](https://blogs.msdn.com/b/dotnet/archive/2013/10/09/asp-net-app-suspend-responsive-shared-net-web-hosting.aspx).

<a id="knownissues"></a>
## <a name="known-issues-and-breaking-changes"></a>Bilinen sorunlar ve yeni değişiklikler

Bu bölümde, Visual Studio 2013 için bilinen sorunlar ve ASP.NET ve Web Araçları'ndaki önemli değişiklikler açıklanmaktadır.

### <a name="nuget"></a>NuGet

- [Yeni paket geri yükleme, Mono üzerinde çalışmaz, SLN dosyasını kullanırken](https://nuget.codeplex.com/workitem/3596) – bir gelecek nuget.exe indirmesinde düzeltilecektir ve [NuGet.CommandLine paket](http://www.nuget.org/packages/NuGet.CommandLine/) güncelleştirin.
- [Yeni paket geri yükleme, Wix projeleriyle çalışmıyor](https://nuget.codeplex.com/workitem/3598) – bir gelecek nuget.exe indirmesinde düzeltilecektir ve [NuGet.CommandLine paket](http://www.nuget.org/packages/NuGet.CommandLine/) güncelleştirin.
- [Otomatik paket geri yükleme projelerin bir çözüm klasörü altında çalışmaz](https://nuget.codeplex.com/workitem/3625) – NuGet 2.8 düzeltilecektir.

### <a name="aspnet-web-api"></a>ASP.NET Web API

1. `ODataQueryOptions<T>.ApplyTo(IQueryable)` döndürmüyor `IQueryable<T>` için destek ekledik olarak her zaman `$select` ve `$expand`.

    Önceki örneklerimizi `ODataQueryOptions<T>` her zaman Integer dönüş değeri `ApplyTo` için `IQueryable<T>`. Bu sorgu seçeneği için daha önce çalışan biz daha önce desteklenen (`$filter`, `$orderby`, `$skip`, `$top`) sorgu şeklini değiştirmeyin. Destekliyoruz göre `$select` ve `$expand` dönüş değeri `ApplyTo` değişmeyecek `IQueryable<T>` her zaman.

    [!code-csharp[Main](release-notes/samples/sample21.cs)]

    Daha önce'nden örnek kodu kullanıyorsanız, istemciyi değil gönderirseniz, çalışmaya devam eder `$select` ve `$expand`. Ancak, desteklemek istediğiniz `$select` ve `$expand` için bu kodu değiştirmek zorunda.

    [!code-csharp[Main](release-notes/samples/sample22.cs)]
2. **Toplu istek sırasında Request.Url veya RequestContext.Url null**

    Bir toplu işlem senaryosunda **UrlHelper** den erişildiğinde null **Request.Url** veya **RequestContext.Url**.

    Bu sorun şu anda burada izlenir: [BatchRequestContext.Url isteği toplu işleme için null](http://aspnetwebstack.codeplex.com/workitem/1301).

    Yeni bir örneğini oluşturmak için bu sorunun geçici çözümü olan **UrlHelper**, aşağıdaki örnekte olduğu gibi:

    **UrlHelper yeni bir örneğini oluşturma**

    [!code-csharp[Main](release-notes/samples/sample23.cs)]

### <a name="aspnet-mvc"></a>ASP.NET MVC

1. Görünüm veri göndermek zaman AntiForgerToken doğrulama yapmak görünümler varsa MVC5 ve OrgAuth, kullanırken, şu hatayı arasında gelebilir:

    **Hata**:

    *'/' Uygulamasında sunucu hatası.*

    <em>Bir talep türü '<http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier>'veya'<http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider>' üzerinde sağlanan Claimsıdentity yoktu. Talep tabanlı kimlik doğrulaması ile sahteciliğe karşı koruma belirteci desteğini etkinleştirmek için lütfen yapılandırılan talep sağlayıcısı hem de bu talepler ürettiği Claimsıdentity örneklerinde sağladığını doğrulayın. Yapılandırılmış Talep sağlayıcı benzersiz bir tanımlayıcı olarak bunun yerine farklı talep türü kullanıyorsa, statik AntiForgeryConfig.UniqueClaimTypeIdentifier özelliği ayarlanarak yapılandırılabilir.</em>

    **Geçici çözüm**:

    Sorunu gidermek için Global.asax dosyasında aşağıdaki satırı ekleyin:

    `AntiForgeryConfig.UniqueClaimTypeIdentifier = ClaimTypes.Name;`

    Bu, sonraki sürümde düzeltilecektir.
2. MVC4 uygulaması için MVC5 yükseltme yaptıktan sonra çözümü oluşturun ve başlatın. Şu hatayı göreceksiniz:

    [A] [İçin B]System.Web.WebPages.Razor.Configuration.HostSection. System.Web.WebPages.Razor.Configuration.HostSection atanamaz Tür A kaynaklanan ' System.Web.WebPages.Razor, sürüm 2.0.0.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35 ' konumunda ' Default' bağlamında ' C:\windows\Microsoft.Net\assembly\GAC\_MSIL\System.Web.WebPages.Razor\ v4.0\_2.0.0.0\_\_31bf3856ad364e35\System.Web.WebPages.Razor.dll'. Tür B kaynaklanan ' System.Web.WebPages.Razor, sürüm 3.0.0.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35 ' konumunda ' Default' bağlamında ' C:\Windows\Microsoft.NET\Framework\v4.0.30319\Temporary ASP.NET Files\root\6d05bbd0\ e8b5908e\assembly\dl3\c9cbca63\f8910382\_6273ce01\System.Web.WebPages.Razor.dll'.

    Yukarıdaki hatayı düzeltmek için açık *tüm* (görünümleri klasöründe Devralınanlar da dahil) Web.config dosyalarından proje ve aşağıdakileri yapın:

   1. Tüm oluşumlarını sürümü "4.0.0.0" "System.Web.Mvc" için "5.0.0.0" güncelleştirin.
   2. "System.Web.Helpers", "2.0.0.0" sürümünün tüm oluşumları güncelleştirme &quot;System.Web.WebPages&quot; ve &quot;System.Web.WebPages.Razor&quot; "3.0.0.0" için

      Örneğin, yukarıdaki değişiklikler yaptıktan sonra derleme bağlamaları şu şekilde görünmelidir:

      [!code-xml[Main](release-notes/samples/sample24.xml)]

      MVC 5 MVC 4 projelerini yükseltme hakkında daha fazla bilgi için bkz: [bir ASP.NET MVC 4 ve Web API projelerini ASP.NET MVC 5 ve Web API 2'ye yükseltme](../../../mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md).
3. İstemci tarafı doğrulama jQuery örtük doğrulaması ile kullanırken, doğrulama iletisi bazen türüne sahip bir HTML input öğesi için yanlış = 'number'. Doğrulama hatası için gerekli bir değer ("Yaş alan gerekiyor") dataValues geçerli bir sayı gerekli olduğunu doğru iletisi yerine girildiğinde gösterilir.

    Bu sorun için bir model oluşturma ve düzenleme görünümlerde bir tamsayı özelliği ile iskele kurulmuş kodu ile yaygın olarak bulunur.

    Bu sorunu çözmek için gelen Düzenleyicisi Yardımcısı değiştirin:

    `@Html.EditorFor(person => person.Age)`

    Hedef:

    `@Html.TextBoxFor(person => person.Age)`
4. ASP.NET MVC 5 kısmi güven artık destekler. MVC veya Webapı ikili dosyalar için bağlama projeleri kaldırmalısınız [SecurityTransparent](https://msdn.microsoft.com/library/system.security.securitytransparentattribute.aspx) özniteliği ve [AllowPartiallyTrustedCallers](https://msdn.microsoft.com/library/system.security.allowpartiallytrustedcallersattribute.aspx) özniteliği. Bu öznitelikleri kaldırma, aşağıdaki gibi derleyici hatalarını ortadan kaldırır.

    `Attempt by security transparent method ‘MyComponent' to access security critical type 'System.Web.Mvc.MvcHtmlString' failed. Assembly 'PagedList.Mvc, Version=4.3.0.0, Culture=neutral, PublicKeyToken=abbb863e9397c5e1' is marked with the AllowPartiallyTrustedCallersAttribute, and uses the level 2 security transparency model. Level 2 transparency causes all methods in AllowPartiallyTrustedCallers assemblies to become security transparent by default, which may be the cause of this exception.`

    > Not, bir yan etkisi Bu, aynı uygulamada 4.0 ve 5.0 derlemeleri kullanamazsınız. Tümünü 5.0 güncelleştirmeniz gerekir.

### <a name="spa-template-with-facebook-authorization-may-cause-instability-in-ie-while-the-web-site-is-hosted-in-intranet-zone"></a>Web sitesi intranet bölgesinde barındırılıyor olsa Facebook yetkilendirme SPA şablonuyla IE'de kararsız hale gelmesine neden

SPA şablon Facebook dış oturum sağlar. Oturum açma, şablonla oluşturulan projeyi yerel olarak çalışırken, IE çökmesine neden olabilir.

Çözüm:

1. İnternet bölgesi web sitesinde barındırmak; veya

2. Senaryo, IE dışındaki bir tarayıcıda test edin.

### <a name="web-forms-scaffolding"></a>Yapı İskelesi Web formları

Web Forms yapı İskelesi VS2013 ' kaldırıldı ve Visual Studio için gelecekte yapılacak bir güncelleştirmenin kullanıma sunulacak. Ancak bir Web Forms projesindeki yapı iskelesi MVC bağımlılıkları ekleyerek ve yapı iskelesi MVC için oluşturma kullanmaya devam edebilirsiniz. WebForms ve MVC proje içerir.

MVC Web Forms projenize eklemek için yeni iskele kurulmuş öğe ekleme ve seçin **MVC 5 bağımlılıkları**. En az ya da tam tüm betikler gibi içerik dosyaları ihtiyacınız olup olmadığına bağlı olarak seçin. Ardından, projenizde görünümleri ve denetleyici oluşturur, MVC için iskele kurulmuş öğe ekleyin.

### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a>MVC ve Web APİ'si yapı iskelesini - HTTP 404 bulunamadı hatası

İskele kurulmuş öğe projeye eklenirken bir hatayla karşılaştı, projenizi tutarsız bir durumda bırakılır mümkündür. Bazı yapı iskelesi yapılacak değişiklikler geri alınır ancak yüklü NuGet paketleri gibi diğer değişiklikleri geri alınmaz. Yönlendirme yapılandırma değişikliklerini geri giderek iskele kurulmuş öğe olduğunda kullanıcıların bu HTTP 404 hatası alırsınız.

Geçici çözüm:

- MVC için bu hatayı düzeltmek için yeni iskele kurulmuş öğe ekleme ve MVC 5 bağımlılıkları seçin (en az veya tam). Bu işlem tüm gerekli değişiklikleri projenize ekleyin.
- Web API'si için bu hatayı düzeltmek için:

  1. WebApiConfig sınıfı projenize ekleyin.

      [!code-csharp[Main](release-notes/samples/sample25.cs)]

      [!code-vb[Main](release-notes/samples/sample26.vb)]
  2. WebApiConfig.Register yapılandırın\_yöntemi Global.asax dosyasında şu şekilde başlatın:

      [!code-csharp[Main](release-notes/samples/sample27.cs)]

      [!code-vb[Main](release-notes/samples/sample28.vb)]
