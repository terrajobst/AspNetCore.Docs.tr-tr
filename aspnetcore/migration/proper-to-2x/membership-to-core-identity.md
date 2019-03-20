---
title: ASP.NET üyelik kimlik doğrulamasını ASP.NET Core 2.0 Identity'ye geçirme
author: isaac2004
description: ASP.NET Core 2.0 kimliği için üyelik kimlik doğrulaması kullanarak varolan ASP.NET uygulamalarını geçirmeyi öğrenin.
ms.author: scaddie
ms.custom: mvc
ms.date: 01/10/2019
uid: migration/proper-to-2x/membership-to-core-identity
ms.openlocfilehash: 3b708da13ff9f2887eee87ea17844312a4fe1b8d
ms.sourcegitcommit: 57792e5f594db1574742588017c708350958bdf0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58264731"
---
# <a name="migrate-from-aspnet-membership-authentication-to-aspnet-core-20-identity"></a>ASP.NET üyelik kimlik doğrulamasını ASP.NET Core 2.0 Identity'ye geçirme

Tarafından [Isaac Levin](https://isaaclevin.com)

Bu makalede, ASP.NET Core 2.0 kimliği için üyelik kimlik doğrulaması kullanarak ASP.NET uygulamaları için veritabanı şemasını geçirme gösterilmektedir.

> [!NOTE]
> Bu belge, veritabanı şemasını ASP.NET üyelik tabanlı uygulamalar için ASP.NET Core kimliği için kullanılan veritabanı şemasını geçirmek için gerekli olan adımları sağlar. ASP.NET üyelik tabanlı kimlik doğrulamasını ASP.NET Identity'ye geçirme hakkında daha fazla bilgi için bkz. [mevcut bir uygulamayı SQL üyeliğinden ASP.NET Identity'ye geçirme](/aspnet/identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity). ASP.NET Core kimliği hakkında daha fazla bilgi için bkz: [ASP.NET Core kimliği giriş](xref:security/authentication/identity).

## <a name="review-of-membership-schema"></a>Üyelik şeması gözden geçirme

ASP.NET 2.0 önce geliştiricilerin uygulamalarını tüm kimlik doğrulama ve yetkilendirme işlemi oluşturmaya görevli. ASP.NET 2.0 ile ASP.NET uygulamaları içinde güvenlik işlemek için ortak bir çözüm sağlayarak üyelik sunulmuştur. Geliştiriciler ile bir SQL Server veritabanına bir şema bootstrap için artık [aspnet_regsql.exe](https://msdn.microsoft.com/library/ms229862.aspx) komutu. Bu komutu çalıştırdıktan sonra aşağıdaki tablolarda veritabanında oluşturulmuş.

  ![Üyelik tablolarını](identity/_static/membership-tables.png)

ASP.NET Core 2.0 kimliği için mevcut uygulamaları geçirmek için bu tablolardaki verilerin yeni kimlik şema tarafından kullanılan tablolar için geçirilmesi gerekiyor.

## <a name="aspnet-core-identity-20-schema"></a>ASP.NET Core kimlik 2.0 şeması

ASP.NET Core 2.0 izleyen [kimlik](/aspnet/identity/index) ASP.NET 4.5 içinde tanıtılan ilkesi. Uygulama çerçeveleri arasında ilkesini paylaşılan olsa bile ASP.NET Core sürümleri arasında farklı (bkz [geçirme kimlik doğrulaması ve kimlik için ASP.NET Core 2.0](xref:migration/1x-to-2x/index)).

ASP.NET Core 2.0 kimliği için şemasını görüntülemek için en hızlı yolu, yeni bir ASP.NET Core 2.0 uygulaması oluşturmaktır. Visual Studio 2017'de şu adımları izleyin:

1. **Dosya** > **Yeni** > **Proje**’yi seçin.
1. Yeni bir **ASP.NET Core Web uygulaması** adlı proje *CoreIdentitySample*.
1. Seçin **ASP.NET Core 2.0** seçin ve açılan **Web uygulaması**. Bu şablon üreten bir [Razor sayfaları](xref:razor-pages/index) uygulama. Tıklatmadan önce **Tamam**, tıklayın **kimlik doğrulamayı Değiştir**.
1. Seçin **bireysel kullanıcı hesapları** kimlik şablonları. Son olarak, tıklayın **Tamam**, ardından **Tamam**. Visual Studio, ASP.NET Core kimliği şablonunu kullanarak bir proje oluşturur.
1. Seçin **Araçları** > **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu** açmak için **PaketYöneticisiKonsolu** (PMC) penceresi.
1. Proje kök dizininde PMC gidin ve çalıştırma [Entity Framework (EF) çekirdek](/ef/core) `Update-Database` komutu.

    ASP.NET Core 2.0 kimlik EF Core kimlik doğrulaması veri depolama veritabanı ile etkileşim kurmak için kullanır. İçin yeni oluşturulan bir uygulamanın çalışması için sırayla var. Bu verileri depolamak için bir veritabanı olması gerekir. Yeni bir uygulama oluşturduktan sonra bir veritabanı ortam içinde şema incelemek için en hızlı yolu kullanarak veritabanını oluşturmak için olan [EF Core geçişleri](/ef/core/managing-schemas/migrations/). Bu işlem, bu şema taklit eden bir veritabanı, yerel olarak veya başka bir yerde, oluşturur. Daha fazla bilgi için yukarıdaki belgelerini inceleyin.

    EF Core komutları belirtilen veritabanı için bağlantı dizesini kullanın *appsettings.json*. Bir veritabanı bağlantı dizesi hedefleyen *localhost* adlı *asp net core kimliği*. Bu ayarda EF Core kullanacak şekilde yapılandırılmış `DefaultConnection` bağlantı dizesi.

    ```json
    {
      "ConnectionStrings": {
        "DefaultConnection": "Server=localhost;Database=aspnet-core-identity;Trusted_Connection=True;MultipleActiveResultSets=true"
      }
    }
    ```

1. Seçin **görünümü** > **SQL Server Nesne Gezgini**. Belirtilen veritabanı adı için karşılık gelen düğümünü `ConnectionStrings:DefaultConnection` özelliği *appsettings.json*.

    `Update-Database` Komutu oluşturulan şemasıyla belirtilen veritabanı ve uygulama başlatma için gereken tüm verileri. Aşağıdaki görüntüde ile önceki adımlarda oluşturulan tablo yapısı gösterilmektedir.

    ![Kimlik tabloları](identity/_static/identity-tables.png)

## <a name="migrate-the-schema"></a>Geçiş şeması

Tablo yapıları ve alanları üyelik hem de ASP.NET Core kimliği için küçük farklılıklar vardır. Desen, ASP.NET ve ASP.NET Core uygulamaları ile kimlik doğrulama/yetkilendirme için önemli ölçüde değişti. Kimlikle hala kullanılan anahtar nesneler *kullanıcılar* ve *rolleri*. Eşleme tablolar için işte *kullanıcılar*, *rolleri*, ve *UserRoles*.

### <a name="users"></a>Kullanıcılar

|*Identity<br>(dbo.AspNetUsers)*        ||*Membership<br>(dbo.aspnet_Users / dbo.aspnet_Membership)*||
|----------------------------------------|-----------------------------------------------------------|
|**Alan adı**                 |**Tür**|**Alan adı**                                    |**Tür**|
|`Id`                           |dize  |`aspnet_Users.UserId`                             |dize  |
|`UserName`                     |dize  |`aspnet_Users.UserName`                           |dize  |
|`Email`                        |dize  |`aspnet_Membership.Email`                         |dize  |
|`NormalizedUserName`           |dize  |`aspnet_Users.LoweredUserName`                    |dize  |
|`NormalizedEmail`              |dize  |`aspnet_Membership.LoweredEmail`                  |dize  |
|`PhoneNumber`                  |dize  |`aspnet_Users.MobileAlias`                        |dize  |
|`LockoutEnabled`               |bit     |`aspnet_Membership.IsLockedOut`                   |bit     |

> [!NOTE]
> Tüm alan eşlemelerini üyeliğinin bire bir ilişkiler ASP.NET Core kimliği için benzer. Yukarıdaki tabloda, varsayılan üyelik kullanıcısı şemasını alır ve ASP.NET Core kimliği şemaya eşler. Üyelik için kullanılan herhangi bir özel alanları el ile eşlenmesi gerekir. Bu eşleme nebyla nalezena mapa Pro parolaları, parola ölçütlerini hem parola salts ikisi arasında geçirme gibi bulunur. **Parola null olarak bırakın ve kullanıcıların parolalarını sıfırlamalarına olanak istemeniz önerilir.** ASP.NET Core kimliği içinde `LockoutEnd` kullanıcıya kilitlenmişse bazı tarih gelecekte ayarlanması gerekir. Bu, geçiş öncesinde bir betik içinde gösterilir.

### <a name="roles"></a>Rolleri

|*Kimlik<br>(dbo. AspNetRoles)*        ||*Üyelik<br>(dbo.aspnet_Roles)*||
|----------------------------------------|-----------------------------------|
|**Alan adı**                 |**Tür**|**Alan adı**   |**Tür**         |
|`Id`                           |dize  |`RoleId`         | dize          |
|`Name`                         |dize  |`RoleName`       | dize          |
|`NormalizedName`               |dize  |`LoweredRoleName`| dize          |

### <a name="user-roles"></a>Kullanıcı Rolleri

|*Identity<br>(dbo.AspNetUserRoles)*||*Membership<br>(dbo.aspnet_UsersInRoles)*||
|------------------------------------|------------------------------------------|
|**Alan adı**           |**Tür**  |**Alan adı**|**Tür**                   |
|`RoleId`                 |dize    |`RoleId`      |dize                     |
|`UserId`                 |dize    |`UserId`      |dize                     |

Önceki eşleme tabloları için geçiş betiği oluştururken başvuru *kullanıcılar* ve *rolleri*. Aşağıdaki örnek, iki veritabanı bir veritabanı sunucusuna sahip olduğunuz varsayılır. Bir veritabanı mevcut ASP.NET üyelik şeması ve verileri içerir. Diğer *CoreIdentitySample* daha önce açıklanan adımları kullanarak veritabanı oluşturuldu. Daha fazla ayrıntı için eklenen satır içi açıklamalardır.

```sql
-- THIS SCRIPT NEEDS TO RUN FROM THE CONTEXT OF THE MEMBERSHIP DB
BEGIN TRANSACTION MigrateUsersAndRoles
USE aspnetdb

-- INSERT USERS
INSERT INTO CoreIdentitySample.dbo.AspNetUsers
            (Id,
             UserName,
             NormalizedUserName,
             PasswordHash,
             SecurityStamp,
             EmailConfirmed,
             PhoneNumber,
             PhoneNumberConfirmed,
             TwoFactorEnabled,
             LockoutEnd,
             LockoutEnabled,
             AccessFailedCount,
             Email,
             NormalizedEmail)
SELECT aspnet_Users.UserId,
       aspnet_Users.UserName,
       -- The NormalizedUserName value is upper case in ASP.NET Core Identity
       UPPER(aspnet_Users.UserName),
       -- Creates an empty password since passwords don't map between the 2 schemas
       '',
       /*
        The SecurityStamp token is used to verify the state of an account and
        is subject to change at any time. It should be initialized as a new ID.
       */
       NewID(),
       /*
        EmailConfirmed is set when a new user is created and confirmed via email.
        Users must have this set during migration to reset passwords.
       */
       1,
       aspnet_Users.MobileAlias,
       CASE
         WHEN aspnet_Users.MobileAlias IS NULL THEN 0
         ELSE 1
       END,
       -- 2FA likely wasn't setup in Membership for users, so setting as false.
       0,
       CASE
         -- Setting lockout date to time in the future (1,000 years)
         WHEN aspnet_Membership.IsLockedOut = 1 THEN Dateadd(year, 1000,
                                                     Sysutcdatetime())
         ELSE NULL
       END,
       aspnet_Membership.IsLockedOut,
       /*
        AccessFailedAccount is used to track failed logins. This is stored in
        Membership in multiple columns. Setting to 0 arbitrarily.
       */
       0,
       aspnet_Membership.Email,
       -- The NormalizedEmail value is upper case in ASP.NET Core Identity
       UPPER(aspnet_Membership.Email)
FROM   aspnet_Users
       LEFT OUTER JOIN aspnet_Membership
                    ON aspnet_Membership.ApplicationId =
                       aspnet_Users.ApplicationId
                       AND aspnet_Users.UserId = aspnet_Membership.UserId
       LEFT OUTER JOIN CoreIdentitySample.dbo.AspNetUsers
                    ON aspnet_Membership.UserId = AspNetUsers.Id
WHERE  AspNetUsers.Id IS NULL

-- INSERT ROLES
INSERT INTO CoreIdentitySample.dbo.AspNetRoles(Id, Name)
SELECT RoleId, RoleName
FROM aspnet_Roles;

-- INSERT USER ROLES
INSERT INTO CoreIdentitySample.dbo.AspNetUserRoles(UserId, RoleId)
SELECT UserId, RoleId
FROM aspnet_UsersInRoles;

IF @@ERROR <> 0
  BEGIN
    ROLLBACK TRANSACTION MigrateUsersAndRoles
    RETURN
  END

COMMIT TRANSACTION MigrateUsersAndRoles
```

Önceki komut tamamlandıktan sonra daha önce oluşturduğunuz ASP.NET Core kimliği uygulama üyelik kullanıcılarının ile doldurulur. Kullanıcıların oturum açma önce parolalarını değiştirmesi gerekir.

> [!NOTE]
> Üyelik Sistemi kullanıcılara e-posta adresi ile eşleşmedi kullanıcı adları varsa bunu uygun hale getirmek için daha önce oluşturulan uygulama için değişiklik gerekmez. Varsayılan şablonu bekliyor `UserName` ve `Email` ile aynı. Bunlar farklı durumlar için oturum açma işlemi kullanmak için değiştirilmesi gerektiğinde `UserName` yerine `Email`.

İçinde `PageModel` konumunda bulunan oturum açma sayfasının *Pages\Account\Login.cshtml.cs*, kaldırma `[EmailAddress]` özniteliğini *e-posta* özelliği. Yeniden adlandırın *UserName*. Bu değişikliği gerektiren her yerde `EmailAddress` , içinde açıklanan *görünümü* ve *PageModel*. Sonuç aşağıdaki gibi görünür:

 ![Sabit oturum açma](identity/_static/fixed-login.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, kullanıcıların SQL üyeliğinden ASP.NET Core 2.0 kimliği için bağlantı noktası öğrendiniz. ASP.NET Core kimliği hakkında daha fazla bilgi için bkz. [kimliğe giriş](xref:security/authentication/identity).
