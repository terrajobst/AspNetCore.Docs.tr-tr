---
uid: web-forms/overview/older-versions-security/roles/creating-and-managing-roles-cs
title: Rolleri (C#) oluşturarak ve yöneterek | Microsoft Docs
author: rick-anderson
description: Bu öğretici rolleri framework yapılandırmak için gerekli adımları inceler. Web sayfalarını oluşturmak ve rolleri silmek için oluşturacaksınız.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/24/2008
ms.topic: article
ms.assetid: 113f10b3-a19a-471b-8ff6-db3c79ce8a91
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/roles/creating-and-managing-roles-cs
msc.type: authoredcontent
ms.openlocfilehash: a4ea7e76e023cd436d1d8ac52307a3ac17267fef
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="creating-and-managing-roles-c"></a>Oluşturma ve yönetme rolleri (C#)
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indirme](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/CS.09.zip) veya [PDF indirin](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/aspnet_tutorial09_CreatingRoles_cs.pdf)

> Bu öğretici rolleri framework yapılandırmak için gerekli adımları inceler. Web sayfalarını oluşturmak ve rolleri silmek için oluşturacaksınız.


## <a name="introduction"></a>Giriş

İçinde <a id="_msoanchor_1"> </a> [ *kullanıcı tabanlı bir yetkilendirme* ](../membership/user-based-authorization-cs.md) sayfaları kümesinden belirli kullanıcılara kısıtlamak için URL yetkilendirmesi kullanarak Aranan ve bildirim temelli incelediniz öğretici ve bir ASP.NET sayfasının işlevselliği ziyaret kullanıcıyı temel alarak ayarlamak için programlı teknikler. Sayfa erişim veya kullanıcı tarafından temelinde işlevselliği için izin verme, ancak, bir bakım senaryolarda çok sayıda kullanıcı hesabının bulunduğu veya kullanıcıların ayrıcalıkları genellikle değiştiğinde kabus olabilir. Bir kullanıcı kazanır veya belirli bir görevi gerçekleştirmek için yetkilendirme kaybederse dilediğiniz zaman yönetici uygun URL yetkilendirme kuralları, bildirim temelli biçimlendirme ve kod güncelleştirmesi gerekir.

Genellikle kullanıcıların gruplar halinde sınıflandırmak yardımcı olabilir veya *rolleri* ve ardından bir rol tarafından rol temelinde izinleri uygulamak için. Örneğin, çoğu web uygulamaları sayfaları veya yalnızca yönetici kullanıcılar için ayrılmış görevleri belirli bir kümesine sahiptir. Teknikleri kullanarak öğrenilen *kullanıcı tabanlı bir yetkilendirme* öğretici, eklediğimiz uygun URL yetkilendirme kuralları, bildirim temelli biçimlendirme ve yönetim görevlerini gerçekleştirmek belirtilen kullanıcı hesaplarını izin vermek için kod. Ancak yeni bir yönetici eklediyseniz veya var olan bir yönetici iptal kendi yönetim haklarına sahip gerekirse, biz dönün ve yapılandırma dosyalarını ve web sayfalarını güncelleştirme yapmanız gerekir. Rolleri ile ancak biz Yöneticiler adlı bir rol oluşturun ve güvenilen kullanıcılarla yöneticileri rolüne atayın. Ardından, biz uygun URL yetkilendirme kuralları, bildirim temelli biçimlendirme ve çeşitli yönetim görevlerini gerçekleştirmek Administrators rolünün izin vermek için kodu ekleyin. Yerinde bu altyapısıyla siteye yeni yöneticiler ekleme veya var olanları kaldırma dahil olmak üzere veya kullanıcı Yöneticiler rolden kaldırma kadar basittir. Hiçbir yapılandırma, bildirim temelli işaretleme veya kod değişikliklerinden gereklidir.

ASP.NET rollerini tanımlama ve kullanıcı hesaplarıyla ilişkilendirmek için bir rol çerçeve sunar. Rolleri çerçevesiyle biz oluşturabilir ve rolleri, silebilir veya kullanıcılar bir rolden kaldırmak, belirli bir role ait olan ve bir kullanıcının belirli bir role ait olup olmadığını bildirmek kullanıcıları belirlemek için kullanıcılar ekleyin. Rolleri framework yapılandırıldıktan sonra biz URL yetkilendirme kuralları aracılığıyla rol rollü temelinde sayfalarına erişimi sınırlamak ve gösterebilir veya ek bilgiler veya şu anda oturum açmış kullanıcının rollerine dayalı bir sayfada işlevselliği gizleyebilirsiniz.

Bu öğretici rolleri framework yapılandırmak için gerekli adımları inceler. Web sayfalarını oluşturmak ve rolleri silmek için oluşturacaksınız. İçinde <a id="_msoanchor_2"> </a> [ *kullanıcıları rollere atama* ](assigning-roles-to-users-cs.md) öğretici biz bakabilir ekleme ve kullanıcıların rollerini kaldırın. Ve <a id="_msoanchor_3"> </a> [ *rol tabanlı yetkilendirme* ](role-based-authorization-cs.md) biz sayfa işlevselliği bağlı olarak ayarlamak nasıl birlikte rol rollü temelinde sayfalarına erişimi sınırlamak nasıl görürsünüz Öğreticisi ziyaret kullanıcı rolü. Haydi başlayalım!

## <a name="step-1-adding-new-aspnet-pages"></a>1. adım: Yeni ASP.NET sayfaları ekleme

Bu öğretici ve sonraki iki biz çeşitli rolleri ilgili işlevleri ve özellikleri inceleniyor. ASP.NET sayfaları bu öğreticileri incelenmesi konuları uygulamak için bir dizi ihtiyacımız. Şimdi bu sayfaları oluşturun ve site haritası güncelleştirin.

Başlangıç adlı projeye yeni bir klasör oluşturarak `Roles`. Ardından, dört yeni ASP.NET sayfaları ekleme `Roles` klasörü, her sayfasıyla bağlama `Site.master` ana sayfa. Sayfa adı:

- `ManageRoles.aspx`
- `UsersAndRoles.aspx`
- `CreateUserWizardWithRoles.aspx`
- `RoleBasedAuthorization.aspx`

Bu noktada, projenizin Solution Explorer Şekil 1'de gösterilen ekran benzer görünmelidir.


[![Dört yeni sayfa rolleri klasöre eklendi](creating-and-managing-roles-cs/_static/image2.png)](creating-and-managing-roles-cs/_static/image1.png)

**Şekil 1**: dört yeni sayfalar eklenmiştir `Roles` klasörü ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-and-managing-roles-cs/_static/image3.png))


Her sayfada bu noktada, iki içerik, her ana sayfanın ContentPlaceHolders için denetimleriniz gerekir: `MainContent` ve `LoginContent`.

[!code-aspx[Main](creating-and-managing-roles-cs/samples/sample1.aspx)]

Sözcüğünün `LoginContent` ContentPlaceHolder'ın varsayılan biçimlendirme oturum açarken veya devre dışı olup olmadığını kullanıcının kimliği doğrulanır bağlı olarak site için bir bağlantı görüntülenir. Varlığını `Content2` içerik ASP.NET sayfası denetiminde ancak ana sayfanın varsayılan biçimlendirme geçersiz kılar. Biz anlatıldığı gibi <a id="_msoanchor_4"> </a> [ *form kimlik doğrulaması bir genel bakış* ](../introduction/an-overview-of-forms-authentication-cs.md) varsayılan işaretleme geçersiz kılma öğretici, burada değil istiyoruz görüntülemek için oturum açma ile ilgili sayfalarında kullanışlıdır Sol sütunda seçenekleri.

Bu dört sayfaları için ancak için ana sayfa varsayılan biçimlendirmesini göstermek istiyoruz `LoginContent` ContentPlaceHolder. Bu nedenle, kaldırmak için bildirim temelli biçimlendirme `Content2` içerik denetimi. Bunu yaptıktan sonra her dört sayfanın biçimlendirme yalnızca bir içerik denetimi içermelidir.

Son olarak, site haritası şimdi güncelleştirme (`Web.sitemap`) bu yeni web sayfaları eklenecek. Aşağıdaki XML sonra eklemek `<siteMapNode>` üyelik öğreticileri için eklediğimiz.

[!code-xml[Main](creating-and-managing-roles-cs/samples/sample2.xml)]

Site haritasını güncelleştirilmiş bir tarayıcı aracılığıyla sitesini ziyaret edin. Şekil 2'de görüldüğü gibi sol taraftaki gezinti şimdi rolleri öğreticileri için öğeleri içerir.


[![Dört yeni sayfa rolleri klasöre eklendi](creating-and-managing-roles-cs/_static/image5.png)](creating-and-managing-roles-cs/_static/image4.png)

**Şekil 2**: dört yeni sayfalar eklenmiştir `Roles` klasörü ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-and-managing-roles-cs/_static/image6.png))


## <a name="step-2-specifying-and-configuring-the-roles-framework-provider"></a>2. adım: Belirtme ve rolleri Framework sağlayıcısı yapılandırma

Üyelik framework gibi rolleri framework sağlayıcı modeli üzerinde oluşturulmuştur. ' Da anlatıldığı gibi <a id="_msoanchor_5"> </a> [ *güvenlik temel kavramları ve ASP.NET Destek* ](../introduction/security-basics-and-asp-net-support-cs.md) öğretici, .NET Framework üç yerleşik rol sağlayıcıları ile birlikte gelir: [ `AuthorizationStoreRoleProvider` ](https://msdn.microsoft.com/library/system.web.security.authorizationstoreroleprovider.aspx) , [ `WindowsTokenRoleProvider` ](https://msdn.microsoft.com/library/system.web.security.windowstokenroleprovider.aspx), ve [ `SqlRoleProvider` ](https://msdn.microsoft.com/library/system.web.security.sqlroleprovider.aspx). Bu öğretici seri odaklanır `SqlRoleProvider`, rol deposu olarak bir Microsoft SQL Server veritabanı kullanır.

Kapak altındaki rolleri framework ve `SqlRoleProvider` üyelik framework gibi iş ve `SqlMembershipProvider`. .NET Framework içeren bir `Roles` rolleri framework API'sine gören sınıf. `Roles` Sınıfına sahip statik yöntemler gibi `CreateRole`, `DeleteRole`, `GetAllRoles`, `AddUserToRole`, `IsUserInRole`, vb. Aşağıdaki yöntemlerden birini çağrıldığında, `Roles` sınıfı yapılandırılmış sağlayıcı çağrısı atar. `SqlRoleProvider` Role özgü tablolarla çalışır (`aspnet_Roles` ve `aspnet_UsersInRoles`) yanıt.

Kullanmak için `SqlRoleProvider` uygulamamız sağlayıcısında, biz ne deposu olarak kullanmak için veritabanı belirtmeniz gerekir. `SqlRoleProvider` Belirli veritabanı tabloları, görünümleri ve saklı yordamlar için belirtilen rol deposu bekliyor. Bu gerekli veritabanı nesnelerini kullanılarak eklenebilir [ `aspnet_regsql.exe` aracı](https://msdn.microsoft.com/library/ms229862.aspx). Şemanın için gerekli olan bir veritabanı zaten bu noktada sahibiz `SqlRoleProvider`. Geri <a id="_msoanchor_6"> </a> [ *SQL Server üyelik şema oluşturma* ](../membership/creating-the-membership-schema-in-sql-server-cs.md) adlı bir veritabanı oluşturduğumuz öğretici `SecurityTutorials.mdf` ve kullanılan `aspnet_regsql.exe` uygulama eklemek için gerekli veritabanı nesnelerini dahil Hizmetleri `SqlRoleProvider`. Bu nedenle yalnızca Rol desteğini etkinleştirmek ve kullanmak için rolleri framework bildirmek ihtiyacımız `SqlRoleProvider` ile `SecurityTutorials.mdf` veritabanı rol deposu olarak.

Rolleri çerçevesi aracılığıyla yapılandırılan &lt; `roleManager` &gt; uygulamanın öğesinde `Web.config` dosya. Varsayılan olarak, rol destek devre dışıdır. Etkinleştirmek için ayarlamalısınız [ &lt; `roleManager` &gt; ](https://msdn.microsoft.com/library/ms164660.aspx) öğenin `enabled` özniteliğini `true` sözlüğüdür:

[!code-xml[Main](creating-and-managing-roles-cs/samples/sample3.xml)]

Varsayılan olarak, tüm web uygulamalarının adlı bir rol sağlayıcısı sahip `AspNetSqlRoleProvider` türü `SqlRoleProvider`. Bu varsayılan sağlayıcı kaydı yapılmadığı `machine.config` (konumunda bulunan `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\CONFIG`):

[!code-xml[Main](creating-and-managing-roles-cs/samples/sample4.xml)]

Sağlayıcının `connectionStringName` özniteliği kullanılır rol deposu belirtir. `AspNetSqlRoleProvider` Sağlayıcısı bu öznitelik ayarlar `LocalSqlServer`, ayrıca tanımlanan içinde `machine.config` ve varsayılan olarak, bir SQL Server 2005 Express Edition veritabanına noktaları `App_Data` adlı klasörü `aspnet.mdf`.

Sonuç olarak, biz yalnızca rolleri framework bizim uygulamanın içinde herhangi bir sağlayıcı bilgi belirtmeden etkinleştirirseniz `Web.config` dosya, uygulamayı kullanan kayıtlı varsayılan rol sağlayıcı `AspNetSqlRoleProvider`. Varsa `~/App_Data/aspnet.mdf` veritabanı yok, ASP.NET çalışma zamanı otomatik olarak oluşturun ve uygulama hizmetleri şema ekleyin. Ancak, biz kullanmak istemiyorsanız `aspnet.mdf` ; bunun yerine, biz kullanmak istediğiniz veritabanı `SecurityTutorials.mdf` biz zaten oluşturulan ve uygulama hizmetleri eklenir veritabanı. Bu değişikliği iki yoldan biriyle gerçekleştirilebilir:

- <strong>İçin bir değer belirtin</strong><strong>`LocalSqlServer`</strong><strong>bağlantı dizesi adı</strong><strong>`Web.config`</strong><strong>.</strong> Üzerine tarafından `LocalSqlServer` bağlantı dizesi adı değerindeki `Web.config`, kayıtlı varsayılan rol sağlayıcı kullanıyoruz (`AspNetSqlRoleProvider`) ve düzgün çalışması `SecurityTutorials.mdf` veritabanı. Bu teknik hakkında daha fazla bilgi için bkz: [Scott Guthrie](https://weblogs.asp.net/scottgu/)'s blog gönderisi [ASP.NET 2.0 uygulama hizmetlerini yapılandırma kullanım SQL Server 2000 veya SQL Server 2005](https://weblogs.asp.net/scottgu/archive/2005/08/25/423703.aspx).
- <strong>Yeni bir kayıtlı sağlayıcısı türü eklemek</strong><strong>`SqlRoleProvider`</strong><strong>ve yapılandırma kendi</strong><strong>`connectionStringName`</strong><strong>işaretedecekşekildeayarlama</strong> <strong>`SecurityTutorials.mdf`</strong> <strong>veritabanı.</strong> Bu önerilen ve kullanılan yaklaşımdır <a id="_msoanchor_7"> </a> [ *SQL Server üyelik şema oluşturma* ](../membership/creating-the-membership-schema-in-sql-server-cs.md) öğretici, bu ise t Bu öğreticide kullanacak yaklaşım.

Aşağıdaki roller yapılandırma biçimlendirme eklemek `Web.config` dosya. Bu biçimlendirme adlı yeni bir sağlayıcı kaydeder `SecurityTutorialsSqlRoleProvider`.

[!code-xml[Main](creating-and-managing-roles-cs/samples/sample5.xml)]

Yukarıdaki biçimlendirme tanımlar `SecurityTutorialsSqlRoleProvider` varsayılan sağlayıcı olarak (aracılığıyla `defaultProvider` özniteliğini `<roleManager>` öğesi). Ayrıca ayarlar `SecurityTutorialsSqlRoleProvider`'s `applicationName` ayarını `SecurityTutorials`, aynı olduğu `applicationName` üyelik sağlayıcısı tarafından kullanılan ayar (`SecurityTutorialsSqlMembershipProvider`). Burada gösterilmeyen sırada [ `<add>` öğesi](https://msdn.microsoft.com/library/ms164662.aspx) için `SqlRoleProvider` , içerebilir bir `commandTimeout` özniteliği veritabanı zaman aşımı süresini saniye cinsinden belirtin. Varsayılan değer 30'dur.

Yerinde bu yapılandırma biçimlendirme ile biz uygulamamız içinde rol işlevselliği kullanmaya başlamak hazırsınız.

> [!NOTE]
> Yukarıdaki yapılandırma biçimlendirme kullanarak gösterilmektedir &lt; `roleManager` &gt; öğenin `enabled` ve `defaultProvider` öznitelikleri. Kullanıcı tarafından olarak rol bilgilerini nasıl rolleri framework ilişkilendirir etkileyen diğer öznitelikleri mevcuttur. Bu ayarları inceleyeceğiz <a id="_msoanchor_8"> </a> [ *rol tabanlı yetkilendirme* ](role-based-authorization-cs.md) Öğreticisi.


## <a name="step-3-examining-the-roles-api"></a>3. adım: rol API'si inceleniyor

Rolleri framework'ün işlevselliği yoluyla kullanıma sunulan [ `Roles` sınıfı](https://msdn.microsoft.com/library/system.web.security.roles.aspx), rol tabanlı işlemleri gerçekleştirmek için on üç statik yöntemler içerir. Ne zaman oluşturma ve silme adımları 4'te rolleri ele ve 6 kullanacağız [ `CreateRole` ](https://msdn.microsoft.com/library/system.web.security.roles.createrole.aspx) ve [ `DeleteRole` ](https://msdn.microsoft.com/library/system.web.security.roles.deleterole.aspx) rolü ekleme veya bir sistemden kaldırma yöntemleri.

Sistemdeki tüm rollerin listesini almak için kullanın [ `GetAllRoles` yöntemi](https://msdn.microsoft.com/library/system.web.security.roles.getallroles.aspx) (adım 5 bakın). [ `RoleExists` Yöntemi](https://msdn.microsoft.com/library/system.web.security.roles.roleexists.aspx) belirtilen bir rolün var olup olmadığını gösteren bir Boole değeri döndürür.

Sonraki öğreticide kullanıcılar rolleriyle ilişkilendirmek üzere nasıl inceleyeceğiz. `Roles` Sınıfının [ `AddUserToRole` ](https://msdn.microsoft.com/library/system.web.security.roles.addusertorole.aspx), [ `AddUserToRoles` ](https://msdn.microsoft.com/library/system.web.security.roles.addusertoroles.aspx), [ `AddUsersToRole` ](https://msdn.microsoft.com/library/system.web.security.roles.adduserstorole.aspx), ve [ `AddUsersToRoles` ](https://msdn.microsoft.com/library/system.web.security.roles.adduserstoroles.aspx) yöntemlerden bir veya daha fazla kullanıcı için bir veya daha fazla rol ekleyin. Kullanıcıların rollerini kaldırmak için kullanın [ `RemoveUserFromRole` ](https://msdn.microsoft.com/library/system.web.security.roles.removeuserfromrole.aspx), [ `RemoveUserFromRoles` ](https://msdn.microsoft.com/library/system.web.security.roles.removeuserfromroles.aspx), [ `RemoveUsersFromRole` ](https://msdn.microsoft.com/library/system.web.security.roles.removeusersfromrole.aspx), veya [ `RemoveUsersFromRoles` ](https://msdn.microsoft.com/library/system.web.security.roles.removeusersfromroles.aspx) yöntemleri.

İçinde <a id="_msoanchor_9"> </a> [ *rol tabanlı yetkilendirme* ](role-based-authorization-cs.md) öğretici biz programlı olarak göster veya şu anda oturum açmış kullanıcının rolüne dayalı işlevselliği Gizle yolları konumunda görünür. Bunu başarmak için biz kullanabilirsiniz `Role` sınıfının [ `FindUsersInRole` ](https://msdn.microsoft.com/library/system.web.security.roles.findusersinrole.aspx), [ `GetRolesForUser` ](https://msdn.microsoft.com/library/system.web.security.roles.getrolesforuser.aspx), [ `GetUsersInRole` ](https://msdn.microsoft.com/library/system.web.security.roles.getusersinrole.aspx), veya [ `IsUserInRole` ](https://msdn.microsoft.com/library/system.web.security.roles.isuserinrole.aspx) yöntemleri.

> [!NOTE]
> Her zaman bu yöntemlerden birini çağrılır, göz önünde bulundurmanız `Roles` sınıfı yapılandırılmış sağlayıcı çağrısı atar. Örneğimizde, bu çağrı için gönderiliyor anlamına gelir `SqlRoleProvider`. `SqlRoleProvider` Üzerinde çağrılan yöntem dayalı olarak uygun veritabanı işlemini gerçekleştirir. Örneğin, kod `Roles.CreateRole("Administrators")` sonuçlanır `SqlRoleProvider` yürütme `aspnet_Roles_CreateRole` saklı içine yeni bir kayıt ekler yordamı `aspnet_Roles` Yöneticiler adlı tablo.


Bu öğreticinin geri kalanında kullanarak arar `Roles` sınıfının `CreateRole`, `GetAllRoles`, ve `DeleteRole` sistem rollerini yönetmek için yöntemler.

## <a name="step-4-creating-new-roles"></a>4. adım: Yeni rol oluşturma

Rolleri rasgele grubu kullanıcıları için bir yol sunar ve bu gruplandırma yetkilendirme kuralları uygulamak için daha kullanışlı bir yöntem için en yaygın olarak kullanılır. Ancak rolleri bir yetkilendirme mekanizması olarak kullanmak için önce uygulamanın hangi rollerin mevcut tanımlamak ihtiyacımız. Ne yazık ki, ASP.NET CreateRoleWizard denetim içermez. Uygun kullanıcı arabirimi oluşturmak ve kendisini rolleri API'sini çağırmak için ihtiyacımız yeni rolleri eklemek için. İyi haber bunu gerçekleştirmek çok kolay olmasıdır.

> [!NOTE]
> Yoktur, ancak hiçbir CreateRoleWizard Web denetimi [ASP.NET Web sitesi yönetim aracı](https://msdn.microsoft.com/library/ms228053.aspx), görüntüleme ve web uygulamanızın yapılandırma yönetmeye yardımcı olmak için tasarlanmış yerel bir ASP.NET uygulaması olduğu. Ancak, büyük bir ASP.NET Web Sitesi Yönetim Aracı'nı fan için iki nedenden dolayı değilim. İlk olarak, bir bit buggy ve kullanıcı deneyimini istenen için çok bırakır. İkinci olarak, ASP.NET Web Sitesi Yönetim Aracı'nı yalnızca yerel olarak çalışmak üzere dinamik site rollerinde uzaktan yönetmeniz gerekiyorsa kendi Rol Yönetim web sayfaları oluşturma gerekecek anlamı tasarlanmıştır. Bu iki nedenden dolayı üzerinde ASP.NET Web sitesi yönetim aracı güvenmek yerine gerekli rol yönetim araçları, bir web sayfasındaki derleme Bu öğretici ve sonraki odaklanır.


Açık `ManageRoles.aspx` sayfasındaki `Roles` klasörü ve metin kutusu ve bir düğme Web denetimi sayfasına ekleyin. Metin kutusu denetiminin ayarlamak `ID` özelliğine `RoleName` ve düğmenin `ID` ve `Text` özelliklerine `CreateRoleButton` ve rol oluşturmak, sırasıyla. Bu noktada, sayfanızın bildirim temelli biçimlendirme aşağıdakine benzer görünmelidir:

[!code-aspx[Main](creating-and-managing-roles-cs/samples/sample6.aspx)]

Ardından, çift `CreateRoleButton` düğme denetimi oluşturmak için Tasarımcısı'nda bir `Click` olay işleyicisi ve aşağıdaki kodu ekleyin:

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample7.cs)]

Yukarıdaki kod, girilen bölünen rol adı atayarak başlatır `RoleName` metin kutusuna `newRoleName` değişkeni. Ardından, `Roles` sınıfının `RoleExists` yöntemi belirlemek için çağrılır rolü `newRoleName` sistemde zaten mevcut. Rol mevcut değilse çağrısıyla oluşturulur `CreateRole` yöntemi. Varsa `CreateRole` yöntemi sistemde zaten bir rol adı geçirilen bir `ProviderException` özel durumu oluşur. Kod rolü zaten çağırmadan önce sisteminde mevcut olmadığından emin olmak için ilk denetler. Bu yüzden `CreateRole`. `Click` Olay işleyicisi sonucuna Temizleme tarafından `RoleName` TextBox'ın `Text` özelliği.

> [!NOTE]
> Ne olacağını merak ediyor kullanıcı herhangi bir değer girmezse `RoleName` metin kutusu. Değer içine aktarılırsa `CreateRole` yöntemi `null` ya da boş bir dize, bir özel durum oluşturulur. Benzer şekilde, virgül rol adı içeriyorsa, özel durum oluşturuldu. Sonuç olarak, sayfa kullanıcının bir rol girer ve her virgülü içermediğinden emin olmak için doğrulama denetimlerinin içermelidir. Okuyucu için bir alıştırma olarak bırakın


Yöneticiler adlı bir rol oluşturalım. Ziyaret `ManageRoles.aspx` sayfasında bir tarayıcıdan, Yöneticiler, metin kutusuna yazın (bkz. Şekil 3) ve ardından Rol Oluştur düğmesine tıklayın.


[![Yöneticiler rolü oluşturma](creating-and-managing-roles-cs/_static/image8.png)](creating-and-managing-roles-cs/_static/image7.png)

**Şekil 3**: Yöneticiler rolü oluşturma ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-and-managing-roles-cs/_static/image9.png))


Ne olur? Geri gönderimin oluşur, ancak rol gerçekte boyunca görsel ipucu yok sistemine eklenmiş. Adım 5'i görsel geribildirim dahil etmek için bu sayfada güncelleştireceğiz. Şimdilik, ancak rol giderek oluşturulduğunu doğrulayabilirsiniz `SecurityTutorials.mdf` veritabanını ve verileri görüntüleme `aspnet_Roles` tablo. Şekil 4'te gösterildiği gibi `aspnet_Roles` tablosu yalnızca eklenen Yöneticiler rolleri için bir kayıt içerir.


[![Tablo aspnet_Roles yöneticileri için bir satır var](creating-and-managing-roles-cs/_static/image11.png)](creating-and-managing-roles-cs/_static/image10.png)

**Şekil 4**: `aspnet_Roles` tablolu bir satır için Yöneticiler ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-and-managing-roles-cs/_static/image12.png))


## <a name="step-5-displaying-the-roles-in-the-system"></a>5. adım: sistemde rolleri görüntüleme

Şimdi büyütmek `ManageRoles.aspx` sayfa sistemde geçerli rollerin listesini içerir. Bunu gerçekleştirmek için bir GridView denetimi sayfasına ekleyin ve ayarlayın, `ID` özelliğine `RoleList`. Ardından, adlı sayfanın arka plan kodu sınıfına bir yöntem eklemek `DisplayRolesInGrid` aşağıdaki kodu kullanarak:

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample8.cs)]

`Roles` Sınıfının `GetAllRoles` yöntemi döndürür tüm rollerin sistemde dize dizisi. Bu dize dizisi sonra GridView bağlıdır. Sayfa ilk kez yüklendiğinde rollerin listesini GridView bağlamak amacıyla çağırmak ihtiyacımız `DisplayRolesInGrid` sayfanın yönteminden `Page_Load` olay işleyicisi. Aşağıdaki kod, sayfa ilk sitesini ziyaret ettiğinizde, ancak sonraki Geri göndermeler bu yöntemi çağırır.

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample9.cs)]

Bu kod yerinde bir tarayıcı aracılığıyla sayfasını ziyaret edin. Şekil 5 gösterildiği gibi bir kılavuz öğesi olarak etiketlenmiş için tek bir sütun görmeniz gerekir. Kılavuz adım 4'te eklediğimiz yöneticileri rol için bir satır içerir.


[![GridView, tek bir sütunda rollerini görüntüler](creating-and-managing-roles-cs/_static/image14.png)](creating-and-managing-roles-cs/_static/image13.png)

**Şekil 5**: GridView, tek bir sütunda rollerini görüntüler ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-and-managing-roles-cs/_static/image15.png))


GridView çünkü madde etiketli silmenizin bir sütunu görüntüler GridView's `AutoGenerateColumns` özelliği otomatik olarak her bir özellik için bir sütun oluşturmak GridView neden olur (varsayılan) True olarak ayarlanmış kendi `DataSource`. Bir dizi dizideki öğeler, bu nedenle GridView tek sütunda temsil eden tek bir özelliğe sahip.

GridView verilerle görüntülerken, açıkça my sütunları tanımlamak yerine bunları örtük olarak GridView tarafından oluşturulan istemiyorum. Açıkça sütunları tanımlayarak veri biçimi, sütunları yeniden düzenleyin ve diğer ortak görevler gerçekleştirmek çok daha kolay olur. Bu nedenle, böylece sütunlarını açıkça tanımlanmış şimdi GridView'ın bildirim temelli biçimlendirme güncelleştirin.

Başlangıç GridView's ayarlayarak `AutoGenerateColumns` özelliğini false olarak ayarlayın. Ardından, bir TemplateField kılavuzuna ekleyin ayarlayın, `HeaderText` rollere, özellik ve yapılandırma kendi `ItemTemplate` böylece dizinin içeriğini görüntüler. Bunu gerçekleştirmek için adlı bir etiket Web denetimi ekleme `RoleNameLabel` için `ItemTemplate` ve bağlama kendi `Text` özelliğine `Container.DataItem`.

Bu özellikler ve `ItemTemplate`'s içeriği bildirimli olarak veya GridView'ın alanları iletişim kutusu ve Şablonları Düzenle arabirimi ayarlanabilir. İletişim kutusu alanları ulaşmak için GridView kullanıcının akıllı etiket Düzenle sütunları bağlantıya tıklayın. Ardından, ayarlamak için otomatik oluşturma alanları onay kutusunun işaretini `AutoGenerateColumns` özelliği False olarak ve bir TemplateField GridView için ekleme ayarını kendi `HeaderText` rolüne özelliği. Tanımlamak için `ItemTemplate`'s içeriklerini, GridView kullanıcının akıllı etiketten Şablonları Düzenle seçeneğini seçin. Bir etiket Web denetimi sürükleyin `ItemTemplate`ayarlayın kendi `ID` özelliğine `RoleNameLabel`ve bağlama ayarlarını yapılandırmak için kendi `Text` özelliği bağlı `Container.DataItem`.

İşiniz bittiğinde kullandığınız hangi yaklaşımın bağımsız olarak GridView'in sonuçta elde edilen bildirim temelli biçimlendirme aşağıdakine benzer görünmelidir.

[!code-aspx[Main](creating-and-managing-roles-cs/samples/sample10.aspx)]

> [!NOTE]
> Veri bağlama söz dizimini kullanarak dizinin içeriğini görüntülenen `<%# Container.DataItem %>`. Bir dizinin içeriğini görüntüleme GridView bağlı olduğunda bu sözdizimi neden kullanılır kapsamlı açıklaması Bu öğretici kapsamında değildir. Bu konuyla ilgili daha fazla bilgi için başvurmak [skaler dizi bir veri Web denetime bağlama](http://aspnet.4guysfromrolla.com/articles/082504-1.aspx).


Şu anda `RoleList` sayfa ilk sitesini ziyaret ettiğinizde GridView rollerin listesini yalnızca bağlı. Yeni bir rolü eklendiğinde kılavuz yenilemek gerekir. Bunu başarmak için güncelleştirme `CreateRoleButton` düğmenin `Click` onun çağırır olay işleyicisi `DisplayRolesInGrid` yeni bir rol oluşturduysanız yöntemi.

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample11.cs)]

Artık kullanıcı eklediğinde yeni bir rol `RoleList` rolü başarıyla oluşturuldu visual geribildirim sağlama GridView geri göndermede, yeni eklenen rol gösterir. Bunu göstermek için ziyaret `ManageRoles.aspx` sayfasında bir tarayıcıdan ve denetçiler adında bir rolü ekleyin. Rol Oluştur düğmesine tıkladığınızda, üzerine geri gönderimin olun ve kılavuz Yöneticiler ve bunun yanı sıra yeni rol, denetçiler içerecek şekilde güncelleştirir.


[![Denetçiler rolüne eklendi sahip](creating-and-managing-roles-cs/_static/image17.png)](creating-and-managing-roles-cs/_static/image16.png)

**Şekil 6**: denetçiler rolüne sahip eklendi ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-and-managing-roles-cs/_static/image18.png))


## <a name="step-6-deleting-roles"></a>6. adım: Rollerini silme

Bu noktada bir kullanıcı yeni bir rol oluşturabilir ve varolan tüm rollerden görüntülemek `ManageRoles.aspx` sayfası. Şirketinizdeki kullanıcıların da rollerini silmesine izin verin. `Roles.DeleteRole` Yöntemi iki aşırı sahiptir:

- [`DeleteRole(roleName)`](https://msdn.microsoft.com/library/ek4sywc0.aspx) -rolü siler *roleName*. Rolü bir veya daha fazla üye içeriyorsa özel durum oluşur.
- [`DeleteRole(roleName, throwOnPopulatedRole)`](https://msdn.microsoft.com/library/38h6wf59.aspx) -rolü siler *roleName*. Varsa *throwOnPopulateRole* olan `true`, rolü bir veya daha fazla üye içeriyorsa, bir özel durum oluşturulduktan sonra. Varsa *throwOnPopulateRole* olan `false`, rolü veya herhangi bir üye içerip içermediğini silinirse. Dahili olarak, `DeleteRole(roleName)` yöntem çağrılarını `DeleteRole(roleName, true)`.

`DeleteRole` , Yöntemi bir özel durum da oluşturur *roleName* olan `null` ya da boş bir dize veya *roleName* virgül içerir. Varsa *roleName* sistemde yok `DeleteRole` sessiz bir şekilde, bir özel durum yükseltme olmadan başarısız olur.

Şimdi GridView büyütmek `ManageRoles.aspx` dahil etmek için bir silme düğmesi, tıklatıldığında, seçili rolü siler. Alanları iletişim kutusuna gidip CommandField seçeneği altında bulunan bir Delete düğmesi ekleme GridView Sil düğmesini ekleyerek başlayın. Delete solundaki sütun düğmesine tıklayın ve ayarlama yapmak kendi `DeleteText` özelliği rolü silmek için.


[![RoleList GridView Delete düğme ekleme](creating-and-managing-roles-cs/_static/image20.png)](creating-and-managing-roles-cs/_static/image19.png)

**Şekil 7**: Sil düğme eklemek `RoleList` GridView ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-and-managing-roles-cs/_static/image21.png))


Sil düğmesini ekledikten sonra GridView'ın bildirim temelli biçimlendirme aşağıdakine benzer görünmelidir:

[!code-aspx[Main](creating-and-managing-roles-cs/samples/sample12.aspx)]

Ardından, olay işleyicisi GridView için 's oluşturmak `RowDeleting` olay. Bu rol Sil düğmesine tıklandığında, geri göndermede tetiklenir olayıdır. Olay işleyicisi için aşağıdaki kodu ekleyin.

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample13.cs)]

Program aracılığıyla başvurarak kod başlatır `RoleNameLabel` Web, rol Sil düğmesine tıklanana satır denetiminde. `Roles.DeleteRole` Yöntemi sonra çağrılır, tümleştirilmesidir `Text` , `RoleNameLabel` ve `false`, böylece bağımsız olarak mı yoksa rolünü silme rolüyle ilişkilendirilen kullanıcı. Son olarak, `RoleList` GridView, böylece yalnızca silinen rol artık kılavuzda görünür yenilenir.

> [!NOTE]
> Rol Sil düğmesine rolü silmeden önce kullanıcıdan onay herhangi bir tür gerektirmez. Eylemi onaylamak için en kolay yollarından biri bir istemci-tarafı Onayla iletişim kutusudur. Bu teknik hakkında daha fazla bilgi için bkz: [ekleme istemci-tarafı onay olduğunda silme](https://asp.net/learn/data-access/tutorial-42-cs.aspx).


## <a name="summary"></a>Özet

Birçok web uygulamaları belirli yetkilendirme kuralları ya da yalnızca belirli kullanıcılar sınıfları için kullanılabilir sayfa düzeyinde işlevi vardır. Örneğin, bir dizi web sayfası, yalnızca yöneticiler erişebilir olabilir. Bir kullanıcı tarafından temelinde bu yetkilendirme kurallarını tanımlama yerine görmemeleri bir rolüne dayalı kurallar tanımlamak daha yararlı olacaktır. Diğer bir deyişle, açıkça Scott ve Jisun Yönetim web sayfaları erişmesine izin vermek yerine, bir daha rahat Yöneticiler rolünün üyeleri bu sayfaları erişmesine izin vermek için yaklaşımdır ve ardından Scott ve Jisun ait kullanıcı olarak belirtmek için Yöneticiler rolü.

Rolleri framework rollerini oluşturmak ve yönetmek kolay hale getirir. Biz bu öğreticide kullanmak üzere roller çerçevesini yapılandırmak nasıl incelenmesi `SqlRoleProvider`, rol deposu olarak bir Microsoft SQL Server veritabanı kullanır. Ayrıca sistemde mevcut rolleri listeler ve oluşturulacak yeni roller sağlayan bir web sayfası ve silinecek var olanları oluşturduk. Kullanıcıları rollere atama ve rol tabanlı yetkilendirme uygulamak nasıl sonraki öğreticilerde göreceğiz.

Mutluluk programlama!

### <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [ASP.NET 2.0'ın inceleniyor üyelik, roller ve profil](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Nasıl yapılır: ASP.NET 2.0 Rol Yöneticisi'ni kullanın](https://msdn.microsoft.com/library/ms998314.aspx)
- [Rol sağlayıcıları](https://msdn.microsoft.com/library/aa478950.aspx)
- [Kendi Web sitesi yönetim aracı alınıyor](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)
- [İçin teknik belgeler `<roleManager>` öğesi](https://msdn.microsoft.com/library/ms164660.aspx)
- [Üyelik ve Rol Yöneticisi API'ları kullanma](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/security/membership.aspx)

### <a name="about-the-author"></a>Yazar hakkında

Scott Mitchell, birden çok ASP/ASP.NET books yazar ve 4GuysFromRolla.com, kurucusu 1998 itibaren Microsoft Web teknolojileri ile çalışmaktadır. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri  *[kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Tan adresindeki ulaşılabilir [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) veya kendi blog aracılığıyla [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. Bu öğretici için sağlama gözden geçirenler Alicja Maziarz, Suchi Banerjee ve Teresa Murphy içerir. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bir satırında bana bırak [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Next](assigning-roles-to-users-cs.md)
