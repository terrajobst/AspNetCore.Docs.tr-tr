---
title: "Mac için ASP.NET Core MVC ve Visual Studio ile çalışmaya başlama"
author: rick-anderson
description: "ASP.NET Core MVC ve Visual Studio ile çalışmaya başlama"
keywords: "ASP.NET Core, MVC, Mac, Entity Framework için Visual Studio"
ms.author: riande
manager: wpickett
ms.date: 8/23/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-mac/start-mvc
ms.openlocfilehash: 028d6d3246908a9cd44a6834449d2fdbc9cae0b8
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2017
---
# <a name="getting-started-with-aspnet-core-mvc-and-visual-studio-for-mac"></a>Mac için ASP.NET Core MVC ve Visual Studio ile çalışmaya başlama

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Bu öğretici bir ASP.NET Core MVC web app kullanarak oluşturma temelleri öğretilmektedir [Mac için Visual Studio](https://www.visualstudio.com/vs/visual-studio-mac/). [!INCLUDE[consider RP](../../includes/razor.md)]

Bu öğretici 3 sürümü vardır:

* macOS: [Mac için Visual Studio ile bir ASP.NET Core MVC uygulaması oluşturma](xref:tutorials/first-mvc-app-mac/start-mvc)
* Windows: [Visual Studio ile ASP.NET Core MVC uygulaması oluşturma](xref:tutorials/first-mvc-app/start-mvc)
* MacOS, Linux ve Windows: [Visual Studio Code ile ASP.NET Core MVC uygulaması oluşturma](xref:tutorials/first-mvc-app-xplat/start-mvc)

## <a name="prerequisites"></a>Önkoşullar

Bu öğretici gerektirir [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) veya sonraki bir sürümü. Bkz: [pdf](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-mvc-app-mac/start-mvc/8-23-17.pdf) ASP.NET Core 1.1 sürümünün.

Aşağıdaki yükleyin:

- [.NET core 2.0.0 SDK](https://www.microsoft.com/net/core) veya daha yenisi
- [Mac için Visual Studio](https://www.visualstudio.com/vs/visual-studio-mac/)

## <a name="create-a-web-app"></a>Bir web uygulaması oluşturma

Visual Studio'dan seçin **Dosya > Yeni bir çözüm**.

![macOS yeni çözüm](../first-web-api-mac/_static/sln.png)

Seçin **.NET Core uygulaması > ASP.NET Core > Web uygulaması > sonraki**.

![macOS yeni proje iletişim kutusu](start-mvc/1.png)

Proje adı **MvcMovie**ve ardından **oluşturma**.

![macOS yeni proje iletişim kutusu](start-mvc/2.png)

### <a name="launch-the-app"></a>Uygulamayı başlatın

Visual Studio'da seçin **Çalıştır > hata ayıklama olmadan Başlat** uygulamayı başlatmak için. Visual Studio başlatır [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview), bir tarayıcı başlatılır ve gider `http://localhost:port`, burada *bağlantı noktası* rastgele seçilen bağlantı noktası numarasıdır.

![Yeni Proje tarayıcıyla](start-mvc/b1.png)

* Adres çubuğunu gösterir `localhost:port#` bir şey yok gibi ve `example.com`. Çünkü `localhost` , yerel bilgisayarınızın standart barındırıcı adıdır. Visual Studio web projesini oluşturduğunda, rastgele bir bağlantı noktası web sunucusu için kullanılır. Uygulamayı çalıştırdığınızda, farklı bir bağlantı noktası görürsünüz.
* Hata ayıklama veya hata ayıklama olmayan modundan uygulamada başlatabilirsiniz **çalıştırmak** menüsü.

Varsayılan şablonu verir **hakkında giriş** ve **kişi** bağlantılar. Yukarıdaki tarayıcı resimde bu bağlantıları göstermez. Tarayıcınız boyutuna bağlı olarak, bunları göstermek için Gezinti simgesini gerekebilir.

![Yeni Proje tarayıcıyla](start-mvc/b2.png)

Bu öğreticinin sonraki bölümünde, MVC konusunda bilgi edinmek ve bazı kod yazmayı başlatır.

>[!div class="step-by-step"]
[Sonraki](adding-controller.md)  
