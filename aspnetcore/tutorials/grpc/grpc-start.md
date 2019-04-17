---
title: "Öğretici: ASP.NET core'da gRPC kullanmaya başlama"
author: juntaoluo
description: Bu öğretici serisinde, ASP.NET Core, gRPC bir hizmet oluşturma işlemi gösterilmektedir. GRPC hizmeti projesi oluşturmak, proto dosyasını düzenleyin ve bir çift yönlü çağrı akış ekleme hakkında bilgi edinin.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 2/26/2019
uid: tutorials/grpc/grpc-start
ms.openlocfilehash: a67050331cc8563b1baf5312b92910c1bbdfbd69
ms.sourcegitcommit: 57a974556acd09363a58f38c26f74dc21e0d4339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59672573"
---
# <a name="tutorial-get-started-with-grpc-service-in-aspnet-core"></a><span data-ttu-id="a8cbd-104">Öğretici: ASP.NET Core’da gRPC hizmeti ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="a8cbd-104">Tutorial: Get started with gRPC service in ASP.NET Core</span></span>

<span data-ttu-id="a8cbd-105">Tarafından [John Luo](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="a8cbd-105">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="a8cbd-106">Bu öğretici, ASP.NET Core, gRPC hizmeti oluşturmaya ilişkin temel bilgileri öğretir.</span><span class="sxs-lookup"><span data-stu-id="a8cbd-106">This tutorial teaches the basics of building a gRPC service on ASP.NET Core.</span></span>

<span data-ttu-id="a8cbd-107">Sonunda, greetings yankılayan gRPC hizmet sahip olacaksınız.</span><span class="sxs-lookup"><span data-stu-id="a8cbd-107">At the end, you'll have a gRPC service that echoes greetings.</span></span>

[!INCLUDE[View or download sample code](~/includes/grpc/download.md)]

<span data-ttu-id="a8cbd-108">Bu öğreticide şunları yaptınız:</span><span class="sxs-lookup"><span data-stu-id="a8cbd-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a8cbd-109">GRPC hizmeti oluşturun.</span><span class="sxs-lookup"><span data-stu-id="a8cbd-109">Create a gRPC service.</span></span>
> * <span data-ttu-id="a8cbd-110">GRPC hizmet çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="a8cbd-110">Run the gRPC service.</span></span>
> * <span data-ttu-id="a8cbd-111">Proje dosyalarını inceleyin.</span><span class="sxs-lookup"><span data-stu-id="a8cbd-111">Examine the project files.</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-all-3.0.md)]

## <a name="create-a-grpc-service"></a><span data-ttu-id="a8cbd-112">GRPC hizmeti oluşturma</span><span class="sxs-lookup"><span data-stu-id="a8cbd-112">Create a gRPC service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a8cbd-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a8cbd-113">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a8cbd-114">Visual Studio'dan **dosya** menüsünde **yeni** > **proje**.</span><span class="sxs-lookup"><span data-stu-id="a8cbd-114">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="a8cbd-115">Yeni bir ASP.NET Core Web uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="a8cbd-115">Create a new ASP.NET Core Web Application.</span></span>
  <span data-ttu-id="a8cbd-116">![Yeni ASP.NET Core Web uygulaması](grpc-start/_static/np_3_0.1.png)</span><span class="sxs-lookup"><span data-stu-id="a8cbd-116">![new ASP.NET Core Web Application](grpc-start/_static/np_3_0.1.png)</span></span>
* <span data-ttu-id="a8cbd-117">Projeyi adlandırın **GrpcGreeter**.</span><span class="sxs-lookup"><span data-stu-id="a8cbd-117">Name the project **GrpcGreeter**.</span></span> <span data-ttu-id="a8cbd-118">Projeyi adlandırın önemlidir *GrpcGreeter* kodu kopyalayıp, ad alanlarını eşleşecek şekilde.</span><span class="sxs-lookup"><span data-stu-id="a8cbd-118">It's important to name the project *GrpcGreeter* so the namespaces will match when you copy and paste code.</span></span>
  <span data-ttu-id="a8cbd-119">![Yeni ASP.NET Core Web uygulaması](grpc-start/_static/np_3_0.2.png)</span><span class="sxs-lookup"><span data-stu-id="a8cbd-119">![new ASP.NET Core Web Application](grpc-start/_static/np_3_0.2.png)</span></span>
* <span data-ttu-id="a8cbd-120">Seçin **.NET Core** ve **ASP.NET Core 3.0** açılır.</span><span class="sxs-lookup"><span data-stu-id="a8cbd-120">Select **.NET Core** and **ASP.NET Core 3.0** in the dropdown.</span></span> <span data-ttu-id="a8cbd-121">Seçin **gRPC hizmet** şablonu.</span><span class="sxs-lookup"><span data-stu-id="a8cbd-121">Choose the **gRPC Service** template.</span></span>

  <span data-ttu-id="a8cbd-122">Aşağıdaki başlangıç projesini oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="a8cbd-122">The following starter project is created:</span></span>

  ![Çözüm Gezgini](grpc-start/_static/se3.0.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a8cbd-124">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a8cbd-124">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="a8cbd-125">Açık [tümleşik Terminalini](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="a8cbd-125">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="a8cbd-126">Dizinleri (`cd`) proje içeren bir klasör.</span><span class="sxs-lookup"><span data-stu-id="a8cbd-126">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="a8cbd-127">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="a8cbd-127">Run the following commands:</span></span>

  ```console
  dotnet new grpc -o GrpcGreeter
  code -r GrpcGreeter
  ```

  * <span data-ttu-id="a8cbd-128">`dotnet new` Komut yeni bir gRPC hizmetinde oluşturur *GrpcGreeter* klasör.</span><span class="sxs-lookup"><span data-stu-id="a8cbd-128">The `dotnet new` command creates a new gRPC service in the *GrpcGreeter* folder.</span></span>
  * <span data-ttu-id="a8cbd-129">`code` Komutu açılır *GrpcGreeter* Visual Studio Code yeni bir örneğini klasöründe.</span><span class="sxs-lookup"><span data-stu-id="a8cbd-129">The `code` command opens the *GrpcGreeter* folder in a new instance of Visual Studio Code.</span></span>

  <span data-ttu-id="a8cbd-130">Bir iletişim kutusu görünür **gerekli varlıkları oluşturun ve hata ayıklama 'GrpcGreeter' eksik. Bunları eklensin mi?**</span><span class="sxs-lookup"><span data-stu-id="a8cbd-130">A dialog box appears with **Required assets to build and debug are missing from 'GrpcGreeter'. Add them?**</span></span>
* <span data-ttu-id="a8cbd-131">Seçin **Evet**</span><span class="sxs-lookup"><span data-stu-id="a8cbd-131">Select **Yes**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="a8cbd-132">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a8cbd-132">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="a8cbd-133">Bir terminalde aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="a8cbd-133">From a terminal, run the following commands:</span></span>

```console
  dotnet new grpc -o GrpcGreeter
  cd GrpcGreeter
```

<span data-ttu-id="a8cbd-134">Kullanım komutları önceki [.NET Core CLI](/dotnet/core/tools/dotnet) gRPC hizmet oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="a8cbd-134">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a gRPC service.</span></span>

### <a name="open-the-project"></a><span data-ttu-id="a8cbd-135">Projeyi açın</span><span class="sxs-lookup"><span data-stu-id="a8cbd-135">Open the project</span></span>

<span data-ttu-id="a8cbd-136">Visual Studio'dan seçin **Dosya > Aç**ve ardından *GrpcGreeter.sln* dosya.</span><span class="sxs-lookup"><span data-stu-id="a8cbd-136">From Visual Studio, select **File > Open**, and then select the *GrpcGreeter.sln* file.</span></span>

<!-- End of VS tabs -->

---

### <a name="run-the-service"></a><span data-ttu-id="a8cbd-137">Hizmet çalıştırma</span><span class="sxs-lookup"><span data-stu-id="a8cbd-137">Run the service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a8cbd-138">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a8cbd-138">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a8cbd-139">Hata Ayıklayıcı olmadan gRPC hizmeti çalıştırmak için CTRL + F5 tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="a8cbd-139">Press Ctrl+F5 to run the gRPC service without the debugger.</span></span>

  <span data-ttu-id="a8cbd-140">Visual Studio hizmeti, bir komut istemi'nde çalışır.</span><span class="sxs-lookup"><span data-stu-id="a8cbd-140">Visual Studio runs the service in a command prompt.</span></span> <span data-ttu-id="a8cbd-141">Hizmet üzerinde dinleme başlatıldı günlükleri göster `http://localhost:50051`.</span><span class="sxs-lookup"><span data-stu-id="a8cbd-141">The logs show that the service started listening on `http://localhost:50051`.</span></span>

  ![Yeni ASP.NET Core Web uygulaması](grpc-start/_static/server_start.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="a8cbd-143">Visual Studio Code'u / Visual Studio Mac için</span><span class="sxs-lookup"><span data-stu-id="a8cbd-143">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="a8cbd-144">GRPC Greeter projeyi GrpcGreeter kullanılarak komut satırından çalıştırmak `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="a8cbd-144">Run the gRPC Greeter project GrpcGreeter from the command line using `dotnet run`.</span></span> <span data-ttu-id="a8cbd-145">Hizmet üzerinde dinleme başlatıldı günlükleri göster `http://localhost:50051`.</span><span class="sxs-lookup"><span data-stu-id="a8cbd-145">The logs show that the service started listening on `http://localhost:50051`.</span></span>

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
```

<!-- End of combined VS/Mac tabs -->

---

<span data-ttu-id="a8cbd-146">Sonraki öğreticiye Greeter hizmeti test etmek için gerekli bir gRPC istemci derleme işlemi gösterilecektir.</span><span class="sxs-lookup"><span data-stu-id="a8cbd-146">The next tutorial will demonstrate how to build a gRPC client, which is required to test the Greeter service.</span></span>

### <a name="examine-the-project-files-of-the-grpc-project"></a><span data-ttu-id="a8cbd-147">GRPC projenin proje dosyalarını inceleyin</span><span class="sxs-lookup"><span data-stu-id="a8cbd-147">Examine the project files of the gRPC project</span></span>

<span data-ttu-id="a8cbd-148">GrpcGreeter dosyaları:</span><span class="sxs-lookup"><span data-stu-id="a8cbd-148">GrpcGreeter files:</span></span>

* <span data-ttu-id="a8cbd-149">greet.proto: *Protos/greet.proto* dosyası tanımlar `Greeter` gRPC ve gRPC sunucu varlıkları oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a8cbd-149">greet.proto: The *Protos/greet.proto* file defines the `Greeter` gRPC and is used to generate the gRPC server assets.</span></span> <span data-ttu-id="a8cbd-150">Daha fazla bilgi için bkz. <xref:grpc/index>.</span><span class="sxs-lookup"><span data-stu-id="a8cbd-150">For more information, see <xref:grpc/index>.</span></span>
* <span data-ttu-id="a8cbd-151">*Hizmetleri* klasörü: Uygulamasını içerir `Greeter` hizmeti.</span><span class="sxs-lookup"><span data-stu-id="a8cbd-151">*Services* folder: Contains the implementation of the `Greeter` service.</span></span>
* <span data-ttu-id="a8cbd-152">*appSettings.json*: Kestrel tarafından kullanılan Protokolü gibi yapılandırma verilerini içerir.</span><span class="sxs-lookup"><span data-stu-id="a8cbd-152">*appSettings.json*:Contains configuration data, such as protocol used by Kestrel.</span></span> <span data-ttu-id="a8cbd-153">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="a8cbd-153">For more information, see <xref:fundamentals/configuration/index>.</span></span>
* <span data-ttu-id="a8cbd-154">*Program.cs*: GRPC hizmeti için giriş noktası içerir.</span><span class="sxs-lookup"><span data-stu-id="a8cbd-154">*Program.cs*: Contains the entry point for the gRPC service.</span></span> <span data-ttu-id="a8cbd-155">Daha fazla bilgi için bkz. <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="a8cbd-155">For more information, see <xref:fundamentals/host/web-host>.</span></span>
* <span data-ttu-id="a8cbd-156">Startup.cs: Uygulama davranışını yapılandıran kodunu içerir.</span><span class="sxs-lookup"><span data-stu-id="a8cbd-156">Startup.cs: Contains code that configures app behavior.</span></span> <span data-ttu-id="a8cbd-157">Daha fazla bilgi için bkz. <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="a8cbd-157">For more information, see <xref:fundamentals/startup>.</span></span>

### <a name="test-the-service"></a><span data-ttu-id="a8cbd-158">Hizmeti test etme</span><span class="sxs-lookup"><span data-stu-id="a8cbd-158">Test the service</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a8cbd-159">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="a8cbd-159">Additional resources</span></span>

<span data-ttu-id="a8cbd-160">Bu öğreticide şunları yaptınız:</span><span class="sxs-lookup"><span data-stu-id="a8cbd-160">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a8cbd-161">GRPC hizmet oluşturdunuz.</span><span class="sxs-lookup"><span data-stu-id="a8cbd-161">Created a gRPC service.</span></span>
> * <span data-ttu-id="a8cbd-162">GRPC hizmet çalıştı.</span><span class="sxs-lookup"><span data-stu-id="a8cbd-162">Ran the gRPC service.</span></span>
> * <span data-ttu-id="a8cbd-163">Proje dosyalarını incelenir.</span><span class="sxs-lookup"><span data-stu-id="a8cbd-163">Examined the project files.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="a8cbd-164">Sonraki: Bir .NET Core gRPC istemcisi oluşturma</span><span class="sxs-lookup"><span data-stu-id="a8cbd-164">Next: Create a .NET Core gRPC client</span></span>](xref:tutorials/grpc/grpc-client)