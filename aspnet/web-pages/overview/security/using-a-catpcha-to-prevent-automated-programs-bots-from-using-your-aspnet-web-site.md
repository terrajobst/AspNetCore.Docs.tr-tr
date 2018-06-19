---
uid: web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
title: Bot, ASP.NET Web Razor kullanmasını önlemek için bir güvenlik kodu kullanarak) Site | Microsoft Docs
author: microsoft
description: Bu makalede bir ASP.NET Web Pages'da (Razor) görevler önleyecek otomatik programları (aracılarını) ReCaptcha (bir güvenlik önlemi) kullanımı açıklanmaktadır biz...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/21/2012
ms.topic: article
ms.assetid: 2b381a41-2cb3-40c0-8545-1d393e22877f
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
msc.type: authoredcontent
ms.openlocfilehash: 75e80a3e7ebe787852152404bf2e0bf88a1a6a56
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
ms.locfileid: "26573282"
---
<a name="using-a-captcha-to-prevent-bots-from-using-your-aspnet-web-razor-site"></a>Site bot, ASP.NET Web Razor kullanmasını önlemek için bir güvenlik kodu kullanarak)
====================
tarafından [Microsoft](https://github.com/microsoft)

> Bu makalede, bir ASP.NET Web sayfaları (Razor) Web sitesi görevleri önleyecek otomatik programları (aracılarını) ReCaptcha (bir güvenlik önlemi) kullanımı açıklanmaktadır.
> 
> **Öğrenecekleriniz:** 
> 
> - Bir güvenlik kodu test sitenize ekleme.
> 
> Bu makalede sunulan ASP.NET özellikleri şunlardır:
> 
> - `ReCaptcha` Yardımcısı.
> 
> > [!NOTE]
> > Bu makaledeki bilgileri, ASP.NET Web sayfaları 1.0 ve Web Pages 2 için geçerlidir.


## <a name="about-captchas"></a>CAPTCHAs hakkında

Kaydetme kullanıcıların, sitenizde veya hatta yalnızca izin vermek istediğiniz zaman bir adı ve URL (like blog yorum için) girin, sahte adlarının sel alabilirsiniz. Bunlar genellikle bulabilirsiniz her Web sitesinin URL'leri bırakmayı deneyin otomatik programları (aracılarını) tarafından bırakılır. (Bir ortak motivasyon, satış ürünleri URL'lerini yerleştirmektir.)

Bir kullanıcının gerçek kişinin ve bilgisayar programı kullanarak olduğundan emin olun yardımcı bir *CAPTCHA* kaydetmek veya aksi halde, adlarını ve site girin kullanıcıları doğrulamak için. CAPTCHA bilgisayarlar ve insanlar birbirinden bildirmek tamamen otomatik ortak Turing test için anlamına gelir. Bir güvenlik kodu olduğu bir *sınama yanıtı* yapmak bir kişi için kolay, ancak sabit bir otomatik programın yapmak, kullanıcı sorulan bir şeyler için test. CAPTCHA en yaygın türü burada bazı bozuk harf bakın ve bunları yazın istenir biridir. (Bozulma harfleri geçirilirse aracılarını için sabit yapmak için varsayılır.)

## <a name="adding-a-recaptcha-test"></a>ReCaptcha testine ekleme

ASP.NET sayfaları, kullandığınız `ReCaptcha` ReCaptcha hizmetini temel alan bir güvenlik kodu test oluşturmak için yardımcı ([http://recaptcha.net](http://recaptcha.net)). `ReCaptcha` Yardımcısı, kullanıcılar sayfa doğrulanır önce doğru girmek zorunda iki bozuk sözcükler görüntüsünü görüntüler. Kullanıcı yanıtı ReCaptcha.Net hizmeti tarafından doğrulanır.

![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.jpg)

1. Web sitenizi ReCaptcha.Net adresindeki kayıt ([http://recaptcha.net](http://recaptcha.net)). Kayıt tamamladıktan sonra bir ortak anahtar ve özel anahtarı elde edersiniz.
2. ASP.NET Web Yardımcıları kitaplığı açıklandığı gibi Web sitenize ekleyin [yükleme Yardımcıları bir ASP.NET Web Pages sitesinde](https://go.microsoft.com/fwlink/?LinkId=252372), henüz yapmadıysanız.
3. Henüz yoksa bir  *\_AppStart.cshtml* dosya, bir Web sitesinin kök klasöründe adlı bir dosya oluşturun  *\_AppStart.cshtml*.
4. Aşağıdakileri ekleyin `Recaptcha` yardımcı ayarlarında  *\_AppStart.cshtml* dosyası: 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample1.cshtml?highlight=6-7)]
5. Ayarlama `PublicKey` ve `PrivateKey` kendi ortak ve özel anahtarları kullanarak özellikleri.
6. Kaydet  *\_AppStart.cshtml* dosya ve kapatın.
7. Adlı yeni bir sayfa bir Web sitesinin kök klasörü oluşturmak *Recaptcha.cshtml*.
8. Varolan içeriği aşağıdakiyle değiştirin: 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample2.cshtml)]
9. Çalıştırma *Recaptcha.cshtml* sayfasını bir tarayıcıda. Varsa `PrivateKey` değeri geçerli, bir düğmeyi ve ReCaptcha denetimi sayfasında görüntülenir. Anahtarları içinde genel olarak olmayan ayarlarsanız  *\_AppStart.html*, sayfaya bir hata görüntüler. 

    ![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.png)
10. Test için sözcükleri girin. ReCaptcha testi başarılı olursa, bunu belirten bir ileti görürsünüz. Aksi takdirde bir hata iletisi görüntülenir ve ReCaptcha denetim görünürler.

> [!NOTE]
> Bilgisayarınızı proxy sunucusu kullanan bir etki alanındaysa, yapılandırmanız gerekebilir `defaultproxy` öğesinin *Web.config* dosya. Aşağıdaki örnekte gösterildiği bir *Web.config* ile dosya `defaultproxy` ReCaptcha hizmetin çalışmasını etkinleştirmek için yapılandırılmış öğesi.
> 
> [!code-xml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample3.xml)]


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Ek Kaynaklar


- [ASP.NET Web sayfaları siteleri için site genelinde davranışını özelleştirme](https://go.microsoft.com/fwlink/?LinkId=202906)
- [ReCaptcha site](https://www.google.com/recaptcha)
