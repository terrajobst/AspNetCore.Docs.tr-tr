---
uid: web-pages/overview/data/working-with-files
title: "Bir ASP.NET Web sayfaları (Razor) sitesindeki dosyaları ile çalışma | Microsoft Docs"
author: tfitzmac
description: "Bu bölümde, okuma, yazma, ekleme, silme ve dosyaları karşıya yükleme açıklanmaktadır."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: eee916e4-ba4c-439a-a24e-68df7d45a569
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/data/working-with-files
msc.type: authoredcontent
ms.openlocfilehash: 0f119f8fb4873e55292203f21a2efd8f26793ae4
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="working-with-files-in-an-aspnet-web-pages-razor-site"></a>Bir ASP.NET Web sayfaları (Razor) sitesinde dosyalarıyla çalışma
====================
tarafından [zel FitzMacken](https://github.com/tfitzmac)

> Bu makalede, okuma, yazma, ekleme, silme ve ASP.NET Web sayfaları (Razor) sitesi dosyaları karşıya yükleme açıklanmaktadır.
> 
> > [!NOTE]
> > Resimler yükleyin ve bunları değiştirmek istiyorsanız (örneğin, çevirme veya bunları yeniden boyutlandırma), bkz: [bir ASP.NET Web Pages sitesinde görüntülerle çalışma](https://go.microsoft.com/fwlink/?LinkId=202897).
> 
> 
> **Öğrenecekleriniz:** 
> 
> - Bir metin dosyası oluşturun ve ona veri yazmak nasıl.
> - Varolan dosyasına veri eklemek nasıl.
> - Bir dosyayı okuma ve ondan görüntülemek nasıl.
> - Bir Web sitesinden dosyaları silmek nasıl.
> - Kullanıcılar bir dosya veya birden çok dosya karşıya yükleme sağlama.
> 
> Bu makalede sunulan özellikler programlama ASP.NET şunlardır:
> 
> - `File` Dosyaları yönetmek için bir yol sağlayan nesne.
> - `FileUpload` Yardımcısı.
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


<a id="Creating_a_Text_File"></a>
## <a name="creating-a-text-file-and-writing-data-to-it"></a>Bir metin dosyası oluşturma ve veri yazma

Bir veritabanı, Web sitenizdeki kullanmanın yanı sıra dosyalarla çalışabilir. Örneğin, site verilerini depolamak için basit bir yol olarak metin dosyalarını kullanabilirsiniz. (Verileri depolamak için kullanılan bir metin dosyası adlandırılan bir *düz dosya*.) Metin dosyaları olabilir farklı biçimlerde gibi *.txt*, *.xml*, veya *.csv* (virgülle ayrılmış değerler).

Bir metin dosyasına veri depolamak istiyorsanız, kullanabileceğiniz `File.WriteAllText` yöntemi oluşturmak için dosya belirtin ve ona yazılacak veriler. Bu yordamda, üç basit bir form içeren bir sayfa oluşturacaksınız `input` öğeleri (ad, Soyadı ve e-posta adresi) ve bir **gönderme** düğmesi. Kullanıcı formu gönderdiğinde, kullanıcının giriş bir metin dosyasında depolamak.

1. Adlı yeni bir klasör oluşturun *uygulama\_veri*, zaten yoksa.
2. Web sitenizin kök adlı yeni bir dosya oluşturun *UserData.cshtml*.
3. Varolan içeriği aşağıdakiyle değiştirin: 

    [!code-cshtml[Main](working-with-files/samples/sample1.cshtml)]

    HTML biçimlendirmesi form üç metin kutusu oluşturur. Kodda, kullandığınız `IsPost` işleme başlamadan önce sayfa gönderildikten olup olmadığını belirlemek için özellik.

    İlk kullanıcı girişi alma ve değişkenleri atamak için bir görevdir. Kod sonra ayrı değişkenlerin değerleri dizesine sonra farklı bir değişkende depolanan bir virgülle ayrılmış, art arda ekler. Virgül ayırıcısı tırnak işaretleri içinde yer alan bir dize olduğuna dikkat edin (","), virgül, oluşturmakta olduğunuz büyük dizeye tam anlamıyla katıştırma olduğundan. Eklediğiniz birlikte birleştirme veri sonunda `Environment.NewLine`. Satır sonu (yeni satır karakteri) ekler. Bu birleştirme ile oluşturmakta olduğunuz şuna benzer bir dize şöyledir:

    [!code-css[Main](working-with-files/samples/sample2.css)]

    (Bir görünmez satır sonu ile sonunda.)

    Daha sonra bir değişken oluşturma (`dataFile`) verileri depolamak için dosya adını ve konumunu içerir. Konum ayarlamak için bazı özel olarak işlenmesi gerekir. Web siteleri, onu gibi mutlak yollar kodda başvurmak için bir hatalı deneyimdir *C:\Folder\File.txt* web sunucusundaki dosyaları. Bir Web sitesi taşınırsa, mutlak bir yol yanlış olabilir. Ayrıca, barındırılan bir site için (için kendi bilgisayarınızda aygıtlardır) genellikle bile kodu yazarken doğru yolu nedir tanımadığınız.

    (Örneğin, bir dosya yazma için ancak bazen şimdi,), tam bir yol gerekir. Çözüm `MapPath` yöntemi `Server` nesnesi. Bu, Web sitenize tam yolunu döndürür. Web sitesi kök için kullanıcı yolu `~` işleci (site represen için sanal kök) için `MapPath`. (Ayrıca bir alt klasör adı, gibi geçirebilirsiniz *~/App\_veri /*, için o alt klasör yolu.) Ardından, tam yolunu oluşturmak için hangi yöntemi döndürür üzerine ek bilgi arada kullanabilirsiniz. Bu örnekte, bir dosya adı ekleyin. (Daha fazla bilgiyi dosya ve klasör yollarında ile çalışma hakkında [ASP.NET Web sayfalarını programlama kullanarak Razor sözdizimi giriş](https://go.microsoft.com/fwlink/?LinkId=195205#ID_WorkingWithFileAndFolderPaths).)

    Dosya kaydedilir *uygulama\_veri* klasör. Bu klasör açıklandığı gibi veri dosyalarını depolamak için kullanılır ASP.NET özel klasöründedir [ASP.NET Web sayfaları sitelerdeki veritabanı ile çalışmaya giriş](https://go.microsoft.com/fwlink/?LinkId=195209).

    `WriteAllText` Yöntemi `File` nesneyi veri dosyasına yazar. Bu yöntemi iki parametre alır: adıyla (yol) yazmak için dosya ve yazılacak gerçek veriler. İlk parametre adını taşıyan bildirimi bir `@` öneki olarak karakter. Bu verbatim dize sabit değeri sağlama ve gibi karakterler ASP "/" özel yollarla yorumlanmamalıdır. (Daha fazla bilgi için bkz: [ASP.NET Web programlama kullanarak Razor sözdizimi giriş](https://go.microsoft.com/fwlink/?LinkId=195205#ID_WorkingWithFileAndFolderPaths).)

    > [!NOTE]
    > Kodunuz dosyaları kaydetmek için sırayla *uygulama\_veri* klasörü, uygulama okuma-yazma bu klasör için izinleri. Geliştirme bilgisayarınızda bu genellikle bir sorun değildir. Ancak, bir barındırma sağlayıcısının web sunucusuna sitenizi yayımladığınızda, açık bir şekilde bu izinleri ayarlamanız gerekebilir. Bu kodu bir barındırma sağlayıcısının sunucusunda çalıştırın ve hataları alırsanız bu izinleri ayarlamak nasıl öğrenmek için barındırma sağlayıcısına başvurun.

- Bir tarayıcıda. Sayfayı çalıştırın. 

    ![](working-with-files/_static/image1.jpg)
- Alanlarına değerleri girin ve ardından **gönderme**.
- Tarayıcıyı kapatın.
- Projeye dönün ve görünümü yenileyin.
- Açık *data.txt* dosya. Formda gönderilen veri dosyasıdır. 

    ![[Görüntü]](working-with-files/_static/image2.jpg)
- Kapat *data.txt* dosya.

<a id="Appending_Data"></a>
## <a name="appending-data-to-an-existing-file"></a>Varolan bir dosyaya veri ekleme

Önceki örnekte kullanılan `WriteAllText` veri yalnızca bir parçasının içinde var bir metin dosyası oluşturmak için. Yeniden yöntemini çağırın ve aynı dosya adına geçirmek istiyorsanız, var olan dosyayı tamamen üzerine yazılır. Bir dosyayı oluşturduktan sonra ancak genellikle yeni veri dosyasının sonuna eklemek istediğiniz. Bu kullanarak yapabilirsiniz `AppendAllText` yöntemi `File` nesnesi.

1. Web sitesi, bir kopyasını *UserData.cshtml* dosya ve kopya adı *UserDataMultiple.cshtml*.
2. Kod bloğu açmadan önce Değiştir `<!DOCTYPE html>` aşağıdaki kod bloğu etiketi: 

    [!code-cshtml[Main](working-with-files/samples/sample3.cshtml)]

    Bu kod bir değişiklik, içinde önceki örnekten içeriyor. Yerine `WriteAllText`, kullandığı `the AppendAllText` yöntemi. Yöntemlere benzer dışında `AppendAllText` veri dosyasının sonuna ekler. İle `WriteAllText`, `AppendAllText` zaten yoksa dosyası oluşturur.
3. Bir tarayıcıda. Sayfayı çalıştırın.
4. Alanları için değerleri girin ve ardından **gönderme**.
5. Daha fazla veri ekleme ve formu yeniden gönderin.
6. Dönüş projenize, proje klasörüne sağ tıklayın ve ardından **yenileme**.
7. Açık *data.txt* dosya. Şimdi girdiğiniz yeni verileri de içerir. 

    ![[Görüntü]](working-with-files/_static/image3.jpg)

<a id="Reading_and_Displaying_Data"></a>
## <a name="reading-and-displaying-data-from-a-file"></a>Okuma ve bir dosyadan verileri görüntüleme

Verileri bir metin dosyasına yazma gerekmez olsa bile, büyük olasılıkla bazen birinden verileri okumak gerekir. Bunu yapmak için tekrar kullanabilirsiniz `File` nesnesi. Kullanabileceğiniz `File` her satır ayrı ayrı okumak için nesne (satır sonları tarafından ayrılmış olarak) veya nasıl ayrılmış olsun tek bir öğe okunamıyor.

Bu yordamda okuyun ve önceki örnekte oluşturulan veri görüntüler gösterilmiştir.

1. Web sitenizin kök adlı yeni bir dosya oluşturun *DisplayData.cshtml*.
2. Varolan içeriği aşağıdakiyle değiştirin: 

    [!code-cshtml[Main](working-with-files/samples/sample4.cshtml)]

    Kod, önceki örnekte adlı bir değişkende oluşturduğunuz dosyasını okuyarak başlatır `userData`, bu yöntem çağrısı kullanarak:

    [!code-css[Main](working-with-files/samples/sample5.css)]

    Bunu yapmak için kod içindedir bir `if` deyimi. Bir dosyayı okuma istediğinizde, bunu kullanmak için iyi bir fikirdir `File.Exists` önce dosyanın mevcut olup olmadığını belirlemek amacıyla yöntemi. Kod ayrıca dosya boş olup olmadığını denetler.

    Sayfa gövdesi iki içeren `foreach` döngüsü, bir diğer içe geçmiş. Dış `foreach` döngü veri dosyasından bir defada bir satır alır. Bu durumda, satırları satır sonları dosya &#8212;tanımlanır; diğer bir deyişle, her bir veri öğesi kendi satırında ' dir. Yeni bir öğe dış döngü oluşturur (`<li>` öğesi) sıralı liste içinde (`<ol>` öğesi).

    İç döngü ayırıcı olarak virgül kullanarak öğeleri (alanları) her veri satırı ayıran. (Önceki örneği temel alarak, bu üç alan &#8212;her satırında anlamına gelir; ilk adını, soyadını ve e-posta adresi, her bir virgülle ayrılmış) İç döngü da oluşturur bir `<ul>` veri satırına her bir alan için madde listesi ve bir listesini görüntüler.

    Kod iki veri türü, bir dizi kullanmayı gösterir ve `char` veri türü. Dizi gereklidir çünkü `File.ReadAllLines` yöntemi veri dizisi olarak döndürür. `char` Veri türü, çünkü gereklidir `Split` yöntemi döndürür bir `array` her öğe türü olduğu `char`. (Dizileri hakkında daha fazla bilgi için bkz: [ASP.NET Web programlama kullanarak Razor sözdizimi giriş](https://go.microsoft.com/fwlink/?LinkId=202890#ID_CollectionsAndObjects).)
3. Bir tarayıcıda. Sayfayı çalıştırın. Önceki örneklerde girdiğiniz veriler görüntülenir. 

    ![[Görüntü]](working-with-files/_static/image4.jpg)

> [!TIP] 
> 
> **Microsoft Excel virgülle ayrılmış bir dosyadan verileri görüntüleme**
> 
> Microsoft Excel elektronik tablo virgülle ayrılmış bir dosya olarak bulunan verileri kaydetmek için kullanabileceğiniz (*.csv* dosyası). Bunu yaptığınızda, Excel biçiminde değil düz metin dosyası kaydedilir. Elektronik tablodaki her satır bir metin dosyasındaki satır sonu ile ayrılır ve her bir veri öğesi virgülle ayrılmış. Yalnızca kodunuzun veri dosyasında adını değiştirerek virgülle ayrılmış bir Excel dosyasını okumak için önceki örnekte gösterilen kodu kullanabilirsiniz.


<a id="Deleting_Files"></a>
## <a name="deleting-files"></a>Dosyaları silme

Web sitesinden dosyaları silmek için kullanabileceğiniz `File.Delete` yöntemi. Bu yordam, kullanıcıların bir görüntü Sil izin verecek şekilde gösterilmektedir (*.jpg* dosyası) gelen bir *görüntüleri* dosyanın adını biliyorsanız, klasör.

> [!NOTE] 
> 
> **Önemli** bir üretim Web sitesi, genellikle kimin verilerde değişiklik yapabilir izin kısıtlayın. Sitesinde görevleri gerçekleştirmek için Kullanıcıları yetkilendirmek için yol ve üyelik ayarlama hakkında bilgi için bkz: [güvenlik ekleme ve bir ASP.NET Web sayfaları Site üyeliği](https://go.microsoft.com/fwlink/?LinkId=202904).


1. Adlı bir alt Web sitesi oluşturma *görüntüleri*.
2. Bir veya daha fazla kopyasını *.jpg* içine dosyaları *görüntüleri* klasör.
3. Web sitesinin kök dizininde adlı yeni bir dosya oluşturun *FileDelete.cshtml*.
4. Varolan içeriği aşağıdakiyle değiştirin: 

    [!code-cshtml[Main](working-with-files/samples/sample6.cshtml)]

    Bu sayfa, kullanıcıların bir görüntü dosyasının adını girebilecekleri bir form içerir. Bunlar girmeyin *.jpg* dosya adı uzantısı; dosya adı bu gibi kısıtlayarak, Yardım kullanıcıların, sitenizde rastgele dosyalar silmesi önlenir.

    Kod kullanıcının girdiği ve tam yolunu oluşturur dosya adını okur. Kod yolu oluşturmak için geçerli Web sitesi yolu kullanır (tarafından döndürülen `Server.MapPath` yöntemi), *görüntüleri* klasör adı, kullanıcı tarafından sağlanan ad ve sabit değerli bir dize olarak ".jpg".

    Kod çağrıları dosyayı silmek için `File.Delete` yöntemi, az önce oluşturulan tam yolunu geçirme. İşaretleme sonunda kod dosyası silindi bir onay iletisi görüntüler.
5. Bir tarayıcıda. Sayfayı çalıştırın. 

    ![[Görüntü]](working-with-files/_static/image5.jpg)
6. Silin ve ardından dosyanın adını girin **gönderme**. Dosya silinmişse, dosyanın adını sayfasının en altında görüntülenir.

<a id="Letting_Users_Upload_a_File"></a>
## <a name="letting-users-upload-a-file"></a>Veren kullanıcılar karşıya bir dosya

`FileUpload` Yardımcı kullanıcıların Web sitenize dosyaları karşıya yükleme. Aşağıdaki yordam tek bir dosyayı karşıya kullanıcıların izin gösterilmiştir.

1. ASP.NET Web Yardımcıları kitaplığı açıklandığı gibi Web sitenize ekleyin [yükleme Yardımcıları bir ASP.NET Web Pages sitesinde](https://go.microsoft.com/fwlink/?LinkId=252372), daha önce ekleyemiyor.
2. İçinde *uygulama\_veri* klasörü, yeni bir klasör oluşturun ve adlandırın *UploadedFiles*.
3. Kök dizininde adlı yeni bir dosya oluşturun *FileUpload.cshtml*.
4. Sayfa varolan içeriği aşağıdakiyle değiştirin: 

    [!code-cshtml[Main](working-with-files/samples/sample7.cshtml)]

    Sayfa gövde bölümünü kullanır `FileUpload` Yardımcısı yükleme kutusu ve büyük olasılıkla aşina düğmeleri oluşturun:

    ![[Görüntü]](working-with-files/_static/image6.jpg)

    İçin ayarladığınız özellikleri `FileUpload` yardımcı karşıya yüklenecek dosyayı için tek bir kutu istediğiniz ve okuma için Gönder düğmesinin istediğinizi belirtin **karşıya**. (Daha fazla kutuları makalenin sonraki bölümlerinde eklenecektir.)

    Kullanıcı tıkladığında **karşıya**, sayfanın üst kısmındaki kod dosyası alır ve kaydeder. `Request` Form alanlardan değerlerini almak için normalde kullandığınız nesnesi de sahip bir `Files` dosya (veya dosyaları) içeren bir dizi, yüklediğiniz. Dizi &#8212;belirli konumlara dışında tek tek dosyaları alabilirsiniz; Örneğin, ilk yüklenen dosya almak için size `Request.Files[0]`, ikinci dosya almak için size `Request.Files[1]`ve benzeri. (Programlamada genellikle sayım sıfırda başladığını unutmayın.)

    Yüklenen dosya getirme, onu bir değişkende yerleştirdiğiniz (burada, `uploadedFile`) böylece üzerinde çalışabilirsiniz. Yüklenen dosya adını belirlemek için yalnızca size kendi `FileName` özelliği. Ancak, ne zaman kullanıcı karşıya yükleyen bir dosya `FileName` tam yolu içerir kullanıcının özgün adı içeriyor. Şuna benzeyebilir:

    *C:\Users\Public\Sample.txt*

    Sunucunuz için değil, kullanıcının bilgisayarındaki yolu olduğu için bu yol bilgileri yine de istemezsiniz. Yalnızca gerçek dosya adı istediğiniz (*örnek.txt*). Yalnızca bir yoldan dosya kullanarak Şerit `Path.GetFileName` yöntemi şuna benzer:

    [!code-csharp[Main](working-with-files/samples/sample8.cs)]

    `Path` Çeşitli yöntemler yolları Şerit, yolları birleştirmek ve benzeri için kullanabileceğiniz şöyle sahip bir yardımcı programı bir nesnedir.

    Karşıya yüklenen dosyanın adını kabulünüzü sonra siteniz yüklenen dosya depolamak istediğiniz yeri için yeni bir yol oluşturabilirsiniz. Bu durumda, birleştirme `Server.MapPath`, klasör adları (*uygulama\_veri/UploadedFiles*) ve yeni bir yol oluşturmak için yeni kırpılmış dosya adı. Karşıya yüklenen dosyanın sonra çağırabilirsiniz `SaveAs` yöntemi gerçekte dosyayı kaydedin.
5. Bir tarayıcıda. Sayfayı çalıştırın. 

    ![[Görüntü]](working-with-files/_static/image7.jpg)
6. Tıklatın **Gözat** ve karşıya yüklemek için bir dosya seçin. 

    ![[Görüntü]](working-with-files/_static/image8.jpg)

    Metin kutusunun yanındaki **Gözat** düğmesi yolu ve dosya konumunu içerir.

    ![[Görüntü]](working-with-files/_static/image9.jpg)
7. Tıklatın **karşıya**.
8. Web sitesi, proje klasörüne sağ tıklayın ve ardından **yenileme**.
9. Açık *UploadedFiles* klasör. Karşıya dosya klasöründe bulunur. 

    ![[Görüntü]](working-with-files/_static/image10.jpg)

<a id="Letting_Users_Upload_Multiple_Files"></a>
## <a name="letting-users-upload-multiple-files"></a>İzin vererek kullanıcıların birden çok dosya karşıya yükleme

Önceki örnekte, bir dosyayı karşıya yüklemeyi açmalarına olanak tanır. Ancak, kullanabileceğiniz `FileUpload` aynı anda birden fazla dosyayı karşıya yüklemeyi Yardımcısı. Bu, aynı anda bir dosyayı karşıya yüklemeyi can sıkıcı olduğu fotoğrafları, karşıya yükleme gibi senaryolar için kullanışlıdır. (Fotoğrafları karşıya yükleme hakkında bilgi edinebilirsiniz [bir ASP.NET Web Pages sitesinde görüntülerle çalışma](https://go.microsoft.com/fwlink/?LinkId=202897).) Bu örnek, birden fazla karşıya yüklemek için aynı tekniği kullanabilirsiniz ancak aynı anda iki karşıya kullanıcıların izin vermek nasıl gösterir.

1. ASP.NET Web Yardımcıları kitaplığı açıklandığı gibi Web sitenize ekleyin [yükleme Yardımcıları bir ASP.NET Web Pages sitesinde](https://go.microsoft.com/fwlink/?LinkId=252372), henüz yapmadıysanız.
2. Adlı yeni bir sayfa oluşturma *FileUploadMultiple.cshtml*.
3. Sayfa varolan içeriği aşağıdakiyle değiştirin:  

    [!code-cshtml[Main](working-with-files/samples/sample9.cshtml)]

    Bu örnekte, `FileUpload` yardımcı sayfasının gövdesindeki iki dosyaları karşıya yükleme kullanıcıların izin vermek için varsayılan olarak yapılandırılır. Çünkü `allowMoreFilesToBeAdded` ayarlanır `true`, Yardımcısı kullanıcı daha fazla karşıya yükleme kutuları Ekle olanak sağlayan bir bağlantı oluşturur:

    ![[Görüntü]](working-with-files/_static/image11.jpg)

    Kullanıcı karşıya yükleme dosyalarını işlemek için önceki örnekte &#8212;kullanılan aynı temel teknik kod kullanır; bir dosyadan al `Request.Files` ve kaydedin. (Çeşitli öğeler de dahil olmak üzere, doğru dosya adını ve yolunu almak için yapmanız gerekir.) Bu süre yenilik, kullanıcı birden çok dosya karşıya yükleme ve birçok tanımadığınız ' dir. Öğrenmek için alabilirsiniz `Request.Files.Count`.

    Elle bu numara ile aracılığıyla döngüye girer `Request.Files`her dosyayı sırayla almak ve kaydedin. Bilinen kaç kez koleksiyonu üzerinden döngü istediğinizde, kullanabileceğiniz bir `for` döngü şuna benzer:

    [!code-csharp[Main](working-with-files/samples/sample10.cs)]

    Değişkeni `i` ayarladığınız herhangi bir üst sınır sıfırdan gidecek yalnızca bir geçici sayacıdır. Bu durumda, üst sınır dosya sayısıdır. Ancak sayacı sıfırda ASP.NET senaryolarda sayım için tipik olan başladığından üst sınırı gerçekte bir'den küçük dosya sayısı. (Üç dosyaları karşıya yüklediyseniz sayısı 2 sıfırdır.)

    `uploadedCount` Değişkeni başarıyla karşıya yüklendi ve kaydedilmiş olan tüm dosyaları toplar. Bu kod beklenen dosya karşıya yüklenecek mümkün olmayabilir olasılığı için hesapları.
4. Bir tarayıcıda. Sayfayı çalıştırın. Tarayıcı sayfası ve onun iki karşıya yükleme kutularını görüntüler.
5. Karşıya yüklemek için iki dosya seçin.
6. Tıklatın **başka bir dosya eklemek**. Sayfaya yeni bir karşıya yükleme kutusu görüntüler. 

    ![[Görüntü]](working-with-files/_static/image12.jpg)
7. Tıklatın **karşıya**.
8. Web sitesi, proje klasörüne sağ tıklayın ve ardından **yenileme**.
9. Açık *UploadedFiles* başarıyla karşıya yüklenen dosyaların görmek için klasör.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Ek Kaynaklar


[Bir ASP.NET Web Pages sitesinde görüntüleri ile çalışma](https://go.microsoft.com/fwlink/?LinkId=202897)

[Bir CSV dosyasına dışarı aktarma](https://msdn.microsoft.com/library/ms155919.aspx)
