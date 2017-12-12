---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/intro-to-aspnet-mvc-3
title: "ASP.NET MVC 3 (VB) giriş | Microsoft Docs"
author: Rick-Anderson
description: "Bu öğretici Microsoft Visual Web Developer 2010 Express Service Pack olan 1, kullanarak bir ASP.NET MVC Web uygulaması oluşturmanın temellerini öğretmek..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: a1b3d789-93b4-487f-b90d-80c9c9b4f8fa
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/intro-to-aspnet-mvc-3
msc.type: authoredcontent
ms.openlocfilehash: f0717e8d635818322be8b3242efe756a20351340
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="intro-to-aspnet-mvc-3-vb"></a>ASP.NET MVC 3 (VB) giriş
====================
Tarafından [Rick Anderson](https://github.com/Rick-Anderson)

> Bu öğretici Microsoft Visual Web Developer 2010 Express Service Pack ücretsiz Microsoft Visual Studio sürümünü olan 1, kullanarak bir ASP.NET MVC Web uygulaması oluşturmanın temellerini öğretmek. Başlamadan önce aşağıda listelenen önkoşulları kurduğunuzdan emin olun. Bunların tümünün aşağıdaki bağlantıyı tıklatarak yükleyin: [Web Platformu yükleyicisi](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternatif olarak, aşağıdaki bağlantıları kullanarak önkoşulları ayrı ayrı yükleyebilirsiniz:
> 
> - [Visual Studio Web Developer Express SP1 önkoşulları](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 araçları güncelleştirme](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(çalışma zamanı + araçları destekler)
> 
> Visual Web Developer 2010 yerine Visual Studio 2010 kullanıyorsanız, aşağıdaki bağlantıyı tıklatarak önkoşulları yükleyin: [Visual Studio 2010 önkoşulları](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> VB.NET kaynak kodu ile Visual Web Developer projesi bu konuya eşlik etmek kullanılabilir. [VB.NET Eki](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). C# tercih ederseniz, geçiş [C# sürümü](../cs/intro-to-aspnet-mvc-3.md) Bu öğreticinin.


Bu öğretici Microsoft Visual Web Developer 2010 Express Service Pack ücretsiz Microsoft Visual Studio sürümünü olan 1, kullanarak bir ASP.NET MVC Web uygulaması oluşturmanın temellerini öğretmek. Başlamadan önce aşağıda listelenen önkoşulları kurduğunuzdan emin olun. Bunların tümünün aşağıdaki bağlantıyı tıklatarak yükleyin: [Web Platformu yükleyicisi](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternatif olarak, aşağıdaki bağlantıları kullanarak önkoşulları ayrı ayrı yükleyebilirsiniz:

- [Visual Studio Web Developer Express SP1 önkoşulları](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
- [ASP.NET MVC 3 araçları güncelleştirme](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
- [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(çalışma zamanı + araçları destekler)

Visual Web Developer 2010 yerine Visual Studio 2010 kullanıyorsanız, aşağıdaki bağlantıyı tıklatarak önkoşulları yükleyin: [Visual Studio 2010 önkoşulları](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).

VB kaynak kodu ile Visual Web Developer projesi bu konuya eşlik etmek kullanılabilir. [VB sürümü burada karşıdan](https://code.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=14824). CSharp tercih ederseniz, geçiş [CSharp sürüm](../cs/intro-to-aspnet-mvc-3.md) Bu öğreticinin.

## <a name="what-youll-build"></a>Ne oluşturacağınız

Oluşturma, düzenleme ve veritabanından filmler listeleme destekleyen basit bir film listesi uygulaması uygulamanız. Aşağıda oluşturacağınız uygulamasının iki ekran görüntüleri verilmiştir. Veritabanından filmler listesini görüntüleyen bir sayfa içerir:

[![MoviesWithVariousSm](intro-to-aspnet-mvc-3/_static/image2.png)](intro-to-aspnet-mvc-3/_static/image1.png)

Uygulaması, ekleme, düzenleme ve tek tek olanları hakkında ayrıntılara bakın yanı sıra, filmler silme sağlar. Tüm veri girişi senaryolar veritabanında depolanan verileri doğru olduğundan emin olmak için doğrulama içerir.

[![CreateFormSo](intro-to-aspnet-mvc-3/_static/image4.png)](intro-to-aspnet-mvc-3/_static/image3.png)

## <a name="skills-youll-learn"></a>Bilgi edineceksiniz yetenekleri

Öğrenecekleriniz aşağıda verilmiştir:

- Yeni bir ASP.NET MVC projesi oluşturma
- Entity Framework code first kullanarak yeni bir veritabanı oluşturma
- ASP.NET MVC denetleyicileri ve görünümler oluşturma
- Nasıl alınacağını ve verileri görüntüle
- Verileri düzenleme ve veri doğrulama etkinleştirme

## <a name="getting-started"></a>Başlarken

Visual Web Developer 2010 Express (kısaca "VWD") çalıştırarak ve seçin **yeni proje** gelen **Başlat** sayfası.

Visual Web Developer bir IDE veya tümleşik geliştirme ortamını değil. Belgeleri yazmak için Microsoft Word kullanma gibi uygulamaları oluşturmak için bir IDE kullanacaksınız. Visual Web Developer ile bir araç çubuğu size çeşitli seçenekleri gösteren üstünde yoktur. IDE içinde görevleri gerçekleştirmek için başka bir yol sağlayan bir menüsünü yoktur. (Örneğin, seçmek yerine **yeni proje** gelen **Başlat** sayfasında, menüsünü kullanın ve seçin **dosya** &gt; **yeni proje**.)

[![](intro-to-aspnet-mvc-3/_static/image6.png)](intro-to-aspnet-mvc-3/_static/image5.png)

## <a name="creating-your-first-application"></a>İlk uygulamanızı oluşturma

Seçiminizi Visual Basic veya Visual C# programlama dili olarak kullanan uygulamalar oluşturabilirsiniz. Bu öğretici için Visual Basic sol tarafta seçin, sonra seçin **ASP.NET MVC 3 Web uygulaması**. Projeniz "MvcMovie" olarak adlandırın ve ardından **Tamam**.

![1NewMVCproj_sm](intro-to-aspnet-mvc-3/_static/image7.png)

İçinde **yeni ASP.NET MVC 3 projesinin** iletişim kutusunda **Internet uygulama**. Bırakın **Razor** varsayılan görünüm altyapısı olarak bulunabilir.

![1InternetAppRazor_SM](intro-to-aspnet-mvc-3/_static/image8.png)

**Tamam**'ı tıklatın. Visual Web Developer varsayılan bir şablon, yeni oluşturduğunuz, ASP.NET MVC proje için kullanılan çalışan bir uygulama şu anda herhangi bir şey yapmadan elinizde! Bu bir basit "Hello World!" olur Proje ve kullanıcının uygulamanızı başlatmak için uygun bir yerdir.

[![](intro-to-aspnet-mvc-3/_static/image10.png)](intro-to-aspnet-mvc-3/_static/image9.png)

Gelen **hata ayıklama** menüsünde, select **hata ayıklamayı Başlat**.

![](intro-to-aspnet-mvc-3/_static/image11.png)

Klavye kısayolu hata ayıklamayı başlatmak için F5 olduğuna dikkat edin.

F5 geliştirme web sunucusu başlatmak ve web uygulamanızı çalıştırmak Visual Web Developer neden olur. VWD bir tarayıcı başlatılır ve uygulamanın giriş sayfasını açar. Tarayıcının adres çubuğunda diyor bildirimi `localhost` bir şey yok gibi ve `example.com`. Çünkü `localhost` her zaman, bu durumda yalnızca yerleşik uygulama çalışan kendi yerel bilgisayara gösterir. Bir web projesi VWD çalıştırdığında, rastgele bir bağlantı noktası proje için kullanılır. Aşağıdaki resimde 43246 rastgele bağlantı noktası numarasıdır. Projenizi muhtemelen farklı bir bağlantı noktası kullanır.

![](intro-to-aspnet-mvc-3/_static/image12.png)

Kutudan çıktığında bu varsayılan şablonu ziyaret etmek için iki sayfaları ve temel oturum açma sayfasına sunar. Şimdi bu uygulamayı nasıl çalıştığını değiştirin ve biraz işleminde ASP.NET MVC hakkında bilgi edinin. Tarayıcınızı kapatın ve biraz kod değiştirelim.

>[!div class="step-by-step"]
[Sonraki](adding-a-controller.md)
