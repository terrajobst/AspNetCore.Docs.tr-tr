---
uid: signalr/overview/testing-and-debugging/unit-testing-signalr-applications
title: SignalR uygulamalarında birim testi | Microsoft Docs
author: pfletcher
description: Bu makalede, SignalR 2.0 birim testi özelliklerini kullanmayı açıklar.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: d1983524-e0d5-4ee6-9d87-1f552f7cb964
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/testing-and-debugging/unit-testing-signalr-applications
msc.type: authoredcontent
ms.openlocfilehash: cff866716cb1179e02b930f33cb0f8c33d4a6cf0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870849"
---
<a name="unit-testing-signalr-applications"></a><span data-ttu-id="fda71-103">Birim testi SignalR uygulamalarını</span><span class="sxs-lookup"><span data-stu-id="fda71-103">Unit Testing SignalR Applications</span></span>
====================
<span data-ttu-id="fda71-104">tarafından [CAN Fletcher'dan](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="fda71-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="fda71-105">Bu makalede, SignalR 2 birim testi özelliklerini kullanmayı açıklar.</span><span class="sxs-lookup"><span data-stu-id="fda71-105">This article describes using the Unit Testing features of SignalR 2.</span></span> 
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="fda71-106">Bu konuda kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="fda71-106">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="fda71-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="fda71-107">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="fda71-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="fda71-108">.NET 4.5</span></span>
> - <span data-ttu-id="fda71-109">SignalR sürüm 2</span><span class="sxs-lookup"><span data-stu-id="fda71-109">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="fda71-110">Sorularınız ve yorumlarınız</span><span class="sxs-lookup"><span data-stu-id="fda71-110">Questions and comments</span></span>
> 
> <span data-ttu-id="fda71-111">Lütfen Bu öğretici beğendiğinizi nasıl ve ne biz sayfanın sonundaki açıklamalarında artabileceğini görüşlerinizi.</span><span class="sxs-lookup"><span data-stu-id="fda71-111">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="fda71-112">Öğretici için doğrudan ilgili olmayan sorularınız varsa, bunları nakledebilirsiniz [ASP.NET SignalR Forumu](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) veya [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="fda71-112">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<a id="unit"></a>
## <a name="unit-testing-signalr-applications"></a><span data-ttu-id="fda71-113">SignalR uygulamalarında birim testi</span><span class="sxs-lookup"><span data-stu-id="fda71-113">Unit testing SignalR applications</span></span>

<span data-ttu-id="fda71-114">SignalR uygulamanız için birim testleri oluşturmak için birim testi özellikleri SignalR 2'de kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fda71-114">You can use the unit test features in SignalR 2 to create unit tests for your SignalR application.</span></span> <span data-ttu-id="fda71-115">SignalR 2 içeren [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) test hub'ı yöntemlerinizi benzetimini yapmak için sahte bir nesneyi oluşturmak için kullanılan arabirim.</span><span class="sxs-lookup"><span data-stu-id="fda71-115">SignalR 2 includes the [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) interface, which can be used to create a mock object to simulate your hub methods for testing.</span></span>

<span data-ttu-id="fda71-116">Bu bölümde, oluşturduğunuz uygulama için birim testleri ekleyeceksiniz [Başlarken Öğreticisi](../getting-started/tutorial-getting-started-with-signalr.md) kullanarak [XUnit.net](https://github.com/xunit/xunit) ve [Moq](https://github.com/Moq/moq4).</span><span class="sxs-lookup"><span data-stu-id="fda71-116">In this section, you'll add unit tests for the application created in the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md) using [XUnit.net](https://github.com/xunit/xunit) and [Moq](https://github.com/Moq/moq4).</span></span>

<span data-ttu-id="fda71-117">XUnit.net test denetlemek için kullanılacak; Moq oluşturmak için kullanılacak bir [mock](http://en.wikipedia.org/wiki/Mock_object) test etmek için nesne.</span><span class="sxs-lookup"><span data-stu-id="fda71-117">XUnit.net will be used to control the test; Moq will be used to create a [mock](http://en.wikipedia.org/wiki/Mock_object) object for testing.</span></span> <span data-ttu-id="fda71-118">Diğer mocking çerçeveleri isterseniz kullanılabilir; [NSubstitute](http://nsubstitute.github.io/) de iyi bir seçimdir.</span><span class="sxs-lookup"><span data-stu-id="fda71-118">Other mocking frameworks can be used if desired; [NSubstitute](http://nsubstitute.github.io/) is also a good choice.</span></span> <span data-ttu-id="fda71-119">Bu öğretici iki yolla sahte nesne ayarlanacağı gösterilmiştir: ilk olarak kullanarak bir `dynamic` (.NET Framework 4'te tanıtılan) nesne ve ikincisi, bir arabirim kullanarak.</span><span class="sxs-lookup"><span data-stu-id="fda71-119">This tutorial demonstrates how to set up the mock object in two ways: First, using a `dynamic` object (introduced in .NET Framework 4), and second, using an interface.</span></span>

### <a name="contents"></a><span data-ttu-id="fda71-120">İçindekiler</span><span class="sxs-lookup"><span data-stu-id="fda71-120">Contents</span></span>

<span data-ttu-id="fda71-121">Bu öğretici aşağıdaki bölümleri içerir.</span><span class="sxs-lookup"><span data-stu-id="fda71-121">This tutorial contains the following sections.</span></span>

- [<span data-ttu-id="fda71-122">Birim testi ile dinamik</span><span class="sxs-lookup"><span data-stu-id="fda71-122">Unit testing with Dynamic</span></span>](#dynamic)
- [<span data-ttu-id="fda71-123">Birim türü tarafından testi</span><span class="sxs-lookup"><span data-stu-id="fda71-123">Unit testing by type</span></span>](#type)

<a id="dynamic"></a>
### <a name="unit-testing-with-dynamic"></a><span data-ttu-id="fda71-124">Birim testi ile dinamik</span><span class="sxs-lookup"><span data-stu-id="fda71-124">Unit testing with Dynamic</span></span>

<span data-ttu-id="fda71-125">Bu bölümde, oluşturduğunuz uygulama için bir birim testi ekleyeceksiniz [Başlarken Öğreticisi](../getting-started/tutorial-getting-started-with-signalr.md) bir dinamik Nesne kullanma.</span><span class="sxs-lookup"><span data-stu-id="fda71-125">In this section, you'll add a unit test for the application created in the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md) using a dynamic object.</span></span>

1. <span data-ttu-id="fda71-126">Yükleme [XUnit Çalıştırıcısı uzantısı](https://visualstudiogallery.msdn.microsoft.com/463c5987-f82b-46c8-a97e-b1cde42b9099) Visual Studio 2013 için.</span><span class="sxs-lookup"><span data-stu-id="fda71-126">Install the [XUnit Runner extension](https://visualstudiogallery.msdn.microsoft.com/463c5987-f82b-46c8-a97e-b1cde42b9099) for Visual Studio 2013.</span></span>
2. <span data-ttu-id="fda71-127">Ya da tamamlamak [Başlarken Öğreticisi](../getting-started/tutorial-getting-started-with-signalr.md), veya tamamlanmış uygulamadan indirme [MSDN kod Galerisi'nden](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).</span><span class="sxs-lookup"><span data-stu-id="fda71-127">Either complete the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md), or download the completed application from [MSDN Code Gallery](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).</span></span>
3. <span data-ttu-id="fda71-128">Başlarken uygulaması indirme sürümünü kullanıyorsanız, açık **Paket Yöneticisi Konsolu** tıklatıp **geri** SignalR paketini projeye eklemek için.</span><span class="sxs-lookup"><span data-stu-id="fda71-128">If you are using the download version of the Getting Started application, open **Package Manager Console** and click **Restore** to add the SignalR package to the project.</span></span>

    ![Paket geri yükleme](unit-testing-signalr-applications/_static/image1.png)
4. <span data-ttu-id="fda71-130">Birim testi için çözümü bir proje ekleyin.</span><span class="sxs-lookup"><span data-stu-id="fda71-130">Add a project to the solution for the unit test.</span></span> <span data-ttu-id="fda71-131">Çözümünüze sağ **Çözüm Gezgini** seçip **Ekle**, **yeni proje...** . Altında **C#** düğümü, select **Windows** düğümü.</span><span class="sxs-lookup"><span data-stu-id="fda71-131">Right-click your solution in **Solution Explorer** and select **Add**, **New Project...**. Under the **C#** node, select the **Windows** node.</span></span> <span data-ttu-id="fda71-132">Seçin **sınıf kitaplığı**.</span><span class="sxs-lookup"><span data-stu-id="fda71-132">Select **Class Library**.</span></span> <span data-ttu-id="fda71-133">Yeni proje adı **TestLibrary** tıklatıp **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="fda71-133">Name the new project **TestLibrary** and click **OK**.</span></span>

    ![Test kitaplığı oluşturma](unit-testing-signalr-applications/_static/image2.png)
5. <span data-ttu-id="fda71-135">Bir başvuru test kitaplığı projesinde SignalRChat projeye ekleyin.</span><span class="sxs-lookup"><span data-stu-id="fda71-135">Add a reference in the test library project to the SignalRChat project.</span></span> <span data-ttu-id="fda71-136">Sağ **TestLibrary** proje ve seçin **Ekle**, **başvuru...** . Seçin **projeleri** düğümü altında **çözüm** düğümü ve onay **SignalRChat**.</span><span class="sxs-lookup"><span data-stu-id="fda71-136">Right-click the **TestLibrary** project and select **Add**, **Reference...**. Select the **Projects** node under the **Solution** node, and check **SignalRChat**.</span></span> <span data-ttu-id="fda71-137">**Tamam**'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="fda71-137">Click **OK**.</span></span>

    ![Proje Başvurusu Ekle](unit-testing-signalr-applications/_static/image3.png)
6. <span data-ttu-id="fda71-139">SignalR, Moq ve XUnit paketlere Ekle **TestLibrary** projesi.</span><span class="sxs-lookup"><span data-stu-id="fda71-139">Add the SignalR, Moq, and XUnit packages to the **TestLibrary** project.</span></span> <span data-ttu-id="fda71-140">İçinde **Paket Yöneticisi Konsolu**ayarlayın **varsayılan proje** açılır **TestLibrary**.</span><span class="sxs-lookup"><span data-stu-id="fda71-140">In the **Package Manager Console**, set the **Default Project** dropdown to **TestLibrary**.</span></span> <span data-ttu-id="fda71-141">Konsol penceresinde aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="fda71-141">Run the following commands in the console window:</span></span>

   - `Install-Package Microsoft.AspNet.SignalR`
   - `Install-Package Moq`
   - `Install-Package XUnit`

     ![Paket yüklemek için](unit-testing-signalr-applications/_static/image4.png)
7. <span data-ttu-id="fda71-143">Test dosyası oluşturun.</span><span class="sxs-lookup"><span data-stu-id="fda71-143">Create the test file.</span></span> <span data-ttu-id="fda71-144">Sağ **TestLibrary** proje ve tıklatın **Ekle...** , **Sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="fda71-144">Right-click the **TestLibrary** project and click **Add...**, **Class**.</span></span> <span data-ttu-id="fda71-145">Yeni sınıf **Tests.cs**.</span><span class="sxs-lookup"><span data-stu-id="fda71-145">Name the new class **Tests.cs**.</span></span>
8. <span data-ttu-id="fda71-146">Tests.cs içeriğini aşağıdaki kodla değiştirin.</span><span class="sxs-lookup"><span data-stu-id="fda71-146">Replace the contents of Tests.cs with the following code.</span></span>

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample1.cs)]

    <span data-ttu-id="fda71-147">Yukarıdaki kod test istemcisi kullanılarak oluşturulan `Mock` nesnesinin [Moq](https://github.com/Moq/moq4) tür kitaplığı [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (SignalR 2.1 atamak `dynamic` türü için parametre.) `IHubCallerConnectionContext` İstemcide yöntemleri ile başlattığınıza proxy nesnesi bir arabirimdir.</span><span class="sxs-lookup"><span data-stu-id="fda71-147">In the code above, a test client is created using the `Mock` object from the [Moq](https://github.com/Moq/moq4) library, of type [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (in SignalR 2.1, assign `dynamic` for the type parameter.) The `IHubCallerConnectionContext` interface is the proxy object with which you invoke methods on the client.</span></span> <span data-ttu-id="fda71-148">`broadcastMessage` Çağıran böylece işlevi sahte istemcisi ardından tanımlanan `ChatHub` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="fda71-148">The `broadcastMessage` function is then defined for the mock client so that it can be called by the `ChatHub` class.</span></span> <span data-ttu-id="fda71-149">Test altyapısı sonra çağırır `Send` yöntemi `ChatHub` sırayla mocked çağırır sınıfı `broadcastMessage` işlevi.</span><span class="sxs-lookup"><span data-stu-id="fda71-149">The test engine then calls the `Send` method of the `ChatHub` class, which in turn calls the mocked `broadcastMessage` function.</span></span>
9. <span data-ttu-id="fda71-150">Tuşlarına basarak çözümü oluşturun **F6**.</span><span class="sxs-lookup"><span data-stu-id="fda71-150">Build the solution by pressing **F6**.</span></span>
10. <span data-ttu-id="fda71-151">Birim testi çalıştırma.</span><span class="sxs-lookup"><span data-stu-id="fda71-151">Run the unit test.</span></span> <span data-ttu-id="fda71-152">Visual Studio'da seçin **Test**, **Windows**, **Test Gezgini**.</span><span class="sxs-lookup"><span data-stu-id="fda71-152">In Visual Studio, select **Test**, **Windows**, **Test Explorer**.</span></span> <span data-ttu-id="fda71-153">Test Gezgini penceresinde sağ **HubsAreMockableViaDynamic** seçip **seçili Testleri Çalıştır**.</span><span class="sxs-lookup"><span data-stu-id="fda71-153">In the Test Explorer window, right-click **HubsAreMockableViaDynamic** and select **Run Selected Tests**.</span></span>

    ![Test Gezgini](unit-testing-signalr-applications/_static/image5.png)
11. <span data-ttu-id="fda71-155">Test Gezgini penceresinin alt bölmesinde denetleyerek geçilen testi doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="fda71-155">Verify that the test passed by checking the lower pane in the Test Explorer window.</span></span> <span data-ttu-id="fda71-156">Pencere testini geçtiğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="fda71-156">The window will show that the test passed.</span></span>

    ![Sınama başarılı](unit-testing-signalr-applications/_static/image6.png)

<a id="type"></a>
### <a name="unit-testing-by-type"></a><span data-ttu-id="fda71-158">Birim türü tarafından testi</span><span class="sxs-lookup"><span data-stu-id="fda71-158">Unit testing by type</span></span>

<span data-ttu-id="fda71-159">Bu bölümde, oluşturduğunuz uygulama için bir sınama ekleyeceksiniz [Başlarken Öğreticisi](../getting-started/tutorial-getting-started-with-signalr.md) sınanacak yöntemi içeren bir arabirim kullanarak.</span><span class="sxs-lookup"><span data-stu-id="fda71-159">In this section, you'll add a test for the application created in the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md) using an interface that contains the method to be tested.</span></span>

1. <span data-ttu-id="fda71-160">Adım 1-7'de tamamlamak [birim testi ile dinamik](#dynamic) yukarıdaki öğretici.</span><span class="sxs-lookup"><span data-stu-id="fda71-160">Complete steps 1-7 in the [Unit testing with Dynamic](#dynamic) tutorial above.</span></span>
2. <span data-ttu-id="fda71-161">Tests.cs içeriğini aşağıdaki kodla değiştirin.</span><span class="sxs-lookup"><span data-stu-id="fda71-161">Replace the contents of Tests.cs with the following code.</span></span>

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample2.cs)]

    <span data-ttu-id="fda71-162">Yukarıdaki kod imzası tanımlayan bir arabirim oluşturulur `broadcastMessage` yöntemi için test altyapısı sahte istemci oluşturacaktır.</span><span class="sxs-lookup"><span data-stu-id="fda71-162">In the code above, an interface is created defining the signature of the `broadcastMessage` method for which the test engine will create a mock client.</span></span> <span data-ttu-id="fda71-163">Sahte bir istemci, ardından kullanılarak oluşturulur `Mock` türünde nesne [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (SignalR 2.1 atamak `dynamic` tür parametresi için.) `IHubCallerConnectionContext` İstemcide yöntemleri ile başlattığınıza proxy nesnesi bir arabirimdir.</span><span class="sxs-lookup"><span data-stu-id="fda71-163">A mock client is then created using the `Mock` object, of type [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (in SignalR 2.1, assign `dynamic` for the type parameter.) The `IHubCallerConnectionContext` interface is the proxy object with which you invoke methods on the client.</span></span>

    <span data-ttu-id="fda71-164">Test sonra bir örneğini oluşturur `ChatHub`ve sahte bir sürümüne oluşturur `broadcastMessage` çağırarak sırayla çağrılır yöntemi `Send` hub'ında yöntemi.</span><span class="sxs-lookup"><span data-stu-id="fda71-164">The test then creates an instance of `ChatHub`, and then creates a mock version of the `broadcastMessage` method, which in turn is invoked by calling the `Send` method on the hub.</span></span>
3. <span data-ttu-id="fda71-165">Tuşlarına basarak çözümü oluşturun **F6**.</span><span class="sxs-lookup"><span data-stu-id="fda71-165">Build the solution by pressing **F6**.</span></span>
4. <span data-ttu-id="fda71-166">Birim testi çalıştırma.</span><span class="sxs-lookup"><span data-stu-id="fda71-166">Run the unit test.</span></span> <span data-ttu-id="fda71-167">Visual Studio'da seçin **Test**, **Windows**, **Test Gezgini**.</span><span class="sxs-lookup"><span data-stu-id="fda71-167">In Visual Studio, select **Test**, **Windows**, **Test Explorer**.</span></span> <span data-ttu-id="fda71-168">Test Gezgini penceresinde sağ **HubsAreMockableViaDynamic** seçip **seçili Testleri Çalıştır**.</span><span class="sxs-lookup"><span data-stu-id="fda71-168">In the Test Explorer window, right-click **HubsAreMockableViaDynamic** and select **Run Selected Tests**.</span></span>

    ![Test Gezgini](unit-testing-signalr-applications/_static/image7.png)
5. <span data-ttu-id="fda71-170">Test Gezgini penceresinin alt bölmesinde denetleyerek geçilen testi doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="fda71-170">Verify that the test passed by checking the lower pane in the Test Explorer window.</span></span> <span data-ttu-id="fda71-171">Pencere testini geçtiğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="fda71-171">The window will show that the test passed.</span></span>

    ![Sınama başarılı](unit-testing-signalr-applications/_static/image8.png)
