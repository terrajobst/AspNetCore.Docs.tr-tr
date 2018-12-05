---
title: Mac için Visual Studio ile ASP.NET Core MVC ile çalışmaya başlama
author: rick-anderson
description: ASP.NET Core MVC ve Visual Studio ile çalışmaya başlama hakkında bilgi edinin
ms.author: riande
ms.custom: mvc
ms.date: 12/01/2018
uid: tutorials/first-mvc-app-mac/start-mvc
ms.openlocfilehash: 1e3cc91003ac946ac2e5efcd79e20d9215ded902
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52862232"
---
# <a name="get-started-with-aspnet-core-mvc-and-visual-studio-for-mac"></a>Mac için Visual Studio ile ASP.NET Core MVC ile çalışmaya başlama

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Bu öğreticide bir ASP.NET Core MVC kullanarak web uygulamasını oluşturmaya ilişkin temel bilgileri öğretir [Mac için Visual Studio](https://www.visualstudio.com/vs/visual-studio-mac/).

[!INCLUDE [consider RP](../../includes/razor.md)]

Bu öğreticinin 3 sürümü vardır:

* macOS: [Mac için Visual Studio ile bir ASP.NET Core MVC uygulaması oluşturun](xref:tutorials/first-mvc-app-mac/start-mvc)
* Windows: [Visual Studio ile ASP.NET Core MVC uygulaması oluşturun](xref:tutorials/first-mvc-app/start-mvc)
* Linux, macOS ve Windows: [Visual Studio Code ile ASP.NET Core MVC uygulaması oluşturun](xref:tutorials/first-mvc-app-xplat/start-mvc)

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [](~/includes/net-core-prereqs-macos.md)]

## <a name="create-a-web-app"></a>Bir web uygulaması oluşturma

Visual Studio'dan seçin **dosya** > **yeni çözüm**.

![Yeni çözüm macOS](../first-web-api-mac/_static/sln.png)

Seçin **.NET Core uygulaması** > **ASP.NET Core** > **ASP.NET Core Web uygulaması (MVC)** > **sonraki**.

![macOS yeni proje iletişim kutusu](start-mvc/1.png)

Projeyi adlandırın **MvcMovie**ve ardından **Oluştur**.

![macOS yeni proje iletişim kutusu](start-mvc/2.png)

### <a name="launch-the-app"></a>Uygulamayı başlatın

Visual Studio'da **çalıştırma** > **hata ayıklama olmadan Başlat** uygulamayı başlatın. Visual Studio başlatır [Kestrel](xref:fundamentals/servers/index#kestrel) sunucusunda, bir tarayıcı başlatır ve gider `http://localhost:port`burada *bağlantı noktası* bir rastgele seçilen bağlantı noktası numarasıdır.

![Yeni Proje ile tarayıcı](start-mvc/b1.png)

* Adres çubuğu gösterir `localhost:port#` gibi bir şey `example.com`. Çünkü `localhost` standart, yerel bilgisayar adıdır. Visual Studio, bir web projesi oluşturduğunda, web sunucusu için rastgele bir bağlantı noktası kullanılır. Uygulamayı çalıştırdığınızda, farklı bir bağlantı noktası görürsünüz.
* Uygulamada hata ayıklama veya hata ayıklama olmayan moddan başlatabilirsiniz **çalıştırma** menüsü.

Varsayılan şablonu size **giriş**, **hakkında**, ve **kişi** bağlantıları. Önceki tarayıcı resimde, bu bağlantıları göstermez. Tarayıcınız boyutuna bağlı olarak, gösterileceğinin Gezinti simgesine tıklamanız gerekebilir.

![Yeni Proje ile tarayıcı](start-mvc/b2.png)

Bu öğreticinin sonraki bölümünde, MVC konusunda bilgi ve biraz kod yazmaya başlayın.

> [!div class="step-by-step"]
> [Next](adding-controller.md)  
