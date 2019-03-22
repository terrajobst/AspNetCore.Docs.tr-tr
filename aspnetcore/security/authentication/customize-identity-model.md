---
title: ASP.NET core'da kimlik modeli özelleştirme
author: ajcvickers
description: Bu makalede, ASP.NET Core kimliği için Entity Framework Core veri modeli özelleştirmeyi açıklar.
ms.author: avickers
ms.date: 09/24/2018
uid: security/authentication/customize_identity_model
ms.openlocfilehash: 0aa7448ac37a97a4d09a04caf365f641f22f5997
ms.sourcegitcommit: a1c43150ed46aa01572399e8aede50d4668745ca
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/21/2019
ms.locfileid: "58327307"
---
# <a name="identity-model-customization-in-aspnet-core"></a>ASP.NET core'da kimlik modeli özelleştirme

Tarafından [Arthur Vickers](https://github.com/ajcvickers)

ASP.NET Core kimlik yönetimi ve ASP.NET Core uygulamalarında kullanıcı hesaplarını depolamak için bir çerçeve sunar. Kimlik, projenize eklenir, **bireysel kullanıcı hesapları** kimlik doğrulama mekanizması olarak seçilir. Varsayılan olarak, kimlik bir Entity Framework (EF), yararlanır çekirdek veri modeli. Bu makalede, kimlik modeli özelleştirmeyi açıklar.

## <a name="identity-and-ef-core-migrations"></a>Kimlik ve EF Core geçişleri

Model İnceleme önce kimlik'ile nasıl çalıştığını anlamak kullanışlıdır [EF Core geçişleri](/ef/core/managing-schemas/migrations/) bir veritabanı oluşturmak için. En üst düzeyinde bir işlemdir:

1. Tanımlayın veya güncelleştirme bir [kod veri modelinde](/ef/core/modeling/).
1. Bu model, veritabanına uygulanabilir değişiklikleri küçültmesini bir geçiş ekleyin.
1. Onay geçiş amacınızı doğru şekilde temsil eder.
1. Geçiş modeli ile eşitlenmiş olmasını veritabanını güncellemek için geçerlidir.
1. 1 ile daha fazla model iyileştirmek ve veritabanı eşitlenmiş halde tutun için 4 arasındaki adımları yineleyin.

Ekleme ve geçiş uygulamak için aşağıdaki yaklaşımlardan birini kullanın:

* **Paket Yöneticisi Konsolu** Visual Studio kullanıyorsanız (PMC) penceresi. Daha fazla bilgi için [EF Core PMC Araçları](/ef/core/miscellaneous/cli/powershell).
* .NET Core komut satırı kullanılarak, CLI. Daha fazla bilgi için [EF Core .NET komut satırı araçları](/ef/core/miscellaneous/cli/dotnet).
* Tıklayarak **geçerli geçişleri** düğmesi uygulamayı çalıştırdığınızda hata sayfasında.

ASP.NET Core geliştirme zamanı hata sayfası işleyicisine sahiptir. Uygulamayı çalıştırdığınızda, işleyici geçişler uygulayabilirsiniz. Üretim uygulamaları için genellikle daha fazla olur geçişleri SQL komut dosyaları üret ve veritabanı değişikliklerinin denetlenen bir uygulama ve veritabanı dağıtımının bir parçası dağıtmak uygun.

Kimlik kullanarak yeni bir uygulama oluşturduğunuzda, 1 ve 2 numaralı adımları zaten tamamlanmış. Diğer bir deyişle, ilk veri modelini zaten var ve ilk geçiş projeye eklendi. İlk geçişten hala veritabanına uygulanması gerekiyor. İlk geçişten aşağıdaki yaklaşımlardan birini uygulanabilir:

* Çalıştırma `Update-Database` PMC içinde.
* Çalıştırma `dotnet ef database update` komut kabuğu'nda.
* Tıklayın **geçerli geçişleri** düğmesi uygulamayı çalıştırdığınızda hata sayfasında.

Modelde değişiklikler yapıldıkça önceki adımları yineleyin.

## <a name="the-identity-model"></a>Kimlik modeli

### <a name="entity-types"></a>Varlık türleri

Aşağıdaki varlık türlerini kimlik modeli oluşur.

|varlık türü|Açıklama                                                  |
|-----------|-------------------------------------------------------------|
|`User`     |Kullanıcıyı temsil eder.                                         |
|`Role`     |Bir rolü temsil eder.                                           |
|`UserClaim`|Bir kullanıcının sahip olduğu bir talebi temsil eder.                    |
|`UserToken`|Bir kullanıcı için bir kimlik doğrulama belirteci temsil eder.               |
|`UserLogin`|Bir kullanıcı bir oturum açma ile ilişkilendirir.                              |
|`RoleClaim`|Bir roldeki tüm kullanıcılara verilen talebi temsil eder.|
|`UserRole` |Kullanıcılar ve roller ilişkilendirir birleştirme varlık.               |

### <a name="entity-type-relationships"></a>Varlık türü ilişkileri

[Varlık türleri](#entity-types) aşağıdaki şekillerde birbirleriyle ilişkili:

* Her `User` birçok sahip `UserClaims`.
* Her `User` birçok sahip `UserLogins`.
* Her `User` birçok sahip `UserTokens`.
* Her `Role` olabilir birçok ilişkili `RoleClaims`.
* Her `User` olabilir birçok ilişkili `Roles`ve her `Role` çoğu ile ilişkili olabilir `Users`. Bu veritabanında bir birleşim tablosunu gerektiren bir çoktan çoğa bir ilişkidir. Birleşim tablosu tarafından temsil edilen `UserRole` varlık.

### <a name="default-model-configuration"></a>Varsayılan model yapılandırma

Kimlik birçok tanımlar *bağlamı sınıfları* türünden devralınır <xref:Microsoft.EntityFrameworkCore.DbContext> yapılandırıp modelini kullanın. Bu yapılandırma yapılır kullanarak [EF Core kod ilk Fluent API'si](/ef/core/modeling/) içinde <xref:Microsoft.EntityFrameworkCore.DbContext.OnModelCreating*> bağlamı sınıfının yöntemi. Varsayılan yapılandırma verilmiştir:

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

### <a name="model-generic-types"></a>Model genel türler

Varsayılan kimlik tanımlar [ortak dil çalışma zamanı](/dotnet/standard/glossary#clr) yukarıda listelenen her bir varlık türü için (CLR) türleri. Bu tür tüm ön eki *kimlik*:

* `IdentityUser`
* `IdentityRole`
* `IdentityUserClaim`
* `IdentityUserToken`
* `IdentityUserLogin`
* `IdentityRoleClaim`
* `IdentityUserRole`

Bu tür doğrudan kullanmak yerine türleri temel sınıf olarak uygulamanın kendi türleri için kullanılabilir. `DbContext` Farklı CLR türleri için bir veya daha fazla varlık türleri modelinde kullanılabilir olacak şekilde kimlik tarafından tanımlanan sınıflara genel,. Bu genel türler de izin `User` değiştirilmesi için birincil anahtar (PK) veri türü.

Kimlik desteğiyle rolleri için kullanılırken bir <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext> sınıfı kullanılmalıdır. Örneğin:

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

(Yalnızca talep) rol, bu durumda kimlik kullanmak da mümkündür bir <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityUserContext%601> sınıfı kullanılmalıdır:

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

## <a name="customize-the-model"></a>Model özelleştirme

Model özelleştirme için başlangıç noktası uygun içerik türünden türetilmesi sağlamaktır. Bkz: [Model genel türler](#model-generic-types) bölümü. Bu bağlam türü özel olarak adlandırılır `ApplicationDbContext` ve ASP.NET Core şablonları tarafından oluşturulur.

Bağlam model iki şekilde yapılandırmak için kullanılır:

* Varlık ve genel tür parametreleri için anahtar türleri sağlama.
* Geçersiz kılma `OnModelCreating` bu tür eşlemesini değiştirmek için.

Geçersiz kılarken `OnModelCreating`, `base.OnModelCreating` ilk kez çağrılması gerekir; geçersiz kılma yapılandırmasını sonra çağrılmalıdır. EF Core genel yapılandırma için bir son bir WINS ilkesi vardır. Örneğin, varsa `ToTable` yöntemi bir varlık türü için bir tablo adıyla önce çağrılır ve daha sonra tekrar farklı bir tablo adı ile ikinci çağrıda tablo adı daha sonra kullanılır.

### <a name="custom-user-data"></a>Özel kullanıcı verileri

[Özel kullanıcı verilerini](xref:security/authentication/add-user-data) devralarak desteklenen `IdentityUser`. Bu tür adı uygulamadır `ApplicationUser`:

```csharp
public class ApplicationUser : IdentityUser
{
    public string CustomTag { get; set; }
}
```

Kullanım `ApplicationUser` bağlamı için genel bir bağımsız değişken türü:

```csharp
public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }
}
```

Geçersiz kılmak için gerek yoktur `OnModelCreating` içinde `ApplicationDbContext` sınıfı. EF Core eşler `CustomTag` gereği özelliği. Ancak, veritabanını yeni bir güncelleştirilmesi gerekiyor `CustomTag` sütun. Sütun oluşturmak için bir geçiş ekleyin ve ardından veritabanını açıklandığı gibi güncelleştirin [kimlik ve EF Core geçişleri](#identity-and-ef-core-migrations).

Güncelleştirme `Startup.ConfigureServices` yeni `ApplicationUser` sınıfı:

::: moniker range=">= aspnetcore-2.1"

```csharp
services.AddDefaultIdentity<ApplicationUser>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultUI();
```

ASP.NET Core 2.1 veya daha sonra kimlik Razor sınıf kitaplığı sağlanır. Daha fazla bilgi için bkz. <xref:security/authentication/scaffold-identity>. Sonuç olarak, önceki kod yapılan bir çağrı gerektirir. <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>. Kimlik iskele kurucu kimlik dosyalar projeye eklemek için kullanıldıysa çağrısını kaldırın `AddDefaultUI`. Daha fazla bilgi için bkz.:

* [İskele Kimliği](xref:security/authentication/scaffold-identity)
* [Ekleme, indirmek ve kimlik için özel kullanıcı verilerini sil](xref:security/authentication/add-user-data)

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();
```

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext, Guid>()
        .AddDefaultTokenProviders();
```

::: moniker-end

### <a name="change-the-primary-key-type"></a>Birincil anahtar türünü değiştirme

Veritabanı oluşturulduktan sonra PK sütunun veri türü için çok sayıda veritabanı sistemlerinde sorunlu farklıdır. PK değiştirilmesi genellikle, bırakarak ve tabloyu yeniden oluşturmayı içerir. Bu nedenle, veritabanı oluşturulurken anahtar türleri ilk geçiş belirtilmelidir.

PK türünü değiştirmek için aşağıdaki adımları izleyin:

1. Veritabanı oluşturduysanız çalıştırarak PK değişiklikten önce `Drop-Database` (PMC) veya `dotnet ef database drop` (CLI silmek için .NET Core).
2. Veritabanının silinmesi, onayladıktan sonra ilk geçiş işlemine kaldırmak `Remove-Migration` (PMC) veya `dotnet ef migrations remove` (.NET Core CLI).
3. Güncelleştirme `ApplicationDbContext` öğesinden türetilen sınıfın <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext%603>. Yeni anahtar türü için belirtin `TKey`. Örneğin, kullanılacak bir `Guid` anahtar türü:

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

    Önceki kodda, Genel sınıflar <xref:Microsoft.AspNetCore.Identity.IdentityUser%601> ve <xref:Microsoft.AspNetCore.Identity.IdentityRole%601> yeni anahtar türü kullanmak için belirtilmelidir.

    ::: moniker-end

    ::: moniker range="<= aspnetcore-1.1"

    Önceki kodda, Genel sınıflar <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityUser%601> ve <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityRole%601> yeni anahtar türü kullanmak için belirtilmelidir.

    ::: moniker-end

    `Startup.ConfigureServices` Genel kullanıcı kullanacak şekilde güncelleştirilmesi gerekir:

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

4. Özel durumunda `ApplicationUser` sınıfı kullanılıyor, devralınacak sınıfını güncelleştirme `IdentityUser`. Örneğin:

    ::: moniker range="<= aspnetcore-1.1"

    [!code-csharp[](customize-identity-model/samples/1.1/MvcSampleApp/Models/ApplicationUser.cs?name=snippet_ApplicationUser&highlight=4)]

    ::: moniker-end

    ::: moniker range=">= aspnetcore-2.0"

    [!code-csharp[](customize-identity-model/samples/2.1/RazorPagesSampleApp/Data/ApplicationUser.cs?name=snippet_ApplicationUser&highlight=4)]

    ::: moniker-end

    Güncelleştirme `ApplicationDbContext` özel başvurmak için `ApplicationUser` sınıfı:

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

    Özel bir veritabanı bağlamı sınıfının kimlik hizmeti eklerken kaydetme `Startup.ConfigureServices`:

    ::: moniker range=">= aspnetcore-2.1"

    ```csharp
    services.AddDefaultIdentity<ApplicationUser>()
            .AddEntityFrameworkStores<ApplicationDbContext>()
            .AddDefaultUI()
            .AddDefaultTokenProviders();
    ```

    Birincil anahtarın veri türü analiz ederek algılanır <xref:Microsoft.EntityFrameworkCore.DbContext> nesne.

    ASP.NET Core 2.1 veya daha sonra kimlik Razor sınıf kitaplığı sağlanır. Daha fazla bilgi için bkz. <xref:security/authentication/scaffold-identity>. Sonuç olarak, önceki kod yapılan bir çağrı gerektirir. <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>. Kimlik iskele kurucu kimlik dosyalar projeye eklemek için kullanıldıysa çağrısını kaldırın `AddDefaultUI`.

    ::: moniker-end

    ::: moniker range="= aspnetcore-2.0"

    ```csharp
    services.AddIdentity<ApplicationUser, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext>()
            .AddDefaultTokenProviders();
    ```

    Birincil anahtarın veri türü analiz ederek algılanır <xref:Microsoft.EntityFrameworkCore.DbContext> nesne.

    ::: moniker-end

    ::: moniker range="<= aspnetcore-1.1"

    ```csharp
    services.AddIdentity<ApplicationUser, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext, Guid>()
            .AddDefaultTokenProviders();
    ```

    <xref:Microsoft.Extensions.DependencyInjection.IdentityEntityFrameworkBuilderExtensions.AddEntityFrameworkStores*> Yöntemi kabul bir `TKey` birincil anahtarın veri türünü gösteren tür.

    ::: moniker-end

5. Özel durumunda `ApplicationRole` sınıfı kullanılıyor, devralınacak sınıfını güncelleştirme `IdentityRole<TKey>`. Örneğin:

    [!code-csharp[](customize-identity-model/samples/2.1/RazorPagesSampleApp/Data/ApplicationRole.cs?name=snippet_ApplicationRole&highlight=4)]

    Güncelleştirme `ApplicationDbContext` özel başvurmak için `ApplicationRole` sınıfı. Örneğin, aşağıdaki sınıf özel başvuran `ApplicationUser` ve özel bir `ApplicationRole`:

    ::: moniker range=">= aspnetcore-2.1"

    [!code-csharp[](customize-identity-model/samples/2.1/RazorPagesSampleApp/Data/ApplicationDbContext.cs?name=snippet_ApplicationDbContext&highlight=5-6)]

    Özel bir veritabanı bağlamı sınıfının kimlik hizmeti eklerken kaydetme `Startup.ConfigureServices`:

    [!code-csharp[](customize-identity-model/samples/2.1/RazorPagesSampleApp/Startup.cs?name=snippet_ConfigureServices&highlight=13-16)]

    Birincil anahtarın veri türü analiz ederek algılanır <xref:Microsoft.EntityFrameworkCore.DbContext> nesne.

    ASP.NET Core 2.1 veya daha sonra kimlik Razor sınıf kitaplığı sağlanır. Daha fazla bilgi için bkz. <xref:security/authentication/scaffold-identity>. Sonuç olarak, önceki kod yapılan bir çağrı gerektirir. <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>. Kimlik iskele kurucu kimlik dosyalar projeye eklemek için kullanıldıysa çağrısını kaldırın `AddDefaultUI`.

    ::: moniker-end

    ::: moniker range="= aspnetcore-2.0"

    [!code-csharp[](customize-identity-model/samples/2.0/RazorPagesSampleApp/Data/ApplicationDbContext.cs?name=snippet_ApplicationDbContext&highlight=5-6)]

    Özel bir veritabanı bağlamı sınıfının kimlik hizmeti eklerken kaydetme `Startup.ConfigureServices`:

    [!code-csharp[](customize-identity-model/samples/2.0/RazorPagesSampleApp/Startup.cs?name=snippet_ConfigureServices&highlight=7-9)]

    Birincil anahtarın veri türü analiz ederek algılanır <xref:Microsoft.EntityFrameworkCore.DbContext> nesne.

    ::: moniker-end

    ::: moniker range="<= aspnetcore-1.1"

    [!code-csharp[](customize-identity-model/samples/1.1/MvcSampleApp/Data/ApplicationDbContext.cs?name=snippet_ApplicationDbContext&highlight=5-6)]

    Özel bir veritabanı bağlamı sınıfının kimlik hizmeti eklerken kaydetme `Startup.ConfigureServices`:

    [!code-csharp[](customize-identity-model/samples/1.1/MvcSampleApp/Startup.cs?name=snippet_ConfigureServices&highlight=7-9)]

    <xref:Microsoft.Extensions.DependencyInjection.IdentityEntityFrameworkBuilderExtensions.AddEntityFrameworkStores*> Yöntemi kabul bir `TKey` birincil anahtarın veri türünü gösteren tür.

    ::: moniker-end

### <a name="add-navigation-properties"></a>Gezinti özellikleri ekleyin

İlişkiler için model yapılandırmasını değiştirme başka değişiklikler yaparak değerinden daha zor olabilir. Var olan ilişkileri değiştirmek yerine yeni, ek ilişkiler oluşturmak için dikkatli olunması gerekir. Özellikle, değiştirilen ilişki aynı yabancı anahtar (FK) özelliği var olan ilişkiyi belirtmeniz gerekir. Örneğin, arasındaki ilişkiyi `Users` ve `UserClaims` , varsayılan olarak, aşağıda belirtilen ise:

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

FK bu ilişki için belirtilen `UserClaim.UserId` özelliği. `HasMany` ve `WithOne` Gezinti özellikleri olmayan bir ilişki oluşturmak için bağımsız değişkenler olmadan verilir.

Bir gezinme özelliği için ekleme `ApplicationUser` sağlayan ilişkili `UserClaims` kullanıcıdan başvurulmak üzere:

```csharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<IdentityUserClaim<string>> Claims { get; set; }
}
```

`TKey` İçin `IdentityUserClaim<TKey>` PK kullanıcı için belirtilen bir tür. Bu durumda, `TKey` olduğu `string` Varsayılanları kullanıldığından. Sahip **değil** PK türü `UserClaim` varlık türü.

Gezinti özelliği var, bunu yapılandırılmalıdır `OnModelCreating`:

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

İlişki, daha önce yalnızca yapılan çağrıda belirtilen Gezinti özelliğine sahip olduğu gibi yapılandırıldığını fark `HasMany`.

Gezinme özelliklerini EF modeli, veritabanı değil yalnızca mevcut. İlişki için FK değişmediği için güncelleştirilecek veritabanı bu tür bir model değişikliği gerektirmez. Bu değişikliği yaptıktan sonra geçiş ekleyerek denetlenebilir. `Up` Ve `Down` yöntemlerdir boş.

### <a name="add-all-user-navigation-properties"></a>Tüm kullanıcı Gezinti özellikleri ekleyin

Yukarıdaki bölüme kılavuz kullanarak, aşağıdaki örnekte, kullanıcı tüm ilişkiler tek yönlü bir gezinti özelliklerini yapılandırır:

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

### <a name="add-user-and-role-navigation-properties"></a>Kullanıcı ve rol Gezinti özellikleri ekleyin

Yukarıdaki bölüme kılavuz kullanarak, aşağıdaki örnekte tüm ilişkiler için gezinme özelliklerinin kullanıcı ve rol yapılandırır:

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

* Bu örnek ayrıca içerir `UserRole` katılma, kullanıcıların çok-çok ilişkisi rollerine gitmek için gerekli olan varlık.
* Gezinti özellikleri, değişimi yansıtmak için tür değiştirmeyi unutmayın `ApplicationXxx` türleri artık kullanıldığı yerine `IdentityXxx` türleri.
* Kullanmayı unutmayın `ApplicationXxx` genel olarak `ApplicationContext` tanımı.

### <a name="add-all-navigation-properties"></a>Tüm gezinti özellikleri ekleyin

Yukarıdaki bölüme kılavuz kullanarak, aşağıdaki örnekte tüm varlık türleri üzerinde tüm ilişkiler için Gezinti özellikleri yapılandırır:

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

Kimlik modelinde kullanılan anahtar türünü değiştirerek önceki bölümlerde gösterilmiştir. Bileşik anahtarlar kullanılacak kimlik anahtar modelini değiştirme, önerilen desteklenen veya değil. Bir bileşik anahtarı ile kimlik kullanarak, Identity manager kod modeli ile nasıl etkileştiğini değiştirilmesini kapsar. Bu belgenin kapsamı dışındadır özelleştirmedir.

### <a name="change-tablecolumn-names-and-facets"></a>Tablo/sütun adlarını ve modeller

Tablo ve sütun adlarını değiştirmek için çağrı `base.OnModelCreating`. Ardından, tüm varsayılanları geçersiz kılmak için yapılandırma ekleyin. Örneğin, tüm kimlik tabloları adını değiştirmek için şunu yazın:

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

Bu örnekler, varsayılan kimlik türlerini kullanın. Bir uygulama türü gibi kullanıyorsanız `ApplicationUser`, varsayılan türü yerine bu tür yapılandırın.

Aşağıdaki örnek, bazı sütun adlarını değiştirir:

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

Veritabanı sütunlarının bazı türleri belirli ile yapılandırılabilir *modelleri* (örneğin, maksimum `string` izin verilen uzunluk). Aşağıdaki örnekte en çok uzunlukları sütun için çeşitli ayarlar `string` modelinde özellikleri:

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

Şemalar, veritabanı sağlayıcıları arasında farklı şekilde davranabilir. SQL Server için varsayılan olarak tüm tabloları oluşturmaktır *dbo* şema. Tablolar farklı bir şema oluşturulabilir. Örneğin:

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    base.OnModelCreating(modelBuilder);

    modelBuilder.HasDefaultSchema("notdbo");
}
```

::: moniker range=">= aspnetcore-2.1"

### <a name="lazy-loading"></a>Yavaş yükleniyor

Bu bölümde, yavaş yükleniyor proxy'si kimlik modeli için destek eklendi. Gezinti özellikleri, yüklenen ilk sağlamaya gerek kalmadan kullanılacak olanak tanıdığından Lazy yüklenirken kullanışlıdır.

Varlık türleri yapılabilir uygun çeşitli yollarla yavaş yükleniyor açıklandığı [EF Core belgeleri](/ef/core/querying/related-data#lazy-loading). Kolaylık olması için Gecikmeli yükleme proxy'leri gerektiren kullanın:

* Yüklenmesini [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) paket.
* Bir çağrı <xref:Microsoft.EntityFrameworkCore.ProxiesExtensions.UseLazyLoadingProxies*> içinde <xref:Microsoft.Extensions.DependencyInjection.EntityFrameworkServiceCollectionExtensions.AddDbContext*>.
* Genel varlık türleri ile `public virtual` Gezinti özellikleri.

Aşağıdaki örnek, arama gösterir `UseLazyLoadingProxies` içinde `Startup.ConfigureServices`:

```csharp
services
    .AddDbContext<ApplicationDbContext>(
        b => b.UseSqlServer(connectionString)
              .UseLazyLoadingProxies())
    .AddDefaultIdentity<ApplicationUser>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

Önceki örneklerde varlık türlerine Gezinti özellikleri ekleme Kılavuzu'na bakın.

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:security/authentication/scaffold-identity>

::: moniker-end
