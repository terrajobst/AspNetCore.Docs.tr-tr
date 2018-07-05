---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/intro-to-aspnet-mvc-3
title: ASP.NET MVC 3 (VB) giriş | Microsoft Docs
author: Rick-Anderson
description: Bu öğreticide, Microsoft Visual Web Developer 2010 Express Service Pack, 1, kullanarak bir ASP.NET MVC Web uygulaması oluşturmaya yönelik temel bilgiler sağlanır...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: a1b3d789-93b4-487f-b90d-80c9c9b4f8fa
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/intro-to-aspnet-mvc-3
msc.type: authoredcontent
ms.openlocfilehash: 1a870f10d39980335c7acb4a543907fecd408eb2
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37372359"
---
<a name="intro-to-aspnet-mvc-3-vb"></a>ASP.NET MVC 3 (VB) giriş
====================
Tarafından [Rick Anderson](https://github.com/Rick-Anderson)

> Bu öğreticide, Microsoft Visual Web Developer 2010 Express Service Pack ücretsiz bir Microsoft Visual Studio sürümü olan 1, kullanarak bir ASP.NET MVC Web uygulaması oluşturmaya yönelik temel bilgiler sağlanır. Başlamadan önce aşağıda listelenen ön yüklediğiniz emin olun. Aşağıdaki bağlantıya tıklayarak bunların tümünü yükleyebilirsiniz: [Web Platformu yükleyicisi](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternatif olarak, aşağıdaki bağlantıları kullanarak önkoşulları ayrı ayrı yükleyebilirsiniz:
> 
> - [Visual Studio Web Developer Express SP1 önkoşulları](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 araçları güncelleştirme](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(çalışma zamanı + araçları desteği)
> 
> Visual Web Developer 2010 yerine Visual Studio 2010 kullanıyorsanız, aşağıdaki bağlantıyı tıklatarak önkoşulları yükleyin: [Visual Studio 2010 önkoşulları](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Bu konuya eşlik etmek üzere bir Visual Web Developer proje VB.NET kaynak koduyla birlikte kullanılabilir. [VB.NET Eki](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). C# tercih ederseniz, geçiş [C# sürümü](../cs/intro-to-aspnet-mvc-3.md) Bu öğreticinin.


Bu öğreticide, Microsoft Visual Web Developer 2010 Express Service Pack ücretsiz bir Microsoft Visual Studio sürümü olan 1, kullanarak bir ASP.NET MVC Web uygulaması oluşturmaya yönelik temel bilgiler sağlanır. Başlamadan önce aşağıda listelenen ön yüklediğiniz emin olun. Aşağıdaki bağlantıya tıklayarak bunların tümünü yükleyebilirsiniz: [Web Platformu yükleyicisi](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternatif olarak, aşağıdaki bağlantıları kullanarak önkoşulları ayrı ayrı yükleyebilirsiniz:

- [Visual Studio Web Developer Express SP1 önkoşulları](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
- [ASP.NET MVC 3 araçları güncelleştirme](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
- [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(çalışma zamanı + araçları desteği)

Visual Web Developer 2010 yerine Visual Studio 2010 kullanıyorsanız, aşağıdaki bağlantıyı tıklatarak önkoşulları yükleyin: [Visual Studio 2010 önkoşulları](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).

Bu konuya eşlik etmek üzere bir Visual Web Developer proje VB kaynak koduyla birlikte kullanılabilir. [VB sürümü burada indirme](https://code.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=14824). CSharp tercih ederseniz, geçiş [CSharp sürüm](../cs/intro-to-aspnet-mvc-3.md) Bu öğreticinin.

## <a name="what-youll-build"></a>Ne oluşturacaksınız

Oluşturma, düzenleme ve veritabanı filmler listeleme destekleyen basit bir film listeleme uygulama uygulayacaksınız. Aşağıda oluşturacağınız uygulama iki ekran görüntüleri verilmiştir. Film veritabanı listesini görüntüleyen bir sayfa içerir:

[![MoviesWithVariousSm](intro-to-aspnet-mvc-3/_static/image2.png)](intro-to-aspnet-mvc-3/_static/image1.png)

Uygulama, ekleme, düzenleme ve tek tek olanları hakkında ayrıntılara bakın yanı sıra, filmler Sil da sağlar. Tüm veri girişi senaryolar veritabanında depolanan verileri doğru olduğundan emin olmak için doğrulama içerir.

[![CreateFormSo](intro-to-aspnet-mvc-3/_static/image4.png)](intro-to-aspnet-mvc-3/_static/image3.png)

## <a name="skills-youll-learn"></a>Beceriler hakkında bilgi edineceksiniz

Öğrenecekleriniz aşağıda verilmiştir:

- Yeni bir ASP.NET MVC projesi oluşturma
- Entity Framework code-first kullanarak yeni bir veritabanı oluşturma
- ASP.NET MVC denetleyicileri ve görünümleri oluşturma
- Nasıl alınacağını ve verileri görüntüle
- Verileri düzenleme ve veri doğrulamasını etkinleştirme

## <a name="getting-started"></a>Başlarken

Visual Web Developer 2010 Express (kısaca "VWD") çalıştırarak ve seçin **yeni proje** gelen **Başlat** sayfası.

Visual Web Developer, bir IDE ya da tümleşik geliştirme ortamı yöneliktir. Belgeler yazmak için Microsoft Word kullanma gibi bir IDE uygulamalar oluşturmak için kullanırsınız. Visual Web Developer kullanarak bir araç çubuğu için çeşitli seçenekler kullanılabilir gösteren üstünde yoktur. IDE'de görevleri gerçekleştirmek için başka bir yol sağlayan bir menüsünde de mevcuttur. (Örneğin seçmek yerine **yeni proje** gelen **Başlat** sayfasında menüsünü kullanın ve seçin **dosya** &gt; **YeniProje**.)

[![](intro-to-aspnet-mvc-3/_static/image6.png)](intro-to-aspnet-mvc-3/_static/image5.png)

## <a name="creating-your-first-application"></a>İlk uygulamanızı oluşturma

Programlama dili olarak Visual Basic veya Visual C# kullanarak uygulamalar oluşturabilirsiniz. Bu öğretici için Visual Basic soldaki seçip **ASP.NET MVC 3, Web uygulaması**. "MvcMovie" projenizi adlandırın ve ardından **Tamam**.

![1NewMVCproj_sm](intro-to-aspnet-mvc-3/_static/image7.png)

İçinde **yeni ASP.NET MVC 3 projesini** iletişim kutusunda **Internet uygulaması**. Bırakın **Razor** varsayılan görünüm altyapısı olarak.

![1InternetAppRazor_SM](intro-to-aspnet-mvc-3/_static/image8.png)

**Tamam**'ı tıklatın. Çalışan bir uygulama şu anda hiçbir şey yapmadan sahip olduğunuz visual Web Developer az önce oluşturduğunuz bir ASP.NET MVC projesi için varsayılan bir şablon kullanılan! Bu, bir basit "Hello World!" Proje ve kullanıcının uygulamanızı başlatmak için iyi bir yerdir.

[![](intro-to-aspnet-mvc-3/_static/image10.png)](intro-to-aspnet-mvc-3/_static/image9.png)

Gelen **hata ayıklama** menüsünde **hata ayıklamayı Başlat**.

![](intro-to-aspnet-mvc-3/_static/image11.png)

Klavye kısayolu hata ayıklamayı Başlat F5 olduğuna dikkat edin.

F5'e bir geliştirme web sunucusunu başlatmak ve web uygulamanızı çalıştırmak Visual Web Developer neden olur. VWD bir tarayıcı başlatır ve uygulamanın giriş sayfası açılır. Tarayıcının adres çubuğunda yazılı bildirimi `localhost` gibi bir şey `example.com`. Çünkü `localhost` her zaman bu durumda yeni oluşturduğunuz uygulamayı çalıştıran kendi yerel bilgisayara işaret eder. Bir web projesi VWD çalıştığında, proje için rastgele bir bağlantı noktası kullanılır. Aşağıdaki görüntüde, 43246 rastgele bağlantı noktası numarasıdır. Projeniz büyük olasılıkla farklı bir bağlantı noktası kullanır.

![](intro-to-aspnet-mvc-3/_static/image12.png)

Kullanıma hazır, bu varsayılan şablonu ziyaret etmek için iki sayfa ve bir temel oturum açma sayfası sunar. Şimdi bu uygulamayı nasıl çalıştığını değiştirmek ve biraz işlemde ASP.NET MVC hakkında bilgi edinin. Tarayıcınızı kapatın ve bazı kod değiştirelim.

> [!div class="step-by-step"]
> [Next](adding-a-controller.md)
