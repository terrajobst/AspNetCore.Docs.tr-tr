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
# <a name="identity-model-customization"></a><span data-ttu-id="a9d45-103">Kimlik modeli özelleştirme</span><span class="sxs-lookup"><span data-stu-id="a9d45-103">Identity model customization</span></span>

<span data-ttu-id="a9d45-104">Tarafından [Arthur Vickers](https://github.com/ajcvickers)</span><span class="sxs-lookup"><span data-stu-id="a9d45-104">By [Arthur Vickers](https://github.com/ajcvickers)</span></span>

<span data-ttu-id="a9d45-105">ASP.NET Core kimlik yönetmek ve ASP.NET Core uygulamaları kullanıcı hesaplarını depolamak için bir çerçeve sağlar.</span><span class="sxs-lookup"><span data-stu-id="a9d45-105">ASP.NET Core Identity provides a framework for managing and storing user accounts in ASP.NET Core applications.</span></span> <span data-ttu-id="a9d45-106">Kimlik doğrulama mekanizması olarak "Tek tek kullanıcı hesaplarını" seçildiğinde kimlik projenize eklenir.</span><span class="sxs-lookup"><span data-stu-id="a9d45-106">Identity is added to your project when "Individual User Accounts" is selected as the authentication mechanism.</span></span> <span data-ttu-id="a9d45-107">Varsayılan olarak, kimlik bir Entity Framework (EF), yararlanır çekirdek veri modeli.</span><span class="sxs-lookup"><span data-stu-id="a9d45-107">By default, Identity makes use of an Entity Framework (EF) Core data model.</span></span> <span data-ttu-id="a9d45-108">Bu makalede, kimlik modelini özelleştirmek açıklar.</span><span class="sxs-lookup"><span data-stu-id="a9d45-108">This article describes how to customize the Identity model.</span></span>

<a name="identity-migrations"></a>

## <a name="identity-and-ef-core-migrations"></a><span data-ttu-id="a9d45-109">Kimlik ve EF çekirdek geçişleri</span><span class="sxs-lookup"><span data-stu-id="a9d45-109">Identity and EF Core Migrations</span></span>

<span data-ttu-id="a9d45-110">Model inceleniyor önce kimlik ile nasıl çalıştığını anlamak yararlı olacaktır [EF çekirdek geçişler](/ef/core/managing-schemas/migrations/) oluşturup veritabanını güncelleştirmek için.</span><span class="sxs-lookup"><span data-stu-id="a9d45-110">Before examining the model, it's useful to understand how Identity works with [EF Core Migrations](/ef/core/managing-schemas/migrations/) to create and update a database.</span></span> <span data-ttu-id="a9d45-111">En üst düzeyde bir işlemdir:</span><span class="sxs-lookup"><span data-stu-id="a9d45-111">At the top level, the process is:</span></span>

1. <span data-ttu-id="a9d45-112">Tanımlayın veya güncelleştirme bir [kodundaki veri modeli](/ef/core/modeling/).</span><span class="sxs-lookup"><span data-stu-id="a9d45-112">Define or update a [data model in code](/ef/core/modeling/).</span></span>
1. <span data-ttu-id="a9d45-113">Bu model veritabanına uygulanabilir değişiklikleri içine çevirmek için bir geçiş ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a9d45-113">Add a migration to translate this model into changes that can be applied to the database.</span></span>
1. <span data-ttu-id="a9d45-114">Geçiş, ilkenizin amacını doğru şekilde temsil eden denetleyin.</span><span class="sxs-lookup"><span data-stu-id="a9d45-114">Check that the migration correctly represents your intentions.</span></span>
1. <span data-ttu-id="a9d45-115">Model ile eşitlenmiş olması için veritabanını güncelleştirmek için geçiş uygulanır.</span><span class="sxs-lookup"><span data-stu-id="a9d45-115">Apply the migration to update the database to be in sync with the model.</span></span>
1. <span data-ttu-id="a9d45-116">Daha fazla model daraltın ve veritabanı eşitlenmiş tutmak için 1-4 arası adımları yineleyin.</span><span class="sxs-lookup"><span data-stu-id="a9d45-116">Repeat steps 1-4 to further refine the model and keep the database in sync.</span></span>

<span data-ttu-id="a9d45-117">Geçişler eklenir ve kullanılarak uygulanan:</span><span class="sxs-lookup"><span data-stu-id="a9d45-117">Migrations are added and applied using:</span></span>

* <span data-ttu-id="a9d45-118">[Paket Yöneticisi Konsolu](/ef/core/miscellaneous/cli/powershell) (Visual Studio kullanıyorsanız PMC).</span><span class="sxs-lookup"><span data-stu-id="a9d45-118">The [Package Manager Console](/ef/core/miscellaneous/cli/powershell) (PMC) if you are using Visual Studio.</span></span>
* <span data-ttu-id="a9d45-119">[Dotnet komutları](/ef/core/miscellaneous/cli/dotnet) komut satırında.</span><span class="sxs-lookup"><span data-stu-id="a9d45-119">The [dotnet commands](/ef/core/miscellaneous/cli/dotnet) on the command line.</span></span>
* <span data-ttu-id="a9d45-120">Uygulamayı çalıştırdığınızda hata sayfasında geçişler bağlantısını seçerek.</span><span class="sxs-lookup"><span data-stu-id="a9d45-120">Selecting the migrations link on the error page when the application is run.</span></span>

<span data-ttu-id="a9d45-121">ASP.NET Core uygulamayı çalıştırdığınızda geçişler uygulamak için kullanılan bir geliştirme zamanı hata sayfası işleyicisi vardır.</span><span class="sxs-lookup"><span data-stu-id="a9d45-121">ASP.NET Core has a development-time error page handler that can be used to apply migrations when the application is run.</span></span> <span data-ttu-id="a9d45-122">Üretim uygulamaları için genellikle daha yararlıdır geçişleri SQL komut dosyaları oluşturmak ve bu veritabanına denetlenen bir uygulama ve veritabanı dağıtımının bir parçası dağıtmak uygun.</span><span class="sxs-lookup"><span data-stu-id="a9d45-122">For production applications, it is often more appropriate to generate SQL scripts from the migrations and deploy these to the database as part of a controlled application and database deployment.</span></span>

<span data-ttu-id="a9d45-123">Kimlik bilgileriniz kullanılarak yeni bir uygulama oluşturduğunuzda, 1 ve 2 yukarıdaki adımları zaten tamamlandı.</span><span class="sxs-lookup"><span data-stu-id="a9d45-123">When a new application using Identity is created, steps 1 and 2 above have already been completed.</span></span> <span data-ttu-id="a9d45-124">Diğer bir deyişle, ilk veri modelinde zaten var ve ilk geçiş projeye eklendi.</span><span class="sxs-lookup"><span data-stu-id="a9d45-124">That is, the initial data model already exists, and the initial migration has been added to the project.</span></span> <span data-ttu-id="a9d45-125">İlk geçiş hala veritabanına uygulanması gerekiyor.</span><span class="sxs-lookup"><span data-stu-id="a9d45-125">The initial migration still needs to be applied to the database.</span></span> <span data-ttu-id="a9d45-126">İlk geçiş Update-Database (PMC), dotnet ef veritabanı güncelleştirmesi (.NET Core CLI) komutunu çalıştırarak veya uygulamayı çalıştırdığınızda hata sayfası kullanılarak yapılabilir.</span><span class="sxs-lookup"><span data-stu-id="a9d45-126">The initial migration can be done by running the Update-Database (PMC), the dotnet ef database update (.NET Core CLI) command, or by using the error page when the application is run.</span></span>

<span data-ttu-id="a9d45-127">Yukarıdaki adımları modeline yapılan değişiklikler gibi yinelenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="a9d45-127">The preceding steps need to be repeated as changes are made to the model.</span></span>

<a name="identity-model"></a>

## <a name="the-identity-model"></a><span data-ttu-id="a9d45-128">Kimlik modeli</span><span class="sxs-lookup"><span data-stu-id="a9d45-128">The Identity model</span></span>

### <a name="entity-types"></a><span data-ttu-id="a9d45-129">Varlık türleri</span><span class="sxs-lookup"><span data-stu-id="a9d45-129">Entity types</span></span>

<span data-ttu-id="a9d45-130">Kimlik modeli yedi varlık türlerini oluşur:</span><span class="sxs-lookup"><span data-stu-id="a9d45-130">The Identity model consists of seven entity types:</span></span>

* <span data-ttu-id="a9d45-131">`User` -kullanıcısını temsil eder.</span><span class="sxs-lookup"><span data-stu-id="a9d45-131">`User` - represents the user</span></span>
* <span data-ttu-id="a9d45-132">`Role` -bir rolü temsil eder</span><span class="sxs-lookup"><span data-stu-id="a9d45-132">`Role` - represents a role</span></span>
* <span data-ttu-id="a9d45-133">`UserClaim` -bir kullanıcının sahip bir talebi temsil eder</span><span class="sxs-lookup"><span data-stu-id="a9d45-133">`UserClaim` - represents a claim that a user possess</span></span>
* <span data-ttu-id="a9d45-134">`UserToken` -bir kullanıcı için bir kimlik doğrulama belirteci temsil eder</span><span class="sxs-lookup"><span data-stu-id="a9d45-134">`UserToken` - represents an authentication token for a user</span></span>
* <span data-ttu-id="a9d45-135">`UserLogin` -Kullanıcı bir oturum ile ilişkilendirir</span><span class="sxs-lookup"><span data-stu-id="a9d45-135">`UserLogin` - associates a user with a login</span></span>
* <span data-ttu-id="a9d45-136">`RoleClaim` -bir rolü içindeki tüm kullanıcılara verilen talebi temsil eder</span><span class="sxs-lookup"><span data-stu-id="a9d45-136">`RoleClaim` - represents a claim that is granted to all users within a role</span></span>
* <span data-ttu-id="a9d45-137">`UserRole` -Kullanıcılar ve roller ilişkilendirir varlık katılma</span><span class="sxs-lookup"><span data-stu-id="a9d45-137">`UserRole` - join entity that associates users and roles</span></span>

### <a name="entity-type-relationships"></a><span data-ttu-id="a9d45-138">Varlık türü ilişkileri</span><span class="sxs-lookup"><span data-stu-id="a9d45-138">Entity type relationships</span></span>

<span data-ttu-id="a9d45-139">Bu varlık türleri, aşağıdaki yollarla birbiriyle:</span><span class="sxs-lookup"><span data-stu-id="a9d45-139">These entity types are related to each other in the following ways:</span></span>

* <span data-ttu-id="a9d45-140">Her `User` birçok olabilir `UserClaims`</span><span class="sxs-lookup"><span data-stu-id="a9d45-140">Each `User` can have many `UserClaims`</span></span>
* <span data-ttu-id="a9d45-141">Her `User` birçok olabilir `UserLogins`</span><span class="sxs-lookup"><span data-stu-id="a9d45-141">Each `User` can have many `UserLogins`</span></span>
* <span data-ttu-id="a9d45-142">Her `User` birçok olabilir `UserTokens`</span><span class="sxs-lookup"><span data-stu-id="a9d45-142">Each `User` can have many `UserTokens`</span></span>
* <span data-ttu-id="a9d45-143">Her `Role` olabilir birçok ilişkili `RoleClaims`</span><span class="sxs-lookup"><span data-stu-id="a9d45-143">Each `Role` can have many associated `RoleClaims`</span></span>
* <span data-ttu-id="a9d45-144">Her `User` olabilir birçok ilişkili `Roles`ve her `Role` birden çok kullanıcıya sahip ilişkilendirilebilir</span><span class="sxs-lookup"><span data-stu-id="a9d45-144">Each `User` can have many associated `Roles`, and each `Role` can be associated with many Users</span></span>
  * <span data-ttu-id="a9d45-145">Veritabanındaki bir birleşim tablosuna gerektiren çok-çok ilişkisi budur.</span><span class="sxs-lookup"><span data-stu-id="a9d45-145">This is a many-to-many relationship, which requires a join table in the database.</span></span> <span data-ttu-id="a9d45-146">Birleştirme tablosu tarafından temsil edilen `UserRole` varlık.</span><span class="sxs-lookup"><span data-stu-id="a9d45-146">The join table is represented by the `UserRole` entity.</span></span>

### <a name="default-model-configuration"></a><span data-ttu-id="a9d45-147">Varsayılan model yapılandırma</span><span class="sxs-lookup"><span data-stu-id="a9d45-147">Default model configuration</span></span>

<span data-ttu-id="a9d45-148">Kimliği "öğesinden devralan bağlam sınıflarını" çeşitli tanımlar [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) yapılandırıp modeli kullanır.</span><span class="sxs-lookup"><span data-stu-id="a9d45-148">Identity defines a variety of "context classes" which inherit from [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) to configure and use the model.</span></span> <span data-ttu-id="a9d45-149">Bu yapılandırma yapılır kullanarak [EF çekirdek Code First Fluent API'SİNDE](/ef/core/modeling/) içinde [OnModelCreating](/dotnet/api/microsoft.entityframeworkcore.dbcontext.onmodelcreating#Microsoft_EntityFrameworkCore_DbContext_OnModelCreating_Microsoft_EntityFrameworkCore_ModelBuilder_) bağlamı sınıfının yöntemi.</span><span class="sxs-lookup"><span data-stu-id="a9d45-149">This configuration is done using the [EF Core Code First Fluent API](/ef/core/modeling/) in the [OnModelCreating](/dotnet/api/microsoft.entityframeworkcore.dbcontext.onmodelcreating#Microsoft_EntityFrameworkCore_DbContext_OnModelCreating_Microsoft_EntityFrameworkCore_ModelBuilder_) method of the context class.</span></span> <span data-ttu-id="a9d45-150">Varsayılan yapılandırmadır:</span><span class="sxs-lookup"><span data-stu-id="a9d45-150">The default configuration is:</span></span>

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

### <a name="model-generic-types"></a><span data-ttu-id="a9d45-151">Model genel türler</span><span class="sxs-lookup"><span data-stu-id="a9d45-151">Model generic types</span></span>

<span data-ttu-id="a9d45-152">Kimlik, yukarıda listelenen Varlık türlerinin her biri için varsayılan CLR türlerini tanımlar.</span><span class="sxs-lookup"><span data-stu-id="a9d45-152">Identity defines default CLR types for each of the entity types listed above.</span></span> <span data-ttu-id="a9d45-153">Bu tür tüm "Kimliği ile" önek: `IdentityUser`, `IdentityRole`, `IdentityUserClaim`, `IdentityUserToken`, `IdentityUserLogin`, `IdentityRoleClaim`, ve `IdentityUserRole`.</span><span class="sxs-lookup"><span data-stu-id="a9d45-153">These types are all prefixed with "Identity": `IdentityUser`, `IdentityRole`, `IdentityUserClaim`, `IdentityUserToken`, `IdentityUserLogin`, `IdentityRoleClaim`, and `IdentityUserRole`.</span></span>

<span data-ttu-id="a9d45-154">Bu tür doğrudan kullanmak yerine türleri temel sınıfları uygulamanın kendi türleri için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="a9d45-154">Rather than using these types directly, the types can be used as base classes for the application's own types.</span></span> <span data-ttu-id="a9d45-155">`DbContext` Kimliği tarafından tanımlanan sınıflardır genel sağlayacak şekilde bir veya daha fazla modeldeki varlık türü için farklı CLR türleri kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="a9d45-155">The `DbContext` classes defined by Identity are generic such that different CLR types can be used for one or more of the entity types in the model.</span></span> <span data-ttu-id="a9d45-156">Bu genel türü Ayrıca kullanıcı birincil anahtar türü için değiştirilmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="a9d45-156">These generic types also allow for the type of the User primary key to be changed.</span></span>

<span data-ttu-id="a9d45-157">Kimlik desteğiyle rolleri için kullanılırken bir [Identitydbcontext](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identitydbcontext) sınıfı kullanılmalıdır:</span><span class="sxs-lookup"><span data-stu-id="a9d45-157">When using Identity with support for roles, an [IdentityDbContext](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identitydbcontext) class should be used:</span></span>

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

<span data-ttu-id="a9d45-158">(Yalnızca talepler) rollerini; bu durumda kimlik kullanmak da mümkündür bir `IdentityUserContext` sınıfı kullanılmalıdır:</span><span class="sxs-lookup"><span data-stu-id="a9d45-158">It is also possible to use Identity without roles (only claims), in which case an `IdentityUserContext` class should be used:</span></span>


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

## <a name="customizing-the-model"></a><span data-ttu-id="a9d45-159">Model özelleştirme</span><span class="sxs-lookup"><span data-stu-id="a9d45-159">Customizing the model</span></span>

<span data-ttu-id="a9d45-160">Model özelleştirmek için uygun bağlamı türünden türemesi için başlangıçtır; önceki bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="a9d45-160">The starting point for customizing the model is to derive from the appropriate context type; see the preceding section.</span></span> <span data-ttu-id="a9d45-161">Bu içerik türü geleneksel olarak adlandırılır `ApplicationDbContext` ve ASP.NET Core şablonları tarafından oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="a9d45-161">This context type is customarily called `ApplicationDbContext` and is created by the ASP.NET Core templates.</span></span>

<span data-ttu-id="a9d45-162">Bağlam model iki şekilde yapılandırmak için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="a9d45-162">The context is used to configure the model in two ways:</span></span>
* <span data-ttu-id="a9d45-163">Varlık ve anahtar türleri için genel tür parametreleri sağlayarak.</span><span class="sxs-lookup"><span data-stu-id="a9d45-163">By supplying entity and key types for the generic type parameters.</span></span>
* <span data-ttu-id="a9d45-164">Geçersiz kılma tarafından `OnModelCreating` bu tür eşlemesi değiştirmek için.</span><span class="sxs-lookup"><span data-stu-id="a9d45-164">By overriding `OnModelCreating` to modify the mapping of these types.</span></span>

<span data-ttu-id="a9d45-165">Geçersiz kılarken `OnModelCreating`, `base.OnModelCreating` ilk olarak, geçersiz kılma çağrılmalıdır yapılandırma çağrılabilir sonraki.</span><span class="sxs-lookup"><span data-stu-id="a9d45-165">When overriding `OnModelCreating`, `base.OnModelCreating` should be called first, the overriding configuration should be called next.</span></span> <span data-ttu-id="a9d45-166">EF çekirdek yapılandırması için bir son bir WINS ilkesi genellikle sahiptir.</span><span class="sxs-lookup"><span data-stu-id="a9d45-166">EF Core generally has a last-one-wins policy for configuration.</span></span> <span data-ttu-id="a9d45-167">Örneğin, varsa `ToTable` için bir varlık türü farklı bir tablo adı ile daha sonra yeniden ve adlı bir tablo adı ile ilk sonra tablo ikinci çağrısında kullanılan bir addır.</span><span class="sxs-lookup"><span data-stu-id="a9d45-167">For example, if the `ToTable` for an entity type is called first with one table name and then again later with a different table name, then the table name in the second call is the one that is used.</span></span>

### <a name="using-a-custom-user-type"></a><span data-ttu-id="a9d45-168">Özel bir kullanıcı türü kullanma</span><span class="sxs-lookup"><span data-stu-id="a9d45-168">Using a custom User type</span></span>

<span data-ttu-id="a9d45-169">Özel bir kullanıcı türü kullanmak için türü oluşturursanız ve onu devralınmalıdır `IdentityUser`.</span><span class="sxs-lookup"><span data-stu-id="a9d45-169">To use a custom User type, create the type and have it inherit from `IdentityUser`.</span></span> <span data-ttu-id="a9d45-170">Bu tür adı uygulamadır `ApplicationUser`.</span><span class="sxs-lookup"><span data-stu-id="a9d45-170">It is customary to name this type `ApplicationUser`.</span></span> <span data-ttu-id="a9d45-171">Bu tür uygulamaların tipik olarak değil temel türdeki ek özellikler vardır, aksi takdirde olurdu hiçbir değer oluşturma içinde.</span><span class="sxs-lookup"><span data-stu-id="a9d45-171">This type will typically have additional properties not in the base type, otherwise there would be no value in creating it.</span></span> <span data-ttu-id="a9d45-172">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="a9d45-172">For example:</span></span>

```CSharp
public class ApplicationUser : IdentityUser
{
    public string CustomTag { get; set; }
}
```

<span data-ttu-id="a9d45-173">Sonraki bağlam için genel bir bağımsız değişken olarak bu türü kullanın:</span><span class="sxs-lookup"><span data-stu-id="a9d45-173">Next use this type as a generic argument for the context:</span></span>

```CSharp
public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }
}
```

<span data-ttu-id="a9d45-174">Güncelleştirme `ConfigureServices` yeni `ApplicationUser` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="a9d45-174">Update `ConfigureServices` to use the new `ApplicationUser` class:</span></span>

```CSharp
services.AddDefaultIdentity<ApplicationUser>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

<span data-ttu-id="a9d45-175">Geçersiz kılmak için gerek yoktur `OnModelCreating` EF çekirdek eşler için burada `CustomTag` kural tarafından özelliği.</span><span class="sxs-lookup"><span data-stu-id="a9d45-175">There is no need to override `OnModelCreating` here because EF Core will map the `CustomTag` property by convention.</span></span> <span data-ttu-id="a9d45-176">Ancak, veritabanını yeni bir almak için güncelleştirilmesi gerekecek `CustomTag` sütun.</span><span class="sxs-lookup"><span data-stu-id="a9d45-176">However, the database will need to be updated to get a new `CustomTag` column.</span></span> <span data-ttu-id="a9d45-177">Bunu yapmak için bir geçiş ekleyin ve ardından veritabanı açıklandığı gibi güncelleştirin [kimlik ve EF çekirdek geçişler](#identity-migrations).</span><span class="sxs-lookup"><span data-stu-id="a9d45-177">To do this, add a migration, and then update the database as described in  [Identity and EF Core Migrations](#identity-migrations).</span></span>

### <a name="changing-the-key-type"></a><span data-ttu-id="a9d45-178">Anahtar türü değiştirme</span><span class="sxs-lookup"><span data-stu-id="a9d45-178">Changing the key type</span></span>

<span data-ttu-id="a9d45-179">Veritabanı oluşturulduktan sonra bir birincil anahtar (PK) sütunun türünü değiştirme birçok veritabanı sistemlerinde sorunlu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a9d45-179">Changing the type of a primary key (PK) column after the database has been created is problematic on many database systems.</span></span> <span data-ttu-id="a9d45-180">BA genellikle bırakarak ve tablo yeniden oluşturma değiştirilmesidir.</span><span class="sxs-lookup"><span data-stu-id="a9d45-180">Changing the PK typically involves dropping and re-creating the table.</span></span> <span data-ttu-id="a9d45-181">Bu nedenle, veritabanı oluşturulduğunda, hedef anahtar türleri oluşturulur, anahtar türleri ilk geçişte belirtilmesi önerilir.</span><span class="sxs-lookup"><span data-stu-id="a9d45-181">Therefore, it is recommended that key types be specified in the initial migration such that the target key types are created when the database is created.</span></span>

<span data-ttu-id="a9d45-182">Veritabanını, sonra oluşturulduysa `Drop-Database` (PMC) veya `dotnet ef database drop` (.NET Core CLI) silmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="a9d45-182">If the database has been created, then `Drop-Database` (PMC) or `dotnet ef database drop` (.NET Core CLI) can be used to delete it.</span></span>

<span data-ttu-id="a9d45-183">Veritabanı yok onaylandıktan sonra ilk geçişle kaldırmak `Remove-Migration` (PMC) veya `dotnet ef migrations remove` (.NET Core CLI).</span><span class="sxs-lookup"><span data-stu-id="a9d45-183">Once it is confirmed that the database doesn't exist,  remove the initial migration with `Remove-Migration` (PMC) or `dotnet ef migrations remove` (.NET Core CLI).</span></span>

<span data-ttu-id="a9d45-184">Güncelleştirme `ApplicationDbContext` için yeni anahtar türü belirterek farklı bir temel sınıfı kullanmak için `TKey`.</span><span class="sxs-lookup"><span data-stu-id="a9d45-184">Update the `ApplicationDbContext` to use a different base class, specifying the new key type for `TKey`.</span></span> <span data-ttu-id="a9d45-185">Örneğin, kullanılacak bir `Guid` anahtarı:</span><span class="sxs-lookup"><span data-stu-id="a9d45-185">For example, to use a `Guid` key:</span></span>

```CSharp
public class ApplicationDbContext : IdentityDbContext<IdentityUser<Guid>, IdentityRole<Guid>, Guid>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }
}
```

<span data-ttu-id="a9d45-186">Dikkat Genel sınıflar `IdentityUser<TKey>` ve `IdentityRole<TKey>` yeni anahtar türünü kullanmak için de belirtilmelidir.</span><span class="sxs-lookup"><span data-stu-id="a9d45-186">Notice that the generic classes `IdentityUser<TKey>` and `IdentityRole<TKey>` must also be specified to use the new key type.</span></span> <span data-ttu-id="a9d45-187">`ConfigureServices` Genel kullanıcı kullanacak şekilde güncelleştirilmesi gerekir:</span><span class="sxs-lookup"><span data-stu-id="a9d45-187">`ConfigureServices` must be updated to use the generic user:</span></span>

```CSharp
services.AddDefaultIdentity<IdentityUser<Guid>>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

<span data-ttu-id="a9d45-188">Özel bir varsa `ApplicationUser` olan kullanılmakta devralınacak güncelleştirmek `IdentityUser<TKey>`.</span><span class="sxs-lookup"><span data-stu-id="a9d45-188">If a custom `ApplicationUser` is being used, update it to inherit from `IdentityUser<TKey>`.</span></span> <span data-ttu-id="a9d45-189">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="a9d45-189">For example:</span></span>

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

### <a name="adding-navigation-properties"></a><span data-ttu-id="a9d45-190">Gezinti özellikleri ekleme</span><span class="sxs-lookup"><span data-stu-id="a9d45-190">Adding navigation properties</span></span>

<span data-ttu-id="a9d45-191">İlişkiler için model yapılandırmasını değiştirme bir daha zor başka değişiklikler yaparak daha olabilir.</span><span class="sxs-lookup"><span data-stu-id="a9d45-191">Changing the model configuration for relationships can be a more difficult than making other changes.</span></span> <span data-ttu-id="a9d45-192">Var olan ilişkileri değiştirmek yerine yeni bir ek ilişkiler oluşturmak için dikkatli olunması gerekir.</span><span class="sxs-lookup"><span data-stu-id="a9d45-192">Care must be taken to replace the existing relationships rather than create a new additional relationships.</span></span> <span data-ttu-id="a9d45-193">Özellikle, değiştirilen ilişki aynı yabancı anahtar özelliği var olan bir ilişki olarak belirtmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="a9d45-193">In particular, the changed relationship must specify the same foreign key property as the existing relationship.</span></span> <span data-ttu-id="a9d45-194">Örneğin, arasındaki ilişkiyi `Users` ve `UserClaims` olarak belirtilen varsayılan olarak seçilir:</span><span class="sxs-lookup"><span data-stu-id="a9d45-194">For example, the relationship between `Users` and `UserClaims` is by default specified as:</span></span>

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

<span data-ttu-id="a9d45-195">Bu ilişki için yabancı anahtar olarak belirtilen `UserClaim.UserId` özelliği.</span><span class="sxs-lookup"><span data-stu-id="a9d45-195">The foreign key for this relationship is specified as the `UserClaim.UserId` property.</span></span> <span data-ttu-id="a9d45-196">`HasMany` ve `WithOne` Gezinti özellikleri olmayan ilişki oluşturmak için bağımsız değişkenler olmadan denir.</span><span class="sxs-lookup"><span data-stu-id="a9d45-196">`HasMany` and `WithOne` are called without arguments to create the relationship without navigation properties.</span></span>

<span data-ttu-id="a9d45-197">Bir gezinti özelliği Ekle `ApplicationUser` izin ilişkili `UserClaims` kullanıcıdan başvurulacak:</span><span class="sxs-lookup"><span data-stu-id="a9d45-197">Add a navigation property to `ApplicationUser` that will allow associated `UserClaims` to be referenced from the user:</span></span>

```CSharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<IdentityUserClaim<string>> Claims { get; set; }
}
```

<span data-ttu-id="a9d45-198">Unutmayın `TKey` için `IdentityUserClaim<TKey>` kullanıcıları--bu durumda, birincil anahtar için belirtilen tür `string` biz Varsayılanları kullandığımızdan.</span><span class="sxs-lookup"><span data-stu-id="a9d45-198">Note that the `TKey` for `IdentityUserClaim<TKey>` is type specified for the primary key of users--in this case `string` since we're using the defaults.</span></span> <span data-ttu-id="a9d45-199">Bu **değil** için birincil anahtar türü `UserClaim` varlık türü.</span><span class="sxs-lookup"><span data-stu-id="a9d45-199">It is **not** the primary key type for the `UserClaim` entity type.</span></span>

<span data-ttu-id="a9d45-200">Gezinti özelliği var. göre de yapılandırılmalıdır `OnModelCreating`:</span><span class="sxs-lookup"><span data-stu-id="a9d45-200">Now that the navigation property exists it must be configured in `OnModelCreating`:</span></span>

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

<span data-ttu-id="a9d45-201">Daha önce yalnızca çağrısında belirtilen bir gezinti özelliğine sahip olduğu gibi ilişki yapılandırılmış fark `HasMany`.</span><span class="sxs-lookup"><span data-stu-id="a9d45-201">Notice that relationship is configured exactly as it was before, only with a navigation property specified in the call to `HasMany`.</span></span>

<span data-ttu-id="a9d45-202">Gezinti özellikleri yalnızca EF modeli, veritabanı yok.</span><span class="sxs-lookup"><span data-stu-id="a9d45-202">The navigation properties only exist in the EF model, not the database.</span></span> <span data-ttu-id="a9d45-203">Yabancı anahtar ilişkisi için değişmediği için bu tür bir model değişikliği güncelleştirilmesi veritabanına gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="a9d45-203">Because the foreign key for the relationship has not changed, this kind of model change does not require the database to be updated.</span></span> <span data-ttu-id="a9d45-204">Bu değişikliği yaptıktan sonra geçiş ekleyerek denetlenebilir: `Up` ve `Down` yöntemleri boş.</span><span class="sxs-lookup"><span data-stu-id="a9d45-204">This can be checked by adding a migration after making the change: the `Up` and `Down` methods are empty.</span></span>

### <a name="adding-all-user-navigation-properties"></a><span data-ttu-id="a9d45-205">Tüm kullanıcı Gezinti özellikleri ekleme</span><span class="sxs-lookup"><span data-stu-id="a9d45-205">Adding all User navigation properties</span></span>

<span data-ttu-id="a9d45-206">Yukarıdaki bölümü kılavuz olarak kullanarak, kullanıcı tüm ilişkiler için tek yönlü gezinme özelliklerinin yapılandıran örnek aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="a9d45-206">Using the section above as guidance, here is an example that configures unidirectional navigation properties for all relationships on User:</span></span>

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

### <a name="adding-user-and-role-navigation-properties"></a><span data-ttu-id="a9d45-207">Kullanıcı ve rol Gezinti özellikleri ekleme</span><span class="sxs-lookup"><span data-stu-id="a9d45-207">Adding User and Role navigation properties</span></span>

<span data-ttu-id="a9d45-208">Yukarıdaki bölümü kılavuz olarak kullanarak, kullanıcı ve rol Gezinti özellikleri için tüm ilişkiler yapılandıran örnek aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="a9d45-208">Using the section above as guidance, here is an example that configures navigation properties for all relationships on User and Role:</span></span>

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

<span data-ttu-id="a9d45-209">Notlar:</span><span class="sxs-lookup"><span data-stu-id="a9d45-209">Notes:</span></span>

* <span data-ttu-id="a9d45-210">Bu örnek ayrıca içerir `UserRole` katılma, kullanıcıların çok-çok ilişkisi rollere gitmek için gerekli olan varlık.</span><span class="sxs-lookup"><span data-stu-id="a9d45-210">This example also includes the `UserRole` join entity, which is needed to navigate the many-to-many relationship from Users to Roles.</span></span>
* <span data-ttu-id="a9d45-211">Yansıtacak şekilde gezinti özellikleri türlerini değiştirmeyi unutmayın `ApplicationXxx` türleri şimdi kullanıldığından yerine `IdentityXxx` türleri.</span><span class="sxs-lookup"><span data-stu-id="a9d45-211">Remember to change the types of the navigation properties to reflect that `ApplicationXxx` types are now being used instead of `IdentityXxx` types.</span></span>
* <span data-ttu-id="a9d45-212">Kullanmayı unutmayın `ApplicationXxx` genel olarak `ApplicationContext` tanımı.</span><span class="sxs-lookup"><span data-stu-id="a9d45-212">Remember to use the `ApplicationXxx` in the generic `ApplicationContext` definition.</span></span>

### <a name="adding-all-navigation-properties"></a><span data-ttu-id="a9d45-213">Tüm gezinti özellikleri ekleme</span><span class="sxs-lookup"><span data-stu-id="a9d45-213">Adding all navigation properties</span></span>

<span data-ttu-id="a9d45-214">Yukarıdaki bölümü kılavuz olarak kullanarak, gezinti özellikleri için tüm ilişkiler üzerinde tüm varlık türlerinin yapılandıran örnek aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="a9d45-214">Using the section above as guidance, here is an example that configures navigation properties for all relationships on all entity types:</span></span>

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

### <a name="using-composite-keys"></a><span data-ttu-id="a9d45-215">Bileşik anahtarlar kullanarak</span><span class="sxs-lookup"><span data-stu-id="a9d45-215">Using composite keys</span></span>

<span data-ttu-id="a9d45-216">Önceki bölümlerde kimlik modelinde kullanılan anahtar türü değiştirme gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="a9d45-216">The preceding sections demonstrated changing the type of key used in the Identity model.</span></span> <span data-ttu-id="a9d45-217">Bileşik anahtarlar kullanılacak kimlik anahtar modelini değiştirme önerilen ve desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="a9d45-217">Changing the Identity key model to use composite keys is not supported or recommended.</span></span> <span data-ttu-id="a9d45-218">Kimlikle bileşik bir anahtar kullanarak kimlik yöneticisi kodu desteklenmeyen bir özelleştirmedir modeli ile ve bu belgenin kapsamı dışında nasıl etkileşim kurduğu değiştirme içerir.</span><span class="sxs-lookup"><span data-stu-id="a9d45-218">Using a composite key with Identity would involve changing how the Identity manager code interacts with the model, which is an unsupported customization and beyond the scope of this document.</span></span>

### <a name="changing-tablecolumn-names-and-facets"></a><span data-ttu-id="a9d45-219">Tablo/sütun adları ve modelleri değiştirme</span><span class="sxs-lookup"><span data-stu-id="a9d45-219">Changing table/column names and facets</span></span>

<span data-ttu-id="a9d45-220">Tablolar ve sütunlar adlarını değiştirmek için arama `base.OnModelCreating`ve tüm varsayılanları geçersiz kılmak için yapılandırma ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a9d45-220">To change the names of tables and columns, call `base.OnModelCreating`, and then add configuration to override any of the defaults.</span></span> <span data-ttu-id="a9d45-221">Örneğin, tüm kimlik tabloları adını değiştirmek için şunu yazın:</span><span class="sxs-lookup"><span data-stu-id="a9d45-221">For example, to change the name of all the identity tables:</span></span>

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

<span data-ttu-id="a9d45-222">Bu örnekler varsayılan kimlik türlerinin kullandığını unutmayın.</span><span class="sxs-lookup"><span data-stu-id="a9d45-222">Note that these examples use the default Identity types.</span></span> <span data-ttu-id="a9d45-223">Uygulama türü gibi kullanıyorsanız `ApplicationUser` varsayılan türü yerine bu tür yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="a9d45-223">If you are using an application type, such as `ApplicationUser` then configure that type instead of the default type.</span></span>

<span data-ttu-id="a9d45-224">Bazı sütun adlarını değiştiren örnek aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="a9d45-224">Here is an example that changes some column names:</span></span>

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

<span data-ttu-id="a9d45-225">Bazı veritabanı sütunlarının türleri belirli ile yapılandırılabilir _modelleri_ gibi izin verilen maksimum dize uzunluğu.</span><span class="sxs-lookup"><span data-stu-id="a9d45-225">Some types of database columns can be configured with certain _facets_ such as the maximum string length allowed.</span></span> <span data-ttu-id="a9d45-226">Modelde birkaç dize özellikleri için sütun en çok uzunlukları ayarlayan örnek aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="a9d45-226">Here is an example that sets column max lengths for several string properties in the model:</span></span>

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

### <a name="mapping-to-a-different-schema"></a><span data-ttu-id="a9d45-227">Farklı bir şema eşleme</span><span class="sxs-lookup"><span data-stu-id="a9d45-227">Mapping to a different schema</span></span>

<span data-ttu-id="a9d45-228">Şemalar farklı veritabanı sağlayıcıları farklı davranabilir, ancak SQL Server için tüm tabloları "dbo" şemada oluşturmak için varsayılandır.</span><span class="sxs-lookup"><span data-stu-id="a9d45-228">Schemas can behave differently in different database providers, but for SQL Server, the default is to create all tables in the "dbo" schema.</span></span> <span data-ttu-id="a9d45-229">Bunun yerine farklı bir şemaya tablolar oluşturmak için değiştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="a9d45-229">This can be changed to instead create the tables in a different schema.</span></span> <span data-ttu-id="a9d45-230">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="a9d45-230">For example:</span></span>

```CSharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    base.OnModelCreating(modelBuilder);

    modelBuilder.HasDefaultSchema("notdbo");
}
```

### <a name="lazy-loading"></a><span data-ttu-id="a9d45-231">Yavaş yükleniyor</span><span class="sxs-lookup"><span data-stu-id="a9d45-231">Lazy loading</span></span>

<span data-ttu-id="a9d45-232">Bu bölümde kimlik modeli yükleme yavaş proxy'leri için destek eklendi.</span><span class="sxs-lookup"><span data-stu-id="a9d45-232">In this section support for lazy-loading proxies in the Identity model is added.</span></span> <span data-ttu-id="a9d45-233">Yüklü olan ilk sağlama olmadan kullanılacak gezinti özellikleri olanak tanıdığından Lazy yüklenirken kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="a9d45-233">Lazy-loading is useful since it allows navigation properties to be used without first ensuring they are loaded.</span></span>

<span data-ttu-id="a9d45-234">Varlık türleri yapılabilir uygun çeşitli yollarla yavaş yükleniyor açıklandığı gibi [EF Core belgeleri](/ef/core/querying/related-data#lazy-loading).</span><span class="sxs-lookup"><span data-stu-id="a9d45-234">Entity types can be made suitable for lazy-loading in several ways, as described in the [EF Core documentation](/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="a9d45-235">Kolaylık olması için yükleme yavaş proxy'leri gerektiren kullanırız:</span><span class="sxs-lookup"><span data-stu-id="a9d45-235">For simplicity, we will use lazy-loading proxies, which requires:</span></span>

* <span data-ttu-id="a9d45-236">Yüklemesini [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) paket.</span><span class="sxs-lookup"><span data-stu-id="a9d45-236">Installation of the [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) package.</span></span>
* <span data-ttu-id="a9d45-237">Çağrı `.UseLazyLoadingProxies()` içinde `AddDbContext`.</span><span class="sxs-lookup"><span data-stu-id="a9d45-237">A call to `.UseLazyLoadingProxies()` inside `AddDbContext`.</span></span>
* <span data-ttu-id="a9d45-238">Genel sanal Gezinti özellikleri genel varlık türleriyle.</span><span class="sxs-lookup"><span data-stu-id="a9d45-238">Public entity types with public virtual navigation properties.</span></span>

<span data-ttu-id="a9d45-239">Çağırma örneği `.UseLazyLoadingProxies()`:</span><span class="sxs-lookup"><span data-stu-id="a9d45-239">Here is an example of calling `.UseLazyLoadingProxies()`:</span></span>

```CSharp
services
    .AddDbContext<ApplicationDbContext>(
        b => b.UseSqlServer(connectionString)
            .UseLazyLoadingProxies())
    .AddDefaultIdentity<ApplicationUser>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

<span data-ttu-id="a9d45-240">Geri gezinti özellikleri için varlık türlerini eklemek için örnekler bakın.</span><span class="sxs-lookup"><span data-stu-id="a9d45-240">Refer back to the examples above for adding navigation properties to the entity types.</span></span>
