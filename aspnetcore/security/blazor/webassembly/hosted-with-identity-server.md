---
title: Kimlik sunucusuyla bir ASP.NET Core Blazor Weelsembly barındırılan uygulamasını güvenli hale getirme
author: guardrex
description: Bir [IdentityServer](https://identityserver.io/) arka ucu kullanan Visual Studio içinden kimlik doğrulaması ile yeni Blazor barındırılan bir uygulama oluşturmak için
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/16/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/hosted-with-identity-server
ms.openlocfilehash: a3993bf635e5a7aae408d72796015f2414e13c14
ms.sourcegitcommit: 5bdc54162d7dea8d9fa54ac3055678db23586af1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/17/2020
ms.locfileid: "79434479"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-hosted-app-with-identity-server"></a><span data-ttu-id="a380f-103">Kimlik sunucusuyla bir ASP.NET Core Blazor Weelsembly barındırılan uygulamasını güvenli hale getirme</span><span class="sxs-lookup"><span data-stu-id="a380f-103">Secure an ASP.NET Core Blazor WebAssembly hosted app with Identity Server</span></span>

<span data-ttu-id="a380f-104">, [Javier Calvarro Nelson](https://github.com/javiercn) ve [Luke Latham](https://github.com/guardrex) 'e göre</span><span class="sxs-lookup"><span data-stu-id="a380f-104">By [Javier Calvarro Nelson](https://github.com/javiercn) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

<span data-ttu-id="a380f-105">Visual Studio 'da, kullanıcıların ve API çağrılarının kimliğini doğrulamak için [IdentityServer](https://identityserver.io/) kullanan yeni bir Blazor barındırılan uygulama oluşturmak için:</span><span class="sxs-lookup"><span data-stu-id="a380f-105">To create a new Blazor hosted app in Visual Studio that uses [IdentityServer](https://identityserver.io/) to authenticate users and API calls:</span></span>

1. <span data-ttu-id="a380f-106">Yeni bir **Blazor WebAssembly** uygulaması oluşturmak Için Visual Studio 'yu kullanın.</span><span class="sxs-lookup"><span data-stu-id="a380f-106">Use Visual Studio to create a new **Blazor WebAssembly** app.</span></span> <span data-ttu-id="a380f-107">Daha fazla bilgi için bkz. <xref:blazor/get-started>.</span><span class="sxs-lookup"><span data-stu-id="a380f-107">For more information, see <xref:blazor/get-started>.</span></span>
1. <span data-ttu-id="a380f-108">**Yeni Blazor uygulaması oluştur** Iletişim kutusunda **kimlik doğrulama** bölümünde **Değiştir** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="a380f-108">In the **Create a new Blazor app** dialog, select **Change** in the **Authentication** section.</span></span>
1. <span data-ttu-id="a380f-109">**Her kullanıcı hesabını** ve ardından **Tamam ' ı**seçin.</span><span class="sxs-lookup"><span data-stu-id="a380f-109">Select **Individual User Accounts** followed by **OK**.</span></span>
1. <span data-ttu-id="a380f-110">**Gelişmiş** bölümünde **ASP.NET Core barındırılan** onay kutusunu seçin.</span><span class="sxs-lookup"><span data-stu-id="a380f-110">Select the **ASP.NET Core hosted** checkbox in the **Advanced** section.</span></span>
1. <span data-ttu-id="a380f-111">**Oluştur** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="a380f-111">Select the **Create** button.</span></span>

<span data-ttu-id="a380f-112">Uygulamayı bir komut kabuğunda oluşturmak için aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="a380f-112">To create the app in a command shell, execute the following command:</span></span>

```dotnetcli
dotnet new blazorwasm -au Individual -ho
```

<span data-ttu-id="a380f-113">Mevcut değilse bir proje klasörü oluşturan çıkış konumunu belirtmek için, çıkış seçeneğini komuta bir yol (örneğin, `-o BlazorSample`) ile birlikte ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a380f-113">To specify the output location, which creates a project folder if it doesn't exist, include the output option in the command with a path (for example, `-o BlazorSample`).</span></span> <span data-ttu-id="a380f-114">Klasör adı Ayrıca projenin adının bir parçası haline gelir.</span><span class="sxs-lookup"><span data-stu-id="a380f-114">The folder name also becomes part of the project's name.</span></span>

## <a name="server-app-configuration"></a><span data-ttu-id="a380f-115">Sunucu uygulaması yapılandırması</span><span class="sxs-lookup"><span data-stu-id="a380f-115">Server app configuration</span></span>

<span data-ttu-id="a380f-116">Aşağıdaki bölümlerde, kimlik doğrulama desteği dahil edildiğinde projenin eklemeleri açıklanır.</span><span class="sxs-lookup"><span data-stu-id="a380f-116">The following sections describe additions to the project when authentication support is included.</span></span>

### <a name="startup-class"></a><span data-ttu-id="a380f-117">Başlangıç sınıfı</span><span class="sxs-lookup"><span data-stu-id="a380f-117">Startup class</span></span>

<span data-ttu-id="a380f-118">`Startup` sınıfı aşağıdaki eklemelere sahiptir:</span><span class="sxs-lookup"><span data-stu-id="a380f-118">The `Startup` class has the following additions:</span></span>

* <span data-ttu-id="a380f-119">`Startup.ConfigureServices` içinde:</span><span class="sxs-lookup"><span data-stu-id="a380f-119">In `Startup.ConfigureServices`:</span></span>

  * <span data-ttu-id="a380f-120">Varsayılan Kullanıcı arabirimine sahip kimlik:</span><span class="sxs-lookup"><span data-stu-id="a380f-120">Identity with the default UI:</span></span>

    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlite(Configuration.GetConnectionString("DefaultConnection")));

    services.AddDefaultIdentity<ApplicationUser>()
        .AddDefaultUI(UIFramework.Bootstrap4)
        .AddEntityFrameworkStores<ApplicationDbContext>();
    ```

  * <span data-ttu-id="a380f-121">IdentityServer 'ın en üstünde bazı varsayılan ASP.NET Core kuralları ayarlayan ek bir <xref:Microsoft.Extensions.DependencyInjection.IdentityServerBuilderConfigurationExtensions.AddApiAuthorization%2A> yardımcı yöntemi olan IdentityServer:</span><span class="sxs-lookup"><span data-stu-id="a380f-121">IdentityServer with an additional <xref:Microsoft.Extensions.DependencyInjection.IdentityServerBuilderConfigurationExtensions.AddApiAuthorization%2A> helper method that sets up some default ASP.NET Core conventions on top of IdentityServer:</span></span>

    ```csharp
    services.AddIdentityServer()
        .AddApiAuthorization<ApplicationUser, ApplicationDbContext>();
    ```

  * <span data-ttu-id="a380f-122">Kimliği, IdentityServer tarafından üretilen JWT belirteçlerini doğrulamak üzere uygulamayı yapılandıran ek bir <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A> Yardımcısı yöntemiyle kimlik doğrulaması:</span><span class="sxs-lookup"><span data-stu-id="a380f-122">Authentication with an additional <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A> helper method that configures the app to validate JWT tokens produced by IdentityServer:</span></span>

    ```csharp
    services.AddAuthentication()
        .AddIdentityServerJwt();
    ```

* <span data-ttu-id="a380f-123">`Startup.Configure` içinde:</span><span class="sxs-lookup"><span data-stu-id="a380f-123">In `Startup.Configure`:</span></span>

  * <span data-ttu-id="a380f-124">İstek kimlik bilgilerini doğrulamadan ve Kullanıcı istek bağlamında ayarlamaktan sorumlu kimlik doğrulama ara yazılımı:</span><span class="sxs-lookup"><span data-stu-id="a380f-124">The authentication middleware that is responsible for validating the request credentials and setting the user on the request context:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

  * <span data-ttu-id="a380f-125">Açık KIMLIK Connect (OıDC) uç noktalarını kullanıma sunan IdentityServer ara yazılımı:</span><span class="sxs-lookup"><span data-stu-id="a380f-125">The IdentityServer middleware that exposes the Open ID Connect (OIDC) endpoints:</span></span>

    ```csharp
    app.UseIdentityServer();
    ```

### <a name="addapiauthorization"></a><span data-ttu-id="a380f-126">Addadpiauthorization</span><span class="sxs-lookup"><span data-stu-id="a380f-126">AddApiAuthorization</span></span>

<span data-ttu-id="a380f-127"><xref:Microsoft.Extensions.DependencyInjection.IdentityServerBuilderConfigurationExtensions.AddApiAuthorization%2A> yardımcı yöntemi, [IdentityServer](https://identityserver.io/) 'ı ASP.NET Core senaryolar için yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="a380f-127">The <xref:Microsoft.Extensions.DependencyInjection.IdentityServerBuilderConfigurationExtensions.AddApiAuthorization%2A> helper method configures [IdentityServer](https://identityserver.io/) for ASP.NET Core scenarios.</span></span> <span data-ttu-id="a380f-128">IdentityServer, uygulama güvenliği sorunlarını işlemeye yönelik güçlü ve genişletilebilir bir çerçevedir.</span><span class="sxs-lookup"><span data-stu-id="a380f-128">IdentityServer is a powerful and extensible framework for handling app security concerns.</span></span> <span data-ttu-id="a380f-129">IdentityServer, en yaygın senaryolar için gereksiz karmaşıklık sunar.</span><span class="sxs-lookup"><span data-stu-id="a380f-129">IdentityServer exposes unnecessary complexity for the most common scenarios.</span></span> <span data-ttu-id="a380f-130">Sonuç olarak, iyi bir başlangıç noktası düşüntiğimiz bir dizi kural ve yapılandırma seçeneği sağlanır.</span><span class="sxs-lookup"><span data-stu-id="a380f-130">Consequently, a set of conventions and configuration options is provided that we consider a good starting point.</span></span> <span data-ttu-id="a380f-131">Kimlik doğrulama gereksinimleriniz değiştikçe, IdentityServer 'ın tam gücü, kimlik doğrulamasını uygulamanın gereksinimlerine uyacak şekilde özelleştirmek için hala kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="a380f-131">Once your authentication needs change, the full power of IdentityServer is still available to customize authentication to suit an app's requirements.</span></span>

### <a name="addidentityserverjwt"></a><span data-ttu-id="a380f-132">Addentityserverjwt</span><span class="sxs-lookup"><span data-stu-id="a380f-132">AddIdentityServerJwt</span></span>

<span data-ttu-id="a380f-133"><xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A> yardımcı yöntemi, varsayılan kimlik doğrulama işleyicisi olarak uygulama için bir ilke düzeni yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="a380f-133">The <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A> helper method configures a policy scheme for the app as the default authentication handler.</span></span> <span data-ttu-id="a380f-134">İlke, kimliğin kimlik URL 'SI alanında `/Identity`herhangi bir alt yolda yönlendirilen tüm istekleri işlemesini sağlamak üzere yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="a380f-134">The policy is configured to allow Identity to handle all requests routed to any subpath in the Identity URL space `/Identity`.</span></span> <span data-ttu-id="a380f-135"><xref:Microsoft.AspNetCore.Authentication.JwtBearer.JwtBearerHandler> diğer tüm istekleri işler.</span><span class="sxs-lookup"><span data-stu-id="a380f-135">The <xref:Microsoft.AspNetCore.Authentication.JwtBearer.JwtBearerHandler> handles all other requests.</span></span> <span data-ttu-id="a380f-136">Ayrıca, bu yöntem:</span><span class="sxs-lookup"><span data-stu-id="a380f-136">Additionally, this method:</span></span>

* <span data-ttu-id="a380f-137">Bir `{APPLICATION NAME}API` API kaynağını, IdentityServer ile `{APPLICATION NAME}API`varsayılan kapsamına kaydeder.</span><span class="sxs-lookup"><span data-stu-id="a380f-137">Registers an `{APPLICATION NAME}API` API resource with IdentityServer with a default scope of `{APPLICATION NAME}API`.</span></span>
* <span data-ttu-id="a380f-138">Uygulama için IdentityServer tarafından verilen belirteçleri doğrulamak üzere JWT taşıyıcı belirteç ara yazılımını yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="a380f-138">Configures the JWT Bearer Token Middleware to validate tokens issued by IdentityServer for the app.</span></span>

### <a name="weatherforecastcontroller"></a><span data-ttu-id="a380f-139">Dalgalı bir denetleyici</span><span class="sxs-lookup"><span data-stu-id="a380f-139">WeatherForecastController</span></span>

<span data-ttu-id="a380f-140">`WeatherForecastController` (*denetleyiciler/dalgalı değer denetleyicisi. cs*) içinde, [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) özniteliği sınıfa uygulanır.</span><span class="sxs-lookup"><span data-stu-id="a380f-140">In the `WeatherForecastController` (*Controllers/WeatherForecastController.cs*), the [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) attribute is applied to the class.</span></span> <span data-ttu-id="a380f-141">Özniteliği, kullanıcıya kaynağa erişim için varsayılan ilkeye göre yetkilendirilmiş olması gerektiğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="a380f-141">The attribute indicates that the user must be authorized based on the default policy to access the resource.</span></span> <span data-ttu-id="a380f-142">Varsayılan yetkilendirme ilkesi, daha önce bahsedilen ilke şemasına <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A> tarafından ayarlanan varsayılan kimlik doğrulama düzenini kullanacak şekilde yapılandırılmıştır.</span><span class="sxs-lookup"><span data-stu-id="a380f-142">The default authorization policy is configured to use the default authentication scheme, which is set up by <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A> to the policy scheme that was mentioned earlier.</span></span> <span data-ttu-id="a380f-143">Yardımcı yöntemi, uygulama istekleri için varsayılan işleyici olarak <xref:Microsoft.AspNetCore.Authentication.JwtBearer.JwtBearerHandler> yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="a380f-143">The helper method configures <xref:Microsoft.AspNetCore.Authentication.JwtBearer.JwtBearerHandler> as the default handler for requests to the app.</span></span>

### <a name="applicationdbcontext"></a><span data-ttu-id="a380f-144">ApplicationDbContext</span><span class="sxs-lookup"><span data-stu-id="a380f-144">ApplicationDbContext</span></span>

<span data-ttu-id="a380f-145">`ApplicationDbContext` (*Data/ApplicationDbContext. cs*) öğesinde aynı <xref:Microsoft.EntityFrameworkCore.DbContext> kimlik Içinde, IdentityServer şemasını dahil etmek için <xref:Microsoft.AspNetCore.ApiAuthorization.IdentityServer.ApiAuthorizationDbContext%601> genişlettiği özel durum ile birlikte kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a380f-145">In the `ApplicationDbContext` (*Data/ApplicationDbContext.cs*), the same <xref:Microsoft.EntityFrameworkCore.DbContext> is used in Identity with the exception that it extends <xref:Microsoft.AspNetCore.ApiAuthorization.IdentityServer.ApiAuthorizationDbContext%601> to include the schema for IdentityServer.</span></span> <span data-ttu-id="a380f-146"><xref:Microsoft.AspNetCore.ApiAuthorization.IdentityServer.ApiAuthorizationDbContext%601> <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext>türetilir.</span><span class="sxs-lookup"><span data-stu-id="a380f-146"><xref:Microsoft.AspNetCore.ApiAuthorization.IdentityServer.ApiAuthorizationDbContext%601> is derived from <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext>.</span></span>

<span data-ttu-id="a380f-147">Veritabanı şeması üzerinde tam denetim elde etmek için, kullanılabilir kimlik <xref:Microsoft.EntityFrameworkCore.DbContext> sınıflarından birinden devralma ve bağlamı `OnModelCreating` yönteminde `builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)` çağırarak kimlik şemasını içerecek şekilde yapılandırma.</span><span class="sxs-lookup"><span data-stu-id="a380f-147">To gain full control of the database schema, inherit from one of the available Identity <xref:Microsoft.EntityFrameworkCore.DbContext> classes and configure the context to include the Identity schema by calling `builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)` in the `OnModelCreating` method.</span></span>

### <a name="oidcconfigurationcontroller"></a><span data-ttu-id="a380f-148">Oıdcconfigurationcontroller</span><span class="sxs-lookup"><span data-stu-id="a380f-148">OidcConfigurationController</span></span>

<span data-ttu-id="a380f-149">`OidcConfigurationController` (*Controllers/Oıdcconfigurationcontroller. cs*), istemci uç noktası OIDC parametrelerine hizmeti sunacak şekilde sağlanır.</span><span class="sxs-lookup"><span data-stu-id="a380f-149">In the `OidcConfigurationController` (*Controllers/OidcConfigurationController.cs*), the client endpoint is provisioned to serve OIDC parameters.</span></span>

### <a name="app-settings-files"></a><span data-ttu-id="a380f-150">Uygulama ayarları dosyaları</span><span class="sxs-lookup"><span data-stu-id="a380f-150">App settings files</span></span>

<span data-ttu-id="a380f-151">Proje kökündeki App settings dosyasında (*appSettings. JSON*), `IdentityServer` bölümünde yapılandırılan istemcilerin listesi açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="a380f-151">In the app settings file (*appsettings.json*) at the project root, the `IdentityServer` section describes the list of configured clients.</span></span> <span data-ttu-id="a380f-152">Aşağıdaki örnekte, tek bir istemci vardır.</span><span class="sxs-lookup"><span data-stu-id="a380f-152">In the following example, there's a single client.</span></span> <span data-ttu-id="a380f-153">İstemci adı, uygulama adına karşılık gelir ve bir kural tarafından OAuth `ClientId` parametresine eşlenir.</span><span class="sxs-lookup"><span data-stu-id="a380f-153">The client name corresponds to the app name and is mapped by convention to the OAuth `ClientId` parameter.</span></span> <span data-ttu-id="a380f-154">Profil, yapılandırılan uygulama türünü gösterir.</span><span class="sxs-lookup"><span data-stu-id="a380f-154">The profile indicates the app type being configured.</span></span> <span data-ttu-id="a380f-155">Profil, sunucu için yapılandırma işlemini basitleştiren kuralları yönlendirmek için dahili olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a380f-155">The profile is used internally to drive conventions that simplify the configuration process for the server.</span></span> <!-- There are several profiles available, as explained in the [Application profiles](#application-profiles) section. -->

```json
"IdentityServer": {
  "Clients": {
    "BlazorApplicationWithAuthentication.Client": {
      "Profile": "IdentityServerSPA"
    }
  }
}
```

<span data-ttu-id="a380f-156">Geliştirme ortamı uygulama ayarları dosyasında (*appSettings. Development. JSON*) proje kökünde, `IdentityServer` bölümünde belirteçleri imzalamak için kullanılan anahtar açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="a380f-156">In the Development environment app settings file (*appsettings.Development.json*) at the project root, the `IdentityServer` section describes the key used to sign tokens.</span></span> <!-- When deploying to production, a key needs to be provisioned and deployed alongside the app, as explained in the [Deploy to production](#deploy-to-production) section. -->

```json
"IdentityServer": {
  "Key": {
    "Type": "Development"
  }
}
```

## <a name="client-app-configuration"></a><span data-ttu-id="a380f-157">İstemci uygulama yapılandırması</span><span class="sxs-lookup"><span data-stu-id="a380f-157">Client app configuration</span></span>

### <a name="authentication-package"></a><span data-ttu-id="a380f-158">Kimlik doğrulama paketi</span><span class="sxs-lookup"><span data-stu-id="a380f-158">Authentication package</span></span>

<span data-ttu-id="a380f-159">Bireysel kullanıcı hesapları (`Individual`) kullanmak üzere bir uygulama oluşturulduğunda, uygulama otomatik olarak uygulamanın proje dosyasındaki `Microsoft.AspNetCore.Components.WebAssembly.Authentication` paketi için bir paket başvurusu alır.</span><span class="sxs-lookup"><span data-stu-id="a380f-159">When an app is created to use Individual User Accounts (`Individual`), the app automatically receives a package reference for the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package in the app's project file.</span></span> <span data-ttu-id="a380f-160">Paket, uygulamanın kullanıcıların kimliğini doğrulamasına ve korunan API 'Leri çağırmak için belirteçleri almasına yardımcı olan bir dizi temel sunar.</span><span class="sxs-lookup"><span data-stu-id="a380f-160">The package provides a set of primitives that help the app authenticate users and obtain tokens to call protected APIs.</span></span>

<span data-ttu-id="a380f-161">Bir uygulamaya kimlik doğrulaması ekliyorsanız, paketi uygulamanın proje dosyasına el ile ekleyin:</span><span class="sxs-lookup"><span data-stu-id="a380f-161">If adding authentication to an app, manually add the package to the app's project file:</span></span>

```xml
<PackageReference 
    Include="Microsoft.AspNetCore.Components.WebAssembly.Authentication" 
    Version="{VERSION}" />
```

<span data-ttu-id="a380f-162">Önceki paket başvurusunda `{VERSION}`, <xref:blazor/get-started> makalesinde gösterilen `Microsoft.AspNetCore.Blazor.Templates` paketinin sürümü ile değiştirin.</span><span class="sxs-lookup"><span data-stu-id="a380f-162">Replace `{VERSION}` in the preceding package reference with the version of the `Microsoft.AspNetCore.Blazor.Templates` package shown in the <xref:blazor/get-started> article.</span></span>

### <a name="api-authorization-support"></a><span data-ttu-id="a380f-163">API yetkilendirme desteği</span><span class="sxs-lookup"><span data-stu-id="a380f-163">API authorization support</span></span>

<span data-ttu-id="a380f-164">Kullanıcıları kimlik doğrulama desteği, `Microsoft.AspNetCore.Components.WebAssembly.Authentication` paketi içinde sunulan genişletme yöntemi tarafından hizmet kapsayıcısına takılır.</span><span class="sxs-lookup"><span data-stu-id="a380f-164">The support for authenticating users is plugged into the service container by the extension method provided inside the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package.</span></span> <span data-ttu-id="a380f-165">Bu yöntem, uygulamanın var olan yetkilendirme sistemiyle etkileşime geçmesini sağlamak için gereken tüm hizmetleri ayarlar.</span><span class="sxs-lookup"><span data-stu-id="a380f-165">This method sets up all the services needed for the app to interact with the existing authorization system.</span></span>

```csharp
builder.Services.AddApiAuthorization();
```

<span data-ttu-id="a380f-166">Varsayılan olarak, uygulama yapılandırmasını `_configuration/{client-id}`' dan kurala göre yükler.</span><span class="sxs-lookup"><span data-stu-id="a380f-166">By default, it loads the configuration for the app by convention from `_configuration/{client-id}`.</span></span> <span data-ttu-id="a380f-167">Kural gereği, istemci KIMLIĞI uygulamanın derleme adına ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="a380f-167">By convention, the client ID is set to the app's assembly name.</span></span> <span data-ttu-id="a380f-168">Bu URL, aþýrý yükleme seçeneklerini çağırarak ayrı bir uç noktaya işaret etmek üzere değiştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="a380f-168">This URL can be changed to point to a separate endpoint by calling the overload with options.</span></span>

### <a name="index-page"></a><span data-ttu-id="a380f-169">Dizin sayfası</span><span class="sxs-lookup"><span data-stu-id="a380f-169">Index page</span></span>

[!INCLUDE[](~/includes/blazor-security/index-page.md)]

### <a name="app-component"></a><span data-ttu-id="a380f-170">Uygulama bileşeni</span><span class="sxs-lookup"><span data-stu-id="a380f-170">App component</span></span>

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

### <a name="redirecttologin-component"></a><span data-ttu-id="a380f-171">RedirectToLogin bileşeni</span><span class="sxs-lookup"><span data-stu-id="a380f-171">RedirectToLogin component</span></span>

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

### <a name="logindisplay-component"></a><span data-ttu-id="a380f-172">LoginDisplay bileşeni</span><span class="sxs-lookup"><span data-stu-id="a380f-172">LoginDisplay component</span></span>

<span data-ttu-id="a380f-173">`LoginDisplay` bileşeni (*Shared/LoginDisplay.* razor) `MainLayout` bileşeninde (*Shared/mainlayout. Razor*) işlenir ve aşağıdaki davranışları yönetir:</span><span class="sxs-lookup"><span data-stu-id="a380f-173">The `LoginDisplay` component (*Shared/LoginDisplay.razor*) is rendered in the `MainLayout` component (*Shared/MainLayout.razor*) and manages the following behaviors:</span></span>

* <span data-ttu-id="a380f-174">Kimliği doğrulanmış kullanıcılar için:</span><span class="sxs-lookup"><span data-stu-id="a380f-174">For authenticated users:</span></span>
  * <span data-ttu-id="a380f-175">Geçerli Kullanıcı adını görüntüler.</span><span class="sxs-lookup"><span data-stu-id="a380f-175">Displays the current user name.</span></span>
  * <span data-ttu-id="a380f-176">ASP.NET Core kimliği içindeki kullanıcı profili sayfasına bir bağlantı sunar.</span><span class="sxs-lookup"><span data-stu-id="a380f-176">Offers a link to the user profile page in ASP.NET Core Identity.</span></span>
  * <span data-ttu-id="a380f-177">Uygulamanın oturumunu kapatmak için bir düğme sunar.</span><span class="sxs-lookup"><span data-stu-id="a380f-177">Offers a button to log out of the app.</span></span>
* <span data-ttu-id="a380f-178">Anonim kullanıcılar için:</span><span class="sxs-lookup"><span data-stu-id="a380f-178">For anonymous users:</span></span>
  * <span data-ttu-id="a380f-179">Kayıt için seçeneği sunar.</span><span class="sxs-lookup"><span data-stu-id="a380f-179">Offers the option to register.</span></span>
  * <span data-ttu-id="a380f-180">Oturum açma seçeneği sunar.</span><span class="sxs-lookup"><span data-stu-id="a380f-180">Offers the option to log in.</span></span>

```razor
@using Microsoft.AspNetCore.Components.Authorization
@using Microsoft.AspNetCore.Components.WebAssembly.Authentication
@inject NavigationManager Navigation
@inject SignOutSessionStateManager SignOutManager

<AuthorizeView>
    <Authorized>
        <a href="authentication/profile">Hello, @context.User.Identity.Name!</a>
        <button class="nav-link btn btn-link" @onclick="BeginSignOut">
            Log out
        </button>
    </Authorized>
    <NotAuthorized>
        <a href="authentication/register">Register</a>
        <a href="authentication/login">Log in</a>
    </NotAuthorized>
</AuthorizeView>

@code {
    private async Task BeginSignOut(MouseEventArgs args)
    {
        await SignOutManager.SetSignOutState();
        Navigation.NavigateTo("authentication/logout");
    }
}
```

### <a name="authentication-component"></a><span data-ttu-id="a380f-181">Kimlik doğrulama bileşeni</span><span class="sxs-lookup"><span data-stu-id="a380f-181">Authentication component</span></span>

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

### <a name="fetchdata-component"></a><span data-ttu-id="a380f-182">FetchData bileşeni</span><span class="sxs-lookup"><span data-stu-id="a380f-182">FetchData component</span></span>

[!INCLUDE[](~/includes/blazor-security/fetchdata-component.md)]

## <a name="run-the-app"></a><span data-ttu-id="a380f-183">Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="a380f-183">Run the app</span></span>

<span data-ttu-id="a380f-184">Uygulamayı sunucu projesinden çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="a380f-184">Run the app from the Server project.</span></span> <span data-ttu-id="a380f-185">Visual Studio 'Yu kullanırken **Çözüm Gezgini** ' de sunucu projesini seçin ve araç çubuğundaki **Çalıştır** düğmesini seçin veya uygulamayı **Hata Ayıkla** menüsünden başlatın.</span><span class="sxs-lookup"><span data-stu-id="a380f-185">When using Visual Studio, select the Server project in **Solution Explorer** and select the **Run** button in the toolbar or start the app from the **Debug** menu.</span></span>

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]
