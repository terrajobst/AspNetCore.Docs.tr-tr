---
title: ASP.NET Core uygulamasının Ayrıntılar ve silme yöntemlerini inceleyin
author: rick-anderson
description: Ayrıntılar denetleyicisi yöntemi ve temel ASP.NET Core MVC uygulamasında görünüm hakkında bilgi edinin.
ms.author: riande
ms.date: 12/13/2018
uid: tutorials/first-mvc-app/details
ms.openlocfilehash: d19e8cdb63da2bb9c66db1943dfcec183d432401
ms.sourcegitcommit: 2719c70cd15a430479ab4007ff3e197fbf5dfee0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68862980"
---
# <a name="examine-the-details-and-delete-methods-of-an-aspnet-core-app"></a>ASP.NET Core uygulamasının Ayrıntılar ve silme yöntemlerini inceleyin

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Film denetleyicisini açın ve `Details` yöntemi inceleyin:

[!code-csharp[](start-mvc/sample/MvcMovie22/Controllers/MoviesController.cs?name=snippet_details)]

Bu eylem yöntemini oluşturan MVC yapı iskelesi altyapısı, yöntemi çağıran bir HTTP isteğini gösteren bir açıklama ekler. Bu durumda, üç URL segmentine, `Movies` denetleyiciye `Details` , yönteme ve bir `id` değere sahip bir get isteği olur. Bu kesimleri geri çağır *Startup.cs*içinde tanımlanmıştır.

[!code-csharp[](start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

EF, `FirstOrDefaultAsync` yöntemi kullanarak verileri aramanızı kolaylaştırır. Yöntemi içinde yerleşik olarak bulunan önemli bir güvenlik özelliği, kodun, onunla herhangi bir şey yapmayı denemeden önce arama yönteminin bir filmi buldığını doğrulamasından kaynaklanmaktadır. Örneğin, bir korsan bağlantıları `http://localhost:{PORT}/Movies/Details/1` tarafından oluşturulan URL 'yi (veya gerçek bir filmi temsil eden başka bir değer) gibi `http://localhost:{PORT}/Movies/Details/12345` bir konuma değiştirerek siteye hata verebilir. Null bir filmi denetmediyseniz, uygulama bir özel durum oluşturur.

`Delete` Ve`DeleteConfirmed` yöntemlerini inceleyin.

[!code-csharp[](start-mvc/sample/MvcMovie22/Controllers/MoviesController.cs?name=snippet_delete)]

`HTTP GET Delete` Yöntemin belirtilen filmi silmediğini unutmayın. Bu, silme işlemini gönderebileceğiniz (HttpPost) filmin bir görünümünü döndürür. Bir GET isteğine yanıt olarak silme işlemi gerçekleştirme (veya bu konuyla ilgili olarak, düzenleme işlemi gerçekleştirme, oluşturma işlemi yapma veya verileri değiştiren başka bir işlem) bir güvenlik deliği açılır.

Verileri silen `DeleteConfirmed` yöntemi, http post yöntemine benzersiz bir imza veya ad vermek için olarak adlandırılır. `[HttpPost]` İki yöntem imzası aşağıda gösterilmiştir:

[!code-csharp[](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete2)]

[!code-csharp[](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete3)]

Ortak dil çalışma zamanı (CLR), aşırı yüklenmiş yöntemlerin benzersiz bir parametre imzasına sahip olmasını gerektirir (aynı yöntem adı ancak farklı parametre listesi). Ancak, her ikisi de aynı `Delete` parametre imzasına sahip olan--get için bir tane olmak üzere iki yöntem gereklidir. (Her ikisi de parametre olarak tek bir tamsayıyı kabul etmelidir.)

Bu sorunun iki yaklaşımı vardır, biri yöntemlere farklı adlar vermektir. Bu, önceki örnekte bulunan yapı iskelesi mekanizmasına göre yapılır. Ancak, bu küçük bir sorun ortaya çıkarır: ASP.NET bir URL 'nin segmentlerini ada göre eylem yöntemlerine eşler ve bir yöntemi yeniden adlandırırsanız, yönlendirme normalde bu yöntemi bulamaz. Çözüm, örnekte gördüğünüz şeydir. Bu, `ActionName("Delete")` `DeleteConfirmed` yöntemine özniteliği eklemektir. Bu öznitelik, yönlendirme sistemi için eşleme gerçekleştirerek, bir post isteği için/Delete/içeren bir URL `DeleteConfirmed` yöntemi bulur.

Özdeş adlara ve imzalara sahip yöntemler için bir diğer yaygın çalışma yapay, POST yönteminin imzasını bir ek (kullanılmamış) parametre içerecek şekilde değiştirecek. Bu, `notUsed` parametreyi eklediğimiz sırada önceki bir gönderimiz tarafından yaptığımız. `[HttpPost] Delete` Yöntemi için burada aynı şeyi yapabilirsiniz:

```csharp
// POST: Movies/Delete/6
[HttpPost]
[ValidateAntiForgeryToken]
public async Task<IActionResult> Delete(int id, bool notUsed)
```

### <a name="publish-to-azure"></a>Azure'a Yayımlama

Azure 'a dağıtma hakkında bilgi için bkz [. Öğretici: Azure App Service](/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb)' de bir .NET Core ve SQL veritabanı Web uygulaması oluşturun.

> [!div class="step-by-step"]
> [Önceki](validation.md)
