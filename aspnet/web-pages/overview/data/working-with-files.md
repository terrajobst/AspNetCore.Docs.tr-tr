---
uid: web-pages/overview/data/working-with-files
title: Bir ASP.NET Web sayfaları (Razor) sitesinde dosyaları ile çalışma | Microsoft Docs
author: tfitzmac
description: Bu bölümde, okuma, yazma, ekleme, silme ve dosyaları karşıya yükleme açıklanmaktadır.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: eee916e4-ba4c-439a-a24e-68df7d45a569
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/data/working-with-files
msc.type: authoredcontent
ms.openlocfilehash: bd2b432eaa8bff31a8c4432d249308049a4c6aec
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37364678"
---
<a name="working-with-files-in-an-aspnet-web-pages-razor-site"></a>Bir ASP.NET Web sayfaları (Razor) sitesinde dosyalarıyla çalışma
====================
tarafından [Tom FitzMacken](https://github.com/tfitzmac)

> Bu makalede, okuma, yazma, ekleme, silme ve bir ASP.NET Web sayfaları (Razor) sitesinde dosyaları karşıya yükleme açıklanmaktadır.
> 
> > [!NOTE]
> > Görüntüleri karşıya yüklemek ve bunları yönetmek istiyorsanız (örneğin, çevirme veya bunları yeniden boyutlandırma), bkz: [bir ASP.NET Web sayfaları sitesinde görüntülerle çalışma](https://go.microsoft.com/fwlink/?LinkId=202897).
> 
> 
> **Öğrenecekleriniz:** 
> 
> - Nasıl bir metin dosyası oluşturun ve veri yazma.
> - Varolan bir dosyaya veri ekleme yapma.
> - Nasıl bir dosyayı okumak ve buradan görüntüleyin.
> - Nasıl bir Web sitesinden dosya silinir.
> - Kullanıcıların bir veya birden çok dosya karşıya nasıl.
> 
> ASP.NET programlama makalesinde sunulan özellikler şunlardır:
> 
> - `File` Dosyaları yönetmek için bir yol sağlayan nesne.
> - `FileUpload` Yardımcısı.
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


<a id="Creating_a_Text_File"></a>
## <a name="creating-a-text-file-and-writing-data-to-it"></a>Bir metin dosyası oluşturma ve veri yazma

Siteniz veritabanı kullanmanın yanı sıra dosyaları ile çalışabilir. Örneğin, site verilerini depolamak için basit bir yol olarak metin dosyalarını kullanabilirsiniz. (Veri depolamak için kullanılan bir metin dosyası adlandırılan bir *düz dosya*.) Metin dosyası olabilir farklı biçimlerde gibi *.txt*, *.xml*, veya *.csv* (virgülle ayrılmış değerler).

Bir metin dosyasına veri depolamak istiyorsanız, kullanabileceğiniz `File.WriteAllText` yöntemi oluşturmak için dosyasını belirtin ve ona yazılacak veriler. Bu yordamda üç basit bir form içeren bir sayfa oluşturacaksınız `input` öğeleri (ad, Soyadı ve e-posta adresi) ve bir **Gönder** düğmesi. Kullanıcı formu gönderdiğinde, bir metin dosyasına kullanıcının girişi depolayacağınızı.

1. Adlı yeni bir klasör oluşturun *uygulama\_veri*, zaten yoksa.
2. Web sitenizi kök adlı yeni bir dosya oluşturun *UserData.cshtml*.
3. Mevcut içeriğini aşağıdakiyle değiştirin: 

    [!code-cshtml[Main](working-with-files/samples/sample1.cshtml)]

    HTML biçimlendirmeyi üç metin kutuları formu oluşturur. Kod içinde kullanmanız `IsPost` işleme başlamadan önce sayfa gönderildikten olup olmadığını belirlemek için özellik.

    Kullanıcı girişi almak ve değişkenlere atamak için ilk görevdir bakın. Kod, dizesine sonra farklı bir değişkende depolanan bir virgülle ayrılmış, sonra ayrı değişkenlerin değerleri birleştirir. Virgül ayırıcısına tırnak içinde yer alan bir dize olduğuna dikkat edin (","), virgül, oluşturmakta olduğunuz büyük dizeye tam anlamıyla ekleme yapıyorsanız olduğundan. Verilerin birlikte birleştirme sonunda, eklediğiniz `Environment.NewLine`. Bu, bir satır sonu (yeni satır karakteri) ekler. Bu birleştirme ile oluşturmakta olduğunuz şuna benzer bir dizedir:

    [!code-css[Main](working-with-files/samples/sample2.css)]

    (Bir görünmez satır sonu ile sonunda.)

    Ardından bir değişken oluşturun (`dataFile`) verileri depolamak için dosya adını ve konumunu içerir. Konum ayarlama, bazı özel işlem gerektiriyor. İsteğe bağlı olarak Web siteleri, kod içinde mutlak yollar gibi başvurmak için hatalı bir uygulama olan *C:\Folder\File.txt* web sunucusundaki dosyaları. Bir Web sitesi taşınırsa, mutlak bir yol yanlış olabilir. Ayrıca, barındırılan bir site için (kendi bilgisayarınızda olarak), genellikle bile kodu yazarken doğru yolu nedir bilmiyorum.

    (Bir dosyaya yazmak için ancak bazı durumlarda artık,) gibi bir tam yol gerekir. Çözüm `MapPath` yöntemi `Server` nesne. Bu, uygulamanızın Web sitenize tam yolunu döndürür. Web sitesi kök için kullanıcı olan yolu almak için `~` işleci (site represen için sanal kök) için `MapPath`. (Ayrıca bir alt klasör adı, gibi geçirebilirsiniz *~/App\_veri /*, o alt klasör için olan yolu almak için.) Ardından, üzerine tam yolunu oluşturmak için hangi yöntemi döndürür. ek bilgi bitiştirebilirsiniz. Bu örnekte, bir dosya adı ekleyin. (Daha fazla dosya ve klasör yolları ile çalışma hakkında [ASP.NET Web sayfaları programlama kullanarak Razor sözdizimi giriş](https://go.microsoft.com/fwlink/?LinkId=195205#ID_WorkingWithFileAndFolderPaths).)

    Dosya kaydedilir *uygulama\_veri* klasör. Bu klasör, veri dosyalarını depolamak için açıklandığı gibi kullanılan ASP.NET özel klasöründedir [ASP.NET Web sayfaları sitelerinde bir veritabanıyla çalışmaya giriş](https://go.microsoft.com/fwlink/?LinkId=195209).

    `WriteAllText` Yöntemi `File` nesneyi veri dosyasına yazar. Bu yöntem iki parametre alır: adıyla (yol) yazmak için dosya ve yazılacak gerçek veriler. İlk parametre adını taşıyan bildirimi bir `@` öneki olarak karakter. Bu verbatim dize sabit değeri sağladığınızı ve gibi karakterler ASP "/" özel yollarla yorumlanmamalıdır. (Daha fazla bilgi için [giriş kullanımına ASP.NET Web programlama Razor söz dizimi](https://go.microsoft.com/fwlink/?LinkId=195205#ID_WorkingWithFileAndFolderPaths).)

    > [!NOTE]
    > Dosyaları kaydetmek, kodunuzda sırayla *uygulama\_veri* klasörü, uygulamanın okuma-yazma bu klasörün izinlerini. Geliştirme bilgisayarınızda bu genellikle bir sorun değildir. Ancak, bir barındırma sağlayıcısının web sunucusuna sitenizi yayımladığınızda, açıkça bu izinleri ayarlamak gerekebilir. Bu kod bir barındırma sağlayıcısının sunucusunda çalıştırın ve hata iletisi alıyorum, bu izinleri ayarlama konusunda bilgi edinmek için barındırma sağlayıcısı ile kontrol edin.

- Sayfanın tarayıcıda çalıştırın. 

    ![](working-with-files/_static/image1.jpg)
- Alanlara değerleri girin ve ardından **Gönder**.
- Tarayıcıyı kapatın.
- Projeye dönün ve görünümü yenileyin.
- Açık *data.txt* dosya. Formda gönderilen veriler dosyasıdır. 

    ![[image]](working-with-files/_static/image2.jpg)
- Kapat *data.txt* dosya.

<a id="Appending_Data"></a>
## <a name="appending-data-to-an-existing-file"></a>Varolan bir dosyaya veri ekleme

Önceki örnekte, kullandığınız `WriteAllText` içinde yalnızca bir veri parçasını tablename'e sahip bir metin dosyası oluşturmak için. Yöntemini yeniden çağırın ve aynı dosya adına geçirmek, var olan dosyayı tamamen üzerine yazılır. Bir dosyayı oluşturduktan sonra Bununla birlikte, genellikle yeni bir veri dosyasının sonuna eklemek istediğiniz. Bu kullanarak yapabileceğiniz `AppendAllText` yöntemi `File` nesne.

1. Web sitesi, bir kopyasını *UserData.cshtml* dosya ve kopya adı *UserDataMultiple.cshtml*.
2. Açmadan önce kod bloğunu `<!DOCTYPE html>` aşağıdaki kod bloğu etiketi: 

    [!code-cshtml[Main](working-with-files/samples/sample3.cshtml)]

    Bu kod bir değişiklik önceki örnekten var. Yerine `WriteAllText`, kullandığı `the AppendAllText` yöntemi. Yöntemleri benzer dışında `AppendAllText` veri dosyasının sonuna ekler. Olduğu gibi `WriteAllText`, `AppendAllText` zaten yoksa dosyayı oluşturur.
3. Sayfanın tarayıcıda çalıştırın.
4. Alanlar için değerleri girin ve ardından **Gönder**.
5. Daha fazla veri ekleme ve formu yeniden gönderin.
6. Dönüş projenize, proje klasörü sağ tıklatın ve ardından **Yenile**.
7. Açık *data.txt* dosya. Artık, yeni girdiğiniz yeni veriler içerir. 

    ![[image]](working-with-files/_static/image3.jpg)

<a id="Reading_and_Displaying_Data"></a>
## <a name="reading-and-displaying-data-from-a-file"></a>Bir dosyadan verileri görüntüleme ve okuma

Bir metin dosyasına veri yazma gerekmez olsa bile, büyük olasılıkla bazen birinden veri okumak gerekir. Bunu yapmak için yeniden kullanabileceğiniz `File` nesne. Kullanabileceğiniz `File` her satır ayrı ayrı okuma nesnesine (satır sonlarıyla ayırarak) veya tek bir öğe nasıl ayrılmış ne olursa olsun okumak için.

Bu yordam okumak ve önceki örnekte oluşturulan verileri görüntülemek nasıl gösterir.

1. Web sitenizi kök adlı yeni bir dosya oluşturun *DisplayData.cshtml*.
2. Mevcut içeriğini aşağıdakiyle değiştirin: 

    [!code-cshtml[Main](working-with-files/samples/sample4.cshtml)]

    Kod, önceki örnekte adlı bir değişkende oluşturduğunuz dosyasını okuyarak başlatır `userData`, bu yöntem çağrısı kullanarak:

    [!code-css[Main](working-with-files/samples/sample5.css)]

    Bunu yapmak için kodu içindedir bir `if` deyimi. Bir dosyayı okumak istediğinizde, bunu kullanmak için iyi bir fikirdir `File.Exists` önce dosyanın mevcut olup olmadığını belirlemek için yöntemi. Kod Ayrıca, dosyanın boş olup olmadığını denetler.

    Sayfanın gövdesi iki tane `foreach` döngüsü, bir diğer içinde iç içe geçmiş. Dış `foreach` döngü veri dosyasından bir defada bir satır alır. Bu durumda, satırları dosyadaki satır sonları tanımlanır &#8212; diğer bir deyişle, her veri kendi satırında öğesidir. Yeni bir öğe dış döngü oluşturur (`<li>` öğesi) sıralı bir liste içinde (`<ol>` öğesi).

    İç döngü ayırıcı olarak virgül kullanarak öğeleri (alanlar) her veri satırı ayıran. (Önceki örneği temel alarak, her satırı üç alanı içeren başka bir deyişle &#8212; ad, Soyadı ve e-posta adresi, her bir virgülle ayrılmış.) İç döngü ayrıca oluşturur bir `<ul>` veri satırında her bir alan için madde listesi ve bir listesini görüntüler.

    Kod, iki veri türü, bir dizi kullanmayı gösterir ve `char` veri türü. Dizi gereklidir çünkü `File.ReadAllLines` yöntemi, verileri bir dizi olarak döndürür. `char` Çünkü veri türünün gerekli olduğu `Split` yöntemi döndürür bir `array` her öğe türü olduğu `char`. (Diziler hakkında daha fazla bilgi için bkz: [giriş kullanımına ASP.NET Web programlama Razor söz dizimi](https://go.microsoft.com/fwlink/?LinkId=202890#ID_CollectionsAndObjects).)
3. Sayfanın tarayıcıda çalıştırın. Önceki örneklerde girdiğiniz veriler görüntülenir. 

    ![[image]](working-with-files/_static/image4.jpg)

> [!TIP] 
> 
> **Virgülle ayrılmış bir Microsoft Excel dosyasındaki verileri görüntüleme**
> 
> Virgülle ayrılmış bir dosya olarak bir elektronik tablodaki verileri kaydetmek için Microsoft Excel'i kullanabilirsiniz (*.csv* dosyası). Bunu yaptığınızda dosya düz metin olarak Excel biçiminde kaydedilir. Elektronik tablodaki her satır bir metin dosyasındaki satır sonu tarafından ayrılmış ve her veri öğesinin virgülle ayrılır. Önceki örnekte gösterilen kod, virgülle ayrılmış bir Excel dosyası kodunuzda veri dosyasının adını değiştirerek okumak için kullanabilirsiniz.


<a id="Deleting_Files"></a>
## <a name="deleting-files"></a>Dosyaları silme

Dosyalar, Web sitesinden silmek için kullanabileceğiniz `File.Delete` yöntemi. Bu yordam, kullanıcıların bir görüntüyü silin gösterilmektedir (*.jpg* dosya) gelen bir *görüntüleri* bunlar dosyasının adını biliyorsanız klasör.

> [!NOTE] 
> 
> **Önemli** bir üretim Web sitesine, genelde kimin değişiklik verilere izin kısıtlama. Sitede görevleri gerçekleştirmek için Kullanıcıları yetkilendirmek için yolu ve üyeliği ayarlama hakkında bilgi için [güvenlik ekleme ve bir ASP.NET Web sayfaları sitesinde üyelik](https://go.microsoft.com/fwlink/?LinkId=202904).


1. Adlı bir alt Web sitesi oluşturma *görüntüleri*.
2. Bir veya daha fazla kopyasını *.jpg* dosyalarınızı *görüntüleri* klasör.
3. Web sitesinin kök dizininde adlı yeni bir dosya oluşturmak *FileDelete.cshtml*.
4. Mevcut içeriğini aşağıdakiyle değiştirin: 

    [!code-cshtml[Main](working-with-files/samples/sample6.cshtml)]

    Bu sayfa, kullanıcıların bir görüntü dosyasının adını girebileceğiniz bir form içerir. Bunlar girmeyin *.jpg* dosya adı uzantısı; bu gibi dosya adını kısıtlayarak, Yardım kullanıcıların rastgele dosyalar sitenizde silmesini engeller.

    Kodu, kullanıcının girdiği ve ardından tam bir yol oluşturur dosya adı okur. Kod yolu oluşturmak için geçerli Web sitesi yolunu kullanır (tarafından döndürülen `Server.MapPath` yöntemi), *görüntüleri* klasör adı, sağlanan kullanıcı adı ve bir sabit dize olarak ".jpg".

    Kod çağrıları dosyayı silmek için `File.Delete` yöntemi, yalnızca oluşturulan tam yolunu geçirerek. Biçimlendirme sonunda kod dosyası silindi bir onay iletisi görüntüler.
5. Sayfanın tarayıcıda çalıştırın. 

    ![[image]](working-with-files/_static/image5.jpg)
6. Silin ve ardından dosya adını **Gönder**. Dosya silinmiş, dosyanın adını sayfanın alt kısmında görüntülenir.

<a id="Letting_Users_Upload_a_File"></a>
## <a name="letting-users-upload-a-file"></a>İzin vermek kullanıcı bir dosyayı karşıya yükleyin

`FileUpload` Yardımcı kullanıcılar Web sitenize karşıya dosya yükleme sağlar. Aşağıdaki yordam, kullanıcıların tek bir dosyayı karşıya işlemini göstermektedir.

1. ASP.NET Web Yardımcıları kitaplığı açıklandığı Web sitenize ekleyin [yükleme Yardımcıları bir ASP.NET Web sayfaları sitesinde](https://go.microsoft.com/fwlink/?LinkId=252372), daha önce ekleyemiyor.
2. İçinde *uygulama\_veri* klasöründe yeni bir klasör oluşturun ve adlandırın *UploadedFiles*.
3. Kök dizininde adlı yeni bir dosya oluşturmak *FileUpload.cshtml*.
4. Sayfanın mevcut içeriği aşağıdakiyle değiştirin: 

    [!code-cshtml[Main](working-with-files/samples/sample7.cshtml)]

    Sayfanın gövdesi bölümünü kullanır `FileUpload` yardımcı yükleme kutusu ve büyük olasılıkla alışık olduğunuz bir düğme oluşturmak için:

    ![[image]](working-with-files/_static/image6.jpg)

    İçin ayarladığınız özellikleri `FileUpload` Yardımcısı belirtin, karşıya yüklenecek dosyanın için tek bir kutu istediğiniz ve okuma için Gönder düğmesinin istediğiniz **karşıya**. (Daha fazla kutuları makalenin sonraki bölümlerinde ekleyeceksiniz.)

    Kullanıcı tıkladığında **karşıya**, sayfanın üstündeki kod dosyasını alır ve onu kaydeder. `Request` Form alanının değerleri almak için normalde kullandığınız nesne de sahip bir `Files` dosya (veya dosyaları) içeren bir dizi, karşıya yüklendi. Dizideki belirli konumlara dışında tek tek dosyaları alabilirsiniz &#8212; Örneğin, karşıya yüklenen dosya ilk almak için size `Request.Files[0]`, ikinci dosyasını almak için size `Request.Files[1]`ve benzeri. (Programlamada, genellikle Sayım sıfırdan başladığını unutmayın.)

    Karşıya yüklenen bir dosya getirilemedi, bunu bir değişkende yerleştirdiğiniz (burada `uploadedFile`) ve böylece onu işleyebilirsiniz. Karşıya yüklenen dosya adını belirlemek için yalnızca size kendi `FileName` özelliği. Ancak, ne zaman kullanıcı karşıya bir dosya `FileName` yolun tamamını içerir kullanıcı asıl adını içerir. Şuna benzeyebilir:

    *C:\Users\Public\Sample.txt*

    Yol sunucunuz için değil, kullanıcının bilgisayarında olduğundan, bu yol bilgisi istemezsiniz. Yalnızca gerçek dosya adı istiyorsanız (*örnek.txt*). Yalnızca yoldan dosyasını kullanarak çıkarmanız `Path.GetFileName` böyle bir yöntemi:

    [!code-csharp[Main](working-with-files/samples/sample8.cs)]

    `Path` Nesnedir yöntemler gibi bu yolları Şerit, yolları Birleştir ve benzeri için kullanabileceğiniz bir dizi olan bir yardımcı program.

    Karşıya yüklenen dosya adını yönettiniz sonra Web sitenize karşıya yüklenen dosya depolamak istediğiniz yeri için yeni bir yol oluşturabilirsiniz. Bu durumda, birleştirerek `Server.MapPath`, klasör adları (*uygulama\_veri/UploadedFiles*) ve yeni bir yol oluşturmak için yeni kesilmiş dosya adı. Karşıya yüklenen dosyanın sonra çağırabilirsiniz `SaveAs` gerçekten dosyayı kaydetmek için yöntemi.
5. Sayfanın tarayıcıda çalıştırın. 

    ![[image]](working-with-files/_static/image7.jpg)
6. Tıklayın **Gözat** ve karşıya yüklenecek dosyayı seçin. 

    ![[image]](working-with-files/_static/image8.jpg)

    Metin kutusunun yanındaki **Gözat** düğmesi yolu ve dosya konumunu içerir.

    ![[image]](working-with-files/_static/image9.jpg)
7. Tıklayın **karşıya**.
8. Web sitesi, proje klasörü sağ tıklatın ve ardından **Yenile**.
9. Açık *UploadedFiles* klasör. Karşıya yüklediğiniz dosyanın, klasöründe bulunur. 

    ![[image]](working-with-files/_static/image10.jpg)

<a id="Letting_Users_Upload_Multiple_Files"></a>
## <a name="letting-users-upload-multiple-files"></a>İzin vererek kullanıcılar birden çok dosya yükleme

Önceki örnekte, bir dosyayı karşıya yüklemeyi kullanıcılar olanak tanır. Ancak, kullanabileceğiniz `FileUpload` aynı anda birden fazla dosya yüklemek için yardımcı. Bu, bir kerede tek bir dosya karşıya yükleme yorucu bir süreç olduğu fotoğrafları karşıya yükleme gibi senaryolar için kullanışlıdır. (Fotoğrafları karşıya yükleme hakkında bilgi edinebilirsiniz [bir ASP.NET Web sayfaları sitesinde görüntülerle çalışma](https://go.microsoft.com/fwlink/?LinkId=202897).) Bu örnek, birden fazla karşıya yükleme için aynı tekniği kullanabilseniz iki bir kerede karşıya kullanıcılara bildirmek üzere nasıl gösterir.

1. ASP.NET Web Yardımcıları kitaplığı açıklandığı Web sitenize ekleyin [yükleme Yardımcıları bir ASP.NET Web sayfaları sitesinde](https://go.microsoft.com/fwlink/?LinkId=252372), henüz yapmadıysanız.
2. Adlı yeni bir sayfa oluşturun *FileUploadMultiple.cshtml*.
3. Sayfanın mevcut içeriği aşağıdakiyle değiştirin:  

    [!code-cshtml[Main](working-with-files/samples/sample9.cshtml)]

    Bu örnekte, `FileUpload` yardımcı sayfasının gövdesindeki kullanıcıların iki dosya karşıya yükleme izin vermek için varsayılan olarak yapılandırılır. Çünkü `allowMoreFilesToBeAdded` ayarlanır `true`, Yardımcısı kullanıcı daha fazla karşıya yükleme kutuları Ekle sağlayan bir bağlantı oluşturur:

    ![[image]](working-with-files/_static/image11.jpg)

    Kullanıcı yüklemeleri dosyalarını işlemek için kod, önceki örnekte kullanılan aynı temel teknik kullanır &#8212; bir dosyadan al `Request.Files` ve kaydedin. (Çeşitli şeyler de dahil olmak üzere doğru dosya adını ve yolunu almak için gerçekleştirmeniz gereken.) Bu süre yenilik kullanıcı birden çok dosya karşıya yükleme ve birçok bilmiyorum ' dir. Öğrenmek için alabilirsiniz `Request.Files.Count`.

    Bu sayı, el ile döngüye girer `Request.Files`her dosyasını sırayla getirmek ve kaydedin. Bilinen birkaç kez koleksiyonu aracılığıyla döngü istediğinizde, kullanabileceğiniz bir `for` döngü, şöyle:

    [!code-csharp[Main](working-with-files/samples/sample10.cs)]

    Değişken `i` yalnızca sıfırdan ayarladığınız her bir üst sınırı olacak bir geçici sayacı. Bu durumda, üst sınır dosyalarının sayısıdır. Ancak sayacı sıfırda ASP.NET senaryolarda sayım için tipik olan başladığından üst sınırı aslında bir'den küçük dosya sayısı. (Üç dosyayı karşıya yüklenirse, sayısı 2 sıfırdır.)

    `uploadedCount` Değişkeni başarıyla yüklenmiş ve kaydedilmiş tüm dosyaları toplar. Bu kod için beklenen bir dosya karşıya yüklenecek mümkün olmayabilir olasılığı hesaplar.
4. Sayfanın tarayıcıda çalıştırın. Tarayıcı sayfa ve kendi iki yükleme kutularını görüntüler.
5. Karşıya yüklemek için iki dosya seçin.
6. Tıklayın **başka bir dosyaya ekleme**. Sayfa yeni bir yükleme kutusu görüntüler. 

    ![[image]](working-with-files/_static/image12.jpg)
7. Tıklayın **karşıya**.
8. Web sitesi, proje klasörü sağ tıklatın ve ardından **Yenile**.
9. Açık *UploadedFiles* başarıyla karşıya yüklendi dosyaları görmek için klasör.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Ek Kaynaklar


[Bir ASP.NET Web sayfaları sitesinde görüntülerle çalışma](https://go.microsoft.com/fwlink/?LinkId=202897)

[Bir CSV dosyasına aktarılıyor](https://msdn.microsoft.com/library/ms155919.aspx)
