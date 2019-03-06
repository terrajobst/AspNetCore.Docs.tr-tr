---
title: ASP.NET core'da tek sayfalık uygulamalar için kimlik doğrulamaya giriş
author: ''
description: ASP.NET Core uygulaması içinde barındırılan bir tek sayfalı uygulama kimlik kullanın.
ms.author: ''
ms.date: 03/05/2018
uid: security/authentication/identity/spa
ms.openlocfilehash: cf04ec1ff0ae9afea066fd1864ab0a7956ace32c
ms.sourcegitcommit: 036d4b03fd86ca5bb378198e29ecf2704257f7b2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2019
ms.locfileid: "57346820"
---
# <a name="authentication-and-authorization-for-spas"></a><span data-ttu-id="86296-103">Kimlik doğrulama ve yetkilendirme Spa'lar için</span><span class="sxs-lookup"><span data-stu-id="86296-103">Authentication and authorization for SPAs</span></span>

<span data-ttu-id="86296-104">ASP.NET 3. 0'yeni desteğimiz için API yetkilendirme kullanarak tek sayfalı uygulama kimlik doğrulaması için destek sunuyoruz.</span><span class="sxs-lookup"><span data-stu-id="86296-104">In ASP.NET 3.0 we are introducing support for authentication in single page applications using our new support for API authorization.</span></span> <span data-ttu-id="86296-105">Bu destek için kimlik doğrulama ve kullanıcıları ve kimlik sunucusu Open ID Connect uygulamak için depolama üzerinde ASP.NET Core kimliği birleşimini temel alır.</span><span class="sxs-lookup"><span data-stu-id="86296-105">This support is based on a combination of ASP.NET Core Identity for authenticating and storing users and Identity Server for implementing Open ID Connect.</span></span>

<span data-ttu-id="86296-106">Yeni bir kimlik doğrulama parametresi için sunduğumuz Angular ekledik ve mvc ve razor sayfaları şablonlarımız ile kimlik doğrulaması parametresinde benzer React şablonları izin verilen değerler 'None' ve 'Bireysel'.</span><span class="sxs-lookup"><span data-stu-id="86296-106">We have added a new authentication parameter to our Angular and React templates that is similar to the authentication parameter in our mvc and razor pages templates with allowed values 'None' and 'Individual'.</span></span>

## <a name="create-an-angular-app-with-api-authorization-support"></a><span data-ttu-id="86296-107">API ile yetkilendirme desteği Angular uygulama oluşturma</span><span class="sxs-lookup"><span data-stu-id="86296-107">Create an Angular app with API authorization support</span></span>

<span data-ttu-id="86296-108">Kimlik doğrulama ve kullanıcıları yetkilendirme desteğiyle yeni Angular uygulaması oluşturmak için bir komut kabuğunu açın ve aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="86296-108">To create a new Angular app with support for authentication and authorization of users, open a command shell and run the following command:</span></span>

```console
dotnet new angular -o <output_directory_name> -au Individual
```

<span data-ttu-id="86296-109">Önceki komutu ile bir ASP.NET Core uygulaması oluşturur. bir *ClientApp* Angular uygulamasını içeren dizin.</span><span class="sxs-lookup"><span data-stu-id="86296-109">The preceding command creates an ASP.NET Core app with a *ClientApp* directory containing the Angular app.</span></span>

## <a name="create-a-react-app-with-api-authorization-support"></a><span data-ttu-id="86296-110">API yetkilendirme desteğiyle bir React uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="86296-110">Create a React app with API authorization support</span></span>

<span data-ttu-id="86296-111">Kimlik doğrulama ve kullanıcıları yetkilendirme desteği ile yeni bir React uygulaması oluşturmak için bir komut kabuğunu açın ve aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="86296-111">To create a new React app with support for authentication and authorization of users, open a command shell and run the following command:</span></span>

```console
dotnet new react -o <output_directory_name> -au Individual
```

<span data-ttu-id="86296-112">Önceki komutu ile bir ASP.NET Core uygulaması oluşturur. bir *ClientApp* React uygulamayı içeren dizin.</span><span class="sxs-lookup"><span data-stu-id="86296-112">The preceding command creates an ASP.NET Core app with a *ClientApp* directory containing the React app.</span></span>

## <a name="general-description-of-the-aspnet-core-components-of-the-app"></a><span data-ttu-id="86296-113">ASP.NET Core bileşenlerinin uygulamasının genel açıklaması</span><span class="sxs-lookup"><span data-stu-id="86296-113">General description of the ASP.NET Core components of the app</span></span>

<span data-ttu-id="86296-114">Kimlik doğrulaması için destek ekliyoruz, birkaç proje eklemeler vardır:</span><span class="sxs-lookup"><span data-stu-id="86296-114">There are several additions to the project when we include support for authentication:</span></span>

### <a name="startup-class"></a><span data-ttu-id="86296-115">Başlangıç sınıfı</span><span class="sxs-lookup"><span data-stu-id="86296-115">Startup class</span></span>

<span data-ttu-id="86296-116">Başlangıç sınıfı aşağıdaki kodda baktığımızda aşağıdaki eklemeler veriyoruz:</span><span class="sxs-lookup"><span data-stu-id="86296-116">If we look at the code in the Startup class below we can appreciate the following inclusions:</span></span>
* <span data-ttu-id="86296-117">İçinde `public void ConfigureServices(IServiceCollection services)`:</span><span class="sxs-lookup"><span data-stu-id="86296-117">Inside `public void ConfigureServices(IServiceCollection services)`:</span></span>
  * <span data-ttu-id="86296-118">Kimliği ' % s'varsayılan kullanıcı Arabirimi ile.</span><span class="sxs-lookup"><span data-stu-id="86296-118">Identity with the default UI.</span></span>
    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
      options.UseSqlite(Configuration.GetConnectionString("DefaultConnection")));

    services.AddDefaultIdentity<ApplicationUser>()
      .AddDefaultUI(UIFramework.Bootstrap4)
      .AddEntityFrameworkStores<ApplicationDbContext>();
    ```
  * <span data-ttu-id="86296-119">Kimlik sunucusu ile ek bir AddApiAuthorization yardımcı yöntem bu ayarları bazı varsayılan kimlik Server üzerinde ASP.NET kuralları.</span><span class="sxs-lookup"><span data-stu-id="86296-119">Identity Server with an additional AddApiAuthorization helper method that setups some default ASP.NET Conventions on top of Identity Server.</span></span>
    ```csharp
    services.AddIdentityServer()
      .AddApiAuthorization<ApplicationUser, ApplicationDbContext>();
    ```
  * <span data-ttu-id="86296-120">Kimlik doğrulaması ile kimlik sunucusu tarafından üretilen Jwt belirteçleri doğrulamak üzere uygulamayı yapılandırır bir ek AddIdentityServerJwt yardımcı yöntemi.</span><span class="sxs-lookup"><span data-stu-id="86296-120">Authentication with an additional AddIdentityServerJwt helper method that configures the application to validate Jwt tokens produced by Identity Server.</span></span> 
    ```csharp
    services.AddAuthentication()
      .AddIdentityServerJwt();
    ```
* <span data-ttu-id="86296-121">İçinde `public void Configure(IApplicationBuilder app)`:</span><span class="sxs-lookup"><span data-stu-id="86296-121">Inside `public void Configure(IApplicationBuilder app)`:</span></span>
  * <span data-ttu-id="86296-122">Gelen istek kimlik doğrulama ve kullanıcı istek içeriğine ayarını sorumludur kimlik doğrulaması ara yazılımı.</span><span class="sxs-lookup"><span data-stu-id="86296-122">The authentication middleware that is responsible for validating the credentials in the incoming request and setting the user on the request context.</span></span>
    ```csharp
    app.UseAuthentication();
    ```
  * <span data-ttu-id="86296-123">Open ID Connect uç noktayı kullanıma sokan kimlik sunucusu ara yazılımı.</span><span class="sxs-lookup"><span data-stu-id="86296-123">The identity server middleware that exposes the Open ID Connect endpoints.</span></span>
    ```csharp
    app.UseIdentityServer();
    ```

### <a name="addapiauthorization"></a><span data-ttu-id="86296-124">AddApiAuthorization</span><span class="sxs-lookup"><span data-stu-id="86296-124">AddApiAuthorization</span></span> 
<span data-ttu-id="86296-125">Bu yardımcı yöntem kimlik Server'ın desteklenen bizim yapılandırmasını kullanmak üzere yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="86296-125">This helper method configures Identity Server to use our supported configuration.</span></span> <span data-ttu-id="86296-126">Kimlik sunucusu uygulama güvenlik kaygıları işlemek için çok güçlü ve Genişletilebilir bir çerçeve olan ancak hakkında bilgi edinmek için en yaygın senaryolar için ihtiyaç duymayacağımız karmaşıklık çok sunan aynı zamanda, bu nedenle Seçtiğimiz kuralları kümesi ve yapılandırma seçenekleri düşünüyoruz sizin için iyi bir başlangıç noktasıdır.</span><span class="sxs-lookup"><span data-stu-id="86296-126">Identity Server is a very powerful and extensible framework for handling application security concerns but at the same time that exposes a lot of complexity that we don't need to know about for the most common scenarios, so we choose a set of conventions and configuration options for you that we consider are a good starting point.</span></span> <span data-ttu-id="86296-127">Kimlik Doğrulamanızın ihtiyaçları değiştiğinde sonra şekilde gereksinimlerinize uyacak şekilde özelleştirebilirsiniz gücünden türde kimlik sunucusu için yine de kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="86296-127">Once your authentication needs change the full power of Identity Server is still available to you so you can customize it to suit your needs.</span></span>

### <a name="addidentityserverjwt"></a><span data-ttu-id="86296-128">AddIdentityServerJwt</span><span class="sxs-lookup"><span data-stu-id="86296-128">AddIdentityServerJwt</span></span>
<span data-ttu-id="86296-129">Bu yardımcı yöntemi, uygulama için bir ilke düzeni varsayılan kimlik doğrulaması işleyici yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="86296-129">This helper method configures a policy scheme for the application as the default authentication handler.</span></span> <span data-ttu-id="86296-130">İlke kimlik herhangi bir alt kimlik URL'si alanında Git tüm istekleri işlemeye izin vermek için yapılandırılmış "/ kimlik" ve diğer tüm istekleri işleyen JwtBearerHandler izin vermek için.</span><span class="sxs-lookup"><span data-stu-id="86296-130">The policy is configured to let identity handle all the requests that go to any subpath in the Identity url space "/Identity" and to let the JwtBearerHandler handle all other requests.</span></span>
<span data-ttu-id="86296-131">Bu yöntem kaydeder Addionally bir `<<ApplicationName>>API` kimlik sunucusu varsayılan kapsamı ile API kaynak `<<ApplicationName>>API` ve uygulama için kimlik sunucusu tarafından verilen belirteçleri doğrulamak için JWT taşıyıcı belirteç ara yazılımı yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="86296-131">Addionally this method registers an `<<ApplicationName>>API` Api resource with identity server with a default scope of `<<ApplicationName>>API` and configures the JWT Bearer token middleware to validate tokens issued by Identity Server for the application.</span></span>

### <a name="sampledatacontroller"></a><span data-ttu-id="86296-132">SampleDataController</span><span class="sxs-lookup"><span data-stu-id="86296-132">SampleDataController</span></span>
<span data-ttu-id="86296-133">Dosyayı Controllers\SampleDataController.cs baktığımızda, biz inceleyebileceğiniz `[Authorize]` kullanıcı yetki verilmesi gerektiğini belirten sınıfına uygulanan bir öznitelik tabanlı kaynağa erişmek için varsayılan ilke.</span><span class="sxs-lookup"><span data-stu-id="86296-133">If we look at the file Controllers\SampleDataController.cs we can observe the `[Authorize]` attribute applied to the class that indicates that the user needs to be authorized based on the default policy to access the resource.</span></span> <span data-ttu-id="86296-134">Varsayılan yetkilendirme ilkesi tarafından ayarlanan varsayılan kimlik doğrulama düzeni kullanmak üzere yapılandırılmış olur `AddIdentityServerJwt` biz yukarıda bahsedilen ilke düzenine JwtBearer işleyici yapma gibi yardımcı yöntemi tarafından varsayılan işleyici için yapılandırılmış Uygulama istekleri.</span><span class="sxs-lookup"><span data-stu-id="86296-134">The default authorization policy happens to be configured to use the default authentication scheme which is set up by `AddIdentityServerJwt` to the policy scheme that we mentioned above, making the JwtBearer handler configured by such helper method the default handler for requests to the app.</span></span>

### <a name="applicationdbcontext"></a><span data-ttu-id="86296-135">ApplicationDbContext</span><span class="sxs-lookup"><span data-stu-id="86296-135">ApplicationDbContext</span></span>
<span data-ttu-id="86296-136">Data\ApplicationDbContext.cs dosyasında baktığımızda kullandığımız bu durumun Kimlik'te ApiAuthorizationDbContext (Identitydbcontext daha türetilmiş bir sınıftan) genişletir aynı Dbcontext'e görebiliriz kimlik sunucusu için şemayı içerecek şekilde.</span><span class="sxs-lookup"><span data-stu-id="86296-136">If we look at the file in Data\ApplicationDbContext.cs we can see the same DbContext we use in identity with the exception that it extends ApiAuthorizationDbContext (a more derived class from IdentityDbContext) to include the schema for Identity Server.</span></span>
<span data-ttu-id="86296-137">Veritabanı şeması tam denetim istiyoruz biz oluşabilir yalnızca kullanılabilir kimlik DbContext sınıflarının birinden devralan ve bağlam çağırarak kimlik şemayı içerecek şekilde yapılandırmak `builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)` üzerinde `OnModelCreating` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="86296-137">If we want full control of the database schema we can simply inherit from one of the available Identity DbContext classes and configure the context to include the identity schema by calling `builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)` on the `OnModelCreating` method.</span></span>

### <a name="oidcconfigurationcontroller"></a><span data-ttu-id="86296-138">OidcConfigurationController</span><span class="sxs-lookup"><span data-stu-id="86296-138">OidcConfigurationController</span></span>
<span data-ttu-id="86296-139">Biz bakarsanız dosyasını şu uç nokta görebilirsiniz Controllers\OidcConfigurationController.cs olduğumuz istemci kullanması gereken OIDC parametreleri hizmet ayakta.</span><span class="sxs-lookup"><span data-stu-id="86296-139">If we look at the file Controllers\OidcConfigurationController.cs we can see the endpoint that we stand-up to serve the OIDC parameters that the client needs to use.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="86296-140">appsettings.json</span><span class="sxs-lookup"><span data-stu-id="86296-140">appsettings.json</span></span>
<span data-ttu-id="86296-141">Proje kökündeki appsettings.json dosyasını baktığımızda, yeni bir görebiliriz `IdentityServer` listesini açıklayan bölümü yapılandırılmış istemciler ve tek bir istemci olduğunu görebiliriz.</span><span class="sxs-lookup"><span data-stu-id="86296-141">If we look at the appsettings.json file on the root of the project, we can see a new `IdentityServer` section that describes the list of configured clients and we can see that there is a single client.</span></span> <span data-ttu-id="86296-142">İstemci uygulama adına karşılık gelir ve oAuth ClientID parametresi için kural olarak eşleştirilir.</span><span class="sxs-lookup"><span data-stu-id="86296-142">The name of the client corresponds to the name of the application and is mapped by convention to the oAuth ClientId parameter.</span></span> <span data-ttu-id="86296-143">Yapılandırmakta olduğunuz uygulamanın türüne profili gösterir ve bunu dahili olarak sunucu için yapılandırma işlemini basitleştirmek sürücü kuralları için kullanırız.</span><span class="sxs-lookup"><span data-stu-id="86296-143">The profile indicates what type of application we are configuring, and we use it internally to drive conventions that simplify the configuration process for the server.</span></span> <span data-ttu-id="86296-144">Birkaç profil kullanılabilir aşağıdaki bölümde açıklanan vardır.</span><span class="sxs-lookup"><span data-stu-id="86296-144">There are several profiles available explained in the section below.</span></span>

```json
"IdentityServer": {
  "Clients": {
    "angularindividualpreview3final": {
      "Profile": "IdentityServerSPA"
    }
  }
}
```

### <a name="appsettingsdevelopmentjson"></a><span data-ttu-id="86296-145">appSettings. Development.JSON</span><span class="sxs-lookup"><span data-stu-id="86296-145">appsettings.Development.json</span></span>
<span data-ttu-id="86296-146">Biz appsettings bakarsanız. Development.JSON dosya projenin kökünde, yeni bir görebiliriz `IdentityServer` Belirteçleri imzalamak için kullanıyoruz anahtar açıklayan bölümü.</span><span class="sxs-lookup"><span data-stu-id="86296-146">If we look at the appsettings.Development.json file on the root of the project, we can see a new `IdentityServer` section that describes the key we are using to sign tokens.</span></span> <span data-ttu-id="86296-147">Üretime dağıtırken bir anahtar sağlanır ve aşağıda açıklandığı gibi uygulamanın yanı sıra dağıtılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="86296-147">When deploying to production a key needs to be provisioned and deployed alongside the application as explained below.</span></span>

```json
  "IdentityServer": {
    "Key": {
      "Type": "Development"
    }
  }
}
```

## <a name="general-description-of-the-angular-application"></a><span data-ttu-id="86296-148">Angular uygulamasını genel açıklaması</span><span class="sxs-lookup"><span data-stu-id="86296-148">General description of the Angular application</span></span>
<span data-ttu-id="86296-149">Kimlik doğrulama ve yetkilendirme Angular şablonu API desteği, kendi Angular modülünde yaşar.</span><span class="sxs-lookup"><span data-stu-id="86296-149">The support for authentication and API authorization in the Angular template lives in its own Angular module.</span></span> <span data-ttu-id="86296-150">ClientApp\src\api yetkilendirme ve bunun altında aşağıdaki öğelerden oluşur:</span><span class="sxs-lookup"><span data-stu-id="86296-150">Under ClientApp\src\api-authorization and it is composed of the following elements:</span></span>
* <span data-ttu-id="86296-151">3 bileşenler:</span><span class="sxs-lookup"><span data-stu-id="86296-151">3 Components:</span></span>
  * <span data-ttu-id="86296-152">Oturum açma bileşeni: Uygulama için oturum açma akışını yönetir.</span><span class="sxs-lookup"><span data-stu-id="86296-152">Login component: Handles the login flow for the app.</span></span>
  * <span data-ttu-id="86296-153">Oturum kapatma bileşeni: Uygulama oturum kapatma akış işler.</span><span class="sxs-lookup"><span data-stu-id="86296-153">Logout component: Handles the logout flow for the app.</span></span>
  * <span data-ttu-id="86296-154">Oturum açma menü bileşeni: Geçerli görüntüleyen bir pencere öğesi kullanıcı profili yönetip oturumunuzu bağlantıları ile kullanıcının kimlik doğrulamasını ya da oturum açın veya kullanıcı kimliği doğrulanmamış olduğunda kaydetmek için bağlar.</span><span class="sxs-lookup"><span data-stu-id="86296-154">Login menu component: A widget that displays the current authenticated user with links to manage the user profile and log out or links to log in or register when the user is not authenticated.</span></span>
* <span data-ttu-id="86296-155">Bir rota guard `AuthorizeGuard` yolları eklenebilir ve rota gitmeden önce doğrulanmasını gerektirir.</span><span class="sxs-lookup"><span data-stu-id="86296-155">A route guard `AuthorizeGuard` that can be added to routes and requires a user to be authenticated before visiting the route.</span></span>
* <span data-ttu-id="86296-156">Bir http dinleyiciyi `AuthorizeInterceptor` kullanıcının kimliği doğrulandığında, API'yi hedefleyen giden HTTP isteklerini erişim belirtecini ekler.</span><span class="sxs-lookup"><span data-stu-id="86296-156">An http interceptor `AuthorizeInterceptor` that attaches the access token to outgoing HTTP requests targeting the API when the user is authenticated.</span></span>
* <span data-ttu-id="86296-157">Bir hizmet `AuthorizeService` kimlik doğrulama işlemi alt düzey ayrıntıları işler ve geri kalanı için uygulama tüketim için kimliği doğrulanmış kullanıcı hakkında bilgileri gösterir.</span><span class="sxs-lookup"><span data-stu-id="86296-157">A service `AuthorizeService` that handles the lower level details of the authentication process and exposes information about the authenticated user to the rest of the application for consumption.</span></span>
* <span data-ttu-id="86296-158">Yollar tanımlayan bir angular modülü, uygulamanın kimlik doğrulaması bölümlerine ile ilişkili ve oturum açma menü bileşeni, dinleyiciyi, koruma ve uygulama geri kalanından hizmetini kullanıma sunar.</span><span class="sxs-lookup"><span data-stu-id="86296-158">An angular module that defines routes associated with the authentication parts of the application and exposes the login menu component, the interceptor, the guard and the service for consumption from the rest of the application.</span></span>

## <a name="general-description-of-the-react-application"></a><span data-ttu-id="86296-159">React uygulamanın genel açıklaması</span><span class="sxs-lookup"><span data-stu-id="86296-159">General description of the React application</span></span>
<span data-ttu-id="86296-160">Kimlik doğrulama ve yetkilendirme ClientApp\src\components\api authorization\ ve altında React şablonu olduğu API desteği aşağıdaki öğelerden oluşur:</span><span class="sxs-lookup"><span data-stu-id="86296-160">The support for authentication and API authorization in the React template lives under ClientApp\src\components\api-authorization\ and it is composed of the following elements:</span></span>
* <span data-ttu-id="86296-161">4 bileşenler:</span><span class="sxs-lookup"><span data-stu-id="86296-161">4 Components:</span></span>
  * <span data-ttu-id="86296-162">Oturum açma bileşeni: Uygulama için oturum açma akışını yönetir.</span><span class="sxs-lookup"><span data-stu-id="86296-162">Login component: Handles the login flow for the app.</span></span>
  * <span data-ttu-id="86296-163">Oturum kapatma bileşeni: Uygulama oturum kapatma akış işler.</span><span class="sxs-lookup"><span data-stu-id="86296-163">Logout component: Handles the logout flow for the app.</span></span>
  * <span data-ttu-id="86296-164">Oturum açma menü bileşeni: Geçerli görüntüleyen bir pencere öğesi kullanıcı profili yönetip oturumunuzu bağlantıları ile kullanıcının kimlik doğrulamasını ya da oturum açın veya kullanıcı kimliği doğrulanmamış olduğunda kaydetmek için bağlar.</span><span class="sxs-lookup"><span data-stu-id="86296-164">Login menu component: A widget that displays the current authenticated user with links to manage the user profile and log out or links to log in or register when the user is not authenticated.</span></span>
  * <span data-ttu-id="86296-165">AuthorizeRoute: Bileşen işlemeden önce doğrulanması bir kullanıcı gerektiren bir yol bileşeni bileşen parametresinde belirtilen.</span><span class="sxs-lookup"><span data-stu-id="86296-165">AuthorizeRoute: A route component that requires a user to be authenticated before rendering the component indicated in the Component parameter.</span></span>
  * <span data-ttu-id="86296-166">Dışarı aktarılan bir `authService` sınıfının örneğini `AuthorizeService` kimlik doğrulama işlemi alt düzey ayrıntıları işler ve geri kalanı için uygulama tüketim için kimliği doğrulanmış kullanıcı hakkında bilgileri gösterir.</span><span class="sxs-lookup"><span data-stu-id="86296-166">An exported `authService` instance of class `AuthorizeService` that handles the lower level details of the authentication process and exposes information about the authenticated user to the rest of the application for consumption.</span></span>

<span data-ttu-id="86296-167">Çözüm ana bileşenleri gördük, size belirli bir uygulama için tek tek senaryoları göz atabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="86296-167">Now that we've seen the main components of the solution, we can take a specific look at individual scenarios for the application:</span></span>

## <a name="requiring-authorization-on-a-new-api"></a><span data-ttu-id="86296-168">Yeni bir API üzerinde gerektiren yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="86296-168">Requiring authorization on a new API</span></span>
<span data-ttu-id="86296-169">Sistem, yetkilendirme için yeni API'leri zorunlu tutmak için basit hale getirmek için hazır yapılandırıldı.</span><span class="sxs-lookup"><span data-stu-id="86296-169">The system is configured out of the box to make it trivial to require authorization for new APIs.</span></span> <span data-ttu-id="86296-170">Bunu yapmak için yalnızca yeni bir denetleyici oluşturma ve ekleme `[Authorize]` denetleyicideki tüm eylem veya denetleyici sınıfı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="86296-170">In order to do so, simply create a new controller and add the `[Authorize]` attribute to the controller class or to any action within the controller.</span></span>

## <a name="protecting-a-client-side-route-angular"></a><span data-ttu-id="86296-171">Bir istemci-tarafı yol (Angular) koruma</span><span class="sxs-lookup"><span data-stu-id="86296-171">Protecting a client-side route (Angular)</span></span>
<span data-ttu-id="86296-172">İstemci tarafı rota koruma authorize guard bir rota yapılandırırken çalıştırılacak cf listesine ekleyerek gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="86296-172">Protecting a client side route is done by adding the authorize guard to the list of guards to run when configuring a route.</span></span> <span data-ttu-id="86296-173">Örneğin, verileri getirme rota ana uygulama angular modülü içinde nasıl yapılandırıldığını görebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="86296-173">As an example you can see how the fetch-data route is configured within the main app angular module:</span></span>

```ts
RouterModule.forRoot([
  // ...
  { path: 'fetch-data', component: FetchDataComponent, canActivate: [AuthorizeGuard] },
])
```

<span data-ttu-id="86296-174">Bir rota koruma gerçek bir uç nokta koruma sağlamaz bahsetmek önemlidir (yine de gerektiren bir `[Authorize]` özniteliğinin) ancak bu, kimliği doğrulanmamış olduğunda belirli istemci tarafı yol gezinme yalnızca kullanıcının engeller.</span><span class="sxs-lookup"><span data-stu-id="86296-174">It is important to mention that protecting a route doesn't protect the actual endpoint (which still requires an `[Authorize]` attribute applied to it) but that it only prevents the user from navigating to the given client side route when it is not authenticated.</span></span>

## <a name="authenticate-api-requests-angular"></a><span data-ttu-id="86296-175">API isteklerinin (Angular) kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="86296-175">Authenticate API requests (Angular)</span></span>

<span data-ttu-id="86296-176">Kimlik doğrulaması tarafında barındırılan API'ler isteklerini uygulama otomatik olarak uygulama tarafından tanımlanmış HTTP istemci dinleyiciyi kullanılarak gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="86296-176">Authenticating requests to APIs hosted along side the application is done automatically through the use of the HTTP client interceptor defined by the application.</span></span>

## <a name="protect-a-client-side-route-react"></a><span data-ttu-id="86296-177">Bir istemci-tarafı yol (React) koruma</span><span class="sxs-lookup"><span data-stu-id="86296-177">Protect a client-side route (React)</span></span>

<span data-ttu-id="86296-178">İstemci tarafı rota korumak yerine düz rota bileşen AuthorizeRoute bileşeni kullanılarak yapılır.</span><span class="sxs-lookup"><span data-stu-id="86296-178">Protecting a client side route is done by using the AuthorizeRoute component instead of the plain Route component.</span></span> <span data-ttu-id="86296-179">Örneğin, verileri getirme rota uygulama bileşeni içinde nasıl yapılandırıldığını görebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="86296-179">As an example you can see how the fetch-data route is configured within the App component:</span></span>

```jsx
<AuthorizeRoute path='/fetch-data' component={FetchData} />
```

<span data-ttu-id="86296-180">Bir rota koruma gerçek bir uç nokta koruma sağlamaz bahsetmek önemlidir (yine de gerektiren bir `[Authorize]` özniteliğinin) ancak bu, kimliği doğrulanmamış olduğunda belirli istemci tarafı yol gezinme yalnızca kullanıcının engeller.</span><span class="sxs-lookup"><span data-stu-id="86296-180">It is important to mention that protecting a route doesn't protect the actual endpoint (which still requires an `[Authorize]` attribute applied to it) but that it only prevents the user from navigating to the given client side route when it is not authenticated.</span></span>

## <a name="authenticate-api-requests-react"></a><span data-ttu-id="86296-181">API isteklerinin (React) kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="86296-181">Authenticate API requests (React)</span></span>

<span data-ttu-id="86296-182">React ile isteklerine kimlik doğrulaması yapılır ilk içeri aktararak `authService` gelen örnek `AuthorizeService` authService erişim belirteci alma ve ardından aşağıda gösterildiği gibi isteği ekleniyor.</span><span class="sxs-lookup"><span data-stu-id="86296-182">Authenticating requests with react is done by first importing the `authService` instance from the `AuthorizeService` and then retrieving the access token from the authService and attaching it to the request as shown below.</span></span> <span data-ttu-id="86296-183">React bileşenlerde bu genellikle componentDidMount yaşam döngüsü yöntemi veya sonucunda bazı kullanıcı etkileşimi sonucunda gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="86296-183">In react components this is typically done in the componentDidMount lifecycle method or as the result from some user interaction.</span></span>

### <a name="import-the-authservice-into-your-component"></a><span data-ttu-id="86296-184">Bileşeniniz authService içe</span><span class="sxs-lookup"><span data-stu-id="86296-184">Import the authService into your component</span></span>

```js
import authService from './api-authorization/AuthorizeService'
```

### <a name="retrieve-and-attach-the-access-token-to-the-response"></a><span data-ttu-id="86296-185">Alın ve yanıtı erişim belirteci ekleme</span><span class="sxs-lookup"><span data-stu-id="86296-185">Retrieve and attach the access token to the response</span></span>

```js
async populateWeatherData() {
  const token = await authService.getAccessToken();
  const response = await fetch('api/SampleData/WeatherForecasts', {
    headers: !token ? {} : { 'Authorization': `Bearer ${token}` }
  });
  const data = await response.json();
  this.setState({ forecasts: data, loading: false });
}
```

## <a name="deploy-into-production"></a><span data-ttu-id="86296-186">Üretime dağıtma</span><span class="sxs-lookup"><span data-stu-id="86296-186">Deploy into production</span></span>

<span data-ttu-id="86296-187">Çeşitli kaynakları sağlamak için ihtiyacımız olan üretim uygulamasına dağıtmak için:</span><span class="sxs-lookup"><span data-stu-id="86296-187">In order to deploy the application into production we need to provision several resources:</span></span>
* <span data-ttu-id="86296-188">Kimlik kullanıcı hesaplarını ve kimlik sunucusu depolamak için bir veritabanı verir.</span><span class="sxs-lookup"><span data-stu-id="86296-188">A database to store the Identity user accounts and the identity server grants.</span></span>
* <span data-ttu-id="86296-189">Belirteçleri imzalamak için kullanmak üzere bir üretim sertifika.</span><span class="sxs-lookup"><span data-stu-id="86296-189">A production certificate to use for signing tokens.</span></span>
  * <span data-ttu-id="86296-190">Bu sertifika için belirli gereksinimler yoktur; otomatik olarak imzalanan bir sertifika veya bir CA yetkilisi sağlanan bir sertifika olabilir.</span><span class="sxs-lookup"><span data-stu-id="86296-190">There are no specific requirements for this certificate; it can be a self-signed certificate or a certificate provisioned through a CA authority.</span></span>
  * <span data-ttu-id="86296-191">Powershell veya openssl gibi standart araçlar aracılığıyla oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="86296-191">It can be generated through standard tools like powershell or openssl.</span></span>
  * <span data-ttu-id="86296-192">Hedef makinelerde sertifika deposuna yüklü veya güçlü bir parola ile bir pfx dosyası olarak dağıtılabilir.</span><span class="sxs-lookup"><span data-stu-id="86296-192">It can be installed into the certificate store on the target machines or deployed as a pfx file with a strong password.</span></span>

### <a name="example-deploy-into-azure-websites"></a><span data-ttu-id="86296-193">Örnek: Azure Web dağıtma</span><span class="sxs-lookup"><span data-stu-id="86296-193">Example: Deploy into Azure Websites</span></span>

<span data-ttu-id="86296-194">Bu bölümde sertifika depolama alanında depolanan bir sertifika kullanarak Azure Web siteleri için uygulamayı dağıtmak için kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="86296-194">In this section we are going to deploy the application to Azure websites using a certificate stored in the certificate store.</span></span> <span data-ttu-id="86296-195">Biz sertifika deposundan bir certicate yüklemek için uygulamayı değiştirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="86296-195">We need to modify the application to load a certicate from the certificate store.</span></span> <span data-ttu-id="86296-196">Bunu yapmak için app service planı biz daha sonraki bir adımda yapılandırdığınızda standart katmanda en az olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="86296-196">To do so, our app service plan needs to be at least on the standard tier when we configure in a later step.</span></span> <span data-ttu-id="86296-197">Uygulamamızı içinde biz yalnızca anahtar ayrıntılarını içerecek şekilde appsettings.json IdentityServer bölümüne değiştirmeniz gerekir:</span><span class="sxs-lookup"><span data-stu-id="86296-197">In our application we simply need to modify the IdentityServer section on appsettings.json to include the key details:</span></span>
```json
  "IdentityServer": {
    "Key": {
      "Type": "Store",
      "StoreName": "My",
      "StoreLocation": "CurrentUser",
      "Name": "CN=MyApplication"
    }
  }
}
```
* <span data-ttu-id="86296-198">Sertifika adı özelliği ayırt edici konu sertifikasının karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="86296-198">The name property on certificate corresponds with the distinguished subject for the certificate.</span></span>
* <span data-ttu-id="86296-199">Depolama konumu (CurrentUrser veya LocalMachine) sertifika yükleneceği temsil eder.</span><span class="sxs-lookup"><span data-stu-id="86296-199">The store location represents where to load the certificate from (CurrentUrser or LocalMachine).</span></span>
* <span data-ttu-id="86296-200">Depo adını, sertifika, bu durumda kullanıcı Kişisel depoya işaret depolandığı sertifika deposunun adını temsil eder.</span><span class="sxs-lookup"><span data-stu-id="86296-200">The store name represents the name of the certificate store where the certificate is stored, in this case it points to the personal user store.</span></span>

<span data-ttu-id="86296-201">Yer alan adımları uygulayarak bir uygulamayı Azure Web siteleri'ne dağıtma [uygulamasını Azure'a dağıtma](xref:tutorials/publish-to-azure-webapp-using-vs#deploy-the-app-to-azure) gerekli Azure kaynakları oluşturmak ve uygulamayı üretime dağıtın.</span><span class="sxs-lookup"><span data-stu-id="86296-201">To deploy to Azure Websites, deploy the app following the steps in [Deploy the app to Azure](xref:tutorials/publish-to-azure-webapp-using-vs#deploy-the-app-to-azure) to create the necessary Azure resources and deploy the app to production.</span></span>

<span data-ttu-id="86296-202">Bunu yaptıktan sonra uygulamayı Azure'a dağıtılır, ancak yine de uygulama tarafından kullanılacak bir sertifika ayarlamak üzere ihtiyacımız henüz tamamen işlevsel değildir.</span><span class="sxs-lookup"><span data-stu-id="86296-202">After doing this, the application is deployed into Azure but is not yet completely functional as we still need to setup the certificate to be used by the application.</span></span> <span data-ttu-id="86296-203">Bunu yapmak için açıklanan adımları izleyin ve kullanmak için olduğumuz sertifikanın parmak izi sağlamak ihtiyacımız [sertifikalarınızı yüklemek](/azure/app-service/app-service-web-ssl-cert-load#load-your-certificates).</span><span class="sxs-lookup"><span data-stu-id="86296-203">To do so, we need to have the thumbprint for the certificate we are going to use and follow the steps described in [Load your certificates](/azure/app-service/app-service-web-ssl-cert-load#load-your-certificates).</span></span>

<span data-ttu-id="86296-204">Bu adımları SSL bahsetmek yoktur "Özel Sertifikalar" bölümünde portalında nerede biz uygulamamız ile kullanmak için sağlanan sertifika karşıya yükleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="86296-204">While these steps mention SSL, there is a "Private certificates" section on the portal where we can upload our provisioned certificate to use with our app.</span></span>

<span data-ttu-id="86296-205">Bu adımdan sonra biz uygulamamız yeniden başlatabiliyor olmanız ve tamamen işlevsel olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="86296-205">After this step, we should be able to restart our application and it should be completely functional.</span></span>

## <a name="other-configuration-options"></a><span data-ttu-id="86296-206">Diğer yapılandırma seçenekleri</span><span class="sxs-lookup"><span data-stu-id="86296-206">Other configuration options</span></span>
<span data-ttu-id="86296-207">Desteğimiz için API yetkilendirme kuralları, varsayılan değerleri ve tek sayfa uygulamaları deneyimini kolaylaştırmak için geliştirmeler kümesiyle kimlik sunucusu üzerinde oluşturur.</span><span class="sxs-lookup"><span data-stu-id="86296-207">Our support for API authorization builds on top of Identity Server with a set of conventions, default values and enhancements to simplify the experience for Single Page Applications.</span></span> <span data-ttu-id="86296-208">Needless çok deyin sunuyoruz tümleştirmeler senaryonuz erişmezse kimlik Server'ın arka planda kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="86296-208">Needless to say, the full power of Identity Server is available behind the scenes if the integrations that we offer don't cover your scenario.</span></span> <span data-ttu-id="86296-209">Destek kuruluşumuz tarafından dağıtılan tüm uygulamalar burada oluşturulan ve "Birinci taraf" uygulamaları dediğimiz üzerinde odaklanır.</span><span class="sxs-lookup"><span data-stu-id="86296-209">Our support is focused on what we call "first-party" applications, where all the applications are created and deployed by our organization.</span></span> <span data-ttu-id="86296-210">Bu nedenle onay ve Federasyon gibi şeyler için destek sunuyoruz yok.</span><span class="sxs-lookup"><span data-stu-id="86296-210">As such we don't offer support for things like consent or federation.</span></span> <span data-ttu-id="86296-211">Bu senaryolar için Bizim önerimiz kimlik sunucusunu kendi belgelerini izleyin kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="86296-211">For those scenarios our recommendation is to use Identity Server and follow their documentation.</span></span>

### <a name="application-profiles"></a><span data-ttu-id="86296-212">Uygulama profilleri</span><span class="sxs-lookup"><span data-stu-id="86296-212">Application profiles</span></span>
<span data-ttu-id="86296-213">Uygulama profilleri, kendi parametrelerini daha tanımlamanız uygulamalar için önceden tanımlanmış yapılandırmalardır.</span><span class="sxs-lookup"><span data-stu-id="86296-213">Application profiles are predefined configurations for applications that further define their parameters.</span></span> <span data-ttu-id="86296-214">Şu anda iki profili destekliyoruz:</span><span class="sxs-lookup"><span data-stu-id="86296-214">At this time we support two profiles:</span></span>
* <span data-ttu-id="86296-215">IdentityServerSPA: Kimlik sunucusu tek bir birim olarak birlikte barındırılan bir tek sayfalı uygulama temsil eder.</span><span class="sxs-lookup"><span data-stu-id="86296-215">IdentityServerSPA: Represents a single page application hosted alongside Identity Server as a single unit.</span></span>
  * <span data-ttu-id="86296-216">Varsayılan olarak redirect_uri `/authentication/login-callback`.</span><span class="sxs-lookup"><span data-stu-id="86296-216">The redirect_uri defaults to `/authentication/login-callback`.</span></span>
  * <span data-ttu-id="86296-217">Varsayılan olarak post_logout_redirect_uri `/authentication/logout-callback`.</span><span class="sxs-lookup"><span data-stu-id="86296-217">The post_logout_redirect_uri defaults to `/authentication/logout-callback`.</span></span>
  * <span data-ttu-id="86296-218">Kapsamları kümesini içeren `openid`, `profile`ve uygulama API'leri için tanımlanan her kapsam.</span><span class="sxs-lookup"><span data-stu-id="86296-218">The set of scopes includes the `openid`, `profile`, and every scope defined for the APIs in the application.</span></span>
  * <span data-ttu-id="86296-219">İzin verilen OIDC yanıt türleri kümesi `id_token token` veya bunların her birini tek tek (`id_token`, `token`).</span><span class="sxs-lookup"><span data-stu-id="86296-219">The set of allowed OIDC response types is `id_token token` or each of them individually (`id_token`, `token`).</span></span>
  * <span data-ttu-id="86296-220">İzin verilen yanıt modu `fragment`.</span><span class="sxs-lookup"><span data-stu-id="86296-220">The allowed response mode is `fragment`.</span></span>
* <span data-ttu-id="86296-221">SPA: Kimlik sunucusu ile barındırılmayan bir tek sayfalı uygulama temsil eder.</span><span class="sxs-lookup"><span data-stu-id="86296-221">SPA: Represents a single page application that is not hosted with Identity Server.</span></span>
  * <span data-ttu-id="86296-222">Kapsamları kümesini içeren `openid`, `profile`ve uygulama API'leri için tanımlanan her kapsam.</span><span class="sxs-lookup"><span data-stu-id="86296-222">The set of scopes includes the `openid`, `profile`, and every scope defined for the APIs in the application.</span></span>
  * <span data-ttu-id="86296-223">İzin verilen OIDC yanıt türleri kümesi `id_token token` veya bunların her birini tek tek (`id_token`, `token`).</span><span class="sxs-lookup"><span data-stu-id="86296-223">The set of allowed OIDC response types is `id_token token` or each of them individually (`id_token`, `token`).</span></span>
  * <span data-ttu-id="86296-224">İzin verilen yanıt modu `fragment`.</span><span class="sxs-lookup"><span data-stu-id="86296-224">The allowed response mode is `fragment`.</span></span>
* <span data-ttu-id="86296-225">IdentityServerJwt: Yanı sıra barındırılan bir API ile kimlik sunucusunu temsil eder.</span><span class="sxs-lookup"><span data-stu-id="86296-225">IdentityServerJwt: Represents an API that is hosted alongside with Identity Server.</span></span>
  * <span data-ttu-id="86296-226">Uygulama, uygulama adı için varsayılan olarak tek bir kapsam sahip olacak şekilde yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="86296-226">The application is configured to have a single scope that defaults to the application name.</span></span>
* <span data-ttu-id="86296-227">API: Değil barındırılan bir API ile kimlik sunucusunu temsil eder.</span><span class="sxs-lookup"><span data-stu-id="86296-227">API: Represents an API that is not hosted with Identity Server.</span></span>
  * <span data-ttu-id="86296-228">Uygulama, uygulama adı için varsayılan olarak tek bir kapsam sahip olacak şekilde yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="86296-228">The application is configured to have a single scope that defaults to the application name.</span></span>

### <a name="configuration-through-appsettings"></a><span data-ttu-id="86296-229">AppSettings aracılığıyla yapılandırma</span><span class="sxs-lookup"><span data-stu-id="86296-229">Configuration through AppSettings</span></span>
<span data-ttu-id="86296-230">Uygulamaları yapılandırma sistemimiz istemciler veya kaynaklar listesine sırasıyla ekleyerek için bunları yapılandırabiliriz.</span><span class="sxs-lookup"><span data-stu-id="86296-230">We can configure the applications through our configuration system by adding them to the list of Clients or Resources respectively.</span></span> 

<span data-ttu-id="86296-231">İstemciler yapılandırırken yapılandırabiliriz `redirect_uri` ve `post_logout_redirect_uri` aşağıda gösterildiği gibi:</span><span class="sxs-lookup"><span data-stu-id="86296-231">When configuring clients we can configure the `redirect_uri` and the `post_logout_redirect_uri` as shown below:</span></span>
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

<span data-ttu-id="86296-232">Kaynakları yapılandırırken size kapsamları kaynak için aşağıda gösterildiği gibi yapılandırabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="86296-232">When configuring resources we can configure the scopes for the resource as shown below:</span></span>
```json
"IdentityServer": {
  "Resources": {
    "MyExternalApi": {
      "Profile": "API",
      "Scopes": "a b c",
    }
  }
}
```

### <a name="configuration-through-code"></a><span data-ttu-id="86296-233">Kod ile yapılandırma</span><span class="sxs-lookup"><span data-stu-id="86296-233">Configuration through code</span></span>
<span data-ttu-id="86296-234">Biz, istemciler ve kaynakları bir aşırı yüklemesini seçeneklerini yapılandırmak için bir eylemde AddApiAuthorization kullanarak kod aracılığıyla da yapılandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="86296-234">We can also configure the clients and resources through code using an overload of AddApiAuthorization that takes an action to configure options.</span></span>
```csharp
AddApiAuthorization<ApplicationUser, ApplicationDbContext>(options =>
{
    options.Clients.AddSPA(
        "My SPA",
        spa => spa.WithRedirectUri("http://www.example.com/authentication/login-callback")
            .WithLogoutRedirectUri("http://www.example.com/authentication/logout-callback"));

    options.ApiResources.AddApiResource("MyExternalApi", resource => resource.WithScopes("a", "b", "c"));
});
```
