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
# <a name="tutorial-create-a-net-core-grpc-client"></a><span data-ttu-id="3125e-104">Öğretici: Bir .NET Core gRPC istemcisi oluşturma</span><span class="sxs-lookup"><span data-stu-id="3125e-104">Tutorial: Create a .NET Core gRPC client</span></span>

<span data-ttu-id="3125e-105">Tarafından [John Luo](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="3125e-105">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="3125e-106">Bu öğreticide, .NET Core oluşturma işlemi gösterilmektedir [gRPC](https://grpc.io/docs/guides/) gRPC Hizmetleri ile iletişim kurabilen istemci.</span><span class="sxs-lookup"><span data-stu-id="3125e-106">This tutorial shows how to create a .NET Core [gRPC](https://grpc.io/docs/guides/) client that can communicate with gRPC services.</span></span>

<span data-ttu-id="3125e-107">Sonunda, gRPC Greeter hizmet ile iletişim kuran gRPC istemci gerekir.</span><span class="sxs-lookup"><span data-stu-id="3125e-107">At the end, you'll have a gRPC client that communicates with the gRPC Greeter service.</span></span>

[!INCLUDE[View or download sample code](~/includes/grpc/downloadClient.md)]

<span data-ttu-id="3125e-108">Bu öğreticide şunları yaptınız:</span><span class="sxs-lookup"><span data-stu-id="3125e-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="3125e-109">GRPC istemci oluşturun.</span><span class="sxs-lookup"><span data-stu-id="3125e-109">Create a gRPC client.</span></span>
> * <span data-ttu-id="3125e-110">Önceki öğreticide oluşturulan Greeter hizmet gRPC karşı hizmet çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="3125e-110">Run the service against the gRPC Greeter service created in the previous tutorial.</span></span>
> * <span data-ttu-id="3125e-111">Proje dosyalarını inceleyin.</span><span class="sxs-lookup"><span data-stu-id="3125e-111">Examine the project files.</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-all-3.0.md)]

## <a name="create-a-net-console-application"></a><span data-ttu-id="3125e-112">Bir .NET konsol uygulaması oluşturun</span><span class="sxs-lookup"><span data-stu-id="3125e-112">Create a .NET console application</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3125e-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3125e-113">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="3125e-114">Yönergeleri izleyerek [burada](https://docs.microsoft.com/en-us/dotnet/core/tutorials/with-visual-studio) adıyla bir konsol uygulaması oluşturmak için *GrpcGreeterClient*.</span><span class="sxs-lookup"><span data-stu-id="3125e-114">Follow the instructions [here](https://docs.microsoft.com/en-us/dotnet/core/tutorials/with-visual-studio) to create a console app with the name *GrpcGreeterClient*.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="3125e-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="3125e-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="3125e-116">Yönergeleri izleyerek [burada](https://docs.microsoft.com/en-us/dotnet/core/tutorials/with-visual-studio-code) adıyla bir konsol uygulaması oluşturmak için *GrpcGreeterClient*.</span><span class="sxs-lookup"><span data-stu-id="3125e-116">Follow the instructions [here](https://docs.microsoft.com/en-us/dotnet/core/tutorials/with-visual-studio-code) to create a console app with the name *GrpcGreeterClient*.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="3125e-117">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3125e-117">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="3125e-118">Yönergeleri izleyerek [burada](https://docs.microsoft.com/en-us/dotnet/core/tutorials/using-on-mac-vs-full-solution) adıyla bir konsol uygulaması oluşturmak için *GrpcGreeterClient*.</span><span class="sxs-lookup"><span data-stu-id="3125e-118">Follow the instructions [here](https://docs.microsoft.com/en-us/dotnet/core/tutorials/using-on-mac-vs-full-solution) to create a console app with the name *GrpcGreeterClient*.</span></span>

<!-- End of VS tabs -->

---

## <a name="add-required-packages"></a><span data-ttu-id="3125e-119">Gerekli paketleri Ekle</span><span class="sxs-lookup"><span data-stu-id="3125e-119">Add required packages</span></span>

<span data-ttu-id="3125e-120">Aşağıdaki paketler gRPC istemci projeye ekleyin:</span><span class="sxs-lookup"><span data-stu-id="3125e-120">Add the following packages to the gRPC client project:</span></span>

* <span data-ttu-id="3125e-121">[Grpc.Core](https://www.nuget.org/packages/Grpc.Core), içeren C# C çekirdek istemci API'si.</span><span class="sxs-lookup"><span data-stu-id="3125e-121">[Grpc.Core](https://www.nuget.org/packages/Grpc.Core), which contains the C# API for the C-core client.</span></span>
* <span data-ttu-id="3125e-122">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), protobuf içeren API'leri için ileti C#.</span><span class="sxs-lookup"><span data-stu-id="3125e-122">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), which contains protobuf message APIs for C#.</span></span>
* <span data-ttu-id="3125e-123">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), içeren C# protobuf dosyaları için araç desteği.</span><span class="sxs-lookup"><span data-stu-id="3125e-123">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), which contains C# tooling support for protobuf files.</span></span> <span data-ttu-id="3125e-124">Bağımlılık ile işaretlenmiş şekilde araçları paket çalışma zamanında gerekli değil `PrivateAssets="All"`.</span><span class="sxs-lookup"><span data-stu-id="3125e-124">The tooling package isn't required at runtime, so the dependency is marked with `PrivateAssets="All"`.</span></span>

<span data-ttu-id="3125e-125">Paketleri ile aşağıdaki yaklaşımlardan eklenebilir:</span><span class="sxs-lookup"><span data-stu-id="3125e-125">Packages can be added with the following approaches:</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3125e-126">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3125e-126">Visual Studio</span></span>](#tab/visual-studio)

### <a name="pmc-option-to-install-packages"></a><span data-ttu-id="3125e-127">PMC seçeneği paketleri yüklemek için</span><span class="sxs-lookup"><span data-stu-id="3125e-127">PMC option to install packages</span></span>

* <span data-ttu-id="3125e-128">Visual Studio'dan seçin **Araçları** > **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu**</span><span class="sxs-lookup"><span data-stu-id="3125e-128">From Visual Studio, select **Tools** > **NuGet Package Manager** > **Package Manager Console**</span></span>
* <span data-ttu-id="3125e-129">Gelen **Paket Yöneticisi Konsolu** penceresinde olduğu dizine gidin *GrpcGreeterClient.csproj* dosya yok.</span><span class="sxs-lookup"><span data-stu-id="3125e-129">From the **Package Manager Console** window, navigate to the directory in which the *GrpcGreeterClient.csproj* file exists.</span></span>
* <span data-ttu-id="3125e-130">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="3125e-130">Run the following command:</span></span>

    ```powershell
    Install-Package Grpc.Core
    ```

* <span data-ttu-id="3125e-131">Yineleme `Install-Package` Google.Protobuf ve Grpc.Tools</span><span class="sxs-lookup"><span data-stu-id="3125e-131">Repeat the `Install-Package` for Google.Protobuf and Grpc.Tools</span></span>

<!-- Tutorials shouldn't have multiple options. Select what you think is the best approach. Recommend you removed this approach. -->

### <a name="manage-nuget-packages-option-to-install-packages"></a><span data-ttu-id="3125e-132">Paketleri yüklemek için seçeneği NuGet paketlerini Yönet</span><span class="sxs-lookup"><span data-stu-id="3125e-132">Manage NuGet Packages option to install packages</span></span>

* <span data-ttu-id="3125e-133">Projeye sağ **Çözüm Gezgini** > **NuGet paketlerini Yönet**</span><span class="sxs-lookup"><span data-stu-id="3125e-133">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
* <span data-ttu-id="3125e-134">Ayarlama **paket kaynağı** "nuget.org'da"</span><span class="sxs-lookup"><span data-stu-id="3125e-134">Set the **Package source** to "nuget.org"</span></span>
* <span data-ttu-id="3125e-135">Arama kutusuna "Grpc.Core"</span><span class="sxs-lookup"><span data-stu-id="3125e-135">Enter "Grpc.Core" in the search box</span></span>
* <span data-ttu-id="3125e-136">"Grpc.Core" paketinden seçin **Gözat** sekmesine **yükleyin**</span><span class="sxs-lookup"><span data-stu-id="3125e-136">Select the "Grpc.Core" package from the **Browse** tab and click **Install**</span></span>
* <span data-ttu-id="3125e-137">Google.Protobuf ve Grpc.Tools için yineleyin.</span><span class="sxs-lookup"><span data-stu-id="3125e-137">Repeat for Google.Protobuf and Grpc.Tools</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="3125e-138">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="3125e-138">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="3125e-139">Aşağıdaki komutu çalıştırın **tümleşik Terminalini**:</span><span class="sxs-lookup"><span data-stu-id="3125e-139">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add TodoApi.csproj package Grpc.Core
```

<span data-ttu-id="3125e-140">Google.Protobuf ve Grpc.Tools için yineleyin.</span><span class="sxs-lookup"><span data-stu-id="3125e-140">Repeat for Google.Protobuf and Grpc.Tools</span></span>

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="3125e-141">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3125e-141">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="3125e-142">Sağ *paketleri* klasöründe **çözüm bölmesi** > **paketleri Ekle...**</span><span class="sxs-lookup"><span data-stu-id="3125e-142">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="3125e-143">Ayarlama **paketleri Ekle** pencerenin **kaynak** "nuget.org" açılır menüsünü</span><span class="sxs-lookup"><span data-stu-id="3125e-143">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="3125e-144">Arama kutusuna "Grpc.Core"</span><span class="sxs-lookup"><span data-stu-id="3125e-144">Enter "Grpc.Core" in the search box</span></span>
* <span data-ttu-id="3125e-145">Sonuçlar bölmesinde "Grpc.Core" paketi seçin ve tıklayın **' paket Ekle**</span><span class="sxs-lookup"><span data-stu-id="3125e-145">Select the "Grpc.Core" package from the results pane and click **Add Package**</span></span>
* <span data-ttu-id="3125e-146">Google.Protobuf ve Grpc.Tools için yineleyin.</span><span class="sxs-lookup"><span data-stu-id="3125e-146">Repeat for Google.Protobuf and Grpc.Tools</span></span>

---

## <a name="add-the-greetproto-file"></a><span data-ttu-id="3125e-147">Greet.proto dosyası ekleme</span><span class="sxs-lookup"><span data-stu-id="3125e-147">Add the greet.proto file</span></span>

<span data-ttu-id="3125e-148">Kopyalama **Protos\greet.proto** Greeter hizmet gRPC dosyasına gRPC istemci projesi.</span><span class="sxs-lookup"><span data-stu-id="3125e-148">Copy the **Protos\greet.proto** file from the gRPC Greeter service to the gRPC client project.</span></span> <span data-ttu-id="3125e-149">Ekleme **greet.proto** dosyasını `<Protobuf>` GrpcGreeterClient proje dosyasının öğesi grubu:</span><span class="sxs-lookup"><span data-stu-id="3125e-149">Add the **greet.proto** file to the `<Protobuf>` item group of the GrpcGreeterClient project file:</span></span>

```XML
<Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
```

> [!NOTE]
> <span data-ttu-id="3125e-150">Projeye sağ tıklatıp seçerek GrpcGreeterClient proje dosyasını açabilirsiniz **Düzenle GrpcGreeterClient.csproj** açılan menüden seçeneği.</span><span class="sxs-lookup"><span data-stu-id="3125e-150">You can open the project file of GrpcGreeterClient by right-clicking the project and selecting the **Edit GrpcGreeterClient.csproj** option from the dropdown menu.</span></span>
>
> ![Yeni ASP.NET Core Web uygulaması](grpc-start/_static/edit_csproj.png)
>
> <span data-ttu-id="3125e-152">Alternatif olarak, GrpcGreeterClient dizine gidin ve düzenleme `GrpcGreeterClient.csproj` tercih ettiğiniz düzenleyiciyi ile.</span><span class="sxs-lookup"><span data-stu-id="3125e-152">Alternatively, you can navigate to the GrpcGreeterClient directory and edit the `GrpcGreeterClient.csproj` with your favorite editor.</span></span>

<span data-ttu-id="3125e-153">`GrpcServices="Client"` Özniteliği, yalnızca eklenir C# istemci varlıklar için dahil edilen protobuf dosyası oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="3125e-153">The `GrpcServices="Client"` attribute is added so that only the C# client assets are generated for the included protobuf file.</span></span> <span data-ttu-id="3125e-154">Nesil tetiklemek için istemci projesinin oluşturun C# istemci varlıklar.</span><span class="sxs-lookup"><span data-stu-id="3125e-154">Build the client project to trigger the generation of the C# client assets.</span></span>

## <a name="create-a-greeterclient-and-invoke-the-sayhello-unary-call"></a><span data-ttu-id="3125e-155">SayHello birli arama çağırmak ve bir GreeterClient oluşturma</span><span class="sxs-lookup"><span data-stu-id="3125e-155">Create a GreeterClient and invoke the SayHello unary call</span></span>

<span data-ttu-id="3125e-156">Aşağıdaki kodu ekleyin `Main` yöntemi `Program.cs` gRPC istemci projesinin dosya:</span><span class="sxs-lookup"><span data-stu-id="3125e-156">Add the following code to `Main` method of the `Program.cs` file of the gRPC client project:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=snippet)]

<span data-ttu-id="3125e-157">Aşağıdakileri kullanarak gerekli türlerine erişmek için deyimleri gereklidir:</span><span class="sxs-lookup"><span data-stu-id="3125e-157">To access the required types the following using statements are required:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=using)]

<span data-ttu-id="3125e-158">GreeterClient örneği tarafından oluşturulan bir `Channel` gRPC hizmeti bağlantısı oluşturma ve oluşturmak için kullanmaya yönelik bilgileri içeren `GreeterClient`:</span><span class="sxs-lookup"><span data-stu-id="3125e-158">The GreeterClient is created by instantiating a `Channel` containing the information for creating the connection to the gRPC service and using it to construct the `GreeterClient`:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=snippet&highlight=4-5)]

<span data-ttu-id="3125e-159">Birli çağrı GreeterClient içeren `SayHello` olduğu çağrılacak zaman uyumsuz olarak:</span><span class="sxs-lookup"><span data-stu-id="3125e-159">The GreeterClient contains the unary call `SayHello` which can be invoked asynchronously:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=snippet&highlight=7-8)]

<span data-ttu-id="3125e-160">Sonuçları `SayHello` çağrı depolanan `reply` , ardından görüntülenebilir:</span><span class="sxs-lookup"><span data-stu-id="3125e-160">The results of the `SayHello` call is stored in `reply` which can then be displayed:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=snippet&highlight=9)]

<span data-ttu-id="3125e-161">`Channel` Tarafından kullanılan tüm kaynakları serbest bırakmak için işlemleri tamamladıktan sonra istemcinin kapatılması:</span><span class="sxs-lookup"><span data-stu-id="3125e-161">The `Channel` used by the client should be shut down when operations have finished to release all resources:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=snippet&highlight=11)]

> [!NOTE]
> <span data-ttu-id="3125e-162">Türlerinde önce projeyi derlemek ihtiyacınız olacak **Greeter** ad alanı çözümlenebilir.</span><span class="sxs-lookup"><span data-stu-id="3125e-162">You will need to build the project before the types in the **Greeter** namespace can be resolved.</span></span> <span data-ttu-id="3125e-163">Bu türler, derleme sırasında otomatik olarak oluşturulur ve bir derleme çalıştırılmadan önce kullanılamaz olduğu.</span><span class="sxs-lookup"><span data-stu-id="3125e-163">These types are generated automatically during build and are not be available before a build is run.</span></span>

## <a name="test-the-grpc-client-with-the-grpc-greeter-service"></a><span data-ttu-id="3125e-164">GRPC Greeter hizmet gRPC istemciyle test etme</span><span class="sxs-lookup"><span data-stu-id="3125e-164">Test the gRPC client with the gRPC Greeter service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3125e-165">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3125e-165">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="3125e-166">Greeter önceki öğreticide oluşturulan hizmetinin çalıştığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="3125e-166">Ensure the Greeter service created in the previous tutorial is running.</span></span>

* <span data-ttu-id="3125e-167">Hizmet çalışmaya başladıktan sonra dönmek **GrpcGreeterClient** projeyi başlangıç projesi olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="3125e-167">Once the service is running, return to the **GrpcGreeterClient** project set it as the Startup Project.</span></span> <span data-ttu-id="3125e-168">Hata Ayıklayıcı olmadan istemciyi çalıştırmak için CTRL + F5 tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="3125e-168">Press Ctrl+F5 to run the client without the debugger.</span></span>

  <span data-ttu-id="3125e-169">İstemci bir karşılama "GreeterClient" adını içeren bir iletiyle hizmetine gönderir.</span><span class="sxs-lookup"><span data-stu-id="3125e-169">The client sends a greeting to the service with a message containing its name "GreeterClient".</span></span> <span data-ttu-id="3125e-170">Hizmet, komut satırında görüntülenen yanıt olarak bir "Hello GreeterClient" ileti gönderir.</span><span class="sxs-lookup"><span data-stu-id="3125e-170">The service will send a message "Hello GreeterClient" as a response that is displayed in the command prompt.</span></span>

  ![Yeni ASP.NET Core Web uygulaması](grpc-start/_static/client.png)

  <span data-ttu-id="3125e-172">Hizmet, komut istemine yazılan günlüklerinde başarılı görüşmesinin ayrıntılarını kaydeder.</span><span class="sxs-lookup"><span data-stu-id="3125e-172">The service records the details of the successful call in the logs written to the command prompt.</span></span>

  ![Yeni ASP.NET Core Web uygulaması](grpc-start/_static/server_complete.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="3125e-174">Visual Studio Code'u / Visual Studio Mac için</span><span class="sxs-lookup"><span data-stu-id="3125e-174">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="3125e-175">Greeter önceki öğreticide oluşturulan hizmetinin çalıştığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="3125e-175">Ensure the Greeter service created in the previous tutorial is running.</span></span>

* <span data-ttu-id="3125e-176">İstemci projesi GrpcGreeter.Client kullanılarak ayrı bir komut satırından çalıştırmak `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="3125e-176">Run the Client project GrpcGreeter.Client from the separate command line using `dotnet run`.</span></span>

<span data-ttu-id="3125e-177">İstemci bir karşılama "GreeterClient" adını içeren bir iletiyle hizmetine gönderir.</span><span class="sxs-lookup"><span data-stu-id="3125e-177">The client sends a greeting to the service with a message containing its name "GreeterClient".</span></span> <span data-ttu-id="3125e-178">Hizmet, komut satırında görüntülenen yanıt olarak bir "Hello GreeterClient" ileti gönderir.</span><span class="sxs-lookup"><span data-stu-id="3125e-178">The service will send a message "Hello GreeterClient" as a response that is displayed in the command prompt.</span></span>

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

<span data-ttu-id="3125e-179">Hizmet, komut istemine yazılan günlüklerinde başarılı görüşmesinin ayrıntılarını kaydeder.</span><span class="sxs-lookup"><span data-stu-id="3125e-179">The service records the details of the successful call in the logs written to the command prompt.</span></span>

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

### <a name="examine-the-project-files-of-the-grpc-project"></a><span data-ttu-id="3125e-180">GRPC projenin proje dosyalarını inceleyin</span><span class="sxs-lookup"><span data-stu-id="3125e-180">Examine the project files of the gRPC project</span></span>

<span data-ttu-id="3125e-181">gRPC istemci GrpcGreeterClient dosyası:</span><span class="sxs-lookup"><span data-stu-id="3125e-181">gRPC client GrpcGreeterClient file:</span></span>

<span data-ttu-id="3125e-182">*Program.cs* gRPC istemci için giriş noktası ve mantığı içerir.</span><span class="sxs-lookup"><span data-stu-id="3125e-182">*Program.cs* contains the entry point and logic for the gRPC client.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3125e-183">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="3125e-183">Additional resources</span></span>

<span data-ttu-id="3125e-184">Bu öğreticide şunları yaptınız:</span><span class="sxs-lookup"><span data-stu-id="3125e-184">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="3125e-185">GRPC istemci oluşturun.</span><span class="sxs-lookup"><span data-stu-id="3125e-185">Create a gRPC client.</span></span>
> * <span data-ttu-id="3125e-186">Önceki öğreticide oluşturulan Greeter hizmet gRPC karşı hizmet çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="3125e-186">Run the service against the gRPC Greeter service created in the previous tutorial.</span></span>
> * <span data-ttu-id="3125e-187">Proje dosyalarını inceleyin.</span><span class="sxs-lookup"><span data-stu-id="3125e-187">Examine the project files.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="3125e-188">Önceki: Bir gRPC Greeter hizmeti oluşturma</span><span class="sxs-lookup"><span data-stu-id="3125e-188">Previous: Create a gRPC Greeter Service</span></span>](xref:tutorials/grpc/grpc-start)
