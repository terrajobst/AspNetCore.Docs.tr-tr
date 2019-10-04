---
title: ASP.NET Core dış sağlayıcılardan ek talepler ve belirteçler kalıcı hale getirme
author: guardrex
description: Dış sağlayıcılardan ek talepler ve belirteçler oluşturmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/01/2019
uid: security/authentication/social/additional-claims
ms.openlocfilehash: cdf263df8d1aa17ea3820a16ecbd10abce9d683d
ms.sourcegitcommit: 73e255e846e414821b8cc20ffa3aec946735cd4e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2019
ms.locfileid: "71925150"
---
# <a name="persist-additional-claims-and-tokens-from-external-providers-in-aspnet-core"></a><span data-ttu-id="605d4-103">ASP.NET Core dış sağlayıcılardan ek talepler ve belirteçler kalıcı hale getirme</span><span class="sxs-lookup"><span data-stu-id="605d4-103">Persist additional claims and tokens from external providers in ASP.NET Core</span></span>

<span data-ttu-id="605d4-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="605d4-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="605d4-105">ASP.NET Core bir uygulama, Facebook, Google, Microsoft ve Twitter gibi dış kimlik doğrulama sağlayıcılarından ek talepler ve belirteçler oluşturabilir.</span><span class="sxs-lookup"><span data-stu-id="605d4-105">An ASP.NET Core app can establish additional claims and tokens from external authentication providers, such as Facebook, Google, Microsoft, and Twitter.</span></span> <span data-ttu-id="605d4-106">Her sağlayıcı, platformdaki kullanıcılar hakkında farklı bilgiler ortaya koyar, ancak kullanıcı verilerini alma ve ek taleplere dönüştürme için olan model aynı olur.</span><span class="sxs-lookup"><span data-stu-id="605d4-106">Each provider reveals different information about users on its platform, but the pattern for receiving and transforming user data into additional claims is the same.</span></span>

<span data-ttu-id="605d4-107">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="605d4-107">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="605d4-108">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="605d4-108">Prerequisites</span></span>

<span data-ttu-id="605d4-109">Uygulamada hangi dış kimlik doğrulama sağlayıcılarının destekileceğine karar verin.</span><span class="sxs-lookup"><span data-stu-id="605d4-109">Decide which external authentication providers to support in the app.</span></span> <span data-ttu-id="605d4-110">Her sağlayıcı için, uygulamayı kaydedin ve bir istemci KIMLIĞI ve istemci parolası alın.</span><span class="sxs-lookup"><span data-stu-id="605d4-110">For each provider, register the app and obtain a client ID and client secret.</span></span> <span data-ttu-id="605d4-111">Daha fazla bilgi için bkz. <xref:security/authentication/social/index>.</span><span class="sxs-lookup"><span data-stu-id="605d4-111">For more information, see <xref:security/authentication/social/index>.</span></span> <span data-ttu-id="605d4-112">Örnek uygulama [Google kimlik doğrulama sağlayıcısını](xref:security/authentication/google-logins)kullanır.</span><span class="sxs-lookup"><span data-stu-id="605d4-112">The sample app uses the [Google authentication provider](xref:security/authentication/google-logins).</span></span>

## <a name="set-the-client-id-and-client-secret"></a><span data-ttu-id="605d4-113">İstemci KIMLIĞINI ve gizli anahtarı ayarlama</span><span class="sxs-lookup"><span data-stu-id="605d4-113">Set the client ID and client secret</span></span>

<span data-ttu-id="605d4-114">OAuth kimlik doğrulama sağlayıcısı, istemci KIMLIĞI ve istemci parolası kullanarak bir uygulamayla güven ilişkisi kurar.</span><span class="sxs-lookup"><span data-stu-id="605d4-114">The OAuth authentication provider establishes a trust relationship with an app using a client ID and client secret.</span></span> <span data-ttu-id="605d4-115">Uygulama sağlayıcıya kaydedildiğinde dış kimlik doğrulama sağlayıcısı tarafından uygulama için istemci KIMLIĞI ve istemci gizli anahtarı değerleri oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="605d4-115">Client ID and client secret values are created for the app by the external authentication provider when the app is registered with the provider.</span></span> <span data-ttu-id="605d4-116">Uygulamanın kullandığı her bir dış sağlayıcı, sağlayıcının istemci KIMLIĞI ve istemci parolası ile bağımsız olarak yapılandırılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="605d4-116">Each external provider that the app uses must be configured independently with the provider's client ID and client secret.</span></span> <span data-ttu-id="605d4-117">Daha fazla bilgi için, senaryonuza uygulanan dış kimlik doğrulama sağlayıcısı konularına bakın:</span><span class="sxs-lookup"><span data-stu-id="605d4-117">For more information, see the external authentication provider topics that apply to your scenario:</span></span>

* [<span data-ttu-id="605d4-118">Facebook kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="605d4-118">Facebook authentication</span></span>](xref:security/authentication/facebook-logins)
* [<span data-ttu-id="605d4-119">Google kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="605d4-119">Google authentication</span></span>](xref:security/authentication/google-logins)
* [<span data-ttu-id="605d4-120">Microsoft kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="605d4-120">Microsoft authentication</span></span>](xref:security/authentication/microsoft-logins)
* [<span data-ttu-id="605d4-121">Twitter kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="605d4-121">Twitter authentication</span></span>](xref:security/authentication/twitter-logins)
* [<span data-ttu-id="605d4-122">Diğer kimlik doğrulama sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="605d4-122">Other authentication providers</span></span>](xref:security/authentication/otherlogins)
* [<span data-ttu-id="605d4-123">Openıdconnect</span><span class="sxs-lookup"><span data-stu-id="605d4-123">OpenIdConnect</span></span>](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2)

<span data-ttu-id="605d4-124">Örnek uygulama Google kimlik doğrulama sağlayıcısını Google tarafından sağlanmış istemci KIMLIĞI ve istemci gizli anahtarı ile yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="605d4-124">The sample app configures the Google authentication provider with a client ID and client secret provided by Google:</span></span>

[!code-csharp[](additional-claims/samples/3.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=4,9)]

## <a name="establish-the-authentication-scope"></a><span data-ttu-id="605d4-125">Kimlik doğrulama kapsamını oluşturma</span><span class="sxs-lookup"><span data-stu-id="605d4-125">Establish the authentication scope</span></span>

<span data-ttu-id="605d4-126">@No__t-0 belirterek sağlayıcıdan alma izinlerinin listesini belirtin.</span><span class="sxs-lookup"><span data-stu-id="605d4-126">Specify the list of permissions to retrieve from the provider by specifying the <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>.</span></span> <span data-ttu-id="605d4-127">Ortak dış sağlayıcılar için kimlik doğrulama kapsamları aşağıdaki tabloda görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="605d4-127">Authentication scopes for common external providers appear in the following table.</span></span>

| <span data-ttu-id="605d4-128">Sağlayıcı</span><span class="sxs-lookup"><span data-stu-id="605d4-128">Provider</span></span>  | <span data-ttu-id="605d4-129">`Scope`</span><span class="sxs-lookup"><span data-stu-id="605d4-129">Scope</span></span>                                                            |
| --------- | ---------------------------------------------------------------- |
| <span data-ttu-id="605d4-130">Facebook</span><span class="sxs-lookup"><span data-stu-id="605d4-130">Facebook</span></span>  | `https://www.facebook.com/dialog/oauth`                          |
| <span data-ttu-id="605d4-131">Google</span><span class="sxs-lookup"><span data-stu-id="605d4-131">Google</span></span>    | `https://www.googleapis.com/auth/userinfo.profile`               |
| <span data-ttu-id="605d4-132">Microsoft</span><span class="sxs-lookup"><span data-stu-id="605d4-132">Microsoft</span></span> | `https://login.microsoftonline.com/common/oauth2/v2.0/authorize` |
| <span data-ttu-id="605d4-133">Twitter</span><span class="sxs-lookup"><span data-stu-id="605d4-133">Twitter</span></span>   | `https://api.twitter.com/oauth/authenticate`                     |

<span data-ttu-id="605d4-134">Örnek uygulamada, Google 'ın `userinfo.profile` kapsamı, <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder> ' de <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> çağrıldığında Framework tarafından otomatik olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="605d4-134">In the sample app, Google's `userinfo.profile` scope is automatically added by the framework when <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> is called on the <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder>.</span></span> <span data-ttu-id="605d4-135">Uygulama ek kapsamlar gerektiriyorsa, bunları seçeneklere ekleyin.</span><span class="sxs-lookup"><span data-stu-id="605d4-135">If the app requires additional scopes, add them to the options.</span></span> <span data-ttu-id="605d4-136">Aşağıdaki örnekte, bir kullanıcının Doğum gününü almak için Google `https://www.googleapis.com/auth/user.birthday.read` kapsamı eklenir:</span><span class="sxs-lookup"><span data-stu-id="605d4-136">In the following example, the Google `https://www.googleapis.com/auth/user.birthday.read` scope is added in order to retrieve a user's birthday:</span></span>

```csharp
options.Scope.Add("https://www.googleapis.com/auth/user.birthday.read");
```

## <a name="map-user-data-keys-and-create-claims"></a><span data-ttu-id="605d4-137">Kullanıcı veri anahtarlarını eşleme ve talepler oluşturma</span><span class="sxs-lookup"><span data-stu-id="605d4-137">Map user data keys and create claims</span></span>

<span data-ttu-id="605d4-138">Sağlayıcının seçeneklerinde, oturum açma sırasında okunacak uygulama kimliği için dış sağlayıcının JSON Kullanıcı verilerinde her anahtar/alt anahtar için bir <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> veya <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonSubKey*> belirtin.</span><span class="sxs-lookup"><span data-stu-id="605d4-138">In the provider's options, specify a <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> or <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonSubKey*> for each key/subkey in the external provider's JSON user data for the app identity to read on sign in.</span></span> <span data-ttu-id="605d4-139">Talep türleri hakkında daha fazla bilgi için bkz. <xref:System.Security.Claims.ClaimTypes>.</span><span class="sxs-lookup"><span data-stu-id="605d4-139">For more information on claim types, see <xref:System.Security.Claims.ClaimTypes>.</span></span>

<span data-ttu-id="605d4-140">Örnek uygulama, Google user Data 'daki `locale` ve `picture` anahtarlarından yerel ayar (`urn:google:locale`) ve resim (`urn:google:picture`) talepleri oluşturur:</span><span class="sxs-lookup"><span data-stu-id="605d4-140">The sample app creates locale (`urn:google:locale`) and picture (`urn:google:picture`) claims from the `locale` and `picture` keys in Google user data:</span></span>

[!code-csharp[](additional-claims/samples/3.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=13-14)]

<span data-ttu-id="605d4-141">@No__t-0 ' da bir <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`), <xref:Microsoft.AspNetCore.Identity.SignInManager%601.SignInAsync*> ile uygulamada oturum açtı.</span><span class="sxs-lookup"><span data-stu-id="605d4-141">In <xref:Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync*>, an <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) is signed into the app with <xref:Microsoft.AspNetCore.Identity.SignInManager%601.SignInAsync*>.</span></span> <span data-ttu-id="605d4-142">@No__t-0, oturum açma işlemi sırasında <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*> ' den kullanılabilir Kullanıcı verileri için `ApplicationUser` taleplerini saklayabilir.</span><span class="sxs-lookup"><span data-stu-id="605d4-142">During the sign in process, the <xref:Microsoft.AspNetCore.Identity.UserManager%601> can store an `ApplicationUser` claims for user data available from the <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>.</span></span>

<span data-ttu-id="605d4-143">Örnek uygulamada `OnPostConfirmationAsync` (*Account/ExternalLogin. cshtml. cs*), <xref:System.Security.Claims.ClaimTypes.GivenName> için bir talep dahil, oturum açan `ApplicationUser` için yerel ayar (`urn:google:locale`) ve resim (`urn:google:picture`) taleplerini belirler:</span><span class="sxs-lookup"><span data-stu-id="605d4-143">In the sample app, `OnPostConfirmationAsync` (*Account/ExternalLogin.cshtml.cs*) establishes the locale (`urn:google:locale`) and picture (`urn:google:picture`) claims for the signed in `ApplicationUser`, including a claim for <xref:System.Security.Claims.ClaimTypes.GivenName>:</span></span>

[!code-csharp[](additional-claims/samples/3.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=35-51)]

<span data-ttu-id="605d4-144">Varsayılan olarak, bir kullanıcının talepleri kimlik doğrulama tanımlama bilgisinde depolanır.</span><span class="sxs-lookup"><span data-stu-id="605d4-144">By default, a user's claims are stored in the authentication cookie.</span></span> <span data-ttu-id="605d4-145">Kimlik doğrulama tanımlama bilgisi çok büyükse, bunun nedeni uygulamanın başarısız olmasına neden olabilir:</span><span class="sxs-lookup"><span data-stu-id="605d4-145">If the authentication cookie is too large, it can cause the app to fail because:</span></span>

* <span data-ttu-id="605d4-146">Tarayıcı, tanımlama bilgisi üstbilgisinin çok uzun olduğunu algılar.</span><span class="sxs-lookup"><span data-stu-id="605d4-146">The browser detects that the cookie header is too long.</span></span>
* <span data-ttu-id="605d4-147">İsteğin genel boyutu çok büyük.</span><span class="sxs-lookup"><span data-stu-id="605d4-147">The overall size of the request is too large.</span></span>

<span data-ttu-id="605d4-148">Kullanıcı isteklerini işlemek için büyük miktarda Kullanıcı verisi gerekliyse:</span><span class="sxs-lookup"><span data-stu-id="605d4-148">If a large amount of user data is required for processing user requests:</span></span>

* <span data-ttu-id="605d4-149">İstek işleme için Kullanıcı taleplerinin sayısını ve boyutunu yalnızca uygulamanın gerektirdiği şekilde sınırlayın.</span><span class="sxs-lookup"><span data-stu-id="605d4-149">Limit the number and size of user claims for request processing to only what the app requires.</span></span>
* <span data-ttu-id="605d4-150">Kimlik bilgisi kimlik doğrulaması @no__t ara yazılımı için bir özel <xref:Microsoft.AspNetCore.Authentication.Cookies.ITicketStore> kullanın.</span><span class="sxs-lookup"><span data-stu-id="605d4-150">Use a custom <xref:Microsoft.AspNetCore.Authentication.Cookies.ITicketStore> for the Cookie Authentication Middleware's <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.SessionStore> to store identity across requests.</span></span> <span data-ttu-id="605d4-151">Yalnızca istemciye küçük bir oturum tanımlayıcı anahtarı gönderilirken sunucuda büyük miktarlarda kimlik bilgilerini koruyun.</span><span class="sxs-lookup"><span data-stu-id="605d4-151">Preserve large quantities of identity information on the server while only sending a small session identifier key to the client.</span></span>

## <a name="save-the-access-token"></a><span data-ttu-id="605d4-152">Erişim belirtecini Kaydet</span><span class="sxs-lookup"><span data-stu-id="605d4-152">Save the access token</span></span>

<span data-ttu-id="605d4-153"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> başarılı bir yetkilendirmeden sonra erişim ve yenileme belirteçlerinin <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> ' de depolanması gerekip gerekmediğini tanımlar.</span><span class="sxs-lookup"><span data-stu-id="605d4-153"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> defines whether access and refresh tokens should be stored in the <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> after a successful authorization.</span></span> <span data-ttu-id="605d4-154">`SaveTokens`, son kimlik doğrulama tanımlama bilgisinin boyutunu azaltmak için varsayılan olarak `false` olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="605d4-154">`SaveTokens` is set to `false` by default to reduce the size of the final authentication cookie.</span></span>

<span data-ttu-id="605d4-155">Örnek uygulama, <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions> ' de `SaveTokens` değerini `true` olarak ayarlar:</span><span class="sxs-lookup"><span data-stu-id="605d4-155">The sample app sets the value of `SaveTokens` to `true` in <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:</span></span>

[!code-csharp[](additional-claims/samples/3.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=15)]

<span data-ttu-id="605d4-156">@No__t-0 yürütüldüğünde, erişim belirtecini ([Externalloginınfo. AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)), `ApplicationUser` ' nin `AuthenticationProperties` ' teki dış sağlayıcıdan depolayın.</span><span class="sxs-lookup"><span data-stu-id="605d4-156">When `OnPostConfirmationAsync` executes, store the access token ([ExternalLoginInfo.AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) from the external provider in the `ApplicationUser`'s `AuthenticationProperties`.</span></span>

<span data-ttu-id="605d4-157">Örnek uygulama, erişim belirtecini `OnPostConfirmationAsync` ' a (Yeni Kullanıcı kaydı) ve `OnGetCallbackAsync` ' i (önceden kaydedilmiş Kullanıcı) *Account/ExternalLogin. cshtml. cs*içinde kaydeder:</span><span class="sxs-lookup"><span data-stu-id="605d4-157">The sample app saves the access token in `OnPostConfirmationAsync` (new user registration) and `OnGetCallbackAsync` (previously registered user) in *Account/ExternalLogin.cshtml.cs*:</span></span>

[!code-csharp[](additional-claims/samples/3.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=54-56)]

## <a name="how-to-add-additional-custom-tokens"></a><span data-ttu-id="605d4-158">Ek özel belirteçler ekleme</span><span class="sxs-lookup"><span data-stu-id="605d4-158">How to add additional custom tokens</span></span>

<span data-ttu-id="605d4-159">@No__t-0 ' ın bir parçası olarak depolanan özel bir belirtecin nasıl ekleneceğini göstermek için, örnek uygulama, `TicketCreated` ' [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) için geçerli <xref:System.DateTime> ' ye sahip <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> ekler:</span><span class="sxs-lookup"><span data-stu-id="605d4-159">To demonstrate how to add a custom token, which is stored as part of `SaveTokens`, the sample app adds an <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> with the current <xref:System.DateTime> for an [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) of `TicketCreated`:</span></span>

[!code-csharp[](additional-claims/samples/3.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=17-30)]

## <a name="creating-and-adding-claims"></a><span data-ttu-id="605d4-160">Talepler oluşturma ve ekleme</span><span class="sxs-lookup"><span data-stu-id="605d4-160">Creating and adding claims</span></span>

<span data-ttu-id="605d4-161">Framework, koleksiyona talepler oluşturmak ve eklemek için ortak eylemler ve genişletme yöntemleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="605d4-161">The framework provides common actions and extension methods for creating and adding claims to the collection.</span></span> <span data-ttu-id="605d4-162">Daha fazla bilgi için <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions> ve <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionUniqueExtensions> ' a bakın.</span><span class="sxs-lookup"><span data-stu-id="605d4-162">For more information, see the <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions> and <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionUniqueExtensions>.</span></span>

<span data-ttu-id="605d4-163">Kullanıcılar, <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction> ' dan türeterek ve soyut <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.Run*> metodunu uygulayarak özel eylemleri tanımlayabilir.</span><span class="sxs-lookup"><span data-stu-id="605d4-163">Users can define custom actions by deriving from <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction> and implementing the abstract <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.Run*> method.</span></span>

<span data-ttu-id="605d4-164">Daha fazla bilgi için bkz. <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims>.</span><span class="sxs-lookup"><span data-stu-id="605d4-164">For more information, see <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims>.</span></span>

## <a name="removal-of-claim-actions-and-claims"></a><span data-ttu-id="605d4-165">Talep eylemlerinin ve taleplerin kaldırılması</span><span class="sxs-lookup"><span data-stu-id="605d4-165">Removal of claim actions and claims</span></span>

<span data-ttu-id="605d4-166">[Claimactioncollection. Remove (dize)](xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimActionCollection.Remove*) , koleksiyonda verilen <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> için tüm talep eylemlerini kaldırır.</span><span class="sxs-lookup"><span data-stu-id="605d4-166">[ClaimActionCollection.Remove(String)](xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimActionCollection.Remove*) removes all claim actions for the given <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> from the collection.</span></span> <span data-ttu-id="605d4-167">[Claimactioncollectionmapextensions. DeleteClaim (ClaimActionCollection, String)](xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*) , kimliğin verilen <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> talebini siler.</span><span class="sxs-lookup"><span data-stu-id="605d4-167">[ClaimActionCollectionMapExtensions.DeleteClaim(ClaimActionCollection, String)](xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*) deletes a claim of the given <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> from the identity.</span></span> <span data-ttu-id="605d4-168"><xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*> birincil olarak, protokol tarafından oluşturulan talepleri kaldırmak için [OpenID Connect (OıDC)](/azure/active-directory/develop/v2-protocols-oidc) ile birlikte kullanılır.</span><span class="sxs-lookup"><span data-stu-id="605d4-168"><xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*> is primarily used with [OpenID Connect (OIDC)](/azure/active-directory/develop/v2-protocols-oidc) to remove protocol-generated claims.</span></span>

## <a name="sample-app-output"></a><span data-ttu-id="605d4-169">Örnek uygulama çıkışı</span><span class="sxs-lookup"><span data-stu-id="605d4-169">Sample app output</span></span>

```
User Claims

http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier
    9b342344f-7aab-43c2-1ac1-ba75912ca999
http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name
    someone@gmail.com
AspNet.Identity.SecurityStamp
    7D4312MOWRYYBFI1KXRPHGOSTBVWSFDE
http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname
    Judy
urn:google:locale
    en
urn:google:picture
    https://lh4.googleusercontent.com/-XXXXXX/XXXXXX/XXXXXX/XXXXXX/photo.jpg

Authentication Properties

.Token.access_token
    yc23.AlvoZqz56...1lxltXV7D-ZWP9
.Token.token_type
    Bearer
.Token.expires_at
    2019-04-11T22:14:51.0000000+00:00
.Token.TicketCreated
    4/11/2019 9:14:52 PM
.TokenNames
    access_token;token_type;expires_at;TicketCreated
.persistent
.issued
    Thu, 11 Apr 2019 20:51:06 GMT
.expires
    Thu, 25 Apr 2019 20:51:06 GMT

```

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="605d4-170">ASP.NET Core bir uygulama, Facebook, Google, Microsoft ve Twitter gibi dış kimlik doğrulama sağlayıcılarından ek talepler ve belirteçler oluşturabilir.</span><span class="sxs-lookup"><span data-stu-id="605d4-170">An ASP.NET Core app can establish additional claims and tokens from external authentication providers, such as Facebook, Google, Microsoft, and Twitter.</span></span> <span data-ttu-id="605d4-171">Her sağlayıcı, platformdaki kullanıcılar hakkında farklı bilgiler ortaya koyar, ancak kullanıcı verilerini alma ve ek taleplere dönüştürme için olan model aynı olur.</span><span class="sxs-lookup"><span data-stu-id="605d4-171">Each provider reveals different information about users on its platform, but the pattern for receiving and transforming user data into additional claims is the same.</span></span>

<span data-ttu-id="605d4-172">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="605d4-172">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="605d4-173">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="605d4-173">Prerequisites</span></span>

<span data-ttu-id="605d4-174">Uygulamada hangi dış kimlik doğrulama sağlayıcılarının destekileceğine karar verin.</span><span class="sxs-lookup"><span data-stu-id="605d4-174">Decide which external authentication providers to support in the app.</span></span> <span data-ttu-id="605d4-175">Her sağlayıcı için, uygulamayı kaydedin ve bir istemci KIMLIĞI ve istemci parolası alın.</span><span class="sxs-lookup"><span data-stu-id="605d4-175">For each provider, register the app and obtain a client ID and client secret.</span></span> <span data-ttu-id="605d4-176">Daha fazla bilgi için bkz. <xref:security/authentication/social/index>.</span><span class="sxs-lookup"><span data-stu-id="605d4-176">For more information, see <xref:security/authentication/social/index>.</span></span> <span data-ttu-id="605d4-177">Örnek uygulama [Google kimlik doğrulama sağlayıcısını](xref:security/authentication/google-logins)kullanır.</span><span class="sxs-lookup"><span data-stu-id="605d4-177">The sample app uses the [Google authentication provider](xref:security/authentication/google-logins).</span></span>

## <a name="set-the-client-id-and-client-secret"></a><span data-ttu-id="605d4-178">İstemci KIMLIĞINI ve gizli anahtarı ayarlama</span><span class="sxs-lookup"><span data-stu-id="605d4-178">Set the client ID and client secret</span></span>

<span data-ttu-id="605d4-179">OAuth kimlik doğrulama sağlayıcısı, istemci KIMLIĞI ve istemci parolası kullanarak bir uygulamayla güven ilişkisi kurar.</span><span class="sxs-lookup"><span data-stu-id="605d4-179">The OAuth authentication provider establishes a trust relationship with an app using a client ID and client secret.</span></span> <span data-ttu-id="605d4-180">Uygulama sağlayıcıya kaydedildiğinde dış kimlik doğrulama sağlayıcısı tarafından uygulama için istemci KIMLIĞI ve istemci gizli anahtarı değerleri oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="605d4-180">Client ID and client secret values are created for the app by the external authentication provider when the app is registered with the provider.</span></span> <span data-ttu-id="605d4-181">Uygulamanın kullandığı her bir dış sağlayıcı, sağlayıcının istemci KIMLIĞI ve istemci parolası ile bağımsız olarak yapılandırılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="605d4-181">Each external provider that the app uses must be configured independently with the provider's client ID and client secret.</span></span> <span data-ttu-id="605d4-182">Daha fazla bilgi için, senaryonuza uygulanan dış kimlik doğrulama sağlayıcısı konularına bakın:</span><span class="sxs-lookup"><span data-stu-id="605d4-182">For more information, see the external authentication provider topics that apply to your scenario:</span></span>

* [<span data-ttu-id="605d4-183">Facebook kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="605d4-183">Facebook authentication</span></span>](xref:security/authentication/facebook-logins)
* [<span data-ttu-id="605d4-184">Google kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="605d4-184">Google authentication</span></span>](xref:security/authentication/google-logins)
* [<span data-ttu-id="605d4-185">Microsoft kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="605d4-185">Microsoft authentication</span></span>](xref:security/authentication/microsoft-logins)
* [<span data-ttu-id="605d4-186">Twitter kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="605d4-186">Twitter authentication</span></span>](xref:security/authentication/twitter-logins)
* [<span data-ttu-id="605d4-187">Diğer kimlik doğrulama sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="605d4-187">Other authentication providers</span></span>](xref:security/authentication/otherlogins)
* [<span data-ttu-id="605d4-188">Openıdconnect</span><span class="sxs-lookup"><span data-stu-id="605d4-188">OpenIdConnect</span></span>](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2)

<span data-ttu-id="605d4-189">Örnek uygulama Google kimlik doğrulama sağlayıcısını Google tarafından sağlanmış istemci KIMLIĞI ve istemci gizli anahtarı ile yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="605d4-189">The sample app configures the Google authentication provider with a client ID and client secret provided by Google:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=4,9)]

## <a name="establish-the-authentication-scope"></a><span data-ttu-id="605d4-190">Kimlik doğrulama kapsamını oluşturma</span><span class="sxs-lookup"><span data-stu-id="605d4-190">Establish the authentication scope</span></span>

<span data-ttu-id="605d4-191">@No__t-0 belirterek sağlayıcıdan alma izinlerinin listesini belirtin.</span><span class="sxs-lookup"><span data-stu-id="605d4-191">Specify the list of permissions to retrieve from the provider by specifying the <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>.</span></span> <span data-ttu-id="605d4-192">Ortak dış sağlayıcılar için kimlik doğrulama kapsamları aşağıdaki tabloda görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="605d4-192">Authentication scopes for common external providers appear in the following table.</span></span>

| <span data-ttu-id="605d4-193">Sağlayıcı</span><span class="sxs-lookup"><span data-stu-id="605d4-193">Provider</span></span>  | <span data-ttu-id="605d4-194">`Scope`</span><span class="sxs-lookup"><span data-stu-id="605d4-194">Scope</span></span>                                                            |
| --------- | ---------------------------------------------------------------- |
| <span data-ttu-id="605d4-195">Facebook</span><span class="sxs-lookup"><span data-stu-id="605d4-195">Facebook</span></span>  | `https://www.facebook.com/dialog/oauth`                          |
| <span data-ttu-id="605d4-196">Google</span><span class="sxs-lookup"><span data-stu-id="605d4-196">Google</span></span>    | `https://www.googleapis.com/auth/userinfo.profile`               |
| <span data-ttu-id="605d4-197">Microsoft</span><span class="sxs-lookup"><span data-stu-id="605d4-197">Microsoft</span></span> | `https://login.microsoftonline.com/common/oauth2/v2.0/authorize` |
| <span data-ttu-id="605d4-198">Twitter</span><span class="sxs-lookup"><span data-stu-id="605d4-198">Twitter</span></span>   | `https://api.twitter.com/oauth/authenticate`                     |

<span data-ttu-id="605d4-199">Örnek uygulamada, Google 'ın `userinfo.profile` kapsamı, <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder> ' de <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> çağrıldığında Framework tarafından otomatik olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="605d4-199">In the sample app, Google's `userinfo.profile` scope is automatically added by the framework when <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> is called on the <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder>.</span></span> <span data-ttu-id="605d4-200">Uygulama ek kapsamlar gerektiriyorsa, bunları seçeneklere ekleyin.</span><span class="sxs-lookup"><span data-stu-id="605d4-200">If the app requires additional scopes, add them to the options.</span></span> <span data-ttu-id="605d4-201">Aşağıdaki örnekte, bir kullanıcının Doğum gününü almak için Google `https://www.googleapis.com/auth/user.birthday.read` kapsamı eklenir:</span><span class="sxs-lookup"><span data-stu-id="605d4-201">In the following example, the Google `https://www.googleapis.com/auth/user.birthday.read` scope is added in order to retrieve a user's birthday:</span></span>

```csharp
options.Scope.Add("https://www.googleapis.com/auth/user.birthday.read");
```

## <a name="map-user-data-keys-and-create-claims"></a><span data-ttu-id="605d4-202">Kullanıcı veri anahtarlarını eşleme ve talepler oluşturma</span><span class="sxs-lookup"><span data-stu-id="605d4-202">Map user data keys and create claims</span></span>

<span data-ttu-id="605d4-203">Sağlayıcının seçeneklerinde, oturum açma sırasında okunacak uygulama kimliği için dış sağlayıcının JSON Kullanıcı verilerinde her anahtar/alt anahtar için bir <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> veya <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonSubKey*> belirtin.</span><span class="sxs-lookup"><span data-stu-id="605d4-203">In the provider's options, specify a <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> or <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonSubKey*> for each key/subkey in the external provider's JSON user data for the app identity to read on sign in.</span></span> <span data-ttu-id="605d4-204">Talep türleri hakkında daha fazla bilgi için bkz. <xref:System.Security.Claims.ClaimTypes>.</span><span class="sxs-lookup"><span data-stu-id="605d4-204">For more information on claim types, see <xref:System.Security.Claims.ClaimTypes>.</span></span>

<span data-ttu-id="605d4-205">Örnek uygulama, Google user Data 'daki `locale` ve `picture` anahtarlarından yerel ayar (`urn:google:locale`) ve resim (`urn:google:picture`) talepleri oluşturur:</span><span class="sxs-lookup"><span data-stu-id="605d4-205">The sample app creates locale (`urn:google:locale`) and picture (`urn:google:picture`) claims from the `locale` and `picture` keys in Google user data:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=13-14)]

<span data-ttu-id="605d4-206">@No__t-0 ' da bir <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`), <xref:Microsoft.AspNetCore.Identity.SignInManager%601.SignInAsync*> ile uygulamada oturum açtı.</span><span class="sxs-lookup"><span data-stu-id="605d4-206">In <xref:Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync*>, an <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) is signed into the app with <xref:Microsoft.AspNetCore.Identity.SignInManager%601.SignInAsync*>.</span></span> <span data-ttu-id="605d4-207">@No__t-0, oturum açma işlemi sırasında <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*> ' den kullanılabilir Kullanıcı verileri için `ApplicationUser` taleplerini saklayabilir.</span><span class="sxs-lookup"><span data-stu-id="605d4-207">During the sign in process, the <xref:Microsoft.AspNetCore.Identity.UserManager%601> can store an `ApplicationUser` claims for user data available from the <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>.</span></span>

<span data-ttu-id="605d4-208">Örnek uygulamada `OnPostConfirmationAsync` (*Account/ExternalLogin. cshtml. cs*), <xref:System.Security.Claims.ClaimTypes.GivenName> için bir talep dahil, oturum açan `ApplicationUser` için yerel ayar (`urn:google:locale`) ve resim (`urn:google:picture`) taleplerini belirler:</span><span class="sxs-lookup"><span data-stu-id="605d4-208">In the sample app, `OnPostConfirmationAsync` (*Account/ExternalLogin.cshtml.cs*) establishes the locale (`urn:google:locale`) and picture (`urn:google:picture`) claims for the signed in `ApplicationUser`, including a claim for <xref:System.Security.Claims.ClaimTypes.GivenName>:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=35-51)]

<span data-ttu-id="605d4-209">Varsayılan olarak, bir kullanıcının talepleri kimlik doğrulama tanımlama bilgisinde depolanır.</span><span class="sxs-lookup"><span data-stu-id="605d4-209">By default, a user's claims are stored in the authentication cookie.</span></span> <span data-ttu-id="605d4-210">Kimlik doğrulama tanımlama bilgisi çok büyükse, bunun nedeni uygulamanın başarısız olmasına neden olabilir:</span><span class="sxs-lookup"><span data-stu-id="605d4-210">If the authentication cookie is too large, it can cause the app to fail because:</span></span>

* <span data-ttu-id="605d4-211">Tarayıcı, tanımlama bilgisi üstbilgisinin çok uzun olduğunu algılar.</span><span class="sxs-lookup"><span data-stu-id="605d4-211">The browser detects that the cookie header is too long.</span></span>
* <span data-ttu-id="605d4-212">İsteğin genel boyutu çok büyük.</span><span class="sxs-lookup"><span data-stu-id="605d4-212">The overall size of the request is too large.</span></span>

<span data-ttu-id="605d4-213">Kullanıcı isteklerini işlemek için büyük miktarda Kullanıcı verisi gerekliyse:</span><span class="sxs-lookup"><span data-stu-id="605d4-213">If a large amount of user data is required for processing user requests:</span></span>

* <span data-ttu-id="605d4-214">İstek işleme için Kullanıcı taleplerinin sayısını ve boyutunu yalnızca uygulamanın gerektirdiği şekilde sınırlayın.</span><span class="sxs-lookup"><span data-stu-id="605d4-214">Limit the number and size of user claims for request processing to only what the app requires.</span></span>
* <span data-ttu-id="605d4-215">Kimlik bilgisi kimlik doğrulaması @no__t ara yazılımı için bir özel <xref:Microsoft.AspNetCore.Authentication.Cookies.ITicketStore> kullanın.</span><span class="sxs-lookup"><span data-stu-id="605d4-215">Use a custom <xref:Microsoft.AspNetCore.Authentication.Cookies.ITicketStore> for the Cookie Authentication Middleware's <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.SessionStore> to store identity across requests.</span></span> <span data-ttu-id="605d4-216">Yalnızca istemciye küçük bir oturum tanımlayıcı anahtarı gönderilirken sunucuda büyük miktarlarda kimlik bilgilerini koruyun.</span><span class="sxs-lookup"><span data-stu-id="605d4-216">Preserve large quantities of identity information on the server while only sending a small session identifier key to the client.</span></span>

## <a name="save-the-access-token"></a><span data-ttu-id="605d4-217">Erişim belirtecini Kaydet</span><span class="sxs-lookup"><span data-stu-id="605d4-217">Save the access token</span></span>

<span data-ttu-id="605d4-218"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> başarılı bir yetkilendirmeden sonra erişim ve yenileme belirteçlerinin <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> ' de depolanması gerekip gerekmediğini tanımlar.</span><span class="sxs-lookup"><span data-stu-id="605d4-218"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> defines whether access and refresh tokens should be stored in the <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> after a successful authorization.</span></span> <span data-ttu-id="605d4-219">`SaveTokens`, son kimlik doğrulama tanımlama bilgisinin boyutunu azaltmak için varsayılan olarak `false` olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="605d4-219">`SaveTokens` is set to `false` by default to reduce the size of the final authentication cookie.</span></span>

<span data-ttu-id="605d4-220">Örnek uygulama, <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions> ' de `SaveTokens` değerini `true` olarak ayarlar:</span><span class="sxs-lookup"><span data-stu-id="605d4-220">The sample app sets the value of `SaveTokens` to `true` in <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=15)]

<span data-ttu-id="605d4-221">@No__t-0 yürütüldüğünde, erişim belirtecini ([Externalloginınfo. AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)), `ApplicationUser` ' nin `AuthenticationProperties` ' teki dış sağlayıcıdan depolayın.</span><span class="sxs-lookup"><span data-stu-id="605d4-221">When `OnPostConfirmationAsync` executes, store the access token ([ExternalLoginInfo.AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) from the external provider in the `ApplicationUser`'s `AuthenticationProperties`.</span></span>

<span data-ttu-id="605d4-222">Örnek uygulama, erişim belirtecini `OnPostConfirmationAsync` ' a (Yeni Kullanıcı kaydı) ve `OnGetCallbackAsync` ' i (önceden kaydedilmiş Kullanıcı) *Account/ExternalLogin. cshtml. cs*içinde kaydeder:</span><span class="sxs-lookup"><span data-stu-id="605d4-222">The sample app saves the access token in `OnPostConfirmationAsync` (new user registration) and `OnGetCallbackAsync` (previously registered user) in *Account/ExternalLogin.cshtml.cs*:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=54-56)]

## <a name="how-to-add-additional-custom-tokens"></a><span data-ttu-id="605d4-223">Ek özel belirteçler ekleme</span><span class="sxs-lookup"><span data-stu-id="605d4-223">How to add additional custom tokens</span></span>

<span data-ttu-id="605d4-224">@No__t-0 ' ın bir parçası olarak depolanan özel bir belirtecin nasıl ekleneceğini göstermek için, örnek uygulama, `TicketCreated` ' [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) için geçerli <xref:System.DateTime> ' ye sahip <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> ekler:</span><span class="sxs-lookup"><span data-stu-id="605d4-224">To demonstrate how to add a custom token, which is stored as part of `SaveTokens`, the sample app adds an <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> with the current <xref:System.DateTime> for an [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) of `TicketCreated`:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=17-30)]

## <a name="creating-and-adding-claims"></a><span data-ttu-id="605d4-225">Talepler oluşturma ve ekleme</span><span class="sxs-lookup"><span data-stu-id="605d4-225">Creating and adding claims</span></span>

<span data-ttu-id="605d4-226">Framework, koleksiyona talepler oluşturmak ve eklemek için ortak eylemler ve genişletme yöntemleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="605d4-226">The framework provides common actions and extension methods for creating and adding claims to the collection.</span></span> <span data-ttu-id="605d4-227">Daha fazla bilgi için <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions> ve <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionUniqueExtensions> ' a bakın.</span><span class="sxs-lookup"><span data-stu-id="605d4-227">For more information, see the <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions> and <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionUniqueExtensions>.</span></span>

<span data-ttu-id="605d4-228">Kullanıcılar, <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction> ' dan türeterek ve soyut <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.Run*> metodunu uygulayarak özel eylemleri tanımlayabilir.</span><span class="sxs-lookup"><span data-stu-id="605d4-228">Users can define custom actions by deriving from <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction> and implementing the abstract <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.Run*> method.</span></span>

<span data-ttu-id="605d4-229">Daha fazla bilgi için bkz. <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims>.</span><span class="sxs-lookup"><span data-stu-id="605d4-229">For more information, see <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims>.</span></span>

## <a name="removal-of-claim-actions-and-claims"></a><span data-ttu-id="605d4-230">Talep eylemlerinin ve taleplerin kaldırılması</span><span class="sxs-lookup"><span data-stu-id="605d4-230">Removal of claim actions and claims</span></span>

<span data-ttu-id="605d4-231">[Claimactioncollection. Remove (dize)](xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimActionCollection.Remove*) , koleksiyonda verilen <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> için tüm talep eylemlerini kaldırır.</span><span class="sxs-lookup"><span data-stu-id="605d4-231">[ClaimActionCollection.Remove(String)](xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimActionCollection.Remove*) removes all claim actions for the given <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> from the collection.</span></span> <span data-ttu-id="605d4-232">[Claimactioncollectionmapextensions. DeleteClaim (ClaimActionCollection, String)](xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*) , kimliğin verilen <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> talebini siler.</span><span class="sxs-lookup"><span data-stu-id="605d4-232">[ClaimActionCollectionMapExtensions.DeleteClaim(ClaimActionCollection, String)](xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*) deletes a claim of the given <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> from the identity.</span></span> <span data-ttu-id="605d4-233"><xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*> birincil olarak, protokol tarafından oluşturulan talepleri kaldırmak için [OpenID Connect (OıDC)](/azure/active-directory/develop/v2-protocols-oidc) ile birlikte kullanılır.</span><span class="sxs-lookup"><span data-stu-id="605d4-233"><xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*> is primarily used with [OpenID Connect (OIDC)](/azure/active-directory/develop/v2-protocols-oidc) to remove protocol-generated claims.</span></span>

## <a name="sample-app-output"></a><span data-ttu-id="605d4-234">Örnek uygulama çıkışı</span><span class="sxs-lookup"><span data-stu-id="605d4-234">Sample app output</span></span>

```
User Claims

http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier
    9b342344f-7aab-43c2-1ac1-ba75912ca999
http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name
    someone@gmail.com
AspNet.Identity.SecurityStamp
    7D4312MOWRYYBFI1KXRPHGOSTBVWSFDE
http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname
    Judy
urn:google:locale
    en
urn:google:picture
    https://lh4.googleusercontent.com/-XXXXXX/XXXXXX/XXXXXX/XXXXXX/photo.jpg

Authentication Properties

.Token.access_token
    yc23.AlvoZqz56...1lxltXV7D-ZWP9
.Token.token_type
    Bearer
.Token.expires_at
    2019-04-11T22:14:51.0000000+00:00
.Token.TicketCreated
    4/11/2019 9:14:52 PM
.TokenNames
    access_token;token_type;expires_at;TicketCreated
.persistent
.issued
    Thu, 11 Apr 2019 20:51:06 GMT
.expires
    Thu, 25 Apr 2019 20:51:06 GMT

```

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="605d4-235">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="605d4-235">Additional resources</span></span>

* <span data-ttu-id="605d4-236">[ASPNET/aspnetcore mühendislik SocialSample app](https://github.com/aspnet/AspNetCore/tree/master/src/Security/Authentication/samples/SocialSample) &ndash; bağlantılı örnek uygulama, [ASPNET/aspnetcore GitHub deposunun](https://github.com/aspnet/AspNetCore) `master` mühendislik dalında bulunur.</span><span class="sxs-lookup"><span data-stu-id="605d4-236">[aspnet/AspNetCore engineering SocialSample app](https://github.com/aspnet/AspNetCore/tree/master/src/Security/Authentication/samples/SocialSample) &ndash; The linked sample app is on the [aspnet/AspNetCore GitHub repo's](https://github.com/aspnet/AspNetCore) `master` engineering branch.</span></span> <span data-ttu-id="605d4-237">@No__t-0 dalı, ASP.NET Core sonraki sürümü için etkin geliştirme kapsamındaki kodu içerir.</span><span class="sxs-lookup"><span data-stu-id="605d4-237">The `master` branch contains code under active development for the next release of ASP.NET Core.</span></span> <span data-ttu-id="605d4-238">Yayınlanmış bir ASP.NET Core sürümü için örnek uygulamanın bir sürümünü görmek için, **dal** açılan listesini kullanarak bir yayın dalı seçin (örneğin `release/{X.Y}`).</span><span class="sxs-lookup"><span data-stu-id="605d4-238">To see a version of the sample app for a released version of ASP.NET Core, use the **Branch** drop down list to select a release branch (for example `release/{X.Y}`).</span></span>
