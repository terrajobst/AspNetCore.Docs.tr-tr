---
title: ASP.NET Core kimlik birincil anahtar veri türünü yapılandırın
author: AdrienTorris
description: ASP.NET Core kimliği birincil anahtar için istenen veri türü yapılandırma adımlarını hakkında bilgi edinin.
manager: wpickett
ms.author: scaddie
ms.date: 09/28/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/identity-primary-key-configuration
ms.openlocfilehash: ce654492dc7bab6c031c9f82555f877f642171ce
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
---
# <a name="configure-identity-primary-key-data-type-in-aspnet-core"></a>ASP.NET Core kimlik birincil anahtar veri türünü yapılandırın

ASP.NET Core kimlik, bir birincil anahtar temsil etmek için kullanılan veri türü yapılandırmanıza olanak sağlar. Kimlik kullanır `string` varsayılan veri türü. Bu davranışı geçersiz kılabilirsiniz.

## <a name="customize-the-primary-key-data-type"></a>Birincil anahtar veri türünü özelleştirme

1. Özel bir uygulamasını oluşturma [IdentityUser](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuser-1) sınıfı. Kullanıcı nesneleri oluşturmak için kullanılacak türünü temsil eder. Aşağıdaki örnekte, varsayılan `string` türü ile değiştirilir `Guid`.

    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationUser.cs?highlight=4&range=7-13)]

2. Özel bir uygulamasını oluşturma [IdentityRole](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identityrole-1) sınıfı. Bu rol nesneleri oluşturmak için kullanılacak türünü temsil eder. Aşağıdaki örnekte, varsayılan `string` türü ile değiştirilir `Guid`.

    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationRole.cs?highlight=3&range=7-12)]

3. Özel veritabanı bağlamı sınıfının oluşturun. Kimliği için kullanılan Entity Framework veritabanı bağlamı sınıfının devralır. `TUser` Ve `TRole` bağımsız değişkenleri önceki adımda sırasıyla oluşturduğunuz özel kullanıcı ve rol sınıflarını başvuru. `Guid` Veri türü için birincil anahtar tanımlandı.

    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Data/ApplicationDbContext.cs?highlight=3&range=9-26)]

4. Özel veritabanı bağlamı sınıfının kimliği hizmeti uygulamanın başlangıç sınıfında eklerken kaydedin.

   #### <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)
    `AddEntityFrameworkStores` Accept yöntemi olmayan bir `TKey` bağımsız olarak mı ASP.NET Core 1.x. Birincil anahtarın veri türü çözümleyerek algılanır `DbContext` nesnesi.

    [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=6-8&range=25-37)]

   #### <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)
    `AddEntityFrameworkStores` Yöntemi kabul eden bir `TKey` birincil anahtarın veri türü belirten bağımsız değişkeni.

    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=9-11&range=39-55)]

   * * *
## <a name="test-the-changes"></a>Değişiklikleri test

Yapılandırma değişiklikleri tamamladıktan sonra birincil anahtar temsil eden özellik yeni veri türünü gösterir. Aşağıdaki örnek, bir MVC denetleyicisi özelliğinde erişme gösterir.

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Controllers/AccountController.cs?name=snippet_GetCurrentUserId&highlight=6)]
