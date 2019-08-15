---
title: ASP.NET Core .NET Core gRPC istemcisi ve sunucusu oluşturma
author: juntaoluo
description: Bu öğreticide, ASP.NET Core bir gRPC hizmeti ve gRPC istemcisinin nasıl oluşturulacağı gösterilmektedir. GRPC hizmeti projesi oluşturmayı, Proto dosyasını düzenlemeyi ve çift yönlü akış araması eklemeyi öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 08/07/2019
uid: tutorials/grpc/grpc-start
ms.openlocfilehash: 496f659bd51e2404a936bea8aad77e674e1a285d
ms.sourcegitcommit: 476ea5ad86a680b7b017c6f32098acd3414c0f6c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/14/2019
ms.locfileid: "69022489"
---
# <a name="tutorial-create-a-grpc-client-and-server-in-aspnet-core"></a>Öğretici: ASP.NET Core bir gRPC istemcisi ve sunucusu oluşturma

[John Luo](https://github.com/juntaoluo) tarafından

Bu öğreticide, bir .NET Core [GRPC](https://grpc.io/docs/guides/) istemcisinin ve bir ASP.NET Core GRPC sunucusunun nasıl oluşturulacağı gösterilmektedir.

Sonda, gRPC Greeter hizmeti ile iletişim kuran bir gRPC istemcisine sahip olacaksınız.

[Örnek kodu görüntüle veya indir](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([indirme](xref:index#how-to-download-a-sample)).

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * GRPC sunucusu oluşturun.
> * GRPC istemcisi oluşturun.
> * GRPC istemci hizmetini gRPC Greeter hizmeti ile test edin.

## <a name="prerequisites"></a>Önkoşullar

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-grpc-service"></a>GRPC hizmeti oluşturma

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Visual Studio'dan **dosya** menüsünde **yeni** > **proje**.
* **Yeni proje oluştur** iletişim kutusunda **ASP.NET Core Web uygulaması**' nı seçin.
* **İleri** Seç
* Projeyi **Grpcgreeter**olarak adlandırın. Kodu kopyalayıp yapıştırdığınızda ad alanlarının eşleşmesi için, proje *Grpcgreeter* adında bir ad vermek önemlidir.
* **Oluştur**’u seçin
* **Yeni ASP.NET Core Web uygulaması oluştur** iletişim kutusunda:
  * Açılan menülerde **.NET Core** ve **ASP.NET Core 3,0** ' i seçin. 
  * **GRPC hizmeti** şablonunu seçin.
  * **Oluştur**’u seçin

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Açık [tümleşik Terminalini](https://code.visualstudio.com/docs/editor/integrated-terminal).
* Dizinleri (`cd`), projeyi içerecek bir klasöre değiştirin.
* Aşağıdaki komutları çalıştırın:

  ```console
  dotnet new grpc -o GrpcGreeter
  code -r GrpcGreeter
  ```

  * Komut grpcgreeter klasöründe yeni bir GRPC hizmeti oluşturur. `dotnet new`
  * Komut, Visual Studio Code yeni bir örneğinde *grpcgreeter* klasörünü açar. `code`

  **Gerekli varlıkların derlenmesi ve hata ayıklaması için ' grpcgreeter ' içinde eksik bir iletişim kutusu görüntülenir. Bunları ekleyin mi?**
* **Evet** ' i seçin

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

Terminalden aşağıdaki komutları çalıştırın:

```console
  dotnet new grpc -o GrpcGreeter
  cd GrpcGreeter
```

Yukarıdaki komutlar, gRPC hizmeti oluşturmak için [.NET Core CLI](/dotnet/core/tools/dotnet) kullanır.

### <a name="open-the-project"></a>Projeyi açın

Visual Studio 'da **dosya > aç**' ı seçin ve ardından *grpcgreeter. sln* dosyasını seçin.

<!-- End of VS tabs -->

---

### <a name="run-the-service"></a>Hizmeti çalıştırın

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Hata `Ctrl+F5` ayıklayıcı olmadan GRPC hizmetini çalıştırmak için tuşuna basın.

  Visual Studio, hizmeti bir komut isteminde çalıştırır.

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Mac için Visual Studio](#tab/visual-studio-code+visual-studio-mac)

* İle`dotnet run`komut satırından GRPC Greeter projesi *grpcgreeter* öğesini çalıştırın.

<!-- End of combined VS/Mac tabs -->

---

Günlükler hizmeti dinlediği `https://localhost:5001`hizmetini gösterir.

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: https://localhost:5001
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
```

> [!NOTE]
> GRPC şablonu, [Aktarım Katmanı Güvenliği (TLS)](https://tools.ietf.org/html/rfc5246)kullanmak üzere yapılandırılmıştır. gRPC istemcilerinin, sunucuyu çağırmak için HTTPS kullanması gerekir.
>
> macOS, TLS ile ASP.NET Core gRPC 'yi desteklemez. MacOS 'ta gRPC hizmetlerini başarıyla çalıştırmak için ek yapılandırma gerekir. Daha fazla bilgi için bkz. [macOS üzerinde gRPC uygulaması ASP.NET Core başlatılamıyor](xref:grpc/troubleshoot#unable-to-start-aspnet-core-grpc-app-on-macos).

### <a name="examine-the-project-files"></a>Proje dosyalarını inceleyin

*Grpcgreeter* proje dosyaları:

* *Greet. proto*: *Prototips/Greet. proto* dosyası `Greeter` GRPC 'yi tanımlar ve GRPC sunucu varlıklarını oluşturmak için kullanılır. Daha fazla bilgi için bkz. [gRPC 'ye giriş](xref:grpc/index).
* *Hizmetler* klasörü: `Greeter` Hizmetin uygulamasını içerir.
* *appSettings. JSON*: Kestrel tarafından kullanılan protokol gibi yapılandırma verilerini içerir. Daha fazla bilgi için bkz. <xref:fundamentals/configuration/index>.
* *Program.cs*: GRPC hizmeti için giriş noktasını içerir. Daha fazla bilgi için bkz. <xref:fundamentals/host/generic-host>.
* *Startup.cs*: Uygulama davranışını yapılandıran kodu içerir. Daha fazla bilgi için bkz. [uygulama başlatma](xref:fundamentals/startup).

## <a name="create-the-grpc-client-in-a-net-console-app"></a>Bir .NET konsol uygulamasında gRPC istemcisini oluşturma

## <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Visual Studio 'nun ikinci bir örneğini açın.
* Menü çubuğundan **Dosya** > **Yeni** > **Proje** ' yi seçin.
* **Yeni proje oluştur** Iletişim kutusunda **konsol uygulaması (.NET Core)** seçeneğini belirleyin.
* **İleri** Seç
* **Ad** metin kutusuna "GrpcGreeterClient" yazın.
* **Oluştur**’u seçin.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Açık [tümleşik Terminalini](https://code.visualstudio.com/docs/editor/integrated-terminal).
* Dizinleri (`cd`), projeyi içerecek bir klasöre değiştirin.
* Aşağıdaki komutları çalıştırın:

```console
dotnet new console -o GrpcGreeterClient
code -r GrpcGreeterClient
```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

*Grpcgreeterclient*adlı bir konsol uygulaması oluşturmak için [buradaki](/dotnet/core/tutorials/using-on-mac-vs-full-solution) yönergeleri izleyin.

<!-- End of VS tabs -->

---

### <a name="add-required-packages"></a>Gerekli paketleri Ekle

GRPC istemci projesi aşağıdaki paketleri gerektirir:

* .NET Core istemcisini içeren [GRPC .net. Client](https://www.nuget.org/packages/Grpc.Net.Client).
* İçin C#prototipsiz Ileti API 'Leri içeren [Google. protoarabellek](https://www.nuget.org/packages/Google.Protobuf/).
* Prototipleme dosyaları için araç desteğini C# Içeren [GRPC. Tools](https://www.nuget.org/packages/Grpc.Tools/). Araç çalışma zamanında gerekli değildir, bu nedenle bağımlılık ile `PrivateAssets="All"`işaretlenir.

### <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Paket Yöneticisi Konsolu (PMC) veya NuGet Paketlerini Yönet ' i kullanarak paketleri yükler.

#### <a name="pmc-option-to-install-packages"></a>Paket yüklemek için PMC seçeneği

* Visual Studio 'da **Araçlar** > **NuGet Paket Yöneticisi** > **Paket Yöneticisi konsolu** ' nu seçin.
* **Paket Yöneticisi konsolu** penceresinde, *Grpcgreeterclient. csproj* dosyasının bulunduğu dizine gidin.
* Aşağıdaki komutları çalıştırın:

 ```powershell
Install-Package Grpc.Net.Client
Install-Package Google.Protobuf
Install-Package Grpc.Tools
```

#### <a name="manage-nuget-packages-option-to-install-packages"></a>Paket yüklemek için NuGet Paketlerini Yönet seçeneği

* **Çözüm Gezgini** > **NuGet Paketlerini Yönet** ' de projeye sağ tıklayın
* **Tarayıcı** sekmesini seçin.
* Arama kutusuna **GRPC .net. Client** girin.
* **Araştır** sekmesinden **GRPC .net. Client** paketini seçin ve ardından **Install**' ı seçin.
* `Google.Protobuf` Ve`Grpc.Tools`için tekrarlayın.

### <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

**Tümleşik terminalden**aşağıdaki komutları çalıştırın:

```console
dotnet add GrpcGreeterClient.csproj package Grpc.Net.Client
dotnet add GrpcGreeterClient.csproj package Google.Protobuf
dotnet add GrpcGreeterClient.csproj package Grpc.Tools
```

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

* **Çözüm bölmesi** paket > **Ekle** ' de paketler klasörüne sağ tıklayın
* Arama kutusuna **GRPC .net. Client** girin.
* Sonuçlar bölmesinden **GRPC .net. Client** paketini seçin ve **paket Ekle** ' yi seçin.
* `Google.Protobuf` Ve`Grpc.Tools`için tekrarlayın.

---

### <a name="add-greetproto"></a>Greet. proto Ekle

* GRPC istemci projesinde bir **Prototips** klasörü oluşturun.
* **Protos\bilgisem. proto** dosyasını GRPC Greeter hizmetinden GRPC istemci projesine kopyalayın.
* *Grpcgreeterclient. csproj* proje dosyasını düzenleyin:

  # <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

  Projeye sağ tıklayın ve **Proje dosyasını Düzenle**' yi seçin.

  # <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code) 

  *Grpcgreeterclient. csproj* dosyasını seçin.

  # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

  Projeye sağ tıklayın ve **dosyayı düzenle > araçlar**' ı seçin.

  ---

* **Greet. proto** dosyasına başvuran `<Protobuf>` bir öğesi olan bir öğe grubu ekleyin:

  ```XML
  <ItemGroup>
    <Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
  </ItemGroup>
  ```

### <a name="create-the-greeter-client"></a>Greeter istemcisini oluşturma

`GrpcGreeter` Ad alanındaki türleri oluşturmak için projeyi derleyin. `GrpcGreeter` Türler yapı işlemi tarafından otomatik olarak oluşturulur.

GRPC Client *program.cs* dosyasını aşağıdaki kodla güncelleştirin:

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet2)]

*Program.cs* , GRPC istemcisinin giriş noktasını ve mantığını içerir.

Greeter istemcisi şu şekilde oluşturulur:

* GRPC `HttpClient` hizmetine bağlantı oluşturmak için bilgileri içeren bir örneği oluşturma.
* Greeter istemcisini oluşturmak için kullanarak: `HttpClient`

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=3-6)]

Greeter istemcisi zaman uyumsuz `SayHello` yöntemi çağırır. `SayHello` Çağrının sonucu görüntülenir:

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=7-9)]

## <a name="test-the-grpc-client-with-the-grpc-greeter-service"></a>GRPC istemcisini gRPC Greeter hizmeti ile test etme

### <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Greeter hizmetinde, hata ayıklayıcı olmadan `Ctrl+F5` sunucuyu başlatmak için tuşuna basın.
* Projede, hata ayıklayıcı olmadan istemcisini başlatmak için tuşuna basın `Ctrl+F5`. `GrpcGreeterClient`

### <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Mac için Visual Studio](#tab/visual-studio-code+visual-studio-mac)

* Greeter hizmetini başlatın.
* İstemcisini başlatın.

<!-- End of combined VS/Mac tabs -->

---

İstemci, "GreeterClient" adını içeren bir iletiyle hizmete bir tebrik gönderir. Hizmet, "Hello GreeterClient" iletisini yanıt olarak gönderir. Komut isteminde "Hello GreeterClient" yanıtı görüntülenir:

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

GRPC hizmeti, komut istemine yazılan günlüklerde başarılı çağrının ayrıntılarını kaydeder.

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: https://localhost:5001
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
      Content root path: C:\GH\aspnet\docs\4\Docs\aspnetcore\tutorials\grpc\grpc-start\sample\GrpcGreeter
info: Microsoft.AspNetCore.Hosting.Diagnostics[1]
      Request starting HTTP/2 POST https://localhost:5001/Greet.Greeter/SayHello application/grpc
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[0]
      Executing endpoint 'gRPC - /Greet.Greeter/SayHello'
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[1]
      Executed endpoint 'gRPC - /Greet.Greeter/SayHello'
info: Microsoft.AspNetCore.Hosting.Diagnostics[2]
      Request finished in 78.32260000000001ms 200 application/grpc
```

### <a name="next-steps"></a>Sonraki adımlar

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
