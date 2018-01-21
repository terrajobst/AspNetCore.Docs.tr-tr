---
title: "ASP.NET Core MVC ve Visual Studio ile çalışmaya başlama"
author: rick-anderson
description: "ASP.NET Core MVC ve Visual Studio ile çalışmaya başlama"
ms.author: riande
manager: wpickett
ms.date: 10/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: a55b0b2a52856755b89e8ae6a2598c713f1d436d
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="getting-started-with-aspnet-core-mvc-and-visual-studio"></a>ASP.NET Core MVC ve Visual Studio ile çalışmaya başlama

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE[consider RP](../../includes/razor.md)]

Bu öğretici 3 sürümü vardır:

* macOS: [Mac için Visual Studio ile ASP.NET Core MVC uygulama oluşturma](xref:tutorials/first-mvc-app-mac/start-mvc)
* Windows: [Visual Studio ile ASP.NET Core MVC uygulaması oluşturma](xref:tutorials/first-mvc-app/start-mvc)
* macOS, Linux ve Windows: [Visual Studio Code ile bir ASP.NET Core MVC uygulaması oluşturma](xref:tutorials/first-mvc-app-xplat/start-mvc)

## <a name="install-visual-studio-and-net-core"></a>Visual Studio ve .NET Core yükleyin

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Visual Studio Community 2017 yükleyin. Topluluk indirme seçin. Visual Studio yüklü 2017 varsa bu adımı atlayın.

* [Visual Studio 2017 giriş sayfası yükleyicisi](https://www.visualstudio.com/)

Yükleyiciyi çalıştırın ve aşağıdaki iş yüklerini seçin:

* **ASP.NET ve web geliştirme** (altında **Web ve bulut**)
* **.NET core platformlar arası geliştirme** (altında **diğer Toolsets**)

![**ASP.NET ve web geliştirme ** (altında ** Web ve bulut **)](start-mvc/_static/web_workload.png)

![* *.NET çekirdek arası arası platfrom geliştirme ** (altında ** diğer Toolsets **)](start-mvc/_static/x_plat_wl.png)

---

## <a name="create-a-web-app"></a>Bir web uygulaması oluşturma

Visual Studio'dan seçin **Dosya > Yeni > Proje**.

![Dosya > Yeni > Proje](start-mvc/_static/alt_new_project.png)

Tamamlamak **yeni proje** iletişim:

* Sol bölmede, dokunun **.NET Core**
* Orta bölmede, dokunun **ASP.NET çekirdek Web uygulaması (.NET çekirdek)**
* (Kod kopyaladığınızda, ad alanı eşleşecek şekilde "MvcMovie" proje adı önemlidir.) "MvcMovie" proje adı
* Dokunun **Tamam**

![Yeni Proje iletişim kutusunda, sol bölmede, ASP.NET Core web .net core ](start-mvc/_static/new_project2.png)


# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Tamamlamak **yeni ASP.NET çekirdek Web uygulaması (.NET Core) - MvcMovie** iletişim:

* Sürüm Seçici açılan kutusunda **ASP.NET Core 2.-**
* Seçin **Application(Model-View-Controller) Web**
* Dokunun **Tamam**.

![Yeni Proje iletişim kutusunda, sol bölmede, ASP.NET Core web .net core ](start-mvc/_static/new_project22.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Tamamlamak **yeni ASP.NET çekirdek Web uygulaması (.NET Core) - MvcMovie** iletişim:

* Sürüm Seçici açılan kutu dokunun içinde **ASP.NET Core 1.1**
* Dokunun **Web uygulaması**
* Varsayılan tutmak **doğrulaması yok**
* Dokunun **Tamam**.

![Yeni ASP.NET Core web uygulaması](start-mvc/_static/p3.png)

---

Visual Studio, yeni oluşturduğunuz MVC proje için varsayılan bir şablon kullanılır. Bir çalışma şu anda bir proje adı girerek ve birkaç seçenek seçerek uygulamanız. Bu bir basit başlangıç projesi ve başlatmak için uygun bir yerdir,

Dokunun **F5** uygulamayı hata ayıklama modunda çalıştırmak için veya **Ctrl-F5** olmayan hata ayıklama modunda.
<!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![Uygulamayı çalıştırma](start-mvc/_static/1.png)

* Visual Studio başlatır [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) ve uygulamanızı çalışır. Adres çubuğunun bildirim `localhost:port#` bir şey yok gibi ve `example.com`. Çünkü `localhost` , yerel bilgisayarınızın standart barındırıcı adıdır. Visual Studio web projesini oluşturduğunda, rastgele bir bağlantı noktası web sunucusu için kullanılır. Yukarıdaki resimde 5000 bağlantı noktası numarasıdır. Tarayıcı gösterir URL'de `localhost:5000`. Uygulamayı çalıştırdığınızda, farklı bir bağlantı noktası görürsünüz.
* Uygulama başlatma **Ctrl + F5** (olmayan hata ayıklama modu), kod değişiklikleri yapabilir, dosyayı kaydedin, tarayıcıyı yenilemek ve kod değişiklikleri görmek olanak sağlar. Çoğu geliştirici, hızlı bir şekilde uygulamayı başlatın ve değişiklikleri görmek için olmayan hata ayıklama modu kullanmayı tercih eder.
* Hata ayıklama veya hata ayıklama olmayan modundan uygulamada başlatabilirsiniz **hata ayıklama** menü öğesi:

![Menü hata ayıklama](start-mvc/_static/debug_menu.png)

* Uygulama dokunarak ayıklayabilirsiniz **IIS Express** düğmesi

![IIS Express](start-mvc/_static/iis_express.png)

Varsayılan şablonu, çalışma sunar **hakkında giriş** ve **kişi** bağlantılar. Yukarıdaki tarayıcı resimde bu bağlantıları göstermez. Tarayıcınız boyutuna bağlı olarak, bunları göstermek için Gezinti simgesini gerekebilir.

![sağ üstteki gezinti simgesi](start-mvc/_static/2.png)

Hata ayıklama modunda çalışıyormuş dokunun **Shift + F5** hata ayıklamasını durdurmak için.

Bu öğreticinin sonraki bölümünde, biz MVC hakkında bilgi edinin ve biraz kod yazmaya başlamadan.

>[!div class="step-by-step"]
[Next](adding-controller.md)  
