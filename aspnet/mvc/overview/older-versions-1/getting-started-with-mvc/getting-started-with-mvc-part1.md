---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
title: ASP.NET MVC'ye giriş | Microsoft Docs
author: shanselman
description: ASP.NET MVC ile ilgili temel bilgileri tanıtan bir başlangıç Öğreticisi budur. Okuyan ve yazan bir veritabanından basit bir web uygulaması oluşturun.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: bf4a1c19-0a94-4208-b268-a96ddcf26946
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
msc.type: authoredcontent
ms.openlocfilehash: 2d9c1dd0dd3c9f892b42b0f29ac3361a7f2b638c
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48910869"
---
<a name="intro-to-aspnet-mvc"></a>ASP.NET MVC'ye giriş
====================
tarafından [Scott Hanselman](https://github.com/shanselman)

> > [!NOTE]
> > Bu öğreticide kullanılabiliyorsa, güncelleştirilmiş bir sürümünü [burada](../../getting-started/introduction/getting-started.md) kullanarak [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013). Bu öğretici birçok iyileştirme sağlayan ASP.NET MVC 5 yeni öğretici kullanır.
>
>
> ASP.NET MVC ile ilgili temel bilgileri tanıtan bir başlangıç Öğreticisi budur. Okuyan ve yazan bir veritabanından basit bir web uygulaması oluşturacaksınız. Ziyaret [ASP.NET MVC eğitim Merkezi](../../../index.md) diğer ASP.NET MVC, öğreticilerimiz ve örneklerimizden bulunacak.


Bizim ilk ASP.NET MVC Web uygulaması kullanarak olalım [Visual Web Developer 2010 Express'i](https://www.microsoft.com/express/Web/). Bize oluşturun ve filmler listesinde biraz film listesi uygulaması oluşturacağız.

## <a name="what-youll-build"></a>Ne oluşturacaksınız

Aşağıda oluşturacağınız uygulama iki ekran görüntüleri verilmiştir. Film çeşitli sütunları içeren basit bir tablo gerekir.

[![Film listesi - Windows Internet Explorer (12)](getting-started-with-mvc-part1/_static/image2.png)](getting-started-with-mvc-part1/_static/image1.png)

Ve filmler listesine ekleyebilirsiniz oluşturma Form sahip olacaksınız.

[![Bir filmi - Windows Internet Explorer (2) oluşturma](getting-started-with-mvc-part1/_static/image4.png)](getting-started-with-mvc-part1/_static/image3.png)

## <a name="skills-youll-learn"></a>Beceriler hakkında bilgi edineceksiniz

Bu öğreticide Visual Studio kullanarak bir ASP.NET MVC Web uygulaması oluşturmaya yönelik temel bilgiler sağlanır. Şunları öğreneceksiniz:

- Yeni bir ASP.NET MVC projesi oluşturma
- SQL Server ile yeni bir veritabanı oluşturma
- ASP.NET MVC denetleyicileri ve görünümleri oluşturma
- Nasıl alınacağını ve verileri görüntüle
- Verileri düzenleme ve veri doğrulamasını etkinleştirme
- Veritabanı şemasını güncelleştirme

## <a name="get-started"></a>Başlarken

Visual Web Developer 2010 (ben bunu "VWD" andan itibaren ararız) Express ve yeni bir proje seçin, başlangıç ekranından çalıştırarak başlayın.

Visual Web Developer, bir IDE veya tümleşik Geliştirici Ortamı ' dir. Belgeler yazmak için Microsoft Word kullanma gibi bir IDE uygulamalar oluşturmak için kullanırsınız. Araç, hem de de kullanmış dosyasına seçin menü için çeşitli seçenekler kullanılabilir gösteren üstünde yok | Yeni bir proje.

[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image6.png)](getting-started-with-mvc-part1/_static/image5.png)

## <a name="creating-your-first-application"></a>İlk uygulamanızı oluşturma

Visual Basic veya Visual C# kullanarak uygulamalar oluşturabilirsiniz. Şimdilik seçin Visual C# solda, ardından "ASP.NET MVC 2 Web uygulaması." seçin "Film" projenizi adlandırın ve Tamam'a tıklayın.

[![Yeni Proje](getting-started-with-mvc-part1/_static/image8.png)](getting-started-with-mvc-part1/_static/image7.png)

Sağ tarafta tüm dosya ve klasörlerin uygulamanızda gösteren Çözüm Gezgini ' dir. Büyük ortada, kodunuzu düzenleyin ve zamanınızın çoğunu harcama penceredir. Çalışan bir uygulama şu anda hiçbir şey yapmadan sahip olduğunuz visual Studio ASP.NET MVC projesi için az önce oluşturduğunuz varsayılan bir şablon kullanılan! Bu, bir basit "Merhaba Dünya! Proje ve uygulamamız için başlatmak için iyi bir yerdir.

[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image10.png)](getting-started-with-mvc-part1/_static/image9.png)

Araç çubuğu "yürütme" düğmesini seçin.

![Hata Ayıklamayı Başlat](getting-started-with-mvc-part1/_static/image11.png)

Bu, programı derleyin ve uygulamanızı bir web tarayıcısında başlatın sağ işaret eden yeşil bir ok olur.

*Not: Bunun yerine klavyenizde F5 tuşuna basın veya yapabilirsiniz hata ayıklama - seçin&gt;hata ayıklamaya Başla "Debug" menüsünden.*

Bu, bir geliştirme web sunucusunu başlatmak ve web uygulamamıza (yapılandırma veya yoktur bunu etkinleştirmek için gerekli adımları el ile) çalıştırmak Visual Web Developer neden olur. Ardından bir tarayıcıyı başlatın ve uygulamanın giriş sayfası göz atmak için yapılandırmanız. Aşağıda tarayıcının adres çubuğuna "localhost" ve example.com şöyle diyor dikkat edin. Bu durumda yalnızca oluşturulan uygulama çalıştıran kendi yerel bilgisayar için - her zaman localhost işaret eder olmasıdır.

[![Giriş sayfası](getting-started-with-mvc-part1/_static/image13.png)](getting-started-with-mvc-part1/_static/image12.png)

Kullanıma hazır, bu varsayılan şablonu ziyaret etmek için iki sayfa ve bir temel oturum açma sayfası sunar. Şimdi bu uygulamayı nasıl çalıştığını değiştirmek ve biraz işlemde ASP.NET MVC hakkında bilgi edinin. Tarayıcınızı kapatın ve bazı kod değiştirme olanak tanır.

> [!div class="step-by-step"]
> [Next](getting-started-with-mvc-part2.md)
