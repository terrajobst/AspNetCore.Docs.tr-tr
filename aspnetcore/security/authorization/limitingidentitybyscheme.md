---
title: ASP.NET Core belirli bir şemayla yetkilendir
author: rick-anderson
description: Bu makalede, birden çok kimlik doğrulama yöntemleriyle çalışırken kimliğin belirli bir şemayla nasıl sınırlandırılacağını açıklamaktadır.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 11/08/2019
uid: security/authorization/limitingidentitybyscheme
ms.openlocfilehash: 38da80519b9d5d097c24d38b5a37503174629fc4
ms.sourcegitcommit: 4818385c3cfe0805e15138a2c1785b62deeaab90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/09/2019
ms.locfileid: "73896962"
---
# <a name="authorize-with-a-specific-scheme-in-aspnet-core"></a><span data-ttu-id="63809-103">ASP.NET Core belirli bir şemayla yetkilendir</span><span class="sxs-lookup"><span data-stu-id="63809-103">Authorize with a specific scheme in ASP.NET Core</span></span>

<span data-ttu-id="63809-104">Tek sayfalı uygulamalar (maça 'Lar) gibi bazı senaryolarda, birden çok kimlik doğrulama yöntemi kullanılması yaygındır.</span><span class="sxs-lookup"><span data-stu-id="63809-104">In some scenarios, such as Single Page Applications (SPAs), it's common to use multiple authentication methods.</span></span> <span data-ttu-id="63809-105">Örneğin, uygulama, JavaScript istekleri için oturum açma ve JWT taşıyıcı kimlik doğrulaması için tanımlama bilgisi tabanlı kimlik doğrulaması kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="63809-105">For example, the app may use cookie-based authentication to log in and JWT bearer authentication for JavaScript requests.</span></span> <span data-ttu-id="63809-106">Bazı durumlarda, uygulamanın bir kimlik doğrulama işleyicisinin birden çok örneği olabilir.</span><span class="sxs-lookup"><span data-stu-id="63809-106">In some cases, the app may have multiple instances of an authentication handler.</span></span> <span data-ttu-id="63809-107">Örneğin, biri temel kimlik içeren ve bir Multi-Factor Authentication (MFA) tetiklendiğinde bir tane olan iki tanımlama bilgisi işleyicisi oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="63809-107">For example, two cookie handlers where one contains a basic identity and one is created when a multi-factor authentication (MFA) has been triggered.</span></span> <span data-ttu-id="63809-108">Kullanıcı ek güvenlik gerektiren bir işlem istediği için MFA tetiklenebilir.</span><span class="sxs-lookup"><span data-stu-id="63809-108">MFA may be triggered because the user requested an operation that requires extra security.</span></span>

<span data-ttu-id="63809-109">Kimlik doğrulaması sırasında kimlik doğrulama hizmeti yapılandırıldığında bir kimlik doğrulama düzeni adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="63809-109">An authentication scheme is named when the authentication service is configured during authentication.</span></span> <span data-ttu-id="63809-110">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="63809-110">For example:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Code omitted for brevity

    services.AddAuthentication()
        .AddCookie(options => {
            options.LoginPath = "/Account/Unauthorized/";
            options.AccessDeniedPath = "/Account/Forbidden/";
        })
        .AddJwtBearer(options => {
            options.Audience = "http://localhost:5001/";
            options.Authority = "http://localhost:5000/";
        });
```

<span data-ttu-id="63809-111">Yukarıdaki kodda iki kimlik doğrulama işleyicisi eklenmiştir: biri tanımlama bilgileri ve bir taşıyıcı için bir tane.</span><span class="sxs-lookup"><span data-stu-id="63809-111">In the preceding code, two authentication handlers have been added: one for cookies and one for bearer.</span></span>

>[!NOTE]
><span data-ttu-id="63809-112">Varsayılan düzeni belirtmek `HttpContext.User` özelliğinin bu kimliğe ayarlandığı sonuçları elde ediyor.</span><span class="sxs-lookup"><span data-stu-id="63809-112">Specifying the default scheme results in the `HttpContext.User` property being set to that identity.</span></span> <span data-ttu-id="63809-113">Bu davranış istenmiyorsa, `AddAuthentication`parametresiz biçimini çağırarak devre dışı bırakın.</span><span class="sxs-lookup"><span data-stu-id="63809-113">If that behavior isn't desired, disable it by invoking the parameterless form of `AddAuthentication`.</span></span>

## <a name="selecting-the-scheme-with-the-authorize-attribute"></a><span data-ttu-id="63809-114">Yetkilendir özniteliğiyle düzeni seçme</span><span class="sxs-lookup"><span data-stu-id="63809-114">Selecting the scheme with the Authorize attribute</span></span>

<span data-ttu-id="63809-115">Uygulama, yetkilendirme noktasında kullanılacak işleyiciyi gösterir.</span><span class="sxs-lookup"><span data-stu-id="63809-115">At the point of authorization, the app indicates the handler to be used.</span></span> <span data-ttu-id="63809-116">`[Authorize]`için virgülle ayrılmış bir kimlik doğrulama düzeni listesi geçirerek uygulamanın yetkilendirdiği işleyiciyi seçin.</span><span class="sxs-lookup"><span data-stu-id="63809-116">Select the handler with which the app will authorize by passing a comma-delimited list of authentication schemes to `[Authorize]`.</span></span> <span data-ttu-id="63809-117">`[Authorize]` özniteliği, varsayılan olarak yapılandırılıp yapılandırılmadığını ne olursa olsun, kullanılacak kimlik doğrulama düzenini veya düzenlerini belirtir.</span><span class="sxs-lookup"><span data-stu-id="63809-117">The `[Authorize]` attribute specifies the authentication scheme or schemes to use regardless of whether a default is configured.</span></span> <span data-ttu-id="63809-118">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="63809-118">For example:</span></span>

```csharp
[Authorize(AuthenticationSchemes = AuthSchemes)]
public class MixedController : Controller
    // Requires the following imports:
    // using Microsoft.AspNetCore.Authentication.Cookies;
    // using Microsoft.AspNetCore.Authentication.JwtBearer;
    private const string AuthSchemes =
        CookieAuthenticationDefaults.AuthenticationScheme + "," +
        JwtBearerDefaults.AuthenticationScheme;
```

<span data-ttu-id="63809-119">Yukarıdaki örnekte, hem tanımlama bilgisi hem de taşıyıcı işleyiciler çalışır ve geçerli kullanıcı için bir kimlik oluşturma ve ekleme şansı vardır.</span><span class="sxs-lookup"><span data-stu-id="63809-119">In the preceding example, both the cookie and bearer handlers run and have a chance to create and append an identity for the current user.</span></span> <span data-ttu-id="63809-120">Yalnızca tek bir düzen belirterek, karşılık gelen işleyici çalışır.</span><span class="sxs-lookup"><span data-stu-id="63809-120">By specifying a single scheme only, the corresponding handler runs.</span></span>

```csharp
[Authorize(AuthenticationSchemes = 
    JwtBearerDefaults.AuthenticationScheme)]
public class MixedController : Controller
```

<span data-ttu-id="63809-121">Yukarıdaki kodda yalnızca "taşıyıcı" düzenine sahip işleyici çalışır.</span><span class="sxs-lookup"><span data-stu-id="63809-121">In the preceding code, only the handler with the "Bearer" scheme runs.</span></span> <span data-ttu-id="63809-122">Tanımlama bilgisi tabanlı kimlikler yok sayılır.</span><span class="sxs-lookup"><span data-stu-id="63809-122">Any cookie-based identities are ignored.</span></span>

## <a name="selecting-the-scheme-with-policies"></a><span data-ttu-id="63809-123">İlkeleri olan düzeni seçme</span><span class="sxs-lookup"><span data-stu-id="63809-123">Selecting the scheme with policies</span></span>

<span data-ttu-id="63809-124">[İlkede](xref:security/authorization/policies)istenen şemaları belirtmeyi tercih ediyorsanız, ilkenizi eklerken `AuthenticationSchemes` koleksiyonu ayarlayabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="63809-124">If you prefer to specify the desired schemes in [policy](xref:security/authorization/policies), you can set the `AuthenticationSchemes` collection when adding your policy:</span></span>

```csharp
services.AddAuthorization(options =>
{
    options.AddPolicy("Over18", policy =>
    {
        policy.AuthenticationSchemes.Add(JwtBearerDefaults.AuthenticationScheme);
        policy.RequireAuthenticatedUser();
        policy.Requirements.Add(new MinimumAgeRequirement());
    });
});
```

<span data-ttu-id="63809-125">Yukarıdaki örnekte, "Over18" ilkesi yalnızca "taşıyıcı" işleyicisi tarafından oluşturulan kimliğe göre çalışır.</span><span class="sxs-lookup"><span data-stu-id="63809-125">In the preceding example, the "Over18" policy only runs against the identity created by the "Bearer" handler.</span></span> <span data-ttu-id="63809-126">`[Authorize]` özniteliğinin `Policy` özelliğini ayarlayarak ilkeyi kullanın:</span><span class="sxs-lookup"><span data-stu-id="63809-126">Use the policy by setting the `[Authorize]` attribute's `Policy` property:</span></span>

```csharp
[Authorize(Policy = "Over18")]
public class RegistrationController : Controller
```

::: moniker range=">= aspnetcore-2.0"

## <a name="use-multiple-authentication-schemes"></a><span data-ttu-id="63809-127">Birden çok kimlik doğrulama şeması kullanma</span><span class="sxs-lookup"><span data-stu-id="63809-127">Use multiple authentication schemes</span></span>

<span data-ttu-id="63809-128">Bazı uygulamaların birden çok tür kimlik doğrulamasını desteklemesi gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="63809-128">Some apps may need to support multiple types of authentication.</span></span> <span data-ttu-id="63809-129">Örneğin, uygulamanız Azure Active Directory kullanıcıların kimliğini ve bir kullanıcılar veritabanından kimlik doğrulaması yapabilir.</span><span class="sxs-lookup"><span data-stu-id="63809-129">For example, your app might authenticate users from Azure Active Directory and from a users database.</span></span> <span data-ttu-id="63809-130">Diğer bir örnek, hem Active Directory Federasyon Hizmetleri (AD FS) hem de Azure Active Directory B2C kullanıcıların kimliğini doğrulayan bir uygulamadır.</span><span class="sxs-lookup"><span data-stu-id="63809-130">Another example is an app that authenticates users from both Active Directory Federation Services and Azure Active Directory B2C.</span></span> <span data-ttu-id="63809-131">Bu durumda, uygulamanın birkaç verenler tarafından bir JWT taşıyıcı belirtecini kabul etmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="63809-131">In this case, the app should accept a JWT bearer token from several issuers.</span></span>

<span data-ttu-id="63809-132">Kabul etmek istediğiniz tüm kimlik doğrulama düzenlerini ekleyin.</span><span class="sxs-lookup"><span data-stu-id="63809-132">Add all authentication schemes you'd like to accept.</span></span> <span data-ttu-id="63809-133">Örneğin, `Startup.ConfigureServices` aşağıdaki kod farklı verenler ile iki JWT taşıyıcı kimlik doğrulama şeması ekler:</span><span class="sxs-lookup"><span data-stu-id="63809-133">For example, the following code in `Startup.ConfigureServices` adds two JWT bearer authentication schemes with different issuers:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Code omitted for brevity

    services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
        .AddJwtBearer(options =>
        {
            options.Audience = "https://localhost:5000/";
            options.Authority = "https://localhost:5000/identity/";
        })
        .AddJwtBearer("AzureAD", options =>
        {
            options.Audience = "https://localhost:5000/";
            options.Authority = "https://login.microsoftonline.com/eb971100-6f99-4bdc-8611-1bc8edd7f436/";
        });
}
```

> [!NOTE]
> <span data-ttu-id="63809-134">Varsayılan kimlik doğrulama düzenine `JwtBearerDefaults.AuthenticationScheme`yalnızca bir JWT taşıyıcı kimlik doğrulaması kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="63809-134">Only one JWT bearer authentication is registered with the default authentication scheme `JwtBearerDefaults.AuthenticationScheme`.</span></span> <span data-ttu-id="63809-135">Ek kimlik doğrulaması, benzersiz bir kimlik doğrulama şemasına kaydedilmelidir.</span><span class="sxs-lookup"><span data-stu-id="63809-135">Additional authentication has to be registered with a unique authentication scheme.</span></span>

<span data-ttu-id="63809-136">Bir sonraki adım, varsayılan yetkilendirme ilkesini her iki kimlik doğrulama şemasını kabul edecek şekilde güncelleştirmesidir.</span><span class="sxs-lookup"><span data-stu-id="63809-136">The next step is to update the default authorization policy to accept both authentication schemes.</span></span> <span data-ttu-id="63809-137">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="63809-137">For example:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Code omitted for brevity

    services.AddAuthorization(options =>
    {
        var defaultAuthorizationPolicyBuilder = new AuthorizationPolicyBuilder(
            JwtBearerDefaults.AuthenticationScheme,
            "AzureAD");
        defaultAuthorizationPolicyBuilder = 
            defaultAuthorizationPolicyBuilder.RequireAuthenticatedUser();
        options.DefaultPolicy = defaultAuthorizationPolicyBuilder.Build();
    });
}
```

<span data-ttu-id="63809-138">Varsayılan yetkilendirme ilkesi geçersiz kılındığından, denetleyicilerde `[Authorize]` özniteliğini kullanmak mümkündür.</span><span class="sxs-lookup"><span data-stu-id="63809-138">As the default authorization policy is overridden, it's possible to use the `[Authorize]` attribute in controllers.</span></span> <span data-ttu-id="63809-139">Denetleyici daha sonra ilk veya ikinci veren tarafından JWT veren istekleri kabul eder.</span><span class="sxs-lookup"><span data-stu-id="63809-139">The controller then accepts requests with JWT issued by the first or second issuer.</span></span>

::: moniker-end
