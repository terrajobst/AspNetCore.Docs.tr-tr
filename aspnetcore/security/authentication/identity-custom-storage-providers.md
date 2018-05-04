---
title: ASP.NET Core kimliği için özel depolama sağlayıcıları
author: ardalis
description: Özel depolama sağlayıcıları ASP.NET Core kimliği için yapılandırmayı öğrenin.
manager: wpickett
ms.author: riande
ms.date: 05/24/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/identity-custom-storage-providers
ms.openlocfilehash: bd5e5219765dfea0305fa02e79e5423266ce4df2
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
---
# <a name="custom-storage-providers-for-aspnet-core-identity"></a>ASP.NET Core kimliği için özel depolama sağlayıcıları

Tarafından [Steve Smith](https://ardalis.com/)

ASP.NET Core kimliği bir özel depolama sağlayıcısı oluşturmak ve uygulamanıza bağlanmak sağlayan genişletilebilir bir sistemdir. Bu konu, ASP.NET Core kimliği için özelleştirilmiş depolama sağlayıcısı oluşturmayı açıklar. Kendi depolama sağlayıcısı oluşturmak için önemli kavramları içerir, ancak bir adım adım kılavuz değildir.

[Görüntülemek veya örnek Github'dan indirdiğinizde](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample).

## <a name="introduction"></a>Giriş

Varsayılan olarak, ASP.NET Core kimlik sistemi kullanıcı bilgilerini Entity Framework Çekirdek kullanarak bir SQL Server veritabanında saklar. Birçok uygulamalar için bu yaklaşım iyi çalışır. Ancak, farklı Kalıcılık mekanizması veya veri şeması kullanmayı tercih edebilirsiniz. Örneğin:

* Kullandığınız [Azure Table Storage](https://docs.microsoft.com/azure/storage/) veya başka bir veri deposu.
* Veritabanı tabloları farklı bir yapıya sahip. 
* Gibi farklı veri erişimi yaklaşımı kullanmak isteyebilirsiniz [Dapper](https://github.com/StackExchange/Dapper). 

Her durumda, depolama yönteminiz için özelleştirilmiş bir sağlayıcı yazma ve bu sağlayıcıyı uygulamanıza takın.

ASP.NET Core kimlik "Tek tek kullanıcı hesaplarını" seçeneği ile Visual Studio Proje şablonları dahil edilir.

.NET Core CLI kullanırken eklemek `-au Individual`:

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
```

## <a name="the-aspnet-core-identity-architecture"></a>ASP.NET Core kimliği mimarisi

ASP.NET Core kimliği, yöneticileri ve depoları adlı sınıfları oluşur. *Yöneticileri* kimlik kullanıcı oluşturma gibi işlemleri gerçekleştirmek için bir uygulama geliştiricisi kullanan üst düzey sınıflarıdır. *Depoları* kullanıcılar ve roller gibi varlıkları nasıl kalıcı belirtin alt düzey sınıflar. Depoları izleyin [havuz deseni](http://deviq.com/repository-pattern/) ve Kalıcılık mekanizması ile yakından bağlı değildir. Yöneticileri (yapılandırma dışında) uygulama kodunu değiştirmeden Kalıcılık mekanizması değiştirebilirsiniz anlamına gelir, depolarına birbirinden ayrılır.

Aşağıdaki diyagramda depoları veri erişim katmanı ile etkileşim sırasında bir web uygulaması yöneticileri ile nasıl etkileşim gösterir.

![ASP.NET Core uygulamaları (örneğin, 'UserManager', 'RoleManager') yöneticileri ile çalışır. Yöneticileri, hangi iletişim depoları (örneğin, ' UserStore') ile Entity Framework Çekirdek gibi bir kitaplık kullanılarak bir veri kaynağı ile çalışır.](identity-custom-storage-providers/_static/identity-architecture-diagram.png)

Özel depolama sağlayıcısı oluşturmak için bu veri erişim katmanı (yeşil ve gri kutularına Yukarıdaki diyagramda) ile veri kaynağı, veri erişim katmanı ve etkileşim mağaza sınıfları oluşturun. Yöneticileri veya bunlarla (yukarıdaki mavi kutuları) etkileşim uygulama kodunuz özelleştirme gerek yoktur.

Yeni bir örneğini oluştururken `UserManager` veya `RoleManager` kullanıcı sınıfı tür sağlamak ve deposu sınıfının bir örneği bağımsız değişken olarak geçirin. Bu yaklaşım, özelleştirilmiş sınıflarınızı ASP.NET Core takın olanak sağlar. 

[Yeni depolama sağlayıcısı kullanmak için uygulamayı yeniden](#reconfigure-app-to-use-new-storage-provider) örneği gösterilmektedir `UserManager` ve `RoleManager` özelleştirilmiş deposuyla.

## <a name="aspnet-core-identity-stores-data-types"></a>Veri türleri ASP.NET Core kimlik depolar

[ASP.NET Core kimliği](https://github.com/aspnet/identity) veri türleri aşağıdaki bölümlerde ayrıntılı:

### <a name="users"></a>Kullanıcılar

Kayıtlı kullanıcıların web sitenizin. [IdentityUser](/dotnet/api/microsoft.aspnet.identity.corecompat.identityuser) türü genişletilmiş veya kendi özel tür için örnek olarak kullanılmıştır. Kendi özel kimlik depolama çözümü uygulamak için belirli bir türünden devralan gerek yoktur.

### <a name="user-claims"></a>Kullanıcı talepleri

Bir dizi deyimleri (veya [talep](/dotnet/api/system.security.claims.claim)) kullanıcının kimliğini temsil eder kullanıcı hakkında. Kullanıcının kimliğini rolleri aracılığıyla elde edilebilir daha büyük ifade etkinleştirebilirsiniz.

### <a name="user-logins"></a>Kullanıcı oturumu

Dış kimlik doğrulama sağlayıcısı (Facebook veya gibi bir Microsoft hesabı) hakkında bilgi bir kullanıcı oturum açarken kullanılacak. [Örnek](/dotnet/api/microsoft.aspnet.identity.corecompat.identityuserlogin)

### <a name="roles"></a>Roller

Siteniz için yetkilendirme grupları. Rol Kimliği ve rol adı (ör. "Yönetici" veya "Çalışan") içerir. [Örnek](/dotnet/api/microsoft.aspnet.identity.corecompat.identityrole)

## <a name="the-data-access-layer"></a>Veri erişim katmanı

Bu konuda, kullanacaksanız kalıcılığı mekanizmasının ve o mekanizması için varlıkların nasıl oluşturulacağı hakkında bilgi sahibi olduğunuz varsayılır. Bu konuda depoları veya veri erişim sınıfları oluşturmak nasıl kullanılacağı hakkındaki ayrıntıları sağlamaz; ASP.NET Core kimliği ile çalışırken tasarım kararları hakkında bazı öneriler sağlar.

Serbestlik çok miktarda veri erişim katmanı için özelleştirilmiş depolama sağlayıcısı tasarlarken vardır. Yalnızca uygulamanızı kullanmayı düşündüğünüz özellikleri için Kalıcılık mekanizmaları oluşturmanız gerekir. Örneğin, uygulamanızda rolleri kullanmıyorsanız, roller veya kullanıcı rolü ilişkileri için depolama oluşturmanız gerekmez. Teknoloji ve varolan altyapı ASP.NET Core kimliği varsayılan uygulamasından çok farklı bir yapı gerektirebilir. Veri erişim katmanı'nızda depolama uygulamanızı yapısı ile çalışmak için mantığı sağlar.

Veri erişim katmanı verileri ASP.NET Core kimlikten bir veri kaynağına kaydetmek için mantığı sağlar. Veri erişim katmanı, özelleştirilmiş depolama sağlayıcısı için kullanıcı ve rol bilgilerini depolamak için aşağıdaki sınıflar içerebilir.

### <a name="context-class"></a>Bağlam sınıfı

İçin Kalıcılık mekanizması bağlanmak ve sorguları yürütmek için bilgileri yalıtır. Çeşitli veri sınıfları genellikle bağımlılık ekleme sağlanan bu sınıfının bir örneğini gerektirir. [Örnek](/dotnet/api/microsoft.aspnet.identity.corecompat.identitydbcontext-1).

### <a name="user-storage"></a>Kullanıcı depolama

Kullanıcı bilgilerini (örneğin, kullanıcı adı ve parola karması) alır ve depolar. [Örnek](/dotnet/api/microsoft.aspnet.identity.corecompat.userstore-1)

### <a name="role-storage"></a>Rol depolama

Rol bilgilerini (örneğin, rol adı) alır ve depolar. [Örnek](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.rolestore-1)

### <a name="userclaims-storage"></a>UserClaims depolama

Kullanıcı talebi bilgileri (örneğin, talep türü ve değeri) alır ve depolar. [Örnek](/dotnet/api/microsoft.aspnet.identity.corecompat.userstore-1)

### <a name="userlogins-storage"></a>UserLogins depolama

Kullanıcı oturum açma bilgilerini (örneğin, bir dış kimlik doğrulama sağlayıcısı) alır ve depolar. [Örnek](/dotnet/api/microsoft.aspnet.identity.corecompat.userstore-1)

### <a name="userrole-storage"></a>UserRole depolama

Hangi rollerin hangi kullanıcılara atanan alır ve depolar. [Örnek](/dotnet/api/microsoft.aspnet.identity.corecompat.userstore-1)

**İpucu:** yalnızca uygulamanızı kullanmayı düşündüğünüz sınıfları uygulayın.

Veri erişim sınıfları Kalıcılık mekanizması için veri işlemleri gerçekleştirmek için kod sağlar. Örneğin, özel bir sağlayıcı içinde yeni bir kullanıcı oluşturmak için aşağıdaki kodu sahip olabileceğiniz *depolamak* sınıfı:

[!code-csharp[](identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/CustomUserStore.cs?name=createuser&highlight=7)]

Kullanıcı oluşturmak için uygulama mantığını bulunduğu ``_usersTable.CreateAsync`` aşağıda gösterilen yöntemi.

## <a name="customize-the-user-class"></a>Kullanıcı sınıfı özelleştirme

Bir depolama sağlayıcısı uygularken, eşdeğer olan bir kullanıcı sınıfı oluşturmak [ `IdentityUser` sınıfı](/dotnet/api/microsoft.aspnet.identity.corecompat.identityuser).

En az kullanıcı sınıfınıza içermelidir bir `Id` ve `UserName` özelliği.

`IdentityUser` Sınıfı tanımlayan özellikleri, ``UserManager`` gerçekleştirirken çağrıları istenen işlemleri. Varsayılan türü `Id` özellik bir dize, ancak öğesinden devralmalı `IdentityUser<TKey, TUserClaim, TUserRole, TUserLogin, TUserToken>` ve farklı bir tür belirtin. Veri türü dönüştürmelerini işlemek için depolama uygulaması framework bekliyor.

## <a name="customize-the-user-store"></a>Kullanıcı deposunda özelleştirme

Oluşturma bir `UserStore` kullanıcının tüm veri işlemleri için yöntemleri sağlayan sınıf. Bu sınıf eşdeğerdir [UserStore<TUser> ](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.userstore-1) sınıfı. İçinde `UserStore` sınıfı, uygulama `IUserStore<TUser>` ve gereken isteğe bağlı arabirimler. Uygulamanızda sağlanan işlevselliği temel uygulamak için hangi isteğe bağlı arabirimler seçin.

### <a name="optional-interfaces"></a>İsteğe bağlı arabirimler

- Iuserrolestore /dotnet/api/microsoft.aspnetcore.identity.iuserrolestore-1
- Iuserclaimstore /dotnet/api/microsoft.aspnetcore.identity.iuserclaimstore-1
- Iuserpasswordstore /dotnet/api/microsoft.aspnetcore.identity.iuserpasswordstore-1
- IUserSecurityStampStore <!-- make these all links and remove / -->
- Iuseremailstore
- IPhoneNumberStore
- Iqueryableuserstore
- Iuserloginstore
- Iusertwofactorstore
- Iuserlockoutstore

İsteğe bağlı arabirimler devralınmalıdır `IUserStore`. Depolama kısmen uygulanan örnek kullanıcı görebilirsiniz [burada](https://github.com/aspnet/Docs/blob/master/aspnetcore/security/authentication/identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/CustomUserStore.cs).

İçinde `UserStore` sınıfı, işlemleri gerçekleştirmek için oluşturulan veri erişim sınıfları kullanın. Bu, bağımlılık ekleme kullanılarak geçirilir. Örneğin, SQL Server'daki Dapper uygulaması `UserStore` sınıfına sahip `CreateAsync` bir örneği kullanan yöntemi `DapperUsersTable` yeni bir kayıt eklemek için:

[!code-csharp[](identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/DapperUsersTable.cs?name=createuser&highlight=7)]

### <a name="interfaces-to-implement-when-customizing-user-store"></a>Kullanıcı deposu özelleştirirken uygulamak için arabirimleri

- **Iuserstore**  
 [Iuserstore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserstore-1) kullanıcı deposunda uygulanmalı yalnızca arabirimi bir arabirimdir. Oluşturma, güncelleştirme, silme ve kullanıcıları almak için yöntemleri tanımlar.
- **Iuserclaimstore**  
 [Iuserclaimstore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserclaimstore-1) arabirimi uygulayan kullanıcı talepleri etkinleştirmek için yöntemleri tanımlar. Ekleme, kaldırma ve kullanıcı taleplerini almak için yöntemler içerir.
- **Iuserloginstore**  
 [Iuserloginstore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserloginstore-1) uygulamak Dış kimlik doğrulama sağlayıcıları etkinleştirmek için yöntemleri tanımlar. Ekleme, kaldırma ve kullanıcı oturum açma ve oturum açma bilgilerine dayalı bir kullanıcı almak için bir yöntem almak için yöntemler içerir.
- **Iuserrolestore**  
 [Iuserrolestore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserrolestore-1) arabirimi uygulayan bir kullanıcı rolü için eşlemek için yöntemleri tanımlar. Eklemek, kaldırmak ve bir kullanıcının rollerini ve bir kullanıcı bir role atanmış olmadığını denetlemek için bir yöntem almak için yöntemler içerir.
- **Iuserpasswordstore**  
 [Iuserpasswordstore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserpasswordstore-1) arabirimi uygulayan karma parolalar kalıcı hale getirmek için yöntemleri tanımlar. Karma hale getirilen parola ve kullanıcı bir parola ayarlanmış olup olmadığını belirten bir yöntem ayarlama ve alma için yöntemler içerir.
- **IUserSecurityStampStore**  
 [IUserSecurityStampStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iusersecuritystampstore-1) arabirimi uygulayan bir güvenlik damgası kullanıcının hesap bilgilerini değişip değişmediğini belirten kullanmak için yöntemleri tanımlar. Bir kullanıcı parolasını değiştirir veya ekler veya oturum açma bilgileri kaldırdığında bu damga güncelleştirilir. Kullanıcının güvenlik damgasını ayarlama ve alma için yöntemler içerir.
- **Iusertwofactorstore**  
 [Iusertwofactorstore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactorstore-1) arabirimi uygulayan iki faktörlü kimlik doğrulamasını desteklemek için yöntemleri tanımlar. Bir kullanıcı için iki Etmenli kimlik doğrulamasının etkin olup olmadığını ayarlama ve alma için yöntemler içerir.
- **IUserPhoneNumberStore**  
 [Iuserphonenumberstore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserphonenumberstore-1) arabirimi uygulayan kullanıcı telefon numaralarını depolamak için yöntemleri tanımlar. Telefon numarası ve olup telefon numarasının onaylanıp ayarlama ve alma için yöntemler içerir.
- **Iuseremailstore**  
 [Iuseremailstore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuseremailstore-1) arabirimi uygulayan kullanıcı e-posta adreslerini depolamak için yöntemleri tanımlar. E-posta adresi ve e-posta olup onaylandıktan ayarlama ve alma için yöntemler içerir.
- **Iuserlockoutstore**  
 [Iuserlockoutstore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserlockoutstore-1) arabirimi uygulayan bir hesap kilitleme hakkında bilgi depolamak için yöntemleri tanımlar. Başarısız erişim denemesi ve kilitlemeleri izlemek için yöntemler içerir.
- **Iqueryableuserstore**  
 [Iqueryableuserstore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iqueryableuserstore-1) arabirimi sorgulanabilir bir kullanıcı deposunun sağlamak için üyeleri uygulama tanımlar.

Yalnızca gerekli olan arabirimler uygulamanızda uygulayın. Örneğin:

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

### <a name="identityuserclaim-identityuserlogin-and-identityuserrole"></a>IdentityUserClaim, IdentityUserLogin ve IdentityUserRole

``Microsoft.AspNet.Identity.EntityFramework`` Ad alanında uygulamaları [IdentityUserClaim](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuserclaim-1), [IdentityUserLogin](/dotnet/api/microsoft.aspnet.identity.corecompat.identityuserlogin), ve [IdentityUserRole](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuserrole-1) sınıfları. Bu özellikler kullanıyorsanız, bu sınıfları, kendi sürümlerini oluşturabilir ve uygulamanızı özelliklerini tanımlamak isteyebilirsiniz. Ancak, bazen bu varlıkları belleğe (ekleme veya kullanıcının talep kaldırma gibi) temel işlemleri gerçekleştirirken yüklememeye daha etkilidir. Bunun yerine, arka uç mağaza sınıfları bu işlemleri doğrudan veri kaynağına yürütebilir. Örneğin, ``UserStore.GetClaimsAsync`` yöntemi çağırabilirsiniz ``userClaimTable.FindByUserId(user.Id)`` talepler listesinin doğrudan tablo ve bir sorgu yürütmek için yöntemi.

## <a name="customize-the-role-class"></a>Rol sınıfı özelleştirme

Bir rol depolama sağlayıcısı uygularken, bir özel rol türü oluşturabilirsiniz. Belirli bir arabirim uygulamak, ancak gerekir bir `Id` ve genellikle olacak bir `Name` özelliği.

Örnek bir rol sınıf verilmiştir:

[!code-csharp[](identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/ApplicationRole.cs)]

## <a name="customize-the-role-store"></a>Rol deposu özelleştirme

Oluşturabileceğiniz bir ``RoleStore`` rolleri tüm veri işlemleri için yöntemleri sağlayan sınıf. Bu sınıf eşdeğerdir [RoleStore<TRole> ](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.rolestore-1) sınıfı. İçinde `RoleStore` sınıfı, uygulamanız ``IRoleStore<TRole>`` ve isteğe bağlı olarak ``IQueryableRoleStore<TRole>`` arabirimi.

- **Irolestore&lt;TRole&gt;**  
 [Irolestore](/dotnet/api/microsoft.aspnetcore.identity.irolestore-1) arabirimi rol deposu sınıfında uygulanacak yöntemleri tanımlar. Oluşturma, güncelleştirme, silme ve rolleri almak için yöntemler içerir.
- **RoleStore&lt;TRole&gt;**  
 Özelleştirmek için `RoleStore`, uygulayan bir sınıf oluşturun `IRoleStore` arabirimi. 

## <a name="reconfigure-app-to-use-new-storage-provider"></a>Yeni depolama sağlayıcısı kullanmak için uygulamayı yeniden yapılandırın

Bir depolama sağlayıcısı uyguladıktan sonra kullanmak için uygulamanızı yapılandırın. Uygulamanız varsayılan sağlayıcı kullandıysanız, özel sağlayıcınız ile değiştirin.

1. Kaldırma `Microsoft.AspNetCore.EntityFramework.Identity` NuGet paketi.
1. Depolama sağlayıcısı ayrı bir proje veya paket bulunuyorsa, kendisine bir başvuru ekleyin.
1. Tüm başvuruları değiştirin `Microsoft.AspNetCore.EntityFramework.Identity` kullanarak ad alanı için depolama sağlayıcısının deyimi.
1. İçinde ``ConfigureServices`` yöntemi, değişiklik `AddIdentity` özel türlerinizi kullanılacak yöntemi. Bu amaç için kendi genişletme yöntemleri oluşturabilirsiniz. Bkz: [IdentityServiceCollectionExtensions](https://github.com/aspnet/Identity/blob/rel/1.1.0/src/Microsoft.AspNetCore.Identity/IdentityServiceCollectionExtensions.cs) bir örnek.
1. Roller kullanıyorsanız, güncelleştirme `RoleManager` kullanmak için `RoleStore` sınıfı.
1. Bağlantı dizesi ve kimlik bilgileri için uygulamanızın yapılandırmasını güncelleştirin.

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

- [ASP.NET kimliği için özel depolama sağlayıcıları](https://docs.microsoft.com/aspnet/identity/overview/extensibility/overview-of-custom-storage-providers-for-aspnet-identity)
- [ASP.NET Core kimliği](https://github.com/aspnet/identity) -bu havuzda deposu sağlayıcıları tutulan topluluk bağlantılarını içerir.
