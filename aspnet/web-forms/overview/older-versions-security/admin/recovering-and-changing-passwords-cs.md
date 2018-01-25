---
uid: web-forms/overview/older-versions-security/admin/recovering-and-changing-passwords-cs
title: "Kurtarma ve parolaları (C#) değiştirme | Microsoft Docs"
author: rick-anderson
description: "ASP.NET kurtarma ve parolaları değiştirme yardımcı için iki Web denetimleri içerir. PasswordRecovery denetimi kendi kayıp pa kurtarmak bir ziyaretçi sağlar..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2008
ms.topic: article
ms.assetid: 19c4d042-4e34-4b44-9f1d-6bf2253ba366
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/admin/recovering-and-changing-passwords-cs
msc.type: authoredcontent
ms.openlocfilehash: 76c02a3da7dffad25a7bee03efff6b693f261d85
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="recovering-and-changing-passwords-c"></a>Kurtarma ve parolaları (C#) değiştirme
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indirme](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/CS.13.zip) veya [PDF indirin](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/aspnet_tutorial13_ChangingPasswords_cs.pdf)

> ASP.NET kurtarma ve parolaları değiştirme yardımcı için iki Web denetimleri içerir. Kayıp parolasını kurtarmak bir ziyaretçi PasswordRecovery denetimi sağlar. Kullanıcının parolasını güncelleştirmek bu denetim sağlar. Bu öğretici serinin PasswordRecovery anlatıldığı gibi diğer oturum açma ile ilgili Web denetimleri ve parola değiştirme arka planda sıfırlama veya kullanıcıların parolalarını değiştirmek için üyelik framework ile çalışma denetler.


## <a name="introduction"></a>Giriş

Web siteleri arasında için my banka, yardımcı şirket, telefon şirketi, e-posta hesaplarına ve kişiselleştirilmiş web portalı, çoğu kişi gibi anımsaması farklı parolalar düzinelerce sahibim. Bugünlerde ezberleme için çok fazla sayıda kimlik bilgileriyle kendi Parolanızı unutmanız kişiler için seyrek değil. Bu hesap için kullanıcı hesapları sunan Web siteleri bir kullanıcının parolasını kurtarmak bir yol eklemeniz gerekir. Bu işlem genellikle yeni, rastgele bir parola oluşturma ve dosyayı kullanıcının e-posta adresine eposta içerir. Yeni parolalarını aldıktan sonra kullanıcıların çoğunun siteye geri dönün ve rastgele oluşturulmuş bir daha etkileyici bir parolasını değiştirin.

ASP.NET kurtarma ve parolaları değiştirme yardımcı için iki Web denetimleri içerir. Kayıp parolasını kurtarmak bir ziyaretçi PasswordRecovery denetimi sağlar. Kullanıcının parolasını güncelleştirmek bu denetim sağlar. Bu öğretici serinin PasswordRecovery anlatıldığı gibi diğer oturum açma ile ilgili Web denetimleri ve parola değiştirme arka planda sıfırlama veya kullanıcıların parolalarını değiştirmek için üyelik framework ile çalışma denetler.

Bu iki denetimlerini kullanarak bu öğreticide inceleyeceğiz. Programlı olarak değiştirin ve aracılığıyla bir kullanıcının parolasını sıfırlamak nasıl de göreceğiz `MembershipUser` sınıfının `ChangePassword` ve `ResetPassword` yöntemleri.

## <a name="step-1-helping-users-recover-lost-passwords"></a>1. adım: Parolaları kayıp yardımcı kullanıcılar Kurtar

Kullanıcı hesaplarını destekleyen tüm Web siteleri kullanıcılara parolalarını kurtarmak için bazı mekanizma sağlamanız gerekir. İyi haber bu tür işlevselliği ASP.NET uygulama PasswordRecovery Web denetimi sayesinde çok kolay olmasıdır. Kullanıcıdan kullanıcı adı için bir arabirim PasswordRecovery denetimini işler ve gerekirse, kendi güvenlik sorusuna verilen yanıt. Ardından kullanıcı parolalarını e.

> [!NOTE]
> E-posta iletilerini düz metin kablo üzerinden iletilir çünkü güvenlik riskleri e-posta yoluyla bir kullanıcının parolasını göndermeyle ilgili vardır.


PasswordRecovery denetimi üç görünüm içerir:

- **Kullanıcı adı** -ziyaretçi için kullanıcı adı ister. Bu ilk görünümdür.
- **Soru**-kullanıcının kullanıcı adı ve güvenlik sorusunu kullanıcının kendi güvenlik sorusunun yanıtını girmek metin kutusu birlikte metin olarak görüntüler.
- **Başarı**-parolasını postayla kullanıcı bildiren bir ileti görüntüler.

Görünümler görüntülenir ve aşağıdaki üyelik yapılandırma ayarlarına bağlıdır PasswordRecovery denetimi tarafından gerçekleştirilen eylemler bağlıdır:

- `RequiresQuestionAndAnswer`
- `EnablePasswordRetrieval`
- `EnablePasswordReset`

Üyelik framework'ün `RequiresQuestionAndAnswer` ayarı, kullanıcıların bir güvenlik sorusu ve yanıtı bir hesabı için kaydolurken belirtmelisiniz olup olmadığını belirtir. Biz anlatıldığı gibi <a id="_msoanchor_1"> </a> [ *kullanıcı hesapları oluşturma* ](../membership/creating-user-accounts-cs.md) Öğreticisi, eğer `RequiresQuestionAndAnswer` TextBox CreateUserWizard'ın arabirimi içerir (varsayılan) True olduğundan Yeni kullanıcının güvenlik sorusu ve yanıtı için denetimleri; varsa `RequiresQuestionAndAnswer` false, bu tür hiçbir bilgi toplanmaz. Benzer şekilde, varsa `RequiresQuestionAndAnswer` True, ardından kullanıcının kullanıcı adlarını girdikten sonra sorunun görüntüleyin PasswordRecovery denetimi görüntüler; yalnızca kullanıcı doğru güvenlik yanıtı girerse parola kurtarılır. Varsa `RequiresQuestionAndAnswer` PasswordRecovery denetimi düz kullanıcıadı görünümünden başarı görünümüne taşır. ancak, False ' tır.

Kullanıcı kendi kullanıcı adı - ya da kendi kullanıcı adı ve güvenlik yanıt, sağladığınız sonra `RequiresQuestionAndAnswer` true'dur - PasswordRecovery parolasını kullanıcı e-postalar. Varsa `EnablePasswordRetrieval` seçeneği True olarak ayarlayın, sonra kullanıcı, geçerli parolaları e-posta. False olarak ayarlarsanız ve `EnablePasswordReset` PasswordRecovery denetimi kullanıcı için yeni, rastgele bir parola oluşturur ve bu yeni parola onlara e-postalar True değerine ayarlanır. Her iki `EnablePasswordRetrieval` ve `EnablePasswordReset` False olduğunda, PasswordRecovery denetimi bir özel durum oluşturur.

> [!NOTE]
> Sözcüğünün `SqlMembershipProvider` kullanıcıların parolalarını üç biçimlerinden birinde depolar: temizleyin, Hashed (varsayılan) veya şifreli. Kullanılan depolama mekanizmasını üyelik yapılandırma ayarlarına bağlıdır; Tanıtım uygulama Hashed parola biçimini kullanır. Hashed parolası biçimi kullanılırken `EnablePasswordRetrieval` seçeneği sistem gerçek kullanıcının parolasını veritabanında depolanan karma sürümünden belirleyemediğinden False olarak ayarlanmalıdır.


Şekil 1 nasıl PasswordRecovery'nın arabirimi ve davranış Etkileyenler tarafından üyeliği yapılandırması gösterilmektedir.


[![RequiresQuestionAndAnswer, EnablePasswordRetrieval ve EnablePasswordReset PasswordRecovery denetimin görünümünü ve davranışını etkilemek](recovering-and-changing-passwords-cs/_static/image2.png)](recovering-and-changing-passwords-cs/_static/image1.png)

**Şekil 1**: `RequiresQuestionAndAnswer`, `EnablePasswordRetrieval`, ve `EnablePasswordReset` PasswordRecovery denetimin görünümünü ve davranışını etkilemek ([tam boyutlu görüntüyü görüntülemek için tıklatın](recovering-and-changing-passwords-cs/_static/image3.png))


> [!NOTE]
> İçinde <a id="_msoanchor_2"> </a> [ *SQL Server üyelik şema oluşturma* ](../membership/creating-the-membership-schema-in-sql-server-cs.md) biz yapılandırılmış üyelik sağlayıcısının ayarlayarak öğretici `RequiresQuestionAndAnswer` true olarak `EnablePasswordRetrieval` için False ve `EnablePasswordReset` true.


### <a name="using-the-passwordrecovery-control"></a>PasswordRecovery denetimi kullanma

Bir ASP.NET sayfası PasswordRecovery denetimi kullanarak bakalım. Açık `RecoverPassword.aspx` ve sürükleyin ve araç tasarımcıya PasswordRecovery denetimi bırakma; ayarlamak kendi `ID` için `RecoverPwd`. Oturum açma ve CreateUserWizard Web denetimleri gibi etiketleri, metin kutuları, düğmeler ve doğrulama denetimleri içeren zengin bir bileşik arabirimini PasswordRecovery denetimin görünümleri oluşturmak. Denetimin stil özellikleri aracılığıyla ya da şablonlara görünümleri dönüştürerek görünümleri görünümünü özelleştirebilirsiniz. I bunu bir alıştırma olarak ilgi Okuyucu için bırakın.

Bir kullanıcı bu sayfayı ziyaret ettiğinde kendisinin kendi kullanıcı adı girin ve Gönder düğmesine tıklayın. Biz ayarladığınızdan `RequiresQuestionAndAnswer` özelliğini True bizim üyelik yapılandırma ayarlarında PasswordRecovery kontrol edecek sonra soru görünüm. Kullanıcı kendi doğru güvenlik yanıtı girip gönderme tıklattığında sonra PasswordRecovery denetimi rastgele oluşturulan bir kullanıcının parolasını güncelleştirin ve bu parolayı dosya e-posta adresine e-posta. Tüm olası bize tek satırlık bir kod yazmak zorunda!

Bu sayfayı test etmeden önce bir son parçası eğilimindedir yapılandırmasının yoktur: posta teslim ayarları belirtmek ihtiyacımız `Web.config`. PasswordRecovery denetimi, e-posta göndermek için bu ayarları kullanır.

Posta teslim yapılandırması aracılığıyla belirtilen [ `<system.net>` öğesi](https://msdn.microsoft.com/library/6484zdc1.aspx)'s [ `<mailSettings>` öğesi](https://msdn.microsoft.com/library/w355a94k.aspx). Kullanım [ `<smtp>` öğesi](https://msdn.microsoft.com/library/ms164240.aspx) teslimat yöntemini ve varsayılan adresinden belirtmek için. Aşağıdaki biçimlendirmede adlı bir ağ SMTP sunucusunu kullanmak için posta ayarlarını yapılandırır `smtp.example.com` bağlantı noktası 25 ve kullanıcı adı/parola kimlik bilgileri kullanıcı adı ve parola.

> [!NOTE]
> `<system.net>`bir alt öğe kökün `<configuration>` öğesi ve bir eşdüzeyi `<system.web>`. Bu nedenle, değil put `<system.net>` öğesi içinde `<system.web>` öğesi; bunun yerine, aynı düzeyde yerleştirin.


[!code-xml[Main](recovering-and-changing-passwords-cs/samples/sample1.xml)]

Ağ üzerinde bir SMTP sunucusu kullanarak ek olarak gönderilecek e-posta iletilerini borç burada bir toplama dizini alternatif olarak belirtebilirsiniz.

SMTP ayarlarını yapılandırdıktan sonra ziyaret `RecoverPassword.aspx` bir tarayıcı aracılığıyla sayfası. İlk kullanıcı deposunda mevcut olmayan bir kullanıcı adı girmeyi deneyin. Şekil 2'de görüldüğü gibi PasswordRecovery denetimi kullanıcı bilgilerini erişilemiyor belirten bir ileti görüntüler. İleti metnini denetimin özelleştirilebilir [ `UserNameFailureText` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.usernamefailuretext.aspx).


[![Geçersiz kullanıcı adı girildiğinde bir hata iletisi görüntülenir](recovering-and-changing-passwords-cs/_static/image5.png)](recovering-and-changing-passwords-cs/_static/image4.png)

**Şekil 2**: geçersiz kullanıcı adı girildiğinde bir hata iletisi görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](recovering-and-changing-passwords-cs/_static/image6.png))


Şimdi bir kullanıcı adı girin. Sistemdeki erişebilirsiniz ve, güvenlik yanıt e-posta adresi olan bir hesabın kullanıcı adı bilmeniz kullanın. Kullanıcı adı girerek ve Gönder'i tıklatmak sonra PasswordRecovery denetimi soru görünümünü görüntüler. Olarak kullanıcıadı görünümüyle girerseniz, yanlış bir yanıt bir hata iletisi (bkz: Şekil 3) PasswordRecovery denetimi görüntüler. Kullanım [ `QuestionFailureText` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.questionfailuretext.aspx) bu hata iletisini özelleştirmek için.


[![Kullanıcı geçersiz güvenlik yanıt girerse bir hata iletisi görüntülenir](recovering-and-changing-passwords-cs/_static/image8.png)](recovering-and-changing-passwords-cs/_static/image7.png)

**Şekil 3**: geçersiz bir güvenlik yanıtı kullanıcının girdiği durumlarda bir hata iletisi görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](recovering-and-changing-passwords-cs/_static/image9.png))


Son olarak, doğru güvenlik yanıtı girin ve Gönder'i tıklatın. Arka planda PasswordRecovery denetimi rastgele bir parola oluşturur, kullanıcı hesabına atar, yeni parolalarını kullanıcı bildiren bir e-posta gönderir (bkz. Şekil 4) ve Başarı görünümünün görüntüler.


[![Kullanıcı bir e-posta HIS yeni parolayla gönderilir](recovering-and-changing-passwords-cs/_static/image11.png)](recovering-and-changing-passwords-cs/_static/image10.png)

**Şekil 4**: kullanıcı bir e-posta HIS yeni parolayla gönderilen ([tam boyutlu görüntüyü görüntülemek için tıklatın](recovering-and-changing-passwords-cs/_static/image12.png))


### <a name="customizing-the-email"></a>E-posta özelleştirme

PasswordRecovery denetimi tarafından gönderilen e-posta varsayılan yerine donuk (bkz: Şekil 4) ' dir. Belirtilen hesap iletinin gönderildiği `<smtp>` öğenin `from` parola konu özniteliğiyle ve düz metin gövdesi:

Lütfen siteye geri dönün ve aşağıdaki bilgileri kullanarak oturum açın.

Kullanıcı adı: *kullanıcı adı*

Parola: *parola*

Bu ileti PasswordRecovery denetim için bir olay işleyicisi aracılığıyla programlı olarak özelleştirilebilir [ `SendingMail` olay](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.sendingmail.aspx), bildirimli olarak aracılığıyla veya [ `MailDefinition` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.maildefinition.aspx). Bu seçeneklerin ikisi de inceleyelim.

`SendingMail` Sağ e-posta iletisi gönderilir ve program aracılığıyla e-posta iletisi ayarlamak için bizim son şansınızdır önce olay tetiklenir. Bu olay oluştuğunda, olay işleyicisi türünde bir nesneye iletilir [ `MailMessageEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.mailmessageeventargs.aspx), `Message` özelliği gönderilmek üzere e-posta için bir başvuru içeriyor.

İçin bir olay işleyicisi oluşturun `SendingMail` olay ve program aracılığıyla ekler aşağıdaki kodu ekleyin `webmaster@example.com` bilgi listesi.

[!code-csharp[Main](recovering-and-changing-passwords-cs/samples/sample2.cs)]

E-posta iletisi bildirime de yapılandırılabilir. PasswordRecovery's `MailDefinition` özelliği olan bir nesne türü [ `MailDefinition` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.aspx). `MailDefinition` Sınıfı sunar dahil olmak üzere, e-posta ile ilgili özelliklerin bir konak `From`, `CC`, `Priority`, `Subject`, `IsBodyHtml`, `BodyFileName`ve diğerleri. Yeni başlayanlar ayarlamak [ `Subject` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.subject.aspx) birden parola sıfırlama gibi (parola), varsayılan olarak kullanılan daha açıklayıcı bir şey...

Ayrı bir e-posta şablonu dosyası oluşturmak için ihtiyacımız e-posta iletisinin gövdesi özelleştirmek için gövdesine ait içeriği içerir. Başlangıç adlı Web sitesini yeni bir klasör oluşturarak `EmailTemplates`. Ardından, yeni bir metin dosyası adlı bu klasöre eklemek `PasswordRecovery.txt` ve aşağıdaki içeriği ekleyin:

[!code-aspx[Main](recovering-and-changing-passwords-cs/samples/sample3.aspx)]

Yer tutucuları kullanımına dikkat edin `<%UserName%>` ve `<%Password%>`. PasswordRecovery denetimi iki bu yer tutucuları kullanıcının kullanıcı adı ve e-postayı göndermeden önce kurtarılan parola ile otomatik olarak değiştirir.

Son olarak, noktası `MailDefinition`'s [ `BodyFileName` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.bodyfilename.aspx) yeni oluşturduğumuz e-posta şablonu için (`~/EmailTemplates/PasswordRecovery.txt`).

Tekrar ziyaret etmiş değişiklikler bu yaptıktan sonra `RecoverPassword.aspx` sayfa ve kullanıcı adı ve güvenlik yanıtınız girin. Aldığınız bir Şekil 5'te benzer bir e-posta gerekir. Unutmayın `webmaster@example.com` CC olur ve konu ve gövde güncelleştirilen olmuştur.


[![Konu, gövde ve bilgi listesi güncelleştirildi](recovering-and-changing-passwords-cs/_static/image14.png)](recovering-and-changing-passwords-cs/_static/image13.png)

**Şekil 5**: konu, gövde ve bilgi listesi güncelleştirildi ([tam boyutlu görüntüyü görüntülemek için tıklatın](recovering-and-changing-passwords-cs/_static/image15.png))


Bir HTML biçimli e-posta göndermek için [ `IsBodyHtml` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.isbodyhtml.aspx) True (varsayılan değeri: False) ve güncelleştirme e-posta şablonu HTML eklenecek.

`MailDefinition` Özelliği PasswordRecovery sınıfı için benzersiz değil. Adım 2'de göreceğiz gibi bu denetim sunduğu bir `MailDefinition` özelliği. Ayrıca, böyle bir özellik CreateUserWizard denetimi içerir, otomatik olarak yeni kullanıcılara Hoş Geldiniz e-posta iletisi göndermek için yapılandırabilirsiniz.

> [!NOTE]
> Şu anda hiç bağlantı yok ulaşmak için sol gezinti içinde `RecoverPassword.aspx` sayfası. Bir kullanıcı yalnızca aynen başarıyla sitesinde oturum açmak için işleyemezse, bu sayfayı ziyaret ilgileniyor. Bu nedenle, güncelleştirme `Login.aspx` bir bağlantı eklemek için sayfa `RecoverPassword.aspx` sayfası.


### <a name="programmatically-resetting-a-users-password"></a>Program aracılığıyla bir kullanıcının parolasını sıfırlama

Çağrıları PasswordRecovery bir kullanıcının parolasını sıfırlama denetlemek `MembershipUser` nesnenin [ `ResetPassword` yöntemi](https://msdn.microsoft.com/library/system.web.security.membershipuser.resetpassword.aspx). Bu yöntem iki aşırı yüklemeye sahip:

- **[`ResetPassword`](https://msdn.microsoft.com/library/d94bdzz2.aspx)**-bir kullanıcının parolasını sıfırlar. Bu aşırı kullanırsanız `RequiresQuestionAndAnswer` yanlış.
- **[`ResetPassword(securityAnswer)`](https://msdn.microsoft.com/library/d90zte4w.aspx)**-bir kullanıcının parola eksikse sağlanan sıfırlar *securityAnswer* doğrudur. Bu aşırı kullanırsanız `RequiresQuestionAndAnswer` true'dur.

Her iki aşırı yeni, rastgele oluşturulan parola döndür.

Üyelik Framework'teki diğer yöntemleriyle gibi `ResetPassword` yapılandırılmış sağlayıcı yöntemi temsil eder. `SqlMembershipProvider` Çağırır `aspnet_Membership_ResetPassword` kullanıcının kullanıcı adını, yeni parola ve diğer alanlar arasında sağlanan parola yanıtı geçirme saklı yordamı,. Saklı yordam parola yanıtı eşleştiğinden ve kullanıcının parolasını güncelleştirmeler sağlar.

Alt düzey uygulama notları birkaç:

- Out kilitli bir kullanıcı kendi parolanızı sıfırlayamazsınız. Ancak, onaylanmamış bir kullanıcı olabilir. Kilitli çıkışı ele alınacaktır ve daha ayrıntılı olarak durumları onaylanan <a id="_msoanchor_3"> </a> [ *Unlocking ve onaylama kullanıcı* ](unlocking-and-approving-user-accounts-cs.md) hesapları öğretici.
- Parola yanıtı doğru değilse, kullanıcının başarısız parola yanıtı girişimi sayısı artar. Geçersiz güvenlik yanıtı denemelerinin belirli bir süre içinde belirtilen zaman penceresi meydana gelirse, kullanıcının kilitli.

### <a name="a-word-on-how-the-random-passwords-are-generated"></a>Bir sözcük nasıl rastgele parolaları üzerinde oluşturulur

Şekil 4 ve 5'teki e-posta iletilerini gösterilen rastgele oluşturulan parolalarını üyelik sınıfının tarafından oluşturulan [ `GeneratePassword` yöntemi](https://msdn.microsoft.com/library/system.web.security.membership.generatepassword.aspx). Bu yöntem iki tamsayı giriş parametre - kabul *uzunluğu* ve *numberOfNonAlphanumericCharacters* - ve en az bir dize döndürür *uzunluğu* en uzun ile karakterleri az *numberOfNonAlphanumericCharacters* alfasayısal olmayan karakter sayısı. Üyelik sınıfları veya oturum açma ile ilgili Web denetimleri içinde bu yöntem çağrılır, bu iki parametre için değerler üyelik yapılandırmasının tarafından belirlenir `MinRequiredPasswordLength` ve `MinRequiredNonalphanumericCharacters` 7 ve 1, sırasıyla ayarlarız özellikleri.

`GeneratePassword` Yöntemi olduğunu hiçbir sapması hangi rastgele karakterler seçili olduğundan emin olmak için şifreleme açısından güçlü bir rastgele sayı üreticisinin kullanır. Ayrıca, `GeneratePassword` olan `public`, rastgele dizeleri veya parolaları oluşturmak ihtiyacınız varsa, bunu doğrudan ASP.NET uygulamanızdan kullanabileceğiniz anlamına gelir.

> [!NOTE]
> `SqlMembershipProvider` Sınıfı oluşturur rastgele bir parola her zaman en az 14 karakter uzunluğunda, dolayısıyla `MinRequiredPasswordLength` değerinden 14 yapılır ve ardından değeri yok sayılır.


## <a name="step-2-changing-passwords"></a>2. adım: Parolaları değiştirme

Rastgele oluşturulan parolalarını unutmayın zordur. Şekil 4'te gösterilen parola göz önünde bulundurun: `WWGUZv(f2yM:Bd`. Bellek yürüten deneyin! Deyin needless için bir kullanıcı bu tür bir rastgele oluşturulan parola gönderildikten sonra aynen daha etkileyici bir şey için parolayı değiştirmek istersiniz.

Bir kullanıcının kendi parolasını değiştirmek bir arabirim oluşturmak için bu denetim kullanın. Çok PasswordRecovery denetimi gibi bu denetim iki görünümde oluşur: parola değiştirme ve başarı. Parola değiştirme görünüm eski ve yeni parolalarını kullanıcıya sorar. Doğru eski parolayı ve minimum uzunluğu ve alfasayısal olmayan karakter gereksinimleri karşılayan yeni bir parola sağlayarak sonra bu denetim kullanıcının parolasını güncelleştirir ve başarı görünüm görüntüler.

> [!NOTE]
> Bu denetim çağırarak kullanıcının parolasını değiştirir `MembershipUser` nesnenin [ `ChangePassword` yöntemi](https://msdn.microsoft.com/library/system.web.security.membershipuser.changepassword.aspx). Parola değiştirme yöntemi iki kabul `string` giriş parametreleri - *EskiParola* ve *#newpassword*- ve kullanıcı hesabıyla güncelleştirmeleri *#newpassword*, sağlanan varsayılarak *EskiParola* doğrudur.


Açık `ChangePassword.aspx` sayfasında ve bu denetim adlandırma sayfasına ekleme `ChangePwd`. Bu noktada, Tasarım görünümüne Parolayı Değiştir göstermesi gerekir (bkz. Şekil 6) görüntüleyin. Gibi PasswordRecovery denetimi ile denetimin akıllı etiket aracılığıyla görünümleri arasında geçiş yapabilirsiniz. Ayrıca, bu görünümlere görünümlerini getirilebilir stil özellikleri kullanılarak veya bir şablona dönüştürerek özelleştirilebilir.


[![Bu denetim için bir sayfa ekleyin](recovering-and-changing-passwords-cs/_static/image17.png)](recovering-and-changing-passwords-cs/_static/image16.png)

**Şekil 6**: sayfasına bu denetim ekleme ([tam boyutlu görüntüyü görüntülemek için tıklatın](recovering-and-changing-passwords-cs/_static/image18.png))


Bu denetim şu anda oturum açmış kullanıcının parolasını güncelleştirebilirsiniz *veya* başka belirtilen kullanıcının parolası. Şekil 6 gösterildiği gibi yalnızca üç TextBox denetimi varsayılan parola değiştirme görünümü oluşturur: biri eski parola ve iki yeni parolası. Bu varsayılan arabirimi, şu anda oturum açmış kullanıcının parolasını güncelleştirmek için kullanılır.

Başka bir kullanıcının parolasını güncelleştirmek için bu denetim kullanmak için denetimin ayarlamak [ `DisplayUserName` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.changepassword.displayusername.aspx) true. Bunun yapılması dördüncü bir metin kutusu parolasını değiştirmek için kullanıcının kullanıcı adı için isteyen sayfasına ekler.

Ayarı `DisplayUserName` için true'dur oturum açmak zorunda kalmadan kendi parolasını değiştirme çıkışı oturum açmış bir kullanıcı izin vermek istiyorsanız yararlıdır. Kişisel olarak, her kullanıcının parolasını değiştirmek izin vermeden önce oturum açmak için bir kullanıcının gerektiren ile yanlış bir şey yok düşünüyorum. Bu nedenle, bırakın `DisplayUserName` (varsayılan) False olarak ayarlayın. Bu kararı ancak biz aslında bu sayfayı ulaşmasını anonim kullanıcılar engelleme. Anonim kullanıcıların ziyaret engellemek amacıyla sitenin URL yetkilendirme kuralları güncelleştirme `ChangePassword.aspx`. URL yetkilendirme kural sözdizimi, bellek yenilemeniz gerekir geri oluştuysa, <a id="_msoanchor_4"> </a> [ *kullanıcı tabanlı bir yetkilendirme* ](../membership/user-based-authorization-cs.md) Öğreticisi.

> [!NOTE]
> Görünebilir `DisplayUserName` özelliği diğer kullanıcıların parolalarını değiştirmek Yöneticiler izin vermek için yararlıdır. Ancak, bile `DisplayUserName` doğru eski parolayı bilinen ve girilen True olarak ayarlayın. Adım 3'te kullanıcıların parolalarını değiştirmek Yöneticiler izin vermekle teknikleri hakkında konuşur.


Ziyaret `ChangePassword.aspx` sayfasında bir tarayıcıdan ve parolanızı değiştirin. Parola uzunluğu ve alfasayısal olmayan karakter gereksinimleri üyelik yapılandırmasında belirtilen uymadığı yeni bir parola girerseniz, bir hata iletisi görüntüleniyor unutmayın (bkz. Şekil 7).


[![Bu denetim için bir sayfa ekleyin](recovering-and-changing-passwords-cs/_static/image20.png)](recovering-and-changing-passwords-cs/_static/image19.png)

**Şekil 7**: sayfasına bu denetim ekleme ([tam boyutlu görüntüyü görüntülemek için tıklatın](recovering-and-changing-passwords-cs/_static/image21.png))


Doğru eski parolayı ve geçerli yeni bir parola, oturum açan kullanıcının girdikten sonra Parola değiştirildi ve başarı görünümü görüntülenir.

### <a name="sending-a-confirmation-email"></a>Bir onay e-posta gönderme

Varsayılan olarak, bu denetim parolasını yalnızca güncelleştirildi kullanıcıya bir e-posta iletisi göndermez. Bir e-posta göndermek istiyorsanız, denetimin yalnızca yapılandırma `MailDefinition` özelliği. Kullanıcı yeni parolasını içeren bir HTML biçimli e-posta gönderilir, böylece bu denetim yapılandıralım.

Başlangıç yeni bir dosyada oluşturarak `EmailTemplates` adlı klasörü `ChangePassword.htm`. Aşağıdaki biçimlendirmede ekleyin:

[!code-html[Main](recovering-and-changing-passwords-cs/samples/sample4.html)]

Ardından, parola değiştirme denetimin ayarlayın `MailDefinition` özelliğin `BodyFileName`, `IsBodyHtml`, ve `Subject` özelliklerine ~ / EmailTemplates/ChangePassword.htm, True ve parolanızı değişti! sırasıyla.

Bu değişiklikleri yaptıktan sonra sayfa yeniden ziyaret ve parolanızı yeniden değiştirin. Bu süre, bu denetim dosyası kullanıcının e-posta adresine özelleştirilmiş, HTML biçimli e-posta gönderir (bkz. Şekil 8).


[![Kullanıcı, Their parolanın değiştirilmesi bildiren bir e-posta iletisi](recovering-and-changing-passwords-cs/_static/image23.png)](recovering-and-changing-passwords-cs/_static/image22.png)

**Şekil 8**: bir e-posta iletisi bildirir kullanıcı olduğunu Their parola değişti ([tam boyutlu görüntüyü görüntülemek için tıklatın](recovering-and-changing-passwords-cs/_static/image24.png))


## <a name="step-3-allowing-administrators-to-change-users-passwords"></a>3. adım: Yöneticilerin kullanıcıların parolalarını değiştirme izin verme

Bir ortak kullanıcı hesaplarını destekleyen uygulamalar diğer kullanıcıların parolalarını değiştirmek bir yönetici kullanıcı yeteneği özelliğidir. Bazen sistem kullanıcıların kendi parolalarını değiştirmek özelliği eksik olduğundan bu işlevselliği gereklidir. Böyle bir durumda, tek yolu bir kullanıcının kendi Unutulan parolayı kurtarmak yeni bir parola atamak için yönetici olacaktır. Kullanıcılar bu kendilerini yapma yeteneği gibi PasswordRecovery ve parola değiştirme denetimleriyle ancak, yönetici kullanıcıların kendilerini kullanıcıların parolalarını değiştirme ile meşgul değil.

Ancak ne istemciniz konusunda ısrar yönetici kullanıcılar diğer kullanıcıların parolalarını değiştirin yapabiliyor olmanız gerekir? Ne yazık ki, bu işlevsellik ekleme biraz iş olabilir. Bir kullanıcının parolasını değiştirmek için eski ve yeni parola için sağlanması gereken `MembershipUser` nesnesinin `ChangePassword` yöntemi ancak yönetici döndürmemelidir olan bir kullanıcının parolasını değiştirmek için bilmesi.

İlk kullanıcının parolasını sıfırlamak ve aşağıdaki gibi kod kullanarak yeni parolayı değiştirmek için bir geçici çözüm şöyledir:

[!code-aspx[Main](recovering-and-changing-passwords-cs/samples/sample5.aspx)]

Bu kod, hakkında bilgi alma başlatır *kullanıcıadı*, parolayı değiştirmek için yönetici isteyen kullanıcı olduğu. Ardından, `ResetPassword` yöntemi çağrıldığında, hangi atar ve kullanıcı yeni, rastgele bir parola. Bu rastgele oluşturulan parola yöntemi tarafından döndürülen ve değişkeninde depolanan `resetPwd`. Kullanıcının parolasını biliyoruz, biz çağrısıyla değiştirebilirsiniz `ChangePassword`.

Sorun üyeliği sistem yapılandırması ayarlarsanız bu kod yalnızca çalışmasıdır şekilde `RequiresQuestionAndAnswer` yanlış. Varsa `RequiresQuestionAndAnswer` uygulamamız ile olduğu gibi doğru ise, sonra `ResetPassword` yöntemi ilişkin güvenlik sorusuyla geçirilmesi gerekir, aksi takdirde bir özel durum oluşturur.

Üyelik framework Güvenlik sorusu ve yanıtı gerektirecek şekilde yapılandırılmışsa ve henüz istemciniz, yöneticilerin kullanıcıların parolalarını değiştirmesi mümkün konusunda ısrar, üç seçeneğiniz vardır:

- Elinizde throw hava ve istemci bu yapılamaz tek şey olduğunu söyleyin.
- Ayarlama `RequiresQuestionAndAnswer` False. Bu daha az güvenli bir uygulamada sonuçlanır. Alınan bir kullanıcı başka bir kullanıcının e-posta gelen kutusuna erişim elde edilen düşünün. Belki de gizliliği ihlal edilmiş kullanıcı yemeği için Git masa ayrıldı ve iş istasyonunda kilitlemek alamadık veya belki de e-posta ortak terminal durumundan erişilen ve oturumu alamadık. Alınan kullanıcı her iki durumda da ziyaret edebilirsiniz `RecoverPassword.aspx` sayfasında ve kullanıcının kullanıcı adını girin. Sistem ardından kurtarılan parola ilişkin güvenlik sorusuyla sormadan e-posta gönderir.
- Üyelik framework ve SQL Server veritabanı ile doğrudan iş tarafından oluşturulan Soyutlama Katmanı atlama. Adlı bir saklı yordam üyelik şeması içerir `aspnet_Membership_SetPassword` , bir kullanıcının parolasını ayarlar ve güvenlik yanıtı veya eski parola, görevi gerçekleştirmek için gerektirmez.

Bu seçeneklerin hiçbirini özellikle çekici, ancak nasıl bir geliştirici ömrünü bazen gider.

Şimdi oluştu ve üçüncü bir yaklaşım atlar kod yazma uygulanan `Membership` ve `MembershipUser` sınıfları ve doğrudan karşı çalışır `SecurityTutorials` veritabanı.

> [!NOTE]
> Veritabanıyla doğrudan çalışarak üyelik framework tarafından sağlanan kapsülleme shattered. Bu karara bize bağlar, `SqlMembershipProvider`, kodumuza taşınabilir daha az yapma. Ayrıca, bu kod üyelik şema değişirse ASP.NET sürümlerini gelecekte beklendiği gibi çalışmayabilir. Bu yaklaşımın bir geçici bir çözüm değildir ve çoğu geçici çözümler gibi en iyi yöntemler örneği değil.


Kod bazı paragrafta BITS içeriyor ve oldukça uzun. Bu nedenle, kapsamlı bir incelendiğinde bu öğreticiyle dağıtmayı istemiyorsanız. Daha fazla bilgi ilgileniyorsanız, Bu öğretici ve ziyaret için kodu indirme `~/Administration/ManageUsers.aspx` sayfası. İçinde oluşturduğumuz bu sayfayı <a id="_msoanchor_5"> </a> [önceki öğretici](building-an-interface-to-select-one-user-account-from-many-cs.md), her kullanıcı listeler. Bir bağlantı eklemek için GridView güncelleştirilmiş `UserInformation.aspx` sayfası, seçilen kullanıcının kullanıcı adı querystring aracılığıyla geçirme. `UserInformation.aspx` Sayfasında, parolasını değiştirmek için seçilen kullanıcı ve metin kutuları hakkında bilgileri görüntüler (bkz. Şekil 9).

Yeni parola girme, ikinci metin kutusuna onaylama ve güncelleştirme kullanıcı düğmesini tıklatarak sonra geri gönderimin ensues ve `aspnet_Membership_SetPassword` saklı yordam çağrıldığında, kullanıcının parolasını güncelleştiriliyor. I koduyla daha aşina işlevselliği parolasını değiştirildi kullanıcıya bir e-posta göndererek içerecek şekilde genişletmeyi deneyin bu okuyucuların ve bu işlevselliği ilgilenen teşvik edin.


[![Bir yönetici bir kullanıcının parolasını değiştirebilir](recovering-and-changing-passwords-cs/_static/image26.png)](recovering-and-changing-passwords-cs/_static/image25.png)

**Şekil 9**: bir yönetici bir kullanıcının parolasını değiştirebilir ([tam boyutlu görüntüyü görüntülemek için tıklatın](recovering-and-changing-passwords-cs/_static/image27.png))


> [!NOTE]
> `UserInformation.aspx` Üyelik framework parolalar düz veya Hashed biçiminde depolamak için yapılandırılmışsa, şu anda yalnızca sayfa. Bu işlevsellik eklemek için davet karşın yeni parolayı şifrelemek için kodu eksik. Öneririz gerekli kod ekleme yolu gibi decompiler kullanmaktır [Reflector](http://www.aisto.com/roeder/dotnet/) .NET Framework; yöntemleri için kaynak kodunu incelemek için inceleyerek Başlat `SqlMembershipProvider` sınıfının `ChangePassword` yöntemi. Bu parola karmasını oluşturmak için kod yazma kullanılacak tekniğidir.


## <a name="summary"></a>Özet

ASP.NET kullanıcıların parolalarını yönetmenize yardımcı olmak için iki denetim sunar. PasswordRecovery denetimi parolalarını unutmuş olanlar için yararlıdır. Üyelik framework'ün yapılandırmasına bağlı olarak, kullanıcı ya da varolan parolasını ya da yeni, rastgele oluşturulan parola e-posta ile. Bu denetim bir kullanıcının parolasını güncelleştirmesi olanak tanır.

Oturum açma ve CreateUserWizard denetimleri gibi bildirim temelli biçimlendirme lick veya kod yazmak zorunda kalmadan, zengin kullanıcı arabirimi PasswordRecovery ve parola değiştirme denetimlerini işlemeye. Varsayılan kullanıcı arabirimi, ihtiyaçlarınızı karşılamıyorsa, stil özellikleri çeşitli üzerinden özelleştirebilirsiniz. Alternatif olarak, denetimleri arabirimleri bile daha hassas bir denetim derecesini için şablonlar dönüştürülür. Arka planda bu denetimler üyelik API kullanan çağırma `MembershipUser` nesnenin `ResetPassword` ve `ChangePassword` yöntemleri.

Mutluluk programlama!

### <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Bu denetim QuickStarts](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/changepassword.aspx)
- [PasswordRecovery denetimi QuickStarts](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/passwordrecovery.aspx)
- [ASP.NET e-posta gönderme](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)
- [`System.Net.Mail`Sık sorulan sorular](http://www.systemnetmail.com/)

### <a name="about-the-author"></a>Yazar hakkında

Scott Mitchell, birden çok ASP/ASP.NET books yazar ve 4GuysFromRolla.com, kurucusu 1998 itibaren Microsoft Web teknolojileri ile çalışmaktadır. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri  *[kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Tan adresindeki ulaşılabilir [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) veya kendi blog aracılığıyla [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. Bu öğretici için sağlama gözden geçirenler Michael Emmings ve Suchi Banerjee içerir. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bir satırında bana bırak[mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Önceki](building-an-interface-to-select-one-user-account-from-many-cs.md)
[sonraki](unlocking-and-approving-user-accounts-cs.md)
