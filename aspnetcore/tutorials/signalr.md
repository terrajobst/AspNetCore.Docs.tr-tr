---
title: ASP.NET Core SignalR kullanmaya başlama
author: bradygaster
description: Bu öğreticide, ASP.NET Core SignalRkullanan bir sohbet uygulaması oluşturacaksınız.
ms.author: bradyg
ms.custom: mvc
ms.date: 11/21/2019
no-loc:
- SignalR
uid: tutorials/signalr
ms.openlocfilehash: 55ebdbfa4556deca74a6cdf0638307425cd1a01a
ms.sourcegitcommit: 3e503ef510008e77be6dd82ee79213c9f7b97607
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/22/2019
ms.locfileid: "74317495"
---
# <a name="tutorial-get-started-with-aspnet-core-opno-locsignalr"></a>Öğretici: ASP.NET Core SignalR ile çalışmaya başlama

::: moniker range=">= aspnetcore-3.0"

Bu öğreticide SignalRkullanarak gerçek zamanlı bir uygulama oluşturmanın temelleri öğretilir. Aşağıdakilerin nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Web projesi oluşturun.
> * SignalR istemci kitaplığı ekleyin.
> * SignalR hub 'ı oluşturun.
> * Projeyi SignalRkullanacak şekilde yapılandırın.
> * Herhangi bir istemciden tüm bağlı istemcilere ileti gönderen kodu ekleyin.

Sonunda, çalışan bir sohbet uygulamanız olacaktır:

![[! Üs. NO-LOC (SignalR)] örnek uygulama](signalr/_static/3.x/signalr-get-started-finished.png)

## <a name="prerequisites"></a>Önkoşullar

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-web-app-project"></a>Web uygulaması projesi oluşturma

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

* Menüden **dosya > yeni proje**' yi seçin.

* **Yeni proje oluştur** iletişim kutusunda **ASP.NET Core Web uygulaması**' nı seçin ve ardından **İleri**' yi seçin.

* **Yeni projenizi yapılandırın** iletişim kutusunda, proje *signalrchat*adını adlandırın ve ardından **Oluştur**' u seçin.

* **Yeni bir ASP.NET Core Web uygulaması oluştur** iletişim kutusunda **.net Core** ve **ASP.NET Core 3,0**' i seçin. 

* Razor Pages kullanan bir proje oluşturmak için **Web uygulaması** ' nı seçin ve ardından **Oluştur**' u seçin.

  ![Visual Studio 'da yeni proje iletişim kutusu](signalr/_static/3.x/signalr-new-project-dialog.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

* [Tümleşik Terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) ' i yeni proje klasörünün oluşturulacağı klasöre açın.

* Aşağıdaki komutları çalıştırın:

   ```dotnetcli
   dotnet new webapp -o SignalRChat
   code -r SignalRChat
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

* Menüden **dosya > yeni çözüm**' i seçin.

* **.NET Core > App > Web uygulaması** ' nı ( **Web uygulaması (Model-View-Controller)** seçmeyin) seçin ve ardından **İleri**' yi seçin.

* **Hedef çerçevenin** **.NET Core 3,0**olarak ayarlandığından emin olun ve ardından **İleri**' yi seçin.

* Projeyi *Signalrchat*olarak adlandırın ve **Oluştur**' u seçin.

---

## <a name="add-the-opno-locsignalr-client-library"></a>SignalR istemci kitaplığı ekleme

SignalR sunucusu kitaplığı, ASP.NET Core 3,0 paylaşılan çerçevesine dahildir. JavaScript istemci kitaplığı projeye otomatik olarak dahil değildir. Bu öğreticide, istemci kitaplığını *unpkg*'den almak Için kitaplık Yöneticisi 'Ni (Libman) kullanacaksınız. unpkg, Node. js Paket Yöneticisi NPM 'de bulunan her şeyi teslim edebilen bir içerik teslim ağı (CDN)).

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

* **Çözüm Gezgini**, projeye sağ tıklayın ve > **Istemci tarafı kitaplığı** **Ekle** ' yi seçin.

* **Istemci tarafı kitaplığı Ekle** Iletişim kutusunda **sağlayıcı** için **unpkg**seçeneğini belirleyin.

* **Kitaplık**için `@microsoft/signalr@latest`girin.

* **Belirli dosyaları seç**' i seçin, *dağ/Browser* klasörünü genişletin ve *SignalR. js* ve *SignalR. min. js*' yi seçin.

* **Hedef konumu** *Wwwroot/js/SignalR/* olarak ayarlayın ve **yüklemeyi**seçin.

  ![Istemci tarafı kitaplığı Ekle iletişim kutusu-kitaplık Seç](signalr/_static/3.x/find-signalr-client-libs-select-files.png)

  LibMan, bir *Wwwroot/js/SignalR* klasörü oluşturur ve seçilen dosyaları buna kopyalar.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

* Tümleşik terminalde, LibMan 'ı yüklemek için aşağıdaki komutu çalıştırın.

  ```dotnetcli
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* SignalR istemci kitaplığını LibMan kullanarak almak için aşağıdaki komutu çalıştırın. Çıktıyı görmeden önce birkaç saniye beklemeniz gerekebilir.

  ```console
  libman install @microsoft/signalr@latest -p unpkg -d wwwroot/js/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  Parametreler aşağıdaki seçenekleri belirtir:
  * Unpkg sağlayıcısını kullanın.
  * Dosyaları *Wwwroot/js/SignalR* hedefine kopyalayın.
  * Yalnızca belirtilen dosyaları kopyala.

  Çıktı aşağıdaki örneğe benzer şekilde görünür:

  ```console
  wwwroot/js/signalr/dist/browser/signalr.js written to disk
  wwwroot/js/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@microsoft/signalr@latest" to "wwwroot/js/signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

* **Terminalde**, Libman 'ı yüklemek için aşağıdaki komutu çalıştırın.

  ```dotnetcli
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* Proje klasörüne gidin ( *Signalrchat. csproj* dosyasını içeren bir dosya).

* SignalR istemci kitaplığını LibMan kullanarak almak için aşağıdaki komutu çalıştırın.

  ```console
  libman install @microsoft/signalr@latest -p unpkg -d wwwroot/js/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  Parametreler aşağıdaki seçenekleri belirtir:
  * Unpkg sağlayıcısını kullanın.
  * Dosyaları *Wwwroot/js/SignalR* hedefine kopyalayın.
  * Yalnızca belirtilen dosyaları kopyala.

  Çıktı aşağıdaki örneğe benzer şekilde görünür:

  ```console
  wwwroot/js/signalr/dist/browser/signalr.js written to disk
  wwwroot/js/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@microsoft/signalr@latest" to "wwwroot/js/signalr"
  ```

---

## <a name="create-a-opno-locsignalr-hub"></a>SignalR hub 'ı oluşturma

*Hub* , istemci-sunucu iletişimini işleyen yüksek düzeyli bir işlem hattı görevi gören bir sınıftır.

* SignalRChat proje klasöründe bir *hub* klasörü oluşturun.

* *Hub 'lar* klasöründe, aşağıdaki kodla bir *ChatHub.cs* dosyası oluşturun:

  [!code-csharp[ChatHub](signalr/sample-snapshot/3.x/ChatHub.cs)]

  `ChatHub` sınıfı SignalR `Hub` sınıfından devralır. `Hub` sınıfı bağlantıları, grupları ve mesajlaşmayı yönetir.

  `SendMessage` yöntemi, tüm istemcilere ileti göndermek için bağlı bir istemci tarafından çağrılabilir. Yöntemi çağıran JavaScript istemci kodu Öğreticinin ilerleyen kısımlarında gösterilmektedir. SignalR kod, en fazla ölçeklenebilirlik sağlamak için zaman uyumsuzdur.

## <a name="configure-opno-locsignalr"></a>SignalR Yapılandır

SignalR sunucusu, SignalRSignalR istekleri geçirilecek şekilde yapılandırılmalıdır.

* Aşağıdaki Vurgulanan kodu *Startup.cs* dosyasına ekleyin.

  [!code-csharp[Startup](signalr/sample-snapshot/3.x/Startup.cs?highlight=11,28,55)]

  Bu değişiklikler, ASP.NET Core bağımlılığı ekleme ve yönlendirme sistemlerine SignalR ekler.

## <a name="add-opno-locsignalr-client-code"></a>SignalR istemci kodu ekle

* *Pages\ındex.cshtml* içindeki içeriği şu kodla değiştirin:

  [!code-cshtml[Index](signalr/sample-snapshot/3.x/Index.cshtml)]

  Yukarıdaki kod:

  * Ad ve ileti metni ve Gönder düğmesi için metin kutuları oluşturur.
  * SignalR hub 'ından alınan iletileri görüntülemek için `id="messagesList"` içeren bir liste oluşturur.
  * Bir sonraki adımda oluşturduğunuz SignalR ve *sohbet. js* uygulama koduna yönelik betik başvurularını içerir.

* *Wwwroot/js* klasöründe, aşağıdaki kodla bir *chat. js* dosyası oluşturun:

  [!code-javascript[chat](signalr/sample-snapshot/3.x/chat.js)]

  Yukarıdaki kod:

  * Bir bağlantı oluşturur ve başlatır.
  * , Hub 'a ileti gönderen bir işleyiciye Gönder düğmesine ekler.
  * , Hub 'dan iletileri alan ve bunları listeye ekleyen bir işleyici olan bağlantı nesnesine ekler.

## <a name="run-the-app"></a>Uygulamayı çalıştırma

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Uygulamayı hata ayıklamadan çalıştırmak için **CTRL + F5** tuşlarına basın.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Tümleşik terminalde aşağıdaki komutu çalıştırın:

  ```dotnetcli
  dotnet watch run -p SignalRChat.csproj
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

* Menüden, **hata ayıklama olmadan başlat > Çalıştır**' ı seçin.

---

* Adres çubuğundan URL 'yi kopyalayın, başka bir tarayıcı örneği veya sekme açın ve adres çubuğuna URL 'YI yapıştırın.

* Tarayıcı ' yı seçin, bir ad ve ileti girin ve **Ileti gönder** düğmesini seçin.

  Ad ve ileti anında her iki sayfada da görüntülenir.

  ![[! Üs. NO-LOC (SignalR)] örnek uygulama](signalr/_static/3.x/signalr-get-started-finished.png)

> [!TIP]
> * Uygulama işe yaramazsa, tarayıcı geliştirici araçlarınızı (F12) açın ve konsola gidin. HTML ve JavaScript kodunuzla ilgili hatalarla karşılaşabilirsiniz. Örneğin, *SignalR. js* ' yi yönlendirenden farklı bir klasöre yerleştirdiğinizi varsayalım. Bu durumda, bu dosyaya başvuru çalışmaz ve konsolunda 404 hatası görürsünüz.
>   ![SignalR. js bulunamadı hatası](signalr/_static/3.x/f12-console.png)
> * Chrome 'da hata ERR_SPDY_INADEQUATE_TRANSPORT_SECURITY alırsanız, geliştirme sertifikanızı güncelleştirmek için şu komutları çalıştırın:
>
>   ```dotnetcli
>   dotnet dev-certs https --clean
>   dotnet dev-certs https --trust
>   ```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Bu öğreticide SignalRkullanarak gerçek zamanlı bir uygulama oluşturmanın temelleri öğretilir. Aşağıdakilerin nasıl yapıldığını öğreneceksiniz: 

> [!div class="checklist"]  
> * Web projesi oluşturun.   
> * SignalR istemci kitaplığı ekleyin.   
> * SignalR hub 'ı oluşturun. 
> * Projeyi SignalRkullanacak şekilde yapılandırın. 
> * Herhangi bir istemciden tüm bağlı istemcilere ileti gönderen kodu ekleyin.  
Sonunda, çalışan bir sohbet uygulamasına sahipsiniz: ![[! Üs. NO-LOC (SignalR)] örnek uygulama](signalr/_static/2.x/signalr-get-started-finished.png)   

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

* Menüden **dosya > yeni proje**' yi seçin. 

* **Yeni proje** iletişim kutusunda, **yüklü > Visual C# > Web > ASP.NET Core Web uygulaması**' nı seçin. Projeyi *Signalrchat*olarak adlandırın. 

  ![Visual Studio 'da yeni proje iletişim kutusu](signalr/_static/2.x/signalr-new-project-dialog.png)    

* Razor Pages kullanan bir proje oluşturmak için **Web uygulaması** ' nı seçin. 

* **.NET Core**'un hedef çerçevesini seçin, **ASP.NET Core 2,2**' i seçin ve **Tamam**' ı tıklatın.    

  ![Visual Studio 'da yeni proje iletişim kutusu](signalr/_static/2.x/signalr-new-project-choose-type.png)   

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)    

* [Tümleşik Terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) ' i yeni proje klasörünün oluşturulacağı klasöre açın.  

* Aşağıdaki komutları çalıştırın:   

   ```dotnetcli 
   dotnet new webapp -o SignalRChat 
   code -r SignalRChat  
   ```  

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)   

* Menüden **dosya > yeni çözüm**' i seçin.    

* **.NET Core > App > ASP.NET Core Web uygulaması** ' nı ( **ASP.NET Core Web uygulaması (MVC)** seçmeyin) seçin.  

* **İleri**’yi seçin.  

* Projeyi *Signalrchat*olarak adlandırın ve **Oluştur**' u seçin.   

--- 

## <a name="add-the-opno-locsignalr-client-library"></a>SignalR istemci kitaplığı ekleme 

SignalR sunucusu kitaplığı, `Microsoft.AspNetCore.App` metapackage 'e dahildir. JavaScript istemci kitaplığı projeye otomatik olarak dahil değildir. Bu öğreticide, istemci kitaplığını *unpkg*'den almak Için kitaplık Yöneticisi 'Ni (Libman) kullanacaksınız. unpkg, Node. js Paket Yöneticisi NPM 'de bulunan her şeyi teslim edebilen bir içerik teslim ağı (CDN)).  

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)  

* **Çözüm Gezgini**, projeye sağ tıklayın ve > **Istemci tarafı kitaplığı** **Ekle** ' yi seçin.  

* **Istemci tarafı kitaplığı Ekle** Iletişim kutusunda **sağlayıcı** için **unpkg**seçeneğini belirleyin. 

* **Kitaplık**için `@microsoft/signalr@3`girin ve Önizleme olmayan en son sürümü seçin.  

  ![Istemci tarafı kitaplığı Ekle iletişim kutusu-kitaplık Seç](signalr/_static/2.x/libman1.png)   

* **Belirli dosyaları seç**' i seçin, *dağ/Browser* klasörünü genişletin ve *SignalR. js* ve *SignalR. min. js*' yi seçin. 

* **Hedef konumu** *Wwwroot/lib/SignalR/* olarak ayarlayın ve **yüklemeyi**seçin.    

  ![Istemci tarafı kitaplığı Ekle iletişim kutusu-dosya ve hedef seçme](signalr/_static/2.x/libman2.png) 

  LibMan, bir *Wwwroot/LIB/SignalR* klasörü oluşturur ve seçilen dosyaları buna kopyalar.    

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)    

* Tümleşik terminalde, LibMan 'ı yüklemek için aşağıdaki komutu çalıştırın.  

  ```dotnetcli  
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli   
  ```   

* SignalR istemci kitaplığını LibMan kullanarak almak için aşağıdaki komutu çalıştırın. Çıktıyı görmeden önce birkaç saniye beklemeniz gerekebilir. 

  ```console    
  libman install @microsoft/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js 
  ```   

  Parametreler aşağıdaki seçenekleri belirtir: 
  * Unpkg sağlayıcısını kullanın. 
  * Dosyaları *Wwwroot/lib/SignalR* hedefine kopyalayın.    
  * Yalnızca belirtilen dosyaları kopyala.  

  Çıktı aşağıdaki örneğe benzer şekilde görünür:  

  ```console    
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk   
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk   
  Installed library "@microsoft/signalr@3.0.1" to "wwwroot/lib/signalr" 
  ```   

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)   

* **Terminalde**, Libman 'ı yüklemek için aşağıdaki komutu çalıştırın. 

  ```dotnetcli  
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli   
  ```   

* Proje klasörüne gidin ( *Signalrchat. csproj* dosyasını içeren bir dosya). 

* SignalR istemci kitaplığını LibMan kullanarak almak için aşağıdaki komutu çalıştırın.    

  ```console    
  libman install @microsoft/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js 
  ```   

  Parametreler aşağıdaki seçenekleri belirtir: 
  * Unpkg sağlayıcısını kullanın. 
  * Dosyaları *Wwwroot/lib/SignalR* hedefine kopyalayın.    
  * Yalnızca belirtilen dosyaları kopyala.  

  Çıktı aşağıdaki örneğe benzer şekilde görünür:  

  ```console    
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk   
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk   
  Installed library "@microsoft/signalr@3.x.x" to "wwwroot/lib/signalr" 
  ```   

--- 

## <a name="create-a-opno-locsignalr-hub"></a>SignalR hub 'ı oluşturma   

*Hub* , istemci-sunucu iletişimini işleyen yüksek düzeyli bir işlem hattı görevi gören bir sınıftır.   

* SignalRChat proje klasöründe bir *hub* klasörü oluşturun.    

* *Hub 'lar* klasöründe, aşağıdaki kodla bir *ChatHub.cs* dosyası oluşturun: 

  [!code-csharp[Startup](signalr/sample-snapshot/2.x/ChatHub.cs)]   

  `ChatHub` sınıfı SignalR `Hub` sınıfından devralır. `Hub` sınıfı bağlantıları, grupları ve mesajlaşmayı yönetir.  

  `SendMessage` yöntemi, tüm istemcilere ileti göndermek için bağlı bir istemci tarafından çağrılabilir. Yöntemi çağıran JavaScript istemci kodu Öğreticinin ilerleyen kısımlarında gösterilmektedir. SignalR kod, en fazla ölçeklenebilirlik sağlamak için zaman uyumsuzdur.    

## <a name="configure-opno-locsignalr"></a>SignalR Yapılandır  

SignalR sunucusu, SignalRSignalR istekleri geçirilecek şekilde yapılandırılmalıdır.    

* Aşağıdaki Vurgulanan kodu *Startup.cs* dosyasına ekleyin.  

  [!code-csharp[Startup](signalr/sample-snapshot/2.x/Startup.cs?highlight=7,33,52-55)]  

  Bu değişiklikler, ASP.NET Core bağımlılık ekleme sistemine ve ara yazılım ardışık düzenine SignalR ekler.  

## <a name="add-opno-locsignalr-client-code"></a>SignalR istemci kodu ekle    

* *Pages\ındex.cshtml* içindeki içeriği şu kodla değiştirin:  

  [!code-cshtml[Index](signalr/sample-snapshot/2.x/Index.cshtml)]   

  Yukarıdaki kod:   

  * Ad ve ileti metni ve Gönder düğmesi için metin kutuları oluşturur.  
  * SignalR hub 'ından alınan iletileri görüntülemek için `id="messagesList"` içeren bir liste oluşturur.   
  * Bir sonraki adımda oluşturduğunuz SignalR ve *sohbet. js* uygulama koduna yönelik betik başvurularını içerir.    

* *Wwwroot/js* klasöründe, aşağıdaki kodla bir *chat. js* dosyası oluşturun:  

  [!code-javascript[Index](signalr/sample-snapshot/2.x/chat.js)]    

  Yukarıdaki kod:   

  * Bir bağlantı oluşturur ve başlatır.    
  * , Hub 'a ileti gönderen bir işleyiciye Gönder düğmesine ekler. 
  * , Hub 'dan iletileri alan ve bunları listeye ekleyen bir işleyici olan bağlantı nesnesine ekler.  

## <a name="run-the-app"></a>Uygulamayı çalıştırma  

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)   

* Uygulamayı hata ayıklamadan çalıştırmak için **CTRL + F5** tuşlarına basın.   

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code) 

* Tümleşik terminalde aşağıdaki komutu çalıştırın:    

  ```dotnetcli
  dotnet run -p SignalRChat.csproj
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

* Menüden, **hata ayıklama olmadan başlat > Çalıştır**' ı seçin.

---

* Adres çubuğundan URL 'yi kopyalayın, başka bir tarayıcı örneği veya sekme açın ve adres çubuğuna URL 'YI yapıştırın.

* Tarayıcı ' yı seçin, bir ad ve ileti girin ve **Ileti gönder** düğmesini seçin.  

  Ad ve ileti anında her iki sayfada da görüntülenir.   

  ![[! Üs. NO-LOC (SignalR)] örnek uygulama](signalr/_static/2.x/signalr-get-started-finished.png) 

> [!TIP]    
> Uygulama işe yaramazsa, tarayıcı geliştirici araçlarınızı (F12) açın ve konsola gidin. HTML ve JavaScript kodunuzla ilgili hatalarla karşılaşabilirsiniz. Örneğin, *SignalR. js* ' yi yönlendirenden farklı bir klasöre yerleştirdiğinizi varsayalım. Bu durumda, bu dosyaya başvuru çalışmaz ve konsolunda 404 hatası görürsünüz.   
> ![SignalR. js bulunamadı hatası](signalr/_static/2.x/f12-console.png)    
## <a name="additional-resources"></a>Ek kaynaklar 
* [Bu öğreticinin YouTube sürümü](https://www.youtube.com/watch?v=iKlVmu-r0JQ)   

::: moniker-end
