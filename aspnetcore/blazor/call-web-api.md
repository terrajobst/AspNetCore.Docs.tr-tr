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
# <a name="call-a-web-api-from-aspnet-core-blazor"></a>ASP.NET Core Blazor web API'si çağırma

Tarafından [Luke Latham](https://github.com/guardrex) ve [Daniel Roth](https://github.com/danroth27)

Blazor istemci tarafı uygulamalar, web API'leri kullanarak önceden yapılandırılmış bir çağrı `HttpClient` hizmeti. JavaScript içerebilen istekler compose [Fetch API'sini](https://developer.mozilla.org/docs/Web/API/Fetch_API) seçenekleri, Blazor JSON Yardımcıları kullanarak veya <xref:System.Net.Http.HttpRequestMessage>.

Blazor sunucu tarafı uygulamalar çağrı web API'lerini kullanarak <xref:System.Net.Http.HttpClient> genellikle kullanılarak oluşturulan örnekleri <xref:System.Net.Http.IHttpClientFactory>. Daha fazla bilgi için bkz. <xref:fundamentals/http-requests>.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

Blazor istemci-tarafı örnekleri için örnek uygulama aşağıdaki bileşenlerde bakın:

* Web API'si çağırma (*Pages/CallWebAPI.razor*)
* HTTP isteği Tester (*Components/HTTPRequestTester.razor*)

## <a name="httpclient-and-json-helpers"></a>HttpClient ve JSON Yardımcıları

Blazor istemci-tarafı uygulamalarında [HttpClient](xref:fundamentals/http-requests) istekleri kaynak sunucuya geri yapmak için önceden yapılandırılmış bir hizmet olarak kullanılabilir. `HttpClient` ve JSON Yardımcıları üçüncü taraf web API uç noktalarına çağrı için de kullanılır. `HttpClient` tarayıcı kullanılarak uygulanan [Fetch API'sini](https://developer.mozilla.org/docs/Web/API/Fetch_API) ve aynı çıkış noktası ilkesi zorlama dahil olmak üzere kendi sınırlamalara tabi.

İstemcinin temel adresini kaynak sunucu adresi olarak ayarlanır. Ekleme bir `HttpClient` kullanarak `@inject` yönergesi:

```cshtml
@using System.Net.Http
@inject HttpClient Http
```

Aşağıdaki örneklerde, bir Todo web API işlemleri oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemleri. Örnekleri temel alan bir `TodoItem` depolayan sınıfı:

* Kimliği (`Id`, `long`) &ndash; öğesinin benzersiz kimliği.
* Ad (`Name`, `string`) &ndash; öğesinin adı.
* Durum (`IsComplete`, `bool`) &ndash; Todo öğesini tamamlandıysa göstergesi.

```csharp
private class TodoItem
{
    public long Id { get; set; }
    public string Name { get; set; }
    public bool IsComplete { get; set; }
}
```

JSON yardımcı yöntemler bir URI (bir web API'sini aşağıdaki örneklerde) isteklerini göndermek ve yanıtın işlem:

* `GetJsonAsync` &ndash; Bir HTTP GET isteği gönderir ve bir nesne oluşturmak için JSON yanıt gövdesini ayrıştırır.

  Aşağıdaki kodda, `_todoItems` bileşen tarafından görüntülenir. `GetTodoItems` Yöntemi, bileşenin tamamlandığında tetiklenir işleme ([OnInitAsync](xref:blazor/components#lifecycle-methods)). Örnek uygulamayı tam bir örnek için bkz.

  ```cshtml
  @using System.Net.Http
  @inject HttpClient Http

  @code {
      private TodoItem[] _todoItems;

      protected override async Task OnInitAsync() => 
          _todoItems = await Http.GetJsonAsync<TodoItem[]>("api/todo");
  }
  ```

* `PostJsonAsync` &ndash; JSON ile kodlanmış içeriği de dahil olmak üzere bir HTTP POST isteği gönderir ve bir nesne oluşturmak için JSON yanıt gövdesini ayrıştırır.

  Aşağıdaki kodda, `_newItemName` bağlı bir bileşen öğesi tarafından sağlanır. `AddItem` Yöntemi seçerek tetiklenir bir `<button>` öğesi. Örnek uygulamayı tam bir örnek için bkz.

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

* `PutJsonAsync` &ndash; JSON ile kodlanmış içeriği de dahil olmak üzere bir HTTP PUT İsteği gönderir.

  Aşağıdaki kodda, `_editItem` değerleri `Name` ve `IsCompleted` bileşenin ilişkili öğeler tarafından sağlanır. Öğenin `Id` öğe başka bir kullanıcı Arabirimi kısmında seçildiğinde ayarlanır ve `EditItem` çağrılır. `SaveItem` Yöntemi kaydetme seçerek tetiklenir `<button>` öğesi. Örnek uygulamayı tam bir örnek için bkz.

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

<xref:System.Net.Http> HTTP istekleri göndermek ve HTTP yanıtlarını almak için ek genişletme yöntemleri içerir. [HttpClient.DeleteAsync](xref:System.Net.Http.HttpClient.DeleteAsync*) bir web API'sine HTTP DELETE isteği göndermek için kullanılır.

Aşağıdaki kodda, silme `<button>` öğesi çağrıları `DeleteItem` yöntemi. Bağımlı `<input>` öğesi sağlayan `id` öğesi silinemedi. Örnek uygulamayı tam bir örnek için bkz.

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

## <a name="cross-origin-resource-sharing-cors"></a>Çıkış noktaları arası kaynak paylaşımı (CORS)

Tarayıcı güvenlik bir Web sayfası, Web sayfasında sunulan olandan farklı bir etki alanı istekleri yapmasını engeller. Bu kısıtlama adlı *aynı çıkış noktası İlkesi*. Aynı kaynak İlkesi, kötü niyetli site başka bir siteden hassas verileri okumasını önler. Farklı bir kaynaktan bir uç nokta için tarayıcıdan istekler yapmasını *uç nokta* etkinleştirmelisiniz [çıkış noktaları arası kaynak paylaşımı (CORS)](https://www.w3.org/TR/cors/).

Örnek uygulaması Web API çağrısı bileşen CORS kullanımını gösterir (*Pages/CallWebAPI.razor*).

Çıkış noktaları arası kaynak paylaşımı (CORS) istekleri uygulamanıza sağlamak için diğer sitelere izin vermek için bkz: <xref:security/cors>.

## <a name="httpclient-and-httprequestmessage-with-fetch-api-request-options"></a>HttpClient ve getirme API'siyle HttpRequestMessage istek seçenekleri

WebAssembly üzerinde bir Blazor istemci-tarafı uygulaması çalışırken kullanması [HttpClient](xref:fundamentals/http-requests) ve <xref:System.Net.Http.HttpRequestMessage> istekleri özelleştirmek için. Örneğin, istek URI'si, HTTP yöntemi ve istenen istek üst bilgileri belirtebilirsiniz.

Temel alınan JavaScript için istek seçenekleri sağlamak [Fetch API'sini](https://developer.mozilla.org/docs/Web/API/Fetch_API) kullanarak `WebAssemblyHttpMessageHandler.FetchArgs` istek özelliği. Aşağıdaki örnekte gösterildiği gibi `credentials` özelliği şu değerlerden birine ayarlanır:

* `FetchCredentialsOption.Include` ("ekleme") &ndash; Bile çıkış noktaları arası istekleri (örneğin, tanımlama veya HTTP kimlik doğrulama üst bilgileri) kimlik bilgilerini göndermek için tarayıcı önerir. CORS ilkesini kimlik bilgilerine izin verecek şekilde yapılandırıldığında, yalnızca izin verilir.
* `FetchCredentialsOption.Omit` ("Atla") &ndash; Hiçbir zaman kimlik bilgilerini (örneğin, tanımlama veya HTTP kimlik doğrulama üst bilgileri) göndermek için tarayıcı önerir.
* `FetchCredentialsOption.SameOrigin` ("aynı çıkış noktası") &ndash; Yalnızca hedef URL'ye çağıran uygulama olarak aynı kaynak üzerinde ise (örneğin, tanımlama veya HTTP kimlik doğrulama üst bilgileri) kimlik bilgilerini göndermek için tarayıcı önerir.

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

Fetch API'sini seçenekleri hakkında daha fazla bilgi için bkz. [MDN web belgeleri: WindowOrWorkerGlobalScope.fetch():Parameters](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters).

Kimlik bilgilerini (yetkilendirme tanımlama/üst bilgiler), CORS istekleri gönderirken `Authorization` üstbilgi CORS ilkesinin izin verilmelidir.

Aşağıdaki ilke için yapılandırma içerir:

* İstek kaynakları (`http://localhost:5000`, `https://localhost:5001`).
* Herhangi bir yöntem (Fiili).
* `Content-Type` ve `Authorization` üstbilgileri. Özel bir başlık izin vermek için (örneğin, `x-custom-header`), üstbilgi çağırırken listesinde <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>.
* Kimlik bilgileri kümesi ile istemci tarafı JavaScript kodu (`credentials` özelliğini `include`).

```csharp
app.UseCors(policy => 
    policy.WithOrigins("http://localhost:5000", "https://localhost:5001")
    .AllowAnyMethod()
    .WithHeaders(HeaderNames.ContentType, HeaderNames.Authorization, "x-custom-header")
    .AllowCredentials());
```

Daha fazla bilgi için <xref:security/cors> ve örnek uygulamanın HTTP isteği Tester bileşeni (*Components/HTTPRequestTester.razor*).

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:fundamentals/http-requests>
* [Çıkış noktaları kaynak paylaşımı (CORS) W3C arası](https://www.w3.org/TR/cors/)
