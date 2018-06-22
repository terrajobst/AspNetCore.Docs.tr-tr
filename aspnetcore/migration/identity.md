---
title: Kimlik doğrulama ve kimlik için ASP.NET Core geçirme
author: ardalis
description: Kimlik doğrulama ve kimlik bir ASP.NET MVC projesinde ASP.NET Core MVC projesinde geçirmek öğrenin.
ms.author: riande
ms.date: 10/14/2016
uid: migration/identity
ms.openlocfilehash: e05d72ca78c7b8191a47f78cda31ee40e04d0706
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36275703"
---
# <a name="migrate-authentication-and-identity-to-aspnet-core"></a><span data-ttu-id="2f287-103">Kimlik doğrulama ve kimlik için ASP.NET Core geçirme</span><span class="sxs-lookup"><span data-stu-id="2f287-103">Migrate Authentication and Identity to ASP.NET Core</span></span>

<span data-ttu-id="2f287-104">Tarafından [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="2f287-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="2f287-105">Önceki makalede, biz [ASP.NET Core MVC için bir ASP.NET MVC projesinde yapılandırma geçişi](xref:migration/configuration).</span><span class="sxs-lookup"><span data-stu-id="2f287-105">In the previous article, we [migrated configuration from an ASP.NET MVC project to ASP.NET Core MVC](xref:migration/configuration).</span></span> <span data-ttu-id="2f287-106">Bu makalede, kayıt, oturum açma ve kullanıcı yönetimi özellikleri geçirin.</span><span class="sxs-lookup"><span data-stu-id="2f287-106">In this article, we migrate the registration, login, and user management features.</span></span>

## <a name="configure-identity-and-membership"></a><span data-ttu-id="2f287-107">Kimlik ve üyelik yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="2f287-107">Configure Identity and Membership</span></span>

<span data-ttu-id="2f287-108">ASP.NET MVC uygulamasında ASP.NET Identity'de kullanarak kimlik doğrulama ve kimlik özellikler yapılandırıldı *Startup.Auth.cs* ve *IdentityConfig.cs*, bulunan *App_Start* klasör.</span><span class="sxs-lookup"><span data-stu-id="2f287-108">In ASP.NET MVC, authentication and identity features are configured using ASP.NET Identity in *Startup.Auth.cs* and *IdentityConfig.cs*, located in the *App_Start* folder.</span></span> <span data-ttu-id="2f287-109">Bu özellikler yapılandırılan ASP.NET Core MVC'de *haline*.</span><span class="sxs-lookup"><span data-stu-id="2f287-109">In ASP.NET Core MVC, these features are configured in *Startup.cs*.</span></span>

<span data-ttu-id="2f287-110">Yükleme `Microsoft.AspNetCore.Identity.EntityFrameworkCore` ve `Microsoft.AspNetCore.Authentication.Cookies` NuGet paketleri.</span><span class="sxs-lookup"><span data-stu-id="2f287-110">Install the `Microsoft.AspNetCore.Identity.EntityFrameworkCore` and `Microsoft.AspNetCore.Authentication.Cookies` NuGet packages.</span></span>

<span data-ttu-id="2f287-111">Ardından, açın *haline* ve güncelleştirme `Startup.ConfigureServices` Entity Framework ve kimlik Hizmetleri kullanılacak yöntemi:</span><span class="sxs-lookup"><span data-stu-id="2f287-111">Then, open *Startup.cs* and update the `Startup.ConfigureServices` method to use Entity Framework and Identity services:</span></span>

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

<span data-ttu-id="2f287-112">Bu noktada, biz ASP.NET MVC projeden henüz geçirilmeyen henüz Yukarıdaki kod başvurulan iki tür vardır: `ApplicationDbContext` ve `ApplicationUser`.</span><span class="sxs-lookup"><span data-stu-id="2f287-112">At this point, there are two types referenced in the above code that we haven't yet migrated from the ASP.NET MVC project: `ApplicationDbContext` and `ApplicationUser`.</span></span> <span data-ttu-id="2f287-113">Yeni bir *modelleri* ASP.NET Core klasöründe proje ve onu bu türlerine karşılık gelen iki sınıf ekleyin.</span><span class="sxs-lookup"><span data-stu-id="2f287-113">Create a new *Models* folder in the ASP.NET Core project, and add two classes to it corresponding to these types.</span></span> <span data-ttu-id="2f287-114">ASP.NET MVC bu sınıfları sürümleri bulacaksınız */Models/IdentityModels.cs*, ancak daha açık olduğu için geçirilen projedeki başına bir dosyayı kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="2f287-114">You will find the ASP.NET MVC versions of these classes in */Models/IdentityModels.cs*, but we will use one file per class in the migrated project since that's more clear.</span></span>

<span data-ttu-id="2f287-115">*ApplicationUser.cs*:</span><span class="sxs-lookup"><span data-stu-id="2f287-115">*ApplicationUser.cs*:</span></span>

```csharp
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;

namespace NewMvcProject.Models
{
  public class ApplicationUser : IdentityUser
  {
  }
}
```

<span data-ttu-id="2f287-116">*ApplicationDbContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="2f287-116">*ApplicationDbContext.cs*:</span></span>

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
            // Customize the ASP.NET Identity model and override the defaults if needed.
            // For example, you can rename the ASP.NET Identity table names and more.
            // Add your customizations after calling base.OnModelCreating(builder);
        }
    }
}
```

<span data-ttu-id="2f287-117">ASP.NET Core MVC Starter Web projesi kullanıcıların kadar özelleştirme içermeyen veya `ApplicationDbContext`.</span><span class="sxs-lookup"><span data-stu-id="2f287-117">The ASP.NET Core MVC Starter Web project doesn't include much customization of users, or the `ApplicationDbContext`.</span></span> <span data-ttu-id="2f287-118">Gerçek bir uygulama geçirirken, ayrıca tüm özel özellikleri ve yöntemleri, uygulamanızın kullanıcının geçiş yapmanız ve `DbContext` sınıfları yanı sıra, uygulamanızı kullanan diğer Model sınıfları.</span><span class="sxs-lookup"><span data-stu-id="2f287-118">When migrating a real app, you also need to migrate all of the custom properties and methods of your app's user and `DbContext` classes, as well as any other Model classes your app utilizes.</span></span> <span data-ttu-id="2f287-119">Örneğin, varsa, `DbContext` sahip bir `DbSet<Album>`, geçiş yapmanız `Album` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="2f287-119">For example, if your `DbContext` has a `DbSet<Album>`, you need to migrate the `Album` class.</span></span>

<span data-ttu-id="2f287-120">Bu dosyalar varken, sahip *haline* güncelleştirerek derlemek için dosya yapılabilir kendi `using` deyimleri:</span><span class="sxs-lookup"><span data-stu-id="2f287-120">With these files in place, the *Startup.cs* file can be made to compile by updating its `using` statements:</span></span>

```csharp
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Identity;
using Microsoft.AspNetCore.Hosting;
using Microsoft.EntityFrameworkCore;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.DependencyInjection;
```

<span data-ttu-id="2f287-121">Bizim uygulama artık kimlik doğrulaması ve kimlik hizmetlerini desteklemek hazırdır.</span><span class="sxs-lookup"><span data-stu-id="2f287-121">Our app is now ready to support authentication and Identity services.</span></span> <span data-ttu-id="2f287-122">Bunu yalnızca bu özellikleri kullanıcılara açık olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2f287-122">It just needs to have these features exposed to users.</span></span>

## <a name="migrate-registration-and-login-logic"></a><span data-ttu-id="2f287-123">Kayıt ve oturum açma mantığı geçirme</span><span class="sxs-lookup"><span data-stu-id="2f287-123">Migrate registration and login logic</span></span>

<span data-ttu-id="2f287-124">Uygulama için yapılandırılmış kimlik Hizmetleri ve Entity Framework ve SQL Server kullanarak yapılandırılmış veri erişimi, biz kayıt ve oturum açma desteğini uygulamaya eklemek hazırsınız.</span><span class="sxs-lookup"><span data-stu-id="2f287-124">With Identity services configured for the app and data access configured using Entity Framework and SQL Server, we're ready to add support for registration and login to the app.</span></span> <span data-ttu-id="2f287-125">Sözcüğünün [geçiş sürecinde önceki](xref:migration/mvc#migrate-the-layout-file) biz başvuru kılınmıştır *_LoginPartial* içinde *_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="2f287-125">Recall that [earlier in the migration process](xref:migration/mvc#migrate-the-layout-file) we commented out a reference to *_LoginPartial* in *_Layout.cshtml*.</span></span> <span data-ttu-id="2f287-126">Şimdi bu kod, dönüş açıklama durumundan çıkarmanız ve gerekli denetleyicileri ve oturum açma işlevselliği destekleyecek şekilde görünümlerde ekleme zamanı geldi.</span><span class="sxs-lookup"><span data-stu-id="2f287-126">Now it's time to return to that code, uncomment it, and add in the necessary controllers and views to support login functionality.</span></span>

<span data-ttu-id="2f287-127">Açıklamadan çıkarın `@Html.Partial` satırından *_Layout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="2f287-127">Uncomment the `@Html.Partial` line in *_Layout.cshtml*:</span></span>

```cshtml
      <li>@Html.ActionLink("Contact", "Contact", "Home")</li>
    </ul>
    @*@Html.Partial("_LoginPartial")*@
  </div>
</div>
```

<span data-ttu-id="2f287-128">Şimdi, adlı yeni bir Razor Görünüm Ekle *_LoginPartial* için *görünümler/paylaşılan* klasörü:</span><span class="sxs-lookup"><span data-stu-id="2f287-128">Now, add a new Razor view called *_LoginPartial* to the *Views/Shared* folder:</span></span>

<span data-ttu-id="2f287-129">Güncelleştirme *_LoginPartial.cshtml* aşağıdaki kodla (tüm içeriğini değiştirin):</span><span class="sxs-lookup"><span data-stu-id="2f287-129">Update *_LoginPartial.cshtml* with the following code (replace all of its contents):</span></span>

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

<span data-ttu-id="2f287-130">Bu noktada, tarayıcınızda siteyi Yenile yapabiliyor olmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="2f287-130">At this point, you should be able to refresh the site in your browser.</span></span>

## <a name="summary"></a><span data-ttu-id="2f287-131">Özet</span><span class="sxs-lookup"><span data-stu-id="2f287-131">Summary</span></span>

<span data-ttu-id="2f287-132">ASP.NET Core değişiklikleri için ASP.NET Identity özellikler sunar.</span><span class="sxs-lookup"><span data-stu-id="2f287-132">ASP.NET Core introduces changes to the ASP.NET Identity features.</span></span> <span data-ttu-id="2f287-133">Bu makalede, ASP.NET Identity kimlik doğrulaması ve kullanıcı yönetim özelliklerine ASP.NET Core geçirmek nasıl gördünüz.</span><span class="sxs-lookup"><span data-stu-id="2f287-133">In this article, you have seen how to migrate the authentication and user management features of ASP.NET Identity to ASP.NET Core.</span></span>
