---
uid: web-pages/overview/ui-layouts-and-themes/4-working-with-forms
title: "ASP.NET Web sayfaları (Razor) siteleri HTML formları ile çalışma | Microsoft Docs"
author: tfitzmac
description: "Metin kutuları, onay kutularını, radyo düğmeleri ve açılır listeleri gibi kullanıcı girişini denetimleri nerede yerleştirdiğiniz bir bölümü bir HTML belgesinin biçimidir. Formları kullanma uyu..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2014
ms.topic: article
ms.assetid: f3f4b8c8-e8f6-4474-ad94-69228a6c01ee
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/4-working-with-forms
msc.type: authoredcontent
ms.openlocfilehash: 8579c444fd19d1a366349cc09f9f768de23055f8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="working-with-html-forms-in-aspnet-web-pages-razor-sites"></a>ASP.NET Web sayfaları (Razor) siteleri HTML formları ile çalışma
====================
tarafından [zel FitzMacken](https://github.com/tfitzmac)

> Bu makale, bir HTML formuna (ile metin kutuları ve düğmeler) işlemek açıklamaktadır çalışırken bir ASP.NET Web sayfaları (Razor) Web sitesi.
> 
> **Öğrenecekleriniz:** 
> 
> - Bir HTML formu oluşturma
> - Kullanıcı girişi formdan okumak nasıl.
> - Kullanıcı girişi doğrulama yapma.
> - Sayfa gönderildikten sonra form değerleri geri yükleme.
> 
> Programlama Kavramları makalesinde sunulan ASP.NET şunlardır:
> 
> - `Request` Nesnesi.
> - Giriş doğrulaması.
> - HTML encoding.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Öğreticide kullanılan yazılım sürümleri
> 
> 
> - ASP.NET Web sayfaları (Razor) 3
>   
> 
> Bu öğreticide, ASP.NET Web Pages 2 ile de çalışır.


## <a name="creating-a-simple-html-form"></a>Basit bir HTML Form oluşturma

1. Yeni bir Web sitesi oluşturun.
2. Kök klasöründe adlı bir web sayfası oluşturmak *Form.cshtml* ve aşağıdaki biçimlendirme girin:

    [!code-html[Main](4-working-with-forms/samples/sample1.html)]
3. Tarayıcınızda sayfası başlatın. (Webmatrix'te, içinde **dosyaları** çalışma alanında, dosyaya sağ tıklayın ve ardından **başlatma tarayıcıda**.) Üç giriş alanları ile basit bir form ve **gönderme** düğmesi görüntülenir.

    ![Üç metin kutusu ile bir biçimde ekran görüntüsü.](4-working-with-forms/_static/image1.jpg)

    ' A tıklarsanız bu noktada **gönderme** düğmesi, hiçbir şey olmaz. Formun kullanışlı hale getirmek için sunucuda çalışacak biraz kod eklemeniz gerekir.

## <a name="reading-user-input-from-the-form"></a>Kullanıcı girişi formdan okuma

Formu işlemeye gönderilen alan değerlerini okuyan ve bunlarla bir şey olmadığından kodu ekleyin. Bu yordam alanları okuyun ve kullanıcı girişini sayfada görüntülemek nasıl gösterir. (Bir üretim uygulamasında, genellikle kullanıcı girişi ile daha ilginç şeyler yapabilirsiniz. Bu makalede veritabanları ile çalışma hakkında gerçekleştirirsiniz.)

1. Üstündeki *Form.cshtml* dosya, aşağıdaki kodu girin:

    [!code-cshtml[Main](4-working-with-forms/samples/sample2.cshtml)]

    Kullanıcı sayfa ilk kez istediğinde, yalnızca boş form görüntülenir. (Olur) kullanıcıya biçiminde doldurur ve ardından tıklattığında **gönderme**. Bu (postaları) gönderen sunucu için kullanıcı girişi. Varsayılan olarak, istek aynı sayfasına gider (yani, *Form.cshtml*).

    Bu süre sayfa gönderdiğinizde, girdiğiniz değerleri yalnızca formun görüntülenir:

    ![Sayfada görüntülenen girdiğiniz değerleri gösteren ekran görüntüsü.](4-working-with-forms/_static/image2.jpg)

    Sayfa için kod bakabilirsiniz. İlk kullandığınız `IsPost` sayfa mi gönderiliyor &#8212; diğer bir deyişle, bir kullanıcı olup tıklattınız belirlemek amacıyla yöntemi **gönderme** düğmesi. Bu bir post ise `IsPost` true değerini döndürür. Bu bir ilk isteği (GET isteği) veya geri gönderme (bir POST isteği) ile çalışıyorsanız olup olmadığını belirlemek için standart ASP.NET Web Pages'de yoludur. (GET ve POST hakkında daha fazla bilgi için bkz: "HTTP GET ve POST ve IsPost Property" kenar [ASP.NET Web sayfalarını programlama kullanarak Razor sözdizimi giriş](https://go.microsoft.com/fwlink/?LinkId=202890#SB_HttpGetPost).)

    Ardından, gelen kullanıcı doldurulan değerleri alma `Request.Form` nesne ve put bunları değişkenler için daha sonra. `Request.Form` Nesne sayfası, her bir anahtar tarafından tanımlanan gönderildikleri tüm değerleri içerir. Anahtar eşdeğerdir `name` okumak istediğiniz form alanının özniteliği. Örneğin, okumak için `companyname` alan (metin kutusu), kullandığınız `Request.Form["companyname"]`.

    Form değerleri depolanır `Request.Form` dizeleri olarak nesne. Bu nedenle, değeri olan bir sayı veya bir tarih veya başka bir türü olarak çalışmak varsa, bu tür bir dizeden dönüştürmek sahip. Örnekte, `AsInt` yöntemi `Request.Form` (bir çalışan sayısı içeren) çalışanlar alanın değeri bir tamsayıya dönüştürmek için kullanılır.
2. Tarayıcınızda sayfasını başlatmak, form alanları doldurun ve tıklayın **gönderme**. Sayfaya girdiğiniz değerleri görüntüler.

> [!TIP] 
> 
> <a id="SB_HTMLEncoding"></a>
> ### <a name="html-encoding-for-appearance-and-security"></a>HTML görünümü ve güvenlik için kodlama
> 
> HTML var. özel kullanımlar gibi karakterler için `<`, `>`, ve `&`. Şu özel karakterleri bunlar değil beklenirken görünüyorsa, Görünüm ve web sayfanızın işlevselliğini zarar. Örneğin, tarayıcı yorumlar `<` (boşlukla izlenir sürece) bir HTML öğesi başına olarak gibi karakter `<b>` veya `<input ...>`. Tarayıcı öğesi tanımazsa, sadece ile başlayan dize atar `<` bunu bir şey, ulaşana kadar yeniden tanır. Belli ki, bu sayfadaki bazı tuhaf işleme neden olabilir.
> 
> HTML kodlaması bu ayrılmış karakterleri tarayıcılar doğru simge olarak yorumlanan kod ile değiştirir. Örneğin, `<` karakter ile değiştirilir `&lt;` ve `>` karakter ile değiştirilir `&gt;`. Tarayıcı bu değiştirme dizelerini görmek istediğiniz karakter olarak işler.
> 
> Bu, HTML dizeleri görüntülemek istediğiniz zaman kodlama kullanmak için (bir kullanıcıdan aldığınız giriş) iyi bir fikirdir. Bunu yapmazsanız, bir kullanıcı web kötü amaçlı bir komut dosyası çalıştırma veya başka bir şey yapmak için site güvenliği tehlikeye atar veya değil ne düşündüğünüz sayfanızı almak deneyebilirsiniz. (Bu, izlerseniz kullanıcı girişi nedenini bildirmeden bir yerde saklayın ve daha sonra &#8212;görüntüler; örneğin, bir blog yorum kullanıcı gözden geçirin, özellikle önemlidir veya şuna benzer.)
> 
> ASP.NET Web sayfaları bu sorunları otomatik olarak önlemeye yardımcı olmak için HTML olarak kodlar herhangi bir metin içeriği o kodunuzdan çıktı. Örneğin, bir değişken veya kod gibi kullanan bir ifade içeriğini görüntülediğinizde `@MyVar`, ASP.NET Web sayfaları, çıktı otomatik olarak kodlar.


## <a name="validating-user-input"></a>Kullanıcı girişini doğrulama

Kullanıcıları, hataları yapar. Bir alanı doldurmanız isteyin ve bunlar, unuttunuz veya çalışanların sayısını girin isteyin ve bunlar bunun yerine bir ad yazın. Onu işlemeden önce bir form doğru doldurulmuş olduğundan emin olmak için kullanıcının giriş doğrulayın.

Bu yordamda, kullanıcı onları boş bırakın kaydetmedi emin olmak için tüm üç form alanlarını doğrulamak gösterilmiştir. Ayrıca, çalışan sayısı değeri bir sayı olup olmadığını denetleyin. Hatalar varsa, bir hata görüntülersiniz kullanıcının hangi değerlerin bildiren ileti doğrulama geçilmedi.

1. İçinde *Form.cshtml* dosya, ilk kod bloğunu aşağıdaki kodla değiştirin: 

    [!code-cshtml[Main](4-working-with-forms/samples/sample3.cshtml)]

    Kullanıcı girişi doğrulamak için kullandığınız `Validation` Yardımcısı. Çağırarak gerekli alanları kaydedin `Validation.RequireField`. Çağırarak doğrulama diğer türleri kaydedin `Validation.Add` ve doğrulamak için alan ve gerçekleştirmek için doğrulama türünü belirtme.

    Sayfa çalıştığında, ASP.NET, tüm doğrulama yapar. Çağırarak sonuçları kontrol edebilirsiniz `Validation.IsValid`, hangi her şeyi geçirilen, true değerini döndürür ve herhangi bir alan doğrulama başarısız olursa false. Genellikle, arama `Validation.IsValid` kullanıcı girişi herhangi bir işlem gerçekleştirmeden önce.
2. Güncelleştirme `<body>` üç çağrı ekleyerek öğesi `Html.ValidationMessage` yöntemi şuna benzer:

    [!code-cshtml[Main](4-working-with-forms/samples/sample4.cshtml?highlight=8,13,18)]

    Doğrulama hata iletilerini görüntülemek için Html çağırabilirsiniz.`ValidationMessage` ve ileti için istediğiniz alanın adını geçirin.
3. Sayfayı çalıştırın. Alanları boş bırakın ve tıklayın **gönderme**. Hata iletilerine bakın.

    ![Kullanıcı girişi doğrulama geçmiyor durumunda görüntülenen hata iletilerini gösteren ekran görüntüsü.](4-working-with-forms/_static/image3.jpg)
4. Bir dize (örneğin, "ABC") eklemek **çalışan sayısı** alanına gelin ve **gönderme** yeniden. Belirten bir hata göreceğiniz bu süre bir dize özelliği, bir tamsayı doğru biçimde değil.

    ![Kullanıcılar çalışanların alan için bir dize girin, görüntülenen hata iletilerini gösteren ekran görüntüsü.](4-working-with-forms/_static/image4.jpg)

ASP.NET Web sayfaları, böylece kullanıcılar tarayıcıda anında geri bildirim alma istemci komut dosyası kullanarak doğrulama otomatik olarak gerçekleştirme yeteneğini de dahil olmak üzere kullanıcı girişini doğrulama için daha fazla seçenek sağlar. Bkz: [ek kaynaklar](#Additional_Resources) daha fazla bilgi için sonraki bölümü.

## <a name="restoring-form-values-after-postbacks"></a>Form değerleri Geri göndermeler sonra geri yükleme

Önceki bölümde sayfa sınandığında, doğrulama hatası oluştu, her şeyi (yalnızca geçersiz veriler) girdiğiniz kaldırılmıştır ve tüm alanlar için değerleri yeniden girin gerekiyordu fark etmiş olabileceğiniz. Bu önemli bir noktayı gösterir: sıfırdan bir sayfa gönderme işlemesi ve sayfayı yeniden oluşturmak zaman sayfa yeniden oluşturulur. Gördüğünüz gibi bu sayfada zaman gönderildiği olan herhangi bir değeri kaybolur anlamına gelir.

Bu kolayca, ancak düzeltebilirsiniz. Gönderildikleri değerleri erişimi (içinde `Request.Form` sayfa işlendiğinde, bu değerleri form alanlarına doldurabilirsiniz nesnesini.

1. İçinde *Form.cshtml* dosya, yerine `value` özniteliklerini `<input>` kullanarak öğeleri `value` özniteliği.: 

    [!code-cshtml[Main](4-working-with-forms/samples/sample5.cshtml?highlight=13,19,25)]

    `value` Özniteliği `<input>` öğelerini dinamik olarak dışı alan değeri okumak için ayarlanmış `Request.Form` nesnesi. Sayfa istenen ilk kez değerleri `Request.Form` nesne tüm boş. Böylece form boş olduğundan bu sorun, yoktur.
2. Tarayıcınızda sayfasını başlatmak, form alanları doldurun veya boş bırakın ve tıklatın **gönderme**. Gönderilen değerleri gösteren bir sayfa görüntülenir.

    ![Formlar 5](4-working-with-forms/_static/image5.jpg)

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Ek Kaynaklar

- [Web kullanıcı girişi alma 1,001 yolları](https://msdn.microsoft.com/library/ms971057.aspx)
- [Formları kullanarak ve kullanıcı girişini işleme](https://msdn.microsoft.com/library/ms525182(VS.90).aspx)
- [ASP.NET Web Sayfaları Sitelerinde Kullanıcı Girişini Doğrulama](https://go.microsoft.com/fwlink/?LinkId=253002)
- [Otomatik Tamamlama HTML formlarında kullanma](https://msdn.microsoft.com/library/ms533032(VS.85).aspx)
