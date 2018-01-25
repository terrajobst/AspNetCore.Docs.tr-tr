---
uid: web-forms/overview/older-versions-security/roles/assigning-roles-to-users-vb
title: "(VB) kullanıcılara roller atama | Microsoft Docs"
author: rick-anderson
description: "Bu öğreticide biz hangi kullanıcıların hangi rollere ait yönetmeye yardımcı olmak üzere iki ASP.NET sayfaları oluşturacaksınız. İlk sayfasını görmek için tesis içerecektir..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/24/2008
ms.topic: article
ms.assetid: fd208ee9-69cc-4467-9783-b4e039bdd1d3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/roles/assigning-roles-to-users-vb
msc.type: authoredcontent
ms.openlocfilehash: c790f5f9b486b6598955459827c07ec9ad33ae38
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="assigning-roles-to-users-vb"></a>(VB) kullanıcılara roller atama
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indirme](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/VB.10.zip) veya [PDF indirin](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/aspnet_tutorial10_AssigningRoles_vb.pdf)

> Bu öğreticide biz hangi kullanıcıların hangi rollere ait yönetmeye yardımcı olmak üzere iki ASP.NET sayfaları oluşturacaksınız. Hangi kullanıcıların belirli bir role ait görmek için tesis ilk sayfasına ekleneceğini belirli bir kullanıcının ait olduğu için hangi rollerin ve atama veya belirli bir kullanıcının belirli bir rolden kaldırma yeteneği. Yeni oluşturulan kullanıcı ait hangi rolleri belirlemek üzere bir adımı içeren ikinci sayfasında biz CreateUserWizard denetim kullanmasıdır. Bu, bir yöneticinin yeni kullanıcı hesapları oluşturmak mümkün olduğu senaryolarda kullanışlıdır.


## <a name="introduction"></a>Giriş

<a id="_msoanchor_1"> </a> [Önceki öğretici](creating-and-managing-roles-vb.md) rolleri framework incelenmesi ve `SqlRoleProvider`; nasıl kullanılacağını gördüğümüz `Roles` oluşturmak, almak ve rollerini silmek için sınıf. Oluşturma ve rollerini silme ek olarak, biz atayın veya kullanıcılar bir rolden kaldırmak gerekir. Ne yazık ki, ASP.NET hangi kullanıcıların hangi rollere ait yönetmek için tüm Web denetimleri ile birlikte gelmez. Bunun yerine, biz bu ilişkileri yönetmek için kendi ASP.NET sayfaları oluşturmanız gerekir. İyi haber o ekleme ve kullanıcıları rollere kaldırma oldukça kolaydır. `Roles` Sınıfı bir veya daha fazla rol için bir veya daha fazla kullanıcı eklemek için yöntemler içerir.

Bu öğreticide biz hangi kullanıcıların hangi rollere ait yönetmeye yardımcı olmak üzere iki ASP.NET sayfaları oluşturacaksınız. Hangi kullanıcıların belirli bir role ait görmek için tesis ilk sayfasına ekleneceğini belirli bir kullanıcının ait olduğu için hangi rollerin ve atama veya belirli bir kullanıcının belirli bir rolden kaldırma yeteneği. Yeni oluşturulan kullanıcı ait hangi rolleri belirlemek üzere bir adımı içeren ikinci sayfasında biz CreateUserWizard denetim kullanmasıdır. Bu, bir yöneticinin yeni kullanıcı hesapları oluşturmak mümkün olduğu senaryolarda kullanışlıdır.

Haydi başlayalım!

## <a name="listing-what-users-belong-to-what-roles"></a>Hangi rollere ait hangi kullanıcıların listeleme

İlk iş sırası Bu öğretici için kendisinden rollere kullanıcıları atanabilir bir web sayfası oluşturmaktır. Kullanıcıları rollere atama ile biz kendisini ilgilendiren önce şimdi önce hangi kullanıcıların hangi rollere ait belirlemekte nasıl yoğunlaşabilirsiniz. Bu bilgileri görüntülemek için iki yolu vardır: "rol" veya "kullanıcı tarafından." Bir rol seçin ve ardından bunları ("rol tarafından" görüntüle) rolüne ait tüm kullanıcıları göster ziyaretçi izin verebilir veya bir kullanıcı seçin ve ardından o kullanıcıya ("kullanıcı tarafından" görüntüle) atanan rollerden göstermek için ziyaretçi sor.

"Rol" görünümü ziyaretçi burada belirli bir role ait kullanıcılar kümesini bilmek ister durumlarda kullanışlıdır. "kullanıcı" görünümü ziyaretçi belirli bir kullanıcının rollerini bilmeniz gerektiğinde idealdir. Hem "rol" ve "kullanıcı tarafından" dahil sayfamızı şimdi sahip arabirimler.

Biz "kullanıcı tarafından" arabirimi oluşturma ile başlar. Bu arabirim, aşağı açılan liste ve onay kutularını listesini oluşur. Aşağı açılan liste kullanıcılar kümesiyle sistemde doldurulur; onay kutularını rolleri numaralandırır. Bir kullanıcı aşağı açılan listeden seçerek kullanıcının ait olduğu bu rollerden kontrol eder. Sayfasını ziyaret kişi sonra denetleyebilir veya eklemek veya karşılık gelen rollerden Seçili kullanıcıyı kaldırmak için onay kutusunun işaretini kaldırın.

> [!NOTE]
> Bir açılır listesini kullanarak kullanıcı hesaplarını değil Web siteleri için ideal seçim olabilir yerde kullanıcı hesapları yüzlerce. Aşağı açılan liste seçeneklerden daha kısa bir listeden bir öğe seçmek bir kullanıcı izin vermek için tasarlanmıştır. Liste öğeleri sayısı arttıkça hızla yönetilmeleri olur. Potansiyel olarak çok sayıda kullanıcı hesaplarına sahip olan bir Web sitesi oluşturuyorsanız, disk belleğine alınabilir GridView veya listeleyen bir filtrelenebilir arabirimi ziyaretçi bir harf seçmenizi ister gibi bir alternatif kullanıcı arabirimini kullanarak isteyebilirsiniz ve ardından yalnızca Kullanıcı adı seçilen harfle başlayan kullanıcılarla gösterir.


## <a name="step-1-building-the-by-user-user-interface"></a>1. adım: "kullanıcı tarafından" kullanıcı arabirimi oluşturma

Açık `UsersAndRoles.aspx` sayfası. Sayfanın en üstünde adlı bir etiket Web denetimi ekleme `ActionStatus` ve temizleyin, `Text` özelliği. İletileri gibi görüntüleme gerçekleştirilen eylemler hakkında geri bildirim sağlamak için bu etiket kullanacağız, "kullanıcı Tito Yöneticiler rolüne eklenmiş olan" veya "Kullanıcı Jisun denetçiler rolünden kaldırılmıştır." Bunlar yapmak için iletileri göze etiketin ayarlamak `CssClass` "Önemli" özelliği.

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample1.aspx)]

Ardından, aşağıdaki CSS sınıf tanımına ekleme `Styles.css` stil sayfası:

[!code-css[Main](assigning-roles-to-users-vb/samples/sample2.css)]

Bu CSS tanımı büyük, kırmızı bir yazı tipi kullanarak etiketi görüntülemek için tarayıcıyı yönlendirir. Şekil 1, Visual Studio tasarımcısı aracılığıyla Bu etkiyi gösterir.


[![Etiketin CssClass özelliğini büyük, kırmızı yazı tipiyle sonuçları](assigning-roles-to-users-vb/_static/image2.png)](assigning-roles-to-users-vb/_static/image1.png)

**Şekil 1**: etiketin `CssClass` büyük, kırmızı yazı tipi özelliği sonuçlarında ([tam boyutlu görüntüyü görüntülemek için tıklatın](assigning-roles-to-users-vb/_static/image3.png))


Ardından, bir DropDownList sayfasına ekleme ayarlayın kendi `ID` özelliğine `UserList`ve kendi `AutoPostBack` özelliğinin True. Tüm kullanıcıların sistemde listelemek için bu DropDownList kullanacağız. Bu DropDownList MembershipUser nesnelerin bir koleksiyona bağlı. Görüntü MembershipUser nesnesi, kullanıcı adı özelliği (ve liste öğeleri değeri olarak kullanmak üzere) DropDownList istiyoruz olduğundan DropDownList's kümesi `DataTextField` ve `DataValueField` "UserName" özellikleri.

Adlı bir yineleyici DropDownList altında eklemek `UsersRoleList`. Bu yineleyici tüm rollerin sistemde onay kutularını bir dizi olarak listelenir. Yineleyici 's tanımlamak `ItemTemplate` aşağıdaki bildirim temelli biçimlendirme kullanarak:

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample3.aspx)]

`ItemTemplate` Biçimlendirme içeren adlı tek bir onay kutusu Web Denetim `RoleCheckBox`. CheckBox'ın `AutoPostBack` özelliği True olarak ayarlanmış ve `Text` özelliği bağlı `Container.DataItem`. Veri bağlama sözdizimi yalnızca nedeni `Container.DataItem` rolleri framework rol adları listesini bir dize dizisi olarak döndürür ve biz yineleyici bağlama Bu dize dizisi olduğundan olduğu. Bir veri Web denetimine bağlı bir dizinin içeriğini görüntülemek için bu sözdizimi neden kullanılır kapsamlı bir açıklama Bu öğretici kapsamında değildir. Bu konuyla ilgili daha fazla bilgi için başvurmak [skaler dizi bir veri Web denetime bağlama](http://aspnet.4guysfromrolla.com/articles/082504-1.aspx).

Bu noktada, "kullanıcı tarafından" arabiriminin bildirim temelli biçimlendirme aşağıdakine benzer görünmelidir:

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample4.aspx)]

Biz şimdi DropDownList kullanıcı hesaplarına kümesini ve yineleyici rollere kümesi bağlamak için kod yazmaya hazırsınız. Sayfa arka plan kodu sınıfında adlı bir yöntem ekleyin `BindUsersToUserList` ve başka adlı `BindRolesList`, aşağıdaki kodu kullanarak:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample5.vb)]

`BindUsersToUserList` Yöntemi sistemindeki tüm kullanıcı hesaplarını alır [ `Membership.GetAllUsers` yöntemi](https://msdn.microsoft.com/library/dy8swhya.aspx). Bu döndürür bir [ `MembershipUserCollection` nesne](https://msdn.microsoft.com/library/system.web.security.membershipusercollection.aspx), koleksiyonu olduğu [ `MembershipUser` örnekleri](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx). Bu koleksiyonu daha sonra bağlı `UserList` DropDownList. `MembershipUser` Koleksiyon gibi çeşitli özellikler, içerir, oluşma şekli örnekleri `UserName`, `Email`, `CreationDate`, ve `IsOnline`. Değerini görüntülemek için DropDownList istemek için `UserName` özelliği emin `UserList` DropDownList'ın `DataTextField` ve `DataValueField` özellikleri "UserName" ayarlandı.

> [!NOTE]
> `Membership.GetAllUsers` Yöntemi iki aşırı vardır: biri hiç giriş parametresi kabul eder ve tüm kullanıcıların döndüren, diğeri sayfa dizini ve sayfa boyutu tamsayı değerlerini alır ve yalnızca belirtilen kullanıcı alt kümesini döndürür. Büyük miktarda disk belleğine alınabilir bir kullanıcı arabirimi öğesi görüntülenmesini kullanıcı hesapları olduğunda, yalnızca kullanıcı hesaplarını yerine bunların tümünün kesin kısmı döndürdüğünden ikinci aşırı kullanıcıları daha verimli bir şekilde bir sayfadan için kullanılabilir.


`BindRolesToList` Yöntemi başlatır çağırarak `Roles` sınıfının [ `GetAllRoles` yöntemi](https://msdn.microsoft.com/library/system.web.security.roles.getallroles.aspx), sistem rollerini içeren bir dize dizisi döndürür. Bu dize dizisi sonra yineleyici bağlıdır.

Son olarak, biz sayfa ilk kez yüklendiğinde bu iki yöntem çağırmanız gerekir. Aşağıdaki kodu ekleyin `Page_Load` olay işleyicisi:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample6.vb)]

Bu kod yerinde bir tarayıcı aracılığıyla sayfasını ziyaret edin için bir dakikanızı ayırın; Ekranınızın Şekil 2'ye benzer görünmelidir. Tüm kullanıcı hesaplarını aşağı açılan listesinde ve altında her bir rol bir onay kutusu olarak görünen doldurulur. Ayarlarız çünkü `AutoPostBack` seçili kullanıcı değiştirme veya denetimi veya rol kaldırılırsa DropDownList ve özelliklerini onay kutularını true geri gönderimin neden olur. Bu eylemler işlemek için kod yazmaya henüz çünkü hiçbir eylem ancak, gerçekleştirilir. Biz, sonraki iki bölümde bu görevler üstesinden gelmek.


[![Kullanıcılar ve roller sayfasını görüntüler](assigning-roles-to-users-vb/_static/image5.png)](assigning-roles-to-users-vb/_static/image4.png)

**Şekil 2**: kullanıcılar ve roller sayfasını görüntüler ([tam boyutlu görüntüyü görüntülemek için tıklatın](assigning-roles-to-users-vb/_static/image6.png))


### <a name="checking-the-roles-the-selected-user-belongs-to"></a>Rolleri denetimi seçilen kullanıcıya ait

Sayfa ilk kez yüklendiğinde veya ziyaretçi aşağı açılan listeden yeni bir kullanıcı seçtiğinde güncelleştirmek ihtiyacımız `UsersRoleList`ait onay kutularını böylece yalnızca seçilen kullanıcı bu role aitse verilen rol checkbox denetlenir. Bunu gerçekleştirmek için adlı bir yöntem oluşturun `CheckRolesForSelectedUser` aşağıdaki kod ile:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample7.vb)]

Yukarıdaki kod, seçili kullanıcının kim olduğunu belirleyerek başlatır. Ardından rolleri sınıfının kullanır [ `GetRolesForUser(userName)` yöntemi](https://msdn.microsoft.com/library/system.web.security.roles.getrolesforuser.aspx) rolleri bir dize dizisi olarak belirtilen kullanıcının kümesi dönün. Ardından, yineleyici'nın öğelerini numaralandırılır ve her öğenin `RoleCheckBox` onay kutusunu programlı olarak başvurulur. Karşılık gelen için rol içinde içindeyse onay kutusu işaretliyse `selectedUsersRoles` dize dizisi.

> [!NOTE]
> `Linq.Enumerable.Contains(Of String)(...)` ASP.NET 2.0 sürümünü kullanıyorsanız, söz dizimi derlenmez. `Contains(Of String)` Yöntemi parçasıdır [LINQ Kitaplığı](http://en.wikipedia.org/wiki/Language_Integrated_Query), ASP.NET 3.5 için yeni olan. ASP.NET 2.0 sürümünü kullanmaya devam ediyorsanız kullanmak [ `Array.IndexOf(Of String)` yöntemi](https://msdn.microsoft.com/library/eha9t187.aspx) yerine.


`CheckRolesForSelectedUser` Yöntemi iki durumda da çağrılması gerekir: sayfa ilk kez yüklendiğinde ve her `UserList` DropDownList'ın seçili dizin değiştirildi. Bu nedenle, bu yöntemi çağırın `Page_Load` olay işleyicisi (çağrıları sonra `BindUsersToUserList` ve `BindRolesToList`). Ayrıca, olay işleyicisi DropDownList için 's oluşturmak `SelectedIndexChanged` olay ve buradan bu yöntemi çağırın.

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample8.vb)]

Yerinde şu kodla sayfanın tarayıcı yoluyla test edebilirsiniz. Ancak, bu yana `UsersAndRoles.aspx` sayfa şu anda eksik kullanıcıları rollere atama olanağı, hiçbir kullanıcı rolleri sahiptir. Bu kod çalışan word alabilir ve bunu daha sonra yapar olduğunu doğrulamak için kullanıcıları rollere biraz bekledikten sonra atamak için kullanılan arabirimi oluşturacağız veya kayıtlar ekleme kullanıcıları rollere el ile ekleyebilirsiniz `aspnet_UsersInRoles` bu functi test etmek için tablo Şimdi onality.

### <a name="assigning-and-removing-users-from-roles"></a>Atama ve kullanıcıları rollerden kaldırma

Ne zaman ziyaretçi ya da bir onay kutusu temizler `UsersRoleList` yineleyici ihtiyacımız eklemek veya seçili kullanıcı karşılık gelen rolünden kaldırmak için. CheckBox'ın `AutoPostBack` özelliği şu anda yineleyicideki bir onay kutusu işaretli veya işaretsiz zaman hangi geri gönderimin neden True olarak ayarlayın. Kısacası, olay işleyici onay kutusunu 's oluşturmak ihtiyacımız `CheckChanged` olay. Onay kutusu yineleyici denetiminde olduğundan, olay işleyici tesisat el ile eklemeniz gerekir. Başlangıç olay işleyicisi arka plandaki kod sınıfı olarak ekleyerek bir `Protected` yöntemi, şu şekilde:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample9.vb)]

Bir dakika içinde bu olay işleyicisi için kod yazma için döndürür. Ancak ilk Haydi tesisat işleme olay tamamlayın. Yineleyici'nın içinde onay kutusu gelen `ItemTemplate`, ekleme `OnCheckedChanged="RoleCheckBox_CheckChanged"`. Bu sözdiziminin bağlayan `RoleCheckBox_CheckChanged` olay işleyicisine `RoleCheckBox`'s `CheckedChanged` olay.

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample10.aspx)]

Bizim son tamamlamak için bir görevdir `RoleCheckBox_CheckChanged` olay işleyicisi. Bu onay kutusu örneği hangi rolü işaretli veya işaretsiz aracılığıyla bize bildiren çünkü olayı onay kutusu denetimini başvurarak başlatmak ihtiyacımız kendi `Text` ve `Checked` özellikleri. Seçilen kullanıcının kullanıcı adı ile birlikte bu bilgileri kullanarak, biz ekleme veya kullanıcı rolü kaldırma `Roles` sınıfının [ `AddUserToRole` ](https://msdn.microsoft.com/library/system.web.security.roles.addusertorole.aspx) veya [ `RemoveUserFromRole` yöntemi](https://msdn.microsoft.com/library/system.web.security.roles.removeuserfromrole.aspx).

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample11.vb)]

Yukarıdaki kod, program aracılığıyla aracılığıyla kullanıma olayı onay kutusunu başvurarak başlatır `sender` giriş parametresi. Onay kutusu işaretli değilse, Seçilen kullanıcıyı rolünden kaldırılır belirtilen rol, aksi takdirde eklenir. Her iki durumda da `ActionStatus` etiketi yalnızca gerçekleştirilen eylem özetleyen bir ileti görüntülenir.

Bu sayfa bir tarayıcı aracılığıyla kullanıma test etmek için bir dakikanızı ayırın. Kullanıcı Tito seçin ve ardından Tito hem yöneticilerin hem de Denetçiler rollere ekleyin.


[![Yöneticiler ve denetçiler rolleri Tito eklendi](assigning-roles-to-users-vb/_static/image8.png)](assigning-roles-to-users-vb/_static/image7.png)

**Şekil 3**: Yöneticiler ve denetçiler rollere Tito eklendi ([tam boyutlu görüntüyü görüntülemek için tıklatın](assigning-roles-to-users-vb/_static/image9.png))


Ardından, kullanıcı Bruce'a aşağı açılan listeden seçin. Geri gönderimin yoktur ve yineleyici'nın onay kutularını aracılığıyla güncelleştirilir `CheckRolesForSelectedUser`. Bruce'a henüz hiçbir role ait değil olduğundan, iki onay kutusu işaretli. Ardından, Bruce'a denetçiler role ekleyin.


[![Bruce'a denetçiler rolüne eklendi](assigning-roles-to-users-vb/_static/image11.png)](assigning-roles-to-users-vb/_static/image10.png)

**Şekil 4**: Bruce'a denetçiler rolüne eklendi ([tam boyutlu görüntüyü görüntülemek için tıklatın](assigning-roles-to-users-vb/_static/image12.png))


Daha fazla işlevselliğini doğrulamak için `CheckRolesForSelectedUser` yöntemi, Tito veya Bruce'a başka kullanıcı seçin. Bunlar hiçbir role ait değil belirten, onay kutularını nasıl otomatik olarak işaretli unutmayın. Tito için döndür. Hem yöneticilerin hem de Denetçiler onay kutularını denetlenmesi.

## <a name="step-2-building-the-by-roles-user-interface"></a>2. adım: "Tarafından rolleri" kullanıcı arabirimi oluşturma

Bu noktada biz "kullanıcılar tarafından" arabirimi tamamladınız ve "rolleri tarafından" arabirimi tackling başlatmaya hazırsınız demektir. "Rolleri tarafından" arabirimi kullanıcıdan aşağı açılan listeden bir rol seçin ve ardından GridView o role ait kullanıcılar kümesini görüntüler.

Başka bir DropDownList denetimine ekleme `UsersAndRoles.aspx page`. Bu bir yineleyici denetim altına yerleştirin, adlandırın `RoleList`ve kendi `AutoPostBack` özelliğinin True. Bu altında GridView ekleyin ve adını `RolesUserList`. Bu GridView, seçili role ait kullanıcılar listelenir. GridView's ayarlamak `AutoGenerateColumns` özelliğini false olarak ayarlayın, ızgaranın bir TemplateField eklemek `Columns` toplama ve kümesi kendi `HeaderText` "Kullanıcılar" özelliğine. TemplateField's tanımlamak `ItemTemplate` böylece databinding ifadesinin değerini görüntüler `Container.DataItem` içinde `Text` adlı bir etiket özelliği `UserNameLabel`.

Ekleme ve GridView yapılandırdıktan sonra "rol tarafından" arabiriminin bildirim temelli biçimlendirme aşağıdakine benzer görünmelidir:

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample12.aspx)]

Doldurmak ihtiyacımız `RoleList` sistem rollerinde kümesiyle DropDownList. Bunu başarmak için güncelleştirme `BindRolesToList` bağlar, böylece yöntemi tarafından döndürülen dize dizisi `Roles.GetAllRoles` yönteme `RolesList` DropDownList (yanı sıra `UsersRoleList` yineleyici).

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample13.vb)]

Son iki satırları `BindRolesToList` yöntemi için roller kümesi bağlamak için eklenmiştir `RoleList` DropDownList denetimi. Şekil 5 – sistem rolleri ile doldurulan bir açılır liste tarayıcısından görüntülendiğinde sonuç gösterir.


[![Rolleri RoleList DropDownList görüntülenir](assigning-roles-to-users-vb/_static/image14.png)](assigning-roles-to-users-vb/_static/image13.png)

**Şekil 5**: roller görüntülenir `RoleList` DropDownList ([tam boyutlu görüntüyü görüntülemek için tıklatın](assigning-roles-to-users-vb/_static/image15.png))


### <a name="displaying-the-users-that-belong-to-the-selected-role"></a>Seçilen Role ait kullanıcılar görüntüleme

Sayfa ilk ne zaman yüklendi veya yeni bir rolü zaman seçilir `RoleList` DropDownList, ihtiyacımız GridView o role ait olan kullanıcıların listesini görüntülemek. Adlı bir yöntem oluşturma `DisplayUsersBelongingToRole` aşağıdaki kodu kullanarak:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample14.vb)]

Bu yöntem seçili rolünden alarak başlatır `RoleList` DropDownList. Daha sonra kullanır [ `Roles.GetUsersInRole(roleName)` yöntemi](https://msdn.microsoft.com/library/system.web.security.roles.getusersinrole.aspx) bir dize dizisi, bu role ait kullanıcılar kullanıcı adları alınamadı. Bu dizi sonra bağlı `RolesUserList` GridView.

Bu yöntem iki durumlarda çağrılması gerekir: sayfa ilk kez yüklendiğinde ve ne zaman içinde seçilen rol `RoleList` DropDownList değişiklikleri. Bu nedenle, güncelleştirme `Page_Load` olay işleyicisi çağrısından sonra bu yöntem çağrılır böylece `CheckRolesForSelectedUser`. Ardından, bir olay işleyicisi oluşturun `RoleList`'s `SelectedIndexChanged` olayı ve buradan, çok bu yöntemi çağırın.

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample15.vb)]

Bu kod, yerinde `RolesUserList` GridView, seçili role ait kullanıcılarla görüntülemelidir. Şekil 6 gösterildiği gibi iki üyeleri denetçiler rolü oluşur: Bruce'a ve Tito.


[![GridView, seçili Role ait bu kullanıcılar listelenmiştir.](assigning-roles-to-users-vb/_static/image17.png)](assigning-roles-to-users-vb/_static/image16.png)

**Şekil 6**: GridView listeler bu kullanıcılar, ait seçili rol için ([tam boyutlu görüntüyü görüntülemek için tıklatın](assigning-roles-to-users-vb/_static/image18.png))


### <a name="removing-users-from-the-selected-role"></a>Seçili rolünden kullanıcıları kaldırma

Şimdi büyütmek `RolesUserList` GridView bir sütunu içeren "Kaldır" düğmeler. Belirli bir kullanıcı için "Kaldır" düğmesini tıklatarak bu rolden kaldırır.

GridView Sil düğmesine alan ekleyerek başlayın. Bu alan en dosyalanmış sol görünür ve değişiklik yapmak, `DeleteText` "Sil" (varsayılan) özelliğine "Kaldır".


[![Ekleme](assigning-roles-to-users-vb/_static/image20.png)](assigning-roles-to-users-vb/_static/image19.png)

**Şekil 7**: GridView için "Kaldır" düğmesi ekleme ([tam boyutlu görüntüyü görüntülemek için tıklatın](assigning-roles-to-users-vb/_static/image21.png))


"Kaldır" düğmesine tıklandığında geri gönderimin ensues ve GridView's `RowDeleting` olayı oluşturulur. Bu olay için bir olay işleyicisi oluşturun ve kullanıcı seçili rolünden kaldırır kod yazmanız gerekir. Olay işleyicisi oluşturun ve aşağıdaki kodu ekleyin:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample16.vb)]

Kod, seçili rol adı belirleyerek başlatır. Ardından, program aracılığıyla başvuruları `UserNameLabel` , "Kaldır" düğmesine tıklanana kaldırmak için kullanıcı adını belirlemek için satır denetiminden. Kullanıcı rolü için bir çağrı aracılığıyla kaldırılır `Roles.RemoveUserFromRole` yöntemi. `RolesUserList` Aracılığıyla bir ileti görüntülenir ve GridView sonra yenilendiğini `ActionStatus` etiket denetimini.

> [!NOTE]
> "Kaldır" düğmesini herhangi bir tür kullanıcı rolünden kaldırmadan önce kullanıcıdan onay gerektirmez. Belirli bir düzeyde kullanıcı onayı eklemenizi davet. Eylemi onaylamak için en kolay yollarından biri bir istemci-tarafı Onayla iletişim kutusudur. Bu teknik hakkında daha fazla bilgi için bkz: [ekleme istemci-tarafı onay olduğunda silme](https://asp.net/learn/data-access/tutorial-42-vb.aspx).


Şekil 8 kullanıcı Tito denetçiler grubundan kaldırdıktan sonra sayfada görüntülenir.


[![Ne yazık ki Tito artık bir yönetici değil](assigning-roles-to-users-vb/_static/image23.png)](assigning-roles-to-users-vb/_static/image22.png)

**Şekil 8**: ne yazık ki Tito artık bir denetleyicidir ([tam boyutlu görüntüyü görüntülemek için tıklatın](assigning-roles-to-users-vb/_static/image24.png))


### <a name="adding-new-users-to-the-selected-role"></a>Seçili rol için yeni kullanıcı ekleme

Seçili rolünden kullanıcıları kaldırma yanı sıra bu sayfaya ziyaretçi Ayrıca seçili rol için bir kullanıcı eklemek görebilmeniz gerekir. Seçili rol için bir kullanıcı eklemek için en iyi arabirimi kullanıcı hesaplarına sahip olmayı beklediğiniz sayısına bağlıdır. Web sitenizi birkaç düzine kullanıcı hesaplarını barındırmak veya daha küçükse, burada bir DropDownList kullanabilirsiniz. Olabilir, kullanıcı hesaplarını binlerce hesapları, belirli bir hesap için arama yoluyla sayfa veya kullanıcı hesapları başka bir şekilde filtrelemek için ziyaretçi izin veren bir kullanıcı arabirimi eklemek istersiniz.

Bu sayfa için kullanıcı hesaplarının sayısına bakılmaksızın sistemde çalışan çok basit bir arabirim kullanalım. Yani, seçili role eklemek istediği kullanıcının kullanıcı adı yazın ziyaretçi isteyen bir metin kutusu kullanacağız. Bu adda kullanıcı yok veya kullanıcı rolünün bir üyesi ise, bir ileti görüntüler `ActionStatus` etiketi. Ancak kullanıcı varsa ve rolünün bir üyesi değil, biz role ekleyin ve kılavuz yenileyin.

TextBox ve GridView düğmesini ekleyin. TextBox's ayarlamak `ID` için `UserNameToAddToRole` ve düğmenin ayarlayın `ID` ve `Text` özelliklerine `AddUserToRoleButton` ve "Kullanıcı için Rol Ekle", sırasıyla.

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample17.aspx)]

Ardından, oluşturun bir `Click` için olay işleyicisini `AddUserToRoleButton` ve aşağıdaki kodu ekleyin:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample18.vb)]

Kodda çoğunluğu `Click` olay işleyicisi çeşitli doğrulama denetimleri gerçekleştirir. Ziyaretçi bir kullanıcı tarafından sağlanan sağlar `UserNameToAddToRole` TextBox kullanıcı sistemde var ve bunlar zaten seçili role ait değil. Bunları herhangi biri başarısız işaretlerse, uygun bir mesaj görüntülenir `ActionStatus` ve olay işleyicisi çıkıldı. Tüm denetimler başarılı olursa kullanıcı rolüne eklenir `Roles.AddUserToRole` yöntemi. Metin kutusu ait aşağıdaki `Text` özelliği temizlendiğinde, GridView yenilenir ve `ActionStatus` etiket belirtilen kullanıcı için seçili rolü başarıyla eklendiğini belirten bir ileti görüntülenir.

> [!NOTE]
> Belirtilen kullanıcı zaten seçili rolüne ait olmadığını emin olmak için kullanırız [ `Roles.IsUserInRole(userName, roleName)` yöntemi](https://msdn.microsoft.com/library/system.web.security.roles.isuserinrole.aspx), gösteren bir Boole değeri döndürür olup olmadığını *kullanıcıadı* üyesi*roleName*. Bu yöntemde yeniden kullanacağız <a id="_msoanchor_2"> </a> [sonraki öğretici](role-based-authorization-vb.md) rol tabanlı yetkilendirme baktığınızda biz.


Bir tarayıcı aracılığıyla sayfasını ziyaret edin ve denetçiler rolünden seçin `RoleList` DropDownList. Geçersiz kullanıcı adı girmeyi deneyin – kullanıcı sistemde yok açıklayan bir ileti görürsünüz.


[![Mevcut olmayan kullanıcı Role eklenemiyor](assigning-roles-to-users-vb/_static/image26.png)](assigning-roles-to-users-vb/_static/image25.png)

**Şekil 9**: varolmayan kullanıcı rol ekleyemezsiniz ([tam boyutlu görüntüyü görüntülemek için tıklatın](assigning-roles-to-users-vb/_static/image27.png))


Artık geçerli bir kullanıcı eklemeyi deneyin. Bir tane Tito denetçiler rolünü yeniden ekleyin.


[![Tito yine bir denetleyicidir!](assigning-roles-to-users-vb/_static/image29.png)](assigning-roles-to-users-vb/_static/image28.png)

**Şekil 10**: Tito olan yine bir yönetici!  ([Tam boyutlu görüntüyü görüntülemek için tıklatın](assigning-roles-to-users-vb/_static/image30.png))


## <a name="step-3-cross-updating-the-by-user-and-by-role-interfaces"></a>3. adım: Arası güncelleştirme "kullanıcı" ve "rol" arabirimleri

`UsersAndRoles.aspx` Sayfası kullanıcıları ve rolleri yönetmek için iki farklı arabirimleri sunar. Şu anda bir arabiriminde yapılan bir değişikliği hemen diğer yansıtılmaz, mümkün olması için bu iki arabirim birbirinden bağımsız olarak davranır. Örneğin, sayfa ziyaretçiye denetçiler rolünden seçer düşünün `RoleList` Bruce'a ve Tito üyeleri listeler DropDownList. Ardından, ziyaretçi Tito gelen seçer `UserList` Yöneticiler ve denetçiler onay denetler DropDownList `UsersRoleList` yineleyici. Ziyaretçi sonra yineleyici yönetici rolünden temizler, Tito denetçiler rolünden kaldırılır, ancak bu değişiklik "rol tarafından" arabiriminde yansıtılmaz. Denetçiler rolünün bir üyesi olduğu GridView hala Tito gösterir.

Bu rol işaretli veya işaretsiz gelen olduğunda GridView yenilemek için ihtiyacımız sorunu gidermek için `UsersRoleList` yineleyici. Benzer şekilde, biz her bir kullanıcı kaldırılmış veya "rolü tarafından" arabiriminden role eklenen yineleyici yenilemeniz gerekir.

"Kullanıcı tarafından" arabirimi Yineleyicideki çağırarak yenilenir `CheckRolesForSelectedUser` yöntemi. "Rol tarafından" arabirimi değiştirilebilir `RolesUserList` GridView'ın `RowDeleting` olay işleyicisi ve `AddUserToRoleButton` düğmenin `Click` olay işleyicisi. Bu nedenle, çağırmak ihtiyacımız `CheckRolesForSelectedUser` bu yöntemlerin her biri yönteminden.

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample19.vb)]

Benzer şekilde, "rol tarafından" arabiriminde GridView çağırarak yenilenir `DisplayUsersBelongingToRole` yöntemi ve "kullanıcı tarafından" arabirimi aracılığıyla değiştirilmiş `RoleCheckBox_CheckChanged` olay işleyicisi. Bu nedenle, çağırmak ihtiyacımız `DisplayUsersBelongingToRole` bu olay işleyicisi yönteminden.

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample20.vb)]

Bu küçük kod değişikliklerle "kullanıcı" ve "rol" arabirimleri artık doğru arası güncelleştirme. Bunu doğrulamak için bir tarayıcı aracılığıyla sayfasını ziyaret edin ve Tito seçip denetçiler gelen `UserList` ve `RoleList` DropDownLists, sırasıyla. "Kullanıcı tarafından" arabirimi Yineleyicideki gelen Tito için denetçiler rol işaretini gibi Tito otomatik olarak "rolü tarafından" arabiriminde GridView kaldırılır olduğunu unutmayın. "Rol tarafından" arabiriminden denetçiler rolüne Tito otomatik olarak ekleme "kullanıcı tarafından" arabiriminde denetçiler onay kutusunu yeniden denetler.

## <a name="step-4-customizing-the-createuserwizard-to-include-a-specify-roles-step"></a>4. adım: bir "Rolleri belirtin" adımı eklemek için CreateUserWizard özelleştirme

İçinde <a id="_msoanchor_3"> </a> [ *kullanıcı hesapları oluşturma* ](../membership/creating-user-accounts-vb.md) CreateUserWizard Web denetimi yeni bir kullanıcı hesabı oluşturmak için bir arabirim sağlamak için nasıl kullanılacağını gördüğümüz öğretici. CreateUserWizard denetim iki yoldan biriyle kullanılabilir:

- Site, kendi kullanıcı hesabı oluşturmak ziyaretçi için bir yol olarak ve
- Bir araç olarak yöneticiler yeni hesapları oluşturmak için

İlk kullanım örneğindeki bir ziyaretçi siteye gelir ve sitesinde kaydetmek için kendi bilgilerini girme CreateUserWizard çıkışı doldurur. İkinci durumda, yönetici yeni bir hesap için başka bir kişiye oluşturur.

Başka bir kişi için bir yönetici tarafından bir hesap oluşturulduğunda, yeni kullanıcı hesabına ait hangi rolleri belirlemek üzere yöneticinin izin vermek faydalı olabilir. İçinde <a id="_msoanchor_4"> </a> [ *depolama* *ek kullanıcı bilgilerini* ](../membership/storing-additional-user-information-vb.md) ek ekleyerek CreateUserWizard özelleştirmeyi gördüğümüz Öğreticisi `WizardSteps`. Yeni kullanıcının rollerini belirtmek için CreateUserWizard için ek bir adım eklemek ne bakalım.

Açık `CreateUserWizardWithRoles.aspx` sayfasında ve adlı CreateUserWizard denetim ekleme `RegisterUserWithRoles`. Denetimin ayarlamak `ContinueDestinationPageUrl` özelliğine "~ / Default.aspx". Buradaki bir yönetici bu CreateUserWizard denetim yeni kullanıcı hesapları oluşturmak için kullanacağınız, denetimin ayarlı, çünkü `LoginCreatedUser` özelliğini false olarak ayarlayın. Bu `LoginCreatedUser` özelliği ziyaretçi otomatik olarak yeni oluşturulan kullanıcı oturum ve doğru olarak varsayılan olarak olup olmadığını belirtir. Bir yönetici yeni hesap oluşturduğunda kendisine kendisi imzalanmış tutmak istiyoruz çünkü biz False olarak ayarlayın.

Ardından, "Ekle/Kaldır `WizardSteps`..." seçenek CreateUserWizard kullanıcının akıllı etiketten ve yeni bir ekleme `WizardStep`, ayar kendi `ID` için `SpecifyRolesStep`. Taşıma `SpecifyRolesStep WizardStep` böylece "Oturum yukarı için yeni hesabınız" adımından sonra ancak "Tamamlandı" adım önce gelir. Ayarlama `WizardStep`'s `Title` özelliği "Rollere belirtin", kendi `StepType` özelliğine `Step`ve kendi `AllowReturn` özelliğini false olarak ayarlayın.


[![Ekleme](assigning-roles-to-users-vb/_static/image32.png)](assigning-roles-to-users-vb/_static/image31.png)

**Şekil 11**: "Rolleri belirtin" ekleme `WizardStep` CreateUserWizard için ([tam boyutlu görüntüyü görüntülemek için tıklatın](assigning-roles-to-users-vb/_static/image33.png))


Bu değişiklikten sonra CreateUserWizard'ın bildirim temelli biçimlendirme aşağıdaki gibi görünmelidir:

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample21.aspx)]

Rollerdeki "belirtin" `WizardStep`, adlandırılmış bir CheckBoxList eklemek `RoleList.` bu CheckBoxList kullanılabilir roller listesinden, hangi rolleri denetleme sayfasını ziyaret kişi etkinleştirme yeni oluşturulan kullanıcı ait.

Şu iki kodlama görevlerle sol: ilk biz doldurmanız gerekir `RoleList` ; sistem rolleriyle CheckBoxList ikinci olarak, kullanıcı "Tamamlandı" adım "Rol belirtin" adımdan taşıdığında seçili roller için oluşturulan kullanıcı eklemek ihtiyacımız. Biz ilk görevde gerçekleştirebileceğiniz `Page_Load` olay işleyicisi. Aşağıdaki başvurular programlı olarak kod `RoleList` ilk onay sayfasını ziyaret edin ve rolleri sistemde kendisine bağlar.

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample22.vb)]

Yukarıdaki kod tanıdık gelecektir. İçinde <a id="_msoanchor_5"> </a> [ *depolama* *ek kullanıcı bilgilerini* ](../membership/storing-additional-user-information-vb.md) iki kullandık öğretici `FindControl` Web denetimi başvurmak için deyimleri gelen bir özel içinde `WizardStep`. Ve rolleri CheckBoxList'in bağlar kod Bu öğreticide daha önce yapılmadı.

İkinci bir programlama görevi gerçekleştirmek için biz "Rolleri belirtin" adım zaman tamamlandı bilmeniz gerekir. CreateUserWizard sahip geri çağırma bir `ActiveStepChanged` ziyaretçi varlıklardan bir adım diğerine her zaman harekete olay. Burada kullanıcının "Tamamlandı" adım aşması durumunda belirleyebilirsiniz; Bu durumda, biz seçili rollere kullanıcı eklemeniz gerekir.

İçin bir olay işleyicisi oluşturun `ActiveStepChanged` olay ve aşağıdaki kodu ekleyin:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample23.vb)]

Kullanıcı yalnızca "Tamamlandı" adım ulaştı, olay işleyicisi öğeler numaralandırır `RoleList` CheckBoxList ve yeni oluşturulan kullanıcı seçili rollere atanır.

Bir tarayıcı aracılığıyla bu sayfasını ziyaret edin. CreateUserWizard ilk adımı, yeni kullanıcının kullanıcı adı, parola, e-posta ve diğer önemli bilgiler için ister standart "Oturum yukarı için yeni hesabınız" adım olacaktır. Wanda adlı yeni bir kullanıcı oluşturmak için gereken bilgileri girin.


[![Wanda adlı yeni bir kullanıcı oluşturun](assigning-roles-to-users-vb/_static/image35.png)](assigning-roles-to-users-vb/_static/image34.png)

**Şekil 12**: adlı yeni bir kullanıcı Wanda oluşturma ([tam boyutlu görüntüyü görüntülemek için tıklatın](assigning-roles-to-users-vb/_static/image36.png))


"Kullanıcı oluştur" düğmesine tıklayın. CreateUserWizard dahili olarak çağırır `Membership.CreateUser` yöntemi, yeni kullanıcı hesabı ve ardından sonraki adıma ilerler oluşturma "Roller'i belirtin." Sistem rolleri burada listelenir. Denetçiler onay kutusunu işaretleyin ve İleri'yi tıklatın.


[![Wanda denetçiler rolünün bir üyesi yapın](assigning-roles-to-users-vb/_static/image38.png)](assigning-roles-to-users-vb/_static/image37.png)

**Şekil 13**: Wanda denetçiler rolünün bir üyesi yapın ([tam boyutlu görüntüyü görüntülemek için tıklatın](assigning-roles-to-users-vb/_static/image39.png))


İleri'yi tıklatmadan neden geri gönderimin ve güncelleştirmeleri `ActiveStep` "Tamamlandı" adıma. İçinde `ActiveStepChanged` olay işleyicisi, en son oluşturulan kullanıcı hesabı denetçiler rolüne atanır. Bunu doğrulamak için dönmek `UsersAndRoles.aspx` sayfasında ve gelen denetçiler seçin `RoleList` DropDownList. Şekil 14 gösterildiği gibi denetçiler artık üç kullanıcılarının oluşur: Bruce'a, Tito ve Wanda.


[![Tüm denetçiler şunlardır: Bruce'a, Tito ve Wanda](assigning-roles-to-users-vb/_static/image41.png)](assigning-roles-to-users-vb/_static/image40.png)

**Şekil 14**: Bruce'a, Tito ve Wanda olan tüm denetçiler ([tam boyutlu görüntüyü görüntülemek için tıklatın](assigning-roles-to-users-vb/_static/image42.png))


## <a name="summary"></a>Özet

Rolleri framework belirli bir kullanıcının rollerini ve hangi kullanıcıların belirli bir role ait belirlemek için yöntemler hakkında bilgi almak için yöntemleri sunar. Ayrıca, çeşitli bir veya daha fazla rol için bir veya daha fazla kullanıcı eklemek ve kaldırmak için yöntemler vardır. Bu öğreticide biz yalnızca iki bu yöntemleri odaklanmış: `AddUserToRole` ve `RemoveUserFromRole`. Tek bir rol için birden çok kullanıcı eklemek ve birden fazla rol tek bir kullanıcıya atamak için tasarlanmış ek çeşitlemesi vardır.

Bu öğretici içerecek şekilde CreateUserWizard denetimini genişletme bir bakış da dahil bir `WizardStep` yeni oluşturulan kullanıcı rolleri belirtmek için. Bu tür bir adımı yönetici yeni kullanıcılar için kullanıcı hesapları oluşturma işlemini kolaylaştırmaya yardımcı olabilir.

Bu noktada biz nasıl oluşturulacağı ve rollerini silme ve ekleme ve kullanıcıların rollerini kaldırın gördünüz. Ancak, rol tabanlı yetkilendirme uygulamayı aramak henüz. İçinde <a id="_msoanchor_6"> </a> [öğretici aşağıdaki](role-based-authorization-vb.md) sayfa düzeyinde işlevselliği sınırlamak için şu anda oturum açmış kullanıcının rollerine dayalı olarak nasıl biz bir rol tarafından rol temelinde URL yetkilendirme kuralları tanımlama adresindeki de arar.

Mutluluk programlama!

### <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [ASP.NET Web sitesi yönetim aracına genel bakış](https://msdn.microsoft.com/library/ms228053.aspx)
- [ASP inceleniyor. NET'in üyelik, roller ve profil](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Kendi Web sitesi yönetim aracı alınıyor](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)

### <a name="about-the-author"></a>Yazar hakkında

Scott Mitchell, birden çok ASP/ASP.NET books yazar ve 4GuysFromRolla.com, kurucusu 1998 itibaren Microsoft Web teknolojileri ile çalışmaktadır. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri  *[kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Tan adresindeki ulaşılabilir [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) veya kendi blog aracılığıyla [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Özel teşekkürler...

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. Bu öğretici için sağlama İnceleme Teresa Murphy oluştu. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bir satırında bana bırak[mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Önceki](creating-and-managing-roles-vb.md)
[sonraki](role-based-authorization-vb.md)
