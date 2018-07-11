---
title: ASP.NET Core MVC ve Visual Studio ile çalışmaya başlama
author: rick-anderson
description: ASP.NET Core MVC ve Visual Studio ile çalışmaya başlama hakkında bilgi edinin.
ms.author: riande
ms.date: 10/07/2017
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: 1fb3947023843341403f4355c6ae1e61d7e4f6b1
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38217984"
---
# <a name="get-started-with-aspnet-core-mvc-and-visual-studio"></a>ASP.NET Core MVC ve Visual Studio ile çalışmaya başlama

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [consider RP](~/includes/razor.md)]

Bu öğreticinin 3 sürümü vardır:

* macOS: [Mac için Visual Studio ile ASP.NET Core MVC uygulaması oluşturma](xref:tutorials/first-mvc-app-mac/start-mvc)
* Windows: [Visual Studio ile bir ASP.NET Core MVC uygulaması oluşturma](xref:tutorials/first-mvc-app/start-mvc)
* macOS, Linux ve Windows: [Visual Studio Code ile ASP.NET Core MVC uygulaması oluşturma](xref:tutorials/first-mvc-app-xplat/start-mvc)

## <a name="install-visual-studio-and-net-core"></a>Visual Studio ve .NET Core yükleyin

::: moniker range=">= aspnetcore-2.1"

[! [] (~/İncludes/net-core-prereqs-windows.md) içerir [](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-web-app"></a>Bir web uygulaması oluşturma

Visual Studio'dan seçin **Dosya > Yeni > Proje**.

![Dosya > Yeni > Proje](start-mvc/_static/alt_new_project.png)

Tamamlamak **yeni proje** iletişim:

* Sol bölmede, dokunun **.NET Core**
* Orta bölmede dokunun **ASP.NET Core Web uygulaması (.NET Core)**
* (Kod kopyaladığınızda, ad alanı eşleşecek şekilde "MvcMovie" proje adı önemlidir.) "MvcMovie" proje adı
* Dokunun **Tamam**

![Yeni Proje iletişim kutusunda, sol bölmede, ASP.NET Core web'de .net core ](start-mvc/_static/new_project2-21.png)

Tamamlamak **yeni ASP.NET Core Web uygulaması (.NET Core) - MvcMovie** iletişim:

* Sürüm Seçici açılan kutusunda seçin **ASP.NET Core 2.1**
* Seçin **Application(Model-View-Controller) Web**
* Dokunun **Tamam**.

![Yeni Proje iletişim kutusunda, sol bölmede, ASP.NET Core web'de .net core ](start-mvc/_static/new_project22-21.png)

Visual Studio, yeni oluşturduğunuz MVC projesi için varsayılan bir şablon kullanılır. Çalışan bir uygulamayı şu anda bir proje adı girerek ve bazı Seçenekler'i seçerek sizde. Bu temel başlangıç projesini ve başlatmak için iyi bir yerdir,

Dokunun **F5** uygulamayı hata ayıklama modunda çalıştırmak için veya **Ctrl-F5** olmayan hata ayıklama modunda.
<!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![Uygulamayı çalıştırma](start-mvc/_static/1.png)

* Visual Studio başlatır [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) ve uygulamanızı çalışır. Adres çubuğuna gösteren uyarı `localhost:port#` gibi bir şey `example.com`. Çünkü `localhost` standart, yerel bilgisayar adıdır. Visual Studio, bir web projesi oluşturduğunda, web sunucusu için rastgele bir bağlantı noktası kullanılır. Yukarıdaki görüntüde, bağlantı noktası numarasını 5000'dir. Tarayıcı gösterir URL'de `localhost:5000`. Uygulamayı çalıştırdığınızda, farklı bir bağlantı noktası görürsünüz.
* Uygulamayı başlatma **Ctrl + F5** (hata ayıklama olmayan mod), kod değişiklikleri yapabilir, dosyayı kaydetmek, tarayıcıyı yenileyin ve kod değişikliklerini görebilirsiniz olanak sağlar. Geliştiricilerin çoğu, hızlı bir şekilde uygulamayı başlatın ve değişiklikleri görmek için hata ayıklama olmayan modu kullanmayı tercih eder.
* Uygulamada hata ayıklama veya hata ayıklama olmayan moddan başlatabilirsiniz **hata ayıklama** menü öğesi:

![Menü hata ayıklama](start-mvc/_static/debug_menu.png)

* Dokunarak uygulamayı hata ayıklaması yapabilirsiniz **IIS Express** düğmesi

![IIS Express](start-mvc/_static/iis_express.png)

Varsayılan şablonu size çalışma **hakkında giriş** ve **kişi** bağlantıları. Yukarıdaki tarayıcı resimde, bu bağlantıları göstermez. Tarayıcınız boyutuna bağlı olarak, gösterileceğinin Gezinti simgesine tıklamanız gerekebilir.

![sağ üstteki gezinti simgesi](start-mvc/_static/2.png)

Hata ayıklama modunda çalışıyormuş dokunun **Shift + F5** hata ayıklamayı durdurmak için.

Bu öğreticinin sonraki bölümünde, biz MVC konusunda bilgi ve biraz kod yazmaya.

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[! [] (~/İncludes/net-core-prereqs.md) içerir [](~/includes/net-core-prereqs.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Visual Studio Community 2017'yi yükleyin. Topluluk indir'i seçin. Visual Studio 2017 varsa bu adımı atlayın.

* [Visual Studio 2017 giriş sayfası yükleyicisi](https://www.visualstudio.com/)

Yükleyiciyi çalıştırın ve aşağıdaki iş yükleri seçin:

* **ASP.NET ve web geliştirme** (altında **Web ve bulut**)
* **.NET core çoklu platform geliştirme** (altında **diğer araç takımları**)

![**ASP.NET ve web geliştirme ** (altında ** Web ve bulut **)](start-mvc/_static/web_workload.png)

![* *.NET çekirdek arası arası platfrom geliştirme ** (altında ** diğer araç takımları **)](start-mvc/_static/x_plat_wl.png)

---

## <a name="create-a-web-app"></a>Bir web uygulaması oluşturma

Visual Studio'dan seçin **Dosya > Yeni > Proje**.

![Dosya > Yeni > Proje](start-mvc/_static/alt_new_project.png)

Tamamlamak **yeni proje** iletişim:

* Sol bölmede, dokunun **.NET Core**
* Orta bölmede dokunun **ASP.NET Core Web uygulaması (.NET Core)**
* (Kod kopyaladığınızda, ad alanı eşleşecek şekilde "MvcMovie" proje adı önemlidir.) "MvcMovie" proje adı
* Dokunun **Tamam**

![Yeni Proje iletişim kutusunda, sol bölmede, ASP.NET Core web'de .net core ](start-mvc/_static/new_project2.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Tamamlamak **yeni ASP.NET Core Web uygulaması (.NET Core) - MvcMovie** iletişim:

* Sürüm Seçici açılan kutusunda seçin **ASP.NET Core 2.-**
* Seçin **Application(Model-View-Controller) Web**
* Dokunun **Tamam**.

![Yeni Proje iletişim kutusunda, sol bölmede, ASP.NET Core web'de .net core ](start-mvc/_static/new_project22.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Tamamlamak **yeni ASP.NET Core Web uygulaması (.NET Core) - MvcMovie** iletişim:

* Sürüm Seçici açılan kutusunda tap içindeki **ASP.NET Core 1.1**
* Dokunun **Web uygulaması**
* Varsayılan tutun **kimlik doğrulaması yok**
* Dokunun **Tamam**.

![Yeni ASP.NET Core web uygulaması](start-mvc/_static/p3.png)

---

Visual Studio, yeni oluşturduğunuz MVC projesi için varsayılan bir şablon kullanılır. Çalışan bir uygulamayı şu anda bir proje adı girerek ve bazı Seçenekler'i seçerek sizde. Bu temel başlangıç projesini ve başlatmak için iyi bir yerdir,

Dokunun **F5** uygulamayı hata ayıklama modunda çalıştırmak için veya **Ctrl-F5** olmayan hata ayıklama modunda.
<!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![Uygulamayı çalıştırma](start-mvc/_static/1.png)

* Visual Studio başlatır [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) ve uygulamanızı çalışır. Adres çubuğuna gösteren uyarı `localhost:port#` gibi bir şey `example.com`. Çünkü `localhost` standart, yerel bilgisayar adıdır. Visual Studio, bir web projesi oluşturduğunda, web sunucusu için rastgele bir bağlantı noktası kullanılır. Yukarıdaki görüntüde, bağlantı noktası numarasını 5000'dir. Tarayıcı gösterir URL'de `localhost:5000`. Uygulamayı çalıştırdığınızda, farklı bir bağlantı noktası görürsünüz.
* Uygulamayı başlatma **Ctrl + F5** (hata ayıklama olmayan mod), kod değişiklikleri yapabilir, dosyayı kaydetmek, tarayıcıyı yenileyin ve kod değişikliklerini görebilirsiniz olanak sağlar. Geliştiricilerin çoğu, hızlı bir şekilde uygulamayı başlatın ve değişiklikleri görmek için hata ayıklama olmayan modu kullanmayı tercih eder.
* Uygulamada hata ayıklama veya hata ayıklama olmayan moddan başlatabilirsiniz **hata ayıklama** menü öğesi:

![Menü hata ayıklama](start-mvc/_static/debug_menu.png)

* Dokunarak uygulamayı hata ayıklaması yapabilirsiniz **IIS Express** düğmesi

![IIS Express](start-mvc/_static/iis_express.png)

Varsayılan şablonu size çalışma **hakkında giriş** ve **kişi** bağlantıları. Yukarıdaki tarayıcı resimde, bu bağlantıları göstermez. Tarayıcınız boyutuna bağlı olarak, gösterileceğinin Gezinti simgesine tıklamanız gerekebilir.

![sağ üstteki gezinti simgesi](start-mvc/_static/2.png)

Hata ayıklama modunda çalışıyormuş dokunun **Shift + F5** hata ayıklamayı durdurmak için.

Bu öğreticinin sonraki bölümünde, biz MVC konusunda bilgi ve biraz kod yazmaya.

::: moniker-end
> [!div class="step-by-step"]
> [Next](adding-controller.md)  
