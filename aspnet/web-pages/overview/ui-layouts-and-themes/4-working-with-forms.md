---
uid: web-pages/overview/ui-layouts-and-themes/4-working-with-forms
title: ASP.NET Web sayfaları (Razor) siteleri HTML formları ile çalışma | Microsoft Docs
author: tfitzmac
description: Metin kutuları, onay kutuları, radyo düğmeleri ve aşağı açılır listeleri gibi kullanıcı girişi denetimleri yerleştirdiğiniz yere bir bölümü bir HTML belgesinin biçimidir. Formları kullanma ne...
ms.author: aspnetcontent
ms.date: 02/10/2014
ms.assetid: f3f4b8c8-e8f6-4474-ad94-69228a6c01ee
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/4-working-with-forms
msc.type: authoredcontent
ms.openlocfilehash: 4658e80d63c9c3665f197071f3f3d1420362e4a3
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820291"
---
<a name="working-with-html-forms-in-aspnet-web-pages-razor-sites"></a>ASP.NET Web sayfaları (Razor) sitesinde HTML formlarla çalışma
====================
tarafından [Tom FitzMacken](https://github.com/tfitzmac)

> Bu makalede bir HTML formuna (ile metin kutuları ve düğmeler) işlemek nasıl bir ASP.NET Web sayfaları (Razor) Web sitesinde çalışırken.
> 
> **Öğrenecekleriniz:** 
> 
> - Bir HTML formu oluşturma
> - Kullanıcı Giriş formundan okumak nasıl.
> - Kullanıcı girişini doğrulama yapma.
> - Form değerleri, sayfa gönderildikten sonra geri yükleme.
> 
> Programlama Kavramları makalesinde sunulan ASP.NET şunlardır:
> 
> - `Request` Nesne.
> - Giriş doğrulaması.
> - HTML kodlaması.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Bu öğreticide kullanılan yazılım sürümleri
> 
> 
> - ASP.NET Web sayfaları (Razor) 3
>   
> 
> Bu öğreticide, ASP.NET Web Pages 2 ile de çalışır.


## <a name="creating-a-simple-html-form"></a>Basit bir HTML formu oluşturma

1. Yeni bir Web sitesi oluşturun.
2. Kök klasöründe bir web sayfası oluşturma *Form.cshtml* ve aşağıdaki biçimlendirme girin:

    [!code-html[Main](4-working-with-forms/samples/sample1.html)]
3. Sayfanın tarayıcıda başlatın. (Webmatrix'te, içinde **dosyaları** çalışma alanında, dosyaya sağ tıklayın ve ardından **tarayıcıda Başlat**.) Üç giriş alanlarını basit bir formla ve **Gönder** düğmesi görüntülenir.

    ![Üç metin kutusu ile bir formun ekran görüntüsü.](4-working-with-forms/_static/image1.jpg)

    Tıklarsanız bu noktada **Gönder** düğmesi, hiçbir şey olmaz. Form kullanışlı hale getirmek için sunucuda çalıştırılacak bazı kod eklemeniz gerekir.

## <a name="reading-user-input-from-the-form"></a>Kullanıcı giriş formu okuma

Formu işlemeye gönderilen alan değerlerini okur ve bunları ile bir sorun mu kodu ekleyin. Bu yordam okunur alanlar ve kullanıcı girişi sayfada görüntülemek nasıl gösterir. (Bir üretim uygulamasında, genellikle kullanıcı girişi ile daha ilgi çekici bir şeyler yapın. Bu makalede, veritabanları ile çalışma hakkında gerçekleştirirsiniz.)

1. Üst kısmındaki *Form.cshtml* dosyasında, aşağıdaki kodu girin:

    [!code-cshtml[Main](4-working-with-forms/samples/sample2.cshtml)]

    Kullanıcının ilk sayfa istediğinde, yalnızca boş formu görüntülenir. (Bu olacaktır) kullanıcı biçiminde doldurur ve ardından tıkladığında **Gönder**. (Posta) gönderdiğinde bu sunucu için kullanıcı girişi. Varsayılan olarak, aynı sayfaya isteğin gittiğini (yani, *Form.cshtml*).

    Bu süre sayfa gönderdiğinizde, girdiğiniz değerleri yalnızca formun görüntülenir:

    ![Sayfada görüntülenen girmiş olduğunuz değerleri gösteren ekran görüntüsü.](4-working-with-forms/_static/image2.jpg)

    Kod sayfasına bakın. Öncelikle `IsPost` sayfa gönderilen olup olmadığını belirlemek için yöntemi &#8212; kullanılıp bir kullanıcı başka bir deyişle, tıklanan **Gönder** düğmesi. Bu bir post ise `IsPost` true değerini döndürür. Bu ilk bir istek (bir GET isteği) veya bir geri gönderme (bir POST isteği) ile çalışıyorsanız olup olmadığını belirlemek için standart ASP.NET Web Pages'de yoludur. (GET ve POST hakkında daha fazla bilgi için bkz: "HTTP alma ve POST ve IsPost Property" kenar [ASP.NET Web sayfaları programlama kullanarak Razor sözdizimi giriş](https://go.microsoft.com/fwlink/?LinkId=202890#SB_HttpGetPost).)

    Ardından, gelen kullanıcı doldurulan değerleri alın `Request.Form` nesne ve put onları değişkenlerinde daha sonra kullanmak üzere. `Request.Form` Nesne sayfası, her bir anahtar tarafından tanımlanmış gönderilen tüm değerleri içerir. Anahtar eşdeğerdir `name` okumak istediğiniz form alanının özniteliği. Örneğin, okuma için `companyname` alanı (metin kutusu) kullandığınız `Request.Form["companyname"]`.

    Form değerlerini depolanır `Request.Form` dizeler olarak nesnesi. Bu nedenle, bir sayı veya bir tarih veya başka bir tür bir değer ile çalışmak zorunda, dizenin bu türe dönüştürmeniz gerekir. Örnekte, `AsInt` yöntemi `Request.Form` (çalışan sayısı içeren) çalışanlar alanın değeri bir tamsayıya dönüştürmek için kullanılır.
2. Tarayıcınızda sayfasını başlatmak, form alanlarını doldurun ve tıklayın **Gönder**. Sayfa girdiğiniz değerleri görüntüler.

> [!TIP] 
> 
> <a id="SB_HTMLEncoding"></a>
> ### <a name="html-encoding-for-appearance-and-security"></a>HTML görünümü ve güvenlik için kodlama
> 
> HTML var. özel kullanımlar gibi karakterler için `<`, `>`, ve `&`. Bu özel karakterlerin nerede bunlar beklendiği görünüyorsa, Görünüm ve işlevselliği, web sayfanızın sicil. Örneğin, tarayıcı yorumlar `<` (bir boşluk ile izlenir sürece) gibi bir HTML öğesi başlangıcı olarak karakter `<b>` veya `<input ...>`. Yalnızca tarayıcı öğe tanımazsa ile başlayan dize atar `<` , bir şey ulaşana kadar yeniden tanır. Kuşkusuz, bu sayfasındaki bazı tuhaf işleme neden olabilir.
> 
> HTML kodlaması tarayıcılar doğru simgesi olarak yorumlar bir kodu bu ayrılmış karakterleri değiştirir. Örneğin, `<` karakter ile değiştirilir `&lt;` ve `>` karakter ile değiştirilir `&gt;`. Tarayıcının bu değiştirme dizelerini, görmek istediğiniz karakter olarak işler.
> 
> Bu, HTML kodlaması dizeler görüntülemek istediğiniz zaman kullanmak için (aldığınız bir kullanıcıdan giriş), iyi bir fikirdir. Aksi takdirde, bir kullanıcı, web kötü amaçlı bir komut dosyası çalıştırma veya başka bir şey, kendi site güvenliği tehlikeye atar veya sadece ne istediğiniz sayfaya ulaşmak deneyebilirsiniz. (Kullanıcı girişi alan, bir yer depolar ve daha sonra görüntülemek, bu özellikle önemlidir &#8212; Örneğin, bir blog yorum, kullanıcı gözden geçirme ya da benzer bir şey olarak.)
> 
> Otomatik olarak ASP.NET Web Pages bu sorunları önlemeye yardımcı olmak için HTML olarak kodlar herhangi bir metin içeriği o kodunuzdan çıktı. Örneğin, bir değişken veya kod kullanarak bir ifade içeriğini görüntülediğinizde `@MyVar`, ASP.NET Web sayfaları, çıktı otomatik olarak kodlar.


## <a name="validating-user-input"></a>Kullanıcı girişini doğrulama

Kullanıcılar hata yapar. Bir alanı doldurmak için isteyin ve bunlar için unuttunuz veya çalışanların sayısını girin isteyin ve bunlar bunun yerine bir ad yazın. Bu işlemeden önce bir form doğru şekilde doldurulmuş olduğunu emin olmak için kullanıcının girişi doğrulayın.

Bu yordam, kullanıcı boş bırakabilirsiniz yaramadı emin olmak için tüm üç form alanlarını doğrulama işlemi gösterilmektedir. Ayrıca, çalışan sayısı değeri bir sayı olduğunu denetleyin. Hatalar varsa bir hata görüntüleyeceksiniz kullanıcının hangi değerleri belirten ileti doğrulama başarılı olmadı.

1. İçinde *Form.cshtml* dosya, ilk kod bloğunu aşağıdaki kodla değiştirin: 

    [!code-cshtml[Main](4-working-with-forms/samples/sample3.cshtml)]

    Kullanıcı girişini onaylamak için kullandığınız `Validation` Yardımcısı. Gerekli alanları kayıt `Validation.RequireField`. Çağırarak diğer doğrulama türlerini kaydetme `Validation.Add` ve doğrulamak için alan ve gerçekleştirmek için doğrulama türünü belirtin.

    Sayfa çalıştığında, ASP.NET, tüm doğrulama yapar. Çağırarak sonuçları kontrol edebilirsiniz `Validation.IsValid`, her şeyi aktarılırsa true döndüren ve herhangi bir alan doğrulama başarısız olursa false. Genellikle, arama `Validation.IsValid` kullanıcı girişi herhangi bir işlem gerçekleştirmeden önce.
2. Güncelleştirme `<body>` üç çağrıları ekleyerek öğesi `Html.ValidationMessage` böyle bir yöntemi:

    [!code-cshtml[Main](4-working-with-forms/samples/sample4.cshtml?highlight=8,13,18)]

    Doğrulama hatası iletilerini görüntülemek için Html çağırabilirsiniz.`ValidationMessage` ve ileti için istediğiniz alanın adı geçirin.
3. Sayfayı çalıştırın. Alanları boş bırakın ve tıklayın **Gönder**. Hata iletisi görürsünüz.

    ![Kullanıcı girişini doğrulama başarısız olursa görüntülenen hata iletilerini gösteren ekran görüntüsü.](4-working-with-forms/_static/image3.jpg)
4. Bir dize (örneğin, "ABC") eklemek **Employee Count** alan ve tıklayın **Gönder** yeniden. Bu süre belirten bir hata göreceğiniz dizesi doğru biçimde, yani, bir tamsayı değil.

    ![Kullanıcılar çalışanların alan için bir dize girin, görüntülenen hata iletilerini gösteren ekran görüntüsü.](4-working-with-forms/_static/image4.jpg)

ASP.NET Web sayfaları, böylece kullanıcılar, tarayıcıda anında geri bildirim almak otomatik olarak istemci komut dosyasını kullanarak doğrulama gerçekleştirme yeteneği de dahil olmak üzere, kullanıcı girişini doğrulama için diğer seçenekler sağlar. Bkz: [ek kaynaklar](#Additional_Resources) daha fazla bilgi için sonraki bölümü.

## <a name="restoring-form-values-after-postbacks"></a>Form değerleri Geri göndermeler sonra geri yükleme

Sayfasında önceki bölümde Test ettiğinizde, bir doğrulama hatası oluştu, her şeyi (yalnızca geçersiz veriler) girdiğiniz kaybolur ve tüm alanlar için değerleri yeniden girmek zorunda fark etmiş. Önemli bir nokta bunu göstermektedir: sıfırdan bir sayfası gönderme, işleyin ve sonra sayfayı yeniden oluşturmak, sayfayı yeniden oluşturulur. Gördüğünüz gibi bu sayfada zaman gönderildiği olan tüm değerler kaybedilir anlamına gelir.

Bunu kolayca, Bununla birlikte düzeltebilir. Gönderilen değerlere erişebilirsiniz (içinde `Request.Form` sayfa işlendiğinde form alanlarına değerler doldurabilir. Bu nedenle, nesne.

1. İçinde *Form.cshtml* dosyası, değiştirin `value` özniteliklerini `<input>` kullanarak öğeleri `value` özniteliği.: 

    [!code-cshtml[Main](4-working-with-forms/samples/sample5.cshtml?highlight=13,19,25)]

    `value` Özniteliği `<input>` öğeleri dinamik olarak dışı alan değerini okumak için ayarlandı `Request.Form` nesne. İstenen sayfa, ilk kez değerleri `Request.Form` nesne tüm boş. Böylece form boş olduğundan bu güzel, budur.
2. Tarayıcınızda sayfasını başlatmak, form alanlarını doldurun veya boş bırakabilirsiniz ve tıklayın **Gönder**. Gönderilen değerleri gösteren bir sayfa görüntülenir.

    ![Form-5](4-working-with-forms/_static/image5.jpg)

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Ek Kaynaklar

- [Web kullanıcı girişi alma 1,001 yolları](https://msdn.microsoft.com/library/ms971057.aspx)
- [Formları kullanarak ve kullanıcı girişini işleme](https://msdn.microsoft.com/library/ms525182(VS.90).aspx)
- [ASP.NET Web Sayfaları Sitelerinde Kullanıcı Girişini Doğrulama](https://go.microsoft.com/fwlink/?LinkId=253002)
- [HTML formlarına otomatik tamamlama özelliğini kullanma](https://msdn.microsoft.com/library/ms533032(VS.85).aspx)
