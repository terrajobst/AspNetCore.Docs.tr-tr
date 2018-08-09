---
title: 'Öğretici: SignalR üzerinde ASP.NET Core ile çalışmaya başlama'
author: tdykstra
description: Bu öğreticide, ASP.NET Core için SignalR kullanan bir sohbet uygulaması oluşturma.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/08/2018
uid: tutorials/signalr
ms.openlocfilehash: 2c1c46b4a608eb0d39287a5261ed7c1f847a644e
ms.sourcegitcommit: 29dfe436f54a27fbb4f6494bc639d16c75001fab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2018
ms.locfileid: "39722483"
---
# <a name="tutorial-get-started-with-signalr-on-aspnet-core"></a><span data-ttu-id="c4d57-103">Öğretici: SignalR üzerinde ASP.NET Core ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="c4d57-103">Tutorial: Get started with SignalR on ASP.NET Core</span></span>

<span data-ttu-id="c4d57-104">Bu öğreticide SignalR kullanarak gerçek zamanlı bir uygulama oluşturmaya ilişkin temel bilgileri size öğretir.</span><span class="sxs-lookup"><span data-stu-id="c4d57-104">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="c4d57-105">Aşağıdakilerin nasıl yapıldığını öğreneceksiniz:</span><span class="sxs-lookup"><span data-stu-id="c4d57-105">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c4d57-106">SignalR üzerinde ASP.NET Core kullanan bir web uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="c4d57-106">Create a web app that uses SignalR on ASP.NET Core.</span></span>
> * <span data-ttu-id="c4d57-107">Bir SignalR hub'ı sunucuda oluşturun.</span><span class="sxs-lookup"><span data-stu-id="c4d57-107">Create a SignalR hub on the server.</span></span>
> * <span data-ttu-id="c4d57-108">SignalR hub'ına JavaScript istemcilerinden bağlanın.</span><span class="sxs-lookup"><span data-stu-id="c4d57-108">Connect to the SignalR hub from JavaScript clients.</span></span>
> * <span data-ttu-id="c4d57-109">Bağlanan tüm istemciler için herhangi bir istemciden ileti göndermek için hub'ı kullanın.</span><span class="sxs-lookup"><span data-stu-id="c4d57-109">Use the hub to send messages from any client to all connected clients.</span></span>

<span data-ttu-id="c4d57-110">Sonunda bir çalışma sohbet uygulaması oluşturmuş olacaksınız:</span><span class="sxs-lookup"><span data-stu-id="c4d57-110">At the end you'll have a working chat app:</span></span>

![SignalR örnek uygulaması](signalr/_static/signalr-get-started-finished.png)

<span data-ttu-id="c4d57-112">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="c4d57-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c4d57-113">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="c4d57-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c4d57-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c4d57-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="c4d57-115">[Visual Studio 2017 sürüm 15.7.3 veya üzeri](https://www.visualstudio.com/downloads/) ile **ASP.NET ve web geliştirme** iş yükü</span><span class="sxs-lookup"><span data-stu-id="c4d57-115">[Visual Studio 2017 version 15.7.3 or later](https://www.visualstudio.com/downloads/) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="c4d57-116">.NET core SDK 2.1 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="c4d57-116">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="c4d57-117">[npm](https://www.npmjs.com/get-npm) (SignalR JavaScript istemci kitaplığı için kullanılan Node.js için Paket Yöneticisi.)</span><span class="sxs-lookup"><span data-stu-id="c4d57-117">[npm](https://www.npmjs.com/get-npm) (Package manager for Node.js, used for the SignalR JavaScript client library.)</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c4d57-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c4d57-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="c4d57-119">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c4d57-119">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="c4d57-120">.NET core SDK 2.1 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="c4d57-120">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="c4d57-121">Visual Studio Code için C#</span><span class="sxs-lookup"><span data-stu-id="c4d57-121">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* <span data-ttu-id="c4d57-122">[npm](https://www.npmjs.com/get-npm) (SignalR JavaScript istemci kitaplığı için kullanılan Node.js için Paket Yöneticisi.)</span><span class="sxs-lookup"><span data-stu-id="c4d57-122">[npm](https://www.npmjs.com/get-npm) (Package manager for Node.js, used for the SignalR JavaScript client library.)</span></span>

---

## <a name="create-the-project"></a><span data-ttu-id="c4d57-123">Projeyi oluşturma</span><span class="sxs-lookup"><span data-stu-id="c4d57-123">Create the project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c4d57-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c4d57-124">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="c4d57-125">Menüden **Dosya > Yeni proje**.</span><span class="sxs-lookup"><span data-stu-id="c4d57-125">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="c4d57-126">İçinde **yeni proje** iletişim kutusunda **yüklü > Visual C# > Web > ASP.NET Core Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="c4d57-126">In the **New Project** dialog, select **Installed > Visual C# > Web > ASP.NET Core Web Application**.</span></span> <span data-ttu-id="c4d57-127">Projeyi adlandırın *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="c4d57-127">Name the project *SignalRChat*.</span></span>

  ![Visual Studio'da yeni proje iletişim kutusu](signalr/_static/signalr-new-project-dialog.png)

* <span data-ttu-id="c4d57-129">Seçin **Web uygulaması** Razor sayfaları kullanan bir proje oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="c4d57-129">Select **Web Application** to create a project that uses Razor Pages.</span></span>

* <span data-ttu-id="c4d57-130">Hedef Framework'ü olduğundan emin olun **ASP.NET Core 2.1**ve ardından **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="c4d57-130">Make sure that the target framework is **ASP.NET Core 2.1**, and then select **OK**.</span></span> 

  ![Visual Studio'da yeni proje iletişim kutusu](signalr/_static/signalr-new-project-choose-type.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c4d57-132">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c4d57-132">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="c4d57-133">Yeni bir proje için kullanabileceğiniz bir klasöre açın.</span><span class="sxs-lookup"><span data-stu-id="c4d57-133">Open a folder that you can use for a new project.</span></span>

* <span data-ttu-id="c4d57-134">İçinde **tümleşik Terminalini**, aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="c4d57-134">In the **Integrated Terminal**, run the following command:</span></span>

   ```console
   dotnet new webapp -o SignalRChat
   ```

   [!INCLUDE[](~/includes/webapp-alias-notice.md)]

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="c4d57-135">SignalR istemci kitaplığı Ekle</span><span class="sxs-lookup"><span data-stu-id="c4d57-135">Add the SignalR client library</span></span>

<span data-ttu-id="c4d57-136">SignalR server kitaplığı dahil [Microsoft.AspnetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="c4d57-136">The SignalR server library is included in the [Microsoft.AspnetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="c4d57-137">Ancak Node.js Paket Yöneticisi olan npm'den JavaScript istemci kitaplığını alma gerekir.</span><span class="sxs-lookup"><span data-stu-id="c4d57-137">But you have to get the JavaScript client library from npm, the Node.js package manager.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c4d57-138">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c4d57-138">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="c4d57-139">İçinde **Paket Yöneticisi Konsolu** (PMC) proje klasörüne değiştirin (içeren *SignalRChat.csproj* dosyası).</span><span class="sxs-lookup"><span data-stu-id="c4d57-139">In **Package Manager Console** (PMC), change to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

  ```console
  cd SignalRChat
  ``` 

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c4d57-140">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c4d57-140">Visual Studio Code</span></span>](#tab/visual-studio-code/)

2. <span data-ttu-id="c4d57-141">Yeni proje klasörüne geçin.</span><span class="sxs-lookup"><span data-stu-id="c4d57-141">Change to the new project folder.</span></span>

  ```console
  cd SignalRChat
  ``` 

---

* <span data-ttu-id="c4d57-142">Oluşturmak için npm Başlatıcı çalıştırma bir *package.json* dosyası:</span><span class="sxs-lookup"><span data-stu-id="c4d57-142">Run the npm initializer to create a *package.json* file:</span></span>

  ```console
  npm init -y
  ```

  <span data-ttu-id="c4d57-143">Komut çıktısı aşağıdaki örneğe benzer oluşturur:</span><span class="sxs-lookup"><span data-stu-id="c4d57-143">The command creates output similar to the following example:</span></span>

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

* <span data-ttu-id="c4d57-144">İstemci Kitaplığı paketi yükleyin:</span><span class="sxs-lookup"><span data-stu-id="c4d57-144">Install the client library package:</span></span>

  ```console
  npm install @aspnet/signalr
  ```

  <span data-ttu-id="c4d57-145">Komut çıktısı aşağıdaki örneğe benzer oluşturur:</span><span class="sxs-lookup"><span data-stu-id="c4d57-145">The command creates output similar to the following example:</span></span>

  ```
  npm : npm notice created a lockfile as package-lock.json. You should commit this file.
  At line:1 char:1
  + npm install @aspnet/signalr
  + ~~~~~~~~~~~~~~~~~~~~~~~~~~~
      + CategoryInfo          : NotSpecified: (npm notice crea...mmit this file.:String) [], RemoteException
      + FullyQualifiedErrorId : NativeCommandError
  WARN
   SignalRChat@1.0.0 No description
  WARN
   SignalRChat@1.0.0 No repository field.
  + @aspnet/signalr@1.0.2
  added 1 package in 1.398s
  ```

<span data-ttu-id="c4d57-146">`npm install` Altında bir alt komut indirdiğiniz JavaScript istemci Kitaplığı *node_modules*.</span><span class="sxs-lookup"><span data-stu-id="c4d57-146">The `npm install` command downloaded the JavaScript client library to a subfolder under *node_modules*.</span></span> <span data-ttu-id="c4d57-147">Buradan altında bir klasöre kopyalayın *wwwroot* , sohbet uygulaması web sayfasından başvurabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c4d57-147">Copy it from there to a folder under *wwwroot* that you can reference from the chat app web page.</span></span>

* <span data-ttu-id="c4d57-148">Oluşturma bir *signalr* klasöründe *wwwroot/lib*.</span><span class="sxs-lookup"><span data-stu-id="c4d57-148">Create a *signalr* folder in *wwwroot/lib*.</span></span>

* <span data-ttu-id="c4d57-149">Kopyalama *signalr.js* dosya *node_modules/@aspnet/signalr/dist/browser* yeni *wwwroot/lib/signalr* klasör.</span><span class="sxs-lookup"><span data-stu-id="c4d57-149">Copy the *signalr.js* file from *node_modules/@aspnet/signalr/dist/browser* to the new *wwwroot/lib/signalr* folder.</span></span>

## <a name="create-the-signalr-hub"></a><span data-ttu-id="c4d57-150">SignalR hub'ı oluşturma</span><span class="sxs-lookup"><span data-stu-id="c4d57-150">Create the SignalR hub</span></span>

<span data-ttu-id="c4d57-151">A [hub](xref:signalr/hubs) istemci-sunucu iletişimi işleyen bir üst düzey işlem hattı hizmet veren bir sınıftır.</span><span class="sxs-lookup"><span data-stu-id="c4d57-151">A [hub](xref:signalr/hubs) is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="c4d57-152">Oluşturma SignalRChat proje klasöründe bir *Hubs* klasör.</span><span class="sxs-lookup"><span data-stu-id="c4d57-152">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="c4d57-153">İçinde *Hubs* klasör oluşturma bir *ChatHub.cs* dosyasındaki kodu aşağıdaki kodla:</span><span class="sxs-lookup"><span data-stu-id="c4d57-153">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

  <span data-ttu-id="c4d57-154">`ChatHub` Sınıfından devralan SignalR öğesinden [Hub](/dotnet/api/microsoft.aspnetcore.signalr.hub) sınıfı.</span><span class="sxs-lookup"><span data-stu-id="c4d57-154">The `ChatHub` class inherits from the SignalR [Hub](/dotnet/api/microsoft.aspnetcore.signalr.hub) class.</span></span> <span data-ttu-id="c4d57-155">`Hub` Sınıfı, bağlantılar, grupları ve mesajlaşma yönetir.</span><span class="sxs-lookup"><span data-stu-id="c4d57-155">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="c4d57-156">`SendMessage` Yöntemi herhangi bir bağlı istemci tarafından çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="c4d57-156">The `SendMessage` method can be called by any connected client.</span></span> <span data-ttu-id="c4d57-157">Bu, tüm istemcilere alınan ileti gönderir.</span><span class="sxs-lookup"><span data-stu-id="c4d57-157">It sends the received message to all clients.</span></span> <span data-ttu-id="c4d57-158">SignalR kodudur maksimum ölçeklenebilirlik sağlamak için zaman uyumsuz.</span><span class="sxs-lookup"><span data-stu-id="c4d57-158">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="c4d57-159">Projeyi SignalR kullanacak şekilde yapılandırma</span><span class="sxs-lookup"><span data-stu-id="c4d57-159">Configure the project to use SignalR</span></span>

<span data-ttu-id="c4d57-160">SignalR sunucusu, SignalR için SignalR isteklerini iletmek için yapılandırılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="c4d57-160">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="c4d57-161">Aşağıdaki vurgulanmış kodu ekleyin *Startup.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="c4d57-161">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=7,33,52-55)]

  <span data-ttu-id="c4d57-162">Bu değişiklikler için SignalR ekler [bağımlılık ekleme](xref:fundamentals/dependency-injection) sistem ve [ara yazılım](xref:fundamentals/middleware/index) işlem hattı.</span><span class="sxs-lookup"><span data-stu-id="c4d57-162">These changes add SignalR to the the [dependency injection](xref:fundamentals/dependency-injection) system and the [middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

## <a name="create-the-signalr-client-code"></a><span data-ttu-id="c4d57-163">SignalR istemci kodu oluşturma</span><span class="sxs-lookup"><span data-stu-id="c4d57-163">Create the SignalR client code</span></span>

* <span data-ttu-id="c4d57-164">İçeriği Değiştir *Pages\Index.cshtml* aşağıdaki:</span><span class="sxs-lookup"><span data-stu-id="c4d57-164">Replace the content in *Pages\Index.cshtml* with the following:</span></span>

  [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

  <span data-ttu-id="c4d57-165">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="c4d57-165">The preceding code:</span></span>

  * <span data-ttu-id="c4d57-166">Metin kutuları için adı ve ileti metni ve bir Gönder düğmesi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="c4d57-166">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="c4d57-167">İçeren bir liste oluşturur `id="messagesList"` SignalR hub'ından alınan iletileri görüntülemek için.</span><span class="sxs-lookup"><span data-stu-id="c4d57-167">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="c4d57-168">SignalR için komut dosyası başvuruları içerir ve *chat.js* sonraki adımda oluşturduğunuz uygulama kodu.</span><span class="sxs-lookup"><span data-stu-id="c4d57-168">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="c4d57-169">İçinde *wwwroot/js* klasör oluşturma bir *chat.js* dosyasındaki kodu aşağıdaki kodla:</span><span class="sxs-lookup"><span data-stu-id="c4d57-169">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

  <span data-ttu-id="c4d57-170">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="c4d57-170">The preceding code:</span></span>

  * <span data-ttu-id="c4d57-171">Oluşturur ve bir bağlantı başlatır.</span><span class="sxs-lookup"><span data-stu-id="c4d57-171">Creates and starts a connection.</span></span>
  * <span data-ttu-id="c4d57-172">Gönder düğmesine hub'a iletiler gönderen bir işleyici ekler.</span><span class="sxs-lookup"><span data-stu-id="c4d57-172">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="c4d57-173">Bağlantı nesnesi için hub'ından iletiler alan ve bunları listeye ekler bir işleyici ekler.</span><span class="sxs-lookup"><span data-stu-id="c4d57-173">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="c4d57-174">Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="c4d57-174">Run the app</span></span>

* <span data-ttu-id="c4d57-175">Tuşuna **CTRL + F5** uygulamayı hata ayıklama olmadan çalıştırmak için.</span><span class="sxs-lookup"><span data-stu-id="c4d57-175">Press **CTRL+F5** to run the app without debugging.</span></span>

* <span data-ttu-id="c4d57-176">Adres çubuğundan URL'yi kopyalayın, başka bir tarayıcı örneğinde veya sekmesi açın ve adres çubuğuna URL'yi yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="c4d57-176">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="c4d57-177">Ya da tarayıcı seçin, bir ad ve ileti girin ve seçin **Gönder** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="c4d57-177">Choose either browser, enter a name and message, and select the **Send** button.</span></span>

  <span data-ttu-id="c4d57-178">Her iki sayfalarında, adını ve iletisini anında görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="c4d57-178">The name and message are displayed on both pages instantly.</span></span>

  ![SignalR örnek uygulaması](signalr/_static/signalr-get-started-finished.png)

> [!TIP]
> <span data-ttu-id="c4d57-180">Uygulama işe yaramazsa, tarayıcı Geliştirici Araçları'nı (F12) açın ve konsoluna gidin.</span><span class="sxs-lookup"><span data-stu-id="c4d57-180">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="c4d57-181">HTML ve JavaScript kodunuza ilgili hatalar görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c4d57-181">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="c4d57-182">Örneğin, eklediğiniz varsayalım *signalr.js* yönlendirilmiş değerinden farklı bir klasöre.</span><span class="sxs-lookup"><span data-stu-id="c4d57-182">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="c4d57-183">Bu durumda bu dosyaya başvuru çalışmaz ve bir 404 hatası konsolunda görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="c4d57-183">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
> <span data-ttu-id="c4d57-184">![signalr.js bulunamadı hatası](signalr/_static/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="c4d57-184">![signalr.js not found error](signalr/_static/f12-console.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="c4d57-185">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="c4d57-185">Next steps</span></span>

<span data-ttu-id="c4d57-186">Farklı etki alanlarından bir SignalR uygulamalarına bağlanabilmeniz için istemcilerin istiyorsanız, çıkış noktaları arası kaynak paylaşımı (CORS) etkinleştirme gerekir.</span><span class="sxs-lookup"><span data-stu-id="c4d57-186">If you want clients to connect to a SignalR app from different domains, you have to enable Cross-Origin Resource Sharing (CORS).</span></span> <span data-ttu-id="c4d57-187">Daha fazla bilgi için [çıkış noktaları arası kaynak paylaşımı](xref:signalr/security?view=aspnetcore-2.1#cross-origin-resource-sharing).</span><span class="sxs-lookup"><span data-stu-id="c4d57-187">For more information, see [Cross-origin resource sharing](xref:signalr/security?view=aspnetcore-2.1#cross-origin-resource-sharing).</span></span>

<span data-ttu-id="c4d57-188">SignalR hub'ları ve JavaScript istemcileri hakkında daha fazla bilgi için şu kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="c4d57-188">To learn more about SignalR, hubs, and JavaScript clients, see these resources:</span></span>

* [<span data-ttu-id="c4d57-189">ASP.NET Core signalr'a giriş</span><span class="sxs-lookup"><span data-stu-id="c4d57-189">Introduction to SignalR for ASP.NET Core</span></span>](xref:signalr/introduction)
* [<span data-ttu-id="c4d57-190">ASP.NET Core signalr'da hubs'ı kullanma</span><span class="sxs-lookup"><span data-stu-id="c4d57-190">Use hubs in SignalR for ASP.NET Core</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="c4d57-191">ASP.NET Core SignalR JavaScript istemcisi</span><span class="sxs-lookup"><span data-stu-id="c4d57-191">ASP.NET Core SignalR JavaScript client</span></span>](xref:signalr/javascript-client)
