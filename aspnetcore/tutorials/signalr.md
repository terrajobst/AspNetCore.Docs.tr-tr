---
title: ASP.NET Core SignalR ile çalışmaya başlama
author: bradygaster
description: Bu öğreticide, ASP.NET Core SignalR kullanan bir sohbet uygulaması oluşturacaksınız.
monikerRange: '>= aspnetcore-3.0'
ms.author: bradyg
ms.custom: mvc
ms.date: 10/03/2019
uid: tutorials/signalr
ms.openlocfilehash: 843416cf00c9241f8c05b1aba041ebbe7a05cf80
ms.sourcegitcommit: 73a451e9a58ac7102f90b608d661d8c23dd9bbaf
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2019
ms.locfileid: "72037636"
---
# <a name="tutorial-get-started-with-aspnet-core-signalr"></a><span data-ttu-id="cceb6-103">Öğretici: ASP.NET Core SignalR ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="cceb6-103">Tutorial: Get started with ASP.NET Core SignalR</span></span>

<span data-ttu-id="cceb6-104">Bu öğreticide, SignalR kullanarak gerçek zamanlı bir uygulama oluşturmanın temelleri öğretilir.</span><span class="sxs-lookup"><span data-stu-id="cceb6-104">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="cceb6-105">Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:</span><span class="sxs-lookup"><span data-stu-id="cceb6-105">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="cceb6-106">Web projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="cceb6-106">Create a web project.</span></span>
> * <span data-ttu-id="cceb6-107">SignalR istemci kitaplığını ekleyin.</span><span class="sxs-lookup"><span data-stu-id="cceb6-107">Add the SignalR client library.</span></span>
> * <span data-ttu-id="cceb6-108">Bir SignalR hub 'ı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="cceb6-108">Create a SignalR hub.</span></span>
> * <span data-ttu-id="cceb6-109">Projeyi SignalR kullanacak şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="cceb6-109">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="cceb6-110">Herhangi bir istemciden tüm bağlı istemcilere ileti gönderen kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="cceb6-110">Add code that sends messages from any client to all connected clients.</span></span>

<span data-ttu-id="cceb6-111">Sonunda, çalışan bir sohbet uygulamanız olacaktır:</span><span class="sxs-lookup"><span data-stu-id="cceb6-111">At the end, you'll have a working chat app:</span></span>

![SignalR örnek uygulaması](signalr/_static/3.x/signalr-get-started-finished.png)

## <a name="prerequisites"></a><span data-ttu-id="cceb6-113">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="cceb6-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="cceb6-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cceb6-114">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="cceb6-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="cceb6-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="cceb6-116">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cceb6-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-web-app-project"></a><span data-ttu-id="cceb6-117">Web uygulaması projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="cceb6-117">Create a web app project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="cceb6-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cceb6-118">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="cceb6-119">Menüden **dosya > yeni proje**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="cceb6-119">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="cceb6-120">**Yeni proje oluştur** iletişim kutusunda **ASP.NET Core Web uygulaması**' nı seçin ve ardından **İleri**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="cceb6-120">In the **Create a new project** dialog, select **ASP.NET Core Web Application**, and then select **Next**.</span></span>

* <span data-ttu-id="cceb6-121">**Yeni projenizi yapılandırın** iletişim kutusunda, proje *signalrchat*adını adlandırın ve ardından **Oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="cceb6-121">In the **Configure your new project** dialog, name the project *SignalRChat*, and then select **Create**.</span></span>

* <span data-ttu-id="cceb6-122">**Yeni bir ASP.NET Core Web uygulaması oluştur** iletişim kutusunda **.net Core** ve **ASP.NET Core 3,0**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="cceb6-122">In the **Create a new ASP.NET Core web Application** dialog, select **.NET Core** and **ASP.NET Core 3.0**.</span></span> 

* <span data-ttu-id="cceb6-123">Razor Pages kullanan bir proje oluşturmak için **Web uygulaması** ' nı seçin ve ardından **Oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="cceb6-123">Select **Web Application** to create a project that uses Razor Pages, and then select **Create**.</span></span>

  ![Visual Studio 'da yeni proje iletişim kutusu](signalr/_static/3.x/signalr-new-project-dialog.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="cceb6-125">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="cceb6-125">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="cceb6-126">[Tümleşik Terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) ' i yeni proje klasörünün oluşturulacağı klasöre açın.</span><span class="sxs-lookup"><span data-stu-id="cceb6-126">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) to the folder in which the new project folder will be created.</span></span>

* <span data-ttu-id="cceb6-127">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="cceb6-127">Run the following commands:</span></span>

   ```dotnetcli
   dotnet new webapp -o SignalRChat
   code -r SignalRChat
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="cceb6-128">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cceb6-128">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="cceb6-129">Menüden **dosya > yeni çözüm**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="cceb6-129">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="cceb6-130">**.NET Core > App > Web uygulaması** ' nı ( **Web uygulaması (Model-View-Controller)** seçmeyin) seçin ve ardından **İleri**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="cceb6-130">Select **.NET Core > App > Web Application** (Don't select **Web Application (Model-View-Controller)**), and then select **Next**.</span></span>

* <span data-ttu-id="cceb6-131">**Hedef çerçevenin** **.NET Core 3,0**olarak ayarlandığından emin olun ve ardından **İleri**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="cceb6-131">Make sure the **Target Framework** is set to **.NET Core 3.0**, and then select **Next**.</span></span>

* <span data-ttu-id="cceb6-132">Projeyi *Signalrchat*olarak adlandırın ve **Oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="cceb6-132">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="cceb6-133">SignalR istemci kitaplığını ekleme</span><span class="sxs-lookup"><span data-stu-id="cceb6-133">Add the SignalR client library</span></span>

<span data-ttu-id="cceb6-134">SignalR sunucu kitaplığı ASP.NET Core 3,0 paylaşılan çerçevesine dahildir.</span><span class="sxs-lookup"><span data-stu-id="cceb6-134">The SignalR server library is included in the ASP.NET Core 3.0 shared framework.</span></span> <span data-ttu-id="cceb6-135">JavaScript istemci kitaplığı projeye otomatik olarak dahil değildir.</span><span class="sxs-lookup"><span data-stu-id="cceb6-135">The JavaScript client library isn't automatically included in the project.</span></span> <span data-ttu-id="cceb6-136">Bu öğreticide, istemci kitaplığını *unpkg*'den almak Için kitaplık Yöneticisi 'Ni (Libman) kullanacaksınız.</span><span class="sxs-lookup"><span data-stu-id="cceb6-136">For this tutorial, you use Library Manager (LibMan) to get the client library from *unpkg*.</span></span> <span data-ttu-id="cceb6-137">unpkg, Node. js Paket Yöneticisi NPM 'de bulunan her şeyi teslim edebilen bir içerik teslim ağı (CDN)).</span><span class="sxs-lookup"><span data-stu-id="cceb6-137">unpkg is a content delivery network (CDN)) that can deliver anything found in npm, the Node.js package manager.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="cceb6-138">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cceb6-138">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="cceb6-139">**Çözüm Gezgini**, projeye sağ tıklayın ve  > **Istemci tarafı kitaplığı** **Ekle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="cceb6-139">In **Solution Explorer**, right-click the project, and select **Add** > **Client-Side Library**.</span></span>

* <span data-ttu-id="cceb6-140">**Istemci tarafı kitaplığı Ekle** Iletişim kutusunda **sağlayıcı** için **unpkg**seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="cceb6-140">In the **Add Client-Side Library** dialog, for **Provider** select **unpkg**.</span></span>

* <span data-ttu-id="cceb6-141">**Kitaplık**için `@microsoft/signalr@latest` girin.</span><span class="sxs-lookup"><span data-stu-id="cceb6-141">For **Library**, enter `@microsoft/signalr@latest`.</span></span>

* <span data-ttu-id="cceb6-142">**Belirli dosyaları seç**' i seçin, *dağ/Browser* klasörünü genişletin ve *SignalR. js* ve *SignalR. min. js*' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="cceb6-142">Select **Choose specific files**, expand the *dist/browser* folder, and select *signalr.js* and *signalr.min.js*.</span></span>

* <span data-ttu-id="cceb6-143">**Hedef konumu** *Wwwroot/js/SignalR/* olarak ayarlayın ve **yüklemeyi**seçin.</span><span class="sxs-lookup"><span data-stu-id="cceb6-143">Set **Target Location** to *wwwroot/js/signalr/*, and select **Install**.</span></span>

  ![Istemci tarafı kitaplığı Ekle iletişim kutusu-kitaplık Seç](signalr/_static/3.x/find-signalr-client-libs-select-files.png)

  <span data-ttu-id="cceb6-145">LibMan, bir *Wwwroot/js/SignalR* klasörü oluşturur ve seçilen dosyaları buna kopyalar.</span><span class="sxs-lookup"><span data-stu-id="cceb6-145">LibMan creates a *wwwroot/js/signalr* folder and copies the selected files to it.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="cceb6-146">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="cceb6-146">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="cceb6-147">Tümleşik terminalde, LibMan 'ı yüklemek için aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="cceb6-147">In the integrated terminal, run the following command to install LibMan.</span></span>

  ```dotnetcli
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="cceb6-148">LibMan kullanarak SignalR istemci kitaplığını almak için aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="cceb6-148">Run the following command to get the SignalR client library by using LibMan.</span></span> <span data-ttu-id="cceb6-149">Çıktıyı görmeden önce birkaç saniye beklemeniz gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="cceb6-149">You might have to wait a few seconds before seeing output.</span></span>

  ```console
  libman install @microsoft/signalr@latest -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="cceb6-150">Parametreler aşağıdaki seçenekleri belirtir:</span><span class="sxs-lookup"><span data-stu-id="cceb6-150">The parameters specify the following options:</span></span>
  * <span data-ttu-id="cceb6-151">Unpkg sağlayıcısını kullanın.</span><span class="sxs-lookup"><span data-stu-id="cceb6-151">Use the unpkg provider.</span></span>
  * <span data-ttu-id="cceb6-152">Dosyaları *Wwwroot/js/SignalR* hedefine kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="cceb6-152">Copy files to the *wwwroot/js/signalr* destination.</span></span>
  * <span data-ttu-id="cceb6-153">Yalnızca belirtilen dosyaları kopyala.</span><span class="sxs-lookup"><span data-stu-id="cceb6-153">Copy only the specified files.</span></span>

  <span data-ttu-id="cceb6-154">Çıktı aşağıdaki örneğe benzer şekilde görünür:</span><span class="sxs-lookup"><span data-stu-id="cceb6-154">The output looks like the following example:</span></span>

  ```console
  wwwroot/js/signalr/dist/browser/signalr.js written to disk
  wwwroot/js/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@microsoft/signalr@latest" to "wwwroot/js/signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="cceb6-155">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cceb6-155">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="cceb6-156">**Terminalde**, Libman 'ı yüklemek için aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="cceb6-156">In the **Terminal**, run the following command to install LibMan.</span></span>

  ```dotnetcli
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="cceb6-157">Proje klasörüne gidin ( *Signalrchat. csproj* dosyasını içeren bir dosya).</span><span class="sxs-lookup"><span data-stu-id="cceb6-157">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="cceb6-158">LibMan kullanarak SignalR istemci kitaplığını almak için aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="cceb6-158">Run the following command to get the SignalR client library by using LibMan.</span></span>

  ```console
  libman install @microsoft/signalr@latest -p unpkg -d wwwroot/js/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="cceb6-159">Parametreler aşağıdaki seçenekleri belirtir:</span><span class="sxs-lookup"><span data-stu-id="cceb6-159">The parameters specify the following options:</span></span>
  * <span data-ttu-id="cceb6-160">Unpkg sağlayıcısını kullanın.</span><span class="sxs-lookup"><span data-stu-id="cceb6-160">Use the unpkg provider.</span></span>
  * <span data-ttu-id="cceb6-161">Dosyaları *Wwwroot/js/SignalR* hedefine kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="cceb6-161">Copy files to the *wwwroot/js/signalr* destination.</span></span>
  * <span data-ttu-id="cceb6-162">Yalnızca belirtilen dosyaları kopyala.</span><span class="sxs-lookup"><span data-stu-id="cceb6-162">Copy only the specified files.</span></span>

  <span data-ttu-id="cceb6-163">Çıktı aşağıdaki örneğe benzer şekilde görünür:</span><span class="sxs-lookup"><span data-stu-id="cceb6-163">The output looks like the following example:</span></span>

  ```console
  wwwroot/js/signalr/dist/browser/signalr.js written to disk
  wwwroot/js/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@microsoft/signalr@latest" to "wwwroot/js/signalr"
  ```

---

## <a name="create-a-signalr-hub"></a><span data-ttu-id="cceb6-164">SignalR hub 'ı oluşturma</span><span class="sxs-lookup"><span data-stu-id="cceb6-164">Create a SignalR hub</span></span>

<span data-ttu-id="cceb6-165">*Hub* , istemci-sunucu iletişimini işleyen yüksek düzeyli bir işlem hattı görevi gören bir sınıftır.</span><span class="sxs-lookup"><span data-stu-id="cceb6-165">A *hub* is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="cceb6-166">SignalRChat proje klasöründe bir *hub* klasörü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="cceb6-166">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="cceb6-167">*Hub 'lar* klasöründe, aşağıdaki kodla bir *ChatHub.cs* dosyası oluşturun:</span><span class="sxs-lookup"><span data-stu-id="cceb6-167">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[ChatHub](signalr/sample-snapshot/3.x/ChatHub.cs)]

  <span data-ttu-id="cceb6-168">@No__t-0 sınıfı, SignalR `Hub` sınıfından devralır.</span><span class="sxs-lookup"><span data-stu-id="cceb6-168">The `ChatHub` class inherits from the SignalR `Hub` class.</span></span> <span data-ttu-id="cceb6-169">@No__t-0 sınıfı bağlantıları, grupları ve mesajlaşmayı yönetir.</span><span class="sxs-lookup"><span data-stu-id="cceb6-169">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="cceb6-170">@No__t-0 yöntemi, tüm istemcilere ileti göndermek için bağlı bir istemci tarafından çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="cceb6-170">The `SendMessage` method can be called by a connected client to send a message to all clients.</span></span> <span data-ttu-id="cceb6-171">Yöntemi çağıran JavaScript istemci kodu Öğreticinin ilerleyen kısımlarında gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="cceb6-171">JavaScript client code that calls the method is shown later in the tutorial.</span></span> <span data-ttu-id="cceb6-172">SignalR kodu, maksimum ölçeklenebilirlik sağlamak için zaman uyumsuzdur.</span><span class="sxs-lookup"><span data-stu-id="cceb6-172">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-signalr"></a><span data-ttu-id="cceb6-173">SignalR 'yi yapılandırma</span><span class="sxs-lookup"><span data-stu-id="cceb6-173">Configure SignalR</span></span>

<span data-ttu-id="cceb6-174">SignalR isteklerini SignalR 'ye iletmek için SignalR sunucusunun yapılandırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="cceb6-174">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="cceb6-175">Aşağıdaki Vurgulanan kodu *Startup.cs* dosyasına ekleyin.</span><span class="sxs-lookup"><span data-stu-id="cceb6-175">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample-snapshot/3.x/Startup.cs?highlight=11,28,55)]

  <span data-ttu-id="cceb6-176">Bu değişiklikler ASP.NET Core bağımlılığı ekleme ve yönlendirme sistemlerine SignalR ekler.</span><span class="sxs-lookup"><span data-stu-id="cceb6-176">These changes add SignalR to the ASP.NET Core dependency injection and routing systems.</span></span>

## <a name="add-signalr-client-code"></a><span data-ttu-id="cceb6-177">SignalR istemci kodu ekle</span><span class="sxs-lookup"><span data-stu-id="cceb6-177">Add SignalR client code</span></span>

* <span data-ttu-id="cceb6-178">*Pages\ındex.cshtml* içindeki içeriği şu kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="cceb6-178">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](signalr/sample-snapshot/3.x/Index.cshtml)]

  <span data-ttu-id="cceb6-179">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="cceb6-179">The preceding code:</span></span>

  * <span data-ttu-id="cceb6-180">Ad ve ileti metni ve Gönder düğmesi için metin kutuları oluşturur.</span><span class="sxs-lookup"><span data-stu-id="cceb6-180">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="cceb6-181">SignalR hub 'ından alınan iletileri görüntülemek için `id="messagesList"` içeren bir liste oluşturur.</span><span class="sxs-lookup"><span data-stu-id="cceb6-181">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="cceb6-182">SignalR için betik başvurularını ve sonraki adımda oluşturduğunuz *chat. js* uygulama kodunu içerir.</span><span class="sxs-lookup"><span data-stu-id="cceb6-182">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="cceb6-183">*Wwwroot/js* klasöründe, aşağıdaki kodla bir *chat. js* dosyası oluşturun:</span><span class="sxs-lookup"><span data-stu-id="cceb6-183">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[chat](signalr/sample-snapshot/3.x/chat.js)]

  <span data-ttu-id="cceb6-184">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="cceb6-184">The preceding code:</span></span>

  * <span data-ttu-id="cceb6-185">Bir bağlantı oluşturur ve başlatır.</span><span class="sxs-lookup"><span data-stu-id="cceb6-185">Creates and starts a connection.</span></span>
  * <span data-ttu-id="cceb6-186">, Hub 'a ileti gönderen bir işleyiciye Gönder düğmesine ekler.</span><span class="sxs-lookup"><span data-stu-id="cceb6-186">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="cceb6-187">, Hub 'dan iletileri alan ve bunları listeye ekleyen bir işleyici olan bağlantı nesnesine ekler.</span><span class="sxs-lookup"><span data-stu-id="cceb6-187">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="cceb6-188">Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="cceb6-188">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="cceb6-189">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cceb6-189">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="cceb6-190">Uygulamayı hata ayıklamadan çalıştırmak için **CTRL + F5** tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="cceb6-190">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="cceb6-191">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="cceb6-191">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="cceb6-192">Tümleşik terminalde aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="cceb6-192">In the integrated terminal, run the following command:</span></span>

  ```dotnetcli
  dotnet watch run -p SignalRChat.csproj
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="cceb6-193">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cceb6-193">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="cceb6-194">Menüden, **hata ayıklama olmadan başlat > Çalıştır**' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="cceb6-194">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="cceb6-195">Adres çubuğundan URL 'yi kopyalayın, başka bir tarayıcı örneği veya sekme açın ve adres çubuğuna URL 'YI yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="cceb6-195">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="cceb6-196">Tarayıcı ' yı seçin, bir ad ve ileti girin ve **Ileti gönder** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="cceb6-196">Choose either browser, enter a name and message, and select the **Send Message** button.</span></span>

  <span data-ttu-id="cceb6-197">Ad ve ileti anında her iki sayfada da görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="cceb6-197">The name and message are displayed on both pages instantly.</span></span>

  ![SignalR örnek uygulaması](signalr/_static/3.x/signalr-get-started-finished.png)

> [!TIP]
> * <span data-ttu-id="cceb6-199">Uygulama işe yaramazsa, tarayıcı geliştirici araçlarınızı (F12) açın ve konsola gidin.</span><span class="sxs-lookup"><span data-stu-id="cceb6-199">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="cceb6-200">HTML ve JavaScript kodunuzla ilgili hatalarla karşılaşabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="cceb6-200">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="cceb6-201">Örneğin, *SignalR. js* ' yi yönlendirenden farklı bir klasöre yerleştirdiğinizi varsayalım.</span><span class="sxs-lookup"><span data-stu-id="cceb6-201">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="cceb6-202">Bu durumda, bu dosyaya başvuru çalışmaz ve konsolunda 404 hatası görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="cceb6-202">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
>   <span data-ttu-id="cceb6-203">@no__t -0signalr. js bulunamadı hatası @ no__t-1</span><span class="sxs-lookup"><span data-stu-id="cceb6-203">![signalr.js not found error](signalr/_static/3.x/f12-console.png)</span></span>
> * <span data-ttu-id="cceb6-204">Chrome 'da ERR_SPDY_INADEQUATE_TRANSPORT_SECURITY hatasını alırsanız, geliştirme sertifikanızı güncelleştirmek için şu komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="cceb6-204">If you get the error ERR_SPDY_INADEQUATE_TRANSPORT_SECURITY in Chrome, run these commands to update your development certificate:</span></span>
>
>   ```dotnetcli
>   dotnet dev-certs https --clean
>   dotnet dev-certs https --trust
>   ```

## <a name="next-steps"></a><span data-ttu-id="cceb6-205">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="cceb6-205">Next steps</span></span>

<span data-ttu-id="cceb6-206">SignalR hakkında daha fazla bilgi edinmek için bkz. giriş:</span><span class="sxs-lookup"><span data-stu-id="cceb6-206">To learn more about SignalR, see the introduction:</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="cceb6-207">ASP.NET Core SignalR 'ye giriş</span><span class="sxs-lookup"><span data-stu-id="cceb6-207">Introduction to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
