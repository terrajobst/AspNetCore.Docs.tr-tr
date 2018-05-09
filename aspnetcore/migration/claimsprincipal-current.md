---
title: ClaimsPrincipal.Current geçirme
author: mjrousos
description: Geçerli kimliği doğrulanmış kullanıcının kimlik ve ASP.NET Core Taleplerde almak için ClaimsPrincipal.Current yapmaktan öğrenin.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 05/04/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/claimsprincipal-current
ms.openlocfilehash: ea43d17e76380baf57cd9debbc508e8812cfa4a6
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2018
---
# <a name="migrate-from-claimsprincipalcurrent"></a>ClaimsPrincipal.Current geçirme

ASP.NET projelerinde kullanmak için ortak [ClaimsPrincipal.Current](/dotnet/api/system.security.claims.claimsprincipal.current) geçerli almak için kullanıcının kimlik ve talep kimliği. ASP.NET çekirdek, bu özellik artık ayarlanır. Ona bağlı kodu farklı yollarla geçerli kimliği doğrulanmış kullanıcının kimliğini almak için güncelleştirilmesi gerekir.

## <a name="context-specific-data-instead-of-static-data"></a>Statik veri yerine bağlam özgü verileri

ASP.NET Core, her ikisi de değerlerini kullanırken `ClaimsPrincipal.Current` ve `Thread.CurrentPrincipal` oluşturulmamış. Bu özellikler her iki ASP.NET Core genellikle önler statik durumu temsil eder. Bunun yerine, ASP.NET Core'nın mimarisidir bağlam özgü hizmet koleksiyonlarından bağımlılıkları (örneğin, geçerli kullanıcının kimliğini) almak için (kullanarak kendi [bağımlılık ekleme](xref:fundamentals/dependency-injection) (dı) modeli). Dahası, `Thread.CurrentPrincipal` zaman uyumsuz bazı senaryolarda değişiklikleri korunmayabilir statik iş parçacığı olduğundan (ve `ClaimsPrincipal.Current` yalnızca çağırır `Thread.CurrentPrincipal` varsayılan olarak).

Statik üyeler sorunları iş parçacığı, her türlü anlamak için aşağıdaki kod parçacığını zaman uyumsuz senaryolarda göz önünde bulundurun açabilir:

```csharp
// Create a ClaimsPrincipal and set Thread.CurrentPrincipal
var identity = new ClaimsIdentity();
identity.AddClaim(new Claim(ClaimTypes.Name, "User1"));
Thread.CurrentPrincipal = new ClaimsPrincipal(identity);

// Check the current user
Console.WriteLine($"Current user: {Thread.CurrentPrincipal?.Identity.Name}");

// For the method to complete asynchronously
await Task.Yield();

// Check the current user after
Console.WriteLine($"Current user: {Thread.CurrentPrincipal?.Identity.Name}");
```

Yukarıdaki örnek kod kümeleri `Thread.CurrentPrincipal` ve önce ve sonra bir zaman uyumsuz çağrı bekleyen değerini denetler. `Thread.CurrentPrincipal` özel *iş parçacığı* üzerinde ayarlanır ve farklı bir iş parçacığı üzerinde yürütme bekleme sonrasında devam etmek büyük olasılıkla bir yöntemdir. Sonuç olarak, `Thread.CurrentPrincipal` zaman önce denetlenir ancak çağrısından sonra NULL'dur var olan `await Task.Yield()`.

Test kimlikleri kolayca yerleştirilebilir uygulamanın dı hizmet koleksiyonundan geçerli kullanıcının kimliğini alma de daha sınanabilir taşımaktadır.

## <a name="retrieve-the-current-user-in-an-aspnet-core-app"></a>Geçerli kullanıcı bir ASP.NET Core uygulamasında alma

Geçerli kimliği doğrulanmış kullanıcının almak için birkaç seçenek `ClaimsPrincipal` ASP.NET Core yerine içinde `ClaimsPrincipal.Current`:

* **ControllerBase.User**. MVC denetleyicileri olan geçerli kimliği doğrulanmış kullanıcı erişebilir kendi [kullanıcı](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.user) özelliği.
* **HttpContext.User**. Geçerli erişimi olan bileşenleri `HttpContext` (örneğin, Ara), geçerli kullanıcının alabilirsiniz `ClaimsPrincipal` gelen [HttpContext.User](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user).
* **Çağrıyı yapandan geçirilen**. Geçerli erişim olmadan kitaplıkları `HttpContext` genellikle denetleyicileri veya ara yazılımı bileşenleri olarak adlandırılır ve geçerli kullanıcının kimliğini bağımsız değişken olarak geçirilen olabilir.
* **IHttpContextAccessor**. ASP.NET Core Geçirilmekte olan ASP.NET projesi kolayca geçerli kullanıcının kimliğini tüm gerekli konumlara geçirmek için çok büyük olabilir. Böyle durumlarda, [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) geçici bir çözüm olarak kullanılabilir. `IHttpContextAccessor` Geçerli erişebiliyor `HttpContext` (varsa). ASP.NET Core'nın dı odaklı mimari ile çalışmak için henüz güncelleştirilmemiş kurmadı kod geçerli kullanıcının kimliğini almak için kısa vadeli bir çözüm olacaktır:

  * Olun `IHttpContextAccessor` çağırarak dı kapsayıcısında kullanılabilir [AddHttpContextAccessor](https://github.com/aspnet/Hosting/issues/793) içinde `Startup.ConfigureServices`.
  * Bir örneğini almak `IHttpContextAccessor` başlatma sırasında ve statik bir değişkende saklayın. Örneğinin geçerli kullanıcının statik özelliğinden daha önce alırken kod için kullanılabilir hale getirilir.
  * Geçerli kullanıcının almak `ClaimsPrincipal` kullanarak `HttpContextAccessor.HttpContext?.User`. Bir HTTP isteği bağlamı dışında bu kodu kullandıysanız `HttpContext` null.

Son seçenek, kullanarak `IHttpContextAccessor`, ASP.NET Core ilkeleri (statik bağımlılıkları eklenen bağımlılıkları tercih ederek) aykırı olduğu. Statik bağımlılığını sonunda kaldıracağınızı `IHttpContextAccessor` Yardımcısı. Bu yararlı bir köprü rağmen daha önce kullandığınız büyük mevcut ASP.NET uygulamaları geçirilirken olabilir `ClaimsPrincipal.Current`.
