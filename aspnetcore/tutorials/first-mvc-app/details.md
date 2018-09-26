---
title: Ayrıntılarını inceleyin ve Delete metotlarını bir ASP.NET Core uygulaması
author: rick-anderson
description: Ayrıntıları denetleyicisi yöntem hakkında bilgi edinin ve temel bir ASP.NET Core MVC uygulamasında görüntüleyebilirsiniz.
ms.author: riande
ms.date: 03/07/2017
uid: tutorials/first-mvc-app/details
ms.openlocfilehash: 9864abb54483c0ccf911aaf704a1beae007b32a4
ms.sourcegitcommit: 599ebae5c2d6fcb22dfa6ae7d1f4bdfcacb79af4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2018
ms.locfileid: "47211019"
---
# <a name="examine-the-details-and-delete-methods-of-an-aspnet-core-app"></a>Ayrıntılarını inceleyin ve Delete metotlarını bir ASP.NET Core uygulaması

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Film denetleyicisi açın ve incelemek `Details` yöntemi:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](start-mvc/sample/MvcMovie21/Controllers/MoviesController.cs?name=snippet_details)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_details)]

::: moniker-end

Bu eylem yöntemine oluşturulan MVC yapı iskelesi altyapısı yöntemi çağıran bir HTTP isteği gösteren bir yorum ekler. Bu durumda, üç URL kesimleri ile bir GET isteği, `Movies` denetleyicisi `Details` yöntemi ve bir `id` değeri. Bu kesimler tanımlanmış geri çağırma *Startup.cs*.

[!code-csharp[](start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

EF kullanarak verileri için arama yapmayı kolaylaştırır `SingleOrDefaultAsync` yöntemi. Yönteme yerleşik bir önemli güvenlik kod arama yöntemi şey denemeden önce bir filmi buldu doğrular özelliğidir. Örneğin, bir bilgisayar korsanının hataları siteye bağlantılardan tarafından oluşturulan URL değiştirerek neden olabilirdi `http://localhost:xxxx/Movies/Details/1` gibi bir şey `http://localhost:xxxx/Movies/Details/12345` (veya gerçek bir film temsil etmez başka bir değer). Null film denetlemedi, uygulama bir özel durumlar oluşturan.

İnceleme `Delete` ve `DeleteConfirmed` yöntemleri.

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](start-mvc/sample/MvcMovie21/Controllers/MoviesController.cs?name=snippet_delete)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete)]

::: moniker-end

Unutmayın `HTTP GET Delete` yöntemi belirtilen film silme değil, film bir görünümünü (HttpPost) gönderebileceği silme işlemi geri döndürür. Bir GET'e yanıt olarak bir silme işlemi gerçekleştirme isteği (veya bir düzenleme işlemini gerçekleştirirken bu konular için işlem veya veriler değiştiğinde başka bir işlem oluşturun) bir güvenlik boşluğu açılır.

`[HttpPost]` Verilerini siler yöntemi adlı `DeleteConfirmed` HTTP POST yöntemi için benzersiz bir imza veya ad vermek için. İki yöntem imzaları aşağıda verilmiştir:

[!code-csharp[](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete2)]

[!code-csharp[](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete3)]


Ortak dil çalışma zamanı (CLR) aşırı yüklenmiş yöntemler benzersiz parametre imzası (yöntemi aynı ada ancak farklı parametre listesi) olmasını gerektirir. Bununla birlikte, burada iki gerekir `Delete` --Get--diğeri POST yöntemleri her ikisi de aynı parametre imzasına sahip. (Her ikisi de tek bir tamsayı bir parametre olarak kabul etmeniz gerekir.)

Bu sorunun iki yaklaşım vardır, yöntemleri farklı adlar vermek için biridir. Önceki örnekte yapı iskelesi mekanizması ne yaptığını olmasıdır. Ancak, bu küçük bir sorunla sunar: ASP.NET eylem yöntemleri için bir URL kesimleri adıyla eşler ve bir yöntem yeniden adlandırırsanız, normal olarak Yönlendirme bu yöntem bulmak saptayamazdınız. Eklenecek olan örnekte gördüğünüz çözümüdür `ActionName("Delete")` özniteliğini `DeleteConfirmed` yöntemi. Bu öznitelik bir POST isteği için /Delete/ içeren bir URL bulabilirsiniz, böylece eşleme için yönlendirme sistem gerçekleştirir. `DeleteConfirmed` yöntemi.

Bir ek eklemek için POST yöntemini imzası yapay olarak değiştirmek için başka bir yaygın geçici aynı adlara ve imzaları olan yöntemler için olan (kullanılmayan) parametre. Eklediğimiz olduğunda ne önceki gönderide yaptığımız olan `notUsed` parametresi. Aynı şeyi burada yapabilirsiniz `[HttpPost] Delete` yöntemi:

```csharp
// POST: Movies/Delete/6
[ValidateAntiForgeryToken]
public async Task<IActionResult> Delete(int id, bool notUsed)
```

### <a name="publish-to-azure"></a>Azure'da yayımlama

Bkz: Azure'a dağıtma hakkında bilgi [öğretici: azure'da SQL veritabanı ile ASP.NET uygulaması derleme](/azure/app-service/app-service-web-tutorial-dotnet-sqldatabase). Yönergesi, bir ASP.NET uygulaması için bir ASP.NET Core uygulaması olan ancak adımlar aynıdır.

> [!div class="step-by-step"]
> [Önceki](validation.md)
