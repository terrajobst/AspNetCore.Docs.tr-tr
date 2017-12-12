---
title: "Kimlik birincil anahtar veri türünü yapılandırın"
author: AdrienTorris
description: "Bu makalede, ASP.NET Core kimliği birincil anahtar için istenen veri türü yapılandırma adımlarını açıklar."
keywords: ASP.NET Core, kimlik, birincil anahtar
ms.author: scaddie
manager: wpickett
ms.date: 09/28/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity-primary-key-configuration
ms.openlocfilehash: 5734a9aa86fb2831bd054593ad41c3e3bda4729e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="configure-the-aspnet-core-identity-primary-key-data-type"></a><span data-ttu-id="834e6-104">ASP.NET Core kimliği birincil anahtar veri türünü yapılandırın</span><span class="sxs-lookup"><span data-stu-id="834e6-104">Configure the ASP.NET Core Identity primary key data type</span></span>

<span data-ttu-id="834e6-105">ASP.NET Core kimlik, bir birincil anahtar temsil etmek için kullanılan veri türü yapılandırmanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="834e6-105">ASP.NET Core Identity allows you to configure the data type used to represent a primary key.</span></span> <span data-ttu-id="834e6-106">Kimlik kullanır `string` varsayılan veri türü.</span><span class="sxs-lookup"><span data-stu-id="834e6-106">Identity uses the `string` data type by default.</span></span> <span data-ttu-id="834e6-107">Bu davranışı geçersiz kılabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="834e6-107">You can override this behavior.</span></span>

## <a name="customize-the-primary-key-data-type"></a><span data-ttu-id="834e6-108">Birincil anahtar veri türünü özelleştirme</span><span class="sxs-lookup"><span data-stu-id="834e6-108">Customize the primary key data type</span></span>

1. <span data-ttu-id="834e6-109">Özel bir uygulamasını oluşturma [IdentityUser](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuser-1) sınıfı.</span><span class="sxs-lookup"><span data-stu-id="834e6-109">Create a custom implementation of the [IdentityUser](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuser-1) class.</span></span> <span data-ttu-id="834e6-110">Kullanıcı nesneleri oluşturmak için kullanılacak türünü temsil eder.</span><span class="sxs-lookup"><span data-stu-id="834e6-110">It represents the type to be used for creating user objects.</span></span> <span data-ttu-id="834e6-111">Aşağıdaki örnekte, varsayılan `string` türü ile değiştirilir `Guid`.</span><span class="sxs-lookup"><span data-stu-id="834e6-111">In the following example, the default `string` type is replaced with `Guid`.</span></span>

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationUser.cs?highlight=4&range=7-13)]

1. <span data-ttu-id="834e6-112">Özel bir uygulamasını oluşturma [IdentityRole](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityrole-1) sınıfı.</span><span class="sxs-lookup"><span data-stu-id="834e6-112">Create a custom implementation of the [IdentityRole](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityrole-1) class.</span></span> <span data-ttu-id="834e6-113">Bu rol nesneleri oluşturmak için kullanılacak türünü temsil eder.</span><span class="sxs-lookup"><span data-stu-id="834e6-113">It represents the type to be used for creating role objects.</span></span> <span data-ttu-id="834e6-114">Aşağıdaki örnekte, varsayılan `string` türü ile değiştirilir `Guid`.</span><span class="sxs-lookup"><span data-stu-id="834e6-114">In the following example, the default `string` type is replaced with `Guid`.</span></span>
    
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationRole.cs?highlight=3&range=7-12)]
    
1. <span data-ttu-id="834e6-115">Özel veritabanı bağlamı sınıfının oluşturun.</span><span class="sxs-lookup"><span data-stu-id="834e6-115">Create a custom database context class.</span></span> <span data-ttu-id="834e6-116">Kimliği için kullanılan Entity Framework veritabanı bağlamı sınıfının devralır.</span><span class="sxs-lookup"><span data-stu-id="834e6-116">It inherits from the Entity Framework database context class used for Identity.</span></span> <span data-ttu-id="834e6-117">`TUser` Ve `TRole` bağımsız değişkenleri önceki adımda sırasıyla oluşturduğunuz özel kullanıcı ve rol sınıflarını başvuru.</span><span class="sxs-lookup"><span data-stu-id="834e6-117">The `TUser` and `TRole` arguments reference the custom user and role classes created in the previous step, respectively.</span></span> <span data-ttu-id="834e6-118">`Guid` Veri türü için birincil anahtar tanımlandı.</span><span class="sxs-lookup"><span data-stu-id="834e6-118">The `Guid` data type is defined for the primary key.</span></span>

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Data/ApplicationDbContext.cs?highlight=3&range=9-26)]
    
1. <span data-ttu-id="834e6-119">Özel veritabanı bağlamı sınıfının kimliği hizmeti uygulamanın başlangıç sınıfında eklerken kaydedin.</span><span class="sxs-lookup"><span data-stu-id="834e6-119">Register the custom database context class when adding the Identity service in the app's startup class.</span></span>

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="834e6-120">ASP.NET 2.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="834e6-120">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)
    
    <span data-ttu-id="834e6-121">`AddEntityFrameworkStores` Accept yöntemi olmayan bir `TKey` bağımsız olarak mı ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="834e6-121">The `AddEntityFrameworkStores` method doesn't accept a `TKey` argument as it did in ASP.NET Core 1.x.</span></span> <span data-ttu-id="834e6-122">Birincil anahtarın veri türü çözümleyerek algılanır `DbContext` nesnesi.</span><span class="sxs-lookup"><span data-stu-id="834e6-122">The primary key's data type is inferred by analyzing the `DbContext` object.</span></span>
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=6-8&range=25-37)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="834e6-123">ASP.NET 1.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="834e6-123">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)
    
    <span data-ttu-id="834e6-124">`AddEntityFrameworkStores` Yöntemi kabul eden bir `TKey` birincil anahtarın veri türü belirten bağımsız değişkeni.</span><span class="sxs-lookup"><span data-stu-id="834e6-124">The `AddEntityFrameworkStores` method accepts a `TKey` argument indicating the primary key's data type.</span></span>
    
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=9-11&range=39-55)]
    
    ---

## <a name="test-the-changes"></a><span data-ttu-id="834e6-125">Değişiklikleri test</span><span class="sxs-lookup"><span data-stu-id="834e6-125">Test the changes</span></span>

<span data-ttu-id="834e6-126">Yapılandırma değişiklikleri tamamladıktan sonra birincil anahtar temsil eden özellik yeni veri türünü gösterir.</span><span class="sxs-lookup"><span data-stu-id="834e6-126">Upon completion of the configuration changes, the property representing the primary key reflects the new data type.</span></span> <span data-ttu-id="834e6-127">Aşağıdaki örnek, bir MVC denetleyicisi özelliğinde erişme gösterir.</span><span class="sxs-lookup"><span data-stu-id="834e6-127">The following example demonstrates accessing the property in an MVC controller.</span></span>

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Controllers/AccountController.cs?name=snippet_GetCurrentUserId&highlight=6)]
