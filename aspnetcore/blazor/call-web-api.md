---
title: ASP.NET Core Blazor bir Web API 'SI çağırma
author: guardrex
description: Çıkış noktaları arası kaynak paylaşımı (CORS) istekleri yapma dahil olmak üzere JSON yardımcıları kullanarak bir Blazor uygulamasından Web API 'SI çağırmayı öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/15/2019
no-loc:
- Blazor
uid: blazor/call-web-api
ms.openlocfilehash: b5c57317005d0072410542bad322458b1cb3f5ee
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2019
ms.locfileid: "73962720"
---
# <a name="call-a-web-api-from-aspnet-core-opno-locblazor"></a><span data-ttu-id="51853-103">ASP.NET Core Blazor bir Web API 'SI çağırma</span><span class="sxs-lookup"><span data-stu-id="51853-103">Call a web API from ASP.NET Core Blazor</span></span>

<span data-ttu-id="51853-104">[Luke Latham](https://github.com/guardrex), [Daniel Roth](https://github.com/danroth27)ve [Juan de la Cruz](https://github.com/juandelacruz23) tarafından</span><span class="sxs-lookup"><span data-stu-id="51853-104">By [Luke Latham](https://github.com/guardrex), [Daniel Roth](https://github.com/danroth27), and [Juan De la Cruz](https://github.com/juandelacruz23)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor<span data-ttu-id="51853-105"> WebAssembly Apps, önceden yapılandırılmış bir `HttpClient` hizmetini kullanarak Web API 'Lerini çağırır.</span><span class="sxs-lookup"><span data-stu-id="51853-105"> WebAssembly apps call web APIs using a preconfigured `HttpClient` service.</span></span> <span data-ttu-id="51853-106">Blazor JSON yardımcıları veya <xref:System.Net.Http.HttpRequestMessage>kullanarak JavaScript [getirme API 'si](https://developer.mozilla.org/docs/Web/API/Fetch_API) seçeneklerini içerebilen oluşturma istekleri.</span><span class="sxs-lookup"><span data-stu-id="51853-106">Compose requests, which can include JavaScript [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) options, using Blazor JSON helpers or with <xref:System.Net.Http.HttpRequestMessage>.</span></span>

Blazor<span data-ttu-id="51853-107"> Server Apps, genellikle <xref:System.Net.Http.IHttpClientFactory>kullanılarak oluşturulan <xref:System.Net.Http.HttpClient> örneklerini kullanarak Web API 'Lerini çağırır.</span><span class="sxs-lookup"><span data-stu-id="51853-107"> Server apps call web APIs using <xref:System.Net.Http.HttpClient> instances typically created using <xref:System.Net.Http.IHttpClientFactory>.</span></span> <span data-ttu-id="51853-108">Daha fazla bilgi için bkz. <xref:fundamentals/http-requests>.</span><span class="sxs-lookup"><span data-stu-id="51853-108">For more information, see <xref:fundamentals/http-requests>.</span></span>

<span data-ttu-id="51853-109">[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="51853-109">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="51853-110">Blazor WebAssembly örnekleri için örnek uygulamada aşağıdaki bileşenlere bakın:</span><span class="sxs-lookup"><span data-stu-id="51853-110">For Blazor WebAssembly examples, see the following components in the sample app:</span></span>

* <span data-ttu-id="51853-111">Web API çağrısı (*Pages/CallWebAPI. Razor*)</span><span class="sxs-lookup"><span data-stu-id="51853-111">Call Web API (*Pages/CallWebAPI.razor*)</span></span>
* <span data-ttu-id="51853-112">HTTP Isteği Sınayıcısı (*Bileşenler/HTTPRequestTester. Razor*)</span><span class="sxs-lookup"><span data-stu-id="51853-112">HTTP Request Tester (*Components/HTTPRequestTester.razor*)</span></span>

## <a name="httpclient-and-json-helpers"></a><span data-ttu-id="51853-113">HttpClient ve JSON yardımcıları</span><span class="sxs-lookup"><span data-stu-id="51853-113">HttpClient and JSON helpers</span></span>

<span data-ttu-id="51853-114">Blazor WebAssembly uygulamalarında, [HttpClient](xref:fundamentals/http-requests) , istekleri kaynak sunucuya geri getirmek için önceden yapılandırılmış bir hizmet olarak sunulmaktadır.</span><span class="sxs-lookup"><span data-stu-id="51853-114">In Blazor WebAssembly apps, [HttpClient](xref:fundamentals/http-requests) is available as a preconfigured service for making requests back to the origin server.</span></span> <span data-ttu-id="51853-115">`HttpClient` JSON yardımcıları kullanmak için `Microsoft.AspNetCore.Blazor.HttpClient`bir paket başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="51853-115">To use `HttpClient` JSON helpers, add a package reference to `Microsoft.AspNetCore.Blazor.HttpClient`.</span></span> <span data-ttu-id="51853-116">`HttpClient` ve JSON yardımcıları, üçüncü taraf Web API uç noktalarını çağırmak için de kullanılır.</span><span class="sxs-lookup"><span data-stu-id="51853-116">`HttpClient` and JSON helpers are also used to call third-party web API endpoints.</span></span> <span data-ttu-id="51853-117">`HttpClient` tarayıcı [getirme API 'si](https://developer.mozilla.org/docs/Web/API/Fetch_API) kullanılarak uygulanır ve aynı kaynak ilkesini zorlama dahil olmak üzere sınırlamalarına tabidir.</span><span class="sxs-lookup"><span data-stu-id="51853-117">`HttpClient` is implemented using the browser [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) and is subject to its limitations, including enforcement of the same origin policy.</span></span>

<span data-ttu-id="51853-118">İstemcinin temel adresi, kaynak sunucunun adresine ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="51853-118">The client's base address is set to the originating server's address.</span></span> <span data-ttu-id="51853-119">`@inject` yönergesini kullanarak bir `HttpClient` örneği ekleme:</span><span class="sxs-lookup"><span data-stu-id="51853-119">Inject an `HttpClient` instance using the `@inject` directive:</span></span>

```cshtml
@using System.Net.Http
@inject HttpClient Http
```

<span data-ttu-id="51853-120">Aşağıdaki örneklerde, bir Todo Web API 'SI oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemlerini işler.</span><span class="sxs-lookup"><span data-stu-id="51853-120">In the following examples, a Todo web API processes create, read, update, and delete (CRUD) operations.</span></span> <span data-ttu-id="51853-121">Örnekler, şunu depolayan bir `TodoItem` sınıfına dayalıdır:</span><span class="sxs-lookup"><span data-stu-id="51853-121">The examples are based on a `TodoItem` class that stores the:</span></span>

* <span data-ttu-id="51853-122">KIMLIK (`Id`, `long`) &ndash; öğenin benzersiz KIMLIĞI.</span><span class="sxs-lookup"><span data-stu-id="51853-122">ID (`Id`, `long`) &ndash; Unique ID of the item.</span></span>
* <span data-ttu-id="51853-123">Ad (`Name`, `string`) öğenin &ndash; adı.</span><span class="sxs-lookup"><span data-stu-id="51853-123">Name (`Name`, `string`) &ndash; Name of the item.</span></span>
* <span data-ttu-id="51853-124">Durum (`IsComplete`, `bool`) &ndash; göstergesi, Todo öğesi bitmiştir.</span><span class="sxs-lookup"><span data-stu-id="51853-124">Status (`IsComplete`, `bool`) &ndash; Indication if the Todo item is finished.</span></span>

```csharp
private class TodoItem
{
    public long Id { get; set; }
    public string Name { get; set; }
    public bool IsComplete { get; set; }
}
```

<span data-ttu-id="51853-125">JSON yardımcı yöntemleri bir URI 'ye (aşağıdaki örneklerde bir Web API 'si) istek gönderir ve yanıtı işler:</span><span class="sxs-lookup"><span data-stu-id="51853-125">JSON helper methods send requests to a URI (a web API in the following examples) and process the response:</span></span>

* <span data-ttu-id="51853-126">`GetJsonAsync` &ndash; bir HTTP GET isteği gönderir ve bir nesne oluşturmak için JSON yanıt gövdesini ayrıştırır.</span><span class="sxs-lookup"><span data-stu-id="51853-126">`GetJsonAsync` &ndash; Sends an HTTP GET request and parses the JSON response body to create an object.</span></span>

  <span data-ttu-id="51853-127">Aşağıdaki kodda `_todoItems` bileşen tarafından görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="51853-127">In the following code, the `_todoItems` are displayed by the component.</span></span> <span data-ttu-id="51853-128">`GetTodoItems` yöntemi, bileşen işlemeyi tamamladığında tetiklenir ([Onınitializedadsync](xref:blazor/components#lifecycle-methods)).</span><span class="sxs-lookup"><span data-stu-id="51853-128">The `GetTodoItems` method is triggered when the component is finished rendering ([OnInitializedAsync](xref:blazor/components#lifecycle-methods)).</span></span> <span data-ttu-id="51853-129">Örnek uygulamaya bkz. örnek uygulama.</span><span class="sxs-lookup"><span data-stu-id="51853-129">See the sample app for a complete example.</span></span>

  ```cshtml
  @using System.Net.Http
  @inject HttpClient Http

  @code {
      private TodoItem[] _todoItems;

      protected override async Task OnInitializedAsync() => 
          _todoItems = await Http.GetJsonAsync<TodoItem[]>("api/TodoItems");
  }
  ```

* <span data-ttu-id="51853-130">`PostJsonAsync` &ndash; JSON kodlu içerik dahil bir HTTP POST isteği gönderir ve bir nesne oluşturmak için JSON yanıt gövdesini ayrıştırır.</span><span class="sxs-lookup"><span data-stu-id="51853-130">`PostJsonAsync` &ndash; Sends an HTTP POST request, including JSON-encoded content, and parses the JSON response body to create an object.</span></span>

  <span data-ttu-id="51853-131">Aşağıdaki kodda, `_newItemName` bileşenin bağlantılı bir öğesi tarafından sağlanır.</span><span class="sxs-lookup"><span data-stu-id="51853-131">In the following code, `_newItemName` is provided by a bound element of the component.</span></span> <span data-ttu-id="51853-132">`AddItem` yöntemi bir `<button>` öğesi seçilerek tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="51853-132">The `AddItem` method is triggered by selecting a `<button>` element.</span></span> <span data-ttu-id="51853-133">Örnek uygulamaya bkz. örnek uygulama.</span><span class="sxs-lookup"><span data-stu-id="51853-133">See the sample app for a complete example.</span></span>

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
          await Http.PostJsonAsync("api/TodoItems", addItem);
      }
  }
  ```

* <span data-ttu-id="51853-134">`PutJsonAsync` &ndash; JSON kodlu içerik dahil bir HTTP PUT isteği gönderir.</span><span class="sxs-lookup"><span data-stu-id="51853-134">`PutJsonAsync` &ndash; Sends an HTTP PUT request, including JSON-encoded content.</span></span>

  <span data-ttu-id="51853-135">Aşağıdaki kodda, `Name` ve `IsCompleted` değerlerinin `_editItem`, bileşenin bağlantılı öğeleri tarafından sağlanır.</span><span class="sxs-lookup"><span data-stu-id="51853-135">In the following code, `_editItem` values for `Name` and `IsCompleted` are provided by bound elements of the component.</span></span> <span data-ttu-id="51853-136">Öğe `Id`, öğe Kullanıcı arabiriminin başka bir bölümünde seçildiğinde ayarlanır ve `EditItem` çağırılır.</span><span class="sxs-lookup"><span data-stu-id="51853-136">The item's `Id` is set when the item is selected in another part of the UI and `EditItem` is called.</span></span> <span data-ttu-id="51853-137">`SaveItem` yöntemi, Kaydet `<button>` öğesi seçilerek tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="51853-137">The `SaveItem` method is triggered by selecting the Save `<button>` element.</span></span> <span data-ttu-id="51853-138">Örnek uygulamaya bkz. örnek uygulama.</span><span class="sxs-lookup"><span data-stu-id="51853-138">See the sample app for a complete example.</span></span>

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
          await Http.PutJsonAsync($"api/TodoItems/{_editItem.Id}, _editItem);
  }
  ```

<span data-ttu-id="51853-139"><xref:System.Net.Http>, HTTP istekleri göndermeye ve HTTP yanıtlarını almaya yönelik ek uzantı yöntemleri içerir.</span><span class="sxs-lookup"><span data-stu-id="51853-139"><xref:System.Net.Http> includes additional extension methods for sending HTTP requests and receiving HTTP responses.</span></span> <span data-ttu-id="51853-140">[HttpClient. DeleteAsync](xref:System.Net.Http.HttpClient.DeleteAsync*) , BIR Web API 'SINE http delete isteği göndermek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="51853-140">[HttpClient.DeleteAsync](xref:System.Net.Http.HttpClient.DeleteAsync*) is used to send an HTTP DELETE request to a web API.</span></span>

<span data-ttu-id="51853-141">Aşağıdaki kodda, DELETE `<button>` öğesi `DeleteItem` yöntemini çağırır.</span><span class="sxs-lookup"><span data-stu-id="51853-141">In the following code, the Delete `<button>` element calls the `DeleteItem` method.</span></span> <span data-ttu-id="51853-142">Bağlantılı `<input>` öğesi, silinecek öğenin `id` sağlar.</span><span class="sxs-lookup"><span data-stu-id="51853-142">The bound `<input>` element supplies the `id` of the item to delete.</span></span> <span data-ttu-id="51853-143">Örnek uygulamaya bkz. örnek uygulama.</span><span class="sxs-lookup"><span data-stu-id="51853-143">See the sample app for a complete example.</span></span>

```cshtml
@using System.Net.Http
@inject HttpClient Http

<input @bind="_id" />
<button @onclick="@DeleteItem">Delete</button>

@code {
    private long _id;

    private async Task DeleteItem() =>
        await Http.DeleteAsync($"api/TodoItems/{_id}");
}
```

## <a name="cross-origin-resource-sharing-cors"></a><span data-ttu-id="51853-144">Çıkış noktaları arası kaynak paylaşımı (CORS)</span><span class="sxs-lookup"><span data-stu-id="51853-144">Cross-origin resource sharing (CORS)</span></span>

<span data-ttu-id="51853-145">Tarayıcı güvenliği, bir Web sayfasının, Web sayfasını sunduğundan farklı bir etki alanına istek yapmasını engeller.</span><span class="sxs-lookup"><span data-stu-id="51853-145">Browser security prevents a webpage from making requests to a different domain than the one that served the webpage.</span></span> <span data-ttu-id="51853-146">Bu kısıtlamaya *aynı-Origin ilkesi*adı verilir.</span><span class="sxs-lookup"><span data-stu-id="51853-146">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="51853-147">Aynı-kaynak ilkesi, kötü niyetli bir sitenin gizli verileri başka bir siteden okumasını engeller.</span><span class="sxs-lookup"><span data-stu-id="51853-147">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="51853-148">Tarayıcıdan farklı bir kaynağa sahip bir uç noktaya istek yapmak için *uç noktanın* , [çıkış noktaları arası kaynak PAYLAŞıMı 'nı (CORS)](https://www.w3.org/TR/cors/)etkinleştirmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="51853-148">To make requests from the browser to an endpoint with a different origin, the *endpoint* must enable [cross-origin resource sharing (CORS)](https://www.w3.org/TR/cors/).</span></span>

<span data-ttu-id="51853-149">Örnek uygulama, Web API bileşeninde CORS 'nin kullanımını gösterir (*Pages/CallWebAPI. Razor*).</span><span class="sxs-lookup"><span data-stu-id="51853-149">The sample app demonstrates the use of CORS in the Call Web API component (*Pages/CallWebAPI.razor*).</span></span>

<span data-ttu-id="51853-150">Diğer sitelerin uygulamanıza çıkış noktaları arası kaynak paylaşımı (CORS) istekleri yapmasına izin vermek için <xref:security/cors> ' a bakın.</span><span class="sxs-lookup"><span data-stu-id="51853-150">To allow other sites to make cross-origin resource sharing (CORS) requests to your app, see <xref:security/cors>.</span></span>

## <a name="httpclient-and-httprequestmessage-with-fetch-api-request-options"></a><span data-ttu-id="51853-151">API istek seçeneklerini getiren HttpClient ve HttpRequestMessage</span><span class="sxs-lookup"><span data-stu-id="51853-151">HttpClient and HttpRequestMessage with Fetch API request options</span></span>

<span data-ttu-id="51853-152">Blazor WebAssembly uygulamasında WebAssembly üzerinde çalışırken, istekleri özelleştirmek için [HttpClient](xref:fundamentals/http-requests) ve <xref:System.Net.Http.HttpRequestMessage> kullanın.</span><span class="sxs-lookup"><span data-stu-id="51853-152">When running on WebAssembly in a Blazor WebAssembly app, use [HttpClient](xref:fundamentals/http-requests) and <xref:System.Net.Http.HttpRequestMessage> to customize requests.</span></span> <span data-ttu-id="51853-153">Örneğin, istek URI 'SI, HTTP yöntemi ve istenen istek üst bilgilerini belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="51853-153">For example, you can specify the request URI, HTTP method, and any desired request headers.</span></span>

<span data-ttu-id="51853-154">İstek üzerinde `WebAssemblyHttpMessageHandler.FetchArgs` özelliğini kullanarak temel alınan JavaScript [GETIRME API](https://developer.mozilla.org/docs/Web/API/Fetch_API) 'sine istek seçenekleri sağlayın.</span><span class="sxs-lookup"><span data-stu-id="51853-154">Supply request options to the underlying JavaScript [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) using the `WebAssemblyHttpMessageHandler.FetchArgs` property on the request.</span></span> <span data-ttu-id="51853-155">Aşağıdaki örnekte gösterildiği gibi, `credentials` özelliği aşağıdaki değerlerden herhangi birine ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="51853-155">As shown in the following example, the `credentials` property is set to any of the following values:</span></span>

* <span data-ttu-id="51853-156">`FetchCredentialsOption.Include` ("include") &ndash;, tarayıcıya olan kimlik bilgilerini (örneğin, tanımlama bilgileri veya HTTP kimlik doğrulama üstbilgileri), çapraz kaynak istekleri için de gönderir.</span><span class="sxs-lookup"><span data-stu-id="51853-156">`FetchCredentialsOption.Include` ("include") &ndash; Advises the browser to send credentials (such as cookies or HTTP authentication headers) even for cross-origin requests.</span></span> <span data-ttu-id="51853-157">Yalnızca CORS ilkesi kimlik bilgilerine izin verecek şekilde yapılandırıldığında izin verilir.</span><span class="sxs-lookup"><span data-stu-id="51853-157">Only allowed when the CORS policy is configured to allow credentials.</span></span>
* <span data-ttu-id="51853-158">`FetchCredentialsOption.Omit` ("atla") &ndash;, tarayıcıya kimlik bilgileri (örneğin, tanımlama bilgileri veya HTTP kimlik doğrulama üstbilgileri) göndermemeyi önerir.</span><span class="sxs-lookup"><span data-stu-id="51853-158">`FetchCredentialsOption.Omit` ("omit") &ndash; Advises the browser never to send credentials (such as cookies or HTTP auth headers).</span></span>
* <span data-ttu-id="51853-159">`FetchCredentialsOption.SameOrigin` ("aynı-Origin") &ndash;, yalnızca hedef URL, çağıran uygulamayla aynı kaynak üzerinde olduğunda kimlik bilgilerini (tanımlama bilgileri veya HTTP kimlik doğrulama üstbilgileri gibi) göndermek için tarayıcıya sahip olur.</span><span class="sxs-lookup"><span data-stu-id="51853-159">`FetchCredentialsOption.SameOrigin` ("same-origin") &ndash; Advises the browser to send credentials (such as cookies or HTTP auth headers) only if the target URL is on the same origin as the calling application.</span></span>

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
            RequestUri = new Uri("https://localhost:10000/api/TodoItems"),
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

<span data-ttu-id="51853-160">API seçenekleri getirme hakkında daha fazla bilgi için bkz. [MDN Web belgeleri: WindowOrWorkerGlobalScope. Fetch ():P arameters](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters).</span><span class="sxs-lookup"><span data-stu-id="51853-160">For more information on Fetch API options, see [MDN web docs: WindowOrWorkerGlobalScope.fetch():Parameters](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters).</span></span>

<span data-ttu-id="51853-161">CORS isteklerindeki kimlik bilgileri (yetkilendirme tanımlama bilgileri/üstbilgileri) gönderilirken, CORS ilkesi tarafından `Authorization` üst bilgisine izin verilmelidir.</span><span class="sxs-lookup"><span data-stu-id="51853-161">When sending credentials (authorization cookies/headers) on CORS requests, the `Authorization` header must be allowed by the CORS policy.</span></span>

<span data-ttu-id="51853-162">Aşağıdaki ilke için yapılandırma içerir:</span><span class="sxs-lookup"><span data-stu-id="51853-162">The following policy includes configuration for:</span></span>

* <span data-ttu-id="51853-163">İstek kaynakları (`http://localhost:5000`, `https://localhost:5001`).</span><span class="sxs-lookup"><span data-stu-id="51853-163">Request origins (`http://localhost:5000`, `https://localhost:5001`).</span></span>
* <span data-ttu-id="51853-164">Any yöntemi (fiil).</span><span class="sxs-lookup"><span data-stu-id="51853-164">Any method (verb).</span></span>
* <span data-ttu-id="51853-165">`Content-Type` ve `Authorization` üst bilgileri.</span><span class="sxs-lookup"><span data-stu-id="51853-165">`Content-Type` and `Authorization` headers.</span></span> <span data-ttu-id="51853-166">Özel bir üstbilgiye (örneğin, `x-custom-header`) izin vermek için, <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> çağrılırken üstbilgiyi listeleyin.</span><span class="sxs-lookup"><span data-stu-id="51853-166">To allow a custom header (for example, `x-custom-header`), list the header when calling <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>.</span></span>
* <span data-ttu-id="51853-167">İstemci tarafı JavaScript kodu tarafından ayarlanan kimlik bilgileri (`credentials` özelliği `include` olarak ayarlanır).</span><span class="sxs-lookup"><span data-stu-id="51853-167">Credentials set by client-side JavaScript code (`credentials` property set to `include`).</span></span>

```csharp
app.UseCors(policy => 
    policy.WithOrigins("http://localhost:5000", "https://localhost:5001")
    .AllowAnyMethod()
    .WithHeaders(HeaderNames.ContentType, HeaderNames.Authorization, "x-custom-header")
    .AllowCredentials());
```

<span data-ttu-id="51853-168">Daha fazla bilgi için bkz. <xref:security/cors> ve örnek uygulamanın HTTP Isteği Sınayıcısı bileşeni (*Bileşenler/HTTPRequestTester. Razor*).</span><span class="sxs-lookup"><span data-stu-id="51853-168">For more information, see <xref:security/cors> and the sample app's HTTP Request Tester component (*Components/HTTPRequestTester.razor*).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="51853-169">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="51853-169">Additional resources</span></span>

* <xref:fundamentals/http-requests>
* <xref:security/enforcing-ssl>
* [<span data-ttu-id="51853-170">Kestrel HTTPS uç noktası yapılandırması</span><span class="sxs-lookup"><span data-stu-id="51853-170">Kestrel HTTPS endpoint configuration</span></span>](xref:fundamentals/servers/kestrel#endpoint-configuration)
* [<span data-ttu-id="51853-171">W3C üzerinde çapraz kaynak kaynak paylaşımı (CORS)</span><span class="sxs-lookup"><span data-stu-id="51853-171">Cross Origin Resource Sharing (CORS) at W3C</span></span>](https://www.w3.org/TR/cors/)
