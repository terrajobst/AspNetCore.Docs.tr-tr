---
title: "Geçirme kimlik doğrulama ve kimlik"
author: ardalis
description: 
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/identity
ms.openlocfilehash: f02d9472ea0aa1dceae3f53c812776aab85ab54e
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2018
---
# <a name="migrating-authentication-and-identity"></a>Geçirme kimlik doğrulama ve kimlik

<a name="migration-identity"></a>

Tarafından [Steve Smith](https://ardalis.com/)

Önceki makalesi biz [ASP.NET Core MVC için bir ASP.NET MVC projesinde yapılandırma geçişi](configuration.md). Bu makalede, kayıt, oturum açma ve kullanıcı yönetimi özellikleri geçirin.

## <a name="configure-identity-and-membership"></a>Kimlik ve üyelik yapılandırın.

ASP.NET MVC uygulamasında ASP.NET Identity Startup.Auth.cs ve App_Start klasöründe IdentityConfig.cs kullanarak kimlik doğrulama ve kimlik özellikler yapılandırıldı. Bu özellikler yapılandırılan ASP.NET Core MVC'de *haline*.

Yükleme `Microsoft.AspNetCore.Identity.EntityFrameworkCore` ve `Microsoft.AspNetCore.Authentication.Cookies` NuGet paketleri.

Ardından, haline ve güncelleştirme açın `ConfigureServices()` Entity Framework ve kimlik Hizmetleri kullanılacak yöntemi:

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

Bu noktada, biz ASP.NET MVC projeden henüz geçirilmeyen henüz Yukarıdaki kod başvurulan iki tür vardır: `ApplicationDbContext` ve `ApplicationUser`. Yeni bir *modelleri* ASP.NET Core klasöründe proje ve onu bu türlerine karşılık gelen iki sınıf ekleyin. ASP.NET MVC bu sınıfları sürümleri bulacaksınız `/Models/IdentityModels.cs`, ancak daha açık olduğu için geçirilen projedeki başına bir dosyayı kullanacağız.

ApplicationUser.cs:

```csharp
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;

namespace NewMvc6Project.Models
{
  public class ApplicationUser : IdentityUser
  {
  }
}
```

ApplicationDbContext.cs:

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

ASP.NET Core MVC Starter Web projesi kadar özelleştirme kullanıcı ya da ApplicationDbContext içermez. Gerçek bir uygulamada geçirirken, ayrıca tüm özel özellikleri ve yöntemleri, (örneğin, DbContext DbSetvarsa,uygulamanızınkullanandiğermodelisınıflarıyanısıra,uygulamanızınkullanıcıveDbContextsınıflarıgeçişyapmanızgerekecektir<Album>, Elbette albüm sınıfı geçiş yapmanız gerekecektir).

Bu dosyaları yerinde kendi kullanarak güncelleştirerek derlemek için haline dosya yapılabilmesi için deyimleri:

```csharp
using Microsoft.Framework.ConfigurationModel;
using Microsoft.AspNetCore.Hosting;
using NewMvc6Project.Models;
using Microsoft.AspNetCore.Identity;
```

Uygulamamız, kimlik doğrulama ve kimlik hizmetlerini desteklemeye hazırdır -, yalnızca bu özellikleri kullanıcılara açık olması gerekir.

## <a name="migrate-registration-and-login-logic"></a>Kayıt ve oturum açma mantığı geçirme

Uygulama ve Entity Framework ve SQL Server kullanarak yapılandırılmış veri erişimi için yapılandırılmış kimlik Hizmetleri ile biz şimdi uygulama kayıt ve oturum açma desteği eklemek hazır olursunuz. Sözcüğünün [geçiş sürecinde önceki](mvc.md#migrate-layout-file) biz _Layout.cshtml _LoginPartial başvuru kılınmıştır. Şimdi bu kod, dönüş açıklama durumundan çıkarmanız ve gerekli denetleyicileri ve oturum açma işlevselliği destekleyecek şekilde görünümlerde ekleme zamanı geldi.

Güncelleştirme _Layout.cshtml; açıklamadan çıkarın @Html.Partial satır:

```cshtml
      <li>@Html.ActionLink("Contact", "Contact", "Home")</li>
    </ul>
    @*@Html.Partial("_LoginPartial")*@
  </div>
</div>
```

Şimdi, MVC görünümü görünümler/paylaşılan klasöre _LoginPartial adlı yeni bir sayfa ekleyin:

Aşağıdaki kod ile _LoginPartial.cshtml güncelleştirme (tüm içeriğini değiştirin):

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

Bu noktada, tarayıcınızda siteyi Yenile yapabiliyor olmanız gerekir.

## <a name="summary"></a>Özet

ASP.NET Core değişiklikleri için ASP.NET Identity özellikler sunar. Bu makalede, bir ASP.NET Identity kimlik doğrulaması ve kullanıcı yönetim özelliklerine ASP.NET Core geçirmek nasıl gördünüz.
