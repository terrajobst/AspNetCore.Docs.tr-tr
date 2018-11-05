---
uid: web-pages/overview/data/5-working-with-data
title: ASP.NET Web veritabanıyla çalışmaya giriş sayfaları (Razor) siteler | Microsoft Docs
author: Rick-Anderson
description: Bu bölümde, bir veritabanından veri erişim ve ASP.NET Web Pages kullanılarak görüntüleme açıklar.
ms.author: riande
ms.date: 02/18/2014
ms.assetid: 673d502f-2c16-4a6f-bb63-dbfd9a77ef47
msc.legacyurl: /web-pages/overview/data/5-working-with-data
msc.type: authoredcontent
ms.openlocfilehash: a688761c87376aa93463c13eaa07858d3acb9dc2
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021696"
---
<a name="introduction-to-working-with-a-database-in-aspnet-web-pages-razor-sites"></a>ASP.NET Web veritabanıyla çalışmaya giriş sayfaları (Razor) siteler
====================
tarafından [Tom FitzMacken](https://github.com/tfitzmac)

> Bu makalede, bir ASP.NET Web sayfaları (Razor) Web sitesinde bir veritabanı oluşturmak için Microsoft WebMatrix araçları kullanmayı ve görüntüleme, ekleme, düzenleme ve verilerini sil olanak tanıyan sayfalarının nasıl oluşturulacağını açıklar.
> 
> **Öğrenecekleriniz:** 
> 
> - Bir veritabanı oluşturma
> - Bir veritabanına bağlanma.
> - Bir web sayfasında verileri görüntülemek nasıl.
> - Ekleme, güncelleştirme ve veritabanı kayıtlarını sil nasıl.
> 
> Bu makalede sunulan özellikler şunlardır:
> 
> - Microsoft SQL Server Compact Edition veritabanı ile çalışma.
> - SQL sorguları ile çalışma.
> - `Database` Sınıfı.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Bu öğreticide kullanılan yazılım sürümleri
> 
> 
> - ASP.NET Web sayfaları (Razor) 2
> - WebMatrix 2
>   
> 
> Bu öğreticide, WebMatrix 3'ile de çalışır. ASP.NET Web Pages 3 ve Visual Studio 2013 (veya Visual Studio Express 2013 Web için); kullanabilirsiniz Ancak, kullanıcı arabiriminin farklı olacaktır.


## <a name="introduction-to-databases"></a>Veritabanlarına giriş

Bir normal adres defteri düşünün. Kullanıcının adres defterinde her giriş için (diğer bir deyişle, her kişi için) birkaç bölümü ele alınmakta ad, Soyadı, adresini, e-posta adresi ve telefon numarası gibi bilgileri vardır.

Bir tipik böyle resim verileri satırlar ve sütunlarla tablo olarak yoludur. Veritabanı bağlamında, her satır genellikle bir kaydı olarak adlandırılır. Her sütun (bazen alanları olarak adlandırılır) her veri türü için bir değer içeriyor: ad, son adı ve benzeri.

| **ID** | **FirstName** | **Soyadı** | **Adresi** | **E-posta** | **Telefon** |
| --- | --- | --- | --- | --- | --- |
| 1. | Jim | Abrus | 210 100th St SE Orcas WA 98031 | jim@contoso.com | 555 0100 |
| 2 | Terry | Adams | 1234, ana, St., Seattle, WA, 99011 | terry@cohowinery.com | 555 0101 |

Çoğu veritabanı tabloları için tablo müşteri numarası, hesap numarası, vb. gibi benzersiz bir tanımlayıcı içeren sütun olması gerekir. Bu tablonun bilinir *birincil anahtar*, ve tablodaki her satır tanımlamak için kullanın. Örnekte, kimlik sütunu adres defteri için birincil anahtardır.

Veritabanlarının temel bilgilere sahip basit bir veritabanı oluşturma ve ekleme, değiştirme ve verileri silme gibi işlemleri hakkında bilgi edinmeye hazır.

> [!TIP] 
> 
> **İlişkisel veritabanları**
> 
> Metin dosyalarını ve elektronik tablolar dahil olmak üzere çok sayıda veri depolayabilirsiniz. İş kullanımlar için veriler, ilişkisel bir veritabanında depolanır.
> 
> Bu makalede çok derin bir şekilde veritabanlarına gideceği anlamına gelmez. Ancak, bunlar hakkında biraz anlamak yararlı bulabilirsiniz. İlişkisel bir veritabanında bilgileri ayrı tablolara mantıksal olarak ayrılır. Örneğin, okullar için bir veritabanı sınıfı teklifleri ve öğrenciler için ayrı tablolar içerebilir. Dinamik olarak sağlayan veritabanı (SQL Server gibi) yazılım destekler güçlü komutlarını tablolar arasında ilişki oluşturun. Örneğin, bir zamanlama oluşturmak için öğrencilerinizi ve sınıflarınızı arasında mantıksal bir ilişki kurmak için ilişkisel veritabanını kullanabilirsiniz. Ayrı tablolarda veri depolama, tablo yapısı karmaşıklığını azaltır ve gereksiz verileri tabloda tutmak gereksinimini azaltır.


## <a name="creating-a-database"></a>Veritabanı Oluşturma

Bu yordam Webmatrix'te SQL Server Compact veritabanı tasarım aracı kullanarak SmallBakery adlı bir veritabanı oluşturma işlemini gösterir. Kod kullanarak bir veritabanı oluşturabilirsiniz, ancak WebMatrix gibi bir tasarım aracını kullanarak veritabanı tablo ve veritabanı oluşturmak için daha tipik bir durumdur.

1. WebMatrix başlatın ve hızlı başlangıç sayfasında **Site şablondan**.
2. Seçin **boş Site**hem de **Site adı** kutusunu "SmallBakery" girin ve ardından **Tamam**. Site oluşturulur ve Webmatrix'te görüntülenir.
3. Sol bölmede **veritabanları** çalışma.
4. Şeritte tıklayın **yeni veritabanı**. Siteniz olarak aynı ada sahip boş bir veritabanı oluşturulur.
5. Sol bölmede genişletin **SmallBakery.sdf** düğümünü ve ardından **tabloları**.
6. Şeritte tıklayın **yeni tablo**. WebMatrix, Tablo Tasarımcısı açılır.

    ![[image]](5-working-with-data/_static/image1.jpg)
7. Tıklayın **adı** sütun girin &quot;kimliği&quot;.
8. İçinde **veri türü** sütunundaki **int**.
9. Ayarlama **olan birincil anahtar?** ve **olduğunu belirlemek?** seçenekleri **Evet**.

    Adından da anlaşılacağı gibi **olan birincil anahtar** veritabanı bu tablonun birincil anahtar olacaktır söyler. **Kimlik** her yeni kayıt için bir kimlik numarası otomatik olarak oluşturmak ve sonraki sıralı sayıyı (1'den başlayarak) atamak için veritabanı söyler.
10. Sonraki satırda tıklayın. Yeni bir sütun tanımı Düzenleyicisi'ni başlatır.
11. Adı değerini girin &quot;adı&quot;.
12. İçin **veri türü**, seçin &quot;nvarchar&quot; ve uzunluğu 50'ye ayarlayın. *Var* parçası `nvarchar` veritabanı bu sütun için verilerin boyutu kayıt kaydı gösterebilir bir dize olmasını söyler. ( *n* önek temsil *Ulusal*, alanın temsil eden bir alfabetik karakter verileri içerebileceği belirten veya yazı sistemi &#8212; diğer bir deyişle, alanın Unicode verileri tutar.)
13. Ayarlama **null değerlere izin ver** seçeneğini **Hayır**. Bu, zorunlu kılacak *adı* sütun değil boş bırakılır.
14. Bu aynı işlemi kullanılarak adlı bir sütun oluşturma *açıklama*. Ayarlama **veri türü** "nvarchar" ve uzunluğunu ve küme için 50 **null değerlere izin ver** false.
15. Adlı bir sütun oluşturma *fiyat*. Ayarlama **"para" veri türüne** ayarlayıp **null değerlere izin ver** false.
16. Üstteki kutuya tablosunu adlandırın &quot;ürün&quot;.

    İşiniz bittiğinde tanımı şöyle görünür:

    ![[image]](5-working-with-data/_static/image2.jpg)
17. Tablo kaydetmek için CTRL + S tuşuna basın.

## <a name="adding-data-to-the-database"></a>Veritabanına veri ekleme

Şimdi bazı örnek veriler, makalenin sonraki bölümlerinde ile çalışacaksınız veritabanınıza ekleyebilirsiniz.

1. Sol bölmede genişletin **SmallBakery.sdf** düğümünü ve ardından **tabloları**.
2. Product tablosuna sağ tıklayın ve ardından **veri**.
3. Düzenleme bölmesinde aşağıdaki kayıtlarını girin:

    | **Ad** | **Açıklama** | **Fiyat** |
    | --- | --- | --- |
    | Ekmek | Her gün yeni desteklenmiş. | 2.99 |
    | Strawberry Shortcake | Organik Çilek ile bizim bahçesi yapıldı. | 9.99 |
    | Apple pasta | Annenizin'ın pasta yalnızca ikinci. | 12.99 |
    | Pecan pasta | Pecans istiyorsanız, bu size göre. | 10.99 |
    | Limonlu pasta | Dünyanın en iyi lemons ile yapılır. | 11.99 |
    | Leziz çörekler | Bunlar, çocukları ve, çocuk seveceksiniz. | 7.99 |

    İçin herhangi bir şey girmeniz gerekmez unutmayın *kimliği* sütun. Oluştururken *kimliği* sütununda ayarladığınız kendi **olan kimlik** özelliği true olarak otomatik olarak doldurulması neden olur.

    Veri girme işlemini tamamladığınızda, Tablo Tasarımcısı şu şekilde görünür:

    ![[image]](5-working-with-data/_static/image3.jpg)
4. Veritabanı verilerini içeren sekmeyi kapatın.

## <a name="displaying-data-from-a-database"></a>Veritabanından veri görüntüleme

Verilerle bir veritabanı içinde başladıktan sonra bir ASP.NET web sayfasındaki verileri görüntüleyebilir. Görüntülenecek tablo satırları seçmek için veritabanına geçirdiğiniz bir komuttur bir SQL deyimi kullanın.

1. Sol bölmede **dosyaları** çalışma.
2. Web sitesinin kök dizininde adlı yeni bir CSHTML sayfa oluşturma *ListProducts.cshtml*.
3. Mevcut biçimlendirme aşağıdakiyle değiştirin:

    [!code-cshtml[Main](5-working-with-data/samples/sample1.cshtml)]

    İlk kod bloğunda açtığınız *SmallBakery.sdf* daha önce oluşturduğunuz bir dosya (veritabanı). `Database.Open` Yöntemi varsayar *.sdf* dosyasıdır Web sitenize ait *uygulama\_veri* klasör. (Belirtmeniz gerekmez bildirimi *.sdf* uzantısı &#8212; bunu yaparsanız, aslında `Open` yöntemi çalışmaz.)

    > [!NOTE]
    > *Uygulama\_veri* veri dosyalarını depolamak için kullanılan ASP.NET özel bir klasörde bir klasördür. Daha fazla bilgi için [bir veritabanına bağlanma](#SB_ConnectingToADatabase) bu makalenin ilerleyen bölümlerinde.

    Aşağıdaki SQL kullanarak veritabanını sorgulamak için bir istek daha sonra yaptığınız `Select` deyimi:

    [!code-sql[Main](5-working-with-data/samples/sample2.sql)]

    Deyiminde `Product` sorgu tabloya tanımlar. `*` Karakter tablodan tüm sütunları sorgu döndürmesi gerektiğini belirtir. (, Ayrıca sütunları tek tek, sütunların yalnızca bir bölümünü görmek istiyorsanız, virgülle ayırarak listeleyebilirsiniz.) `Order By` Yan tümcesi verileri nasıl sıralanması gerektiğini belirtir &#8212; tarafından bu durumda, *adı* sütun. Bu veriler değerini göre alfabetik olarak sıralanır anlamına gelir *adı* her satır için sütun.

    Sayfasının gövdesinde, verileri görüntülemek için kullanılan HTML tablosuna biçimlendirmeyi oluşturur. İçinde `<tbody>` öğesi, kullandığınız bir `foreach` tek tek sorgu tarafından döndürülen her veri satırı almak için döngü. Her veri satırı için bir HTML tablo satırı oluştur (`<tr>` öğesi). HTML tablosu hücrelerinin oluşturup (`<td>` öğeleri) her sütun için. Döngü gittiğiniz her zaman kullanılabilir sonraki satırda veritabanından bulunduğu `row` değişken (Bu, ayarladığınız `foreach` deyimi). Tek bir sütun satırdan almak için kullanabileceğiniz `row.Name` veya `row.Description` veya her sütun, adıdır.
4. Sayfanın tarayıcıda çalıştırın. (Emin sayfanın içinde seçili **dosyaları** çalıştırmadan önce çalışma alanı.) Sayfanın aşağıdakine benzer bir liste görüntüler:

    ![[image]](5-working-with-data/_static/image4.jpg)

> [!TIP] 
> 
> **Yapılandırılmış sorgu dili (SQL)**
> 
> SQL veritabanındaki verileri yönetmek için kullanılan çoğu ilişkisel veritabanı içinde bir dildir. Bu verileri almak ve güncelleştirmek izin veren ve oluşturma, değiştirme ve veritabanı tabloları yönetmenize izin veren komutlarını içerir. Bir programlama dili (Webmatrix'te kullanmakta olduğunuz bir gibi) farklı SQL ile SQL, veritabanı istediklerinizi söyleyin ve nasıl veri elde etmek veya görevi gerçekleştirmek için veritabanının iş olduğunu fikir olduğundan. Bazı SQL komutlarını örnekleri ve ne yaptıklarını şunlardır:
> 
> `SELECT Id, Name, Price FROM Product WHERE Price > 10.00 ORDER BY Name`
> 
> Bu getirir *kimliği*, *adı*, ve *fiyat* kayıt sütunlarından *ürün* varsa Tablo değerini *Fiyat* 10'dan fazla ve değerlerine göre alfabetik sırada sonuçları döndürür *adı* sütun. Bu komut hiçbir kayıt eşleşiyorsa karşılayan ölçütleri ya da boş kayıtları içeren bir sonuç kümesi döndürür.
> 
> `INSERT INTO Product (Name, Description, Price) VALUES ("Croissant", "A flaky delight", 1.99)`
> 
> Bu yeni bir kayıt ekler *ürün* tablosu ayarlama, *adı* sütuna &quot;Croissant&quot;, *açıklama* sütunu&quot; Güvenilir olmayan bir beğeni&quot;fiyat 1.99 için.
> 
> `DELETE FROM Product WHERE ExpirationDate < "01/01/2008"`
> 
> Bu komut, kayıtları siler *ürün* sona erme tarihi sütununu 1 Ocak 2008'den önceki bir tablo. (Bu varsayar *ürün* tablolu böyle bir sütunu tabii.) Tarih GG/AA/YYYY biçiminde buraya girilir ancak bölgeniz için kullanılan biçiminde girilmelidir.
> 
> `Insert Into` Ve `Delete` komutları yoksa sonuç kümesi döndürür. Bunun yerine, bunlar komutu tarafından kaç tane kaydın etkilendiğini belirten bir sayı döndürür.
> 
> Veritabanında uygun izinlere sahip bazıları bu işlemleri (örneğin, ekleme ve kayıt silme) için işlem isteme işlemi gerekir. Bu, genellikle veritabanına bağlanırken bir kullanıcı adı ve parola sağlamanız gerekir üretim veritabanları için neden olur.
> 
> SQL komutlarını onlarca vardır, ancak hepsi bu gibi deseni izler. Veritabanı tabloları oluşturmak, bir tablodaki kayıtların sayısını, fiyatları hesaplayın ve birçok diğer işlemleri gerçekleştirmek için SQL komutlarını kullanabilirsiniz.


## <a name="inserting-data-in-a-database"></a>Bir veritabanında veri ekleme

Bu bölümde, yeni ürün eklemek kullanıcıların olanak sağlayan bir sayfa oluşturma işlemi gösterilmektedir *ürün* veritabanı tablosu. Yeni bir ürün kaydı eklendikten sonra güncelleştirilmiş tablo kullanarak sayfası görüntüler *ListProducts.cshtml* önceki bölümde oluşturduğunuz sayfası.

Sayfa kullanıcının girdiği verileri veritabanı için geçerli olduğundan emin olmak için doğrulama içerir. Örneğin, kod sayfasında bir değer için gerekli olan tüm sütunlar girildiğinden emin olur.

1. Web sitesi, adlı yeni bir CSHTML dosyası oluşturma *InsertProducts.cshtml*.
2. Mevcut biçimlendirme aşağıdakiyle değiştirin:

    [!code-cshtml[Main](5-working-with-data/samples/sample3.cshtml)]

    Sayfanın gövdesi bir ad, açıklama ve fiyat girin kullanıcıların üç metin kutusu ile bir HTML formuna içerir. Kullanıcılar'ı tıklattığınızda **Ekle** düğmesi, kod sayfasının üst kısmındaki açılır bağlantı *SmallBakery.sdf* veritabanı. Ardından kullanıcı kullanılarak gönderilen değerler Al `Request` nesne ve yerel değişkenlere değerler atayın.

    Kullanıcı gerekli her sütun için bir değer girdiğinizde doğrulamak için her kayıt `<input>` doğrulamak istediğiniz öğe:

    [!code-csharp[Main](5-working-with-data/samples/sample4.cs)]

    `Validation` Yardımcısı var olup olmadığını denetler bir değer kaydettiğinize göre alanların her birinde. Tüm alanları denetleyerek doğrulama başarılı olup olmadığını sınayabilirsiniz `Validation.IsValid()`, kullanıcıdan size bilgi işlemeden önce genellikle yaparsınız:

    [!code-csharp[Main](5-working-with-data/samples/sample5.cs)]

    ( `&&` İşleci anlamına gelir ve — bu test *bu form gönderme ve tüm alanları doğrulama başarılı*.)

    Tüm sütunları doğrulanmış durumunda (hiçbiri boş), devam edin ve verileri ekleyin ve aşağıda gösterildiği yürütmek için bir SQL deyimi oluşturmak:

    [!code-csharp[Main](5-working-with-data/samples/sample6.cs)]

    Değerler eklemek parametre yer tutucular içerir (`@0`, `@1`, `@2`).

    > [!NOTE]
    > Önceki örnekte gördüğünüz gibi bir güvenlik önlemi olarak parametrelerini kullanarak bir SQL deyimi için her zaman değerlerini geçirin. Bu kullanıcının verileri doğrulamak için bir şans verir ve ayrıca veritabanınıza (bazen SQL ekleme saldırılarına karşı adlandırılır) kötü amaçlı komut gönderme girişimleri karşı korunmasına yardımcı olur.

    Sorguyu yürütmek için yer tutucular yerine değerleri içeren değişkenlerini iletmeden bu bildirimi kullanın:

    [!code-csharp[Main](5-working-with-data/samples/sample7.cs)]

    Sonra `Insert Into` deyimi yürütüldüğünde, kullanıcının bu satırı kullanarak olduğu ürünleri listeler sayfasına gönderin:

    [!code-javascript[Main](5-working-with-data/samples/sample8.js)]

    Doğrulama başarılı olmadıysa INSERT atlayın. Bunun yerine, bir yardımcı toplanan hata iletileri (varsa) görüntüleyen sayfanın vardır:

    [!code-cshtml[Main](5-working-with-data/samples/sample9.cshtml)]

    Stil bloğu biçimlendirmede adlı CSS sınıf tanımını içeren bildirim `.validation-summary-errors`. Bu, varsayılan olarak kullanılan CSS sınıfının adını `<div>` tüm doğrulama hatalarını içeren öğe. Bu durumda, CSS sınıfının doğrulama özeti hata içinde kalın ve kırmızı renkte görüntülenir, ancak tanımlayabileceğiniz belirtir `.validation-summary-errors` biçimlendirmeleri istediğiniz görüntülemek için sınıf.

### <a name="testing-the-insert-page"></a>INSERT sayfasını test etme

1. Bir tarayıcıda sayfasını görüntüleyin. Sayfasında aşağıdaki çizimde gösterilen bir benzer bir form görüntüler.

    ![[image]](5-working-with-data/_static/image5.jpg)
2. Tüm sütunlar için değerleri girin, ancak siz bıraktığınızdan emin olun *fiyat* sütunu boş.
3. **Ekle**'ye tıklayın. Sayfasında, aşağıdaki çizimde gösterildiği gibi bir hata iletisi görüntüler. (Yeni bir kayıt oluşturulur.)

    ![[image]](5-working-with-data/_static/image6.jpg)
4. Tamamen formu doldurun ve ardından **Ekle**. Bu kez, *ListProducts.cshtml* sayfası görüntülenir ve burada yeni kayıt gösterilir.

## <a name="updating-data-in-a-database"></a>Bir veritabanındaki verileri güncelleştirme

Verileri bir tabloya girildikten sonra güncelleştirmeniz gerekebilir. Bu yordamda daha önce veri ekleme için oluşturulan ilkeleri benzer iki sayfalarının nasıl oluşturulacağını gösterir. İlk sayfa ürünleri görüntüler ve değiştirmek için kullanıcıların olanak tanır. İkinci sayfasında gerçekten düzenlemeleri yapın ve bunları kaydetme olanağı sunar.

> [!NOTE] 
> 
> **Önemli** bir üretim Web sitesine, genelde kimin değişiklik verilere izin kısıtlama. Sitede görevleri gerçekleştirmek için Kullanıcıları yetkilendirmek için yolu ve üyeliği ayarlama hakkında bilgi için [güvenlik ekleme ve bir ASP.NET Web sayfaları sitesinde üyelik](https://go.microsoft.com/fwlink/?LinkId=202904).


1. Web sitesi, adlı yeni bir CSHTML dosyası oluşturma *EditProducts.cshtml*.
2. Dosya mevcut işaretlemede aşağıdakiyle değiştirin:

    [!code-cshtml[Main](5-working-with-data/samples/sample10.cshtml)]

    Bu sayfa arasındaki tek fark ve *ListProducts.cshtml* sayfası daha önce bu sayfadaki HTML tablosu görüntüler için ek bir sütun içerdiğini dandır bir **Düzenle** bağlantı. Bu bağlantıya tıkladığınızda, size gereken *UpdateProducts.cshtml* (Bu, sonraki oluşturursunuz) sayfası burada düzenleyebilirsiniz seçili kayıt.

    Konum oluşturan koda **Düzenle** bağlantı:

    [!code-cshtml[Main](5-working-with-data/samples/sample11.cshtml)]

    Bu, bir HTML oluşturur `<a>` öğesi olan `href` özniteliği dinamik olarak ayarlayın. `href` Özniteliği kullanıcı bağlantıyı tıklattığında görüntülenecek belirtir. Bu işlem ayrıca geçirir `Id` bağlantıya geçerli satır değeri. Sayfa çalıştığında, sayfa kaynağı bu bağlantılar içerebilir:

    [!code-html[Main](5-working-with-data/samples/sample12.html)]

    Dikkat `href` özniteliği `UpdateProducts/n`burada *n* ürün numarası. Bir kullanıcı için aşağıdaki bağlantılardan birini tıkladığında çıkan URL şöyle görünür:

    `http://localhost:18816/UpdateProducts/6`

    Diğer bir deyişle, düzenlenecek ürün sayısı, URL'de geçirilir.
3. Bir tarayıcıda sayfasını görüntüleyin. Sayfa verileri bunun gibi bir biçimde görüntüler:

    ![[image]](5-working-with-data/_static/image7.jpg)

    Ardından, gerçekten verileri güncelleştirme olanağı sayfanın oluşturacaksınız. Güncelleştirme sayfasını kullanıcının girdiği verileri doğrulamak için doğrulama içerir. Örneğin, kod sayfasında bir değer için gerekli olan tüm sütunlar girildiğinden emin olur.
4. Web sitesi, adlı yeni bir CSHTML dosyası oluşturma *UpdateProducts.cshtml*.
5. Dosya mevcut işaretlemede aşağıdakiyle değiştirin.

    [!code-cshtml[Main](5-working-with-data/samples/sample13.cshtml)]

    Sayfanın gövdesi bir HTML formuna bir ürün burada görüntülenir ve burada kullanıcıları düzenleyebilirsiniz içerir. Görüntülenecek bir ürün almak için bu SQL deyimini kullanın:

    [!code-sql[Main](5-working-with-data/samples/sample14.sql)]

    Bu ürün Kimliğine geçirilen değeri ile eşleşen seçecektir `@0` parametresi. (Çünkü *kimliği* birincil anahtar ve bu nedenle gereken benzersiz olmalı, yalnızca bir ürünü kayıt şimdiye kadar bu şekilde seçilebilir.) Bu kimliği değerini almak için `Select` deyimi, sayfaya aşağıdaki sözdizimini kullanarak URL'nin bir parçası geçirilen değer bilgi edinebilirsiniz:

    [!code-csharp[Main](5-working-with-data/samples/sample15.cs)]

    Ürün kaydı gerçekten getirmek için kullandığınız `QuerySingle` yöntemi yalnızca bir kayıt döndürür:

    [!code-csharp[Main](5-working-with-data/samples/sample16.cs)]

    Tek bir satır halinde döndürülür `row` değişkeni. Her bir sütunun veri almak ve bunun gibi yerel değişkenler atayın:

    [!code-csharp[Main](5-working-with-data/samples/sample17.cs)]

    Formu için biçimlendirme içinde bu değerleri otomatik olarak bireysel metin kutularında aşağıdaki gibi katıştırılmış bir kod kullanarak görüntülenir:

    [!code-html[Main](5-working-with-data/samples/sample18.html)]

    Bu kod parçası, güncelleştirilecek ürün kaydı görüntüler. Kayıt görüntülenen sonra kullanıcının her bir sütun düzenleyebilirsiniz.

    Kullanıcı gönderdiğinde form tıklayarak **güncelleştirme** button, kodda `if(IsPost)` bloğu çalışır. Bu kullanıcının değerleri alır `Request` nesne, değişken değerlerini depolar ve her sütun doldurulmuş olduğunu doğrular. Doğrulama testlerini geçerse, kod aşağıdaki SQL Update deyimi oluşturur:

    [!code-sql[Main](5-working-with-data/samples/sample19.sql)]

    Bir SQL `Update` deyimi, güncelleştirme ve değerini ayarlamak için her bir sütun belirtin. Bu kodu, değerleri parametre yer tutucuları kullanılarak belirtilen `@0`, `@1`, `@2`ve benzeri. (Güvenlik için daha önce belirtildiği gibi her zaman değerleri için bir SQL deyimi parametreleri kullanarak geçirmeniz.)

    Çağırdığınızda `db.Execute` yöntemi sırayla SQL deyiminde parametrelerine karşılık gelen değerleri içeren değişkenlerini geçirin:

    [!code-csharp[Main](5-working-with-data/samples/sample20.cs)]

    Sonra `Update` deyimi yürütüldüğünde, kullanıcıyı Düzen sayfasına yeniden yönlendirmek için aşağıdaki yöntemi çağırın:

    [!code-cshtml[Main](5-working-with-data/samples/sample21.cshtml)]

    Bir kullanıcı veritabanındaki verilerle güncelleştirilmiş bir listesini görür ve başka bir ürün düzenleyebilirsiniz etkisidir.
6. Sayfayı kaydedin.
7. Çalıştırma *EditProducts.cshtml* sayfa (güncelleştirme sayfada değil) ve ardından **Düzenle** düzenlemek için bir ürün seçin. *UpdateProducts.cshtml* sayfası görüntülenirse, seçtiğiniz kaydı gösteriliyor.

    ![[image]](5-working-with-data/_static/image8.jpg)
8. Bir değişiklik yapın ve tıklayın **güncelleştirme**. Ürün listesini yeniden güncelleştirilmiş verilerinizle gösterilir.

## <a name="deleting-data-in-a-database"></a>Bir veritabanındaki verileri silme

Bu bölümde, bir üründen silme kullanıcıların gösterilmektedir *ürün* veritabanı tablosu. Örnek iki sayfa içerir. İlk sayfasında, bir kaydı silmek için kullanıcıları seçin. Silinecek kaydın daha sonra kaydı silmek istediğinizi onaylayın olanak sağlayan bir ikinci sayfasında görüntülenir.

> [!NOTE] 
> 
> **Önemli** bir üretim Web sitesine, genelde kimin değişiklik verilere izin kısıtlama. Sitede görevleri gerçekleştirmek için kullanıcı yetkilendirmek için yol ve üyeliği ayarlama hakkında bilgi için [güvenlik ekleme ve bir ASP.NET Web sayfaları sitesinde üyelik](https://go.microsoft.com/fwlink/?LinkId=202904).


1. Web sitesi, adlı yeni bir CSHTML dosyası oluşturma *ListProductsForDelete.cshtml*.
2. Mevcut biçimlendirme aşağıdakiyle değiştirin:

    [!code-cshtml[Main](5-working-with-data/samples/sample22.cshtml)]

    Bu sayfa benzer *EditProducts.cshtml* sayfasından daha önce. Ancak, görüntüleme yerine bir **Düzenle** bağlantı her ürün için görüntüler bir **Sil** bağlantı. **Sil** bağlantı, biçimlendirmeyi aşağıdaki katıştırılmış kodu kullanılarak oluşturulur:

    [!code-cshtml[Main](5-working-with-data/samples/sample23.cshtml)]

    Bu, kullanıcılar bu bağlantıya tıkladığında şuna benzer bir URL oluşturur:

    `http://<server>/DeleteProduct/4`

    Adlı sayfanın URL'sini çağırır *DeleteProduct.cshtml* (Bu, sonraki oluşturursunuz) ve silmek için ürün kimliği geçirir (burada, 4).
3. Dosyayı kaydedin ve açık bırakın.
4. Adlı başka bir CHTML dosya oluşturma *DeleteProduct.cshtml*. Mevcut içeriğini aşağıdakiyle değiştirin:

    [!code-cshtml[Main](5-working-with-data/samples/sample24.cshtml)]

    Bu sayfa tarafından çağrılır *ListProductsForDelete.cshtml* ve kullanıcıların bir ürünü silmek istediğinizi onaylayın olanak sağlar. Silinecek ürün listelemek için aşağıdaki kodu kullanarak URL'den silmek için ürün Kimliğini alın:

    [!code-csharp[Main](5-working-with-data/samples/sample25.cs)]

    Sayfa kullanıcının gerçekten kaydı silmek için bir düğmeye tıklayın ardından ister. Bu önemli bir güvenlik önlemidir: Web sitenizi güncelleştirmek veya veri silme gibi hassas işlemler gerçekleştirdiğinizde, bu işlemlerin her zaman yapılmalıdır kullanarak bir gönderme işlemi, bir alma işlemi. Böylece silme işlemi, bir Al işlemi kullanılarak gerçekleştirilebilir sitenizi ayarlanıp ayarlanmadığını herkes gibi bir URL geçirebilirsiniz `http://<server>/DeleteProduct/4` ve istediği herhangi bir şey veritabanından silebilirsiniz. Doğrulama ekleme ve silme işlemi yalnızca bir POST kullanılarak gerçekleştirilebilir. böylece sayfa kodlama sitenize güvenlik ölçü ekleyin.

    Post işlemi budur ve kimliği boş olmayan ilk onaylar aşağıdaki kodu kullanarak gerçek silme işlemi gerçekleştirilir:

    [!code-csharp[Main](5-working-with-data/samples/sample26.cs)]

    Kodu belirtilen kaydını siler ve sonra kullanıcı listesi sayfasına yönlendiren bir SQL deyimi çalıştırır.
5. Çalıştırma *ListProductsForDelete.cshtml* bir tarayıcıda.

    ![[image]](5-working-with-data/_static/image9.jpg)
6. Tıklayın **Sil** ürünlerinden biri için bağlantı. *DeleteProduct.cshtml* o kaydı silmek istediğinizi onaylamak için sayfası görüntülenir.
7. Tıklayın **Sil** düğmesi. Ürün kaydı silinir ve sayfanın bir güncelleştirilen ürün listesi ile yenilenir.

> [!TIP]
> 
> <a id="SB_ConnectingToADatabase"></a>
> ### <a name="connecting-to-a-database"></a>Bir veritabanına bağlanma
> 
> Bir veritabanına iki yolla bağlanabilir. İlk kullanmaktır `Database.Open` yöntemi ve veritabanı dosyası adını belirtin (daha az *.sdf* uzantısı):
> 
> `var db = Database.Open("SmallBakery");`
> 
> `Open` Yöntemi varsayar. *sdf* dosyasıdır Web sitesinin içinde *uygulama\_veri* klasör. Bu klasör, özellikle verileri tutmak için tasarlanmıştır. Örneğin, Web sitesinin verileri okumasına ve yazmasına izin vermek için uygun izinlere sahip ve bir güvenlik önlemi olarak WebMatrix access dosyalarını bu klasörden izin vermiyor.
> 
> İkinci bir bağlantı dizesi kullanmaktır. Bir bağlantı dizesi, bir veritabanına bağlanma hakkında bilgi içerir. Bu bir dosya yolu içerebilir veya bir yerel veya uzak sunucuya bir kullanıcı adı ve parola, sunucuya bağlanmak için bir SQL Server veritabanının adını içerebilir. (Veri merkezi olarak yönetilen bir SQL Server sürümünde tutarsanız, olduğu gibi bir barındırma sağlayıcısının sitesinde, her zaman bir bağlantı dizesi veritabanı bağlantısı bilgilerini belirtmek için kullanın.)
> 
> Webmatrix'te, bağlantı dizeleri genellikle adlı bir XML dosyasında depolanan *Web.config*. Adından da anlaşılacağı gibi kullanabileceğiniz bir *Web.config* sitenizi gerektirebilecek herhangi bir bağlantı dizesi dahil olmak üzere, sitenin yapılandırma bilgilerini depolamak için Web sitenizin kök dosyasında. Bir bağlantı dizesi örneği bir *Web.config* dosya, aşağıdaki gibi görünebilir:
> 
> [!code-xml[Main](5-working-with-data/samples/sample27.xml)]
> 
> Örnekte, bağlantı dizesini işaret yere bir sunucu üzerinde çalışan SQL Server örneğini bir veritabanına (yerel aksine *.sdf* dosyası). Uygun adlarla yerine gerek `myServer` ve `myDatabase`ve SQL Server oturum açma değerlerini belirtin `username` ve `password`. (Kullanıcı adı ve parola değerleri mutlaka aynı Windows kimlik bilgilerinizi veya kendi sunucuya oturum açmak için verdiği barındırma sağlayıcınızda değerleri olarak değildir. Gereksinim duyduğunuz tam değerlerini yöneticisine bakın.)
> 
> `Database.Open` Ya da bir veritabanı adı geçirmenize izin verdiğinden yöntemi esnek, *.sdf* dosya ya da depolanan bir bağlantı dizesi adı *Web.config* dosya. Aşağıdaki örnek, önceki örnekte gösterilen bağlantı dizesi kullanarak bir veritabanına bağlanmak gösterilmektedir:
> 
> [!code-cshtml[Main](5-working-with-data/samples/sample28.cshtml)]
> 
> Belirtildiği gibi `Database.Open` yöntemi, bir veritabanı adı veya bir bağlantı dizesi geçirmenize olanak sağlar ve kullanmak hangi keşfedeceksiniz. Dağıtımı yaptığınızda, bu çok yararlı olur (Yayımlama), Web sitesi. Kullanabileceğiniz bir *.sdf* dosyası *uygulama\_veri* geliştirirken ve sitenizi test ederken klasör. Sitenizi bir üretim sunucusuna taşıdığınızda, bağlantı dizesinde kullanabilirsiniz *Web.config* aynı ada sahip bir dosya, *.sdf* barındırma sağlayıcısının noktalarına &#8212;kodunuzu değiştirmek zorunda olmadan.
> 
> Son olarak, doğrudan bir bağlantı dizesi ile çalışmak istiyorsanız, çağırabilirsiniz `Database.OpenConnectionString` yöntemi ve gerçek bağlantı dizesi yalnızca birinde adı yerine geçişi *Web.config* dosya. Bu herhangi bir nedenden dolayı yoksa erişiminiz bağlantı dizesine durumlarda yararlı olabilir (veya içinde gibi değerler *.sdf* dosya adı) kadar sayfa çalıştırılır. Ancak, çoğu senaryo için kullanabileceğiniz `Database.Open` bu makalede açıklandığı gibi.


## <a name="additional-resources"></a>Ek Kaynaklar

- [SQL Server Compact](https://www.microsoft.com/sqlserver/2008/en/us/compact.aspx)
- [SQL Server veya MySQL veritabanına WebMatrix bağlanma](https://go.microsoft.com/fwlink/?LinkId=208661)
- [ASP.NET Web Sayfaları Sitelerinde Kullanıcı Girişini Doğrulama](https://go.microsoft.com/fwlink/?LinkId=253002)
