---
title: ASP.NET Core kimliğini yapılandırma
author: AdrienTorris
description: ASP.NET Core kimlik varsayılan değerlerini anlayın ve kimlik özelliklerini özel değerleri kullanacak şekilde yapılandırmayı öğrenin.
ms.author: riande
ms.date: 02/11/2019
uid: security/authentication/identity-configuration
ms.openlocfilehash: 823182bed2cb953e07f9374d135868aeb2be9c60
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78661408"
---
# <a name="configure-aspnet-core-identity"></a><span data-ttu-id="261a4-103">ASP.NET Core kimliğini yapılandırma</span><span class="sxs-lookup"><span data-stu-id="261a4-103">Configure ASP.NET Core Identity</span></span>

<span data-ttu-id="261a4-104">ASP.NET Core kimlik, parola ilkesi, kilitleme ve tanımlama bilgisi yapılandırması gibi ayarların varsayılan değerlerini kullanır.</span><span class="sxs-lookup"><span data-stu-id="261a4-104">ASP.NET Core Identity uses default values for settings such as password policy, lockout, and cookie configuration.</span></span> <span data-ttu-id="261a4-105">Bu ayarlar `Startup` sınıfında geçersiz kılınabilir.</span><span class="sxs-lookup"><span data-stu-id="261a4-105">These settings can be overridden in the `Startup` class.</span></span>

## <a name="identity-options"></a><span data-ttu-id="261a4-106">Kimlik seçenekleri</span><span class="sxs-lookup"><span data-stu-id="261a4-106">Identity options</span></span>

<span data-ttu-id="261a4-107">[Identityoptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) sınıfı, kimlik sistemini yapılandırmak için kullanılabilen seçenekleri temsil eder.</span><span class="sxs-lookup"><span data-stu-id="261a4-107">The [IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) class represents the options that can be used to configure the Identity system.</span></span> <span data-ttu-id="261a4-108">`IdentityOptions`, `AddIdentity` veya `AddDefaultIdentity`çağrıldıktan **sonra** ayarlanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="261a4-108">`IdentityOptions` must be set **after** calling `AddIdentity` or `AddDefaultIdentity`.</span></span>

### <a name="claims-identity"></a><span data-ttu-id="261a4-109">Talep kimliği</span><span class="sxs-lookup"><span data-stu-id="261a4-109">Claims Identity</span></span>

<span data-ttu-id="261a4-110">[Identityoptions. ClaimsIdentity](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.claimsidentity) , aşağıdaki tabloda gösterilen özelliklerle [Claimsıdentityoptions](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions) 'ı belirtir.</span><span class="sxs-lookup"><span data-stu-id="261a4-110">[IdentityOptions.ClaimsIdentity](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.claimsidentity) specifies the [ClaimsIdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions) with the properties shown in the following table.</span></span>

| <span data-ttu-id="261a4-111">Özellik</span><span class="sxs-lookup"><span data-stu-id="261a4-111">Property</span></span> | <span data-ttu-id="261a4-112">Açıklama</span><span class="sxs-lookup"><span data-stu-id="261a4-112">Description</span></span> | <span data-ttu-id="261a4-113">Varsayılan</span><span class="sxs-lookup"><span data-stu-id="261a4-113">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="261a4-114">RoleClaimType</span><span class="sxs-lookup"><span data-stu-id="261a4-114">RoleClaimType</span></span>](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.roleclaimtype) | <span data-ttu-id="261a4-115">Rol talebi için kullanılan talep türünü alır veya ayarlar.</span><span class="sxs-lookup"><span data-stu-id="261a4-115">Gets or sets the claim type used for a role claim.</span></span> | [<span data-ttu-id="261a4-116">ClaimTypes. Role</span><span class="sxs-lookup"><span data-stu-id="261a4-116">ClaimTypes.Role</span></span>](/dotnet/api/system.security.claims.claimtypes.role) |
| [<span data-ttu-id="261a4-117">SecurityStampClaimType</span><span class="sxs-lookup"><span data-stu-id="261a4-117">SecurityStampClaimType</span></span>](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.securitystampclaimtype) | <span data-ttu-id="261a4-118">Güvenlik damga talebi için kullanılan talep türünü alır veya ayarlar.</span><span class="sxs-lookup"><span data-stu-id="261a4-118">Gets or sets the claim type used for the security stamp claim.</span></span> | `AspNet.Identity.SecurityStamp` |
| [<span data-ttu-id="261a4-119">Userıdclaimtype</span><span class="sxs-lookup"><span data-stu-id="261a4-119">UserIdClaimType</span></span>](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.useridclaimtype) | <span data-ttu-id="261a4-120">Kullanıcı tanımlayıcısı talebi için kullanılan talep türünü alır veya ayarlar.</span><span class="sxs-lookup"><span data-stu-id="261a4-120">Gets or sets the claim type used for the user identifier claim.</span></span> | [<span data-ttu-id="261a4-121">ClaimTypes. NameIdentifier</span><span class="sxs-lookup"><span data-stu-id="261a4-121">ClaimTypes.NameIdentifier</span></span>](/dotnet/api/system.security.claims.claimtypes.nameidentifier) |
| [<span data-ttu-id="261a4-122">UserNameClaimType</span><span class="sxs-lookup"><span data-stu-id="261a4-122">UserNameClaimType</span></span>](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.usernameclaimtype) | <span data-ttu-id="261a4-123">Kullanıcı adı talebi için kullanılan talep türünü alır veya ayarlar.</span><span class="sxs-lookup"><span data-stu-id="261a4-123">Gets or sets the claim type used for the user name claim.</span></span> | [<span data-ttu-id="261a4-124">ClaimTypes.Name</span><span class="sxs-lookup"><span data-stu-id="261a4-124">ClaimTypes.Name</span></span>](/dotnet/api/system.security.claims.claimtypes.name) |

### <a name="lockout"></a><span data-ttu-id="261a4-125">Kilitleme</span><span class="sxs-lookup"><span data-stu-id="261a4-125">Lockout</span></span>

<span data-ttu-id="261a4-126">[Passwordsignınasync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync#Microsoft_AspNetCore_Identity_SignInManager_1_PasswordSignInAsync_System_String_System_String_System_Boolean_System_Boolean_) yönteminde kilitleme ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="261a4-126">Lockout is set in the [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync#Microsoft_AspNetCore_Identity_SignInManager_1_PasswordSignInAsync_System_String_System_String_System_Boolean_System_Boolean_) method:</span></span>

[!code-csharp[](identity-configuration/sample/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=9)]

<span data-ttu-id="261a4-127">Yukarıdaki kod `Login` Identity şablonunu temel alır.</span><span class="sxs-lookup"><span data-stu-id="261a4-127">The preceding code is based on the `Login` Identity template.</span></span> 

<span data-ttu-id="261a4-128">Kilitleme seçenekleri `StartUp.ConfigureServices`olarak ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="261a4-128">Lockout options are set in `StartUp.ConfigureServices`:</span></span>

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_lock)]

<span data-ttu-id="261a4-129">Önceki kod, [ıdentityoptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) [lockoutoptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) değerlerini varsayılan değerlerle ayarlar.</span><span class="sxs-lookup"><span data-stu-id="261a4-129">The preceding code sets the [IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) with default values.</span></span>

<span data-ttu-id="261a4-130">Başarılı bir kimlik doğrulaması başarısız erişim denemesi sayısını sıfırlar ve saatini sıfırlar.</span><span class="sxs-lookup"><span data-stu-id="261a4-130">A successful authentication resets the failed access attempts count and resets the clock.</span></span>

<span data-ttu-id="261a4-131">[Identityoptions. kilitleme](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.lockout) , tabloda gösterilen özelliklerle [Lockoutoptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) değerini belirtir.</span><span class="sxs-lookup"><span data-stu-id="261a4-131">[IdentityOptions.Lockout](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.lockout) specifies the [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) with the properties shown in the table.</span></span>

| <span data-ttu-id="261a4-132">Özellik</span><span class="sxs-lookup"><span data-stu-id="261a4-132">Property</span></span> | <span data-ttu-id="261a4-133">Açıklama</span><span class="sxs-lookup"><span data-stu-id="261a4-133">Description</span></span> | <span data-ttu-id="261a4-134">Varsayılan</span><span class="sxs-lookup"><span data-stu-id="261a4-134">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="261a4-135">AllowedForNewUsers</span><span class="sxs-lookup"><span data-stu-id="261a4-135">AllowedForNewUsers</span></span>](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.allowedfornewusers) | <span data-ttu-id="261a4-136">Yeni bir kullanıcının kilitlenip kilitlenmeyeceğini belirler.</span><span class="sxs-lookup"><span data-stu-id="261a4-136">Determines if a new user can be locked out.</span></span> | `true` |
| [<span data-ttu-id="261a4-137">DefaultLockoutTimeSpan</span><span class="sxs-lookup"><span data-stu-id="261a4-137">DefaultLockoutTimeSpan</span></span>](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan) | <span data-ttu-id="261a4-138">Bir kilitleme gerçekleştiğinde kullanıcının kilitlediği zaman miktarı.</span><span class="sxs-lookup"><span data-stu-id="261a4-138">The amount of time a user is locked out when a lockout occurs.</span></span> | <span data-ttu-id="261a4-139">5 dakika</span><span class="sxs-lookup"><span data-stu-id="261a4-139">5 minutes</span></span> |
| [<span data-ttu-id="261a4-140">MaxFailedAccessAttempts</span><span class="sxs-lookup"><span data-stu-id="261a4-140">MaxFailedAccessAttempts</span></span>](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) | <span data-ttu-id="261a4-141">Kilitleme etkinse, bir Kullanıcı kilitlenene kadar başarısız olan erişim girişimlerinin sayısı.</span><span class="sxs-lookup"><span data-stu-id="261a4-141">The number of failed access attempts until a user is locked out, if lockout is enabled.</span></span> | <span data-ttu-id="261a4-142">5</span><span class="sxs-lookup"><span data-stu-id="261a4-142">5</span></span> |

### <a name="password"></a><span data-ttu-id="261a4-143">Parola</span><span class="sxs-lookup"><span data-stu-id="261a4-143">Password</span></span>

<span data-ttu-id="261a4-144">Varsayılan olarak, kimlik, parolaların büyük harf, küçük harf, rakam ve alfasayısal olmayan bir karakter içermesini gerektirir.</span><span class="sxs-lookup"><span data-stu-id="261a4-144">By default, Identity requires that passwords contain an uppercase character, lowercase character, a digit, and a non-alphanumeric character.</span></span> <span data-ttu-id="261a4-145">Parolalar en az altı karakter uzunluğunda olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="261a4-145">Passwords must be at least six characters long.</span></span> <span data-ttu-id="261a4-146">[Passwordoseçenekleri](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) `Startup.ConfigureServices`içinde ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="261a4-146">[PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) can be set in `Startup.ConfigureServices`.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_pw)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

::: moniker-end

<span data-ttu-id="261a4-147">[Identityoptions. Password](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.password) , tabloda gösterilen özelliklerle birlikte [passwordotions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) belirler.</span><span class="sxs-lookup"><span data-stu-id="261a4-147">[IdentityOptions.Password](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.password) specifies the [PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) with the properties shown in the table.</span></span>

::: moniker range=">= aspnetcore-2.0"

| <span data-ttu-id="261a4-148">Özellik</span><span class="sxs-lookup"><span data-stu-id="261a4-148">Property</span></span> | <span data-ttu-id="261a4-149">Açıklama</span><span class="sxs-lookup"><span data-stu-id="261a4-149">Description</span></span> | <span data-ttu-id="261a4-150">Varsayılan</span><span class="sxs-lookup"><span data-stu-id="261a4-150">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="261a4-151">RequireDigit</span><span class="sxs-lookup"><span data-stu-id="261a4-151">RequireDigit</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredigit) | <span data-ttu-id="261a4-152">Parolada 0-9 arasında bir sayı gerekir.</span><span class="sxs-lookup"><span data-stu-id="261a4-152">Requires a number between 0-9 in the password.</span></span> | `true` |
| [<span data-ttu-id="261a4-153">RequiredLength</span><span class="sxs-lookup"><span data-stu-id="261a4-153">RequiredLength</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredlength) | <span data-ttu-id="261a4-154">Parola uzunluğu alt sınırı.</span><span class="sxs-lookup"><span data-stu-id="261a4-154">The minimum length of the password.</span></span> | <span data-ttu-id="261a4-155">6</span><span class="sxs-lookup"><span data-stu-id="261a4-155">6</span></span> |
| [<span data-ttu-id="261a4-156">Requireküçük harf</span><span class="sxs-lookup"><span data-stu-id="261a4-156">RequireLowercase</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirelowercase) | <span data-ttu-id="261a4-157">Parolada küçük harfli bir karakter gerektirir.</span><span class="sxs-lookup"><span data-stu-id="261a4-157">Requires a lowercase character in the password.</span></span> | `true` |
| [<span data-ttu-id="261a4-158">Talep ırenonalfasayısal</span><span class="sxs-lookup"><span data-stu-id="261a4-158">RequireNonAlphanumeric</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirenonalphanumeric) | <span data-ttu-id="261a4-159">Parolada alfasayısal olmayan bir karakter gerektirir.</span><span class="sxs-lookup"><span data-stu-id="261a4-159">Requires a non-alphanumeric character in the password.</span></span> | `true` |
| [<span data-ttu-id="261a4-160">RequiredUniqueChars</span><span class="sxs-lookup"><span data-stu-id="261a4-160">RequiredUniqueChars</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireduniquechars) | <span data-ttu-id="261a4-161">Yalnızca ASP.NET Core 2,0 veya üzeri için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="261a4-161">Only applies to ASP.NET Core 2.0 or later.</span></span><br><br> <span data-ttu-id="261a4-162">Paroladaki ayrı karakter sayısını gerektirir.</span><span class="sxs-lookup"><span data-stu-id="261a4-162">Requires the number of distinct characters in the password.</span></span> | <span data-ttu-id="261a4-163">1\.</span><span class="sxs-lookup"><span data-stu-id="261a4-163">1</span></span> |
| [<span data-ttu-id="261a4-164">Requirebüyük</span><span class="sxs-lookup"><span data-stu-id="261a4-164">RequireUppercase</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireuppercase) | <span data-ttu-id="261a4-165">Parolada büyük harfli bir karakter gerektirir.</span><span class="sxs-lookup"><span data-stu-id="261a4-165">Requires an uppercase character in the password.</span></span> | `true` |

::: moniker-end

::: moniker range="< aspnetcore-2.0"

| <span data-ttu-id="261a4-166">Özellik</span><span class="sxs-lookup"><span data-stu-id="261a4-166">Property</span></span> | <span data-ttu-id="261a4-167">Açıklama</span><span class="sxs-lookup"><span data-stu-id="261a4-167">Description</span></span> | <span data-ttu-id="261a4-168">Varsayılan</span><span class="sxs-lookup"><span data-stu-id="261a4-168">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="261a4-169">RequireDigit</span><span class="sxs-lookup"><span data-stu-id="261a4-169">RequireDigit</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredigit) | <span data-ttu-id="261a4-170">Parolada 0-9 arasında bir sayı gerekir.</span><span class="sxs-lookup"><span data-stu-id="261a4-170">Requires a number between 0-9 in the password.</span></span> | `true` |
| [<span data-ttu-id="261a4-171">RequiredLength</span><span class="sxs-lookup"><span data-stu-id="261a4-171">RequiredLength</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredlength) | <span data-ttu-id="261a4-172">Parola uzunluğu alt sınırı.</span><span class="sxs-lookup"><span data-stu-id="261a4-172">The minimum length of the password.</span></span> | <span data-ttu-id="261a4-173">6</span><span class="sxs-lookup"><span data-stu-id="261a4-173">6</span></span> |
| [<span data-ttu-id="261a4-174">Requireküçük harf</span><span class="sxs-lookup"><span data-stu-id="261a4-174">RequireLowercase</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirelowercase) | <span data-ttu-id="261a4-175">Parolada küçük harfli bir karakter gerektirir.</span><span class="sxs-lookup"><span data-stu-id="261a4-175">Requires a lowercase character in the password.</span></span> | `true` |
| [<span data-ttu-id="261a4-176">Talep ırenonalfasayısal</span><span class="sxs-lookup"><span data-stu-id="261a4-176">RequireNonAlphanumeric</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirenonalphanumeric) | <span data-ttu-id="261a4-177">Parolada alfasayısal olmayan bir karakter gerektirir.</span><span class="sxs-lookup"><span data-stu-id="261a4-177">Requires a non-alphanumeric character in the password.</span></span> | `true` |
| [<span data-ttu-id="261a4-178">Requirebüyük</span><span class="sxs-lookup"><span data-stu-id="261a4-178">RequireUppercase</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireuppercase) | <span data-ttu-id="261a4-179">Parolada büyük harfli bir karakter gerektirir.</span><span class="sxs-lookup"><span data-stu-id="261a4-179">Requires an uppercase character in the password.</span></span> | `true` |

::: moniker-end

### <a name="sign-in"></a><span data-ttu-id="261a4-180">Oturum açma</span><span class="sxs-lookup"><span data-stu-id="261a4-180">Sign-in</span></span>

<span data-ttu-id="261a4-181">Aşağıdaki kod `SignIn` ayarlarını ayarlar (varsayılan değerlere):</span><span class="sxs-lookup"><span data-stu-id="261a4-181">The following code sets `SignIn` settings (to default values):</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_si)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)] 

::: moniker-end

<span data-ttu-id="261a4-182">[Identityoptions. signıd](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.signin) , tabloda gösterilen özelliklerle [SignInOptions](/dotnet/api/microsoft.aspnetcore.identity.signinoptions) 'ı belirtir.</span><span class="sxs-lookup"><span data-stu-id="261a4-182">[IdentityOptions.SignIn](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.signin) specifies the [SignInOptions](/dotnet/api/microsoft.aspnetcore.identity.signinoptions) with the properties shown in the table.</span></span>

| <span data-ttu-id="261a4-183">Özellik</span><span class="sxs-lookup"><span data-stu-id="261a4-183">Property</span></span> | <span data-ttu-id="261a4-184">Açıklama</span><span class="sxs-lookup"><span data-stu-id="261a4-184">Description</span></span> | <span data-ttu-id="261a4-185">Varsayılan</span><span class="sxs-lookup"><span data-stu-id="261a4-185">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="261a4-186">RequireConfirmedEmail</span><span class="sxs-lookup"><span data-stu-id="261a4-186">RequireConfirmedEmail</span></span>](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedemail) | <span data-ttu-id="261a4-187">Oturum açmak için onaylanan bir e-posta gerekir.</span><span class="sxs-lookup"><span data-stu-id="261a4-187">Requires a confirmed email to sign in.</span></span> | `false` |
| [<span data-ttu-id="261a4-188">RequireConfirmedPhoneNumber</span><span class="sxs-lookup"><span data-stu-id="261a4-188">RequireConfirmedPhoneNumber</span></span>](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedphonenumber) | <span data-ttu-id="261a4-189">Oturum açmak için onaylanmış bir telefon numarası gerektirir.</span><span class="sxs-lookup"><span data-stu-id="261a4-189">Requires a confirmed phone number to sign in.</span></span> | `false` |

### <a name="tokens"></a><span data-ttu-id="261a4-190">Belirteçler</span><span class="sxs-lookup"><span data-stu-id="261a4-190">Tokens</span></span>

<span data-ttu-id="261a4-191">[Identityoptions. Tokens](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.tokens) , tabloda gösterilen özelliklerle birlikte [tokenoptions](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions) belirler.</span><span class="sxs-lookup"><span data-stu-id="261a4-191">[IdentityOptions.Tokens](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.tokens) specifies the [TokenOptions](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions) with the properties shown in the table.</span></span>

|                                                        <span data-ttu-id="261a4-192">Özellik</span><span class="sxs-lookup"><span data-stu-id="261a4-192">Property</span></span>                                                         |                                                                                      <span data-ttu-id="261a4-193">Açıklama</span><span class="sxs-lookup"><span data-stu-id="261a4-193">Description</span></span>                                                                                      |
|-------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     [<span data-ttu-id="261a4-194">Kimlik doğrulayan Tortokenprovider</span><span class="sxs-lookup"><span data-stu-id="261a4-194">AuthenticatorTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.authenticatortokenprovider)     |                                       <span data-ttu-id="261a4-195">İki öğeli oturum açma işlemlerini kimlik doğrulayıcı ile doğrulamak için kullanılan `AuthenticatorTokenProvider` alır veya ayarlar.</span><span class="sxs-lookup"><span data-stu-id="261a4-195">Gets or sets the `AuthenticatorTokenProvider` used to validate two-factor sign-ins with an authenticator.</span></span>                                       |
|       [<span data-ttu-id="261a4-196">ChangeEmailTokenProvider</span><span class="sxs-lookup"><span data-stu-id="261a4-196">ChangeEmailTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changeemailtokenprovider)       |                                     <span data-ttu-id="261a4-197">E-posta değişikliği onay e-postalarında kullanılan belirteçleri oluşturmak için kullanılan `ChangeEmailTokenProvider` alır veya ayarlar.</span><span class="sxs-lookup"><span data-stu-id="261a4-197">Gets or sets the `ChangeEmailTokenProvider` used to generate tokens used in email change confirmation emails.</span></span>                                     |
| [<span data-ttu-id="261a4-198">ChangePhoneNumberTokenProvider</span><span class="sxs-lookup"><span data-stu-id="261a4-198">ChangePhoneNumberTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changephonenumbertokenprovider) |                                      <span data-ttu-id="261a4-199">Telefon numaralarını değiştirirken kullanılan belirteçleri oluşturmak için kullanılan `ChangePhoneNumberTokenProvider` alır veya ayarlar.</span><span class="sxs-lookup"><span data-stu-id="261a4-199">Gets or sets the `ChangePhoneNumberTokenProvider` used to generate tokens used when changing phone numbers.</span></span>                                      |
| [<span data-ttu-id="261a4-200">EmailConfirmationTokenProvider</span><span class="sxs-lookup"><span data-stu-id="261a4-200">EmailConfirmationTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.emailconfirmationtokenprovider) |                                             <span data-ttu-id="261a4-201">Hesap onaylama e-postalarında kullanılan belirteçleri oluşturmak için kullanılan belirteç sağlayıcısını alır veya ayarlar.</span><span class="sxs-lookup"><span data-stu-id="261a4-201">Gets or sets the token provider used to generate tokens used in account confirmation emails.</span></span>                                              |
|     [<span data-ttu-id="261a4-202">PasswordResetTokenProvider</span><span class="sxs-lookup"><span data-stu-id="261a4-202">PasswordResetTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.passwordresettokenprovider)     | <span data-ttu-id="261a4-203">Parola sıfırlama e-postalarında kullanılan belirteçleri oluşturmak için kullanılan [ıusertwofactortokenprovider\<TUser >](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactortokenprovider-1) alır veya ayarlar.</span><span class="sxs-lookup"><span data-stu-id="261a4-203">Gets or sets the [IUserTwoFactorTokenProvider\<TUser>](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactortokenprovider-1) used to generate tokens used in password reset emails.</span></span> |
|                    [<span data-ttu-id="261a4-204">ProviderMap</span><span class="sxs-lookup"><span data-stu-id="261a4-204">ProviderMap</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.providermap)                    |                <span data-ttu-id="261a4-205">Sağlayıcının adı olarak kullanılan anahtarla bir [Kullanıcı belirteci sağlayıcısı](/dotnet/api/microsoft.aspnetcore.identity.tokenproviderdescriptor) oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="261a4-205">Used to construct a [User Token Provider](/dotnet/api/microsoft.aspnetcore.identity.tokenproviderdescriptor) with the key used as the provider's name.</span></span>                 |

### <a name="user"></a><span data-ttu-id="261a4-206">Kullanıcı</span><span class="sxs-lookup"><span data-stu-id="261a4-206">User</span></span>

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_user)]

<span data-ttu-id="261a4-207">[Identityoptions. User](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.user) , tabloda gösterilen özelliklerle [UserOptions](/dotnet/api/microsoft.aspnetcore.identity.useroptions) 'ı belirtir.</span><span class="sxs-lookup"><span data-stu-id="261a4-207">[IdentityOptions.User](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.user) specifies the [UserOptions](/dotnet/api/microsoft.aspnetcore.identity.useroptions) with the properties shown in the table.</span></span>

| <span data-ttu-id="261a4-208">Özellik</span><span class="sxs-lookup"><span data-stu-id="261a4-208">Property</span></span> | <span data-ttu-id="261a4-209">Açıklama</span><span class="sxs-lookup"><span data-stu-id="261a4-209">Description</span></span> | <span data-ttu-id="261a4-210">Varsayılan</span><span class="sxs-lookup"><span data-stu-id="261a4-210">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="261a4-211">AllowedUserNameCharacters</span><span class="sxs-lookup"><span data-stu-id="261a4-211">AllowedUserNameCharacters</span></span>](/dotnet/api/microsoft.aspnetcore.identity.useroptions.allowedusernamecharacters) | <span data-ttu-id="261a4-212">Kullanıcı adında izin verilen karakterler.</span><span class="sxs-lookup"><span data-stu-id="261a4-212">Allowed characters in the username.</span></span> | <span data-ttu-id="261a4-213">abcdefghijklmnopqrstuvwxyz</span><span class="sxs-lookup"><span data-stu-id="261a4-213">abcdefghijklmnopqrstuvwxyz</span></span><br><span data-ttu-id="261a4-214">ABCDEFGHIJKLMNOPQRSTUVWXYZ</span><span class="sxs-lookup"><span data-stu-id="261a4-214">ABCDEFGHIJKLMNOPQRSTUVWXYZ</span></span><br><span data-ttu-id="261a4-215">0123456789</span><span class="sxs-lookup"><span data-stu-id="261a4-215">0123456789</span></span><br><span data-ttu-id="261a4-216">-.\_@+</span><span class="sxs-lookup"><span data-stu-id="261a4-216">-.\_@+</span></span> |
| [<span data-ttu-id="261a4-217">RequireUniqueEmail</span><span class="sxs-lookup"><span data-stu-id="261a4-217">RequireUniqueEmail</span></span>](/dotnet/api/microsoft.aspnetcore.identity.useroptions.requireuniqueemail) | <span data-ttu-id="261a4-218">Her kullanıcının benzersiz bir e-postasına sahip olmasını gerektirir.</span><span class="sxs-lookup"><span data-stu-id="261a4-218">Requires each user to have a unique email.</span></span> | `false` |

### <a name="cookie-settings"></a><span data-ttu-id="261a4-219">Tanımlama bilgisi ayarları</span><span class="sxs-lookup"><span data-stu-id="261a4-219">Cookie settings</span></span>

<span data-ttu-id="261a4-220">`Startup.ConfigureServices`içinde uygulamanın tanımlama bilgilerini yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="261a4-220">Configure the app's cookie in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="261a4-221">`AddIdentity` veya `AddDefaultIdentity`çağrıldıktan **sonra** [configureapplicationcookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_ConfigureApplicationCookie_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_Cookies_CookieAuthenticationOptions__) çağrılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="261a4-221">[ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_ConfigureApplicationCookie_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_Cookies_CookieAuthenticationOptions__) must be called **after** calling `AddIdentity` or `AddDefaultIdentity`.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_cookie)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

::: moniker-end

<span data-ttu-id="261a4-222">Daha fazla bilgi için bkz. [tanımlama, ıeauthenticationoptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions).</span><span class="sxs-lookup"><span data-stu-id="261a4-222">For more information, see [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions).</span></span>

## <a name="password-hasher-options"></a><span data-ttu-id="261a4-223">Parola hasher seçenekleri</span><span class="sxs-lookup"><span data-stu-id="261a4-223">Password Hasher options</span></span>

<span data-ttu-id="261a4-224"><xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions> parola karmalama seçeneklerini alır ve ayarlar.</span><span class="sxs-lookup"><span data-stu-id="261a4-224"><xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions> gets and sets options for password hashing.</span></span>

| <span data-ttu-id="261a4-225">Seçenek</span><span class="sxs-lookup"><span data-stu-id="261a4-225">Option</span></span> | <span data-ttu-id="261a4-226">Açıklama</span><span class="sxs-lookup"><span data-stu-id="261a4-226">Description</span></span> |
| ------ | ----------- |
| <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.CompatibilityMode> | <span data-ttu-id="261a4-227">Yeni parolalar Karma oluşturulurken kullanılan uyumluluk modu.</span><span class="sxs-lookup"><span data-stu-id="261a4-227">The compatibility mode used when hashing new passwords.</span></span> <span data-ttu-id="261a4-228"><xref:Microsoft.AspNetCore.Identity.PasswordHasherCompatibilityMode.IdentityV3> değerini varsayılan olarak alır.</span><span class="sxs-lookup"><span data-stu-id="261a4-228">Defaults to <xref:Microsoft.AspNetCore.Identity.PasswordHasherCompatibilityMode.IdentityV3>.</span></span> <span data-ttu-id="261a4-229">Karma parolanın *Biçim işaretleyicisi*olarak adlandırılan ilk baytı, parolayı karma olarak kullanılan karma algoritmanın sürümünü belirtir.</span><span class="sxs-lookup"><span data-stu-id="261a4-229">The first byte of a hashed password, called a *format marker*, specifies the version of the hashing algorithm used to hash the password.</span></span> <span data-ttu-id="261a4-230">Bir karmaya karşı bir parola doğrulanırken <xref:Microsoft.AspNetCore.Identity.PasswordHasher`1.VerifyHashedPassword*> yöntemi ilk bayta göre doğru algoritmayı seçer.</span><span class="sxs-lookup"><span data-stu-id="261a4-230">When verifying a password against a hash, the <xref:Microsoft.AspNetCore.Identity.PasswordHasher`1.VerifyHashedPassword*> method selects the correct algorithm based on the first byte.</span></span> <span data-ttu-id="261a4-231">Bir istemci, parolayı karma hale almak için hangi algoritma sürümünün kullanıldığına bakılmaksızın kimlik doğrulaması yapabilir.</span><span class="sxs-lookup"><span data-stu-id="261a4-231">A client is able to authenticate regardless of which version of the algorithm was used to hash the password.</span></span> <span data-ttu-id="261a4-232">Uyumluluk modunun ayarlanması *yeni parolaların*karmasını etkiler.</span><span class="sxs-lookup"><span data-stu-id="261a4-232">Setting the compatibility mode affects the hashing of *new passwords*.</span></span> |
| <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.IterationCount> | <span data-ttu-id="261a4-233">PBKDF2 kullanarak parolaları karmalama sırasında kullanılan yineleme sayısı.</span><span class="sxs-lookup"><span data-stu-id="261a4-233">The number of iterations used when hashing passwords using PBKDF2.</span></span> <span data-ttu-id="261a4-234">Bu değer yalnızca <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.CompatibilityMode> <xref:Microsoft.AspNetCore.Identity.PasswordHasherCompatibilityMode.IdentityV3>olarak ayarlandığında kullanılır.</span><span class="sxs-lookup"><span data-stu-id="261a4-234">This value is only used when the <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.CompatibilityMode> is set to <xref:Microsoft.AspNetCore.Identity.PasswordHasherCompatibilityMode.IdentityV3>.</span></span> <span data-ttu-id="261a4-235">Değerin pozitif bir tamsayı olması ve varsayılan olarak `10000`olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="261a4-235">The value must be a positive integer and defaults to `10000`.</span></span> |

<span data-ttu-id="261a4-236">Aşağıdaki örnekte <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.IterationCount>, `Startup.ConfigureServices``12000` olarak ayarlanmıştır:</span><span class="sxs-lookup"><span data-stu-id="261a4-236">In the following example, the <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.IterationCount> is set to `12000` in `Startup.ConfigureServices`:</span></span>

```csharp
// using Microsoft.AspNetCore.Identity;

services.Configure<PasswordHasherOptions>(option =>
{
    option.IterationCount = 12000;
});
```
