---
title: "Öğretici: JavaScript ile ASP.NET Core Web API 'SI çağırma"
author: rick-anderson
description: JavaScript ile ASP.NET Core Web API 'sini çağırmayı öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 08/27/2019
uid: tutorials/web-api-javascript
ms.openlocfilehash: 0070816149d64fc1d71d453eb0f135050c78597a
ms.sourcegitcommit: de17150e5ec7507d7114dde0e5dbc2e45a66ef53
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70116657"
---
# <a name="tutorial-call-an-aspnet-core-web-api-with-javascript"></a>Öğretici: JavaScript ile ASP.NET Core Web API 'SI çağırma

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Bu öğreticide, [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API)kullanılarak JavaScript ile ASP.NET Core Web API 'sinin nasıl çağrılacağını gösterilmektedir.

::: moniker range="< aspnetcore-3.0"

ASP.NET Core 2,2 için, [JavaScript ile Web API 'Sini çağırma](xref:tutorials/first-web-api#call-the-web-api-with-javascript)2,2 sürümüne bakın.

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

## <a name="prerequisites"></a>Önkoşullar

* Tüm [öğreticiyi: Web API 'SI oluşturma](xref:tutorials/first-web-api)
* CSS, HTML ve JavaScript ile benzerlik

## <a name="call-the-web-api-with-javascript"></a>JavaScript ile Web API 'sini çağırma

Bu bölümde, Yapılacaklar öğeleri oluşturmak ve yönetmek için form içeren bir HTML sayfası ekleyeceksiniz. Olay işleyicileri sayfadaki öğelere iliştirilir. Olay işleyicileri, Web API 'sinin eylem yöntemlerine HTTP istekleri sonucu vermez. Fetch API 'nin `fetch` işlevi her http isteğini başlatır.

İşlevi, `Response` nesne olarak `Promise` temsil edilen bir http yanıtı içeren bir nesnesi döndürür. `fetch` Ortak bir model, `json` `Response` nesnesinde işlevini çağırarak JSON yanıt gövdesini ayıklamaya yönelik olur. JavaScript, sayfayı Web API 'sinin yanıtından alınan ayrıntılarla güncelleştirir.

En basit `fetch` çağrı, yolu temsil eden tek bir parametre kabul eder. `init` Nesne olarak bilinen ikinci bir parametre isteğe bağlıdır. `init`HTTP isteğini yapılandırmak için kullanılır.

1. Uygulamayı [statik dosyaları sunacak](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) ve [varsayılan dosya eşlemesini etkinleştirecek](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_)şekilde yapılandırın. Aşağıdaki vurgulanan kod, `Configure` *Startup.cs*yönteminde gereklidir:

    [!code-csharp[](first-web-api/samples/3.0/TodoApi/StartupJavaScript.cs?highlight=8-9&name=snippet_configure)]

1. Proje kökünde bir *Wwwroot* dizini oluşturun.

1. Adlı bir HTML dosyası ekleyin *index.html* için *wwwroot* dizin. Dosyanın içeriğini aşağıdaki biçimlendirme ile değiştirin:

    [!code-html[](first-web-api/samples/3.0/TodoApi/wwwroot/index.html)]

1. Adlı bir JavaScript dosyası ekleyin *site.js* için *wwwroot* dizin. Dosyanın içeriğini aşağıdaki kodla değiştirin:

    [!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/js/site.js?name=snippet_SiteJs)]

ASP.NET Core proje başlatma ayarlarında bir değişiklik HTML sayfasını yerel olarak test etmek için gerekli:

1. Açık *Properties\launchSettings.json*.
1. Kaldırma `launchUrl` , açmak için uygulamayı zorlamak için özellik *index.html*&mdash;projenin varsayılan dosya.

Bu örnek, Web API 'sinin tüm CRUD yöntemlerini çağırır. Web API isteklerinin açıklamaları aşağıda verilmiştir.

### <a name="get-a-list-of-to-do-items"></a>Yapılacaklar öğelerinin bir listesini alın

Aşağıdaki kodda, *API/todoıtems* yoluna BIR http get isteği gönderilir:

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/js/site.js?name=snippet_GetItems)]

Web API 'si başarılı bir durum kodu döndürdüğünde, `_displayItems` işlev çağrılır. Tarafından `_displayItems` kabul edilen dizi parametresindeki her Yapılacaklar öğesi, **Düzenle** ve **Sil** düğmeleri içeren bir tabloya eklenir. Web API isteği başarısız olursa, tarayıcının konsoluna bir hata kaydedilir.

### <a name="add-a-to-do-item"></a>Yapılacak İş Öğesi Ekle

Aşağıdaki kodda:

* Bir `item` değişken, yapılacaklar öğesinin nesne sabit gösterimini oluşturmak için bildirilmiştir.
* Aşağıdaki seçeneklerle bir getirme isteği yapılandırılır:
    * `method`&mdash;HTTP SONRASı eylem fiilini belirtir.
    * `body`&mdash;istek gövdesinin JSON temsilini belirtir. JSON, içinde `item` depolanan nesne sabit değeri [JSON. stringbelirt](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify) işlevine geçirilerek oluşturulur.
    * `headers`&mdash;`Accept` ve`Content-Type` http istek üst bilgilerini belirtir. Her iki üst bilgi, `application/json` sırasıyla alınan ve gönderilen medya türünü belirtmek için olarak ayarlanır.
* *API/todoıtems* yoluna BIR http post isteği gönderilir.

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/js/site.js?name=snippet_AddItem)]

Web API 'si başarılı bir durum kodu döndürdüğünde, `getItems` işlev HTML tablosunu güncelleştirmek için çağrılır. Web API isteği başarısız olursa, tarayıcının konsoluna bir hata kaydedilir.

### <a name="update-a-to-do-item"></a>Yapılacak iş öğesini güncelleştirme

Bir yapılacaklar öğesinin güncelleştirilmesi bir tane eklemeye benzer; Ancak, iki önemli fark vardır:

* Yol, güncelleştirilecek öğenin benzersiz tanımlayıcısı ile sone düzeltildi. Örneğin, *api/todoıtems/1*.
* HTTP eylemi fiili, `method` seçeneğinde gösterildiği gibi konur.

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/js/site.js?name=snippet_UpdateItem)]

### <a name="delete-a-to-do-item"></a>Yapılacak iş öğesi silme

Bir yapılacaklar öğesini silmek için isteğin `method` seçeneğini olarak `DELETE` ayarlayın ve URL 'de öğenin benzersiz tanımlayıcısını belirtin.

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/js/site.js?name=snippet_DeleteItem)]

Web API 'SI yardım sayfaları oluşturmayı öğrenmek için bir sonraki öğreticiye ilerleyin:

> [!div class="nextstepaction"]
> <xref:tutorials/get-started-with-swashbuckle>

::: moniker-end
