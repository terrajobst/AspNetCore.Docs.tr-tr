---
title: ASP.NET Core .NET Core gRPC istemci ve sunucu oluşturma
author: juntaoluo
description: Bu öğreticide, ASP.NET Core, hizmet ve gRPC gRPC istemci oluşturma işlemi gösterilmektedir. GRPC hizmeti projesi oluşturmak, proto dosyasını düzenleyin ve bir çift yönlü çağrı akış ekleme hakkında bilgi edinin.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 5/30/2019
uid: tutorials/grpc/grpc-start
ms.openlocfilehash: 2b4325d2413e335a3061a7695def88a1b23ee52b
ms.sourcegitcommit: 4d05e30567279072f1b070618afe58ae1bcefd5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66376366"
---
# <a name="tutorial-create-a-grpc-client-and-server-in-aspnet-core"></a>Öğretici: ASP.NET Core gRPC istemci ve sunucu oluşturma

Tarafından [John Luo](https://github.com/juntaoluo)

Bu öğreticide, .NET Core oluşturma işlemi gösterilmektedir [gRPC](https://grpc.io/docs/guides/) istemci ve sunucu bir ASP.NET Core gRPC.

Sonunda, gRPC Greeter hizmet ile iletişim kuran gRPC istemci gerekir.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample)).

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * GRPC bir sunucu oluşturun.
> * GRPC istemci oluşturun.
> * GRPC Greeter hizmeti ile gRPC İstemci hizmetini test edin.

[!INCLUDE[](~/includes/net-core-prereqs-all-3.0.md)]

## <a name="create-a-grpc-service"></a>GRPC hizmeti oluşturma

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Visual Studio'dan **dosya** menüsünde **yeni** > **proje**.
* İçinde **yeni bir proje oluşturma** iletişim kutusunda **ASP.NET Core Web uygulaması**.
* Seçin **İleri**
* Projeyi adlandırın **GrpcGreeter**. Projeyi adlandırın önemlidir *GrpcGreeter* kodu kopyalayıp, ad alanlarını eşleşecek şekilde.
* **Oluştur**’u seçin
* İçinde **yeni bir ASP.NET Core Web uygulaması oluşturma** iletişim:
  * Seçin **.NET Core** ve **ASP.NET Core 3.0** açılan menüler içinde. 
  * Seçin **gRPC hizmet** şablonu.
  * **Oluştur**’u seçin

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Açık [tümleşik Terminalini](https://code.visualstudio.com/docs/editor/integrated-terminal).
* Dizinleri (`cd`) proje içeren bir klasör.
* Aşağıdaki komutları çalıştırın:

  ```console
  dotnet new grpc -o GrpcGreeter
  code -r GrpcGreeter
  ```

  * `dotnet new` Komut yeni bir gRPC hizmetinde oluşturur *GrpcGreeter* klasör.
  * `code` Komutu açılır *GrpcGreeter* Visual Studio Code yeni bir örneğini klasöründe.

  Bir iletişim kutusu görünür **gerekli varlıkları oluşturun ve hata ayıklama 'GrpcGreeter' eksik. Bunları eklensin mi?**
* Seçin **Evet**

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

Bir terminalde aşağıdaki komutları çalıştırın:

```console
  dotnet new grpc -o GrpcGreeter
  cd GrpcGreeter
```

Kullanım komutları önceki [.NET Core CLI](/dotnet/core/tools/dotnet) gRPC hizmet oluşturmak için.

### <a name="open-the-project"></a>Projeyi açın

Visual Studio'dan seçin **Dosya > Aç**ve ardından *GrpcGreeter.sln* dosya.

<!-- End of VS tabs -->

---

### <a name="run-the-service"></a>Hizmet çalıştırma

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Hata Ayıklayıcı olmadan gRPC hizmeti çalıştırmak için CTRL + F5 tuşlarına basın.

  Visual Studio hizmeti, bir komut istemi'nde çalışır.

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code'u / Visual Studio Mac için](#tab/visual-studio-code+visual-studio-mac)

* GRPC Greeter projeyi GrpcGreeter kullanılarak komut satırından çalıştırmak `dotnet run`.

<!-- End of combined VS/Mac tabs -->

---

Üzerinde dinleme hizmeti günlükleri göster `http://localhost:50051`.

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: http://localhost:50051
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
```

### <a name="examine-the-project-files"></a>Proje dosyalarını inceleyin

GrpcGreeter dosyaları:

* *greet.proto*: *Protos/greet.proto* dosyası tanımlar `Greeter` gRPC ve gRPC sunucu varlıkları oluşturmak için kullanılır. Daha fazla bilgi için [gRPC giriş](xref:grpc/index).
* *Hizmetleri* klasörü: Uygulamasını içerir `Greeter` hizmeti.
* *appSettings.json*: Kestrel'i tarafından kullanılan Protokolü gibi yapılandırma verilerini içerir. Daha fazla bilgi için bkz. <xref:fundamentals/configuration/index>.
* *Program.cs*: GRPC hizmeti için giriş noktası içerir. Daha fazla bilgi için bkz. <xref:fundamentals/host/web-host>.
* *Startup.cs*: Uygulama davranışını yapılandıran kodunu içerir. Daha fazla bilgi için [uygulama başlatma](xref:fundamentals/startup).

## <a name="create-the-grpc-client-in-a-net-console-app"></a>GRPC istemci bir .NET konsol uygulaması oluşturun

## <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Seçin **dosya** > **yeni** > **proje** menü çubuğundan.
* İçinde **yeni bir proje oluşturma** iletişim kutusunda **konsol uygulaması (.NET Core)** .
* Seçin **İleri**
* İçinde **adı** metin kutusunda, "GrpcGreeterClient" girin.
* **Oluştur**’u seçin.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Açık [tümleşik Terminalini](https://code.visualstudio.com/docs/editor/integrated-terminal).
* Dizinleri (`cd`) proje içeren bir klasör.
* Aşağıdaki komutları çalıştırın:

```console
dotnet new console -o GrpcGreeterClient
code -r GrpcGreeterClient
```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

Yönergeleri izleyerek [burada](/dotnet/core/tutorials/using-on-mac-vs-full-solution) adıyla bir konsol uygulaması oluşturmak için *GrpcGreeterClient*.

<!-- End of VS tabs -->

---

### <a name="add-required-packages"></a>Gerekli paketleri Ekle

Aşağıdaki paketler gRPC istemci projeye ekleyin:

* [Grpc.Core](https://www.nuget.org/packages/Grpc.Core), içeren C# C çekirdek istemci API'si.
* [Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), protobuf içeren API'leri için ileti C#.
* [Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), içeren C# protobuf dosyaları için araç desteği. Bağımlılık ile işaretlenmiş şekilde araçları paket çalışma zamanında gerekli değil `PrivateAssets="All"`.

### <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Paket Yöneticisi Konsolu (PMC) veya NuGet paketlerini Yönet'i kullanarak paketleri yükleyin

####  <a name="pmc-option-to-install-packages"></a>PMC seçeneği paketleri yüklemek için

* Visual Studio'dan seçin **Araçları** > **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu**
* Gelen **Paket Yöneticisi Konsolu** penceresinde olduğu dizine gidin *GrpcGreeterClient.csproj* dosya yok.
* Aşağıdaki komutları çalıştırın:

 ```powershell
Install-Package Grpc.Core
Install-Package Grpc.Protobuf
Install-Package Grpc.Tools
```

#### <a name="manage-nuget-packages-option-to-install-packages"></a>Paketleri yüklemek için seçeneği NuGet paketlerini Yönet

* Projeye sağ **Çözüm Gezgini** > **NuGet paketlerini Yönet**
* Seçin **Gözat** sekmesi.
* Girin **Grpc.Core** arama kutusuna.
* Seçin **Grpc.Core** gelen paket **Gözat** sekmenize **yükleme**.
* Yineleme için `Google.Protobuf` ve `Grpc.Tools`.

### <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Aşağıdaki komutları çalıştırın **tümleşik Terminalini**:

```console
dotnet add GrpcGreeterClient.csproj package Grpc.Core
dotnet add GrpcGreeterClient.csproj package Google.Protobuf
dotnet add GrpcGreeterClient.csproj package Grpc.Tools
```

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

* Sağ **paketleri** klasöründe **çözüm bölmesi** > **paketleri Ekle**
* Girin **Grpc.Core** arama kutusuna.
* Seçin **Grpc.Core** seçin ve sonuçlar bölmesinden paketini **' paket Ekle**
* Yineleme için `Google.Protobuf` ve `Grpc.Tools`.

---

### <a name="add-greetproto"></a>Greet.proto ekleyin

* Oluşturma bir **Protos** gRPC istemci proje klasöründe.
* Kopyalama **Protos\greet.proto** Greeter hizmet gRPC dosyasına gRPC istemci projesi.
* Düzen *GrpcGreeterClient.csproj* proje dosyası:

  # <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

  Projeye sağ tıklayıp **Düzenle GrpcGreeterClient.csproj**.

  # <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code) 

  Seçin *GrpcGreeterClient.csproj* dosya.

  # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

  Projeyi sağ tıklatın ve seçin **Araçlar > Dosya Düzenle**.

  ------

* Ekleme **greet.proto** dosyasını `<Protobuf>` GrpcGreeterClient proje dosyasının öğesi grubu:

  ```XML
  <ItemGroup>
    <Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
  </ItemGroup>
  ```

Nesil tetiklemek için istemci projesinin oluşturun C# istemci varlıklar.

### <a name="create-the-greater-client"></a>Büyük istemcisi oluşturma

Türlerini oluşturmak için projeyi derleyin **Greeter** ad alanı. `Greeter` Türleri yapı işlemi tarafından otomatik olarak oluşturulur.

GRPC istemci güncelleştirmesi *Program.cs* dosyasındaki kodu aşağıdaki kodla:

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet2)]

*Program.cs* gRPC istemci için giriş noktası ve mantığı içerir.

Daha fazla istemci tarafından oluşturulur:

* Örnekleme bir `Channel` gRPC hizmeti bağlantısı oluşturma hakkında bilgi içeren.
* Kullanarak `Channel` büyük istemci oluşturmak için:

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=4-6)]

Daha fazla istemci zaman uyumsuz olarak çağırır `SayHello` yöntemi. Sonucu `SayHello` çağrı görüntülenir:

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=7-9)]

Kapatma `Channel` tüm kaynakları serbest bırakmak için işlemleri tamamladığınızda, istemci tarafından kullanılmış.

## <a name="test-the-grpc-client-with-the-grpc-greeter-service"></a>GRPC Greeter hizmet gRPC istemciyle test etme

### <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Greeter hizmetinde hata ayıklayıcı olmadan sunucu başlatmak için Ctrl + F5 tuşlarına basın.
* GrpcGreeterClient projesinde hata ayıklayıcı olmadan sunucu başlatmak için Ctrl + F5 tuşlarına basın.

### <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code'u / Visual Studio Mac için](#tab/visual-studio-code+visual-studio-mac)

* Greeter hizmetini başlatın.
* İstemcisini başlatın.

<!-- End of combined VS/Mac tabs -->

---

İstemci bir karşılama "GreeterClient" adını içeren bir iletiyle hizmetine gönderir. Hizmet yanıt olarak "Hello GreeterClient" iletisi gönderir. "Hello GreeterClient" yanıt komut isteminde görüntülenir:

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

GRPC hizmeti, komut istemine yazılan günlüklerinde başarılı görüşmesinin ayrıntılarını kaydeder.

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: http://localhost:50051
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
      Content root path: C:\GH\aspnet\docs\4\Docs\aspnetcore\tutorials\grpc\grpc-start\sample\GrpcGreeter
info: Microsoft.AspNetCore.Hosting.Diagnostics[1]
      Request starting HTTP/2 POST http://localhost:50051/Greet.Greeter/SayHello application/grpc
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
