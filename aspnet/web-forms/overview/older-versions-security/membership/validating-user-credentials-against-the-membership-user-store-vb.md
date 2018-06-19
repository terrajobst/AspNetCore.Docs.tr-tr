---
uid: web-forms/overview/older-versions-security/membership/validating-user-credentials-against-the-membership-user-store-vb
title: Üyelik kullanıcı deposu (VB) karşı kullanıcı kimlik bilgileri doğrulanıyor | Microsoft Docs
author: rick-anderson
description: Bu öğreticide programlı anlamına gelir ve oturum açma denetimi kullanarak üyelik kullanıcı deposunda karşı bir kullanıcının kimlik bilgilerini doğrulamak nasıl inceleyeceğiz...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/18/2008
ms.topic: article
ms.assetid: 17772912-b47b-4557-9ce9-80f22df642f7
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/membership/validating-user-credentials-against-the-membership-user-store-vb
msc.type: authoredcontent
ms.openlocfilehash: f8d3de9736d901e02096d20345650b47c47897ae
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30891340"
---
<a name="validating-user-credentials-against-the-membership-user-store-vb"></a>Üyelik kullanıcı deposu (VB) karşı kullanıcı kimlik bilgileri doğrulanıyor
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indirme](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_06_VB.zip) veya [PDF indirin](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial06_LoggingIn_vb.pdf)

> Bu öğreticide programlı anlamına gelir ve oturum açma denetimi kullanarak üyelik kullanıcı deposunda karşı bir kullanıcının kimlik bilgilerini doğrulamak nasıl inceleyeceğiz. Ayrıca oturum açma denetimin görünümünü ve davranışını özelleştirmek nasıl tümleştirildiği incelenmektedir.


## <a name="introduction"></a>Giriş

İçinde <a id="Tutorial05"> </a> [önceki öğretici](creating-user-accounts-vb.md) üyelik Framework'te yeni bir kullanıcı hesabı oluşturmak nasıl inceledik. Biz öncelikle programlı olarak aracılığıyla kullanıcı hesapları oluşturma Aranan `Membership` sınıfının `CreateUser` yöntemi ve ardından CreateUserWizard Web denetimi kullanma. Ancak, oturum açma sayfasına şu anda sağlanan kimlik bilgilerinin sabit kodlanmış kullanıcı adı ve parola çiftlerinin listesini karşı doğrular. Üyelik framework'ün kullanıcı deposu karşı kimlik doğrulama için oturum açma sayfasının mantığı güncelleştirmeye gerekir.

Çok gibi kullanıcı hesapları oluşturma ile kimlik bilgileri programlı olarak veya bildirimli olarak doğrulanabilir. Üyelik API'si program aracılığıyla kullanıcı deposunda karşı bir kullanıcının kimlik bilgilerini doğrulamak için bir yöntem içerir. Ve ASP.NET kutularındaki kullanıcı adı ve parolasını ve oturum açmak için bir düğmeye sahip bir kullanıcı arabirimi işler oturum açma Web denetimi ile birlikte gelir.

Bu öğreticide programlı anlamına gelir ve oturum açma denetimi kullanarak üyelik kullanıcı deposunda karşı bir kullanıcının kimlik bilgilerini doğrulamak nasıl inceleyeceğiz. Ayrıca oturum açma denetimin görünümünü ve davranışını özelleştirmek nasıl tümleştirildiği incelenmektedir. Haydi başlayalım!

## <a name="step-1-validating-credentials-against-the-membership-user-store"></a>1. adım: Üyeliği kullanıcı deposunda karşı kimlik bilgileri doğrulanıyor

Forms kimlik doğrulaması kullanan Web siteleri için bir kullanıcı Web sitesine bir oturum açma sayfasını ziyaret edip kimlik bilgilerini girme oturum açar. Bu kimlik bilgileri, sonra da kullanıcı deposunda karşı karşılaştırılır. Geçerliyse, kullanıcı kimliğini ve ziyaretçi güvenilirliğini gösteren bir güvenlik belirteci forms kimlik doğrulaması bileti verilir.

Bir kullanıcı üyeliği framework karşı doğrulamak için kullanın `Membership` sınıfının [ `ValidateUser` yöntemi](https://msdn.microsoft.com/library/system.web.security.membership.validateuser.aspx). `ValidateUser` Yöntemi alır iki giriş parametrelerinde - `username` ve `password` - ve kimlik bilgilerinin geçerli olup olmadığını gösteren bir Boole değeri döndürür. İle gibi `CreateUser` biz incelenmesi önceki öğreticide yöntemi `ValidateUser` yöntemi yapılandırılmış üyelik sağlayıcısı gerçek doğrulama atar.

`SqlMembershipProvider` Belirtilen kullanıcının parolasını aracılığıyla elde ederek sağlanan kimlik bilgilerini doğrular `aspnet_Membership_GetPasswordWithFormat` saklı yordamı. Sözcüğünün `SqlMembershipProvider` üç biçimlerden birini kullanarak kullanıcıların parolalarını depolar: şifreli ya da karma temizleyin. `aspnet_Membership_GetPasswordWithFormat` Saklı yordam ham biçimde parolayı döndürür. Şifrelenmiş veya karma parolalar için `SqlMembershipProvider` dönüştüren `password` içine geçirilen değeri `ValidateUser` karşılığını yönteme şifrelenmiş veya durumu karma ve veritabanı tarafından döndürülen ile karşılaştırır. Kullanıcı tarafından girilen biçimlendirilmiş parola veritabanında depolanan parola uyarsa, kimlik bilgileri geçerli değil.

Şimdi oturum açma sayfamızı güncelleştirme (~ /`Login.aspx`) böylece sağlanan kimlik bilgilerinin üyelik framework kullanıcı deposunda karşı doğrular. Bu oturum açma sayfasına oluşturduğumuz geri <a id="Tutorial02"> </a> [ *form kimlik doğrulaması bir genel bakış* ](../introduction/an-overview-of-forms-authentication-vb.md) kullanıcı adı ve parola, iki metin kutuları ile bir arabirim oluşturma öğretici, bir Beni anımsa onay ve oturum açma düğmesi (bkz: Şekil 1). Kod girilen kimlik bilgilerine sabit kodlanmış kullanıcı adı ve parola çiftlerinin listesini (Scott/parola, Jisun/parola ve Sam/parola) karşı doğrular. İçinde <a id="Tutorial03"> </a> [ *Forms kimlik doğrulaması yapılandırması ve Gelişmiş konular* ](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md) biz güncelleştirilmiş formlarında ek bilgileri depolamak için oturum açma sayfasının kod Öğreticisi kimlik doğrulama anahtarı'nın `UserData` özelliği.


[![İki metin kutuları, bir CheckBoxList ve bir düğme oturum açma sayfasının arabirimi içerir](validating-user-credentials-against-the-membership-user-store-vb/_static/image2.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image1.png)

**Şekil 1**: oturum açma sayfasının arabirimi içeren iki metin kutuları, bir CheckBoxList ve bir düğme ([tam boyutlu görüntüyü görüntülemek için tıklatın](validating-user-credentials-against-the-membership-user-store-vb/_static/image3.png))


Oturum açma sayfasının kullanıcı arabirimi değişmeden kalır, ancak oturum açma düğmenin değiştirmek ihtiyacımız `Click` kullanıcının üyelik framework kullanıcı deposunda karşı doğrulama kodunu içeren olay işleyicisi. Olay işleyicisi güncelleştirin, böylece kendi kod aşağıdaki gibidir:

[!code-vb[Main](validating-user-credentials-against-the-membership-user-store-vb/samples/sample1.vb)]

Bu kod çok kolaydır. Arayarak Başlat `Membership.ValidateUser` yöntemi, sağlanan kullanıcı adı ve parola geçirme. Bu yöntem True değerini döndürür sonra siteye kullanıcı oturumu `FormsAuthentication` sınıfının RedirectFromLoginPage yöntemi. (Biz anlatıldığı gibi <a id="Tutorial02"> </a> [ *form kimlik doğrulaması bir genel bakış* ](../introduction/an-overview-of-forms-authentication-vb.md) öğretici, `FormsAuthentication.RedirectFromLoginPage` forms kimlik doğrulaması bileti oluşturur ve ardından kullanıcı yönlendirir uygun sayfaya.) Kimlik bilgileri ancak geçersiz olduğunda `InvalidCredentialsMessage` etiketi görüntülenir, kullanıcının kendi kullanıcı adı veya parola yanlış olduğunu bildiren.

Tüm olan İşte bu kadar!

Oturum açma sayfasına beklendiği gibi çalışıp çalışmadığını sınamak için önceki öğreticide oluşturduğunuz kullanıcı hesaplarından birisini ile oturum açma girişimi. Veya, hesabınız henüz oluşturmadıysanız, devam etmek ve birinden oluşturmak `~/Membership/CreatingUserAccounts.aspx` sayfası.

> [!NOTE]
> Kullanıcı kendi kimlik bilgilerini girer ve oturum açma sayfası formunun gönderir, kendi parola gibi kimlik bilgilerini de web sunucusu için Internet üzerinden iletilirken *düz metin*. Ağ trafiği algılaması korsan kullanıcı adı ve parola görebilirsiniz anlamına gelir. Bunu önlemek için bunu kullanarak ağ trafiğini şifrelemek için önemlidir [Güvenli Yuva Katmanı (SSL)](http://en.wikipedia.org/wiki/Secure_Sockets_Layer). Bu, kimlik bilgilerini (yanı sıra tüm sayfanın HTML biçimlendirmesi) web sunucusu tarafından alınana kadar kullanıcılar tarayıcı bırakın andan şifrelenmiş güvence altına alır.


### <a name="how-the-membership-framework-handles-invalid-login-attempts"></a>Üyelik Framework geçersiz oturum açma denemesi nasıl işler?

Bir ziyaretçi oturum açma sayfasına ulaşana ve kimlik bilgilerini gönderdiğinde, tarayıcısında oturum açma sayfasına bir HTTP isteği yapar. Kimlik bilgilerinin geçerli olduğundan, HTTP yanıtı bir tanımlama bilgisinde kimlik doğrulaması bileti içerir. Bu nedenle, sitenize bölün çalışılırken bir korsan ayrıntısına geçerli bir kullanıcı adı ve parola konumundaki tahmin ile oturum açma sayfasına HTTP istekleri gönderen bir program oluşturabilirsiniz. Parola tahmin doğru ise, oturum açma sayfasına geçerli bir kullanıcı adı/parola çifti stumbled, bu noktada program bilir kimlik doğrulama bileti tanımlama döndürür. Özellikle parolası zayıf olduğunda deneme yanılma, böyle bir program bir kullanıcının parolasını stumble olabilir.

Bu tür deneme yanılma saldırıları önlemek için belirli bir sayıda belirli bir süre içinde başarısız oturum açma girişimi varsa bir kullanıcı üyeliği framework kilitler. Tam parametreleri aşağıdaki iki üyelik sağlayıcısı yapılandırma ayarları yapılandırılabilir:

- `maxInvalidPasswordAttempts` -kaç geçersiz parola belirtir denemeleri, Hesap kilitlenmeden önce süre içinde kullanıcı için verilir. Varsayılan değer 5'tir.
- `passwordAttemptWindow` -süre sırasında belirtilen sayıda geçersiz oturum açma denemesi neden olacak hesap kilitlenmesi dakika cinsinden belirtir. Varsayılan değer 10'dur.

Bir kullanıcı kilitlendi, bir yönetici kendi hesabını açana kadar aynen oturum açılamaz. Bir kullanıcı, kilitlendiğinde `ValidateUser` yöntemi olacaktır *her zaman* dönmek `False`geçerli kimlik bilgileri sağlanan olsa bile. Bu davranış bir korsan sitenize yanılma yöntemlerle kesintiye uğrar olasılığını azaltır, ancak yalnızca kendi parolasını unutursa veya yanlışlıkla Caps Lock açmıştır ya da hatalı yazarak günde sahip bir geçerli kullanıcı kilitleme sona erdirebilirsiniz.

Ne yazık ki, bir kullanıcı hesabının kilidini açmak için yerleşik aracı yoktur. Bir hesap kilidini açmak için veritabanı değiştirebilirsiniz doğrudan - değiştirmek `IsLockedOut` alanındaki `aspnet_Membership` tablo için uygun kullanıcı hesabı - ya da kilitlerini seçeneklerle hesapları kilitli listeleyen web tabanlı bir arabirim oluşturun. Sonraki öğreticide ortak kullanıcı hesabı ve rol ilgili görevleri gerçekleştirmeye oluşturma yönetim arabirimleri inceleyeceğiz.

> [!NOTE]
> Bir dezavantajı `ValidateUser` yöntemdir sağlanan kimlik bilgileri geçersiz olduğunda, bu nedenle konusunda herhangi bir açıklama sağlamaz. Kullanıcı Mağazası'nda, eşleşen bir kullanıcı adı/parola çifti olduğundan veya kullanıcı henüz onaylanmadığı için veya kullanıcı kilitlendi için kimlik bilgileri geçersiz olabilir. 4. adımda kullanıcıların oturum açma girişimi başarısız olduğunda bir ayrıntılı ileti kullanıcıya göstermek nasıl göreceğiz.


## <a name="step-2-collecting-credentials-through-the-login-web-control"></a>2. adım: Oturum açma Web denetimi ile kimlik bilgileri toplama

[Oturum açma Web denetimi](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.aspx) varsayılan kullanıcı arabirimi çok benzeyen oluşturduğumuz geri işler <a id="Tutorial02"> </a> [ *form kimlik doğrulaması bir genel bakış* ](../introduction/an-overview-of-forms-authentication-vb.md) Öğreticisi. Oturum açma denetimi kullanarak bize ziyaretçi kimlik bilgileri toplamak için arabirim oluşturmak zorunda işlemlerini kaydeder. Ayrıca, oturum açma denetimi otomatik olarak (gönderilen kimlik bilgilerinin geçerli olduğunu varsayarak), kullanıcı böylece bize herhangi bir kod yazmak zorunda kalmaktan açar.

Şimdi güncelleştirme `Login.aspx`, el ile oluşturulan arabirimi değiştiriliyor ve kod bir oturum açma denetimi. Başlangıç varolan biçimlendirme kaldırarak ve kod `Login.aspx`. Depolayabileceği silebilir veya yalnızca açıklamadan çıkarın. Bildirim temelli biçimlendirme açıklama eklemek için kendisiyle çevreleyen `<%--` ve `--%>` sınırlayıcısı. Bu sınırlayıcılar el ile girebilir veya Şekil 2'de görüldüğü gibi çıkışı açıklama ve açıklama araç çubuğunda seçilen satırlar simgesi çıkışı ardından metne seçebilirsiniz. Benzer şekilde, arka plandaki kod sınıfı seçili kod çıkışı açıklama eklemek için seçilen satırlar simgesi çıkışı açıklamayı kullanabilirsiniz.


[![Varolan bildirim temelli biçimlendirme ve Login.aspx kaynak kodunda açıklama](validating-user-credentials-against-the-membership-user-store-vb/_static/image5.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image4.png)

**Şekil 2**: açıklama çıkışı varolan bildirim temelli biçimlendirme ve Login.aspx kaynak kodunda ([tam boyutlu görüntüyü görüntülemek için tıklatın](validating-user-credentials-against-the-membership-user-store-vb/_static/image6.png))


> [!NOTE]
> Seçili satırları simgesi çıkışı yorum, bildirim temelli biçimlendirme Visual Studio 2005'te görüntülerken kullanılabilir değil. Visual Studio 2008 kullanmıyorsanız el ile eklemeniz gerekir `<%--` ve `--%>` sınırlayıcısı.


Ardından, sayfayı açın araç kutusundan bir oturum açma denetimi sürükleyin ve ayarlayın, `ID` özelliğine `myLogin`. Bu noktada, ekranınızın Şekil 3'e benzer görünmelidir. Oturum açma denetimin varsayılan arabirimi kullanıcı adı ve parola, bir Beni anımsa sonraki sefer onay kutusunu ve bir günlük düğmesine TextBox denetimleri içerdiğine dikkat edin. Ayrıca `RequiredFieldValidator` iki metin kutuları için kontrol eder.


[![Bir oturum açma denetimi sayfasına ekleme](validating-user-credentials-against-the-membership-user-store-vb/_static/image8.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image7.png)

**Şekil 3**: sayfasına bir oturum açma denetim ekleme ([tam boyutlu görüntüyü görüntülemek için tıklatın](validating-user-credentials-against-the-membership-user-store-vb/_static/image9.png))


Ve bitmek! Oturum açma denetimin oturum aç düğmesine tıklandığında, geri gönderimin ortaya çıkar ve oturum açma denetimi çağıracak `Membership.ValidateUser` yöntemi, girilen kullanıcı adı ve parola geçirme. Kimlik bilgileri geçersiz oturum açma denetimi gibi bildiren bir ileti görüntüler. Ancak, kimlik bilgilerinin geçerli olduğundan, oturum açma denetimi forms kimlik doğrulaması bileti oluşturur ve kullanıcının uygun sayfasına yönlendirir.

Oturum açma denetimi dört etkene başarılı oturum açma sırasında kullanıcıya yönlendirmek için uygun sayfaya belirlemek için kullanır:

- Oturum açma denetimi oturum açma sayfasında tarafından tanımlanan olup `loginUrl` forms kimlik doğrulaması yapılandırması; bu ayarın varsayılan değer: `Login.aspx`
- Varlığını bir `ReturnUrl` sorgu dizesi parametresi
- Oturum açma denetimin değerini [ `DestinationUrl` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.destinationpageurl.aspx)
- `defaultUrl` Değeri belirtilen forms kimlik doğrulaması yapılandırma ayarları; bu ayarın varsayılan değeri Default.aspx

Şekil 4 nasıl gösterilmektedir, uygun sayfaya kararını ulaşması için bu dört parametre oturum açma denetimi kullanır.


[![Bir oturum açma denetimi sayfasına ekleme](validating-user-credentials-against-the-membership-user-store-vb/_static/image11.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image10.png)

**Şekil 4**: sayfasına bir oturum açma denetim ekleme ([tam boyutlu görüntüyü görüntülemek için tıklatın](validating-user-credentials-against-the-membership-user-store-vb/_static/image12.png))


Bir tarayıcı aracılığıyla sitesini ziyaret edip üyelik Framework'te var olan bir kullanıcı oturum açma tarafından oturum açma denetimi sınamak için bir dakikanızı ayırın.

Oturum açma denetimin işlenmiş yüksek oranda yapılandırılabilir arabirimidir. Bir dizi görünümünü etkileyen özellikleri vardır; daha, oturum açma denetimi düzenini denetleyebilmeniz için bir şablona kullanıcı arabirimi öğeleri dönüştürülebilir. Bu adımın geri kalanını düzeni ve görünümünü özelleştirmek nasıl inceler.

### <a name="customizing-the-login-controls-appearance"></a>Oturum açma denetiminin görünümünü özelleştirme

Oturum açma denetimin varsayılan özellik ayarlarını bir başlık (günlük) sahip bir kullanıcı arabirimi oluşturmak, TextBox ve etiket için kullanıcı adı ve parola girdi, bir Beni anımsa yanındaki onay kutusunu ve oturum aç düğmesine zaman denetler. Bu öğeleri görünümlerini çok sayıda oturum açma denetimin özelliklerini tüm yapılandırılabilir. Ayrıca, ek kullanıcı arabirimi öğeleri - yeni bir kullanıcı hesabı - oluşturmak için bir sayfaya bir bağlantı gibi bir özelliğin ya da iki ayarlayarak eklenebilir.

Şimdi bizim Login denetiminin görünümünü oluşturan gussy için birkaç dakikanızı ayırın. Bu yana `Login.aspx` sayfasında, sayfanın üst kısmındaki oturum açma bildiren metin zaten vardır, oturum açma denetimin başlık gereksiz. Bu nedenle, temizleyin [ `TitleText` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.titletext.aspx) oturum açma denetimin başlığı kaldırmak için değer.

Kullanıcı adı: ve parola: iki TextBox solundaki etiketleri aracılığıyla özelleştirilebilir [ `UserNameLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.usernamelabeltext.aspx) ve [ `PasswordLabelText` özellikleri](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.passwordlabeltext.aspx)sırasıyla. Kullanıcı adı değiştirelim: kullanıcı adı okunamıyor etiketi:. Etiket ve metin kutusu stilleri aracılığıyla yapılandırılabilir [ `LabelStyle` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.labelstyle.aspx) ve [ `TextBoxStyle` özellikleri](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.textboxstyle.aspx)sırasıyla.

Sonraki zaman CheckBox'ın metin özelliği oturum açma denetimin ayarlanabilir Beni anımsa [ `RememberMeText` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.remembermetext.aspx), ve kendi varsayılan durumu aracılığıyla yapılandırılabilir işaretli [ `RememberMeSet` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.remembermeset.aspx)(hangi varsayılan değeri False). Bir tane ayarlamak `RememberMeSet` Beni anımsa sonraki saat onay kutusunu böylece true özelliği varsayılan olarak denetlenir.

Oturum açma denetimi kullanıcı arabirimi denetimlerini düzenini ayarlamak için iki özellikleri sunar. [ `TextLayout` Özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.textlayout.aspx) gösterir olup olmadığını Username: ve parola: etiketleri görünür bunların karşılık gelen kutularındaki (varsayılan) ya da bunları üstünde sol. [ `Orientation` Özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.orientation.aspx) kullanıcı adı ve parola girişlerini dikey yer olup olmadığını gösterir (bir üst üste) ya da yatay olarak. Varsayılan değerlerine ayarlayın bu iki özellik bırakın kullanacağım ancak, bu iki özellik elde edilen etkisini görmek için varsayılan olmayan değerlerine ayarlamayı deneyin öneririz.

> [!NOTE]
> Sonraki bölümde, oturum açma denetimin yerleşimi, yapılandırma biz Düzen denetimin kullanıcı arabirimi öğeleri kesin düzenini tanımlamak için şablonlar kullanarak arar.


Oturum açma denetimin özellik ayarlarını ayarlayarak kaydırma [ `CreateUserText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.createusertext.aspx) ve [ `CreateUserUrl` özellikleri](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.createuserurl.aspx) için henüz kayıtlı değil mi? Bir hesap oluşturun! ve `~/Membership/CreatingUserAccounts.aspx`sırasıyla. Bu sayfaya işaret eden oturum açma denetimin arabirimine oluşturduğumuz içinde bir köprü ekler <a id="Tutorial05"> </a> [önceki Öğreticisi](creating-user-accounts-vb.md). Oturum açma denetimin [ `HelpPageText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.helppagetext.aspx) ve [ `HelpPageUrl` özellikleri](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.helppageurl.aspx) ve [ `PasswordRecoveryText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.passwordrecoverytext.aspx) ve [ `PasswordRecoveryUrl` özellikleri](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.passwordrecoveryurl.aspx) bağlantılar Yardım sayfası ve bir parola kurtarma sayfa işleme aynı şekilde çalışır.

Bu özellik değişiklikleri yaptıktan sonra oturum açma denetiminizin bildirim temelli biçimlendirme ve görünüm, Şekil 5'te gösterilen benzer görünmelidir.


[![Oturum açma denetimin özelliklerini değerleri görünümünü dikte](validating-user-credentials-against-the-membership-user-store-vb/_static/image14.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image13.png)

**Şekil 5**: oturum açma denetimin özelliklerini değerleri dikte ait Görünüm ([tam boyutlu görüntüyü görüntülemek için tıklatın](validating-user-credentials-against-the-membership-user-store-vb/_static/image15.png))


### <a name="configuring-the-login-controls-layout"></a>Oturum açma denetimin düzeni yapılandırma

Bir HTML arabirimde kullanıma oturum açma Web denetimin varsayılan kullanıcı arabirimi yerleştirir `<table>`. Ancak ne işlenmiş çıkış üzerinde daha hassas denetim ihtiyacımız var? Biz değiştirmek istediğiniz belki `<table>` bir dizi `<div>` etiketler. Veya ne uygulamamız kimlik doğrulaması için ek kimlik bilgileri gerektiriyor? Birçok finansal Web siteleri, örneğin, yalnızca bir kullanıcı adı ve parola, ancak aynı zamanda kişisel kimlik numarası (PIN) veya diğer tanımlayıcı bilgiler sağlamak kullanıcıların gerektirir. Ne olursa olsun nedenleri olabilir, oturum açma denetimi, biz açıkça arabiriminin bildirim temelli biçimlendirme tanımlayabilirsiniz bir şablona dönüştürmek mümkündür.

Ek kimlik bilgileri toplamak için oturum açma denetimi güncelleştirmek için iki işlem yapmanız gerekir:

1. Ek kimlik bilgileri toplamak için Web control(s) dahil etmek için oturum açma denetimin arabirimini güncelleştirin.
2. Böylece bir kullanıcı yalnızca kendi kullanıcı adı ve parola geçerli ise ve ek kimlik bilgilerini çok geçerli kimlik doğrulaması oturum açma denetim iç kimlik doğrulaması mantığının geçersiz kılar.

İlk görevi gerçekleştirmek için şu oturum açma denetimi şablona dönüştürün ve gerekli Web denetimleri eklemeniz gerekir. İkinci görev için olduğu gibi oturum açma denetimin kimlik doğrulaması mantığı ve denetim için bir olay işleyicisi oluşturarak yerini [ `Authenticate` olay](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.authenticate.aspx).

Böylece kullanıcılar kendi kullanıcı adı, parola ve e-posta adresi için ister ve yalnızca kendi e-posta adresi dosyada sağlanan e-posta adresi eşleşmesi durumunda kullanıcının kimliğini doğrular şimdi oturum açma denetimi güncelleştirin. Biz öncelikle bir şablona oturum açma denetimin arabirimi dönüştürmeniz gerekir. Oturum açma denetimin akıllı etiketten dönüştürme şablonu seçeneğini seçin.


[![Oturum açma denetimi şablona dönüştürme](validating-user-credentials-against-the-membership-user-store-vb/_static/image17.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image16.png)

**Şekil 6**: oturum açma denetimi şablona dönüştürme ([tam boyutlu görüntüyü görüntülemek için tıklatın](validating-user-credentials-against-the-membership-user-store-vb/_static/image18.png))


> [!NOTE]
> Oturum açma denetimi önceden template sürüme dönmek için denetimin akıllı etiket sıfırlama bağlantısını tıklatın.


Oturum açma denetimi için bir şablon dönüştürme ekler bir `LayoutTemplate` denetimin bildirim temelli biçimlendirme HTML öğeleri ve kullanıcı arabirimini tanımlayan Web denetimleri için. Şekil 7'de görüldüğü gibi bir şablona denetimi dönüştürme bir dizi Özellikler penceresinden gibi kaldırır `TitleText`, `CreateUserUrl`, vb., sonra bu özellik değerleri, bir şablon kullanırken göz ardı edilir.


[![Daha az özellikler kullanılabilir olduğunda oturum açma denetim için bir şablon dönüştürülür:](validating-user-credentials-against-the-membership-user-store-vb/_static/image20.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image19.png)

**Şekil 7**: daha az özelliklerdir kullanılabilir olduğunda oturum açma denetim için bir şablon dönüştürülür ([tam boyutlu görüntüyü görüntülemek için tıklatın](validating-user-credentials-against-the-membership-user-store-vb/_static/image21.png))


HTML biçimlendirmede `LayoutTemplate` gerektiğinde değiştirilebilir. Benzer şekilde, tüm yeni Web denetimleri şablona eklemekten çekinmeyin. Ancak, bu oturum açma denetimin çekirdek Web denetimleri şablonda kalır ve bunların atanmış tutmak önemlidir `ID` değerleri. Özellikle, yeniden adlandırmak veya kaldırmayın `UserName` veya `Password` metin kutuları, `RememberMe` onay kutusunu `LoginButton` düğmesi, `FailureText` etiketi veya `RequiredFieldValidator` kontrol eder.

Ziyaretçi e-posta adresi toplamak için biz TextBox şablona eklemeniz gerekir. Tablo satırının arasında aşağıdaki bildirim temelli biçimlendirmeyi ekleyin (`<tr>`) içeren `Password` TextBox ve Beni anımsa sonraki tutan tablo satırındaki onay kutusunu süresi:

[!code-aspx[Main](validating-user-credentials-against-the-membership-user-store-vb/samples/sample2.aspx)]

Ekledikten sonra `Email` metin kutusuna, bir tarayıcı aracılığıyla sayfasını ziyaret edin. Şekil 8'de görüldüğü gibi oturum açma denetimin kullanıcı arabirimi şimdi üçüncü metin kutusu içerir.


[![Oturum açma denetimi, bir metin kutusu artık için kullanıcının e-posta adresini içerir.](validating-user-credentials-against-the-membership-user-store-vb/_static/image23.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image22.png)

**Şekil 8**: oturum açma denetimi, bir metin kutusu artık için kullanıcının e-posta adresini içerir. ([tam boyutlu görüntüyü görüntülemek için tıklatın](validating-user-credentials-against-the-membership-user-store-vb/_static/image24.png))


Bu noktada, oturum açma denetimi kullanmaya devam `Membership.ValidateUser` sağlanan kimlik bilgilerini doğrulamak için yöntemi. Değer gelenlere, girilen `Email` metin kutusu kullanıcı oturum açabilir üzerinde şifrelemeyle sahiptir. Adım 3'te kimlik bilgilerini yalnızca kullanıcı adı ve parolasının geçerli olduğundan ve dosya e-posta adresi ile sağlanan e-posta adresi eşleştiğini geçerli olarak kabul edilir böylece oturum açma denetimin kimlik doğrulaması mantığı geçersiz kılmak nasıl ele alacağız.

## <a name="step-3-modifying-the-login-controls-authentication-logic"></a>3. adım: oturum açma denetimin kimlik doğrulaması mantığı değiştirme

Her bir ziyaretçi sağlar, kimlik bilgilerini ve oturum aç düğmesine, geri gönderimin ensues tıklar ve oturum açma, kimlik doğrulama iş akışı ilerleyerek denetler. İş akışı yükselterek başlar [ `LoggingIn` olay](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.loggingin.aspx). Bu olayla ilişkili tüm olay işleyicileri ayarlayarak işlem günlüğünde iptal edebilirsiniz `e.Cancel` özelliğine `True`.

İşlem günlüğünde iptal edilmemiş, iş akışı yükselterek ilerler [ `Authenticate` olay](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.authenticate.aspx). Bir olay işleyicisi için ise `Authenticate` olay, ardından sağlanan kimlik bilgilerinin geçerli olup olmadığını belirlemek için sorumlu. Hiçbir olay işleyicisi belirtildiyse, oturum açma denetimi kullanır `Membership.ValidateUser` kimlik bilgilerinin geçerliliğini belirlemek için yöntem.

Sağlanan kimlik bilgilerinin geçerli sonra forms kimlik doğrulaması biletinin oluşturulduğu [ `LoggedIn` olay](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.loggedin.aspx) ortaya çıkar ve kullanıcı uygun sayfaya yönlendirilir. Ancak, kimlik bilgileri geçersiz sayılan, sonra [ `LoginError` olay](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.loginerror.aspx) tetiklenir ve kullanıcı kimlik bilgilerini geçersiz olduğunu bildiren bir ileti görüntülenir. Yalnızca denetim varsayılan olarak, oturum açma hatası durumunda ayarlar kendi `FailureText` denetimin metin özelliğini (oturum açma girişiminiz başarılı değildi. hata iletisi etiket Lütfen yeniden deneyin). Ancak, oturum açma denetimin [ `FailureAction` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.failureaction.aspx) ayarlanır `RedirectToLoginPage`, sonra oturum açma denetimi sorunları bir `Response.Redirect` sorgu dizesi parametresi ekleme oturum açma sayfasına `loginfailure=1` (neden olan oturum açma hata iletisini görüntülemek üzere Denetim).

Şekil 9 kimlik doğrulama iş akışı akış çizelgesi sunar.


[![Oturum açma denetimin kimlik doğrulama iş akışı](validating-user-credentials-against-the-membership-user-store-vb/_static/image26.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image25.png)

**Şekil 9**: oturum açma denetimin kimlik doğrulama iş akışı ([tam boyutlu görüntüyü görüntülemek için tıklatın](validating-user-credentials-against-the-membership-user-store-vb/_static/image27.png))


> [!NOTE]
> Ne zaman kullanacağınız düşünüyorsunuz varsa `FailureAction`'s `RedirectToLogin` seçeneği sayfasında, aşağıdaki senaryoyu göz önünde bulundurun. Şu anda bizim `Site.master` ana sayfa stranger anonim bir kullanıcı tarafından ziyaret edildiğinde sol sütunda görüntülenen metin, Hello, şu anda var ancak biz metnin bir oturum açma denetimi ile değiştirmek istediğinizi düşünelim. Bu, oturum açma sayfasına doğrudan gitmek erişmeleri yerine bu sitedeki herhangi bir sayfadan oturum açmak anonim kullanıcı olanak tanır. Bir kullanıcının ana sayfa tarafından işlenen oturum açma denetimi aracılığıyla oturum açamadı, ancak, bu oturum açma sayfasına yeniden yönlendirmek için mantıklı olabilir (`Login.aspx`) büyük olasılıkla bu sayfaya ek yönergeler, bağlantılar ve bağlantıları oluşturma gibi diğer Yardım - içerdiğinden bir Yeni hesap veya ana sayfaya eklenmedi kayıp parola - alınamıyor.


### <a name="creating-theauthenticateevent-handler"></a>Oluşturma`Authenticate`olay işleyicisi

Özel kimlik doğrulama mantığımızı takın için oturum açma denetim için bir olay işleyicisi oluşturmak ihtiyacımız `Authenticate` olay. Bir olay işleyicisi oluşturma `Authenticate` olay, aşağıdaki olay işleyici tanımının üretir:

[!code-vb[Main](validating-user-credentials-against-the-membership-user-store-vb/samples/sample3.vb)]

Gördüğünüz gibi `Authenticate` olay işleyicisi türünde bir nesne geçirilen [ `AuthenticateEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.authenticateeventargs.aspx) ikinci kendi giriş parametresi olarak. `AuthenticateEventArgs` Sınıfı içeren adlı bir Boolean özelliği `Authenticated` sağlanan kimlik bilgilerinin geçerli olup olmadığını belirtmek için kullanılır. Bizim görev, sağlanan kimlik bilgilerinin geçerli olup olmadığını belirleyen kodu buraya yazın ve ayarlamak için ise `e.Authenticate` özelliği uygun şekilde.

### <a name="determining-and-validating-the-supplied-credentials"></a>Belirleme ve sağlanan kimlik bilgileri doğrulanıyor

Oturum açma denetimin kullanmak [ `UserName` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.username.aspx) ve [ `Password` özellikleri](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.password.aspx) kullanıcı tarafından girilen kullanıcı adı ve parola kimlik bilgileri belirlemek için. Tüm ek Web denetimlere girilen değerleri belirlemek için (gibi `Email` metin kutusuna önceki adımda eklediğimiz), kullanın `LoginControlID.FindControl`("*`controlID`*") Web programlı bir başvuru almak için Şablonda, Denetim `ID` özelliği eşittir *`controlID`*. Örneğin, bir başvuru almak için `Email` metin kutusuna, aşağıdaki kodu kullanın:

`Dim EmailTextBox As TextBox = CType(myLogin.FindControl("Email"), TextBox)`

Kullanıcının kimlik bilgilerini doğrulamak için şu iki işlem yapmanız gerekir:

1. Sağlanan kullanıcı adı ve parola geçerli olduğundan emin olun
2. Girilen e-posta adresi oturum açma girişiminde bulunan kullanıcının dosya e-posta adresi ile eşleştiğinden emin olun

Yalnızca kullanabileceğiniz ilk denetimi gerçekleştirmek için `Membership.ValidateUser` adım 1'de gördüğümüz gibi yöntemi. İkinci kontrol edin, böylece biz TextBox denetimine girilen e-posta adresine karşılaştırabilirsiniz kullanıcının e-posta adresini belirlemek ihtiyacımız. Belirli bir kullanıcı hakkında bilgi almak için kullanmak `Membership` sınıfının [ `GetUser` yöntemi](https://msdn.microsoft.com/library/system.web.security.membership.getuser.aspx).

`GetUser` Yöntemi aşırı sayısı sahiptir. Parametreleri geçirme olmadan kullanılırsa, şu anda oturum açmış olan kullanıcının hakkında bilgi verir. Belirli bir kullanıcı hakkında bilgi almak için çağrı `GetUser` kendi kullanıcı geçirme. Her iki durumda da `GetUser` döndüren bir [ `MembershipUser` nesne](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx), gibi özellikleri olan `UserName`, `Email`, `IsApproved`, `IsOnline`ve benzeri.

Aşağıdaki kod bu iki denetimleri uygular. Her ikisi de, ardından geçirirseniz `e.Authenticate` ayarlanır `True`, aksi takdirde atanan `False`.

[!code-vb[Main](validating-user-credentials-against-the-membership-user-store-vb/samples/sample4.vb)]

Doğru kullanıcı adını, parolayı ve e-posta adresini girerek geçerli bir kullanıcı oturum açmak yerinde bu kodu ile çalışır. Yeniden deneyin, ancak bu kez var yanlış e-posta adresini kullanır (bkz. Şekil 10). Son olarak, mevcut olmayan bir kullanıcı adı kullanarak üçüncü bir kez deneyin. İlk durumda, başarıyla siteye oturum açmış, ancak son iki durumda da oturum açma denetimin geçersiz kimlik bilgileri iletiyi görmeniz gerekir.


[![Yanlış e-posta adresi sağlanırken Tito oturum açamıyorsanız](validating-user-credentials-against-the-membership-user-store-vb/_static/image29.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image28.png)

**Şekil 10**: Tito olamaz günlük içinde olduğunda sağladığını yanlış bir e-posta adresi ([tam boyutlu görüntüyü görüntülemek için tıklatın](validating-user-credentials-against-the-membership-user-store-vb/_static/image30.png))


> [!NOTE]
> 1. adımda nasıl üyelik Framework işleme geçersiz oturum açma denemesi bölümünde anlatıldığı gibi zaman `Membership.ValidateUser` yöntemi çağrılır ve geçersiz kimlik bilgileri geçirilen, geçersiz oturum açma denemesi izler ve belirli bir aşarsanız kullanıcı kilitler Belirtilen zaman penceresi içinde geçersiz denemeleri eşiği. Bizim Özel kimlik doğrulama mantığı çağrıları itibaren `ValidateUser` yöntemi, hatalı bir parolanın geçerli bir kullanıcı adı için geçersiz oturum açma girişimi sayacı Artır, ancak bu sayaç, kullanıcı adı ve parola nerede geçerli durumda artırılır değil ancak e-posta adresi doğru değil. Olasılığı olan bir bilgisayar korsanının kullanıcı adı ve parola biliyorum, ancak kullanıcının e-posta adresini belirlemek için deneme yanılma saldırısı teknikleri kullanmak zorunda olduğunu olası olduğundan bu davranış uygundur.


## <a name="step-4-improving-the-login-controls-invalid-credentials-message"></a>4. adım: oturum açma denetimin geçersiz kimlik bilgileri iletisi artırma

Bir kullanıcı geçersiz kimlik bilgileriyle oturum girişiminde bulunduğunda, oturum açma denetimi oturum açma denemesi başarısız olduğunu belirten bir ileti görüntüler. Özellikle, denetimi tarafından belirtilen iletisi görüntüler kendi [ `FailureText` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.failuretext.aspx), oturum açma girişiminiz varsayılan değeri olan başarılı olmadı. Lütfen yeniden deneyin.

Geri çağırma neden bir kullanıcının kimlik bilgileri geçersiz olabilir pek çok nedeni vardır:

- Kullanıcı adı yok
- Kullanıcı adı var, ancak parola geçersiz
- Kullanıcı adı ve parola geçerlidir, ancak kullanıcı henüz onaylanmamış
- Kullanıcı adı ve parola geçerlidir, ancak kullanıcının kilitlenip çıkış (belirtilen zaman çerçevesinde geçersiz oturum açma girişimlerinin sayısını aştığı için büyük olasılıkla)

Ve özel kimlik doğrulama mantığı kullanırken diğer nedenleri olabilir. Örneğin, kodla yazdığımız adım 3, kullanıcı adı ve parola geçerli olabilir, ancak e-posta adresi yanlış olabilir.

Neden kimlik bilgileri geçersiz bağımsız olarak, oturum açma denetimi aynı hata iletisini görüntüler. Bu geri bildirim eksikliği kullanıcı hesabı henüz henüz onaylanmamış veya kimin kilitlendi için kafa karıştırıcı olabilir. Biraz iş ile ancak biz daha uygun bir ileti görüntüler oturum açma denetimi olabilir.

Her bir kullanıcı çalışır Login denetimini başlatır geçersiz kimlik bilgileri ile oturum açmak kendi `LoginError` olay. Şimdi bu olay için bir olay işleyicisi oluşturun ve aşağıdaki kodu ekleyin:

[!code-vb[Main](validating-user-credentials-against-the-membership-user-store-vb/samples/sample5.vb)]

Yukarıdaki kod, oturum açma denetimin ayarlayarak başlatır `FailureText` özelliğinin varsayılan değeri (oturum açma girişiminiz başarılı değildi. Lütfen yeniden deneyin). Sağlanan kullanıcı adı olan bir kullanıcı hesabına eşlemeleri olmadığını görmek için o denetler. Bu nedenle, elde edilen başvuruyorsa `MembershipUser` nesnenin `IsLockedOut` ve `IsApproved` hesap kilitlendi, veya henüz onaylanmadığı belirlemek için özellikler. Her iki durumda da `FailureText` özelliği için karşılık gelen bir değer güncelleştirilir.

Bu kodu test etmek için var olan bir kullanıcı olarak oturum açın, ancak yanlış bir parola kullanın girişimi. Bu beş kez satır 10 dakikalık bir zaman çerçevesinde içinde yapın ve hesap kilitlenir. Şekil 11 gösterir, sonraki oturum açma denemeleri her zaman (doğru parolayla bile) başarısız ancak şimdi daha açıklayıcı görüntüleme gibi hesabınız çok fazla geçersiz oturum açma denemesi nedeniyle kilitlendi. Lütfen Hesap kilidi iletinizi sağlamak için yöneticinize başvurun.


[![Tito çok fazla geçersiz oturum açma denemesi gerçekleştirilen ve kilitlendi](validating-user-credentials-against-the-membership-user-store-vb/_static/image32.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image31.png)

**Şekil 11**: Tito gerçekleştirilen çok fazla sayıda geçersiz oturum açma denemesi ve var olan kilitli Out ([tam boyutlu görüntüyü görüntülemek için tıklatın](validating-user-credentials-against-the-membership-user-store-vb/_static/image33.png))


## <a name="summary"></a>Özet

Önceki Bu öğretici, sabit kodlanmış kullanıcı adı/parola çiftlerinin listesini karşı sağlanan kimlik bilgileri oturum açma sayfamızı doğrulandı. Bu öğreticide, üyelik framework karşı kimlik doğrulamaya sayfa güncelleştirildi. 1. adımda biz kullanarak Aranan `Membership.ValidateUser` yöntemi programlı olarak. 2. adımda biz bizim el ile oluşturulan kullanıcı arabirimi ve kod oturum açma denetimi ile değiştirilir.

Oturum açma denetimi bir standart oturum açma kullanıcı arabirimi işler ve otomatik olarak kullanıcının kimlik bilgilerini üyelik framework karşı doğrular. Ayrıca, geçerli kimlik bilgileri olması durumunda oturum açma denetimi form kimlik doğrulaması oturum açtığında. Kısacası, tam olarak işlevsel oturum açma kullanıcı deneyiminde bir, hiçbir ek bildirim temelli işaretleme veya kod gerekli sayfaya oturum açma denetimi yalnızca sürükleyerek kullanılabilir. Daha fazla nedir, oturum açma denetimi üst düzeyde bir işlenen kullanıcı arabirimi ve kimlik doğrulama mantığını denetime izin vermeyi yüksek oranda özelleştirilebilir.

Bu noktada, Web sitesi ziyaretçileri yeni kullanıcı hesabı ve günlük siteye oluşturabilirsiniz, ancak kimliği doğrulanmış kullanıcıyı temel alarak sayfalara erişimi kısıtlama adresindeki aramak henüz. Şu anda, herhangi bir kullanıcı kimliği doğrulanmış veya anonim, herhangi bir sayfayı sitemizde görüntüleyebilirsiniz. Bir kullanıcı tarafından temelinde bizim sitenin sayfalarına erişimi denetleme yanı sıra, işlevsellik kullanıcıya bağlıdır belirli sayfalarını olabilir. Erişim ve oturum açan kullanıcıyı temel alarak sayfa işlevselliği sınırlamak nasıl sonraki öğretici bakar.

Mutluluk programlama!

### <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Kilitli ve onaylanmamış kullanıcılar için özel iletilerin görüntüleme](http://aspnet.4guysfromrolla.com/articles/050306-1.aspx)
- [ASP.NET 2.0'ın inceleniyor üyelik, roller ve profil](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Nasıl yapılır: ASP.NET oturum açma sayfası oluşturma](https://msdn.microsoft.com/library/ms178331.aspx)
- [Oturum açma denetimi teknik belgeler](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.aspx)
- [Oturum açma denetimleri kullanma](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/security/login.aspx)

### <a name="about-the-author"></a>Yazar hakkında

Scott Mitchell, birden çok ASP/ASP.NET books yazar ve 4GuysFromRolla.com, kurucusu 1998 itibaren Microsoft Web teknolojileri ile çalışmaktadır. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri  *[kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Tan adresindeki ulaşılabilir [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) veya kendi blog aracılığıyla [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. Bu öğretici için sağlama gözden geçirenler Teresa Murphy ve Michael Olivero yoktu. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bana bir satırında bırakma [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Önceki](creating-user-accounts-vb.md)
> [sonraki](user-based-authorization-vb.md)
