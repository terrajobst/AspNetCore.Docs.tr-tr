---
uid: mvc/overview/releases/whats-new-in-aspnet-mvc-52
title: ASP.NET MVC 5.2 yenilikler | Microsoft Docs
author: microsoft
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/25/2014
ms.topic: article
ms.assetid: 97972587-2720-48b4-b158-f35f2e855fbf
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/releases/whats-new-in-aspnet-mvc-52
msc.type: authoredcontent
ms.openlocfilehash: 94f6131fdb86d1694c1f563c5f6806f119c71266
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="whats-new-in-aspnet-mvc-52"></a>ASP.NET MVC 5.2 yenilikler nelerdir?
====================
tarafından [Microsoft](https://github.com/microsoft)

Bu konuda, ASP.NET MVC 5.2 için Yenilikler açıklanmaktadır [Microsoft.AspNet.MVC 5.2.2](#52) ve [ASP.NET MVC 5.2.3 Beta](#mvc523Beta)

- [Yazılım gereksinimleri](#softRequire)
- [İndir](#download)
- [Belgeler](#documentation)
- [ASP.NET MVC 5.2 yeni özellikler](#new-features)

    - [Yönlendirme geliştirmeleri özniteliği](#attributerouting)
- [Bilinen sorunlar ve yeni değişiklikler](#knownbreakingchanges)
- [Hata düzeltmeleri](#bug-fixes)
- [Microsoft.AspNet.MVC 5.2.2](#52)

<a id="softRequire"></a>
## <a name="software-requirements"></a>Yazılım Gereksinimleri

- Visual Studio 2012: İndirme [ASP.NET ve Web Araçları Visual Studio 2012 için 2013.1](https://go.microsoft.com/fwlink/?LinkId=390062).
- Visual Studio 2013: İndirebilirsiniz [Visual Studio 2013 güncelleştirme](https://go.microsoft.com/fwlink/?LinkId=390064) ya da daha yüksek. Bu güncelleştirme, ASP.NET MVC 5.2 Razor görünümleri düzenleme için gereklidir.

<a id="download"></a>
## <a name="download"></a>İndir

Çalışma zamanı özellikleri NuGet galerisinde NuGet paketlerini olarak yayımlanmıştır. Tüm çalışma zamanı paketleri izleyin [anlamsal sürüm oluşturma](http://semver.org/) belirtimi. En son ASP.NET MVC 5.2 paketini aşağıdaki sürümü vardır: "5.2.0". Yükleme veya güncelleştirme bu paketleri yoluyla [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/). Sürüm, karşılık gelen yerelleştirilmiş NuGet paketlerini de içerir.

Yükleme veya NuGet Paket Yöneticisi konsolu kullanılarak yayımlanan NuGet paketlerini güncelleştirin:

Install-Package Microsoft.AspNet.Mvc-sürüm 5.2.0

<a id="documentation"></a>
## <a name="documentation"></a>Belgeler

Öğreticiler ve ASP.NET MVC 5.2 ilgili diğer bilgileri ASP.NET web sitesinden kullanılabilir ([https://www.asp.net/mvc](../../index.md)).

<a id="new-features"></a>
## <a name="new-features-in-aspnet-mvc-52"></a>ASP.NET MVC 5.2 yeni özellikler

<a id="attributerouting"></a>
### <a name="attribute-routing-improvements"></a>Yönlendirme geliştirmeleri özniteliği

Öznitelik şimdi yönlendirme öznitelik rotaları nasıl bulunan ve yapılandırılmış üzerinde tam denetim verir IDirectRouteProvider adlı bir genişletilebilirlik noktası sağlar. Bir IDirectRouteProvider eylemlerin ve denetleyicilerin tam olarak hangi yönlendirme yapılandırması için bu eylemleri istendiğini belirtmek için ilişkili rota bilgilerle birlikte listesini sağlamaktan sorumludur. IDirectRouteProvider uygulaması MapAttributes/MapHttpAttributeRoutes çağrılırken belirtilebilir.

IDirectRouteProvider özelleştirme DefaultDirectRouteProvider bizim varsayılan uygulaması genişleterek kolay olacaktır. Bu sınıf öznitelikleri bulma, rota girişleri oluşturmak ve rota öneki ve alan öneki keşfetmek için mantığı değiştirmek için ayrı geçersiz kılınabilir sanal yöntemleri sağlar.

Yeni öznitelik yönlendirme genişletilmesinde IDirectRouteProvider ile bir kullanıcı aşağıdakileri yapabilirsiniz:

1. Öznitelik rotaları devralma destekler. Örneğin, aşağıdaki senaryoda Blog ve depolama denetleyicileri BaseController tarafından tanımlanan bir özniteliği yol kuralı kullanıyor. 

    [!code-csharp[Main](whats-new-in-aspnet-mvc-52/samples/sample1.cs)]
2. Öznitelik rotaları için rota adlarını otomatik olarak üret. 

    [!code-csharp[Main](whats-new-in-aspnet-mvc-52/samples/sample2.cs)]
3. Yolları yönlendirme tablosuna eklenir önce tek bir merkezi konumda rota öneklerini değiştirin.
4. Aranacak öznitelik yönlendirme istediğiniz denetleyicileri filtreler. 3 ve 4 blog'a yakında umuyoruz.

### <a name="facebook-fixes-for-changed-api-surface"></a>Facebook için değiştirilmiş API yüzeyi giderir

MVC Facebook paket [kesildi](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.2%20RC&amp;assignedTo=All&amp;component=Facebook&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=All) Facebook tarafından yapılan birkaç API değişiklikler nedeniyle. Ayrıca yeni bir Facebook paket (Bu sorunları düzeltmek için Microsoft.AspNet.Facebook 1.0.0) sunacağımız.

<a id="knownbreakingchanges"></a>
## <a name="known-issues-and-breaking-changes"></a>Bilinen sorunlar ve yeni değişiklikler

### <a name="scaffolding-mvcweb-api-into-a-project-with-520-packages-results-in-512-packages-for-ones-that-dont-already-exist-in-the-project"></a>Yapı iskelesi MVC/Web API projesine 5.2.0 ile 5.1.2 paketleri sonuçlarında paketler için olanları projede zaten mevcut değil

ASP.NET MVC 5.2.0 NuGet paketlerini güncelleştirme ASP.NET yapı iskelesi gibi Visual Studio araçlarını veya ASP.NET Web uygulaması proje şablonu güncelleştirmez. ASP.NET çalışma zamanı paketleri (örneğin 5.1.2 güncelleştirme 2 ')'ın önceki sürümünü kullanırlar. Sonuç olarak, bunlar zaten projelerinizde yoksa ASP.NET yapı iskelesi gerekli paketleri, önceki sürümünü (örneğin güncelleştirme 2'deki 5.1.2) yükleyin. Ancak, Visual Studio 2013 RTM veya güncelleştirme 1 ASP.NET iskele projelerinizi son paketlerinde üzerine yazmaz. Web API 2.2 veya ASP.NET MVC 5.2 projelerinizi paketleri güncelleştirdikten sonra ASP.NET yapı iskelesi kullanırsanız, Web API ve ASP.NET MVC sürümleri tutarlı olduğundan emin olun.

### <a name="microsoftjqueryunobtrusivevalidation-nuget-package-installation-fails-because-it-is-unable-to-find-a-version-of-microsoftjqueryunobtrusivevalidation-compatible-to-jquery-141"></a>Microsoft.jQuery.Unobtrusive.Validation sürümü jQuery 1.4.1 uyumlu bulamıyor olduğundan Microsoft.jQuery.Unobtrusive.Validation NuGet paketi yüklemesi başarısız

Microsoft.jQuery.Unobtrusive.Validation gerektirir jQuery &gt;1.8 ve jQuery.Validation = &gt;1.8 =. But,JQuery.validation (1.8) jQuery gerekir (&#8805; 1.3.2 &amp; &amp; &#8804; 1.6). Aynı anda NuGet jQuery.Validation 1.8 ve JQuery 1.8 yüklediğinde, bu nedenle, başarısız olur. Bu sorun gördüğünüzde, yalnızca jQuery.Validation için sürümünü güncelleştirebilir &gt; =  [1.8.0.1](https://www.nuget.org/packages/jQuery.Validation/1.8.0.1) sabit jQuery cap olan ilk olarak, yüklemeniz gerekir Microsoft.jQuery.Unobtrusive.Validation.

### <a name="the-jqueryvalidation-nuget-package-version-1130-does-not-recognize-some-international-email-addresses"></a>Jquery. Doğrulama nuget Paket sürümü 1.13.0 bazı uluslararası e-posta adresleri tanımıyor

jQuery.Validation nuget Paketi 1.11.1 geçerli e-posta adresleri aşağıdaki tanıdığı en son bilinen sürüm sürümüdür. Sonraki sürümlerle tanıması mümkün olmayabilir. Örneğin:

E-posta adresi uluslararası hale getirme (EAI) standardını (örn., [&#29992; &#25143;@domain.com ](mailto:&#29992;&#25143;@domain.com))   
 EAI + Internationalized kaynak tanımlayıcıları (IRIS) (örn., [&#29992; &#25143; @&#1076; &#1086; &#1084; &#1077; &#1085;. &#1088; &#1092; ](mailto:&#29992;&#25143;@&#1076;&#1086;&#1084;&#1077;&#1085;.&#1088;&#1092;))

Konumundaki sorunu bildiren [https://github.com/jzaefferer/jquery-validation/issues/1222](https://github.com/jzaefferer/jquery-validation/issues/1222)

### <a name="syntax-highlighting-for-razor-views-in-visual-studio-2013"></a>Visual Studio 2013'te Razor görünümleri için söz dizimi vurgulama

ASP.NET MVC 5.2 için Visual Studio 2013 güncelleştirmeden güncelleştirirseniz, Visual Studio Düzenleyicisi desteği Razor görünümleri düzenlerken, sözdizimi vurgulama için almazsınız. Bu destek almak için Visual Studio 2013 güncelleştirmeniz gerekir.

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a>Hata düzeltmeleri ve küçük özellik güncelleştirmeleri

Bu sürüm çeşitli hata düzeltmeleri ve küçük özellik içerir güncelleştirmeler. Tam listesini burada bulabilirsiniz:

- [5.2 paketi](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2%20RC&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)

<a id="52"></a>
## <a name="microsoftaspnetmvc-522"></a>Microsoft.AspNet.MVC 5.2.2

Bu sürümde herhangi bir yeni özellikler veya hata düzeltmeleri MVC'de sahip değil. Biz yapılan bir [Web sayfalarında değiştirmek](https://blogs.msdn.com/b/webdev/archive/2014/07/28/announcing-the-beta-release-of-web-pages-3-2-1.aspx) önemli performans iyileştirme ve size ait bu Web sayfaları yeni sürümü bağımlı diğer tüm bağımlı paketler sonradan güncelleştirdiniz.

<a id="mvc523Beta"></a>
## <a name="aspnet-mvc-523-beta"></a>ASP.NET MVC 5.2.3 Beta

Yayın hakkında bilgi edinebilirsiniz [burada](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx). Bu sürüm yalnızca hata düzeltmeleri içerir. Kullanabileceğiniz [bu sorguyu](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=MVC&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) bu sürümde giderilen sorunlar listesini görmek için.
