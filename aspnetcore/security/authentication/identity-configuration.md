---
title: "ASP.NET Core kimliğini yapılandırmak"
author: AdrienTorris
description: "ASP.NET Core kimlik varsayılan değerleri anlamak ve özel değerleri kullanmak için çeşitli kimlik özelliklerini yapılandırın."
keywords: "ASP.NET Core, kimlik, kimlik doğrulaması, güvenlik"
ms.author: scaddie
manager: wpickett
ms.date: 09/18/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity-configuration
ms.openlocfilehash: 2861ca474e7e82da81943966394a92040ce96ab8
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="configure-identity"></a><span data-ttu-id="5a3aa-104">Kimliği yapılandırmak</span><span class="sxs-lookup"><span data-stu-id="5a3aa-104">Configure Identity</span></span>

<span data-ttu-id="5a3aa-105">ASP.NET Core kimliği vardır, uygulamanızın kolayca kılabilirsiniz bazı varsayılan davranışlar `Startup` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="5a3aa-105">ASP.NET Core Identity has some default behaviors that you can override easily in your application's `Startup` class.</span></span>

## <a name="passwords-policy"></a><span data-ttu-id="5a3aa-106">Parola İlkesi</span><span class="sxs-lookup"><span data-stu-id="5a3aa-106">Passwords policy</span></span>

<span data-ttu-id="5a3aa-107">Varsayılan olarak, parolalar bir büyük harf, küçük harf, rakam ve alfasayısal olmayan karakter içerdiğini kimlik gerektirir.</span><span class="sxs-lookup"><span data-stu-id="5a3aa-107">By default, Identity requires that passwords contain an uppercase character, lowercase character, a digit, and a non-alphanumeric character.</span></span> <span data-ttu-id="5a3aa-108">Diğer bazı kısıtlamalar vardır.</span><span class="sxs-lookup"><span data-stu-id="5a3aa-108">There are also some other restrictions.</span></span> <span data-ttu-id="5a3aa-109">Parola kısıtlamaları basitleştirmek istiyorsanız, bunu yapabilirsiniz `Startup` uygulamanızın sınıfı.</span><span class="sxs-lookup"><span data-stu-id="5a3aa-109">If you want to simplify password restrictions, you can do that in the `Startup` class of your application.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5a3aa-110">ASP.NET 2.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="5a3aa-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="5a3aa-111">ASP.NET Core eklenen 2.0 `RequiredUniqueChars` özelliği.</span><span class="sxs-lookup"><span data-stu-id="5a3aa-111">ASP.NET Core 2.0 added the `RequiredUniqueChars` property.</span></span> <span data-ttu-id="5a3aa-112">Aksi takdirde, ASP.NET Core aynı seçeneklerdir 1.x.</span><span class="sxs-lookup"><span data-stu-id="5a3aa-112">Otherwise, the options are the same from ASP.NET Core 1.x.</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5a3aa-113">ASP.NET 1.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="5a3aa-113">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

---

<span data-ttu-id="5a3aa-114">`IdentityOptions.Password`aşağıdaki özelliklere sahiptir:</span><span class="sxs-lookup"><span data-stu-id="5a3aa-114">`IdentityOptions.Password` has the following properties:</span></span>
* <span data-ttu-id="5a3aa-115">`RequireDigit`: 0-9 parolayı arasında bir sayı gerektirir.</span><span class="sxs-lookup"><span data-stu-id="5a3aa-115">`RequireDigit`: Requires a number between 0-9 in the password.</span></span> <span data-ttu-id="5a3aa-116">Varsayılan olarak true.</span><span class="sxs-lookup"><span data-stu-id="5a3aa-116">Defaults to true.</span></span>
* <span data-ttu-id="5a3aa-117">`RequiredLength`: Minimum parola uzunluğu.</span><span class="sxs-lookup"><span data-stu-id="5a3aa-117">`RequiredLength`: The minimum length of the password.</span></span> <span data-ttu-id="5a3aa-118">Varsayılan olarak 6.</span><span class="sxs-lookup"><span data-stu-id="5a3aa-118">Defaults to 6.</span></span>
* <span data-ttu-id="5a3aa-119">`RequireNonAlphanumeric`: Bir alfasayısal olmayan karakter parola gerektirir.</span><span class="sxs-lookup"><span data-stu-id="5a3aa-119">`RequireNonAlphanumeric`: Requires a non-alphanumeric character in the password.</span></span> <span data-ttu-id="5a3aa-120">Varsayılan olarak true.</span><span class="sxs-lookup"><span data-stu-id="5a3aa-120">Defaults to true.</span></span>
* <span data-ttu-id="5a3aa-121">`RequireUppercase`: Bir büyük harf karakter parola gerektirir.</span><span class="sxs-lookup"><span data-stu-id="5a3aa-121">`RequireUppercase`: Requires an upper case character in the password.</span></span> <span data-ttu-id="5a3aa-122">Varsayılan olarak true.</span><span class="sxs-lookup"><span data-stu-id="5a3aa-122">Defaults to true.</span></span>
* <span data-ttu-id="5a3aa-123">`RequireLowercase`: Bir küçük harf karakter parola gerektirir.</span><span class="sxs-lookup"><span data-stu-id="5a3aa-123">`RequireLowercase`: Requires a lower case character in the password.</span></span> <span data-ttu-id="5a3aa-124">Varsayılan olarak true.</span><span class="sxs-lookup"><span data-stu-id="5a3aa-124">Defaults to true.</span></span>
* <span data-ttu-id="5a3aa-125">`RequiredUniqueChars`: Parola ayrı karakter sayısını gerektirir.</span><span class="sxs-lookup"><span data-stu-id="5a3aa-125">`RequiredUniqueChars`: Requires the number of distinct characters in the password.</span></span> <span data-ttu-id="5a3aa-126">Varsayılan olarak 1.</span><span class="sxs-lookup"><span data-stu-id="5a3aa-126">Defaults to 1.</span></span>


## <a name="users-lockout"></a><span data-ttu-id="5a3aa-127">Kullanıcının kilitleme</span><span class="sxs-lookup"><span data-stu-id="5a3aa-127">User's lockout</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,39-42,50-52)]

<span data-ttu-id="5a3aa-128">`IdentityOptions.Lockout`aşağıdaki özelliklere sahiptir:</span><span class="sxs-lookup"><span data-stu-id="5a3aa-128">`IdentityOptions.Lockout` has the following properties:</span></span>
* <span data-ttu-id="5a3aa-129">`DefaultLockoutTimeSpan`: Bir kilitleme oluştuğunda süreyi bir kullanıcının kilitlendiği.</span><span class="sxs-lookup"><span data-stu-id="5a3aa-129">`DefaultLockoutTimeSpan`: The amount of time a user is locked out when a lockout occurs.</span></span> <span data-ttu-id="5a3aa-130">Varsayılan olarak 5 dakika.</span><span class="sxs-lookup"><span data-stu-id="5a3aa-130">Defaults to 5 minutes.</span></span>
* <span data-ttu-id="5a3aa-131">`MaxFailedAccessAttempts`: Kilitleme etkinse, bir kullanıcı kilitli kadar başarısız erişim denemesi sayısı.</span><span class="sxs-lookup"><span data-stu-id="5a3aa-131">`MaxFailedAccessAttempts`: The number of failed access attempts until a user is locked out, if lockout is enabled.</span></span> <span data-ttu-id="5a3aa-132">Varsayılan olarak 5.</span><span class="sxs-lookup"><span data-stu-id="5a3aa-132">Defaults to 5.</span></span>
* <span data-ttu-id="5a3aa-133">`AllowedForNewUsers`: Yeni bir kullanıcı kilitlenip kilitlenmeyeceği olmadığını belirler. Varsayılan olarak true.</span><span class="sxs-lookup"><span data-stu-id="5a3aa-133">`AllowedForNewUsers`: Determines if a new user can be locked out. Defaults to true.</span></span>


## <a name="sign-in-settings"></a><span data-ttu-id="5a3aa-134">Ayarları'nda oturum açın</span><span class="sxs-lookup"><span data-stu-id="5a3aa-134">Sign in settings</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)]

<span data-ttu-id="5a3aa-135">`IdentityOptions.SignIn`aşağıdaki özelliklere sahiptir:</span><span class="sxs-lookup"><span data-stu-id="5a3aa-135">`IdentityOptions.SignIn` has the following properties:</span></span>
* <span data-ttu-id="5a3aa-136">`RequireConfirmedEmail`: Oturum açmak için bir onay e-posta gerektirir.</span><span class="sxs-lookup"><span data-stu-id="5a3aa-136">`RequireConfirmedEmail`: Requires a confirmed email to sign in.</span></span> <span data-ttu-id="5a3aa-137">Varsayılan ayar FALSE seçeneğidir.</span><span class="sxs-lookup"><span data-stu-id="5a3aa-137">Defaults to false.</span></span>
* <span data-ttu-id="5a3aa-138">`RequireConfirmedPhoneNumber`: Oturum açmak onaylanan telefon numarası gerektirir.</span><span class="sxs-lookup"><span data-stu-id="5a3aa-138">`RequireConfirmedPhoneNumber`: Requires a confirmed phone number to sign in.</span></span> <span data-ttu-id="5a3aa-139">Varsayılan ayar FALSE seçeneğidir.</span><span class="sxs-lookup"><span data-stu-id="5a3aa-139">Defaults to false.</span></span>


## <a name="user-validation-settings"></a><span data-ttu-id="5a3aa-140">Kullanıcı doğrulama ayarları</span><span class="sxs-lookup"><span data-stu-id="5a3aa-140">User validation settings</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,48-52)]

<span data-ttu-id="5a3aa-141">`IdentityOptions.User`aşağıdaki özelliklere sahiptir:</span><span class="sxs-lookup"><span data-stu-id="5a3aa-141">`IdentityOptions.User` has the following properties:</span></span>
* <span data-ttu-id="5a3aa-142">`RequireUniqueEmail`: Her kullanıcının benzersiz bir e-posta olmasını gerektirir.</span><span class="sxs-lookup"><span data-stu-id="5a3aa-142">`RequireUniqueEmail`: Requires each User to have a unique email.</span></span> <span data-ttu-id="5a3aa-143">Varsayılan ayar FALSE seçeneğidir.</span><span class="sxs-lookup"><span data-stu-id="5a3aa-143">Defaults to false.</span></span>
* <span data-ttu-id="5a3aa-144">`AllowedUserNameCharacters`: Kullanıcı adı izin verilen karakter.</span><span class="sxs-lookup"><span data-stu-id="5a3aa-144">`AllowedUserNameCharacters`: Allowed characters in the username.</span></span> <span data-ttu-id="5a3aa-145">Varsayılan olarak abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+.</span><span class="sxs-lookup"><span data-stu-id="5a3aa-145">Defaults to abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+.</span></span>

## <a name="applications-cookie-settings"></a><span data-ttu-id="5a3aa-146">Uygulamanın tanımlama bilgisi ayarları</span><span class="sxs-lookup"><span data-stu-id="5a3aa-146">Application's cookie settings</span></span>

<span data-ttu-id="5a3aa-147">Parola İlkesi gibi uygulama tanımlama bilgisinin tüm ayarları değiştirilebilir `Startup` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="5a3aa-147">Like the passwords policy, all the settings of the application's cookie can be changed in the `Startup` class.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5a3aa-148">ASP.NET 2.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="5a3aa-148">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="5a3aa-149">Altında `ConfigureServices` içinde `Startup` sınıfı, uygulamanın tanımlama bilgisi yapılandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5a3aa-149">Under `ConfigureServices` in the `Startup` class, you can configure the application's cookie.</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5a3aa-150">ASP.NET 1.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="5a3aa-150">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

--- 

<span data-ttu-id="5a3aa-151">`CookieAuthenticationOptions`aşağıdaki özelliklere sahiptir:</span><span class="sxs-lookup"><span data-stu-id="5a3aa-151">`CookieAuthenticationOptions` has the following properties:</span></span>
* <span data-ttu-id="5a3aa-152">`Cookie.Name`: Tanımlama bilgisinin adı.</span><span class="sxs-lookup"><span data-stu-id="5a3aa-152">`Cookie.Name`: The name of the cookie.</span></span> <span data-ttu-id="5a3aa-153">Varsayılan olarak alır. AspNetCore.Cookies.</span><span class="sxs-lookup"><span data-stu-id="5a3aa-153">Defaults to .AspNetCore.Cookies.</span></span>
* <span data-ttu-id="5a3aa-154">`Cookie.HttpOnly`: Doğru olduğunda, tanımlama bilgisinin istemci tarafı komut dosyalarından erişilebilir değil.</span><span class="sxs-lookup"><span data-stu-id="5a3aa-154">`Cookie.HttpOnly`: When true, the cookie is not accessible from client-side scripts.</span></span> <span data-ttu-id="5a3aa-155">Varsayılan olarak true.</span><span class="sxs-lookup"><span data-stu-id="5a3aa-155">Defaults to true.</span></span>
* <span data-ttu-id="5a3aa-156">`ExpireTimeSpan`: Kimlik doğrulaması bileti tanımlama bilgisinde saklanan ne kadar süre oluşturulduğu noktadan itibaren geçerli kalacağını denetler.</span><span class="sxs-lookup"><span data-stu-id="5a3aa-156">`ExpireTimeSpan`: Controls how much time the authentication ticket stored in the cookie will remain valid from the point it is created.</span></span> <span data-ttu-id="5a3aa-157">Varsayılan olarak 14 gün.</span><span class="sxs-lookup"><span data-stu-id="5a3aa-157">Defaults to 14 days.</span></span>
* <span data-ttu-id="5a3aa-158">`LoginPath`: Bir kullanıcı yetkisiz olduğunda, bu oturum açma yoluna yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="5a3aa-158">`LoginPath`: When a user is unauthorized, they will be redirected to this path to login.</span></span> <span data-ttu-id="5a3aa-159">Varsayılan olarak/Account/oturum açma.</span><span class="sxs-lookup"><span data-stu-id="5a3aa-159">Defaults to /Account/Login.</span></span>
* <span data-ttu-id="5a3aa-160">`LogoutPath`: Bir kullanıcı oturum açtığında bu yoluna yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="5a3aa-160">`LogoutPath`: When a user is logged out, they will be redirected to this path.</span></span> <span data-ttu-id="5a3aa-161">Varsayılan olarak/Account/oturum kapatma.</span><span class="sxs-lookup"><span data-stu-id="5a3aa-161">Defaults to /Account/Logout.</span></span>
* <span data-ttu-id="5a3aa-162">`AccessDeniedPath`: Bir kullanıcı bir yetkilendirme denetim başarısız olduğunda, bu yolu yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="5a3aa-162">`AccessDeniedPath`: When a user fails an authorization check, they will be redirected to this path.</span></span> <span data-ttu-id="5a3aa-163">Varsayılan olarak/Account/erişim engellendi.</span><span class="sxs-lookup"><span data-stu-id="5a3aa-163">Defaults to /Account/AccessDenied.</span></span>
* <span data-ttu-id="5a3aa-164">`SlidingExpiration`: Doğru olduğunda, yeni bir tanımlama bilgisi zaman geçerli tanımlama bilgisi sona erme penceresinin yarısından fazlasını ilerlemiş yeni bir sona erme saati ile verilir.</span><span class="sxs-lookup"><span data-stu-id="5a3aa-164">`SlidingExpiration`: When true, a new cookie will be issued with a new expiration time when the current cookie is more than halfway through the expiration window.</span></span> <span data-ttu-id="5a3aa-165">Varsayılan olarak true.</span><span class="sxs-lookup"><span data-stu-id="5a3aa-165">Defaults to true.</span></span>
* <span data-ttu-id="5a3aa-166">`ReturnUrlParameter`: ReturnUrlParameter bir 401 yetkilendirilmedi durum kodu oturum açma yoluna 302 yeniden yönlendirme olarak değiştirildiğinde, hangi ara yazılım tarafından eklenen sorgu dizesi parametresinin adını belirler.</span><span class="sxs-lookup"><span data-stu-id="5a3aa-166">`ReturnUrlParameter`: The ReturnUrlParameter determines the name of the query string parameter which is appended by the middleware when a 401 Unauthorized status code is changed to a 302 redirect onto the login path.</span></span>
* <span data-ttu-id="5a3aa-167">`AuthenticationScheme`: Bu yalnızca ASP.NET Core için ilgili 1.x.</span><span class="sxs-lookup"><span data-stu-id="5a3aa-167">`AuthenticationScheme`: This is only relevant for ASP.NET Core 1.x.</span></span> <span data-ttu-id="5a3aa-168">Belirli kimlik doğrulaması düzeni için mantıksal adı.</span><span class="sxs-lookup"><span data-stu-id="5a3aa-168">The logical name for a particular authentication scheme.</span></span>
* <span data-ttu-id="5a3aa-169">`AutomaticAuthenticate`: Bu bayrağı yalnızca ASP.NET Core için ilgili 1.x.</span><span class="sxs-lookup"><span data-stu-id="5a3aa-169">`AutomaticAuthenticate`: This flag is only relevant for ASP.NET Core 1.x.</span></span> <span data-ttu-id="5a3aa-170">Doğru olduğunda, tanımlama bilgisi kimlik doğrulamasını her istek çalıştırın ve oluşturulan herhangi bir seri hale getirilmiş asıl yeniden yapılandırma ve doğrulama girişimi.</span><span class="sxs-lookup"><span data-stu-id="5a3aa-170">When true, cookie authentication should run on every request and attempt to validate and reconstruct any serialized principal it created.</span></span>

