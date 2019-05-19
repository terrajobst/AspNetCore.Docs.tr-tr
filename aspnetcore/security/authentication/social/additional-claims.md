---
title: Ek talep ve ASP.NET Core, dış sağlayıcılardan gelen belirteçleri kalıcı
author: guardrex
description: Ek talep ve dış sağlayıcılardan gelen belirteçleri oluşturma hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/14/2019
uid: security/authentication/social/additional-claims
ms.openlocfilehash: e18287e5a4171b3f7a6daa122111448b8447c1da
ms.sourcegitcommit: ccbb84ae307a5bc527441d3d509c20b5c1edde05
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2019
ms.locfileid: "65874853"
---
# <a name="persist-additional-claims-and-tokens-from-external-providers-in-aspnet-core"></a><span data-ttu-id="138c3-103">Ek talep ve ASP.NET Core, dış sağlayıcılardan gelen belirteçleri kalıcı</span><span class="sxs-lookup"><span data-stu-id="138c3-103">Persist additional claims and tokens from external providers in ASP.NET Core</span></span>

<span data-ttu-id="138c3-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="138c3-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="138c3-105">ASP.NET Core uygulaması ek talep ve Facebook, Google, Microsoft ve Twitter gibi dış kimlik sağlayıcılardan gelen belirteçleri oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="138c3-105">An ASP.NET Core app can establish additional claims and tokens from external authentication providers, such as Facebook, Google, Microsoft, and Twitter.</span></span> <span data-ttu-id="138c3-106">Her bir sağlayıcı platformu farklı kullanıcılar hakkında bilgi gösterir, ancak alma ve kullanıcı verilerini ek talepleri dönüştürme için desen aynıdır.</span><span class="sxs-lookup"><span data-stu-id="138c3-106">Each provider reveals different information about users on its platform, but the pattern for receiving and transforming user data into additional claims is the same.</span></span>

<span data-ttu-id="138c3-107">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="138c3-107">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="138c3-108">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="138c3-108">Prerequisites</span></span>

<span data-ttu-id="138c3-109">Uygulamayı desteklemek için hangi Dış kimlik doğrulama sağlayıcıları karar verin.</span><span class="sxs-lookup"><span data-stu-id="138c3-109">Decide which external authentication providers to support in the app.</span></span> <span data-ttu-id="138c3-110">Tüm sağlayıcılar için uygulamayı kaydetme ve bir istemci kimliği ve istemci gizli anahtarını alın.</span><span class="sxs-lookup"><span data-stu-id="138c3-110">For each provider, register the app and obtain a client ID and client secret.</span></span> <span data-ttu-id="138c3-111">Daha fazla bilgi için bkz. <xref:security/authentication/social/index>.</span><span class="sxs-lookup"><span data-stu-id="138c3-111">For more information, see <xref:security/authentication/social/index>.</span></span> <span data-ttu-id="138c3-112">Örnek uygulama kullandığı [Google kimlik doğrulama sağlayıcısı](xref:security/authentication/google-logins).</span><span class="sxs-lookup"><span data-stu-id="138c3-112">The sample app uses the [Google authentication provider](xref:security/authentication/google-logins).</span></span>

## <a name="set-the-client-id-and-client-secret"></a><span data-ttu-id="138c3-113">İstemci Kimliğini ve istemci gizli dizisi ayarlayın</span><span class="sxs-lookup"><span data-stu-id="138c3-113">Set the client ID and client secret</span></span>

<span data-ttu-id="138c3-114">OAuth kimlik doğrulama sağlayıcısı, bir istemci kimliği ve istemci gizli anahtarı kullanarak bir uygulama ile bir güven ilişkisi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="138c3-114">The OAuth authentication provider establishes a trust relationship with an app using a client ID and client secret.</span></span> <span data-ttu-id="138c3-115">Uygulamayı sağlayıcıda kaydedildiğinde istemci Kimliğini ve istemci gizli değerleri uygulama için Dış kimlik doğrulama sağlayıcısı tarafından oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="138c3-115">Client ID and client secret values are created for the app by the external authentication provider when the app is registered with the provider.</span></span> <span data-ttu-id="138c3-116">Uygulamanın kullandığı her bir dış sağlayıcı bağımsız olarak sağlayıcının istemci Kimliğini ve istemci gizli anahtarı ile yapılandırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="138c3-116">Each external provider that the app uses must be configured independently with the provider's client ID and client secret.</span></span> <span data-ttu-id="138c3-117">Daha fazla bilgi için senaryonuz için geçerli olan dış kimlik doğrulama sağlayıcısı konulara bakın:</span><span class="sxs-lookup"><span data-stu-id="138c3-117">For more information, see the external authentication provider topics that apply to your scenario:</span></span>

* [<span data-ttu-id="138c3-118">Facebook kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="138c3-118">Facebook authentication</span></span>](xref:security/authentication/facebook-logins)
* [<span data-ttu-id="138c3-119">Google kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="138c3-119">Google authentication</span></span>](xref:security/authentication/google-logins)
* [<span data-ttu-id="138c3-120">Microsoft kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="138c3-120">Microsoft authentication</span></span>](xref:security/authentication/microsoft-logins)
* [<span data-ttu-id="138c3-121">Twitter kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="138c3-121">Twitter authentication</span></span>](xref:security/authentication/twitter-logins)
* [<span data-ttu-id="138c3-122">Diğer kimlik doğrulama sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="138c3-122">Other authentication providers</span></span>](xref:security/authentication/otherlogins)
* [<span data-ttu-id="138c3-123">OpenIdConnect</span><span class="sxs-lookup"><span data-stu-id="138c3-123">OpenIdConnect</span></span>](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2)

<span data-ttu-id="138c3-124">Örnek uygulama, bir istemci kimliği ve istemci gizli anahtarı Google tarafından sağlanan Google kimlik doğrulama sağlayıcısı yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="138c3-124">The sample app configures the Google authentication provider with a client ID and client secret provided by Google:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=4,9)]

## <a name="establish-the-authentication-scope"></a><span data-ttu-id="138c3-125">Kimlik doğrulama kapsamı oluşturmak</span><span class="sxs-lookup"><span data-stu-id="138c3-125">Establish the authentication scope</span></span>

<span data-ttu-id="138c3-126">Belirterek sağlayıcıdan almak için izinler listesinden belirtin <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>.</span><span class="sxs-lookup"><span data-stu-id="138c3-126">Specify the list of permissions to retrieve from the provider by specifying the <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>.</span></span> <span data-ttu-id="138c3-127">Yaygın dış sağlayıcıları için kimlik doğrulaması kapsamları aşağıdaki tabloda görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="138c3-127">Authentication scopes for common external providers appear in the following table.</span></span>

| <span data-ttu-id="138c3-128">Sağlayıcı</span><span class="sxs-lookup"><span data-stu-id="138c3-128">Provider</span></span>  | <span data-ttu-id="138c3-129">Kapsam</span><span class="sxs-lookup"><span data-stu-id="138c3-129">Scope</span></span>                                                            |
| --------- | ---------------------------------------------------------------- |
| <span data-ttu-id="138c3-130">Facebook</span><span class="sxs-lookup"><span data-stu-id="138c3-130">Facebook</span></span>  | `https://www.facebook.com/dialog/oauth`                          |
| <span data-ttu-id="138c3-131">Google</span><span class="sxs-lookup"><span data-stu-id="138c3-131">Google</span></span>    | `https://www.googleapis.com/auth/userinfo.profile`               |
| <span data-ttu-id="138c3-132">Microsoft</span><span class="sxs-lookup"><span data-stu-id="138c3-132">Microsoft</span></span> | `https://login.microsoftonline.com/common/oauth2/v2.0/authorize` |
| <span data-ttu-id="138c3-133">Twitter</span><span class="sxs-lookup"><span data-stu-id="138c3-133">Twitter</span></span>   | `https://api.twitter.com/oauth/authenticate`                     |

<span data-ttu-id="138c3-134">Örnek uygulamanın, Google `userinfo.profile` kapsam framework tarafından otomatik olarak eklenir, <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> üzerinde çağrılır <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="138c3-134">In the sample app, Google's `userinfo.profile` scope is automatically added by the framework when <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> is called on the <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder>.</span></span> <span data-ttu-id="138c3-135">Uygulamayı ek kapsamlarla gerektiriyorsa, bunları seçenekleri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="138c3-135">If the app requires additional scopes, add them to the options.</span></span> <span data-ttu-id="138c3-136">Aşağıdaki örnekte, Google `https://www.googleapis.com/auth/user.birthday.read` kapsamı, bir kullanıcının doğum gününü almak için eklenir:</span><span class="sxs-lookup"><span data-stu-id="138c3-136">In the following example, the Google `https://www.googleapis.com/auth/user.birthday.read` scope is added in order to retrieve a user's birthday:</span></span>

```csharp
options.Scope.Add("https://www.googleapis.com/auth/user.birthday.read");
```

## <a name="map-user-data-keys-and-create-claims"></a><span data-ttu-id="138c3-137">Kullanıcı veri anahtarları eşleyin ve talep oluşturma</span><span class="sxs-lookup"><span data-stu-id="138c3-137">Map user data keys and create claims</span></span>

<span data-ttu-id="138c3-138">Sağlayıcının seçeneklerinde belirtin bir <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> veya <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonSubKey*> her anahtar/alt oturum açma okumak uygulama kimliği için harici sağlayıcıya ait JSON kullanıcı verileri.</span><span class="sxs-lookup"><span data-stu-id="138c3-138">In the provider's options, specify a <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> or <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonSubKey*> for each key/subkey in the external provider's JSON user data for the app identity to read on sign in.</span></span> <span data-ttu-id="138c3-139">Talep türleri hakkında daha fazla bilgi için bkz. <xref:System.Security.Claims.ClaimTypes>.</span><span class="sxs-lookup"><span data-stu-id="138c3-139">For more information on claim types, see <xref:System.Security.Claims.ClaimTypes>.</span></span>

<span data-ttu-id="138c3-140">Örnek uygulamayı yerel ayar oluşturur (`urn:google:locale`) ve resim (`urn:google:picture`) gelen talepleri `locale` ve `picture` Google kullanıcı verileri anahtarları:</span><span class="sxs-lookup"><span data-stu-id="138c3-140">The sample app creates locale (`urn:google:locale`) and picture (`urn:google:picture`) claims from the `locale` and `picture` keys in Google user data:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=13-14)]

<span data-ttu-id="138c3-141">İçinde <xref:Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync*>e <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) ile uygulama oturum açmış <xref:Microsoft.AspNetCore.Identity.SignInManager%601.SignInAsync*>.</span><span class="sxs-lookup"><span data-stu-id="138c3-141">In <xref:Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync*>, an <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) is signed into the app with <xref:Microsoft.AspNetCore.Identity.SignInManager%601.SignInAsync*>.</span></span> <span data-ttu-id="138c3-142">Oturum açma işlemi sırasında <xref:Microsoft.AspNetCore.Identity.UserManager%601> depolayabilirsiniz bir `ApplicationUser` bulunan kullanıcı verileri için talep <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>.</span><span class="sxs-lookup"><span data-stu-id="138c3-142">During the sign in process, the <xref:Microsoft.AspNetCore.Identity.UserManager%601> can store an `ApplicationUser` claims for user data available from the <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>.</span></span>

<span data-ttu-id="138c3-143">Örnek uygulamada `OnPostConfirmationAsync` (*Account/ExternalLogin.cshtml.cs*) yerel ayarı oluşturur (`urn:google:locale`) ve resim (`urn:google:picture`) imzalı için talepleri de `ApplicationUser`, için talep dahil olmak üzere <xref:System.Security.Claims.ClaimTypes.GivenName> :</span><span class="sxs-lookup"><span data-stu-id="138c3-143">In the sample app, `OnPostConfirmationAsync` (*Account/ExternalLogin.cshtml.cs*) establishes the locale (`urn:google:locale`) and picture (`urn:google:picture`) claims for the signed in `ApplicationUser`, including a claim for <xref:System.Security.Claims.ClaimTypes.GivenName>:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=35-51)]

<span data-ttu-id="138c3-144">Varsayılan olarak, bir kullanıcının talebi kimlik doğrulama tanımlama bilgisinde depolanır.</span><span class="sxs-lookup"><span data-stu-id="138c3-144">By default, a user's claims are stored in the authentication cookie.</span></span> <span data-ttu-id="138c3-145">Kimlik doğrulama tanımlama çok büyükse, uygulamanın başarısız olmasına neden olabilir:</span><span class="sxs-lookup"><span data-stu-id="138c3-145">If the authentication cookie is too large, it can cause the app to fail because:</span></span>

* <span data-ttu-id="138c3-146">Tarayıcı tanımlama bilgisi üstbilgisi çok uzun olduğunu algılar.</span><span class="sxs-lookup"><span data-stu-id="138c3-146">The browser detects that the cookie header is too long.</span></span>
* <span data-ttu-id="138c3-147">Genel istek boyutu çok büyük.</span><span class="sxs-lookup"><span data-stu-id="138c3-147">The overall size of the request is too large.</span></span>

<span data-ttu-id="138c3-148">Kullanıcı verileri, büyük bir miktarını, kullanıcı isteklerini işlemek için gerekli ise:</span><span class="sxs-lookup"><span data-stu-id="138c3-148">If a large amount of user data is required for processing user requests:</span></span>

* <span data-ttu-id="138c3-149">Sayı ve istek işleme için yalnızca ne gerektiren uygulama için kullanıcı taleplerini boyutunu sınırlandırın.</span><span class="sxs-lookup"><span data-stu-id="138c3-149">Limit the number and size of user claims for request processing to only what the app requires.</span></span>
* <span data-ttu-id="138c3-150">Özel bir kullanın <xref:Microsoft.AspNetCore.Authentication.Cookies.ITicketStore> tanımlama bilgisi kimlik doğrulaması ara için <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.SessionStore> isteklerdeki kimliklerin depolanacağı için.</span><span class="sxs-lookup"><span data-stu-id="138c3-150">Use a custom <xref:Microsoft.AspNetCore.Authentication.Cookies.ITicketStore> for the Cookie Authentication Middleware's <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.SessionStore> to store identity across requests.</span></span> <span data-ttu-id="138c3-151">Sunucudaki kimlik bilgileri yalnızca küçük bir oturum tanımlayıcısı anahtarı istemciye gönderme sırasında büyük miktarlarda korur.</span><span class="sxs-lookup"><span data-stu-id="138c3-151">Preserve large quantities of identity information on the server while only sending a small session identifier key to the client.</span></span>

## <a name="save-the-access-token"></a><span data-ttu-id="138c3-152">Erişim belirtecini kaydetme</span><span class="sxs-lookup"><span data-stu-id="138c3-152">Save the access token</span></span>

<span data-ttu-id="138c3-153"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> erişim ve yenileme belirteçleri de depolanıp depolanmayacağını tanımlar <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> başarılı yetkilendirme sonrasında.</span><span class="sxs-lookup"><span data-stu-id="138c3-153"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> defines whether access and refresh tokens should be stored in the <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> after a successful authorization.</span></span> <span data-ttu-id="138c3-154">`SaveTokens` ayarlanır `false` son kimlik doğrulama tanımlama bilgisinin boyutunu azaltmak için varsayılan olarak.</span><span class="sxs-lookup"><span data-stu-id="138c3-154">`SaveTokens` is set to `false` by default to reduce the size of the final authentication cookie.</span></span>

<span data-ttu-id="138c3-155">Örnek uygulamayı değerini ayarlar `SaveTokens` için `true` içinde <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:</span><span class="sxs-lookup"><span data-stu-id="138c3-155">The sample app sets the value of `SaveTokens` to `true` in <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=15)]

<span data-ttu-id="138c3-156">Zaman `OnPostConfirmationAsync` yürütür, depolama ve erişim belirteci ([ExternalLoginInfo.AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) dış sağlayıcıdan `ApplicationUser`'s `AuthenticationProperties`.</span><span class="sxs-lookup"><span data-stu-id="138c3-156">When `OnPostConfirmationAsync` executes, store the access token ([ExternalLoginInfo.AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) from the external provider in the `ApplicationUser`'s `AuthenticationProperties`.</span></span>

<span data-ttu-id="138c3-157">Erişim belirtecinde örnek uygulamayı kaydeder `OnPostConfirmationAsync` (yeni kullanıcı kaydı) ve `OnGetCallbackAsync` (önceden kaydedilmiş kullanıcı) içinde *Account/ExternalLogin.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="138c3-157">The sample app saves the access token in `OnPostConfirmationAsync` (new user registration) and `OnGetCallbackAsync` (previously registered user) in *Account/ExternalLogin.cshtml.cs*:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=54-56)]

## <a name="how-to-add-additional-custom-tokens"></a><span data-ttu-id="138c3-158">Ek özel belirteçler ekleme</span><span class="sxs-lookup"><span data-stu-id="138c3-158">How to add additional custom tokens</span></span>

<span data-ttu-id="138c3-159">Bir parçası olarak depolanan özel bir belirteç ekleme göstermek için `SaveTokens`, örnek uygulamayı ekler bir <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> geçerli <xref:System.DateTime> için bir [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) , `TicketCreated`:</span><span class="sxs-lookup"><span data-stu-id="138c3-159">To demonstrate how to add a custom token, which is stored as part of `SaveTokens`, the sample app adds an <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> with the current <xref:System.DateTime> for an [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) of `TicketCreated`:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=17-28)]

## <a name="creating-and-adding-claims"></a><span data-ttu-id="138c3-160">Oluşturma ve talep ekleme</span><span class="sxs-lookup"><span data-stu-id="138c3-160">Creating and adding claims</span></span>

<span data-ttu-id="138c3-161">Framework, ortak Eylemler ve oluşturmak ve talep koleksiyona eklemek için genişletme yöntemleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="138c3-161">The framework provides common actions and extension methods for creating and adding claims to the collection.</span></span> <span data-ttu-id="138c3-162">Daha fazla bilgi için <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions> ve <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionUniqueExtensions>.</span><span class="sxs-lookup"><span data-stu-id="138c3-162">For more information, see the <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions> and <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionUniqueExtensions>.</span></span>

<span data-ttu-id="138c3-163">Kullanıcılar, türetilen özel eylemler tanımlayabilir <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction> ve Özet uygulama <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.Run*> yöntemi.</span><span class="sxs-lookup"><span data-stu-id="138c3-163">Users can define custom actions by deriving from <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction> and implementing the abstract <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.Run*> method.</span></span>

<span data-ttu-id="138c3-164">Daha fazla bilgi için bkz. <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims>.</span><span class="sxs-lookup"><span data-stu-id="138c3-164">For more information, see <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims>.</span></span>

## <a name="removal-of-claim-actions-and-claims"></a><span data-ttu-id="138c3-165">Talep Eylemler ve taleplerini kaldırma</span><span class="sxs-lookup"><span data-stu-id="138c3-165">Removal of claim actions and claims</span></span>

<span data-ttu-id="138c3-166">[ClaimActionCollection.Remove(String)](xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimActionCollection.Remove*) kaldırır tüm eylemler için talep verilen <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> koleksiyondan.</span><span class="sxs-lookup"><span data-stu-id="138c3-166">[ClaimActionCollection.Remove(String)](xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimActionCollection.Remove*) removes all claim actions for the given <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> from the collection.</span></span> <span data-ttu-id="138c3-167">[ClaimActionCollectionMapExtensions.DeleteClaim (ClaimActionCollection, String)](xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*) bir talep siler verilen <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> gelen kimlik.</span><span class="sxs-lookup"><span data-stu-id="138c3-167">[ClaimActionCollectionMapExtensions.DeleteClaim(ClaimActionCollection, String)](xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*) deletes a claim of the given <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> from the identity.</span></span> <span data-ttu-id="138c3-168"><xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*> ile kullanılır [Openıd Connect (OIDC)](/azure/active-directory/develop/v2-protocols-oidc) protokolü tarafından oluşturulan talepleri kaldırmak için.</span><span class="sxs-lookup"><span data-stu-id="138c3-168"><xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*> is primarily used with [OpenID Connect (OIDC)](/azure/active-directory/develop/v2-protocols-oidc) to remove protocol-generated claims.</span></span>

## <a name="sample-app-output"></a><span data-ttu-id="138c3-169">Örnek uygulama çıktısı</span><span class="sxs-lookup"><span data-stu-id="138c3-169">Sample app output</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="138c3-170">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="138c3-170">Additional resources</span></span>

* <span data-ttu-id="138c3-171">[ASP.NET/AspNetCore mühendislik SocialSample uygulama](https://github.com/aspnet/AspNetCore/tree/master/src/Security/Authentication/samples/SocialSample) &ndash; bağlı örnek uygulamasını açıktır [aspnet/AspNetCore GitHub deponun](https://github.com/aspnet/AspNetCore) `master` mühendislik dal.</span><span class="sxs-lookup"><span data-stu-id="138c3-171">[aspnet/AspNetCore engineering SocialSample app](https://github.com/aspnet/AspNetCore/tree/master/src/Security/Authentication/samples/SocialSample) &ndash; The linked sample app is on the [aspnet/AspNetCore GitHub repo's](https://github.com/aspnet/AspNetCore) `master` engineering branch.</span></span> <span data-ttu-id="138c3-172">`master` Dalı ASP.NET core'un sonraki active geliştirme aşamasındaki kodun içerir.</span><span class="sxs-lookup"><span data-stu-id="138c3-172">The `master` branch contains code under active development for the next release of ASP.NET Core.</span></span> <span data-ttu-id="138c3-173">ASP.NET Core yayımlanmış bir sürüm için örnek uygulama sürümünü görmek için **dal** açılan listesinden bir sürüm dalı seçin (örneğin `release/2.2`).</span><span class="sxs-lookup"><span data-stu-id="138c3-173">To see a version of the sample app for a released version of ASP.NET Core, use the **Branch** drop down list to select a release branch (for example `release/2.2`).</span></span>
