---
title: ASP.NET Core SignalR kullanmaya başlama
author: bradygaster
description: Bu öğreticide, ASP.NET Core SignalRkullanan bir sohbet uygulaması oluşturacaksınız.
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: tutorials/signalr
ms.openlocfilehash: 962cc0318ebbfc7fac16ca0947a2e3e83e51665c
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2019
ms.locfileid: "73964039"
---
# <a name="tutorial-get-started-with-aspnet-core-opno-locsignalr"></a><span data-ttu-id="ac7d6-103">Öğretici: ASP.NET Core SignalR ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="ac7d6-103">Tutorial: Get started with ASP.NET Core SignalR</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="ac7d6-104">Bu öğreticide SignalRkullanarak gerçek zamanlı bir uygulama oluşturmanın temelleri öğretilir.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-104">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="ac7d6-105">Aşağıdakilerin nasıl yapıldığını öğreneceksiniz:</span><span class="sxs-lookup"><span data-stu-id="ac7d6-105">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ac7d6-106">Bir Web projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-106">Create a web project.</span></span>
> * <span data-ttu-id="ac7d6-107">SignalR istemci kitaplığı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-107">Add the SignalR client library.</span></span>
> * <span data-ttu-id="ac7d6-108">SignalR hub 'ı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-108">Create a SignalR hub.</span></span>
> * <span data-ttu-id="ac7d6-109">Projeyi SignalRkullanacak şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-109">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="ac7d6-110">Herhangi bir istemciden tüm bağlı istemcilere ileti gönderen kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-110">Add code that sends messages from any client to all connected clients.</span></span>

<span data-ttu-id="ac7d6-111">Sonunda, çalışan bir sohbet uygulamanız olacaktır:</span><span class="sxs-lookup"><span data-stu-id="ac7d6-111">At the end, you'll have a working chat app:</span></span>

![[! Üs. NO-LOC (SignalR)]<span data-ttu-id="ac7d6-112"> örnek uygulama</span><span class="sxs-lookup"><span data-stu-id="ac7d6-112"> sample app</span></span>](signalr/_static/3.x/signalr-get-started-finished.png)

## <a name="prerequisites"></a><span data-ttu-id="ac7d6-113">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="ac7d6-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ac7d6-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ac7d6-114">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ac7d6-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ac7d6-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="ac7d6-116">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ac7d6-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-web-app-project"></a><span data-ttu-id="ac7d6-117">Web uygulaması projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="ac7d6-117">Create a web app project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ac7d6-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ac7d6-118">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="ac7d6-119">Menüden **dosya > yeni proje**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-119">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="ac7d6-120">**Yeni proje oluştur** iletişim kutusunda **ASP.NET Core Web uygulaması**' nı seçin ve ardından **İleri**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-120">In the **Create a new project** dialog, select **ASP.NET Core Web Application**, and then select **Next**.</span></span>

* <span data-ttu-id="ac7d6-121">**Yeni projenizi yapılandırın** iletişim kutusunda, proje *signalrchat*adını adlandırın ve ardından **Oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-121">In the **Configure your new project** dialog, name the project *SignalRChat*, and then select **Create**.</span></span>

* <span data-ttu-id="ac7d6-122">**Yeni bir ASP.NET Core Web uygulaması oluştur** iletişim kutusunda **.net Core** ve **ASP.NET Core 3,0**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-122">In the **Create a new ASP.NET Core web Application** dialog, select **.NET Core** and **ASP.NET Core 3.0**.</span></span> 

* <span data-ttu-id="ac7d6-123">Razor Pages kullanan bir proje oluşturmak için **Web uygulaması** ' nı seçin ve ardından **Oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-123">Select **Web Application** to create a project that uses Razor Pages, and then select **Create**.</span></span>

  ![Visual Studio 'da yeni proje iletişim kutusu](signalr/_static/3.x/signalr-new-project-dialog.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ac7d6-125">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ac7d6-125">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="ac7d6-126">[Tümleşik Terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) ' i yeni proje klasörünün oluşturulacağı klasöre açın.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-126">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) to the folder in which the new project folder will be created.</span></span>

* <span data-ttu-id="ac7d6-127">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="ac7d6-127">Run the following commands:</span></span>

   ```dotnetcli
   dotnet new webapp -o SignalRChat
   code -r SignalRChat
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="ac7d6-128">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ac7d6-128">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="ac7d6-129">Menüden **dosya > yeni çözüm**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-129">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="ac7d6-130">**.NET Core > App > Web uygulaması** ' nı ( **Web uygulaması (Model-View-Controller)** seçmeyin) seçin ve ardından **İleri**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-130">Select **.NET Core > App > Web Application** (Don't select **Web Application (Model-View-Controller)**), and then select **Next**.</span></span>

* <span data-ttu-id="ac7d6-131">**Hedef çerçevenin** **.NET Core 3,0**olarak ayarlandığından emin olun ve ardından **İleri**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-131">Make sure the **Target Framework** is set to **.NET Core 3.0**, and then select **Next**.</span></span>

* <span data-ttu-id="ac7d6-132">Projeyi *Signalrchat*olarak adlandırın ve **Oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-132">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-opno-locsignalr-client-library"></a><span data-ttu-id="ac7d6-133">SignalR istemci kitaplığı ekleme</span><span class="sxs-lookup"><span data-stu-id="ac7d6-133">Add the SignalR client library</span></span>

<span data-ttu-id="ac7d6-134">SignalR sunucusu kitaplığı, ASP.NET Core 3,0 paylaşılan çerçevesine dahildir.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-134">The SignalR server library is included in the ASP.NET Core 3.0 shared framework.</span></span> <span data-ttu-id="ac7d6-135">JavaScript istemci kitaplığı projeye otomatik olarak dahil değildir.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-135">The JavaScript client library isn't automatically included in the project.</span></span> <span data-ttu-id="ac7d6-136">Bu öğreticide, istemci kitaplığını *unpkg*'den almak Için kitaplık Yöneticisi 'Ni (Libman) kullanacaksınız.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-136">For this tutorial, you use Library Manager (LibMan) to get the client library from *unpkg*.</span></span> <span data-ttu-id="ac7d6-137">unpkg, Node. js Paket Yöneticisi NPM 'de bulunan her şeyi teslim edebilen bir içerik teslim ağı (CDN)).</span><span class="sxs-lookup"><span data-stu-id="ac7d6-137">unpkg is a content delivery network (CDN)) that can deliver anything found in npm, the Node.js package manager.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ac7d6-138">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ac7d6-138">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="ac7d6-139">**Çözüm Gezgini**, projeye sağ tıklayın ve > **Istemci tarafı kitaplığı** **Ekle** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-139">In **Solution Explorer**, right-click the project, and select **Add** > **Client-Side Library**.</span></span>

* <span data-ttu-id="ac7d6-140">**Istemci tarafı kitaplığı Ekle** Iletişim kutusunda **sağlayıcı** için **unpkg**seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-140">In the **Add Client-Side Library** dialog, for **Provider** select **unpkg**.</span></span>

* <span data-ttu-id="ac7d6-141">**Kitaplık**için `@microsoft/signalr@latest`girin.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-141">For **Library**, enter `@microsoft/signalr@latest`.</span></span>

* <span data-ttu-id="ac7d6-142">**Belirli dosyaları seç**' i seçin, *dağ/Browser* klasörünü genişletin ve *SignalR. js* ve *SignalR. min. js*' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-142">Select **Choose specific files**, expand the *dist/browser* folder, and select *signalr.js* and *signalr.min.js*.</span></span>

* <span data-ttu-id="ac7d6-143">**Hedef konumu** *Wwwroot/js/SignalR/* olarak ayarlayın ve **yüklemeyi**seçin.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-143">Set **Target Location** to *wwwroot/js/signalr/*, and select **Install**.</span></span>

  ![Istemci tarafı kitaplığı Ekle iletişim kutusu-kitaplık Seç](signalr/_static/3.x/find-signalr-client-libs-select-files.png)

  <span data-ttu-id="ac7d6-145">LibMan, bir *Wwwroot/js/SignalR* klasörü oluşturur ve seçilen dosyaları buna kopyalar.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-145">LibMan creates a *wwwroot/js/signalr* folder and copies the selected files to it.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ac7d6-146">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ac7d6-146">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="ac7d6-147">Tümleşik terminalde, LibMan 'ı yüklemek için aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-147">In the integrated terminal, run the following command to install LibMan.</span></span>

  ```dotnetcli
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="ac7d6-148">SignalR istemci kitaplığını LibMan kullanarak almak için aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-148">Run the following command to get the SignalR client library by using LibMan.</span></span> <span data-ttu-id="ac7d6-149">Çıktıyı görmeden önce birkaç saniye beklemeniz gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-149">You might have to wait a few seconds before seeing output.</span></span>

  ```console
  libman install @microsoft/signalr@latest -p unpkg -d wwwroot/js/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="ac7d6-150">Parametreler aşağıdaki seçenekleri belirtir:</span><span class="sxs-lookup"><span data-stu-id="ac7d6-150">The parameters specify the following options:</span></span>
  * <span data-ttu-id="ac7d6-151">Unpkg sağlayıcısını kullanın.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-151">Use the unpkg provider.</span></span>
  * <span data-ttu-id="ac7d6-152">Dosyaları *Wwwroot/js/SignalR* hedefine kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-152">Copy files to the *wwwroot/js/signalr* destination.</span></span>
  * <span data-ttu-id="ac7d6-153">Yalnızca belirtilen dosyaları kopyala.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-153">Copy only the specified files.</span></span>

  <span data-ttu-id="ac7d6-154">Çıktı aşağıdaki örneğe benzer şekilde görünür:</span><span class="sxs-lookup"><span data-stu-id="ac7d6-154">The output looks like the following example:</span></span>

  ```console
  wwwroot/js/signalr/dist/browser/signalr.js written to disk
  wwwroot/js/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@microsoft/signalr@latest" to "wwwroot/js/signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="ac7d6-155">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ac7d6-155">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="ac7d6-156">**Terminalde**, Libman 'ı yüklemek için aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-156">In the **Terminal**, run the following command to install LibMan.</span></span>

  ```dotnetcli
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="ac7d6-157">Proje klasörüne gidin ( *Signalrchat. csproj* dosyasını içeren bir dosya).</span><span class="sxs-lookup"><span data-stu-id="ac7d6-157">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="ac7d6-158">SignalR istemci kitaplığını LibMan kullanarak almak için aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-158">Run the following command to get the SignalR client library by using LibMan.</span></span>

  ```console
  libman install @microsoft/signalr@latest -p unpkg -d wwwroot/js/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="ac7d6-159">Parametreler aşağıdaki seçenekleri belirtir:</span><span class="sxs-lookup"><span data-stu-id="ac7d6-159">The parameters specify the following options:</span></span>
  * <span data-ttu-id="ac7d6-160">Unpkg sağlayıcısını kullanın.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-160">Use the unpkg provider.</span></span>
  * <span data-ttu-id="ac7d6-161">Dosyaları *Wwwroot/js/SignalR* hedefine kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-161">Copy files to the *wwwroot/js/signalr* destination.</span></span>
  * <span data-ttu-id="ac7d6-162">Yalnızca belirtilen dosyaları kopyala.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-162">Copy only the specified files.</span></span>

  <span data-ttu-id="ac7d6-163">Çıktı aşağıdaki örneğe benzer şekilde görünür:</span><span class="sxs-lookup"><span data-stu-id="ac7d6-163">The output looks like the following example:</span></span>

  ```console
  wwwroot/js/signalr/dist/browser/signalr.js written to disk
  wwwroot/js/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@microsoft/signalr@latest" to "wwwroot/js/signalr"
  ```

---

## <a name="create-a-opno-locsignalr-hub"></a><span data-ttu-id="ac7d6-164">SignalR hub 'ı oluşturma</span><span class="sxs-lookup"><span data-stu-id="ac7d6-164">Create a SignalR hub</span></span>

<span data-ttu-id="ac7d6-165">*Hub* , istemci-sunucu iletişimini işleyen yüksek düzeyli bir işlem hattı görevi gören bir sınıftır.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-165">A *hub* is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="ac7d6-166">SignalRChat proje klasöründe bir *hub* klasörü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-166">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="ac7d6-167">*Hub 'lar* klasöründe, aşağıdaki kodla bir *ChatHub.cs* dosyası oluşturun:</span><span class="sxs-lookup"><span data-stu-id="ac7d6-167">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[ChatHub](signalr/sample-snapshot/3.x/ChatHub.cs)]

  <span data-ttu-id="ac7d6-168">`ChatHub` sınıfı SignalR `Hub` sınıfından devralır.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-168">The `ChatHub` class inherits from the SignalR `Hub` class.</span></span> <span data-ttu-id="ac7d6-169">`Hub` sınıfı bağlantıları, grupları ve mesajlaşmayı yönetir.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-169">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="ac7d6-170">`SendMessage` yöntemi, tüm istemcilere ileti göndermek için bağlı bir istemci tarafından çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-170">The `SendMessage` method can be called by a connected client to send a message to all clients.</span></span> <span data-ttu-id="ac7d6-171">Yöntemi çağıran JavaScript istemci kodu Öğreticinin ilerleyen kısımlarında gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-171">JavaScript client code that calls the method is shown later in the tutorial.</span></span> SignalR<span data-ttu-id="ac7d6-172"> kod, en fazla ölçeklenebilirlik sağlamak için zaman uyumsuzdur.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-172"> code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-opno-locsignalr"></a><span data-ttu-id="ac7d6-173">SignalR Yapılandır</span><span class="sxs-lookup"><span data-stu-id="ac7d6-173">Configure SignalR</span></span>

<span data-ttu-id="ac7d6-174">SignalR sunucusu, SignalRSignalR istekleri geçirilecek şekilde yapılandırılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-174">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="ac7d6-175">Aşağıdaki Vurgulanan kodu *Startup.cs* dosyasına ekleyin.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-175">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample-snapshot/3.x/Startup.cs?highlight=11,28,55)]

  <span data-ttu-id="ac7d6-176">Bu değişiklikler, ASP.NET Core bağımlılığı ekleme ve yönlendirme sistemlerine SignalR ekler.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-176">These changes add SignalR to the ASP.NET Core dependency injection and routing systems.</span></span>

## <a name="add-opno-locsignalr-client-code"></a><span data-ttu-id="ac7d6-177">SignalR istemci kodu ekle</span><span class="sxs-lookup"><span data-stu-id="ac7d6-177">Add SignalR client code</span></span>

* <span data-ttu-id="ac7d6-178">*Pages\ındex.cshtml* içindeki içeriği şu kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="ac7d6-178">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](signalr/sample-snapshot/3.x/Index.cshtml)]

  <span data-ttu-id="ac7d6-179">Önceki kod:</span><span class="sxs-lookup"><span data-stu-id="ac7d6-179">The preceding code:</span></span>

  * <span data-ttu-id="ac7d6-180">Ad ve ileti metni ve Gönder düğmesi için metin kutuları oluşturur.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-180">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="ac7d6-181">SignalR hub 'ından alınan iletileri görüntülemek için `id="messagesList"` içeren bir liste oluşturur.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-181">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="ac7d6-182">Bir sonraki adımda oluşturduğunuz SignalR ve *sohbet. js* uygulama koduna yönelik betik başvurularını içerir.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-182">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="ac7d6-183">*Wwwroot/js* klasöründe, aşağıdaki kodla bir *chat. js* dosyası oluşturun:</span><span class="sxs-lookup"><span data-stu-id="ac7d6-183">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[chat](signalr/sample-snapshot/3.x/chat.js)]

  <span data-ttu-id="ac7d6-184">Önceki kod:</span><span class="sxs-lookup"><span data-stu-id="ac7d6-184">The preceding code:</span></span>

  * <span data-ttu-id="ac7d6-185">Bir bağlantı oluşturur ve başlatır.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-185">Creates and starts a connection.</span></span>
  * <span data-ttu-id="ac7d6-186">, Hub 'a ileti gönderen bir işleyiciye Gönder düğmesine ekler.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-186">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="ac7d6-187">, Hub 'dan iletileri alan ve bunları listeye ekleyen bir işleyici olan bağlantı nesnesine ekler.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-187">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="ac7d6-188">Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="ac7d6-188">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ac7d6-189">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ac7d6-189">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="ac7d6-190">Uygulamayı hata ayıklamadan çalıştırmak için **CTRL + F5** tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-190">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ac7d6-191">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ac7d6-191">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="ac7d6-192">Tümleşik terminalde aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="ac7d6-192">In the integrated terminal, run the following command:</span></span>

  ```dotnetcli
  dotnet watch run -p SignalRChat.csproj
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="ac7d6-193">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ac7d6-193">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="ac7d6-194">Menüden, **hata ayıklama olmadan başlat > Çalıştır**' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-194">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="ac7d6-195">Adres çubuğundan URL 'yi kopyalayın, başka bir tarayıcı örneği veya sekme açın ve adres çubuğuna URL 'YI yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-195">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="ac7d6-196">Tarayıcı ' yı seçin, bir ad ve ileti girin ve **Ileti gönder** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-196">Choose either browser, enter a name and message, and select the **Send Message** button.</span></span>

  <span data-ttu-id="ac7d6-197">Ad ve ileti anında her iki sayfada da görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-197">The name and message are displayed on both pages instantly.</span></span>

  ![[! Üs. NO-LOC (SignalR)]<span data-ttu-id="ac7d6-198"> örnek uygulama</span><span class="sxs-lookup"><span data-stu-id="ac7d6-198"> sample app</span></span>](signalr/_static/3.x/signalr-get-started-finished.png)

> [!TIP]
> * <span data-ttu-id="ac7d6-199">Uygulama işe yaramazsa, tarayıcı geliştirici araçlarınızı (F12) açın ve konsola gidin.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-199">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="ac7d6-200">HTML ve JavaScript kodunuzla ilgili hatalarla karşılaşabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-200">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="ac7d6-201">Örneğin, *SignalR. js* ' yi yönlendirenden farklı bir klasöre yerleştirdiğinizi varsayalım.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-201">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="ac7d6-202">Bu durumda, bu dosyaya başvuru çalışmaz ve konsolunda 404 hatası görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-202">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
>   <span data-ttu-id="ac7d6-203">![SignalR. js bulunamadı hatası](signalr/_static/3.x/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="ac7d6-203">![signalr.js not found error](signalr/_static/3.x/f12-console.png)</span></span>
> * <span data-ttu-id="ac7d6-204">Chrome 'da hata ERR_SPDY_INADEQUATE_TRANSPORT_SECURITY alırsanız, geliştirme sertifikanızı güncelleştirmek için şu komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="ac7d6-204">If you get the error ERR_SPDY_INADEQUATE_TRANSPORT_SECURITY in Chrome, run these commands to update your development certificate:</span></span>
>
>   ```dotnetcli
>   dotnet dev-certs https --clean
>   dotnet dev-certs https --trust
>   ```

## <a name="next-steps"></a><span data-ttu-id="ac7d6-205">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="ac7d6-205">Next steps</span></span>

<span data-ttu-id="ac7d6-206">SignalRhakkında daha fazla bilgi edinmek için bkz. giriş:</span><span class="sxs-lookup"><span data-stu-id="ac7d6-206">To learn more about SignalR, see the introduction:</span></span>

> [!div class="nextstepaction"]
> <span data-ttu-id="ac7d6-207">[ASP.NET Core SignalR giriş](xref:signalr/introduction)</span><span class="sxs-lookup"><span data-stu-id="ac7d6-207">[Introduction to ASP.NET Core SignalR](xref:signalr/introduction)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="ac7d6-208">Bu öğreticide SignalRkullanarak gerçek zamanlı bir uygulama oluşturmanın temelleri öğretilir.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-208">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="ac7d6-209">Aşağıdakilerin nasıl yapıldığını öğreneceksiniz:</span><span class="sxs-lookup"><span data-stu-id="ac7d6-209">You learn how to:</span></span> 

> [!div class="checklist"]  
> * <span data-ttu-id="ac7d6-210">Bir Web projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-210">Create a web project.</span></span>   
> * <span data-ttu-id="ac7d6-211">SignalR istemci kitaplığı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-211">Add the SignalR client library.</span></span>   
> * <span data-ttu-id="ac7d6-212">SignalR hub 'ı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-212">Create a SignalR hub.</span></span> 
> * <span data-ttu-id="ac7d6-213">Projeyi SignalRkullanacak şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-213">Configure the project to use SignalR.</span></span> 
> * <span data-ttu-id="ac7d6-214">Herhangi bir istemciden tüm bağlı istemcilere ileti gönderen kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-214">Add code that sends messages from any client to all connected clients.</span></span>  
<span data-ttu-id="ac7d6-215">Sonunda, çalışan bir sohbet uygulamasına sahipsiniz: ![[! Üs. NO-LOC (SignalR)] örnek uygulama](signalr/_static/2.x/signalr-get-started-finished.png)</span><span class="sxs-lookup"><span data-stu-id="ac7d6-215">At the end, you'll have a working chat app: ![SignalR sample app](signalr/_static/2.x/signalr-get-started-finished.png)</span></span>   

## <a name="prerequisites"></a><span data-ttu-id="ac7d6-216">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="ac7d6-216">Prerequisites</span></span>    

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ac7d6-217">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ac7d6-217">Visual Studio</span></span>](#tab/visual-studio)   

[!INCLUDE[](~/includes/net-core-prereqs-vs2017-2.2.md)] 

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ac7d6-218">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ac7d6-218">Visual Studio Code</span></span>](#tab/visual-studio-code) 

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]    

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="ac7d6-219">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ac7d6-219">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)   

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]    

--- 

## <a name="create-a-web-project"></a><span data-ttu-id="ac7d6-220">Web projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="ac7d6-220">Create a web project</span></span> 

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ac7d6-221">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ac7d6-221">Visual Studio</span></span>](#tab/visual-studio/)  

* <span data-ttu-id="ac7d6-222">Menüden **dosya > yeni proje**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-222">From the menu, select **File > New Project**.</span></span> 

* <span data-ttu-id="ac7d6-223">**Yeni proje** iletişim kutusunda, **yüklü > Visual C# > Web > ASP.NET Core Web uygulaması**' nı seçin.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-223">In the **New Project** dialog, select **Installed > Visual C# > Web > ASP.NET Core Web Application**.</span></span> <span data-ttu-id="ac7d6-224">Projeyi *Signalrchat*olarak adlandırın.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-224">Name the project *SignalRChat*.</span></span> 

  ![Visual Studio 'da yeni proje iletişim kutusu](signalr/_static/2.x/signalr-new-project-dialog.png)    

* <span data-ttu-id="ac7d6-226">Razor Pages kullanan bir proje oluşturmak için **Web uygulaması** ' nı seçin.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-226">Select **Web Application** to create a project that uses Razor Pages.</span></span> 

* <span data-ttu-id="ac7d6-227">**.NET Core**'un hedef çerçevesini seçin, **ASP.NET Core 2,2**' i seçin ve **Tamam**' ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-227">Select a target framework of **.NET Core**, select **ASP.NET Core 2.2**, and click **OK**.</span></span>    

  ![Visual Studio 'da yeni proje iletişim kutusu](signalr/_static/2.x/signalr-new-project-choose-type.png)   

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ac7d6-229">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ac7d6-229">Visual Studio Code</span></span>](#tab/visual-studio-code/)    

* <span data-ttu-id="ac7d6-230">[Tümleşik Terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) ' i yeni proje klasörünün oluşturulacağı klasöre açın.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-230">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) to the folder in which the new project folder will be created.</span></span>  

* <span data-ttu-id="ac7d6-231">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="ac7d6-231">Run the following commands:</span></span>   

   ```dotnetcli 
   dotnet new webapp -o SignalRChat 
   code -r SignalRChat  
   ```  

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="ac7d6-232">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ac7d6-232">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)   

* <span data-ttu-id="ac7d6-233">Menüden **dosya > yeni çözüm**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-233">From the menu, select **File > New Solution**.</span></span>    

* <span data-ttu-id="ac7d6-234">**.NET Core > App > ASP.NET Core Web uygulaması** ' nı ( **ASP.NET Core Web uygulaması (MVC)** seçmeyin) seçin.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-234">Select **.NET Core > App > ASP.NET Core Web App** (Don't select **ASP.NET Core Web App (MVC)**).</span></span>  

* <span data-ttu-id="ac7d6-235">**İleri ' yi**seçin.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-235">Select **Next**.</span></span>  

* <span data-ttu-id="ac7d6-236">Projeyi *Signalrchat*olarak adlandırın ve **Oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-236">Name the project *SignalRChat*, and then select **Create**.</span></span>   

--- 

## <a name="add-the-opno-locsignalr-client-library"></a><span data-ttu-id="ac7d6-237">SignalR istemci kitaplığı ekleme</span><span class="sxs-lookup"><span data-stu-id="ac7d6-237">Add the SignalR client library</span></span> 

<span data-ttu-id="ac7d6-238">SignalR sunucusu kitaplığı, `Microsoft.AspNetCore.App` metapackage 'e dahildir.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-238">The SignalR server library is included in the `Microsoft.AspNetCore.App` metapackage.</span></span> <span data-ttu-id="ac7d6-239">JavaScript istemci kitaplığı projeye otomatik olarak dahil değildir.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-239">The JavaScript client library isn't automatically included in the project.</span></span> <span data-ttu-id="ac7d6-240">Bu öğreticide, istemci kitaplığını *unpkg*'den almak Için kitaplık Yöneticisi 'Ni (Libman) kullanacaksınız.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-240">For this tutorial, you use Library Manager (LibMan) to get the client library from *unpkg*.</span></span> <span data-ttu-id="ac7d6-241">unpkg, Node. js Paket Yöneticisi NPM 'de bulunan her şeyi teslim edebilen bir içerik teslim ağı (CDN)).</span><span class="sxs-lookup"><span data-stu-id="ac7d6-241">unpkg is a content delivery network (CDN)) that can deliver anything found in npm, the Node.js package manager.</span></span>  

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ac7d6-242">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ac7d6-242">Visual Studio</span></span>](#tab/visual-studio/)  

* <span data-ttu-id="ac7d6-243">**Çözüm Gezgini**, projeye sağ tıklayın ve > **Istemci tarafı kitaplığı** **Ekle** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-243">In **Solution Explorer**, right-click the project, and select **Add** > **Client-Side Library**.</span></span>  

* <span data-ttu-id="ac7d6-244">**Istemci tarafı kitaplığı Ekle** Iletişim kutusunda **sağlayıcı** için **unpkg**seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-244">In the **Add Client-Side Library** dialog, for **Provider** select **unpkg**.</span></span> 

* <span data-ttu-id="ac7d6-245">**Kitaplık**için `@aspnet/signalr@1` girin ve Önizleme olmayan en son sürümü seçin.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-245">For **Library**, enter `@aspnet/signalr@1`, and select the latest version that isn't preview.</span></span> 

  ![Istemci tarafı kitaplığı Ekle iletişim kutusu-kitaplık Seç](signalr/_static/2.x/libman1.png)   

* <span data-ttu-id="ac7d6-247">**Belirli dosyaları seç**' i seçin, *dağ/Browser* klasörünü genişletin ve *SignalR. js* ve *SignalR. min. js*' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-247">Select **Choose specific files**, expand the *dist/browser* folder, and select *signalr.js* and *signalr.min.js*.</span></span> 

* <span data-ttu-id="ac7d6-248">**Hedef konumu** *Wwwroot/lib/SignalR/* olarak ayarlayın ve **yüklemeyi**seçin.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-248">Set **Target Location** to *wwwroot/lib/signalr/*, and select **Install**.</span></span>    

  ![Istemci tarafı kitaplığı Ekle iletişim kutusu-dosya ve hedef seçme](signalr/_static/2.x/libman2.png) 

  <span data-ttu-id="ac7d6-250">LibMan, bir *Wwwroot/LIB/SignalR* klasörü oluşturur ve seçilen dosyaları buna kopyalar.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-250">LibMan creates a *wwwroot/lib/signalr* folder and copies the selected files to it.</span></span>    

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ac7d6-251">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ac7d6-251">Visual Studio Code</span></span>](#tab/visual-studio-code/)    

* <span data-ttu-id="ac7d6-252">Tümleşik terminalde, LibMan 'ı yüklemek için aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-252">In the integrated terminal, run the following command to install LibMan.</span></span>  

  ```dotnetcli  
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli   
  ```   

* <span data-ttu-id="ac7d6-253">SignalR istemci kitaplığını LibMan kullanarak almak için aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-253">Run the following command to get the SignalR client library by using LibMan.</span></span> <span data-ttu-id="ac7d6-254">Çıktıyı görmeden önce birkaç saniye beklemeniz gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-254">You might have to wait a few seconds before seeing output.</span></span> 

  ```console    
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js    
  ```   

  <span data-ttu-id="ac7d6-255">Parametreler aşağıdaki seçenekleri belirtir:</span><span class="sxs-lookup"><span data-stu-id="ac7d6-255">The parameters specify the following options:</span></span> 
  * <span data-ttu-id="ac7d6-256">Unpkg sağlayıcısını kullanın.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-256">Use the unpkg provider.</span></span> 
  * <span data-ttu-id="ac7d6-257">Dosyaları *Wwwroot/lib/SignalR* hedefine kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-257">Copy files to the *wwwroot/lib/signalr* destination.</span></span>    
  * <span data-ttu-id="ac7d6-258">Yalnızca belirtilen dosyaları kopyala.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-258">Copy only the specified files.</span></span>  

  <span data-ttu-id="ac7d6-259">Çıktı aşağıdaki örneğe benzer şekilde görünür:</span><span class="sxs-lookup"><span data-stu-id="ac7d6-259">The output looks like the following example:</span></span>  

  ```console    
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk   
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk   
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"    
  ```   

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="ac7d6-260">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ac7d6-260">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)   

* <span data-ttu-id="ac7d6-261">**Terminalde**, Libman 'ı yüklemek için aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-261">In the **Terminal**, run the following command to install LibMan.</span></span> 

  ```dotnetcli  
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli   
  ```   

* <span data-ttu-id="ac7d6-262">Proje klasörüne gidin ( *Signalrchat. csproj* dosyasını içeren bir dosya).</span><span class="sxs-lookup"><span data-stu-id="ac7d6-262">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span> 

* <span data-ttu-id="ac7d6-263">SignalR istemci kitaplığını LibMan kullanarak almak için aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-263">Run the following command to get the SignalR client library by using LibMan.</span></span>    

  ```console    
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js    
  ```   

  <span data-ttu-id="ac7d6-264">Parametreler aşağıdaki seçenekleri belirtir:</span><span class="sxs-lookup"><span data-stu-id="ac7d6-264">The parameters specify the following options:</span></span> 
  * <span data-ttu-id="ac7d6-265">Unpkg sağlayıcısını kullanın.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-265">Use the unpkg provider.</span></span> 
  * <span data-ttu-id="ac7d6-266">Dosyaları *Wwwroot/lib/SignalR* hedefine kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-266">Copy files to the *wwwroot/lib/signalr* destination.</span></span>    
  * <span data-ttu-id="ac7d6-267">Yalnızca belirtilen dosyaları kopyala.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-267">Copy only the specified files.</span></span>  

  <span data-ttu-id="ac7d6-268">Çıktı aşağıdaki örneğe benzer şekilde görünür:</span><span class="sxs-lookup"><span data-stu-id="ac7d6-268">The output looks like the following example:</span></span>  

  ```console    
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk   
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk   
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"    
  ```   

--- 

## <a name="create-a-opno-locsignalr-hub"></a><span data-ttu-id="ac7d6-269">SignalR hub 'ı oluşturma</span><span class="sxs-lookup"><span data-stu-id="ac7d6-269">Create a SignalR hub</span></span>   

<span data-ttu-id="ac7d6-270">*Hub* , istemci-sunucu iletişimini işleyen yüksek düzeyli bir işlem hattı görevi gören bir sınıftır.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-270">A *hub* is a class that serves as a high-level pipeline that handles client-server communication.</span></span>   

* <span data-ttu-id="ac7d6-271">SignalRChat proje klasöründe bir *hub* klasörü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-271">In the SignalRChat project folder, create a *Hubs* folder.</span></span>    

* <span data-ttu-id="ac7d6-272">*Hub 'lar* klasöründe, aşağıdaki kodla bir *ChatHub.cs* dosyası oluşturun:</span><span class="sxs-lookup"><span data-stu-id="ac7d6-272">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span> 

  [!code-csharp[Startup](signalr/sample-snapshot/2.x/ChatHub.cs)]   

  <span data-ttu-id="ac7d6-273">`ChatHub` sınıfı SignalR `Hub` sınıfından devralır.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-273">The `ChatHub` class inherits from the SignalR `Hub` class.</span></span> <span data-ttu-id="ac7d6-274">`Hub` sınıfı bağlantıları, grupları ve mesajlaşmayı yönetir.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-274">The `Hub` class manages connections, groups, and messaging.</span></span>  

  <span data-ttu-id="ac7d6-275">`SendMessage` yöntemi, tüm istemcilere ileti göndermek için bağlı bir istemci tarafından çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-275">The `SendMessage` method can be called by a connected client to send a message to all clients.</span></span> <span data-ttu-id="ac7d6-276">Yöntemi çağıran JavaScript istemci kodu Öğreticinin ilerleyen kısımlarında gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-276">JavaScript client code that calls the method is shown later in the tutorial.</span></span> SignalR<span data-ttu-id="ac7d6-277"> kod, en fazla ölçeklenebilirlik sağlamak için zaman uyumsuzdur.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-277"> code is asynchronous to provide maximum scalability.</span></span>    

## <a name="configure-opno-locsignalr"></a><span data-ttu-id="ac7d6-278">SignalR Yapılandır</span><span class="sxs-lookup"><span data-stu-id="ac7d6-278">Configure SignalR</span></span>  

<span data-ttu-id="ac7d6-279">SignalR sunucusu, SignalRSignalR istekleri geçirilecek şekilde yapılandırılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-279">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>    

* <span data-ttu-id="ac7d6-280">Aşağıdaki Vurgulanan kodu *Startup.cs* dosyasına ekleyin.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-280">Add the following highlighted code to the *Startup.cs* file.</span></span>  

  [!code-csharp[Startup](signalr/sample-snapshot/2.x/Startup.cs?highlight=7,33,52-55)]  

  <span data-ttu-id="ac7d6-281">Bu değişiklikler, ASP.NET Core bağımlılık ekleme sistemine ve ara yazılım ardışık düzenine SignalR ekler.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-281">These changes add SignalR to the ASP.NET Core dependency injection system and the middleware pipeline.</span></span>  

## <a name="add-opno-locsignalr-client-code"></a><span data-ttu-id="ac7d6-282">SignalR istemci kodu ekle</span><span class="sxs-lookup"><span data-stu-id="ac7d6-282">Add SignalR client code</span></span>    

* <span data-ttu-id="ac7d6-283">*Pages\ındex.cshtml* içindeki içeriği şu kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="ac7d6-283">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>  

  [!code-cshtml[Index](signalr/sample-snapshot/2.x/Index.cshtml)]   

  <span data-ttu-id="ac7d6-284">Önceki kod:</span><span class="sxs-lookup"><span data-stu-id="ac7d6-284">The preceding code:</span></span>   

  * <span data-ttu-id="ac7d6-285">Ad ve ileti metni ve Gönder düğmesi için metin kutuları oluşturur.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-285">Creates text boxes for name and message text, and a submit button.</span></span>  
  * <span data-ttu-id="ac7d6-286">SignalR hub 'ından alınan iletileri görüntülemek için `id="messagesList"` içeren bir liste oluşturur.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-286">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>   
  * <span data-ttu-id="ac7d6-287">Bir sonraki adımda oluşturduğunuz SignalR ve *sohbet. js* uygulama koduna yönelik betik başvurularını içerir.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-287">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>    

* <span data-ttu-id="ac7d6-288">*Wwwroot/js* klasöründe, aşağıdaki kodla bir *chat. js* dosyası oluşturun:</span><span class="sxs-lookup"><span data-stu-id="ac7d6-288">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>  

  [!code-javascript[Index](signalr/sample-snapshot/2.x/chat.js)]    

  <span data-ttu-id="ac7d6-289">Önceki kod:</span><span class="sxs-lookup"><span data-stu-id="ac7d6-289">The preceding code:</span></span>   

  * <span data-ttu-id="ac7d6-290">Bir bağlantı oluşturur ve başlatır.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-290">Creates and starts a connection.</span></span>    
  * <span data-ttu-id="ac7d6-291">, Hub 'a ileti gönderen bir işleyiciye Gönder düğmesine ekler.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-291">Adds to the submit button a handler that sends messages to the hub.</span></span> 
  * <span data-ttu-id="ac7d6-292">, Hub 'dan iletileri alan ve bunları listeye ekleyen bir işleyici olan bağlantı nesnesine ekler.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-292">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>  

## <a name="run-the-app"></a><span data-ttu-id="ac7d6-293">Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="ac7d6-293">Run the app</span></span>  

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ac7d6-294">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ac7d6-294">Visual Studio</span></span>](#tab/visual-studio)   

* <span data-ttu-id="ac7d6-295">Uygulamayı hata ayıklamadan çalıştırmak için **CTRL + F5** tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-295">Press **CTRL+F5** to run the app without debugging.</span></span>   

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ac7d6-296">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ac7d6-296">Visual Studio Code</span></span>](#tab/visual-studio-code) 

* <span data-ttu-id="ac7d6-297">Tümleşik terminalde aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="ac7d6-297">In the integrated terminal, run the following command:</span></span>    

  ```dotnetcli  
  dotnet run -p SignalRChat.csproj  
  ```   

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="ac7d6-298">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ac7d6-298">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)   

* <span data-ttu-id="ac7d6-299">Menüden, **hata ayıklama olmadan başlat > Çalıştır**' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-299">From the menu, select **Run > Start Without Debugging**.</span></span>  

--- 

* <span data-ttu-id="ac7d6-300">Adres çubuğundan URL 'yi kopyalayın, başka bir tarayıcı örneği veya sekme açın ve adres çubuğuna URL 'YI yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-300">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>    

* <span data-ttu-id="ac7d6-301">Tarayıcı ' yı seçin, bir ad ve ileti girin ve **Ileti gönder** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-301">Choose either browser, enter a name and message, and select the **Send Message** button.</span></span>  

  <span data-ttu-id="ac7d6-302">Ad ve ileti anında her iki sayfada da görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-302">The name and message are displayed on both pages instantly.</span></span>   

  ![[! Üs. NO-LOC (SignalR)]<span data-ttu-id="ac7d6-303"> örnek uygulama</span><span class="sxs-lookup"><span data-stu-id="ac7d6-303"> sample app</span></span>](signalr/_static/2.x/signalr-get-started-finished.png) 

> [!TIP]    
> <span data-ttu-id="ac7d6-304">Uygulama işe yaramazsa, tarayıcı geliştirici araçlarınızı (F12) açın ve konsola gidin.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-304">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="ac7d6-305">HTML ve JavaScript kodunuzla ilgili hatalarla karşılaşabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-305">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="ac7d6-306">Örneğin, *SignalR. js* ' yi yönlendirenden farklı bir klasöre yerleştirdiğinizi varsayalım.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-306">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="ac7d6-307">Bu durumda, bu dosyaya başvuru çalışmaz ve konsolunda 404 hatası görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-307">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>   
> <span data-ttu-id="ac7d6-308">![SignalR. js bulunamadı hatası](signalr/_static/2.x/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="ac7d6-308">![signalr.js not found error](signalr/_static/2.x/f12-console.png)</span></span>    
## <a name="additional-resources"></a><span data-ttu-id="ac7d6-309">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="ac7d6-309">Additional resources</span></span> 
* [<span data-ttu-id="ac7d6-310">Bu öğreticinin YouTube sürümü</span><span class="sxs-lookup"><span data-stu-id="ac7d6-310">Youtube version of this tutorial</span></span>](https://www.youtube.com/watch?v=iKlVmu-r0JQ)   

## <a name="next-steps"></a><span data-ttu-id="ac7d6-311">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="ac7d6-311">Next steps</span></span>   

<span data-ttu-id="ac7d6-312">Bu öğreticide, nasıl yapılacağını öğrendiniz:</span><span class="sxs-lookup"><span data-stu-id="ac7d6-312">In this tutorial, you learned how to:</span></span>   

> [!div class="checklist"]  
> * <span data-ttu-id="ac7d6-313">Bir Web uygulaması projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-313">Create a web app project.</span></span>   
> * <span data-ttu-id="ac7d6-314">SignalR istemci kitaplığı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-314">Add the SignalR client library.</span></span>   
> * <span data-ttu-id="ac7d6-315">SignalR hub 'ı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-315">Create a SignalR hub.</span></span> 
> * <span data-ttu-id="ac7d6-316">Projeyi SignalRkullanacak şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-316">Configure the project to use SignalR.</span></span> 
> * <span data-ttu-id="ac7d6-317">Herhangi bir istemciden tüm bağlı istemcilere ileti göndermek için hub 'ı kullanan kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="ac7d6-317">Add code that uses the hub to send messages from any client to all connected clients.</span></span>   
<span data-ttu-id="ac7d6-318">SignalRhakkında daha fazla bilgi edinmek için bkz. giriş:</span><span class="sxs-lookup"><span data-stu-id="ac7d6-318">To learn more about SignalR, see the introduction:</span></span>    
> [!div class="nextstepaction"] 
> <span data-ttu-id="ac7d6-319">[ASP.NET Core SignalR giriş](xref:signalr/introduction)</span><span class="sxs-lookup"><span data-stu-id="ac7d6-319">[Introduction to ASP.NET Core SignalR](xref:signalr/introduction)</span></span>   
::: moniker-end

