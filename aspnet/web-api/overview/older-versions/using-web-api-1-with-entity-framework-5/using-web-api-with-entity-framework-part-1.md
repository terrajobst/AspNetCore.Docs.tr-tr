---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
title: '1. Kısım: Genel bakış ve proje oluşturma | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/03/2012
ms.topic: article
ms.assetid: 94421d86-68c4-4471-bf5f-82d654a17252
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
msc.type: authoredcontent
ms.openlocfilehash: f9cdff0cb0cad9adad546c8f8d46ba9b010e1079
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873332"
---
<a name="part-1-overview-and-creating-the-project"></a><span data-ttu-id="7ca6e-102">1. Kısım: Genel bakış ve projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="7ca6e-102">Part 1: Overview and Creating the Project</span></span>
====================
<span data-ttu-id="7ca6e-103">tarafından [CAN Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="7ca6e-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="7ca6e-104">Tamamlanan projenizi indirin</span><span class="sxs-lookup"><span data-stu-id="7ca6e-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

<span data-ttu-id="7ca6e-105">Entity Framework bir nesne/ilişkisel bir eşleme çerçevedir.</span><span class="sxs-lookup"><span data-stu-id="7ca6e-105">Entity Framework is an object/relational mapping framework.</span></span> <span data-ttu-id="7ca6e-106">İlişkisel bir veritabanındaki varlıklar için etki alanı nesnelerini kodunuzda eşler.</span><span class="sxs-lookup"><span data-stu-id="7ca6e-106">It maps the domain objects in your code to entities in a relational database.</span></span> <span data-ttu-id="7ca6e-107">Çoğunlukla, Entity Framework bunu sizin için mvc'deki olduğundan veritabanı katmanı hakkında endişelenmeniz gerekmez.</span><span class="sxs-lookup"><span data-stu-id="7ca6e-107">For the most part, you do not have to worry about the database layer, because Entity Framework takes care of it for you.</span></span> <span data-ttu-id="7ca6e-108">Kodunuzu nesneleri yönetir ve değişiklikler veritabanına kalıcı.</span><span class="sxs-lookup"><span data-stu-id="7ca6e-108">Your code manipulates the objects, and changes are persisted to a database.</span></span>

## <a name="about-the-tutorial"></a><span data-ttu-id="7ca6e-109">Öğretici hakkında</span><span class="sxs-lookup"><span data-stu-id="7ca6e-109">About the Tutorial</span></span>

<span data-ttu-id="7ca6e-110">Bu öğreticide, bir basit depolama uygulaması oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="7ca6e-110">In this tutorial, you will create a simple store application.</span></span> <span data-ttu-id="7ca6e-111">Uygulama için iki ana bölümü vardır.</span><span class="sxs-lookup"><span data-stu-id="7ca6e-111">There are two main parts to the application.</span></span> <span data-ttu-id="7ca6e-112">Normal kullanıcılara ürünleri görüntülemek ve siparişleri oluşturun:</span><span class="sxs-lookup"><span data-stu-id="7ca6e-112">Normal users can view products and create orders:</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image1.png)

<span data-ttu-id="7ca6e-113">Yöneticiler oluşturamaz, silemez veya ürünleri düzenleyin:</span><span class="sxs-lookup"><span data-stu-id="7ca6e-113">Administrators can create, delete, or edit products:</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image2.png)

## <a name="skills-youll-learn"></a><span data-ttu-id="7ca6e-114">Bilgi edineceksiniz yetenekleri</span><span class="sxs-lookup"><span data-stu-id="7ca6e-114">Skills You'll Learn</span></span>

<span data-ttu-id="7ca6e-115">Öğrenecekleriniz aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="7ca6e-115">Here's what you'll learn:</span></span>

- <span data-ttu-id="7ca6e-116">Entity Framework ile ASP.NET Web API kullanma</span><span class="sxs-lookup"><span data-stu-id="7ca6e-116">How to use Entity Framework with ASP.NET Web API.</span></span>
- <span data-ttu-id="7ca6e-117">Knockout.js dinamik istemci kullanıcı Arabirimi oluşturmak için nasıl kullanılacağını.</span><span class="sxs-lookup"><span data-stu-id="7ca6e-117">How to use knockout.js to create a dynamic client UI.</span></span>
- <span data-ttu-id="7ca6e-118">Form kimlik doğrulaması Web API ile kullanıcıların kimliklerini doğrulamak için nasıl kullanılacağını.</span><span class="sxs-lookup"><span data-stu-id="7ca6e-118">How to use forms authentication with Web API to authenticate users.</span></span>

<span data-ttu-id="7ca6e-119">Bu öğretici müstakil olsa da, aşağıdaki öğreticiler ilk okumak isteyebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="7ca6e-119">Although this tutorial is self-contained, you might want to read the following tutorials first:</span></span>

- [<span data-ttu-id="7ca6e-120">İlk ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="7ca6e-120">Your First ASP.NET Web API</span></span>](../../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)
- [<span data-ttu-id="7ca6e-121">CRUD işlemleri destekleyen bir Web API oluşturma</span><span class="sxs-lookup"><span data-stu-id="7ca6e-121">Creating a Web API that Supports CRUD Operations</span></span>](../creating-a-web-api-that-supports-crud-operations.md)

<span data-ttu-id="7ca6e-122">Bazı bilgisine [ASP.NET MVC](../../../../mvc/index.md) de yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="7ca6e-122">Some knowledge of [ASP.NET MVC](../../../../mvc/index.md) is also helpful.</span></span>

## <a name="overview"></a><span data-ttu-id="7ca6e-123">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="7ca6e-123">Overview</span></span>

<span data-ttu-id="7ca6e-124">Yüksek bir düzeyde uygulama mimarisi şöyledir:</span><span class="sxs-lookup"><span data-stu-id="7ca6e-124">At a high level, here is the architecture of the application:</span></span>

- <span data-ttu-id="7ca6e-125">ASP.NET MVC için istemci HTML sayfaları oluşturur.</span><span class="sxs-lookup"><span data-stu-id="7ca6e-125">ASP.NET MVC generates the HTML pages for the client.</span></span>
- <span data-ttu-id="7ca6e-126">ASP.NET Web API CRUD işlemleri verileri (ürünleri ve siparişleri) kullanıma sunar.</span><span class="sxs-lookup"><span data-stu-id="7ca6e-126">ASP.NET Web API exposes CRUD operations on the data (products and orders).</span></span>
- <span data-ttu-id="7ca6e-127">Entity Framework veritabanı varlıklarının içinde Web API tarafından kullanılan C# modelleri çevirir.</span><span class="sxs-lookup"><span data-stu-id="7ca6e-127">Entity Framework translates the C# models used by Web API into database entities.</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image3.png)

<span data-ttu-id="7ca6e-128">Etki alanı nesnelerini uygulamasının çeşitli katmanına nasıl temsil edildiğini Aşağıdaki diyagramda gösterilmiştir: veritabanı katmanı, nesne modeli ve son olarak istemci HTTP üzerinden veri iletmek için kullanılan gönderme biçimini.</span><span class="sxs-lookup"><span data-stu-id="7ca6e-128">The following diagram shows how the domain objects are represented at various layers of the application: The database layer, the object model, and finally the wire format, which is used to transmit data to the client via HTTP.</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image4.png)

## <a name="create-the-visual-studio-project"></a><span data-ttu-id="7ca6e-129">Visual Studio projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="7ca6e-129">Create the Visual Studio Project</span></span>

<span data-ttu-id="7ca6e-130">Visual Web Developer Express veya Visual Studio'nın tam sürümünü kullanarak Eğitmen proje oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7ca6e-130">You can create the tutorial project using either Visual Web Developer Express or the full version of Visual Studio.</span></span>

<span data-ttu-id="7ca6e-131">Gelen **Başlat** sayfasında, **yeni proje**.</span><span class="sxs-lookup"><span data-stu-id="7ca6e-131">From the **Start** page, click **New Project**.</span></span>

<span data-ttu-id="7ca6e-132">İçinde **şablonları** bölmesinde, **yüklü şablonlar** ve genişletin **Visual C#** düğümü.</span><span class="sxs-lookup"><span data-stu-id="7ca6e-132">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="7ca6e-133">Altında **Visual C#** seçin **Web**.</span><span class="sxs-lookup"><span data-stu-id="7ca6e-133">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="7ca6e-134">Proje şablonları listesinde seçin **ASP.NET MVC 4 Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="7ca6e-134">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="7ca6e-135">"ProductStore" adını verin ve projeyi tıklatın **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="7ca6e-135">Name the project "ProductStore" and click **OK**.</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image5.png)

<span data-ttu-id="7ca6e-136">İçinde **yeni ASP.NET MVC 4 proje** iletişim kutusunda **Internet uygulama** tıklatıp **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="7ca6e-136">In the **New ASP.NET MVC 4 Project** dialog, select **Internet Application** and click **OK**.</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image6.png)

<span data-ttu-id="7ca6e-137">"Internet uygulama" şablonu form kimlik doğrulamasını destekleyen bir ASP.NET MVC uygulaması oluşturur.</span><span class="sxs-lookup"><span data-stu-id="7ca6e-137">The "Internet Application" template creates an ASP.NET MVC application that supports forms authentication.</span></span> <span data-ttu-id="7ca6e-138">Uygulamayı şimdi çalıştırırsanız, zaten bazı özelliklere sahiptir:</span><span class="sxs-lookup"><span data-stu-id="7ca6e-138">If you run the application now, it already has some features:</span></span>

- <span data-ttu-id="7ca6e-139">Yeni kullanıcılar sağ üst köşesinde yer alan "Register" bağlantısını tıklatarak kaydedebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7ca6e-139">New users can register by clicking the "Register" link in the upper right corner.</span></span>
- <span data-ttu-id="7ca6e-140">Kayıtlı kullanıcılar "Oturum" bağlantısını tıklatarak oturum açabilir.</span><span class="sxs-lookup"><span data-stu-id="7ca6e-140">Registered users can log in by clicking the "Log in" link.</span></span>

<span data-ttu-id="7ca6e-141">Üyelik bilgilerini otomatik olarak oluşturulan bir veritabanında kalıcı hale getirilir.</span><span class="sxs-lookup"><span data-stu-id="7ca6e-141">Membership information is persisted in a database that gets created automatically.</span></span> <span data-ttu-id="7ca6e-142">ASP.NET MVC form kimlik doğrulaması hakkında daha fazla bilgi için bkz: [izlenecek yol: ASP.NET MVC, form kimlik doğrulaması kullanarak](https://msdn.microsoft.com/library/ff398049(VS.98).aspx).</span><span class="sxs-lookup"><span data-stu-id="7ca6e-142">For more information about forms authentication in ASP.NET MVC, see [Walkthrough: Using Forms Authentication in ASP.NET MVC](https://msdn.microsoft.com/library/ff398049(VS.98).aspx).</span></span>

## <a name="update-the-css-file"></a><span data-ttu-id="7ca6e-143">CSS dosyası güncelleştir</span><span class="sxs-lookup"><span data-stu-id="7ca6e-143">Update the CSS File</span></span>

<span data-ttu-id="7ca6e-144">Bu adım yüzeysel bağlıdır, ancak önceki ekran görüntüleri gibi işleme sayfaları hale getirir.</span><span class="sxs-lookup"><span data-stu-id="7ca6e-144">This step is cosmetic, but it will make the pages render like the earlier screen shots.</span></span>

<span data-ttu-id="7ca6e-145">Çözüm Gezgini'nde, içerik klasörünü genişletin ve Site.css adlı dosyayı açın.</span><span class="sxs-lookup"><span data-stu-id="7ca6e-145">In Solution Explorer, expand the Content folder and open the file named Site.css.</span></span> <span data-ttu-id="7ca6e-146">Aşağıdaki CSS stil ekleyin:</span><span class="sxs-lookup"><span data-stu-id="7ca6e-146">Add the following CSS styles:</span></span>

[!code-css[Main](using-web-api-with-entity-framework-part-1/samples/sample1.css)]

> [!div class="step-by-step"]
> [<span data-ttu-id="7ca6e-147">Next</span><span class="sxs-lookup"><span data-stu-id="7ca6e-147">Next</span></span>](using-web-api-with-entity-framework-part-2.md)
