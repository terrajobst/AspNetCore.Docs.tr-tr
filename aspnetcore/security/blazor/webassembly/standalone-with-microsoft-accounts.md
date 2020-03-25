---
title: Microsoft hesaplarıyla bir ASP.NET Core Blazor WebAssembly tek başına uygulamasını güvenli hale getirme
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/09/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/standalone-with-microsoft-accounts
ms.openlocfilehash: be73bec971f96bd64afc735a1ea750d47c7bc383
ms.sourcegitcommit: 91dc1dd3d055b4c7d7298420927b3fd161067c64
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80219265"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-standalone-app-with-microsoft-accounts"></a><span data-ttu-id="64c03-102">Microsoft hesaplarıyla bir ASP.NET Core Blazor WebAssembly tek başına uygulamasını güvenli hale getirme</span><span class="sxs-lookup"><span data-stu-id="64c03-102">Secure an ASP.NET Core Blazor WebAssembly standalone app with Microsoft Accounts</span></span>

<span data-ttu-id="64c03-103">, [Javier Calvarro Nelson](https://github.com/javiercn) ve [Luke Latham](https://github.com/guardrex) 'e göre</span><span class="sxs-lookup"><span data-stu-id="64c03-103">By [Javier Calvarro Nelson](https://github.com/javiercn) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

<span data-ttu-id="64c03-104">Kimlik doğrulaması için [Azure Active Directory (AAD) Ile Microsoft hesapları](/azure/active-directory/develop/quickstart-register-app#register-a-new-application-using-the-azure-portal) kullanan bir Blazor WebAssembly tek başına uygulaması oluşturmak için:</span><span class="sxs-lookup"><span data-stu-id="64c03-104">To create a Blazor WebAssembly standalone app that uses [Microsoft Accounts with Azure Active Directory (AAD)](/azure/active-directory/develop/quickstart-register-app#register-a-new-application-using-the-azure-portal) for authentication:</span></span>

1. [<span data-ttu-id="64c03-105">AAD kiracısı ve Web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="64c03-105">Create an AAD tenant and web application</span></span>](/azure/active-directory/develop/v2-overview)

   <span data-ttu-id="64c03-106">Azure portal **Azure Active Directory** > **uygulama KAYıTLARı** alanına bir AAD uygulaması kaydedin:</span><span class="sxs-lookup"><span data-stu-id="64c03-106">Register a AAD app in the **Azure Active Directory** > **App registrations** area of the Azure portal:</span></span>

   <span data-ttu-id="64c03-107">1 \.</span><span class="sxs-lookup"><span data-stu-id="64c03-107">1\.</span></span> <span data-ttu-id="64c03-108">Uygulama için bir **ad** sağlayın (örneğin, **Blazor istemci AAD**).</span><span class="sxs-lookup"><span data-stu-id="64c03-108">Provide a **Name** for the app (for example, **Blazor Client AAD**).</span></span><br>
   <span data-ttu-id="64c03-109">2 \.</span><span class="sxs-lookup"><span data-stu-id="64c03-109">2\.</span></span> <span data-ttu-id="64c03-110">**Desteklenen hesap türleri**' nde, **herhangi bir kuruluş dizininde hesaplar**' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="64c03-110">In **Supported account types**, select **Accounts in any organizational directory**.</span></span><br>
   <span data-ttu-id="64c03-111">3 \.</span><span class="sxs-lookup"><span data-stu-id="64c03-111">3\.</span></span> <span data-ttu-id="64c03-112">**Yeniden yönlendirme URI 'si** açılan listesini **Web**olarak ayarlayın ve `https://localhost:5001/authentication/login-callback`yeniden yönlendirme URI 'si sağlayın.</span><span class="sxs-lookup"><span data-stu-id="64c03-112">Leave the **Redirect URI** drop down set to **Web**, and provide a redirect URI of `https://localhost:5001/authentication/login-callback`.</span></span><br>
   <span data-ttu-id="64c03-113">4 \.</span><span class="sxs-lookup"><span data-stu-id="64c03-113">4\.</span></span> <span data-ttu-id="64c03-114">**Yönetici tarafından OpenID ve offline_access izinleri için Izin ver** onay kutusunu > **izinleri** devre dışı bırakın.</span><span class="sxs-lookup"><span data-stu-id="64c03-114">Disable the **Permissions** > **Grant admin concent to openid and offline_access permissions** check box.</span></span><br>
   <span data-ttu-id="64c03-115">5 \.</span><span class="sxs-lookup"><span data-stu-id="64c03-115">5\.</span></span> <span data-ttu-id="64c03-116">**Kaydol**’u seçin.</span><span class="sxs-lookup"><span data-stu-id="64c03-116">Select **Register**.</span></span>

   <span data-ttu-id="64c03-117">**Kimlik doğrulama** > **Platform yapılandırmalarında** **Web** > :</span><span class="sxs-lookup"><span data-stu-id="64c03-117">In **Authentication** > **Platform configurations** > **Web**:</span></span>

   <span data-ttu-id="64c03-118">1 \.</span><span class="sxs-lookup"><span data-stu-id="64c03-118">1\.</span></span> <span data-ttu-id="64c03-119">`https://localhost:5001/authentication/login-callback` **yeniden yönlendirme URI 'sinin** mevcut olduğunu onaylayın.</span><span class="sxs-lookup"><span data-stu-id="64c03-119">Confirm the **Redirect URI** of `https://localhost:5001/authentication/login-callback` is present.</span></span><br>
   <span data-ttu-id="64c03-120">2 \.</span><span class="sxs-lookup"><span data-stu-id="64c03-120">2\.</span></span> <span data-ttu-id="64c03-121">**Örtük izin**Için, **erişim belirteçleri** ve **Kimlik belirteçleri**onay kutularını seçin.</span><span class="sxs-lookup"><span data-stu-id="64c03-121">For **Implicit grant**, select the check boxes for **Access tokens** and **ID tokens**.</span></span><br>
   <span data-ttu-id="64c03-122">3 \.</span><span class="sxs-lookup"><span data-stu-id="64c03-122">3\.</span></span> <span data-ttu-id="64c03-123">Uygulamanın kalan varsayılan değerleri bu deneyim için kabul edilebilir.</span><span class="sxs-lookup"><span data-stu-id="64c03-123">The remaining defaults for the app are acceptable for this experience.</span></span><br>
   <span data-ttu-id="64c03-124">4 \.</span><span class="sxs-lookup"><span data-stu-id="64c03-124">4\.</span></span> <span data-ttu-id="64c03-125">**Kaydet** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="64c03-125">Select the **Save** button.</span></span>

   <span data-ttu-id="64c03-126">Uygulama KIMLIĞINI (Istemci KIMLIĞI) kaydedin (örneğin, `11111111-1111-1111-1111-111111111111`).</span><span class="sxs-lookup"><span data-stu-id="64c03-126">Record the Application ID (Client ID) (for example, `11111111-1111-1111-1111-111111111111`).</span></span>

1. <span data-ttu-id="64c03-127">Aşağıdaki komutta yer tutucuları, daha önce kaydedilen bilgilerle değiştirin ve komutu bir komut kabuğu 'nda yürütün:</span><span class="sxs-lookup"><span data-stu-id="64c03-127">Replace the placeholders in the following command with the information recorded earlier and execute the command in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -au SingleOrg --client-id "{CLIENT ID}" --tenant-id "common"
   ```

   <span data-ttu-id="64c03-128">Mevcut değilse bir proje klasörü oluşturan çıkış konumunu belirtmek için, çıkış seçeneğini komuta bir yol (örneğin, `-o BlazorSample`) ile birlikte ekleyin.</span><span class="sxs-lookup"><span data-stu-id="64c03-128">To specify the output location, which creates a project folder if it doesn't exist, include the output option in the command with a path (for example, `-o BlazorSample`).</span></span> <span data-ttu-id="64c03-129">Klasör adı Ayrıca projenin adının bir parçası haline gelir.</span><span class="sxs-lookup"><span data-stu-id="64c03-129">The folder name also becomes part of the project's name.</span></span>

<span data-ttu-id="64c03-130">Uygulamayı oluşturduktan sonra şunları yapmanız gerekir:</span><span class="sxs-lookup"><span data-stu-id="64c03-130">After creating the app, you should be able to:</span></span>

* <span data-ttu-id="64c03-131">Bir Microsoft hesabı kullanarak uygulamada oturum açın.</span><span class="sxs-lookup"><span data-stu-id="64c03-131">Log into the app using a Microsoft Account.</span></span>
* <span data-ttu-id="64c03-132">Uygulamayı doğru şekilde yapılandırdığınıza yönelik tek başına Blazor uygulamalarla aynı yaklaşımı kullanarak Microsoft API 'Leri için erişim belirteçleri isteyin.</span><span class="sxs-lookup"><span data-stu-id="64c03-132">Request access tokens for Microsoft APIs using the same approach as for standalone Blazor apps provided that you have configured the app correctly.</span></span> <span data-ttu-id="64c03-133">Daha fazla bilgi için bkz. [hızlı başlangıç: Web API 'leri göstermek için uygulama yapılandırma](/azure/active-directory/develop/quickstart-configure-app-expose-web-apis).</span><span class="sxs-lookup"><span data-stu-id="64c03-133">For more information, see [Quickstart: Configure an application to expose web APIs](/azure/active-directory/develop/quickstart-configure-app-expose-web-apis).</span></span>

## <a name="authentication-package"></a><span data-ttu-id="64c03-134">Kimlik doğrulama paketi</span><span class="sxs-lookup"><span data-stu-id="64c03-134">Authentication package</span></span>

<span data-ttu-id="64c03-135">Iş veya okul hesaplarını (`SingleOrg`) kullanmak üzere bir uygulama oluşturulduğunda, uygulama [Microsoft kimlik doğrulama kitaplığı](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`) için bir paket başvurusu otomatik olarak alır.</span><span class="sxs-lookup"><span data-stu-id="64c03-135">When an app is created to use Work or School Accounts (`SingleOrg`), the app automatically receives a package reference for the [Microsoft Authentication Library](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`).</span></span> <span data-ttu-id="64c03-136">Paket, uygulamanın kullanıcıların kimliğini doğrulamasına ve korunan API 'Leri çağırmak için belirteçleri almasına yardımcı olan bir dizi temel sunar.</span><span class="sxs-lookup"><span data-stu-id="64c03-136">The package provides a set of primitives that help the app authenticate users and obtain tokens to call protected APIs.</span></span>

<span data-ttu-id="64c03-137">Bir uygulamaya kimlik doğrulaması ekliyorsanız, paketi uygulamanın proje dosyasına el ile ekleyin:</span><span class="sxs-lookup"><span data-stu-id="64c03-137">If adding authentication to an app, manually add the package to the app's project file:</span></span>

```xml
<PackageReference Include="Microsoft.Authentication.WebAssembly.Msal" 
    Version="{VERSION}" />
```

<span data-ttu-id="64c03-138">Önceki paket başvurusunda `{VERSION}`, <xref:blazor/get-started> makalesinde gösterilen `Microsoft.AspNetCore.Blazor.Templates` paketinin sürümü ile değiştirin.</span><span class="sxs-lookup"><span data-stu-id="64c03-138">Replace `{VERSION}` in the preceding package reference with the version of the `Microsoft.AspNetCore.Blazor.Templates` package shown in the <xref:blazor/get-started> article.</span></span>

<span data-ttu-id="64c03-139">`Microsoft.Authentication.WebAssembly.Msal` paketi, `Microsoft.AspNetCore.Components.WebAssembly.Authentication` paketini uygulamaya göre geçişli olarak ekler.</span><span class="sxs-lookup"><span data-stu-id="64c03-139">The `Microsoft.Authentication.WebAssembly.Msal` package transitively adds the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package to the app.</span></span>

## <a name="authentication-service-support"></a><span data-ttu-id="64c03-140">Kimlik doğrulama hizmeti desteği</span><span class="sxs-lookup"><span data-stu-id="64c03-140">Authentication service support</span></span>

<span data-ttu-id="64c03-141">Kullanıcıları kimlik doğrulama desteği, hizmet kapsayıcısına `Microsoft.Authentication.WebAssembly.Msal` paketi tarafından sağlanmış `AddMsalAuthentication` uzantısı yöntemiyle kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="64c03-141">Support for authenticating users is registered in the service container with the `AddMsalAuthentication` extension method provided by the `Microsoft.Authentication.WebAssembly.Msal` package.</span></span> <span data-ttu-id="64c03-142">Bu yöntem, uygulamanın kimlik sağlayıcısıyla (IP) etkileşim kurması için gereken tüm hizmetleri ayarlar.</span><span class="sxs-lookup"><span data-stu-id="64c03-142">This method sets up all of the services required for the app to interact with the Identity Provider (IP).</span></span>

<span data-ttu-id="64c03-143">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="64c03-143">*Program.cs*:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    var authentication = options.ProviderOptions.Authentication;
    authentication.Authority = "{AUTHORITY}";
    authentication.ClientId = "{CLIENT ID}";
});
```

<span data-ttu-id="64c03-144">`AddMsalAuthentication` yöntemi, bir uygulamanın kimliğini doğrulamak için gereken parametreleri yapılandırmak için bir geri çağırma işlemini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="64c03-144">The `AddMsalAuthentication` method accepts a callback to configure the parameters required to authenticate an app.</span></span> <span data-ttu-id="64c03-145">Uygulamanın yapılandırılması için gereken değerler, uygulamayı kaydettiğinizde Microsoft hesapları yapılandırmasından elde edilebilir.</span><span class="sxs-lookup"><span data-stu-id="64c03-145">The values required for configuring the app can be obtained from the Microsoft Accounts configuration when you register the app.</span></span>

## <a name="index-page"></a><span data-ttu-id="64c03-146">Dizin sayfası</span><span class="sxs-lookup"><span data-stu-id="64c03-146">Index page</span></span>

[!INCLUDE[](~/includes/blazor-security/index-page-msal.md)]

## <a name="app-component"></a><span data-ttu-id="64c03-147">Uygulama bileşeni</span><span class="sxs-lookup"><span data-stu-id="64c03-147">App component</span></span>

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

## <a name="redirecttologin-component"></a><span data-ttu-id="64c03-148">RedirectToLogin bileşeni</span><span class="sxs-lookup"><span data-stu-id="64c03-148">RedirectToLogin component</span></span>

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

## <a name="logindisplay-component"></a><span data-ttu-id="64c03-149">LoginDisplay bileşeni</span><span class="sxs-lookup"><span data-stu-id="64c03-149">LoginDisplay component</span></span>

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

## <a name="authentication-component"></a><span data-ttu-id="64c03-150">Kimlik doğrulama bileşeni</span><span class="sxs-lookup"><span data-stu-id="64c03-150">Authentication component</span></span>

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a><span data-ttu-id="64c03-151">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="64c03-151">Additional resources</span></span>

* [<span data-ttu-id="64c03-152">Hızlı başlangıç: Microsoft Identity platformu ile uygulama kaydetme</span><span class="sxs-lookup"><span data-stu-id="64c03-152">Quickstart: Register an application with the Microsoft identity platform</span></span>](/azure/active-directory/develop/quickstart-register-app#register-a-new-application-using-the-azure-portal)
* [<span data-ttu-id="64c03-153">Hızlı başlangıç: Web API 'Lerini kullanıma sunmak için uygulama yapılandırma</span><span class="sxs-lookup"><span data-stu-id="64c03-153">Quickstart: Configure an application to expose web APIs</span></span>](/azure/active-directory/develop/quickstart-configure-app-expose-web-apis)
