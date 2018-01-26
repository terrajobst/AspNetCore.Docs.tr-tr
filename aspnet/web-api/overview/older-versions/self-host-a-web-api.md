---
uid: web-api/overview/older-versions/self-host-a-web-api
title: "ASP.NET Web API 1 (C#) kendini barındırma | Microsoft Docs"
author: MikeWasson
description: "ASP.NET Web API IIS gerektirmez. Kendi ana bilgisayar işlemi otomatik olarak bir web API barındırabilir. Bu öğretici, bir konsol applic içinde bir web API barındırmak gösterilmektedir..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2012
ms.topic: article
ms.assetid: be5ab1e2-4140-4275-ac59-ca82a1bac0c1
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/self-host-a-web-api
msc.type: authoredcontent
ms.openlocfilehash: 564f859e73a88ac9c5f27e9b8f7409ec126642f8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="self-host-aspnet-web-api-1-c"></a><span data-ttu-id="8855d-105">ASP.NET Web API 1 (C#) kendini barındırma</span><span class="sxs-lookup"><span data-stu-id="8855d-105">Self-Host ASP.NET Web API 1 (C#)</span></span>
====================
<span data-ttu-id="8855d-106">tarafından [CAN Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="8855d-106">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="8855d-107">ASP.NET Web API IIS gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="8855d-107">ASP.NET Web API does not require IIS.</span></span> <span data-ttu-id="8855d-108">Kendi ana bilgisayar işlemi otomatik olarak bir web API barındırabilir.</span><span class="sxs-lookup"><span data-stu-id="8855d-108">You can self-host a web API in your own host process.</span></span> <span data-ttu-id="8855d-109">Bu öğretici, bir konsol uygulaması içindeki bir web API barındırmak gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="8855d-109">This tutorial shows how to host a web API inside a console application.</span></span>
> 
> <span data-ttu-id="8855d-110">**Yeni uygulamalar OWIN Web API Self barındırmak için kullanmanız gerekir.**</span><span class="sxs-lookup"><span data-stu-id="8855d-110">**New applications should use OWIN to self-host Web API.**</span></span> <span data-ttu-id="8855d-111">Bkz: [OWIN ASP.NET Web API 2 Self barındırmak için kullandığınız](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="8855d-111">See [Use OWIN to Self-Host ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="8855d-112">Öğreticide kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="8855d-112">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="8855d-113">Web API 1</span><span class="sxs-lookup"><span data-stu-id="8855d-113">Web API 1</span></span>
> - <span data-ttu-id="8855d-114">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="8855d-114">Visual Studio 2012</span></span>


## <a name="create-the-console-application-project"></a><span data-ttu-id="8855d-115">Konsol uygulama projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="8855d-115">Create the Console Application Project</span></span>

<span data-ttu-id="8855d-116">Visual Studio'yu başlatın ve seçin **yeni proje** gelen **Başlat** sayfası.</span><span class="sxs-lookup"><span data-stu-id="8855d-116">Start Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="8855d-117">Veya **dosya** menüsünde, select **yeni** ve ardından **proje**.</span><span class="sxs-lookup"><span data-stu-id="8855d-117">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="8855d-118">İçinde **şablonları** bölmesinde, **yüklü şablonlar** ve genişletin **Visual C#** düğümü.</span><span class="sxs-lookup"><span data-stu-id="8855d-118">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="8855d-119">Altında **Visual C#**seçin **Windows**.</span><span class="sxs-lookup"><span data-stu-id="8855d-119">Under **Visual C#**, select **Windows**.</span></span> <span data-ttu-id="8855d-120">Proje şablonları listesinde seçin **konsol uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="8855d-120">In the list of project templates, select **Console Application**.</span></span> <span data-ttu-id="8855d-121">Proje adı &quot;SelfHost&quot; tıklatıp **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="8855d-121">Name the project &quot;SelfHost&quot; and click **OK**.</span></span>

![](self-host-a-web-api/_static/image1.png)

## <a name="set-the-target-framework-visual-studio-2010"></a><span data-ttu-id="8855d-122">Hedef Framework'ü (Visual Studio 2010) ayarlayın</span><span class="sxs-lookup"><span data-stu-id="8855d-122">Set the Target Framework (Visual Studio 2010)</span></span>

<span data-ttu-id="8855d-123">Visual Studio 2010 kullanıyorsanız, .NET Framework 4.0 hedef çerçevesini değiştirebilir.</span><span class="sxs-lookup"><span data-stu-id="8855d-123">If you are using Visual Studio 2010, change the target framework to .NET Framework 4.0.</span></span> <span data-ttu-id="8855d-124">(Varsayılan olarak, proje şablonu hedefleri [.Net Framework istemci profili](https://msdn.microsoft.com/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile).)</span><span class="sxs-lookup"><span data-stu-id="8855d-124">(By default, the project template targets the [.Net Framework Client Profile](https://msdn.microsoft.com/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile).)</span></span>

<span data-ttu-id="8855d-125">Çözüm Gezgini'nde projeye sağ tıklayın ve seçin **özellikleri**.</span><span class="sxs-lookup"><span data-stu-id="8855d-125">In Solution Explorer, right-click the project and select **Properties**.</span></span> <span data-ttu-id="8855d-126">İçinde **hedef framework** açılır listesinde, .NET Framework 4.0 hedef çerçevesini değiştirebilir.</span><span class="sxs-lookup"><span data-stu-id="8855d-126">In the **Target framework** dropdown list, change the target framework to .NET Framework 4.0.</span></span> <span data-ttu-id="8855d-127">Değişikliği uygulamak için istendiğinde tıklatın **Evet**.</span><span class="sxs-lookup"><span data-stu-id="8855d-127">When prompted to apply the change, click **Yes**.</span></span>

![](self-host-a-web-api/_static/image2.png)

## <a name="install-nuget-package-manager"></a><span data-ttu-id="8855d-128">NuGet Paket Yöneticisi'ni yükleyin</span><span class="sxs-lookup"><span data-stu-id="8855d-128">Install NuGet Package Manager</span></span>

<span data-ttu-id="8855d-129">NuGet Paket Yöneticisi, Web API derlemeleri ASP.NET olmayan projeye eklemek için kolay bir yoludur.</span><span class="sxs-lookup"><span data-stu-id="8855d-129">The NuGet Package Manager is the easiest way to add the Web API assemblies to a non-ASP.NET project.</span></span>

<span data-ttu-id="8855d-130">NuGet Paket Yöneticisi yüklü olup olmadığını denetlemek için **Araçları** Visual Studio menüsünde.</span><span class="sxs-lookup"><span data-stu-id="8855d-130">To check if NuGet Package Manager is installed, click the **Tools** menu in Visual Studio.</span></span> <span data-ttu-id="8855d-131">Öğe adı verilen bir menü görürseniz **kitaplık Paket Yöneticisi**, NuGet Paket Yöneticisi sahip.</span><span class="sxs-lookup"><span data-stu-id="8855d-131">If you see a menu item called **Library Package Manager**, then you have NuGet Package Manager.</span></span>

<span data-ttu-id="8855d-132">NuGet Paket Yöneticisi'ni yüklemek için:</span><span class="sxs-lookup"><span data-stu-id="8855d-132">To install NuGet Package Manager:</span></span>

1. <span data-ttu-id="8855d-133">Visual Studio'yu başlatın.</span><span class="sxs-lookup"><span data-stu-id="8855d-133">Start Visual Studio.</span></span>
2. <span data-ttu-id="8855d-134">Gelen **Araçları** menüsünde, select **Uzantılar ve güncelleştirmeler**.</span><span class="sxs-lookup"><span data-stu-id="8855d-134">From the **Tools** menu, select **Extensions and Updates**.</span></span>
3. <span data-ttu-id="8855d-135">İçinde **Uzantılar ve güncelleştirmeler** iletişim kutusunda **çevrimiçi**.</span><span class="sxs-lookup"><span data-stu-id="8855d-135">In the **Extensions and Updates** dialog, select **Online**.</span></span>
4. <span data-ttu-id="8855d-136">"NuGet Paket Yöneticisi" göremiyorsanız, arama kutusuna "nuget Paket Yöneticisi" yazın.</span><span class="sxs-lookup"><span data-stu-id="8855d-136">If you don't see "NuGet Package Manager", type "nuget package manager" in the search box.</span></span>
5. <span data-ttu-id="8855d-137">NuGet Paket Yöneticisi'ni seçin ve tıklatın **karşıdan**.</span><span class="sxs-lookup"><span data-stu-id="8855d-137">Select the NuGet Package Manager and click **Download**.</span></span>
6. <span data-ttu-id="8855d-138">İndirme tamamlandıktan sonra yüklemeniz istenir.</span><span class="sxs-lookup"><span data-stu-id="8855d-138">After the download completes, you will be prompted to install.</span></span>
7. <span data-ttu-id="8855d-139">Yükleme tamamlandıktan sonra Visual Studio yeniden başlatmanız istenebilir.</span><span class="sxs-lookup"><span data-stu-id="8855d-139">After the installation completes, you might be prompted to restart Visual Studio.</span></span>

![](self-host-a-web-api/_static/image3.png)

## <a name="add-the-web-api-nuget-package"></a><span data-ttu-id="8855d-140">Web API NuGet paketi ekleme</span><span class="sxs-lookup"><span data-stu-id="8855d-140">Add the Web API NuGet Package</span></span>

<span data-ttu-id="8855d-141">NuGet Paket Yöneticisi yüklendikten sonra Web API Self-Host paketini projenize ekleyin.</span><span class="sxs-lookup"><span data-stu-id="8855d-141">After NuGet Package Manager is installed, add the Web API Self-Host package to your project.</span></span>

1. <span data-ttu-id="8855d-142">Gelen **Araçları** menüsünde, select **kitaplık Paket Yöneticisi**.</span><span class="sxs-lookup"><span data-stu-id="8855d-142">From the **Tools** menu, select **Library Package Manager**.</span></span> <span data-ttu-id="8855d-143">*Not*: Bu menü öğesi, bu NuGet Paket Yöneticisi'nin yüklendiğinden emin olun görmezseniz.</span><span class="sxs-lookup"><span data-stu-id="8855d-143">*Note*: If do you not see this menu item, make sure that NuGet Package Manager installed correctly.</span></span>
2. <span data-ttu-id="8855d-144">Seçin **çözüm için NuGet paketlerini Yönet...**</span><span class="sxs-lookup"><span data-stu-id="8855d-144">Select **Manage NuGet Packages for Solution...**</span></span>
3. <span data-ttu-id="8855d-145">İçinde **NugGet paketlerini Yönet** iletişim kutusunda **çevrimiçi**.</span><span class="sxs-lookup"><span data-stu-id="8855d-145">In the **Manage NugGet Packages** dialog, select **Online**.</span></span>
4. <span data-ttu-id="8855d-146">Arama kutusuna &quot;Microsoft.AspNet.WebApi.SelfHost&quot;.</span><span class="sxs-lookup"><span data-stu-id="8855d-146">In the search box, type &quot;Microsoft.AspNet.WebApi.SelfHost&quot;.</span></span>
5. <span data-ttu-id="8855d-147">ASP.NET Web API Self Host paketi seçin ve **yükleme**.</span><span class="sxs-lookup"><span data-stu-id="8855d-147">Select the ASP.NET Web API Self Host package and click **Install**.</span></span>
6. <span data-ttu-id="8855d-148">Paket yüklendikten sonra tıklatın **kapatmak** iletişim kutusunu kapatmak için.</span><span class="sxs-lookup"><span data-stu-id="8855d-148">After the package installs, click **Close** to close the dialog.</span></span>

> [!NOTE]
> <span data-ttu-id="8855d-149">Microsoft.AspNet.WebApi.SelfHost, değil AspNetWebApi.SelfHost adlı paketini yüklediğinizden emin olun.</span><span class="sxs-lookup"><span data-stu-id="8855d-149">Make sure to install the package named Microsoft.AspNet.WebApi.SelfHost, not AspNetWebApi.SelfHost.</span></span>


![](self-host-a-web-api/_static/image4.png)

## <a name="create-the-model-and-controller"></a><span data-ttu-id="8855d-150">Model ve denetleyici oluşturma</span><span class="sxs-lookup"><span data-stu-id="8855d-150">Create the Model and Controller</span></span>

<span data-ttu-id="8855d-151">Bu öğretici olarak aynı model ve denetleyici sınıfları kullanır [Başlarken](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) Öğreticisi.</span><span class="sxs-lookup"><span data-stu-id="8855d-151">This tutorial uses the same model and controller classes as the [Getting Started](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) tutorial.</span></span>

<span data-ttu-id="8855d-152">Adlı genel bir sınıf ekleyin `Product`.</span><span class="sxs-lookup"><span data-stu-id="8855d-152">Add a public class named `Product`.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample1.cs)]

<span data-ttu-id="8855d-153">Adlı genel bir sınıf ekleyin `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="8855d-153">Add a public class named `ProductsController`.</span></span> <span data-ttu-id="8855d-154">Bu sınıftan türeyen **System.Web.Http.ApiController**.</span><span class="sxs-lookup"><span data-stu-id="8855d-154">Derive this class from **System.Web.Http.ApiController**.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample2.cs)]

<span data-ttu-id="8855d-155">Bu denetleyici kodda hakkında daha fazla bilgi için bkz: [Başlarken](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) Öğreticisi.</span><span class="sxs-lookup"><span data-stu-id="8855d-155">For more information about the code in this controller, see the [Getting Started](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) tutorial.</span></span> <span data-ttu-id="8855d-156">Bu denetleyici üç GET eylemleri tanımlar:</span><span class="sxs-lookup"><span data-stu-id="8855d-156">This controller defines three GET actions:</span></span>

| <span data-ttu-id="8855d-157">URI</span><span class="sxs-lookup"><span data-stu-id="8855d-157">URI</span></span> | <span data-ttu-id="8855d-158">Açıklama</span><span class="sxs-lookup"><span data-stu-id="8855d-158">Description</span></span> |
| --- | --- |
| <span data-ttu-id="8855d-159">/ api/ürünleri</span><span class="sxs-lookup"><span data-stu-id="8855d-159">/api/products</span></span> | <span data-ttu-id="8855d-160">Tüm ürünlerin listesini alın.</span><span class="sxs-lookup"><span data-stu-id="8855d-160">Get a list of all products.</span></span> |
| <span data-ttu-id="8855d-161">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="8855d-161">/api/products/*id*</span></span> | <span data-ttu-id="8855d-162">Ürün Kimliği tarafından Al</span><span class="sxs-lookup"><span data-stu-id="8855d-162">Get a product by ID.</span></span> |
| <span data-ttu-id="8855d-163">/api/products/?category=*category*</span><span class="sxs-lookup"><span data-stu-id="8855d-163">/api/products/?category=*category*</span></span> | <span data-ttu-id="8855d-164">Kategoriye göre ürünlerin listesini alın.</span><span class="sxs-lookup"><span data-stu-id="8855d-164">Get a list of products by category.</span></span> |

## <a name="host-the-web-api"></a><span data-ttu-id="8855d-165">Web API ana bilgisayar</span><span class="sxs-lookup"><span data-stu-id="8855d-165">Host the Web API</span></span>

<span data-ttu-id="8855d-166">Program.cs dosyasını açın ve aşağıdaki using deyimlerini:</span><span class="sxs-lookup"><span data-stu-id="8855d-166">Open the file Program.cs and add the following using statements:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample3.cs)]

<span data-ttu-id="8855d-167">Aşağıdaki kodu ekleyin **Program** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="8855d-167">Add the following code to the **Program** class.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample4.cs)]

## <a name="optional-add-an-http-url-namespace-reservation"></a><span data-ttu-id="8855d-168">(İsteğe bağlı) Bir HTTP URL'si Namespace ayırması ekleme</span><span class="sxs-lookup"><span data-stu-id="8855d-168">(Optional) Add an HTTP URL Namespace Reservation</span></span>

<span data-ttu-id="8855d-169">Bu uygulama dinler `http://localhost:8080/`.</span><span class="sxs-lookup"><span data-stu-id="8855d-169">This application listens to `http://localhost:8080/`.</span></span> <span data-ttu-id="8855d-170">Varsayılan olarak, belirli bir HTTP adreste dinleme yönetici ayrıcalıkları gerekiyor.</span><span class="sxs-lookup"><span data-stu-id="8855d-170">By default, listening at a particular HTTP address requires administrator privileges.</span></span> <span data-ttu-id="8855d-171">Öğretici çalıştırdığınızda, bu nedenle, bu hatayı alabilirsiniz: "HTTP URL http://+:8080/ kaydedemedi" Bu hatayı önlemek için iki yolu vardır:</span><span class="sxs-lookup"><span data-stu-id="8855d-171">When you run the tutorial, therefore, you may get this error: "HTTP could not register URL http://+:8080/" There are two ways to avoid this error:</span></span>

- <span data-ttu-id="8855d-172">Visual Studio yükseltilmiş yönetici izinleriyle çalışmasını veya</span><span class="sxs-lookup"><span data-stu-id="8855d-172">Run Visual Studio with elevated administrator permissions, or</span></span>
- <span data-ttu-id="8855d-173">URL ayırmak için hesap izinlerinize vermek için Netsh.exe kullanın.</span><span class="sxs-lookup"><span data-stu-id="8855d-173">Use Netsh.exe to give your account permissions to reserve the URL.</span></span>

<span data-ttu-id="8855d-174">Netsh.exe kullanmak için yönetici ayrıcalıklarıyla bir komut istemi açın ve aşağıdaki komutu: aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="8855d-174">To use Netsh.exe, open a command prompt with administrator privileges and enter the following command:following command:</span></span>

[!code-console[Main](self-host-a-web-api/samples/sample5.cmd)]

<span data-ttu-id="8855d-175">Burada *Makine\kullanıcı adı* kullanıcı hesabıdır.</span><span class="sxs-lookup"><span data-stu-id="8855d-175">where *machine\username* is your user account.</span></span>

<span data-ttu-id="8855d-176">Kendi kendine barındırma tamamladığınızda, ayırmayı silmek emin olun:</span><span class="sxs-lookup"><span data-stu-id="8855d-176">When you are finished self-hosting, be sure to delete the reservation:</span></span>

[!code-console[Main](self-host-a-web-api/samples/sample6.cmd)]

## <a name="call-the-web-api-from-a-client-application-c"></a><span data-ttu-id="8855d-177">Web API çağrısı bir istemci uygulamadan (C#)</span><span class="sxs-lookup"><span data-stu-id="8855d-177">Call the Web API from a Client Application (C#)</span></span>

<span data-ttu-id="8855d-178">Web API'si çağıran basit bir konsol uygulaması yazalım.</span><span class="sxs-lookup"><span data-stu-id="8855d-178">Let's write a simple console application that calls the web API.</span></span>

<span data-ttu-id="8855d-179">Yeni bir konsol uygulama projesi ekleyin:</span><span class="sxs-lookup"><span data-stu-id="8855d-179">Add a new console application project to the solution:</span></span>

- <span data-ttu-id="8855d-180">Çözüm Gezgini'nde çözüme sağ tıklayın ve seçin **Yeni Proje Ekle**.</span><span class="sxs-lookup"><span data-stu-id="8855d-180">In Solution Explorer, right-click the solution and select **Add New Project**.</span></span>
- <span data-ttu-id="8855d-181">Adlı yeni bir konsol uygulaması oluşturun &quot;ClientApp&quot;.</span><span class="sxs-lookup"><span data-stu-id="8855d-181">Create a new console application named &quot;ClientApp&quot;.</span></span>

![](self-host-a-web-api/_static/image5.png)

<span data-ttu-id="8855d-182">NuGet paket ASP.NET Web API Core Libraries paketini eklemek için Yöneticisi'ni kullanın:</span><span class="sxs-lookup"><span data-stu-id="8855d-182">Use NuGet Package Manager to add the ASP.NET Web API Core Libraries package:</span></span>

- <span data-ttu-id="8855d-183">Araçlar menüsünden seçin **kitaplık Paket Yöneticisi**.</span><span class="sxs-lookup"><span data-stu-id="8855d-183">From the Tools menu, select **Library Package Manager**.</span></span>
- <span data-ttu-id="8855d-184">Seçin **çözüm için NuGet paketlerini Yönet...**</span><span class="sxs-lookup"><span data-stu-id="8855d-184">Select **Manage NuGet Packages for Solution...**</span></span>
- <span data-ttu-id="8855d-185">İçinde **NuGet paketlerini Yönet** iletişim kutusunda **çevrimiçi**.</span><span class="sxs-lookup"><span data-stu-id="8855d-185">In the **Manage NuGet Packages** dialog, select **Online**.</span></span>
- <span data-ttu-id="8855d-186">Arama kutusuna &quot;Microsoft.AspNet.WebApi.Client&quot;.</span><span class="sxs-lookup"><span data-stu-id="8855d-186">In the search box, type &quot;Microsoft.AspNet.WebApi.Client&quot;.</span></span>
- <span data-ttu-id="8855d-187">Microsoft ASP.NET Web API Client Libraries paketi seçin ve **yükleme**.</span><span class="sxs-lookup"><span data-stu-id="8855d-187">Select the Microsoft ASP.NET Web API Client Libraries package and click **Install**.</span></span>

<span data-ttu-id="8855d-188">Bir başvuru ClientApp SelfHost projeye ekleyin:</span><span class="sxs-lookup"><span data-stu-id="8855d-188">Add a reference in ClientApp to the SelfHost project:</span></span>

- <span data-ttu-id="8855d-189">Çözüm Gezgini'nde ClientApp projesine sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="8855d-189">In Solution Explorer, right-click the ClientApp project.</span></span>
- <span data-ttu-id="8855d-190">Seçin **başvuru ekleme**.</span><span class="sxs-lookup"><span data-stu-id="8855d-190">Select **Add Reference**.</span></span>
- <span data-ttu-id="8855d-191">İçinde **başvuru Yöneticisi** iletişim altında **çözüm**seçin **projeleri**.</span><span class="sxs-lookup"><span data-stu-id="8855d-191">In the **Reference Manager** dialog, under **Solution**, select **Projects**.</span></span>
- <span data-ttu-id="8855d-192">SelfHost projesini seçin.</span><span class="sxs-lookup"><span data-stu-id="8855d-192">Select the SelfHost project.</span></span>
- <span data-ttu-id="8855d-193">**Tamam**'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="8855d-193">Click **OK**.</span></span>

![](self-host-a-web-api/_static/image6.png)

<span data-ttu-id="8855d-194">Client/Program.cs dosyasını açın.</span><span class="sxs-lookup"><span data-stu-id="8855d-194">Open the Client/Program.cs file.</span></span> <span data-ttu-id="8855d-195">Aşağıdakileri ekleyin **kullanarak** deyimi:</span><span class="sxs-lookup"><span data-stu-id="8855d-195">Add the following **using** statement:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample7.cs)]

<span data-ttu-id="8855d-196">Statik eklemek **HttpClient** örneği:</span><span class="sxs-lookup"><span data-stu-id="8855d-196">Add a static **HttpClient** instance:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample8.cs)]

<span data-ttu-id="8855d-197">Tüm ürünler, liste Kimliğine göre bir ürün ve liste ürünler kategoriye göre listelemek için aşağıdaki yöntemleri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="8855d-197">Add the following methods to list all products, list a product by ID, and list products by category.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample9.cs)]

<span data-ttu-id="8855d-198">Bu yöntemlerin her biri aynı düzeni izler:</span><span class="sxs-lookup"><span data-stu-id="8855d-198">Each of these methods follows the same pattern:</span></span>

1. <span data-ttu-id="8855d-199">Çağrı **HttpClient.GetAsync** uygun URI GET isteğini göndermek için.</span><span class="sxs-lookup"><span data-stu-id="8855d-199">Call **HttpClient.GetAsync** to send a GET request to the appropriate URI.</span></span>
2. <span data-ttu-id="8855d-200">Çağrı **HttpResponseMessage.EnsureSuccessStatusCode**.</span><span class="sxs-lookup"><span data-stu-id="8855d-200">Call **HttpResponseMessage.EnsureSuccessStatusCode**.</span></span> <span data-ttu-id="8855d-201">HTTP yanıt durumu hata kodu ise, bu yöntem bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="8855d-201">This method throws an exception if the HTTP response status is an error code.</span></span>
3. <span data-ttu-id="8855d-202">Çağrı **ReadAsAsync&lt;T&gt;**  HTTP yanıtı bir CLR türü seri durumdan çıkarılamadı.</span><span class="sxs-lookup"><span data-stu-id="8855d-202">Call **ReadAsAsync&lt;T&gt;** to deserialize a CLR type from the HTTP response.</span></span> <span data-ttu-id="8855d-203">Tanımlanan bir genişletme yöntemi, bu yöntemdir **System.Net.Http.HttpContentExtensions**.</span><span class="sxs-lookup"><span data-stu-id="8855d-203">This method is an extension method, defined in **System.Net.Http.HttpContentExtensions**.</span></span>

<span data-ttu-id="8855d-204">**GetAsync** ve **ReadAsAsync** yöntemleridir her ikisi de zaman uyumsuz.</span><span class="sxs-lookup"><span data-stu-id="8855d-204">The **GetAsync** and **ReadAsAsync** methods are both asynchronous.</span></span> <span data-ttu-id="8855d-205">Döndürmeleri **görev** zaman uyumsuz işlemi temsil eden nesne.</span><span class="sxs-lookup"><span data-stu-id="8855d-205">They return **Task** objects that represent the asynchronous operation.</span></span> <span data-ttu-id="8855d-206">Alma **sonuç** özelliği işlemi tamamlanana kadar iş parçacığı engeller.</span><span class="sxs-lookup"><span data-stu-id="8855d-206">Getting the **Result** property blocks the thread until the operation completes.</span></span>

<span data-ttu-id="8855d-207">Engelleyici olmayan aramalarda de dahil olmak üzere HttpClient kullanma hakkında daha fazla bilgi için bkz: [bir Web API öğesinden bir .NET istemci çağırma](../advanced/calling-a-web-api-from-a-net-client.md).</span><span class="sxs-lookup"><span data-stu-id="8855d-207">For more information about using HttpClient, including how to make non-blocking calls, see [Calling a Web API From a .NET Client](../advanced/calling-a-web-api-from-a-net-client.md).</span></span>

<span data-ttu-id="8855d-208">Bu yöntemleri çağrılmadan önce BaseAddress özelliğini HttpClient örneğine ayarlayın "`http://localhost:8080`".</span><span class="sxs-lookup"><span data-stu-id="8855d-208">Before calling these methods, set the BaseAddress property on the HttpClient instance to "`http://localhost:8080`".</span></span> <span data-ttu-id="8855d-209">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="8855d-209">For example:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample10.cs)]

<span data-ttu-id="8855d-210">Bu aşağıdaki çıktı.</span><span class="sxs-lookup"><span data-stu-id="8855d-210">This should output the following.</span></span> <span data-ttu-id="8855d-211">(İlk SelfHost uygulamayı çalıştırmak unutmayın.)</span><span class="sxs-lookup"><span data-stu-id="8855d-211">(Remember to run the SelfHost application first.)</span></span>

[!code-console[Main](self-host-a-web-api/samples/sample11.cmd)]

![](self-host-a-web-api/_static/image7.png)
