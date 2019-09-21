---
title: ASP.NET Core Blazor 'ten bir Web API 'SI çağırma
author: guardrex
description: "\"Çıkış noktaları arası kaynak paylaşımı (CORS) istekleri de dahil olmak üzere JSON yardımcıları kullanarak bir Blazor uygulamasından Web API 'SI çağırmayı öğrenin."
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/13/2019
uid: blazor/call-web-api
ms.openlocfilehash: 23131ac0357b722e7f229fcfe5dab8590cf34739
ms.sourcegitcommit: e5a74f882c14eaa0e5639ff082355e130559ba83
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71168052"
---
# <a name="call-a-web-api-from-aspnet-core-blazor"></a><span data-ttu-id="0d182-103">ASP.NET Core Blazor 'ten bir Web API 'SI çağırma</span><span class="sxs-lookup"><span data-stu-id="0d182-103">Call a web API from ASP.NET Core Blazor</span></span>

<span data-ttu-id="0d182-104">, [Luke Latham](https://github.com/guardrex) ve [Daniel Roth](https://github.com/danroth27) tarafından</span><span class="sxs-lookup"><span data-stu-id="0d182-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="0d182-105">Blazor webassembly Apps önceden yapılandırılmış `HttpClient` bir hizmeti kullanarak Web API 'lerini çağırır.</span><span class="sxs-lookup"><span data-stu-id="0d182-105">Blazor WebAssembly apps call web APIs using a preconfigured `HttpClient` service.</span></span> <span data-ttu-id="0d182-106">Blazor JSON yardımcıları veya ile <xref:System.Net.Http.HttpRequestMessage>kullanarak JavaScript [getirme API 'si](https://developer.mozilla.org/docs/Web/API/Fetch_API) seçeneklerini içerebilen oluşturma istekleri.</span><span class="sxs-lookup"><span data-stu-id="0d182-106">Compose requests, which can include JavaScript [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) options, using Blazor JSON helpers or with <xref:System.Net.Http.HttpRequestMessage>.</span></span>

<span data-ttu-id="0d182-107">Blazor Server Apps, genellikle kullanılarak <xref:System.Net.Http.HttpClient> <xref:System.Net.Http.IHttpClientFactory>oluşturulan örnekleri kullanarak Web API 'lerini çağırır.</span><span class="sxs-lookup"><span data-stu-id="0d182-107">Blazor Server apps call web APIs using <xref:System.Net.Http.HttpClient> instances typically created using <xref:System.Net.Http.IHttpClientFactory>.</span></span> <span data-ttu-id="0d182-108">Daha fazla bilgi için bkz. <xref:fundamentals/http-requests>.</span><span class="sxs-lookup"><span data-stu-id="0d182-108">For more information, see <xref:fundamentals/http-requests>.</span></span>

<span data-ttu-id="0d182-109">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0d182-109">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="0d182-110">Blazor WebAssembly örnekleri için örnek uygulamada aşağıdaki bileşenlere bakın:</span><span class="sxs-lookup"><span data-stu-id="0d182-110">For Blazor WebAssembly examples, see the following components in the sample app:</span></span>

* <span data-ttu-id="0d182-111">Web API çağrısı (*Pages/CallWebAPI. Razor*)</span><span class="sxs-lookup"><span data-stu-id="0d182-111">Call Web API (*Pages/CallWebAPI.razor*)</span></span>
* <span data-ttu-id="0d182-112">HTTP Isteği Sınayıcısı (*Bileşenler/HTTPRequestTester. Razor*)</span><span class="sxs-lookup"><span data-stu-id="0d182-112">HTTP Request Tester (*Components/HTTPRequestTester.razor*)</span></span>

## <a name="httpclient-and-json-helpers"></a><span data-ttu-id="0d182-113">HttpClient ve JSON yardımcıları</span><span class="sxs-lookup"><span data-stu-id="0d182-113">HttpClient and JSON helpers</span></span>

<span data-ttu-id="0d182-114">Blazor WebAssembly uygulamalarında [HttpClient](xref:fundamentals/http-requests) , istekleri kaynak sunucuya geri getirmek için önceden yapılandırılmış bir hizmet olarak sunulmaktadır.</span><span class="sxs-lookup"><span data-stu-id="0d182-114">In Blazor WebAssembly apps, [HttpClient](xref:fundamentals/http-requests) is available as a preconfigured service for making requests back to the origin server.</span></span> <span data-ttu-id="0d182-115">JSON yardımcıları `HttpClient` kullanmak için, öğesine `Microsoft.AspNetCore.Blazor.HttpClient`bir paket başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="0d182-115">To use `HttpClient` JSON helpers, add a package reference to `Microsoft.AspNetCore.Blazor.HttpClient`.</span></span> <span data-ttu-id="0d182-116">`HttpClient`ve JSON yardımcıları, üçüncü taraf Web API uç noktalarını çağırmak için de kullanılır.</span><span class="sxs-lookup"><span data-stu-id="0d182-116">`HttpClient` and JSON helpers are also used to call third-party web API endpoints.</span></span> <span data-ttu-id="0d182-117">`HttpClient`, tarayıcı [getirme API 'si](https://developer.mozilla.org/docs/Web/API/Fetch_API) kullanılarak uygulanır ve aynı kaynak ilkesini zorlama dahil olmak üzere sınırlamalarına tabidir.</span><span class="sxs-lookup"><span data-stu-id="0d182-117">`HttpClient` is implemented using the browser [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) and is subject to its limitations, including enforcement of the same origin policy.</span></span>

<span data-ttu-id="0d182-118">İstemcinin temel adresi, kaynak sunucunun adresine ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="0d182-118">The client's base address is set to the originating server's address.</span></span> <span data-ttu-id="0d182-119">Yönergesini kullanarak bir `HttpClient` örnek ekleme: `@inject`</span><span class="sxs-lookup"><span data-stu-id="0d182-119">Inject an `HttpClient` instance using the `@inject` directive:</span></span>

```cshtml
@using System.Net.Http
@inject HttpClient Http
```

<span data-ttu-id="0d182-120">Aşağıdaki örneklerde, bir Todo Web API 'SI oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemlerini işler.</span><span class="sxs-lookup"><span data-stu-id="0d182-120">In the following examples, a Todo web API processes create, read, update, and delete (CRUD) operations.</span></span> <span data-ttu-id="0d182-121">Örnekler şunları depolayan bir `TodoItem` sınıfa dayalıdır:</span><span class="sxs-lookup"><span data-stu-id="0d182-121">The examples are based on a `TodoItem` class that stores the:</span></span>

* <span data-ttu-id="0d182-122">Öğenin kimliği`Id`( `long`, &ndash; ) benzersiz kimliği.</span><span class="sxs-lookup"><span data-stu-id="0d182-122">ID (`Id`, `long`) &ndash; Unique ID of the item.</span></span>
* <span data-ttu-id="0d182-123">Öğenin adı`Name`( `string`, &ndash; ).</span><span class="sxs-lookup"><span data-stu-id="0d182-123">Name (`Name`, `string`) &ndash; Name of the item.</span></span>
* <span data-ttu-id="0d182-124">Todo öğesi`IsComplete`tamamlandığında `bool`durum &ndash; (,) göstergesi.</span><span class="sxs-lookup"><span data-stu-id="0d182-124">Status (`IsComplete`, `bool`) &ndash; Indication if the Todo item is finished.</span></span>

```csharp
private class TodoItem
{
    public long Id { get; set; }
    public string Name { get; set; }
    public bool IsComplete { get; set; }
}
```

<span data-ttu-id="0d182-125">JSON yardımcı yöntemleri bir URI 'ye (aşağıdaki örneklerde bir Web API 'si) istek gönderir ve yanıtı işler:</span><span class="sxs-lookup"><span data-stu-id="0d182-125">JSON helper methods send requests to a URI (a web API in the following examples) and process the response:</span></span>

* <span data-ttu-id="0d182-126">`GetJsonAsync`&ndash; Bir http get isteği gönderir ve bir nesne oluşturmak için JSON yanıt gövdesini ayrıştırır.</span><span class="sxs-lookup"><span data-stu-id="0d182-126">`GetJsonAsync` &ndash; Sends an HTTP GET request and parses the JSON response body to create an object.</span></span>

  <span data-ttu-id="0d182-127">Aşağıdaki kodda,, `_todoItems` bileşeni tarafından görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="0d182-127">In the following code, the `_todoItems` are displayed by the component.</span></span> <span data-ttu-id="0d182-128">Yöntemi, bileşen işlemeyi tamamladığında tetiklenir ([onınitializedadsync).](xref:blazor/components#lifecycle-methods) `GetTodoItems`</span><span class="sxs-lookup"><span data-stu-id="0d182-128">The `GetTodoItems` method is triggered when the component is finished rendering ([OnInitializedAsync](xref:blazor/components#lifecycle-methods)).</span></span> <span data-ttu-id="0d182-129">Örnek uygulamaya bkz. örnek uygulama.</span><span class="sxs-lookup"><span data-stu-id="0d182-129">See the sample app for a complete example.</span></span>

  ```cshtml
  @using System.Net.Http
  @inject HttpClient Http

  @code {
      private TodoItem[] _todoItems;

      protected override async Task OnInitializedAsync() => 
          _todoItems = await Http.GetJsonAsync<TodoItem[]>("api/todo");
  }
  ```

* <span data-ttu-id="0d182-130">`PostJsonAsync`&ndash; JSON kodlu içerik dahil olmak üzere bir http post isteği gönderir ve bir nesne oluşturmak için JSON yanıt gövdesini ayrıştırır.</span><span class="sxs-lookup"><span data-stu-id="0d182-130">`PostJsonAsync` &ndash; Sends an HTTP POST request, including JSON-encoded content, and parses the JSON response body to create an object.</span></span>

  <span data-ttu-id="0d182-131">Aşağıdaki kodda, `_newItemName` bileşenin bağlantılı bir öğesi tarafından sağlanır.</span><span class="sxs-lookup"><span data-stu-id="0d182-131">In the following code, `_newItemName` is provided by a bound element of the component.</span></span> <span data-ttu-id="0d182-132">`AddItem` Yöntemi bir`<button>` öğesi seçilerek tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="0d182-132">The `AddItem` method is triggered by selecting a `<button>` element.</span></span> <span data-ttu-id="0d182-133">Örnek uygulamaya bkz. örnek uygulama.</span><span class="sxs-lookup"><span data-stu-id="0d182-133">See the sample app for a complete example.</span></span>

  ```cshtml
  @using System.Net.Http
  @inject HttpClient Http

  <input @bind="_newItemName" placeholder="New Todo Item" />
  <button @onclick="@AddItem">Add</button>

  @code {
      private string _newItemName;

      private async Task AddItem()
      {
          var addItem = new TodoItem { Name = _newItemName, IsComplete = false };
          await Http.PostJsonAsync("api/todo", addItem);
      }
  }
  ```

* <span data-ttu-id="0d182-134">`PutJsonAsync`&ndash; JSON kodlu içerik dahil olmak üzere bir http put isteği gönderir.</span><span class="sxs-lookup"><span data-stu-id="0d182-134">`PutJsonAsync` &ndash; Sends an HTTP PUT request, including JSON-encoded content.</span></span>

  <span data-ttu-id="0d182-135">Aşağıdaki kodda, `_editItem` `Name` ve `IsCompleted` değerleri bileşenin bağlantılı öğelerine göre sağlanır.</span><span class="sxs-lookup"><span data-stu-id="0d182-135">In the following code, `_editItem` values for `Name` and `IsCompleted` are provided by bound elements of the component.</span></span> <span data-ttu-id="0d182-136">Öğe, Kullanıcı arabiriminin başka bir bölümünde seçildiğinde ayarlanır ve `EditItem` çağrılır. `Id`</span><span class="sxs-lookup"><span data-stu-id="0d182-136">The item's `Id` is set when the item is selected in another part of the UI and `EditItem` is called.</span></span> <span data-ttu-id="0d182-137">Yöntemi, Kaydet `<button>` öğesi seçilerek tetiklenir. `SaveItem`</span><span class="sxs-lookup"><span data-stu-id="0d182-137">The `SaveItem` method is triggered by selecting the Save `<button>` element.</span></span> <span data-ttu-id="0d182-138">Örnek uygulamaya bkz. örnek uygulama.</span><span class="sxs-lookup"><span data-stu-id="0d182-138">See the sample app for a complete example.</span></span>

  ```cshtml
  @using System.Net.Http
  @inject HttpClient Http

  <input type="checkbox" @bind="_editItem.IsComplete" />
  <input @bind="_editItem.Name" />
  <button @onclick="@SaveItem">Save</button>

  @code {
      private TodoItem _editItem = new TodoItem();

      private void EditItem(long id)
      {
          var editItem = _todoItems.Single(i => i.Id == id);
          _editItem = new TodoItem { Id = editItem.Id, Name = editItem.Name, 
              IsComplete = editItem.IsComplete };
      }

      private async Task SaveItem() =>
          await Http.PutJsonAsync($"api/todo/{_editItem.Id}, _editItem);
  }
  ```

<span data-ttu-id="0d182-139"><xref:System.Net.Http>HTTP istekleri göndermeye ve HTTP yanıtlarını almaya yönelik ek uzantı yöntemleri içerir.</span><span class="sxs-lookup"><span data-stu-id="0d182-139"><xref:System.Net.Http> includes additional extension methods for sending HTTP requests and receiving HTTP responses.</span></span> <span data-ttu-id="0d182-140">[HttpClient. DeleteAsync](xref:System.Net.Http.HttpClient.DeleteAsync*) , BIR Web API 'SINE http delete isteği göndermek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="0d182-140">[HttpClient.DeleteAsync](xref:System.Net.Http.HttpClient.DeleteAsync*) is used to send an HTTP DELETE request to a web API.</span></span>

<span data-ttu-id="0d182-141">Aşağıdaki kodda, Delete `<button>` öğesi `DeleteItem` yöntemini çağırır.</span><span class="sxs-lookup"><span data-stu-id="0d182-141">In the following code, the Delete `<button>` element calls the `DeleteItem` method.</span></span> <span data-ttu-id="0d182-142">Bağlantılı `<input>` öğe, silinecek öğenin `id` bir listesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="0d182-142">The bound `<input>` element supplies the `id` of the item to delete.</span></span> <span data-ttu-id="0d182-143">Örnek uygulamaya bkz. örnek uygulama.</span><span class="sxs-lookup"><span data-stu-id="0d182-143">See the sample app for a complete example.</span></span>

```cshtml
@using System.Net.Http
@inject HttpClient Http

<input @bind="_id" />
<button @onclick="@DeleteItem">Delete</button>

@code {
    private long _id;

    private async Task DeleteItem() =>
        await Http.DeleteAsync($"api/todo/{_id}");
}
```

## <a name="cross-origin-resource-sharing-cors"></a><span data-ttu-id="0d182-144">Çıkış noktaları arası kaynak paylaşımı (CORS)</span><span class="sxs-lookup"><span data-stu-id="0d182-144">Cross-origin resource sharing (CORS)</span></span>

<span data-ttu-id="0d182-145">Tarayıcı güvenliği, bir Web sayfasının, Web sayfasını sunduğundan farklı bir etki alanına istek yapmasını engeller.</span><span class="sxs-lookup"><span data-stu-id="0d182-145">Browser security prevents a webpage from making requests to a different domain than the one that served the webpage.</span></span> <span data-ttu-id="0d182-146">Bu kısıtlamaya *aynı-Origin ilkesi*adı verilir.</span><span class="sxs-lookup"><span data-stu-id="0d182-146">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="0d182-147">Aynı-kaynak ilkesi, kötü niyetli bir sitenin gizli verileri başka bir siteden okumasını engeller.</span><span class="sxs-lookup"><span data-stu-id="0d182-147">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="0d182-148">Tarayıcıdan farklı bir kaynağa sahip bir uç noktaya istek yapmak için *uç noktanın* , [çıkış noktaları arası kaynak PAYLAŞıMı 'nı (CORS)](https://www.w3.org/TR/cors/)etkinleştirmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="0d182-148">To make requests from the browser to an endpoint with a different origin, the *endpoint* must enable [cross-origin resource sharing (CORS)](https://www.w3.org/TR/cors/).</span></span>

<span data-ttu-id="0d182-149">Örnek uygulama, Web API bileşeninde CORS 'nin kullanımını gösterir (*Pages/CallWebAPI. Razor*).</span><span class="sxs-lookup"><span data-stu-id="0d182-149">The sample app demonstrates the use of CORS in the Call Web API component (*Pages/CallWebAPI.razor*).</span></span>

<span data-ttu-id="0d182-150">Diğer sitelerin uygulamanıza çıkış noktaları arası kaynak paylaşımı (CORS) istekleri yapmasına izin vermek için bkz <xref:security/cors>.</span><span class="sxs-lookup"><span data-stu-id="0d182-150">To allow other sites to make cross-origin resource sharing (CORS) requests to your app, see <xref:security/cors>.</span></span>

## <a name="httpclient-and-httprequestmessage-with-fetch-api-request-options"></a><span data-ttu-id="0d182-151">API istek seçeneklerini getiren HttpClient ve HttpRequestMessage</span><span class="sxs-lookup"><span data-stu-id="0d182-151">HttpClient and HttpRequestMessage with Fetch API request options</span></span>

<span data-ttu-id="0d182-152">Blazor webassembly uygulamasında webassembly üzerinde çalışırken, [HttpClient](xref:fundamentals/http-requests) kullanın ve <xref:System.Net.Http.HttpRequestMessage> istekleri özelleştirin.</span><span class="sxs-lookup"><span data-stu-id="0d182-152">When running on WebAssembly in a Blazor WebAssembly app, use [HttpClient](xref:fundamentals/http-requests) and <xref:System.Net.Http.HttpRequestMessage> to customize requests.</span></span> <span data-ttu-id="0d182-153">Örneğin, istek URI 'SI, HTTP yöntemi ve istenen istek üst bilgilerini belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0d182-153">For example, you can specify the request URI, HTTP method, and any desired request headers.</span></span>

<span data-ttu-id="0d182-154">İstek üzerindeki `WebAssemblyHttpMessageHandler.FetchArgs` özelliği kullanarak temeldeki JavaScript [getirme API](https://developer.mozilla.org/docs/Web/API/Fetch_API) 'sine yönelik istek seçeneklerini sağlayın.</span><span class="sxs-lookup"><span data-stu-id="0d182-154">Supply request options to the underlying JavaScript [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) using the `WebAssemblyHttpMessageHandler.FetchArgs` property on the request.</span></span> <span data-ttu-id="0d182-155">Aşağıdaki örnekte gösterildiği gibi, `credentials` özelliği aşağıdaki değerlerden herhangi birine ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="0d182-155">As shown in the following example, the `credentials` property is set to any of the following values:</span></span>

* <span data-ttu-id="0d182-156">`FetchCredentialsOption.Include`("Ekle") &ndash; Çapraz kaynak istekleri için bile, tarayıcıya kimlik bilgileri (örneğin, tanımlama bilgileri veya http kimlik doğrulama üstbilgileri) göndermesini önerir.</span><span class="sxs-lookup"><span data-stu-id="0d182-156">`FetchCredentialsOption.Include` ("include") &ndash; Advises the browser to send credentials (such as cookies or HTTP authentication headers) even for cross-origin requests.</span></span> <span data-ttu-id="0d182-157">Yalnızca CORS ilkesi kimlik bilgilerine izin verecek şekilde yapılandırıldığında izin verilir.</span><span class="sxs-lookup"><span data-stu-id="0d182-157">Only allowed when the CORS policy is configured to allow credentials.</span></span>
* <span data-ttu-id="0d182-158">`FetchCredentialsOption.Omit`("atla") &ndash; Tarayıcıya kimlik bilgileri (örneğin, tanımlama bilgileri veya http kimlik doğrulama üstbilgileri) göndermemeyi önerir.</span><span class="sxs-lookup"><span data-stu-id="0d182-158">`FetchCredentialsOption.Omit` ("omit") &ndash; Advises the browser never to send credentials (such as cookies or HTTP auth headers).</span></span>
* <span data-ttu-id="0d182-159">`FetchCredentialsOption.SameOrigin`("aynı kaynak") &ndash; Yalnızca hedef URL, çağıran uygulamayla aynı kaynak üzerinde olduğunda kimlik bilgileri (tanımlama bilgileri veya http kimlik doğrulama üstbilgileri gibi) göndermek için tarayıcıya sahip olur.</span><span class="sxs-lookup"><span data-stu-id="0d182-159">`FetchCredentialsOption.SameOrigin` ("same-origin") &ndash; Advises the browser to send credentials (such as cookies or HTTP auth headers) only if the target URL is on the same origin as the calling application.</span></span>

```cshtml
@using System.Net.Http
@using System.Net.Http.Headers
@inject HttpClient Http

@code {
    private async Task PostRequest()
    {
        Http.DefaultRequestHeaders.Authorization =
            new AuthenticationHeaderValue("Bearer", "{OAUTH TOKEN}");

        var requestMessage = new HttpRequestMessage()
        {
            Method = new HttpMethod("POST"),
            RequestUri = new Uri("https://localhost:10000/api/todo"),
            Content = 
                new StringContent(
                    @"{""name"":""A New Todo Item"",""isComplete"":false}")
        };

        requestMessage.Content.Headers.ContentType = 
            new System.Net.Http.Headers.MediaTypeHeaderValue(
                "application/json");

        requestMessage.Content.Headers.TryAddWithoutValidation(
            "x-custom-header", "value");
        
        requestMessage.Properties[WebAssemblyHttpMessageHandler.FetchArgs] = new
        { 
            credentials = FetchCredentialsOption.Include
        };

        var response = await Http.SendAsync(requestMessage);
        var responseStatusCode = response.StatusCode;
        var responseBody = await response.Content.ReadAsStringAsync();
    }
}
```

<span data-ttu-id="0d182-160">API seçenekleri getirme hakkında daha fazla bilgi için bkz [. MDN Web belgeleri: WindowOrWorkerGlobalScope. Fetch ():P arametre](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters).</span><span class="sxs-lookup"><span data-stu-id="0d182-160">For more information on Fetch API options, see [MDN web docs: WindowOrWorkerGlobalScope.fetch():Parameters](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters).</span></span>

<span data-ttu-id="0d182-161">CORS isteklerindeki kimlik bilgileri (yetkilendirme tanımlama bilgileri/üstbilgileri) gönderilirken, `Authorization` bir üst bilgiye CORS ilkesi tarafından izin verilmelidir.</span><span class="sxs-lookup"><span data-stu-id="0d182-161">When sending credentials (authorization cookies/headers) on CORS requests, the `Authorization` header must be allowed by the CORS policy.</span></span>

<span data-ttu-id="0d182-162">Aşağıdaki ilke için yapılandırma içerir:</span><span class="sxs-lookup"><span data-stu-id="0d182-162">The following policy includes configuration for:</span></span>

* <span data-ttu-id="0d182-163">İstek kaynakları (`http://localhost:5000`, `https://localhost:5001`).</span><span class="sxs-lookup"><span data-stu-id="0d182-163">Request origins (`http://localhost:5000`, `https://localhost:5001`).</span></span>
* <span data-ttu-id="0d182-164">Any yöntemi (fiil).</span><span class="sxs-lookup"><span data-stu-id="0d182-164">Any method (verb).</span></span>
* <span data-ttu-id="0d182-165">`Content-Type`ve `Authorization` üst bilgiler.</span><span class="sxs-lookup"><span data-stu-id="0d182-165">`Content-Type` and `Authorization` headers.</span></span> <span data-ttu-id="0d182-166">Özel bir üstbilgiye (örneğin, `x-custom-header`) izin vermek için, çağrılırken <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>üstbilgiyi listeleyin.</span><span class="sxs-lookup"><span data-stu-id="0d182-166">To allow a custom header (for example, `x-custom-header`), list the header when calling <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>.</span></span>
* <span data-ttu-id="0d182-167">İstemci tarafı JavaScript kodu tarafından ayarlanan kimlik bilgileri (`credentials` özellik olarak `include`ayarlanır).</span><span class="sxs-lookup"><span data-stu-id="0d182-167">Credentials set by client-side JavaScript code (`credentials` property set to `include`).</span></span>

```csharp
app.UseCors(policy => 
    policy.WithOrigins("http://localhost:5000", "https://localhost:5001")
    .AllowAnyMethod()
    .WithHeaders(HeaderNames.ContentType, HeaderNames.Authorization, "x-custom-header")
    .AllowCredentials());
```

<span data-ttu-id="0d182-168">Daha fazla bilgi için, <xref:security/cors> bkz. ve örnek uygulamanın http isteği Sınayıcısı bileşeni (*Bileşenler/httprequesttester. Razor*).</span><span class="sxs-lookup"><span data-stu-id="0d182-168">For more information, see <xref:security/cors> and the sample app's HTTP Request Tester component (*Components/HTTPRequestTester.razor*).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0d182-169">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="0d182-169">Additional resources</span></span>

* <xref:fundamentals/http-requests>
* [<span data-ttu-id="0d182-170">W3C üzerinde çapraz kaynak kaynak paylaşımı (CORS)</span><span class="sxs-lookup"><span data-stu-id="0d182-170">Cross Origin Resource Sharing (CORS) at W3C</span></span>](https://www.w3.org/TR/cors/)
