---
uid: identity/overview/extensibility/overview-of-custom-storage-providers-for-aspnet-identity
title: ASP.NET kimliği için özel depolama sağlayıcıları genel bakış | Microsoft Docs
author: tfitzmac
description: ASP.NET kimliği, kendi depolama sağlayıcısı oluşturun ve Uy yeniden çalışma olmadan uygulamanıza takın olanak tanıyan genişletilebilir bir sistemdir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/13/2014
ms.topic: article
ms.assetid: 681a9204-462e-4260-9a0b-19f0644d6ad7
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /identity/overview/extensibility/overview-of-custom-storage-providers-for-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 06e3ad3b74bf94806f56da9f579255bf2917bc48
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="overview-of-custom-storage-providers-for-aspnet-identity"></a>ASP.NET kimliği için özel depolama sağlayıcıları genel bakış
====================
tarafından [zel FitzMacken](https://github.com/tfitzmac)

> ASP.NET kimliği, kendi depolama sağlayıcısı oluşturun ve uygulamayı yeniden çalışma olmadan uygulamanıza takın olanak tanıyan genişletilebilir bir sistemdir. Bu konu, ASP.NET Identity için özelleştirilmiş depolama sağlayıcısı oluşturmayı açıklar. Kendi depolama sağlayıcısı oluşturmak için önemli kavramları içerir, ancak özel depolama sağlayıcısı uygulaması hakkında adım adım kılavuz değil.
> 
> Özel depolama sağlayıcıyı uygulama örneği için bkz: [özel bir MySQL ASP.NET Identity depolama sağlayıcısı uygulama](implementing-a-custom-mysql-aspnet-identity-storage-provider.md).
> 
> Bu konu, ASP.NET Identity 2.0 için güncelleştirilmiştir.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Öğreticide kullanılan yazılım sürümleri
> 
> 
> - Visual Studio 2013 güncelleştirme 2 ile
> - ASP.NET kimliği 2


## <a name="introduction"></a>Giriş

Varsayılan olarak, ASP.NET Identity sistem kullanıcı bilgilerini bir SQL Server veritabanında saklar ve veritabanı oluşturmak için Entity Framework Code First kullanır. Birçok uygulama için bu yaklaşım iyi çalışır. Ancak, farklı türde bir Azure Table Storage gibi Kalıcılık mekanizması kullanmayı tercih edebilirsiniz veya veritabanı tablolarını varsayılan uygulaması daha çok farklı bir yapı ile zaten sahip olabilir. Her iki durumda da, depolama mekanizmasını için özelleştirilmiş bir sağlayıcı yazma ve bu sağlayıcıyı uygulamanıza takın.

ASP.NET Identity birçok Visual Studio 2013 şablonları varsayılan olarak dahil edilir. ASP.NET Identity için güncelleştirmeleri almak [Microsoft ASP.NET Identity EntityFramework NuGet paketi](http://www.nuget.org/packages/Microsoft.AspNet.Identity.EntityFramework/).

Bu konu aşağıdaki bölümleri içermektedir:

- [Mimarisini anlama](#architecture)
- [Depolanan verilerin anlama](#data)
- [Veri erişim katmanı oluşturma](#dal)
- [Kullanıcı sınıfı özelleştirme](#user)
- [Kullanıcı deposunda özelleştirme](#userstore)
- [Rol sınıfı özelleştirme](#role)
- [Rol deposu özelleştirme](#rolestore)
- [Yeni depolama sağlayıcısı kullanmak için uygulamayı yeniden yapılandırın](#reconfigure)
- [Diğer uygulamalardan özel depolama sağlayıcıları](#other)

<a id="architecture"></a>
## <a name="understand-the-architecture"></a>Mimarisini anlama

ASP.NET Identity yöneticileri ve depoları adlı sınıflardan oluşur. Yöneticileri ASP.NET Identity sistem bir kullanıcı oluşturma gibi işlemleri gerçekleştirmek için bir uygulama geliştiricisi kullanan üst düzey sınıflarıdır. Depoları kullanıcılar ve roller gibi varlıkları nasıl kalıcı belirtin düşük düzeyli sınıflarıdır. Depoları ile kalıcılığı mekanizmasının yakından bağlı değildir, ancak yöneticileri depoları tüm uygulama kesintiye uğratmadan Kalıcılık mekanizması değiştirebilirsiniz anlamı birbirinden ayrılır.

Aşağıdaki diyagramda, web uygulaması yöneticilerine nasıl etkileşim kurduğunu gösterir ve depoları ile veri erişim katmanı etkileşim.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image1.png)

ASP.NET kimliği için özelleştirilmiş depolama sağlayıcısı oluşturmak için bu veri erişim katmanı ile veri kaynağı, veri erişim katmanı ve etkileşim mağaza sınıfları oluşturmanız gerekir. Kullanıcının ancak göre verileri farklı depolama sistemi için kaydedilen veri işlemleri gerçekleştirmek için aynı yönetici API'ları kullanmaya devam edebilirsiniz.

UserManager veya RoleManager yeni bir örneğini oluştururken kullanıcı sınıfı tür sağlamak için yönetici sınıfları özelleştirmek ve deposu sınıfının bir örneği bir bağımsız değişken olarak geçirmek gerekmez. Bu yaklaşım, özelleştirilmiş sınıflarınızı mevcut yapıya takın olanak sağlar. UserManager ve RoleManager bölümünde, özelleştirilmiş deposu sınıflarla örneği oluşturmak nasıl göreceksiniz [yeni depolama sağlayıcısı kullanmak için uygulamayı yeniden](#reconfigure).

<a id="data"></a>
## <a name="understand-the-data-that-is-stored"></a>Depolanan verilerin anlama

Özel depolama sağlayıcısını uygulamak için ASP.NET kimliği ile kullanılan veri türlerini anlamak ve hangi özellikleri, uygulamanızla ilgili karar vermeniz gerekir.

| Veri | Açıklama |
| --- | --- |
| Kullanıcılar | Kayıtlı kullanıcıların web sitenizin. Kullanıcı kimliği ve kullanıcı adını içerir. Kullanıcılar, siteye özgü kimlik bilgilerinizle oturum karma parola içerebilir (yerine Facebook gibi dış sitelerden kimlik bilgilerini kullanarak) ve herhangi bir şey kullanıcı kimlik bilgilerini değişip değişmediğini belirtmek için güvenlik damgası. E-posta adresi de dahil, telefon numarası, iki Etmenli kimlik doğrulamasının etkin olup olmadığını oturumları başarısız olan geçerli sayısı ve olup bir hesap kilitlendi. |
| Kullanıcı talepleri | Kullanıcının kimliğini temsil eder kullanıcı hakkındaki deyimleri (veya talep) kümesi. Kullanıcının kimliğini rolleri aracılığıyla elde edilebilir daha büyük ifade etkinleştirebilirsiniz. |
| Kullanıcı oturumu | Dış kimlik doğrulama sağlayıcısı (gibi Facebook) hakkında bilgi bir kullanıcı oturum açarken kullanılacak. |
| Roller | Siteniz için yetkilendirme grupları. Rol Kimliği ve rol adı (ör. "Yönetici" veya "Çalışan") içerir. |

<a id="dal"></a>
## <a name="create-the-data-access-layer"></a>Veri erişim katmanı oluşturma

Bu konuda, kullanacaksanız kalıcılığı mekanizmasının ve o mekanizması için varlıkların nasıl oluşturulacağı hakkında bilgi sahibi olduğunuz varsayılır. Bu konuda depoları veya veri erişim sınıfları oluşturmak nasıl kullanılacağı hakkındaki ayrıntıları sağlamaz; Bunun yerine, ASP.NET kimliği ile çalışırken vermeniz gereken tasarım kararları hakkında bazı öneriler sağlar.

Sağlayıcı depolamak için özelleştirilmiş depoları tasarlarken özgürlük birçok var. Yalnızca, uygulamanızda kullanmak istediğiniz özellikleri için depoları oluşturmanız gerekir. Örneğin, uygulamanızda rolleri kullanmıyorsanız, roller veya kullanıcı rolleri için depolama alanı oluşturmak gerekmez. Teknoloji ve varolan altyapı ASP.NET Identity varsayılan uygulamasından çok farklı bir yapı gerektirebilir. Veri erişim katmanı'nızda, depoları yapısı ile çalışmak için mantığı sağlar.

Veri depoları ASP.NET Identity 2.0 için bir MySQL düzeyi için bkz: [MySQLIdentity.sql](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLIdentity.sql).

Veri erişim katmanı verileri ASP.NET kimlikten veri kaynağınıza kaydetmek için mantığı sağlar. Veri erişim katmanı, özelleştirilmiş depolama sağlayıcısı için kullanıcı ve rol bilgilerini depolamak için aşağıdaki sınıflar içerebilir.

| örneği | Açıklama | Örnek |
| --- | --- | --- |
| Bağlam | İçin Kalıcılık mekanizması bağlanmak ve sorguları yürütmek için bilgileri yalıtır. Bu sınıf, veri erişim katmanı taşımaktadır. Diğer veri sınıflarını işlemlerini gerçekleştirmek için bu sınıfının bir örneğini gerektirir. Ayrıca, bu sınıfın örneğini deposu sınıflarıyla başlatacak. | [MySQLDatabase](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLDatabase.cs) |
| Kullanıcı depolama | Kullanıcı bilgilerini (örneğin, kullanıcı adı ve parola karması) alır ve depolar. | [UserTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserTable.cs) |
| Rol depolama | Rol bilgilerini (örneğin, rol adı) alır ve depolar. | [RoleTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleTable.cs) |
| UserClaims depolama | Kullanıcı talebi bilgileri (örneğin, talep türü ve değeri) alır ve depolar. | [UserClaimsTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserClaimsTable.cs) |
| UserLogins depolama | Kullanıcı oturum açma bilgilerini (örneğin, bir dış kimlik doğrulama sağlayıcısı) alır ve depolar. | [UserLoginsTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserLoginsTable.cs) |
| UserRole depolama | Bir kullanıcı atandığı hangi rollerin alır ve depolar. | [UserRoleTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserRoleTable.cs) |

Yeniden, uygulamanızda kullanmak istediğiniz sınıfları uygulamak yeterlidir.

Veri erişim sınıfları, belirli Kalıcılık mekanizması için veri işlemleri gerçekleştirmek için kod sağlar. Örneğin, MySQL uygulama içinde yeni bir kayıt kullanıcıları veritabanı tablosuna eklemek için bir yöntem UserTable sınıfı içerir. Adlı değişken `_database` MySQLDatabase sınıfının bir örneğidir.

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample1.cs)]

Veri erişim sınıfları oluşturduktan sonra veri erişim katmanı'ndaki belirli yöntemlerini çağıran mağaza sınıfları oluşturmanız gerekir.

<a id="user"></a>
## <a name="customize-the-user-class"></a>Kullanıcı sınıfı özelleştirme

Kendi depolama sağlayıcısı uygularken, eşdeğer olan bir kullanıcı sınıfı oluşturmalısınız [IdentityUser](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework.identityuser(v=vs.108).aspx) sınıfını [Microsoft.ASP.NET.Identity.EntityFramework](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework(v=vs.108).aspx) ad alanı:

Aşağıdaki diyagramda, oluşturmalısınız IdentityUser sınıfı ve bu sınıfta uygulamak için arabirimi gösterir.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image2.png)

[Iuser&lt;TKey&gt; ](https://msdn.microsoft.com/library/dn613291(v=vs.108).aspx) arabirimi UserManager gerçekleştirme işlemleri istendiğinde arama girişiminde özellikleri tanımlar. Arabirimi iki özellikleri - kimliği ve kullanıcı adını içerir. [Iuser&lt;TKey&gt; ](https://msdn.microsoft.com/library/dn613291(v=vs.108).aspx) arabirimi genel aracılığıyla kullanıcı için anahtar türünü belirtmenize olanak sağlar **TKey** parametresi. ID özelliği TKey parametresinin değeri ile eşleşen.

Identity framework de sağlar [Iuser](https://msdn.microsoft.com/library/microsoft.aspnet.identity.iuser(v=vs.108).aspx) arabirimi (olmadan genel parametresini) anahtar için bir dize değerini kullanmak istediğinizde.

IdentityUser sınıf Iuser uygulayan ve herhangi bir ek özellikler veya oluşturucuları için web sitenizde kullanıcıları içerir. Aşağıdaki örnek, bir tamsayı anahtarı kullanan bir IdentityUser sınıfı gösterir. ID alanı ayarlamak **int** genel parametresini değer ile eşleşmelidir. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample2.cs)]

 Tam bir uygulama için bkz: [IdentityUser (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityUser.cs). 

<a id="userstore"></a>
## <a name="customize-the-user-store"></a>Kullanıcı deposunda özelleştirme

Kullanıcının tüm veri işlemleri için yöntemler sağlayan bir UserStore sınıfı oluşturabilir. Bu sınıf eşdeğerdir [UserStore&lt;TUser&gt; ](https://msdn.microsoft.com/library/dn315446(v=vs.108).aspx) sınıfını [Microsoft.ASP.NET.Identity.EntityFramework](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework(v=vs.108).aspx) ad alanı. UserStore sınıfınızda uygulamanız [Iuserstore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613276(v=vs.108).aspx) ve isteğe bağlı arabirimler. Uygulamanızda sağlamak istediğiniz işlevselliği temel uygulamak için hangi isteğe bağlı arabirimler seçin.

Aşağıdaki resimde oluşturmanız gerekir UserStore sınıfı ile ilgili arabirimler gösterilir.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image3.png)

Visual Studio'da varsayılan proje şablonu isteğe bağlı arabirimler birçoğu kullanıcı deposunda uygulanmıştır varsayar kodunu içerir. Varsayılan şablonu özelleştirilmiş kullanıcı deposuyla kullanıyorsanız, isteğe bağlı arabirimler, kullanıcı deposunda uygulamak veya artık uygulanmadı arabirimler yöntemleri çağırmak için şablon kodu değiştirmeniz gerekir.

 Aşağıdaki örnek, bir basit kullanıcı deposu sınıfı gösterir. **TUser** genel parametre genellikle tanımladığınız IdentityUser sınıf olan kullanıcı sınıfınıza türünü alır. **TKey** genel parametre kullanıcı anahtarınızı türünü alır. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample3.cs)]

 Bu örnekte, bir parametre alan oluşturucu adlı *veritabanı* ExampleDatabase veri erişim sınıfınızda geçirmek nasıl yalnızca bir çizimi türüdür. Örneğin, MySQL uygulamasında bu oluşturucuyu MySQLDatabase türünde bir parametre alır. 

UserStore sınıfınız işlemlerini gerçekleştirmek için oluşturulan veri erişim sınıfları kullanın. Örneğin, MySQL uygulamasında UserStore sınıfının yeni bir kayıt eklemek için UserTable örneği kullanan CreateAsync yöntemi vardır. **Ekle** yöntemi **userTable** önceki bölümde gösterilen aynı yöntemi bir nesnedir. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample4.cs)]

### <a name="interfaces-to-implement-when-customizing-user-store"></a>Kullanıcı deposu özelleştirirken uygulamak için arabirimleri

Sonraki resmi her arabiriminde tanımlanan işlevselliği hakkında daha fazla ayrıntı gösterir. Tüm isteğe bağlı arabirimler Iuserstore devralır.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image4.png)

- **IUserStore**  
  [Iuserstore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613278(v=vs.108).aspx) kullanıcı deponuzda uygulanmalı yalnızca arabirimi bir arabirimdir. Oluşturma, güncelleştirme, silme ve kullanıcıları almak için yöntemleri tanımlar.
- **IUserClaimStore**  
  [Iuserclaimstore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613265(v=vs.108).aspx) arabirimi tanımlar yöntemleri kullanıcı talepleri etkinleştirmek için kullanıcı deposunda uygulamalıdır. Yöntemleri veya ekleme, kaldırma ve kullanıcı taleplerini alma içerir.
- **IUserLoginStore**  
  [Iuserloginstore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613272(v=vs.108).aspx) yöntemleri tanımlar Dış kimlik doğrulama sağlayıcıları etkinleştirmek için kullanıcı deposunda uygulamalıdır. Ekleme, kaldırma ve kullanıcı oturum açma ve oturum açma bilgilerine dayalı bir kullanıcı almak için bir yöntem almak için yöntemler içerir.
- **IUserRoleStore**  
  [Iuserrolestore&lt;TKey, TUser&gt; ](https://msdn.microsoft.com/library/dn613276(v=vs.108).aspx) arabirim yöntemleri tanımlayan bir kullanıcı rolü için eşlemek için kullanıcı deposunda uygulamalıdır. Eklemek, kaldırmak ve bir kullanıcının rollerini ve bir kullanıcı bir role atanmış olmadığını denetlemek için bir yöntem almak için yöntemler içerir.
- **Iuserpasswordstore**  
  [Iuserpasswordstore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613273(v=vs.108).aspx) arabirimi tanımlar kalıcı hale getirmek için kullanıcı deposunda uygulanmalı yöntemleri parolaların karma. Karma hale getirilen parola ve kullanıcı bir parola ayarlanmış olup olmadığını belirten bir yöntem ayarlama ve alma için yöntemler içerir.
- **IUserSecurityStampStore**  
  [IUserSecurityStampStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613277(v=vs.108).aspx) arabirimi tanımlar kullanıcının hesap bilgilerini değişip değişmediğini belirten bir güvenlik damgası kullanmak için kullanıcı deposunda uygulanmalı yöntemleri . Bir kullanıcı parolasını değiştirir veya ekler veya oturum açma bilgileri kaldırdığında bu damga güncelleştirilir. Kullanıcının güvenlik damgasını ayarlama ve alma için yöntemler içerir.
- **IUserTwoFactorStore**  
  [Iusertwofactorstore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613279(v=vs.108).aspx) arabirimi iki faktörlü kimlik doğrulaması uygulama için uygulama gereken yöntemleri tanımlar. Bir kullanıcı için iki Etmenli kimlik doğrulamasının etkin olup olmadığını ayarlama ve alma için yöntemler içerir.
- **IUserPhoneNumberStore**  
  [Iuserphonenumberstore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613275(v=vs.108).aspx) arabirimi kullanıcı telefon numaralarını depolamak için uygulanmalı yöntemleri tanımlar. Telefon numarası ve olup telefon numarasının onaylanıp ayarlama ve alma için yöntemler içerir.
- **IUserEmailStore**  
  [Iuseremailstore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613143(v=vs.108).aspx) arabirimi kullanıcı e-posta adreslerini depolamak için uygulanmalı yöntemleri tanımlar. E-posta adresi ve e-posta olup onaylandıktan ayarlama ve alma için yöntemler içerir.
- **IUserLockoutStore**  
  [Iuserlockoutstore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613271(v=vs.108).aspx) arabirimi bir hesap kilitleme hakkında bilgi depolamak için uygulanmalı yöntemleri tanımlar. Başarısız erişim denemelerinin geçerli sayısını alma, alma ve alma hesabı kilitlenebilir ve bitiş tarihi kilitleme ayarı sayısını artırma denemeleri başarısız olup olmadığını ayarlama ve başarısız girişim sayısı sıfırlamak için yöntemler içerir.
- **IQueryableUserStore**  
  [Iqueryableuserstore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613267(v=vs.108).aspx) arabirimi sorgulanabilir bir kullanıcı deposunun sağlamak için uygulanmalı üyeleri tanımlar. Sorgulanabilir kullanıcılar tutan bir özellik içerir.

  Gerekli olan arabirimler uygulamanızda uygulamak; gibi Iuserclaimstore, Iuserloginstore, Iuserrolestore, Iuserpasswordstore ve IUserSecurityStampStore arabirimleri aşağıda gösterildiği gibi. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample5.cs)]

(Tüm arabirimleri dahil) tam bir uygulama için bkz: [UserStore (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserStore.cs).

### <a name="identityuserclaim-identityuserlogin-and-identityuserrole"></a>IdentityUserClaim, IdentityUserLogin ve IdentityUserRole

Microsoft.ASPNET.Identity.entityframework ad alanı, uygulamalarını içeren [IdentityUserClaim](https://msdn.microsoft.com/library/dn613250(v=vs.108).aspx), [IdentityUserLogin](https://msdn.microsoft.com/library/dn613251(v=vs.108).aspx), ve [IdentityUserRole](https://msdn.microsoft.com/library/dn613252(v=vs.108).aspx) sınıflar. Bu özellikler kullanıyorsanız, bu sınıfları, kendi sürümlerini oluşturabilir ve uygulamanızı özelliklerini tanımlamak isteyebilirsiniz. Ancak, bazen bu varlıkları belleğe (ekleme veya kullanıcının talep kaldırma gibi) temel işlemleri gerçekleştirirken yüklememeye daha etkilidir. Bunun yerine, arka uç mağaza sınıfları bu işlemleri doğrudan veri kaynağına yürütebilir. Örneğin, userClaimTable.FindByUserId(user. UserStore.GetClaimsAsync() yöntemi çağırabilirsiniz Talepler listesinin doğrudan tablo ve bir sorgu yürütmek için kimliği) yöntemi.

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample6.cs)]

<a id="role"></a>
## <a name="customize-the-role-class"></a>Rol sınıfı özelleştirme

Kendi depolama sağlayıcısı uygularken, eşdeğer olan rol sınıfı oluşturmalısınız [IdentityRole](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework.identityrole(v=vs.108).aspx) sınıfını [Microsoft.ASP.NET.Identity.EntityFramework](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework(v=vs.108).aspx) ad alanı:

Aşağıdaki diyagramda, oluşturmalısınız IdentityRole sınıfı ve bu sınıfta uygulamak için arabirimi gösterir.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image5.png)

[IRole&lt;TKey&gt; ](https://msdn.microsoft.com/library/dn613268(v=vs.108).aspx) arabirimi RoleManager gerçekleştirme işlemleri istendiğinde arama girişiminde özellikleri tanımlar. Arabirimi iki özellikleri - kimliği ve adını içerir. [IRole&lt;TKey&gt; ](https://msdn.microsoft.com/library/dn613268(v=vs.108).aspx) arabirimi genel aracılığıyla rolü için anahtar türü belirtmenize olanak sağlar **TKey** parametresi. ID özelliği TKey parametresinin değeri ile eşleşen.

Identity framework de sağlar [IRole](https://msdn.microsoft.com/library/microsoft.aspnet.identity.irole(v=vs.108).aspx) arabirimi (olmadan genel parametresini) anahtar için bir dize değerini kullanmak istediğinizde.

Aşağıdaki örnek, bir tamsayı anahtarı kullanan bir IdentityRole sınıfı gösterir. ID alanı int'e genel parametresinin değeri eşleşecek şekilde ayarlanır. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample7.cs)]

 Tam bir uygulama için bkz: [IdentityRole (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityRole.cs). 

<a id="rolestore"></a>
## <a name="customize-the-role-store"></a>Rol deposu özelleştirme

Tüm veri işlemleri rolleri için yöntemler sağlar RoleStore sınıfı oluşturabilir. Bu sınıf eşdeğerdir [RoleStore&lt;TRole&gt; ](https://msdn.microsoft.com/library/dn468181(v=vs.108).aspx) Microsoft.ASP.NET.Identity.EntityFramework ad alanında sınıfı. RoleStore sınıfınızda uygulamanız [Irolestore&lt;TRole, TKey&gt; ](https://msdn.microsoft.com/library/dn613266(v=vs.108).aspx) ve isteğe bağlı olarak [Iqueryablerolestore&lt;TRole, TKey&gt; ](https://msdn.microsoft.com/library/dn613262(v=vs.108).aspx) arabirim.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image6.png)

Aşağıdaki örnek, bir rol deposu sınıfı gösterir. TRole genel parametre genellikle tanımladığınız IdentityRole sınıf olan rol sınıfınız türünü alır. TKey genel parametre rol anahtarınızı türünü alır. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample8.cs)]

- **IRoleStore&lt;TRole&gt;**  
  [Irolestore](https://msdn.microsoft.com/library/dn468195.aspx) arabirimi rol deposu sınıfınızda uygulanacak yöntemleri tanımlar. Oluşturma, güncelleştirme, silme ve rolleri almak için yöntemler içerir.
- **RoleStore&lt;TRole&gt;**  
  RoleStore özelleştirmek için Irolestore arabirimini uygulayan bir sınıf oluşturun. Bu sınıf, uygulama yeterlidir, sisteminizde rolleri kullanmak istediğiniz. Adlı bir parametre alan oluşturucu *veritabanı* ExampleDatabase veri erişim sınıfınızda geçirmek nasıl yalnızca bir çizimi türüdür. Örneğin, MySQL uygulamasında bu oluşturucuyu MySQLDatabase türünde bir parametre alır.  
  
  Tam bir uygulama için bkz: [RoleStore (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleStore.cs) .

<a id="reconfigure"></a>
## <a name="reconfigure-application-to-use-new-storage-provider"></a>Yeni depolama sağlayıcısı kullanmak için uygulamayı yeniden yapılandırın

Yeni depolama alanı sağlayıcınıza uyguladık. Artık, uygulamanızı bu depolama sağlayıcısı kullanacak şekilde yapılandırmanız gerekir. Varsayılan depolama sağlayıcısı projenizde içeriyorsa, varsayılan sağlayıcı kaldırın ve sağlayıcınız ile değiştirin.

### <a name="replace-default-storage-provider-in-mvc-project"></a>Varsayılan depolama sağlayıcısı MVC projesindeki değiştirin

1. İçinde **NuGet paketlerini Yönet** penceresinde kaldırmak **Microsoft ASP.NET Identity EntityFramework** paket. Bu paket için Identity.EntityFramework yüklü paketler arayarak bulabilirsiniz.  
    ![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image7.png) Ayrıca Entity Framework kaldırmak istiyorsanız istenir. Uygulamanızın diğer bölümlerinde, gerekmiyorsa kaldırabilirsiniz.
2. Modeller klasörü IdentityModels.cs dosyasında silin veya çıkışı açıklama **ApplicationUser** ve **ApplicationDbContext** sınıfları. Bir MVC uygulamasındaki tüm IdentityModels.cs dosyayı silebilirsiniz. Web Forms uygulamasında, iki sınıf silmek ancak aynı zamanda IdentityModels.cs dosyada bulunan yardımcı sınıfı tutmak emin olun.
3. Depolama sağlayıcısı ayrı bir projede bulunuyorsa, web uygulamanızda kendisine bir başvuru ekleyin.
4. Tüm başvuruları değiştirin `using Microsoft.AspNet.Identity.EntityFramework;` kullanarak ad alanı için depolama sağlayıcısının deyimi.
5. İçinde **Startup.Auth.cs** sınıfı, değişiklik **ConfigureAuth** uygun bağlamı tek bir örneğini kullanmak için yöntem. 

    [!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample9.cs?highlight=3)]
6. Uygulamasında\_başlangıç klasörü, açık **IdentityConfig.cs**. ApplicationUserManager sınıfında değiştirme **oluşturma** özelleştirilmiş kullanıcı deponuza kullanan bir kullanıcı Yöneticisi döndürülecek yöntemi. 

    [!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample10.cs?highlight=3)]
7. Tüm başvuruları değiştirin **ApplicationUser** ile **IdentityUser**.
8. Varsayılan proje bazı üyeler Iuser arabiriminde tanımlı değil kullanıcı sınıfı içerir; e-posta, PasswordHash ve GenerateUserIdentityAsync gibi. Kullanıcı sınıfınıza bu üyeleri yoksa uygulamadan veya bu üyeleri kullanan kodu değiştirmeniz gerekir.
9. RoleManager'ın tüm örneklerini oluşturduysanız, bu kodu yeni RoleStore sınıfınızın kullanacak şekilde değiştirin.  

    [!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample11.cs)]
10. Varsayılan proje anahtarı için bir dize değerine sahip bir kullanıcı sınıfı için tasarlanmıştır. Kullanıcı sınıfınıza (örneğin, bir tamsayı) anahtarı için farklı bir tür varsa, proje türü ile çalışmayı değiştirmeniz gerekir. Bkz: [değiştirmek ASP.NET Identity kullanıcılar için birincil anahtar](change-primary-key-for-users-in-aspnet-identity.md).
11. Gerekli olursa, bağlantı dizesi Web.config dosyasına ekleyin.

<a id="other"></a>
## <a name="other-resources"></a>Diğer kaynaklar

- Blog: [ASP.NET Identity uygulama](http://odetocode.com/blogs/scott/archive/2014/01/20/implementing-asp-net-identity.aspx)
- Öğretici ve GIT kodu: [Simple.Data Asp.Net kimlik sağlayıcısı](http://designcoderelease.blogspot.co.uk/2015/03/simpledata-aspnet-identity-provider.html)
- Öğretici:[temel kimlik hesapları ayarlama ve bunları dış bir DB işaret](http://typecastexception.com/post/2013/10/27/Configuring-Db-Connection-and-Code-First-Migration-for-Identity-Accounts-in-ASPNET-MVC-5-and-Visual-Studio-2013.aspx). Tarafından [ @xivSolutions ](https://twitter.com/xivSolutions).
- Öğretici[: özel MySQL ASP.NET Identity depolama sağlayıcıyı uygulama](implementing-a-custom-mysql-aspnet-identity-storage-provider.md)
- [CodeFluent varlıklar](http://blog.codefluententities.com/2014/04/30/asp-net-identity-v2-and-codefluent-entities/) tarafından [SoftFluent](http://www.softfluent.com/)
- [Azure Table Storage](https://www.nuget.org/packages/accidentalfish.aspnet.identity.azure/) Ahmet CAN tarafından.
- Azure Table Storage: [AspNet.Identity.TableStorage](https://github.com/stuartleeks/leeksnet.AspNet.Identity.TableStorage) tarafından [ @stuartleeks ](https://twitter.com/stuartleeks).
- [CouchDB / Cloudant Daniel Wertheim tarafından.](https://github.com/danielwertheim/mycouch.aspnet.identity)
- Esnek arama[y: esnek kimlik](https://github.com/bmbsqd/elastic-identity) Bombsquad AB. tarafından
- [MongoDB](http://www.nuget.org/packages/MongoDB.AspNet.Identity/) Jonathan Sheely Jonathan Sheely tarafından.
- [NHibernate.AspNet.Identity](https://github.com/milesibastos/NHibernate.AspNet.Identity) by Antônio Milesi Bastos.
- [RavenDB](http://www.nuget.org/packages/AspNet.Identity.RavenDB/1.0.0) tarafından [ @tourismgeek ](https://twitter.com/tourismgeek).
- [RavenDB.AspNet.Identity](https://github.com/ILMServices/RavenDB.AspNet.Identity) by [ILMServices](http://www.ilmservice.com/).
- Redis: [Redis.AspNet.Identity](https://github.com/aminjam/Redis.AspNet.Identity)
- Kod için "ilk veritabanı" kullanıcı deposunu T4 EF oluşturmak için şablonları: [AspNet.Identity.EntityFramework](https://github.com/cbfrank/AspNet.Identity.EntityFramework)
