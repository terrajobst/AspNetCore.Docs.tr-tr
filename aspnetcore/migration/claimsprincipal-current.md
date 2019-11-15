---
title: ClaimsPrincipal. Current öğesinden geçir
author: mjrousos
description: Geçerli kimliği doğrulanmış kullanıcının kimliğini ve ASP.NET Core taleplerini almak için ClaimsPrincipal. Current ' dan uzağa geçiş yapmayı öğrenin.
ms.author: scaddie
ms.custom: mvc
ms.date: 03/26/2019
uid: migration/claimsprincipal-current
ms.openlocfilehash: f7472f5b851d3869da3d26b881e276ce4ca004fb
ms.sourcegitcommit: 231780c8d7848943e5e9fd55e93f437f7e5a371d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2019
ms.locfileid: "74115973"
---
# <a name="migrate-from-claimsprincipalcurrent"></a>ClaimsPrincipal. Current öğesinden geçir

ASP.NET 4. x projelerinde, geçerli kimliği doğrulanmış kullanıcının kimliğini ve taleplerini almak için [ClaimsPrincipal. Current](/dotnet/api/system.security.claims.claimsprincipal.current) kullanımı yaygındır. ASP.NET Core, bu özellik artık ayarlı değildir. Kendisine bağlı olan kodun, geçerli kimliği doğrulanmış kullanıcının kimliğini farklı yollarla alması için güncelleştirilmesi gerekir.

## <a name="context-specific-data-instead-of-static-data"></a>Statik veriler yerine içeriğe özgü veriler

ASP.NET Core kullanırken, hem `ClaimsPrincipal.Current` hem de `Thread.CurrentPrincipal` değerleri ayarlı değildir. Bu özelliklerin her ikisi de ASP.NET Core genellikle kaçınan statik durumu temsil eder. Bunun yerine, ASP.NET Core mimarisi, bağlama özgü hizmet koleksiyonlarından (geçerli kullanıcının kimliği gibi) bağımlılıkları ( [bağımlılık ekleme](xref:fundamentals/dependency-injection) (dı) modelini kullanarak) almak. Ayrıca, iş parçacığı statik olan `Thread.CurrentPrincipal`, bazı zaman uyumsuz senaryolarda değişiklikleri kalıcı hale getiremeyebilir (ve `ClaimsPrincipal.Current` yalnızca varsayılan olarak `Thread.CurrentPrincipal` çağırır).

Statik üyelerin zaman uyumsuz senaryolarda neden olabileceği sorun sayısını anlamak için aşağıdaki kod parçacığını göz önünde bulundurun:

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

Önceki örnek kod `Thread.CurrentPrincipal` ayarlar ve zaman uyumsuz bir çağrıyı beklerken önce ve sonra değerini denetler. `Thread.CurrentPrincipal`, ayarlandığı *iş parçacığına* özgüdür ve Yöntem await 'dan sonra farklı bir iş parçacığında yürütmeyi sürdürülemez. Sonuç olarak, `Thread.CurrentPrincipal` ilk kez denetlendiğinde, ancak `await Task.Yield()`çağrısından sonra null olduğunda vardır.

Test kimlikleri kolay bir şekilde eklenebilir olduğundan, geçerli kullanıcının kimliğini uygulamanın dı hizmeti koleksiyonundan almak daha kararlı değildir.

## <a name="retrieve-the-current-user-in-an-aspnet-core-app"></a>ASP.NET Core uygulamasındaki geçerli kullanıcıyı alma

Geçerli kimliği doğrulanmış kullanıcının `ClaimsPrincipal` `ClaimsPrincipal.Current`yerine ASP.NET Core almak için birkaç seçenek vardır:

* **ControllerBase. User**. MVC denetleyicileri, geçerli kimliği doğrulanmış kullanıcıya [Kullanıcı](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.user) özelliği ile erişebilir.
* **HttpContext. User**. Geçerli `HttpContext` erişimi olan bileşenler (örneğin, ara yazılım) geçerli kullanıcının `ClaimsPrincipal` [HttpContext. User](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user)öğesinden alabilir.
* **Çağırandan geçirildi**. Geçerli `HttpContext` erişimi olmayan kitaplıklar genellikle denetleyiciler veya ara yazılım bileşenlerinden çağırılır ve geçerli kullanıcının kimliği bir bağımsız değişken olarak geçirilebilir.
* **Ihttpcontextaccessor**. ASP.NET Core geçirilecek proje, geçerli kullanıcının kimliğini tüm gerekli konumlara kolayca geçiremeyecek kadar büyük olabilir. Bu gibi durumlarda, [ıhttpcontextaccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) geçici çözüm olarak kullanılabilir. `IHttpContextAccessor` geçerli `HttpContext` erişebilir (varsa). Eğer, kullanılıyorsa bkz. <xref:fundamentals/httpcontext>. Geçerli kullanıcının kimliğini, ASP.NET Core dı odaklı mimarisiyle çalışacak şekilde güncelleştirilmemiş kodda almaya yönelik kısa süreli bir çözüm şu şekilde olacaktır:

  * `Startup.ConfigureServices`içinde [Addhttpcontextaccessor](https://github.com/aspnet/Hosting/issues/793) 'ı çağırarak DI kapsayıcısında `IHttpContextAccessor` kullanılabilir hale getirin.
  * Başlatma sırasında `IHttpContextAccessor` bir örneğini alın ve statik bir değişkende depolayın. Örnek, daha önce geçerli kullanıcıyı statik bir özellikten alan kod için kullanılabilir hale getirilir.
  * `HttpContextAccessor.HttpContext?.User`kullanarak geçerli kullanıcının `ClaimsPrincipal` alın. Bu kod bir HTTP isteği bağlamı dışında kullanılırsa `HttpContext` null olur.

Statik bir değişkende depolanan bir `IHttpContextAccessor` örneğini kullanan son seçenek, ASP.NET Core ilkelerine karşı (statik bağımlılıklara eklenen bağımlılıklar tercih edilir). Bunun yerine bağımlılık ekleme işleminden sonunda `IHttpContextAccessor` örnekleri almayı planlayın. Statik bir yardımcı, daha önce `ClaimsPrincipal.Current`kullanan büyük var olan ASP.NET uygulamalarını geçirirken yararlı bir köprü olabilir.
