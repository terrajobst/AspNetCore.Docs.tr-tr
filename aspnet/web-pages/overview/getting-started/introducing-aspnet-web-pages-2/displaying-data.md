---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data
title: Verileri görüntüleme - ASP.NET Web sayfaları ile tanışın | Microsoft Docs
author: tfitzmac
description: Bu öğretici Webmatrix'te bir veritabanı oluşturulacağını ve ASP.NET Web sayfaları (Razor) kullanırken veritabanı verilerinin bir sayfada nasıl görüntüleneceğini gösterir. Bu, y varsayar...
ms.author: aspnetcontent
ms.date: 05/28/2015
ms.assetid: b3a006a0-3ea2-4d45-b833-e20e3a3c0a1a
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data
msc.type: authoredcontent
ms.openlocfilehash: eeceb08e3aa281c45a2cfe35af4f23b76a5b1d25
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37834303"
---
<a name="introducing-aspnet-web-pages---displaying-data"></a>ASP.NET Web sayfalarına giriş - verileri görüntüleme
====================
tarafından [Tom FitzMacken](https://github.com/tfitzmac)

> Bu öğretici Webmatrix'te bir veritabanı oluşturulacağını ve ASP.NET Web sayfaları (Razor) kullanırken veritabanı verilerinin bir sayfada nasıl görüntüleneceğini gösterir. Bu seriyi aracılığıyla bitirdiğinizi [ASP.NET Web sayfaları programlama giriş](../introducing-razor-syntax-c.md).
> 
> Öğrenecekleriniz:
> 
> - Bir veritabanını ve veritabanı tabloları oluşturmak için WebMatrix araçları kullanma
> - Verileri bir veritabanına eklemek için WebMatrix araçları kullanma
> - Nasıl bir sayfada veritabanından veri görüntüleme.
> - ASP.NET Web sayfaları'nda SQL komutları çalıştırmayı öğrenin.
> - Nasıl özelleştireceğinizi `WebGrid` Yardımcısı sayfalama ve sıralama eklenecek ve veri görünümünü değiştirmek için.
>   
> 
> Ele alınan özelliklerin/teknolojiler:
> 
> - WebMatrix veritabanı araçları.
> - `WebGrid` Yardımcısı.


## <a name="what-youll-build"></a>Ne oluşturacaksınız

Önceki öğreticide, ASP.NET Web sayfaları için sunulan (*.cshtml* dosyaları), Razor sözdizimi ile ilgili temel bilgileri ve Yardımcıları. Bu öğreticide, serinin geri kalanı için kullanacağınız gerçek bir web uygulaması oluşturma başlarsınız. Uygulamayı görüntüleme, ekleme, değiştirme ve filmlerle ilgili bilgiler silme olanak sağlayan bir basit film uygulamasıdır.

Bu öğretici ile işiniz bittiğinde bu sayfanız gibi film listesini görüntülemek mümkün olacaktır:

![CSS sınıf adları için parametreleri içeren WebGrid görüntü ayarlama](displaying-data/_static/image1.png)

Ancak, başlamak için bir veritabanı oluşturmanız gerekir.

## <a name="a-very-brief-introduction-to-databases"></a>Veritabanları için çok kısa bir giriş

Bu öğreticide yalnızca veritabanlarını briefest giriş sağlar. Veritabanı deneyimi varsa, kısa bu bölümü atlayabilirsiniz.

Bir veritabanı bilgilerini içeren bir veya daha fazla tablo içeren &mdash; Örneğin, müşteri, sipariş ve satıcılar veya Öğrenciler ve Öğretmenler, sınıfları ve notlarınızı tablolar. Yapısal olarak, bir veritabanı gibi bir elektronik tablodur. Bir normal adres defteri düşünün. Kullanıcının adres defterinde her giriş için (diğer bir deyişle, her kişi için) birkaç bölümü ele alınmakta ad, Soyadı, adresini, e-posta adresi ve telefon numarası gibi bilgileri vardır.

![Basit bir kılavuz olarak örnek veritabanı tablosu](displaying-data/_static/image2.png)

(Satır bazen denir *kayıtları*, ve sütunları bazen denir *alanları*.)

Çoğu veritabanı tabloları için tablo müşteri numarası, hesap numarası ve benzeri gibi benzersiz bir değer içeren bir sütun olması gerekir. Bu değer, tablo bilinen *birincil anahtar*, ve tablodaki her satır tanımlamak için kullanın. Örnekte, önceki örnekte gösterilen adres defteri için birincil anahtarı kimliği sütundur.

Web uygulamalarında yaptığınız işin çoğunu veritabanından bilgileri okumak ve bir sayfa görüntüleme oluşur. Ayrıca sıklıkla kullanıcılardan bilgi toplamak ve bir veritabanına ekleme veya veritabanında zaten var olan kayıtların değiştirilmesi. (Bu öğretici kümesi sırasında bu işlemlerin tümü ele.)

Veritabanı iş artık çok karmaşık olabilir ve özel bilgi gerekli kılabilirsiniz. Bu öğretici kümesi için yine de, kullandıkça, tüm açıklanacaktır yalnızca temel kavramları anlamanız gerekir.

## <a name="creating-a-database"></a>Veritabanı oluşturma

WebMatrix, bir veritabanı oluşturmak ve veritabanında tablolar oluşturmak için kolaylaştıran araçlar içerir. (Bir veritabanının yapısını veritabanı adlandırılır *şema*.) Bu öğretici kümesi için yalnızca bir tablo içerdiğinden bir veritabanı oluşturacaksınız &mdash; filmler.

Zaten yapmadıysanız, WebMatrix açın ve önceki öğreticide oluşturduğunuz WebPagesMovies sitesini açın.

Sol bölmede **veritabanı** çalışma.

![WebMatrix veritabanı çalışma alanı sekmesi](displaying-data/_static/image3.png)

Veritabanı ile ilgili görevleri göstermek için Şerit değişir. Şeritte tıklayın **yeni veritabanı**.

![WebMatrix Şeritte 'Yeni Veritabanı' düğmesi](displaying-data/_static/image4.png)

WebMatrix, SQL Server CE veritabanı oluşturur (bir *.sdf* dosya) siteniz olarak aynı ada sahip &mdash; *WebPagesMovies.sdf*. (Bu burada olmaz, ancak sahip olduğu sürece istediğiniz dosyası istediğiniz bir şey adlandırabilirsiniz bir *.sdf* uzantısı.)

## <a name="creating-a-table"></a>Tablo oluşturma

Şeritte tıklayın **yeni tablo**. Tablo Tasarımcısı, WebMatrix yeni bir sekmede açılır. (Yeni tablo seçenek kullanılabilir değilse, yeni veritabanı soldaki ağaç görünümünde seçili olduğundan emin olun.)

![WebMatrix Şeritte 'Yeni tablo' düğmesi](displaying-data/_static/image5.png)

"Film" (burada Filigran "Tablo adı girin" ifadesini içeren) üst metin kutusuna girin.

![WebMatrix Veritabanı Tasarımcısı'nda bir tablo adı girerek](displaying-data/_static/image6.png)

Tablo adı altında tek tek sütunlara tanımladığınız bölmesidir. İçin *filmler* tablo Bu öğreticide, yalnızca birkaç sütunu oluşturacaksınız: *kimliği*, *başlık*, *Tarz*, ve *Yıl*.

İçinde **adı** kutusunda, "ID" girin. Burada bir değer girerek yeni bir sütun için tüm denetimleri etkinleştirir.

İçin sekmesinde **veri türü** listesindeki **int**. Bu değer, kimlik sütunu tamsayı (sayı) verileri içerdiğini belirtir.

> [!NOTE]
> Tüm burada daha fazla (çok) adlandırılır olmaz, ancak bu kılavuzda gitmek için standart Windows klavye hareketlerini kullanın. Örneğin, alanlar arasında sekme, yalnızca, listedeki bir öğe seçmek için yazmaya başlayın ve benzeri.


Geçmiş sekmesinde **varsayılan değer** kutusunu (yani, boş bırakın). İçin sekmesinde **olan birincil anahtar** onay kutusunu işaretleyin ve bu seçeneği belirleyin. Bu seçenek, veritabanını söyler *kimliği* sütun ayrı satırlara tanımlayan veriler içerir. (Diğer bir deyişle, her satırda benzersiz bir değer satır bulmak için kullanabileceğiniz Kimlik sütununda gerekir.)

Seçin **olan kimlik** seçeneği. Bu seçenek, veritabanı, otomatik olarak her yeni satırda sonraki sıralı sayıyı oluşturmasını söyler. ( **Olan kimlik** çalışır, ayrıca yalnızca seçtiğiniz durumunda seçeneği **olan birincil anahtar** seçeneği.)

Sonraki kılavuz satırda tıklayın veya iki kez geçerli satır tamamlamak için SEKME tuşuna basın. Ya da hareket geçerli satırı kaydeder ve bir sonraki başlatır. Dikkat **varsayılan değer** artık sütun yazan **Null**. (Varsayılan değer için varsayılan değer speak kadar çok NULL'dur.)

Bittiğinde yeni tanımlama **kimliği** sütun, Tasarımcı Bu çizim gibi görünür:

![WebMatrix Veritabanı Tasarımcısı sonra filmler tablosunun kimlik sütunu tanımlama](displaying-data/_static/image7.png)

Bir sonraki sütun oluşturmak için kutuya tıklayın **adı** sütun. Sütun için "Title" girin ve ardından **nvarchar** için **veri türü** değeri. "Var" bölümünü **nvarchar** veritabanı bu sütun için verilerin boyutu kayıt kaydı gösterebilir bir dize olmasını söyler. (Alan bir harf veya yazı sistemi için karakter verileri tutabilir gösteren "n" ön eki "ulusal" temsil eder; diğer bir deyişle, alanın Unicode verileri tutar.)

Seçeneğini belirlediğinizde **nvarchar**, alan için en fazla uzunluk girebileceğiniz başka bir kutusu görüntülenir. Bu öğreticide ile çalışacaksınız hiçbir film adı 50 karakterden daha uzun olacağını varsayımıyla, 50 girin.

Skip **varsayılan değer** temizleyin **null değerlere izin ver** seçeneği. Bir başlık olmayan veritabanına girilir tüm filmlere izin veritabanına istemezsiniz.

İşiniz bittiğinde ve sonraki satıra taşıma, Tasarımcı Bu çizim gibi görünür:

![WebMatrix Veritabanı Tasarımcısı sonra filmler tablonun başlık sütunu tanımlama](displaying-data/_static/image8.png)

Yalnızca 30 ayarlayın uzunluğu dışında "Tarzı" adlı bir sütun oluşturmak için aşağıdaki adımları yineleyin.

"Year" adlı başka bir sütun oluşturun. Veri türü için seçin **nchar** (değil **nvarchar**) ve uzunluğu 4'e ayarlayın. Değişken boyutlu bir sütun gerektirmeyen bu nedenle bir yıl için 4 basamaklı bir sayı "1995" veya "2010" gibi kullanın dağıtacağız.

Tamamlanmış tasarım benzer aşağıda verilmiştir:

![Tüm alanlar için filmler tablo tanımlandıktan sonra WebMatrix Veritabanı Tasarımcısı](displaying-data/_static/image9.png)

CTRL + S tuşuna basın veya tıklayın **Kaydet** hızlı erişim araç çubuğu düğmesi. Veritabanı Tasarımcısı sekmeyi kapatarak kapatın.

## <a name="adding-some-example-data"></a>Bazı örnek verileri ekleme

Bu öğretici serisinde daha sonra bir biçimde yeni filmler girebileceğiniz bir sayfa oluşturacaksınız. Şimdilik, ancak bir sayfada daha sonra görüntüleyebileceğiniz bazı örnek veriler ekleyebilirsiniz.

İçinde **veritabanı** çalışma webmatrix'te gösteren bir ağaç var olduğuna dikkat edin *.sdf* daha önce oluşturduğunuz dosya. Düğüm, yeni açmak *.sdf* dosyasını ve ardından açın **tabloları** düğümü.

![WebMatrix veritabanı çalışma alanı ile ağaç filmler tabloya Aç](displaying-data/_static/image10.png)

Sağ **filmler** düğümünü seçip **veri**. WebMatrix için veri girebildiğiniz kılavuz açılır *filmler* tablosu:

![Veritabanı girişi kılavuzunda WebMatrix (boş)](displaying-data/_static/image11.png)

Tıklayın **başlık** sütun ve "Olduğunda Harry karşılanıyor Sally" girin. Taşı **Tarz** sütun (Tab tuşunu kullanabilirsiniz) ve "Romantik Komedi" girin. Taşı **yıl** sütun ve "1989" girin:

![Bir kayıt webmatrix'te veritabanı girişi kılavuz](displaying-data/_static/image12.png)

Enter tuşuna basın ve WebMatrix yeni filmden kaydeder. Dikkat **kimliği** sütunu doldurulur.

![Webmatrix'te veritabanı girişi kılavuz bir kayıt ve otomatik olarak oluşturulan kimliği](displaying-data/_static/image13.png)

Başka bir film (örneğin, "gitti ile Rüzgar", "DRAM", "1939") girin. Kimlik sütunu yeniden doldurulur:

![Webmatrix'te iki kaydı ve otomatik olarak oluşturulan kimlikleri içeren veritabanı girişi kılavuz](displaying-data/_static/image14.png)

Üçüncü bir filmi (örneğin, "Ghostbusters", "Komedi") girin. Bir deney bırakın **yıl** sütun boş bırakın ve ardından Enter tuşuna basın. Seçili olduğundan **null değerlere izin ver** seçeneği, veritabanı bir hatayı gösterir:

![Gerekli sütun değeri boş bırakılması durumunda 'Geçersiz veri' bir hata görüntülenmiyor.](displaying-data/_static/image15.png)

Tıklayın **Tamam** dönün ("Ghostbusters" yılı 1984 içindir) girişi düzeltin ve Enter tuşuna basın.

Birkaç filmleri 8 bulunana kadar veya şekilde doldurun. (8 girdikten sonra disk belleği ile çalışmak kolaylaştırır. Ancak, çok fazla ise, yalnızca birkaç şimdilik girin.) Gerçek veriler önemli değildir.

![Webmatrix'te ya da kayıtları içeren veritabanı girişi kılavuz](displaying-data/_static/image16.png)

Tüm filmlere hatasız girdiğiniz kimliği değerleri sıralı olarak kullanılır. Tamamlanmamış film kaydı kaydetmeye çalıştığında kimlik numaraları sıralı olmayabilir. Bu durumda, bu sorun değildir. Sayıları herhangi belirli anlamlara sahip değilseniz ve önemli olan tek şey içinde benzersiz olan *filmler* tablo.

Veritabanı Tasarımcısı içeren sekmeyi kapatın.

Artık bir web sayfasında bu verileri görüntülemeye kapatabilirsiniz.

## <a name="displaying-data-in-a-page-by-using-the-webgrid-helper"></a>WebGrid Yardımcısını kullanarak verileri bir sayfa görüntüleme

Verileri bir sayfada görüntülemek için kullanılacak gideceğinizi `WebGrid` Yardımcısı. Bu yardımcı kılavuz ya da tablo (satırları ve sütunları) bir görüntü oluşturur. Gördüğünüz gibi mümkün İyileştir kılavuz biçimlendirme ve diğer özelliklere sahip olacaksınız.

Kılavuz çalıştırmak için birkaç satır kod yazmak zorunda kalırsınız. Bu birkaç satır düzeni neredeyse tüm Bu öğreticide bunu veri erişimi için bir tür olarak hizmet verecektir.

> [!NOTE]
> Aslında, verileri bir sayfada görüntülemek için birçok seçeneğiniz de vardır; `WebGrid` Yardımcısı yalnızca biridir. Bu öğreticide, verileri görüntülemek için en kolay yolu olduğundan ve makul ölçüde esnek olduğundan seçtik. Sonraki öğretici kümesinde verileri görüntülemek nasıl daha doğrudan denetime size sayfanın verilerle çalışmak için daha fazla bir "elle" gibi kullanabileceğiniz öğreneceksiniz.


Webmatrix'te sol bölmesinde **dosyaları** çalışma.

Oluşturduğunuz yeni veritabanı *uygulama\_veri* klasör. Klasör zaten yoksa, WebMatrix yeni veritabanınız için oluşturuldu. (Yardımcıları daha önce yüklediyseniz klasör var.)

Ağaç görünümünde, Web sitesinin kök seçin. Web sitesinin kök seçmeniz gerekir; Aksi takdirde, uygulamaya yeni bir dosya eklenebilir\_veri klasörü.

Şeritte tıklayın **yeni**. İçinde **bir dosya türünü seçin** kutusunda **CSHTML**.

İçinde **adı** kutusunda, yeni sayfayı "Movies.cshtml" olarak adlandırın:

!['Filmler' sayfasını gösteren 'Bir dosya türünü seçin' iletişim kutusu](displaying-data/_static/image17.png)

Tıklayın **Tamam** düğmesi. WebMatrix, çatı bazı öğelere sahip yeni bir dosya açar. Öncelikle, veritabanından veri almak için kod yazmanız. Ardından, gerçekten verileri görüntülemek için sayfanın biçimlendirme ekleyeceksiniz.

### <a name="writing-the-data-query-code"></a>Verileri sorgu kod yazma

Sayfanın üst kısmındaki arasında `@{` ve `}` karakter aşağıdaki kodu girin. (Açılış ve kapanış küme ayraçları arasına Bu kod girdiğinizden emin olun.)

[!code-csharp[Main](displaying-data/samples/sample1.cs)]

İlk satır, daha önce oluşturduğunuz veritabanı, veritabanı ile bir şey yapmadan önce her zaman ilk adım olan açılır. Size `Database.Open` açmak için veritabanının yöntemi adı. Eklemezseniz bildirimi *.sdf* adı. `Open` Yöntemi aranırken olduğunu varsayar bir *.sdf* dosyası (diğer bir deyişle, *WebPagesMovies.sdf*) ve *.sdf* dosyası *Uygulama\_ Veri* klasör. (Biz, daha önce Not *uygulama\_veri* klasör ayrılmıştır; bu senaryo burada ASP.NET yapar adı hakkındaki varsayımların yerlerden biri.)

Veritabanı açıldığında, ona bir başvuru adlı değişken yerleştirilir `db`. (Olan her şeyi adlandırılabilir.) `db` Değişkendir veritabanıyla etkileşim kurma nasıl elde edersiniz.

Veritabanı verilerini gerçekten getirir kullanarak ikinci satır `Query` yöntemi. Bu kodu nasıl çalıştığını dikkat edin: `db` değişken açılan veritabanına bir başvuru içeriyor ve çağırmayı `Query` yöntemi kullanarak `db` değişkeni (`db.Query`).

Bir SQL sorgusu olduğundan `Select` deyimi. (Açıklama daha sonra SQL hakkında biraz arka plan için bakın.) Deyiminde `Movies` sorgu tabloya tanımlar. `*` Karakter tablodan tüm sütunları sorgu döndürmesi gerektiğini belirtir. (Size de sütunları tek tek virgülle ayırarak listeleyebilirsiniz.)

Sorgunun sonuçlarını varsa, döndürülen ve içinde kullanılabilir hale `selectedData` değişkeni. Yeniden değişkeni hiçbir şey adlandırılmış.

Son olarak, üçüncü satır, kullanmak istediğiniz ASP `WebGrid` Yardımcısı. Oluşturduğunuz (*örneği*) kullanarak yardımcı nesnesi `new` anahtar sözcüğü ve sorgu sonuçları aracılığıyla geçirin `selectedData` değişkeni. Yeni `WebGrid` nesne, veritabanı sorgusunun sonuçları ile birlikte kullanılabilir yapılan `grid` değişkeni. Sonucu, gerçekten verileri sayfasında görüntülemek için bir dakika içinde gerekir.

Bu aşamada, veritabanı açılır, verileri yönettiniz istediğiniz ve hazırladığınız `WebGrid` verilerle Yardımcısı. Sonraki işaretleme sayfasında oluşturmaktır.

> [!TIP] 
> 
> **Yapılandırılmış sorgu dili (SQL)**
> 
> SQL veritabanındaki verileri yönetmek için kullanılan çoğu ilişkisel veritabanı içinde bir dildir. Bu verileri almak ve güncelleştirmek izin veren ve oluşturmak, değiştirmek ve veritabanı tablolarındaki verileri yönetmek, sağlayan komutları içerir. SQL, bir programlama dili (gibi C# ' ta) farklıdır. SQL veritabanı istediklerinizi söyleyin ve nasıl veri elde etmek veya görevi gerçekleştirmek için veritabanının iş olduğu. Bazı SQL komutlarını örnekleri ve ne yaptıklarını şunlardır:
> 
> `Select * From Movies`
> 
> `SELECT ID, Name, Price FROM Product WHERE Price > 10.00 ORDER BY Name`
> 
> İlk `Select` deyimi tüm sütunları alır (tarafından belirtilen `*`) öğesinden *filmler* tablo.
> 
> İkinci `Select` deyimi kayıtlarındaki kimliği, ad ve fiyat sütunları getirir *ürün* fiyat sütun değeri 10'dan fazla olan tablo. Komut, Ad sütununda değerlerine göre alfabetik sırada sonuçları döndürür. Fiyat ölçütüyle eşleşen kayıt yok, komut boş döndürür.
> 
> `INSERT INTO Product (Name, Description, Price) VALUES ('Croissant', 'A flaky delight', 1.99)`
> 
> Bu komut yeni bir kayıt ekler *ürün* tablosu, Ad sütununda "Croissant", "A güvenilir olmayan beğeni" için açıklama sütununun ve fiyat için 1.99 ayarlama.
> 
> Sayısal olmayan değer belirlediniz, değeri tek tırnak işareti (çift tırnak işaretleri değil, C# gibi) içine dikkat edin. Bu metin veya tarih değerleri etrafında ancak değil numaralarını etrafında tırnak işareti kullanın.
> 
> `DELETE FROM Product WHERE ExpirationDate < '01/01/2008'`
> 
> Bu komut, kayıtları siler *ürün* sona erme tarihi sütununu 1 Ocak 2008'den önceki bir tablo. (Komut *ürün* tablolu böyle bir sütunu tabii.) Tarih GG/AA/YYYY biçiminde buraya girilir ancak bölgeniz için kullanılan biçiminde girilmelidir.
> 
> `Insert` Ve `Delete` komutları yoksa sonuç kümesi döndürür. Bunun yerine, bunlar komutu tarafından kaç tane kaydın etkilendiğini belirten bir sayı döndürür.
> 
> Veritabanında uygun izinlere sahip bazıları bu işlemleri (örneğin, ekleme ve kayıt silme) için işlem isteme işlemi gerekir. İşte bu nedenle üretim veritabanları genellikle veritabanına bağlanırken bir kullanıcı adı ve parola sağlamanız gerekir.
> 
> SQL komutları onlarca vardır, ancak hepsi Burada gördüğünüz komutları gibi deseni izler. Veritabanı tabloları oluşturmak, bir tablodaki kayıtların sayısını, fiyatları hesaplayın ve birçok diğer işlemleri gerçekleştirmek için SQL komutlarını kullanabilirsiniz.


### <a name="adding-markup-to-display-the-data"></a>Verileri görüntülemek için biçimlendirme ekleme

İçinde `<head>` öğesi kümesi içeriğini `<title>` "Filmler" öğesine:

[!code-html[Main](displaying-data/samples/sample2.html?highlight=3)]

İçinde `<body>` sayfasının öğesi ekleyin:

[!code-html[Main](displaying-data/samples/sample3.html)]

İşte bu kadar. `grid` Oluşturduğunuz zaman, oluşturduğunuz değeri değişkendir `WebGrid` kod daha önce nesneye.

WebMatrix ağaç görünümünde, sayfanın sağ tıklayıp **tarayıcıda Başlat**. Bu sayfa şöyle görürsünüz:

![Varsayılan WebGrid Yardımcısı filmler tablo çıktısı](displaying-data/_static/image18.png)

Sütuna göre sıralamak için sütun başlığını bağlantısına tıklayın. Bir başlığa tıklayarak sıralayın işaretleyebilmesine yerleşik olan bir özelliktir **WebGrid** Yardımcısı.

`GetHtml` Yöntem adından da anlaşılacağı gibi verileri görüntüleyen biçimlendirme oluşturur. Varsayılan olarak, `GetHtml` yöntemi oluşturur bir HTML `<table>` öğesi. (İsterseniz, işleme sayfasını tarayıcıda kaynağını bakarak doğrulayabilirsiniz.)

## <a name="modifying-the-look-of-the-grid"></a>Kılavuz görünümünü değiştirme

Kullanarak `WebGrid` yalnızca yaptığınız gibi yardımcı kolaydır, ancak sonuçta elde edilen görüntü düz. `WebGrid` Yardımcı olan seçenekleri her türlü verinin nasıl görüntüleneceğini denetlemenize olanak sağlayan. Bu öğreticide keşfetmek için çok kullanabileceğiniz birçok, ancak bu bölümde bu seçenekler bir kısmının fikir verecektir. Bu serideki sonraki öğreticilerde, bazı ek seçenekler ele alınacaktır.

### <a name="specifying-individual-columns-to-display"></a>Görüntülenecek bireysel sütunları belirtme

Başlamak için yalnızca belirli sütunları görüntülemek istediğinizi belirtebilirsiniz. Gördüğünüz gibi varsayılan olarak, kılavuz dört sütun gösterir. *filmler* tablo.

İçinde *Movies.cshtml* dosyası, değiştirin `@grid.GetHtml()` aşağıdakilerle eklediğiniz biçimlendirme:

[!code-css[Main](displaying-data/samples/sample4.css)]

Hangi sütunların görüntüleneceğini yardımcı bildirmek için dahil bir `columns` parametresi için `GetHtml` yöntemi ve sütunlar koleksiyonu geçirin. Koleksiyondaki her bir sütun eklemek için belirtin. Dahil ederek görüntülemek için bir bireysel sütunda belirttiğiniz bir `grid.Column` nesne ve istediğiniz veri sütununun adını geçirin. (Bu sütunların SQL sorgu sonuçlarını eklenmelidir — `WebGrid` Yardımcısı, sorgu tarafından döndürülen olmayan sütunları görüntüleyemiyor.)

Başlatma *Movies.cshtml* sayfasını tarayıcıda yeniden ve aşağıdakine benzer bir ekran bu alışınızda (kimlik sütunu yok görüntülendiğini dikkat edin):

![Yalnızca seçili sütunların gösteren WebGrid görüntüleme](displaying-data/_static/image19.png)

### <a name="changing-the-look-of-the-grid"></a>Kılavuz görünümünü değiştirme

Bazıları, sonraki öğreticilerde bu kümeye incelenecek sütunları görüntülemek için çok sayıda videonuz daha fazla seçenek vardır. Şimdilik, bu bölümde, bir bütün olarak kılavuz stili yollarını başlatacaktır.

İçinde `<head>` kapatmadan önce yalnızca sayfanın bölümüne `</head>` etiketi, aşağıdaki `<style>` öğesi:

[!code-css[Main](displaying-data/samples/sample5.css)]

Sınıflar adlandırılan bu CSS biçimlendirmesi tanımlar `grid`, `head`ve benzeri. Bu stil tanımları ayrı bir yerleştirebilirsiniz *.css* dosya ve bu sayfaya bağlantı. (Aslında, bu daha sonra Bu öğretici kümesinde gerçekleştirirsiniz.) Ancak bu öğretici için şeyler kolaylaştırmak için aynı sayfanın içinde verileri görüntüler.

Alabileceğiniz artık `WebGrid` Yardımcısı bu stil sınıflarını kullanın. Yardımcı bir dizi özelliği vardır (örneğin, `tableStyle`) bu amaç için — bir CSS stil sınıf adı atamanız ve bu sınıf adı Yardımcısı tarafından işlenen biçimlendirme bir parçası olarak işlenir.

Değişiklik `grid.GetHtml` biçimlendirme BT'nin artık şu kod gibi görünür:

[!code-css[Main](displaying-data/samples/sample6.css)]

Eklediğiniz yenilikler aşağıda olan `tableStyle`, `headerStyle`, ve `alternatingRowStyle` parametreleri `GetHtml` yöntemi. Bu parametreleri için bir dakika önce eklemiş olduğunuz CSS stilleri adlarını ayarlandı.

Sayfayı çalıştırın ve bu süre önce daha çok daha az düz görünür bir kılavuz görebilirsiniz:

![CSS sınıf adları için parametreleri içeren WebGrid görüntü ayarlama](displaying-data/_static/image20.png)

Ne kadar `GetHtml` oluşturulan yöntemi göz önünde bulundurmanız sayfasını tarayıcıda kaynağındaki. Biz burada ayrıntıya gitmiyor, ancak en önemli nokta bu gibi parametreleri belirterek `tableStyle`, aşağıdaki gibi HTML etiketleri oluşturulacak kılavuz neden:

`<table class="grid">`

`<table>` Etiketi vardı bir `class` eklendiğinde özniteliği, daha önce eklediğiniz CSS kurallardan biri başvuruyor. Bu kod temel düzeni gösterir &mdash; için farklı parametreler `GetHtml` yöntemi izin verin, başvuru CSS sınıfları yöntem ardından birlikte biçimlendirme oluşturur. CSS sınıfları ile neler size bağlıdır.

## <a name="adding-paging"></a>Sayfalama ekleme

Bu öğretici için son görev, disk belleği kılavuza ekleyeceksiniz. Tek seferde tüm filmlere görüntülenecek sorun hemen. Ancak, filmler yüzlerce eklediyseniz, sayfa görüntüleme uzun elde edersiniz.

Sayfa kodunda oluşturan satırı değiştirin `WebGrid` aşağıdaki koda nesnesi:

[!code-csharp[Main](displaying-data/samples/sample7.cs)]

Öğesinden önce tek fark, eklediğiniz bir `rowsPerPage` 3'e ayarlanan parametre.

Sayfayı çalıştırın. Kılavuz saati artı veritabanınızdaki filmler sayfadan olanak tanıyan bir gezinti bağlantıları 3 satır görüntüler:

![Disk belleği ile WebGrid görüntüleme](displaying-data/_static/image21.png)

## <a name="coming-up-next"></a>Sıradaki gelen

Sonraki öğreticide, bir forma kullanıcı girişi almak için Razor ve C# kodu kullanmayı öğreneceksiniz. Başlık veya türe göre filmler bulabilmesi filmler sayfasına bir arama kutusu ekleyeceksiniz.

## <a name="complete-listing-for-movies-page"></a>Tam listesi için filmler sayfası

[!code-cshtml[Main](displaying-data/samples/sample8.cshtml)]

## <a name="additional-resources"></a>Ek Kaynaklar

- [ASP.NET Web programlama Razor söz dizimini kullanarak giriş](https://go.microsoft.com/fwlink/?LinkID=202890)

> [!div class="step-by-step"]
> [Önceki](intro-to-web-pages-programming.md)
> [İleri](form-basics.md)
