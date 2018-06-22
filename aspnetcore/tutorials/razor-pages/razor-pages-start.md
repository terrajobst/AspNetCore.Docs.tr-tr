---
title: ASP.NET Core Razor sayfalarında kullanmaya başlama
author: rick-anderson
description: Bir ASP.NET Core Razor sayfalarının web uygulaması oluşturmanın temel bilgileri bulur. Razor sayfalarının ASP.NET Core web iş yükleri için önerilir.
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: e317b49f2ad33c392de33bc32a87f67bb8cb72a0
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278050"
---
# <a name="get-started-with-razor-pages-in-aspnet-core"></a>ASP.NET Core Razor sayfalarında kullanmaya başlama

tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range="= aspnetcore-2.0"

Bu öğretici ASP.NET Core 2.1 sürümüne izleyin öneririz. Bunun **çok** izleyin daha kolay ve daha fazla özellikleri kapsar. Seçin **ASP.NET Core 2.1** sürüm Seçici içinde.

![Sürüm Seçici TOC içinde](razor-pages-start/_static/v21.png)

::: moniker-end

Bu öğretici, bir ASP.NET Core Razor sayfalarının web uygulaması oluşturmanın temel öğretir. Razor sayfalarının ASP.NET Core web uygulamaları için kullanıcı Arabirimi oluşturmak için önerilen yöntem olduğu.

Bu öğretici için üç sürümü vardır:

* Windows: Bu öğretici
* MacOS: [Razor sayfalarının Visual Studio ile Mac için kullanmaya başlama](xref:tutorials/razor-pages-mac/razor-pages-start)
* macOS, Linux ve Windows: [ASP.NET Core Razor sayfalarında, Visual Studio Code ile çalışmaya başlama](xref:tutorials/razor-pages-vsc/razor-pages-start)

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

::: moniker range=">= aspnetcore-2.1"

## <a name="prerequisites"></a>Önkoşullar

[! [] (~/İncludes/net-core-prereqs-windows.md) içerir [](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-razor-web-app"></a>Bir Razor web uygulaması oluşturma

* Visual Studio'dan **dosya** menüsünde, select **yeni** > **proje**.
* Yeni bir ASP.NET çekirdek Web uygulaması oluşturun. Proje adı **RazorPagesMovie**. Proje adı önemlidir *RazorPagesMovie* ad alanları, kopyala/yapıştır kod zaman eşleşecek şekilde.
 ![Yeni ASP.NET çekirdek Web uygulaması](razor-pages-start/_static/np_2.1.png)
* Seçin **ASP.NET Core 2.1** açılır ve ardından **Web uygulaması**.

 ![Yeni ASP.NET çekirdek Web uygulaması](razor-pages-start/_static/np_2_2.1.png)

Visual Studio şablon bir başlangıç projesi oluşturur:

![Çözüm Gezgini](razor-pages-start/_static/se2.1.png)

Tuşuna **F5** uygulamayı hata ayıklama modunda çalıştırmak için veya **Ctrl-F5** hata ayıklayıcı eklemeden çalıştırmak için. Seçin **kabul** İzleme'onayı için. Bu uygulamayı, kişisel bilgi izlemez. Oluşturulan şablon kodunun karşılamak amacıyla varlıklar içeren [genel veri koruma düzenleme (GDPR)](xref:security/gdpr).

![Giriş veya dizin sayfası](razor-pages-start/_static/homeGDPR.png)

Aşağıdaki resimde, izleme kabul ettikten sonra uygulama gösterilmektedir:

![Giriş veya dizin sayfası](razor-pages-start/_static/home2.1.png)

* Visual Studio başlatır [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) ve uygulamayı çalıştırır. Adres çubuğunu gösterir `localhost:port#` bir şey yok gibi ve `example.com`. Çünkü `localhost` , yerel bilgisayarınızın standart barındırıcı adıdır. Localhost yalnızca yerel bilgisayara gelen web isteklerini işlevi görür. Visual Studio web projesini oluşturduğunda, rastgele bir bağlantı noktası web sunucusu için kullanılır. Önceki görüntüde 5000 bağlantı noktası numarasıdır. Uygulamayı çalıştırdığınızda, farklı bir bağlantı noktası görürsünüz.
* Uygulama başlatma **Ctrl + F5** (olmayan hata ayıklama modu), kod değişiklikleri yapabilir, dosyayı kaydedin, tarayıcıyı yenilemek ve kod değişiklikleri görmek olanak sağlar. Çoğu geliştirici, hızlı bir şekilde uygulamayı başlatın ve değişiklikleri görmek için olmayan hata ayıklama modu kullanmayı tercih eder.

[!INCLUDE [razor-pages-start](~/includes/RP/2.1/razor-pages-start.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

## <a name="prerequisites"></a>Önkoşullar

[! [] (~/İncludes/net-core-prereqs-windows.md) içerir [](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-razor-web-app"></a>Bir Razor web uygulaması oluşturma

* Visual Studio'dan **dosya** menüsünde, select **yeni** > **proje**.
* Yeni bir ASP.NET çekirdek Web uygulaması oluşturun. Proje adı **RazorPagesMovie**. Proje adı önemlidir *RazorPagesMovie* ad alanları, kopyala/yapıştır kod zaman eşleşecek şekilde.
  ![Yeni ASP.NET çekirdek Web uygulaması](../../razor-pages/index/_static/np.png)
* Seçin **ASP.NET Core 2.0** açılır ve ardından **Web uygulaması**.

  [!INCLUDE [install 2.0](~/includes/dotnetcore-on-dotnetfx-vs.md)]

Visual Studio şablon bir başlangıç projesi oluşturur:

![Çözüm Gezgini](razor-pages-start/_static/se.png)

Tuşuna **F5** uygulamayı hata ayıklama modunda çalıştırmak için veya **Ctrl-F5** hata ayıklayıcı eklemeden çalıştırmak için

![Giriş veya dizin sayfası](razor-pages-start/_static/home.png)

* Visual Studio başlatır [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) ve uygulamanızı çalışır. Adres çubuğunu gösterir `localhost:port#` bir şey yok gibi ve `example.com`. Çünkü `localhost` , yerel bilgisayarınızın standart barındırıcı adıdır. Localhost yalnızca yerel bilgisayara gelen web isteklerini işlevi görür. Visual Studio web projesini oluşturduğunda, rastgele bir bağlantı noktası web sunucusu için kullanılır. Önceki görüntüde 5000 bağlantı noktası numarasıdır. Uygulamayı çalıştırdığınızda, farklı bir bağlantı noktası görürsünüz.
* Uygulama başlatma **Ctrl + F5** (olmayan hata ayıklama modu), kod değişiklikleri yapabilir, dosyayı kaydedin, tarayıcıyı yenilemek ve kod değişiklikleri görmek olanak sağlar. Çoğu geliştirici, hızlı bir şekilde uygulamayı başlatın ve değişiklikleri görmek için olmayan hata ayıklama modu kullanmayı tercih eder.

[!INCLUDE [razor-pages-start](~/includes/RP/2.1/razor-pages-start.md)]

::: moniker-end

> [!div class="step-by-step"]
> [Sonraki: bir modeli ekleme](xref:tutorials/razor-pages/model)
