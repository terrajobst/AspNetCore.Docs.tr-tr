---
title: ASP.NET core'da Google dış oturum açma Kurulumu
author: rick-anderson
description: Bu öğreticide, mevcut bir ASP.NET Core uygulamasına Google hesabı kullanıcı kimlik doğrulaması tümleştirmesini gösterilmektedir.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 06/19/2019
uid: security/authentication/google-logins
ms.openlocfilehash: e12d831d2e0a5c9acae5ea41fb4187ad4ca6b0ea
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71082481"
---
# <a name="google-external-login-setup-in-aspnet-core"></a><span data-ttu-id="972df-103">ASP.NET core'da Google dış oturum açma Kurulumu</span><span class="sxs-lookup"><span data-stu-id="972df-103">Google external login setup in ASP.NET Core</span></span>

<span data-ttu-id="972df-104">Tarafından [Valeriy Novytskyy](https://github.com/01binary) ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="972df-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="972df-105">[Eski Google + API 'leri, 7 mart 2019 itibariyle kapatıldı](https://developers.google.com/+/api-shutdown).</span><span class="sxs-lookup"><span data-stu-id="972df-105">[Legacy Google+ APIs have been shut down as of March 7, 2019](https://developers.google.com/+/api-shutdown).</span></span> <span data-ttu-id="972df-106">Google + oturum açma ve geliştiricilerin yeni bir Google oturum açma sistemine taşınması gerekir.</span><span class="sxs-lookup"><span data-stu-id="972df-106">Google+ sign in and developers must move to a new Google sign in system.</span></span> <span data-ttu-id="972df-107">Google kimlik doğrulaması için ASP.NET Core 2,1 ve 2,2 paketleri değişikliklere uyum sağlayacak şekilde güncelleştirildi.</span><span class="sxs-lookup"><span data-stu-id="972df-107">The ASP.NET Core 2.1 and 2.2 packages for Google Authentication have be updated to accommodate the changes.</span></span> <span data-ttu-id="972df-108">ASP.NET Core daha fazla bilgi ve geçici azaltmaları için [Bu GitHub sorununa](https://github.com/aspnet/AspNetCore/issues/6486)bakın.</span><span class="sxs-lookup"><span data-stu-id="972df-108">For more information and temporary mitigations for ASP.NET Core, see [this GitHub issue](https://github.com/aspnet/AspNetCore/issues/6486).</span></span> <span data-ttu-id="972df-109">Bu öğretici yeni Kurulum işlemiyle güncelleştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="972df-109">This tutorial has been updated with the new setup process.</span></span>

<span data-ttu-id="972df-110">Bu öğreticide, kullanıcıların [önceki sayfada](xref:security/authentication/social/index)oluşturulan ASP.NET Core 2,2 projesini kullanarak Google hesabıyla oturum açmasını nasıl etkinleştireceğinizi gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="972df-110">This tutorial shows you how to enable users to sign in with their Google account using the ASP.NET Core 2.2 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-a-google-api-console-project-and-client-id"></a><span data-ttu-id="972df-111">Google API konsol projesi ve istemci KIMLIĞI oluşturma</span><span class="sxs-lookup"><span data-stu-id="972df-111">Create a Google API Console project and client ID</span></span>

* <span data-ttu-id="972df-112">[Google oturum açma ile Web uygulamanızı tümleştirme](https://developers.google.com/identity/sign-in/web/devconsole-project) ve **Proje yapılandırma**' yı seçme bölümüne gidin.</span><span class="sxs-lookup"><span data-stu-id="972df-112">Navigate to [Integrating Google Sign-In into your web app](https://developers.google.com/identity/sign-in/web/devconsole-project) and select **CONFIGURE A PROJECT**.</span></span>
* <span data-ttu-id="972df-113">**OAuth Istemcinizi yapılandırın** Iletişim kutusunda **Web sunucusu**' nu seçin.</span><span class="sxs-lookup"><span data-stu-id="972df-113">In the **Configure your OAuth client** dialog, select **Web server**.</span></span>
* <span data-ttu-id="972df-114">**Yetkili yeniden yönlendirme URI 'leri** metin girişi kutusunda, YENIDEN yönlendirme URI 'sini ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="972df-114">In the **Authorized redirect URIs** text entry box, set the redirect URI.</span></span> <span data-ttu-id="972df-115">Örneğin, `https://localhost:5001/signin-google`</span><span class="sxs-lookup"><span data-stu-id="972df-115">For example, `https://localhost:5001/signin-google`</span></span>
* <span data-ttu-id="972df-116">**ISTEMCI kimliğini** ve **istemci parolasını**kaydedin.</span><span class="sxs-lookup"><span data-stu-id="972df-116">Save the **Client ID** and **Client Secret**.</span></span>
* <span data-ttu-id="972df-117">Siteyi dağıttığınızda, **Google konsolundan**yeni ortak URL 'yi kaydedin.</span><span class="sxs-lookup"><span data-stu-id="972df-117">When deploying the site, register the new public url from the **Google Console**.</span></span>

## <a name="store-google-clientid-and-clientsecret"></a><span data-ttu-id="972df-118">Store Google ClientID ve ClientSecret</span><span class="sxs-lookup"><span data-stu-id="972df-118">Store Google ClientID and ClientSecret</span></span>

<span data-ttu-id="972df-119">Google `Client ID` ve `Client Secret` [gizli yönetici](xref:security/app-secrets)gibi hassas ayarları depolayın.</span><span class="sxs-lookup"><span data-stu-id="972df-119">Store sensitive settings such as the Google `Client ID` and `Client Secret` with the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="972df-120">Bu öğreticinin amaçları doğrultusunda belirteçleri `Authentication:Google:ClientId` ve `Authentication:Google:ClientSecret`şunları adlandırın:</span><span class="sxs-lookup"><span data-stu-id="972df-120">For the purposes of this tutorial, name the tokens `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret`:</span></span>

```dotnetcli
dotnet user-secrets set "Authentication:Google:ClientId" "X.apps.googleusercontent.com"
dotnet user-secrets set "Authentication:Google:ClientSecret" "<client secret>"
```

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="972df-121">API [konsolunuzun](https://console.developers.google.com/apis/dashboard)API kimlik bilgilerini ve kullanımını yönetebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="972df-121">You can manage your API credentials and usage in the [API Console](https://console.developers.google.com/apis/dashboard).</span></span>

## <a name="configure-google-authentication"></a><span data-ttu-id="972df-122">Google kimlik doğrulamasını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="972df-122">Configure Google authentication</span></span>

<span data-ttu-id="972df-123">Google hizmetini şu şekilde `Startup.ConfigureServices`ekleyin:</span><span class="sxs-lookup"><span data-stu-id="972df-123">Add the Google service to `Startup.ConfigureServices`:</span></span>

[!code-csharp[](~/security/authentication/social/social-code/StartupGoogle.cs?name=snippet_ConfigureServices&highlight=10-18)]

[!INCLUDE [default settings configuration](includes/default-settings2-2.md)]

## <a name="sign-in-with-google"></a><span data-ttu-id="972df-124">Google ile oturum aç</span><span class="sxs-lookup"><span data-stu-id="972df-124">Sign in with Google</span></span>

* <span data-ttu-id="972df-125">Uygulamayı çalıştırın ve **oturum aç**' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="972df-125">Run the app and click **Log in**.</span></span> <span data-ttu-id="972df-126">Google ile oturum açma seçeneği görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="972df-126">An option to sign in with Google appears.</span></span>
* <span data-ttu-id="972df-127">Kimlik doğrulaması için Google 'a yönlendiren **Google** düğmesine tıklayın.</span><span class="sxs-lookup"><span data-stu-id="972df-127">Click the **Google** button, which redirects to Google for authentication.</span></span>
* <span data-ttu-id="972df-128">Google kimlik bilgilerinizi girdikten sonra, Web sitesine geri yönlendirilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="972df-128">After entering your Google credentials, you are redirected back to the web site.</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

<span data-ttu-id="972df-129">Google kimlik doğrulaması tarafından desteklenen yapılandırma seçenekleri hakkında daha fazla bilgi için bkz. APIbaşvurusu.<xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions></span><span class="sxs-lookup"><span data-stu-id="972df-129">See the <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions> API reference for more information on configuration options supported by Google authentication.</span></span> <span data-ttu-id="972df-130">Bu kullanıcı ile ilgili farklı bilgi istemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="972df-130">This can be used to request different information about the user.</span></span>

## <a name="change-the-default-callback-uri"></a><span data-ttu-id="972df-131">Varsayılan geri çağırma URI 'sini değiştirme</span><span class="sxs-lookup"><span data-stu-id="972df-131">Change the default callback URI</span></span>

<span data-ttu-id="972df-132">URI segmenti `/signin-google` Google kimlik doğrulama sağlayıcısı varsayılan geri arama olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="972df-132">The URI segment `/signin-google` is set as the default callback of the Google authentication provider.</span></span> <span data-ttu-id="972df-133">Google kimlik doğrulaması ara yazılımı üzerinden devralınan yapılandırırken, varsayılan geri arama URI'si değiştirebilirsiniz [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) özelliği [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) sınıfı.</span><span class="sxs-lookup"><span data-stu-id="972df-133">You can change the default callback URI while configuring the Google authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) class.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="972df-134">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="972df-134">Troubleshooting</span></span>

* <span data-ttu-id="972df-135">Oturum açma işe yaramazsa ve herhangi bir hata alamıyorsanız, sorunu ayıklamanın daha kolay olması için geliştirme moduna geçin.</span><span class="sxs-lookup"><span data-stu-id="972df-135">If the sign-in doesn't work and you aren't getting any errors, switch to development mode to make the issue easier to debug.</span></span>
* <span data-ttu-id="972df-136">Kimlik ' de `services.AddIdentity` `ConfigureServices`arayarak yapılandırılmamışsa, sonuç *doğrulaması için ArgumentException: ' Signınscheme ' seçeneği sağlanmalıdır*.</span><span class="sxs-lookup"><span data-stu-id="972df-136">If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate results in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="972df-137">Bu öğreticide kullanılan proje şablonu, bu gerçekleştirilir sağlar.</span><span class="sxs-lookup"><span data-stu-id="972df-137">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="972df-138">Site veritabanı, ilk geçiş uygulayarak oluşturulmamış varsa *bir veritabanı işlemi başarısız istek işlenirken* hata.</span><span class="sxs-lookup"><span data-stu-id="972df-138">If the site database has not been created by applying the initial migration, you get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="972df-139">Veritabanını oluşturmak için **geçişleri Uygula** ' yı seçin ve hatanın ötesinde devam etmek için sayfayı yenileyin.</span><span class="sxs-lookup"><span data-stu-id="972df-139">Select **Apply Migrations** to create the database, and refresh the page to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="972df-140">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="972df-140">Next steps</span></span>

* <span data-ttu-id="972df-141">Bu makalede, Google ile kimliğini nasıl doğrulayabileceğiniz gösterdi.</span><span class="sxs-lookup"><span data-stu-id="972df-141">This article showed how you can authenticate with Google.</span></span> <span data-ttu-id="972df-142">Listelenen diğer sağlayıcıları ile kimlik doğrulaması için benzer bir yaklaşım izleyerek [önceki sayfaya](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="972df-142">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>
* <span data-ttu-id="972df-143">Uygulamayı Azure `ClientSecret` 'da yayımladıktan sonra Google API konsolundaki öğesini sıfırlayın.</span><span class="sxs-lookup"><span data-stu-id="972df-143">Once you publish the app to Azure, reset the `ClientSecret` in the Google API Console.</span></span>
* <span data-ttu-id="972df-144">Ayarlama `Authentication:Google:ClientId` ve `Authentication:Google:ClientSecret` Azure portalında uygulama ayarları olarak.</span><span class="sxs-lookup"><span data-stu-id="972df-144">Set the `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="972df-145">Yapılandırma sistemi ortam değişkenlerinden anahtarları okumak için ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="972df-145">The configuration system is set up to read keys from environment variables.</span></span>
