---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
title: ASP.NET MVC giriş | Microsoft Docs
author: shanselman
description: ASP.NET MVC temelleri tanıtır bir başlangıç Öğreticisi budur. Okuyan ve yazan bir veritabanından basit bir web uygulaması oluşturun.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: bf4a1c19-0a94-4208-b268-a96ddcf26946
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
msc.type: authoredcontent
ms.openlocfilehash: 476d832e389b9b5a26fe2d552ca648c79b100056
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="intro-to-aspnet-mvc"></a>ASP.NET MVC giriş
====================
tarafından [Scott Hanselman](https://github.com/shanselman)

> > [!NOTE]
> > Bu öğretici kullanılabiliyorsa, güncelleştirilmiş bir sürümünü [burada](../../getting-started/introduction/getting-started.md) kullanarak [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads). Yeni öğretici Bu öğretici birçok iyileştirme sağlayan ASP.NET MVC 5 kullanır.
> 
> 
> ASP.NET MVC temelleri tanıtır bir başlangıç Öğreticisi budur. Okuyan ve yazan bir veritabanından basit bir web uygulaması oluşturacaksınız. Ziyaret [ASP.NET MVC öğrenme Merkezi](../../../index.md) diğer ASP.NET MVC öğreticiler ve örnekleri bulunamadı.


Bizim ilk ASP.NET MVC Web uygulaması kullanarak olalım [Visual Web Developer 2010 Express](https://www.microsoft.com/express/Web/). Biz, bize oluşturur ve filmler listesinde biraz film listesi uygulaması yapacağız.

## <a name="what-youll-build"></a>Ne oluşturacağınız

Burada, oluşturacağınız uygulamasının iki ekran görüntüleri verilmiştir. Film çeşitli sütunları içeren basit bir tablo sahip olur.

[![Film listesi - Windows Internet Explorer (12)](getting-started-with-mvc-part1/_static/image2.png)](getting-started-with-mvc-part1/_static/image1.png)

Ve filmler listesine ekleyebilmek için bir kayıt formunu sahip olacaksınız.

[![Windows Internet Explorer (2) bir filmi - oluşturun](getting-started-with-mvc-part1/_static/image4.png)](getting-started-with-mvc-part1/_static/image3.png)

## <a name="skills-youll-learn"></a>Bilgi edineceksiniz yetenekleri

Bu öğretici Visual Studio kullanarak bir ASP.NET MVC Web uygulaması oluşturmanın temellerini öğretmek. Şunları öğreneceksiniz:

- Yeni bir ASP.NET MVC projesi oluşturma
- SQL Server ile yeni bir veritabanı oluşturma
- ASP.NET MVC denetleyicileri ve görünümler oluşturma
- Nasıl alınacağını ve verileri görüntüle
- Verileri düzenleme ve veri doğrulama etkinleştirme
- Veritabanı şemasının güncelleştirme

## <a name="get-started"></a>Başlarken

Visual Web Developer 2010 (ı "VWD" şu andan itibaren çağrısından) Express ve yeni proje seçin Başlat ekranından çalıştırarak başlayın.

Visual Web Developer bir IDE veya tümleşik geliştirme ortamı değil. Belgeleri yazmak için Microsoft Word kullanma gibi uygulamaları oluşturmak için bir IDE kullanacaksınız. Bir araç çubuğu, yanı sıra de kullanmış seçin dosyasına menü için çeşitli seçenekleri gösteren üstünde yoktur | Yeni proje.

[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image6.png)](getting-started-with-mvc-part1/_static/image5.png)

## <a name="creating-your-first-application"></a>İlk uygulamanızı oluşturma

Visual Basic veya Visual C# kullanarak uygulama oluşturabilirsiniz. Şu an seçin Visual C# solda, ardından "ASP.NET MVC 2 Web uygulaması." seçin "Filmler" projenizi adlandırın ve Tamam'ı tıklatın.

[![New Project](getting-started-with-mvc-part1/_static/image8.png)](getting-started-with-mvc-part1/_static/image7.png)

Sağ tarafta tüm dosya ve klasörler, uygulamanızda gösteren Çözüm Gezgini ' dir. Büyük ortasında burada kodunuzu düzenleyin ve çoğu zaman, harcamanız penceredir. Visual Studio varsayılan bir şablon, yeni oluşturduğunuz, ASP.NET MVC proje için kullanılan çalışan bir uygulama şu anda herhangi bir şey yapmadan elinizde! Bu bir basit "Hello World! olur Proje ve uygulamamız için başlatmak için iyi bir yerdir.

[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image10.png)](getting-started-with-mvc-part1/_static/image9.png)

Araç çubuğuna "Çalıştır" düğmesini seçin.

![Hata ayıklama başlatılamıyor](getting-started-with-mvc-part1/_static/image11.png)

Programınızı derlemek ve uygulamanızı bir web tarayıcısında başlatın sağa gösteren yeşil ok olur.

*Not: Yerine klavyenizde F5'e basın veya için hata ayıklama - seçin&gt;hata ayıklamayı Başlat "Debug" menüsünden.*

Bu, bir geliştirme web sunucusu başlatmak ve (yapılandırma veya yok bunu etkinleştirmek için gereken adımları el ile) web uygulamamızı çalıştırmak Visual Web Developer neden olur. Ardından bir tarayıcıyı başlatacak ve uygulama giriş sayfası göz atmak için yapılandırın. Aşağıda tarayıcının adres çubuğunda "localhost" ve example.com gibi şeyler diyor dikkat edin. Localhost her zaman bu durumda yalnızca oluşturduğumuz uygulama çalışan kendi yerel bilgisayarına - işaret eden olmasıdır.

[![Giriş sayfası](getting-started-with-mvc-part1/_static/image13.png)](getting-started-with-mvc-part1/_static/image12.png)

Kutudan çıktığında bu varsayılan şablonu ziyaret etmek için iki sayfaları ve temel oturum açma sayfasına sunar. Şimdi bu uygulamayı nasıl çalıştığını değiştirin ve biraz işleminde ASP.NET MVC hakkında bilgi edinin. Tarayıcınızı kapatın ve bazı kodunu değiştirmek olanak tanır.

> [!div class="step-by-step"]
> [Next](getting-started-with-mvc-part2.md)
