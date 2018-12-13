---
title: ASP.NET Core Signalr'yi kullanmaya başlayın
author: tdykstra
description: Bu öğreticide, ASP.NET Core SignalR kullanan bir sohbet uygulaması oluşturma.
ms.author: tdykstra
ms.custom: mvc
ms.date: 11/30/2018
uid: tutorials/signalr
ms.openlocfilehash: 904031c58b06f12d41902802f8ab3927b29ae94b
ms.sourcegitcommit: a16352c1c88a71770ab3922200a8cd148fb278a6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/13/2018
ms.locfileid: "53335357"
---
# <a name="tutorial-get-started-with-aspnet-core-signalr"></a><span data-ttu-id="0d845-103">Öğretici: ASP.NET Core Signalr'yi kullanmaya başlayın</span><span class="sxs-lookup"><span data-stu-id="0d845-103">Tutorial: Get started with ASP.NET Core SignalR</span></span>

<span data-ttu-id="0d845-104">Bu öğreticide SignalR kullanarak gerçek zamanlı bir uygulama oluşturmaya ilişkin temel bilgileri size öğretir.</span><span class="sxs-lookup"><span data-stu-id="0d845-104">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="0d845-105">Aşağıdakilerin nasıl yapıldığını öğreneceksiniz:</span><span class="sxs-lookup"><span data-stu-id="0d845-105">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="0d845-106">Web projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="0d845-106">Create a web project.</span></span>
> * <span data-ttu-id="0d845-107">SignalR istemci kitaplığı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="0d845-107">Add the SignalR client library.</span></span>
> * <span data-ttu-id="0d845-108">Bir SignalR hub'ı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="0d845-108">Create a SignalR hub.</span></span>
> * <span data-ttu-id="0d845-109">Projeyi SignalR kullanacak şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="0d845-109">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="0d845-110">Herhangi bir istemciden bağlanan tüm istemciler için iletileri gönderen kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="0d845-110">Add code that sends messages from any client to all connected clients.</span></span>

<span data-ttu-id="0d845-111">Sonunda, bir çalışma sohbet uygulaması oluşturmuş olacaksınız:</span><span class="sxs-lookup"><span data-stu-id="0d845-111">At the end, you'll have a working chat app:</span></span>

![SignalR örnek uygulaması](signalr/_static/signalr-get-started-finished.png)

<span data-ttu-id="0d845-113">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="0d845-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

[!INCLUDE [|Prerequisites](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-web-project"></a><span data-ttu-id="0d845-114">Bir web projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="0d845-114">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0d845-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0d845-115">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="0d845-116">Menüden **Dosya > Yeni proje**.</span><span class="sxs-lookup"><span data-stu-id="0d845-116">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="0d845-117">İçinde **yeni proje** iletişim kutusunda **yüklü > Visual C# > Web > ASP.NET Core Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="0d845-117">In the **New Project** dialog, select **Installed > Visual C# > Web > ASP.NET Core Web Application**.</span></span> <span data-ttu-id="0d845-118">Projeyi adlandırın *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="0d845-118">Name the project *SignalRChat*.</span></span>

  ![Visual Studio'da yeni proje iletişim kutusu](signalr/_static/signalr-new-project-dialog.png)

* <span data-ttu-id="0d845-120">Seçin **Web uygulaması** Razor sayfaları kullanan bir proje oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="0d845-120">Select **Web Application** to create a project that uses Razor Pages.</span></span>

* <span data-ttu-id="0d845-121">Bir hedef Framework'ü seçin **.NET Core**seçin **ASP.NET Core 2.2**, tıklatıp **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="0d845-121">Select a target framework of **.NET Core**, select **ASP.NET Core 2.2**, and click **OK**.</span></span>

  ![Visual Studio'da yeni proje iletişim kutusu](signalr/_static/signalr-new-project-choose-type.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0d845-123">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0d845-123">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="0d845-124">Açık [tümleşik Terminalini](https://code.visualstudio.com/docs/editor/integrated-terminal) yeni proje klasörü oluşturulacağı klasör.</span><span class="sxs-lookup"><span data-stu-id="0d845-124">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) to the folder in which the new project folder will be created.</span></span>

* <span data-ttu-id="0d845-125">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="0d845-125">Run the following commands:</span></span>

   ```console
   dotnet new webapp -o SignalRChat
   code -r SignalRChat
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="0d845-126">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0d845-126">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="0d845-127">Menüden **Dosya > Yeni Çözüm**.</span><span class="sxs-lookup"><span data-stu-id="0d845-127">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="0d845-128">Seçin **.NET Core > Uygulama > ASP.NET Core Web uygulaması** (seçmeyin **ASP.NET Core Web uygulaması (MVC)**).</span><span class="sxs-lookup"><span data-stu-id="0d845-128">Select **.NET Core > App > ASP.NET Core Web App** (Don't select **ASP.NET Core Web App (MVC)**).</span></span>

* <span data-ttu-id="0d845-129">**İleri**’yi seçin.</span><span class="sxs-lookup"><span data-stu-id="0d845-129">Select **Next**.</span></span>

* <span data-ttu-id="0d845-130">Projeyi adlandırın *SignalRChat*ve ardından **Oluştur**.</span><span class="sxs-lookup"><span data-stu-id="0d845-130">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="0d845-131">SignalR istemci kitaplığı Ekle</span><span class="sxs-lookup"><span data-stu-id="0d845-131">Add the SignalR client library</span></span>

<span data-ttu-id="0d845-132">SignalR server kitaplığı dahil `Microsoft.AspNetCore.App` metapackage.</span><span class="sxs-lookup"><span data-stu-id="0d845-132">The SignalR server library is included in the `Microsoft.AspNetCore.App` metapackage.</span></span> <span data-ttu-id="0d845-133">JavaScript istemci kitaplığı, otomatik olarak projeye dahil değil.</span><span class="sxs-lookup"><span data-stu-id="0d845-133">The JavaScript client library isn't automatically included in the project.</span></span> <span data-ttu-id="0d845-134">Bu öğretici için Kitaplık Yöneticisi'ni (LibMan) istemci kitaplığını almak için kullanmak *unpkg*.</span><span class="sxs-lookup"><span data-stu-id="0d845-134">For this tutorial, you use Library Manager (LibMan) to get the client library from *unpkg*.</span></span> <span data-ttu-id="0d845-135">unpkg bir içerik teslim ağı (CDN) olan), teslim edebilir herhangi bir şey npm, Node.js Paket Yöneticisi bulunamadı.</span><span class="sxs-lookup"><span data-stu-id="0d845-135">unpkg is a content delivery network (CDN)) that can deliver anything found in npm, the Node.js package manager.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0d845-136">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0d845-136">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="0d845-137">İçinde **Çözüm Gezgini**projeye sağ tıklayıp seçin **Ekle** > **istemci tarafı kitaplık**.</span><span class="sxs-lookup"><span data-stu-id="0d845-137">In **Solution Explorer**, right-click the project, and select **Add** > **Client-Side Library**.</span></span>

* <span data-ttu-id="0d845-138">İçinde **istemci tarafı kitaplık Ekle** iletişim için **sağlayıcısı** seçin **unpkg**.</span><span class="sxs-lookup"><span data-stu-id="0d845-138">In the **Add Client-Side Library** dialog, for **Provider** select **unpkg**.</span></span> 

* <span data-ttu-id="0d845-139">İçin **Kitaplığı**, girin `@aspnet/signalr@1`, Önizleme olmayan en son sürümü seçin.</span><span class="sxs-lookup"><span data-stu-id="0d845-139">For **Library**, enter `@aspnet/signalr@1`, and select the latest version that isn't preview.</span></span>

  ![İstemci tarafı kitaplık iletişim - select Kütüphane ekleyin](signalr/_static/libman1.png)

* <span data-ttu-id="0d845-141">Seçin **belirli dosyaları seçin**, genişletme *dist/tarayıcı* klasörü ve select *signalr.js* ve *signalr.min.js*.</span><span class="sxs-lookup"><span data-stu-id="0d845-141">Select **Choose specific files**, expand the *dist/browser* folder, and select *signalr.js* and *signalr.min.js*.</span></span>

* <span data-ttu-id="0d845-142">Ayarlama **hedef konum** için *wwwroot/lib/signalr/* seçip **yükleme**.</span><span class="sxs-lookup"><span data-stu-id="0d845-142">Set **Target Location** to *wwwroot/lib/signalr/*, and select **Install**.</span></span>

  ![İstemci tarafı kitaplık iletişim - dosyaları seçin ve hedef Ekle](signalr/_static/libman2.png)

  <span data-ttu-id="0d845-144">LibMan oluşturur bir *wwwroot/lib/signalr* klasörü ve seçili dosyaları kopyalar.</span><span class="sxs-lookup"><span data-stu-id="0d845-144">LibMan creates a *wwwroot/lib/signalr* folder and copies the selected files to it.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0d845-145">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0d845-145">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="0d845-146">Tümleşik terminalde LibMan yüklemek için aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="0d845-146">In the integrated terminal, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="0d845-147">SignalR istemci kitaplığı LibMan almak için aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="0d845-147">Run the following command to get the SignalR client library by using LibMan.</span></span> <span data-ttu-id="0d845-148">Çıktıyı görmeye önce birkaç saniye beklemeniz gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="0d845-148">You might have to wait a few seconds before seeing output.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="0d845-149">Parametreleri aşağıdaki seçenekleri belirtin:</span><span class="sxs-lookup"><span data-stu-id="0d845-149">The parameters specify the following options:</span></span>
  * <span data-ttu-id="0d845-150">Unpkg sağlayıcısını kullanın.</span><span class="sxs-lookup"><span data-stu-id="0d845-150">Use the unpkg provider.</span></span>
  * <span data-ttu-id="0d845-151">Dosyaları kopyalamak *wwwroot/lib/signalr* hedef.</span><span class="sxs-lookup"><span data-stu-id="0d845-151">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="0d845-152">Yalnızca belirtilen dosyaları kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="0d845-152">Copy only the specified files.</span></span>

  <span data-ttu-id="0d845-153">Çıktı aşağıdaki örnekteki gibi görünür:</span><span class="sxs-lookup"><span data-stu-id="0d845-153">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="0d845-154">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0d845-154">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="0d845-155">İçinde **Terminal**, LibMan yüklemek için aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="0d845-155">In the **Terminal**, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="0d845-156">Proje klasörüne gidin (içeren *SignalRChat.csproj* dosyası).</span><span class="sxs-lookup"><span data-stu-id="0d845-156">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="0d845-157">SignalR istemci kitaplığı LibMan almak için aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="0d845-157">Run the following command to get the SignalR client library by using LibMan.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="0d845-158">Parametreleri aşağıdaki seçenekleri belirtin:</span><span class="sxs-lookup"><span data-stu-id="0d845-158">The parameters specify the following options:</span></span>
  * <span data-ttu-id="0d845-159">Unpkg sağlayıcısını kullanın.</span><span class="sxs-lookup"><span data-stu-id="0d845-159">Use the unpkg provider.</span></span>
  * <span data-ttu-id="0d845-160">Dosyaları kopyalamak *wwwroot/lib/signalr* hedef.</span><span class="sxs-lookup"><span data-stu-id="0d845-160">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="0d845-161">Yalnızca belirtilen dosyaları kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="0d845-161">Copy only the specified files.</span></span>

  <span data-ttu-id="0d845-162">Çıktı aşağıdaki örnekteki gibi görünür:</span><span class="sxs-lookup"><span data-stu-id="0d845-162">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

---

## <a name="create-a-signalr-hub"></a><span data-ttu-id="0d845-163">Bir SignalR hub'ı oluşturma</span><span class="sxs-lookup"><span data-stu-id="0d845-163">Create a SignalR hub</span></span>

<span data-ttu-id="0d845-164">A *hub* istemci-sunucu iletişimi işleyen bir üst düzey işlem hattı hizmet veren bir sınıftır.</span><span class="sxs-lookup"><span data-stu-id="0d845-164">A *hub* is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="0d845-165">Oluşturma SignalRChat proje klasöründe bir *Hubs* klasör.</span><span class="sxs-lookup"><span data-stu-id="0d845-165">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="0d845-166">İçinde *Hubs* klasör oluşturma bir *ChatHub.cs* dosyasındaki kodu aşağıdaki kodla:</span><span class="sxs-lookup"><span data-stu-id="0d845-166">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

  <span data-ttu-id="0d845-167">`ChatHub` Sınıfından devralan SignalR öğesinden `Hub` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="0d845-167">The `ChatHub` class inherits from the SignalR `Hub` class.</span></span> <span data-ttu-id="0d845-168">`Hub` Sınıfı, bağlantılar, grupları ve mesajlaşma yönetir.</span><span class="sxs-lookup"><span data-stu-id="0d845-168">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="0d845-169">`SendMessage` Yöntemi herhangi bir bağlı istemci tarafından çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="0d845-169">The `SendMessage` method can be called by any connected client.</span></span> <span data-ttu-id="0d845-170">Bu, tüm istemcilere alınan ileti gönderir.</span><span class="sxs-lookup"><span data-stu-id="0d845-170">It sends the received message to all clients.</span></span> <span data-ttu-id="0d845-171">SignalR kodudur maksimum ölçeklenebilirlik sağlamak için zaman uyumsuz.</span><span class="sxs-lookup"><span data-stu-id="0d845-171">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-signalr"></a><span data-ttu-id="0d845-172">SignalR yapılandırın</span><span class="sxs-lookup"><span data-stu-id="0d845-172">Configure SignalR</span></span>

<span data-ttu-id="0d845-173">SignalR sunucusu, SignalR için SignalR isteklerini iletmek için yapılandırılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="0d845-173">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="0d845-174">Aşağıdaki vurgulanmış kodu ekleyin *Startup.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="0d845-174">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=7,33,52-55)]

  <span data-ttu-id="0d845-175">Bu değişiklikler, ASP.NET Core bağımlılık ekleme sistemi ve ara yazılım ardışık düzenini SignalR ekleyin.</span><span class="sxs-lookup"><span data-stu-id="0d845-175">These changes add SignalR to the ASP.NET Core dependency injection system and the middleware pipeline.</span></span>

## <a name="add-signalr-client-code"></a><span data-ttu-id="0d845-176">SignalR istemci kodu ekleyin</span><span class="sxs-lookup"><span data-stu-id="0d845-176">Add SignalR client code</span></span>

* <span data-ttu-id="0d845-177">İçeriği Değiştir *Pages\Index.cshtml* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="0d845-177">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

  <span data-ttu-id="0d845-178">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="0d845-178">The preceding code:</span></span>

  * <span data-ttu-id="0d845-179">Metin kutuları için adı ve ileti metni ve bir Gönder düğmesi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="0d845-179">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="0d845-180">İçeren bir liste oluşturur `id="messagesList"` SignalR hub'ından alınan iletileri görüntülemek için.</span><span class="sxs-lookup"><span data-stu-id="0d845-180">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="0d845-181">SignalR için komut dosyası başvuruları içerir ve *chat.js* sonraki adımda oluşturduğunuz uygulama kodu.</span><span class="sxs-lookup"><span data-stu-id="0d845-181">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="0d845-182">İçinde *wwwroot/js* klasör oluşturma bir *chat.js* dosyasındaki kodu aşağıdaki kodla:</span><span class="sxs-lookup"><span data-stu-id="0d845-182">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

  <span data-ttu-id="0d845-183">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="0d845-183">The preceding code:</span></span>

  * <span data-ttu-id="0d845-184">Oluşturur ve bir bağlantı başlatır.</span><span class="sxs-lookup"><span data-stu-id="0d845-184">Creates and starts a connection.</span></span>
  * <span data-ttu-id="0d845-185">Gönder düğmesine hub'a iletiler gönderen bir işleyici ekler.</span><span class="sxs-lookup"><span data-stu-id="0d845-185">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="0d845-186">Bağlantı nesnesi için hub'ından iletiler alan ve bunları listeye ekler bir işleyici ekler.</span><span class="sxs-lookup"><span data-stu-id="0d845-186">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="0d845-187">Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="0d845-187">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0d845-188">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0d845-188">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="0d845-189">Tuşuna **CTRL + F5** uygulamayı hata ayıklama olmadan çalıştırmak için.</span><span class="sxs-lookup"><span data-stu-id="0d845-189">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0d845-190">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0d845-190">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="0d845-191">Tümleşik terminalde aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="0d845-191">In the integrated terminal, run the following command:</span></span>

  ```console
  dotnet run -p SignalRChat.csproj
  ```
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="0d845-192">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0d845-192">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="0d845-193">Menüden **çalıştırın > hata ayıklama olmadan Başlat**.</span><span class="sxs-lookup"><span data-stu-id="0d845-193">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="0d845-194">Adres çubuğundan URL'yi kopyalayın, başka bir tarayıcı örneğinde veya sekmesi açın ve adres çubuğuna URL'yi yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="0d845-194">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="0d845-195">Ya da tarayıcı seçin, bir ad ve ileti girin ve seçin **iletisi gönder** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="0d845-195">Choose either browser, enter a name and message, and select the **Send Message** button.</span></span>

  <span data-ttu-id="0d845-196">Her iki sayfalarında, adını ve iletisini anında görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="0d845-196">The name and message are displayed on both pages instantly.</span></span>

  ![SignalR örnek uygulaması](signalr/_static/signalr-get-started-finished.png)

> [!TIP]
> <span data-ttu-id="0d845-198">Uygulama işe yaramazsa, tarayıcı Geliştirici Araçları'nı (F12) açın ve konsoluna gidin.</span><span class="sxs-lookup"><span data-stu-id="0d845-198">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="0d845-199">HTML ve JavaScript kodunuza ilgili hatalar görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0d845-199">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="0d845-200">Örneğin, eklediğiniz varsayalım *signalr.js* yönlendirilmiş değerinden farklı bir klasöre.</span><span class="sxs-lookup"><span data-stu-id="0d845-200">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="0d845-201">Bu durumda bu dosyaya başvuru çalışmaz ve bir 404 hatası konsolunda görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="0d845-201">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
> <span data-ttu-id="0d845-202">![signalr.js bulunamadı hatası](signalr/_static/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="0d845-202">![signalr.js not found error](signalr/_static/f12-console.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="0d845-203">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="0d845-203">Next steps</span></span>

<span data-ttu-id="0d845-204">Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:</span><span class="sxs-lookup"><span data-stu-id="0d845-204">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="0d845-205">Bir web uygulaması projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="0d845-205">Create a web app project.</span></span>
> * <span data-ttu-id="0d845-206">SignalR istemci kitaplığı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="0d845-206">Add the SignalR client library.</span></span>
> * <span data-ttu-id="0d845-207">Bir SignalR hub'ı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="0d845-207">Create a SignalR hub.</span></span>
> * <span data-ttu-id="0d845-208">Projeyi SignalR kullanacak şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="0d845-208">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="0d845-209">Bağlanan tüm istemciler için herhangi bir istemciden ileti göndermek için hub'ı kullanan kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="0d845-209">Add code that uses the hub to send messages from any client to all connected clients.</span></span>

<span data-ttu-id="0d845-210">SignalR hakkında daha fazla bilgi için girişine bakın:</span><span class="sxs-lookup"><span data-stu-id="0d845-210">To learn more about SignalR, see the introduction:</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="0d845-211">ASP.NET Core signalr'a giriş</span><span class="sxs-lookup"><span data-stu-id="0d845-211">Introduction to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
