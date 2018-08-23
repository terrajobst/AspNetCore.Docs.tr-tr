---
uid: web-pages/overview/ui-layouts-and-themes/9-working-with-images
title: Bir ASP.NET Web sayfaları (Razor) sitesinde görüntüleri ile çalışma | Microsoft Docs
author: tfitzmac
description: Bu bölümde, ekleyebilir, görüntüleyebilir ve görüntüleri işlemek nasıl gösterir (yeniden boyutlandırma, çevirme ve filigranlar ekleyin), Web sitenizin.
ms.author: riande
ms.date: 02/20/2014
ms.assetid: 778c4e58-4372-4d25-bab9-aec4a8d8e38d
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/9-working-with-images
msc.type: authoredcontent
ms.openlocfilehash: e609cd1c6ab74b5b40d28bde353501dbacb5d544
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754680"
---
<a name="working-with-images-in-an-aspnet-web-pages-razor-site"></a>Bir ASP.NET Web sayfaları (Razor) sitesinde görüntülerle çalışma
====================
tarafından [Tom FitzMacken](https://github.com/tfitzmac)

> Bu makalede ekleyebilir, görüntüleyebilir ve görüntüleri işlemek gösterilmektedir (yeniden boyutlandırma, çevirme ve filigranlar eklemek) bir ASP.NET Web sayfaları (Razor) Web sitesinde.
> 
> Öğrenecekleriniz:
> 
> - Görüntüyü bir sayfaya dinamik olarak ekleme.
> - Kullanıcıların bir görüntüyü karşıya yükleme yapma.
> - Bir görüntüyü yeniden boyutlandırmak nasıl.
> - Görüntü döndürme veya nasıl.
> - Resme bir filigran ekleme yapma.
> - Filigran olarak görüntü kullanma
> 
> ASP.NET programlama makalesinde sunulan özellikler şunlardır:
> 
> - `WebImage` Yardımcısı.
> - `Path` Nesne yolu ve dosya adlarını değiştirmek olanak tanıyan yöntemler sağlar.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Bu öğreticide kullanılan yazılım sürümleri
> 
> 
> - ASP.NET Web sayfaları (Razor) 2
> - WebMatrix 2
>   
> 
> Bu öğreticide, WebMatrix 3'ile de çalışır.


<a id="Adding_an_Image"></a>
## <a name="adding-an-image-to-a-web-page-dynamically"></a>Görüntü bir Web sayfasına dinamik olarak ekleme

Web sitesi geliştirme yaptığınız sırada Web sitenize ve tek tek sayfaları görüntüleri ekleyebilirsiniz. Ayrıca kullanıcılar, bunları profil fotoğrafı ekleme izin vererek gibi görevler için yararlı olabilir, görüntüleri karşıya sağlayabilirsiniz.

Bir görüntü zaten sitenizde kullanılabilir ve yalnızca bir sayfada görüntülemek istiyorsanız, bir HTML kullandığınız `<img>` öğesi şöyle:

[!code-html[Main](9-working-with-images/samples/sample1.html)]

Bazı durumlarda, ancak dinamik olarak görüntüler ihtiyacınız &#8212; diğer bir deyişle, hangi sayfanın çalışıncaya kadar görüntülenecek görüntü bilmiyorum.

Bu bölümdeki yordamda bir görüntüyü burada kullanıcıları görüntü adları listesinden görüntü dosya adı belirtin hareket halindeyken gösterilmektedir. Aşağı açılan listeden, görüntünün adını seçin ve bunlar sayfası gönderdiğinizde, seçili görüntü görüntülenir.

![[Görüntü]](9-working-with-images/_static/image1.jpg "ch9images 1.jpg")

1. Webmatrix'te, yeni bir Web sitesi oluşturun.
2. Adlı yeni bir sayfa ekleyin *DynamicImage.cshtml*.
3. Web sitesinin kök klasöründe yeni bir klasör ekleyin ve adlandırın *görüntüleri*.
4. Dört yansımalarına Ekle *görüntüleri* oluşturduğunuz klasör. (Herhangi bir görüntü elinizde yapmak kullanışlı olacaktır, ancak bir sayfaya sığması.) Görüntüleri yeniden adlandır *Photo1.jpg*, *Photo2.jpg*, *Photo3.jpg*, ve *Photo4.jpg*. (Kullanmayacağınız *Photo4.jpg* Bu yordam, ancak bu makalenin sonraki bölümlerinde kullanacaksınız.)
5. Dört görüntü, salt okunur olarak işaretlenmemiş olduğunu doğrulayın.
6. Sayfanın mevcut içeriği aşağıdakiyle değiştirin:

    [!code-cshtml[Main](9-working-with-images/samples/sample2.cshtml)]

    Sayfanın gövdesi bir açılan liste vardır (bir `<select>` öğesi) adlandırılan `photoChoice`. Liste Üç seçenek vardır ve `value` her liste seçeneği özniteliğine sahip içine girdiğiniz görüntülerden birini adını *görüntüleri* klasör. Esas olarak, listenin gibi kolay bir ad seçmesine izin verir &quot;fotoğraf 1&quot;, ve ardından geçirir *.jpg* sayfa gönderildiğinde dosya adı.

    Kodda, kullanıcının seçimini (diğer bir deyişle, görüntü dosyası adı) listeden okuyarak alabileceğiniz `Request["photoChoice"]`. İlk olup olmadığını bir seçim hiç görürsünüz. Varsa, klasörün adını görüntüler ve kullanıcının resim dosyası adı oluşan bir görüntü için bir yol oluşturun. (Bir yol oluşturulmaya çalışıldı ancak hiçbir oluştu `Request["photoChoice"]`, bir hata elde edersiniz.) Bu göreli bir yol şöyle olur:

    *görüntüleri/Photo1.jpg*

    Yolun adlı bir değişkende depolanır `imagePath` bu sayfada daha sonra ihtiyacınız olacak.

    Gövdesinde vardır, ayrıca bir `<img>` kullanıcının seçtiği görüntüyü görüntülemek için kullanılan bir öğe. `src` Özniteliği ayarlanmamış bir dosya adı veya URL'si, statik bir öğe görüntülemek için yaptığınız gibi. Bunun yerine, kümesine `@imagePath`, yani bu kodda ayarladığınız yolundan değerini alır.

    Sayfa çalıştığında, ilk kez yine de görüntülemek için resim yok kullanıcı hiçbir şey seçili taşınmadığından için. Bu normalde, açardı `src` öznitelik boş olur ve kırmızı bir resim gösterebilir &quot;x&quot; (veya bir görüntü bulamadığında ne olursa olsun tarayıcı işler). Bunu önlemek için eklediğiniz `<img>` öğesinde bir `if` görmek için sınar blok olmadığını `imagePath` değişkeni olan her şeyi içinde. Kullanıcının bir seçimi yaptıysanız `imagePath` yol içeriyor. Kullanıcı bir görüntü seçin olmadı veya bu ilk kez kullanıyorsanız, sayfa görüntülenir, `<img>` öğe değilse bile çizilir.
7. Dosyayı kaydedin ve sayfanın tarayıcıda çalıştırın. (Emin sayfanın içinde seçili **dosyaları** çalıştırmadan önce çalışma alanı.)
8. Aşağı açılan listeden bir görüntü seçin ve ardından **örnek görüntü**. Farklı görüntüler için farklı seçimler gördüğünüzden emin olun.

<a id="Uploading_an_Image"></a>
## <a name="uploading-an-image"></a>Görüntüyü karşıya yükleme

Önceki örnekte bir görüntüyü dinamik olarak nasıl oluşturulacağını gösterir, ancak Web sitenizde bulunan görüntüleriyle birlikte çalıştı. Bu yordam, kullanıcıların daha sonra sayfada görüntülenen bir görüntüyü karşıya yükleme işlemi gösterilmektedir. ASP.NET'te, görüntüleri kullanarak hareket halindeyken işleyebileceğiniz `WebImage` Yardımcısı, oluşturmak, yönetmek ve görüntüleri kaydetme olanak sağlayan yöntemleri vardır. `WebImage` Yardımcısı gibi tüm yaygın web görüntü dosya türlerini destekler, *.jpg*, *.png*, ve *.bmp*. Bu makale boyunca kullanacağınız *.jpg* görüntüler, ancak herhangi birini kullanabilirsiniz resim türleri.

![[Görüntü]](9-working-with-images/_static/image2.jpg "ch9images 2.jpg")

1. Yeni bir sayfa ekleyin ve adlandırın *UploadImage.cshtml*.
2. Sayfanın mevcut içeriği aşağıdakiyle değiştirin: 

    [!code-cshtml[Main](9-working-with-images/samples/sample3.cshtml)]

    Metin gövdesi olan bir `<input type="file">` , kullanıcıların karşıya yüklenecek dosyayı seçin olanak sağlayan bir öğe. Simgeye **Gönder**, dosyanın çekilen form ile birlikte gönderilir.

    Karşıya yüklenen görüntüyü almak için kullandığınız `WebImage` görüntülerle çalışma için kullanışlı bir yöntem her türlü olan Yardımcısı. Özellikle, kullandığınız `WebImage.GetImageFromRequest` (varsa) karşıya yüklenen görüntüyü almak ve adlı bir değişkende depolamak için `photo`.

    Birçok iş Bu örnekte, alma ve ayarlama dosya ve yol adlarını içerir. Kullanıcı karşıya yüklenen görüntünün adını (ve yalnızca adı) alın ve sonra görüntüyü depolamak için nereye gideceğinizi için yeni bir yol oluşturmak istediğiniz sorunudur. Kullanıcılar, potansiyel olarak aynı ada sahip birden fazla görüntü karşıya yüklenemedi çünkü benzersiz adlar oluşturmak ve mevcut resimleri kullanıcıları eskisinin üzerine yazmayın emin olmak için biraz ek bir kod kullanın.

    Görüntü gerçekten karşıya yüklediyseniz (test `if (photo != null)`), görüntünün görüntü adı alınmaya `FileName` özelliği. Kullanıcı görüntü yüklediğinde `FileName` kullanıcının bilgisayarında yolundan içerir kullanıcının özgün adı içeriyor. Şuna benzeyebilir:

    *C:\Users\Joe\Pictures\SamplePhoto1.jpg*

    Bu yol bilgisi ancak istemediğiniz &#8212; gerçek dosya adı yalnızca istiyorsanız (*SamplePhoto1.jpg*). Yalnızca yoldan dosyasını kullanarak çıkarmanız `Path.GetFileName` böyle bir yöntemi:

    [!code-csharp[Main](9-working-with-images/samples/sample4.cs)]

    Ardından özgün adına bir GUID ekleyerek yeni bir benzersiz bir dosya adı oluşturun. (GUID'ler hakkında daha fazla bilgi için bkz. [hakkında GUID'leri](#SB_AboutGUIDs) bu makalenin ilerleyen bölümlerinde.) Ardından, kaydetmek için kullanabileceğiniz tam yolunu oluşturun. Kaydetme yolu oluşur yeni dosya adı, ' % s'klasörü (görüntüler) ve geçerli Web sitesi konumu.

    > [!NOTE]
    > Dosyaları kaydetmek, kodunuzda sırayla *görüntüleri* klasörü, uygulamanın okuma-yazma bu klasörün izinlerini. Geliştirme bilgisayarınızda bu genellikle bir sorun değildir. Ancak, bir barındırma sağlayıcısının web sunucusuna sitenizi yayımladığınızda, açıkça bu izinleri ayarlamak gerekebilir. Bu kod bir barındırma sağlayıcısının sunucusunda çalıştırın ve hata iletisi alıyorum, bu izinleri ayarlama konusunda bilgi edinmek için barındırma sağlayıcısı ile kontrol edin.

    Son olarak, kaydetme geçirdiğiniz yolu `Save` yöntemi `WebImage` Yardımcısı. Bu, karşıya yüklenen görüntüyü yeni adıyla depolar. Kayıt yöntemi şu şekilde görünür: `photo.Save(@"~\" + imagePath)`. Tam yol eklenir `@"~\"`, geçerli Web sitesi konumu olduğu. (Hakkında bilgi için `~` işleci bkz [giriş kullanımına ASP.NET Web programlama Razor söz dizimi](https://go.microsoft.com/fwlink/?LinkId=202890#ID_WorkingWithFileAndFolderPaths).)

    Önceki örnekte olduğu gibi sayfasının gövdesinde bir `<img>` görüntüyü görüntülemek için öğesi. Varsa `imagePath` ayarlandı, `<img>` öğesi işlenir ve kendi `src` özniteliği `imagePath` değeri.
3. Sayfanın tarayıcıda çalıştırın.
4. Bir görüntüyü karşıya yükleme ve sayfa içinde görüntülendiğinden emin olun.
5. Sitenizi açın *görüntüleri* klasör. Dosya adı şuna benzer yeni bir dosya eklendiğini görürsünüz: 

    *45ea4527-7ddd-4965-b9ca-c6444982b342\_MyPhoto.png*

    Adının önüne bir GUID ile karşıya yüklediğiniz görüntüyü budur. (Kendi dosya farklı bir GUID olacaktır ve büyük olasılıkla farklı bir şey adlı *MyPhoto.png*.)

> [!TIP] 
> 
> <a id="SB_AboutGUIDs"></a>
> ### <a name="about-guids"></a>GUID'leri hakkında
> 
> Bir GUID (genel benzersiz Tanımlayıcı) genellikle bunun gibi bir biçimde işlenen bir tanımlayıcıdır: `936DA01F-9ABD-4d9d-80C7-02AF85C822A8`. Sayı ve harf (A-F) gelen her GUID için farklılık gösterir ancak tüm 8-4-4-4-12 karakter grupları kullanmanın desenini izler. (Teknik olarak, bir GUID bir 16-baytlık/128-bit sayıdır.) Bir GUID gerektiğinde bir GUID sizin için oluşturduğu özel kodu çağırabilir. Sayı devasa boyutu arasındaki fikir GUID'leri arkasında olan (3.4 x 10<sup>38</sup>) ve öğeyi oluşturan algoritma, sonuçta elde edilen sayı neredeyse bir tür olması garanti edilir. Bu nedenle aynı adı iki kez kullanmayacağınız garanti etmeniz gerekir, herhangi bir şeyi adları oluşturmaya yönelik en iyi yolu guıd'lerdir. Dezavantajı, adı yalnızca kodunda kullanılırken kullanılacak eğilimi gösterir şekilde GUID'leri özellikle kullanıcı dostu olmayan olur.


<a id="Resizing_an_Image"></a>
## <a name="resizing-an-image"></a>Görüntüyü Yeniden Boyutlandırma

Web sitenizi bir kullanıcı görüntülerden kabul ederse, görüntülemek veya kaydetmek için önce görüntüleri yeniden boyutlandırma isteyebilirsiniz. Yeniden kullanabileceğiniz `WebImage` bu Yardımcısı.

Bu yordam, bir küçük resim oluşturma ve ardından Web sitesi küçük resim ve özgün resmin kaydetmek için karşıya yüklenen bir görüntüyü yeniden boyutlandırma işlemi gösterilmektedir. Küçük Resim sayfada görüntülemenize ve kullanıcıları tam boyutlu görüntüyü yeniden yönlendirmek için bir köprü kullanın.

![[Görüntü]](9-working-with-images/_static/image3.jpg "ch9images 3.jpg")

1. Adlı yeni bir sayfa ekleyin *Thumbnail.cshtml*.
2. İçinde *görüntüleri* klasöründe adlı bir alt klasör oluşturun *thumbs*.
3. Sayfanın mevcut içeriği aşağıdakiyle değiştirin: 

    [!code-cshtml[Main](9-working-with-images/samples/sample5.cshtml)]

    Bu kod, önceki örnekte koda benzer. Bu kod resmi kaydeder, iki kez, normalde bir kez fark ve görüntüyü küçük resim bir kopyasını oluşturduktan sonra bir kez. Önce karşıya yüklenen görüntüyü almak ve kaydedin *görüntüleri* klasör. Ardından, küçük resim görüntüsü için yeni bir yol de oluşturur. Gerçekten küçük resim oluşturmak için arama `WebImage` yardımcının `Resize` 60 piksel 60-piksel görüntüsü oluşturmak için yöntemi. Bu örnek nasıl, en boy oranını korumak ve durumunda (yeni boyutu gerçekten görüntüyü daha büyük hale getirir) genişletilmesini görüntünün nasıl engelleyebilir gösterir. Yeniden boyutlandırılan resim ardından kaydedilir *thumbs* alt.

    Biçimlendirme sonunda, aynı kullandığınız `<img>` dinamik öğeyle `src` görüntü koşullu olarak göstermek için önceki örneklerde gördünüz özniteliği. Bu durumda, küçük resim görüntüler. Ayrıca bir `<a>` büyük görüntünün sürümünü bir köprü oluşturmak için öğesi. Olduğu gibi `src` özniteliği `<img>` öğesi, ayarladığınız `href` özniteliği `<a>` öğesi içinde ne olursa olsun dinamik olarak `imagePath`. Yolun bir URL olarak çalışabilir emin olmak için geçirdiğiniz `imagePath` için `Html.AttributeEncode` yöntemi URL'de kullanılabilir bir karakter yolunda ayrılmış karakterleri dönüştürür.
4. Sayfanın tarayıcıda çalıştırın.
5. Fotoğrafı karşıya yükleme ve küçük resim gösterildiğini doğrulayın.
6. Tam boyutlu görüntüyü görmek için küçük resme tıklayın.
7. İçinde *görüntüleri* ve *görüntüleri/thumbs*, yeni dosyaları eklendiğini unutmayın.

<a id="Rotating_and_Flipping"></a>
## <a name="rotating-and-flipping-an-image"></a>Görüntüyü çevirme ve döndürme

`WebImage` Yardımcı ayrıca sağlar, çevirme ve döndürme görüntüleri. Bu yordamda, sunucudan bir görüntüyü Al, görüntüyü (dikey) ters çevir, kaydedin ve ardından döndürülen resim sayfada görüntülemek gösterilmiştir. Bu örnekte, yalnızca sunucu üzerinde zaten sahip bir dosya kullanıyorsanız (*Photo2.jpg*). Gerçek bir uygulamada, büyük olasılıkla önceki örneklerde olduğu gibi adları dinamik olarak almak bir görüntüyü çevir.

![[Görüntü]](9-working-with-images/_static/image4.jpg "ch9images 4.jpg")

1. Adlı yeni bir sayfa ekleyin *FlipImage.cshtml*.
2. Sayfanın mevcut içeriği aşağıdakiyle değiştirin: 

    [!code-cshtml[Main](9-working-with-images/samples/sample6.cshtml)]

    Kod `WebImage` görüntü sunucudan almak için yardımcı. Görüntüleri kaydetmek için önceki örneklerde kullanılan aynı yöntemleri kullanarak bir resmin yolunu oluşturun ve bu yolu kullanarak bir görüntü oluşturduğunuzda geçirdiğiniz `WebImage`:

    [!code-javascript[Main](9-working-with-images/samples/sample7.js)]

    Görüntü bulunursa, önceki örneklerde olduğu gibi yeni bir yol ve dosya adı, oluşturun. Görüntü ters çevirmek için çağrı `FlipVertical` yöntemi ve ardından, kaydetmek görüntü yeniden.

    Görüntüyü kullanarak sayfada yeniden görüntülenen `<img>` öğeyle `src` özniteliğini `imagePath`.
3. Sayfanın tarayıcıda çalıştırın. Görüntüyü *Photo2.jpg* tersyüz gösterilir.
4. Sayfayı yenileyin veya yeniden görüntü çevrilen sağ tarafındaki yukarı yeniden görmek için sayfayı isteyin.

Görüntü döndürme için hariç çağırmak yerine, aynı kodu kullanın. `FlipVertical` veya `FlipHorizontal`, çağırmanızı `RotateLeft` veya `RotateRight`.

<a id="Adding_a_Watermark"></a>
## <a name="adding-a-watermark-to-an-image"></a>Resme bir filigran ekleme

Web sitenize birini eklediğinizde, bu görüntüleri kaydedin veya bir sayfada görüntülemek için önce bir filigran görüntüye eklemek isteyebilirsiniz. Kişiler, filigranlar genellikle telif hakkı bilgileri görüntüye eklemek veya iş adlarına bildirmek için kullanın.

![[Görüntü]](9-working-with-images/_static/image5.jpg "ch9images 5.jpg")

1. Adlı yeni bir sayfa ekleyin *Watermark.cshtml*.
2. Sayfanın mevcut içeriği aşağıdakiyle değiştirin: 

    [!code-cshtml[Main](9-working-with-images/samples/sample8.cshtml)]

    Bu kod, kodda benzer *FlipImage.cshtml* sayfasından daha önce (bu sefer olsa da, kullandığı *Photo3.jpg* dosyası). Filigran eklemek için çağrı `WebImage` yardımcının `AddTextWatermark` görüntü kaydetmeden önce yöntemi. Çağrısında `AddTextWatermark`, metin geçirdiğiniz &quot;My Filigran&quot;, yazı tipi rengi sarı olarak ayarlayın ve için Arial yazı tipi ailesini ayarlayın. (Burada, gösterilmese `WebImage` yardımcı ayrıca opaklık, yazı tipi ailesi ve yazı tipi boyutu ve konumu Filigran metninin belirtmenizi sağlar.) Görüntüyü kaydederken salt okunur olmamalıdır.

    Önce gördüğünüz gibi görüntü sayfasında kullanarak gösterilen `<img>` src özniteliği olan öğe kümesine `@imagePath`.
3. Sayfanın tarayıcıda çalıştırın. Metin "My Filigran" görüntüsünün sağ alt köşedeki dikkat edin.

<a id="Using_an_Image_as_a_Watermark"></a>
## <a name="using-an-image-as-a-watermark"></a>Filigran olarak görüntü kullanma

Metin için Filigran kullanmak yerine, başka bir görüntü kullanabilirsiniz. Kişiler bazen bir şirket logosu gibi görüntülerini filigran olarak veya bunların bir filigran resmi yerine metin için telif hakkı bilgileri kullanın.

![[Görüntü]](9-working-with-images/_static/image6.jpg "ch9images 6.jpg")

1. Adlı yeni bir sayfa ekleyin *ImageWatermark.cshtml*.
2. Görüntüye ekleme *görüntüleri* logo olarak kullanın ve görüntüyü tekrar adlandırmanız klasörü *MyCompanyLogo.jpg*. Bu görüntü 80 piksel genişliğinde ve 20 piksel yüksekliğinde ayarlandığında açıkça görebileceğiniz bir görüntü olmalıdır.
3. Sayfanın mevcut içeriği aşağıdakiyle değiştirin: 

    [!code-cshtml[Main](9-working-with-images/samples/sample9.cshtml)]

    Önceki örneklerle kod başka bir değişim budur. Bu durumda, çağırmanızı `AddImageWatermark` Filigran resminin hedef görüntüsüne eklemek için (*Photo3.jpg*) görüntü kaydetmeden önce. Çağırdığınızda `AddImageWatermark`, 20 piksel yüksekliği ve 80 piksel genişliği ayarlayın. *MyCompanyLogo.jpg* görüntüsü Merkezi'nde yatay olarak Hizala ve hedef görüntü alt kısmında dikey hizalı. Opaklık % 100'e ayarlanır ve doldurma 10 piksel olarak ayarlanır. Filigran resminin hedef görüntüden daha büyük ise, hiçbir şey olmaz. Filigran resminin hedef görüntüden daha büyüktür ve sıfır olarak görüntü Filigran doldurma ayarlarsanız, filigran göz ardı edilir.

    Önce görüntü kullanarak görüntüleme gibi `<img>` öğesi ve dinamik `src` özniteliği.
4. Sayfanın tarayıcıda çalıştırın. Filigran resminin ana görüntü en altında göründüğüne dikkat edin.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Ek Kaynaklar


[Bir ASP.NET Web sayfaları sitesinde dosyalarıyla çalışma](https://go.microsoft.com/fwlink/?LinkId=202896)

[ASP.NET Web sayfaları programlama Razor söz dizimini kullanarak giriş](https://go.microsoft.com/fwlink/?LinkID=251587)
