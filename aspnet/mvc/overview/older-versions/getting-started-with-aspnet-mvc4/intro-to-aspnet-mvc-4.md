---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4
title: ASP.NET MVC 4'e giriş | Microsoft Docs
author: Rick-Anderson
description: Bu öğreticide Visual Studio 2013 burada kullanarak kullanılabiliyorsa, güncelleştirilmiş bir sürüm. Yeni t birçok iyileştirme sağlayan ASP.NET MVC 5 öğreticide...
ms.author: riande
ms.date: 08/15/2012
ms.assetid: ed66530a-04d5-49eb-b76a-85be1f57c437
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: ea3d1517192ded0e5372c49897bb1fec33324b6f
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912403"
---
<a name="intro-to-aspnet-mvc-4"></a>ASP.NET MVC 4'e giriş
====================
Tarafından [Rick Anderson]((https://twitter.com/RickAndMSFT))

> Bu öğreticide kullanılabiliyorsa, güncelleştirilmiş bir sürümünü [burada](../../getting-started/introduction/getting-started.md) kullanarak [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013). Bu öğretici birçok iyileştirme sağlayan ASP.NET MVC 5 yeni öğretici kullanır.
>
> Bu öğreticide Microsoft kullanarak bir ASP.NET MVC 4 Web uygulaması oluşturmaya ilişkin temel bilgileri sağlanır [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) veya Visual Web Developer 2010 Express Service Pack 1. Visual Studio 2012 önerilir öğreticiyi tamamlamak için herhangi bir şey yüklemeniz gerekmez. Visual Studio 2010 kullanıyorsanız, aşağıdaki bileşenleri yüklemeniz gerekir. Aşağıdaki bağlantılara tıklayarak bunların tümünü yükleyebilirsiniz:
>
> - [Visual Studio Web Developer Express SP1 önkoşulları](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 4 için WPI yükleyici](https://go.microsoft.com/fwlink/?LinkId=243392)
> - [LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0)
> - [SSDT](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx)
>
> Visual Web Developer 2010 yerine Visual Studio 2010 kullanıyorsanız, yükleme [ASP.NET MVC 4 için WPI yükleyici](https://go.microsoft.com/fwlink/?LinkId=243392) ve: [Visual Studio 2010 önkoşulları](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)
>
> C# kaynak kodu içeren bir Visual Web Developer proje, bu konuya eşlik etmek üzere kullanılabilir. [C# sürümü indirme](https://code.msdn.microsoft.com/Intro-to-ASPNET-MVC-4-61d0219d/file/114480/1/MvcMovie.zip).
>
> Öğreticide uygulamayı Visual Studio'da çalıştırın. Ayrıca uygulama kullanılabilir Internet üzerinden bir barındırma sağlayıcısına dağıtarak yapabilirsiniz. Microsoft'un sunduğu en fazla 10 web siteleri için ücretsiz bir web barındırma bir [ücretsiz deneme hesabınızı Windows Azure](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604). Visual Studio web projesini bir Windows Azure Web sitesine dağıtma hakkında daha fazla bilgi için bkz: [oluşturun ve bir ASP.NET web sitesi ve Visual Studio ile SQL veritabanı dağıtma](https://docs.microsoft.com/dotnet/azure/). Bu öğretici ayrıca Windows Azure SQL veritabanı'na (eski adı SQL Azure) SQL Server veritabanınızı dağıtmak için Entity Framework Code First Migrations'ı kullanma işlemini gösterir.
>
> Bu öğreticide, Rick Anderson tarafından yazılmış ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ).


## <a name="what-youll-build"></a>Ne oluşturacaksınız

> [!NOTE]
> Bu öğreticide kullanılabiliyorsa, güncelleştirilmiş bir sürümünü [burada](../../getting-started/introduction/getting-started.md) kullanarak [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013). Bu öğretici birçok iyileştirme sağlayan ASP.NET MVC 5 yeni öğretici kullanır.


Oluşturma, düzenleme, arama ve veritabanından filmler listeleme destekleyen basit bir film listeleme uygulama uygulayacaksınız. Aşağıda oluşturacağınız uygulama iki ekran görüntüleri verilmiştir. Film veritabanı listesini görüntüleyen bir sayfa içerir:

![](intro-to-aspnet-mvc-4/_static/image1.png)

Uygulama, ekleme, düzenleme ve tek tek olanları hakkında ayrıntılara bakın yanı sıra, filmler Sil da sağlar. Tüm veri girişi senaryolar veritabanında depolanan verileri doğru olduğundan emin olmak için doğrulama içerir.

![](intro-to-aspnet-mvc-4/_static/image2.png)

## <a name="getting-started"></a>Başlarken

Visual Studio Express 2012 veya Visual Web Developer 2010 Express'i çalıştırarak başlayın. Ekran görüntüleri, bu Visual Studio Express 2012 serisi kullanın, ancak çoğu Visual Studio 2010 SP1, Visual Studio 2012, Visual Studio Express 2012 veya Visual Web Developer 2010 Express ile Bu öğreticiyi tamamlayabilirsiniz. Seçin **yeni proje** gelen **Başlat** sayfası.

Visual Studio IDE, veya tümleşik geliştirme ortamı ' dir. Belgeler yazmak için Microsoft Word kullanma gibi bir IDE uygulamalar oluşturmak için kullanırsınız. Visual Studio'da bir araç çubuğu için çeşitli seçenekler kullanılabilir gösteren üstünde yoktur. IDE'de görevleri gerçekleştirmek için başka bir yol sağlayan bir menüsünde de mevcuttur. (Örneğin seçmek yerine **yeni proje** gelen **Başlat** sayfasında menüsünü kullanın ve seçin **dosya** &gt; **YeniProje**.)

![](intro-to-aspnet-mvc-4/_static/image3.png)

## <a name="creating-your-first-application"></a>İlk uygulamanızı oluşturma

Programlama dili olarak Visual Basic veya Visual C# kullanarak uygulamalar oluşturabilirsiniz. Visual C# sol tarafta'i seçin ve ardından **ASP.NET MVC 4 Web uygulaması**. Projenizi adlandırın &quot;MvcMovie&quot; ve ardından **Tamam**.

![](intro-to-aspnet-mvc-4/_static/image4.png)

İçinde **yeni ASP.NET MVC 4 proje** iletişim kutusunda **Internet uygulaması**. Bırakın **Razor** varsayılan görünüm altyapısı olarak.

![](intro-to-aspnet-mvc-4/_static/image5.png)

**Tamam**'ı tıklatın. Çalışan bir uygulama şu anda hiçbir şey yapmadan sahip olduğunuz visual Studio ASP.NET MVC projesi için az önce oluşturduğunuz varsayılan bir şablon kullanılan! Bu basit bir &quot;Merhaba Dünya!&quot; proje ve kullanıcının uygulamanızı başlatmak için iyi bir yerdir.

![](intro-to-aspnet-mvc-4/_static/image6.png)

Gelen **hata ayıklama** menüsünde **hata ayıklamayı Başlat**.

![](intro-to-aspnet-mvc-4/_static/image7.png)

Klavye kısayolu hata ayıklamayı Başlat F5 olduğuna dikkat edin.

F5 IIS Express'i başlatın ve web uygulamanızı çalıştırmak Visual Studio neden olur. Visual Studio bir tarayıcı başlatır ve uygulamanın giriş sayfası açılır. Tarayıcının adres çubuğunda yazılı bildirimi `localhost` gibi bir şey `example.com`. Çünkü `localhost` her zaman bu durumda yeni oluşturduğunuz uygulamayı çalıştıran kendi yerel bilgisayara işaret eder. Visual Studio web projesini çalıştığında, web sunucusu için rastgele bir bağlantı noktası kullanılır. Aşağıdaki görüntüde, 41788 bağlantı noktası numarasıdır. Uygulamayı çalıştırdığınızda, büyük olasılıkla farklı bir bağlantı noktası görürsünüz.

![](intro-to-aspnet-mvc-4/_static/image8.png)

Kullanıma hazır bu varsayılan şablonu, ev, kişi ve ilgili sayfaları sunar. Ayrıca kayıt ve oturum açmak için destek sağlar ve Facebook ve Twitter için bağlar. Sonraki adım, bu uygulama çalışma şeklini değiştirmek ve ASP.NET MVC hakkında biraz bilgi sağlamaktır. Tarayıcınızı kapatın ve bazı kod değiştirelim.

> [!div class="step-by-step"]
> [Next](adding-a-controller.md)
