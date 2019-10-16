---
title: "Öğretici: JavaScript ile ASP.NET Core Web API 'SI çağırma"
author: rick-anderson
description: JavaScript ile ASP.NET Core Web API 'sini çağırmayı öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 10/15/2019
uid: tutorials/web-api-javascript
ms.openlocfilehash: bbe261307f6f68af002cb98cc4895888ade7f61c
ms.sourcegitcommit: dd026eceee79e943bd6b4a37b144803b50617583
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72378698"
---
# <a name="tutorial-call-an-aspnet-core-web-api-with-javascript"></a><span data-ttu-id="e369e-103">Öğretici: JavaScript ile ASP.NET Core Web API 'SI çağırma</span><span class="sxs-lookup"><span data-stu-id="e369e-103">Tutorial: Call an ASP.NET Core web API with JavaScript</span></span>

<span data-ttu-id="e369e-104">[Rick Anderson](https://twitter.com/RickAndMSFT) tarafından</span><span class="sxs-lookup"><span data-stu-id="e369e-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="e369e-105">Bu öğreticide, [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API)kullanılarak JavaScript ile ASP.NET Core Web API 'sinin nasıl çağrılacağını gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="e369e-105">This tutorial shows how to call an ASP.NET Core web API with JavaScript, using the [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e369e-106">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="e369e-106">Prerequisites</span></span>

* <span data-ttu-id="e369e-107">Tüm [öğreticiyi: Web API 'Si oluşturma](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="e369e-107">Complete [Tutorial: Create a web API](xref:tutorials/first-web-api)</span></span>
* <span data-ttu-id="e369e-108">CSS, HTML ve JavaScript ile benzerlik</span><span class="sxs-lookup"><span data-stu-id="e369e-108">Familiarity with CSS, HTML, and JavaScript</span></span>

## <a name="call-the-web-api-with-javascript"></a><span data-ttu-id="e369e-109">JavaScript ile Web API 'sini çağırma</span><span class="sxs-lookup"><span data-stu-id="e369e-109">Call the web API with JavaScript</span></span>

<span data-ttu-id="e369e-110">Bu bölümde, Yapılacaklar öğeleri oluşturmak ve yönetmek için form içeren bir HTML sayfası ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="e369e-110">In this section, you'll add an HTML page containing forms for creating and managing to-do items.</span></span> <span data-ttu-id="e369e-111">Olay işleyicileri sayfadaki öğelere iliştirilir.</span><span class="sxs-lookup"><span data-stu-id="e369e-111">Event handlers are attached to elements on the page.</span></span> <span data-ttu-id="e369e-112">Olay işleyicileri, Web API 'sinin eylem yöntemlerine HTTP istekleri sonucu vermez.</span><span class="sxs-lookup"><span data-stu-id="e369e-112">The event handlers result in HTTP requests to the web API's action methods.</span></span> <span data-ttu-id="e369e-113">Fetch API 'sinin `fetch` işlevi her HTTP isteğini başlatır.</span><span class="sxs-lookup"><span data-stu-id="e369e-113">The Fetch API's `fetch` function initiates each HTTP request.</span></span>

<span data-ttu-id="e369e-114">@No__t-0 işlevi, `Response` nesnesi olarak temsil edilen bir HTTP yanıtı içeren bir `Promise` nesnesi döndürür.</span><span class="sxs-lookup"><span data-stu-id="e369e-114">The `fetch` function returns a `Promise` object, which contains an HTTP response represented as a `Response` object.</span></span> <span data-ttu-id="e369e-115">Ortak bir model, `Response` nesnesinde `json` işlevini çağırarak JSON yanıt gövdesini ayıklamaya yönelik olur.</span><span class="sxs-lookup"><span data-stu-id="e369e-115">A common pattern is to extract the JSON response body by invoking the `json` function on the `Response` object.</span></span> <span data-ttu-id="e369e-116">JavaScript, sayfayı Web API 'sinin yanıtından alınan ayrıntılarla güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="e369e-116">JavaScript updates the page with the details from the web API's response.</span></span>

<span data-ttu-id="e369e-117">En basit `fetch` çağrısı, yolu temsil eden tek bir parametre kabul eder.</span><span class="sxs-lookup"><span data-stu-id="e369e-117">The simplest `fetch` call accepts a single parameter representing the route.</span></span> <span data-ttu-id="e369e-118">@No__t-0 nesnesi olarak bilinen ikinci bir parametre isteğe bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="e369e-118">A second parameter, known as the `init` object, is optional.</span></span> <span data-ttu-id="e369e-119">`init`, HTTP isteğini yapılandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="e369e-119">`init` is used to configure the HTTP request.</span></span>

1. <span data-ttu-id="e369e-120">Uygulamayı [statik dosyaları sunacak](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) ve [varsayılan dosya eşlemesini etkinleştirecek](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_)şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="e369e-120">Configure the app to [serve static files](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [enable default file mapping](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span> <span data-ttu-id="e369e-121">Aşağıdaki vurgulanan kod, *Startup.cs*öğesinin `Configure` yönteminde gereklidir:</span><span class="sxs-lookup"><span data-stu-id="e369e-121">The following highlighted code is needed in the `Configure` method of *Startup.cs*:</span></span>

    [!code-csharp[](first-web-api/samples/3.0/TodoApi/StartupJavaScript.cs?highlight=8-9&name=snippet_configure)]

1. <span data-ttu-id="e369e-122">Proje kökünde bir *Wwwroot* dizini oluşturun.</span><span class="sxs-lookup"><span data-stu-id="e369e-122">Create a *wwwroot* directory in the project root.</span></span>

1. <span data-ttu-id="e369e-123">*Wwwroot* dizinine *index. HTML* adlı bir HTML dosyası ekleyin.</span><span class="sxs-lookup"><span data-stu-id="e369e-123">Add an HTML file named *index.html* to the *wwwroot* directory.</span></span> <span data-ttu-id="e369e-124">İçeriğini aşağıdaki biçimlendirmeyle değiştirin:</span><span class="sxs-lookup"><span data-stu-id="e369e-124">Replace its contents with the following markup:</span></span>

    [!code-html[](first-web-api/samples/3.0/TodoApi/wwwroot/index.html)]

1. <span data-ttu-id="e369e-125">*Wwwroot* dizinine *site. js* adlı bir JavaScript dosyası ekleyin.</span><span class="sxs-lookup"><span data-stu-id="e369e-125">Add a JavaScript file named *site.js* to the *wwwroot* directory.</span></span> <span data-ttu-id="e369e-126">İçeriğini şu kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="e369e-126">Replace its contents with the following code:</span></span>

    [!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/js/site.js?name=snippet_SiteJs)]

<span data-ttu-id="e369e-127">HTML sayfasını yerel olarak test etmek için ASP.NET Core projesinin başlatma ayarlarındaki bir değişikliğin yapılması gerekebilir:</span><span class="sxs-lookup"><span data-stu-id="e369e-127">A change to the ASP.NET Core project's launch settings may be required to test the HTML page locally:</span></span>

1. <span data-ttu-id="e369e-128">*Properties\launchSettings.JSON*'i açın.</span><span class="sxs-lookup"><span data-stu-id="e369e-128">Open *Properties\launchSettings.json*.</span></span>
1. <span data-ttu-id="e369e-129">@No__t-0 özelliğini kaldırarak uygulamayı *Dizin. html*&mdash; ' de projenin varsayılan dosyasında açılmasını zorunlu kılın.</span><span class="sxs-lookup"><span data-stu-id="e369e-129">Remove the `launchUrl` property to force the app to open at *index.html*&mdash;the project's default file.</span></span>

<span data-ttu-id="e369e-130">Bu örnek, Web API 'sinin tüm CRUD yöntemlerini çağırır.</span><span class="sxs-lookup"><span data-stu-id="e369e-130">This sample calls all of the CRUD methods of the web API.</span></span> <span data-ttu-id="e369e-131">Web API isteklerinin açıklamaları aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="e369e-131">Following are explanations of the web API requests.</span></span>

### <a name="get-a-list-of-to-do-items"></a><span data-ttu-id="e369e-132">Yapılacaklar öğelerinin bir listesini alın</span><span class="sxs-lookup"><span data-stu-id="e369e-132">Get a list of to-do items</span></span>

<span data-ttu-id="e369e-133">Aşağıdaki kodda, *API/todoıtems* yoluna BIR http get isteği gönderilir:</span><span class="sxs-lookup"><span data-stu-id="e369e-133">In the following code, an HTTP GET request is sent to the *api/TodoItems* route:</span></span>

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/js/site.js?name=snippet_GetItems)]

<span data-ttu-id="e369e-134">Web API 'SI başarılı bir durum kodu döndürdüğünde `_displayItems` işlevi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="e369e-134">When the web API returns a successful status code, the `_displayItems` function is invoked.</span></span> <span data-ttu-id="e369e-135">@No__t-0 tarafından kabul edilen dizi parametresindeki her Yapılacaklar öğesi, **Düzenle** ve **Sil** düğmeleriyle bir tabloya eklenir.</span><span class="sxs-lookup"><span data-stu-id="e369e-135">Each to-do item in the array parameter accepted by `_displayItems` is added to a table with **Edit** and **Delete** buttons.</span></span> <span data-ttu-id="e369e-136">Web API isteği başarısız olursa, tarayıcının konsoluna bir hata kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="e369e-136">If the web API request fails, an error is logged to the browser's console.</span></span>

### <a name="add-a-to-do-item"></a><span data-ttu-id="e369e-137">Yapılacaklar öğesi ekleme</span><span class="sxs-lookup"><span data-stu-id="e369e-137">Add a to-do item</span></span>

<span data-ttu-id="e369e-138">Aşağıdaki kodda:</span><span class="sxs-lookup"><span data-stu-id="e369e-138">In the following code:</span></span>

* <span data-ttu-id="e369e-139">@No__t-0 değişkeni, yapılacaklar öğesinin nesne sabit gösterimini oluşturmak için bildirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="e369e-139">An `item` variable is declared to construct an object literal representation of the to-do item.</span></span>
* <span data-ttu-id="e369e-140">Aşağıdaki seçeneklerle bir getirme isteği yapılandırılır:</span><span class="sxs-lookup"><span data-stu-id="e369e-140">A Fetch request is configured with the following options:</span></span>
  * <span data-ttu-id="e369e-141">`method` @ no__t-1post HTTP eylem fiilini belirtir.</span><span class="sxs-lookup"><span data-stu-id="e369e-141">`method`&mdash;specifies the POST HTTP action verb.</span></span>
  * <span data-ttu-id="e369e-142">`body` @ no__t-1istek gövdesinin JSON temsilini belirtir.</span><span class="sxs-lookup"><span data-stu-id="e369e-142">`body`&mdash;specifies the JSON representation of the request body.</span></span> <span data-ttu-id="e369e-143">JSON, `item` ' da depolanan nesne sabit değeri [JSON. stringbelirt](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify) işlevine geçirilerek oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="e369e-143">The JSON is produced by passing the object literal stored in `item` to the [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify) function.</span></span>
  * <span data-ttu-id="e369e-144">`headers` @ no__t-1`Accept` ve `Content-Type` HTTP istek üst bilgilerini belirtir.</span><span class="sxs-lookup"><span data-stu-id="e369e-144">`headers`&mdash;specifies the `Accept` and `Content-Type` HTTP request headers.</span></span> <span data-ttu-id="e369e-145">Her iki üst bilgi de `application/json` olarak ayarlanır ve sırasıyla alınan ve gönderilen medya türünü belirtir.</span><span class="sxs-lookup"><span data-stu-id="e369e-145">Both headers are set to `application/json` to specify the media type being received and sent, respectively.</span></span>
* <span data-ttu-id="e369e-146">*API/todoıtems* yoluna BIR http post isteği gönderilir.</span><span class="sxs-lookup"><span data-stu-id="e369e-146">An HTTP POST request is sent to the *api/TodoItems* route.</span></span>

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/js/site.js?name=snippet_AddItem)]

<span data-ttu-id="e369e-147">Web API 'SI başarılı bir durum kodu döndürdüğünde, HTML tablosunu güncelleştirmek için `getItems` işlevi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="e369e-147">When the web API returns a successful status code, the `getItems` function is invoked to update the HTML table.</span></span> <span data-ttu-id="e369e-148">Web API isteği başarısız olursa, tarayıcının konsoluna bir hata kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="e369e-148">If the web API request fails, an error is logged to the browser's console.</span></span>

### <a name="update-a-to-do-item"></a><span data-ttu-id="e369e-149">Yapılacaklar öğesini güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="e369e-149">Update a to-do item</span></span>

<span data-ttu-id="e369e-150">Bir yapılacaklar öğesinin güncelleştirilmesi bir tane eklemeye benzer; Ancak, iki önemli fark vardır:</span><span class="sxs-lookup"><span data-stu-id="e369e-150">Updating a to-do item is similar to adding one; however, there are two significant differences:</span></span>

* <span data-ttu-id="e369e-151">Yol, güncelleştirilecek öğenin benzersiz tanımlayıcısı ile sone düzeltildi.</span><span class="sxs-lookup"><span data-stu-id="e369e-151">The route is suffixed with the unique identifier of the item to update.</span></span> <span data-ttu-id="e369e-152">Örneğin, *api/todoıtems/1*.</span><span class="sxs-lookup"><span data-stu-id="e369e-152">For example, *api/TodoItems/1*.</span></span>
* <span data-ttu-id="e369e-153">HTTP eylemi fiili, `method` seçeneğinde gösterildiği gibi konur.</span><span class="sxs-lookup"><span data-stu-id="e369e-153">The HTTP action verb is PUT, as indicated by the `method` option.</span></span>

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/js/site.js?name=snippet_UpdateItem)]

### <a name="delete-a-to-do-item"></a><span data-ttu-id="e369e-154">Bir yapılacaklar öğesini silme</span><span class="sxs-lookup"><span data-stu-id="e369e-154">Delete a to-do item</span></span>

<span data-ttu-id="e369e-155">Bir yapılacaklar öğesini silmek için, isteğin `method` seçeneğini `DELETE` olarak ayarlayın ve URL 'de öğenin benzersiz tanımlayıcısını belirtin.</span><span class="sxs-lookup"><span data-stu-id="e369e-155">To delete a to-do item, set the request's `method` option to `DELETE` and specify the item's unique identifier in the URL.</span></span>

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/js/site.js?name=snippet_DeleteItem)]

<span data-ttu-id="e369e-156">Web API 'SI yardım sayfaları oluşturmayı öğrenmek için bir sonraki öğreticiye ilerleyin:</span><span class="sxs-lookup"><span data-stu-id="e369e-156">Advance to the next tutorial to learn how to generate web API help pages:</span></span>

> [!div class="nextstepaction"]
> <xref:tutorials/get-started-with-swashbuckle>
