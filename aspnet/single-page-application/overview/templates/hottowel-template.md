---
uid: single-page-application/overview/templates/hottowel-template
title: Hot Towel şablonu | Microsoft Docs
author: madskristensen
description: HotTowel şablonu
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/09/2013
ms.topic: article
ms.assetid: 75af2e17-6ed3-4d24-8ea1-bc340027c318
ms.technology: ''
msc.legacyurl: /single-page-application/overview/templates/hottowel-template
msc.type: authoredcontent
ms.openlocfilehash: de81f12f57d7f2fb7c6478bfa1f3a278ae905a39
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388558"
---
<a name="hot-towel-template"></a>Hot Towel şablonu
====================
tarafından [Mads Kristensen](https://github.com/madskristensen)

> Hot Towel MVC şablonu tarafından John Papa yazılır
> 
> İndirmek için hangi sürümünü seçin:
> 
> [Visual Studio 2012 için Hot Towel MVC şablonu](https://visualstudiogallery.msdn.microsoft.com/1f68fbe8-b4e9-4968-9fd3-ddc7cbc52dca)
> 
> [Visual Studio 2013 için Hot Towel MVC şablonu](https://visualstudiogallery.msdn.microsoft.com/1eb8780d-d522-4dcf-bf56-56f0eab305c2)
> 
> 
> Hot Towel: olmadan bir SPA gitmek istemediğinden!


Nereden başlayacağınızı karar veremez ancak bir SPA oluşturmak istiyorsunuz? Hot Towel kullanın ve saniyeler içinde bir SPA ve üzerinde oluşturmak için ihtiyacınız olan tüm araçları gerekir!

Hot Towel tek sayfa uygulama (SPA) ASP.NET ile oluşturmak için harika bir başlangıç noktası oluşturur. Kullanıma hazır, bunu bir modüler yapı, kod, görünümü gezinti, veri bağlama, zengin veri yönetimini ve basit ama Zarif stil sağlar. Hot Towel uygulamanıza tesisat odaklanmanızı sağlayacak bir SPA oluşturmak için ihtiyacınız olan her şeyi sağlar.

> Gelen bir SPA oluşturma hakkında daha fazla bilgi [John Papa'nın videolar, öğreticiler ve Pluralsight kurslarına](http://johnpapa.net/spa?vsix).


## <a name="application-structure"></a>Uygulama yapısı

Hot Towel SPA uygulamanızı tanımlayan JavaScript ve HTML dosyaları içeren bir uygulama klasörü sağlar.

İçinde uygulama klasörü:

- durandal
- hizmetler
- viewmodel'lar
- görünümler

Uygulama klasör modülleri koleksiyonunu içerir. Bu modüller işlevi kapsülleyen ve diğer modüllerin üzerinde bağımlılıkları bildirin. Görünüm klasörü uygulamanız için bir HTML ve viewmodel'lar klasör görünümleri (ortak MVVM desen) sunu mantığını içerir. Hizmetleri klasörü, uygulamanızın HTTP veri alma veya yerel depolama etkileşimi gibi gerekebilir tüm ortak Hizmetleri barındırmak için idealdir. Hizmet modüllerden kod yeniden kullanımı için birden çok viewmodel'lar için yaygındır.

## <a name="aspnet-mvc-server-side-application-structure"></a>ASP.NET MVC sunucu tarafı uygulama yapısı

Hot Towel üzerinde tanıdık ve güçlü ASP.NET MVC yapısı oluşturur.

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
- [Durandal.js](http://Durandaljs.com) -gezinti ve görünümü oluşturma
- [Knockout.js](http://Knockoutjs.com) -veri bağlamaları
- [Require.js](http://requirejs.org) -modülerlik AMD ve en iyi duruma getirme
- [Toastr.js](http://jpapa.me/c7toastr) -açılan iletiler
- [Twitter Bootstrap](http://twitter.github.com/bootstrap/) - sağlam CSS stil oluşturma

## <a name="installing-via-the-visual-studio-2012-hot-towel-spa-template"></a>Visual Studio 2012 Hot Towel SPA şablonu yükleme

Hot Towel Visual Studio 2012 şablon olarak yüklenebilir. Tıklamanız yeterli `File`  |  `New Project` ve `ASP.NET MVC 4 Web Application`. Ardından ' Hot Towel tek sayfalı uygulama "şablonu ve çalıştırın!

## <a name="installing-via-the-nuget-package"></a>NuGet paketi yükleme

Hot Towel Ayrıca var olan boş bir ASP.NET MVC projesi çoğaltan bir NuGet paketidir. Nuget kullanarak yalnızca yükleyin ve ardından çalıştırın!

[!code-powershell[Main](hottowel-template/samples/sample1.ps1)]

## <a name="how-do-i-build-on-hot-towel"></a>Hot Towel üzerinde nasıl oluşturabilirim?

Yalnızca kod ekleyerek başlayın!

1. Tercihen Entity Framework ve Webapı (gerçekten ile Breeze.js aydınlatmak), kendi sunucu tarafı kodu ekleyin
2. Görünümlere ekleme `App/views` klasörü
3. Ekleme için viewmodel'lar `App/viewmodels` klasörü
4. HTML ve Boşaltılan veri bağlamaları yeni görünümlerinize ekleme
5. Gezinti yolları güncelleştir `shell.js`

## <a name="walkthrough-of-the-htmljavascript"></a>HTML/JavaScript gözden geçirme

### <a name="viewshottowelindexcshtml"></a>Views/HotTowel/index.cshtml

Index.cshtml başlangıç rota ve MVC uygulaması için Görünüm ' dir. Bu, tüm standart meta etiketler, css bağlantılar ve beklediğiniz JavaScript başvurular içerir. Tek bir gövde içeren `<div>` istendiklerinde tüm içeriği (görünümlerinizde) yerleştirileceği olduğu. `@Scripts.Render` Require.js giriş noktası bulunan uygulama kodunu çalıştırmak için kullandığı `main.js` dosya. Giriş ekranı, uygulamanızı yüklenirken giriş ekranı oluşturma işlemini göstermek için sağlanmıştır.

[!code-cshtml[Main](hottowel-template/samples/sample2.cshtml)]

### <a name="appmainjs"></a>App/Main.js

`main.js` Dosya, uygulama yüklendikten hemen sonra çalıştırılacak kodunu içerir. Gezinti yollarınızı tanımlamak, görünümler, başlangıç ayarlayın ve tüm Kurulum/uygulamanızın verilerini hazırlama gibi önyüklemesi gerçekleştirmek istediğiniz budur.

`main.js` Dosyası durandal'ın modüllerinin Başlat uygulama clark'i yardımcı olmak için birkaç tanımlar. Tanımlama bildirimi işlevi kullanabilmesi için modülleri bağımlılıklarını Çözümle yardımcı olur. İlk hata ayıklama iletilerini hangi olayların konsol penceresinde uygulamanın gerçekleştirdiği ileti göndermesi etkinleştirilir. Uygulamayı başlatmak için durandal framework app.start kodu bildirir. Kuralları, böylece tüm görünümlere durandal bilir ve viewmodel'lar sırasıyla aynı adlandırılmış klasörlerde bulunan ayarlanır. Son olarak, `app.setRoot` yükleri devreye `shell` önceden tanımlanmış bir şablon kullanarak `entrance` animasyon.

[!code-javascript[Main](hottowel-template/samples/sample3.js)]

## <a name="views"></a>Görünümler

Görünümler içinde bulunan `App/views` klasör.

### <a name="shellhtml"></a>Shell.HTML

`shell.html` Ana düzen için HTML içerir. Tüm diğer görünümlerinizde yere in tarafında oluşacaktır, `shell` görünümü. Hot Towel sağlayan bir `shell` üç bölgeleriyle: üst bilgi, bir içerik alanı ve alt bilgi. Her biri bu bölgeler ile yüklenen içeriği form istendiğinde diğer görünümleri.

`compose` Üstbilgi ve altbilgi bağlamalarda sabit işaret edecek şekilde Hot Towel içinde kodlanmış `nav` ve `footer` görünümleri, sırasıyla. Bölüm için oluşturma bağlama `#content` bağlı `router` modülün etkin öğeyi. Diğer bir deyişle, karşılık gelen bir görünümü olan bir gezinti bağlantısı tıkladığınızda Bu alanda yüklenir.

[!code-html[Main](hottowel-template/samples/sample4.html)]

### <a name="navhtml"></a>NAV.HTML

`nav.html` SPA Gezinti bağlantıları içerir. Burada menüsü yapısı, örneğin yerleştirilebilir budur. Genellikle veri bağlama (Knockout kullanarak) için budur `router` modülünde, tanımladığınız Gezinti görüntülemek için `shell.js`. Knockout arar veri bağlama özniteliklerini ve bunları bağlar `shell` viewmodel Gezinti tariflerini görüntülemek ve bir progressbar (Twitter Bootstrap ile) göstermek için ise `router` modülüdür başka bir (bkz: tek bir görünümde gezinme ortasında `router.isNavigating`).

[!code-html[Main](hottowel-template/samples/sample5.html)]

### <a name="homehtml-and-detailshtml"></a>Home.HTML ve details.html

Bu görünümler, HTML için özel görünümleri içerir. Zaman `home` bağlantısını `nav` Görünüm menüsünde tıklandığında `home` görünümü içerik alanında bulunan yerleştirilecek `shell` görünümü. Bu görünümler, genişletilmiş veya kendi özel görünümlerinizi ile değiştirilmiştir.

### <a name="footerhtml"></a>Footer.HTML

`footer.html` Altbilgisindeki sayfanın alt kısmında görüntülenen HTML içeren `shell` görünümü.

## <a name="viewmodels"></a>Viewmodel'lar

İçinde bulunan Viewmodel'lar `App/viewmodels` klasör.

### <a name="shelljs"></a>Shell.js

`shell` Viewmodel özellikleri ve bağlı işlevler içerir `shell` görünümü. Genellikle bu menü Gezinti bağlamaları bulunduğu, (bkz `router.mapNav` mantıksal).

[!code-javascript[Main](hottowel-template/samples/sample6.js)]

### <a name="homejs-and-detailsjs"></a>Home.js ve details.js

Bu viewmodel'lar bağlı işlevleri ve özellikleri içeren `home` görünümü. Ayrıca görünüm için sunu mantığı içeren ve veri ve görünüm arasında bir Yapıştırıcı işlevi görür.

[!code-javascript[Main](hottowel-template/samples/sample7.js)]

## <a name="services"></a>Hizmetler

Hizmetleri uygulama/hizmetleri klasöründe bulunur. İdeal olarak, gelecekteki hizmetlerinizi alma ve uzak veriler gönderilirken sorumlu olan dataservice modülü gibi yerleştirilebilir.

### <a name="loggerjs"></a>Logger.js

Hot Towel sağlayan bir `logger` Hizmetleri klasörü modülünde. `logger` Modülüdür günlük iletilerini konsola ve toasts açılır kullanıcıya için idealdir.
