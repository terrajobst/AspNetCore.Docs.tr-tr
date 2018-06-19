---
uid: web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
title: Bir ASP.NET Web dış sitelere kullanarak oturum sayfaları (Razor) Site | Microsoft Docs
author: tfitzmac
description: Bu makalede, Facebook, Google, Twitter, Yahoo ve diğer siteleri kullanarak ASP.NET Web sayfaları (Razor) sitenize oturum açmak açıklanmaktadır — diğer bir deyişle, desteklemek nasıl...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/21/2014
ms.topic: article
ms.assetid: ef852096-a5bf-47b3-9945-125cde065093
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 47d15686194b15b7b06a99d63125c19a41f91ed9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
ms.locfileid: "26573357"
---
<a name="logging-in-using-external-sites-in-an-aspnet-web-pages-razor-site"></a>Bir ASP.NET Web sayfaları (Razor) sitede dış sitelere kullanarak günlüğe kaydetme
====================
tarafından [zel FitzMacken](https://github.com/tfitzmac)

> Bu makalede, Facebook, Google, Twitter, Yahoo ve diğer siteleri kullanarak ASP.NET Web sayfaları (Razor) sitenize oturum açmak açıklanmaktadır — diğer bir deyişle, OAuth ve Openıd sitenizdeki destekleme.
> 
> Öğrenecekleriniz:
> 
> - WebMatrix başlangıç sitesi şablonu kullandığınızda diğer sitelerdeki oturum açmayı etkinleştirme yapma.
> 
> Bu makalede sunulan ASP.NET özelliğidir:
> 
> - `OAuthWebSecurity` Yardımcısı.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Öğreticide kullanılan yazılım sürümleri
> 
> 
> - ASP.NET Web sayfaları (Razor) 2
> - WebMatrix 3

ASP.NET Web sayfaları için destek içerir [OAuth](http://oauth.net/) ve [Openıd](http://openid.net/) sağlayıcıları. Bu sağlayıcılar kullanarak, sitenizi Facebook, Twitter, Microsoft ve Google varolan kimlik bilgilerini kullanarak içine kullanıcılar oturum sağlayabilirsiniz. Örneğin, bir Facebook hesabı kullanarak oturum açın, kullanıcılar yalnızca kendi kullanıcı bilgilerinin nerede girmeleri Facebook oturum açma sayfasına yönlendiren bir Facebook simgesi seçebilirsiniz. Bunlar daha sonra Facebook oturum açma hesabıyla sitenizdeki ilişkilendirebilirsiniz. Bir ilişkili Web Pages üyelik özelliklerine kullanıcıları (sosyal ağ sitelerinde oturum açmayı dahil) çoklu oturum açma bilgileri ilişkilendirebilirsiniz Web sitenizde tek bir hesap yeniliktir.

Bu görüntü oturum açma sayfasından gösterir **başlangıç sitesi** şablonu, bir kullanıcı bir dış hesapla günlük kaydını etkinleştirmek için bir Facebook, Twitter, Google veya Microsoft simgesi seçim burada:

![Dış sağlayıcıları](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image1.png)

Birkaç satır kod uncommenting tarafından OAuth ve Openıd üyelik etkinleştirebilirsiniz **başlangıç sitesi** şablonu. Özellikler ve yöntemler OAuth çalışmak için kullanın ve Openıd sağlayıcılarının olan `WebMatrix.Security.OAuthWebSecurity` sınıfı. **Başlangıç sitesi** şablonu tam üyelik altyapı içerir, kullanıcıların oturum yerel kimlik bilgileri veya bunları başka bir siteden kullanarak sitenizi uygulamasına izin vermeniz gerekiyor bir oturum açma sayfası, üyelik veritabanının ve tüm kodu ile tamamlandı .

Bu bölümde temel alan bir siteye dış sitelerden oturum açmasına izin vermek nasıl bir örnek **başlangıç sitesi** şablonu. Başlangıç sitesi oluşturduktan sonra bu (ayrıntıları izleyin) yapın:

- Bir OAuth sağlayıcısı (Facebook, Twitter ve Microsoft) kullanan siteleri için dış sitesinde bir uygulama oluşturun. Bu sitelere yönelik oturum açma özelliği çağırmak için gerekir uygulama anahtarları sağlar.
- Bir Openıd sağlayıcısı (Google) kullanan siteler için bir uygulama oluşturmak zorunda değil. Bu sitelerin tümünde için size bir hesap oturum açma için ve geliştirici uygulamaları oluşturmak için olmalıdır.

    > [!NOTE]
    > Oturum açma bilgileri test etmek için bir yerel Web sitesi URL'si kullanamazlar Microsoft uygulamalarını yalnızca bir çalışma Web sitesi için Canlı bir URL kabul edin.
- Web sitenizi birkaç dosyalarında, uygun kimlik doğrulama sağlayıcısını belirtmek için ve oturum açma, kullanmak istediğiniz siteye göndermek için düzenleyin.

Bu makalede aşağıdaki görevler için ayrı yönergeler sağlar:

- [Google oturum açmalar etkinleştirme](#To_enable_Google_logins)
- [Facebook oturum açma bilgileri etkinleştirme](#To_enable_Facebook_logins)
- [Twitter oturumları etkinleştirme](#To_enable_Twitter_logins)

<a id="To_enable_Google_logins"></a>
## <a name="enabling-google-logins"></a>Google oturum açmalar etkinleştirme

1. Oluşturun veya WebMatrix başlangıç sitesi şablonu temel alan bir ASP.NET Web Pages sitesinde açın.
2. Açık  *\_AppStart.cshtml* sayfasında ve kod aşağıdaki satırı açıklamadan çıkarın. 

    [!code-css[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample1.css)]

### <a name="testing-google-login"></a>Google oturum açma test etme

1. Çalıştırma *default.cshtml* sitenizin sayfasını seçin **oturum** düğmesi.
2. Üzerinde *oturum açma* sayfasında **oturum açmak için başka bir hizmet kullanın** bölümünde, ya da seçin **Google** veya **Yahoo** Gönder düğmesi. Bu örnek, Google oturum açma kullanır. 

    Web sayfasının isteği Google oturum açma sayfasına yönlendirir.

    ![Google oturum açma](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image2.png)
3. Varolan bir Google hesabı kimlik bilgilerini girin.
4. Google izin vermek isteyip istemediğinizi isterse *Localhost* hesap bilgilerinden kullanmak için tıklatın **izin**.

    Kod kullanıcının kimliğini doğrulamak için Google belirtecini kullanır ve ardından bu sayfaya, Web sitenizde döndürür. Bu sayfa, kullanıcıların kendi Google oturum açma Web sitenizde var olan bir hesapla ilişkilendirmek sağlar. veya yeni bir hesap ile dış oturum açma ilişkilendirmek için sitenizde kaydedebilirsiniz.

    ![OAuth 5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image3.png)
5. Seçin **ilişkilendirmek** düğmesi. Tarayıcı, uygulamanızın giriş sayfasına döndürür.

<a id="To_enable_Facebook_logins"></a>
## <a name="enabling-facebook-logins"></a>Facebook oturum açma bilgileri etkinleştirme

1. Git [Facebook geliştiriciler site](https://developers.facebook.com/apps) (henüz oturum açmadıysanız oturum açma).
2. Seçin **yeni uygulama oluşturma** düğmesine tıklayın ve ardından ad ve yeni uygulama oluşturmak için istemleri izleyin.
3. Bölümünde **Facebook ile uygulamanızı nasıl tümleştirecek seçin**, seçin **Web sitesi** bölümü.
4. Doldurmak **Site URL'si** alan sitenizin URL'si (örneğin, `http://www.example.com`). **Etki alanı** alan isteğe bağlıdır; tüm etki alanı için kimlik doğrulaması sağlamak için bunu kullanabilirsiniz (gibi *example.com*). 

    > [!NOTE]
    > Yerel bilgisayarınızda bir URL ile bir site çalıştırıyorsanız, ister `http://localhost:12345` (numarasının olduğu bir yerel bağlantı noktası numarası), bu değer eklemek **Site URL'si** sitenizi test etmek için alan. Ancak, yerel site değişikliklerinizi bağlantı noktası numarasını kurduğunda, güncelleştirmeniz gerekecektir **Site URL'si** uygulamanızın alan.
5. Seçin **Değişiklikleri Kaydet** düğmesi.
6. Seçin **uygulamaları** yeniden sekmesini tıklatın ve ardından, uygulamanız için başlangıç sayfasını görüntüleyin.
7. Kopya **uygulama kimliği** ve **uygulama gizli anahtarı** uygulamanız için değerleri ve bir geçici metin dosyasına yapıştırın. Bu değerler, Web sitesi kodunuzda Facebook sağlayıcıya geçer.
8. Facebook Geliştirici site çıkın.

Böylece kullanıcılar olacak şimdi iki sayfalara siteniz siteye, Facebook hesaplarını kullanarak oturum açabilecek değişiklik.

1. Oluşturun veya WebMatrix başlangıç sitesi şablonu temel alan bir ASP.NET Web Pages sitesinde açın.
2. Açık  *\_AppStart.cshtml* sayfasında ve Facebook OAuth sağlayıcısı için kodun açıklamasını kaldırın. Uncommented kod bloğunu aşağıdaki gibi görünür: 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample2.cs)]
3. Kopya **uygulama kimliği** değeri olarak Facebook uygulaması değerinden `appId` parametre (tırnak içinde).
4. Kopya **uygulama gizli anahtarı** Facebook uygulaması olarak değerinden `appSecret` parametre değeri.
5. Dosyayı kaydedin ve kapatın.

### <a name="testing-facebook-login"></a>Test Facebook oturum açma

1. Sitenin çalışması *default.cshtml* sayfasında ve seçin **oturum açma** düğmesi.
2. Üzerinde *oturum açma* sayfasında **oturum açmak için başka bir hizmet kullanın** bölümünde, seçin **Facebook** simgesi. 

    Web sayfasının isteği Facebook oturum açma sayfasına yönlendirir.

    ![OAuth 2](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image4.png)
3. Bir Facebook hesabıyla oturum açın. 

    Kod, kimlik doğrulaması için Facebook belirtecini kullanır ve ardından bir sayfaya nerede Facebook oturum açma, sitenizin oturum açma ile ilişkilendirebilirsiniz döndürür. Kullanıcı adınız veya e-posta adresiniz içine girilir **e-posta** form üzerinde alan.

    ![OAuth 5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image5.png)
4. Seçin **ilişkilendirmek** düğmesi. 

    Giriş sayfasına tarayıcı döndürür ve kaydedilir.

<a id="To_enable_Twitter_logins"></a>
## <a name="enabling-twitter-logins"></a>Twitter oturumları etkinleştirme

1. Gözat [Twitter geliştiriciler site](https://dev.twitter.com/).
2. Seçin **bir uygulama oluşturmak** bağlamak ve siteye oturum açın.
3. Üzerinde **uygulama oluşturma** formunda, doldurmak **adı** ve **açıklama** alanları.
4. İçinde **Web sitesi** alanında, sitenizin URL'sini girin (örneğin, `http://www.example.com`). 

    > [!NOTE]
    > Siteniz yerel olarak test ediyorsanız (gibi bir URL'yi kullanarak `http://localhost:12345`), Twitter URL kabul. Ancak, yerel bir geri döngü IP adresi kullanmak mümkün olabilir (örneğin `http://127.0.0.1:12345`). Uygulamanızı yerel olarak test sürecini basitleştirir. Ancak, yerel site bağlantı noktası numarasını her değiştiğinde güncelleştirmeniz gerekecek **Web sitesi** uygulamanızın alan.
5. İçinde **geri çağırma URL'si** alan, kullanıcıların Twitter oturum açtıktan sonra dönmek için istediğiniz Web sayfası için bir URL girin. Örneğin, kullanıcıların (hangi oturum açma durumlarını algılar) başlangıç sitesi giriş sayfasına göndermek için girdiğiniz aynı URL'yi girin **Web sitesi** alan.
6. Koşulları kabul edin ve seçin **Twitter uygulamanızı oluşturma** düğmesi.
7. Üzerinde **uygulamalarım** giriş sayfasında, oluşturduğunuz uygulamayı seçin.
8. Üzerinde **ayrıntıları** sekmesinde alt kısmına kaydırın ve seçin **oluşturma My erişim belirteci** düğmesi.
9. Üzerinde **ayrıntıları** sekmesinde, kopya **tüketici anahtarı** ve **tüketici gizli** uygulamanız için değerleri ve bir geçici metin dosyasına yapıştırın. Web sitesi kodunuzda bu değerleri Twitter sağlayıcıya geçmesi.
10. Twitter site çıkın.

Kullanıcıların siteye Twitter hesaplarını kullanarak oturum mümkün olmayacaktır şimdi iki sayfalara Web sitenizde değişiklik.

1. Oluşturun veya WebMatrix başlangıç sitesi şablonu temel alan bir ASP.NET Web Pages sitesinde açın.
2. Açık  *\_AppStart.cshtml* sayfasında ve Twitter OAuth sağlayıcısı için kodun açıklamasını kaldırın. Uncommented kod bloğunu şöyle görünür: 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample3.cs)]
3. Kopya **tüketici anahtarı** değeri olarak Twitter uygulama değerinden `consumerKey` parametre (tırnak içinde).
4. Kopya **tüketici gizli** değeri olarak Twitter uygulama değerinden `consumerSecret` parametresi.
5. Dosyayı kaydedin ve kapatın.

### <a name="testing-twitter-login"></a>Test Twitter oturum açma

1. Çalıştırma *default.cshtml* sitenizin sayfasını ve **oturum açma** düğmesi.
2. Üzerinde *oturum açma* sayfasında **oturum açmak için başka bir hizmet kullanın** bölümünde, seçin **Twitter** simgesi. 

    Web sayfasının isteği için oluşturduğunuz uygulamayı bir Twitter oturum açma sayfasına yönlendirir.

    ![OAuth-4](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image6.png)
3. Twitter hesabıyla oturum açın.
4. Kod kullanıcının kimliğini doğrulamak için Twitter belirtecini kullanır ve ardından, bir sayfaya burada ilişkilendirebilirsiniz Web sitesi hesabınızla oturum açma döndürür. Ad veya e-posta adresinizi içine girilir **e-posta** form üzerinde alan.

    ![OAuth 5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image7.png)
5. Seçin **ilişkilendirmek** düğmesi. 

    Giriş sayfasına tarayıcı döndürür ve kaydedilir.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Ek Kaynaklar


- [Site genelinde davranışını özelleştirme](https://go.microsoft.com/fwlink/?LinkId=202906)
- [Güvenlik ve üyelik için ASP.NET Web sayfaları sitesi ekleme](https://go.microsoft.com/fwlink/?LinkID=202904)
