---
title: Kimlik doğrulaması ve kimlik için ASP.NET Core geçişi
author: ardalis
description: Kimlik doğrulaması ve kimlik, bir ASP.NET Core MVC projesini ASP.NET MVC projesinde geçirmeyi öğrenin.
ms.author: riande
ms.date: 10/14/2016
uid: migration/identity
ms.openlocfilehash: 72e62e78e37325ec47d54abbc11a875ae87fb63a
ms.sourcegitcommit: af8a6eb5375ef547a52ffae22465e265837aa82b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2019
ms.locfileid: "41755674"
---
# <a name="migrate-authentication-and-identity-to-aspnet-core"></a><span data-ttu-id="2a634-103">Kimlik doğrulaması ve kimlik için ASP.NET Core geçişi</span><span class="sxs-lookup"><span data-stu-id="2a634-103">Migrate Authentication and Identity to ASP.NET Core</span></span>

<span data-ttu-id="2a634-104">Tarafından [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="2a634-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="2a634-105">Önceki makalede, biz [yapılandırma, ASP.NET Core MVC için ASP.NET MVC projesinde geçişi](xref:migration/configuration).</span><span class="sxs-lookup"><span data-stu-id="2a634-105">In the previous article, we [migrated configuration from an ASP.NET MVC project to ASP.NET Core MVC](xref:migration/configuration).</span></span> <span data-ttu-id="2a634-106">Bu makalede, biz kaydı, oturum açma ve kullanıcı yönetimi özellikleri geçirin.</span><span class="sxs-lookup"><span data-stu-id="2a634-106">In this article, we migrate the registration, login, and user management features.</span></span>

## <a name="configure-identity-and-membership"></a><span data-ttu-id="2a634-107">Kimlik ve üyelik yapılandırın</span><span class="sxs-lookup"><span data-stu-id="2a634-107">Configure Identity and Membership</span></span>

<span data-ttu-id="2a634-108">ASP.NET MVC, ASP.NET Identity'de kullanarak kimlik doğrulaması ve kimlik özellikler yapılandırıldı *Startup.Auth.cs* ve *IdentityConfig.cs*, bulunan *App_Start* klasör.</span><span class="sxs-lookup"><span data-stu-id="2a634-108">In ASP.NET MVC, authentication and identity features are configured using ASP.NET Identity in *Startup.Auth.cs* and *IdentityConfig.cs*, located in the *App_Start* folder.</span></span> <span data-ttu-id="2a634-109">Bu özellikler yapılandırılan ASP.NET Core MVC *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="2a634-109">In ASP.NET Core MVC, these features are configured in *Startup.cs*.</span></span>

<span data-ttu-id="2a634-110">Yükleme `Microsoft.AspNetCore.Identity.EntityFrameworkCore` ve `Microsoft.AspNetCore.Authentication.Cookies` NuGet paketleri.</span><span class="sxs-lookup"><span data-stu-id="2a634-110">Install the `Microsoft.AspNetCore.Identity.EntityFrameworkCore` and `Microsoft.AspNetCore.Authentication.Cookies` NuGet packages.</span></span>

<span data-ttu-id="2a634-111">Ardından, açın *Startup.cs* ve güncelleştirme `Startup.ConfigureServices` Entity Framework ve kimlik Hizmetleri yöntemi:</span><span class="sxs-lookup"><span data-stu-id="2a634-111">Then, open *Startup.cs* and update the `Startup.ConfigureServices` method to use Entity Framework and Identity services:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add EF services to the services container.
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("DefaultConnection")));

    services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

     services.AddMvc();
}
```

<span data-ttu-id="2a634-112">Bu noktada, biz ASP.NET MVC projeden henüz geçirilmeyen henüz yukarıdaki kodda başvurulan iki tür vardır: `ApplicationDbContext` ve `ApplicationUser`.</span><span class="sxs-lookup"><span data-stu-id="2a634-112">At this point, there are two types referenced in the above code that we haven't yet migrated from the ASP.NET MVC project: `ApplicationDbContext` and `ApplicationUser`.</span></span> <span data-ttu-id="2a634-113">Yeni bir *modelleri* klasöründe ASP.NET Core projesi ve ona bu türlerine karşılık gelen iki sınıf ekleyin.</span><span class="sxs-lookup"><span data-stu-id="2a634-113">Create a new *Models* folder in the ASP.NET Core project, and add two classes to it corresponding to these types.</span></span> <span data-ttu-id="2a634-114">ASP.NET MVC, bu sınıfları sürümlerini bulabilirsiniz */Models/IdentityModels.cs*, ancak, daha açık olduğundan geçirilen projedeki her bir dosya kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="2a634-114">You will find the ASP.NET MVC versions of these classes in */Models/IdentityModels.cs*, but we will use one file per class in the migrated project since that's more clear.</span></span>

<span data-ttu-id="2a634-115">*ApplicationUser.cs*:</span><span class="sxs-lookup"><span data-stu-id="2a634-115">*ApplicationUser.cs*:</span></span>

```csharp
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;

namespace NewMvcProject.Models
{
  public class ApplicationUser : IdentityUser
  {
  }
}
```

<span data-ttu-id="2a634-116">*ApplicationDbContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="2a634-116">*ApplicationDbContext.cs*:</span></span>

```csharp
using Microsoft.AspNetCore.Identity.EntityFramework;
using Microsoft.Data.Entity;

namespace NewMvcProject.Models
{
    public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
    {
        public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
            : base(options)
        {
        }

        protected override void OnModelCreating(ModelBuilder builder)
        {
            base.OnModelCreating(builder);
            // Customize the ASP.NET Core Identity model and override the defaults if needed.
            // For example, you can rename the ASP.NET Core Identity table names and more.
            // Add your customizations after calling base.OnModelCreating(builder);
        }
    }
}
```

<span data-ttu-id="2a634-117">ASP.NET Core MVC başlangıç Web Proje kullanıcı kadar özelleştirme içermeyen veya `ApplicationDbContext`.</span><span class="sxs-lookup"><span data-stu-id="2a634-117">The ASP.NET Core MVC Starter Web project doesn't include much customization of users, or the `ApplicationDbContext`.</span></span> <span data-ttu-id="2a634-118">Gerçek bir uygulamada geçirirken, ayrıca tüm özel özellikleri ve yöntemleri, uygulamanızın kullanıcının geçmeniz ve `DbContext` sınıflarının yanı sıra, uygulamanızı kullanan başka bir Model sınıfları.</span><span class="sxs-lookup"><span data-stu-id="2a634-118">When migrating a real app, you also need to migrate all of the custom properties and methods of your app's user and `DbContext` classes, as well as any other Model classes your app utilizes.</span></span> <span data-ttu-id="2a634-119">Örneğin, varsa, `DbContext` sahip bir `DbSet<Album>`, taşımaya gerek `Album` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="2a634-119">For example, if your `DbContext` has a `DbSet<Album>`, you need to migrate the `Album` class.</span></span>

<span data-ttu-id="2a634-120">Bu dosyaları yerinde, ile *Startup.cs* dosyasını güncelleştirerek derlemek için yapılabilir, `using` ifadeleri:</span><span class="sxs-lookup"><span data-stu-id="2a634-120">With these files in place, the *Startup.cs* file can be made to compile by updating its `using` statements:</span></span>

```csharp
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Identity;
using Microsoft.AspNetCore.Hosting;
using Microsoft.EntityFrameworkCore;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.DependencyInjection;
```

<span data-ttu-id="2a634-121">Uygulamamızı kimlik doğrulaması ve kimlik Hizmetleri desteklemek artık hazırdır.</span><span class="sxs-lookup"><span data-stu-id="2a634-121">Our app is now ready to support authentication and Identity services.</span></span> <span data-ttu-id="2a634-122">Bunu yalnızca kullanıcılara açık hale bu özelliklere sahip olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="2a634-122">It just needs to have these features exposed to users.</span></span>

## <a name="migrate-registration-and-login-logic"></a><span data-ttu-id="2a634-123">Kayıt ve oturum açma mantığı geçirme</span><span class="sxs-lookup"><span data-stu-id="2a634-123">Migrate registration and login logic</span></span>

<span data-ttu-id="2a634-124">Kimlik Hizmetleri uygulama için yapılandırılmış ve yapılandırılmış Entity Framework ve SQL Server'ı kullanarak veri erişimi, uygulamaya kayıt ve oturum açma desteği eklemek hazırız.</span><span class="sxs-lookup"><span data-stu-id="2a634-124">With Identity services configured for the app and data access configured using Entity Framework and SQL Server, we're ready to add support for registration and login to the app.</span></span> <span data-ttu-id="2a634-125">Sözcüğünün [önceki geçiş sürecinde](xref:migration/mvc#migrate-the-layout-file) biz başvuru yorum *_LoginPartial* içinde *_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="2a634-125">Recall that [earlier in the migration process](xref:migration/mvc#migrate-the-layout-file) we commented out a reference to *_LoginPartial* in *_Layout.cshtml*.</span></span> <span data-ttu-id="2a634-126">Artık bu kod, iade açıklamasını kaldırın ve gerekli denetleyicileri veya oturum açma işlevlerini desteklemek için Görünüm ekleme zamanı geldi.</span><span class="sxs-lookup"><span data-stu-id="2a634-126">Now it's time to return to that code, uncomment it, and add in the necessary controllers and views to support login functionality.</span></span>

<span data-ttu-id="2a634-127">Açıklamadan çıkarın `@Html.Partial` satırına *_Layout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="2a634-127">Uncomment the `@Html.Partial` line in *_Layout.cshtml*:</span></span>

```cshtml
      <li>@Html.ActionLink("Contact", "Contact", "Home")</li>
    </ul>
    @*@Html.Partial("_LoginPartial")*@
  </div>
</div>
```

<span data-ttu-id="2a634-128">Adlı yeni bir Razor görünüm şimdi ekleyin *_LoginPartial* için *görünümler/paylaşılan* klasörü:</span><span class="sxs-lookup"><span data-stu-id="2a634-128">Now, add a new Razor view called *_LoginPartial* to the *Views/Shared* folder:</span></span>

<span data-ttu-id="2a634-129">Güncelleştirme *_LoginPartial.cshtml* aşağıdaki kodla (tüm içeriğini değiştirin):</span><span class="sxs-lookup"><span data-stu-id="2a634-129">Update *_LoginPartial.cshtml* with the following code (replace all of its contents):</span></span>

```cshtml
@inject SignInManager<ApplicationUser> SignInManager
@inject UserManager<ApplicationUser> UserManager

@if (SignInManager.IsSignedIn(User))
{
    <form asp-area="" asp-controller="Account" asp-action="Logout" method="post" id="logoutForm" class="navbar-right">
        <ul class="nav navbar-nav navbar-right">
            <li>
                <a asp-area="" asp-controller="Manage" asp-action="Index" title="Manage">Hello @UserManager.GetUserName(User)!</a>
            </li>
            <li>
                <button type="submit" class="btn btn-link navbar-btn navbar-link">Log out</button>
            </li>
        </ul>
    </form>
}
else
{
    <ul class="nav navbar-nav navbar-right">
        <li><a asp-area="" asp-controller="Account" asp-action="Register">Register</a></li>
        <li><a asp-area="" asp-controller="Account" asp-action="Login">Log in</a></li>
    </ul>
}
```

<span data-ttu-id="2a634-130">Bu noktada, sitenin tarayıcınızda yenileyemiyoruz olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2a634-130">At this point, you should be able to refresh the site in your browser.</span></span>

## <a name="summary"></a><span data-ttu-id="2a634-131">Özet</span><span class="sxs-lookup"><span data-stu-id="2a634-131">Summary</span></span>

<span data-ttu-id="2a634-132">ASP.NET Core ASP.NET Identity özellikleri için değişiklikler yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="2a634-132">ASP.NET Core introduces changes to the ASP.NET Identity features.</span></span> <span data-ttu-id="2a634-133">Bu makalede, ASP.NET Core kimlik doğrulaması ve kullanıcı yönetim özelliklerine ASP.NET Identity'ye geçirme gördünüz.</span><span class="sxs-lookup"><span data-stu-id="2a634-133">In this article, you have seen how to migrate the authentication and user management features of ASP.NET Identity to ASP.NET Core.</span></span>
