---
title: Kimlik doğrulaması ve kimlik için ASP.NET Core geçişi
author: ardalis
description: Kimlik doğrulaması ve kimlik, bir ASP.NET Core MVC projesini ASP.NET MVC projesinde geçirmeyi öğrenin.
ms.author: riande
ms.date: 10/14/2016
uid: migration/identity
ms.openlocfilehash: 72e62e78e37325ec47d54abbc11a875ae87fb63a
ms.sourcegitcommit: b72bbc9ae91e4bd37c9ea9b2d09ebf47afb25dd7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/11/2019
ms.locfileid: "41755674"
---
# <a name="migrate-authentication-and-identity-to-aspnet-core"></a>Kimlik doğrulaması ve kimlik için ASP.NET Core geçişi

Tarafından [Steve Smith](https://ardalis.com/)

Önceki makalede, biz [yapılandırma, ASP.NET Core MVC için ASP.NET MVC projesinde geçişi](xref:migration/configuration). Bu makalede, biz kaydı, oturum açma ve kullanıcı yönetimi özellikleri geçirin.

## <a name="configure-identity-and-membership"></a>Kimlik ve üyelik yapılandırın

ASP.NET MVC, ASP.NET Identity'de kullanarak kimlik doğrulaması ve kimlik özellikler yapılandırıldı *Startup.Auth.cs* ve *IdentityConfig.cs*, bulunan *App_Start* klasör. Bu özellikler yapılandırılan ASP.NET Core MVC *Startup.cs*.

Yükleme `Microsoft.AspNetCore.Identity.EntityFrameworkCore` ve `Microsoft.AspNetCore.Authentication.Cookies` NuGet paketleri.

Ardından, açın *Startup.cs* ve güncelleştirme `Startup.ConfigureServices` Entity Framework ve kimlik Hizmetleri yöntemi:

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

Bu noktada, biz ASP.NET MVC projeden henüz geçirilmeyen henüz yukarıdaki kodda başvurulan iki tür vardır: `ApplicationDbContext` ve `ApplicationUser`. Yeni bir *modelleri* klasöründe ASP.NET Core projesi ve ona bu türlerine karşılık gelen iki sınıf ekleyin. ASP.NET MVC, bu sınıfları sürümlerini bulabilirsiniz */Models/IdentityModels.cs*, ancak, daha açık olduğundan geçirilen projedeki her bir dosya kullanacağız.

*ApplicationUser.cs*:

```csharp
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;

namespace NewMvcProject.Models
{
  public class ApplicationUser : IdentityUser
  {
  }
}
```

*ApplicationDbContext.cs*:

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

ASP.NET Core MVC başlangıç Web Proje kullanıcı kadar özelleştirme içermeyen veya `ApplicationDbContext`. Gerçek bir uygulamada geçirirken, ayrıca tüm özel özellikleri ve yöntemleri, uygulamanızın kullanıcının geçmeniz ve `DbContext` sınıflarının yanı sıra, uygulamanızı kullanan başka bir Model sınıfları. Örneğin, varsa, `DbContext` sahip bir `DbSet<Album>`, taşımaya gerek `Album` sınıfı.

Bu dosyaları yerinde, ile *Startup.cs* dosyasını güncelleştirerek derlemek için yapılabilir, `using` ifadeleri:

```csharp
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Identity;
using Microsoft.AspNetCore.Hosting;
using Microsoft.EntityFrameworkCore;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.DependencyInjection;
```

Uygulamamızı kimlik doğrulaması ve kimlik Hizmetleri desteklemek artık hazırdır. Bunu yalnızca kullanıcılara açık hale bu özelliklere sahip olması gerekir.

## <a name="migrate-registration-and-login-logic"></a>Kayıt ve oturum açma mantığı geçirme

Kimlik Hizmetleri uygulama için yapılandırılmış ve yapılandırılmış Entity Framework ve SQL Server'ı kullanarak veri erişimi, uygulamaya kayıt ve oturum açma desteği eklemek hazırız. Sözcüğünün [önceki geçiş sürecinde](xref:migration/mvc#migrate-the-layout-file) biz başvuru yorum *_LoginPartial* içinde *_Layout.cshtml*. Artık bu kod, iade açıklamasını kaldırın ve gerekli denetleyicileri veya oturum açma işlevlerini desteklemek için Görünüm ekleme zamanı geldi.

Açıklamadan çıkarın `@Html.Partial` satırına *_Layout.cshtml*:

```cshtml
      <li>@Html.ActionLink("Contact", "Contact", "Home")</li>
    </ul>
    @*@Html.Partial("_LoginPartial")*@
  </div>
</div>
```

Adlı yeni bir Razor görünüm şimdi ekleyin *_LoginPartial* için *görünümler/paylaşılan* klasörü:

Güncelleştirme *_LoginPartial.cshtml* aşağıdaki kodla (tüm içeriğini değiştirin):

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

Bu noktada, sitenin tarayıcınızda yenileyemiyoruz olmalıdır.

## <a name="summary"></a>Özet

ASP.NET Core ASP.NET Identity özellikleri için değişiklikler yapılmıştır. Bu makalede, ASP.NET Core kimlik doğrulaması ve kullanıcı yönetim özelliklerine ASP.NET Identity'ye geçirme gördünüz.
