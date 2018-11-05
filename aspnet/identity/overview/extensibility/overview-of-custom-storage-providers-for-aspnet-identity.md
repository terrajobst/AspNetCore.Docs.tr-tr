---
uid: identity/overview/extensibility/overview-of-custom-storage-providers-for-aspnet-identity
title: ASP.NET kimliği için özel depolama sağlayıcıları genel bakış | Microsoft Docs
author: Rick-Anderson
description: ASP.NET Identity Uy yeniden çalışma olmadan uygulamanıza eklenir ve kendi depolama sağlayıcısı oluşturma olanak tanıyan genişletilebilir bir sistemdir...
ms.author: riande
ms.date: 10/13/2014
ms.assetid: 681a9204-462e-4260-9a0b-19f0644d6ad7
msc.legacyurl: /identity/overview/extensibility/overview-of-custom-storage-providers-for-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: e7461098f93bf64d6ff0d0e4ecdb64338f96be8b
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021774"
---
<a name="overview-of-custom-storage-providers-for-aspnet-identity"></a>ASP.NET Identity için Özel Depolama Sağlayıcılarına Genel Bakış
====================
tarafından [Tom FitzMacken](https://github.com/tfitzmac)

> ASP.NET Identity kendi depolama sağlayıcısı oluşturursanız ve uygulamayı yeniden çalışma olmadan uygulamanıza eklenir olanak tanıyan genişletilebilir bir sistemdir. Bu konuda, ASP.NET kimliği için özelleştirilmiş depolama sağlayıcıyı oluşturmayı açıklar. Kendi depolama sağlayıcısı oluşturmak için önemli kavramları kapsar, ancak bir özel depolama sağlayıcısı uygulama adım adım kılavuz değildir.
> 
> Özel depolama sağlayıcısı uygulama örneği için bkz: [bir özel MySQL ASP.NET Identity depolama sağlayıcısı uygulama](implementing-a-custom-mysql-aspnet-identity-storage-provider.md).
> 
> Bu konuda, ASP.NET Identity 2.0 için güncelleştirildi.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Bu öğreticide kullanılan yazılım sürümleri
> 
> 
> - Güncelleştirme 2 ile Visual Studio 2013
> - ASP.NET Identity 2


## <a name="introduction"></a>Giriş

ASP.NET kimlik sistemi, varsayılan olarak, kullanıcı bilgilerini bir SQL Server veritabanında depolar ve veritabanını oluşturmak için Entity Framework Code First'ı kullanır. Birçok uygulama için bu yaklaşım işe yarar. Ancak, farklı türde bir Azure tablo depolama gibi Kalıcılık mekanizması kullanmayı tercih edebilirsiniz veya varsayılan uygulama çok farklı bir yapıda veritabanı tablolarıyla zaten olabilir. Her iki durumda da, depolama mekanizması için özelleştirilmiş bir sağlayıcı yazma ve o sağlayıcı uygulamanıza eklenir.

ASP.NET Identity varsayılan olarak birçok Visual Studio 2013 dahil edilir. ASP.NET ıdentity'ye güncelleştirmeleri almak [Microsoft AspNet kimliği EntityFramework NuGet paketini](http://www.nuget.org/packages/Microsoft.AspNet.Identity.EntityFramework/).

Bu konu aşağıdaki bölümleri içermektedir:

- [Mimarisini anlama](#architecture)
- [Depolanan verilerin anlama](#data)
- [Veri erişim katmanını oluşturma](#dal)
- [Kullanıcı sınıfı özelleştirme](#user)
- [Kullanıcı deposu özelleştirme](#userstore)
- [Rol sınıfı özelleştirme](#role)
- [Rol deposu özelleştirme](#rolestore)
- [Uygulama yeni depolaması sağlayıcısı kullanacak şekilde yeniden yapılandırın](#reconfigure)
- [Diğer uygulamaları özel depolama sağlayıcıları](#other)

<a id="architecture"></a>
## <a name="understand-the-architecture"></a>Mimarisini anlama

ASP.NET Identity yöneticileri ve depoları adlı sınıftan oluşur. Yöneticiler bir kullanıcı ASP.NET kimlik sistemi oluşturma gibi işlemleri gerçekleştirmek için bir uygulama geliştiricisi kullanan üst düzey sınıflardır. Kullanıcılar ve roller gibi varlıkları nasıl kalıcı belirtin alt düzey sınıflar depolarıdır. Depoları yakından Kalıcılık mekanizması ile bağlı, ancak yöneticileri mağazalardan Kalıcılık mekanizması uygulamanın tamamı kesintiye uğratmadan değiştirebileceğiniz anlamına gelir birbirinden ayrılmıştır.

Aşağıdaki diyagramda, web uygulamanızın yöneticileri ile nasıl etkileşim kurduğu gösterilmektedir ve depolarının veri erişim katmanı ile etkileşim.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image1.png)

ASP.NET Identity bir özelleştirilmiş depolama sağlayıcısı oluşturmak için veri kaynağı, veri erişim katmanı ve etkileşim depolama sınıfları ile bu veri erişim katmanı oluşturmanız gerekir. Artık, verileri farklı bir depolama sistemine kaydedilir ancak kullanıcı veri işlemleri gerçekleştirmek için aynı manager API'lerini kullanmaya devam edebilirsiniz.

UserManager veya RoleManager yeni bir örneğini oluştururken kullanıcı sınıf türü sağladığından manager sınıfları özelleştirme ve depolama sınıfının bir örneği bir bağımsız değişken olarak geçirin gerekmez. Bu yaklaşım, özelleştirilmiş sınıflarınızı mevcut yapıya takın sağlar. Bölümünde, özelleştirilmiş depolama sınıfına sahip UserManager ve RoleManager örneğini oluşturma konusunda görürsünüz [yeni depolama sağlayıcısı kullanmak için uygulamayı yeniden](#reconfigure).

<a id="data"></a>
## <a name="understand-the-data-that-is-stored"></a>Depolanan verilerin anlama

Bir özel depolama sağlayıcısını uygulamak için ASP.NET Identity ile kullanılan veri türlerini anlamak ve hangi özellikleri, uygulamanızla ilgili karar verin.

| Veri | Açıklama |
| --- | --- |
| Kullanıcılar | Kayıtlı kullanıcıların web sitenizin. Kullanıcı kimliği ve kullanıcı adını içerir. Sitenize belirli kimlik bilgileriyle kullanıcıların oturum açarsanız, bir karma hale getirilen parolayla içerebilir (yerine Facebook gibi dış sitelerden kimlik bilgilerini kullanarak) ve her şeyi kullanıcı kimlik bilgilerini değiştirilip değiştirilmediğini göstermek için bir güvenlik damgası. E-posta adresi de dahil, telefon numarası, geçerli iki Etmenli kimlik doğrulamasının etkin olup olmadığını oturumları başarısız ve bir hesap olup kilitlendi. |
| Kullanıcı talepleri | Kullanıcının kimliğini temsil eden kullanıcı hakkında deyimleri (veya talep) kümesi. Greater ifadesini daha rolleri sağlanabilir kullanıcının kimliğinin etkinleştirebilirsiniz. |
| Kullanıcı oturumu açma | Dış kimlik doğrulama sağlayıcısı (gibi Facebook) hakkında bilgi bir kullanıcı oturum açarken kullanılacak. |
| Rolleri | Siteniz için yetkilendirme gruplar. Rol Kimliği ve rol adı (ör. "Yönetici" veya "Employee") içerir. |

<a id="dal"></a>
## <a name="create-the-data-access-layer"></a>Veri erişim katmanını oluşturma

Bu konuda, kullanacaksanız Kalıcılık mekanizması ve bu mekanizma için varlıklar oluşturma hakkında bilgi sahibi olduğunuz varsayılır. Bu konuda depoları veya veri erişim sınıfları oluşturma hakkında ayrıntılı bilgi sağlamaz; Bunun yerine, ASP.NET Identity ile çalışırken, yapmanız gereken tasarım kararlarını ilgili bazı öneriler sağlar.

Sağlayıcı depolamak için özelleştirilmiş depoları tasarlarken özgürlük birçok var. Uygulamanızda kullanmak istediğiniz özellikler için depoları oluşturmanız yeterlidir. Örneğin, uygulamanızda rolleri kullanmıyorsanız, roller veya kullanıcı rolleri için depolama oluşturma gerekmez. ASP.NET Identity varsayılan uygulamasından çok farklı bir yapı, teknoloji ve mevcut altyapınızı gerektirebilir. Veri erişim katmanındaki depolarınızın yapısı ile çalışmak için mantığı sağlar.

ASP.NET Identity 2.0 için veri depoları için bir MySQL düzeyi bkz [MySQLIdentity.sql](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLIdentity.sql).

Veri erişim katmanında veri kaynağınıza ASP.NET Identity verileri kaydetmek için mantığı sağlar. Veri erişim katmanı, özelleştirilmiş depolama sağlayıcısı için kullanıcı ve rol bilgilerini depolamak için aşağıdaki sınıfları içerebilir.

| örneği | Açıklama | Örnek |
| --- | --- | --- |
| Bağlam | Kalıcılık mekanizmanızı bağlanmak ve sorgu yürütmek için bilgileri yalıtır. Bu sınıf, veri erişim katmanı için önemlidir. Diğer veri sınıfları işlemlerini gerçekleştirmek için bu sınıfın bir örneğini gerektirir. Ayrıca, depolama sınıfları bu sınıfın bir örneği ile başlatılır. | [MySQLDatabase](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLDatabase.cs) |
| Kullanıcı depolama | Kullanıcı bilgilerini (örneğin, kullanıcı adı ve parola karması) alır ve depolar. | [UserTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserTable.cs) |
| Rol depolama | Rol bilgilerini (örneğin, rol adı) alır ve depolar. | [RoleTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleTable.cs) |
| UserClaims depolama | Kullanıcı talebi bilgilerini (örneğin, talep türü ve değeri) alır ve depolar. | [UserClaimsTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserClaimsTable.cs) |
| UserLogins depolama | Kullanıcı oturum açma bilgilerini (örneğin, bir dış kimlik doğrulama sağlayıcısı) alır ve depolar. | [UserLoginsTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserLoginsTable.cs) |
| UserRole depolama | Bir kullanıcıya atanan rollerle alır ve depolar. | [UserRoleTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserRoleTable.cs) |

Yine, uygulamanızda kullanmayı planladığınız sınıfları uygulamak yeterlidir.

Veri erişim sınıfları ', belirli bir Kalıcılık mekanizması için veri işlemleri gerçekleştirmek için kod sağlar. Örneğin, MySQL uygulaması içinde kullanıcılar veritabanı tablosunun yeni bir kayıt eklemek için bir yöntem UserTable sınıfı içerir. Adlı değişken `_database` MySQLDatabase sınıfının bir örneğidir.

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample1.cs)]

Veri erişim sınıfları oluşturduktan sonra veri erişim katmanında belirli yöntemleri çağıran deposu sınıflar oluşturmanız gerekir.

<a id="user"></a>
## <a name="customize-the-user-class"></a>Kullanıcı sınıfı özelleştirme

Kendi depolama sağlayıcısını uygularken değerine eşdeğer olan bir kullanıcı sınıfı oluşturmalısınız [IdentityUser](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework.identityuser(v=vs.108).aspx) sınıfını [Microsoft.ASP.NET.Identity.EntityFramework](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework(v=vs.108).aspx) ad alanı:

Aşağıdaki diyagramda, oluşturmanız gereken IdentityUser sınıfı ve bu sınıfta gerçekleştirmek için arabirimi gösterir.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image2.png)

[Iuser&lt;TKey&gt; ](https://msdn.microsoft.com/library/dn613291(v=vs.108).aspx) arabirimi UserManager istenen işlem gerçekleştiriliyor aramanızı dener özellikleri tanımlar. Arabirim, iki özellik - kimliği ve kullanıcı adını içerir. [Iuser&lt;TKey&gt; ](https://msdn.microsoft.com/library/dn613291(v=vs.108).aspx) arabirimi aracılığıyla genel kullanıcı için anahtar türünü belirtmenize imkan tanır **TKey** parametresi. ID özelliği türü ile eşleşen TKey parametrenin değeri.

Ayrıca kimlik çerçevesi sağlar [Iuser](https://msdn.microsoft.com/library/microsoft.aspnet.identity.iuser(v=vs.108).aspx) arabirimi (genel parametre) olmadan anahtar için bir dize değerini kullanmak istediğinizde.

IdentityUser sınıfı Iuser uygulayan ve herhangi bir ek özellikler veya web sitenizde kullanıcıları için oluşturucular içeriyor. Aşağıdaki örnek, bir tamsayı anahtarı kullanan bir IdentityUser sınıfı gösterir. Kimliği alanı **int** genel parametresinin değer ile eşleşmelidir. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample2.cs)]

 Tam bir uygulama için bkz: [IdentityUser (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityUser.cs). 

<a id="userstore"></a>
## <a name="customize-the-user-store"></a>Kullanıcı deposu özelleştirme

Kullanıcının tüm veri işlemleri için yöntemleri sağlayan UserStore sınıf oluşturabilir. Bu sınıf eşdeğerdir [UserStore&lt;TUser&gt; ](https://msdn.microsoft.com/library/dn315446(v=vs.108).aspx) sınıfını [Microsoft.ASP.NET.Identity.EntityFramework](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework(v=vs.108).aspx) ad alanı. UserStore sınıfınızda, uygulamanız [Iuserstore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613276(v=vs.108).aspx) ve isteğe bağlı arabirimler. Hangi isteğe bağlı bir arabirim uygulamak için uygulamanızda sağlamak istiyorsanız işlevselliğini göre seçin.

Aşağıdaki görüntüde UserStore sınıfı oluşturmanız gerekir ve ilgili arabirimleri gösterilmektedir.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image3.png)

Visual Studio'da varsayılan proje şablonu, isteğe bağlı arabirimlerin çoğu kullanıcı deposunda uygulanmıştır varsayan kod içerir. Bir özelleştirilmiş kullanıcı deposu ile varsayılan şablonu kullanıyorsanız, isteğe bağlı arabirimler kullanıcı deponuzda uygulamak veya artık uygulanmadı arabirimlerde yöntemleri çağırmak için şablon kodunu alter gerekir.

 Aşağıdaki örnek, basit bir kullanıcı deposu sınıfı gösterir. **TUser** genel parametre, genellikle tanımladığınız IdentityUser sınıfı olan kullanıcı sınıfınıza türünü alır. **TKey** genel parametre kullanıcı anahtarınızı türünü alır. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample3.cs)]

 Bu örnekte, bir parametre olarak alan oluşturucu adlı *veritabanı* ExampleDatabase nasıl geçirileceğini veri erişim Sınıfınız içinde yalnızca bir gösterimi türüdür. Örneğin, MySQL uygulamasında, bu Oluşturucusu MySQLDatabase türünde bir parametre alır. 

UserStore Sınıfınız içinde işlemleri gerçekleştirmek için oluşturduğunuz veri erişim sınıfları kullanın. Örneğin, MySQL uygulamasında UserStore sınıfın yeni bir kayıt eklemek için UserTable örneğini kullanan CreateAsync yöntemi vardır. **Ekle** metodunda **userTable** nesnedir önceki bölümde gösterilen aynı yöntemi. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample4.cs)]

### <a name="interfaces-to-implement-when-customizing-user-store"></a>Kullanıcı deposu özelleştirirken uygulanacak arabirimleri

Sıradaki resimde, her arabirim içinde tanımlanmış işlevler hakkında daha fazla ayrıntı gösterilmektedir. Tüm isteğe bağlı arabirimler Iuserstore devralır.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image4.png)

- **Iuserstore**  
  [Iuserstore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613278(v=vs.108).aspx) kullanıcı deponuzda uygulanmalı yalnızca arabirim arabirimidir. Bu, oluşturma, güncelleştirme, silme ve kullanıcıları alınırken için yöntemleri tanımlar.
- **Iuserclaimstore**  
  [Iuserclaimstore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613265(v=vs.108).aspx) arabirim yöntemleri tanımlar kullanıcı talepleri etkinleştirmek için kullanıcı deposunda uygulamalıdır. Bu yöntemleri veya ekleme, kaldırma ve kullanıcı taleplerini alma içerir.
- **Iuserloginstore**  
  [Iuserloginstore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613272(v=vs.108).aspx) yöntemlerini Dış kimlik doğrulama sağlayıcıları etkinleştirmek için kullanıcı deposunda uygulamalıdır. Bu, ekleme, kaldırma ve kullanıcı oturum açma bilgileri ve oturum açma bilgilerine dayalı bir kullanıcı almak için bir yöntem almak için yöntemler içerir.
- **Iuserrolestore**  
  [Iuserrolestore&lt;TKey, TUser&gt; ](https://msdn.microsoft.com/library/dn613276(v=vs.108).aspx) arabirim yöntemleri tanımlayan bir role kullanıcı eşlemek için kullanıcı deposunda uygulamalıdır. Bu, ekleme, kaldırma ve bir kullanıcının rollerini ve bir kullanıcı bir role atanmış ise denetlemek için bir yöntem almak için yöntemler içerir.
- **Iuserpasswordstore**  
  [Iuserpasswordstore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613273(v=vs.108).aspx) arabirimi tanımlar kalıcı hale getirmek için kullanıcı deposunda uygulanmalı yöntemleri parolaların karma. Bu, alma ve karma hale getirilen parola ve kullanıcı bir parola olarak ayarlanmış olup olmadığını gösteren bir yöntemi ayarlama için yöntemleri içerir.
- **IUserSecurityStampStore**  
  [IUserSecurityStampStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613277(v=vs.108).aspx) arabirimi tanımlar için kullanıcının hesap bilgilerini değiştirilip değiştirilmediğini gösteren bir güvenlik damgasını kullanmak için kullanıcı deposunda uygulanmalı yöntemleri . Bir kullanıcı parolasını değiştirir veya ekler veya oturumları kaldırır. Bu damga güncelleştirilir. Bu, alma ve güvenlik damgasını ayarlama için yöntemleri içerir.
- **Iusertwofactorstore**  
  [Iusertwofactorstore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613279(v=vs.108).aspx) arabirimi iki faktörlü kimlik doğrulaması uygulamak için uygulama gereken yöntemleri tanımlar. Bu, alma ve iki faktörlü kimlik doğrulamasını bir kullanıcı için etkinleştirilip etkinleştirilmediğini ayarlama için yöntemleri içerir.
- **IUserPhoneNumberStore**  
  [Iuserphonenumberstore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613275(v=vs.108).aspx) arabirimi kullanıcı telefon numaralarını depolamak için uygulanmalı yöntemleri tanımlar. Bu, alma ve telefon numarasını ve telefon numarasının onaylanıp olup ayarlama için yöntemleri içerir.
- **Iuseremailstore**  
  [Iuseremailstore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613143(v=vs.108).aspx) arabirimi kullanıcı e-posta adresleri saklamak için uygulanmalı yöntemleri tanımlar. Bu, alma ve e-posta adresini ve e-posta olup olmadığını onaylandıktan ayarlama için yöntemleri içerir.
- **Iuserlockoutstore**  
  [Iuserlockoutstore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613271(v=vs.108).aspx) arabirim bir hesap kilitleme hakkında bilgi depolamak için uygulamanız gereken yöntemleri tanımlar. Bu alma başarısız erişim denemelerinin geçerli sayısını, alma ve alma hesabı atılır ve bitiş tarihi kilitleme ayarı sayısı artan girişimleri başarısız olup olmadığını ayarlama ve başarısız oturum açma girişimlerinin sayısını sıfırlamak için yöntemler içerir.
- **Iqueryableuserstore**  
  [Iqueryableuserstore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613267(v=vs.108).aspx) arabirimi sorgulanabilir bir kullanıcı deposunun sağlamak için uygulanmalı üyeleri tanımlar. Sorgulanabilir kullanıcılar tutan bir özellik içeriyor.

  Gerekli arabirimler uygulamanıza; gibi Iuserclaimstore, Iuserloginstore, Iuserrolestore, Iuserpasswordstore ve IUserSecurityStampStore arabirimleri aşağıda gösterildiği gibi. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample5.cs)]

(Tüm arabirimlerin dahil olmak üzere) tam bir uygulama için bkz: [UserStore (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserStore.cs).

### <a name="identityuserclaim-identityuserlogin-and-identityuserrole"></a>IdentityUserClaim IdentityUserLogin ve IdentityUserRole

Microsoft.ASPNET.Identity.entityframework ad alanı içeriyor uygulamalarına [IdentityUserClaim](https://msdn.microsoft.com/library/dn613250(v=vs.108).aspx), [IdentityUserLogin](https://msdn.microsoft.com/library/dn613251(v=vs.108).aspx), ve [IdentityUserRole](https://msdn.microsoft.com/library/dn613252(v=vs.108).aspx) sınıflar. Bu özelliklerin kullanmanız durumunda bu sınıfların kendi sürüm oluşturma ve uygulamanızın özelliklerini tanımlamak isteyebilirsiniz. Ancak, bazen bu varlıkları belleğe (örneğin, ekleme veya bir kullanıcı talebinin kaldırma) temel işlemleri gerçekleştirirken yüklememeye daha etkilidir. Bunun yerine, arka uç depolama sınıfları bu işlemleri doğrudan veri kaynağına yürütebilir. Örneğin, UserStore.GetClaimsAsync() yöntemi userClaimTable.FindByUserId(user. çağırabilir Doğrudan tablo ve talep dönmesini bir sorgu yürütmek için kimliği) yöntemi.

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample6.cs)]

<a id="role"></a>
## <a name="customize-the-role-class"></a>Rol sınıfı özelleştirme

Kendi depolama sağlayıcısını uygularken eşdeğer olan rol sınıfı oluşturmalısınız [IdentityRole](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework.identityrole(v=vs.108).aspx) sınıfını [Microsoft.ASP.NET.Identity.EntityFramework](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework(v=vs.108).aspx) ad alanı:

Aşağıdaki diyagramda, oluşturmanız gereken IdentityRole sınıfı ve bu sınıfta gerçekleştirmek için arabirimi gösterir.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image5.png)

[IRole&lt;TKey&gt; ](https://msdn.microsoft.com/library/dn613268(v=vs.108).aspx) arabirimi RoleManager istenen işlem gerçekleştiriliyor aramanızı dener özellikleri tanımlar. Arabirim, iki özellik - kimliğini ve adını içerir. [IRole&lt;TKey&gt; ](https://msdn.microsoft.com/library/dn613268(v=vs.108).aspx) arabirimi aracılığıyla genel rolü için anahtar türünü belirtmenize imkan tanır **TKey** parametresi. ID özelliği türü ile eşleşen TKey parametrenin değeri.

Ayrıca kimlik çerçevesi sağlar [IRole](https://msdn.microsoft.com/library/microsoft.aspnet.identity.irole(v=vs.108).aspx) arabirimi (genel parametre) olmadan anahtar için bir dize değerini kullanmak istediğinizde.

Aşağıdaki örnek, bir tamsayı anahtarı kullanan bir IdentityRole sınıfı gösterir. Kimliği alanı, int, genel parametresinin değeri eşleşecek şekilde ayarlanır. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample7.cs)]

 Tam bir uygulama için bkz: [IdentityRole (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityRole.cs). 

<a id="rolestore"></a>
## <a name="customize-the-role-store"></a>Rol deposu özelleştirme

Roller tüm veri işlemleri için yöntemleri sağlayan RoleStore sınıf oluşturabilir. Bu sınıf eşdeğerdir [RoleStore&lt;TRole&gt; ](https://msdn.microsoft.com/library/dn468181(v=vs.108).aspx) Microsoft.ASP.NET.Identity.EntityFramework ad alanında sınıf. RoleStore sınıfınızda, uygulamanız [Irolestore&lt;TRole, TKey&gt; ](https://msdn.microsoft.com/library/dn613266(v=vs.108).aspx) ve isteğe bağlı olarak [Iqueryablerolestore&lt;TRole, TKey&gt; ](https://msdn.microsoft.com/library/dn613262(v=vs.108).aspx) arabirim.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image6.png)

Aşağıdaki örnek, bir rol deposu sınıfı gösterir. TRole genel parametre türü, genellikle tanımladığınız IdentityRole sınıfı olan rol sınıfınızın alır. TKey genel parametre rol anahtarınızı türünü alır. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample8.cs)]

- **Irolestore&lt;TRole&gt;**  
  [Irolestore](https://msdn.microsoft.com/library/dn468195.aspx) arabirimi rol deposu Sınıfınız içinde uygulanacak yöntemleri tanımlar. Bu, oluşturma, güncelleştirme, silme ve rolleri almak için yöntemler içerir.
- **RoleStore&lt;TRole&gt;**  
  RoleStore özelleştirmek için Irolestore arabirimi uygulayan bir sınıf oluşturun. Bu sınıf, uygulama yeterlidir, sisteminizde rolleri kullanmak istiyorsanız. Adlı bir parametre olarak alan oluşturucu *veritabanı* ExampleDatabase nasıl geçirileceğini veri erişim Sınıfınız içinde yalnızca bir gösterimi türüdür. Örneğin, MySQL uygulamasında, bu Oluşturucusu MySQLDatabase türünde bir parametre alır.  
  
  Tam bir uygulama için bkz: [RoleStore (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleStore.cs) .

<a id="reconfigure"></a>
## <a name="reconfigure-application-to-use-new-storage-provider"></a>Uygulama yeni depolaması sağlayıcısı kullanacak şekilde yeniden yapılandırın

Yeni depolama alanı sağlayıcınızla uyguladınız. Artık, uygulamanız bu depolaması sağlayıcısı kullanacak şekilde yapılandırmanız gerekir. Varsayılan depolama sağlayıcısını projenizde içeriyorsa, varsayılan sağlayıcıyı kaldırmak ve sağlayıcınız ile değiştirin.

### <a name="replace-default-storage-provider-in-mvc-project"></a>Varsayılan depolama sağlayıcısını MVC projesindeki değiştirin

1. İçinde **NuGet paketlerini Yönet** penceresinde kaldırma **Microsoft ASP.NET Identity EntityFramework** paket. Bu paketin yüklü paketleri Identity.EntityFramework için arama yaparak bulabilirsiniz.  
    ![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image7.png) Ayrıca Entity Framework kaldırmak istiyorsanız istenir. Uygulamanızın diğer bölümlerinde, gerekmiyorsa kaldırabilirsiniz.
2. Modeller klasörü IdentityModels.cs dosyasında silin veya açıklama **ApplicationUser** ve **ApplicationDbContext** sınıfları. Bir MVC uygulamasındaki tüm IdentityModels.cs dosyayı silebilirsiniz. Bir Web Forms uygulaması iki sınıf Sil ancak ayrıca IdentityModels.cs dosyasında bulunan yardımcı sınıf tutmak olduğundan emin olun.
3. Depolama alanı sağlayıcınızla ayrı bir projede yer alıyorsa, web uygulamanızda buna bir başvuru ekleyin.
4. Tüm başvuruları değiştirin `using Microsoft.AspNet.Identity.EntityFramework;` ile kullanarak bir depolama alanı sağlayıcınızla ad alanı bildirimi.
5. İçinde **Startup.Auth.cs** sınıfı, değişiklik **ConfigureAuth** uygun bağlamı tek bir örneğini kullanmak için yöntemi. 

    [!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample9.cs?highlight=3)]
6. Uygulamasında\_başlangıç klasörü, açık **IdentityConfig.cs**. ApplicationUserManager sınıfında değiştirme **Oluştur** , özelleştirilmiş kullanıcı deposu kullanan bir kullanıcı yöneticisini döndürmek için yöntemi. 

    [!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample10.cs?highlight=3)]
7. Tüm başvuruları değiştirin **ApplicationUser** ile **IdentityUser**.
8. Varsayılan proje Iuser arabiriminde tanımlanmayan kullanıcı sınıfının bazı üyeleri içerir; e-posta, PasswordHash ve GenerateUserIdentityAsync gibi. Kullanıcı sınıfınıza bu üyelere sahip değilse, uygulamadan veya bu üyeleri kullanan kod değiştirmeniz gerekir.
9. RoleManager tüm örneklerini oluşturduysanız, yeni RoleStore sınıfınıza kullanmak için bu kodu değiştirin.  

    [!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample11.cs)]
10. Varsayılan proje anahtarı için bir dize değerine sahip bir kullanıcı sınıfı için tasarlanmıştır. Kullanıcı sınıfınıza anahtar (örneğin, bir tamsayı) için farklı bir tür varsa, proje türünüz ile çalışacak şekilde değiştirmeniz gerekir. Bkz: [ASP.NET ıdentity'de kullanıcılar için birincil anahtarı değiştirme](change-primary-key-for-users-in-aspnet-identity.md).
11. Gerekirse, bağlantı dizesini Web.config dosyasına ekleyin.

<a id="other"></a>
## <a name="other-resources"></a>Diğer kaynaklar

- Blog: [ASP.NET Identity uygulama](http://odetocode.com/blogs/scott/archive/2014/01/20/implementing-asp-net-identity.aspx)
- Kod, öğretici ve GIT: [Simple.Data Asp.Net kimlik sağlayıcısı](http://designcoderelease.blogspot.co.uk/2015/03/simpledata-aspnet-identity-provider.html)
- Öğretici:[temel kimlik hesaplarını ayarlama ve bunları dış bir DB işaret eden](http://typecastexception.com/post/2013/10/27/Configuring-Db-Connection-and-Code-First-Migration-for-Identity-Accounts-in-ASPNET-MVC-5-and-Visual-Studio-2013.aspx). Tarafından [ @xivSolutions ](https://twitter.com/xivSolutions).
- Öğretici[: özel MySQL ASP.NET Identity depolama sağlayıcısı uygulama](implementing-a-custom-mysql-aspnet-identity-storage-provider.md)
- [CodeFluent varlıkları](http://blog.codefluententities.com/2014/04/30/asp-net-identity-v2-and-codefluent-entities/) tarafından [SoftFluent](http://www.softfluent.com/)
- [Azure tablo depolaması](https://www.nuget.org/packages/accidentalfish.aspnet.identity.azure/) James Randall tarafından.
- Azure tablo depolaması: [AspNet.Identity.TableStorage](https://github.com/stuartleeks/leeksnet.AspNet.Identity.TableStorage) tarafından [ @stuartleeks ](https://twitter.com/stuartleeks).
- [CouchDB / Daniel Wertheim tarafından Cloudant.](https://github.com/danielwertheim/mycouch.aspnet.identity)
- Esnek arama[h: esnek kimlik](https://github.com/bmbsqd/elastic-identity) Bombsquad AB. tarafından
- [MongoDB](http://www.nuget.org/packages/MongoDB.AspNet.Identity/) Jonathan Sheely Jonathan Sheely tarafından.
- [NHibernate.AspNet.Identity](https://github.com/milesibastos/NHibernate.AspNet.Identity) Antônio Milesi Bastos tarafından.
- [RavenDB](http://www.nuget.org/packages/AspNet.Identity.RavenDB/1.0.0) tarafından [ @tourismgeek ](https://twitter.com/tourismgeek).
- [RavenDB.AspNet.Identity](https://github.com/ILMServices/RavenDB.AspNet.Identity) tarafından [ILMServices](http://www.ilmservice.com/).
- Redis: [Redis.AspNet.Identity](https://github.com/aminjam/Redis.AspNet.Identity)
- EF oluşturmak için T4 şablonlarını kod için bir "ilk veritabanı" kullanıcı deposu: [AspNet.Identity.EntityFramework](https://github.com/cbfrank/AspNet.Identity.EntityFramework)
