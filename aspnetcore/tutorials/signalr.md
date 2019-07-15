---
title: ASP.NET Core Signalr'yi kullanmaya başlayın
author: bradygaster
description: Bu öğreticide, ASP.NET Core SignalR kullanan bir sohbet uygulaması oluşturma.
ms.author: bradyg
ms.custom: mvc
ms.date: 07/08/2019
uid: tutorials/signalr
ms.openlocfilehash: 53d3763a93cc72b6bcf85b64a706500299b3597f
ms.sourcegitcommit: 040aedca220ed24ee1726e6886daf6906f95a028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/15/2019
ms.locfileid: "67893709"
---
# <a name="tutorial-get-started-with-aspnet-core-signalr"></a><span data-ttu-id="83d2f-103">Öğretici: ASP.NET Core Signalr'yi kullanmaya başlayın</span><span class="sxs-lookup"><span data-stu-id="83d2f-103">Tutorial: Get started with ASP.NET Core SignalR</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="83d2f-104">Bu öğreticide SignalR kullanarak gerçek zamanlı bir uygulama oluşturmaya ilişkin temel bilgileri size öğretir.</span><span class="sxs-lookup"><span data-stu-id="83d2f-104">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="83d2f-105">Aşağıdakilerin nasıl yapıldığını öğreneceksiniz:</span><span class="sxs-lookup"><span data-stu-id="83d2f-105">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="83d2f-106">Web projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="83d2f-106">Create a web project.</span></span>
> * <span data-ttu-id="83d2f-107">SignalR istemci kitaplığı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="83d2f-107">Add the SignalR client library.</span></span>
> * <span data-ttu-id="83d2f-108">Bir SignalR hub'ı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="83d2f-108">Create a SignalR hub.</span></span>
> * <span data-ttu-id="83d2f-109">Projeyi SignalR kullanacak şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="83d2f-109">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="83d2f-110">Herhangi bir istemciden bağlanan tüm istemciler için iletileri gönderen kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="83d2f-110">Add code that sends messages from any client to all connected clients.</span></span>

<span data-ttu-id="83d2f-111">Sonunda, bir çalışma sohbet uygulaması oluşturmuş olacaksınız:</span><span class="sxs-lookup"><span data-stu-id="83d2f-111">At the end, you'll have a working chat app:</span></span>

![SignalR örnek uygulaması](signalr/_static/3.x/signalr-get-started-finished.png)

## <a name="prerequisites"></a><span data-ttu-id="83d2f-113">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="83d2f-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="83d2f-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="83d2f-114">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="83d2f-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="83d2f-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="83d2f-116">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="83d2f-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-web-app-project"></a><span data-ttu-id="83d2f-117">Bir web uygulaması projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="83d2f-117">Create a web app project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="83d2f-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="83d2f-118">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="83d2f-119">Menüden **Dosya > Yeni proje**.</span><span class="sxs-lookup"><span data-stu-id="83d2f-119">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="83d2f-120">İçinde **yeni bir proje oluşturma** iletişim kutusunda **ASP.NET Core Web uygulaması**ve ardından **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="83d2f-120">In the **Create a new project** dialog, select **ASP.NET Core Web Application**, and then select **Next**.</span></span>

* <span data-ttu-id="83d2f-121">İçinde **yeni projenizi yapılandırın** iletişim kutusunda, proje adı *SignalRChat*ve ardından **Oluştur**.</span><span class="sxs-lookup"><span data-stu-id="83d2f-121">In the **Configure your new project** dialog, name the project *SignalRChat*, and then select **Create**.</span></span>

* <span data-ttu-id="83d2f-122">İçinde **yeni bir ASP.NET Core Web uygulaması oluşturma** iletişim kutusunda **.NET Core** ve **ASP.NET Core 3.0**.</span><span class="sxs-lookup"><span data-stu-id="83d2f-122">In the **Create a new ASP.NET Core Web Application** dialog, select **.NET Core** and **ASP.NET Core 3.0**.</span></span> 

* <span data-ttu-id="83d2f-123">Seçin **Web uygulaması** Razor sayfaları kullanan bir proje oluşturun ve ardından **Oluştur**.</span><span class="sxs-lookup"><span data-stu-id="83d2f-123">Select **Web Application** to create a project that uses Razor Pages, and then select **Create**.</span></span>

  ![Visual Studio'da yeni proje iletişim kutusu](signalr/_static/3.x/signalr-new-project-dialog.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="83d2f-125">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="83d2f-125">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="83d2f-126">Açık [tümleşik Terminalini](https://code.visualstudio.com/docs/editor/integrated-terminal) yeni proje klasörü oluşturulacağı klasör.</span><span class="sxs-lookup"><span data-stu-id="83d2f-126">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) to the folder in which the new project folder will be created.</span></span>

* <span data-ttu-id="83d2f-127">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="83d2f-127">Run the following commands:</span></span>

   ```console
   dotnet new webapp -o SignalRChat
   code -r SignalRChat
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="83d2f-128">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="83d2f-128">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="83d2f-129">Menüden **Dosya > Yeni Çözüm**.</span><span class="sxs-lookup"><span data-stu-id="83d2f-129">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="83d2f-130">Seçin **.NET Core > Uygulama > Web uygulaması** (seçmeyin **Web uygulaması (Model-View-Controller)** ) ve ardından **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="83d2f-130">Select **.NET Core > App > Web Application** (Don't select **Web Application (Model-View-Controller)**), and then select **Next**.</span></span>

* <span data-ttu-id="83d2f-131">Emin **hedef Framework'ü** ayarlanır **.NET Core 3.0**ve ardından **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="83d2f-131">Make sure the **Target Framework** is set to **.NET Core 3.0**, and then select **Next**.</span></span>

* <span data-ttu-id="83d2f-132">Projeyi adlandırın *SignalRChat*ve ardından **Oluştur**.</span><span class="sxs-lookup"><span data-stu-id="83d2f-132">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="83d2f-133">SignalR istemci kitaplığı Ekle</span><span class="sxs-lookup"><span data-stu-id="83d2f-133">Add the SignalR client library</span></span>

<span data-ttu-id="83d2f-134">SignalR server kitaplığı ASP.NET Core 3.0 paylaşılan framework dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="83d2f-134">The SignalR server library is included in the ASP.NET Core 3.0 shared framework.</span></span> <span data-ttu-id="83d2f-135">JavaScript istemci kitaplığı, otomatik olarak projeye dahil değil.</span><span class="sxs-lookup"><span data-stu-id="83d2f-135">The JavaScript client library isn't automatically included in the project.</span></span> <span data-ttu-id="83d2f-136">Bu öğretici için Kitaplık Yöneticisi'ni (LibMan) istemci kitaplığını almak için kullanmak *unpkg*.</span><span class="sxs-lookup"><span data-stu-id="83d2f-136">For this tutorial, you use Library Manager (LibMan) to get the client library from *unpkg*.</span></span> <span data-ttu-id="83d2f-137">unpkg bir içerik teslim ağı (CDN) olan), teslim edebilir herhangi bir şey npm, Node.js Paket Yöneticisi bulunamadı.</span><span class="sxs-lookup"><span data-stu-id="83d2f-137">unpkg is a content delivery network (CDN)) that can deliver anything found in npm, the Node.js package manager.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="83d2f-138">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="83d2f-138">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="83d2f-139">İçinde **Çözüm Gezgini**projeye sağ tıklayıp seçin **Ekle** > **istemci tarafı kitaplık**.</span><span class="sxs-lookup"><span data-stu-id="83d2f-139">In **Solution Explorer**, right-click the project, and select **Add** > **Client-Side Library**.</span></span>

* <span data-ttu-id="83d2f-140">İçinde **istemci tarafı kitaplık Ekle** iletişim için **sağlayıcısı** seçin **unpkg**.</span><span class="sxs-lookup"><span data-stu-id="83d2f-140">In the **Add Client-Side Library** dialog, for **Provider** select **unpkg**.</span></span>

* <span data-ttu-id="83d2f-141">İçin **Kitaplığı**, girin `@aspnet/signalr@next`.</span><span class="sxs-lookup"><span data-stu-id="83d2f-141">For **Library**, enter `@aspnet/signalr@next`.</span></span>
<!-- when 3.0 is released, change @next to @latest -->

* <span data-ttu-id="83d2f-142">Seçin **belirli dosyaları seçin**, genişletme *dist/tarayıcı* klasörü ve select *signalr.js* ve *signalr.min.js*.</span><span class="sxs-lookup"><span data-stu-id="83d2f-142">Select **Choose specific files**, expand the *dist/browser* folder, and select *signalr.js* and *signalr.min.js*.</span></span>

* <span data-ttu-id="83d2f-143">Ayarlama **hedef konum** için *wwwroot/lib/signalr/* seçip **yükleme**.</span><span class="sxs-lookup"><span data-stu-id="83d2f-143">Set **Target Location** to *wwwroot/lib/signalr/*, and select **Install**.</span></span>

  ![İstemci tarafı kitaplık iletişim - select Kütüphane ekleyin](signalr/_static/3.x/libman1.png)

  <span data-ttu-id="83d2f-145">LibMan oluşturur bir *wwwroot/lib/signalr* klasörü ve seçili dosyaları kopyalar.</span><span class="sxs-lookup"><span data-stu-id="83d2f-145">LibMan creates a *wwwroot/lib/signalr* folder and copies the selected files to it.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="83d2f-146">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="83d2f-146">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="83d2f-147">Tümleşik terminalde LibMan yüklemek için aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="83d2f-147">In the integrated terminal, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="83d2f-148">SignalR istemci kitaplığı LibMan almak için aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="83d2f-148">Run the following command to get the SignalR client library by using LibMan.</span></span> <span data-ttu-id="83d2f-149">Çıktıyı görmeye önce birkaç saniye beklemeniz gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="83d2f-149">You might have to wait a few seconds before seeing output.</span></span>

  ```console
  libman install @aspnet/signalr@next -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="83d2f-150">Parametreleri aşağıdaki seçenekleri belirtin:</span><span class="sxs-lookup"><span data-stu-id="83d2f-150">The parameters specify the following options:</span></span>
  * <span data-ttu-id="83d2f-151">Unpkg sağlayıcısını kullanın.</span><span class="sxs-lookup"><span data-stu-id="83d2f-151">Use the unpkg provider.</span></span>
  * <span data-ttu-id="83d2f-152">Dosyaları kopyalamak *wwwroot/lib/signalr* hedef.</span><span class="sxs-lookup"><span data-stu-id="83d2f-152">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="83d2f-153">Yalnızca belirtilen dosyaları kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="83d2f-153">Copy only the specified files.</span></span>

  <span data-ttu-id="83d2f-154">Çıktı aşağıdaki örnekteki gibi görünür:</span><span class="sxs-lookup"><span data-stu-id="83d2f-154">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@next" to "wwwroot/lib/signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="83d2f-155">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="83d2f-155">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="83d2f-156">İçinde **Terminal**, LibMan yüklemek için aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="83d2f-156">In the **Terminal**, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="83d2f-157">Proje klasörüne gidin (içeren *SignalRChat.csproj* dosyası).</span><span class="sxs-lookup"><span data-stu-id="83d2f-157">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="83d2f-158">SignalR istemci kitaplığı LibMan almak için aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="83d2f-158">Run the following command to get the SignalR client library by using LibMan.</span></span>

  ```console
  libman install @aspnet/signalr@next -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="83d2f-159">Parametreleri aşağıdaki seçenekleri belirtin:</span><span class="sxs-lookup"><span data-stu-id="83d2f-159">The parameters specify the following options:</span></span>
  * <span data-ttu-id="83d2f-160">Unpkg sağlayıcısını kullanın.</span><span class="sxs-lookup"><span data-stu-id="83d2f-160">Use the unpkg provider.</span></span>
  * <span data-ttu-id="83d2f-161">Dosyaları kopyalamak *wwwroot/lib/signalr* hedef.</span><span class="sxs-lookup"><span data-stu-id="83d2f-161">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="83d2f-162">Yalnızca belirtilen dosyaları kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="83d2f-162">Copy only the specified files.</span></span>

  <span data-ttu-id="83d2f-163">Çıktı aşağıdaki örnekteki gibi görünür:</span><span class="sxs-lookup"><span data-stu-id="83d2f-163">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@next" to "wwwroot/lib/signalr"
  ```

---

## <a name="create-a-signalr-hub"></a><span data-ttu-id="83d2f-164">Bir SignalR hub'ı oluşturma</span><span class="sxs-lookup"><span data-stu-id="83d2f-164">Create a SignalR hub</span></span>

<span data-ttu-id="83d2f-165">A *hub* istemci-sunucu iletişimi işleyen bir üst düzey işlem hattı hizmet veren bir sınıftır.</span><span class="sxs-lookup"><span data-stu-id="83d2f-165">A *hub* is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="83d2f-166">Oluşturma SignalRChat proje klasöründe bir *Hubs* klasör.</span><span class="sxs-lookup"><span data-stu-id="83d2f-166">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="83d2f-167">İçinde *Hubs* klasör oluşturma bir *ChatHub.cs* dosyasındaki kodu aşağıdaki kodla:</span><span class="sxs-lookup"><span data-stu-id="83d2f-167">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[Startup](signalr/sample-snapshot/3.x/ChatHub.cs)]

  <span data-ttu-id="83d2f-168">`ChatHub` Sınıfından devralan SignalR öğesinden `Hub` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="83d2f-168">The `ChatHub` class inherits from the SignalR `Hub` class.</span></span> <span data-ttu-id="83d2f-169">`Hub` Sınıfı, bağlantılar, grupları ve mesajlaşma yönetir.</span><span class="sxs-lookup"><span data-stu-id="83d2f-169">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="83d2f-170">`SendMessage` Yöntemi, tüm istemciler için bir ileti göndermek için bir bağlı istemci tarafından çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="83d2f-170">The `SendMessage` method can be called by a connected client to send a message to all clients.</span></span> <span data-ttu-id="83d2f-171">Bu yöntemi çağıran JavaScript istemci kodu öğreticinin ilerleyen bölümlerinde gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="83d2f-171">JavaScript client code that calls the method is shown later in the tutorial.</span></span> <span data-ttu-id="83d2f-172">SignalR kodudur maksimum ölçeklenebilirlik sağlamak için zaman uyumsuz.</span><span class="sxs-lookup"><span data-stu-id="83d2f-172">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-signalr"></a><span data-ttu-id="83d2f-173">SignalR yapılandırın</span><span class="sxs-lookup"><span data-stu-id="83d2f-173">Configure SignalR</span></span>

<span data-ttu-id="83d2f-174">SignalR sunucusu, SignalR için SignalR isteklerini iletmek için yapılandırılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="83d2f-174">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="83d2f-175">Aşağıdaki vurgulanmış kodu ekleyin *Startup.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="83d2f-175">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample-snapshot/3.x/Startup.cs?highlight=6,30,58)]

  <span data-ttu-id="83d2f-176">Bu değişiklikler, ASP.NET Core bağımlılık ekleme ve sistemleri yönlendirme SignalR ekleyin.</span><span class="sxs-lookup"><span data-stu-id="83d2f-176">These changes add SignalR to the ASP.NET Core dependency injection and routing systems.</span></span>

## <a name="add-signalr-client-code"></a><span data-ttu-id="83d2f-177">SignalR istemci kodu ekleyin</span><span class="sxs-lookup"><span data-stu-id="83d2f-177">Add SignalR client code</span></span>

* <span data-ttu-id="83d2f-178">İçeriği Değiştir *Pages\Index.cshtml* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="83d2f-178">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](signalr/sample-snapshot/3.x/Index.cshtml)]

  <span data-ttu-id="83d2f-179">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="83d2f-179">The preceding code:</span></span>

  * <span data-ttu-id="83d2f-180">Metin kutuları için adı ve ileti metni ve bir Gönder düğmesi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="83d2f-180">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="83d2f-181">İçeren bir liste oluşturur `id="messagesList"` SignalR hub'ından alınan iletileri görüntülemek için.</span><span class="sxs-lookup"><span data-stu-id="83d2f-181">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="83d2f-182">SignalR için komut dosyası başvuruları içerir ve *chat.js* sonraki adımda oluşturduğunuz uygulama kodu.</span><span class="sxs-lookup"><span data-stu-id="83d2f-182">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="83d2f-183">İçinde *wwwroot/js* klasör oluşturma bir *chat.js* dosyasındaki kodu aşağıdaki kodla:</span><span class="sxs-lookup"><span data-stu-id="83d2f-183">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[Index](signalr/sample-snapshot/3.x/chat.js)]

  <span data-ttu-id="83d2f-184">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="83d2f-184">The preceding code:</span></span>

  * <span data-ttu-id="83d2f-185">Oluşturur ve bir bağlantı başlatır.</span><span class="sxs-lookup"><span data-stu-id="83d2f-185">Creates and starts a connection.</span></span>
  * <span data-ttu-id="83d2f-186">Gönder düğmesine hub'a iletiler gönderen bir işleyici ekler.</span><span class="sxs-lookup"><span data-stu-id="83d2f-186">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="83d2f-187">Bağlantı nesnesi için hub'ından iletiler alan ve bunları listeye ekler bir işleyici ekler.</span><span class="sxs-lookup"><span data-stu-id="83d2f-187">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="83d2f-188">Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="83d2f-188">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="83d2f-189">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="83d2f-189">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="83d2f-190">Tuşuna **CTRL + F5** uygulamayı hata ayıklama olmadan çalıştırmak için.</span><span class="sxs-lookup"><span data-stu-id="83d2f-190">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="83d2f-191">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="83d2f-191">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="83d2f-192">Tümleşik terminalde aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="83d2f-192">In the integrated terminal, run the following command:</span></span>

  ```console
  dotnet run -p SignalRChat.csproj
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="83d2f-193">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="83d2f-193">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="83d2f-194">Menüden **çalıştırın > hata ayıklama olmadan Başlat**.</span><span class="sxs-lookup"><span data-stu-id="83d2f-194">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="83d2f-195">Adres çubuğundan URL'yi kopyalayın, başka bir tarayıcı örneğinde veya sekmesi açın ve adres çubuğuna URL'yi yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="83d2f-195">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="83d2f-196">Ya da tarayıcı seçin, bir ad ve ileti girin ve seçin **iletisi gönder** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="83d2f-196">Choose either browser, enter a name and message, and select the **Send Message** button.</span></span>

  <span data-ttu-id="83d2f-197">Her iki sayfalarında, adını ve iletisini anında görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="83d2f-197">The name and message are displayed on both pages instantly.</span></span>

  ![SignalR örnek uygulaması](signalr/_static/3.x/signalr-get-started-finished.png)

> [!TIP]
> * <span data-ttu-id="83d2f-199">Uygulama işe yaramazsa, tarayıcı Geliştirici Araçları'nı (F12) açın ve konsoluna gidin.</span><span class="sxs-lookup"><span data-stu-id="83d2f-199">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="83d2f-200">HTML ve JavaScript kodunuza ilgili hatalar görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="83d2f-200">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="83d2f-201">Örneğin, eklediğiniz varsayalım *signalr.js* yönlendirilmiş değerinden farklı bir klasöre.</span><span class="sxs-lookup"><span data-stu-id="83d2f-201">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="83d2f-202">Bu durumda bu dosyaya başvuru çalışmaz ve bir 404 hatası konsolunda görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="83d2f-202">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
>   <span data-ttu-id="83d2f-203">![signalr.js bulunamadı hatası](signalr/_static/3.x/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="83d2f-203">![signalr.js not found error](signalr/_static/3.x/f12-console.png)</span></span>
> * <span data-ttu-id="83d2f-204">Chrome'da ERR_SPDY_INADEQUATE_TRANSPORT_SECURITY veya NS_ERROR_NET_INADEQUATE_SECURITY Firefox'ta hata alırsanız, geliştirme sertifikanızı güncelleştirmek için şu komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="83d2f-204">If you get the error ERR_SPDY_INADEQUATE_TRANSPORT_SECURITY in Chrome or NS_ERROR_NET_INADEQUATE_SECURITY in Firefox, run these commands to update your development certificate:</span></span>
>   ```
>   dotnet dev-certs https --clean
>   dotnet dev-certs https --trust
>   ```

## <a name="next-steps"></a><span data-ttu-id="83d2f-205">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="83d2f-205">Next steps</span></span>

<span data-ttu-id="83d2f-206">SignalR hakkında daha fazla bilgi için girişine bakın:</span><span class="sxs-lookup"><span data-stu-id="83d2f-206">To learn more about SignalR, see the introduction:</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="83d2f-207">ASP.NET Core signalr'a giriş</span><span class="sxs-lookup"><span data-stu-id="83d2f-207">Introduction to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="83d2f-208">Bu öğreticide SignalR kullanarak gerçek zamanlı bir uygulama oluşturmaya ilişkin temel bilgileri size öğretir.</span><span class="sxs-lookup"><span data-stu-id="83d2f-208">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="83d2f-209">Aşağıdakilerin nasıl yapıldığını öğreneceksiniz:</span><span class="sxs-lookup"><span data-stu-id="83d2f-209">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="83d2f-210">Web projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="83d2f-210">Create a web project.</span></span>
> * <span data-ttu-id="83d2f-211">SignalR istemci kitaplığı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="83d2f-211">Add the SignalR client library.</span></span>
> * <span data-ttu-id="83d2f-212">Bir SignalR hub'ı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="83d2f-212">Create a SignalR hub.</span></span>
> * <span data-ttu-id="83d2f-213">Projeyi SignalR kullanacak şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="83d2f-213">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="83d2f-214">Herhangi bir istemciden bağlanan tüm istemciler için iletileri gönderen kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="83d2f-214">Add code that sends messages from any client to all connected clients.</span></span>

<span data-ttu-id="83d2f-215">Sonunda, bir çalışma sohbet uygulaması oluşturmuş olacaksınız:</span><span class="sxs-lookup"><span data-stu-id="83d2f-215">At the end, you'll have a working chat app:</span></span>

![SignalR örnek uygulaması](signalr/_static/2.x/signalr-get-started-finished.png)

## <a name="prerequisites"></a><span data-ttu-id="83d2f-217">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="83d2f-217">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="83d2f-218">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="83d2f-218">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2017-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="83d2f-219">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="83d2f-219">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="83d2f-220">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="83d2f-220">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="83d2f-221">Bir web projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="83d2f-221">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="83d2f-222">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="83d2f-222">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="83d2f-223">Menüden **Dosya > Yeni proje**.</span><span class="sxs-lookup"><span data-stu-id="83d2f-223">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="83d2f-224">İçinde **yeni proje** iletişim kutusunda **yüklü > Visual C# > Web > ASP.NET Core Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="83d2f-224">In the **New Project** dialog, select **Installed > Visual C# > Web > ASP.NET Core Web Application**.</span></span> <span data-ttu-id="83d2f-225">Projeyi adlandırın *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="83d2f-225">Name the project *SignalRChat*.</span></span>

  ![Visual Studio'da yeni proje iletişim kutusu](signalr/_static/2.x/signalr-new-project-dialog.png)

* <span data-ttu-id="83d2f-227">Seçin **Web uygulaması** Razor sayfaları kullanan bir proje oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="83d2f-227">Select **Web Application** to create a project that uses Razor Pages.</span></span>

* <span data-ttu-id="83d2f-228">Bir hedef Framework'ü seçin **.NET Core**seçin **ASP.NET Core 2.2**, tıklatıp **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="83d2f-228">Select a target framework of **.NET Core**, select **ASP.NET Core 2.2**, and click **OK**.</span></span>

  ![Visual Studio'da yeni proje iletişim kutusu](signalr/_static/2.x/signalr-new-project-choose-type.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="83d2f-230">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="83d2f-230">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="83d2f-231">Açık [tümleşik Terminalini](https://code.visualstudio.com/docs/editor/integrated-terminal) yeni proje klasörü oluşturulacağı klasör.</span><span class="sxs-lookup"><span data-stu-id="83d2f-231">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) to the folder in which the new project folder will be created.</span></span>

* <span data-ttu-id="83d2f-232">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="83d2f-232">Run the following commands:</span></span>

   ```console
   dotnet new webapp -o SignalRChat
   code -r SignalRChat
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="83d2f-233">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="83d2f-233">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="83d2f-234">Menüden **Dosya > Yeni Çözüm**.</span><span class="sxs-lookup"><span data-stu-id="83d2f-234">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="83d2f-235">Seçin **.NET Core > Uygulama > ASP.NET Core Web uygulaması** (seçmeyin **ASP.NET Core Web uygulaması (MVC)** ).</span><span class="sxs-lookup"><span data-stu-id="83d2f-235">Select **.NET Core > App > ASP.NET Core Web App** (Don't select **ASP.NET Core Web App (MVC)**).</span></span>

* <span data-ttu-id="83d2f-236">**İleri**’yi seçin.</span><span class="sxs-lookup"><span data-stu-id="83d2f-236">Select **Next**.</span></span>

* <span data-ttu-id="83d2f-237">Projeyi adlandırın *SignalRChat*ve ardından **Oluştur**.</span><span class="sxs-lookup"><span data-stu-id="83d2f-237">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="83d2f-238">SignalR istemci kitaplığı Ekle</span><span class="sxs-lookup"><span data-stu-id="83d2f-238">Add the SignalR client library</span></span>

<span data-ttu-id="83d2f-239">SignalR server kitaplığı dahil `Microsoft.AspNetCore.App` metapackage.</span><span class="sxs-lookup"><span data-stu-id="83d2f-239">The SignalR server library is included in the `Microsoft.AspNetCore.App` metapackage.</span></span> <span data-ttu-id="83d2f-240">JavaScript istemci kitaplığı, otomatik olarak projeye dahil değil.</span><span class="sxs-lookup"><span data-stu-id="83d2f-240">The JavaScript client library isn't automatically included in the project.</span></span> <span data-ttu-id="83d2f-241">Bu öğretici için Kitaplık Yöneticisi'ni (LibMan) istemci kitaplığını almak için kullanmak *unpkg*.</span><span class="sxs-lookup"><span data-stu-id="83d2f-241">For this tutorial, you use Library Manager (LibMan) to get the client library from *unpkg*.</span></span> <span data-ttu-id="83d2f-242">unpkg bir içerik teslim ağı (CDN) olan), teslim edebilir herhangi bir şey npm, Node.js Paket Yöneticisi bulunamadı.</span><span class="sxs-lookup"><span data-stu-id="83d2f-242">unpkg is a content delivery network (CDN)) that can deliver anything found in npm, the Node.js package manager.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="83d2f-243">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="83d2f-243">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="83d2f-244">İçinde **Çözüm Gezgini**projeye sağ tıklayıp seçin **Ekle** > **istemci tarafı kitaplık**.</span><span class="sxs-lookup"><span data-stu-id="83d2f-244">In **Solution Explorer**, right-click the project, and select **Add** > **Client-Side Library**.</span></span>

* <span data-ttu-id="83d2f-245">İçinde **istemci tarafı kitaplık Ekle** iletişim için **sağlayıcısı** seçin **unpkg**.</span><span class="sxs-lookup"><span data-stu-id="83d2f-245">In the **Add Client-Side Library** dialog, for **Provider** select **unpkg**.</span></span>

* <span data-ttu-id="83d2f-246">İçin **Kitaplığı**, girin `@aspnet/signalr@1`, Önizleme olmayan en son sürümü seçin.</span><span class="sxs-lookup"><span data-stu-id="83d2f-246">For **Library**, enter `@aspnet/signalr@1`, and select the latest version that isn't preview.</span></span>

  ![İstemci tarafı kitaplık iletişim - select Kütüphane ekleyin](signalr/_static/2.x/libman1.png)

* <span data-ttu-id="83d2f-248">Seçin **belirli dosyaları seçin**, genişletme *dist/tarayıcı* klasörü ve select *signalr.js* ve *signalr.min.js*.</span><span class="sxs-lookup"><span data-stu-id="83d2f-248">Select **Choose specific files**, expand the *dist/browser* folder, and select *signalr.js* and *signalr.min.js*.</span></span>

* <span data-ttu-id="83d2f-249">Ayarlama **hedef konum** için *wwwroot/lib/signalr/* seçip **yükleme**.</span><span class="sxs-lookup"><span data-stu-id="83d2f-249">Set **Target Location** to *wwwroot/lib/signalr/*, and select **Install**.</span></span>

  ![İstemci tarafı kitaplık iletişim - dosyaları seçin ve hedef Ekle](signalr/_static/2.x/libman2.png)

  <span data-ttu-id="83d2f-251">LibMan oluşturur bir *wwwroot/lib/signalr* klasörü ve seçili dosyaları kopyalar.</span><span class="sxs-lookup"><span data-stu-id="83d2f-251">LibMan creates a *wwwroot/lib/signalr* folder and copies the selected files to it.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="83d2f-252">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="83d2f-252">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="83d2f-253">Tümleşik terminalde LibMan yüklemek için aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="83d2f-253">In the integrated terminal, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="83d2f-254">SignalR istemci kitaplığı LibMan almak için aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="83d2f-254">Run the following command to get the SignalR client library by using LibMan.</span></span> <span data-ttu-id="83d2f-255">Çıktıyı görmeye önce birkaç saniye beklemeniz gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="83d2f-255">You might have to wait a few seconds before seeing output.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="83d2f-256">Parametreleri aşağıdaki seçenekleri belirtin:</span><span class="sxs-lookup"><span data-stu-id="83d2f-256">The parameters specify the following options:</span></span>
  * <span data-ttu-id="83d2f-257">Unpkg sağlayıcısını kullanın.</span><span class="sxs-lookup"><span data-stu-id="83d2f-257">Use the unpkg provider.</span></span>
  * <span data-ttu-id="83d2f-258">Dosyaları kopyalamak *wwwroot/lib/signalr* hedef.</span><span class="sxs-lookup"><span data-stu-id="83d2f-258">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="83d2f-259">Yalnızca belirtilen dosyaları kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="83d2f-259">Copy only the specified files.</span></span>

  <span data-ttu-id="83d2f-260">Çıktı aşağıdaki örnekteki gibi görünür:</span><span class="sxs-lookup"><span data-stu-id="83d2f-260">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="83d2f-261">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="83d2f-261">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="83d2f-262">İçinde **Terminal**, LibMan yüklemek için aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="83d2f-262">In the **Terminal**, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="83d2f-263">Proje klasörüne gidin (içeren *SignalRChat.csproj* dosyası).</span><span class="sxs-lookup"><span data-stu-id="83d2f-263">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="83d2f-264">SignalR istemci kitaplığı LibMan almak için aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="83d2f-264">Run the following command to get the SignalR client library by using LibMan.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="83d2f-265">Parametreleri aşağıdaki seçenekleri belirtin:</span><span class="sxs-lookup"><span data-stu-id="83d2f-265">The parameters specify the following options:</span></span>
  * <span data-ttu-id="83d2f-266">Unpkg sağlayıcısını kullanın.</span><span class="sxs-lookup"><span data-stu-id="83d2f-266">Use the unpkg provider.</span></span>
  * <span data-ttu-id="83d2f-267">Dosyaları kopyalamak *wwwroot/lib/signalr* hedef.</span><span class="sxs-lookup"><span data-stu-id="83d2f-267">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="83d2f-268">Yalnızca belirtilen dosyaları kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="83d2f-268">Copy only the specified files.</span></span>

  <span data-ttu-id="83d2f-269">Çıktı aşağıdaki örnekteki gibi görünür:</span><span class="sxs-lookup"><span data-stu-id="83d2f-269">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

---

## <a name="create-a-signalr-hub"></a><span data-ttu-id="83d2f-270">Bir SignalR hub'ı oluşturma</span><span class="sxs-lookup"><span data-stu-id="83d2f-270">Create a SignalR hub</span></span>

<span data-ttu-id="83d2f-271">A *hub* istemci-sunucu iletişimi işleyen bir üst düzey işlem hattı hizmet veren bir sınıftır.</span><span class="sxs-lookup"><span data-stu-id="83d2f-271">A *hub* is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="83d2f-272">Oluşturma SignalRChat proje klasöründe bir *Hubs* klasör.</span><span class="sxs-lookup"><span data-stu-id="83d2f-272">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="83d2f-273">İçinde *Hubs* klasör oluşturma bir *ChatHub.cs* dosyasındaki kodu aşağıdaki kodla:</span><span class="sxs-lookup"><span data-stu-id="83d2f-273">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[Startup](signalr/sample-snapshot/2.x/ChatHub.cs)]

  <span data-ttu-id="83d2f-274">`ChatHub` Sınıfından devralan SignalR öğesinden `Hub` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="83d2f-274">The `ChatHub` class inherits from the SignalR `Hub` class.</span></span> <span data-ttu-id="83d2f-275">`Hub` Sınıfı, bağlantılar, grupları ve mesajlaşma yönetir.</span><span class="sxs-lookup"><span data-stu-id="83d2f-275">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="83d2f-276">`SendMessage` Yöntemi, tüm istemciler için bir ileti göndermek için bir bağlı istemci tarafından çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="83d2f-276">The `SendMessage` method can be called by a connected client to send a message to all clients.</span></span> <span data-ttu-id="83d2f-277">Bu yöntemi çağıran JavaScript istemci kodu öğreticinin ilerleyen bölümlerinde gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="83d2f-277">JavaScript client code that calls the method is shown later in the tutorial.</span></span> <span data-ttu-id="83d2f-278">SignalR kodudur maksimum ölçeklenebilirlik sağlamak için zaman uyumsuz.</span><span class="sxs-lookup"><span data-stu-id="83d2f-278">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-signalr"></a><span data-ttu-id="83d2f-279">SignalR yapılandırın</span><span class="sxs-lookup"><span data-stu-id="83d2f-279">Configure SignalR</span></span>

<span data-ttu-id="83d2f-280">SignalR sunucusu, SignalR için SignalR isteklerini iletmek için yapılandırılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="83d2f-280">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="83d2f-281">Aşağıdaki vurgulanmış kodu ekleyin *Startup.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="83d2f-281">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample-snapshot/2.x/Startup.cs?highlight=7,33,52-55)]

  <span data-ttu-id="83d2f-282">Bu değişiklikler, ASP.NET Core bağımlılık ekleme sistemi ve ara yazılım ardışık düzenini SignalR ekleyin.</span><span class="sxs-lookup"><span data-stu-id="83d2f-282">These changes add SignalR to the ASP.NET Core dependency injection system and the middleware pipeline.</span></span>

## <a name="add-signalr-client-code"></a><span data-ttu-id="83d2f-283">SignalR istemci kodu ekleyin</span><span class="sxs-lookup"><span data-stu-id="83d2f-283">Add SignalR client code</span></span>

* <span data-ttu-id="83d2f-284">İçeriği Değiştir *Pages\Index.cshtml* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="83d2f-284">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](signalr/sample-snapshot/2.x/Index.cshtml)]

  <span data-ttu-id="83d2f-285">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="83d2f-285">The preceding code:</span></span>

  * <span data-ttu-id="83d2f-286">Metin kutuları için adı ve ileti metni ve bir Gönder düğmesi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="83d2f-286">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="83d2f-287">İçeren bir liste oluşturur `id="messagesList"` SignalR hub'ından alınan iletileri görüntülemek için.</span><span class="sxs-lookup"><span data-stu-id="83d2f-287">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="83d2f-288">SignalR için komut dosyası başvuruları içerir ve *chat.js* sonraki adımda oluşturduğunuz uygulama kodu.</span><span class="sxs-lookup"><span data-stu-id="83d2f-288">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="83d2f-289">İçinde *wwwroot/js* klasör oluşturma bir *chat.js* dosyasındaki kodu aşağıdaki kodla:</span><span class="sxs-lookup"><span data-stu-id="83d2f-289">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[Index](signalr/sample-snapshot/2.x/chat.js)]

  <span data-ttu-id="83d2f-290">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="83d2f-290">The preceding code:</span></span>

  * <span data-ttu-id="83d2f-291">Oluşturur ve bir bağlantı başlatır.</span><span class="sxs-lookup"><span data-stu-id="83d2f-291">Creates and starts a connection.</span></span>
  * <span data-ttu-id="83d2f-292">Gönder düğmesine hub'a iletiler gönderen bir işleyici ekler.</span><span class="sxs-lookup"><span data-stu-id="83d2f-292">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="83d2f-293">Bağlantı nesnesi için hub'ından iletiler alan ve bunları listeye ekler bir işleyici ekler.</span><span class="sxs-lookup"><span data-stu-id="83d2f-293">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="83d2f-294">Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="83d2f-294">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="83d2f-295">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="83d2f-295">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="83d2f-296">Tuşuna **CTRL + F5** uygulamayı hata ayıklama olmadan çalıştırmak için.</span><span class="sxs-lookup"><span data-stu-id="83d2f-296">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="83d2f-297">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="83d2f-297">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="83d2f-298">Tümleşik terminalde aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="83d2f-298">In the integrated terminal, run the following command:</span></span>

  ```console
  dotnet run -p SignalRChat.csproj
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="83d2f-299">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="83d2f-299">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="83d2f-300">Menüden **çalıştırın > hata ayıklama olmadan Başlat**.</span><span class="sxs-lookup"><span data-stu-id="83d2f-300">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="83d2f-301">Adres çubuğundan URL'yi kopyalayın, başka bir tarayıcı örneğinde veya sekmesi açın ve adres çubuğuna URL'yi yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="83d2f-301">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="83d2f-302">Ya da tarayıcı seçin, bir ad ve ileti girin ve seçin **iletisi gönder** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="83d2f-302">Choose either browser, enter a name and message, and select the **Send Message** button.</span></span>

  <span data-ttu-id="83d2f-303">Her iki sayfalarında, adını ve iletisini anında görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="83d2f-303">The name and message are displayed on both pages instantly.</span></span>

  ![SignalR örnek uygulaması](signalr/_static/2.x/signalr-get-started-finished.png)

> [!TIP]
> <span data-ttu-id="83d2f-305">Uygulama işe yaramazsa, tarayıcı Geliştirici Araçları'nı (F12) açın ve konsoluna gidin.</span><span class="sxs-lookup"><span data-stu-id="83d2f-305">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="83d2f-306">HTML ve JavaScript kodunuza ilgili hatalar görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="83d2f-306">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="83d2f-307">Örneğin, eklediğiniz varsayalım *signalr.js* yönlendirilmiş değerinden farklı bir klasöre.</span><span class="sxs-lookup"><span data-stu-id="83d2f-307">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="83d2f-308">Bu durumda bu dosyaya başvuru çalışmaz ve bir 404 hatası konsolunda görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="83d2f-308">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
> <span data-ttu-id="83d2f-309">![signalr.js bulunamadı hatası](signalr/_static/2.x/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="83d2f-309">![signalr.js not found error](signalr/_static/2.x/f12-console.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="83d2f-310">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="83d2f-310">Next steps</span></span>

<span data-ttu-id="83d2f-311">Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:</span><span class="sxs-lookup"><span data-stu-id="83d2f-311">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="83d2f-312">Bir web uygulaması projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="83d2f-312">Create a web app project.</span></span>
> * <span data-ttu-id="83d2f-313">SignalR istemci kitaplığı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="83d2f-313">Add the SignalR client library.</span></span>
> * <span data-ttu-id="83d2f-314">Bir SignalR hub'ı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="83d2f-314">Create a SignalR hub.</span></span>
> * <span data-ttu-id="83d2f-315">Projeyi SignalR kullanacak şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="83d2f-315">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="83d2f-316">Bağlanan tüm istemciler için herhangi bir istemciden ileti göndermek için hub'ı kullanan kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="83d2f-316">Add code that uses the hub to send messages from any client to all connected clients.</span></span>

<span data-ttu-id="83d2f-317">SignalR hakkında daha fazla bilgi için girişine bakın:</span><span class="sxs-lookup"><span data-stu-id="83d2f-317">To learn more about SignalR, see the introduction:</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="83d2f-318">ASP.NET Core signalr'a giriş</span><span class="sxs-lookup"><span data-stu-id="83d2f-318">Introduction to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)

::: moniker-end
