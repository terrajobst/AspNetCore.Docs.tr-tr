---
uid: web-pages/overview/data/5-working-with-data
title: "ASP.NET Web bir veritabanı ile çalışmaya giriş sayfaları (Razor) siteleri | Microsoft Docs"
author: tfitzmac
description: "Bu bölümde, bir veritabanından veri erişim ve ASP.NET Web sayfalarını kullanarak görüntülemek açıklar."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2014
ms.topic: article
ms.assetid: 673d502f-2c16-4a6f-bb63-dbfd9a77ef47
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/data/5-working-with-data
msc.type: authoredcontent
ms.openlocfilehash: 460af471a1b0650f8d782d582ce6cd9a06664d5c
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/15/2018
---
<a name="introduction-to-working-with-a-database-in-aspnet-web-pages-razor-sites"></a>ASP.NET Web bir veritabanı ile çalışmaya giriş (Razor) sayfaları
====================
tarafından [zel FitzMacken](https://github.com/tfitzmac)

> Bu makalede, bir ASP.NET Web sayfaları (Razor) Web sitesinde bir veritabanı oluşturmak için Microsoft WebMatrix araçlarını kullanmayı ve görüntüleme, ekleme, düzenleme ve veri silme olanak tanıyan sayfalarının nasıl oluşturulacağını açıklar.
> 
> **Öğrenecekleriniz:** 
> 
> - Bir veritabanı oluşturmak nasıl.
> - Bir veritabanına bağlanma.
> - Bir web sayfasında verileri görüntülemek nasıl.
> - Nasıl Ekle, Güncelleştir ve veritabanı kayıtlarını silin.
> 
> Bu makalede sunulan özellikler şunlardır:
> 
> - Microsoft SQL Server Compact Edition veritabanı ile çalışıyor.
> - SQL sorguları ile çalışma.
> - `Database` Sınıfı.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Öğreticide kullanılan yazılım sürümleri
> 
> 
> - ASP.NET Web sayfaları (Razor) 2
> - WebMatrix 2
>   
> 
> Bu öğretici, WebMatrix 3 ile de çalışır. ASP.NET Web Pages 3 ve Visual Studio 2013 (veya Visual Studio Express 2013 Web için); kullanabilirsiniz Ancak, kullanıcı arabirimi farklı olacaktır.


## <a name="introduction-to-databases"></a>Veritabanları giriş

Bir genel adres defteri düşünün. Adres defteri her giriş için (diğer bir deyişle, her kişi için) ad, Soyadı, adresini, e-posta adresi ve telefon numarası gibi bilgileri çeşitli parçalarını sahip.

Satırları ve sütunları içeren bir tablo resim verileri bu gibi tipik bir şekilde gibidir. Veritabanı bağlamında her satır genellikle bir kayıt olarak adlandırılır. (Bazen alanlar olarak adlandırılır) her bir sütunun her veri türü için bir değer içeriyor: ad, son adı ve benzeri.

| **ID** | **FirstName** | **Soyadı** | **Adres** | **E-posta** | **Telefon** |
| --- | --- | --- | --- | --- | --- |
| 1. | Jim | Abrus | 210 100th St SE Orcas WA 98031 | jim@contoso.com | 555 0100 |
| 2 | Terry | Adams | 1234 ana St. Seattle WA 99011 | terry@cohowinery.com | 555 0101 |

İçin çoğu veritabanı tabloları, tablonun bir müşteri numarası, hesap numarası, vb. gibi benzersiz bir tanımlayıcı içeren bir sütun sahip olması gerekir. Bu tablonun bilinir *birincil anahtar*, ve tablodaki her satır tanımlamak için kullanın. Örnekte, adres defteri için birincil anahtarı kimliği sütundur.

Veritabanlarının temel bilgilere sahip basit bir veritabanı oluşturun ve ekleme, değiştirme ve verileri silme gibi işlemleri gerçekleştirmek öğrenmek hazırsınız.

> [!TIP] 
> 
> **İlişkisel veritabanları**
> 
> Metin dosyaları ve elektronik tablolar dahil olmak üzere çok sayıda veri depolayabilirsiniz. Çoğu iş kullanımları, yine de veri ilişkisel bir veritabanında depolanır.
> 
> Bu makalede çok derine veritabanlarına gideceği anlamına gelmez. Ancak, siz bunları hakkında biraz anlamak yararlı olabilir. İlişkisel bir veritabanında bilgileri ayrı tablolara mantıksal olarak ayrılır. Örneğin, bir veritabanı için bir okul sınıfı teklifleri ve öğrenciler için ayrı tablolar içerebilir. Dinamik olarak sağlayan veritabanı yazılımı (örneğin, SQL Server) destekler güçlü komutları tablolar arasında ilişkiler oluşturun. Örneğin, bir zamanlama oluşturmak için Öğrenciler ve sınıflar arasında mantıksal bir ilişki kurmak için ilişkisel veritabanı kullanabilirsiniz. Ayrı tablolarda veri depolama tablo yapısı karmaşıklığını azaltır ve yedek verileri tablolarda tutabilirsiniz ihtiyacını azaltır.


## <a name="creating-a-database"></a>Veritabanı oluşturma

Bu yordam Webmatrix'te dahil SQL Server Compact veritabanı tasarım aracını kullanarak SmallBakery adlı bir veritabanı oluşturmak nasıl gösterir. Kod kullanarak bir veritabanı oluşturabilirsiniz, ancak WebMatrix gibi bir tasarım aracı kullanarak veritabanı tabloları ve veritabanı oluşturmak için daha genel bir durumdur.

1. WebMatrix başlatın ve hızlı başlangıç sayfasında, tıklatın **Site şablondan**.
2. Seçin **boş Site**hem de **Site adı** kutusunu "SmallBakery" girin ve ardından **Tamam**. Site oluşturulur ve Webmatrix'te görüntülenir.
3. Sol bölmede **veritabanları** çalışma.
4. Şeritte tıklatın **yeni veritabanı**. Boş bir veritabanı, sitenizin aynı ada sahip oluşturulur.
5. Sol bölmede **SmallBakery.sdf** düğümünü ve ardından **tabloları**.
6. Şeritte tıklatın **yeni tablo**. WebMatrix Tablo Tasarımcısı'nı açar.

    ![[Görüntü]](5-working-with-data/_static/image1.jpg)
7. ' I tıklatın **adı** sütun ve girin &quot;kimliği&quot;.
8. İçinde **veri türü** sütun, select **int**.
9. Ayarlama **birincil anahtarı?** ve **olduğunu belirlemek?** seçeneklerini **Evet**.

    Adı da anlaşılacağı gibi **birincil anahtarı** bu tablonun birincil anahtarı olacaktır veritabanı söyler. **Kimliktir** otomatik olarak her yeni bir kayıt için bir kimlik numarası oluşturmak ve (1'den başlayarak) bir sonraki sıralı numara atamak için veritabanı söyler.
10. Sonraki satırda'ı tıklatın. Yeni bir sütun tanımı Düzenleyicisi'ni başlatır.
11. Ad değer için girin &quot;adı&quot;.
12. İçin **veri türü**, seçin &quot;nvarchar&quot; ve uzunluğu 50'ye ayarlayın. *Var* parçası `nvarchar` bu sütun için veri büyüklüğü kayıt kaydı farklılık bir dize olmasını veritabanı söyler. ( *n* önek temsil *Ulusal*, alanın herhangi alfabe temsil eden karakter veri tutabilen belirten veya sistem yazma &#8212; diğer bir deyişle, alanın Unicode verileri tutar.)
13. Ayarlama **null değerlere izin ver** için seçenek **Hayır**. Bu zorunlu kılacak *adı* sütun değil boş bırakılır.
14. Bu aynı işlemi kullanarak oluşturduğunuz adlı bir sütun *açıklama*. Ayarlama **veri türü** "nvarchar" ve uzunluğu ve kümesi için 50 **null değerlere izin ver** false.
15. Adlı bir sütun oluşturmak *fiyat*. Ayarlama **"para" veri türüne** ve **null değerlere izin ver** false.
16. Üst kutusunda tablo adı &quot;ürün&quot;.

    İşiniz bittiğinde, tanım şuna benzeyecektir:

    ![[Görüntü]](5-working-with-data/_static/image2.jpg)
17. Tabloyu kaydetmek için CTRL + S tuşlarına basın.

## <a name="adding-data-to-the-database"></a>Veritabanına veri ekleme

Artık makalenin sonraki bölümlerinde ile karşılaşmayacağınızı veritabanınıza bazı örnek veriler ekleyebilirsiniz.

1. Sol bölmede **SmallBakery.sdf** düğümünü ve ardından **tabloları**.
2. Ürün tabloyu sağ tıklatın ve ardından **veri**.
3. Düzen bölmesinde aşağıdaki kayıtlarını girin:

    | **Ad** | **Açıklama** | **Fiyat** |
    | --- | --- | --- |
    | Ekmek | Fırın baştan her gün. | 2.99 |
    | Çilekli Shortcake | İle organik Çilek bizim bahçesi yapılan. | 9.99 |
    | Apple Pie | Annenizin'ın pasta yalnızca ikinci. | 12.99 |
    | Pecan pasta | Pecans istiyorsanız, bunu sizin için yazılmıştır. | 10.99 |
    | Limonlu pasta | Dünyanın en iyi lemons yapılan. | 11.99 |
    | Leziz çörekler | Bunlar, çocuklarınızın ve, çocuk memnuniyet. | 7.99 |

    Herhangi bir şey için girmek zorunda kalmadan unutmayın *kimliği* sütun. Oluşturduğunuzda *kimliği* sütun, ayarladığınız kendi **Is Identity** özelliği true olarak otomatik olarak doldurulması neden olur.

    Veri girmeyi tamamladığınızda, Tablo Tasarımcısı şuna benzeyecektir:

    ![[Görüntü]](5-working-with-data/_static/image3.jpg)
4. Veritabanı verileri içeren sekmeyi kapatın.

## <a name="displaying-data-from-a-database"></a>Bir veritabanındaki verileri görüntüleme

Verilerle bir veritabanı içinde getirdikten sonra verileri bir ASP.NET web sayfasında görüntüleyebilirsiniz. Görüntülenecek tablo satırları seçmek için veritabanına geçirdiğiniz komuttur bir SQL deyimini kullanın.

1. Sol bölmede **dosyaları** çalışma.
2. Web sitesinin kök dizininde adlı yeni bir CSHTML sayfası oluşturun *ListProducts.cshtml*.
3. Varolan biçimlendirme aşağıdakiyle değiştirin:

    [!code-cshtml[Main](5-working-with-data/samples/sample1.cshtml)]

    İlk kod bloğunda açtığınız *SmallBakery.sdf* daha önce oluşturduğunuz dosya (veritabanı). `Database.Open` Yöntemi varsayar *.sdf* dosyadır, Web sitenizin içinde *uygulama\_veri* klasör. (Belirtmenize gerek yoktur bildirimi *.sdf* uzantısı &#8212; bunu yaparsanız, aslında `Open` yöntemi çalışmayacak.)

    > [!NOTE]
    > *Uygulama\_veri* veri dosyalarını depolamak için kullanılan ASP.NET özel bir klasöre bir klasördür. Daha fazla bilgi için bkz: [bir veritabanına bağlanma](#SB_ConnectingToADatabase) bu makalenin ilerisinde yer.

    Aşağıdaki SQL kullanarak veritabanını sorgulamak için bir istek daha sonra yaptığınız `Select` deyimi:

    [!code-sql[Main](5-working-with-data/samples/sample2.sql)]

    Deyiminde `Product` sorgu tabloya tanımlar. `*` Karakteri belirtir sorgu tablodan tüm sütunları döndürmelidir. (Ayrıca sütunları ayrı ayrı yalnızca bazı sütunları görmek istiyorsanız, virgülle ayrılmış listelediğiniz.) `Order By` Yan tümcesi gösterir verileri nasıl sıralanması gerektiğini &#8212; bu durumda, tarafından *adı* sütun. Bu veriler değerini alfabetik sıraya göre sıralanır anlamına gelir *adı* her satır için sütun.

    Sayfasının gövdesinde biçimlendirme verileri görüntülemek için kullanılan bir HTML tablosu oluşturur. İçinde `<tbody>` öğesi, kullandığınız bir `foreach` tek tek sorgu tarafından döndürülen her veri satırı almak için döngü. Her veri satırı için bir HTML tablo satırı oluştur (`<tr>` öğesi). HTML tablo hücrelerinin oluşturup (`<td>` öğeleri) her sütun için. Sonraki kullanılabilir satır veritabanından döngü gittiğiniz her zaman kullanılıyor `row` değişken (Bu bölümünde ayarladığınız `foreach` deyimi). Tek bir sütun satırdaki almak için kullanabileceğiniz `row.Name` veya `row.Description` veya ne olursa olsun sütunu, adıdır.
4. Bir tarayıcıda. Sayfayı çalıştırın. (Emin olun sayfa seçildiğinde, **dosyaları** çalıştırmadan önce onu çalışma.) Sayfa aşağıdakine benzer bir listesi görüntülenir:

    ![[Görüntü]](5-working-with-data/_static/image4.jpg)

> [!TIP] 
> 
> **Yapılandırılmış sorgu dili (SQL)**
> 
> SQL, çoğu ilişkisel veritabanları veritabanındaki verileri yönetmek için kullanılan bir dildir. Verileri almak ve güncelleştirmek sağlar ve oluşturma, değiştirme ve veritabanı tablolarını yönetmenize imkan sağlayan komutlar içerir. SQL bir programlama dili (gibi Webmatrix'te kullanmakta olduğunuz bir) farklı SQL ile istediğiniz veritabanını söyleyin ve veri almak veya görevi gerçekleştirmek nasıl ekleneceğini veritabanının iş olduğu fikir olduğundan. Bazı SQL komutlarını örnekleri ve ne yaptıklarını şunlardır:
> 
> `SELECT Id, Name, Price FROM Product WHERE Price > 10.00 ORDER BY Name`
> 
> Bu getirir *kimliği*, *adı*, ve *fiyat* kayıtlarında sütunlarından *ürün* , tablo değerini *fiyat* birden fazla 10'dur ve değerlerine dayalı alfabetik sırada sonuçları döndürür *adı* sütun. Bu komut, hiçbir kayıt eşleşiyorsa karşılayan ölçüt ya da boş kayıtlarını içeren bir sonuç kümesi döndürür.
> 
> `INSERT INTO Product (Name, Description, Price) VALUES ("Croissant", "A flaky delight", 1.99)`
> 
> Bu yeni bir kayıt ekler *ürün* ayarı tablo *adı* sütuna &quot;Croissant&quot;, *açıklama* sütunu&quot; Anormal zevk aldığı&quot;ve 1.99 için fiyat.
> 
> `DELETE FROM Product WHERE ExpirationDate < "01/01/2008"`
> 
> Bu komut, kayıtları siler *ürün* , sona erme tarihi sütun 1 Ocak 2008'den önceki tablo. (Bu varsayar *ürün* tablolu böyle bir sütunu Elbette.) Tarihi GG/AA/YYYY biçiminde buraya girilen ancak bölgeniz için kullanılan biçimde girilmelidir.
> 
> `Insert Into` Ve `Delete` komutları sonuç kümeleri dönüş yok. Bunun yerine, bunlar kaç kayıt komutu tarafından etkilendiğini belirten bir sayı döndürür.
> 
> Bu işlemler (örneğin, ekleme ve kayıtları silme) bazıları için veritabanında uygun izinlere sahip olması işlemi isteyen işlem vardır. Bu, genellikle veritabanına bağlanan bir kullanıcı adı ve parola sağlamak zorunda üretim veritabanları için neden olur.
> 
> SQL komutlarını düzinelerce vardır, ancak bunların tümü böyle bir yol izler. Veritabanı tabloları oluşturmak, bir tablodaki kayıtların sayısı, fiyatlarını hesaplamak ve çok daha fazla işlemleri gerçekleştirmek için SQL komutlarını kullanabilirsiniz.


## <a name="inserting-data-in-a-database"></a>Bir veritabanında veri ekleme

Bu bölüm, kullanıcı için yeni bir ürün Ekle olanak sağlayan bir sayfa oluşturmak gösterilmiştir *ürün* veritabanı tablosu. Yeni ürün kaydı eklendikten sonra güncelleştirilmiş tablosunu kullanarak sayfasını görüntüler *ListProducts.cshtml* önceki bölümde oluşturduğunuz sayfası.

Sayfa kullanıcının girdiği verileri veritabanı için geçerli olduğundan emin olmak için doğrulama içerir. Örneğin, sayfa kodunda bir değer için gerekli olan tüm sütunlar girdiğinizden emin emin olur.

1. Web sitesi adlı yeni bir CSHTML dosyası oluşturma *InsertProducts.cshtml*.
2. Varolan biçimlendirme aşağıdakiyle değiştirin:

    [!code-cshtml[Main](5-working-with-data/samples/sample3.cshtml)]

    Ad, açıklama ve fiyat girin, kullanıcıların izin üç metin kutusu ile bir HTML formuna sayfa gövdesi içerir. Kullanıcılar'ı tıklattığınızda **Ekle** düğmesi, sayfanın üst kısmındaki kod açar bağlantı *SmallBakery.sdf* veritabanı. Ardından kullanıcı kullanılarak gönderilen değerleri almak `Request` nesne ve yerel değişkenlere değerler atayın.

    Kullanıcı gerekli her sütun için bir değer girdiğinizde doğrulamak için her kayıt `<input>` doğrulamak istediğiniz öğe:

    [!code-csharp[Main](5-working-with-data/samples/sample4.cs)]

    `Validation` Yardımcı var olup olmadığını denetler bir değer kayıtlı alanların her birinde. Tüm alanlar denetleyerek doğrulamayı geçen olup olmadığını sınayabilirsiniz `Validation.IsValid()`, hangi, genellikle kullanıcıdan size bilgi işlem önce yapın:

    [!code-csharp[Main](5-working-with-data/samples/sample5.cs)]

    ( `&&` İşleci anlamına gelir ve — bu test *bu form gönderme ve tüm alanları doğrulama başarılı olursa*.)

    Tüm sütunları doğrulanacak (hiçbiri boş), devam edin ve veri eklemek ve ardından sonraki gösterildiği gibi yürütmek için bir SQL deyimi oluşturun:

    [!code-csharp[Main](5-working-with-data/samples/sample6.cs)]

    Değerler eklemek parametre yer tutucuları içerir (`@0`, `@1`, `@2`).

    > [!NOTE]
    > Önceki örnekte gördüğünüz gibi bir güvenlik önlemi olarak parametrelerini kullanarak bir SQL deyimi için her zaman değerlerini geçirin. Bu, kullanıcının verileri doğrulamak için bir fırsat sunar, artı (bazen SQL ekleme saldırıları adlandırılır) veritabanınıza kötü amaçlı komut gönderme girişimlerine karşı korunmasına yardımcı olur.

    Sorguyu yürütmek için bu bildirimi için yer tutucular yerine değerleri içeren değişkenler geçirme kullanın:

    [!code-csharp[Main](5-working-with-data/samples/sample7.cs)]

    Sonra `Insert Into` deyimi yürütülen, kullanıcı bu satırını kullanarak ürünleri listeler sayfasına gönder:

    [!code-javascript[Main](5-working-with-data/samples/sample8.js)]

    Doğrulama başarısız oldu, INSERT atlayın. Bunun yerine, bir yardımcı birikmiş hata iletileri (varsa) görüntülemek için sayfanın vardır:

    [!code-cshtml[Main](5-working-with-data/samples/sample9.cshtml)]

    Biçimlendirme stili bloğunda adlı bir CSS sınıf tanımını içeren bildirim `.validation-summary-errors`. Varsayılan olarak kullanılan CSS sınıfı adıdır `<div>` tüm doğrulama hatalarını içeren öğe. Bu durumda, doğrulama özeti hataları kırmızı olarak görüntülenir ve buna kalın, ancak tanımlayabileceğiniz CSS sınıfı belirtir `.validation-summary-errors` herhangi istediğiniz biçimlendirme görüntülenecek sınıf.

### <a name="testing-the-insert-page"></a>INSERT sayfa test etme

1. Sayfasını bir tarayıcıda görüntülemek. Sayfasında aşağıdaki çizimde gösterilen benzeyen bir form görüntüler.

    ![[Görüntü]](5-working-with-data/_static/image5.jpg)
2. Tüm sütunlar için değerler girin, ancak siz bıraktığınızdan emin olun *fiyat* sütun boş.
3. **Ekle**'ye tıklayın. Sayfasında, aşağıdaki çizimde gösterildiği gibi bir hata iletisi görüntüler. (Yeni bir kayıt oluşturulur.)

    ![[Görüntü]](5-working-with-data/_static/image6.jpg)
4. Tamamen formu doldurun ve ardından **Ekle**. Bu süre, *ListProducts.cshtml* sayfası görüntülenir ve yeni kayıt gösterir.

## <a name="updating-data-in-a-database"></a>Veritabanındaki verileri güncelleştirme

Bir tabloya veri girildikten sonra güncelleştirmeniz gerekebilir. Bu yordam, önceki veri ekleme için oluşturduğunuz olanları benzer iki sayfalarının nasıl oluşturulacağını gösterir. İlk sayfa değiştirmek için kullanıcıların seçmesine izin veren ve ürünleri görüntüler. İkinci sayfa gerçekten düzenlemeleri yapın ve bunları kaydetmek kullanıcılar sağlar.

> [!NOTE] 
> 
> **Önemli** bir üretim Web sitesi, genellikle kimin verilerde değişiklik yapabilir izin kısıtlayın. Sitesinde görevleri gerçekleştirmek için Kullanıcıları yetkilendirmek için yol ve üyelik ayarlama hakkında bilgi için bkz: [güvenlik ekleme ve bir ASP.NET Web sayfaları Site üyeliği](https://go.microsoft.com/fwlink/?LinkId=202904).


1. Web sitesi adlı yeni bir CSHTML dosyası oluşturma *EditProducts.cshtml*.
2. Dosyanın mevcut biçimlendirmede aşağıdakiyle değiştirin:

    [!code-cshtml[Main](5-working-with-data/samples/sample10.cshtml)]

    Bu sayfayı arasındaki tek fark ve *ListProducts.cshtml* sayfasında daha önce bu sayfadaki HTML tablosu görüntüler ek bir sütun içeren bulunan bir **Düzenle** bağlantı. Bu bağlantıya tıkladığınızda, size gereken *UpdateProducts.cshtml* (sonraki oluşturacaksınız) sayfasını burada düzenleyebilirsiniz seçili kaydı.

    Ara oluşturan koda **Düzenle** bağlantı:

    [!code-cshtml[Main](5-working-with-data/samples/sample11.cshtml)]

    Bu, bir HTML oluşturur `<a>` öğesi, `href` özniteliğini dinamik olarak ayarlayın. `href` Özniteliği, kullanıcı bağlantıyı tıklattığında görüntülenecek sayfa belirtir. Ayrıca geçirir `Id` bağlantısına geçerli satır değeri. Sayfa çalıştığında, sayfa kaynağı bu gibi bağlantılar içerebilir:

    [!code-html[Main](5-working-with-data/samples/sample12.html)]

    Dikkat `href` özniteliği `UpdateProducts/n`, burada *n* bir ürün sayıdır. Bir kullanıcı bu bağlantılardan birini tıkladığında, sonuçta elde edilen URL şöyle görünür:

    `http://localhost:18816/UpdateProducts/6`

    Diğer bir deyişle, düzenlenecek ürün numarası URL'de geçirilir.
3. Sayfasını bir tarayıcıda görüntülemek. Sayfa verileri şuna benzer bir biçimde görüntüler:

    ![[Görüntü]](5-working-with-data/_static/image7.jpg)

    Ardından, gerçekte verileri güncelleştirmek kullanıcıların sağlayan sayfanın oluşturacaksınız. Güncelleştirme sayfasını kullanıcının girdiği verileri doğrulamak için doğrulama içerir. Örneğin, sayfa kodunda bir değer için gerekli olan tüm sütunlar girdiğinizden emin emin olur.
4. Web sitesi adlı yeni bir CSHTML dosyası oluşturma *UpdateProducts.cshtml*.
5. Dosyanın mevcut biçimlendirmede aşağıdakiyle değiştirin.

    [!code-cshtml[Main](5-working-with-data/samples/sample13.cshtml)]

    Sayfa gövdesi bir ürün görüntülendiği ve kullanıcılar, düzenleyebileceğiniz bir HTML formuna içerir. Görüntülenecek ürün almak için bu SQL deyimini kullanın:

    [!code-sql[Main](5-working-with-data/samples/sample14.sql)]

    Bu kimliği geçirilen değeri ile eşleşen ürün seçecektir `@0` parametresi. (Çünkü *kimliği* birincil anahtar ve bu nedenle gereken benzersiz, yalnızca bir ürün kaydı bu şekilde şimdiye kadar seçilebilir.) Bunun için geçirmek için kimliği değeri almak için `Select` deyimi, sayfaya URL'SİNİN bir parçası aşağıdaki sözdizimi kullanılarak geçirilen değer okuyabilir:

    [!code-csharp[Main](5-working-with-data/samples/sample15.cs)]

    Ürün kaydı gerçekten getirmek için kullandığınız `QuerySingle` yalnızca bir kayıt döndürür yöntemi:

    [!code-csharp[Main](5-working-with-data/samples/sample16.cs)]

    Tek bir satır halinde döndürülen `row` değişkeni. Her sütun dışında veri almak ve bu gibi yerel değişkenleri atayabilirsiniz:

    [!code-csharp[Main](5-working-with-data/samples/sample17.cs)]

    Form için biçimlendirmede bu değerleri otomatik olarak tek tek metin kutularına aşağıdaki gibi katıştırılmış bir kod kullanarak görüntülenir:

    [!code-html[Main](5-working-with-data/samples/sample18.html)]

    Bu kod parçası, güncelleştirilmesi ürün kaydı görüntüler. Kayıt görüntülenen sonra kullanıcı sütunları tek tek düzenleyebilirsiniz.

    Kullanıcı formu teslim ettiğinde tıklayarak **güncelleştirme** button, kodda `if(IsPost)` engelleme çalıştırır. Bu kullanıcının değerleri alır `Request` nesnesi değişkenlerinin değerlerini saklar ve her bir sütunun doldurulmuş olduğunu doğrular. Doğrulama başarılı olursa, aşağıdaki SQL güncelleştirme deyimini kod oluşturur:

    [!code-sql[Main](5-working-with-data/samples/sample19.sql)]

    Bir SQL `Update` deyimi, güncelleştirme ve değerini ayarlamak için her bir sütunun belirtin. Bu kodda değerleri parametre yer tutucuları kullanılarak belirtilir `@0`, `@1`, `@2`ve benzeri. (Güvenlik için daha önce belirtildiği gibi her zaman değerleri bir SQL deyimi parametreleri kullanarak geçmelidir.)

    Çağırdığınızda `db.Execute` yöntemi, geçirdiğiniz sırayla SQL deyimindeki parametreleri karşılık gelen değerleri içeren değişkenleri:

    [!code-csharp[Main](5-working-with-data/samples/sample20.cs)]

    Sonra `Update` deyimi yürütüldü, kullanıcıyı Düzenle sayfasına yeniden yönlendirmek için aşağıdaki yöntemi çağırın:

    [!code-cshtml[Main](5-working-with-data/samples/sample21.cshtml)]

    Bir kullanıcı veritabanındaki verileri güncelleştirilmiş bir listesini görür ve başka bir ürün düzenleyebilirsiniz etkisidir.
6. Sayfayı kaydedin.
7. Çalıştırma *EditProducts.cshtml* sayfa (güncelleştirme sayfada değil) ve ardından **Düzenle** düzenlemek için bir ürün seçmek için. *UpdateProducts.cshtml* sayfası görüntülenirse, seçtiğiniz kaydı gösteriliyor.

    ![[Görüntü]](5-working-with-data/_static/image8.jpg)
8. Değişiklik yaparak tıklatın **güncelleştirme**. Ürün listesini yeniden güncelleştirilmiş verilerinizle gösterilir.

## <a name="deleting-data-in-a-database"></a>Veritabanındaki verileri silme

Bu bölümde, kullanıcıların bir üründen silme izin vermek gösterilmiştir *ürün* veritabanı tablosu. Örneğin iki sayfa oluşur. İlk sayfasında, kullanıcıların silmek için bir kayıt seçin. Silinecek kaydı daha sonra bunların kaydı silmek istediğinizi onaylamak olanak sağlayan bir ikinci sayfasında görüntülenir.

> [!NOTE] 
> 
> **Önemli** bir üretim Web sitesi, genellikle kimin verilerde değişiklik yapabilir izin kısıtlayın. Üyelik ayarlama hakkında ve sitesinde görevleri gerçekleştirmek için kullanıcıya yetki yolları hakkında bilgi için bkz: [güvenlik ekleme ve bir ASP.NET Web sayfaları Site üyeliği](https://go.microsoft.com/fwlink/?LinkId=202904).


1. Web sitesi adlı yeni bir CSHTML dosyası oluşturma *ListProductsForDelete.cshtml*.
2. Varolan biçimlendirme aşağıdakiyle değiştirin:

    [!code-cshtml[Main](5-working-with-data/samples/sample22.cshtml)]

    Bu sayfayı benzer *EditProducts.cshtml* sayfasından daha önce. Ancak, görüntüleme yerine bir **Düzenle** bağlantı her ürün için görüntülediği bir **silmek** bağlantı. **Silmek** bağlantı biçimlendirmede aşağıdaki katıştırılmış kod kullanılarak oluşturulur:

    [!code-cshtml[Main](5-working-with-data/samples/sample23.cshtml)]

    Bu, kullanıcılar bağlantıyı tıklattığında şuna benzer bir URL oluşturur:

    `http://<server>/DeleteProduct/4`

    Adlı bir sayfaya URL çağırır *DeleteProduct.cshtml* (hangi sonraki oluşturacaksınız) ve silmek için ürün Kimliğini geçirir (burada, 4).
3. Dosyayı kaydedin, ancak açık bırakın.
4. Adlı başka bir CHTML dosyası oluşturun *DeleteProduct.cshtml*. Varolan içeriği aşağıdakiyle değiştirin:

    [!code-cshtml[Main](5-working-with-data/samples/sample24.cshtml)]

    Bu sayfa tarafından çağrılır *ListProductsForDelete.cshtml* ve kullanıcıların bir ürünü silmek istediğinizi onaylamak olanak sağlar. Silinecek ürün listelemek için aşağıdaki kodu kullanarak URL'den silmek için ürün Kimliğini alın:

    [!code-csharp[Main](5-working-with-data/samples/sample25.cs)]

    Sayfa sonra gerçekte kaydını silmek için bir düğmeye kullanıcıya sorar. Bu önemli bir güvenlik önlemidir: Web sitenizi güncelleştirmek veya verileri silme gibi hassas işlemler gerçekleştirdiğinizde bu işlemlerin her zaman yapılan GET işlemi bir POST işlemini kullanarak. Bir silme işlemi alma işlemi kullanılarak gerçekleştirilebilir. böylece, sitenizi ayarlanıp ayarlanmadığını herhangi bir URL gibi geçirebilirsiniz `http://<server>/DeleteProduct/4` ve istedikleri herhangi bir şey veritabanından silebilirsiniz. Onay ekleme ve silme yalnızca bir POST kullanılarak gerçekleştirilebilir. böylece sayfa kodlama, sitenize güvenlik ölçüsü ekleyin.

    Gerçek silme işlemi, ilk onaylar post işlemine budur ve kimliği boş değilse aşağıdaki kodu kullanarak gerçekleştirilir:

    [!code-csharp[Main](5-working-with-data/samples/sample26.cs)]

    Kodu belirtilen kaydını siler ve sonra kullanıcı geri listeleme sayfasına yönlendiren bir SQL deyimini çalıştırır.
5. Çalıştırma *ListProductsForDelete.cshtml* bir tarayıcıda.

    ![[Görüntü]](5-working-with-data/_static/image9.jpg)
6. Tıklatın **silme** bir ürün için bağlantı. *DeleteProduct.cshtml* sayfası, bu kayıt silmek istediğinizi onaylamak için görüntülenir.
7. Tıklatın **silmek** düğmesi. Ürün kaydı silinir ve sayfa güncelleştirilen ürün listeyle yenilenir.

> [!TIP] 
> 
> <a id="SB_ConnectingToADatabase"></a>
> ### <a name="connecting-to-a-database"></a>Bir veritabanına bağlanma
> 
> Bir veritabanına iki yolla bağlanabilir. İlk kullanmaktır `Database.Open` yöntemi ve veritabanı dosyasının adını belirtmek için (daha az *.sdf* uzantısı):
> 
> `var db = Database.Open("SmallBakery");`
> 
> `Open` Yöntemi varsayar. *sdf* dosyasıdır Web sitesinin içinde *uygulama\_veri* klasör. Bu klasör, özellikle verileri tutmak için tasarlanmıştır. Örneğin, veri okumak veya yazmak için Web sitesine izin vermek için uygun izinlere sahip ve bir güvenlik önlemi olarak WebMatrix erişim dosyaları bu klasörden izin vermiyor.
> 
> İkinci yol, bir bağlantı dizesi kullanmaktır. Bir bağlantı dizesi, bir veritabanına bağlanma hakkında bilgi içerir. Bu, bir dosya yolu içerebilir veya bir yerel veya uzak sunucu bir kullanıcı adı ve parola, sunucuya bağlanmak için bir SQL Server veritabanı adını içerebilir. (Veri merkezi olarak yönetilen bir SQL Server sürümünde tutarsanız, gibi bir barındırma sağlayıcısının sitesinde, her zaman bir bağlantı dizesi veritabanı bağlantısı bilgilerini belirtmek için kullanın.)
> 
> Webmatrix'te, bağlantı dizeleri genellikle adlı bir XML dosyasında depolanır *Web.config*. Adından da anlaşılacağı gibi kullanabileceğiniz bir *Web.config* sitenizi gerektirebilecek herhangi bir bağlantı dizesi dahil olmak üzere sitenin yapılandırma bilgilerini depolamak için Web sitenizin kök dosyasında. Bir bağlantı dizesi örneği bir *Web.config* dosya şu şekilde görünebilir:
> 
> [!code-xml[Main](5-working-with-data/samples/sample27.xml)]
> 
> Herhangi bir yerde bir sunucu üzerinde çalışan SQL Server örneği bir veritabanında örnekte, bağlantı dizesi işaret (yerel aksine *.sdf* dosyası). İçin uygun adlar yerine gerek `myServer` ve `myDatabase`ve SQL Server oturum açma değerlerini belirtin `username` ve `password`. (Kullanıcı adı ve parola değerleri mutlaka aynı Windows kimlik bilgilerinizi veya kendi sunucularına oturum açma için verdiği barındırma sağlayıcınız değerleri olarak değildir. Gereksinim duyduğunuz değerleri tam yöneticisine bakın.)
> 
> `Database.Open` Ya da bir veritabanı adı geçirmenize izin verdiğinden yöntemi esnek, *.sdf* dosya veya depolanan bir bağlantı dizesi adını *Web.config* dosya. Aşağıdaki örnek, önceki örnekte gösterilen bağlantı dizesi kullanılarak veritabanına bağlanmak gösterilmektedir:
> 
> [!code-cshtml[Main](5-working-with-data/samples/sample28.cshtml)]
> 
> Belirtildiği gibi `Database.Open` yöntemi, bir veritabanı adı veya bir bağlantı dizesi geçirmenize olanak sağlar ve kullanmak hangi şekil. Dağıttığınız bu oldukça yararlıdır (yayımladığınızda), Web sitesi. Kullanabileceğiniz bir *.sdf* dosyasını *uygulama\_veri* geliştirirken ve sitenizi test ederken klasör. Sitenizi üretim sunucusuna taşıdığınızda, bağlantı dizesinde kullanabilirsiniz *Web.config* aynı ada sahip dosya, *.sdf* barındırma sağlayıcısının noktalarına &#8212;tüm kodunuzu değiştirmek zorunda kalmadan.
> 
> Son olarak, doğrudan bir bağlantı dizesi ile çalışmak isterseniz, çağırabilirsiniz `Database.OpenConnectionString` yöntemi ve gerçek bağlantı dizesi yalnızca birinde adı yerine geçişi *Web.config* dosya. Bu herhangi bir nedenden dolayı yok erişiminiz bağlantı dizesine durumlarda kullanışlı olabilir (veya içinde gibi değerler *.sdf* dosya adı) sayfa çalışıncaya kadar. Bununla birlikte, çoğu senaryo için kullanabileceğiniz `Database.Open` bu makalede anlatıldığı gibi.


## <a name="additional-resources"></a>Ek Kaynaklar

- [SQL Server Compact](https://www.microsoft.com/sqlserver/2008/en/us/compact.aspx)
- [SQL Server veya MySQL veritabanında WebMatrix bağlanma](https://go.microsoft.com/fwlink/?LinkId=208661)
- [ASP.NET Web Sayfaları Sitelerinde Kullanıcı Girişini Doğrulama](https://go.microsoft.com/fwlink/?LinkId=253002)
