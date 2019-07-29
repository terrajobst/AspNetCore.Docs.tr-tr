---
title: "Öğretici: ASP.NET Core kullanarak jQuery ile Web API 'SI çağırma"
author: rick-anderson
description: JQuery ile ASP.NET Core Web API 'sinin nasıl çağrılacağını öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 07/20/2019
uid: tutorials/web-api-jquery
ms.openlocfilehash: eb8b2453fd037170a49f531fea4c3ef1c056292d
ms.sourcegitcommit: 0efb9e219fef481dee35f7b763165e488aa6cf9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2019
ms.locfileid: "68602587"
---
# <a name="tutorial-call-an-aspnet-core-web-api-with-jquery"></a>Öğretici: JQuery ile ASP.NET Core Web API 'SI çağırma

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Bu öğreticide, jQuery ile ASP.NET Core Web API 'sinin nasıl çağrılacağını gösterilmektedir

::: moniker range="< aspnetcore-3.0"

ASP.NET Core 2,2 için, [Web API 'Sini jQuery Ile çağırma](xref:tutorials/first-web-api#call-the-api-with-jquery)2,2 sürümüne bakın.

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

## <a name="prerequisites"></a>Önkoşullar

Tüm [öğreticiyi: Web API 'SI oluşturma](xref:tutorials/first-web-api)

## <a name="call-the-api-with-jquery"></a>JQuery ile API çağırma

Bu bölümde, Web'i çağırmaya jQuery kullanan bir HTML sayfasına eklenen API. jQuery isteği başlatır ve API'nin yanıt Ayrıntıları sayfası güncelleştirir.

Uygulamayı [statik dosyalara sunacak](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) şekilde yapılandırın ve aşağıdaki vurgulanmış kodla *Startup.cs* güncelleştirerek [varsayılan dosya eşlemesini etkinleştirin](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) :

[!code-csharp[](first-web-api/samples/3.0/TodoApi/StartupJquery.cs?highlight=8-9&name=snippet_configure)]

Oluşturma bir *wwwroot* proje dizininde klasör.

Adlı bir HTML dosyası ekleyin *index.html* için *wwwroot* dizin. Dosyanın içeriğini aşağıdaki biçimlendirme ile değiştirin:

[!code-html[](first-web-api/samples/3.0/TodoApi/wwwroot/index.html)]

Adlı bir JavaScript dosyası ekleyin *site.js* için *wwwroot* dizin. Dosyanın içeriğini aşağıdaki kodla değiştirin:

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

ASP.NET Core proje başlatma ayarlarında bir değişiklik HTML sayfasını yerel olarak test etmek için gerekli:

* Açık *Properties\launchSettings.json*.
* Kaldırma `launchUrl` , açmak için uygulamayı zorlamak için özellik *index.html*&mdash;projenin varsayılan dosya.

JQuery almak için birkaç yolu vardır. Yukarıdaki kod parçacığında, bir CDN kitaplığı yüklenir.

Bu örnek, tüm API CRUD yöntemleri çağırır. API çağrıları açıklamaları aşağıda verilmiştir.

### <a name="get-a-list-of-to-do-items"></a>Yapılacaklar öğelerinin bir listesini alın

JQuery [ajax](https://api.jquery.com/jquery.ajax/) işlev gönderen bir `GET` Yapılacaklar öğelerini dizisini temsil eden JSON döndüren API isteği. `success` İstek başarılı olursa geri çağırma işlevi çağrılır. Geri çağırma içinde DOM Yapılacaklar bilgilerle güncelleştirilir.

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a>Yapılacak İş Öğesi Ekle

[Ajax](https://api.jquery.com/jquery.ajax/) işlev gönderen bir `POST` istek ile istek gövdesindeki yapılacak iş öğesi. `accepts` Ve `contentType` seçeneklerini ayarlamak `application/json` gönderilen ve alınan medya türü belirtmek için. Yapılacak iş öğesi kullanarak JSON'a dönüştürülür [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify). API'yi bir başarılı durum kodu döndürdüğünde `getData` işlevi HTML tablosu güncelleştirmek için çağrılır.

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a>Yapılacak iş öğesini güncelleştirme

Yapılacak iş öğesi güncelleştirilirken bir eklemeye benzerdir. `url` Öğenin benzersiz tanıtıcısı eklemek için değişiklikleri ve `type` olduğu `PUT`.

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a>Yapılacak iş öğesi silme

Yapılacak iş öğesi silme gerçekleştirilir ayarlayarak `type` AJAX çağrısı hedefi üzerinde `DELETE` ve öğenin benzersiz tanıtıcısı URL'yi belirterek.

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]

API Yardım sayfaları oluşturma hakkında bilgi edinmek için sonraki öğreticiye ilerleyin:

> [!div class="nextstepaction"]
> <xref:tutorials/get-started-with-swashbuckle>
::: moniker-end