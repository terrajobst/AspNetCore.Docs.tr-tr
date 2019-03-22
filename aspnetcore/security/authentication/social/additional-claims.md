---
title: Ek talep ve ASP.NET Core, dış sağlayıcılardan gelen belirteçleri kalıcı
author: guardrex
description: Ek talep ve dış sağlayıcılardan gelen belirteçleri oluşturma hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 11/11/2018
uid: security/authentication/social/additional-claims
ms.openlocfilehash: 49c323fab64bd4ea52dd1d8cf2e43a79d4d0d0dc
ms.sourcegitcommit: a1c43150ed46aa01572399e8aede50d4668745ca
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/21/2019
ms.locfileid: "58327358"
---
# <a name="persist-additional-claims-and-tokens-from-external-providers-in-aspnet-core"></a><span data-ttu-id="f43fc-103">Ek talep ve ASP.NET Core, dış sağlayıcılardan gelen belirteçleri kalıcı</span><span class="sxs-lookup"><span data-stu-id="f43fc-103">Persist additional claims and tokens from external providers in ASP.NET Core</span></span>

<span data-ttu-id="f43fc-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="f43fc-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="f43fc-105">ASP.NET Core uygulaması ek talep ve Facebook, Google, Microsoft ve Twitter gibi dış kimlik sağlayıcılardan gelen belirteçleri oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f43fc-105">An ASP.NET Core app can establish additional claims and tokens from external authentication providers, such as Facebook, Google, Microsoft, and Twitter.</span></span> <span data-ttu-id="f43fc-106">Her bir sağlayıcı platformu farklı kullanıcılar hakkında bilgi gösterir, ancak alma ve kullanıcı verilerini ek talepleri dönüştürme için desen aynıdır.</span><span class="sxs-lookup"><span data-stu-id="f43fc-106">Each provider reveals different information about users on its platform, but the pattern for receiving and transforming user data into additional claims is the same.</span></span>

<span data-ttu-id="f43fc-107">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f43fc-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f43fc-108">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="f43fc-108">Prerequisites</span></span>

<span data-ttu-id="f43fc-109">Uygulamayı desteklemek için hangi Dış kimlik doğrulama sağlayıcıları karar verin.</span><span class="sxs-lookup"><span data-stu-id="f43fc-109">Decide which external authentication providers to support in the app.</span></span> <span data-ttu-id="f43fc-110">Tüm sağlayıcılar için uygulamayı kaydetme ve bir istemci kimliği ve istemci gizli anahtarını alın.</span><span class="sxs-lookup"><span data-stu-id="f43fc-110">For each provider, register the app and obtain a client ID and client secret.</span></span> <span data-ttu-id="f43fc-111">Daha fazla bilgi için bkz. <xref:security/authentication/social/index>.</span><span class="sxs-lookup"><span data-stu-id="f43fc-111">For more information, see <xref:security/authentication/social/index>.</span></span> <span data-ttu-id="f43fc-112">[Örnek uygulaması](#sample-app-instructions) kullanan [Google kimlik doğrulama sağlayıcısı](xref:security/authentication/google-logins).</span><span class="sxs-lookup"><span data-stu-id="f43fc-112">The [sample app](#sample-app-instructions) uses the [Google authentication provider](xref:security/authentication/google-logins).</span></span>

## <a name="set-the-client-id-and-client-secret"></a><span data-ttu-id="f43fc-113">İstemci Kimliğini ve istemci gizli dizisi ayarlayın</span><span class="sxs-lookup"><span data-stu-id="f43fc-113">Set the client ID and client secret</span></span>

<span data-ttu-id="f43fc-114">OAuth kimlik doğrulama sağlayıcısı, bir istemci kimliği ve istemci gizli anahtarı kullanarak bir uygulama ile bir güven ilişkisi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="f43fc-114">The OAuth authentication provider establishes a trust relationship with an app using a client ID and client secret.</span></span> <span data-ttu-id="f43fc-115">Uygulamayı sağlayıcıda kaydedildiğinde istemci Kimliğini ve istemci gizli değerleri uygulama için Dış kimlik doğrulama sağlayıcısı tarafından oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="f43fc-115">Client ID and client secret values are created for the app by the external authentication provider when the app is registered with the provider.</span></span> <span data-ttu-id="f43fc-116">Uygulamanın kullandığı her bir dış sağlayıcı bağımsız olarak sağlayıcının istemci Kimliğini ve istemci gizli anahtarı ile yapılandırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="f43fc-116">Each external provider that the app uses must be configured independently with the provider's client ID and client secret.</span></span> <span data-ttu-id="f43fc-117">Daha fazla bilgi için senaryonuz için geçerli olan dış kimlik doğrulama sağlayıcısı konulara bakın:</span><span class="sxs-lookup"><span data-stu-id="f43fc-117">For more information, see the external authentication provider topics that apply to your scenario:</span></span>

* [<span data-ttu-id="f43fc-118">Facebook kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="f43fc-118">Facebook authentication</span></span>](xref:security/authentication/facebook-logins)
* [<span data-ttu-id="f43fc-119">Google kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="f43fc-119">Google authentication</span></span>](xref:security/authentication/google-logins)
* [<span data-ttu-id="f43fc-120">Microsoft kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="f43fc-120">Microsoft authentication</span></span>](xref:security/authentication/microsoft-logins)
* [<span data-ttu-id="f43fc-121">Twitter kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="f43fc-121">Twitter authentication</span></span>](xref:security/authentication/twitter-logins)
* [<span data-ttu-id="f43fc-122">Diğer kimlik doğrulama sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="f43fc-122">Other authentication providers</span></span>](xref:security/authentication/otherlogins)
* [<span data-ttu-id="f43fc-123">OpenIdConnect</span><span class="sxs-lookup"><span data-stu-id="f43fc-123">OpenIdConnect</span></span>](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2)

<span data-ttu-id="f43fc-124">Örnek uygulama, bir istemci kimliği ve istemci gizli anahtarı Google tarafından sağlanan Google kimlik doğrulama sağlayıcısı yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="f43fc-124">The sample app configures the Google authentication provider with a client ID and client secret provided by Google:</span></span>

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=4,6)]

## <a name="establish-the-authentication-scope"></a><span data-ttu-id="f43fc-125">Kimlik doğrulama kapsamı oluşturmak</span><span class="sxs-lookup"><span data-stu-id="f43fc-125">Establish the authentication scope</span></span>

<span data-ttu-id="f43fc-126">Belirterek sağlayıcıdan almak için izinler listesinden belirtin <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>.</span><span class="sxs-lookup"><span data-stu-id="f43fc-126">Specify the list of permissions to retrieve from the provider by specifying the <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>.</span></span> <span data-ttu-id="f43fc-127">Yaygın dış sağlayıcıları için kimlik doğrulaması kapsamları aşağıdaki tabloda görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="f43fc-127">Authentication scopes for common external providers appear in the following table.</span></span>

| <span data-ttu-id="f43fc-128">Sağlayıcı</span><span class="sxs-lookup"><span data-stu-id="f43fc-128">Provider</span></span>  | <span data-ttu-id="f43fc-129">Kapsam</span><span class="sxs-lookup"><span data-stu-id="f43fc-129">Scope</span></span>                                                            |
| --------- | ---------------------------------------------------------------- |
| <span data-ttu-id="f43fc-130">Facebook</span><span class="sxs-lookup"><span data-stu-id="f43fc-130">Facebook</span></span>  | `https://www.facebook.com/dialog/oauth`                          |
| <span data-ttu-id="f43fc-131">Google</span><span class="sxs-lookup"><span data-stu-id="f43fc-131">Google</span></span>    | `https://www.googleapis.com/auth/plus.login`                     |
| <span data-ttu-id="f43fc-132">Microsoft</span><span class="sxs-lookup"><span data-stu-id="f43fc-132">Microsoft</span></span> | `https://login.microsoftonline.com/common/oauth2/v2.0/authorize` |
| <span data-ttu-id="f43fc-133">Twitter</span><span class="sxs-lookup"><span data-stu-id="f43fc-133">Twitter</span></span>   | `https://api.twitter.com/oauth/authenticate`                     |

<span data-ttu-id="f43fc-134">Google örnek uygulamanın eklediği `plus.login` Google + oturum açma izinleri istemek için kapsamı:</span><span class="sxs-lookup"><span data-stu-id="f43fc-134">The sample app adds the Google `plus.login` scope to request Google+ sign in permissions:</span></span>

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=7)]

## <a name="map-user-data-keys-and-create-claims"></a><span data-ttu-id="f43fc-135">Kullanıcı veri anahtarları eşleyin ve talep oluşturma</span><span class="sxs-lookup"><span data-stu-id="f43fc-135">Map user data keys and create claims</span></span>

<span data-ttu-id="f43fc-136">Sağlayıcının seçeneklerinde belirtin bir <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> her anahtarın dış sağlayıcının JSON kullanıcı verileri için oturum açma okumak uygulama kimliği.</span><span class="sxs-lookup"><span data-stu-id="f43fc-136">In the provider's options, specify a <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> for each key in the external provider's JSON user data for the app identity to read on sign in.</span></span> <span data-ttu-id="f43fc-137">Talep türleri hakkında daha fazla bilgi için bkz. <xref:System.Security.Claims.ClaimTypes>.</span><span class="sxs-lookup"><span data-stu-id="f43fc-137">For more information on claim types, see <xref:System.Security.Claims.ClaimTypes>.</span></span>

<span data-ttu-id="f43fc-138">Örnek uygulamayı oluşturur bir <xref:System.Security.Claims.ClaimTypes.Gender> gelen talep `gender` Google kullanıcı verileri anahtar:</span><span class="sxs-lookup"><span data-stu-id="f43fc-138">The sample app creates a <xref:System.Security.Claims.ClaimTypes.Gender> claim from the `gender` key in Google user data:</span></span>

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=8)]

<span data-ttu-id="f43fc-139">İçinde <xref:Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync*>e <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) ile uygulama oturum açmış <xref:Microsoft.AspNetCore.Identity.SignInManager%601.SignInAsync*>.</span><span class="sxs-lookup"><span data-stu-id="f43fc-139">In <xref:Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync*>, an <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) is signed into the app with <xref:Microsoft.AspNetCore.Identity.SignInManager%601.SignInAsync*>.</span></span> <span data-ttu-id="f43fc-140">Oturum açma işlemi sırasında <xref:Microsoft.AspNetCore.Identity.UserManager%601> depolayabilirsiniz bir `ApplicationUser` bulunan kullanıcı verileri için talep <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>.</span><span class="sxs-lookup"><span data-stu-id="f43fc-140">During the sign in process, the <xref:Microsoft.AspNetCore.Identity.UserManager%601> can store an `ApplicationUser` claim for user data available from the <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>.</span></span>

<span data-ttu-id="f43fc-141">Örnek uygulamada `OnPostConfirmationAsync` (*Account/ExternalLogin.cshtml.cs*) oluşturan bir <xref:System.Security.Claims.ClaimTypes.Gender> imzalı için talep içinde `ApplicationUser`:</span><span class="sxs-lookup"><span data-stu-id="f43fc-141">In the sample app, `OnPostConfirmationAsync` (*Account/ExternalLogin.cshtml.cs*) establishes a <xref:System.Security.Claims.ClaimTypes.Gender> claim for the signed in `ApplicationUser`:</span></span>

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=30-31)]

## <a name="save-the-access-token"></a><span data-ttu-id="f43fc-142">Erişim belirtecini kaydetme</span><span class="sxs-lookup"><span data-stu-id="f43fc-142">Save the access token</span></span>

<span data-ttu-id="f43fc-143"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> erişim ve yenileme belirteçleri de depolanıp depolanmayacağını tanımlar <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> başarılı yetkilendirme sonrasında.</span><span class="sxs-lookup"><span data-stu-id="f43fc-143"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> defines whether access and refresh tokens should be stored in the <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> after a successful authorization.</span></span> <span data-ttu-id="f43fc-144">`SaveTokens` ayarlanır `false` son kimlik doğrulama tanımlama bilgisinin boyutunu azaltmak için varsayılan olarak.</span><span class="sxs-lookup"><span data-stu-id="f43fc-144">`SaveTokens` is set to `false` by default to reduce the size of the final authentication cookie.</span></span>

<span data-ttu-id="f43fc-145">Örnek uygulamayı değerini ayarlar `SaveTokens` için `true` içinde <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:</span><span class="sxs-lookup"><span data-stu-id="f43fc-145">The sample app sets the value of `SaveTokens` to `true` in <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:</span></span>

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=9)]

<span data-ttu-id="f43fc-146">Zaman `OnPostConfirmationAsync` yürütür, depolama ve erişim belirteci ([ExternalLoginInfo.AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) dış sağlayıcıdan `ApplicationUser`'s `AuthenticationProperties`.</span><span class="sxs-lookup"><span data-stu-id="f43fc-146">When `OnPostConfirmationAsync` executes, store the access token ([ExternalLoginInfo.AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) from the external provider in the `ApplicationUser`'s `AuthenticationProperties`.</span></span>

<span data-ttu-id="f43fc-147">Örnek uygulama, erişim belirtecinde kaydeder:</span><span class="sxs-lookup"><span data-stu-id="f43fc-147">The sample app saves the access token in:</span></span>

* <span data-ttu-id="f43fc-148">`OnPostConfirmationAsync` &ndash; Yeni kullanıcı kaydı için yürütür.</span><span class="sxs-lookup"><span data-stu-id="f43fc-148">`OnPostConfirmationAsync` &ndash; Executes for new user registration.</span></span>
* <span data-ttu-id="f43fc-149">`OnGetCallbackAsync` &ndash; Önceden kaydedilmiş kullanıcı uygulamada oturum açtığında yürütür.</span><span class="sxs-lookup"><span data-stu-id="f43fc-149">`OnGetCallbackAsync` &ndash; Executes when a previously registered user signs into the app.</span></span>

<span data-ttu-id="f43fc-150">*Account/ExternalLogin.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="f43fc-150">*Account/ExternalLogin.cshtml.cs*:</span></span>

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=34-35)]

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnGetCallbackAsync&highlight=31-32)]

## <a name="how-to-add-additional-custom-tokens"></a><span data-ttu-id="f43fc-151">Ek özel belirteçler ekleme</span><span class="sxs-lookup"><span data-stu-id="f43fc-151">How to add additional custom tokens</span></span>

<span data-ttu-id="f43fc-152">Bir parçası olarak depolanan özel bir belirteç ekleme göstermek için `SaveTokens`, örnek uygulamayı ekler bir <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> geçerli <xref:System.DateTime> için bir [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) , `TicketCreated`:</span><span class="sxs-lookup"><span data-stu-id="f43fc-152">To demonstrate how to add a custom token, which is stored as part of `SaveTokens`, the sample app adds an <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> with the current <xref:System.DateTime> for an [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) of `TicketCreated`:</span></span>

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=10-21)]

## <a name="sample-app-instructions"></a><span data-ttu-id="f43fc-153">Örnek uygulama yönergeleri</span><span class="sxs-lookup"><span data-stu-id="f43fc-153">Sample app instructions</span></span>

<span data-ttu-id="f43fc-154">Örnek uygulamayı gösterir nasıl yapılır:</span><span class="sxs-lookup"><span data-stu-id="f43fc-154">The sample app demonstrates how to:</span></span>

* <span data-ttu-id="f43fc-155">Kullanıcının cinsiyeti Google'dan alın ve bir cinsiyet talebi değeri depolar.</span><span class="sxs-lookup"><span data-stu-id="f43fc-155">Obtain the user's gender from Google and store a gender claim with the value.</span></span>
* <span data-ttu-id="f43fc-156">Google erişim belirtecini kullanıcının Store `AuthenticationProperties`.</span><span class="sxs-lookup"><span data-stu-id="f43fc-156">Store the Google access token in the user's `AuthenticationProperties`.</span></span>

<span data-ttu-id="f43fc-157">Örnek uygulamayı kullanmak için:</span><span class="sxs-lookup"><span data-stu-id="f43fc-157">To use the sample app:</span></span>

1. <span data-ttu-id="f43fc-158">Uygulamayı kaydetme ve geçerli istemci Kimliğini ve Google kimlik doğrulaması için istemci parolası alın.</span><span class="sxs-lookup"><span data-stu-id="f43fc-158">Register the app and obtain a valid client ID and client secret for Google authentication.</span></span> <span data-ttu-id="f43fc-159">Daha fazla bilgi için bkz. <xref:security/authentication/google-logins>.</span><span class="sxs-lookup"><span data-stu-id="f43fc-159">For more information, see <xref:security/authentication/google-logins>.</span></span>
1. <span data-ttu-id="f43fc-160">İstemci Kimliğini ve istemci gizli anahtarı'nda sağlamak <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions> , `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="f43fc-160">Provide the client ID and client secret to the app in the <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions> of `Startup.ConfigureServices`.</span></span>
1. <span data-ttu-id="f43fc-161">Uygulamayı çalıştırın ve My talep sayfası isteme.</span><span class="sxs-lookup"><span data-stu-id="f43fc-161">Run the app and request the My Claims page.</span></span> <span data-ttu-id="f43fc-162">Kullanıcı oturum açmadı, uygulama için Google yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="f43fc-162">When the user isn't signed in, the app redirects to Google.</span></span> <span data-ttu-id="f43fc-163">Google ile oturum aç.</span><span class="sxs-lookup"><span data-stu-id="f43fc-163">Sign in with Google.</span></span> <span data-ttu-id="f43fc-164">Google kullanıcı uygulamaya geri yönlendirir (`/Home/MyClaims`).</span><span class="sxs-lookup"><span data-stu-id="f43fc-164">Google redirects the user back to the app (`/Home/MyClaims`).</span></span> <span data-ttu-id="f43fc-165">Kullanıcının kimliği doğrulanır ve My talep sayfası yüklenir.</span><span class="sxs-lookup"><span data-stu-id="f43fc-165">The user is authenticated, and the My Claims page is loaded.</span></span> <span data-ttu-id="f43fc-166">Cinsiyet talep altında **kullanıcı taleplerini** Google'dan alınan değer.</span><span class="sxs-lookup"><span data-stu-id="f43fc-166">The gender claim is present under **User Claims** with the value obtained from Google.</span></span> <span data-ttu-id="f43fc-167">Erişim belirteci görünür **kimlik doğrulaması özelliklerini**.</span><span class="sxs-lookup"><span data-stu-id="f43fc-167">The access token appears in the **Authentication Properties**.</span></span>

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

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]
