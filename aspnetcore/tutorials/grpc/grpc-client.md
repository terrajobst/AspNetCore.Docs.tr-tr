---
title: 'Öğretici: Bir .NET Core gRPC istemcisi oluşturma'
author: juntaoluo
description: Bu öğretici serisinde, ASP.NET Core, gRPC bir hizmet oluşturma işlemi gösterilmektedir. GRPC hizmeti projesi oluşturmak, proto dosyasını düzenleyin ve bir çift yönlü çağrı akış ekleme hakkında bilgi edinin.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 4/10/2019
uid: tutorials/grpc/grpc-client
ms.openlocfilehash: 031afbfaf097c518a85400b0b6abbc135c1bc611
ms.sourcegitcommit: 57a974556acd09363a58f38c26f74dc21e0d4339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59675793"
---
# <a name="tutorial-create-a-net-core-grpc-client"></a>Öğretici: Bir .NET Core gRPC istemcisi oluşturma

Tarafından [John Luo](https://github.com/juntaoluo)

Bu öğreticide, .NET Core oluşturma işlemi gösterilmektedir [gRPC](https://grpc.io/docs/guides/) gRPC Hizmetleri ile iletişim kurabilen istemci.

Sonunda, gRPC Greeter hizmet ile iletişim kuran gRPC istemci gerekir.

[!INCLUDE[View or download sample code](~/includes/grpc/downloadClient.md)]

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * GRPC istemci oluşturun.
> * Önceki öğreticide oluşturulan Greeter hizmet gRPC karşı hizmet çalıştırın.
> * Proje dosyalarını inceleyin.

[!INCLUDE[](~/includes/net-core-prereqs-all-3.0.md)]

## <a name="create-a-net-console-application"></a>Bir .NET konsol uygulaması oluşturun

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Yönergeleri izleyerek [burada](https://docs.microsoft.com/en-us/dotnet/core/tutorials/with-visual-studio) adıyla bir konsol uygulaması oluşturmak için *GrpcGreeterClient*.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Yönergeleri izleyerek [burada](https://docs.microsoft.com/en-us/dotnet/core/tutorials/with-visual-studio-code) adıyla bir konsol uygulaması oluşturmak için *GrpcGreeterClient*.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

Yönergeleri izleyerek [burada](https://docs.microsoft.com/en-us/dotnet/core/tutorials/using-on-mac-vs-full-solution) adıyla bir konsol uygulaması oluşturmak için *GrpcGreeterClient*.

<!-- End of VS tabs -->

---

## <a name="add-required-packages"></a>Gerekli paketleri Ekle

Aşağıdaki paketler gRPC istemci projeye ekleyin:

* [Grpc.Core](https://www.nuget.org/packages/Grpc.Core), içeren C# C çekirdek istemci API'si.
* [Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), protobuf içeren API'leri için ileti C#.
* [Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), içeren C# protobuf dosyaları için araç desteği. Bağımlılık ile işaretlenmiş şekilde araçları paket çalışma zamanında gerekli değil `PrivateAssets="All"`.

Paketleri ile aşağıdaki yaklaşımlardan eklenebilir:

### <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

### <a name="pmc-option-to-install-packages"></a>PMC seçeneği paketleri yüklemek için

* Visual Studio'dan seçin **Araçları** > **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu**
* Gelen **Paket Yöneticisi Konsolu** penceresinde olduğu dizine gidin *GrpcGreeterClient.csproj* dosya yok.
* Şu komutu çalıştırın:

    ```powershell
    Install-Package Grpc.Core
    ```

* Yineleme `Install-Package` Google.Protobuf ve Grpc.Tools

<!-- Tutorials shouldn't have multiple options. Select what you think is the best approach. Recommend you removed this approach. -->

### <a name="manage-nuget-packages-option-to-install-packages"></a>Paketleri yüklemek için seçeneği NuGet paketlerini Yönet

* Projeye sağ **Çözüm Gezgini** > **NuGet paketlerini Yönet**
* Ayarlama **paket kaynağı** "nuget.org'da"
* Arama kutusuna "Grpc.Core"
* "Grpc.Core" paketinden seçin **Gözat** sekmesine **yükleyin**
* Google.Protobuf ve Grpc.Tools için yineleyin.

### <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Aşağıdaki komutu çalıştırın **tümleşik Terminalini**:

```console
dotnet add TodoApi.csproj package Grpc.Core
```

Google.Protobuf ve Grpc.Tools için yineleyin.

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

* Sağ *paketleri* klasöründe **çözüm bölmesi** > **paketleri Ekle...**
* Ayarlama **paketleri Ekle** pencerenin **kaynak** "nuget.org" açılır menüsünü
* Arama kutusuna "Grpc.Core"
* Sonuçlar bölmesinde "Grpc.Core" paketi seçin ve tıklayın **' paket Ekle**
* Google.Protobuf ve Grpc.Tools için yineleyin.

---

## <a name="add-the-greetproto-file"></a>Greet.proto dosyası ekleme

Kopyalama **Protos\greet.proto** Greeter hizmet gRPC dosyasına gRPC istemci projesi. Ekleme **greet.proto** dosyasını `<Protobuf>` GrpcGreeterClient proje dosyasının öğesi grubu:

```XML
<Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
```

> [!NOTE]
> Projeye sağ tıklatıp seçerek GrpcGreeterClient proje dosyasını açabilirsiniz **Düzenle GrpcGreeterClient.csproj** açılan menüden seçeneği.
>
> ![Yeni ASP.NET Core Web uygulaması](grpc-start/_static/edit_csproj.png)
>
> Alternatif olarak, GrpcGreeterClient dizine gidin ve düzenleme `GrpcGreeterClient.csproj` tercih ettiğiniz düzenleyiciyi ile.

`GrpcServices="Client"` Özniteliği, yalnızca eklenir C# istemci varlıklar için dahil edilen protobuf dosyası oluşturulur. Nesil tetiklemek için istemci projesinin oluşturun C# istemci varlıklar.

## <a name="create-a-greeterclient-and-invoke-the-sayhello-unary-call"></a>SayHello birli arama çağırmak ve bir GreeterClient oluşturma

Aşağıdaki kodu ekleyin `Main` yöntemi `Program.cs` gRPC istemci projesinin dosya:

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=snippet)]

Aşağıdakileri kullanarak gerekli türlerine erişmek için deyimleri gereklidir:

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=using)]

GreeterClient örneği tarafından oluşturulan bir `Channel` gRPC hizmeti bağlantısı oluşturma ve oluşturmak için kullanmaya yönelik bilgileri içeren `GreeterClient`:

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=snippet&highlight=4-5)]

Birli çağrı GreeterClient içeren `SayHello` olduğu çağrılacak zaman uyumsuz olarak:

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=snippet&highlight=7-8)]

Sonuçları `SayHello` çağrı depolanan `reply` , ardından görüntülenebilir:

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=snippet&highlight=9)]

`Channel` Tarafından kullanılan tüm kaynakları serbest bırakmak için işlemleri tamamladıktan sonra istemcinin kapatılması:

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=snippet&highlight=11)]

> [!NOTE]
> Türlerinde önce projeyi derlemek ihtiyacınız olacak **Greeter** ad alanı çözümlenebilir. Bu türler, derleme sırasında otomatik olarak oluşturulur ve bir derleme çalıştırılmadan önce kullanılamaz olduğu.

## <a name="test-the-grpc-client-with-the-grpc-greeter-service"></a>GRPC Greeter hizmet gRPC istemciyle test etme

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Greeter önceki öğreticide oluşturulan hizmetinin çalıştığından emin olun.

* Hizmet çalışmaya başladıktan sonra dönmek **GrpcGreeterClient** projeyi başlangıç projesi olarak ayarlayın. Hata Ayıklayıcı olmadan istemciyi çalıştırmak için CTRL + F5 tuşlarına basın.

  İstemci bir karşılama "GreeterClient" adını içeren bir iletiyle hizmetine gönderir. Hizmet, komut satırında görüntülenen yanıt olarak bir "Hello GreeterClient" ileti gönderir.

  ![Yeni ASP.NET Core Web uygulaması](grpc-start/_static/client.png)

  Hizmet, komut istemine yazılan günlüklerinde başarılı görüşmesinin ayrıntılarını kaydeder.

  ![Yeni ASP.NET Core Web uygulaması](grpc-start/_static/server_complete.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code'u / Visual Studio Mac için](#tab/visual-studio-code+visual-studio-mac)

* Greeter önceki öğreticide oluşturulan hizmetinin çalıştığından emin olun.

* İstemci projesi GrpcGreeter.Client kullanılarak ayrı bir komut satırından çalıştırmak `dotnet run`.

İstemci bir karşılama "GreeterClient" adını içeren bir iletiyle hizmetine gönderir. Hizmet, komut satırında görüntülenen yanıt olarak bir "Hello GreeterClient" ileti gönderir.

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

Hizmet, komut istemine yazılan günlüklerinde başarılı görüşmesinin ayrıntılarını kaydeder.

```console
dbug: Grpc.AspNetCore.Server.Internal.GrpcServiceBinder[1]
      Added gRPC method 'SayHello' to service 'Greet.Greeter'. Method type: 'Unary', route pattern: '/Greet.Greeter/SayHello'.
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: http://localhost:50051
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
      Content root path: C:\gh\Docs\aspnetcore\tutorials\grpc\grpc-start\samples\GrpcGreeter
info: Microsoft.AspNetCore.Hosting.Internal.GenericWebHostService[1]
      Request starting HTTP/2 POST http://localhost:50051/Greet.Greeter/SayHello application/grpc
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[0]
      Executing endpoint 'gRPC - /Greet.Greeter/SayHello'
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[1]
      Executed endpoint 'gRPC - /Greet.Greeter/SayHello'
info: Microsoft.AspNetCore.Hosting.Internal.GenericWebHostService[2]
      Request finished in 194.5798ms 200 application/grpc
```

<!-- End of combined VS/Mac tabs -->

---

### <a name="examine-the-project-files-of-the-grpc-project"></a>GRPC projenin proje dosyalarını inceleyin

gRPC istemci GrpcGreeterClient dosyası:

*Program.cs* gRPC istemci için giriş noktası ve mantığı içerir.

## <a name="additional-resources"></a>Ek kaynaklar

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * GRPC istemci oluşturun.
> * Önceki öğreticide oluşturulan Greeter hizmet gRPC karşı hizmet çalıştırın.
> * Proje dosyalarını inceleyin.

> [!div class="step-by-step"]
> [Önceki: Bir gRPC Greeter hizmeti oluşturma](xref:tutorials/grpc/grpc-start)
