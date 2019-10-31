---
title: ASP.NET Core 'de talep tabanlı yetkilendirme
author: rick-anderson
description: ASP.NET Core uygulamasında yetkilendirme için talep denetimleri eklemeyi öğrenin.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/claims
ms.openlocfilehash: e289851aafcbc7e3b3f60ab9fbe4b182a78bdf8a
ms.sourcegitcommit: de0fc77487a4d342bcc30965ec5c142d10d22c03
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2019
ms.locfileid: "73143434"
---
# <a name="claims-based-authorization-in-aspnet-core"></a><span data-ttu-id="0ca28-103">ASP.NET Core 'de talep tabanlı yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="0ca28-103">Claims-based authorization in ASP.NET Core</span></span>

<a name="security-authorization-claims-based"></a>

<span data-ttu-id="0ca28-104">Bir kimlik oluşturulduğunda, güvenilen bir taraf tarafından verilen bir veya daha fazla talep atanabilir.</span><span class="sxs-lookup"><span data-stu-id="0ca28-104">When an identity is created it may be assigned one or more claims issued by a trusted party.</span></span> <span data-ttu-id="0ca28-105">Talep, konunun ne yapabileceğini temsil eden bir ad değer çiftidir.</span><span class="sxs-lookup"><span data-stu-id="0ca28-105">A claim is a name value pair that represents what the subject is, not what the subject can do.</span></span> <span data-ttu-id="0ca28-106">Örneğin, bir yerel bir itici lisans yetkilisi tarafından verilen bir sürücü lisansına sahip olabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0ca28-106">For example, you may have a driver's license, issued by a local driving license authority.</span></span> <span data-ttu-id="0ca28-107">Sürücünüzün lisansının, bu tarihte Doğum tarihi vardır.</span><span class="sxs-lookup"><span data-stu-id="0ca28-107">Your driver's license has your date of birth on it.</span></span> <span data-ttu-id="0ca28-108">Bu durumda, talep adı `DateOfBirth`olabilir, örneğin, talep değeri Doğum tarihiniz olur, örneğin `8th June 1970` ve veren lisans yetkilisi olur.</span><span class="sxs-lookup"><span data-stu-id="0ca28-108">In this case the claim name would be `DateOfBirth`, the claim value would be your date of birth, for example `8th June 1970` and the issuer would be the driving license authority.</span></span> <span data-ttu-id="0ca28-109">Talep tabanlı yetkilendirme, en basit, bir talebin değerini denetler ve bu değere göre bir kaynağa erişim sağlar.</span><span class="sxs-lookup"><span data-stu-id="0ca28-109">Claims based authorization, at its simplest, checks the value of a claim and allows access to a resource based upon that value.</span></span> <span data-ttu-id="0ca28-110">Örneğin, gece kulübünün erişim istiyorsanız yetkilendirme süreci şu olabilir:</span><span class="sxs-lookup"><span data-stu-id="0ca28-110">For example if you want access to a night club the authorization process might be:</span></span>

<span data-ttu-id="0ca28-111">Kapılı güvenlik müdürü, Doğum talepinizin tarihini ve erişim izni vermeden önce veren (itici lisans yetkilisi) tarafından güvenip güvenmeyeceğini değerlendirir.</span><span class="sxs-lookup"><span data-stu-id="0ca28-111">The door security officer would evaluate the value of your date of birth claim and whether they trust the issuer (the driving license authority) before granting you access.</span></span>

<span data-ttu-id="0ca28-112">Bir kimlik birden çok değer içeren birden fazla talep içerebilir ve aynı türde birden fazla talep içerebilir.</span><span class="sxs-lookup"><span data-stu-id="0ca28-112">An identity can contain multiple claims with multiple values and can contain multiple claims of the same type.</span></span>

## <a name="adding-claims-checks"></a><span data-ttu-id="0ca28-113">Talep denetimleri ekleme</span><span class="sxs-lookup"><span data-stu-id="0ca28-113">Adding claims checks</span></span>

<span data-ttu-id="0ca28-114">Talep tabanlı yetkilendirme denetimleri bildirime dayalı-geliştirici, bunları kendi kodlarında bir denetleyiciye veya denetleyici içindeki bir eyleme göre, geçerli kullanıcının sahip olması gereken talepleri belirterek ve isteğe bağlı olarak, talebin bu değere erişmesi için tutması gereken değeri istenen kaynak.</span><span class="sxs-lookup"><span data-stu-id="0ca28-114">Claim based authorization checks are declarative - the developer embeds them within their code, against a controller or an action within a controller, specifying claims which the current user must possess, and optionally the value the claim must hold to access the requested resource.</span></span> <span data-ttu-id="0ca28-115">Talep gereksinimleri ilke tabanlıdır, geliştiricilerin talep gereksinimlerini ifade eden bir ilke oluşturması ve kaydetmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="0ca28-115">Claims requirements are policy based, the developer must build and register a policy expressing the claims requirements.</span></span>

<span data-ttu-id="0ca28-116">En basit talep ilkesi türü bir talep olup olmadığını arar ve değeri denetlemez.</span><span class="sxs-lookup"><span data-stu-id="0ca28-116">The simplest type of claim policy looks for the presence of a claim and doesn't check the value.</span></span>

<span data-ttu-id="0ca28-117">Önce ilkeyi oluşturmanız ve kaydetmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="0ca28-117">First you need to build and register the policy.</span></span> <span data-ttu-id="0ca28-118">Bu, normalde *Startup.cs* dosyanızdaki `ConfigureServices()` bir parçasını alan yetkilendirme hizmeti yapılandırmasının bir parçası olarak gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="0ca28-118">This takes place as part of the Authorization service configuration, which normally takes part in `ConfigureServices()` in your *Startup.cs* file.</span></span>

::: moniker range=">= aspnetcore-3.0"

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddControllersWithViews();
    services.AddRazorPages();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("EmployeeOnly", policy => policy.RequireClaim("EmployeeNumber"));
    });
}
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("EmployeeOnly", policy => policy.RequireClaim("EmployeeNumber"));
    });
}
```

::: moniker-end

<span data-ttu-id="0ca28-119">Bu durumda `EmployeeOnly` ilkesi geçerli kimlik üzerinde bir `EmployeeNumber` talebi olup olmadığını denetler.</span><span class="sxs-lookup"><span data-stu-id="0ca28-119">In this case the `EmployeeOnly` policy checks for the presence of an `EmployeeNumber` claim on the current identity.</span></span>

<span data-ttu-id="0ca28-120">İlke adını belirtmek için `AuthorizeAttribute` özniteliğinde `Policy` özelliğini kullanarak ilkeyi uygularsınız;</span><span class="sxs-lookup"><span data-stu-id="0ca28-120">You then apply the policy using the `Policy` property on the `AuthorizeAttribute` attribute to specify the policy name;</span></span>

```csharp
[Authorize(Policy = "EmployeeOnly")]
public IActionResult VacationBalance()
{
    return View();
}
```

<span data-ttu-id="0ca28-121">`AuthorizeAttribute` özniteliği denetleyicinin tamamına uygulanabilir, bu örnekte yalnızca ilkeyle eşleşen kimlikler denetleyicideki herhangi bir eyleme erişime izin verilir.</span><span class="sxs-lookup"><span data-stu-id="0ca28-121">The `AuthorizeAttribute` attribute can be applied to an entire controller, in this instance only identities matching the policy will be allowed access to any Action on the controller.</span></span>

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class VacationController : Controller
{
    public ActionResult VacationBalance()
    {
    }
}
```

<span data-ttu-id="0ca28-122">`AuthorizeAttribute` özniteliğiyle korunan bir denetleyiciniz varsa, ancak belirli eylemlere anonim erişime izin vermek istiyorsanız `AllowAnonymousAttribute` özniteliğini uygularsınız.</span><span class="sxs-lookup"><span data-stu-id="0ca28-122">If you have a controller that's protected by the `AuthorizeAttribute` attribute, but want to allow anonymous access to particular actions you apply the `AllowAnonymousAttribute` attribute.</span></span>

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class VacationController : Controller
{
    public ActionResult VacationBalance()
    {
    }

    [AllowAnonymous]
    public ActionResult VacationPolicy()
    {
    }
}
```

<span data-ttu-id="0ca28-123">Çoğu talep bir değerle gelir.</span><span class="sxs-lookup"><span data-stu-id="0ca28-123">Most claims come with a value.</span></span> <span data-ttu-id="0ca28-124">İlke oluştururken izin verilen değerlerin bir listesini belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0ca28-124">You can specify a list of allowed values when creating the policy.</span></span> <span data-ttu-id="0ca28-125">Aşağıdaki örnek yalnızca çalışan numarası 1, 2, 3, 4 veya 5 olan çalışanlar için başarılı olur.</span><span class="sxs-lookup"><span data-stu-id="0ca28-125">The following example would only succeed for employees whose employee number was 1, 2, 3, 4 or 5.</span></span>

::: moniker range=">= aspnetcore-3.0"

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddControllersWithViews();
    services.AddRazorPages();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("Founders", policy =>
                          policy.RequireClaim("EmployeeNumber", "1", "2", "3", "4", "5"));
    });
}
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("Founders", policy =>
                          policy.RequireClaim("EmployeeNumber", "1", "2", "3", "4", "5"));
    });
}
```

::: moniker-end
### <a name="add-a-generic-claim-check"></a><span data-ttu-id="0ca28-126">Genel talep denetimi ekleme</span><span class="sxs-lookup"><span data-stu-id="0ca28-126">Add a generic claim check</span></span>

<span data-ttu-id="0ca28-127">Talep değeri tek bir değer değilse veya bir dönüşüm gerekliyse, [Requireassertion](/dotnet/api/microsoft.aspnetcore.authorization.authorizationpolicybuilder.requireassertion)kullanın.</span><span class="sxs-lookup"><span data-stu-id="0ca28-127">If the claim value isn't a single value or a transformation is required, use [RequireAssertion](/dotnet/api/microsoft.aspnetcore.authorization.authorizationpolicybuilder.requireassertion).</span></span> <span data-ttu-id="0ca28-128">Daha fazla bilgi için bkz. bir [ilkeyi yerine getirmek için bir Func kullanma](xref:security/authorization/policies#using-a-func-to-fulfill-a-policy).</span><span class="sxs-lookup"><span data-stu-id="0ca28-128">For more information, see [Using a func to fulfill a policy](xref:security/authorization/policies#using-a-func-to-fulfill-a-policy).</span></span>

## <a name="multiple-policy-evaluation"></a><span data-ttu-id="0ca28-129">Birden çok Ilke değerlendirmesi</span><span class="sxs-lookup"><span data-stu-id="0ca28-129">Multiple Policy Evaluation</span></span>

<span data-ttu-id="0ca28-130">Bir denetleyiciye veya eyleme birden çok ilke uygularsanız, erişim verilmeden önce tüm ilkelerin geçmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="0ca28-130">If you apply multiple policies to a controller or action, then all policies must pass before access is granted.</span></span> <span data-ttu-id="0ca28-131">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="0ca28-131">For example:</span></span>

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class SalaryController : Controller
{
    public ActionResult Payslip()
    {
    }

    [Authorize(Policy = "HumanResources")]
    public ActionResult UpdateSalary()
    {
    }
}
```

<span data-ttu-id="0ca28-132">Yukarıdaki örnekte `EmployeeOnly` ilkesini karşılayan herhangi bir kimlik, denetleyicide ilke zorlandığında `Payslip` eyleme erişebilir.</span><span class="sxs-lookup"><span data-stu-id="0ca28-132">In the above example any identity which fulfills the `EmployeeOnly` policy can access the `Payslip` action as that policy is enforced on the controller.</span></span> <span data-ttu-id="0ca28-133">Ancak `UpdateSalary` eylemini çağırmak için kimlik hem `EmployeeOnly` ilkesini hem *de* `HumanResources` ilkesini yerine getirmelidir.</span><span class="sxs-lookup"><span data-stu-id="0ca28-133">However in order to call the `UpdateSalary` action the identity must fulfill *both* the `EmployeeOnly` policy and the `HumanResources` policy.</span></span>

<span data-ttu-id="0ca28-134">Doğum talebinde bulunmak gibi daha karmaşık ilkeler isterseniz, bu tarihten itibaren bir yaşı hesaplamak, daha sonra yaşı 21 veya daha eski bir tarihte denetlemek için [özel ilke işleyicileri](xref:security/authorization/policies)yazmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="0ca28-134">If you want more complicated policies, such as taking a date of birth claim, calculating an age from it then checking the age is 21 or older then you need to write [custom policy handlers](xref:security/authorization/policies).</span></span>
