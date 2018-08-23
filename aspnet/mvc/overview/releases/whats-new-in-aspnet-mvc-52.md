---
uid: mvc/overview/releases/whats-new-in-aspnet-mvc-52
title: ASP.NET MVC 5.2 sürümündeki yenilikler | Microsoft Docs
author: microsoft
description: ''
ms.author: riande
ms.date: 12/25/2014
ms.assetid: 97972587-2720-48b4-b158-f35f2e855fbf
msc.legacyurl: /mvc/overview/releases/whats-new-in-aspnet-mvc-52
msc.type: authoredcontent
ms.openlocfilehash: 8c3c5de55396635d2e7f2b7726f54be1c06bb691
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41751941"
---
<a name="whats-new-in-aspnet-mvc-52"></a>ASP.NET MVC 5.2 sürümündeki yenilikler
====================
tarafından [Microsoft](https://github.com/microsoft)

Bu konuda, ASP.NET MVC 5.2 için Yenilikler açıklanır [Microsoft.AspNet.MVC 5.2.2](#52) ve [ASP.NET MVC 5.2.3 Beta](#mvc523Beta)

- [Yazılım gereksinimleri](#softRequire)
- [İndir](#download)
- [Belgeler](#documentation)
- [ASP.NET MVC 5.2 sürümündeki yenilikler](#new-features)

    - [Öznitelik yönlendirme geliştirmeleri](#attributerouting)
- [Bilinen sorunlar ve yeni değişiklikler](#knownbreakingchanges)
- [Hata düzeltmeleri](#bug-fixes)
- [Microsoft.AspNet.MVC 5.2.2](#52)

<a id="softRequire"></a>
## <a name="software-requirements"></a>Yazılım Gereksinimleri

- Visual Studio 2012: İndirme [ASP.NET ve Web Araçları 2013.1 Visual Studio 2012 için](https://go.microsoft.com/fwlink/?LinkId=390062).
- Visual Studio 2013'ü: İndirin [Visual Studio 2013 güncelleştirme](https://go.microsoft.com/fwlink/?LinkId=390064) veya üzeri. Bu güncelleştirme, ASP.NET MVC 5.2 Razor görünümleri düzenleme için gereklidir.

<a id="download"></a>
## <a name="download"></a>İndir

Çalışma zamanı özellikleri, NuGet galerisindeki NuGet paketleri olarak kullanıma sunulur. Tüm çalışma zamanı paketlerini izleyin [Semantic Versioning](http://semver.org/) belirtimi. ASP.NET MVC 5.2 paketin en son sürümü var: "5.2.0". Yükleme veya güncelleştirme yoluyla bu paketleri [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/). Sürüm, karşılık gelen yerelleştirilmiş NuGet paketleri de içerir.

NuGet Paket Yöneticisi konsolu kullanarak için kullanıma sunulan NuGet paketlerini güncelleştirin ya da yükleyin:

Install-Package Microsoft.AspNet.Mvc-sürüm 5.2.0

<a id="documentation"></a>
## <a name="documentation"></a>Belgeler

Öğreticiler ve diğer bilgileri ASP.NET MVC 5.2 ASP.NET web sitesinden kullanılabilir ([https://www.asp.net/mvc](../../index.md)).

<a id="new-features"></a>
## <a name="new-features-in-aspnet-mvc-52"></a>ASP.NET MVC 5.2 sürümündeki yenilikler

<a id="attributerouting"></a>
### <a name="attribute-routing-improvements"></a>Öznitelik yönlendirme geliştirmeleri

Öznitelik şimdi yönlendirme öznitelik rotaları nasıl bulunan ve yapılandırılmış üzerinde tam denetim sağlayan IDirectRouteProvider adlı bir genişletilebilirlik noktası sağlar. Bir IDirectRouteProvider listesini eylemlerin ve denetleyicilerin birlikte tam olarak hangi yönlendirme yapılandırması için bu eylemlerin istendiğini belirtmek için ilişkili rota bilgilerini sağlamaktan sorumludur. IDirectRouteProvider uygulaması MapAttributes/MapHttpAttributeRoutes çağırırken belirtilebilir.

IDirectRouteProvider özelleştirme varsayılan kararlılığımızın DefaultDirectRouteProvider genişleterek kolay olacaktır. Bu sınıf öznitelikleri bulmak için rota girişleri oluşturup rota öneki ve alan öneki bulunması için mantığı değiştirmek için ayrı geçersiz kılınabilir sanal yöntemleri sağlar.

Yeni öznitelik yönlendirme genişletilmesinde IDirectRouteProvider ile bir kullanıcı aşağıdakileri yapabilirsiniz:

1. Öznitelik rotaları devralınmasını destekler. Örneğin, aşağıdaki senaryoda, Blog ve Store denetleyicileri BaseController tarafından tanımlanan bir öznitelik yönlendirme kuralı kullanıyor. 

    [!code-csharp[Main](whats-new-in-aspnet-mvc-52/samples/sample1.cs)]
2. Öznitelik rotaları için rota adlarını otomatik olarak oluşturur. 

    [!code-csharp[Main](whats-new-in-aspnet-mvc-52/samples/sample2.cs)]
3. Rotaları için rota tablosu eklenin önce tek bir merkezden yol ön ekleri değiştirin.
4. Aranacak öznitelik yönlendirme istediğiniz denetleyicileri filtreleyin. 3 ve 4 Bloga yakında umuyoruz.

### <a name="facebook-fixes-for-changed-api-surface"></a>Facebook için değiştirilmiş bir API yüzeyi giderir.

MVC Facebook paket [kesildi](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.2%20RC&amp;assignedTo=All&amp;component=Facebook&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=All) Facebook tarafından yapılan birkaç API değişiklikleri nedeniyle. Yeni Facebook paketi (Bu sorunları Microsoft.AspNet.Facebook 1.0.0) aynı zamanda sunacağımız.

<a id="knownbreakingchanges"></a>
## <a name="known-issues-and-breaking-changes"></a>Bilinen sorunlar ve yeni değişiklikler

### <a name="scaffolding-mvcweb-api-into-a-project-with-520-packages-results-in-512-packages-for-ones-that-dont-already-exist-in-the-project"></a>Yapı iskelesi MVC/Web API projesine 5.2.0 ile 5.1.2 paketleri sonuçlarında paketleri için projede zaten mevcut olmayan hizmetlerdir

ASP.NET MVC 5.2.0 için NuGet paketlerini güncelleştirme, Visual Studio Araçları gibi ASP.NET iskeleti oluşturma veya ASP.NET Web uygulaması proje şablonu güncelleştirmez. ASP.NET çalışma zamanı paketlerini (örneğin 5.1.2 güncelleştirme 2 ')'ın önceki sürümünü kullanırlar. Sonuç olarak, zaten projelerinizde kullanılabilir yoksa ASP.NET iskeleti oluşturma önceki sürümünü (örneğin 5.1.2 güncelleştirme 2) gerekli paketleri yükleyin. Ancak, en son paketlerin projelerinizde te ASP.NET iskeleti oluşturma veya güncelleştirme 1'Visual Studio 2013 RTM üzerine yazılmaz. Sonra Web API 2.2 veya ASP.NET MVC 5.2 projelerinizin paketlerin güncelleştirilmesi, ASP.NET iskeleti oluşturma kullanırsanız, Web API ve ASP.NET MVC sürümleri tutarlı olduğundan emin olun.

### <a name="microsoftjqueryunobtrusivevalidation-nuget-package-installation-fails-because-it-is-unable-to-find-a-version-of-microsoftjqueryunobtrusivevalidation-compatible-to-jquery-141"></a>Jquery'e 1.4.1 uyumlu Microsoft.jQuery.Unobtrusive.Validation sürümünü bulamıyor olduğundan Microsoft.jQuery.Unobtrusive.Validation NuGet paket yüklemesi başarısız olur.

JQuery Microsoft.jQuery.Unobtrusive.Validation gerektirir &gt;1.8 ve jQuery.Validation = &gt;1.8 =. JQuery but,JQuery.validation (1.8) gerekiyor (&#8805; 1.3.2 &amp; &amp; &#8804; 1.6). Aynı zamanda, NuGet jQuery.Validation 1.8 ve JQuery 1.8 yüklediğinde, bu nedenle, başarısız olur. Bu sorun gördüğünüzde, yalnızca için jQuery.Validation sürümünü güncelleştirebilir &gt; =  [1.8.0.1](https://www.nuget.org/packages/jQuery.Validation/1.8.0.1) sabit jQuery büyük harf olan ilk olarak, yükleyebilmesi için o olmalıdır Microsoft.jQuery.Unobtrusive.Validation.

### <a name="the-jqueryvalidation-nuget-package-version-1130-does-not-recognize-some-international-email-addresses"></a>Jquery. Bazı uluslararası e-posta adresi doğrulama nuget paketi sürüm 1.13.0 tanımıyor

jQuery.Validation nuget paketi sürüm 1.11.1, geçerli bir e-posta adreslerini takip eder son bilinen sürümüdür. Sonraki tüm sürümlerinde tanıması mümkün olmayabilir. Örneğin:

E-posta adresi uluslararası duruma getirme (EAI) standardını (örneğin, [ &#29992; &#25143; @domain.com ](mailto:&#29992;&#25143;@domain.com))   
 EAI + Internationalized kaynak tanımlayıcıları (Iris) (örn., [ &#29992; &#25143;@&#1076;&#1086;&#1084;&#1077;&#1085;.&#1088; &#1092;](mailto:&#29992;&#25143;@&#1076;&#1086;&#1084;&#1077;&#1085;.&#1088;&#1092;))

Sorun, bildirilir [https://github.com/jzaefferer/jquery-validation/issues/1222](https://github.com/jzaefferer/jquery-validation/issues/1222)

### <a name="syntax-highlighting-for-razor-views-in-visual-studio-2013"></a>Visual Studio 2013'te Razor görünümleri için söz dizimi vurgulama

ASP.NET MVC 5.2 için Visual Studio 2013 güncelleştirmeden güncelleştirirseniz, Visual Studio Düzenleyicisi desteği Razor görünümleri düzenlenirken söz dizimi vurgulama için almazsınız. Bu destek almak için Visual Studio 2013 güncelleştirmeniz gerekir.

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a>Hata düzeltmeleri ve alt özellik güncelleştirmeleri

Bu sürüm çeşitli hata düzeltmeleri ve küçük özelliği içerir güncelleştirmeleri. Tam listesini burada bulabilirsiniz:

- [5.2 paket](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2%20RC&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)

<a id="52"></a>
## <a name="microsoftaspnetmvc-522"></a>Microsoft.AspNet.MVC 5.2.2

Bu sürüm, MVC'de herhangi bir yeni özellik veya hata düzeltmesi içermez. Yaptık bir [Web sayfaları'nda değiştirmek](https://blogs.msdn.com/b/webdev/archive/2014/07/28/announcing-the-beta-release-of-web-pages-3-2-1.aspx) önemli bir performans geliştirmesi için ve sonrasında Web sayfaları'nın bu yeni sürümüne bağımlı olduğumuz diğer tüm bağlı paketleri güncelleştirdik.

<a id="mvc523Beta"></a>
## <a name="aspnet-mvc-523-beta"></a>ASP.NET MVC 5.2.3 Beta

Yayın hakkında bilgi edinebilirsiniz [burada](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx). Bu sürüm yalnızca hata düzeltmeleri içerir. Kullanabileceğiniz [bu sorgu](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=MVC&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) bu sürümde giderilen sorunlar listesinde görmek için.
