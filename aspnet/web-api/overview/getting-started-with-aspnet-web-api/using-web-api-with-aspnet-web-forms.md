---
uid: web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
title: ASP.NET Web Forms ile Web API kullanma | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 04/03/2012
ms.assetid: 25da8c3f-4e90-4946-9765-4f160985e1e4
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
msc.type: authoredcontent
ms.openlocfilehash: c9da38d290a812e94ed9473386ba6b897447dac5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37840894"
---
<a name="using-web-api-with-aspnet-web-forms"></a><span data-ttu-id="52008-102">ASP.NET Web Forms ile Web API kullanma</span><span class="sxs-lookup"><span data-stu-id="52008-102">Using Web API with ASP.NET Web Forms</span></span>
====================
<span data-ttu-id="52008-103">tarafından [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="52008-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="52008-104">ASP.NET Web API, ASP.NET MVC ile paketlenmiştir olsa da, Web API'si için geleneksel bir ASP.NET Web Forms uygulaması eklemek kolay bir işlemdir.</span><span class="sxs-lookup"><span data-stu-id="52008-104">Although ASP.NET Web API is packaged with ASP.NET MVC, it is easy to add Web API to a traditional ASP.NET Web Forms application.</span></span> <span data-ttu-id="52008-105">Bu öğreticide, adım adım açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="52008-105">This tutorial walks you through the steps.</span></span>

## <a name="overview"></a><span data-ttu-id="52008-106">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="52008-106">Overview</span></span>

<span data-ttu-id="52008-107">Bir Web Forms uygulaması'nda Web API'sini kullanmak için iki ana adım vardır:</span><span class="sxs-lookup"><span data-stu-id="52008-107">To use Web API in a Web Forms application, there are two main steps:</span></span>

- <span data-ttu-id="52008-108">Türetilen bir Web API denetleyicisi ekleme **ApiController** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="52008-108">Add a Web API controller that derives from the **ApiController** class.</span></span>
- <span data-ttu-id="52008-109">Bir rota tablosuna ekleme **uygulama\_Başlat** yöntemi.</span><span class="sxs-lookup"><span data-stu-id="52008-109">Add a route table to the **Application\_Start** method.</span></span>

## <a name="create-a-web-forms-project"></a><span data-ttu-id="52008-110">Web Forms projesi oluşturun</span><span class="sxs-lookup"><span data-stu-id="52008-110">Create a Web Forms Project</span></span>

<span data-ttu-id="52008-111">Visual Studio'yu başlatın ve seçin **yeni proje** gelen **Başlat** sayfası.</span><span class="sxs-lookup"><span data-stu-id="52008-111">Start Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="52008-112">Veya **dosya** menüsünde **yeni** ardından **proje**.</span><span class="sxs-lookup"><span data-stu-id="52008-112">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="52008-113">İçinde **şablonları** bölmesinde **yüklü şablonlar** genişletin **Visual C#** düğümü.</span><span class="sxs-lookup"><span data-stu-id="52008-113">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="52008-114">Altında **Visual C#** seçin **Web**.</span><span class="sxs-lookup"><span data-stu-id="52008-114">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="52008-115">Proje şablonları listesinde seçin **ASP.NET Web Forms uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="52008-115">In the list of project templates, select **ASP.NET Web Forms Application**.</span></span> <span data-ttu-id="52008-116">Proje için bir ad girin ve tıklayın **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="52008-116">Enter a name for the project and click **OK**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image1.png)

## <a name="create-the-model-and-controller"></a><span data-ttu-id="52008-117">Model ve denetleyici oluşturma</span><span class="sxs-lookup"><span data-stu-id="52008-117">Create the Model and Controller</span></span>

<span data-ttu-id="52008-118">Bu öğreticide aynı model ve denetleyici sınıflar olarak [Başlarken](tutorial-your-first-web-api.md) öğretici.</span><span class="sxs-lookup"><span data-stu-id="52008-118">This tutorial uses the same model and controller classes as the [Getting Started](tutorial-your-first-web-api.md) tutorial.</span></span>

<span data-ttu-id="52008-119">İlk olarak, bir model sınıfı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="52008-119">First, add a model class.</span></span> <span data-ttu-id="52008-120">İçinde **Çözüm Gezgini**, projeye sağ tıklayıp seçin **sınıfı Ekle**.</span><span class="sxs-lookup"><span data-stu-id="52008-120">In **Solution Explorer**, right-click the project and select **Add Class**.</span></span> <span data-ttu-id="52008-121">' % S'sınıfı ürün adı ve aşağıdaki uygulama ekleyin:</span><span class="sxs-lookup"><span data-stu-id="52008-121">Name the class Product, and add the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample1.cs)]

<span data-ttu-id="52008-122">Ardından, bir Web API denetleyicisi proje, A ekleyin. *denetleyicisi* Web API'si için HTTP isteklerini işleyen nesne.</span><span class="sxs-lookup"><span data-stu-id="52008-122">Next, add a Web API controller to the project., A *controller* is the object that handles HTTP requests for Web API.</span></span>

<span data-ttu-id="52008-123">İçinde **Çözüm Gezgini**, projeye sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="52008-123">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="52008-124">Seçin **Add New Item**.</span><span class="sxs-lookup"><span data-stu-id="52008-124">Select **Add New Item**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image2.png)

<span data-ttu-id="52008-125">Altında **yüklü şablonlar**, genişletme **Visual C#** seçip **Web**.</span><span class="sxs-lookup"><span data-stu-id="52008-125">Under **Installed Templates**, expand **Visual C#** and select **Web**.</span></span> <span data-ttu-id="52008-126">Şablonlar listesinden seçip **Web API denetleyici sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="52008-126">Then, from the list of templates, select **Web API Controller Class**.</span></span> <span data-ttu-id="52008-127">Denetleyici "ProductsController" adını verin ve tıklayın **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="52008-127">Name the controller "ProductsController" and click **Add**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image3.png)

<span data-ttu-id="52008-128">**Yeni Öğe Ekle** Sihirbazı ProductsController.cs adlı bir dosya oluşturur.</span><span class="sxs-lookup"><span data-stu-id="52008-128">The **Add New Item** wizard will create a file named ProductsController.cs.</span></span> <span data-ttu-id="52008-129">Sihirbaz dahil yöntemleri silin ve aşağıdaki yöntemleri ekleyin:</span><span class="sxs-lookup"><span data-stu-id="52008-129">Delete the methods that the wizard included and add the following methods:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample2.cs)]

<span data-ttu-id="52008-130">Bu denetleyicideki kod hakkında daha fazla bilgi için bkz. [Başlarken](tutorial-your-first-web-api.md) öğretici.</span><span class="sxs-lookup"><span data-stu-id="52008-130">For more information about the code in this controller, see the [Getting Started](tutorial-your-first-web-api.md) tutorial.</span></span>

## <a name="add-routing-information"></a><span data-ttu-id="52008-131">Yönlendirme bilgileri</span><span class="sxs-lookup"><span data-stu-id="52008-131">Add Routing Information</span></span>

<span data-ttu-id="52008-132">Ardından, URI yolu şekilde ekleyeceğiz, bir URI'leri formun &quot;/API'si/ürünler/&quot; denetleyiciye yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="52008-132">Next, we'll add a URI route so that URIs of the form &quot;/api/products/&quot; are routed to the controller.</span></span>

<span data-ttu-id="52008-133">İçinde **Çözüm Gezgini**, Global.asax Global.asax.cs arka plan kod dosyasını açmak için çift tıklayın.</span><span class="sxs-lookup"><span data-stu-id="52008-133">In **Solution Explorer**, double-click Global.asax to open the code-behind file Global.asax.cs.</span></span> <span data-ttu-id="52008-134">Aşağıdaki **kullanarak** deyimi.</span><span class="sxs-lookup"><span data-stu-id="52008-134">Add the following **using** statement.</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample3.cs)]

<span data-ttu-id="52008-135">Ardından aşağıdaki kodu ekleyin **uygulama\_Başlat** yöntemi:</span><span class="sxs-lookup"><span data-stu-id="52008-135">Then add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample4.cs)]

<span data-ttu-id="52008-136">Yönlendirme tabloları hakkında daha fazla bilgi için bkz. [ASP.NET Web API'de yönlendirme](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="52008-136">For more information about routing tables, see [Routing in ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span></span>

## <a name="add-client-side-ajax"></a><span data-ttu-id="52008-137">İstemci tarafı AJAX ekleme</span><span class="sxs-lookup"><span data-stu-id="52008-137">Add Client-Side AJAX</span></span>

<span data-ttu-id="52008-138">Tüm web istemcilerin erişebileceği bir API oluşturmak için gereken budur.</span><span class="sxs-lookup"><span data-stu-id="52008-138">That's all you need to create a web API that clients can access.</span></span> <span data-ttu-id="52008-139">Şimdi, API'yi çağırmak için jQuery kullanan bir HTML sayfası ekleyelim.</span><span class="sxs-lookup"><span data-stu-id="52008-139">Now let's add an HTML page that uses jQuery to call the API.</span></span>

<span data-ttu-id="52008-140">Ana sayfanıza emin olun (örneğin, *Site.Master*) içeren bir `ContentPlaceHolder` ile `ID="HeadContent"`:</span><span class="sxs-lookup"><span data-stu-id="52008-140">Make sure your master page (for example, *Site.Master*) includes a `ContentPlaceHolder` with `ID="HeadContent"`:</span></span>

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample8.html)]

<span data-ttu-id="52008-141">Default.aspx dosyasını açın.</span><span class="sxs-lookup"><span data-stu-id="52008-141">Open the file Default.aspx.</span></span> <span data-ttu-id="52008-142">Ana içerik bölümü Demirbaş metni gösterildiği gibi değiştirin:</span><span class="sxs-lookup"><span data-stu-id="52008-142">Replace the boilerplate text that is in the main content section, as shown:</span></span>

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample5.aspx)]

<span data-ttu-id="52008-143">Ardından, jQuery kaynak dosyasına bir başvuru ekleyin `HeaderContent` bölümü:</span><span class="sxs-lookup"><span data-stu-id="52008-143">Next, add a reference to the jQuery source file in the `HeaderContent` section:</span></span>

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample6.aspx?highlight=2)]

<span data-ttu-id="52008-144">Not: Kolayca betik başvurusu dosyasından sürükleyip bırakarak ekleyebilirsiniz **Çözüm Gezgini** içine kod düzenleyicisi penceresi.</span><span class="sxs-lookup"><span data-stu-id="52008-144">Note: You can easily add the script reference by dragging and dropping the file from **Solution Explorer** into the code editor window.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image4.png)

<span data-ttu-id="52008-145">Aşağıdaki komut dosyası bloğu jQuery komut dosyası etiketi ekleyin:</span><span class="sxs-lookup"><span data-stu-id="52008-145">Below the jQuery script tag, add the following script block:</span></span>

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample7.html)]

<span data-ttu-id="52008-146">Belge yüklendiğinde bu betik bir AJAX isteğinde bulunur. &quot;API/ürünleri&quot;.</span><span class="sxs-lookup"><span data-stu-id="52008-146">When the document loads, this script makes an AJAX request to &quot;api/products&quot;.</span></span> <span data-ttu-id="52008-147">İstek, JSON biçiminde ürünlerin listesini döndürür.</span><span class="sxs-lookup"><span data-stu-id="52008-147">The request returns a list of products in JSON format.</span></span> <span data-ttu-id="52008-148">Betik, ürün bilgisi HTML tablosuna ekler.</span><span class="sxs-lookup"><span data-stu-id="52008-148">The script adds the product information to the HTML table.</span></span>

<span data-ttu-id="52008-149">Uygulamayı çalıştırdığınızda, şu şekilde görünmelidir:</span><span class="sxs-lookup"><span data-stu-id="52008-149">When you run the application, it should look like this:</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image5.png)
