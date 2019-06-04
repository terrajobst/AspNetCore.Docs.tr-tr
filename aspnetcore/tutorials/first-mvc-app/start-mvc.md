---
title: ASP.NET Core MVC ile çalışmaya başlama
author: rick-anderson
description: ASP.NET Core MVC ile çalışmaya başlama hakkında bilgi edinin.
ms.author: riande
ms.date: 04/24/2019
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: dc3499c89860190b76d6be7b8abeeaef827880d6
ms.sourcegitcommit: a1364109d11d414121a6337b611bee61d6e489e9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66491254"
---
# <a name="get-started-with-aspnet-core-mvc"></a>ASP.NET Core MVC ile çalışmaya başlama

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [consider RP](~/includes/razor.md)]

Bu öğreticide bir ASP.NET Core MVC web uygulaması oluşturmaya ilişkin temel bilgileri size öğretir.

Uygulama bir veritabanı başlık yönetir. Aşağıdakilerin nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Bir web uygulaması oluşturun.
> * Ekleme ve bir modeli iskelesini.
> * Bir veritabanı ile çalışır.
> * Arama ve doğrulama ekleyin.

Sonunda, yönetmek ve film verileri görüntüleyen bir uygulama vardır.

[!INCLUDE[](~/includes/mvc-intro/download.md)]

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-web-app"></a>Web uygulaması oluşturma

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Visual Studio seçin **yeni bir proje oluşturma**.

* Selecct **ASP.NET Core Web uygulaması** seçip **sonraki**.

![Yeni ASP.NET Core Web uygulaması](start-mvc/_static/np_2.1.png)

* Projeyi adlandırın **MvcMovie** seçip **Oluştur**. Projeyi adlandırın önemlidir **MvcMovie** kod kopyaladığınızda, ad alanı eşleşmesi.

  ![Yeni ASP.NET Core Web uygulaması](start-mvc/_static/config.png)


* Seçin **Web Application(Model-View-Controller)** ve ardından **Oluştur**.

![Yeni Proje iletişim kutusunda, sol bölmede, ASP.NET Core web .NET Core ](start-mvc/_static/new_project22-21.png)

Visual Studio, yeni oluşturduğunuz MVC projesi için varsayılan şablonu kullanılır. Çalışan bir uygulamayı şu anda bir proje adı girerek ve bazı Seçenekler'i seçerek sizde. Bu temel başlangıç projesini ve başlatmak için iyi bir yerdir.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Bu öğretici, VS Code ile familarity varsayar. Bkz: [VS Code ile çalışmaya başlama](https://code.visualstudio.com/docs) ve [Visual Studio Code Yardım](#visual-studio-code-help) daha fazla bilgi için.

* Açık [tümleşik Terminalini](https://code.visualstudio.com/docs/editor/integrated-terminal).
* Dizinleri (`cd`) proje içeren bir klasör.
* Şu komutu çalıştırın:

   ```console
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * Bir iletişim kutusu görünür **gerekli varlıkları oluşturun ve hata ayıklama 'MvcMovie' eksik. Bunları eklensin mi?**  Seçin **Evet**

  * `dotnet new mvc -o MvcMovie`: yeni bir ASP.NET Core MVC projesindeki oluşturur *MvcMovie* klasör.
  * `code -r MvcMovie`: Yükleri *MvcMovie.csproj* proje dosyası Visual Studio code'da.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

* Seçin **dosya** > **yeni çözüm**.

  ![Yeni çözüm macOS](./start-mvc/_static/new_project_vsmac.png)

* Seçin **.NET Core** > **uygulama** > **Web uygulaması (Model-View-Controller)**  > **sonraki**.

  ![macOS yeni proje iletişim kutusu](./start-mvc/_static/new_project_mvc_vsmac.png)

* İçinde **, yeni ASP.NET Core Web API'sini yapılandırma** iletişim kutusunda varsayılan değerleri kabul **hedef Framework'ü** , **.NET Core 2.2**.

  ![macOS .NET Core 2.2 seçimi](./start-mvc/_static/new_project_22_vsmac.png)

* Projeyi adlandırın **MvcMovie**ve ardından **Oluştur**.

---

### <a name="run-the-app"></a>Uygulamayı çalıştırma

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Seçin **Ctrl-F5** uygulamayı olmayan hata ayıklama modunda çalıştırmak için.

[!INCLUDE[](~/includes/trustCertVS.md)]

* Visual Studio başlatır [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) ve uygulamayı çalıştırır. Adres çubuğuna gösteren uyarı `localhost:port#` gibi bir şey `example.com`. Çünkü `localhost` standart, yerel bilgisayar adıdır. Visual Studio, bir web projesi oluşturduğunda, web sunucusu için rastgele bir bağlantı noktası kullanılır.
* Ctrl + F5 (hata ayıklama olmayan mod) ile uygulamayı çalıştırdığınızda, kod değişiklikleri yapabilir, dosyayı kaydetmek, tarayıcıyı yenileyin ve kod değişikliklerini görebilirsiniz olanak tanır. Geliştiricilerin çoğu, hızlı bir şekilde uygulamayı başlatın ve değişiklikleri görmek için hata ayıklama olmayan modu kullanmayı tercih eder.
* Uygulamada hata ayıklama veya hata ayıklama olmayan moddan başlatabilirsiniz **hata ayıklama** menü öğesi:

  ![Menü hata ayıklama](start-mvc/_static/debug_menu.png)

* Seçerek uygulama hatalarını ayıklayabilir **IIS Express** düğmesi

  ![IIS Express](start-mvc/_static/iis_express.png)

* Seçin **kabul** izleme için onay verme. Bu uygulama, kişisel bilgi izlemez. Oluşturulan şablon kodunun karşılamanıza yardımcı olmak üzere varlıkları içeren [genel veri koruma yönetmeliği (GDPR)](xref:security/gdpr).

  ![Giriş ya da dizin sayfası](start-mvc/_static/privacy.png)

  Aşağıdaki görüntüde, izleme kabul ettikten sonra uygulama gösterilir:

  ![Giriş ya da dizin sayfası](start-mvc/_static/home2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Hata Ayıklayıcı olmadan çalıştırmak için CTRL + F5 tuşlarına basın.

[!INCLUDE[](~/includes/trustCertVSC.md)]

  Visual Studio Code başlar [Kestrel](xref:fundamentals/servers/kestrel), bir tarayıcı başlatır ve gider `https://localhost:5001`. Adres çubuğu gösterir `localhost:port:5001` gibi bir şey `example.com`. Çünkü `localhost` standart yerel bilgisayar adıdır. Localhost yalnızca yerel bilgisayara gelen web isteklerini işlevi görür.

  Ctrl + F5 (hata ayıklama olmayan mod) ile uygulamayı çalıştırdığınızda, kod değişiklikleri yapabilir, dosyayı kaydetmek, tarayıcıyı yenileyin ve kod değişikliklerini görebilirsiniz olanak tanır. Geliştiricilerin çoğu, sayfayı yenileyin ve değişiklikleri görüntülemek için hata ayıklama olmayan modu kullanmayı tercih eder.

* Seçin **kabul** izleme için onay verme. Bu uygulama, kişisel bilgi izlemez. Oluşturulan şablon kodunun karşılamanıza yardımcı olmak üzere varlıkları içeren [genel veri koruma yönetmeliği (GDPR)](xref:security/gdpr).

  ![Giriş ya da dizin sayfası](start-mvc/_static/privacy.png)

  Aşağıdaki görüntüde, izleme kabul ettikten sonra uygulama gösterilir:

  ![Giriş ya da dizin sayfası](start-mvc/_static/home2.2.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

Seçin **çalıştırma** > **hata ayıklama olmadan Başlat** uygulamayı başlatın. Başlatıldığında Mac için Visual Studio [Kestrel](xref:fundamentals/servers/index#kestrel) sunucusunda, bir tarayıcı başlatır ve gider `http://localhost:port`burada *bağlantı noktası* bir rastgele seçilen bağlantı noktası numarasıdır.

[!INCLUDE[](~/includes/trustCertMac.md)]

* Adres çubuğu gösterir `localhost:port#` gibi bir şey `example.com`. Çünkü `localhost` standart, yerel bilgisayar adıdır. Visual Studio, bir web projesi oluşturduğunda, web sunucusu için rastgele bir bağlantı noktası kullanılır. Uygulamayı çalıştırdığınızda, farklı bir bağlantı noktası görürsünüz.
* Uygulamada hata ayıklama veya hata ayıklama olmayan moddan başlatabilirsiniz **çalıştırma** menüsü.

* Seçin **kabul** izleme için onay verme. Bu uygulama, kişisel bilgi izlemez. Oluşturulan şablon kodunun karşılamanıza yardımcı olmak üzere varlıkları içeren [genel veri koruma yönetmeliği (GDPR)](xref:security/gdpr).

  ![Giriş ya da dizin sayfası](./start-mvc/_static/output_privacy_macos.png)

  Aşağıdaki görüntüde, izleme kabul ettikten sonra uygulama gösterilir:

  ![Giriş ya da dizin sayfası](./start-mvc/_static/output_macos.png)

---

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

Bu öğreticinin sonraki bölümünde, MVC konusunda bilgi ve biraz kod yazmaya başlayın.

> [!div class="step-by-step"]
> [Next](adding-controller.md)
