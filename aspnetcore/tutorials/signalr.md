---
title: ASP.NET Core Signalr'yi kullanmaya başlayın
author: bradygaster
description: Bu öğreticide, ASP.NET Core SignalR kullanan bir sohbet uygulaması oluşturma.
ms.author: bradyg
ms.custom: mvc
ms.date: 07/08/2019
uid: tutorials/signalr
ms.openlocfilehash: 53d3763a93cc72b6bcf85b64a706500299b3597f
ms.sourcegitcommit: 040aedca220ed24ee1726e6886daf6906f95a028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/15/2019
ms.locfileid: "67893709"
---
# <a name="tutorial-get-started-with-aspnet-core-signalr"></a>Öğretici: ASP.NET Core Signalr'yi kullanmaya başlayın

::: moniker range=">= aspnetcore-3.0"

Bu öğreticide SignalR kullanarak gerçek zamanlı bir uygulama oluşturmaya ilişkin temel bilgileri size öğretir. Aşağıdakilerin nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Web projesi oluşturun.
> * SignalR istemci kitaplığı ekleyin.
> * Bir SignalR hub'ı oluşturun.
> * Projeyi SignalR kullanacak şekilde yapılandırın.
> * Herhangi bir istemciden bağlanan tüm istemciler için iletileri gönderen kodu ekleyin.

Sonunda, bir çalışma sohbet uygulaması oluşturmuş olacaksınız:

![SignalR örnek uygulaması](signalr/_static/3.x/signalr-get-started-finished.png)

## <a name="prerequisites"></a>Önkoşullar

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-web-app-project"></a>Bir web uygulaması projesi oluşturma

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

* Menüden **Dosya > Yeni proje**.

* İçinde **yeni bir proje oluşturma** iletişim kutusunda **ASP.NET Core Web uygulaması**ve ardından **sonraki**.

* İçinde **yeni projenizi yapılandırın** iletişim kutusunda, proje adı *SignalRChat*ve ardından **Oluştur**.

* İçinde **yeni bir ASP.NET Core Web uygulaması oluşturma** iletişim kutusunda **.NET Core** ve **ASP.NET Core 3.0**. 

* Seçin **Web uygulaması** Razor sayfaları kullanan bir proje oluşturun ve ardından **Oluştur**.

  ![Visual Studio'da yeni proje iletişim kutusu](signalr/_static/3.x/signalr-new-project-dialog.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

* Açık [tümleşik Terminalini](https://code.visualstudio.com/docs/editor/integrated-terminal) yeni proje klasörü oluşturulacağı klasör.

* Aşağıdaki komutları çalıştırın:

   ```console
   dotnet new webapp -o SignalRChat
   code -r SignalRChat
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

* Menüden **Dosya > Yeni Çözüm**.

* Seçin **.NET Core > Uygulama > Web uygulaması** (seçmeyin **Web uygulaması (Model-View-Controller)** ) ve ardından **sonraki**.

* Emin **hedef Framework'ü** ayarlanır **.NET Core 3.0**ve ardından **sonraki**.

* Projeyi adlandırın *SignalRChat*ve ardından **Oluştur**.

---

## <a name="add-the-signalr-client-library"></a>SignalR istemci kitaplığı Ekle

SignalR server kitaplığı ASP.NET Core 3.0 paylaşılan framework dahil edilir. JavaScript istemci kitaplığı, otomatik olarak projeye dahil değil. Bu öğretici için Kitaplık Yöneticisi'ni (LibMan) istemci kitaplığını almak için kullanmak *unpkg*. unpkg bir içerik teslim ağı (CDN) olan), teslim edebilir herhangi bir şey npm, Node.js Paket Yöneticisi bulunamadı.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

* İçinde **Çözüm Gezgini**projeye sağ tıklayıp seçin **Ekle** > **istemci tarafı kitaplık**.

* İçinde **istemci tarafı kitaplık Ekle** iletişim için **sağlayıcısı** seçin **unpkg**.

* İçin **Kitaplığı**, girin `@aspnet/signalr@next`.
<!-- when 3.0 is released, change @next to @latest -->

* Seçin **belirli dosyaları seçin**, genişletme *dist/tarayıcı* klasörü ve select *signalr.js* ve *signalr.min.js*.

* Ayarlama **hedef konum** için *wwwroot/lib/signalr/* seçip **yükleme**.

  ![İstemci tarafı kitaplık iletişim - select Kütüphane ekleyin](signalr/_static/3.x/libman1.png)

  LibMan oluşturur bir *wwwroot/lib/signalr* klasörü ve seçili dosyaları kopyalar.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

* Tümleşik terminalde LibMan yüklemek için aşağıdaki komutu çalıştırın.

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* SignalR istemci kitaplığı LibMan almak için aşağıdaki komutu çalıştırın. Çıktıyı görmeye önce birkaç saniye beklemeniz gerekebilir.

  ```console
  libman install @aspnet/signalr@next -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  Parametreleri aşağıdaki seçenekleri belirtin:
  * Unpkg sağlayıcısını kullanın.
  * Dosyaları kopyalamak *wwwroot/lib/signalr* hedef.
  * Yalnızca belirtilen dosyaları kopyalayın.

  Çıktı aşağıdaki örnekteki gibi görünür:

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@next" to "wwwroot/lib/signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

* İçinde **Terminal**, LibMan yüklemek için aşağıdaki komutu çalıştırın.

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* Proje klasörüne gidin (içeren *SignalRChat.csproj* dosyası).

* SignalR istemci kitaplığı LibMan almak için aşağıdaki komutu çalıştırın.

  ```console
  libman install @aspnet/signalr@next -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  Parametreleri aşağıdaki seçenekleri belirtin:
  * Unpkg sağlayıcısını kullanın.
  * Dosyaları kopyalamak *wwwroot/lib/signalr* hedef.
  * Yalnızca belirtilen dosyaları kopyalayın.

  Çıktı aşağıdaki örnekteki gibi görünür:

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@next" to "wwwroot/lib/signalr"
  ```

---

## <a name="create-a-signalr-hub"></a>Bir SignalR hub'ı oluşturma

A *hub* istemci-sunucu iletişimi işleyen bir üst düzey işlem hattı hizmet veren bir sınıftır.

* Oluşturma SignalRChat proje klasöründe bir *Hubs* klasör.

* İçinde *Hubs* klasör oluşturma bir *ChatHub.cs* dosyasındaki kodu aşağıdaki kodla:

  [!code-csharp[Startup](signalr/sample-snapshot/3.x/ChatHub.cs)]

  `ChatHub` Sınıfından devralan SignalR öğesinden `Hub` sınıfı. `Hub` Sınıfı, bağlantılar, grupları ve mesajlaşma yönetir.

  `SendMessage` Yöntemi, tüm istemciler için bir ileti göndermek için bir bağlı istemci tarafından çağrılabilir. Bu yöntemi çağıran JavaScript istemci kodu öğreticinin ilerleyen bölümlerinde gösterilmektedir. SignalR kodudur maksimum ölçeklenebilirlik sağlamak için zaman uyumsuz.

## <a name="configure-signalr"></a>SignalR yapılandırın

SignalR sunucusu, SignalR için SignalR isteklerini iletmek için yapılandırılmalıdır.

* Aşağıdaki vurgulanmış kodu ekleyin *Startup.cs* dosya.

  [!code-csharp[Startup](signalr/sample-snapshot/3.x/Startup.cs?highlight=6,30,58)]

  Bu değişiklikler, ASP.NET Core bağımlılık ekleme ve sistemleri yönlendirme SignalR ekleyin.

## <a name="add-signalr-client-code"></a>SignalR istemci kodu ekleyin

* İçeriği Değiştir *Pages\Index.cshtml* aşağıdaki kod ile:

  [!code-cshtml[Index](signalr/sample-snapshot/3.x/Index.cshtml)]

  Yukarıdaki kod:

  * Metin kutuları için adı ve ileti metni ve bir Gönder düğmesi oluşturur.
  * İçeren bir liste oluşturur `id="messagesList"` SignalR hub'ından alınan iletileri görüntülemek için.
  * SignalR için komut dosyası başvuruları içerir ve *chat.js* sonraki adımda oluşturduğunuz uygulama kodu.

* İçinde *wwwroot/js* klasör oluşturma bir *chat.js* dosyasındaki kodu aşağıdaki kodla:

  [!code-javascript[Index](signalr/sample-snapshot/3.x/chat.js)]

  Yukarıdaki kod:

  * Oluşturur ve bir bağlantı başlatır.
  * Gönder düğmesine hub'a iletiler gönderen bir işleyici ekler.
  * Bağlantı nesnesi için hub'ından iletiler alan ve bunları listeye ekler bir işleyici ekler.

## <a name="run-the-app"></a>Uygulamayı çalıştırma

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Tuşuna **CTRL + F5** uygulamayı hata ayıklama olmadan çalıştırmak için.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Tümleşik terminalde aşağıdaki komutu çalıştırın:

  ```console
  dotnet run -p SignalRChat.csproj
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

* Menüden **çalıştırın > hata ayıklama olmadan Başlat**.

---

* Adres çubuğundan URL'yi kopyalayın, başka bir tarayıcı örneğinde veya sekmesi açın ve adres çubuğuna URL'yi yapıştırın.

* Ya da tarayıcı seçin, bir ad ve ileti girin ve seçin **iletisi gönder** düğmesi.

  Her iki sayfalarında, adını ve iletisini anında görüntülenir.

  ![SignalR örnek uygulaması](signalr/_static/3.x/signalr-get-started-finished.png)

> [!TIP]
> * Uygulama işe yaramazsa, tarayıcı Geliştirici Araçları'nı (F12) açın ve konsoluna gidin. HTML ve JavaScript kodunuza ilgili hatalar görebilirsiniz. Örneğin, eklediğiniz varsayalım *signalr.js* yönlendirilmiş değerinden farklı bir klasöre. Bu durumda bu dosyaya başvuru çalışmaz ve bir 404 hatası konsolunda görürsünüz.
>   ![signalr.js bulunamadı hatası](signalr/_static/3.x/f12-console.png)
> * Chrome'da ERR_SPDY_INADEQUATE_TRANSPORT_SECURITY veya NS_ERROR_NET_INADEQUATE_SECURITY Firefox'ta hata alırsanız, geliştirme sertifikanızı güncelleştirmek için şu komutları çalıştırın:
>   ```
>   dotnet dev-certs https --clean
>   dotnet dev-certs https --trust
>   ```

## <a name="next-steps"></a>Sonraki adımlar

SignalR hakkında daha fazla bilgi için girişine bakın:

> [!div class="nextstepaction"]
> [ASP.NET Core signalr'a giriş](xref:signalr/introduction)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Bu öğreticide SignalR kullanarak gerçek zamanlı bir uygulama oluşturmaya ilişkin temel bilgileri size öğretir. Aşağıdakilerin nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Web projesi oluşturun.
> * SignalR istemci kitaplığı ekleyin.
> * Bir SignalR hub'ı oluşturun.
> * Projeyi SignalR kullanacak şekilde yapılandırın.
> * Herhangi bir istemciden bağlanan tüm istemciler için iletileri gönderen kodu ekleyin.

Sonunda, bir çalışma sohbet uygulaması oluşturmuş olacaksınız:

![SignalR örnek uygulaması](signalr/_static/2.x/signalr-get-started-finished.png)

## <a name="prerequisites"></a>Önkoşullar

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2017-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-web-project"></a>Bir web projesi oluşturma

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

* Menüden **Dosya > Yeni proje**.

* İçinde **yeni proje** iletişim kutusunda **yüklü > Visual C# > Web > ASP.NET Core Web uygulaması**. Projeyi adlandırın *SignalRChat*.

  ![Visual Studio'da yeni proje iletişim kutusu](signalr/_static/2.x/signalr-new-project-dialog.png)

* Seçin **Web uygulaması** Razor sayfaları kullanan bir proje oluşturmak için.

* Bir hedef Framework'ü seçin **.NET Core**seçin **ASP.NET Core 2.2**, tıklatıp **Tamam**.

  ![Visual Studio'da yeni proje iletişim kutusu](signalr/_static/2.x/signalr-new-project-choose-type.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

* Açık [tümleşik Terminalini](https://code.visualstudio.com/docs/editor/integrated-terminal) yeni proje klasörü oluşturulacağı klasör.

* Aşağıdaki komutları çalıştırın:

   ```console
   dotnet new webapp -o SignalRChat
   code -r SignalRChat
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

* Menüden **Dosya > Yeni Çözüm**.

* Seçin **.NET Core > Uygulama > ASP.NET Core Web uygulaması** (seçmeyin **ASP.NET Core Web uygulaması (MVC)** ).

* **İleri**’yi seçin.

* Projeyi adlandırın *SignalRChat*ve ardından **Oluştur**.

---

## <a name="add-the-signalr-client-library"></a>SignalR istemci kitaplığı Ekle

SignalR server kitaplığı dahil `Microsoft.AspNetCore.App` metapackage. JavaScript istemci kitaplığı, otomatik olarak projeye dahil değil. Bu öğretici için Kitaplık Yöneticisi'ni (LibMan) istemci kitaplığını almak için kullanmak *unpkg*. unpkg bir içerik teslim ağı (CDN) olan), teslim edebilir herhangi bir şey npm, Node.js Paket Yöneticisi bulunamadı.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

* İçinde **Çözüm Gezgini**projeye sağ tıklayıp seçin **Ekle** > **istemci tarafı kitaplık**.

* İçinde **istemci tarafı kitaplık Ekle** iletişim için **sağlayıcısı** seçin **unpkg**.

* İçin **Kitaplığı**, girin `@aspnet/signalr@1`, Önizleme olmayan en son sürümü seçin.

  ![İstemci tarafı kitaplık iletişim - select Kütüphane ekleyin](signalr/_static/2.x/libman1.png)

* Seçin **belirli dosyaları seçin**, genişletme *dist/tarayıcı* klasörü ve select *signalr.js* ve *signalr.min.js*.

* Ayarlama **hedef konum** için *wwwroot/lib/signalr/* seçip **yükleme**.

  ![İstemci tarafı kitaplık iletişim - dosyaları seçin ve hedef Ekle](signalr/_static/2.x/libman2.png)

  LibMan oluşturur bir *wwwroot/lib/signalr* klasörü ve seçili dosyaları kopyalar.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

* Tümleşik terminalde LibMan yüklemek için aşağıdaki komutu çalıştırın.

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* SignalR istemci kitaplığı LibMan almak için aşağıdaki komutu çalıştırın. Çıktıyı görmeye önce birkaç saniye beklemeniz gerekebilir.

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  Parametreleri aşağıdaki seçenekleri belirtin:
  * Unpkg sağlayıcısını kullanın.
  * Dosyaları kopyalamak *wwwroot/lib/signalr* hedef.
  * Yalnızca belirtilen dosyaları kopyalayın.

  Çıktı aşağıdaki örnekteki gibi görünür:

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

* İçinde **Terminal**, LibMan yüklemek için aşağıdaki komutu çalıştırın.

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* Proje klasörüne gidin (içeren *SignalRChat.csproj* dosyası).

* SignalR istemci kitaplığı LibMan almak için aşağıdaki komutu çalıştırın.

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  Parametreleri aşağıdaki seçenekleri belirtin:
  * Unpkg sağlayıcısını kullanın.
  * Dosyaları kopyalamak *wwwroot/lib/signalr* hedef.
  * Yalnızca belirtilen dosyaları kopyalayın.

  Çıktı aşağıdaki örnekteki gibi görünür:

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

---

## <a name="create-a-signalr-hub"></a>Bir SignalR hub'ı oluşturma

A *hub* istemci-sunucu iletişimi işleyen bir üst düzey işlem hattı hizmet veren bir sınıftır.

* Oluşturma SignalRChat proje klasöründe bir *Hubs* klasör.

* İçinde *Hubs* klasör oluşturma bir *ChatHub.cs* dosyasındaki kodu aşağıdaki kodla:

  [!code-csharp[Startup](signalr/sample-snapshot/2.x/ChatHub.cs)]

  `ChatHub` Sınıfından devralan SignalR öğesinden `Hub` sınıfı. `Hub` Sınıfı, bağlantılar, grupları ve mesajlaşma yönetir.

  `SendMessage` Yöntemi, tüm istemciler için bir ileti göndermek için bir bağlı istemci tarafından çağrılabilir. Bu yöntemi çağıran JavaScript istemci kodu öğreticinin ilerleyen bölümlerinde gösterilmektedir. SignalR kodudur maksimum ölçeklenebilirlik sağlamak için zaman uyumsuz.

## <a name="configure-signalr"></a>SignalR yapılandırın

SignalR sunucusu, SignalR için SignalR isteklerini iletmek için yapılandırılmalıdır.

* Aşağıdaki vurgulanmış kodu ekleyin *Startup.cs* dosya.

  [!code-csharp[Startup](signalr/sample-snapshot/2.x/Startup.cs?highlight=7,33,52-55)]

  Bu değişiklikler, ASP.NET Core bağımlılık ekleme sistemi ve ara yazılım ardışık düzenini SignalR ekleyin.

## <a name="add-signalr-client-code"></a>SignalR istemci kodu ekleyin

* İçeriği Değiştir *Pages\Index.cshtml* aşağıdaki kod ile:

  [!code-cshtml[Index](signalr/sample-snapshot/2.x/Index.cshtml)]

  Yukarıdaki kod:

  * Metin kutuları için adı ve ileti metni ve bir Gönder düğmesi oluşturur.
  * İçeren bir liste oluşturur `id="messagesList"` SignalR hub'ından alınan iletileri görüntülemek için.
  * SignalR için komut dosyası başvuruları içerir ve *chat.js* sonraki adımda oluşturduğunuz uygulama kodu.

* İçinde *wwwroot/js* klasör oluşturma bir *chat.js* dosyasındaki kodu aşağıdaki kodla:

  [!code-javascript[Index](signalr/sample-snapshot/2.x/chat.js)]

  Yukarıdaki kod:

  * Oluşturur ve bir bağlantı başlatır.
  * Gönder düğmesine hub'a iletiler gönderen bir işleyici ekler.
  * Bağlantı nesnesi için hub'ından iletiler alan ve bunları listeye ekler bir işleyici ekler.

## <a name="run-the-app"></a>Uygulamayı çalıştırma

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Tuşuna **CTRL + F5** uygulamayı hata ayıklama olmadan çalıştırmak için.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Tümleşik terminalde aşağıdaki komutu çalıştırın:

  ```console
  dotnet run -p SignalRChat.csproj
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

* Menüden **çalıştırın > hata ayıklama olmadan Başlat**.

---

* Adres çubuğundan URL'yi kopyalayın, başka bir tarayıcı örneğinde veya sekmesi açın ve adres çubuğuna URL'yi yapıştırın.

* Ya da tarayıcı seçin, bir ad ve ileti girin ve seçin **iletisi gönder** düğmesi.

  Her iki sayfalarında, adını ve iletisini anında görüntülenir.

  ![SignalR örnek uygulaması](signalr/_static/2.x/signalr-get-started-finished.png)

> [!TIP]
> Uygulama işe yaramazsa, tarayıcı Geliştirici Araçları'nı (F12) açın ve konsoluna gidin. HTML ve JavaScript kodunuza ilgili hatalar görebilirsiniz. Örneğin, eklediğiniz varsayalım *signalr.js* yönlendirilmiş değerinden farklı bir klasöre. Bu durumda bu dosyaya başvuru çalışmaz ve bir 404 hatası konsolunda görürsünüz.
> ![signalr.js bulunamadı hatası](signalr/_static/2.x/f12-console.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Bir web uygulaması projesi oluşturun.
> * SignalR istemci kitaplığı ekleyin.
> * Bir SignalR hub'ı oluşturun.
> * Projeyi SignalR kullanacak şekilde yapılandırın.
> * Bağlanan tüm istemciler için herhangi bir istemciden ileti göndermek için hub'ı kullanan kodu ekleyin.

SignalR hakkında daha fazla bilgi için girişine bakın:

> [!div class="nextstepaction"]
> [ASP.NET Core signalr'a giriş](xref:signalr/introduction)

::: moniker-end
