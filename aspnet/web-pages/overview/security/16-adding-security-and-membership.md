---
uid: web-pages/overview/security/16-adding-security-and-membership
title: "Güvenlik ve üyelik ekleme bir ASP.NET Web sayfaları (Razor) Site | Microsoft Docs"
author: tfitzmac
description: "Bu bölümde bazı sayfalarda oturum açan kişi için kullanılabilir olacak şekilde, Web sitenizin güvenli gösterilmiştir. (Ayrıca sayfaları tha oluşturma görürsünüz"
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/24/2014
ms.topic: article
ms.assetid: 7a77c2c0-deea-4290-a9c3-97958891758e
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/security/16-adding-security-and-membership
msc.type: authoredcontent
ms.openlocfilehash: af2eeb128cff554e7ae3d903e2117861087344e9
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="adding-security-and-membership-to-an-aspnet-web-pages-razor-site"></a>ASP.NET Web sayfaları (Razor) sitesi için güvenlik ve üyelik ekleme
====================
tarafından [zel FitzMacken](https://github.com/tfitzmac)

> Bu makalede, bazı sayfalarda oturum açan kişi için kullanılabilir olacak şekilde, bir ASP.NET Web sayfaları (Razor) Web sitesini güvenli açıklanmaktadır. (Ayrıca herkes erişebilir sayfalarının nasıl oluşturulacağını de görürsünüz.)
> 
> **Öğrenecekleriniz:** 
> 
> - Bazı sayfaları için yalnızca üyelerine erişimi sınırlayabilirsiniz böylece kayıt sayfası ve oturum açma sayfasına sahip bir Web sitesi oluşturma
> - Genel ve yalnızca üye sayfaları oluşturma
> - Sitenizde farklı güvenlik izinlerine sahip gruplar olan rolleri tanımlama ve kullanıcılar için rol atama.
> - CAPTCHA hesapları üye oluşturma otomatik programları (aracılarını) önlemek için nasıl kullanılacağını.
>   
> 
> Bu makalede sunulan ASP.NET özellikleri şunlardır:
> 
> - WebMatrix **başlangıç sitesi** şablonu.
> - `WebSecurity` Yardımcısı ve `Roles` sınıfı.
> - `ReCaptcha` Yardımcısı.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Öğreticide kullanılan yazılım sürümleri
> 
> 
> - ASP.NET Web sayfaları (Razor) 2
> - WebMatrix 3
> - ASP.NET Web Yardımcıları kitaplığı


Sitenizi ayarlayabilirsiniz, böylece kullanıcılar &#8212;oturum açabilir; diğer bir deyişle, böylece sitenin desteklediği *üyelik*. Bu nedenlerle yararlı olabilir. Örneğin, siteniz yalnızca üyelerine kullanılabilir olması gerektiğini sayfaları olabilir. Bazı durumlarda, kullanıcıların bir yorum yazın ya da geri bildirim göndermek için oturum gerektirebilir.

Web sitenizi üyelik destekliyorsa bile, kullanıcılar bazı sayfalarda sitesinde kullanmadan önce oturum açmak için mutlaka gerekli değildir. Oturum olmayan kullanıcılar olarak bilinen *anonim kullanıcılar*.

Bir kullanıcı, Web sitenizde kaydedebilirsiniz ve sonra siteye oturum açabilirsiniz. Web sitesi, bir kullanıcı adı (bir e-posta adresi) ve kullanıcılar kimin bunlar olmasını talep olduğunu doğrulamak için bir parola gerektirir. Bu işlem günlüğü'nde ve bir kullanıcının kimliğini onaylayan olarak bilinir *kimlik doğrulama*.

Güvenlik ve üyelik farklı şekilde ayarlayabilirsiniz:

- WebMatrix kullanıyorsanız, yeni site dayalı olarak oluşturmak için kolay bir yoludur **başlangıç sitesi** şablonu. Bu şablon, güvenlik ve üyelik için zaten yapılandırılmış ve bir kayıt sayfası, oturum açma sayfası ve benzeri zaten sahip.

    Şablon tarafından oluşturulan site kullanıcıların Facebook, Google, gibi dış sitelerden kullanarak oturum açın ve Twitter izin vermek için bir seçenek de vardır.
- Güvenlik için mevcut bir site eklemek istiyorsanız veya kullanmak istemiyorsanız, **başlangıç sitesi** şablonu, kendi kayıt sayfası, oturum açma sayfası ve benzeri oluşturabilirsiniz.

Bu makalede ilk seçeneğini odaklanır &mdash; kullanarak güvenlik ekleme **başlangıç sitesi** şablonu. Ayrıca, kendi güvenlik uygulamaları hakkında bazı temel bilgi sağlar ve bunun hakkında daha fazla bilgi için bağlantılar sağlar. Daha fazla ayrıntı ayrı bir makalede açıklanan dış oturumları nasıl etkinleştirileceği hakkında bilgi yoktur.

## <a name="creating-website-security-using-the-starter-site-template"></a>Web sitesi güvenliği başlangıç sitesi şablonu kullanarak oluşturma

Webmatrix'te, kullandığınız **başlangıç sitesi** aşağıdakileri içeren bir Web sitesi oluşturmak için şablonu:

- Üyeler için kullanıcı adları ve parolaları depolamak için kullanılan bir veritabanı.
- Anonim (yeni) kullanıcılar kaydolabilecekleri kayıt sayfası.
- Oturum açma ve oturum kapatma sayfası.
- Parola kurtarma ve sıfırlama sayfası.

Aşağıdaki yordam, siteyi oluşturmak ve yapılandırmak açıklar.

1. WebMatrix başlatmak ve **Hızlı Başlangıç** sayfasında, **Site şablondan**.
2. Seçin **başlangıç sitesi** şablonu ve ardından **Tamam**. WebMatrix yeni bir site oluşturur.
3. Sol bölmede **dosyaları** çalışma Seçici.
4. Web sitenizin kök klasöründe açın  *\_AppStart.cshtml* genel ayarları içeren için kullanılan özel bir dosyadır dosya. Kullanılarak geçersiz kılınan bazı deyimleri içeren `//` karakterler:

    [!code-csharp[Main](16-adding-security-and-membership/samples/sample1.cs)]

    Bu deyimler yapılandırma `WebMail` e-posta göndermek için kullanılan yardımcı. Üyelik Sistemi kullanıcılar kaydederken veya bunların parolalarını değiştirmek istediğinizde onay mesajları göndermek için e-posta kullanabilirsiniz. (Kullanıcılar kaydettikten sonra Örneğin, kayıt işlemini bitirmek için tıklatabileceği bir bağlantı içeren bir e-posta elde.)

    E-posta gönderme gerektiren bir SMTP sunucusuna erişim açıklandığı gibi [e-posta bir ASP.NET Web sayfaları Site Ekleme](https://go.microsoft.com/fwlink/?LinkId=202899). E-posta ayarları bu Orta depolayacağınız  *\_AppStart.cshtml* bunları sürekli olarak e-posta gönderebilirsiniz her sayfasına kod gerekmez dosyasını. (Kayıt veritabanını ayarlamak için SMTP ayarlarını yapılandırmak gerekmez; e-posta diğer kullanıcılardan doğrulamak ve kullanıcıların unutulmuş parola sıfırlama izin vermek istiyorsanız, SMTP ayarlarını yalnızca gerekir.)
5. Kaldırarak deyimleri açıklamadan çıkarın `//` her biri from in front of.

    E-posta onayı ayarlama istemiyorsanız, bu adımı ve sonraki adıma atlayabilirsiniz. SMTP değerleri ayarlanmamışsa, yeni hesabı bir onay e-postayı hemen kullanılabilir.
6. Kod aşağıdaki e-posta ile ilgili ayarları değiştirin:

    - Ayarlama `WebMail.SmtpServer` erişiminiz SMTP sunucusunun adı.
    - Bırakın `WebMail.EnableSsl` kümesine `true`. Bu ayar şifreleyerek SMTP sunucusuna gönderilen kimlik bilgilerinin güvenliğini sağlar.
    - Ayarlama `WebMail.UserName` SMTP sunucusu hesabının kullanıcı adı.
    - Ayarlama `WebMail.Password` SMTP sunucusu hesabının parolasına.
    - Ayarlama `WebMail.From` kendi e-posta adresi. Bu, ileti gönderilen e-posta adresidir.

    > [!NOTE] 
    > 
    > **İpucu** bu özelliklerin değerlerini hakkında ek bilgi için bkz: [e-posta ayarlarını yapılandırma](https://go.microsoft.com/fwlink/?LinkID=202906#configuring_email_settings) içinde [Site genelinde davranışı ASP.NET Web sayfaları için özelleştirme](https://go.microsoft.com/fwlink/?LinkID=202906).
7. Kaydet ve Kapat  *\_AppStart.cshtml*.
8. Çalıştırma *Default.cshtml* sayfasını bir tarayıcıda.

    ![Güvenlik üyelik 2](16-adding-security-and-membership/_static/image1.png)

    > [!NOTE]
    > Bir özelliğin bir örneği olmalıdır bildiren bir hata görürseniz `ExtendedMembershipProvider`, site ASP.NET Web Pages üyelik sistemini (SimpleMembership) kullanmak üzere yapılandırılmamış olabilir. Bir barındırma sağlayıcısının sunucusu yerel sunucunuz farklı bir biçimde yapılandırılmışsa bu bazen ortaya çıkabilir. Bu sorunu gidermek için aşağıdaki öğeyi sitenin ekleyin *Web.config* dosyası:
    > 
    > [!code-xml[Main](16-adding-security-and-membership/samples/sample2.xml)]
    > 
    > Bu öğenin alt öğesi olarak ekleme `<configuration>` öğesi ve eşi olarak `<system.web>` öğesi.
9. Sayfanın sağ üst köşesinde tıklatın **kaydetmek** bağlantı. *Register.cshtml* sayfası görüntülenir.
10. Bir kullanıcı adı ve parola girin ve ardından **kaydetmek**.

    ![Güvenlik üyelik 3](16-adding-security-and-membership/_static/image2.png)

    Web sitesinden oluşturduğunuzda **başlangıç sitesi** şablonu, adlı bir veritabanı *StarterSite.sdf* sitenin içinde oluşturulduğu *uygulama\_veri* klasör. Kayıt sırasında kullanıcı bilgilerinizi veritabanına eklenir. SMTP değerleri ayarlarsanız, bir ileti kaydolma işlemini tamamlamak için kullandığınız e-posta adresine gönderilir.

    ![Güvenlik üyelik 4](16-adding-security-and-membership/_static/image3.png)
11. E-posta programınızı gidin ve onaylama kodunuz ve köprü siteye sahip olacak ileti bulunamadı.
12. Hesabınızı etkinleştirmek için köprüye tıklayın. Onay köprü bir kayıt onayı sayfası açılır.

    ![Güvenlik üyelik 5](16-adding-security-and-membership/_static/image4.png)
- Tıklatın **oturum açma** bağlamak ve ardından kaydettiğiniz hesap kullanarak oturum açın.

    ' De oturum sonra **oturum açma** ve **kaydetmek** bağlantılar almıştır bir **oturum kapatma** bağlantı. Oturum açma adınız bir bağlantı görüntülenir. (Bağlantı parolanızı değiştirebileceğiniz bir sayfaya gitmek olanak tanır.)

    ![Güvenlik üyelik 6](16-adding-security-and-membership/_static/image5.png)

    > [!NOTE]
    > Varsayılan olarak, ASP.NET web sayfaları kimlik bilgilerini sunucuya düz metin olarak (okunabilir metin olarak) gönderir. Üretim sitesinin Güvenli HTTP kullanması gereken (https:// olarak da bilinen, *Güvenli Yuva Katmanı* veya SSL) sunucuyla paylaşılan hassas bilgileri şifrelemek için. Gerekli e-posta için gönderilecek iletilerin ayarlayarak SSL kullanarak `WebMail.EnableSsl=true` önceki örnekte olduğu gibi. SSL hakkında daha fazla bilgi için bkz: [Web iletişimi güvenli hale getirme: sertifikaları, SSL ve https://](https://go.microsoft.com/fwlink/?LinkId=208660).

## <a name="additional-membership-functionality-in-the-site"></a>Sitedeki ek üyelik işlevselliğini

Sitenizi kullanıcıların hesaplarını yönetme sağlayan diğer işlevlerini içerir. Kullanıcılar aşağıdakileri yapabilirsiniz:

- Kullanıcıların parolalarını değiştirin. Oturum açtıktan sonra bunlar bir bağlantıdır) kullanıcı adı (tıklatabilirsiniz. Bu sayfaya burada yeni bir parola oluşturabilirsiniz götüren (*Account/ChangePassword.cshtml*).
- Unutulan parolayı kurtarın. Oturum açma sayfasında bir bağlantı yok (**parolanızı unuttunuz mu?**), bir sayfaya götüren (*Account/ForgotPassword.cshtml*) burada bunlar girebilirsiniz bir e-posta adresi. Site bunları yeni bir parola ayarlamak için tıklatabileceği bir bağlantı içeren bir e-posta iletisi gönderir (*Account/PasswordReset.cshtml*).

Ayrıca, kullanıcıların bir dış site kullanarak oturum açın, daha sonra açıklandığı gibi da yapabilirsiniz sağlayabilirsiniz.

<a id="Creating_a_Members-Only_Page"></a>
## <a name="creating-a-members-only-page"></a>Yalnızca üyelere sayfası oluşturma

Anda için herkes herhangi bir sayfaya, Web sitenizin göz atabilirsiniz. Ancak, yalnızca oturum açmış kişilere kullanılabilir sayfaları sahip isteyebilirsiniz (diğer bir deyişle, üyelerine). ASP.NET, yalnızca oturum açmış üyeleri tarafından erişilebilen sayfaları oluşturmanızı sağlar. Anonim kullanıcılar yalnızca üyelere sayfa erişmeye çalışırsa, tipik olarak, bunları oturum açma sayfasına yeniden yönlendir.

Bu yordamda, yalnızca oturum açan kullanıcılar için kullanılabilir olan sayfalar içerecek bir klasörün oluşturacaksınız.

1. Sitenin kök dizininde yeni bir klasör oluşturun. (Şeritte altındaki oku **yeni** ve ardından **yeni klasör**.)
2. Yeni bir klasör adı *üyeleri*.
3. İçinde *üyeleri* klasörü, yeni bir sayfa oluşturun ve onu adlı *MembersInformation.cshtml*.
4. Var olan içerik aşağıdaki kodu ve İşaretleme ile değiştirin:

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample3.cshtml)]

    Bu kod testleri `IsAuthenticated` özelliği `WebSecurity` döndürür nesne `true` kullanıcının oturum açtığı durumunda. Kod çağrılarında, kullanıcının oturum açmadığı varsa `Response.Redirect` kullanıcıya gönderilecek *Login.cshtml* sayfasındaki *hesap* klasör.

    Yeniden yönlendirme URL'sini içeren bir `returnUrl` sorgusu kullanır dize değerini `Request.Url.LocalPath` geçerli sayfasının yolu ayarlamak için. Ayarlarsanız `returnUrl` böyle sorgu dizesi değeri (ve dönüş URL'SİNİN bir yerel yol ise), oturum açtıktan sonra oturum açma sayfasına bu sayfaya kullanıcılar döndürür.

    Kod ayrıca ayarlar  *\_SiteLayout.cshtml* sayfa kendi düzen sayfası olarak. (Düzen sayfaları hakkında daha fazla bilgi için bkz: [ASP.NET Web Pages sitelerinde tutarlı bir düzen oluşturma](https://go.microsoft.com/fwlink/?LinkId=202891).)
5. Siteyi çalıştırın. Hala oturum açmadıysanız tıklatın **oturum kapatma** sayfanın üstündeki düğmesi.
6. Tarayıcıda, sayfa istek */üyeleri/MembersInformation*. Örneğin, URL şöyle olabilir:

    `http://localhost:38366/Members/MembersInformation`

    (Bağlantı noktası numarası (38366) büyük olasılıkla, URL'de farklı olacaktır.)

    İçin yönlendirilirsiniz *Login.cshtml* sayfasında, oturum olmayan gerektiğinden.
- Daha önce oluşturduğunuz hesabı kullanarak oturum açın. Geri yönlendirilirsiniz *MembersInformation* sayfası. Oturum açtınız olduğundan, bu süre, sayfa içeriğini görürsünüz.

Birden çok sayfa güvenli şekilde bunu yapabilirsiniz:

- Güvenlik denetimi her sayfasına ekleyin.
- Oluşturma bir  *\_PageStart.cshtml* Burada, korumalı sayfaları tutun ve güvenlik denetimi var. ekleme klasörde sayfa. *\_PageStart.cshtml* sayfa genel sayfa klasördeki tüm sayfalar için bir tür olarak davranır. Bu teknik daha ayrıntılı olarak anlatılmıştır [Site genelinde davranışı ASP.NET Web sayfaları için özelleştirme](https://go.microsoft.com/fwlink/?LinkId=202906#Using__PageStart.cshtml_to_Restrict_Folder_Access).

## <a name="creating-security-for-groups-of-users-roles"></a>Güvenlik grupları (roller) için oluşturma

Sitenizde çok sayıda üye varsa, bunları bir sayfa görürsünüz yapmasına izin vermeden önce her kullanıcı için izni ayrı ayrı denetlemek için etkin değil. Neler yapabileceğiniz grupları oluşturmak için bunun yerine olduğu veya *rolleri*, tek tek üyeleri için ait. Ardından rolüne dayalı izinleri kontrol edebilirsiniz. Bu bölümde, oluşturacağınız bir &quot;yönetici&quot; rolü ve kullanıcılar için erişilebilir olan bir sayfa oluşturma (kimin ait), bu rol.

ASP.NET üyelik sistemini rolleri desteklemek üzere ayarlanmış. Ancak, üyelik kayıt ve oturum açma, aksine **başlangıç sitesi** şablonu içermiyor yardımcı sayfalarını rolleri yönetme. (Rollerini yönetme kullanıcı görevi yerine bir yönetim görevi olur.) Ancak, WebMatrix üyelik veritabanında doğrudan grupları ekleyebilirsiniz.

1. Webmatrix'te tıklatın **veritabanları** çalışma Seçici.
2. Sol bölmede açmak *StarterSite.sdf* düğümü, açık **tabloları** düğümü ve çift tıklatarak *Web sayfalarını\_rolleri* tablo.

    ![Güvenlik üyelik 7](16-adding-security-and-membership/_static/image6.png)
3. Adlı bir rol eklemek &quot;yönetici&quot;. *Roleıd* alanı doldurulur otomatik olarak. (Birincil anahtar ve bir tanımla alanı olmasını açıklandığı şekilde ayarlanmış [ASP.NET Web sayfaları sitelerdeki veritabanı ile çalışmaya giriş](https://go.microsoft.com/fwlink/?LinkId=202893).)
4. Değeri için kullanılır, Not *Roleıd* alan. (Bu, tanımladığınız ilk rol ise, onu 1 olacaktır.)

    ![Güvenlik üyelik 8](16-adding-security-and-membership/_static/image7.png)
5. Kapat *Web sayfalarını\_rolleri* tablo.
6. Açık *USERPROFILE* tablo.
7. Not *UserID* bir veya daha sonra Kapat tablosu ve tablo kullanıcılar değeri.
8. Açık *Web sayfalarını\_UserInRoles* tablo ve girin bir *UserID* ve *Roleıd* tabloya değeri. Örneğin, kullanıcı 2 içine yerleştirilecek &quot;yönetici&quot; rolü, bu değerler girmeniz gerekir:

    ![Güvenlik üyelik 9](16-adding-security-and-membership/_static/image8.png)
9. Kapat *Web sayfalarını\_UsersInRoles* tablo.

    Tanımlı rollere sahip olduğunuza göre Bu roldeki kullanıcılar için erişilebilir olan bir sayfa yapılandırabilirsiniz.
10. Web sitesi kök klasöründe adlı yeni bir sayfa oluşturma *AdminError.cshtml* ve varolan içeriğini aşağıdaki kodla değiştirin. Bu sayfaya erişim izin verilmeyen kullanıcılar için yönlendirilir sayfası olacaktır.

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample4.cshtml)]
11. Web sitesi kök klasöründe adlı yeni bir sayfa oluşturma *AdminOnly.cshtml* ve var olan kodu aşağıdaki kodla değiştirin:

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample5.cshtml)]

    `Roles.IsUserInRole` Yöntemi döndürür `true` geçerli kullanıcının belirtilen rolde (Bu durumda, "Yönetici" rolünü) üyesi olup olmadığı.
12. Çalıştırma *Default.cshtml* bir tarayıcıda, ancak oturum yok. (Zaten oturum açmadıysanız, oturumu kapatın.)
13. Tarayıcının adres çubuğunda eklemek *AdminOnly* URL. (Diğer bir deyişle, istek *AdminOnly.cshtml* dosyası.) İçin yönlendirilirsiniz *AdminError.cshtml* sayfasında, bir kullanıcı olarak oturum açmış değil çünkü &quot;yönetici&quot; rol.
14. Geri dönüp *Default.cshtml* ve eklediğiniz kullanıcı olarak oturum açma &quot;yönetici&quot; rol.
15. Gözat *AdminOnly.cshtml* sayfası. Bu süre sayfasına bakın.

## <a name="preventing-automated-programs-from-joining-your-website"></a>Web sitenizi birleştirme otomatik programları engelleyerek

Oturum açma sayfasına otomatik programları durdurmaz (bazen denir *web robots* veya *aracılarını*) ile Web sitenizi kaydetme gelen. Bu yordamı, kayıt sayfası için bir ReCaptcha testi etkinleştirmeyi açıklar.

![/media/38777/ch16securitymembership-18.jpg](16-adding-security-and-membership/_static/image1.jpg)

1. Web sitenizi ReCaptcha.Net adresindeki kayıt ([http://recaptcha.net](http://recaptcha.net)). Kayıt tamamladıktan sonra bir ortak anahtar ve özel anahtarı elde edersiniz.
2. ASP.NET Web Yardımcıları kitaplığı açıklandığı gibi Web sitenize ekleyin [yükleme Yardımcıları bir ASP.NET Web Pages sitesinde](https://go.microsoft.com/fwlink/?LinkId=252372), henüz yapmadıysanız.
3. İçinde *hesap* dosya adlı klasörü, açık *Register.cshtml*.
4. Sayfanın üstündeki kodda aşağıdaki satırları bulun ve kaldırarak açıklamadan çıkarın `//` açıklama karakterler:

    [!code-csharp[Main](16-adding-security-and-membership/samples/sample6.cs)]
5. Değiştir `PRIVATE_KEY` kendi ReCaptcha özel anahtara sahip.
6. Sayfa Biçimlendirme kaldırmak `@*` ve `*@` sayfası biçimlendirme aşağıdaki satırları geçici yorum karakterlerinden:

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample7.cshtml)]
7. Değiştir `PUBLIC_KEY` anahtarınız ile.
8. Zaten kaldırılmış yapmadıysanız, kaldırmanız `<div>` "CAPTCHA doğrulamasını etkinleştirmek için" ile başlayan metin içeren öğe. (Tamamını `<div>` öğesi ve içeriği.)

1. Çalıştırma *Default.cshtml* bir tarayıcıda. Siteye oturum açtınız tıklatmak **oturum kapatma** bağlantı.
2. Tıklatın **kaydetmek** kullanarak CAPTCHA test kayıt test ve bağlama.

    ![Güvenlik üyelik 10](16-adding-security-and-membership/_static/image9.png)

Hakkında daha fazla bilgi için `ReCaptcha` Yardımcısı, bkz: [bir CATPCHA önlemek otomatik programlar (aracılarını) kullanarak bilgisayarınızı ASP.NET Web sitesinden kullanarak](https://go.microsoft.com/fwlink/?LinkId=251967).

<a id="Additional_Resources"></a>
## <a name="letting-users-log-in-using-an-external-site"></a>Kullanıcıların dış sitelerden kullanarak oturum açın

**Başlangıç sitesi** şablonu içerir kod ve kullanıcıların sağlayan biçimlendirme Facebook, Windows Live, Twitter, Google veya Yahoo kullanarak oturum açın. Varsayılan olarak, bu işlev etkin değil. Genel yordam veren kullanıcılar için bu dış sağlayıcıları kullanarak oturum açın bu kullanmaktır:

- Dış sitelere desteklemek istediğiniz karar verebilirsiniz.
- Gerekli olursa, o siteye gidin ve bir oturum açma uygulama ayarlama. (Örneğin, bu Facebook oturum açma bilgileri izin vermek üzere yapmanız gereken.)
- Sitenizdeki, sağlayıcı yapılandırın. Çoğu durumda, yalnızca bazı kodda açıklamadan çıkarın gerekir  *\_AppStart.cshtml* dosya.
- Biçimlendirme kişiler sağlayan kayıt sayfasına ekleme oturum açma için dış siteye bağlantı. Genellikle gerekir ve metin biraz değiştirmek biçimlendirmesi kopyalayabilirsiniz.

Konusundaki adım adım yönergeler bulabilirsiniz [etkinleştirme oturum açma bir ASP.NET Web Pages sitesinde dış sitelerden](https://go.microsoft.com/fwlink/?LinkId=251969).

Başka bir siteden bir kullanıcı oturum açtığında sonra kullanıcı, sitenize döndürür ve *ilişkilendirir* sitenizle bu oturum açma. Gerçekte, bu kullanıcının dış oturum açma için sitenizdeki bir üyelik girişi oluşturur. Bu, dış oturum açmayla normal tesis üyelik (örneğin, roller) kullanmanızı sağlar.

## <a name="adding-security-to-an-existing-website"></a>Mevcut bir Web sitesine güvenlik ekleme

Bu makalenin yordamı kullanmaya dayalıdır **başlangıç sitesi** şablon Web sitesi güvenliği için temel olarak. Başlangıç size pratik değilse **başlangıç sitesi** şablonu veya bu şablonu temel alan bir site olacak şekilde ilgili sayfaları kopyalamak için güvenlik aynı türde kendi sitede kendiniz kodlayarak uygulayabilirsiniz. Aynı tür sayfaları oluşturma — kaydı, oturum açma vb. — ve ardından üyelik ayarlamak için Yardımcıları ve sınıfları kullanın.

Temel işlem blog gönderisinde açıklandığı [ASP.NET Razor güvenlik uygulamak için en temel yolu](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2240). İşlerin çoğunu yapılır aşağıdaki yöntemleri ve özellikleri kullanarak `WebSecurity` Yardımcısı:

- [WebSecurty.UserExists](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.userexists(v=vs.99).aspx), [WebSecurity.CreateUserAndAccount](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.createuserandaccount(v=vs.99).aspx). Bu yöntemler birisi zaten kayıtlı olup olmadığını belirlemenize olanak tanır ve bunları kaydetmek için.
- [WebSecurty.IsAuthenticated](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.isauthenticated(v=vs.99).aspx). Bu özellik, geçerli kullanıcının oturumu olup olmadığını belirlemenize olanak sağlar. Bunlar zaten oturum açmamış olan, kullanıcılar bir oturum açma sayfasına yeniden yönlendirmek kullanışlıdır.
- [WebSecurity.Login](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.login(v=vs.99).aspx), [WebSecurity.Logout](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.logout(v=vs.99).aspx). Bu yöntemler bir kullanıcı veya oturum açın.
- [WebSecurity.CurrentUserName](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.currentusername(v=vs.99).aspx). Bu özellik, geçerli kullanıcının oturum açma adı (kullanıcı oturum açtıysa) görüntülemek için yararlıdır.
- [WebSecurity.ConfirmAccount](https://msdn.microsoft.com/library/gg569286(v=vs.99).aspx). Kaydı için e-posta onayı ayarladıysanız, bu yöntem kullanışlıdır. (Ayrıntıları blog gönderisinde açıklandığı [için ASP.NET Web sayfaları güvenlik onayı özelliğini kullanarak](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2267).)

Rolleri yönetmek için kullanabileceğiniz [rolleri](https://msdn.microsoft.com/library/gg538398(v=vs.99).aspx) ve [üyelik](https://msdn.microsoft.com/library/gg569035(v=vs.99).aspx) sınıfları, blog girişi açıklandığı gibi.

## <a name="additional-resources"></a>Ek Kaynaklar

- [Site Geneline Yönelik Davranışını Özelleştirme](https://go.microsoft.com/fwlink/?LinkId=202906)
- [Güvenliğini sağlama Web iletişimler: Sertifikaları, SSL ve https://](https://go.microsoft.com/fwlink/?LinkId=208660)
- [ASP.NET Razor güvenlik uygulamak için en temel yolu](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2240) ve [için ASP.NET Web sayfaları güvenlik onayı özelliğini kullanarak](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2267). ASP.NET üyelik özellikleri kullanmadan uygulanmasını açıklar blog gönderileri bunlar **başlangıç sitesi** şablonu.
- [Bir ASP.NET Web Sayfaları Sitesinde Dış Sitelerden Oturum Açmayı Etkinleştirme](https://go.microsoft.com/fwlink/?LinkId=251969)
- [WebSecurity sınıfı API Başvurusu](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity(v=vs.99)) (MSDN)
- [SimpleRoleProvider sınıfı API Başvurusu](https://msdn.microsoft.com/library/webmatrix.webdata.simpleroleprovider(v=vs.99)) (MSDN)
- [SimpleMembershipProvider sınıfı API Başvurusu](https://msdn.microsoft.com/library/webmatrix.webdata.simplemembershipprovider(v=vs.99)) (MSDN)
