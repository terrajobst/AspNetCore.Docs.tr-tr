---
title: ASP.NET core'da talep tabanlı yetkilendirme
author: rick-anderson
description: Yetkilendirme talep olup olmadığını denetler ASP.NET Core uygulaması eklemeyi öğrenin.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/claims
ms.openlocfilehash: 6b60ae5515819b017ab577f655ed91ee4d8ed0dd
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65086149"
---
# <a name="claims-based-authorization-in-aspnet-core"></a><span data-ttu-id="ca303-103">ASP.NET core'da talep tabanlı yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="ca303-103">Claims-based authorization in ASP.NET Core</span></span>

<a name="security-authorization-claims-based"></a>

<span data-ttu-id="ca303-104">Bir kimlik oluşturulduğunda güvenilen bir taraf tarafından verilen bir veya daha fazla talep atanabilir.</span><span class="sxs-lookup"><span data-stu-id="ca303-104">When an identity is created it may be assigned one or more claims issued by a trusted party.</span></span> <span data-ttu-id="ca303-105">Bir talep hangi konu temsil eden bir ad değer çifti olan, olmayan hangi konu yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ca303-105">A claim is a name value pair that represents what the subject is, not what the subject can do.</span></span> <span data-ttu-id="ca303-106">Örneğin, yerel bir sürüş lisans yetkilisi tarafından verilen bir sürücünün lisansı olabilir.</span><span class="sxs-lookup"><span data-stu-id="ca303-106">For example, you may have a driver's license, issued by a local driving license authority.</span></span> <span data-ttu-id="ca303-107">Sürücünüzün lisansı Doğum tarihinize üzerinde yok.</span><span class="sxs-lookup"><span data-stu-id="ca303-107">Your driver's license has your date of birth on it.</span></span> <span data-ttu-id="ca303-108">Talep adı bu durumda olacaktır `DateOfBirth`, talep değerini Doğum, örneğin olacaktır `8th June 1970` ve sürüş lisans yetki veren olur.</span><span class="sxs-lookup"><span data-stu-id="ca303-108">In this case the claim name would be `DateOfBirth`, the claim value would be your date of birth, for example `8th June 1970` and the issuer would be the driving license authority.</span></span> <span data-ttu-id="ca303-109">En basit şekliyle, talep tabanlı yetkilendirme, bir talebin değerini denetler ve bu değeri alarak bir kaynağa erişim izni verir.</span><span class="sxs-lookup"><span data-stu-id="ca303-109">Claims based authorization, at its simplest, checks the value of a claim and allows access to a resource based upon that value.</span></span> <span data-ttu-id="ca303-110">Yetkilendirme işlemi için bir gece kulübü erişmek isterseniz örnek olabilir için:</span><span class="sxs-lookup"><span data-stu-id="ca303-110">For example if you want access to a night club the authorization process might be:</span></span>

<span data-ttu-id="ca303-111">Kapı güvenlik Müdürü, tarihini Doğum talep ve olup (sürüş lisans yetkilisi) veren size erişim vermeden önce güvendikleri değerini değerlendirecek.</span><span class="sxs-lookup"><span data-stu-id="ca303-111">The door security officer would evaluate the value of your date of birth claim and whether they trust the issuer (the driving license authority) before granting you access.</span></span>

<span data-ttu-id="ca303-112">Bir kimlik ile birden çok değer birden fazla talep içerebilir ve aynı türde birden fazla talep içerebilir.</span><span class="sxs-lookup"><span data-stu-id="ca303-112">An identity can contain multiple claims with multiple values and can contain multiple claims of the same type.</span></span>

## <a name="adding-claims-checks"></a><span data-ttu-id="ca303-113">Denetimleri talep ekleme</span><span class="sxs-lookup"><span data-stu-id="ca303-113">Adding claims checks</span></span>

<span data-ttu-id="ca303-114">Talep tabanlı yetkilendirme denetimleri bildirim temelli - Geliştirici bunları kendi kodundaki bir denetleyici veya eylem bir denetleyici içinde karşı geçerli kullanıcı verilerine sahip olması gerekir ve isteğe bağlı olarak, talep değeri erişimi tutmanız gereken talepleri belirtme ekler İstenen kaynak.</span><span class="sxs-lookup"><span data-stu-id="ca303-114">Claim based authorization checks are declarative - the developer embeds them within their code, against a controller or an action within a controller, specifying claims which the current user must possess, and optionally the value the claim must hold to access the requested resource.</span></span> <span data-ttu-id="ca303-115">Talepler, ilke tabanlı gereksinimleridir Geliştirici oluşturun ve talep gereksinimleri ifade bir ilkeyi kaydetmek gerekir.</span><span class="sxs-lookup"><span data-stu-id="ca303-115">Claims requirements are policy based, the developer must build and register a policy expressing the claims requirements.</span></span>

<span data-ttu-id="ca303-116">En basit türü talep İlkesi talebi varlığı arar ve değeri denetlemez.</span><span class="sxs-lookup"><span data-stu-id="ca303-116">The simplest type of claim policy looks for the presence of a claim and doesn't check the value.</span></span>

<span data-ttu-id="ca303-117">İlk oluşturun ve ilkeyi kaydetmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="ca303-117">First you need to build and register the policy.</span></span> <span data-ttu-id="ca303-118">Bu normalde bölümü alır, yetkilendirme hizmet yapılandırmasının parçası olarak gerçekleşmeden `ConfigureServices()` içinde *Startup.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="ca303-118">This takes place as part of the Authorization service configuration, which normally takes part in `ConfigureServices()` in your *Startup.cs* file.</span></span>

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

<span data-ttu-id="ca303-119">Bu durumda `EmployeeOnly` ilke varlığını denetler bir `EmployeeNumber` geçerli kimliğini talep.</span><span class="sxs-lookup"><span data-stu-id="ca303-119">In this case the `EmployeeOnly` policy checks for the presence of an `EmployeeNumber` claim on the current identity.</span></span>

<span data-ttu-id="ca303-120">Ardından ilkeyi kullanan uygulama `Policy` özelliği `AuthorizeAttribute` ; ilke adını belirtmek için özniteliği</span><span class="sxs-lookup"><span data-stu-id="ca303-120">You then apply the policy using the `Policy` property on the `AuthorizeAttribute` attribute to specify the policy name;</span></span>

```csharp
[Authorize(Policy = "EmployeeOnly")]
public IActionResult VacationBalance()
{
    return View();
}
```

<span data-ttu-id="ca303-121">`AuthorizeAttribute` Özniteliği bu örnekte, tüm bir denetleyici için kimlikleri ilkeyle eşleşen herhangi bir işlem erişim denetleyicisinde izin verilecek yalnızca uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="ca303-121">The `AuthorizeAttribute` attribute can be applied to an entire controller, in this instance only identities matching the policy will be allowed access to any Action on the controller.</span></span>

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class VacationController : Controller
{
    public ActionResult VacationBalance()
    {
    }
}
```

<span data-ttu-id="ca303-122">Tarafından korunan bir denetleyici varsa `AuthorizeAttribute` özniteliği ancak uyguladığınız belirli eylemlere anonim erişime izin vermek istediğiniz `AllowAnonymousAttribute` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="ca303-122">If you have a controller that's protected by the `AuthorizeAttribute` attribute, but want to allow anonymous access to particular actions you apply the `AllowAnonymousAttribute` attribute.</span></span>

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

<span data-ttu-id="ca303-123">Çoğu talep değeri ile gelir.</span><span class="sxs-lookup"><span data-stu-id="ca303-123">Most claims come with a value.</span></span> <span data-ttu-id="ca303-124">İlkeyi oluştururken, izin verilen değerlerin bir listesini belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ca303-124">You can specify a list of allowed values when creating the policy.</span></span> <span data-ttu-id="ca303-125">Aşağıdaki örnek 1, 2, 3, 4 veya 5 ayarlanmış çalışan numarası olan çalışanlar için yalnızca başarılı olabilir.</span><span class="sxs-lookup"><span data-stu-id="ca303-125">The following example would only succeed for employees whose employee number was 1, 2, 3, 4 or 5.</span></span>

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

### <a name="add-a-generic-claim-check"></a><span data-ttu-id="ca303-126">Genel talep Denetimi Ekle</span><span class="sxs-lookup"><span data-stu-id="ca303-126">Add a generic claim check</span></span>

<span data-ttu-id="ca303-127">Talep değeri tek bir değer değil veya bir dönüştürme gerekli değildir, kullanın [RequireAssertion](/dotnet/api/microsoft.aspnetcore.authorization.authorizationpolicybuilder.requireassertion).</span><span class="sxs-lookup"><span data-stu-id="ca303-127">If the claim value isn't a single value or a transformation is required, use [RequireAssertion](/dotnet/api/microsoft.aspnetcore.authorization.authorizationpolicybuilder.requireassertion).</span></span> <span data-ttu-id="ca303-128">Daha fazla bilgi için [bir ilkeyi karşılamak için bir func kullanarak](xref:security/authorization/policies#using-a-func-to-fulfill-a-policy).</span><span class="sxs-lookup"><span data-stu-id="ca303-128">For more information, see [Using a func to fulfill a policy](xref:security/authorization/policies#using-a-func-to-fulfill-a-policy).</span></span>

## <a name="multiple-policy-evaluation"></a><span data-ttu-id="ca303-129">Birden çok ilke değerlendirmesi</span><span class="sxs-lookup"><span data-stu-id="ca303-129">Multiple Policy Evaluation</span></span>

<span data-ttu-id="ca303-130">Bir denetleyici veya eylem için birden çok ilke uygularsanız, erişim sağlanmadan önce tüm ilkeleri geçmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="ca303-130">If you apply multiple policies to a controller or action, then all policies must pass before access is granted.</span></span> <span data-ttu-id="ca303-131">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="ca303-131">For example:</span></span>

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

<span data-ttu-id="ca303-132">Yukarıdaki örnekte, karşıladığı tüm kimlik `EmployeeOnly` erişim ilkesi `Payslip` denetleyicisinde eylemi olarak bu ilke zorunlu tutulur.</span><span class="sxs-lookup"><span data-stu-id="ca303-132">In the above example any identity which fulfills the `EmployeeOnly` policy can access the `Payslip` action as that policy is enforced on the controller.</span></span> <span data-ttu-id="ca303-133">Ancak çağırmak için `UpdateSalary` gerekir kimlik karşılamak eylem *hem* `EmployeeOnly` ilke ve `HumanResources` ilkesi.</span><span class="sxs-lookup"><span data-stu-id="ca303-133">However in order to call the `UpdateSalary` action the identity must fulfill *both* the `EmployeeOnly` policy and the `HumanResources` policy.</span></span>

<span data-ttu-id="ca303-134">Daha karmaşık ilkeleri istiyorsanız, talep doğum tarihi alma gibi verilerden bir yaş hesaplama sonra geçerlilik süresi denetimi 21 veya eski rolüyse yazmanız gereken [özel ilke işleyicileri](xref:security/authorization/policies).</span><span class="sxs-lookup"><span data-stu-id="ca303-134">If you want more complicated policies, such as taking a date of birth claim, calculating an age from it then checking the age is 21 or older then you need to write [custom policy handlers](xref:security/authorization/policies).</span></span>
