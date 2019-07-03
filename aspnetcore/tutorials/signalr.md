---
title: ASP.NET Core Signalr'yi kullanmaya başlayın
author: bradygaster
description: Bu öğreticide, ASP.NET Core SignalR kullanan bir sohbet uygulaması oluşturma.
ms.author: bradyg
ms.custom: mvc
ms.date: 11/30/2018
uid: tutorials/signalr
ms.openlocfilehash: 9a4296550a17ac2c348f2406e9f5b39877b02b59
ms.sourcegitcommit: d6e51c60439f03a8992bda70cc982ddb15d3f100
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67555925"
---
# <a name="tutorial-get-started-with-aspnet-core-signalr"></a><span data-ttu-id="e14da-103">Öğretici: ASP.NET Core Signalr'yi kullanmaya başlayın</span><span class="sxs-lookup"><span data-stu-id="e14da-103">Tutorial: Get started with ASP.NET Core SignalR</span></span>

<span data-ttu-id="e14da-104">Bu öğreticide SignalR kullanarak gerçek zamanlı bir uygulama oluşturmaya ilişkin temel bilgileri size öğretir.</span><span class="sxs-lookup"><span data-stu-id="e14da-104">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="e14da-105">Aşağıdakilerin nasıl yapıldığını öğreneceksiniz:</span><span class="sxs-lookup"><span data-stu-id="e14da-105">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="e14da-106">Web projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="e14da-106">Create a web project.</span></span>
> * <span data-ttu-id="e14da-107">SignalR istemci kitaplığı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="e14da-107">Add the SignalR client library.</span></span>
> * <span data-ttu-id="e14da-108">Bir SignalR hub'ı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="e14da-108">Create a SignalR hub.</span></span>
> * <span data-ttu-id="e14da-109">Projeyi SignalR kullanacak şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="e14da-109">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="e14da-110">Herhangi bir istemciden bağlanan tüm istemciler için iletileri gönderen kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="e14da-110">Add code that sends messages from any client to all connected clients.</span></span>

<span data-ttu-id="e14da-111">Sonunda, bir çalışma sohbet uygulaması oluşturmuş olacaksınız:</span><span class="sxs-lookup"><span data-stu-id="e14da-111">At the end, you'll have a working chat app:</span></span>

![SignalR örnek uygulaması](signalr/_static/signalr-get-started-finished.png)

<span data-ttu-id="e14da-113">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="e14da-113">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e14da-114">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="e14da-114">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e14da-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e14da-115">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2017-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e14da-116">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e14da-116">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e14da-117">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e14da-117">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="e14da-118">Bir web projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="e14da-118">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e14da-119">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e14da-119">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="e14da-120">Menüden **Dosya > Yeni proje**.</span><span class="sxs-lookup"><span data-stu-id="e14da-120">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="e14da-121">İçinde **yeni proje** iletişim kutusunda **yüklü > Visual C# > Web > ASP.NET Core Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="e14da-121">In the **New Project** dialog, select **Installed > Visual C# > Web > ASP.NET Core Web Application**.</span></span> <span data-ttu-id="e14da-122">Projeyi adlandırın *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="e14da-122">Name the project *SignalRChat*.</span></span>

  ![Visual Studio'da yeni proje iletişim kutusu](signalr/_static/signalr-new-project-dialog.png)

* <span data-ttu-id="e14da-124">Seçin **Web uygulaması** Razor sayfaları kullanan bir proje oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="e14da-124">Select **Web Application** to create a project that uses Razor Pages.</span></span>

* <span data-ttu-id="e14da-125">Bir hedef Framework'ü seçin **.NET Core**seçin **ASP.NET Core 2.2**, tıklatıp **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="e14da-125">Select a target framework of **.NET Core**, select **ASP.NET Core 2.2**, and click **OK**.</span></span>

  ![Visual Studio'da yeni proje iletişim kutusu](signalr/_static/signalr-new-project-choose-type.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e14da-127">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e14da-127">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="e14da-128">Açık [tümleşik Terminalini](https://code.visualstudio.com/docs/editor/integrated-terminal) yeni proje klasörü oluşturulacağı klasör.</span><span class="sxs-lookup"><span data-stu-id="e14da-128">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) to the folder in which the new project folder will be created.</span></span>

* <span data-ttu-id="e14da-129">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="e14da-129">Run the following commands:</span></span>

   ```console
   dotnet new webapp -o SignalRChat
   code -r SignalRChat
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e14da-130">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e14da-130">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="e14da-131">Menüden **Dosya > Yeni Çözüm**.</span><span class="sxs-lookup"><span data-stu-id="e14da-131">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="e14da-132">Seçin **.NET Core > Uygulama > ASP.NET Core Web uygulaması** (seçmeyin **ASP.NET Core Web uygulaması (MVC)** ).</span><span class="sxs-lookup"><span data-stu-id="e14da-132">Select **.NET Core > App > ASP.NET Core Web App** (Don't select **ASP.NET Core Web App (MVC)**).</span></span>

* <span data-ttu-id="e14da-133">**İleri**’yi seçin.</span><span class="sxs-lookup"><span data-stu-id="e14da-133">Select **Next**.</span></span>

* <span data-ttu-id="e14da-134">Projeyi adlandırın *SignalRChat*ve ardından **Oluştur**.</span><span class="sxs-lookup"><span data-stu-id="e14da-134">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="e14da-135">SignalR istemci kitaplığı Ekle</span><span class="sxs-lookup"><span data-stu-id="e14da-135">Add the SignalR client library</span></span>

<span data-ttu-id="e14da-136">SignalR server kitaplığı dahil `Microsoft.AspNetCore.App` metapackage.</span><span class="sxs-lookup"><span data-stu-id="e14da-136">The SignalR server library is included in the `Microsoft.AspNetCore.App` metapackage.</span></span> <span data-ttu-id="e14da-137">JavaScript istemci kitaplığı, otomatik olarak projeye dahil değil.</span><span class="sxs-lookup"><span data-stu-id="e14da-137">The JavaScript client library isn't automatically included in the project.</span></span> <span data-ttu-id="e14da-138">Bu öğretici için Kitaplık Yöneticisi'ni (LibMan) istemci kitaplığını almak için kullanmak *unpkg*.</span><span class="sxs-lookup"><span data-stu-id="e14da-138">For this tutorial, you use Library Manager (LibMan) to get the client library from *unpkg*.</span></span> <span data-ttu-id="e14da-139">unpkg bir içerik teslim ağı (CDN) olan), teslim edebilir herhangi bir şey npm, Node.js Paket Yöneticisi bulunamadı.</span><span class="sxs-lookup"><span data-stu-id="e14da-139">unpkg is a content delivery network (CDN)) that can deliver anything found in npm, the Node.js package manager.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e14da-140">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e14da-140">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="e14da-141">İçinde **Çözüm Gezgini**projeye sağ tıklayıp seçin **Ekle** > **istemci tarafı kitaplık**.</span><span class="sxs-lookup"><span data-stu-id="e14da-141">In **Solution Explorer**, right-click the project, and select **Add** > **Client-Side Library**.</span></span>

* <span data-ttu-id="e14da-142">İçinde **istemci tarafı kitaplık Ekle** iletişim için **sağlayıcısı** seçin **unpkg**.</span><span class="sxs-lookup"><span data-stu-id="e14da-142">In the **Add Client-Side Library** dialog, for **Provider** select **unpkg**.</span></span>

* <span data-ttu-id="e14da-143">İçin **Kitaplığı**, girin `@aspnet/signalr@1`, Önizleme olmayan en son sürümü seçin.</span><span class="sxs-lookup"><span data-stu-id="e14da-143">For **Library**, enter `@aspnet/signalr@1`, and select the latest version that isn't preview.</span></span>

  ![İstemci tarafı kitaplık iletişim - select Kütüphane ekleyin](signalr/_static/libman1.png)

* <span data-ttu-id="e14da-145">Seçin **belirli dosyaları seçin**, genişletme *dist/tarayıcı* klasörü ve select *signalr.js* ve *signalr.min.js*.</span><span class="sxs-lookup"><span data-stu-id="e14da-145">Select **Choose specific files**, expand the *dist/browser* folder, and select *signalr.js* and *signalr.min.js*.</span></span>

* <span data-ttu-id="e14da-146">Ayarlama **hedef konum** için *wwwroot/lib/signalr/* seçip **yükleme**.</span><span class="sxs-lookup"><span data-stu-id="e14da-146">Set **Target Location** to *wwwroot/lib/signalr/*, and select **Install**.</span></span>

  ![İstemci tarafı kitaplık iletişim - dosyaları seçin ve hedef Ekle](signalr/_static/libman2.png)

  <span data-ttu-id="e14da-148">LibMan oluşturur bir *wwwroot/lib/signalr* klasörü ve seçili dosyaları kopyalar.</span><span class="sxs-lookup"><span data-stu-id="e14da-148">LibMan creates a *wwwroot/lib/signalr* folder and copies the selected files to it.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e14da-149">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e14da-149">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="e14da-150">Tümleşik terminalde LibMan yüklemek için aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="e14da-150">In the integrated terminal, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="e14da-151">SignalR istemci kitaplığı LibMan almak için aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="e14da-151">Run the following command to get the SignalR client library by using LibMan.</span></span> <span data-ttu-id="e14da-152">Çıktıyı görmeye önce birkaç saniye beklemeniz gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="e14da-152">You might have to wait a few seconds before seeing output.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="e14da-153">Parametreleri aşağıdaki seçenekleri belirtin:</span><span class="sxs-lookup"><span data-stu-id="e14da-153">The parameters specify the following options:</span></span>
  * <span data-ttu-id="e14da-154">Unpkg sağlayıcısını kullanın.</span><span class="sxs-lookup"><span data-stu-id="e14da-154">Use the unpkg provider.</span></span>
  * <span data-ttu-id="e14da-155">Dosyaları kopyalamak *wwwroot/lib/signalr* hedef.</span><span class="sxs-lookup"><span data-stu-id="e14da-155">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="e14da-156">Yalnızca belirtilen dosyaları kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="e14da-156">Copy only the specified files.</span></span>

  <span data-ttu-id="e14da-157">Çıktı aşağıdaki örnekteki gibi görünür:</span><span class="sxs-lookup"><span data-stu-id="e14da-157">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e14da-158">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e14da-158">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="e14da-159">İçinde **Terminal**, LibMan yüklemek için aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="e14da-159">In the **Terminal**, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="e14da-160">Proje klasörüne gidin (içeren *SignalRChat.csproj* dosyası).</span><span class="sxs-lookup"><span data-stu-id="e14da-160">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="e14da-161">SignalR istemci kitaplığı LibMan almak için aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="e14da-161">Run the following command to get the SignalR client library by using LibMan.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="e14da-162">Parametreleri aşağıdaki seçenekleri belirtin:</span><span class="sxs-lookup"><span data-stu-id="e14da-162">The parameters specify the following options:</span></span>
  * <span data-ttu-id="e14da-163">Unpkg sağlayıcısını kullanın.</span><span class="sxs-lookup"><span data-stu-id="e14da-163">Use the unpkg provider.</span></span>
  * <span data-ttu-id="e14da-164">Dosyaları kopyalamak *wwwroot/lib/signalr* hedef.</span><span class="sxs-lookup"><span data-stu-id="e14da-164">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="e14da-165">Yalnızca belirtilen dosyaları kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="e14da-165">Copy only the specified files.</span></span>

  <span data-ttu-id="e14da-166">Çıktı aşağıdaki örnekteki gibi görünür:</span><span class="sxs-lookup"><span data-stu-id="e14da-166">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

---

## <a name="create-a-signalr-hub"></a><span data-ttu-id="e14da-167">Bir SignalR hub'ı oluşturma</span><span class="sxs-lookup"><span data-stu-id="e14da-167">Create a SignalR hub</span></span>

<span data-ttu-id="e14da-168">A *hub* istemci-sunucu iletişimi işleyen bir üst düzey işlem hattı hizmet veren bir sınıftır.</span><span class="sxs-lookup"><span data-stu-id="e14da-168">A *hub* is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="e14da-169">Oluşturma SignalRChat proje klasöründe bir *Hubs* klasör.</span><span class="sxs-lookup"><span data-stu-id="e14da-169">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="e14da-170">İçinde *Hubs* klasör oluşturma bir *ChatHub.cs* dosyasındaki kodu aşağıdaki kodla:</span><span class="sxs-lookup"><span data-stu-id="e14da-170">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

  <span data-ttu-id="e14da-171">`ChatHub` Sınıfından devralan SignalR öğesinden `Hub` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="e14da-171">The `ChatHub` class inherits from the SignalR `Hub` class.</span></span> <span data-ttu-id="e14da-172">`Hub` Sınıfı, bağlantılar, grupları ve mesajlaşma yönetir.</span><span class="sxs-lookup"><span data-stu-id="e14da-172">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="e14da-173">`SendMessage` Yöntemi, tüm istemciler için bir ileti göndermek için bir bağlı istemci tarafından çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="e14da-173">The `SendMessage` method can be called by a connected client to send a message to all clients.</span></span> <span data-ttu-id="e14da-174">Bu yöntemi çağıran JavaScript istemci kodu öğreticinin ilerleyen bölümlerinde gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="e14da-174">JavaScript client code that calls the method is shown later in the tutorial.</span></span> <span data-ttu-id="e14da-175">SignalR kodudur maksimum ölçeklenebilirlik sağlamak için zaman uyumsuz.</span><span class="sxs-lookup"><span data-stu-id="e14da-175">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-signalr"></a><span data-ttu-id="e14da-176">SignalR yapılandırın</span><span class="sxs-lookup"><span data-stu-id="e14da-176">Configure SignalR</span></span>

<span data-ttu-id="e14da-177">SignalR sunucusu, SignalR için SignalR isteklerini iletmek için yapılandırılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="e14da-177">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="e14da-178">Aşağıdaki vurgulanmış kodu ekleyin *Startup.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="e14da-178">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=7,33,52-55)]

  <span data-ttu-id="e14da-179">Bu değişiklikler, ASP.NET Core bağımlılık ekleme sistemi ve ara yazılım ardışık düzenini SignalR ekleyin.</span><span class="sxs-lookup"><span data-stu-id="e14da-179">These changes add SignalR to the ASP.NET Core dependency injection system and the middleware pipeline.</span></span>

## <a name="add-signalr-client-code"></a><span data-ttu-id="e14da-180">SignalR istemci kodu ekleyin</span><span class="sxs-lookup"><span data-stu-id="e14da-180">Add SignalR client code</span></span>

* <span data-ttu-id="e14da-181">İçeriği Değiştir *Pages\Index.cshtml* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="e14da-181">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

  <span data-ttu-id="e14da-182">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="e14da-182">The preceding code:</span></span>

  * <span data-ttu-id="e14da-183">Metin kutuları için adı ve ileti metni ve bir Gönder düğmesi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="e14da-183">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="e14da-184">İçeren bir liste oluşturur `id="messagesList"` SignalR hub'ından alınan iletileri görüntülemek için.</span><span class="sxs-lookup"><span data-stu-id="e14da-184">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="e14da-185">SignalR için komut dosyası başvuruları içerir ve *chat.js* sonraki adımda oluşturduğunuz uygulama kodu.</span><span class="sxs-lookup"><span data-stu-id="e14da-185">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="e14da-186">İçinde *wwwroot/js* klasör oluşturma bir *chat.js* dosyasındaki kodu aşağıdaki kodla:</span><span class="sxs-lookup"><span data-stu-id="e14da-186">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

  <span data-ttu-id="e14da-187">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="e14da-187">The preceding code:</span></span>

  * <span data-ttu-id="e14da-188">Oluşturur ve bir bağlantı başlatır.</span><span class="sxs-lookup"><span data-stu-id="e14da-188">Creates and starts a connection.</span></span>
  * <span data-ttu-id="e14da-189">Gönder düğmesine hub'a iletiler gönderen bir işleyici ekler.</span><span class="sxs-lookup"><span data-stu-id="e14da-189">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="e14da-190">Bağlantı nesnesi için hub'ından iletiler alan ve bunları listeye ekler bir işleyici ekler.</span><span class="sxs-lookup"><span data-stu-id="e14da-190">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="e14da-191">Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="e14da-191">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e14da-192">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e14da-192">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e14da-193">Tuşuna **CTRL + F5** uygulamayı hata ayıklama olmadan çalıştırmak için.</span><span class="sxs-lookup"><span data-stu-id="e14da-193">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e14da-194">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e14da-194">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="e14da-195">Tümleşik terminalde aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="e14da-195">In the integrated terminal, run the following command:</span></span>

  ```console
  dotnet run -p SignalRChat.csproj
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e14da-196">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e14da-196">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="e14da-197">Menüden **çalıştırın > hata ayıklama olmadan Başlat**.</span><span class="sxs-lookup"><span data-stu-id="e14da-197">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="e14da-198">Adres çubuğundan URL'yi kopyalayın, başka bir tarayıcı örneğinde veya sekmesi açın ve adres çubuğuna URL'yi yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="e14da-198">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="e14da-199">Ya da tarayıcı seçin, bir ad ve ileti girin ve seçin **iletisi gönder** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="e14da-199">Choose either browser, enter a name and message, and select the **Send Message** button.</span></span>

  <span data-ttu-id="e14da-200">Her iki sayfalarında, adını ve iletisini anında görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="e14da-200">The name and message are displayed on both pages instantly.</span></span>

  ![SignalR örnek uygulaması](signalr/_static/signalr-get-started-finished.png)

> [!TIP]
> <span data-ttu-id="e14da-202">Uygulama işe yaramazsa, tarayıcı Geliştirici Araçları'nı (F12) açın ve konsoluna gidin.</span><span class="sxs-lookup"><span data-stu-id="e14da-202">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="e14da-203">HTML ve JavaScript kodunuza ilgili hatalar görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e14da-203">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="e14da-204">Örneğin, eklediğiniz varsayalım *signalr.js* yönlendirilmiş değerinden farklı bir klasöre.</span><span class="sxs-lookup"><span data-stu-id="e14da-204">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="e14da-205">Bu durumda bu dosyaya başvuru çalışmaz ve bir 404 hatası konsolunda görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="e14da-205">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
> <span data-ttu-id="e14da-206">![signalr.js bulunamadı hatası](signalr/_static/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="e14da-206">![signalr.js not found error](signalr/_static/f12-console.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="e14da-207">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="e14da-207">Next steps</span></span>

<span data-ttu-id="e14da-208">Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:</span><span class="sxs-lookup"><span data-stu-id="e14da-208">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="e14da-209">Bir web uygulaması projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="e14da-209">Create a web app project.</span></span>
> * <span data-ttu-id="e14da-210">SignalR istemci kitaplığı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="e14da-210">Add the SignalR client library.</span></span>
> * <span data-ttu-id="e14da-211">Bir SignalR hub'ı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="e14da-211">Create a SignalR hub.</span></span>
> * <span data-ttu-id="e14da-212">Projeyi SignalR kullanacak şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="e14da-212">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="e14da-213">Bağlanan tüm istemciler için herhangi bir istemciden ileti göndermek için hub'ı kullanan kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="e14da-213">Add code that uses the hub to send messages from any client to all connected clients.</span></span>

<span data-ttu-id="e14da-214">SignalR hakkında daha fazla bilgi için girişine bakın:</span><span class="sxs-lookup"><span data-stu-id="e14da-214">To learn more about SignalR, see the introduction:</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="e14da-215">ASP.NET Core signalr'a giriş</span><span class="sxs-lookup"><span data-stu-id="e14da-215">Introduction to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
