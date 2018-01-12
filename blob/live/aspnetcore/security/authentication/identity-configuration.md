---
title: "ASP.NET Core kimliğini yapılandırmak"
author: AdrienTorris
description: "ASP.NET Core kimlik varsayılan değerleri anlamak ve özel değerleri kullanmak için çeşitli kimlik özelliklerini yapılandırın."
keywords: "ASP.NET Core, kimlik, kimlik doğrulaması, güvenlik"
ms.author: scaddie
manager: wpickett
ms.date: 01/11/2018
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity-configuration
ms.openlocfilehash: ac204cb89aac1f90adc64c4f0bec4e946cb8c4d9
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/11/2018
---
# <a name="configure-identity"></a><span data-ttu-id="81f44-104">Kimliği yapılandırmak</span><span class="sxs-lookup"><span data-stu-id="81f44-104">Configure Identity</span></span>

<span data-ttu-id="81f44-105">ASP.NET Core kimliği parola ilkesi, kilitleme süresini ve uygulamanızın kolayca kılabilirsiniz tanımlama bilgisi ayarları gibi uygulamalarda ortak davranışları sahip `Startup` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="81f44-105">ASP.NET Core Identity has common behaviors in applications such as password policy, lockout time, and cookie settings that you can override easily in your application's `Startup` class.</span></span>

## <a name="passwords-policy"></a><span data-ttu-id="81f44-106">Parola İlkesi</span><span class="sxs-lookup"><span data-stu-id="81f44-106">Passwords policy</span></span>

<span data-ttu-id="81f44-107">Varsayılan olarak, parolalar bir büyük harf, küçük harf, rakam ve alfasayısal olmayan karakter içerdiğini kimlik gerektirir.</span><span class="sxs-lookup"><span data-stu-id="81f44-107">By default, Identity requires that passwords contain an uppercase character, lowercase character, a digit, and a non-alphanumeric character.</span></span> <span data-ttu-id="81f44-108">Diğer bazı kısıtlamalar vardır.</span><span class="sxs-lookup"><span data-stu-id="81f44-108">There are also some other restrictions.</span></span> <span data-ttu-id="81f44-109">Parola kısıtlamaları basitleştirmek için değişiklik `ConfigureServices` yöntemi `Startup` uygulamanızın sınıfı.</span><span class="sxs-lookup"><span data-stu-id="81f44-109">To simplify password restrictions, modify the `ConfigureServices` method of the `Startup` class of your application.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="81f44-110">ASP.NET 2.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="81f44-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="81f44-111">ASP.NET Core eklenen 2.0 `RequiredUniqueChars` özelliği.</span><span class="sxs-lookup"><span data-stu-id="81f44-111">ASP.NET Core 2.0 added the `RequiredUniqueChars` property.</span></span> <span data-ttu-id="81f44-112">Aksi takdirde, ASP.NET Core aynı seçeneklerdir 1.x.</span><span class="sxs-lookup"><span data-stu-id="81f44-112">Otherwise, the options are the same from ASP.NET Core 1.x.</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="81f44-113">ASP.NET 1.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="81f44-113">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

---

<span data-ttu-id="81f44-114">`IdentityOptions.Password`aşağıdaki özelliklere sahiptir:</span><span class="sxs-lookup"><span data-stu-id="81f44-114">`IdentityOptions.Password` has the following properties:</span></span>

| <span data-ttu-id="81f44-115">Özellik</span><span class="sxs-lookup"><span data-stu-id="81f44-115">Property</span></span>                | <span data-ttu-id="81f44-116">Açıklama</span><span class="sxs-lookup"><span data-stu-id="81f44-116">Description</span></span>                       | <span data-ttu-id="81f44-117">Varsayılan</span><span class="sxs-lookup"><span data-stu-id="81f44-117">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `RequireDigit`          | <span data-ttu-id="81f44-118">Parola 0-9 arasında bir sayı gerektirir.</span><span class="sxs-lookup"><span data-stu-id="81f44-118">Requires a number between 0-9 in the password.</span></span> | <span data-ttu-id="81f44-119">true</span><span class="sxs-lookup"><span data-stu-id="81f44-119">true</span></span> |
| `RequiredLength`        | <span data-ttu-id="81f44-120">Minimum parola uzunluğu.</span><span class="sxs-lookup"><span data-stu-id="81f44-120">The minimum length of the password.</span></span> | <span data-ttu-id="81f44-121">6</span><span class="sxs-lookup"><span data-stu-id="81f44-121">6</span></span> |
| `RequireNonAlphanumeric`| <span data-ttu-id="81f44-122">Parolada alfasayısal olmayan karakter gerektirir.</span><span class="sxs-lookup"><span data-stu-id="81f44-122">Requires a non-alphanumeric character in the password.</span></span> | <span data-ttu-id="81f44-123">true</span><span class="sxs-lookup"><span data-stu-id="81f44-123">true</span></span> |
| `RequireUppercase`      | <span data-ttu-id="81f44-124">Bir büyük harf karakter parola gerektirir.</span><span class="sxs-lookup"><span data-stu-id="81f44-124">Requires an upper case character in the password.</span></span> | <span data-ttu-id="81f44-125">true</span><span class="sxs-lookup"><span data-stu-id="81f44-125">true</span></span> |
| `RequireLowercase`      | <span data-ttu-id="81f44-126">Parola küçük harf karakter gerektirir.</span><span class="sxs-lookup"><span data-stu-id="81f44-126">Requires a lower case character in the password.</span></span> | <span data-ttu-id="81f44-127">true</span><span class="sxs-lookup"><span data-stu-id="81f44-127">true</span></span> |
| `RequiredUniqueChars`   | <span data-ttu-id="81f44-128">Parola ayrı karakter sayısını gerektirir.</span><span class="sxs-lookup"><span data-stu-id="81f44-128">Requires the number of distinct characters in the password.</span></span> | <span data-ttu-id="81f44-129">1.</span><span class="sxs-lookup"><span data-stu-id="81f44-129">1</span></span> |


## <a name="users-lockout"></a><span data-ttu-id="81f44-130">Kullanıcının kilitleme</span><span class="sxs-lookup"><span data-stu-id="81f44-130">User's lockout</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,39-42,50-52)]

<span data-ttu-id="81f44-131">`IdentityOptions.Lockout`aşağıdaki özelliklere sahiptir:</span><span class="sxs-lookup"><span data-stu-id="81f44-131">`IdentityOptions.Lockout` has the following properties:</span></span>

| <span data-ttu-id="81f44-132">Özellik</span><span class="sxs-lookup"><span data-stu-id="81f44-132">Property</span></span>                | <span data-ttu-id="81f44-133">Açıklama</span><span class="sxs-lookup"><span data-stu-id="81f44-133">Description</span></span>                       | <span data-ttu-id="81f44-134">Varsayılan</span><span class="sxs-lookup"><span data-stu-id="81f44-134">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `DefaultLockoutTimeSpan` | <span data-ttu-id="81f44-135">Bir kilitleme oluştuğunda süreyi bir kullanıcının kilitlendiği.</span><span class="sxs-lookup"><span data-stu-id="81f44-135">The amount of time a user is locked out when a lockout occurs.</span></span>  | <span data-ttu-id="81f44-136">5 dakika</span><span class="sxs-lookup"><span data-stu-id="81f44-136">5 minutes</span></span>  |
| `MaxFailedAccessAttempts` | <span data-ttu-id="81f44-137">Kilitleme etkinse, bir kullanıcı kilitli kadar başarısız erişim denemesi sayısı.</span><span class="sxs-lookup"><span data-stu-id="81f44-137">The number of failed access attempts until a user is locked out, if lockout is enabled.</span></span>  | <span data-ttu-id="81f44-138">5</span><span class="sxs-lookup"><span data-stu-id="81f44-138">5</span></span> |
| `AllowedForNewUsers` | <span data-ttu-id="81f44-139">Yeni bir kullanıcı kilitlenip kilitlenmeyeceği olmadığını belirler.</span><span class="sxs-lookup"><span data-stu-id="81f44-139">Determines if a new user can be locked out.</span></span>  | <span data-ttu-id="81f44-140">true</span><span class="sxs-lookup"><span data-stu-id="81f44-140">true</span></span> |

## <a name="sign-in-settings"></a><span data-ttu-id="81f44-141">Ayarları'nda oturum açın</span><span class="sxs-lookup"><span data-stu-id="81f44-141">Sign in settings</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)]

<span data-ttu-id="81f44-142">`IdentityOptions.SignIn`aşağıdaki özelliklere sahiptir:</span><span class="sxs-lookup"><span data-stu-id="81f44-142">`IdentityOptions.SignIn` has the following properties:</span></span>

| <span data-ttu-id="81f44-143">Özellik</span><span class="sxs-lookup"><span data-stu-id="81f44-143">Property</span></span>                | <span data-ttu-id="81f44-144">Açıklama</span><span class="sxs-lookup"><span data-stu-id="81f44-144">Description</span></span>                       | <span data-ttu-id="81f44-145">Varsayılan</span><span class="sxs-lookup"><span data-stu-id="81f44-145">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `RequireConfirmedEmail` | <span data-ttu-id="81f44-146">Oturum açmak için bir onay e-posta gerektirir.</span><span class="sxs-lookup"><span data-stu-id="81f44-146">Requires a confirmed email to sign in.</span></span> | <span data-ttu-id="81f44-147">false</span><span class="sxs-lookup"><span data-stu-id="81f44-147">false</span></span>  |
| `RequireConfirmedPhoneNumber` |  <span data-ttu-id="81f44-148">Oturum açmak onaylanan telefon numarası gerektirir.</span><span class="sxs-lookup"><span data-stu-id="81f44-148">Requires a confirmed phone number to sign in.</span></span> | <span data-ttu-id="81f44-149">false</span><span class="sxs-lookup"><span data-stu-id="81f44-149">false</span></span>  |

## <a name="user-validation-settings"></a><span data-ttu-id="81f44-150">Kullanıcı doğrulama ayarları</span><span class="sxs-lookup"><span data-stu-id="81f44-150">User validation settings</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,48-52)]

<span data-ttu-id="81f44-151">`IdentityOptions.User`aşağıdaki özelliklere sahiptir:</span><span class="sxs-lookup"><span data-stu-id="81f44-151">`IdentityOptions.User` has the following properties:</span></span>

| <span data-ttu-id="81f44-152">Özellik</span><span class="sxs-lookup"><span data-stu-id="81f44-152">Property</span></span>                | <span data-ttu-id="81f44-153">Açıklama</span><span class="sxs-lookup"><span data-stu-id="81f44-153">Description</span></span>                       | <span data-ttu-id="81f44-154">Varsayılan</span><span class="sxs-lookup"><span data-stu-id="81f44-154">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `RequireUniqueEmail`  | <span data-ttu-id="81f44-155">Her kullanıcının benzersiz bir e-posta olmasını gerektirir.</span><span class="sxs-lookup"><span data-stu-id="81f44-155">Requires each User to have a unique email.</span></span> | <span data-ttu-id="81f44-156">false</span><span class="sxs-lookup"><span data-stu-id="81f44-156">false</span></span>  |
| `AllowedUserNameCharacters`  | <span data-ttu-id="81f44-157">Kullanıcı adı izin verilen karakter.</span><span class="sxs-lookup"><span data-stu-id="81f44-157">Allowed characters in the username.</span></span> | <span data-ttu-id="81f44-158">abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+</span><span class="sxs-lookup"><span data-stu-id="81f44-158">abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+</span></span> |


## <a name="applications-cookie-settings"></a><span data-ttu-id="81f44-159">Uygulamanın tanımlama bilgisi ayarları</span><span class="sxs-lookup"><span data-stu-id="81f44-159">Application's cookie settings</span></span>

<span data-ttu-id="81f44-160">Parola İlkesi gibi uygulama tanımlama bilgisinin tüm ayarları değiştirilebilir `Startup` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="81f44-160">Like the passwords policy, all the settings of the application's cookie can be changed in the `Startup` class.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="81f44-161">ASP.NET 2.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="81f44-161">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="81f44-162">Altında `ConfigureServices` içinde `Startup` sınıfı, uygulamanın tanımlama bilgisi yapılandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="81f44-162">Under `ConfigureServices` in the `Startup` class, you can configure the application's cookie.</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="81f44-163">ASP.NET 1.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="81f44-163">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

---

<span data-ttu-id="81f44-164">`CookieAuthenticationOptions`aşağıdaki özelliklere sahiptir:</span><span class="sxs-lookup"><span data-stu-id="81f44-164">`CookieAuthenticationOptions` has the following properties:</span></span>

| <span data-ttu-id="81f44-165">Özellik</span><span class="sxs-lookup"><span data-stu-id="81f44-165">Property</span></span>                | <span data-ttu-id="81f44-166">Açıklama</span><span class="sxs-lookup"><span data-stu-id="81f44-166">Description</span></span>                       | <span data-ttu-id="81f44-167">Varsayılan</span><span class="sxs-lookup"><span data-stu-id="81f44-167">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `Cookie.Name`  | <span data-ttu-id="81f44-168">Tanımlama bilgisinin adı.</span><span class="sxs-lookup"><span data-stu-id="81f44-168">The name of the cookie.</span></span>  | <span data-ttu-id="81f44-169">. AspNetCore.Cookies.</span><span class="sxs-lookup"><span data-stu-id="81f44-169">.AspNetCore.Cookies.</span></span>  |
| `Cookie.HttpOnly`  | <span data-ttu-id="81f44-170">Doğru olduğunda, tanımlama bilgisinin istemci tarafı komut dosyalarından erişilebilir değil.</span><span class="sxs-lookup"><span data-stu-id="81f44-170">When true, the cookie is not accessible from client-side scripts.</span></span>  |  <span data-ttu-id="81f44-171">true</span><span class="sxs-lookup"><span data-stu-id="81f44-171">true</span></span> |
| `ExpireTimeSpan`  | <span data-ttu-id="81f44-172">Kimlik doğrulaması bileti tanımlama bilgisinde saklanan ne kadar süre oluşturulduğu noktadan itibaren geçerli kalacağını denetler.</span><span class="sxs-lookup"><span data-stu-id="81f44-172">Controls how much time the authentication ticket stored in the cookie will remain valid from the point it is created.</span></span>  | <span data-ttu-id="81f44-173">14 gün</span><span class="sxs-lookup"><span data-stu-id="81f44-173">14 days</span></span>  |
| `LoginPath`  | <span data-ttu-id="81f44-174">Bir kullanıcı yetkisiz olduğunda, bu oturum açma yoluna yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="81f44-174">When a user is unauthorized, they will be redirected to this path to login.</span></span> | <span data-ttu-id="81f44-175">/ Hesap/oturum açma</span><span class="sxs-lookup"><span data-stu-id="81f44-175">/Account/Login</span></span>  |
| `LogoutPath`  | <span data-ttu-id="81f44-176">Bir kullanıcı oturum açıldığında, bu yolu yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="81f44-176">When a user is logged out, they will be redirected to this path.</span></span>  | <span data-ttu-id="81f44-177">/ Hesap/oturum kapatma</span><span class="sxs-lookup"><span data-stu-id="81f44-177">/Account/Logout</span></span>  |
| `AccessDeniedPath`  | <span data-ttu-id="81f44-178">Bir kullanıcı bir yetkilendirme denetim başarısız olduğunda, bu yolu yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="81f44-178">When a user fails an authorization check, they will be redirected to this path.</span></span>  |   |
| `SlidingExpiration`  | <span data-ttu-id="81f44-179">Doğru olduğunda, yeni bir tanımlama bilgisi zaman geçerli tanımlama bilgisi sona erme penceresinin yarısından fazlasını ilerlemiş yeni bir sona erme saati ile verilir.</span><span class="sxs-lookup"><span data-stu-id="81f44-179">When true, a new cookie will be issued with a new expiration time when the current cookie is more than halfway through the expiration window.</span></span>  | <span data-ttu-id="81f44-180">/ Hesap/erişim engellendi</span><span class="sxs-lookup"><span data-stu-id="81f44-180">/Account/AccessDenied</span></span> |
| `ReturnUrlParameter`  | <span data-ttu-id="81f44-181">Bir 401 yetkilendirilmedi durum kodu oturum açma yoluna 302 yeniden yönlendirme olarak değiştirildiğinde, hangi ara yazılım tarafından eklenen sorgu dizesi parametresinin adını belirler.</span><span class="sxs-lookup"><span data-stu-id="81f44-181">Determines the name of the query string parameter which is appended by the middleware when a 401 Unauthorized status code is changed to a 302 redirect onto the login path.</span></span>  |  <span data-ttu-id="81f44-182">true</span><span class="sxs-lookup"><span data-stu-id="81f44-182">true</span></span> |
| `AuthenticationScheme`  | <span data-ttu-id="81f44-183">Bu yalnızca ASP.NET Core için ilgili 1.x.</span><span class="sxs-lookup"><span data-stu-id="81f44-183">This is only relevant for ASP.NET Core 1.x.</span></span> <span data-ttu-id="81f44-184">Belirli kimlik doğrulaması düzeni için mantıksal adı.</span><span class="sxs-lookup"><span data-stu-id="81f44-184">The logical name for a particular authentication scheme.</span></span> |  |
| `AutomaticAuthenticate`  | <span data-ttu-id="81f44-185">Bu bayrak yalnızca ASP.NET Core için ilgili 1.x.</span><span class="sxs-lookup"><span data-stu-id="81f44-185">This flag is only relevant for ASP.NET Core 1.x.</span></span> <span data-ttu-id="81f44-186">Doğru olduğunda, tanımlama bilgisi kimlik doğrulamasını her istek çalıştırın ve oluşturulan herhangi bir seri hale getirilmiş asıl yeniden yapılandırma ve doğrulama girişimi.</span><span class="sxs-lookup"><span data-stu-id="81f44-186">When true, cookie authentication should run on every request and attempt to validate and reconstruct any serialized principal it created.</span></span>  |  |