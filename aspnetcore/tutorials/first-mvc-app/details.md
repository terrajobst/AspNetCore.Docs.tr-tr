---
title: Ayrıntıları inceleyin ve silme yöntemleri bir ASP.NET Core uygulama
author: rick-anderson
description: Ayrıntılar denetleyicisi yöntem hakkında bilgi edinin ve temel bir ASP.NET Core MVC uygulama görüntüleyin.
ms.author: riande
ms.date: 03/07/2017
uid: tutorials/first-mvc-app/details
ms.openlocfilehash: f7f9a369e3e612542140fcf1091b21037e530a91
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278654"
---
# <a name="examine-the-details-and-delete-methods-of-an-aspnet-core-app"></a>Ayrıntıları inceleyin ve silme yöntemleri bir ASP.NET Core uygulama

tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Film denetleyicisi açın ve incelemek `Details` yöntemi:

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](start-mvc/sample/MvcMovie21/Controllers/MoviesController.cs?name=snippet_details)]

::: moniker-end
::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_details)]

::: moniker-end

Bu eylem yöntemine oluşturulan MVC yapı iskelesi altyapısı yöntemini çağıran bir HTTP isteği gösteren bir açıklama ekler. Bu durumda bir GET isteği üç URL kesimine sahip olup `Movies` denetleyicisi `Details` yöntemi ve bir `id` değeri. Bu kesimler tanımlanmış geri çağırma *haline*.

[!code-csharp[](start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

EF verileri kullanarak için arama yapmayı kolaylaştırır `SingleOrDefaultAsync` yöntemi. Yönteme yerleşik bir önemli güvenlik kodu şey denemeden önce search yöntemini film buldu doğrular özelliğidir. Örneğin, bir bilgisayar korsanının siteye bağlantılardan tarafından oluşturulan URL değiştirerek hatalara `http://localhost:xxxx/Movies/Details/1` gibi bir `http://localhost:xxxx/Movies/Details/12345` (veya gerçek bir filmi temsil etmez başka bir değer). Null film işaretlerseniz alamadık, uygulama bir özel durum.

İncelemek `Delete` ve `DeleteConfirmed` yöntemleri.

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](start-mvc/sample/MvcMovie21/Controllers/MoviesController.cs?name=snippet_delete)]

::: moniker-end
::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete)]

::: moniker-end

Unutmayın `HTTP GET Delete` yöntemi belirtilen film Sil değil, bir filmi görünümünü (HttpPost) gönderebileceği silme değerini döndürür. Bir GET'e yanıt olarak bir silme işlemi isteği (veya bir düzenleme işlemi gerçekleştirilirken Bu konular için işlem veya veriler değiştiğinde başka bir işlem oluşturmak) güvenlik boşluğu açar.

`[HttpPost]` Verileri siler yöntemi adlandırılan `DeleteConfirmed` HTTP POST yöntemi için benzersiz bir imza veya ad vermek için. İki yöntem imzaları aşağıda verilmiştir:

[!code-csharp[](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete2)]

[!code-csharp[](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete3)]


Ortak dil çalışma zamanı (CLR) benzersiz parametre imzası (aynı yöntem adı ancak farklı parametrelerin listesi) sağlamak için aşırı yüklenmiş yöntemler gerektirir. Ancak, burada iki gerekir `Delete` --biri--get ve POST için bir yöntem her ikisi de aynı parametre imzaya sahip. (Her ikisi de tek bir tamsayı bir parametre olarak kabul etmeniz gerekir.)

Bu sorunun iki yaklaşım vardır, farklı adlar yöntemleri vermek biridir. Önceki örnekte yapı iskelesi mekanizması ne yaptığını olmasıdır. Ancak, bu küçük bir sorunla sunar: ASP.NET eylem yöntemleri için bir URL kesimleri adıyla eşler ve bir yöntem yeniden adlandırırsanız, normal olarak Yönlendirme bu yöntem bulmak bağlanamayacak. Eklemek için örnekte bkz çözümdür `ActionName("Delete")` özniteliğini `DeleteConfirmed` yöntemi. Böylece bir POST isteği için /Delete/ içeren bir URL bulur, öznitelik eşleme için yönlendirme sistem gerçekleştirir. `DeleteConfirmed` yöntemi.

Aynı ad ve imza sahip yöntemleri için başka bir Ortak üstesinden gelme yapay fazladan dahil etmek için POST yöntemini imza değiştirmektir (kullanılmayan) parametre. Biz eklendiğinde neler önceki postasına yaptığımız olan `notUsed` parametresi. Aynı şeyi burada yapabilirsiniz `[HttpPost] Delete` yöntemi:

```csharp
// POST: Movies/Delete/6
[ValidateAntiForgeryToken]
public async Task<IActionResult> Delete(int id, bool notUsed)
```

### <a name="publish-to-azure"></a>Azure'a yayımlama

Bkz: [Visual Studio kullanarak Azure App Service için ASP.NET Core web uygulama yayımlama](xref:tutorials/publish-to-azure-webapp-using-vs) bu uygulama için Visual Studio kullanarak Azure yayımlama hakkında yönergeler için.  Uygulama aynı zamanda gelen yayımlanabilir [komut satırı](xref:tutorials/publish-to-azure-webapp-using-cli).

> [!div class="step-by-step"]
> [Önceki](validation.md)
