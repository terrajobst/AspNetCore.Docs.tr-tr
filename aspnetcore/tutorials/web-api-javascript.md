---
title: "Öğretici: JavaScript ile ASP.NET Core Web API 'SI çağırma"
author: rick-anderson
description: JavaScript ile ASP.NET Core Web API 'sini çağırmayı öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 08/27/2019
uid: tutorials/web-api-javascript
ms.openlocfilehash: 0070816149d64fc1d71d453eb0f135050c78597a
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/13/2019
ms.locfileid: "72378698"
---
# <a name="tutorial-call-an-aspnet-core-web-api-with-javascript"></a>Öğretici: JavaScript ile ASP.NET Core Web API 'SI çağırma

[Rick Anderson](https://twitter.com/RickAndMSFT) tarafından

Bu öğreticide, [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API)kullanılarak JavaScript ile ASP.NET Core Web API 'sinin nasıl çağrılacağını gösterilmektedir.

::: moniker range="< aspnetcore-3.0"

ASP.NET Core 2,2 için, [JavaScript ile Web API 'Sini çağırma](xref:tutorials/first-web-api#call-the-web-api-with-javascript)2,2 sürümüne bakın.

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

## <a name="prerequisites"></a>Prerequisites

* Tüm [öğreticiyi: Web API 'Si oluşturma](xref:tutorials/first-web-api)
* CSS, HTML ve JavaScript ile benzerlik

## <a name="call-the-web-api-with-javascript"></a>JavaScript ile Web API 'sini çağırma

Bu bölümde, Yapılacaklar öğeleri oluşturmak ve yönetmek için form içeren bir HTML sayfası ekleyeceksiniz. Olay işleyicileri sayfadaki öğelere iliştirilir. Olay işleyicileri, Web API 'sinin eylem yöntemlerine HTTP istekleri sonucu vermez. Fetch API 'sinin `fetch` işlevi her HTTP isteğini başlatır.

`fetch` işlevi, bir `Response` nesnesi olarak temsil edilen bir HTTP yanıtı içeren bir `Promise` nesnesi döndürür. Ortak bir model, `Response` nesnesinde `json` işlevini çağırarak JSON yanıt gövdesini ayıklamaya yönelik olur. JavaScript, sayfayı Web API 'sinin yanıtından alınan ayrıntılarla güncelleştirir.

En basit `fetch` çağrısı, yolu temsil eden tek bir parametre kabul eder. `init` nesnesi olarak bilinen ikinci bir parametre isteğe bağlıdır. `init`, HTTP isteğini yapılandırmak için kullanılır.

1. Uygulamayı [statik dosyaları sunacak](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) ve [varsayılan dosya eşlemesini etkinleştirecek](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_)şekilde yapılandırın. Aşağıdaki vurgulanan kod, *Startup.cs*öğesinin `Configure` yönteminde gereklidir:

    [!code-csharp[](first-web-api/samples/3.0/TodoApi/StartupJavaScript.cs?highlight=8-9&name=snippet_configure)]

1. Proje kökünde bir *Wwwroot* dizini oluşturun.

1. *Wwwroot* dizinine *index. HTML* adlı bir HTML dosyası ekleyin. İçeriğini aşağıdaki biçimlendirmeyle değiştirin:

    [!code-html[](first-web-api/samples/3.0/TodoApi/wwwroot/index.html)]

1. *Wwwroot* dizinine *site. js* adlı bir JavaScript dosyası ekleyin. İçeriğini şu kodla değiştirin:

    [!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/js/site.js?name=snippet_SiteJs)]

HTML sayfasını yerel olarak test etmek için ASP.NET Core projesinin başlatma ayarlarındaki bir değişikliğin yapılması gerekebilir:

1. *Properties\launchSettings.JSON*'i açın.
1. Uygulamanın *index. html* &mdash;the projenin varsayılan dosyasında açılmasını zorlamak için `launchUrl` özelliğini kaldırın.

Bu örnek, Web API 'sinin tüm CRUD yöntemlerini çağırır. Web API isteklerinin açıklamaları aşağıda verilmiştir.

### <a name="get-a-list-of-to-do-items"></a>Yapılacaklar öğelerinin bir listesini alın

Aşağıdaki kodda, *API/todoıtems* yoluna BIR http get isteği gönderilir:

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/js/site.js?name=snippet_GetItems)]

Web API 'SI başarılı bir durum kodu döndürdüğünde `_displayItems` işlevi çağrılır. `_displayItems` tarafından kabul edilen dizi parametresindeki her Yapılacaklar öğesi, **Düzenle** ve **Sil** düğmeleriyle bir tabloya eklenir. Web API isteği başarısız olursa, tarayıcının konsoluna bir hata kaydedilir.

### <a name="add-a-to-do-item"></a>Yapılacaklar öğesi ekleme

Aşağıdaki kodda:

* `item` değişken, yapılacaklar öğesinin nesne sabit gösterimini oluşturmak için bildirilmiştir.
* Aşağıdaki seçeneklerle bir getirme isteği yapılandırılır:
    * `method`&mdash;HTTP eyleminin SONRASı fiilini belirtir.
    * `body`&mdash;, istek gövdesinin JSON temsilini belirtir. JSON, `item` ' da depolanan nesne sabit değeri [JSON. stringbelirt](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify) işlevine geçirilerek oluşturulur.
    * `headers`&mdash;`Accept` ve `Content-Type` HTTP istek üst bilgilerini belirtir. Her iki üst bilgi de `application/json` olarak ayarlanır ve sırasıyla alınan ve gönderilen medya türünü belirtir.
* *API/todoıtems* yoluna BIR http post isteği gönderilir.

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/js/site.js?name=snippet_AddItem)]

Web API 'SI başarılı bir durum kodu döndürdüğünde, HTML tablosunu güncelleştirmek için `getItems` işlevi çağrılır. Web API isteği başarısız olursa, tarayıcının konsoluna bir hata kaydedilir.

### <a name="update-a-to-do-item"></a>Yapılacaklar öğesini güncelleştirme

Bir yapılacaklar öğesinin güncelleştirilmesi bir tane eklemeye benzer; Ancak, iki önemli fark vardır:

* Yol, güncelleştirilecek öğenin benzersiz tanımlayıcısı ile sone düzeltildi. Örneğin, *api/todoıtems/1*.
* HTTP eylemi fiili, `method` seçeneğinde gösterildiği gibi konur.

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/js/site.js?name=snippet_UpdateItem)]

### <a name="delete-a-to-do-item"></a>Bir yapılacaklar öğesini silme

Bir yapılacaklar öğesini silmek için, isteğin `method` seçeneğini `DELETE` olarak ayarlayın ve URL 'de öğenin benzersiz tanımlayıcısını belirtin.

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/js/site.js?name=snippet_DeleteItem)]

Web API 'SI yardım sayfaları oluşturmayı öğrenmek için bir sonraki öğreticiye ilerleyin:

> [!div class="nextstepaction"]
> <xref:tutorials/get-started-with-swashbuckle>

::: moniker-end
