---
uid: visual-studio/overview/2013/release-notes
title: "ASP.NET ve Web Araçları Visual Studio 2013 sürüm notları | Microsoft Docs"
author: microsoft
description: "Bu belgede, ASP.NET ve Web Araçları Visual Studio 2013 için sürüm açıklanmaktadır."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 08815768-2702-42ae-ae85-0a59934a11d1
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/release-notes
msc.type: authoredcontent
ms.openlocfilehash: 7f38a0f2693aeb2a4884b9c03719b583423957a8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-and-web-tools-for-visual-studio-2013-release-notes"></a>ASP.NET ve Web Araçları Visual Studio 2013 sürüm notları
====================
tarafından [Microsoft](https://github.com/microsoft)

> Bu belgede, ASP.NET ve Web Araçları Visual Studio 2013 için sürüm açıklanmaktadır.


## <a name="contents"></a>İçindekiler

- [Yükleme notları](#TOC1)
- [Belgeler](#TOC2)
- [Yazılım gereksinimleri](#TOC4)

### <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-2013"></a>ASP.NET ve Web Araçları Visual Studio 2013 için yeni özellikler

- [One ASP.NET](#TOC6)
- [Yeni Web projesi deneyimi](#newproj)
- [ASP.NET Scaffolding](#scaffold)
- [Tarayıcı Bağlantısı](#browser-link)
- [Visual Studio Web Düzenleyicisi geliştirmeleri](#web-editor)
- [Visual Studio'da Azure App Service Web Apps desteği](#waws)
- [Web yayımlama geliştirmeleri](#publish)
- [NuGet 2.7](#nuget)
- [ASP.NET Web formları](#TOC9)
- [ASP.NET MVC 5](#TOC10)
- [ASP.NET Web API 2](#TOC11)
- [ASP.NET SignalR](#TOC13)
- [ASP.NET kimliği](#TOC8)
- [Microsoft OWIN bileşenleri](#TOC7)
- [Entity Framework 6](#ef6)
- [ASP.NET Razor 3](#TOC14)
- [ASP.NET uygulama askıya alma](#TOC15)
- [Bilinen sorunlar ve yeni değişiklikler](#knownissues)

<a id="TOC1"></a>
## <a name="installation-notes"></a>Yükleme notları

ASP.NET ve Web Araçları Visual Studio 2013 için ana yükleyicisinde paketlenmiştir ve indirilebilir [burada](https://www.asp.net/downloads).

<a id="TOC2"></a>
## <a name="documentation"></a>Belgeler

Öğreticiler ve Visual Studio 2013 için ASP.NET ve Web Araçları hakkında diğer bilgi web'da [ASP.NET web sitesi](https://www.asp.net/).

<a id="TOC4"></a>
## <a name="software-requirements"></a>Yazılım Gereksinimleri

ASP.NET ve Web Araçları, Visual Studio 2013 gerektirir.

<a id="TOC5"></a>
## <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-2013"></a>ASP.NET ve Web Araçları Visual Studio 2013 için yeni özellikler

Aşağıdaki bölümlerde sürümünde tanıtılan özellikleri açıklanmaktadır.

<a id="TOC6"></a>
## <a name="one-aspnet"></a>One ASP.NET

Visual Studio 2013 sürümüyle, biz kolayca karıştırmak ve istediğiniz olanlarla eşleşmesi ASP.NET teknolojilerini kullanarak deneyimi birleştirin doğru bir adım gerçekleştirmişsiniz. Örneğin, MVC kullanarak bir proje başlatın ve kolayca Web formları sayfaları daha sonra projeye ekleyin veya bir Web Forms proje ile Web API iskelesi. Bir ASP.NET tümünü sizin için ASP.NET sevdiğiniz şeyleri gerçekleştirmek için bir geliştirici olarak kolaylaştırarak hakkında ' dir. Seçtiğiniz hangi teknoloji olsun, bir ASP.NET arka plandaki güvenilen çerçevesinin oluşturmakta olduğunuz güvenirlik olabilir.

<a id="newproj"></a>
## <a name="new-web-project-experience"></a>Yeni Web projesi deneyimi

Biz, Visual Studio 2013'te yeni web projeleri oluşturma deneyimi Gelişmiş. İçinde **yeni ASP.NET Web projesi** iletişim, istediğiniz, herhangi bir bileşimini teknolojileri (Web Forms, MVC, Web API) yapılandırma, kimlik doğrulama seçeneklerini yapılandırmak ve birim testi projesi eklemek proje türü seçebilirsiniz.

![New ASP.NET Project](release-notes/_static/image1.png)

Yeni iletişim kutusu şablonları birçoğu için varsayılan kimlik doğrulama seçenekleri değiştirmenize olanak tanır. Örneğin, bir ASP.NET Web Forms projesi oluşturduğunuzda, aşağıdaki seçeneklerden birini seçebilirsiniz:

- Kimlik doğrulaması yok
- Bireysel kullanıcı hesapları (ASP.NET üyelik veya sosyal sağlayıcısı günlüğüne)
- Kurumsal hesaplar (Internet uygulamasını Active Directory'de)
- Windows kimlik doğrulaması (Active Directory içinde bir intranet uygulaması)

![Kimlik doğrulama seçenekleri](release-notes/_static/image2.png)

Web projeleri oluşturmak için yeni işlem hakkında daha fazla bilgi için bkz: [Visual Studio 2013'da ASP.NET Web projeleri oluşturma](creating-web-projects-in-visual-studio.md). Yeni kimlik doğrulama seçenekleri hakkında daha fazla bilgi için bkz: [ASP.NET Identity](#TOC8) belgesinde.

<a id="scaffold"></a>
## <a name="aspnet-scaffolding"></a>ASP.NET Scaffolding

ASP.NET İskele bir ASP.NET Web uygulamaları için kod oluşturma çerçevedir. Bir veri modeli ile etkileşime giren projeniz Demirbaş kod eklemek kolaylaştırır.

Visual Studio'nun önceki sürümleri yapı iskelesi ASP.NET MVC projelerine sınırlı. Visual Studio 2013 ile Web Forms dahil olmak üzere tüm ASP.NET projesi için yapı iskelesi artık kullanabilirsiniz. Visual Studio 2013 oluşturma sayfaları şu anda Web Forms projesi için desteklemiyor, ancak yapı iskelesi ile Web Forms projeye MVC bağımlılıkları ekleyerek kullanmaya devam edebilirsiniz. İçin Web formları sayfaları oluşturmaya yönelik destek gelecek bir güncelleştirmede eklenir.

Yapı iskelesi kullanırken, tüm gerekli bağımlılıkların projede yüklü olduğundan emin olun. Bir ASP.NET Web Forms projeyle başlatın ve sonra bir Web API denetleyicisi eklemek için yapı iskelesi kullanın, örneğin, gerekli NuGet paket ve başvuruları projenize otomatik olarak eklenir.

Bir Web Forms projeye MVC yapı iskelesi eklemek için Ekle bir **yeni iskele kurulmuş öğe** seçip **MVC 5 bağımlılıkları** iletişim penceresinde. MVC iskele için iki seçenek vardır; Minimal ve tam. En az seçerseniz, yalnızca NuGet paketlerini ve ASP.NET MVC için başvuruları projenize eklenir. Tam seçeneğini belirlerseniz MVC projesinde gerekli içerik dosyaların yanı sıra en az bağımlılıkları eklenir.

Zaman uyumsuz denetleyicileri iskele desteği yeni Entity Framework 6 zaman uyumsuz özelliklerini kullanır.

Daha fazla bilgi ve öğreticiler için bkz: [ASP.NET yapı İskelesi genel bakış](aspnet-scaffolding-overview.md).

<a id="browser-link"></a>
## <a name="browser-link--signalr-channel-between-browser-and-visual-studio"></a>Tarayıcı bağlantısı – tarayıcı ve Visual Studio arasında SignalR kanal

Yeni [tarayıcı bağlantısı](using-browser-link.md) özelliği, birden çok tarayıcı Visual Studio'ya bağlanın ve tüm bir araç çubuğu düğmesini tıklatarak yenileme olanak tanır. Birden çok tarayıcı mobil öykünücülerinden dahil olmak üzere, geliştirme siteye bağlayabilir ve tüm aynı anda tüm tarayıcılar yenileme için Yenile'yi tıklatın. Tarayıcı bağlantısı da geliştiricilerin tarayıcı bağlantısı uzantıları yazmak için bir API sunar.

![](release-notes/_static/image3.png)

Tarayıcı bağlantısı API'sini yararlanmak geliştiricilere etkinleştirerek, Visual Studio ile bağlı herhangi bir tarayıcıda arasında sınırları kestiği çok Gelişmiş senaryolar oluşturmak mümkün olur. Web Essentials Visual Studio ve tarayıcının geliştirici araçları, mobil öykünücülerinden ve çok daha fazlasını denetleme uzak arasında tümleşik bir deneyim oluşturmak için API avantajlarından yararlanır.

<a id="web-editor"></a>
## <a name="visual-studio-web-editor-enhancements"></a>Visual Studio Web Düzenleyicisi geliştirmeleri

Visual Studio 2013 Razor dosyaları ve HTML dosyaları için yeni bir HTML düzenleyicisi web uygulamalarını içerir. Yeni HTML düzenleyicisi üzerinde HTML5 dayalı tek bir birleşik şema sağlar. Otomatik küme parantezi tamamlama, jQuery UI ve IntelliSense özniteliği, IntelliSense gruplandırma, kimlik ve sınıf adı IntelliSense ve daha iyi performans, biçimlendirme dahil olmak üzere diğer geliştirmeler özniteliği AngularJS sahiptir ve akıllı etiketler.

Aşağıdaki ekran görüntüsü, önyükleme özniteliği IntelliSense HTML Düzenleyicisi'ni kullanarak gösterir.

![HTML Düzenleyicisi'nde IntelliSense](release-notes/_static/image4.png)

Visual Studio 2013 de yerleşik düzenleyicileri daha az hem CoffeeScript ile birlikte gelir. LESS düzenleyici ile tüm harika özellikler CSS Düzenleyicisi'nden gelen ve değişkenleri ve mixins için belirli IntelliSense belgelerde daha az sahip @import zinciri.

<a id="waws"></a>
## <a name="azure-app-service-web-apps-support-in-visual-studio"></a>Visual Studio'da Azure App Service Web Apps desteği

Visual Studio 2013'te .NET 2.2 için Azure SDK'sı ile kullandığınız **Sunucu Gezgini** doğrudan uzak web uygulamalarınızı ile etkileşim kurmak için. Azure hesabınızda oturum açın, yeni web uygulamaları oluşturmak, uygulama yapılandırma, gerçek zamanlı günlükleri ve daha fazla bilgi görüntüleyin. SDK 2.2 hemen sonra gelen yayımlanan, Azure'nın uzaktan hata ayıklama modunda çalıştırmak kullanabileceksiniz. .NET için Azure SDK'sının geçerli sürüm yüklediğinizde, Azure App Service Web Apps için yeni özelliklerin çoğunu Visual Studio 2012'de de çalışır.

Daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Azure App Service'te bir ASP.NET web uygulaması oluşturma](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/)
- [Visual Studio kullanarak Azure App Service web uygulamasında sorun giderme](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/)

<a id="publish"></a>
## <a name="web-publish-enhancements"></a>Web yayımlama geliştirmeleri

Visual Studio 2013 yeni ve geliştirilmiş Web yayımlama özellikleri içerir. Birkaç tanesi aşağıda verilmiştir:

- Kolayca [Web.config dosyasını şifreleme otomatikleştirmek](https://go.microsoft.com/fwlink/?LinkId=325529). (Bu bağlantı ve aşağıdaki iki kadar günde 10/17 aşamalarda kullanılamayabilir MSDN'de belgelerin üzerine gelin.)
- Kolayca [uygulama dağıtımı sırasında çevrimdışı duruma getirmeden otomatikleştirmek](https://go.microsoft.com/fwlink/?LinkId=325530).
- Web dağıtımı için yapılandırma [son değiştirilen tarihi yerine dosya sağlama toplamı kullanmak](https://go.microsoft.com/fwlink/?LinkId=325531) hangi dosyaların sunucusuna kopyalanması belirlemek için.
- Hızlı bir şekilde (Web.config dahil) tek tek seçilen dosyaları FTP kullanıyorsanız veya dosya sistemi yayımladığınızda yöntemleri yanı sıra Web dağıtımı ile yayımlayın.

ASP.NET web dağıtımı hakkında daha fazla bilgi için bkz: [ASP.NET sitesi](https://go.microsoft.com/fwlink/?LinkId=322027).

<a id="nuget"></a>
## <a name="nuget-27"></a>NuGet 2.7

NuGet 2.7 yakından açıklanan yeni özellikleri zengin bir dizi içeren [NuGet 2.7 Sürüm Notları](http://docs.nuget.org/docs/release-notes/nuget-2.7).

Bu sürümü NuGet paketlerini indirmek NuGet paket geri yükleme özelliği için açık izin sağlamak için gereken de kaldırır. İzin (ve ilişkili onay NuGet Tercihleri iletişim kutusuna) NuGet yükleyerek şimdi verilir. Şimdi paket geri yükleme, yalnızca varsayılan olarak çalışır.

<a id="TOC9"></a>
## <a name="aspnet-web-forms"></a>ASP.NET Web Forms

### <a name="one-aspnet"></a>One ASP.NET

Web Forms proje şablonları ile yeni bir ASP.NET deneyimi sorunsuz şekilde tümleşir. MVC ve Web API Web Forms projenize destekler ve bir ASP.NET proje oluşturma Sihirbazı'nı kullanarak kimlik doğrulaması yapılandırabilirsiniz ekleyebilirsiniz. Daha fazla bilgi için bkz: [Visual Studio 2013'da ASP.NET Web projeleri oluşturma](creating-web-projects-in-visual-studio.md).

### <a name="aspnet-identity"></a>ASP.NET Kimlik

Web Forms proje şablonları yeni ASP.NET Identity framework desteği. Ayrıca, şablonları artık Web Forms Intranet projesi oluşturulmasını destekler. Daha fazla bilgi için bkz: [kimlik doğrulama yöntemleri](creating-web-projects-in-visual-studio.md#auth) içinde **Visual Studio 2013'da ASP.NET Web projeleri oluşturma**.

### <a name="bootstrap"></a>önyükleme

Web Forms şablonlarını kullanma [önyükleme](http://twitter.github.io/bootstrap/) bir şık ve esnek kolayca özelleştirebileceğiniz görünüm sağlamak için. Daha fazla bilgi için bkz: [Visual Studio 2013 web projesi şablonları önyükleme](creating-web-projects-in-visual-studio.md#bootstrap).

<a id="TOC10"></a>
## <a name="aspnet-mvc-5"></a>ASP.NET MVC 5

### <a name="one-aspnet"></a>One ASP.NET

Web MVC proje şablonları ile yeni bir ASP.NET deneyimi sorunsuz şekilde tümleşir. MVC projenizi özelleştirebilir ve bir ASP.NET proje oluşturma Sihirbazı'nı kullanarak kimlik doğrulaması yapılandırın. Bir tanıtım öğretici ASP.NET MVC 5 bulunabilir [ASP.NET MVC 5 ile çalışmaya başlama](../../../mvc/overview/getting-started/introduction/getting-started.md).

MVC 4 projeleri MVC 5'e yükseltme hakkında daha fazla bilgi için bkz: [bir ASP.NET MVC 4 ve Web API projesi ASP.NET MVC 5 ve Web API 2'ye yükseltme](../../../mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md).

### <a name="aspnet-identity"></a>ASP.NET Kimlik

MVC proje şablonları, ASP.NET Identity kimlik doğrulama ve kimlik yönetimi için kullanmak üzere güncelleştirilmiştir. Facebook ve Google kimlik doğrulama ve yeni üyelik API'si gösteren bir öğretici bulunabilir [Facebook ve Google OAuth2 ve Openıd oturum açma ile bir ASP.NET MVC 5 uygulaması oluşturmak](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) ve [auth ile bir ASP.NET MVC uygulaması oluşturma ve SQL DB ve Azure App Service'e dağıtma](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/).

### <a name="bootstrap"></a>önyükleme

MVC proje şablonu kullanmak için güncelleştirilmiş [önyükleme](http://getbootstrap.com/) bir şık ve esnek kolayca özelleştirebileceğiniz görünüm sağlamak için. Daha fazla bilgi için bkz: [Visual Studio 2013 web projesi şablonları önyükleme](creating-web-projects-in-visual-studio.md#bootstrap).

### <a name="authentication-filters"></a>Kimlik doğrulaması filtreleri

Kimlik doğrulaması filtreleri olan yeni bir ASP.NET MVC ardışık düzenine yetkilendirme filtreleri önce çalıştıran ve kimlik doğrulama mantığı başına-eylemi belirtmenize olanak veren ASP.NET MVC filtre tür başına denetleyicisi, veya genel olarak tüm denetleyicileri için. Kimlik doğrulaması filtreleri kimlik bilgileri isteği işlemek ve karşılık gelen asıl sağlayın. Kimlik doğrulaması filtreleri, kimlik doğrulama sınaması yetkisiz isteklerine yanıt olarak da ekleyebilirsiniz.

### <a name="filter-overrides"></a>Filtre geçersiz kılar

Şimdi bir geçersiz kılma filtresi belirterek belirli bir eylem yöntemini veya denetleyicileri için hangi filtre uygulamak geçersiz kılabilirsiniz. Geçersiz kılma filtreleri verilen kapsam (eylem veya denetleyici) için çalıştırılmamalıdır filtre türleri kümesi belirtin. Bu genel olarak geçerli ancak ardından belirli eylemler veya denetleyicileri uygulama bazı genel filtreleri hariç filtreleri yapılandırmanıza olanak sağlar.

### <a name="attribute-routing"></a>Öznitelik yönlendirme

ASP.NET MVC şimdi destekleyen bir katkı Tim McCall, yazarı tarafından sayesinde özniteliği yönlendirme [http://attributerouting.net](http://attributerouting.net). Öznitelik yönlendirme ile eylemlerin ve denetleyicilerin yorumlama yollarınızı belirtebilirsiniz.

<a id="TOC11"></a>
## <a name="aspnet-web-api-2"></a>ASP.NET Web API 2

### <a name="attribute-routing"></a>Öznitelik yönlendirme

ASP.NET Web API artık destekliyor katkı Tim McCall, yazarı tarafından sayesinde özniteliği yönlendirme [http://attributerouting.net](http://attributerouting.net). Öznitelik yönlendirme ile Web API yollarınızı eylemleri ve bu gibi denetleyicileri yorumlama belirtebilirsiniz:

[!code-csharp[Main](release-notes/samples/sample1.cs)]

Öznitelik yönlendirme, web API URI'ler üzerinde daha fazla denetim sağlar. Örneğin, tek bir API denetleyicisi kullanarak bir kaynak hiyerarşi kolayca tanımlayabilirsiniz:

[!code-csharp[Main](release-notes/samples/sample2.cs)]

Özniteliği de yönlendirme, isteğe bağlı parametreler, varsayılan değerleri ve rota kısıtlamaları belirtmek için kullanışlı bir sözdizimi sağlar:

[!code-csharp[Main](release-notes/samples/sample3.cs)]

Öznitelik yönlendirme hakkında daha fazla bilgi için bkz: [özniteliği yönlendirme Web API 2'deki](../../../web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2.md).

### <a name="oauth-20"></a>OAuth 2.0

Web API ve tek sayfa uygulaması proje şablonları artık OAuth 2.0 kullanarak yetkilendirmeyi destekler. OAuth 2.0 korunan kaynakları'na istemci erişimini yetkilendirmek için bir çerçevedir. Çeşitli tarayıcılar ve mobil cihazlar dahil olmak üzere istemciler için çalışır.

Destek için OAuth 2.0 taşıyıcı kimlik doğrulaması için Microsoft OWIN bileşenleri tarafından sağlanan ve yetkilendirme sunucusu rolünü uygulama yeni güvenlik ara yazılım üzerinde temel alır. Alternatif olarak, istemciler Azure Active Directory veya ADFS Windows Server 2012 R2 gibi bir kuruluş yetkilendirme sunucusu kullanarak yetki verilebilir.

### <a name="odata-improvements"></a>OData geliştirmeleri

**$Select, $ desteği'ni genişletin, $batch ve $value**

ASP.NET Web API OData $select için artık tam destek sahiptir, $'ni genişletin ve $value. Toplu işleme ve değişiklik kümesi işleme isteği için $batch de kullanabilirsiniz.

$Select ve $expand seçeneklerini genişletin olanak sağlayan bir OData uç noktadan döndürülen verileri şeklini değiştirin. Daha fazla bilgi için bkz: [Introducing $select ve $expand genişletin Web API OData desteği](../../../web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value.md).

**Geliştirilmiş genişletilebilirliği**

OData biçimlendiricilerini genişletilebilir. Atom girişi meta verileri ekleme, adlandırılmış akış ve medya bağlantısı girişler destekleyen, örneği ek açıklama ekleyin ve bağlantıları nasıl oluşturulduğu özelleştirme.

**Türü daha az desteği**

CLR türleri, varlık türleri için tanımlamak gerek kalmadan OData Hizmetleri artık oluşturabilirsiniz. Bunun yerine, OData denetleyicilerinizi alın veya dönüş örneklerini **IEdmObject**, hangi serileştirme/seri durumdan OData biçimlendiricilerini şunlardır.

**Varolan modeli yeniden kullanma**

Var olan bir varlık veri modeli (EDM) zaten varsa, şimdi onu doğrudan yeni bir tane oluşturmak zorunda kalmadan yerine tekrar kullanabilirsiniz. Örneğin, Entity Framework kullanıyorsanız, EF sizin için oluşturur EDM kullanabilirsiniz.

### <a name="request-batching"></a>Toplu işleme istek

Toplu işleme istek ağ trafiğini azaltmak ve daha az chatty kullanıcı arabirimi bir yumuşak sağlamak için tek bir HTTP POST isteği, birden çok işleme birleştirir. ASP.NET Web API isteği toplu işleme için birkaç stratejileri artık destekler:

- OData hizmetine $batch uç noktasını kullanın.
- Birden çok istek tek bir MIME çok bölümlü istek paketi.
- Özel bir toplu işleme biçimini kullanın.

Toplu işleme istek etkinleştirmek için bir yol toplu işleyici ile Web API yapılandırmanızı ekleyin:

[!code-csharp[Main](release-notes/samples/sample4.cs)]

Ayrıca denetleyebilirsiniz olup olmadığını istekleri veya sıralı olarak ya da herhangi bir sırada yürütülür.

### <a name="portable-aspnet-web-api-client"></a>Taşınabilir ASP.NET Web API istemcisi

ASP.NET Web API İstemci artık Windows mağazası ve Windows Phone 8 uygulamalar arasında iş taşınabilir sınıf kitaplıkları oluşturmak için de kullanabilirsiniz. İstemci ile sunucu arasında paylaşılabilir taşınabilir biçimlendiricileri de oluşturabilirsiniz.

### <a name="improved-testability"></a>Geliştirilmiş Test Edilebilirlik

Web API 2 yapar birimine çok daha kolay test API denetleyicisi. Yalnızca istek iletisi ve yapılandırma ile API denetleyicisi oluşturma ve test etmek istediğiniz eylem yöntemini çağırın. Mock kolaydır **UrlHelper** bağlantı oluşturma gerçekleştiren eylem yöntemleri için sınıf.

### <a name="ihttpactionresult"></a>IHttpActionResult

Şimdi Web API eylem yöntemleri sonucunu kapsülleyen Ihttpactionresult uygulayabilirsiniz. Bir Web API eylem yönteminden döndürülen Ihttpactionresult sonuç yanıt iletisi oluşturmak için ASP.NET Web API çalışma zamanı tarafından yürütülür. Bir Ihttpactionresult birim basitleştirmek için tüm Web API eylemden döndürülebilecek Web API uygulamanızı test etme. Özel durum kodları döndürme için sonuçları dahil olmak üzere kutusu dışında Ihttpactionresult uygulamaları sayısı sağlanan kolaylık olması için içerik veya içerik anlaşması yanıtlarını biçimlendirilmiş.

### <a name="httprequestcontext"></a>HttpRequestContext

Yeni **HttpRequestContext** isteğe bağlıdır ancak istekten hemen kullanılabilir olmayan herhangi bir durum izler. Örneğin, kullanabileceğiniz **HttpRequestContext** rota verileri, istemci sertifikası, istekle ilişkili asıl almak için **UrlHelper** ve sanal yol kökü. Kolayca oluşturabileceğiniz bir **HttpRequestContext** için birim testi amaçlar.

İstek için asıl güvenmek yerine istekle akıtılan çünkü **Thread.CurrentPrincipal**, Web API ardışık düzeninde olsa asıl artık istek ömrü kullanılabilir durumdadır.

### <a name="cors"></a>CORS

Brock Allen alanından başka bir harika katkı sayesinde ASP.NET artık tam olarak arası kaynak isteği Paylaşımı (CORS) destekler.

Tarayıcı güvenlik bir web sayfası AJAX istekleri başka bir etki alanına yapmasını engeller. [CORS](http://www.w3.org/TR/cors/) sunucusunun aynı kaynak İlkesi hafifletin sağlar W3C standardıdır. CORS kullanarak, bir sunucu açıkça bazı cross-origin istekleri başkalarının reddetme çalışırken izin verebilirsiniz.

Web API 2 CORS denetim öncesi isteklerde otomatik işlenmesini de dahil olmak üzere, artık destekler. Daha fazla bilgi için bkz: [ASP.NET Web API çıkış noktaları arası istekleri etkinleştirme](../../../web-api/overview/security/enabling-cross-origin-requests-in-web-api.md).

### <a name="authentication-filters"></a>Kimlik doğrulaması filtreleri

Kimlik doğrulaması filtreleri olan yeni bir ASP.NET Web API ardışık düzeninde yetkilendirme filtreleri önce çalıştıran ve kimlik doğrulama mantığı başına-eylemi belirtmenize olanak veren ASP.NET Web API filtrede tür başına denetleyicisi, veya genel olarak tüm denetleyicileri için. Kimlik doğrulaması filtreleri kimlik bilgileri isteği işlemek ve karşılık gelen asıl sağlayın. Kimlik doğrulaması filtreleri, kimlik doğrulama sınaması yetkisiz isteklerine yanıt olarak da ekleyebilirsiniz.

### <a name="filter-overrides"></a>Filtre geçersiz kılma

Şimdi bir geçersiz kılma filtresi belirterek bir belirtilen eylem yöntemini veya denetleyicileri, hangi filtre uygulamak geçersiz kılabilirsiniz. Geçersiz kılma filtreleri verilen kapsam (eylem veya denetleyici) için çalıştırmamalısınız filtre türleri kümesi belirtin. Bu, genel filtreler ekleyin, ama bazı belirli eylemleri veya denetleyicileri hariç olanak sağlar.

### <a name="owin-integration"></a>OWIN tümleştirme

ASP.NET Web API artık tam olarak OWIN destekler ve herhangi bir OWIN özellikli ana üzerinde çalıştırılabilir. De dahil olduğu bir **HostAuthenticationFilter** OWIN kimlik doğrulama sistemi ile tümleştirme sağlar.

OWIN Tümleştirmesi ile Web API SignalR gibi diğer OWIN ara yazılımı yanı sıra kendi işleminde kendi kendine barındırabilir. Daha fazla bilgi için bkz: [Self-Host ASP.NET Web API kullanımı OWIN](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).

<a id="TOC13"></a>
## <a name="aspnet-signalr-20"></a>ASP.NET SignalR 2.0

Aşağıdaki bölümlerde SignalR 2.0 özellikleri açıklanmaktadır.

- [OWIN üzerinde oluşturulmuş](#builtonowin)
- [MapHubs ve MapConnection MapSignalR sunulmuştur](#MapSignalR)
- [Etki alanları arası desteği](#crossdomain)
- [iOS ve Android MonoTouch ve MonoDroid aracılığıyla desteği](#mobile)
- [Taşınabilir .NET istemcisi](#portable)
- [Yeni paket kendini barındırma](#selfhost)
- [Geriye dönük olarak uyumlu sunucu desteği](#backwardcompat)
- [.NET 4.0 için sunucu desteği kaldırıldı](#remove40)
- [İstemciler ve gruplar listesine ileti gönderme](#messagelist)
- [Belirli bir kullanıcı için bir ileti gönderme](#sendtouser)
- [Daha iyi hata işleme desteği](#errorhandling)
- [Daha kolay birim hub'ları test etme](#unittesting)
- [JavaScript hata işleme](#javascripterror)

Mevcut bir 1.x projesini SignalR 2.0 sürümüne yükseltmek nasıl bir örnek için bkz: [bir SignalR yükseltme 1.x proje](../../../signalr/overview/releases/upgrading-signalr-1x-projects-to-20.md).

<a id="builtonowin"></a>
### <a name="built-on-owin"></a>OWIN üzerinde oluşturulmuş

SignalR 2.0 tamamen üzerinde kurulu [OWIN (.NET için açık Web arabirimi)](http://owin.org/). Bu değişiklik Kurulum işlemi için SignalR web barındırılan ve kendi kendini barındıran SignalR uygulamalar arasında çok daha tutarlı hale getirir, ancak ayrıca bir dizi API değişiklikleri istedi.

<a id="MapSignalR"></a>

### <a name="maphubs-and-mapconnection-are-now-mapsignalr"></a>MapHubs ve MapConnection MapSignalR sunulmuştur

OWIN standartları ile uyumluluk için bu yöntemleri için yeniden adlandırıldığı `MapSignalR`. `MapSignalR`parametreleri tüm hub'ları eşler olmadan adlı (olarak `MapHubs` sürümünde mu 1.x); tek tek eşlemek için **PersistentConnection** nesneleri, tür parametresi ve bağlantı olarak URL uzantısı olarak bağlantı türünü belirtin ilk bağımsız değişken.

`MapSignalR` Yöntemi, bir Owın başlangıç sınıfı çağrılır. Visual Studio 2013 Owın başlangıç sınıfı için yeni bir şablon içerir; Bu şablonu kullanmak için aşağıdakileri yapın:

1. Projeye sağ tıklayın
2. Seçin **ekleme**, **yeni öğe...**
3. Seçin **Owın başlangıç sınıfı**. Yeni sınıf **haline**.

İçinde bir **Web uygulaması,** Owın başlangıç sınıfı içeren `MapSignalR` aşağıda gösterildiği gibi Web.Config dosyası uygulama ayarları düğümünde bir giriş kullanılarak Owın'ın başlatma işlemi yöntemi daha sonra eklenen.

İçinde bir **uygulama'kendi kendini barındıran**, başlangıç sınıfı türü parametresi olarak geçirilen `WebApp.Start` yöntemi.

**Hub'ları ve SignalR bağlantıları eşleme 1.x (bir web uygulaması genel uygulama dosyasından):** 

[!code-csharp[Main](release-notes/samples/sample5.cs)]

**Eşleme hub'ları ve bağlantıları SignalR 2.0 (dosyadan bir Owın başlangıç sınıfı):** 

[!code-csharp[Main](release-notes/samples/sample6.cs)]

İçinde bir **uygulama'kendi kendini barındıran**, başlangıç sınıfı için tür parametre olarak geçirilen `WebApp.Start` aşağıda gösterildiği gibi yöntemi.

[!code-csharp[Main](release-notes/samples/sample7.cs)]

<a id="crossdomain"></a>

### <a name="cross-domain-support"></a>Etki alanları arası desteği

Etki alanı istekleri arası 1.x tek EnableCrossDomain tarafından kontrol SignalR bayrak. Bu bayrak JSONP ve CORS isteklerini denetlenir. Daha fazla esneklik için tüm CORS desteği SignalR sunucu bileşeninden kaldırıldı (JavaScript lients kullanmaya devam CORS normalde tarayıcı onu destekleyen algılanırsa), ve yeni OWIN ara yazılımı sağlandıktan bu senaryoları desteklemek kullanılabilir.

SignalR 2. 0'da, (etki alanları arası istekleri eski tarayıcılarda desteklemek için), istemcide varsa JSONP gereklidir, açıkça ayarlayarak etkinleştirilmesi gerekir `EnableJSONP` üzerinde `HubConfiguration` nesnesine `true`, aşağıda gösterildiği gibi. CORS az güvenlidir JSONP varsayılan olarak devre dışıdır.

SignalR 2. 0 ' yeni CORS Ara eklemek için Ekle `Microsoft.Owin.Cors` projenizi ve çağrı kitaplığa `UseCors` aşağıdaki bölümde gösterildiği gibi SignalR Ara önce.

**Microsoft.Owin.Cors projenize ekleme**: Bu Kitaplığı'nı yüklemek için Paket Yöneticisi konsolunda aşağıdaki komutu çalıştırın:

[!code-powershell[Main](release-notes/samples/sample8.ps1)]

Bu komut 2.0.0 ekleyeceksiniz projenize paketin sürümü.

**UseCors çağırma**

Aşağıdaki kod parçacıkları nasıl SignalR öğesinde etki alanları arası bağlantılar uygulandığını gösteren 1.x ve 2. 0.

**SignalR öğesinde etki alanları arası istekleri uygulama 1.x (genel uygulama dosyadan)**

[!code-csharp[Main](release-notes/samples/sample9.cs)]

**Uygulama etki alanları arası isteklerde SignalR 2. 0 (C# kod dosyası)**

Aşağıdaki kod, CORS veya JSONP SignalR 2.0 projede etkinleştirme gösterilmiştir. Bu kod örneği kullanan `Map` ve `RunSignalR` yerine `MapSignalR`, CORS desteği gerektiren SignalR istekleri için CORS Ara çalışır (yerine belirtilen yoldaki tüm trafik için `MapSignalR`.) `Map` belirli bir URL öneki için yerine tüm uygulama için çalıştırmak için gereken diğer tüm ara yazılımı için de kullanılabilir.

[!code-csharp[Main](release-notes/samples/sample10.cs)]

<a id="mobile"></a>

### <a name="ios-and-android-support-via-monotouch-and-monodroid"></a>iOS ve Android MonoTouch ve MonoDroid aracılığıyla desteği

İOS ve MonoTouch ve MonoDroid bileşenlerini kullanan Android istemciler için destek eklenmiştir [Xamarin Kitaplığı](https://xamarin.com/). Bunların nasıl kullanılacağını hakkında daha fazla bilgi için bkz: [kullanarak Xamarin bileşen](https://github.com/SignalR/SignalR/wiki/Building-Mono.Mobile.sln). Bu bileşenlerin kullanılabilir olması [Xamarin mağazasında](https://store.xamarin.com/) SignalR RTW yayın olduğunda kullanılabilir.

<a id="portable"></a>### Taşınabilir .NET istemcisi

Platformlar arası geliştirme, Silverlight, WinRT daha iyi kolaylaştırmak ve Windows Phone istemcileri, aşağıdaki platformları destekleyen bir tek taşınabilir .NET istemcisi ile değiştirilmiştir:

- NET 4.5
- Silverlight 5
- WinRT (Windows mağazası uygulamaları için .NET)
- Windows Phone 8

<a id="selfhost"></a>

### <a name="new-self-host-package"></a>Yeni paket kendini barındırma

SignalR kendi kendini barındıran (bir işlemde barındırılan SignalR uygulamalarını veya başka bir uygulamanın yerine bir web sunucusunda barındırılan) kullanmaya başlama kolaylaştırmak için bir NuGet paketi artık yoktur. SignalR ile yerleşik bir kendi kendini barındıran proje yükseltmek için 1.x, Microsoft.AspNet.SignalR.Owin paketini kaldırın ve Microsoft.AspNet.SignalR.SelfHost paketi ekleyin. Kendini barındırma paketi ile çalışmaya başlama hakkında daha fazla bilgi için bkz: [Öğreticisi: SignalR kendini barındırma](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).

<a id="backwardcompat"></a>

### <a name="backward-compatible-server-support"></a>Geriye dönük olarak uyumlu sunucu desteği

' In önceki sürümlerinde SignalR, istemcide kullanılan SignalR paket ve aynı olması için gereken sunucu sürümleri. Güncelleştirme zor olurdu kalın istemci uygulamalarını desteklemek için eski bir istemciyi daha yeni bir sunucu sürümünü kullanarak SignalR 2.0 artık destekler. **Not: SignalR 2.0 yeni istemcilerle eski sürümleriyle oluşturulan sunucularını desteklemez.**

<a id="remove40"></a>

### <a name="removed-server-support-for-net-40"></a>.NET 4.0 için sunucu desteği kaldırıldı

SignalR 2.0, .NET 4.0 ile sunucu birlikte çalışabilirlik için destek bıraktı. .NET 4.5 SignalR 2.0 sunucularıyla kullanılması gerekir. SignalR 2.0 için bir .NET 4.0 istemci hala var.

<a id="messagelist"></a>

### <a name="sending-a-message-to-a-list-of-clients-and-groups"></a>İstemciler ve gruplar listesine ileti gönderme

SignalR 2. 0'da, istemci ve grup kimlikleri listesini kullanarak ileti göndermek mümkündür. Aşağıdaki kod parçacıkları bunun nasıl yapılacağı gösterilmektedir.

**İstemcileri ve PersistentConnection kullanarak grupları listesine ileti gönderme**

[!code-csharp[Main](release-notes/samples/sample11.cs)]

**İstemcileri ve hub'ları kullanarak grupları listesine ileti gönderme**

[!code-csharp[Main](release-notes/samples/sample12.cs)]

<a id="sendtouser"></a>

### <a name="sending-a-message-to-a-specific-user"></a>Belirli bir kullanıcı için bir ileti gönderme

Bu özellik, USERID belirtmek kullanıcılara bir Irequest IUserIdProvider yeni arabirimi aracılığıyla göre:

**IUserIdProvider arabirimi**

[!code-csharp[Main](release-notes/samples/sample13.cs)]

Varsayılan olarak kullanıcının IPrincipal.Identity.Name kullanıcı adı olarak kullandığı uygulaması olacaktır.

Hub'ları, bu kullanıcılara yeni bir API aracılığıyla iletileri göndermek kullanabileceksiniz:

**API Clients.User kullanma**

[!code-csharp[Main](release-notes/samples/sample14.cs)]

<a id="errorhandling"></a>

### <a name="better-error-handling-support"></a>Daha iyi hata işleme desteği

Kullanıcılar şimdi throw **HubException** herhangi bir hub çağrısının gelen. Oluşturucusunun **HubException** dize ileti ve nesneyi alabilir ek hata verileri. SignalR otomatik-özel durum seri hale getirmek ve burada bu reddetme/hub yöntemi çağrısının başarısız kullanılacak istemciye göndermek.

**Ayrıntılı hub'ı özel durumlar Göster** ayarı var. şifrelemeyle **HubException** veya değil; istemciye gönderilen her zaman gönderildi.

**Sunucu tarafı kodu bir HubException istemciye gönderme gösterme**

[!code-csharp[Main](release-notes/samples/sample15.cs)]

**Sunucudan gönderilen bir HubException yanıtlama gösteren JavaScript istemci kodu**

[!code-html[Main](release-notes/samples/sample16.html)]

**Sunucudan gönderilen bir HubException yanıtlama gösteren .NET istemci kodu**

[!code-csharp[Main](release-notes/samples/sample17.cs)]

<a id="unittesting"></a>

### <a name="easier-unit-testing-of-hubs"></a>Daha kolay birim hub'ları test etme

SignalR 2.0 içerir adlı bir arabirim `IHubCallerConnectionContext` sahte istemci tarafı çağrılarını oluşturmak kolaylaştırır hub'ları üzerinde. Aşağıdaki kod parçacıkları popüler test harnesses ile bu arabirimi kullanarak göstermek [xUnit.net](https://github.com/xunit/xunit) ve [moq](https://code.google.com/p/moq/).

**Birim SignalR ile xUnit.net testi**

[!code-csharp[Main](release-notes/samples/sample18.cs)]

**Birim SignalR ile moq testi**

[!code-csharp[Main](release-notes/samples/sample19.cs)]

<a id="javascripterror"></a>

### <a name="javascript-error-handling"></a>JavaScript hata işleme

SignalR 2. 0'da, tüm JavaScript hata işleme geri aramalar ham dizeler yerine JavaScript hata nesneleri döndürür. Bu hata işleyicileri için daha zengin bilgi akışı SignalR sağlar. İç özel duruma gelen alabilirsiniz `source` hata özelliğidir.

**Start.Fail özel durumu işler JavaScript istemci kodu**

[!code-javascript[Main](release-notes/samples/sample20.js)]

<a id="TOC8"></a>
## <a name="aspnet-identity"></a>ASP.NET Kimlik

### <a name="new-aspnet-membership-system"></a>Yeni ASP.NET üyelik sistemini

ASP.NET, ASP.NET uygulamaları için yeni üyelik sistemi kimliğidir. ASP.NET Identity, kullanıcı özel profil verileri uygulama verileriyle tümleştirmeyi kolaylaştırır. ASP.NET Identity Kalıcılık modeli kullanıcı profilleri için uygulamanızda seçmenize olanak sağlar. Verileri bir SQL Server veritabanına veya Azure depolama tabloları gibi NoSQL veri depolarına dahil olmak üzere başka bir veri deposu depolayabilirsiniz. Daha fazla bilgi için bkz: [tek tek kullanıcı hesaplarını](creating-web-projects-in-visual-studio.md#indauth) içinde **Visual Studio 2013'da ASP.NET Web projeleri oluşturma**.

### <a name="claims-based-authentication"></a>Beyana dayalı kimlik doğrulama

ASP.NET, şimdi burada kullanıcının kimliğini güvenilir verene gelen talepler kümesi olarak temsil edilir talep tabanlı kimlik doğrulaması, destekler. Kullanıcılar, kimliği doğrulanmış bir kullanıcı adı ve parola bir uygulama veritabanında tutulan veya sosyal kimlik sağlayıcısı kullanarak olabilir (örneğin: Microsoft Accounts, Facebook, Google, Twitter), veya Azure Active Directory kuruluş hesaplarıyla kullanarak veya Active Directory Federasyon Hizmetleri (ADFS).

### <a name="integration-with-azure-active-directory-and-windows-server-active-directory"></a>Azure Active Directory ve Windows Server Active Directory ile tümleştirme

Artık Azure Active Directory veya Windows Server Active Directory (AD) kimlik doğrulaması için kullanmak ASP.NET projeleri oluşturabilirsiniz. Daha fazla bilgi için bkz: [Kurumsal hesaplar](creating-web-projects-in-visual-studio.md#orgauth) içinde **Visual Studio 2013'da ASP.NET Web projeleri oluşturma**.

### <a name="owin-integration"></a>OWIN tümleştirme

ASP.NET kimlik doğrulaması şimdi tüm OWIN tabanlı ana bilgisayarda kullanılan OWIN ara yazılımı temel alır. OWIN hakkında daha fazla bilgi için aşağıdakilere bakın [Microsoft OWIN bileşenleri](#TOC7) bölümü.

<a id="TOC7"></a>
## <a name="microsoft-owin-components"></a>Microsoft OWIN bileşenleri

[.NET için Web Arabirimi'ni açmak](http://owin.org/) (OWIN) .NET web sunucuları ve web uygulamaları arasındaki bir Özet tanımlar. OWIN web uygulamaları konak belirsiz yapmadan sunucu web uygulamasından ayrıştırır. Örneğin, IIS'de bir OWIN tabanlı web uygulaması konağı ya da özel bir işlem olarak kendini barındırma.

Microsoft OWIN bileşenleri (Katana proje olarak da bilinir) sunulan değişiklikler, yeni sunucu ve ana bileşenlerini, yeni yardımcı kitaplıkları ve ara yazılımı ve yeni kimlik doğrulaması ara yazılımı içerir.

OWIN ve Katana hakkında daha fazla bilgi için bkz: [OWIN ve Katana yenilikler](../../../aspnet/overview/owin-and-katana/index.md).

**Not: [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) uygulamalar IIS Klasik modda çalışamaz; tümleşik modunda çalıştırılması gerekir.**

**Not: [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) uygulamalarını tam güvende çalıştırmanız gerekir.**

### <a name="new-servers-and-hosts"></a>Yeni sunucuları ve ana bilgisayarlar

Bu sürümle birlikte, kendi kendini barındıran senaryoları etkinleştirmek için yeni bileşenler eklendi. Bu bileşenler aşağıdaki NuGet paketleri şunlardır:

- **Microsoft.Owin.Host.HttpListener**. Kullanan bir OWIN sunucusu sağlar **HttpListener** HTTP isteklerini dinlemeye ve OWIN ardışık düzenine yönlendirir.
- **Microsoft.Owin.Hosting** bir konsol uygulaması veya Windows hizmeti gibi özel bir işlemde bir OWIN ardışık kendini barındırma isteyen geliştiriciler için bir kitaplık sağlar.
- **OwinHost**. Tek başına yürütülebilir dosya sarmalar sağlar `Microsoft.Owin.Hosting` ve bir özel ana bilgisayar uygulaması yazmak zorunda kalmadan bir OWIN ardışık kendini barındırma olanak tanır.

Ayrıca, `Microsoft.Owin.Host.SystemWeb` paket şimdi için ipuçları sağlamak üzere ara yazılım sağlar **SystemWeb** sunucu, belirli bir ASP.NET ardışık düzen aşaması sırasında ara yazılımın çağrılması gerektiğini belirten. Bu özellik, erken ASP.NET ardışık düzeninde çalışması gerektiğini kimlik doğrulaması ara yazılımı için özellikle yararlıdır.

### <a name="helper-libraries-and-middleware"></a>Yardımcı kitaplıkları ve Ara

Yalnızca işlev ve tür tanımları OWIN belirtiminden kullanarak OWIN bileşenlerini yazabilirsiniz olsa da yeni `Microsoft.Owin` paket soyutlamalar daha kullanıcı dostu kümesi sağlar. Çeşitli önceki paketler bu pakete birleştirir (örn., `Owin.Extensions`, `Owin.Types`) bir tek, iyi yapılandırılmış nesne modeline daha sonra kolayca diğer OWIN bileşenleri tarafından kullanılabilir. Aslında, Microsoft OWIN bileşenlerin çoğunun şimdi bu paketi kullanan.

> [!NOTE]
> [OWIN](http://www.owin.org) uygulamalar IIS Klasik modda çalışamaz; tümleşik modunda çalıştırılması gerekir.

> [!NOTE]
> [OWIN](http://www.owin.org) uygulamalarını tam güvende çalıştırmanız gerekir.

Bu sürüm, çalışan bir OWIN uygulama doğrulamak için ara yazılım artı hataları araştırmanıza yardımcı olması için hata sayfası ara yazılımını içeren Microsoft.Owin.Diagnostics paketi de içerir.

### <a name="authentication-components"></a>Kimlik doğrulaması bileşenleri

Aşağıdaki kimlik doğrulama bileşenleri kullanılabilir.

- **Microsoft.Owin.Security.ActiveDirectory**. Şirket içi veya bulut tabanlı dizin hizmetlerini kullanarak kimlik doğrulamasını etkinleştirir.
- **Microsoft.Owin.Security.Cookies** etkinleştirir kimlik doğrulaması tanımlama bilgileri kullanma. Bu paketi daha önce adlı `Microsoft.Owin.Security.Forms`.
- **Microsoft.Owin.Security.Facebook** Facebook'ın OAuth tabanlı hizmet kullanarak etkinleştirir kimlik doğrulaması.
- **Microsoft.Owin.Security.Google** etkinleştirir kimlik doğrulaması kullanarak Google Openıd tabanlı hizmet.
- **Microsoft.Owin.Security.Jwt** JWT belirteçleri kullanarak etkinleştirir kimlik doğrulaması.
- **Microsoft.Owin.Security.MicrosoftAccount** Enables authentication using Microsoft accounts.
- **Microsoft.Owin.Security.OAuth**. Ara yazılım yanı sıra bir OAuth yetkilendirme sunucusu taşıyıcı belirteçlerin kimlik doğrulamasını sağlar.
- **Microsoft.Owin.Security.Twitter** Twitter'ın OAuth tabanlı hizmet kullanarak etkinleştirir kimlik doğrulaması.

Bu sürüm ayrıca içerir `Microsoft.Owin.Cors` çıkış noktaları arası HTTP isteklerini işlemek için ara yazılımı içeren paket.

> [!NOTE]
> Visual Studio 2013'ın son sürümünde JWT imzalama desteği kaldırılmıştır.

<a id="ef6"></a>
## <a name="entity-framework-6"></a>Entity Framework 6

Yeni özellikler ve diğer değişiklikler Entity Framework 6 listesi için bkz: [Entity Framework sürüm geçmişi](https://msdn.com/data/jj574253).

<a id="TOC14"></a>
## <a name="aspnet-razor-3"></a>ASP.NET Razor 3

ASP.NET Razor 3 aşağıdaki yeni özellikleri içerir:

- Sekme düzenleme desteği. Preivously, **belgeyi Biçimlendir** komutu, otomatik girintileme ve Visual Studio'da biçimlendirme otomatik işe doğru kullanırken **sekmeleri tut** seçeneği. Bu değişiklik, Visual Studio için biçimlendirme sekmesini Razor kodu biçimlendirme düzeltir.
- Bağlantıları oluşturulurken URL yeniden yazma kuralı desteği.
- Güvenliği saydam öznitelik kaldırma.
 > [!NOTE]
 > Bu önemli bir değişiklik ve Razor 2 MVC5 veya MVC5 karşı derlenmiş derlemeler ile uyumsuz durumdayken Razor 3 MVC4 ve önceki sürümlerinde, uyumsuz hale getirir.

Visual Studio 2013'te yayın öncesi sürümlerinden sabit razor 3 sorunları bulunabilir [burada](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Resolved%7cClosed&type=All&priority=All&release=All%7cv5.0%2bPreview%7cv5.0%2bRC%7cv5.0%2bRTM&assignedTo=All&component=Web%2bPages%252fRazor&reasonClosed=Fixed&sortField=LastUpdatedDate&sortDirection=Descending&page=0).

<a id="TOC15"></a>
## <a name="aspnet-app-suspend"></a>ASP.NET uygulama askıya alma

ASP.NET uygulama askıya alma tüketicisinin kullanıcı deneyimi ve çok sayıda tek bir makinede ASP.NET siteleri barındırmak için ekonomik model değişiklikleri .NET Framework 4.5.1'deki oyun değiştirme bir özelliktir. Daha fazla bilgi için bkz: [ASP.NET uygulama Askıya Al – esnek paylaşılan .NET web barındırma](https://blogs.msdn.com/b/dotnet/archive/2013/10/09/asp-net-app-suspend-responsive-shared-net-web-hosting.aspx).

<a id="knownissues"></a>
## <a name="known-issues-and-breaking-changes"></a>Bilinen sorunlar ve yeni değişiklikler

Bu bölümde, Visual Studio 2013 için bilinen sorunlar ve ASP.NET ve Web Araçları önemli değişiklikler açıklanmaktadır.

### <a name="nuget"></a>NuGet

- [SLN dosyasını kullanırken, yeni paket geri yüklemesi üzerinde Mono işe yaramazsa](https://nuget.codeplex.com/workitem/3596) – bir yaklaşan nuget.exe indirme düzeltilecektir ve [NuGet.CommandLine paket](http://www.nuget.org/packages/NuGet.CommandLine/) güncelleştirin.
- [Yeni paket geri yüklemesi Wix projelerle işe yaramazsa](https://nuget.codeplex.com/workitem/3598) – bir yaklaşan nuget.exe indirme düzeltilecektir ve [NuGet.CommandLine paket](http://www.nuget.org/packages/NuGet.CommandLine/) güncelleştirin.
- [Otomatik paket geri yüklemesi çözüm klasörünün altında projeleri için işe yaramazsa](https://nuget.codeplex.com/workitem/3625) – NuGet 2.8 düzeltilecektir.

### <a name="aspnet-web-api"></a>ASP.NET Web API

1. `ODataQueryOptions<T>.ApplyTo(IQueryable)`döndürmez `IQueryable<T>` desteği eklediğimiz olarak her zaman `$select` ve `$expand`.

    Önceki örneklerimizi `ODataQueryOptions<T>` her zaman Integer dönüş değeri `ApplyTo` için `IQueryable<T>`. Bu sorgu seçenekleri olduğundan daha önce çalışan biz daha önce desteklenen (`$filter`, `$orderby`, `$skip`, `$top`) şekli sorgusunun değiştirmeyin. Destekliyoruz göre `$select` ve `$expand` yönteminden döndürülen değer `ApplyTo` değişmeyecek `IQueryable<T>` her zaman.

    [!code-csharp[Main](release-notes/samples/sample21.cs)]

    Örnek kodu daha önce kullanıyorsanız, istemci değil gönderirseniz, çalışmaya devam eder `$select` ve `$expand`. Ancak, desteklemek istiyorsanız `$select` ve `$expand` için bu kodu değiştirmeniz gerekir.

    [!code-csharp[Main](release-notes/samples/sample22.cs)]
2. **Request.Url veya RequestContext.Url toplu isteği sırasında null**

    Bir toplu senaryosunda **UrlHelper** erişilen zaman null **Request.Url** veya **RequestContext.Url**.

    Bu sorun şu anda burada izlenir: [BatchRequestContext.Url isteği toplu işleme için null](http://aspnetwebstack.codeplex.com/workitem/1301).

    Yeni bir örneğini oluşturmak için bu sorun için geçici çözüm olan **UrlHelper**, aşağıdaki örnekte olduğu gibi:

    **UrlHelper yeni bir örneğini oluşturma**

    [!code-csharp[Main](release-notes/samples/sample23.cs)]

### <a name="aspnet-mvc"></a>ASP.NET MVC

1. Veri görünümüne duyurduğumuzda AntiForgerToken doğrulama yapmak görünümler varsa MVC5 ve OrgAuth, kullanırken, aşağıdaki hata arasında gelebilir:

    **Hata**:

    *'/' Uygulamasında sunucu hatası.*

    *Bir talep türü 'http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier' veya 'http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider' üzerinde sağlanan Claimsıdentity yoktu. Talep tabanlı kimlik doğrulaması ile sahteciliğe karşı koruma belirteci desteğini etkinleştirmek için lütfen yapılandırılmış Talep sağlayıcı ürettiği Claimsıdentity örneklerinde bu talepler her ikisi de sağladığını doğrulayın. Yapılandırılmış Talep sağlayıcı yerine farklı bir talep türüyle benzersiz bir tanımlayıcı olarak kullanıyorsa, AntiForgeryConfig.UniqueClaimTypeIdentifier statik özelliği ayarlanarak yapılandırılabilir.*

    **Geçici çözüm**:

    Sorunu gidermek için Global.asax dosyasında aşağıdaki satırı ekleyin:

    `AntiForgeryConfig.UniqueClaimTypeIdentifier = ClaimTypes.Name;`

    Bu, bir sonraki sürümde düzeltilecektir.
2. MVC4 uygulaması için MVC5 yükseltme yaptıktan sonra çözümü oluşturun ve başlatın. Aşağıdaki hata görmeniz gerekir:

    [A]System.Web.WebPages.Razor.Configuration.HostSection cannot be cast to [B]System.Web.WebPages.Razor.Configuration.HostSection. Tür A kaynaklanan ' System.Web.WebPages.Razor, sürüm 2.0.0.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35 ' konumunda ' Default' bağlamda ' C:\windows\Microsoft.Net\assembly\GAC\_MSIL\System.Web.WebPages.Razor\ v4.0\_2.0.0.0\_\_31bf3856ad364e35\System.Web.WebPages.Razor.dll'. Tür B kaynaklanan ' System.Web.WebPages.Razor, sürüm 3.0.0.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35 ' konumunda ' Default' bağlamda ' C:\Windows\Microsoft.NET\Framework\v4.0.30319\Temporary ASP.NET Files\root\6d05bbd0\ e8b5908e\assembly\dl3\c9cbca63\f8910382\_6273ce01\System.Web.WebPages.Razor.dll'.

    Yukarıdaki hatayı düzeltmek için açık *tüm* (görünümler klasöründe olanlar dahil) Web.config dosyasında projenizi ve aşağıdakileri yapın:

    1. Sürümüne "4.0.0.0" "System.Web.Mvc" "5.0.0.0" tüm oluşumlarını güncelleştirin.
    2. "System.Web.Helpers", "2.0.0.0" sürümünü tüm oluşumlarını güncelleştirme &quot;System.Web.WebPages&quot; ve &quot;System.Web.WebPages.Razor&quot; "3.0.0.0" için

    Örneğin, yukarıdaki değişiklikleri yaptıktan sonra derleme bağlamaları aşağıdaki gibi görünmelidir:

    [!code-xml[Main](release-notes/samples/sample24.xml)]

    MVC 4 projeleri MVC 5'e yükseltme hakkında daha fazla bilgi için bkz: [bir ASP.NET MVC 4 ve Web API projesi ASP.NET MVC 5 ve Web API 2'ye yükseltme](../../../mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md).
3. İstemci tarafı doğrulama örtük doğrulama jQuery ile kullanırken, doğrulama iletisi bazen türüne sahip bir HTML input öğesi için geçersiz = 'number'. Doğrulama hatasını gerekli değeri ("Yaş alanı gereklidir") geçersiz bir sayı geçerli bir sayı gerekli olduğunu doğru ileti yerine girildiğinde gösterilir.

    Bu sorun sık kurulmuş kodu ile oluşturma ve düzenleme görünümleri bir tamsayı özelliği ile bir model için bulunamadı.

    Bu sorunu çözmek için gelen Düzenleyicisi yardımcı değiştirin:

    `@Html.EditorFor(person => person.Age)`

    Hedef:

    `@Html.TextBoxFor(person => person.Age)`
4. ASP.NET MVC 5 artık kısmi güven destekler. MVC veya Webapı ikili dosyaları bağlama projeleri kaldırmalısınız [SecurityTransparent](https://msdn.microsoft.com/library/system.security.securitytransparentattribute.aspx) özniteliği ve [AllowPartiallyTrustedCallers](https://msdn.microsoft.com/library/system.security.allowpartiallytrustedcallersattribute.aspx) özniteliği. Bu öznitelikler kaldırma derleyici hataları aşağıdaki gibi ortadan kaldırır.

    `Attempt by security transparent method ‘MyComponent' to access security critical type 'System.Web.Mvc.MvcHtmlString' failed. Assembly 'PagedList.Mvc, Version=4.3.0.0, Culture=neutral, PublicKeyToken=abbb863e9397c5e1' is marked with the AllowPartiallyTrustedCallersAttribute, and uses the level 2 security transparency model. Level 2 transparency causes all methods in AllowPartiallyTrustedCallers assemblies to become security transparent by default, which may be the cause of this exception.`

    > Not, bir yan etkisi Bu, aynı uygulamada 4.0 ve 5.0 derlemeleri kullanamazsınız. Bunların tümünün 5.0 güncelleştirmeniz gerekir.

### <a name="spa-template-with-facebook-authorization-may-cause-instability-in-ie-while-the-web-site-is-hosted-in-intranet-zone"></a>Web sitesi intranet bölgesinde barındırılmasına karşın Facebook yetkilendirme SPA şablonla IE kararsızlığına neden olabilir

SPA şablonu Facebook dış oturum sağlar. Şablon kullanılarak oluşturulan projeyi yerel olarak çalıştırırken, oturum açma IE çökmesine neden olabilir.

Çözüm:

1. Web sitesi Internet bölgesinde barındırması; veya

2. IE dışında bir tarayıcıda senaryosunu sınayın.

### <a name="web-forms-scaffolding"></a>Web Forms Scaffolding

Web Forms İskele VS2013 kaldırıldı ve Visual Studio için gelecekteki bir güncelleştirme kullanıma sunulacaktır. Ancak, MVC bağımlılıkları ekleyerek ve yapı iskelesi MVC için oluşturma Web Forms projedeki yapı iskelesi kullanabilirsiniz. Projenizi Web Forms ve MVC bir birleşimini içerir.

MVC Web Forms projenize eklemek için yeni iskele kurulmuş Öğe Ekle ve seçin **MVC 5 bağımlılıkları**. En az veya betikleri gibi içerik tüm dosyaların gerekmediğini bağlı olarak tam seçin. Ardından, görünümler ve denetleyicisi projenizde oluşturacak MVC için kurulmuş öğe ekleyin.

### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a>MVC and Web API Scaffolding - HTTP 404, Not Found error

Bir projeye iskele kurulmuş öğe olduğunda hatayla karşılaşırsanız, projenizin tutarsız bir durumda bırakılır mümkündür. Bazı yapı iskelesi yapılan değişiklikler geri alınacak ancak yüklü NuGet paketleri gibi diğer değişiklikler geri alınacak değil. Yönlendirme yapılandırması değişiklikler geri alınır, gezinme öğeleri iskele kurulmuş kullanıcılar bir HTTP 404 hata alır.

Geçici çözüm:

- MVC için bu hatayı düzeltmek için yeni iskele kurulmuş Öğe Ekle ve MVC 5 bağımlılıkları seçin (en az veya tam). Bu işlem tüm gerekli değişiklikleri projenize ekleyin.
- Web API için bu hatayı düzeltmek için:

    1. WebApiConfig sınıfı projenize ekleyin.

        [!code-csharp[Main](release-notes/samples/sample25.cs)]

        [!code-vb[Main](release-notes/samples/sample26.vb)]
    2. Uygulamada WebApiConfig.Register yapılandırma\_yöntemi Global.asax dosyasında şu şekilde başlatın:

        [!code-csharp[Main](release-notes/samples/sample27.cs)]

        [!code-vb[Main](release-notes/samples/sample28.vb)]
