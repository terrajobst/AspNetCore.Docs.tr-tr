---
title: "Geçirme kimlik doğrulama ve kimlik"
author: ardalis
description: 
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/identity
ms.openlocfilehash: a3aa976578d8db089c69bf888f492f9cd7df824b
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="migrating-authentication-and-identity"></a><span data-ttu-id="03799-102">Geçirme kimlik doğrulama ve kimlik</span><span class="sxs-lookup"><span data-stu-id="03799-102">Migrating Authentication and Identity</span></span>

<a name="migration-identity"></a>

<span data-ttu-id="03799-103">Tarafından [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="03799-103">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="03799-104">Önceki makalesi biz [ASP.NET Core MVC için bir ASP.NET MVC projesinde yapılandırma geçişi](configuration.md).</span><span class="sxs-lookup"><span data-stu-id="03799-104">In the previous article we [migrated configuration from an ASP.NET MVC project to ASP.NET Core MVC](configuration.md).</span></span> <span data-ttu-id="03799-105">Bu makalede, kayıt, oturum açma ve kullanıcı yönetimi özellikleri geçirin.</span><span class="sxs-lookup"><span data-stu-id="03799-105">In this article, we migrate the registration, login, and user management features.</span></span>

## <a name="configure-identity-and-membership"></a><span data-ttu-id="03799-106">Kimlik ve üyelik yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="03799-106">Configure Identity and Membership</span></span>

<span data-ttu-id="03799-107">ASP.NET MVC uygulamasında ASP.NET Identity Startup.Auth.cs ve App_Start klasöründe IdentityConfig.cs kullanarak kimlik doğrulama ve kimlik özellikler yapılandırıldı.</span><span class="sxs-lookup"><span data-stu-id="03799-107">In ASP.NET MVC, authentication and identity features are configured using ASP.NET Identity in Startup.Auth.cs and IdentityConfig.cs, located in the App_Start folder.</span></span> <span data-ttu-id="03799-108">Bu özellikler yapılandırılan ASP.NET Core MVC'de *haline*.</span><span class="sxs-lookup"><span data-stu-id="03799-108">In ASP.NET Core MVC, these features are configured in *Startup.cs*.</span></span>

<span data-ttu-id="03799-109">Yükleme `Microsoft.AspNetCore.Identity.EntityFrameworkCore` ve `Microsoft.AspNetCore.Authentication.Cookies` NuGet paketleri.</span><span class="sxs-lookup"><span data-stu-id="03799-109">Install the `Microsoft.AspNetCore.Identity.EntityFrameworkCore` and `Microsoft.AspNetCore.Authentication.Cookies` NuGet packages.</span></span>

<span data-ttu-id="03799-110">Ardından, haline ve güncelleştirme açın `ConfigureServices()` Entity Framework ve kimlik Hizmetleri kullanılacak yöntemi:</span><span class="sxs-lookup"><span data-stu-id="03799-110">Then, open Startup.cs and update the `ConfigureServices()` method to use Entity Framework and Identity services:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
  // Add EF services to the services container.
  services.AddEntityFramework(Configuration)
    .AddSqlServer()
    .AddDbContext<ApplicationDbContext>();

  // Add Identity services to the services container.
  services.AddIdentity<ApplicationUser, IdentityRole>(Configuration)
    .AddEntityFrameworkStores<ApplicationDbContext>();

  services.AddMvc();
}
```

<span data-ttu-id="03799-111">Bu noktada, biz ASP.NET MVC projeden henüz geçirilmeyen henüz Yukarıdaki kod başvurulan iki tür vardır: `ApplicationDbContext` ve `ApplicationUser`.</span><span class="sxs-lookup"><span data-stu-id="03799-111">At this point, there are two types referenced in the above code that we haven't yet migrated from the ASP.NET MVC project: `ApplicationDbContext` and `ApplicationUser`.</span></span> <span data-ttu-id="03799-112">Yeni bir *modelleri* ASP.NET Core klasöründe proje ve onu bu türlerine karşılık gelen iki sınıf ekleyin.</span><span class="sxs-lookup"><span data-stu-id="03799-112">Create a new *Models* folder in the ASP.NET Core project, and add two classes to it corresponding to these types.</span></span> <span data-ttu-id="03799-113">ASP.NET MVC bu sınıfları sürümleri bulacaksınız `/Models/IdentityModels.cs`, ancak daha açık olduğu için geçirilen projedeki başına bir dosyayı kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="03799-113">You will find the ASP.NET MVC versions of these classes in `/Models/IdentityModels.cs`, but we will use one file per class in the migrated project since that's more clear.</span></span>

<span data-ttu-id="03799-114">ApplicationUser.cs:</span><span class="sxs-lookup"><span data-stu-id="03799-114">ApplicationUser.cs:</span></span>

```csharp
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;

namespace NewMvc6Project.Models
{
  public class ApplicationUser : IdentityUser
  {
  }
}
```

<span data-ttu-id="03799-115">ApplicationDbContext.cs:</span><span class="sxs-lookup"><span data-stu-id="03799-115">ApplicationDbContext.cs:</span></span>

```csharp
using Microsoft.AspNetCore.Identity.EntityFramework;
using Microsoft.Data.Entity;

namespace NewMvc6Project.Models
{
  public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
  {
    public ApplicationDbContext()
    {
      Database.EnsureCreated();
    }

    protected override void OnConfiguring(DbContextOptionsBuilder options)
    {
      options.UseSqlServer();
    }
  }
}
```

<span data-ttu-id="03799-116">ASP.NET Core MVC Starter Web projesi kadar özelleştirme kullanıcı ya da ApplicationDbContext içermez.</span><span class="sxs-lookup"><span data-stu-id="03799-116">The ASP.NET Core MVC Starter Web project doesn't include much customization of users, or the ApplicationDbContext.</span></span> <span data-ttu-id="03799-117">Gerçek bir uygulamada geçirirken, ayrıca tüm özel özellikleri ve yöntemleri, (örneğin, DbContext DbSetvarsa,uygulamanızınkullanandiğermodelisınıflarıyanısıra,uygulamanızınkullanıcıveDbContextsınıflarıgeçişyapmanızgerekecektir<Album>, Elbette albüm sınıfı geçiş yapmanız gerekecektir).</span><span class="sxs-lookup"><span data-stu-id="03799-117">When migrating a real application, you will also need to migrate all of the custom properties and methods of your application's user and DbContext classes, as well as any other Model classes your application utilizes (for example, if your DbContext has a DbSet<Album>, you will of course need to migrate the Album class).</span></span>

<span data-ttu-id="03799-118">Bu dosyaları yerinde kendi kullanarak güncelleştirerek derlemek için haline dosya yapılabilmesi için deyimleri:</span><span class="sxs-lookup"><span data-stu-id="03799-118">With these files in place, the Startup.cs file can be made to compile by updating its using statements:</span></span>

```csharp
using Microsoft.Framework.ConfigurationModel;
using Microsoft.AspNetCore.Hosting;
using NewMvc6Project.Models;
using Microsoft.AspNetCore.Identity;
```

<span data-ttu-id="03799-119">Uygulamamız, kimlik doğrulama ve kimlik hizmetlerini desteklemeye hazırdır -, yalnızca bu özellikleri kullanıcılara açık olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="03799-119">Our application is now ready to support authentication and identity services - it just needs to have these features exposed to users.</span></span>

## <a name="migrate-registration-and-login-logic"></a><span data-ttu-id="03799-120">Kayıt ve oturum açma mantığı geçirme</span><span class="sxs-lookup"><span data-stu-id="03799-120">Migrate Registration and Login Logic</span></span>

<span data-ttu-id="03799-121">Uygulama ve Entity Framework ve SQL Server kullanarak yapılandırılmış veri erişimi için yapılandırılmış kimlik Hizmetleri ile biz şimdi uygulama kayıt ve oturum açma desteği eklemek hazır olursunuz.</span><span class="sxs-lookup"><span data-stu-id="03799-121">With identity services configured for the application and data access configured using Entity Framework and SQL Server, we are now ready to add support for registration and login to the application.</span></span> <span data-ttu-id="03799-122">Sözcüğünün [geçiş sürecinde önceki](mvc.md#migrate-layout-file) biz _Layout.cshtml _LoginPartial başvuru kılınmıştır.</span><span class="sxs-lookup"><span data-stu-id="03799-122">Recall that [earlier in the migration process](mvc.md#migrate-layout-file) we commented out a reference to _LoginPartial in _Layout.cshtml.</span></span> <span data-ttu-id="03799-123">Şimdi bu kod, dönüş açıklama durumundan çıkarmanız ve gerekli denetleyicileri ve oturum açma işlevselliği destekleyecek şekilde görünümlerde ekleme zamanı geldi.</span><span class="sxs-lookup"><span data-stu-id="03799-123">Now it's time to return to that code, uncomment it, and add in the necessary controllers and views to support login functionality.</span></span>

<span data-ttu-id="03799-124">Güncelleştirme _Layout.cshtml; açıklamadan çıkarın @Html.Partial satır:</span><span class="sxs-lookup"><span data-stu-id="03799-124">Update _Layout.cshtml; uncomment the @Html.Partial line:</span></span>

```cshtml
      <li>@Html.ActionLink("Contact", "Contact", "Home")</li>
    </ul>
    @*@Html.Partial("_LoginPartial")*@
  </div>
</div>
```

<span data-ttu-id="03799-125">Şimdi, MVC görünümü görünümler/paylaşılan klasöre _LoginPartial adlı yeni bir sayfa ekleyin:</span><span class="sxs-lookup"><span data-stu-id="03799-125">Now, add a new MVC View Page called _LoginPartial to the Views/Shared folder:</span></span>

<span data-ttu-id="03799-126">Aşağıdaki kod ile _LoginPartial.cshtml güncelleştirme (tüm içeriğini değiştirin):</span><span class="sxs-lookup"><span data-stu-id="03799-126">Update _LoginPartial.cshtml with the following code (replace all of its contents):</span></span>

```cshtml
@inject SignInManager<User> SignInManager
@inject UserManager<User> UserManager

@if (SignInManager.IsSignedIn(User))
{
    <form asp-area="" asp-controller="Account" asp-action="LogOff" method="post" id="logoutForm" class="navbar-right">
        <ul class="nav navbar-nav navbar-right">
            <li>
                <a asp-area="" asp-controller="Manage" asp-action="Index" title="Manage">Hello @UserManager.GetUserName(User)!</a>
            </li>
            <li>
                <button type="submit" class="btn btn-link navbar-btn navbar-link">Log off</button>
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

<span data-ttu-id="03799-127">Bu noktada, tarayıcınızda siteyi Yenile yapabiliyor olmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="03799-127">At this point, you should be able to refresh the site in your browser.</span></span>

## <a name="summary"></a><span data-ttu-id="03799-128">Özet</span><span class="sxs-lookup"><span data-stu-id="03799-128">Summary</span></span>

<span data-ttu-id="03799-129">ASP.NET Core değişiklikleri için ASP.NET Identity özellikler sunar.</span><span class="sxs-lookup"><span data-stu-id="03799-129">ASP.NET Core introduces changes to the ASP.NET Identity features.</span></span> <span data-ttu-id="03799-130">Bu makalede, bir ASP.NET Identity kimlik doğrulaması ve kullanıcı yönetim özelliklerine ASP.NET Core geçirmek nasıl gördünüz.</span><span class="sxs-lookup"><span data-stu-id="03799-130">In this article, you have seen how to migrate the authentication and user management features of an ASP.NET Identity to ASP.NET Core.</span></span>
