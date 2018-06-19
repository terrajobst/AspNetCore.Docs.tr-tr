---
uid: web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
title: ASP.NET Web Forms ile Web API kullanma | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/03/2012
ms.topic: article
ms.assetid: 25da8c3f-4e90-4946-9765-4f160985e1e4
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
msc.type: authoredcontent
ms.openlocfilehash: af918671a8bcc97a0050ea033ccd14dd96416e73
ms.sourcegitcommit: 037d3900f739dbaa2ba14158e3d7dc81478952ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2017
ms.locfileid: "26575856"
---
<a name="using-web-api-with-aspnet-web-forms"></a><span data-ttu-id="12d7f-102">ASP.NET Web Forms ile Web API kullanma</span><span class="sxs-lookup"><span data-stu-id="12d7f-102">Using Web API with ASP.NET Web Forms</span></span>
====================
<span data-ttu-id="12d7f-103">tarafından [CAN Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="12d7f-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="12d7f-104">ASP.NET Web API, ASP.NET MVC ile paketlenmiştir rağmen geleneksel bir ASP.NET Web Forms uygulamasına Web API ekleme kolaydır.</span><span class="sxs-lookup"><span data-stu-id="12d7f-104">Although ASP.NET Web API is packaged with ASP.NET MVC, it is easy to add Web API to a traditional ASP.NET Web Forms application.</span></span> <span data-ttu-id="12d7f-105">Bu öğretici adımlarda size yol gösterir.</span><span class="sxs-lookup"><span data-stu-id="12d7f-105">This tutorial walks you through the steps.</span></span>

## <a name="overview"></a><span data-ttu-id="12d7f-106">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="12d7f-106">Overview</span></span>

<span data-ttu-id="12d7f-107">Bir Web Forms uygulamasında Web API kullanmak için iki ana adım vardır:</span><span class="sxs-lookup"><span data-stu-id="12d7f-107">To use Web API in a Web Forms application, there are two main steps:</span></span>

- <span data-ttu-id="12d7f-108">Türetilen bir Web API denetleyicisi eklemek **ApiController** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="12d7f-108">Add a Web API controller that derives from the **ApiController** class.</span></span>
- <span data-ttu-id="12d7f-109">Bir rota tablosuna eklemek **uygulama\_Başlat** yöntemi.</span><span class="sxs-lookup"><span data-stu-id="12d7f-109">Add a route table to the **Application\_Start** method.</span></span>

## <a name="create-a-web-forms-project"></a><span data-ttu-id="12d7f-110">Web Forms projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="12d7f-110">Create a Web Forms Project</span></span>

<span data-ttu-id="12d7f-111">Visual Studio'yu başlatın ve seçin **yeni proje** gelen **Başlat** sayfası.</span><span class="sxs-lookup"><span data-stu-id="12d7f-111">Start Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="12d7f-112">Veya **dosya** menüsünde, select **yeni** ve ardından **proje**.</span><span class="sxs-lookup"><span data-stu-id="12d7f-112">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="12d7f-113">İçinde **şablonları** bölmesinde, **yüklü şablonlar** ve genişletin **Visual C#** düğümü.</span><span class="sxs-lookup"><span data-stu-id="12d7f-113">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="12d7f-114">Altında **Visual C#** seçin **Web**.</span><span class="sxs-lookup"><span data-stu-id="12d7f-114">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="12d7f-115">Proje şablonları listesinde seçin **ASP.NET Web Forms uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="12d7f-115">In the list of project templates, select **ASP.NET Web Forms Application**.</span></span> <span data-ttu-id="12d7f-116">Proje için bir ad girin ve tıklayın **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="12d7f-116">Enter a name for the project and click **OK**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image1.png)

## <a name="create-the-model-and-controller"></a><span data-ttu-id="12d7f-117">Model ve denetleyici oluşturma</span><span class="sxs-lookup"><span data-stu-id="12d7f-117">Create the Model and Controller</span></span>

<span data-ttu-id="12d7f-118">Bu öğretici olarak aynı model ve denetleyici sınıfları kullanır [Başlarken](tutorial-your-first-web-api.md) Öğreticisi.</span><span class="sxs-lookup"><span data-stu-id="12d7f-118">This tutorial uses the same model and controller classes as the [Getting Started](tutorial-your-first-web-api.md) tutorial.</span></span>

<span data-ttu-id="12d7f-119">İlk olarak, bir model sınıfı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="12d7f-119">First, add a model class.</span></span> <span data-ttu-id="12d7f-120">İçinde **Çözüm Gezgini**, projeye sağ tıklayın ve seçin **sınıfı Ekle**.</span><span class="sxs-lookup"><span data-stu-id="12d7f-120">In **Solution Explorer**, right-click the project and select **Add Class**.</span></span> <span data-ttu-id="12d7f-121">Ürün sınıf adını ve aşağıdaki uygulama ekleyin:</span><span class="sxs-lookup"><span data-stu-id="12d7f-121">Name the class Product, and add the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample1.cs)]

<span data-ttu-id="12d7f-122">Ardından, Web API denetleyicisi proje, A ekleyin. *denetleyicisi* için Web API HTTP isteklerini işler nesnesidir.</span><span class="sxs-lookup"><span data-stu-id="12d7f-122">Next, add a Web API controller to the project., A *controller* is the object that handles HTTP requests for Web API.</span></span>

<span data-ttu-id="12d7f-123">İçinde **Çözüm Gezgini**, projeye sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="12d7f-123">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="12d7f-124">Seçin **Yeni Öğe Ekle**.</span><span class="sxs-lookup"><span data-stu-id="12d7f-124">Select **Add New Item**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image2.png)

<span data-ttu-id="12d7f-125">Altında **yüklü şablonlar**, genişletin **Visual C#** seçip **Web**.</span><span class="sxs-lookup"><span data-stu-id="12d7f-125">Under **Installed Templates**, expand **Visual C#** and select **Web**.</span></span> <span data-ttu-id="12d7f-126">Şablonları listesinden seçip **Web API denetleyicisi sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="12d7f-126">Then, from the list of templates, select **Web API Controller Class**.</span></span> <span data-ttu-id="12d7f-127">"ProductsController" Denetleyici adı ve'ı tıklatın **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="12d7f-127">Name the controller "ProductsController" and click **Add**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image3.png)

<span data-ttu-id="12d7f-128">**Yeni Öğe Ekle** Sihirbazı ProductsController.cs adlı bir dosya oluşturur.</span><span class="sxs-lookup"><span data-stu-id="12d7f-128">The **Add New Item** wizard will create a file named ProductsController.cs.</span></span> <span data-ttu-id="12d7f-129">Sihirbaz dahil yöntemlerini silin ve aşağıdaki yöntemleri ekleyin:</span><span class="sxs-lookup"><span data-stu-id="12d7f-129">Delete the methods that the wizard included and add the following methods:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample2.cs)]

<span data-ttu-id="12d7f-130">Bu denetleyici kodda hakkında daha fazla bilgi için bkz: [Başlarken](tutorial-your-first-web-api.md) Öğreticisi.</span><span class="sxs-lookup"><span data-stu-id="12d7f-130">For more information about the code in this controller, see the [Getting Started](tutorial-your-first-web-api.md) tutorial.</span></span>

## <a name="add-routing-information"></a><span data-ttu-id="12d7f-131">Yönlendirme bilgileri</span><span class="sxs-lookup"><span data-stu-id="12d7f-131">Add Routing Information</span></span>

<span data-ttu-id="12d7f-132">Ardından, bir URI yolu böylece ekleyeceğiz bu URI biçiminde &quot;/api/ürünler/&quot; denetleyiciye yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="12d7f-132">Next, we'll add a URI route so that URIs of the form &quot;/api/products/&quot; are routed to the controller.</span></span>

<span data-ttu-id="12d7f-133">İçinde **Çözüm Gezgini**, Global.asax Global.asax.cs arka plan kodu dosyayı açmak için çift tıklayın.</span><span class="sxs-lookup"><span data-stu-id="12d7f-133">In **Solution Explorer**, double-click Global.asax to open the code-behind file Global.asax.cs.</span></span> <span data-ttu-id="12d7f-134">Aşağıdakileri ekleyin **kullanarak** deyimi.</span><span class="sxs-lookup"><span data-stu-id="12d7f-134">Add the following **using** statement.</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample3.cs)]

<span data-ttu-id="12d7f-135">Ardından aşağıdaki kodu ekleyin **uygulama\_Başlat** yöntemi:</span><span class="sxs-lookup"><span data-stu-id="12d7f-135">Then add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample4.cs)]

<span data-ttu-id="12d7f-136">Yönlendirme tabloları hakkında daha fazla bilgi için bkz: [ASP.NET Web API'de yönlendirme](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="12d7f-136">For more information about routing tables, see [Routing in ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span></span>

## <a name="add-client-side-ajax"></a><span data-ttu-id="12d7f-137">İstemci tarafı AJAX ekleme</span><span class="sxs-lookup"><span data-stu-id="12d7f-137">Add Client-Side AJAX</span></span>

<span data-ttu-id="12d7f-138">Tüm web istemcileri erişebilir API'si oluşturmak için gereken budur.</span><span class="sxs-lookup"><span data-stu-id="12d7f-138">That's all you need to create a web API that clients can access.</span></span> <span data-ttu-id="12d7f-139">Şimdi, API'yi çağırmak için jQuery kullanan bir HTML sayfası ekleyelim.</span><span class="sxs-lookup"><span data-stu-id="12d7f-139">Now let's add an HTML page that uses jQuery to call the API.</span></span>

<span data-ttu-id="12d7f-140">Ana sayfanızın emin olun (örneğin, *Site.Master*) içeren bir `ContentPlaceHolder` ile `ID="HeadContent"`:</span><span class="sxs-lookup"><span data-stu-id="12d7f-140">Make sure your master page (for example, *Site.Master*) includes a `ContentPlaceHolder` with `ID="HeadContent"`:</span></span>

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample8.html)]

<span data-ttu-id="12d7f-141">Default.aspx dosyasını açın.</span><span class="sxs-lookup"><span data-stu-id="12d7f-141">Open the file Default.aspx.</span></span> <span data-ttu-id="12d7f-142">Ana içerik bölümü Demirbaş metni gösterildiği gibi değiştirin:</span><span class="sxs-lookup"><span data-stu-id="12d7f-142">Replace the boilerplate text that is in the main content section, as shown:</span></span>

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample5.aspx)]

<span data-ttu-id="12d7f-143">Ardından, jQuery kaynak dosyasına bir başvuru ekleyin `HeaderContent` bölümü:</span><span class="sxs-lookup"><span data-stu-id="12d7f-143">Next, add a reference to the jQuery source file in the `HeaderContent` section:</span></span>

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample6.aspx?highlight=2)]

<span data-ttu-id="12d7f-144">Not: Kolayca komut dosyası başvurusunu dosyasından bırakarak ekleyebilirsiniz **Çözüm Gezgini** Kod Düzenleyicisi penceresine.</span><span class="sxs-lookup"><span data-stu-id="12d7f-144">Note: You can easily add the script reference by dragging and dropping the file from **Solution Explorer** into the code editor window.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image4.png)

<span data-ttu-id="12d7f-145">Aşağıdaki betik bloğu jQuery komut dosyası etiketinin ekleyin:</span><span class="sxs-lookup"><span data-stu-id="12d7f-145">Below the jQuery script tag, add the following script block:</span></span>

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample7.html)]

<span data-ttu-id="12d7f-146">Belge yüklendiğinde, bu komut dosyası için bir AJAX isteği yapar &quot;API/ürünleri&quot;.</span><span class="sxs-lookup"><span data-stu-id="12d7f-146">When the document loads, this script makes an AJAX request to &quot;api/products&quot;.</span></span> <span data-ttu-id="12d7f-147">İstek JSON biçiminde ürünlerin listesini döndürür.</span><span class="sxs-lookup"><span data-stu-id="12d7f-147">The request returns a list of products in JSON format.</span></span> <span data-ttu-id="12d7f-148">Betik ürün bilgilerini HTML tabloya ekler.</span><span class="sxs-lookup"><span data-stu-id="12d7f-148">The script adds the product information to the HTML table.</span></span>

<span data-ttu-id="12d7f-149">Uygulamayı çalıştırdığınızda, aşağıdaki gibi görünmelidir:</span><span class="sxs-lookup"><span data-stu-id="12d7f-149">When you run the application, it should look like this:</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image5.png)
