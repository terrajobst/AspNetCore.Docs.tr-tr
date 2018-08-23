---
uid: web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
title: ASP.NET Web Forms ile Web API kullanma | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 04/03/2012
ms.assetid: 25da8c3f-4e90-4946-9765-4f160985e1e4
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
msc.type: authoredcontent
ms.openlocfilehash: a14bf0abd8c5d603cf3859891f855415cf3df9f3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754447"
---
<a name="using-web-api-with-aspnet-web-forms"></a><span data-ttu-id="61a49-102">ASP.NET Web Forms ile Web API kullanma</span><span class="sxs-lookup"><span data-stu-id="61a49-102">Using Web API with ASP.NET Web Forms</span></span>
====================
<span data-ttu-id="61a49-103">tarafından [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="61a49-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="61a49-104">ASP.NET Web API, ASP.NET MVC ile paketlenmiştir olsa da, Web API'si için geleneksel bir ASP.NET Web Forms uygulaması eklemek kolay bir işlemdir.</span><span class="sxs-lookup"><span data-stu-id="61a49-104">Although ASP.NET Web API is packaged with ASP.NET MVC, it is easy to add Web API to a traditional ASP.NET Web Forms application.</span></span> <span data-ttu-id="61a49-105">Bu öğreticide, adım adım açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="61a49-105">This tutorial walks you through the steps.</span></span>

## <a name="overview"></a><span data-ttu-id="61a49-106">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="61a49-106">Overview</span></span>

<span data-ttu-id="61a49-107">Bir Web Forms uygulaması'nda Web API'sini kullanmak için iki ana adım vardır:</span><span class="sxs-lookup"><span data-stu-id="61a49-107">To use Web API in a Web Forms application, there are two main steps:</span></span>

- <span data-ttu-id="61a49-108">Türetilen bir Web API denetleyicisi ekleme **ApiController** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="61a49-108">Add a Web API controller that derives from the **ApiController** class.</span></span>
- <span data-ttu-id="61a49-109">Bir rota tablosuna ekleme **uygulama\_Başlat** yöntemi.</span><span class="sxs-lookup"><span data-stu-id="61a49-109">Add a route table to the **Application\_Start** method.</span></span>

## <a name="create-a-web-forms-project"></a><span data-ttu-id="61a49-110">Web Forms projesi oluşturun</span><span class="sxs-lookup"><span data-stu-id="61a49-110">Create a Web Forms Project</span></span>

<span data-ttu-id="61a49-111">Visual Studio'yu başlatın ve seçin **yeni proje** gelen **Başlat** sayfası.</span><span class="sxs-lookup"><span data-stu-id="61a49-111">Start Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="61a49-112">Veya **dosya** menüsünde **yeni** ardından **proje**.</span><span class="sxs-lookup"><span data-stu-id="61a49-112">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="61a49-113">İçinde **şablonları** bölmesinde **yüklü şablonlar** genişletin **Visual C#** düğümü.</span><span class="sxs-lookup"><span data-stu-id="61a49-113">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="61a49-114">Altında **Visual C#** seçin **Web**.</span><span class="sxs-lookup"><span data-stu-id="61a49-114">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="61a49-115">Proje şablonları listesinde seçin **ASP.NET Web Forms uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="61a49-115">In the list of project templates, select **ASP.NET Web Forms Application**.</span></span> <span data-ttu-id="61a49-116">Proje için bir ad girin ve tıklayın **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="61a49-116">Enter a name for the project and click **OK**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image1.png)

## <a name="create-the-model-and-controller"></a><span data-ttu-id="61a49-117">Model ve denetleyici oluşturma</span><span class="sxs-lookup"><span data-stu-id="61a49-117">Create the Model and Controller</span></span>

<span data-ttu-id="61a49-118">Bu öğreticide aynı model ve denetleyici sınıflar olarak [Başlarken](tutorial-your-first-web-api.md) öğretici.</span><span class="sxs-lookup"><span data-stu-id="61a49-118">This tutorial uses the same model and controller classes as the [Getting Started](tutorial-your-first-web-api.md) tutorial.</span></span>

<span data-ttu-id="61a49-119">İlk olarak, bir model sınıfı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="61a49-119">First, add a model class.</span></span> <span data-ttu-id="61a49-120">İçinde **Çözüm Gezgini**, projeye sağ tıklayıp seçin **sınıfı Ekle**.</span><span class="sxs-lookup"><span data-stu-id="61a49-120">In **Solution Explorer**, right-click the project and select **Add Class**.</span></span> <span data-ttu-id="61a49-121">' % S'sınıfı ürün adı ve aşağıdaki uygulama ekleyin:</span><span class="sxs-lookup"><span data-stu-id="61a49-121">Name the class Product, and add the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample1.cs)]

<span data-ttu-id="61a49-122">Ardından, bir Web API denetleyicisi proje, A ekleyin. *denetleyicisi* Web API'si için HTTP isteklerini işleyen nesne.</span><span class="sxs-lookup"><span data-stu-id="61a49-122">Next, add a Web API controller to the project., A *controller* is the object that handles HTTP requests for Web API.</span></span>

<span data-ttu-id="61a49-123">İçinde **Çözüm Gezgini**, projeye sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="61a49-123">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="61a49-124">Seçin **Add New Item**.</span><span class="sxs-lookup"><span data-stu-id="61a49-124">Select **Add New Item**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image2.png)

<span data-ttu-id="61a49-125">Altında **yüklü şablonlar**, genişletme **Visual C#** seçip **Web**.</span><span class="sxs-lookup"><span data-stu-id="61a49-125">Under **Installed Templates**, expand **Visual C#** and select **Web**.</span></span> <span data-ttu-id="61a49-126">Şablonlar listesinden seçip **Web API denetleyici sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="61a49-126">Then, from the list of templates, select **Web API Controller Class**.</span></span> <span data-ttu-id="61a49-127">Denetleyici "ProductsController" adını verin ve tıklayın **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="61a49-127">Name the controller "ProductsController" and click **Add**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image3.png)

<span data-ttu-id="61a49-128">**Yeni Öğe Ekle** Sihirbazı ProductsController.cs adlı bir dosya oluşturur.</span><span class="sxs-lookup"><span data-stu-id="61a49-128">The **Add New Item** wizard will create a file named ProductsController.cs.</span></span> <span data-ttu-id="61a49-129">Sihirbaz dahil yöntemleri silin ve aşağıdaki yöntemleri ekleyin:</span><span class="sxs-lookup"><span data-stu-id="61a49-129">Delete the methods that the wizard included and add the following methods:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample2.cs)]

<span data-ttu-id="61a49-130">Bu denetleyicideki kod hakkında daha fazla bilgi için bkz. [Başlarken](tutorial-your-first-web-api.md) öğretici.</span><span class="sxs-lookup"><span data-stu-id="61a49-130">For more information about the code in this controller, see the [Getting Started](tutorial-your-first-web-api.md) tutorial.</span></span>

## <a name="add-routing-information"></a><span data-ttu-id="61a49-131">Yönlendirme bilgileri</span><span class="sxs-lookup"><span data-stu-id="61a49-131">Add Routing Information</span></span>

<span data-ttu-id="61a49-132">Ardından, URI yolu şekilde ekleyeceğiz, bir URI'leri formun &quot;/API'si/ürünler/&quot; denetleyiciye yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="61a49-132">Next, we'll add a URI route so that URIs of the form &quot;/api/products/&quot; are routed to the controller.</span></span>

<span data-ttu-id="61a49-133">İçinde **Çözüm Gezgini**, Global.asax Global.asax.cs arka plan kod dosyasını açmak için çift tıklayın.</span><span class="sxs-lookup"><span data-stu-id="61a49-133">In **Solution Explorer**, double-click Global.asax to open the code-behind file Global.asax.cs.</span></span> <span data-ttu-id="61a49-134">Aşağıdaki **kullanarak** deyimi.</span><span class="sxs-lookup"><span data-stu-id="61a49-134">Add the following **using** statement.</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample3.cs)]

<span data-ttu-id="61a49-135">Ardından aşağıdaki kodu ekleyin **uygulama\_Başlat** yöntemi:</span><span class="sxs-lookup"><span data-stu-id="61a49-135">Then add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample4.cs)]

<span data-ttu-id="61a49-136">Yönlendirme tabloları hakkında daha fazla bilgi için bkz. [ASP.NET Web API'de yönlendirme](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="61a49-136">For more information about routing tables, see [Routing in ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span></span>

## <a name="add-client-side-ajax"></a><span data-ttu-id="61a49-137">İstemci tarafı AJAX ekleme</span><span class="sxs-lookup"><span data-stu-id="61a49-137">Add Client-Side AJAX</span></span>

<span data-ttu-id="61a49-138">Tüm web istemcilerin erişebileceği bir API oluşturmak için gereken budur.</span><span class="sxs-lookup"><span data-stu-id="61a49-138">That's all you need to create a web API that clients can access.</span></span> <span data-ttu-id="61a49-139">Şimdi, API'yi çağırmak için jQuery kullanan bir HTML sayfası ekleyelim.</span><span class="sxs-lookup"><span data-stu-id="61a49-139">Now let's add an HTML page that uses jQuery to call the API.</span></span>

<span data-ttu-id="61a49-140">Ana sayfanıza emin olun (örneğin, *Site.Master*) içeren bir `ContentPlaceHolder` ile `ID="HeadContent"`:</span><span class="sxs-lookup"><span data-stu-id="61a49-140">Make sure your master page (for example, *Site.Master*) includes a `ContentPlaceHolder` with `ID="HeadContent"`:</span></span>

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample8.html)]

<span data-ttu-id="61a49-141">Default.aspx dosyasını açın.</span><span class="sxs-lookup"><span data-stu-id="61a49-141">Open the file Default.aspx.</span></span> <span data-ttu-id="61a49-142">Ana içerik bölümü Demirbaş metni gösterildiği gibi değiştirin:</span><span class="sxs-lookup"><span data-stu-id="61a49-142">Replace the boilerplate text that is in the main content section, as shown:</span></span>

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample5.aspx)]

<span data-ttu-id="61a49-143">Ardından, jQuery kaynak dosyasına bir başvuru ekleyin `HeaderContent` bölümü:</span><span class="sxs-lookup"><span data-stu-id="61a49-143">Next, add a reference to the jQuery source file in the `HeaderContent` section:</span></span>

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample6.aspx?highlight=2)]

<span data-ttu-id="61a49-144">Not: Kolayca betik başvurusu dosyasından sürükleyip bırakarak ekleyebilirsiniz **Çözüm Gezgini** içine kod düzenleyicisi penceresi.</span><span class="sxs-lookup"><span data-stu-id="61a49-144">Note: You can easily add the script reference by dragging and dropping the file from **Solution Explorer** into the code editor window.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image4.png)

<span data-ttu-id="61a49-145">Aşağıdaki komut dosyası bloğu jQuery komut dosyası etiketi ekleyin:</span><span class="sxs-lookup"><span data-stu-id="61a49-145">Below the jQuery script tag, add the following script block:</span></span>

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample7.html)]

<span data-ttu-id="61a49-146">Belge yüklendiğinde bu betik bir AJAX isteğinde bulunur. &quot;API/ürünleri&quot;.</span><span class="sxs-lookup"><span data-stu-id="61a49-146">When the document loads, this script makes an AJAX request to &quot;api/products&quot;.</span></span> <span data-ttu-id="61a49-147">İstek, JSON biçiminde ürünlerin listesini döndürür.</span><span class="sxs-lookup"><span data-stu-id="61a49-147">The request returns a list of products in JSON format.</span></span> <span data-ttu-id="61a49-148">Betik, ürün bilgisi HTML tablosuna ekler.</span><span class="sxs-lookup"><span data-stu-id="61a49-148">The script adds the product information to the HTML table.</span></span>

<span data-ttu-id="61a49-149">Uygulamayı çalıştırdığınızda, şu şekilde görünmelidir:</span><span class="sxs-lookup"><span data-stu-id="61a49-149">When you run the application, it should look like this:</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image5.png)
