---
title: Kimlik doğrulaması ve kimliği ASP.NET Core geçir
author: ardalis
description: Bir ASP.NET MVC projesinden kimlik doğrulaması ve kimliği ASP.NET Core MVC projesine geçirmeyi öğrenin.
ms.author: riande
ms.date: 3/22/2020
uid: migration/identity
ms.openlocfilehash: c5727c974e455144d04e66fe14ea591e160cb963
ms.sourcegitcommit: 91dc1dd3d055b4c7d7298420927b3fd161067c64
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80219200"
---
# <a name="migrate-authentication-and-identity-to-aspnet-core"></a>Kimlik doğrulaması ve kimliği ASP.NET Core geçir

[Steve Smith](https://ardalis.com/) tarafından

Önceki makalede, [yapılandırmayı ASP.NET Core MVC 'ye bir ASP.NET MVC projesinden geçirdik](xref:migration/configuration). Bu makalede, kayıt, oturum açma ve Kullanıcı yönetimi özelliklerini geçiririz.

## <a name="configure-identity-and-membership"></a>Kimliği ve üyeliği yapılandırma

ASP.NET MVC 'de, kimlik doğrulama ve kimlik özellikleri, *App_Start* klasöründe bulunan *Startup.Auth.cs* ve *IdentityConfig.cs*ASP.NET Identity kullanılarak yapılandırılır. ASP.NET Core MVC 'de, bu özellikler *Startup.cs*' de yapılandırılır.

Aşağıdaki NuGet paketlerini yükler:

* `Microsoft.AspNetCore.Identity.EntityFrameworkCore`
* `Microsoft.AspNetCore.Authentication.Cookies`
* `Microsoft.EntityFrameworkCore.SqlServer`

*Startup.cs*' de, `Startup.ConfigureServices` yöntemini Entity Framework ve kimlik hizmetlerini kullanacak şekilde güncelleştirin:

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

Bu noktada, yukarıdaki kodda henüz ASP.NET MVC projesinden geçirdiğimiz iki tür vardır: `ApplicationDbContext` ve `ApplicationUser`. ASP.NET Core projesinde yeni *modeller* klasörü oluşturun ve bu türlere karşılık gelen kendisine iki sınıf ekleyin. Bu sınıfların ASP.NET MVC sürümlerini */models/ıdentitymodels.cs*içinde bulacaksınız, ancak geçirilen projede sınıf başına bir dosya kullanacağız çünkü bu daha net bir şekilde yapılır.

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
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;
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

ASP.NET Core MVC Başlatıcı Web projesi kullanıcıların çok fazla özelleştirmesini veya `ApplicationDbContext`içermez. Gerçek bir uygulamayı geçirirken uygulamanızın kullanıcı ve `DbContext` sınıflarının tüm özel özelliklerini ve yöntemlerini, uygulamanızın de sahip olduğu diğer model sınıflarını geçirmeniz gerekir. Örneğin, `DbContext` bir `DbSet<Album>`varsa `Album` sınıfını geçirmeniz gerekir.

Bu dosyalarla birlikte, *Startup.cs* dosyası `using` deyimleri güncelleştirilerek derlenmek üzere yapılabilir:

```csharp
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Identity;
using Microsoft.AspNetCore.Hosting;
using Microsoft.EntityFrameworkCore;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.DependencyInjection;
```

Uygulamamız artık kimlik doğrulama ve kimlik hizmetlerini desteklemeye hazırdır. Yalnızca bu özelliklerin kullanıcılara açık olması gerekir.

## <a name="migrate-registration-and-login-logic"></a>Kayıt ve oturum açma mantığını geçirme

Uygulama için yapılandırılmış kimlik hizmetleri ve Entity Framework ve SQL Server kullanılarak yapılandırılan veri erişimi sayesinde, kayıt ve uygulamaya oturum açma desteği eklemeye hazırız. [Daha önce geçiş sürecinde,](xref:migration/mvc#migrate-the-layout-file) *_Layout. cshtml*içindeki *_LoginPartial* bir başvuruyu yorumlayacağız. Artık bu koda geri dönmeli, bu kodun açıklamasını kaldırın ve oturum açma işlevlerini desteklemek için gerekli denetleyiciler ve görünümlerde ekleme zamanı vardır.

*_Layout. cshtml*'deki `@Html.Partial` satırının açıklamasını kaldırın:

```cshtml
      <li>@Html.ActionLink("Contact", "Contact", "Home")</li>
    </ul>
    @*@Html.Partial("_LoginPartial")*@
  </div>
</div>
```

Şimdi, *Görünümler/paylaşılan* klasöre *_LoginPartial* adlı yeni bir Razor görünümü ekleyin:

*_LoginPartial. cshtml* 'yi aşağıdaki kodla güncelleştirin (tüm içeriğini değiştirin):

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

Bu noktada, tarayıcıda siteyi yenileyebilmelisiniz.

## <a name="summary"></a>Özet

ASP.NET Core, ASP.NET Identity özelliklerinde yapılan değişiklikleri tanıtır. Bu makalede, ASP.NET Identity kimlik doğrulaması ve Kullanıcı Yönetimi özelliklerinin ASP.NET Core 'ye nasıl geçirileceğiyle karşılaşmış olursunuz.
