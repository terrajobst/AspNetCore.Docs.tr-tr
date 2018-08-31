---
title: Ek talep ve ASP.NET Core, dış sağlayıcılardan gelen belirteçleri kalıcı
author: guardrex
description: Ek talep ve dış sağlayıcılardan gelen belirteçleri oluşturma hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/28/2018
uid: security/authentication/social/additional-claims
ms.openlocfilehash: 6386dd5f0bd901be01cf56081a6b9ca7f408f9f9
ms.sourcegitcommit: a669c4e3f42e387e214a354ac4143555602e6f66
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/31/2018
ms.locfileid: "43336236"
---
# <a name="persist-additional-claims-and-tokens-from-external-providers-in-aspnet-core"></a><span data-ttu-id="e0223-103">Ek talep ve ASP.NET Core, dış sağlayıcılardan gelen belirteçleri kalıcı</span><span class="sxs-lookup"><span data-stu-id="e0223-103">Persist additional claims and tokens from external providers in ASP.NET Core</span></span>

<span data-ttu-id="e0223-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="e0223-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="e0223-105">ASP.NET Core uygulaması ek talep ve Facebook, Google, Microsoft ve Twitter gibi dış kimlik sağlayıcılardan gelen belirteçleri oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e0223-105">An ASP.NET Core app can establish additional claims and tokens from external authentication providers, such as Facebook, Google, Microsoft, and Twitter.</span></span> <span data-ttu-id="e0223-106">Her bir sağlayıcı platformu farklı kullanıcılar hakkında bilgi gösterir, ancak alma ve kullanıcı verilerini ek talepleri dönüştürme için desen aynıdır.</span><span class="sxs-lookup"><span data-stu-id="e0223-106">Each provider reveals different information about users on its platform, but the pattern for receiving and transforming user data into additional claims is the same.</span></span>

<span data-ttu-id="e0223-107">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e0223-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="prerequisite"></a><span data-ttu-id="e0223-108">Önkoşul</span><span class="sxs-lookup"><span data-stu-id="e0223-108">Prerequisite</span></span>

<span data-ttu-id="e0223-109">Uygulamayı desteklemek için hangi Dış kimlik doğrulama sağlayıcıları karar verin.</span><span class="sxs-lookup"><span data-stu-id="e0223-109">Decide which external authentication providers to support in the app.</span></span> <span data-ttu-id="e0223-110">Tüm sağlayıcılar için uygulamayı kaydetme ve bir istemci kimliği ve istemci gizli anahtarını alın.</span><span class="sxs-lookup"><span data-stu-id="e0223-110">For each provider, register the app and obtain a client ID and client secret.</span></span> <span data-ttu-id="e0223-111">Daha fazla bilgi için bkz. <xref:security/authentication/social/index>.</span><span class="sxs-lookup"><span data-stu-id="e0223-111">For more information, see <xref:security/authentication/social/index>.</span></span> <span data-ttu-id="e0223-112">[Örnek uygulaması](#sample-app-instructions) kullanan [Google kimlik doğrulama sağlayıcısı](xref:security/authentication/google-logins).</span><span class="sxs-lookup"><span data-stu-id="e0223-112">The [sample app](#sample-app-instructions) uses the [Google authentication provider](xref:security/authentication/google-logins).</span></span>

## <a name="authentication-provider-configuration"></a><span data-ttu-id="e0223-113">Kimlik doğrulama sağlayıcısı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="e0223-113">Authentication provider configuration</span></span>

### <a name="set-the-client-id-and-client-secret"></a><span data-ttu-id="e0223-114">İstemci Kimliğini ve istemci gizli dizisi ayarlayın</span><span class="sxs-lookup"><span data-stu-id="e0223-114">Set the client ID and client secret</span></span>

<span data-ttu-id="e0223-115">OAuth kimlik doğrulama sağlayıcısı, bir istemci kimliği ve istemci gizli anahtarı kullanarak bir uygulama ile bir güven ilişkisi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="e0223-115">The OAuth authentication provider establishes a trust relationship with an app using a client ID and client secret.</span></span> <span data-ttu-id="e0223-116">Uygulamayı sağlayıcıda kaydedildiğinde istemci Kimliğini ve istemci gizli değerleri uygulama için Dış kimlik doğrulama sağlayıcısı tarafından oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="e0223-116">Client ID and client secret values are created for the app by the external authentication provider when the app is registered with the provider.</span></span> <span data-ttu-id="e0223-117">Uygulamanın kullandığı her bir dış sağlayıcı bağımsız olarak sağlayıcının istemci Kimliğini ve istemci gizli anahtarı ile yapılandırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="e0223-117">Each external provider that the app uses must be configured independently with the provider's client ID and client secret.</span></span> <span data-ttu-id="e0223-118">Daha fazla bilgi için senaryonuz için geçerli olan dış kimlik doğrulama sağlayıcısı konulara bakın:</span><span class="sxs-lookup"><span data-stu-id="e0223-118">For more information, see the external authentication provider topics that apply to your scenario:</span></span>

* [<span data-ttu-id="e0223-119">Facebook kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="e0223-119">Facebook authentication</span></span>](xref:security/authentication/facebook-logins)
* [<span data-ttu-id="e0223-120">Google kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="e0223-120">Google authentication</span></span>](xref:security/authentication/google-logins)
* [<span data-ttu-id="e0223-121">Microsoft kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="e0223-121">Microsoft authentication</span></span>](xref:security/authentication/microsoft-logins)
* [<span data-ttu-id="e0223-122">Twitter kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="e0223-122">Twitter authentication</span></span>](xref:security/authentication/twitter-logins)
* [<span data-ttu-id="e0223-123">Diğer kimlik doğrulama sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="e0223-123">Other authentication providers</span></span>](xref:security/authentication/otherlogins)
* [<span data-ttu-id="e0223-124">Openıdconnect</span><span class="sxs-lookup"><span data-stu-id="e0223-124">OpenIdConnect</span></span>](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2)

<span data-ttu-id="e0223-125">Örnek uygulama, bir istemci kimliği ve istemci gizli anahtarı Google tarafından sağlanan Google kimlik doğrulama sağlayıcısı yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="e0223-125">The sample app configures the Google authentication provider with a client ID and client secret provided by Google:</span></span>

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=4,6)]

### <a name="establish-the-authentication-scope"></a><span data-ttu-id="e0223-126">Kimlik doğrulama kapsamı oluşturmak</span><span class="sxs-lookup"><span data-stu-id="e0223-126">Establish the authentication scope</span></span>

<span data-ttu-id="e0223-127">Belirterek sağlayıcıdan almak için izinler listesinden belirtin <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>.</span><span class="sxs-lookup"><span data-stu-id="e0223-127">Specify the list of permissions to retrieve from the provider by specifying the <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>.</span></span> <span data-ttu-id="e0223-128">Yaygın dış sağlayıcıları için kimlik doğrulaması kapsamları aşağıdaki tabloda görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="e0223-128">Authentication scopes for common external providers appear in the following table.</span></span>

| <span data-ttu-id="e0223-129">Sağlayıcı</span><span class="sxs-lookup"><span data-stu-id="e0223-129">Provider</span></span>  | <span data-ttu-id="e0223-130">Kapsam</span><span class="sxs-lookup"><span data-stu-id="e0223-130">Scope</span></span>                                                            |
| --------- | ---------------------------------------------------------------- |
| <span data-ttu-id="e0223-131">Facebook</span><span class="sxs-lookup"><span data-stu-id="e0223-131">Facebook</span></span>  | `https://www.facebook.com/dialog/oauth`                          |
| <span data-ttu-id="e0223-132">Google</span><span class="sxs-lookup"><span data-stu-id="e0223-132">Google</span></span>    | `https://www.googleapis.com/auth/plus.login`                     |
| <span data-ttu-id="e0223-133">Microsoft</span><span class="sxs-lookup"><span data-stu-id="e0223-133">Microsoft</span></span> | `https://login.microsoftonline.com/common/oauth2/v2.0/authorize` |
| <span data-ttu-id="e0223-134">Twitter</span><span class="sxs-lookup"><span data-stu-id="e0223-134">Twitter</span></span>   | `https://api.twitter.com/oauth/authenticate`                     |

<span data-ttu-id="e0223-135">Google örnek uygulamanın eklediği `plus.login` Google + oturum açma izinleri istemek için kapsamı:</span><span class="sxs-lookup"><span data-stu-id="e0223-135">The sample app adds the Google `plus.login` scope to request Google+ sign in permissions:</span></span>

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=7)]

### <a name="map-user-data-keys-and-create-claims"></a><span data-ttu-id="e0223-136">Kullanıcı veri anahtarları eşleyin ve talep oluşturma</span><span class="sxs-lookup"><span data-stu-id="e0223-136">Map user data keys and create claims</span></span>

<span data-ttu-id="e0223-137">Sağlayıcının seçeneklerinde belirtin bir <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> her anahtarın dış sağlayıcının JSON kullanıcı verileri için oturum açma okumak uygulama kimliği.</span><span class="sxs-lookup"><span data-stu-id="e0223-137">In the provider's options, specify a <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> for each key in the external provider's JSON user data for the app identity to read on sign in.</span></span> <span data-ttu-id="e0223-138">Talep türleri hakkında daha fazla bilgi için bkz. <xref:System.Security.Claims.ClaimTypes>.</span><span class="sxs-lookup"><span data-stu-id="e0223-138">For more information on claim types, see <xref:System.Security.Claims.ClaimTypes>.</span></span>

<span data-ttu-id="e0223-139">Örnek uygulamayı oluşturur bir <xref:System.Security.Claims.ClaimTypes.Gender> gelen talep `gender` Google kullanıcı verileri anahtar:</span><span class="sxs-lookup"><span data-stu-id="e0223-139">The sample app creates a <xref:System.Security.Claims.ClaimTypes.Gender> claim from the `gender` key in Google user data:</span></span>

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=8)]

<span data-ttu-id="e0223-140">İçinde <xref:Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync*>e <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) ile uygulama oturum açmış <xref:Microsoft.AspNetCore.Identity.SignInManager`1.SignInAsync*>.</span><span class="sxs-lookup"><span data-stu-id="e0223-140">In <xref:Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync*>, an <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) is signed into the app with <xref:Microsoft.AspNetCore.Identity.SignInManager`1.SignInAsync*>.</span></span> <span data-ttu-id="e0223-141">Oturum açma işlemi sırasında <xref:Microsoft.AspNetCore.Identity.UserManager`1> depolayabilirsiniz bir `ApplicationUser` bulunan kullanıcı verileri için talep <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>.</span><span class="sxs-lookup"><span data-stu-id="e0223-141">During the sign in process, the <xref:Microsoft.AspNetCore.Identity.UserManager`1> can store an `ApplicationUser` claim for user data available from the <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>.</span></span>

<span data-ttu-id="e0223-142">Örnek uygulamada `OnPostConfirmationAsync` (*Account/ExternalLogin.cshtml.cs*) oluşturan bir <xref:System.Security.Claims.ClaimTypes.Gender> imzalı için talep içinde `ApplicationUser`:</span><span class="sxs-lookup"><span data-stu-id="e0223-142">In the sample app, `OnPostConfirmationAsync` (*Account/ExternalLogin.cshtml.cs*) establishes a <xref:System.Security.Claims.ClaimTypes.Gender> claim for the signed in `ApplicationUser`:</span></span>

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=30-31)]

### <a name="save-the-access-token"></a><span data-ttu-id="e0223-143">Erişim belirtecini kaydetme</span><span class="sxs-lookup"><span data-stu-id="e0223-143">Save the access token</span></span>

<span data-ttu-id="e0223-144"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> erişim ve yenileme belirteçleri de depolanıp depolanmayacağını tanımlar <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> başarılı yetkilendirme sonrasında.</span><span class="sxs-lookup"><span data-stu-id="e0223-144"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> defines whether access and refresh tokens should be stored in the <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> after a successful authorization.</span></span> <span data-ttu-id="e0223-145">`SaveTokens` ayarlanır `false` son kimlik doğrulama tanımlama bilgisinin boyutunu azaltmak için varsayılan olarak.</span><span class="sxs-lookup"><span data-stu-id="e0223-145">`SaveTokens` is set to `false` by default to reduce the size of the final authentication cookie.</span></span>

<span data-ttu-id="e0223-146">Örnek uygulamayı değerini ayarlar `SaveTokens` için `true` içinde <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:</span><span class="sxs-lookup"><span data-stu-id="e0223-146">The sample app sets the value of `SaveTokens` to `true` in <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:</span></span>

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=9)]

<span data-ttu-id="e0223-147">Zaman `OnPostConfirmationAsync` yürütür, depolama ve erişim belirteci ([ExternalLoginInfo.AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) dış sağlayıcıdan `ApplicationUser`'s `AuthenticationProperties`.</span><span class="sxs-lookup"><span data-stu-id="e0223-147">When `OnPostConfirmationAsync` executes, store the access token ([ExternalLoginInfo.AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) from the external provider in the `ApplicationUser`'s `AuthenticationProperties`.</span></span>

<span data-ttu-id="e0223-148">Örnek uygulama, erişim belirtecinde kaydeder:</span><span class="sxs-lookup"><span data-stu-id="e0223-148">The sample app saves the access token in:</span></span>

* <span data-ttu-id="e0223-149">`OnPostConfirmationAsync` &ndash; Yeni kullanıcı kaydı için yürütür.</span><span class="sxs-lookup"><span data-stu-id="e0223-149">`OnPostConfirmationAsync` &ndash; Executes for new user registration.</span></span>
* <span data-ttu-id="e0223-150">`OnGetCallbackAsync` &ndash; Önceden kaydedilmiş kullanıcı uygulamada oturum açtığında yürütür.</span><span class="sxs-lookup"><span data-stu-id="e0223-150">`OnGetCallbackAsync` &ndash; Executes when a previously registered user signs into the app.</span></span>

<span data-ttu-id="e0223-151">*Account/ExternalLogin.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="e0223-151">*Account/ExternalLogin.cshtml.cs*:</span></span>

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=34-35)]

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnGetCallbackAsync&highlight=31-32)]

### <a name="how-to-add-additional-custom-tokens"></a><span data-ttu-id="e0223-152">Ek özel belirteçler ekleme</span><span class="sxs-lookup"><span data-stu-id="e0223-152">How to add additional custom tokens</span></span>

<span data-ttu-id="e0223-153">Bir parçası olarak depolanan özel bir belirteç ekleme göstermek için `SaveTokens`, örnek uygulamayı ekler bir <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> geçerli <xref:System.DateTime> için bir [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) , `TicketCreated`:</span><span class="sxs-lookup"><span data-stu-id="e0223-153">To demonstrate how to add a custom token, which is stored as part of `SaveTokens`, the sample app adds an <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> with the current <xref:System.DateTime> for an [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) of `TicketCreated`:</span></span>

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=10-21)]

## <a name="sample-app-instructions"></a><span data-ttu-id="e0223-154">Örnek uygulama yönergeleri</span><span class="sxs-lookup"><span data-stu-id="e0223-154">Sample app instructions</span></span>

<span data-ttu-id="e0223-155">Örnek uygulamayı gösterir nasıl yapılır:</span><span class="sxs-lookup"><span data-stu-id="e0223-155">The sample app demonstrates how to:</span></span>

* <span data-ttu-id="e0223-156">Kullanıcının cinsiyeti Google'dan alın ve bir cinsiyet talebi değeri depolar.</span><span class="sxs-lookup"><span data-stu-id="e0223-156">Obtain the user's gender from Google and store a gender claim with the value.</span></span>
* <span data-ttu-id="e0223-157">Google erişim belirtecini kullanıcının Store `AuthenticationProperties`.</span><span class="sxs-lookup"><span data-stu-id="e0223-157">Store the Google access token in the user's `AuthenticationProperties`.</span></span>

<span data-ttu-id="e0223-158">Örnek uygulamayı kullanmak için:</span><span class="sxs-lookup"><span data-stu-id="e0223-158">To use the sample app:</span></span>

1. <span data-ttu-id="e0223-159">Uygulamayı kaydetme ve geçerli istemci Kimliğini ve Google kimlik doğrulaması için istemci parolası alın.</span><span class="sxs-lookup"><span data-stu-id="e0223-159">Register the app and obtain a valid client ID and client secret for Google authentication.</span></span> <span data-ttu-id="e0223-160">Daha fazla bilgi için bkz. <xref:security/authentication/google-logins>.</span><span class="sxs-lookup"><span data-stu-id="e0223-160">For more information, see <xref:security/authentication/google-logins>.</span></span>
1. <span data-ttu-id="e0223-161">İstemci Kimliğini ve istemci gizli anahtarı'nda sağlamak <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions> , `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="e0223-161">Provide the client ID and client secret to the app in the <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions> of `Startup.ConfigureServices`.</span></span>
1. <span data-ttu-id="e0223-162">Uygulamayı çalıştırın ve My talep sayfası isteme.</span><span class="sxs-lookup"><span data-stu-id="e0223-162">Run the app and request the My Claims page.</span></span> <span data-ttu-id="e0223-163">Kullanıcı oturum açmadı, uygulama için Google yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="e0223-163">When the user isn't signed in, the app redirects to Google.</span></span> <span data-ttu-id="e0223-164">Google ile oturum aç.</span><span class="sxs-lookup"><span data-stu-id="e0223-164">Sign in with Google.</span></span> <span data-ttu-id="e0223-165">Google kullanıcı uygulamaya geri yönlendirir (`/Home/MyClaims`).</span><span class="sxs-lookup"><span data-stu-id="e0223-165">Google redirects the user back to the app (`/Home/MyClaims`).</span></span> <span data-ttu-id="e0223-166">Kullanıcının kimliği doğrulanır ve My talep sayfası yüklenir.</span><span class="sxs-lookup"><span data-stu-id="e0223-166">The user is authenticated, and the My Claims page is loaded.</span></span> <span data-ttu-id="e0223-167">Cinsiyet talep altında **kullanıcı taleplerini** Google'dan alınan değer.</span><span class="sxs-lookup"><span data-stu-id="e0223-167">The gender claim is present under **User Claims** with the value obtained from Google.</span></span> <span data-ttu-id="e0223-168">Erişim belirteci görünür **kimlik doğrulaması özelliklerini**.</span><span class="sxs-lookup"><span data-stu-id="e0223-168">The access token appears in the **Authentication Properties**.</span></span>

```
User Claims

http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier
    b36a7b09-9135-4810-b7a5-78697ff23e99
http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name
    username@gmail.com
AspNet.Identity.SecurityStamp
    29G2TB881ATCUQFJSRFG1S0QJ0OOAWVT
http://schemas.xmlsoap.org/ws/2005/05/identity/claims/gender
    female
http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod
    Google

Authentication Properties

.Token.access_token
    bv42.Dgw...GQMv9ArLPs
.Token.token_type
    Bearer
.Token.expires_at
    2018-08-27T19:08:00.0000000+00:00
.Token.TicketCreated
    8/27/2018 6:08:00 PM
.TokenNames
    access_token;token_type;expires_at;TicketCreated
.issued
    Mon, 27 Aug 2018 18:08:05 GMT
.expires
    Mon, 10 Sep 2018 18:08:05 GMT
```
