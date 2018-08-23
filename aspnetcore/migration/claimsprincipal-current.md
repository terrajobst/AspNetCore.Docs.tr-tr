---
title: ClaimsPrincipal.Current geçirme
author: mjrousos
description: Geçerli kimliği doğrulanmış kullanıcının kimliğine ve ASP.NET Core Taleplerde alınacak ClaimsPrincipal.Current uzağa geçirmeyi öğrenin.
ms.author: scaddie
ms.custom: mvc
ms.date: 05/04/2018
uid: migration/claimsprincipal-current
ms.openlocfilehash: 35c3389798041e141c45bf0a76fa9d7285212768
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41757389"
---
# <a name="migrate-from-claimsprincipalcurrent"></a>ClaimsPrincipal.Current geçirme

ASP.NET 4.x projelerinde kullanmak yaygın [ClaimsPrincipal.Current](/dotnet/api/system.security.claims.claimsprincipal.current) geçerli alınacak kullanıcının kimliğine ve talep kimliği. ASP.NET Core, bu özellik artık ayarlanır. Üzerine bağlı kod farklı yollarla geçerli kimliği doğrulanmış kullanıcının kimliğini almak için güncelleştirilmesi gerekir.

## <a name="context-specific-data-instead-of-static-data"></a>Statik veri yerine bağlam özgü veriler

ASP.NET Core, her ikisi de değerlerini kullanırken `ClaimsPrincipal.Current` ve `Thread.CurrentPrincipal` ayarlanmıyor. Bu özellikler hem ASP.NET Core genellikle önler statik bir durumu temsil eder. Bunun yerine, ASP.NET Core'nın mimaridir bağlam özgü hizmet koleksiyonlardan bağımlılıkları (örneğin, geçerli kullanıcının kimlik) almak için (kullanarak kendi [bağımlılık ekleme](xref:fundamentals/dependency-injection) (dı) modeli). Dahası, `Thread.CurrentPrincipal` değişiklikleri zaman uyumsuz bazı senaryolarda korunmayabilir statik iş parçacığı olduğundan (ve `ClaimsPrincipal.Current` yalnızca çağıran `Thread.CurrentPrincipal` varsayılan olarak).

Statik üyeleri sorunları iş parçacığı tür anlamak için aşağıdaki kod parçacığı zaman uyumsuz senaryolar göz önünde bulundurun açabilir:

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

Yukarıdaki örnek kod kümelerini `Thread.CurrentPrincipal` ve önce ve sonra bekleyen zaman uyumsuz bir çağrı değerini denetler. `Thread.CurrentPrincipal` özel *iş parçacığı* üzerinde ayarlanır ve await sonra farklı bir iş parçacığında yürütmeye devam etmek büyük olasılıkla yöntemidir. Sonuç olarak, `Thread.CurrentPrincipal` zaman önce teslim ancak çağrısından sonra null olup `await Task.Yield()`.

Test kimlikleri bir kolayca yerleştirilebilir geçerli kullanıcının kimliğini uygulamanın DI hizmet koleksiyonundan alma, daha fazla test edilebilir olduğu.

## <a name="retrieve-the-current-user-in-an-aspnet-core-app"></a>Geçerli kullanıcının bir ASP.NET Core uygulaması alma

Geçerli kimliği doğrulanmış kullanıcının almak için birkaç seçenek vardır `ClaimsPrincipal` ASP.NET core'da yerine `ClaimsPrincipal.Current`:

* **ControllerBase.User**. MVC denetleyicileri ile kimliği doğrulanmış geçerli kullanıcının erişebileceği kendi [kullanıcı](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.user) özelliği.
* **HttpContext.User**. Geçerli erişimi olan bileşenleri `HttpContext` (örneğin, Ara), geçerli kullanıcının alabilirsiniz `ClaimsPrincipal` gelen [HttpContext.User](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user).
* **Çağrıyı yapandan geçirilen**. Geçerli erişim olmadan kitaplıkları `HttpContext` genellikle denetleyicileri veya ara yazılımı bileşenleri olarak adlandırılır ve bağımsız değişken olarak geçirilen geçerli kullanıcının kimlik olabilir.
* **IHttpContextAccessor**. ASP.NET Core Geçirilmekte olan proje, geçerli kullanıcının kimlik için gerekli tüm konumlara kolayca geçirmek için çok büyük olabilir. Bu gibi durumlarda [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) geçici bir çözüm olarak kullanılabilir. `IHttpContextAccessor` Geçerli erişebildiğinden `HttpContext` (varsa). ASP.NET Core'nın DI denetimli mimari ile çalışmak için henüz güncelleştirilmemişse kodda geçerli kullanıcının kimliğini almak için kısa vadeli bir çözüm olacaktır:

  * Olun `IHttpContextAccessor` çağırarak DI kapsayıcısında kullanılabilir [AddHttpContextAccessor](https://github.com/aspnet/Hosting/issues/793) içinde `Startup.ConfigureServices`.
  * Bir kopyasını almak `IHttpContextAccessor` başlatma sırasında ve statik bir değişkende depolayın. Örnek bir statik özelliği geçerli kullanıcının daha önce almaya kod için kullanılabilir yapılır.
  * Geçerli kullanıcının almak `ClaimsPrincipal` kullanarak `HttpContextAccessor.HttpContext?.User`. Bu kod, bir HTTP isteği bağlamı dışında kullanılırsa, `HttpContext` null.

En son seçeneği kullanılarak `IHttpContextAccessor`, ASP.NET Core ilkeleri (eklenen bağımlılıkları için statik bağımlılıkları belgelemeyi) aykırı olduğu. Statik bağımlılığı sonunda kaldıracağınızı `IHttpContextAccessor` Yardımcısı. Bu yararlı bir köprü rağmen daha önce kullandığınız büyük varolan ASP.NET uygulamalarını geçirilirken olabilir `ClaimsPrincipal.Current`.
