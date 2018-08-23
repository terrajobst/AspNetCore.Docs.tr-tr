---
title: 'Öğretici: SignalR üzerinde ASP.NET Core ile çalışmaya başlama'
author: tdykstra
description: Bu öğreticide, ASP.NET Core için SignalR kullanan bir sohbet uygulaması oluşturma.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/20/2018
uid: tutorials/signalr
ms.openlocfilehash: db7f31963f6a4280069f1f4f82a547e2879e64bb
ms.sourcegitcommit: d27317c16f113e7c111583042ec7e4c5a26adf6f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2018
ms.locfileid: "41756364"
---
# <a name="tutorial-get-started-with-signalr-on-aspnet-core"></a><span data-ttu-id="b9431-103">Öğretici: SignalR üzerinde ASP.NET Core ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="b9431-103">Tutorial: Get started with SignalR on ASP.NET Core</span></span>

<span data-ttu-id="b9431-104">Bu öğreticide SignalR kullanarak gerçek zamanlı bir uygulama oluşturmaya ilişkin temel bilgileri size öğretir.</span><span class="sxs-lookup"><span data-stu-id="b9431-104">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="b9431-105">Aşağıdakilerin nasıl yapıldığını öğreneceksiniz:</span><span class="sxs-lookup"><span data-stu-id="b9431-105">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b9431-106">SignalR üzerinde ASP.NET Core kullanan bir web uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="b9431-106">Create a web app that uses SignalR on ASP.NET Core.</span></span>
> * <span data-ttu-id="b9431-107">Bir SignalR hub'ı sunucuda oluşturun.</span><span class="sxs-lookup"><span data-stu-id="b9431-107">Create a SignalR hub on the server.</span></span>
> * <span data-ttu-id="b9431-108">SignalR hub'ına JavaScript istemcilerinden bağlanın.</span><span class="sxs-lookup"><span data-stu-id="b9431-108">Connect to the SignalR hub from JavaScript clients.</span></span>
> * <span data-ttu-id="b9431-109">Bağlanan tüm istemciler için herhangi bir istemciden ileti göndermek için hub'ı kullanın.</span><span class="sxs-lookup"><span data-stu-id="b9431-109">Use the hub to send messages from any client to all connected clients.</span></span>

<span data-ttu-id="b9431-110">Sonunda, bir çalışma sohbet uygulaması oluşturmuş olacaksınız:</span><span class="sxs-lookup"><span data-stu-id="b9431-110">At the end, you'll have a working chat app:</span></span>

![SignalR örnek uygulaması](signalr/_static/signalr-get-started-finished.png)

<span data-ttu-id="b9431-112">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="b9431-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b9431-113">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="b9431-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b9431-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b9431-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="b9431-115">[Visual Studio 2017 sürüm 15.7.3 veya üzeri](https://www.visualstudio.com/downloads/) ile **ASP.NET ve web geliştirme** iş yükü</span><span class="sxs-lookup"><span data-stu-id="b9431-115">[Visual Studio 2017 version 15.7.3 or later](https://www.visualstudio.com/downloads/) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="b9431-116">.NET core SDK 2.1 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="b9431-116">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="b9431-117">[npm](https://www.npmjs.com/get-npm) (SignalR JavaScript istemci kitaplığı için kullanılan Node.js için Paket Yöneticisi.)</span><span class="sxs-lookup"><span data-stu-id="b9431-117">[npm](https://www.npmjs.com/get-npm) (Package manager for Node.js, used for the SignalR JavaScript client library.)</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b9431-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b9431-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="b9431-119">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b9431-119">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="b9431-120">.NET core SDK 2.1 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="b9431-120">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="b9431-121">Visual Studio Code için C#</span><span class="sxs-lookup"><span data-stu-id="b9431-121">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* <span data-ttu-id="b9431-122">[npm](https://www.npmjs.com/get-npm) (SignalR JavaScript istemci kitaplığı için kullanılan Node.js için Paket Yöneticisi.)</span><span class="sxs-lookup"><span data-stu-id="b9431-122">[npm](https://www.npmjs.com/get-npm) (Package manager for Node.js, used for the SignalR JavaScript client library.)</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="b9431-123">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b9431-123">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* [<span data-ttu-id="b9431-124">Sürüm 7.5.4 Mac için Visual Studio veya üzeri</span><span class="sxs-lookup"><span data-stu-id="b9431-124">Visual Studio for Mac version 7.5.4 or later</span></span>](https://www.visualstudio.com/downloads/)
* <span data-ttu-id="b9431-125">[.NET core SDK 2.1 veya üzeri](https://www.microsoft.com/net/download/all) (Visual Studio yüklemesine dahil)</span><span class="sxs-lookup"><span data-stu-id="b9431-125">[.NET Core SDK 2.1 or later](https://www.microsoft.com/net/download/all) (included in the Visual Studio install)</span></span>
* <span data-ttu-id="b9431-126">[npm](https://www.npmjs.com/get-npm) (SignalR JavaScript istemci kitaplığı için kullanılan Node.js için Paket Yöneticisi.)</span><span class="sxs-lookup"><span data-stu-id="b9431-126">[npm](https://www.npmjs.com/get-npm) (Package manager for Node.js, used for the SignalR JavaScript client library.)</span></span>

---

## <a name="create-the-project"></a><span data-ttu-id="b9431-127">Projeyi oluşturma</span><span class="sxs-lookup"><span data-stu-id="b9431-127">Create the project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b9431-128">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b9431-128">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="b9431-129">Menüden **Dosya > Yeni proje**.</span><span class="sxs-lookup"><span data-stu-id="b9431-129">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="b9431-130">İçinde **yeni proje** iletişim kutusunda **yüklü > Visual C# > Web > ASP.NET Core Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="b9431-130">In the **New Project** dialog, select **Installed > Visual C# > Web > ASP.NET Core Web Application**.</span></span> <span data-ttu-id="b9431-131">Projeyi adlandırın *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="b9431-131">Name the project *SignalRChat*.</span></span>

  ![Visual Studio'da yeni proje iletişim kutusu](signalr/_static/signalr-new-project-dialog.png)

* <span data-ttu-id="b9431-133">Seçin **Web uygulaması** Razor sayfaları kullanan bir proje oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="b9431-133">Select **Web Application** to create a project that uses Razor Pages.</span></span>

* <span data-ttu-id="b9431-134">Bir hedef Framework'ü seçin **.NET Core**seçin **ASP.NET Core 2.1**, tıklatıp **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="b9431-134">Select a target framework of **.NET Core**, select **ASP.NET Core 2.1**, and click **OK**.</span></span>

  ![Visual Studio'da yeni proje iletişim kutusu](signalr/_static/signalr-new-project-choose-type.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b9431-136">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b9431-136">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="b9431-137">Yeni bir proje için kullanabileceğiniz bir klasöre açın.</span><span class="sxs-lookup"><span data-stu-id="b9431-137">Open a folder that you can use for a new project.</span></span>

* <span data-ttu-id="b9431-138">İçinde **tümleşik Terminalini**, aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="b9431-138">In the **Integrated Terminal**, run the following command:</span></span>

   ```console
   dotnet new webapp -o SignalRChat
   ```

   [!INCLUDE[](~/includes/webapp-alias-notice.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="b9431-139">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b9431-139">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="b9431-140">Menüden **Dosya > Yeni Çözüm**.</span><span class="sxs-lookup"><span data-stu-id="b9431-140">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="b9431-141">Seçin **.NET Core > Uygulama > ASP.NET Core Web uygulaması** (seçmeyin **ASP.NET Core Web uygulaması (MVC)**).</span><span class="sxs-lookup"><span data-stu-id="b9431-141">Select **.NET Core > App > ASP.NET Core Web App** (Don't select **ASP.NET Core Web App (MVC)**).</span></span>

* <span data-ttu-id="b9431-142">Seçin **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="b9431-142">Select **Next**.</span></span>

* <span data-ttu-id="b9431-143">Projeyi adlandırın *SignalRChat*ve ardından **Oluştur**.</span><span class="sxs-lookup"><span data-stu-id="b9431-143">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="b9431-144">SignalR istemci kitaplığı Ekle</span><span class="sxs-lookup"><span data-stu-id="b9431-144">Add the SignalR client library</span></span>

<span data-ttu-id="b9431-145">SignalR server kitaplığı dahil [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="b9431-145">The SignalR server library is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="b9431-146">Ancak Node.js Paket Yöneticisi olan npm'den JavaScript istemci kitaplığını alma gerekir.</span><span class="sxs-lookup"><span data-stu-id="b9431-146">But you have to get the JavaScript client library from npm, the Node.js package manager.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b9431-147">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b9431-147">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="b9431-148">İçinde **Paket Yöneticisi Konsolu** (PMC) proje klasörüne değiştirin (içeren *SignalRChat.csproj* dosyası).</span><span class="sxs-lookup"><span data-stu-id="b9431-148">In **Package Manager Console** (PMC), change to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

  ```console
  cd SignalRChat
  ```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b9431-149">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b9431-149">Visual Studio Code</span></span>](#tab/visual-studio-code/)

2. <span data-ttu-id="b9431-150">Yeni proje klasörüne geçin.</span><span class="sxs-lookup"><span data-stu-id="b9431-150">Change to the new project folder.</span></span>

  ```console
  cd SignalRChat
  ``` 

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="b9431-151">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b9431-151">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="b9431-152">İçinde **Terminal**, proje klasörüne gidin (içeren *SignalRChat.csproj* dosyası).</span><span class="sxs-lookup"><span data-stu-id="b9431-152">In the **Terminal**, navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

---

* <span data-ttu-id="b9431-153">Oluşturmak için npm Başlatıcı çalıştırma bir *package.json* dosyası:</span><span class="sxs-lookup"><span data-stu-id="b9431-153">Run the npm initializer to create a *package.json* file:</span></span>

  ```console
  npm init -y
  ```

  <span data-ttu-id="b9431-154">Komut çıktısı aşağıdaki örneğe benzer oluşturur:</span><span class="sxs-lookup"><span data-stu-id="b9431-154">The command creates output similar to the following example:</span></span>

  ```console
  Wrote to C:\tmp\SignalRChat\package.json:
  {
    "name": "SignalRChat",
    "version": "1.0.0",
    "description": "",
    "main": "index.js",
    "scripts": {
      "test": "echo \"Error: no test specified\" && exit 1"
    },
    "keywords": [],
    "author": "",
    "license": "ISC"0
  }
  ```

* <span data-ttu-id="b9431-155">İstemci Kitaplığı paketi yükleyin:</span><span class="sxs-lookup"><span data-stu-id="b9431-155">Install the client library package:</span></span>

  ```console
  npm install @aspnet/signalr
  ```

  <span data-ttu-id="b9431-156">Komut çıktısı aşağıdaki örneğe benzer oluşturur:</span><span class="sxs-lookup"><span data-stu-id="b9431-156">The command creates output similar to the following example:</span></span>

  ```
  npm notice created a lockfile as package-lock.json. You should commit this file.
  npm WARN signalrchat@1.0.0 No description
  npm WARN signalrchat@1.0.0 No repository field.

  + @aspnet/signalr@1.0.2
  added 1 package in 0.98s
  ```

<span data-ttu-id="b9431-157">`npm install` Altında bir alt komut indirdiğiniz JavaScript istemci Kitaplığı *node_modules*.</span><span class="sxs-lookup"><span data-stu-id="b9431-157">The `npm install` command downloaded the JavaScript client library to a subfolder under *node_modules*.</span></span> <span data-ttu-id="b9431-158">Buradan altında bir klasöre kopyalayın *wwwroot* , sohbet uygulaması web sayfasından başvurabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b9431-158">Copy it from there to a folder under *wwwroot* that you can reference from the chat app web page.</span></span>

* <span data-ttu-id="b9431-159">Oluşturma bir *signalr* klasöründe *wwwroot/lib*.</span><span class="sxs-lookup"><span data-stu-id="b9431-159">Create a *signalr* folder in *wwwroot/lib*.</span></span>

* <span data-ttu-id="b9431-160">Kopyalama *signalr.js* dosya *node_modules/@aspnet/signalr/dist/browser* yeni *wwwroot/lib/signalr* klasör.</span><span class="sxs-lookup"><span data-stu-id="b9431-160">Copy the *signalr.js* file from *node_modules/@aspnet/signalr/dist/browser* to the new *wwwroot/lib/signalr* folder.</span></span>

## <a name="create-the-signalr-hub"></a><span data-ttu-id="b9431-161">SignalR hub'ı oluşturma</span><span class="sxs-lookup"><span data-stu-id="b9431-161">Create the SignalR hub</span></span>

<span data-ttu-id="b9431-162">A [hub](xref:signalr/hubs) istemci-sunucu iletişimi işleyen bir üst düzey işlem hattı hizmet veren bir sınıftır.</span><span class="sxs-lookup"><span data-stu-id="b9431-162">A [hub](xref:signalr/hubs) is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="b9431-163">Oluşturma SignalRChat proje klasöründe bir *Hubs* klasör.</span><span class="sxs-lookup"><span data-stu-id="b9431-163">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="b9431-164">İçinde *Hubs* klasör oluşturma bir *ChatHub.cs* dosyasındaki kodu aşağıdaki kodla:</span><span class="sxs-lookup"><span data-stu-id="b9431-164">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

  <span data-ttu-id="b9431-165">`ChatHub` Sınıfından devralan SignalR öğesinden [Hub](/dotnet/api/microsoft.aspnetcore.signalr.hub) sınıfı.</span><span class="sxs-lookup"><span data-stu-id="b9431-165">The `ChatHub` class inherits from the SignalR [Hub](/dotnet/api/microsoft.aspnetcore.signalr.hub) class.</span></span> <span data-ttu-id="b9431-166">`Hub` Sınıfı, bağlantılar, grupları ve mesajlaşma yönetir.</span><span class="sxs-lookup"><span data-stu-id="b9431-166">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="b9431-167">`SendMessage` Yöntemi herhangi bir bağlı istemci tarafından çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="b9431-167">The `SendMessage` method can be called by any connected client.</span></span> <span data-ttu-id="b9431-168">Bu, tüm istemcilere alınan ileti gönderir.</span><span class="sxs-lookup"><span data-stu-id="b9431-168">It sends the received message to all clients.</span></span> <span data-ttu-id="b9431-169">SignalR kodudur maksimum ölçeklenebilirlik sağlamak için zaman uyumsuz.</span><span class="sxs-lookup"><span data-stu-id="b9431-169">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="b9431-170">Projeyi SignalR kullanacak şekilde yapılandırma</span><span class="sxs-lookup"><span data-stu-id="b9431-170">Configure the project to use SignalR</span></span>

<span data-ttu-id="b9431-171">SignalR sunucusu, SignalR için SignalR isteklerini iletmek için yapılandırılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="b9431-171">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="b9431-172">Aşağıdaki vurgulanmış kodu ekleyin *Startup.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="b9431-172">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=7,33,52-55)]

  <span data-ttu-id="b9431-173">Bu değişiklikler için SignalR ekler [bağımlılık ekleme](xref:fundamentals/dependency-injection) sistem ve [ara yazılım](xref:fundamentals/middleware/index) işlem hattı.</span><span class="sxs-lookup"><span data-stu-id="b9431-173">These changes add SignalR to the [dependency injection](xref:fundamentals/dependency-injection) system and the [middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

## <a name="create-the-signalr-client-code"></a><span data-ttu-id="b9431-174">SignalR istemci kodu oluşturma</span><span class="sxs-lookup"><span data-stu-id="b9431-174">Create the SignalR client code</span></span>

* <span data-ttu-id="b9431-175">İçeriği Değiştir *Pages\Index.cshtml* aşağıdaki:</span><span class="sxs-lookup"><span data-stu-id="b9431-175">Replace the content in *Pages\Index.cshtml* with the following:</span></span>

  [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

  <span data-ttu-id="b9431-176">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="b9431-176">The preceding code:</span></span>

  * <span data-ttu-id="b9431-177">Metin kutuları için adı ve ileti metni ve bir Gönder düğmesi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="b9431-177">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="b9431-178">İçeren bir liste oluşturur `id="messagesList"` SignalR hub'ından alınan iletileri görüntülemek için.</span><span class="sxs-lookup"><span data-stu-id="b9431-178">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="b9431-179">SignalR için komut dosyası başvuruları içerir ve *chat.js* sonraki adımda oluşturduğunuz uygulama kodu.</span><span class="sxs-lookup"><span data-stu-id="b9431-179">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="b9431-180">İçinde *wwwroot/js* klasör oluşturma bir *chat.js* dosyasındaki kodu aşağıdaki kodla:</span><span class="sxs-lookup"><span data-stu-id="b9431-180">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

  <span data-ttu-id="b9431-181">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="b9431-181">The preceding code:</span></span>

  * <span data-ttu-id="b9431-182">Oluşturur ve bir bağlantı başlatır.</span><span class="sxs-lookup"><span data-stu-id="b9431-182">Creates and starts a connection.</span></span>
  * <span data-ttu-id="b9431-183">Gönder düğmesine hub'a iletiler gönderen bir işleyici ekler.</span><span class="sxs-lookup"><span data-stu-id="b9431-183">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="b9431-184">Bağlantı nesnesi için hub'ından iletiler alan ve bunları listeye ekler bir işleyici ekler.</span><span class="sxs-lookup"><span data-stu-id="b9431-184">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="b9431-185">Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="b9431-185">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b9431-186">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b9431-186">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="b9431-187">Tuşuna **CTRL + F5** uygulamayı hata ayıklama olmadan çalıştırmak için.</span><span class="sxs-lookup"><span data-stu-id="b9431-187">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b9431-188">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b9431-188">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="b9431-189">Tuşuna **CTRL + F5** uygulamayı hata ayıklama olmadan çalıştırmak için.</span><span class="sxs-lookup"><span data-stu-id="b9431-189">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="b9431-190">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b9431-190">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="b9431-191">Menüden **çalıştırın > hata ayıklama olmadan Başlat**.</span><span class="sxs-lookup"><span data-stu-id="b9431-191">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="b9431-192">Adres çubuğundan URL'yi kopyalayın, başka bir tarayıcı örneğinde veya sekmesi açın ve adres çubuğuna URL'yi yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="b9431-192">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="b9431-193">Ya da tarayıcı seçin, bir ad ve ileti girin ve seçin **Gönder** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="b9431-193">Choose either browser, enter a name and message, and select the **Send** button.</span></span>

  <span data-ttu-id="b9431-194">Her iki sayfalarında, adını ve iletisini anında görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="b9431-194">The name and message are displayed on both pages instantly.</span></span>

  ![SignalR örnek uygulaması](signalr/_static/signalr-get-started-finished.png)

> [!TIP]
> <span data-ttu-id="b9431-196">Uygulama işe yaramazsa, tarayıcı Geliştirici Araçları'nı (F12) açın ve konsoluna gidin.</span><span class="sxs-lookup"><span data-stu-id="b9431-196">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="b9431-197">HTML ve JavaScript kodunuza ilgili hatalar görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b9431-197">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="b9431-198">Örneğin, eklediğiniz varsayalım *signalr.js* yönlendirilmiş değerinden farklı bir klasöre.</span><span class="sxs-lookup"><span data-stu-id="b9431-198">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="b9431-199">Bu durumda bu dosyaya başvuru çalışmaz ve bir 404 hatası konsolunda görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="b9431-199">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
> <span data-ttu-id="b9431-200">![signalr.js bulunamadı hatası](signalr/_static/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="b9431-200">![signalr.js not found error](signalr/_static/f12-console.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="b9431-201">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="b9431-201">Next steps</span></span>

<span data-ttu-id="b9431-202">Farklı etki alanlarından bir SignalR uygulamalarına bağlanabilmeniz için istemcilerin istiyorsanız, çıkış noktaları arası kaynak paylaşımı (CORS) etkinleştirme gerekir.</span><span class="sxs-lookup"><span data-stu-id="b9431-202">If you want clients to connect to a SignalR app from different domains, you have to enable Cross-Origin Resource Sharing (CORS).</span></span> <span data-ttu-id="b9431-203">Daha fazla bilgi için [çıkış noktaları arası kaynak paylaşımı](xref:signalr/security?view=aspnetcore-2.1#cross-origin-resource-sharing).</span><span class="sxs-lookup"><span data-stu-id="b9431-203">For more information, see [Cross-origin resource sharing](xref:signalr/security?view=aspnetcore-2.1#cross-origin-resource-sharing).</span></span>

<span data-ttu-id="b9431-204">SignalR hub'ları ve JavaScript istemcileri hakkında daha fazla bilgi için şu kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="b9431-204">To learn more about SignalR, hubs, and JavaScript clients, see these resources:</span></span>

* [<span data-ttu-id="b9431-205">ASP.NET Core signalr'a giriş</span><span class="sxs-lookup"><span data-stu-id="b9431-205">Introduction to SignalR for ASP.NET Core</span></span>](xref:signalr/introduction)
* [<span data-ttu-id="b9431-206">ASP.NET Core signalr'da hubs'ı kullanma</span><span class="sxs-lookup"><span data-stu-id="b9431-206">Use hubs in SignalR for ASP.NET Core</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="b9431-207">ASP.NET Core SignalR JavaScript istemcisi</span><span class="sxs-lookup"><span data-stu-id="b9431-207">ASP.NET Core SignalR JavaScript client</span></span>](xref:signalr/javascript-client)
