---
title: ASP.NET Core ile Microsoft hesabı dış oturum açma kurulumu
author: rick-anderson
description: Bu örnek, Microsoft hesabı kullanıcı kimlik doğrulamasının mevcut bir ASP.NET Core uygulamasına tümleştirilmesini gösterir.
ms.author: riande
ms.custom: mvc
ms.date: 05/11/2019
uid: security/authentication/microsoft-logins
ms.openlocfilehash: 91ace293fd16cd180b3d5c183c637af6db1d08c3
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71082335"
---
# <a name="microsoft-account-external-login-setup-with-aspnet-core"></a><span data-ttu-id="14599-103">ASP.NET Core ile Microsoft hesabı dış oturum açma kurulumu</span><span class="sxs-lookup"><span data-stu-id="14599-103">Microsoft Account external login setup with ASP.NET Core</span></span>

<span data-ttu-id="14599-104">Tarafından [Valeriy Novytskyy](https://github.com/01binary) ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="14599-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="14599-105">Bu örnekte, kullanıcıların [önceki sayfada](xref:security/authentication/social/index)oluşturulan ASP.NET Core 2,2 projesini kullanarak Microsoft hesabı oturum açmasını nasıl etkinleştireceğinizi gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="14599-105">This sample shows you how to enable users to sign in with their Microsoft account using the ASP.NET Core 2.2 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-the-app-in-microsoft-developer-portal"></a><span data-ttu-id="14599-106">Uygulamayı Microsoft Geliştirici Portalında oluşturma</span><span class="sxs-lookup"><span data-stu-id="14599-106">Create the app in Microsoft Developer Portal</span></span>

* <span data-ttu-id="14599-107">[Azure portal uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) sayfasına gidin ve bir Microsoft hesabı oluşturun veya oturum açın:</span><span class="sxs-lookup"><span data-stu-id="14599-107">Navigate to the [Azure portal - App registrations](https://go.microsoft.com/fwlink/?linkid=2083908) page and create or sign into a Microsoft account:</span></span>

<span data-ttu-id="14599-108">Microsoft hesabı yoksa **bir tane oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="14599-108">If you don't have a Microsoft account, select **Create one**.</span></span> <span data-ttu-id="14599-109">Oturum açtıktan sonra **uygulama kayıtları** sayfasına yönlendirilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="14599-109">After signing in you are redirected to the **App registrations** page:</span></span>

* <span data-ttu-id="14599-110">**Yeni kayıt** Seç</span><span class="sxs-lookup"><span data-stu-id="14599-110">Select **New registration**</span></span>
* <span data-ttu-id="14599-111">Bir **ad**girin.</span><span class="sxs-lookup"><span data-stu-id="14599-111">Enter a **Name**.</span></span>
* <span data-ttu-id="14599-112">**Desteklenen hesap türleri**için bir seçenek belirleyin.</span><span class="sxs-lookup"><span data-stu-id="14599-112">Select an option for **Supported account types**.</span></span>  <!-- Accounts for any org work with MS domain accounts. Most folks probably want the last option, personal MS accounts -->
* <span data-ttu-id="14599-113">**Yeniden yönlendirme URI 'si**altında, `/signin-microsoft` eklenen geliştirme URL 'nizi girin.</span><span class="sxs-lookup"><span data-stu-id="14599-113">Under **Redirect URI**, enter your development URL with `/signin-microsoft` appended.</span></span> <span data-ttu-id="14599-114">Örneğin: `https://localhost:44389/signin-microsoft`.</span><span class="sxs-lookup"><span data-stu-id="14599-114">For example, `https://localhost:44389/signin-microsoft`.</span></span> <span data-ttu-id="14599-115">Bu örnekte daha sonra yapılandırılan Microsoft kimlik doğrulama şeması, OAuth akışını uygulamak için `/signin-microsoft` rotadaki istekleri otomatik olarak işleymeyecektir.</span><span class="sxs-lookup"><span data-stu-id="14599-115">The Microsoft authentication scheme configured later in this sample will automatically handle requests at `/signin-microsoft` route to implement the OAuth flow.</span></span>
* <span data-ttu-id="14599-116">**Kaydol** ' u seçin</span><span class="sxs-lookup"><span data-stu-id="14599-116">Select **Register**</span></span>

### <a name="create-client-secret"></a><span data-ttu-id="14599-117">İstemci parolası oluştur</span><span class="sxs-lookup"><span data-stu-id="14599-117">Create client secret</span></span>

* <span data-ttu-id="14599-118">Sol bölmede **sertifikalar & gizlilikler**' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="14599-118">In the left pane, select **Certificates & secrets**.</span></span>
* <span data-ttu-id="14599-119">**İstemci gizli**dizileri altında **yeni istemci parolası** ' nı seçin.</span><span class="sxs-lookup"><span data-stu-id="14599-119">Under **Client secrets**, select **New client secret**</span></span>

  * <span data-ttu-id="14599-120">İstemci parolası için bir açıklama ekleyin.</span><span class="sxs-lookup"><span data-stu-id="14599-120">Add a description for the client secret.</span></span>
  * <span data-ttu-id="14599-121">**Ekle** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="14599-121">Select the **Add** button.</span></span>

* <span data-ttu-id="14599-122">**İstemci**gizli dizileri altında, istemci parolasının değerini kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="14599-122">Under **Client secrets**, copy the value of the client secret.</span></span>

> [!NOTE]
> <span data-ttu-id="14599-123">URI segmenti `/signin-microsoft` , Microsoft kimlik doğrulama sağlayıcısı 'nın varsayılan geri çağırması olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="14599-123">The URI segment `/signin-microsoft` is set as the default callback of the Microsoft authentication provider.</span></span> <span data-ttu-id="14599-124">Microsoft kimlik doğrulama ara yazılımını, [Microsoftaccountoptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions) sınıfının devralınmış [remoteauthenticationoptions. callbackpath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) özelliği aracılığıyla YAPıLANDıRıRKEN varsayılan geri çağırma URI 'sini değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="14599-124">You can change the default callback URI while configuring the Microsoft authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions) class.</span></span>

## <a name="store-the-microsoft-client-id-and-client-secret"></a><span data-ttu-id="14599-125">Microsoft istemci KIMLIĞINI ve istemci gizli anahtarını depolayın</span><span class="sxs-lookup"><span data-stu-id="14599-125">Store the Microsoft client ID and client secret</span></span>

<span data-ttu-id="14599-126">`ClientId` [Gizli yönetimi](xref:security/app-secrets)güvenli bir şekilde depolamak ve `ClientSecret` kullanmak için aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="14599-126">Run the following commands to securely store `ClientId` and `ClientSecret` using [Secret Manager](xref:security/app-secrets):</span></span>

```dotnetcli
dotnet user-secrets set Authentication:Microsoft:ClientId <Client-Id>
dotnet user-secrets set Authentication:Microsoft:ClientSecret <Client-Secret>
```

<span data-ttu-id="14599-127">Gizli ayarları, `ClientId` [gizli Yöneticisi](xref:security/app-secrets)kullanarak `ClientSecret` Microsoft ve uygulama yapılandırmanıza bağlayın.</span><span class="sxs-lookup"><span data-stu-id="14599-127">Link sensitive settings like Microsoft `ClientId` and `ClientSecret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="14599-128">Bu örneğin amaçları doğrultusunda belirteçleri `Authentication:Microsoft:ClientId` ve `Authentication:Microsoft:ClientSecret`' ı adlandırın.</span><span class="sxs-lookup"><span data-stu-id="14599-128">For the purposes of this sample, name the tokens `Authentication:Microsoft:ClientId` and `Authentication:Microsoft:ClientSecret`.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="configure-microsoft-account-authentication"></a><span data-ttu-id="14599-129">Microsoft hesabı kimlik doğrulamasını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="14599-129">Configure Microsoft Account Authentication</span></span>

<span data-ttu-id="14599-130">Microsoft hesabı hizmetini şu şekilde `Startup.ConfigureServices`ekleyin:</span><span class="sxs-lookup"><span data-stu-id="14599-130">Add the Microsoft Account service to `Startup.ConfigureServices`:</span></span>

[!code-csharp[](~/security/authentication/social/social-code/StartupMS.cs?name=snippet&highlight=10-14)]

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

<span data-ttu-id="14599-131">Microsoft hesabı kimlik doğrulaması tarafından desteklenen yapılandırma seçenekleri hakkında daha fazla bilgi için bkz. [Microsoftaccountoptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) API başvurusu.</span><span class="sxs-lookup"><span data-stu-id="14599-131">See the [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) API reference for more information on configuration options supported by Microsoft Account authentication.</span></span> <span data-ttu-id="14599-132">Bu kullanıcı ile ilgili farklı bilgi istemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="14599-132">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-microsoft-account"></a><span data-ttu-id="14599-133">Microsoft hesabıyla oturum açın hesabı</span><span class="sxs-lookup"><span data-stu-id="14599-133">Sign in with Microsoft Account</span></span>

<span data-ttu-id="14599-134">Öğesini çalıştırın ve **oturum aç**' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="14599-134">Run the and click **Log in**.</span></span> <span data-ttu-id="14599-135">Microsoft ile oturum açma seçeneği görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="14599-135">An option to sign in with Microsoft appears.</span></span> <span data-ttu-id="14599-136">Microsoft 'a tıkladığınızda, kimlik doğrulaması için Microsoft 'a yönlendirilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="14599-136">When you click on Microsoft, you are redirected to Microsoft for authentication.</span></span> <span data-ttu-id="14599-137">Microsoft hesabınızla oturum açtıktan sonra (henüz oturum açmadıysanız), uygulamanın bilgilerinize erişmesine izin vermek isteyip istemediğiniz sorulur:</span><span class="sxs-lookup"><span data-stu-id="14599-137">After signing in with your Microsoft Account (if not already signed in) you will be prompted to let the app access your info:</span></span>

<span data-ttu-id="14599-138">**Evet** ' e dokunduktan sonra, e-postanızı ayarlayabileceğiniz Web sitesine geri yönlendirilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="14599-138">Tap **Yes** and you will be redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="14599-139">Şu anda Microsoft kimlik bilgilerinizi kullanarak oturum açtınız:</span><span class="sxs-lookup"><span data-stu-id="14599-139">You are now logged in using your Microsoft credentials:</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="14599-140">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="14599-140">Troubleshooting</span></span>

* <span data-ttu-id="14599-141">Microsoft hesabı sağlayıcısı sizi bir oturum açma hatası sayfasına yönlendirirse, URI 'deki `#` (hashtag) hata başlığına ve açıklama sorgu dizesi parametrelerine göz önüne alın.</span><span class="sxs-lookup"><span data-stu-id="14599-141">If the Microsoft Account provider redirects you to a sign in error page, note the error title and description query string parameters directly following the `#` (hashtag) in the Uri.</span></span>

  <span data-ttu-id="14599-142">Hata iletisi Microsoft kimlik doğrulamasıyla ilgili bir sorun olduğunu gösteriyorsa, en sık karşılaşılan neden, uygulama URI 'niz **Web** platformu Için belirtilen **yeniden yönlendirme URI** 'lerinden hiçbiriyle eşleşmemedir.</span><span class="sxs-lookup"><span data-stu-id="14599-142">Although the error message seems to indicate a problem with Microsoft authentication, the most common cause is your application Uri not matching any of the **Redirect URIs** specified for the **Web** platform.</span></span>
* <span data-ttu-id="14599-143">Kimlik ' de `services.AddIdentity` `ConfigureServices`çağırarak yapılandırılmamışsa, kimlik doğrulaması girişimi *ArgumentException ile sonuçlanır: ' Signınscheme ' seçeneği sağlanmalıdır*.</span><span class="sxs-lookup"><span data-stu-id="14599-143">If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="14599-144">Bu örnekte kullanılan proje şablonu bunun yapılmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="14599-144">The project template used in this sample ensures that this is done.</span></span>
* <span data-ttu-id="14599-145">Site veritabanı, ilk geçiş uygulayarak oluşturulmamış alırsa *bir veritabanı işlemi başarısız istek işlenirken* hata.</span><span class="sxs-lookup"><span data-stu-id="14599-145">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="14599-146">Dokunun **geçerli geçişleri** veritabanı oluşturma ve hata devam etmek için yenilemek için.</span><span class="sxs-lookup"><span data-stu-id="14599-146">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="14599-147">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="14599-147">Next steps</span></span>

* <span data-ttu-id="14599-148">Bu makale, Microsoft ile nasıl kimlik doğrulayacağınızı gösterdi.</span><span class="sxs-lookup"><span data-stu-id="14599-148">This article showed how you can authenticate with Microsoft.</span></span> <span data-ttu-id="14599-149">Listelenen diğer sağlayıcıları ile kimlik doğrulaması için benzer bir yaklaşım izleyerek [önceki sayfaya](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="14599-149">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="14599-150">Web sitenizi Azure Web App 'e yayımladığınızda, Microsoft Geliştirici Portalında yeni bir istemci gizli dizileri oluşturun.</span><span class="sxs-lookup"><span data-stu-id="14599-150">Once you publish your web site to Azure web app, create a new client secrets in the Microsoft Developer Portal.</span></span>

* <span data-ttu-id="14599-151">Ayarlama `Authentication:Microsoft:ClientId` ve `Authentication:Microsoft:ClientSecret` Azure portalında uygulama ayarları olarak.</span><span class="sxs-lookup"><span data-stu-id="14599-151">Set the `Authentication:Microsoft:ClientId` and `Authentication:Microsoft:ClientSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="14599-152">Yapılandırma sistemi ortam değişkenlerinden anahtarları okumak için ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="14599-152">The configuration system is set up to read keys from environment variables.</span></span>