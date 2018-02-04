---
title: "ASP.NET Core kaynak tabanlı yetkilendirme"
author: scottaddie
description: "Bir Authorize özniteliği yeterli olmaz, bir ASP.NET Core uygulamada kaynak tabanlı bir yetkilendirme uygulamak öğrenin."
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 11/07/2017
ms.devlang: csharp
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/resourcebased
ms.openlocfilehash: 723e371e0d0b4877f96898c68cd59b433fa97dc1
ms.sourcegitcommit: 7a87d66cf1d01febe6635c7306f2f679434901d1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/03/2018
---
# <a name="resource-based-authorization"></a>Kaynak tabanlı bir yetkilendirme

Yetkilendirme stratejisi erişilen kaynak bağlıdır. Bir yazar özelliği olan bir belgeyi göz önünde bulundurun. Yalnızca yazarın belge güncelleştirme izin verilmez. Sonuç olarak, yetkilendirme değerlendirme oluşabilmesi için öncelikle belge veri deposundan alınması gerekir.

Veri bağlama ve sayfa işleyici veya belge yükleyen eylem yürütülmesi önce öznitelik değerlendirme oluşur. Bu nedenlerle, bildirim temelli yetkilendirme ile bir `[Authorize]` özniteliği yeterli olmaz. Bunun yerine, bir özel yetkilendirme yöntemi çağırabilirsiniz&mdash;kesinlik temelli yetkilendirme olarak bilinen bir stili.

Kullanım [örnek uygulamaları](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample)) Bu konuda açıklanan özellikleri keşfetmek için.

[Yetkilendirme tarafından korunan kullanıcı verileri ile ASP.NET Core uygulama oluşturma](xref:security/authorization/secure-data) kaynak tabanlı bir yetkilendirme kullanan örnek bir uygulama içerir.

## <a name="use-imperative-authorization"></a>Kesinlik temelli yetkilendirme kullanın

Yetkilendirme olarak gerçekleştirilen bir [IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice) hizmet ve hizmet koleksiyonu içinde kayıtlı `Startup` sınıfı. Hizmet aracılığıyla kullanılabilir hale getirileceğini [bağımlılık ekleme](xref:fundamentals/dependency-injection#fundamentals-dependency-injection) sayfa işleyicileri veya eylemler.

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Controllers/DocumentController.cs?name=snippet_IAuthServiceDI&highlight=6)]

`IAuthorizationService`iki sahip `AuthorizeAsync` yöntemi aşırı: kaynak ve ilke adını ve diğer kaynak ve değerlendirmek için gereksinimlerin listesi kabul bir kabul etme.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          IEnumerable<IAuthorizationRequirement> requirements);
Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          string policyName);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
Task<bool> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          IEnumerable<IAuthorizationRequirement> requirements);
Task<bool> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          string policyName);
```

---

<a name="security-authorization-resource-based-imperative"></a>

Aşağıdaki örnekte, güvenli olması için kaynağın özel yüklendi `Document` nesnesi. Bir `AuthorizeAsync` aşırı geçerli kullanıcının sağlanan belgeyi düzenlemesine izin verilip verilmediğini belirlemek için çağrılır. Özel bir "EditPolicy" yetkilendirme ilkesi karar katılır. Bkz: [özel ilke tabanlı yetkilendirme](xref:security/authorization/policies) yetkilendirme ilkeleri oluşturma hakkında daha fazla bilgi için.

> [!NOTE]
> Aşağıdaki kod örnekleri varsayın kimlik doğrulaması çalıştıktan ve kümesi `User` özelliği.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/Edit.cshtml.cs?name=snippet_DocumentEditHandler)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentEditAction)]

---

## <a name="write-a-resource-based-handler"></a>Kaynak tabanlı işleyicisi yazma

Kaynak tabanlı bir yetkilendirme çok farklı değildir için bir işleyici yazmak [düz gereksinimleri işleyicisi yazma](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler). Bir özel gereksinim sınıf oluşturun ve bir gereksinim işleyici sınıf uygulama. İşleyici sınıfını gereksinimi ve kaynak türünü belirtir. Örneğin, bir işleyici kullanan bir `SameAuthorRequirement` gereksinim ve `Document` kaynak arar:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

---

Gereksinim ve işleyicisinde kaydetmek `Startup.ConfigureServices` yöntemi:

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Startup.cs?name=snippet_ConfigureServicesSample&highlight=3-7,9)]

### <a name="operational-requirements"></a>İşletimsel gereksinimleri

CRUD sonuçları temel alarak kararları yapıyorsanız, (**C**oluştur, **R**okunur, **U**pdate'i, **D**Sil) işlemleri, kullanın [OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) yardımcı sınıfı. Bu sınıf, her işlem türü için ayrı bir sınıf yerine tek bir işleyici yazmanıza olanak sağlar. Kullanmak için bazı işlem adları sağlayın:

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_OperationsClass)]

İşleyici kullanılarak aşağıdaki gibi uygulanan bir `OperationAuthorizationRequirement` gereksinim ve `Document` kaynak:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

---

Kaynak, kullanıcının kimliğini ve gereksinimi 's kullanarak işlemi önceki işleyici doğrular `Name` özelliği.

Bir işlem kaynak işleyicisi çağırmak için işlem çağrılırken, belirtin `AuthorizeAsync` sayfası işleyicisi veya eylem. Aşağıdaki örnek, kimliği doğrulanmış kullanıcının sağlanan belgeyi görüntülemek için izin verilip verilmediğini belirler.

> [!NOTE]
> Aşağıdaki kod örnekleri varsayın kimlik doğrulaması çalıştıktan ve kümesi `User` özelliği.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/View.cshtml.cs?name=snippet_DocumentViewHandler&highlight=10-11)]

Yetkilendirme başarılı olursa, belgeyi görüntülemek için sayfanın döndürülür. Yetkilendirme başarısız olur, ancak kullanıcının kimliği doğrulanır varsa, döndürme `ForbidResult` yetkilendirme başarısız oldu herhangi bir kimlik doğrulaması ara yazılımı bildirir. A `ChallengeResult` kimlik doğrulaması gerçekleştirilmesi gereken döndürülür. Etkileşimli tarayıcı istemcileri için kullanıcının oturum açma sayfasına yeniden yönlendirmek uygun olabilir.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentViewAction&highlight=11-12)]

Yetkilendirme başarılı olursa, belge için görünümü döndürülür. Yetkilendirme başarısız olursa, döndürme `ChallengeResult` herhangi bir kimlik doğrulaması ara yazılımı yetkilendirme başarısız oldu ve ara yazılım uygun yanıtı alabilir bildirir. Uygun bir yanıt bir 401 veya 403 durum kodu döndürüyor. Etkileşimli tarayıcı istemcileri için kullanıcı bir oturum açma sayfasına yeniden yönlendirmeye anlamına gelebilir.

---
