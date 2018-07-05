---
uid: web-forms/overview/older-versions-security/admin/unlocking-and-approving-user-accounts-cs
title: Kilit açma ve kullanıcı hesapları (C#) onaylama | Microsoft Docs
author: rick-anderson
description: Bu öğreticide, yöneticilerin yönetmek için bir web sayfası oluşturmak gösterilir kullanıcıların kilitli ve durumları onaylandı. Nasıl yeni kullanıcılar o onaylamak de göreceğiz...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2008
ms.topic: article
ms.assetid: 5346aab1-9974-489f-a065-ae3883b8a350
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-security/admin/unlocking-and-approving-user-accounts-cs
msc.type: authoredcontent
ms.openlocfilehash: f6bf76579898a8ff36b18380100ce4ab2e74fc8d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37392722"
---
<a name="unlocking-and-approving-user-accounts-c"></a>Kullanıcı hesaplarının kilidini açma ve onaylama (C#)
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indir](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/CS.14.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/aspnet_tutorial14_UnlockAndApprove_cs.pdf)

> Bu öğreticide, yöneticilerin yönetmek için bir web sayfası oluşturmak gösterilir kullanıcıların kilitli ve durumları onaylandı. Nasıl yeni kullanıcılar yalnızca e-posta adresi doğruladıktan sonra onaylamak de göreceğiz.


## <a name="introduction"></a>Giriş

Bir kullanıcı adı, parola ve e-posta ile birlikte her kullanıcı hesabının kullanıcı sitesine oturum açabilir dikte iki durum alanlarına sahip: kilitli ve onaylandı. Geçersiz kimlik bilgileri kez belirtilen sayıda dakika (varsayılan ayarları bir kullanıcı 10 dakika içinde 5 geçersiz oturum açma denemesinden sonra kilitlemesine) içinde belirtilen sayıda sağlarsanız, bir kullanıcı otomatik olarak kilitlidir. Onaylanan durumu, yeni bir kullanıcı sitesinde oturum açmadan önce bazı eylemler burada niteler gereken senaryolarda yararlıdır. Örneğin, bir kullanıcı ilk kez e-posta adresi doğrulama veya oturum açabilmeniz edilmeden önce bir yönetici tarafından onaylanması gerekebilir.

Çünkü kilitli bir out veya onaylanmamış olamaz, kullanıcı oturum açma, bu durumlar sıfırlama merak ediyor için yalnızca doğal anahtardır. ASP.NET herhangi bir yerleşik işlevsellik içermiyor veya kullanıcıların yönetmek için Web denetimleri kilitlendi ve bu kararlar siteden siteye olarak ele alınması gerektiğinden durumları, kısmen onaylanmış. Bazı siteler, tüm yeni kullanıcı hesapları (varsayılan davranış) otomatik olarak onayla. Diğer yönetici sahip yeni hesapları onaylayın veya RMS'ye kaydolurken sağlanan e-posta adresine gönderilen bir bağlantıyı ziyaret ettikleri kadar kullanıcılar onaylıyor musunuz. Benzer şekilde, durumlarını yönetici sıfırlar kadar bazı siteler kullanıcıların kilitleme, diğer sitelere bir URL ile kullanıma kilitli kullanıcıya bir e-posta gönderirken, kendi hesabının kilidini açmak için ziyaret edebilirsiniz.

Bu öğreticide, yöneticilerin yönetmek için bir web sayfası oluşturmak gösterilir kullanıcıların kilitli ve durumları onaylandı. Nasıl yeni kullanıcılar yalnızca e-posta adresi doğruladıktan sonra onaylamak de göreceğiz.

## <a name="step-1-managing-users-locked-out-and-approved-statuses"></a>1. adım: Kullanıcıların yönetme ve kilitli durumları Onaylandı

İçinde <a id="Tutorial12"> </a> [ *birçoğundan seçin bir kullanıcı hesabı için bir arabirim oluşturma* ](building-an-interface-to-select-one-user-account-from-many-cs.md) biz oluşturulan her kullanıcı hesabında bir disk belleği, listelenen bir sayfa öğretici filtrelenmiş GridView. Kılavuz, her bir kullanıcı adı ve e-posta, bunların onaylanmış ve çıkış kilitli durumlarını, olup şu anda çevrimiçi ve kullanıcı ile ilgili açıklamalar listelenmektedir. Kullanıcılarınızın yönetmek için onaylanmış ve durumları kilitli, biz bu kılavuz düzenlenebilir hale getirebilecek. Bir kullanıcının onaylı durumunu değiştirmek için yönetici ilk kullanıcı hesabını bulun ve ardından karşılık gelen GridView satır denetimi veya onaylanan onay kutusunun seçilirliği kaldırıldığında düzenleyin. Alternatif olarak, ayrı bir ASP.NET sayfası aracılığıyla onaylanmış ve çıkış kilitli durumları yönetiyoruz.

Bu öğretici için iki ASP.NET sayfaları kullanalım: `ManageUsers.aspx` ve `UserInformation.aspx`. Buradaki olan `ManageUsers.aspx` sistemde kullanıcı hesaplarını listeler sırada `UserInformation.aspx` yöneticinin belirli bir kullanıcı için onaylanmış ve çıkış kilitli durumları yönetmenizi sağlar. Bizim ilk iş sırası içinde GridView genişletmek için olan `ManageUsers.aspx` bağlantılar içeren bir sütun işleyen bir HyperLinkField eklenecek. Her bağlantı işaret edecek şekilde istiyoruz `UserInformation.aspx?user=UserName`burada *kullanıcıadı* düzenlemek için kullanıcı adıdır.

> [!NOTE]
> Kodu indirdiyseniz <a id="Tutorial13"> </a> [ *kurtarma ve parolaları değiştirme* ](recovering-and-changing-passwords-cs.md) fark etmiş, öğretici `ManageUsers.aspx` sayfası zaten bir dizi içeriyor " Bağlantıları yönetme"ve `UserInformation.aspx` sayfası, seçilen kullanıcının parolasını değiştirmek için bir arabirim sağlar. Üyelik API'si atlanarak ve bir kullanıcının parolasını değiştirmek için SQL Server veritabanıyla doğrudan işletim çalıştığı için Bu öğretici ile ilişkili kod işlevselliği çoğaltmayacak verdi. Bu öğretici ile sıfırdan başlar `UserInformation.aspx` sayfası.


### <a name="adding-manage-links-to-theuseraccountsgridview"></a>Ekleme "Yönet" bağlantıları`UserAccounts`GridView

Açık `ManageUsers.aspx` sayfa ve ekleme için bir HyperLinkField `UserAccounts` GridView. HyperLinkField'ın ayarlamak `Text` "Manage" özelliğini ve kendi `DataNavigateUrlFields` ve `DataNavigateUrlFormatString` özelliklerine `UserName` ve "UserInformation.aspx?user={0}", sırasıyla. Bu ayarlar tüm köprülerin "Manage" metin görüntüler, ancak her bağlantı uygun başarılı şekilde HyperLinkField yapılandırma *kullanıcı adı* sorgu dizesi değeri.

GridView'a HyperLinkField ekledikten sonra görüntülemek için bir dakikanızı ayırarak `ManageUsers.aspx` tarayıcısından sayfası. Şekil 1 gösterildiği gibi her GridView satır artık "Yönet" bağlantısını içerir. Bruce "Manage" bağlantısına işaret `UserInformation.aspx?user=Bruce`Dave "Manage" bağlantısına işaret bilgileriyse `UserInformation.aspx?user=Dave`.


[![HyperLinkField ekler bir](unlocking-and-approving-user-accounts-cs/_static/image2.png)](unlocking-and-approving-user-accounts-cs/_static/image1.png)

**Şekil 1**: HyperLinkField "Yönet" bağlantısını için her bir kullanıcı hesabı ekler ([tam boyutlu görüntüyü görmek için tıklatın](unlocking-and-approving-user-accounts-cs/_static/image3.png))


Kullanıcı arabirimi oluşturur ve için kod `UserInformation.aspx` bir dakika, ancak ilk Şimdi Sohbet sayfasında hakkında programlı bir kullanıcı olarak nasıl değiştirildiğini kilitli ve durumları onaylandı. [ `MembershipUser` Sınıfı](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx) sahip [ `IsLockedOut` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.islockedout.aspx) ve [ `IsApproved` özellikleri](https://msdn.microsoft.com/library/system.web.security.membershipuser.isapproved.aspx). `IsLockedOut` Özelliği salt okunur. Program aracılığıyla bir kullanıcının oturumunu kilitlemek için bir mekanizma yoktur; bir kullanıcının kilidini açmak için kullanması `MembershipUser` sınıfın [ `UnlockUser` yöntemi](https://msdn.microsoft.com/library/system.web.security.membershipuser.unlockuser.aspx). `IsApproved` Özelliği okunabilir ve yazılabilir. Bu özellik değişiklikleri kaydetmek için çağrılacak ihtiyacımız `Membership` sınıfın [ `UpdateUser` yöntemi](https://msdn.microsoft.com/library/system.web.security.membership.updateuser.aspx), değiştirilmiş içinde geçen `MembershipUser` nesne.

Çünkü `IsApproved` özelliği okunabilir ve yazılabilir, bir onay kutusu denetimi muhtemelen bu özellik yapılandırmak için en iyi kullanıcı arabirimi öğesidir. Ancak, bir onay kutusu için işe yaramaz `IsLockedOut` özelliğini bir yönetici bir kullanıcının oturumunu kilitlenemiyor çünkü kendisi yalnızca kullanıcının kilidini açmak. Uygun kullanıcı arabirimi için `IsLockedOut` özelliği olan bir düğme, tıklandığında, kullanıcı hesabının kilidini açar. Bu düğme, yalnızca kullanıcıya kilitlenmişse etkinleştirilmelidir.

### <a name="creating-theuserinformationaspxpage"></a>Oluşturma`UserInformation.aspx`sayfası

Artık kullanıcı arabiriminde uygulamak hazırız `UserInformation.aspx`. Bu sayfayı açın ve aşağıdaki Web denetimleri ekleyin:

- Köprü denetim, tıklandığında yöneticinin döndürür `ManageUsers.aspx` sayfası.
- Seçilen kullanıcının adını görüntülemek için bir etiket Web denetimi. Bu etiketin `ID` için `UserNameLabel` ve temizleyin, `Text` özelliği.
- Adlı bir onay kutusu denetimi `IsApproved`. Ayarlama, `AutoPostBack` özelliğini `true`.
- Kullanıcı görüntülemek için bir etiket denetimi son tarihini kilitli. Bu etiket adı `LastLockedOutDateLabel` ve temizleyin, `Text` özelliği.
- Kullanıcının kilidi kaldırma için bir düğme. Bu düğmeyi adlandırın `UnlockUserButton` ve kendi `Text` "Kullanıcı kilidini aç" özelliği.
- Durum iletilerini görüntülemek için "kullanıcı onaylı durumunu güncelleştirildi,."gibi bir etiket denetimi Bu denetim adı `StatusMessage`kullanıma temizleyin, `Text` özelliği ve kümesi kendi `CssClass` özelliğini `Important`. ( `Important` CSS sınıfı içinde tanımlanan `Styles.css` stil sayfası dosyası; büyük, kırmızı bir yazı tipinde karşılık gelen metin görüntüler.)

Bu denetimler ekledikten sonra Visual Studio Tasarım görünümünde Şekil 2'de ekran şuna benzemelidir.


[![UserInformation.aspx için kullanıcı arabirimi oluşturma](unlocking-and-approving-user-accounts-cs/_static/image5.png)](unlocking-and-approving-user-accounts-cs/_static/image4.png)

**Şekil 2**: kullanıcı arabirimi oluşturma `UserInformation.aspx` ([tam boyutlu görüntüyü görmek için tıklatın](unlocking-and-approving-user-accounts-cs/_static/image6.png))


Kullanıcı arabirimi tam olarak, bizim sonraki görev ayarlamaktır `IsApproved` onay kutusunu ve diğer denetimleri göre seçilen kullanıcının bilgi. Sayfa için bir olay işleyicisi oluşturun `Load` olay ve aşağıdaki kodu ekleyin:

[!code-csharp[Main](unlocking-and-approving-user-accounts-cs/samples/sample1.cs)]

Yukarıdaki kod, sayfa ve değil sonraki geri gönderme ilk kez ziyaret budur sağlayarak başlatır. Ardından geçirilen kullanıcı adı okur `user` querystring alan ve bu kullanıcı hesabıyla ilgili bilgileri alır `Membership.GetUser(username)` yöntemi. Sorgu dizesi kullanıcı adı sağlandı veya belirtilen kullanıcı bulunamadı, yönetici geri gönderilir `ManageUsers.aspx` sayfası.

`MembershipUser` Nesnenin `UserName` değerinin görüntülendiği ardından `UserNameLabel` ve `IsApproved` onay kutusu işaretli göre `IsApproved` özellik değeri.

`MembershipUser` Nesnenin [ `LastLockoutDate` özelliği](https://msdn.microsoft.com/library/system.web.security.membershipuser.lastlockoutdate.aspx) döndürür bir `DateTime` kullanıcının en son ne zaman gösteren değer kilitlendi. Döndürülen değer, kullanıcı asla kilitlenmişse, üyelik sağlayıcısına bağlıdır. Yeni bir hesap oluşturulduğunda `SqlMembershipProvider` ayarlar `aspnet_Membership` tablonun `LastLockoutDate` alanı `1754-01-01 12:00:00 AM`. Yukarıdaki kodu boş bir dize olarak görüntüler `LastLockoutDateLabel` varsa `LastLockoutDate` özelliği yıl önce ortaya 2000; Aksi takdirde tarih bölümü `LastLockoutDate` özelliği etikette görüntülenir. `UnlockUserButton'` s `Enabled` özelliği, kullanıcının kullanıcıya kilitlenmişse bu düğme yalnızca etkin olacağını anlamı durumu, kilitli ayarlanır.

Test etmek için birkaç dakikanızı `UserInformation.aspx` tarayıcısından sayfası. Elbette, başlangıç yapmanız gerekir `ManageUsers.aspx` ve yönetmek için bir kullanıcı hesabını seçin. Gelen bağlı `UserInformation.aspx`, dikkat `IsApproved` onay kutusu yalnızca kullanıcı onaylanırsa işaretli. Kullanıcı hiç olmadığı kadar kilitlenmişse, kendi son tarihini kilitli görüntülenir. Yalnızca kullanıcı şu anda kilitli kullanıcı Kilidini Aç düğmesi etkinleştirilir. Denetleme veya işaretini `IsApproved` onay kutusunun veya kilidini kullanıcı düğmeye tıklandığında geri göndermeye neden olur, ancak henüz için yaptığımız olduğundan herhangi bir değişiklik kullanıcı hesabına yapılan bu olayları için olay işleyicileri oluşturun.

Visual Studio'ya geri dönün ve olay işleyicileri `IsApproved` CheckBox'ın `CheckedChanged` olay ve `UnlockUser` düğmenin `Click` olay. İçinde `CheckedChanged` olay işleyicisi, kullanıcının ayarlamak `IsApproved` özelliğini `Checked` onay kutusunu ve ardından bir çağrı aracılığıyla değişiklikleri kaydetme özelliği `Membership.UpdateUser`. İçinde `Click` olay işleyicisi, yalnızca çağrı `MembershipUser` nesnenin `UnlockUser` yöntemi. Her iki olay işleyicileri, uygun bir ileti görüntüler `StatusMessage` etiketi.

[!code-csharp[Main](unlocking-and-approving-user-accounts-cs/samples/sample2.cs)]

### <a name="testing-theuserinformationaspxpage"></a>Test`UserInformation.aspx`sayfası

Bu olay işleyicileri ile yerinde sayfayı yeniden ziyaret hem de onaylanmayan bir kullanıcı. Şekil 3'te gösterildiği gibi kısa bir kullanıcının belirten sayfada iletisini görmeniz gerekir `IsApproved` özelliği başarıyla değiştirildi.


[![Chris Onaylanmadı kaldırıldı](unlocking-and-approving-user-accounts-cs/_static/image8.png)](unlocking-and-approving-user-accounts-cs/_static/image7.png)

**Şekil 3**: Onaylanmadı Chris'in rolünüzün ([tam boyutlu görüntüyü görmek için tıklatın](unlocking-and-approving-user-accounts-cs/_static/image9.png))


Ardından, oturum kapatma ve hesabı kullanıcısı olarak oturum açmayı deneyin yalnızca onaylanmadı. Kullanıcı onaylı değil çünkü oturum açamıyor. Kullanıcı oturum açamıyorum nedeni ne olursa olsun, varsayılan olarak, oturum açma denetimi aynı iletiyi görüntüler. Ancak <a id="Tutorial6"> </a> [ *doğrulanırken kullanıcı kimlik bilgilerine karşı üyelik kullanıcı Store* ](../membership/validating-user-credentials-against-the-membership-user-store-cs.md) öğretici daha uygun bir ileti görüntülemek için oturum açma denetimi geliştirme sırasında incelemiştik. Şekil 4'te gösterildiği gibi Chris, hesabı henüz onaylanmadığı için yaptığı oturum açamıyorum olduğunu açıklayan bir ileti gösterilir.


[![Chris olamaz oturum açma çünkü HIS Onaylanmadı hesabıdır](unlocking-and-approving-user-accounts-cs/_static/image11.png)](unlocking-and-approving-user-accounts-cs/_static/image10.png)

**Şekil 4**: Chris olamaz oturum açma çünkü HIS hesabıdır Onaylanmadı ([tam boyutlu görüntüyü görmek için tıklatın](unlocking-and-approving-user-accounts-cs/_static/image12.png))


Kilitli çıkış işlevselliğini test etmek için onaylı bir kullanıcı olarak oturum açma, ancak yanlış bir parola girişimi. Bu işlemi gerekli sayıda kullanıcının hesap kilitlendi kadar tekrarlayın. Oturum açma denetimi özel göstermek için aynı zamanda güncelleştirildiği kullanıma kilitli bir hesaptan oturum açmaya çalışırken, ileti. Oturum açma sayfasında şu iletiyi görüyor başlattıktan sonra hesabınız kilitlendi olduğunu bildiğiniz: "hesabınızı çok fazla geçersiz oturum açma denemesi nedeniyle kilitlendi. Hesabınızın kilidi için yöneticisine başvurun."

Geri dönüp `ManageUsers.aspx` sayfasında ve çıkış kilitli kullanıcı Yönet bağlantısına tıklayın. Şekil 5 gösterildiği gibi bir değer görürsünüz `LastLockedOutDateLabel` kilidini kullanıcı düğmenin etkinleştirilmesi gerekir. Kullanıcı hesabının kilidini açmak için kullanıcının kilidini aç düğmesine tıklayın. Kullanıcı kilidini açtınız sonra tekrar oturum açabilmeniz olacaktır.


[![Dave dışında sistem kilitlendi](unlocking-and-approving-user-accounts-cs/_static/image14.png)](unlocking-and-approving-user-accounts-cs/_static/image13.png)

**Şekil 5**: Dave sahip olan kilitli çıkış sisteminin ([tam boyutlu görüntüyü görmek için tıklatın](unlocking-and-approving-user-accounts-cs/_static/image15.png))


## <a name="step-2-specifying-new-users-approved-status"></a>2. adım: Yeni kullanıcıların belirtme durumunu Onaylandı

Onaylanan durumu, yeni bir kullanıcı oturum açma ve sitenin kullanıcıya özgü özelliklere erişim açmadan önce gerçekleştirilmesi gereken bazı eylemleri istediğiniz senaryolarda yararlıdır. Örneğin, oturum açma ve kaydolma sayfaları hariç tüm sayfaları yalnızca kimliği doğrulanmış kullanıcılar için erişilebilir olduğu özel bir Web sitesi çalışıyor olabilir. Ancak bir yabancı Web siteniz ulaşırsa ne kaydolma sayfasında bulur ve bir hesap oluşturur? Bunun gerçekleşmesini önlemek için kayıt sayfasına taşıyabilirsiniz bir `Administration` klasör ve bir yönetici el ile her hesap oluşturmanızı gerektirir. Alternatif olarak, herkesin kaydolma sağlayan ancak bir yönetici kullanıcı hesabı onaylayana kadar site erişimi engelle.

Varsayılan olarak, yeni hesaplar CreateUserWizard denetim onaylar. Denetimin kullanarak bu davranışı yapılandırabileceğiniz [ `DisableCreatedUser` özelliği](https://msdn.microsoft.com/en-gb/library/system.web.ui.webcontrols.createuserwizard.disablecreateduser.aspx). Bu özellik kümesine `true` yeni kullanıcı hesaplarını onaylamak değil.

> [!NOTE]
> Varsayılan olarak CreateUserWizard denetimi yeni kullanıcı hesabı otomatik olarak günlüğe kaydeder. Bu davranış, denetim tarafından dikte [ `LoginCreatedUser` özelliği](https://msdn.microsoft.com/en-gb/library/system.web.ui.webcontrols.createuserwizard.logincreateduser.aspx). Onaylanmamış kullanıcıların siteye oturum açamıyorum çünkü zaman `DisableCreatedUser` olduğu `true` yeni kullanıcı hesabının siteye değerinden bağımsız olarak günlüğe kaydedilmez `LoginCreatedUser` özelliği.


Yeni kullanıcı hesapları ile program aracılığıyla oluşturuyorsanız `Membership.CreateUser` yöntemi onaylanmamış kullanıcı hesabı oluşturmak için kullanın, yeni kullanıcının kabul eden aşırı `IsApproved` giriş parametresi olarak özellik değeri.

## <a name="step-3-approving-users-by-verifying-their-email-address"></a>3. adım: Kullanıcıları e-posta adresi doğrulayarak onaylama

Bunlar kaydı sırasında sağlanan e-posta adresi doğrulayana kadar kullanıcı hesaplarını destekleyen birçok Web sitesi yeni kullanıcılar onaylıyor musunuz. Bu doğrulama işlemi, bir benzersiz, onaylanmış e-posta adresi gerektirir ve kayıt işleminde fazladan bir adım ekler gibi diğer ne'er-do-wells robotlar ve istenmeyen posta gönderenlere ihlalini savuşturmanın yaygın olarak kullanılır. Bu modelde, yeni kullanıcı kaydolduğunda doğrulama sayfasına bir bağlantı içeren bir e-posta iletisi gönderilir. Bağlantıyı ziyaret ederek kullanıcı e-postayı aldığı ve bu nedenle, e-posta sağlanan adres geçerli olduğu kanıtlanmıştır. Doğrulama sayfası, kullanıcının onaylamak için sorumludur. Bu otomatik olarak başvurulabilir, böylece bu sayfaya ulaşana herhangi bir kullanıcı onaylama ortaya çıkabilir veya yalnızca kullanıcı gibi bazı ek bilgiler sağladıktan sonra bir [CAPTCHA](http://en.wikipedia.org/wiki/Captcha).

Bu iş akışı uyum sağlamak için önce yeni kullanıcıların onaylanmamış hesap oluşturma sayfasına güncelleştirmek ihtiyacımız var. Açık `EnhancedCreateUserWizard.aspx` sayfasını `Membership` klasörü ve CreateUserWizard denetim kümenin `DisableCreatedUser` özelliğini `true`.

Ardından, yeni kullanıcının hesabını onaylamak yönergelerini içeren bir e-posta göndermek için CreateUserWizard denetim gerekir. Özellikle, biz bağlantısını e-posta dahil edilir `Verification.aspx` sayfa (hangi henüz için yaptığımız oluşturun), yeni kullanıcının içinde geçen `UserId` sorgu dizesi aracılığıyla. `Verification.aspx` Sayfasında belirtilen kullanıcı için arama ve bunları onaylanan işaretleyin.

### <a name="sending-a-verification-email-to-new-users"></a>Yeni kullanıcılar için bir doğrulama e-posta gönderme

CreateUserWizard denetiminden bir e-posta göndermek için yapılandırma, `MailDefinition` özelliği uygun şekilde. Bölümünde açıklandığı gibi <a id="Tutorial13"> </a> [önceki öğreticide](recovering-and-changing-passwords-cs.md), ChangePassword vePasswordRecovery denetimleri içeren bir [ `MailDefinition` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.maildefinition.aspx) aynı şekilde çalışır CreateUserWizard denetim.

> [!NOTE]
> Kullanılacak `MailDefinition` posta teslim belirtmeniz gerekir özellik seçenekleri `Web.config`. Daha fazla bilgi için [ASP.NET e-posta gönderme](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx).


Adlı yeni bir e-posta şablonu oluşturarak başlayın `CreateUserWizard.txt` içinde `EmailTemplates` klasör. Aşağıdaki metni için şablonu kullanın:

[!code-aspx[Main](unlocking-and-approving-user-accounts-cs/samples/sample3.aspx)]

Ayarlama `MailDefinition'` s `BodyFileName` özelliğini "~ / EmailTemplates/CreateUserWizard.txt" ve kendi `Subject` özelliği "Benim bir Web sitesine Hoş Geldiniz! Lütfen hesabınızı etkinleştirin."

Unutmayın `CreateUserWizard.txt` e-posta şablonu içeren bir `<%VerificationUrl%>` yer tutucu. Burada URL'sini `Verification.aspx` sayfa yerleştirilir. CreateUserWizard otomatik olarak değiştirir `<%UserName%>` ve `<%Password%>` yer tutucuları yeni hesabın kullanıcı adı ve parola ile yerleşik yoktur ancak `<%VerificationUrl%>` yer tutucu. Uygun doğrulama URL'si ile el ile değiştirmek ihtiyacımız var.

Bunu gerçekleştirmek için bir olay işleyicisi oluşturun CreateUserWizard için 's [ `SendingMail` olay](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.sendingmail.aspx) ve aşağıdaki kodu ekleyin:

[!code-csharp[Main](unlocking-and-approving-user-accounts-cs/samples/sample4.cs)]

`SendingMail` Olayı tetikler sonra `CreatedUser` olay, zaman yukarıdaki olay işleyicisi yürütülür yeni kullanıcı hesabı zaten oluşturulduğundan anlamına gelir. Yeni kullanıcının erişip `UserId` çağırarak değeri `Membership.GetUser` tümleştirilmesidir yöntemi `UserName` CreateUserWizard denetimine girilen. Ardından, doğrulama URL'si de oluşturulur. Deyim `Request.Url.GetLeftPart(UriPartial.Authority)` döndürür `http://yourserver.com` URL; bölümü `Request.ApplicationPath` uygulama kökü burada döndürür yolu. URL olarak tanımlanan sonra doğrulama `Verification.aspx?ID=userId`. Bu iki dizeyi daha sonra tam URL'yi oluşturmak için bitiştirilir. Son olarak, e-posta ileti gövdesi (`e.Message.Body`) sahip tüm oluşumlarını `<%VerificationUrl%>` içeren tam URL'nin değiştirildi.

Siteye oturum açamaz, yani yeni kullanıcıların onaylanmamış, net etkisidir. Ayrıca, otomatik olarak bir bağlantı içeren bir e-posta doğrulama URL'si gönderilmeden (bkz. Şekil 6).


[![Yeni kullanıcı doğrulama URL'si bağlantısını içeren bir e-posta alır.](unlocking-and-approving-user-accounts-cs/_static/image17.png)](unlocking-and-approving-user-accounts-cs/_static/image16.png)

**Şekil 6**: yeni kullanıcı doğrulama URL'si bağlantısını içeren bir e-posta alır ([tam boyutlu görüntüyü görmek için tıklatın](unlocking-and-approving-user-accounts-cs/_static/image18.png))


> [!NOTE]
> CreateUserWizard denetimin varsayılan CreateUserWizard adım kullanıcı hesaplarına oluşturuldu ve bir devam düğmesi görüntüler bildiren bir ileti görüntüler. Bu seçeneğe tıkladığınızda alan kullanıcı denetiminin tarafından belirtilen URL'ye `ContinueDestinationPageUrl` özelliği. İçinde CreateUserWizard `EnhancedCreateUserWizard.aspx` yeni kullanıcılara göndermek için yapılandırıldığı `~/Membership/AdditionalUserInfo.aspx`, kullanıcı kendi memleketinin bulunduğu, giriş sayfası URL'si ve imza için ister. Bu bilgiler yalnızca tarafından oturum açmış kullanıcılar eklenebilir olduğundan, kullanıcılar sitenin giriş sayfasına geri göndermek için bu özelliği güncelleştirmek için mantıklıdır (`~/Default.aspx`). Ayrıca, `EnhancedCreateUserWizard.aspx` sayfası ya da CreateUserWizard adım genişletilebilir bir doğrulama e-postası gönderildi ve kadar bu e-posta içindeki yönergeleri izleyin, hesap etkin olmayacaktır kullanıcıyı bilgilendirmek üzere. Bu değişiklikler için okuyucu bir alıştırma olarak bırakın.


### <a name="creating-the-verification-page"></a>Doğrulama sayfası oluşturma

Bizim son görev oluşturmaktır `Verification.aspx` sayfası. Bu sayfayı ile ilişkilendirme kök klasöre Ekle `Site.master` ana sayfa. Siteye eklenen önceki içerik sayfaların çoğu ile yaptığımız gibi başvuran içerik denetimi kaldırın `LoginContent` ContentPlaceHolder içerik sayfası ana sayfanın kullanmayacağından varsayılan içerik.

Bir etiket Web denetimine ekleme `Verification.aspx` sayfasında kendi `ID` için `StatusMessage` ve metin özelliğini temizleyin. Ardından, oluşturma `Page_Load` olay işleyicisi ve aşağıdaki kodu ekleyin:

[!code-csharp[Main](unlocking-and-approving-user-accounts-cs/samples/sample5.cs)]

Yukarıdaki kod toplu doğrular `UserId` aracılığıyla sağlanan sorgu dizesi varsa, geçerli olduğunu `Guid` değeri ve var olan bir kullanıcı hesabına başvuruyor. Tüm bu denetimleri geçerse, kullanıcı hesabı onaylanır; Aksi takdirde, uygun bir durum iletisi görüntülenir.

Şekil 7 gösterir `Verification.aspx` sayfasında bir tarayıcıdan ziyaret edildiğinde.


[![Yeni kullanıcı hesabı artık onaylanmış olan](unlocking-and-approving-user-accounts-cs/_static/image20.png)](unlocking-and-approving-user-accounts-cs/_static/image19.png)

**Şekil 7**: Yeni kullanıcının hesabı, artık onaylı ([tam boyutlu görüntüyü görmek için tıklatın](unlocking-and-approving-user-accounts-cs/_static/image21.png))


## <a name="summary"></a>Özet

Üyelik kullanıcı hesaplarının tümünü kullanıcı siteye bağlanabilir olup olmadığını belirleyen iki durumlara sahip: `IsLockedOut` ve `IsApproved`. Bu özelliklerin her ikisi de olmalıdır `true` kullanıcı oturum açma için.

Kullanıcı kilitli durumu, bir bilgisayar korsanının deneme yanılma yöntemleri bir siteye bozucu olasılığını azaltmak için bir güvenlik önlemi olarak kullanılır. Belirli bir sayıda belirli bir zaman penceresi içinde geçersiz oturum açma denemesi olduğunda özellikle, bir kullanıcı kilitli. Bu sınırlar üyelik sağlayıcısı ayarlar aracılığıyla yapılandırılabilir `Web.config`.

Onaylanan durumu yeni kullanıcılar bazı eylemleri ortaya çıkan kadar açmasını engellemek için bir yol yaygın olarak kullanılır. Belki de site yeni hesaplar önce yönetici tarafından onaylanması gerekir veya e-posta adresi doğrulayarak adım 3'te gördüğümüz gibi.

Mutlu programlama!

### <a name="about-the-author"></a>Yazar hakkında

Scott Mitchell, birden çok ASP/ASP.NET Books yazar ve poshbeauty.com sitesinin 4GuysFromRolla.com, Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan  *[Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott, konumunda ulaşılabilir [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) veya kendi blog'da aracılığıyla [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Özel performanstan...

Bu öğretici serisinde, birçok yararlı Gözden Geçiren tarafından gözden geçirildi. Yaklaşan My MSDN makaleleri gözden geçirme ilgileniyor musunuz? Bu durumda, bir satır bana bırak [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](recovering-and-changing-passwords-cs.md)
> [İleri](building-an-interface-to-select-one-user-account-from-many-vb.md)
