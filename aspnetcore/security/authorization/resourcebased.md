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
# <a name="resource-based-authorization-in-aspnet-core"></a><span data-ttu-id="22536-103">ASP.NET core'da kaynak tabanlı yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="22536-103">Resource-based authorization in ASP.NET Core</span></span>

<span data-ttu-id="22536-104">Erişilen kaynak yetkilendirme stratejisi bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="22536-104">Authorization strategy depends upon the resource being accessed.</span></span> <span data-ttu-id="22536-105">Bir yazar özelliğine sahip bir belge göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="22536-105">Consider a document that has an author property.</span></span> <span data-ttu-id="22536-106">Yalnızca yazarın belge güncelleştirmesi izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="22536-106">Only the author is allowed to update the document.</span></span> <span data-ttu-id="22536-107">Sonuç olarak, yetkilendirme değerlendirmesi gerçekleşebilmesi belge veri deposundan alınması gerekir.</span><span class="sxs-lookup"><span data-stu-id="22536-107">Consequently, the document must be retrieved from the data store before authorization evaluation can occur.</span></span>

<span data-ttu-id="22536-108">Öznitelik değerlendirme, veri bağlama ve sayfa işleyicisi veya belge yükler eylem yürütme önce gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="22536-108">Attribute evaluation occurs before data binding and before execution of the page handler or action that loads the document.</span></span> <span data-ttu-id="22536-109">Bu nedenlerle, bildirim temelli yetkilendirme ile bir `[Authorize]` değil öznitelik yeterli.</span><span class="sxs-lookup"><span data-stu-id="22536-109">For these reasons, declarative authorization with an `[Authorize]` attribute doesn't suffice.</span></span> <span data-ttu-id="22536-110">Bunun yerine, bir özel yetkilendirme yöntemi çağırabilirsiniz&mdash;olarak bilinen bir stil *kesinlik temelli yetkilendirme*.</span><span class="sxs-lookup"><span data-stu-id="22536-110">Instead, you can invoke a custom authorization method&mdash;a style known as *imperative authorization*.</span></span>

<span data-ttu-id="22536-111">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="22536-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="22536-112">[Kullanıcı verilerinin yetkilendirme tarafından korunduğu ile bir ASP.NET Core uygulaması oluşturma](xref:security/authorization/secure-data) kaynak tabanlı yetkilendirme kullanan bir örnek uygulamayı içerir.</span><span class="sxs-lookup"><span data-stu-id="22536-112">[Create an ASP.NET Core app with user data protected by authorization](xref:security/authorization/secure-data) contains a sample app that uses resource-based authorization.</span></span>

## <a name="use-imperative-authorization"></a><span data-ttu-id="22536-113">Kesinlik temelli yetkilendirme kullanın</span><span class="sxs-lookup"><span data-stu-id="22536-113">Use imperative authorization</span></span>

<span data-ttu-id="22536-114">Yetkilendirme olarak gerçekleştirilen bir [Iauthorizationservice](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice) hizmet ve hizmet koleksiyonu içinde kayıtlı `Startup` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="22536-114">Authorization is implemented as an [IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice) service and is registered in the service collection within the `Startup` class.</span></span> <span data-ttu-id="22536-115">Hizmet aracılığıyla kullanılabilir hale getirileceğini [bağımlılık ekleme](xref:fundamentals/dependency-injection) sayfa işleyicileri veya eylemler.</span><span class="sxs-lookup"><span data-stu-id="22536-115">The service is made available via [dependency injection](xref:fundamentals/dependency-injection) to page handlers or actions.</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Controllers/DocumentController.cs?name=snippet_IAuthServiceDI&highlight=6)]

<span data-ttu-id="22536-116">`IAuthorizationService` iki `AuthorizeAsync` yöntemi aşırı yüklemeleri: kaynak ve ilke adını ve diğer kaynak ve değerlendirmek için gereksinimler listesini kabul eden bir kabul etme.</span><span class="sxs-lookup"><span data-stu-id="22536-116">`IAuthorizationService` has two `AuthorizeAsync` method overloads: one accepting the resource and the policy name and the other accepting the resource and a list of requirements to evaluate.</span></span>

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

<span data-ttu-id="22536-117">Aşağıdaki örnekte, özel kaynağının korunması için yüklenen `Document` nesne.</span><span class="sxs-lookup"><span data-stu-id="22536-117">In the following example, the resource to be secured is loaded into a custom `Document` object.</span></span> <span data-ttu-id="22536-118">Bir `AuthorizeAsync` aşırı yükleme, geçerli kullanıcının belirtilen belgeyi düzenlemek için izin verilip verilmeyeceğini belirlemek için çağrılır.</span><span class="sxs-lookup"><span data-stu-id="22536-118">An `AuthorizeAsync` overload is invoked to determine whether the current user is allowed to edit the provided document.</span></span> <span data-ttu-id="22536-119">Özel bir "EditPolicy" yetkilendirme ilkesi karar katılır.</span><span class="sxs-lookup"><span data-stu-id="22536-119">A custom "EditPolicy" authorization policy is factored into the decision.</span></span> <span data-ttu-id="22536-120">Bkz: [özel ilke tabanlı yetkilendirme](xref:security/authorization/policies) yetkilendirme ilkeleri oluşturma hakkında daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="22536-120">See [Custom policy-based authorization](xref:security/authorization/policies) for more on creating authorization policies.</span></span>

> [!NOTE]
> <span data-ttu-id="22536-121">Aşağıdaki kod örnekleri kimlik doğrulaması çalıştırıldığı varsayılır ve kümesi `User` özelliği.</span><span class="sxs-lookup"><span data-stu-id="22536-121">The following code samples assume authentication has run and set the `User` property.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/Edit.cshtml.cs?name=snippet_DocumentEditHandler)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentEditAction)]

::: moniker-end

## <a name="write-a-resource-based-handler"></a><span data-ttu-id="22536-122">Bir kaynak tabanlı işleyicisi yazma</span><span class="sxs-lookup"><span data-stu-id="22536-122">Write a resource-based handler</span></span>

<span data-ttu-id="22536-123">Kaynak tabanlı yetkilendirme çok farklı değildir için bir işleyici yazma [düz gereksinimleri işleyicisi yazma](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler).</span><span class="sxs-lookup"><span data-stu-id="22536-123">Writing a handler for resource-based authorization isn't much different than [writing a plain requirements handler](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler).</span></span> <span data-ttu-id="22536-124">Özel gereksinim sınıfı oluşturun ve bir gereksinim işleyicisi sınıfı.</span><span class="sxs-lookup"><span data-stu-id="22536-124">Create a custom requirement class, and implement a requirement handler class.</span></span> <span data-ttu-id="22536-125">Bir gereksinim sınıfı oluşturma hakkında daha fazla bilgi için bkz. [gereksinimleri](xref:security/authorization/policies#requirements).</span><span class="sxs-lookup"><span data-stu-id="22536-125">For more information on creating a requirement class, see [Requirements](xref:security/authorization/policies#requirements).</span></span>

<span data-ttu-id="22536-126">İşleyici sınıfı, hem gereksinim hem de kaynak türünü belirtir.</span><span class="sxs-lookup"><span data-stu-id="22536-126">The handler class specifies both the requirement and resource type.</span></span> <span data-ttu-id="22536-127">Örneğin, bir işleyici kullanan bir `SameAuthorRequirement` ve `Document` kaynak aşağıdadır:</span><span class="sxs-lookup"><span data-stu-id="22536-127">For example, a handler utilizing a `SameAuthorRequirement` and a `Document` resource follows:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

::: moniker-end

<span data-ttu-id="22536-128">Önceki örnekte, Imagine `SameAuthorRequirement` daha fazla genel, özel bir durumdur `SpecificAuthorRequirement` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="22536-128">In the preceding example, imagine that `SameAuthorRequirement` is a special case of a more generic `SpecificAuthorRequirement` class.</span></span> <span data-ttu-id="22536-129">`SpecificAuthorRequirement` (Gösterilmemiştir) sınıfı içeren bir `Name` yazarının adını temsil eden özellik.</span><span class="sxs-lookup"><span data-stu-id="22536-129">The `SpecificAuthorRequirement` class (not shown) contains a `Name` property representing the name of the author.</span></span> <span data-ttu-id="22536-130">`Name` Özelliği, geçerli kullanıcı için ayarlanmış olması.</span><span class="sxs-lookup"><span data-stu-id="22536-130">The `Name` property could be set to the current user.</span></span>

<span data-ttu-id="22536-131">Kaydetme gereksinimi ve işleyicisinde `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="22536-131">Register the requirement and handler in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Startup.cs?name=snippet_ConfigureServicesSample&highlight=3-7,9)]

### <a name="operational-requirements"></a><span data-ttu-id="22536-132">İşletimsel gereksinimleri</span><span class="sxs-lookup"><span data-stu-id="22536-132">Operational requirements</span></span>

<span data-ttu-id="22536-133">CRUD (oluşturma, okuma, güncelleştirme, silme) işlemlerinin sonuçları temel alarak kararları yapıyorsanız kullanmak [OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) yardımcı sınıfı.</span><span class="sxs-lookup"><span data-stu-id="22536-133">If you're making decisions based on the outcomes of CRUD (Create, Read, Update, Delete) operations, use the [OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) helper class.</span></span> <span data-ttu-id="22536-134">Bu sınıf, her işlem türü için ayrı bir sınıf yerine tek bir işleyici yazmanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="22536-134">This class enables you to write a single handler instead of an individual class for each operation type.</span></span> <span data-ttu-id="22536-135">Bunu kullanmak için bazı işlem adları sağlar:</span><span class="sxs-lookup"><span data-stu-id="22536-135">To use it, provide some operation names:</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_OperationsClass)]

<span data-ttu-id="22536-136">İşleyicisi aşağıdaki gibi kullanılarak uygulanan bir `OperationAuthorizationRequirement` gereksinim ve `Document` kaynak:</span><span class="sxs-lookup"><span data-stu-id="22536-136">The handler is implemented as follows, using an `OperationAuthorizationRequirement` requirement and a `Document` resource:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

::: moniker-end

<span data-ttu-id="22536-137">Kaynak ve kullanıcının kimliğini gereksinimin işlemi önceki işleyici doğrular `Name` özelliği.</span><span class="sxs-lookup"><span data-stu-id="22536-137">The preceding handler validates the operation using the resource, the user's identity, and the requirement's `Name` property.</span></span>

<span data-ttu-id="22536-138">Bir operasyonel kaynak işleyicisi çağırmak için işlemi çağrılırken belirtin `AuthorizeAsync` sayfası işleyicisi veya eylem.</span><span class="sxs-lookup"><span data-stu-id="22536-138">To call an operational resource handler, specify the operation when invoking `AuthorizeAsync` in your page handler or action.</span></span> <span data-ttu-id="22536-139">Aşağıdaki örnek, kimliği doğrulanmış kullanıcı belirtilen belgeyi görüntülemek için izinli olup olmadığını belirler.</span><span class="sxs-lookup"><span data-stu-id="22536-139">The following example determines whether the authenticated user is permitted to view the provided document.</span></span>

> [!NOTE]
> <span data-ttu-id="22536-140">Aşağıdaki kod örnekleri kimlik doğrulaması çalıştırıldığı varsayılır ve kümesi `User` özelliği.</span><span class="sxs-lookup"><span data-stu-id="22536-140">The following code samples assume authentication has run and set the `User` property.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/View.cshtml.cs?name=snippet_DocumentViewHandler&highlight=10-11)]

<span data-ttu-id="22536-141">Yetkilendirme başarılı olursa, belgeyi görüntülemek için sayfanın döndürülür.</span><span class="sxs-lookup"><span data-stu-id="22536-141">If authorization succeeds, the page for viewing the document is returned.</span></span> <span data-ttu-id="22536-142">Yetkilendirme başarısız olur ancak kullanıcının kimliği doğrulanır ve varsa, döndüren `ForbidResult` yetkilendirme başarısız oldu. kimlik doğrulaması ara yazılımı konusunda bilgilendirir.</span><span class="sxs-lookup"><span data-stu-id="22536-142">If authorization fails but the user is authenticated, returning `ForbidResult` informs any authentication middleware that authorization failed.</span></span> <span data-ttu-id="22536-143">A `ChallengeResult` kimlik doğrulaması gerçekleştirilmesi gereken döndürülür.</span><span class="sxs-lookup"><span data-stu-id="22536-143">A `ChallengeResult` is returned when authentication must be performed.</span></span> <span data-ttu-id="22536-144">Etkileşimli tarayıcı istemcileri için kullanıcının oturum açma sayfasına yeniden yönlendirmek uygun olabilir.</span><span class="sxs-lookup"><span data-stu-id="22536-144">For interactive browser clients, it may be appropriate to redirect the user to a login page.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentViewAction&highlight=11-12)]

<span data-ttu-id="22536-145">Yetkilendirme başarılı olursa, belge görünümü döndürülür.</span><span class="sxs-lookup"><span data-stu-id="22536-145">If authorization succeeds, the view for the document is returned.</span></span> <span data-ttu-id="22536-146">Yetkilendirme başarısız olursa döndüren `ChallengeResult` herhangi bir kimlik doğrulaması ara yazılım kimlik doğrulaması başarısız oldu ve ara yazılım uygun yanıtı alabilir bildirir.</span><span class="sxs-lookup"><span data-stu-id="22536-146">If authorization fails, returning `ChallengeResult` informs any authentication middleware that authorization failed, and the middleware can take the appropriate response.</span></span> <span data-ttu-id="22536-147">Uygun bir yanıt durum kodu 401 veya 403 döndürüyor.</span><span class="sxs-lookup"><span data-stu-id="22536-147">An appropriate response could be returning a 401 or 403 status code.</span></span> <span data-ttu-id="22536-148">Etkileşimli tarayıcı istemcileri için kullanıcı bir oturum açma sayfasına yeniden yönlendiriliyorsunuz gelebilir.</span><span class="sxs-lookup"><span data-stu-id="22536-148">For interactive browser clients, it could mean redirecting the user to a login page.</span></span>

::: moniker-end
