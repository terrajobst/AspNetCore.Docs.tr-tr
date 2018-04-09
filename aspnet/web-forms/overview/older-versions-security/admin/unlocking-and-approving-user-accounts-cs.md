---
uid: web-forms/overview/older-versions-security/admin/unlocking-and-approving-user-accounts-cs
title: Kilit açma ve onaylama kullanıcı hesapları (C#) | Microsoft Docs
author: rick-anderson
description: Bu öğretici bir web sayfası yöneticilerin yönetmek nasıl oluşturulduğunu gösterir kullanıcıların kilitli ve durumlarını onaylandı. Yeni kullanıcılar o nasıl de göreceğiz...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2008
ms.topic: article
ms.assetid: 5346aab1-9974-489f-a065-ae3883b8a350
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/admin/unlocking-and-approving-user-accounts-cs
msc.type: authoredcontent
ms.openlocfilehash: 8b3d69e96513192bc73a2a6a7cbb4c6e33eb610b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="unlocking-and-approving-user-accounts-c"></a>Yüklemeyi kaldırma ve onaylama kullanıcı hesapları (C#)
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indirme](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/CS.14.zip) veya [PDF indirin](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/aspnet_tutorial14_UnlockAndApprove_cs.pdf)

> Bu öğretici bir web sayfası yöneticilerin yönetmek nasıl oluşturulduğunu gösterir kullanıcıların kilitli ve durumlarını onaylandı. Yeni kullanıcılar yalnızca kendi e-posta adresi doğruladıktan sonra onaylamak nasıl de göreceğiz.


## <a name="introduction"></a>Giriş

Bir kullanıcı adı, parola ve e-posta ile birlikte her kullanıcı hesabının kullanıcı siteye oturum açabilir dikte iki durum alanları vardır: kilitli ve onaylandı. Belirtilen kaç kez belirtilen sayıda dakika (varsayılan ayarları bir kullanıcı 10 dakika içinde 5 geçersiz oturum açma denemesinden sonra kilitlemesine) içinde geçersiz kimlik bilgilerini sağlarsanız, bir kullanıcı otomatik olarak kilitlidir. Onaylanan durumu, yeni bir kullanıcı siteye oturum açmadan önce bazı eylemler burada gerçekleşen gereken senaryolarda kullanışlıdır. Örneğin, bir kullanıcı ilk kendi e-posta adresini doğrulayın veya oturum açma bölümlemeye önce yönetici tarafından onaylanması gerekebilir.

Çünkü kilitli bir çıkış veya onaylanmamış olamaz kullanıcı oturum açma, bu durumlar nasıl sıfırlanabilir merak ediyor yalnızca doğal. ASP.NET herhangi bir yerleşik işlevsellik içermiyor veya kullanıcıların yönetmek için Web denetimleri kilitlendi ve bu kararların bir siteden siteye temelinde ele alınması gerektiğinden durumları, kısmen onaylandı. Bazı siteler tüm yeni kullanıcı hesapları (varsayılan davranış) otomatik olarak onayla. Başka bir yönetici var yeni hesapları onaylamak ya da bunlar kaydolurken sağlanan e-posta adresine gönderilen bir bağlantıyı ziyaret kadar kullanıcılar değil. Benzer şekilde, bir yönetici durumlarını sıfırlar kadar bazı siteler kullanıcıların oturumunu kilitleme, diğer sitelere bir URL ile kullanıma kilitli kullanıcıya bir e-posta gönderirken kendi hesabının kilidini açmak için ziyaret edebilecekleri.

Bu öğretici bir web sayfası yöneticilerin yönetmek nasıl oluşturulduğunu gösterir kullanıcıların kilitli ve durumlarını onaylandı. Yeni kullanıcılar yalnızca kendi e-posta adresi doğruladıktan sonra onaylamak nasıl de göreceğiz.

## <a name="step-1-managing-users-locked-out-and-approved-statuses"></a>1. adım: Kullanıcıların yönetme ve kilitli durumları Onaylandı

İçinde <a id="Tutorial12"> </a> [ *bir arabirim için bir kullanıcı hesabı seçin birçok özellikten oluşturma* ](building-an-interface-to-select-one-user-account-from-many-cs.md) biz oluşturulan her kullanıcı hesabında bir disk belleği, listelenen bir sayfa öğretici GridView filtrelenir. Izgaranın her bir kullanıcı adı ve e-posta, kendi onaylanmış ve çıkışı kilitli durumları, olup şu anda çevrimiçi ve kullanıcı hakkında herhangi bir açıklama listeler. Kullanıcıların yönetmek için onaylanmış ve durumlarını kilitli, bu kılavuz düzenlenebilir vermiyoruz. Bir kullanıcının onaylanan durumunu değiştirmek için yönetici ilk kullanıcı hesabını bulun ve ardından karşılık gelen GridView satır denetleme veya onaylanan onay kutusunun işaretini kaldırdığınızda düzenleyin. Alternatif olarak, biz ayrı bir ASP.NET sayfası aracılığıyla onaylanmış ve çıkışı kilitli durumları yönetmek.

Bu öğretici için iki ASP.NET sayfaları kullanalım: `ManageUsers.aspx` ve `UserInformation.aspx`. Buradaki olan `ManageUsers.aspx` sistemde, kullanıcı hesaplarını listeler sırada `UserInformation.aspx` yöneticinin onaylanmış ve çıkışı kilitli durumları belirli bir kullanıcı için yönetmenize olanak sağlar. GridView büyütmek için bizim ilk iş sırası olan `ManageUsers.aspx` bağlantılar bir sütun olarak işleyen bir HyperLinkField eklenecek. İşaret etmek için her bir bağlantının istiyoruz `UserInformation.aspx?user=UserName`, burada *kullanıcıadı* düzenlemek için kullanıcı adıdır.

> [!NOTE]
> Kodunu yüklediyseniz <a id="Tutorial13"> </a> [ *kurtarma ve parolaları değiştirme* ](recovering-and-changing-passwords-cs.md) fark, öğretici `ManageUsers.aspx` sayfa zaten bir dizi içeriyor " Bağlantıları Yönet"ve `UserInformation.aspx` sayfası seçilen kullanıcının parolasını değiştirmek için bir arabirim sağlar. Bu işlevselliği üyelik API'si atlamak ve bir kullanıcının parolasını değiştirmek için SQL Server veritabanıyla doğrudan işletim tarafından çalışılan çünkü bu öğretici ile ilişkili kodda çoğaltmayacak vermiştir. Bu öğretici ile sıfırdan başlatır `UserInformation.aspx` sayfası.


### <a name="adding-manage-links-to-theuseraccountsgridview"></a>Ekleme "Manage" Bağlantılar`UserAccounts`GridView

Açık `ManageUsers.aspx` sayfasında ve bir HyperLinkField eklemek `UserAccounts` GridView. HyperLinkField's ayarlamak `Text` "Manage" özelliğine ve kendi `DataNavigateUrlFields` ve `DataNavigateUrlFormatString` özelliklerine `UserName` ve "UserInformation.aspx?user={0}", sırasıyla. Bu ayarları HyperLinkField tüm köprülerin "Manage" metin görüntüler, ancak her bir bağlantının uygun geçirir gibi yapılandırın *kullanıcıadı* sorgu dizesi değeri.

GridView HyperLinkField ekledikten sonra görüntülemek için bir dakikanızı ayırın `ManageUsers.aspx` bir tarayıcı aracılığıyla sayfası. Şekil 1'de görüldüğü gibi her GridView satır şimdi bir "Manage" bağlantısı içerir. Bruce'a "Manage" bağlantı noktaları için `UserInformation.aspx?user=Bruce`, Dave "Manage" bağlantısına işaret ise `UserInformation.aspx?user=Dave`.


[![HyperLinkField ekler bir](unlocking-and-approving-user-accounts-cs/_static/image2.png)](unlocking-and-approving-user-accounts-cs/_static/image1.png)

**Şekil 1**: HyperLinkField her kullanıcı hesabı için bir "Manage" bağlantı ekler ([tam boyutlu görüntüyü görüntülemek için tıklatın](unlocking-and-approving-user-accounts-cs/_static/image3.png))


Kullanıcı arabirimi oluşturur ve için kod `UserInformation.aspx` bir süre, ancak ilk konuşması şimdi sayfa hakkında programlı bir kullanıcı olarak nasıl değiştirildiğini kilitli ve durumlarını onaylandı. [ `MembershipUser` Sınıfı](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx) sahip [ `IsLockedOut` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.islockedout.aspx) ve [ `IsApproved` özellikleri](https://msdn.microsoft.com/library/system.web.security.membershipuser.isapproved.aspx). `IsLockedOut` Özelliği salt okunur durumdadır. Program aracılığıyla bir kullanıcı kilitlemek için bir mekanizma vardır; bir kullanıcının kilidini açmak için kullanmak `MembershipUser` sınıfının [ `UnlockUser` yöntemi](https://msdn.microsoft.com/library/system.web.security.membershipuser.unlockuser.aspx). `IsApproved` Özelliği okunabilir ve yazılabilir. Bu özellik için değişiklikleri kaydetmek için aranacak ihtiyacımız `Membership` sınıfının [ `UpdateUser` yöntemi](https://msdn.microsoft.com/library/system.web.security.membership.updateuser.aspx), değiştirilmiş içinde geçen `MembershipUser` nesnesi.

Çünkü `IsApproved` özelliği okunabilir ve yazılabilir, bir onay kutusu denetimi büyük olasılıkla bu özellik yapılandırmak için en iyi bir kullanıcı arabirimi öğesi değil. Ancak, bir onay kutusu için çalışmaz `IsLockedOut` özelliği yönetici bir kullanıcı kilitlenemiyor olduğundan, bunları bir kullanıcı kilidini yalnızca. Uygun kullanıcı arabirimi için `IsLockedOut` özelliği olan bir düğme, tıklatıldığında, kullanıcı hesabının kilidini açar. Kullanıcıya kilitlenmişse bu düğme yalnızca etkinleştirilmiş olmalıdır.

### <a name="creating-theuserinformationaspxpage"></a>Oluşturma`UserInformation.aspx`sayfası

Biz artık kullanıcı arabiriminde uygulamak hazır `UserInformation.aspx`. Bu sayfayı açın ve aşağıdaki Web denetimleri ekleyin:

- Köprü denetim, tıklatıldığında yöneticinin döndürür `ManageUsers.aspx` sayfası.
- Seçilen kullanıcının adını görüntülemek için bir etiket Web denetimi. Bu etiketin ayarlamak `ID` için `UserNameLabel` ve temizleyin, `Text` özelliği.
- Adlı bir onay kutusu denetimi `IsApproved`. Ayarlama, `AutoPostBack` özelliğine `true`.
- Kullanıcı görüntülemek için bir etiket denetimi son tarihini kilitli. Bu etiket adı `LastLockedOutDateLabel` ve temizleyin, `Text` özelliği.
- Kullanıcının kilidini açmak için bir düğme. Bu düğme adının `UnlockUserButton` ve kendi `Text` "Kilidini kullanıcı" özelliğine.
- "Kullanıcının onaylanan durumu güncelleştirildi,."gibi durum iletilerini görüntülemek için bir etiket denetimi Bu denetim adı `StatusMessage`, kullanıma açık kendi `Text` özelliği ve kümesi kendi `CssClass` özelliğine `Important`. ( `Important` CSS sınıfı tanımlanmış `Styles.css` stil sayfası dosyası; büyük, kırmızı yazı tipiyle ilgili metin gösterir.)

Bu denetimler eklendikten sonra Visual Studio Tasarım görünümünde Şekil 2'de ekran şuna benzemelidir.


[![UserInformation.aspx için kullanıcı arabirimi oluşturma](unlocking-and-approving-user-accounts-cs/_static/image5.png)](unlocking-and-approving-user-accounts-cs/_static/image4.png)

**Şekil 2**: için kullanıcı arabirimi oluşturma `UserInformation.aspx` ([tam boyutlu görüntüyü görüntülemek için tıklatın](unlocking-and-approving-user-accounts-cs/_static/image6.png))


Kullanıcı arabirimi tam bizim sonraki görev ayarlamaktır `IsApproved` onay kutusunu ve diğer denetimlerin göre seçilen kullanıcının bilgi. Sayfa için bir olay işleyicisi oluşturun `Load` olay ve aşağıdaki kodu ekleyin:

[!code-csharp[Main](unlocking-and-approving-user-accounts-cs/samples/sample1.cs)]

Yukarıdaki kod, bu sayfaya ve değil sonraki geri gönderimin ilk kez ziyaret ediyorsunuz sağlayarak başlatır. Geçirilecek kullanıcı adı daha sonra okur `user` querystring alan ve bu kullanıcı hesabı ile ilgili bilgileri alır `Membership.GetUser(username)` yöntemi. Hiçbir kullanıcı adı sorgu dizesi sağlandı veya belirtilen kullanıcı bulunamadı, yönetici geri gönderilir `ManageUsers.aspx` sayfası.

`MembershipUser` Nesnenin `UserName` değeri görüntülenen sonra `UserNameLabel` ve `IsApproved` onay kutusu işaretli göre `IsApproved` özellik değeri.

`MembershipUser` Nesnenin [ `LastLockoutDate` özelliği](https://msdn.microsoft.com/library/system.web.security.membershipuser.lastlockoutdate.aspx) döndürür bir `DateTime` zaman kullanıcı son gösteren değer kilitlendi. Kullanıcı hiçbir zaman kilitlenmişse, döndürülen değer üyelik sağlayıcısına bağlıdır. Yeni bir hesap oluşturulduğunda, `SqlMembershipProvider` ayarlar `aspnet_Membership` tablonun `LastLockoutDate` alanı `1754-01-01 12:00:00 AM`. Yukarıdaki kod içindeki boş bir dize görüntüler `LastLockoutDateLabel` varsa `LastLockoutDate` özelliği yıl önce gerçekleşir 2000; Aksi takdirde tarih bölümü `LastLockoutDate` özelliği etiketinde görüntülenir. `UnlockUserButton'` s `Enabled` özelliği, kullanıcının kullanıcıya kilitlenmişse bu düğme yalnızca etkin olacağını anlamına gelen durum kilitli ayarlanır.

Test etmek için bir dakikanızı ayırın `UserInformation.aspx` bir tarayıcı aracılığıyla sayfası. Elbette, başlangıç yapmanız gerekir `ManageUsers.aspx` ve yönetmek için bir kullanıcı hesabı seçin. Geliş bağlı `UserInformation.aspx`, unutmayın `IsApproved` onay kutusunu kullanıcı onaylanırsa yalnızca denetlenir. Kullanıcının herhangi bir zamanda kilitlenmişse, kendi son tarihini kilitli görüntülenir. Yalnızca kullanıcı şu anda kilitli kilidini kullanıcı düğmesi etkinleştirilir. Denetleme veya işaretleyerek `IsApproved` onay kutusunu veya kilidini kullanıcı düğmesini tıklatarak geri gönderimin neden olur, ancak biz için henüz olduğunuz çünkü değişikliğe kullanıcı hesabına yapılan bu olayları için olay işleyicileri oluşturma.

Visual Studio'ya dönmek ve olay işleyicileri oluşturma `IsApproved` CheckBox'ın `CheckedChanged` olay ve `UnlockUser` düğmenin `Click` olay. İçinde `CheckedChanged` olay işleyici ayarlamayı kullanıcının `IsApproved` özelliğine `Checked` özelliği onay kutusunu ve ardından bir çağrı aracılığıyla değişiklikleri kaydetme `Membership.UpdateUser`. İçinde `Click` olay işleyicisi, yalnızca çağrısı `MembershipUser` nesnesinin `UnlockUser` yöntemi. Her iki olay işleyicileri uygun bir ileti görüntüler `StatusMessage` etiketi.

[!code-csharp[Main](unlocking-and-approving-user-accounts-cs/samples/sample2.cs)]

### <a name="testing-theuserinformationaspxpage"></a>Sınama`UserInformation.aspx`sayfası

Yerinde bu olay işleyicileri ile sayfa yeniden ziyaret ve onaylanmamış bir kullanıcı. Şekil 3'te gösterildiği gibi kullanıcının gösteren sayfasında ileti kısaca görmelisiniz `IsApproved` özelliği başarıyla değiştirildi.


[![Chris Onaylanmadı bırakıldı](unlocking-and-approving-user-accounts-cs/_static/image8.png)](unlocking-and-approving-user-accounts-cs/_static/image7.png)

**Şekil 3**: Chris Onaylanmadı süredir ([tam boyutlu görüntüyü görüntülemek için tıklatın](unlocking-and-approving-user-accounts-cs/_static/image9.png))


Ardından, oturum kapatma ve hesabı kullanıcı olarak oturum açmayı deneyin yalnızca onaylanmadı. Kullanıcı onaylanmamış olduğundan oturum açılamaz. Kullanıcı oturum açılamaz neden bağımsız olarak, varsayılan olarak, oturum açma denetimi aynı iletisi görüntüler. Ancak <a id="Tutorial6"> </a> [ *doğrulama kullanıcı kimlik bilgilerini karşı üyeliği kullanıcı deposu* ](../membership/validating-user-credentials-against-the-membership-user-store-cs.md) Aranan daha uygun bir mesaj görüntülemek için oturum açma denetimi geliştirme sırasında öğretici. Şekil 4'te gösterildiği gibi Chris, hesabı henüz onaylanmamış çünkü kendisine oturum açılamaz olduğunu belirten bir ileti gösterilir.


[![Chris olamaz oturum açma için HIS Onaylanmadı hesabıdır](unlocking-and-approving-user-accounts-cs/_static/image11.png)](unlocking-and-approving-user-accounts-cs/_static/image10.png)

**Şekil 4**: Chris olamaz oturum açma için HIS hesabıdır Onaylanmadı ([tam boyutlu görüntüyü görüntülemek için tıklatın](unlocking-and-approving-user-accounts-cs/_static/image12.png))


Out kilitli işlevselliğini sınamak için onaylanmış bir kullanıcı olarak oturum açma, ancak yanlış bir parola kullanmak girişimi. Bu işlem gerekli sayısı için kullanıcı hesabına kilitlendi kadar yineleyin. Oturum açma denetimi özel göstermek için aynı zamanda güncelleştirildiği çıkışı kilitli bir hesaptan oturum açma girişimi varsa iletisi. Oturum açma sayfasına aşağıdaki iletiyi görmeye başlayacaksınız sonra bir hesap kilitlendi olduğunu bildiğiniz: "hesabınız çok fazla geçersiz oturum açma denemesi nedeniyle kilitlendi. Lütfen hesabınızın kilidini için yöneticiye başvurun".

Geri dönüp `ManageUsers.aspx` sayfasında ve çıkışı kilitli kullanıcı Yönet bağlantısına tıklayın. Şekil 5 gösterildiği gibi bir değer görmelisiniz `LastLockedOutDateLabel` kilidini kullanıcı düğmesi etkinleştirilmiş olmalıdır. Kullanıcı hesabının kilidini açmak için kullanıcı kilidini aç düğmesini tıklatın. Kullanıcı kilidi sonra tekrar oturum açabilmeniz olacaktır.


[![Sistem dışı Dave kilitlendi](unlocking-and-approving-user-accounts-cs/_static/image14.png)](unlocking-and-approving-user-accounts-cs/_static/image13.png)

**Şekil 5**: Dave sahip edilmiş kilitli çıkış sisteminin ([tam boyutlu görüntüyü görüntülemek için tıklatın](unlocking-and-approving-user-accounts-cs/_static/image15.png))


## <a name="step-2-specifying-new-users-approved-status"></a>2. adım: Yeni kullanıcıları belirten durum Onaylandı

Onaylanan durum yeni bir kullanıcı oturum açma ve sitenin kullanıcıya özgü özelliklere erişmek açmadan önce gerçekleştirilmesi gereken bazı eylemleri istediğiniz senaryolarda kullanışlıdır. Örneğin, oturum açma ve kaydolma sayfaları dışında tüm sayfaları yalnızca kimliği doğrulanmış kullanıcılar için erişilebilir olduğu özel bir Web sitesi çalışıyor olabilir. Ancak bir yabancı Web sitenizi ulaşırsa olanlar kaydolma sayfasına bulur ve bir hesap oluşturur? Bunun olmaması için kaydolma sayfasına gitmek bir `Administration` klasörünü ve bir yönetici el ile her hesabı oluşturmanızı gerektirir. Alternatif olarak, herkesin kaydolma sağlayan ancak bir yönetici kullanıcı hesabı onaylanana kadar sitesine erişimi engelle.

Varsayılan olarak, yeni hesapları CreateUserWizard denetim onaylar. Denetimin kullanarak bu davranışı yapılandırabilirsiniz [ `DisableCreatedUser` özelliği](https://msdn.microsoft.com/en-gb/library/system.web.ui.webcontrols.createuserwizard.disablecreateduser.aspx). Bu özelliği ayarlamak `true` yeni kullanıcı hesapları onaylamanız değil.

> [!NOTE]
> Varsayılan olarak CreateUserWizard denetim yeni kullanıcı hesabı otomatik olarak günlüğe kaydeder. Bu davranış denetimin tarafından dikte edilir [ `LoginCreatedUser` özelliği](https://msdn.microsoft.com/en-gb/library/system.web.ui.webcontrols.createuserwizard.logincreateduser.aspx). Onaylanmamış kullanıcıların siteye oturum açılamaz çünkü zaman `DisableCreatedUser` olan `true` yeni kullanıcı hesabı değerini bakılmaksızın sitede oturum açmadığı `LoginCreatedUser` özelliği.


Yeni kullanıcı hesapları aracılığıyla programlı olarak oluşturuyorsanız `Membership.CreateUser` onaylanmamış kullanıcı hesabı oluşturmak için yöntem, yeni kullanıcının kabul aşırı birini kullanın `IsApproved` giriş parametresi olarak özellik değeri.

## <a name="step-3-approving-users-by-verifying-their-email-address"></a>3. adım: Kullanıcılar kendi e-posta adresi doğrulayarak onaylama

Kullanıcı hesaplarını destekleyen birçok Web sitesi kaydedilirken sağlanan e-posta adresini doğrulayın kadar yeni kullanıcılar onaylamanız değil. Bu doğrulama işlemi, yaygın olarak bir benzersiz, doğrulanmış e-posta adresi gerektirir ve kaydolma işlemini ek bir adım ekler bot, istenmeyen posta gönderenlerin ve diğer ne'er-do-wells engel olmak için kullanılır. Bu modelde, yeni bir kullanıcı oturum açtığında doğrulama sayfasına bir bağlantı içeren bir e-posta iletisi gönderilir. Bağlantıyı ziyaret ederek kullanıcı e-posta aldı ve bu nedenle, e-posta sağlanan adres geçerli kanıtlanmış. Doğrulama sayfasında, kullanıcı onaylama için sorumludur. Bu otomatik olarak, böylece bu sayfayı ulaştığında herhangi bir kullanıcı onaylama kaynaklanıyor olabilir ya da yalnızca kullanıcının gibi bazı ek bilgiler sağlar sonra bir [CAPTCHA](http://en.wikipedia.org/wiki/Captcha).

Bu iş akışı uyum sağlayacak şekilde Biz yeni kullanıcılar onaylanmamış; böylece ilk hesap oluşturma sayfası güncelleştirmeniz gerekir. Açık `EnhancedCreateUserWizard.aspx` sayfasındaki `Membership` klasörü ve CreateUserWizard denetim kümesinin `DisableCreatedUser` özelliğine `true`.

Ardından, yeni kullanıcı hesabını onaylamak yönergelerini içeren bir e-posta göndermek için CreateUserWizard denetimini yapılandırmak ihtiyacımız. Özellikle, biz bir bağlantı için e-postadaki içerir `Verification.aspx` sayfa (hangi biz için henüz olduğunuz oluşturma), yeni kullanıcının geçen `UserId` querystring aracılığıyla. `Verification.aspx` Sayfasında belirtilen kullanıcı için arama ve bunları onaylanmış işaretleyin.

### <a name="sending-a-verification-email-to-new-users"></a>Yeni kullanıcılar için bir doğrulama e-postası gönderme

CreateUserWizard denetiminden bir e-posta göndermek için yapılandırma, `MailDefinition` özelliği uygun şekilde. ' Da anlatıldığı gibi <a id="Tutorial13"> </a> [önceki öğretici](recovering-and-changing-passwords-cs.md), parola değiştirme ve PasswordRecovery denetimleri içerir bir [ `MailDefinition` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.maildefinition.aspx) aynı şekilde çalışır CreateUserWizard denetimin.

> [!NOTE]
> Kullanılacak `MailDefinition` posta teslim belirtmeniz gerekir özellik seçenekleri `Web.config`. Daha fazla bilgi için bkz [ASP.NET e-posta gönderme](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx).


Başlangıç adlı yeni bir e-posta şablonu oluşturarak `CreateUserWizard.txt` içinde `EmailTemplates` klasör. Aşağıdaki metni için şablonu kullanın:

[!code-aspx[Main](unlocking-and-approving-user-accounts-cs/samples/sample3.aspx)]

Ayarlama `MailDefinition'` s `BodyFileName` özelliğine "~ / EmailTemplates/CreateUserWizard.txt" ve kendi `Subject` özelliğine "My Web sitesine Hoş Geldiniz! Lütfen hesabınızı etkinleştirin."

Unutmayın `CreateUserWizard.txt` e-posta şablonu içeren bir `<%VerificationUrl%>` yer tutucu. Bu yerdir URL'sini `Verification.aspx` sayfa yerleştirilir. CreateUserWizard otomatik olarak değiştirir `<%UserName%>` ve `<%Password%>` yer tutucuları yeni hesabın kullanıcı adı ve parola, yerleşik bir ancak `<%VerificationUrl%>` yer tutucu. El ile uygun doğrulama URL'si ile değiştirmeniz gerekir.

Bunu başarmak için olay işleyicisi CreateUserWizard için 's oluşturmak [ `SendingMail` olay](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.sendingmail.aspx) ve aşağıdaki kodu ekleyin:

[!code-csharp[Main](unlocking-and-approving-user-accounts-cs/samples/sample4.cs)]

`SendingMail` Olay harekete sonra `CreatedUser` olayı, zaman yukarıdaki olay işleyicisi yürütür yeni kullanıcı hesabı zaten oluşturulduğunu anlamına gelir. Yeni kullanıcının erişmek için `UserId` çağırarak değeri `Membership.GetUser` tümleştirilmesidir yöntemi `UserName` CreateUserWizard denetime girilen. Ardından, doğrulama URL'si biçimlendirilmemiş. Deyim `Request.Url.GetLeftPart(UriPartial.Authority)` döndürür `http://yourserver.com` URL'sinin; `Request.ApplicationPath` uygulama kökü burada döndürür yolu. URL olarak tanımlanan sonra doğrulama `Verification.aspx?ID=userId`. Bu iki dizeler ardından tam URL'yi oluşturmak üzere birleşir. Son olarak, e-posta ileti gövdesi (`e.Message.Body`) tüm oluşumlarını sahip `<%VerificationUrl%>` tam URL ile değiştirilir.

Siteye oturum açamıyor yani yeni kullanıcılar onaylanmamış, net etkisidir. Ayrıca, otomatik olarak bir bağlantı içeren bir e-posta doğrulama URL'si gönderilmeden (bkz. Şekil 6).


[![Yeni kullanıcı doğrulama URL'si için bir bağlantı içeren bir e-posta alır](unlocking-and-approving-user-accounts-cs/_static/image17.png)](unlocking-and-approving-user-accounts-cs/_static/image16.png)

**Şekil 6**: yeni kullanıcı doğrulama URL'si için bir bağlantı içeren bir e-posta alır ([tam boyutlu görüntüyü görüntülemek için tıklatın](unlocking-and-approving-user-accounts-cs/_static/image18.png))


> [!NOTE]
> CreateUserWizard denetimin varsayılan CreateUserWizard adım kullanıcı hesaplarında oluşturulup oluşturulmadığını ve devam et düğmesi görüntüler bildiren bir ileti görüntüler. Bu tıklamak götürür kullanıcı denetimin tarafından belirtilen URL'ye `ContinueDestinationPageUrl` özelliği. CreateUserWizard `EnhancedCreateUserWizard.aspx` yeni kullanıcılara göndermek için yapılandırılan `~/Membership/AdditionalUserInfo.aspx`, kullanıcı kendi memleket, giriş sayfası URL'si ve imza için ister. Bu bilgiler yalnızca olarak oturum açmış kullanıcılarda eklenebilir olduğundan, kullanıcılar sitenin giriş sayfası göndermek için bu özelliği güncelleştirmek için bir anlam (`~/Default.aspx`). Ayrıca, `EnhancedCreateUserWizard.aspx` sayfa veya CreateUserWizard adım genişletilebilir bir doğrulama e-postası gönderildi ve bu e-posta'ndaki yönergeleri izleyin kadar hesaplarında etkin olmaz kullanıcı bilgilendirmek için. Bu değişiklikler için okuyucu bir alıştırma olarak bırakın.


### <a name="creating-the-verification-page"></a>Doğrulama sayfası oluşturma

Bizim son görevi oluşturmaktır `Verification.aspx` sayfası. Bu sayfa ile ilişkilendirme kök klasörüne eklemek `Site.master` ana sayfa. Biz siteye eklenen önceki içerik sayfaların çoğu ile yaptığınız gibi başvuran içerik denetimi kaldırılmaya `LoginContent` ContentPlaceHolder içerik sayfasını ana sayfanın kullanmayacağından varsayılan içerik.

Bir etiket Web denetimine ekleme `Verification.aspx` sayfasında kendi `ID` için `StatusMessage` ve metin özelliğini temizleyin. Ardından, oluşturun `Page_Load` olay işleyicisi ve aşağıdaki kodu ekleyin:

[!code-csharp[Main](unlocking-and-approving-user-accounts-cs/samples/sample5.cs)]

Yukarıdaki kod toplu doğrular `UserId` aracılığıyla sağlanan sorgu dizesi varsa, geçerli bir olduğunu `Guid` değeri ve var olan bir kullanıcı hesabına başvuruyor. Tüm bu denetimleri geçerse, kullanıcı hesabı onaylanır; Aksi takdirde, uygun durum iletisi görüntülenir.

Şekil 7 gösterir `Verification.aspx` sayfasında bir tarayıcıdan sitesini ziyaret ettiğinizde.


[![Yeni kullanıcı hesabı artık onaylanmış olan](unlocking-and-approving-user-accounts-cs/_static/image20.png)](unlocking-and-approving-user-accounts-cs/_static/image19.png)

**Şekil 7**: yeni kullanıcı hesabı olan şimdi onaylanmış ([tam boyutlu görüntüyü görüntülemek için tıklatın](unlocking-and-approving-user-accounts-cs/_static/image21.png))


## <a name="summary"></a>Özet

Tüm üyeliği kullanıcı hesapları kullanıcı siteye oturum olup olmadığını belirleyen iki durumlara sahip: `IsLockedOut` ve `IsApproved`. Bu özelliklerin her ikisi olmalıdır `true` kullanıcı oturum açma için.

Kullanıcı kilitli durumu, bir siteye yanılma yöntemlerle yeni bir bilgisayar korsanının olasılığını azaltmak için bir güvenlik önlemi olarak kullanılır. Belirli bir sayıda belirli bir zaman aralığı içinde geçersiz oturum açma denemesi varsa özellikle, bir kullanıcının kilitlendiği. Bu sınırlar içinde üyelik sağlayıcısı ayarları aracılığıyla yapılandırılabilir `Web.config`.

Onaylanan durumu bazı eylemleri ortaya çıkan kadar açmasını yeni kullanıcılar engellemek için bir araç olarak yaygın olarak kullanılır. Belki de site yeni hesapları önce yönetici tarafından onaylanması gerekir veya kendi e-posta adresi doğrulayarak adım 3'te gördüğümüz gibi.

Mutluluk programlama!

### <a name="about-the-author"></a>Yazar hakkında

Scott Mitchell, birden çok ASP/ASP.NET books yazar ve 4GuysFromRolla.com, kurucusu 1998 itibaren Microsoft Web teknolojileri ile çalışmaktadır. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri  *[kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Tan adresindeki ulaşılabilir [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) veya kendi blog aracılığıyla [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Özel teşekkürler...

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bir satırında bana bırak [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](recovering-and-changing-passwords-cs.md)
> [sonraki](building-an-interface-to-select-one-user-account-from-many-vb.md)
