---
title: Azure Active Directory ile bir ASP.NET Core Blazor Weelsembly barındırılan uygulaması güvenli hale getirme
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/16/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/hosted-with-azure-active-directory
ms.openlocfilehash: 2ddbc9791ec9b31d55c9c6017d9d6d5be5c8dec8
ms.sourcegitcommit: 5bdc54162d7dea8d9fa54ac3055678db23586af1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/17/2020
ms.locfileid: "79434492"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-hosted-app-with-azure-active-directory"></a><span data-ttu-id="bde56-102">Azure Active Directory ile bir ASP.NET Core Blazor Weelsembly barındırılan uygulaması güvenli hale getirme</span><span class="sxs-lookup"><span data-stu-id="bde56-102">Secure an ASP.NET Core Blazor WebAssembly hosted app with Azure Active Directory</span></span>

<span data-ttu-id="bde56-103">, [Javier Calvarro Nelson](https://github.com/javiercn) ve [Luke Latham](https://github.com/guardrex) 'e göre</span><span class="sxs-lookup"><span data-stu-id="bde56-103">By [Javier Calvarro Nelson](https://github.com/javiercn) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]



<span data-ttu-id="bde56-104">Bu makalede, kimlik doğrulaması için [Azure Active Directory (AAD)](https://azure.microsoft.com/services/active-directory/) kullanan bir [Blazor weelsembly barındırılan uygulamasının](xref:blazor/hosting-models#blazor-webassembly) nasıl oluşturulacağı açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="bde56-104">This article describes how to create a [Blazor WebAssembly hosted app](xref:blazor/hosting-models#blazor-webassembly) that uses [Azure Active Directory (AAD)](https://azure.microsoft.com/services/active-directory/) for authentication.</span></span>

## <a name="register-apps-in-aad-b2c-and-create-solution"></a><span data-ttu-id="bde56-105">Uygulamaları AAD B2C kaydetme ve çözüm oluşturma</span><span class="sxs-lookup"><span data-stu-id="bde56-105">Register apps in AAD B2C and create solution</span></span>

### <a name="create-a-tenant"></a><span data-ttu-id="bde56-106">Kiracı oluşturma</span><span class="sxs-lookup"><span data-stu-id="bde56-106">Create a tenant</span></span>

<span data-ttu-id="bde56-107">Hızlı başlangıç: AAD 'de kiracı oluşturmak için [bir kiracı ayarlama](/azure/active-directory/develop/quickstart-create-new-tenant) bölümündeki yönergeleri izleyin.</span><span class="sxs-lookup"><span data-stu-id="bde56-107">Follow the guidance in [Quickstart: Set up a tenant](/azure/active-directory/develop/quickstart-create-new-tenant) to create a tenant in AAD.</span></span>

### <a name="register-a-server-api-app"></a><span data-ttu-id="bde56-108">Sunucu API 'SI uygulaması kaydetme</span><span class="sxs-lookup"><span data-stu-id="bde56-108">Register a server API app</span></span>

<span data-ttu-id="bde56-109">Hızlı Başlangıç bölümündeki yönergeleri izleyin: *sunucu API 'si* **Azure Active Directory** uygulamasına yönelik bir AAD uygulamasını Azure Portal > **uygulama kayıtları** alanına kaydetmek Için MICROSOFT Identity platformu ve sonraki Azure AAD [ile uygulamayı kaydetme](/azure/active-directory/develop/quickstart-register-app) konusuna bakın:</span><span class="sxs-lookup"><span data-stu-id="bde56-109">Follow the guidance in [Quickstart: Register an application with the Microsoft identity platform](/azure/active-directory/develop/quickstart-register-app) and subsequent Azure AAD topics to register an AAD app for the *Server API app* in the **Azure Active Directory** > **App registrations** area of the Azure portal:</span></span>

1. <span data-ttu-id="bde56-110">**Yeni kayıt**seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="bde56-110">Select **New registration**.</span></span>
1. <span data-ttu-id="bde56-111">Uygulama için bir **ad** sağlayın (örneğin, **Blazor Server AAD**).</span><span class="sxs-lookup"><span data-stu-id="bde56-111">Provide a **Name** for the app (for example, **Blazor Server AAD**).</span></span>
1. <span data-ttu-id="bde56-112">Desteklenen bir **Hesap türü**seçin.</span><span class="sxs-lookup"><span data-stu-id="bde56-112">Choose a **Supported account types**.</span></span> <span data-ttu-id="bde56-113">Bu deneyim için **yalnızca bu kuruluş dizininde** (tek kiracı) hesaplar seçebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="bde56-113">You may select **Accounts in this organizational directory only** (single tenant) for this experience.</span></span>
1. <span data-ttu-id="bde56-114">*Sunucu API 'si uygulaması* Bu senaryoda **yeniden yönlendirme URI 'si** gerektirmez, bu nedenle açılan kutudan **Web** 'e ve yeniden yönlendirme URI 'si girmeyin.</span><span class="sxs-lookup"><span data-stu-id="bde56-114">The *Server API app* doesn't require a **Redirect URI** in this scenario, so leave the drop down set to **Web** and don't enter a redirect URI.</span></span>
1. <span data-ttu-id="bde56-115">**Yönetici tarafından OpenID ve offline_access izinleri için Izin ver** onay kutusunu > **izinleri** devre dışı bırakın.</span><span class="sxs-lookup"><span data-stu-id="bde56-115">Disable the **Permissions** > **Grant admin concent to openid and offline_access permissions** check box.</span></span>
1. <span data-ttu-id="bde56-116">**Kaydol**’u seçin.</span><span class="sxs-lookup"><span data-stu-id="bde56-116">Select **Register**.</span></span>

<span data-ttu-id="bde56-117">Uygulama, oturum açma veya uer profil erişimi gerektirmediğinden, **API izinlerinde** **Microsoft Graph** > **User. Read** iznini kaldırın.</span><span class="sxs-lookup"><span data-stu-id="bde56-117">In **API permissions**, remove the **Microsoft Graph** > **User.Read** permission, as the app doesn't require sign in or uer profile access.</span></span>

<span data-ttu-id="bde56-118">**API 'Yi kullanıma**sunma bölümünde:</span><span class="sxs-lookup"><span data-stu-id="bde56-118">In **Expose an API**:</span></span>

1. <span data-ttu-id="bde56-119">**Kapsam ekle**’yi seçin.</span><span class="sxs-lookup"><span data-stu-id="bde56-119">Select **Add a scope**.</span></span>
1. <span data-ttu-id="bde56-120">**Kaydet ve devam et**’i seçin.</span><span class="sxs-lookup"><span data-stu-id="bde56-120">Select **Save and continue**.</span></span>
1. <span data-ttu-id="bde56-121">Bir **kapsam adı** sağlayın (örneğin, `API.Access`).</span><span class="sxs-lookup"><span data-stu-id="bde56-121">Provide a **Scope name** (for example, `API.Access`).</span></span>
1. <span data-ttu-id="bde56-122">**Yönetici izni görünen adı** sağlayın (örneğin, `Access API`).</span><span class="sxs-lookup"><span data-stu-id="bde56-122">Provide an **Admin consent display name** (for example, `Access API`).</span></span>
1. <span data-ttu-id="bde56-123">**Yönetici onay açıklaması** sağlayın (örneğin, `Allows the app to access server app API endpoints.`).</span><span class="sxs-lookup"><span data-stu-id="bde56-123">Provide an **Admin consent description** (for example, `Allows the app to access server app API endpoints.`).</span></span>
1. <span data-ttu-id="bde56-124">**Durumun** **etkin**olarak ayarlandığını onaylayın.</span><span class="sxs-lookup"><span data-stu-id="bde56-124">Confirm that the **State** is set to **Enabled**.</span></span>
1. <span data-ttu-id="bde56-125">**Kapsam Ekle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="bde56-125">Select **Add scope**.</span></span>

<span data-ttu-id="bde56-126">Aşağıdaki bilgileri kaydedin:</span><span class="sxs-lookup"><span data-stu-id="bde56-126">Record the following information:</span></span>

* <span data-ttu-id="bde56-127">*Sunucu API 'si uygulaması* Uygulama KIMLIĞI (Istemci KIMLIĞI) (örneğin, `11111111-1111-1111-1111-111111111111`)</span><span class="sxs-lookup"><span data-stu-id="bde56-127">*Server API app* Application ID (Client ID) (for example, `11111111-1111-1111-1111-111111111111`)</span></span>
* <span data-ttu-id="bde56-128">Dizin KIMLIĞI (kiracı KIMLIĞI) (örneğin, `222222222-2222-2222-2222-222222222222`)</span><span class="sxs-lookup"><span data-stu-id="bde56-128">Directory ID (Tenant ID) (for example, `222222222-2222-2222-2222-222222222222`)</span></span>
* <span data-ttu-id="bde56-129">AAD kiracı etki alanı (örneğin, `contoso.onmicrosoft.com`)</span><span class="sxs-lookup"><span data-stu-id="bde56-129">AAD Tenant domain (for example, `contoso.onmicrosoft.com`)</span></span>
* <span data-ttu-id="bde56-130">Varsayılan kapsam (örneğin, `API.Access`)</span><span class="sxs-lookup"><span data-stu-id="bde56-130">Default scope (for example, `API.Access`)</span></span>

### <a name="register-a-client-app"></a><span data-ttu-id="bde56-131">İstemci uygulamasını kaydetme</span><span class="sxs-lookup"><span data-stu-id="bde56-131">Register a client app</span></span>

<span data-ttu-id="bde56-132">Hızlı başlangıç: **Azure Active Directory** > **uygulama kayıtları** Azure Portal alanına *istemci uygulaması* Için bir AAD uygulaması kaydetmek üzere MICROSOFT Identity platformu ve sonraki Azure AAD [ile uygulamayı kaydetme](/azure/active-directory/develop/quickstart-register-app) bölümündeki yönergeleri izleyin:</span><span class="sxs-lookup"><span data-stu-id="bde56-132">Follow the guidance in [Quickstart: Register an application with the Microsoft identity platform](/azure/active-directory/develop/quickstart-register-app) and subsequent Azure AAD topics to register a AAD app for the *Client app* in the **Azure Active Directory** > **App registrations** area of the Azure portal:</span></span>

1. <span data-ttu-id="bde56-133">**Yeni kayıt**seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="bde56-133">Select **New registration**.</span></span>
1. <span data-ttu-id="bde56-134">Uygulama için bir **ad** sağlayın (örneğin, **Blazor istemci AAD**).</span><span class="sxs-lookup"><span data-stu-id="bde56-134">Provide a **Name** for the app (for example, **Blazor Client AAD**).</span></span>
1. <span data-ttu-id="bde56-135">Desteklenen bir **Hesap türü**seçin.</span><span class="sxs-lookup"><span data-stu-id="bde56-135">Choose a **Supported account types**.</span></span> <span data-ttu-id="bde56-136">Bu deneyim için **yalnızca bu kuruluş dizininde** (tek kiracı) hesaplar seçebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="bde56-136">You may select **Accounts in this organizational directory only** (single tenant) for this experience.</span></span>
1. <span data-ttu-id="bde56-137">**Yeniden yönlendirme URI 'si** açılan listesini **Web**olarak ayarlayın ve `https://localhost:5001/authentication/login-callback`yeniden yönlendirme URI 'si sağlayın.</span><span class="sxs-lookup"><span data-stu-id="bde56-137">Leave the **Redirect URI** drop down set to **Web**, and provide a redirect URI of `https://localhost:5001/authentication/login-callback`.</span></span>
1. <span data-ttu-id="bde56-138">**Yönetici tarafından OpenID ve offline_access izinleri için Izin ver** onay kutusunu > **izinleri** devre dışı bırakın.</span><span class="sxs-lookup"><span data-stu-id="bde56-138">Disable the **Permissions** > **Grant admin concent to openid and offline_access permissions** check box.</span></span>
1. <span data-ttu-id="bde56-139">**Kaydol**’u seçin.</span><span class="sxs-lookup"><span data-stu-id="bde56-139">Select **Register**.</span></span>

<span data-ttu-id="bde56-140">**Kimlik doğrulama** > **Platform yapılandırmalarında** **Web** > :</span><span class="sxs-lookup"><span data-stu-id="bde56-140">In **Authentication** > **Platform configurations** > **Web**:</span></span>

1. <span data-ttu-id="bde56-141">`https://localhost:5001/authentication/login-callback` **yeniden yönlendirme URI 'sinin** mevcut olduğunu onaylayın.</span><span class="sxs-lookup"><span data-stu-id="bde56-141">Confirm the **Redirect URI** of `https://localhost:5001/authentication/login-callback` is present.</span></span>
1. <span data-ttu-id="bde56-142">**Örtük izin**Için, **erişim belirteçleri** ve **Kimlik belirteçleri**onay kutularını seçin.</span><span class="sxs-lookup"><span data-stu-id="bde56-142">For **Implicit grant**, select the check boxes for **Access tokens** and **ID tokens**.</span></span>
1. <span data-ttu-id="bde56-143">Uygulamanın kalan varsayılan değerleri bu deneyim için kabul edilebilir.</span><span class="sxs-lookup"><span data-stu-id="bde56-143">The remaining defaults for the app are acceptable for this experience.</span></span>
1. <span data-ttu-id="bde56-144">**Kaydet** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="bde56-144">Select the **Save** button.</span></span>

<span data-ttu-id="bde56-145">**API izinleri**:</span><span class="sxs-lookup"><span data-stu-id="bde56-145">In **API permissions**:</span></span>

1. <span data-ttu-id="bde56-146">Uygulamanın **Microsoft Graph** > **User. Read** iznine sahip olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="bde56-146">Confirm that the app has **Microsoft Graph** > **User.Read** permission.</span></span>
1. <span data-ttu-id="bde56-147">**Izin Ekle** ' yi ve ardından **API 'lerim**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="bde56-147">Select **Add a permission** followed by **My APIs**.</span></span>
1. <span data-ttu-id="bde56-148">**Ad** SÜTUNUNDAN *sunucu API uygulamasını* (ÖRNEĞIN, **Blazor Server AAD**) seçin.</span><span class="sxs-lookup"><span data-stu-id="bde56-148">Select the *Server API app* from the **Name** column (for example, **Blazor Server AAD**).</span></span>
1. <span data-ttu-id="bde56-149">**API** listesini açın.</span><span class="sxs-lookup"><span data-stu-id="bde56-149">Open the **API** list.</span></span>
1. <span data-ttu-id="bde56-150">API 'ye erişimi etkinleştirin (örneğin, `API.Access`).</span><span class="sxs-lookup"><span data-stu-id="bde56-150">Enable access to the API (for example, `API.Access`).</span></span>
1. <span data-ttu-id="bde56-151">**Izin Ekle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="bde56-151">Select **Add permissions**.</span></span>
1. <span data-ttu-id="bde56-152">**{Tenant Name} için yönetici Içeriği ver** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="bde56-152">Select the **Grant admin content for {TENANT NAME}** button.</span></span> <span data-ttu-id="bde56-153">Onaylamak için **Evet**’i seçin.</span><span class="sxs-lookup"><span data-stu-id="bde56-153">Select **Yes** to confirm.</span></span>

<span data-ttu-id="bde56-154">*İstemci* UYGULAMASı uygulama kimliğini (istemci kimliği) kaydedin (örneğin, `33333333-3333-3333-3333-333333333333`).</span><span class="sxs-lookup"><span data-stu-id="bde56-154">Record the *Client app* Application ID (Client ID) (for example, `33333333-3333-3333-3333-333333333333`).</span></span>

### <a name="create-the-app"></a><span data-ttu-id="bde56-155">Uygulama oluşturma</span><span class="sxs-lookup"><span data-stu-id="bde56-155">Create the app</span></span>

<span data-ttu-id="bde56-156">Aşağıdaki komutta yer tutucuları, daha önce kaydedilen bilgilerle değiştirin ve komutu bir komut kabuğu 'nda yürütün:</span><span class="sxs-lookup"><span data-stu-id="bde56-156">Replace the placeholders in the following command with the information recorded earlier and execute the command in a command shell:</span></span>

```dotnetcli
dotnet new blazorwasm -au SingleOrg --api-client-id "{SERVER API APP CLIENT ID}" --app-id-uri "{SERVER API APP CLIENT ID}" --client-id "{CLIENT APP CLIENT ID}" --default-scope "{DEFAULT SCOPE}" --domain "{DOMAIN}" -ho --tenant-id "{TENANT ID}"
```

<span data-ttu-id="bde56-157">Mevcut değilse bir proje klasörü oluşturan çıkış konumunu belirtmek için, çıkış seçeneğini komuta bir yol (örneğin, `-o BlazorSample`) ile birlikte ekleyin.</span><span class="sxs-lookup"><span data-stu-id="bde56-157">To specify the output location, which creates a project folder if it doesn't exist, include the output option in the command with a path (for example, `-o BlazorSample`).</span></span> <span data-ttu-id="bde56-158">Klasör adı Ayrıca projenin adının bir parçası haline gelir.</span><span class="sxs-lookup"><span data-stu-id="bde56-158">The folder name also becomes part of the project's name.</span></span>

> [!NOTE]
> <span data-ttu-id="bde56-159">Varsayılan erişim belirteci kapsamında önemli bir yapılandırma değişikliği için [kimlik doğrulama hizmeti desteği](#Authentication service support) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="bde56-159">See the [Authentication service support](#Authentication service support) section for an important configuration change to the default access token scope.</span></span> <span data-ttu-id="bde56-160">Blazor WebAssembly şablonu tarafından belirtilen değerin, *istemci uygulaması* şablondan oluşturulduktan sonra el ile değiştirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="bde56-160">The value provided by the Blazor WebAssembly template must be manually changed after the *Client app* is created from the template.</span></span>

## <a name="server-app-configuration"></a><span data-ttu-id="bde56-161">Sunucu uygulaması yapılandırması</span><span class="sxs-lookup"><span data-stu-id="bde56-161">Server app configuration</span></span>

<span data-ttu-id="bde56-162">*Bu bölüm, çözümün **sunucu** uygulamasıyla ilgilidir.*</span><span class="sxs-lookup"><span data-stu-id="bde56-162">*This section pertains to the solution's **Server** app.*</span></span>

### <a name="authentication-package"></a><span data-ttu-id="bde56-163">Kimlik doğrulama paketi</span><span class="sxs-lookup"><span data-stu-id="bde56-163">Authentication package</span></span>

<span data-ttu-id="bde56-164">ASP.NET Core Web API 'Lerine yapılan kimlik doğrulama ve yetkilendirme desteği `Microsoft.AspNetCore.Authentication.AzureAD.UI`tarafından sağlanır:</span><span class="sxs-lookup"><span data-stu-id="bde56-164">The support for authenticating and authorizing calls to ASP.NET Core Web APIs is provided by the `Microsoft.AspNetCore.Authentication.AzureAD.UI`:</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.Authentication.AzureAD.UI" 
    Version="3.1.0" />
```

### <a name="authentication-service-support"></a><span data-ttu-id="bde56-165">Kimlik doğrulama hizmeti desteği</span><span class="sxs-lookup"><span data-stu-id="bde56-165">Authentication service support</span></span>

<span data-ttu-id="bde56-166">`AddAuthentication` yöntemi, uygulama içinde kimlik doğrulama hizmetlerini ayarlar ve JWT taşıyıcı işleyicisini varsayılan kimlik doğrulama yöntemi olarak yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="bde56-166">The `AddAuthentication` method sets up authentication services within the app and configures the JWT Bearer handler as the default authentication method.</span></span> <span data-ttu-id="bde56-167">`AddAzureADBearer` yöntemi, Azure Active Directory tarafından yayılan belirteçleri doğrulamak için gereken JWT taşıyıcı işleyicisinde belirli parametreleri ayarlar:</span><span class="sxs-lookup"><span data-stu-id="bde56-167">The `AddAzureADBearer` method sets up the specific parameters in the JWT Bearer handler required to validate tokens emitted by the Azure Active Directory:</span></span>

```csharp
services.AddAuthentication(AzureADDefaults.BearerAuthenticationScheme)
    .AddAzureADBearer(options => Configuration.Bind("AzureAd", options));
```

<span data-ttu-id="bde56-168">`UseAuthentication` ve `UseAuthorization` şunları doğrulayın:</span><span class="sxs-lookup"><span data-stu-id="bde56-168">`UseAuthentication` and `UseAuthorization` ensure that:</span></span>

* <span data-ttu-id="bde56-169">Uygulama, gelen isteklerde belirteçleri ayrıştırmaya ve doğrulamaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="bde56-169">The app attempts to parse and validate tokens on incoming requests.</span></span>
* <span data-ttu-id="bde56-170">Uygun kimlik bilgileri olmadan korunan kaynağa erişmeye çalışan istekler başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="bde56-170">Any request attempting to access a protected resource without proper credentials fails.</span></span>

```csharp
app.UseAuthentication();
app.UseAuthorization();
```

### <a name="app-settings"></a><span data-ttu-id="bde56-171">Uygulama ayarları</span><span class="sxs-lookup"><span data-stu-id="bde56-171">App settings</span></span>

<span data-ttu-id="bde56-172">*AppSettings. JSON* dosyası, erişim belirteçlerini doğrulamak IÇIN kullanılan JWT taşıyıcı işleyicisini yapılandırma seçeneklerini içerir.</span><span class="sxs-lookup"><span data-stu-id="bde56-172">The *appsettings.json* file contains the options to configure the JWT bearer handler used to validate access tokens.</span></span>

```json
{
  "AzureAd": {
    "Instance": "https://login.microsoftonline.com/",
    "Domain": "{DOMAIN}",
    "TenantId": "{TENANT ID}",
    "ClientId": "{API CLIENT ID}",
  }
}
```

### <a name="weatherforecast-controller"></a><span data-ttu-id="bde56-173">Hava tahmin denetleyicisi</span><span class="sxs-lookup"><span data-stu-id="bde56-173">WeatherForecast controller</span></span>

<span data-ttu-id="bde56-174">Dalgalı tahmin denetleyicisi (*denetleyiciler/dalgalı) denetleyici. cs*), denetleyiciye uygulanan `[Authorize]` özniteliği ile korunan bir API sunar.</span><span class="sxs-lookup"><span data-stu-id="bde56-174">The WeatherForecast controller (*Controllers/WeatherForecastController.cs*) exposes a protected API with the `[Authorize]` attribute applied to the controller.</span></span> <span data-ttu-id="bde56-175">Bunun anlaşılması **önemlidir** :</span><span class="sxs-lookup"><span data-stu-id="bde56-175">It's **important** to understand that:</span></span>

* <span data-ttu-id="bde56-176">Bu API denetleyicisindeki `[Authorize]` özniteliği, bu API 'yi yetkisiz erişime karşı koruyan tek şeydir.</span><span class="sxs-lookup"><span data-stu-id="bde56-176">The `[Authorize]` attribute in this API controller is the only thing that protect this API from unauthorized access.</span></span>
* <span data-ttu-id="bde56-177">Blazor WebAssembly uygulamasında kullanılan `[Authorize]` özniteliği yalnızca uygulamanın, uygulamanın düzgün şekilde çalışması için yetkilendirilmiş olması gerektiği konusunda bir ipucu işlevi görür.</span><span class="sxs-lookup"><span data-stu-id="bde56-177">The `[Authorize]` attribute used in the Blazor WebAssembly app only serves as a hint to the app that the user should be authorized for the app to work correctly.</span></span>

```csharp
[Authorize]
[ApiController]
[Route("[controller]")]
public class WeatherForecastController : ControllerBase
{
    [HttpGet]
    public IEnumerable<WeatherForecast> Get()
    {
        ...
    }
}
```

## <a name="client-app-configuration"></a><span data-ttu-id="bde56-178">İstemci uygulama yapılandırması</span><span class="sxs-lookup"><span data-stu-id="bde56-178">Client app configuration</span></span>

<span data-ttu-id="bde56-179">*Bu bölüm, çözümün **istemci** uygulaması ile ilgilidir.*</span><span class="sxs-lookup"><span data-stu-id="bde56-179">*This section pertains to the solution's **Client** app.*</span></span>

### <a name="authentication-package"></a><span data-ttu-id="bde56-180">Kimlik doğrulama paketi</span><span class="sxs-lookup"><span data-stu-id="bde56-180">Authentication package</span></span>

<span data-ttu-id="bde56-181">Iş veya okul hesaplarını (`SingleOrg`) kullanmak üzere bir uygulama oluşturulduğunda, uygulama [Microsoft kimlik doğrulama kitaplığı](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`) için bir paket başvurusu otomatik olarak alır.</span><span class="sxs-lookup"><span data-stu-id="bde56-181">When an app is created to use Work or School Accounts (`SingleOrg`), the app automatically receives a package reference for the [Microsoft Authentication Library](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`).</span></span> <span data-ttu-id="bde56-182">Paket, uygulamanın kullanıcıların kimliğini doğrulamasına ve korunan API 'Leri çağırmak için belirteçleri almasına yardımcı olan bir dizi temel sunar.</span><span class="sxs-lookup"><span data-stu-id="bde56-182">The package provides a set of primitives that help the app authenticate users and obtain tokens to call protected APIs.</span></span>

<span data-ttu-id="bde56-183">Bir uygulamaya kimlik doğrulaması ekliyorsanız, paketi uygulamanın proje dosyasına el ile ekleyin:</span><span class="sxs-lookup"><span data-stu-id="bde56-183">If adding authentication to an app, manually add the package to the app's project file:</span></span>

```xml
<PackageReference Include="Microsoft.Authentication.WebAssembly.Msal" 
    Version="{VERSION}" />
```

<span data-ttu-id="bde56-184">Önceki paket başvurusunda `{VERSION}`, <xref:blazor/get-started> makalesinde gösterilen `Microsoft.AspNetCore.Blazor.Templates` paketinin sürümü ile değiştirin.</span><span class="sxs-lookup"><span data-stu-id="bde56-184">Replace `{VERSION}` in the preceding package reference with the version of the `Microsoft.AspNetCore.Blazor.Templates` package shown in the <xref:blazor/get-started> article.</span></span>

<span data-ttu-id="bde56-185">`Microsoft.Authentication.WebAssembly.Msal` paketi, `Microsoft.AspNetCore.Components.WebAssembly.Authentication` paketini uygulamaya göre geçişli olarak ekler.</span><span class="sxs-lookup"><span data-stu-id="bde56-185">The `Microsoft.Authentication.WebAssembly.Msal` package transitively adds the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package to the app.</span></span>

### <a name="authentication-service-support"></a><span data-ttu-id="bde56-186">Kimlik doğrulama hizmeti desteği</span><span class="sxs-lookup"><span data-stu-id="bde56-186">Authentication service support</span></span>

<span data-ttu-id="bde56-187">Kullanıcıları kimlik doğrulama desteği, hizmet kapsayıcısına `Microsoft.Authentication.WebAssembly.Msal` paketi tarafından sağlanmış `AddMsalAuthentication` uzantısı yöntemiyle kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="bde56-187">Support for authenticating users is registered in the service container with the `AddMsalAuthentication` extension method provided by the `Microsoft.Authentication.WebAssembly.Msal` package.</span></span> <span data-ttu-id="bde56-188">Bu yöntem, uygulamanın kimlik sağlayıcısıyla (IP) etkileşim kurması için gereken tüm hizmetleri ayarlar.</span><span class="sxs-lookup"><span data-stu-id="bde56-188">This method sets up all of the services required for the app to interact with the Identity Provider (IP).</span></span>

<span data-ttu-id="bde56-189">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="bde56-189">*Program.cs*:</span></span>

<span data-ttu-id="bde56-190">*İstemci uygulaması* oluşturulduğunda, varsayılan erişim belirteci kapsamı `api://{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}`biçimindedir.</span><span class="sxs-lookup"><span data-stu-id="bde56-190">When the *Client app* is generated, the default access token scope is of the format `api://{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}`.</span></span> <span data-ttu-id="bde56-191">**Kapsam değerinin `api://` kısmını kaldırın.**</span><span class="sxs-lookup"><span data-stu-id="bde56-191">**Remove the `api://` portion of the scope value.**</span></span> <span data-ttu-id="bde56-192">Bu sorun gelecekteki bir önizleme sürümünde giderilecektir.</span><span class="sxs-lookup"><span data-stu-id="bde56-192">This issue will be addressed in a future preview release.</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    var authentication = options.ProviderOptions.Authentication;
    authentication.Authority = "https://login.microsoftonline.com/{TENANT ID}";
    authentication.ClientId = "{CLIENT ID}";
    options.ProviderOptions.DefaultAccessTokenScopes.Add(
        "{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}");
});
```

> [!NOTE]
> <span data-ttu-id="bde56-193">Varsayılan erişim belirteci kapsamı `{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}` biçiminde olmalıdır (örneğin, `11111111-1111-1111-1111-111111111111/API.Access`).</span><span class="sxs-lookup"><span data-stu-id="bde56-193">The default access token scope must be in the format `{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}` (for example, `11111111-1111-1111-1111-111111111111/API.Access`).</span></span> <span data-ttu-id="bde56-194">Kapsam ayarına bir düzen veya şema ve ana bilgisayar sağlanmışsa (Azure portalında gösterildiği gibi), *istemci uygulaması* *sunucu apı*uygulamasından bir *401 Yetkisiz* yanıt aldığında işlenmeyen bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="bde56-194">If a scheme or scheme and host is provided to the scope setting (as shown in the Azure Portal), the *Client app* throws an unhandled exception when it receives a *401 Unauthorized* response from the *Server API app*.</span></span>

<span data-ttu-id="bde56-195">`AddMsalAuthentication` yöntemi, bir uygulamanın kimliğini doğrulamak için gereken parametreleri yapılandırmak için bir geri çağırma işlemini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="bde56-195">The `AddMsalAuthentication` method accepts a callback to configure the parameters required to authenticate an app.</span></span> <span data-ttu-id="bde56-196">Uygulamanın yapılandırılması için gereken değerler, uygulamayı kaydettiğinizde Azure Portal AAD yapılandırmasından elde edilebilir.</span><span class="sxs-lookup"><span data-stu-id="bde56-196">The values required for configuring the app can be obtained from the Azure Portal AAD configuration when you register the app.</span></span>

<span data-ttu-id="bde56-197">Varsayılan erişim belirteci kapsamları, erişim belirteci kapsamlarının listesini temsil eder:</span><span class="sxs-lookup"><span data-stu-id="bde56-197">The default access token scopes represent the list of access token scopes that are:</span></span>

* <span data-ttu-id="bde56-198">Oturum açma isteğine varsayılan olarak dahildir.</span><span class="sxs-lookup"><span data-stu-id="bde56-198">Included by default in the sign in request.</span></span>
* <span data-ttu-id="bde56-199">Kimlik doğrulamasından hemen sonra bir erişim belirteci sağlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="bde56-199">Used to provision an access token immediately after authentication.</span></span>

<span data-ttu-id="bde56-200">Tüm kapsamlar Azure Active Directory kuralları başına aynı uygulamaya ait olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="bde56-200">All scopes must belong to the same app per Azure Active Directory rules.</span></span> <span data-ttu-id="bde56-201">Gerektiğinde ek API uygulamaları için ek kapsamlar eklenebilir:</span><span class="sxs-lookup"><span data-stu-id="bde56-201">Additional scopes can be added for additional API apps as needed:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultAccessTokenScopes.Add(
        "{SERVER API APP CLIENT ID}/{SCOPE}");
});
```

### <a name="index-page"></a><span data-ttu-id="bde56-202">Dizin sayfası</span><span class="sxs-lookup"><span data-stu-id="bde56-202">Index page</span></span>

[!INCLUDE[](~/includes/blazor-security/index-page.md)]

### <a name="app-component"></a><span data-ttu-id="bde56-203">Uygulama bileşeni</span><span class="sxs-lookup"><span data-stu-id="bde56-203">App component</span></span>

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

### <a name="redirecttologin-component"></a><span data-ttu-id="bde56-204">RedirectToLogin bileşeni</span><span class="sxs-lookup"><span data-stu-id="bde56-204">RedirectToLogin component</span></span>

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

### <a name="logindisplay-component"></a><span data-ttu-id="bde56-205">LoginDisplay bileşeni</span><span class="sxs-lookup"><span data-stu-id="bde56-205">LoginDisplay component</span></span>

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

### <a name="authentication-component"></a><span data-ttu-id="bde56-206">Kimlik doğrulama bileşeni</span><span class="sxs-lookup"><span data-stu-id="bde56-206">Authentication component</span></span>

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

### <a name="fetchdata-component"></a><span data-ttu-id="bde56-207">FetchData bileşeni</span><span class="sxs-lookup"><span data-stu-id="bde56-207">FetchData component</span></span>

[!INCLUDE[](~/includes/blazor-security/fetchdata-component.md)]

## <a name="run-the-app"></a><span data-ttu-id="bde56-208">Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="bde56-208">Run the app</span></span>

<span data-ttu-id="bde56-209">Uygulamayı sunucu projesinden çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="bde56-209">Run the app from the Server project.</span></span> <span data-ttu-id="bde56-210">Visual Studio 'Yu kullanırken **Çözüm Gezgini** ' de sunucu projesini seçin ve araç çubuğundaki **Çalıştır** düğmesini seçin veya uygulamayı **Hata Ayıkla** menüsünden başlatın.</span><span class="sxs-lookup"><span data-stu-id="bde56-210">When using Visual Studio, select the Server project in **Solution Explorer** and select the **Run** button in the toolbar or start the app from the **Debug** menu.</span></span>

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a><span data-ttu-id="bde56-211">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="bde56-211">Additional resources</span></span>

* <xref:security/authentication/azure-active-directory/index>
