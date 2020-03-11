---
title: ASP.NET Core 'da kimlik modeli özelleştirmesi
author: ajcvickers
description: Bu makalede, ASP.NET Core kimliği için temel Entity Framework Core veri modelinin nasıl özelleştirileceği açıklanır.
ms.author: avickers
ms.date: 07/01/2019
uid: security/authentication/customize_identity_model
ms.openlocfilehash: f549fdff4a416b5fadcb2b1078b051bbab8e402e
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78656081"
---
# <a name="identity-model-customization-in-aspnet-core"></a>ASP.NET Core 'da kimlik modeli özelleştirmesi

[Arthur Vicranlar](https://github.com/ajcvickers) tarafından

ASP.NET Core kimlik, ASP.NET Core uygulamalarda Kullanıcı hesaplarını yönetmek ve depolamak için bir çerçeve sağlar. Kimlik doğrulama mekanizması olarak **bireysel kullanıcı hesapları** seçildiğinde, projenize kimlik eklenir. Varsayılan olarak, kimlik Entity Framework (EF) temel veri modelini kullanır. Bu makalede kimlik modelinin nasıl özelleştirileceği açıklanır.

## <a name="identity-and-ef-core-migrations"></a>Kimlik ve EF Core geçişleri

Modeli incelemeden önce, bir veritabanı oluşturmak ve güncelleştirmek için kimlik [EF Core geçişlerle](/ef/core/managing-schemas/migrations/) nasıl çalıştığını anlamak yararlı olur. En üst düzeyde, işlem şu şekilde yapılır:

1. [Kodda bir veri modeli](/ef/core/modeling/)tanımlayın veya güncelleştirin.
1. Bu modeli veritabanına uygulanabilecek değişikliklere dönüştürmek için bir geçiş ekleyin.
1. Geçişin, amaclarınızı doğru şekilde temsil ettiğini denetleyin.
1. Veritabanını modeliyle eşitlenmiş olacak şekilde güncelleştirmek için geçişi uygulayın.
1. Modeli daha belirginleştirmek ve veritabanını eşitlenmiş halde tutmak için 1 ile 4 arasındaki adımları tekrarlayın.

Geçişleri eklemek ve uygulamak için aşağıdaki yaklaşımlardan birini kullanın:

* Visual Studio kullanıyorsanız **Paket Yöneticisi konsolu** (PMC) penceresi. Daha fazla bilgi için bkz. [EF Core PMC araçları](/ef/core/miscellaneous/cli/powershell).
* Komut satırı kullanılıyorsa .NET Core CLI. Daha fazla bilgi için bkz. [.NET komut satırı araçları EF Core](/ef/core/miscellaneous/cli/dotnet).
* Uygulama çalıştırıldığında hata sayfasındaki **geçişleri Uygula** düğmesine tıklanın.

ASP.NET Core bir geliştirme zamanı hata sayfası işleyicisine sahiptir. İşleyici, uygulama çalıştırıldığında geçişleri uygulayabilir. Üretim uygulamaları tipik olarak geçişlerden SQL betikleri oluşturur ve veritabanı değişiklikleri denetimli bir uygulama ve veritabanı dağıtımının bir parçası olarak dağıtılır.

Kimlik kullanan yeni bir uygulama oluşturulduğunda, yukarıdaki 1. ve 2. adım zaten tamamlanmıştır. Diğer bir deyişle, ilk veri modeli zaten var ve ilk geçiş projeye eklendi. İlk geçişin hala veritabanına uygulanması gerekir. İlk geçiş aşağıdaki yaklaşımlardan biri aracılığıyla uygulanabilir:

* `Update-Database` PMC 'de çalıştırın.
* `dotnet ef database update` komut kabuğu 'nda çalıştırın.
* Uygulama çalıştırıldığında hata sayfasında **geçişleri Uygula** düğmesine tıklayın.

Modelde değişiklikler yapıldığından önceki adımları yineleyin.

## <a name="the-identity-model"></a>Kimlik modeli

### <a name="entity-types"></a>Varlık türleri

Kimlik modeli aşağıdaki varlık türlerinden oluşur.

|Varlık türü|Açıklama                                                  |
|-----------|-------------------------------------------------------------|
|`User`     |Kullanıcıyı temsil eder.                                         |
|`Role`     |Bir rolü temsil eder.                                           |
|`UserClaim`|Bir kullanıcının sahip olduğu talebi temsil eder.                    |
|`UserToken`|Bir kullanıcı için kimlik doğrulama belirtecini temsil eder.               |
|`UserLogin`|Kullanıcıyı bir oturum ile ilişkilendirir.                              |
|`RoleClaim`|Bir rol içindeki tüm kullanıcılara verilen bir talebi temsil eder.|
|`UserRole` |Kullanıcıları ve rolleri ilişkilendiren bir JOIN varlığı.               |

### <a name="entity-type-relationships"></a>Varlık türü ilişkileri

[Varlık türleri](#entity-types) , aşağıdaki yollarla birbirleriyle ilişkilidir:

* Her `User` birçok `UserClaims`olabilir.
* Her `User` birçok `UserLogins`olabilir.
* Her `User` birçok `UserTokens`olabilir.
* Her `Role` ilişkili birçok `RoleClaims`olabilir.
* Her `User` ilişkili birçok `Roles`olabilir ve her `Role` birçok `Users`ilişkilendirilebilir. Bu, veritabanında bir JOIN tablosu gerektiren çoktan çoğa bir ilişkidir. JOIN tablosu `UserRole` varlıkla temsil edilir.

### <a name="default-model-configuration"></a>Varsayılan model yapılandırması

Kimlik, modeli yapılandırmak ve kullanmak için [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) 'ten devraldığı birçok *bağlam sınıfını* tanımlar. Bu yapılandırma, bağlam sınıfının [Onmodeloluþturma](/dotnet/api/microsoft.entityframeworkcore.dbcontext.onmodelcreating) yönteminde [EF Core Code First floent API 'si](/ef/core/modeling/) kullanılarak yapılır. Varsayılan yapılandırma:

```csharp
builder.Entity<TUser>(b =>
{
    // Primary key
    b.HasKey(u => u.Id);

    // Indexes for "normalized" username and email, to allow efficient lookups
    b.HasIndex(u => u.NormalizedUserName).HasName("UserNameIndex").IsUnique();
    b.HasIndex(u => u.NormalizedEmail).HasName("EmailIndex");

    // Maps to the AspNetUsers table
    b.ToTable("AspNetUsers");

    // A concurrency token for use with the optimistic concurrency checking
    b.Property(u => u.ConcurrencyStamp).IsConcurrencyToken();

    // Limit the size of columns to use efficient database types
    b.Property(u => u.UserName).HasMaxLength(256);
    b.Property(u => u.NormalizedUserName).HasMaxLength(256);
    b.Property(u => u.Email).HasMaxLength(256);
    b.Property(u => u.NormalizedEmail).HasMaxLength(256);

    // The relationships between User and other entity types
    // Note that these relationships are configured with no navigation properties

    // Each User can have many UserClaims
    b.HasMany<TUserClaim>().WithOne().HasForeignKey(uc => uc.UserId).IsRequired();

    // Each User can have many UserLogins
    b.HasMany<TUserLogin>().WithOne().HasForeignKey(ul => ul.UserId).IsRequired();

    // Each User can have many UserTokens
    b.HasMany<TUserToken>().WithOne().HasForeignKey(ut => ut.UserId).IsRequired();

    // Each User can have many entries in the UserRole join table
    b.HasMany<TUserRole>().WithOne().HasForeignKey(ur => ur.UserId).IsRequired();
});

builder.Entity<TUserClaim>(b =>
{
    // Primary key
    b.HasKey(uc => uc.Id);

    // Maps to the AspNetUserClaims table
    b.ToTable("AspNetUserClaims");
});

builder.Entity<TUserLogin>(b =>
{
    // Composite primary key consisting of the LoginProvider and the key to use
    // with that provider
    b.HasKey(l => new { l.LoginProvider, l.ProviderKey });

    // Limit the size of the composite key columns due to common DB restrictions
    b.Property(l => l.LoginProvider).HasMaxLength(128);
    b.Property(l => l.ProviderKey).HasMaxLength(128);

    // Maps to the AspNetUserLogins table
    b.ToTable("AspNetUserLogins");
});

builder.Entity<TUserToken>(b =>
{
    // Composite primary key consisting of the UserId, LoginProvider and Name
    b.HasKey(t => new { t.UserId, t.LoginProvider, t.Name });

    // Limit the size of the composite key columns due to common DB restrictions
    b.Property(t => t.LoginProvider).HasMaxLength(maxKeyLength);
    b.Property(t => t.Name).HasMaxLength(maxKeyLength);

    // Maps to the AspNetUserTokens table
    b.ToTable("AspNetUserTokens");
});

builder.Entity<TRole>(b =>
{
    // Primary key
    b.HasKey(r => r.Id);

    // Index for "normalized" role name to allow efficient lookups
    b.HasIndex(r => r.NormalizedName).HasName("RoleNameIndex").IsUnique();

    // Maps to the AspNetRoles table
    b.ToTable("AspNetRoles");

    // A concurrency token for use with the optimistic concurrency checking
    b.Property(r => r.ConcurrencyStamp).IsConcurrencyToken();

    // Limit the size of columns to use efficient database types
    b.Property(u => u.Name).HasMaxLength(256);
    b.Property(u => u.NormalizedName).HasMaxLength(256);

    // The relationships between Role and other entity types
    // Note that these relationships are configured with no navigation properties

    // Each Role can have many entries in the UserRole join table
    b.HasMany<TUserRole>().WithOne().HasForeignKey(ur => ur.RoleId).IsRequired();

    // Each Role can have many associated RoleClaims
    b.HasMany<TRoleClaim>().WithOne().HasForeignKey(rc => rc.RoleId).IsRequired();
});

builder.Entity<TRoleClaim>(b =>
{
    // Primary key
    b.HasKey(rc => rc.Id);

    // Maps to the AspNetRoleClaims table
    b.ToTable("AspNetRoleClaims");
});

builder.Entity<TUserRole>(b =>
{
    // Primary key
    b.HasKey(r => new { r.UserId, r.RoleId });

    // Maps to the AspNetUserRoles table
    b.ToTable("AspNetUserRoles");
});
```

### <a name="model-generic-types"></a>Model genel türleri

Kimlik, yukarıda listelenen her varlık türü için varsayılan [ortak dil çalışma zamanı](/dotnet/standard/glossary#clr) (CLR) türlerini tanımlar. Bu türlerin öneki şu *kimliğe*sahiptir:

* `IdentityUser`
* `IdentityRole`
* `IdentityUserClaim`
* `IdentityUserToken`
* `IdentityUserLogin`
* `IdentityRoleClaim`
* `IdentityUserRole`

Bu türleri doğrudan kullanmak yerine, türler uygulamanın kendi türleri için temel sınıflar olarak kullanılabilir. Kimliğe göre tanımlanan `DbContext` sınıfları geneldir, bu nedenle modeldeki bir veya daha fazla varlık türü için farklı CLR türleri kullanılabilir. Bu genel türler Ayrıca `User` birincil anahtar (PK) veri türünün değiştirilmesine izin verir.

Rol desteğiyle kimlik kullanılırken bir <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext> sınıfı kullanılmalıdır. Örnek:

```csharp
// Uses all the built-in Identity types
// Uses `string` as the key type
public class IdentityDbContext
    : IdentityDbContext<IdentityUser, IdentityRole, string>
{
}

// Uses the built-in Identity types except with a custom User type
// Uses `string` as the key type
public class IdentityDbContext<TUser>
    : IdentityDbContext<TUser, IdentityRole, string>
        where TUser : IdentityUser
{
}

// Uses the built-in Identity types except with custom User and Role types
// The key type is defined by TKey
public class IdentityDbContext<TUser, TRole, TKey> : IdentityDbContext<
    TUser, TRole, TKey, IdentityUserClaim<TKey>, IdentityUserRole<TKey>,
    IdentityUserLogin<TKey>, IdentityRoleClaim<TKey>, IdentityUserToken<TKey>>
        where TUser : IdentityUser<TKey>
        where TRole : IdentityRole<TKey>
        where TKey : IEquatable<TKey>
{
}

// No built-in Identity types are used; all are specified by generic arguments
// The key type is defined by TKey
public abstract class IdentityDbContext<
    TUser, TRole, TKey, TUserClaim, TUserRole, TUserLogin, TRoleClaim, TUserToken>
    : IdentityUserContext<TUser, TKey, TUserClaim, TUserLogin, TUserToken>
         where TUser : IdentityUser<TKey>
         where TRole : IdentityRole<TKey>
         where TKey : IEquatable<TKey>
         where TUserClaim : IdentityUserClaim<TKey>
         where TUserRole : IdentityUserRole<TKey>
         where TUserLogin : IdentityUserLogin<TKey>
         where TRoleClaim : IdentityRoleClaim<TKey>
         where TUserToken : IdentityUserToken<TKey>
```

Rol olmadan (yalnızca talepler) kimlik kullanmak da mümkündür, bu durumda <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityUserContext%601> bir sınıf kullanılmalıdır:

```csharp
// Uses the built-in non-role Identity types except with a custom User type
// Uses `string` as the key type
public class IdentityUserContext<TUser>
    : IdentityUserContext<TUser, string>
        where TUser : IdentityUser
{
}

// Uses the built-in non-role Identity types except with a custom User type
// The key type is defined by TKey
public class IdentityUserContext<TUser, TKey> : IdentityUserContext<
    TUser, TKey, IdentityUserClaim<TKey>, IdentityUserLogin<TKey>,
    IdentityUserToken<TKey>>
        where TUser : IdentityUser<TKey>
        where TKey : IEquatable<TKey>
{
}

// No built-in Identity types are used; all are specified by generic arguments, with no roles
// The key type is defined by TKey
public abstract class IdentityUserContext<
    TUser, TKey, TUserClaim, TUserLogin, TUserToken> : DbContext
        where TUser : IdentityUser<TKey>
        where TKey : IEquatable<TKey>
        where TUserClaim : IdentityUserClaim<TKey>
        where TUserLogin : IdentityUserLogin<TKey>
        where TUserToken : IdentityUserToken<TKey>
{
}
```

## <a name="customize-the-model"></a>Modeli özelleştirme

Model özelleştirmesi için başlangıç noktası uygun bağlam türünden türetilmelidir. [Model genel türler](#model-generic-types) bölümüne bakın. Bu bağlam türü, geleneksel `ApplicationDbContext` ve ASP.NET Core şablonları tarafından oluşturulur.

Bağlam, modeli iki şekilde yapılandırmak için kullanılır:

* Genel tür parametreleri için varlık ve anahtar türleri sağlama.
* Bu türlerin eşlemesini değiştirmek için `OnModelCreating` geçersiz kılma.

`OnModelCreating`geçersiz kıldığınızda, önce `base.OnModelCreating` çağrılmalıdır; geçersiz kılan yapılandırma Next çağrılmalıdır. EF Core, yapılandırma için genellikle son bir WINS ilkesine sahiptir. Örneğin, bir varlık türü için `ToTable` yöntemi ilk olarak bir tablo adı ve daha sonra farklı bir tablo adıyla çağrılırsa ikinci çağrıda tablo adı kullanılır.

### <a name="custom-user-data"></a>Özel Kullanıcı verileri

<!--
set projNam=WebApp1
dotnet new webapp -o %projNam%
cd %projNam%
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design 
dotnet aspnet-codegenerator identity  -dc ApplicationDbContext --useDefaultUI 
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
 -->

[Özel Kullanıcı verileri](xref:security/authentication/add-user-data) `IdentityUser`devralınırken desteklenir. Bu türü `ApplicationUser`adlandırmak sizin için önemlidir:

```csharp
public class ApplicationUser : IdentityUser
{
    public string CustomTag { get; set; }
}
```

Bağlam için genel bir bağımsız değişken olarak `ApplicationUser` türünü kullanın:

```csharp
public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }

    protected override void OnModelCreating(ModelBuilder builder)
    {
        base.OnModelCreating(builder);
    }
}
```

`ApplicationDbContext` sınıfında `OnModelCreating` geçersiz kılmasına gerek yoktur. EF Core, `CustomTag` özelliğini kuralına göre eşler. Ancak, veritabanının yeni bir `CustomTag` sütunu oluşturacak şekilde güncelleştirilmesi gerekir. Sütunu oluşturmak için bir geçiş ekleyin ve ardından, [kimlik ve EF Core geçişleri](#identity-and-ef-core-migrations)bölümünde açıklandığı gibi veritabanını güncelleştirin.

*Sayfaları/paylaşılan/_LoginPartial. cshtml* 'yi güncelleştirin ve `IdentityUser` `ApplicationUser`ile değiştirin:

```cshtml
@using Microsoft.AspNetCore.Identity
@using WebApp1.Areas.Identity.Data
@inject SignInManager<ApplicationUser> SignInManager
@inject UserManager<ApplicationUser> UserManager
```

*Alanlarý/Identity/ıdentityhostingstartup. cs* veya `Startup.ConfigureServices` güncelleştirin ve `IdentityUser` `ApplicationUser`ile değiştirin.

```csharp
services.AddDefaultIdentity<ApplicationUser>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultUI();
```

ASP.NET Core 2,1 veya üzeri sürümlerde, kimlik Razor sınıf kitaplığı olarak sağlanır. Daha fazla bilgi için bkz. <xref:security/authentication/scaffold-identity>. Sonuç olarak, yukarıdaki kod <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>için bir çağrı gerektirir. Projeye kimlik dosyaları eklemek için Identity desteği kullanılmışsa `AddDefaultUI`çağrısını kaldırın. Daha fazla bilgi için bkz.

* [İskele Kimliği](xref:security/authentication/scaffold-identity)
* [Kimliğe özel kullanıcı verileri ekleyin, indirin ve silin](xref:security/authentication/add-user-data)

### <a name="change-the-primary-key-type"></a>Birincil anahtar türünü değiştirme

Veritabanı oluşturulduktan sonra PK sütununun veri türünün bir değişikliği birçok veritabanı sisteminde sorunlu olur. PK 'nin değiştirilmesi genellikle tabloyu bırakmayı ve yeniden oluşturmayı içerir. Bu nedenle, veritabanı oluşturulduğunda ilk geçişte anahtar türleri belirtilmelidir.

PK türünü değiştirmek için şu adımları izleyin:

1. Veritabanı PK değişikliğinden önce oluşturulduysa, silmek için `Drop-Database` (PMC) veya `dotnet ef database drop` (.NET Core CLI) çalıştırın.
2. Veritabanını silme işlemini onayladıktan sonra, `Remove-Migration` (PMC) veya `dotnet ef migrations remove` (.NET Core CLI) ile ilk geçişi kaldırın.
3. <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext%603>türetmek için `ApplicationDbContext` sınıfını güncelleştirin. `TKey`için yeni anahtar türünü belirtin. Örneğin, bir `Guid` anahtar türü kullanmak için:

    ```csharp
    public class ApplicationDbContext
        : IdentityDbContext<IdentityUser<Guid>, IdentityRole<Guid>, Guid>
    {
        public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
            : base(options)
        {
        }
    }
    ```

    ::: moniker range=">= aspnetcore-2.0"

    Yukarıdaki kodda, yeni anahtar türünü kullanmak için <xref:Microsoft.AspNetCore.Identity.IdentityUser%601> ve <xref:Microsoft.AspNetCore.Identity.IdentityRole%601> genel sınıfların belirtilmesi gerekir.

    ::: moniker-end

    ::: moniker range="<= aspnetcore-1.1"

    Yukarıdaki kodda, yeni anahtar türünü kullanmak için <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityUser%601> ve <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityRole%601> genel sınıfların belirtilmesi gerekir.

    ::: moniker-end

    `Startup.ConfigureServices` genel kullanıcıyı kullanacak şekilde güncelleştirilmeleri gerekir:

    ::: moniker range=">= aspnetcore-2.1"

    ```csharp
    services.AddDefaultIdentity<IdentityUser<Guid>>()
            .AddEntityFrameworkStores<ApplicationDbContext>()
            .AddDefaultTokenProviders();
    ```

    ::: moniker-end

    ::: moniker range="= aspnetcore-2.0"

    ```csharp
    services.AddIdentity<IdentityUser<Guid>, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext>()
            .AddDefaultTokenProviders();
    ```

    ::: moniker-end

    ::: moniker range="<= aspnetcore-1.1"

    ```csharp
    services.AddIdentity<IdentityUser<Guid>, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext, Guid>()
            .AddDefaultTokenProviders();
    ```

    ::: moniker-end

4. Özel bir `ApplicationUser` sınıfı kullanılıyorsa, sınıfı `IdentityUser`devralacak şekilde güncelleştirin. Örnek:

    ::: moniker range="<= aspnetcore-1.1"

    [!code-csharp[](customize-identity-model/samples/1.1/MvcSampleApp/Models/ApplicationUser.cs?name=snippet_ApplicationUser&highlight=4)]

    ::: moniker-end

    ::: moniker range=">= aspnetcore-2.0"

    [!code-csharp[](customize-identity-model/samples/2.1/RazorPagesSampleApp/Data/ApplicationUser.cs?name=snippet_ApplicationUser&highlight=4)]

    ::: moniker-end

    Özel `ApplicationUser` sınıfına başvurmak için `ApplicationDbContext` güncelleştirin:

    ```csharp
    public class ApplicationDbContext
        : IdentityDbContext<ApplicationUser, IdentityRole<Guid>, Guid>
    {
        public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
            : base(options)
        {
        }
    }
    ```

    Kimlik hizmetini `Startup.ConfigureServices`eklerken özel veritabanı bağlamı sınıfını kaydedin:

    ::: moniker range=">= aspnetcore-2.1"

    ```csharp
    services.AddDefaultIdentity<ApplicationUser>()
            .AddEntityFrameworkStores<ApplicationDbContext>()
            .AddDefaultUI()
            .AddDefaultTokenProviders();
    ```

    Birincil anahtarın veri türü, [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) nesnesi analiz edilirken algılanır.

    ASP.NET Core 2,1 veya üzeri sürümlerde, kimlik Razor sınıf kitaplığı olarak sağlanır. Daha fazla bilgi için bkz. <xref:security/authentication/scaffold-identity>. Sonuç olarak, yukarıdaki kod <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>için bir çağrı gerektirir. Projeye kimlik dosyaları eklemek için Identity desteği kullanılmışsa `AddDefaultUI`çağrısını kaldırın.

    ::: moniker-end

    ::: moniker range="= aspnetcore-2.0"

    ```csharp
    services.AddIdentity<ApplicationUser, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext>()
            .AddDefaultTokenProviders();
    ```

    Birincil anahtarın veri türü, [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) nesnesi analiz edilirken algılanır.

    ::: moniker-end

    ::: moniker range="<= aspnetcore-1.1"

    ```csharp
    services.AddIdentity<ApplicationUser, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext, Guid>()
            .AddDefaultTokenProviders();
    ```

    <xref:Microsoft.Extensions.DependencyInjection.IdentityEntityFrameworkBuilderExtensions.AddEntityFrameworkStores*> yöntemi, birincil anahtarın veri türünü gösteren bir `TKey` türü kabul eder.

    ::: moniker-end

5. Özel bir `ApplicationRole` sınıfı kullanılıyorsa, sınıfı `IdentityRole<TKey>`devralacak şekilde güncelleştirin. Örnek:

    [!code-csharp[](customize-identity-model/samples/2.1/RazorPagesSampleApp/Data/ApplicationRole.cs?name=snippet_ApplicationRole&highlight=4)]

    Özel `ApplicationRole` sınıfına başvurmak için `ApplicationDbContext` güncelleştirin. Örneğin, aşağıdaki sınıf özel bir `ApplicationUser` ve özel bir `ApplicationRole`başvurur:

    ::: moniker range=">= aspnetcore-2.1"

    [!code-csharp[](customize-identity-model/samples/2.1/RazorPagesSampleApp/Data/ApplicationDbContext.cs?name=snippet_ApplicationDbContext&highlight=5-6)]

    Kimlik hizmetini `Startup.ConfigureServices`eklerken özel veritabanı bağlamı sınıfını kaydedin:

    [!code-csharp[](customize-identity-model/samples/2.1/RazorPagesSampleApp/Startup.cs?name=snippet_ConfigureServices&highlight=13-16)]

    Birincil anahtarın veri türü, [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) nesnesi analiz edilirken algılanır.

    ASP.NET Core 2,1 veya üzeri sürümlerde, kimlik Razor sınıf kitaplığı olarak sağlanır. Daha fazla bilgi için bkz. <xref:security/authentication/scaffold-identity>. Sonuç olarak, yukarıdaki kod <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>için bir çağrı gerektirir. Projeye kimlik dosyaları eklemek için Identity desteği kullanılmışsa `AddDefaultUI`çağrısını kaldırın.

    ::: moniker-end

    ::: moniker range="= aspnetcore-2.0"

    [!code-csharp[](customize-identity-model/samples/2.0/RazorPagesSampleApp/Data/ApplicationDbContext.cs?name=snippet_ApplicationDbContext&highlight=5-6)]

    Kimlik hizmetini `Startup.ConfigureServices`eklerken özel veritabanı bağlamı sınıfını kaydedin:

    [!code-csharp[](customize-identity-model/samples/2.0/RazorPagesSampleApp/Startup.cs?name=snippet_ConfigureServices&highlight=7-9)]

    Birincil anahtarın veri türü, [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) nesnesi analiz edilirken algılanır.

    ::: moniker-end

    ::: moniker range="<= aspnetcore-1.1"

    [!code-csharp[](customize-identity-model/samples/1.1/MvcSampleApp/Data/ApplicationDbContext.cs?name=snippet_ApplicationDbContext&highlight=5-6)]

    Kimlik hizmetini `Startup.ConfigureServices`eklerken özel veritabanı bağlamı sınıfını kaydedin:

    [!code-csharp[](customize-identity-model/samples/1.1/MvcSampleApp/Startup.cs?name=snippet_ConfigureServices&highlight=7-9)]

    <xref:Microsoft.Extensions.DependencyInjection.IdentityEntityFrameworkBuilderExtensions.AddEntityFrameworkStores*> yöntemi, birincil anahtarın veri türünü gösteren bir `TKey` türü kabul eder.

    ::: moniker-end

### <a name="add-navigation-properties"></a>Gezinti özellikleri ekle

İlişkiler için model yapılandırmasının değiştirilmesi, başka değişiklikler yapmaktan daha zor olabilir. Yeni, ek ilişkiler oluşturmak yerine var olan ilişkilerin yerini almak için dikkatli olunmalıdır. Özellikle, değiştirilen ilişki var olan ilişki olarak aynı yabancı anahtar (FK) özelliğini belirtmelidir. Örneğin, `Users` ve `UserClaims` arasındaki ilişki varsayılan olarak aşağıdaki şekilde belirtilmiştir:

```csharp
builder.Entity<TUser>(b =>
{
    // Each User can have many UserClaims
    b.HasMany<TUserClaim>()
     .WithOne()
     .HasForeignKey(uc => uc.UserId)
     .IsRequired();
});
```

Bu ilişki için FK `UserClaim.UserId` özelliği olarak belirtilir. `HasMany` ve `WithOne`, gezinme özellikleri olmadan ilişki oluşturmak için bağımsız değişkenler olmadan çağrılır.

`ApplicationUser` ilişkili `UserClaims` kullanıcının başvuralmasına izin veren bir gezinti özelliği ekleyin:

```csharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<IdentityUserClaim<string>> Claims { get; set; }
}
```

`IdentityUserClaim<TKey>` için `TKey`, Kullanıcı PK için belirtilen türdür. Bu durumda, varsayılanlar kullanıldığından `TKey` `string`. `UserClaim` varlık türü için PK türü **değildir** .

Artık gezinti özelliği var olduğuna göre, `OnModelCreating`' de yapılandırılması gerekir:

```csharp
public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        base.OnModelCreating(modelBuilder);

        modelBuilder.Entity<ApplicationUser>(b =>
        {
            // Each User can have many UserClaims
            b.HasMany(e => e.Claims)
                .WithOne()
                .HasForeignKey(uc => uc.UserId)
                .IsRequired();
        });
    }
}
```

İlişkinin yalnızca `HasMany`çağrısında belirtilen bir gezinti özelliği ile, yalnızca daha önce olduğu gibi yapılandırıldığından emin olun.

Gezinti özellikleri, veritabanında değil yalnızca EF modelinde bulunur. İlişki için FK değişmediğinden, bu tür bir model değişikliği veritabanının güncelleştirilmesini gerektirmez. Bu, değişiklik yapıldıktan sonra bir geçiş eklenerek denetlenebilir. `Up` ve `Down` yöntemleri boş.

### <a name="add-all-user-navigation-properties"></a>Tüm kullanıcı gezinti özelliklerini Ekle

Aşağıdaki örnek, kılavuz olarak yukarıdaki bölümü kullanarak, Kullanıcı üzerindeki tüm ilişkiler için tek yönlü gezinti özelliklerini yapılandırır:

```csharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<IdentityUserClaim<string>> Claims { get; set; }
    public virtual ICollection<IdentityUserLogin<string>> Logins { get; set; }
    public virtual ICollection<IdentityUserToken<string>> Tokens { get; set; }
    public virtual ICollection<IdentityUserRole<string>> UserRoles { get; set; }
}
```

```csharp
public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        base.OnModelCreating(modelBuilder);

        modelBuilder.Entity<ApplicationUser>(b =>
        {
            // Each User can have many UserClaims
            b.HasMany(e => e.Claims)
                .WithOne()
                .HasForeignKey(uc => uc.UserId)
                .IsRequired();

            // Each User can have many UserLogins
            b.HasMany(e => e.Logins)
                .WithOne()
                .HasForeignKey(ul => ul.UserId)
                .IsRequired();

            // Each User can have many UserTokens
            b.HasMany(e => e.Tokens)
                .WithOne()
                .HasForeignKey(ut => ut.UserId)
                .IsRequired();

            // Each User can have many entries in the UserRole join table
            b.HasMany(e => e.UserRoles)
                .WithOne()
                .HasForeignKey(ur => ur.UserId)
                .IsRequired();
        });
    }
}
```

### <a name="add-user-and-role-navigation-properties"></a>Kullanıcı ve rol gezinti özellikleri ekleme

Aşağıdaki örnek, kılavuz olarak yukarıdaki bölümü kullanarak, Kullanıcı ve roldeki tüm ilişkiler için gezinti özelliklerini yapılandırır:

```csharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<IdentityUserClaim<string>> Claims { get; set; }
    public virtual ICollection<IdentityUserLogin<string>> Logins { get; set; }
    public virtual ICollection<IdentityUserToken<string>> Tokens { get; set; }
    public virtual ICollection<ApplicationUserRole> UserRoles { get; set; }
}

public class ApplicationRole : IdentityRole
{
    public virtual ICollection<ApplicationUserRole> UserRoles { get; set; }
}

public class ApplicationUserRole : IdentityUserRole<string>
{
    public virtual ApplicationUser User { get; set; }
    public virtual ApplicationRole Role { get; set; }
}
```

```csharp
public class ApplicationDbContext
    : IdentityDbContext<
        ApplicationUser, ApplicationRole, string,
        IdentityUserClaim<string>, ApplicationUserRole, IdentityUserLogin<string>,
        IdentityRoleClaim<string>, IdentityUserToken<string>>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        base.OnModelCreating(modelBuilder);

        modelBuilder.Entity<ApplicationUser>(b =>
        {
            // Each User can have many UserClaims
            b.HasMany(e => e.Claims)
                .WithOne()
                .HasForeignKey(uc => uc.UserId)
                .IsRequired();

            // Each User can have many UserLogins
            b.HasMany(e => e.Logins)
                .WithOne()
                .HasForeignKey(ul => ul.UserId)
                .IsRequired();

            // Each User can have many UserTokens
            b.HasMany(e => e.Tokens)
                .WithOne()
                .HasForeignKey(ut => ut.UserId)
                .IsRequired();

            // Each User can have many entries in the UserRole join table
            b.HasMany(e => e.UserRoles)
                .WithOne(e => e.User)
                .HasForeignKey(ur => ur.UserId)
                .IsRequired();
        });

        modelBuilder.Entity<ApplicationRole>(b =>
        {
            // Each Role can have many entries in the UserRole join table
            b.HasMany(e => e.UserRoles)
                .WithOne(e => e.Role)
                .HasForeignKey(ur => ur.RoleId)
                .IsRequired();
        });

    }
}
```

Notlar:

* Bu örnek ayrıca, kullanıcılardan rollere kadar çoktan çoğa ilişkiye gitmek için gereken `UserRole` JOIN varlığını da içerir.
* Gezinti özelliklerinin türlerini, `ApplicationXxx` türlerinin `IdentityXxx` türler yerine artık kullanıldığını yansıtacak şekilde değiştirmeyi unutmayın.
* Genel `ApplicationContext` tanımındaki `ApplicationXxx` kullanmayı unutmayın.

### <a name="add-all-navigation-properties"></a>Tüm gezinti özelliklerini Ekle

Aşağıdaki örnek, kılavuz olarak yukarıdaki bölümü kullanarak tüm varlık türlerindeki tüm ilişkiler için gezinti özelliklerini yapılandırır:

```csharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<ApplicationUserClaim> Claims { get; set; }
    public virtual ICollection<ApplicationUserLogin> Logins { get; set; }
    public virtual ICollection<ApplicationUserToken> Tokens { get; set; }
    public virtual ICollection<ApplicationUserRole> UserRoles { get; set; }
}

public class ApplicationRole : IdentityRole
{
    public virtual ICollection<ApplicationUserRole> UserRoles { get; set; }
    public virtual ICollection<ApplicationRoleClaim> RoleClaims { get; set; }
}

public class ApplicationUserRole : IdentityUserRole<string>
{
    public virtual ApplicationUser User { get; set; }
    public virtual ApplicationRole Role { get; set; }
}

public class ApplicationUserClaim : IdentityUserClaim<string>
{
    public virtual ApplicationUser User { get; set; }
}

public class ApplicationUserLogin : IdentityUserLogin<string>
{
    public virtual ApplicationUser User { get; set; }
}

public class ApplicationRoleClaim : IdentityRoleClaim<string>
{
    public virtual ApplicationRole Role { get; set; }
}

public class ApplicationUserToken : IdentityUserToken<string>
{
    public virtual ApplicationUser User { get; set; }
}
```

```csharp
public class ApplicationDbContext
    : IdentityDbContext<
        ApplicationUser, ApplicationRole, string,
        ApplicationUserClaim, ApplicationUserRole, ApplicationUserLogin,
        ApplicationRoleClaim, ApplicationUserToken>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        base.OnModelCreating(modelBuilder);

        modelBuilder.Entity<ApplicationUser>(b =>
        {
            // Each User can have many UserClaims
            b.HasMany(e => e.Claims)
                .WithOne(e => e.User)
                .HasForeignKey(uc => uc.UserId)
                .IsRequired();

            // Each User can have many UserLogins
            b.HasMany(e => e.Logins)
                .WithOne(e => e.User)
                .HasForeignKey(ul => ul.UserId)
                .IsRequired();

            // Each User can have many UserTokens
            b.HasMany(e => e.Tokens)
                .WithOne(e => e.User)
                .HasForeignKey(ut => ut.UserId)
                .IsRequired();

            // Each User can have many entries in the UserRole join table
            b.HasMany(e => e.UserRoles)
                .WithOne(e => e.User)
                .HasForeignKey(ur => ur.UserId)
                .IsRequired();
        });

        modelBuilder.Entity<ApplicationRole>(b =>
        {
            // Each Role can have many entries in the UserRole join table
            b.HasMany(e => e.UserRoles)
                .WithOne(e => e.Role)
                .HasForeignKey(ur => ur.RoleId)
                .IsRequired();

            // Each Role can have many associated RoleClaims
            b.HasMany(e => e.RoleClaims)
                .WithOne(e => e.Role)
                .HasForeignKey(rc => rc.RoleId)
                .IsRequired();
        });
    }
}
```

### <a name="use-composite-keys"></a>Bileşik anahtarlar kullanın

Önceki bölümlerde, kimlik modelinde kullanılan anahtar türünü değiştirme gösterilmiştir. Kimlik anahtarı modelinin bileşik anahtarları kullanacak şekilde değiştirilmesi desteklenmez veya önerilmez. Bir bileşik anahtarın kimlik ile kullanılması, Identity Manager kodunun modelle nasıl etkileşime gireceğini değiştirmenizi içerir. Bu özelleştirme, bu belgenin kapsamı dışındadır.

### <a name="change-tablecolumn-names-and-facets"></a>Tablo/sütun adlarını ve modelleri değiştirme

Tablo ve sütunların adlarını değiştirmek için `base.OnModelCreating`çağırın. Ardından, varsayılan ayarları geçersiz kılmak için yapılandırma ekleyin. Örneğin, tüm kimlik tablolarının adını değiştirmek için:

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    base.OnModelCreating(modelBuilder);

    modelBuilder.Entity<IdentityUser>(b =>
    {
        b.ToTable("MyUsers");
    });

    modelBuilder.Entity<IdentityUserClaim<string>>(b =>
    {
        b.ToTable("MyUserClaims");
    });

    modelBuilder.Entity<IdentityUserLogin<string>>(b =>
    {
        b.ToTable("MyUserLogins");
    });

    modelBuilder.Entity<IdentityUserToken<string>>(b =>
    {
        b.ToTable("MyUserTokens");
    });

    modelBuilder.Entity<IdentityRole>(b =>
    {
        b.ToTable("MyRoles");
    });

    modelBuilder.Entity<IdentityRoleClaim<string>>(b =>
    {
        b.ToTable("MyRoleClaims");
    });

    modelBuilder.Entity<IdentityUserRole<string>>(b =>
    {
        b.ToTable("MyUserRoles");
    });
}
```

Bu örnekler varsayılan kimlik türlerini kullanır. `ApplicationUser`gibi bir uygulama türü kullanıyorsanız, varsayılan tür yerine bu türü yapılandırın.

Aşağıdaki örnek bazı sütun adlarını değiştirir:

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    base.OnModelCreating(modelBuilder);

    modelBuilder.Entity<IdentityUser>(b =>
    {
        b.Property(e => e.Email).HasColumnName("EMail");
    });

    modelBuilder.Entity<IdentityUserClaim<string>>(b =>
    {
        b.Property(e => e.ClaimType).HasColumnName("CType");
        b.Property(e => e.ClaimValue).HasColumnName("CValue");
    });
}
```

Bazı veritabanı sütunları türleri belirli *modellerle* yapılandırılabilir (örneğin, izin verilen en fazla `string` uzunluğu). Aşağıdaki örnek, modeldeki birkaç `string` özelliği için en fazla sütun uzunluğunu ayarlar:

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    base.OnModelCreating(modelBuilder);

    modelBuilder.Entity<IdentityUser>(b =>
    {
        b.Property(u => u.UserName).HasMaxLength(128);
        b.Property(u => u.NormalizedUserName).HasMaxLength(128);
        b.Property(u => u.Email).HasMaxLength(128);
        b.Property(u => u.NormalizedEmail).HasMaxLength(128);
    });

    modelBuilder.Entity<IdentityUserToken<string>>(b =>
    {
        b.Property(t => t.LoginProvider).HasMaxLength(128);
        b.Property(t => t.Name).HasMaxLength(128);
    });
}
```

### <a name="map-to-a-different-schema"></a>Farklı bir şemaya eşleme

Şemalar, veritabanı sağlayıcıları genelinde farklı davranabilir. SQL Server için varsayılan, *dbo* şemasında tüm tabloları oluşturmaktır. Tablolar farklı bir şemada oluşturulabilir. Örnek:

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    base.OnModelCreating(modelBuilder);

    modelBuilder.HasDefaultSchema("notdbo");
}
```

::: moniker range=">= aspnetcore-2.1"

### <a name="lazy-loading"></a>geç yükleme

Bu bölümde, kimlik modelindeki yavaş yükleme proxy 'leri için destek eklenmiştir. Yavaş yükleme, gezinti özelliklerinin önce yüklendiklerinden emin olmadan kullanılmasına izin verdiğinden yararlıdır.

Varlık türleri, [EF Core belgelerinde](/ef/core/querying/related-data#lazy-loading)açıklandığı gibi çeşitli yollarla yavaş yükleme için uygun hale getirilebilir. Basitlik için, aşağıdakileri gerektiren yavaş yükleme proxy 'leri kullanın:

* [Microsoft. EntityFrameworkCore. proxy](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) paketi yüklemesi.
* [Adddbcontext\<tcontext >](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext)içinde <xref:Microsoft.EntityFrameworkCore.ProxiesExtensions.UseLazyLoadingProxies*> çağrısı.
* `public virtual` gezinti özelliklerine sahip ortak varlık türleri.

Aşağıdaki örnek `UseLazyLoadingProxies` `Startup.ConfigureServices`çağırma gösterilmektedir:

```csharp
services
    .AddDbContext<ApplicationDbContext>(
        b => b.UseSqlServer(connectionString)
              .UseLazyLoadingProxies())
    .AddDefaultIdentity<ApplicationUser>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

Varlık türlerine gezinti özellikleri ekleme hakkında rehberlik için yukarıdaki örneklere bakın.

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:security/authentication/scaffold-identity>

::: moniker-end
