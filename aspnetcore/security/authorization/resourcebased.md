---
title: ASP.NET Core kaynak tabanlı yetkilendirme
author: scottaddie
description: Yetkilendirme özniteliği yeterli olmadığında kaynak tabanlı yetkilendirmeyi bir ASP.NET Core uygulamasına uygulamayı öğrenin.
ms.author: scaddie
ms.custom: mvc
ms.date: 11/15/2018
uid: security/authorization/resourcebased
ms.openlocfilehash: acc931da1be0940fac72b0aabe07ab17ca7e63bd
ms.sourcegitcommit: 6628cd23793b66e4ce88788db641a5bbf470c3c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73659996"
---
# <a name="resource-based-authorization-in-aspnet-core"></a>ASP.NET Core kaynak tabanlı yetkilendirme

Yetkilendirme stratejisi erişilmekte olan kaynağa bağlıdır. Yazar özelliği olan bir belge düşünün. Yalnızca yazarın belgeyi güncelleştirmesine izin verilir. Sonuç olarak, yetkilendirme değerlendirmesi gerçekleşebilmesi için belgenin veri deposundan alınması gerekir.

Öznitelik değerlendirmesi, veri bağlamadan önce ve belgeyi yükleyen sayfa işleyicisinin veya eylemin yürütmeden önce oluşur. Bu nedenlerden dolayı, bir `[Authorize]` özniteliği ile bildirime dayalı yetkilendirme yok olacaktır. Bunun yerine, zorunlu *Yetkilendirme*olarak bilinen &mdash;a stili özel bir yetkilendirme yöntemi çağırabilirsiniz.

::: moniker range=">= aspnetcore-3.0"
[Örnek kodu görüntüleyin veya indirin](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples/3_0) ([nasıl indirilir](xref:index#how-to-download-a-sample)).
::: moniker-end

 ::: moniker range=">= aspnetcore-2.0 < aspnetcore-3.0"
[Örnek kodu görüntüleyin veya indirin](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples/2_2) ([nasıl indirilir](xref:index#how-to-download-a-sample)).
::: moniker-end

::: moniker range="<= aspnetcore-1.1"
[Örnek kodu görüntüleyin veya indirin](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples/1_1) ([nasıl indirilir](xref:index#how-to-download-a-sample)).
::: moniker-end

[Yetkilendirme ile korunan kullanıcı verileriyle ASP.NET Core uygulama oluşturma](xref:security/authorization/secure-data) kaynak tabanlı yetkilendirme kullanan bir örnek uygulama içerir.

## <a name="use-imperative-authorization"></a>Kesinlik temelli yetkilendirme kullan

Yetkilendirme bir [IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice) hizmeti olarak uygulanır ve `Startup` sınıfı içindeki hizmet koleksiyonuna kaydedilir. Hizmet, sayfa işleyicilere veya eylemlere [bağımlılık ekleme](xref:fundamentals/dependency-injection) yoluyla kullanılabilir hale getirilir.

[!code-csharp[](resourcebased/samples/3_0/ResourceBasedAuthApp2/Controllers/DocumentController.cs?name=snippet_IAuthServiceDI&highlight=6)]

`IAuthorizationService` iki `AuthorizeAsync` yöntemi aşırı yüklemesi vardır: kaynağın ve ilke adının yanı sıra kaynağı kabul eden diğeri, değerlendirmek için gereksinimlerin bir listesi.

::: moniker range=">= aspnetcore-2.0"

```csharp
Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          IEnumerable<IAuthorizationRequirement> requirements);
Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          string policyName);
```

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

```csharp
Task<bool> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          IEnumerable<IAuthorizationRequirement> requirements);
Task<bool> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          string policyName);
```

::: moniker-end

<a name="security-authorization-resource-based-imperative"></a>

Aşağıdaki örnekte, güvenli hale getirilme kaynağı özel bir `Document` nesnesine yüklenir. Geçerli kullanıcının belirtilen belgeyi düzenlemesine izin verilip verilmeyeceğini belirlemekte bir `AuthorizeAsync` aşırı yüklemesi çağrılır. Özel bir "EditPolicy" yetkilendirme ilkesi karara göre belirlenir. Yetkilendirme ilkeleri oluşturma hakkında daha fazla bilgi için bkz. [özel ilke tabanlı yetkilendirme](xref:security/authorization/policies) .

> [!NOTE]
> Aşağıdaki kod örnekleri, kimlik doğrulamasının çalıştırıldığını varsayar ve `User` özelliğini ayarlar.

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/3_0/ResourceBasedAuthApp2/Pages/Document/Edit.cshtml.cs?name=snippet_DocumentEditHandler)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/1_1/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentEditAction)]

::: moniker-end

## <a name="write-a-resource-based-handler"></a>Kaynak tabanlı işleyici yazma

Kaynak tabanlı yetkilendirme için bir işleyici yazmak, [düz gereksinimler işleyicisi yazmaktan](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler)çok farklı değildir. Özel bir gereksinim sınıfı oluşturun ve bir gereksinim işleyicisi sınıfı uygulayın. Gereksinim sınıfı oluşturma hakkında daha fazla bilgi için bkz. [Requirements](xref:security/authorization/policies#requirements).

Handler sınıfı hem gereksinim hem de kaynak türünü belirtir. Örneğin, bir `SameAuthorRequirement` ve `Document` kaynağı kullanan bir işleyici aşağıdaki gibidir:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/3_0/ResourceBasedAuthApp2/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/1_1/ResourceBasedAuthApp1/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

::: moniker-end

Yukarıdaki örnekte, `SameAuthorRequirement` daha genel `SpecificAuthorRequirement` sınıfının özel bir durumu olduğunu düşünün. `SpecificAuthorRequirement` sınıfı (gösterilmez) yazarın adını temsil eden bir `Name` özelliği içerir. `Name` özelliği geçerli kullanıcıya ayarlanabilir.

Gereksinimi ve işleyiciyi `Startup.ConfigureServices` Kaydet:

::: moniker range=">= aspnetcore-3.0"
[!code-csharp[](resourcebased/samples/3_0/ResourceBasedAuthApp2/Startup.cs?name=snippet_ConfigureServicesSample&highlight=4-8,10)]
::: moniker-end

 ::: moniker range=">= aspnetcore-2.0 < aspnetcore-3.0"
[!code-csharp[](resourcebased/samples/2_2/ResourceBasedAuthApp2/Startup.cs?name=snippet_ConfigureServicesSample&highlight=3-7,9)]
::: moniker-end

::: moniker range="<= aspnetcore-1.1"
[!code-csharp[](resourcebased/samples/1_1/ResourceBasedAuthApp1/Startup.cs?name=snippet_ConfigureServicesSample&highlight=3-7,9)]
::: moniker-end

### <a name="operational-requirements"></a>İşletimsel gereksinimler

CRUD (oluşturma, okuma, güncelleştirme, silme) işlemlerinin sonuçlarını temel alan kararlar verirken [Operationauthorizationrequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) yardımcı sınıfını kullanın. Bu sınıf her işlem türü için tek bir sınıf yerine tek bir işleyici yazmanızı sağlar. Kullanmak için, bazı işlem adlarını sağlayın:

[!code-csharp[](resourcebased/samples/3_0/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_OperationsClass)]

İşleyici, bir `OperationAuthorizationRequirement` gereksinimi ve `Document` kaynağı kullanılarak aşağıdaki gibi uygulanır:

 ::: moniker range=">= aspnetcore-2.0"
[!code-csharp[](resourcebased/samples/3_0/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/1_1/ResourceBasedAuthApp1/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

::: moniker-end

Önceki işleyici, kaynağı, kullanıcının kimliğini ve gereksinimin `Name` özelliğini kullanarak işlemi doğrular.

## <a name="challenge-and-forbid-with-an-operational-resource-handler"></a>İşletimsel kaynak işleyicisiyle Challenge ve fordeklarasyon

Bu bölümde, Challenge ve fordeklarasyon eylem sonuçlarının nasıl işlendiği ve çekişme ve fordeklarasyonu 'nin nasıl farklı olduğu gösterilmektedir.

İşletimsel bir kaynak işleyicisini çağırmak için, sayfa işleyicinizde veya eylemde `AuthorizeAsync` çağırırken işlemi belirtin. Aşağıdaki örnek, kimliği doğrulanmış kullanıcının belirtilen belgeyi görüntülemesine izin verilip verilmeyeceğini belirler.

> [!NOTE]
> Aşağıdaki kod örnekleri, kimlik doğrulamasının çalıştırıldığını varsayar ve `User` özelliğini ayarlar.

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/3_0/ResourceBasedAuthApp2/Pages/Document/View.cshtml.cs?name=snippet_DocumentViewHandler&highlight=10-11)]

Yetkilendirme başarılı olursa belgeyi görüntüleme sayfası döndürülür. Yetkilendirme başarısız olursa, ancak kullanıcının kimliği doğrulanırsa, `ForbidResult` döndürülüyor, yetkilendirme başarısız olan tüm kimlik doğrulama ara yazılımını bilgilendirir. Kimlik doğrulaması gerçekleştirilmesi gerektiğinde bir `ChallengeResult` döndürülür. Etkileşimli tarayıcı istemcileri için, kullanıcıyı bir oturum açma sayfasına yönlendirmek uygun olabilir.

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/1_1/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentViewAction&highlight=11-12)]

Yetkilendirme başarılı olursa belge görünümü döndürülür. Yetkilendirme başarısız olursa, döndüren `ChallengeResult` kimlik doğrulama ara yazılımı yetkilendirme başarısız olur ve ara yazılım uygun yanıtı alabilir. Uygun bir yanıt 401 veya 403 durum kodu döndürüyor olabilir. Etkileşimli tarayıcı istemcileri için kullanıcıyı bir oturum açma sayfasına yeniden yönlendirmek anlamına gelebilir.

::: moniker-end
