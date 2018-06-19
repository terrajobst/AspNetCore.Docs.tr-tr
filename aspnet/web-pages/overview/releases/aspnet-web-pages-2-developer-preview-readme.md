---
uid: web-pages/overview/releases/aspnet-web-pages-2-developer-preview-readme
title: ASP.NET Web Pages 2 Geliştirici önizlemesi Benioku | Microsoft Docs
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/14/2011
ms.topic: article
ms.assetid: 159a92e2-e011-4da7-b61d-2edde2a967da
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/releases/aspnet-web-pages-2-developer-preview-readme
msc.type: authoredcontent
ms.openlocfilehash: 1a43b2b12af9cd223d8a3622239743f7c431f617
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30899096"
---
<a name="aspnet-web-pages-2-developer-preview-readme"></a>ASP.NET Web Pages 2 Geliştirici önizlemesi Benioku dosyası
====================
tarafından [Microsoft](https://github.com/microsoft)

## <a name="aspnet-web-pages-2-developer-preview-readme"></a>ASP.NET Web Pages 2 Geliştirici önizlemesi Benioku dosyası

14 Eylül 2011

### <a name="contents"></a>İçindekiler

#### <a id="_Toc303701284"></a>  Yükleme notları

Web Pages 2 Geliştirici önizlemesi yüklemek için bu seçenekler vardır:

- WebMatrix 2 Beta yüklemesi kullanılarak [Web Platformu yükleyicisi](https://go.microsoft.com/fwlink/?LinkId=226883). WebMatrix, ASP.NET Web sayfaları içeren bir ücretsiz web geliştirme araçları kümesidir. Daha fazla bilgi için yükleme bölümüne bakın [ASP.NET Web Pages 2 Geliştirici önizlemesi üst özelliklerinde](https://go.microsoft.com/fwlink/?LinkID=227824).

- Web Pages 2 Geliştirici önizlemesi kullanarak doğrudan yükleme [bağlantı karşıdan](https://go.microsoft.com/fwlink/?LinkID=226335). Düzenleyici Not Defteri gibi bir metin kullanarak Web Pages uygulamaları oluşturmak istiyorsanız bu yaklaşımı kullanın. Web Pages 2 uygulamaları çalıştırmak için IIS 7.5 Express olması gerekir. (Bu WebMatrix ile otomatik olarak dahil edilir.) IIS Express, kullanarak bir Web Pages sayfası test konusunda ipuçları görmek için "Oluşturma ve test ASP.NET sayfaları bilgisayarınızı kendi Metin Düzenleyici'yi kullanma" kenar [WebMatrix ve ASP.NET Web sayfaları ile çalışmaya başlama](https://go.microsoft.com/fwlink/?LinkId=202889).

ASP.NET Web Pages 2 Geliştirici önizlemesi yüklenebilir ve yan yana çalıştırabilirsiniz ASP.NET Web sayfaları 1. <a id="a"></a>Ayrıntılar için "Çalışan Web Pages uygulamaları yan yana" bölümüne bakın [Web Pages 2 Geliştirici önizlemesi üst özelliklerinde](https://go.microsoft.com/fwlink/?LinkID=227824).

#### <a id="_Toc303701285"></a>  Belgeleri

Öğreticiler ve diğer bilgiler hakkında ASP.NET Web sayfaları ASP.NET Web sitesi Web Pages sayfasında kullanılabilir ([https://www.asp.net/web-pages/](../../index.md)). Yeni özellikler ve geliştirmeler Web Pages 2 hakkında daha fazla bilgi için bkz: [Web Pages 2 Geliştirici önizlemesi üst özelliklerinde](https://go.microsoft.com/fwlink/?LinkID=227824).

#### <a id="_Toc303701286"></a>  Destek

<a id="_Toc209852135"></a><a id="_Toc255833657"></a> Bu bir önizleme sürümüdür ve resmi olarak desteklenmez. Bu sürüm ile birlikte çalışma hakkında sorularınız varsa, bunları ASP.NET Web sayfaları Forumu sonrası ([ https://forums.asp.net/1224.aspx/1?WebMatrix ](https://forums.asp.net/1224.aspx/1?WebMatrix) ), burada ASP.NET topluluk üyeleri resmi olmayan destek sık sağlayabilir.

#### <a id="_Toc303701287"></a>  Yazılım gereksinimleri

ASP.NET Web Pages 2 .NET Framework 4 gerektirir. Ayrıca .NET Framework 4.5 Developer Preview sürümüyle de çalışır.

<a id="_Toc303701288"></a><a id="_Breaking_Changes"></a>

#### <a name="fixes-known-issues-and-breaking-changes"></a>Düzeltmeleri, bilinen sorunlar ve önemli değişiklikler

<a id="_Toc224729061"></a><a id="_Toc238051347"></a>

- **Olan\* yöntemleri (örneğin, IsDateTime) şimdi dönüş doğru değerlerin tüm kültürler için kullanılabilecek.** Bazı yöntemler ister *IsDateTime* döndürülen *false* bunlar döndürdü *true* kültüre özgü denetimleri daha önce gerçekleştirdiğiniz olduğundan. Bu yöntemleri şimdi kültür dikkate düzeltildi. Önemli bir değişiklik budur; Uygulamanızın üzerinde eski davranışı dayalıysa, kesilir.
- **Href yöntemi davranışı değişmiştir.** Daha önce Href("~/SomeFile") çağırma şu anda yürütülmekte olan dosya göreli bir URL döndürecektir. Şimdi Href("~/SomeFile") her zaman uygulamanın kökünden mutlak bir yol döndürür. Çoğu durumda bu davranış dönüş değeri bir fark yapmaz. Bu değişiklik, belirli Ajax senaryolarında düzeltmek için yapılmıştır. Örneğin, aşağıdaki kod örneği göz önünde bulundurun: 

    [!code-cshtml[Main](aspnet-web-pages-2-developer-preview-readme/samples/sample1.cshtml)]

    Bu kodu daha önce Images/Logo.jpg için bu sayfayı bir Ajax isteği için yanlış olur sorununu çözmek. Şimdi köküne çözümleyecek (/ MySite/Images/Logo.jpg).
- **HttpContext.RedirectLocal yöntemi değişti**. Bu yöntem, artık yalnızca geçerli uygulama göreli URL kabul eder. Tam URL'leri reddedilir.
- **ModelState.IsValid yöntemi şimdi doğrula önce çağrılacak gerektirir**. Yeni giriş doğrulaması yöntemlerini kullanmak için uygulamanızı dönüştürme ve aradığınız *ModelState.IsValid* yöntemi, şimdi çağırmanız gerekir *Validation.Validate* önceden. Örneğin, artık bu deseni izlemenizi gerekir: 

    [!code-csharp[Main](aspnet-web-pages-2-developer-preview-readme/samples/sample2.cs)]

  Ancak, yeni giriş doğrulaması yöntemleri kullanırsanız kullanma öneririz *ModelState.IsValid*. Bunun yerine, kodunuzu bu gibi yapılandırın: 

    [!code-csharp[Main](aspnet-web-pages-2-developer-preview-readme/samples/sample3.cs)]
- **Internet Explorer 7 ve Internet Explorer 8, istemci tarafı doğrulama çalışmıyor**. İstemci tarafı doğrulama jQuery 1.6.2, uyumsuzluklar nedeniyle varsayılan proje şablonu ile dahil edildiği çalışmaz. (Sunucu tarafı doğrulama çalışır.).

#### <a id="_Toc303701289"></a>  Sorumluluk reddi

© 2011 Microsoft Corporation. Tüm hakları saklıdır. Bu belgede sağlanan "olarak-değil." URL ve diğer Internet Web sitesi başvuruları dahil olmak üzere bu belgede belirtilen bilgiler ve görüntüler bildirim yapılmadan değiştirilebilir. Kullanım riski size aittir.
