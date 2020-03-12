---
title: Kimlik doğrulama kitaplığıyla bir ASP.NET Core Blazor WebAssembly tek başına uygulamasını güvenli hale getirme
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/09/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/standalone-with-authentication-library
ms.openlocfilehash: f9cc2884dcd94c729c45a056ae4327a2c75d34be
ms.sourcegitcommit: 98bcf5fe210931e3eb70f82fd675d8679b33f5d6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/11/2020
ms.locfileid: "79083757"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-standalone-app-with-the-authentication-library"></a><span data-ttu-id="feb79-102">Kimlik doğrulama kitaplığıyla bir ASP.NET Core Blazor WebAssembly tek başına uygulamasını güvenli hale getirme</span><span class="sxs-lookup"><span data-stu-id="feb79-102">Secure an ASP.NET Core Blazor WebAssembly standalone app with the Authentication library</span></span>

<span data-ttu-id="feb79-103">, [Javier Calvarro Nelson](https://github.com/javiercn) ve [Luke Latham](https://github.com/guardrex) 'e göre</span><span class="sxs-lookup"><span data-stu-id="feb79-103">By [Javier Calvarro Nelson](https://github.com/javiercn) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

<span data-ttu-id="feb79-104">`Microsoft.AspNetCore.Components.WebAssembly.Authentication` kitaplığı kullanan bir Blazor WebAssembly tek başına uygulaması oluşturmak için, komut kabuğu 'nda aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="feb79-104">To create a Blazor WebAssembly standalone app that uses `Microsoft.AspNetCore.Components.WebAssembly.Authentication` library, execute the following command in a command shell:</span></span>

```dotnetcli
dotnet new blazorwasm -au Individual
```

<span data-ttu-id="feb79-105">Mevcut değilse bir proje klasörü oluşturan çıkış konumunu belirtmek için, çıkış seçeneğini komuta bir yol (örneğin, `-o BlazorSample`) ile birlikte ekleyin.</span><span class="sxs-lookup"><span data-stu-id="feb79-105">To specify the output location, which creates a project folder if it doesn't exist, include the output option in the command with a path (for example, `-o BlazorSample`).</span></span> <span data-ttu-id="feb79-106">Klasör adı Ayrıca projenin adının bir parçası haline gelir.</span><span class="sxs-lookup"><span data-stu-id="feb79-106">The folder name also becomes part of the project's name.</span></span>

<span data-ttu-id="feb79-107">Visual Studio 'da [bir Blazor WebAssembly uygulaması oluşturun](xref:blazor/get-started).</span><span class="sxs-lookup"><span data-stu-id="feb79-107">In Visual Studio, [create a Blazor WebAssembly app](xref:blazor/get-started).</span></span> <span data-ttu-id="feb79-108">**Uygulama içi kullanıcı hesaplarını depola** seçeneğiyle **bireysel kullanıcı hesapları** için **kimlik doğrulamasını** ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="feb79-108">Set **Authentication** to **Individual User Accounts** with the **Store user accounts in-app** option.</span></span>

## <a name="authentication-package"></a><span data-ttu-id="feb79-109">Kimlik doğrulama paketi</span><span class="sxs-lookup"><span data-stu-id="feb79-109">Authentication package</span></span>

<span data-ttu-id="feb79-110">Tek tek kullanıcı hesaplarını kullanmak üzere bir uygulama oluşturulduğunda, uygulama otomatik olarak uygulamanın proje dosyasındaki `Microsoft.AspNetCore.Components.WebAssembly.Authentication` paketi için bir paket başvurusu alır.</span><span class="sxs-lookup"><span data-stu-id="feb79-110">When an app is created to use Individual User Accounts, the app automatically receives a package reference for the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package in the app's project file.</span></span> <span data-ttu-id="feb79-111">Paket, uygulamanın kullanıcıların kimliğini doğrulamasına ve korunan API 'Leri çağırmak için belirteçleri almasına yardımcı olan bir dizi temel sunar.</span><span class="sxs-lookup"><span data-stu-id="feb79-111">The package provides a set of primitives that help the app authenticate users and obtain tokens to call protected APIs.</span></span>

<span data-ttu-id="feb79-112">Bir uygulamaya kimlik doğrulaması ekliyorsanız, paketi uygulamanın proje dosyasına el ile ekleyin:</span><span class="sxs-lookup"><span data-stu-id="feb79-112">If adding authentication to an app, manually add the package to the app's project file:</span></span>

```xml
<PackageReference 
    Include="Microsoft.AspNetCore.Components.WebAssembly.Authentication" 
    Version="{VERSION}" />
```

<span data-ttu-id="feb79-113">Önceki paket başvurusunda `{VERSION}`, <xref:blazor/get-started> makalesinde gösterilen `Microsoft.AspNetCore.Blazor.Templates` paketinin sürümü ile değiştirin.</span><span class="sxs-lookup"><span data-stu-id="feb79-113">Replace `{VERSION}` in the preceding package reference with the version of the `Microsoft.AspNetCore.Blazor.Templates` package shown in the <xref:blazor/get-started> article.</span></span>

## <a name="authentication-service-support"></a><span data-ttu-id="feb79-114">Kimlik doğrulama hizmeti desteği</span><span class="sxs-lookup"><span data-stu-id="feb79-114">Authentication service support</span></span>

<span data-ttu-id="feb79-115">Kullanıcıları kimlik doğrulama desteği, hizmet kapsayıcısına `Microsoft.AspNetCore.Components.WebAssembly.Authentication` paketi tarafından sağlanmış `AddOidcAuthentication` uzantısı yöntemiyle kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="feb79-115">Support for authenticating users is registered in the service container with the `AddOidcAuthentication` extension method provided by the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package.</span></span> <span data-ttu-id="feb79-116">Bu yöntem, uygulamanın kimlik sağlayıcısıyla (IP) etkileşim kurması için gereken tüm hizmetleri ayarlar.</span><span class="sxs-lookup"><span data-stu-id="feb79-116">This method sets up all of the services required for the app to interact with the Identity Provider (IP).</span></span>

<span data-ttu-id="feb79-117">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="feb79-117">*Program.cs*:</span></span>

```csharp
builder.Services.AddOidcAuthentication(options =>
{
    options.ProviderOptions.Authority = "{AUTHORITY}";
    options.ProviderOptions.ClientId = "{CLIENT ID}";
});
```

<span data-ttu-id="feb79-118">Tek başına uygulamalar için kimlik doğrulama desteği, Open ID Connect (OıDC) kullanılarak sunulur.</span><span class="sxs-lookup"><span data-stu-id="feb79-118">Authentication support for standalone apps is offered using Open ID Connect (OIDC).</span></span> <span data-ttu-id="feb79-119">`AddOidcAuthentication` yöntemi, OıDC kullanarak bir uygulamanın kimliğini doğrulamak için gereken parametreleri yapılandırmak üzere bir geri çağırma işlemini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="feb79-119">The `AddOidcAuthentication` method accepts a callback to configure the parameters required to authenticate an app using OIDC.</span></span> <span data-ttu-id="feb79-120">Uygulamayı yapılandırmak için gereken değerler, Google, Microsoft veya diğer OıDC uyumlu sağlayıcı gibi IP 'den elde edilebilir.</span><span class="sxs-lookup"><span data-stu-id="feb79-120">The values required for configuring the app can be obtained from the IP, such as Google, Microsoft, or other OIDC-compliant provider.</span></span> <span data-ttu-id="feb79-121">Uygulamayı kaydettiğinizde, genellikle çevrimiçi portalında gerçekleşen değerleri alın.</span><span class="sxs-lookup"><span data-stu-id="feb79-121">Obtain the values when you register the app, which typically occurs in their online portal.</span></span>

## <a name="index-page"></a><span data-ttu-id="feb79-122">Dizin sayfası</span><span class="sxs-lookup"><span data-stu-id="feb79-122">Index page</span></span>

[!INCLUDE[](~/includes/blazor-security/index-page.md)]

## <a name="app-component"></a><span data-ttu-id="feb79-123">Uygulama bileşeni</span><span class="sxs-lookup"><span data-stu-id="feb79-123">App component</span></span>

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

## <a name="redirecttologin-component"></a><span data-ttu-id="feb79-124">RedirectToLogin bileşeni</span><span class="sxs-lookup"><span data-stu-id="feb79-124">RedirectToLogin component</span></span>

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

## <a name="logindisplay-component"></a><span data-ttu-id="feb79-125">LoginDisplay bileşeni</span><span class="sxs-lookup"><span data-stu-id="feb79-125">LoginDisplay component</span></span>

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

## <a name="authentication-component"></a><span data-ttu-id="feb79-126">Kimlik doğrulama bileşeni</span><span class="sxs-lookup"><span data-stu-id="feb79-126">Authentication component</span></span>

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]
