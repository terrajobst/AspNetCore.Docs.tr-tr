---
title: ASP.NET Core SignalR ile çalışmaya başlama
author: bradygaster
description: Bu öğreticide, ASP.NET Core SignalR kullanan bir sohbet uygulaması oluşturacaksınız.
ms.author: bradyg
ms.custom: mvc
ms.date: 07/08/2019
uid: tutorials/signalr
ms.openlocfilehash: 2dfa994b9763a0139cb70cbf9847ac3b02b568e4
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71081963"
---
# <a name="tutorial-get-started-with-aspnet-core-signalr"></a><span data-ttu-id="22736-103">Öğretici: ASP.NET Core SignalR ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="22736-103">Tutorial: Get started with ASP.NET Core SignalR</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="22736-104">Bu öğreticide, SignalR kullanarak gerçek zamanlı bir uygulama oluşturmanın temelleri öğretilir.</span><span class="sxs-lookup"><span data-stu-id="22736-104">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="22736-105">Aşağıdakilerin nasıl yapıldığını öğreneceksiniz:</span><span class="sxs-lookup"><span data-stu-id="22736-105">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="22736-106">Web projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="22736-106">Create a web project.</span></span>
> * <span data-ttu-id="22736-107">SignalR istemci kitaplığını ekleyin.</span><span class="sxs-lookup"><span data-stu-id="22736-107">Add the SignalR client library.</span></span>
> * <span data-ttu-id="22736-108">Bir SignalR hub 'ı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="22736-108">Create a SignalR hub.</span></span>
> * <span data-ttu-id="22736-109">Projeyi SignalR kullanacak şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="22736-109">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="22736-110">Herhangi bir istemciden tüm bağlı istemcilere ileti gönderen kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="22736-110">Add code that sends messages from any client to all connected clients.</span></span>

<span data-ttu-id="22736-111">Sonunda, çalışan bir sohbet uygulamanız olacaktır:</span><span class="sxs-lookup"><span data-stu-id="22736-111">At the end, you'll have a working chat app:</span></span>

![SignalR örnek uygulaması](signalr/_static/3.x/signalr-get-started-finished.png)

## <a name="prerequisites"></a><span data-ttu-id="22736-113">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="22736-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="22736-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="22736-114">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="22736-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="22736-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="22736-116">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="22736-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-web-app-project"></a><span data-ttu-id="22736-117">Web uygulaması projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="22736-117">Create a web app project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="22736-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="22736-118">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="22736-119">Menüden **dosya > yeni proje**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="22736-119">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="22736-120">**Yeni proje oluştur** iletişim kutusunda **ASP.NET Core Web uygulaması**' nı seçin ve ardından **İleri**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="22736-120">In the **Create a new project** dialog, select **ASP.NET Core Web Application**, and then select **Next**.</span></span>

* <span data-ttu-id="22736-121">**Yeni projenizi yapılandırın** iletişim kutusunda, proje *signalrchat*adını adlandırın ve ardından **Oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="22736-121">In the **Configure your new project** dialog, name the project *SignalRChat*, and then select **Create**.</span></span>

* <span data-ttu-id="22736-122">**Yeni bir ASP.NET Core Web uygulaması oluştur** iletişim kutusunda **.net Core** ve **ASP.NET Core 3,0**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="22736-122">In the **Create a new ASP.NET Core Web Application** dialog, select **.NET Core** and **ASP.NET Core 3.0**.</span></span> 

* <span data-ttu-id="22736-123">Razor Pages kullanan bir proje oluşturmak için **Web uygulaması** ' nı seçin ve ardından **Oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="22736-123">Select **Web Application** to create a project that uses Razor Pages, and then select **Create**.</span></span>

  ![Visual Studio 'da yeni proje iletişim kutusu](signalr/_static/3.x/signalr-new-project-dialog.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="22736-125">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="22736-125">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="22736-126">[Tümleşik Terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) ' i yeni proje klasörünün oluşturulacağı klasöre açın.</span><span class="sxs-lookup"><span data-stu-id="22736-126">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) to the folder in which the new project folder will be created.</span></span>

* <span data-ttu-id="22736-127">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="22736-127">Run the following commands:</span></span>

   ```dotnetcli
   dotnet new webapp -o SignalRChat
   code -r SignalRChat
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="22736-128">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="22736-128">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="22736-129">Menüden **dosya > yeni çözüm**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="22736-129">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="22736-130">**.NET Core > App > Web uygulaması** ' nı ( **Web uygulaması (Model-View-Controller)** seçmeyin) seçin ve ardından **İleri**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="22736-130">Select **.NET Core > App > Web Application** (Don't select **Web Application (Model-View-Controller)**), and then select **Next**.</span></span>

* <span data-ttu-id="22736-131">**Hedef çerçevenin** **.NET Core 3,0**olarak ayarlandığından emin olun ve ardından **İleri**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="22736-131">Make sure the **Target Framework** is set to **.NET Core 3.0**, and then select **Next**.</span></span>

* <span data-ttu-id="22736-132">Projeyi *Signalrchat*olarak adlandırın ve **Oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="22736-132">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="22736-133">SignalR istemci kitaplığını ekleme</span><span class="sxs-lookup"><span data-stu-id="22736-133">Add the SignalR client library</span></span>

<span data-ttu-id="22736-134">SignalR sunucu kitaplığı ASP.NET Core 3,0 paylaşılan çerçevesine dahildir.</span><span class="sxs-lookup"><span data-stu-id="22736-134">The SignalR server library is included in the ASP.NET Core 3.0 shared framework.</span></span> <span data-ttu-id="22736-135">JavaScript istemci kitaplığı projeye otomatik olarak dahil değildir.</span><span class="sxs-lookup"><span data-stu-id="22736-135">The JavaScript client library isn't automatically included in the project.</span></span> <span data-ttu-id="22736-136">Bu öğreticide, istemci kitaplığını *unpkg*'den almak Için kitaplık Yöneticisi 'Ni (Libman) kullanacaksınız.</span><span class="sxs-lookup"><span data-stu-id="22736-136">For this tutorial, you use Library Manager (LibMan) to get the client library from *unpkg*.</span></span> <span data-ttu-id="22736-137">unpkg, Node. js Paket Yöneticisi NPM 'de bulunan her şeyi teslim edebilen bir içerik teslim ağı (CDN)).</span><span class="sxs-lookup"><span data-stu-id="22736-137">unpkg is a content delivery network (CDN)) that can deliver anything found in npm, the Node.js package manager.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="22736-138">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="22736-138">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="22736-139">**Çözüm Gezgini**' de projeye sağ tıklayın ve **istemci tarafı kitaplığı** **Ekle** > ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="22736-139">In **Solution Explorer**, right-click the project, and select **Add** > **Client-Side Library**.</span></span>

* <span data-ttu-id="22736-140">**Istemci tarafı kitaplığı Ekle** Iletişim kutusunda **sağlayıcı** için **unpkg**seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="22736-140">In the **Add Client-Side Library** dialog, for **Provider** select **unpkg**.</span></span>

* <span data-ttu-id="22736-141">**Kitaplık**için girin `@aspnet/signalr@next`.</span><span class="sxs-lookup"><span data-stu-id="22736-141">For **Library**, enter `@aspnet/signalr@next`.</span></span>
<!-- when 3.0 is released, change @next to @latest -->

* <span data-ttu-id="22736-142">**Belirli dosyaları seç**' i seçin, *dağ/Browser* klasörünü genişletin ve *SignalR. js* ve *SignalR. min. js*' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="22736-142">Select **Choose specific files**, expand the *dist/browser* folder, and select *signalr.js* and *signalr.min.js*.</span></span>

* <span data-ttu-id="22736-143">**Hedef konumu** *Wwwroot/lib/SignalR/* olarak ayarlayın ve **yüklemeyi**seçin.</span><span class="sxs-lookup"><span data-stu-id="22736-143">Set **Target Location** to *wwwroot/lib/signalr/*, and select **Install**.</span></span>

  ![Istemci tarafı kitaplığı Ekle iletişim kutusu-kitaplık Seç](signalr/_static/3.x/libman1.png)

  <span data-ttu-id="22736-145">LibMan, bir *Wwwroot/LIB/SignalR* klasörü oluşturur ve seçilen dosyaları buna kopyalar.</span><span class="sxs-lookup"><span data-stu-id="22736-145">LibMan creates a *wwwroot/lib/signalr* folder and copies the selected files to it.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="22736-146">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="22736-146">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="22736-147">Tümleşik terminalde, LibMan 'ı yüklemek için aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="22736-147">In the integrated terminal, run the following command to install LibMan.</span></span>

  ```dotnetcli
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="22736-148">LibMan kullanarak SignalR istemci kitaplığını almak için aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="22736-148">Run the following command to get the SignalR client library by using LibMan.</span></span> <span data-ttu-id="22736-149">Çıktıyı görmeden önce birkaç saniye beklemeniz gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="22736-149">You might have to wait a few seconds before seeing output.</span></span>

  ```console
  libman install @aspnet/signalr@next -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="22736-150">Parametreler aşağıdaki seçenekleri belirtir:</span><span class="sxs-lookup"><span data-stu-id="22736-150">The parameters specify the following options:</span></span>
  * <span data-ttu-id="22736-151">Unpkg sağlayıcısını kullanın.</span><span class="sxs-lookup"><span data-stu-id="22736-151">Use the unpkg provider.</span></span>
  * <span data-ttu-id="22736-152">Dosyaları *Wwwroot/lib/SignalR* hedefine kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="22736-152">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="22736-153">Yalnızca belirtilen dosyaları kopyala.</span><span class="sxs-lookup"><span data-stu-id="22736-153">Copy only the specified files.</span></span>

  <span data-ttu-id="22736-154">Çıktı aşağıdaki örneğe benzer şekilde görünür:</span><span class="sxs-lookup"><span data-stu-id="22736-154">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@next" to "wwwroot/lib/signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="22736-155">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="22736-155">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="22736-156">**Terminalde**, Libman 'ı yüklemek için aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="22736-156">In the **Terminal**, run the following command to install LibMan.</span></span>

  ```dotnetcli
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="22736-157">Proje klasörüne gidin ( *Signalrchat. csproj* dosyasını içeren bir dosya).</span><span class="sxs-lookup"><span data-stu-id="22736-157">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="22736-158">LibMan kullanarak SignalR istemci kitaplığını almak için aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="22736-158">Run the following command to get the SignalR client library by using LibMan.</span></span>

  ```console
  libman install @aspnet/signalr@next -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="22736-159">Parametreler aşağıdaki seçenekleri belirtir:</span><span class="sxs-lookup"><span data-stu-id="22736-159">The parameters specify the following options:</span></span>
  * <span data-ttu-id="22736-160">Unpkg sağlayıcısını kullanın.</span><span class="sxs-lookup"><span data-stu-id="22736-160">Use the unpkg provider.</span></span>
  * <span data-ttu-id="22736-161">Dosyaları *Wwwroot/lib/SignalR* hedefine kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="22736-161">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="22736-162">Yalnızca belirtilen dosyaları kopyala.</span><span class="sxs-lookup"><span data-stu-id="22736-162">Copy only the specified files.</span></span>

  <span data-ttu-id="22736-163">Çıktı aşağıdaki örneğe benzer şekilde görünür:</span><span class="sxs-lookup"><span data-stu-id="22736-163">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@next" to "wwwroot/lib/signalr"
  ```

---

## <a name="create-a-signalr-hub"></a><span data-ttu-id="22736-164">SignalR hub 'ı oluşturma</span><span class="sxs-lookup"><span data-stu-id="22736-164">Create a SignalR hub</span></span>

<span data-ttu-id="22736-165">*Hub* , istemci-sunucu iletişimini işleyen yüksek düzeyli bir işlem hattı görevi gören bir sınıftır.</span><span class="sxs-lookup"><span data-stu-id="22736-165">A *hub* is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="22736-166">SignalRChat proje klasöründe bir *hub* klasörü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="22736-166">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="22736-167">*Hub 'lar* klasöründe, aşağıdaki kodla bir *ChatHub.cs* dosyası oluşturun:</span><span class="sxs-lookup"><span data-stu-id="22736-167">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[Startup](signalr/sample-snapshot/3.x/ChatHub.cs)]

  <span data-ttu-id="22736-168">Sınıfı, SignalR `Hub` sınıfından devralır. `ChatHub`</span><span class="sxs-lookup"><span data-stu-id="22736-168">The `ChatHub` class inherits from the SignalR `Hub` class.</span></span> <span data-ttu-id="22736-169">`Hub` Sınıfı bağlantıları, grupları ve mesajlaşmayı yönetir.</span><span class="sxs-lookup"><span data-stu-id="22736-169">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="22736-170">Yöntemi `SendMessage` , tüm istemcilere ileti göndermek için bağlı bir istemci tarafından çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="22736-170">The `SendMessage` method can be called by a connected client to send a message to all clients.</span></span> <span data-ttu-id="22736-171">Yöntemi çağıran JavaScript istemci kodu Öğreticinin ilerleyen kısımlarında gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="22736-171">JavaScript client code that calls the method is shown later in the tutorial.</span></span> <span data-ttu-id="22736-172">SignalR kodu, maksimum ölçeklenebilirlik sağlamak için zaman uyumsuzdur.</span><span class="sxs-lookup"><span data-stu-id="22736-172">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-signalr"></a><span data-ttu-id="22736-173">SignalR 'yi yapılandırma</span><span class="sxs-lookup"><span data-stu-id="22736-173">Configure SignalR</span></span>

<span data-ttu-id="22736-174">SignalR isteklerini SignalR 'ye iletmek için SignalR sunucusunun yapılandırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="22736-174">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="22736-175">Aşağıdaki Vurgulanan kodu *Startup.cs* dosyasına ekleyin.</span><span class="sxs-lookup"><span data-stu-id="22736-175">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample-snapshot/3.x/Startup.cs?highlight=6,30,58)]

  <span data-ttu-id="22736-176">Bu değişiklikler ASP.NET Core bağımlılığı ekleme ve yönlendirme sistemlerine SignalR ekler.</span><span class="sxs-lookup"><span data-stu-id="22736-176">These changes add SignalR to the ASP.NET Core dependency injection and routing systems.</span></span>

## <a name="add-signalr-client-code"></a><span data-ttu-id="22736-177">SignalR istemci kodu ekle</span><span class="sxs-lookup"><span data-stu-id="22736-177">Add SignalR client code</span></span>

* <span data-ttu-id="22736-178">*Pages\ındex.cshtml* içindeki içeriği şu kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="22736-178">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](signalr/sample-snapshot/3.x/Index.cshtml)]

  <span data-ttu-id="22736-179">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="22736-179">The preceding code:</span></span>

  * <span data-ttu-id="22736-180">Ad ve ileti metni ve Gönder düğmesi için metin kutuları oluşturur.</span><span class="sxs-lookup"><span data-stu-id="22736-180">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="22736-181">SignalR hub 'ından `id="messagesList"` alınan iletileri görüntülemek için içeren bir liste oluşturur.</span><span class="sxs-lookup"><span data-stu-id="22736-181">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="22736-182">SignalR için betik başvurularını ve sonraki adımda oluşturduğunuz *chat. js* uygulama kodunu içerir.</span><span class="sxs-lookup"><span data-stu-id="22736-182">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="22736-183">*Wwwroot/js* klasöründe, aşağıdaki kodla bir *chat. js* dosyası oluşturun:</span><span class="sxs-lookup"><span data-stu-id="22736-183">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[Index](signalr/sample-snapshot/3.x/chat.js)]

  <span data-ttu-id="22736-184">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="22736-184">The preceding code:</span></span>

  * <span data-ttu-id="22736-185">Bir bağlantı oluşturur ve başlatır.</span><span class="sxs-lookup"><span data-stu-id="22736-185">Creates and starts a connection.</span></span>
  * <span data-ttu-id="22736-186">, Hub 'a ileti gönderen bir işleyiciye Gönder düğmesine ekler.</span><span class="sxs-lookup"><span data-stu-id="22736-186">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="22736-187">, Hub 'dan iletileri alan ve bunları listeye ekleyen bir işleyici olan bağlantı nesnesine ekler.</span><span class="sxs-lookup"><span data-stu-id="22736-187">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="22736-188">Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="22736-188">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="22736-189">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="22736-189">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="22736-190">Uygulamayı hata ayıklamadan çalıştırmak için **CTRL + F5** tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="22736-190">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="22736-191">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="22736-191">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="22736-192">Tümleşik terminalde aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="22736-192">In the integrated terminal, run the following command:</span></span>

  ```dotnetcli
  dotnet run -p SignalRChat.csproj
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="22736-193">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="22736-193">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="22736-194">Menüden, **hata ayıklama olmadan başlat > Çalıştır**' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="22736-194">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="22736-195">Adres çubuğundan URL 'yi kopyalayın, başka bir tarayıcı örneği veya sekme açın ve adres çubuğuna URL 'YI yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="22736-195">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="22736-196">Tarayıcı ' yı seçin, bir ad ve ileti girin ve **Ileti gönder** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="22736-196">Choose either browser, enter a name and message, and select the **Send Message** button.</span></span>

  <span data-ttu-id="22736-197">Ad ve ileti anında her iki sayfada da görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="22736-197">The name and message are displayed on both pages instantly.</span></span>

  ![SignalR örnek uygulaması](signalr/_static/3.x/signalr-get-started-finished.png)

> [!TIP]
> * <span data-ttu-id="22736-199">Uygulama işe yaramazsa, tarayıcı geliştirici araçlarınızı (F12) açın ve konsola gidin.</span><span class="sxs-lookup"><span data-stu-id="22736-199">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="22736-200">HTML ve JavaScript kodunuzla ilgili hatalarla karşılaşabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="22736-200">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="22736-201">Örneğin, *SignalR. js* ' yi yönlendirenden farklı bir klasöre yerleştirdiğinizi varsayalım.</span><span class="sxs-lookup"><span data-stu-id="22736-201">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="22736-202">Bu durumda, bu dosyaya başvuru çalışmaz ve konsolunda 404 hatası görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="22736-202">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
>   <span data-ttu-id="22736-203">![SignalR. js bulunamadı hatası](signalr/_static/3.x/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="22736-203">![signalr.js not found error](signalr/_static/3.x/f12-console.png)</span></span>
> * <span data-ttu-id="22736-204">ERR_SPDY_INADEQUATE_TRANSPORT_SECURITY hatasını, Firefox 'ta Chrome veya NS_ERROR_NET_INADEQUATE_SECURITY ' de alırsanız, geliştirme sertifikanızı güncelleştirmek için şu komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="22736-204">If you get the error ERR_SPDY_INADEQUATE_TRANSPORT_SECURITY in Chrome or NS_ERROR_NET_INADEQUATE_SECURITY in Firefox, run these commands to update your development certificate:</span></span>
>
>   ```dotnetcli
>   dotnet dev-certs https --clean
>   dotnet dev-certs https --trust
>   ```

## <a name="next-steps"></a><span data-ttu-id="22736-205">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="22736-205">Next steps</span></span>

<span data-ttu-id="22736-206">SignalR hakkında daha fazla bilgi edinmek için bkz. giriş:</span><span class="sxs-lookup"><span data-stu-id="22736-206">To learn more about SignalR, see the introduction:</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="22736-207">ASP.NET Core SignalR 'ye giriş</span><span class="sxs-lookup"><span data-stu-id="22736-207">Introduction to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="22736-208">Bu öğreticide, SignalR kullanarak gerçek zamanlı bir uygulama oluşturmanın temelleri öğretilir.</span><span class="sxs-lookup"><span data-stu-id="22736-208">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="22736-209">Aşağıdakilerin nasıl yapıldığını öğreneceksiniz:</span><span class="sxs-lookup"><span data-stu-id="22736-209">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="22736-210">Web projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="22736-210">Create a web project.</span></span>
> * <span data-ttu-id="22736-211">SignalR istemci kitaplığını ekleyin.</span><span class="sxs-lookup"><span data-stu-id="22736-211">Add the SignalR client library.</span></span>
> * <span data-ttu-id="22736-212">Bir SignalR hub 'ı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="22736-212">Create a SignalR hub.</span></span>
> * <span data-ttu-id="22736-213">Projeyi SignalR kullanacak şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="22736-213">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="22736-214">Herhangi bir istemciden tüm bağlı istemcilere ileti gönderen kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="22736-214">Add code that sends messages from any client to all connected clients.</span></span>

<span data-ttu-id="22736-215">Sonunda, çalışan bir sohbet uygulamanız olacaktır:</span><span class="sxs-lookup"><span data-stu-id="22736-215">At the end, you'll have a working chat app:</span></span>

![SignalR örnek uygulaması](signalr/_static/2.x/signalr-get-started-finished.png)

## <a name="prerequisites"></a><span data-ttu-id="22736-217">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="22736-217">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="22736-218">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="22736-218">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2017-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="22736-219">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="22736-219">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="22736-220">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="22736-220">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="22736-221">Bir web projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="22736-221">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="22736-222">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="22736-222">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="22736-223">Menüden **dosya > yeni proje**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="22736-223">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="22736-224">**Yeni proje** iletişim kutusunda, **yüklü > Visual C# > Web > ASP.NET Core Web uygulaması**' nı seçin.</span><span class="sxs-lookup"><span data-stu-id="22736-224">In the **New Project** dialog, select **Installed > Visual C# > Web > ASP.NET Core Web Application**.</span></span> <span data-ttu-id="22736-225">Projeyi *Signalrchat*olarak adlandırın.</span><span class="sxs-lookup"><span data-stu-id="22736-225">Name the project *SignalRChat*.</span></span>

  ![Visual Studio 'da yeni proje iletişim kutusu](signalr/_static/2.x/signalr-new-project-dialog.png)

* <span data-ttu-id="22736-227">Razor Pages kullanan bir proje oluşturmak için **Web uygulaması** ' nı seçin.</span><span class="sxs-lookup"><span data-stu-id="22736-227">Select **Web Application** to create a project that uses Razor Pages.</span></span>

* <span data-ttu-id="22736-228">**.NET Core**'un hedef çerçevesini seçin, **ASP.NET Core 2,2**' i seçin ve **Tamam**' ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="22736-228">Select a target framework of **.NET Core**, select **ASP.NET Core 2.2**, and click **OK**.</span></span>

  ![Visual Studio 'da yeni proje iletişim kutusu](signalr/_static/2.x/signalr-new-project-choose-type.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="22736-230">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="22736-230">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="22736-231">[Tümleşik Terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) ' i yeni proje klasörünün oluşturulacağı klasöre açın.</span><span class="sxs-lookup"><span data-stu-id="22736-231">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) to the folder in which the new project folder will be created.</span></span>

* <span data-ttu-id="22736-232">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="22736-232">Run the following commands:</span></span>

   ```dotnetcli
   dotnet new webapp -o SignalRChat
   code -r SignalRChat
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="22736-233">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="22736-233">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="22736-234">Menüden **dosya > yeni çözüm**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="22736-234">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="22736-235">**.NET Core > App > ASP.NET Core Web uygulaması** ' nı ( **ASP.NET Core Web uygulaması (MVC)** seçmeyin) seçin.</span><span class="sxs-lookup"><span data-stu-id="22736-235">Select **.NET Core > App > ASP.NET Core Web App** (Don't select **ASP.NET Core Web App (MVC)**).</span></span>

* <span data-ttu-id="22736-236">**İleri**’yi seçin.</span><span class="sxs-lookup"><span data-stu-id="22736-236">Select **Next**.</span></span>

* <span data-ttu-id="22736-237">Projeyi *Signalrchat*olarak adlandırın ve **Oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="22736-237">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="22736-238">SignalR istemci kitaplığını ekleme</span><span class="sxs-lookup"><span data-stu-id="22736-238">Add the SignalR client library</span></span>

<span data-ttu-id="22736-239">SignalR sunucu kitaplığı, `Microsoft.AspNetCore.App` metapackage 'e dahildir.</span><span class="sxs-lookup"><span data-stu-id="22736-239">The SignalR server library is included in the `Microsoft.AspNetCore.App` metapackage.</span></span> <span data-ttu-id="22736-240">JavaScript istemci kitaplığı projeye otomatik olarak dahil değildir.</span><span class="sxs-lookup"><span data-stu-id="22736-240">The JavaScript client library isn't automatically included in the project.</span></span> <span data-ttu-id="22736-241">Bu öğreticide, istemci kitaplığını *unpkg*'den almak Için kitaplık Yöneticisi 'Ni (Libman) kullanacaksınız.</span><span class="sxs-lookup"><span data-stu-id="22736-241">For this tutorial, you use Library Manager (LibMan) to get the client library from *unpkg*.</span></span> <span data-ttu-id="22736-242">unpkg, Node. js Paket Yöneticisi NPM 'de bulunan her şeyi teslim edebilen bir içerik teslim ağı (CDN)).</span><span class="sxs-lookup"><span data-stu-id="22736-242">unpkg is a content delivery network (CDN)) that can deliver anything found in npm, the Node.js package manager.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="22736-243">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="22736-243">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="22736-244">**Çözüm Gezgini**' de projeye sağ tıklayın ve **istemci tarafı kitaplığı** **Ekle** > ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="22736-244">In **Solution Explorer**, right-click the project, and select **Add** > **Client-Side Library**.</span></span>

* <span data-ttu-id="22736-245">**Istemci tarafı kitaplığı Ekle** Iletişim kutusunda **sağlayıcı** için **unpkg**seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="22736-245">In the **Add Client-Side Library** dialog, for **Provider** select **unpkg**.</span></span>

* <span data-ttu-id="22736-246">**Kitaplık**için, öğesini `@aspnet/signalr@1`girin ve Önizleme olmayan en son sürümü seçin.</span><span class="sxs-lookup"><span data-stu-id="22736-246">For **Library**, enter `@aspnet/signalr@1`, and select the latest version that isn't preview.</span></span>

  ![Istemci tarafı kitaplığı Ekle iletişim kutusu-kitaplık Seç](signalr/_static/2.x/libman1.png)

* <span data-ttu-id="22736-248">**Belirli dosyaları seç**' i seçin, *dağ/Browser* klasörünü genişletin ve *SignalR. js* ve *SignalR. min. js*' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="22736-248">Select **Choose specific files**, expand the *dist/browser* folder, and select *signalr.js* and *signalr.min.js*.</span></span>

* <span data-ttu-id="22736-249">**Hedef konumu** *Wwwroot/lib/SignalR/* olarak ayarlayın ve **yüklemeyi**seçin.</span><span class="sxs-lookup"><span data-stu-id="22736-249">Set **Target Location** to *wwwroot/lib/signalr/*, and select **Install**.</span></span>

  ![Istemci tarafı kitaplığı Ekle iletişim kutusu-dosya ve hedef seçme](signalr/_static/2.x/libman2.png)

  <span data-ttu-id="22736-251">LibMan, bir *Wwwroot/LIB/SignalR* klasörü oluşturur ve seçilen dosyaları buna kopyalar.</span><span class="sxs-lookup"><span data-stu-id="22736-251">LibMan creates a *wwwroot/lib/signalr* folder and copies the selected files to it.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="22736-252">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="22736-252">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="22736-253">Tümleşik terminalde, LibMan 'ı yüklemek için aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="22736-253">In the integrated terminal, run the following command to install LibMan.</span></span>

  ```dotnetcli
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="22736-254">LibMan kullanarak SignalR istemci kitaplığını almak için aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="22736-254">Run the following command to get the SignalR client library by using LibMan.</span></span> <span data-ttu-id="22736-255">Çıktıyı görmeden önce birkaç saniye beklemeniz gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="22736-255">You might have to wait a few seconds before seeing output.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="22736-256">Parametreler aşağıdaki seçenekleri belirtir:</span><span class="sxs-lookup"><span data-stu-id="22736-256">The parameters specify the following options:</span></span>
  * <span data-ttu-id="22736-257">Unpkg sağlayıcısını kullanın.</span><span class="sxs-lookup"><span data-stu-id="22736-257">Use the unpkg provider.</span></span>
  * <span data-ttu-id="22736-258">Dosyaları *Wwwroot/lib/SignalR* hedefine kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="22736-258">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="22736-259">Yalnızca belirtilen dosyaları kopyala.</span><span class="sxs-lookup"><span data-stu-id="22736-259">Copy only the specified files.</span></span>

  <span data-ttu-id="22736-260">Çıktı aşağıdaki örneğe benzer şekilde görünür:</span><span class="sxs-lookup"><span data-stu-id="22736-260">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="22736-261">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="22736-261">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="22736-262">**Terminalde**, Libman 'ı yüklemek için aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="22736-262">In the **Terminal**, run the following command to install LibMan.</span></span>

  ```dotnetcli
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="22736-263">Proje klasörüne gidin ( *Signalrchat. csproj* dosyasını içeren bir dosya).</span><span class="sxs-lookup"><span data-stu-id="22736-263">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="22736-264">LibMan kullanarak SignalR istemci kitaplığını almak için aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="22736-264">Run the following command to get the SignalR client library by using LibMan.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="22736-265">Parametreler aşağıdaki seçenekleri belirtir:</span><span class="sxs-lookup"><span data-stu-id="22736-265">The parameters specify the following options:</span></span>
  * <span data-ttu-id="22736-266">Unpkg sağlayıcısını kullanın.</span><span class="sxs-lookup"><span data-stu-id="22736-266">Use the unpkg provider.</span></span>
  * <span data-ttu-id="22736-267">Dosyaları *Wwwroot/lib/SignalR* hedefine kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="22736-267">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="22736-268">Yalnızca belirtilen dosyaları kopyala.</span><span class="sxs-lookup"><span data-stu-id="22736-268">Copy only the specified files.</span></span>

  <span data-ttu-id="22736-269">Çıktı aşağıdaki örneğe benzer şekilde görünür:</span><span class="sxs-lookup"><span data-stu-id="22736-269">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

---

## <a name="create-a-signalr-hub"></a><span data-ttu-id="22736-270">SignalR hub 'ı oluşturma</span><span class="sxs-lookup"><span data-stu-id="22736-270">Create a SignalR hub</span></span>

<span data-ttu-id="22736-271">*Hub* , istemci-sunucu iletişimini işleyen yüksek düzeyli bir işlem hattı görevi gören bir sınıftır.</span><span class="sxs-lookup"><span data-stu-id="22736-271">A *hub* is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="22736-272">SignalRChat proje klasöründe bir *hub* klasörü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="22736-272">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="22736-273">*Hub 'lar* klasöründe, aşağıdaki kodla bir *ChatHub.cs* dosyası oluşturun:</span><span class="sxs-lookup"><span data-stu-id="22736-273">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[Startup](signalr/sample-snapshot/2.x/ChatHub.cs)]

  <span data-ttu-id="22736-274">Sınıfı, SignalR `Hub` sınıfından devralır. `ChatHub`</span><span class="sxs-lookup"><span data-stu-id="22736-274">The `ChatHub` class inherits from the SignalR `Hub` class.</span></span> <span data-ttu-id="22736-275">`Hub` Sınıfı bağlantıları, grupları ve mesajlaşmayı yönetir.</span><span class="sxs-lookup"><span data-stu-id="22736-275">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="22736-276">Yöntemi `SendMessage` , tüm istemcilere ileti göndermek için bağlı bir istemci tarafından çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="22736-276">The `SendMessage` method can be called by a connected client to send a message to all clients.</span></span> <span data-ttu-id="22736-277">Yöntemi çağıran JavaScript istemci kodu Öğreticinin ilerleyen kısımlarında gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="22736-277">JavaScript client code that calls the method is shown later in the tutorial.</span></span> <span data-ttu-id="22736-278">SignalR kodu, maksimum ölçeklenebilirlik sağlamak için zaman uyumsuzdur.</span><span class="sxs-lookup"><span data-stu-id="22736-278">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-signalr"></a><span data-ttu-id="22736-279">SignalR 'yi yapılandırma</span><span class="sxs-lookup"><span data-stu-id="22736-279">Configure SignalR</span></span>

<span data-ttu-id="22736-280">SignalR isteklerini SignalR 'ye iletmek için SignalR sunucusunun yapılandırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="22736-280">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="22736-281">Aşağıdaki Vurgulanan kodu *Startup.cs* dosyasına ekleyin.</span><span class="sxs-lookup"><span data-stu-id="22736-281">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample-snapshot/2.x/Startup.cs?highlight=7,33,52-55)]

  <span data-ttu-id="22736-282">Bu değişiklikler ASP.NET Core bağımlılığı ekleme sistemine ve ara yazılım ardışık düzenine SignalR ekler.</span><span class="sxs-lookup"><span data-stu-id="22736-282">These changes add SignalR to the ASP.NET Core dependency injection system and the middleware pipeline.</span></span>

## <a name="add-signalr-client-code"></a><span data-ttu-id="22736-283">SignalR istemci kodu ekle</span><span class="sxs-lookup"><span data-stu-id="22736-283">Add SignalR client code</span></span>

* <span data-ttu-id="22736-284">*Pages\ındex.cshtml* içindeki içeriği şu kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="22736-284">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](signalr/sample-snapshot/2.x/Index.cshtml)]

  <span data-ttu-id="22736-285">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="22736-285">The preceding code:</span></span>

  * <span data-ttu-id="22736-286">Ad ve ileti metni ve Gönder düğmesi için metin kutuları oluşturur.</span><span class="sxs-lookup"><span data-stu-id="22736-286">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="22736-287">SignalR hub 'ından `id="messagesList"` alınan iletileri görüntülemek için içeren bir liste oluşturur.</span><span class="sxs-lookup"><span data-stu-id="22736-287">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="22736-288">SignalR için betik başvurularını ve sonraki adımda oluşturduğunuz *chat. js* uygulama kodunu içerir.</span><span class="sxs-lookup"><span data-stu-id="22736-288">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="22736-289">*Wwwroot/js* klasöründe, aşağıdaki kodla bir *chat. js* dosyası oluşturun:</span><span class="sxs-lookup"><span data-stu-id="22736-289">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[Index](signalr/sample-snapshot/2.x/chat.js)]

  <span data-ttu-id="22736-290">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="22736-290">The preceding code:</span></span>

  * <span data-ttu-id="22736-291">Bir bağlantı oluşturur ve başlatır.</span><span class="sxs-lookup"><span data-stu-id="22736-291">Creates and starts a connection.</span></span>
  * <span data-ttu-id="22736-292">, Hub 'a ileti gönderen bir işleyiciye Gönder düğmesine ekler.</span><span class="sxs-lookup"><span data-stu-id="22736-292">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="22736-293">, Hub 'dan iletileri alan ve bunları listeye ekleyen bir işleyici olan bağlantı nesnesine ekler.</span><span class="sxs-lookup"><span data-stu-id="22736-293">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="22736-294">Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="22736-294">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="22736-295">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="22736-295">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="22736-296">Uygulamayı hata ayıklamadan çalıştırmak için **CTRL + F5** tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="22736-296">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="22736-297">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="22736-297">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="22736-298">Tümleşik terminalde aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="22736-298">In the integrated terminal, run the following command:</span></span>

  ```dotnetcli
  dotnet run -p SignalRChat.csproj
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="22736-299">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="22736-299">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="22736-300">Menüden, **hata ayıklama olmadan başlat > Çalıştır**' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="22736-300">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="22736-301">Adres çubuğundan URL 'yi kopyalayın, başka bir tarayıcı örneği veya sekme açın ve adres çubuğuna URL 'YI yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="22736-301">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="22736-302">Tarayıcı ' yı seçin, bir ad ve ileti girin ve **Ileti gönder** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="22736-302">Choose either browser, enter a name and message, and select the **Send Message** button.</span></span>

  <span data-ttu-id="22736-303">Ad ve ileti anında her iki sayfada da görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="22736-303">The name and message are displayed on both pages instantly.</span></span>

  ![SignalR örnek uygulaması](signalr/_static/2.x/signalr-get-started-finished.png)

> [!TIP]
> <span data-ttu-id="22736-305">Uygulama işe yaramazsa, tarayıcı geliştirici araçlarınızı (F12) açın ve konsola gidin.</span><span class="sxs-lookup"><span data-stu-id="22736-305">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="22736-306">HTML ve JavaScript kodunuzla ilgili hatalarla karşılaşabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="22736-306">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="22736-307">Örneğin, *SignalR. js* ' yi yönlendirenden farklı bir klasöre yerleştirdiğinizi varsayalım.</span><span class="sxs-lookup"><span data-stu-id="22736-307">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="22736-308">Bu durumda, bu dosyaya başvuru çalışmaz ve konsolunda 404 hatası görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="22736-308">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
> <span data-ttu-id="22736-309">![SignalR. js bulunamadı hatası](signalr/_static/2.x/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="22736-309">![signalr.js not found error](signalr/_static/2.x/f12-console.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="22736-310">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="22736-310">Next steps</span></span>

<span data-ttu-id="22736-311">Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:</span><span class="sxs-lookup"><span data-stu-id="22736-311">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="22736-312">Bir Web uygulaması projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="22736-312">Create a web app project.</span></span>
> * <span data-ttu-id="22736-313">SignalR istemci kitaplığını ekleyin.</span><span class="sxs-lookup"><span data-stu-id="22736-313">Add the SignalR client library.</span></span>
> * <span data-ttu-id="22736-314">Bir SignalR hub 'ı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="22736-314">Create a SignalR hub.</span></span>
> * <span data-ttu-id="22736-315">Projeyi SignalR kullanacak şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="22736-315">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="22736-316">Herhangi bir istemciden tüm bağlı istemcilere ileti göndermek için hub 'ı kullanan kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="22736-316">Add code that uses the hub to send messages from any client to all connected clients.</span></span>

<span data-ttu-id="22736-317">SignalR hakkında daha fazla bilgi edinmek için bkz. giriş:</span><span class="sxs-lookup"><span data-stu-id="22736-317">To learn more about SignalR, see the introduction:</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="22736-318">ASP.NET Core SignalR 'ye giriş</span><span class="sxs-lookup"><span data-stu-id="22736-318">Introduction to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)

::: moniker-end
