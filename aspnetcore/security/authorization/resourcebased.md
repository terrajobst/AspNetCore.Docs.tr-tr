---
title: ASP.NET core'da kaynak tabanlı yetkilendirme
author: scottaddie
description: Bir Authorize özniteliği yeterli olmaz, kaynak tabanlı yetkilendirme bir ASP.NET Core uygulaması içinde uygulamayı öğrenin.
ms.author: scaddie
ms.custom: mvc
ms.date: 11/15/2018
uid: security/authorization/resourcebased
ms.openlocfilehash: 6a163caa26b277fbee6b9d61f8f1d16a60c75b03
ms.sourcegitcommit: d3392f688cfebc1f25616da7489664d69c6ee330
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/16/2018
ms.locfileid: "51818375"
---
# <a name="resource-based-authorization-in-aspnet-core"></a>ASP.NET core'da kaynak tabanlı yetkilendirme

Erişilen kaynak yetkilendirme stratejisi bağlıdır. Bir yazar özelliğine sahip bir belge göz önünde bulundurun. Yalnızca yazarın belge güncelleştirmesi izin verilmez. Sonuç olarak, yetkilendirme değerlendirmesi gerçekleşebilmesi belge veri deposundan alınması gerekir.

Öznitelik değerlendirme, veri bağlama ve sayfa işleyicisi veya belge yükler eylem yürütme önce gerçekleşir. Bu nedenlerle, bildirim temelli yetkilendirme ile bir `[Authorize]` değil öznitelik yeterli. Bunun yerine, bir özel yetkilendirme yöntemi çağırabilirsiniz&mdash;olarak bilinen bir stil *kesinlik temelli yetkilendirme*.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample)).

[Kullanıcı verilerinin yetkilendirme tarafından korunduğu ile bir ASP.NET Core uygulaması oluşturma](xref:security/authorization/secure-data) kaynak tabanlı yetkilendirme kullanan bir örnek uygulamayı içerir.

## <a name="use-imperative-authorization"></a>Kesinlik temelli yetkilendirme kullanın

Yetkilendirme olarak gerçekleştirilen bir [Iauthorizationservice](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice) hizmet ve hizmet koleksiyonu içinde kayıtlı `Startup` sınıfı. Hizmet aracılığıyla kullanılabilir hale getirileceğini [bağımlılık ekleme](xref:fundamentals/dependency-injection) sayfa işleyicileri veya eylemler.

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Controllers/DocumentController.cs?name=snippet_IAuthServiceDI&highlight=6)]

`IAuthorizationService` iki `AuthorizeAsync` yöntemi aşırı yüklemeleri: kaynak ve ilke adını ve diğer kaynak ve değerlendirmek için gereksinimler listesini kabul eden bir kabul etme.

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

Aşağıdaki örnekte, özel kaynağının korunması için yüklenen `Document` nesne. Bir `AuthorizeAsync` aşırı yükleme, geçerli kullanıcının belirtilen belgeyi düzenlemek için izin verilip verilmeyeceğini belirlemek için çağrılır. Özel bir "EditPolicy" yetkilendirme ilkesi karar katılır. Bkz: [özel ilke tabanlı yetkilendirme](xref:security/authorization/policies) yetkilendirme ilkeleri oluşturma hakkında daha fazla bilgi için.

> [!NOTE]
> Aşağıdaki kod örnekleri kimlik doğrulaması çalıştırıldığı varsayılır ve kümesi `User` özelliği.

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/Edit.cshtml.cs?name=snippet_DocumentEditHandler)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentEditAction)]

::: moniker-end

## <a name="write-a-resource-based-handler"></a>Bir kaynak tabanlı işleyicisi yazma

Kaynak tabanlı yetkilendirme çok farklı değildir için bir işleyici yazma [düz gereksinimleri işleyicisi yazma](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler). Özel gereksinim sınıfı oluşturun ve bir gereksinim işleyicisi sınıfı. Bir gereksinim sınıfı oluşturma hakkında daha fazla bilgi için bkz. [gereksinimleri](xref:security/authorization/policies#requirements).

İşleyici sınıfı, hem gereksinim hem de kaynak türünü belirtir. Örneğin, bir işleyici kullanan bir `SameAuthorRequirement` ve `Document` kaynak aşağıdadır:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

::: moniker-end

Önceki örnekte, Imagine `SameAuthorRequirement` daha fazla genel, özel bir durumdur `SpecificAuthorRequirement` sınıfı. `SpecificAuthorRequirement` (Gösterilmemiştir) sınıfı içeren bir `Name` yazarının adını temsil eden özellik. `Name` Özelliği, geçerli kullanıcı için ayarlanmış olması.

Kaydetme gereksinimi ve işleyicisinde `Startup.ConfigureServices`:

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Startup.cs?name=snippet_ConfigureServicesSample&highlight=3-7,9)]

### <a name="operational-requirements"></a>İşletimsel gereksinimleri

CRUD (oluşturma, okuma, güncelleştirme, silme) işlemlerinin sonuçları temel alarak kararları yapıyorsanız kullanmak [OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) yardımcı sınıfı. Bu sınıf, her işlem türü için ayrı bir sınıf yerine tek bir işleyici yazmanıza olanak sağlar. Bunu kullanmak için bazı işlem adları sağlar:

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_OperationsClass)]

İşleyicisi aşağıdaki gibi kullanılarak uygulanan bir `OperationAuthorizationRequirement` gereksinim ve `Document` kaynak:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

::: moniker-end

Kaynak ve kullanıcının kimliğini gereksinimin işlemi önceki işleyici doğrular `Name` özelliği.

Bir operasyonel kaynak işleyicisi çağırmak için işlemi çağrılırken belirtin `AuthorizeAsync` sayfası işleyicisi veya eylem. Aşağıdaki örnek, kimliği doğrulanmış kullanıcı belirtilen belgeyi görüntülemek için izinli olup olmadığını belirler.

> [!NOTE]
> Aşağıdaki kod örnekleri kimlik doğrulaması çalıştırıldığı varsayılır ve kümesi `User` özelliği.

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/View.cshtml.cs?name=snippet_DocumentViewHandler&highlight=10-11)]

Yetkilendirme başarılı olursa, belgeyi görüntülemek için sayfanın döndürülür. Yetkilendirme başarısız olur ancak kullanıcının kimliği doğrulanır ve varsa, döndüren `ForbidResult` yetkilendirme başarısız oldu. kimlik doğrulaması ara yazılımı konusunda bilgilendirir. A `ChallengeResult` kimlik doğrulaması gerçekleştirilmesi gereken döndürülür. Etkileşimli tarayıcı istemcileri için kullanıcının oturum açma sayfasına yeniden yönlendirmek uygun olabilir.

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentViewAction&highlight=11-12)]

Yetkilendirme başarılı olursa, belge görünümü döndürülür. Yetkilendirme başarısız olursa döndüren `ChallengeResult` herhangi bir kimlik doğrulaması ara yazılım kimlik doğrulaması başarısız oldu ve ara yazılım uygun yanıtı alabilir bildirir. Uygun bir yanıt durum kodu 401 veya 403 döndürüyor. Etkileşimli tarayıcı istemcileri için kullanıcı bir oturum açma sayfasına yeniden yönlendiriliyorsunuz gelebilir.

::: moniker-end
