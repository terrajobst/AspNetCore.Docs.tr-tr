---
uid: web-pages/overview/security/16-adding-security-and-membership
title: ASP.NET Web için güvenlik ve üyelik ekleme sayfaları (Razor) sitesinde | Microsoft Docs
author: tfitzmac
description: Bu bölümde Web sitenizi sayfalardan bazıları yalnızca oturum açan kişinin için kullanılabilir olacak şekilde güvenliğinin nasıl sağlanacağını gösterir. (Ayrıca sayfaları tha oluşturma görürsünüz
ms.author: riande
ms.date: 02/24/2014
ms.assetid: 7a77c2c0-deea-4290-a9c3-97958891758e
msc.legacyurl: /web-pages/overview/security/16-adding-security-and-membership
msc.type: authoredcontent
ms.openlocfilehash: ae574706ecd14f1cafdb2d8b6340477e50246a32
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754134"
---
<a name="adding-security-and-membership-to-an-aspnet-web-pages-razor-site"></a>Bir ASP.NET Web sayfaları (Razor) sitesinde için güvenlik ve üyelik ekleme
====================
tarafından [Tom FitzMacken](https://github.com/tfitzmac)

> Bu makalede, sayfalardan bazıları yalnızca oturum açan kişinin için kullanılabilir olacak şekilde bir ASP.NET Web sayfaları (Razor) Web sitesini güvenli hale getirmek açıklanmaktadır. (Ayrıca, herkesin erişebileceği sayfaları oluşturmak nasıl görürsünüz.)
> 
> **Öğrenecekleriniz:** 
> 
> - Bazı sayfaları için yalnızca üyelerine erişimi sınırlayabilir. böylece kayıt sayfasına ve oturum açma sayfasına sahip bir Web sitesi oluşturma
> - Genel ve yalnızca üye sayfaları oluşturma
> - Sitenizde farklı güvenlik izni olan grupları olan rolleri tanımlama ve kullanıcıları bir role atama.
> - CAPTCHA otomatik programların (botlar) hesapları üye oluşturmasını önlemek için kullanma
>   
> 
> Bu makalede sunulan ASP.NET özellikleri şunlardır:
> 
> - WebMatrix **başlangıç sitesi** şablonu.
> - `WebSecurity` Yardımcısı ve `Roles` sınıfı.
> - `ReCaptcha` Yardımcısı.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Bu öğreticide kullanılan yazılım sürümleri
> 
> 
> - ASP.NET Web sayfaları (Razor) 2
> - WebMatrix 3
> - ASP.NET Web Yardımcıları kitaplığı


Sitenizi ayarlayabilirsiniz, böylece kullanıcılar'ın içine oturum &#8212; diğer bir deyişle, böylece sitenin desteklediği *üyelik*. Bu nedenlerle yararlı olabilir. Örneğin, siteniz yalnızca üyeleri için kullanılabilir olması gerektiğini sayfaları olabilir. Bazı durumlarda, kullanıcıların bir yorum yazın ya da geri bildirim göndermek için oturum gerektirebilir.

Web sitenizi üyelik destekler olsa bile, kullanıcıların bazı sayfaları sitesinde kullanmadan önce oturum açmak için mutlaka gerekli değildir. Oturumunuz açık kullanıcılar olarak bilinir *anonim kullanıcılar*.

Bir kullanıcı sitenizde kaydedebilir ve sonra siteye oturum açabilirsiniz. Web sitesi, bir kullanıcı adı (e-posta adresi) ve kullanıcıların iddia kim olduğunu doğrulamak için bir parola gerektirir. Bu işlem, oturum açma ve bir kullanıcının kimliğini onaylayan olarak da bilinen *kimlik doğrulaması*.

Güvenlik ve üyelik farklı şekillerde ayarlayabilirsiniz:

- WebMatrix kullanıyorsanız, yeni site dayalı olarak oluşturmak için kolay bir yolu olan **başlangıç sitesi** şablonu. Bu şablon, güvenlik ve üyelik için zaten yapılandırıldı ve bir kayıt sayfasında, oturum açma sayfası ve benzeri zaten sahip.

    Şablon tarafından oluşturulan site Ayrıca, kullanıcıların Facebook, Google gibi dış bir siteye kullanarak oturum açın veya Twitter olanak tanımak için bir seçenek de sahiptir.
- Güvenlik için mevcut bir site eklemek istiyorsanız veya kullanmak istemiyorsanız **başlangıç sitesi** şablonu, kendi kayıt sayfasında, oturum açma sayfası ve benzeri oluşturabilirsiniz.

Bu makalede ilk seçeneğine odaklanmaktadır &mdash; kullanarak güvenlik ekleme **başlangıç sitesi** şablonu. Bu, kendi güvenlik uygulamaları hakkında bazı temel bilgi de sağlar ve ardından bunu nasıl yapacağınız hakkında daha fazla bilgi için bağlantılar sağlar. Daha ayrıntılı olarak ayrı bir makalede açıklanan harici oturum açmaları nasıl etkinleştirileceği hakkında bilgi de mevcuttur.

## <a name="creating-website-security-using-the-starter-site-template"></a>Başlangıç sitesi şablonu kullanarak Web sitesi güvenlik oluşturma

Webmatrix'te, kullandığınız **başlangıç sitesi** aşağıdakileri içeren bir Web sitesi oluşturmak üzere şablonu:

- Üyeleriniz için kullanıcı adları ve parolaları depolamak için kullanılan bir veritabanı.
- (Yeni) anonim kullanıcılar kaydolabilecekleri bir kayıt sayfası.
- Oturum açma ve kapatmanın sayfası.
- Bir parola kurtarma ve sıfırlama sayfası.

Aşağıdaki yordam, siteyi oluşturmak ve yapılandırmak açıklar.

1. WebMatrix başlatmak ve **Hızlı Başlangıç** sayfasında **Site şablondan**.
2. Seçin **başlangıç sitesi** şablonu ve ardından **Tamam**. WebMatrix yeni bir site oluşturur.
3. Sol bölmede **dosyaları** çalışma alanı Seçici.
4. Web sitenizin kök klasörü açın  *\_AppStart.cshtml* dosyasını genel ayarları içermesi için kullanılan özel bir dosyadır. Kullanılarak geçersiz kılınan bazı deyimleri içeren `//` karakter:

    [!code-csharp[Main](16-adding-security-and-membership/samples/sample1.cs)]

    Bu deyimler yapılandırma `WebMail` Yardımcısı, e-posta göndermek için kullanılabilir. Üyelik Sistemi kullanıcılar kaydederken veya kullanıcılardan parolalarını istedikleri zaman onay mesajları göndermek için e-posta kullanabilirsiniz. (Kullanıcıların kaydettikten sonra Örneğin, kullanıcılar kayıt işlemini tamamlamak için tıklayabileceği bağlantısını içeren bir e-posta alın.)

    E-posta gönderme gerektiren bir SMTP sunucusuna erişim açıklandığı [ekleme e-posta bir ASP.NET Web sayfaları sitesinde](https://go.microsoft.com/fwlink/?LinkId=202899). Bu orta içinde e-posta ayarlarını depolayacak  *\_AppStart.cshtml* bunları art arda her sayfasına e-posta göndermek için kod gerekmez dosyasını. (Bir kayıt veritabanı ayarlamak için SMTP ayarlarıyla yapılandırmanıza gerek yoktur; yalnızca kendi e-posta diğer kullanıcılardan doğrulamak ve kullanıcıların Unutulan parolayı sıfırlamak istiyorsanız SMTP ayarlarını gerekir.)
5. Deyimleri kaldırarak metindeki açıklamayı silin `//` from in front of her biri.

    E-posta onayı ayarlama istemiyorsanız, bu adımı ve sonraki adıma atlayabilirsiniz. SMTP değerleri ayarlanmamışsa, yeni hesabı bir onay e-posta hemen kullanılabilir.
6. Kod aşağıdaki e-postayla ilgili ayarları değiştirin:

   - Ayarlama `WebMail.SmtpServer` erişiminiz SMTP sunucusunun adı.
   - Bırakın `WebMail.EnableSsl` kümesine `true`. Bu ayar şifreleyerek SMTP sunucusuna gönderilen kimlik bilgilerini korur.
   - Ayarlama `WebMail.UserName` , SMTP sunucusu hesabının kullanıcı adı.
   - Ayarlama `WebMail.Password` , SMTP sunucusu hesabının parolasına.
   - Ayarlama `WebMail.From` kendi e-posta adresi. İletinin gönderildiği e-posta adresidir.

     > [!NOTE] 
     > 
     > **İpucu** bu özelliklerin değerlerini hakkında ek bilgi için bkz: [e-posta ayarlarını yapılandırma](https://go.microsoft.com/fwlink/?LinkID=202906#configuring_email_settings) içinde [Site genelinde davranışı ASP.NET Web sayfaları için özelleştirme](https://go.microsoft.com/fwlink/?LinkID=202906).
7. Kaydet ve Kapat  *\_AppStart.cshtml*.
8. Çalıştırma *Default.cshtml* sayfasını bir tarayıcıda.

    ![Güvenlik üyelik 2](16-adding-security-and-membership/_static/image1.png)

   > [!NOTE]
   > Örneği bir özellik olması gerektiğini söyleyen bir hata görürseniz `ExtendedMembershipProvider`, sitenin ASP.NET Web Pages üyelik sisteminin (SimpleMembership) kullanmak için yapılandırılmamış olabilir. Bu, bazen bir barındırma sağlayıcısının sunucusu yerel sunucunuzun farklı şekilde yapılandırıldıysa oluşabilir. Bu sorunu gidermek için sitenin şu öğeyi ekleyin *Web.config* dosyası:
   > 
   > [!code-xml[Main](16-adding-security-and-membership/samples/sample2.xml)]
   > 
   > Bu öğenin alt öğesi olarak ekleyin `<configuration>` öğesi ve eşi `<system.web>` öğesi.
9. Sayfanın sağ üst köşesinde bulunan tıklayın **kaydetme** bağlantı. *Register.cshtml* sayfası görüntülenir.
10. Bir kullanıcı adı ve parola girin ve ardından **kaydetme**.

    ![Güvenlik üyelik 3](16-adding-security-and-membership/_static/image2.png)

    Web sitesinden oluştururken **başlangıç sitesi** şablonu, adlı bir veritabanı *StarterSite.sdf* sitenin içinde oluşturulmuş *uygulama\_veri* klasör. Kayıt sırasında kullanıcı bilgilerinizi veritabanına eklenir. SMTP değerleri ayarlarsanız, bir ileti kaydetme tamamlamak için kullandığınız e-posta adresine gönderilir.

    ![Güvenlik üyelik 4](16-adding-security-and-membership/_static/image3.png)
11. E-posta programınızı gidin ve onaylama kodunuz ve köprü siteye olacaktır iletisi bulunamadı.
12. Hesabınızı etkinleştirmek için köprüye tıklayın. Onay köprü kayıt onay sayfası açılır.

    ![Güvenlik üyelik 5](16-adding-security-and-membership/_static/image4.png)
13. Tıklayın **oturum açma** bağlantısını ve ardından kaydettiğiniz hesap kullanarak oturum açın.

      ' De oturum açtıktan sonra **oturum açma** ve **kaydetme** bağlantıları almıştır bir **oturum kapatma** bağlantı. Oturum açma adınız bir bağlantı olarak görüntülenir. (Bağlantı, parolanızı değiştirebileceğiniz bir sayfaya gitmek izin verir.)

      ![Güvenlik üyelik 6](16-adding-security-and-membership/_static/image5.png)

      > [!NOTE]
      > Varsayılan olarak, ASP.NET web sayfaları kimlik bilgilerini sunucuya düz metin olarak (kullanıcı tarafından okunabilen metin olarak) gönderin. Üretim sitesini Güvenli HTTP kullanmanız gerekir (https:// olarak da bilinen, *Güvenli Yuva Katmanı* veya SSL) sunucuyla paylaşılan hassas bilgileri şifrelemek için. Gerekli e-posta için gönderilecek iletilerin ayarlayarak SSL kullanarak `WebMail.EnableSsl=true` önceki örnekte olduğu gibi. SSL hakkında daha fazla bilgi için bkz: [Web iletişimi güvenli hale getirme: sertifikaları, SSL ve https://](https://go.microsoft.com/fwlink/?LinkId=208660).

## <a name="additional-membership-functionality-in-the-site"></a>Sitedeki ek üyelik işlevselliğini

Site hesaplarını yönetme olanağı diğer işlevleri içerir. Kullanıcılar aşağıdakileri yapabilirsiniz:

- Kullanıcıların parolalarını değiştirin. Oturum açtıktan sonra bunlar bir bağlantıdır) kullanıcı adı (tıklayabilirsiniz. Bu sayfaya burada yeni bir parola oluşturabilirsiniz götüren (*Account/ChangePassword.cshtml*).
- Unutulan parolayı kurtarın. Oturum açma sayfasında bir bağlantı yoktur (**parolanızı unuttunuz mu?**), kullanıcıları bir sayfaya götürür (*Account/ForgotPassword.cshtml*) nerede bunlar girebilirsiniz bir e-posta adresi. Site bunları yeni bir parola ayarlamak için tıklatabileceği bağlantı içeren bir e-posta iletisi gönderir (*Account/PasswordReset.cshtml*).

Ayrıca, daha sonra açıklandığı gibi dış bir siteye kullanarak oturum Ayrıca kullanıcılar da sağlayabilirsiniz.

<a id="Creating_a_Members-Only_Page"></a>
## <a name="creating-a-members-only-page"></a>Yalnızca üyelere özgü sayfası oluşturma

Şimdilik, herkesin Web sitenizi herhangi bir sayfaya göz atabilirsiniz. Ancak, yalnızca oturum kişilere kullanılabilir sayfa isteyebilirsiniz (diğer bir deyişle, üyeleri için). ASP.NET yalnızca oturum açmış üyeleri tarafından erişilebilen sayfa oluşturmanıza olanak sağlar. Anonim kullanıcılar yalnızca üyelere özgü bir sayfaya erişmeye çalışırsanız, genellikle, bunları oturum açma sayfasına yönlendir.

Bu yordamda, yalnızca oturum açan kullanıcılar için kullanılabilir olan sayfaları içerecek bir klasörün oluşturacaksınız.

1. Bir site kökünde, yeni bir klasör oluşturun. (Şeritte altındaki oka **yeni** seçip **yeni klasör**.)
2. Yeni klasör adı *üyeleri*.
3. İçinde *üyeleri* klasöründe yeni bir sayfa oluşturun ve ona *MembersInformation.cshtml*.
4. Var olan içerik, aşağıdaki kod ve İşaretleme ile değiştirin:

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample3.cshtml)]

    Bu kodu test `IsAuthenticated` özelliği `WebSecurity` döndüren nesne `true` kullanıcı açmışsa. Kod çağrıları, kullanıcının oturum açmadığı varsa `Response.Redirect` kullanıcıya gönderilecek *Login.cshtml* sayfasını *hesabı* klasör.

    Yeniden yönlendirme URL'sini içeren bir `returnUrl` sorgusu kullanan bir dize değerini `Request.Url.LocalPath` geçerli sayfasının yolu. Ayarlarsanız `returnUrl` sorgu dizesi değerini (ve dönüş URL'si yerel bir yol ise), oturum açtıktan sonra oturum açma sayfasına bu sayfaya kullanıcının döneceğini gösterir.

    Kod ayrıca ayarlar  *\_SiteLayout.cshtml* sayfası olarak, düzen sayfası. (Düzen sayfaları hakkında daha fazla bilgi için bkz. [ASP.NET Web Pages sitelerinde tutarlı bir düzen oluşturma](https://go.microsoft.com/fwlink/?LinkId=202891).)
5. Siteyi çalıştırın. Hala oturum tıklayın **oturum kapatma** sayfanın üstünde düğme.
6. Tarayıcıda, sayfa istek */üyeler/MembersInformation*. Örneğin, URL şuna benzeyebilir:

    `http://localhost:38366/Members/MembersInformation`

    (Bağlantı noktası numarası (38366) büyük olasılıkla, URL'de farklı olacaktır.)

    İçin yönlendirilirsiniz *Login.cshtml* günlüğe değildir çünkü sayfa.
7. Daha önce oluşturduğunuz hesabı kullanarak oturum açın. Geri yönlendirilirsiniz *MembersInformation* sayfası. Bu kez oturum açtınız dolayı sayfa içeriğini görebilirsiniz.

Birden çok sayfa güvenli şekilde bunu yapabilirsiniz:

- Güvenlik denetimi her sayfaya ekleyin.
- Oluşturma bir  *\_PageStart.cshtml* sayfa klasöründe korumalı sayfaları tutmak ve güvenlik denetimi ekleyebileceğiniz yerdir. *\_PageStart.cshtml* sayfası genel sayfa klasördeki tüm sayfalar için bir tür olarak görev yapar. Bu teknik, daha ayrıntılı olarak açıklandığı [Site genelinde davranışı ASP.NET Web sayfaları için özelleştirme](https://go.microsoft.com/fwlink/?LinkId=202906#Using__PageStart.cshtml_to_Restrict_Folder_Access).

## <a name="creating-security-for-groups-of-users-roles"></a>(Roller) kullanıcı grupları için güvenlik oluşturma

Sitenizde çok sayıda üye varsa, bunları bir sayfa görmeniz yapmasına izin vermeden önce her kullanıcı için izni ayrı ayrı kontrol etmek için etkin değil. Neler yapabileceğinizi grupları oluşturmak için bunun yerine olduğu veya *rolleri*, tek tek üyeleri için ait. Ardından, rol tabanlı izinleri kontrol edebilirsiniz. Bu bölümde, oluşturacağınız bir &quot;yönetici&quot; rol ve ardından olan kullanıcılar için erişilebilir olan bir sayfa oluşturun (kimin ait) içinde bu rolü.

ASP.NET üyelik sistemi rollerini destekleyecek şekilde ayarlanır. Ancak, üyelik kayıt ve oturum açma, aksine **başlangıç sitesi** şablon içermiyor yardımcı olan sayfaları rolleri yönetin. (Rollerini yönetme bir yönetim görevini yerine bir kullanıcı görevini içindir.) Ancak WebMatrix üyelik veritabanında doğrudan grupları ekleyebilirsiniz.

1. Webmatrix'te, tıklayın **veritabanları** çalışma alanı Seçici.
2. Sol bölmede açın *StarterSite.sdf* düğümü, açık **tabloları** düğüm gittikten sonra çift tıklayarak *Web sayfalarını\_rolleri* tablo.

    ![Güvenlik üyelik 7](16-adding-security-and-membership/_static/image6.png)
3. Adlı bir rol eklemek &quot;yönetici&quot;. *Roleıd* alan doldurulur otomatik olarak. (Birincil anahtar ve bir kimlik alanı olarak açıklandığı şekilde ayarlandı [ASP.NET Web sayfaları sitelerinde bir veritabanıyla çalışmaya giriş](https://go.microsoft.com/fwlink/?LinkId=202893).)
4. İçin değer olarak Not *Roleıd* alan. (Bu, tanımlama ilk rol ise, bu 1 olacaktır.)

    ![Güvenlik üyelik 8](16-adding-security-and-membership/_static/image7.png)
5. Kapat *Web sayfalarını\_rolleri* tablo.
6. Açık *USERPROFILE* tablo.
7. Not *UserID* bir veya daha sonra close tablosu ve tablo kullanıcılar değeri.
8. Açık *Web sayfalarını\_UserInRoles* girin ve tablo bir *UserID* ve *Roleıd* tabloya değeri. Örneğin, kullanıcı 2 içine yerleştirmek için &quot;yönetici&quot; rolü, bu değerleri girebilirsiniz:

    ![Güvenlik üyelik 9](16-adding-security-and-membership/_static/image8.png)
9. Kapat *Web sayfalarını\_UsersInRoles* tablo.

    Tanımlı rollere sahip olduğunuza göre Bu roldeki kullanıcılar tarafından erişilebilen bir sayfa yapılandırabilirsiniz.
10. Web sitesi kök klasöründe adlı yeni bir sayfa oluşturmak *AdminError.cshtml* ve mevcut içeriğini aşağıdaki kodla değiştirin. Bu sayfaya erişim izin verilmiyorsa kullanıcılar yönlendirilirsiniz sayfa olacaktır.

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample4.cshtml)]
11. Web sitesi kök klasöründe adlı yeni bir sayfa oluşturmak *AdminOnly.cshtml* ve varolan kodu aşağıdaki kodla değiştirin:

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample5.cshtml)]

    `Roles.IsUserInRole` Yöntemi döndürür `true` geçerli kullanıcının belirtilen rolde (Bu durumda, "Yönetici" rolünü) üyesi olup olmadığını.
12. Çalıştırma *Default.cshtml* bir tarayıcıda, ancak oturum yok. (Henüz oturum açmadıysanız, oturumunuzu.)
13. Tarayıcının adres çubuğuna ekleyin *AdminOnly* URL. (Diğer bir deyişle, istek *AdminOnly.cshtml* dosyası.) İçin yönlendirilirsiniz *AdminError.cshtml* sayfasında bir kullanıcı olarak oturum açmış değilseniz çünkü &quot;yönetici&quot; rol.
14. Geri dönüp *Default.cshtml* ve eklediğiniz bir kullanıcı olarak oturum &quot;yönetici&quot; rol.
15. Gözat *AdminOnly.cshtml* sayfası. Şu sayfayı görürsünüz.

## <a name="preventing-automated-programs-from-joining-your-website"></a>Otomatik programları sitenize katılmasını engelleme

Oturum açma sayfasına otomatik programları durdurmaz (bazen denir *web robotları* veya *botlar*) gelen siteniz ile kaydediliyor. Bu yordamda, kayıt sayfası için bir ReCaptcha test etkinleştirmek açıklanmaktadır.

![/media/38777/ch16securitymembership-18.jpg](16-adding-security-and-membership/_static/image1.jpg)

1. ReCaptcha.Net adresindeki Web sitenize kaydetme ([http://recaptcha.net](http://recaptcha.net)). Kaydı tamamladıktan sonra ortak anahtar ve özel anahtarı alırsınız.
2. ASP.NET Web Yardımcıları kitaplığı açıklandığı Web sitenize ekleyin [yükleme Yardımcıları bir ASP.NET Web sayfaları sitesinde](https://go.microsoft.com/fwlink/?LinkId=252372), henüz yapmadıysanız.
3. İçinde *hesabı* dosya adında klasör, açık *Register.cshtml*.
4. Sayfanın üstündeki kodda aşağıdaki satırları bulun ve bunları kaldırarak metindeki açıklamayı silin `//` yorum karakterleri:

    [!code-csharp[Main](16-adding-security-and-membership/samples/sample6.cs)]
5. Değiştirin `PRIVATE_KEY` kendi ReCaptcha özel anahtara sahip.
6. Sayfanın biçimlendirmesine kaldırmak `@*` ve `*@` sayfa biçimlendirmeyi aşağıdaki satırları etrafında yorum karakterleri:

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample7.cshtml)]
7. Değiştirin `PUBLIC_KEY` anahtarınızı.
8. Zaten kaldırılmış yapmadıysanız, kaldırmanız `<div>` "CAPTCHA doğrulamasını etkinleştirmek için" ile başlayan metin içeren öğe. (Tüm kaldırmak `<div>` öğesi ve içeriği.)

9. Çalıştırma *Default.cshtml* bir tarayıcıda. Siteye oturum açmadıysanız, tıklayın **oturum kapatma** bağlantı.
10. Tıklayın **kaydetme** bağlamak ve CAPTCHA test kullanarak kayıt test edin.

     ![Güvenlik üyelik 10](16-adding-security-and-membership/_static/image9.png)

Hakkında daha fazla bilgi için `ReCaptcha` yardımcıyı bkz [bir CATPCHA önlemek otomatik programlar (Botlar) kullanarak ASP.NET Web sitenizin gelen kullanarak](https://go.microsoft.com/fwlink/?LinkId=251967).

<a id="Additional_Resources"></a>
## <a name="letting-users-log-in-using-an-external-site"></a>Kullanıcıların dış bir siteye kullanarak oturum açın

**Başlangıç sitesi** şablon içeren kod ve kullanıcıların sağlayan biçimlendirme Facebook, Windows Live, Twitter, Google ve Yahoo kullanarak oturum açın. Varsayılan olarak, bu işlev etkin değil. Genel yordam kullanıcılara izin vermek için bu dış sağlayıcıları kullanarak oturum bu kullanmaktır:

- Desteklemek istediğiniz, dış sitelerin karar verin.
- Gerekirse, o siteye gidin ve bir oturum açma uygulamasını ayarlama. (Örneğin, Facebook oturum açma bilgileri izin vermek üzere bunu gerekir.)
- Sitenizdeki, sağlayıcı yapılandırın. Çoğu durumda, koda açıklama durumundan çıkarın yeterlidir  *\_AppStart.cshtml* dosya.
- Biçimlendirme sağlayan kişiler kayıt sayfasına ekleyin. oturum açmak için dış Web sitesine bağlantı. Genellikle, gereksinim ve metin biraz değiştirmek biçimlendirme kopyalayabilirsiniz.

Konusundaki adım adım yönergeleri bulabilirsiniz [bir ASP.NET Web sayfaları sitesinde dış sitelerden oturum etkinleştirme](https://go.microsoft.com/fwlink/?LinkId=251969).

Başka bir siteden bir kullanıcı oturum açtığında sonra kullanıcı, siteye geri döndüğünde ve *ilişkilendirir* sitenizde oturum açma. Aslında, bu kullanıcının dış oturum açma için sitenizde bir üyelik girişi oluşturur. Bu üyelik (örneğin, rolleri) normal akreditasyonlu dış oturum açmayla kullanmanıza olanak sağlar.

## <a name="adding-security-to-an-existing-website"></a>Güvenlik mevcut bir Web sitesine ekleme

Bu makaledeki yordamı kullanarak kullanır **başlangıç sitesi** Web güvenlik temeli olarak şablon. Başlatmayı sizin için pratik değilse **başlangıç sitesi** şablonu veya o şablonu temel alan bir site şekilde ilgili sayfaları kopyalamak için aynı güvenlik türünü kendi sitede kendiniz kodlayarak uygulayabilirsiniz. Aynı tür sayfaları oluşturun — kaydı, oturum açma ve benzeri — ve ardından üyeliği ayarlamak için Yardımcıları ve sınıfları kullanın.

Temel işlem blog gönderisinde açıklanan [ASP.NET Razor güvenliği uygulamak için en basit yol](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2240). İşin çoğunu yapılır aşağıdaki yöntemleri ve özellikleri kullanarak `WebSecurity` yardımcı:

- [WebSecurty.UserExists](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.userexists(v=vs.99).aspx), [WebSecurity.CreateUserAndAccount](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.createuserandaccount(v=vs.99).aspx). Bu yöntemlerden birisi zaten kayıtlı olup olmadığını belirlemenize olanak tanır ve bunları kaydetmek için.
- [WebSecurty.IsAuthenticated](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.isauthenticated(v=vs.99).aspx). Bu özellik, geçerli kullanıcının oturumu olup olmadığını belirlemenize olanak tanır. Bu, varsa, bunlar zaten oturum açmamış olan kullanıcılar bir oturum açma sayfasına yeniden yönlendirmek kullanışlıdır.
- [WebSecurity.Login](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.login(v=vs.99).aspx), [WebSecurity.Logout](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.logout(v=vs.99).aspx). Bu yöntemler, bir kullanıcı veya oturum.
- [WebSecurity.CurrentUserName](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.currentusername(v=vs.99).aspx). Bu özellik, oturum açmış geçerli kullanıcı adını (kullanıcının oturum açtıysa) görüntülemek için yararlıdır.
- [WebSecurity.ConfirmAccount](https://msdn.microsoft.com/library/gg569286(v=vs.99).aspx). Kaydı için e-posta onayı ayarladıysanız, bu yöntem kullanışlıdır. (Ayrıntıları blog gönderisinde açıklanan [için ASP.NET Web Pages güvenlik onayı özelliğini kullanarak](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2267).)

Rolleri yönetmek için kullanabileceğiniz [rolleri](https://msdn.microsoft.com/library/gg538398(v=vs.99).aspx) ve [üyelik](https://msdn.microsoft.com/library/gg569035(v=vs.99).aspx) blog girişi açıklandığı gibi sınıflar.

## <a name="additional-resources"></a>Ek Kaynaklar

- [Site Geneline Yönelik Davranışını Özelleştirme](https://go.microsoft.com/fwlink/?LinkId=202906)
- [Web iletişimler güvenliğini sağlama: Sertifikaları, SSL ve https://](https://go.microsoft.com/fwlink/?LinkId=208660)
- [ASP.NET Razor güvenliği uygulamak için en temel yolu](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2240) ve [için ASP.NET Web Pages güvenlik onayı özelliğini kullanarak](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2267). ASP.NET üyelik özellikleri kullanmadan uygulanmasını nasıl açıklayan blog gönderilerini bunlar **başlangıç sitesi** şablonu.
- [Bir ASP.NET Web Sayfaları Sitesinde Dış Sitelerden Oturum Açmayı Etkinleştirme](https://go.microsoft.com/fwlink/?LinkId=251969)
- [WebSecurity sınıfı API Başvurusu](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity(v=vs.99)) (MSDN)
- [SimpleRoleProvider sınıfı API Başvurusu](https://msdn.microsoft.com/library/webmatrix.webdata.simpleroleprovider(v=vs.99)) (MSDN)
- [SimpleMembershipProvider sınıfı API Başvurusu](https://msdn.microsoft.com/library/webmatrix.webdata.simplemembershipprovider(v=vs.99)) (MSDN)
