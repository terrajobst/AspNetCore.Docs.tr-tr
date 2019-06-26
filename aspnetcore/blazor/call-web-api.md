---
title: ASP.NET Core Blazor web API'si çağırma
author: guardrex
description: Çıkış noktaları arası kaynak paylaşımı (CORS) istekleri yapma dahil olmak üzere JSON Yardımcıları kullanarak Blazor uygulamasından web API'si çağırma hakkında bilgi edinin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 06/25/2019
uid: blazor/call-web-api
ms.openlocfilehash: 1a13f9f1f9e660b39a1df584e49198c4bbb61533
ms.sourcegitcommit: 47cc13ab90913af9a2887cef0896bb4e9aba4dd5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2019
ms.locfileid: "67399181"
---
# <a name="call-a-web-api-from-aspnet-core-blazor"></a><span data-ttu-id="56e97-103">ASP.NET Core Blazor web API'si çağırma</span><span class="sxs-lookup"><span data-stu-id="56e97-103">Call a web API from ASP.NET Core Blazor</span></span>

<span data-ttu-id="56e97-104">Tarafından [Luke Latham](https://github.com/guardrex) ve [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="56e97-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="56e97-105">Blazor istemci tarafı uygulamalar, web API'leri kullanarak önceden yapılandırılmış bir çağrı `HttpClient` hizmeti.</span><span class="sxs-lookup"><span data-stu-id="56e97-105">Blazor client-side apps call web APIs using a preconfigured `HttpClient` service.</span></span> <span data-ttu-id="56e97-106">JavaScript içerebilen istekler compose [Fetch API'sini](https://developer.mozilla.org/docs/Web/API/Fetch_API) seçenekleri, Blazor JSON Yardımcıları kullanarak veya <xref:System.Net.Http.HttpRequestMessage>.</span><span class="sxs-lookup"><span data-stu-id="56e97-106">Compose requests, which can include JavaScript [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) options, using Blazor JSON helpers or with <xref:System.Net.Http.HttpRequestMessage>.</span></span>

<span data-ttu-id="56e97-107">Blazor sunucu tarafı uygulamalar çağrı web API'lerini kullanarak <xref:System.Net.Http.HttpClient> genellikle kullanılarak oluşturulan örnekleri <xref:System.Net.Http.IHttpClientFactory>.</span><span class="sxs-lookup"><span data-stu-id="56e97-107">Blazor server-side apps call web APIs using <xref:System.Net.Http.HttpClient> instances typically created using <xref:System.Net.Http.IHttpClientFactory>.</span></span> <span data-ttu-id="56e97-108">Daha fazla bilgi için bkz. <xref:fundamentals/http-requests>.</span><span class="sxs-lookup"><span data-stu-id="56e97-108">For more information, see <xref:fundamentals/http-requests>.</span></span>

<span data-ttu-id="56e97-109">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="56e97-109">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="56e97-110">Blazor istemci-tarafı örnekleri için örnek uygulama aşağıdaki bileşenlerde bakın:</span><span class="sxs-lookup"><span data-stu-id="56e97-110">For Blazor client-side examples, see the following components in the sample app:</span></span>

* <span data-ttu-id="56e97-111">Web API'si çağırma (*Pages/CallWebAPI.razor*)</span><span class="sxs-lookup"><span data-stu-id="56e97-111">Call Web API (*Pages/CallWebAPI.razor*)</span></span>
* <span data-ttu-id="56e97-112">HTTP isteği Tester (*Components/HTTPRequestTester.razor*)</span><span class="sxs-lookup"><span data-stu-id="56e97-112">HTTP Request Tester (*Components/HTTPRequestTester.razor*)</span></span>

## <a name="httpclient-and-json-helpers"></a><span data-ttu-id="56e97-113">HttpClient ve JSON Yardımcıları</span><span class="sxs-lookup"><span data-stu-id="56e97-113">HttpClient and JSON helpers</span></span>

<span data-ttu-id="56e97-114">Blazor istemci-tarafı uygulamalarında [HttpClient](xref:fundamentals/http-requests) istekleri kaynak sunucuya geri yapmak için önceden yapılandırılmış bir hizmet olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="56e97-114">In Blazor client-side apps, [HttpClient](xref:fundamentals/http-requests) is available as a preconfigured service for making requests back to the origin server.</span></span> <span data-ttu-id="56e97-115">`HttpClient` ve JSON Yardımcıları üçüncü taraf web API uç noktalarına çağrı için de kullanılır.</span><span class="sxs-lookup"><span data-stu-id="56e97-115">`HttpClient` and JSON helpers are also used to call third-party web API endpoints.</span></span> <span data-ttu-id="56e97-116">`HttpClient` tarayıcı kullanılarak uygulanan [Fetch API'sini](https://developer.mozilla.org/docs/Web/API/Fetch_API) ve aynı çıkış noktası ilkesi zorlama dahil olmak üzere kendi sınırlamalara tabi.</span><span class="sxs-lookup"><span data-stu-id="56e97-116">`HttpClient` is implemented using the browser [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) and is subject to its limitations, including enforcement of the same origin policy.</span></span>

<span data-ttu-id="56e97-117">İstemcinin temel adresini kaynak sunucu adresi olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="56e97-117">The client's base address is set to the originating server's address.</span></span> <span data-ttu-id="56e97-118">Ekleme bir `HttpClient` kullanarak `@inject` yönergesi:</span><span class="sxs-lookup"><span data-stu-id="56e97-118">Inject an `HttpClient` instance using the `@inject` directive:</span></span>

```cshtml
@using System.Net.Http
@inject HttpClient Http
```

<span data-ttu-id="56e97-119">Aşağıdaki örneklerde, bir Todo web API işlemleri oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemleri.</span><span class="sxs-lookup"><span data-stu-id="56e97-119">In the following examples, a Todo web API processes create, read, update, and delete (CRUD) operations.</span></span> <span data-ttu-id="56e97-120">Örnekleri temel alan bir `TodoItem` depolayan sınıfı:</span><span class="sxs-lookup"><span data-stu-id="56e97-120">The examples are based on a `TodoItem` class that stores the:</span></span>

* <span data-ttu-id="56e97-121">Kimliği (`Id`, `long`) &ndash; öğesinin benzersiz kimliği.</span><span class="sxs-lookup"><span data-stu-id="56e97-121">ID (`Id`, `long`) &ndash; Unique ID of the item.</span></span>
* <span data-ttu-id="56e97-122">Ad (`Name`, `string`) &ndash; öğesinin adı.</span><span class="sxs-lookup"><span data-stu-id="56e97-122">Name (`Name`, `string`) &ndash; Name of the item.</span></span>
* <span data-ttu-id="56e97-123">Durum (`IsComplete`, `bool`) &ndash; Todo öğesini tamamlandıysa göstergesi.</span><span class="sxs-lookup"><span data-stu-id="56e97-123">Status (`IsComplete`, `bool`) &ndash; Indication if the Todo item is finished.</span></span>

```csharp
private class TodoItem
{
    public long Id { get; set; }
    public string Name { get; set; }
    public bool IsComplete { get; set; }
}
```

<span data-ttu-id="56e97-124">JSON yardımcı yöntemler bir URI (bir web API'sini aşağıdaki örneklerde) isteklerini göndermek ve yanıtın işlem:</span><span class="sxs-lookup"><span data-stu-id="56e97-124">JSON helper methods send requests to a URI (a web API in the following examples) and process the response:</span></span>

* <span data-ttu-id="56e97-125">`GetJsonAsync` &ndash; Bir HTTP GET isteği gönderir ve bir nesne oluşturmak için JSON yanıt gövdesini ayrıştırır.</span><span class="sxs-lookup"><span data-stu-id="56e97-125">`GetJsonAsync` &ndash; Sends an HTTP GET request and parses the JSON response body to create an object.</span></span>

  <span data-ttu-id="56e97-126">Aşağıdaki kodda, `_todoItems` bileşen tarafından görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="56e97-126">In the following code, the `_todoItems` are displayed by the component.</span></span> <span data-ttu-id="56e97-127">`GetTodoItems` Yöntemi, bileşenin tamamlandığında tetiklenir işleme ([OnInitAsync](xref:blazor/components#lifecycle-methods)).</span><span class="sxs-lookup"><span data-stu-id="56e97-127">The `GetTodoItems` method is triggered when the component is finished rendering ([OnInitAsync](xref:blazor/components#lifecycle-methods)).</span></span> <span data-ttu-id="56e97-128">Örnek uygulamayı tam bir örnek için bkz.</span><span class="sxs-lookup"><span data-stu-id="56e97-128">See the sample app for a complete example.</span></span>

  ```cshtml
  @using System.Net.Http
  @inject HttpClient Http

  @code {
      private TodoItem[] _todoItems;

      protected override async Task OnInitAsync() => 
          _todoItems = await Http.GetJsonAsync<TodoItem[]>("api/todo");
  }
  ```

* <span data-ttu-id="56e97-129">`PostJsonAsync` &ndash; JSON ile kodlanmış içeriği de dahil olmak üzere bir HTTP POST isteği gönderir ve bir nesne oluşturmak için JSON yanıt gövdesini ayrıştırır.</span><span class="sxs-lookup"><span data-stu-id="56e97-129">`PostJsonAsync` &ndash; Sends an HTTP POST request, including JSON-encoded content, and parses the JSON response body to create an object.</span></span>

  <span data-ttu-id="56e97-130">Aşağıdaki kodda, `_newItemName` bağlı bir bileşen öğesi tarafından sağlanır.</span><span class="sxs-lookup"><span data-stu-id="56e97-130">In the following code, `_newItemName` is provided by a bound element of the component.</span></span> <span data-ttu-id="56e97-131">`AddItem` Yöntemi seçerek tetiklenir bir `<button>` öğesi.</span><span class="sxs-lookup"><span data-stu-id="56e97-131">The `AddItem` method is triggered by selecting a `<button>` element.</span></span> <span data-ttu-id="56e97-132">Örnek uygulamayı tam bir örnek için bkz.</span><span class="sxs-lookup"><span data-stu-id="56e97-132">See the sample app for a complete example.</span></span>

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

* <span data-ttu-id="56e97-133">`PutJsonAsync` &ndash; JSON ile kodlanmış içeriği de dahil olmak üzere bir HTTP PUT İsteği gönderir.</span><span class="sxs-lookup"><span data-stu-id="56e97-133">`PutJsonAsync` &ndash; Sends an HTTP PUT request, including JSON-encoded content.</span></span>

  <span data-ttu-id="56e97-134">Aşağıdaki kodda, `_editItem` değerleri `Name` ve `IsCompleted` bileşenin ilişkili öğeler tarafından sağlanır.</span><span class="sxs-lookup"><span data-stu-id="56e97-134">In the following code, `_editItem` values for `Name` and `IsCompleted` are provided by bound elements of the component.</span></span> <span data-ttu-id="56e97-135">Öğenin `Id` öğe başka bir kullanıcı Arabirimi kısmında seçildiğinde ayarlanır ve `EditItem` çağrılır.</span><span class="sxs-lookup"><span data-stu-id="56e97-135">The item's `Id` is set when the item is selected in another part of the UI and `EditItem` is called.</span></span> <span data-ttu-id="56e97-136">`SaveItem` Yöntemi kaydetme seçerek tetiklenir `<button>` öğesi.</span><span class="sxs-lookup"><span data-stu-id="56e97-136">The `SaveItem` method is triggered by selecting the Save `<button>` element.</span></span> <span data-ttu-id="56e97-137">Örnek uygulamayı tam bir örnek için bkz.</span><span class="sxs-lookup"><span data-stu-id="56e97-137">See the sample app for a complete example.</span></span>

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

<span data-ttu-id="56e97-138"><xref:System.Net.Http> HTTP istekleri göndermek ve HTTP yanıtlarını almak için ek genişletme yöntemleri içerir.</span><span class="sxs-lookup"><span data-stu-id="56e97-138"><xref:System.Net.Http> includes additional extension methods for sending HTTP requests and receiving HTTP responses.</span></span> <span data-ttu-id="56e97-139">[HttpClient.DeleteAsync](xref:System.Net.Http.HttpClient.DeleteAsync*) bir web API'sine HTTP DELETE isteği göndermek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="56e97-139">[HttpClient.DeleteAsync](xref:System.Net.Http.HttpClient.DeleteAsync*) is used to send an HTTP DELETE request to a web API.</span></span>

<span data-ttu-id="56e97-140">Aşağıdaki kodda, silme `<button>` öğesi çağrıları `DeleteItem` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="56e97-140">In the following code, the Delete `<button>` element calls the `DeleteItem` method.</span></span> <span data-ttu-id="56e97-141">Bağımlı `<input>` öğesi sağlayan `id` öğesi silinemedi.</span><span class="sxs-lookup"><span data-stu-id="56e97-141">The bound `<input>` element supplies the `id` of the item to delete.</span></span> <span data-ttu-id="56e97-142">Örnek uygulamayı tam bir örnek için bkz.</span><span class="sxs-lookup"><span data-stu-id="56e97-142">See the sample app for a complete example.</span></span>

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

## <a name="cross-origin-resource-sharing-cors"></a><span data-ttu-id="56e97-143">Çıkış noktaları arası kaynak paylaşımı (CORS)</span><span class="sxs-lookup"><span data-stu-id="56e97-143">Cross-origin resource sharing (CORS)</span></span>

<span data-ttu-id="56e97-144">Tarayıcı güvenlik bir Web sayfası, Web sayfasında sunulan olandan farklı bir etki alanı istekleri yapmasını engeller.</span><span class="sxs-lookup"><span data-stu-id="56e97-144">Browser security prevents a webpage from making requests to a different domain than the one that served the webpage.</span></span> <span data-ttu-id="56e97-145">Bu kısıtlama adlı *aynı çıkış noktası İlkesi*.</span><span class="sxs-lookup"><span data-stu-id="56e97-145">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="56e97-146">Aynı kaynak İlkesi, kötü niyetli site başka bir siteden hassas verileri okumasını önler.</span><span class="sxs-lookup"><span data-stu-id="56e97-146">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="56e97-147">Farklı bir kaynaktan bir uç nokta için tarayıcıdan istekler yapmasını *uç nokta* etkinleştirmelisiniz [çıkış noktaları arası kaynak paylaşımı (CORS)](https://www.w3.org/TR/cors/).</span><span class="sxs-lookup"><span data-stu-id="56e97-147">To make requests from the browser to an endpoint with a different origin, the *endpoint* must enable [cross-origin resource sharing (CORS)](https://www.w3.org/TR/cors/).</span></span>

<span data-ttu-id="56e97-148">Örnek uygulaması Web API çağrısı bileşen CORS kullanımını gösterir (*Pages/CallWebAPI.razor*).</span><span class="sxs-lookup"><span data-stu-id="56e97-148">The sample app demonstrates the use of CORS in the Call Web API component (*Pages/CallWebAPI.razor*).</span></span>

<span data-ttu-id="56e97-149">Çıkış noktaları arası kaynak paylaşımı (CORS) istekleri uygulamanıza sağlamak için diğer sitelere izin vermek için bkz: <xref:security/cors>.</span><span class="sxs-lookup"><span data-stu-id="56e97-149">To allow other sites to make cross-origin resource sharing (CORS) requests to your app, see <xref:security/cors>.</span></span>

## <a name="httpclient-and-httprequestmessage-with-fetch-api-request-options"></a><span data-ttu-id="56e97-150">HttpClient ve getirme API'siyle HttpRequestMessage istek seçenekleri</span><span class="sxs-lookup"><span data-stu-id="56e97-150">HttpClient and HttpRequestMessage with Fetch API request options</span></span>

<span data-ttu-id="56e97-151">WebAssembly üzerinde bir Blazor istemci-tarafı uygulaması çalışırken kullanması [HttpClient](xref:fundamentals/http-requests) ve <xref:System.Net.Http.HttpRequestMessage> istekleri özelleştirmek için.</span><span class="sxs-lookup"><span data-stu-id="56e97-151">When running on WebAssembly in a Blazor client-side app, use [HttpClient](xref:fundamentals/http-requests) and <xref:System.Net.Http.HttpRequestMessage> to customize requests.</span></span> <span data-ttu-id="56e97-152">Örneğin, istek URI'si, HTTP yöntemi ve istenen istek üst bilgileri belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="56e97-152">For example, you can specify the request URI, HTTP method, and any desired request headers.</span></span>

<span data-ttu-id="56e97-153">Temel alınan JavaScript için istek seçenekleri sağlamak [Fetch API'sini](https://developer.mozilla.org/docs/Web/API/Fetch_API) kullanarak `WebAssemblyHttpMessageHandler.FetchArgs` istek özelliği.</span><span class="sxs-lookup"><span data-stu-id="56e97-153">Supply request options to the underlying JavaScript [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) using the `WebAssemblyHttpMessageHandler.FetchArgs` property on the request.</span></span> <span data-ttu-id="56e97-154">Aşağıdaki örnekte gösterildiği gibi `credentials` özelliği şu değerlerden birine ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="56e97-154">As shown in the following example, the `credentials` property is set to any of the following values:</span></span>

* <span data-ttu-id="56e97-155">`FetchCredentialsOption.Include` ("ekleme") &ndash; Bile çıkış noktaları arası istekleri (örneğin, tanımlama veya HTTP kimlik doğrulama üst bilgileri) kimlik bilgilerini göndermek için tarayıcı önerir.</span><span class="sxs-lookup"><span data-stu-id="56e97-155">`FetchCredentialsOption.Include` ("include") &ndash; Advises the browser to send credentials (such as cookies or HTTP authentication headers) even for cross-origin requests.</span></span> <span data-ttu-id="56e97-156">CORS ilkesini kimlik bilgilerine izin verecek şekilde yapılandırıldığında, yalnızca izin verilir.</span><span class="sxs-lookup"><span data-stu-id="56e97-156">Only allowed when the CORS policy is configured to allow credentials.</span></span>
* <span data-ttu-id="56e97-157">`FetchCredentialsOption.Omit` ("Atla") &ndash; Hiçbir zaman kimlik bilgilerini (örneğin, tanımlama veya HTTP kimlik doğrulama üst bilgileri) göndermek için tarayıcı önerir.</span><span class="sxs-lookup"><span data-stu-id="56e97-157">`FetchCredentialsOption.Omit` ("omit") &ndash; Advises the browser never to send credentials (such as cookies or HTTP auth headers).</span></span>
* <span data-ttu-id="56e97-158">`FetchCredentialsOption.SameOrigin` ("aynı çıkış noktası") &ndash; Yalnızca hedef URL'ye çağıran uygulama olarak aynı kaynak üzerinde ise (örneğin, tanımlama veya HTTP kimlik doğrulama üst bilgileri) kimlik bilgilerini göndermek için tarayıcı önerir.</span><span class="sxs-lookup"><span data-stu-id="56e97-158">`FetchCredentialsOption.SameOrigin` ("same-origin") &ndash; Advises the browser to send credentials (such as cookies or HTTP auth headers) only if the target URL is on the same origin as the calling application.</span></span>

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

<span data-ttu-id="56e97-159">Fetch API'sini seçenekleri hakkında daha fazla bilgi için bkz. [MDN web belgeleri: WindowOrWorkerGlobalScope.fetch():Parameters](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters).</span><span class="sxs-lookup"><span data-stu-id="56e97-159">For more information on Fetch API options, see [MDN web docs: WindowOrWorkerGlobalScope.fetch():Parameters](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters).</span></span>

<span data-ttu-id="56e97-160">Kimlik bilgilerini (yetkilendirme tanımlama/üst bilgiler), CORS istekleri gönderirken `Authorization` üstbilgi CORS ilkesinin izin verilmelidir.</span><span class="sxs-lookup"><span data-stu-id="56e97-160">When sending credentials (authorization cookies/headers) on CORS requests, the `Authorization` header must be allowed by the CORS policy.</span></span>

<span data-ttu-id="56e97-161">Aşağıdaki ilke için yapılandırma içerir:</span><span class="sxs-lookup"><span data-stu-id="56e97-161">The following policy includes configuration for:</span></span>

* <span data-ttu-id="56e97-162">İstek kaynakları (`http://localhost:5000`, `https://localhost:5001`).</span><span class="sxs-lookup"><span data-stu-id="56e97-162">Request origins (`http://localhost:5000`, `https://localhost:5001`).</span></span>
* <span data-ttu-id="56e97-163">Herhangi bir yöntem (Fiili).</span><span class="sxs-lookup"><span data-stu-id="56e97-163">Any method (verb).</span></span>
* <span data-ttu-id="56e97-164">`Content-Type` ve `Authorization` üstbilgileri.</span><span class="sxs-lookup"><span data-stu-id="56e97-164">`Content-Type` and `Authorization` headers.</span></span> <span data-ttu-id="56e97-165">Özel bir başlık izin vermek için (örneğin, `x-custom-header`), üstbilgi çağırırken listesinde <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>.</span><span class="sxs-lookup"><span data-stu-id="56e97-165">To allow a custom header (for example, `x-custom-header`), list the header when calling <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>.</span></span>
* <span data-ttu-id="56e97-166">Kimlik bilgileri kümesi ile istemci tarafı JavaScript kodu (`credentials` özelliğini `include`).</span><span class="sxs-lookup"><span data-stu-id="56e97-166">Credentials set by client-side JavaScript code (`credentials` property set to `include`).</span></span>

```csharp
app.UseCors(policy => 
    policy.WithOrigins("http://localhost:5000", "https://localhost:5001")
    .AllowAnyMethod()
    .WithHeaders(HeaderNames.ContentType, HeaderNames.Authorization, "x-custom-header")
    .AllowCredentials());
```

<span data-ttu-id="56e97-167">Daha fazla bilgi için <xref:security/cors> ve örnek uygulamanın HTTP isteği Tester bileşeni (*Components/HTTPRequestTester.razor*).</span><span class="sxs-lookup"><span data-stu-id="56e97-167">For more information, see <xref:security/cors> and the sample app's HTTP Request Tester component (*Components/HTTPRequestTester.razor*).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="56e97-168">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="56e97-168">Additional resources</span></span>

* <xref:fundamentals/http-requests>
* [<span data-ttu-id="56e97-169">Çıkış noktaları kaynak paylaşımı (CORS) W3C arası</span><span class="sxs-lookup"><span data-stu-id="56e97-169">Cross Origin Resource Sharing (CORS) at W3C</span></span>](https://www.w3.org/TR/cors/)
