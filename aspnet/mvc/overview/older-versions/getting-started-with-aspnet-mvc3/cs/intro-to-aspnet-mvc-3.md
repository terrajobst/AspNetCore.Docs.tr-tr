---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3
title: ASP.NET MVC 3 (C#) giriş | Microsoft Docs
author: Rick-Anderson
description: Bu öğreticide, Microsoft Visual Web Developer 2010 Express Service Pack, 1, kullanarak bir ASP.NET MVC Web uygulaması oluşturmaya yönelik temel bilgiler sağlanır...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 86a80b35-88bd-4b7c-bd58-f6e7997197d4
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3
msc.type: authoredcontent
ms.openlocfilehash: d4cda748638a6396750fe2ef7713dd0b2a4555d6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37379870"
---
<a name="intro-to-aspnet-mvc-3-c"></a>Giriş ASP.NET MVC 3 (C#)
====================
Tarafından [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Bu öğreticide güncelleştirilmiş bir sürümü kullanılabilir [burada](../../../getting-started/introduction/getting-started.md) ASP.NET MVC 5 ve Visual Studio 2013'ü kullanır. Bu, daha güvenli ve izlemek çok daha kolay ve daha fazla özelliklerini gösterir.
> 
> 
> Bu öğreticide, Microsoft Visual Web Developer 2010 Express Service Pack ücretsiz bir Microsoft Visual Studio sürümü olan 1, kullanarak bir ASP.NET MVC Web uygulaması oluşturmaya yönelik temel bilgiler sağlanır. Başlamadan önce aşağıda listelenen ön yüklediğiniz emin olun. Aşağıdaki bağlantıya tıklayarak bunların tümünü yükleyebilirsiniz: [Web Platformu yükleyicisi](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternatif olarak, aşağıdaki bağlantıları kullanarak önkoşulları ayrı ayrı yükleyebilirsiniz:
> 
> - [Visual Studio Web Developer Express SP1 önkoşulları](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 araçları güncelleştirme](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(çalışma zamanı + araçları desteği)
> 
> Visual Web Developer 2010 yerine Visual Studio 2010 kullanıyorsanız, aşağıdaki bağlantıyı tıklatarak önkoşulları yükleyin: [Visual Studio 2010 önkoşulları](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> C# kaynak kodu içeren bir Visual Web Developer proje, bu konuya eşlik etmek üzere kullanılabilir. [C# sürümü indirme](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Visual Basic tercih ederseniz, geçiş [Visual Basic sürümü](../vb/intro-to-aspnet-mvc-3.md) Bu öğreticinin.


## <a name="what-youll-build"></a>Ne oluşturacaksınız

Oluşturma, düzenleme ve veritabanı filmler listeleme destekleyen basit bir film listeleme uygulama uygulayacaksınız. Aşağıda oluşturacağınız uygulama iki ekran görüntüleri verilmiştir. Film veritabanı listesini görüntüleyen bir sayfa içerir:

![MoviesWithVariousSm](intro-to-aspnet-mvc-3/_static/image1.png)

Uygulama, ekleme, düzenleme ve tek tek olanları hakkında ayrıntılara bakın yanı sıra, filmler Sil da sağlar. Tüm veri girişi senaryolar veritabanında depolanan verileri doğru olduğundan emin olmak için doğrulama içerir.

![](intro-to-aspnet-mvc-3/_static/image2.png)

## <a name="skills-youll-learn"></a>Beceriler hakkında bilgi edineceksiniz

Öğrenecekleriniz aşağıda verilmiştir:

- Yeni bir ASP.NET MVC projesi oluşturma
- ASP.NET MVC denetleyicileri ve görünümleri oluşturma
- Entity Framework Code First paradigması kullanarak yeni bir veritabanı oluşturma
- Nasıl alınacağını ve verileri görüntüle.
- Verileri düzenleme ve veri doğrulama etkinleştirme hakkında.

## <a name="getting-started"></a>Başlarken

Visual Web Developer 2010 Express'in ("Visual Web Developer" kısaca) çalıştırarak ve seçin **yeni proje** gelen **Başlat** sayfası.

Visual Web Developer, bir IDE ya da tümleşik geliştirme ortamı yöneliktir. Belgeler yazmak için Microsoft Word kullanma gibi bir IDE uygulamalar oluşturmak için kullanırsınız. Visual Web Developer kullanarak bir araç çubuğu için çeşitli seçenekler kullanılabilir gösteren üstünde yoktur. IDE'de görevleri gerçekleştirmek için başka bir yol sağlayan bir menüsünde de mevcuttur. (Örneğin seçmek yerine **yeni proje** gelen **Başlat** sayfasında menüsünü kullanın ve seçin **dosya** &gt; **YeniProje**.)

[![](intro-to-aspnet-mvc-3/_static/image4.png)](intro-to-aspnet-mvc-3/_static/image3.png)

## <a name="creating-your-first-application"></a>İlk uygulamanızı oluşturma

Programlama dili olarak Visual Basic veya Visual C# kullanarak uygulamalar oluşturabilirsiniz. Visual C# sol tarafta'i seçin ve ardından **ASP.NET MVC 3, Web uygulaması**. "MvcMovie" projenizi adlandırın ve ardından **Tamam**. (Visual Basic tercih ederseniz, geçiş [Visual Basic sürümü](../vb/intro-to-aspnet-mvc-3.md) Bu öğreticinin.)

![](intro-to-aspnet-mvc-3/_static/image5.png)

İçinde **yeni ASP.NET MVC 3 projesini** iletişim kutusunda **Internet uygulaması**. Denetleme **kullanımı HTML5 biçimlendirme** bırakıp **Razor** varsayılan görünüm altyapısı olarak.

![](intro-to-aspnet-mvc-3/_static/image6.png)

**Tamam**'ı tıklatın. Çalışan bir uygulama şu anda hiçbir şey yapmadan sahip olduğunuz visual Web Developer az önce oluşturduğunuz bir ASP.NET MVC projesi için varsayılan bir şablon kullanılan! Bu, bir basit "Hello World!" Proje ve kullanıcının uygulamanızı başlatmak için iyi bir yerdir.

[![](intro-to-aspnet-mvc-3/_static/image8.png)](intro-to-aspnet-mvc-3/_static/image7.png)

Gelen **hata ayıklama** menüsünde **hata ayıklamayı Başlat**.

![](intro-to-aspnet-mvc-3/_static/image9.png)

Klavye kısayolu hata ayıklamayı Başlat F5 olduğuna dikkat edin.

F5'e bir geliştirme web sunucusunu başlatmak ve web uygulamanızı çalıştırmak Visual Web Developer neden olur. Visual Web Developer, ardından bir tarayıcı başlatır ve uygulamanın giriş sayfası açılır. Tarayıcının adres çubuğunda yazılı bildirimi `localhost` gibi bir şey `example.com`. Çünkü `localhost` her zaman bu durumda yeni oluşturduğunuz uygulamayı çalıştıran kendi yerel bilgisayara işaret eder. Visual Web Developer bir web projesi çalıştığında, web sunucusu için rastgele bir bağlantı noktası kullanılır. Aşağıdaki görüntüde, 43246 rastgele bağlantı noktası numarasıdır. Uygulamayı çalıştırdığınızda, büyük olasılıkla farklı bir bağlantı noktası görürsünüz.

![](intro-to-aspnet-mvc-3/_static/image10.png)

Kullanıma hazır bu varsayılan şablonu ziyaret etmek için iki sayfa ve bir temel oturum açma sayfası sunar. Bu uygulamanın nasıl çalıştığını değiştirmek ve biraz işlemde ASP.NET MVC hakkında bilgi edinmek için sonraki adımdır bakın. Tarayıcınızı kapatın ve bazı kod değiştirelim.

> [!div class="step-by-step"]
> [Next](adding-a-controller.md)
