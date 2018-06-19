---
uid: web-pages/overview/getting-started/11-adding-email-to-your-web-site
title: E-posta gönderirken bir ASP.NET Web sayfaları (Razor) Site | Microsoft Docs
author: tfitzmac
description: Bu bölümde bir Web sitesinden bir otomatik e-posta iletisi göndermek nasıl açıklanmaktadır.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: fc49bcb9-f1a9-4048-8c3f-b60951853200
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/11-adding-email-to-your-web-site
msc.type: authoredcontent
ms.openlocfilehash: 9be242d238c627a9557fe7ff7e596974e5b7d1c8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30896522"
---
<a name="sending-email-from-an-aspnet-web-pages-razor-site"></a>Bir ASP.NET Web sayfaları (Razor) sitesinden e-posta gönderme
====================
tarafından [zel FitzMacken](https://github.com/tfitzmac)

> Bu makalede, ASP.NET Web sayfaları (Razor) kullandığınızda, bir Web sitesinden bir e-posta iletisi göndermek açıklanmaktadır.
> 
> Öğrenecekleriniz:
> 
> - Web sitenizi bir e-posta iletisi göndermek nasıl.
> - E-posta iletisine bir dosya eklemek nasıl.
> 
> Bu makalede sunulan ASP.NET özelliğidir:
> 
> - `WebMail` Yardımcısı.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Öğreticide kullanılan yazılım sürümleri
> 
> 
> - ASP.NET Web sayfaları (Razor) 3
>   
> 
> Bu öğreticide, ASP.NET Web Pages 2 ile de çalışır.


<a id="Sending_Email_Messages"></a>
## <a name="sending-email-messages-from-your-website"></a>Web sitenizi e-posta iletileri gönderme

Her türlü neden Web sitesinden e-posta göndermeniz gerekebilir nedenleri vardır. Kullanıcılara doğrulama ileti gönderme ya da bildirimler (yeni bir kullanıcı kaydettiği Örneğin,.) kendinize gönderebilir `WebMail` Yardımcısı, e-posta göndermek kolaylaştırır.

Kullanılacak `WebMail` Yardımcısı, elinizde bir SMTP sunucusuna erişim izni. (SMTP anlamına gelir *Basit Posta Aktarım Protokolü*.) Bir SMTP sunucusu iletileri alıcının sunucusuna yalnızca ileten bir e-posta sunucusudur &#8212; e-posta giden tarafında bulunur. Web siteniz için bir barındırma sağlayıcısı kullanıyorsanız, bunlar büyük olasılıkla e-posta ile ayarladığınız ve bunlar, SMTP sunucunuzun adını nedir anlayabilirsiniz. Genellikle bir şirket ağı içinde çalışıyorsanız, bir yönetici veya BT departmanınız kullanabileceğiniz bir SMTP sunucusu hakkında bilgi verebilir. Evde çalışıyorsanız, hatta kendi SMTP sunucusunun adını anlayabilirsiniz, normal e-posta sağlayıcısı kullanarak test mümkün olabilir. Genelde gerekir:

- SMTP sunucusunun adı.
- Bağlantı noktası numarası. Bu neredeyse her zaman 25'tir. Ancak, ISS 587 numaralı bağlantı noktalarını kullanmanızı gerektirebilir. E-posta için Güvenli Yuva Katmanı (SSL) kullanıyorsanız, farklı bir bağlantı noktası gerekebilir. E-posta sağlayıcınıza başvurun.
- Kimlik bilgileri (kullanıcı adı, parola).

Bu yordamda, iki sayfa oluşturun. İlk sayfasının bir teknik destek formda doldurma gibi bir açıklama girin kullanıcılar olanak sağlayan bir formu vardır. İlk sayfa ikinci bir sayfa için kendi bilgileri gönderir. İkinci sayfasında kod kullanıcının bilgilerini ayıklar ve e-posta iletisi gönderir. Ayrıca, sorun raporu alındı onaylayan bir ileti görüntüler.

![[Görüntü]](11-adding-email-to-your-web-site/_static/image1.jpg)

> [!NOTE]
> Bu örneği basit tutmak için kod başlatır `WebMail` Yardımcısı sağ kullandığınız bu sayfasında. Başlatır ancak gerçek Web siteleri için şöyle başlatma kodu genel bir dosyaya koymak için iyi bir fikir, böylelikle `WebMail` , Web sitenizin tüm dosyalar için Yardımcısı. Daha fazla bilgi için bkz: [Site genelinde davranışı ASP.NET Web sayfaları için özelleştirme](https://go.microsoft.com/fwlink/?LinkId=202906#Setting_Values_For_Helpers).


1. Yeni bir Web sitesi oluşturun.
2. Adlı yeni bir sayfa ekleyin *EmailRequest.cshtml* ve aşağıdaki biçimlendirmeyi ekleyin: 

    [!code-html[Main](11-adding-email-to-your-web-site/samples/sample1.html)]

    Dikkat `action` form öğesinin özniteliği ayarlanmış *ProcessRequest.cshtml*. Bu, formun geçerli sayfaya geri yerine bu sayfaya gönderilecek anlamına gelir.
3. Adlı yeni bir sayfa ekleyin *ProcessRequest.cshtml* Web sitesine ve aşağıdaki kodu ve biçimlendirme ekleyin:   

    [!code-cshtml[Main](11-adding-email-to-your-web-site/samples/sample2.cshtml)]

    Kodda sayfasına gönderildikleri form alanlarını değerini alın. Ardından çağıran `WebMail` yardımcının `Send` oluşturmak ve e-posta iletisi göndermek için yöntem. Bu durumda, kullanılacak değerler formdan gönderildikleri değerlerle birleştirme metin oluşur.

    Bu sayfa için kod içindedir bir `try/catch` bloğu. Bir e-posta gönderme denemesi herhangi bir nedenden dolayı işe yaramazsa (örneğin, ayarları doğru değil), kodda `catch` bloğu çalıştırır ve ayarlar `errorMessage` oluştu hata değişkeni. (Hakkında daha fazla bilgi için `try/catch` blokları veya `<text>` etiketlemek için bkz: [ASP.NET Web sayfalarını programlama kullanarak Razor sözdizimi giriş](https://go.microsoft.com/fwlink/?LinkID=251587#ID_HandlingErrors).)

    Sayfasının gövdesindeki varsa `errorMessage` değişkenidir boş (varsayılan), kullanıcı e-posta iletisi gönderilen bir ileti görür. Varsa `errorMessage` değişkeni true, kullanıcı görür bir ileti gönderilirken bir sorun oluştu ileti ayarlanır.

    Bir hata iletisi görüntüler sayfasının bölümünde, ek bir test fark: `if(debuggingFlag)`. Bu e-posta gönderme konusunda sorun yaşıyorsanız, true olarak ayarlanan bir değişkendir. Zaman `debuggingFlag` true olur ve e-posta gönderilirken bir sorun varsa, e-posta iletisi göndermeye çalışırken ne olursa olsun ASP.NET bildirdi gösteren bir ek hata iletisi görüntülenir. Orta, ancak Uyarı: e-posta iletisine gönderemediğinde, ASP.NET raporları hata iletileri genel olabilir. Örneğin, varsa (örneğin, bir hata sunucu adında yaptığınız nedeniyle), ASP.NET SMTP sunucusu kişi olamaz, hata `Failure sending mail`.

    > [!NOTE] 
    > 
    > **Önemli** bir özel durum nesneden bir hata iletisi ulaştıklarında (`ex` kodda), yapmak *değil* düzenli olarak bu iletiyi üzerinden kullanıcılara geçirin. Özel durum nesneleri genellikle kullanıcılar değil görmeniz gerekir ve bir güvenlik açığı bile olabilir bilgilerini içerir. İşte bu nedenle bu kodu değişken içeriyor. `debuggingFlag` kullanılan bir anahtar hata iletisini görüntülemek ve neden değişken varsayılan olarak false olarak ayarlanır. Bu değişken true (ve bu nedenle hata iletisini görüntülemek için) ayarlamalısınız *yalnızca* e-posta gönderme ile ilgili bir sorun yaşıyoruz ve hata ayıklama gerekiyorsa. Herhangi bir sorunu düzelttikten sonra ayarlamak `debuggingFlag` false dön.

    Değiştirme aşağıdaki e-posta kodu ilgili ayarları:

   - Ayarlama `your-SMTP-host` erişiminiz SMTP sunucusunun adı.
   - Ayarlama `your-user-name-here` SMTP sunucusu hesabının kullanıcı adı.
   - Ayarlama `your-account-password` SMTP sunucusu hesabının parolasına.
   - Ayarlama `your-email-address-here` kendi e-posta adresi. Bu, ileti gönderilen e-posta adresidir. (Bazı e-posta sağlayıcısı farklı bir belirtmenize izin vermeyin `From` adres ve kullanıcı adı olarak kullanacağı `From` adresi.)

     > [!TIP] 
     > 
     > <a id="configuring_email_settings"></a>
     > ### <a name="configuring-email-settings"></a>E-posta ayarlarını yapılandırma
     > 
     > Bazen SMTP sunucusu, bağlantı noktası numarası ve benzeri için doğru ayarlara sahip olduğundan emin olmak zor olabilir. Aşağıda, birkaç ipucu verilmiştir:
     > 
     > - SMTP sunucusu adı şöyle görülür `smtp.provider.com` veya `smtp.provider.net`. Ancak, bir barındırma sağlayıcısına sitenizi yayımlarsanız, SMTP sunucu adı bu noktada olabilir `localhost`. Bu durum, yayımladıktan sonra sitenizi sağlayıcının sunucusunda çalıştığından, e-posta sunucusu uygulamanız açısından bakıldığında yerel olabilir çünkü. Bu değişikliği sunucu adları, yayımlama işleminin bir parçası SMTP sunucu adını değiştirmek zorunda anlamına gelebilir.
     > - Bağlantı noktası numarası genellikle 25'tir. Ancak, bazı sağlayıcılar bağlantı noktasını kullanacak biçimde 587 veya bazı başka bir bağlantı gerektirir.
     > - Doğru kimlik bilgilerini kullandığınızdan emin olun. Bir barındırma sağlayıcısına sitenizi yayımladıysanız, sağlayıcı özellikle e-posta için belirtilir kimlik bilgileri kullanın. Bu, yayımlama için kullandığınız kimlik bilgilerinden farklı olabilir.
     > - Bazen kimlik bilgilerini hiç gerekmez. Kişisel ISS kullanarak e-posta gönderiyorsanız, e-posta sağlayıcınız kimlik bilgilerinizi zaten biliyor olabilirsiniz. Yayımladığınızda, yerel bilgisayarınızda test ne zaman farklı kimlik bilgileri kullanmanız gerekebilir.
     > - E-posta sağlayıcınız şifreleme kullanıyorsa, ayarlamanız gerekir `WebMail.EnableSsl` için `true`.
4. Çalıştırma *EmailRequest.cshtml* sayfasını bir tarayıcıda. (Emin olun sayfa seçildiğinde, **dosyaları** çalıştırmadan önce onu çalışma.)
5. Adınızı ve sorun açıklamasını girin ve ardından **gönderme** düğmesi. İçin yönlendirilirsiniz *ProcessRequest.cshtml* sayfasında, iletinizi onaylar ve hangi, bir e-posta iletisi gönderiyor. 

    ![[Görüntü]](11-adding-email-to-your-web-site/_static/image2.jpg)

<a id="Sending_a_File"></a>
## <a name="sending-a-file-using-email"></a>E-posta kullanarak dosya gönderme

E-posta iletileri için eklenen dosyaları da gönderebilirsiniz. Bu yordamda, bir metin dosyası ve iki HTML sayfası oluşturun. Metin dosyasını bir e-posta eki olarak kullanacaksınız.

1. Web sitesi, yeni bir metin dosyası ekleyin ve adını *dosyam.txt*.
2. Aşağıdaki metni kopyalayıp dosyaya yapıştırın: 

    `Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.`
3. Adlı bir sayfa oluşturma *SendFile.cshtml* ve aşağıdaki biçimlendirmeyi ekleyin: 

    [!code-html[Main](11-adding-email-to-your-web-site/samples/sample3.html)]
4. Adlı bir sayfa oluşturma *ProcessFile.cshtml* ve aşağıdaki biçimlendirmeyi ekleyin: 

    [!code-cshtml[Main](11-adding-email-to-your-web-site/samples/sample4.cshtml)]
5. Değiştirme aşağıdaki e-posta kodu örneği ile ilişkili ayarları:

    - Ayarlama `your-SMTP-host` erişiminiz bir SMTP sunucusunun adı.
    - Ayarlama `your-user-name-here` SMTP sunucusu hesabının kullanıcı adı.
    - Ayarlama `your-email-address-here` kendi e-posta adresi. Bu, ileti gönderilen e-posta adresidir.
    - Ayarlama `your-account-password` SMTP sunucusu hesabının parolasına.
    - Ayarlama `target-email-address-here` kendi e-posta adresi. (Daha önce genellikle bir e-posta başka bir kişiye gönderir, ancak test etmek için kendinize gönderebilirsiniz gibi.)
6. Çalıştırma *SendFile.cshtml* sayfasını bir tarayıcıda.
7. Adınızı, bir konu satırı ve eklemek için metin dosyasının adını girin (*dosyam.txt*).
8. Tıklatın `Submit` düğmesi. Önce yönlendirilirsiniz gibi *ProcessFile.cshtml* iletinizi onaylar ve hangi gönderir, e-posta iletisine ekli dosyasıyla sayfa.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Ek Kaynaklar


- [ASP.NET Web Sayfaları (Razor) Sorun Giderme Kılavuzu](https://go.microsoft.com/fwlink/?LinkId=253001)
- [Basit Posta Aktarım Protokolü](https://msdn.microsoft.com/library/aa480435.aspx)
- [ASP.NET Web sayfaları için özelleştirme site genelinde davranışı](https://go.microsoft.com/fwlink/?LinkId=202906)
