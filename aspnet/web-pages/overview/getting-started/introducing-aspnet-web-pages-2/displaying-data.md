---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data
title: ASP.NET Web sayfaları sunarak - verileri görüntüleme | Microsoft Docs
author: tfitzmac
description: Bu öğretici Webmatrix'te bir veritabanı oluşturmak nasıl ve ASP.NET Web sayfaları (Razor) kullandığınızda veritabanı verilerinin bir sayfasında nasıl görüntüleneceğini gösterir. Y varsayar...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: b3a006a0-3ea2-4d45-b833-e20e3a3c0a1a
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data
msc.type: authoredcontent
ms.openlocfilehash: 6c66e5fb0a1a49da411286e19c7954f83055c3fd
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30898463"
---
<a name="introducing-aspnet-web-pages---displaying-data"></a>ASP.NET Web sayfaları sunarak - verileri görüntüleme
====================
tarafından [zel FitzMacken](https://github.com/tfitzmac)

> Bu öğretici Webmatrix'te bir veritabanı oluşturmak nasıl ve ASP.NET Web sayfaları (Razor) kullandığınızda veritabanı verilerinin bir sayfasında nasıl görüntüleneceğini gösterir. Seri aracılığıyla tamamladığınızdan varsayar [ASP.NET Web sayfaları programlama giriş](../introducing-razor-syntax-c.md).
> 
> Öğrenecekleriniz:
> 
> - Nasıl bir veritabanı ve veritabanı tabloları oluşturmak için WebMatrix araçlarını kullanın.
> - Verileri bir veritabanına eklemek için WebMatrix araçlarını kullanma
> - Veritabanı verileri bir sayfada görüntülemek nasıl.
> - ASP.NET Web Pages'da SQL komutlarını çalıştırmak nasıl.
> - Nasıl özelleştirileceğini `WebGrid` yardımcı verilerin görüntülenme biçimini değiştirme ve disk belleği ve sıralama eklemek için.
>   
> 
> Özellikler/teknolojilerini ele alınan:
> 
> - WebMatrix veritabanı araçları.
> - `WebGrid` Yardımcısı.


## <a name="what-youll-build"></a>Ne oluşturacağınız

Önceki öğreticide, ASP.NET Web sayfaları için sunulan (*.cshtml* dosyaları), Razor sözdizimi ile ilgili temel bilgiler ve Yardımcıları için. Bu öğreticide, seri geri kalanı için kullanacağınız gerçek web uygulaması oluşturma başlarsınız. Uygulamayı görüntülemek, eklemek, değiştirmek ve filmler ilişkin bilgileri silmek olanak sağlayan bir basit film uygulamasıdır.

Bu öğreticiyi tamamladığınızda, bu sayfayı gibi görünüyor film listesini görüntülemek kullanabileceksiniz:

![CSS sınıf adları için parametreleri içeren WebGrid görüntü ayarlama](displaying-data/_static/image1.png)

Ancak başlamak için bir veritabanı oluşturmanız gerekir.

## <a name="a-very-brief-introduction-to-databases"></a>Veritabanları için çok kısa bir giriş

Bu öğretici veritabanları yalnızca briefest giriş sağlar. Veritabanı deneyimi varsa, kısa bu bölümü atlayabilirsiniz.

Veritabanı bilgileri içeren bir veya daha fazla tabloları içeren &mdash; Örneğin, müşteriler, siparişler ve satıcılar veya Öğrenciler, Öğretmen, sınıflar ve dereceleri tabloları. Yapısal olarak, bir veritabanı tablosunda bir elektronik tablo gibi ' dir. Bir genel adres defteri düşünün. Adres defteri her giriş için (diğer bir deyişle, her kişi için) ad, Soyadı, adresini, e-posta adresi ve telefon numarası gibi bilgileri çeşitli parçalarını sahip.

![Örnek veritabanı tablosu basit bir kılavuz olarak](displaying-data/_static/image2.png)

(Satırları bazen denir *kayıtları*, ve sütunları bazen denir *alanları*.)

Çoğu veritabanı tabloları, tablonun bir müşteri numarası, hesap numarası vb. gibi benzersiz bir değer içeren bir sütun sahip olması gerekir. Bu değer tablonun bilinen *birincil anahtar*, ve tablodaki her satır tanımlamak için kullanın. Örnekte, önceki örnekte gösterilen adres defteri için birincil anahtarı kimliği sütundur.

Web uygulamalarında bunu işin çoğunu veritabanından bilgileri okumak ve bir sayfada görüntülemek oluşur. Ayrıca sıklıkla kullanıcılardan bilgi toplamak ve bir veritabanına eklemek veya zaten veritabanında olan kayıtlarını değiştirmeniz. (Biz Bu öğretici kümesi esnasında bu işlemlerin tümü ele alacağız.)

Veritabanı çalışma enormously karmaşık olabilir ve Uzman bilgilerini gerektirebilir. Bu öğretici kümesi için yine de bunları ilerledikçe, tüm açıklandığı yalnızca temel kavramları anlamanız gerekir.

## <a name="creating-a-database"></a>Veritabanı oluşturma

WebMatrix kolaylaştıran bir veritabanı oluşturmak ve veritabanında tablolarda oluşturmak için Araçlar içerir. (Bir veritabanı yapısını veritabanı adlandırılır *şema*.) Bu öğretici kümesi için yalnızca bir tablo içerdiği bir veritabanı oluşturacaksınız &mdash; filmler.

Zaten yapmadıysanız WebMatrix açın ve önceki öğreticide oluşturduğunuz WebPagesMovies siteyi açın.

Sol bölmede **veritabanı** çalışma.

![WebMatrix veritabanı çalışma alanı sekmesi](displaying-data/_static/image3.png)

Veritabanı ile ilgili görevleri göstermek için Şerit değişir. Şeritte tıklatın **yeni veritabanı**.

![WebMatrix Şeritte 'Yeni Veritabanı' düğmesi](displaying-data/_static/image4.png)

WebMatrix, SQL Server CE veritabanı oluşturur (bir *.sdf* dosyası) siteniz olarak aynı ada sahip &mdash; *WebPagesMovies.sdf*. (Bu burada olmaz ancak onu olduğu sürece, dosyayı istediğiniz bir şey adlandırabilirsiniz bir *.sdf* uzantısı.)

## <a name="creating-a-table"></a>Tablo oluşturma

Şeritte tıklatın **yeni tablo**. WebMatrix Tablo Tasarımcısı yeni bir pencerede açılır. (Yeni bir tablo seçeneği kullanılabilir değilse, yeni veritabanı soldaki ağaç görünümünde seçili olduğundan emin olun.)

![WebMatrix Şeritte 'Yeni Table' düğmesi](displaying-data/_static/image5.png)

(Burada Filigran "Enter tablo adı" diyor) üst metin kutusuna "Filmler" girin.

![WebMatrix Veritabanı Tasarımcısı'nda bir tablo adı girme](displaying-data/_static/image6.png)

Tablo adı altında ayrı ayrı sütunları tanımladığınız yerlerde bölmesidir. İçin *filmler* tablo Bu öğreticide, yalnızca birkaç sütun oluşturacaksınız: *kimliği*, *başlık*, *Tarz*, ve *yıl*.

İçinde **adı** kutusunda, "ID" girin. Buraya bir değer girme yeni bir sütun için tüm denetimler etkinleştirir.

İçin sekme **veri türü** listesinde ve seçin **int**. Bu değer ID sütunu tamsayı (sayı) veri içerdiğini belirtir.

> [!NOTE]
> Herhangi bir şuradan daha fazla (çok) adlandırılır çalışmaz, ancak bu kılavuzda gezinmek için standart Windows klavye hareketleri kullanabilirsiniz. Örneğin, alanlar arasında sekmesinde, yalnızca, bir listesindeki bir öğeyi seçmek için yazmaya başlayın ve benzeri.


Geçmiş sekmesinde **varsayılan değer** kutusunu (yani, boş bırakın). İçin sekme **birincil anahtarı** onay kutusunu işaretleyin ve seçin. Bu seçenek, veritabanını söyler *kimliği* sütun tek tek satır tanımlayan verileri içerir. (Diğer bir deyişle, her satırda benzersiz bir değer satır bulmak için kullanabileceğiniz Kimlik sütununda sahip olacaktır.)

Seçin **Is Identity** seçeneği. Bu seçenek, veritabanı, otomatik olarak yeni her satır için bir sonraki sıralı numara oluşturacağı söyler. ( **Is Identity** , aynı zamanda yalnızca seçtiğiniz durumunda works seçeneği **birincil anahtarı** seçeneği.)

Sonraki kılavuz satıra tıklayın veya geçerli satırda iki kez tamamlamak için SEKME tuşuna basın. Her iki hareketi geçerli satır kaydeder ve bir sonraki başlatır. Dikkat **varsayılan değer** sütun şimdi diyor **Null**. (Varsayılan değer için varsayılan değer speak kadar çok NULL'dur.)

Ne zaman tamamlandı yeni tanımlama **kimliği** sütun, tasarımcının Bu çizim gibi görünür:

![Kimlik sütunu filmler tablo için tanımlama sonra WebMatrix Veritabanı Tasarımcısı](displaying-data/_static/image7.png)

Sonraki sütun oluşturmak için kutuya tıklayın **adı** sütun. Sütun için "Title" girin ve ardından **nvarchar** için **veri türü** değeri. "Var" bölümünü **nvarchar** bu sütun için veri büyüklüğü kayıt kaydı farklılık bir dize olmasını veritabanı söyler. (Alan bir harf veya sistem yazma için karakter veri tutabilen belirten "n" öneki "ulusal" temsil eder — diğer bir deyişle, alanın Unicode verileri tutar.)

Seçtiğinizde **nvarchar**, alan için en fazla uzunluk girebilecekleri başka bir kutusu görüntülenir. Bu öğreticide ile karşılaşmayacağınızı hiçbir filmi 50 karakterden daha uzun olacağını varsayım üzerinde 50 girin.

Atla **varsayılan değer** ve temizleyin **null değerlere izin ver** seçeneği. Tüm filmler veritabanına girilir bir başlık sahip olmayan izin vermek için veritabanı istemezsiniz.

İşiniz bittiğinde ve sonraki satıra taşıma Tasarımcı Bu çizim gibi görünür:

![Film tablosu için başlık sütunu tanımlama sonra WebMatrix Veritabanı Tasarımcısı](displaying-data/_static/image8.png)

Uzunluk dışında "Tarz" adlı bir sütun oluşturmak için bu adımları yalnızca 30 ayarlamak yineleyin.

"Yıl" adlı başka bir sütun oluşturun Veri türü için seçin **nchar** (değil **nvarchar**) ve uzunluk 4'e ayarlayın. Bir değişken boyutlu sütun gerektirmeyen şekilde yıl için "1995" veya "2010" gibi 4 basamaklı bir sayı kullanın paylaşacağız.

Tamamlanmış tasarım benzer aşağıda verilmiştir:

![Tüm alanlar için filmler tablosu tanımlandıktan sonra WebMatrix Veritabanı Tasarımcısı](displaying-data/_static/image9.png)

CTRL + S tuşlarına basın veya tıklatın **kaydetmek** hızlı erişim araç çubuğu düğmesi. Veritabanı Tasarımcısı'nda sekmesini kapatarak kapatın.

## <a name="adding-some-example-data"></a>Bazı örnek veri ekleme

Daha sonra Bu öğretici serisinde bir formda yeni filmler girebilecekleri bir sayfa oluşturacaksınız. Şimdilik, ancak daha sonra bir sayfa üzerinde görüntüleyebilirsiniz bazı örnek veriler ekleyebilirsiniz.

İçinde **veritabanı** WebMatrix, olmadığını gösteren bir ağaç olduğuna dikkat edin Workspace'te *.sdf* daha önce oluşturduğunuz dosya. Düğüm, yeni açmak *.sdf* dosya ve ardından açın **tabloları** düğümü.

![Ağaç filmler tabloya açık ile WebMatrix veritabanı çalışma alanı](displaying-data/_static/image10.png)

Sağ **filmler** düğümünü ve ardından **veri**. WebMatrix açar burada girebilirsiniz veri için bir kılavuz *filmler* tablosu:

![Veritabanı girişi kılavuzunda WebMatrix (boş)](displaying-data/_static/image11.png)

Tıklatın **başlık** sütun ve "Olduğunda Harry karşılanır değiştirmemesi" girin. Taşıma **Tarz** (için SEKME tuşunu kullanabilirsiniz) sütun ve "Romantik Komedi" girin. Taşıma **yıl** sütun ve "1989" girin:

![Veritabanı girişi kılavuzunda WebMatrix bir kayıtla](displaying-data/_static/image12.png)

Enter tuşuna basın ve WebMatrix yeni film kaydeder. Dikkat **kimliği** sütun doldurulur.

![Veritabanı girişi kılavuzunda WebMatrix bir kayıt ve otomatik olarak oluşturulan kimliği](displaying-data/_static/image13.png)

Başka bir filmi (örneğin, "gitti ile Rüzgar", "Ekranda", "1939") girin. ID sütunu yeniden doldurulur:

![Veritabanı girişi kılavuzunda WebMatrix iki kayıtlarla ve otomatik olarak oluşturulan kimlikleri](displaying-data/_static/image14.png)

Üçüncü bir filmi (örneğin, "Ghostbusters", "Komedi") girin. Bir deney bırakın **yıl** sütununu boş bırakın ve ardından Enter tuşuna basın. Seçili olduğundan **null değerlere izin ver** seçenek, veritabanını bir hata gösterir:

![Gerekli sütun değeri boş bırakılırsa 'Geçersiz veri' hatası görüntülenir](displaying-data/_static/image15.png)

Tıklatın **Tamam** geri dönün ve ("Ghostbusters" için yıl de 1984) girişi düzeltin ve ardından Enter tuşuna basın.

Çeşitli filmler 8 kadar veya şekilde doldurun. (8 girerek, daha sonra disk belleği ile çalışmak kolaylaştırır. Ancak, çok fazla ise, birkaç şimdilik girin.) Gerçek veri önemli değildir.

![Veritabanı girişi kılavuzunda WebMatrix ya da kayıtlarla](displaying-data/_static/image16.png)

Hatasız tüm filmler girdiğiniz kimlik değerleri sıralı olarak kullanılır. Tamamlanmamış film Kaydet çalıştıysanız kimlik numaraları sıralı olmayabilir. Öyleyse, bu sorun değildir. Sayıları yapısında bir anlamı yoktur ve önemli olan tek şey benzersiz *filmler* tablo.

Veritabanı Tasarımcısı'nda içeren sekmeyi kapatın.

Şimdi bu verileri bir web sayfasında görüntüleme için etkinleştirebilirsiniz.

## <a name="displaying-data-in-a-page-by-using-the-webgrid-helper"></a>WebGrid Yardımcısını kullanarak bir sayfasında verileri görüntüleme

Verileri bir sayfada görüntülemek için kullanacağınız `WebGrid` Yardımcısı. Bu yardımcı kılavuz veya tablosunda (satırları ve sütunları) bir görüntü oluşturur. Gördüğünüz gibi mümkün İyileştir biçimlendirme ve diğer özelliklerle kılavuz olacaktır.

Kılavuz çalıştırmak için birkaç satır kod yazmanız gerekir. Bu birkaç satır düzeni neredeyse tüm Bu öğreticide bunu veri erişimi için bir tür olarak hizmet verecektir.

> [!NOTE]
> Aslında, verileri bir sayfada görüntülemek için birçok seçeneğiniz de vardır; `WebGrid` yardımcı tek dosyasıdır. Bu öğreticide, verileri görüntülemek için en kolay yolu olduğundan ve makul esnek olduğundan seçtik. Sonraki öğretici kümesinde verilerinin nasıl görüntüleneceğini üzerinde daha doğrudan denetim sağlayan sayfasındaki verilerle çalışmak için daha "manual" yolu kullanmayı görürsünüz.


WebMatrix sol bölmede tıklatın **dosyaları** çalışma.

Oluşturduğunuz yeni veritabanı *uygulama\_veri* klasör. Klasör zaten var alamadık, WebMatrix yeni veritabanınız için oluşturuldu. (Yardımcıları daha önce yüklediyseniz klasörü var.)

Ağaç görünümünde, Web sitesinin kök seçin. Web sitesinin kök seçmeniz gerekir; Aksi takdirde, uygulama için yeni dosya eklenebilir\_veri klasörü.

Şeritte tıklatın **yeni**. İçinde **bir dosya türünü seçin** kutusunda, seçin **CSHTML**.

İçinde **adı** kutusunda, yeni sayfa "Movies.cshtml" adı:

!['Filmler' sayfasını gösteren 'Bir dosya türünü seçin' iletişim kutusu](displaying-data/_static/image17.png)

Tıklatın **Tamam** düğmesi. WebMatrix yeni bir dosya iskelet bazı öğelerinde ile açılır. İlk veritabanından veri almak gitmek için bazı kod yazacaksınız. Ardından gerçekte verileri görüntülemek için sayfanın biçimlendirme ekleyeceksiniz.

### <a name="writing-the-data-query-code"></a>Veri sorgu kod yazma

Sayfanın üstündeki arasında `@{` ve `}` karakter aşağıdaki kodu girin. (Bu kod açma ve kapatma küme ayraçları arasında girdiğinizden emin olun.)

[!code-csharp[Main](displaying-data/samples/sample1.cs)]

İlk satırı daha önce oluşturduğunuz veritabanı, veritabanı ile bir şey yapmadan önce her zaman ilk adımı olduğu açılır. Size `Database.Open` açmak için veritabanının yöntemi adı. Eklemezseniz bildirimi *.sdf* adı. `Open` Yöntemi varsayar aramakta, bir *.sdf* dosyası (diğer bir deyişle, *WebPagesMovies.sdf*) ve *.sdf* dosyası *uygulama\_ Veri* klasör. (Daha önce biz, not ettiğiniz *uygulama\_veri* klasörü ayrılmıştır; burada ASP.NET yapar bu ad ilgili yerlerden biri bu senaryo.)

Veritabanı açıldığında bir başvuru adlı değişken konur `db`. (Olan her şeyi adlandırılabilir.) `db` Değişkenidir veritabanıyla etkileşim kurma nasıl elde edersiniz.

Veritabanı verileri gerçekten getirir kullanarak ikinci satır `Query` yöntemi. Bu kodu nasıl çalıştığını dikkat edin: `db` değişkeni açılan veritabanına bir başvuru içeriyor ve çağırmayı `Query` kullanarak yöntemi `db` değişkeni (`db.Query`).

Bir SQL sorgusu olduğundan `Select` deyimi. (Bkz. açıklama daha sonra SQL hakkında biraz arka plan için.) Deyiminde `Movies` sorgu tabloya tanımlar. `*` Karakteri belirtir sorgu tablodan tüm sütunları döndürmelidir. (Ayrıca sütunları ayrı ayrı virgülle ayırarak listelediğiniz.)

Sorgunun sonuçlarını varsa, döndürülen ve içinde kullanılabilir hale `selectedData` değişkeni. Yeniden değişkeni herhangi bir şey adlandırılmış.

Son olarak, üçüncü satır örneği kullanmak istediğiniz ASP `WebGrid` Yardımcısı. Oluşturduğunuz (*örneği*) kullanarak yardımcı nesnesi `new` anahtar sözcüğü ve yoluyla sorgu sonuçları geçirin `selectedData` değişkeni. Yeni `WebGrid` nesnesi, veritabanı sorgusu sonuçlarını birlikte kullanılabilir hale getirilir içinde `grid` değişkeni. Verileri gerçekte sayfasında görüntülenecek bir dakika içinde sonucunda ortaya çıkan gerekir.

Bu aşamada, veritabanı açıldı, verileri kabulünüzü istediğiniz ve hazır `WebGrid` veri Yardımcısı. Sonraki biçimlendirme sayfasında oluşturmaktır.

> [!TIP] 
> 
> **Yapılandırılmış sorgu dili (SQL)**
> 
> SQL, çoğu ilişkisel veritabanları veritabanındaki verileri yönetmek için kullanılan bir dildir. Verileri almak ve güncelleştirmek sağlar ve oluşturma, değiştirme ve veritabanı tablolarındaki verileri yönetmenize imkan sağlayan komutlar içerir. SQL, bir programlama dili (ör. C# ' ta) farklıdır. SQL, istediğiniz veritabanını söyleyin ve veri almak veya görevi gerçekleştirmek nasıl ekleneceğini veritabanının iş olması. Bazı SQL komutlarını örnekleri ve ne yaptıklarını şunlardır:
> 
> `Select * From Movies`
> 
> `SELECT ID, Name, Price FROM Product WHERE Price > 10.00 ORDER BY Name`
> 
> İlk `Select` deyimi tüm sütunları alır (tarafından belirtilen `*`) gelen *filmler* tablo.
> 
> İkinci `Select` deyimi kayıtlarındaki Kimliğini, adını ve fiyat sütunları getirir *ürün* fiyat sütun değeri birden fazla 10 olan tablo. Komut adı sütun değerlerine göre alfabetik sırada sonuçları döndürür. Fiyat ölçütüyle eşleşen kayıt yok, komut boş döndürür.
> 
> `INSERT INTO Product (Name, Description, Price) VALUES ('Croissant', 'A flaky delight', 1.99)`
> 
> Bu komut yeni bir kayıt ekler *ürün* tablosu, Ad sütununda "Croissant", "A anormal zevk aldığı" açıklama sütuna ve fiyat 1.99 için ayarlama.
> 
> Sayısal olmayan değer belirtirken değeri tek tırnak işareti (çift tırnak işaretleri değil, olduğu gibi C#) arasına alınıp dikkat edin. Bu metin veya tarih değerlerini geçici ancak olmayan sayılar etrafında tırnak işaretleri kullanın.
> 
> `DELETE FROM Product WHERE ExpirationDate < '01/01/2008'`
> 
> Bu komut, kayıtları siler *ürün* , sona erme tarihi sütun 1 Ocak 2008'den önceki tablo. (Komut varsayar *ürün* tablolu böyle bir sütunu Elbette.) Tarihi GG/AA/YYYY biçiminde buraya girilen ancak bölgeniz için kullanılan biçimde girilmelidir.
> 
> `Insert` Ve `Delete` komutları sonuç kümeleri dönüş yok. Bunun yerine, bunlar kaç kayıt komutu tarafından etkilendiğini belirten bir sayı döndürür.
> 
> Bu işlemler (örneğin, ekleme ve kayıtları silme) bazıları için veritabanında uygun izinlere sahip olması işlemi isteyen işlem vardır. İşte bu nedenle üretim veritabanları genellikle veritabanına bağlandığında, bir kullanıcı adını ve parolasını sağlamanız gerekir.
> 
> SQL komutlarını düzinelerce vardır, ancak bunların tümü burada gördüğünüz komutları gibi bir yol izler. Veritabanı tabloları oluşturmak, bir tablodaki kayıtların sayısı, fiyatlarını hesaplamak ve çok daha fazla işlemleri gerçekleştirmek için SQL komutlarını kullanabilirsiniz.


### <a name="adding-markup-to-display-the-data"></a>Verileri görüntülemek için biçimlendirme ekleme

İçinde `<head>` öğesi, kümesi içeriğini `<title>` "Filmler" öğesine:

[!code-html[Main](displaying-data/samples/sample2.html?highlight=3)]

İçinde `<body>` öğe sayfasının aşağıdakileri ekleyin:

[!code-html[Main](displaying-data/samples/sample3.html)]

İşte bu kadar. `grid` Değişkenidir oluştururken oluşturduğunuz değeri `WebGrid` önceki kod nesne.

WebMatrix ağaç görünümünde, sayfanın sağ tıklatıp **başlatma tarayıcıda**. Bu sayfa şöyle görürsünüz:

![Varsayılan WebGrid Yardımcısı filmler tablo çıktısı](displaying-data/_static/image18.png)

Sütuna göre sıralamak için sütun başlığını bağlantısına tıklayın. Bir başlığını tıklatarak sıralama yapamamasına içinde yerleşik bir özelliktir **WebGrid** Yardımcısı.

`GetHtml` Yöntemi, adından da anlaşılacağı gibi verileri görüntüler biçimlendirme oluşturur. Varsayılan olarak, `GetHtml` yöntem oluşturur HTML `<table>` öğesi. (İsterseniz, işleme sayfasını tarayıcıda kaynak bakarak doğrulayabilirsiniz.)

## <a name="modifying-the-look-of-the-grid"></a>Kılavuz görünümünü değiştirme

Kullanarak `WebGrid` yalnızca yaptığınız gibi yardımcı kolaydır, ancak elde edilen görüntü düz. `WebGrid` Yardımcı verinin nasıl görüntüleneceğini denetlemenize olanak sağlayan, her türlü seçenekleri vardır. Vardır Bu öğreticide keşfetmek için çok daha fazla, ancak bu bölümde bu seçenekler bazıları hakkında fikir verir. Bu serideki sonraki öğreticileri içinde birkaç ek seçenekler ele alınacaktır.

### <a name="specifying-individual-columns-to-display"></a>Görüntülenecek sütunları tek tek belirtme

Başlatmak için yalnızca belirli sütunları görüntülemek istediğinizi belirtebilirsiniz. Gördüğünüz gibi varsayılan olarak, ızgara sütunları dört gösterir. *filmler* tablo.

İçinde *Movies.cshtml* dosya, yerine `@grid.GetHtml()` şununla eklediğiniz biçimlendirme:

[!code-css[Main](displaying-data/samples/sample4.css)]

Hangi sütunların görüntüleneceğini yardımcı bildirmek için eklediğiniz bir `columns` parametresi için `GetHtml` yöntemi ve sütun koleksiyonu geçirin. Koleksiyondaki her sütun eklemek için belirtin. Ekleyerek görüntülemek için tek bir sütun belirttiğiniz bir `grid.Column` nesne ve istediğiniz veri sütununun adını geçirin. (Bu sütunları SQL sorgu sonuçlarında eklenmelidir — `WebGrid` Yardımcısı, sorgu tarafından döndürülen olmayan sütunları görüntüleyemiyor.)

Başlatma *Movies.cshtml* sayfasını tarayıcıda yeniden ve aşağıdakine benzer bir görünüm elde bu saati (hiç kimlik sütunu görüntülendiğini fark):

![Yalnızca seçili sütunların gösteren WebGrid görüntüleme](displaying-data/_static/image19.png)

### <a name="changing-the-look-of-the-grid"></a>Kılavuz görünümünü değiştirme

Bazıları, bu kümesindeki sonraki öğreticileri, incelenecek sütunlarını görüntüleme için oldukça birkaç daha fazla seçenek vardır. Şu an için bu bölümde, bir bütün olarak kılavuz stil yollarını tanıtılacaktır.

İçinde `<head>` yalnızca kapatmadan önce sayfasının bölümünde `</head>` etiketi, aşağıdakileri ekleyin `<style>` öğe:

[!code-css[Main](displaying-data/samples/sample5.css)]

Bu CSS biçimlendirmesi adlı sınıflarını tanımlayan `grid`, `head`ve benzeri. Bu stil tanımları ayrı bir koyabilirsiniz *.css* dosya ve sayfaya bağlantı. (Aslında, bu daha sonra Bu öğretici kümesinde gerçekleştirirsiniz.) Ama Bu öğretici için işleri kolaylaştırmak için aynı sayfanın içinde verileri görüntüler.

Alabileceğiniz artık `WebGrid` Yardımcısı bu stil sınıflarını kullanın. Yardımcı bir dizi özelliği vardır (örneğin, `tableStyle`) yalnızca bu amaç için — bir CSS stil sınıf adı atanıncaya ve bu sınıf adı Yardımcısı tarafından işlenen biçimlendirme bir parçası olarak işlenir.

Değişiklik `grid.GetHtml` onun artık bu kodu gibi görünür biçimlendirme:

[!code-css[Main](displaying-data/samples/sample6.css)]

Burada yenilikler, eklediğiniz olan `tableStyle`, `headerStyle`, ve `alternatingRowStyle` parametreleri `GetHtml` yöntemi. Bu parametreler biraz önce eklenen CSS stilleri adları için ayarlanmış.

Sayfayı çalıştırın ve bu süreden önce değerinden daha az düz benzeyen bir kılavuz bakın:

![CSS sınıf adları için parametreleri içeren WebGrid görüntü ayarlama](displaying-data/_static/image20.png)

Görmek için `GetHtml` oluşturulan yöntemi göz önünde bulundurmanız sayfasını tarayıcıda kaynaktaki. Burada ayrıntıya gitmek çalışmaz, ancak en önemli nokta, gibi parametreleri belirterek olan `tableStyle`, aşağıdaki gibi HTML etiketleri oluşturmak kılavuz neden:

`<table class="grid">`

`<table>` Etiketi olan bir `class` kendisine eklenmiş özniteliği, daha önce eklediğiniz CSS kurallardan biri başvuruyor. Bu kod, temel düzeni gösterilir &mdash; için farklı parametreler `GetHtml` yöntemi izin verin, başvuru CSS sınıfları yöntem sonra birlikte biçimlendirme oluşturur. CSS sınıflarının ne size bağlıdır.

## <a name="adding-paging"></a>Disk belleği ekleme

Bu öğretici için son görev olarak kılavuza disk belleği ekleyeceksiniz. Şimdi, filmler aynı anda görüntülenecek herhangi bir sorun sağ. Ancak filmler yüzlerce eklediyseniz, sayfa görüntüsü uzun alırlar.

Sayfa kodunda oluşturur satırı değiştirin `WebGrid` aşağıdaki kodu nesnesine:

[!code-csharp[Main](displaying-data/samples/sample7.cs)]

Tek farkı önce olduğundan, eklediğiniz bir `rowsPerPage` 3 ayarlanmış parametresi.

Sayfayı çalıştırın. Kılavuz 3 satır saati artı veritabanınızdaki filmler sayfadan izin Gezinti bağlantıları görüntüler:

![Disk belleği ile WebGrid görüntüleme](displaying-data/_static/image21.png)

## <a name="coming-up-next"></a>Sıradaki gelen

Sonraki öğreticide Razor ve C# kod içinde bir formun kullanıcı girişi almak için nasıl kullanılacağını öğreneceksiniz. Başlığı veya türe göre filmler bulabilmesi için bir arama kutusu filmler sayfasına ekleyeceksiniz.

## <a name="complete-listing-for-movies-page"></a>Tam listesi için filmler sayfası

[!code-cshtml[Main](displaying-data/samples/sample8.cshtml)]

## <a name="additional-resources"></a>Ek Kaynaklar

- [Razor sözdizimini kullanan ASP.NET Web programlamaya giriş](https://go.microsoft.com/fwlink/?LinkID=202890)

> [!div class="step-by-step"]
> [Önceki](intro-to-web-pages-programming.md)
> [sonraki](form-basics.md)
