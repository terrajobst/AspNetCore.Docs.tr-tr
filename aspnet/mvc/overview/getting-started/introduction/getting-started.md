---
uid: mvc/overview/getting-started/introduction/getting-started
title: ASP.NET MVC 5 ile çalışmaya başlama | Microsoft Docs
author: Rick-Anderson
ms.author: riande
ms.date: 10/04/2018
ms.assetid: f3d8adbe-55e7-4fd4-84a8-7155bc45c676
msc.legacyurl: /mvc/overview/getting-started/introduction/getting-started
msc.type: authoredcontent
ms.openlocfilehash: 2cc9364b815cae0207fc59784303c6a0906f1b94
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48578451"
---
<a name="getting-started-with-aspnet-mvc-5"></a>ASP.NET MVC 5 kullanmaya başlama
====================
Tarafından [Rick Anderson]((https://twitter.com/RickAndMSFT))

[!INCLUDE [consider RP](../../../../includes/razor.md)]

Bu öğreticide bir ASP.NET MVC 5 web uygulamasını kullanarak oluşturmanın temellerini öğretir [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017). Öğreticinin son kaynak kodu bulunan [GitHub](https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie/MvcMovie).

Bu öğretici tarafından yazılmıştır [Scott Guthrie](https://weblogs.asp.net/scottgu/) (twitter[ @scottgu ](https://twitter.com/scottgu) ), [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](https://twitter.com/shanselman) ) , ve [Rick Anderson](https://twitter.com/RickAndMSFT) ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )

Bu uygulamayı Azure'a dağıtmak için bir Azure hesabına ihtiyacınız vardır:

- Yapabilecekleriniz [ücretsiz bir Azure hesabı açabilirsiniz](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -KREDİLERİ edinin, ücretli Azure hizmetlerini denemek için kullanabileceğiniz ve hatta kullanıldıktan sonra en fazla hesabı tutabilir ve ücretsiz Azure hizmetlerini kullanabilirsiniz.
- Yapabilecekleriniz [MSDN abone Avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -MSDN aboneliğiniz size kredi verir, ücretli Azure hizmetlerinizi kullanabildiğiniz her ay.

## <a name="get-started"></a>Kullanmaya başlayın

Başlayın [Visual Studio 2017'yi yüklemeden](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017). Daha sonra Visual Studio'yu açın.

Visual Studio IDE, veya tümleşik geliştirme ortamı ' dir. Belgeler yazmak için Microsoft Word kullanma gibi bir IDE uygulamalar oluşturmak için kullanırsınız. Visual Studio'da alt kısmında bulunan çeşitli seçeneklerle, gösteren bir liste yoktur. IDE'de görevleri gerçekleştirmek için başka bir yol sağlayan bir menüsünde de mevcuttur. Örneğin, seçmek yerine **yeni proje** üzerinde **başlangıç sayfası**, menü çubuğunu kullanın ve seçin **dosya** > **YeniProje**.

![](getting-started/_static/image1.png)

## <a name="create-your-first-app"></a>İlk uygulamanızı oluşturma

Üzerinde **başlangıç sayfası**seçin **yeni proje**. İçinde **yeni proje** iletişim kutusunda **Visual C#** kategori solda, ardından **Web**ve ardından **ASP.NET Web uygulaması (.NET Framework)**  proje şablonu. "MvcMovie" projenizi adlandırın ve ardından **Tamam**.

![](getting-started/_static/image2.png)

İçinde **yeni ASP.NET Web uygulaması** iletişim kutusunda seçin **MVC** seçip **Tamam**.

![](getting-started/_static/image3.png)

Çalışan bir uygulama şu anda hiçbir şey yapmadan sahip olduğunuz visual Studio ASP.NET MVC projesi için az önce oluşturduğunuz varsayılan bir şablon kullanılan! Bu, bir basit "Hello World!" Proje ve kullanıcının uygulamanızı başlatmak için iyi bir yerdir.

![](getting-started/_static/image4.png)

Tuşuna **F5** hata ayıklama başlatılamıyor. Bastığınızda **F5**, Visual Studio başlatılır [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) ve web uygulamanızı çalıştırır. Visual Studio bir tarayıcı başlatır ve uygulamanın giriş sayfası açılır. Tarayıcının adres çubuğunda yazılı bildirimi `localhost:port#` gibi bir şey `example.com`. Çünkü `localhost` her zaman bu durumda yeni oluşturduğunuz uygulamayı çalıştıran kendi yerel bilgisayara işaret eder. Visual Studio web projesini çalıştığında, web sunucusu için rastgele bir bağlantı noktası kullanılır. Aşağıdaki görüntüde, bağlantı noktası numarası 1234 ise. Uygulamayı çalıştırdığınızda, farklı bir bağlantı noktası görürsünüz.

![](getting-started/_static/image5.png)

Kullanıma hazır bu varsayılan şablonu size `Home`, `Contact`, ve `About` sayfaları. Yukarıdaki resimde göstermez **giriş**, **hakkında**, ve **kişi** bağlantıları. Tarayıcı pencerenizin boyutuna bağlı olarak, bu bağlantıları görmek için Gezinti simgesi tıklamanız gerekebilir.

![](getting-started/_static/image6.png)

Uygulama kayıt ve oturum açma desteği de sağlar. Sonraki adım, bu uygulama çalışma şeklini değiştirmek ve ASP.NET MVC hakkında biraz bilgi sağlamaktır. ASP.NET MVC uygulamasını kapatın ve bazı kod değiştirelim.

Geçerli öğreticiler listesi için bkz. [makaleleri önerilen MVC](../mvc-learning-sequence.md).

## <a name="see-this-app-running-on-azure"></a>Azure üzerinde çalışan bu uygulamayı bakın

Canlı web uygulaması olarak çalışan tamamlanmış site görmek ister misiniz? Aşağıdaki düğmeye tıklayarak Azure hesabınızda bir tam sürümü uygulama dağıtabilirsiniz.

[![](https://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?repository=https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie&amp;WT.mc_id=deploy_azure_aspnet)

Bu çözüm, Azure'a dağıtmak için bir Azure hesabına ihtiyacınız var. Zaten bir hesabınız yoksa, oluşturmak için aşağıdaki seçeneklerden birini kullanın:

- [Ücretsiz bir Azure hesabı açabilirsiniz](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -KREDİLERİ edinin, ücretli Azure hizmetlerini denemek için kullanabileceğiniz ve hatta kullanıldıktan sonra en fazla hesabı tutabilir ve ücretsiz Azure hizmetlerini kullanabilirsiniz.
- [Visual Studio abone Avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers) -Your Visual Studio aboneliği size kredi verir, ücretli Azure hizmetlerinizi kullanabildiğiniz her ay.

> [!div class="step-by-step"]
> [Next](adding-a-controller.md)
