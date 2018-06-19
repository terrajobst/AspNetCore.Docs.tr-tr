---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4
title: ASP.NET MVC 4 giriş | Microsoft Docs
author: Rick-Anderson
description: Bu öğretici, burada Visual Studio 2013 kullanarak kullanılabiliyorsa, güncelleştirilmiş bir sürüm. Yeni öğretici t birçok iyileştirme sağlayan ASP.NET MVC 5 kullanır...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2012
ms.topic: article
ms.assetid: ed66530a-04d5-49eb-b76a-85be1f57c437
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 519bac22ba2607931c5f3123b9b567859a2b3d1c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869055"
---
<a name="intro-to-aspnet-mvc-4"></a>ASP.NET MVC 4 giriş
====================
tarafından [Rick Anderson](https://github.com/Rick-Anderson)

> Bu öğretici kullanılabiliyorsa, güncelleştirilmiş bir sürümünü [burada](../../getting-started/introduction/getting-started.md) kullanarak [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads). Yeni öğretici Bu öğretici birçok iyileştirme sağlayan ASP.NET MVC 5 kullanır.
> 
> Bu öğretici Microsoft kullanarak bir ASP.NET MVC 4 Web uygulaması oluşturmanın temellerini öğretmek [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) veya Visual Web Developer 2010 Express Service Pack 1. Visual Studio 2012 önerilir öğreticiyi tamamlamak için herhangi bir şey yüklemeniz gerekmez. Visual Studio 2010 kullanıyorsanız, aşağıdaki bileşenleri yüklemeniz gerekir. Bunların tümünün aşağıdaki bağlantılara tıklayarak yükleyebilirsiniz:
> 
> - [Visual Studio Web Developer Express SP1 önkoşulları](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 4 için WPI yükleyicisi](https://go.microsoft.com/fwlink/?LinkId=243392)
> - [LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0)
> - [SSDT](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx)
> 
> Visual Web Developer 2010 yerine Visual Studio 2010 kullanıyorsanız, yükleme [WPI yükleyici ASP.NET MVC 4 için](https://go.microsoft.com/fwlink/?LinkId=243392) ve: [Visual Studio 2010 önkoşulları](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)
> 
> C# kaynak kodu ile Visual Web Developer projesi bu konuya eşlik etmek kullanılabilir. [C# sürümü](https://code.msdn.microsoft.com/Intro-to-ASPNET-MVC-4-61d0219d/file/114480/1/MvcMovie.zip).
> 
> Öğreticide uygulamayı Visual Studio'da çalıştırın. Ayrıca uygulama kullanılabilir Internet üzerinden bir barındırma sağlayıcısına dağıtarak yapabilirsiniz. Microsoft, 10 web siteleri için ücretsiz bir web barındırma sunar bir [Ücretsiz Windows Azure deneme sürümü hesabı](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604). Windows Azure Web sitesi için bir Visual Studio web projesi dağıtma hakkında daha fazla bilgi için bkz: [oluşturma ve bir ASP.NET web sitesi ve Visual Studio ile SQL veritabanı dağıtma](https://docs.microsoft.com/dotnet/azure/). Bu öğretici, ayrıca, SQL Server veritabanınızın Windows Azure SQL veritabanına (önceki adıyla SQL Azure) dağıtmak için Entity Framework Code First Migrations kullanmayı gösterir.
> 
> Bu öğretici Rick Anderson tarafından yazılan ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ).


## <a name="what-youll-build"></a>Ne oluşturacağınız

> [!NOTE]
> Bu öğretici kullanılabiliyorsa, güncelleştirilmiş bir sürümünü [burada](../../getting-started/introduction/getting-started.md) kullanarak [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads). Yeni öğretici Bu öğretici birçok iyileştirme sağlayan ASP.NET MVC 5 kullanır.


Oluşturma, düzenleme, arama ve veritabanından filmler listeleme destekleyen basit bir film listesi uygulaması uygulamanız. Aşağıda oluşturacağınız uygulamasının iki ekran görüntüleri verilmiştir. Veritabanından filmler listesini görüntüleyen bir sayfa içerir:

![](intro-to-aspnet-mvc-4/_static/image1.png)

Uygulaması, ekleme, düzenleme ve tek tek olanları hakkında ayrıntılara bakın yanı sıra, filmler silme sağlar. Tüm veri girişi senaryolar veritabanında depolanan verileri doğru olduğundan emin olmak için doğrulama içerir.

![](intro-to-aspnet-mvc-4/_static/image2.png)

## <a name="getting-started"></a>Başlarken

Visual Studio Express 2012 veya Visual Web Developer 2010 Express çalıştırarak başlayın. Bu seri kullanımı Visual Studio Express 2012, ancak ekran görüntüleri çoğunu, bu öğreticide Visual Studio 2010 SP1, Visual Studio 2012, Visual Studio Express 2012 veya Visual Web Developer 2010 Express ile tamamlayabilirsiniz. Seçin **yeni proje** gelen **Başlat** sayfası.

Visual Studio, bir IDE veya tümleşik geliştirme ortamını değil. Belgeleri yazmak için Microsoft Word kullanma gibi uygulamaları oluşturmak için bir IDE kullanacaksınız. Visual Studio'da bir araç çubuğu size çeşitli seçenekleri gösteren üstünde yoktur. IDE içinde görevleri gerçekleştirmek için başka bir yol sağlayan bir menüsünü yoktur. (Örneğin, seçmek yerine **yeni proje** gelen **Başlat** sayfasında, menüsünü kullanın ve seçin **dosya** &gt; **yeni proje**.)

![](intro-to-aspnet-mvc-4/_static/image3.png)

## <a name="creating-your-first-application"></a>İlk uygulamanızı oluşturma

Visual Basic veya Visual C# programlama dili olarak kullanan uygulamalar oluşturabilirsiniz. Visual C# sol tarafta'i seçin ve ardından **ASP.NET MVC 4 Web uygulaması**. Projenizin adı &quot;MvcMovie&quot; ve ardından **Tamam**.

![](intro-to-aspnet-mvc-4/_static/image4.png)

İçinde **yeni ASP.NET MVC 4 proje** iletişim kutusunda **Internet uygulama**. Bırakın **Razor** varsayılan görünüm altyapısı olarak bulunabilir.

![](intro-to-aspnet-mvc-4/_static/image5.png)

**Tamam**'ı tıklatın. Visual Studio varsayılan bir şablon, yeni oluşturduğunuz, ASP.NET MVC proje için kullanılan çalışan bir uygulama şu anda herhangi bir şey yapmadan elinizde! Basit bir budur &quot;Hello World!&quot; proje ve kullanıcının uygulamanızı başlatmak için uygun bir yerdir.

![](intro-to-aspnet-mvc-4/_static/image6.png)

Gelen **hata ayıklama** menüsünde, select **hata ayıklamayı Başlat**.

![](intro-to-aspnet-mvc-4/_static/image7.png)

Klavye kısayolu hata ayıklamayı başlatmak için F5 olduğuna dikkat edin.

F5 IIS Express başlatmak ve web uygulamanızı çalıştırmak Visual Studio neden olur. Visual Studio bir tarayıcı başlatılır ve uygulamanın giriş sayfasını açar. Tarayıcının adres çubuğunda diyor bildirimi `localhost` bir şey yok gibi ve `example.com`. Çünkü `localhost` her zaman, bu durumda yalnızca yerleşik uygulama çalışan kendi yerel bilgisayara gösterir. Visual Studio web projesini çalıştığında, web sunucusu için rastgele bir bağlantı noktası kullanılır. Aşağıdaki resimde 41788 bağlantı noktası numarasıdır. Uygulamayı çalıştırdığınızda, farklı bir bağlantı noktası büyük olasılıkla görürsünüz.

![](intro-to-aspnet-mvc-4/_static/image8.png)

Bu varsayılan şablonu kullanıma hazır giriş, kişi ve ilgili sayfaları sunar. Ayrıca kayıt ve oturum açmak için destek sağlar ve Facebook ve Twitter bağlar. Bu uygulama şeklini değiştirmek ve biraz ASP.NET MVC hakkında bilgi almak için sonraki adımdır bakın. Tarayıcınızı kapatın ve biraz kod değiştirelim.

> [!div class="step-by-step"]
> [Next](adding-a-controller.md)
