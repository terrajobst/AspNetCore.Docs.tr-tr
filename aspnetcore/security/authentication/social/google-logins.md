---
title: ASP.NET core'da Google dış oturum açma Kurulumu
author: rick-anderson
description: Bu öğreticide, mevcut bir ASP.NET Core uygulamasına Google hesabı kullanıcı kimlik doğrulaması tümleştirmesini gösterilmektedir.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 10/30/2019
uid: security/authentication/google-logins
ms.openlocfilehash: 83f45143eca1be43410880bfd875a3fce1d2e9c9
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78667512"
---
# <a name="google-external-login-setup-in-aspnet-core"></a><span data-ttu-id="af2a1-103">ASP.NET core'da Google dış oturum açma Kurulumu</span><span class="sxs-lookup"><span data-stu-id="af2a1-103">Google external login setup in ASP.NET Core</span></span>

<span data-ttu-id="af2a1-104">Tarafından [Valeriy Novyıtskyy](https://github.com/01binary) ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="af2a1-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="af2a1-105">Bu öğreticide, kullanıcıların [önceki sayfada](xref:security/authentication/social/index)oluşturulan ASP.NET Core 3,0 projesini kullanarak Google hesabıyla oturum açmasını nasıl etkinleştireceğinizi gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="af2a1-105">This tutorial shows you how to enable users to sign in with their Google account using the ASP.NET Core 3.0 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-a-google-api-console-project-and-client-id"></a><span data-ttu-id="af2a1-106">Google API konsol projesi ve istemci KIMLIĞI oluşturma</span><span class="sxs-lookup"><span data-stu-id="af2a1-106">Create a Google API Console project and client ID</span></span>

* <span data-ttu-id="af2a1-107">[Microsoft. AspNetCore. Authentication. Google](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google)'ı yükler.</span><span class="sxs-lookup"><span data-stu-id="af2a1-107">Install [Microsoft.AspNetCore.Authentication.Google](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google).</span></span>
* <span data-ttu-id="af2a1-108">[Google oturum açma ile Web uygulamanızı tümleştirme](https://developers.google.com/identity/sign-in/web/devconsole-project) ve **Proje yapılandırma**' yı seçme bölümüne gidin.</span><span class="sxs-lookup"><span data-stu-id="af2a1-108">Navigate to [Integrating Google Sign-In into your web app](https://developers.google.com/identity/sign-in/web/devconsole-project) and select **CONFIGURE A PROJECT**.</span></span>
* <span data-ttu-id="af2a1-109">**OAuth Istemcinizi yapılandırın** Iletişim kutusunda **Web sunucusu**' nu seçin.</span><span class="sxs-lookup"><span data-stu-id="af2a1-109">In the **Configure your OAuth client** dialog, select **Web server**.</span></span>
* <span data-ttu-id="af2a1-110">**Yetkili yeniden yönlendirme URI 'leri** metin girişi kutusunda, YENIDEN yönlendirme URI 'sini ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="af2a1-110">In the **Authorized redirect URIs** text entry box, set the redirect URI.</span></span> <span data-ttu-id="af2a1-111">Örneğin, `https://localhost:44312/signin-google`</span><span class="sxs-lookup"><span data-stu-id="af2a1-111">For example, `https://localhost:44312/signin-google`</span></span>
* <span data-ttu-id="af2a1-112">**ISTEMCI kimliğini** ve **istemci parolasını**kaydedin.</span><span class="sxs-lookup"><span data-stu-id="af2a1-112">Save the **Client ID** and **Client Secret**.</span></span>
* <span data-ttu-id="af2a1-113">Siteyi dağıttığınızda, **Google konsolundan**yeni ortak URL 'yi kaydedin.</span><span class="sxs-lookup"><span data-stu-id="af2a1-113">When deploying the site, register the new public url from the **Google Console**.</span></span>

## <a name="store-google-clientid-and-clientsecret"></a><span data-ttu-id="af2a1-114">Store Google ClientID ve ClientSecret</span><span class="sxs-lookup"><span data-stu-id="af2a1-114">Store Google ClientID and ClientSecret</span></span>

<span data-ttu-id="af2a1-115">Google `Client ID` ve `Client Secret` gibi hassas ayarları [gizli yönetici](xref:security/app-secrets)ile depolayın.</span><span class="sxs-lookup"><span data-stu-id="af2a1-115">Store sensitive settings such as the Google `Client ID` and `Client Secret` with the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="af2a1-116">Bu öğreticinin amaçları doğrultusunda belirteçleri `Authentication:Google:ClientId` ve `Authentication:Google:ClientSecret`olarak adlandırın:</span><span class="sxs-lookup"><span data-stu-id="af2a1-116">For the purposes of this tutorial, name the tokens `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret`:</span></span>

```dotnetcli
dotnet user-secrets set "Authentication:Google:ClientId" "<client id>"
dotnet user-secrets set "Authentication:Google:ClientSecret" "<client secret>"
```

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="af2a1-117">API [konsolunuzun](https://console.developers.google.com/apis/dashboard)API kimlik bilgilerini ve kullanımını yönetebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="af2a1-117">You can manage your API credentials and usage in the [API Console](https://console.developers.google.com/apis/dashboard).</span></span>

## <a name="configure-google-authentication"></a><span data-ttu-id="af2a1-118">Google kimlik doğrulamasını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="af2a1-118">Configure Google authentication</span></span>

<span data-ttu-id="af2a1-119">Google Service 'i `Startup.ConfigureServices`ekleyin:</span><span class="sxs-lookup"><span data-stu-id="af2a1-119">Add the Google service to `Startup.ConfigureServices`:</span></span>

[!code-csharp[](~/security/authentication/social/social-code/3.x/StartupGoogle3x.cs?highlight=11-19)]

[!INCLUDE [default settings configuration](includes/default-settings2-2.md)]

## <a name="sign-in-with-google"></a><span data-ttu-id="af2a1-120">Google ile oturum aç</span><span class="sxs-lookup"><span data-stu-id="af2a1-120">Sign in with Google</span></span>

* <span data-ttu-id="af2a1-121">Uygulamayı çalıştırın ve **oturum aç**' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="af2a1-121">Run the app and click **Log in**.</span></span> <span data-ttu-id="af2a1-122">Google ile oturum açma seçeneği görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="af2a1-122">An option to sign in with Google appears.</span></span>
* <span data-ttu-id="af2a1-123">Kimlik doğrulaması için Google 'a yönlendiren **Google** düğmesine tıklayın.</span><span class="sxs-lookup"><span data-stu-id="af2a1-123">Click the **Google** button, which redirects to Google for authentication.</span></span>
* <span data-ttu-id="af2a1-124">Google kimlik bilgilerinizi girdikten sonra, Web sitesine geri yönlendirilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="af2a1-124">After entering your Google credentials, you are redirected back to the web site.</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

<span data-ttu-id="af2a1-125">Google kimlik doğrulaması tarafından desteklenen yapılandırma seçenekleri hakkında daha fazla bilgi için <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions> API başvurusuna bakın.</span><span class="sxs-lookup"><span data-stu-id="af2a1-125">See the <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions> API reference for more information on configuration options supported by Google authentication.</span></span> <span data-ttu-id="af2a1-126">Bu kullanıcı ile ilgili farklı bilgi istemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="af2a1-126">This can be used to request different information about the user.</span></span>

## <a name="change-the-default-callback-uri"></a><span data-ttu-id="af2a1-127">Varsayılan geri çağırma URI 'sini değiştirme</span><span class="sxs-lookup"><span data-stu-id="af2a1-127">Change the default callback URI</span></span>

<span data-ttu-id="af2a1-128">`/signin-google` URI segmenti, Google kimlik doğrulama sağlayıcısı 'nın varsayılan geri çağırması olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="af2a1-128">The URI segment `/signin-google` is set as the default callback of the Google authentication provider.</span></span> <span data-ttu-id="af2a1-129">[GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) sınıfının devralınmış [remoteauthenticationoptions. callbackpath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) özelliği aracılığıyla Google kimlik doğrulama ara yazılımını YAPıLANDıRıRKEN varsayılan geri çağırma URI 'sini değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="af2a1-129">You can change the default callback URI while configuring the Google authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) class.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="af2a1-130">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="af2a1-130">Troubleshooting</span></span>

* <span data-ttu-id="af2a1-131">Oturum açma işe yaramazsa ve herhangi bir hata alamıyorsanız, sorunu ayıklamanın daha kolay olması için geliştirme moduna geçin.</span><span class="sxs-lookup"><span data-stu-id="af2a1-131">If the sign-in doesn't work and you aren't getting any errors, switch to development mode to make the issue easier to debug.</span></span>
* <span data-ttu-id="af2a1-132">Kimlik `ConfigureServices``services.AddIdentity` çağırarak, ArgumentException 'de sonuçların doğrulanması denenerek, *' Signınscheme ' seçeneğinin sağlanması gerekir*.</span><span class="sxs-lookup"><span data-stu-id="af2a1-132">If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate results in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="af2a1-133">Bu öğreticide kullanılan proje şablonu, bu gerçekleştirilir sağlar.</span><span class="sxs-lookup"><span data-stu-id="af2a1-133">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="af2a1-134">Site veritabanı ilk geçiş uygulanarak oluşturulmadıysa, *istek hatasını Işlerken bir veritabanı işlemi başarısız oldu* .</span><span class="sxs-lookup"><span data-stu-id="af2a1-134">If the site database has not been created by applying the initial migration, you get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="af2a1-135">Veritabanını oluşturmak için **geçişleri Uygula** ' yı seçin ve hatanın ötesinde devam etmek için sayfayı yenileyin.</span><span class="sxs-lookup"><span data-stu-id="af2a1-135">Select **Apply Migrations** to create the database, and refresh the page to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="af2a1-136">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="af2a1-136">Next steps</span></span>

* <span data-ttu-id="af2a1-137">Bu makalede, Google ile kimliğini nasıl doğrulayabileceğiniz gösterdi.</span><span class="sxs-lookup"><span data-stu-id="af2a1-137">This article showed how you can authenticate with Google.</span></span> <span data-ttu-id="af2a1-138">[Önceki sayfada](xref:security/authentication/social/index)listelenen diğer sağlayıcılarla kimlik doğrulaması yapmak için benzer bir yaklaşımı izleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="af2a1-138">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>
* <span data-ttu-id="af2a1-139">Uygulamayı Azure 'da yayımladıktan sonra, Google API konsolundaki `ClientSecret` sıfırlayın.</span><span class="sxs-lookup"><span data-stu-id="af2a1-139">Once you publish the app to Azure, reset the `ClientSecret` in the Google API Console.</span></span>
* <span data-ttu-id="af2a1-140">`Authentication:Google:ClientId` ve `Authentication:Google:ClientSecret` Azure portal uygulama ayarları olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="af2a1-140">Set the `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="af2a1-141">Yapılandırma sistemi ortam değişkenlerinden anahtarları okumak için ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="af2a1-141">The configuration system is set up to read keys from environment variables.</span></span>
