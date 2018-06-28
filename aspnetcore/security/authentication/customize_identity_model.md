---
title: Kimlik modeli özelleştirme
author: ajcvickers
description: Bu makalede, ASP.NET Core kimliği için Entity Framework Çekirdek veri modeli özelleştirmek açıklar.
ms.author: avickers
ms.date: 04/12/2018
ms.prod: asp.net-core
uid: security/authentication/customize_identity_model
ms.openlocfilehash: b44b4fd0f24d245b969588a7226ea6aacbe2a722
ms.sourcegitcommit: 7003d27b607e529642ded0400aa48ae692a0e666
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37036920"
---
# <a name="identity-model-customization"></a>Kimlik modeli özelleştirme

Tarafından [Arthur Vickers](https://github.com/ajcvickers)

ASP.NET Core kimlik yönetmek ve ASP.NET Core uygulamaları kullanıcı hesaplarını depolamak için bir çerçeve sağlar. Kimlik doğrulama mekanizması olarak "Tek tek kullanıcı hesaplarını" seçildiğinde kimlik projenize eklenir. Varsayılan olarak, kimlik bir Entity Framework (EF), yararlanır çekirdek veri modeli. Bu makalede, kimlik modelini özelleştirmek açıklar.

<a name="identity-migrations"></a>

## <a name="identity-and-ef-core-migrations"></a>Kimlik ve EF çekirdek geçişleri

Model inceleniyor önce kimlik ile nasıl çalıştığını anlamak yararlı olacaktır [EF çekirdek geçişler](/ef/core/managing-schemas/migrations/) oluşturup veritabanını güncelleştirmek için. En üst düzeyde bir işlemdir:

1. Tanımlayın veya güncelleştirme bir [kodundaki veri modeli](/ef/core/modeling/).
1. Bu model veritabanına uygulanabilir değişiklikleri içine çevirmek için bir geçiş ekleyin.
1. Geçiş, ilkenizin amacını doğru şekilde temsil eden denetleyin.
1. Model ile eşitlenmiş olması için veritabanını güncelleştirmek için geçiş uygulanır.
1. Daha fazla model daraltın ve veritabanı eşitlenmiş tutmak için 1-4 arası adımları yineleyin.

Geçişler eklenir ve kullanılarak uygulanan:

* [Paket Yöneticisi Konsolu](/ef/core/miscellaneous/cli/powershell) (Visual Studio kullanıyorsanız PMC).
* [Dotnet komutları](/ef/core/miscellaneous/cli/dotnet) komut satırında.
* Uygulamayı çalıştırdığınızda hata sayfasında geçişler bağlantısını seçerek.

ASP.NET Core uygulamayı çalıştırdığınızda geçişler uygulamak için kullanılan bir geliştirme zamanı hata sayfası işleyicisi vardır. Üretim uygulamaları için genellikle daha yararlıdır geçişleri SQL komut dosyaları oluşturmak ve bu veritabanına denetlenen bir uygulama ve veritabanı dağıtımının bir parçası dağıtmak uygun.

Kimlik bilgileriniz kullanılarak yeni bir uygulama oluşturduğunuzda, 1 ve 2 yukarıdaki adımları zaten tamamlandı. Diğer bir deyişle, ilk veri modelinde zaten var ve ilk geçiş projeye eklendi. İlk geçiş hala veritabanına uygulanması gerekiyor. İlk geçiş Update-Database (PMC), dotnet ef veritabanı güncelleştirmesi (.NET Core CLI) komutunu çalıştırarak veya uygulamayı çalıştırdığınızda hata sayfası kullanılarak yapılabilir.

Yukarıdaki adımları modeline yapılan değişiklikler gibi yinelenmesi gerekir.

<a name="identity-model"></a>

## <a name="the-identity-model"></a>Kimlik modeli

### <a name="entity-types"></a>Varlık türleri

Kimlik modeli yedi varlık türlerini oluşur:

* `User` -kullanıcısını temsil eder.
* `Role` -bir rolü temsil eder
* `UserClaim` -bir kullanıcının sahip bir talebi temsil eder
* `UserToken` -bir kullanıcı için bir kimlik doğrulama belirteci temsil eder
* `UserLogin` -Kullanıcı bir oturum ile ilişkilendirir
* `RoleClaim` -bir rolü içindeki tüm kullanıcılara verilen talebi temsil eder
* `UserRole` -Kullanıcılar ve roller ilişkilendirir varlık katılma

### <a name="entity-type-relationships"></a>Varlık türü ilişkileri

Bu varlık türleri, aşağıdaki yollarla birbiriyle:

* Her `User` birçok olabilir `UserClaims`
* Her `User` birçok olabilir `UserLogins`
* Her `User` birçok olabilir `UserTokens`
* Her `Role` olabilir birçok ilişkili `RoleClaims`
* Her `User` olabilir birçok ilişkili `Roles`ve her `Role` birden çok kullanıcıya sahip ilişkilendirilebilir
  * Veritabanındaki bir birleşim tablosuna gerektiren çok-çok ilişkisi budur. Birleştirme tablosu tarafından temsil edilen `UserRole` varlık.

### <a name="default-model-configuration"></a>Varsayılan model yapılandırma

Kimliği "öğesinden devralan bağlam sınıflarını" çeşitli tanımlar [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) yapılandırıp modeli kullanır. Bu yapılandırma yapılır kullanarak [EF çekirdek Code First Fluent API'SİNDE](/ef/core/modeling/) içinde [OnModelCreating](/dotnet/api/microsoft.entityframeworkcore.dbcontext.onmodelcreating#Microsoft_EntityFrameworkCore_DbContext_OnModelCreating_Microsoft_EntityFrameworkCore_ModelBuilder_) bağlamı sınıfının yöntemi. Varsayılan yapılandırmadır:

```CSharp
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

    // Limit the size of the composite key columns due to common database restrictions
    b.Property(l => l.LoginProvider).HasMaxLength(128);
    b.Property(l => l.ProviderKey).HasMaxLength(128);

    // Maps to the AspNetUserLogins table
    b.ToTable("AspNetUserLogins");
});

builder.Entity<TUserToken>(b =>
{
    // Composite primary key consisting of the UserId, LoginProvider and Name
    b.HasKey(t => new { t.UserId, t.LoginProvider, t.Name });

    // Limit the size of the composite key columns due to common database restrictions
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

Kimlik, yukarıda listelenen Varlık türlerinin her biri için varsayılan CLR türlerini tanımlar. Bu tür tüm "Kimliği ile" önek: `IdentityUser`, `IdentityRole`, `IdentityUserClaim`, `IdentityUserToken`, `IdentityUserLogin`, `IdentityRoleClaim`, ve `IdentityUserRole`.

Bu tür doğrudan kullanmak yerine türleri temel sınıfları uygulamanın kendi türleri için kullanılabilir. `DbContext` Kimliği tarafından tanımlanan sınıflardır genel sağlayacak şekilde bir veya daha fazla modeldeki varlık türü için farklı CLR türleri kullanılabilir. Bu genel türü Ayrıca kullanıcı birincil anahtar türü için değiştirilmesine izin verir.

Kimlik desteğiyle rolleri için kullanılırken bir [Identitydbcontext](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identitydbcontext) sınıfı kullanılmalıdır:

```CSharp
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
public class IdentityDbContext<TUser, TRole, TKey>
    : IdentityDbContext<TUser, TRole, TKey, IdentityUserClaim<TKey>, IdentityUserRole<TKey>, IdentityUserLogin<TKey>, IdentityRoleClaim<TKey>, IdentityUserToken<TKey>>
        where TUser : IdentityUser<TKey>
        where TRole : IdentityRole<TKey>
        where TKey : IEquatable<TKey>
{
}

// No built-in Identity types are used; all are specified by generic arguments
// The key type is defined by TKey
public abstract class IdentityDbContext<TUser, TRole, TKey, TUserClaim, TUserRole, TUserLogin, TRoleClaim, TUserToken>
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

(Yalnızca talepler) rollerini; bu durumda kimlik kullanmak da mümkündür bir `IdentityUserContext` sınıfı kullanılmalıdır:


```CSharp
// Uses the built-in non-role Identity types except with a custom User type
// Uses `string` as the key type
public class IdentityUserContext<TUser>
    : IdentityUserContext<TUser, string>
        where TUser : IdentityUser
{
}

// Uses the built-in non-role Identity types except with a custom User type
// The key type is defined by TKey
public class IdentityUserContext<TUser, TKey>
    : IdentityUserContext<TUser, TKey, IdentityUserClaim<TKey>, IdentityUserLogin<TKey>, IdentityUserToken<TKey>>
        where TUser : IdentityUser<TKey>
        where TKey : IEquatable<TKey>
{
}

// No built-in Identity types are used; all are specified by generic arguments, with no roles
// The key type is defined by TKey
public abstract class IdentityUserContext<TUser, TKey, TUserClaim, TUserLogin, TUserToken>
    : DbContext
        where TUser : IdentityUser<TKey>
        where TKey : IEquatable<TKey>
        where TUserClaim : IdentityUserClaim<TKey>
        where TUserLogin : IdentityUserLogin<TKey>
        where TUserToken : IdentityUserToken<TKey>
{
}
```

## <a name="customizing-the-model"></a>Model özelleştirme

Model özelleştirmek için uygun bağlamı türünden türemesi için başlangıçtır; önceki bölümüne bakın. Bu içerik türü geleneksel olarak adlandırılır `ApplicationDbContext` ve ASP.NET Core şablonları tarafından oluşturulur.

Bağlam model iki şekilde yapılandırmak için kullanılır:
* Varlık ve anahtar türleri için genel tür parametreleri sağlayarak.
* Geçersiz kılma tarafından `OnModelCreating` bu tür eşlemesi değiştirmek için.

Geçersiz kılarken `OnModelCreating`, `base.OnModelCreating` ilk olarak, geçersiz kılma çağrılmalıdır yapılandırma çağrılabilir sonraki. EF çekirdek yapılandırması için bir son bir WINS ilkesi genellikle sahiptir. Örneğin, varsa `ToTable` için bir varlık türü farklı bir tablo adı ile daha sonra yeniden ve adlı bir tablo adı ile ilk sonra tablo ikinci çağrısında kullanılan bir addır.

### <a name="using-a-custom-user-type"></a>Özel bir kullanıcı türü kullanma

Özel bir kullanıcı türü kullanmak için türü oluşturursanız ve onu devralınmalıdır `IdentityUser`. Bu tür adı uygulamadır `ApplicationUser`. Bu tür uygulamaların tipik olarak değil temel türdeki ek özellikler vardır, aksi takdirde olurdu hiçbir değer oluşturma içinde. Örneğin:

```CSharp
public class ApplicationUser : IdentityUser
{
    public string CustomTag { get; set; }
}
```

Sonraki bağlam için genel bir bağımsız değişken olarak bu türü kullanın:

```CSharp
public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }
}
```

Güncelleştirme `ConfigureServices` yeni `ApplicationUser` sınıfı:

```CSharp
services.AddDefaultIdentity<ApplicationUser>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

Geçersiz kılmak için gerek yoktur `OnModelCreating` EF çekirdek eşler için burada `CustomTag` kural tarafından özelliği. Ancak, veritabanını yeni bir almak için güncelleştirilmesi gerekecek `CustomTag` sütun. Bunu yapmak için bir geçiş ekleyin ve ardından veritabanı açıklandığı gibi güncelleştirin [kimlik ve EF çekirdek geçişler](#identity-migrations).

### <a name="changing-the-key-type"></a>Anahtar türü değiştirme

Veritabanı oluşturulduktan sonra bir birincil anahtar (PK) sütunun türünü değiştirme birçok veritabanı sistemlerinde sorunlu oluşturur. BA genellikle bırakarak ve tablo yeniden oluşturma değiştirilmesidir. Bu nedenle, veritabanı oluşturulduğunda, hedef anahtar türleri oluşturulur, anahtar türleri ilk geçişte belirtilmesi önerilir.

Veritabanını, sonra oluşturulduysa `Drop-Database` (PMC) veya `dotnet ef database drop` (.NET Core CLI) silmek için kullanılabilir.

Veritabanı yok onaylandıktan sonra ilk geçişle kaldırmak `Remove-Migration` (PMC) veya `dotnet ef migrations remove` (.NET Core CLI).

Güncelleştirme `ApplicationDbContext` için yeni anahtar türü belirterek farklı bir temel sınıfı kullanmak için `TKey`. Örneğin, kullanılacak bir `Guid` anahtarı:

```CSharp
public class ApplicationDbContext : IdentityDbContext<IdentityUser<Guid>, IdentityRole<Guid>, Guid>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }
}
```

Dikkat Genel sınıflar `IdentityUser<TKey>` ve `IdentityRole<TKey>` yeni anahtar türünü kullanmak için de belirtilmelidir. `ConfigureServices` Genel kullanıcı kullanacak şekilde güncelleştirilmesi gerekir:

```CSharp
services.AddDefaultIdentity<IdentityUser<Guid>>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

Özel bir varsa `ApplicationUser` olan kullanılmakta devralınacak güncelleştirmek `IdentityUser<TKey>`. Örneğin:

```CSharp
public class ApplicationUser : IdentityUser<Guid>
{
    public string CustomTag { get; set; }
}

public class ApplicationDbContext : IdentityDbContext<ApplicationUser, IdentityRole<Guid>, Guid>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }
}
```

```CSharp
services.AddDefaultIdentity<ApplicationUser>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

### <a name="adding-navigation-properties"></a>Gezinti özellikleri ekleme

İlişkiler için model yapılandırmasını değiştirme bir daha zor başka değişiklikler yaparak daha olabilir. Var olan ilişkileri değiştirmek yerine yeni bir ek ilişkiler oluşturmak için dikkatli olunması gerekir. Özellikle, değiştirilen ilişki aynı yabancı anahtar özelliği var olan bir ilişki olarak belirtmeniz gerekir. Örneğin, arasındaki ilişkiyi `Users` ve `UserClaims` olarak belirtilen varsayılan olarak seçilir:

```CSharp
builder.Entity<TUser>(b =>
{
    // Each User can have many UserClaims
    b.HasMany<TUserClaim>()
     .WithOne()
     .HasForeignKey(uc => uc.UserId)
     .IsRequired();
});
```

Bu ilişki için yabancı anahtar olarak belirtilen `UserClaim.UserId` özelliği. `HasMany` ve `WithOne` Gezinti özellikleri olmayan ilişki oluşturmak için bağımsız değişkenler olmadan denir.

Bir gezinti özelliği Ekle `ApplicationUser` izin ilişkili `UserClaims` kullanıcıdan başvurulacak:

```CSharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<IdentityUserClaim<string>> Claims { get; set; }
}
```

Unutmayın `TKey` için `IdentityUserClaim<TKey>` kullanıcıları--bu durumda, birincil anahtar için belirtilen tür `string` biz Varsayılanları kullandığımızdan. Bu **değil** için birincil anahtar türü `UserClaim` varlık türü.

Gezinti özelliği var. göre de yapılandırılmalıdır `OnModelCreating`:

```CSharp
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

Daha önce yalnızca çağrısında belirtilen bir gezinti özelliğine sahip olduğu gibi ilişki yapılandırılmış fark `HasMany`.

Gezinti özellikleri yalnızca EF modeli, veritabanı yok. Yabancı anahtar ilişkisi için değişmediği için bu tür bir model değişikliği güncelleştirilmesi veritabanına gerektirmez. Bu değişikliği yaptıktan sonra geçiş ekleyerek denetlenebilir: `Up` ve `Down` yöntemleri boş.

### <a name="adding-all-user-navigation-properties"></a>Tüm kullanıcı Gezinti özellikleri ekleme

Yukarıdaki bölümü kılavuz olarak kullanarak, kullanıcı tüm ilişkiler için tek yönlü gezinme özelliklerinin yapılandıran örnek aşağıda verilmiştir:

```CSharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<IdentityUserClaim<string>> Claims { get; set; }
    public virtual ICollection<IdentityUserLogin<string>> Logins { get; set; }
    public virtual ICollection<IdentityUserToken<string>> Tokens { get; set; }
    public virtual ICollection<IdentityUserRole<string>> UserRoles { get; set; }
}
```

```CSharp
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

### <a name="adding-user-and-role-navigation-properties"></a>Kullanıcı ve rol Gezinti özellikleri ekleme

Yukarıdaki bölümü kılavuz olarak kullanarak, kullanıcı ve rol Gezinti özellikleri için tüm ilişkiler yapılandıran örnek aşağıda verilmiştir:

```CSharp
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

```CSharp
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

* Bu örnek ayrıca içerir `UserRole` katılma, kullanıcıların çok-çok ilişkisi rollere gitmek için gerekli olan varlık.
* Yansıtacak şekilde gezinti özellikleri türlerini değiştirmeyi unutmayın `ApplicationXxx` türleri şimdi kullanıldığından yerine `IdentityXxx` türleri.
* Kullanmayı unutmayın `ApplicationXxx` genel olarak `ApplicationContext` tanımı.

### <a name="adding-all-navigation-properties"></a>Tüm gezinti özellikleri ekleme

Yukarıdaki bölümü kılavuz olarak kullanarak, gezinti özellikleri için tüm ilişkiler üzerinde tüm varlık türlerinin yapılandıran örnek aşağıda verilmiştir:

```CSharp
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

```CSharp
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

### <a name="using-composite-keys"></a>Bileşik anahtarlar kullanarak

Önceki bölümlerde kimlik modelinde kullanılan anahtar türü değiştirme gösterilmektedir. Bileşik anahtarlar kullanılacak kimlik anahtar modelini değiştirme önerilen ve desteklenmez. Kimlikle bileşik bir anahtar kullanarak kimlik yöneticisi kodu desteklenmeyen bir özelleştirmedir modeli ile ve bu belgenin kapsamı dışında nasıl etkileşim kurduğu değiştirme içerir.

### <a name="changing-tablecolumn-names-and-facets"></a>Tablo/sütun adları ve modelleri değiştirme

Tablolar ve sütunlar adlarını değiştirmek için arama `base.OnModelCreating`ve tüm varsayılanları geçersiz kılmak için yapılandırma ekleyin. Örneğin, tüm kimlik tabloları adını değiştirmek için şunu yazın:

```CSharp
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

Bu örnekler varsayılan kimlik türlerinin kullandığını unutmayın. Uygulama türü gibi kullanıyorsanız `ApplicationUser` varsayılan türü yerine bu tür yapılandırın.

Bazı sütun adlarını değiştiren örnek aşağıda verilmiştir:

```CSharp
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

Bazı veritabanı sütunlarının türleri belirli ile yapılandırılabilir _modelleri_ gibi izin verilen maksimum dize uzunluğu. Modelde birkaç dize özellikleri için sütun en çok uzunlukları ayarlayan örnek aşağıda verilmiştir:

```CSharp
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

### <a name="mapping-to-a-different-schema"></a>Farklı bir şema eşleme

Şemalar farklı veritabanı sağlayıcıları farklı davranabilir, ancak SQL Server için tüm tabloları "dbo" şemada oluşturmak için varsayılandır. Bunun yerine farklı bir şemaya tablolar oluşturmak için değiştirilebilir. Örneğin:

```CSharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    base.OnModelCreating(modelBuilder);

    modelBuilder.HasDefaultSchema("notdbo");
}
```

### <a name="lazy-loading"></a>Yavaş yükleniyor

Bu bölümde kimlik modeli yükleme yavaş proxy'leri için destek eklendi. Yüklü olan ilk sağlama olmadan kullanılacak gezinti özellikleri olanak tanıdığından Lazy yüklenirken kullanışlıdır.

Varlık türleri yapılabilir uygun çeşitli yollarla yavaş yükleniyor açıklandığı gibi [EF Core belgeleri](/ef/core/querying/related-data#lazy-loading). Kolaylık olması için yükleme yavaş proxy'leri gerektiren kullanırız:

* Yüklemesini [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) paket.
* Çağrı `.UseLazyLoadingProxies()` içinde `AddDbContext`.
* Genel sanal Gezinti özellikleri genel varlık türleriyle.

Çağırma örneği `.UseLazyLoadingProxies()`:

```CSharp
services
    .AddDbContext<ApplicationDbContext>(
        b => b.UseSqlServer(connectionString)
            .UseLazyLoadingProxies())
    .AddDefaultIdentity<ApplicationUser>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

Geri gezinti özellikleri için varlık türlerini eklemek için örnekler bakın.
