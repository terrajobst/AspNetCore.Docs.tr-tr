---
title: "Talep tabanlı yetkilendirme"
author: rick-anderson
description: "Bu belge, yetkilendirme talep denetler ASP.NET Core uygulama eklemek açıklanmaktadır."
keywords: ASP.NET Core, yetkilendirme talep
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 737be5cd-3511-4f1c-b0ce-65403fb5eed3
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/claims
ms.openlocfilehash: eebaddabdd360f34b6ff44e8f4f9f1f10fda6406
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="claims-based-authorization"></a><span data-ttu-id="6f406-104">Talep tabanlı yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="6f406-104">Claims-Based Authorization</span></span>

<a name="security-authorization-claims-based"></a>

<span data-ttu-id="6f406-105">Kimlikteki oluşturulduğunda, güvenilen bir taraf tarafından verilen bir veya daha fazla talep atanabilir.</span><span class="sxs-lookup"><span data-stu-id="6f406-105">When an identity is created it may be assigned one or more claims issued by a trusted party.</span></span> <span data-ttu-id="6f406-106">Bir talep hangi konu temsil eden çiftidir ad değeri, değil hangi konu yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6f406-106">A claim is name value pair that represents what the subject is, not what the subject can do.</span></span> <span data-ttu-id="6f406-107">Örneğin, bir yerel yönlendirmeli lisans yetkilisi tarafından verilen bir sürücünün lisansına sahip olabilir.</span><span class="sxs-lookup"><span data-stu-id="6f406-107">For example, you may have a driver's license, issued by a local driving license authority.</span></span> <span data-ttu-id="6f406-108">Sürücünüzün lisansı Doğum tarihiniz üzerinde yok.</span><span class="sxs-lookup"><span data-stu-id="6f406-108">Your driver's license has your date of birth on it.</span></span> <span data-ttu-id="6f406-109">Bu durumda talep adı olacaktır `DateOfBirth`, talep değeri, Doğum tarihiniz örneğin olur `8th June 1970` ve veren yönlendirmeli lisans yetkilisi olacaktır.</span><span class="sxs-lookup"><span data-stu-id="6f406-109">In this case the claim name would be `DateOfBirth`, the claim value would be your date of birth, for example `8th June 1970` and the issuer would be the driving license authority.</span></span> <span data-ttu-id="6f406-110">En basit şekliyle, talep tabanlı yetkilendirme, bir talebin değerini denetler ve bu değer temel bir kaynağa erişim izni verir.</span><span class="sxs-lookup"><span data-stu-id="6f406-110">Claims based authorization, at its simplest, checks the value of a claim and allows access to a resource based upon that value.</span></span> <span data-ttu-id="6f406-111">Yetkilendirme işlemi için gece kulübü erişmek isterseniz örnek olabilir için:</span><span class="sxs-lookup"><span data-stu-id="6f406-111">For example if you want access to a night club the authorization process might be:</span></span>

<span data-ttu-id="6f406-112">Kapı güvenlik yetkilisi Doğum talep ve bunlar veren (yönlendirmeli lisans yetkilisi), erişim vermeden önce güvenip tarih değeri değerlendirmek.</span><span class="sxs-lookup"><span data-stu-id="6f406-112">The door security officer would evaluate the value of your date of birth claim and whether they trust the issuer (the driving license authority) before granting you access.</span></span>

<span data-ttu-id="6f406-113">Kimlikteki birden çok değer ile birden fazla talep içerebilir ve aynı türde birden fazla talep içerebilir.</span><span class="sxs-lookup"><span data-stu-id="6f406-113">An identity can contain multiple claims with multiple values and can contain multiple claims of the same type.</span></span>

## <a name="adding-claims-checks"></a><span data-ttu-id="6f406-114">Talep denetimleri ekleme</span><span class="sxs-lookup"><span data-stu-id="6f406-114">Adding claims checks</span></span>

<span data-ttu-id="6f406-115">Talep tabanlı yetkilendirme denetimleri bildirim temelli - Geliştirici bunları kendi kodundaki bir denetleyici veya eylemin bir denetleyici içinde karşı geçerli kullanıcının sahip olması gerekir ve isteğe bağlı olarak değeri talep erişimi taşıması gerekir taleplerini belirtme eklemeler İstenen kaynak.</span><span class="sxs-lookup"><span data-stu-id="6f406-115">Claim based authorization checks are declarative - the developer embeds them within their code, against a controller or an action within a controller, specifying claims which the current user must possess, and optionally the value the claim must hold to access the requested resource.</span></span> <span data-ttu-id="6f406-116">Talepler, ilke tabanlı gereksinimleridir Geliştirici oluşturma ve talep gereksinimleri ifade bir ilkeyi kaydetmek gerekir.</span><span class="sxs-lookup"><span data-stu-id="6f406-116">Claims requirements are policy based, the developer must build and register a policy expressing the claims requirements.</span></span>

<span data-ttu-id="6f406-117">En basit türünü bir talep varlığını ilke arar talep ve değeri denetlemez.</span><span class="sxs-lookup"><span data-stu-id="6f406-117">The simplest type of claim policy looks for the presence of a claim and does not check the value.</span></span>

<span data-ttu-id="6f406-118">İlk yapı ve ilke kaydetmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="6f406-118">First you need to build and register the policy.</span></span> <span data-ttu-id="6f406-119">Bu bölümü normalde alan içinde yetkilendirme hizmet yapılandırmasının parçası olarak gerçekleşir `ConfigureServices()` içinde *haline* dosya.</span><span class="sxs-lookup"><span data-stu-id="6f406-119">This takes place as part of the Authorization service configuration, which normally takes part in `ConfigureServices()` in your *Startup.cs* file.</span></span>

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

<span data-ttu-id="6f406-120">Bu durumda `EmployeeOnly` ilke varlığını denetler bir `EmployeeNumber` geçerli kimliğini talep.</span><span class="sxs-lookup"><span data-stu-id="6f406-120">In this case the `EmployeeOnly` policy checks for the presence of an `EmployeeNumber` claim on the current identity.</span></span>

<span data-ttu-id="6f406-121">Ardından İlkesi kullanarak uygulama `Policy` özellikte `AuthorizeAttribute` ilke adı; belirtmek için özniteliği</span><span class="sxs-lookup"><span data-stu-id="6f406-121">You then apply the policy using the `Policy` property on the `AuthorizeAttribute` attribute to specify the policy name;</span></span>

```csharp
[Authorize(Policy = "EmployeeOnly")]
public IActionResult VacationBalance()
{
    return View();
}
```

<span data-ttu-id="6f406-122">`AuthorizeAttribute` Özniteliği bu örnekte bir tüm denetleyicisine denetleyicisinde herhangi bir eylem erişimin ilkeyle eşleşen kimlikler izin verilecek yalnızca uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="6f406-122">The `AuthorizeAttribute` attribute can be applied to an entire controller, in this instance only identities matching the policy will be allowed access to any Action on the controller.</span></span>

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class VacationController : Controller
{
    public ActionResult VacationBalance()
    {
    }
}
```

<span data-ttu-id="6f406-123">Tarafından korunan bir denetleyiciniz varsa `AuthorizeAttribute` özniteliği, ancak uyguladığınız belirli eylemlere anonim erişime izin vermek istediğiniz `AllowAnonymousAttribute` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="6f406-123">If you have a controller that is protected by the `AuthorizeAttribute` attribute, but want to allow anonymous access to particular actions you apply the `AllowAnonymousAttribute` attribute.</span></span>

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

<span data-ttu-id="6f406-124">Çoğu talep değeri ile gelir.</span><span class="sxs-lookup"><span data-stu-id="6f406-124">Most claims come with a value.</span></span> <span data-ttu-id="6f406-125">İlke oluştururken, izin verilen değerler listesini belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6f406-125">You can specify a list of allowed values when creating the policy.</span></span> <span data-ttu-id="6f406-126">Aşağıdaki örnek 1, 2, 3, 4 veya 5 çalışan sayıda olan çalışanlar için yalnızca başarılı.</span><span class="sxs-lookup"><span data-stu-id="6f406-126">The following example would only succeed for employees whose employee number was 1, 2, 3, 4 or 5.</span></span>

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

## <a name="multiple-policy-evaluation"></a><span data-ttu-id="6f406-127">Birden çok ilke değerlendirmesi</span><span class="sxs-lookup"><span data-stu-id="6f406-127">Multiple Policy Evaluation</span></span>

<span data-ttu-id="6f406-128">Denetleyici veya eylem için birden çok ilke uygularsanız, erişim verilmeden önce tüm ilkeler geçmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="6f406-128">If you apply multiple policies to a controller or action, then all policies must pass before access is granted.</span></span> <span data-ttu-id="6f406-129">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="6f406-129">For example:</span></span>

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

<span data-ttu-id="6f406-130">Yukarıdaki örnekte karşılayan tüm kimlik `EmployeeOnly` İlkesi erişebilir `Payslip` eylemi olarak bu ilkeyi denetleyicisinde zorlanır.</span><span class="sxs-lookup"><span data-stu-id="6f406-130">In the above example any identity which fulfills the `EmployeeOnly` policy can access the `Payslip` action as that policy is enforced on the controller.</span></span> <span data-ttu-id="6f406-131">Ancak çağırmak için `UpdateSalary` gerekir kimliğini karşılamak eylem *her ikisi de* `EmployeeOnly` İlkesi ve `HumanResources` ilkesi.</span><span class="sxs-lookup"><span data-stu-id="6f406-131">However in order to call the `UpdateSalary` action the identity must fulfill *both* the `EmployeeOnly` policy and the `HumanResources` policy.</span></span>

<span data-ttu-id="6f406-132">Daha karmaşık ilkeleri isterseniz, Doğum talep tarihi alma gibi bir yaş dışarı hesaplama sonra geçerlilik süresi denetimi 21 ya da eski sonra yazmanız gereken [özel ilke işleyicileri](policies.md).</span><span class="sxs-lookup"><span data-stu-id="6f406-132">If you want more complicated policies, such as taking a date of birth claim, calculating an age from it then checking the age is 21 or older then you need to write [custom policy handlers](policies.md).</span></span>
