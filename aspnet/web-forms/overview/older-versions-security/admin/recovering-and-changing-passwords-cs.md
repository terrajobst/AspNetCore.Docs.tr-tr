---
uid: web-forms/overview/older-versions-security/admin/recovering-and-changing-passwords-cs
title: (C#) parolaları kurtarma ve değiştirme | Microsoft Docs
author: rick-anderson
description: ASP.NET parolaları kurtarma ve değiştirme ile Yardım için iki Web denetimleri içerir. Kayıp pa, kurtarılır ziyaretçisi PasswordRecovery denetimi sağlar...
ms.author: aspnetcontent
ms.date: 04/01/2008
ms.assetid: 19c4d042-4e34-4b44-9f1d-6bf2253ba366
msc.legacyurl: /web-forms/overview/older-versions-security/admin/recovering-and-changing-passwords-cs
msc.type: authoredcontent
ms.openlocfilehash: e63d3d1153c81c1bb54a1fb6bb242df032899511
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37824344"
---
<a name="recovering-and-changing-passwords-c"></a>(C#) parolaları kurtarma ve değiştirme
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indir](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/CS.13.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/aspnet_tutorial13_ChangingPasswords_cs.pdf)

> ASP.NET parolaları kurtarma ve değiştirme ile Yardım için iki Web denetimleri içerir. Kayıp parolasını kurtarmayı ziyaretçisi PasswordRecovery denetimi sağlar. ChangePassword denetimi, kullanıcının parolasını güncelleştirme izin verir. Bu öğretici serisinin PasswordRecovery anlatıldığı gibi diğer oturum açma ile ilgili Web denetimleri ve sıfırlama veya kullanıcıların parolalarını değiştirmek için arka planda üyelik framework ile çalışma ChangePassword denetler.


## <a name="introduction"></a>Giriş

My banka, yardımcı şirket, telefon şirketi, e-posta hesaplarına ve kişiselleştirilmiş web portalı için Web siteleri arasında, çoğu kişi gibi hatırlamak farklı parolalar onlarca sahibim. Bugünlerde ezberlemeniz çok sayıda kimlik bilgileriyle kişilerin, Parolanızı unutmanız için sık karşılaşılan bir durum değil. Bu hesap için kullanıcı hesapları sunan Web siteleri bir kullanıcının parolasını kurtarmak bir yol içermesi gerekir. Bu işlem genellikle yeni, rastgele bir parola oluşturma ve dosya çubuğunda kullanıcının e-posta adresine eposta içerir. Yeni parolalarını aldıktan sonra çoğu kullanıcının siteye geri dönün ve daha etkileyici bir rastgele oluşturulmuş bir kullanıcının parolasını değiştirin.

ASP.NET parolaları kurtarma ve değiştirme ile Yardım için iki Web denetimleri içerir. Kayıp parolasını kurtarmayı ziyaretçisi PasswordRecovery denetimi sağlar. ChangePassword denetimi, kullanıcının parolasını güncelleştirme izin verir. Bu öğretici serisinin PasswordRecovery anlatıldığı gibi diğer oturum açma ile ilgili Web denetimleri ve sıfırlama veya kullanıcıların parolalarını değiştirmek için arka planda üyelik framework ile çalışma ChangePassword denetler.

Bu öğreticide bu iki denetimi kullanarak inceleyeceğiz. Program aracılığıyla değiştirme ve aracılığıyla bir kullanıcının parolasını sıfırlama de göreceğiz `MembershipUser` sınıfın `ChangePassword` ve `ResetPassword` yöntemleri.

## <a name="step-1-helping-users-recover-lost-passwords"></a>1. adım: Parola kayıp yardımcı kullanıcılar Kurtar

Kullanıcı hesaplarının desteklediği tüm Web sitelerinin kullanıcılara kullanıcıların Unutulan parolaları kurtarmak için bazı mekanizma sağlamanız gerekir. Güzel bir haberimiz var gibi işlevselliği ASP.NET'te uygulama PasswordRecovery Web denetimi sayesinde barındırmamıza olmasıdır. Kullanıcıdan kullanıcı adı için bir arabirim PasswordRecovery denetimi yapar ve gerekirse, kendi güvenlik sorusuna verilen yanıt. Ardından kullanıcı parolalarını e.

> [!NOTE]
> E-posta iletilerini düz metin hat üzerinden iletilen olduğundan güvenlik riskleri ile bir kullanıcının parolasını e-posta ile gönderme kullanılan vardır.


PasswordRecovery denetimi üç görünüm içerir:

- **Kullanıcı adı** -ziyaretçi için kullanıcı adı ister. Bu ilk görünümdür.
- **Soru**-kullanıcının kullanıcı adı ve güvenlik sorusunu metin, metin kutusu için kendi güvenlik sorusuna verilen yanıt girmesini birlikte görüntüler.
- **Başarı**-kullanıcının parolasını kaydedilse bildiren bir ileti görüntüler.

Görünümler görüntülenir ve aşağıdaki üyelik yapılandırma ayarlarına PasswordRecovery denetimi tarafından gerçekleştirilen eylemler bağlıdır:

- `RequiresQuestionAndAnswer`
- `EnablePasswordRetrieval`
- `EnablePasswordReset`

Üyelik framework'ün `RequiresQuestionAndAnswer` ayarı, kullanıcıların bir güvenlik sorusunu ve yanıtını bir hesap için kaydolurken belirtmelisiniz olup olmadığını belirtir. Açıkladığımız gibi <a id="_msoanchor_1"> </a> [ *kullanıcı hesapları oluşturma* ](../membership/creating-user-accounts-cs.md) Öğreticisi, eğer `RequiresQuestionAndAnswer` CreateUserWizard'ın arabirimi TextBox içerir sonra True (varsayılan) ise Yeni kullanıcının güvenlik sorusu ve yanıtı için denetimleri; varsa `RequiresQuestionAndAnswer` yanlış, bu tür hiçbir bilgi toplanmaz. Benzer şekilde, varsa `RequiresQuestionAndAnswer` True ve ardından kullanıcının kullanıcı adı girmesinden sonra sorunun görüntüleyin PasswordRecovery denetim görüntüler; yalnızca kullanıcı doğru güvenlik yanıtı girerse parola kurtarılır. Varsa `RequiresQuestionAndAnswer` PasswordRecovery denetimi doğrudan kullanıcı adı görünümünden başarı görünümüne taşır. ancak, False ' tır.

Kullanıcı kendi kullanıcı adı - veya kendi kullanıcı adı ve güvenlik yanıt, sağlanan sonra `RequiresQuestionAndAnswer` true'dur - PasswordRecovery kullanıcının parolasını e-posta gönderir. Varsa `EnablePasswordRetrieval` seçeneğini True olarak ayarlayın ve ardından, geçerli parolaları kullanıcının e-postayla. False olarak ayarlarsanız ve `EnablePasswordReset` PasswordRecovery denetimi, kullanıcı için yeni, rastgele bir parola oluşturur ve bu yeni bir parola onlara e-postaları True olarak ayarlanır. Her iki `EnablePasswordRetrieval` ve `EnablePasswordReset` False, PasswordRecovery denetimi bir özel durum oluşturur.

> [!NOTE]
> Bu geri çağırma `SqlMembershipProvider` kullanıcıların parolalarını üç biçimlerinden birinde depolar: Temizle, Hashed (varsayılan) veya şifreli. Kullanılan depolama mekanizmasını üyelik yapılandırma ayarlarına bağlıdır; demo uygulamayı Hashed parolası biçimini kullanır. Hashed parolası biçimi kullanılırken `EnablePasswordRetrieval` seçeneği sistem gerçek kullanıcının parolasını veritabanında depolanan karma sürümünden belirleyemediğinden False olarak ayarlanmalıdır.


Şekil 1 nasıl PasswordRecovery'nın arabirimi ve davranışı etkilenir ile üyelik yapılandırmayı gösterir.


[![RequiresQuestionAndAnswer EnablePasswordRetrieval ve EnablePasswordReset PasswordRecovery denetimin görünümünü ve davranışını etkiler.](recovering-and-changing-passwords-cs/_static/image2.png)](recovering-and-changing-passwords-cs/_static/image1.png)

**Şekil 1**: `RequiresQuestionAndAnswer`, `EnablePasswordRetrieval`, ve `EnablePasswordReset` PasswordRecovery denetimin görünümünü ve davranışını etkileyen ([tam boyutlu görüntüyü görmek için tıklatın](recovering-and-changing-passwords-cs/_static/image3.png))


> [!NOTE]
> İçinde <a id="_msoanchor_2"> </a> [ *SQL Server'da üyelik şeması oluşturma* ](../membership/creating-the-membership-schema-in-sql-server-cs.md) biz yapılandırılmış üyelik sağlayıcısını ayarlayarak öğretici `RequiresQuestionAndAnswer` true olarak `EnablePasswordRetrieval` için Yanlış ve `EnablePasswordReset` true.


### <a name="using-the-passwordrecovery-control"></a>PasswordRecovery denetimini kullanma

Bir ASP.NET sayfasında PasswordRecovery denetimi kullanarak bakalım. Açık `RecoverPassword.aspx` ve sürükleyin ve tasarımcı araç kutusundan PasswordRecovery denetimi bırakın; olarak kendi `ID` için `RecoverPwd`. Oturum açma ve CreateUserWizard Web denetimleri gibi etiketler, metin kutuları, düğmeler ve doğrulama denetimleri içeren zengin bir bileşik arabirimi PasswordRecovery denetimin görünümleri işleyin. Denetimin stil özellikleri aracılığıyla veya görünümleri şablonlara çevirmek görünümleri görünümünü özelleştirebilirsiniz. Ben bunu bir alıştırma olarak için ilginizi okuyucu bırakın.

Bir kullanıcı bu sayfayı ziyaret ettiğinde o kişinin kullanıcı adı girin ve Gönder düğmesine tıklayın. Biz ayarladığınızdan `RequiresQuestionAndAnswer` özelliği true olarak bizim PasswordRecovery üyelik yapılandırma ayarlarını kontrol edecek sonra soru görünümünü görüntüleyin. Kullanıcı kendi doğru güvenlik yanıtı girer ve Gönder'i tıklatır sonra PasswordRecovery denetimi rastgele oluşturulmuş bir kullanıcının parolasını güncelleştirin ve bu parola dosya çubuğunda e-posta adresine e-posta. Tüm olası bize tek satırlık bir kod yazmak zorunda!

Bu sayfayı test etmeden önce bir son parçası eğilimindedir yapılandırmasının yoktur: posta teslim ayarları belirtmek ihtiyacımız `Web.config`. PasswordRecovery denetimi, e-posta göndermek için bu ayarları kullanır.

Posta teslim yapılandırması aracılığıyla belirtilen [ `<system.net>` öğesi](https://msdn.microsoft.com/library/6484zdc1.aspx)'s [ `<mailSettings>` öğesi](https://msdn.microsoft.com/library/w355a94k.aspx). Kullanım [ `<smtp>` öğesi](https://msdn.microsoft.com/library/ms164240.aspx) teslimat yöntemini ve varsayılan adresinden belirtmek için. Aşağıdaki biçimlendirmede adlı bir ağ SMTP sunucusu kullanmak için e-posta ayarlarını yapılandırır `smtp.example.com` 25 numaralı bağlantı noktasında ve kullanıcı adı/parola kimlik kullanıcı adı ve parola.

> [!NOTE]
> `<system.net>` kök bir alt öğesidir `<configuration>` öğesi ve bir eşdüzeyi `<system.web>`. Bu nedenle, değil put `<system.net>` öğesiyle `<system.web>` öğesi; bunun yerine, bunu aynı düzeyde koyun.


[!code-xml[Main](recovering-and-changing-passwords-cs/samples/sample1.xml)]

Bir SMTP sunucusu ağ üzerinde kullanmanın yanı sıra, bir toplama dizini gönderilecek e-posta iletilerini burada borç alternatif olarak belirtebilirsiniz.

SMTP ayarlarını yapılandırdıktan sonra ziyaret `RecoverPassword.aspx` tarayıcısından sayfası. İlk kullanıcı deposunda mevcut olmayan bir kullanıcı adı girmeyi deneyin. Şekil 2 gösterildiği gibi PasswordRecovery denetimi kullanıcı bilgilerini erişilemedi belirten bir ileti görüntüler. İleti metni denetimin özelleştirilebilir [ `UserNameFailureText` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.usernamefailuretext.aspx).


[![Geçersiz kullanıcı adı girildiğinde bir hata iletisi görüntülenir](recovering-and-changing-passwords-cs/_static/image5.png)](recovering-and-changing-passwords-cs/_static/image4.png)

**Şekil 2**: geçersiz kullanıcı adı girildiğinde bir hata iletisi görüntülenir ([tam boyutlu görüntüyü görmek için tıklatın](recovering-and-changing-passwords-cs/_static/image6.png))


Artık bir kullanıcı adı girin. Sisteminde erişebileceğiniz ve, güvenlik yanıt e-posta adresine sahip bir hesabın kullanıcı adını biliyorsanız kullanın. Kullanıcı adı girerek ve Gönder seçeneğine sonra PasswordRecovery denetimi soru görünümünü görüntüler. Olarak kullanıcı adı görünümüyle girerseniz yanlış bir yanıt bir hata iletisi (bkz: Şekil 3) PasswordRecovery denetim görüntüler. Kullanım [ `QuestionFailureText` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.questionfailuretext.aspx) bu hata iletisini özelleştirmek için.


[![Kullanıcı geçersiz güvenlik yanıtı girerse bir hata iletisi görüntülenir](recovering-and-changing-passwords-cs/_static/image8.png)](recovering-and-changing-passwords-cs/_static/image7.png)

**Şekil 3**: geçersiz bir güvenlik yanıtı kullanıcının girdiği durumlarda bir hata iletisi görüntülenir ([tam boyutlu görüntüyü görmek için tıklatın](recovering-and-changing-passwords-cs/_static/image9.png))


Son olarak, doğru güvenlik yanıtı girin ve Gönder'e tıklayın. Planda, PasswordRecovery denetimi rastgele bir parola oluşturur, kullanıcı hesabına atar, kullanıcının yeni parolasını, bildiren bir e-posta gönderir (bkz. Şekil 4) ve ardından başarı görünümünü görüntüler.


[![Kullanıcı HIS yeni bir parola içeren bir e-posta gönderilir.](recovering-and-changing-passwords-cs/_static/image11.png)](recovering-and-changing-passwords-cs/_static/image10.png)

**Şekil 4**: kullanıcı HIS yeni bir parola içeren bir e-posta gönderilir ([tam boyutlu görüntüyü görmek için tıklatın](recovering-and-changing-passwords-cs/_static/image12.png))


### <a name="customizing-the-email"></a>E-posta özelleştirme

PasswordRecovery denetim tarafından gönderilen e-posta varsayılan yerine donuk (bkz: Şekil 4) ' dir. Belirtilen hesapta iletinin gönderildiği `<smtp>` öğenin `from` parola konu özniteliğiyle ve düz metin gövdesi:

Lütfen siteye geri dönün ve aşağıdaki bilgileri kullanarak oturum açın.

Kullanıcı adı: *kullanıcı adı*

Parola: *parola*

Bu ileti PasswordRecovery denetim için bir olay işleyicisi ile programlı olarak özelleştirilebilir [ `SendingMail` olay](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.sendingmail.aspx), bildirimli olarak aracılığıyla veya [ `MailDefinition` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.maildefinition.aspx). Bu iki seçenek araştıralım.

`SendingMail` Doğru e-posta iletisi gönderilir ve program aracılığıyla e-posta iletisi ayarlamak için sunduğumuz son şansınızdır önce olay harekete geçirilir. Bu olay harekete geçirildiğinde, olay işleyicisi türü bir nesne geçirilir [ `MailMessageEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.mailmessageeventargs.aspx), olan `Message` özelliği gönderilmek üzere e-posta için bir başvuru içerir.

İçin bir olay işleyicisi oluşturun `SendingMail` olay ve programlı olarak ekler aşağıdaki kodu ekleyin `webmaster@example.com` bilgi listesi.

[!code-csharp[Main](recovering-and-changing-passwords-cs/samples/sample2.cs)]

E-posta iletisi de bildirime yapılandırılabilir. PasswordRecovery'nın `MailDefinition` özelliği bir nesne türü [ `MailDefinition` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.aspx). `MailDefinition` Sınıfı sunar e-postayla ilgili özellikler dahil olmak üzere çok sayıda `From`, `CC`, `Priority`, `Subject`, `IsBodyHtml`, `BodyFileName`ve diğerleri. Yeni başlayanlar için ayarlamak [ `Subject` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.subject.aspx) birden (parola) varsayılan parola sıfırlama gibi kullanılan daha açıklayıcı bir şey...

Ayrı bir e-posta şablonu dosyası oluşturmak için ihtiyacımız olan e-posta iletisinin gövdesini özelleştirmek için içerikleri gövdesine ait içerir. Yeni bir klasör adlı Web sitesi oluşturarak başlayın `EmailTemplates`. Ardından, bu klasöre adlı yeni bir metin dosyası ekleyin `PasswordRecovery.txt` ve aşağıdaki içeriği ekleyin:

[!code-aspx[Main](recovering-and-changing-passwords-cs/samples/sample3.aspx)]

Yer tutucuları kullanımına dikkat edin `<%UserName%>` ve `<%Password%>`. PasswordRecovery denetimi otomatik olarak bu iki yer tutucuyu kullanıcının kullanıcı adı ve e-postayı göndermeden önce kurtarılan parolasını değiştirir.

Son olarak işaret `MailDefinition`'s [ `BodyFileName` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.bodyfilename.aspx) oluşturduğumuz e-posta şablonu için (`~/EmailTemplates/PasswordRecovery.txt`).

Bunlar yapmadan uygulayamayan değiştirdikten sonra `RecoverPassword.aspx` sayfasında ve kullanıcı adı ve güvenlik yanıtını girin. Aldığınız bir Şekil 5'te şuna benzer bir e-posta gerekir. Unutmayın `webmaster@example.com` bilgi gerekir ve konu ve gövde güncelleştirildi.


[![Konu, gövde ve bilgi listesi güncelleştirildi](recovering-and-changing-passwords-cs/_static/image14.png)](recovering-and-changing-passwords-cs/_static/image13.png)

**Şekil 5**: konu, gövde ve bilgi listesi güncelleştirildi ([tam boyutlu görüntüyü görmek için tıklatın](recovering-and-changing-passwords-cs/_static/image15.png))


Bir HTML biçimli e-posta göndermek için [ `IsBodyHtml` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.isbodyhtml.aspx) True (varsayılan değeri: False) ve güncelleştirme e-posta şablonu HTML eklenecek.

`MailDefinition` Özelliği PasswordRecovery sınıfı için benzersiz değil. Adım 2'de göreceğiz gibi ChangePassword denetimi sunduğu bir `MailDefinition` özelliği. Ayrıca, böyle bir özellik CreateUserWizard denetimi içeren otomatik olarak yeni kullanıcılara bir Hoş Geldiniz e-posta iletisi göndermek için yapılandırabilirsiniz.

> [!NOTE]
> Şu anda hiç bağlantı yok ulaşmak için sol taraftaki gezinti `RecoverPassword.aspx` sayfası. Bu sayfayı ziyaret Filiz başarıyla oturum açma site işleyemezse, yalnızca bu kullanıcı ilgisini. Bu nedenle, güncelleştirme `Login.aspx` bir bağlantı eklemek için sayfa `RecoverPassword.aspx` sayfası.


### <a name="programmatically-resetting-a-users-password"></a>Program aracılığıyla bir kullanıcının parolasını sıfırlama

Çağrıları PasswordRecovery bir kullanıcının parolasını sıfırlama denetlemek `MembershipUser` nesnenin [ `ResetPassword` yöntemi](https://msdn.microsoft.com/library/system.web.security.membershipuser.resetpassword.aspx). Bu yöntemin iki aşırı yüklemesi vardır:

- **[`ResetPassword`](https://msdn.microsoft.com/library/d94bdzz2.aspx)** -bir kullanıcının parolasını sıfırlar. Bu aşırı yüklemesini kullanın `RequiresQuestionAndAnswer` yanlış.
- **[`ResetPassword(securityAnswer)`](https://msdn.microsoft.com/library/d90zte4w.aspx)** -bir kullanıcının parola yalnızca Eğer sağlanan sıfırlar *securityAnswer* doğrudur. Bu aşırı yüklemesini kullanın `RequiresQuestionAndAnswer` true'dur.

Her iki aşırı yüklemeleri, yeni, rastgele oluşturulmuş parolasını döndürün.

Üyelik Framework'teki diğer yöntemlerle gibi `ResetPassword` yapılandırılan sağlayıcı yöntemi temsil eder. `SqlMembershipProvider` Çağırır `aspnet_Membership_ResetPassword` kullanıcının kullanıcı adını, yeni parola ve diğer alanlar arasında sağlanan parola yanıtı geçirme saklı yordamı,. Saklı yordam sağlar: parola yanıtı eşleşir ve ardından kullanıcının parolasını güncelleştirir.

Birkaç alt düzey uygulama notları:

- Çıkış kilitli bir kullanıcı kendi parolanızı sıfırlayamazsınız. Ancak, onaylanmamış bir kullanıcı olabilir. Kilitli kullanıma anlatacak ve durumları daha ayrıntılı olarak onaylanmış <a id="_msoanchor_3"> </a> [ *Unlocking ve onaylama kullanıcı* ](unlocking-and-approving-user-accounts-cs.md) hesapları öğretici.
- Parola yanıtı yanlışsa, kullanıcının başarısız parola yanıtı girişimi sayısı artar. Belirtilen geçersiz güvenlik yanıtı denemelerinin sayısı, belirtilen zaman aralığında meydana gelirse, kullanıcının kilitli.

### <a name="a-word-on-how-the-random-passwords-are-generated"></a>Bir sözcük nasıl rastgele parolalar üzerinde oluşturulur

Şekil 4 ve 5'teki e-posta iletilerini gösterilen işareti, rasgele üretilen parola üyelik sınıfın tarafından oluşturulan [ `GeneratePassword` yöntemi](https://msdn.microsoft.com/library/system.web.security.membership.generatepassword.aspx). Bu yöntemi, iki tamsayı girdi parametrelerinin - kabul *uzunluğu* ve *numberOfNonAlphanumericCharacters* -en az bir dize döndürür *uzunluğu* en uzun olan karakterler az *numberOfNonAlphanumericCharacters* alfasayısal olmayan karakter sayısı. Üyelik sınıfları veya oturum açmayla ilgili Web denetimleri içinde bu yöntem çağrılır, bu iki parametre değerlerini üyelik yapılandırması tarafından belirlenir `MinRequiredPasswordLength` ve `MinRequiredNonalphanumericCharacters` özellikleri, 7 ve 1, sırasıyla ayarlarız.

`GeneratePassword` Yöntemi olduğunu hiçbir sapması hangi rastgele karakterler seçili emin olmak için şifreleme açısından güçlü bir rasgele sayı üreteci kullanır. Ayrıca, `GeneratePassword` olduğu `public`, rastgele dizeleri veya parolaları oluşturmak ihtiyacınız varsa, bunu ASP.NET uygulamanızdan doğrudan kullanabileceğiniz anlamına gelir.

> [!NOTE]
> `SqlMembershipProvider` Sınıfı, rastgele bir parola her zaman oluşturur, dolayısıyla en az 14 karakter `MinRequiredPasswordLength` 14'den küçük olan sonra değeri yoksayılır.


## <a name="step-2-changing-passwords"></a>2. adım: Parola değiştirme

Rastgele oluşturulan parolalarını hatırlamak zordur. Şekil 4'te gösterilen parolayı göz önünde bulundurun: `WWGUZv(f2yM:Bd`. Bellek, işleme deneyin! Deyin needless için bir kullanıcı bu tür bir rastgele oluşturulmuş parolasını gönderildikten sonra Filiz parola daha etkileyici bir şeyle değiştirmek isteyebilirsiniz.

ChangePassword denetimi, bir kullanıcının parolasını değiştirmek bir kullanıcı arabirimi oluşturmak için kullanın. Çok PasswordRecovery denetimi gibi iki görünüm ChangePassword denetimi içerir: parola değiştirme ve başarı. Parola Değiştir görünümü kullanıcı için eski ve yeni parolalarını ister. Eski parola doğru ve minimum uzunluğu ve alfasayısal olmayan karakter gereksinimleri karşılayan yeni bir parola sağlayarak, bağlı ChangePassword denetimi, kullanıcının parolasını güncelleştirir ve başarı görünüm görüntüler.

> [!NOTE]
> ChangePassword denetimi çağırarak kullanıcının parolasını değiştirir `MembershipUser` nesnenin [ `ChangePassword` yöntemi](https://msdn.microsoft.com/library/system.web.security.membershipuser.changepassword.aspx). ChangePassword yöntemi iki kabul `string` giriş parametreleri - *oldPassword* ve *#newpassword*- ve kullanıcı hesabıyla güncelleştirmeleri *#newpassword*, sağlanan varsayılarak *oldPassword* doğrudur.


Açık `ChangePassword.aspx` adlandırma sayfasına bir ChangePassword denetimi ekleyin ve sayfa `ChangePwd`. Bu noktada, Tasarım görünümünde parolasını değiştirme göstermesi gerekir (bkz. Şekil 6) görüntüleyin. Gibi PasswordRecovery denetimiyle, akıllı etiket denetimin aracılığıyla görünüm arasında geçiş yapabilirsiniz. Ayrıca, bu görünümlere görünümleri aracılığıyla çeşitli stil özellikleri ya da bir şablona dönüştürerek özelleştirilebilir.


[![ChangePassword denetimi sayfasına ekleme](recovering-and-changing-passwords-cs/_static/image17.png)](recovering-and-changing-passwords-cs/_static/image16.png)

**Şekil 6**: sayfaya ChangePassword denetimi ekleyin ([tam boyutlu görüntüyü görmek için tıklatın](recovering-and-changing-passwords-cs/_static/image18.png))


ChangePassword denetimi şu anda oturum açmış kullanıcının parolasını güncelleştirebilirsiniz *veya* başka belirtilen kullanıcının parolası. Şekil 6 gösterildiği gibi yalnızca üç TextBox denetimleri varsayılan parola değiştirme görünümü oluşturur: biri eski parolayı, iki yeni parola. Bu varsayılan arabirim, o anda oturum açmış kullanıcının parolasını güncelleştirmek için kullanılır.

Başka bir kullanıcının parolasını güncelleştirmek için ChangePassword denetimi kullanmak için denetimin ayarlamak [ `DisplayUserName` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.changepassword.displayusername.aspx) true. Bunun yapılması dördüncü bir metin kutusu için kullanıcının kullanıcı adını değiştirmek için parola istemi sayfasına ekler.

Ayar `DisplayUserName` için True, oturum açmak zorunda kalmadan kendi parolasını değiştirme kullanıma oturum açmış bir kullanıcı izin vermek istiyorsanız kullanışlıdır. Kişisel oturum açmak için bir kullanıcı kendi parolasını değiştirmek her izin vermeden önce gerektiren ile yanlış bir şey düşünüyorum. Bu nedenle, bırakın `DisplayUserName` (varsayılan) False olarak ayarlayın. Bu kararı ancak biz aslında bu sayfayı ulaşmasını anonim kullanıcılar normalleştirilmesi. Site URL yetkilendirme kuralları, anonim kullanıcıların ziyaret reddetmek için güncelleştirme `ChangePassword.aspx`. URL yetkilendirme kural sözdizimi, bellek yenilemeniz gerektiğinde, kiracıurl <a id="_msoanchor_4"> </a> [ *kullanıcı tabanlı yetkilendirme* ](../membership/user-based-authorization-cs.md) öğretici.

> [!NOTE]
> Görünebilir `DisplayUserName` özelliği, diğer kullanıcıların parolalarını değiştirmek Yöneticiler izin vermek için kullanışlıdır. Ancak, bile `DisplayUserName` doğru eski parolayı bilinen ve girilen True olarak ayarlayın. Adım 3'te kullanıcıların parolalarını değiştirmek Yöneticiler vermeye yönelik teknikleri hakkında konuşur.


Ziyaret `ChangePassword.aspx` sayfasında bir tarayıcıdan ve parolanızı değiştirin. Parola uzunluğu ve alfasayısal olmayan karakter gereksinimleri üyelik yapılandırmasında belirtilen karşılamazsa, yeni bir parola girerseniz, bir hata iletisi görüntülendiğine dikkat edin (bkz. Şekil 7).


[![ChangePassword denetimi sayfasına ekleme](recovering-and-changing-passwords-cs/_static/image20.png)](recovering-and-changing-passwords-cs/_static/image19.png)

**Şekil 7**: sayfaya ChangePassword denetimi ekleyin ([tam boyutlu görüntüyü görmek için tıklatın](recovering-and-changing-passwords-cs/_static/image21.png))


Eski parola doğru ve geçerli yeni bir parola, oturum açmış kullanıcının girdikten sonra parolanın değiştirilmesi ve başarı görünümü görüntülenir.

### <a name="sending-a-confirmation-email"></a>Bir onay e-posta gönderme

Varsayılan olarak, bu denetim parolasını yalnızca güncelleştirilmiş kullanıcıya bir e-posta iletisi göndermez. E-posta göndermek istiyorsanız, denetimin yalnızca yapılandırma `MailDefinition` özelliği. Kullanıcının yeni parolasını içeren bir HTML biçimli e-posta gönderilmesi ChangePassword denetimi yapılandıralım.

Yeni bir dosya oluşturarak başlayın `EmailTemplates` adlı klasöre `ChangePassword.htm`. Aşağıdaki işaretlemeyi ekleyin:

[!code-html[Main](recovering-and-changing-passwords-cs/samples/sample4.html)]

Ardından, ChangePassword denetimin ayarlamak `MailDefinition` özelliğin `BodyFileName`, `IsBodyHtml`, ve `Subject` özelliklerine ~ / EmailTemplates/ChangePassword.htm True ve parolanız değiştirildi! sırasıyla.

Bu değişiklikleri yaptıktan sonra sayfayı yeniden ziyaret ve parolanızı tekrar değiştirin. Bu kez, ChangePassword denetimi dosya çubuğunda kullanıcının e-posta adresine özelleştirilmiş, HTML biçimli e-posta gönderir (bkz. Şekil 8).


[![Kullanıcı, Their parolanın değiştirilmesi bildiren bir e-posta iletisi](recovering-and-changing-passwords-cs/_static/image23.png)](recovering-and-changing-passwords-cs/_static/image22.png)

**Şekil 8**: bir e-posta iletisini bildirir, Their Parola değiştirildi ([tam boyutlu görüntüyü görmek için tıklatın](recovering-and-changing-passwords-cs/_static/image24.png))


## <a name="step-3-allowing-administrators-to-change-users-passwords"></a>3. adım: Yöneticilerin kullanıcıların parolalarını değiştirme izin verme

Kullanıcı hesaplarını destekleyen uygulamalar içinde ortak bir özellik, diğer kullanıcıların parolalarını değiştirmek yönetici kullanıcı için olanağıdır. Bazen bu işlev kullanıcıların kendi parolalarını değiştirmek sistem olmadığı için gereklidir. Böyle bir durumda, yönetici yeni bir parola atanacak Unutulan parolalarını kurtarmak bir kullanıcı için tek yolu olacaktır. Kullanıcılar bu kendilerini yapma özelliği olarak PasswordRecovery ve ChangePassword denetimleriyle ancak yönetici kullanıcıların kendilerini kullanıcıların parolalarını değiştirme ile meşgul değil.

Ancak, yönetici kullanıcılar diğer kullanıcıların parolalarını değiştirmesi mümkün olacağını istemcinizi konusunda ne ısrar? Ne yazık ki bu işlevselliği ekleme biraz iş olabilir. Bir kullanıcının parolasını değiştirmek için eski ve yeni parola için sağlanması gereken `MembershipUser` nesnenin `ChangePassword` yöntemi, ancak bir yönetici olmamalıdır sahip bir kullanıcının parolasını değiştirmek için yerinizi bilmesi.

İlk kullanıcının parola sıfırlama ve aşağıdaki gibi bir kod kullanarak yeni parolayı değiştirmek için bir geçici çözüm şöyledir:

[!code-aspx[Main](recovering-and-changing-passwords-cs/samples/sample5.aspx)]

Bu kod hakkında bilgi alarak başlatır *kullanıcıadı*, parolayı değiştirmek için yönetici isteyen kullanıcı olduğu. Ardından, `ResetPassword` yöntemi çağrılır, hangi atar ve kullanıcı yeni, rastgele bir parola. Bu rastgele oluşturulmuş parolasını yöntem tarafından döndürülen ve değişkeninde depolanan `resetPwd`. Kullanıcının parolasını biliyoruz, biz bunu çağrısıyla değiştirebilirsiniz `ChangePassword`.

Sorun üyelik sistemi yapılandırmasını ayarlayın, bu kod yalnızca çalışıyor olması gibi `RequiresQuestionAndAnswer` yanlış. Varsa `RequiresQuestionAndAnswer` uygulamamız ile olduğu gibi True ise sonra `ResetPassword` yöntemi güvenlik yanıtı geçirilmesi gerekir, aksi takdirde bir özel durum oluşturur.

Üyelik framework bir güvenlik sorusu ve yanıtı isteyecek şekilde yapılandırılması ve henüz istemcinizi konusunda ısrar Yöneticileri kullanıcıların parolalarını değiştirmesi mümkün ise üç seçeneğiniz vardır:

- Ellerinizi havada throw ve istemci bu yapılamaz yalnızca bir şey olduğunu söyleyin.
- Ayarlama `RequiresQuestionAndAnswer` False. Bu, daha az güvenli bir uygulamada sonuçlanır. Alınan kullanıcı başka bir kullanıcının e-posta gelen kutusuna erişim elde ettiğini düşünün. Belki de gizliliği ihlal edilmiş kullanıcı yemeğe Git masa ayrıldı ve kendi iş istasyonu kilitleme olmadı veya belki de bunlar genel terminalden e-postasına erişim ve oturum kapatma kaydetmedi. Alınan kullanıcı her iki durumda da ziyaret edebilirsiniz `RecoverPassword.aspx` sayfasında ve kullanıcının kullanıcı adını girin. Sistem ardından kurtarılan parola güvenlik yanıt sormadan e-posta gönderilir.
- Üyelik framework ve SQL Server veritabanı ile doğrudan iş tarafından oluşturulan Soyutlama Katmanı atlama. Üyelik şeması adlı bir saklı yordam içeren `aspnet_Membership_SetPassword` , bir kullanıcının parolasını ayarlar ve eski parola ve güvenlik yanıtı görevini gerçekleştirmek için gerektirmez.

Bu seçeneklerin hiçbirini özellikle çekici, ancak, bir geliştirici ömrünü nasıl bazen gider.

Oluşturmuştum ve üçüncü bir yaklaşım atlar kod yazma uygulanan `Membership` ve `MembershipUser` sınıfları ve doğrudan karşı çalışır `SecurityTutorials` veritabanı.

> [!NOTE]
> Veritabanı ile doğrudan çalışarak, üyelik framework tarafından sağlanan kapsülleme shattered. Bu karar bize bölümlere `SqlMembershipProvider`, kodumuz az taşınabilir hale getirme. Ayrıca, bu kod, üyelik şeması değişirse, ASP gelecekte beklendiği gibi çalışmayabilir. Bu geçici bir çözüm yaklaşımdır ve çoğu geçici çözümler gibi en iyi bir örneği değil.


Kod bazı paragrafta BITS sahiptir ve çok uzun. Bu nedenle, bu öğreticiyle ayrıntılı bir incelenmesi basmakalıpların istemezsiniz. Daha fazla bilgi edinmek istiyorsanız, ziyaret edin ve Bu öğretici için kod indirme `~/Administration/ManageUsers.aspx` sayfası. Bu sayfa, oluşturduğumuz <a id="_msoanchor_5"> </a> [önceki öğretici](building-an-interface-to-select-one-user-account-from-many-cs.md), her kullanıcı listeler. GridView'ın bir bağlantı eklemek için güncelleştirilmiş `UserInformation.aspx` sayfası, seçilen kullanıcının kullanıcı adı bir sorgu dizesi aracılığıyla geçirme. `UserInformation.aspx` Sayfasında, parolasını değiştirmek için metin kutuları ve seçili kullanıcı hakkında bilgileri görüntüler (bkz. Şekil 9).

Yeni parola girme, ikinci metin kutusuna onaylama ve güncelleştirme kullanıcı düğmeye tıklandığında sonra bir geri gönderme ensues ve `aspnet_Membership_SetPassword` saklı yordam çağrıldığında, kullanıcının parolası güncelleştiriliyor. Ben, kod ile daha aşina işlevselliğini parolasını değiştirildi kullanıcıya bir e-posta göndererek içerecek şekilde genişletmeyi deneyin bu okuyucuların ve bu işlevin ilgilenen teşvik edin.


[![Bir yönetici bir kullanıcının parolasını değiştirebilir](recovering-and-changing-passwords-cs/_static/image26.png)](recovering-and-changing-passwords-cs/_static/image25.png)

**Şekil 9**: bir yönetici bir kullanıcının parolasını değiştirebilir ([tam boyutlu görüntüyü görmek için tıklatın](recovering-and-changing-passwords-cs/_static/image27.png))


> [!NOTE]
> `UserInformation.aspx` Üyelik framework parolalar düz veya Hashed biçiminde depolamak için yapılandırılmışsa, şu anda yalnızca sayfa. Bu işlevsellik eklemek için davet olsa da yeni parolayı şifrelemek için kodu eksik. Önerilir gerekli kodu eklemek gibi bir kaynak koda dönüştürücü kullanmaktır [Reflector](http://www.aisto.com/roeder/dotnet/) ; .NET Framework yöntemleri için kaynak kodunu incelemek için inceleyerek başlayalım `SqlMembershipProvider` sınıfın `ChangePassword` yöntemi. Ben bir parola karmasını oluşturmak için kod yazmak için kullanılan yöntem budur.


## <a name="summary"></a>Özet

ASP.NET, kullanıcıların parolalarını yönetmenize yardımcı olmak için iki denetimi sunar. PasswordRecovery denetimi parolalarını unutmuş kişiler için kullanışlıdır. Üyelik framework'ün yapılandırmasına bağlı olarak, kullanıcı parolalarını mevcut veya yeni, rastgele oluşturulmuş parolasını ya da kaydedilse. Bir kullanıcı parolasını güncelleştirmesi ChangePassword denetimi sağlar.

Oturum açma ve CreateUserWizard denetimler gibi bildirim temelli biçimlendirme bir lick veya satırlık bir kod yazmak zorunda kalmadan, zengin kullanıcı arabirimi PasswordRecovery ve ChangePassword denetimlerini işlemeye. Varsayılan kullanıcı arabirimi, gereksinimlerinizi karşılamaması durumunda bu stil özellikleri çeşitli özelleştirebilirsiniz. Alternatif olarak, Denetim arabirimleri için bir daha kontrol derecesi şablonlarına dönüştürülebilir. Arka planda, üyelik API'si bu denetimleri kullanın çağırma `MembershipUser` nesnenin `ResetPassword` ve `ChangePassword` yöntemleri.

Mutlu programlama!

### <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide ele alınan konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [ChangePassword denetimi hızlı Başlangıçlar](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/changepassword.aspx)
- [PasswordRecovery denetimi hızlı Başlangıçlar](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/passwordrecovery.aspx)
- [ASP.NET ile e-posta gönderme](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)
- [`System.Net.Mail` Sık sorulan sorular](http://www.systemnetmail.com/)

### <a name="about-the-author"></a>Yazar hakkında

Scott Mitchell, birden çok ASP/ASP.NET Books yazar ve poshbeauty.com sitesinin 4GuysFromRolla.com, Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan  *[Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott, konumunda ulaşılabilir [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) veya kendi blog'da aracılığıyla [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici serisinde, birçok yararlı Gözden Geçiren tarafından gözden geçirildi. Bu öğretici için müşteri adayı gözden geçirenler, Michael Emmings ve Suchi Banerjee içerir. Yaklaşan My MSDN makaleleri gözden geçirme ilgileniyor musunuz? Bu durumda, bir satır bana bırak [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](building-an-interface-to-select-one-user-account-from-many-cs.md)
> [İleri](unlocking-and-approving-user-accounts-cs.md)
