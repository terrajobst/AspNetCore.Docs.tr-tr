---
title: ASP.NET Core Signalr'yi kullanmaya başlayın
author: bradygaster
description: Bu öğreticide, ASP.NET Core SignalR kullanan bir sohbet uygulaması oluşturma.
ms.author: bradyg
ms.custom: mvc
ms.date: 07/08/2019
uid: tutorials/signalr
ms.openlocfilehash: fd3324dfa0731ae16747178d83bd93ed95dd15ce
ms.sourcegitcommit: bee530454ae2b3c25dc7ffebf93536f479a14460
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67724481"
---
# <a name="tutorial-get-started-with-aspnet-core-signalr"></a><span data-ttu-id="c4726-103">Öğretici: ASP.NET Core Signalr'yi kullanmaya başlayın</span><span class="sxs-lookup"><span data-stu-id="c4726-103">Tutorial: Get started with ASP.NET Core SignalR</span></span>

<span data-ttu-id="c4726-104">Bu öğreticide SignalR kullanarak gerçek zamanlı bir uygulama oluşturmaya ilişkin temel bilgileri size öğretir.</span><span class="sxs-lookup"><span data-stu-id="c4726-104">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="c4726-105">Aşağıdakilerin nasıl yapıldığını öğreneceksiniz:</span><span class="sxs-lookup"><span data-stu-id="c4726-105">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c4726-106">Web projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="c4726-106">Create a web project.</span></span>
> * <span data-ttu-id="c4726-107">SignalR istemci kitaplığı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="c4726-107">Add the SignalR client library.</span></span>
> * <span data-ttu-id="c4726-108">Bir SignalR hub'ı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="c4726-108">Create a SignalR hub.</span></span>
> * <span data-ttu-id="c4726-109">Projeyi SignalR kullanacak şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="c4726-109">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="c4726-110">Herhangi bir istemciden bağlanan tüm istemciler için iletileri gönderen kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="c4726-110">Add code that sends messages from any client to all connected clients.</span></span>

<span data-ttu-id="c4726-111">Sonunda, bir çalışma sohbet uygulaması oluşturmuş olacaksınız:</span><span class="sxs-lookup"><span data-stu-id="c4726-111">At the end, you'll have a working chat app:</span></span>

![SignalR örnek uygulaması](signalr/_static/signalr-get-started-finished.png)

## <a name="prerequisites"></a><span data-ttu-id="c4726-113">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="c4726-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c4726-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c4726-114">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c4726-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c4726-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="c4726-116">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c4726-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-web-app-project"></a><span data-ttu-id="c4726-117">Bir web uygulaması projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="c4726-117">Create a web app project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c4726-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c4726-118">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="c4726-119">Menüden **Dosya > Yeni proje**.</span><span class="sxs-lookup"><span data-stu-id="c4726-119">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="c4726-120">İçinde **yeni bir proje oluşturma** iletişim kutusunda **ASP.NET Core Web uygulaması**ve ardından **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="c4726-120">In the **Create a new project** dialog, select **ASP.NET Core Web Application**, and then select **Next**.</span></span>

* <span data-ttu-id="c4726-121">İçinde **yeni projenizi yapılandırın** iletişim kutusunda, proje adı *SignalRChat*ve ardından **Oluştur**.</span><span class="sxs-lookup"><span data-stu-id="c4726-121">In the **Configure your new project** dialog, name the project *SignalRChat*, and then select **Create**.</span></span>

* <span data-ttu-id="c4726-122">İçinde **yeni bir ASP.NET Core Web uygulaması oluşturma** iletişim kutusunda **.NET Core** ve **ASP.NET Core 3.0**.</span><span class="sxs-lookup"><span data-stu-id="c4726-122">In the **Create a new ASP.NET Core Web Application** dialog, select **.NET Core** and **ASP.NET Core 3.0**.</span></span> 

* <span data-ttu-id="c4726-123">Seçin **Web uygulaması** Razor sayfaları kullanan bir proje oluşturun ve ardından **Oluştur**.</span><span class="sxs-lookup"><span data-stu-id="c4726-123">Select **Web Application** to create a project that uses Razor Pages, and then select **Create**.</span></span>

  ![Visual Studio'da yeni proje iletişim kutusu](signalr/_static/signalr-new-project-dialog.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c4726-125">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c4726-125">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="c4726-126">Açık [tümleşik Terminalini](https://code.visualstudio.com/docs/editor/integrated-terminal) yeni proje klasörü oluşturulacağı klasör.</span><span class="sxs-lookup"><span data-stu-id="c4726-126">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) to the folder in which the new project folder will be created.</span></span>

* <span data-ttu-id="c4726-127">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="c4726-127">Run the following commands:</span></span>

   ```console
   dotnet new webapp -o SignalRChat
   code -r SignalRChat
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="c4726-128">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c4726-128">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="c4726-129">Menüden **Dosya > Yeni Çözüm**.</span><span class="sxs-lookup"><span data-stu-id="c4726-129">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="c4726-130">Seçin **.NET Core > Uygulama > Web uygulaması** (seçmeyin **Web uygulaması (Model-View-Controller)** ) ve ardından **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="c4726-130">Select **.NET Core > App > Web Application** (Don't select **Web Application (Model-View-Controller)**), and then select **Next**.</span></span>

* <span data-ttu-id="c4726-131">Emin **hedef Framework'ü** ayarlanır **.NET Core 3.0**ve ardından **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="c4726-131">Make sure the **Target Framework** is set to **.NET Core 3.0**, and then select **Next**.</span></span>

* <span data-ttu-id="c4726-132">Projeyi adlandırın *SignalRChat*ve ardından **Oluştur**.</span><span class="sxs-lookup"><span data-stu-id="c4726-132">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="c4726-133">SignalR istemci kitaplığı Ekle</span><span class="sxs-lookup"><span data-stu-id="c4726-133">Add the SignalR client library</span></span>

<span data-ttu-id="c4726-134">SignalR server kitaplığı ASP.NET Core 3.0 paylaşılan framework dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="c4726-134">The SignalR server library is included in the ASP.NET Core 3.0 shared framework.</span></span> <span data-ttu-id="c4726-135">JavaScript istemci kitaplığı, otomatik olarak projeye dahil değil.</span><span class="sxs-lookup"><span data-stu-id="c4726-135">The JavaScript client library isn't automatically included in the project.</span></span> <span data-ttu-id="c4726-136">Bu öğretici için Kitaplık Yöneticisi'ni (LibMan) istemci kitaplığını almak için kullanmak *unpkg*.</span><span class="sxs-lookup"><span data-stu-id="c4726-136">For this tutorial, you use Library Manager (LibMan) to get the client library from *unpkg*.</span></span> <span data-ttu-id="c4726-137">unpkg bir içerik teslim ağı (CDN) olan), teslim edebilir herhangi bir şey npm, Node.js Paket Yöneticisi bulunamadı.</span><span class="sxs-lookup"><span data-stu-id="c4726-137">unpkg is a content delivery network (CDN)) that can deliver anything found in npm, the Node.js package manager.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c4726-138">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c4726-138">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="c4726-139">İçinde **Çözüm Gezgini**projeye sağ tıklayıp seçin **Ekle** > **istemci tarafı kitaplık**.</span><span class="sxs-lookup"><span data-stu-id="c4726-139">In **Solution Explorer**, right-click the project, and select **Add** > **Client-Side Library**.</span></span>

* <span data-ttu-id="c4726-140">İçinde **istemci tarafı kitaplık Ekle** iletişim için **sağlayıcısı** seçin **unpkg**.</span><span class="sxs-lookup"><span data-stu-id="c4726-140">In the **Add Client-Side Library** dialog, for **Provider** select **unpkg**.</span></span>

* <span data-ttu-id="c4726-141">İçin **Kitaplığı**, girin `@aspnet/signalr@next`.</span><span class="sxs-lookup"><span data-stu-id="c4726-141">For **Library**, enter `@aspnet/signalr@next`.</span></span>
<!-- when 3.0 is released, change @next to @latest -->

* <span data-ttu-id="c4726-142">Seçin **belirli dosyaları seçin**, genişletme *dist/tarayıcı* klasörü ve select *signalr.js* ve *signalr.min.js*.</span><span class="sxs-lookup"><span data-stu-id="c4726-142">Select **Choose specific files**, expand the *dist/browser* folder, and select *signalr.js* and *signalr.min.js*.</span></span>

* <span data-ttu-id="c4726-143">Ayarlama **hedef konum** için *wwwroot/lib/signalr/* seçip **yükleme**.</span><span class="sxs-lookup"><span data-stu-id="c4726-143">Set **Target Location** to *wwwroot/lib/signalr/*, and select **Install**.</span></span>

  ![İstemci tarafı kitaplık iletişim - select Kütüphane ekleyin](signalr/_static/libman1.png)

  <span data-ttu-id="c4726-145">LibMan oluşturur bir *wwwroot/lib/signalr* klasörü ve seçili dosyaları kopyalar.</span><span class="sxs-lookup"><span data-stu-id="c4726-145">LibMan creates a *wwwroot/lib/signalr* folder and copies the selected files to it.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c4726-146">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c4726-146">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="c4726-147">Tümleşik terminalde LibMan yüklemek için aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="c4726-147">In the integrated terminal, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="c4726-148">SignalR istemci kitaplığı LibMan almak için aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="c4726-148">Run the following command to get the SignalR client library by using LibMan.</span></span> <span data-ttu-id="c4726-149">Çıktıyı görmeye önce birkaç saniye beklemeniz gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="c4726-149">You might have to wait a few seconds before seeing output.</span></span>

  ```console
  libman install @aspnet/signalr@next -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="c4726-150">Parametreleri aşağıdaki seçenekleri belirtin:</span><span class="sxs-lookup"><span data-stu-id="c4726-150">The parameters specify the following options:</span></span>
  * <span data-ttu-id="c4726-151">Unpkg sağlayıcısını kullanın.</span><span class="sxs-lookup"><span data-stu-id="c4726-151">Use the unpkg provider.</span></span>
  * <span data-ttu-id="c4726-152">Dosyaları kopyalamak *wwwroot/lib/signalr* hedef.</span><span class="sxs-lookup"><span data-stu-id="c4726-152">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="c4726-153">Yalnızca belirtilen dosyaları kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="c4726-153">Copy only the specified files.</span></span>

  <span data-ttu-id="c4726-154">Çıktı aşağıdaki örnekteki gibi görünür:</span><span class="sxs-lookup"><span data-stu-id="c4726-154">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@next" to "wwwroot/lib/signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="c4726-155">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c4726-155">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="c4726-156">İçinde **Terminal**, LibMan yüklemek için aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="c4726-156">In the **Terminal**, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="c4726-157">Proje klasörüne gidin (içeren *SignalRChat.csproj* dosyası).</span><span class="sxs-lookup"><span data-stu-id="c4726-157">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="c4726-158">SignalR istemci kitaplığı LibMan almak için aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="c4726-158">Run the following command to get the SignalR client library by using LibMan.</span></span>

  ```console
  libman install @aspnet/signalr@next -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="c4726-159">Parametreleri aşağıdaki seçenekleri belirtin:</span><span class="sxs-lookup"><span data-stu-id="c4726-159">The parameters specify the following options:</span></span>
  * <span data-ttu-id="c4726-160">Unpkg sağlayıcısını kullanın.</span><span class="sxs-lookup"><span data-stu-id="c4726-160">Use the unpkg provider.</span></span>
  * <span data-ttu-id="c4726-161">Dosyaları kopyalamak *wwwroot/lib/signalr* hedef.</span><span class="sxs-lookup"><span data-stu-id="c4726-161">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="c4726-162">Yalnızca belirtilen dosyaları kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="c4726-162">Copy only the specified files.</span></span>

  <span data-ttu-id="c4726-163">Çıktı aşağıdaki örnekteki gibi görünür:</span><span class="sxs-lookup"><span data-stu-id="c4726-163">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@next" to "wwwroot/lib/signalr"
  ```

---

## <a name="create-a-signalr-hub"></a><span data-ttu-id="c4726-164">Bir SignalR hub'ı oluşturma</span><span class="sxs-lookup"><span data-stu-id="c4726-164">Create a SignalR hub</span></span>

<span data-ttu-id="c4726-165">A *hub* istemci-sunucu iletişimi işleyen bir üst düzey işlem hattı hizmet veren bir sınıftır.</span><span class="sxs-lookup"><span data-stu-id="c4726-165">A *hub* is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="c4726-166">Oluşturma SignalRChat proje klasöründe bir *Hubs* klasör.</span><span class="sxs-lookup"><span data-stu-id="c4726-166">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="c4726-167">İçinde *Hubs* klasör oluşturma bir *ChatHub.cs* dosyasındaki kodu aşağıdaki kodla:</span><span class="sxs-lookup"><span data-stu-id="c4726-167">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[Startup](signalr/sample-snapshot/ChatHub.cs)]

  <span data-ttu-id="c4726-168">`ChatHub` Sınıfından devralan SignalR öğesinden `Hub` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="c4726-168">The `ChatHub` class inherits from the SignalR `Hub` class.</span></span> <span data-ttu-id="c4726-169">`Hub` Sınıfı, bağlantılar, grupları ve mesajlaşma yönetir.</span><span class="sxs-lookup"><span data-stu-id="c4726-169">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="c4726-170">`SendMessage` Yöntemi, tüm istemciler için bir ileti göndermek için bir bağlı istemci tarafından çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="c4726-170">The `SendMessage` method can be called by a connected client to send a message to all clients.</span></span> <span data-ttu-id="c4726-171">Bu yöntemi çağıran JavaScript istemci kodu öğreticinin ilerleyen bölümlerinde gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="c4726-171">JavaScript client code that calls the method is shown later in the tutorial.</span></span> <span data-ttu-id="c4726-172">SignalR kodudur maksimum ölçeklenebilirlik sağlamak için zaman uyumsuz.</span><span class="sxs-lookup"><span data-stu-id="c4726-172">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-signalr"></a><span data-ttu-id="c4726-173">SignalR yapılandırın</span><span class="sxs-lookup"><span data-stu-id="c4726-173">Configure SignalR</span></span>

<span data-ttu-id="c4726-174">SignalR sunucusu, SignalR için SignalR isteklerini iletmek için yapılandırılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="c4726-174">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="c4726-175">Aşağıdaki vurgulanmış kodu ekleyin *Startup.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="c4726-175">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample-snapshot/Startup.cs?highlight=6,30,58)]

  <span data-ttu-id="c4726-176">Bu değişiklikler, ASP.NET Core bağımlılık ekleme ve sistemleri yönlendirme SignalR ekleyin.</span><span class="sxs-lookup"><span data-stu-id="c4726-176">These changes add SignalR to the ASP.NET Core dependency injection and routing systems.</span></span>

## <a name="add-signalr-client-code"></a><span data-ttu-id="c4726-177">SignalR istemci kodu ekleyin</span><span class="sxs-lookup"><span data-stu-id="c4726-177">Add SignalR client code</span></span>

* <span data-ttu-id="c4726-178">İçeriği Değiştir *Pages\Index.cshtml* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="c4726-178">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](signalr/sample-snapshot/Index.cshtml)]

  <span data-ttu-id="c4726-179">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="c4726-179">The preceding code:</span></span>

  * <span data-ttu-id="c4726-180">Metin kutuları için adı ve ileti metni ve bir Gönder düğmesi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="c4726-180">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="c4726-181">İçeren bir liste oluşturur `id="messagesList"` SignalR hub'ından alınan iletileri görüntülemek için.</span><span class="sxs-lookup"><span data-stu-id="c4726-181">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="c4726-182">SignalR için komut dosyası başvuruları içerir ve *chat.js* sonraki adımda oluşturduğunuz uygulama kodu.</span><span class="sxs-lookup"><span data-stu-id="c4726-182">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="c4726-183">İçinde *wwwroot/js* klasör oluşturma bir *chat.js* dosyasındaki kodu aşağıdaki kodla:</span><span class="sxs-lookup"><span data-stu-id="c4726-183">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[Index](signalr/sample-snapshot/chat.js)]

  <span data-ttu-id="c4726-184">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="c4726-184">The preceding code:</span></span>

  * <span data-ttu-id="c4726-185">Oluşturur ve bir bağlantı başlatır.</span><span class="sxs-lookup"><span data-stu-id="c4726-185">Creates and starts a connection.</span></span>
  * <span data-ttu-id="c4726-186">Gönder düğmesine hub'a iletiler gönderen bir işleyici ekler.</span><span class="sxs-lookup"><span data-stu-id="c4726-186">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="c4726-187">Bağlantı nesnesi için hub'ından iletiler alan ve bunları listeye ekler bir işleyici ekler.</span><span class="sxs-lookup"><span data-stu-id="c4726-187">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="c4726-188">Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="c4726-188">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c4726-189">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c4726-189">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="c4726-190">Tuşuna **CTRL + F5** uygulamayı hata ayıklama olmadan çalıştırmak için.</span><span class="sxs-lookup"><span data-stu-id="c4726-190">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c4726-191">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c4726-191">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="c4726-192">Tümleşik terminalde aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="c4726-192">In the integrated terminal, run the following command:</span></span>

  ```console
  dotnet run -p SignalRChat.csproj
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="c4726-193">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c4726-193">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="c4726-194">Menüden **çalıştırın > hata ayıklama olmadan Başlat**.</span><span class="sxs-lookup"><span data-stu-id="c4726-194">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="c4726-195">Adres çubuğundan URL'yi kopyalayın, başka bir tarayıcı örneğinde veya sekmesi açın ve adres çubuğuna URL'yi yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="c4726-195">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="c4726-196">Ya da tarayıcı seçin, bir ad ve ileti girin ve seçin **iletisi gönder** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="c4726-196">Choose either browser, enter a name and message, and select the **Send Message** button.</span></span>

  <span data-ttu-id="c4726-197">Her iki sayfalarında, adını ve iletisini anında görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="c4726-197">The name and message are displayed on both pages instantly.</span></span>

  ![SignalR örnek uygulaması](signalr/_static/signalr-get-started-finished.png)

> [!TIP]
> * <span data-ttu-id="c4726-199">Uygulama işe yaramazsa, tarayıcı Geliştirici Araçları'nı (F12) açın ve konsoluna gidin.</span><span class="sxs-lookup"><span data-stu-id="c4726-199">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="c4726-200">HTML ve JavaScript kodunuza ilgili hatalar görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c4726-200">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="c4726-201">Örneğin, eklediğiniz varsayalım *signalr.js* yönlendirilmiş değerinden farklı bir klasöre.</span><span class="sxs-lookup"><span data-stu-id="c4726-201">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="c4726-202">Bu durumda bu dosyaya başvuru çalışmaz ve bir 404 hatası konsolunda görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="c4726-202">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
>   <span data-ttu-id="c4726-203">![signalr.js bulunamadı hatası](signalr/_static/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="c4726-203">![signalr.js not found error](signalr/_static/f12-console.png)</span></span>
> * <span data-ttu-id="c4726-204">Chrome'da ERR_SPDY_INADEQUATE_TRANSPORT_SECURITY veya NS_ERROR_NET_INADEQUATE_SECURITY Firefox'ta hata alırsanız, geliştirme sertifikanızı güncelleştirmek için şu komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="c4726-204">If you get the error ERR_SPDY_INADEQUATE_TRANSPORT_SECURITY in Chrome or NS_ERROR_NET_INADEQUATE_SECURITY in Firefox, run these commands to update your development certificate:</span></span>
>   ```
>   dotnet dev-certs https --clean
>   dotnet dev-certs https --trust
>   ```

## <a name="next-steps"></a><span data-ttu-id="c4726-205">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="c4726-205">Next steps</span></span>

<span data-ttu-id="c4726-206">SignalR hakkında daha fazla bilgi için girişine bakın:</span><span class="sxs-lookup"><span data-stu-id="c4726-206">To learn more about SignalR, see the introduction:</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="c4726-207">ASP.NET Core signalr'a giriş</span><span class="sxs-lookup"><span data-stu-id="c4726-207">Introduction to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
