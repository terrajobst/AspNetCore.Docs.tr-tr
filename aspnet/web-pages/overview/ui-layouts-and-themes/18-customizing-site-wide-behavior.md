---
uid: web-pages/overview/ui-layouts-and-themes/18-customizing-site-wide-behavior
title: Site genelinde davranış (Razor) ASP.NET Web sayfaları için özelleştirme | Microsoft Docs
author: tfitzmac
description: Bu bölümde, Web sitenizin tamamını veya bir klasörün tamamına yerine yalnızca bir sayfa ayarları yapmak açıklanmaktadır.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: e158bed7-226f-4275-b02e-7553bd58c669
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/18-customizing-site-wide-behavior
msc.type: authoredcontent
ms.openlocfilehash: 4457318bcf1d2886eb8ed68fdd795eea7905368b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30899207"
---
<a name="customizing-site-wide-behavior-for-aspnet-web-pages-razor-sites"></a>ASP.NET Web sayfaları (Razor) sitesi için site genelinde davranışını özelleştirme
====================
tarafından [zel FitzMacken](https://github.com/tfitzmac)

> Bu makalede, bir ASP.NET Web sayfaları (Razor) Web sayfaları için site tarafı ayarlarını yapma açıklanmaktadır.
> 
> Öğrenecekleriniz:
> 
> - Kodu çalıştırmak için izin veren nasıl kümesi bir sitedeki tüm sayfalar için (global değerleri veya Yardımcısı ayarları) değerleri.
> - Bir klasördeki tüm sayfalar için değerleri ayarlamanıza olanak tanır kodu çalıştırmak nasıl.
> - Önce ve sonra bir sayfa kodu çalıştırmak nasıl yükler.
> - Bir merkezi hata sayfası hataları göndermek nasıl.
> - Bir klasördeki tüm sayfalar için kimlik doğrulaması ekleme yapma.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Öğreticide kullanılan yazılım sürümleri
> 
> 
> - ASP.NET Web sayfaları (Razor) 2
> - WebMatrix 3
> - ASP.NET Web Yardımcıları kitaplığı (NuGet paketi)
>   
> 
> Bu öğretici ASP.NET Web Pages 3 ile de çalışır ve Visual Studio 2013 (veya Visual Studio Express 2013 Web için) dışında ASP.NET Web Yardımcıları kitaplığı kullanamazsınız.


<a id="Adding_Website_Startup_Code"></a>
## <a name="adding-website-startup-code-for-aspnet-web-pages"></a>ASP.NET Web sayfaları için Web sitesi başlangıç kod ekleme

Bu sayfa için gerekli olan tüm kod çok ASP.NET Web Pages'da yazdığınız kodun belirli bir sayfayı içerebilir. Örneğin, bir sayfa bir e-posta iletisi gönderirse, bu işlem için tüm kod tek bir sayfayla put mümkündür. Bu e-posta göndermek için ayarları başlatmak için kod içerebilir (diğer bir deyişle, SMTP sunucusu için) ve e-posta iletisi göndermek için.

Ancak, bazı durumlarda, sitenin herhangi bir sayfasında çalışmadan önce bazı kodlar çalıştırmak isteyebilirsiniz. Bu sitede herhangi bir kullanılabilir değerler için faydalı (olarak adlandırılan *global değerleri*.) Örneğin, bazı Yardımcıları e-posta ayarları veya hesabı anahtarları gibi değerler sağlamanızı gerektirir. Bu ayarlar genel değerleri tutmak için kullanışlı olabilir.

Adlı bir sayfa oluşturarak bunu yapabilirsiniz  *\_AppStart.cshtml* sitenin kök. Bu sayfa varsa, sitedeki herhangi bir sayfayı istenen ilk kez çalışır. Bu nedenle, genel değerlerini ayarlamak için kodu çalıştırmak iyi yerdir. (Çünkü  *\_AppStart.cshtml* bir alt çizgi ön ekine sahip ASP.NET olmaz Gönder sayfa tarayıcıya kullanıcıların bunu doğrudan istemesi durumunda bile.)

Aşağıdaki diyagramda gösterildiği nasıl  *\_AppStart.cshtml* sayfasında çalışır. Bir sayfa için bir istek geldiğinde ve bu ise ilk istek herhangi sayfasında site, ASP.NET ilk denetimleri olup bir  *\_AppStart.cshtml* sayfa bulunmaktadır. Bu durumda, herhangi bir kod  *\_AppStart.cshtml* sayfasında çalıştırır ve ardından istenen sayfa çalıştırır.

![[Görüntü]](18-customizing-site-wide-behavior/_static/image1.jpg)

## <a name="setting-global-values-for-your-website"></a>Web siteniz için genel değerleri ayarlama

1. Bir WebMatrix Web sitesinin kök klasöründe adlı bir dosya oluşturun  *\_AppStart.cshtml*. Dosyayı sitenin kök dizininde olması gerekir.
2. Varolan içeriği aşağıdakiyle değiştirin: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample1.cshtml)]

    Bu kod bir değer depolar `AppState` sitesindeki tüm sayfalara otomatik olarak kullanılabilir sözlük. Dikkat  *\_AppStart.cshtml* dosya yok herhangi biçimlendirmesi içinde. Sayfa kodu çalıştırın ve başlangıçta istenen sayfasına yeniden yönlendir.

    > [!NOTE]
    > Kod geçirdiğinizde dikkatli olun  *\_AppStart.cshtml* dosya. Kod içinde herhangi bir hata oluşursa  *\_AppStart.cshtml* olmaz dosyası, Web sitesini Başlat.
3. Kök klasöründe adlı yeni bir sayfa oluşturma *AppName.cshtml*.
4. Varsayılan biçimlendirme ve kodun aşağıdakiyle değiştirin: 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample2.html)]

    Bu kod değerinden ayıklar `AppState` içinde ayarladığınız nesne  *\_AppStart.cshtml* sayfası.
5. Çalıştırma *AppName.cshtml* sayfasını bir tarayıcıda. (Emin olun sayfa seçildiğinde, **dosyaları** çalıştırmadan önce onu çalışma.) Sayfa genel değeri görüntüler. 

    ![[Görüntü]](18-customizing-site-wide-behavior/_static/image2.jpg)

<a id="Setting_Values_For_Helpers"></a>
## <a name="setting-values-for-helpers"></a>Ayar değerleri için Yardımcıları

İçin iyi bir kullanım  *\_AppStart.cshtml* dosyasıdır sitenizdeki kullanan ve sahip başlatılması Yardımcıları değerlerini ayarlamak için. Tipik örnekleridir e-posta ayarlarını `WebMail` Yardımcısı ve özel ve ortak anahtarları `ReCaptcha` Yardımcısı. Bu gibi durumlarda, bir kez değerler ayarlayabileceğiniz  *\_AppStart.cshtml* ve sonra bunlar zaten tüm sayfalar için sitenizdeki hazırsınız.

Bu yordam nasıl ayarlanacağını gösterir `WebMail` ayarları genel. (Kullanma hakkında daha fazla bilgi için `WebMail` Yardımcısı, bkz: [e-posta bir ASP.NET Web sayfaları Site Ekleme](../getting-started/11-adding-email-to-your-web-site.md).)

1. ASP.NET Web Yardımcıları kitaplığı açıklandığı gibi Web sitenize ekleyin [yükleme Yardımcıları bir ASP.NET Web Pages sitesinde](https://go.microsoft.com/fwlink/?LinkId=252372), onu zaten eklemediniz.
2. Henüz yoksa bir  *\_AppStart.cshtml* dosya, bir Web sitesinin kök klasöründe adlı bir dosya oluşturun  *\_AppStart.cshtml*.
3. Aşağıdakileri ekleyin `WebMail` ayarlar  *\_AppStart.cshtml* dosyası: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample3.cshtml?highlight=2-7)]

    Değiştirme aşağıdaki e-posta kodu ilgili ayarları:

   - Ayarlama `your-SMTP-host` erişiminiz SMTP sunucusunun adı.
   - Ayarlama `your-user-name-here` SMTP sunucusu hesabının kullanıcı adı.
   - Ayarlama `your-account-password` SMTP sunucusu hesabının parolasına.
   - Ayarlama `your-email-address-here` kendi e-posta adresi. Bu, ileti gönderilen e-posta adresidir. (Bazı e-posta sağlayıcısı farklı bir belirtmenize izin vermeyin `From` adres ve kullanıcı adı olarak kullanacağı `From` adresi.)

     SMTP ayarları hakkında daha fazla bilgi için bkz: [e-posta ayarlarını yapılandırma](https://go.microsoft.com/fwlink/?LinkID=202899#configuring_email_settings) makalede [bir ASP.NET Web sayfaları (Razor) sitesinden e-posta gönderme](https://go.microsoft.com/fwlink/?LinkID=202899) ve [gönderme epostaileilgilisorunları](https://go.microsoft.com/fwlink/?LinkId=253001#email)içinde [ASP.NET Web sayfaları (Razor) sorun giderme kılavuzu](https://go.microsoft.com/fwlink/?LinkId=253001).
4. Kaydet  *\_AppStart.cshtml* dosya ve kapatın.
5. Adlı yeni bir sayfa bir Web sitesinin kök klasörü oluşturmak *TestEmail.cshtml*.
6. Varolan içeriği aşağıdakiyle değiştirin: 

     [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample4.cshtml)]
7. Çalıştırma *TestEmail.cshtml* sayfasını bir tarayıcıda.
8. Kendinize bir e-posta iletisi gönderin ve ardından alanları doldurun **Gönder**.
9. İleti kabulünüzü emin olmak için e-postanızı kontrol edin.

Bu örnek önemli bir parçası, genellikle değişmez ayarları olan — adını, SMTP sunucunuza ve e-posta kimlik bilgilerinizi ister — ayarlanır  *\_AppStart.cshtml* dosya. Böylece bunları burada size e-posta göndermek yeniden her sayfasında ayarlamanız gerekmez. (Bazı nedenlerden dolayı bu ayarları değiştirmeniz gerekiyorsa, bunları tek tek bir sayfada ayarlayabilirsiniz rağmen.) Sayfasında, yalnızca genellikle alıcı ve e-posta iletisinin gövdesi gibi her zaman değiştirme değerlerini ayarlayın.

<a id="Running_Code_Before_and_After"></a>
## <a name="running-code-before-and-after-files-in-a-folder"></a>Önce ve sonra bir klasördeki dosyaları çalışan kod

Hemen kullanabileceğiniz gibi  *\_AppStart.cshtml* sitedeki sayfalar çalıştırmadan önce kod yazmak için önce (ve sonra) çalışan bir kod yazabilirsiniz çalıştırmak belirli bir klasörde herhangi bir sayfayı. Bu, bir kullanıcı bir sayfa klasöründe çalıştırmadan önce oturum aynı düzeni sayfa bir klasördeki tüm sayfalar için veya denetleme ayarlama gibi şeyler için kullanışlıdır.

Sayfalar için özellikle klasörleri kod adındaki bir dosyada oluşturabileceğiniz  *\_PageStart.cshtml*. Aşağıdaki diyagramda gösterildiği nasıl  *\_PageStart.cshtml* sayfasında çalışır. Bir istek için bir sayfa geldiğinde, ASP.NET ilk denetler bir  *\_AppStart.cshtml* sayfasında ve çalıştıran. ASP.NET olup olmadığını denetler sonra bir  *\_PageStart.cshtml* sayfasında ve Öyleyse, çalıştırır. Ardından, istenen sayfa çalıştırır.

İçinde  *\_PageStart.cshtml* sayfasında, istenen sayfanın dahil ederek çalıştırmasına istediğiniz işleme sırasında nerede belirtebilirsiniz bir `RunPage` yöntemi. Bu, istenen sayfa çalışmadan önce kodu çalıştırmak sağlar ve daha sonra yeniden sonra. Eklemezseniz, `RunPage`, tüm kodda  *\_PageStart.cshtml* çalıştırır ve istenen sayfa çalışır otomatik olarak.

![[Görüntü]](18-customizing-site-wide-behavior/_static/image3.jpg)

ASP.NET hiyerarşisini oluşturmanıza olanak tanır  *\_PageStart.cshtml* dosyaları. Koyabilirsiniz bir  *\_PageStart.cshtml* sitesinin kök ve herhangi bir alt dosya. Bir sayfa istendiğinde  *\_PageStart.cshtml* en üst düzey (en yakın bir site kökünde) çalışır ve ardından, dosyayı  *\_PageStart.cshtml* sonraki dosyasında İstenen sayfaya içeren klasörü istek ulaşana kadar vb. alt yapısı aşağı alt. Tüm geçerli sonra  *\_PageStart.cshtml* dosyaları çalıştırıp, istenen sayfa çalıştırılır.

Örneğin, aşağıdaki bileşimi olabilir  *\_PageStart.cshtml* dosyaları ve *Default.cshtml* dosyası:

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample5.cshtml)]

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample6.cshtml)]

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample7.cshtml)]

Çalıştırdığınızda */myfolder/default.cshtml*, aşağıdaki görürsünüz:

[!code-console[Main](18-customizing-site-wide-behavior/samples/sample8.cmd)]

## <a name="running-initialization-code-for-all-pages-in-a-folder"></a>Bir klasördeki tüm sayfalar için başlatma kodu çalıştırma

İçin iyi bir kullanım  *\_PageStart.cshtml* dosyaları olan tek bir klasördeki tüm dosyalar aynı düzen sayfasını başlatılamadı.

1. Kök klasöründe adlı yeni bir klasör oluşturun *InitPages*.
2. İçinde *InitPages* Web sitenizin klasör adında bir dosya oluşturun  *\_PageStart.cshtml* ve varsayılan biçimlendirme ve kodun şununla değiştirin: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample9.cshtml)]
3. Web sitesinin kök dizininde adlı bir klasör oluşturun *paylaşılan*.
4. İçinde *paylaşılan* klasör adında bir dosya oluşturun  *\_Layout1.cshtml* ve varsayılan biçimlendirme ve kodun şununla değiştirin: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample10.cshtml)]
5. İçinde *InitPages* klasör adında bir dosya oluşturun *Content1.cshtml* ve varolan içeriği şununla değiştirin: 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample11.html)]
6. İçinde *InitPages* klasör adında başka bir dosya oluşturun *Content2.cshtml* ve varsayılan biçimlendirme şununla değiştirin: 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample12.html)]
7. Çalıştırma *Content1.cshtml* bir tarayıcıda. 

    ![[Görüntü]](18-customizing-site-wide-behavior/_static/image4.jpg)

    Zaman *Content1.cshtml* sayfa çalıştığında,  *\_PageStart.cshtml* dosya kümeleri `Layout` ve ayrıca ayarlar `PageData["MyBackground"]` bir renk. İçinde *Content1.cshtml*, rengini ve düzenini uygulanır.
8. Görüntü *Content2.cshtml* bir tarayıcıda. 

    Her iki sayfa içinde başlatılan gibi aynı düzen sayfasını ve renk kullan düzeni aynı olduğundan  *\_PageStart.cshtml*.

## <a name="using-pagestartcshtml-to-handle-errors"></a>Kullanarak \_hataları işlemek için PageStart.cshtml

Başka bir iyi kullanmak için  *\_PageStart.cshtml* dosyasıdır (özel durumlar) birinde oluşabilir programlama hataları işlemek için bir yol oluşturmak için *.cshtml* bir klasörde sayfa. Bu örnek, bunu yapmanın bir yolu gösterir.

1. Kök klasöründe adlı bir klasör oluşturun *InitCatch*.
2. İçinde *InitCatch* Web sitenizin klasör adında bir dosya oluşturun  *\_PageStart.cshtml* ve varolan biçimlendirme ve kodun şununla değiştirin: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample13.cshtml)]

    Bu kodda, istenen sayfa açıkça çağırarak çalıştırmayı deneyin. `RunPage` yönteminin içinde bir `try` bloğu. Herhangi bir programlama hata istenen oluşursa sayfası, içinde kod `catch` engelleme çalıştırır. Bu durumda, kodu bir sayfasına yönlendirir (*Error.cshtml*) ve URL parçası olarak bir hatayla karşılaştı dosyasının adını geçirir. (Kısa süre içinde sayfa oluşturacaksınız.)
3. İçinde *InitCatch* Web sitenizin klasör adında bir dosya oluşturun *Exception.cshtml* ve varolan biçimlendirme ve kodun şununla değiştirin: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample14.cshtml)]

    Bu örneğin amaçları doğrultusunda, bu sayfada gerçekleştirmekte olduğunuz kasıtlı olarak bir hata var olmayan bir veritabanı dosyasını açmaya çalışırken tarafından oluşturuyor.
4. Kök klasöründe adlı bir dosya oluşturun *Error.cshtml* ve varolan biçimlendirme ve kodun şununla değiştirin: 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample15.html)]

    Bu sayfadaki ifade `@Request["source"]` URL dışında bir değer alır ve görüntüler.
5. Araç çubuğunda **kaydetmek**.
6. Çalıştırma *Exception.cshtml* bir tarayıcıda. 

    ![[Görüntü]](18-customizing-site-wide-behavior/_static/image5.jpg)

    Bir hata oluştuğundan *Exception.cshtml*,  *\_PageStart.cshtml* sayfa yeniden yönlendirmeleri için *Error.cshtml* iletisini görüntüler dosya.

    Özel durumlar hakkında daha fazla bilgi için bkz: [ASP.NET Web sayfalarını programlama kullanarak Razor sözdizimi giriş](https://go.microsoft.com/fwlink/?LinkID=251587).

<a id="Using__PageStart.cshtml_to_Restrict_Folder_Access"></a>
## <a name="using-pagestartcshtml-to-restrict-folder-access"></a>Kullanarak \_PageStart.cshtml klasörüne erişimi kısıtlama

Aynı zamanda  *\_PageStart.cshtml* bir klasördeki tüm dosyalara erişimi kısıtlamak için dosya.

1. Kullanarak yeni bir Web sitesini Webmatrix'te oluşturmak **Site şablondan** seçeneği.
2. Kullanılabilir şablonları arasından seçim **başlangıç sitesi**.
3. Kök klasöründe adlı bir klasör oluşturun *AuthenticatedContent*.
4. İçinde *AuthenticatedContent* klasör adında bir dosya oluşturun  *\_PageStart.cshtml* ve varolan biçimlendirme ve kodun şununla değiştirin: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample16.cshtml)]

    Kod, klasördeki tüm dosyaları önbelleğe alınması engelleyerek başlatır. (Bu ortak bilgisayarlar, bir kullanıcının önbelleğe alınan sayfaları sonraki kullanıcı için kullanılabilir olmasını istediğiniz yok gibi senaryolar için gereklidir.) Ardından, kod sayfalar klasöründe görüntüleyebilmeniz için önce kullanıcı siteye oturum açtığı olup olmadığını belirler. Kullanıcı imzalı değilse, kod oturum açma sayfasına yönlendirir. Oturum açma sayfasına kullanıcı adlı bir sorgu dizesi değeri eklerseniz, başlangıçta istenen sayfaya dönmek `ReturnUrl`.
5. Yeni bir sayfa oluşturma *AuthenticatedContent* adlı klasörü *Page.cshtml*.
6. Varsayılan biçimlendirme aşağıdakiyle değiştirin:  

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample17.cshtml)]
7. Çalıştırma *Page.cshtml* bir tarayıcıda. Kod bir oturum açma sayfasına yönlendirir. Oturum açmayı önce kaydetmeniz gerekir. Kayıtlı ve oturum açtıktan sonra sayfasına gidin ve içeriğini görüntüleyin.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Ek Kaynaklar

[ASP.NET Web sayfaları Razor sözdizimini kullanarak programlama giriş](https://go.microsoft.com/fwlink/?LinkID=251587)
