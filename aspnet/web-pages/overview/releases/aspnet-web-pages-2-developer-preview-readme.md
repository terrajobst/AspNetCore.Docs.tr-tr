---
uid: web-pages/overview/releases/aspnet-web-pages-2-developer-preview-readme
title: ASP.NET Web sayfaları 2 Geliştirici önizlemesi Benioku dosyası | Microsoft Docs
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/14/2011
ms.topic: article
ms.assetid: 159a92e2-e011-4da7-b61d-2edde2a967da
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/releases/aspnet-web-pages-2-developer-preview-readme
msc.type: authoredcontent
ms.openlocfilehash: 914e99491908294cea1e04dd23ccdb66c1dc05a3
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37364436"
---
<a name="aspnet-web-pages-2-developer-preview-readme"></a>ASP.NET Web sayfaları 2 Geliştirici önizlemesi Benioku dosyası
====================
tarafından [Microsoft](https://github.com/microsoft)

## <a name="aspnet-web-pages-2-developer-preview-readme"></a>ASP.NET Web sayfaları 2 Geliştirici önizlemesi Benioku dosyası

14 Eylül 2011

### <a name="contents"></a>İçindekiler

#### <a id="_Toc303701284"></a>  Yükleme notları

Web sayfaları 2 Geliştirici Önizlemesi'ni yüklemek için bu seçenekler vardır:

- WebMatrix 2 Beta yüklemesi kullanılarak [Web Platformu yükleyicisi](https://go.microsoft.com/fwlink/?LinkId=226883). WebMatrix ASP.NET Web sayfaları içeren bir ücretsiz web geliştirme araçları kümesidir. Daha fazla bilgi için bkz. yükleme bölümünde [üst özellikleri ASP.NET Web sayfaları 2 Geliştirici önizlemesi](https://go.microsoft.com/fwlink/?LinkID=227824).

- Web sayfaları 2 Geliştirici önizlemesi kullanarak doğrudan yüklemek [indirme bağlantısı](https://go.microsoft.com/fwlink/?LinkID=226335). Düzenleyici Not Defteri gibi bir metin kullanarak Web Pages uygulamaları oluşturmak istiyorsanız bu yaklaşımı kullanın. Web sayfaları 2 uygulamaları çalıştırmak için IIS 7.5 Express olması gerekir. (Bu otomatik olarak WebMatrix ile eklenmiştir.) IIS Express kullanarak bir Web Pages sayfası test etme hakkında ipuçları görebileceği için Kenar çubuğunda "Oluşturma ve sınama ASP.NET sayfaları kullanarak bilgisayarınızı kendi metin düzenleyicisi" [WebMatrix ve ASP.NET Web sayfaları ile çalışmaya başlama](https://go.microsoft.com/fwlink/?LinkId=202889).

ASP.NET Web sayfaları 2 Geliştirici önizlemesi yüklü olması ve yan yana çalıştırabilirsiniz ASP.NET Web sayfaları 1. <a id="a"></a>Ayrıntılar için konusundaki "Çalışan Web Pages uygulamaları yan yana" bölümüne bakın. [Web sayfaları 2 Geliştirici önizlemesi üst özelliklerinde](https://go.microsoft.com/fwlink/?LinkID=227824).

#### <a id="_Toc303701285"></a>  Belgeleri

Öğreticiler ve diğer ASP.NET Web sayfaları hakkında bilgiler Web sitesinin ASP.NET Web sayfaları sayfasında kullanılabilir ([https://www.asp.net/web-pages/](../../index.md)). Yeni özellikler ve Web Pages 2'deki geliştirmeler hakkında daha fazla bilgi için bkz: [Web sayfaları 2 Geliştirici önizlemesi üst özelliklerinde](https://go.microsoft.com/fwlink/?LinkID=227824).

#### <a id="_Toc303701286"></a>  Destek

<a id="_Toc209852135"></a><a id="_Toc255833657"></a> Bu bir önizleme sürümüdür ve resmi olarak desteklenmez. Bu sürümle birlikte çalışma hakkında sorularınız varsa, bunları ASP.NET Web Pages foruma ([ https://forums.asp.net/1224.aspx/1?WebMatrix ](https://forums.asp.net/1224.aspx/1?WebMatrix) ), burada ASP.NET topluluk üyelerinin resmi olmayan destek sık sağlayabilir.

#### <a id="_Toc303701287"></a>  Yazılım gereksinimleri

ASP.NET Web Pages 2, .NET Framework 4 gerektirir. Ayrıca .NET Framework 4.5 Developer Preview sürümüyle de çalışır.

<a id="_Toc303701288"></a><a id="_Breaking_Changes"></a>

#### <a name="fixes-known-issues-and-breaking-changes"></a>Düzeltmeleri ve bilinen sorunlar bozucu değişiklikler

<a id="_Toc224729061"></a><a id="_Toc238051347"></a>

- **Olan\* yöntemleri (örneğin, IsDateTime) şimdi dönüş doğru değerleri tüm kültürler için kullanılabilecek.** Bazı yöntemler ister *IsDateTime* daha önce döndürülen *false* olduğunda bunlar döndürülen *true* kültüre özgü denetimleri daha önce gerçekleştirdiğiniz olduğundan. Bu yöntemler artık kültür hesaba katması için düzeltildi. Bu, bir değişiklik, Uygulamanız eski davranışına dayanıyorsa, çalışmamasına neden olur.
- **Href yöntemin davranışı değişti.** Daha önce Href("~/SomeFile") arama şu anda yürütülmekte olan dosya göreli bir URL döndürecektir. Şimdi Href("~/SomeFile") her zaman uygulamanın kökünden mutlak bir yol döndürür. Çoğu durumda, bu davranışı, dönüş değeri bir fark yaratmayacaktır. Ajax senaryolarında bulunan belirli düzeltmek için bu değişiklik yapılmıştır. Örneğin, aşağıdaki kod örneği göz önünde bulundurun: 

    [!code-cshtml[Main](aspnet-web-pages-2-developer-preview-readme/samples/sample1.cshtml)]

    Bu kod, bu sayfa için bir Ajax isteği için yanlış olacaktır Images/Logo.jpg için daha önce çözmek. Bunu şimdi köküne çözümler (/ MySite/Images/Logo.jpg).
- **HttpContext.RedirectLocal yöntemi değişti**. Bu yöntem, artık yalnızca geçerli uygulama göreli URL'ler kabul eder. Tam URL'leri reddedilir.
- **ModelState.IsValid yöntemi artık ilk doğrulama çağırmanızı gerektirir**. Uygulamanızı yeni bir giriş doğrulama yöntemlerini kullanacak şekilde dönüştürme ve aradığınız *ModelState.IsValid* yöntemi, artık çağırmanız *Validation.Validate* önceden. Örneğin, artık bu deseni izlemelidir: 

    [!code-csharp[Main](aspnet-web-pages-2-developer-preview-readme/samples/sample2.cs)]

  Ancak, yoksa, yeni bir giriş doğrulama yöntemlerini kullanıyorsanız kullanmanızı öneririz *ModelState.IsValid*. Bunun yerine, kodunuzu bunun gibi yapılandırın: 

    [!code-csharp[Main](aspnet-web-pages-2-developer-preview-readme/samples/sample3.cs)]
- **Internet Explorer 7 ve Internet Explorer 8'de, istemci tarafı doğrulama çalışmıyor**. İstemci tarafı doğrulama 1.6.2, jQuery uyumsuzluklar nedeniyle varsayılan proje şablonuyla dahil edildiği çalışmaz. (Sunucu tarafı doğrulama çalışır.).

#### <a id="_Toc303701289"></a>  Sorumluluk reddi

© 2011 Microsoft Corporation. Tüm hakları saklıdır. Bu belgede sağlanan "olarak-olduğundan." Bilgi ve URL ve diğer Internet Web sitesi başvuruları dahil olmak üzere bu belgede, bildirilmeksizin değiştirilebilir. Bunu kullanarak riski size aittir.
