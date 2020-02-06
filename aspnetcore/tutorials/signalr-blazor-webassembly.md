---
title: Blazor WebAssembly ile ASP.NET Core SignalR kullanma
author: guardrex
description: Blazor WebAssembly ile ASP.NET Core SignalR kullanan bir sohbet uygulaması oluşturun.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/31/2020
no-loc:
- Blazor
- SignalR
uid: tutorials/signalr-blazor-webassembly
ms.openlocfilehash: d3605c0823e9ec3ce34fb781da66a7470aa00622
ms.sourcegitcommit: 0e21d4f8111743bcb205a2ae0f8e57910c3e8c25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77034180"
---
# <a name="use-aspnet-core-signalr-with-blazor-webassembly"></a>Blazor WebAssembly ile ASP.NET Core SignalR kullanma

[Daniel Roth](https://github.com/danroth27) ve [Luke Latham](https://github.com/guardrex) tarafından

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Bu öğreticide, Blazor WebAssembly ile SignalR kullanarak gerçek zamanlı bir uygulama oluşturmanın temelleri öğretilir. Aşağıdakilerin nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Blazor WebAssembly barındırılan uygulama projesi oluşturma
> * SignalR istemci kitaplığını ekleme
> * SignalR hub 'ı ekleme
> * SignalR hub 'ı için SignalR Hizmetleri ve uç nokta ekleme
> * Sohbet için Razor bileşeni kodu ekleme

Bu öğreticinin sonunda, çalışan bir sohbet uygulamanız olacaktır.

[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/signalr-blazor-webassembly/samples/) ([nasıl indirileceği](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Prerequisites

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.1.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.1.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.1.md)]

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli/)

[!INCLUDE[](~/includes/3.1-SDK.md)]

---

## <a name="create-a-hosted-blazor-webassembly-app-project"></a>Barındırılan Blazor WebAssembly uygulama projesi oluşturma

[Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) şablonunu yükler. [Microsoft. AspNetCore. Blazor. Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) paketinin önizleme sürümü vardır, ancak Blazor WebAssembly önizlemededir. Bir komut kabuğunda, aşağıdaki komutu yürütün:

```dotnetcli
dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.2.0-preview1.20073.1
```

Araç seçiminiz için yönergeleri izleyin:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Yeni bir proje oluşturun.

1. **Blazor uygulamasını** seçin ve **İleri ' yi**seçin.

1. **Proje adı** alanına "BlazorSignalRApp" yazın. **Konum** girişinin doğru olduğunu onaylayın veya proje için bir konum belirtin. **Oluştur**’u seçin.

1. **Blazor WebAssembly uygulama** şablonunu seçin.

1. **Gelişmiş**' in altında, **ASP.NET Core barındırılan** onay kutusunu seçin.

1. **Oluştur**’u seçin.

> [!NOTE]
> Visual Studio 'nun yeni bir sürümünü yükselttiyseniz veya yüklediyseniz ve Blazor WebAssembly şablonu VS Kullanıcı arabiriminde görünmüyorsa, daha önce gösterilen `dotnet new` komutunu kullanarak şablonu yeniden yükleyin.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

1. Bir komut kabuğunda, aşağıdaki komutu yürütün:

   ```dotnetcli
   dotnet new blazorwasm --hosted --output BlazorSignalRApp
   ```

1. Visual Studio Code, uygulamanın proje klasörünü açın.

1. Uygulamayı derlemek ve hata ayıklamak için varlık Ekle iletişim kutusu göründüğünde **Evet**' i seçin. Visual Studio Code, *. vscode* klasörünü oluşturulan *Launch. JSON* ve *Tasks. JSON* dosyaları ile otomatik olarak ekler.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

1. Bir komut kabuğunda, aşağıdaki komutu yürütün:

   ```dotnetcli
   dotnet new blazorwasm --hosted --output BlazorSignalRApp
   ```

1. Mac için Visual Studio, proje klasörüne gidip projenin çözüm dosyasını ( *. sln*) açarak projeyi açın.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli/)

Bir komut kabuğunda, aşağıdaki komutu yürütün:

```dotnetcli
dotnet new blazorwasm --hosted --output BlazorSignalRApp
```

---

## <a name="add-the-signalr-client-library"></a>SignalR istemci kitaplığını ekleme

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

1. **Çözüm Gezgini**, **BlazorSignalRApp. Client** projesine sağ tıklayın ve **NuGet Paketlerini Yönet**' i seçin.

1. **NuGet Paketlerini Yönet** iletişim kutusunda, **paket kaynağının** *NuGet.org*olarak ayarlandığını doğrulayın.

1. Araştır **seçiliyken,** arama kutusuna "Microsoft. aspnetcore. SignalR. Client" yazın.

1. Arama sonuçlarında `Microsoft.AspNetCore.SignalR.Client` paketini seçin ve ardından **Install**' ı seçin.

1. **Değişiklikleri Önizle** iletişim kutusu görüntülenirse **Tamam**' ı seçin.

1. **Lisans kabulü** iletişim kutusu görüntülenirse, lisans şartlarını kabul ediyorsanız **kabul ediyorum** ' u seçin.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

**Tümleşik terminalde** (araç çubuğundan > **terminalini** **görüntüleyin** ), aşağıdaki komutları yürütün:

```dotnetcli
dotnet add Client package Microsoft.AspNetCore.SignalR.Client
```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

1. **Çözüm** kenar çubuğunda **BlazorSignalRApp. Client** projesine sağ tıklayın ve **NuGet Paketlerini Yönet**' i seçin.

1. **NuGet Paketlerini Yönet** iletişim kutusunda, kaynak açılan kutusunun *NuGet.org*olarak ayarlandığını doğrulayın.

1. Araştır **seçiliyken,** arama kutusuna "Microsoft. aspnetcore. SignalR. Client" yazın.

1. Arama sonuçlarında `Microsoft.AspNetCore.SignalR.Client` paketinin yanındaki onay kutusunu işaretleyin ve **paket Ekle**' yi seçin.

1. **Lisans kabulü** iletişim kutusu görüntülenirse, lisans şartlarını kabul ediyorsanız **kabul et** ' i seçin.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli/)

Komut kabuğu 'nda aşağıdaki komutları yürütün:

```dotnetcli
cd BlazorSignalRApp
dotnet add Client package Microsoft.AspNetCore.SignalR.Client
```

---

## <a name="add-a-signalr-hub"></a>SignalR hub 'ı ekleme

**BlazorSignalRApp. Server** projesinde, bir *hub* (plural) klasörü oluşturun ve aşağıdaki `ChatHub` sınıfını (*hub/ChatHub. cs*) ekleyin:

[!code-csharp[](signalr-blazor-webassembly/samples/3.x/BlazorSignalRApp/Server/Hubs/ChatHub.cs)]

## <a name="add-signalr-services-and-an-endpoint-for-the-signalr-hub"></a>SignalR hub 'ı için SignalR Hizmetleri ve uç nokta ekleme

1. **BlazorSignalRApp. Server** projesinde, *Startup.cs* dosyasını açın.

1. `ChatHub` sınıfı için ad alanını dosyanın en üstüne ekleyin:

   ```csharp
   using BlazorSignalRApp.Server.Hubs;
   ```

1. SignalR hizmetlerini `Startup.ConfigureServices`ekleyin:

   ```csharp
   services.AddSignalR();
   ```

1. Varsayılan denetleyici yolu ve istemci tarafı geri dönüş uç noktaları arasında `Startup.Configure`, Hub için bir uç nokta ekleyin:

   [!code-csharp[](signalr-blazor-webassembly/samples/3.x/BlazorSignalRApp/Server/Startup.cs?name=snippet&highlight=4)]

## <a name="add-razor-component-code-for-chat"></a>Sohbet için Razor bileşeni kodu ekleme

1. **BlazorSignalRApp. Client** projesinde *Pages/Index. Razor* dosyasını açın.

1. İşaretlemeyi aşağıdaki kodla değiştirin:

[!code-razor[](signalr-blazor-webassembly/samples/3.x/BlazorSignalRApp/Client/Pages/Index.razor)]

## <a name="run-the-app"></a>Uygulamayı çalıştırma

1. Araç kılavuzunuz için yönergeleri izleyin:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. **Çözüm Gezgini**, **BlazorSignalRApp. Server** projesini seçin. Uygulamayı hata ayıklamadan çalıştırmak için **CTRL + F5** tuşlarına basın.

1. Adres çubuğundan URL 'yi kopyalayın, başka bir tarayıcı örneği veya sekme açın ve adres çubuğuna URL 'YI yapıştırın.

1. Tarayıcı ' yı seçin, bir ad ve ileti girin ve **Gönder** düğmesini seçin. Ad ve ileti her iki sayfada da anında görüntülenir:

   ![SignalR Blazor WebAssembly örnek uygulaması, değiştirilen iletileri gösteren iki tarayıcı penceresinde açılır.](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)

   Tırnak: *yıldız Trek VI: bulunan ülke* &copy;1991 [Paramount](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

1. Araç çubuğundan hata **ayıkla** > **Çalıştır** ' ı seçin.

1. Adres çubuğundan URL 'yi kopyalayın, başka bir tarayıcı örneği veya sekme açın ve adres çubuğuna URL 'YI yapıştırın.

1. Tarayıcı ' yı seçin, bir ad ve ileti girin ve **Gönder** düğmesini seçin. Ad ve ileti her iki sayfada da anında görüntülenir:

   ![SignalR Blazor WebAssembly örnek uygulaması, değiştirilen iletileri gösteren iki tarayıcı penceresinde açılır.](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)

   Tırnak: *yıldız Trek VI: bulunan ülke* &copy;1991 [Paramount](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

1. **Çözüm** kenar çubuğunda **BlazorSignalRApp. Server** projesini seçin. Menüden, **hata ayıklama olmadan başlat** > **Çalıştır** ' ı seçin.

1. Adres çubuğundan URL 'yi kopyalayın, başka bir tarayıcı örneği veya sekme açın ve adres çubuğuna URL 'YI yapıştırın.

1. Tarayıcı ' yı seçin, bir ad ve ileti girin ve **Gönder** düğmesini seçin. Ad ve ileti her iki sayfada da anında görüntülenir:

   ![SignalR Blazor WebAssembly örnek uygulaması, değiştirilen iletileri gösteren iki tarayıcı penceresinde açılır.](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)

   Tırnak: *yıldız Trek VI: bulunan ülke* &copy;1991 [Paramount](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli/)

1. Komut kabuğu 'nda aşağıdaki komutları yürütün:

   ```dotnetcli
   cd Server
   dotnet run
   ```

1. Adres çubuğundan URL 'yi kopyalayın, başka bir tarayıcı örneği veya sekme açın ve adres çubuğuna URL 'YI yapıştırın.

1. Tarayıcı ' yı seçin, bir ad ve ileti girin ve **Gönder** düğmesini seçin. Ad ve ileti her iki sayfada da anında görüntülenir:

   ![SignalR Blazor WebAssembly örnek uygulaması, değiştirilen iletileri gösteren iki tarayıcı penceresinde açılır.](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)

   Tırnak: *yıldız Trek VI: bulunan ülke* &copy;1991 [Paramount](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)

---

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Blazor Weelsembly barındırılan uygulama projesi oluşturma
> * SignalR istemci kitaplığı ekleme
> * SignalR hub 'ı ekleme
> * SignalR hub 'ı için SignalR Hizmetleri ve uç nokta ekleme
> * Sohbet için Razor bileşeni kodu ekleme

Blazor uygulamaları oluşturma hakkında daha fazla bilgi için Blazor belgelerine bakın:

> [!div class="nextstepaction"]
> <xref:blazor/index>

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:signalr/introduction>
