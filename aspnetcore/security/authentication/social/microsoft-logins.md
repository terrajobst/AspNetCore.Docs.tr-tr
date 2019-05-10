---
title: ASP.NET Core ile Microsoft Account dış oturum açma Kurulumu
author: rick-anderson
description: Bu örnek, mevcut bir ASP.NET Core uygulamasına Microsoft hesabı kullanıcı kimlik doğrulaması tümleştirmesini gösterir.
ms.author: riande
ms.custom: mvc
ms.date: 5/11/2019
uid: security/authentication/microsoft-logins
ms.openlocfilehash: 1c78cc957b6ff77c91c8ca4aef59a1cacd85a8ca
ms.sourcegitcommit: 3376f224b47a89acf329b2d2f9260046a372f924
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2019
ms.locfileid: "65517084"
---
# <a name="microsoft-account-external-login-setup-with-aspnet-core"></a><span data-ttu-id="cfaff-103">ASP.NET Core ile Microsoft Account dış oturum açma Kurulumu</span><span class="sxs-lookup"><span data-stu-id="cfaff-103">Microsoft Account external login setup with ASP.NET Core</span></span>

<span data-ttu-id="cfaff-104">Tarafından [Valeriy Novytskyy](https://github.com/01binary) ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="cfaff-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="cfaff-105">Bu örnek üzerinde oluşturulan ASP.NET Core 2.2 projesini kullanarak kendi Microsoft hesabıyla oturum açmasını sağlamak gösterilmektedir [önceki sayfaya](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="cfaff-105">This sample shows you how to enable users to sign in with their Microsoft account using the ASP.NET Core 2.2 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-the-app-in-microsoft-developer-portal"></a><span data-ttu-id="cfaff-106">Microsoft Geliştirici portalında uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="cfaff-106">Create the app in Microsoft Developer Portal</span></span>

* <span data-ttu-id="cfaff-107">Gidin [Azure Portalı - Uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) sayfasında oluşturarak veya mevcut bir Microsoft hesabı olarak oturum açın:</span><span class="sxs-lookup"><span data-stu-id="cfaff-107">Navigate to the [Azure portal - App registrations](https://go.microsoft.com/fwlink/?linkid=2083908) page and create or sign into a Microsoft account:</span></span>

<span data-ttu-id="cfaff-108">Bir Microsoft hesabınız yoksa, seçin **oluşturmak**.</span><span class="sxs-lookup"><span data-stu-id="cfaff-108">If you don't have a Microsoft account, select **Create one**.</span></span> <span data-ttu-id="cfaff-109">Oturum açtıktan sonra yönlendirilirsiniz **uygulama kayıtları** sayfası:</span><span class="sxs-lookup"><span data-stu-id="cfaff-109">After signing in you are redirected to the **App registrations** page:</span></span>

* <span data-ttu-id="cfaff-110">Seçin **yeni kayıt**</span><span class="sxs-lookup"><span data-stu-id="cfaff-110">Select **New registration**</span></span>
* <span data-ttu-id="cfaff-111">Girin bir **adı**.</span><span class="sxs-lookup"><span data-stu-id="cfaff-111">Enter a **Name**.</span></span>
* <span data-ttu-id="cfaff-112">Bir seçeneğini **desteklenen hesap türleri**.</span><span class="sxs-lookup"><span data-stu-id="cfaff-112">Select an option for **Supported account types**.</span></span>  <!-- Accounts for any org work with MS domain accounts. Most folks probably want the last option, personal MS accounts -->
* <span data-ttu-id="cfaff-113">Altında **yeniden yönlendirme URI'si**, geliştirme URL'nizi girin `/signin-microsoft` eklenir.</span><span class="sxs-lookup"><span data-stu-id="cfaff-113">Under **Redirect URI**, enter your development URL with `/signin-microsoft` appended.</span></span> <span data-ttu-id="cfaff-114">Örneğin: `https://localhost:44389/signin-microsoft`</span><span class="sxs-lookup"><span data-stu-id="cfaff-114">For example, `https://localhost:44389/signin-microsoft`.</span></span> <span data-ttu-id="cfaff-115">Bu örnekte, yapılandırılmış Microsoft kimlik doğrulama düzeni istekleri otomatik olarak işleyecek `/signin-microsoft` OAuth akışını uygulamak için rota.</span><span class="sxs-lookup"><span data-stu-id="cfaff-115">The Microsoft authentication scheme configured later in this sample will automatically handle requests at `/signin-microsoft` route to implement the OAuth flow.</span></span>
* <span data-ttu-id="cfaff-116">Seçin **kaydetme**</span><span class="sxs-lookup"><span data-stu-id="cfaff-116">Select **Register**</span></span>

### <a name="create-client-secret"></a><span data-ttu-id="cfaff-117">İstemci gizli anahtarı oluşturma</span><span class="sxs-lookup"><span data-stu-id="cfaff-117">Create client secret</span></span>

* <span data-ttu-id="cfaff-118">Sol bölmede seçin **sertifikaları ve parolaları**.</span><span class="sxs-lookup"><span data-stu-id="cfaff-118">In the left pane, select **Certificates & secrets**.</span></span>
* <span data-ttu-id="cfaff-119">Altında **istemci gizli dizileri**seçin **yeni istemci gizli anahtarı**</span><span class="sxs-lookup"><span data-stu-id="cfaff-119">Under **Client secrets**, select **New client secret**</span></span>

  * <span data-ttu-id="cfaff-120">İstemci gizli anahtarı için bir açıklama ekleyin.</span><span class="sxs-lookup"><span data-stu-id="cfaff-120">Add a description for the client secret.</span></span>
  * <span data-ttu-id="cfaff-121">Seçin **Ekle** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="cfaff-121">Select the **Add** button.</span></span>

* <span data-ttu-id="cfaff-122">Altında **istemci gizli dizileri**, gizli anahtar değerini kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="cfaff-122">Under **Client secrets**, copy the value of the client secret.</span></span>

> [!NOTE]
> <span data-ttu-id="cfaff-123">URI segmenti `/signin-microsoft` Microsoft kimlik doğrulama sağlayıcısı varsayılan geri arama olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="cfaff-123">The URI segment `/signin-microsoft` is set as the default callback of the Microsoft authentication provider.</span></span> <span data-ttu-id="cfaff-124">Microsoft kimlik doğrulaması ara yazılımı üzerinden devralınan yapılandırırken, varsayılan geri arama URI'si değiştirebilirsiniz [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) özelliği [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions) sınıfı.</span><span class="sxs-lookup"><span data-stu-id="cfaff-124">You can change the default callback URI while configuring the Microsoft authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions) class.</span></span>

## <a name="store-the-microsoft-client-id-and-client-secret"></a><span data-ttu-id="cfaff-125">Microsoft istemci Kimliğini ve istemci gizli Store</span><span class="sxs-lookup"><span data-stu-id="cfaff-125">Store the Microsoft client ID and client secret</span></span>

<span data-ttu-id="cfaff-126">Güvenli bir şekilde depolamak için aşağıdaki komutları çalıştırın `ClientId` ve `ClientSecret` kullanarak [gizli dizi Yöneticisi](xref:security/app-secrets):</span><span class="sxs-lookup"><span data-stu-id="cfaff-126">Run the following commands to securely store `ClientId` and `ClientSecret` using [Secret Manager](xref:security/app-secrets):</span></span>

```console
dotnet user-secrets set Authentication:Microsoft:ClientId <Client-Id>
dotnet user-secrets set Authentication:Microsoft:ClientSecret <Client-Secret>
```

<span data-ttu-id="cfaff-127">Bağlantı Microsoft gibi hassas ayarlar `ClientId` ve `ClientSecret` yapılandırma kullanarak uygulama [gizli dizi Yöneticisi](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="cfaff-127">Link sensitive settings like Microsoft `ClientId` and `ClientSecret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="cfaff-128">Bu örnek amacı doğrultusunda, belirteçleri ad `Authentication:Microsoft:ClientId` ve `Authentication:Microsoft:ClientSecret`.</span><span class="sxs-lookup"><span data-stu-id="cfaff-128">For the purposes of this sample, name the tokens `Authentication:Microsoft:ClientId` and `Authentication:Microsoft:ClientSecret`.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="configure-microsoft-account-authentication"></a><span data-ttu-id="cfaff-129">Microsoft hesabı kimlik doğrulamasını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="cfaff-129">Configure Microsoft Account Authentication</span></span>

<span data-ttu-id="cfaff-130">Microsoft Account hizmetini de eklemek `ConfigureServices` yönteminde *Startup.cs* dosyası:</span><span class="sxs-lookup"><span data-stu-id="cfaff-130">Add the Microsoft Account service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

[!code-csharp[](~/security/authentication/social/social-code/StartupMS.cs?name=snippet&highlight=10-14)]

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

<span data-ttu-id="cfaff-131">Bkz: [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) Microsoft Account kimlik doğrulama tarafından desteklenen yapılandırma seçenekleri hakkında daha fazla bilgi için API Başvurusu.</span><span class="sxs-lookup"><span data-stu-id="cfaff-131">See the [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) API reference for more information on configuration options supported by Microsoft Account authentication.</span></span> <span data-ttu-id="cfaff-132">Bu kullanıcı ile ilgili farklı bilgi istemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="cfaff-132">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-microsoft-account"></a><span data-ttu-id="cfaff-133">Microsoft hesabıyla oturum açın</span><span class="sxs-lookup"><span data-stu-id="cfaff-133">Sign in with Microsoft Account</span></span>

<span data-ttu-id="cfaff-134">Çalıştırma tıklatıp **oturum**.</span><span class="sxs-lookup"><span data-stu-id="cfaff-134">Run the and click **Log in**.</span></span> <span data-ttu-id="cfaff-135">Microsoft'ta oturum açmak için bir seçenek görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="cfaff-135">An option to sign in with Microsoft appears.</span></span> <span data-ttu-id="cfaff-136">Microsoft'a tıkladığınızda, kimlik doğrulaması için Microsoft'a yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="cfaff-136">When you click on Microsoft, you are redirected to Microsoft for authentication.</span></span> <span data-ttu-id="cfaff-137">(Zaten açtıysanız) Microsoft Account imzaladıktan sonra uygulamayı bilgilere erişmesine izin istenir:</span><span class="sxs-lookup"><span data-stu-id="cfaff-137">After signing in with your Microsoft Account (if not already signed in) you will be prompted to let the app access your info:</span></span>

<span data-ttu-id="cfaff-138">Dokunun **Evet** ve web e-postanızı ayarlayabileceğiniz siteye geri yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="cfaff-138">Tap **Yes** and you will be redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="cfaff-139">Artık, Microsoft kimlik bilgilerinizi kullanarak kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="cfaff-139">You are now logged in using your Microsoft credentials:</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="cfaff-140">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="cfaff-140">Troubleshooting</span></span>

* <span data-ttu-id="cfaff-141">Microsoft Account sağlayıcısı oturum açma hatası sayfasına yönlendirir, doğrudan aşağıdaki hata başlığı ve açıklamayı sorgu dizesi parametrelerini Not `#` (diyez etiketi) URI.</span><span class="sxs-lookup"><span data-stu-id="cfaff-141">If the Microsoft Account provider redirects you to a sign in error page, note the error title and description query string parameters directly following the `#` (hashtag) in the Uri.</span></span>

  <span data-ttu-id="cfaff-142">Microsoft kimlik doğrulama ile ilgili bir sorun göstermek için hata iletisini gibi görünüyor ancak uygulamanızı herhangi bir eşleşmeyen URI en yaygın nedeni **yeniden yönlendirme URI'leri** için belirtilen **Web** platformu .</span><span class="sxs-lookup"><span data-stu-id="cfaff-142">Although the error message seems to indicate a problem with Microsoft authentication, the most common cause is your application Uri not matching any of the **Redirect URIs** specified for the **Web** platform.</span></span>
* <span data-ttu-id="cfaff-143">Kimlik çağırarak yapılandırılmazsa `services.AddIdentity` içinde `ConfigureServices`, kimlik doğrulaması yapmaya sonuçlanır *ArgumentException: 'SignInScheme' seçeneği belirtilmelidir*.</span><span class="sxs-lookup"><span data-stu-id="cfaff-143">If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="cfaff-144">Bu örnekte kullanılan proje şablonu, bu gerçekleştirilir sağlar.</span><span class="sxs-lookup"><span data-stu-id="cfaff-144">The project template used in this sample ensures that this is done.</span></span>
* <span data-ttu-id="cfaff-145">Site veritabanı, ilk geçiş uygulayarak oluşturulmamış alırsa *bir veritabanı işlemi başarısız istek işlenirken* hata.</span><span class="sxs-lookup"><span data-stu-id="cfaff-145">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="cfaff-146">Dokunun **geçerli geçişleri** veritabanı oluşturma ve hata devam etmek için yenilemek için.</span><span class="sxs-lookup"><span data-stu-id="cfaff-146">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cfaff-147">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="cfaff-147">Next steps</span></span>

* <span data-ttu-id="cfaff-148">Bu makalede, Microsoft ile kimliğini nasıl doğrulayabileceğiniz gösterdi.</span><span class="sxs-lookup"><span data-stu-id="cfaff-148">This article showed how you can authenticate with Microsoft.</span></span> <span data-ttu-id="cfaff-149">Listelenen diğer sağlayıcıları ile kimlik doğrulaması için benzer bir yaklaşım izleyerek [önceki sayfaya](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="cfaff-149">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="cfaff-150">Web sitenizi Azure web uygulaması yayımladıktan sonra yeni bir istemci gizli dizileri Microsoft Geliştirici Portalı'nda oluşturun.</span><span class="sxs-lookup"><span data-stu-id="cfaff-150">Once you publish your web site to Azure web app, create a new client secrets in the Microsoft Developer Portal.</span></span>

* <span data-ttu-id="cfaff-151">Ayarlama `Authentication:Microsoft:ClientId` ve `Authentication:Microsoft:ClientSecret` Azure portalında uygulama ayarları olarak.</span><span class="sxs-lookup"><span data-stu-id="cfaff-151">Set the `Authentication:Microsoft:ClientId` and `Authentication:Microsoft:ClientSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="cfaff-152">Yapılandırma sistemi ortam değişkenlerinden anahtarları okumak için ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="cfaff-152">The configuration system is set up to read keys from environment variables.</span></span>