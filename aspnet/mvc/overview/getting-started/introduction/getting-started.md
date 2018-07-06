---
uid: mvc/overview/getting-started/introduction/getting-started
title: ASP.NET MVC 5 ile çalışmaya başlama | Microsoft Docs
author: Rick-Anderson
description: "Not: Burada Visual Studio 2015'i kullanarak bu öğreticinin güncelleştirilmiş bir sürümü kullanılabilir. Yeni birçok improvem sağlayan ASP.NET Core MVC 6, öğreticide..."
ms.author: aspnetcontent
ms.date: 05/28/2015
ms.assetid: f3d8adbe-55e7-4fd4-84a8-7155bc45c676
msc.legacyurl: /mvc/overview/getting-started/introduction/getting-started
msc.type: authoredcontent
ms.openlocfilehash: 85d5d3292ff99ade6995c710e2728c41255def4c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37823222"
---
<a name="getting-started-with-aspnet-mvc-5"></a>.NET MVC 5 ile Çalışmaya Başlama
====================
Tarafından [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [consider RP](../../../../includes/razor.md)]

 Bu öğreticide bir ASP.NET MVC 5 web uygulamasını kullanarak oluşturmanın temellerini yardımcı olacak [Visual Studio 2017](https://www.visualstudio.com/). Öğretici için son kaynak bulunan [GitHub](https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie/MvcMovie)


 Bu öğretici tarafından yazılmıştır [Scott Guthrie](https://weblogs.asp.net/scottgu/) (twitter[ @scottgu ](https://twitter.com/scottgu) ), [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](https://twitter.com/shanselman) ) , ve [Rick Anderson](https://twitter.com/RickAndMSFT) ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )

 Bu uygulamayı Azure'a dağıtmak için bir Azure hesabına ihtiyacınız vardır:

 - Yapabilecekleriniz [ücretsiz bir Azure hesabı açabilirsiniz](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -KREDİLERİ edinin, ücretli Azure hizmetlerini denemek için kullanabileceğiniz ve hatta kullanıldıktan sonra en fazla hesabı tutabilir ve ücretsiz Azure hizmetlerini kullanabilirsiniz.
 - Yapabilecekleriniz [MSDN abone Avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -MSDN aboneliğiniz size kredi verir, ücretli Azure hizmetlerinizi kullanabildiğiniz her ay.


## <a name="getting-started"></a>Başlarken

Yükleme ve çalıştırmaya başlayın [Visual Studio 2017](https://www.visualstudio.com/).

Visual Studio IDE, veya tümleşik geliştirme ortamı ' dir. Belgeler yazmak için Microsoft Word kullanma gibi bir IDE uygulamalar oluşturmak için kullanırsınız. Visual Studio'da alt kısmında bulunan çeşitli seçeneklerle, gösteren bir liste yoktur. IDE'de görevleri gerçekleştirmek için başka bir yol sağlayan bir menüsünde de mevcuttur. (Örneğin seçmek yerine **yeni proje** gelen **Başlat** sayfasında menüsünü kullanın ve seçin **dosya** &gt; **YeniProje**.)


![](getting-started/_static/image1.png)  


## <a name="creating-your-first-application"></a>İlk uygulamanızı oluşturma

Tıklayın **yeni proje**, ardından Visual C# sol tarafta, ardından **Web** seçip **ASP.NET Web uygulaması (.NET Framework)**. "MvcMovie" projenizi adlandırın ve ardından **Tamam**.

![](getting-started/_static/image2.png)

İçinde **yeni ASP.NET projesi** iletişim kutusunda, tıklayın **MVC** ve ardından **Tamam**.

![](getting-started/_static/image3.png)

Çalışan bir uygulama şu anda hiçbir şey yapmadan sahip olduğunuz visual Studio ASP.NET MVC projesi için az önce oluşturduğunuz varsayılan bir şablon kullanılan! Bu, bir basit "Hello World!" Proje ve kullanıcının uygulamanızı başlatmak için iyi bir yerdir.

![](getting-started/_static/image4.png)

Hata ayıklamayı başlatmak için F5'e tıklayın. F5 başlatmak Visual Studio neden [IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) web uygulamanızı oluşturup çalıştırdınız. Visual Studio bir tarayıcı başlatır ve uygulamanın giriş sayfası açılır. Tarayıcının adres çubuğunda yazılı bildirimi `localhost:port#` gibi bir şey `example.com`. Çünkü `localhost` her zaman bu durumda yeni oluşturduğunuz uygulamayı çalıştıran kendi yerel bilgisayara işaret eder. Visual Studio web projesini çalıştığında, web sunucusu için rastgele bir bağlantı noktası kullanılır. Aşağıdaki görüntüde, bağlantı noktası numarası 1234 ise. Uygulamayı çalıştırdığınızda, farklı bir bağlantı noktası görürsünüz.

![](getting-started/_static/image5.png)

Kullanıma hazır bu varsayılan şablonu, ev, kişi ve ilgili sayfaları sunar. Yukarıdaki resimde göstermez **giriş**, **hakkında** ve **kişi** bağlantıları. Tarayıcı pencerenizin boyutuna bağlı olarak, bu bağlantıları görmek için Gezinti simgesi tıklamanız gerekebilir.

![](getting-started/_static/image6.png)  

Uygulama kayıt ve oturum açma desteği de sağlar. Sonraki adım, bu uygulama çalışma şeklini değiştirmek ve ASP.NET MVC hakkında biraz bilgi sağlamaktır. ASP.NET MVC uygulamasını kapatın ve bazı kod değiştirelim.

Geçerli öğreticiler listesi için bkz. [makaleleri önerilen MVC](../mvc-learning-sequence.md).

## <a name="see-this-app-running-on-azure"></a>Azure üzerinde çalışan bu uygulamayı bakın

Canlı web uygulaması olarak çalışan tamamlanmış site görmek ister misiniz? Aşağıdaki düğmeye tıklayarak Azure hesabınızda bir tam sürümü uygulama dağıtabilirsiniz.

[![](https://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?repository=https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie&amp;WT.mc_id=deploy_azure_aspnet)

Bu çözüm, Azure'a dağıtmak için bir Azure hesabına ihtiyacınız var. Bir hesap zaten yoksa, aşağıdaki seçenekleriniz:

- [Ücretsiz bir Azure hesabı açabilirsiniz](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -KREDİLERİ edinin, ücretli Azure hizmetlerini denemek için kullanabileceğiniz ve hatta kullanıldıktan sonra en fazla hesabı tutabilir ve ücretsiz Azure hizmetlerini kullanabilirsiniz.
- [MSDN abone Avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -MSDN aboneliğiniz size kredi verir, ücretli Azure hizmetlerinizi kullanabildiğiniz her ay.

> [!div class="step-by-step"]
> [Next](adding-a-controller.md)
