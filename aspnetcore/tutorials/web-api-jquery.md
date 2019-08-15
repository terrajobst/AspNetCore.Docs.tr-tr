---
title: "Öğretici: ASP.NET Core kullanarak jQuery ile Web API 'SI çağırma"
author: rick-anderson
description: JQuery ile ASP.NET Core Web API 'sinin nasıl çağrılacağını öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 07/20/2019
uid: tutorials/web-api-jquery
ms.openlocfilehash: a319e4b4ce09e9b09afeaff065d5740276deb115
ms.sourcegitcommit: 476ea5ad86a680b7b017c6f32098acd3414c0f6c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/14/2019
ms.locfileid: "69022554"
---
# <a name="tutorial-call-an-aspnet-core-web-api-with-jquery"></a><span data-ttu-id="659bc-103">Öğretici: JQuery ile ASP.NET Core Web API 'SI çağırma</span><span class="sxs-lookup"><span data-stu-id="659bc-103">Tutorial: Call an ASP.NET Core web API with jQuery</span></span>

<span data-ttu-id="659bc-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="659bc-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="659bc-105">Bu öğreticide, jQuery ile ASP.NET Core Web API 'sinin nasıl çağrılacağını gösterilmektedir</span><span class="sxs-lookup"><span data-stu-id="659bc-105">This tutorial shows how to call an ASP.NET Core web API with jQuery</span></span>

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="659bc-106">ASP.NET Core 2,2 için, [Web API 'Sini jQuery Ile çağırma](xref:tutorials/first-web-api#call-the-api-with-jquery)2,2 sürümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="659bc-106">For ASP.NET Core 2.2, see the 2.2 version of [Call the Web API with jQuery](xref:tutorials/first-web-api#call-the-api-with-jquery).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

## <a name="prerequisites"></a><span data-ttu-id="659bc-107">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="659bc-107">Prerequisites</span></span>

* <span data-ttu-id="659bc-108">Tüm [öğreticiyi: Web API 'SI oluşturma](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="659bc-108">Complete [Tutorial: Create a web API](xref:tutorials/first-web-api)</span></span>
* <span data-ttu-id="659bc-109">CSS, HTML, JavaScript ve jQuery ile benzerlik</span><span class="sxs-lookup"><span data-stu-id="659bc-109">Familiarity with CSS, HTML, JavaScript, and jQuery</span></span>

## <a name="call-the-api-with-jquery"></a><span data-ttu-id="659bc-110">JQuery ile API çağırma</span><span class="sxs-lookup"><span data-stu-id="659bc-110">Call the API with jQuery</span></span>

<span data-ttu-id="659bc-111">Bu bölümde, Web'i çağırmaya jQuery kullanan bir HTML sayfasına eklenen API.</span><span class="sxs-lookup"><span data-stu-id="659bc-111">In this section, an HTML page is added that uses jQuery to call the web api.</span></span> <span data-ttu-id="659bc-112">jQuery isteği başlatır ve API'nin yanıt Ayrıntıları sayfası güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="659bc-112">jQuery initiates the request and updates the page with the details from the API's response.</span></span>

<span data-ttu-id="659bc-113">Uygulamayı [statik dosyalara sunacak](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) şekilde yapılandırın ve aşağıdaki vurgulanmış kodla *Startup.cs* güncelleştirerek [varsayılan dosya eşlemesini etkinleştirin](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) :</span><span class="sxs-lookup"><span data-stu-id="659bc-113">Configure the app to [serve static files](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [enable default file mapping](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) by updating *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/StartupJquery.cs?highlight=8-9&name=snippet_configure)]

<span data-ttu-id="659bc-114">Oluşturma bir *wwwroot* proje dizininde klasör.</span><span class="sxs-lookup"><span data-stu-id="659bc-114">Create a *wwwroot* folder in the project directory.</span></span>

<span data-ttu-id="659bc-115">Adlı bir HTML dosyası ekleyin *index.html* için *wwwroot* dizin.</span><span class="sxs-lookup"><span data-stu-id="659bc-115">Add an HTML file named *index.html* to the *wwwroot* directory.</span></span> <span data-ttu-id="659bc-116">Dosyanın içeriğini aşağıdaki biçimlendirme ile değiştirin:</span><span class="sxs-lookup"><span data-stu-id="659bc-116">Replace its contents with the following markup:</span></span>

[!code-html[](first-web-api/samples/3.0/TodoApi/wwwroot/index.html)]

<span data-ttu-id="659bc-117">Adlı bir JavaScript dosyası ekleyin *site.js* için *wwwroot* dizin.</span><span class="sxs-lookup"><span data-stu-id="659bc-117">Add a JavaScript file named *site.js* to the *wwwroot* directory.</span></span> <span data-ttu-id="659bc-118">Dosyanın içeriğini aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="659bc-118">Replace its contents with the following code:</span></span>

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

<span data-ttu-id="659bc-119">ASP.NET Core proje başlatma ayarlarında bir değişiklik HTML sayfasını yerel olarak test etmek için gerekli:</span><span class="sxs-lookup"><span data-stu-id="659bc-119">A change to the ASP.NET Core project's launch settings may be required to test the HTML page locally:</span></span>

* <span data-ttu-id="659bc-120">Açık *Properties\launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="659bc-120">Open *Properties\launchSettings.json*.</span></span>
* <span data-ttu-id="659bc-121">Kaldırma `launchUrl` , açmak için uygulamayı zorlamak için özellik *index.html*&mdash;projenin varsayılan dosya.</span><span class="sxs-lookup"><span data-stu-id="659bc-121">Remove the `launchUrl` property to force the app to open at *index.html*&mdash;the project's default file.</span></span>

<span data-ttu-id="659bc-122">JQuery almak için birkaç yolu vardır.</span><span class="sxs-lookup"><span data-stu-id="659bc-122">There are several ways to get jQuery.</span></span> <span data-ttu-id="659bc-123">Yukarıdaki kod parçacığında, bir CDN kitaplığı yüklenir.</span><span class="sxs-lookup"><span data-stu-id="659bc-123">In the preceding snippet, the library is loaded from a CDN.</span></span>

<span data-ttu-id="659bc-124">Bu örnek, tüm API CRUD yöntemleri çağırır.</span><span class="sxs-lookup"><span data-stu-id="659bc-124">This sample calls all of the CRUD methods of the API.</span></span> <span data-ttu-id="659bc-125">API çağrıları açıklamaları aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="659bc-125">Following are explanations of the calls to the API.</span></span>

### <a name="get-a-list-of-to-do-items"></a><span data-ttu-id="659bc-126">Yapılacaklar öğelerinin bir listesini alın</span><span class="sxs-lookup"><span data-stu-id="659bc-126">Get a list of to-do items</span></span>

<span data-ttu-id="659bc-127">JQuery [ajax](https://api.jquery.com/jquery.ajax/) işlev gönderen bir `GET` Yapılacaklar öğelerini dizisini temsil eden JSON döndüren API isteği.</span><span class="sxs-lookup"><span data-stu-id="659bc-127">The jQuery [ajax](https://api.jquery.com/jquery.ajax/) function sends a `GET` request to the API, which returns JSON representing an array of to-do items.</span></span> <span data-ttu-id="659bc-128">`success` İstek başarılı olursa geri çağırma işlevi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="659bc-128">The `success` callback function is invoked if the request succeeds.</span></span> <span data-ttu-id="659bc-129">Geri çağırma içinde DOM Yapılacaklar bilgilerle güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="659bc-129">In the callback, the DOM is updated with the to-do information.</span></span>

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a><span data-ttu-id="659bc-130">Yapılacak İş Öğesi Ekle</span><span class="sxs-lookup"><span data-stu-id="659bc-130">Add a to-do item</span></span>

<span data-ttu-id="659bc-131">[Ajax](https://api.jquery.com/jquery.ajax/) işlev gönderen bir `POST` istek ile istek gövdesindeki yapılacak iş öğesi.</span><span class="sxs-lookup"><span data-stu-id="659bc-131">The [ajax](https://api.jquery.com/jquery.ajax/) function sends a `POST` request with the to-do item in the request body.</span></span> <span data-ttu-id="659bc-132">`accepts` Ve `contentType` seçeneklerini ayarlamak `application/json` gönderilen ve alınan medya türü belirtmek için.</span><span class="sxs-lookup"><span data-stu-id="659bc-132">The `accepts` and `contentType` options are set to `application/json` to specify the media type being received and sent.</span></span> <span data-ttu-id="659bc-133">Yapılacak iş öğesi kullanarak JSON'a dönüştürülür [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span><span class="sxs-lookup"><span data-stu-id="659bc-133">The to-do item is converted to JSON by using [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span></span> <span data-ttu-id="659bc-134">API'yi bir başarılı durum kodu döndürdüğünde `getData` işlevi HTML tablosu güncelleştirmek için çağrılır.</span><span class="sxs-lookup"><span data-stu-id="659bc-134">When the API returns a successful status code, the `getData` function is invoked to update the HTML table.</span></span>

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a><span data-ttu-id="659bc-135">Yapılacak iş öğesini güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="659bc-135">Update a to-do item</span></span>

<span data-ttu-id="659bc-136">Yapılacak iş öğesi güncelleştirilirken bir eklemeye benzerdir.</span><span class="sxs-lookup"><span data-stu-id="659bc-136">Updating a to-do item is similar to adding one.</span></span> <span data-ttu-id="659bc-137">`url` Öğenin benzersiz tanıtıcısı eklemek için değişiklikleri ve `type` olduğu `PUT`.</span><span class="sxs-lookup"><span data-stu-id="659bc-137">The `url` changes to add the unique identifier of the item, and the `type` is `PUT`.</span></span>

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a><span data-ttu-id="659bc-138">Yapılacak iş öğesi silme</span><span class="sxs-lookup"><span data-stu-id="659bc-138">Delete a to-do item</span></span>

<span data-ttu-id="659bc-139">Yapılacak iş öğesi silme gerçekleştirilir ayarlayarak `type` AJAX çağrısı hedefi üzerinde `DELETE` ve öğenin benzersiz tanıtıcısı URL'yi belirterek.</span><span class="sxs-lookup"><span data-stu-id="659bc-139">Deleting a to-do item is accomplished by setting the `type` on the AJAX call to `DELETE` and specifying the item's unique identifier in the URL.</span></span>

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]

<span data-ttu-id="659bc-140">API Yardım sayfaları oluşturma hakkında bilgi edinmek için sonraki öğreticiye ilerleyin:</span><span class="sxs-lookup"><span data-stu-id="659bc-140">Advance to the next tutorial to learn how to generate API help pages:</span></span>

> [!div class="nextstepaction"]
> <xref:tutorials/get-started-with-swashbuckle>
::: moniker-end
