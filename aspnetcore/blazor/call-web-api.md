---
title: ASP.NET Core Blazor 'ten bir Web API 'SI çağırma
author: guardrex
description: "\"Çıkış noktaları arası kaynak paylaşımı (CORS) istekleri de dahil olmak üzere JSON yardımcıları kullanarak bir Blazor uygulamasından Web API 'SI çağırmayı öğrenin."
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/13/2019
uid: blazor/call-web-api
ms.openlocfilehash: 60ebd01bc07da22cd1dcd0b16297ee54c97867fc
ms.sourcegitcommit: f5f0ff65d4e2a961939762fb00e654491a2c772a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/15/2019
ms.locfileid: "69030381"
---
# <a name="call-a-web-api-from-aspnet-core-blazor"></a>ASP.NET Core Blazor 'ten bir Web API 'SI çağırma

, [Luke Latham](https://github.com/guardrex) ve [Daniel Roth](https://github.com/danroth27) tarafından

Blazor istemci tarafı uygulamalar, önceden yapılandırılmış `HttpClient` bir hizmeti kullanarak Web API 'lerini çağırır. Blazor JSON yardımcıları veya ile <xref:System.Net.Http.HttpRequestMessage>kullanarak JavaScript [getirme API 'si](https://developer.mozilla.org/docs/Web/API/Fetch_API) seçeneklerini içerebilen oluşturma istekleri.

Blazor sunucu tarafı uygulamalar, genellikle kullanılarak <xref:System.Net.Http.HttpClient> <xref:System.Net.Http.IHttpClientFactory>oluşturulan örnekleri kullanarak Web API 'lerini çağırır. Daha fazla bilgi için bkz. <xref:fundamentals/http-requests>.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

Blazor istemci tarafı örnekleri için örnek uygulamada aşağıdaki bileşenlere bakın:

* Web API çağrısı (*Pages/CallWebAPI. Razor*)
* HTTP Isteği Sınayıcısı (*Bileşenler/HTTPRequestTester. Razor*)

## <a name="httpclient-and-json-helpers"></a>HttpClient ve JSON yardımcıları

Blazor istemci tarafı uygulamalarda, [HttpClient](xref:fundamentals/http-requests) , istekleri kaynak sunucuya geri getirmek için önceden yapılandırılmış bir hizmet olarak kullanılabilir. JSON yardımcıları `HttpClient` kullanmak için, öğesine `Microsoft.AspNetCore.Blazor.HttpClient`bir paket başvurusu ekleyin. `HttpClient`ve JSON yardımcıları, üçüncü taraf Web API uç noktalarını çağırmak için de kullanılır. `HttpClient`, tarayıcı [getirme API 'si](https://developer.mozilla.org/docs/Web/API/Fetch_API) kullanılarak uygulanır ve aynı kaynak ilkesini zorlama dahil olmak üzere sınırlamalarına tabidir.

İstemcinin temel adresi, kaynak sunucunun adresine ayarlanır. Yönergesini kullanarak bir `HttpClient` örnek ekleme: `@inject`

```cshtml
@using System.Net.Http
@inject HttpClient Http
```

Aşağıdaki örneklerde, bir Todo Web API 'SI oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemlerini işler. Örnekler şunları depolayan bir `TodoItem` sınıfa dayalıdır:

* Öğenin kimliği`Id`( `long`, &ndash; ) benzersiz kimliği.
* Öğenin adı`Name`( `string`, &ndash; ).
* Todo öğesi`IsComplete`tamamlandığında `bool`durum &ndash; (,) göstergesi.

```csharp
private class TodoItem
{
    public long Id { get; set; }
    public string Name { get; set; }
    public bool IsComplete { get; set; }
}
```

JSON yardımcı yöntemleri bir URI 'ye (aşağıdaki örneklerde bir Web API 'si) istek gönderir ve yanıtı işler:

* `GetJsonAsync`&ndash; Bir http get isteği gönderir ve bir nesne oluşturmak için JSON yanıt gövdesini ayrıştırır.

  Aşağıdaki kodda,, `_todoItems` bileşeni tarafından görüntülenir. Yöntemi, bileşen işlemeyi tamamladığında tetiklenir ([onınitializedadsync).](xref:blazor/components#lifecycle-methods) `GetTodoItems` Örnek uygulamaya bkz. örnek uygulama.

  ```cshtml
  @using System.Net.Http
  @inject HttpClient Http

  @code {
      private TodoItem[] _todoItems;

      protected override async Task OnInitializedAsync() => 
          _todoItems = await Http.GetJsonAsync<TodoItem[]>("api/todo");
  }
  ```

* `PostJsonAsync`&ndash; JSON kodlu içerik dahil olmak üzere bir http post isteği gönderir ve bir nesne oluşturmak için JSON yanıt gövdesini ayrıştırır.

  Aşağıdaki kodda, `_newItemName` bileşenin bağlantılı bir öğesi tarafından sağlanır. `AddItem` Yöntemi bir`<button>` öğesi seçilerek tetiklenir. Örnek uygulamaya bkz. örnek uygulama.

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

* `PutJsonAsync`&ndash; JSON kodlu içerik dahil olmak üzere bir http put isteği gönderir.

  Aşağıdaki kodda, `_editItem` `Name` ve `IsCompleted` değerleri bileşenin bağlantılı öğelerine göre sağlanır. Öğe, Kullanıcı arabiriminin başka bir bölümünde seçildiğinde ayarlanır ve `EditItem` çağrılır. `Id` Yöntemi, Kaydet `<button>` öğesi seçilerek tetiklenir. `SaveItem` Örnek uygulamaya bkz. örnek uygulama.

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

<xref:System.Net.Http>HTTP istekleri göndermeye ve HTTP yanıtlarını almaya yönelik ek uzantı yöntemleri içerir. [HttpClient. DeleteAsync](xref:System.Net.Http.HttpClient.DeleteAsync*) , BIR Web API 'SINE http delete isteği göndermek için kullanılır.

Aşağıdaki kodda, Delete `<button>` öğesi `DeleteItem` yöntemini çağırır. Bağlantılı `<input>` öğe, silinecek öğenin `id` bir listesini sağlar. Örnek uygulamaya bkz. örnek uygulama.

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

Tarayıcı güvenliği, bir Web sayfasının, Web sayfasını sunduğundan farklı bir etki alanına istek yapmasını engeller. Bu kısıtlamaya *aynı-Origin ilkesi*adı verilir. Aynı-kaynak ilkesi, kötü niyetli bir sitenin gizli verileri başka bir siteden okumasını engeller. Tarayıcıdan farklı bir kaynağa sahip bir uç noktaya istek yapmak için *uç noktanın* , [çıkış noktaları arası kaynak PAYLAŞıMı 'nı (CORS)](https://www.w3.org/TR/cors/)etkinleştirmesi gerekir.

Örnek uygulama, Web API bileşeninde CORS 'nin kullanımını gösterir (*Pages/CallWebAPI. Razor*).

Diğer sitelerin uygulamanıza çıkış noktaları arası kaynak paylaşımı (CORS) istekleri yapmasına izin vermek için bkz <xref:security/cors>.

## <a name="httpclient-and-httprequestmessage-with-fetch-api-request-options"></a>API istek seçeneklerini getiren HttpClient ve HttpRequestMessage

Blazor istemci tarafı uygulamasında webassembly üzerinde çalışırken, [HttpClient](xref:fundamentals/http-requests) kullanın ve <xref:System.Net.Http.HttpRequestMessage> istekleri özelleştirin. Örneğin, istek URI 'SI, HTTP yöntemi ve istenen istek üst bilgilerini belirtebilirsiniz.

İstek üzerindeki `WebAssemblyHttpMessageHandler.FetchArgs` özelliği kullanarak temeldeki JavaScript [getirme API](https://developer.mozilla.org/docs/Web/API/Fetch_API) 'sine yönelik istek seçeneklerini sağlayın. Aşağıdaki örnekte gösterildiği gibi, `credentials` özelliği aşağıdaki değerlerden herhangi birine ayarlanır:

* `FetchCredentialsOption.Include`("Ekle") &ndash; Çapraz kaynak istekleri için bile, tarayıcıya kimlik bilgileri (örneğin, tanımlama bilgileri veya http kimlik doğrulama üstbilgileri) göndermesini önerir. Yalnızca CORS ilkesi kimlik bilgilerine izin verecek şekilde yapılandırıldığında izin verilir.
* `FetchCredentialsOption.Omit`("atla") &ndash; Tarayıcıya kimlik bilgileri (örneğin, tanımlama bilgileri veya http kimlik doğrulama üstbilgileri) göndermemeyi önerir.
* `FetchCredentialsOption.SameOrigin`("aynı kaynak") &ndash; Yalnızca hedef URL, çağıran uygulamayla aynı kaynak üzerinde olduğunda kimlik bilgileri (tanımlama bilgileri veya http kimlik doğrulama üstbilgileri gibi) göndermek için tarayıcıya sahip olur.

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

API seçenekleri getirme hakkında daha fazla bilgi için bkz [. MDN Web belgeleri: WindowOrWorkerGlobalScope. Fetch ():P arametre](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters).

CORS isteklerindeki kimlik bilgileri (yetkilendirme tanımlama bilgileri/üstbilgileri) gönderilirken, `Authorization` bir üst bilgiye CORS ilkesi tarafından izin verilmelidir.

Aşağıdaki ilke için yapılandırma içerir:

* İstek kaynakları (`http://localhost:5000`, `https://localhost:5001`).
* Any yöntemi (fiil).
* `Content-Type`ve `Authorization` üst bilgiler. Özel bir üstbilgiye (örneğin, `x-custom-header`) izin vermek için, çağrılırken <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>üstbilgiyi listeleyin.
* İstemci tarafı JavaScript kodu tarafından ayarlanan kimlik bilgileri (`credentials` özellik olarak `include`ayarlanır).

```csharp
app.UseCors(policy => 
    policy.WithOrigins("http://localhost:5000", "https://localhost:5001")
    .AllowAnyMethod()
    .WithHeaders(HeaderNames.ContentType, HeaderNames.Authorization, "x-custom-header")
    .AllowCredentials());
```

Daha fazla bilgi için, <xref:security/cors> bkz. ve örnek uygulamanın http isteği Sınayıcısı bileşeni (*Bileşenler/httprequesttester. Razor*).

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:fundamentals/http-requests>
* [W3C üzerinde çapraz kaynak kaynak paylaşımı (CORS)](https://www.w3.org/TR/cors/)
