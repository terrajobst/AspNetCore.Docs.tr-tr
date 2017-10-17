---
title: "ASP.NET Core Razor sayfalarında ile çalışmaya başlama"
author: rick-anderson
description: "ASP.NET Core Razor sayfalarında ile çalışmaya başlama"
keywords: "ASP.NET Core, Razor sayfalarının Razor, MVC"
ms.author: riande
manager: wpickett
ms.date: 08/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 9d647657f21dd7e952808e5fe020f7a9e8767cd8
ms.sourcegitcommit: 3ba32b2b6425ed94604cb0f681db0d5bb5f8ad58
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/28/2017
---
# <a name="getting-started-with-razor-pages-in-aspnet-core"></a>ASP.NET Core Razor sayfalarında ile çalışmaya başlama

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Bu öğretici, bir ASP.NET Core Razor sayfalarının web uygulaması oluşturmanın temel öğretir. Tamamlamanız önerilir [Razor sayfalarının giriş](xref:mvc/razor-pages/index) bu öğreticiye başlamadan önce. Razor sayfalarının ASP.NET Core web uygulamaları için kullanıcı Arabirimi oluşturmak için önerilen yöntem olduğu.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

## <a name="create-a-razor-web-app"></a>Bir Razor web uygulaması oluşturma

* Visual Studio'dan **dosya** menüsünde, select **yeni** > **proje**.
* Yeni bir ASP.NET çekirdek Web uygulaması oluşturun. Proje adı **RazorPagesMovie**. Proje adı önemlidir *RazorPagesMovie* ad alanları, kopyala/yapıştır kod zaman eşleşecek şekilde.
  ![Yeni ASP.NET çekirdek Web uygulaması](../../mvc/razor-pages/index/_static/np.png)
* Seçin **ASP.NET Core 2.0** açılır ve ardından **Web uygulaması**.
  ![Web uygulaması (Razor sayfalarının)](../../mvc/razor-pages/index/_static/np2.png)

Visual Studio şablon bir başlangıç projesi oluşturur:

![Çözüm Gezgini](razor-pages-start/_static/se.png)

Tuşuna **F5** uygulamayı hata ayıklama modunda çalıştırmak için veya **Ctrl-F5** hata ayıklayıcı eklemeden çalıştırmak için

![Giriş veya dizin sayfası](razor-pages-start/_static/home.png)

* Visual Studio başlatır [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) ve uygulamanızı çalışır. Adres çubuğunu gösterir `localhost:port#` bir şey yok gibi ve `example.com`. Çünkü `localhost` , yerel bilgisayarınızın standart barındırıcı adıdır. Localhost yalnızca yerel bilgisayara gelen web isteklerini işlevi görür. Visual Studio web projesini oluşturduğunda, rastgele bir bağlantı noktası web sunucusu için kullanılır. Önceki görüntüde 5000 bağlantı noktası numarasıdır. Uygulamayı çalıştırdığınızda, farklı bir bağlantı noktası görürsünüz.
* Uygulama başlatma **Ctrl + F5** (olmayan hata ayıklama modu), kod değişiklikleri yapabilir, dosyayı kaydedin, tarayıcıyı yenilemek ve kod değişiklikleri görmek olanak sağlar. Çoğu geliştirici, hızlı bir şekilde uygulamayı başlatın ve değişiklikleri görmek için olmayan hata ayıklama modu kullanmayı tercih eder.

[!INCLUDE[razor-pages-start](../../includes/RP/razor-pages-start.md)]

>[!div class="step-by-step"]
[Sonraki: bir modeli ekleme](xref:tutorials/razor-pages/model)
