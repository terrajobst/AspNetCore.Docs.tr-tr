---
title: ASP.NET core'da tek sayfalı uygulamalar için kimlik doğrulamaya giriş
author: javiercn
description: ASP.NET Core uygulaması içinde barındırılan bir tek sayfalı uygulama kimliğiyle kullanın.
monikerRange: '>= aspnetcore-3.0'
ms.author: scaddie
ms.custom: mvc
ms.date: 03/07/2019
uid: security/authentication/identity/spa
ms.openlocfilehash: 302a5e10a70e40e75ab9fe4b3e5a98c4e847b822
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67815227"
---
# <a name="authentication-and-authorization-for-spas"></a><span data-ttu-id="ae4e2-103">Kimlik doğrulama ve yetkilendirme Spa'lar için</span><span class="sxs-lookup"><span data-stu-id="ae4e2-103">Authentication and authorization for SPAs</span></span>

<span data-ttu-id="ae4e2-104">ASP.NET Core 3.0 veya üstü API yetkilendirme için destek kullanarak tek sayfa uygulamaları (Spa'lar) kimlik doğrulaması sunar.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-104">ASP.NET Core 3.0 or later offers authentication in Single Page Apps (SPAs) using the support for API authorization.</span></span> <span data-ttu-id="ae4e2-105">Kimlik doğrulama ve kullanıcıları depolamak için ASP.NET Core kimliği ile birlikte [IdentityServer](https://identityserver.io/) Open ID Connect uygulamak için.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-105">ASP.NET Core Identity for authenticating and storing users is combined with [IdentityServer](https://identityserver.io/) for implementing Open ID Connect.</span></span>

<span data-ttu-id="ae4e2-106">Authentication parametresi eklendi **Angular** ve **React** proje kimlik doğrulaması parametresinde benzer şablonları **Web uygulaması (Model-View-Controller)**  (MVC) ve **Web uygulaması** (Razor sayfaları) proje şablonları.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-106">An authentication parameter was added to the **Angular** and **React** project templates that is similar to the authentication parameter in the **Web Application (Model-View-Controller)** (MVC) and **Web Application** (Razor Pages) project templates.</span></span> <span data-ttu-id="ae4e2-107">İzin verilen parametre değerleri **hiçbiri** ve **bireysel**.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-107">The allowed parameter values are **None** and **Individual**.</span></span> <span data-ttu-id="ae4e2-108">**React.js ve Redux** proje şablonu, kimlik doğrulaması parametresi şu anda desteklemiyor.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-108">The **React.js and Redux** project template doesn't support the authentication parameter at this time.</span></span>

## <a name="create-an-app-with-api-authorization-support"></a><span data-ttu-id="ae4e2-109">API yetkilendirme desteği ile uygulama oluşturma</span><span class="sxs-lookup"><span data-stu-id="ae4e2-109">Create an app with API authorization support</span></span>

<span data-ttu-id="ae4e2-110">Kullanıcı kimlik doğrulaması ve yetkilendirme, Angular ve Spa'lar React ile kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-110">User authentication and authorization can be used with both Angular and React SPAs.</span></span> <span data-ttu-id="ae4e2-111">Bir komut kabuğunu açın ve aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="ae4e2-111">Open a command shell, and run the following command:</span></span>

<span data-ttu-id="ae4e2-112">**Angular**:</span><span class="sxs-lookup"><span data-stu-id="ae4e2-112">**Angular**:</span></span>

```console
dotnet new angular -o <output_directory_name> -au Individual
```

<span data-ttu-id="ae4e2-113">**React**:</span><span class="sxs-lookup"><span data-stu-id="ae4e2-113">**React**:</span></span>

```console
dotnet new react -o <output_directory_name> -au Individual
```

<span data-ttu-id="ae4e2-114">Önceki komutu ile bir ASP.NET Core uygulaması oluşturur. bir *ClientApp* SPA içeren dizin.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-114">The preceding command creates an ASP.NET Core app with a *ClientApp* directory containing the SPA.</span></span>

## <a name="general-description-of-the-aspnet-core-components-of-the-app"></a><span data-ttu-id="ae4e2-115">ASP.NET Core bileşenlerinin uygulamasının genel açıklaması</span><span class="sxs-lookup"><span data-stu-id="ae4e2-115">General description of the ASP.NET Core components of the app</span></span>

<span data-ttu-id="ae4e2-116">Aşağıdaki bölümlerde, kimlik doğrulama desteği dahil edildiğinde projeye eklemeler açıklanmaktadır:</span><span class="sxs-lookup"><span data-stu-id="ae4e2-116">The following sections describe additions to the project when authentication support is included:</span></span>

### <a name="startup-class"></a><span data-ttu-id="ae4e2-117">Başlangıç sınıfı</span><span class="sxs-lookup"><span data-stu-id="ae4e2-117">Startup class</span></span>

<span data-ttu-id="ae4e2-118">`Startup` Sınıfı aşağıdaki eklemelerle sahiptir:</span><span class="sxs-lookup"><span data-stu-id="ae4e2-118">The `Startup` class has the following additions:</span></span>

* <span data-ttu-id="ae4e2-119">İçinde `Startup.ConfigureServices` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="ae4e2-119">Inside the `Startup.ConfigureServices` method:</span></span>
  * <span data-ttu-id="ae4e2-120">Kimliği ' % s'varsayılan kullanıcı Arabirimi ile:</span><span class="sxs-lookup"><span data-stu-id="ae4e2-120">Identity with the default UI:</span></span>

    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlite(Configuration.GetConnectionString("DefaultConnection")));

    services.AddDefaultIdentity<ApplicationUser>()
        .AddDefaultUI(UIFramework.Bootstrap4)
        .AddEntityFrameworkStores<ApplicationDbContext>();
    ```

  * <span data-ttu-id="ae4e2-121">Bir ek IdentityServer `AddApiAuthorization` yardımcı yöntemi bu ayarları bazı varsayılan IdentityServer üzerinde ASP.NET Core kuralları:</span><span class="sxs-lookup"><span data-stu-id="ae4e2-121">IdentityServer with an additional `AddApiAuthorization` helper method that setups some default ASP.NET Core conventions on top of IdentityServer:</span></span>

    ```csharp
    services.AddIdentityServer()
        .AddApiAuthorization<ApplicationUser, ApplicationDbContext>();
    ```

  * <span data-ttu-id="ae4e2-122">Kimlik doğrulaması ile ek `AddIdentityServerJwt` JWT belirteçleri doğrulamak üzere uygulamayı yapılandırır yardımcı yöntemi tarafından IdentityServer üretilen:</span><span class="sxs-lookup"><span data-stu-id="ae4e2-122">Authentication with an additional `AddIdentityServerJwt` helper method that configures the app to validate JWT tokens produced by IdentityServer:</span></span>

    ```csharp
    services.AddAuthentication()
        .AddIdentityServerJwt();
    ```

* <span data-ttu-id="ae4e2-123">İçinde `Startup.Configure` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="ae4e2-123">Inside the `Startup.Configure` method:</span></span>
  * <span data-ttu-id="ae4e2-124">İstek kimlik doğrulama ve kullanıcı istek içeriğine ayarını sorumludur kimlik doğrulaması ara yazılımı:</span><span class="sxs-lookup"><span data-stu-id="ae4e2-124">The authentication middleware that is responsible for validating the request credentials and setting the user on the request context:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

  * <span data-ttu-id="ae4e2-125">Open ID Connect uç noktayı kullanıma sokan IdentityServer ara:</span><span class="sxs-lookup"><span data-stu-id="ae4e2-125">The IdentityServer middleware that exposes the Open ID Connect endpoints:</span></span>

    ```csharp
    app.UseIdentityServer();
    ```

### <a name="addapiauthorization"></a><span data-ttu-id="ae4e2-126">AddApiAuthorization</span><span class="sxs-lookup"><span data-stu-id="ae4e2-126">AddApiAuthorization</span></span>

<span data-ttu-id="ae4e2-127">Bu yardımcı yöntem IdentityServer bizim desteklenen yapılandırmasını kullanmak üzere yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-127">This helper method configures IdentityServer to use our supported configuration.</span></span> <span data-ttu-id="ae4e2-128">IdentityServer uygulama güvenlik kaygıları işleme için güçlü ve Genişletilebilir bir çerçevedir.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-128">IdentityServer is a powerful and extensible framework for handling app security concerns.</span></span> <span data-ttu-id="ae4e2-129">Aynı anda en yaygın senaryolar için gereksiz karmaşıklık kullanıma sunar.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-129">At the same time, that exposes unnecessary complexity for the most common scenarios.</span></span> <span data-ttu-id="ae4e2-130">Sonuç olarak, birtakım kuralları ve yapılandırma seçenekleri, iyi bir başlangıç noktası olarak kabul edilir sağlanır.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-130">Consequently, a set of conventions and configuration options is provided to you that are considered a good starting point.</span></span> <span data-ttu-id="ae4e2-131">Kimlik Doğrulamanızın ihtiyaçları değiştiğinde sonra IdentityServer'ın kimlik doğrulaması gereksinimlerinize göre özelleştirmek yine de kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-131">Once your authentication needs change, the full power of IdentityServer is still available to customize authentication to suit your needs.</span></span>

### <a name="addidentityserverjwt"></a><span data-ttu-id="ae4e2-132">AddIdentityServerJwt</span><span class="sxs-lookup"><span data-stu-id="ae4e2-132">AddIdentityServerJwt</span></span>

<span data-ttu-id="ae4e2-133">Bu yardımcı yöntemi, uygulama için bir ilke düzeni varsayılan kimlik doğrulaması işleyici yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-133">This helper method configures a policy scheme for the app as the default authentication handler.</span></span> <span data-ttu-id="ae4e2-134">İlke kimlik herhangi bir alt kimlik URL'si alanında yönlendirilen tüm istekleri işlemeye izin vermek için yapılandırılmış "/ kimlik".</span><span class="sxs-lookup"><span data-stu-id="ae4e2-134">The policy is configured to let Identity handle all requests routed to any subpath in the Identity URL space "/Identity".</span></span> <span data-ttu-id="ae4e2-135">`JwtBearerHandler` Diğer tüm istekleri işler.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-135">The `JwtBearerHandler` handles all other requests.</span></span> <span data-ttu-id="ae4e2-136">Ayrıca, bu yöntem kaydeder bir `<<ApplicationName>>API` IdentityServer API kaynakla varsayılan kapsamı ile `<<ApplicationName>>API` ve uygulama için IdentityServer tarafından verilen belirteçleri doğrulamak için JWT taşıyıcı belirteç ara yazılımı yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-136">Additionally, this method registers an `<<ApplicationName>>API` API resource with IdentityServer with a default scope of `<<ApplicationName>>API` and configures the JWT Bearer token middleware to validate tokens issued by IdentityServer for the app.</span></span>

### <a name="sampledatacontroller"></a><span data-ttu-id="ae4e2-137">SampleDataController</span><span class="sxs-lookup"><span data-stu-id="ae4e2-137">SampleDataController</span></span>

<span data-ttu-id="ae4e2-138">İçinde *Controllers\SampleDataController.cs* dosya, fark `[Authorize]` kullanıcı yetki verilmesi gerektiğini belirten sınıfına uygulanan bir öznitelik tabanlı kaynağa erişmek için varsayılan ilke.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-138">In the *Controllers\SampleDataController.cs* file, notice the `[Authorize]` attribute applied to the class that indicates that the user needs to be authorized based on the default policy to access the resource.</span></span> <span data-ttu-id="ae4e2-139">Varsayılan kimlik doğrulama şeması tarafından ayarlanan kullanacak şekilde yapılandırılması için varsayılan yetkilendirme ilkesi olur `AddIdentityServerJwt` yukarıda, yapmadan bahsedilen ilke şemasına `JwtBearerHandler` gibi yardımcı yöntemi tarafından yapılandırılan varsayılan işleyicisi Uygulama istekleri.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-139">The default authorization policy happens to be configured to use the default authentication scheme, which is set up by `AddIdentityServerJwt` to the policy scheme that was mentioned above, making the `JwtBearerHandler` configured by such helper method the default handler for requests to the app.</span></span>

### <a name="applicationdbcontext"></a><span data-ttu-id="ae4e2-140">ApplicationDbContext</span><span class="sxs-lookup"><span data-stu-id="ae4e2-140">ApplicationDbContext</span></span>

<span data-ttu-id="ae4e2-141">İçinde *Data\ApplicationDbContext.cs* dosya, aynı dikkat edin `DbContext` kimliğinde bunu genişleten bir özel durum ile kullanılan `ApiAuthorizationDbContext` (daha türetilmiş sınıftan `IdentityDbContext`) IdentityServer için şemayı içerecek şekilde.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-141">In the *Data\ApplicationDbContext.cs* file, notice the same `DbContext` is used in Identity with the exception that it extends `ApiAuthorizationDbContext` (a more derived class from `IdentityDbContext`) to include the schema for IdentityServer.</span></span>

<span data-ttu-id="ae4e2-142">Veritabanı şeması tam olarak denetlemek için kullanılabilen kimlik birinden devralan `DbContext` sınıfları ve bağlam çağırarak kimlik şemayı içerecek şekilde yapılandırmak `builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)` üzerinde `OnModelCreating` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-142">To gain full control of the database schema, inherit from one of the available Identity `DbContext` classes and configure the context to include the Identity schema by calling `builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)` on the `OnModelCreating` method.</span></span>

### <a name="oidcconfigurationcontroller"></a><span data-ttu-id="ae4e2-143">OidcConfigurationController</span><span class="sxs-lookup"><span data-stu-id="ae4e2-143">OidcConfigurationController</span></span>

<span data-ttu-id="ae4e2-144">İçinde *Controllers\OidcConfigurationController.cs* dosya, sağlanan istemci kullanması gereken OIDC parametreleri hizmet için uç nokta dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-144">In the *Controllers\OidcConfigurationController.cs* file, notice the endpoint that's provisioned to serve the OIDC parameters that the client needs to use.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="ae4e2-145">appsettings.json</span><span class="sxs-lookup"><span data-stu-id="ae4e2-145">appsettings.json</span></span>

<span data-ttu-id="ae4e2-146">İçinde *appsettings.json* dosya proje kökündeki var. yeni bir `IdentityServer` listesini açıklayan bölümünde yapılandırılan istemciler.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-146">In the *appsettings.json* file of the project root, there's a new `IdentityServer` section that describes the list of configured clients.</span></span> <span data-ttu-id="ae4e2-147">Aşağıdaki örnekte, tek bir istemci yoktur.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-147">In the following example, there's a single client.</span></span> <span data-ttu-id="ae4e2-148">İstemci adı uygulama adına karşılık gelir ve OAuth için kural tarafından eşleştirilen `ClientId` parametresi.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-148">The client name corresponds to the app name and is mapped by convention to the OAuth `ClientId` parameter.</span></span> <span data-ttu-id="ae4e2-149">Profil, yapılandırılan uygulama türünü belirtir.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-149">The profile indicates the app type being configured.</span></span> <span data-ttu-id="ae4e2-150">Ayrıca, sunucu için yapılandırma işlemini basitleştirmek sürücü kuralları için dahili olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-150">It's used internally to drive conventions that simplify the configuration process for the server.</span></span> <span data-ttu-id="ae4e2-151">Kullanılabilir birkaç profil, açıklandığı şekilde [uygulama profilleri](#application-profiles) bölümü.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-151">There are several profiles available, as explained in the [Application profiles](#application-profiles) section.</span></span>

```json
"IdentityServer": {
  "Clients": {
    "angularindividualpreview3final": {
      "Profile": "IdentityServerSPA"
    }
  }
}
```

### <a name="appsettingsdevelopmentjson"></a><span data-ttu-id="ae4e2-152">appSettings. Development.JSON</span><span class="sxs-lookup"><span data-stu-id="ae4e2-152">appsettings.Development.json</span></span>

<span data-ttu-id="ae4e2-153">İçinde *appsettings. Development.JSON* proje kök dosya var. bir `IdentityServer` Belirteçleri imzalamak için kullanılan anahtarı açıklayan bölümü.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-153">In the *appsettings.Development.json* file of the project root, there's an `IdentityServer` section that describes the key used to sign tokens.</span></span> <span data-ttu-id="ae4e2-154">Üretime dağıtırken, bir anahtarın sağlanabilir ve uygulama ile birlikte dağıtılan açıklandığı şekilde gerekir [üretime dağıtma](#deploy-to-production) bölümü.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-154">When deploying to production, a key needs to be provisioned and deployed alongside the app, as explained in the [Deploy to production](#deploy-to-production) section.</span></span>

```json
"IdentityServer": {
  "Key": {
    "Type": "Development"
  }
}
```

## <a name="general-description-of-the-angular-app"></a><span data-ttu-id="ae4e2-155">Angular uygulamanın genel açıklaması</span><span class="sxs-lookup"><span data-stu-id="ae4e2-155">General description of the Angular app</span></span>

<span data-ttu-id="ae4e2-156">Angular içinde API yetkilendirme ve kimlik doğrulama desteği şablon bulunduğu kendi Angular modülünde *ClientApp\src\api yetkilendirme* dizin.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-156">The authentication and API authorization support in the Angular template resides in its own Angular module in the *ClientApp\src\api-authorization* directory.</span></span> <span data-ttu-id="ae4e2-157">Modül aşağıdaki öğelerden oluşur:</span><span class="sxs-lookup"><span data-stu-id="ae4e2-157">The module is composed of the following elements:</span></span>

* <span data-ttu-id="ae4e2-158">3 bileşenler:</span><span class="sxs-lookup"><span data-stu-id="ae4e2-158">3 components:</span></span>
  * <span data-ttu-id="ae4e2-159">*Login.Component.TS*: Uygulamanın oturum açma akışını yönetir.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-159">*login.component.ts*: Handles the app's login flow.</span></span>
  * <span data-ttu-id="ae4e2-160">*Logout.Component.TS*: Uygulamanın oturum kapatma akış işler.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-160">*logout.component.ts*: Handles the app's logout flow.</span></span>
  * <span data-ttu-id="ae4e2-161">*oturum açma menu.component.ts*: Aşağıdaki adımlardan birini bağlantılarını görüntüleyen bir pencere öğesi:</span><span class="sxs-lookup"><span data-stu-id="ae4e2-161">*login-menu.component.ts*: A widget that displays one of the following sets of links:</span></span>
    * <span data-ttu-id="ae4e2-162">Kullanıcı profili yönetimi ve oturumu kapatma kullanıcının kimliği doğrulandığında bağlantıları.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-162">User profile management and log out links when the user is authenticated.</span></span>
    * <span data-ttu-id="ae4e2-163">Kayıt ve oturum açma kullanıcı kimliği değil, bağlantıları.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-163">Registration and log in links when the user isn't authenticated.</span></span>
* <span data-ttu-id="ae4e2-164">Bir rota guard `AuthorizeGuard` yolları eklenebilir ve rota gitmeden önce doğrulanmasını gerektirir.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-164">A route guard `AuthorizeGuard` that can be added to routes and requires a user to be authenticated before visiting the route.</span></span>
* <span data-ttu-id="ae4e2-165">Bir HTTP dinleyiciyi `AuthorizeInterceptor` kullanıcının kimliği doğrulandığında, API'yi hedefleyen giden HTTP isteklerini erişim belirtecini ekler.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-165">An HTTP interceptor `AuthorizeInterceptor` that attaches the access token to outgoing HTTP requests targeting the API when the user is authenticated.</span></span>
* <span data-ttu-id="ae4e2-166">Bir hizmet `AuthorizeService` kimlik doğrulama işlemi alt düzey ayrıntıları işler ve geri kalanı için tüketim için uygulama kimliği doğrulanmış kullanıcı hakkında bilgileri gösterir.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-166">A service `AuthorizeService` that handles the lower-level details of the authentication process and exposes information about the authenticated user to the rest of the app for consumption.</span></span>
* <span data-ttu-id="ae4e2-167">Uygulamanın kimlik doğrulaması bölümleriyle ilgili yolları tanımlar Angular modülü.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-167">An Angular module that defines routes associated with the authentication parts of the app.</span></span> <span data-ttu-id="ae4e2-168">Oturum açma menü bileşeni, dinleyiciyi, koruma ve uygulama geri kalanından hizmetini kullanıma sunar.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-168">It exposes the login menu component, the interceptor, the guard, and the service for consumption from the rest of the app.</span></span>

## <a name="general-description-of-the-react-app"></a><span data-ttu-id="ae4e2-169">React uygulamanın genel açıklaması</span><span class="sxs-lookup"><span data-stu-id="ae4e2-169">General description of the React app</span></span>

<span data-ttu-id="ae4e2-170">Kimlik doğrulama ve yetkilendirme React şablonu API desteği bulunan *ClientApp\src\components\api yetkilendirme* dizin.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-170">The support for authentication and API authorization in the React template resides in the *ClientApp\src\components\api-authorization* directory.</span></span> <span data-ttu-id="ae4e2-171">Bunu aşağıdaki öğelerden oluşur:</span><span class="sxs-lookup"><span data-stu-id="ae4e2-171">It's composed of the following elements:</span></span>

* <span data-ttu-id="ae4e2-172">4 bileşenler:</span><span class="sxs-lookup"><span data-stu-id="ae4e2-172">4 components:</span></span>
  * <span data-ttu-id="ae4e2-173">*Login.js*: Uygulamanın oturum açma akışını yönetir.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-173">*Login.js*: Handles the app's login flow.</span></span>
  * <span data-ttu-id="ae4e2-174">*Logout.js*: Uygulamanın oturum kapatma akış işler.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-174">*Logout.js*: Handles the app's logout flow.</span></span>
  * <span data-ttu-id="ae4e2-175">*LoginMenu.js*: Aşağıdaki adımlardan birini bağlantılarını görüntüleyen bir pencere öğesi:</span><span class="sxs-lookup"><span data-stu-id="ae4e2-175">*LoginMenu.js*: A widget that displays one of the following sets of links:</span></span>
    * <span data-ttu-id="ae4e2-176">Kullanıcı profili yönetimi ve oturumu kapatma kullanıcının kimliği doğrulandığında bağlantıları.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-176">User profile management and log out links when the user is authenticated.</span></span>
    * <span data-ttu-id="ae4e2-177">Kayıt ve oturum açma kullanıcı kimliği değil, bağlantıları.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-177">Registration and log in links when the user isn't authenticated.</span></span>
  * <span data-ttu-id="ae4e2-178">*AuthorizeRoute.js*: Belirtilen bileşen işlemeden önce doğrulanması bir kullanıcı gerektiren bir yol bileşeni `Component` parametresi.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-178">*AuthorizeRoute.js*: A route component that requires a user to be authenticated before rendering the component indicated in the `Component` parameter.</span></span>
* <span data-ttu-id="ae4e2-179">Dışarı aktarılan bir `authService` sınıfının örneğini `AuthorizeService` kimlik doğrulama işlemi alt düzey ayrıntıları işler ve geri kalanı için tüketim için uygulama kimliği doğrulanmış kullanıcı hakkında bilgileri gösterir.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-179">An exported `authService` instance of class `AuthorizeService` that handles the lower-level details of the authentication process and exposes information about the authenticated user to the rest of the app for consumption.</span></span>

<span data-ttu-id="ae4e2-180">Çözüm ana bileşenleri gördüğünüze göre uygulama için bireysel senaryolar daha ayrıntılı bir göz atabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-180">Now that you've seen the main components of the solution, you can take a deeper look at individual scenarios for the app.</span></span>

## <a name="require-authorization-on-a-new-api"></a><span data-ttu-id="ae4e2-181">Yeni bir API üzerinde Yetkilendirme gerektirir</span><span class="sxs-lookup"><span data-stu-id="ae4e2-181">Require authorization on a new API</span></span>

<span data-ttu-id="ae4e2-182">Varsayılan olarak, sistem, yetkilendirme için yeni API'leri kolayca gerektirecek şekilde yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-182">By default, the system is configured to easily require authorization for new APIs.</span></span> <span data-ttu-id="ae4e2-183">Bunu yapmak için yeni bir denetleyici oluşturma ve ekleme `[Authorize]` denetleyicideki tüm eylem veya denetleyici sınıfı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-183">To do so, create a new controller and add the `[Authorize]` attribute to the controller class or to any action within the controller.</span></span>

## <a name="protect-a-client-side-route-angular"></a><span data-ttu-id="ae4e2-184">Bir istemci-tarafı yol (Angular) koruma</span><span class="sxs-lookup"><span data-stu-id="ae4e2-184">Protect a client-side route (Angular)</span></span>

<span data-ttu-id="ae4e2-185">Bir istemci-tarafı yol koruma authorize guard bir rota yapılandırırken çalıştırılacak cf listesine ekleyerek gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-185">Protecting a client-side route is done by adding the authorize guard to the list of guards to run when configuring a route.</span></span> <span data-ttu-id="ae4e2-186">Örneğin, gördüğünüz nasıl `fetch-data` rota ana uygulama Angular modülü içinde yapılandırılır:</span><span class="sxs-lookup"><span data-stu-id="ae4e2-186">As an example, you can see how the `fetch-data` route is configured within the main app Angular module:</span></span>

```typescript
RouterModule.forRoot([
  // ...
  { path: 'fetch-data', component: FetchDataComponent, canActivate: [AuthorizeGuard] },
])
```

<span data-ttu-id="ae4e2-187">Bir rota koruma gerçek bir uç nokta koruma sağlamaz bahsetmek önemlidir (yine de gerektiren bir `[Authorize]` özniteliğinin) doğrulaması yapılmaz, belirli bir istemci-tarafı rota gezinme yalnızca kullanıcının engeller ancak bu.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-187">It's important to mention that protecting a route doesn't protect the actual endpoint (which still requires an `[Authorize]` attribute applied to it) but that it only prevents the user from navigating to the given client-side route when it isn't authenticated.</span></span>

## <a name="authenticate-api-requests-angular"></a><span data-ttu-id="ae4e2-188">API isteklerinin (Angular) kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="ae4e2-188">Authenticate API requests (Angular)</span></span>

<span data-ttu-id="ae4e2-189">Uygulamanın yanı sıra barındırılan API'lere isteklerinde kimlik doğrulama kullanarak uygulama tarafından tanımlanmış HTTP istemci dinleyiciyi otomatik olarak yapılır.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-189">Authenticating requests to APIs hosted alongside the app is done automatically through the use of the HTTP client interceptor defined by the app.</span></span>

## <a name="protect-a-client-side-route-react"></a><span data-ttu-id="ae4e2-190">Bir istemci-tarafı yol (React) koruma</span><span class="sxs-lookup"><span data-stu-id="ae4e2-190">Protect a client-side route (React)</span></span>

<span data-ttu-id="ae4e2-191">Bir istemci-tarafı rota korur `AuthorizeRoute` düz yerine bileşen `Route` bileşeni.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-191">Protect a client-side route by using the `AuthorizeRoute` component instead of the plain `Route` component.</span></span> <span data-ttu-id="ae4e2-192">Örneğin, fark nasıl `fetch-data` yol içinde yapılandırılmış `App` bileşeni:</span><span class="sxs-lookup"><span data-stu-id="ae4e2-192">For example, notice how the `fetch-data` route is configured within the `App` component:</span></span>

```jsx
<AuthorizeRoute path='/fetch-data' component={FetchData} />
```

<span data-ttu-id="ae4e2-193">Bir rota koruma:</span><span class="sxs-lookup"><span data-stu-id="ae4e2-193">Protecting a route:</span></span>

* <span data-ttu-id="ae4e2-194">Gerçek bir uç nokta koruma değil (yine de gerektiren bir `[Authorize]` özniteliğinin).</span><span class="sxs-lookup"><span data-stu-id="ae4e2-194">Doesn't protect the actual endpoint (which still requires an `[Authorize]` attribute applied to it).</span></span>
* <span data-ttu-id="ae4e2-195">Yalnızca kullanıcı doğrulaması yapılmaz, belirli bir istemci-tarafı rota gitmesini engeller.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-195">Only prevents the user from navigating to the given client-side route when it isn't authenticated.</span></span>

## <a name="authenticate-api-requests-react"></a><span data-ttu-id="ae4e2-196">API isteklerinin (React) kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="ae4e2-196">Authenticate API requests (React)</span></span>

<span data-ttu-id="ae4e2-197">React ile isteklerine kimlik doğrulaması yapılır ilk içeri aktararak `authService` gelen örnek `AuthorizeService`.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-197">Authenticating requests with React is done by first importing the `authService` instance from the `AuthorizeService`.</span></span> <span data-ttu-id="ae4e2-198">Erişim belirteci alınır `authService` ve aşağıda gösterildiği gibi isteğe bağlı.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-198">The access token is retrieved from the `authService` and is attached to the request as shown below.</span></span> <span data-ttu-id="ae4e2-199">React bileşenlerde bu iş genellikle yapılabilir `componentDidMount` yaşam döngüsü yöntemi veya kullanıcı etkileşimi sonucu olarak.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-199">In React components, this work is typically done in the `componentDidMount` lifecycle method or as the result from some user interaction.</span></span>

### <a name="import-the-authservice-into-your-component"></a><span data-ttu-id="ae4e2-200">Bileşeniniz authService içe</span><span class="sxs-lookup"><span data-stu-id="ae4e2-200">Import the authService into your component</span></span>

```javascript
import authService from './api-authorization/AuthorizeService'
```

### <a name="retrieve-and-attach-the-access-token-to-the-response"></a><span data-ttu-id="ae4e2-201">Alın ve yanıtı erişim belirteci ekleme</span><span class="sxs-lookup"><span data-stu-id="ae4e2-201">Retrieve and attach the access token to the response</span></span>

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

## <a name="deploy-to-production"></a><span data-ttu-id="ae4e2-202">Üretime dağıtma</span><span class="sxs-lookup"><span data-stu-id="ae4e2-202">Deploy to production</span></span>

<span data-ttu-id="ae4e2-203">Uygulamayı üretime dağıtmak için aşağıdaki kaynakları sağlanması gerekir:</span><span class="sxs-lookup"><span data-stu-id="ae4e2-203">To deploy the app to production, the following resources need to be provisioned:</span></span>

* <span data-ttu-id="ae4e2-204">Kimlik kullanıcı hesapları ve IdentityServer depolamak için bir veritabanı verir.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-204">A database to store the Identity user accounts and the IdentityServer grants.</span></span>
* <span data-ttu-id="ae4e2-205">Belirteçleri imzalamak için kullanmak üzere bir üretim sertifika.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-205">A production certificate to use for signing tokens.</span></span>
  * <span data-ttu-id="ae4e2-206">Bu sertifika için belirli gereksinimler yoktur; otomatik olarak imzalanan bir sertifika veya bir CA yetkilisi sağlanan bir sertifika olabilir.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-206">There are no specific requirements for this certificate; it can be a self-signed certificate or a certificate provisioned through a CA authority.</span></span>
  * <span data-ttu-id="ae4e2-207">PowerShell veya OpenSSL gibi standart araçlar aracılığıyla oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-207">It can be generated through standard tools like PowerShell or OpenSSL.</span></span>
  * <span data-ttu-id="ae4e2-208">Hedef makinelerde sertifika deposuna yüklenmesi veya olarak dağıtılan bir *.pfx* güçlü bir parola ile dosya.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-208">It can be installed into the certificate store on the target machines or deployed as a *.pfx* file with a strong password.</span></span>

### <a name="example-deploy-to-azure-websites"></a><span data-ttu-id="ae4e2-209">Örnek: Azure Web Siteleri'ne dağıtma</span><span class="sxs-lookup"><span data-stu-id="ae4e2-209">Example: Deploy to Azure Websites</span></span>

<span data-ttu-id="ae4e2-210">Bu bölümde, uygulama sertifika depolama alanında depolanan bir sertifika kullanarak Azure Web siteleri'ne dağıtma açıklanır.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-210">This section describes deploying the app to Azure websites using a certificate stored in the certificate store.</span></span> <span data-ttu-id="ae4e2-211">Sertifika deposundan bir sertifikayı yüklemek için uygulamayı değiştirmek için App Service planı, daha sonraki bir adımda yapılandırdığınızda üzerinde en az standart katmanı olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-211">To modify the app to load a certificate from the certificate store, the App Service plan needs to be on at least the Standard tier when you configure in a later step.</span></span> <span data-ttu-id="ae4e2-212">Uygulamanın *appsettings.json* dosya, değişiklik `IdentityServer` bölümü önemli ayrıntıları içerir:</span><span class="sxs-lookup"><span data-stu-id="ae4e2-212">In the app's *appsettings.json* file, modify the `IdentityServer` section to include the key details:</span></span>

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

* <span data-ttu-id="ae4e2-213">Sertifika adı özelliği ayırt edici konu sertifikasının karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-213">The name property on certificate corresponds with the distinguished subject for the certificate.</span></span>
* <span data-ttu-id="ae4e2-214">Sertifikadan yükleneceği depolama konumu temsil eder (`CurrentUser` veya `LocalMachine`).</span><span class="sxs-lookup"><span data-stu-id="ae4e2-214">The store location represents where to load the certificate from (`CurrentUser` or `LocalMachine`).</span></span>
* <span data-ttu-id="ae4e2-215">Depo adını sertifikanın depolandığı sertifika deposunun adını temsil eder.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-215">The store name represents the name of the certificate store where the certificate is stored.</span></span> <span data-ttu-id="ae4e2-216">Bu durumda, kullanıcı Kişisel depoya işaret eder.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-216">In this case, it points to the personal user store.</span></span>

<span data-ttu-id="ae4e2-217">Yer alan adımları uygulayarak bir uygulamayı Azure Web siteleri'ne dağıtma [uygulamasını Azure'a dağıtma](xref:tutorials/publish-to-azure-webapp-using-vs#deploy-the-app-to-azure) gerekli Azure kaynakları oluşturmak ve uygulamayı üretime dağıtın.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-217">To deploy to Azure Websites, deploy the app following the steps in [Deploy the app to Azure](xref:tutorials/publish-to-azure-webapp-using-vs#deploy-the-app-to-azure) to create the necessary Azure resources and deploy the app to production.</span></span>

<span data-ttu-id="ae4e2-218">Yukarıdaki yönergeleri izleyerek sonra uygulamayı Azure'a dağıtılır, ancak henüz işlevsel değil.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-218">After following the preceding instructions, the app is deployed to Azure but isn't yet functional.</span></span> <span data-ttu-id="ae4e2-219">Uygulama tarafından kullanılan sertifikayı yine de ayarlanması gerekir.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-219">The certificate used by the app still needs to be set up.</span></span> <span data-ttu-id="ae4e2-220">Kullanılacak sertifika parmak izini bulun ve açıklanan adımları [sertifikalarınızı yüklemek](/azure/app-service/app-service-web-ssl-cert-load#load-the-certificate-in-code).</span><span class="sxs-lookup"><span data-stu-id="ae4e2-220">Locate the thumbprint for the certificate to be used, and follow the steps described in [Load your certificates](/azure/app-service/app-service-web-ssl-cert-load#load-the-certificate-in-code).</span></span>

<span data-ttu-id="ae4e2-221">While bu adımları bahsetmek SSL, bir **özel sertifikaları** Portal karşıya yüklersiniz uygulamayla kullanmak için sağlanan sertifikanın bölümü.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-221">While these steps mention SSL, there's a **Private certificates** section on the portal where you can upload the provisioned certificate to use with the app.</span></span>

<span data-ttu-id="ae4e2-222">Bu adımdan sonra uygulamayı yeniden başlatın ve işlev olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-222">After this step, restart the app and it should be functional.</span></span>

## <a name="other-configuration-options"></a><span data-ttu-id="ae4e2-223">Diğer yapılandırma seçenekleri</span><span class="sxs-lookup"><span data-stu-id="ae4e2-223">Other configuration options</span></span>

<span data-ttu-id="ae4e2-224">API yetkilendirme için destek IdentityServer üzerine kuralları, varsayılan değerleri ve geliştirmeleri Spa'lar deneyimini basitleştirmek için bir dizi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-224">The support for API authorization builds on top of IdentityServer with a set of conventions, default values, and enhancements to simplify the experience for SPAs.</span></span> <span data-ttu-id="ae4e2-225">Deyin needless için ASP.NET Core tümleştirmeler senaryonuz erişmezse IdentityServer'ın arka planda kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-225">Needless to say, the full power of IdentityServer is available behind the scenes if the ASP.NET Core integrations don't cover your scenario.</span></span> <span data-ttu-id="ae4e2-226">ASP.NET Core desteği kuruluşumuz tarafından dağıtılan tüm uygulamalar burada oluşturulan ve "Birinci taraf" uygulamaları odaklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-226">The ASP.NET Core support is focused on "first-party" apps, where all the apps are created and deployed by our organization.</span></span> <span data-ttu-id="ae4e2-227">Bu nedenle, destek, onay ve Federasyon gibi şeyler için sunulan değil.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-227">As such, support isn't offered for things like consent or federation.</span></span> <span data-ttu-id="ae4e2-228">Bu senaryolar için IdentityServer kullanın ve kendi belgelerini izleyin.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-228">For those scenarios, use IdentityServer and follow their documentation.</span></span>

### <a name="application-profiles"></a><span data-ttu-id="ae4e2-229">Uygulama profilleri</span><span class="sxs-lookup"><span data-stu-id="ae4e2-229">Application profiles</span></span>

<span data-ttu-id="ae4e2-230">Uygulama profilleri, kendi parametrelerini daha tanımlamanız uygulamalar için önceden tanımlanmış yapılandırmalardır.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-230">Application profiles are predefined configurations for apps that further define their parameters.</span></span> <span data-ttu-id="ae4e2-231">Şu anda aşağıdaki profilleri desteklenir:</span><span class="sxs-lookup"><span data-stu-id="ae4e2-231">At this time, the following profiles are supported:</span></span>

* <span data-ttu-id="ae4e2-232">`IdentityServerSPA`: Tek bir birim olarak IdentityServer yanı sıra barındırılan bir SPA temsil eder.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-232">`IdentityServerSPA`: Represents a SPA hosted alongside IdentityServer as a single unit.</span></span>
  * <span data-ttu-id="ae4e2-233">`redirect_uri` Varsayılan olarak `/authentication/login-callback`.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-233">The `redirect_uri` defaults to `/authentication/login-callback`.</span></span>
  * <span data-ttu-id="ae4e2-234">`post_logout_redirect_uri` Varsayılan olarak `/authentication/logout-callback`.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-234">The `post_logout_redirect_uri` defaults to `/authentication/logout-callback`.</span></span>
  * <span data-ttu-id="ae4e2-235">Kapsamları kümesini içeren `openid`, `profile`ve uygulama API'leri için tanımlanan her kapsam.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-235">The set of scopes includes the `openid`, `profile`, and every scope defined for the APIs in the app.</span></span>
  * <span data-ttu-id="ae4e2-236">İzin verilen OIDC yanıt türleri kümesi `id_token token` veya bunların her birini tek tek (`id_token`, `token`).</span><span class="sxs-lookup"><span data-stu-id="ae4e2-236">The set of allowed OIDC response types is `id_token token` or each of them individually (`id_token`, `token`).</span></span>
  * <span data-ttu-id="ae4e2-237">İzin verilen yanıt modu `fragment`.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-237">The allowed response mode is `fragment`.</span></span>
* <span data-ttu-id="ae4e2-238">`SPA`: Barındırılan değil bir SPA IdentityServer ile temsil eder.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-238">`SPA`: Represents a SPA that isn't hosted with IdentityServer.</span></span>
  * <span data-ttu-id="ae4e2-239">Kapsamları kümesini içeren `openid`, `profile`ve uygulama API'leri için tanımlanan her kapsam.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-239">The set of scopes includes the `openid`, `profile`, and every scope defined for the APIs in the app.</span></span>
  * <span data-ttu-id="ae4e2-240">İzin verilen OIDC yanıt türleri kümesi `id_token token` veya bunların her birini tek tek (`id_token`, `token`).</span><span class="sxs-lookup"><span data-stu-id="ae4e2-240">The set of allowed OIDC response types is `id_token token` or each of them individually (`id_token`, `token`).</span></span>
  * <span data-ttu-id="ae4e2-241">İzin verilen yanıt modu `fragment`.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-241">The allowed response mode is `fragment`.</span></span>
* <span data-ttu-id="ae4e2-242">`IdentityServerJwt`: Birlikte barındırılan bir API ile IdentityServer temsil eder.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-242">`IdentityServerJwt`: Represents an API that is hosted alongside with IdentityServer.</span></span>
  * <span data-ttu-id="ae4e2-243">Uygulama, uygulama adı için varsayılan olarak tek bir kapsam sahip olacak şekilde yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-243">The app is configured to have a single scope that defaults to the app name.</span></span>
* <span data-ttu-id="ae4e2-244">`API`: Barındırılan değil bir API ile IdentityServer temsil eder.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-244">`API`: Represents an API that isn't hosted with IdentityServer.</span></span>
  * <span data-ttu-id="ae4e2-245">Uygulama, uygulama adı için varsayılan olarak tek bir kapsam sahip olacak şekilde yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-245">The app is configured to have a single scope that defaults to the app name.</span></span>

### <a name="configuration-through-appsettings"></a><span data-ttu-id="ae4e2-246">AppSettings aracılığıyla yapılandırma</span><span class="sxs-lookup"><span data-stu-id="ae4e2-246">Configuration through AppSettings</span></span>

<span data-ttu-id="ae4e2-247">Yapılandırma sistemi aracılığıyla uygulamalar listesine ekleyerek yapılandırma `Clients` veya `Resources`.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-247">Configure the apps through the configuration system by adding them to the list of `Clients` or `Resources`.</span></span>

<span data-ttu-id="ae4e2-248">Her istemcinin yapılandırma `redirect_uri` ve `post_logout_redirect_uri` aşağıdaki örnekte gösterildiği gibi özelliği:</span><span class="sxs-lookup"><span data-stu-id="ae4e2-248">Configure each client's `redirect_uri` and `post_logout_redirect_uri` property, as shown in the following example:</span></span>

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

<span data-ttu-id="ae4e2-249">Kaynakları yapılandırırken, kaynak için kapsamları aşağıda gösterildiği gibi yapılandırabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="ae4e2-249">When configuring resources, you can configure the scopes for the resource as shown below:</span></span>

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

### <a name="configuration-through-code"></a><span data-ttu-id="ae4e2-250">Kod ile yapılandırma</span><span class="sxs-lookup"><span data-stu-id="ae4e2-250">Configuration through code</span></span>

<span data-ttu-id="ae4e2-251">İstemciler ve kaynakları bir aşırı yüklemesini kullanarak kod aracılığıyla da yapılandırabilirsiniz `AddApiAuthorization` seçeneklerini yapılandırmak için bir eylemi alır.</span><span class="sxs-lookup"><span data-stu-id="ae4e2-251">You can also configure the clients and resources through code using an overload of `AddApiAuthorization` that takes an action to configure options.</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="ae4e2-252">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="ae4e2-252">Additional resources</span></span>

* <xref:spa/angular>
* <xref:spa/react>
