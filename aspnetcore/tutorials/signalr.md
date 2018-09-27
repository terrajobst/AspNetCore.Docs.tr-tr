---
title: 'Öğretici: SignalR üzerinde ASP.NET Core ile çalışmaya başlama'
author: tdykstra
description: Bu öğreticide, ASP.NET Core için SignalR kullanan bir sohbet uygulaması oluşturma.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/31/2018
uid: tutorials/signalr
ms.openlocfilehash: 6f93d6dc664f68425ef0fa0d02f9011e4875bc33
ms.sourcegitcommit: 9bdba90b2c97a4016188434657194b2d7027d6e3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/27/2018
ms.locfileid: "47402139"
---
# <a name="tutorial-get-started-with-signalr-on-aspnet-core"></a><span data-ttu-id="9d27f-103">Öğretici: SignalR üzerinde ASP.NET Core ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="9d27f-103">Tutorial: Get started with SignalR on ASP.NET Core</span></span>

<span data-ttu-id="9d27f-104">Bu öğreticide SignalR kullanarak gerçek zamanlı bir uygulama oluşturmaya ilişkin temel bilgileri size öğretir.</span><span class="sxs-lookup"><span data-stu-id="9d27f-104">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="9d27f-105">Aşağıdakilerin nasıl yapıldığını öğreneceksiniz:</span><span class="sxs-lookup"><span data-stu-id="9d27f-105">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="9d27f-106">SignalR üzerinde ASP.NET Core kullanan bir web uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="9d27f-106">Create a web app that uses SignalR on ASP.NET Core.</span></span>
> * <span data-ttu-id="9d27f-107">Bir SignalR hub'ı sunucuda oluşturun.</span><span class="sxs-lookup"><span data-stu-id="9d27f-107">Create a SignalR hub on the server.</span></span>
> * <span data-ttu-id="9d27f-108">SignalR hub'ına JavaScript istemcilerinden bağlanın.</span><span class="sxs-lookup"><span data-stu-id="9d27f-108">Connect to the SignalR hub from JavaScript clients.</span></span>
> * <span data-ttu-id="9d27f-109">Bağlanan tüm istemciler için herhangi bir istemciden ileti göndermek için hub'ı kullanın.</span><span class="sxs-lookup"><span data-stu-id="9d27f-109">Use the hub to send messages from any client to all connected clients.</span></span>

<span data-ttu-id="9d27f-110">Sonunda, bir çalışma sohbet uygulaması oluşturmuş olacaksınız:</span><span class="sxs-lookup"><span data-stu-id="9d27f-110">At the end, you'll have a working chat app:</span></span>

![SignalR örnek uygulaması](signalr/_static/signalr-get-started-finished.png)

<span data-ttu-id="9d27f-112">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="9d27f-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9d27f-113">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="9d27f-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9d27f-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9d27f-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="9d27f-115">[Visual Studio 2017 sürüm 15,8 veya üzeri](https://www.visualstudio.com/downloads/) ile **ASP.NET ve web geliştirme** iş yükü</span><span class="sxs-lookup"><span data-stu-id="9d27f-115">[Visual Studio 2017 version 15.8 or later](https://www.visualstudio.com/downloads/) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="9d27f-116">.NET core SDK 2.1 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="9d27f-116">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9d27f-117">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9d27f-117">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="9d27f-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9d27f-118">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="9d27f-119">.NET core SDK 2.1 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="9d27f-119">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="9d27f-120">Visual Studio Code için C#</span><span class="sxs-lookup"><span data-stu-id="9d27f-120">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9d27f-121">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9d27f-121">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* [<span data-ttu-id="9d27f-122">Sürüm 7.5.4 Mac için Visual Studio veya üzeri</span><span class="sxs-lookup"><span data-stu-id="9d27f-122">Visual Studio for Mac version 7.5.4 or later</span></span>](https://www.visualstudio.com/downloads/)
* <span data-ttu-id="9d27f-123">[.NET core SDK 2.1 veya üzeri](https://www.microsoft.com/net/download/all) (Visual Studio yüklemesine dahil)</span><span class="sxs-lookup"><span data-stu-id="9d27f-123">[.NET Core SDK 2.1 or later](https://www.microsoft.com/net/download/all) (included in the Visual Studio install)</span></span>

---

## <a name="create-the-project"></a><span data-ttu-id="9d27f-124">Projeyi oluşturma</span><span class="sxs-lookup"><span data-stu-id="9d27f-124">Create the project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9d27f-125">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9d27f-125">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="9d27f-126">Menüden **Dosya > Yeni proje**.</span><span class="sxs-lookup"><span data-stu-id="9d27f-126">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="9d27f-127">İçinde **yeni proje** iletişim kutusunda **yüklü > Visual C# > Web > ASP.NET Core Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="9d27f-127">In the **New Project** dialog, select **Installed > Visual C# > Web > ASP.NET Core Web Application**.</span></span> <span data-ttu-id="9d27f-128">Projeyi adlandırın *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="9d27f-128">Name the project *SignalRChat*.</span></span>

  ![Visual Studio'da yeni proje iletişim kutusu](signalr/_static/signalr-new-project-dialog.png)

* <span data-ttu-id="9d27f-130">Seçin **Web uygulaması** Razor sayfaları kullanan bir proje oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="9d27f-130">Select **Web Application** to create a project that uses Razor Pages.</span></span>

* <span data-ttu-id="9d27f-131">Bir hedef Framework'ü seçin **.NET Core**seçin **ASP.NET Core 2.1**, tıklatıp **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="9d27f-131">Select a target framework of **.NET Core**, select **ASP.NET Core 2.1**, and click **OK**.</span></span>

  ![Visual Studio'da yeni proje iletişim kutusu](signalr/_static/signalr-new-project-choose-type.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9d27f-133">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9d27f-133">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="9d27f-134">Yeni bir proje için kullanabileceğiniz bir klasöre açın.</span><span class="sxs-lookup"><span data-stu-id="9d27f-134">Open a folder that you can use for a new project.</span></span>

* <span data-ttu-id="9d27f-135">İçinde **tümleşik Terminalini**, aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="9d27f-135">In the **Integrated Terminal**, run the following command:</span></span>

   ```console
   dotnet new webapp -o SignalRChat
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9d27f-136">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9d27f-136">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="9d27f-137">Menüden **Dosya > Yeni Çözüm**.</span><span class="sxs-lookup"><span data-stu-id="9d27f-137">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="9d27f-138">Seçin **.NET Core > Uygulama > ASP.NET Core Web uygulaması** (seçmeyin **ASP.NET Core Web uygulaması (MVC)**).</span><span class="sxs-lookup"><span data-stu-id="9d27f-138">Select **.NET Core > App > ASP.NET Core Web App** (Don't select **ASP.NET Core Web App (MVC)**).</span></span>

* <span data-ttu-id="9d27f-139">Seçin **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="9d27f-139">Select **Next**.</span></span>

* <span data-ttu-id="9d27f-140">Projeyi adlandırın *SignalRChat*ve ardından **Oluştur**.</span><span class="sxs-lookup"><span data-stu-id="9d27f-140">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="9d27f-141">SignalR istemci kitaplığı Ekle</span><span class="sxs-lookup"><span data-stu-id="9d27f-141">Add the SignalR client library</span></span>

<span data-ttu-id="9d27f-142">SignalR server kitaplığı dahil [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="9d27f-142">The SignalR server library is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="9d27f-143">JavaScript istemci kitaplığı, otomatik olarak projeye dahil değil.</span><span class="sxs-lookup"><span data-stu-id="9d27f-143">The JavaScript client library isn't automatically included in the project.</span></span> <span data-ttu-id="9d27f-144">Bu öğretici için kullandığınız [Kitaplık Yöneticisi'ni (LibMan)](xref:client-side/libman/index) istemci Kitaplığı'ndan almak için *unpkg*.</span><span class="sxs-lookup"><span data-stu-id="9d27f-144">For this tutorial, you use [Library Manager (LibMan)](xref:client-side/libman/index) to get the client library from *unpkg*.</span></span> <span data-ttu-id="9d27f-145">[unpkg](https://unpkg.com/#/) olduğu bir [içerik teslim ağı](https://wikipedia.org/wiki/Content_delivery_network) , teslim edebilir bulunan herhangi bir şey [npm, Node.js Paket Yöneticisi](https://www.npmjs.com/get-npm).</span><span class="sxs-lookup"><span data-stu-id="9d27f-145">[unpkg](https://unpkg.com/#/) is a [content delivery network](https://wikipedia.org/wiki/Content_delivery_network) that can deliver anything found in [npm, the Node.js package manager](https://www.npmjs.com/get-npm).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9d27f-146">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9d27f-146">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="9d27f-147">İçinde **Çözüm Gezgini**projeye sağ tıklayıp seçin **Ekle** > **istemci tarafı kitaplık**.</span><span class="sxs-lookup"><span data-stu-id="9d27f-147">In **Solution Explorer**, right-click the project, and select **Add** > **Client-Side Library**.</span></span>

* <span data-ttu-id="9d27f-148">İçinde **istemci tarafı kitaplık Ekle** iletişim için **sağlayıcısı** seçin **unpkg**.</span><span class="sxs-lookup"><span data-stu-id="9d27f-148">In the **Add Client-Side Library** dialog, for **Provider** select **unpkg**.</span></span> 

* <span data-ttu-id="9d27f-149">İçin **Kitaplığı**, girin _@aspnet/signalr @1_, Önizleme olmayan en son sürümü seçin.</span><span class="sxs-lookup"><span data-stu-id="9d27f-149">For **Library**, enter _@aspnet/signalr@1_, and select the latest version that isn't preview.</span></span>

  ![İstemci tarafı kitaplık iletişim - select Kütüphane ekleyin](signalr/_static/libman1.png)

* <span data-ttu-id="9d27f-151">Seçin **belirli dosyaları seçin**, genişletme *dist/tarayıcı* klasörü ve select *signalr.js* ve *signalr.min.js*.</span><span class="sxs-lookup"><span data-stu-id="9d27f-151">Select **Choose specific files**, expand the *dist/browser* folder, and select *signalr.js* and *signalr.min.js*.</span></span>

* <span data-ttu-id="9d27f-152">Ayarlama **hedef konum** için *wwwroot/lib/signalr/* seçip **yükleme**.</span><span class="sxs-lookup"><span data-stu-id="9d27f-152">Set **Target Location** to *wwwroot/lib/signalr/*, and select **Install**.</span></span>

  ![İstemci tarafı kitaplık iletişim - dosyaları seçin ve hedef Ekle](signalr/_static/libman2.png)

  <span data-ttu-id="9d27f-154">[LibMan](xref:client-side/libman/index) oluşturur bir *wwwroot/lib/signalr* klasörü ve seçili dosyaları kopyalar.</span><span class="sxs-lookup"><span data-stu-id="9d27f-154">[LibMan](xref:client-side/libman/index) creates a *wwwroot/lib/signalr* folder and copies the selected files to it.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9d27f-155">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9d27f-155">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="9d27f-156">İçinde **tümleşik Terminalini**, LibMan yüklemek için aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="9d27f-156">In the **Integrated Terminal**, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="9d27f-157">Proje klasörüne gidin (içeren *SignalRChat.csproj* dosyası).</span><span class="sxs-lookup"><span data-stu-id="9d27f-157">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="9d27f-158">SignalR istemci kitaplığı LibMan almak için aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="9d27f-158">Run the following command to get the SignalR client library by using LibMan.</span></span> <span data-ttu-id="9d27f-159">Çıktıyı görmeye önce birkaç saniye beklemeniz gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="9d27f-159">You might have to wait a few seconds before seeing output.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="9d27f-160">Parametreleri aşağıdaki seçenekleri belirtin:</span><span class="sxs-lookup"><span data-stu-id="9d27f-160">The parameters specify the following options:</span></span>
  * <span data-ttu-id="9d27f-161">Unpkg sağlayıcısını kullanın.</span><span class="sxs-lookup"><span data-stu-id="9d27f-161">Use the unpkg provider.</span></span>
  * <span data-ttu-id="9d27f-162">Dosyaları kopyalamak *wwwroot/lib/signalr* hedef.</span><span class="sxs-lookup"><span data-stu-id="9d27f-162">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="9d27f-163">Yalnızca belirtilen dosyaları kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="9d27f-163">Copy only the specified files.</span></span>

  <span data-ttu-id="9d27f-164">Çıktı aşağıdaki örnekteki gibi görünür:</span><span class="sxs-lookup"><span data-stu-id="9d27f-164">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9d27f-165">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9d27f-165">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="9d27f-166">İçinde **Terminal**, LibMan yüklemek için aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="9d27f-166">In the **Terminal**, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="9d27f-167">Proje klasörüne gidin (içeren *SignalRChat.csproj* dosyası).</span><span class="sxs-lookup"><span data-stu-id="9d27f-167">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="9d27f-168">SignalR istemci kitaplığı LibMan almak için aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="9d27f-168">Run the following command to get the SignalR client library by using LibMan.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="9d27f-169">Parametreleri aşağıdaki seçenekleri belirtin:</span><span class="sxs-lookup"><span data-stu-id="9d27f-169">The parameters specify the following options:</span></span>
  * <span data-ttu-id="9d27f-170">Unpkg sağlayıcısını kullanın.</span><span class="sxs-lookup"><span data-stu-id="9d27f-170">Use the unpkg provider.</span></span>
  * <span data-ttu-id="9d27f-171">Dosyaları kopyalamak *wwwroot/lib/signalr* hedef.</span><span class="sxs-lookup"><span data-stu-id="9d27f-171">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="9d27f-172">Yalnızca belirtilen dosyaları kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="9d27f-172">Copy only the specified files.</span></span>

  <span data-ttu-id="9d27f-173">Çıktı aşağıdaki örnekteki gibi görünür:</span><span class="sxs-lookup"><span data-stu-id="9d27f-173">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

---

## <a name="create-the-signalr-hub"></a><span data-ttu-id="9d27f-174">SignalR hub'ı oluşturma</span><span class="sxs-lookup"><span data-stu-id="9d27f-174">Create the SignalR hub</span></span>

<span data-ttu-id="9d27f-175">A [hub](xref:signalr/hubs) istemci-sunucu iletişimi işleyen bir üst düzey işlem hattı hizmet veren bir sınıftır.</span><span class="sxs-lookup"><span data-stu-id="9d27f-175">A [hub](xref:signalr/hubs) is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="9d27f-176">Oluşturma SignalRChat proje klasöründe bir *Hubs* klasör.</span><span class="sxs-lookup"><span data-stu-id="9d27f-176">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="9d27f-177">İçinde *Hubs* klasör oluşturma bir *ChatHub.cs* dosyasındaki kodu aşağıdaki kodla:</span><span class="sxs-lookup"><span data-stu-id="9d27f-177">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

  <span data-ttu-id="9d27f-178">`ChatHub` Sınıfından devralan SignalR öğesinden [Hub](/dotnet/api/microsoft.aspnetcore.signalr.hub) sınıfı.</span><span class="sxs-lookup"><span data-stu-id="9d27f-178">The `ChatHub` class inherits from the SignalR [Hub](/dotnet/api/microsoft.aspnetcore.signalr.hub) class.</span></span> <span data-ttu-id="9d27f-179">`Hub` Sınıfı, bağlantılar, grupları ve mesajlaşma yönetir.</span><span class="sxs-lookup"><span data-stu-id="9d27f-179">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="9d27f-180">`SendMessage` Yöntemi herhangi bir bağlı istemci tarafından çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="9d27f-180">The `SendMessage` method can be called by any connected client.</span></span> <span data-ttu-id="9d27f-181">Bu, tüm istemcilere alınan ileti gönderir.</span><span class="sxs-lookup"><span data-stu-id="9d27f-181">It sends the received message to all clients.</span></span> <span data-ttu-id="9d27f-182">SignalR kodudur maksimum ölçeklenebilirlik sağlamak için zaman uyumsuz.</span><span class="sxs-lookup"><span data-stu-id="9d27f-182">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="9d27f-183">Projeyi SignalR kullanacak şekilde yapılandırma</span><span class="sxs-lookup"><span data-stu-id="9d27f-183">Configure the project to use SignalR</span></span>

<span data-ttu-id="9d27f-184">SignalR sunucusu, SignalR için SignalR isteklerini iletmek için yapılandırılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="9d27f-184">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="9d27f-185">Aşağıdaki vurgulanmış kodu ekleyin *Startup.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="9d27f-185">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=7,33,52-55)]

  <span data-ttu-id="9d27f-186">Bu değişiklikler için SignalR ekler [bağımlılık ekleme](xref:fundamentals/dependency-injection) sistem ve [ara yazılım](xref:fundamentals/middleware/index) işlem hattı.</span><span class="sxs-lookup"><span data-stu-id="9d27f-186">These changes add SignalR to the [dependency injection](xref:fundamentals/dependency-injection) system and the [middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

## <a name="create-the-signalr-client-code"></a><span data-ttu-id="9d27f-187">SignalR istemci kodu oluşturma</span><span class="sxs-lookup"><span data-stu-id="9d27f-187">Create the SignalR client code</span></span>

* <span data-ttu-id="9d27f-188">İçeriği Değiştir *Pages\Index.cshtml* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="9d27f-188">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

  <span data-ttu-id="9d27f-189">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="9d27f-189">The preceding code:</span></span>

  * <span data-ttu-id="9d27f-190">Metin kutuları için adı ve ileti metni ve bir Gönder düğmesi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="9d27f-190">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="9d27f-191">İçeren bir liste oluşturur `id="messagesList"` SignalR hub'ından alınan iletileri görüntülemek için.</span><span class="sxs-lookup"><span data-stu-id="9d27f-191">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="9d27f-192">SignalR için komut dosyası başvuruları içerir ve *chat.js* sonraki adımda oluşturduğunuz uygulama kodu.</span><span class="sxs-lookup"><span data-stu-id="9d27f-192">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="9d27f-193">İçinde *wwwroot/js* klasör oluşturma bir *chat.js* dosyasındaki kodu aşağıdaki kodla:</span><span class="sxs-lookup"><span data-stu-id="9d27f-193">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

  <span data-ttu-id="9d27f-194">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="9d27f-194">The preceding code:</span></span>

  * <span data-ttu-id="9d27f-195">Oluşturur ve bir bağlantı başlatır.</span><span class="sxs-lookup"><span data-stu-id="9d27f-195">Creates and starts a connection.</span></span>
  * <span data-ttu-id="9d27f-196">Gönder düğmesine hub'a iletiler gönderen bir işleyici ekler.</span><span class="sxs-lookup"><span data-stu-id="9d27f-196">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="9d27f-197">Bağlantı nesnesi için hub'ından iletiler alan ve bunları listeye ekler bir işleyici ekler.</span><span class="sxs-lookup"><span data-stu-id="9d27f-197">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="9d27f-198">Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="9d27f-198">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9d27f-199">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9d27f-199">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="9d27f-200">Tuşuna **CTRL + F5** uygulamayı hata ayıklama olmadan çalıştırmak için.</span><span class="sxs-lookup"><span data-stu-id="9d27f-200">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9d27f-201">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9d27f-201">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="9d27f-202">Tuşuna **CTRL + F5** uygulamayı hata ayıklama olmadan çalıştırmak için.</span><span class="sxs-lookup"><span data-stu-id="9d27f-202">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9d27f-203">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9d27f-203">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="9d27f-204">Menüden **çalıştırın > hata ayıklama olmadan Başlat**.</span><span class="sxs-lookup"><span data-stu-id="9d27f-204">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="9d27f-205">Adres çubuğundan URL'yi kopyalayın, başka bir tarayıcı örneğinde veya sekmesi açın ve adres çubuğuna URL'yi yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="9d27f-205">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="9d27f-206">Ya da tarayıcı seçin, bir ad ve ileti girin ve seçin **Gönder** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="9d27f-206">Choose either browser, enter a name and message, and select the **Send** button.</span></span>

  <span data-ttu-id="9d27f-207">Her iki sayfalarında, adını ve iletisini anında görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="9d27f-207">The name and message are displayed on both pages instantly.</span></span>

  ![SignalR örnek uygulaması](signalr/_static/signalr-get-started-finished.png)

> [!TIP]
> <span data-ttu-id="9d27f-209">Uygulama işe yaramazsa, tarayıcı Geliştirici Araçları'nı (F12) açın ve konsoluna gidin.</span><span class="sxs-lookup"><span data-stu-id="9d27f-209">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="9d27f-210">HTML ve JavaScript kodunuza ilgili hatalar görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9d27f-210">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="9d27f-211">Örneğin, eklediğiniz varsayalım *signalr.js* yönlendirilmiş değerinden farklı bir klasöre.</span><span class="sxs-lookup"><span data-stu-id="9d27f-211">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="9d27f-212">Bu durumda bu dosyaya başvuru çalışmaz ve bir 404 hatası konsolunda görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="9d27f-212">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
> <span data-ttu-id="9d27f-213">![signalr.js bulunamadı hatası](signalr/_static/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="9d27f-213">![signalr.js not found error](signalr/_static/f12-console.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="9d27f-214">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="9d27f-214">Next steps</span></span>

<span data-ttu-id="9d27f-215">Farklı etki alanlarından bir SignalR uygulamalarına bağlanabilmeniz için istemcilerin istiyorsanız, çıkış noktaları arası kaynak paylaşımı (CORS) etkinleştirme gerekir.</span><span class="sxs-lookup"><span data-stu-id="9d27f-215">If you want clients to connect to a SignalR app from different domains, you have to enable Cross-Origin Resource Sharing (CORS).</span></span> <span data-ttu-id="9d27f-216">Daha fazla bilgi için [çıkış noktaları arası kaynak paylaşımı](xref:signalr/security?view=aspnetcore-2.1#cross-origin-resource-sharing).</span><span class="sxs-lookup"><span data-stu-id="9d27f-216">For more information, see [Cross-origin resource sharing](xref:signalr/security?view=aspnetcore-2.1#cross-origin-resource-sharing).</span></span>

<span data-ttu-id="9d27f-217">SignalR hub'ları ve JavaScript istemcileri hakkında daha fazla bilgi için şu kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="9d27f-217">To learn more about SignalR, hubs, and JavaScript clients, see these resources:</span></span>

* [<span data-ttu-id="9d27f-218">ASP.NET Core signalr'a giriş</span><span class="sxs-lookup"><span data-stu-id="9d27f-218">Introduction to SignalR for ASP.NET Core</span></span>](xref:signalr/introduction)
* [<span data-ttu-id="9d27f-219">ASP.NET Core signalr'da hubs'ı kullanma</span><span class="sxs-lookup"><span data-stu-id="9d27f-219">Use hubs in SignalR for ASP.NET Core</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="9d27f-220">ASP.NET Core SignalR JavaScript istemcisi</span><span class="sxs-lookup"><span data-stu-id="9d27f-220">ASP.NET Core SignalR JavaScript client</span></span>](xref:signalr/javascript-client)
