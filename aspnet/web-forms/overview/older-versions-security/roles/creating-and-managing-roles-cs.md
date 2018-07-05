---
uid: web-forms/overview/older-versions-security/roles/creating-and-managing-roles-cs
title: Rolleri (C#) oluşturma ve yönetme | Microsoft Docs
author: rick-anderson
description: Bu öğreticide, rolleri framework yapılandırmak için gerekli adımları inceler. Web sayfalarının oluşturun ve rollerini silme oluşturacaksınız.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/24/2008
ms.topic: article
ms.assetid: 113f10b3-a19a-471b-8ff6-db3c79ce8a91
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-security/roles/creating-and-managing-roles-cs
msc.type: authoredcontent
ms.openlocfilehash: af7f3a1c45f0e87f146e8fb1f3aff37a13847ba8
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37390795"
---
<a name="creating-and-managing-roles-c"></a>Rolleri (C#) oluşturma ve yönetme
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indir](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/CS.09.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/aspnet_tutorial09_CreatingRoles_cs.pdf)

> Bu öğreticide, rolleri framework yapılandırmak için gerekli adımları inceler. Web sayfalarının oluşturun ve rollerini silme oluşturacaksınız.


## <a name="introduction"></a>Giriş

İçinde <a id="_msoanchor_1"> </a> [ *kullanıcı tabanlı yetkilendirme* ](../membership/user-based-authorization-cs.md) sayfaları kümesinden belirli kullanıcılara kısıtlamak için URL yetkilendirmesi kullanarak baktığı ve bildirim temelli incelediniz öğretici ve ziyaret kullanıcıya bağlı olarak bir ASP.NET sayfasının işlevselliğini ayarlamak için programlama teknikleri. Sayfa erişim veya kullanıcı tarafından temelinde işlevselliği için izin verme, ancak bir bakım senaryolarda çok sayıda kullanıcı hesabının bulunduğu ya da kullanıcıların ayrıcalıkları genellikle değiştirdiğinizde kabus. Bir kullanıcı kazançları ya da belirli bir görevi gerçekleştirmek için yetkilendirme kaybeder yönetici uygun URL yetkilendirme kuralları, bildirim temelli işaretleme ve kod güncelleştirilmesi gerekiyor.

Genellikle kullanıcılar, gruplar halinde sınıflandırmak için yardımcı veya *rolleri* ve ardından bir rol rollü temelinde izinleri uygulamak için. Örneğin, çoğu web uygulaması belirli kümesi sayfaları veya yalnızca yönetici kullanıcılar için ayrılmış görevleri vardır. Teknikleri kullanarak öğrendiniz *kullanıcı tabanlı yetkilendirme* öğretici eklediğimiz uygun URL yetkilendirme kuralları, bildirim temelli biçimlendirme ve yönetim görevlerini gerçekleştirmek belirtilen kullanıcı hesaplarını izin verecek kod. Ancak yeni bir yönetici eklendiyse veya var olan bir yönetici iptal kendi yönetim haklarına sahip olmanız gerekiyorsa, biz dönün ve web sayfaları ve yapılandırma dosyalarını güncelleştirmek gerekir. Rolleri ile ancak biz Yöneticiler adlı bir rol oluşturun ve güvenilen kullanıcılarla Yöneticiler rolüne atayın. Ardından, uygun URL yetkilendirme kuralları, bildirim temelli biçimlendirme ve çeşitli yönetim görevlerini gerçekleştirmek Yöneticiler rolünün izin verecek kod ekleriz. Bu altyapı yerinde yeni yöneticiler ekleme veya var olanları kaldırma dahil olmak üzere veya kullanıcının Yöneticiler rolünden kaldırılması kadar basittir. Hiçbir yapılandırma, bildirim temelli işaretleme veya kod değişikliği gerekli değildir.

ASP.NET roller tanımlayarak ve bunları kullanıcı hesabıyla ilişkilendirmek için bir rol altyapı sunar. Rolleri çerçevesiyle oluşturabilmek ve rollerini silin veya kullanıcılar bir rolden kaldırmak için belirli bir role ait ve bir kullanıcının belirli role ait olup olmadığını bildirmek kullanıcılar belirlemek kullanıcılar ekleyin. Rolleri framework yapılandırıldıktan sonra biz URL yetkilendirme kuralları aracılığıyla rol rollü temelinde sayfalarına erişimi sınırlamak ve gösterebilir veya ek bilgiler veya o anda oturum açmış kullanıcı rollerine göre bir sayfa işlevselliğini gizleyebilirsiniz.

Bu öğreticide, rolleri framework yapılandırmak için gerekli adımları inceler. Web sayfalarının oluşturun ve rollerini silme oluşturacaksınız. İçinde <a id="_msoanchor_2"> </a> [ *kullanıcılara roller atama* ](assigning-roles-to-users-cs.md) öğretici size nasıl eklemek ve kullanıcıların rollerini kaldırmak görünür. Ve <a id="_msoanchor_3"> </a> [ *rol tabanlı yetkilendirme* ](role-based-authorization-cs.md) öğretici şu sayfa işlevselliği bağlı olarak ayarlamak nasıl birlikte rol rollü temelinde sayfalarına erişimi sınırlamak nasıl görürsünüz ziyaret kullanıcı rolü. Haydi başlayalım!

## <a name="step-1-adding-new-aspnet-pages"></a>1. adım: Yeni ASP.NET sayfaları ekleme

Bu öğretici ve sonraki iki biz çeşitli rolleri ile ilgili işlevler ve yetenekleri inceleniyor. ASP.NET sayfaları, Bu öğretici incelenirken konuları uygulamak için bir dizi ihtiyacımız. Şimdi bu sayfaları oluşturun ve site haritası güncelleştirin.

Adlı projede yeni bir klasör oluşturarak başlayın `Roles`. Ardından, dört yeni ASP.NET sayfaları ekleyin `Roles` klasörü, her bir sayfa ile bağlama `Site.master` ana sayfa. Sayfa adı:

- `ManageRoles.aspx`
- `UsersAndRoles.aspx`
- `CreateUserWizardWithRoles.aspx`
- `RoleBasedAuthorization.aspx`

Bu noktada, projenizin Çözüm Gezgini, Şekil 1'de gösterilen ekran şuna benzemelidir.


[![Dört yeni sayfa rolleri klasöre eklenen](creating-and-managing-roles-cs/_static/image2.png)](creating-and-managing-roles-cs/_static/image1.png)

**Şekil 1**: dört yeni sayfalar eklenmiştir `Roles` klasörü ([tam boyutlu görüntüyü görmek için tıklatın](creating-and-managing-roles-cs/_static/image3.png))


Her sayfanın bu noktada, iki içerik denetimlerini, her bir ana sayfanın ContentPlaceHolder biri olması gerekir: `MainContent` ve `LoginContent`.

[!code-aspx[Main](creating-and-managing-roles-cs/samples/sample1.aspx)]

Bu geri çağırma `LoginContent` ContentPlaceHolder'ın varsayılan biçimlendirme oturum açamayabilir veya kullanıcının kimliği doğrulanır olup olmadığına bağlı olarak site oturumunu bağlantısı görüntülenir. Varlığını `Content2` içerik ASP.NET sayfasındaki denetimi ana sayfanın varsayılan biçimlendirme ancak geçersiz kılar. Açıkladığımız gibi <a id="_msoanchor_4"> </a> [ *form kimlik doğrulaması bir genel bakış* ](../introduction/an-overview-of-forms-authentication-cs.md) Öğreticisi, varsayılan biçimlendirme geçersiz kılma değil istediğimiz görüntülemek için oturum açma ile ilgili sayfalarında yararlıdır Sol sütunda seçenekleri.

Bu dört sayfaları için ancak ana sayfa için varsayılan işaretlemesini göstermek istiyoruz `LoginContent` ContentPlaceHolder. Bu nedenle, bildirim temelli biçimlendirme için kaldırma `Content2` içerik denetimi. Bunu yaptıktan sonra her biri dört sayfanın biçimlendirme yalnızca bir içerik denetimi içermesi gerekir.

Son olarak, site haritası güncelleştirelim (`Web.sitemap`) bu yeni web sayfaları dahil etmek için. Sonra aşağıdaki XML ekleme `<siteMapNode>` üyelik öğreticileri için ekledik.

[!code-xml[Main](creating-and-managing-roles-cs/samples/sample2.xml)]

Site haritası ile güncelleştirilmiş bir tarayıcı aracılığıyla sitesini ziyaret edin. Şekil 2 gösterildiği gibi sol taraftaki gezinti artık rolleri öğreticileri için öğeleri içerir.


[![Dört yeni sayfa rolleri klasöre eklenen](creating-and-managing-roles-cs/_static/image5.png)](creating-and-managing-roles-cs/_static/image4.png)

**Şekil 2**: dört yeni sayfalar eklenmiştir `Roles` klasörü ([tam boyutlu görüntüyü görmek için tıklatın](creating-and-managing-roles-cs/_static/image6.png))


## <a name="step-2-specifying-and-configuring-the-roles-framework-provider"></a>2. adım: Belirtme ve rolleri Framework sağlayıcısını yapılandırma

Üyelik framework gibi rolleri framework sağlayıcı modeli yerleşik olarak bulunur. Bölümünde açıklandığı gibi <a id="_msoanchor_5"> </a> [ *temel güvenlik kavramları ve ASP.NET desteği* ](../introduction/security-basics-and-asp-net-support-cs.md) öğretici, .NET Framework üç yerleşik rol sağlayıcıları ile birlikte gelir: [ `AuthorizationStoreRoleProvider` ](https://msdn.microsoft.com/library/system.web.security.authorizationstoreroleprovider.aspx) , [ `WindowsTokenRoleProvider` ](https://msdn.microsoft.com/library/system.web.security.windowstokenroleprovider.aspx), ve [ `SqlRoleProvider` ](https://msdn.microsoft.com/library/system.web.security.sqlroleprovider.aspx). Bu öğretici serisinin odaklanır `SqlRoleProvider`, rol deposu bir Microsoft SQL Server veritabanı kullanır.

Aslında rolleri framework ve `SqlRoleProvider` üyelik framework gibi çalışır ve `SqlMembershipProvider`. .NET Framework'ü içeren bir `Roles` rolleri Framework API gören sınıf. `Roles` Sınıfında statik yöntemler gibi `CreateRole`, `DeleteRole`, `GetAllRoles`, `AddUserToRole`, `IsUserInRole`ve böyle devam eder. Aşağıdaki yöntemlerden birini çağrıldığında `Roles` sınıfı yapılandırılan sağlayıcı çağrısı atar. `SqlRoleProvider` Çalışır role özgü tabloları (`aspnet_Roles` ve `aspnet_UsersInRoles`) yanıt.

Kullanmak için `SqlRoleProvider` uygulamamız sağlayıcısı, biz ne deposu olarak kullanmak için veritabanı belirtmeniz gerekir. `SqlRoleProvider` Belirli veritabanı tablolarını, görünümlerini ve saklı yordamlar için belirtilen rol deposu bekliyor. Bu gerekli veritabanı nesnelerini kullanarak eklenebilir [ `aspnet_regsql.exe` aracı](https://msdn.microsoft.com/library/ms229862.aspx). Bir veritabanı için gerekli şema zaten bu noktada sahibiz `SqlRoleProvider`. Geri <a id="_msoanchor_6"> </a> [ *SQL Server'da üyelik şeması oluşturma* ](../membership/creating-the-membership-schema-in-sql-server-cs.md) adlı bir veritabanı oluşturduk öğretici `SecurityTutorials.mdf` ve kullanılan `aspnet_regsql.exe` uygulama eklemek için gerekli veritabanı nesnelerini dahil Hizmetler `SqlRoleProvider`. Bu nedenle yalnızca Rol desteğini etkinleştirmek ve kullanmak için rolleri framework bildirmek ihtiyacımız `SqlRoleProvider` ile `SecurityTutorials.mdf` veritabanı rol deposu olarak.

Rolleri framework aracılığıyla yapılandırılır &lt; `roleManager` &gt; uygulamanın öğesinde `Web.config` dosya. Varsayılan olarak, rol desteğini devre dışıdır. Bunu etkinleştirmek için ayarlamanız gerekir [ &lt; `roleManager` &gt; ](https://msdn.microsoft.com/library/ms164660.aspx) öğenin `enabled` özniteliğini `true` şu şekilde:

[!code-xml[Main](creating-and-managing-roles-cs/samples/sample3.xml)]

Varsayılan olarak, tüm web uygulamalarının adlı bir rol sağlayıcısı sahip `AspNetSqlRoleProvider` türü `SqlRoleProvider`. Bu varsayılan sağlayıcı kaydedilmiştir `machine.config` (konumundaki `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\CONFIG`):

[!code-xml[Main](creating-and-managing-roles-cs/samples/sample4.xml)]

Sağlayıcının `connectionStringName` kullanılan rol deposu özniteliği belirtir. `AspNetSqlRoleProvider` Sağlayıcısı bu öznitelik ayarlar `LocalSqlServer`, ayrıca tanımlanan `machine.config` ve varsayılan olarak bir SQL Server 2005 Express Edition veritabanına noktaları `App_Data` adlı klasöre `aspnet.mdf`.

Sonuç olarak, biz yalnızca rolleri framework bizim uygulamanın herhangi bir sağlayıcı bilgi belirtmeden etkinleştirirseniz `Web.config` dosya, uygulamayı kullanan kayıtlı varsayılan rol sağlayıcı `AspNetSqlRoleProvider`. Varsa `~/App_Data/aspnet.mdf` veritabanı yok, ASP.NET çalışma zamanı otomatik olarak oluşturun ve uygulama hizmetleri şeması ekleyin. Ancak biz kullanmak istemiyorsanız `aspnet.mdf` veritabanı; bunun yerine kullanılacak istiyoruz `SecurityTutorials.mdf` veritabanı biz zaten oluşturulan ve uygulama hizmetleri eklenir. Bu değişikliği iki yoldan biriyle gerçekleştirilebilir:

- <strong>İçin bir değer belirtin</strong><strong>`LocalSqlServer`</strong><strong>bağlantı dizesi adı</strong><strong>`Web.config`</strong><strong>.</strong> Üzerine tarafından `LocalSqlServer` bağlantı dizesi adı değerinin `Web.config`, kayıtlı varsayılan rol sağlayıcı kullanabiliriz (`AspNetSqlRoleProvider`) ve ile düzgün çalışmak `SecurityTutorials.mdf` veritabanı. Bu yöntem hakkında daha fazla bilgi için bkz. [Scott Guthrie](https://weblogs.asp.net/scottgu/)ait blog gönderisi [kullanım SQL Server 2000 veya SQL Server 2005 ASP.NET 2.0 uygulama hizmetlerini yapılandırma](https://weblogs.asp.net/scottgu/archive/2005/08/25/423703.aspx).
- <strong>Yeni bir kayıtlı sağlayıcı türü Ekle</strong><strong>`SqlRoleProvider`</strong><strong>ve yapılandırma,</strong><strong>`connectionStringName`</strong><strong>içinişaretedecekşekildeayarlama</strong> <strong>`SecurityTutorials.mdf`</strong> <strong>veritabanı.</strong> Önerilen ve kullanılan yaklaşım budur <a id="_msoanchor_7"> </a> [ *SQL Server'da üyelik şeması oluşturma* ](../membership/creating-the-membership-schema-in-sql-server-cs.md) öğretici ve bu, ı Bu öğreticide kullanacağınız yaklaşım.

Aşağıdaki rolleri yapılandırma biçimlendirme eklemek `Web.config` dosya. Bu işaretleme adlı yeni bir sağlayıcı kaydeder `SecurityTutorialsSqlRoleProvider`.

[!code-xml[Main](creating-and-managing-roles-cs/samples/sample5.xml)]

Yukarıdaki biçimlendirme tanımlar `SecurityTutorialsSqlRoleProvider` varsayılan sağlayıcı olarak (aracılığıyla `defaultProvider` özniteliğini `<roleManager>` öğesi). Ayrıca ayarlar `SecurityTutorialsSqlRoleProvider`'s `applicationName` ayarını `SecurityTutorials`, aynı olduğu `applicationName` üyelik sağlayıcısı tarafından kullanılan ayar (`SecurityTutorialsSqlMembershipProvider`). Burada gösterilmeyen sırada [ `<add>` öğesi](https://msdn.microsoft.com/library/ms164662.aspx) için `SqlRoleProvider` de içerebilir bir `commandTimeout` saniyeler içinde veritabanı zaman aşımı süresi belirtmek için özniteliği. Varsayılan değer 30'dur.

Bu yapılandırma biçimlendirme ile yerinde uygulamamız içinde rol işlevselliği kullanmaya başlamak hazırız.

> [!NOTE]
> Yukarıdaki yapılandırma biçimlendirme kullanılması gösterilmektedir &lt; `roleManager` &gt; öğenin `enabled` ve `defaultProvider` öznitelikleri. Rolleri framework kullanıcı tarafından olarak rol bilgilerini nasıl ilişkilendirir etkileyen diğer öznitelikleri vardır. Bu ayarları inceleyeceğiz <a id="_msoanchor_8"> </a> [ *rol tabanlı yetkilendirme* ](role-based-authorization-cs.md) öğretici.


## <a name="step-3-examining-the-roles-api"></a>3. adım: rol API'si İnceleme

Rolleri framework'ün işlevleri aracılığıyla sunulan [ `Roles` sınıfı](https://msdn.microsoft.com/library/system.web.security.roles.aspx), rol tabanlı işlemler gerçekleştirmeye yönelik on üç statik yöntemler içerir. Ne zaman şu oluşturma ve silme adımları 4 rollerinde konuları ve 6 kullanacağız [ `CreateRole` ](https://msdn.microsoft.com/library/system.web.security.roles.createrole.aspx) ve [ `DeleteRole` ](https://msdn.microsoft.com/library/system.web.security.roles.deleterole.aspx) ekleme veya bir rolü sistemden kaldırma yöntemleri.

Sistemdeki tüm rollerin listesini almak için kullanın [ `GetAllRoles` yöntemi](https://msdn.microsoft.com/library/system.web.security.roles.getallroles.aspx) (bkz. 5. adım). [ `RoleExists` Yöntemi](https://msdn.microsoft.com/library/system.web.security.roles.roleexists.aspx) belirtilen rolün var olup olmadığını gösteren bir Boole değeri döndürür.

Sonraki öğreticide kullanıcı rolleriyle ilişkilendirmek üzere nasıl inceleyeceğiz. `Roles` Sınıfın [ `AddUserToRole` ](https://msdn.microsoft.com/library/system.web.security.roles.addusertorole.aspx), [ `AddUserToRoles` ](https://msdn.microsoft.com/library/system.web.security.roles.addusertoroles.aspx), [ `AddUsersToRole` ](https://msdn.microsoft.com/library/system.web.security.roles.adduserstorole.aspx), ve [ `AddUsersToRoles` ](https://msdn.microsoft.com/library/system.web.security.roles.adduserstoroles.aspx) yöntemleri, bir veya daha fazla rol için bir veya daha fazla kullanıcı ekleyin. Kullanıcıların rollerini kaldırmak için [ `RemoveUserFromRole` ](https://msdn.microsoft.com/library/system.web.security.roles.removeuserfromrole.aspx), [ `RemoveUserFromRoles` ](https://msdn.microsoft.com/library/system.web.security.roles.removeuserfromroles.aspx), [ `RemoveUsersFromRole` ](https://msdn.microsoft.com/library/system.web.security.roles.removeusersfromrole.aspx), veya [ `RemoveUsersFromRoles` ](https://msdn.microsoft.com/library/system.web.security.roles.removeusersfromroles.aspx) yöntemleri.

İçinde <a id="_msoanchor_9"> </a> [ *rol tabanlı yetkilendirme* ](role-based-authorization-cs.md) öğretici biz programlı olarak göster veya gizle işlevleri şu anda oturum açmış kullanıcının rol tabanlı yolu konumunda görünür. Bunu gerçekleştirmek için kullanabiliriz `Role` sınıfın [ `FindUsersInRole` ](https://msdn.microsoft.com/library/system.web.security.roles.findusersinrole.aspx), [ `GetRolesForUser` ](https://msdn.microsoft.com/library/system.web.security.roles.getrolesforuser.aspx), [ `GetUsersInRole` ](https://msdn.microsoft.com/library/system.web.security.roles.getusersinrole.aspx), veya [ `IsUserInRole` ](https://msdn.microsoft.com/library/system.web.security.roles.isuserinrole.aspx) yöntemleri.

> [!NOTE]
> İstediğiniz zaman aşağıdaki yöntemlerden birini çağrılır, göz önünde bulundurun `Roles` sınıfı yapılandırılan sağlayıcı çağrısı atar. Bu örnekte, bu çağrı için gönderildiği anlamına gelir `SqlRoleProvider`. `SqlRoleProvider` Çağrılan yönteme dayalı olarak uygun veritabanı işlemini gerçekleştirir. Örneğin, kod `Roles.CreateRole("Administrators")` sonuçlanır `SqlRoleProvider` yürütme `aspnet_Roles_CreateRole` saklı yordamı, yeni bir kayıt ekleyen `aspnet_Roles` Yöneticiler adlı tablo.


Kullanarak bu öğreticinin geri kalanında görünür `Roles` sınıfın `CreateRole`, `GetAllRoles`, ve `DeleteRole` sistemde rolleri yönetmek için yöntemler.

## <a name="step-4-creating-new-roles"></a>4. adım: Yeni rol oluşturma

Rolleri rasgele grubu kullanıcıları için bir yol sunmak ve bu gruplandırma yetkilendirme kurallarını uygulamak için daha kullanışlı bir yöntem için en yaygın olarak kullanılır. Ancak rolleri bir yetkilendirme mekanizması kullanmak için size ilk uygulamada hangi rollerin mevcut tanımlamanız gerekir. Ne yazık ki, ASP.NET CreateRoleWizard denetim içermez. Uygun kullanıcı arabirimi oluşturmak ve kendimize rolleri API'yi çağırmak için ihtiyacımız olan yeni rol eklemek için. Güzel bir haberimiz var bunu yapmanın çok kolay olmasıdır.

> [!NOTE]
> Yoktur, ancak hiçbir CreateRoleWizard Web denetimi [ASP.NET Web sitesi yönetim aracı](https://msdn.microsoft.com/library/ms228053.aspx), görüntüleme ve yönetme, web uygulamanızın yapılandırma yardımcı olmak için tasarlanan yerel bir ASP.NET uygulaması olduğu. Ancak, büyük bir ASP.NET Web Sitesi Yönetim Aracı'nı fan için iki nedenden dolayı değilim. İlk olarak, biraz da önemlidir ve istenen için çok fazla kullanıcı deneyimini bırakır. İkinci olarak, ASP.NET Web Sitesi Yönetim Aracı'nı yalnızca yerel olarak çalışmak için Canlı site rollerinde uzaktan yönetmek ihtiyacınız varsa, kendi rol yönetimi web sayfaları oluşturmak olacağını anlamı tasarlanmıştır. Bu iki nedenden dolayı Bu öğretici ve sonraki, ASP.NET Web sitesi yönetim aracı üzerinde güvenmek yerine gerekli rol yönetim araçları, bir web sayfasındaki oluşturmaya odaklanır.


Açık `ManageRoles.aspx` sayfasını `Roles` klasör ve bir metin kutusu ve düğme Web Denetimi sayfaya ekleyin. TextBox denetiminin ayarlamak `ID` özelliğini `RoleName` ve düğmenin `ID` ve `Text` özelliklerine `CreateRoleButton` ve Rol Oluştur, sırasıyla. Bu noktada, bildirim temelli işaretleme, sayfanın aşağıdakine benzer görünmelidir:

[!code-aspx[Main](creating-and-managing-roles-cs/samples/sample6.aspx)]

Ardından, çift `CreateRoleButton` düğme denetimi oluşturmak için tasarımcıda bir `Click` olay işleyicisi ve ardından aşağıdaki kodu ekleyin:

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample7.cs)]

Yukarıdaki kod, girilen kırpılmış rol adı atayarak başlatır `RoleName` TextBox'a `newRoleName` değişkeni. Ardından, `Roles` sınıfın `RoleExists` yöntemi belirlemek için çağrılır rol `newRoleName` sistemde zaten mevcut. Rolü mevcut değilse bir çağrı aracılığıyla oluşturulur `CreateRole` yöntemi. Varsa `CreateRole` yöntemi sistemde zaten mevcut bir rol adı geçen bir `ProviderException` özel durumu oluşturulur. Kod rolü zaten çağırmadan önce sistemdeki bulunmadığından emin olmak için ilk denetler. Bu yüzden `CreateRole`. `Click` Olay işleyicisi sonucuna öğenizin `RoleName` metin kutusunun `Text` özelliği.

> [!NOTE]
> Ne olacağını merak kullanıcı herhangi bir değer girmezse `RoleName` metin. İçine geçirilen değer varsa `CreateRole` yöntemi `null` ya da boş bir dize, bir özel durum oluşturulur. Benzer şekilde, rol adı virgül içeriyorsa bir özel durum oluşturulur. Sonuç olarak, sayfa, kullanıcının bir rol girer ve herhangi bir virgül içermediğinden emin olmak için doğrulama denetimleri içermesi gerekir. Okuyucu için bir alıştırma olarak bırakın


Yöneticiler adlı bir rol oluşturalım. Ziyaret `ManageRoles.aspx` sayfasında bir tarayıcıdan, Yöneticiler, metin kutusuna bir metin yazın (bkz: Şekil 3) ve ardından Rol Oluştur düğmesine tıklayın.


[![Yöneticiler rolü oluşturun](creating-and-managing-roles-cs/_static/image8.png)](creating-and-managing-roles-cs/_static/image7.png)

**Şekil 3**: Yöneticiler rol oluşturabilir ([tam boyutlu görüntüyü görmek için tıklatın](creating-and-managing-roles-cs/_static/image9.png))


Ne olur? Bir geri gönderme gerçekleşir, ancak rol gerçekten olan hiçbir görsel ipucu yok sisteme eklendi. Bu sayfa görsel geri bildirim eklemek için adım 5'te güncelleştireceğiz. Şimdilik, ancak rol giderek oluşturulduğunu doğrulayabilirsiniz `SecurityTutorials.mdf` veritabanı ve verileri görüntüleme `aspnet_Roles` tablo. Şekil 4'te gösterildiği gibi `aspnet_Roles` yeni eklenen Yöneticiler rolleri için bir kayıt tablosu içerir.


[![Tablo aspnet_Roles Yöneticiler için bir satır vardır.](creating-and-managing-roles-cs/_static/image11.png)](creating-and-managing-roles-cs/_static/image10.png)

**Şekil 4**: `aspnet_Roles` tablolu bir satır için Yöneticiler ([tam boyutlu görüntüyü görmek için tıklatın](creating-and-managing-roles-cs/_static/image12.png))


## <a name="step-5-displaying-the-roles-in-the-system"></a>5. adım: sistemde rolleri görüntüleme

Github'dan genişletmek `ManageRoles.aspx` sistemde geçerli rollerin listesini eklenecek sayfa. Bunu gerçekleştirmek için sayfaya bir GridView denetimi ekleyin ve ayarlamak kendi `ID` özelliğini `RoleList`. Ardından, bir yöntem için sayfa arka plan kod sınıf adlı ekleyin `DisplayRolesInGrid` aşağıdaki kodu kullanarak:

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample8.cs)]

`Roles` Sınıfın `GetAllRoles` yöntemi döndürür tüm rollerin sistemde dize dizisi. Bu dize dizisi, ardından GridView'a bağlıdır. Sayfa ilk yüklendiğinde rollerin listesini GridView'a bağlamak için çağrılacak gerekiyor `DisplayRolesInGrid` sayfanın yönteminden `Page_Load` olay işleyicisi. Aşağıdaki kod, sayfa ilk ziyaret edildiğinde, ancak sonraki Geri göndermeler bu yöntemi çağırır.

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample9.cs)]

Bu kod bir yerde bir tarayıcı aracılığıyla sayfasını ziyaret edin. Şekil 5 gösterildiği gibi öğeyi etiketli tek sütunlu bir kılavuz görmeniz gerekir. Grid 4. adımda eklediğimiz yöneticileri rolü için bir satır içerir.


[![GridView rol tek bir sütunda görüntülenir.](creating-and-managing-roles-cs/_static/image14.png)](creating-and-managing-roles-cs/_static/image13.png)

**Şekil 5**: GridView tek bir sütunda rollerini görüntüler ([tam boyutlu görüntüyü görmek için tıklatın](creating-and-managing-roles-cs/_static/image15.png))


GridView öğesi olduğundan etiketli silmenizin sütun görüntüler GridView'ın `AutoGenerateColumns` özelliği otomatik olarak her bir özellik için bir sütun oluşturmak GridView neden olur (varsayılan) True olarak ayarlanır, `DataSource`. Bir dizi dizideki öğelerin, bu nedenle tek GridView sütunu gösteren tek bir özelliğe sahip.

GridView ile verileri görüntüleme, açıkça my sütunları tanımlamak yerine bunları GridView tarafından örtük olarak üretilir istemiyorum. Verilerin biçimlendirilmesi çok daha kolay sütunları açıkça tanımlanması, sütunları yeniden düzenleyebilir ve başka yaygın görevleri gerçekleştirebilirsiniz. Bu nedenle, böylece sütunlarını açıkça tanımlanmış GridView'ın bildirim temelli biçimlendirme güncelleştirelim.

GridView'ın ayarlayarak başlatın `AutoGenerateColumns` özelliğini False. Ardından, bir TemplateField kılavuza ekleme ayarlayın, `HeaderText` rolleri özelliğini ve yapılandırma kendi `ItemTemplate` dizinin içeriğini görüntüler. Bunu gerçekleştirmek için adlı bir etiket Web denetimi ekleme `RoleNameLabel` için `ItemTemplate` ve bağlama kendi `Text` özelliğini `Container.DataItem`.

Bu özellikler ve `ItemTemplate`ait içeriği bildirimli olarak veya GridView'ın alanları iletişim kutusu ve düzenleme şablonlarını arabirimi ayarlanabilir. İletişim kutusu alanları ulaşmak için akıllı etiket GridView'ın sütunları Düzenle bağlantısına tıklayın. Ardından, ayarlanacak otomatik oluştur alanların onay kutusunun işaretini kaldırın `AutoGenerateColumns` özelliği false olarak ve bir TemplateField GridView'a ekleme ayarı kendi `HeaderText` rolüne özelliği. Tanımlamak için `ItemTemplate`ait içeriği GridView'ın akıllı etiketten Şablonları Düzenle seçeneğini seçin. Bir etiket Web denetimi sürükleyin `ItemTemplate`ayarlayın, `ID` özelliğini `RoleNameLabel`ve veri bağlama ayarlarını yapılandırmak için kendi `Text` özelliği bağlı `Container.DataItem`.

İşiniz bittiğinde kullandığınız hangi yaklaşımı bağımsız olarak elde edilen tanımlayıcı biçimlendirme GridView'ın aşağıdakine benzer görünmelidir.

[!code-aspx[Main](creating-and-managing-roles-cs/samples/sample10.aspx)]

> [!NOTE]
> Veri bağlama söz dizimi kullanılarak dizinin içeriğini görüntülenen `<%# Container.DataItem %>`. Bu öğreticinin kapsamı dışındadır GridView'a bağlı bir dizinin içeriklerini görüntüleme neden bu söz dizimi kullanılır, kapsamlı bir açıklamasıdır. Bu konular hakkında daha fazla bilgi için başvurmak [veri Web denetimi için bir skaler dizi bağlama](http://aspnet.4guysfromrolla.com/articles/082504-1.aspx).


Şu anda `RoleList` sayfa ilk ziyaret edildiğinde GridView rollerin listesini yalnızca bağlı. Yeni bir rol eklendiğinde kılavuz yenilemek ihtiyacımız var. Bunu yapmak için güncelleştirme `CreateRoleButton` düğmenin `Click` BT'nin çağırır, böylece olay işleyicisi `DisplayRolesInGrid` yöntemi yeni bir rolü oluşturulur.

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample11.cs)]

Artık kullanıcı eklediğinde yeni bir rol `RoleList` GridView yeni eklenen rol geri gönderme üzerinde gösterir. rol başarıyla oluşturulduğunu görsel geri bildirim sağlama. Bunu açıklamak üzere; ziyaret `ManageRoles.aspx` sayfasında bir tarayıcıdan ve denetçiler adlı bir rol ekleyin. Rol Oluştur düğmesine tıklayarak, bağlı bir geri gönderme ardından ve kılavuz, Yöneticiler ve bunun yanı sıra yeni rol, denetçilere içerecek şekilde güncelleştirir.


[![Denetçiler rolü eklendi sahiptir.](creating-and-managing-roles-cs/_static/image17.png)](creating-and-managing-roles-cs/_static/image16.png)

**Şekil 6**: denetçiler rolüne sahip eklendi ([tam boyutlu görüntüyü görmek için tıklatın](creating-and-managing-roles-cs/_static/image18.png))


## <a name="step-6-deleting-roles"></a>6. adım: Rol siliniyor

Bu noktada kullanıcı yeni bir rol oluşturabilir ve gelen tüm var olan rolleri görüntülemek `ManageRoles.aspx` sayfası. Şimdi de rollerini silme açmasına imkan tanıyın. `Roles.DeleteRole` Yönteminin iki aşırı yüklemesi vardır:

- [`DeleteRole(roleName)`](https://msdn.microsoft.com/library/ek4sywc0.aspx) -bir rolü siler *roleName*. Rol, bir veya daha fazla üye içeriyorsa bir özel durum oluşturulur.
- [`DeleteRole(roleName, throwOnPopulatedRole)`](https://msdn.microsoft.com/library/38h6wf59.aspx) -bir rolü siler *roleName*. Varsa *throwOnPopulateRole* olduğu `true`, sonra rolü bir veya daha fazla üye içeriyorsa bir özel durum oluşturulur. Varsa *throwOnPopulateRole* olduğu `false`, rolü veya herhangi bir üyeyi içerip içermediğini silinirse. Dahili olarak `DeleteRole(roleName)` yöntem çağrılarını `DeleteRole(roleName, true)`.

`DeleteRole` Olursa yöntem bir özel durum da oluşturur *roleName* olduğu `null` ya da boş bir dize veya *roleName* virgül içerir. Varsa *roleName* sistemde mevcut değil `DeleteRole` sessiz bir şekilde, bir özel durum oluşturma olmadan başarısız oluyor.

Şimdi de GridView büyütmek `ManageRoles.aspx` dahil etmek için bir silme düğme, tıklandığında, seçili rolü siler. Alanları iletişim kutusuna gidip CommandField seçeneği altında bulunan bir Sil düğmesini ekleme GridView'a Sil düğmesini ekleyerek başlayın. Sil düğmesini en sol sütunu ve ayarlayın olun, `DeleteText` rolü silme özelliği.


[![RoleList GridView'a Sil düğmesi ekleme](creating-and-managing-roles-cs/_static/image20.png)](creating-and-managing-roles-cs/_static/image19.png)

**Şekil 7**: silme düğme eklemek `RoleList` GridView ([tam boyutlu görüntüyü görmek için tıklatın](creating-and-managing-roles-cs/_static/image21.png))


Sil düğmesini ekledikten sonra GridView'ın bildirim temelli biçimlendirme aşağıdakine benzer görünmelidir:

[!code-aspx[Main](creating-and-managing-roles-cs/samples/sample12.aspx)]

Ardından, GridView için ait bir olay işleyicisi oluşturun `RowDeleting` olay. Rolü Sil düğmesine tıklandığında, geri göndermede geçirilen olay budur. Olay işleyicisine aşağıdaki kodu ekleyin.

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample13.cs)]

Kod, programlı olarak başvuruda bulunarak başlatır `RoleNameLabel` Web, rolü Sil düğmesine tıklanana satır denetimi. `Roles.DeleteRole` Yöntemi ardından çağrılır, tümleştirilmesidir `Text` , `RoleNameLabel` ve `false`, böylece rol bağımsız olarak silme herhangi bir kullanıcı rolüyle ilişkilendirilen vardır. Son olarak, `RoleList` GridView tam olarak silinen rol artık kılavuzda görünmesi yenilenir.

> [!NOTE]
> Rolü Sil düğmesini her türlü rol silmeden önce kullanıcıdan onay gerektirmez. Eylemi onaylamak için en kolay yollarından biri bir istemci-tarafı Onayla iletişim kutusudur. Bu yöntem hakkında daha fazla bilgi için bkz. [ekleme istemci tarafı doğrulama zaman silme](https://asp.net/learn/data-access/tutorial-42-cs.aspx).


## <a name="summary"></a>Özet

Birçok web uygulamaları, belirli yetkilendirme kuralları veya yalnızca belirli kullanıcılara sınıfları için kullanılabilir sayfa düzeyi işlevi vardır. Örneğin, bir dizi web sayfası, yalnızca yöneticiler erişebilir olabilir. Bu yetkilendirme kuralları kullanıcı tarafından olarak tanımlamak yerine aktardığınızda genellikle bir rol tabanlı kurallar tanımlamak daha yararlı olacaktır. Diğer bir deyişle, kullanıcıların Scott ve Jisun Yönetim web sayfalarına erişmek için açıkça izin yerine, bir daha sürdürülebilir Yöneticileri rolünün üyeleri bu sayfalara erişmek için izin vermek için yaklaşımdır ve Scott ve Jisun ait kullanıcılar olarak belirtmek için Yöneticiler rolü.

Rolleri framework roller oluşturmak ve yönetmek kolay hale getirir. Biz bu öğreticide kullanmak üzere roller çerçevesini yapılandırmak nasıl incelenirken `SqlRoleProvider`, rol deposu bir Microsoft SQL Server veritabanı kullanır. Ayrıca, sistemde var olan rolleri listeler ve oluşturulacak yeni roller sağlayan bir web sayfası ve silinecek var olanları oluşturduk. Sonraki öğreticilerde, kullanıcıları rollere atama ve rol tabanlı yetkilendirme uygulama göreceğiz.

Mutlu programlama!

### <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide ele alınan konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [ASP.NET 2.0'ın İnceleme üyelik, roller ve profil](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Nasıl yapılır: ASP.NET 2.0 Rol Yöneticisi'ni kullanma](https://msdn.microsoft.com/library/ms998314.aspx)
- [Rol sağlayıcıları](https://msdn.microsoft.com/library/aa478950.aspx)
- [Kendi Web sitesi yönetim aracı alınıyor](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)
- [İçin teknik belgeler `<roleManager>` öğesi](https://msdn.microsoft.com/library/ms164660.aspx)
- [Üyelik ve rol Manager API'leri kullanma](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/security/membership.aspx)

### <a name="about-the-author"></a>Yazar hakkında

Scott Mitchell, birden çok ASP/ASP.NET Books yazar ve poshbeauty.com sitesinin 4GuysFromRolla.com, Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan  *[Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott, konumunda ulaşılabilir [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) veya kendi blog'da aracılığıyla [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici serisinde, birçok yararlı Gözden Geçiren tarafından gözden geçirildi. Bu öğretici için müşteri adayı gözden geçirenler Alicja Maziarz Suchi Banerjee ve Teresa Murphy içerir. Yaklaşan My MSDN makaleleri gözden geçirme ilgileniyor musunuz? Bu durumda, bir satır bana bırak [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Next](assigning-roles-to-users-cs.md)
