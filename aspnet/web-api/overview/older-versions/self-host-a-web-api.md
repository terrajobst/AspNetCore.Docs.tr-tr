---
uid: web-api/overview/older-versions/self-host-a-web-api
title: ASP.NET Web API 1 (C#) barındırma | Microsoft Docs
author: MikeWasson
description: ASP.NET Web API, IIS gerektirmez. Web API'si kendi ana bilgisayar işlemi kendi kendine barındırabilirsiniz. Bu öğreticide, bir konsolu applic içinde bir web API'sini barındıran gösterilmektedir...
ms.author: riande
ms.date: 01/26/2012
ms.assetid: be5ab1e2-4140-4275-ac59-ca82a1bac0c1
msc.legacyurl: /web-api/overview/older-versions/self-host-a-web-api
msc.type: authoredcontent
ms.openlocfilehash: 63d192a6fa2aafef3770d5b0b97ec32e001b69db
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912708"
---
<a name="self-host-aspnet-web-api-1-c"></a><span data-ttu-id="2c900-105">Barındırılan ASP.NET Web API 1 (C#)</span><span class="sxs-lookup"><span data-stu-id="2c900-105">Self-Host ASP.NET Web API 1 (C#)</span></span>
====================
<span data-ttu-id="2c900-106">tarafından [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="2c900-106">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="2c900-107">ASP.NET Web API, IIS gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="2c900-107">ASP.NET Web API does not require IIS.</span></span> <span data-ttu-id="2c900-108">Web API'si kendi ana bilgisayar işlemi kendi kendine barındırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2c900-108">You can self-host a web API in your own host process.</span></span> <span data-ttu-id="2c900-109">Bu öğreticide, bir konsol uygulaması içinde bir web API'sini barındıran gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="2c900-109">This tutorial shows how to host a web API inside a console application.</span></span>
> 
> <span data-ttu-id="2c900-110">**Yeni uygulama, barındırılan Web API'si için OWIN kullanmanız gerekir.**</span><span class="sxs-lookup"><span data-stu-id="2c900-110">**New applications should use OWIN to self-host Web API.**</span></span> <span data-ttu-id="2c900-111">Bkz: [OWIN, ASP.NET Web API 2 barındırma kullanmasını](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="2c900-111">See [Use OWIN to Self-Host ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="2c900-112">Bu öğreticide kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="2c900-112">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="2c900-113">Web API 1</span><span class="sxs-lookup"><span data-stu-id="2c900-113">Web API 1</span></span>
> - <span data-ttu-id="2c900-114">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="2c900-114">Visual Studio 2012</span></span>


## <a name="create-the-console-application-project"></a><span data-ttu-id="2c900-115">Konsol uygulama projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="2c900-115">Create the Console Application Project</span></span>

<span data-ttu-id="2c900-116">Visual Studio'yu başlatın ve seçin **yeni proje** gelen **Başlat** sayfası.</span><span class="sxs-lookup"><span data-stu-id="2c900-116">Start Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="2c900-117">Veya **dosya** menüsünde **yeni** ardından **proje**.</span><span class="sxs-lookup"><span data-stu-id="2c900-117">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="2c900-118">İçinde **şablonları** bölmesinde **yüklü şablonlar** genişletin **Visual C#** düğümü.</span><span class="sxs-lookup"><span data-stu-id="2c900-118">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="2c900-119">Altında **Visual C#** seçin **Windows**.</span><span class="sxs-lookup"><span data-stu-id="2c900-119">Under **Visual C#**, select **Windows**.</span></span> <span data-ttu-id="2c900-120">Proje şablonları listesinde seçin **konsol uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="2c900-120">In the list of project templates, select **Console Application**.</span></span> <span data-ttu-id="2c900-121">Projeyi adlandırın &quot;SelfHost&quot; tıklatıp **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="2c900-121">Name the project &quot;SelfHost&quot; and click **OK**.</span></span>

![](self-host-a-web-api/_static/image1.png)

## <a name="set-the-target-framework-visual-studio-2010"></a><span data-ttu-id="2c900-122">Hedef Framework'ü (Visual Studio 2010) ayarlama</span><span class="sxs-lookup"><span data-stu-id="2c900-122">Set the Target Framework (Visual Studio 2010)</span></span>

<span data-ttu-id="2c900-123">Visual Studio 2010 kullanıyorsanız, hedef çerçeveyi .NET Framework 4.0 ile değiştirin.</span><span class="sxs-lookup"><span data-stu-id="2c900-123">If you are using Visual Studio 2010, change the target framework to .NET Framework 4.0.</span></span> <span data-ttu-id="2c900-124">(Varsayılan olarak, proje şablonunun hedeflediği [.Net Framework istemci profili](https://msdn.microsoft.com/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile).)</span><span class="sxs-lookup"><span data-stu-id="2c900-124">(By default, the project template targets the [.Net Framework Client Profile](https://msdn.microsoft.com/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile).)</span></span>

<span data-ttu-id="2c900-125">Çözüm Gezgini'nde projeye sağ tıklayıp seçin **özellikleri**.</span><span class="sxs-lookup"><span data-stu-id="2c900-125">In Solution Explorer, right-click the project and select **Properties**.</span></span> <span data-ttu-id="2c900-126">İçinde **hedef Framework'ü** açılan listesinde, .NET Framework 4.0 için hedef Framework'ü değiştirin.</span><span class="sxs-lookup"><span data-stu-id="2c900-126">In the **Target framework** dropdown list, change the target framework to .NET Framework 4.0.</span></span> <span data-ttu-id="2c900-127">Değişikliği uygulamak için sorulduğunda **Evet**.</span><span class="sxs-lookup"><span data-stu-id="2c900-127">When prompted to apply the change, click **Yes**.</span></span>

![](self-host-a-web-api/_static/image2.png)

## <a name="install-nuget-package-manager"></a><span data-ttu-id="2c900-128">NuGet Paket Yöneticisi'ni yükleyin</span><span class="sxs-lookup"><span data-stu-id="2c900-128">Install NuGet Package Manager</span></span>

<span data-ttu-id="2c900-129">NuGet Paket Yöneticisi, bir ASP.NET olmayan projeye Web API derlemeleri eklemek için en kolay yoludur.</span><span class="sxs-lookup"><span data-stu-id="2c900-129">The NuGet Package Manager is the easiest way to add the Web API assemblies to a non-ASP.NET project.</span></span>

<span data-ttu-id="2c900-130">NuGet Paket Yöneticisi'nin yüklü olup olmadığını denetlemek için **Araçları** Visual Studio'daki menü.</span><span class="sxs-lookup"><span data-stu-id="2c900-130">To check if NuGet Package Manager is installed, click the **Tools** menu in Visual Studio.</span></span> <span data-ttu-id="2c900-131">Bir menü öğesi adlı görürseniz **NuGet Paket Yöneticisi**, NuGet Paket Yöneticisi sahip.</span><span class="sxs-lookup"><span data-stu-id="2c900-131">If you see a menu item called **NuGet Package Manager**, then you have NuGet Package Manager.</span></span>

<span data-ttu-id="2c900-132">NuGet Paket Yöneticisi'ni yüklemek için:</span><span class="sxs-lookup"><span data-stu-id="2c900-132">To install NuGet Package Manager:</span></span>

1. <span data-ttu-id="2c900-133">Visual Studio'yu başlatın.</span><span class="sxs-lookup"><span data-stu-id="2c900-133">Start Visual Studio.</span></span>
2. <span data-ttu-id="2c900-134">Gelen **Araçları** menüsünde **Uzantılar ve güncelleştirmeler**.</span><span class="sxs-lookup"><span data-stu-id="2c900-134">From the **Tools** menu, select **Extensions and Updates**.</span></span>
3. <span data-ttu-id="2c900-135">İçinde **Uzantılar ve güncelleştirmeler** iletişim kutusunda **çevrimiçi**.</span><span class="sxs-lookup"><span data-stu-id="2c900-135">In the **Extensions and Updates** dialog, select **Online**.</span></span>
4. <span data-ttu-id="2c900-136">"Nuget Paket Yöneticisi", "NuGet Paket Yöneticisi" ifadesini görmüyorsanız arama kutusuna yazın.</span><span class="sxs-lookup"><span data-stu-id="2c900-136">If you don't see "NuGet Package Manager", type "nuget package manager" in the search box.</span></span>
5. <span data-ttu-id="2c900-137">NuGet Paket Yöneticisi'ni seçip tıklayın **indirme**.</span><span class="sxs-lookup"><span data-stu-id="2c900-137">Select the NuGet Package Manager and click **Download**.</span></span>
6. <span data-ttu-id="2c900-138">İndirme tamamlandıktan sonra yüklemeniz istenir.</span><span class="sxs-lookup"><span data-stu-id="2c900-138">After the download completes, you will be prompted to install.</span></span>
7. <span data-ttu-id="2c900-139">Yükleme tamamlandıktan sonra Visual Studio'yu yeniden başlatmanız istenebilir.</span><span class="sxs-lookup"><span data-stu-id="2c900-139">After the installation completes, you might be prompted to restart Visual Studio.</span></span>

![](self-host-a-web-api/_static/image3.png)

## <a name="add-the-web-api-nuget-package"></a><span data-ttu-id="2c900-140">Web API NuGet paketi ekleme</span><span class="sxs-lookup"><span data-stu-id="2c900-140">Add the Web API NuGet Package</span></span>

<span data-ttu-id="2c900-141">NuGet Paket Yöneticisi'ni yükledikten sonra Web API Self-Host paketini projenize ekleyin.</span><span class="sxs-lookup"><span data-stu-id="2c900-141">After NuGet Package Manager is installed, add the Web API Self-Host package to your project.</span></span>

1. <span data-ttu-id="2c900-142">Gelen **Araçları** menüsünde **NuGet Paket Yöneticisi**.</span><span class="sxs-lookup"><span data-stu-id="2c900-142">From the **Tools** menu, select **NuGet Package Manager**.</span></span> <span data-ttu-id="2c900-143">*Not*: Bu menü öğesi, bu NuGet Paket Yöneticisi'nin yüklendiğinden emin olun görmüyorsanız.</span><span class="sxs-lookup"><span data-stu-id="2c900-143">*Note*: If do you not see this menu item, make sure that NuGet Package Manager installed correctly.</span></span>
2. <span data-ttu-id="2c900-144">Seçin **çözüm için NuGet paketlerini Yönet**</span><span class="sxs-lookup"><span data-stu-id="2c900-144">Select **Manage NuGet Packages for Solution**</span></span>
3. <span data-ttu-id="2c900-145">İçinde **NugGet paketlerini Yönet** iletişim kutusunda **çevrimiçi**.</span><span class="sxs-lookup"><span data-stu-id="2c900-145">In the **Manage NugGet Packages** dialog, select **Online**.</span></span>
4. <span data-ttu-id="2c900-146">Arama kutusuna &quot;Microsoft.AspNet.WebApi.SelfHost&quot;.</span><span class="sxs-lookup"><span data-stu-id="2c900-146">In the search box, type &quot;Microsoft.AspNet.WebApi.SelfHost&quot;.</span></span>
5. <span data-ttu-id="2c900-147">ASP.NET Web API Self konağı paketi seçin ve tıklayın **yükleme**.</span><span class="sxs-lookup"><span data-stu-id="2c900-147">Select the ASP.NET Web API Self Host package and click **Install**.</span></span>
6. <span data-ttu-id="2c900-148">Paket yüklendikten sonra tıklayın **kapatmak** iletişim kutusunu kapatmak için.</span><span class="sxs-lookup"><span data-stu-id="2c900-148">After the package installs, click **Close** to close the dialog.</span></span>

> [!NOTE]
> <span data-ttu-id="2c900-149">Microsoft.AspNet.WebApi.SelfHost, değil AspNetWebApi.SelfHost adlı paketi yüklediğinizden emin olun.</span><span class="sxs-lookup"><span data-stu-id="2c900-149">Make sure to install the package named Microsoft.AspNet.WebApi.SelfHost, not AspNetWebApi.SelfHost.</span></span>

![](self-host-a-web-api/_static/image4.png)

## <a name="create-the-model-and-controller"></a><span data-ttu-id="2c900-150">Model ve denetleyici oluşturma</span><span class="sxs-lookup"><span data-stu-id="2c900-150">Create the Model and Controller</span></span>

<span data-ttu-id="2c900-151">Bu öğreticide aynı model ve denetleyici sınıflar olarak [Başlarken](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) öğretici.</span><span class="sxs-lookup"><span data-stu-id="2c900-151">This tutorial uses the same model and controller classes as the [Getting Started](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) tutorial.</span></span>

<span data-ttu-id="2c900-152">Adlı bir ortak Sınıf Ekle `Product`.</span><span class="sxs-lookup"><span data-stu-id="2c900-152">Add a public class named `Product`.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample1.cs)]

<span data-ttu-id="2c900-153">Adlı bir ortak Sınıf Ekle `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="2c900-153">Add a public class named `ProductsController`.</span></span> <span data-ttu-id="2c900-154">Bu sınıftan türetilen **System.Web.Http.ApiController**.</span><span class="sxs-lookup"><span data-stu-id="2c900-154">Derive this class from **System.Web.Http.ApiController**.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample2.cs)]

<span data-ttu-id="2c900-155">Bu denetleyicideki kod hakkında daha fazla bilgi için bkz. [Başlarken](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) öğretici.</span><span class="sxs-lookup"><span data-stu-id="2c900-155">For more information about the code in this controller, see the [Getting Started](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) tutorial.</span></span> <span data-ttu-id="2c900-156">Bu denetleyici üç GET eylemleri tanımlar:</span><span class="sxs-lookup"><span data-stu-id="2c900-156">This controller defines three GET actions:</span></span>

| <span data-ttu-id="2c900-157">URI</span><span class="sxs-lookup"><span data-stu-id="2c900-157">URI</span></span> | <span data-ttu-id="2c900-158">Açıklama</span><span class="sxs-lookup"><span data-stu-id="2c900-158">Description</span></span> |
| --- | --- |
| <span data-ttu-id="2c900-159">/ api/ürünleri</span><span class="sxs-lookup"><span data-stu-id="2c900-159">/api/products</span></span> | <span data-ttu-id="2c900-160">Tüm ürünlerin listesini alın.</span><span class="sxs-lookup"><span data-stu-id="2c900-160">Get a list of all products.</span></span> |
| <span data-ttu-id="2c900-161">/API'si/ürünler/*kimliği*</span><span class="sxs-lookup"><span data-stu-id="2c900-161">/api/products/*id*</span></span> | <span data-ttu-id="2c900-162">Bir ürün kimliğine göre Al</span><span class="sxs-lookup"><span data-stu-id="2c900-162">Get a product by ID.</span></span> |
| <span data-ttu-id="2c900-163">/ API'si/ürünler /? kategori =*kategorisi*</span><span class="sxs-lookup"><span data-stu-id="2c900-163">/api/products/?category=*category*</span></span> | <span data-ttu-id="2c900-164">Kategoriye göre ürünlerin listesini alın.</span><span class="sxs-lookup"><span data-stu-id="2c900-164">Get a list of products by category.</span></span> |

## <a name="host-the-web-api"></a><span data-ttu-id="2c900-165">Web API'si barındırın</span><span class="sxs-lookup"><span data-stu-id="2c900-165">Host the Web API</span></span>

<span data-ttu-id="2c900-166">Program.cs dosyasını açın ve aşağıdaki using deyimlerini:</span><span class="sxs-lookup"><span data-stu-id="2c900-166">Open the file Program.cs and add the following using statements:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample3.cs)]

<span data-ttu-id="2c900-167">Aşağıdaki kodu ekleyin **Program** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="2c900-167">Add the following code to the **Program** class.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample4.cs)]

## <a name="optional-add-an-http-url-namespace-reservation"></a><span data-ttu-id="2c900-168">(İsteğe bağlı) Bir HTTP URL'si Namespace ayırma ekleyin</span><span class="sxs-lookup"><span data-stu-id="2c900-168">(Optional) Add an HTTP URL Namespace Reservation</span></span>

<span data-ttu-id="2c900-169">Bu uygulamasının dinleyeceği `http://localhost:8080/`.</span><span class="sxs-lookup"><span data-stu-id="2c900-169">This application listens to `http://localhost:8080/`.</span></span> <span data-ttu-id="2c900-170">Varsayılan olarak, belirli bir HTTP adreste dinleme yönetici ayrıcalıkları gerektirir.</span><span class="sxs-lookup"><span data-stu-id="2c900-170">By default, listening at a particular HTTP address requires administrator privileges.</span></span> <span data-ttu-id="2c900-171">Öğreticiyi çalıştırdığınızda, bu nedenle, bu hatayı alabilirsiniz: "HTTP URL'si kaydedemedi http://+:8080/" Bu hatayı önlemek için iki yolu vardır:</span><span class="sxs-lookup"><span data-stu-id="2c900-171">When you run the tutorial, therefore, you may get this error: "HTTP could not register URL http://+:8080/" There are two ways to avoid this error:</span></span>

- <span data-ttu-id="2c900-172">Visual Studio'yu yönetici izinleriyle çalıştırın veya</span><span class="sxs-lookup"><span data-stu-id="2c900-172">Run Visual Studio with elevated administrator permissions, or</span></span>
- <span data-ttu-id="2c900-173">Netsh.exe URL ayırmak için hesap izinlerinize vermek için kullanın.</span><span class="sxs-lookup"><span data-stu-id="2c900-173">Use Netsh.exe to give your account permissions to reserve the URL.</span></span>

<span data-ttu-id="2c900-174">Netsh.exe kullanmak için yönetici ayrıcalıklarıyla bir komut istemi açın ve aşağıdaki komutu: aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="2c900-174">To use Netsh.exe, open a command prompt with administrator privileges and enter the following command:following command:</span></span>

[!code-console[Main](self-host-a-web-api/samples/sample5.cmd)]

<span data-ttu-id="2c900-175">Burada *Makine\kullanıcı* kullanıcı hesabıdır.</span><span class="sxs-lookup"><span data-stu-id="2c900-175">where *machine\username* is your user account.</span></span>

<span data-ttu-id="2c900-176">İşiniz bittiğinde kendi kendine barındırma, rezervasyon sildiğinizden emin olun:</span><span class="sxs-lookup"><span data-stu-id="2c900-176">When you are finished self-hosting, be sure to delete the reservation:</span></span>

[!code-console[Main](self-host-a-web-api/samples/sample6.cmd)]

## <a name="call-the-web-api-from-a-client-application-c"></a><span data-ttu-id="2c900-177">Bir istemci uygulamasından (C#) Web API'si çağırma</span><span class="sxs-lookup"><span data-stu-id="2c900-177">Call the Web API from a Client Application (C#)</span></span>

<span data-ttu-id="2c900-178">Şimdi web API'si çağıran basit bir konsol uygulaması yazma.</span><span class="sxs-lookup"><span data-stu-id="2c900-178">Let's write a simple console application that calls the web API.</span></span>

<span data-ttu-id="2c900-179">Yeni bir konsol uygulaması projesi çözüme ekleyin:</span><span class="sxs-lookup"><span data-stu-id="2c900-179">Add a new console application project to the solution:</span></span>

- <span data-ttu-id="2c900-180">Çözüm Gezgini'nde çözüme sağ tıklayıp seçin **Yeni Proje Ekle**.</span><span class="sxs-lookup"><span data-stu-id="2c900-180">In Solution Explorer, right-click the solution and select **Add New Project**.</span></span>
- <span data-ttu-id="2c900-181">Adlı yeni bir konsol uygulaması oluşturun &quot;ClientApp&quot;.</span><span class="sxs-lookup"><span data-stu-id="2c900-181">Create a new console application named &quot;ClientApp&quot;.</span></span>

![](self-host-a-web-api/_static/image5.png)

<span data-ttu-id="2c900-182">NuGet paketi ASP.NET Web API çekirdek kitaplıkları paketini eklemek için Yöneticisi'ni kullanın:</span><span class="sxs-lookup"><span data-stu-id="2c900-182">Use NuGet Package Manager to add the ASP.NET Web API Core Libraries package:</span></span>

- <span data-ttu-id="2c900-183">Araçlar menüsü'nden seçin **NuGet Paket Yöneticisi**.</span><span class="sxs-lookup"><span data-stu-id="2c900-183">From the Tools menu, select **NuGet Package Manager**.</span></span>
- <span data-ttu-id="2c900-184">Seçin **çözüm için NuGet paketlerini Yönet**</span><span class="sxs-lookup"><span data-stu-id="2c900-184">Select **Manage NuGet Packages for Solution**</span></span>
- <span data-ttu-id="2c900-185">İçinde **NuGet paketlerini Yönet** iletişim kutusunda **çevrimiçi**.</span><span class="sxs-lookup"><span data-stu-id="2c900-185">In the **Manage NuGet Packages** dialog, select **Online**.</span></span>
- <span data-ttu-id="2c900-186">Arama kutusuna &quot;System.NET.http.Formatting&quot;.</span><span class="sxs-lookup"><span data-stu-id="2c900-186">In the search box, type &quot;Microsoft.AspNet.WebApi.Client&quot;.</span></span>
- <span data-ttu-id="2c900-187">Microsoft ASP.NET Web API İstemci Kitaplığı paketi seçin ve tıklayın **yükleme**.</span><span class="sxs-lookup"><span data-stu-id="2c900-187">Select the Microsoft ASP.NET Web API Client Libraries package and click **Install**.</span></span>

<span data-ttu-id="2c900-188">Bir başvuru ClientApp SelfHost projeye ekleyin:</span><span class="sxs-lookup"><span data-stu-id="2c900-188">Add a reference in ClientApp to the SelfHost project:</span></span>

- <span data-ttu-id="2c900-189">Çözüm Gezgini'nde ClientApp projeye sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="2c900-189">In Solution Explorer, right-click the ClientApp project.</span></span>
- <span data-ttu-id="2c900-190">Seçin **Başvurusu Ekle**.</span><span class="sxs-lookup"><span data-stu-id="2c900-190">Select **Add Reference**.</span></span>
- <span data-ttu-id="2c900-191">İçinde **başvuru Yöneticisi** iletişim altında **çözüm**seçin **projeleri**.</span><span class="sxs-lookup"><span data-stu-id="2c900-191">In the **Reference Manager** dialog, under **Solution**, select **Projects**.</span></span>
- <span data-ttu-id="2c900-192">SelfHost projeyi seçin.</span><span class="sxs-lookup"><span data-stu-id="2c900-192">Select the SelfHost project.</span></span>
- <span data-ttu-id="2c900-193">**Tamam**'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="2c900-193">Click **OK**.</span></span>

![](self-host-a-web-api/_static/image6.png)

<span data-ttu-id="2c900-194">Client/Program.cs dosyasını açın.</span><span class="sxs-lookup"><span data-stu-id="2c900-194">Open the Client/Program.cs file.</span></span> <span data-ttu-id="2c900-195">Aşağıdaki **kullanarak** deyimi:</span><span class="sxs-lookup"><span data-stu-id="2c900-195">Add the following **using** statement:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample7.cs)]

<span data-ttu-id="2c900-196">Statik bir ekleme **HttpClient** örneği:</span><span class="sxs-lookup"><span data-stu-id="2c900-196">Add a static **HttpClient** instance:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample8.cs)]

<span data-ttu-id="2c900-197">Tüm ürünler, bir ürün Kimliğine göre listesi ve ürünleri listeler, kategoriye göre listelemek için aşağıdaki yöntemleri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="2c900-197">Add the following methods to list all products, list a product by ID, and list products by category.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample9.cs)]

<span data-ttu-id="2c900-198">Bu yöntemlerin her biri aynı deseni izler:</span><span class="sxs-lookup"><span data-stu-id="2c900-198">Each of these methods follows the same pattern:</span></span>

1. <span data-ttu-id="2c900-199">Çağrı **HttpClient.GetAsync** uygun URI'ye bir GET isteği göndermek için.</span><span class="sxs-lookup"><span data-stu-id="2c900-199">Call **HttpClient.GetAsync** to send a GET request to the appropriate URI.</span></span>
2. <span data-ttu-id="2c900-200">Çağrı **HttpResponseMessage.EnsureSuccessStatusCode**.</span><span class="sxs-lookup"><span data-stu-id="2c900-200">Call **HttpResponseMessage.EnsureSuccessStatusCode**.</span></span> <span data-ttu-id="2c900-201">HTTP yanıtı durum bir hata kodu ise bu yöntem bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="2c900-201">This method throws an exception if the HTTP response status is an error code.</span></span>
3. <span data-ttu-id="2c900-202">Çağrı **ReadAsAsync&lt;T&gt;**  HTTP yanıtı gelen bir CLR türü seri durumdan çıkarılacak.</span><span class="sxs-lookup"><span data-stu-id="2c900-202">Call **ReadAsAsync&lt;T&gt;** to deserialize a CLR type from the HTTP response.</span></span> <span data-ttu-id="2c900-203">Bu yöntem içinde tanımlanan bir genişletme yöntemi olduğunu **System.Net.Http.HttpContentExtensions**.</span><span class="sxs-lookup"><span data-stu-id="2c900-203">This method is an extension method, defined in **System.Net.Http.HttpContentExtensions**.</span></span>

<span data-ttu-id="2c900-204">**GetAsync** ve **ReadAsAsync** hem de zaman uyumsuz yöntemler şunlardır.</span><span class="sxs-lookup"><span data-stu-id="2c900-204">The **GetAsync** and **ReadAsAsync** methods are both asynchronous.</span></span> <span data-ttu-id="2c900-205">Döndürmeleri **görev** zaman uyumsuz işlemi temsil eden nesneleri.</span><span class="sxs-lookup"><span data-stu-id="2c900-205">They return **Task** objects that represent the asynchronous operation.</span></span> <span data-ttu-id="2c900-206">Başlama **sonucu** özelliği işlem tamamlanana kadar iş parçacığını engeller.</span><span class="sxs-lookup"><span data-stu-id="2c900-206">Getting the **Result** property blocks the thread until the operation completes.</span></span>

<span data-ttu-id="2c900-207">HttpClient, engellemeyen çağrı yapmak nasıl dahil olmak üzere kullanma hakkında daha fazla bilgi için bkz. [bir Web API'si bir .NET istemcinin çağırma](../advanced/calling-a-web-api-from-a-net-client.md).</span><span class="sxs-lookup"><span data-stu-id="2c900-207">For more information about using HttpClient, including how to make non-blocking calls, see [Calling a Web API From a .NET Client](../advanced/calling-a-web-api-from-a-net-client.md).</span></span>

<span data-ttu-id="2c900-208">Bu yöntemleri çağrılmadan önce BaseAddress özelliği için HttpClient örneği üzerinde ayarlanmış "`http://localhost:8080`".</span><span class="sxs-lookup"><span data-stu-id="2c900-208">Before calling these methods, set the BaseAddress property on the HttpClient instance to "`http://localhost:8080`".</span></span> <span data-ttu-id="2c900-209">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="2c900-209">For example:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample10.cs)]

<span data-ttu-id="2c900-210">Bu aşağıdaki çıkışı oluşturmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2c900-210">This should output the following.</span></span> <span data-ttu-id="2c900-211">(İlk SelfHost uygulamayı çalıştırmak unutmayın.)</span><span class="sxs-lookup"><span data-stu-id="2c900-211">(Remember to run the SelfHost application first.)</span></span>

[!code-console[Main](self-host-a-web-api/samples/sample11.cmd)]

![](self-host-a-web-api/_static/image7.png)
