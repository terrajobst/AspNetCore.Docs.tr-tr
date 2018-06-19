---
title: ASP.NET Core 2.0 kimliği için ASP.NET üyelik kimlik doğrulamasını geçirme
author: isaac2004
description: ASP.NET Core 2.0 kimlik üyelik kimlik doğrulaması kullanarak mevcut ASP.NET uygulamaları geçirmek öğrenin.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 04/24/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/proper-to-2x/membership-to-core-identity
ms.openlocfilehash: f0d1099bfda01d036831350e0888ae3830ad3d58
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2018
ms.locfileid: "33851599"
---
# <a name="migrate-from-aspnet-membership-authentication-to-aspnet-core-20-identity"></a>ASP.NET Core 2.0 kimliği için ASP.NET üyelik kimlik doğrulamasını geçirme

Tarafından [Isaac Levin](https://isaaclevin.com)

Bu makalede, ASP.NET Core 2.0 kimlik üyelik kimlik doğrulaması kullanarak ASP.NET uygulamaları için veritabanı şeması geçirme gösterilmektedir.

> [!NOTE]
> Bu belge veritabanı şeması ASP.NET üyelik tabanlı uygulamalar için ASP.NET Core kimliği için kullanılan veritabanı şeması geçirmek için gerekli adımları sağlar. ASP.NET üyelik tabanlı kimlik doğrulamasını ASP.NET Identity geçirme hakkında daha fazla bilgi için bkz: [mevcut bir uygulamayı ASP.NET Identity SQL üyeliğinden geçirmek](/aspnet/identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity). ASP.NET Core kimliği hakkında daha fazla bilgi için bkz: [kimliği ASP.NET Core üzerinde giriş](xref:security/authentication/identity).

## <a name="review-of-membership-schema"></a>Üyelik şemasının gözden geçirme

ASP.NET 2.0 önce geliştiriciler uygulamalarını için tüm kimlik doğrulama ve yetkilendirme işlemi oluşturma ile görevli. ASP.NET 2.0 ile güvenlik ASP.NET uygulamaların içindeki işleme için ortak bir çözüm sağlayarak üyelik sunulmuştur. Geliştiriciler ile bir SQL Server veritabanına bir şema bootstrap mümkün şimdi [aspnet_regsql.exe](https://msdn.microsoft.com/library/ms229862.aspx) komutu. Bu komutu çalıştırdıktan sonra aşağıdaki tablolarda veritabanında oluşturulmuş.

  ![Üyelik tablolarını](identity/_static/membership-tables.png)

ASP.NET Core 2.0 kimliği için var olan uygulamaları geçirmek için bu tablolardaki verileri yeni kimlik şema tarafından kullanılan tablolar için geçirilmesi gerekir.

## <a name="aspnet-core-identity-20-schema"></a>ASP.NET Core kimlik 2.0 şeması

ASP.NET Core 2.0 izleyen [kimlik](/aspnet/identity/index) ASP.NET 4.5 içinde sunulan ilkesi. İlkeye paylaşılan olsa çerçeveleri arasında hatta ASP.NET Core sürümleri arasında farklı uygulamasıdır (bkz [ASP.NET Core 2.0 kimlik doğrulama ve kimlik geçirmek](xref:migration/1x-to-2x/index)).

ASP.NET Core 2.0 Identity için şema görüntülemek için en hızlı yolu, yeni bir ASP.NET Core 2.0 uygulaması oluşturmaktır. Visual Studio 2017'nda aşağıdaki adımları izleyin:

* Select **File** > **New** > **Project**.
* Yeni bir **ASP.NET çekirdek Web uygulaması**ve proje adı *CoreIdentitySample*.
* Seçin **ASP.NET Core 2.0** açılır ve ardından **Web uygulaması**. Bu şablon üreten bir [Razor sayfalarının](xref:mvc/razor-pages/index) uygulama. Tıklatmadan önce **Tamam**, tıklatın **kimlik doğrulamayı Değiştir**.
* Seçin **tek tek kullanıcı hesaplarını** kimlik şablonları için. Son olarak, tıklatın **Tamam**, ardından **Tamam**. Visual Studio ASP.NET Core kimliği şablonunu kullanarak bir proje oluşturur.

ASP.NET Core 2.0 kimliğini kullanır [Entity Framework Çekirdek](/ef/core) kimlik doğrulama verileri depolamak veritabanıyla etkileşim kurmak için. Yeni oluşturulan uygulamanın çalışması için sırayla var. Bu verileri depolamak için bir veritabanı olması gerekiyor. Yeni bir uygulama oluşturduktan sonra bir veritabanı ortamında şema incelemek için en hızlı yolu Entity Framework geçişler kullanarak veritabanı oluşturmaktır. Bu işlem, bir veritabanı, yerel olarak ya da başka bir yerde, hangi bu şema taklit eder oluşturur. Daha fazla bilgi için önceki belgelerini gözden geçirin.

ASP.NET Core kimliği şeması ile bir veritabanı oluşturmak için çalıştırın `Update-Database` Visual Studio'nun komutunu **Paket Yöneticisi Konsolu** (PMC) penceresi&mdash;konumunda bulunan **Araçları**  >  **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu**. PMC çalışan Entity Framework komutları destekler.

Entity Framework komutları belirtilen veritabanı için bağlantı dizesi kullanın *appsettings.json*. Aşağıdaki bağlantı dizesi üzerinde bir veritabanını hedefleyip *localhost* adlı *asp net çekirdek kimlik*. Bu ayar, Entity Framework kullanmak üzere yapılandırılmış `DefaultConnection` bağlantı dizesi.

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=localhost;Database=aspnet-core-identity;Trusted_Connection=True;MultipleActiveResultSets=true"
  }
}
```

Bu komut, şemasıyla belirtilen veritabanı ve uygulama başlatma için gereken tüm verileri oluşturur. Aşağıdaki resimde, yukarıdaki adımları ile oluşturulan tablo yapısı gösterilmektedir.

   ![Kimlik tabloları](identity/_static/identity-tables.png)

## <a name="migrate-the-schema"></a>Şema geçirme

Tablo yapıları ve üyelik ve ASP.NET Core kimlik alanlarında küçük farklılıklar vardır. Desen, ASP.NET ve ASP.NET Core uygulamaları ile kimlik doğrulama/yetkilendirme için önemli ölçüde değişti. Kimlikle hala kullanılan anahtar nesneler *kullanıcılar* ve *rolleri*. İçin eşleme tabloları işte *kullanıcılar*, *rolleri*, ve *UserRoles*.

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
> Tüm alan eşlemelerini ASP.NET Core kimliği üyeliğinin bire bir ilişkiler benzer. Önceki tabloda varsayılan üyelik kullanıcısının şema alır ve ASP.NET Core kimliği şemaya eşler. Üyelik için kullanılan herhangi bir özel alanlar el ile eşlenmesi gerekir. Bu eşleme içinde yoktur parolalar için hiçbir eşleme parola ölçütlerini ve parola salts ikisi arasında geçirme gibi. **Bu parola null olarak bırakın ve kullanıcıların parolalarını sıfırlayabilmesi için sormak için önerilir.** ASP.NET çekirdekli Kimlikteki `LockoutEnd` kullanıcıya kilitlenmişse tarihi gelecekte ayarlamanız gerekir. Bu geçiş komut dosyasındaki gösterilir.

### <a name="roles"></a>Roller

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

Başvuru için bir geçiş komut dosyası oluştururken, önceki eşleme tabloları *kullanıcılar* ve *rolleri*. Aşağıdaki örnek, bir veritabanı sunucusunda iki veritabanı olduğunu varsayar. Bir veritabanı varolan ASP.NET üyelik şemasını ve verileri içerir. Daha önce açıklanan adımları kullanarak diğer veritabanı oluşturuldu. Daha fazla ayrıntı için dahil edilen satır içi açıklamalardır.

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
       --Security Stamp is a token used to verify the state of an account and is subject to change at any time. It should be intialized as a new ID.
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

Bu komut dosyası tamamlandıktan sonra daha önce oluşturduğunuz ASP.NET Core kimlik uygulaması üyelik kullanıcılarının ile doldurulur. Kullanıcıların oturum açmayı önce kullanıcıların parolalarını değiştirmesi gerekir.

> [!NOTE]
> Üyelik Sistemi kullanıcılar kendi e-posta adresi ile eşleşmedi kullanıcı adlarıyla olsaydı, bunu uygun hale getirmek için daha önce oluşturduğunuz uygulama değişiklik gerekmez. Varsayılan şablonu bekliyor `UserName` ve `Email` aynı olmalıdır. Bunlar farklı durumlarda, oturum açma işlemi kullanmak için değiştirilmesi gereken `UserName` yerine `Email`.

İçinde `PageModel` konumunda bulunan oturum açma sayfasının *Pages\Account\Login.cshtml.cs*, kaldırma `[EmailAddress]` özniteliğini *e-posta* özelliği. İçin yeniden adlandırmadan *kullanıcıadı*. Bu değişiklik gerektiren her yerde `EmailAddress` , içinde açıklanan *Görünüm* ve *PageModel*. Sonuç aşağıdaki gibi görünür:

 ![Sabit oturum açma](identity/_static/fixed-login.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, ASP.NET Core 2.0 kimlik SQL üyeliğinin kullanıcılara bağlantı noktası öğrendiniz. ASP.NET Core kimliği ile ilgili daha fazla bilgi için bkz: [kimlik giriş](xref:security/authentication/identity).
