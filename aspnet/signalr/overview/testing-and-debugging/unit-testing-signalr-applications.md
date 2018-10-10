---
uid: signalr/overview/testing-and-debugging/unit-testing-signalr-applications
title: Birim SignalR uygulamalarını test etme | Microsoft Docs
author: pfletcher
description: Bu makalede, SignalR 2.0 birim testi özelliklerinin nasıl kullanılacağını açıklar.
ms.author: riande
ms.date: 06/10/2014
ms.assetid: d1983524-e0d5-4ee6-9d87-1f552f7cb964
msc.legacyurl: /signalr/overview/testing-and-debugging/unit-testing-signalr-applications
msc.type: authoredcontent
ms.openlocfilehash: ba8f5d4577403fe9765641d7ee5d88bde045680a
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48910830"
---
<a name="unit-testing-signalr-applications"></a><span data-ttu-id="33060-103">SignalR uygulamalarına birim testi</span><span class="sxs-lookup"><span data-stu-id="33060-103">Unit Testing SignalR Applications</span></span>
====================
<span data-ttu-id="33060-104">tarafından [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="33060-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="33060-105">Bu makalede, SignalR 2 birim testi özelliklerini kullanmayı açıklar.</span><span class="sxs-lookup"><span data-stu-id="33060-105">This article describes using the Unit Testing features of SignalR 2.</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="33060-106">Bu konu başlığında kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="33060-106">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="33060-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="33060-107">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="33060-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="33060-108">.NET 4.5</span></span>
> - <span data-ttu-id="33060-109">SignalR sürüm 2</span><span class="sxs-lookup"><span data-stu-id="33060-109">SignalR version 2</span></span>
>
>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="33060-110">Sorularınız ve yorumlarınız</span><span class="sxs-lookup"><span data-stu-id="33060-110">Questions and comments</span></span>
>
> <span data-ttu-id="33060-111">Lütfen bu öğreticide sevmediğinizi nasıl ve ne sayfanın alt kısmındaki açıklamalarda geliştirebileceğimiz hakkında geri bildirim bırakın.</span><span class="sxs-lookup"><span data-stu-id="33060-111">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="33060-112">Öğretici için doğrudan ilgili olmayan sorularınız varsa, bunları gönderebilir [ASP.NET SignalR Forumu](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) veya [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="33060-112">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<a id="unit"></a>
## <a name="unit-testing-signalr-applications"></a><span data-ttu-id="33060-113">Birim SignalR uygulamalarını test etme</span><span class="sxs-lookup"><span data-stu-id="33060-113">Unit testing SignalR applications</span></span>

<span data-ttu-id="33060-114">SignalR uygulamanız için birim testleri oluşturmak için SignalR 2'de birim testi özellikleri kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="33060-114">You can use the unit test features in SignalR 2 to create unit tests for your SignalR application.</span></span> <span data-ttu-id="33060-115">SignalR 2 içeren [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) test hub'ı yöntemlerinizi benzetimini yapmak için sahte bir nesne oluşturmak için kullanılan arabirim.</span><span class="sxs-lookup"><span data-stu-id="33060-115">SignalR 2 includes the [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) interface, which can be used to create a mock object to simulate your hub methods for testing.</span></span>

<span data-ttu-id="33060-116">Bu bölümde, oluşturulan uygulama için birim testleri ekleyeceksiniz [Başlarken Öğreticisi](../getting-started/tutorial-getting-started-with-signalr.md) kullanarak [XUnit.net](https://github.com/xunit/xunit) ve [Moq](https://github.com/Moq/moq4).</span><span class="sxs-lookup"><span data-stu-id="33060-116">In this section, you'll add unit tests for the application created in the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md) using [XUnit.net](https://github.com/xunit/xunit) and [Moq](https://github.com/Moq/moq4).</span></span>

<span data-ttu-id="33060-117">XUnit.net test denetlemek için kullanılacak; Moq oluşturmak için kullanılacak bir [sahte](http://en.wikipedia.org/wiki/Mock_object) sınama nesnesi.</span><span class="sxs-lookup"><span data-stu-id="33060-117">XUnit.net will be used to control the test; Moq will be used to create a [mock](http://en.wikipedia.org/wiki/Mock_object) object for testing.</span></span> <span data-ttu-id="33060-118">Sahte işlem diğer çerçeveler, isterseniz kullanılabilir; [NSubstitute](http://nsubstitute.github.io/) de iyi bir seçimdir.</span><span class="sxs-lookup"><span data-stu-id="33060-118">Other mocking frameworks can be used if desired; [NSubstitute](http://nsubstitute.github.io/) is also a good choice.</span></span> <span data-ttu-id="33060-119">Bu öğreticide, iki yolla sahte nesnenin oluşturulacağı gösterilmektedir: ilk olarak kullanarak bir `dynamic` (.NET Framework 4'te sunulmuştur) nesne ve ikinci bir arabirim kullanarak.</span><span class="sxs-lookup"><span data-stu-id="33060-119">This tutorial demonstrates how to set up the mock object in two ways: First, using a `dynamic` object (introduced in .NET Framework 4), and second, using an interface.</span></span>

### <a name="contents"></a><span data-ttu-id="33060-120">İçindekiler</span><span class="sxs-lookup"><span data-stu-id="33060-120">Contents</span></span>

<span data-ttu-id="33060-121">Bu öğreticide, aşağıdaki bölümleri içerir.</span><span class="sxs-lookup"><span data-stu-id="33060-121">This tutorial contains the following sections.</span></span>

- [<span data-ttu-id="33060-122">Birim testi ile dinamik</span><span class="sxs-lookup"><span data-stu-id="33060-122">Unit testing with Dynamic</span></span>](#dynamic)
- [<span data-ttu-id="33060-123">Birim testi türüne göre</span><span class="sxs-lookup"><span data-stu-id="33060-123">Unit testing by type</span></span>](#type)

<a id="dynamic"></a>
### <a name="unit-testing-with-dynamic"></a><span data-ttu-id="33060-124">Birim testi ile dinamik</span><span class="sxs-lookup"><span data-stu-id="33060-124">Unit testing with Dynamic</span></span>

<span data-ttu-id="33060-125">Bu bölümde, oluşturulan uygulama için bir birim testi ekleyeceksiniz [Başlarken Öğreticisi](../getting-started/tutorial-getting-started-with-signalr.md) kullanarak dinamik bir nesne.</span><span class="sxs-lookup"><span data-stu-id="33060-125">In this section, you'll add a unit test for the application created in the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md) using a dynamic object.</span></span>

1. <span data-ttu-id="33060-126">Yükleme [XUnit Çalıştırıcısı uzantısı](https://visualstudiogallery.msdn.microsoft.com/463c5987-f82b-46c8-a97e-b1cde42b9099) Visual Studio 2013 için.</span><span class="sxs-lookup"><span data-stu-id="33060-126">Install the [XUnit Runner extension](https://visualstudiogallery.msdn.microsoft.com/463c5987-f82b-46c8-a97e-b1cde42b9099) for Visual Studio 2013.</span></span>
2. <span data-ttu-id="33060-127">Ya da tamamlamak [Başlarken Öğreticisi](../getting-started/tutorial-getting-started-with-signalr.md), veya tamamlanmış uygulamayı karşıdan [MSDN Kod Galerisi](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).</span><span class="sxs-lookup"><span data-stu-id="33060-127">Either complete the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md), or download the completed application from [MSDN Code Gallery](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).</span></span>
3. <span data-ttu-id="33060-128">Başlarken uygulaması indirme sürümünü kullanıyorsanız, **Paket Yöneticisi Konsolu** tıklatıp **geri** SignalR paketini projeye eklemek için.</span><span class="sxs-lookup"><span data-stu-id="33060-128">If you are using the download version of the Getting Started application, open **Package Manager Console** and click **Restore** to add the SignalR package to the project.</span></span>

    ![Paketleri geri yükle](unit-testing-signalr-applications/_static/image1.png)
4. <span data-ttu-id="33060-130">Birim testi için çözüme bir proje ekleyin.</span><span class="sxs-lookup"><span data-stu-id="33060-130">Add a project to the solution for the unit test.</span></span> <span data-ttu-id="33060-131">Çözümünüzde sağ **Çözüm Gezgini** seçip **Ekle**, **yeni proje...** . Altında **C#** düğümünü **Windows** düğümü.</span><span class="sxs-lookup"><span data-stu-id="33060-131">Right-click your solution in **Solution Explorer** and select **Add**, **New Project...**. Under the **C#** node, select the **Windows** node.</span></span> <span data-ttu-id="33060-132">Seçin **sınıf kitaplığı**.</span><span class="sxs-lookup"><span data-stu-id="33060-132">Select **Class Library**.</span></span> <span data-ttu-id="33060-133">Yeni proje adını **TestLibrary** tıklatıp **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="33060-133">Name the new project **TestLibrary** and click **OK**.</span></span>

    ![Test kitaplığı oluşturma](unit-testing-signalr-applications/_static/image2.png)
5. <span data-ttu-id="33060-135">Bir başvuru test kitaplığı projesinde SignalRChat projeye ekleyin.</span><span class="sxs-lookup"><span data-stu-id="33060-135">Add a reference in the test library project to the SignalRChat project.</span></span> <span data-ttu-id="33060-136">Sağ **TestLibrary** seçin ve proje **Ekle**, **başvurusu...** . Seçin **projeleri** düğümünde **çözüm** düğüm ve onay **SignalRChat**.</span><span class="sxs-lookup"><span data-stu-id="33060-136">Right-click the **TestLibrary** project and select **Add**, **Reference...**. Select the **Projects** node under the **Solution** node, and check **SignalRChat**.</span></span> <span data-ttu-id="33060-137">**Tamam**'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="33060-137">Click **OK**.</span></span>

    ![Proje Başvurusu Ekle](unit-testing-signalr-applications/_static/image3.png)
6. <span data-ttu-id="33060-139">SignalR ve Moq XUnit paketlere Ekle **TestLibrary** proje.</span><span class="sxs-lookup"><span data-stu-id="33060-139">Add the SignalR, Moq, and XUnit packages to the **TestLibrary** project.</span></span> <span data-ttu-id="33060-140">İçinde **Paket Yöneticisi Konsolu**ayarlayın **varsayılan proje** açılan **TestLibrary**.</span><span class="sxs-lookup"><span data-stu-id="33060-140">In the **Package Manager Console**, set the **Default Project** dropdown to **TestLibrary**.</span></span> <span data-ttu-id="33060-141">Konsol penceresinde aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="33060-141">Run the following commands in the console window:</span></span>

   - `Install-Package Microsoft.AspNet.SignalR`
   - `Install-Package Moq`
   - `Install-Package XUnit`

     ![Paketleri yükleme](unit-testing-signalr-applications/_static/image4.png)
7. <span data-ttu-id="33060-143">Test dosyası oluşturun.</span><span class="sxs-lookup"><span data-stu-id="33060-143">Create the test file.</span></span> <span data-ttu-id="33060-144">Sağ **TestLibrary** projesine **Ekle...** , **Sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="33060-144">Right-click the **TestLibrary** project and click **Add...**, **Class**.</span></span> <span data-ttu-id="33060-145">Yeni bir sınıf adı **Tests.cs**.</span><span class="sxs-lookup"><span data-stu-id="33060-145">Name the new class **Tests.cs**.</span></span>
8. <span data-ttu-id="33060-146">Tests.cs içeriğini aşağıdaki kodla değiştirin.</span><span class="sxs-lookup"><span data-stu-id="33060-146">Replace the contents of Tests.cs with the following code.</span></span>

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample1.cs)]

    <span data-ttu-id="33060-147">Yukarıdaki kod test istemcisi kullanılarak oluşturulan `Mock` nesnesinden [Moq](https://github.com/Moq/moq4) tür kitaplığı [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (SignalR 2.1 içinde atama `dynamic` türü için parametre.) `IHubCallerConnectionContext` Arabirimi ile istemci üzerinde yöntemleri çağırmayı proxy nesnedir.</span><span class="sxs-lookup"><span data-stu-id="33060-147">In the code above, a test client is created using the `Mock` object from the [Moq](https://github.com/Moq/moq4) library, of type [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (in SignalR 2.1, assign `dynamic` for the type parameter.) The `IHubCallerConnectionContext` interface is the proxy object with which you invoke methods on the client.</span></span> <span data-ttu-id="33060-148">`broadcastMessage` Çağıran, böylece işlev için sahte istemci sonra tanımlanmış `ChatHub` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="33060-148">The `broadcastMessage` function is then defined for the mock client so that it can be called by the `ChatHub` class.</span></span> <span data-ttu-id="33060-149">Sonra test motoru çağırır `Send` yöntemi `ChatHub` sırayla sahte çağrıları sınıfı `broadcastMessage` işlevi.</span><span class="sxs-lookup"><span data-stu-id="33060-149">The test engine then calls the `Send` method of the `ChatHub` class, which in turn calls the mocked `broadcastMessage` function.</span></span>
9. <span data-ttu-id="33060-150">Basarak çözümü oluşturun **F6**.</span><span class="sxs-lookup"><span data-stu-id="33060-150">Build the solution by pressing **F6**.</span></span>
10. <span data-ttu-id="33060-151">Birim testini çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="33060-151">Run the unit test.</span></span> <span data-ttu-id="33060-152">Visual Studio'da **Test**, **Windows**, **Test Gezgini**.</span><span class="sxs-lookup"><span data-stu-id="33060-152">In Visual Studio, select **Test**, **Windows**, **Test Explorer**.</span></span> <span data-ttu-id="33060-153">Test Gezgini penceresinde sağ **HubsAreMockableViaDynamic** seçip **seçili Testleri Çalıştır**.</span><span class="sxs-lookup"><span data-stu-id="33060-153">In the Test Explorer window, right-click **HubsAreMockableViaDynamic** and select **Run Selected Tests**.</span></span>

    ![Test Gezgini](unit-testing-signalr-applications/_static/image5.png)
11. <span data-ttu-id="33060-155">Test, Test Gezgini penceresinin alt bölmede denetleyerek geçtiğini doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="33060-155">Verify that the test passed by checking the lower pane in the Test Explorer window.</span></span> <span data-ttu-id="33060-156">Pencere geçilen testleri gösterir.</span><span class="sxs-lookup"><span data-stu-id="33060-156">The window will show that the test passed.</span></span>

    ![Test geçildi](unit-testing-signalr-applications/_static/image6.png)

<a id="type"></a>
### <a name="unit-testing-by-type"></a><span data-ttu-id="33060-158">Birim testi türüne göre</span><span class="sxs-lookup"><span data-stu-id="33060-158">Unit testing by type</span></span>

<span data-ttu-id="33060-159">Bu bölümde, oluşturulan uygulama için bir test ekleyeceksiniz [Başlarken Öğreticisi](../getting-started/tutorial-getting-started-with-signalr.md) test edilecek yöntemi içeren bir arabirim kullanarak.</span><span class="sxs-lookup"><span data-stu-id="33060-159">In this section, you'll add a test for the application created in the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md) using an interface that contains the method to be tested.</span></span>

1. <span data-ttu-id="33060-160">Adım 1-7 tamamlamak [birim testi ile dinamik](#dynamic) yukarıdaki öğretici.</span><span class="sxs-lookup"><span data-stu-id="33060-160">Complete steps 1-7 in the [Unit testing with Dynamic](#dynamic) tutorial above.</span></span>
2. <span data-ttu-id="33060-161">Tests.cs içeriğini aşağıdaki kodla değiştirin.</span><span class="sxs-lookup"><span data-stu-id="33060-161">Replace the contents of Tests.cs with the following code.</span></span>

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample2.cs)]

    <span data-ttu-id="33060-162">Yukarıdaki kod imzasını tanımlayan bir arabirimi oluşturulur `broadcastMessage` yöntemi için test motoru sahte bir istemci oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="33060-162">In the code above, an interface is created defining the signature of the `broadcastMessage` method for which the test engine will create a mock client.</span></span> <span data-ttu-id="33060-163">Sahte bir istemci kullanarak sonra oluşturulan `Mock` nesnenin türü [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (SignalR 2.1 içinde atama `dynamic` tür parametresi için.) `IHubCallerConnectionContext` Arabirimi ile istemci üzerinde yöntemleri çağırmayı proxy nesnedir.</span><span class="sxs-lookup"><span data-stu-id="33060-163">A mock client is then created using the `Mock` object, of type [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (in SignalR 2.1, assign `dynamic` for the type parameter.) The `IHubCallerConnectionContext` interface is the proxy object with which you invoke methods on the client.</span></span>

    <span data-ttu-id="33060-164">Test sonra bir örneğini oluşturur `ChatHub`ve ardından sahte bir sürümüne oluşturan `broadcastMessage` yöntemi çağırarak sırayla çağrılır `Send` hub yöntemi.</span><span class="sxs-lookup"><span data-stu-id="33060-164">The test then creates an instance of `ChatHub`, and then creates a mock version of the `broadcastMessage` method, which in turn is invoked by calling the `Send` method on the hub.</span></span>
3. <span data-ttu-id="33060-165">Basarak çözümü oluşturun **F6**.</span><span class="sxs-lookup"><span data-stu-id="33060-165">Build the solution by pressing **F6**.</span></span>
4. <span data-ttu-id="33060-166">Birim testini çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="33060-166">Run the unit test.</span></span> <span data-ttu-id="33060-167">Visual Studio'da **Test**, **Windows**, **Test Gezgini**.</span><span class="sxs-lookup"><span data-stu-id="33060-167">In Visual Studio, select **Test**, **Windows**, **Test Explorer**.</span></span> <span data-ttu-id="33060-168">Test Gezgini penceresinde sağ **HubsAreMockableViaDynamic** seçip **seçili Testleri Çalıştır**.</span><span class="sxs-lookup"><span data-stu-id="33060-168">In the Test Explorer window, right-click **HubsAreMockableViaDynamic** and select **Run Selected Tests**.</span></span>

    ![Test Gezgini](unit-testing-signalr-applications/_static/image7.png)
5. <span data-ttu-id="33060-170">Test, Test Gezgini penceresinin alt bölmede denetleyerek geçtiğini doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="33060-170">Verify that the test passed by checking the lower pane in the Test Explorer window.</span></span> <span data-ttu-id="33060-171">Pencere geçilen testleri gösterir.</span><span class="sxs-lookup"><span data-stu-id="33060-171">The window will show that the test passed.</span></span>

    ![Test geçildi](unit-testing-signalr-applications/_static/image8.png)
