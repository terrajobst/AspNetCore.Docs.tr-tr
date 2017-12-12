---
uid: mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
title: "NerdDinner Öğreticisi giriş | Microsoft Docs"
author: shanselman
description: "Yeni bir çerçeve öğrenmenin en iyi yolu onunla bir şeyler oluşturmaktır. Bu öğreticide ASP.NE kullanarak küçük, ancak tam, bir uygulama oluşturmak nasıl aracılığıyla açıklanmaktadır..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 397522d5-0402-4b94-b810-a2fb564f869d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
msc.type: authoredcontent
ms.openlocfilehash: 57eedb224e26867c78cc399b89f91b95f722074d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="introducing-the-nerddinner-tutorial"></a>NerdDinner Öğreticisi Tanıtımı
====================
tarafından [Scott Hanselman](https://github.com/shanselman)

[PDF indirin](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Yeni bir çerçeve öğrenmenin en iyi yolu onunla bir şeyler oluşturmaktır. Bu öğretici, küçük bir yapı ancak tamamlanması, ASP.NET MVC 1'i kullanarak uygulama nasıl aracılığıyla anlatılmaktadır ve bazı arkasındaki temel kavramları tanıtır.
> 
> ASP.NET MVC 3 kullanıyorsanız, izlemeniz önerilir [MVC 3 ile çalışmaya başlama](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) veya [MVC müzik deposu](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) öğreticileri.


## <a name="nerddinner-tutorial"></a>NerdDinner Öğreticisi

Yeni bir çerçeve öğrenmenin en iyi yolu onunla bir şeyler oluşturmaktır. Bu öğretici, küçük bir yapı ancak tamamlanması, ASP.NET MVC kullanarak uygulamayı nasıl aracılığıyla anlatılmaktadır ve bazı arkasındaki temel kavramları tanıtır.

Biz oluşturmak için uygulama "NerdDinner" adı verilir. NerdDinner bulmak ve çevrimiçi azalma düzenlemek kişiler için kolay bir yol sağlar:

![](introducing-the-nerddinner-tutorial/_static/image1.png)

NerdDinner kayıtlı kullanıcıların oluşturmak, düzenlemek ve azalma silmek olanak tanır. Doğrulama ve iş kuralları tutarlı bir dizi uygulama zorunlu kılar:

![](introducing-the-nerddinner-tutorial/_static/image2.png)

Ziyaretçilerin bunları tutulan yaklaşan azalma aramak için bir AJAX tabanlı eşlemesi kullanabilirsiniz:

![](introducing-the-nerddinner-tutorial/_static/image3.png)

Bir Yemeği tıklayarak bunları ayrıntıları sayfasına nereden öğrenebilir hakkında daha fazla sürer:

![](introducing-the-nerddinner-tutorial/_static/image4.png)

Bunlar Yemeği katılan ilgileniyorsanız oturum açamaz veya sitesinde kaydedin:

![](introducing-the-nerddinner-tutorial/_static/image5.png)

Bunlar daha sonra olay katılmak için bir AJAX tabanlı RSVP bağlantıyı tıklatın:

![](introducing-the-nerddinner-tutorial/_static/image6.png)

![](introducing-the-nerddinner-tutorial/_static/image7.png)

### <a name="implementing-nerddinner"></a>NerdDinner uygulama

Dosya - kullanarak NerdDinner uygulamamız başlamaya kalacaklarını&gt;yepyeni bir ASP.NET MVC projesini oluşturmak için Visual Studio içindeki yeni proje komutu. Ardından artımlı olarak işlev ve özellikleri ekleyeceğiz. Yol boyunca şu konulara değineceğiz:

1. [Yeni bir ASP.NET MVC projesi oluşturmak nasıl](# "yeni bir ASP.NET MVC projesi oluşturma")
2. [Bir veritabanı oluşturmak nasıl](# "bir veritabanı oluşturun")
3. [İş kuralı doğrulamaları ile bir model oluşturmak nasıl](# "iş kuralı doğrulamaları ile bir Model oluşturma")
4. [Denetleyicileri ve görünümleri listeleme/Ayrıntılar UI uygulamak için nasıl kullanılacağını](# "denetleyicileri kullanın ve görünümleri listeleme/Ayrıntılar UI uygulamak için")
5. [CRUD sağlamak nasıl (oluşturma, okuma, güncelleştirme, silme) veri form girişi Destek](# "sağlamak CRUD (oluşturma, okuma, güncelleştirme, silme) veri Form girişini destekler")
6. [ViewData kullanın ve ViewModel sınıfları uygulamak nasıl](# "ViewData kullanın ve uygulama ViewModel sınıfları")
7. [Ana sayfalar ve kısmi kullanarak kullanıcı Arabirimi yeniden kullanmak nasıl](# "ana sayfa kullanarak kullanıcı Arabirimi yeniden kullanma ve kısmi")
8. [Verimli veri Sayfalaması uygulamak üzere nasıl](# "uygulamak verimli veri disk belleği")
9. [Kimlik doğrulama ve yetkilendirme kullanarak uygulamaların güvenliğini sağlamak nasıl](# "güvenli uygulamaları kullanarak kimlik doğrulaması ve yetkilendirme")
10. [AJAX dinamik güncelleştirmelerini göndermek için nasıl kullanılacağını](# "dinamik güncelleştirmeleri sunmak için AJAX'ı kullanın")
11. [AJAX eşleme senaryolar uygulamak için nasıl kullanılacağını](# "eşleme senaryolar uygulamak için AJAX'ı kullanın")
12. [Otomatik birim testi etkinleştirme](# "otomatik birim testi etkinleştir")

Kendi NerdDinner kopyasını oluşturabilirsiniz her tamamlayarak sıfırdan adım Biz bu bölümdeki gözden geçirme. Alternatif olarak, kaynak kodu buraya tamamlanmış bir sürümünü yükleyebilirsiniz: [github'da NerdDinner](https://github.com/AspNetMVPSamples/NerdDinner). Ayrıca isteğe bağlı olarak kullanabileceğiniz [Bu öğretici ücretsiz bir PDF sürümünü yükleyin](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf) öğretici çevrimdışı okumak istiyorsanız.

Uygulamanızı oluşturmak için Visual Studio 2008 veya ücretsiz Visual Web Developer 2008 Express kullanabilirsiniz. Veritabanı için SQL Server veya ücretsiz SQL Server Express kullanabilirsiniz.

ASP.NET MVC, Visual Web Developer 2008 Express ve SQL Server, V2 kullanarak Express (tüm ücretsiz) yükleyebilirsiniz [Microsoft Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx)

### <a name="now-lets-get-started"></a>Şimdi başlayalım...

Biz NerdDinner nedir kapsamına, şimdi bizim kollarýný attıysanız ve biraz kod yazalım.

Biz dosya - kullanarak başlarsınız&gt;NerdDinner uygulaması oluşturmak için Visual Studio'da yeni proje.

>[!div class="step-by-step"]
[Sonraki](create-a-new-aspnet-mvc-project.md)
