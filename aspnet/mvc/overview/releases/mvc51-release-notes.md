---
uid: mvc/overview/releases/mvc51-release-notes
title: ASP.NET MVC 5.1 sürümündeki yenilikler | Microsoft Docs
author: microsoft
description: ''
ms.author: aspnetcontent
ms.date: 02/27/2014
ms.assetid: 9a83a058-9b01-48aa-acce-ec041e694567
msc.legacyurl: /mvc/overview/releases/mvc51-release-notes
msc.type: authoredcontent
ms.openlocfilehash: d2e67f64e725e73c3bf9021295da3fe870079a45
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37825654"
---
<a name="whats-new-in-aspnet-mvc-51"></a>ASP.NET MVC 5.1 sürümündeki yenilikler
====================
tarafından [Microsoft](https://github.com/microsoft)

Bu konuda, ASP.NET Web MVC 5.1 yenilikler açıklanmaktadır.

- [Yazılım gereksinimleri](#SoftwareRequirements)
- [İndir](#download)
- [Belgeler](#documentation)
- [ASP.NET MVC 5.1 yeni özellikler](#new-features)

    - [Öznitelik yönlendirme geliştirmeleri](#AttributeRouting)
    - [Düzenleyici şablonları önyükleme desteği](#Bootstrap)
    - [Görünümlerde enum desteği](#Enum)
    - [MinLength/MaxLength öznitelikler için örtük doğrulama](#Unobtrusive)
    - ['This' bağlamı içinde örtük Ajax destekleme](#thisContext)
- [Bilinen sorunlar ve önemli değişiklikler](#KnownBreakingChanges)- [hata düzeltmeleri](#bug-fixes)

<a id="SoftwareRequirements"></a>
## <a name="software-requirements"></a>Yazılım Gereksinimleri

- Visual Studio 2012: İndirme [ASP.NET ve Web Araçları 2013.1 Visual Studio 2012 için](https://go.microsoft.com/fwlink/?LinkId=390062).
- Visual Studio 2013'ü: İndirin [Visual Studio 2013 Update 1](https://go.microsoft.com/fwlink/?LinkId=390064). Bu güncelleştirme, ASP.NET MVC 5.1 Razor görünümleri düzenleme için gereklidir.

<a id="download"></a>
## <a name="download"></a>İndir

Çalışma zamanı özellikleri, NuGet galerisindeki NuGet paketleri olarak kullanıma sunulur. Tüm çalışma zamanı paketlerini izleyin [Semantic Versioning](http://semver.org/) belirtimi. ASP.NET MVC 5.1 RTM paketi en son sürümü var: "5.1.2". Yükleme veya güncelleştirme yoluyla bu paketleri [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/). Sürüm, karşılık gelen yerelleştirilmiş NuGet paketleri de içerir.

NuGet Paket Yöneticisi konsolu kullanarak için kullanıma sunulan NuGet paketlerini güncelleştirin ya da yükleyin:

[!code-console[Main](mvc51-release-notes/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>Belgeler

Öğreticiler ve ASP.NET MVC 5.1 RTM ile ilgili diğer bilgileri ASP.NET web sitesinden kullanılabilir ( https://www.asp.net). 

<a id="new-features"></a>
## <a name="new-features-in-aspnet-mvc-51"></a>ASP.NET MVC 5.1 yeni özellikler

<a id="AttributeRouting"></a>

### <a name="attribute-routing-improvements"></a>Öznitelik yönlendirme geliştirmeleri

 Öznitelik şimdi destekler kısıtlamaları, sürüm oluşturma ve üst bilgi sağlayarak yönlendirme yol seçimi temel. Öznitelik rotaları birçok yönden aracılığıyla özelleştirilebilir `IDirectRouteFactory` arabirimi ve `RouteFactoryAttribute` sınıfı. Rota öneki ile Genişletilebilir `IRoutePrefix` arabirimi ve `RoutePrefixAttribute` sınıfı. 

<a id="Enum"></a>

### <a name="enum-support-in-views"></a>Görünümlerde enum desteği

1. Yeni `@Html.EnumDropDownListFor()` yardımcı yöntemler. Bu ifade değerlendirilmelidir uyarı ile HTML Yardımcıları çoğunu gibi kullanılması gereken bir [enum](https://msdn.microsoft.com/en-us/library/cc138362.aspx) türü veya [Nullable&lt;T&gt; ](https://msdn.microsoft.com/en-us/library/2cf62fcy.aspx) burada `T` bir olduğu[enum](https://msdn.microsoft.com/en-us/library/cc138362.aspx) türü. Kullanım `EnumHelper.IsValidForEnumHelper()` bu gereksinimleri kontrol ettiğinizden.
2. Yeni `EnumHelper.GetSelectList()` döndüren yöntemler bir `IList<SelectListItem>`. Arama, örneğin, önce bir seçim listesi işlemek gerektiğinde bu faydalıdır `@Html.DropDownListFor()`, veya adlarını görüntülemek istediğiniz zaman, `@Html.EnumDropDownListFor()` gösterir.

Aşağıdaki kod bu API'leri gösterir.

[!code-cshtml[Main](mvc51-release-notes/samples/sample2.cshtml)]

Tam bir örnek gördüğünüz [burada](https://aspnet.codeplex.com/SourceControl/latest#Samples/MVC/EnumSample/).

<a id="Bootstrap"></a>

### <a name="bootstrap-support-for-editor-templates"></a>Düzenleyici şablonları önyükleme desteği

Artık geçirme HTML özniteliklerindeki izin veriyoruz [EditorFor](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.editorextensions.editorfor(v=vs.100).aspx) olarak bir [anonim nesne](https://msdn.microsoft.com/en-us/library/bb397696.aspx).

Örneğin:

[!code-cshtml[Main](mvc51-release-notes/samples/sample3.cshtml)]

<a id="Unobtrusive"></a>

### <a name="unobtrusive-validation-for-minlengthattribute-and-maxlengthattribute"></a>MinLengthAttribute ve MaxLengthAttribute örtük doğrulama

Dize ve dizi türleri için istemci tarafı doğrulama, artık özellikleri ile donatılmış için desteklenecektir [MinLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.minlengthattribute(v=vs.110).aspx) ve [MaxLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.maxlengthattribute(v=vs.110).aspx) öznitelikleri.

<a id="thisContext"></a>

### <a name="supporting-the-this-context-in-unobtrusive-ajax"></a>'This' bağlamı içinde örtük Ajax destekleme

Geri arama işlevlerine (`OnBegin, OnComplete, OnFailure, OnSuccess`) artık aracılığıyla çağrılıyor öğeyi bulmak genişletebilirler `this` bağlamı. Örneğin:

[!code-html[Main](mvc51-release-notes/samples/sample4.html)]

<a id="KnownBreakingChanges"></a>

## <a name="known-issues-and-breaking-changes"></a>Bilinen sorunlar ve yeni değişiklikler

### <a name="attribute-routing"></a>Öznitelik yönlendirme

Öznitelik yönlendirme eşleşme belirsizliklerden artık ilk eşleşme seçmek yerine bir hata rapor eder.

Öznitelik rotaları kullanmaları yasaktır `{controller}` parametresi ve kullanarak `{action}` rota parametresi eylemleri yerleştirilir. Bu parametre kullanır, büyük olasılıkla belirsizlikleri için yol açar. 

### <a name="scaffolding-mvcweb-api-into-a-project-with-51-packages-results-in-50-packages-for-ones-that-dont-already-exist-in-the-project"></a>Yapı iskelesi MVC/Web API projesine 5.0 paketler için projede mevcut 5.1 paketleri sonuçları

ASP.NET MVC 5.1 RTM için NuGet paketlerini güncelleştirme, Visual Studio Araçları gibi ASP.NET iskeleti oluşturma veya ASP.NET Web uygulaması proje şablonu güncelleştirmez. ASP.NET çalışma zamanı paketlerini (5.0.0.0) önceki sürümünü kullanırlar. Sonuç olarak, zaten projelerinizde kullanılabilir yoksa ASP.NET iskeleti oluşturma önceki sürümünü (5.0.0.0) gerekli paketleri yükleyin. Ancak, en son paketlerin projelerinizde te ASP.NET iskeleti oluşturma veya güncelleştirme 1'Visual Studio 2013 RTM üzerine yazılmaz. Sonra Web API 2.1 veya ASP.NET MVC 5.1 projelerinizin paketlerin güncelleştirilmesi, ASP.NET iskeleti oluşturma kullanırsanız, Web API ve ASP.NET MVC sürümleri tutarlı olduğundan emin olun. 

### <a name="syntax-highlighting-for-razor-views-in-visual-studio-2013"></a>Visual Studio 2013'te Razor görünümleri için söz dizimi vurgulama

Visual Studio 2013 güncelleştirme yapmadan ASP.NET MVC 5.1 RTM'YE'ı güncelleştirirseniz, Visual Studio Düzenleyicisi desteği Razor görünümleri düzenlenirken söz dizimi vurgulama için almazsınız. Bu destek almak için Visual Studio 2013 güncelleştirmeniz gerekir. 

### <a name="type-renames"></a>Tür yeniden adlandırma

5.1 RTM'de öznitelik yönlendirme genişletilebilirliği için kullanılan türlerinin bazılarını yeniden adlandırılmıştır.

| **Eski tür adı (5.1 RC)** | **Yeni tür adı (5.1 RTM)** |
| --- | --- |
| IDirectRouteProvider | IDirectRouteFactory |
| RouteProviderAttribute | RouteFactoryAttribute |
| DirectRouteProviderContext | DirectRouteFactoryContext |

<a id="bug-fixes"></a>
## <a name="bug-fixes"></a>Hata Düzeltmeleri

Bu sürüm ayrıca çeşitli hata düzeltmeleri içerir. Tam listesini burada bulabilirsiniz:

- [5.1.0 paket](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.1%20Preview|v5.1%20RTM&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)
- [5.1.1 paket](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.1.1%20RTM&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)

5.1.2 paket IntelliSense güncelleştirme, ancak hiçbir hata düzeltmeleri içerir.
