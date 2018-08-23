---
uid: mvc/mvc5
title: ASP.NET MVC 5 | Microsoft Docs
author: rick-anderson
description: ASP.NET MVC 5 ASP.NET MVC 5 tanınmış tasarım desenleri ve AS. gücünü kullanarak ölçeklenebilir, standartlara dayanan web uygulamaları oluşturmaya yönelik bir çerçevedir...
ms.author: riande
ms.date: 01/20/2014
ms.assetid: f79fbf7f-59e5-4279-a832-c1a0294630f4
msc.legacyurl: /mvc/mvc5
msc.type: content
ms.openlocfilehash: c837560e0ad9618decaba9761da9cf35e0f03f08
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756313"
---
<a name="aspnet-mvc-5"></a>ASP.NET MVC 5
====================
## <a name="whats-new-in-aspnet-mvc-5"></a>ASP.NET MVC 5 yenilikleri

### <a name="one-aspnet"></a>Tek ASP.NET

Web MVC proje şablonları yeni bir ASP.NET deneyimiyle sorunsuzca tümleştirin. MVC projenizi özelleştirmek ve kimlik doğrulaması kullanarak bir ASP.NET projesi oluşturma Sihirbazı'nı yapılandırın. ASP.NET MVC 5 için giriş niteliğindeki bir eğitim şu yolda bulunabilir: [ASP.NET MVC 5 ile çalışmaya başlama](overview/getting-started/introduction/getting-started.md).

MVC 5 MVC 4 projelerini yükseltme hakkında daha fazla bilgi için bkz: [bir ASP.NET MVC 4 ve Web API projelerini ASP.NET MVC 5 ve Web API 2'ye yükseltme](overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md).

### <a name="aspnet-identity"></a>ASP.NET Kimlik

MVC proje şablonları, kimlik ve kimlik yönetimi için ASP.NET Identity kullanacak şekilde güncelleştirildi. Facebook ve Google kimlik doğrulama ve yeni üyelik API'si gösteren bir öğretici bulabilirsiniz [Facebook ve Google OAuth2 ve Openıd oturum açma ile ASP.NET MVC 5 uygulaması oluşturma](overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) ve [ile güvenli bir ASP.NET MVC uygulaması dağıtma Üyelik, OAuth ve SQL veritabanı'na bir Windows Azure Web sitesi](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).

### <a name="bootstrap"></a>Önyükleme

MVC proje şablonu kullanmak için güncelleştirilmiş [önyükleme](http://getbootstrap.com/) bir zarif ve hızlı yanıt veren kolayca özelleştirebileceğiniz görünüm sağlamak için. Daha fazla bilgi için [Bootstrap Visual Studio 2013 web proje şablonlarında](../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap).

### <a name="authentication-filters"></a>Kimlik doğrulama filtreleri

[Kimlik doğrulama filtreleri](http://www.dotnetcurry.com/showarticle.aspx?ID=957) ASP.NET MVC, ASP.NET MVC ardışık düzende yetkilendirme filtreleri önce çalışan ve kimlik doğrulama mantığı eylem başına, belirtmenizi sağlar bir filtrede yeni bir tür olan her denetleyici, veya genel olarak tüm denetleyicileri için. Kimlik doğrulama filtreleri, kimlik bilgileri isteği işlemek ve karşılık gelen sorumlu sağlayın. Kimlik doğrulama filtreleri kimlik doğrulama sınaması, yetkisiz istekleri için yanıt de ekleyebilirsiniz. Bkz: [ASP.NET MVC 5 kimlik doğrulama filtreleri](http://www.dotnetcurry.com/showarticle.aspx?ID=957), [ASP.NET MVC 5 kimlik doğrulama filtrelerini](http://theshravan.net/blog/authentication-filters-in-asp-net-mvc-5/).

### <a name="filter-overrides"></a>Filtresi geçersiz kılmaları

Artık belirterek bir belirtilen eylem yöntemini veya denetleyicileri için hangi Filtreleri Uygula kılabilirsiniz bir [filtre geçersiz kılma](http://www.davidhayden.me/blog/filter-overrides-in-asp-net-mvc-5). Geçersiz kılma filtreler bir belirtilen kapsamın (eylem veya denetleyici) çalıştırılmamalıdır filtre türleri kümesi belirtin. Bu, dünya çapında geçerli, ancak belirli eylemler veya denetleyicileri uygulanmasından bazı genel filtreleri sonra Dışla filtreleri yapılandırmanıza olanak sağlar. Bkz: [ASP.NET MVC 5 ve ASP.NET Web API 2'deki yeni filtresi geçersiz kılmaları özelliği](https://weblogs.asp.net/imranbaloch/archive/2013/09/25/new-filter-overrides-in-asp-net-mvc-5-and-asp-net-web-api-2.aspx), [ASP.NET MVC 5 filtresi geçersiz kılmaları özelliğinin nasıl kullanılacağı](http://hackwebwith.net/how-to-use-the-asp-net-mvc-5-filter-overrides-feature/), ve [filtresi geçersiz kılmaları ASP.NET MVC 5 yenilikleri](http://www.davidhayden.me/blog/filter-overrides-in-asp-net-mvc-5)

### <a name="attribute-routing"></a>Öznitelik yönlendirme

ASP.NET MVC destekler [öznitelik yönlendirme](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx), Tim McCall, yazarı tarafından bir katkı sayesinde [ http://attributerouting.net ](http://attributerouting.net). Öznitelik yönlendirme ile eylemlerin ve denetleyicilerin açıklama ekleyerek yollarınızı belirtebilirsiniz.

## <a name="new-web-project-experience"></a>Yeni Web projesi deneyimi

Visual Studio 2013'te yeni web projeleri oluşturma deneyimini geliştirdik. İçinde **yeni ASP.NET Web projesi** iletişim, istediğiniz herhangi bir birleşimini teknolojileri (Web formları, MVC, Web API'si) yapılandırma, kimlik doğrulama seçeneklerini yapılandırmak ve birim testi projesi ekleyin proje türü seçebilirsiniz.

![Yeni ASP.NET projesi](mvc5/_static/image1.png)

Yeni iletişim kutusu şablonları birçoğu için varsayılan kimlik doğrulama seçenekleri değiştirmenize olanak tanır. Örneğin, bir ASP.NET Web formları projesi oluşturduğunuzda, aşağıdaki seçeneklerden herhangi birini seçebilirsiniz:

- Kimlik doğrulama yok
- Bireysel kullanıcı hesapları (ASP.NET üyeliği veya sosyal sağlayıcılar oturum açma)
- Kurumsal hesaplar (Active Directory'de bir Internet uygulaması)
- Windows kimlik doğrulaması (Active Directory'de bir intranet uygulaması)

![Kimlik doğrulama seçenekleri](mvc5/_static/image2.png)

Web projeleri oluşturmak için yeni süreci hakkında daha fazla bilgi için bkz. [Visual Studio 2013'te ASP.NET Web projeleri oluşturma](../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md). Yeni kimlik doğrulama seçenekleri hakkında daha fazla bilgi için bkz. [ASP.NET Identity](../identity/overview/index.md).

<a id="scaffold"></a>
### <a name="aspnet-scaffolding"></a>ASP.NET iskeleti oluşturma

ASP.NET iskeleti oluşturma bir ASP.NET Web uygulamaları için kod oluşturma çerçevedir. Bu, ortak kod projenize bir veri modeli ile etkileşim eklemek kolaylaştırır.

Visual Studio'nun önceki sürümlerinde, ASP.NET MVC projeleri için yapı iskelesi sınırlıydı. Visual Studio 2013 ile Web Forms dahil olmak üzere tüm ASP.NET proje için yapı iskelesi artık kullanabilirsiniz. Visual Studio 2013'ün oluşturma sayfaları şu anda Web formları projesi desteklemiyor, ancak yapı iskelesi Web Forms ile projeye MVC bağımlılıkları ekleyerek kullanmaya devam edebilirsiniz. İçin Web formları sayfaları oluşturmak için destek, gelecek güncelleştirmelerden birinde eklenecektir.

Yapı iskelesi kullanırken, gerekli tüm bağımlılıkların projede yüklü olduğundan emin olun. Bir ASP.NET Web formları projesi ile başlayın ve ardından bir Web API denetleyicisi eklemek için yapı iskelesi kullanın, örneğin, başvurular ve gerekli NuGet paketlerini projenize otomatik olarak eklenir.

MVC yapı iskelesi Web Forms projesine eklemek için Ekle bir **yeni iskele kurulmuş öğe** seçip **MVC 5 bağımlılıkları** iletişim penceresinde. MVC yapı iskelesini oluşturmak için iki seçenek vardır; Minimal ve tam. En az seçerseniz, yalnızca NuGet paketleri ve ASP.NET MVC için başvuruları projenize eklenir. Tam seçeneği seçerseniz bir MVC projesi için gerekli içerik dosyalarının yanı sıra en az bağımlılıklar eklenir.

Zaman uyumsuz denetleyicilerinin yapı iskelesini oluşturmak için destek, Entity Framework 6 yeni zaman uyumsuz özelliklerini kullanır.

Daha fazla bilgi ve eğitimler için bkz. [ASP.NET yapı İskelesi genel bakış](../visual-studio/overview/2013/aspnet-scaffolding-overview.md).

### <a name="getting-help-and-reporting-issues"></a>Yardım alma ve raporlama sorunları

- [Bilinen sorunlar ve bozucu değişiklikler listesi](../visual-studio/overview/2013/release-notes.md#knownissues)
- Yardım alın ve ASP.NET MVC 5'te tartışın [forumları](https://forums.asp.net/1146.aspx)
- [ASP.NET MVC 5'te hata bildirin](https://github.com/aspnet/AspNetWebStack/issues)
- [Özellik isteğinde bulunmak](http://aspnet.uservoice.com/forums/41201-asp-net-mvc)

### <a name="upgrading-from-aspnet-mvc-4"></a>ASP.NET MVC 4 ' yükseltme

Bkz: [nasıl bir ASP.NET MVC 4 yükseltin ve Web API projesini ASP.NET MVC 5 ve Web API 2](overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md)
