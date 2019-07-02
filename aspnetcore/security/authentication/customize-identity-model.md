---
title: ASP.NET core'da kimlik modeli özelleştirme
author: ajcvickers
description: Bu makalede, ASP.NET Core kimliği için Entity Framework Core veri modeli özelleştirmeyi açıklar.
ms.author: avickers
ms.date: 07/01/2019
uid: security/authentication/customize_identity_model
ms.openlocfilehash: f549fdff4a416b5fadcb2b1078b051bbab8e402e
ms.sourcegitcommit: eb3e51d58dd713eefc242148f45bd9486be3a78a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67500482"
---
# <a name="identity-model-customization-in-aspnet-core"></a><span data-ttu-id="25c5a-103">ASP.NET core'da kimlik modeli özelleştirme</span><span class="sxs-lookup"><span data-stu-id="25c5a-103">Identity model customization in ASP.NET Core</span></span>

<span data-ttu-id="25c5a-104">Tarafından [Arthur Vickers](https://github.com/ajcvickers)</span><span class="sxs-lookup"><span data-stu-id="25c5a-104">By [Arthur Vickers](https://github.com/ajcvickers)</span></span>

<span data-ttu-id="25c5a-105">ASP.NET Core kimlik yönetimi ve ASP.NET Core uygulamalarında kullanıcı hesaplarını depolamak için bir çerçeve sunar.</span><span class="sxs-lookup"><span data-stu-id="25c5a-105">ASP.NET Core Identity provides a framework for managing and storing user accounts in ASP.NET Core apps.</span></span> <span data-ttu-id="25c5a-106">Kimlik, projenize eklenir, **bireysel kullanıcı hesapları** kimlik doğrulama mekanizması olarak seçilir.</span><span class="sxs-lookup"><span data-stu-id="25c5a-106">Identity is added to your project when **Individual User Accounts** is selected as the authentication mechanism.</span></span> <span data-ttu-id="25c5a-107">Varsayılan olarak, kimlik bir Entity Framework (EF), yararlanır çekirdek veri modeli.</span><span class="sxs-lookup"><span data-stu-id="25c5a-107">By default, Identity makes use of an Entity Framework (EF) Core data model.</span></span> <span data-ttu-id="25c5a-108">Bu makalede, kimlik modeli özelleştirmeyi açıklar.</span><span class="sxs-lookup"><span data-stu-id="25c5a-108">This article describes how to customize the Identity model.</span></span>

## <a name="identity-and-ef-core-migrations"></a><span data-ttu-id="25c5a-109">Kimlik ve EF Core geçişleri</span><span class="sxs-lookup"><span data-stu-id="25c5a-109">Identity and EF Core Migrations</span></span>

<span data-ttu-id="25c5a-110">Model İnceleme önce kimlik'ile nasıl çalıştığını anlamak kullanışlıdır [EF Core geçişleri](/ef/core/managing-schemas/migrations/) bir veritabanı oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="25c5a-110">Before examining the model, it's useful to understand how Identity works with [EF Core Migrations](/ef/core/managing-schemas/migrations/) to create and update a database.</span></span> <span data-ttu-id="25c5a-111">En üst düzeyinde bir işlemdir:</span><span class="sxs-lookup"><span data-stu-id="25c5a-111">At the top level, the process is:</span></span>

1. <span data-ttu-id="25c5a-112">Tanımlayın veya güncelleştirme bir [kod veri modelinde](/ef/core/modeling/).</span><span class="sxs-lookup"><span data-stu-id="25c5a-112">Define or update a [data model in code](/ef/core/modeling/).</span></span>
1. <span data-ttu-id="25c5a-113">Bu model, veritabanına uygulanabilir değişiklikleri küçültmesini bir geçiş ekleyin.</span><span class="sxs-lookup"><span data-stu-id="25c5a-113">Add a Migration to translate this model into changes that can be applied to the database.</span></span>
1. <span data-ttu-id="25c5a-114">Onay geçiş amacınızı doğru şekilde temsil eder.</span><span class="sxs-lookup"><span data-stu-id="25c5a-114">Check that the Migration correctly represents your intentions.</span></span>
1. <span data-ttu-id="25c5a-115">Geçiş modeli ile eşitlenmiş olmasını veritabanını güncellemek için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="25c5a-115">Apply the Migration to update the database to be in sync with the model.</span></span>
1. <span data-ttu-id="25c5a-116">1 ile daha fazla model iyileştirmek ve veritabanı eşitlenmiş halde tutun için 4 arasındaki adımları yineleyin.</span><span class="sxs-lookup"><span data-stu-id="25c5a-116">Repeat steps 1 through 4 to further refine the model and keep the database in sync.</span></span>

<span data-ttu-id="25c5a-117">Ekleme ve geçiş uygulamak için aşağıdaki yaklaşımlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="25c5a-117">Use one of the following approaches to add and apply Migrations:</span></span>

* <span data-ttu-id="25c5a-118">**Paket Yöneticisi Konsolu** Visual Studio kullanıyorsanız (PMC) penceresi.</span><span class="sxs-lookup"><span data-stu-id="25c5a-118">The **Package Manager Console** (PMC) window if using Visual Studio.</span></span> <span data-ttu-id="25c5a-119">Daha fazla bilgi için [EF Core PMC Araçları](/ef/core/miscellaneous/cli/powershell).</span><span class="sxs-lookup"><span data-stu-id="25c5a-119">For more information, see [EF Core PMC tools](/ef/core/miscellaneous/cli/powershell).</span></span>
* <span data-ttu-id="25c5a-120">.NET Core komut satırı kullanılarak, CLI.</span><span class="sxs-lookup"><span data-stu-id="25c5a-120">The .NET Core CLI if using the command line.</span></span> <span data-ttu-id="25c5a-121">Daha fazla bilgi için [EF Core .NET komut satırı araçları](/ef/core/miscellaneous/cli/dotnet).</span><span class="sxs-lookup"><span data-stu-id="25c5a-121">For more information, see [EF Core .NET command line tools](/ef/core/miscellaneous/cli/dotnet).</span></span>
* <span data-ttu-id="25c5a-122">Tıklayarak **geçerli geçişleri** düğmesi uygulamayı çalıştırdığınızda hata sayfasında.</span><span class="sxs-lookup"><span data-stu-id="25c5a-122">Clicking the **Apply Migrations** button on the error page when the app is run.</span></span>

<span data-ttu-id="25c5a-123">ASP.NET Core geliştirme zamanı hata sayfası işleyicisine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="25c5a-123">ASP.NET Core has a development-time error page handler.</span></span> <span data-ttu-id="25c5a-124">Uygulamayı çalıştırdığınızda, işleyici geçişler uygulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="25c5a-124">The handler can apply migrations when the app is run.</span></span> <span data-ttu-id="25c5a-125">Üretim uygulamaları, genellikle geçişleri SQL komut dosyaları üret ve denetlenen uygulama ve dağıtım veritabanı kapsamında veritabanı değişiklikleri dağıtın.</span><span class="sxs-lookup"><span data-stu-id="25c5a-125">Production apps typically generate SQL scripts from the migrations and deploy database changes as part of a controlled app and database deployment.</span></span>

<span data-ttu-id="25c5a-126">Kimlik kullanarak yeni bir uygulama oluşturduğunuzda, 1 ve 2 numaralı adımları zaten tamamlanmış.</span><span class="sxs-lookup"><span data-stu-id="25c5a-126">When a new app using Identity is created, steps 1 and 2 above have already been completed.</span></span> <span data-ttu-id="25c5a-127">Diğer bir deyişle, ilk veri modelini zaten var ve ilk geçiş projeye eklendi.</span><span class="sxs-lookup"><span data-stu-id="25c5a-127">That is, the initial data model already exists, and the initial migration has been added to the project.</span></span> <span data-ttu-id="25c5a-128">İlk geçişten hala veritabanına uygulanması gerekiyor.</span><span class="sxs-lookup"><span data-stu-id="25c5a-128">The initial migration still needs to be applied to the database.</span></span> <span data-ttu-id="25c5a-129">İlk geçişten aşağıdaki yaklaşımlardan birini uygulanabilir:</span><span class="sxs-lookup"><span data-stu-id="25c5a-129">The initial migration can be applied via one of the following approaches:</span></span>

* <span data-ttu-id="25c5a-130">Çalıştırma `Update-Database` PMC içinde.</span><span class="sxs-lookup"><span data-stu-id="25c5a-130">Run `Update-Database` in PMC.</span></span>
* <span data-ttu-id="25c5a-131">Çalıştırma `dotnet ef database update` komut kabuğu'nda.</span><span class="sxs-lookup"><span data-stu-id="25c5a-131">Run `dotnet ef database update` in a command shell.</span></span>
* <span data-ttu-id="25c5a-132">Tıklayın **geçerli geçişleri** düğmesi uygulamayı çalıştırdığınızda hata sayfasında.</span><span class="sxs-lookup"><span data-stu-id="25c5a-132">Click the **Apply Migrations** button on the error page when the app is run.</span></span>

<span data-ttu-id="25c5a-133">Modelde değişiklikler yapıldıkça önceki adımları yineleyin.</span><span class="sxs-lookup"><span data-stu-id="25c5a-133">Repeat the preceding steps as changes are made to the model.</span></span>

## <a name="the-identity-model"></a><span data-ttu-id="25c5a-134">Kimlik modeli</span><span class="sxs-lookup"><span data-stu-id="25c5a-134">The Identity model</span></span>

### <a name="entity-types"></a><span data-ttu-id="25c5a-135">Varlık türleri</span><span class="sxs-lookup"><span data-stu-id="25c5a-135">Entity types</span></span>

<span data-ttu-id="25c5a-136">Aşağıdaki varlık türlerini kimlik modeli oluşur.</span><span class="sxs-lookup"><span data-stu-id="25c5a-136">The Identity model consists of the following entity types.</span></span>

|<span data-ttu-id="25c5a-137">varlık türü</span><span class="sxs-lookup"><span data-stu-id="25c5a-137">Entity type</span></span>|<span data-ttu-id="25c5a-138">Açıklama</span><span class="sxs-lookup"><span data-stu-id="25c5a-138">Description</span></span>                                                  |
|-----------|-------------------------------------------------------------|
|`User`     |<span data-ttu-id="25c5a-139">Kullanıcıyı temsil eder.</span><span class="sxs-lookup"><span data-stu-id="25c5a-139">Represents the user.</span></span>                                         |
|`Role`     |<span data-ttu-id="25c5a-140">Bir rolü temsil eder.</span><span class="sxs-lookup"><span data-stu-id="25c5a-140">Represents a role.</span></span>                                           |
|`UserClaim`|<span data-ttu-id="25c5a-141">Bir kullanıcının sahip olduğu bir talebi temsil eder.</span><span class="sxs-lookup"><span data-stu-id="25c5a-141">Represents a claim that a user possesses.</span></span>                    |
|`UserToken`|<span data-ttu-id="25c5a-142">Bir kullanıcı için bir kimlik doğrulama belirteci temsil eder.</span><span class="sxs-lookup"><span data-stu-id="25c5a-142">Represents an authentication token for a user.</span></span>               |
|`UserLogin`|<span data-ttu-id="25c5a-143">Bir kullanıcı bir oturum açma ile ilişkilendirir.</span><span class="sxs-lookup"><span data-stu-id="25c5a-143">Associates a user with a login.</span></span>                              |
|`RoleClaim`|<span data-ttu-id="25c5a-144">Bir roldeki tüm kullanıcılara verilen talebi temsil eder.</span><span class="sxs-lookup"><span data-stu-id="25c5a-144">Represents a claim that's granted to all users within a role.</span></span>|
|`UserRole` |<span data-ttu-id="25c5a-145">Kullanıcılar ve roller ilişkilendirir birleştirme varlık.</span><span class="sxs-lookup"><span data-stu-id="25c5a-145">A join entity that associates users and roles.</span></span>               |

### <a name="entity-type-relationships"></a><span data-ttu-id="25c5a-146">Varlık türü ilişkileri</span><span class="sxs-lookup"><span data-stu-id="25c5a-146">Entity type relationships</span></span>

<span data-ttu-id="25c5a-147">[Varlık türleri](#entity-types) aşağıdaki şekillerde birbirleriyle ilişkili:</span><span class="sxs-lookup"><span data-stu-id="25c5a-147">The [entity types](#entity-types) are related to each other in the following ways:</span></span>

* <span data-ttu-id="25c5a-148">Her `User` birçok sahip `UserClaims`.</span><span class="sxs-lookup"><span data-stu-id="25c5a-148">Each `User` can have many `UserClaims`.</span></span>
* <span data-ttu-id="25c5a-149">Her `User` birçok sahip `UserLogins`.</span><span class="sxs-lookup"><span data-stu-id="25c5a-149">Each `User` can have many `UserLogins`.</span></span>
* <span data-ttu-id="25c5a-150">Her `User` birçok sahip `UserTokens`.</span><span class="sxs-lookup"><span data-stu-id="25c5a-150">Each `User` can have many `UserTokens`.</span></span>
* <span data-ttu-id="25c5a-151">Her `Role` olabilir birçok ilişkili `RoleClaims`.</span><span class="sxs-lookup"><span data-stu-id="25c5a-151">Each `Role` can have many associated `RoleClaims`.</span></span>
* <span data-ttu-id="25c5a-152">Her `User` olabilir birçok ilişkili `Roles`ve her `Role` çoğu ile ilişkili olabilir `Users`.</span><span class="sxs-lookup"><span data-stu-id="25c5a-152">Each `User` can have many associated `Roles`, and each `Role` can be associated with many `Users`.</span></span> <span data-ttu-id="25c5a-153">Bu veritabanında bir birleşim tablosunu gerektiren bir çoktan çoğa bir ilişkidir.</span><span class="sxs-lookup"><span data-stu-id="25c5a-153">This is a many-to-many relationship that requires a join table in the database.</span></span> <span data-ttu-id="25c5a-154">Birleşim tablosu tarafından temsil edilen `UserRole` varlık.</span><span class="sxs-lookup"><span data-stu-id="25c5a-154">The join table is represented by the `UserRole` entity.</span></span>

### <a name="default-model-configuration"></a><span data-ttu-id="25c5a-155">Varsayılan model yapılandırma</span><span class="sxs-lookup"><span data-stu-id="25c5a-155">Default model configuration</span></span>

<span data-ttu-id="25c5a-156">Kimlik birçok tanımlar *bağlamı sınıfları* türünden devralınır [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) yapılandırıp modelini kullanın.</span><span class="sxs-lookup"><span data-stu-id="25c5a-156">Identity defines many *context classes* that inherit from [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) to configure and use the model.</span></span> <span data-ttu-id="25c5a-157">Bu yapılandırma yapılır kullanarak [EF Core kod ilk Fluent API'si](/ef/core/modeling/) içinde [OnModelCreating](/dotnet/api/microsoft.entityframeworkcore.dbcontext.onmodelcreating) bağlamı sınıfının yöntemi.</span><span class="sxs-lookup"><span data-stu-id="25c5a-157">This configuration is done using the [EF Core Code First Fluent API](/ef/core/modeling/) in the [OnModelCreating](/dotnet/api/microsoft.entityframeworkcore.dbcontext.onmodelcreating) method of the context class.</span></span> <span data-ttu-id="25c5a-158">Varsayılan yapılandırma verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="25c5a-158">The default configuration is:</span></span>

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

### <a name="model-generic-types"></a><span data-ttu-id="25c5a-159">Model genel türler</span><span class="sxs-lookup"><span data-stu-id="25c5a-159">Model generic types</span></span>

<span data-ttu-id="25c5a-160">Varsayılan kimlik tanımlar [ortak dil çalışma zamanı](/dotnet/standard/glossary#clr) yukarıda listelenen her bir varlık türü için (CLR) türleri.</span><span class="sxs-lookup"><span data-stu-id="25c5a-160">Identity defines default [Common Language Runtime](/dotnet/standard/glossary#clr) (CLR) types for each of the entity types listed above.</span></span> <span data-ttu-id="25c5a-161">Bu tür tüm ön eki *kimlik*:</span><span class="sxs-lookup"><span data-stu-id="25c5a-161">These types are all prefixed with *Identity*:</span></span>

* `IdentityUser`
* `IdentityRole`
* `IdentityUserClaim`
* `IdentityUserToken`
* `IdentityUserLogin`
* `IdentityRoleClaim`
* `IdentityUserRole`

<span data-ttu-id="25c5a-162">Bu tür doğrudan kullanmak yerine türleri temel sınıf olarak uygulamanın kendi türleri için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="25c5a-162">Rather than using these types directly, the types can be used as base classes for the app's own types.</span></span> <span data-ttu-id="25c5a-163">`DbContext` Farklı CLR türleri için bir veya daha fazla varlık türleri modelinde kullanılabilir olacak şekilde kimlik tarafından tanımlanan sınıflara genel,.</span><span class="sxs-lookup"><span data-stu-id="25c5a-163">The `DbContext` classes defined by Identity are generic, such that different CLR types can be used for one or more of the entity types in the model.</span></span> <span data-ttu-id="25c5a-164">Bu genel türler de izin `User` değiştirilmesi için birincil anahtar (PK) veri türü.</span><span class="sxs-lookup"><span data-stu-id="25c5a-164">These generic types also allow the `User` primary key (PK) data type to be changed.</span></span>

<span data-ttu-id="25c5a-165">Kimlik desteğiyle rolleri için kullanılırken bir <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext> sınıfı kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="25c5a-165">When using Identity with support for roles, an <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext> class should be used.</span></span> <span data-ttu-id="25c5a-166">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="25c5a-166">For example:</span></span>

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

<span data-ttu-id="25c5a-167">(Yalnızca talep) rol, bu durumda kimlik kullanmak da mümkündür bir <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityUserContext%601> sınıfı kullanılmalıdır:</span><span class="sxs-lookup"><span data-stu-id="25c5a-167">It's also possible to use Identity without roles (only claims), in which case an <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityUserContext%601> class should be used:</span></span>

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

## <a name="customize-the-model"></a><span data-ttu-id="25c5a-168">Model özelleştirme</span><span class="sxs-lookup"><span data-stu-id="25c5a-168">Customize the model</span></span>

<span data-ttu-id="25c5a-169">Model özelleştirme için başlangıç noktası uygun içerik türünden türetilmesi sağlamaktır.</span><span class="sxs-lookup"><span data-stu-id="25c5a-169">The starting point for model customization is to derive from the appropriate context type.</span></span> <span data-ttu-id="25c5a-170">Bkz: [Model genel türler](#model-generic-types) bölümü.</span><span class="sxs-lookup"><span data-stu-id="25c5a-170">See the [Model generic types](#model-generic-types) section.</span></span> <span data-ttu-id="25c5a-171">Bu bağlam türü özel olarak adlandırılır `ApplicationDbContext` ve ASP.NET Core şablonları tarafından oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="25c5a-171">This context type is customarily called `ApplicationDbContext` and is created by the ASP.NET Core templates.</span></span>

<span data-ttu-id="25c5a-172">Bağlam model iki şekilde yapılandırmak için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="25c5a-172">The context is used to configure the model in two ways:</span></span>

* <span data-ttu-id="25c5a-173">Varlık ve genel tür parametreleri için anahtar türleri sağlama.</span><span class="sxs-lookup"><span data-stu-id="25c5a-173">Supplying entity and key types for the generic type parameters.</span></span>
* <span data-ttu-id="25c5a-174">Geçersiz kılma `OnModelCreating` bu tür eşlemesini değiştirmek için.</span><span class="sxs-lookup"><span data-stu-id="25c5a-174">Overriding `OnModelCreating` to modify the mapping of these types.</span></span>

<span data-ttu-id="25c5a-175">Geçersiz kılarken `OnModelCreating`, `base.OnModelCreating` ilk kez çağrılması gerekir; geçersiz kılma yapılandırmasını sonra çağrılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="25c5a-175">When overriding `OnModelCreating`, `base.OnModelCreating` should be called first; the overriding configuration should be called next.</span></span> <span data-ttu-id="25c5a-176">EF Core genel yapılandırma için bir son bir WINS ilkesi vardır.</span><span class="sxs-lookup"><span data-stu-id="25c5a-176">EF Core generally has a last-one-wins policy for configuration.</span></span> <span data-ttu-id="25c5a-177">Örneğin, varsa `ToTable` yöntemi bir varlık türü için bir tablo adıyla önce çağrılır ve daha sonra tekrar farklı bir tablo adı ile ikinci çağrıda tablo adı daha sonra kullanılır.</span><span class="sxs-lookup"><span data-stu-id="25c5a-177">For example, if the `ToTable` method for an entity type is called first with one table name and then again later with a different table name, the table name in the second call is used.</span></span>

### <a name="custom-user-data"></a><span data-ttu-id="25c5a-178">Özel kullanıcı verileri</span><span class="sxs-lookup"><span data-stu-id="25c5a-178">Custom user data</span></span>

<!--
set projNam=WebApp1
dotnet new webapp -o %projNam%
cd %projNam%
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design 
dotnet aspnet-codegenerator identity  -dc ApplicationDbContext --useDefaultUI 
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
 -->

<span data-ttu-id="25c5a-179">[Özel kullanıcı verilerini](xref:security/authentication/add-user-data) devralarak desteklenen `IdentityUser`.</span><span class="sxs-lookup"><span data-stu-id="25c5a-179">[Custom user data](xref:security/authentication/add-user-data) is supported by inheriting from `IdentityUser`.</span></span> <span data-ttu-id="25c5a-180">Bu tür adı uygulamadır `ApplicationUser`:</span><span class="sxs-lookup"><span data-stu-id="25c5a-180">It's customary to name this type `ApplicationUser`:</span></span>

```csharp
public class ApplicationUser : IdentityUser
{
    public string CustomTag { get; set; }
}
```

<span data-ttu-id="25c5a-181">Kullanım `ApplicationUser` bağlamı için genel bir bağımsız değişken türü:</span><span class="sxs-lookup"><span data-stu-id="25c5a-181">Use the `ApplicationUser` type as a generic argument for the context:</span></span>

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

<span data-ttu-id="25c5a-182">Geçersiz kılmak için gerek yoktur `OnModelCreating` içinde `ApplicationDbContext` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="25c5a-182">There's no need to override `OnModelCreating` in the `ApplicationDbContext` class.</span></span> <span data-ttu-id="25c5a-183">EF Core eşler `CustomTag` gereği özelliği.</span><span class="sxs-lookup"><span data-stu-id="25c5a-183">EF Core maps the `CustomTag` property by convention.</span></span> <span data-ttu-id="25c5a-184">Ancak, veritabanını yeni bir güncelleştirilmesi gerekiyor `CustomTag` sütun.</span><span class="sxs-lookup"><span data-stu-id="25c5a-184">However, the database needs to be updated to create a new `CustomTag` column.</span></span> <span data-ttu-id="25c5a-185">Sütun oluşturmak için bir geçiş ekleyin ve ardından veritabanını açıklandığı gibi güncelleştirin [kimlik ve EF Core geçişleri](#identity-and-ef-core-migrations).</span><span class="sxs-lookup"><span data-stu-id="25c5a-185">To create the column, add a migration, and then update the database as described in [Identity and EF Core Migrations](#identity-and-ef-core-migrations).</span></span>

<span data-ttu-id="25c5a-186">Güncelleştirme *Pages/Shared/_LoginPartial.cshtml* değiştirin `IdentityUser` ile `ApplicationUser`:</span><span class="sxs-lookup"><span data-stu-id="25c5a-186">Update *Pages/Shared/_LoginPartial.cshtml* and replace `IdentityUser` with `ApplicationUser`:</span></span>

```cshtml
@using Microsoft.AspNetCore.Identity
@using WebApp1.Areas.Identity.Data
@inject SignInManager<ApplicationUser> SignInManager
@inject UserManager<ApplicationUser> UserManager
```

<span data-ttu-id="25c5a-187">Güncelleştirme *Areas/Identity/IdentityHostingStartup.cs* veya `Startup.ConfigureServices` değiştirin `IdentityUser` ile `ApplicationUser`.</span><span class="sxs-lookup"><span data-stu-id="25c5a-187">Update *Areas/Identity/IdentityHostingStartup.cs*  or `Startup.ConfigureServices` and replace `IdentityUser` with `ApplicationUser`.</span></span>

```csharp
services.AddDefaultIdentity<ApplicationUser>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultUI();
```

<span data-ttu-id="25c5a-188">ASP.NET Core 2.1 veya daha sonra kimlik Razor sınıf kitaplığı sağlanır.</span><span class="sxs-lookup"><span data-stu-id="25c5a-188">In ASP.NET Core 2.1 or later, Identity is provided as a Razor Class Library.</span></span> <span data-ttu-id="25c5a-189">Daha fazla bilgi için bkz. <xref:security/authentication/scaffold-identity>.</span><span class="sxs-lookup"><span data-stu-id="25c5a-189">For more information, see <xref:security/authentication/scaffold-identity>.</span></span> <span data-ttu-id="25c5a-190">Sonuç olarak, önceki kod yapılan bir çağrı gerektirir. <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>.</span><span class="sxs-lookup"><span data-stu-id="25c5a-190">Consequently, the preceding code requires a call to <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>.</span></span> <span data-ttu-id="25c5a-191">Kimlik iskele kurucu kimlik dosyalar projeye eklemek için kullanıldıysa çağrısını kaldırın `AddDefaultUI`.</span><span class="sxs-lookup"><span data-stu-id="25c5a-191">If the Identity scaffolder was used to add Identity files to the project, remove the call to `AddDefaultUI`.</span></span> <span data-ttu-id="25c5a-192">Daha fazla bilgi için bkz.:</span><span class="sxs-lookup"><span data-stu-id="25c5a-192">For more information, see:</span></span>

* [<span data-ttu-id="25c5a-193">İskele Kimliği</span><span class="sxs-lookup"><span data-stu-id="25c5a-193">Scaffold Identity</span></span>](xref:security/authentication/scaffold-identity)
* [<span data-ttu-id="25c5a-194">Ekleme, indirmek ve kimlik için özel kullanıcı verilerini sil</span><span class="sxs-lookup"><span data-stu-id="25c5a-194">Add, download, and delete custom user data to Identity</span></span>](xref:security/authentication/add-user-data)

### <a name="change-the-primary-key-type"></a><span data-ttu-id="25c5a-195">Birincil anahtar türünü değiştirme</span><span class="sxs-lookup"><span data-stu-id="25c5a-195">Change the primary key type</span></span>

<span data-ttu-id="25c5a-196">Veritabanı oluşturulduktan sonra PK sütunun veri türü için çok sayıda veritabanı sistemlerinde sorunlu farklıdır.</span><span class="sxs-lookup"><span data-stu-id="25c5a-196">A change to the PK column's data type after the database has been created is problematic on many database systems.</span></span> <span data-ttu-id="25c5a-197">PK değiştirilmesi genellikle, bırakarak ve tabloyu yeniden oluşturmayı içerir.</span><span class="sxs-lookup"><span data-stu-id="25c5a-197">Changing the PK typically involves dropping and re-creating the table.</span></span> <span data-ttu-id="25c5a-198">Bu nedenle, veritabanı oluşturulurken anahtar türleri ilk geçiş belirtilmelidir.</span><span class="sxs-lookup"><span data-stu-id="25c5a-198">Therefore, key types should be specified in the initial migration when the database is created.</span></span>

<span data-ttu-id="25c5a-199">PK türünü değiştirmek için aşağıdaki adımları izleyin:</span><span class="sxs-lookup"><span data-stu-id="25c5a-199">Follow these steps to change the PK type:</span></span>

1. <span data-ttu-id="25c5a-200">Veritabanı oluşturduysanız çalıştırarak PK değişiklikten önce `Drop-Database` (PMC) veya `dotnet ef database drop` (CLI silmek için .NET Core).</span><span class="sxs-lookup"><span data-stu-id="25c5a-200">If the database was created before the PK change, run `Drop-Database` (PMC) or `dotnet ef database drop` (.NET Core CLI) to delete it.</span></span>
2. <span data-ttu-id="25c5a-201">Veritabanının silinmesi, onayladıktan sonra ilk geçiş işlemine kaldırmak `Remove-Migration` (PMC) veya `dotnet ef migrations remove` (.NET Core CLI).</span><span class="sxs-lookup"><span data-stu-id="25c5a-201">After confirming deletion of the database, remove the initial migration with `Remove-Migration` (PMC) or `dotnet ef migrations remove` (.NET Core CLI).</span></span>
3. <span data-ttu-id="25c5a-202">Güncelleştirme `ApplicationDbContext` öğesinden türetilen sınıfın <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext%603>.</span><span class="sxs-lookup"><span data-stu-id="25c5a-202">Update the `ApplicationDbContext` class to derive from <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext%603>.</span></span> <span data-ttu-id="25c5a-203">Yeni anahtar türü için belirtin `TKey`.</span><span class="sxs-lookup"><span data-stu-id="25c5a-203">Specify the new key type for `TKey`.</span></span> <span data-ttu-id="25c5a-204">Örneğin, kullanılacak bir `Guid` anahtar türü:</span><span class="sxs-lookup"><span data-stu-id="25c5a-204">For example, to use a `Guid` key type:</span></span>

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

    <span data-ttu-id="25c5a-205">Önceki kodda, Genel sınıflar <xref:Microsoft.AspNetCore.Identity.IdentityUser%601> ve <xref:Microsoft.AspNetCore.Identity.IdentityRole%601> yeni anahtar türü kullanmak için belirtilmelidir.</span><span class="sxs-lookup"><span data-stu-id="25c5a-205">In the preceding code, the generic classes <xref:Microsoft.AspNetCore.Identity.IdentityUser%601> and <xref:Microsoft.AspNetCore.Identity.IdentityRole%601> must be specified to use the new key type.</span></span>

    ::: moniker-end

    ::: moniker range="<= aspnetcore-1.1"

    <span data-ttu-id="25c5a-206">Önceki kodda, Genel sınıflar <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityUser%601> ve <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityRole%601> yeni anahtar türü kullanmak için belirtilmelidir.</span><span class="sxs-lookup"><span data-stu-id="25c5a-206">In the preceding code, the generic classes <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityUser%601> and <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityRole%601> must be specified to use the new key type.</span></span>

    ::: moniker-end

    <span data-ttu-id="25c5a-207">`Startup.ConfigureServices` Genel kullanıcı kullanacak şekilde güncelleştirilmesi gerekir:</span><span class="sxs-lookup"><span data-stu-id="25c5a-207">`Startup.ConfigureServices` must be updated to use the generic user:</span></span>

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

4. <span data-ttu-id="25c5a-208">Özel durumunda `ApplicationUser` sınıfı kullanılıyor, devralınacak sınıfını güncelleştirme `IdentityUser`.</span><span class="sxs-lookup"><span data-stu-id="25c5a-208">If a custom `ApplicationUser` class is being used, update the class to inherit from `IdentityUser`.</span></span> <span data-ttu-id="25c5a-209">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="25c5a-209">For example:</span></span>

    ::: moniker range="<= aspnetcore-1.1"

    [!code-csharp[](customize-identity-model/samples/1.1/MvcSampleApp/Models/ApplicationUser.cs?name=snippet_ApplicationUser&highlight=4)]

    ::: moniker-end

    ::: moniker range=">= aspnetcore-2.0"

    [!code-csharp[](customize-identity-model/samples/2.1/RazorPagesSampleApp/Data/ApplicationUser.cs?name=snippet_ApplicationUser&highlight=4)]

    ::: moniker-end

    <span data-ttu-id="25c5a-210">Güncelleştirme `ApplicationDbContext` özel başvurmak için `ApplicationUser` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="25c5a-210">Update `ApplicationDbContext` to reference the custom `ApplicationUser` class:</span></span>

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

    <span data-ttu-id="25c5a-211">Özel bir veritabanı bağlamı sınıfının kimlik hizmeti eklerken kaydetme `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="25c5a-211">Register the custom database context class when adding the Identity service in `Startup.ConfigureServices`:</span></span>

    ::: moniker range=">= aspnetcore-2.1"

    ```csharp
    services.AddDefaultIdentity<ApplicationUser>()
            .AddEntityFrameworkStores<ApplicationDbContext>()
            .AddDefaultUI()
            .AddDefaultTokenProviders();
    ```

    <span data-ttu-id="25c5a-212">Birincil anahtarın veri türü analiz ederek algılanır [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) nesne.</span><span class="sxs-lookup"><span data-stu-id="25c5a-212">The primary key's data type is inferred by analyzing the [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) object.</span></span>

    <span data-ttu-id="25c5a-213">ASP.NET Core 2.1 veya daha sonra kimlik Razor sınıf kitaplığı sağlanır.</span><span class="sxs-lookup"><span data-stu-id="25c5a-213">In ASP.NET Core 2.1 or later, Identity is provided as a Razor Class Library.</span></span> <span data-ttu-id="25c5a-214">Daha fazla bilgi için bkz. <xref:security/authentication/scaffold-identity>.</span><span class="sxs-lookup"><span data-stu-id="25c5a-214">For more information, see <xref:security/authentication/scaffold-identity>.</span></span> <span data-ttu-id="25c5a-215">Sonuç olarak, önceki kod yapılan bir çağrı gerektirir. <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>.</span><span class="sxs-lookup"><span data-stu-id="25c5a-215">Consequently, the preceding code requires a call to <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>.</span></span> <span data-ttu-id="25c5a-216">Kimlik iskele kurucu kimlik dosyalar projeye eklemek için kullanıldıysa çağrısını kaldırın `AddDefaultUI`.</span><span class="sxs-lookup"><span data-stu-id="25c5a-216">If the Identity scaffolder was used to add Identity files to the project, remove the call to `AddDefaultUI`.</span></span>

    ::: moniker-end

    ::: moniker range="= aspnetcore-2.0"

    ```csharp
    services.AddIdentity<ApplicationUser, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext>()
            .AddDefaultTokenProviders();
    ```

    <span data-ttu-id="25c5a-217">Birincil anahtarın veri türü analiz ederek algılanır [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) nesne.</span><span class="sxs-lookup"><span data-stu-id="25c5a-217">The primary key's data type is inferred by analyzing the [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) object.</span></span>

    ::: moniker-end

    ::: moniker range="<= aspnetcore-1.1"

    ```csharp
    services.AddIdentity<ApplicationUser, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext, Guid>()
            .AddDefaultTokenProviders();
    ```

    <span data-ttu-id="25c5a-218"><xref:Microsoft.Extensions.DependencyInjection.IdentityEntityFrameworkBuilderExtensions.AddEntityFrameworkStores*> Yöntemi kabul bir `TKey` birincil anahtarın veri türünü gösteren tür.</span><span class="sxs-lookup"><span data-stu-id="25c5a-218">The <xref:Microsoft.Extensions.DependencyInjection.IdentityEntityFrameworkBuilderExtensions.AddEntityFrameworkStores*> method accepts a `TKey` type indicating the primary key's data type.</span></span>

    ::: moniker-end

5. <span data-ttu-id="25c5a-219">Özel durumunda `ApplicationRole` sınıfı kullanılıyor, devralınacak sınıfını güncelleştirme `IdentityRole<TKey>`.</span><span class="sxs-lookup"><span data-stu-id="25c5a-219">If a custom `ApplicationRole` class is being used, update the class to inherit from `IdentityRole<TKey>`.</span></span> <span data-ttu-id="25c5a-220">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="25c5a-220">For example:</span></span>

    [!code-csharp[](customize-identity-model/samples/2.1/RazorPagesSampleApp/Data/ApplicationRole.cs?name=snippet_ApplicationRole&highlight=4)]

    <span data-ttu-id="25c5a-221">Güncelleştirme `ApplicationDbContext` özel başvurmak için `ApplicationRole` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="25c5a-221">Update `ApplicationDbContext` to reference the custom `ApplicationRole` class.</span></span> <span data-ttu-id="25c5a-222">Örneğin, aşağıdaki sınıf özel başvuran `ApplicationUser` ve özel bir `ApplicationRole`:</span><span class="sxs-lookup"><span data-stu-id="25c5a-222">For example, the following class references a custom `ApplicationUser` and a custom `ApplicationRole`:</span></span>

    ::: moniker range=">= aspnetcore-2.1"

    [!code-csharp[](customize-identity-model/samples/2.1/RazorPagesSampleApp/Data/ApplicationDbContext.cs?name=snippet_ApplicationDbContext&highlight=5-6)]

    <span data-ttu-id="25c5a-223">Özel bir veritabanı bağlamı sınıfının kimlik hizmeti eklerken kaydetme `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="25c5a-223">Register the custom database context class when adding the Identity service in `Startup.ConfigureServices`:</span></span>

    [!code-csharp[](customize-identity-model/samples/2.1/RazorPagesSampleApp/Startup.cs?name=snippet_ConfigureServices&highlight=13-16)]

    <span data-ttu-id="25c5a-224">Birincil anahtarın veri türü analiz ederek algılanır [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) nesne.</span><span class="sxs-lookup"><span data-stu-id="25c5a-224">The primary key's data type is inferred by analyzing the [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) object.</span></span>

    <span data-ttu-id="25c5a-225">ASP.NET Core 2.1 veya daha sonra kimlik Razor sınıf kitaplığı sağlanır.</span><span class="sxs-lookup"><span data-stu-id="25c5a-225">In ASP.NET Core 2.1 or later, Identity is provided as a Razor Class Library.</span></span> <span data-ttu-id="25c5a-226">Daha fazla bilgi için bkz. <xref:security/authentication/scaffold-identity>.</span><span class="sxs-lookup"><span data-stu-id="25c5a-226">For more information, see <xref:security/authentication/scaffold-identity>.</span></span> <span data-ttu-id="25c5a-227">Sonuç olarak, önceki kod yapılan bir çağrı gerektirir. <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>.</span><span class="sxs-lookup"><span data-stu-id="25c5a-227">Consequently, the preceding code requires a call to <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>.</span></span> <span data-ttu-id="25c5a-228">Kimlik iskele kurucu kimlik dosyalar projeye eklemek için kullanıldıysa çağrısını kaldırın `AddDefaultUI`.</span><span class="sxs-lookup"><span data-stu-id="25c5a-228">If the Identity scaffolder was used to add Identity files to the project, remove the call to `AddDefaultUI`.</span></span>

    ::: moniker-end

    ::: moniker range="= aspnetcore-2.0"

    [!code-csharp[](customize-identity-model/samples/2.0/RazorPagesSampleApp/Data/ApplicationDbContext.cs?name=snippet_ApplicationDbContext&highlight=5-6)]

    <span data-ttu-id="25c5a-229">Özel bir veritabanı bağlamı sınıfının kimlik hizmeti eklerken kaydetme `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="25c5a-229">Register the custom database context class when adding the Identity service in `Startup.ConfigureServices`:</span></span>

    [!code-csharp[](customize-identity-model/samples/2.0/RazorPagesSampleApp/Startup.cs?name=snippet_ConfigureServices&highlight=7-9)]

    <span data-ttu-id="25c5a-230">Birincil anahtarın veri türü analiz ederek algılanır [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) nesne.</span><span class="sxs-lookup"><span data-stu-id="25c5a-230">The primary key's data type is inferred by analyzing the [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) object.</span></span>

    ::: moniker-end

    ::: moniker range="<= aspnetcore-1.1"

    [!code-csharp[](customize-identity-model/samples/1.1/MvcSampleApp/Data/ApplicationDbContext.cs?name=snippet_ApplicationDbContext&highlight=5-6)]

    <span data-ttu-id="25c5a-231">Özel bir veritabanı bağlamı sınıfının kimlik hizmeti eklerken kaydetme `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="25c5a-231">Register the custom database context class when adding the Identity service in `Startup.ConfigureServices`:</span></span>

    [!code-csharp[](customize-identity-model/samples/1.1/MvcSampleApp/Startup.cs?name=snippet_ConfigureServices&highlight=7-9)]

    <span data-ttu-id="25c5a-232"><xref:Microsoft.Extensions.DependencyInjection.IdentityEntityFrameworkBuilderExtensions.AddEntityFrameworkStores*> Yöntemi kabul bir `TKey` birincil anahtarın veri türünü gösteren tür.</span><span class="sxs-lookup"><span data-stu-id="25c5a-232">The <xref:Microsoft.Extensions.DependencyInjection.IdentityEntityFrameworkBuilderExtensions.AddEntityFrameworkStores*> method accepts a `TKey` type indicating the primary key's data type.</span></span>

    ::: moniker-end

### <a name="add-navigation-properties"></a><span data-ttu-id="25c5a-233">Gezinti özellikleri ekleyin</span><span class="sxs-lookup"><span data-stu-id="25c5a-233">Add navigation properties</span></span>

<span data-ttu-id="25c5a-234">İlişkiler için model yapılandırmasını değiştirme başka değişiklikler yaparak değerinden daha zor olabilir.</span><span class="sxs-lookup"><span data-stu-id="25c5a-234">Changing the model configuration for relationships can be more difficult than making other changes.</span></span> <span data-ttu-id="25c5a-235">Var olan ilişkileri değiştirmek yerine yeni, ek ilişkiler oluşturmak için dikkatli olunması gerekir.</span><span class="sxs-lookup"><span data-stu-id="25c5a-235">Care must be taken to replace the existing relationships rather than create new, additional relationships.</span></span> <span data-ttu-id="25c5a-236">Özellikle, değiştirilen ilişki aynı yabancı anahtar (FK) özelliği var olan ilişkiyi belirtmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="25c5a-236">In particular, the changed relationship must specify the same foreign key (FK) property as the existing relationship.</span></span> <span data-ttu-id="25c5a-237">Örneğin, arasındaki ilişkiyi `Users` ve `UserClaims` , varsayılan olarak, aşağıda belirtilen ise:</span><span class="sxs-lookup"><span data-stu-id="25c5a-237">For example, the relationship between `Users` and `UserClaims` is, by default, specified as follows:</span></span>

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

<span data-ttu-id="25c5a-238">FK bu ilişki için belirtilen `UserClaim.UserId` özelliği.</span><span class="sxs-lookup"><span data-stu-id="25c5a-238">The FK for this relationship is specified as the `UserClaim.UserId` property.</span></span> <span data-ttu-id="25c5a-239">`HasMany` ve `WithOne` Gezinti özellikleri olmayan bir ilişki oluşturmak için bağımsız değişkenler olmadan verilir.</span><span class="sxs-lookup"><span data-stu-id="25c5a-239">`HasMany` and `WithOne` are called without arguments to create the relationship without navigation properties.</span></span>

<span data-ttu-id="25c5a-240">Bir gezinme özelliği için ekleme `ApplicationUser` sağlayan ilişkili `UserClaims` kullanıcıdan başvurulmak üzere:</span><span class="sxs-lookup"><span data-stu-id="25c5a-240">Add a navigation property to `ApplicationUser` that allows associated `UserClaims` to be referenced from the user:</span></span>

```csharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<IdentityUserClaim<string>> Claims { get; set; }
}
```

<span data-ttu-id="25c5a-241">`TKey` İçin `IdentityUserClaim<TKey>` PK kullanıcı için belirtilen bir tür.</span><span class="sxs-lookup"><span data-stu-id="25c5a-241">The `TKey` for `IdentityUserClaim<TKey>` is the type specified for the PK of users.</span></span> <span data-ttu-id="25c5a-242">Bu durumda, `TKey` olduğu `string` Varsayılanları kullanıldığından.</span><span class="sxs-lookup"><span data-stu-id="25c5a-242">In this case, `TKey` is `string` because the defaults are being used.</span></span> <span data-ttu-id="25c5a-243">Sahip **değil** PK türü `UserClaim` varlık türü.</span><span class="sxs-lookup"><span data-stu-id="25c5a-243">It's **not** the PK type for the `UserClaim` entity type.</span></span>

<span data-ttu-id="25c5a-244">Gezinti özelliği var, bunu yapılandırılmalıdır `OnModelCreating`:</span><span class="sxs-lookup"><span data-stu-id="25c5a-244">Now that the navigation property exists, it must be configured in `OnModelCreating`:</span></span>

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

<span data-ttu-id="25c5a-245">İlişki, daha önce yalnızca yapılan çağrıda belirtilen Gezinti özelliğine sahip olduğu gibi yapılandırıldığını fark `HasMany`.</span><span class="sxs-lookup"><span data-stu-id="25c5a-245">Notice that relationship is configured exactly as it was before, only with a navigation property specified in the call to `HasMany`.</span></span>

<span data-ttu-id="25c5a-246">Gezinme özelliklerini EF modeli, veritabanı değil yalnızca mevcut.</span><span class="sxs-lookup"><span data-stu-id="25c5a-246">The navigation properties only exist in the EF model, not the database.</span></span> <span data-ttu-id="25c5a-247">İlişki için FK değişmediği için güncelleştirilecek veritabanı bu tür bir model değişikliği gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="25c5a-247">Because the FK for the relationship hasn't changed, this kind of model change doesn't require the database to be updated.</span></span> <span data-ttu-id="25c5a-248">Bu değişikliği yaptıktan sonra geçiş ekleyerek denetlenebilir.</span><span class="sxs-lookup"><span data-stu-id="25c5a-248">This can be checked by adding a migration after making the change.</span></span> <span data-ttu-id="25c5a-249">`Up` Ve `Down` yöntemlerdir boş.</span><span class="sxs-lookup"><span data-stu-id="25c5a-249">The `Up` and `Down` methods are empty.</span></span>

### <a name="add-all-user-navigation-properties"></a><span data-ttu-id="25c5a-250">Tüm kullanıcı Gezinti özellikleri ekleyin</span><span class="sxs-lookup"><span data-stu-id="25c5a-250">Add all User navigation properties</span></span>

<span data-ttu-id="25c5a-251">Yukarıdaki bölüme kılavuz kullanarak, aşağıdaki örnekte, kullanıcı tüm ilişkiler tek yönlü bir gezinti özelliklerini yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="25c5a-251">Using the section above as guidance, the following example configures unidirectional navigation properties for all relationships on User:</span></span>

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

### <a name="add-user-and-role-navigation-properties"></a><span data-ttu-id="25c5a-252">Kullanıcı ve rol Gezinti özellikleri ekleyin</span><span class="sxs-lookup"><span data-stu-id="25c5a-252">Add User and Role navigation properties</span></span>

<span data-ttu-id="25c5a-253">Yukarıdaki bölüme kılavuz kullanarak, aşağıdaki örnekte tüm ilişkiler için gezinme özelliklerinin kullanıcı ve rol yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="25c5a-253">Using the section above as guidance, the following example configures navigation properties for all relationships on User and Role:</span></span>

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

<span data-ttu-id="25c5a-254">Notlar:</span><span class="sxs-lookup"><span data-stu-id="25c5a-254">Notes:</span></span>

* <span data-ttu-id="25c5a-255">Bu örnek ayrıca içerir `UserRole` katılma, kullanıcıların çok-çok ilişkisi rollerine gitmek için gerekli olan varlık.</span><span class="sxs-lookup"><span data-stu-id="25c5a-255">This example also includes the `UserRole` join entity, which is needed to navigate the many-to-many relationship from Users to Roles.</span></span>
* <span data-ttu-id="25c5a-256">Gezinti özellikleri, değişimi yansıtmak için tür değiştirmeyi unutmayın `ApplicationXxx` türleri artık kullanıldığı yerine `IdentityXxx` türleri.</span><span class="sxs-lookup"><span data-stu-id="25c5a-256">Remember to change the types of the navigation properties to reflect that `ApplicationXxx` types are now being used instead of `IdentityXxx` types.</span></span>
* <span data-ttu-id="25c5a-257">Kullanmayı unutmayın `ApplicationXxx` genel olarak `ApplicationContext` tanımı.</span><span class="sxs-lookup"><span data-stu-id="25c5a-257">Remember to use the `ApplicationXxx` in the generic `ApplicationContext` definition.</span></span>

### <a name="add-all-navigation-properties"></a><span data-ttu-id="25c5a-258">Tüm gezinti özellikleri ekleyin</span><span class="sxs-lookup"><span data-stu-id="25c5a-258">Add all navigation properties</span></span>

<span data-ttu-id="25c5a-259">Yukarıdaki bölüme kılavuz kullanarak, aşağıdaki örnekte tüm varlık türleri üzerinde tüm ilişkiler için Gezinti özellikleri yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="25c5a-259">Using the section above as guidance, the following example configures navigation properties for all relationships on all entity types:</span></span>

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

### <a name="use-composite-keys"></a><span data-ttu-id="25c5a-260">Bileşik anahtarlar kullanın</span><span class="sxs-lookup"><span data-stu-id="25c5a-260">Use composite keys</span></span>

<span data-ttu-id="25c5a-261">Kimlik modelinde kullanılan anahtar türünü değiştirerek önceki bölümlerde gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="25c5a-261">The preceding sections demonstrated changing the type of key used in the Identity model.</span></span> <span data-ttu-id="25c5a-262">Bileşik anahtarlar kullanılacak kimlik anahtar modelini değiştirme, önerilen desteklenen veya değil.</span><span class="sxs-lookup"><span data-stu-id="25c5a-262">Changing the Identity key model to use composite keys isn't supported or recommended.</span></span> <span data-ttu-id="25c5a-263">Bir bileşik anahtarı ile kimlik kullanarak, Identity manager kod modeli ile nasıl etkileştiğini değiştirilmesini kapsar.</span><span class="sxs-lookup"><span data-stu-id="25c5a-263">Using a composite key with Identity involves changing how the Identity manager code interacts with the model.</span></span> <span data-ttu-id="25c5a-264">Bu belgenin kapsamı dışındadır özelleştirmedir.</span><span class="sxs-lookup"><span data-stu-id="25c5a-264">This customization is beyond the scope of this document.</span></span>

### <a name="change-tablecolumn-names-and-facets"></a><span data-ttu-id="25c5a-265">Tablo/sütun adlarını ve modeller</span><span class="sxs-lookup"><span data-stu-id="25c5a-265">Change table/column names and facets</span></span>

<span data-ttu-id="25c5a-266">Tablo ve sütun adlarını değiştirmek için çağrı `base.OnModelCreating`.</span><span class="sxs-lookup"><span data-stu-id="25c5a-266">To change the names of tables and columns, call `base.OnModelCreating`.</span></span> <span data-ttu-id="25c5a-267">Ardından, tüm varsayılanları geçersiz kılmak için yapılandırma ekleyin.</span><span class="sxs-lookup"><span data-stu-id="25c5a-267">Then, add configuration to override any of the defaults.</span></span> <span data-ttu-id="25c5a-268">Örneğin, tüm kimlik tabloları adını değiştirmek için şunu yazın:</span><span class="sxs-lookup"><span data-stu-id="25c5a-268">For example, to change the name of all the Identity tables:</span></span>

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

<span data-ttu-id="25c5a-269">Bu örnekler, varsayılan kimlik türlerini kullanın.</span><span class="sxs-lookup"><span data-stu-id="25c5a-269">These examples use the default Identity types.</span></span> <span data-ttu-id="25c5a-270">Bir uygulama türü gibi kullanıyorsanız `ApplicationUser`, varsayılan türü yerine bu tür yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="25c5a-270">If using an app type such as `ApplicationUser`, configure that type instead of the default type.</span></span>

<span data-ttu-id="25c5a-271">Aşağıdaki örnek, bazı sütun adlarını değiştirir:</span><span class="sxs-lookup"><span data-stu-id="25c5a-271">The following example changes some column names:</span></span>

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

<span data-ttu-id="25c5a-272">Veritabanı sütunlarının bazı türleri belirli ile yapılandırılabilir *modelleri* (örneğin, maksimum `string` izin verilen uzunluk).</span><span class="sxs-lookup"><span data-stu-id="25c5a-272">Some types of database columns can be configured with certain *facets* (for example, the maximum `string` length allowed).</span></span> <span data-ttu-id="25c5a-273">Aşağıdaki örnekte en çok uzunlukları sütun için çeşitli ayarlar `string` modelinde özellikleri:</span><span class="sxs-lookup"><span data-stu-id="25c5a-273">The following example sets column maximum lengths for several `string` properties in the model:</span></span>

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

### <a name="map-to-a-different-schema"></a><span data-ttu-id="25c5a-274">Farklı bir şemaya eşleme</span><span class="sxs-lookup"><span data-stu-id="25c5a-274">Map to a different schema</span></span>

<span data-ttu-id="25c5a-275">Şemalar, veritabanı sağlayıcıları arasında farklı şekilde davranabilir.</span><span class="sxs-lookup"><span data-stu-id="25c5a-275">Schemas can behave differently across database providers.</span></span> <span data-ttu-id="25c5a-276">SQL Server için varsayılan olarak tüm tabloları oluşturmaktır *dbo* şema.</span><span class="sxs-lookup"><span data-stu-id="25c5a-276">For SQL Server, the default is to create all tables in the *dbo* schema.</span></span> <span data-ttu-id="25c5a-277">Tablolar farklı bir şema oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="25c5a-277">The tables can be created in a different schema.</span></span> <span data-ttu-id="25c5a-278">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="25c5a-278">For example:</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    base.OnModelCreating(modelBuilder);

    modelBuilder.HasDefaultSchema("notdbo");
}
```

::: moniker range=">= aspnetcore-2.1"

### <a name="lazy-loading"></a><span data-ttu-id="25c5a-279">Yavaş yükleniyor</span><span class="sxs-lookup"><span data-stu-id="25c5a-279">Lazy loading</span></span>

<span data-ttu-id="25c5a-280">Bu bölümde, yavaş yükleniyor proxy'si kimlik modeli için destek eklendi.</span><span class="sxs-lookup"><span data-stu-id="25c5a-280">In this section, support for lazy-loading proxies in the Identity model is added.</span></span> <span data-ttu-id="25c5a-281">Gezinti özellikleri, yüklenen ilk sağlamaya gerek kalmadan kullanılacak olanak tanıdığından Lazy yüklenirken kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="25c5a-281">Lazy-loading is useful since it allows navigation properties to be used without first ensuring they're loaded.</span></span>

<span data-ttu-id="25c5a-282">Varlık türleri yapılabilir uygun çeşitli yollarla yavaş yükleniyor açıklandığı [EF Core belgeleri](/ef/core/querying/related-data#lazy-loading).</span><span class="sxs-lookup"><span data-stu-id="25c5a-282">Entity types can be made suitable for lazy-loading in several ways, as described in the [EF Core documentation](/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="25c5a-283">Kolaylık olması için Gecikmeli yükleme proxy'leri gerektiren kullanın:</span><span class="sxs-lookup"><span data-stu-id="25c5a-283">For simplicity, use lazy-loading proxies, which requires:</span></span>

* <span data-ttu-id="25c5a-284">Yüklenmesini [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) paket.</span><span class="sxs-lookup"><span data-stu-id="25c5a-284">Installation of the [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) package.</span></span>
* <span data-ttu-id="25c5a-285">Bir çağrı <xref:Microsoft.EntityFrameworkCore.ProxiesExtensions.UseLazyLoadingProxies*> içinde [AddDbContext\<TContext >](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext).</span><span class="sxs-lookup"><span data-stu-id="25c5a-285">A call to <xref:Microsoft.EntityFrameworkCore.ProxiesExtensions.UseLazyLoadingProxies*> inside [AddDbContext\<TContext>](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext).</span></span>
* <span data-ttu-id="25c5a-286">Genel varlık türleri ile `public virtual` Gezinti özellikleri.</span><span class="sxs-lookup"><span data-stu-id="25c5a-286">Public entity types with `public virtual` navigation properties.</span></span>

<span data-ttu-id="25c5a-287">Aşağıdaki örnek, arama gösterir `UseLazyLoadingProxies` içinde `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="25c5a-287">The following example demonstrates calling `UseLazyLoadingProxies` in `Startup.ConfigureServices`:</span></span>

```csharp
services
    .AddDbContext<ApplicationDbContext>(
        b => b.UseSqlServer(connectionString)
              .UseLazyLoadingProxies())
    .AddDefaultIdentity<ApplicationUser>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

<span data-ttu-id="25c5a-288">Önceki örneklerde varlık türlerine Gezinti özellikleri ekleme Kılavuzu'na bakın.</span><span class="sxs-lookup"><span data-stu-id="25c5a-288">Refer to the preceding examples for guidance on adding navigation properties to the entity types.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="25c5a-289">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="25c5a-289">Additional resources</span></span>

* <xref:security/authentication/scaffold-identity>

::: moniker-end
