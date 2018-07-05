---
uid: web-pages/overview/ui-layouts-and-themes/18-customizing-site-wide-behavior
title: ASP.NET Web sayfaları (Razor) siteler için Site geneline yönelik davranışını özelleştirme | Microsoft Docs
author: tfitzmac
description: Bu bölümde, tüm Web sitenizin veya klasörün tamamını yerine yalnızca bir sayfa ayarlarını yapma açıklanmaktadır.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: e158bed7-226f-4275-b02e-7553bd58c669
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/18-customizing-site-wide-behavior
msc.type: authoredcontent
ms.openlocfilehash: 3930c1cceb86e4105463323e0896d7491af5ae56
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37391938"
---
<a name="customizing-site-wide-behavior-for-aspnet-web-pages-razor-sites"></a>ASP.NET Web sayfaları (Razor) siteler için site geneline yönelik davranışını özelleştirme
====================
tarafından [Tom FitzMacken](https://github.com/tfitzmac)

> Bu makalede, bir ASP.NET Web sayfaları (Razor) Web sitesinde site tarafında ayarları sayfaları için nasıl açıklanmaktadır.
> 
> Öğrenecekleriniz:
> 
> - Kodu çalıştırmak için olanak tanıyan nasıl kümesi bir sitedeki tüm sayfalar için (genel değerleri veya Yardımcısı ayarları) değerleri.
> - Bir klasördeki tüm sayfalar için değerleri ayarlamanıza olanak tanıyan kodu çalıştırmayı öğrenin.
> - Kod önce ve sonra bir sayfa yükleri çalıştırmayı öğrenin.
> - Bir merkezi hata sayfasına hataları göndermek nasıl.
> - Bir klasördeki tüm sayfalar için kimlik doğrulaması ekleme yapma.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Bu öğreticide kullanılan yazılım sürümleri
> 
> 
> - ASP.NET Web sayfaları (Razor) 2
> - WebMatrix 3
> - ASP.NET Web Yardımcıları kitaplığı (NuGet paketi)
>   
> 
> Bu öğretici, ASP.NET Web Pages 3 ile de çalışır ve Visual Studio 2013 (veya Visual Studio Express 2013 Web için) dışında ASP.NET Web Yardımcıları kitaplığı kullanamazsınız.


<a id="Adding_Website_Startup_Code"></a>
## <a name="adding-website-startup-code-for-aspnet-web-pages"></a>ASP.NET Web sayfaları için Web sitesi başlangıç kod ekleme

ASP.NET Web sayfaları'nda yazdığınız kodun çoğu için tek tek sayfaları, sayfa için gereken tüm kod içerebilir. Örneğin, bir sayfa bir e-posta iletisi gönderirse, bu işlem için tüm kod içinde tek bir sayfaya yerleştirin mümkündür. Bu e-posta göndermek için ayarları başlatmak için kodu içerebilir (diğer bir deyişle, SMTP sunucusu için) ve e-posta iletisi göndermek için.

Ancak, bazı durumlarda, sitenin herhangi bir sayfada çalışmadan önce bazı kodlar çalıştırmak isteyebilirsiniz. Herhangi bir sitede kullanılabilir değerleri ayarlamak için kullanışlıdır (olarak adlandırılan *genel değerleri*.) Örneğin, bazı Yardımcıları e-posta ayarları veya hesap anahtarları gibi değerler sağlamanızı gerektirir. Genel değerleri bu ayarları saklamak için kullanışlı olabilir.

Adlı bir sayfa oluşturarak bunu yapabilirsiniz  *\_AppStart.cshtml* sitenin kök. Bu sayfa varsa, sitedeki herhangi bir sayfa istenen ilk kez çalıştırır. Bu nedenle, genel değerleri ayarlamak üzere kod çalıştırmak iyi yerdir. (Çünkü  *\_AppStart.cshtml* bir alt çizgi ön ekine sahip ASP.NET olmaz Gönder sayfa tarayıcıya doğrudan isteyen kullanıcılar bile.)

Aşağıdaki diyagramda gösterildiği nasıl  *\_AppStart.cshtml* sayfasında çalışır. Bir sayfa için bir istek halinde sunulur ve bu ilk istek için tüm sayfasında sitede ASP.NET ilk denetimleri olup bir  *\_AppStart.cshtml* sayfa bulunmaktadır. Bu durumda, tüm kod  *\_AppStart.cshtml* sayfasında çalıştırır ve ardından istenen sayfa çalıştırır.

![[image]](18-customizing-site-wide-behavior/_static/image1.jpg)

## <a name="setting-global-values-for-your-website"></a>Web siteniz için genel değerlerini ayarlama

1. Bir WebMatrix Web sitesinin kök klasöründe adlı bir dosya oluşturun  *\_AppStart.cshtml*. Dosya, sitenin kök dizininde olması gerekir.
2. Mevcut içeriğini aşağıdakiyle değiştirin: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample1.cshtml)]

    Bu kod içinde bir değer depolar `AppState` sözlük, sitedeki tüm sayfalar için otomatik olarak kullanılabilir. Dikkat  *\_AppStart.cshtml* dosyasında yok tüm biçimlendirme içinde. Sayfa kodu çalıştırın ve ardından Başlangıçta istenen sayfasına yönlendir.

    > [!NOTE]
    > Kod yerleştirdiğinizde dikkatli olun  *\_AppStart.cshtml* dosya. Kodda herhangi bir hata oluşursa  *\_AppStart.cshtml* olmaz, dosya, Web sitesini başlatmak.
3. Kök klasöründe adlı yeni bir sayfa oluşturmak *AppName.cshtml*.
4. Varsayılan biçimlendirme ve kodu aşağıdakiyle değiştirin: 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample2.html)]

    Bu kodu değerini ayıklar `AppState` ayarladığınız nesne  *\_AppStart.cshtml* sayfası.
5. Çalıştırma *AppName.cshtml* sayfasını bir tarayıcıda. (Emin sayfanın içinde seçili **dosyaları** çalıştırmadan önce çalışma alanı.) Sayfa genel değeri görüntüler. 

    ![[image]](18-customizing-site-wide-behavior/_static/image2.jpg)

<a id="Setting_Values_For_Helpers"></a>
## <a name="setting-values-for-helpers"></a>Ayar değerleri Yardımcıları

İçin iyi bir kullanımı  *\_AppStart.cshtml* sitenizdeki kullanan ve başlatılacak olan Yardımcıları değerlerini ayarlamak için dosyasıdır. Tipik örnekleridir e-posta ayarlarını `WebMail` Yardımcısı ve özel ve ortak anahtarlarını `ReCaptcha` Yardımcısı. Bu gibi durumlarda, bir kez değerler ayarlayabileceğiniz  *\_AppStart.cshtml* ve sonra bunlar henüz tüm sayfalar için sitenizde.

Bu yordamı nasıl ayarlanacağını gösterir `WebMail` ayarları genel. (Kullanma hakkında daha fazla bilgi için `WebMail` yardımcıyı bkz [ekleme e-posta bir ASP.NET Web sayfaları sitesinde](../getting-started/11-adding-email-to-your-web-site.md).)

1. ASP.NET Web Yardımcıları kitaplığı açıklandığı Web sitenize ekleyin [yükleme Yardımcıları bir ASP.NET Web sayfaları sitesinde](https://go.microsoft.com/fwlink/?LinkId=252372), bunu zaten eklemediniz.
2. Henüz yoksa bir  *\_AppStart.cshtml* dosya, bir Web sitesinin kök klasöründe adlı bir dosya oluşturun  *\_AppStart.cshtml*.
3. Aşağıdaki `WebMail` ayarlar  *\_AppStart.cshtml* dosyası: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample3.cshtml?highlight=2-7)]

    Değişiklik şu e-posta kodunda ilgili ayarları:

   - Ayarlama `your-SMTP-host` erişiminiz SMTP sunucusunun adı.
   - Ayarlama `your-user-name-here` , SMTP sunucusu hesabının kullanıcı adı.
   - Ayarlama `your-account-password` , SMTP sunucusu hesabının parolasına.
   - Ayarlama `your-email-address-here` kendi e-posta adresi. İletinin gönderildiği e-posta adresidir. (Bazı e-posta sağlayıcılarının farklı bir belirtmenize izin vermeyin `From` adresi ve kullanıcı adınızı olarak kullanacağı `From` adresi.)

     SMTP ayarları hakkında daha fazla bilgi için bkz. [e-posta ayarlarını yapılandırma](https://go.microsoft.com/fwlink/?LinkID=202899#configuring_email_settings) makaledeki [bir ASP.NET Web sayfaları (Razor) sitesinde e-posta gönderme](https://go.microsoft.com/fwlink/?LinkID=202899) ve [epostagönderirkensorunlarla](https://go.microsoft.com/fwlink/?LinkId=253001#email)içinde [ASP.NET Web sayfaları (Razor) sorun giderme kılavuzu](https://go.microsoft.com/fwlink/?LinkId=253001).
4. Kaydet  *\_AppStart.cshtml* dosya ve kapatın.
5. Bir Web sitesinin kök klasöründe adlı yeni sayfa oluşturma *TestEmail.cshtml*.
6. Mevcut içeriğini aşağıdakiyle değiştirin: 

     [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample4.cshtml)]
7. Çalıştırma *TestEmail.cshtml* sayfasını bir tarayıcıda.
8. Kendinize bir e-posta iletisi gönderin ve ardından alanları doldurun **Gönder**.
9. İleti yönettiniz emin olmak için e-postanızı kontrol edin.

Bu örnek önemli bir parçası olan genellikle değişmez ayarları — adını, SMTP sunucunuza ve e-posta kimlik bilgilerinizi ister — ayarlanan  *\_AppStart.cshtml* dosya. Böylece onları burada size e-posta göndermek yeniden her sayfanın ayarlamanız gerekmez. (Herhangi bir nedenden dolayı bu ayarları değiştirmek istiyorsanız, bunları ayrı ayrı bir sayfada ayarlayabilirsiniz rağmen) Sayfasında, yalnızca genellikle, alıcı ve e-posta iletisinin gövdesini gibi değiştirdiğiniz değerleri ayarlayın.

<a id="Running_Code_Before_and_After"></a>
## <a name="running-code-before-and-after-files-in-a-folder"></a>Önce ve sonra bir klasördeki dosyaları kod çalıştırma

Hemen kullanabileceğiniz gibi  *\_AppStart.cshtml* sitedeki sayfaların çalıştırmadan önce kodu yazmak için önce (ve sonra) çalışan kod yazabilirsiniz. herhangi bir sayfasında belirli bir klasörde çalıştırın. Bu, bir kullanıcı bir sayfa klasöründe çalıştırmadan önce oturum aynı düzen sayfası denetimi için veya bir klasördeki tüm sayfalar için ayarlama gibi şeyler için kullanışlıdır.

Sayfaları için özellikle klasörlerin kod adındaki bir dosyada oluşturabileceğiniz  *\_PageStart.cshtml*. Aşağıdaki diyagramda gösterildiği nasıl  *\_PageStart.cshtml* sayfasında çalışır. Bir istek için bir sayfa geldiğinde, ASP.NET ilk denetleyen bir  *\_AppStart.cshtml* sayfasında ve çalıştırılan. ASP.NET olup olmadığını denetler. ardından bir  *\_PageStart.cshtml* sayfasında ve bu durumda, çalıştırır. Ardından, istenen sayfa çalıştırır.

İçinde  *\_PageStart.cshtml* sayfasında, dahil ederek çalıştırmak için istenen sayfayı istediğiniz işleme sırasında nerede belirtebilirsiniz bir `RunPage` yöntemi. Bu, istenen sayfa çalışmadan önce kod çalıştırmanıza olanak sağlar ve ardından tekrar sonra. Dahil etmezseniz `RunPage`, tüm kodu  *\_PageStart.cshtml* çalıştırır ve istenen sayfa çalışır otomatik olarak.

![[image]](18-customizing-site-wide-behavior/_static/image3.jpg)

ASP.NET hiyerarşisini oluşturmanıza olanak tanıyan  *\_PageStart.cshtml* dosyaları. Koyabilirsiniz bir  *\_PageStart.cshtml* kök sitenin ve tüm alt klasörlerdeki dosya. Bir sayfa istendiğinde  *\_PageStart.cshtml* ardından en üst düzey (en yakın bir site kökünde) çalıştığında, dosyayı  *\_PageStart.cshtml* sonraki dosya istenen sayfayı içeren klasöre istek ulaşana kadar vb. alt yapısı aşağı alt. Tüm geçerli sonra  *\_PageStart.cshtml* dosyaları çalıştırın, istenen sayfa çalıştırır.

Örneğin, aşağıdaki birleşimi olabilir  *\_PageStart.cshtml* dosyaları ve *Default.cshtml* dosyası:

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample5.cshtml)]

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample6.cshtml)]

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample7.cshtml)]

Çalıştırdığınızda */myfolder/default.cshtml*, aşağıdaki görüntüyle karşılaşırsınız:

[!code-console[Main](18-customizing-site-wide-behavior/samples/sample8.cmd)]

## <a name="running-initialization-code-for-all-pages-in-a-folder"></a>Bir klasördeki tüm sayfalar için başlatma kodunu çalıştırma

İçin iyi bir kullanımı  *\_PageStart.cshtml* dosyaları olan tek bir klasördeki tüm dosyaları aynı düzen sayfasını başlatmak için.

1. Kök klasöründe adlı yeni bir klasör oluşturun *InitPages*.
2. İçinde *InitPages* Web sitenizin, klasör adında bir dosya oluşturun  *\_PageStart.cshtml* ve varsayılan biçimlendirme ve kodu aşağıdakiyle değiştirin: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample9.cshtml)]
3. Web sitesinin kök dizininde adlı bir klasör oluşturun *paylaşılan*.
4. İçinde *paylaşılan* klasöründe adlı bir dosya oluşturun  *\_Layout1.cshtml* ve varsayılan biçimlendirme ve kodu aşağıdakiyle değiştirin: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample10.cshtml)]
5. İçinde *InitPages* klasöründe adlı bir dosya oluşturun *Content1.cshtml* ve varolan içeriği aşağıdakiyle değiştirin: 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample11.html)]
6. İçinde *InitPages* klasörü, adlı başka bir dosya oluşturun *Content2.cshtml* ve varsayılan biçimlendirme aşağıdakiyle değiştirin: 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample12.html)]
7. Çalıştırma *Content1.cshtml* bir tarayıcıda. 

    ![[image]](18-customizing-site-wide-behavior/_static/image4.jpg)

    Zaman *Content1.cshtml* sayfa çalıştığında,  *\_PageStart.cshtml* dosya kümelerini `Layout` ve ayrıca ayarlar `PageData["MyBackground"]` bir renk. İçinde *Content1.cshtml*, Düzen ve renk uygulanır.
8. Görüntü *Content2.cshtml* bir tarayıcıda. 

    Her iki sayfa içinde başlatılan gibi aynı düzen sayfasını ve renk kullan düzenini aynı olduğundan  *\_PageStart.cshtml*.

## <a name="using-pagestartcshtml-to-handle-errors"></a>Kullanarak \_PageStart.cshtml hatalarını işleme

Başka bir iyi kullanım için  *\_PageStart.cshtml* dosyasıdır (özel) birinde gerçekleşebilir programlama hatalarını işlemek için bir yol oluşturmak için *.cshtml* bir klasördeki sayfaya. Bu örnekte, bunu yapmanın bir yolu gösterir.

1. Kök klasöründe adlı bir klasör oluşturun *InitCatch*.
2. İçinde *InitCatch* Web sitenizin, klasör adında bir dosya oluşturun  *\_PageStart.cshtml* ve mevcut işaretleme ve kodu aşağıdakiyle değiştirin: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample13.cshtml)]

    Bu kodda, istenen sayfa açıkça çağrılarak çalıştırmayı deneyin. `RunPage` yöntem içinde bir `try` blok. İstenen herhangi bir programlama hata oluşursa sayfası, kod içinde `catch` bloğu çalışır. Bu durumda, kodu bir sayfasına yönlendirir (*Error.cshtml*) ve URL parçası olarak bir hatayla karşılaştı dosyasının adını geçirir. (Kısa bir süre sonra sayfayı oluşturacaksınız.)
3. İçinde *InitCatch* Web sitenizin, klasör adında bir dosya oluşturun *Exception.cshtml* ve mevcut işaretleme ve kodu aşağıdakiyle değiştirin: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample14.cshtml)]

    Bu örneğin amacı doğrultusunda, bu sayfada ne yaptığınızı kasıtlı olarak bir hata var olmayan bir veritabanı dosyasını açmaya çalışarak oluşturuyor.
4. Kök klasöründe adlı bir dosya oluşturun *Error.cshtml* ve mevcut işaretleme ve kodu aşağıdakiyle değiştirin: 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample15.html)]

    Bu sayfada, ifade `@Request["source"]` URL dışında bir değer alır ve görüntüler.
5. Araç çubuğunda **Kaydet**.
6. Çalıştırma *Exception.cshtml* bir tarayıcıda. 

    ![[image]](18-customizing-site-wide-behavior/_static/image5.jpg)

    Bir hata oluştuğundan *Exception.cshtml*,  *\_PageStart.cshtml* sayfasına yeniden yönlendirildiği *Error.cshtml* dosyasını şu ileti görüntülenir.

    Özel durumları hakkında daha fazla bilgi için bkz. [ASP.NET Web sayfaları programlama kullanarak Razor sözdizimi giriş](https://go.microsoft.com/fwlink/?LinkID=251587).

<a id="Using__PageStart.cshtml_to_Restrict_Folder_Access"></a>
## <a name="using-pagestartcshtml-to-restrict-folder-access"></a>Kullanarak \_PageStart.cshtml klasör erişimi kısıtlamak için

Ayrıca  *\_PageStart.cshtml* bir klasördeki tüm dosyalara erişimi kısıtlamak için dosya.

1. Webmatrix'te, kullanarak yeni bir Web sitesi oluşturma **Site şablondan** seçeneği.
2. Kullanılabilen şablonlardan birini seçin **başlangıç sitesi**.
3. Kök klasöründe adlı bir klasör oluşturun *AuthenticatedContent*.
4. İçinde *AuthenticatedContent* klasöründe adlı bir dosya oluşturun  *\_PageStart.cshtml* ve mevcut işaretleme ve kodu aşağıdakiyle değiştirin: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample16.cshtml)]

    Kod, klasördeki tüm dosyaları önbelleğe alınmasını engelleyerek başlatır. (Bu burada bir kullanıcıya ait önbelleğe alınan sayfaları ileri kullanıcılar için uygun olmasını istemediğiniz açık bilgisayarlar gibi senaryolar için gereklidir.) Ardından, kod ve sayfaların herhangi birinin klasöründe görüntülemeden önce kullanıcı siteye oturum açan olup olmadığını belirler. Kullanıcı imzalı değilse, kod oturum açma sayfasına yönlendirir. Oturum açma sayfası kullanıcı adında bir sorgu dizesi değerini içeriyorsa, başlangıçta istenen sayfaya geri `ReturnUrl`.
5. Yeni bir sayfa oluşturmak *AuthenticatedContent* adlı klasöre *Page.cshtml*.
6. Varsayılan biçimlendirme aşağıdakiyle değiştirin:  

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample17.cshtml)]
7. Çalıştırma *Page.cshtml* bir tarayıcıda. Kod bir oturum açma sayfasına yönlendirir. Oturum açma önce kaydetmeniz gerekir. Kayıtlı ve oturum açtıktan sonra sayfasına gidin ve içeriği görüntüleyin.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Ek Kaynaklar

[ASP.NET Web sayfaları programlama Razor söz dizimini kullanarak giriş](https://go.microsoft.com/fwlink/?LinkID=251587)
