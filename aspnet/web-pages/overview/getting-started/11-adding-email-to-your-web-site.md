---
uid: web-pages/overview/getting-started/11-adding-email-to-your-web-site
title: E-posta gönderen bir ASP.NET Web sayfaları (Razor) sitesinde | Microsoft Docs
author: tfitzmac
description: Bu bölümde bir Web sitesinden bir otomatik e-posta iletisi göndermek nasıl açıklanmaktadır.
ms.author: riande
ms.date: 02/20/2014
ms.assetid: fc49bcb9-f1a9-4048-8c3f-b60951853200
msc.legacyurl: /web-pages/overview/getting-started/11-adding-email-to-your-web-site
msc.type: authoredcontent
ms.openlocfilehash: 4429b9df0a6bc689eb3549c13aee598353c438af
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755830"
---
<a name="sending-email-from-an-aspnet-web-pages-razor-site"></a>Bir ASP.NET Web sayfaları (Razor) sitesinden e-posta gönderme
====================
tarafından [Tom FitzMacken](https://github.com/tfitzmac)

> Bu makalede, ASP.NET Web sayfaları (Razor) kullandığınızda, bir Web sitesinden bir e-posta iletisi göndermek açıklanmaktadır.
> 
> Öğrenecekleriniz:
> 
> - Web sitenizi bir e-posta iletisi göndermek nasıl.
> - Nasıl bir e-posta iletisine bir dosya ekleyin.
> 
> Bu makalede sunulan ASP.NET özelliğidir:
> 
> - `WebMail` Yardımcısı.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Bu öğreticide kullanılan yazılım sürümleri
> 
> 
> - ASP.NET Web sayfaları (Razor) 3
>   
> 
> Bu öğreticide, ASP.NET Web Pages 2 ile de çalışır.


<a id="Sending_Email_Messages"></a>
## <a name="sending-email-messages-from-your-website"></a>Sitenizden e-posta gönderme

Her türlü neden sitenizden e-posta göndermesi gerekebilir nedenleri vardır. Kullanıcılar için onay mesajları gönderebilir veya kendiniz için bildirimleri (yeni bir kullanıcı kaydettiği Örneğin,.) gönderebilir `WebMail` Yardımcısı e-posta göndermenizi kolaylaştırır.

Kullanılacak `WebMail` Yardımcısı, sahip olduğunuz bir SMTP sunucusuna erişim izni. (SMTP anlamına gelen *Basit Posta Aktarım Protokolü*.) Bir SMTP sunucusu, yalnızca iletileri alıcının sunucusuna yönlendiren bir e-posta sunucusudur &#8212; e-posta olarak giden adlandırılan tarafıdır. Web siteniz için bir barındırma sağlayıcısı kullanıyorsanız, bunlar büyük olasılıkla e-posta ile ayarladığınız ve bunlar, SMTP sunucunuzun adını ne olduğunu söyleyebilir. Genellikle bir kurumsal ağ içinde çalışıyorsanız, bir yönetici ya da BT departmanınız, kullanabileceğiniz bir SMTP sunucusu hakkında bilgi verebilirsiniz. Evde çalışıyorsanız, hatta kendi SMTP sunucusunun adını söyleyebilirsiniz, sıradan bir e-posta Sağlayıcısı'nı kullanarak test edebilirsiniz olabilir. Genelde gerekir:

- SMTP sunucusunun adı.
- Bağlantı noktası numarası. 25 neredeyse her zaman budur. Ancak, ISS bağlantı noktası 587 kullanmanızı gerektirebilir. E-posta için Güvenli Yuva Katmanı (SSL) kullanıyorsanız, farklı bir bağlantı noktası gerekebilir. E-posta sağlayıcınıza başvurun.
- Kimlik bilgileri (kullanıcı adı, parola).

Bu yordamda, iki sayfaları oluşturun. İlk sayfa alacağı bir teknik destek biçimde doldurma kullanıcılar, bir açıklama girin sağlayan bir biçime sahiptir. İlk sayfa, ikinci bir sayfa için kendi bilgileri gönderir. İkinci sayfasında, kod için kullanıcı bilgilerini ayıklar ve e-posta iletisi gönderir. Ayrıca, sorun raporunda alındı onaylayan bir ileti görüntüler.

![[image]](11-adding-email-to-your-web-site/_static/image1.jpg)

> [!NOTE]
> Bu örneği basit tutmak için kod başlatır `WebMail` kullandığınız, sayfanın sağ Yardımcısı. Başlatma, ancak gerçek Web siteleri için genel bir dosyada başlatma kodu yerleştirmenin iyi bir fikir, böylelikle `WebMail` Yardımcısı, Web sitenizin tüm dosyalar için. Daha fazla bilgi için [Site genelinde davranışı ASP.NET Web sayfaları için özelleştirme](https://go.microsoft.com/fwlink/?LinkId=202906#Setting_Values_For_Helpers).


1. Yeni bir Web sitesi oluşturun.
2. Adlı yeni bir sayfa ekleyin *EmailRequest.cshtml* ve aşağıdaki işaretlemeyi ekleyin: 

    [!code-html[Main](11-adding-email-to-your-web-site/samples/sample1.html)]

    Dikkat `action` form öğesinin özniteliği sınıflandırmalara ayarlandığı *ProcessRequest.cshtml*. Başka bir deyişle formun geçerli sayfaya geri yerine bu sayfaya gönderilir.
3. Adlı yeni bir sayfa ekleyin *ProcessRequest.cshtml* Web sitesine ve aşağıdaki kodu ve biçimlendirmeyi ekleyin:   

    [!code-cshtml[Main](11-adding-email-to-your-web-site/samples/sample2.cshtml)]

    Kod içinde sayfasına gönderilen form alanlarını değerlerini alın. Ardından çağırın `WebMail` yardımcının `Send` oluşturmak ve e-posta iletisi göndermek için yöntemi. Bu durumda, kullanılacak değerler formdan gönderilen değerlerle birleştirme metin oluşur.

    Bu sayfa için kod içinde olan bir `try/catch` blok. E-posta gönderme denemesi herhangi nedenden dolayı çalışmazsa (örneğin, ayarları doğru değil), kodda `catch` bloğu çalıştırır ve ayarlar `errorMessage` değişken için olan hata oluştu. (Hakkında daha fazla bilgi için `try/catch` blokları veya `<text>` etiketlemek için bkz: [ASP.NET Web sayfaları programlama kullanarak Razor sözdizimi giriş](https://go.microsoft.com/fwlink/?LinkID=251587#ID_HandlingErrors).)

    Sayfanın gövdesindeki varsa `errorMessage` değişkendir boş (varsayılan), kullanıcı e-posta ileti gönderen bir ileti görür. Varsa `errorMessage` değişkeni true olarak bir ileti gönderilirken bir sorun oluştu, ileti kullanıcının gördüğü ayarlanır.

    Bir hata iletisini gösteren sayfa bölümünde ek bir test fark: `if(debuggingFlag)`. Bu e-posta gönderme sorun yaşıyorsanız true olarak ayarlanan bir değişkendir. Zaman `debuggingFlag` true ise ve e-posta gönderilirken bir sorun varsa, e-posta göndermeye çalıştığında ne olursa olsun ASP.NET bildirdi gösteren bir ek hata iletisi görüntülenir. Orta, ancak Uyarı: bir e-posta iletisi gönderilemiyor ASP.NET raporları hata iletilerini genel olabilir. Örneğin, ASP.NET ile iletişim kuramıyor SMTP sunucusu (örneğin, bir hata sunucu adı yaptığınız için), hata `Failure sending mail`.

    > [!NOTE] 
    > 
    > **Önemli** bir özel durum nesnesinden bir hata iletisi ulaştığınızda (`ex` kodda), yapmak *değil* düzenli olarak bu iletiyi aracılığıyla kullanıcılara geçirin. Özel durum nesneleri genellikle kullanıcılar değil görmeniz gerekir ve bir güvenlik açığı bile olabilir bilgileri içerir. İşte bu değişkeni bu kodu içerir `debuggingFlag` hata iletisini görüntülemek için bir anahtar kullanılır ve neden değişkeni varsayılan olarak false olarak ayarlanmış. True (ve bu nedenle hata iletisini görüntülemek için), bu değişken ayarlamalısınız *yalnızca* e-posta gönderen bir sorun yaşıyoruz ve hata ayıklamak gerekiyorsa. Sorunları düzelttikten sonra ayarlayın `debuggingFlag` için false.

    Değişiklik şu e-posta kodunda ilgili ayarları:

   - Ayarlama `your-SMTP-host` erişiminiz SMTP sunucusunun adı.
   - Ayarlama `your-user-name-here` , SMTP sunucusu hesabının kullanıcı adı.
   - Ayarlama `your-account-password` , SMTP sunucusu hesabının parolasına.
   - Ayarlama `your-email-address-here` kendi e-posta adresi. İletinin gönderildiği e-posta adresidir. (Bazı e-posta sağlayıcılarının farklı bir belirtmenize izin vermeyin `From` adresi ve kullanıcı adınızı olarak kullanacağı `From` adresi.)

     > [!TIP] 
     > 
     > <a id="configuring_email_settings"></a>
     > ### <a name="configuring-email-settings"></a>E-posta ayarlarını yapılandırma
     > 
     > Bu, bazen bir SMTP sunucusu, bağlantı noktası numarası ve benzeri için doğru ayarları emin olmak için bir mücadele haline gelebilir. Burada birkaç ipucu verilmiştir:
     > 
     > - SMTP sunucusu adını genellikle gibi bir şeydir `smtp.provider.com` veya `smtp.provider.net`. Ancak, bir barındırma sağlayıcısına sitenizi yayımlayın, SMTP sunucusu adını bu noktada olabilir `localhost`. Bu durum, yayımladığınız sonra sitenizi sağlayıcıya ait sunucu üzerinde çalıştığından, e-posta sunucusu yerel uygulamanızı perspektifinden olabilir çünkü. Sunucu adları bu değişikliğe, yayımlama işleminin bir parçası SMTP sunucusu adını değiştirmek gerektiği anlamına gelebilir.
     > - Bağlantı noktası numarası genellikle 25'tir. Ancak, bazı sağlayıcılar 587 veya bazı başka bir bağlantı, bağlantı noktası kullanmayı gerektirir.
     > - Doğru kimlik bilgilerini kullandığınızdan emin olun. Bir barındırma sağlayıcısına sitenizi yayımladığınız sağlayıcısı, e-posta için özellikle belirtti kimlik bilgilerini kullanın. Bunlar, yayımlamak için kullandığınız kimlik bilgilerinizden farklı olabilir.
     > - Bazı durumlarda tüm kimlik bilgisi gerekmez. Kişisel ISS kullanarak e-posta gönderiyorsanız, e-posta sağlayıcınız kimlik bilgilerinizi zaten biliyor olabilirsiniz. Yayımladıktan sonra yerel bilgisayarınızda test ettiğinizde, farklı kimlik bilgileri kullanmanız gerekebilir.
     > - E-posta sağlayıcınız şifreleme kullanıyorsa, ayarlamak zorunda `WebMail.EnableSsl` için `true`.
4. Çalıştırma *EmailRequest.cshtml* sayfasını bir tarayıcıda. (Emin sayfanın içinde seçili **dosyaları** çalıştırmadan önce çalışma alanı.)
5. Adınızı ve sorunun açıklamasını girin ve ardından **Gönder** düğmesi. İçin yönlendirilirsiniz *ProcessRequest.cshtml* sayfasında, iletinizi doğrular ve bu, bir e-posta iletisi gönderir. 

    ![[image]](11-adding-email-to-your-web-site/_static/image2.jpg)

<a id="Sending_a_File"></a>
## <a name="sending-a-file-using-email"></a>Bir dosyayı e-posta ile gönderme

E-posta iletilerini eklenen dosyalar ayrıca gönderebilirsiniz. Bu yordamda, bir metin dosyası ve iki HTML sayfaları oluşturun. Metin dosyasını bir e-posta eki olarak kullanacaksınız.

1. Web sitesinde, yeni bir metin dosyası ekleyin ve adlandırın *MyFile.txt*.
2. Aşağıdaki metni kopyalayın ve dosyaya yapıştırın: 

    `Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.`
3. Adlı bir sayfa oluşturun *SendFile.cshtml* ve aşağıdaki işaretlemeyi ekleyin: 

    [!code-html[Main](11-adding-email-to-your-web-site/samples/sample3.html)]
4. Adlı bir sayfa oluşturun *ProcessFile.cshtml* ve aşağıdaki işaretlemeyi ekleyin: 

    [!code-cshtml[Main](11-adding-email-to-your-web-site/samples/sample4.cshtml)]
5. Değişiklik şu e-posta kod örneğindeki ilgili ayarları:

    - Ayarlama `your-SMTP-host` erişimi olan bir SMTP sunucusunun adı.
    - Ayarlama `your-user-name-here` , SMTP sunucusu hesabının kullanıcı adı.
    - Ayarlama `your-email-address-here` kendi e-posta adresi. İletinin gönderildiği e-posta adresidir.
    - Ayarlama `your-account-password` , SMTP sunucusu hesabının parolasına.
    - Ayarlama `target-email-address-here` kendi e-posta adresi. (Daha önce normalde bir e-posta başka bir kişiye gönderir, ancak test etmek için bunu kendinize gönderebilirsiniz gibi.)
6. Çalıştırma *SendFile.cshtml* sayfasını bir tarayıcıda.
7. Adınızı, bir konu satırı ve eklemek için metin dosyasının adını girin (*MyFile.txt*).
8. Tıklayın `Submit` düğmesi. Önce yönlendirilirsiniz gibi *ProcessFile.cshtml* iletinizi onaylar ve hangi gönderir, bir e-posta iletisine ekli dosya ile sayfası.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Ek Kaynaklar


- [ASP.NET Web Sayfaları (Razor) Sorun Giderme Kılavuzu](https://go.microsoft.com/fwlink/?LinkId=253001)
- [Basit Posta Aktarım Protokolü](https://msdn.microsoft.com/library/aa480435.aspx)
- [ASP.NET Web sayfaları için site geneline yönelik davranışını özelleştirme](https://go.microsoft.com/fwlink/?LinkId=202906)
