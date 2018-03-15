---
title: "ASP.NET Core kimlik birincil anahtar veri türünü yapılandırın"
author: AdrienTorris
description: "ASP.NET Core kimliği birincil anahtar için istenen veri türü yapılandırma adımlarını hakkında bilgi edinin."
manager: wpickett
ms.author: scaddie
ms.date: 09/28/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/identity-primary-key-configuration
ms.openlocfilehash: 02482b81faa64b01765a90c2c6ffe9cf92b1a7e7
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/15/2018
---
# <a name="configure-identity-primary-key-data-type-in-aspnet-core"></a><span data-ttu-id="80fa2-103">ASP.NET Core kimlik birincil anahtar veri türünü yapılandırın</span><span class="sxs-lookup"><span data-stu-id="80fa2-103">Configure Identity primary key data type in ASP.NET Core</span></span>

<span data-ttu-id="80fa2-104">ASP.NET Core kimlik, bir birincil anahtar temsil etmek için kullanılan veri türü yapılandırmanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="80fa2-104">ASP.NET Core Identity allows you to configure the data type used to represent a primary key.</span></span> <span data-ttu-id="80fa2-105">Kimlik kullanır `string` varsayılan veri türü.</span><span class="sxs-lookup"><span data-stu-id="80fa2-105">Identity uses the `string` data type by default.</span></span> <span data-ttu-id="80fa2-106">Bu davranışı geçersiz kılabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="80fa2-106">You can override this behavior.</span></span>

## <a name="customize-the-primary-key-data-type"></a><span data-ttu-id="80fa2-107">Birincil anahtar veri türünü özelleştirme</span><span class="sxs-lookup"><span data-stu-id="80fa2-107">Customize the primary key data type</span></span>

1. <span data-ttu-id="80fa2-108">Özel bir uygulamasını oluşturma [IdentityUser](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuser-1) sınıfı.</span><span class="sxs-lookup"><span data-stu-id="80fa2-108">Create a custom implementation of the [IdentityUser](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuser-1) class.</span></span> <span data-ttu-id="80fa2-109">Kullanıcı nesneleri oluşturmak için kullanılacak türünü temsil eder.</span><span class="sxs-lookup"><span data-stu-id="80fa2-109">It represents the type to be used for creating user objects.</span></span> <span data-ttu-id="80fa2-110">Aşağıdaki örnekte, varsayılan `string` türü ile değiştirilir `Guid`.</span><span class="sxs-lookup"><span data-stu-id="80fa2-110">In the following example, the default `string` type is replaced with `Guid`.</span></span>

    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationUser.cs?highlight=4&range=7-13)]

1. <span data-ttu-id="80fa2-111">Özel bir uygulamasını oluşturma [IdentityRole](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityrole-1) sınıfı.</span><span class="sxs-lookup"><span data-stu-id="80fa2-111">Create a custom implementation of the [IdentityRole](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityrole-1) class.</span></span> <span data-ttu-id="80fa2-112">Bu rol nesneleri oluşturmak için kullanılacak türünü temsil eder.</span><span class="sxs-lookup"><span data-stu-id="80fa2-112">It represents the type to be used for creating role objects.</span></span> <span data-ttu-id="80fa2-113">Aşağıdaki örnekte, varsayılan `string` türü ile değiştirilir `Guid`.</span><span class="sxs-lookup"><span data-stu-id="80fa2-113">In the following example, the default `string` type is replaced with `Guid`.</span></span>
    
    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationRole.cs?highlight=3&range=7-12)]
    
1. <span data-ttu-id="80fa2-114">Özel veritabanı bağlamı sınıfının oluşturun.</span><span class="sxs-lookup"><span data-stu-id="80fa2-114">Create a custom database context class.</span></span> <span data-ttu-id="80fa2-115">Kimliği için kullanılan Entity Framework veritabanı bağlamı sınıfının devralır.</span><span class="sxs-lookup"><span data-stu-id="80fa2-115">It inherits from the Entity Framework database context class used for Identity.</span></span> <span data-ttu-id="80fa2-116">`TUser` Ve `TRole` bağımsız değişkenleri önceki adımda sırasıyla oluşturduğunuz özel kullanıcı ve rol sınıflarını başvuru.</span><span class="sxs-lookup"><span data-stu-id="80fa2-116">The `TUser` and `TRole` arguments reference the custom user and role classes created in the previous step, respectively.</span></span> <span data-ttu-id="80fa2-117">`Guid` Veri türü için birincil anahtar tanımlandı.</span><span class="sxs-lookup"><span data-stu-id="80fa2-117">The `Guid` data type is defined for the primary key.</span></span>

    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Data/ApplicationDbContext.cs?highlight=3&range=9-26)]
    
1. <span data-ttu-id="80fa2-118">Özel veritabanı bağlamı sınıfının kimliği hizmeti uygulamanın başlangıç sınıfında eklerken kaydedin.</span><span class="sxs-lookup"><span data-stu-id="80fa2-118">Register the custom database context class when adding the Identity service in the app's startup class.</span></span>

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="80fa2-119">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="80fa2-119">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)
    
    <span data-ttu-id="80fa2-120">`AddEntityFrameworkStores` Accept yöntemi olmayan bir `TKey` bağımsız olarak mı ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="80fa2-120">The `AddEntityFrameworkStores` method doesn't accept a `TKey` argument as it did in ASP.NET Core 1.x.</span></span> <span data-ttu-id="80fa2-121">Birincil anahtarın veri türü çözümleyerek algılanır `DbContext` nesnesi.</span><span class="sxs-lookup"><span data-stu-id="80fa2-121">The primary key's data type is inferred by analyzing the `DbContext` object.</span></span>
    
    [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=6-8&range=25-37)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="80fa2-122">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="80fa2-122">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)
    
    <span data-ttu-id="80fa2-123">`AddEntityFrameworkStores` Yöntemi kabul eden bir `TKey` birincil anahtarın veri türü belirten bağımsız değişkeni.</span><span class="sxs-lookup"><span data-stu-id="80fa2-123">The `AddEntityFrameworkStores` method accepts a `TKey` argument indicating the primary key's data type.</span></span>
    
    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=9-11&range=39-55)]
    
    ---

## <a name="test-the-changes"></a><span data-ttu-id="80fa2-124">Değişiklikleri test</span><span class="sxs-lookup"><span data-stu-id="80fa2-124">Test the changes</span></span>

<span data-ttu-id="80fa2-125">Yapılandırma değişiklikleri tamamladıktan sonra birincil anahtar temsil eden özellik yeni veri türünü gösterir.</span><span class="sxs-lookup"><span data-stu-id="80fa2-125">Upon completion of the configuration changes, the property representing the primary key reflects the new data type.</span></span> <span data-ttu-id="80fa2-126">Aşağıdaki örnek, bir MVC denetleyicisi özelliğinde erişme gösterir.</span><span class="sxs-lookup"><span data-stu-id="80fa2-126">The following example demonstrates accessing the property in an MVC controller.</span></span>

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Controllers/AccountController.cs?name=snippet_GetCurrentUserId&highlight=6)]
