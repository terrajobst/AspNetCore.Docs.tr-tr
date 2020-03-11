---
title: ASP.NET Core uygulamasının Ayrıntılar ve silme yöntemlerini inceleyin
author: rick-anderson
description: Ayrıntılar denetleyicisi yöntemi ve temel ASP.NET Core MVC uygulamasında görünüm hakkında bilgi edinin.
ms.author: riande
ms.date: 12/13/2018
uid: tutorials/first-mvc-app/details
ms.openlocfilehash: 04eb2efa4e67d84e575580a6248d0b5b567064af
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78662913"
---
# <a name="examine-the-details-and-delete-methods-of-an-aspnet-core-app"></a>ASP.NET Core uygulamasının Ayrıntılar ve silme yöntemlerini inceleyin

Gönderen [Rick Anderson](https://twitter.com/RickAndMSFT)

Film denetleyicisini açın ve `Details` yöntemi inceleyin:

[!code-csharp[](start-mvc/sample/MvcMovie22/Controllers/MoviesController.cs?name=snippet_details)]

Bu eylem yöntemini oluşturan MVC yapı iskelesi altyapısı, yöntemi çağıran bir HTTP isteğini gösteren bir açıklama ekler. Bu durumda, üç URL segmentine sahip bir GET isteği, `Movies` denetleyicisi, `Details` yöntemi ve bir `id` değeri. Bu kesimleri geri çağır *Startup.cs*içinde tanımlanmıştır.

[!code-csharp[](start-mvc/sample/MvcMovie3/Startup.cs?highlight=5&name=snippet_1)]

EF, `FirstOrDefaultAsync` yöntemini kullanarak verileri aramanızı kolaylaştırır. Yöntemi içinde yerleşik olarak bulunan önemli bir güvenlik özelliği, kodun, onunla herhangi bir şey yapmayı denemeden önce arama yönteminin bir filmi buldığını doğrulamasından kaynaklanmaktadır. Örneğin, bir korsan `http://localhost:{PORT}/Movies/Details/1` bağlantıları tarafından oluşturulan URL 'yi `http://localhost:{PORT}/Movies/Details/12345` (veya gerçek bir filmi temsil eden başka bir değer) gibi bir şeye değiştirerek siteye hata verebilir. Null bir filmi denetmediyseniz, uygulama bir özel durum oluşturur.

`Delete` ve `DeleteConfirmed` yöntemlerini inceleyin.

[!code-csharp[](start-mvc/sample/MvcMovie22/Controllers/MoviesController.cs?name=snippet_delete)]

`HTTP GET Delete` yöntemi belirtilen filmi silmediğini unutmayın. Bu, silme işlemini gönderebileceğiniz (HttpPost) filmin bir görünümünü döndürür. Bir GET isteğine yanıt olarak silme işlemi gerçekleştirme (veya bu konuyla ilgili olarak, düzenleme işlemi gerçekleştirme, oluşturma işlemi yapma veya verileri değiştiren başka bir işlem) bir güvenlik deliği açılır.

Verileri silen `[HttpPost]` yöntemi, HTTP POST yöntemine benzersiz bir imza veya ad vermek için `DeleteConfirmed` olarak adlandırılır. İki yöntem imzası aşağıda gösterilmiştir:

[!code-csharp[](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete2)]

[!code-csharp[](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete3)]

Ortak dil çalışma zamanı (CLR), aşırı yüklenmiş yöntemlerin benzersiz bir parametre imzasına sahip olmasını gerektirir (aynı yöntem adı ancak farklı parametre listesi). Bununla birlikte, her ikisi de aynı parametre imzasına sahip olmak üzere iki `Delete` yöntemi (GET için bir tane ve diğeri) gerekir. (Her ikisi de parametre olarak tek bir tamsayıyı kabul etmelidir.)

Bu sorunun iki yaklaşımı vardır, biri yöntemlere farklı adlar vermektir. Bu, önceki örnekte bulunan yapı iskelesi mekanizmasına göre yapılır. Ancak, bu küçük bir sorun ortaya çıkarır: ASP.NET bir URL 'nin segmentlerini ada göre eylem yöntemlerine eşler ve bir yöntemi yeniden adlandırırsanız, yönlendirme normalde bu yöntemi bulamaz. Çözüm, örnekte gördüğünüz şeydir. Bu, `DeleteConfirmed` yöntemine `ActionName("Delete")` özniteliğini eklemektir. Bu öznitelik, yönlendirme sistemi için eşleme gerçekleştirerek, bir POST isteği için/Delete/içeren bir URL 'nin `DeleteConfirmed` yöntemi bulacaktır.

Özdeş adlara ve imzalara sahip yöntemler için bir diğer yaygın çalışma yapay, POST yönteminin imzasını bir ek (kullanılmamış) parametre içerecek şekilde değiştirecek. Bu, `notUsed` parametresini eklediğimiz sırada önceki bir gönderimiz tarafından yaptığımız şeydir. `[HttpPost] Delete` yöntemi için burada aynı şeyi yapabilirsiniz:

```csharp
// POST: Movies/Delete/6
[HttpPost]
[ValidateAntiForgeryToken]
public async Task<IActionResult> Delete(int id, bool notUsed)
```

### <a name="publish-to-azure"></a>Azure’da Yayımlama

Azure 'a dağıtma hakkında bilgi için bkz. [öğretici: Azure App Service .NET Core ve SQL veritabanı Web uygulaması oluşturma](/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb).

> [!div class="step-by-step"]
> [Öncekini](validation.md)
