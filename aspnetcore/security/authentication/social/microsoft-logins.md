---
title: ASP.NET Core ile Microsoft hesabı dış oturum açma kurulumu
author: rick-anderson
description: Bu örnek, Microsoft hesabı kullanıcı kimlik doğrulamasının mevcut bir ASP.NET Core uygulamasına tümleştirilmesini gösterir.
ms.author: riande
ms.custom: mvc
ms.date: 12/4/2019
monikerRange: '>= aspnetcore-3.0'
uid: security/authentication/microsoft-logins
ms.openlocfilehash: ddaae1a25a1dcf167ffae0f24b480e2cde6aca5b
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78659798"
---
# <a name="microsoft-account-external-login-setup-with-aspnet-core"></a><span data-ttu-id="11bc6-103">ASP.NET Core ile Microsoft hesabı dış oturum açma kurulumu</span><span class="sxs-lookup"><span data-stu-id="11bc6-103">Microsoft Account external login setup with ASP.NET Core</span></span>

<span data-ttu-id="11bc6-104">Tarafından [Valeriy Novyıtskyy](https://github.com/01binary) ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="11bc6-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="11bc6-105">Bu örnekte, kullanıcıların [önceki sayfada](xref:security/authentication/social/index)oluşturulan ASP.NET Core 3,0 projesini kullanarak Microsoft hesabı oturum açmasını nasıl etkinleştireceğinizi gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="11bc6-105">This sample shows you how to enable users to sign in with their Microsoft account using the ASP.NET Core 3.0 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-the-app-in-microsoft-developer-portal"></a><span data-ttu-id="11bc6-106">Uygulamayı Microsoft Geliştirici Portalında oluşturma</span><span class="sxs-lookup"><span data-stu-id="11bc6-106">Create the app in Microsoft Developer Portal</span></span>

* <span data-ttu-id="11bc6-107">Projeye [Microsoft. AspNetCore. Authentication. MicrosoftAccount](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount/) NuGet paketini ekleyin.</span><span class="sxs-lookup"><span data-stu-id="11bc6-107">Add the [Microsoft.AspNetCore.Authentication.MicrosoftAccount](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount/) NuGet package to the project.</span></span>
* <span data-ttu-id="11bc6-108">[Azure portal uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) sayfasına gidin ve bir Microsoft hesabı oluşturun veya oturum açın:</span><span class="sxs-lookup"><span data-stu-id="11bc6-108">Navigate to the [Azure portal - App registrations](https://go.microsoft.com/fwlink/?linkid=2083908) page and create or sign into a Microsoft account:</span></span>

<span data-ttu-id="11bc6-109">Microsoft hesabı yoksa **bir tane oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="11bc6-109">If you don't have a Microsoft account, select **Create one**.</span></span> <span data-ttu-id="11bc6-110">Oturum açtıktan sonra, **uygulama kayıtları** sayfasına yönlendirilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="11bc6-110">After signing in, you are redirected to the **App registrations** page:</span></span>

* <span data-ttu-id="11bc6-111">**Yeni kayıt** Seç</span><span class="sxs-lookup"><span data-stu-id="11bc6-111">Select **New registration**</span></span>
* <span data-ttu-id="11bc6-112">Bir **ad**girin.</span><span class="sxs-lookup"><span data-stu-id="11bc6-112">Enter a **Name**.</span></span>
* <span data-ttu-id="11bc6-113">**Desteklenen hesap türleri**için bir seçenek belirleyin.</span><span class="sxs-lookup"><span data-stu-id="11bc6-113">Select an option for **Supported account types**.</span></span>  <!-- Accounts for any org work with MS domain accounts. Most folks probably want the last option, personal MS accounts -->
* <span data-ttu-id="11bc6-114">**Yeniden yönlendirme URI 'si**altında `/signin-microsoft` eklenen geliştirme URL 'nizi girin.</span><span class="sxs-lookup"><span data-stu-id="11bc6-114">Under **Redirect URI**, enter your development URL with `/signin-microsoft` appended.</span></span> <span data-ttu-id="11bc6-115">Örneğin, `https://localhost:5001/signin-microsoft`.</span><span class="sxs-lookup"><span data-stu-id="11bc6-115">For example, `https://localhost:5001/signin-microsoft`.</span></span> <span data-ttu-id="11bc6-116">Bu örnekte daha sonra yapılandırılan Microsoft kimlik doğrulama şeması, OAuth akışını uygulamak için `/signin-microsoft` rotadaki istekleri otomatik olarak işleymeyecektir.</span><span class="sxs-lookup"><span data-stu-id="11bc6-116">The Microsoft authentication scheme configured later in this sample will automatically handle requests at `/signin-microsoft` route to implement the OAuth flow.</span></span>
* <span data-ttu-id="11bc6-117">**Kaydol** ' u seçin</span><span class="sxs-lookup"><span data-stu-id="11bc6-117">Select **Register**</span></span>

### <a name="create-client-secret"></a><span data-ttu-id="11bc6-118">İstemci parolası oluştur</span><span class="sxs-lookup"><span data-stu-id="11bc6-118">Create client secret</span></span>

* <span data-ttu-id="11bc6-119">Sol bölmede **sertifikalar & gizlilikler**' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="11bc6-119">In the left pane, select **Certificates & secrets**.</span></span>
* <span data-ttu-id="11bc6-120">**İstemci gizli**dizileri altında **yeni istemci parolası** ' nı seçin.</span><span class="sxs-lookup"><span data-stu-id="11bc6-120">Under **Client secrets**, select **New client secret**</span></span>

  * <span data-ttu-id="11bc6-121">İstemci parolası için bir açıklama ekleyin.</span><span class="sxs-lookup"><span data-stu-id="11bc6-121">Add a description for the client secret.</span></span>
  * <span data-ttu-id="11bc6-122">**Ekle** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="11bc6-122">Select the **Add** button.</span></span>

* <span data-ttu-id="11bc6-123">**İstemci**gizli dizileri altında, istemci parolasının değerini kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="11bc6-123">Under **Client secrets**, copy the value of the client secret.</span></span>

> [!NOTE]
> <span data-ttu-id="11bc6-124">`/signin-microsoft` URI segmenti, Microsoft kimlik doğrulama sağlayıcısı 'nın varsayılan geri çağırması olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="11bc6-124">The URI segment `/signin-microsoft` is set as the default callback of the Microsoft authentication provider.</span></span> <span data-ttu-id="11bc6-125">Microsoft kimlik doğrulama ara yazılımını, [Microsoftaccountoptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions) sınıfının devralınmış [remoteauthenticationoptions. callbackpath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) özelliği aracılığıyla YAPıLANDıRıRKEN varsayılan geri çağırma URI 'sini değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="11bc6-125">You can change the default callback URI while configuring the Microsoft authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions) class.</span></span>

## <a name="store-the-microsoft-client-id-and-client-secret"></a><span data-ttu-id="11bc6-126">Microsoft istemci KIMLIĞINI ve istemci gizli anahtarını depolayın</span><span class="sxs-lookup"><span data-stu-id="11bc6-126">Store the Microsoft client ID and client secret</span></span>

<span data-ttu-id="11bc6-127">`ClientId` ve `ClientSecret` [gizli Yöneticisi](xref:security/app-secrets)kullanarak güvenli bir şekilde depolamak için aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="11bc6-127">Run the following commands to securely store `ClientId` and `ClientSecret` using [Secret Manager](xref:security/app-secrets):</span></span>

```dotnetcli
dotnet user-secrets set Authentication:Microsoft:ClientId <Client-Id>
dotnet user-secrets set Authentication:Microsoft:ClientSecret <Client-Secret>
```

<span data-ttu-id="11bc6-128">[Gizli](xref:security/app-secrets)ayarları kullanarak Microsoft `ClientId` ve `ClientSecret` gibi hassas ayarları uygulama yapılandırmanıza bağlayın.</span><span class="sxs-lookup"><span data-stu-id="11bc6-128">Link sensitive settings like Microsoft `ClientId` and `ClientSecret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="11bc6-129">Bu örneğin amaçları doğrultusunda belirteçleri `Authentication:Microsoft:ClientId` ve `Authentication:Microsoft:ClientSecret`olarak adlandırın.</span><span class="sxs-lookup"><span data-stu-id="11bc6-129">For the purposes of this sample, name the tokens `Authentication:Microsoft:ClientId` and `Authentication:Microsoft:ClientSecret`.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="configure-microsoft-account-authentication"></a><span data-ttu-id="11bc6-130">Microsoft hesabı kimlik doğrulamasını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="11bc6-130">Configure Microsoft Account Authentication</span></span>

<span data-ttu-id="11bc6-131">Microsoft hesabı hizmetini `Startup.ConfigureServices`ekleyin:</span><span class="sxs-lookup"><span data-stu-id="11bc6-131">Add the Microsoft Account service to the `Startup.ConfigureServices`:</span></span>

[!code-csharp[](~/security/authentication/social/social-code/3.x/StartupMS3x.cs?name=snippet&highlight=10-14)]

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

<span data-ttu-id="11bc6-132">Microsoft hesabı kimlik doğrulaması tarafından desteklenen yapılandırma seçenekleri hakkında daha fazla bilgi için bkz. [Microsoftaccountoptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) API başvurusu.</span><span class="sxs-lookup"><span data-stu-id="11bc6-132">For more information about configuration options supported by Microsoft Account authentication, see the [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) API reference.</span></span> <span data-ttu-id="11bc6-133">Bu kullanıcı ile ilgili farklı bilgi istemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="11bc6-133">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-microsoft-account"></a><span data-ttu-id="11bc6-134">Microsoft hesabıyla oturum açın hesabı</span><span class="sxs-lookup"><span data-stu-id="11bc6-134">Sign in with Microsoft Account</span></span>

<span data-ttu-id="11bc6-135">Uygulamayı çalıştırın ve **oturum aç**' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="11bc6-135">Run the app and click **Log in**.</span></span> <span data-ttu-id="11bc6-136">Microsoft ile oturum açma seçeneği görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="11bc6-136">An option to sign in with Microsoft appears.</span></span> <span data-ttu-id="11bc6-137">Microsoft 'a tıkladığınızda, kimlik doğrulaması için Microsoft 'a yönlendirilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="11bc6-137">When you click on Microsoft, you are redirected to Microsoft for authentication.</span></span> <span data-ttu-id="11bc6-138">Microsoft hesabınızla oturum açtıktan sonra uygulamanın bilgilerinize erişmesine izin vermek isteyip istemediğiniz sorulur:</span><span class="sxs-lookup"><span data-stu-id="11bc6-138">After signing in with your Microsoft Account, you will be prompted to let the app access your info:</span></span>

<span data-ttu-id="11bc6-139">**Evet** ' e dokunduktan sonra, e-postanızı ayarlayabileceğiniz Web sitesine geri yönlendirilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="11bc6-139">Tap **Yes** and you will be redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="11bc6-140">Şu anda Microsoft kimlik bilgilerinizi kullanarak oturum açtınız:</span><span class="sxs-lookup"><span data-stu-id="11bc6-140">You are now logged in using your Microsoft credentials:</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="11bc6-141">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="11bc6-141">Troubleshooting</span></span>

* <span data-ttu-id="11bc6-142">Microsoft hesabı sağlayıcısı sizi bir oturum açma hatası sayfasına yönlendirirse, URI 'deki `#` (hashtag) doğrudan hata başlığına ve açıklama sorgu dizesi parametrelerine göz önüne alın.</span><span class="sxs-lookup"><span data-stu-id="11bc6-142">If the Microsoft Account provider redirects you to a sign in error page, note the error title and description query string parameters directly following the `#` (hashtag) in the Uri.</span></span>

  <span data-ttu-id="11bc6-143">Hata iletisi Microsoft kimlik doğrulamasıyla ilgili bir sorun olduğunu gösteriyorsa, en sık karşılaşılan neden, uygulama URI 'niz **Web** platformu Için belirtilen **yeniden yönlendirme URI** 'lerinden hiçbiriyle eşleşmemedir.</span><span class="sxs-lookup"><span data-stu-id="11bc6-143">Although the error message seems to indicate a problem with Microsoft authentication, the most common cause is your application Uri not matching any of the **Redirect URIs** specified for the **Web** platform.</span></span>
* <span data-ttu-id="11bc6-144">Kimlik, `ConfigureServices``services.AddIdentity` çağırarak yapılandırılmamışsa, kimlik doğrulamaya çalışmak ArgumentException ile sonuçlanır *: ' Signınscheme ' seçeneği sağlanmalıdır*.</span><span class="sxs-lookup"><span data-stu-id="11bc6-144">If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="11bc6-145">Bu örnekte kullanılan proje şablonu bunun yapılmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="11bc6-145">The project template used in this sample ensures that this is done.</span></span>
* <span data-ttu-id="11bc6-146">Site veritabanı ilk geçiş uygulanarak oluşturulmadıysa, *istek hatasını Işlerken bir veritabanı işlemi başarısız* olur.</span><span class="sxs-lookup"><span data-stu-id="11bc6-146">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="11bc6-147">Veritabanını oluşturmak için **geçişleri Uygula** ' ya dokunun ve hatanın ötesinde devam etmek için yenileyin.</span><span class="sxs-lookup"><span data-stu-id="11bc6-147">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="11bc6-148">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="11bc6-148">Next steps</span></span>

* <span data-ttu-id="11bc6-149">Bu makale, Microsoft ile nasıl kimlik doğrulayacağınızı gösterdi.</span><span class="sxs-lookup"><span data-stu-id="11bc6-149">This article showed how you can authenticate with Microsoft.</span></span> <span data-ttu-id="11bc6-150">[Önceki sayfada](xref:security/authentication/social/index)listelenen diğer sağlayıcılarla kimlik doğrulaması yapmak için benzer bir yaklaşımı izleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="11bc6-150">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="11bc6-151">Web sitenizi Azure Web App 'e yayımladığınızda, Microsoft Geliştirici Portalında yeni bir istemci gizli dizileri oluşturun.</span><span class="sxs-lookup"><span data-stu-id="11bc6-151">Once you publish your web site to Azure web app, create a new client secrets in the Microsoft Developer Portal.</span></span>

* <span data-ttu-id="11bc6-152">`Authentication:Microsoft:ClientId` ve `Authentication:Microsoft:ClientSecret` Azure portal uygulama ayarları olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="11bc6-152">Set the `Authentication:Microsoft:ClientId` and `Authentication:Microsoft:ClientSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="11bc6-153">Yapılandırma sistemi ortam değişkenlerinden anahtarları okumak için ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="11bc6-153">The configuration system is set up to read keys from environment variables.</span></span>
