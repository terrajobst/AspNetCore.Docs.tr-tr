---
title: "ASP.NET Core kimliğini yapılandırmak"
author: AdrienTorris
description: "ASP.NET Core kimlik varsayılan değerleri anlamak ve özel değerleri kullanmak için çeşitli kimlik özelliklerini yapılandırın."
manager: wpickett
ms.author: scaddie
ms.date: 01/11/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/identity-configuration
ms.openlocfilehash: cf7dcdb80f5edf9e10960cb08957793c36829a69
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2018
---
# <a name="configure-identity"></a><span data-ttu-id="6078b-103">Kimliği yapılandırmak</span><span class="sxs-lookup"><span data-stu-id="6078b-103">Configure Identity</span></span>

<span data-ttu-id="6078b-104">ASP.NET Core kimliği parola ilkesi, kilitleme süresini ve uygulamanızın kolayca kılabilirsiniz tanımlama bilgisi ayarları gibi uygulamalarda ortak davranışları sahip `Startup` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="6078b-104">ASP.NET Core Identity has common behaviors in applications such as password policy, lockout time, and cookie settings that you can override easily in your application's `Startup` class.</span></span>

## <a name="passwords-policy"></a><span data-ttu-id="6078b-105">Parola İlkesi</span><span class="sxs-lookup"><span data-stu-id="6078b-105">Passwords policy</span></span>

<span data-ttu-id="6078b-106">Varsayılan olarak, parolalar bir büyük harf, küçük harf, rakam ve alfasayısal olmayan karakter içerdiğini kimlik gerektirir.</span><span class="sxs-lookup"><span data-stu-id="6078b-106">By default, Identity requires that passwords contain an uppercase character, lowercase character, a digit, and a non-alphanumeric character.</span></span> <span data-ttu-id="6078b-107">Diğer bazı kısıtlamalar vardır.</span><span class="sxs-lookup"><span data-stu-id="6078b-107">There are also some other restrictions.</span></span> <span data-ttu-id="6078b-108">Parola kısıtlamaları basitleştirmek için değişiklik `ConfigureServices` yöntemi `Startup` uygulamanızın sınıfı.</span><span class="sxs-lookup"><span data-stu-id="6078b-108">To simplify password restrictions, modify the `ConfigureServices` method of the `Startup` class of your application.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6078b-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6078b-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="6078b-110">ASP.NET Core eklenen 2.0 `RequiredUniqueChars` özelliği.</span><span class="sxs-lookup"><span data-stu-id="6078b-110">ASP.NET Core 2.0 added the `RequiredUniqueChars` property.</span></span> <span data-ttu-id="6078b-111">Aksi takdirde, ASP.NET Core aynı seçeneklerdir 1.x.</span><span class="sxs-lookup"><span data-stu-id="6078b-111">Otherwise, the options are the same from ASP.NET Core 1.x.</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6078b-112">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6078b-112">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

---

<span data-ttu-id="6078b-113">`IdentityOptions.Password`aşağıdaki özelliklere sahiptir:</span><span class="sxs-lookup"><span data-stu-id="6078b-113">`IdentityOptions.Password` has the following properties:</span></span>

| <span data-ttu-id="6078b-114">Özellik</span><span class="sxs-lookup"><span data-stu-id="6078b-114">Property</span></span>                | <span data-ttu-id="6078b-115">Açıklama</span><span class="sxs-lookup"><span data-stu-id="6078b-115">Description</span></span>                       | <span data-ttu-id="6078b-116">Varsayılan</span><span class="sxs-lookup"><span data-stu-id="6078b-116">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `RequireDigit`          | <span data-ttu-id="6078b-117">Parola 0-9 arasında bir sayı gerektirir.</span><span class="sxs-lookup"><span data-stu-id="6078b-117">Requires a number between 0-9 in the password.</span></span> | <span data-ttu-id="6078b-118">true</span><span class="sxs-lookup"><span data-stu-id="6078b-118">true</span></span> |
| `RequiredLength`        | <span data-ttu-id="6078b-119">Minimum parola uzunluğu.</span><span class="sxs-lookup"><span data-stu-id="6078b-119">The minimum length of the password.</span></span> | <span data-ttu-id="6078b-120">6</span><span class="sxs-lookup"><span data-stu-id="6078b-120">6</span></span> |
| `RequireNonAlphanumeric`| <span data-ttu-id="6078b-121">Parolada alfasayısal olmayan karakter gerektirir.</span><span class="sxs-lookup"><span data-stu-id="6078b-121">Requires a non-alphanumeric character in the password.</span></span> | <span data-ttu-id="6078b-122">true</span><span class="sxs-lookup"><span data-stu-id="6078b-122">true</span></span> |
| `RequireUppercase`      | <span data-ttu-id="6078b-123">Bir büyük harf karakter parola gerektirir.</span><span class="sxs-lookup"><span data-stu-id="6078b-123">Requires an upper case character in the password.</span></span> | <span data-ttu-id="6078b-124">true</span><span class="sxs-lookup"><span data-stu-id="6078b-124">true</span></span> |
| `RequireLowercase`      | <span data-ttu-id="6078b-125">Parola küçük harf karakter gerektirir.</span><span class="sxs-lookup"><span data-stu-id="6078b-125">Requires a lower case character in the password.</span></span> | <span data-ttu-id="6078b-126">true</span><span class="sxs-lookup"><span data-stu-id="6078b-126">true</span></span> |
| `RequiredUniqueChars`   | <span data-ttu-id="6078b-127">Parola ayrı karakter sayısını gerektirir.</span><span class="sxs-lookup"><span data-stu-id="6078b-127">Requires the number of distinct characters in the password.</span></span> | <span data-ttu-id="6078b-128">1.</span><span class="sxs-lookup"><span data-stu-id="6078b-128">1</span></span> |


## <a name="users-lockout"></a><span data-ttu-id="6078b-129">Kullanıcının kilitleme</span><span class="sxs-lookup"><span data-stu-id="6078b-129">User's lockout</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,39-42,50-52)]

<span data-ttu-id="6078b-130">`IdentityOptions.Lockout`aşağıdaki özelliklere sahiptir:</span><span class="sxs-lookup"><span data-stu-id="6078b-130">`IdentityOptions.Lockout` has the following properties:</span></span>

| <span data-ttu-id="6078b-131">Özellik</span><span class="sxs-lookup"><span data-stu-id="6078b-131">Property</span></span>                | <span data-ttu-id="6078b-132">Açıklama</span><span class="sxs-lookup"><span data-stu-id="6078b-132">Description</span></span>                       | <span data-ttu-id="6078b-133">Varsayılan</span><span class="sxs-lookup"><span data-stu-id="6078b-133">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `DefaultLockoutTimeSpan` | <span data-ttu-id="6078b-134">Bir kilitleme oluştuğunda süreyi bir kullanıcının kilitlendiği.</span><span class="sxs-lookup"><span data-stu-id="6078b-134">The amount of time a user is locked out when a lockout occurs.</span></span>  | <span data-ttu-id="6078b-135">5 dakika</span><span class="sxs-lookup"><span data-stu-id="6078b-135">5 minutes</span></span>  |
| `MaxFailedAccessAttempts` | <span data-ttu-id="6078b-136">Kilitleme etkinse, bir kullanıcı kilitli kadar başarısız erişim denemesi sayısı.</span><span class="sxs-lookup"><span data-stu-id="6078b-136">The number of failed access attempts until a user is locked out, if lockout is enabled.</span></span>  | <span data-ttu-id="6078b-137">5</span><span class="sxs-lookup"><span data-stu-id="6078b-137">5</span></span> |
| `AllowedForNewUsers` | <span data-ttu-id="6078b-138">Yeni bir kullanıcı kilitlenip kilitlenmeyeceği olmadığını belirler.</span><span class="sxs-lookup"><span data-stu-id="6078b-138">Determines if a new user can be locked out.</span></span>  | <span data-ttu-id="6078b-139">true</span><span class="sxs-lookup"><span data-stu-id="6078b-139">true</span></span> |

## <a name="sign-in-settings"></a><span data-ttu-id="6078b-140">Ayarları'nda oturum açın</span><span class="sxs-lookup"><span data-stu-id="6078b-140">Sign in settings</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)]

<span data-ttu-id="6078b-141">`IdentityOptions.SignIn`aşağıdaki özelliklere sahiptir:</span><span class="sxs-lookup"><span data-stu-id="6078b-141">`IdentityOptions.SignIn` has the following properties:</span></span>

| <span data-ttu-id="6078b-142">Özellik</span><span class="sxs-lookup"><span data-stu-id="6078b-142">Property</span></span>                | <span data-ttu-id="6078b-143">Açıklama</span><span class="sxs-lookup"><span data-stu-id="6078b-143">Description</span></span>                       | <span data-ttu-id="6078b-144">Varsayılan</span><span class="sxs-lookup"><span data-stu-id="6078b-144">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `RequireConfirmedEmail` | <span data-ttu-id="6078b-145">Oturum açmak için bir onay e-posta gerektirir.</span><span class="sxs-lookup"><span data-stu-id="6078b-145">Requires a confirmed email to sign in.</span></span> | <span data-ttu-id="6078b-146">false</span><span class="sxs-lookup"><span data-stu-id="6078b-146">false</span></span>  |
| `RequireConfirmedPhoneNumber` |  <span data-ttu-id="6078b-147">Oturum açmak onaylanan telefon numarası gerektirir.</span><span class="sxs-lookup"><span data-stu-id="6078b-147">Requires a confirmed phone number to sign in.</span></span> | <span data-ttu-id="6078b-148">false</span><span class="sxs-lookup"><span data-stu-id="6078b-148">false</span></span>  |

## <a name="user-validation-settings"></a><span data-ttu-id="6078b-149">Kullanıcı doğrulama ayarları</span><span class="sxs-lookup"><span data-stu-id="6078b-149">User validation settings</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,48-52)]

<span data-ttu-id="6078b-150">`IdentityOptions.User`aşağıdaki özelliklere sahiptir:</span><span class="sxs-lookup"><span data-stu-id="6078b-150">`IdentityOptions.User` has the following properties:</span></span>

| <span data-ttu-id="6078b-151">Özellik</span><span class="sxs-lookup"><span data-stu-id="6078b-151">Property</span></span>                | <span data-ttu-id="6078b-152">Açıklama</span><span class="sxs-lookup"><span data-stu-id="6078b-152">Description</span></span>                       | <span data-ttu-id="6078b-153">Varsayılan</span><span class="sxs-lookup"><span data-stu-id="6078b-153">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `RequireUniqueEmail`  | <span data-ttu-id="6078b-154">Her kullanıcının benzersiz bir e-posta olmasını gerektirir.</span><span class="sxs-lookup"><span data-stu-id="6078b-154">Requires each User to have a unique email.</span></span> | <span data-ttu-id="6078b-155">false</span><span class="sxs-lookup"><span data-stu-id="6078b-155">false</span></span>  |
| `AllowedUserNameCharacters`  | <span data-ttu-id="6078b-156">Kullanıcı adı izin verilen karakter.</span><span class="sxs-lookup"><span data-stu-id="6078b-156">Allowed characters in the username.</span></span> | <span data-ttu-id="6078b-157">abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+</span><span class="sxs-lookup"><span data-stu-id="6078b-157">abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+</span></span> |


## <a name="applications-cookie-settings"></a><span data-ttu-id="6078b-158">Uygulamanın tanımlama bilgisi ayarları</span><span class="sxs-lookup"><span data-stu-id="6078b-158">Application's cookie settings</span></span>

<span data-ttu-id="6078b-159">Parola İlkesi gibi uygulama tanımlama bilgisinin tüm ayarları değiştirilebilir `Startup` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="6078b-159">Like the passwords policy, all the settings of the application's cookie can be changed in the `Startup` class.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6078b-160">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6078b-160">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="6078b-161">Altında `ConfigureServices` içinde `Startup` sınıfı, uygulamanın tanımlama bilgisi yapılandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6078b-161">Under `ConfigureServices` in the `Startup` class, you can configure the application's cookie.</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6078b-162">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6078b-162">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

---

<span data-ttu-id="6078b-163">`CookieAuthenticationOptions`aşağıdaki özelliklere sahiptir:</span><span class="sxs-lookup"><span data-stu-id="6078b-163">`CookieAuthenticationOptions` has the following properties:</span></span>

| <span data-ttu-id="6078b-164">Özellik</span><span class="sxs-lookup"><span data-stu-id="6078b-164">Property</span></span>                | <span data-ttu-id="6078b-165">Açıklama</span><span class="sxs-lookup"><span data-stu-id="6078b-165">Description</span></span>                       | <span data-ttu-id="6078b-166">Varsayılan</span><span class="sxs-lookup"><span data-stu-id="6078b-166">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `Cookie.Name`  | <span data-ttu-id="6078b-167">Tanımlama bilgisinin adı.</span><span class="sxs-lookup"><span data-stu-id="6078b-167">The name of the cookie.</span></span>  | <span data-ttu-id="6078b-168">.AspNetCore.Cookies.</span><span class="sxs-lookup"><span data-stu-id="6078b-168">.AspNetCore.Cookies.</span></span>  |
| `Cookie.HttpOnly`  | <span data-ttu-id="6078b-169">Doğru olduğunda, tanımlama bilgisinin istemci tarafı komut dosyalarından erişilebilir değil.</span><span class="sxs-lookup"><span data-stu-id="6078b-169">When true, the cookie isn't accessible from client-side scripts.</span></span>  |  <span data-ttu-id="6078b-170">true</span><span class="sxs-lookup"><span data-stu-id="6078b-170">true</span></span> |
| `ExpireTimeSpan`  | <span data-ttu-id="6078b-171">Kimlik doğrulaması bileti tanımlama bilgisinde saklanan ne kadar süre oluşturulduğu noktadan itibaren geçerli kalacağını denetler.</span><span class="sxs-lookup"><span data-stu-id="6078b-171">Controls how much time the authentication ticket stored in the cookie will remain valid from the point it's created.</span></span>  | <span data-ttu-id="6078b-172">14 gün</span><span class="sxs-lookup"><span data-stu-id="6078b-172">14 days</span></span>  |
| `LoginPath`  | <span data-ttu-id="6078b-173">Bir kullanıcı yetkisiz olduğunda, bu oturum açma yoluna yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="6078b-173">When a user is unauthorized, they will be redirected to this path to login.</span></span> | <span data-ttu-id="6078b-174">/ Hesap/oturum açma</span><span class="sxs-lookup"><span data-stu-id="6078b-174">/Account/Login</span></span>  |
| `LogoutPath`  | <span data-ttu-id="6078b-175">Bir kullanıcı oturum açıldığında, bu yolu yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="6078b-175">When a user is logged out, they will be redirected to this path.</span></span>  | <span data-ttu-id="6078b-176">/ Hesap/oturum kapatma</span><span class="sxs-lookup"><span data-stu-id="6078b-176">/Account/Logout</span></span>  |
| `AccessDeniedPath`  | <span data-ttu-id="6078b-177">Bir kullanıcı bir yetkilendirme denetim başarısız olduğunda, bu yolu yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="6078b-177">When a user fails an authorization check, they will be redirected to this path.</span></span>  |   |
| `SlidingExpiration`  | <span data-ttu-id="6078b-178">Doğru olduğunda, yeni bir tanımlama bilgisi zaman geçerli tanımlama bilgisi sona erme penceresinin yarısından fazlasını ilerlemiş yeni bir sona erme saati ile verilir.</span><span class="sxs-lookup"><span data-stu-id="6078b-178">When true, a new cookie will be issued with a new expiration time when the current cookie is more than halfway through the expiration window.</span></span>  | <span data-ttu-id="6078b-179">/Account/AccessDenied</span><span class="sxs-lookup"><span data-stu-id="6078b-179">/Account/AccessDenied</span></span> |
| `ReturnUrlParameter`  | <span data-ttu-id="6078b-180">Bir 401 yetkilendirilmedi durum kodu oturum açma yoluna 302 yeniden yönlendirme olarak değiştirildiğinde, hangi ara yazılım tarafından eklenen sorgu dizesi parametresinin adını belirler.</span><span class="sxs-lookup"><span data-stu-id="6078b-180">Determines the name of the query string parameter which is appended by the middleware when a 401 Unauthorized status code is changed to a 302 redirect onto the login path.</span></span>  |  <span data-ttu-id="6078b-181">true</span><span class="sxs-lookup"><span data-stu-id="6078b-181">true</span></span> |
| `AuthenticationScheme`  | <span data-ttu-id="6078b-182">Bu yalnızca ASP.NET Core için ilgili 1.x.</span><span class="sxs-lookup"><span data-stu-id="6078b-182">This is only relevant for ASP.NET Core 1.x.</span></span> <span data-ttu-id="6078b-183">Belirli kimlik doğrulaması düzeni için mantıksal adı.</span><span class="sxs-lookup"><span data-stu-id="6078b-183">The logical name for a particular authentication scheme.</span></span> |  |
| `AutomaticAuthenticate`  | <span data-ttu-id="6078b-184">Bu bayrak yalnızca ASP.NET Core için ilgili 1.x.</span><span class="sxs-lookup"><span data-stu-id="6078b-184">This flag is only relevant for ASP.NET Core 1.x.</span></span> <span data-ttu-id="6078b-185">Doğru olduğunda, tanımlama bilgisi kimlik doğrulamasını her istek çalıştırın ve oluşturulan herhangi bir seri hale getirilmiş asıl yeniden yapılandırma ve doğrulama girişimi.</span><span class="sxs-lookup"><span data-stu-id="6078b-185">When true, cookie authentication should run on every request and attempt to validate and reconstruct any serialized principal it created.</span></span>  |  |
