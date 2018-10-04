---
title: ASP.NET Core Signalr'yi kullanmaya başlayın
author: tdykstra
description: Bu öğreticide, ASP.NET Core SignalR kullanan bir sohbet uygulaması oluşturma.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/31/2018
uid: tutorials/signalr
ms.openlocfilehash: 55fb6b1c13549129a00541c1228956a93854ad78
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48578035"
---
# <a name="tutorial-get-started-with-aspnet-core-signalr"></a>Öğretici: ASP.NET Core SignalR ile çalışmaya başlama

Bu öğreticide SignalR kullanarak gerçek zamanlı bir uygulama oluşturmaya ilişkin temel bilgileri size öğretir. Aşağıdakilerin nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Bir web uygulaması projesi oluşturun.
> * SignalR istemci kitaplığı ekleyin.
> * Bir SignalR hub'ı oluşturun.
> * Projeyi SignalR kullanacak şekilde yapılandırın.
> * Bağlanan tüm istemciler için herhangi bir istemciden ileti göndermek için hub'ı kullanan kodu ekleyin.

Sonunda, bir çalışma sohbet uygulaması oluşturmuş olacaksınız:

![SignalR örnek uygulaması](signalr/_static/signalr-get-started-finished.png)

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample)).

## <a name="prerequisites"></a>Önkoşullar

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* [Visual Studio 2017 sürüm 15,8 veya üzeri](https://www.visualstudio.com/downloads/) ile **ASP.NET ve web geliştirme** iş yükü
* [.NET core SDK 2.1 veya üzeri](https://www.microsoft.com/net/download/all)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* [Visual Studio Code](https://code.visualstudio.com/download)
* [.NET core SDK 2.1 veya üzeri](https://www.microsoft.com/net/download/all)
* [Visual Studio Code için C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

* [Sürüm 7.5.4 Mac için Visual Studio veya üzeri](https://www.visualstudio.com/downloads/)
* [.NET core SDK 2.1 veya üzeri](https://www.microsoft.com/net/download/all) (Visual Studio yüklemesine dahil)

---

## <a name="create-a-web-project"></a>Bir web projesi oluşturma

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

* Menüden **Dosya > Yeni proje**.

* İçinde **yeni proje** iletişim kutusunda **yüklü > Visual C# > Web > ASP.NET Core Web uygulaması**. Projeyi adlandırın *SignalRChat*.

  ![Visual Studio'da yeni proje iletişim kutusu](signalr/_static/signalr-new-project-dialog.png)

* Seçin **Web uygulaması** Razor sayfaları kullanan bir proje oluşturmak için.

* Bir hedef Framework'ü seçin **.NET Core**seçin **ASP.NET Core 2.1**, tıklatıp **Tamam**.

  ![Visual Studio'da yeni proje iletişim kutusu](signalr/_static/signalr-new-project-choose-type.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

* Yeni bir proje için kullanabileceğiniz bir klasöre açın.

* İçinde **tümleşik Terminalini**, aşağıdaki komutu çalıştırın:

   ```console
   dotnet new webapp -o SignalRChat
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

* Menüden **Dosya > Yeni Çözüm**.

* Seçin **.NET Core > Uygulama > ASP.NET Core Web uygulaması** (seçmeyin **ASP.NET Core Web uygulaması (MVC)**).

* Seçin **sonraki**.

* Projeyi adlandırın *SignalRChat*ve ardından **Oluştur**.

---

## <a name="add-the-signalr-client-library"></a>SignalR istemci kitaplığı Ekle

SignalR server kitaplığı dahil `Microsoft.AspNetCore.App` metapackage. JavaScript istemci kitaplığı, otomatik olarak projeye dahil değil. Bu öğretici için Kitaplık Yöneticisi'ni (LibMan) istemci kitaplığını almak için kullanmak *unpkg*. unpkg bir içerik teslim ağı (CDN) olan), teslim edebilir herhangi bir şey npm, Node.js Paket Yöneticisi bulunamadı.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

* İçinde **Çözüm Gezgini**projeye sağ tıklayıp seçin **Ekle** > **istemci tarafı kitaplık**.

* İçinde **istemci tarafı kitaplık Ekle** iletişim için **sağlayıcısı** seçin **unpkg**. 

* İçin **Kitaplığı**, girin `@aspnet/signalr@1`, Önizleme olmayan en son sürümü seçin.

  ![İstemci tarafı kitaplık iletişim - select Kütüphane ekleyin](signalr/_static/libman1.png)

* Seçin **belirli dosyaları seçin**, genişletme *dist/tarayıcı* klasörü ve select *signalr.js* ve *signalr.min.js*.

* Ayarlama **hedef konum** için *wwwroot/lib/signalr/* seçip **yükleme**.

  ![İstemci tarafı kitaplık iletişim - dosyaları seçin ve hedef Ekle](signalr/_static/libman2.png)

  LibMan oluşturur bir *wwwroot/lib/signalr* klasörü ve seçili dosyaları kopyalar.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

* İçinde **tümleşik Terminalini**, LibMan yüklemek için aşağıdaki komutu çalıştırın.

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* Proje klasörüne gidin (içeren *SignalRChat.csproj* dosyası).

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

  [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

  `ChatHub` Sınıfından devralan SignalR öğesinden `Hub` sınıfı. `Hub` Sınıfı, bağlantılar, grupları ve mesajlaşma yönetir.

  `SendMessage` Yöntemi herhangi bir bağlı istemci tarafından çağrılabilir. Bu, tüm istemcilere alınan ileti gönderir. SignalR kodudur maksimum ölçeklenebilirlik sağlamak için zaman uyumsuz.

## <a name="configure-signalr"></a>SignalR yapılandırın

SignalR sunucusu, SignalR için SignalR isteklerini iletmek için yapılandırılmalıdır.

* Aşağıdaki vurgulanmış kodu ekleyin *Startup.cs* dosya.

  [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=7,33,52-55)]

  Bu değişiklikler, ASP.NET Core bağımlılık ekleme sistemi ve ara yazılım ardışık düzenini SignalR ekleyin.

## <a name="add-signalr-client-code"></a>SignalR istemci kodu ekleyin

* İçeriği Değiştir *Pages\Index.cshtml* aşağıdaki kod ile:

  [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

  Yukarıdaki kod:

  * Metin kutuları için adı ve ileti metni ve bir Gönder düğmesi oluşturur.
  * İçeren bir liste oluşturur `id="messagesList"` SignalR hub'ından alınan iletileri görüntülemek için.
  * SignalR için komut dosyası başvuruları içerir ve *chat.js* sonraki adımda oluşturduğunuz uygulama kodu.

* İçinde *wwwroot/js* klasör oluşturma bir *chat.js* dosyasındaki kodu aşağıdaki kodla:

  [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

  Yukarıdaki kod:

  * Oluşturur ve bir bağlantı başlatır.
  * Gönder düğmesine hub'a iletiler gönderen bir işleyici ekler.
  * Bağlantı nesnesi için hub'ından iletiler alan ve bunları listeye ekler bir işleyici ekler.

## <a name="run-the-app"></a>Uygulamayı çalıştırma

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Tuşuna **CTRL + F5** uygulamayı hata ayıklama olmadan çalıştırmak için.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Tuşuna **CTRL + F5** uygulamayı hata ayıklama olmadan çalıştırmak için.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

* Menüden **çalıştırın > hata ayıklama olmadan Başlat**.

---

* Adres çubuğundan URL'yi kopyalayın, başka bir tarayıcı örneğinde veya sekmesi açın ve adres çubuğuna URL'yi yapıştırın.

* Ya da tarayıcı seçin, bir ad ve ileti girin ve seçin **Gönder** düğmesi.

  Her iki sayfalarında, adını ve iletisini anında görüntülenir.

  ![SignalR örnek uygulaması](signalr/_static/signalr-get-started-finished.png)

> [!TIP]
> Uygulama işe yaramazsa, tarayıcı Geliştirici Araçları'nı (F12) açın ve konsoluna gidin. HTML ve JavaScript kodunuza ilgili hatalar görebilirsiniz. Örneğin, eklediğiniz varsayalım *signalr.js* yönlendirilmiş değerinden farklı bir klasöre. Bu durumda bu dosyaya başvuru çalışmaz ve bir 404 hatası konsolunda görürsünüz.
> ![signalr.js bulunamadı hatası](signalr/_static/f12-console.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunları öğrendiniz: nasıl yapılır:

> [!div class="checklist"]
> * Bir web uygulaması projesi oluşturun.
> * SignalR istemci kitaplığı ekleyin.
> * Bir SignalR hub'ı oluşturun.
> * Projeyi SignalR kullanacak şekilde yapılandırın.
> * Bağlanan tüm istemciler için herhangi bir istemciden ileti göndermek için hub'ı kullanan kodu ekleyin.

SignalR hakkında daha fazla bilgi için girişine bakın:

> [!div class="nextstepaction"]
> [ASP.NET Core signalr'a giriş](xref:signalr/introduction)
