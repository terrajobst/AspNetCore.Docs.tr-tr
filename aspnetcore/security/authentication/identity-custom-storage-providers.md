---
title: ASP.NET Core kimliği için özel depolama sağlayıcıları
author: ardalis
description: ASP.NET Core kimliği için özel depolama sağlayıcıları yapılandırmayı öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: security/authentication/identity-custom-storage-providers
ms.openlocfilehash: b10731261ca0c748548fcba94a229ba055d46eb5
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090842"
---
# <a name="custom-storage-providers-for-aspnet-core-identity"></a>ASP.NET Core kimliği için özel depolama sağlayıcıları

Tarafından [Steve Smith](https://ardalis.com/)

ASP.NET Core kimliği bir özel depolama sağlayıcısı oluşturmanıza ve bunu uygulamanıza bağlamanıza olanak tanıyan genişletilebilir bir sistemdir. Bu konuda, bir ASP.NET Core kimliği için özelleştirilmiş depolama sağlayıcısı oluşturmayı açıklar. Kendi depolama sağlayıcısı oluşturmak için önemli kavramlar ele alınmaktadır ancak adım adım bir kılavuz değildir.

[Görüntülemek veya örnek Github'dan indirin](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample).

## <a name="introduction"></a>Giriş

Varsayılan olarak, ASP.NET Core kimlik sistemi, kullanıcı bilgilerini Entity Framework Core kullanan SQL Server veritabanında depolar. Birçok uygulama için bu yaklaşım işe yarar. Ancak, farklı Kalıcılık mekanizması ya da veri şeması kullanmayı tercih edebilirsiniz. Örneğin:

* Kullandığınız [Azure tablo depolama](/azure/storage/) veya başka bir veri deposu.
* Veritabanı tablolarınızı farklı bir yapıdadır. 
* Gibi farklı veri erişim yaklaşımı kullanmak isteyebilirsiniz [Dapper](https://github.com/StackExchange/Dapper). 

Her durumda, depolama yönteminiz için özelleştirilmiş bir sağlayıcı yazma ve o sağlayıcı uygulamanıza takın.

ASP.NET Core kimliği "Bireysel kullanıcı hesapları" seçeneği ile Visual Studio Proje şablonları dahil edilir.

.NET Core CLI'yı kullanarak, ekleme `-au Individual`:

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
```

## <a name="the-aspnet-core-identity-architecture"></a>ASP.NET Core kimliği mimarisi

ASP.NET Core kimliği yöneticileri ve depoları adlı sınıftan oluşur. *Yöneticileri* kimlik kullanıcı oluşturmak gibi işlemleri gerçekleştirmek için bir uygulama geliştiricisi kullanan üst düzey sınıflar. *Depoları* kullanıcılar ve roller gibi varlıkları nasıl kalıcı belirtin alt düzey sınıflar. Depoları depo deseni izler ve yakından Kalıcılık mekanizması ile bağlı. Yöneticileri, Kalıcılık mekanizması (yapılandırma dışında) uygulama kodunu değiştirmeden değiştirebileceğiniz anlamına gelir, mağazalardan birbirinden ayrılmıştır.

Aşağıdaki diyagramda depolarının veri erişim katmanı ile etkileşim kurarken bir web uygulaması yöneticileri ile nasıl etkileştiğini gösterilmektedir.

![ASP.NET Core uygulamaları yöneticilerine (örneğin, 'UserManager', 'RoleManager') çalışır. Yöneticileri iletişim depoları (örneğin, ' UserStore') ile Entity Framework Core gibi bir kitaplık kullanılarak bir veri kaynağı ile çalışır.](identity-custom-storage-providers/_static/identity-architecture-diagram.png)

Özel depolama sağlayıcısı oluşturmak için bu veri erişim katmanı'nı (Yukarıdaki diyagramda yer yeşil ve gri olanlar) ile veri kaynağı, veri erişim katmanı ve etkileşim deposu sınıflar oluşturun. Yöneticileri veya bunlarla (yukarıdaki mavi kutu) etkileşim kurar, uygulama kodunuz özelleştirme gerek yoktur.

Yeni bir örneğini oluştururken `UserManager` veya `RoleManager` kullanıcı sınıf türü sağlayın ve depolama sınıfının bir örneği bir bağımsız değişken olarak geçirin. Bu yaklaşım, ASP.NET Core özelleştirilmiş sınıflarınızı takın sağlar. 

[Yeni depolama sağlayıcısı kullanmak için uygulamayı yeniden](#reconfigure-app-to-use-a-new-storage-provider) örneği gösterilmiştir `UserManager` ve `RoleManager` özelleştirilmiş deposuyla.

## <a name="aspnet-core-identity-stores-data-types"></a>ASP.NET Core kimliği veri türleri depolar.

[ASP.NET Core kimliği](https://github.com/aspnet/identity) veri türleri aşağıdaki bölümlerde ayrıntılı:

### <a name="users"></a>Kullanıcılar

Kayıtlı kullanıcıların web sitenizin. [IdentityUser](/dotnet/api/microsoft.aspnet.identity.corecompat.identityuser) türü genişletilmiş veya kendi özel tür için örnek olarak kullanılır. Kendi özel kimlik depolama çözümü uygulamak için belirli bir türden devralmak gerek yoktur.

### <a name="user-claims"></a>Kullanıcı talepleri

İfadeler bir dizi (veya [talep](/dotnet/api/system.security.claims.claim)) kullanıcının kimliğini temsil eden kullanıcı hakkında. Greater ifadesini daha rolleri sağlanabilir kullanıcının kimliğinin etkinleştirebilirsiniz.

### <a name="user-logins"></a>Kullanıcı oturumu açma

Dış kimlik doğrulama sağlayıcısı (örneğin, Facebook veya bir Microsoft hesabı) hakkında bilgi bir kullanıcı oturum açarken kullanılacak. [Örnek](/dotnet/api/microsoft.aspnet.identity.corecompat.identityuserlogin)

### <a name="roles"></a>Rolleri

Siteniz için yetkilendirme gruplar. Rol Kimliği ve rol adı (ör. "Yönetici" veya "Employee") içerir. [Örnek](/dotnet/api/microsoft.aspnet.identity.corecompat.identityrole)

## <a name="the-data-access-layer"></a>Veri erişim katmanı

Bu konuda, kullanacaksanız Kalıcılık mekanizması ve bu mekanizma için varlıklar oluşturma hakkında bilgi sahibi olduğunuz varsayılır. Bu konuda depoları veya veri erişim sınıfları oluşturma hakkında ayrıntılı bilgi sağlamaz; ASP.NET Core kimliği ile çalışırken tasarım kararları hakkında bazı öneriler sağlar.

Veri erişim katmanı için özelleştirilmiş depolama sağlayıcısı tasarlarken çok sayıda özgürlüğü sahip. Uygulamanızda kullanmak istediğiniz özellikler için Kalıcılık mekanizması oluşturmanız yeterlidir. Örneğin, uygulamanızda rolleri kullanmıyorsanız, roller veya kullanıcı rolü ilişkileri için depolama oluşturulması gerekmez. ASP.NET Core kimliği varsayılan uygulamasından çok farklı bir yapı, teknoloji ve mevcut altyapınızı gerektirebilir. Veri erişim katmanındaki depolama uygulamanız yapısı ile çalışmak için mantığı sağlar.

Veri erişim katmanı verileri ASP.NET Core kimlik bir veri kaynağına kaydetmek için mantığı sağlar. Veri erişim katmanı, özelleştirilmiş depolama sağlayıcısı için kullanıcı ve rol bilgilerini depolamak için aşağıdaki sınıfları içerebilir.

### <a name="context-class"></a>Bağlam sınıfı

Kalıcılık mekanizmanızı bağlanmak ve sorgu yürütmek için bilgileri yalıtır. Çeşitli veri sınıfları normalde bağımlılık ekleme sağlanır, bu sınıfın bir örneğini gerektirir. [Örnek](/dotnet/api/microsoft.aspnet.identity.corecompat.identitydbcontext-1).

### <a name="user-storage"></a>Kullanıcı depolama

Kullanıcı bilgilerini (örneğin, kullanıcı adı ve parola karması) alır ve depolar. [Örnek](/dotnet/api/microsoft.aspnet.identity.corecompat.userstore-1)

### <a name="role-storage"></a>Rol depolama

Rol bilgilerini (örneğin, rol adı) alır ve depolar. [Örnek](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.rolestore-1)

### <a name="userclaims-storage"></a>UserClaims depolama

Kullanıcı talebi bilgilerini (örneğin, talep türü ve değeri) alır ve depolar. [Örnek](/dotnet/api/microsoft.aspnet.identity.corecompat.userstore-1)

### <a name="userlogins-storage"></a>UserLogins depolama

Kullanıcı oturum açma bilgilerini (örneğin, bir dış kimlik doğrulama sağlayıcısı) alır ve depolar. [Örnek](/dotnet/api/microsoft.aspnet.identity.corecompat.userstore-1)

### <a name="userrole-storage"></a>UserRole depolama

Hangi rol için hangi kullanıcıların atandığı alır ve depolar. [Örnek](/dotnet/api/microsoft.aspnet.identity.corecompat.userstore-1)

**İpucu:** uygulamanızda kullanmayı düşündüğünüz sınıfları yalnızca uygulayın.

Veri erişim sınıflarda, Kalıcılık mekanizması için veri işlemleri gerçekleştirmek için kod sağlayın. Örneğin, özel bir sağlayıcı içinde yeni bir kullanıcı oluşturmak için aşağıdaki kodu sahip olabileceğiniz *depolamak* sınıfı:

[!code-csharp[](identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/CustomUserStore.cs?name=createuser&highlight=7)]

Kullanıcı oluşturmak için uygulama mantığı bulunduğu `_usersTable.CreateAsync` yöntemi aşağıda gösterilmektedir.

## <a name="customize-the-user-class"></a>Kullanıcı sınıfı özelleştirme

Depolama sağlayıcısı uygulama değerine eşdeğer olan bir kullanıcı sınıfı oluşturduğunuzda [IdentityUser sınıfı](/dotnet/api/microsoft.aspnet.identity.corecompat.identityuser).

En az kullanıcı sınıfınıza içermelidir bir `Id` ve `UserName` özelliği.

`IdentityUser` Sınıfı tanımlayan özellikleri, `UserManager` istenen işlemleri gerçekleştirirken çağrıları. Varsayılan türü `Id` özelliği bir dize olan, ancak öğesinden devralabilir `IdentityUser<TKey, TUserClaim, TUserRole, TUserLogin, TUserToken>` ve farklı bir tür belirtin. Veri türü dönüştürmelerini işlemek için depolama uygulaması framework bekliyor.

## <a name="customize-the-user-store"></a>Kullanıcı deposu özelleştirme

Oluşturma bir `UserStore` kullanıcının tüm veri işlemleri için yöntemleri sağlayan sınıf. Bu sınıf eşdeğerdir [UserStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.userstore-1) sınıfı. İçinde `UserStore` sınıfı, uygulama `IUserStore<TUser>` ve gereken isteğe bağlı arabirimler. Hangi isteğe bağlı bir arabirim uygulamak için uygulamanızda sağlanan işlevselliği göre seçin.

### <a name="optional-interfaces"></a>İsteğe bağlı arabirimler

* [Iuserrolestore](/dotnet/api/microsoft.aspnetcore.identity.iuserrolestore-1)
* [Iuserclaimstore](/dotnet/api/microsoft.aspnetcore.identity.iuserclaimstore-1)
* [Iuserpasswordstore](/dotnet/api/microsoft.aspnetcore.identity.iuserpasswordstore-1)
* [IUserSecurityStampStore](/dotnet/api/microsoft.aspnetcore.identity.iusersecuritystampstore-1)
* [Iuseremailstore](/dotnet/api/microsoft.aspnetcore.identity.iuseremailstore-1)
* [IPhoneNumberStore](/dotnet/api/microsoft.aspnetcore.identity.iphonenumberstore-1)
* [Iqueryableuserstore](/dotnet/api/microsoft.aspnetcore.identity.iqueryableuserstore-1)
* [Iuserloginstore](/dotnet/api/microsoft.aspnetcore.identity.iuserloginstore-1)
* [Iusertwofactorstore](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactorstore-1)
* [Iuserlockoutstore](/dotnet/api/microsoft.aspnetcore.identity.iuserlockoutstore-1)

İsteğe bağlı arabirimler devralınacak `IUserStore<TUser>`. Depolar kısmen uygulanan örnek kullanıcı gördüğünüz [örnek uygulaması](https://github.com/aspnet/Docs/blob/master/aspnetcore/security/authentication/identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/CustomUserStore.cs).

İçinde `UserStore` sınıfı, işlemleri gerçekleştirmek için oluşturduğunuz veri erişim sınıfları kullanın. Bu bağımlılık ekleme kullanılarak geçirilir. Örneğin, Dapper uygulaması, SQL Server'da `UserStore` sınıfında `CreateAsync` bir örneği kullanan yöntemi `DapperUsersTable` yeni bir kayıt eklemek için:

[!code-csharp[](identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/DapperUsersTable.cs?name=createuser&highlight=7)]

### <a name="interfaces-to-implement-when-customizing-user-store"></a>Kullanıcı deposu özelleştirirken uygulanacak arabirimleri

* **Iuserstore**  
 [Iuserstore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserstore-1) kullanıcı deposunda uygulanmalı yalnızca arabirim arabirimidir. Bu, oluşturma, güncelleştirme, silme ve kullanıcıları alınırken için yöntemleri tanımlar.
* **Iuserclaimstore**  
 [Iuserclaimstore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserclaimstore-1) arabirimi uygulayan kullanıcı talepleri etkinleştirmek için yöntemleri tanımlar. Bu, ekleme, kaldırma ve kullanıcı talepleri almak için yöntemler içerir.
* **Iuserloginstore**  
 [Iuserloginstore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserloginstore-1) uygulamak Dış kimlik doğrulama sağlayıcıları etkinleştirmek için yöntemleri tanımlar. Bu, ekleme, kaldırma ve kullanıcı oturum açma bilgileri ve oturum açma bilgilerine dayalı bir kullanıcı almak için bir yöntem almak için yöntemler içerir.
* **Iuserrolestore**  
 [Iuserrolestore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserrolestore-1) arabirimi uygulayan bir role kullanıcı eşlemek için yöntemleri tanımlar. Bu, ekleme, kaldırma ve bir kullanıcının rollerini ve bir kullanıcı bir role atanmış ise denetlemek için bir yöntem almak için yöntemler içerir.
* **Iuserpasswordstore**  
 [Iuserpasswordstore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserpasswordstore-1) arabirimi uygulayan karma parolalar kalıcı hale getirmek için yöntemleri tanımlar. Bu, alma ve karma hale getirilen parola ve kullanıcı bir parola olarak ayarlanmış olup olmadığını gösteren bir yöntemi ayarlama için yöntemleri içerir.
* **IUserSecurityStampStore**  
 [IUserSecurityStampStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iusersecuritystampstore-1) arabirimi uygulayan bir güvenlik damgası için kullanıcının hesap bilgilerini değiştirilip değiştirilmediğini gösteren kullanmak için yöntemleri tanımlar. Bir kullanıcı parolasını değiştirir veya ekler veya oturumları kaldırır. Bu damga güncelleştirilir. Bu, alma ve güvenlik damgasını ayarlama için yöntemleri içerir.
* **Iusertwofactorstore**  
 [Iusertwofactorstore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactorstore-1) arabirimini uygulamak iki faktörlü kimlik doğrulamasını desteklemek için yöntemleri tanımlar. Bu, alma ve iki faktörlü kimlik doğrulamasını bir kullanıcı için etkinleştirilip etkinleştirilmediğini ayarlama için yöntemleri içerir.
* **IUserPhoneNumberStore**  
 [Iuserphonenumberstore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserphonenumberstore-1) arabirimi uygulayan kullanıcı telefon numaralarını depolamak için yöntemleri tanımlar. Bu, alma ve telefon numarasını ve telefon numarasının onaylanıp olup ayarlama için yöntemleri içerir.
* **Iuseremailstore**  
 [Iuseremailstore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuseremailstore-1) arabirimi uygulayan kullanıcı e-posta adresleri saklamak için yöntemleri tanımlar. Bu, alma ve e-posta adresini ve e-posta olup olmadığını onaylandıktan ayarlama için yöntemleri içerir.
* **Iuserlockoutstore**  
 [Iuserlockoutstore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserlockoutstore-1) arabirimi uygulayan bir hesap kilitleme hakkında bilgi depolamak için yöntemleri tanımlar. Bu, başarısız erişim denemesi ve kilitlenmeleri izlemek için yöntemler içerir.
* **Iqueryableuserstore**  
 [Iqueryableuserstore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iqueryableuserstore-1) arabirimi uygulayan sorgulanabilir bir kullanıcı deposunun sağlamak için üyeleri tanımlar.

Uygulamanızda'de gerekli arabirimleri uygulayın. Örneğin:

```csharp
public class UserStore : IUserStore<IdentityUser>,
                         IUserClaimStore<IdentityUser>,
                         IUserLoginStore<IdentityUser>,
                         IUserRoleStore<IdentityUser>,
                         IUserPasswordStore<IdentityUser>,
                         IUserSecurityStampStore<IdentityUser>
{
    // interface implementations not shown
}
```

### <a name="identityuserclaim-identityuserlogin-and-identityuserrole"></a>IdentityUserClaim IdentityUserLogin ve IdentityUserRole

`Microsoft.AspNet.Identity.EntityFramework` Ad alanı içeriyor uygulamalarına [IdentityUserClaim](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuserclaim-1), [IdentityUserLogin](/dotnet/api/microsoft.aspnet.identity.corecompat.identityuserlogin), ve [IdentityUserRole](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuserrole-1) sınıfları. Bu özelliklerin kullanmanız durumunda bu sınıfların kendi sürüm oluşturma ve uygulamanızı özelliklerini tanımlamak isteyebilirsiniz. Ancak, bazen bu varlıkları belleğe (örneğin, ekleme veya bir kullanıcı talebinin kaldırma) temel işlemleri gerçekleştirirken yüklememeye daha etkilidir. Bunun yerine, arka uç depolama sınıfları bu işlemleri doğrudan veri kaynağına yürütebilir. Örneğin, `UserStore.GetClaimsAsync` yöntemi çağırabilir `userClaimTable.FindByUserId(user.Id)` doğrudan tablo ve talep dönmesini bir sorgu yürütmek için yöntemi.

## <a name="customize-the-role-class"></a>Rol sınıfı özelleştirme

Rol depolama sağlayıcısını uygularken, bir özel rol türü oluşturabilirsiniz. Belirli bir arabirim uygulamak zorunda olmadığı, ancak olmalıdır bir `Id` ve genellikle olacak bir `Name` özelliği.

Aşağıdaki örnek bir rol sınıf gösterilmektedir:

[!code-csharp[](identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/ApplicationRole.cs)]

## <a name="customize-the-role-store"></a>Rol deposu özelleştirme

Oluşturabileceğiniz bir `RoleStore` rollerde tüm veri işlemleri için yöntemleri sağlayan sınıf. Bu sınıf eşdeğerdir [RoleStore&lt;TRole&gt; ](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.rolestore-1) sınıfı. İçinde `RoleStore` sınıfı, uygulamanız `IRoleStore<TRole>` ve isteğe bağlı olarak `IQueryableRoleStore<TRole>` arabirimi.

* **Irolestore&lt;TRole&gt;**  
 [Irolestore&lt;TRole&gt; ](/dotnet/api/microsoft.aspnetcore.identity.irolestore-1) arabirimi rol deposuna sınıfında uygulanacak yöntemleri tanımlar. Bu, oluşturma, güncelleştirme, silme ve roller alınıyor için yöntemler içerir.
* **RoleStore&lt;TRole&gt;**  
 Özelleştirme için `RoleStore`, uygulayan bir sınıf oluşturma `IRoleStore<TRole>` arabirimi. 

## <a name="reconfigure-app-to-use-a-new-storage-provider"></a>Yeni bir depolama sağlayıcısı kullanmak için uygulamayı yeniden yapılandırın

Bir depolama sağlayıcısı uyguladıktan sonra bunu kullanmak için uygulamanızı yapılandırın. Uygulamanız varsayılan sağlayıcı kullandıysanız, özel sağlayıcınızın ile değiştirin.

1. Kaldırma `Microsoft.AspNetCore.EntityFramework.Identity` NuGet paketi.
1. Depolama sağlayıcısı ayrı bir proje veya paket içinde yer alıyorsa buna bir başvuru ekleyin.
1. Tüm başvuruları değiştirin `Microsoft.AspNetCore.EntityFramework.Identity` ile kullanarak bir depolama alanı sağlayıcınızla ad alanı bildirimi.
1. İçinde `ConfigureServices` yöntemini, değiştirme `AddIdentity` özel türleriniz için yöntemi. Kendi bu amaç için genişletme yöntemleri oluşturabilirsiniz. Bkz: [IdentityServiceCollectionExtensions](https://github.com/aspnet/Identity/blob/rel/1.1.0/src/Microsoft.AspNetCore.Identity/IdentityServiceCollectionExtensions.cs) örneği.
1. Rolleri kullanıyorsanız, güncelleştirme `RoleManager` kullanmak için `RoleStore` sınıfı.
1. Kimlik bilgileri ve bağlantı dizesi için uygulamanızın yapılandırmasını güncelleştirin.

Örnek:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add identity types
    services.AddIdentity<ApplicationUser, ApplicationRole>()
        .AddDefaultTokenProviders();

    // Identity Services
    services.AddTransient<IUserStore<ApplicationUser>, CustomUserStore>();
    services.AddTransient<IRoleStore<ApplicationRole>, CustomRoleStore>();
    string connectionString = Configuration.GetConnectionString("DefaultConnection");
    services.AddTransient<SqlConnection>(e => new SqlConnection(connectionString));
    services.AddTransient<DapperUsersTable>();

    // additional configuration
}
```

## <a name="references"></a>Referanslar

* [ASP.NET 4.x kimliği için özel depolama sağlayıcıları](/aspnet/identity/overview/extensibility/overview-of-custom-storage-providers-for-aspnet-identity)
* [ASP.NET Core kimliği](https://github.com/aspnet/identity) &ndash; bu depo deposu sağlayıcıları tutulan topluluğu bağlantılarını içerir.
