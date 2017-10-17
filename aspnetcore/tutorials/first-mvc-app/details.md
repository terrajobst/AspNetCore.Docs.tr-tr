---
title: "Ayrıntıları inceleme ve silme yöntemleri"
author: rick-anderson
description: "Basit bir ASP.NET Core MVC uygulaması görünümünde ve ayrıntıları denetleyici yöntemi."
keywords: "ASP.NET Çekirdeği"
ms.author: riande
manager: wpickett
ms.date: 03/07/2017
ms.topic: get-started-article
ms.assetid: 870192b4-8d4f-45c7-8c14-83d02bc0ad79
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/details
ms.openlocfilehash: 28ed7a7a56415d7eb675c06353fb9a8f65fb571f
ms.sourcegitcommit: c9658c0db446f7cb2e443f62b00cf773bed545fa
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/30/2017
---
# <a name="examining-the-details-and-delete-methods"></a>Ayrıntıları inceleme ve silme yöntemleri

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Film denetleyicisi açın ve incelemek `Details` yöntemi:

[!code-csharp[Main](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_details)]

Bu eylem yöntemine oluşturulan MVC yapı iskelesi altyapısı yöntemini çağıran bir HTTP isteği gösteren bir açıklama ekler. Bu durumda bir GET isteği üç URL kesimine sahip olup `Movies` denetleyicisi `Details` yöntemi ve bir `id` değeri. Bu kesimler tanımlanmış geri çağırma *haline*.

[!code-csharp[Main](start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

EF verileri kullanarak için arama yapmayı kolaylaştırır `SingleOrDefaultAsync` yöntemi. Yönteme yerleşik bir önemli güvenlik kodu şey denemeden önce search yöntemini film buldu doğrular özelliğidir. Örneğin, bir bilgisayar korsanının siteye bağlantılardan tarafından oluşturulan URL değiştirerek hatalara `http://localhost:xxxx/Movies/Details/1` gibi bir `http://localhost:xxxx/Movies/Details/12345` (veya gerçek bir filmi temsil etmez başka bir değer). Null film denetlemiyordu, uygulama bir özel durum.

İncelemek `Delete` ve `DeleteConfirmed` yöntemleri.

[!code-csharp[Main](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete)]

Unutmayın `HTTP GET Delete` yöntemi belirtilen film Sil değil, bir filmi görünümünü (HttpPost) gönderebileceği silme değerini döndürür. Bir GET'e yanıt olarak bir silme işlemi isteği (veya bir düzenleme işlemi gerçekleştirilirken Bu konular için işlem veya veriler değiştiğinde başka bir işlem oluşturmak) güvenlik boşluğu açar.

`[HttpPost]` Verileri siler yöntemi adlandırılan `DeleteConfirmed` HTTP POST yöntemi için benzersiz bir imza veya ad vermek için. İki yöntem imzaları aşağıda verilmiştir:

[!code-csharp[Main](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete2)]

[!code-csharp[Main](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete3)]


Ortak dil çalışma zamanı (CLR) benzersiz parametre imzası (aynı yöntem adı ancak farklı parametrelerin listesi) sağlamak için aşırı yüklenmiş yöntemler gerektirir. Ancak, burada iki gerekir `Delete` --biri--get ve POST için bir yöntem her ikisi de aynı parametre imzaya sahip. (Her ikisi de tek bir tamsayı bir parametre olarak kabul etmeniz gerekir.)

Bu sorunun iki yaklaşım vardır, farklı adlar yöntemleri vermek biridir. Önceki örnekte yapı iskelesi mekanizması ne yaptığını olmasıdır. Ancak, bu küçük bir sorunla sunar: ASP.NET eylem yöntemleri için bir URL kesimleri adıyla eşler ve bir yöntem yeniden adlandırırsanız, normal olarak Yönlendirme bu yöntem bulmak bağlanamayacak. Eklemek için örnekte bkz çözümdür `ActionName("Delete")` özniteliğini `DeleteConfirmed` yöntemi. Böylece bir POST isteği için /Delete/ içeren bir URL bulur, öznitelik eşleme için yönlendirme sistem gerçekleştirir. `DeleteConfirmed` yöntemi.

Aynı ad ve imza sahip yöntemleri için başka bir Ortak üstesinden gelme yapay fazladan dahil etmek için POST yöntemini imza değiştirmektir (kullanılmayan) parametre. Biz eklendiğinde neler önceki postasına yaptığımız olan `notUsed` parametresi. Aynı şeyi burada yapabilirsiniz `[HttpPost] Delete` yöntemi:

```csharp
// POST: Movies/Delete/6
[ValidateAntiForgeryToken]
public async Task<IActionResult> Delete(int id, bool notUsed)
```

### <a name="publish-to-azure"></a>Azure'a yayımlama

Bkz: [Visual Studio kullanarak Azure App Service için ASP.NET Core web uygulama yayımlama](xref:tutorials/publish-to-azure-webapp-using-vs) bu uygulama için Azure yayımlama hakkında yönergeler için.

Bu giriş ASP.NET Core MVC tamamlamak için teşekkür ederiz. Çıkmadan açıklamaları veriyoruz. [MVC ve EF çekirdek Başlarken](xref:data/ef-mvc/intro) mükemmel bir izleme Bu öğretici kadar olan.

>[!div class="step-by-step"]
[Önceki](validation.md)
