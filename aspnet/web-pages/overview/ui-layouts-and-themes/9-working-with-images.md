---
uid: web-pages/overview/ui-layouts-and-themes/9-working-with-images
title: Bir ASP.NET Web sayfaları (Razor) sitesinde görüntülerle çalışma | Microsoft Docs
author: tfitzmac
description: Bu bölümde, ekleme, görüntüleme ve görüntüleri işlemek gösterir (yeniden boyutlandırabilir, ters çevirin ve filigran ekleme), Web sitenizin.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: 778c4e58-4372-4d25-bab9-aec4a8d8e38d
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/9-working-with-images
msc.type: authoredcontent
ms.openlocfilehash: 74838dd364a43f3f4c966c1417d0f0b2cc242f28
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="working-with-images-in-an-aspnet-web-pages-razor-site"></a>Bir ASP.NET Web sayfaları (Razor) sitesinde görüntülerle çalışma
====================
tarafından [zel FitzMacken](https://github.com/tfitzmac)

> Bu makalede, ekleme, görüntüleme ve görüntüleri işlemek gösterir (yeniden boyutlandırabilir, ters çevirin ve filigranlar Ekle) bir ASP.NET Web sayfaları (Razor) Web sitesinde.
> 
> Öğrenecekleriniz:
> 
> - Görüntüyü bir sayfaya dinamik olarak ekleme.
> - Bir görüntüyü karşıya yükleyin, kullanıcıların izin yapma.
> - Görüntüyü yeniden boyutlandırma yapma.
> - Görüntüyü döndürme veya nasıl.
> - Filigran görüntüye ekleme.
> - Bir görüntüyü filigran olarak kullanmak nasıl.
> 
> Bu makalede sunulan özellikler programlama ASP.NET şunlardır:
> 
> - `WebImage` Yardımcısı.
> - `Path` Yolu ve dosya adlarını değiştirmek olanak tanıyan yöntemler sağlayan nesne.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Öğreticide kullanılan yazılım sürümleri
> 
> 
> - ASP.NET Web sayfaları (Razor) 2
> - WebMatrix 2
>   
> 
> Bu öğretici, WebMatrix 3 ile de çalışır.


<a id="Adding_an_Image"></a>
## <a name="adding-an-image-to-a-web-page-dynamically"></a>Görüntü bir Web sayfasına dinamik olarak ekleme

Web sitesi geliştirirken sırasında Web sitenizi ve tek tek sayfaları görüntüler ekleyebilirsiniz. Ayrıca kullanıcılar, profil fotoğrafınız eklemek yapmalarına izin gibi görevler için yararlı olabilecek görüntülerine karşıya sağlayabilirsiniz.

Bir görüntü zaten sitenizde kullanılabilir ve yalnızca bir sayfada görüntülemek istediğiniz, bir HTML kullanın `<img>` öğesi şuna benzer:

[!code-html[Main](9-working-with-images/samples/sample1.html)]

Bazı durumlarda, yine de, görüntüleri dinamik olarak göstermek gerekir & #8212; kadar sayfasını görüntülemek için hangi görüntü çalıştıran diğer bir deyişle, tanımadığınız.

Bu bölümdeki yordamı, burada kullanıcılar resim adları listesinden görüntü dosya adını belirtin. kolay bir şekilde bir görüntüyü gösterilmektedir. Görüntü adı açılan listeden seçerek ve sayfa gönderdiğinizde, seçili görüntü görüntülenir.

![[Görüntü] ] (9-working-with-images/_static/image1.jpg "ch9images 1.jpg")

1. Webmatrix'te, yeni bir Web sitesi oluşturun.
2. Adlı yeni bir sayfa ekleyin *DynamicImage.cshtml*.
3. Web sitesinin kök klasöründe yeni bir klasör ekleyin ve adını *görüntüleri*.
4. Dört yansımalarına Ekle *görüntüleri* oluşturduğunuz klasör. (Tüm görüntüleri yapmak eserinizi sahip, ancak bir sayfaya sığmayacak.) Görüntüleri yeniden adlandırma *Photo1.jpg*, *Photo2.jpg*, *Photo3.jpg*, ve *Photo4.jpg*. (Kullanmayacağınız *Photo4.jpg* Bu yordam, ancak bu makalenin sonraki bölümlerinde kullanacaksınız.)
5. Dört görüntüleri, salt okunur olarak işaretlenmemiş olduğunu doğrulayın.
6. Sayfa varolan içeriği aşağıdakiyle değiştirin:

    [!code-cshtml[Main](9-working-with-images/samples/sample2.cshtml)]

    Aşağı açılan listesi sayfasının gövdesi vardır (bir `<select>` öğesi) adlandırılan `photoChoice`. Listenin üç seçenek vardır ve `value` her liste seçeneği özniteliğine sahip içine görüntülerden birini adını *görüntüleri* klasör. Esas olarak, listenin gibi kolay bir ad seçin olanak tanır &quot;fotoğraf 1&quot;, ve ardından geçirir *.jpg* sayfa gönderildiğinde dosya adı.

    Kodda, kullanıcının seçimini (diğer bir deyişle, görüntü dosyası adı) listeden okuyarak alabileceğiniz `Request["photoChoice"]`. İlk olup olmadığını seçimi hiç bakın. Varsa, klasörün adını görüntüler için ve kullanıcının resim dosyası adını oluşan görüntü için bir yol oluşturun. (Bir yolu oluşturmak denedi ancak hiçbir vardı `Request["photoChoice"]`, bir hata alıyorsunuz.) Bu, bu gibi göreli bir yol sonuçlanır:

    *görüntüleri/Photo1.jpg*

    Yolun adlı değişkende depolanır `imagePath` sayfasında kaldırmanız gerekir.

    Gövdesinde vardır, ayrıca bir `<img>` kullanıcı çekilen görüntüyü görüntülemek için kullanılan öğesi. `src` Özniteliği değil olarak ayarlanmış bir dosya adı veya URL'si, statik öğe görüntülemek için yaptığınız gibi. Bunun yerine, kümesine `@imagePath`, yani bu kodda ayarladığınız yolundan değerini alır.

    Sayfa çalıştığında, ilk kez yine de görüntülemek için resim yok kullanıcı herhangi bir şey seçildiğinden kurmadı. Bu normalde, anlamına gelir `src` özniteliği boş olacaktır ve görüntüyü bir kırmızı olarak göstermeniz &quot;x&quot; (veya bir görüntü bulamadığında ne olursa olsun tarayıcı oluşturur). Bunu önlemek için put `<img>` öğesinde bir `if` görmek için sınar blok olup olmadığını `imagePath` değişkeni sahip herhangi bir şey içinde. Kullanıcı bir seçim yaptıysanız `imagePath` yolunu içerir. Kullanıcı bir görüntü seçin alamadık veya bu ilk kez kullanıyorsanız sayfası görüntülenir, varsa `<img>` öğesi değil bile çizilir.
7. Dosyayı kaydedin ve sayfasını bir tarayıcıda çalıştırın. (Emin olun sayfa seçildiğinde, **dosyaları** çalıştırmadan önce onu çalışma.)
8. Aşağı açılan listeden bir görüntü seçin ve ardından **örnek görüntü**. Farklı görüntüler için farklı seçenekleri gördüğünüzden emin olun.

<a id="Uploading_an_Image"></a>
## <a name="uploading-an-image"></a>Görüntüyü karşıya yükleme

Önceki örnekte bir görüntüyü dinamik olarak nasıl oluşturulacağını gösterir, ancak, Web sitenizde bulunan görüntüleri ile çalıştınız. Bu yordamda daha sonra sayfada görüntülenen bir görüntüyü karşıya yükleyin, kullanıcıların izin gösterilmiştir. ASP.NET, görüntüleri kullanarak kolay bir şekilde işleyebilir `WebImage` oluşturmak, değiştirmek ve görüntüleri Kaydet olanak sağlayan yöntemleri vardır Yardımcısı. `WebImage` Yardımcısı dahil olmak üzere tüm ortak web görüntü dosya türlerini destekler, *.jpg*, *.png*, ve *.bmp*. Bu makale kullanacağınız *.jpg* görüntüler, ancak herhangi birini kullanabilirsiniz resim türleri.

![[Görüntü] ] (9-working-with-images/_static/image2.jpg "ch9images 2.jpg")

1. Yeni bir sayfa ekleyin ve adını *UploadImage.cshtml*.
2. Sayfa varolan içeriği aşağıdakiyle değiştirin: 

    [!code-cshtml[Main](9-working-with-images/samples/sample3.cshtml)]

    Metin gövdesi bir `<input type="file">` karşıya yüklenecek dosyayı kullanıcıların seçmesine izin veren öğesi. Bunlar tıklattığınızda **gönderme**, çekilen dosya formun birlikte gönderilir.

    Karşıya yüklenen görüntü almak için kullandığınız `WebImage` görüntüler ile çalışmak için kullanışlı yöntemler her türlü sahip Yardımcısı. Özellikle, kullandığınız `WebImage.GetImageFromRequest` (varsa) karşıya yüklenen görüntü alın ve adlı bir değişkende depolamak için `photo`.

    Bu örnekte iş çok dosya ve yol adlarını ayarlama ve alma içerir. Kullanıcı karşıya resminin adı (ve yalnızca adı) almak ve görüntüyü depolamak nerede bulunacağını için yeni bir yol oluşturmak istediğiniz bir sorundur. Kullanıcılar, potansiyel olarak aynı ada sahip birden fazla görüntü karşıya yüklenemedi çünkü benzersiz adlar oluşturmak ve kullanıcıların varolan resimleri üzerine yazmayın emin olmak için biraz birkaç ek kod kullanın.

    Bir görüntü gerçekten karşıya yüklediyseniz (test `if (photo != null)`), görüntü adı görüntüden 's almak `FileName` özelliği. Kullanıcı görüntü yüklediğinde `FileName` kullanıcının bilgisayarı yolundan içerir kullanıcının özgün adı içeriyor. Şuna benzeyebilir:

    *C:\Users\Joe\Pictures\SamplePhoto1.jpg*

    Bu yol bilgileri ancak istemediğiniz & #8212; yalnızca gerçek dosya adı istediğiniz (*SamplePhoto1.jpg*). Yalnızca bir yoldan dosya kullanarak Şerit `Path.GetFileName` yöntemi şuna benzer:

    [!code-csharp[Main](9-working-with-images/samples/sample4.cs)]

    Sonra özgün adına bir GUID ekleyerek yeni bir benzersiz dosya adı oluşturun. (GUID'ler hakkında daha fazla bilgi için bkz: [hakkında GUID'ler](#SB_AboutGUIDs) bu makalenin ilerisinde yer.) Ardından görüntüsünü kaydetmek için kullanabileceğiniz tam yolunu oluşturun. Kaydetme yolu oluşur yeni dosya adı, klasör (görüntüler) ve geçerli Web sitesi konumu.

    > [!NOTE]
    > Kodunuz dosyaları kaydetmek için sırayla *görüntüleri* klasörü, uygulama okuma-yazma bu klasör için izinleri. Geliştirme bilgisayarınızda bu genellikle bir sorun değildir. Ancak, bir barındırma sağlayıcısının web sunucusuna sitenizi yayımladığınızda, açık bir şekilde bu izinleri ayarlamanız gerekebilir. Bu kodu bir barındırma sağlayıcısının sunucusunda çalıştırın ve hataları alırsanız bu izinleri ayarlamak nasıl öğrenmek için barındırma sağlayıcısına başvurun.

    Son olarak, kaydetme geçirdiğiniz yoluna `Save` yöntemi `WebImage` Yardımcısı. Karşıya yüklenen görüntü yeni adıyla depolar. Save yöntemi şöyle görünür: `photo.Save(@"~\" + imagePath)`. Tam yolunu eklenir `@"~\"`, geçerli Web sitesi konumunu olduğu. (Hakkında bilgi için `~` işleci, bkz: [ASP.NET Web programlama kullanarak Razor sözdizimi giriş](https://go.microsoft.com/fwlink/?LinkId=202890#ID_WorkingWithFileAndFolderPaths).)

    Önceki örnekte olduğu gibi sayfa gövdesini içeren bir `<img>` görüntüyü öğesi. Varsa `imagePath` ayarlandığından, `<img>` öğesi işlenir ve kendi `src` özniteliği `imagePath` değeri.
3. Bir tarayıcıda. Sayfayı çalıştırın.
4. Bir görüntüyü karşıya yükleyin ve sayfanın görüntülendiğinden emin olun.
5. Sitenizdeki, açık *görüntüleri* klasör. Yeni bir dosya, dosya adı şuna benzer eklendi bkz:: 

    *45ea4527-7ddd-4965-b9ca-c6444982b342\_MyPhoto.png*

    Bu, adının önüne bir GUID ile karşıya görüntüsüdür. (Kendi dosya farklı bir GUID olacaktır ve farklı bir şey büyük olasılıkla adlı *MyPhoto.png*.)

> [!TIP] 
> 
> <a id="SB_AboutGUIDs"></a>
> ### <a name="about-guids"></a>GUID'ler hakkında
> 
> Bir GUID (genel benzersiz kimliği) genellikle şuna benzer bir biçimde işlenen bir tanımlayıcıdır: `936DA01F-9ABD-4d9d-80C7-02AF85C822A8`. Rakam ve harf (A-F) başlangıç için her GUID farklılık gösterir ancak tüm 8-4-4-4-12 karakter grubu kullanarak desenini izleyin. (Teknik olarak, bir GUID bir 16 bayt/128-bit sayıdır.) Bir GUID gerektiğinde, sizin için bir GUID oluşturur özel bir kod çağırabilir. GUID'ler arkasında numarası muazzam boyutunu arasında olur (3.4 x 10<sup>38</sup>) ve bunu oluşturmak için algoritması, sonuçta elde edilen numarası neredeyse bir tür için garanti edilmez. Bu nedenle aynı adı iki kez kullanmayacağınız güvence altına almalıdır, herhangi bir şeyi adları oluşturmaya yönelik en iyi yolu guıd'lerdir. Dezavantajı doğal olarak, yalnızca kod içinde kullanılan adı için kullanılan eğilimindedir şekilde GUID'ler özellikle kullanıcı dostu, olmamasıdır.


<a id="Resizing_an_Image"></a>
## <a name="resizing-an-image"></a>Görüntüyü Yeniden Boyutlandırma

Web sitenizi bir kullanıcı görüntülerden kabul ederse, görüntüleme veya onları kaydetmeden önce görüntüleri yeniden boyutlandırma isteyebilirsiniz. Yeniden kullanabileceğiniz `WebImage` bu için Yardımcısı.

Bu yordamda, küçük resim oluşturma ve ardından Web sitesi küçük resim ve özgün görüntüsüne kaydetmek için karşıya yüklenen görüntüyü yeniden boyutlandırma gösterilmiştir. Küçük Resim sayfada görüntülenir ve kullanıcılar tam boyutlu görüntüyü yönlendirmek için bir köprü kullanın.

![[Görüntü] ] (9-working-with-images/_static/image3.jpg "ch9images 3.jpg")

1. Adlı yeni bir sayfa ekleyin *Thumbnail.cshtml*.
2. İçinde *görüntüleri* klasör adında bir alt klasör oluşturun *başparmak*.
3. Sayfa varolan içeriği aşağıdakiyle değiştirin: 

    [!code-cshtml[Main](9-working-with-images/samples/sample5.cshtml)]

    Bu kod, önceki örnekten koda benzer. Bu kod resmi kaydeder, iki kez, bir kez normalde farktır ve görüntünün küçük bir kopyasını oluşturduktan sonra bir kez. İlk karşıya yüklenen görüntü almak ve bunu kaydetmek *görüntüleri* klasör. Ardından, küçük resim için yeni bir yol de oluşturun. Gerçekten küçük resim oluşturmak için arama `WebImage` yardımcının `Resize` 60 piksel 60-piksel görüntüsü oluşturmak için yöntem. Örnek (yeni boyutu gerçekte görüntü büyük yapacağı durumda) genişletilmesini görüntünün nasıl engelleyebilir ve en boy oranını korumak nasıl gösterir. Yeniden boyutlandırılan resim ardından kaydedilen *başparmak* alt klasörü.

    İşaretleme sonunda, aynı kullandığınız `<img>` dinamik bir öğesiyle `src` koşullu görüntü göstermek için önceki örneklerde gördünüz özniteliği. Bu durumda, küçük resim görüntüleyin. Aynı zamanda bir `<a>` görüntüsünün büyük sürümü için köprü oluşturma öğesi. İle `src` özniteliği `<img>` öğesi, ayarladığınız `href` özniteliği `<a>` içinde ne olursa olsun öğesini dinamik olarak `imagePath`. Yolun bir URL olarak çalışabilir emin olmak için geçirdiğiniz `imagePath` için `Html.AttributeEncode` URL'de Tamam karakter yolundaki ayrılmış karakterler dönüştürür yöntemi.
4. Bir tarayıcıda. Sayfayı çalıştırın.
5. Fotoğrafı karşıya yükledikten ve küçük resim göründüğünü doğrulayın.
6. Küçük resim tam boyutlu görüntüyü görmek için tıklatın.
7. İçinde *görüntüleri* ve *görüntüleri/başparmak*, yeni dosyaları eklendiğini unutmayın.

<a id="Rotating_and_Flipping"></a>
## <a name="rotating-and-flipping-an-image"></a>Görüntüyü çevirme ve döndürme

`WebImage` Yardımcı de sağlar çevirme ve döndürme görüntüler. Bu yordamda, sunucudan bir görüntüsünü Al, görüntü (dikey) ters çevir, dosyayı kaydedin ve sonra döndürülen resim sayfada görüntülemek gösterilmiştir. Bu örnekte, sunucu üzerinde zaten sahip bir dosyayı yalnızca kullanıyorsanız (*Photo2.jpg*). Gerçek bir uygulamada büyük olasılıkla önceki örneklerde olduğu gibi adları dinamik olarak almak görüntü çevir.

![[Görüntü] ] (9-working-with-images/_static/image4.jpg "ch9images 4.jpg")

1. Adlı yeni bir sayfa ekleyin *FlipImage.cshtml*.
2. Sayfa varolan içeriği aşağıdakiyle değiştirin: 

    [!code-cshtml[Main](9-working-with-images/samples/sample6.cshtml)]

    Kod kullanan `WebImage` yardımcı görüntü sunucudan alınamadı. Görüntüleri kaydetmek için önceki örneklerde kullanılan teknikle görüntüsünün yolunu oluşturun ve kullanarak bir görüntü oluşturduğunuzda, bu yolun geçirin `WebImage`:

    [!code-javascript[Main](9-working-with-images/samples/sample7.js)]

    Bir görüntü bulunursa, önceki örneklerde olduğu gibi yeni bir yol ve dosya adı, oluşturun. Görüntüyü ters çevirmek için arama `FlipVertical` yöntemi, ardından, kaydetmek ve görüntünün yeniden.

    Görüntü kullanarak sayfada yeniden görüntülenen `<img>` öğeyle `src` özniteliği kümesine `imagePath`.
3. Bir tarayıcıda. Sayfayı çalıştırın. Görüntüsü için *Photo2.jpg* ters gösterilir.
4. Sayfayı yenileyin veya yeniden çevrilen sağdaki yukarı yeniden görüntüdür görmek için page isteyin.

Bir resmi döndürmek için çağırmak yerine hariç aynı kodu kullan `FlipVertical` veya `FlipHorizontal`, çağırmanız `RotateLeft` veya `RotateRight`.

<a id="Adding_a_Watermark"></a>
## <a name="adding-a-watermark-to-an-image"></a>Görüntüye filigran ekleme

Web sitenize görüntüsü eklediğinizde kaydetmek veya bir sayfada görüntülemek için önce bir filigran görüntüye eklemek isteyebilirsiniz. Kişi filigranlar görüntüye telif hakkı bilgileri eklemenize veya kendi iş adı tanıtmak için genellikle kullanır.

![[Görüntü] ] (9-working-with-images/_static/image5.jpg "ch9images 5.jpg")

1. Adlı yeni bir sayfa ekleyin *Watermark.cshtml*.
2. Sayfa varolan içeriği aşağıdakiyle değiştirin: 

    [!code-cshtml[Main](9-working-with-images/samples/sample8.cshtml)]

    Bu kod kodda gibidir *FlipImage.cshtml* sayfasından önceki (bu sefer rağmen bu kullanır *Photo3.jpg* dosya). Filigran eklemek için arama `WebImage` yardımcının `AddTextWatermark` görüntü kaydetmeden önce yöntemi. Çağrısında `AddTextWatermark`, metin geçirdiğiniz &quot;My Filigran&quot;, yazı tipi rengini sarıya ve yazı tipi ailesi için Arial ayarlayın. (Burada gösterilmese de `WebImage` yardımcı ayrıca opaklık, yazı tipi ailesi ve yazı tipi boyutunu ve konumunu Filigran metninin belirtmenizi sağlar.) Görüntüyü kaydederken salt okunur olmamalıdır.

    Önce gördüğünüz gibi görüntü sayfasında kullanarak gösterilen `<img>` src özniteliği olan öğe kümesine `@imagePath`.
3. Bir tarayıcıda. Sayfayı çalıştırın. "My Filigran" metin, görüntü sağ alt köşesinde dikkat edin.

<a id="Using_an_Image_as_a_Watermark"></a>
## <a name="using-an-image-as-a-watermark"></a>Filigran olarak resim kullanma

Metin için Filigran kullanmak yerine, başka bir görüntü kullanabilirsiniz. Kişiler bazen görüntüleri bir şirket logosu gibi filigran olarak veya bunların bir filigran resmi yerine metin için telif hakkı bilgileri kullanın.

![[Görüntü] ] (9-working-with-images/_static/image6.jpg "ch9images 6.jpg")

1. Adlı yeni bir sayfa ekleyin *ImageWatermark.cshtml*.
2. Görüntüye ekleme *görüntüleri* logo olarak kullanın ve görüntüyü tekrar adlandırmanız klasörü *MyCompanyLogo.jpg*. Bu görüntü, 80 piksel genişliğinde ve 20 piksel yüksek ayarlandığında açıkça görebilirsiniz görüntüyü olmalıdır.
3. Sayfa varolan içeriği aşağıdakiyle değiştirin: 

    [!code-cshtml[Main](9-working-with-images/samples/sample9.cshtml)]

    Önceki örneklerle kodu başka bir değişim budur. Bu durumda, çağrı `AddImageWatermark` Filigran resmini hedef görüntüsüne eklemek için (*Photo3.jpg*) görüntüyü kaydetmeden önce. Çağırdığınızda `AddImageWatermark`, 80 piksel ve 20 piksel yüksekliğe genişliğini ayarla. *MyCompanyLogo.jpg* görüntü yatay Merkezi'nde hizalanmış ve hedef görüntü altındaki dikey hizalanmış. Opaklık % 100'e ayarlanır ve doldurma 10 piksel olarak ayarlanır. Filigran resminin hedef görüntüden daha büyük ise, hiçbir şey olmaz. Filigran resminin hedef görüntüden daha büyüktür ve sıfıra görüntü Filigran doldurma ayarlarsanız Filigran göz ardı edilir.

    Önce kullanarak resim görüntülerken `<img>` öğesi ve dinamik `src` özniteliği.
4. Bir tarayıcıda. Sayfayı çalıştırın. Filigran resminin ana görüntü alt kısmındaki görüntülendiğine dikkat edin.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Ek Kaynaklar


[Bir ASP.NET Web sayfaları sitesindeki dosyaları ile çalışma](https://go.microsoft.com/fwlink/?LinkId=202896)

[ASP.NET Web sayfaları Razor sözdizimini kullanarak programlama giriş](https://go.microsoft.com/fwlink/?LinkID=251587)
