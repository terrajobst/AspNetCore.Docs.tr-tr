---
title: ASP.NET Core tek sayfalı uygulamalar için kimlik doğrulamaya giriş
author: javiercn
description: ASP.NET Core uygulamasının içinde barındırılan tek sayfalı bir uygulamayla kimlik kullanın.
monikerRange: '>= aspnetcore-3.0'
ms.author: scaddie
ms.custom: mvc
ms.date: 10/29/2019
uid: security/authentication/identity/spa
ms.openlocfilehash: 5ed5fb61e5989b291523332c6a2ec332f9ca0f6b
ms.sourcegitcommit: e5d4768aaf85703effb4557a520d681af8284e26
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2019
ms.locfileid: "73616614"
---
# <a name="authentication-and-authorization-for-spas"></a><span data-ttu-id="ae5d0-103">Maça kimlik doğrulaması ve yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="ae5d0-103">Authentication and authorization for SPAs</span></span>

<span data-ttu-id="ae5d0-104">ASP.NET Core 3,0 veya üzeri, API yetkilendirmesi desteğini kullanarak tek sayfalı uygulamalarda (Spaon) kimlik doğrulaması sunmaktadır.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-104">ASP.NET Core 3.0 or later offers authentication in Single Page Apps (SPAs) using the support for API authorization.</span></span> <span data-ttu-id="ae5d0-105">Kimlik doğrulama ve depolama için ASP.NET Core kimliği, açık KIMLIK Connect uygulama için [IdentityServer](https://identityserver.io/) ile birleştirilir.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-105">ASP.NET Core Identity for authenticating and storing users is combined with [IdentityServer](https://identityserver.io/) for implementing Open ID Connect.</span></span>

<span data-ttu-id="ae5d0-106">Web uygulamasındaki kimlik doğrulama parametresine benzer bir kimlik doğrulama parametresi ( **Model-View-Controller)** (MVC **) ve** **Web uygulaması** (Razor Pages) Proje şablonları.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-106">An authentication parameter was added to the **Angular** and **React** project templates that is similar to the authentication parameter in the **Web Application (Model-View-Controller)** (MVC) and **Web Application** (Razor Pages) project templates.</span></span> <span data-ttu-id="ae5d0-107">İzin verilen parametre değerleri **none** ve **bireysel**.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-107">The allowed parameter values are **None** and **Individual**.</span></span> <span data-ttu-id="ae5d0-108">**Tepki. js ve Redux** proje şablonu şu anda kimlik doğrulama parametresini desteklemiyor.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-108">The **React.js and Redux** project template doesn't support the authentication parameter at this time.</span></span>

## <a name="create-an-app-with-api-authorization-support"></a><span data-ttu-id="ae5d0-109">API yetkilendirme desteğiyle uygulama oluşturma</span><span class="sxs-lookup"><span data-stu-id="ae5d0-109">Create an app with API authorization support</span></span>

<span data-ttu-id="ae5d0-110">Kullanıcı kimlik doğrulaması ve yetkilendirme, hem angular ile hem de maça 'Ları ile kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-110">User authentication and authorization can be used with both Angular and React SPAs.</span></span> <span data-ttu-id="ae5d0-111">Bir komut kabuğu açın ve şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="ae5d0-111">Open a command shell, and run the following command:</span></span>

<span data-ttu-id="ae5d0-112">**Angular**:</span><span class="sxs-lookup"><span data-stu-id="ae5d0-112">**Angular**:</span></span>

```dotnetcli
dotnet new angular -o <output_directory_name> -au Individual
```

<span data-ttu-id="ae5d0-113">**Tepki**verme:</span><span class="sxs-lookup"><span data-stu-id="ae5d0-113">**React**:</span></span>

```dotnetcli
dotnet new react -o <output_directory_name> -au Individual
```

<span data-ttu-id="ae5d0-114">Yukarıdaki komut, SPA 'yı içeren *clientapp* dizinine sahip bir ASP.NET Core uygulaması oluşturur.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-114">The preceding command creates an ASP.NET Core app with a *ClientApp* directory containing the SPA.</span></span>

## <a name="general-description-of-the-aspnet-core-components-of-the-app"></a><span data-ttu-id="ae5d0-115">Uygulamanın ASP.NET Core bileşenlerinin genel açıklaması</span><span class="sxs-lookup"><span data-stu-id="ae5d0-115">General description of the ASP.NET Core components of the app</span></span>

<span data-ttu-id="ae5d0-116">Aşağıdaki bölümlerde, kimlik doğrulama desteği dahil edildiğinde projenin eklemeleri açıklanır:</span><span class="sxs-lookup"><span data-stu-id="ae5d0-116">The following sections describe additions to the project when authentication support is included:</span></span>

### <a name="startup-class"></a><span data-ttu-id="ae5d0-117">Başlangıç sınıfı</span><span class="sxs-lookup"><span data-stu-id="ae5d0-117">Startup class</span></span>

<span data-ttu-id="ae5d0-118">`Startup` sınıfı aşağıdaki eklemelere sahiptir:</span><span class="sxs-lookup"><span data-stu-id="ae5d0-118">The `Startup` class has the following additions:</span></span>

* <span data-ttu-id="ae5d0-119">`Startup.ConfigureServices` yönteminin içinde:</span><span class="sxs-lookup"><span data-stu-id="ae5d0-119">Inside the `Startup.ConfigureServices` method:</span></span>
  * <span data-ttu-id="ae5d0-120">Varsayılan Kullanıcı arabirimine sahip kimlik:</span><span class="sxs-lookup"><span data-stu-id="ae5d0-120">Identity with the default UI:</span></span>

    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlite(Configuration.GetConnectionString("DefaultConnection")));

    services.AddDefaultIdentity<ApplicationUser>()
        .AddDefaultUI(UIFramework.Bootstrap4)
        .AddEntityFrameworkStores<ApplicationDbContext>();
    ```

  * <span data-ttu-id="ae5d0-121">IdentityServer 'ın en üstünde yer alan bazı varsayılan ASP.NET Core kurallarını yükleyen ek bir `AddApiAuthorization` Yardımcısı yöntemi olan IdentityServer:</span><span class="sxs-lookup"><span data-stu-id="ae5d0-121">IdentityServer with an additional `AddApiAuthorization` helper method that setups some default ASP.NET Core conventions on top of IdentityServer:</span></span>

    ```csharp
    services.AddIdentityServer()
        .AddApiAuthorization<ApplicationUser, ApplicationDbContext>();
    ```

  * <span data-ttu-id="ae5d0-122">Kimliği, IdentityServer tarafından üretilen JWT belirteçlerini doğrulamak üzere uygulamayı yapılandıran ek bir `AddIdentityServerJwt` Yardımcısı yöntemiyle kimlik doğrulaması:</span><span class="sxs-lookup"><span data-stu-id="ae5d0-122">Authentication with an additional `AddIdentityServerJwt` helper method that configures the app to validate JWT tokens produced by IdentityServer:</span></span>

    ```csharp
    services.AddAuthentication()
        .AddIdentityServerJwt();
    ```

* <span data-ttu-id="ae5d0-123">`Startup.Configure` yönteminin içinde:</span><span class="sxs-lookup"><span data-stu-id="ae5d0-123">Inside the `Startup.Configure` method:</span></span>
  * <span data-ttu-id="ae5d0-124">İstek kimlik bilgilerini doğrulamadan ve Kullanıcı istek bağlamında ayarlamaktan sorumlu kimlik doğrulama ara yazılımı:</span><span class="sxs-lookup"><span data-stu-id="ae5d0-124">The authentication middleware that is responsible for validating the request credentials and setting the user on the request context:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

  * <span data-ttu-id="ae5d0-125">Açık KIMLIK bağlantı noktalarını kullanıma sunan IdentityServer ara yazılımı:</span><span class="sxs-lookup"><span data-stu-id="ae5d0-125">The IdentityServer middleware that exposes the Open ID Connect endpoints:</span></span>

    ```csharp
    app.UseIdentityServer();
    ```

### <a name="addapiauthorization"></a><span data-ttu-id="ae5d0-126">Addadpiauthorization</span><span class="sxs-lookup"><span data-stu-id="ae5d0-126">AddApiAuthorization</span></span>

<span data-ttu-id="ae5d0-127">Bu yardımcı yöntemi, IdentityServer 'ı desteklenen yapılandırmamızı kullanacak şekilde yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-127">This helper method configures IdentityServer to use our supported configuration.</span></span> <span data-ttu-id="ae5d0-128">IdentityServer, uygulama güvenliği sorunlarını işlemeye yönelik güçlü ve genişletilebilir bir çerçevedir.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-128">IdentityServer is a powerful and extensible framework for handling app security concerns.</span></span> <span data-ttu-id="ae5d0-129">Aynı zamanda, en yaygın senaryolar için gereksiz karmaşıklık sunan.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-129">At the same time, that exposes unnecessary complexity for the most common scenarios.</span></span> <span data-ttu-id="ae5d0-130">Sonuç olarak, size iyi bir başlangıç noktası olarak kabul edilen bir dizi kural ve yapılandırma seçeneği sağlanır.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-130">Consequently, a set of conventions and configuration options is provided to you that are considered a good starting point.</span></span> <span data-ttu-id="ae5d0-131">Kimlik doğrulama gereksinimleriniz değiştikçe, IdentityServer 'ın tam gücü, kimlik doğrulamasını gereksinimlerinize uyacak şekilde özelleştirmek için hala kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-131">Once your authentication needs change, the full power of IdentityServer is still available to customize authentication to suit your needs.</span></span>

### <a name="addidentityserverjwt"></a><span data-ttu-id="ae5d0-132">Addentityserverjwt</span><span class="sxs-lookup"><span data-stu-id="ae5d0-132">AddIdentityServerJwt</span></span>

<span data-ttu-id="ae5d0-133">Bu yardımcı yöntemi, varsayılan kimlik doğrulama işleyicisi olarak uygulama için bir ilke düzeni yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-133">This helper method configures a policy scheme for the app as the default authentication handler.</span></span> <span data-ttu-id="ae5d0-134">İlke, kimlik URL 'SI alanı "/Identity" içindeki herhangi bir alt yol için tüm isteklerin kimlik işlemesini sağlamak üzere yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-134">The policy is configured to let Identity handle all requests routed to any subpath in the Identity URL space "/Identity".</span></span> <span data-ttu-id="ae5d0-135">`JwtBearerHandler` diğer tüm istekleri işler.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-135">The `JwtBearerHandler` handles all other requests.</span></span> <span data-ttu-id="ae5d0-136">Ayrıca, bu yöntem, IdentityServer 'a `<<ApplicationName>>API` varsayılan kapsamına sahip bir `<<ApplicationName>>API` API kaynağı kaydeder ve JWT taşıyıcı belirteç ara yazılımını, bu uygulama için IdentityServer tarafından verilen belirteçleri doğrulamak için yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-136">Additionally, this method registers an `<<ApplicationName>>API` API resource with IdentityServer with a default scope of `<<ApplicationName>>API` and configures the JWT Bearer token middleware to validate tokens issued by IdentityServer for the app.</span></span>

### <a name="weatherforecastcontroller"></a><span data-ttu-id="ae5d0-137">Dalgalı bir denetleyici</span><span class="sxs-lookup"><span data-stu-id="ae5d0-137">WeatherForecastController</span></span>

<span data-ttu-id="ae5d0-138">*Controllers\dalgalı therforeroı Controller.cs* dosyasında, kullanıcıya kaynağa erişim için varsayılan ilkeye göre yetkilendirileceğini belirten sınıfa uygulanan `[Authorize]` özniteliğine dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-138">In the *Controllers\WeatherForecastController.cs* file, notice the `[Authorize]` attribute applied to the class that indicates that the user needs to be authorized based on the default policy to access the resource.</span></span> <span data-ttu-id="ae5d0-139">Varsayılan yetkilendirme ilkesi, yukarıda belirtilen ilke şemasına `AddIdentityServerJwt` tarafından ayarlanan varsayılan kimlik doğrulama şemasını kullanacak şekilde yapılandırılmıştır. Bu, bu tür yardımcı yöntemi tarafından yapılandırılan `JwtBearerHandler` istekleri için varsayılan işleyicidir uygulama.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-139">The default authorization policy happens to be configured to use the default authentication scheme, which is set up by `AddIdentityServerJwt` to the policy scheme that was mentioned above, making the `JwtBearerHandler` configured by such helper method the default handler for requests to the app.</span></span>

### <a name="applicationdbcontext"></a><span data-ttu-id="ae5d0-140">ApplicationDbContext</span><span class="sxs-lookup"><span data-stu-id="ae5d0-140">ApplicationDbContext</span></span>

<span data-ttu-id="ae5d0-141">*Data\applicationdbcontext.cs* dosyasında, `DbContext` (`IdentityDbContext`' den daha fazla türetilmiş bir sınıf), IdentityServer şemasını dahil etmek için `ApiAuthorizationDbContext` genişlettiği özel durum ile kimliğin aynı kullanıldığını görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-141">In the *Data\ApplicationDbContext.cs* file, notice the same `DbContext` is used in Identity with the exception that it extends `ApiAuthorizationDbContext` (a more derived class from `IdentityDbContext`) to include the schema for IdentityServer.</span></span>

<span data-ttu-id="ae5d0-142">Veritabanı şeması üzerinde tam denetim elde etmek için, kullanılabilir kimlik `DbContext` sınıflarından birinden devralma ve bağlamı `OnModelCreating` yöntemine `builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)` çağırarak kimlik şemasını içerecek şekilde yapılandırma.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-142">To gain full control of the database schema, inherit from one of the available Identity `DbContext` classes and configure the context to include the Identity schema by calling `builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)` on the `OnModelCreating` method.</span></span>

### <a name="oidcconfigurationcontroller"></a><span data-ttu-id="ae5d0-143">Oıdcconfigurationcontroller</span><span class="sxs-lookup"><span data-stu-id="ae5d0-143">OidcConfigurationController</span></span>

<span data-ttu-id="ae5d0-144">*Controllers\oıdcconfigurationcontroller.cs* dosyasında, istemcinin kullanması gereken OIDC parametrelerine hizmeti sağlaması için sağlanan uç noktaya dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-144">In the *Controllers\OidcConfigurationController.cs* file, notice the endpoint that's provisioned to serve the OIDC parameters that the client needs to use.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="ae5d0-145">appSettings. JSON</span><span class="sxs-lookup"><span data-stu-id="ae5d0-145">appsettings.json</span></span>

<span data-ttu-id="ae5d0-146">Proje kökünün *appSettings. JSON* dosyasında, yapılandırılmış istemciler listesini açıklayan yeni bir `IdentityServer` bölümü vardır.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-146">In the *appsettings.json* file of the project root, there's a new `IdentityServer` section that describes the list of configured clients.</span></span> <span data-ttu-id="ae5d0-147">Aşağıdaki örnekte, tek bir istemci vardır.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-147">In the following example, there's a single client.</span></span> <span data-ttu-id="ae5d0-148">İstemci adı, uygulama adına karşılık gelir ve bir kural tarafından OAuth `ClientId` parametresine eşlenir.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-148">The client name corresponds to the app name and is mapped by convention to the OAuth `ClientId` parameter.</span></span> <span data-ttu-id="ae5d0-149">Profil, yapılandırılan uygulama türünü gösterir.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-149">The profile indicates the app type being configured.</span></span> <span data-ttu-id="ae5d0-150">Sunucu için yapılandırma işlemini basitleştiren kuralları yönlendirmek için dahili olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-150">It's used internally to drive conventions that simplify the configuration process for the server.</span></span> <span data-ttu-id="ae5d0-151">[Uygulama profilleri](#application-profiles) bölümünde açıklandığı gibi çeşitli profiller mevcuttur.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-151">There are several profiles available, as explained in the [Application profiles](#application-profiles) section.</span></span>

```json
"IdentityServer": {
  "Clients": {
    "angularindividualpreview3final": {
      "Profile": "IdentityServerSPA"
    }
  }
}
```

### <a name="appsettingsdevelopmentjson"></a><span data-ttu-id="ae5d0-152">appSettings. Development. JSON</span><span class="sxs-lookup"><span data-stu-id="ae5d0-152">appsettings.Development.json</span></span>

<span data-ttu-id="ae5d0-153">, *AppSettings 'de. Proje kökünün geliştirme. JSON* dosyası, belirteçleri imzalamak için kullanılan anahtarı açıklayan bir `IdentityServer` bölümü vardır.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-153">In the *appsettings.Development.json* file of the project root, there's an `IdentityServer` section that describes the key used to sign tokens.</span></span> <span data-ttu-id="ae5d0-154">Üretime dağıtım yaparken, [üretime dağıtma](#deploy-to-production) bölümünde açıklandığı gibi bir anahtarın uygulamayla birlikte sağlanması ve dağıtılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-154">When deploying to production, a key needs to be provisioned and deployed alongside the app, as explained in the [Deploy to production](#deploy-to-production) section.</span></span>

```json
"IdentityServer": {
  "Key": {
    "Type": "Development"
  }
}
```

## <a name="general-description-of-the-angular-app"></a><span data-ttu-id="ae5d0-155">Angular uygulamasının genel açıklaması</span><span class="sxs-lookup"><span data-stu-id="ae5d0-155">General description of the Angular app</span></span>

<span data-ttu-id="ae5d0-156">Angular şablonundaki kimlik doğrulama ve API yetkilendirme desteği *Clientapp\src\api-Authorization* dizinindeki kendi angular modülünde yer alır.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-156">The authentication and API authorization support in the Angular template resides in its own Angular module in the *ClientApp\src\api-authorization* directory.</span></span> <span data-ttu-id="ae5d0-157">Modül aşağıdaki öğelerden oluşur:</span><span class="sxs-lookup"><span data-stu-id="ae5d0-157">The module is composed of the following elements:</span></span>

* <span data-ttu-id="ae5d0-158">3 bileşen:</span><span class="sxs-lookup"><span data-stu-id="ae5d0-158">3 components:</span></span>
  * <span data-ttu-id="ae5d0-159">*login. Component. TS*: uygulamanın oturum açma akışını işler.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-159">*login.component.ts*: Handles the app's login flow.</span></span>
  * <span data-ttu-id="ae5d0-160">*Logout. Component. TS*: uygulamanın oturum kapatma akışını işler.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-160">*logout.component.ts*: Handles the app's logout flow.</span></span>
  * <span data-ttu-id="ae5d0-161">*login-Menu. Component. TS*: aşağıdaki bağlantı kümelerinden birini görüntüleyen pencere öğesi:</span><span class="sxs-lookup"><span data-stu-id="ae5d0-161">*login-menu.component.ts*: A widget that displays one of the following sets of links:</span></span>
    * <span data-ttu-id="ae5d0-162">Kullanıcı profili yönetimi ve kullanıcının kimlik doğrulaması yapıldığında oturum kapatma bağlantıları.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-162">User profile management and log out links when the user is authenticated.</span></span>
    * <span data-ttu-id="ae5d0-163">Kullanıcının kimlik doğrulaması olmadığında kayıt ve oturum açma bağlantıları.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-163">Registration and log in links when the user isn't authenticated.</span></span>
* <span data-ttu-id="ae5d0-164">Rotalara eklenebilen ve yolu ziyaret etmeden önce kullanıcının kimliğinin doğrulanmasını gerektiren bir Route Guard `AuthorizeGuard`.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-164">A route guard `AuthorizeGuard` that can be added to routes and requires a user to be authenticated before visiting the route.</span></span>
* <span data-ttu-id="ae5d0-165">Kullanıcının kimlik doğrulaması yapıldığında API 'YI hedefleyen giden HTTP isteklerine erişim belirtecini ekleyen bir HTTP yakalayıcısı `AuthorizeInterceptor`.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-165">An HTTP interceptor `AuthorizeInterceptor` that attaches the access token to outgoing HTTP requests targeting the API when the user is authenticated.</span></span>
* <span data-ttu-id="ae5d0-166">Kimlik doğrulama işleminin alt düzey ayrıntılarını işleyen ve kimliği doğrulanmış kullanıcı hakkındaki bilgileri, tüketim için uygulamanın geri kalanına getiren bir hizmet `AuthorizeService`.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-166">A service `AuthorizeService` that handles the lower-level details of the authentication process and exposes information about the authenticated user to the rest of the app for consumption.</span></span>
* <span data-ttu-id="ae5d0-167">Uygulamanın kimlik doğrulama bölümleriyle ilişkili yolları tanımlayan angular modülü.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-167">An Angular module that defines routes associated with the authentication parts of the app.</span></span> <span data-ttu-id="ae5d0-168">Oturum açma menü bileşeni, yakalayıcısı, koruyucu ve uygulamanın geri kalanından tüketim için hizmeti sunar.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-168">It exposes the login menu component, the interceptor, the guard, and the service for consumption from the rest of the app.</span></span>

## <a name="general-description-of-the-react-app"></a><span data-ttu-id="ae5d0-169">Tepki verme uygulamasının genel açıklaması</span><span class="sxs-lookup"><span data-stu-id="ae5d0-169">General description of the React app</span></span>

<span data-ttu-id="ae5d0-170">Yanıt verme şablonunda kimlik doğrulama ve API yetkilendirmesi desteği *Clientapp\src\\disk api-Authorization* dizininde bulunur.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-170">The support for authentication and API authorization in the React template resides in the *ClientApp\src\components\api-authorization* directory.</span></span> <span data-ttu-id="ae5d0-171">Şu öğelerden oluşur:</span><span class="sxs-lookup"><span data-stu-id="ae5d0-171">It's composed of the following elements:</span></span>

* <span data-ttu-id="ae5d0-172">4 bileşen:</span><span class="sxs-lookup"><span data-stu-id="ae5d0-172">4 components:</span></span>
  * <span data-ttu-id="ae5d0-173">*Login. js*: uygulamanın oturum açma akışını işler.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-173">*Login.js*: Handles the app's login flow.</span></span>
  * <span data-ttu-id="ae5d0-174">*Logout. js*: uygulamanın oturum kapatma akışını işler.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-174">*Logout.js*: Handles the app's logout flow.</span></span>
  * <span data-ttu-id="ae5d0-175">*Loginmenu. js*: aşağıdaki bağlantı kümelerinden birini görüntüleyen pencere öğesi:</span><span class="sxs-lookup"><span data-stu-id="ae5d0-175">*LoginMenu.js*: A widget that displays one of the following sets of links:</span></span>
    * <span data-ttu-id="ae5d0-176">Kullanıcı profili yönetimi ve kullanıcının kimlik doğrulaması yapıldığında oturum kapatma bağlantıları.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-176">User profile management and log out links when the user is authenticated.</span></span>
    * <span data-ttu-id="ae5d0-177">Kullanıcının kimlik doğrulaması olmadığında kayıt ve oturum açma bağlantıları.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-177">Registration and log in links when the user isn't authenticated.</span></span>
  * <span data-ttu-id="ae5d0-178">*Authorizeroute. js*: `Component` parametresinde belirtilen bileşeni işlemeden önce kullanıcının kimliğinin doğrulanmasını gerektiren bir rota bileşeni.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-178">*AuthorizeRoute.js*: A route component that requires a user to be authenticated before rendering the component indicated in the `Component` parameter.</span></span>
* <span data-ttu-id="ae5d0-179">Bir `AuthorizeService` sınıfının dışa, kimlik doğrulama işleminin alt düzey ayrıntılarını işleyen ve kimliği doğrulanmış kullanıcı hakkında bilgileri, tüketim için uygulamanın geri kalanına getiren bir `authService` örneği.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-179">An exported `authService` instance of class `AuthorizeService` that handles the lower-level details of the authentication process and exposes information about the authenticated user to the rest of the app for consumption.</span></span>

<span data-ttu-id="ae5d0-180">Artık çözümün ana bileşenlerini gördüğünüze göre, uygulama için ayrı senaryolara daha ayrıntılı bir şekilde göz atabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-180">Now that you've seen the main components of the solution, you can take a deeper look at individual scenarios for the app.</span></span>

## <a name="require-authorization-on-a-new-api"></a><span data-ttu-id="ae5d0-181">Yeni bir API 'de yetkilendirme gerektir</span><span class="sxs-lookup"><span data-stu-id="ae5d0-181">Require authorization on a new API</span></span>

<span data-ttu-id="ae5d0-182">Varsayılan olarak, sistem yeni API 'Ler için kolayca yetkilendirme gerektirecek şekilde yapılandırılmıştır.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-182">By default, the system is configured to easily require authorization for new APIs.</span></span> <span data-ttu-id="ae5d0-183">Bunu yapmak için yeni bir denetleyici oluşturun ve denetleyici sınıfına veya denetleyici içindeki herhangi bir eyleme `[Authorize]` özniteliğini ekleyin.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-183">To do so, create a new controller and add the `[Authorize]` attribute to the controller class or to any action within the controller.</span></span>

## <a name="customize-the-api-authentication-handler"></a><span data-ttu-id="ae5d0-184">API kimlik doğrulama işleyicisini özelleştirme</span><span class="sxs-lookup"><span data-stu-id="ae5d0-184">Customize the API authentication handler</span></span>

<span data-ttu-id="ae5d0-185">API 'nin JWT işleyicisinin yapılandırmasını özelleştirmek için <xref:Microsoft.AspNetCore.Builder.JwtBearerOptions> örneğini yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="ae5d0-185">To customize the configuration of the API's JWT handler, configure its <xref:Microsoft.AspNetCore.Builder.JwtBearerOptions> instance:</span></span>

```csharp
services.AddAuthentication()
    .AddIdentityServerJwt();

services.Configure<JwtBearerOptions>(
    IdentityServerJwtConstants.IdentityServerJwtBearerScheme,
    options =>
    {
        ...
    });
```

## <a name="protect-a-client-side-route-angular"></a><span data-ttu-id="ae5d0-186">İstemci tarafı yolunu koruma (angular)</span><span class="sxs-lookup"><span data-stu-id="ae5d0-186">Protect a client-side route (Angular)</span></span>

<span data-ttu-id="ae5d0-187">İstemci tarafı bir yolu korumak, yetkilendirme koruyucusu bir rota yapılandırılırken çalıştırılacak korumalara listesine eklenerek yapılır.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-187">Protecting a client-side route is done by adding the authorize guard to the list of guards to run when configuring a route.</span></span> <span data-ttu-id="ae5d0-188">Örnek olarak, `fetch-data` yolun ana uygulama angular modülünde nasıl yapılandırıldığını görebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="ae5d0-188">As an example, you can see how the `fetch-data` route is configured within the main app Angular module:</span></span>

```typescript
RouterModule.forRoot([
  // ...
  { path: 'fetch-data', component: FetchDataComponent, canActivate: [AuthorizeGuard] },
])
```

<span data-ttu-id="ae5d0-189">Bir yolu korumanın gerçek uç noktayı korumadığını (yine de buna uygulanan bir `[Authorize]` özniteliği gerektirdiğini), ancak kullanıcının kimlik doğrulaması olmadığında yalnızca belirtilen istemci tarafı rotasında gezinmelerini önlediği bahsetmek önemlidir.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-189">It's important to mention that protecting a route doesn't protect the actual endpoint (which still requires an `[Authorize]` attribute applied to it) but that it only prevents the user from navigating to the given client-side route when it isn't authenticated.</span></span>

## <a name="authenticate-api-requests-angular"></a><span data-ttu-id="ae5d0-190">API isteklerinin kimliğini doğrulama (angular)</span><span class="sxs-lookup"><span data-stu-id="ae5d0-190">Authenticate API requests (Angular)</span></span>

<span data-ttu-id="ae5d0-191">Uygulama ile barındırılan API 'lere yönelik kimlik doğrulama istekleri, uygulama tarafından tanımlanan HTTP istemci yakalayıcısı kullanılarak otomatik olarak yapılır.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-191">Authenticating requests to APIs hosted alongside the app is done automatically through the use of the HTTP client interceptor defined by the app.</span></span>

## <a name="protect-a-client-side-route-react"></a><span data-ttu-id="ae5d0-192">İstemci tarafı yolunu koruma (tepki verme)</span><span class="sxs-lookup"><span data-stu-id="ae5d0-192">Protect a client-side route (React)</span></span>

<span data-ttu-id="ae5d0-193">Düz `Route` bileşeni yerine `AuthorizeRoute` bileşenini kullanarak bir istemci tarafı yolunu koruyun.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-193">Protect a client-side route by using the `AuthorizeRoute` component instead of the plain `Route` component.</span></span> <span data-ttu-id="ae5d0-194">Örneğin, `fetch-data` yolunun `App` bileşeni içinde nasıl yapılandırıldığı hakkında dikkat edin:</span><span class="sxs-lookup"><span data-stu-id="ae5d0-194">For example, notice how the `fetch-data` route is configured within the `App` component:</span></span>

```jsx
<AuthorizeRoute path='/fetch-data' component={FetchData} />
```

<span data-ttu-id="ae5d0-195">Bir yolu koruma:</span><span class="sxs-lookup"><span data-stu-id="ae5d0-195">Protecting a route:</span></span>

* <span data-ttu-id="ae5d0-196">Gerçek uç noktayı korumaz (hala buna uygulanan bir `[Authorize]` özniteliği gerektirir).</span><span class="sxs-lookup"><span data-stu-id="ae5d0-196">Doesn't protect the actual endpoint (which still requires an `[Authorize]` attribute applied to it).</span></span>
* <span data-ttu-id="ae5d0-197">Yalnızca kullanıcının kimlik doğrulaması olmadığında verilen istemci tarafı rotasında gezinmelerini engeller.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-197">Only prevents the user from navigating to the given client-side route when it isn't authenticated.</span></span>

## <a name="authenticate-api-requests-react"></a><span data-ttu-id="ae5d0-198">API isteklerinin kimliğini doğrulama (tepki)</span><span class="sxs-lookup"><span data-stu-id="ae5d0-198">Authenticate API requests (React)</span></span>

<span data-ttu-id="ae5d0-199">Yanıt vererek istekleri doğrulamak, öncelikle `authService` örneği `AuthorizeService`içeri aktararak yapılır.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-199">Authenticating requests with React is done by first importing the `authService` instance from the `AuthorizeService`.</span></span> <span data-ttu-id="ae5d0-200">Erişim belirteci `authService` alınır ve aşağıda gösterildiği gibi isteğe iliştirilir.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-200">The access token is retrieved from the `authService` and is attached to the request as shown below.</span></span> <span data-ttu-id="ae5d0-201">Yanıt verme bileşenlerinde, bu iş genellikle `componentDidMount` yaşam döngüsü yönteminde veya bazı Kullanıcı etkileşiminden elde edilen sonuç olarak yapılır.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-201">In React components, this work is typically done in the `componentDidMount` lifecycle method or as the result from some user interaction.</span></span>

### <a name="import-the-authservice-into-your-component"></a><span data-ttu-id="ae5d0-202">AuthService 'i bileşeninizdeki içeri aktarın</span><span class="sxs-lookup"><span data-stu-id="ae5d0-202">Import the authService into your component</span></span>

```javascript
import authService from './api-authorization/AuthorizeService'
```

### <a name="retrieve-and-attach-the-access-token-to-the-response"></a><span data-ttu-id="ae5d0-203">Erişim belirtecini alma ve yanıta iliştirme</span><span class="sxs-lookup"><span data-stu-id="ae5d0-203">Retrieve and attach the access token to the response</span></span>

```javascript
async populateWeatherData() {
  const token = await authService.getAccessToken();
  const response = await fetch('api/SampleData/WeatherForecasts', {
    headers: !token ? {} : { 'Authorization': `Bearer ${token}` }
  });
  const data = await response.json();
  this.setState({ forecasts: data, loading: false });
}
```

## <a name="deploy-to-production"></a><span data-ttu-id="ae5d0-204">Üretime dağıtma</span><span class="sxs-lookup"><span data-stu-id="ae5d0-204">Deploy to production</span></span>

<span data-ttu-id="ae5d0-205">Uygulamayı üretime dağıtmak için aşağıdaki kaynakların sağlanması gerekir:</span><span class="sxs-lookup"><span data-stu-id="ae5d0-205">To deploy the app to production, the following resources need to be provisioned:</span></span>

* <span data-ttu-id="ae5d0-206">Kimliği kullanıcı hesaplarının ve IdentityServer 'ın verdiği bir veritabanı.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-206">A database to store the Identity user accounts and the IdentityServer grants.</span></span>
* <span data-ttu-id="ae5d0-207">Belirteçleri imzalamak için kullanılacak bir üretim sertifikası.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-207">A production certificate to use for signing tokens.</span></span>
  * <span data-ttu-id="ae5d0-208">Bu sertifika için belirli bir gereksinim yoktur; otomatik olarak imzalanan bir sertifika veya bir CA yetkilisi tarafından sağlanan bir sertifika olabilir.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-208">There are no specific requirements for this certificate; it can be a self-signed certificate or a certificate provisioned through a CA authority.</span></span>
  * <span data-ttu-id="ae5d0-209">PowerShell veya OpenSSL gibi standart araçlarla oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-209">It can be generated through standard tools like PowerShell or OpenSSL.</span></span>
  * <span data-ttu-id="ae5d0-210">Hedef makinelerdeki sertifika deposuna yüklenebilir veya güçlü bir parolayla bir *. pfx* dosyası olarak dağıtılabilir.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-210">It can be installed into the certificate store on the target machines or deployed as a *.pfx* file with a strong password.</span></span>

### <a name="example-deploy-to-azure-websites"></a><span data-ttu-id="ae5d0-211">Örnek: Azure Web sitelerine dağıtma</span><span class="sxs-lookup"><span data-stu-id="ae5d0-211">Example: Deploy to Azure Websites</span></span>

<span data-ttu-id="ae5d0-212">Bu bölümde, sertifika deposunda depolanan bir sertifika kullanılarak uygulamanın Azure Web sitelerine dağıtımı açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-212">This section describes deploying the app to Azure websites using a certificate stored in the certificate store.</span></span> <span data-ttu-id="ae5d0-213">Uygulamayı sertifika deposundan bir sertifika yükleyecek şekilde değiştirmek için, daha sonraki bir adımda yapılandırdığınızda App Service planının en azından Standart katmanda olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-213">To modify the app to load a certificate from the certificate store, the App Service plan needs to be on at least the Standard tier when you configure in a later step.</span></span> <span data-ttu-id="ae5d0-214">Uygulamanın *appSettings. JSON* dosyasında, `IdentityServer` bölümünü, anahtar ayrıntılarını içerecek şekilde değiştirin:</span><span class="sxs-lookup"><span data-stu-id="ae5d0-214">In the app's *appsettings.json* file, modify the `IdentityServer` section to include the key details:</span></span>

```json
"IdentityServer": {
  "Key": {
    "Type": "Store",
    "StoreName": "My",
    "StoreLocation": "CurrentUser",
    "Name": "CN=MyApplication"
  }
}
```

* <span data-ttu-id="ae5d0-215">Sertifikadaki ad özelliği, sertifikanın ayırt edici konusuna karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-215">The name property on certificate corresponds with the distinguished subject for the certificate.</span></span>
* <span data-ttu-id="ae5d0-216">Depolama konumu, sertifikanın nereden yükleneceğini (`CurrentUser` veya `LocalMachine`) temsil eder.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-216">The store location represents where to load the certificate from (`CurrentUser` or `LocalMachine`).</span></span>
* <span data-ttu-id="ae5d0-217">Mağaza adı, sertifikanın depolandığı sertifika deposunun adını temsil eder.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-217">The store name represents the name of the certificate store where the certificate is stored.</span></span> <span data-ttu-id="ae5d0-218">Bu durumda, kişisel Kullanıcı deposuna işaret eder.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-218">In this case, it points to the personal user store.</span></span>

<span data-ttu-id="ae5d0-219">Azure Web siteleri 'ne dağıtmak için, gerekli Azure kaynaklarını oluşturmak ve uygulamayı üretime dağıtmak üzere uygulamayı [Azure 'A dağıtma](xref:tutorials/publish-to-azure-webapp-using-vs#deploy-the-app-to-azure) bölümündeki adımları izleyerek uygulamayı dağıtın.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-219">To deploy to Azure Websites, deploy the app following the steps in [Deploy the app to Azure](xref:tutorials/publish-to-azure-webapp-using-vs#deploy-the-app-to-azure) to create the necessary Azure resources and deploy the app to production.</span></span>

<span data-ttu-id="ae5d0-220">Yukarıdaki yönergeleri uyguladıktan sonra, uygulama Azure 'a dağıtılır ancak henüz işlevsel değildir.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-220">After following the preceding instructions, the app is deployed to Azure but isn't yet functional.</span></span> <span data-ttu-id="ae5d0-221">Uygulama tarafından kullanılan sertifikanın hala ayarlanması gerekiyor.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-221">The certificate used by the app still needs to be set up.</span></span> <span data-ttu-id="ae5d0-222">Kullanılacak sertifika için parmak izini bulun ve [sertifikalarınızı yükleme](/azure/app-service/app-service-web-ssl-cert-load#load-the-certificate-in-code)bölümünde açıklanan adımları izleyin.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-222">Locate the thumbprint for the certificate to be used, and follow the steps described in [Load your certificates](/azure/app-service/app-service-web-ssl-cert-load#load-the-certificate-in-code).</span></span>

<span data-ttu-id="ae5d0-223">Bu adımlar SSL 'den bahsetirken portalda, uygulama ile kullanmak üzere sağlanan sertifikayı karşıya yükleyebileceğiniz **özel sertifikalar** bölümü vardır.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-223">While these steps mention SSL, there's a **Private certificates** section on the portal where you can upload the provisioned certificate to use with the app.</span></span>

<span data-ttu-id="ae5d0-224">Bu adımdan sonra uygulamayı yeniden başlatın ve çalışır olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-224">After this step, restart the app and it should be functional.</span></span>

## <a name="other-configuration-options"></a><span data-ttu-id="ae5d0-225">Diğer yapılandırma seçenekleri</span><span class="sxs-lookup"><span data-stu-id="ae5d0-225">Other configuration options</span></span>

<span data-ttu-id="ae5d0-226">API yetkilendirmesi desteği, IdentityServer 'ın en üstünde bir dizi kural, varsayılan değer ve, maça deneyimini basitleştirecek geliştirmeler oluşturur.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-226">The support for API authorization builds on top of IdentityServer with a set of conventions, default values, and enhancements to simplify the experience for SPAs.</span></span> <span data-ttu-id="ae5d0-227">Daha az ki, ASP.NET Core tümleştirmeler senaryonuzu kapsamadıysanız, IdentityServer 'ın tam gücü arka planda kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-227">Needless to say, the full power of IdentityServer is available behind the scenes if the ASP.NET Core integrations don't cover your scenario.</span></span> <span data-ttu-id="ae5d0-228">ASP.NET Core desteği, tüm uygulamaların kuruluşumuza göre oluşturulduğu ve dağıtıldığı "birinci taraf" uygulamalara odaklanır.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-228">The ASP.NET Core support is focused on "first-party" apps, where all the apps are created and deployed by our organization.</span></span> <span data-ttu-id="ae5d0-229">Bu nedenle, izin veya Federasyon gibi şeyler için destek sunulmaz.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-229">As such, support isn't offered for things like consent or federation.</span></span> <span data-ttu-id="ae5d0-230">Bu senaryolar için IdentityServer kullanın ve belgelerini izleyin.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-230">For those scenarios, use IdentityServer and follow their documentation.</span></span>

### <a name="application-profiles"></a><span data-ttu-id="ae5d0-231">Uygulama profilleri</span><span class="sxs-lookup"><span data-stu-id="ae5d0-231">Application profiles</span></span>

<span data-ttu-id="ae5d0-232">Uygulama profilleri, parametrelerini daha fazla tanımlayan uygulamalar için önceden tanımlanmış yapılandırlardır.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-232">Application profiles are predefined configurations for apps that further define their parameters.</span></span> <span data-ttu-id="ae5d0-233">Şu anda, aşağıdaki profiller desteklenir:</span><span class="sxs-lookup"><span data-stu-id="ae5d0-233">At this time, the following profiles are supported:</span></span>

* <span data-ttu-id="ae5d0-234">`IdentityServerSPA`: IdentityServer 'ın yanı sıra tek bir birim olarak barındırılan bir SPA 'yı temsil eder.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-234">`IdentityServerSPA`: Represents a SPA hosted alongside IdentityServer as a single unit.</span></span>
  * <span data-ttu-id="ae5d0-235">`redirect_uri` varsayılan olarak `/authentication/login-callback`.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-235">The `redirect_uri` defaults to `/authentication/login-callback`.</span></span>
  * <span data-ttu-id="ae5d0-236">`post_logout_redirect_uri` varsayılan olarak `/authentication/logout-callback`.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-236">The `post_logout_redirect_uri` defaults to `/authentication/logout-callback`.</span></span>
  * <span data-ttu-id="ae5d0-237">Kapsam kümesi, uygulamadaki API 'Ler için tanımlanan `openid`, `profile`ve tüm kapsamları içerir.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-237">The set of scopes includes the `openid`, `profile`, and every scope defined for the APIs in the app.</span></span>
  * <span data-ttu-id="ae5d0-238">İzin verilen OıDC yanıt türleri kümesi tek tek `id_token token` veya her biri (`id_token`, `token`).</span><span class="sxs-lookup"><span data-stu-id="ae5d0-238">The set of allowed OIDC response types is `id_token token` or each of them individually (`id_token`, `token`).</span></span>
  * <span data-ttu-id="ae5d0-239">İzin verilen yanıt modu `fragment`.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-239">The allowed response mode is `fragment`.</span></span>
* <span data-ttu-id="ae5d0-240">`SPA`: IdentityServer ile barındırılmayan bir SPA 'yı temsil eder.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-240">`SPA`: Represents a SPA that isn't hosted with IdentityServer.</span></span>
  * <span data-ttu-id="ae5d0-241">Kapsam kümesi, uygulamadaki API 'Ler için tanımlanan `openid`, `profile`ve tüm kapsamları içerir.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-241">The set of scopes includes the `openid`, `profile`, and every scope defined for the APIs in the app.</span></span>
  * <span data-ttu-id="ae5d0-242">İzin verilen OıDC yanıt türleri kümesi tek tek `id_token token` veya her biri (`id_token`, `token`).</span><span class="sxs-lookup"><span data-stu-id="ae5d0-242">The set of allowed OIDC response types is `id_token token` or each of them individually (`id_token`, `token`).</span></span>
  * <span data-ttu-id="ae5d0-243">İzin verilen yanıt modu `fragment`.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-243">The allowed response mode is `fragment`.</span></span>
* <span data-ttu-id="ae5d0-244">`IdentityServerJwt`: IdentityServer ile birlikte barındırılan bir API 'YI temsil eder.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-244">`IdentityServerJwt`: Represents an API that is hosted alongside with IdentityServer.</span></span>
  * <span data-ttu-id="ae5d0-245">Uygulama, uygulama adı için varsayılan olarak kullanılan tek bir kapsama sahip olacak şekilde yapılandırılmıştır.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-245">The app is configured to have a single scope that defaults to the app name.</span></span>
* <span data-ttu-id="ae5d0-246">`API`: IdentityServer ile barındırılmayan bir API 'YI temsil eder.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-246">`API`: Represents an API that isn't hosted with IdentityServer.</span></span>
  * <span data-ttu-id="ae5d0-247">Uygulama, uygulama adı için varsayılan olarak kullanılan tek bir kapsama sahip olacak şekilde yapılandırılmıştır.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-247">The app is configured to have a single scope that defaults to the app name.</span></span>

### <a name="configuration-through-appsettings"></a><span data-ttu-id="ae5d0-248">AppSettings aracılığıyla yapılandırma</span><span class="sxs-lookup"><span data-stu-id="ae5d0-248">Configuration through AppSettings</span></span>

<span data-ttu-id="ae5d0-249">Uygulamaları `Clients` veya `Resources`listesine ekleyerek yapılandırma sistemi aracılığıyla yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-249">Configure the apps through the configuration system by adding them to the list of `Clients` or `Resources`.</span></span>

<span data-ttu-id="ae5d0-250">Aşağıdaki örnekte gösterildiği gibi, her bir istemcinin `redirect_uri` ve `post_logout_redirect_uri` özelliğini yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="ae5d0-250">Configure each client's `redirect_uri` and `post_logout_redirect_uri` property, as shown in the following example:</span></span>

```json
"IdentityServer": {
  "Clients": {
    "MySPA": {
      "Profile": "SPA",
      "RedirectUri": "https://www.example.com/authentication/login-callback",
      "LogoutUri": "https://www.example.com/authentication/logout-callback"
    }
  }
}
```

<span data-ttu-id="ae5d0-251">Kaynakları yapılandırırken, kaynak için kapsamları aşağıda gösterildiği gibi yapılandırabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="ae5d0-251">When configuring resources, you can configure the scopes for the resource as shown below:</span></span>

```json
"IdentityServer": {
  "Resources": {
    "MyExternalApi": {
      "Profile": "API",
      "Scopes": "a b c"
    }
  }
}
```

### <a name="configuration-through-code"></a><span data-ttu-id="ae5d0-252">Kod aracılığıyla yapılandırma</span><span class="sxs-lookup"><span data-stu-id="ae5d0-252">Configuration through code</span></span>

<span data-ttu-id="ae5d0-253">Ayrıca, seçenekleri yapılandırmak için bir eylem alan `AddApiAuthorization` aşırı yüklemesini kullanarak, istemcileri ve kaynakları kod aracılığıyla da yapılandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ae5d0-253">You can also configure the clients and resources through code using an overload of `AddApiAuthorization` that takes an action to configure options.</span></span>

```csharp
AddApiAuthorization<ApplicationUser, ApplicationDbContext>(options =>
{
    options.Clients.AddSPA(
        "My SPA", spa =>
        spa.WithRedirectUri("http://www.example.com/authentication/login-callback")
           .WithLogoutRedirectUri(
               "http://www.example.com/authentication/logout-callback"));

    options.ApiResources.AddApiResource("MyExternalApi", resource =>
        resource.WithScopes("a", "b", "c"));
});
```

## <a name="additional-resources"></a><span data-ttu-id="ae5d0-254">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="ae5d0-254">Additional resources</span></span>

* <xref:spa/angular>
* <xref:spa/react>
* <xref:security/authentication/scaffold-identity>
