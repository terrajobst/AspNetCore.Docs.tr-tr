---
title: ASP.NET Core kaynak tabanlı yetkilendirme
author: scottaddie
description: Yetkilendirme özniteliği yeterli olmadığında kaynak tabanlı yetkilendirmeyi bir ASP.NET Core uygulamasına uygulamayı öğrenin.
ms.author: scaddie
ms.custom: mvc
ms.date: 11/15/2018
uid: security/authorization/resourcebased
ms.openlocfilehash: 835592521c714e270595e1448ae6e0aed1707b77
ms.sourcegitcommit: a166291c6708f5949c417874108332856b53b6a9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2019
ms.locfileid: "72589998"
---
# <a name="resource-based-authorization-in-aspnet-core"></a><span data-ttu-id="579a3-103">ASP.NET Core kaynak tabanlı yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="579a3-103">Resource-based authorization in ASP.NET Core</span></span>

<span data-ttu-id="579a3-104">Yetkilendirme stratejisi erişilmekte olan kaynağa bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="579a3-104">Authorization strategy depends upon the resource being accessed.</span></span> <span data-ttu-id="579a3-105">Yazar özelliği olan bir belge düşünün.</span><span class="sxs-lookup"><span data-stu-id="579a3-105">Consider a document that has an author property.</span></span> <span data-ttu-id="579a3-106">Yalnızca yazarın belgeyi güncelleştirmesine izin verilir.</span><span class="sxs-lookup"><span data-stu-id="579a3-106">Only the author is allowed to update the document.</span></span> <span data-ttu-id="579a3-107">Sonuç olarak, yetkilendirme değerlendirmesi gerçekleşebilmesi için belgenin veri deposundan alınması gerekir.</span><span class="sxs-lookup"><span data-stu-id="579a3-107">Consequently, the document must be retrieved from the data store before authorization evaluation can occur.</span></span>

<span data-ttu-id="579a3-108">Öznitelik değerlendirmesi, veri bağlamadan önce ve belgeyi yükleyen sayfa işleyicisinin veya eylemin yürütmeden önce oluşur.</span><span class="sxs-lookup"><span data-stu-id="579a3-108">Attribute evaluation occurs before data binding and before execution of the page handler or action that loads the document.</span></span> <span data-ttu-id="579a3-109">Bu nedenlerden dolayı, bir `[Authorize]` özniteliği ile bildirime dayalı yetkilendirme yok olacaktır.</span><span class="sxs-lookup"><span data-stu-id="579a3-109">For these reasons, declarative authorization with an `[Authorize]` attribute doesn't suffice.</span></span> <span data-ttu-id="579a3-110">Bunun yerine, zorunlu *Yetkilendirme*olarak bilinen &mdash;a stili özel bir yetkilendirme yöntemi çağırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="579a3-110">Instead, you can invoke a custom authorization method&mdash;a style known as *imperative authorization*.</span></span>

<span data-ttu-id="579a3-111">[Örnek kodu görüntüleyin veya indirin](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples) ([nasıl indirilir](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="579a3-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="579a3-112">[Yetkilendirme ile korunan kullanıcı verileriyle ASP.NET Core uygulama oluşturma](xref:security/authorization/secure-data) kaynak tabanlı yetkilendirme kullanan bir örnek uygulama içerir.</span><span class="sxs-lookup"><span data-stu-id="579a3-112">[Create an ASP.NET Core app with user data protected by authorization](xref:security/authorization/secure-data) contains a sample app that uses resource-based authorization.</span></span>

## <a name="use-imperative-authorization"></a><span data-ttu-id="579a3-113">Kesinlik temelli yetkilendirme kullan</span><span class="sxs-lookup"><span data-stu-id="579a3-113">Use imperative authorization</span></span>

<span data-ttu-id="579a3-114">Yetkilendirme bir [IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice) hizmeti olarak uygulanır ve `Startup` sınıfı içindeki hizmet koleksiyonuna kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="579a3-114">Authorization is implemented as an [IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice) service and is registered in the service collection within the `Startup` class.</span></span> <span data-ttu-id="579a3-115">Hizmet, sayfa işleyicilere veya eylemlere [bağımlılık ekleme](xref:fundamentals/dependency-injection) yoluyla kullanılabilir hale getirilir.</span><span class="sxs-lookup"><span data-stu-id="579a3-115">The service is made available via [dependency injection](xref:fundamentals/dependency-injection) to page handlers or actions.</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Controllers/DocumentController.cs?name=snippet_IAuthServiceDI&highlight=6)]

<span data-ttu-id="579a3-116">`IAuthorizationService` iki `AuthorizeAsync` yöntemi aşırı yüklemesi vardır: kaynağın ve ilke adının yanı sıra kaynağı kabul eden diğeri, değerlendirmek için gereksinimlerin bir listesi.</span><span class="sxs-lookup"><span data-stu-id="579a3-116">`IAuthorizationService` has two `AuthorizeAsync` method overloads: one accepting the resource and the policy name and the other accepting the resource and a list of requirements to evaluate.</span></span>

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

<span data-ttu-id="579a3-117">Aşağıdaki örnekte, güvenli hale getirilme kaynağı özel bir `Document` nesnesine yüklenir.</span><span class="sxs-lookup"><span data-stu-id="579a3-117">In the following example, the resource to be secured is loaded into a custom `Document` object.</span></span> <span data-ttu-id="579a3-118">Geçerli kullanıcının belirtilen belgeyi düzenlemesine izin verilip verilmeyeceğini belirlemekte bir `AuthorizeAsync` aşırı yüklemesi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="579a3-118">An `AuthorizeAsync` overload is invoked to determine whether the current user is allowed to edit the provided document.</span></span> <span data-ttu-id="579a3-119">Özel bir "EditPolicy" yetkilendirme ilkesi karara göre belirlenir.</span><span class="sxs-lookup"><span data-stu-id="579a3-119">A custom "EditPolicy" authorization policy is factored into the decision.</span></span> <span data-ttu-id="579a3-120">Yetkilendirme ilkeleri oluşturma hakkında daha fazla bilgi için bkz. [özel ilke tabanlı yetkilendirme](xref:security/authorization/policies) .</span><span class="sxs-lookup"><span data-stu-id="579a3-120">See [Custom policy-based authorization](xref:security/authorization/policies) for more on creating authorization policies.</span></span>

> [!NOTE]
> <span data-ttu-id="579a3-121">Aşağıdaki kod örnekleri, kimlik doğrulamasının çalıştırıldığını varsayar ve `User` özelliğini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="579a3-121">The following code samples assume authentication has run and set the `User` property.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/Edit.cshtml.cs?name=snippet_DocumentEditHandler)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentEditAction)]

::: moniker-end

## <a name="write-a-resource-based-handler"></a><span data-ttu-id="579a3-122">Kaynak tabanlı işleyici yazma</span><span class="sxs-lookup"><span data-stu-id="579a3-122">Write a resource-based handler</span></span>

<span data-ttu-id="579a3-123">Kaynak tabanlı yetkilendirme için bir işleyici yazmak, [düz gereksinimler işleyicisi yazmaktan](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler)çok farklı değildir.</span><span class="sxs-lookup"><span data-stu-id="579a3-123">Writing a handler for resource-based authorization isn't much different than [writing a plain requirements handler](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler).</span></span> <span data-ttu-id="579a3-124">Özel bir gereksinim sınıfı oluşturun ve bir gereksinim işleyicisi sınıfı uygulayın.</span><span class="sxs-lookup"><span data-stu-id="579a3-124">Create a custom requirement class, and implement a requirement handler class.</span></span> <span data-ttu-id="579a3-125">Gereksinim sınıfı oluşturma hakkında daha fazla bilgi için bkz. [Requirements](xref:security/authorization/policies#requirements).</span><span class="sxs-lookup"><span data-stu-id="579a3-125">For more information on creating a requirement class, see [Requirements](xref:security/authorization/policies#requirements).</span></span>

<span data-ttu-id="579a3-126">Handler sınıfı hem gereksinim hem de kaynak türünü belirtir.</span><span class="sxs-lookup"><span data-stu-id="579a3-126">The handler class specifies both the requirement and resource type.</span></span> <span data-ttu-id="579a3-127">Örneğin, bir `SameAuthorRequirement` ve `Document` kaynağı kullanan bir işleyici aşağıdaki gibidir:</span><span class="sxs-lookup"><span data-stu-id="579a3-127">For example, a handler utilizing a `SameAuthorRequirement` and a `Document` resource follows:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

::: moniker-end

<span data-ttu-id="579a3-128">Yukarıdaki örnekte, `SameAuthorRequirement` daha genel `SpecificAuthorRequirement` sınıfının özel bir durumu olduğunu düşünün.</span><span class="sxs-lookup"><span data-stu-id="579a3-128">In the preceding example, imagine that `SameAuthorRequirement` is a special case of a more generic `SpecificAuthorRequirement` class.</span></span> <span data-ttu-id="579a3-129">@No__t_0 sınıfı (gösterilmez) yazarın adını temsil eden bir `Name` özelliği içerir.</span><span class="sxs-lookup"><span data-stu-id="579a3-129">The `SpecificAuthorRequirement` class (not shown) contains a `Name` property representing the name of the author.</span></span> <span data-ttu-id="579a3-130">@No__t_0 özelliği geçerli kullanıcıya ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="579a3-130">The `Name` property could be set to the current user.</span></span>

<span data-ttu-id="579a3-131">Gereksinimi ve işleyiciyi `Startup.ConfigureServices` Kaydet:</span><span class="sxs-lookup"><span data-stu-id="579a3-131">Register the requirement and handler in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Startup.cs?name=snippet_ConfigureServicesSample&highlight=3-7,9)]

### <a name="operational-requirements"></a><span data-ttu-id="579a3-132">İşletimsel gereksinimler</span><span class="sxs-lookup"><span data-stu-id="579a3-132">Operational requirements</span></span>

<span data-ttu-id="579a3-133">CRUD (oluşturma, okuma, güncelleştirme, silme) işlemlerinin sonuçlarını temel alan kararlar verirken [Operationauthorizationrequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) yardımcı sınıfını kullanın.</span><span class="sxs-lookup"><span data-stu-id="579a3-133">If you're making decisions based on the outcomes of CRUD (Create, Read, Update, Delete) operations, use the [OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) helper class.</span></span> <span data-ttu-id="579a3-134">Bu sınıf her işlem türü için tek bir sınıf yerine tek bir işleyici yazmanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="579a3-134">This class enables you to write a single handler instead of an individual class for each operation type.</span></span> <span data-ttu-id="579a3-135">Kullanmak için, bazı işlem adlarını sağlayın:</span><span class="sxs-lookup"><span data-stu-id="579a3-135">To use it, provide some operation names:</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_OperationsClass)]

<span data-ttu-id="579a3-136">İşleyici, bir `OperationAuthorizationRequirement` gereksinimi ve `Document` kaynağı kullanılarak aşağıdaki gibi uygulanır:</span><span class="sxs-lookup"><span data-stu-id="579a3-136">The handler is implemented as follows, using an `OperationAuthorizationRequirement` requirement and a `Document` resource:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

::: moniker-end

<span data-ttu-id="579a3-137">Önceki işleyici, kaynağı, kullanıcının kimliğini ve gereksinimin `Name` özelliğini kullanarak işlemi doğrular.</span><span class="sxs-lookup"><span data-stu-id="579a3-137">The preceding handler validates the operation using the resource, the user's identity, and the requirement's `Name` property.</span></span>

## <a name="challenge-and-forbid-with-an-operational-resource-handler"></a><span data-ttu-id="579a3-138">İşletimsel kaynak işleyicisiyle Challenge ve fordeklarasyon</span><span class="sxs-lookup"><span data-stu-id="579a3-138">Challenge and forbid with an operational resource handler</span></span>

<span data-ttu-id="579a3-139">Bu bölümde, Challenge ve fordeklarasyon eylem sonuçlarının nasıl işlendiği ve çekişme ve fordeklarasyonu 'nin nasıl farklı olduğu gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="579a3-139">This section shows how the challenge and forbid action results are processed and how challenge and forbid differ.</span></span>

<span data-ttu-id="579a3-140">İşletimsel bir kaynak işleyicisini çağırmak için, sayfa işleyicinizde veya eylemde `AuthorizeAsync` çağırırken işlemi belirtin.</span><span class="sxs-lookup"><span data-stu-id="579a3-140">To call an operational resource handler, specify the operation when invoking `AuthorizeAsync` in your page handler or action.</span></span> <span data-ttu-id="579a3-141">Aşağıdaki örnek, kimliği doğrulanmış kullanıcının belirtilen belgeyi görüntülemesine izin verilip verilmeyeceğini belirler.</span><span class="sxs-lookup"><span data-stu-id="579a3-141">The following example determines whether the authenticated user is permitted to view the provided document.</span></span>

> [!NOTE]
> <span data-ttu-id="579a3-142">Aşağıdaki kod örnekleri, kimlik doğrulamasının çalıştırıldığını varsayar ve `User` özelliğini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="579a3-142">The following code samples assume authentication has run and set the `User` property.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/View.cshtml.cs?name=snippet_DocumentViewHandler&highlight=10-11)]

<span data-ttu-id="579a3-143">Yetkilendirme başarılı olursa belgeyi görüntüleme sayfası döndürülür.</span><span class="sxs-lookup"><span data-stu-id="579a3-143">If authorization succeeds, the page for viewing the document is returned.</span></span> <span data-ttu-id="579a3-144">Yetkilendirme başarısız olursa, ancak kullanıcının kimliği doğrulanırsa, `ForbidResult` döndürülüyor, yetkilendirme başarısız olan tüm kimlik doğrulama ara yazılımını bilgilendirir.</span><span class="sxs-lookup"><span data-stu-id="579a3-144">If authorization fails but the user is authenticated, returning `ForbidResult` informs any authentication middleware that authorization failed.</span></span> <span data-ttu-id="579a3-145">Kimlik doğrulaması gerçekleştirilmesi gerektiğinde bir `ChallengeResult` döndürülür.</span><span class="sxs-lookup"><span data-stu-id="579a3-145">A `ChallengeResult` is returned when authentication must be performed.</span></span> <span data-ttu-id="579a3-146">Etkileşimli tarayıcı istemcileri için, kullanıcıyı bir oturum açma sayfasına yönlendirmek uygun olabilir.</span><span class="sxs-lookup"><span data-stu-id="579a3-146">For interactive browser clients, it may be appropriate to redirect the user to a login page.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentViewAction&highlight=11-12)]

<span data-ttu-id="579a3-147">Yetkilendirme başarılı olursa belge görünümü döndürülür.</span><span class="sxs-lookup"><span data-stu-id="579a3-147">If authorization succeeds, the view for the document is returned.</span></span> <span data-ttu-id="579a3-148">Yetkilendirme başarısız olursa, döndüren `ChallengeResult` kimlik doğrulama ara yazılımı yetkilendirme başarısız olur ve ara yazılım uygun yanıtı alabilir.</span><span class="sxs-lookup"><span data-stu-id="579a3-148">If authorization fails, returning `ChallengeResult` informs any authentication middleware that authorization failed, and the middleware can take the appropriate response.</span></span> <span data-ttu-id="579a3-149">Uygun bir yanıt 401 veya 403 durum kodu döndürüyor olabilir.</span><span class="sxs-lookup"><span data-stu-id="579a3-149">An appropriate response could be returning a 401 or 403 status code.</span></span> <span data-ttu-id="579a3-150">Etkileşimli tarayıcı istemcileri için kullanıcıyı bir oturum açma sayfasına yeniden yönlendirmek anlamına gelebilir.</span><span class="sxs-lookup"><span data-stu-id="579a3-150">For interactive browser clients, it could mean redirecting the user to a login page.</span></span>

::: moniker-end
