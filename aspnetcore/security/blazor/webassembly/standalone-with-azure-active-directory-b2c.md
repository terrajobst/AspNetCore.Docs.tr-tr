---
title: Azure Active Directory B2C ile bir ASP.NET Core Blazor WebAssembly tek başına uygulamasını güvenli hale getirme
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/09/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/standalone-with-azure-active-directory-b2c
ms.openlocfilehash: 0ea42943c908d8cf9d083c1cfc568c1835588ce9
ms.sourcegitcommit: 98bcf5fe210931e3eb70f82fd675d8679b33f5d6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/11/2020
ms.locfileid: "79083834"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-standalone-app-with-azure-active-directory-b2c"></a><span data-ttu-id="a4e84-102">Azure Active Directory B2C ile bir ASP.NET Core Blazor WebAssembly tek başına uygulamasını güvenli hale getirme</span><span class="sxs-lookup"><span data-stu-id="a4e84-102">Secure an ASP.NET Core Blazor WebAssembly standalone app with Azure Active Directory B2C</span></span>

<span data-ttu-id="a4e84-103">, [Javier Calvarro Nelson](https://github.com/javiercn) ve [Luke Latham](https://github.com/guardrex) 'e göre</span><span class="sxs-lookup"><span data-stu-id="a4e84-103">By [Javier Calvarro Nelson](https://github.com/javiercn) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

<span data-ttu-id="a4e84-104">Kimlik doğrulaması için [Azure Active Directory (AAD) B2C](/azure/active-directory-b2c/overview) kullanan bir Blazor WebAssembly tek başına uygulaması oluşturmak için:</span><span class="sxs-lookup"><span data-stu-id="a4e84-104">To create a Blazor WebAssembly standalone app that uses [Azure Active Directory (AAD) B2C](/azure/active-directory-b2c/overview) for authentication:</span></span>

1. <span data-ttu-id="a4e84-105">Bir kiracı oluşturmak ve bir Web uygulamasını Azure portalında kaydetmek için aşağıdaki konulardaki yönergeleri izleyin:</span><span class="sxs-lookup"><span data-stu-id="a4e84-105">Follow the guidance in the following topics to create a tenant and register a web app in the Azure Portal:</span></span>

   * <span data-ttu-id="a4e84-106">[AAD B2C kiracı oluşturun](/azure/active-directory-b2c/tutorial-create-tenant) &ndash; aşağıdaki bilgileri kaydedin:</span><span class="sxs-lookup"><span data-stu-id="a4e84-106">[Create an AAD B2C tenant](/azure/active-directory-b2c/tutorial-create-tenant) &ndash; Record the following information:</span></span>

     <span data-ttu-id="a4e84-107">1 \.</span><span class="sxs-lookup"><span data-stu-id="a4e84-107">1\.</span></span> <span data-ttu-id="a4e84-108">AAD B2C örneği (örneğin, sonunda eğik çizgi içeren `https://contoso.b2clogin.com/`)</span><span class="sxs-lookup"><span data-stu-id="a4e84-108">AAD B2C instance (for example, `https://contoso.b2clogin.com/`, which includes the trailing slash)</span></span><br>
     <span data-ttu-id="a4e84-109">2 \.</span><span class="sxs-lookup"><span data-stu-id="a4e84-109">2\.</span></span> <span data-ttu-id="a4e84-110">AAD B2C kiracı etki alanı (örneğin, `contoso.onmicrosoft.com`)</span><span class="sxs-lookup"><span data-stu-id="a4e84-110">AAD B2C Tenant domain (for example, `contoso.onmicrosoft.com`)</span></span>

   * <span data-ttu-id="a4e84-111">[Bir Web uygulamasını kaydedin](/azure/active-directory-b2c/tutorial-register-applications) &ndash; uygulama kaydı sırasında aşağıdaki seçimleri yapın:</span><span class="sxs-lookup"><span data-stu-id="a4e84-111">[Register a web application](/azure/active-directory-b2c/tutorial-register-applications) &ndash; Make the following selections during app registration:</span></span>

     <span data-ttu-id="a4e84-112">1 \.</span><span class="sxs-lookup"><span data-stu-id="a4e84-112">1\.</span></span> <span data-ttu-id="a4e84-113">**Web uygulaması/Web API 'Sini** **Evet**olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="a4e84-113">Set **Web App / Web API** to **Yes**.</span></span><br>
     <span data-ttu-id="a4e84-114">2 \.</span><span class="sxs-lookup"><span data-stu-id="a4e84-114">2\.</span></span> <span data-ttu-id="a4e84-115">**Örtük akışa Izin ver** ' i **Evet**olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="a4e84-115">Set **Allow implicit flow** to **Yes**.</span></span><br>
     <span data-ttu-id="a4e84-116">3 \.</span><span class="sxs-lookup"><span data-stu-id="a4e84-116">3\.</span></span> <span data-ttu-id="a4e84-117">`https://localhost:5001/authentication/login-callback`bir **yanıt URL 'si** ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a4e84-117">Add a **Reply URL** of `https://localhost:5001/authentication/login-callback`.</span></span>

     <span data-ttu-id="a4e84-118">Uygulama KIMLIĞINI (Istemci KIMLIĞI) kaydedin (örneğin, `11111111-1111-1111-1111-111111111111`).</span><span class="sxs-lookup"><span data-stu-id="a4e84-118">Record the Application ID (Client ID) (for example, `11111111-1111-1111-1111-111111111111`).</span></span>

   * <span data-ttu-id="a4e84-119">& Ndash; [Kullanıcı akışları oluşturun](/azure/active-directory-b2c/tutorial-create-user-flows) Kaydolma ve oturum açma Kullanıcı akışı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="a4e84-119">[Create user flows](/azure/active-directory-b2c/tutorial-create-user-flows) & ndash; Create a sign-up and sign-in user flow.</span></span>

     <span data-ttu-id="a4e84-120">Uygulama için oluşturulan kaydolma ve oturum açma Kullanıcı akış adını kaydedin (örneğin, `B2C_1_signupsignin`).</span><span class="sxs-lookup"><span data-stu-id="a4e84-120">Record the sign-up and sign-in user flow name created for the app (for example, `B2C_1_signupsignin`).</span></span>

1. <span data-ttu-id="a4e84-121">Aşağıdaki komutta yer tutucuları, daha önce kaydedilen bilgilerle değiştirin ve komutu bir komut kabuğu 'nda yürütün:</span><span class="sxs-lookup"><span data-stu-id="a4e84-121">Replace the placeholders in the following command with the information recorded earlier and execute the command in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -au IndividualB2C --aad-b2c-instance "{AAD B2C INSTANCE}" --client-id "{CLIENT ID}" --domain "{DOMAIN}" -ssp "{SIGN UP OR SIGN IN POLICY}"
   ```

   <span data-ttu-id="a4e84-122">Mevcut değilse bir proje klasörü oluşturan çıkış konumunu belirtmek için, çıkış seçeneğini komuta bir yol (örneğin, `-o BlazorSample`) ile birlikte ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a4e84-122">To specify the output location, which creates a project folder if it doesn't exist, include the output option in the command with a path (for example, `-o BlazorSample`).</span></span> <span data-ttu-id="a4e84-123">Klasör adı Ayrıca projenin adının bir parçası haline gelir.</span><span class="sxs-lookup"><span data-stu-id="a4e84-123">The folder name also becomes part of the project's name.</span></span>

## <a name="authentication-package"></a><span data-ttu-id="a4e84-124">Kimlik doğrulama paketi</span><span class="sxs-lookup"><span data-stu-id="a4e84-124">Authentication package</span></span>

<span data-ttu-id="a4e84-125">Tek bir B2C hesabı (`IndividualB2C`) kullanmak üzere bir uygulama oluşturulduğunda, uygulama otomatik olarak [Microsoft kimlik doğrulama kitaplığı](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`) için bir paket başvurusu alır.</span><span class="sxs-lookup"><span data-stu-id="a4e84-125">When an app is created to use an Individual B2C Account (`IndividualB2C`), the app automatically receives a package reference for the [Microsoft Authentication Library](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`).</span></span> <span data-ttu-id="a4e84-126">Paket, uygulamanın kullanıcıların kimliğini doğrulamasına ve korunan API 'Leri çağırmak için belirteçleri almasına yardımcı olan bir dizi temel sunar.</span><span class="sxs-lookup"><span data-stu-id="a4e84-126">The package provides a set of primitives that help the app authenticate users and obtain tokens to call protected APIs.</span></span>

<span data-ttu-id="a4e84-127">Bir uygulamaya kimlik doğrulaması ekliyorsanız, paketi uygulamanın proje dosyasına el ile ekleyin:</span><span class="sxs-lookup"><span data-stu-id="a4e84-127">If adding authentication to an app, manually add the package to the app's project file:</span></span>

```xml
<PackageReference Include="Microsoft.Authentication.WebAssembly.Msal" 
    Version="{VERSION}" />
```

<span data-ttu-id="a4e84-128">Önceki paket başvurusunda `{VERSION}`, <xref:blazor/get-started> makalesinde gösterilen `Microsoft.AspNetCore.Blazor.Templates` paketinin sürümü ile değiştirin.</span><span class="sxs-lookup"><span data-stu-id="a4e84-128">Replace `{VERSION}` in the preceding package reference with the version of the `Microsoft.AspNetCore.Blazor.Templates` package shown in the <xref:blazor/get-started> article.</span></span>

<span data-ttu-id="a4e84-129">`Microsoft.Authentication.WebAssembly.Msal` paketi, `Microsoft.AspNetCore.Components.WebAssembly.Authentication` paketini uygulamaya göre geçişli olarak ekler.</span><span class="sxs-lookup"><span data-stu-id="a4e84-129">The `Microsoft.Authentication.WebAssembly.Msal` package transitively adds the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package to the app.</span></span>

## <a name="authentication-service-support"></a><span data-ttu-id="a4e84-130">Kimlik doğrulama hizmeti desteği</span><span class="sxs-lookup"><span data-stu-id="a4e84-130">Authentication service support</span></span>

<span data-ttu-id="a4e84-131">Kullanıcıları kimlik doğrulama desteği, hizmet kapsayıcısına `Microsoft.Authentication.WebAssembly.Msal` paketi tarafından sağlanmış `AddMsalAuthentication` uzantısı yöntemiyle kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="a4e84-131">Support for authenticating users is registered in the service container with the `AddMsalAuthentication` extension method provided by the `Microsoft.Authentication.WebAssembly.Msal` package.</span></span> <span data-ttu-id="a4e84-132">Bu yöntem, uygulamanın kimlik sağlayıcısıyla (IP) etkileşim kurması için gereken tüm hizmetleri ayarlar.</span><span class="sxs-lookup"><span data-stu-id="a4e84-132">This method sets up all of the services required for the app to interact with the Identity Provider (IP).</span></span>

<span data-ttu-id="a4e84-133">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="a4e84-133">*Program.cs*:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    var authentication = options.ProviderOptions.Authentication;
    authentication.Authority = 
        "{AAD B2C INSTANCE}{DOMAIN}/{SIGN UP OR SIGN IN POLICY}";
    authentication.ClientId = "{CLIENT ID}";
    authentication.ValidateAuthority = false;
});
```

<span data-ttu-id="a4e84-134">`AddMsalAuthentication` yöntemi, bir uygulamanın kimliğini doğrulamak için gereken parametreleri yapılandırmak için bir geri çağırma işlemini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="a4e84-134">The `AddMsalAuthentication` method accepts a callback to configure the parameters required to authenticate an app.</span></span> <span data-ttu-id="a4e84-135">Uygulamanın yapılandırılması için gereken değerler, uygulamayı kaydettiğinizde Azure Portal AAD yapılandırmasından elde edilebilir.</span><span class="sxs-lookup"><span data-stu-id="a4e84-135">The values required for configuring the app can be obtained from the Azure Portal AAD configuration when you register the app.</span></span>

<span data-ttu-id="a4e84-136">Blazor WebAssembly şablonu, uygulamayı güvenli bir API için erişim belirteci isteyecek şekilde otomatik olarak yapılandırmaz.</span><span class="sxs-lookup"><span data-stu-id="a4e84-136">The Blazor WebAssembly template doesn't automatically configure the app to request an access token for a secure API.</span></span> <span data-ttu-id="a4e84-137">Oturum açma akışının bir parçası olarak bir belirteç sağlamak için, kapsamı `MsalProviderOptions`varsayılan erişim belirteci kapsamlarına ekleyin:</span><span class="sxs-lookup"><span data-stu-id="a4e84-137">To provision a token as part of the sign-in flow, add the scope to the default access token scopes of the `MsalProviderOptions`:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultAccessTokenScopes.Add(
        "{API ID URI}/{SCOPE}");
});
```

## <a name="index-page"></a><span data-ttu-id="a4e84-138">Dizin sayfası</span><span class="sxs-lookup"><span data-stu-id="a4e84-138">Index page</span></span>

[!INCLUDE[](~/includes/blazor-security/index-page.md)]

## <a name="app-component"></a><span data-ttu-id="a4e84-139">Uygulama bileşeni</span><span class="sxs-lookup"><span data-stu-id="a4e84-139">App component</span></span>

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

## <a name="redirecttologin-component"></a><span data-ttu-id="a4e84-140">RedirectToLogin bileşeni</span><span class="sxs-lookup"><span data-stu-id="a4e84-140">RedirectToLogin component</span></span>

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

## <a name="logindisplay-component"></a><span data-ttu-id="a4e84-141">LoginDisplay bileşeni</span><span class="sxs-lookup"><span data-stu-id="a4e84-141">LoginDisplay component</span></span>

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

## <a name="authentication-component"></a><span data-ttu-id="a4e84-142">Kimlik doğrulama bileşeni</span><span class="sxs-lookup"><span data-stu-id="a4e84-142">Authentication component</span></span>

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a><span data-ttu-id="a4e84-143">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="a4e84-143">Additional resources</span></span>

* <xref:security/authentication/azure-ad-b2c>
* [<span data-ttu-id="a4e84-144">Öğretici: Azure Active Directory B2C kiracısı oluşturma</span><span class="sxs-lookup"><span data-stu-id="a4e84-144">Tutorial: Create an Azure Active Directory B2C tenant</span></span>](/azure/active-directory-b2c/tutorial-create-tenant)
