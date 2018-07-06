---
uid: web-forms/overview/older-versions-security/roles/assigning-roles-to-users-cs
title: (C#) kullanıcılara rol atama | Microsoft Docs
author: rick-anderson
description: Bu öğreticide iki ASP.NET sayfaları, hangi kullanıcıların hangi role ait yönetmeye yardımcı olmak için oluşturulacak. İlk sayfasını görmek için özellikleri içerecek...
ms.author: aspnetcontent
ms.date: 03/24/2008
ms.assetid: d522639a-5aca-421e-9a76-d73f95607f57
msc.legacyurl: /web-forms/overview/older-versions-security/roles/assigning-roles-to-users-cs
msc.type: authoredcontent
ms.openlocfilehash: f8573afc2fd5f12611f88f8bdad7e14389017808
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820136"
---
<a name="assigning-roles-to-users-c"></a>(C#) kullanıcılara rol atama
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indir](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/CS.10.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/aspnet_tutorial10_AssigningRoles_cs.pdf)

> Bu öğreticide iki ASP.NET sayfaları, hangi kullanıcıların hangi role ait yönetmeye yardımcı olmak için oluşturulacak. İlk sayfa tesislerde hangi kullanıcıların belirli bir role ait görmek için içerecektir belirli bir kullanıcıya ait hangi rollerin ve atama veya belirli bir kullanıcı belirli bir rolden kaldırma yeteneği. İkinci sayfasında yeni oluşturulan kullanıcının ait olduğu hangi rolleri belirtmek için bir adım içerir, böylece biz CreateUserWizard denetim genişletecektir. Bu, bir yöneticinin yeni kullanıcı hesapları oluşturmak mümkün olduğu senaryolarda kullanışlıdır.


## <a name="introduction"></a>Giriş

<a id="_msoanchor_1"> </a> [Önceki öğreticide](creating-and-managing-roles-cs.md) rolleri framework incelenir ve `SqlRoleProvider`; nasıl kullanılacağını gördüğümüz `Roles` sınıfı oluşturmak, almak ve rollerini silin. Oluşturma ve rollerini silme yanı sıra, atama veya kullanıcılar bir rolden kaldırmak için gerekir. Ne yazık ki, hangi kullanıcıların hangi role ait yönetmek için herhangi bir Web denetimleri ile ASP.NET gelmez. Bunun yerine, ki bu ilişkilendirmeleri yönetmek için kendi ASP.NET sayfaları oluşturmanız gerekir. Bu ekleme güzel bir haberimiz var olduğundan ve kullanıcıları rollere kaldırma oldukça kolaydır. `Roles` Sınıfı bir veya daha fazla rol için bir veya daha fazla kullanıcı eklemek için yöntemler içerir.

Bu öğreticide iki ASP.NET sayfaları, hangi kullanıcıların hangi role ait yönetmeye yardımcı olmak için oluşturulacak. İlk sayfa tesislerde hangi kullanıcıların belirli bir role ait görmek için içerecektir belirli bir kullanıcıya ait hangi rollerin ve atama veya belirli bir kullanıcı belirli bir rolden kaldırma yeteneği. İkinci sayfasında yeni oluşturulan kullanıcının ait olduğu hangi rolleri belirtmek için bir adım içerir, böylece biz CreateUserWizard denetim genişletecektir. Bu, bir yöneticinin yeni kullanıcı hesapları oluşturmak mümkün olduğu senaryolarda kullanışlıdır.

Haydi başlayalım!

## <a name="listing-what-users-belong-to-what-roles"></a>Hangi kullanıcıların listesi, hangi rollere aittir

İlk iş sırası Bu öğretici için kullanıcılar, rollerine atanabilir bir web sayfası oluşturmaktır. Biz kendimize kullanıcıları rollere atama ile ilgili önce şimdi ilk hangi kullanıcıların hangi role ait belirleme hakkında yoğunlaşabilirsiniz. Bu bilgileri görüntülemek için iki yolu vardır: "rolü" veya "kullanıcı tarafından." Biz bir rol seçin ve ardından bunları ("" rolüne göre görüntüle) rolüne ait tüm kullanıcıları göster ziyaretçi izin verebilir ya da biz ziyaretçi bir kullanıcı seçin ve sonra bu kullanıcıya ("kullanıcı" olarak görünen) atanan rollerin göstermek isteyebilir.

"Tarafından rolü" görünümü ziyaretçi burada belirli bir role ait kullanıcı kümesini bilmek ister durumlarda kullanışlıdır. "kullanıcı tarafından" görünümü, belirli bir kullanıcının rollerini bilmek ziyaretçi gerektiğinde idealdir. Şimdi sayfamızı hem "rolü" ve "kullanıcı tarafından" dahil olan arabirimler.

"Kullanıcı tarafından" arabirimi oluşturma ile başlayacağız. Bu arabirim, aşağı açılan liste ve onay kutularından oluşan bir liste oluşur. Açılır listede, sistemde kullanıcı kümesini doldurulur; onay kutularını rolleri numaralandırır. Aşağı açılan listeden bir kullanıcı seçerek kullanıcının ait olduğu rollerin kontrol eder. Sayfasını ziyaret ederek kişi daha sonra denetleyin veya eklemek veya karşılık gelen rollerden Seçilen kullanıcıyı kaldırmak için onay kutusunun işaretini kaldırın.

> [!NOTE]
> Bir açılan listesini kullanarak kullanıcı hesaplarını değil Web siteleri için ideal bir tercih olabilir burada yüzlerce kullanıcı hesapları. Bir açılır liste seçenekleri görece kısa bir listeden bir öğe bir kullanıcı izin vermek için tasarlanmıştır. Liste öğeleri sayısı arttıkça hızlı bir şekilde kullanışsız olur. Potansiyel olarak çok sayıda kullanıcı hesapları olan bir Web sitesi oluşturuyorsanız alınabilir GridView veya listeleyen bir filtrelenebilir arabirimi ziyaretçi harf seçmenizi ister gibi bir kullanıcı arabirimi aracılığıyla isteyebilirsiniz ve ardından yalnızca Kullanıcı adı seçili harfle başlar bu kullanıcıları gösterir.


## <a name="step-1-building-the-by-user-user-interface"></a>1. adım: "kullanıcı tarafından" kullanıcı arabirimi oluşturma

Açık `UsersAndRoles.aspx` sayfası. Sayfanın üst kısmında adlı bir etiket Web denetimi ekleme `ActionStatus` ve temizleyin, `Text` özelliği. Görüntüleme gibi iletiler, gerçekleştirilecek eylemleri geri bildirim sağlamak için bu etiketi kullanacağız, "kullanıcı Tito Yöneticiler rolüne eklendi" veya "Kullanıcı Jisun denetçiler rolden kaldırıldı." Bunları yapmak için çıkış iletileri bekleme etiketin `CssClass` "Önemli" özelliği.

[!code-aspx[Main](assigning-roles-to-users-cs/samples/sample1.aspx)]

Ardından, aşağıdaki CSS sınıf tanımına ekleyin `Styles.css` stil sayfası:

[!code-css[Main](assigning-roles-to-users-cs/samples/sample2.css)]

Bu CSS tanımı tarayıcıya büyük, kırmızı bir yazı tipi kullanarak etiket bildirir. Şekil 1, Visual Studio tasarımcısı aracılığıyla Bu etkiyi gösterir.


[![Etiketin CssClass özellik büyük, kırmızı bir yazı tipinde sonuçları](assigning-roles-to-users-cs/_static/image2.png)](assigning-roles-to-users-cs/_static/image1.png)

**Şekil 1**: etiketin `CssClass` büyük, kırmızı yazı tipi özellik sonuçlarında ([tam boyutlu görüntüyü görmek için tıklatın](assigning-roles-to-users-cs/_static/image3.png))


Bir DropDownList sayfaya ekleyin. ardından, ayarlayın, `ID` özelliğini `UserList`ve onun `AutoPostBack` özelliği true. Tüm kullanıcıların sistemde listelemek için bu DropDownList kullanacağız. Bu DropDownList MembershipUser nesnelerin bir koleksiyona bağlı. Görüntü UserName özelliği MembershipUser nesnesi (ve liste öğeleri değeri olarak kullanmak için) DropDownList istediğimizden DropDownList'ın ayarlamak `DataTextField` ve `DataValueField` "UserName" özellikleri.

Adlı bir yineleyici DropDownList altında ekleme `UsersRoleList`. Bu Repeater tüm rollerin sistemde onay kutularını bir dizi olarak listelenir. Repeater'ın tanımlamak `ItemTemplate` aşağıdaki bildirim temelli biçimlendirme kullanma:

[!code-aspx[Main](assigning-roles-to-users-cs/samples/sample3.aspx)]

`ItemTemplate` Biçimlendirme adlı tek bir onay kutusu Web denetimi içeren `RoleCheckBox`. CheckBox'ın `AutoPostBack` özelliği True olarak ayarlanır ve `Text` özelliği bağlı `Container.DataItem`. Nedeni bağlama söz dizimi teknolojidir `Container.DataItem` rolleri framework rol adları listesini bir dize dizisi olarak döndürür ve biz yineleyici için bağlama Bu dize dizisi olan olmasıdır. Bu öğreticinin kapsamı dışındadır neden bu söz dizimi veri Web denetimi için bir dizi içeriğini görüntülemek için kullanılan kapsamlı bir açıklamasıdır. Bu konular hakkında daha fazla bilgi için başvurmak [veri Web denetimi için bir skaler dizi bağlama](http://aspnet.4guysfromrolla.com/articles/082504-1.aspx).

Bu noktada, "kullanıcı tarafından" arabiriminin bildirim temelli biçimlendirme aşağıdakine benzer görünmelidir:

[!code-aspx[Main](assigning-roles-to-users-cs/samples/sample4.aspx)]

Artık kullanıcı hesaplarına DropDownList kümesini ve yineleyici için roller kümesini bağlamak için kod yazmaya hazırız. Sayfa arka plan kod sınıfında adlı bir yöntem ekleyin `BindUsersToUserList` ve başka adlı `BindRolesList`, aşağıdaki kodu kullanarak:

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample5.cs)]

`BindUsersToUserList` Yöntemi sistemdeki tüm kullanıcı hesaplarını alır [ `Membership.GetAllUsers` yöntemi](https://msdn.microsoft.com/library/dy8swhya.aspx). Bu döndürür bir [ `MembershipUserCollection` nesne](https://msdn.microsoft.com/library/system.web.security.membershipusercollection.aspx), koleksiyonu olduğu [ `MembershipUser` örnekleri](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx). Bu koleksiyonu daha sonra bağlı `UserList` DropDownList. `MembershipUser` Koleksiyon gibi özellikler, çeşitli içerir, düzenini örnekler `UserName`, `Email`, `CreationDate`, ve `IsOnline`. Değerini görüntülemek için DropDownList isteyin için `UserName` özelliği emin `UserList` DropDownList'ın `DataTextField` ve `DataValueField` özellikleri "UserName" için ayarlandı.

> [!NOTE]
> `Membership.GetAllUsers` Yönteminin iki aşırı yüklemesi vardır: biri, hiç giriş parametresi kabul eden ve tüm kullanıcıları döndürür ve sayfa dizini ve sayfa boyutu tamsayı değerlerini alır ve kullanıcı için belirtilen alt döndürür. Kullanıcı hesaplarını alınabilir kullanıcı arabirimi öğesinde görüntülenen büyük miktarlarda olduğunda, yalnızca kullanıcı hesapları yerine bunların tümünde tam kümesini döndürdüğünden ikinci aşırı yükleme sayfasına kullanıcılar ile daha verimli bir şekilde kullanılabilir.


`BindRolesToList` Yöntemi çağrılarak başlatılır `Roles` sınıfın [ `GetAllRoles` yöntemi](https://msdn.microsoft.com/library/system.web.security.roles.getallroles.aspx), sistemde rolleri içeren dize dizisi döndürür. Bu dize dizisi, ardından yineleyici için bağlıdır.

Son olarak, sayfa ilk yüklendiğinde, bu iki yöntem çağırmak ihtiyacımız var. Aşağıdaki kodu ekleyin `Page_Load` olay işleyicisi:

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample6.cs)]

Yerinde bu kodu ile bir tarayıcı aracılığıyla sayfayı ziyaret etmek için birkaç dakikanızı; Ekranınız, Şekil 2'ye benzer görünmelidir. Tüm kullanıcı hesaplarını aşağı açılan listesinde ve altında her bir rol onay kutusu görünür doldurulur. Biz ayarlandığından `AutoPostBack` DropDownList ve onay kutularını seçili kullanıcı değiştirme veya bir rolü işaretini denetimi özellikleri true geri göndermeye neden olur. Bu eylemler işlemek üzere kod yazmak henüz çünkü hiçbir eylem ancak gerçekleştirilir. Biz, sonraki iki bölümde bu görevler üstesinden.


[![Kullanıcılar ve roller sayfasını görüntüler](assigning-roles-to-users-cs/_static/image5.png)](assigning-roles-to-users-cs/_static/image4.png)

**Şekil 2**: Kullanıcıları ve rolleri sayfası görüntüler ([tam boyutlu görüntüyü görmek için tıklatın](assigning-roles-to-users-cs/_static/image6.png))


### <a name="checking-the-roles-the-selected-user-belongs-to"></a>Rolleri denetimi seçili kullanıcıya ait

Sayfa ilk yüklendiğinde ya da ziyaretçi aşağı açılan listeden yeni bir kullanıcı seçtiğinde, güncelleştirilecek ihtiyacımız `UsersRoleList`ait onay kutularını böylece verilen rol onay kutusu yalnızca, seçili kullanıcının bu role aitse denetlenir. Bunu yapmak için adında bir yöntem oluşturma `CheckRolesForSelectedUser` aşağıdaki kod ile:

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample7.cs)]

Yukarıdaki kod, seçili kullanıcının kim olduğunu belirleyerek başlatır. Ardından rolleri sınıfın kullanır [ `GetRolesForUser(userName)` yöntemi](https://msdn.microsoft.com/library/system.web.security.roles.getrolesforuser.aspx) rolleri bir dize dizisi olarak belirtilen kullanıcının kümesini döndürmek için. Ardından, Yineleyicinin'ın öğeler numaralandırılır ve her öğenin `RoleCheckBox` onay kutusu programlı olarak başvuruluyor. Yalnızca, karşılık gelen rol içinde yer alıyorsa, onay kutusu işaretlenmiş `selectedUsersRoles` dize dizisi.

> [!NOTE]
> `selectedUserRoles.Contains<string>(...)` ASP.NET 2.0 sürümünü kullanıyorsanız, söz dizimi derlenmez. `Contains<string>` Yöntemi parçasıdır [LINQ Kitaplığı](http://en.wikipedia.org/wiki/Language_Integrated_Query), ASP.NET 3.5 için yeni olan. Yine de ASP.NET 2.0 sürümünü kullanıyorsanız, [ `Array.IndexOf<string>` yöntemi](https://msdn.microsoft.com/library/eha9t187.aspx) yerine.


`CheckRolesForSelectedUser` Yöntemi iki durumda çağrılması gerekir: sayfa ilk yüklendiğinde ve her `UserList` DropDownList'ın seçili dizin değiştirilir. Bu nedenle, bu yöntemi çağırın `Page_Load` olay işleyicisi (çağrıları sonra `BindUsersToUserList` ve `BindRolesToList`). Ayrıca, bir olay işleyicisi DropDownList için 's oluşturma `SelectedIndexChanged` olay ve burada bu yöntemi çağırın.

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample8.cs)]

Bu kod bir yerde sayfası tarayıcıda test edebilirsiniz. Ancak, bu yana `UsersAndRoles.aspx` sayfası, kullanıcıları rollere atama özelliği şu anda eksik, hiçbir kullanıcı rolleri sahiptir. Bu kod çalışan word alabilir ve bunu daha sonra yapar olduğunu doğrulamak için kullanıcıları rollere de atama arabirimi oluşturacağız veya kayıtlarını ekleyerek kullanıcıların rollere el ile ekleyebilirsiniz `aspnet_UsersInRoles` bu functi test etmek için tablo Şimdi onality.

### <a name="assigning-and-removing-users-from-roles"></a>Atama ve kullanıcılar rollerinden kaldırılıyor

Ne zaman ziyaretçi denetler veya onay kutusu temizler `UsersRoleList` ihtiyacımız eklemek veya seçili kullanıcı karşılık gelen rolünden kaldırmak için bir yineleyici. CheckBox'ın `AutoPostBack` özelliği şu anda Repeater onay kutusu işaretli veya işaretsiz her durumda geri göndermeye neden True olarak ayarlanır. Kısacası, onay kutusunu ait bir olay işleyicisi oluşturmak ihtiyacımız `CheckChanged` olay. Yineleyici denetiminde onay aşamasında olduğundan, olay işleyicisi ayarlamaları el ile eklemeniz gerekir. Başlangıç olay işleyicisi arka plan kod sınıfı olarak ekleyerek bir `protected` yöntemi şu şekilde:

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample9.cs)]

Birazdan bu olay işleyicisi için kod yazma için döndürür. Ancak ilk olay tesisat işleme şimdi tamamlayın. Repeater'ın içinde onay gelen `ItemTemplate`, ekleme `OnCheckedChanged="RoleCheckBox_CheckChanged"`. Bu söz dizimi bağlayan `RoleCheckBox_CheckChanged` olay işleyicisine `RoleCheckBox`'s `CheckedChanged` olay.

[!code-aspx[Main](assigning-roles-to-users-cs/samples/sample10.aspx)]

Bizim son görev tamamlamaktır `RoleCheckBox_CheckChanged` olay işleyicisi. Bu onay kutusu örneği hangi rolü işaretli veya işaretsiz aracılığıyla bize gösterir çünkü olayı başlatan bir CheckBox denetimi başvurarak başlatmak ihtiyacımız kendi `Text` ve `Checked` özellikleri. Seçilen kullanıcının kullanıcı adı ile birlikte bu bilgileri kullanarak, biz ekleyip kullanıcı rolünden `Roles` sınıfın [ `AddUserToRole` ](https://msdn.microsoft.com/library/system.web.security.roles.addusertorole.aspx) veya [ `RemoveUserFromRole` yöntemi](https://msdn.microsoft.com/library/system.web.security.roles.removeuserfromrole.aspx).

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample11.cs)]

Yukarıdaki kod aracılığıyla kullanılabilir olayı tetikleyen onay programlı olarak başvuruda bulunarak başlar `sender` giriş parametresi. Onay kutusu işaretli değilse, Seçilen kullanıcıyı rolden belirtilen rol, aksi takdirde eklenir. Her iki durumda da `ActionStatus` etiketi, az önce gerçekleştirdiğiniz eylemin özetleyen bir ileti görüntülenir.

Bu sayfa bir tarayıcı aracılığıyla kullanıma test etmek için bir dakikanızı ayırın. Kullanıcı Tito seçin ve ardından Tito hem yöneticilerin hem de Denetçiler rollere ekleyin.


[![Denetçiler roller ve yöneticiler Tito eklendi](assigning-roles-to-users-cs/_static/image8.png)](assigning-roles-to-users-cs/_static/image7.png)

**Şekil 3**: Tito Yöneticiler veya denetçiler rolleri eklendi ([tam boyutlu görüntüyü görmek için tıklatın](assigning-roles-to-users-cs/_static/image9.png))


Ardından, kullanıcı Bruce aşağı açılan listeden seçin. Bir geri gönderme yoktur ve Repeater'ın onay kutularını aracılığıyla güncelleştirilir `CheckRolesForSelectedUser`. Bruce henüz hiçbir role ait değil olduğundan, iki onay kutusu işaretlenmemiştir. Ardından, Bruce denetçiler role ekleyin.


[![Bruce denetçiler Role eklendi](assigning-roles-to-users-cs/_static/image11.png)](assigning-roles-to-users-cs/_static/image10.png)

**Şekil 4**: Bruce denetçiler rolüne eklendi ([tam boyutlu görüntüyü görmek için tıklatın](assigning-roles-to-users-cs/_static/image12.png))


Daha fazla işlevselliğini doğrulamak için `CheckRolesForSelectedUser` yöntemi, bir kullanıcı Tito veya Bruce dışındaki seçin. Onay kutularını nasıl otomatik olarak işaretlenmemiş olduğuna dikkat edin, belirten bunlar hiçbir role ait değil. Tito için döndürür. Hem Yöneticiler hem de Denetçiler onay kutularını denetlenmelidir.

## <a name="step-2-building-the-by-roles-user-interface"></a>2. adım: "Tarafından rolleri" kullanıcı arabirimi oluşturma

Bu noktada biz "kullanıcılar tarafından" arabirimi tamamladınız ve "tarafından rolleri" arabirimi bağlayabileceğiniz başlamaya hazırsınız. "Tarafından rolleri" arabirimi kullanıcıdan aşağı açılan listeden bir rol seçin ve sonra ilgili rolde GridView ait kullanıcı kümesini görüntüler.

Başka bir DropDownList denetimine ekleme `UsersAndRoles.aspx` sayfası. Bunu Repeater denetiminde altındaki yerleştirmek, adlandırın `RoleList`ve onun `AutoPostBack` özelliği true. Altında GridView ekleyin ve adlandırın `RolesUserList`. Bu GridView seçili role ait olan kullanıcıları listeler. GridView'ın ayarlayın `AutoGenerateColumns` özelliği false olarak Ekle bir TemplateField ızgaranın `Columns` toplama ve kümesi kendi `HeaderText` "Kullanıcılar" özelliğini. TemplateField ait tanımlama `ItemTemplate` veri bağlama ifadesi değerini görüntüler `Container.DataItem` içinde `Text` adlı bir etiket özelliği `UserNameLabel`.

Ekleme ve GridView yapılandırdıktan sonra "rolü tarafından" arabiriminin bildirim temelli biçimlendirme aşağıdakine benzer görünmelidir:

[!code-aspx[Main](assigning-roles-to-users-cs/samples/sample12.aspx)]

Doldurmak ihtiyacımız `RoleList` DropDownList sisteminde roller kümesi ile. Bunu yapmak için güncelleştirme `BindRolesToList` , bağlar, bu nedenle yöntemi tarafından döndürülen dize dizisi `Roles.GetAllRoles` yönteme `RolesList` DropDownList (yanı sıra `UsersRoleList` Repeater).

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample13.cs)]

Son iki satırları `BindRolesToList` yöntemi bağlamak için roller kümesi eklenmiştir `RoleList` DropDownList denetimi. Şekil 5 – sistem rolleri ile doldurulmuş bir açılan liste tarayıcısından görüntülendiğinde nihai sonucu gösterir.


[![Rolleri RoleList DropDownList içinde görüntülenir.](assigning-roles-to-users-cs/_static/image14.png)](assigning-roles-to-users-cs/_static/image13.png)

**Şekil 5**: roller görüntülenir `RoleList` DropDownList ([tam boyutlu görüntüyü görmek için tıklatın](assigning-roles-to-users-cs/_static/image15.png))


### <a name="displaying-the-users-that-belong-to-the-selected-role"></a>Seçilen Role ait olan kullanıcıları görüntüleme

Sayfa ilk yüklenen ne zaman ya da yeni bir rolü zaman seçilir `RoleList` DropDownList, ihtiyacımız GridView bu rolüne ait olan kullanıcıların listesini görüntüleyin. Adlı bir yöntem oluşturma `DisplayUsersBelongingToRole` aşağıdaki kodu kullanarak:

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample14.cs)]

Bu yöntem seçili rolünden alarak başlatır `RoleList` DropDownList. Ardından kullanır [ `Roles.GetUsersInRole(roleName)` yöntemi](https://msdn.microsoft.com/library/system.web.security.roles.getusersinrole.aspx) bu role ait kullanıcılar kullanıcı adlarını dize dizi alınacak. Bu dizi sonra bağlı `RolesUserList` GridView.

Bu yöntem iki durumda çağrılması gerekir: sayfa ilk yüklendiğinde ve ne zaman seçili rolde `RoleList` DropDownList değişiklikler. Bu nedenle, güncelleştirme `Page_Load` olay işleyicisi çağırdıktan sonra bu yöntemin çağrılması `CheckRolesForSelectedUser`. Ardından, bir olay işleyicisi oluşturun `RoleList`'s `SelectedIndexChanged` olay ve buradan çok bu yöntemi çağırın.

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample15.cs)]

Bu kod, yerinde `RolesUserList` GridView, seçili role ait kullanıcılarla görüntülemelidir. Şekil 6 gösterildiği gibi denetçilere rolü iki üyesi oluşur: Bruce ve Tito.


[![Seçilen Role ait kullanıcılarla GridView listeler](assigning-roles-to-users-cs/_static/image17.png)](assigning-roles-to-users-cs/_static/image16.png)

**Şekil 6**: GridView listeler bu kullanıcılar, ait seçili rolü ([tam boyutlu görüntüyü görmek için tıklatın](assigning-roles-to-users-cs/_static/image18.png))


### <a name="removing-users-from-the-selected-role"></a>Kullanıcılar seçili rolden kaldırılıyor

Github'dan genişletmek `RolesUserList` GridView sütunu içeren "Kaldır" düğmeleri. Belirli bir kullanıcı için "Kaldır" düğmesine tıklayarak bu rolden kaldırır.

GridView'a Sil düğmesini alan ekleyerek başlayın. Bu alanı dosyalanmış en soldaki görünür ve değiştirme yapma, `DeleteText` "Sil" (varsayılan) özelliğine "Remove".


[![Ekleme](assigning-roles-to-users-cs/_static/image20.png)](assigning-roles-to-users-cs/_static/image19.png)

**Şekil 7**: GridView için "Kaldır" düğmesi ekleyin ([tam boyutlu görüntüyü görmek için tıklatın](assigning-roles-to-users-cs/_static/image21.png))


Bir geri gönderme "Kaldır" düğmesine tıklandığında ensues ve GridView'ın `RowDeleting` olayı oluşturulur. Bu olay için bir olay işleyicisi oluşturun ve seçili rolünden kullanıcı kaldırır kod yazmak ihtiyacımız var. Olay işleyicisi oluşturun ve ardından aşağıdaki kodu ekleyin:

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample16.cs)]

Kod, seçili rol adı belirleyerek başlatır. Ardından, program aracılığıyla başvuruları `UserNameLabel` , "Kaldır" düğmeye tıkladı kaldırılacak kullanıcının kullanıcı adını belirlemek için satır denetimi. Kullanıcı rolden çağrısıyla kaldırılır `Roles.RemoveUserFromRole` yöntemi. `RolesUserList` Aracılığıyla bir ileti görüntülenir ve GridView ardından yenilendiğini `ActionStatus` etiket denetimi.

> [!NOTE]
> "Kaldır" düğmesi, her türlü kullanıcı rolünden kaldırmadan önce kullanıcıdan onay gerektirmez. Belirli bir düzeyde kullanıcı onayı eklemek için davet ettiğim. Eylemi onaylamak için en kolay yollarından biri bir istemci-tarafı Onayla iletişim kutusudur. Bu yöntem hakkında daha fazla bilgi için bkz. [ekleme istemci tarafı doğrulama zaman silme](https://asp.net/learn/data-access/tutorial-42-cs.aspx).


Şekil 8, denetçilere gruptan kullanıcı Tito kaldırıldıktan sonra sayfada gösterilir.


[![Ne yazık ki Tito artık bir yönetici değil](assigning-roles-to-users-cs/_static/image23.png)](assigning-roles-to-users-cs/_static/image22.png)

**Şekil 8**: ne yazık ki Tito artık bir yönetici değil ([tam boyutlu görüntüyü görmek için tıklatın](assigning-roles-to-users-cs/_static/image24.png))


### <a name="adding-new-users-to-the-selected-role"></a>Yeni kullanıcılar, seçili Role ekleniyor

Kullanıcılar, seçili rolden kaldırılıyor yanı sıra bu sayfaya ziyaretçi Ayrıca seçili rolüne kullanıcı eklemek gerekir. Seçili rol için bir kullanıcı eklemek için en iyi arabirimi kullanıcı hesapları, sahip olmayı beklediğiniz sayısına bağlıdır. Web sitenizi birkaç düzine kullanıcı hesaplarını kümelerinizi barındıracak veya daha küçükse, burada bir DropDownList kullanabilirsiniz. Binlerce kullanıcı hesaplarının olasılığı varsa, ziyaretçi hesabından, belirli bir hesap için arama sayfasında veya başka bir biçimde kullanıcı hesaplarını filtrelemek için izin veren bir kullanıcı arabirimi eklemek istersiniz.

Bu sayfa için kullanıcı hesaplarının sayısı ne olursa olsun sistemde çalışan çok basit bir arabirim kullanalım. Yani, seçili role eklemek istediği kullanıcının kullanıcı adı yazın ziyaretçi isteyen TextBox kullanacağız. Bu ada sahip bir kullanıcı yok veya kullanıcı rolünün bir üyesi ise, bir ileti görüntüleyeceğiz `ActionStatus` etiketi. Ancak kullanıcı varsa ve rolünün bir üyesi değil, bunları bir role eklemek ve kılavuz Yenile.

Bir metin kutusu ve düğme GridView altına ekleyin. Metin kutusunun ayarlamak `ID` için `UserNameToAddToRole` ve düğmenin `ID` ve `Text` özelliklerine `AddUserToRoleButton` ve "için" kullanıcı rolüne ekleme, sırasıyla.

[!code-aspx[Main](assigning-roles-to-users-cs/samples/sample17.aspx)]

Ardından, oluşturun bir `Click` için olay işleyicisi `AddUserToRoleButton` ve aşağıdaki kodu ekleyin:

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample18.cs)]

Kodda çoğunu `Click` olay işleyicisi, çeşitli doğrulama denetimleri gerçekleştirir. Ziyaretçi bir kullanıcı tarafından sağlanan sağlar `UserNameToAddToRole` kullanıcı sistemde var olan ve seçilen rol için zaten ait olmayan metin. Aşağıdakilerden herhangi biri başarısız işaretlerse, uygun bir mesaj görüntülenir `ActionStatus` ve olay işleyicisi çıkıldı. Tüm denetimleri başarılı olursa kullanıcı rolüne eklenir `Roles.AddUserToRole` yöntemi. Metin kutusu ait aşağıdaki `Text` özelliği temizlenir, GridView yenilenir ve `ActionStatus` etiket, belirtilen kullanıcı için seçili rolü başarıyla eklendiğini gösteren bir ileti görüntülenir.

> [!NOTE]
> Belirtilen kullanıcı zaten seçili rolüne ait olmadığını sağlamak için kullanırız [ `Roles.IsUserInRole(userName, roleName)` yöntemi](https://msdn.microsoft.com/library/system.web.security.roles.isuserinrole.aspx), belirten bir Boole değeri döndüren olmadığını *kullanıcı adı* üyesi*roleName*. Bu yöntemde tekrar kullanacağız <a id="_msoanchor_2"> </a> [sonraki öğreticiye](role-based-authorization-cs.md) rol tabanlı yetkilendirme ne zaman hazırız.


Bir tarayıcı aracılığıyla sayfasını ziyaret edin ve denetçiler rolünden seçin `RoleList` DropDownList. Geçersiz kullanıcı adı girmeyi deneyin – kullanıcı sistemde mevcut olmadığını açıklayan bir ileti görmeniz gerekir.


[![Bir Role mevcut olmayan kullanıcı eklenemiyor](assigning-roles-to-users-cs/_static/image26.png)](assigning-roles-to-users-cs/_static/image25.png)

**Şekil 9**: rol varolmayan kullanıcı eklenemiyor ([tam boyutlu görüntüyü görmek için tıklatın](assigning-roles-to-users-cs/_static/image27.png))


Şimdi geçerli bir kullanıcı eklemeyi deneyin. Devam edin ve Tito yeniden denetçiler rolüne ekleyin.


[![Tito yine bir denetleyicidir!](assigning-roles-to-users-cs/_static/image29.png)](assigning-roles-to-users-cs/_static/image28.png)

**Şekil 10**: Tito olduğunu bir kez daha bir yönetici!  ([Tam boyutlu görüntüyü görmek için tıklatın](assigning-roles-to-users-cs/_static/image30.png))


## <a name="step-3-cross-updating-the-by-user-and-by-role-interfaces"></a>3. adım: Arası güncelleştirme "kullanıcı" ve "rol" arabirimleri

`UsersAndRoles.aspx` Sayfası, kullanıcıları ve rolleri yönetmek için iki ayrı arabirim sunar. Tek bir arabirimde yapılan bir değişikliği hemen diğer yansıtılmaz, olası, bu nedenle şu anda, bu iki arabirim devredebileceği işlevi görür. Örneğin, ziyaretçi sayfasına denetçiler rolünden seçer Imagine `RoleList` Bruce ve Tito üyeleri listeleyen DropDownList. Ardından, ziyaretçi Tito öğesinden seçer `UserList` yöneticileri ve denetçiler onay kutularını denetleyen içinde DropDownList `UsersRoleList` yineleyici. Ziyaretçi ardından Repeater yönetici rolünden temizler, Tito denetçiler rolden kaldırıldı, ancak bu değişiklik "rolü tarafından" arabiriminde yansıtılmaz. GridView denetçiler rolünün bir üyesi olacak şekilde Tito yine de gösterilir.

İhtiyacımız rol işaretli veya işaretsiz gelen olduğunda GridView yenilemek için bu sorunu gidermek için `UsersRoleList` yineleyici. Benzer şekilde, size her bir kullanıcı kaldırıldı veya "rolü tarafından" arabiriminden role eklenen Repeater yenilemeniz gerekir.

"Kullanıcı tarafından" arabirimi Yineleyicideki çağırarak yenilenir `CheckRolesForSelectedUser` yöntemi. "Rolü tarafından" arabirimi değiştirilebilir `RolesUserList` GridView'ın `RowDeleting` olay işleyicisi ve `AddUserToRoleButton` düğmenin `Click` olay işleyicisi. Bu nedenle, çağrılacak ihtiyacımız `CheckRolesForSelectedUser` yöntemi bu yöntemlerin her biri.

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample19.cs)]

Benzer şekilde, "rolü tarafından" arabiriminde GridView çağırarak yenilenir `DisplayUsersBelongingToRole` yöntemi ve "kullanıcı tarafından" arabirimi aracılığıyla değiştirildiğinde `RoleCheckBox_CheckChanged` olay işleyicisi. Bu nedenle, çağrılacak ihtiyacımız `DisplayUsersBelongingToRole` bu olay işleyicisi yöntemi.

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample20.cs)]

Bu küçük kod değişiklikleriyle "kullanıcı" ve "rol" arabirimleri artık doğru şekilde çapraz güncelleştirme. Bunu doğrulamak için bir tarayıcı aracılığıyla sayfasını ziyaret edin ve Tito seçip gelen denetçiler `UserList` ve `RoleList` DropDownList, sırasıyla. Denetçiler rol "kullanıcı tarafından" arabirimi Yineleyicideki gelen Tito için işaretini kaldırın gibi Tito otomatik olarak "rolü tarafından" arabiriminde GridView kaldırılır olduğunu unutmayın. Otomatik olarak "rolü tarafından" arabiriminden Tito denetçiler rolüne ekleme "kullanıcı tarafından" arabiriminde denetçiler onay kutusunu yeniden denetler.

## <a name="step-4-customizing-the-createuserwizard-to-include-a-specify-roles-step"></a>4. adım: bir "Rollerini belirtin" adımı eklemek için CreateUserWizard özelleştirme

İçinde <a id="_msoanchor_3"> </a> [ *kullanıcı hesapları oluşturma* ](../membership/creating-user-accounts-cs.md) gördüğümüz yeni bir kullanıcı hesabı oluşturmak için bir arabirim sağlayacağı şekilde CreateUserWizard Web denetimi kullanma Öğreticisi. CreateUserWizard denetimi iki yoldan biriyle kullanılabilir:

- Sitesi, kendi kullanıcı hesabı oluşturmak ziyaretçi için bir yol olarak ve
- Bir araç olarak yeni bir hesap oluşturmak Yöneticiler için

İlk kullanım örneğindeki bir ziyaretçi siteye gelir ve sitede kaydetmek için bilgileri girerek CreateUserWizard kullanıma doldurur. İkinci durumda, yönetici yeni bir hesap için başka bir kişi oluşturur.

Başka bir kişi için bir yönetici hesabınız oluşturulduğunda, yöneticinin yeni kullanıcı hesabına ait hangi rolleri belirtmek izin vermek faydalı olabilir. İçinde <a id="_msoanchor_4"> </a> [ *depolama* *ek kullanıcı bilgileri* ](../membership/storing-additional-user-information-cs.md) ek ekleyerek CreateUserWizard özelleştirme gördüğümüz Öğreticisi `WizardSteps`. Yeni kullanıcı rolleri belirtmek için CreateUserWizard için ek bir adım eklemek nasıl göz atalım.

Açık `CreateUserWizardWithRoles.aspx` adlı CreateUserWizard denetim ekleme ve sayfa `RegisterUserWithRoles`. Denetimin ayarlamak `ContinueDestinationPageUrl` özelliğini "~ / Default.aspx". Buradaki yönetici bu CreateUserWizard denetimi yeni kullanıcı hesapları oluşturmak için kullanacağınız olduğundan, denetimin ayarlamak `LoginCreatedUser` özelliğini False. Bu `LoginCreatedUser` özelliği ziyaretçi otomatik yeni oluşturulan kullanıcı oturum açmadığında ve True olarak varsayılan olup olmadığını belirtir. Yöneticinin yeni bir hesap oluşturduğunda kendisine kendisini imzalanmış tutmak istediğimizden bunu False olarak ayarladık.

Ardından, "Ekle/Kaldır `WizardSteps`..." seçeneğini CreateUserWizard'ın akıllı etiketten ve yeni bir `WizardStep`, ayar, `ID` için `SpecifyRolesStep`. Taşıma `SpecifyRolesStep WizardStep` böylece "Sign Up for yeni hesabınız" adımından sonra ancak "Tamamlandı" adım önce gelir. Ayarlama `WizardStep`'s `Title` özelliğini "Rolleri belirtin", kendi `StepType` özelliğini `Step`ve kendi `AllowReturn` özelliğini False.


[![Ekleme](assigning-roles-to-users-cs/_static/image32.png)](assigning-roles-to-users-cs/_static/image31.png)

**Şekil 11**: "Rolleri belirtin" ekleme `WizardStep` CreateUserWizard için ([tam boyutlu görüntüyü görmek için tıklatın](assigning-roles-to-users-cs/_static/image33.png))


Bu değişiklikten sonra CreateUserWizard'ın bildirim temelli biçimlendirme, aşağıdaki gibi görünmelidir:

[!code-aspx[Main](assigning-roles-to-users-cs/samples/sample21.aspx)]

"Rollerinde belirtin" `WizardStep`, adlandırılmış bir CheckBoxList ekleme `RoleList`. Bu CheckBoxList kişi sayfasını ziyaret ederek yeni oluşturulan kullanıcının ait olduğu hangi rolleri işaretleyin etkinleştirme, kullanılabilir roller listelenir.

Şu iki kodlama görevleri kaldı: ilk biz doldurmanız gerekir `RoleList` CheckBoxList; sistem rolleriyle ikinci kullanıcı "Tam" adıma "Rolleri belirtin" adımından hareket ettirdiğinde seçili roller için oluşturulan kullanıcı eklemek ihtiyacımız. Biz ilk görevi gerçekleştirebilirsiniz `Page_Load` olay işleyicisi. Aşağıdaki program aracılığıyla başvuruları kod `RoleList` ilk onay için sayfasını ziyaret edin ve rolleri sistemde bağlar.

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample22.cs)]

Yukarıdaki kod, tanıdık gelecektir. İçinde <a id="_msoanchor_5"> </a> [ *depolama* *ek kullanıcı bilgileri* ](../membership/storing-additional-user-information-cs.md) iki kullandık öğretici `FindControl` Web denetimi başvurmak ifadeleri gelen bir özel içinde `WizardStep`. Ve roller için CheckBoxList bağlayan kodu Bu öğreticide daha önce yapılmadı.

İkinci bir programlama görevi gerçekleştirmek için biz ne zaman "Rolleri belirtin" adım tamamlandı bilmeniz gerekir. CreateUserWizard olan geri çağırma bir `ActiveStepChanged` olayı ziyaretçi başka bir adımda gittiğinde her değişiminde tetikler. Kullanıcı bir "Tam" adım ulaştı burada belirleyebiliriz; Bu durumda, biz seçili rollere kullanıcı eklemeniz gerekir.

İçin bir olay işleyicisi oluşturun `ActiveStepChanged` olay ve aşağıdaki kodu ekleyin:

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample23.cs)]

Kullanıcı yalnızca "Completed" adım ulaştıysa, olay işleyicisi öğelerini numaralandırır `RoleList` CheckBoxList ve yeni oluşturulan kullanıcı seçili rollere atanır.

Bir tarayıcı aracılığıyla bu sayfasını ziyaret edin. İlk CreateUserWizard yeni kullanıcının kullanıcı adı, parola, e-posta ve diğer önemli bilgiler için ister standart "Sign Up for yeni hesabınız" adım adımdır. Wanda adlı yeni bir kullanıcı oluşturmak için bilgileri girin.


[![Wanda adlı yeni bir kullanıcı oluşturma](assigning-roles-to-users-cs/_static/image35.png)](assigning-roles-to-users-cs/_static/image34.png)

**Şekil 12**: adlı yeni bir kullanıcı Wanda oluşturun ([tam boyutlu görüntüyü görmek için tıklatın](assigning-roles-to-users-cs/_static/image36.png))


"Kullanıcı oluştur" düğmesine tıklayın. CreateUserWizard dahili olarak çağırır `Membership.CreateUser` yeni kullanıcı hesabına ve ardından sonraki adıma devam oluşturma yöntemi, "belirtin rolleri." Sistem rolleri burada listelenir. Denetçiler onay kutusunu işaretleyin ve İleri'ye tıklayın.


[![Wanda denetçiler rolünün bir üyesi olun](assigning-roles-to-users-cs/_static/image38.png)](assigning-roles-to-users-cs/_static/image37.png)

**Şekil 13**: Wanda denetçiler rolünün bir üyesi olun ([tam boyutlu görüntüyü görmek için tıklatın](assigning-roles-to-users-cs/_static/image39.png))


İleri'ye tıklama neden bir geri gönderme ve güncelleştirmeleri `ActiveStep` "Tamamlandı" adıma. İçinde `ActiveStepChanged` olay işleyicisi, son oluşturulan kullanıcı hesabını denetçiler rolüne atanır. Bunu doğrulamak için iade `UsersAndRoles.aspx` sayfasında ve gelen denetçiler seçin `RoleList` DropDownList. Şekil 14 gösterildiği gibi denetçilere artık üç kullanıcıları oluşur: Bruce Tito ve Wanda.


[![Bruce Tito ve Wanda tüm denetçiler olan](assigning-roles-to-users-cs/_static/image41.png)](assigning-roles-to-users-cs/_static/image40.png)

**Şekil 14**: Bruce Tito ve Wanda olan tüm denetçiler ([tam boyutlu görüntüyü görmek için tıklatın](assigning-roles-to-users-cs/_static/image42.png))


## <a name="summary"></a>Özet

Rolleri framework belirli bir kullanıcının rollerini ve hangi kullanıcıların belirli bir role ait belirlemek için yöntemleri hakkında bilgi almak için yöntemler sağlar. Ayrıca, bir dizi bir veya daha fazla rol için bir veya daha fazla kullanıcı eklemek ve kaldırmak için yöntem vardır. Bu öğreticide biz yalnızca bu iki yöntem üzerinde odaklanır: `AddUserToRole` ve `RemoveUserFromRole`. Bir role birden çok kullanıcı ekleme ve tek bir kullanıcıya birden çok rol atamak için tasarlanan ek çeşitleri vardır.

Bu öğreticide içerecek şekilde CreateUserWizard denetimini genişletme göz de dahil bir `WizardStep` yeni oluşturulan kullanıcı rolleri belirtmek için. Böyle bir adım, yönetici yeni kullanıcılar için kullanıcı hesapları oluşturma işlemini kolaylaştırmaya yardımcı olabilir.

Bu noktada oluşturma ve rollerini silme ve ekleme ve kullanıcı rollerini kaldırın gördük. Ancak biz henüz rol tabanlı yetkilendirme uygulamayı bakmamız gerekiyor. İçinde <a id="_msoanchor_6"> </a> [öğretici aşağıdaki](role-based-authorization-cs.md) sayfa düzeyi işlevselliği sınırlamak için şu anda oturum açmış kullanıcı rollerine göre nasıl biz bir rol rollü temelinde URL yetkilendirme kurallarını tanımlama sırasında de görünür.

Mutlu programlama!

### <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide ele alınan konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [ASP.NET Web sitesi yönetim aracına genel bakış](https://msdn.microsoft.com/library/ms228053.aspx)
- [ASP inceleniyor. NET üyelik, roller ve profil](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Kendi Web sitesi yönetim aracı alınıyor](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)

### <a name="about-the-author"></a>Yazar hakkında

Scott Mitchell, birden çok ASP/ASP.NET Books yazar ve poshbeauty.com sitesinin 4GuysFromRolla.com, Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan  *[Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott, konumunda ulaşılabilir [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) veya kendi blog'da aracılığıyla [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Özel performanstan...

Bu öğretici serisinde, birçok yararlı Gözden Geçiren tarafından gözden geçirildi. Bu öğretici için müşteri adayı İnceleme Teresa Murphy oluştu. Yaklaşan My MSDN makaleleri gözden geçirme ilgileniyor musunuz? Bu durumda, bir satır bana bırak [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](creating-and-managing-roles-cs.md)
> [İleri](role-based-authorization-cs.md)
