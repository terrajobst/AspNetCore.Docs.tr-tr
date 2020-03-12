---
title: Azure Active Directory ile bir ASP.NET Core Blazor WebAssembly tek başına uygulamasını güvenli hale getirme
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/09/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/standalone-with-azure-active-directory
ms.openlocfilehash: 76bcac29d86a236e56c0eaea24a694c4845ecbcf
ms.sourcegitcommit: 98bcf5fe210931e3eb70f82fd675d8679b33f5d6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/11/2020
ms.locfileid: "79083750"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-standalone-app-with-azure-active-directory"></a><span data-ttu-id="fb0fe-102">Azure Active Directory ile bir ASP.NET Core Blazor WebAssembly tek başına uygulamasını güvenli hale getirme</span><span class="sxs-lookup"><span data-stu-id="fb0fe-102">Secure an ASP.NET Core Blazor WebAssembly standalone app with Azure Active Directory</span></span>

<span data-ttu-id="fb0fe-103">, [Javier Calvarro Nelson](https://github.com/javiercn) ve [Luke Latham](https://github.com/guardrex) 'e göre</span><span class="sxs-lookup"><span data-stu-id="fb0fe-103">By [Javier Calvarro Nelson](https://github.com/javiercn) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

<span data-ttu-id="fb0fe-104">Kimlik doğrulaması için [Azure Active Directory (AAD)](https://azure.microsoft.com/services/active-directory/) kullanan bir Blazor WebAssembly tek başına uygulaması oluşturmak için:</span><span class="sxs-lookup"><span data-stu-id="fb0fe-104">To create a Blazor WebAssembly standalone app that uses [Azure Active Directory (AAD)](https://azure.microsoft.com/services/active-directory/) for authentication:</span></span>

<span data-ttu-id="fb0fe-105">[AAD kiracısı ve Web uygulaması oluşturma](/azure/active-directory/develop/v2-overview):</span><span class="sxs-lookup"><span data-stu-id="fb0fe-105">[Create an AAD tenant and web application](/azure/active-directory/develop/v2-overview):</span></span>

<span data-ttu-id="fb0fe-106">Azure portal **Azure Active Directory** > **uygulama KAYıTLARı** alanına bir AAD uygulaması kaydedin:</span><span class="sxs-lookup"><span data-stu-id="fb0fe-106">Register a AAD app in the **Azure Active Directory** > **App registrations** area of the Azure portal:</span></span>

1. <span data-ttu-id="fb0fe-107">Uygulama için bir **ad** sağlayın (örneğin, **Blazor istemci AAD**).</span><span class="sxs-lookup"><span data-stu-id="fb0fe-107">Provide a **Name** for the app (for example, **Blazor Client AAD**).</span></span>
1. <span data-ttu-id="fb0fe-108">Desteklenen bir **Hesap türü**seçin.</span><span class="sxs-lookup"><span data-stu-id="fb0fe-108">Choose a **Supported account types**.</span></span> <span data-ttu-id="fb0fe-109">Bu **kuruluş dizininde yalnızca** bu deneyim için hesaplar seçebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fb0fe-109">You may select **Accounts in this organizational directory only** for this experience.</span></span>
1. <span data-ttu-id="fb0fe-110">**Yeniden yönlendirme URI 'si** açılan listesini **Web**olarak ayarlayın ve `https://localhost:5001/authentication/login-callback`yeniden yönlendirme URI 'si sağlayın.</span><span class="sxs-lookup"><span data-stu-id="fb0fe-110">Leave the **Redirect URI** drop down set to **Web**, and provide a redirect URI of `https://localhost:5001/authentication/login-callback`.</span></span>
1. <span data-ttu-id="fb0fe-111">**Yönetici tarafından OpenID ve offline_access izinleri için Izin ver** onay kutusunu > **izinleri** devre dışı bırakın.</span><span class="sxs-lookup"><span data-stu-id="fb0fe-111">Disable the **Permissions** > **Grant admin concent to openid and offline_access permissions** check box.</span></span>
1. <span data-ttu-id="fb0fe-112">**Kaydol**’u seçin.</span><span class="sxs-lookup"><span data-stu-id="fb0fe-112">Select **Register**.</span></span>

<span data-ttu-id="fb0fe-113">**Kimlik doğrulama** > **Platform yapılandırmalarında** **Web** > :</span><span class="sxs-lookup"><span data-stu-id="fb0fe-113">In **Authentication** > **Platform configurations** > **Web**:</span></span>

1. <span data-ttu-id="fb0fe-114">`https://localhost:5001/authentication/login-callback` **yeniden yönlendirme URI 'sinin** mevcut olduğunu onaylayın.</span><span class="sxs-lookup"><span data-stu-id="fb0fe-114">Confirm the **Redirect URI** of `https://localhost:5001/authentication/login-callback` is present.</span></span>
1. <span data-ttu-id="fb0fe-115">**Örtük izin**Için, **erişim belirteçleri** ve **Kimlik belirteçleri**onay kutularını seçin.</span><span class="sxs-lookup"><span data-stu-id="fb0fe-115">For **Implicit grant**, select the check boxes for **Access tokens** and **ID tokens**.</span></span>
1. <span data-ttu-id="fb0fe-116">Uygulamanın kalan varsayılan değerleri bu deneyim için kabul edilebilir.</span><span class="sxs-lookup"><span data-stu-id="fb0fe-116">The remaining defaults for the app are acceptable for this experience.</span></span>
1. <span data-ttu-id="fb0fe-117">**Kaydet** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="fb0fe-117">Select the **Save** button.</span></span>

<span data-ttu-id="fb0fe-118">Aşağıdaki bilgileri kaydedin:</span><span class="sxs-lookup"><span data-stu-id="fb0fe-118">Record the following information:</span></span>

* <span data-ttu-id="fb0fe-119">Uygulama KIMLIĞI (Istemci KIMLIĞI) (örneğin, `11111111-1111-1111-1111-111111111111`)</span><span class="sxs-lookup"><span data-stu-id="fb0fe-119">Application ID (Client ID) (for example, `11111111-1111-1111-1111-111111111111`)</span></span>
* <span data-ttu-id="fb0fe-120">Dizin KIMLIĞI (kiracı KIMLIĞI) (örneğin, `22222222-2222-2222-2222-222222222222`)</span><span class="sxs-lookup"><span data-stu-id="fb0fe-120">Directory ID (Tenant ID) (for example, `22222222-2222-2222-2222-222222222222`)</span></span>

<span data-ttu-id="fb0fe-121">Aşağıdaki komutta yer tutucuları, daha önce kaydedilen bilgilerle değiştirin ve komutu bir komut kabuğu 'nda yürütün:</span><span class="sxs-lookup"><span data-stu-id="fb0fe-121">Replace the placeholders in the following command with the information recorded earlier and execute the command in a command shell:</span></span>

```dotnetcli
dotnet new blazorwasm -au SingleOrg --client-id "{CLIENT ID}" --tenant-id "{TENANT ID}"
```

<span data-ttu-id="fb0fe-122">Mevcut değilse bir proje klasörü oluşturan çıkış konumunu belirtmek için, çıkış seçeneğini komuta bir yol (örneğin, `-o BlazorSample`) ile birlikte ekleyin.</span><span class="sxs-lookup"><span data-stu-id="fb0fe-122">To specify the output location, which creates a project folder if it doesn't exist, include the output option in the command with a path (for example, `-o BlazorSample`).</span></span> <span data-ttu-id="fb0fe-123">Klasör adı Ayrıca projenin adının bir parçası haline gelir.</span><span class="sxs-lookup"><span data-stu-id="fb0fe-123">The folder name also becomes part of the project's name.</span></span>

## <a name="authentication-package"></a><span data-ttu-id="fb0fe-124">Kimlik doğrulama paketi</span><span class="sxs-lookup"><span data-stu-id="fb0fe-124">Authentication package</span></span>

<span data-ttu-id="fb0fe-125">Iş veya okul hesaplarını (`SingleOrg`) kullanmak üzere bir uygulama oluşturulduğunda, uygulama [Microsoft kimlik doğrulama kitaplığı](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`) için bir paket başvurusu otomatik olarak alır.</span><span class="sxs-lookup"><span data-stu-id="fb0fe-125">When an app is created to use Work or School Accounts (`SingleOrg`), the app automatically receives a package reference for the [Microsoft Authentication Library](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`).</span></span> <span data-ttu-id="fb0fe-126">Paket, uygulamanın kullanıcıların kimliğini doğrulamasına ve korunan API 'Leri çağırmak için belirteçleri almasına yardımcı olan bir dizi temel sunar.</span><span class="sxs-lookup"><span data-stu-id="fb0fe-126">The package provides a set of primitives that help the app authenticate users and obtain tokens to call protected APIs.</span></span>

<span data-ttu-id="fb0fe-127">Bir uygulamaya kimlik doğrulaması ekliyorsanız, paketi uygulamanın proje dosyasına el ile ekleyin:</span><span class="sxs-lookup"><span data-stu-id="fb0fe-127">If adding authentication to an app, manually add the package to the app's project file:</span></span>

```xml
<PackageReference Include="Microsoft.Authentication.WebAssembly.Msal" 
    Version="{VERSION}" />
```

<span data-ttu-id="fb0fe-128">Önceki paket başvurusunda `{VERSION}`, <xref:blazor/get-started> makalesinde gösterilen `Microsoft.AspNetCore.Blazor.Templates` paketinin sürümü ile değiştirin.</span><span class="sxs-lookup"><span data-stu-id="fb0fe-128">Replace `{VERSION}` in the preceding package reference with the version of the `Microsoft.AspNetCore.Blazor.Templates` package shown in the <xref:blazor/get-started> article.</span></span>

<span data-ttu-id="fb0fe-129">`Microsoft.Authentication.WebAssembly.Msal` paketi, `Microsoft.AspNetCore.Components.WebAssembly.Authentication` paketini uygulamaya göre geçişli olarak ekler.</span><span class="sxs-lookup"><span data-stu-id="fb0fe-129">The `Microsoft.Authentication.WebAssembly.Msal` package transitively adds the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package to the app.</span></span>

## <a name="authentication-service-support"></a><span data-ttu-id="fb0fe-130">Kimlik doğrulama hizmeti desteği</span><span class="sxs-lookup"><span data-stu-id="fb0fe-130">Authentication service support</span></span>

<span data-ttu-id="fb0fe-131">Kullanıcıları kimlik doğrulama desteği, hizmet kapsayıcısına `Microsoft.Authentication.WebAssembly.Msal` paketi tarafından sağlanmış `AddMsalAuthentication` uzantısı yöntemiyle kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="fb0fe-131">Support for authenticating users is registered in the service container with the `AddMsalAuthentication` extension method provided by the `Microsoft.Authentication.WebAssembly.Msal` package.</span></span> <span data-ttu-id="fb0fe-132">Bu yöntem, uygulamanın kimlik sağlayıcısıyla (IP) etkileşim kurması için gereken tüm hizmetleri ayarlar.</span><span class="sxs-lookup"><span data-stu-id="fb0fe-132">This method sets up all of the services required for the app to interact with the Identity Provider (IP).</span></span>

<span data-ttu-id="fb0fe-133">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="fb0fe-133">*Program.cs*:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    var authentication = options.ProviderOptions.Authentication;
    authentication.Authority = "https://login.microsoftonline.com/{TENANT ID}";
    authentication.ClientId = "{CLIENT ID}";
});
```

<span data-ttu-id="fb0fe-134">`AddMsalAuthentication` yöntemi, bir uygulamanın kimliğini doğrulamak için gereken parametreleri yapılandırmak için bir geri çağırma işlemini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="fb0fe-134">The `AddMsalAuthentication` method accepts a callback to configure the parameters required to authenticate an app.</span></span> <span data-ttu-id="fb0fe-135">Uygulamanın yapılandırılması için gereken değerler, uygulamayı kaydettiğinizde Azure Portal AAD yapılandırmasından elde edilebilir.</span><span class="sxs-lookup"><span data-stu-id="fb0fe-135">The values required for configuring the app can be obtained from the Azure Portal AAD configuration when you register the app.</span></span>

<span data-ttu-id="fb0fe-136">Blazor WebAssembly şablonu, uygulamayı güvenli bir API için erişim belirteci isteyecek şekilde otomatik olarak yapılandırmaz.</span><span class="sxs-lookup"><span data-stu-id="fb0fe-136">The Blazor WebAssembly template doesn't automatically configure the app to request an access token for a secure API.</span></span> <span data-ttu-id="fb0fe-137">Oturum açma akışının bir parçası olarak bir belirteç sağlamak için, kapsamı `MsalProviderOptions`varsayılan erişim belirteci kapsamlarına ekleyin:</span><span class="sxs-lookup"><span data-stu-id="fb0fe-137">To provision a token as part of the sign-in flow, add the scope to the default access token scopes of the `MsalProviderOptions`:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultAccessTokenScopes.Add(
        "{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}");
});
```

> [!NOTE]
> <span data-ttu-id="fb0fe-138">Varsayılan erişim belirteci kapsamı `{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}` biçiminde olmalıdır (örneğin, `11111111-1111-1111-1111-111111111111/API.Access`).</span><span class="sxs-lookup"><span data-stu-id="fb0fe-138">The default access token scope must be in the format `{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}` (for example, `11111111-1111-1111-1111-111111111111/API.Access`).</span></span> <span data-ttu-id="fb0fe-139">Kapsam ayarına bir düzen veya şema ve ana bilgisayar sağlanmışsa (Azure portalında gösterildiği gibi), *istemci uygulaması* *sunucu apı*uygulamasından bir *401 Yetkisiz* yanıt aldığında işlenmeyen bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="fb0fe-139">If a scheme or scheme and host is provided to the scope setting (as shown in the Azure Portal), the *Client app* throws an unhandled exception when it receives a *401 Unauthorized* response from the *Server API app*.</span></span>

## <a name="index-page"></a><span data-ttu-id="fb0fe-140">Dizin sayfası</span><span class="sxs-lookup"><span data-stu-id="fb0fe-140">Index page</span></span>

[!INCLUDE[](~/includes/blazor-security/index-page.md)]

## <a name="app-component"></a><span data-ttu-id="fb0fe-141">Uygulama bileşeni</span><span class="sxs-lookup"><span data-stu-id="fb0fe-141">App component</span></span>

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

## <a name="redirecttologin-component"></a><span data-ttu-id="fb0fe-142">RedirectToLogin bileşeni</span><span class="sxs-lookup"><span data-stu-id="fb0fe-142">RedirectToLogin component</span></span>

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

## <a name="logindisplay-component"></a><span data-ttu-id="fb0fe-143">LoginDisplay bileşeni</span><span class="sxs-lookup"><span data-stu-id="fb0fe-143">LoginDisplay component</span></span>

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

## <a name="authentication-component"></a><span data-ttu-id="fb0fe-144">Kimlik doğrulama bileşeni</span><span class="sxs-lookup"><span data-stu-id="fb0fe-144">Authentication component</span></span>

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a><span data-ttu-id="fb0fe-145">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="fb0fe-145">Additional resources</span></span>

* <xref:security/authentication/azure-active-directory/index>
