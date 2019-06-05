---
title: ASP.NET Core .NET Core gRPC istemci ve sunucu oluşturma
author: juntaoluo
description: Bu öğreticide, ASP.NET Core, hizmet ve gRPC gRPC istemci oluşturma işlemi gösterilmektedir. GRPC hizmeti projesi oluşturmak, proto dosyasını düzenleyin ve bir çift yönlü çağrı akış ekleme hakkında bilgi edinin.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 06/05/2019
uid: tutorials/grpc/grpc-start
ms.openlocfilehash: 71e3321819eb7169f0896abe3e07849f59ea6fc7
ms.sourcegitcommit: 5dd2ce9709c9e41142771e652d1a4bd0b5248cec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2019
ms.locfileid: "66692524"
---
# <a name="tutorial-create-a-grpc-client-and-server-in-aspnet-core"></a><span data-ttu-id="e3e19-104">Öğretici: ASP.NET Core gRPC istemci ve sunucu oluşturma</span><span class="sxs-lookup"><span data-stu-id="e3e19-104">Tutorial: Create a gRPC client and server in ASP.NET Core</span></span>

<span data-ttu-id="e3e19-105">Tarafından [John Luo](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="e3e19-105">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="e3e19-106">Bu öğreticide, .NET Core oluşturma işlemi gösterilmektedir [gRPC](https://grpc.io/docs/guides/) istemci ve sunucu bir ASP.NET Core gRPC.</span><span class="sxs-lookup"><span data-stu-id="e3e19-106">This tutorial shows how to create a .NET Core [gRPC](https://grpc.io/docs/guides/) client and an ASP.NET Core gRPC Server.</span></span>

<span data-ttu-id="e3e19-107">Sonunda, gRPC Greeter hizmet ile iletişim kuran gRPC istemci gerekir.</span><span class="sxs-lookup"><span data-stu-id="e3e19-107">At the end, you'll have a gRPC client that communicates with the gRPC Greeter service.</span></span>

<span data-ttu-id="e3e19-108">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="e3e19-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="e3e19-109">Bu öğreticide şunları yaptınız:</span><span class="sxs-lookup"><span data-stu-id="e3e19-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="e3e19-110">GRPC bir sunucu oluşturun.</span><span class="sxs-lookup"><span data-stu-id="e3e19-110">Create a gRPC Server.</span></span>
> * <span data-ttu-id="e3e19-111">GRPC istemci oluşturun.</span><span class="sxs-lookup"><span data-stu-id="e3e19-111">Create a gRPC client.</span></span>
> * <span data-ttu-id="e3e19-112">GRPC Greeter hizmeti ile gRPC İstemci hizmetini test edin.</span><span class="sxs-lookup"><span data-stu-id="e3e19-112">Test the gRPC client service with the gRPC Greeter service.</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-all-3.0.md)]

## <a name="create-a-grpc-service"></a><span data-ttu-id="e3e19-113">GRPC hizmeti oluşturma</span><span class="sxs-lookup"><span data-stu-id="e3e19-113">Create a gRPC service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e3e19-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e3e19-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e3e19-115">Visual Studio'dan **dosya** menüsünde **yeni** > **proje**.</span><span class="sxs-lookup"><span data-stu-id="e3e19-115">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="e3e19-116">İçinde **yeni bir proje oluşturma** iletişim kutusunda **ASP.NET Core Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="e3e19-116">In the **Create a new project** dialog, select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="e3e19-117">Seçin **İleri**</span><span class="sxs-lookup"><span data-stu-id="e3e19-117">Select **Next**</span></span>
* <span data-ttu-id="e3e19-118">Projeyi adlandırın **GrpcGreeter**.</span><span class="sxs-lookup"><span data-stu-id="e3e19-118">Name the project **GrpcGreeter**.</span></span> <span data-ttu-id="e3e19-119">Projeyi adlandırın önemlidir *GrpcGreeter* kodu kopyalayıp, ad alanlarını eşleşecek şekilde.</span><span class="sxs-lookup"><span data-stu-id="e3e19-119">It's important to name the project *GrpcGreeter* so the namespaces will match when you copy and paste code.</span></span>
* <span data-ttu-id="e3e19-120">**Oluştur**’u seçin</span><span class="sxs-lookup"><span data-stu-id="e3e19-120">Select **Create**</span></span>
* <span data-ttu-id="e3e19-121">İçinde **yeni bir ASP.NET Core Web uygulaması oluşturma** iletişim:</span><span class="sxs-lookup"><span data-stu-id="e3e19-121">In the **Create a new ASP.NET Core Web Application** dialog:</span></span>
  * <span data-ttu-id="e3e19-122">Seçin **.NET Core** ve **ASP.NET Core 3.0** açılan menüler içinde.</span><span class="sxs-lookup"><span data-stu-id="e3e19-122">Select **.NET Core** and **ASP.NET Core 3.0** in the dropdown menus.</span></span> 
  * <span data-ttu-id="e3e19-123">Seçin **gRPC hizmet** şablonu.</span><span class="sxs-lookup"><span data-stu-id="e3e19-123">Select the **gRPC Service** template.</span></span>
  * <span data-ttu-id="e3e19-124">**Oluştur**’u seçin</span><span class="sxs-lookup"><span data-stu-id="e3e19-124">Select **Create**</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e3e19-125">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e3e19-125">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="e3e19-126">Açık [tümleşik Terminalini](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="e3e19-126">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="e3e19-127">Dizinleri (`cd`) proje içeren bir klasör.</span><span class="sxs-lookup"><span data-stu-id="e3e19-127">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="e3e19-128">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="e3e19-128">Run the following commands:</span></span>

  ```console
  dotnet new grpc -o GrpcGreeter
  code -r GrpcGreeter
  ```

  * <span data-ttu-id="e3e19-129">`dotnet new` Komut yeni bir gRPC hizmetinde oluşturur *GrpcGreeter* klasör.</span><span class="sxs-lookup"><span data-stu-id="e3e19-129">The `dotnet new` command creates a new gRPC service in the *GrpcGreeter* folder.</span></span>
  * <span data-ttu-id="e3e19-130">`code` Komutu açılır *GrpcGreeter* Visual Studio Code yeni bir örneğini klasöründe.</span><span class="sxs-lookup"><span data-stu-id="e3e19-130">The `code` command opens the *GrpcGreeter* folder in a new instance of Visual Studio Code.</span></span>

  <span data-ttu-id="e3e19-131">Bir iletişim kutusu görünür **gerekli varlıkları oluşturun ve hata ayıklama 'GrpcGreeter' eksik. Bunları eklensin mi?**</span><span class="sxs-lookup"><span data-stu-id="e3e19-131">A dialog box appears with **Required assets to build and debug are missing from 'GrpcGreeter'. Add them?**</span></span>
* <span data-ttu-id="e3e19-132">Seçin **Evet**</span><span class="sxs-lookup"><span data-stu-id="e3e19-132">Select **Yes**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e3e19-133">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e3e19-133">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="e3e19-134">Bir terminalde aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="e3e19-134">From a terminal, run the following commands:</span></span>

```console
  dotnet new grpc -o GrpcGreeter
  cd GrpcGreeter
```

<span data-ttu-id="e3e19-135">Kullanım komutları önceki [.NET Core CLI](/dotnet/core/tools/dotnet) gRPC hizmet oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="e3e19-135">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a gRPC service.</span></span>

### <a name="open-the-project"></a><span data-ttu-id="e3e19-136">Projeyi açın</span><span class="sxs-lookup"><span data-stu-id="e3e19-136">Open the project</span></span>

<span data-ttu-id="e3e19-137">Visual Studio'dan seçin **Dosya > Aç**ve ardından *GrpcGreeter.sln* dosya.</span><span class="sxs-lookup"><span data-stu-id="e3e19-137">From Visual Studio, select **File > Open**, and then select the *GrpcGreeter.sln* file.</span></span>

<!-- End of VS tabs -->

---

### <a name="run-the-service"></a><span data-ttu-id="e3e19-138">Hizmet çalıştırma</span><span class="sxs-lookup"><span data-stu-id="e3e19-138">Run the service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e3e19-139">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e3e19-139">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e3e19-140">Hata Ayıklayıcı olmadan gRPC hizmeti çalıştırmak için CTRL + F5 tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="e3e19-140">Press Ctrl+F5 to run the gRPC service without the debugger.</span></span>

  <span data-ttu-id="e3e19-141">Visual Studio hizmeti, bir komut istemi'nde çalışır.</span><span class="sxs-lookup"><span data-stu-id="e3e19-141">Visual Studio runs the service in a command prompt.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="e3e19-142">Visual Studio Code'u / Visual Studio Mac için</span><span class="sxs-lookup"><span data-stu-id="e3e19-142">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="e3e19-143">GRPC Greeter projeyi GrpcGreeter kullanılarak komut satırından çalıştırmak `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="e3e19-143">Run the gRPC Greeter project GrpcGreeter from the command line using `dotnet run`.</span></span>

<!-- End of combined VS/Mac tabs -->

---

<span data-ttu-id="e3e19-144">Üzerinde dinleme hizmeti günlükleri göster `http://localhost:50051`.</span><span class="sxs-lookup"><span data-stu-id="e3e19-144">The logs show the service listening on `http://localhost:50051`.</span></span>

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: http://localhost:50051
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
```

### <a name="examine-the-project-files"></a><span data-ttu-id="e3e19-145">Proje dosyalarını inceleyin</span><span class="sxs-lookup"><span data-stu-id="e3e19-145">Examine the project files</span></span>

<span data-ttu-id="e3e19-146">GrpcGreeter dosyaları:</span><span class="sxs-lookup"><span data-stu-id="e3e19-146">GrpcGreeter files:</span></span>

* <span data-ttu-id="e3e19-147">*greet.proto*: *Protos/greet.proto* dosyası tanımlar `Greeter` gRPC ve gRPC sunucu varlıkları oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="e3e19-147">*greet.proto*: The *Protos/greet.proto* file defines the `Greeter` gRPC and is used to generate the gRPC server assets.</span></span> <span data-ttu-id="e3e19-148">Daha fazla bilgi için [gRPC giriş](xref:grpc/index).</span><span class="sxs-lookup"><span data-stu-id="e3e19-148">For more information, see [Introduction to gRPC](xref:grpc/index).</span></span>
* <span data-ttu-id="e3e19-149">*Hizmetleri* klasörü: Uygulamasını içerir `Greeter` hizmeti.</span><span class="sxs-lookup"><span data-stu-id="e3e19-149">*Services* folder: Contains the implementation of the `Greeter` service.</span></span>
* <span data-ttu-id="e3e19-150">*appSettings.json*: Kestrel'i tarafından kullanılan Protokolü gibi yapılandırma verilerini içerir.</span><span class="sxs-lookup"><span data-stu-id="e3e19-150">*appSettings.json*: Contains configuration data, such as protocol used by Kestrel.</span></span> <span data-ttu-id="e3e19-151">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="e3e19-151">For more information, see <xref:fundamentals/configuration/index>.</span></span>
* <span data-ttu-id="e3e19-152">*Program.cs*: GRPC hizmeti için giriş noktası içerir.</span><span class="sxs-lookup"><span data-stu-id="e3e19-152">*Program.cs*: Contains the entry point for the gRPC service.</span></span> <span data-ttu-id="e3e19-153">Daha fazla bilgi için bkz. <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="e3e19-153">For more information, see <xref:fundamentals/host/web-host>.</span></span>
* <span data-ttu-id="e3e19-154">*Startup.cs*: Uygulama davranışını yapılandıran kodunu içerir.</span><span class="sxs-lookup"><span data-stu-id="e3e19-154">*Startup.cs*: Contains code that configures app behavior.</span></span> <span data-ttu-id="e3e19-155">Daha fazla bilgi için [uygulama başlatma](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="e3e19-155">For more information, see [App startup](xref:fundamentals/startup).</span></span>

## <a name="create-the-grpc-client-in-a-net-console-app"></a><span data-ttu-id="e3e19-156">GRPC istemci bir .NET konsol uygulaması oluşturun</span><span class="sxs-lookup"><span data-stu-id="e3e19-156">Create the gRPC client in a .NET console app</span></span>

## <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e3e19-157">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e3e19-157">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e3e19-158">Seçin **dosya** > **yeni** > **proje** menü çubuğundan.</span><span class="sxs-lookup"><span data-stu-id="e3e19-158">Select **File** > **New** > **Project** from the menu bar.</span></span>
* <span data-ttu-id="e3e19-159">İçinde **yeni bir proje oluşturma** iletişim kutusunda **konsol uygulaması (.NET Core)** .</span><span class="sxs-lookup"><span data-stu-id="e3e19-159">In the **Create a new project** dialog, select **Console App (.NET Core)**.</span></span>
* <span data-ttu-id="e3e19-160">Seçin **İleri**</span><span class="sxs-lookup"><span data-stu-id="e3e19-160">Select **Next**</span></span>
* <span data-ttu-id="e3e19-161">İçinde **adı** metin kutusunda, "GrpcGreeterClient" girin.</span><span class="sxs-lookup"><span data-stu-id="e3e19-161">In the **Name** text box, enter "GrpcGreeterClient".</span></span>
* <span data-ttu-id="e3e19-162">**Oluştur**’u seçin.</span><span class="sxs-lookup"><span data-stu-id="e3e19-162">Select **Create**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e3e19-163">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e3e19-163">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="e3e19-164">Açık [tümleşik Terminalini](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="e3e19-164">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="e3e19-165">Dizinleri (`cd`) proje içeren bir klasör.</span><span class="sxs-lookup"><span data-stu-id="e3e19-165">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="e3e19-166">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="e3e19-166">Run the following commands:</span></span>

```console
dotnet new console -o GrpcGreeterClient
code -r GrpcGreeterClient
```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e3e19-167">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e3e19-167">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="e3e19-168">Yönergeleri izleyerek [burada](/dotnet/core/tutorials/using-on-mac-vs-full-solution) adıyla bir konsol uygulaması oluşturmak için *GrpcGreeterClient*.</span><span class="sxs-lookup"><span data-stu-id="e3e19-168">Follow the instructions [here](/dotnet/core/tutorials/using-on-mac-vs-full-solution) to create a console app with the name *GrpcGreeterClient*.</span></span>

<!-- End of VS tabs -->

---

### <a name="add-required-packages"></a><span data-ttu-id="e3e19-169">Gerekli paketleri Ekle</span><span class="sxs-lookup"><span data-stu-id="e3e19-169">Add required packages</span></span>

<span data-ttu-id="e3e19-170">Aşağıdaki paketler gRPC istemci projeye ekleyin:</span><span class="sxs-lookup"><span data-stu-id="e3e19-170">Add the following packages to the gRPC client project:</span></span>

* <span data-ttu-id="e3e19-171">[Grpc.Core](https://www.nuget.org/packages/Grpc.Core), içeren C# C çekirdek istemci API'si.</span><span class="sxs-lookup"><span data-stu-id="e3e19-171">[Grpc.Core](https://www.nuget.org/packages/Grpc.Core), which contains the C# API for the C-core client.</span></span>
* <span data-ttu-id="e3e19-172">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), protobuf içeren API'leri için ileti C#.</span><span class="sxs-lookup"><span data-stu-id="e3e19-172">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), which contains protobuf message APIs for C#.</span></span>
* <span data-ttu-id="e3e19-173">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), içeren C# protobuf dosyaları için araç desteği.</span><span class="sxs-lookup"><span data-stu-id="e3e19-173">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), which contains C# tooling support for protobuf files.</span></span> <span data-ttu-id="e3e19-174">Bağımlılık ile işaretlenmiş şekilde araçları paket çalışma zamanında gerekli değil `PrivateAssets="All"`.</span><span class="sxs-lookup"><span data-stu-id="e3e19-174">The tooling package isn't required at runtime, so the dependency is marked with `PrivateAssets="All"`.</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e3e19-175">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e3e19-175">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="e3e19-176">Paket Yöneticisi Konsolu (PMC) veya NuGet paketlerini Yönet'i kullanarak paketleri yükleyin</span><span class="sxs-lookup"><span data-stu-id="e3e19-176">Install the packages using either the Package Manager Console (PMC) or Manage NuGet Package</span></span>

#### <a name="pmc-option-to-install-packages"></a><span data-ttu-id="e3e19-177">PMC seçeneği paketleri yüklemek için</span><span class="sxs-lookup"><span data-stu-id="e3e19-177">PMC option to install packages</span></span>

* <span data-ttu-id="e3e19-178">Visual Studio'dan seçin **Araçları** > **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu**</span><span class="sxs-lookup"><span data-stu-id="e3e19-178">From Visual Studio, select **Tools** > **NuGet Package Manager** > **Package Manager Console**</span></span>
* <span data-ttu-id="e3e19-179">Gelen **Paket Yöneticisi Konsolu** penceresinde olduğu dizine gidin *GrpcGreeterClient.csproj* dosya yok.</span><span class="sxs-lookup"><span data-stu-id="e3e19-179">From the **Package Manager Console** window, navigate to the directory in which the *GrpcGreeterClient.csproj* file exists.</span></span>
* <span data-ttu-id="e3e19-180">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="e3e19-180">Run the following commands:</span></span>

 ```powershell
Install-Package Grpc.Core
Install-Package Google.Protobuf
Install-Package Grpc.Tools
```

#### <a name="manage-nuget-packages-option-to-install-packages"></a><span data-ttu-id="e3e19-181">Paketleri yüklemek için seçeneği NuGet paketlerini Yönet</span><span class="sxs-lookup"><span data-stu-id="e3e19-181">Manage NuGet Packages option to install packages</span></span>

* <span data-ttu-id="e3e19-182">Projeye sağ **Çözüm Gezgini** > **NuGet paketlerini Yönet**</span><span class="sxs-lookup"><span data-stu-id="e3e19-182">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
* <span data-ttu-id="e3e19-183">Seçin **Gözat** sekmesi.</span><span class="sxs-lookup"><span data-stu-id="e3e19-183">Select the **Browse** tab.</span></span>
* <span data-ttu-id="e3e19-184">Girin **Grpc.Core** arama kutusuna.</span><span class="sxs-lookup"><span data-stu-id="e3e19-184">Enter **Grpc.Core** in the search box.</span></span>
* <span data-ttu-id="e3e19-185">Seçin **Grpc.Core** gelen paket **Gözat** sekmenize **yükleme**.</span><span class="sxs-lookup"><span data-stu-id="e3e19-185">Select the **Grpc.Core** package from the **Browse** tab and select **Install**.</span></span>
* <span data-ttu-id="e3e19-186">Yineleme için `Google.Protobuf` ve `Grpc.Tools`.</span><span class="sxs-lookup"><span data-stu-id="e3e19-186">Repeat for `Google.Protobuf` and `Grpc.Tools`.</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e3e19-187">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e3e19-187">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="e3e19-188">Aşağıdaki komutları çalıştırın **tümleşik Terminalini**:</span><span class="sxs-lookup"><span data-stu-id="e3e19-188">Run the following commands from the **Integrated Terminal**:</span></span>

```console
dotnet add GrpcGreeterClient.csproj package Grpc.Core
dotnet add GrpcGreeterClient.csproj package Google.Protobuf
dotnet add GrpcGreeterClient.csproj package Grpc.Tools
```

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e3e19-189">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e3e19-189">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="e3e19-190">Sağ **paketleri** klasöründe **çözüm bölmesi** > **paketleri Ekle**</span><span class="sxs-lookup"><span data-stu-id="e3e19-190">Right-click the **Packages** folder in **Solution Pad** > **Add Packages**</span></span>
* <span data-ttu-id="e3e19-191">Girin **Grpc.Core** arama kutusuna.</span><span class="sxs-lookup"><span data-stu-id="e3e19-191">Enter **Grpc.Core** in the search box.</span></span>
* <span data-ttu-id="e3e19-192">Seçin **Grpc.Core** seçin ve sonuçlar bölmesinden paketini **' paket Ekle**</span><span class="sxs-lookup"><span data-stu-id="e3e19-192">Select the **Grpc.Core** package from the results pane and select **Add Package**</span></span>
* <span data-ttu-id="e3e19-193">Yineleme için `Google.Protobuf` ve `Grpc.Tools`.</span><span class="sxs-lookup"><span data-stu-id="e3e19-193">Repeat for `Google.Protobuf` and `Grpc.Tools`.</span></span>

---

### <a name="add-greetproto"></a><span data-ttu-id="e3e19-194">Greet.proto ekleyin</span><span class="sxs-lookup"><span data-stu-id="e3e19-194">Add greet.proto</span></span>

* <span data-ttu-id="e3e19-195">Oluşturma bir **Protos** gRPC istemci proje klasöründe.</span><span class="sxs-lookup"><span data-stu-id="e3e19-195">Create a **Protos** folder in the gRPC client project.</span></span>
* <span data-ttu-id="e3e19-196">Kopyalama **Protos\greet.proto** Greeter hizmet gRPC dosyasına gRPC istemci projesi.</span><span class="sxs-lookup"><span data-stu-id="e3e19-196">Copy the **Protos\greet.proto** file from the gRPC Greeter service to the gRPC client project.</span></span>
* <span data-ttu-id="e3e19-197">Düzen *GrpcGreeterClient.csproj* proje dosyası:</span><span class="sxs-lookup"><span data-stu-id="e3e19-197">Edit the *GrpcGreeterClient.csproj* project file:</span></span>

  # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e3e19-198">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e3e19-198">Visual Studio</span></span>](#tab/visual-studio) 

  <span data-ttu-id="e3e19-199">Projeye sağ tıklayıp **Düzenle GrpcGreeterClient.csproj**.</span><span class="sxs-lookup"><span data-stu-id="e3e19-199">Right-click the project and select the **Edit GrpcGreeterClient.csproj**.</span></span>

  # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e3e19-200">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e3e19-200">Visual Studio Code</span></span>](#tab/visual-studio-code) 

  <span data-ttu-id="e3e19-201">Seçin *GrpcGreeterClient.csproj* dosya.</span><span class="sxs-lookup"><span data-stu-id="e3e19-201">Select the *GrpcGreeterClient.csproj* file.</span></span>

  # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e3e19-202">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e3e19-202">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  <span data-ttu-id="e3e19-203">Projeyi sağ tıklatın ve seçin **Araçlar > Dosya Düzenle**.</span><span class="sxs-lookup"><span data-stu-id="e3e19-203">Right click the project and select **Tools > Edit File**.</span></span>

  ---

* <span data-ttu-id="e3e19-204">Ekleme **greet.proto** dosyasını `<Protobuf>` GrpcGreeterClient proje dosyasının öğesi grubu:</span><span class="sxs-lookup"><span data-stu-id="e3e19-204">Add the **greet.proto** file to the `<Protobuf>` item group of the GrpcGreeterClient project file:</span></span>

  ```XML
  <ItemGroup>
    <Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
  </ItemGroup>
  ```

<span data-ttu-id="e3e19-205">Nesil tetiklemek için istemci projesinin oluşturun C# istemci varlıklar.</span><span class="sxs-lookup"><span data-stu-id="e3e19-205">Build the client project to trigger the generation of the C# client assets.</span></span>

### <a name="create-the-greeter-client"></a><span data-ttu-id="e3e19-206">Greeter istemcisi oluşturma</span><span class="sxs-lookup"><span data-stu-id="e3e19-206">Create the Greeter client</span></span>

<span data-ttu-id="e3e19-207">Türlerini oluşturmak için projeyi derleyin **Greeter** ad alanı.</span><span class="sxs-lookup"><span data-stu-id="e3e19-207">Build the project to create the types in the **Greeter** namespace.</span></span> <span data-ttu-id="e3e19-208">`Greeter` Türleri yapı işlemi tarafından otomatik olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="e3e19-208">The `Greeter` types are generated automatically by the build process.</span></span>

<span data-ttu-id="e3e19-209">GRPC istemci güncelleştirmesi *Program.cs* dosyasındaki kodu aşağıdaki kodla:</span><span class="sxs-lookup"><span data-stu-id="e3e19-209">Update the gRPC client *Program.cs* file with the following code:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet2)]

<span data-ttu-id="e3e19-210">*Program.cs* gRPC istemci için giriş noktası ve mantığı içerir.</span><span class="sxs-lookup"><span data-stu-id="e3e19-210">*Program.cs* contains the entry point and logic for the gRPC client.</span></span>

<span data-ttu-id="e3e19-211">Greeter istemci tarafından oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="e3e19-211">The Greeter client is created by:</span></span>

* <span data-ttu-id="e3e19-212">Örnekleme bir `Channel` gRPC hizmeti bağlantısı oluşturma hakkında bilgi içeren.</span><span class="sxs-lookup"><span data-stu-id="e3e19-212">Instantiating a `Channel` containing the information for creating the connection to the gRPC service.</span></span>
* <span data-ttu-id="e3e19-213">Kullanarak `Channel` Greeter istemci oluşturmak için:</span><span class="sxs-lookup"><span data-stu-id="e3e19-213">Using the `Channel` to construct the Greeter client:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=4-6)]

<span data-ttu-id="e3e19-214">Greeter istemci zaman uyumsuz olarak çağırır `SayHello` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="e3e19-214">The Greeter client calls the asynchronous `SayHello` method.</span></span> <span data-ttu-id="e3e19-215">Sonucu `SayHello` çağrı görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="e3e19-215">The result of the `SayHello` call is displayed:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=7-9)]

<span data-ttu-id="e3e19-216">Kapatma `Channel` tüm kaynakları serbest bırakmak için işlemleri tamamladığınızda, istemci tarafından kullanılmış.</span><span class="sxs-lookup"><span data-stu-id="e3e19-216">Shut down the `Channel` used by the client when operations have finished to release all resources.</span></span>

## <a name="test-the-grpc-client-with-the-grpc-greeter-service"></a><span data-ttu-id="e3e19-217">GRPC Greeter hizmet gRPC istemciyle test etme</span><span class="sxs-lookup"><span data-stu-id="e3e19-217">Test the gRPC client with the gRPC Greeter service</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e3e19-218">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e3e19-218">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e3e19-219">Greeter hizmetinde hata ayıklayıcı olmadan sunucu başlatmak için Ctrl + F5 tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="e3e19-219">In the Greeter service, press Ctrl+F5 to start the server without the debugger.</span></span>
* <span data-ttu-id="e3e19-220">GrpcGreeterClient projesinde hata ayıklayıcı olmadan sunucu başlatmak için Ctrl + F5 tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="e3e19-220">In the GrpcGreeterClient project, press Ctrl+F5 to start the server without the debugger.</span></span>

### <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="e3e19-221">Visual Studio Code'u / Visual Studio Mac için</span><span class="sxs-lookup"><span data-stu-id="e3e19-221">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="e3e19-222">Greeter hizmetini başlatın.</span><span class="sxs-lookup"><span data-stu-id="e3e19-222">Start the Greeter service.</span></span>
* <span data-ttu-id="e3e19-223">İstemcisini başlatın.</span><span class="sxs-lookup"><span data-stu-id="e3e19-223">Start the client.</span></span>

<!-- End of combined VS/Mac tabs -->

---

<span data-ttu-id="e3e19-224">İstemci bir karşılama "GreeterClient" adını içeren bir iletiyle hizmetine gönderir.</span><span class="sxs-lookup"><span data-stu-id="e3e19-224">The client sends a greeting to the service with a message containing its name "GreeterClient".</span></span> <span data-ttu-id="e3e19-225">Hizmet yanıt olarak "Hello GreeterClient" iletisi gönderir.</span><span class="sxs-lookup"><span data-stu-id="e3e19-225">The service sends the message "Hello GreeterClient" as a response.</span></span> <span data-ttu-id="e3e19-226">"Hello GreeterClient" yanıt komut isteminde görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="e3e19-226">The "Hello GreeterClient" response is displayed in the command prompt:</span></span>

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

<span data-ttu-id="e3e19-227">GRPC hizmeti, komut istemine yazılan günlüklerinde başarılı görüşmesinin ayrıntılarını kaydeder.</span><span class="sxs-lookup"><span data-stu-id="e3e19-227">The gRPC service records the details of the successful call in the logs written to the command prompt.</span></span>

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

### <a name="next-steps"></a><span data-ttu-id="e3e19-228">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="e3e19-228">Next steps</span></span>

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
