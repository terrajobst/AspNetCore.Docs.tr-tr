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
# <a name="use-aspnet-core-signalr-with-blazor-webassembly"></a><span data-ttu-id="301aa-103">Blazor WebAssembly ile ASP.NET Core SignalR kullanma</span><span class="sxs-lookup"><span data-stu-id="301aa-103">Use ASP.NET Core SignalR with Blazor WebAssembly</span></span>

<span data-ttu-id="301aa-104">[Daniel Roth](https://github.com/danroth27) ve [Luke Latham](https://github.com/guardrex) tarafından</span><span class="sxs-lookup"><span data-stu-id="301aa-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="301aa-105">Bu öğreticide, Blazor WebAssembly ile SignalR kullanarak gerçek zamanlı bir uygulama oluşturmanın temelleri öğretilir.</span><span class="sxs-lookup"><span data-stu-id="301aa-105">This tutorial teaches the basics of building a real-time app using SignalR with Blazor WebAssembly.</span></span> <span data-ttu-id="301aa-106">Aşağıdakilerin nasıl yapıldığını öğreneceksiniz:</span><span class="sxs-lookup"><span data-stu-id="301aa-106">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="301aa-107">Blazor WebAssembly barındırılan uygulama projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="301aa-107">Create a Blazor WebAssembly Hosted app project</span></span>
> * <span data-ttu-id="301aa-108">SignalR istemci kitaplığını ekleme</span><span class="sxs-lookup"><span data-stu-id="301aa-108">Add the SignalR client library</span></span>
> * <span data-ttu-id="301aa-109">SignalR hub 'ı ekleme</span><span class="sxs-lookup"><span data-stu-id="301aa-109">Add a SignalR hub</span></span>
> * <span data-ttu-id="301aa-110">SignalR hub 'ı için SignalR Hizmetleri ve uç nokta ekleme</span><span class="sxs-lookup"><span data-stu-id="301aa-110">Add SignalR services and an endpoint for the SignalR hub</span></span>
> * <span data-ttu-id="301aa-111">Sohbet için Razor bileşeni kodu ekleme</span><span class="sxs-lookup"><span data-stu-id="301aa-111">Add Razor component code for chat</span></span>

<span data-ttu-id="301aa-112">Bu öğreticinin sonunda, çalışan bir sohbet uygulamanız olacaktır.</span><span class="sxs-lookup"><span data-stu-id="301aa-112">At the end of this tutorial, you'll have a working chat app.</span></span>

<span data-ttu-id="301aa-113">[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/signalr-blazor-webassembly/samples/) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="301aa-113">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/signalr-blazor-webassembly/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="301aa-114">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="301aa-114">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="301aa-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="301aa-115">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.1.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="301aa-116">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="301aa-116">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.1.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="301aa-117">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="301aa-117">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.1.md)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="301aa-118">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="301aa-118">.NET Core CLI</span></span>](#tab/netcore-cli/)

[!INCLUDE[](~/includes/3.1-SDK.md)]

---

## <a name="create-a-hosted-blazor-webassembly-app-project"></a><span data-ttu-id="301aa-119">Barındırılan Blazor WebAssembly uygulama projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="301aa-119">Create a hosted Blazor WebAssembly app project</span></span>

<span data-ttu-id="301aa-120">[Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) şablonunu yükler.</span><span class="sxs-lookup"><span data-stu-id="301aa-120">Install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template.</span></span> <span data-ttu-id="301aa-121">[Microsoft. AspNetCore. Blazor. Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) paketinin önizleme sürümü vardır, ancak Blazor WebAssembly önizlemededir.</span><span class="sxs-lookup"><span data-stu-id="301aa-121">The [Microsoft.AspNetCore.Blazor.Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) package has a preview version while Blazor WebAssembly is in preview.</span></span> <span data-ttu-id="301aa-122">Bir komut kabuğunda, aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="301aa-122">In a command shell, execute the following command:</span></span>

```dotnetcli
dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.2.0-preview1.20073.1
```

<span data-ttu-id="301aa-123">Araç seçiminiz için yönergeleri izleyin:</span><span class="sxs-lookup"><span data-stu-id="301aa-123">Follow the guidance for your choice of tooling:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="301aa-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="301aa-124">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="301aa-125">Yeni bir proje oluşturun.</span><span class="sxs-lookup"><span data-stu-id="301aa-125">Create a new project.</span></span>

1. <span data-ttu-id="301aa-126">**Blazor uygulamasını** seçin ve **İleri ' yi**seçin.</span><span class="sxs-lookup"><span data-stu-id="301aa-126">Select **Blazor App** and select **Next**.</span></span>

1. <span data-ttu-id="301aa-127">**Proje adı** alanına "BlazorSignalRApp" yazın.</span><span class="sxs-lookup"><span data-stu-id="301aa-127">Type "BlazorSignalRApp" in the **Project name** field.</span></span> <span data-ttu-id="301aa-128">**Konum** girişinin doğru olduğunu onaylayın veya proje için bir konum belirtin.</span><span class="sxs-lookup"><span data-stu-id="301aa-128">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="301aa-129">**Oluştur**’u seçin.</span><span class="sxs-lookup"><span data-stu-id="301aa-129">Select **Create**.</span></span>

1. <span data-ttu-id="301aa-130">**Blazor WebAssembly uygulama** şablonunu seçin.</span><span class="sxs-lookup"><span data-stu-id="301aa-130">Choose the **Blazor WebAssembly App** template.</span></span>

1. <span data-ttu-id="301aa-131">**Gelişmiş**' in altında, **ASP.NET Core barındırılan** onay kutusunu seçin.</span><span class="sxs-lookup"><span data-stu-id="301aa-131">Under **Advanced**, select the **ASP.NET Core hosted** check box.</span></span>

1. <span data-ttu-id="301aa-132">**Oluştur**’u seçin.</span><span class="sxs-lookup"><span data-stu-id="301aa-132">Select **Create**.</span></span>

> [!NOTE]
> <span data-ttu-id="301aa-133">Visual Studio 'nun yeni bir sürümünü yükselttiyseniz veya yüklediyseniz ve Blazor WebAssembly şablonu VS Kullanıcı arabiriminde görünmüyorsa, daha önce gösterilen `dotnet new` komutunu kullanarak şablonu yeniden yükleyin.</span><span class="sxs-lookup"><span data-stu-id="301aa-133">If you upgraded or installed a new version of Visual Studio and the Blazor WebAssembly template doesn't appear in the VS UI, reinstall the template using the `dotnet new` command shown previously.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="301aa-134">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="301aa-134">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="301aa-135">Bir komut kabuğunda, aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="301aa-135">In a command shell, execute the following command:</span></span>

   ```dotnetcli
   dotnet new blazorwasm --hosted --output BlazorSignalRApp
   ```

1. <span data-ttu-id="301aa-136">Visual Studio Code, uygulamanın proje klasörünü açın.</span><span class="sxs-lookup"><span data-stu-id="301aa-136">In Visual Studio Code, open the app's project folder.</span></span>

1. <span data-ttu-id="301aa-137">Uygulamayı derlemek ve hata ayıklamak için varlık Ekle iletişim kutusu göründüğünde **Evet**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="301aa-137">When the dialog appears to add assets to build and debug the app, select **Yes**.</span></span> <span data-ttu-id="301aa-138">Visual Studio Code, *. vscode* klasörünü oluşturulan *Launch. JSON* ve *Tasks. JSON* dosyaları ile otomatik olarak ekler.</span><span class="sxs-lookup"><span data-stu-id="301aa-138">Visual Studio Code automatically adds the *.vscode* folder with generated *launch.json* and *tasks.json* files.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="301aa-139">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="301aa-139">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="301aa-140">Bir komut kabuğunda, aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="301aa-140">In a command shell, execute the following command:</span></span>

   ```dotnetcli
   dotnet new blazorwasm --hosted --output BlazorSignalRApp
   ```

1. <span data-ttu-id="301aa-141">Mac için Visual Studio, proje klasörüne gidip projenin çözüm dosyasını ( *. sln*) açarak projeyi açın.</span><span class="sxs-lookup"><span data-stu-id="301aa-141">In Visual Studio for Mac, open the project by navigating to the project folder and opening the project's solution file (*.sln*).</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="301aa-142">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="301aa-142">.NET Core CLI</span></span>](#tab/netcore-cli/)

<span data-ttu-id="301aa-143">Bir komut kabuğunda, aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="301aa-143">In a command shell, execute the following command:</span></span>

```dotnetcli
dotnet new blazorwasm --hosted --output BlazorSignalRApp
```

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="301aa-144">SignalR istemci kitaplığını ekleme</span><span class="sxs-lookup"><span data-stu-id="301aa-144">Add the SignalR client library</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="301aa-145">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="301aa-145">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="301aa-146">**Çözüm Gezgini**, **BlazorSignalRApp. Client** projesine sağ tıklayın ve **NuGet Paketlerini Yönet**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="301aa-146">In **Solution Explorer**, right-click the **BlazorSignalRApp.Client** project and select **Manage NuGet Packages**.</span></span>

1. <span data-ttu-id="301aa-147">**NuGet Paketlerini Yönet** iletişim kutusunda, **paket kaynağının** *NuGet.org*olarak ayarlandığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="301aa-147">In the **Manage NuGet Packages** dialog, confirm that the **Package source** is set to *nuget.org*.</span></span>

1. <span data-ttu-id="301aa-148">Araştır **seçiliyken,** arama kutusuna "Microsoft. aspnetcore. SignalR. Client" yazın.</span><span class="sxs-lookup"><span data-stu-id="301aa-148">With **Browse** selected, type "Microsoft.AspNetCore.SignalR.Client" in the search box.</span></span>

1. <span data-ttu-id="301aa-149">Arama sonuçlarında `Microsoft.AspNetCore.SignalR.Client` paketini seçin ve ardından **Install**' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="301aa-149">In the search results, select the `Microsoft.AspNetCore.SignalR.Client` package and select **Install**.</span></span>

1. <span data-ttu-id="301aa-150">**Değişiklikleri Önizle** iletişim kutusu görüntülenirse **Tamam**' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="301aa-150">If the **Preview Changes** dialog appears, select **OK**.</span></span>

1. <span data-ttu-id="301aa-151">**Lisans kabulü** iletişim kutusu görüntülenirse, lisans şartlarını kabul ediyorsanız **kabul ediyorum** ' u seçin.</span><span class="sxs-lookup"><span data-stu-id="301aa-151">If the **License Acceptance** dialog appears, select **I Accept** if you agree with the license terms.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="301aa-152">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="301aa-152">Visual Studio Code</span></span>](#tab/visual-studio-code/)

<span data-ttu-id="301aa-153">**Tümleşik terminalde** (araç çubuğundan > **terminalini** **görüntüleyin** ), aşağıdaki komutları yürütün:</span><span class="sxs-lookup"><span data-stu-id="301aa-153">In the **Integrated Terminal** (**View** > **Terminal** from the toolbar), execute the following commands:</span></span>

```dotnetcli
dotnet add Client package Microsoft.AspNetCore.SignalR.Client
```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="301aa-154">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="301aa-154">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="301aa-155">**Çözüm** kenar çubuğunda **BlazorSignalRApp. Client** projesine sağ tıklayın ve **NuGet Paketlerini Yönet**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="301aa-155">In the **Solution** sidebar, right-click the **BlazorSignalRApp.Client** project and select **Manage NuGet Packages**.</span></span>

1. <span data-ttu-id="301aa-156">**NuGet Paketlerini Yönet** iletişim kutusunda, kaynak açılan kutusunun *NuGet.org*olarak ayarlandığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="301aa-156">In the **Manage NuGet Packages** dialog, confirm that the source drop-down is set to *nuget.org*.</span></span>

1. <span data-ttu-id="301aa-157">Araştır **seçiliyken,** arama kutusuna "Microsoft. aspnetcore. SignalR. Client" yazın.</span><span class="sxs-lookup"><span data-stu-id="301aa-157">With **Browse** selected, type "Microsoft.AspNetCore.SignalR.Client" in the search box.</span></span>

1. <span data-ttu-id="301aa-158">Arama sonuçlarında `Microsoft.AspNetCore.SignalR.Client` paketinin yanındaki onay kutusunu işaretleyin ve **paket Ekle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="301aa-158">In the search results, select the check box next to the `Microsoft.AspNetCore.SignalR.Client` package and select **Add Package**.</span></span>

1. <span data-ttu-id="301aa-159">**Lisans kabulü** iletişim kutusu görüntülenirse, lisans şartlarını kabul ediyorsanız **kabul et** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="301aa-159">If the **License Acceptance** dialog appears, select **Accept** if you agree with the license terms.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="301aa-160">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="301aa-160">.NET Core CLI</span></span>](#tab/netcore-cli/)

<span data-ttu-id="301aa-161">Komut kabuğu 'nda aşağıdaki komutları yürütün:</span><span class="sxs-lookup"><span data-stu-id="301aa-161">In a command shell, execute the following commands:</span></span>

```dotnetcli
cd BlazorSignalRApp
dotnet add Client package Microsoft.AspNetCore.SignalR.Client
```

---

## <a name="add-a-signalr-hub"></a><span data-ttu-id="301aa-162">SignalR hub 'ı ekleme</span><span class="sxs-lookup"><span data-stu-id="301aa-162">Add a SignalR hub</span></span>

<span data-ttu-id="301aa-163">**BlazorSignalRApp. Server** projesinde, bir *hub* (plural) klasörü oluşturun ve aşağıdaki `ChatHub` sınıfını (*hub/ChatHub. cs*) ekleyin:</span><span class="sxs-lookup"><span data-stu-id="301aa-163">In the **BlazorSignalRApp.Server** project, create a *Hubs* (plural) folder and add the following `ChatHub` class (*Hubs/ChatHub.cs*):</span></span>

[!code-csharp[](signalr-blazor-webassembly/samples/3.x/BlazorSignalRApp/Server/Hubs/ChatHub.cs)]

## <a name="add-signalr-services-and-an-endpoint-for-the-signalr-hub"></a><span data-ttu-id="301aa-164">SignalR hub 'ı için SignalR Hizmetleri ve uç nokta ekleme</span><span class="sxs-lookup"><span data-stu-id="301aa-164">Add SignalR services and an endpoint for the SignalR hub</span></span>

1. <span data-ttu-id="301aa-165">**BlazorSignalRApp. Server** projesinde, *Startup.cs* dosyasını açın.</span><span class="sxs-lookup"><span data-stu-id="301aa-165">In the **BlazorSignalRApp.Server** project, open the *Startup.cs* file.</span></span>

1. <span data-ttu-id="301aa-166">`ChatHub` sınıfı için ad alanını dosyanın en üstüne ekleyin:</span><span class="sxs-lookup"><span data-stu-id="301aa-166">Add the namespace for the `ChatHub` class to the top of the file:</span></span>

   ```csharp
   using BlazorSignalRApp.Server.Hubs;
   ```

1. <span data-ttu-id="301aa-167">SignalR hizmetlerini `Startup.ConfigureServices`ekleyin:</span><span class="sxs-lookup"><span data-stu-id="301aa-167">Add the SignalR services to `Startup.ConfigureServices`:</span></span>

   ```csharp
   services.AddSignalR();
   ```

1. <span data-ttu-id="301aa-168">Varsayılan denetleyici yolu ve istemci tarafı geri dönüş uç noktaları arasında `Startup.Configure`, Hub için bir uç nokta ekleyin:</span><span class="sxs-lookup"><span data-stu-id="301aa-168">In `Startup.Configure` between the endpoints for the default controller route and the client-side fallback, add an endpoint for the hub:</span></span>

   [!code-csharp[](signalr-blazor-webassembly/samples/3.x/BlazorSignalRApp/Server/Startup.cs?name=snippet&highlight=4)]

## <a name="add-razor-component-code-for-chat"></a><span data-ttu-id="301aa-169">Sohbet için Razor bileşeni kodu ekleme</span><span class="sxs-lookup"><span data-stu-id="301aa-169">Add Razor component code for chat</span></span>

1. <span data-ttu-id="301aa-170">**BlazorSignalRApp. Client** projesinde *Pages/Index. Razor* dosyasını açın.</span><span class="sxs-lookup"><span data-stu-id="301aa-170">In the **BlazorSignalRApp.Client** project, open the *Pages/Index.razor* file.</span></span>

1. <span data-ttu-id="301aa-171">İşaretlemeyi aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="301aa-171">Replace the markup with the following code:</span></span>

[!code-razor[](signalr-blazor-webassembly/samples/3.x/BlazorSignalRApp/Client/Pages/Index.razor)]

## <a name="run-the-app"></a><span data-ttu-id="301aa-172">Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="301aa-172">Run the app</span></span>

1. <span data-ttu-id="301aa-173">Araç kılavuzunuz için yönergeleri izleyin:</span><span class="sxs-lookup"><span data-stu-id="301aa-173">Follow the guidance for your tooling:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="301aa-174">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="301aa-174">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="301aa-175">**Çözüm Gezgini**, **BlazorSignalRApp. Server** projesini seçin.</span><span class="sxs-lookup"><span data-stu-id="301aa-175">In **Solution Explorer**, select the **BlazorSignalRApp.Server** project.</span></span> <span data-ttu-id="301aa-176">Uygulamayı hata ayıklamadan çalıştırmak için **CTRL + F5** tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="301aa-176">Press **Ctrl+F5** to run the app without debugging.</span></span>

1. <span data-ttu-id="301aa-177">Adres çubuğundan URL 'yi kopyalayın, başka bir tarayıcı örneği veya sekme açın ve adres çubuğuna URL 'YI yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="301aa-177">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="301aa-178">Tarayıcı ' yı seçin, bir ad ve ileti girin ve **Gönder** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="301aa-178">Choose either browser, enter a name and message, and select the **Send** button.</span></span> <span data-ttu-id="301aa-179">Ad ve ileti her iki sayfada da anında görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="301aa-179">The name and message are displayed on both pages instantly:</span></span>

   ![SignalR Blazor WebAssembly örnek uygulaması, değiştirilen iletileri gösteren iki tarayıcı penceresinde açılır.](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)

   <span data-ttu-id="301aa-181">Tırnak: *yıldız Trek VI: bulunan ülke* &copy;1991 [Paramount](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span><span class="sxs-lookup"><span data-stu-id="301aa-181">Quotes: *Star Trek VI: The Undiscovered Country* &copy;1991 [Paramount](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="301aa-182">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="301aa-182">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="301aa-183">Araç çubuğundan hata **ayıkla** > **Çalıştır** ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="301aa-183">Select **Debug** > **Run Without Debugging** from the toolbar.</span></span>

1. <span data-ttu-id="301aa-184">Adres çubuğundan URL 'yi kopyalayın, başka bir tarayıcı örneği veya sekme açın ve adres çubuğuna URL 'YI yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="301aa-184">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="301aa-185">Tarayıcı ' yı seçin, bir ad ve ileti girin ve **Gönder** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="301aa-185">Choose either browser, enter a name and message, and select the **Send** button.</span></span> <span data-ttu-id="301aa-186">Ad ve ileti her iki sayfada da anında görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="301aa-186">The name and message are displayed on both pages instantly:</span></span>

   ![SignalR Blazor WebAssembly örnek uygulaması, değiştirilen iletileri gösteren iki tarayıcı penceresinde açılır.](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)

   <span data-ttu-id="301aa-188">Tırnak: *yıldız Trek VI: bulunan ülke* &copy;1991 [Paramount](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span><span class="sxs-lookup"><span data-stu-id="301aa-188">Quotes: *Star Trek VI: The Undiscovered Country* &copy;1991 [Paramount](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="301aa-189">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="301aa-189">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="301aa-190">**Çözüm** kenar çubuğunda **BlazorSignalRApp. Server** projesini seçin.</span><span class="sxs-lookup"><span data-stu-id="301aa-190">In the **Solution** sidebar, select the **BlazorSignalRApp.Server** project.</span></span> <span data-ttu-id="301aa-191">Menüden, **hata ayıklama olmadan başlat** > **Çalıştır** ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="301aa-191">From the menu, select **Run** > **Start Without Debugging**.</span></span>

1. <span data-ttu-id="301aa-192">Adres çubuğundan URL 'yi kopyalayın, başka bir tarayıcı örneği veya sekme açın ve adres çubuğuna URL 'YI yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="301aa-192">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="301aa-193">Tarayıcı ' yı seçin, bir ad ve ileti girin ve **Gönder** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="301aa-193">Choose either browser, enter a name and message, and select the **Send** button.</span></span> <span data-ttu-id="301aa-194">Ad ve ileti her iki sayfada da anında görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="301aa-194">The name and message are displayed on both pages instantly:</span></span>

   ![SignalR Blazor WebAssembly örnek uygulaması, değiştirilen iletileri gösteren iki tarayıcı penceresinde açılır.](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)

   <span data-ttu-id="301aa-196">Tırnak: *yıldız Trek VI: bulunan ülke* &copy;1991 [Paramount](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span><span class="sxs-lookup"><span data-stu-id="301aa-196">Quotes: *Star Trek VI: The Undiscovered Country* &copy;1991 [Paramount](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="301aa-197">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="301aa-197">.NET Core CLI</span></span>](#tab/netcore-cli/)

1. <span data-ttu-id="301aa-198">Komut kabuğu 'nda aşağıdaki komutları yürütün:</span><span class="sxs-lookup"><span data-stu-id="301aa-198">In a command shell, execute the following commands:</span></span>

   ```dotnetcli
   cd Server
   dotnet run
   ```

1. <span data-ttu-id="301aa-199">Adres çubuğundan URL 'yi kopyalayın, başka bir tarayıcı örneği veya sekme açın ve adres çubuğuna URL 'YI yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="301aa-199">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="301aa-200">Tarayıcı ' yı seçin, bir ad ve ileti girin ve **Gönder** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="301aa-200">Choose either browser, enter a name and message, and select the **Send** button.</span></span> <span data-ttu-id="301aa-201">Ad ve ileti her iki sayfada da anında görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="301aa-201">The name and message are displayed on both pages instantly:</span></span>

   ![SignalR Blazor WebAssembly örnek uygulaması, değiştirilen iletileri gösteren iki tarayıcı penceresinde açılır.](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)

   <span data-ttu-id="301aa-203">Tırnak: *yıldız Trek VI: bulunan ülke* &copy;1991 [Paramount](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span><span class="sxs-lookup"><span data-stu-id="301aa-203">Quotes: *Star Trek VI: The Undiscovered Country* &copy;1991 [Paramount](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span></span>

---

## <a name="next-steps"></a><span data-ttu-id="301aa-204">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="301aa-204">Next steps</span></span>

<span data-ttu-id="301aa-205">Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:</span><span class="sxs-lookup"><span data-stu-id="301aa-205">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="301aa-206">Blazor Weelsembly barındırılan uygulama projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="301aa-206">Create a Blazor WebAssembly Hosted app project</span></span>
> * <span data-ttu-id="301aa-207">SignalR istemci kitaplığı ekleme</span><span class="sxs-lookup"><span data-stu-id="301aa-207">Add the SignalR client library</span></span>
> * <span data-ttu-id="301aa-208">SignalR hub 'ı ekleme</span><span class="sxs-lookup"><span data-stu-id="301aa-208">Add a SignalR hub</span></span>
> * <span data-ttu-id="301aa-209">SignalR hub 'ı için SignalR Hizmetleri ve uç nokta ekleme</span><span class="sxs-lookup"><span data-stu-id="301aa-209">Add SignalR services and an endpoint for the SignalR hub</span></span>
> * <span data-ttu-id="301aa-210">Sohbet için Razor bileşeni kodu ekleme</span><span class="sxs-lookup"><span data-stu-id="301aa-210">Add Razor component code for chat</span></span>

<span data-ttu-id="301aa-211">Blazor uygulamaları oluşturma hakkında daha fazla bilgi için Blazor belgelerine bakın:</span><span class="sxs-lookup"><span data-stu-id="301aa-211">To learn more about building Blazor apps, see the Blazor documentation:</span></span>

> [!div class="nextstepaction"]
> <xref:blazor/index>

## <a name="additional-resources"></a><span data-ttu-id="301aa-212">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="301aa-212">Additional resources</span></span>

* <xref:signalr/introduction>
