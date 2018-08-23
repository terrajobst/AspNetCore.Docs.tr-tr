---
title: ASP.NET üyelik kimlik doğrulamasını ASP.NET Core 2.0 Identity'ye geçirme
author: isaac2004
description: ASP.NET Core 2.0 kimliği için üyelik kimlik doğrulaması kullanarak varolan ASP.NET uygulamalarını geçirmeyi öğrenin.
ms.author: scaddie
ms.custom: mvc
ms.date: 04/24/2018
uid: migration/proper-to-2x/membership-to-core-identity
ms.openlocfilehash: 82158ec500151a0bb61fb1da55a53684367d9a4e
ms.sourcegitcommit: 2e054638b69f2b14f6d67d9fa3664999172ee1b2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/14/2018
ms.locfileid: "41753137"
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

* Seçin **dosya** > **yeni** > **proje**.
* Yeni bir **ASP.NET Core Web uygulaması** ve projeyi adlandırın *CoreIdentitySample*.
* Seçin **ASP.NET Core 2.0** seçin ve açılan **Web uygulaması**. Bu şablon üreten bir [Razor sayfaları](xref:razor-pages/index) uygulama. Tıklatmadan önce **Tamam**, tıklayın **kimlik doğrulamayı Değiştir**.
* Seçin **bireysel kullanıcı hesapları** kimlik şablonları. Son olarak, tıklayın **Tamam**, ardından **Tamam**. Visual Studio, ASP.NET Core kimliği şablonunu kullanarak bir proje oluşturur.

ASP.NET Core 2.0 kullanan kimlik [Entity Framework Core](/ef/core) kimlik doğrulama verileri depolamak veritabanıyla etkileşime geçmek için. İçin yeni oluşturulan bir uygulamanın çalışması için sırayla var. Bu verileri depolamak için bir veritabanı olması gerekir. Yeni bir uygulama oluşturduktan sonra en hızlı yolu, bir veritabanı ortam içinde şema incelemek için Entity Framework geçişleri kullanarak veritabanı oluşturmaktır. Bu işlem, bu şema taklit eden bir veritabanı, yerel olarak veya başka bir yerde, oluşturur. Daha fazla bilgi için yukarıdaki belgelerini inceleyin.

Bir veritabanı ile ASP.NET Core kimliği şema oluşturmak için çalıştırma `Update-Database` Visual Studio'nun komutunu **Paket Yöneticisi Konsolu** (PMC) penceresi&mdash;konumunda bulunan **Araçları**  >  **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu**. PMC, Entity Framework komutlarını çalıştırmayı destekliyor.

Entity Framework komutları belirtilen veritabanı için bağlantı dizesini kullanın *appsettings.json*. Bir veritabanı bağlantı dizesi hedefleyen *localhost* adlı *asp net core kimliği*. Bu ayarda, Entity Framework kullanmak üzere yapılandırılmış `DefaultConnection` bağlantı dizesi.

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=localhost;Database=aspnet-core-identity;Trusted_Connection=True;MultipleActiveResultSets=true"
  }
}
```

Bu komut, şema ile belirtilen veritabanı ve uygulama başlatma için gereken tüm verileri oluşturur. Aşağıdaki görüntüde ile önceki adımlarda oluşturulan tablo yapısı gösterilmektedir.

   ![Kimlik tabloları](identity/_static/identity-tables.png)

## <a name="migrate-the-schema"></a>Geçiş şeması

Tablo yapıları ve alanları üyelik hem de ASP.NET Core kimliği için küçük farklılıklar vardır. Desen, ASP.NET ve ASP.NET Core uygulamaları ile kimlik doğrulama/yetkilendirme için önemli ölçüde değişti. Kimlikle hala kullanılan anahtar nesneler *kullanıcılar* ve *rolleri*. Eşleme tablolar için işte *kullanıcılar*, *rolleri*, ve *UserRoles*.

### <a name="users"></a>Kullanıcılar

| *Identity(AspNetUsers)* |   | *Membership(aspnet_Users/aspnet_Membership)* ||
| --- | --- | --- | --- | --- | --- |
| **Alan adı** | **Türü**  |   **Alan adı** | **Türü**  |
|`Id` | dize | `aspnet_Users.UserId` | dize
|`UserName` | dize | `aspnet_Users.UserName` | dize
|`Email` | dize | `aspnet_Membership.Email` | dize
|`NormalizedUserName` | dize | `aspnet_Users.LoweredUserName` | dize
|`NormalizedEmail` | dize | `aspnet_Membership.LoweredEmail` | dize
|`PhoneNumber` | dize | `aspnet_Users.MobileAlias` | dize
|`LockoutEnabled` | bit | `aspnet_Membership.IsLockedOut` | bit

> [!NOTE]
> Tüm alan eşlemelerini üyeliğinin bire bir ilişkiler ASP.NET Core kimliği için benzer. Yukarıdaki tabloda, varsayılan üyelik kullanıcısı şemasını alır ve ASP.NET Core kimliği şemaya eşler. Üyelik için kullanılan herhangi bir özel alanları el ile eşlenmesi gerekir. Bu eşleme nebyla nalezena mapa Pro parolaları, parola ölçütlerini hem parola salts ikisi arasında geçirme gibi bulunur. **Parola null olarak bırakın ve kullanıcıların parolalarını sıfırlamalarına olanak istemeniz önerilir.** ASP.NET Core kimliği içinde `LockoutEnd` kullanıcıya kilitlenmişse bazı tarih gelecekte ayarlanması gerekir. Bu, geçiş öncesinde bir betik içinde gösterilir.

### <a name="roles"></a>Rolleri

| *Identity(AspNetRoles)* |   | *Membership(aspnet_Roles)* ||
| --- | --- | --- | --- | --- | --- |
| **Alan adı** | **Türü**  |   **Alan adı** | **Türü**  |
|`Id` | dize | `RoleId` | dize
|`Name` | dize | `RoleName` | dize
|`NormalizedName` | dize | `LoweredRoleName` | dize

### <a name="user-roles"></a>Kullanıcı Rolleri

| *Identity(AspNetUserRoles)* |   | *Membership(aspnet_UsersInRoles)* ||
| --- | --- | --- | --- | --- | --- |
| **Alan adı** | **Türü**  |   **Alan adı** | **Türü**  |
|`RoleId` | dize | `RoleId` | dize
|`UserId` | dize | `UserId` | dize

Önceki eşleme tabloları için geçiş betiği oluştururken başvuru *kullanıcılar* ve *rolleri*. Aşağıdaki örnek, iki veritabanı bir veritabanı sunucusuna sahip olduğunuz varsayılır. Bir veritabanı mevcut ASP.NET üyelik şeması ve verileri içerir. Daha önce açıklanan adımları kullanarak diğer veritabanıyla oluşturuldu. Daha fazla ayrıntı için eklenen satır içi açıklamalardır.

```sql
-- THIS SCRIPT NEEDS TO RUN FROM THE CONTEXT OF THE MEMBERSHIP DB
BEGIN TRANSACTION MigrateUsersAndRoles
use aspnetdb

-- INSERT USERS
INSERT INTO coreidentity.dbo.aspnetusers
            (id,
             username,
             normalizedusername,
             passwordhash,
             securitystamp,
             emailconfirmed,
             phonenumber,
             phonenumberconfirmed,
             twofactorenabled,
             lockoutend,
             lockoutenabled,
             accessfailedcount,
             email,
             normalizedemail)
SELECT aspnet_users.userid,
       aspnet_users.username,
       aspnet_users.loweredusername,
       --Creates an empty password since passwords don't map between the two schemas
       '',
       --Security Stamp is a token used to verify the state of an account and is subject to change at any time. It should be initialized as a new ID.
       NewID(),
       --EmailConfirmed is set when a new user is created and confirmed via email. Users must have this set during migration to ensure they're able to reset passwords.
       1,
       aspnet_users.mobilealias,
       CASE
         WHEN aspnet_Users.MobileAlias is null THEN 0
         ELSE 1
       END,
       --2-factor Auth likely wasn't setup in Membership for users, so setting as false.
       0,
       CASE
         --Setting lockout date to time in the future (1000 years)
         WHEN aspnet_membership.islockedout = 1 THEN Dateadd(year, 1000,
                                                     Sysutcdatetime())
         ELSE NULL
       END,
       aspnet_membership.islockedout,
       --AccessFailedAccount is used to track failed logins. This is stored in membership in multiple columns. Setting to 0 arbitrarily.
       0,
       aspnet_membership.email,
       aspnet_membership.loweredemail
FROM   aspnet_users
       LEFT OUTER JOIN aspnet_membership
                    ON aspnet_membership.applicationid =
                       aspnet_users.applicationid
                       AND aspnet_users.userid = aspnet_membership.userid
       LEFT OUTER JOIN coreidentity.dbo.aspnetusers
                    ON aspnet_membership.userid = aspnetusers.id
WHERE  aspnetusers.id IS NULL

-- INSERT ROLES
INSERT INTO coreIdentity.dbo.aspnetroles(id,name)
SELECT roleId,rolename
FROM aspnet_roles;

-- INSERT USER ROLES
INSERT INTO coreidentity.dbo.aspnetuserroles(userid,roleid)
SELECT userid,roleid
FROM aspnet_usersinroles;

IF @@ERROR <> 0
  BEGIN
    ROLLBACK TRANSACTION MigrateUsersAndRoles
    RETURN
  END

COMMIT TRANSACTION MigrateUsersAndRoles
```

Bu betik tamamlandıktan sonra daha önce oluşturduğunuz ASP.NET Core kimliği uygulama üyelik kullanıcılarının ile doldurulur. Kullanıcıların oturum açma önce parolalarını değiştirmesi gerekir.

> [!NOTE]
> Üyelik Sistemi kullanıcılara e-posta adresi ile eşleşmedi kullanıcı adları varsa bunu uygun hale getirmek için daha önce oluşturulan uygulama için değişiklik gerekmez. Varsayılan şablonu bekliyor `UserName` ve `Email` ile aynı. Bunlar farklı durumlar için oturum açma işlemi kullanmak için değiştirilmesi gerektiğinde `UserName` yerine `Email`.

İçinde `PageModel` konumunda bulunan oturum açma sayfasının *Pages\Account\Login.cshtml.cs*, kaldırma `[EmailAddress]` özniteliğini *e-posta* özelliği. Yeniden adlandırın *UserName*. Bu değişikliği gerektiren her yerde `EmailAddress` , içinde açıklanan *görünümü* ve *PageModel*. Sonuç aşağıdaki gibi görünür:

 ![Sabit oturum açma](identity/_static/fixed-login.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, kullanıcıların SQL üyeliğinden ASP.NET Core 2.0 kimliği için bağlantı noktası öğrendiniz. ASP.NET Core kimliği hakkında daha fazla bilgi için bkz. [kimliğe giriş](xref:security/authentication/identity).
