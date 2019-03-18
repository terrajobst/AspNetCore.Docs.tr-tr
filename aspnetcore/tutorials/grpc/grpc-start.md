---
title: "Öğretici: ASP.NET core'da gRPC kullanmaya başlama"
author: juntaoluo
description: Bu öğretici serisinde, ASP.NET Core, gRPC bir hizmet oluşturma işlemi gösterilmektedir. GRPC hizmeti projesi oluşturmak, proto dosyasını düzenleyin ve bir çift yönlü çağrı akış ekleme hakkında bilgi edinin.
ms.author: johluo
ms.date: 2/26/2019
uid: tutorials/grpc/grpc-start
ms.openlocfilehash: 793d13d0fbac4f2f46b7cd8490ded294e597ca9d
ms.sourcegitcommit: a467828b5e4eaae291d961ffe2279a571900de23
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/18/2019
ms.locfileid: "58142627"
---
# <a name="tutorial-get-started-with-grpc-service-in-aspnet-core"></a><span data-ttu-id="559e8-104">Öğretici: GRPC hizmetinde ASP.NET Core ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="559e8-104">Tutorial: Get started with gRPC service in ASP.NET Core</span></span>

<span data-ttu-id="559e8-105">Tarafından [John Luo](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="559e8-105">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="559e8-106">Bu öğretici, ASP.NET Core, gRPC hizmeti oluşturmaya ilişkin temel bilgileri öğretir.</span><span class="sxs-lookup"><span data-stu-id="559e8-106">This tutorial teaches the basics of building a gRPC service on ASP.NET Core.</span></span>

<span data-ttu-id="559e8-107">Sonunda, greetings yankılayan gRPC hizmet sahip olacaksınız.</span><span class="sxs-lookup"><span data-stu-id="559e8-107">At the end, you'll have a gRPC service that echoes greetings.</span></span>

[!INCLUDE[View or download sample code](~/includes/grpc/download.md)]

<span data-ttu-id="559e8-108">Bu öğreticide şunları yaptınız:</span><span class="sxs-lookup"><span data-stu-id="559e8-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="559e8-109">GRPC hizmeti oluşturun.</span><span class="sxs-lookup"><span data-stu-id="559e8-109">Create a gRPC service.</span></span>
> * <span data-ttu-id="559e8-110">Hizmet çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="559e8-110">Run the service.</span></span>
> * <span data-ttu-id="559e8-111">Proje dosyalarını inceleyin.</span><span class="sxs-lookup"><span data-stu-id="559e8-111">Examine the project files.</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-all-3.0.md)]

## <a name="create-a-grpc-service"></a><span data-ttu-id="559e8-112">GRPC hizmeti oluşturma</span><span class="sxs-lookup"><span data-stu-id="559e8-112">Create a gRPC service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="559e8-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="559e8-113">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="559e8-114">Visual Studio'dan **dosya** menüsünde **yeni** > **proje**.</span><span class="sxs-lookup"><span data-stu-id="559e8-114">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="559e8-115">Yeni bir ASP.NET Core Web uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="559e8-115">Create a new ASP.NET Core Web Application.</span></span>
  <span data-ttu-id="559e8-116">![Yeni ASP.NET Core Web uygulaması](grpc-start/_static/np_3_0.1.png)</span><span class="sxs-lookup"><span data-stu-id="559e8-116">![new ASP.NET Core Web Application](grpc-start/_static/np_3_0.1.png)</span></span>
* <span data-ttu-id="559e8-117">Projeyi adlandırın **GrpcGreeter**.</span><span class="sxs-lookup"><span data-stu-id="559e8-117">Name the project **GrpcGreeter**.</span></span> <span data-ttu-id="559e8-118">Projeyi adlandırın önemlidir *GrpcGreeter* kodu kopyalayıp, ad alanlarını eşleşecek şekilde.</span><span class="sxs-lookup"><span data-stu-id="559e8-118">It's important to name the project *GrpcGreeter* so the namespaces will match when you copy and paste code.</span></span>
  <span data-ttu-id="559e8-119">![Yeni ASP.NET Core Web uygulaması](grpc-start/_static/np_3_0.2.png)</span><span class="sxs-lookup"><span data-stu-id="559e8-119">![new ASP.NET Core Web Application](grpc-start/_static/np_3_0.2.png)</span></span>
* <span data-ttu-id="559e8-120">Seçin **.NET Core** ve **ASP.NET Core 3.0** açılır.</span><span class="sxs-lookup"><span data-stu-id="559e8-120">Select **.NET Core** and **ASP.NET Core 3.0** in the dropdown.</span></span> <span data-ttu-id="559e8-121">Seçin **gRPC hizmet** şablonu.</span><span class="sxs-lookup"><span data-stu-id="559e8-121">Choose the **gRPC Service** template.</span></span>

  <span data-ttu-id="559e8-122">Aşağıdaki başlangıç projesini oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="559e8-122">The following starter project is created:</span></span>

  ![Çözüm Gezgini](grpc-start/_static/se3.0.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="559e8-124">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="559e8-124">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="559e8-125">Açık [tümleşik Terminalini](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="559e8-125">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="559e8-126">Dizinleri (`cd`) proje içeren bir klasör.</span><span class="sxs-lookup"><span data-stu-id="559e8-126">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="559e8-127">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="559e8-127">Run the following commands:</span></span>

  ```console
  dotnet new grpc -o GrpcGreeter
  code -r GrpcGreeter
  ```

  * <span data-ttu-id="559e8-128">`dotnet new` Komut yeni bir gRPC hizmetinde oluşturur *GrpcGreeter* klasör.</span><span class="sxs-lookup"><span data-stu-id="559e8-128">The `dotnet new` command creates a new gRPC service in the *GrpcGreeter* folder.</span></span>
  * <span data-ttu-id="559e8-129">`code` Komutu açılır *GrpcGreeter* Visual Studio Code yeni bir örneğini klasöründe.</span><span class="sxs-lookup"><span data-stu-id="559e8-129">The `code` command opens the *GrpcGreeter* folder in a new instance of Visual Studio Code.</span></span>

  <span data-ttu-id="559e8-130">Bir iletişim kutusu görünür **gerekli varlıkları oluşturun ve hata ayıklama 'GrpcGreeter' eksik. Bunları eklensin mi?**</span><span class="sxs-lookup"><span data-stu-id="559e8-130">A dialog box appears with **Required assets to build and debug are missing from 'GrpcGreeter'. Add them?**</span></span>
* <span data-ttu-id="559e8-131">Seçin **Evet**</span><span class="sxs-lookup"><span data-stu-id="559e8-131">Select **Yes**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="559e8-132">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="559e8-132">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="559e8-133">Bir terminalde aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="559e8-133">From a terminal, run the following commands:</span></span>

```console
  dotnet new grpc -o GrpcGreeter
  cd GrpcGreeter
```

<span data-ttu-id="559e8-134">Kullanım komutları önceki [.NET Core CLI](/dotnet/core/tools/dotnet) gRPC hizmet oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="559e8-134">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a gRPC service.</span></span>

### <a name="open-the-project"></a><span data-ttu-id="559e8-135">Projeyi açın</span><span class="sxs-lookup"><span data-stu-id="559e8-135">Open the project</span></span>

<span data-ttu-id="559e8-136">Visual Studio'dan seçin **Dosya > Aç**ve ardından *GrpcGreeter.sln* dosya.</span><span class="sxs-lookup"><span data-stu-id="559e8-136">From Visual Studio, select **File > Open**, and then select the *GrpcGreeter.sln* file.</span></span>

<!-- End of VS tabs -->

---

### <a name="test-the-service"></a><span data-ttu-id="559e8-137">Hizmeti test etme</span><span class="sxs-lookup"><span data-stu-id="559e8-137">Test the service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="559e8-138">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="559e8-138">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="559e8-139">Olun **GrpcGreeter.Server** başlangıç projesi olarak ayarlayın ve hata ayıklayıcı olmadan gRPC hizmeti çalıştırmak için Ctrl + F5 tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="559e8-139">Ensure the **GrpcGreeter.Server** is set as the Startup Project and press Ctrl+F5 to run the gRPC service without the debugger.</span></span>

  <span data-ttu-id="559e8-140">Visual Studio hizmeti, bir komut istemi'nde çalışır.</span><span class="sxs-lookup"><span data-stu-id="559e8-140">Visual Studio runs the service in a command prompt.</span></span> <span data-ttu-id="559e8-141">Hizmet üzerinde dinleme başlatıldı günlükleri göster `http://localhost:50051`.</span><span class="sxs-lookup"><span data-stu-id="559e8-141">The logs show that the service started listening on `http://localhost:50051`.</span></span>

  ![Yeni ASP.NET Core Web uygulaması](grpc-start/_static/server_start.png)

* <span data-ttu-id="559e8-143">Hizmet çalışmaya başladıktan sonra ayarlanmış **GrpcGreeter.Client** başlangıç projesi olarak ayarlayın ve hata ayıklayıcı olmadan istemciyi çalıştırmak için Ctrl + F5 tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="559e8-143">Once the service is running, set the **GrpcGreeter.Client** is set as the Startup Project and press Ctrl+F5 to run the client without the debugger.</span></span>

  <span data-ttu-id="559e8-144">İstemci bir karşılama "GreeterClient" adını içeren bir iletiyle hizmetine gönderir.</span><span class="sxs-lookup"><span data-stu-id="559e8-144">The client sends a greeting to the service with a message containing its name "GreeterClient".</span></span> <span data-ttu-id="559e8-145">Hizmet, komut satırında görüntülenen yanıt olarak bir "Hello GreeterClient" ileti gönderir.</span><span class="sxs-lookup"><span data-stu-id="559e8-145">The service will send a message "Hello GreeterClient" as a response that is displayed in the command prompt.</span></span>

  ![Yeni ASP.NET Core Web uygulaması](grpc-start/_static/client.png)

  <span data-ttu-id="559e8-147">Hizmet, komut istemine yazılan günlüklerinde başarılı görüşmesinin ayrıntılarını kaydeder.</span><span class="sxs-lookup"><span data-stu-id="559e8-147">The service records the details of the successful call in the logs written to the command prompt.</span></span>

  ![Yeni ASP.NET Core Web uygulaması](grpc-start/_static/server_complete.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="559e8-149">Visual Studio Code'u / Visual Studio Mac için</span><span class="sxs-lookup"><span data-stu-id="559e8-149">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="559e8-150">Sunucu projesi GrpcGreeter.Server kullanılarak komut satırından çalıştırmak `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="559e8-150">Run the Server project GrpcGreeter.Server from the command line using `dotnet run`.</span></span> <span data-ttu-id="559e8-151">Hizmet üzerinde dinleme başlatıldı günlükleri göster `http://localhost:50051`.</span><span class="sxs-lookup"><span data-stu-id="559e8-151">The logs show that the service started listening on `http://localhost:50051`.</span></span>

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: http://localhost:50051
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
      Content root path: C:\example\GrpcGreeter\GrpcGreeter.Server
```

* <span data-ttu-id="559e8-152">İstemci projesi GrpcGreeter.Client kullanılarak ayrı bir komut satırından çalıştırmak `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="559e8-152">Run the Client project GrpcGreeter.Client from the separate command line using `dotnet run`.</span></span>

<span data-ttu-id="559e8-153">İstemci bir karşılama "GreeterClient" adını içeren bir iletiyle hizmetine gönderir.</span><span class="sxs-lookup"><span data-stu-id="559e8-153">The client sends a greeting to the service with a message containing its name "GreeterClient".</span></span> <span data-ttu-id="559e8-154">Hizmet, komut satırında görüntülenen yanıt olarak bir "Hello GreeterClient" ileti gönderir.</span><span class="sxs-lookup"><span data-stu-id="559e8-154">The service will send a message "Hello GreeterClient" as a response that is displayed in the command prompt.</span></span>

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

<span data-ttu-id="559e8-155">Hizmet, komut istemine yazılan günlüklerinde başarılı görüşmesinin ayrıntılarını kaydeder.</span><span class="sxs-lookup"><span data-stu-id="559e8-155">The service records the details of the successful call in the logs written to the command prompt.</span></span>

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: http://localhost:50051
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
      Content root path: C:\gh\tp\GrpcGreeter\GrpcGreeter.Server
info: Microsoft.AspNetCore.Hosting.Internal.GenericWebHostService[1]
      Request starting HTTP/2 POST http://localhost:50051/Greet.Greeter/SayHello application/grpc
info: Microsoft.AspNetCore.Hosting.Internal.GenericWebHostService[2]
      Request finished in 107.46730000000001ms 200 application/grpc
```

<!-- End of combined VS/Mac tabs -->

---

### <a name="examine-the-project-files-of-the-grpc-project"></a><span data-ttu-id="559e8-156">GRPC projenin proje dosyalarını inceleyin</span><span class="sxs-lookup"><span data-stu-id="559e8-156">Examine the project files of the gRPC project</span></span>

<span data-ttu-id="559e8-157">GrpcGreeter.Server dosyaları:</span><span class="sxs-lookup"><span data-stu-id="559e8-157">GrpcGreeter.Server files:</span></span>

* <span data-ttu-id="559e8-158">greet.proto: *Protos/greet.proto* dosyası tanımlar `Greeter` gRPC ve gRPC sunucu varlıkları oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="559e8-158">greet.proto: The *Protos/greet.proto* file defines the `Greeter` gRPC and is used to generate the gRPC server assets.</span></span> <span data-ttu-id="559e8-159">Daha fazla bilgi için bkz. <xref:grpc/index>.</span><span class="sxs-lookup"><span data-stu-id="559e8-159">For more information, see <xref:grpc/index>.</span></span>
* <span data-ttu-id="559e8-160">*Hizmetleri* klasörü: Uygulamasını içerir `Greeter` hizmeti.</span><span class="sxs-lookup"><span data-stu-id="559e8-160">*Services* folder: Contains the implementation of the `Greeter` service.</span></span>
* <span data-ttu-id="559e8-161">*appSettings.json*: Kestrel tarafından kullanılan Protokolü gibi yapılandırma verilerini içerir.</span><span class="sxs-lookup"><span data-stu-id="559e8-161">*appSettings.json*:Contains configuration data, such as protocol used by Kestrel.</span></span> <span data-ttu-id="559e8-162">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="559e8-162">For more information, see <xref:fundamentals/configuration/index>.</span></span>
* <span data-ttu-id="559e8-163">*Program.cs*: GRPC hizmeti için giriş noktası içerir.</span><span class="sxs-lookup"><span data-stu-id="559e8-163">*Program.cs*: Contains the entry point for the gRPC service.</span></span> <span data-ttu-id="559e8-164">Daha fazla bilgi için bkz. <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="559e8-164">For more information, see <xref:fundamentals/host/web-host>.</span></span>
* <span data-ttu-id="559e8-165">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="559e8-165">Startup.cs</span></span>

<span data-ttu-id="559e8-166">Uygulama davranışını yapılandıran kodunu içerir.</span><span class="sxs-lookup"><span data-stu-id="559e8-166">Contains code that configures app behavior.</span></span> <span data-ttu-id="559e8-167">Daha fazla bilgi için bkz. <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="559e8-167">For more information, see <xref:fundamentals/startup>.</span></span>

<span data-ttu-id="559e8-168">gRPC istemci GrpcGreeter.Client dosyası:</span><span class="sxs-lookup"><span data-stu-id="559e8-168">gRPC client GrpcGreeter.Client file:</span></span>

<span data-ttu-id="559e8-169">*Program.cs* gRPC istemci için giriş noktası ve mantığı içerir.</span><span class="sxs-lookup"><span data-stu-id="559e8-169">*Program.cs* contains the entry point and logic for the gRPC client.</span></span>

<span data-ttu-id="559e8-170">Bu öğreticide şunları yaptınız:</span><span class="sxs-lookup"><span data-stu-id="559e8-170">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="559e8-171">GRPC hizmet oluşturdunuz.</span><span class="sxs-lookup"><span data-stu-id="559e8-171">Created a gRPC service.</span></span>
> * <span data-ttu-id="559e8-172">Hizmet ve istemci hizmeti test etmek kaldı.</span><span class="sxs-lookup"><span data-stu-id="559e8-172">Ran the service and a client to test the service.</span></span>
> * <span data-ttu-id="559e8-173">Proje dosyalarını incelenir.</span><span class="sxs-lookup"><span data-stu-id="559e8-173">Examined the project files.</span></span>
