---
uid: mvc/overview/releases/mvc51-release-notes
title: ASP.NET MVC 5.1 yenilikler | Microsoft Docs
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 9a83a058-9b01-48aa-acce-ec041e694567
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/releases/mvc51-release-notes
msc.type: authoredcontent
ms.openlocfilehash: be10486c9fd39738f44cdda4fedb409058017601
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2018
---
<a name="whats-new-in-aspnet-mvc-51"></a>ASP.NET MVC 5.1 yenilikler nelerdir?
====================
tarafından [Microsoft](https://github.com/microsoft)

Bu konuda, ASP.NET Web MVC 5.1 yenilikler açıklanmaktadır.

- [Yazılım gereksinimleri](#SoftwareRequirements)
- [İndir](#download)
- [Belgeler](#documentation)
- [ASP.NET MVC 5.1 yeni özellikler](#new-features)

    - [Yönlendirme geliştirmeleri özniteliği](#AttributeRouting)
    - [Düzenleyici şablonları önyükleme desteği](#Bootstrap)
    - [Görünümlerde enum desteği](#Enum)
    - [MinLength/MaxLength öznitelikler için örtük doğrulama](#Unobtrusive)
    - ['This' bağlam içinde örtük Ajax destekleme](#thisContext)
- [Bilinen sorunlar ve önemli değişiklikler](#KnownBreakingChanges)- [hata düzeltmeleri](#bug-fixes)

<a id="SoftwareRequirements"></a>
## <a name="software-requirements"></a>Yazılım Gereksinimleri

- Visual Studio 2012: İndirme [ASP.NET ve Web Araçları Visual Studio 2012 için 2013.1](https://go.microsoft.com/fwlink/?LinkId=390062).
- Visual Studio 2013: İndirebilirsiniz [Visual Studio 2013 güncelleştirme 1](https://go.microsoft.com/fwlink/?LinkId=390064). Bu güncelleştirme, ASP.NET MVC 5.1 Razor görünümleri düzenleme için gereklidir.

<a id="download"></a>
## <a name="download"></a>İndir

Çalışma zamanı özellikleri NuGet galerisinde NuGet paketlerini olarak yayımlanmıştır. Tüm çalışma zamanı paketleri izleyin [anlamsal sürüm oluşturma](http://semver.org/) belirtimi. En son ASP.NET MVC 5.1 RTM paketini aşağıdaki sürümü vardır: "5.1.2". Yükleme veya güncelleştirme bu paketleri yoluyla [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/). Sürüm, karşılık gelen yerelleştirilmiş NuGet paketlerini de içerir.

Yükleme veya NuGet Paket Yöneticisi konsolu kullanılarak yayımlanan NuGet paketlerini güncelleştirin:

[!code-console[Main](mvc51-release-notes/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>Belgeler

Öğreticiler ve ASP.NET MVC 5.1 RTM ilgili diğer bilgileri ASP.NET web sitesinden kullanılabilir ( https://www.asp.net). 

<a id="new-features"></a>
## <a name="new-features-in-aspnet-mvc-51"></a>ASP.NET MVC 5.1 yeni özellikler

<a id="AttributeRouting"></a>

### <a name="attribute-routing-improvements"></a>Yönlendirme geliştirmeleri özniteliği

 Öznitelik şimdi destekler kısıtlamaları, sürüm ve üstbilgi etkinleştirme yönlendirme rota seçimi temel. Öznitelik rotaları pek çok görünüşünün şimdi aracılığıyla özelleştirilebilir `IDirectRouteFactory` arabirimi ve `RouteFactoryAttribute` sınıfı. Rota öneki artık yoluyla Genişletilebilir `IRoutePrefix` arabirimi ve `RoutePrefixAttribute` sınıfı. 

<a id="Enum"></a>

### <a name="enum-support-in-views"></a>Görünümlerde enum desteği

1. Yeni `@Html.EnumDropDownListFor()` yardımcı yöntemler. Bu ifade değerlendirilmelidir uyarısıyla ile HTML Yardımcıları çoğunu gibi kullanılması gereken bir [enum](https://msdn.microsoft.com/en-us/library/cc138362.aspx) türü veya [null atanabilir&lt;T&gt; ](https://msdn.microsoft.com/en-us/library/2cf62fcy.aspx) burada `T` bir olduğu[enum](https://msdn.microsoft.com/en-us/library/cc138362.aspx) türü. Kullanım `EnumHelper.IsValidForEnumHelper()` bu gereksinimleri denetlemek için.
2. Yeni `EnumHelper.GetSelectList()` döndürmesi yöntemleri bir `IList<SelectListItem>`. Çağrılırken, örneğin, önce bir seçim listesi yönlendirmek gerektiğinde bu faydalıdır `@Html.DropDownListFor()`, veya adlarını görüntülemek istediğiniz zaman hangi `@Html.EnumDropDownListFor()` gösterir.

Aşağıdaki kod bu API'leri gösterir.

[!code-cshtml[Main](mvc51-release-notes/samples/sample2.cshtml)]

Tam bir örnek görebilirsiniz [burada](https://aspnet.codeplex.com/SourceControl/latest#Samples/MVC/EnumSample/).

<a id="Bootstrap"></a>

### <a name="bootstrap-support-for-editor-templates"></a>Düzenleyici şablonları önyükleme desteği

Şimdi geçirme olarak HTML özniteliklerindeki izin veriyoruz [EditorFor](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.editorextensions.editorfor(v=vs.100).aspx) olarak bir [anonim nesneyi](https://msdn.microsoft.com/en-us/library/bb397696.aspx).

Örneğin:

[!code-cshtml[Main](mvc51-release-notes/samples/sample3.cshtml)]

<a id="Unobtrusive"></a>

### <a name="unobtrusive-validation-for-minlengthattribute-and-maxlengthattribute"></a>MinLengthAttribute ve MaxLengthAttribute için örtük doğrulama

İstemci tarafı doğrulama dize ve dizi türleri için artık özellikleri ile donatılmış için desteklenecektir [MinLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.minlengthattribute(v=vs.110).aspx) ve [MaxLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.maxlengthattribute(v=vs.110).aspx) öznitelikleri.

<a id="thisContext"></a>

### <a name="supporting-the-this-context-in-unobtrusive-ajax"></a>'This' bağlam içinde örtük Ajax destekleme

Geri arama işlevleri (`OnBegin, OnComplete, OnFailure, OnSuccess`) şimdi aracılığıyla bildirilecekse öğesini bulun kuramaz `this` bağlamı. Örneğin:

[!code-html[Main](mvc51-release-notes/samples/sample4.html)]

<a id="KnownBreakingChanges"></a>

## <a name="known-issues-and-breaking-changes"></a>Bilinen sorunlar ve yeni değişiklikler

### <a name="attribute-routing"></a>Öznitelik yönlendirme

Öznitelik yönlendirme eşleşmeleri belirsizlikleri şimdi ilk eşleşmeye seçmek yerine bir hata bildirir.

Öznitelik rotaları yasaklanmış kullanarak `{controller}` parametresini kullanarak gelen ve giden `{action}` rota parametresi yerleştirilen eylemleri. Bu parametreler kullanımları, büyük olasılıkla belirsizlikleri için yol açar. 

### <a name="scaffolding-mvcweb-api-into-a-project-with-51-packages-results-in-50-packages-for-ones-that-dont-already-exist-in-the-project"></a>Yapı iskelesi MVC/Web API projesine projede zaten mevcut olanları 5.0 paketleri 5.1 paketleri sonuçlarında ile

ASP.NET MVC 5.1 RTM için NuGet paketlerini güncelleştirme ASP.NET yapı iskelesi gibi Visual Studio araçlarını veya ASP.NET Web uygulaması proje şablonu güncelleştirmez. ASP.NET çalışma zamanı paketleri (5.0.0.0)'ın önceki sürümünü kullanırlar. Sonuç olarak, bunlar zaten projelerinizde yoksa ASP.NET yapı iskelesi gerekli paketleri, önceki sürümü (5.0.0.0) yükleyin. Ancak, Visual Studio 2013 RTM veya güncelleştirme 1 ASP.NET iskele projelerinizi son paketlerinde üzerine yazmaz. Web API 2.1 veya ASP.NET MVC 5.1 projelerinizi paketleri güncelleştirdikten sonra ASP.NET yapı iskelesi kullanırsanız, Web API ve ASP.NET MVC sürümleri tutarlı olduğundan emin olun. 

### <a name="syntax-highlighting-for-razor-views-in-visual-studio-2013"></a>Visual Studio 2013'te Razor görünümleri için söz dizimi vurgulama

Visual Studio 2013 güncelleştirmeden ASP.NET MVC 5.1 RTM için'ı güncelleştirirseniz, Visual Studio Düzenleyicisi desteği Razor görünümleri düzenlerken, sözdizimi vurgulama için almazsınız. Bu destek almak için Visual Studio 2013 güncelleştirmeniz gerekir. 

### <a name="type-renames"></a>Türü yeniden adlandırma

Bazı öznitelik yönlendirme genişletilebilirliği için kullanılan türleri 5.1 RTM adlandırılır.

| **Eski tür adı (5.1 RC)** | **Yeni tür adı (5.1 RTM)** |
| --- | --- |
| IDirectRouteProvider | IDirectRouteFactory |
| RouteProviderAttribute | RouteFactoryAttribute |
| DirectRouteProviderContext | DirectRouteFactoryContext |

<a id="bug-fixes"></a>
## <a name="bug-fixes"></a>Hata Düzeltmeleri

Bu sürüm çeşitli hata düzeltmeleri de içerir. Tam listesini burada bulabilirsiniz:

- [5.1.0 package](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.1%20Preview|v5.1%20RTM&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)
- [5.1.1 paketi](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.1.1%20RTM&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)

5.1.2 paket IntelliSense güncelleştirmeler ancak hiçbir hata düzeltmeleri içerir.
