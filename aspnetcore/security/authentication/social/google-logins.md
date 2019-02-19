---
title: ASP.NET core'da Google dış oturum açma Kurulumu
author: rick-anderson
description: Bu öğreticide, mevcut bir ASP.NET Core uygulamasına Google hesabı kullanıcı kimlik doğrulaması tümleştirmesini gösterilmektedir.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 1/11/2019
uid: security/authentication/google-logins
ms.openlocfilehash: 5b6bfaafba68eaf15a60b7c512a9e7406e3112ee
ms.sourcegitcommit: 6d8c362a3cb4c634e64cbb3d28069c36bc973581
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/19/2019
ms.locfileid: "56418672"
---
# <a name="google-external-login-setup-in-aspnet-core"></a><span data-ttu-id="bd849-103">ASP.NET core'da Google dış oturum açma Kurulumu</span><span class="sxs-lookup"><span data-stu-id="bd849-103">Google external login setup in ASP.NET Core</span></span>

<span data-ttu-id="bd849-104">Tarafından [Valeriy Novytskyy](https://github.com/01binary) ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="bd849-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="bd849-105">Google Ocak 2019 başladı [kapatma](https://developers.google.com/+/api-shutdown) Google + oturum açın ve geliştiricilerin konumsa için yeni bir Google oturum sistemde Mart tarafından.</span><span class="sxs-lookup"><span data-stu-id="bd849-105">In January 2019 Google started to [shut down](https://developers.google.com/+/api-shutdown) Google+ sign in and developers must move to a new Google sign in system by March.</span></span> <span data-ttu-id="bd849-106">ASP.NET Core 2.1 ve Google kimlik doğrulaması için 2.2 paketleri değişiklikleri uyum sağlamak için Şubat ayında güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="bd849-106">The ASP.NET Core 2.1 and 2.2 packages for Google Authentication will be updated in February to accommodate the changes.</span></span> <span data-ttu-id="bd849-107">Daha fazla bilgi ve ASP.NET Core için geçici risk azaltma işlemleri için bkz. [bu GitHub sorunu](https://github.com/aspnet/AspNetCore/issues/6486).</span><span class="sxs-lookup"><span data-stu-id="bd849-107">For more information and temporary mitigations for ASP.NET Core, see [this GitHub issue](https://github.com/aspnet/AspNetCore/issues/6486).</span></span> <span data-ttu-id="bd849-108">Bu öğreticide yeni Kurulum işlemine güncelleştirildi.</span><span class="sxs-lookup"><span data-stu-id="bd849-108">This tutorial has been updated with the new setup process.</span></span>

<span data-ttu-id="bd849-109">Bu öğreticide oluşturulan ASP.NET Core 2.2 projesini kullanarak kendi Google hesabıyla oturum açmasını sağlamak gösterilmektedir [önceki sayfaya](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="bd849-109">This tutorial shows you how to enable users to sign in with their Google account using the ASP.NET Core 2.2 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-a-google-api-console-project-and-client-id"></a><span data-ttu-id="bd849-110">Google API Konsolu proje ve istemci kodu oluşturma</span><span class="sxs-lookup"><span data-stu-id="bd849-110">Create a Google API Console project and client ID</span></span>

* <span data-ttu-id="bd849-111">Gidin [tümleştirme Google oturum açma web uygulamanıza](https://developers.google.com/identity/sign-in/web/devconsole-project) seçip **bir proje yapılandırma**.</span><span class="sxs-lookup"><span data-stu-id="bd849-111">Navigate to [Integrating Google Sign-In into your web app](https://developers.google.com/identity/sign-in/web/devconsole-project) and select **CONFIGURE A PROJECT**.</span></span>
* <span data-ttu-id="bd849-112">İçinde **OAuth istemcinizi yapılandırma** iletişim kutusunda **Web sunucusu**.</span><span class="sxs-lookup"><span data-stu-id="bd849-112">In the **Configure your OAuth client** dialog, select **Web server**.</span></span>
* <span data-ttu-id="bd849-113">İçinde **yetkili yeniden yönlendirme URI'leri** metin girişi kutusunu, yeniden yönlendirme URI'sini ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="bd849-113">In the **Authorized redirect URIs** text entry box, set the redirect URI.</span></span> <span data-ttu-id="bd849-114">Örneğin, `https://localhost:5001/signin-google`</span><span class="sxs-lookup"><span data-stu-id="bd849-114">For example, `https://localhost:5001/signin-google`</span></span>
* <span data-ttu-id="bd849-115">Kaydet **istemci kimliği** ve **gizli**.</span><span class="sxs-lookup"><span data-stu-id="bd849-115">Save the **Client ID** and **Client Secret**.</span></span>
* <span data-ttu-id="bd849-116">Sitenin dağıtım yaparken, yeni genel URL'den kaydetme **Google konsolunu**.</span><span class="sxs-lookup"><span data-stu-id="bd849-116">When deploying the site, register the new public url from the **Google Console**.</span></span>

## <a name="store-google-clientid-and-clientsecret"></a><span data-ttu-id="bd849-117">Store Google ClientID ve ClientSecret</span><span class="sxs-lookup"><span data-stu-id="bd849-117">Store Google ClientID and ClientSecret</span></span>

<span data-ttu-id="bd849-118">Google gibi hassas ayarlar Store `Client ID` ve `Client Secret` ile [gizli dizi Yöneticisi](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="bd849-118">Store sensitive settings such as the Google `Client ID` and `Client Secret` with the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="bd849-119">Bu öğreticinin amaçları doğrultusunda, belirteçleri ad `Authentication:Google:ClientId` ve `Authentication:Google:ClientSecret`:</span><span class="sxs-lookup"><span data-stu-id="bd849-119">For the purposes of this tutorial, name the tokens `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret`:</span></span>

```console
dotnet user-secrets set "Authentication:Google:ClientId" "X.apps.googleusercontent.com"
dotnet user-secrets set "Authentication:Google:ClientSecret" "<client secret>"
```

<span data-ttu-id="bd849-120">API kimlik bilgileri ve kullanımı ile yönetebileceğiniz [API Konsolu](https://console.developers.google.com/apis/dashboard).</span><span class="sxs-lookup"><span data-stu-id="bd849-120">You can manage your API credentials and usage in the [API Console](https://console.developers.google.com/apis/dashboard).</span></span>

## <a name="configure-google-authentication"></a><span data-ttu-id="bd849-121">Google kimlik doğrulamasını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="bd849-121">Configure Google authentication</span></span>

<span data-ttu-id="bd849-122">Google hizmetine ekleme `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="bd849-122">Add the Google service to `Startup.ConfigureServices`.</span></span>

[!INCLUDE [default settings configuration](includes/default-settings2-2.md)]

## <a name="sign-in-with-google"></a><span data-ttu-id="bd849-123">Google ile oturum aç</span><span class="sxs-lookup"><span data-stu-id="bd849-123">Sign in with Google</span></span>

* <span data-ttu-id="bd849-124">Uygulamayı çalıştırın ve tıklayın **oturum**.</span><span class="sxs-lookup"><span data-stu-id="bd849-124">Run the app and click **Log in**.</span></span> <span data-ttu-id="bd849-125">Google ile oturum açmak için bir seçenek görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="bd849-125">An option to sign in with Google appears.</span></span>
* <span data-ttu-id="bd849-126">Tıklayın **Google** düğmesi için Google kimlik doğrulaması için yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="bd849-126">Click the **Google** button, which redirects to Google for authentication.</span></span>
* <span data-ttu-id="bd849-127">Google kimlik bilgilerinizi girdikten sonra web sitesine yönlendirilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="bd849-127">After entering your Google credentials, you are redirected back to the web site.</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

<span data-ttu-id="bd849-128">Bkz: [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) Google kimlik doğrulama tarafından desteklenen yapılandırma seçenekleri hakkında daha fazla bilgi için API Başvurusu.</span><span class="sxs-lookup"><span data-stu-id="bd849-128">See the [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) API reference for more information on configuration options supported by Google authentication.</span></span> <span data-ttu-id="bd849-129">Bu kullanıcı ile ilgili farklı bilgi istemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="bd849-129">This can be used to request different information about the user.</span></span>

## <a name="change-the-default-callback-uri"></a><span data-ttu-id="bd849-130">Varsayılan geri arama URI'si değiştirme</span><span class="sxs-lookup"><span data-stu-id="bd849-130">Change the default callback URI</span></span>

<span data-ttu-id="bd849-131">URI segmenti `/signin-google` Google kimlik doğrulama sağlayıcısı varsayılan geri arama olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="bd849-131">The URI segment `/signin-google` is set as the default callback of the Google authentication provider.</span></span> <span data-ttu-id="bd849-132">Google kimlik doğrulaması ara yazılımı üzerinden devralınan yapılandırırken, varsayılan geri arama URI'si değiştirebilirsiniz [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) özelliği [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) sınıfı.</span><span class="sxs-lookup"><span data-stu-id="bd849-132">You can change the default callback URI while configuring the Google authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) class.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="bd849-133">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="bd849-133">Troubleshooting</span></span>

* <span data-ttu-id="bd849-134">Hataları almıyor olabilirler ve oturum açma işe yaramazsa, sorunu hata ayıklama daha kolay hale getirmek için geliştirme moduna geçin.</span><span class="sxs-lookup"><span data-stu-id="bd849-134">If the sign-in doesn't work and you aren't getting any errors, switch to development mode to make the issue easier to debug.</span></span>
* <span data-ttu-id="bd849-135">Kimlik çağırarak yapılandırılmazsa `services.AddIdentity` içinde `ConfigureServices`, sonuçları kimlik doğrulaması girişimi sırasında *ArgumentException: 'SignInScheme' seçeneği belirtilmelidir*.</span><span class="sxs-lookup"><span data-stu-id="bd849-135">If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate results in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="bd849-136">Bu öğreticide kullanılan proje şablonu, bu gerçekleştirilir sağlar.</span><span class="sxs-lookup"><span data-stu-id="bd849-136">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="bd849-137">Site veritabanı, ilk geçiş uygulayarak oluşturulmamış varsa *bir veritabanı işlemi başarısız istek işlenirken* hata.</span><span class="sxs-lookup"><span data-stu-id="bd849-137">If the site database has not been created by applying the initial migration, you get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="bd849-138">Dokunun **geçerli geçişleri** veritabanı oluşturma ve hata devam etmek için yenilemek için.</span><span class="sxs-lookup"><span data-stu-id="bd849-138">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bd849-139">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="bd849-139">Next steps</span></span>

* <span data-ttu-id="bd849-140">Bu makalede, Google ile kimliğini nasıl doğrulayabileceğiniz gösterdi.</span><span class="sxs-lookup"><span data-stu-id="bd849-140">This article showed how you can authenticate with Google.</span></span> <span data-ttu-id="bd849-141">Listelenen diğer sağlayıcıları ile kimlik doğrulaması için benzer bir yaklaşım izleyerek [önceki sayfaya](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="bd849-141">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>
* <span data-ttu-id="bd849-142">Uygulamayı Azure'a yayımlama sonra sıfırlama `ClientSecret` Google API konsolunda.</span><span class="sxs-lookup"><span data-stu-id="bd849-142">Once you publish the app to Azure, reset the `ClientSecret` in the Google API Console.</span></span>
* <span data-ttu-id="bd849-143">Ayarlama `Authentication:Google:ClientId` ve `Authentication:Google:ClientSecret` Azure portalında uygulama ayarları olarak.</span><span class="sxs-lookup"><span data-stu-id="bd849-143">Set the `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="bd849-144">Yapılandırma sistemi ortam değişkenlerinden anahtarları okumak için ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="bd849-144">The configuration system is set up to read keys from environment variables.</span></span>
