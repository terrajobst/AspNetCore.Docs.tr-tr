---
title: ASP.NET Core .NET Core gRPC istemci ve sunucu oluşturma
author: juntaoluo
description: Bu öğreticide, ASP.NET Core, hizmet ve gRPC gRPC istemci oluşturma işlemi gösterilmektedir. GRPC hizmeti projesi oluşturmak, proto dosyasını düzenleyin ve bir çift yönlü çağrı akış ekleme hakkında bilgi edinin.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 06/12/2019
uid: tutorials/grpc/grpc-start
ms.openlocfilehash: 76c40d108bc0ce9dec7099b10812c896f0b7777d
ms.sourcegitcommit: f5762967df3be8b8c868229e679301f2f7954679
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67048205"
---
# <a name="tutorial-create-a-grpc-client-and-server-in-aspnet-core"></a><span data-ttu-id="0874c-104">Öğretici: ASP.NET Core gRPC istemci ve sunucu oluşturma</span><span class="sxs-lookup"><span data-stu-id="0874c-104">Tutorial: Create a gRPC client and server in ASP.NET Core</span></span>

<span data-ttu-id="0874c-105">Tarafından [John Luo](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="0874c-105">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="0874c-106">Bu öğreticide, .NET Core oluşturma işlemi gösterilmektedir [gRPC](https://grpc.io/docs/guides/) istemci ve sunucu bir ASP.NET Core gRPC.</span><span class="sxs-lookup"><span data-stu-id="0874c-106">This tutorial shows how to create a .NET Core [gRPC](https://grpc.io/docs/guides/) client and an ASP.NET Core gRPC Server.</span></span>

<span data-ttu-id="0874c-107">Sonunda, gRPC Greeter hizmet ile iletişim kuran gRPC istemci gerekir.</span><span class="sxs-lookup"><span data-stu-id="0874c-107">At the end, you'll have a gRPC client that communicates with the gRPC Greeter service.</span></span>

<span data-ttu-id="0874c-108">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="0874c-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="0874c-109">Bu öğreticide şunları yaptınız:</span><span class="sxs-lookup"><span data-stu-id="0874c-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="0874c-110">GRPC bir sunucu oluşturun.</span><span class="sxs-lookup"><span data-stu-id="0874c-110">Create a gRPC Server.</span></span>
> * <span data-ttu-id="0874c-111">GRPC istemci oluşturun.</span><span class="sxs-lookup"><span data-stu-id="0874c-111">Create a gRPC client.</span></span>
> * <span data-ttu-id="0874c-112">GRPC Greeter hizmeti ile gRPC İstemci hizmetini test edin.</span><span class="sxs-lookup"><span data-stu-id="0874c-112">Test the gRPC client service with the gRPC Greeter service.</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-all-3.0.md)]

## <a name="create-a-grpc-service"></a><span data-ttu-id="0874c-113">GRPC hizmeti oluşturma</span><span class="sxs-lookup"><span data-stu-id="0874c-113">Create a gRPC service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0874c-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0874c-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="0874c-115">Visual Studio'dan **dosya** menüsünde **yeni** > **proje**.</span><span class="sxs-lookup"><span data-stu-id="0874c-115">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="0874c-116">İçinde **yeni bir proje oluşturma** iletişim kutusunda **ASP.NET Core Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="0874c-116">In the **Create a new project** dialog, select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="0874c-117">Seçin **İleri**</span><span class="sxs-lookup"><span data-stu-id="0874c-117">Select **Next**</span></span>
* <span data-ttu-id="0874c-118">Projeyi adlandırın **GrpcGreeter**.</span><span class="sxs-lookup"><span data-stu-id="0874c-118">Name the project **GrpcGreeter**.</span></span> <span data-ttu-id="0874c-119">Projeyi adlandırın önemlidir *GrpcGreeter* kodu kopyalayıp, ad alanlarını eşleşecek şekilde.</span><span class="sxs-lookup"><span data-stu-id="0874c-119">It's important to name the project *GrpcGreeter* so the namespaces will match when you copy and paste code.</span></span>
* <span data-ttu-id="0874c-120">**Oluştur**’u seçin</span><span class="sxs-lookup"><span data-stu-id="0874c-120">Select **Create**</span></span>
* <span data-ttu-id="0874c-121">İçinde **yeni bir ASP.NET Core Web uygulaması oluşturma** iletişim:</span><span class="sxs-lookup"><span data-stu-id="0874c-121">In the **Create a new ASP.NET Core Web Application** dialog:</span></span>
  * <span data-ttu-id="0874c-122">Seçin **.NET Core** ve **ASP.NET Core 3.0** açılan menüler içinde.</span><span class="sxs-lookup"><span data-stu-id="0874c-122">Select **.NET Core** and **ASP.NET Core 3.0** in the dropdown menus.</span></span> 
  * <span data-ttu-id="0874c-123">Seçin **gRPC hizmet** şablonu.</span><span class="sxs-lookup"><span data-stu-id="0874c-123">Select the **gRPC Service** template.</span></span>
  * <span data-ttu-id="0874c-124">**Oluştur**’u seçin</span><span class="sxs-lookup"><span data-stu-id="0874c-124">Select **Create**</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0874c-125">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0874c-125">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="0874c-126">Açık [tümleşik Terminalini](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="0874c-126">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="0874c-127">Dizinleri (`cd`) proje içeren bir klasör.</span><span class="sxs-lookup"><span data-stu-id="0874c-127">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="0874c-128">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="0874c-128">Run the following commands:</span></span>

  ```console
  dotnet new grpc -o GrpcGreeter
  code -r GrpcGreeter
  ```

  * <span data-ttu-id="0874c-129">`dotnet new` Komut yeni bir gRPC hizmetinde oluşturur *GrpcGreeter* klasör.</span><span class="sxs-lookup"><span data-stu-id="0874c-129">The `dotnet new` command creates a new gRPC service in the *GrpcGreeter* folder.</span></span>
  * <span data-ttu-id="0874c-130">`code` Komutu açılır *GrpcGreeter* Visual Studio Code yeni bir örneğini klasöründe.</span><span class="sxs-lookup"><span data-stu-id="0874c-130">The `code` command opens the *GrpcGreeter* folder in a new instance of Visual Studio Code.</span></span>

  <span data-ttu-id="0874c-131">Bir iletişim kutusu görünür **gerekli varlıkları oluşturun ve hata ayıklama 'GrpcGreeter' eksik. Bunları eklensin mi?**</span><span class="sxs-lookup"><span data-stu-id="0874c-131">A dialog box appears with **Required assets to build and debug are missing from 'GrpcGreeter'. Add them?**</span></span>
* <span data-ttu-id="0874c-132">Seçin **Evet**</span><span class="sxs-lookup"><span data-stu-id="0874c-132">Select **Yes**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="0874c-133">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0874c-133">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="0874c-134">Bir terminalde aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="0874c-134">From a terminal, run the following commands:</span></span>

```console
  dotnet new grpc -o GrpcGreeter
  cd GrpcGreeter
```

<span data-ttu-id="0874c-135">Kullanım komutları önceki [.NET Core CLI](/dotnet/core/tools/dotnet) gRPC hizmet oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="0874c-135">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a gRPC service.</span></span>

### <a name="open-the-project"></a><span data-ttu-id="0874c-136">Projeyi açın</span><span class="sxs-lookup"><span data-stu-id="0874c-136">Open the project</span></span>

<span data-ttu-id="0874c-137">Visual Studio'dan seçin **Dosya > Aç**ve ardından *GrpcGreeter.sln* dosya.</span><span class="sxs-lookup"><span data-stu-id="0874c-137">From Visual Studio, select **File > Open**, and then select the *GrpcGreeter.sln* file.</span></span>

<!-- End of VS tabs -->

---

### <a name="run-the-service"></a><span data-ttu-id="0874c-138">Hizmet çalıştırma</span><span class="sxs-lookup"><span data-stu-id="0874c-138">Run the service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0874c-139">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0874c-139">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="0874c-140">Tuşuna `Ctrl+F5` hata ayıklayıcı olmadan gRPC hizmeti çalıştırmak için.</span><span class="sxs-lookup"><span data-stu-id="0874c-140">Press `Ctrl+F5` to run the gRPC service without the debugger.</span></span>

  <span data-ttu-id="0874c-141">Visual Studio hizmeti, bir komut istemi'nde çalışır.</span><span class="sxs-lookup"><span data-stu-id="0874c-141">Visual Studio runs the service in a command prompt.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="0874c-142">Visual Studio Code'u / Visual Studio Mac için</span><span class="sxs-lookup"><span data-stu-id="0874c-142">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="0874c-143">GRPC Greeter projeyi Çalıştır *GrpcGreeter* kullanarak komut satırından `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="0874c-143">Run the gRPC Greeter project *GrpcGreeter* from the command line using `dotnet run`.</span></span>

<!-- End of combined VS/Mac tabs -->

---

<span data-ttu-id="0874c-144">Üzerinde dinleme hizmeti günlükleri göster `http://localhost:50051`.</span><span class="sxs-lookup"><span data-stu-id="0874c-144">The logs show the service listening on `http://localhost:50051`.</span></span>

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: http://localhost:50051
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
```

### <a name="examine-the-project-files"></a><span data-ttu-id="0874c-145">Proje dosyalarını inceleyin</span><span class="sxs-lookup"><span data-stu-id="0874c-145">Examine the project files</span></span>

<span data-ttu-id="0874c-146">*GrpcGreeter* proje dosyaları:</span><span class="sxs-lookup"><span data-stu-id="0874c-146">*GrpcGreeter* project files:</span></span>

* <span data-ttu-id="0874c-147">*greet.proto*: *Protos/greet.proto* dosyası tanımlar `Greeter` gRPC ve gRPC sunucu varlıkları oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="0874c-147">*greet.proto*: The *Protos/greet.proto* file defines the `Greeter` gRPC and is used to generate the gRPC server assets.</span></span> <span data-ttu-id="0874c-148">Daha fazla bilgi için [gRPC giriş](xref:grpc/index).</span><span class="sxs-lookup"><span data-stu-id="0874c-148">For more information, see [Introduction to gRPC](xref:grpc/index).</span></span>
* <span data-ttu-id="0874c-149">*Hizmetleri* klasörü: Uygulamasını içerir `Greeter` hizmeti.</span><span class="sxs-lookup"><span data-stu-id="0874c-149">*Services* folder: Contains the implementation of the `Greeter` service.</span></span>
* <span data-ttu-id="0874c-150">*appSettings.json*: Kestrel'i tarafından kullanılan Protokolü gibi yapılandırma verilerini içerir.</span><span class="sxs-lookup"><span data-stu-id="0874c-150">*appSettings.json*: Contains configuration data, such as protocol used by Kestrel.</span></span> <span data-ttu-id="0874c-151">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="0874c-151">For more information, see <xref:fundamentals/configuration/index>.</span></span>
* <span data-ttu-id="0874c-152">*Program.cs*: GRPC hizmeti için giriş noktası içerir.</span><span class="sxs-lookup"><span data-stu-id="0874c-152">*Program.cs*: Contains the entry point for the gRPC service.</span></span> <span data-ttu-id="0874c-153">Daha fazla bilgi için bkz. <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="0874c-153">For more information, see <xref:fundamentals/host/generic-host>.</span></span>
* <span data-ttu-id="0874c-154">*Startup.cs*: Uygulama davranışını yapılandıran kodunu içerir.</span><span class="sxs-lookup"><span data-stu-id="0874c-154">*Startup.cs*: Contains code that configures app behavior.</span></span> <span data-ttu-id="0874c-155">Daha fazla bilgi için [uygulama başlatma](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="0874c-155">For more information, see [App startup](xref:fundamentals/startup).</span></span>

## <a name="create-the-grpc-client-in-a-net-console-app"></a><span data-ttu-id="0874c-156">GRPC istemci bir .NET konsol uygulaması oluşturun</span><span class="sxs-lookup"><span data-stu-id="0874c-156">Create the gRPC client in a .NET console app</span></span>

## <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0874c-157">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0874c-157">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="0874c-158">Seçin **dosya** > **yeni** > **proje** menü çubuğundan.</span><span class="sxs-lookup"><span data-stu-id="0874c-158">Select **File** > **New** > **Project** from the menu bar.</span></span>
* <span data-ttu-id="0874c-159">İçinde **yeni bir proje oluşturma** iletişim kutusunda **konsol uygulaması (.NET Core)** .</span><span class="sxs-lookup"><span data-stu-id="0874c-159">In the **Create a new project** dialog, select **Console App (.NET Core)**.</span></span>
* <span data-ttu-id="0874c-160">Seçin **İleri**</span><span class="sxs-lookup"><span data-stu-id="0874c-160">Select **Next**</span></span>
* <span data-ttu-id="0874c-161">İçinde **adı** metin kutusunda, "GrpcGreeterClient" girin.</span><span class="sxs-lookup"><span data-stu-id="0874c-161">In the **Name** text box, enter "GrpcGreeterClient".</span></span>
* <span data-ttu-id="0874c-162">**Oluştur**’u seçin.</span><span class="sxs-lookup"><span data-stu-id="0874c-162">Select **Create**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0874c-163">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0874c-163">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="0874c-164">Açık [tümleşik Terminalini](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="0874c-164">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="0874c-165">Dizinleri (`cd`) proje içeren bir klasör.</span><span class="sxs-lookup"><span data-stu-id="0874c-165">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="0874c-166">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="0874c-166">Run the following commands:</span></span>

```console
dotnet new console -o GrpcGreeterClient
code -r GrpcGreeterClient
```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="0874c-167">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0874c-167">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="0874c-168">Yönergeleri izleyerek [burada](/dotnet/core/tutorials/using-on-mac-vs-full-solution) adıyla bir konsol uygulaması oluşturmak için *GrpcGreeterClient*.</span><span class="sxs-lookup"><span data-stu-id="0874c-168">Follow the instructions [here](/dotnet/core/tutorials/using-on-mac-vs-full-solution) to create a console app with the name *GrpcGreeterClient*.</span></span>

<!-- End of VS tabs -->

---

### <a name="add-required-packages"></a><span data-ttu-id="0874c-169">Gerekli paketleri Ekle</span><span class="sxs-lookup"><span data-stu-id="0874c-169">Add required packages</span></span>

<span data-ttu-id="0874c-170">Aşağıdaki paketler gRPC istemci projeye ekleyin:</span><span class="sxs-lookup"><span data-stu-id="0874c-170">Add the following packages to the gRPC client project:</span></span>

* <span data-ttu-id="0874c-171">[Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client), .NET Core istemci içerir.</span><span class="sxs-lookup"><span data-stu-id="0874c-171">[Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client), which contains the .NET Core client.</span></span>
* <span data-ttu-id="0874c-172">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), protobuf içeren API'leri için ileti C#.</span><span class="sxs-lookup"><span data-stu-id="0874c-172">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), which contains protobuf message APIs for C#.</span></span>
* <span data-ttu-id="0874c-173">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), içeren C# protobuf dosyaları için araç desteği.</span><span class="sxs-lookup"><span data-stu-id="0874c-173">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), which contains C# tooling support for protobuf files.</span></span> <span data-ttu-id="0874c-174">Bağımlılık ile işaretlenmiş şekilde araçları paket çalışma zamanında gerekli değil `PrivateAssets="All"`.</span><span class="sxs-lookup"><span data-stu-id="0874c-174">The tooling package isn't required at runtime, so the dependency is marked with `PrivateAssets="All"`.</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0874c-175">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0874c-175">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="0874c-176">Paket Yöneticisi Konsolu (PMC) veya NuGet paketlerini Yönet'i kullanarak paketleri yükleyin.</span><span class="sxs-lookup"><span data-stu-id="0874c-176">Install the packages using either the Package Manager Console (PMC) or Manage NuGet Packages.</span></span>

#### <a name="pmc-option-to-install-packages"></a><span data-ttu-id="0874c-177">PMC seçeneği paketleri yüklemek için</span><span class="sxs-lookup"><span data-stu-id="0874c-177">PMC option to install packages</span></span>

* <span data-ttu-id="0874c-178">Visual Studio'dan seçin **Araçları** > **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu**</span><span class="sxs-lookup"><span data-stu-id="0874c-178">From Visual Studio, select **Tools** > **NuGet Package Manager** > **Package Manager Console**</span></span>
* <span data-ttu-id="0874c-179">Gelen **Paket Yöneticisi Konsolu** penceresinde olduğu dizine gidin *GrpcGreeterClient.csproj* dosya yok.</span><span class="sxs-lookup"><span data-stu-id="0874c-179">From the **Package Manager Console** window, navigate to the directory in which the *GrpcGreeterClient.csproj* file exists.</span></span>
* <span data-ttu-id="0874c-180">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="0874c-180">Run the following commands:</span></span>

 ```powershell
Install-Package Grpc.Net.Client
Install-Package Google.Protobuf
Install-Package Grpc.Tools
```

#### <a name="manage-nuget-packages-option-to-install-packages"></a><span data-ttu-id="0874c-181">Paketleri yüklemek için seçeneği NuGet paketlerini Yönet</span><span class="sxs-lookup"><span data-stu-id="0874c-181">Manage NuGet Packages option to install packages</span></span>

* <span data-ttu-id="0874c-182">Projeye sağ **Çözüm Gezgini** > **NuGet paketlerini Yönet**</span><span class="sxs-lookup"><span data-stu-id="0874c-182">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
* <span data-ttu-id="0874c-183">Seçin **Gözat** sekmesi.</span><span class="sxs-lookup"><span data-stu-id="0874c-183">Select the **Browse** tab.</span></span>
* <span data-ttu-id="0874c-184">Girin **Grpc.Core** arama kutusuna.</span><span class="sxs-lookup"><span data-stu-id="0874c-184">Enter **Grpc.Core** in the search box.</span></span>
* <span data-ttu-id="0874c-185">Seçin **Grpc.Core** gelen paket **Gözat** sekmenize **yükleme**.</span><span class="sxs-lookup"><span data-stu-id="0874c-185">Select the **Grpc.Core** package from the **Browse** tab and select **Install**.</span></span>
* <span data-ttu-id="0874c-186">Yineleme için `Google.Protobuf` ve `Grpc.Tools`.</span><span class="sxs-lookup"><span data-stu-id="0874c-186">Repeat for `Google.Protobuf` and `Grpc.Tools`.</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0874c-187">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0874c-187">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="0874c-188">Aşağıdaki komutları çalıştırın **tümleşik Terminalini**:</span><span class="sxs-lookup"><span data-stu-id="0874c-188">Run the following commands from the **Integrated Terminal**:</span></span>

```console
dotnet add GrpcGreeterClient.csproj package Grpc.Net.Client
dotnet add GrpcGreeterClient.csproj package Google.Protobuf
dotnet add GrpcGreeterClient.csproj package Grpc.Tools
```

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="0874c-189">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0874c-189">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="0874c-190">Sağ **paketleri** klasöründe **çözüm bölmesi** > **paketleri Ekle**</span><span class="sxs-lookup"><span data-stu-id="0874c-190">Right-click the **Packages** folder in **Solution Pad** > **Add Packages**</span></span>
* <span data-ttu-id="0874c-191">Girin **Grpc.Net.Client** arama kutusuna.</span><span class="sxs-lookup"><span data-stu-id="0874c-191">Enter **Grpc.Net.Client** in the search box.</span></span>
* <span data-ttu-id="0874c-192">Seçin **Grpc.Net.Client** seçin ve sonuçlar bölmesinden paketini **' paket Ekle**</span><span class="sxs-lookup"><span data-stu-id="0874c-192">Select the **Grpc.Net.Client** package from the results pane and select **Add Package**</span></span>
* <span data-ttu-id="0874c-193">Yineleme için `Google.Protobuf` ve `Grpc.Tools`.</span><span class="sxs-lookup"><span data-stu-id="0874c-193">Repeat for `Google.Protobuf` and `Grpc.Tools`.</span></span>

---

### <a name="add-greetproto"></a><span data-ttu-id="0874c-194">Greet.proto ekleyin</span><span class="sxs-lookup"><span data-stu-id="0874c-194">Add greet.proto</span></span>

* <span data-ttu-id="0874c-195">Oluşturma bir **Protos** gRPC istemci proje klasöründe.</span><span class="sxs-lookup"><span data-stu-id="0874c-195">Create a **Protos** folder in the gRPC client project.</span></span>
* <span data-ttu-id="0874c-196">Kopyalama **Protos\greet.proto** Greeter hizmet gRPC dosyasına gRPC istemci projesi.</span><span class="sxs-lookup"><span data-stu-id="0874c-196">Copy the **Protos\greet.proto** file from the gRPC Greeter service to the gRPC client project.</span></span>
* <span data-ttu-id="0874c-197">Düzen *GrpcGreeterClient.csproj* proje dosyası:</span><span class="sxs-lookup"><span data-stu-id="0874c-197">Edit the *GrpcGreeterClient.csproj* project file:</span></span>

  # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0874c-198">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0874c-198">Visual Studio</span></span>](#tab/visual-studio) 

  <span data-ttu-id="0874c-199">Projeye sağ tıklayıp **Düzenle GrpcGreeterClient.csproj**.</span><span class="sxs-lookup"><span data-stu-id="0874c-199">Right-click the project and select the **Edit GrpcGreeterClient.csproj**.</span></span>

  # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0874c-200">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0874c-200">Visual Studio Code</span></span>](#tab/visual-studio-code) 

  <span data-ttu-id="0874c-201">Seçin *GrpcGreeterClient.csproj* dosya.</span><span class="sxs-lookup"><span data-stu-id="0874c-201">Select the *GrpcGreeterClient.csproj* file.</span></span>

  # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="0874c-202">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0874c-202">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  <span data-ttu-id="0874c-203">Projeye sağ tıklayıp **Araçlar > Dosya Düzenle**.</span><span class="sxs-lookup"><span data-stu-id="0874c-203">Right-click the project and select **Tools > Edit File**.</span></span>

  ---

* <span data-ttu-id="0874c-204">Ekleme **greet.proto** dosyasını `<Protobuf>` GrpcGreeterClient proje dosyasının öğesi grubu:</span><span class="sxs-lookup"><span data-stu-id="0874c-204">Add the **greet.proto** file to the `<Protobuf>` item group of the GrpcGreeterClient project file:</span></span>

  ```XML
  <ItemGroup>
    <Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
  </ItemGroup>
  ```

<span data-ttu-id="0874c-205">Nesil tetiklemek için istemci projesinin oluşturun C# istemci varlıklar.</span><span class="sxs-lookup"><span data-stu-id="0874c-205">Build the client project to trigger the generation of the C# client assets.</span></span>

### <a name="create-the-greeter-client"></a><span data-ttu-id="0874c-206">Greeter istemcisi oluşturma</span><span class="sxs-lookup"><span data-stu-id="0874c-206">Create the Greeter client</span></span>

<span data-ttu-id="0874c-207">Türlerini oluşturmak için projeyi derleyin **Greeter** ad alanı.</span><span class="sxs-lookup"><span data-stu-id="0874c-207">Build the project to create the types in the **Greeter** namespace.</span></span> <span data-ttu-id="0874c-208">`Greeter` Türleri yapı işlemi tarafından otomatik olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="0874c-208">The `Greeter` types are generated automatically by the build process.</span></span>

<span data-ttu-id="0874c-209">GRPC istemci güncelleştirmesi *Program.cs* dosyasındaki kodu aşağıdaki kodla:</span><span class="sxs-lookup"><span data-stu-id="0874c-209">Update the gRPC client *Program.cs* file with the following code:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet2)]

<span data-ttu-id="0874c-210">*Program.cs* gRPC istemci için giriş noktası ve mantığı içerir.</span><span class="sxs-lookup"><span data-stu-id="0874c-210">*Program.cs* contains the entry point and logic for the gRPC client.</span></span>

<span data-ttu-id="0874c-211">Greeter istemci tarafından oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="0874c-211">The Greeter client is created by:</span></span>

* <span data-ttu-id="0874c-212">Örnekleme bir `HttpClient` gRPC hizmeti bağlantısı oluşturma hakkında bilgi içeren.</span><span class="sxs-lookup"><span data-stu-id="0874c-212">Instantiating an `HttpClient` containing the information for creating the connection to the gRPC service.</span></span>
* <span data-ttu-id="0874c-213">Kullanarak `HttpClient` Greeter istemci oluşturmak için:</span><span class="sxs-lookup"><span data-stu-id="0874c-213">Using the `HttpClient` to construct the Greeter client:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=3-9)]

<span data-ttu-id="0874c-214">Greeter istemci zaman uyumsuz olarak çağırır `SayHello` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="0874c-214">The Greeter client calls the asynchronous `SayHello` method.</span></span> <span data-ttu-id="0874c-215">Sonucu `SayHello` çağrı görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="0874c-215">The result of the `SayHello` call is displayed:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=10-12)]

## <a name="test-the-grpc-client-with-the-grpc-greeter-service"></a><span data-ttu-id="0874c-216">GRPC Greeter hizmet gRPC istemciyle test etme</span><span class="sxs-lookup"><span data-stu-id="0874c-216">Test the gRPC client with the gRPC Greeter service</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0874c-217">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0874c-217">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="0874c-218">Greeter hizmetinde basın `Ctrl+F5` hata ayıklayıcı olmadan sunucu başlatma.</span><span class="sxs-lookup"><span data-stu-id="0874c-218">In the Greeter service, press `Ctrl+F5` to start the server without the debugger.</span></span>
* <span data-ttu-id="0874c-219">İçinde `GrpcGreeterClient` tuşuna basın, proje `Ctrl+F5` hata ayıklayıcı olmadan sunucu başlatma.</span><span class="sxs-lookup"><span data-stu-id="0874c-219">In the `GrpcGreeterClient` project, press `Ctrl+F5` to start the server without the debugger.</span></span>

### <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="0874c-220">Visual Studio Code'u / Visual Studio Mac için</span><span class="sxs-lookup"><span data-stu-id="0874c-220">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="0874c-221">Greeter hizmetini başlatın.</span><span class="sxs-lookup"><span data-stu-id="0874c-221">Start the Greeter service.</span></span>
* <span data-ttu-id="0874c-222">İstemcisini başlatın.</span><span class="sxs-lookup"><span data-stu-id="0874c-222">Start the client.</span></span>

<!-- End of combined VS/Mac tabs -->

---

<span data-ttu-id="0874c-223">İstemci bir karşılama "GreeterClient" adını içeren bir iletiyle hizmetine gönderir.</span><span class="sxs-lookup"><span data-stu-id="0874c-223">The client sends a greeting to the service with a message containing its name "GreeterClient".</span></span> <span data-ttu-id="0874c-224">Hizmet yanıt olarak "Hello GreeterClient" iletisi gönderir.</span><span class="sxs-lookup"><span data-stu-id="0874c-224">The service sends the message "Hello GreeterClient" as a response.</span></span> <span data-ttu-id="0874c-225">"Hello GreeterClient" yanıt komut isteminde görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="0874c-225">The "Hello GreeterClient" response is displayed in the command prompt:</span></span>

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

<span data-ttu-id="0874c-226">GRPC hizmeti, komut istemine yazılan günlüklerinde başarılı görüşmesinin ayrıntılarını kaydeder.</span><span class="sxs-lookup"><span data-stu-id="0874c-226">The gRPC service records the details of the successful call in the logs written to the command prompt.</span></span>

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

### <a name="next-steps"></a><span data-ttu-id="0874c-227">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="0874c-227">Next steps</span></span>

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
