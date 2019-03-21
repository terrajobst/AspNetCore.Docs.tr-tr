---
title: ASP.NET Core kimliği yapılandırma
author: AdrienTorris
description: ASP.NET Core kimliği varsayılan değerleri anlamanıza ve özel değerler kullanılacak kimlik özelliklerini yapılandırmayı öğrenin.
ms.author: riande
ms.date: 02/11/2019
uid: security/authentication/identity-configuration
ms.openlocfilehash: 823182bed2cb953e07f9374d135868aeb2be9c60
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2019
ms.locfileid: "58210125"
---
# <a name="configure-aspnet-core-identity"></a><span data-ttu-id="e847f-103">ASP.NET Core kimliği yapılandırma</span><span class="sxs-lookup"><span data-stu-id="e847f-103">Configure ASP.NET Core Identity</span></span>

<span data-ttu-id="e847f-104">ASP.NET Core kimliği, parola ilkesi ve kilitleme tanımlama bilgisi yapılandırması gibi ayarları için varsayılan değerleri kullanır.</span><span class="sxs-lookup"><span data-stu-id="e847f-104">ASP.NET Core Identity uses default values for settings such as password policy, lockout, and cookie configuration.</span></span> <span data-ttu-id="e847f-105">Bu ayarlar kılınabilir `Startup` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="e847f-105">These settings can be overridden in the `Startup` class.</span></span>

## <a name="identity-options"></a><span data-ttu-id="e847f-106">Kimlik seçenekleri</span><span class="sxs-lookup"><span data-stu-id="e847f-106">Identity options</span></span>

<span data-ttu-id="e847f-107">[IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) sınıf kimlik sistemi yapılandırmak için kullanılabilir seçenekleri temsil eder.</span><span class="sxs-lookup"><span data-stu-id="e847f-107">The [IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) class represents the options that can be used to configure the Identity system.</span></span> <span data-ttu-id="e847f-108">`IdentityOptions` ayarlanmalıdır **sonra** çağırma `AddIdentity` veya `AddDefaultIdentity`.</span><span class="sxs-lookup"><span data-stu-id="e847f-108">`IdentityOptions` must be set **after** calling `AddIdentity` or `AddDefaultIdentity`.</span></span>

### <a name="claims-identity"></a><span data-ttu-id="e847f-109">Talep Kimliği</span><span class="sxs-lookup"><span data-stu-id="e847f-109">Claims Identity</span></span>

<span data-ttu-id="e847f-110">[IdentityOptions.ClaimsIdentity](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.claimsidentity) belirtir [ClaimsIdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions) aşağıdaki tabloda gösterilen özelliklere sahip.</span><span class="sxs-lookup"><span data-stu-id="e847f-110">[IdentityOptions.ClaimsIdentity](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.claimsidentity) specifies the [ClaimsIdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions) with the properties shown in the following table.</span></span>

| <span data-ttu-id="e847f-111">Özellik</span><span class="sxs-lookup"><span data-stu-id="e847f-111">Property</span></span> | <span data-ttu-id="e847f-112">Açıklama</span><span class="sxs-lookup"><span data-stu-id="e847f-112">Description</span></span> | <span data-ttu-id="e847f-113">Varsayılan</span><span class="sxs-lookup"><span data-stu-id="e847f-113">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="e847f-114">RoleClaimType</span><span class="sxs-lookup"><span data-stu-id="e847f-114">RoleClaimType</span></span>](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.roleclaimtype) | <span data-ttu-id="e847f-115">Alır veya bir rolü talep için kullanılan talep türünü ayarlar.</span><span class="sxs-lookup"><span data-stu-id="e847f-115">Gets or sets the claim type used for a role claim.</span></span> | [<span data-ttu-id="e847f-116">ClaimTypes.Role</span><span class="sxs-lookup"><span data-stu-id="e847f-116">ClaimTypes.Role</span></span>](/dotnet/api/system.security.claims.claimtypes.role) |
| [<span data-ttu-id="e847f-117">SecurityStampClaimType</span><span class="sxs-lookup"><span data-stu-id="e847f-117">SecurityStampClaimType</span></span>](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.securitystampclaimtype) | <span data-ttu-id="e847f-118">Alır veya güvenlik damgası talep için kullanılan talep türünü ayarlar.</span><span class="sxs-lookup"><span data-stu-id="e847f-118">Gets or sets the claim type used for the security stamp claim.</span></span> | `AspNet.Identity.SecurityStamp` |
| [<span data-ttu-id="e847f-119">UserIdClaimType</span><span class="sxs-lookup"><span data-stu-id="e847f-119">UserIdClaimType</span></span>](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.useridclaimtype) | <span data-ttu-id="e847f-120">Alır veya kullanıcı tanımlayıcısı talebi için kullanılan talep türünü ayarlar.</span><span class="sxs-lookup"><span data-stu-id="e847f-120">Gets or sets the claim type used for the user identifier claim.</span></span> | [<span data-ttu-id="e847f-121">ClaimTypes.NameIdentifier</span><span class="sxs-lookup"><span data-stu-id="e847f-121">ClaimTypes.NameIdentifier</span></span>](/dotnet/api/system.security.claims.claimtypes.nameidentifier) |
| [<span data-ttu-id="e847f-122">UserNameClaimType</span><span class="sxs-lookup"><span data-stu-id="e847f-122">UserNameClaimType</span></span>](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.usernameclaimtype) | <span data-ttu-id="e847f-123">Alır veya kullanıcı adı talebi için kullanılan talep türünü ayarlar.</span><span class="sxs-lookup"><span data-stu-id="e847f-123">Gets or sets the claim type used for the user name claim.</span></span> | [<span data-ttu-id="e847f-124">ClaimTypes.Name</span><span class="sxs-lookup"><span data-stu-id="e847f-124">ClaimTypes.Name</span></span>](/dotnet/api/system.security.claims.claimtypes.name) |

### <a name="lockout"></a><span data-ttu-id="e847f-125">Kilitleme</span><span class="sxs-lookup"><span data-stu-id="e847f-125">Lockout</span></span>

<span data-ttu-id="e847f-126">Kilitleme kümesinde [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync#Microsoft_AspNetCore_Identity_SignInManager_1_PasswordSignInAsync_System_String_System_String_System_Boolean_System_Boolean_) yöntemi:</span><span class="sxs-lookup"><span data-stu-id="e847f-126">Lockout is set in the [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync#Microsoft_AspNetCore_Identity_SignInManager_1_PasswordSignInAsync_System_String_System_String_System_Boolean_System_Boolean_) method:</span></span>

[!code-csharp[](identity-configuration/sample/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=9)]

<span data-ttu-id="e847f-127">Yukarıdaki kod dayanır `Login` kimlik şablonu.</span><span class="sxs-lookup"><span data-stu-id="e847f-127">The preceding code is based on the `Login` Identity template.</span></span> 

<span data-ttu-id="e847f-128">Kilitleme seçenekleri ayarlanır `StartUp.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="e847f-128">Lockout options are set in `StartUp.ConfigureServices`:</span></span>

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_lock)]

<span data-ttu-id="e847f-129">Önceki kod kümeleri [IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) varsayılan değerlere sahip.</span><span class="sxs-lookup"><span data-stu-id="e847f-129">The preceding code sets the [IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) with default values.</span></span>

<span data-ttu-id="e847f-130">Başarılı bir kimlik doğrulaması başarısız erişim denemesi sayısını sıfırlar ve saatini sıfırlar.</span><span class="sxs-lookup"><span data-stu-id="e847f-130">A successful authentication resets the failed access attempts count and resets the clock.</span></span>

<span data-ttu-id="e847f-131">[IdentityOptions.Lockout](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.lockout) belirtir [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) tabloda gösterilen özelliklere sahip.</span><span class="sxs-lookup"><span data-stu-id="e847f-131">[IdentityOptions.Lockout](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.lockout) specifies the [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) with the properties shown in the table.</span></span>

| <span data-ttu-id="e847f-132">Özellik</span><span class="sxs-lookup"><span data-stu-id="e847f-132">Property</span></span> | <span data-ttu-id="e847f-133">Açıklama</span><span class="sxs-lookup"><span data-stu-id="e847f-133">Description</span></span> | <span data-ttu-id="e847f-134">Varsayılan</span><span class="sxs-lookup"><span data-stu-id="e847f-134">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="e847f-135">AllowedForNewUsers</span><span class="sxs-lookup"><span data-stu-id="e847f-135">AllowedForNewUsers</span></span>](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.allowedfornewusers) | <span data-ttu-id="e847f-136">Yeni bir kullanıcı, kilitlenir belirler.</span><span class="sxs-lookup"><span data-stu-id="e847f-136">Determines if a new user can be locked out.</span></span> | `true` |
| [<span data-ttu-id="e847f-137">DefaultLockoutTimeSpan</span><span class="sxs-lookup"><span data-stu-id="e847f-137">DefaultLockoutTimeSpan</span></span>](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan) | <span data-ttu-id="e847f-138">Bir kilitlenme oluştuğunda süreyi kullanıcı kilitlidir.</span><span class="sxs-lookup"><span data-stu-id="e847f-138">The amount of time a user is locked out when a lockout occurs.</span></span> | <span data-ttu-id="e847f-139">5 dakika</span><span class="sxs-lookup"><span data-stu-id="e847f-139">5 minutes</span></span> |
| [<span data-ttu-id="e847f-140">MaxFailedAccessAttempts</span><span class="sxs-lookup"><span data-stu-id="e847f-140">MaxFailedAccessAttempts</span></span>](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) | <span data-ttu-id="e847f-141">Kilitleme etkinse, bir kullanıcı kilitli kadar başarısız erişim denemesi sayısı.</span><span class="sxs-lookup"><span data-stu-id="e847f-141">The number of failed access attempts until a user is locked out, if lockout is enabled.</span></span> | <span data-ttu-id="e847f-142">5</span><span class="sxs-lookup"><span data-stu-id="e847f-142">5</span></span> |

### <a name="password"></a><span data-ttu-id="e847f-143">Parola</span><span class="sxs-lookup"><span data-stu-id="e847f-143">Password</span></span>

<span data-ttu-id="e847f-144">Varsayılan olarak parola bir büyük harf, küçük harf, rakam ve alfasayısal olmayan karakter içerdiğini kimlik gerektirir.</span><span class="sxs-lookup"><span data-stu-id="e847f-144">By default, Identity requires that passwords contain an uppercase character, lowercase character, a digit, and a non-alphanumeric character.</span></span> <span data-ttu-id="e847f-145">Parola en az altı karakter uzunluğunda olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="e847f-145">Passwords must be at least six characters long.</span></span> <span data-ttu-id="e847f-146">[PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) ayarlanabilir `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="e847f-146">[PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) can be set in `Startup.ConfigureServices`.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_pw)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

::: moniker-end

<span data-ttu-id="e847f-147">[IdentityOptions.Password](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.password) belirtir [PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) tabloda gösterilen özelliklere sahip.</span><span class="sxs-lookup"><span data-stu-id="e847f-147">[IdentityOptions.Password](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.password) specifies the [PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) with the properties shown in the table.</span></span>

::: moniker range=">= aspnetcore-2.0"

| <span data-ttu-id="e847f-148">Özellik</span><span class="sxs-lookup"><span data-stu-id="e847f-148">Property</span></span> | <span data-ttu-id="e847f-149">Açıklama</span><span class="sxs-lookup"><span data-stu-id="e847f-149">Description</span></span> | <span data-ttu-id="e847f-150">Varsayılan</span><span class="sxs-lookup"><span data-stu-id="e847f-150">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="e847f-151">RequireDigit</span><span class="sxs-lookup"><span data-stu-id="e847f-151">RequireDigit</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredigit) | <span data-ttu-id="e847f-152">Parolayı 0-9 arasında bir sayı gerektirir.</span><span class="sxs-lookup"><span data-stu-id="e847f-152">Requires a number between 0-9 in the password.</span></span> | `true` |
| [<span data-ttu-id="e847f-153">RequiredLength</span><span class="sxs-lookup"><span data-stu-id="e847f-153">RequiredLength</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredlength) | <span data-ttu-id="e847f-154">Minimum parola uzunluğu.</span><span class="sxs-lookup"><span data-stu-id="e847f-154">The minimum length of the password.</span></span> | <span data-ttu-id="e847f-155">6</span><span class="sxs-lookup"><span data-stu-id="e847f-155">6</span></span> |
| [<span data-ttu-id="e847f-156">RequireLowercase</span><span class="sxs-lookup"><span data-stu-id="e847f-156">RequireLowercase</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirelowercase) | <span data-ttu-id="e847f-157">Parola küçük harf karakter gerektirir.</span><span class="sxs-lookup"><span data-stu-id="e847f-157">Requires a lowercase character in the password.</span></span> | `true` |
| [<span data-ttu-id="e847f-158">RequireNonAlphanumeric</span><span class="sxs-lookup"><span data-stu-id="e847f-158">RequireNonAlphanumeric</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirenonalphanumeric) | <span data-ttu-id="e847f-159">Paroladaki alfasayısal olmayan bir karakter gerektirir.</span><span class="sxs-lookup"><span data-stu-id="e847f-159">Requires a non-alphanumeric character in the password.</span></span> | `true` |
| [<span data-ttu-id="e847f-160">RequiredUniqueChars</span><span class="sxs-lookup"><span data-stu-id="e847f-160">RequiredUniqueChars</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireduniquechars) | <span data-ttu-id="e847f-161">Yalnızca ASP.NET Core 2.0 ve üzeri için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="e847f-161">Only applies to ASP.NET Core 2.0 or later.</span></span><br><br> <span data-ttu-id="e847f-162">Parola ayrı karakter sayısı gerektirir.</span><span class="sxs-lookup"><span data-stu-id="e847f-162">Requires the number of distinct characters in the password.</span></span> | <span data-ttu-id="e847f-163">1.</span><span class="sxs-lookup"><span data-stu-id="e847f-163">1</span></span> |
| [<span data-ttu-id="e847f-164">RequireUppercase</span><span class="sxs-lookup"><span data-stu-id="e847f-164">RequireUppercase</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireuppercase) | <span data-ttu-id="e847f-165">Parola bir büyük harf karakteri gerektirir.</span><span class="sxs-lookup"><span data-stu-id="e847f-165">Requires an uppercase character in the password.</span></span> | `true` |

::: moniker-end

::: moniker range="< aspnetcore-2.0"

| <span data-ttu-id="e847f-166">Özellik</span><span class="sxs-lookup"><span data-stu-id="e847f-166">Property</span></span> | <span data-ttu-id="e847f-167">Açıklama</span><span class="sxs-lookup"><span data-stu-id="e847f-167">Description</span></span> | <span data-ttu-id="e847f-168">Varsayılan</span><span class="sxs-lookup"><span data-stu-id="e847f-168">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="e847f-169">RequireDigit</span><span class="sxs-lookup"><span data-stu-id="e847f-169">RequireDigit</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredigit) | <span data-ttu-id="e847f-170">Parolayı 0-9 arasında bir sayı gerektirir.</span><span class="sxs-lookup"><span data-stu-id="e847f-170">Requires a number between 0-9 in the password.</span></span> | `true` |
| [<span data-ttu-id="e847f-171">RequiredLength</span><span class="sxs-lookup"><span data-stu-id="e847f-171">RequiredLength</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredlength) | <span data-ttu-id="e847f-172">Minimum parola uzunluğu.</span><span class="sxs-lookup"><span data-stu-id="e847f-172">The minimum length of the password.</span></span> | <span data-ttu-id="e847f-173">6</span><span class="sxs-lookup"><span data-stu-id="e847f-173">6</span></span> |
| [<span data-ttu-id="e847f-174">RequireLowercase</span><span class="sxs-lookup"><span data-stu-id="e847f-174">RequireLowercase</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirelowercase) | <span data-ttu-id="e847f-175">Parola küçük harf karakter gerektirir.</span><span class="sxs-lookup"><span data-stu-id="e847f-175">Requires a lowercase character in the password.</span></span> | `true` |
| [<span data-ttu-id="e847f-176">RequireNonAlphanumeric</span><span class="sxs-lookup"><span data-stu-id="e847f-176">RequireNonAlphanumeric</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirenonalphanumeric) | <span data-ttu-id="e847f-177">Paroladaki alfasayısal olmayan bir karakter gerektirir.</span><span class="sxs-lookup"><span data-stu-id="e847f-177">Requires a non-alphanumeric character in the password.</span></span> | `true` |
| [<span data-ttu-id="e847f-178">RequireUppercase</span><span class="sxs-lookup"><span data-stu-id="e847f-178">RequireUppercase</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireuppercase) | <span data-ttu-id="e847f-179">Parola bir büyük harf karakteri gerektirir.</span><span class="sxs-lookup"><span data-stu-id="e847f-179">Requires an uppercase character in the password.</span></span> | `true` |

::: moniker-end

### <a name="sign-in"></a><span data-ttu-id="e847f-180">oturum açma</span><span class="sxs-lookup"><span data-stu-id="e847f-180">Sign-in</span></span>

<span data-ttu-id="e847f-181">Aşağıdaki kod kümeleri `SignIn` ayarlarına (varsayılan değer):</span><span class="sxs-lookup"><span data-stu-id="e847f-181">The following code sets `SignIn` settings (to default values):</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_si)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)] 

::: moniker-end

<span data-ttu-id="e847f-182">[IdentityOptions.SignIn](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.signin) belirtir [SignInOptions](/dotnet/api/microsoft.aspnetcore.identity.signinoptions) tabloda gösterilen özelliklere sahip.</span><span class="sxs-lookup"><span data-stu-id="e847f-182">[IdentityOptions.SignIn](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.signin) specifies the [SignInOptions](/dotnet/api/microsoft.aspnetcore.identity.signinoptions) with the properties shown in the table.</span></span>

| <span data-ttu-id="e847f-183">Özellik</span><span class="sxs-lookup"><span data-stu-id="e847f-183">Property</span></span> | <span data-ttu-id="e847f-184">Açıklama</span><span class="sxs-lookup"><span data-stu-id="e847f-184">Description</span></span> | <span data-ttu-id="e847f-185">Varsayılan</span><span class="sxs-lookup"><span data-stu-id="e847f-185">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="e847f-186">RequireConfirmedEmail</span><span class="sxs-lookup"><span data-stu-id="e847f-186">RequireConfirmedEmail</span></span>](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedemail) | <span data-ttu-id="e847f-187">Oturum açmak için bir onaylanan e-posta gerektirir.</span><span class="sxs-lookup"><span data-stu-id="e847f-187">Requires a confirmed email to sign in.</span></span> | `false` |
| [<span data-ttu-id="e847f-188">RequireConfirmedPhoneNumber</span><span class="sxs-lookup"><span data-stu-id="e847f-188">RequireConfirmedPhoneNumber</span></span>](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedphonenumber) | <span data-ttu-id="e847f-189">Oturum açmak onaylanan telefon numarası gerektirir.</span><span class="sxs-lookup"><span data-stu-id="e847f-189">Requires a confirmed phone number to sign in.</span></span> | `false` |

### <a name="tokens"></a><span data-ttu-id="e847f-190">Belirteçler</span><span class="sxs-lookup"><span data-stu-id="e847f-190">Tokens</span></span>

<span data-ttu-id="e847f-191">[IdentityOptions.Tokens](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.tokens) belirtir [TokenOptions](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions) tabloda gösterilen özelliklere sahip.</span><span class="sxs-lookup"><span data-stu-id="e847f-191">[IdentityOptions.Tokens](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.tokens) specifies the [TokenOptions](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions) with the properties shown in the table.</span></span>

|                                                        <span data-ttu-id="e847f-192">Özellik</span><span class="sxs-lookup"><span data-stu-id="e847f-192">Property</span></span>                                                         |                                                                                      <span data-ttu-id="e847f-193">Açıklama</span><span class="sxs-lookup"><span data-stu-id="e847f-193">Description</span></span>                                                                                      |
|-------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     [<span data-ttu-id="e847f-194">AuthenticatorTokenProvider</span><span class="sxs-lookup"><span data-stu-id="e847f-194">AuthenticatorTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.authenticatortokenprovider)     |                                       <span data-ttu-id="e847f-195">Alır veya ayarlar `AuthenticatorTokenProvider` iki öğeli bir kimlik doğrulayıcı ile oturum açma işlemleri doğrulamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="e847f-195">Gets or sets the `AuthenticatorTokenProvider` used to validate two-factor sign-ins with an authenticator.</span></span>                                       |
|       [<span data-ttu-id="e847f-196">ChangeEmailTokenProvider</span><span class="sxs-lookup"><span data-stu-id="e847f-196">ChangeEmailTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changeemailtokenprovider)       |                                     <span data-ttu-id="e847f-197">Alır veya ayarlar `ChangeEmailTokenProvider` e-posta değişikliği onay e-postalarda kullanılan belirteçleri oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="e847f-197">Gets or sets the `ChangeEmailTokenProvider` used to generate tokens used in email change confirmation emails.</span></span>                                     |
| [<span data-ttu-id="e847f-198">ChangePhoneNumberTokenProvider</span><span class="sxs-lookup"><span data-stu-id="e847f-198">ChangePhoneNumberTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changephonenumbertokenprovider) |                                      <span data-ttu-id="e847f-199">Alır veya ayarlar `ChangePhoneNumberTokenProvider` telefon numaralarını değiştirirken kullanılan belirteçleri oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="e847f-199">Gets or sets the `ChangePhoneNumberTokenProvider` used to generate tokens used when changing phone numbers.</span></span>                                      |
| [<span data-ttu-id="e847f-200">EmailConfirmationTokenProvider</span><span class="sxs-lookup"><span data-stu-id="e847f-200">EmailConfirmationTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.emailconfirmationtokenprovider) |                                             <span data-ttu-id="e847f-201">Alır veya hesap doğrulama e-postalarda kullanılan belirteçleri oluşturmak için kullanılan belirteç sağlayıcısı ayarlar.</span><span class="sxs-lookup"><span data-stu-id="e847f-201">Gets or sets the token provider used to generate tokens used in account confirmation emails.</span></span>                                              |
|     [<span data-ttu-id="e847f-202">PasswordResetTokenProvider</span><span class="sxs-lookup"><span data-stu-id="e847f-202">PasswordResetTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.passwordresettokenprovider)     | <span data-ttu-id="e847f-203">Alır veya ayarlar [IUserTwoFactorTokenProvider\<TUser >](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactortokenprovider-1) parola sıfırlama e-postalarda kullanılan belirteçleri oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="e847f-203">Gets or sets the [IUserTwoFactorTokenProvider\<TUser>](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactortokenprovider-1) used to generate tokens used in password reset emails.</span></span> |
|                    [<span data-ttu-id="e847f-204">ProviderMap</span><span class="sxs-lookup"><span data-stu-id="e847f-204">ProviderMap</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.providermap)                    |                <span data-ttu-id="e847f-205">Oluşturmak için kullanılan bir [kullanıcı belirteci sağlayıcısı](/dotnet/api/microsoft.aspnetcore.identity.tokenproviderdescriptor) anahtarla sağlayıcının adı olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="e847f-205">Used to construct a [User Token Provider](/dotnet/api/microsoft.aspnetcore.identity.tokenproviderdescriptor) with the key used as the provider's name.</span></span>                 |

### <a name="user"></a><span data-ttu-id="e847f-206">Kullanıcı</span><span class="sxs-lookup"><span data-stu-id="e847f-206">User</span></span>

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_user)]

<span data-ttu-id="e847f-207">[IdentityOptions.User](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.user) belirtir [UserOptions](/dotnet/api/microsoft.aspnetcore.identity.useroptions) tabloda gösterilen özelliklere sahip.</span><span class="sxs-lookup"><span data-stu-id="e847f-207">[IdentityOptions.User](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.user) specifies the [UserOptions](/dotnet/api/microsoft.aspnetcore.identity.useroptions) with the properties shown in the table.</span></span>

| <span data-ttu-id="e847f-208">Özellik</span><span class="sxs-lookup"><span data-stu-id="e847f-208">Property</span></span> | <span data-ttu-id="e847f-209">Açıklama</span><span class="sxs-lookup"><span data-stu-id="e847f-209">Description</span></span> | <span data-ttu-id="e847f-210">Varsayılan</span><span class="sxs-lookup"><span data-stu-id="e847f-210">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="e847f-211">AllowedUserNameCharacters</span><span class="sxs-lookup"><span data-stu-id="e847f-211">AllowedUserNameCharacters</span></span>](/dotnet/api/microsoft.aspnetcore.identity.useroptions.allowedusernamecharacters) | <span data-ttu-id="e847f-212">Kullanıcı adı izin verilen karakter.</span><span class="sxs-lookup"><span data-stu-id="e847f-212">Allowed characters in the username.</span></span> | <span data-ttu-id="e847f-213">abcdefghijklmnopqrstuvwxyz</span><span class="sxs-lookup"><span data-stu-id="e847f-213">abcdefghijklmnopqrstuvwxyz</span></span><br><span data-ttu-id="e847f-214">ABCDEFGHIJKLMNOPQRSTUVWXYZ</span><span class="sxs-lookup"><span data-stu-id="e847f-214">ABCDEFGHIJKLMNOPQRSTUVWXYZ</span></span><br><span data-ttu-id="e847f-215">0123456789</span><span class="sxs-lookup"><span data-stu-id="e847f-215">0123456789</span></span><br><span data-ttu-id="e847f-216">-.\_@+</span><span class="sxs-lookup"><span data-stu-id="e847f-216">-.\_@+</span></span> |
| [<span data-ttu-id="e847f-217">RequireUniqueEmail</span><span class="sxs-lookup"><span data-stu-id="e847f-217">RequireUniqueEmail</span></span>](/dotnet/api/microsoft.aspnetcore.identity.useroptions.requireuniqueemail) | <span data-ttu-id="e847f-218">Her kullanıcının benzersiz bir e-posta olmasını gerektirir.</span><span class="sxs-lookup"><span data-stu-id="e847f-218">Requires each user to have a unique email.</span></span> | `false` |

### <a name="cookie-settings"></a><span data-ttu-id="e847f-219">Tanımlama bilgisi ayarları</span><span class="sxs-lookup"><span data-stu-id="e847f-219">Cookie settings</span></span>

<span data-ttu-id="e847f-220">Uygulamanın tanımlama bilgisini yapılandırın `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="e847f-220">Configure the app's cookie in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="e847f-221">[ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_ConfigureApplicationCookie_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_Cookies_CookieAuthenticationOptions__) çağrılmalıdır **sonra** çağırma `AddIdentity` veya `AddDefaultIdentity`.</span><span class="sxs-lookup"><span data-stu-id="e847f-221">[ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_ConfigureApplicationCookie_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_Cookies_CookieAuthenticationOptions__) must be called **after** calling `AddIdentity` or `AddDefaultIdentity`.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_cookie)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

::: moniker-end

<span data-ttu-id="e847f-222">Daha fazla bilgi için [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions).</span><span class="sxs-lookup"><span data-stu-id="e847f-222">For more information, see [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions).</span></span>

## <a name="password-hasher-options"></a><span data-ttu-id="e847f-223">Parola karma değeri Oluşturucusu seçenekleri</span><span class="sxs-lookup"><span data-stu-id="e847f-223">Password Hasher options</span></span>

<span data-ttu-id="e847f-224"><xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions> parola karma seçeneklerini ayarlar ve alır.</span><span class="sxs-lookup"><span data-stu-id="e847f-224"><xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions> gets and sets options for password hashing.</span></span>

| <span data-ttu-id="e847f-225">Seçenek</span><span class="sxs-lookup"><span data-stu-id="e847f-225">Option</span></span> | <span data-ttu-id="e847f-226">Açıklama</span><span class="sxs-lookup"><span data-stu-id="e847f-226">Description</span></span> |
| ------ | ----------- |
| <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.CompatibilityMode> | <span data-ttu-id="e847f-227">Yeni parola kararken kullanılan uyumluluk modu.</span><span class="sxs-lookup"><span data-stu-id="e847f-227">The compatibility mode used when hashing new passwords.</span></span> <span data-ttu-id="e847f-228">Varsayılan olarak <xref:Microsoft.AspNetCore.Identity.PasswordHasherCompatibilityMode.IdentityV3>.</span><span class="sxs-lookup"><span data-stu-id="e847f-228">Defaults to <xref:Microsoft.AspNetCore.Identity.PasswordHasherCompatibilityMode.IdentityV3>.</span></span> <span data-ttu-id="e847f-229">Adlı bir karma hale getirilen parolayla ilk baytına bir *biçimi işaret*, parola karması için kullanılan karma algoritması sürümünü belirtir.</span><span class="sxs-lookup"><span data-stu-id="e847f-229">The first byte of a hashed password, called a *format marker*, specifies the version of the hashing algorithm used to hash the password.</span></span> <span data-ttu-id="e847f-230">Parola Karması karşı doğrulanırken <xref:Microsoft.AspNetCore.Identity.PasswordHasher`1.VerifyHashedPassword*> yöntemi ilk baytın üzerinde göre doğru algoritmayı seçer.</span><span class="sxs-lookup"><span data-stu-id="e847f-230">When verifying a password against a hash, the <xref:Microsoft.AspNetCore.Identity.PasswordHasher`1.VerifyHashedPassword*> method selects the correct algorithm based on the first byte.</span></span> <span data-ttu-id="e847f-231">Bir istemci bakılmaksızın kimlik doğrulaması için parola karma hale hangi algoritmanın sürümü kullanıldı.</span><span class="sxs-lookup"><span data-stu-id="e847f-231">A client is able to authenticate regardless of which version of the algorithm was used to hash the password.</span></span> <span data-ttu-id="e847f-232">Uyumluluk modu ayarını etkiler, karma *yeni parolalar*.</span><span class="sxs-lookup"><span data-stu-id="e847f-232">Setting the compatibility mode affects the hashing of *new passwords*.</span></span> |
| <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.IterationCount> | <span data-ttu-id="e847f-233">Parolaları PBKDF2 kullanarak kararken kullanılan yineleme sayısı.</span><span class="sxs-lookup"><span data-stu-id="e847f-233">The number of iterations used when hashing passwords using PBKDF2.</span></span> <span data-ttu-id="e847f-234">Bu değer, yalnızca kullanılan zaman <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.CompatibilityMode> ayarlanır <xref:Microsoft.AspNetCore.Identity.PasswordHasherCompatibilityMode.IdentityV3>.</span><span class="sxs-lookup"><span data-stu-id="e847f-234">This value is only used when the <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.CompatibilityMode> is set to <xref:Microsoft.AspNetCore.Identity.PasswordHasherCompatibilityMode.IdentityV3>.</span></span> <span data-ttu-id="e847f-235">Değer pozitif bir tamsayı ve varsayılan olarak olmalıdır `10000`.</span><span class="sxs-lookup"><span data-stu-id="e847f-235">The value must be a positive integer and defaults to `10000`.</span></span> |

<span data-ttu-id="e847f-236">Aşağıdaki örnekte, <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.IterationCount> ayarlanır `12000` içinde `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="e847f-236">In the following example, the <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.IterationCount> is set to `12000` in `Startup.ConfigureServices`:</span></span>

```csharp
// using Microsoft.AspNetCore.Identity;

services.Configure<PasswordHasherOptions>(option =>
{
    option.IterationCount = 12000;
});
```
