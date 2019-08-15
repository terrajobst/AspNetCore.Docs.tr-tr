---
title: ASP.NET Core MVC ile çalışmaya başlama
author: rick-anderson
description: ASP.NET Core MVC ile çalışmaya başlama hakkında bilgi edinin.
ms.author: riande
ms.date: 08/05/2019
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: 36f4811f876a6e35440445103a1f86ae06b31b6a
ms.sourcegitcommit: 476ea5ad86a680b7b017c6f32098acd3414c0f6c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/14/2019
ms.locfileid: "69022519"
---
# <a name="get-started-with-aspnet-core-mvc"></a>ASP.NET Core MVC ile çalışmaya başlama

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE [consider RP](~/includes/razor.md)]

Bu öğretici, ASP.NET Core MVC web uygulaması oluşturma hakkında temel bilgileri öğretir.

Uygulama, bir film başlıkları veritabanını yönetir. Aşağıdakilerin nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Bir Web uygulaması oluşturun.
> * Bir modeli ekleyin ve yapı iskelesi yapın.
> * Bir veritabanıyla çalışın.
> * Arama ve doğrulama ekleyin.

Sonunda, film verilerini yönetebilen ve görüntüleyebilen bir uygulamanız vardır.

[!INCLUDE[](~/includes/mvc-intro/download.md)]

## <a name="prerequisites"></a>Önkoşullar

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-web-app"></a>Web uygulaması oluşturma

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Visual Studio 'da **Yeni proje oluştur**' u seçin.

* **ASP.NET Core Web uygulaması** ' nı seçin ve ardından **İleri**' yi seçin.

![Yeni ASP.NET Core Web uygulaması](start-mvc/_static/np_2.1.png)

* Projeyi **Mvcfilmi** olarak adlandırın ve **Oluştur**' u seçin. Kodu kopyaladığınızda, ad alanının eşleşmesi için, projeyi **Mvcfilmi** olarak adlandırmak önemlidir.

  ![Yeni ASP.NET Core Web uygulaması](start-mvc/_static/config.png)

* **Web uygulaması (Model-View-Controller)** öğesini seçin ve ardından **Oluştur**' u seçin.

![Yeni proje iletişim kutusu, sol bölmede .NET Core, ASP.NET Core Web ](start-mvc/_static/new_project22-21.png)

Visual Studio, az önce oluşturduğunuz MVC projesi için varsayılan şablonu kullandı. Şimdi bir proje adı girip birkaç seçenek belirleyerek, çalışan bir uygulamanız var. Bu, temel bir başlatıcı projem.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Öğreticide familar, VS Code ile varsayılmıştır. Daha fazla bilgi için bkz. [vs Code kullanmaya](https://code.visualstudio.com/docs) başlama ve [Yardım Visual Studio Code](#visual-studio-code-help) .

* Açık [tümleşik Terminalini](https://code.visualstudio.com/docs/editor/integrated-terminal).
* Dizinleri (`cd`), projeyi içerecek bir klasöre değiştirin.
* Şu komutu çalıştırın:

   ```console
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * **Gerekli varlıkların derlenmesi ve hata ayıklaması için ' mvcfilmi ' içinde eksik olan bir iletişim kutusu görüntülenir. Bunları ekleyin mi?**  **Evet** ' i seçin

  * `dotnet new mvc -o MvcMovie`: *Mvcmovie* klasöründe yeni BIR ASP.NET Core MVC projesi oluşturur.
  * `code -r MvcMovie`: Visual Studio Code ' de *Mvcmovie. csproj* proje dosyasını yükler.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

* **Dosya** > **yeni çözüm**' ü seçin.

  ![Yeni çözüm macOS](./start-mvc/_static/new_project_vsmac.png)

* **Daha sonra** **.NET Core** > **App** > **Web uygulaması (Model-View-Controller)** > öğesini seçin.

  ![macOS yeni proje iletişim kutusu](./start-mvc/_static/new_project_mvc_vsmac.png)

* **Yeni ASP.NET Core Web API 'Nizi yapılandırın** iletişim kutusunda, **.NET Core 3,0**'nin **hedef çerçevesini** ayarlayın.

<!-- 
  ![macOS .NET Core 2.2 selection](./start-mvc/_static/new_project_22_vsmac.png)
-->

* Projeyi **Mvcfilmi**olarak adlandırın ve **Oluştur**' u seçin.

---

### <a name="run-the-app"></a>Uygulamayı çalıştırma

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Uygulamayı hata ayıklamasız modda çalıştırmak için **CTRL-F5** ' i seçin.

[!INCLUDE[](~/includes/trustCertVS.md)]

* Visual Studio [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) başlar ve uygulamayı çalıştırır. Adres çubuğunda `localhost:port#` , gibi `example.com`bir şey gösterilmediğine dikkat edin. Bunun nedeni `localhost` , Yerel bilgisayarınız için Standart ana bilgisayar adıdır. Visual Studio bir Web projesi oluşturduğunda, Web sunucusu için rastgele bir bağlantı noktası kullanılır.
* Uygulamayı CTRL + F5 (hata ayıklama modu) ile başlatmak, kod değişiklikleri yapmanıza, dosyayı kaydetmenize, tarayıcıyı yenilemanıza ve kod değişikliklerini görmenize olanak tanır. Birçok geliştirici, uygulamayı hızlı bir şekilde başlatmak ve değişiklikleri görüntülemek için hata ayıklama olmayan modu kullanmayı tercih eder.
* Hata ayıklama menü öğesinden uygulamayı hata ayıklama veya hata ayıklama olmayan modda başlatabilirsiniz:

  ![Hata ayıklama menüsü](start-mvc/_static/debug_menu.png)

* **IIS Express** düğmesini seçerek uygulamada hata ayıklaması yapabilirsiniz

  ![IIS Express](start-mvc/_static/iis_express.png)

  Aşağıdaki görüntüde uygulama gösterilmektedir:

  ![Giriş veya dizin sayfası](start-mvc/_static/home2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Hata ayıklayıcı olmadan çalıştırmak için CTRL + F5 tuşlarına basın.

[!INCLUDE[](~/includes/trustCertVSC.md)]

  Visual Studio Code, [Kestrel](xref:fundamentals/servers/kestrel)başlatır, bir tarayıcı başlatır ve şuraya gider `https://localhost:5001`. Adres çubuğu gibi `example.com`bir `localhost:port:5001` şey gösterir. Bunun nedeni `localhost` , yerel bilgisayar için Standart ana bilgisayar adıdır. Localhost yalnızca yerel bilgisayardan Web isteklerine hizmet verir.

  Uygulamayı CTRL + F5 (hata ayıklama modu) ile başlatmak, kod değişiklikleri yapmanıza, dosyayı kaydetmenize, tarayıcıyı yenilemanıza ve kod değişikliklerini görmenize olanak tanır. Birçok geliştirici, sayfayı yenilemek ve değişiklikleri görüntülemek için hata ayıklama olmayan modu kullanmayı tercih eder.

  ![Giriş veya dizin sayfası](start-mvc/_static/home2.2.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

Uygulamayı başlatmak için**hata ayıklama olmadan Başlat** ' ı seçin. >  Mac için Visual Studio, [Kestrel](xref:fundamentals/servers/index#kestrel) Server 'ı başlatır, bir tarayıcı başlatır ve `http://localhost:port`' a gider ve *bağlantı noktası* rastgele seçilmiş bir bağlantı noktası numarasıdır.

[!INCLUDE[](~/includes/trustCertMac.md)]

* Adres çubuğu gibi `example.com`bir `localhost:port#` şey gösterir. Bunun nedeni `localhost` , Yerel bilgisayarınız için Standart ana bilgisayar adıdır. Visual Studio bir Web projesi oluşturduğunda, Web sunucusu için rastgele bir bağlantı noktası kullanılır. Uygulamayı çalıştırdığınızda, farklı bir bağlantı noktası numarası görürsünüz.
* Uygulamayı **Çalıştır** menüsünden Hata Ayıkla veya hata ayıklama olmayan modda başlatabilirsiniz.

  Aşağıdaki görüntüde uygulama gösterilmektedir:

  ![Giriş veya dizin sayfası](./start-mvc/_static/output_macos.png)

---

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

Bu öğreticinin bir sonraki bölümünde, MVC hakkında bilgi edinirsiniz ve kod yazmaya başlayabilirsiniz.

> [!div class="step-by-step"]
> [Next](adding-controller.md)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE [consider RP](~/includes/razor.md)]

Bu öğretici, ASP.NET Core MVC web uygulaması oluşturma hakkında temel bilgileri öğretir.

Uygulama, bir film başlıkları veritabanını yönetir. Aşağıdakilerin nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Bir Web uygulaması oluşturun.
> * Bir modeli ekleyin ve yapı iskelesi yapın.
> * Bir veritabanıyla çalışın.
> * Arama ve doğrulama ekleyin.

Sonunda, film verilerini yönetebilen ve görüntüleyebilen bir uygulamanız vardır.

[!INCLUDE[](~/includes/mvc-intro/download.md)]

## <a name="prerequisites"></a>Önkoşullar

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---
## <a name="create-a-web-app"></a>Web uygulaması oluşturma

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Visual Studio 'da **Yeni proje oluştur**' u seçin.

* **ASP.NET Core Web uygulamasını** seçip **İleri**' yi seçin.

![Yeni ASP.NET Core Web uygulaması](start-mvc/_static/np_2.1.png)

* Projeyi **Mvcfilmi** olarak adlandırın ve **Oluştur**' u seçin. Kodu kopyaladığınızda, ad alanının eşleşmesi için, projeyi **Mvcfilmi** olarak adlandırmak önemlidir.

  ![Yeni ASP.NET Core Web uygulaması](start-mvc/_static/config.png)


* **Web uygulaması (Model-View-Controller)** öğesini seçin ve ardından **Oluştur**' u seçin.

![Yeni proje iletişim kutusu, sol bölmede .NET Core, ASP.NET Core Web ](start-mvc/_static/new_project22-21.png)

Visual Studio, az önce oluşturduğunuz MVC projesi için varsayılan şablonu kullandı. Şimdi bir proje adı girip birkaç seçenek belirleyerek, çalışan bir uygulamanız var. Bu, temel bir başlatıcı projem ve başlamak için iyi bir yerdir.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Öğreticide familar, VS Code ile varsayılmıştır. Daha fazla bilgi için bkz. [vs Code kullanmaya](https://code.visualstudio.com/docs) başlama ve [Yardım Visual Studio Code](#visual-studio-code-help) .

* Açık [tümleşik Terminalini](https://code.visualstudio.com/docs/editor/integrated-terminal).
* Dizinleri (`cd`), projeyi içerecek bir klasöre değiştirin.
* Şu komutu çalıştırın:

   ```console
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * **Gerekli varlıkların derlenmesi ve hata ayıklaması için ' mvcfilmi ' içinde eksik olan bir iletişim kutusu görüntülenir. Bunları ekleyin mi?**  **Evet** ' i seçin

  * `dotnet new mvc -o MvcMovie`: *Mvcmovie* klasöründe yeni BIR ASP.NET Core MVC projesi oluşturur.
  * `code -r MvcMovie`: Visual Studio Code ' de *Mvcmovie. csproj* proje dosyasını yükler.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

* **Dosya** > **yeni çözüm**' ü seçin.

  ![Yeni çözüm macOS](./start-mvc/_static/new_project_vsmac.png)

* **Daha sonra** **.NET Core** > **App** > **Web uygulaması (Model-View-Controller)** > öğesini seçin.

  ![macOS yeni proje iletişim kutusu](./start-mvc/_static/new_project_mvc_vsmac.png)

* **Yeni ASP.NET Core Web API 'Nizi yapılandırın** iletişim kutusunda, **.NET Core 2,2**'nin varsayılan **hedef çerçevesini** kabul edin.

  ![macOS .NET Core 2,2 seçimi](./start-mvc/_static/new_project_22_vsmac.png)

* Projeyi **Mvcfilmi**olarak adlandırın ve **Oluştur**' u seçin.

---

### <a name="run-the-app"></a>Uygulamayı çalıştırma

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Uygulamayı hata ayıklamasız modda çalıştırmak için **CTRL-F5** ' i seçin.

[!INCLUDE[](~/includes/trustCertVS.md)]

* Visual Studio [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) başlar ve uygulamayı çalıştırır. Adres çubuğunda `localhost:port#` , gibi `example.com`bir şey gösterilmediğine dikkat edin. Bunun nedeni `localhost` , Yerel bilgisayarınız için Standart ana bilgisayar adıdır. Visual Studio bir Web projesi oluşturduğunda, Web sunucusu için rastgele bir bağlantı noktası kullanılır.
* Uygulamayı CTRL + F5 (hata ayıklama modu) ile başlatmak, kod değişiklikleri yapmanıza, dosyayı kaydetmenize, tarayıcıyı yenilemanıza ve kod değişikliklerini görmenize olanak tanır. Birçok geliştirici, uygulamayı hızlı bir şekilde başlatmak ve değişiklikleri görüntülemek için hata ayıklama olmayan modu kullanmayı tercih eder.
* Hata ayıklama menü öğesinden uygulamayı hata ayıklama veya hata ayıklama olmayan modda başlatabilirsiniz:

  ![Hata ayıklama menüsü](start-mvc/_static/debug_menu.png)

* **IIS Express** düğmesini seçerek uygulamada hata ayıklaması yapabilirsiniz

  ![IIS Express](start-mvc/_static/iis_express.png)

* İzlemeye izin vermek için **kabul et** ' i seçin. Bu uygulama kişisel bilgileri izlemez. Şablon tarafından oluşturulan kod, [genel veri koruma yönetmeliği (GDPR)](xref:security/gdpr)buluşmanıza yardımcı olan varlıkları içerir.

  ![Giriş veya dizin sayfası](start-mvc/_static/privacy.png)

  Aşağıdaki görüntüde izlemeyi kabul ettikten sonra uygulama gösterilmektedir:

  ![Giriş veya dizin sayfası](start-mvc/_static/home2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Hata ayıklayıcı olmadan çalıştırmak için CTRL + F5 tuşlarına basın.

[!INCLUDE[](~/includes/trustCertVSC.md)]

  Visual Studio Code, [Kestrel](xref:fundamentals/servers/kestrel)başlatır, bir tarayıcı başlatır ve şuraya gider `https://localhost:5001`. Adres çubuğu gibi `example.com`bir `localhost:port:5001` şey gösterir. Bunun nedeni `localhost` , yerel bilgisayar için Standart ana bilgisayar adıdır. Localhost yalnızca yerel bilgisayardan Web isteklerine hizmet verir.

  Uygulamayı CTRL + F5 (hata ayıklama modu) ile başlatmak, kod değişiklikleri yapmanıza, dosyayı kaydetmenize, tarayıcıyı yenilemanıza ve kod değişikliklerini görmenize olanak tanır. Birçok geliştirici, sayfayı yenilemek ve değişiklikleri görüntülemek için hata ayıklama olmayan modu kullanmayı tercih eder.

* İzlemeye izin vermek için **kabul et** ' i seçin. Bu uygulama kişisel bilgileri izlemez. Şablon tarafından oluşturulan kod, [genel veri koruma yönetmeliği (GDPR)](xref:security/gdpr)buluşmanıza yardımcı olan varlıkları içerir.

  ![Giriş veya dizin sayfası](start-mvc/_static/privacy.png)

  Aşağıdaki görüntüde izlemeyi kabul ettikten sonra uygulama gösterilmektedir:

  ![Giriş veya dizin sayfası](start-mvc/_static/home2.2.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

Uygulamayı başlatmak için**hata ayıklama olmadan Başlat** ' ı seçin. >  Mac için Visual Studio, [Kestrel](xref:fundamentals/servers/index#kestrel) Server 'ı başlatır, bir tarayıcı başlatır ve `http://localhost:port`' a gider ve *bağlantı noktası* rastgele seçilmiş bir bağlantı noktası numarasıdır.

[!INCLUDE[](~/includes/trustCertMac.md)]

* Adres çubuğu gibi `example.com`bir `localhost:port#` şey gösterir. Bunun nedeni `localhost` , Yerel bilgisayarınız için Standart ana bilgisayar adıdır. Visual Studio bir Web projesi oluşturduğunda, Web sunucusu için rastgele bir bağlantı noktası kullanılır. Uygulamayı çalıştırdığınızda, farklı bir bağlantı noktası numarası görürsünüz.
* Uygulamayı **Çalıştır** menüsünden Hata Ayıkla veya hata ayıklama olmayan modda başlatabilirsiniz.

* İzlemeye izin vermek için **kabul et** ' i seçin. Bu uygulama kişisel bilgileri izlemez. Şablon tarafından oluşturulan kod, [genel veri koruma yönetmeliği (GDPR)](xref:security/gdpr)buluşmanıza yardımcı olan varlıkları içerir.

  ![Giriş veya dizin sayfası](./start-mvc/_static/output_privacy_macos.png)

  Aşağıdaki görüntüde izlemeyi kabul ettikten sonra uygulama gösterilmektedir:

  ![Giriş veya dizin sayfası](./start-mvc/_static/output_macos.png)

---

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

Bu öğreticinin bir sonraki bölümünde, MVC hakkında bilgi edinirsiniz ve kod yazmaya başlayabilirsiniz.

> [!div class="step-by-step"]
> [Next](adding-controller.md)

::: moniker-end
