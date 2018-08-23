---
uid: web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
title: Botlar, ASP.NET Web Razor kullanmasını önlemek için CAPTCHA kullanarak) sitesi | Microsoft Docs
author: microsoft
description: Bu makalede, otomatik programların (botlar) ASP.NET Web Pages'da (Razor) görevlerini gerçekleştirmesini engelleyecek şekilde ReCaptcha (bir güvenlik önlemi) kullanmayı açıklar ediyoruz...
ms.author: riande
ms.date: 05/21/2012
ms.assetid: 2b381a41-2cb3-40c0-8545-1d393e22877f
msc.legacyurl: /web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
msc.type: authoredcontent
ms.openlocfilehash: dc014f42490327743764787d58c613b7caa89f1f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756815"
---
<a name="using-a-captcha-to-prevent-bots-from-using-your-aspnet-web-razor-site"></a>Site Botlar, ASP.NET Web Razor kullanmasını önlemek için CAPTCHA kullanarak)
====================
tarafından [Microsoft](https://github.com/microsoft)

> Bu makalede, bir ASP.NET Web sayfaları (Razor) Web sitesinde görevleri önleyecek otomatik programların (botlar) ReCaptcha (bir güvenlik önlemi) kullanmayı açıklar.
> 
> **Öğrenecekleriniz:** 
> 
> - CAPTCHA test sitenize ekleme.
> 
> Bu makalede sunulan ASP.NET özellikleri şunlardır:
> 
> - `ReCaptcha` Yardımcısı.
> 
> > [!NOTE]
> > Bu makaledeki bilgiler, ASP.NET Web sayfaları 1.0 ve Web Pages 2 için geçerlidir.


## <a name="about-captchas"></a>CAPTCHAs hakkında

Sitenizde veya hatta yalnızca kaydetme kişilere izin dilediğiniz zaman bir ad ve URL (like blog açıklaması için) girin, sel sahte adlarını alabilirsiniz. Bunlar genellikle bulabilirsiniz her Web sitesinin URL'leri bırakmayı deneyin otomatik programların (botlar) tarafından bırakılır. (Bir ortak motivasyon, satılan ürünleri URL'lerini yerleştirmektir.)

Bir kullanıcının gerçek kişi ve bilgisayar programı kullanarak olduğundan emin olun yardımcı bir *CAPTCHA* kaydetme veya aksi halde, adlarını ve site girin kullanıcıları doğrulamak için. CAPTCHA bilgisayarlar ve insanlar birbirinden tamamen otomatik genel Turing test için anlamına gelir. CAPTCHA olduğu bir *sınama yanıtı* yapmak bir kişi için kolay, ancak yapmak otomatik bir program için sabit, kullanıcıdan istenir şeyler test. CAPTCHA en yaygın türü, bozuk harfleri bakın ve bunları yazmanız istenir olan biridir. (Bozulma harf çözmek botlar için sabit hale getirmek için varsayılır.)

## <a name="adding-a-recaptcha-test"></a>Bir ReCaptcha Test ekleme

ASP.NET sayfaları'nda kullanabileceğiniz `ReCaptcha` ReCaptcha hizmetini temel alan bir CAPTCHA testini işlenecek Yardımcısı ([http://recaptcha.net](http://recaptcha.net)). `ReCaptcha` Yardımcısı, kullanıcıların doğru sayfanın doğrulanır girmelerini iki bozuk sözcük görüntüsünü görüntüler. Kullanıcı yanıtı ReCaptcha.Net hizmeti tarafından doğrulanır.

![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.jpg)

1. ReCaptcha.Net adresindeki Web sitenize kaydetme ([http://recaptcha.net](http://recaptcha.net)). Kaydı tamamladıktan sonra ortak anahtar ve özel anahtarı alırsınız.
2. ASP.NET Web Yardımcıları kitaplığı açıklandığı Web sitenize ekleyin [yükleme Yardımcıları bir ASP.NET Web sayfaları sitesinde](https://go.microsoft.com/fwlink/?LinkId=252372), henüz yapmadıysanız.
3. Henüz yoksa bir  *\_AppStart.cshtml* dosya, bir Web sitesinin kök klasöründe adlı bir dosya oluşturun  *\_AppStart.cshtml*.
4. Aşağıdaki `Recaptcha` Yardımcısı ayarlarında  *\_AppStart.cshtml* dosyası: 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample1.cshtml?highlight=6-7)]
5. Ayarlama `PublicKey` ve `PrivateKey` kendi ortak ve özel anahtarlar kullanarak özellikleri.
6. Kaydet  *\_AppStart.cshtml* dosya ve kapatın.
7. Bir Web sitesinin kök klasöründe adlı yeni sayfa oluşturma *Recaptcha.cshtml*.
8. Mevcut içeriğini aşağıdakiyle değiştirin: 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample2.cshtml)]
9. Çalıştırma *Recaptcha.cshtml* sayfasını bir tarayıcıda. Varsa `PrivateKey` değeri geçerli, ReCaptcha denetimi ve bir düğmeyi sayfasını görüntüler. Anahtarları içinde genel olarak ayarlamanız değil  *\_AppStart.html*, bir hata sayfası görüntülenecekti. 

    ![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.png)
10. Test için sözcükleri girin. ReCaptcha test başarılı olursa belirlememişse bu bağlamda bir ileti görürsünüz. Aksi takdirde bir hata iletisi görürsünüz ve ReCaptcha denetim görünürler.

> [!NOTE]
> Bilgisayarınız proxy sunucusu kullanan bir etki alanındaysa yapılandırmanız gerekebilir `defaultproxy` öğesinin *Web.config* dosya. Aşağıdaki örnekte gösterildiği bir *Web.config* ile dosya `defaultproxy` ReCaptcha hizmetin çalışmasını etkinleştirmek için yapılandırılmış öğesi.
> 
> [!code-xml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample3.xml)]


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Ek Kaynaklar


- [ASP.NET Web sayfaları sitelerinde için site geneline yönelik davranışını özelleştirme](https://go.microsoft.com/fwlink/?LinkId=202906)
- [ReCaptcha site](https://www.google.com/recaptcha)
