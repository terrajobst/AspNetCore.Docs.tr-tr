---
uid: mvc/mvc5
title: ASP.NET MVC 5 | Microsoft Docs
author: rick-anderson
description: "ASP.NET MVC 5 ASP.NET MVC 5 tanınmış tasarım desenleri ve AS. gücünü kullanarak ölçeklenebilir, standartlara dayalı web uygulamaları oluşturmaya yönelik bir çerçevedir..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: f79fbf7f-59e5-4279-a832-c1a0294630f4
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/mvc5
msc.type: content
ms.openlocfilehash: e57163469ae4606df0fc17e3e054b7696782a084
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-5"></a>ASP.NET MVC 5
====================
## <a name="whats-new-in-aspnet-mvc-5"></a>ASP.NET MVC 5 yenilikler nelerdir?

### <a name="one-aspnet"></a>Bir ASP.NET

Web MVC proje şablonları ile yeni bir ASP.NET deneyimi sorunsuz şekilde tümleşir. MVC projenizi özelleştirebilir ve bir ASP.NET proje oluşturma Sihirbazı'nı kullanarak kimlik doğrulaması yapılandırın. Bir tanıtım öğretici ASP.NET MVC 5 bulunabilir [ASP.NET MVC 5 ile çalışmaya başlama](overview/getting-started/introduction/getting-started.md).

MVC 4 projeleri MVC 5'e yükseltme hakkında daha fazla bilgi için bkz: [bir ASP.NET MVC 4 ve Web API projesi ASP.NET MVC 5 ve Web API 2'ye yükseltme](overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md).

### <a name="aspnet-identity"></a>ASP.NET kimliği

MVC proje şablonları, ASP.NET Identity kimlik doğrulama ve kimlik yönetimi için kullanmak üzere güncelleştirilmiştir. Facebook ve Google kimlik doğrulama ve yeni üyelik API'si gösteren bir öğretici bulunabilir [Facebook ve Google OAuth2 ve Openıd oturum açma ile bir ASP.NET MVC 5 uygulaması oluşturmak](overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) ve [bir Güvenli ASP.NET MVC uygulaması ile dağıtma Üyelik, OAuth ve bir Windows Azure Web sitesi için SQL veritabanı](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).

### <a name="bootstrap"></a>önyükleme

MVC proje şablonu kullanmak için güncelleştirilmiş [önyükleme](http://getbootstrap.com/) bir şık ve esnek kolayca özelleştirebileceğiniz görünüm sağlamak için. Daha fazla bilgi için bkz: [Visual Studio 2013 web projesi şablonları önyükleme](../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap).

### <a name="authentication-filters"></a>Kimlik doğrulaması filtreleri

[Kimlik doğrulaması filtreleri](http://www.dotnetcurry.com/showarticle.aspx?ID=957) yeni bir ASP.NET MVC ardışık düzenine yetkilendirme filtreleri önce çalıştıran ve kimlik doğrulama mantığı başına-eylemi belirtmenize olanak veren ASP.NET MVC filtre tür başına denetleyicisi, veya genel olarak tüm denetleyicileri için. Kimlik doğrulaması filtreleri kimlik bilgileri isteği işlemek ve karşılık gelen asıl sağlayın. Kimlik doğrulaması filtreleri, kimlik doğrulama sınaması yetkisiz isteklerine yanıt olarak da ekleyebilirsiniz. Bkz: [ASP.NET MVC 5 kimlik doğrulaması filtreleri](http://www.dotnetcurry.com/showarticle.aspx?ID=957), [ASP.NET MVC 5 kimlik doğrulama filtrelerini](http://theshravan.net/blog/authentication-filters-in-asp-net-mvc-5/) ve [son olarak yeni ASP.NET MVC 5 kimlik doğrulama filtrelerini!](http://hackwebwith.net/finally-the-new-asp-net-mvc-5-authentication-filters/).

### <a name="filter-overrides"></a>Filtre geçersiz kılar

Şimdi belirterek belirli bir eylem yöntemini veya denetleyicileri için hangi filtre uygulamak kılabilirsiniz bir [filtre geçersiz kılma](http://www.davidhayden.me/blog/filter-overrides-in-asp-net-mvc-5). Geçersiz kılma filtreleri verilen kapsam (eylem veya denetleyici) için çalıştırılmamalıdır filtre türleri kümesi belirtin. Bu genel olarak geçerli ancak ardından belirli eylemler veya denetleyicileri uygulama bazı genel filtreleri hariç filtreleri yapılandırmanıza olanak sağlar. Bkz: [ASP.NET MVC 5 ve ASP.NET Web API 2'deki yeni filtre geçersiz kılmaları özelliği](https://weblogs.asp.net/imranbaloch/archive/2013/09/25/new-filter-overrides-in-asp-net-mvc-5-and-asp-net-web-api-2.aspx), [ASP.NET MVC 5 filtresi geçersiz kılmaları özelliğinin nasıl kullanılacağı](http://hackwebwith.net/how-to-use-the-asp-net-mvc-5-filter-overrides-feature/), ve [filtresi geçersiz kılmaları ASP.NET MVC 5](http://www.davidhayden.me/blog/filter-overrides-in-asp-net-mvc-5)

### <a name="attribute-routing"></a>Öznitelik yönlendirme

ASP.NET MVC destekler [özniteliği yönlendirme](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx), Tim McCall, yazarı tarafından katkı sayesinde [http://attributerouting.net](http://attributerouting.net). Öznitelik yönlendirme ile eylemlerin ve denetleyicilerin yorumlama yollarınızı belirtebilirsiniz.

## <a name="new-web-project-experience"></a>Yeni Web projesi deneyimi

Biz, Visual Studio 2013'te yeni web projeleri oluşturma deneyimi Gelişmiş. İçinde **yeni ASP.NET Web projesi** iletişim, istediğiniz, herhangi bir bileşimini teknolojileri (Web Forms, MVC, Web API) yapılandırma, kimlik doğrulama seçeneklerini yapılandırmak ve birim testi projesi eklemek proje türü seçebilirsiniz.

![Yeni ASP.NET projesi](mvc5/_static/image1.png)

Yeni iletişim kutusu şablonları birçoğu için varsayılan kimlik doğrulama seçenekleri değiştirmenize olanak tanır. Örneğin, bir ASP.NET Web Forms projesi oluşturduğunuzda, aşağıdaki seçeneklerden birini seçebilirsiniz:

- Kimlik doğrulaması yok
- Bireysel kullanıcı hesapları (ASP.NET üyelik veya sosyal sağlayıcısı günlüğüne)
- Kurumsal hesaplar (Internet uygulamasını Active Directory'de)
- Windows kimlik doğrulaması (Active Directory içinde bir intranet uygulaması)

![Kimlik doğrulama seçenekleri](mvc5/_static/image2.png)

Web projeleri oluşturmak için yeni işlem hakkında daha fazla bilgi için bkz: [Visual Studio 2013'da ASP.NET Web projeleri oluşturma](../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md). Yeni kimlik doğrulama seçenekleri hakkında daha fazla bilgi için bkz: [ASP.NET Identity](../identity/overview/index.md).

<a id="scaffold"></a>
### <a name="aspnet-scaffolding"></a>ASP.NET İskele

ASP.NET İskele bir ASP.NET Web uygulamaları için kod oluşturma çerçevedir. Bir veri modeli ile etkileşime giren projeniz Demirbaş kod eklemek kolaylaştırır.

Visual Studio'nun önceki sürümleri yapı iskelesi ASP.NET MVC projelerine sınırlı. Visual Studio 2013 ile Web Forms dahil olmak üzere tüm ASP.NET projesi için yapı iskelesi artık kullanabilirsiniz. Visual Studio 2013 oluşturma sayfaları şu anda Web Forms projesi için desteklemiyor, ancak yapı iskelesi ile Web Forms projeye MVC bağımlılıkları ekleyerek kullanmaya devam edebilirsiniz. İçin Web formları sayfaları oluşturmaya yönelik destek gelecek bir güncelleştirmede eklenir.

Yapı iskelesi kullanırken, tüm gerekli bağımlılıkların projede yüklü olduğundan emin olun. Bir ASP.NET Web Forms projeyle başlatın ve sonra bir Web API denetleyicisi eklemek için yapı iskelesi kullanın, örneğin, gerekli NuGet paket ve başvuruları projenize otomatik olarak eklenir.

Bir Web Forms projeye MVC yapı iskelesi eklemek için Ekle bir **yeni iskele kurulmuş öğe** seçip **MVC 5 bağımlılıkları** iletişim penceresinde. MVC iskele için iki seçenek vardır; Minimal ve tam. En az seçerseniz, yalnızca NuGet paketlerini ve ASP.NET MVC için başvuruları projenize eklenir. Tam seçeneğini belirlerseniz MVC projesinde gerekli içerik dosyaların yanı sıra en az bağımlılıkları eklenir.

Zaman uyumsuz denetleyicileri iskele desteği yeni Entity Framework 6 zaman uyumsuz özelliklerini kullanır.

Daha fazla bilgi ve öğreticiler için bkz: [ASP.NET yapı İskelesi genel bakış](../visual-studio/overview/2013/aspnet-scaffolding-overview.md).

### <a name="getting-help-and-reporting-issues"></a>Yardım alma ve sorunları raporlama

- [Bilinen sorunlar ve en son değişikliklerin listesi](../visual-studio/overview/2013/release-notes.md#knownissues)
- Yardım alın ve ASP.NET MVC 5'te ele [forumları](https://forums.asp.net/1146.aspx)
- [ASP.NET MVC 5 bir hata raporu](https://github.com/aspnet/AspNetWebStack/issues)
- [Özellik isteği oluşturma](http://aspnet.uservoice.com/forums/41201-asp-net-mvc)

### <a name="upgrading-from-aspnet-mvc-4"></a>ASP.NET MVC 4 ' yükseltme

Bkz: [nasıl bir ASP.NET MVC 4 yükseltme ve Web API projesi ASP.NET MVC 5 ve Web API 2](overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md)
