---
title: "Öğretici: JavaScript ile ASP.NET Core Web API 'SI çağırma"
author: rick-anderson
description: JavaScript ile ASP.NET Core Web API 'sini çağırmayı öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 11/26/2019
uid: tutorials/web-api-javascript
ms.openlocfilehash: 2a19a7d16ca8b8f5d6ac8eb99ad919b89f1e368b
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78655255"
---
# <a name="tutorial-call-an-aspnet-core-web-api-with-javascript"></a>Öğretici: JavaScript ile ASP.NET Core Web API 'SI çağırma

Gönderen [Rick Anderson](https://twitter.com/RickAndMSFT)

Bu öğreticide, [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API)kullanılarak JavaScript ile ASP.NET Core Web API 'sinin nasıl çağrılacağını gösterilmektedir.

::: moniker range="< aspnetcore-3.0"

ASP.NET Core 2,2 için, [JavaScript ile Web API 'Sini çağırma](xref:tutorials/first-web-api#call-the-web-api-with-javascript)2,2 sürümüne bakın.

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

## <a name="prerequisites"></a>Önkoşullar

* Tüm [öğreticiyi: Web API 'Si oluşturma](xref:tutorials/first-web-api)
* CSS, HTML ve JavaScript ile benzerlik

## <a name="call-the-web-api-with-javascript"></a>JavaScript ile Web API 'sini çağırma

Bu bölümde, Yapılacaklar öğeleri oluşturmak ve yönetmek için form içeren bir HTML sayfası ekleyeceksiniz. Olay işleyicileri sayfadaki öğelere iliştirilir. Olay işleyicileri, Web API 'sinin eylem yöntemlerine HTTP istekleri sonucu vermez. Fetch API 'sinin `fetch` işlevi her HTTP isteğini başlatır.

`fetch` işlevi, bir `Response` nesnesi olarak temsil edilen HTTP yanıtını içeren bir [Promise](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise) nesnesi döndürür. Ortak bir model, `Response` nesnesinde `json` işlevini çağırarak JSON yanıt gövdesini ayıklamaya yönelik bir modeldir. JavaScript, sayfayı Web API 'sinin yanıtından alınan ayrıntılarla güncelleştirir.

En basit `fetch` çağrısı, yolu temsil eden tek bir parametreyi kabul eder. `init` nesnesi olarak bilinen ikinci bir parametre isteğe bağlıdır. `init` HTTP isteğini yapılandırmak için kullanılır.

1. Uygulamayı [statik dosyaları sunacak](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) ve [varsayılan dosya eşlemesini etkinleştirecek](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_)şekilde yapılandırın. Aşağıdaki vurgulanan kod *Startup.cs*`Configure` yönteminde gereklidir:

    [!code-csharp[](first-web-api/samples/3.0/TodoApi/StartupJavaScript.cs?highlight=8-9&name=snippet_configure)]

1. Proje kökünde bir *Wwwroot* klasörü oluşturun.

1. *Wwwroot* klasörü içinde bir *js* klasörü oluşturun.

1. *Wwwroot* klasörüne *index. HTML* adlı bir HTML dosyası ekleyin. *İndex. html* içeriğini aşağıdaki biçimlendirme ile değiştirin:

    [!code-html[](first-web-api/samples/3.0/TodoApi/wwwroot/index.html)]

1. *Wwwroot/js* klasörüne *site. js* adlı bir JavaScript dosyası ekleyin. *Site. js* içeriğini aşağıdaki kodla değiştirin:

    [!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/js/site.js?name=snippet_SiteJs)]

ASP.NET Core proje başlatma ayarlarında bir değişiklik HTML sayfasını yerel olarak test etmek için gerekli:

1. *Properties\launchSettings.JSON*'i açın.
1. Uygulamayı, projenin varsayılan dosyası&mdash;*Dizin. html* ' de açmaya zorlamak için `launchUrl` özelliğini kaldırın.

Bu örnek, Web API 'sinin tüm CRUD yöntemlerini çağırır. Web API isteklerinin açıklamaları aşağıda verilmiştir.

### <a name="get-a-list-of-to-do-items"></a>Yapılacaklar öğelerinin bir listesini alın

Aşağıdaki kodda, *API/todoıtems* yoluna BIR http get isteği gönderilir:

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/js/site.js?name=snippet_GetItems)]

Web API 'SI başarılı bir durum kodu döndürdüğünde `_displayItems` işlevi çağrılır. `_displayItems` tarafından kabul edilen dizi parametresindeki her Yapılacaklar öğesi, **Düzenle** ve **Sil** düğmeleriyle bir tabloya eklenir. Web API isteği başarısız olursa, tarayıcının konsoluna bir hata kaydedilir.

### <a name="add-a-to-do-item"></a>Yapılacak İş Öğesi Ekle

Aşağıdaki kodda:

* `item` değişken, yapılacaklar öğesinin nesne sabit gösterimini oluşturmak için bildirilmiştir.
* Aşağıdaki seçeneklerle bir getirme isteği yapılandırılır:
  * `method`&mdash;HTTP eyleminin SONRASı fiilini belirtir.
  * `body`&mdash;, istek gövdesinin JSON temsilini belirtir. JSON, `item` ' de depolanan nesne sabit değeri [JSON. stringbelirt](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify) işlevine geçirilerek oluşturulur.
  * `headers`&mdash;`Accept` ve `Content-Type` HTTP istek üst bilgilerini belirtir. Her iki üst bilgi de `application/json` olarak ayarlanır ve sırasıyla, alınan ve gönderilen medya türünü belirtir.
* *API/todoıtems* yoluna BIR http post isteği gönderilir.

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/js/site.js?name=snippet_AddItem)]

Web API 'SI başarılı bir durum kodu döndürdüğünde, `getItems` işlevi HTML tablosunu güncelleştirmek için çağrılır. Web API isteği başarısız olursa, tarayıcının konsoluna bir hata kaydedilir.

### <a name="update-a-to-do-item"></a>Yapılacak iş öğesini güncelleştirme

Bir yapılacaklar öğesinin güncelleştirilmesi bir tane eklemeye benzer; Ancak, iki önemli fark vardır:

* Yol, güncelleştirilecek öğenin benzersiz tanımlayıcısı ile sone düzeltildi. Örneğin, *api/todoıtems/1*.
* HTTP eylemi fiili, `method` seçeneği tarafından belirtilen şekilde konur.

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/js/site.js?name=snippet_UpdateItem)]

### <a name="delete-a-to-do-item"></a>Yapılacak iş öğesi silme

Bir yapılacaklar öğesini silmek için, isteğin `method` seçeneğini `DELETE` olarak ayarlayın ve URL 'de öğenin benzersiz tanımlayıcısını belirtin.

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/js/site.js?name=snippet_DeleteItem)]

Web API 'SI yardım sayfaları oluşturmayı öğrenmek için bir sonraki öğreticiye ilerleyin:

> [!div class="nextstepaction"]
> <xref:tutorials/get-started-with-swashbuckle>

::: moniker-end
