---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
title: '4. Bölüm: yönetici görünümü ekleme | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 07/04/2012
ms.assetid: 792f4513-a508-4d14-a0dd-1a2fe282c7bb
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
msc.type: authoredcontent
ms.openlocfilehash: 8d9d14e5228606a81482eb08086a114ad5145e44
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37828818"
---
<a name="part-4-adding-an-admin-view"></a><span data-ttu-id="2c762-102">4. Bölüm: yönetici görünümü ekleme</span><span class="sxs-lookup"><span data-stu-id="2c762-102">Part 4: Adding an Admin View</span></span>
====================
<span data-ttu-id="2c762-103">tarafından [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="2c762-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="2c762-104">Projeyi yükle</span><span class="sxs-lookup"><span data-stu-id="2c762-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-view"></a><span data-ttu-id="2c762-105">Yönetici görünümü ekleme</span><span class="sxs-lookup"><span data-stu-id="2c762-105">Add an Admin View</span></span>

<span data-ttu-id="2c762-106">Şimdi biz için istemci tarafı açın ve yönetim denetleyicisi veri tüketebilen bir sayfa ekleyin.</span><span class="sxs-lookup"><span data-stu-id="2c762-106">Now we'll turn to the client side, and add a page that can consume data from the Admin controller.</span></span> <span data-ttu-id="2c762-107">Sayfayı oluşturmak, düzenlemek veya AJAX isteklerini denetleyiciye göndererek ürünleri, silme imkan tanıyacak.</span><span class="sxs-lookup"><span data-stu-id="2c762-107">The page will allow users to create, edit, or delete products, by sending AJAX requests to the controller.</span></span>

<span data-ttu-id="2c762-108">Çözüm Gezgini'nde denetleyicileri klasörünü genişletin ve HomeController.cs adlı dosyayı açın.</span><span class="sxs-lookup"><span data-stu-id="2c762-108">In Solution Explorer, expand the Controllers folder and open the file named HomeController.cs.</span></span> <span data-ttu-id="2c762-109">Bu dosya, MVC denetleyicisi içerir.</span><span class="sxs-lookup"><span data-stu-id="2c762-109">This file contains an MVC controller.</span></span> <span data-ttu-id="2c762-110">Adlı bir yöntem ekleyin `Admin`:</span><span class="sxs-lookup"><span data-stu-id="2c762-110">Add a method named `Admin`:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample1.cs)]

<span data-ttu-id="2c762-111">**HttpRouteUrl** yöntemi, web API'si için URI oluşturur ve bu görünüm paketini daha sonra kullanmak üzere depolarız.</span><span class="sxs-lookup"><span data-stu-id="2c762-111">The **HttpRouteUrl** method creates the URI to the web API, and we store this in the view bag for later.</span></span>

<span data-ttu-id="2c762-112">Ardından, içinde metin imleci konumlandırma `Admin` eylem yöntemi, sonra sağ tıklatın ve seçin **Görünüm Ekle**.</span><span class="sxs-lookup"><span data-stu-id="2c762-112">Next, position the text cursor within the `Admin` action method, then right-click and select **Add View**.</span></span> <span data-ttu-id="2c762-113">Bu getirir **Görünüm Ekle** iletişim.</span><span class="sxs-lookup"><span data-stu-id="2c762-113">This will bring up the **Add View** dialog.</span></span>

![](using-web-api-with-entity-framework-part-4/_static/image1.png)

<span data-ttu-id="2c762-114">İçinde **Görünüm Ekle** iletişim kutusunda, "Yönetici" Görünüm adı.</span><span class="sxs-lookup"><span data-stu-id="2c762-114">In the **Add View** dialog, name the view "Admin".</span></span> <span data-ttu-id="2c762-115">Etiketli onay kutusunu seçin **kesin türü belirtilmiş görünüm oluşturmak**.</span><span class="sxs-lookup"><span data-stu-id="2c762-115">Select the check box labeled **Create a strongly-typed view**.</span></span> <span data-ttu-id="2c762-116">Altında **Model sınıfı**, "Ürün (ProductStore.Models)" seçin.</span><span class="sxs-lookup"><span data-stu-id="2c762-116">Under **Model Class**, select "Product (ProductStore.Models)".</span></span> <span data-ttu-id="2c762-117">Diğer seçenekleri varsayılan değerlerinde bırakın.</span><span class="sxs-lookup"><span data-stu-id="2c762-117">Leave all the other options as their default values.</span></span>

![](using-web-api-with-entity-framework-part-4/_static/image2.png)

<span data-ttu-id="2c762-118">Tıklayarak **Ekle** Admin.cshtml görünümler/giriş altında adlı bir dosya ekler.</span><span class="sxs-lookup"><span data-stu-id="2c762-118">Clicking **Add** adds a file named Admin.cshtml under Views/Home.</span></span> <span data-ttu-id="2c762-119">Bu dosyayı açın ve aşağıdaki HTML'yi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="2c762-119">Open this file and add the following HTML.</span></span> <span data-ttu-id="2c762-120">Bu HTML sayfası yapısını tanımlar, ancak hiçbir işlevsellik yukarı henüz hazırlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="2c762-120">This HTML defines the structure of the page, but no functionality is wired up yet.</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample2.cshtml)]

## <a name="create-a-link-to-the-admin-page"></a><span data-ttu-id="2c762-121">Yönetici sayfasına bağlantı oluştur</span><span class="sxs-lookup"><span data-stu-id="2c762-121">Create a Link to the Admin Page</span></span>

<span data-ttu-id="2c762-122">Çözüm Gezgini'nde görünümler klasörünü genişletin ve sonra paylaşılan klasörünü genişletin.</span><span class="sxs-lookup"><span data-stu-id="2c762-122">In Solution Explorer, expand the Views folder and then expand the Shared folder.</span></span> <span data-ttu-id="2c762-123">Adlı dosyayı açın \_Layout.cshtml.</span><span class="sxs-lookup"><span data-stu-id="2c762-123">Open the file named \_Layout.cshtml.</span></span> <span data-ttu-id="2c762-124">Bulun **ul** öğesi kimliği = "menüsünde" ve yönetici görünümü için bir eylem bağlantısı:</span><span class="sxs-lookup"><span data-stu-id="2c762-124">Locate the **ul** element with id = "menu", and an action link for the Admin view:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample3.cshtml)]

> [!NOTE]
> <span data-ttu-id="2c762-125">Örnek projesinde dizenin "Logonuz buraya gelir" gibi birkaç başka yüzeysel değişiklikler, yaptım.</span><span class="sxs-lookup"><span data-stu-id="2c762-125">In the sample project, I made a few other cosmetic changes, such as replacing the string "Your logo here".</span></span> <span data-ttu-id="2c762-126">Bu, uygulamanın işlevselliğini etkilemez.</span><span class="sxs-lookup"><span data-stu-id="2c762-126">These don't affect the functionality of the application.</span></span> <span data-ttu-id="2c762-127">Projenizi indirin ve dosyayı Karşılaştır.</span><span class="sxs-lookup"><span data-stu-id="2c762-127">You can download the project and compare the files.</span></span>


<span data-ttu-id="2c762-128">Uygulamayı çalıştırmak ve giriş sayfasının üst kısmında görünür "Yönetici" bağlantısına tıklayın.</span><span class="sxs-lookup"><span data-stu-id="2c762-128">Run the application and click the "Admin" link that appears at the top of the home page.</span></span> <span data-ttu-id="2c762-129">Yönetici sayfasına aşağıdaki gibi görünmelidir:</span><span class="sxs-lookup"><span data-stu-id="2c762-129">The Admin page should look like the following:</span></span>

![](using-web-api-with-entity-framework-part-4/_static/image3.png)

<span data-ttu-id="2c762-130">Sağdaki şimdi, sayfayı hiçbir şey yapmıyor.</span><span class="sxs-lookup"><span data-stu-id="2c762-130">Right now, the page doesn't do anything.</span></span> <span data-ttu-id="2c762-131">Sonraki bölümde, bir dinamik kullanıcı Arabirimi oluşturmak için Knockout.js kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="2c762-131">In the next section, we'll use Knockout.js to create a dynamic UI.</span></span>

## <a name="add-authorization"></a><span data-ttu-id="2c762-132">Yetkilendirme Ekle</span><span class="sxs-lookup"><span data-stu-id="2c762-132">Add Authorization</span></span>

<span data-ttu-id="2c762-133">Yönetici sayfasına sitesini ziyaret eden herkes şu anda erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="2c762-133">The Admin page is currently accessible to anyone visiting the site.</span></span> <span data-ttu-id="2c762-134">Yöneticilerin kısıtlamak için bu değiştirelim.</span><span class="sxs-lookup"><span data-stu-id="2c762-134">Let's change this to restrict permission to administrators.</span></span>

<span data-ttu-id="2c762-135">Bir "Yönetici" rolü ve bir yönetici kullanıcı ekleyerek başlayın.</span><span class="sxs-lookup"><span data-stu-id="2c762-135">Start by adding an "Administrator" role and an administrator user.</span></span> <span data-ttu-id="2c762-136">Çözüm Gezgini'nde filtreleri klasörünü genişletin ve InitializeSimpleMembershipAttribute.cs adlı dosyayı açın.</span><span class="sxs-lookup"><span data-stu-id="2c762-136">In Solution Explorer, expand the Filters folder and open the file named InitializeSimpleMembershipAttribute.cs.</span></span> <span data-ttu-id="2c762-137">Bulun `SimpleMembershipInitializer` Oluşturucusu.</span><span class="sxs-lookup"><span data-stu-id="2c762-137">Locate the `SimpleMembershipInitializer` constructor.</span></span> <span data-ttu-id="2c762-138">Çağrısından sonra **WebSecurity.InitializeDatabaseConnection**, aşağıdaki kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="2c762-138">After the call to **WebSecurity.InitializeDatabaseConnection**, add the following code:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample4.cs)]

<span data-ttu-id="2c762-139">Bu, "Yönetici" rolünü ekleyin ve bir kullanıcı rolü oluşturmak için quick-and-dirty bir yoludur.</span><span class="sxs-lookup"><span data-stu-id="2c762-139">This is a quick-and-dirty way to add the "Administrator" role and create a user for the role.</span></span>

<span data-ttu-id="2c762-140">Çözüm Gezgini'nde denetleyicileri klasörünü genişletin ve HomeController.cs dosyasını açın.</span><span class="sxs-lookup"><span data-stu-id="2c762-140">In Solution Explorer, expand the Controllers folder and open the HomeController.cs file.</span></span> <span data-ttu-id="2c762-141">Ekleme **Authorize** özniteliğini `Admin` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="2c762-141">Add the **Authorize** attribute to the `Admin` method.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample5.cs)]

<span data-ttu-id="2c762-142">AdminController.cs dosyasını açın ve eklemek **Authorize** özniteliği için tüm `AdminController` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="2c762-142">Open the AdminController.cs file and add the **Authorize** attribute to the entire `AdminController` class.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample6.cs)]

> [!NOTE]
> <span data-ttu-id="2c762-143">MVC ve Web API'si hem de tanımlama **Authorize** farklı ad alanlarında öznitelikleri.</span><span class="sxs-lookup"><span data-stu-id="2c762-143">MVC and Web API both define **Authorize** attributes, in different namespaces.</span></span> <span data-ttu-id="2c762-144">MVC kullanır **System.Web.Mvc.AuthorizeAttribute**, Web API kullanırken **System.Web.Http.AuthorizeAttribute**.</span><span class="sxs-lookup"><span data-stu-id="2c762-144">MVC uses **System.Web.Mvc.AuthorizeAttribute**, while Web API uses **System.Web.Http.AuthorizeAttribute**.</span></span>


<span data-ttu-id="2c762-145">Artık yalnızca Yöneticiler, yönetici sayfasına görüntüleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2c762-145">Now only administrators can view the Admin page.</span></span> <span data-ttu-id="2c762-146">Ayrıca, yönetici denetleyiciye bir HTTP isteği gönderirseniz, istek bir kimlik doğrulama tanımlama bilgisi içermesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="2c762-146">Also, if you send an HTTP request to the Admin controller, the request must contain an authentication cookie.</span></span> <span data-ttu-id="2c762-147">Aksi durumda, sunucu, bir HTTP 401 (yetkisiz) yanıt gönderir.</span><span class="sxs-lookup"><span data-stu-id="2c762-147">If not, the server sends an HTTP 401 (Unauthorized) response.</span></span> <span data-ttu-id="2c762-148">Bir GET isteği göndererek bu Fiddler'da görebilirsiniz `http://localhost:*port*/api/admin`.</span><span class="sxs-lookup"><span data-stu-id="2c762-148">You can see this in Fiddler by sending a GET request to `http://localhost:*port*/api/admin`.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2c762-149">[Önceki](using-web-api-with-entity-framework-part-3.md)
> [İleri](using-web-api-with-entity-framework-part-5.md)</span><span class="sxs-lookup"><span data-stu-id="2c762-149">[Previous](using-web-api-with-entity-framework-part-3.md)
[Next](using-web-api-with-entity-framework-part-5.md)</span></span>
