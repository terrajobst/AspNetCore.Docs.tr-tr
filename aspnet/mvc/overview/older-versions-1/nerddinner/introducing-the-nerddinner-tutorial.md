---
uid: mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
title: NerdDinner Öğreticisine giriş | Microsoft Docs
author: shanselman
description: Yeni bir çerçeve öğrenmenin en iyi yolu olan bir şey oluşturmaktır. Bu öğreticide ASP.NE kullanarak küçük ancak tam bir uygulama oluşturma hakkında bilgi vermektedir...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 397522d5-0402-4b94-b810-a2fb564f869d
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
msc.type: authoredcontent
ms.openlocfilehash: d5efab525841b5c526aa3b656f27b1c42cc74648
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753569"
---
<a name="introducing-the-nerddinner-tutorial"></a>NerdDinner Öğreticisine giriş
====================
tarafından [Scott Hanselman](https://github.com/shanselman)

[PDF'yi indirin](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Yeni bir çerçeve öğrenmenin en iyi yolu olan bir şey oluşturmaktır. Bu öğretici, küçük bir derleme, ancak tamamlamak, ASP.NET MVC 1'i kullanarak uygulama hakkında bilgi vermektedir ve bazı ardındaki temel kavramları tanıtır.
> 
> ASP.NET MVC 3 kullanıyorsanız, takip ettiğiniz öneririz [MVC 3 ile çalışmaya başlama](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) veya [MVC müzik Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) öğreticiler.


## <a name="nerddinner-tutorial"></a>NerdDinner Öğreticisine

Yeni bir çerçeve öğrenmenin en iyi yolu olan bir şey oluşturmaktır. Bu öğretici, küçük bir derleme, ancak tamamlamak, ASP.NET MVC kullanarak uygulama hakkında bilgi vermektedir ve bazı ardındaki temel kavramları tanıtır.

Uygulama oluşturmak için kullanacağız "NerdDinner" adı verilir. NerdDinner çevrimiçi azalma bulun ve bu kişiler için kolay bir yol sağlar:

![](introducing-the-nerddinner-tutorial/_static/image1.png)

NerdDinner kayıtlı kullanıcıların oluşturma, düzenleme ve silme azalma sağlar. Uygulamanın tamamında tutarlı özellik kümesi doğrulama ve iş kuralları zorunlu kılar:

![](introducing-the-nerddinner-tutorial/_static/image2.png)

Ziyaretçiler, bunları tutulmakta yaklaşan azalma aramak için bir AJAX tabanlı eşleme kullanabilirsiniz:

![](introducing-the-nerddinner-tutorial/_static/image3.png)

Bir Akşam Yemeği tıklayarak bunları ayrıntıları sayfasına nereden öğrenebilir hakkında daha fazla sürer:

![](introducing-the-nerddinner-tutorial/_static/image4.png)

Bunlar dinner katılan içinde ilgileniyorsanız oturum açma veya sitesine kaydolun:

![](introducing-the-nerddinner-tutorial/_static/image5.png)

Bunlar, ardından etkinliğine katılın için bir AJAX tabanlı RSVP bağlantısına tıklayabilirsiniz:

![](introducing-the-nerddinner-tutorial/_static/image6.png)

![](introducing-the-nerddinner-tutorial/_static/image7.png)

### <a name="implementing-nerddinner"></a>NerdDinner uygulama

Dosya - kullanarak NerdDinner uygulamamız başlamak kullanacağız&gt;yepyeni bir ASP.NET MVC projesi oluşturmak için Visual Studio'da yeni proje komutu. Ardından artımlı olarak işlevleri ve özellikleri ekleyeceğiz. Süreç boyunca şu konulara değineceğiz:

1. [Yeni bir ASP.NET MVC projesi oluşturma işlemini](# "yeni bir ASP.NET MVC projesi oluşturma")
2. [Bir veritabanı oluşturmak nasıl](# "veritabanı oluşturma")
3. [İş kuralı doğrulamaları ile model oluşturma konusunda](# "iş kuralı doğrulamaları ile Model oluşturma")
4. [Denetleyicileri ve görünümleri kullanarak listeleme/Ayrıntılar kullanıcı Arabirimi gerçekleştirmeyi nasıl](# "kullanın denetleyicileri ve görünümleri listeleme/Ayrıntılar kullanıcı Arabirimi uygulama")
5. [Nasıl CRUD (oluşturma, okuma, güncelleştirme ve silme) veri formu giriş desteği](# "sağlamak CRUD (oluşturma, okuma, güncelleştirme, silme) veri formu giriş desteği")
6. [ViewData kullanma ve ViewModel sınıfları uygulama](# "ViewData kullanma ve ViewModel sınıfları uygulama")
7. [Ana sayfaları ve kısmi bölümleri kullanarak kullanıcı arabirimini yeniden kullanma](# "ana sayfaları kullanarak kullanıcı arabirimini yeniden kullanma ve kısmi bölümleri")
8. [Verimli veri sayfalama uygulama nasıl](# "uygulamak verimli veri sayfalama")
9. [Kimlik doğrulama ve yetkilendirme kullanarak uygulamaların güvenliğini nasıl](# "güvenli uygulamaları kullanarak kimlik doğrulaması ve yetkilendirme")
10. [AJAX dinamik güncelleştirmeler sunma konusunda](# "dinamik güncelleştirmelerini iletmek için AJAX kullanın")
11. [AJAX kullanarak eşleme senaryoları gerçekleştirmeyi nasıl](# "kullanan AJAX eşleme senaryoları uygulama")
12. [Otomatik birim testi etkinleştirme](# "otomatik birim testi etkinleştir")

NerdDinner kopyasına oluşturabileceğinizi her tamamlayarak sıfırdan adım Biz bu bölümdeki gözden geçirme. Alternatif olarak, kaynak kodu buraya tamamlanmış bir sürümünü indirebilirsiniz: [github'da NerdDinner](https://github.com/AspNetMVPSamples/NerdDinner). Ayrıca isteğe bağlı olarak ayrıca [Bu öğretici ücretsiz bir PDF sürümünü indirin](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf) öğretici çevrimdışı okumak istiyorsanız.

Uygulamayı oluşturmak için Visual Studio 2008 ya da ücretsiz Visual Web Developer 2008 Express kullanabilirsiniz. Veritabanı için SQL Server veya ücretsiz SQL Server Express kullanabilirsiniz.

ASP.NET MVC, Visual Web Developer 2008 Express ve SQL Server'ın V2 kullanarak Express (tüm ücretsiz) yükleyebilirsiniz [Microsoft Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx)

### <a name="now-lets-get-started"></a>Şimdi başlayalım...

NerdDinner nedir ele aldığımız, şimdi bizim kollarınızı sıvayın ve biraz kod yazalım.

Şu dosya - kullanarak başlarsınız&gt;NerdDinner uygulama oluşturmak için Visual Studio'da yeni proje.

> [!div class="step-by-step"]
> [Next](create-a-new-aspnet-mvc-project.md)
