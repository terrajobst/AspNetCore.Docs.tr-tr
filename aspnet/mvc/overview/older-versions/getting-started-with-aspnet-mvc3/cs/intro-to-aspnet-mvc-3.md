---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3
title: ASP.NET MVC 3 (C#) giriş | Microsoft Docs
author: Rick-Anderson
description: Bu öğretici Microsoft Visual Web Developer 2010 Express Service Pack olan 1, kullanarak bir ASP.NET MVC Web uygulaması oluşturmanın temellerini öğretmek...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 86a80b35-88bd-4b7c-bd58-f6e7997197d4
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3
msc.type: authoredcontent
ms.openlocfilehash: 9b965df6175051a084de35627160161c116be42d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="intro-to-aspnet-mvc-3-c"></a>Giriş ASP.NET MVC 3 (C#)
====================
tarafından [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Bu öğretici güncelleştirilmiş bir sürümü kullanılabilir [burada](../../../getting-started/introduction/getting-started.md) ASP.NET MVC 5 ve Visual Studio 2013'ü kullanır. Daha güvenli, izlemek çok daha kolaydır ve daha fazla özelliklerini gösterir.
> 
> 
> Bu öğretici Microsoft Visual Web Developer 2010 Express Service Pack ücretsiz Microsoft Visual Studio sürümünü olan 1, kullanarak bir ASP.NET MVC Web uygulaması oluşturmanın temellerini öğretmek. Başlamadan önce aşağıda listelenen önkoşulları kurduğunuzdan emin olun. Bunların tümünün aşağıdaki bağlantıyı tıklatarak yükleyin: [Web Platformu yükleyicisi](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternatif olarak, aşağıdaki bağlantıları kullanarak önkoşulları ayrı ayrı yükleyebilirsiniz:
> 
> - [Visual Studio Web Developer Express SP1 önkoşulları](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 araçları güncelleştirme](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(çalışma zamanı + araçları destekler)
> 
> Visual Web Developer 2010 yerine Visual Studio 2010 kullanıyorsanız, aşağıdaki bağlantıyı tıklatarak önkoşulları yükleyin: [Visual Studio 2010 önkoşulları](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> C# kaynak kodu ile Visual Web Developer projesi bu konuya eşlik etmek kullanılabilir. [C# sürümü](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Visual Basic tercih ederseniz, geçiş [Visual Basic sürüm](../vb/intro-to-aspnet-mvc-3.md) Bu öğreticinin.


## <a name="what-youll-build"></a>Ne oluşturacağınız

Oluşturma, düzenleme ve veritabanından filmler listeleme destekleyen basit bir film listesi uygulaması uygulamanız. Aşağıda oluşturacağınız uygulamasının iki ekran görüntüleri verilmiştir. Veritabanından filmler listesini görüntüleyen bir sayfa içerir:

![MoviesWithVariousSm](intro-to-aspnet-mvc-3/_static/image1.png)

Uygulaması, ekleme, düzenleme ve tek tek olanları hakkında ayrıntılara bakın yanı sıra, filmler silme sağlar. Tüm veri girişi senaryolar veritabanında depolanan verileri doğru olduğundan emin olmak için doğrulama içerir.

![](intro-to-aspnet-mvc-3/_static/image2.png)

## <a name="skills-youll-learn"></a>Bilgi edineceksiniz yetenekleri

Öğrenecekleriniz aşağıda verilmiştir:

- Yeni bir ASP.NET MVC projesi oluşturma
- ASP.NET MVC denetleyicileri ve görünümler oluşturma
- Entity Framework Code First modeli kullanarak yeni bir veritabanı oluşturmak nasıl.
- Nasıl alınacağını ve verileri görüntüle.
- Verileri düzenlemek ve veri doğrulamasını etkinleştirmek nasıl.

## <a name="getting-started"></a>Başlarken

Visual Web Developer 2010 Express ("Visual Web Developer" kısaca) çalıştırarak ve seçin **yeni proje** gelen **Başlat** sayfası.

Visual Web Developer bir IDE veya tümleşik geliştirme ortamını değil. Belgeleri yazmak için Microsoft Word kullanma gibi uygulamaları oluşturmak için bir IDE kullanacaksınız. Visual Web Developer ile bir araç çubuğu size çeşitli seçenekleri gösteren üstünde yoktur. IDE içinde görevleri gerçekleştirmek için başka bir yol sağlayan bir menüsünü yoktur. (Örneğin, seçmek yerine **yeni proje** gelen **Başlat** sayfasında, menüsünü kullanın ve seçin **dosya** &gt; **yeni proje**.)

[![](intro-to-aspnet-mvc-3/_static/image4.png)](intro-to-aspnet-mvc-3/_static/image3.png)

## <a name="creating-your-first-application"></a>İlk uygulamanızı oluşturma

Visual Basic veya Visual C# programlama dili olarak kullanan uygulamalar oluşturabilirsiniz. Visual C# sol tarafta'i seçin ve ardından **ASP.NET MVC 3 Web uygulaması**. Projeniz "MvcMovie" olarak adlandırın ve ardından **Tamam**. (Visual Basic tercih ederseniz, geçiş [Visual Basic sürüm](../vb/intro-to-aspnet-mvc-3.md) Bu öğreticinin.)

![](intro-to-aspnet-mvc-3/_static/image5.png)

İçinde **yeni ASP.NET MVC 3 projesinin** iletişim kutusunda **Internet uygulama**. Denetleme **kullanımı HTML5 biçimlendirme** bırakıp **Razor** varsayılan görünüm altyapısı olarak.

![](intro-to-aspnet-mvc-3/_static/image6.png)

**Tamam**'ı tıklatın. Visual Web Developer varsayılan bir şablon, yeni oluşturduğunuz, ASP.NET MVC proje için kullanılan çalışan bir uygulama şu anda herhangi bir şey yapmadan elinizde! Bu bir basit "Hello World!" olur Proje ve kullanıcının uygulamanızı başlatmak için uygun bir yerdir.

[![](intro-to-aspnet-mvc-3/_static/image8.png)](intro-to-aspnet-mvc-3/_static/image7.png)

Gelen **hata ayıklama** menüsünde, select **hata ayıklamayı Başlat**.

![](intro-to-aspnet-mvc-3/_static/image9.png)

Klavye kısayolu hata ayıklamayı başlatmak için F5 olduğuna dikkat edin.

F5 geliştirme web sunucusu başlatmak ve web uygulamanızı çalıştırmak Visual Web Developer neden olur. Visual Web Developer bir tarayıcı başlatılır ve uygulamanın giriş sayfasını açar. Tarayıcının adres çubuğunda diyor bildirimi `localhost` bir şey yok gibi ve `example.com`. Çünkü `localhost` her zaman, bu durumda yalnızca yerleşik uygulama çalışan kendi yerel bilgisayara gösterir. Visual Web Developer bir web projesi çalıştığında, web sunucusu için rastgele bir bağlantı noktası kullanılır. Aşağıdaki resimde 43246 rastgele bağlantı noktası numarasıdır. Uygulamayı çalıştırdığınızda, farklı bir bağlantı noktası büyük olasılıkla görürsünüz.

![](intro-to-aspnet-mvc-3/_static/image10.png)

Kullanıma hazır bu varsayılan şablonu ziyaret etmek için iki sayfa ve temel oturum açma sayfasına sunar. Bu uygulama şeklini değiştirmek ve biraz işleminde ASP.NET MVC hakkında bilgi almak için sonraki adımdır bakın. Tarayıcınızı kapatın ve biraz kod değiştirelim.

> [!div class="step-by-step"]
> [Next](adding-a-controller.md)
