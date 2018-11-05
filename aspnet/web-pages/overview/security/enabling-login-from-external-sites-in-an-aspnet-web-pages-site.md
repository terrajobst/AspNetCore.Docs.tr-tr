---
uid: web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
title: Dış bir ASP.NET Web sitelerini kullanarak oturum açmaya çalışan sayfaları (Razor) sitesinde | Microsoft Docs
author: Rick-Anderson
description: Bu makalede, ASP.NET Web sayfaları (Razor) sitenizi Facebook, Google, Twitter, Yahoo ve diğer siteleri kullanarak oturum açmak açıklanmaktadır — diğer bir deyişle, nasıl destekleneceğini...
ms.author: riande
ms.date: 02/21/2014
ms.assetid: ef852096-a5bf-47b3-9945-125cde065093
msc.legacyurl: /web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 188a9203ba7b04f5a88d0f802f1a05bf35d58d8c
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021137"
---
<a name="logging-in-using-external-sites-in-an-aspnet-web-pages-razor-site"></a>Bir ASP.NET Web sayfaları (Razor) sitesinde dış sitelere kullanarak günlüğe kaydetme
====================
tarafından [Tom FitzMacken](https://github.com/tfitzmac)

> Bu makalede, ASP.NET Web sayfaları (Razor) sitenizi Facebook, Google, Twitter, Yahoo ve diğer siteleri kullanarak oturum açmak açıklanmaktadır — diğer bir deyişle, OAuth ve Openıd sitenizdeki destekleme.
> 
> Öğrenecekleriniz:
> 
> - WebMatrix başlangıç sitesi şablonunda kullandığınızda oturum açma diğer sitelerden elverişli hale getirme.
> 
> Bu makalede sunulan ASP.NET özelliğidir:
> 
> - `OAuthWebSecurity` Yardımcısı.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Bu öğreticide kullanılan yazılım sürümleri
> 
> 
> - ASP.NET Web sayfaları (Razor) 2
> - WebMatrix 3

ASP.NET Web sayfaları için destek içerir [OAuth](http://oauth.net/) ve [Openıd](http://openid.net/) sağlayıcıları. Bu sağlayıcıları kullanarak, sitenizi Facebook, Twitter, Microsoft ve Google var olan kimlik bilgilerini kullanarak oturum kullanıcılar oturum sağlayabilirsiniz. Örneğin, bir Facebook hesabı kullanarak oturum açın, kullanıcılar yalnızca bunları kullanıcı bilgilerini girdiğiniz yere Facebook oturum açma sayfasına yönlendiren bir Facebook simgesi seçebilirsiniz. Bunlar daha sonra Facebook oturum açma hesabıyla sitenizde ilişkilendirebilirsiniz. Bir ilgili Web sayfalarını üyelik özelliklerine kullanıcılar birden çok oturum açma (sosyal ağ sitelerine oturumlardan dahil) ilişkilendirebilirsiniz tek bir hesap kullanarak Web sitenizde geliştirmedir.

Bu görüntü oturum açma sayfasından gösterir **başlangıç sitesi** şablonu, burada bir kullanıcı seçebilirsiniz dış bir hesabıyla oturum açma etkinleştirmek için bir Facebook, Twitter, Google veya Microsoft simgesi:

![Dış sağlayıcılar](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image1.png)

Birkaç satır kod uncommenting tarafından üyelik, OAuth ve Openıd etkinleştirebilirsiniz **başlangıç sitesi** şablonu. Özellikler ve yöntemler OAuth ile çalışmak için kullanın ve Openıd sağlayıcılarının alanlarındadır `WebMatrix.Security.OAuthWebSecurity` sınıfı. **Başlangıç sitesi** şablonu tam üyelik altyapı içerir, kullanıcıların oturum yerel kimlik bilgilerini veya bu başka bir siteden kullanarak sitenizi uygulamasına izin vermeniz gerekiyor bir oturum açma sayfası, bir üyelik veritabanı ve tüm kodu ile tamamlandı .

Bu bölümde temel alan bir site için dış sitelerden oturum açmasına izin vermek nasıl bir örnek **başlangıç sitesi** şablonu. Başlangıç sitesi oluşturduktan sonra bu (ayrıntıları izleme) yapın:

- Bir OAuth sağlayıcısı (Facebook, Twitter ve Microsoft) kullanan siteler için dış sitesinde bir uygulama oluşturun. Bu, bu siteleri için oturum açma özelliği çağırmak için gereken uygulama anahtarlarını sağlar.
- Bir Openıd sağlayıcısı (Google) kullanan siteler için uygulama oluşturma gerekmez. Tüm bu sitelerden oturum açmak için ve geliştirici uygulamaları oluşturmak için hesabınız gerekir.

    > [!NOTE]
    > Oturum açma bilgileri test etmek için bir yerel Web sitesi URL'si kullanamazsınız. Bu nedenle Microsoft uygulamaları yalnızca bir çalışan Web sitesi için Canlı bir URL kabul edin.
- Web sitenizi birkaç dosyalarında, uygun kimlik doğrulama sağlayıcısını belirtmek için ve bir oturum açma kullanmak istediğiniz siteye göndermek için düzenleyin.

Bu makale aşağıdaki görevleri için ayrı yönergeler sağlar:

- [Google oturum açma etkinleştiriliyor](#To_enable_Google_logins)
- [Facebook oturum açma etkinleştiriliyor](#To_enable_Facebook_logins)
- [Twitter oturum açma etkinleştiriliyor](#To_enable_Twitter_logins)

<a id="To_enable_Google_logins"></a>
## <a name="enabling-google-logins"></a>Google oturum açma etkinleştiriliyor

1. Oluşturun veya WebMatrix başlangıç sitesi şablonu temel alan bir ASP.NET Web sayfaları sitesinde açın.
2. Açık  *\_AppStart.cshtml* sayfasında ve kod aşağıdaki satırı açıklamadan çıkarın. 

    [!code-css[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample1.css)]

### <a name="testing-google-login"></a>Google oturum açma testi

1. Çalıştırma *default.cshtml* sitenizin sayfasını ve **oturum** düğmesi.
2. Üzerinde *oturum açma* sayfasında **başka bir hizmete oturum açmak için kullandığınız** bölümünde, ya da seçin **Google** veya **Yahoo** Gönder düğmesi. Bu örnek, Google oturum açma bilgilerini kullanır. 

    Web sayfasının isteği Google oturum açma sayfasına yönlendirir.

    ![Google oturum açma](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image2.png)
3. Varolan bir Google hesabı kimlik bilgilerini girin.
4. Google izin vermek istediğiniz isterse *Localhost* hesap bilgilerinden kullanmak için **izin**.

    Kodu, kullanıcının kimliğini doğrulamak için Google belirteci kullanır ve bu sayfaya Web sitenizde döndürür. Bu sayfa, kullanıcıların kendi Google oturum açma bilgilerini kullanarak Web sitenizde var olan bir hesapla ilişkilendirmek sağlar veya dış oturum açma ile ilişkilendirmek için sitenizde yeni bir hesap kaydedebilir.

    ![OAuth 5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image3.png)
5. Seçin **ilişkilendirmek** düğmesi. Tarayıcıda uygulama giriş sayfasına döndürür.

<a id="To_enable_Facebook_logins"></a>
## <a name="enabling-facebook-logins"></a>Facebook oturum açma etkinleştiriliyor

1. Git [Facebook geliştiriciler site](https://developers.facebook.com/apps) (henüz oturum açmadıysanız oturum açın).
2. Seçin **yeni uygulama oluştur** düğmesini ve sonra adı ve yeni uygulama oluşturmak için istemleri izleyin.
3. Bölümünde **uygulamanızı Facebook ile nasıl tümleştirilecek seçin**, seçin **Web sitesi** bölümü.
4. Doldurun **Site URL'si** alanına sitenizin URL'siyle (örneğin, `http://www.example.com`). **Etki alanı** alan isteğe bağlıdır; bunu tüm etki alanı için kimlik doğrulaması sağlamak için kullanabilirsiniz (örneğin *example.com*). 

    > [!NOTE]
    > Yerel bilgisayarınızda bir URL ile bir site çalıştırıyorsanız, ister `http://localhost:12345` (sayıdır bir yerel bağlantı noktası numarası), bu değeri ekleyebileceğiniz **Site URL'si** sitenizi test etmek için alan. Ancak, yerel site değişikliği bağlantı noktası numarası için istediğiniz zaman, güncelleştirmeniz gerekecektir **Site URL'si** uygulamanızın alan.
5. Seçin **Değişiklikleri Kaydet** düğmesi.
6. Seçin **uygulamaları** sekmesine ve ardından uygulamanız için başlangıç sayfasını görüntüleyin.
7. Kopyalama **uygulama kimliği** ve **uygulama gizli anahtarı** uygulamanız için değerleri ve bunları bir geçici bir metin dosyasına yapıştırın. Bu değerler, Web sitesi kodunuzda Facebook sağlayıcıya geçer.
8. Facebook Geliştirici sitesi çıkın.

Kullanıcıların böylece artık iki siteniz Facebook hesaplarını kullanarak siteye kayıt yapabiliyor değişiklik.

1. Oluşturun veya WebMatrix başlangıç sitesi şablonu temel alan bir ASP.NET Web sayfaları sitesinde açın.
2. Açık  *\_AppStart.cshtml* sayfasında ve Facebook OAuth sağlayıcısı için kodun açıklamasını kaldırın. Açıklamalı olmayan kod bloğunu aşağıdaki gibi görünür: 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample2.cs)]
3. Kopyalama **uygulama kimliği** değeri olarak Facebook uygulaması değerinden `appId` parametre (tırnak işaretleri içinde).
4. Kopyalama **uygulama gizli anahtarı** Facebook uygulaması değerinden `appSecret` parametre değeri.
5. Dosyayı kaydedin ve kapatın.

### <a name="testing-facebook-login"></a>Facebook oturum açma testi

1. Sitenin çalıştırması *default.cshtml* sayfasında ve **oturum açma** düğmesi.
2. Üzerinde *oturum açma* sayfasında **başka bir hizmete oturum açmak için kullandığınız** bölümünde, seçin **Facebook** simgesi. 

    Web sayfasının isteği Facebook oturum açma sayfasına yönlendirir.

    ![OAuth 2](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image4.png)
3. Bir Facebook hesabınızda oturum açmak. 

    Kod, kimlik doğrulaması için Facebook belirteci kullanır ve burada Facebook oturum açma bilgilerinizi sitenizin oturum açma ile ilişkilendirebilirsiniz bir sayfasına döndürür. Kullanıcı adı veya e-posta adresinizi içine doldurulur **e-posta** formdaki alan.

    ![OAuth 5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image5.png)
4. Seçin **ilişkilendirmek** düğmesi. 

    Tarayıcı giriş sayfasına döndürür ve günlüğe kaydedilir.

<a id="To_enable_Twitter_logins"></a>
## <a name="enabling-twitter-logins"></a>Twitter oturum açma etkinleştiriliyor

1. Gözat [Twitter geliştiriciler site](https://dev.twitter.com/).
2. Seçin **uygulama oluşturma** bağlamak ve siteye oturum.
3. Üzerinde **uygulama oluşturma** oluşturmak, doldurmak **adı** ve **açıklama** alanları.
4. İçinde **Web sitesi** sitenizin URL'sini girin (örneğin, `http://www.example.com`). 

    > [!NOTE]
    > Siteniz yerel olarak test ediyorsanız (gibi bir URL kullanarak `http://localhost:12345`), Twitter URL'si kabul. Ancak, yerel bir geri döngü IP adresini kullanmanız mümkün olabilir (örneğin `http://127.0.0.1:12345`). Bu, uygulamanızı yerel olarak test etme işlemini basitleştirir. Ancak, yerel site bağlantı noktası numarası her değiştiğinde güncellemeniz gerekecektir **Web sitesi** uygulamanızın alan.
5. İçinde **geri çağırma URL'si** alan, kullanıcıların Twitter ile oturum açtıktan sonra dönmek için istediğiniz Web sayfası için bir URL girin. Örneğin, kullanıcıların (Bu, oturum açma durumu algılar) başlangıç sitesi giriş sayfasına göndermek için girdiğiniz aynı URL'yi girin. **Web sitesi** alan.
6. Koşulları kabul edin ve seçin **kendi Twitter uygulamanızı oluşturun** düğmesi.
7. Üzerinde **uygulamalarım** giriş sayfası, oluşturduğunuz uygulamayı seçin.
8. Üzerinde **ayrıntıları** için sekmesinde, kaydırın ve seçin **oluşturma My erişim belirteci** düğmesi.
9. Üzerinde **ayrıntıları** sekmesinde, kopya **tüketici anahtarı** ve **tüketici gizli** uygulamanız için değerleri ve bunları bir geçici bir metin dosyasına yapıştırın. Bu değerler, Web sitesi kodunuzda Twitter sağlayıcıya ileteceksiniz.
10. Twitter site çıkın.

Kullanıcıların Twitter hesaplarını kullanarak siteye bağlanmanız mümkün olacaktır, böylece artık iki Web sitenize değişiklik.

1. Oluşturun veya WebMatrix başlangıç sitesi şablonu temel alan bir ASP.NET Web sayfaları sitesinde açın.
2. Açık  *\_AppStart.cshtml* sayfasında ve Twitter OAuth sağlayıcısı için kodun açıklamasını kaldırın. Açıklamalı olmayan kod bloğu şöyle görünür: 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample3.cs)]
3. Kopyalama **tüketici anahtarı** değeri olarak bir Twitter uygulaması değerinden `consumerKey` parametre (tırnak işaretleri içinde).
4. Kopyalama **tüketici gizli** değeri olarak bir Twitter uygulaması değerinden `consumerSecret` parametresi.
5. Dosyayı kaydedin ve kapatın.

### <a name="testing-twitter-login"></a>Twitter oturum açma testi

1. Çalıştırma *default.cshtml* sitenizin sayfasını ve **oturum açma** düğmesi.
2. Üzerinde *oturum açma* sayfasında **başka bir hizmete oturum açmak için kullandığınız** bölümünde, seçin **Twitter** simgesi. 

    Web sayfasının isteği oluşturduğunuz uygulama için bir Twitter oturum açma sayfasına yönlendirir.

    ![OAuth-4](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image6.png)
3. Bir Twitter hesabınızda oturum açmak.
4. Kod kullanıcının kimliğini doğrulamak için Twitter belirtecini kullanır ve size bir sayfaya burada ilişkilendirebilirsiniz Web sitesi hesabınız ile oturum açma bilgilerinizi döndürür. Ad veya e-posta adresinizi içine doldurulur **e-posta** formdaki alan.

    ![OAuth 5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image7.png)
5. Seçin **ilişkilendirmek** düğmesi. 

    Tarayıcı giriş sayfasına döndürür ve günlüğe kaydedilir.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Ek Kaynaklar


- [Site Geneline Yönelik Davranışını Özelleştirme](https://go.microsoft.com/fwlink/?LinkId=202906)
- [Bir ASP.NET Web sayfaları sitesinde için güvenlik ve üyelik ekleme](https://go.microsoft.com/fwlink/?LinkID=202904)
