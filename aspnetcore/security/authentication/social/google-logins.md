---
title: ASP.NET core'da Google dış oturum açma Kurulumu
author: rick-anderson
description: Bu öğreticide, mevcut bir ASP.NET Core uygulamasına Google hesabı kullanıcı kimlik doğrulaması tümleştirmesini gösterilmektedir.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 1/11/2019
uid: security/authentication/google-logins
ms.openlocfilehash: 44c79b3279db7946b6d89a726bd3f5acfb5f51af
ms.sourcegitcommit: 78339e9891c8676db01a6e81e9cb0cdaa280162f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59705597"
---
# <a name="google-external-login-setup-in-aspnet-core"></a><span data-ttu-id="6d933-103">ASP.NET core'da Google dış oturum açma Kurulumu</span><span class="sxs-lookup"><span data-stu-id="6d933-103">Google external login setup in ASP.NET Core</span></span>

<span data-ttu-id="6d933-104">Tarafından [Valeriy Novytskyy](https://github.com/01binary) ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="6d933-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="6d933-105">[Eski Google + API kapatma 7 Mart 2019 tarihinde](https://developers.google.com/+/api-shutdown).</span><span class="sxs-lookup"><span data-stu-id="6d933-105">[Legacy Google+ APIs have been shut down as of March 7, 2019](https://developers.google.com/+/api-shutdown).</span></span> <span data-ttu-id="6d933-106">Google + oturum açın ve geliştiriciler için yeni bir Google oturum sistemine taşımanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="6d933-106">Google+ sign in and developers must move to a new Google sign in system.</span></span> <span data-ttu-id="6d933-107">Google kimlik doğrulaması için ASP.NET Core 2.1 ve 2.2 paketleri sahip güncelleştirilmesi değişiklikleri uyum sağlamak için.</span><span class="sxs-lookup"><span data-stu-id="6d933-107">The ASP.NET Core 2.1 and 2.2 packages for Google Authentication have be updated to accommodate the changes.</span></span> <span data-ttu-id="6d933-108">Daha fazla bilgi ve ASP.NET Core için geçici risk azaltma işlemleri için bkz. [bu GitHub sorunu](https://github.com/aspnet/AspNetCore/issues/6486).</span><span class="sxs-lookup"><span data-stu-id="6d933-108">For more information and temporary mitigations for ASP.NET Core, see [this GitHub issue](https://github.com/aspnet/AspNetCore/issues/6486).</span></span> <span data-ttu-id="6d933-109">Bu öğreticide yeni Kurulum işlemine güncelleştirildi.</span><span class="sxs-lookup"><span data-stu-id="6d933-109">This tutorial has been updated with the new setup process.</span></span>

<span data-ttu-id="6d933-110">Bu öğreticide oluşturulan ASP.NET Core 2.2 projesini kullanarak kendi Google hesabıyla oturum açmasını sağlamak gösterilmektedir [önceki sayfaya](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="6d933-110">This tutorial shows you how to enable users to sign in with their Google account using the ASP.NET Core 2.2 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-a-google-api-console-project-and-client-id"></a><span data-ttu-id="6d933-111">Google API Konsolu proje ve istemci kodu oluşturma</span><span class="sxs-lookup"><span data-stu-id="6d933-111">Create a Google API Console project and client ID</span></span>

* <span data-ttu-id="6d933-112">Gidin [tümleştirme Google oturum açma web uygulamanıza](https://developers.google.com/identity/sign-in/web/devconsole-project) seçip **bir proje yapılandırma**.</span><span class="sxs-lookup"><span data-stu-id="6d933-112">Navigate to [Integrating Google Sign-In into your web app](https://developers.google.com/identity/sign-in/web/devconsole-project) and select **CONFIGURE A PROJECT**.</span></span>
* <span data-ttu-id="6d933-113">İçinde **OAuth istemcinizi yapılandırma** iletişim kutusunda **Web sunucusu**.</span><span class="sxs-lookup"><span data-stu-id="6d933-113">In the **Configure your OAuth client** dialog, select **Web server**.</span></span>
* <span data-ttu-id="6d933-114">İçinde **yetkili yeniden yönlendirme URI'leri** metin girişi kutusunu, yeniden yönlendirme URI'sini ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="6d933-114">In the **Authorized redirect URIs** text entry box, set the redirect URI.</span></span> <span data-ttu-id="6d933-115">Örneğin, `https://localhost:5001/signin-google`</span><span class="sxs-lookup"><span data-stu-id="6d933-115">For example, `https://localhost:5001/signin-google`</span></span>
* <span data-ttu-id="6d933-116">Kaydet **istemci kimliği** ve **gizli**.</span><span class="sxs-lookup"><span data-stu-id="6d933-116">Save the **Client ID** and **Client Secret**.</span></span>
* <span data-ttu-id="6d933-117">Sitenin dağıtım yaparken, yeni genel URL'den kaydetme **Google konsolunu**.</span><span class="sxs-lookup"><span data-stu-id="6d933-117">When deploying the site, register the new public url from the **Google Console**.</span></span>

## <a name="store-google-clientid-and-clientsecret"></a><span data-ttu-id="6d933-118">Store Google ClientID ve ClientSecret</span><span class="sxs-lookup"><span data-stu-id="6d933-118">Store Google ClientID and ClientSecret</span></span>

<span data-ttu-id="6d933-119">Google gibi hassas ayarlar Store `Client ID` ve `Client Secret` ile [gizli dizi Yöneticisi](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="6d933-119">Store sensitive settings such as the Google `Client ID` and `Client Secret` with the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="6d933-120">Bu öğreticinin amaçları doğrultusunda, belirteçleri ad `Authentication:Google:ClientId` ve `Authentication:Google:ClientSecret`:</span><span class="sxs-lookup"><span data-stu-id="6d933-120">For the purposes of this tutorial, name the tokens `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret`:</span></span>

```console
dotnet user-secrets set "Authentication:Google:ClientId" "X.apps.googleusercontent.com"
dotnet user-secrets set "Authentication:Google:ClientSecret" "<client secret>"
```

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="6d933-121">API kimlik bilgileri ve kullanımı ile yönetebileceğiniz [API Konsolu](https://console.developers.google.com/apis/dashboard).</span><span class="sxs-lookup"><span data-stu-id="6d933-121">You can manage your API credentials and usage in the [API Console](https://console.developers.google.com/apis/dashboard).</span></span>

## <a name="configure-google-authentication"></a><span data-ttu-id="6d933-122">Google kimlik doğrulamasını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="6d933-122">Configure Google authentication</span></span>

<span data-ttu-id="6d933-123">Google hizmetine ekleme `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="6d933-123">Add the Google service to `Startup.ConfigureServices`.</span></span>

[!INCLUDE [default settings configuration](includes/default-settings2-2.md)]

## <a name="sign-in-with-google"></a><span data-ttu-id="6d933-124">Google ile oturum aç</span><span class="sxs-lookup"><span data-stu-id="6d933-124">Sign in with Google</span></span>

* <span data-ttu-id="6d933-125">Uygulamayı çalıştırın ve tıklayın **oturum**.</span><span class="sxs-lookup"><span data-stu-id="6d933-125">Run the app and click **Log in**.</span></span> <span data-ttu-id="6d933-126">Google ile oturum açmak için bir seçenek görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="6d933-126">An option to sign in with Google appears.</span></span>
* <span data-ttu-id="6d933-127">Tıklayın **Google** düğmesi için Google kimlik doğrulaması için yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="6d933-127">Click the **Google** button, which redirects to Google for authentication.</span></span>
* <span data-ttu-id="6d933-128">Google kimlik bilgilerinizi girdikten sonra web sitesine yönlendirilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6d933-128">After entering your Google credentials, you are redirected back to the web site.</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

<span data-ttu-id="6d933-129">Bkz: [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) Google kimlik doğrulama tarafından desteklenen yapılandırma seçenekleri hakkında daha fazla bilgi için API Başvurusu.</span><span class="sxs-lookup"><span data-stu-id="6d933-129">See the [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) API reference for more information on configuration options supported by Google authentication.</span></span> <span data-ttu-id="6d933-130">Bu kullanıcı ile ilgili farklı bilgi istemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="6d933-130">This can be used to request different information about the user.</span></span>

## <a name="change-the-default-callback-uri"></a><span data-ttu-id="6d933-131">Varsayılan geri arama URI'si değiştirme</span><span class="sxs-lookup"><span data-stu-id="6d933-131">Change the default callback URI</span></span>

<span data-ttu-id="6d933-132">URI segmenti `/signin-google` Google kimlik doğrulama sağlayıcısı varsayılan geri arama olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="6d933-132">The URI segment `/signin-google` is set as the default callback of the Google authentication provider.</span></span> <span data-ttu-id="6d933-133">Google kimlik doğrulaması ara yazılımı üzerinden devralınan yapılandırırken, varsayılan geri arama URI'si değiştirebilirsiniz [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) özelliği [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) sınıfı.</span><span class="sxs-lookup"><span data-stu-id="6d933-133">You can change the default callback URI while configuring the Google authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) class.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="6d933-134">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="6d933-134">Troubleshooting</span></span>

* <span data-ttu-id="6d933-135">Hataları almıyor olabilirler ve oturum açma işe yaramazsa, sorunu hata ayıklama daha kolay hale getirmek için geliştirme moduna geçin.</span><span class="sxs-lookup"><span data-stu-id="6d933-135">If the sign-in doesn't work and you aren't getting any errors, switch to development mode to make the issue easier to debug.</span></span>
* <span data-ttu-id="6d933-136">Kimlik çağırarak yapılandırılmazsa `services.AddIdentity` içinde `ConfigureServices`, sonuçları kimlik doğrulaması girişimi sırasında *ArgumentException: 'SignInScheme' seçeneği belirtilmelidir*.</span><span class="sxs-lookup"><span data-stu-id="6d933-136">If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate results in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="6d933-137">Bu öğreticide kullanılan proje şablonu, bu gerçekleştirilir sağlar.</span><span class="sxs-lookup"><span data-stu-id="6d933-137">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="6d933-138">Site veritabanı, ilk geçiş uygulayarak oluşturulmamış varsa *bir veritabanı işlemi başarısız istek işlenirken* hata.</span><span class="sxs-lookup"><span data-stu-id="6d933-138">If the site database has not been created by applying the initial migration, you get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="6d933-139">Dokunun **geçerli geçişleri** veritabanı oluşturma ve hata devam etmek için yenilemek için.</span><span class="sxs-lookup"><span data-stu-id="6d933-139">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6d933-140">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="6d933-140">Next steps</span></span>

* <span data-ttu-id="6d933-141">Bu makalede, Google ile kimliğini nasıl doğrulayabileceğiniz gösterdi.</span><span class="sxs-lookup"><span data-stu-id="6d933-141">This article showed how you can authenticate with Google.</span></span> <span data-ttu-id="6d933-142">Listelenen diğer sağlayıcıları ile kimlik doğrulaması için benzer bir yaklaşım izleyerek [önceki sayfaya](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="6d933-142">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>
* <span data-ttu-id="6d933-143">Uygulamayı Azure'a yayımlama sonra sıfırlama `ClientSecret` Google API konsolunda.</span><span class="sxs-lookup"><span data-stu-id="6d933-143">Once you publish the app to Azure, reset the `ClientSecret` in the Google API Console.</span></span>
* <span data-ttu-id="6d933-144">Ayarlama `Authentication:Google:ClientId` ve `Authentication:Google:ClientSecret` Azure portalında uygulama ayarları olarak.</span><span class="sxs-lookup"><span data-stu-id="6d933-144">Set the `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="6d933-145">Yapılandırma sistemi ortam değişkenlerinden anahtarları okumak için ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="6d933-145">The configuration system is set up to read keys from environment variables.</span></span>
