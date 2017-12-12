---
uid: single-page-application/overview/templates/hottowel-template
title: "Sık kullanılan havlu şablon | Microsoft Docs"
author: madskristensen
description: "HotTowel şablonu"
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/09/2013
ms.topic: article
ms.assetid: 75af2e17-6ed3-4d24-8ea1-bc340027c318
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/templates/hottowel-template
msc.type: authoredcontent
ms.openlocfilehash: bfc6e2c884c422f44e8be5f4f29554ae86f7ecb6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="hot-towel-template"></a>Sık kullanılan havlu şablonu
====================
tarafından [Kristensen Mads](https://github.com/madskristensen)

> Sık kullanılan havlu MVC şablonu tarafından John Papa yazılır
> 
> İndirmek için hangi sürümü seçin:
> 
> [Visual Studio 2012 için sık kullanılan havlu MVC şablonu](https://visualstudiogallery.msdn.microsoft.com/1f68fbe8-b4e9-4968-9fd3-ddc7cbc52dca)
> 
> [Visual Studio 2013 için sık kullanılan havlu MVC şablonu](https://visualstudiogallery.msdn.microsoft.com/1eb8780d-d522-4dcf-bf56-56f0eab305c2)


> Sık kullanılan havlu: biri olmadan SPA gitmek istemeyeceğiniz için!


Bir SPA oluşturmak istediğiniz ancak nerede başlatılacağını karar veremez? Sık kullanılan havlu kullanın ve saniye cinsinden bir SPA ve üzerinde oluşturmak için ihtiyacınız olan araçları gerekir!

Sık kullanılan havlu tek sayfa uygulama (SPA) ASP.NET ile oluşturmak için harika bir başlangıç noktası oluşturur. Kutudan çıktığında, onu modüler yapı, kod, görünümü gezinti, veri bağlama, zengin veri yönetimi ve basit ancak Zarif stil sağlar. Sık kullanılan havlu uygulamanıza, tesisat odaklanabilirsiniz bir SPA oluşturmak için gereken her şeyi sağlar.

> Gelen bir SPA oluşturma hakkında daha fazla bilgi [John Papa'nın videoları, eğitim ve Pluralsight kurslar](http://johnpapa.net/spa?vsix).


## <a name="application-structure"></a>Uygulama yapısı

Sık kullanılan havlu SPA uygulamanızı tanımlamak JavaScript ve HTML dosyaları içeren bir uygulama klasörü sağlar.

Uygulama klasörü içinde:

- Durandal
- hizmetler
- viewmodels
- görünümler

Uygulama klasörünün modülleri koleksiyonunu içerir. Bu modüller işlevleri kapsülleyen ve diğer modüller üzerinde bağımlılıkları bildirin. Görünümler klasöründe, uygulamanız için HTML ve viewmodels klasör görünümleri (ortak MVVM deseni) sunu mantığını içerir. Hizmetleri klasörü, uygulamanızın HTTP veri alma veya yerel depolama etkileşim gibi gerekebilir tüm ortak Hizmetleri barındırmak için idealdir. Hizmet modülleri koddan yeniden kullanmak için birden çok viewmodels için yaygın bir sorundur.

## <a name="aspnet-mvc-server-side-application-structure"></a>ASP.NET MVC sunucu tarafı uygulama yapısı

Sık kullanılan havlu tanıdık ve güçlü ASP.NET MVC yapısını oluşturur.

- Uygulama\_Başlat
- İçerik
- Denetleyiciler
- Modeller
- Komut dosyaları
- Görünümler

## <a name="featured-libraries"></a>Öne çıkan kitaplıkları

- ASP.NET MVC
- ASP.NET Web API
- ASP.NET Web iyileştirme - paketleme ve küçültme
- [BREEZE.js](http://Breezejs.com) -zengin veri yönetimi
- [Durandal.js](http://Durandaljs.com) -gezinti ve görünüm oluşturma
- [Knockout.js](http://Knockoutjs.com) -veri bağlamaları
- [Require.js](http://requirejs.org) -modülerlik AMD ve en iyi duruma getirme
- [Toastr.js](http://jpapa.me/c7toastr) -açılan iletiler
- [Önyükleme twitter](http://twitter.github.com/bootstrap/) - sağlam CSS stil oluşturma

## <a name="installing-via-the-visual-studio-2012-hot-towel-spa-template"></a>Visual Studio 2012 etkin havlu SPA şablonu yükleme

Sık kullanılan havlu Visual Studio 2012 şablon olarak yüklenebilir. Tıklatmanız `File`  |  `New Project` ve `ASP.NET MVC 4 Web Application`. Ardından ' etkin havlu tek sayfa uygulaması "şablonu ve çalıştırın!

## <a name="installing-via-the-nuget-package"></a>NuGet paketi yükleniyor

Sık kullanılan havlu de varolan boş bir ASP.NET MVC projesini güçlendirir bir NuGet paketidir. Yalnızca Nuget kullanarak yükleyin ve ardından çalıştırın!

[!code-powershell[Main](hottowel-template/samples/sample1.ps1)]

## <a name="how-do-i-build-on-hot-towel"></a>Etkin havlu nasıl oluşturulacağına?

Yalnızca kod eklemeye başlayın!

1. Kendi sunucu tarafı kodu, tercihen Entity Framework ve Webapı (gerçekten Breeze.js ile görünmek) ekleyin
2. Görünümlerine ekleme `App/views` klasörü
3. Viewmodels için ekleme `App/viewmodels` klasörü
4. HTML ve Boşaltılan veri bağlamaları yeni görünümlerinize ekleme
5. Gezinti yollar güncelleştir`shell.js`

## <a name="walkthrough-of-the-htmljavascript"></a>HTML/JavaScript gözden geçirme

### <a name="viewshottowelindexcshtml"></a>Views/HotTowel/index.cshtml

Index.cshtml başlangıç rota ve görünüm MVC uygulaması için ' dir. Tüm standart meta etiketleri, css bağlantılar ve beklediğiniz JavaScript başvurular içerir. Tek bir gövdesinde `<div>` istendiklerinde tüm içeriği (görünümlerinizi) yerleştirileceği olduğu. `@Scripts.Render` Require.js bulunan uygulama kodunun için giriş noktası çalıştırmak için kullandığı `main.js` dosya. Karşılama ekranı uygulamanızı yüklenirken giriş ekranı oluşturma göstermek için sağlanmıştır.

[!code-cshtml[Main](hottowel-template/samples/sample2.cshtml)]

### <a name="appmainjs"></a>App/Main.js

`main.js` Dosyası uygulamanızı yüklendikten hemen sonra çalıştırılacak kodu içerir. Gezinti yollarınızı tanımlamak, görünümler, başlangıcı Ayarla ve tüm Kurulum/uygulamanızın veri hazırlama gibi önyükleme gerçekleştirmek istediğiniz budur.

`main.js` Dosyası birkaç başlangıç uygulaması başlangıç yardımcı olmak için durandal'ın modülleri tanımlar. Tanımla deyimi işlevi için kullanılabilir olmaları modülleri bağımlılıkları çözmenize yardımcı olur. İlk hata ayıklama iletilerini uygulama konsol penceresine gerçekleştiriyor iletileri hangi olayların hakkında göndermesi etkinleştirilir. App.start kod uygulamayı başlatmak için durandal framework bildirir. Kuralları, böylece tüm görünümleri durandal bilir ve viewmodels sırasıyla aynı adlandırılmış klasörlerde bulunan ayarlanır. Son olarak, `app.setRoot` yükleri devreye `shell` önceden tanımlanmış bir kullanarak `entrance` animasyon.

[!code-javascript[Main](hottowel-template/samples/sample3.js)]

## <a name="views"></a>Görünümler

Görünümler içinde bulunan `App/views` klasör.

### <a name="shellhtml"></a>Shell.HTML

`shell.html` İçin HTML ana düzeni içerir. Tüm diğer görünümlerinizi tarafında içinde herhangi bir yerde oluşacaktır, `shell` görünümü. Sık kullanılan havlu sağlar bir `shell` üç tür bölgeleriyle: üst bilgi, içerik alanının ve altbilgi. Bu bölgeler her ile yüklenen içeriği istendiğinde diğer görünümlere form.

`compose` Üstbilgi ve altbilgi için bağlamaları sabit işaret etmek için sık kullanılan havlu kodlanmış `nav` ve `footer` sırasıyla görüntüler. Bölümü için oluşturma bağlama `#content` bağlı `router` modülünün etkin öğesi. Diğer bir deyişle, tıkladığınızda bu alana karşılık gelen görünümdür bir gezinti bağlantısı yüklenir.

[!code-html[Main](hottowel-template/samples/sample4.html)]

### <a name="navhtml"></a>NAV.HTML

`nav.html` SPA Gezinti bağlantıları içeriyor. Bu, burada menü yapısı, örneğin yerleştirilebilir değildir. Bu veri bağlama (Boşaltılan kullanarak) için görülür `router` , tanımladığınız Gezinti görüntülemek için Modülü `shell.js`. Boşaltılan veri bağlama öznitelikleri ve onlara bağlar arar `shell` viewmodel Gezinti yolları görüntüler ve (Twitter Bootstrap kullanarak) progressbar göstermek için `router` modülüdür'ın başka bir (görmek bir görünümden gezinme ortasında `router.isNavigating`).

[!code-html[Main](hottowel-template/samples/sample5.html)]

### <a name="homehtml-and-detailshtml"></a>Home.HTML ve details.html

Bu görünümler HTML için özel görünümleri içerir. Zaman `home` bağlamak `nav` görünümün menü tıklandığında, `home` görünümü içerik alanında yerleştirilecek `shell` görünümü. Bu görünümler, genişletilebilir veya kendi özel görünümlerinizi ile değiştirilir.

### <a name="footerhtml"></a>Footer.HTML

`footer.html` İçeren alt kısmındaki altbilgisindeki görüntülenen HTML `shell` görünümü.

## <a name="viewmodels"></a>ViewModels

İçinde bulunan ViewModels `App/viewmodels` klasör.

### <a name="shelljs"></a>Shell.js

`shell` Viewmodel içermektedir özelliklerini ve bağlı olan işlevler `shell` görünümü. Bu menü Gezinti bağlamaları bulunduğu görülür (bkz `router.mapNav` mantığı).

[!code-javascript[Main](hottowel-template/samples/sample6.js)]

### <a name="homejs-and-detailsjs"></a>Home.js ve details.js

Bu viewmodels bağlı olan işlevler ve Özellikler içeren `home` görünümü. Ayrıca görünümün sunu mantığını içerir ve veri ve görünüm arasında bir Yapıştırıcı işlevi görür.

[!code-javascript[Main](hottowel-template/samples/sample7.js)]

## <a name="services"></a>Hizmetler

Hizmetleri uygulama/hizmetleri klasöründe bulunur. İdeal olarak, gelecekteki hizmetlerinizi uzak veri gönderme ve alma için sorumlu olan dataservice modülü gibi yerleştirilemiyor.

### <a name="loggerjs"></a>Logger.js

Sık kullanılan havlu sağlayan bir `logger` Hizmetleri klasörü modülünde. `logger` Modülüdür günlük iletilerini konsola ve bildirimleri açılır kullanıcıya için idealdir.
