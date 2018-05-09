---
title: Kimlik doğrulama ve kimlik için ASP.NET Core geçirme
author: ardalis
description: Kimlik doğrulama ve kimlik bir ASP.NET MVC projesinde ASP.NET Core MVC projesinde geçirmek öğrenin.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/identity
ms.openlocfilehash: 2a80274e9056b41e370f199c7d41865db5fcedd7
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2018
---
# <a name="migrate-authentication-and-identity-to-aspnet-core"></a>Kimlik doğrulama ve kimlik için ASP.NET Core geçirme

Tarafından [Steve Smith](https://ardalis.com/)

Önceki makalede, biz [ASP.NET Core MVC için bir ASP.NET MVC projesinde yapılandırma geçişi](xref:migration/configuration). Bu makalede, kayıt, oturum açma ve kullanıcı yönetimi özellikleri geçirin.

## <a name="configure-identity-and-membership"></a>Kimlik ve üyelik yapılandırın.

ASP.NET MVC uygulamasında ASP.NET Identity'de kullanarak kimlik doğrulama ve kimlik özellikler yapılandırıldı *Startup.Auth.cs* ve *IdentityConfig.cs*, bulunan *App_Start* klasör. Bu özellikler yapılandırılan ASP.NET Core MVC'de *haline*.

Yükleme `Microsoft.AspNetCore.Identity.EntityFrameworkCore` ve `Microsoft.AspNetCore.Authentication.Cookies` NuGet paketleri.

Ardından, açın *haline* ve güncelleştirme `Startup.ConfigureServices` Entity Framework ve kimlik Hizmetleri kullanılacak yöntemi:

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

Bu noktada, biz ASP.NET MVC projeden henüz geçirilmeyen henüz Yukarıdaki kod başvurulan iki tür vardır: `ApplicationDbContext` ve `ApplicationUser`. Yeni bir *modelleri* ASP.NET Core klasöründe proje ve onu bu türlerine karşılık gelen iki sınıf ekleyin. ASP.NET MVC bu sınıfları sürümleri bulacaksınız */Models/IdentityModels.cs*, ancak daha açık olduğu için geçirilen projedeki başına bir dosyayı kullanacağız.

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
            // Customize the ASP.NET Identity model and override the defaults if needed.
            // For example, you can rename the ASP.NET Identity table names and more.
            // Add your customizations after calling base.OnModelCreating(builder);
        }
    }
}
```

ASP.NET Core MVC Starter Web projesi kullanıcıların kadar özelleştirme içermeyen veya `ApplicationDbContext`. Gerçek bir uygulama geçirirken, ayrıca tüm özel özellikleri ve yöntemleri, uygulamanızın kullanıcının geçiş yapmanız ve `DbContext` sınıfları yanı sıra, uygulamanızı kullanan diğer Model sınıfları. Örneğin, varsa, `DbContext` sahip bir `DbSet<Album>`, geçiş yapmanız `Album` sınıfı.

Bu dosyalar varken, sahip *haline* güncelleştirerek derlemek için dosya yapılabilir kendi `using` deyimleri:

```csharp
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Identity;
using Microsoft.AspNetCore.Hosting;
using Microsoft.EntityFrameworkCore;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.DependencyInjection;
```

Bizim uygulama artık kimlik doğrulaması ve kimlik hizmetlerini desteklemek hazırdır. Bunu yalnızca bu özellikleri kullanıcılara açık olmalıdır.

## <a name="migrate-registration-and-login-logic"></a>Kayıt ve oturum açma mantığı geçirme

Uygulama için yapılandırılmış kimlik Hizmetleri ve Entity Framework ve SQL Server kullanarak yapılandırılmış veri erişimi, biz kayıt ve oturum açma desteğini uygulamaya eklemek hazırsınız. Sözcüğünün [geçiş sürecinde önceki](xref:migration/mvc#migrate-the-layout-file) biz başvuru kılınmıştır *_LoginPartial* içinde *_Layout.cshtml*. Şimdi bu kod, dönüş açıklama durumundan çıkarmanız ve gerekli denetleyicileri ve oturum açma işlevselliği destekleyecek şekilde görünümlerde ekleme zamanı geldi.

Açıklamadan çıkarın `@Html.Partial` satırından *_Layout.cshtml*:

```cshtml
      <li>@Html.ActionLink("Contact", "Contact", "Home")</li>
    </ul>
    @*@Html.Partial("_LoginPartial")*@
  </div>
</div>
```

Şimdi, adlı yeni bir Razor Görünüm Ekle *_LoginPartial* için *görünümler/paylaşılan* klasörü:

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

Bu noktada, tarayıcınızda siteyi Yenile yapabiliyor olmanız gerekir.

## <a name="summary"></a>Özet

ASP.NET Core değişiklikleri için ASP.NET Identity özellikler sunar. Bu makalede, ASP.NET Identity kimlik doğrulaması ve kullanıcı yönetim özelliklerine ASP.NET Core geçirmek nasıl gördünüz.
