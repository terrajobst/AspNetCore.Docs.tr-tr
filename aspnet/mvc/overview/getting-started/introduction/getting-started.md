---
uid: mvc/overview/getting-started/introduction/getting-started
title: ASP.NET MVC 5 ile çalışmaya başlama | Microsoft Docs
author: Rick-Anderson
description: 'Not: Burada Visual Studio 2015 kullanarak bu öğreticinin güncelleştirilmiş bir sürümü kullanılabilir. Yeni öğretici birçok improvem sağlar ASP.NET Core MVC 6 kullanır...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: f3d8adbe-55e7-4fd4-84a8-7155bc45c676
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/getting-started
msc.type: authoredcontent
ms.openlocfilehash: 0f1fd2026691d3bc0e81b20a9731879d7a6041bb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="getting-started-with-aspnet-mvc-5"></a>.NET MVC 5 ile Çalışmaya Başlama
====================
tarafından [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [consider RP](../../../../includes/razor.md)]

 Bu öğretici bir ASP.NET MVC 5 web uygulaması kullanılarak oluşturmaya temellerini öğretmek [Visual Studio 2017](https://www.visualstudio.com/). Öğretici için son kaynak bulunan [GitHub](https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie/MvcMovie)


 Bu öğretici tarafından yazıldı [Scott Guthrie](https://weblogs.asp.net/scottgu/) (twitter[ @scottgu ](https://twitter.com/scottgu) ), [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](https://twitter.com/shanselman) ) , ve [Rick Anderson](https://twitter.com/RickAndMSFT) ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )

 Bu uygulamayı Azure'a dağıtmak için bir Azure hesabı gerekir:

 - Yapabilecekleriniz [ücretsiz bir Azure hesabı açabilirsiniz](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -krediler alırsınız, ücretli Azure hizmetlerini denemek için kullanabileceğiniz ve hatta kullanıldıktan sonra en fazla hesabı tutabilir ve ücretsiz Azure hizmetlerini kullanabilirsiniz.
 - Yapabilecekleriniz [MSDN abone Avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -MSDN aboneliğiniz size kredi verir Ücretli Azure hizmetlerinizi kullanabildiğiniz her ay.


## <a name="getting-started"></a>Başlarken

Başlangıç yüklenmesi ve çalıştırılması [Visual Studio 2017](https://www.visualstudio.com/).

Visual Studio, bir IDE veya tümleşik geliştirme ortamını değil. Belgeleri yazmak için Microsoft Word kullanma gibi uygulamaları oluşturmak için bir IDE kullanacaksınız. Visual Studio'da bir listesi için çeşitli seçenekleri gösteren alt boyunca yoktur. IDE içinde görevleri gerçekleştirmek için başka bir yol sağlayan bir menüsünü yoktur. (Örneğin, seçmek yerine **yeni proje** gelen **Başlat** sayfasında, menüsünü kullanın ve seçin **dosya** &gt; **yeni proje**.)


![](getting-started/_static/image1.png)  


## <a name="creating-your-first-application"></a>İlk uygulamanızı oluşturma

Tıklatın **yeni proje**, ardından Visual C# sol tarafta, ardından **Web** ve ardından **ASP.NET Web uygulaması (.NET Framework)**. Projeniz "MvcMovie" olarak adlandırın ve ardından **Tamam**.

![](getting-started/_static/image2.png)

İçinde **yeni ASP.NET projesi** iletişim kutusunda, tıklatın **MVC** ve ardından **Tamam**.

![](getting-started/_static/image3.png)

Visual Studio varsayılan bir şablon, yeni oluşturduğunuz, ASP.NET MVC proje için kullanılan çalışan bir uygulama şu anda herhangi bir şey yapmadan elinizde! Bu bir basit "Hello World!" olur Proje ve kullanıcının uygulamanızı başlatmak için uygun bir yerdir.

![](getting-started/_static/image4.png)

Hata ayıklamayı başlatmak için F5'e tıklayın. F5 neden başlatmak Visual Studio [IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) ve web uygulamanızı çalıştırın. Visual Studio bir tarayıcı başlatılır ve uygulamanın giriş sayfasını açar. Tarayıcının adres çubuğunda diyor bildirimi `localhost:port#` bir şey yok gibi ve `example.com`. Çünkü `localhost` her zaman, bu durumda yalnızca yerleşik uygulama çalışan kendi yerel bilgisayara gösterir. Visual Studio web projesini çalıştığında, web sunucusu için rastgele bir bağlantı noktası kullanılır. Aşağıdaki resimde 1234 bağlantı noktası numarasıdır. Uygulamayı çalıştırdığınızda, farklı bir bağlantı noktası görürsünüz.

![](getting-started/_static/image5.png)

Bu varsayılan şablonu kullanıma hazır giriş, kişi ve ilgili sayfaları sunar. Yukarıdaki resimde göstermiyor **giriş**, **hakkında** ve **kişi** bağlantılar. Tarayıcı pencerenizin boyutuna bağlı olarak, bu bağlantıları görmek için Gezinti simgesini gerekebilir.

![](getting-started/_static/image6.png)  

Uygulama kayıt ve oturum açmak için destek de sağlar. Bu uygulama şeklini değiştirmek ve biraz ASP.NET MVC hakkında bilgi almak için sonraki adımdır bakın. ASP.NET MVC uygulamasını kapatın ve biraz kod değiştirelim.

Geçerli öğreticilerin bir listesi için bkz: [makaleleri önerilen MVC](../mvc-learning-sequence.md).

## <a name="see-this-app-running-on-azure"></a>Azure üzerinde çalışan bu uygulamayı bakın

Canlı web uygulaması çalışan tamamlanmış site görmek ister misiniz? Aşağıdaki düğmeyi tıklatarak, uygulama tam sürümü Azure hesabınızda dağıtabilirsiniz.

[![](https://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?repository=https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie&amp;WT.mc_id=deploy_azure_aspnet)

Bu çözüm Azure'a dağıtmak için bir Azure hesabınız olmalıdır. Bir hesap zaten yoksa, aşağıdaki seçenekler vardır:

- [Ücretsiz bir Azure hesabı açabilirsiniz](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -krediler alırsınız, ücretli Azure hizmetlerini denemek için kullanabileceğiniz ve hatta kullanıldıktan sonra en fazla hesabı tutabilir ve ücretsiz Azure hizmetlerini kullanabilirsiniz.
- [MSDN abone Avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -MSDN aboneliğiniz size kredi verir Ücretli Azure hizmetlerinizi kullanabildiğiniz her ay.

> [!div class="step-by-step"]
> [Next](adding-a-controller.md)
