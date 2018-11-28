---
title: ASP.NET Core Signalr'yi kullanmaya başlayın
author: tdykstra
description: Bu öğreticide, ASP.NET Core SignalR kullanan bir sohbet uygulaması oluşturma.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 11/13/2018
uid: tutorials/signalr
ms.openlocfilehash: 190717dc6e6f9f2766ba92aa7472f4cdea9b6827
ms.sourcegitcommit: e7fafb153b9de7595c2558a0133f8d1c33a3bddb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2018
ms.locfileid: "52458536"
---
# <a name="tutorial-get-started-with-aspnet-core-signalr"></a><span data-ttu-id="86529-103">Öğretici: ASP.NET Core SignalR ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="86529-103">Tutorial: Get started with ASP.NET Core SignalR</span></span>

<span data-ttu-id="86529-104">Bu öğreticide SignalR kullanarak gerçek zamanlı bir uygulama oluşturmaya ilişkin temel bilgileri size öğretir.</span><span class="sxs-lookup"><span data-stu-id="86529-104">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="86529-105">Aşağıdakilerin nasıl yapıldığını öğreneceksiniz:</span><span class="sxs-lookup"><span data-stu-id="86529-105">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="86529-106">Web projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="86529-106">Create a web project.</span></span>
> * <span data-ttu-id="86529-107">SignalR istemci kitaplığı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="86529-107">Add the SignalR client library.</span></span>
> * <span data-ttu-id="86529-108">Bir SignalR hub'ı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="86529-108">Create a SignalR hub.</span></span>
> * <span data-ttu-id="86529-109">Projeyi SignalR kullanacak şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="86529-109">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="86529-110">Herhangi bir istemciden bağlanan tüm istemciler için iletileri gönderen kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="86529-110">Add code that sends messages from any client to all connected clients.</span></span>

<span data-ttu-id="86529-111">Sonunda, bir çalışma sohbet uygulaması oluşturmuş olacaksınız:</span><span class="sxs-lookup"><span data-stu-id="86529-111">At the end, you'll have a working chat app:</span></span>

![SignalR örnek uygulaması](signalr/_static/signalr-get-started-finished.png)

<span data-ttu-id="86529-113">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="86529-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

> [!NOTE]
> <span data-ttu-id="86529-114">ASP.NET Core içindekiler tablosuna yönelik önerilmiş olan yeni bir yapının kullanılabilirliğini test ediyoruz.</span><span class="sxs-lookup"><span data-stu-id="86529-114">We’re testing the usability of a proposed new structure for the ASP.NET Core table of contents.</span></span>  <span data-ttu-id="86529-115">Geçerli veya önerilen içindekiler tablosunda 7 farklı konuyu bulmaya ilişkin alıştırmayı denemek için vaktiniz varsa lütfen [çalışmaya katılmak için buraya tıklayın](https://dpk4xbh5.optimalworkshop.com/treejack/rps16hd5).</span><span class="sxs-lookup"><span data-stu-id="86529-115">If you have a few minutes to try an exercise of finding 7 different topics in the current or proposed table of contents, please [click here to participate in the study](https://dpk4xbh5.optimalworkshop.com/treejack/rps16hd5).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="86529-116">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="86529-116">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="86529-117">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="86529-117">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="86529-118">[Visual Studio 2017 sürüm 15,8 veya üzeri](https://www.visualstudio.com/downloads/) ile **ASP.NET ve web geliştirme** iş yükü</span><span class="sxs-lookup"><span data-stu-id="86529-118">[Visual Studio 2017 version 15.8 or later](https://www.visualstudio.com/downloads/) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="86529-119">.NET core SDK 2.1 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="86529-119">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="86529-120">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="86529-120">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="86529-121">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="86529-121">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="86529-122">.NET core SDK 2.1 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="86529-122">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="86529-123">Visual Studio Code için C#</span><span class="sxs-lookup"><span data-stu-id="86529-123">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="86529-124">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="86529-124">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* [<span data-ttu-id="86529-125">Sürüm 7.5.4 Mac için Visual Studio veya üzeri</span><span class="sxs-lookup"><span data-stu-id="86529-125">Visual Studio for Mac version 7.5.4 or later</span></span>](https://www.visualstudio.com/downloads/)
* <span data-ttu-id="86529-126">[.NET core SDK 2.1 veya üzeri](https://www.microsoft.com/net/download/all) (Visual Studio yüklemesine dahil)</span><span class="sxs-lookup"><span data-stu-id="86529-126">[.NET Core SDK 2.1 or later](https://www.microsoft.com/net/download/all) (included in the Visual Studio install)</span></span>

---

## <a name="create-a-web-project"></a><span data-ttu-id="86529-127">Bir web projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="86529-127">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="86529-128">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="86529-128">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="86529-129">Menüden **Dosya > Yeni proje**.</span><span class="sxs-lookup"><span data-stu-id="86529-129">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="86529-130">İçinde **yeni proje** iletişim kutusunda **yüklü > Visual C# > Web > ASP.NET Core Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="86529-130">In the **New Project** dialog, select **Installed > Visual C# > Web > ASP.NET Core Web Application**.</span></span> <span data-ttu-id="86529-131">Projeyi adlandırın *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="86529-131">Name the project *SignalRChat*.</span></span>

  ![Visual Studio'da yeni proje iletişim kutusu](signalr/_static/signalr-new-project-dialog.png)

* <span data-ttu-id="86529-133">Seçin **Web uygulaması** Razor sayfaları kullanan bir proje oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="86529-133">Select **Web Application** to create a project that uses Razor Pages.</span></span>

* <span data-ttu-id="86529-134">Bir hedef Framework'ü seçin **.NET Core**seçin **ASP.NET Core 2.1**, tıklatıp **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="86529-134">Select a target framework of **.NET Core**, select **ASP.NET Core 2.1**, and click **OK**.</span></span>

  ![Visual Studio'da yeni proje iletişim kutusu](signalr/_static/signalr-new-project-choose-type.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="86529-136">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="86529-136">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="86529-137">Açık [tümleşik Terminalini](https://code.visualstudio.com/docs/editor/integrated-terminal) yeni proje klasörü oluşturulacağı klasör.</span><span class="sxs-lookup"><span data-stu-id="86529-137">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) to the folder in which the new project folder will be created.</span></span>

* <span data-ttu-id="86529-138">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="86529-138">Run the following commands:</span></span>

   ```console
   dotnet new webapp -o SignalRChat
   code -r SignalRChat
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="86529-139">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="86529-139">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="86529-140">Menüden **Dosya > Yeni Çözüm**.</span><span class="sxs-lookup"><span data-stu-id="86529-140">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="86529-141">Seçin **.NET Core > Uygulama > ASP.NET Core Web uygulaması** (seçmeyin **ASP.NET Core Web uygulaması (MVC)**).</span><span class="sxs-lookup"><span data-stu-id="86529-141">Select **.NET Core > App > ASP.NET Core Web App** (Don't select **ASP.NET Core Web App (MVC)**).</span></span>

* <span data-ttu-id="86529-142">Seçin **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="86529-142">Select **Next**.</span></span>

* <span data-ttu-id="86529-143">Projeyi adlandırın *SignalRChat*ve ardından **Oluştur**.</span><span class="sxs-lookup"><span data-stu-id="86529-143">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="86529-144">SignalR istemci kitaplığı Ekle</span><span class="sxs-lookup"><span data-stu-id="86529-144">Add the SignalR client library</span></span>

<span data-ttu-id="86529-145">SignalR server kitaplığı dahil `Microsoft.AspNetCore.App` metapackage.</span><span class="sxs-lookup"><span data-stu-id="86529-145">The SignalR server library is included in the `Microsoft.AspNetCore.App` metapackage.</span></span> <span data-ttu-id="86529-146">JavaScript istemci kitaplığı, otomatik olarak projeye dahil değil.</span><span class="sxs-lookup"><span data-stu-id="86529-146">The JavaScript client library isn't automatically included in the project.</span></span> <span data-ttu-id="86529-147">Bu öğretici için Kitaplık Yöneticisi'ni (LibMan) istemci kitaplığını almak için kullanmak *unpkg*.</span><span class="sxs-lookup"><span data-stu-id="86529-147">For this tutorial, you use Library Manager (LibMan) to get the client library from *unpkg*.</span></span> <span data-ttu-id="86529-148">unpkg bir içerik teslim ağı (CDN) olan), teslim edebilir herhangi bir şey npm, Node.js Paket Yöneticisi bulunamadı.</span><span class="sxs-lookup"><span data-stu-id="86529-148">unpkg is a content delivery network (CDN)) that can deliver anything found in npm, the Node.js package manager.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="86529-149">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="86529-149">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="86529-150">İçinde **Çözüm Gezgini**projeye sağ tıklayıp seçin **Ekle** > **istemci tarafı kitaplık**.</span><span class="sxs-lookup"><span data-stu-id="86529-150">In **Solution Explorer**, right-click the project, and select **Add** > **Client-Side Library**.</span></span>

* <span data-ttu-id="86529-151">İçinde **istemci tarafı kitaplık Ekle** iletişim için **sağlayıcısı** seçin **unpkg**.</span><span class="sxs-lookup"><span data-stu-id="86529-151">In the **Add Client-Side Library** dialog, for **Provider** select **unpkg**.</span></span> 

* <span data-ttu-id="86529-152">İçin **Kitaplığı**, girin `@aspnet/signalr@1`, Önizleme olmayan en son sürümü seçin.</span><span class="sxs-lookup"><span data-stu-id="86529-152">For **Library**, enter `@aspnet/signalr@1`, and select the latest version that isn't preview.</span></span>

  ![İstemci tarafı kitaplık iletişim - select Kütüphane ekleyin](signalr/_static/libman1.png)

* <span data-ttu-id="86529-154">Seçin **belirli dosyaları seçin**, genişletme *dist/tarayıcı* klasörü ve select *signalr.js* ve *signalr.min.js*.</span><span class="sxs-lookup"><span data-stu-id="86529-154">Select **Choose specific files**, expand the *dist/browser* folder, and select *signalr.js* and *signalr.min.js*.</span></span>

* <span data-ttu-id="86529-155">Ayarlama **hedef konum** için *wwwroot/lib/signalr/* seçip **yükleme**.</span><span class="sxs-lookup"><span data-stu-id="86529-155">Set **Target Location** to *wwwroot/lib/signalr/*, and select **Install**.</span></span>

  ![İstemci tarafı kitaplık iletişim - dosyaları seçin ve hedef Ekle](signalr/_static/libman2.png)

  <span data-ttu-id="86529-157">LibMan oluşturur bir *wwwroot/lib/signalr* klasörü ve seçili dosyaları kopyalar.</span><span class="sxs-lookup"><span data-stu-id="86529-157">LibMan creates a *wwwroot/lib/signalr* folder and copies the selected files to it.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="86529-158">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="86529-158">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="86529-159">Tümleşik terminalde LibMan yüklemek için aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="86529-159">In the integrated terminal, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="86529-160">SignalR istemci kitaplığı LibMan almak için aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="86529-160">Run the following command to get the SignalR client library by using LibMan.</span></span> <span data-ttu-id="86529-161">Çıktıyı görmeye önce birkaç saniye beklemeniz gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="86529-161">You might have to wait a few seconds before seeing output.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="86529-162">Parametreleri aşağıdaki seçenekleri belirtin:</span><span class="sxs-lookup"><span data-stu-id="86529-162">The parameters specify the following options:</span></span>
  * <span data-ttu-id="86529-163">Unpkg sağlayıcısını kullanın.</span><span class="sxs-lookup"><span data-stu-id="86529-163">Use the unpkg provider.</span></span>
  * <span data-ttu-id="86529-164">Dosyaları kopyalamak *wwwroot/lib/signalr* hedef.</span><span class="sxs-lookup"><span data-stu-id="86529-164">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="86529-165">Yalnızca belirtilen dosyaları kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="86529-165">Copy only the specified files.</span></span>

  <span data-ttu-id="86529-166">Çıktı aşağıdaki örnekteki gibi görünür:</span><span class="sxs-lookup"><span data-stu-id="86529-166">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="86529-167">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="86529-167">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="86529-168">İçinde **Terminal**, LibMan yüklemek için aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="86529-168">In the **Terminal**, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="86529-169">Proje klasörüne gidin (içeren *SignalRChat.csproj* dosyası).</span><span class="sxs-lookup"><span data-stu-id="86529-169">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="86529-170">SignalR istemci kitaplığı LibMan almak için aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="86529-170">Run the following command to get the SignalR client library by using LibMan.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="86529-171">Parametreleri aşağıdaki seçenekleri belirtin:</span><span class="sxs-lookup"><span data-stu-id="86529-171">The parameters specify the following options:</span></span>
  * <span data-ttu-id="86529-172">Unpkg sağlayıcısını kullanın.</span><span class="sxs-lookup"><span data-stu-id="86529-172">Use the unpkg provider.</span></span>
  * <span data-ttu-id="86529-173">Dosyaları kopyalamak *wwwroot/lib/signalr* hedef.</span><span class="sxs-lookup"><span data-stu-id="86529-173">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="86529-174">Yalnızca belirtilen dosyaları kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="86529-174">Copy only the specified files.</span></span>

  <span data-ttu-id="86529-175">Çıktı aşağıdaki örnekteki gibi görünür:</span><span class="sxs-lookup"><span data-stu-id="86529-175">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

---

## <a name="create-a-signalr-hub"></a><span data-ttu-id="86529-176">Bir SignalR hub'ı oluşturma</span><span class="sxs-lookup"><span data-stu-id="86529-176">Create a SignalR hub</span></span>

<span data-ttu-id="86529-177">A *hub* istemci-sunucu iletişimi işleyen bir üst düzey işlem hattı hizmet veren bir sınıftır.</span><span class="sxs-lookup"><span data-stu-id="86529-177">A *hub* is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="86529-178">Oluşturma SignalRChat proje klasöründe bir *Hubs* klasör.</span><span class="sxs-lookup"><span data-stu-id="86529-178">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="86529-179">İçinde *Hubs* klasör oluşturma bir *ChatHub.cs* dosyasındaki kodu aşağıdaki kodla:</span><span class="sxs-lookup"><span data-stu-id="86529-179">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

  <span data-ttu-id="86529-180">`ChatHub` Sınıfından devralan SignalR öğesinden `Hub` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="86529-180">The `ChatHub` class inherits from the SignalR `Hub` class.</span></span> <span data-ttu-id="86529-181">`Hub` Sınıfı, bağlantılar, grupları ve mesajlaşma yönetir.</span><span class="sxs-lookup"><span data-stu-id="86529-181">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="86529-182">`SendMessage` Yöntemi herhangi bir bağlı istemci tarafından çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="86529-182">The `SendMessage` method can be called by any connected client.</span></span> <span data-ttu-id="86529-183">Bu, tüm istemcilere alınan ileti gönderir.</span><span class="sxs-lookup"><span data-stu-id="86529-183">It sends the received message to all clients.</span></span> <span data-ttu-id="86529-184">SignalR kodudur maksimum ölçeklenebilirlik sağlamak için zaman uyumsuz.</span><span class="sxs-lookup"><span data-stu-id="86529-184">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-signalr"></a><span data-ttu-id="86529-185">SignalR yapılandırın</span><span class="sxs-lookup"><span data-stu-id="86529-185">Configure SignalR</span></span>

<span data-ttu-id="86529-186">SignalR sunucusu, SignalR için SignalR isteklerini iletmek için yapılandırılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="86529-186">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="86529-187">Aşağıdaki vurgulanmış kodu ekleyin *Startup.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="86529-187">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=7,33,52-55)]

  <span data-ttu-id="86529-188">Bu değişiklikler, ASP.NET Core bağımlılık ekleme sistemi ve ara yazılım ardışık düzenini SignalR ekleyin.</span><span class="sxs-lookup"><span data-stu-id="86529-188">These changes add SignalR to the ASP.NET Core dependency injection system and the middleware pipeline.</span></span>

## <a name="add-signalr-client-code"></a><span data-ttu-id="86529-189">SignalR istemci kodu ekleyin</span><span class="sxs-lookup"><span data-stu-id="86529-189">Add SignalR client code</span></span>

* <span data-ttu-id="86529-190">İçeriği Değiştir *Pages\Index.cshtml* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="86529-190">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

  <span data-ttu-id="86529-191">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="86529-191">The preceding code:</span></span>

  * <span data-ttu-id="86529-192">Metin kutuları için adı ve ileti metni ve bir Gönder düğmesi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="86529-192">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="86529-193">İçeren bir liste oluşturur `id="messagesList"` SignalR hub'ından alınan iletileri görüntülemek için.</span><span class="sxs-lookup"><span data-stu-id="86529-193">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="86529-194">SignalR için komut dosyası başvuruları içerir ve *chat.js* sonraki adımda oluşturduğunuz uygulama kodu.</span><span class="sxs-lookup"><span data-stu-id="86529-194">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="86529-195">İçinde *wwwroot/js* klasör oluşturma bir *chat.js* dosyasındaki kodu aşağıdaki kodla:</span><span class="sxs-lookup"><span data-stu-id="86529-195">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

  <span data-ttu-id="86529-196">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="86529-196">The preceding code:</span></span>

  * <span data-ttu-id="86529-197">Oluşturur ve bir bağlantı başlatır.</span><span class="sxs-lookup"><span data-stu-id="86529-197">Creates and starts a connection.</span></span>
  * <span data-ttu-id="86529-198">Gönder düğmesine hub'a iletiler gönderen bir işleyici ekler.</span><span class="sxs-lookup"><span data-stu-id="86529-198">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="86529-199">Bağlantı nesnesi için hub'ından iletiler alan ve bunları listeye ekler bir işleyici ekler.</span><span class="sxs-lookup"><span data-stu-id="86529-199">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="86529-200">Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="86529-200">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="86529-201">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="86529-201">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="86529-202">Tuşuna **CTRL + F5** uygulamayı hata ayıklama olmadan çalıştırmak için.</span><span class="sxs-lookup"><span data-stu-id="86529-202">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="86529-203">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="86529-203">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="86529-204">Tümleşik terminalde aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="86529-204">In the integrated terminal, run the following command:</span></span>

  ```console
  dotnet run -p SignalRChat.csproj
  ```
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="86529-205">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="86529-205">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="86529-206">Menüden **çalıştırın > hata ayıklama olmadan Başlat**.</span><span class="sxs-lookup"><span data-stu-id="86529-206">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="86529-207">Adres çubuğundan URL'yi kopyalayın, başka bir tarayıcı örneğinde veya sekmesi açın ve adres çubuğuna URL'yi yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="86529-207">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="86529-208">Ya da tarayıcı seçin, bir ad ve ileti girin ve seçin **iletisi gönder** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="86529-208">Choose either browser, enter a name and message, and select the **Send Message** button.</span></span>

  <span data-ttu-id="86529-209">Her iki sayfalarında, adını ve iletisini anında görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="86529-209">The name and message are displayed on both pages instantly.</span></span>

  ![SignalR örnek uygulaması](signalr/_static/signalr-get-started-finished.png)

> [!TIP]
> <span data-ttu-id="86529-211">Uygulama işe yaramazsa, tarayıcı Geliştirici Araçları'nı (F12) açın ve konsoluna gidin.</span><span class="sxs-lookup"><span data-stu-id="86529-211">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="86529-212">HTML ve JavaScript kodunuza ilgili hatalar görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="86529-212">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="86529-213">Örneğin, eklediğiniz varsayalım *signalr.js* yönlendirilmiş değerinden farklı bir klasöre.</span><span class="sxs-lookup"><span data-stu-id="86529-213">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="86529-214">Bu durumda bu dosyaya başvuru çalışmaz ve bir 404 hatası konsolunda görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="86529-214">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
> <span data-ttu-id="86529-215">![signalr.js bulunamadı hatası](signalr/_static/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="86529-215">![signalr.js not found error](signalr/_static/f12-console.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="86529-216">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="86529-216">Next steps</span></span>

<span data-ttu-id="86529-217">Bu öğreticide şunları öğrendiniz: nasıl yapılır:</span><span class="sxs-lookup"><span data-stu-id="86529-217">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="86529-218">Bir web uygulaması projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="86529-218">Create a web app project.</span></span>
> * <span data-ttu-id="86529-219">SignalR istemci kitaplığı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="86529-219">Add the SignalR client library.</span></span>
> * <span data-ttu-id="86529-220">Bir SignalR hub'ı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="86529-220">Create a SignalR hub.</span></span>
> * <span data-ttu-id="86529-221">Projeyi SignalR kullanacak şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="86529-221">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="86529-222">Bağlanan tüm istemciler için herhangi bir istemciden ileti göndermek için hub'ı kullanan kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="86529-222">Add code that uses the hub to send messages from any client to all connected clients.</span></span>

<span data-ttu-id="86529-223">SignalR hakkında daha fazla bilgi için girişine bakın:</span><span class="sxs-lookup"><span data-stu-id="86529-223">To learn more about SignalR, see the introduction:</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="86529-224">ASP.NET Core signalr'a giriş</span><span class="sxs-lookup"><span data-stu-id="86529-224">Introduction to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
