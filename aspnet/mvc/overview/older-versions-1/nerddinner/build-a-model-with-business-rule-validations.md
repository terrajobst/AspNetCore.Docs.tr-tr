---
uid: mvc/overview/older-versions-1/nerddinner/build-a-model-with-business-rule-validations
title: İş kuralı doğrulamaları ile Model oluşturma | Microsoft Docs
author: microsoft
description: 3. adım, biz hem sorgulamak için kullanma ve böylelikle veritabanı NerdDinner uygulamamız için güncelleştirme modeli oluşturma işlemi gösterilmektedir.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 0bc191b2-4311-479a-a83a-7f1b1c32e6fe
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/build-a-model-with-business-rule-validations
msc.type: authoredcontent
ms.openlocfilehash: b735c414c629b9ff6617dbf80782d57543306c00
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754774"
---
<a name="build-a-model-with-business-rule-validations"></a>İş kuralı doğrulamaları ile Model oluşturma
====================
tarafından [Microsoft](https://github.com/microsoft)

[PDF'yi indirin](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Adım 3 / ücretsiz budur ["NerdDinner" uygulaması Öğreticisi](introducing-the-nerddinner-tutorial.md) , Yürüyüşü nasıl küçük bir derleme, ancak tamamlandı, ASP.NET MVC 1 kullanarak web uygulaması aracılığıyla.
> 
> 3. adım, biz hem sorgulamak için kullanma ve böylelikle veritabanı NerdDinner uygulamamız için güncelleştirme modeli oluşturma işlemi gösterilmektedir.
> 
> ASP.NET MVC 3 kullanıyorsanız, takip ettiğiniz öneririz [MVC 3 ile çalışmaya başlama](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) veya [MVC müzik Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) öğreticiler.


## <a name="nerddinner-step-3-building-the-model"></a>NerdDinner adım 3: Model derleme

Bir model-view-controller çerçeve "modeli" terimi, doğrulama ve iş kuralları ile tümleştiren karşılık gelen etki alanı mantığı yanı sıra, uygulama verilerini temsil eden nesneleri anlamına gelir. Modeli birçok şekilde MVC tabanlı bir uygulamanın "Kalp" olan ve daha sonra temelde anlatıldığı gibi davranışını sürücüler.

Tüm veri erişim teknolojisi kullanarak ASP.NET MVC çerçevesi destekler ve geliştiricilerin çeşitli dahil olmak üzere modellerini uygulamak için zengin .NET veri seçenekleri arasından seçim yapabilir: LINQ to Entities, LINQ to SQL, NHibernate, LLBLGen Pro, SubSonic, WilsonORM veya yalnızca ham ADO. NET DataReaders veya veri kümeleri.

NerdDinner uygulamamız için oldukça yakından bizim için veritabanı tasarımı karşılık gelir ve bazı özel doğrulama mantığı ve iş kurallarını ekler ve basit bir model oluşturmak için LINQ to SQL kullanmak için kullanacağız. Soyut hemen yardımcı olan bir depo sınıfına sonra rest bizim için kolayca birim testi etkinleştirir ve uygulama veri kalıcılığı uygulamasından uygular.

### <a name="linq-to-sql"></a>LINQ - SQL

LINQ to SQL .NET 3.5 bir parçası olarak gönderilen bir ORM (nesne ilişkisel eşleyicidir) olur.

LINQ to SQL veritabanı tablolarına karşı kodlayabilirsiniz .NET sınıfları eşlemek için kolay bir yol sağlar. NerdDinner uygulamamız için veritabanımızdaki Akşam Yemeği ve RSVP sınıflarına içinde azalma ve RSVP tabloları eşleştirmek için kullanacağız. Azalma ve RSVP tablo sütunlarını Akşam Yemeği ve RSVP sınıflarda özelliklerine karşılık gelir. Her Akşam Yemeği ve RSVP nesne veritabanında azalma veya RSVP tabloları içinde ayrı bir satırda temsil eder.

LINQ to SQL aktarımlardaki bize el ile alma ve güncelleştirme Akşam Yemeği ve RSVP SQL deyimlerini oluşturmak zorunda kalmamak veritabanı veri nesneleriyle. Bunun yerine, Akşam Yemeği ve RSVP sınıfları tanımlarız bunların eşleneceğine nasıl / veritabanı ve aralarındaki ilişkiler. LINQ to SQL ardından etkileşim ve bunları çalışma zamanında kullanmak için uygun SQL yürütme mantığı oluşturma bakım sürer.

VB ve C# içinde LINQ dil desteği Akşam Yemeği ve RSVP almak ifadesel sorgular yazmak için kullanabiliriz nesnelerini veritabanından. Bu veri kod yazmak için ihtiyacımız miktarını en aza indirir ve çok temiz uygulamalar oluşturmanıza olanak tanır.

### <a name="adding-linq-to-sql-classes-to-our-project"></a>Ekleme LINQ to SQL sınıfları Projemizin için

Biz Projemizin içinde "Modelleri" klasörüne sağ tıklayarak başlayın ve seçin **Add -&gt;yeni öğe** menü komutu:

![](build-a-model-with-business-rule-validations/_static/image1.png)

Bu, "Add New Item" iletişim kutusu getirir. Biz "Veri" kategoriye göre filtreleme ve bunun içinde "SQL sınıfları için LINQ" şablonu seçin:

![](build-a-model-with-business-rule-validations/_static/image2.png)

Biz "NerdDinner" öğe adı ve "Ekle" düğmesine tıklayın. Visual Studio bizim \Models dizini altında NerdDinner.dbml dosya ekleyin ve ardından LINQ to SQL Nesne İlişkisel Tasarımcısı açın:

![](build-a-model-with-business-rule-validations/_static/image3.png)

### <a name="creating-data-model-classes-with-linq-to-sql"></a>LINQ to SQL ile veri modeli sınıfları oluşturma

LINQ to SQL hızla var olan veritabanı şema veri modeli sınıfları oluşturma sağlıyor. Yapılacaklar bu biz NerdDinner veritabanı sunucu Gezgini'nde açın ve tabloları seçme içinde model istiyoruz:

![](build-a-model-with-business-rule-validations/_static/image4.png)

Ardından LINQ tabloları SQL Tasarımcı yüzeyine sürükleyebilirsiniz. Bu LINQ SQL zaman yaptığımız Dinner otomatik olarak oluşturur ve RSVP sınıfları (özellikleriyle tablolarını için veritabanı tablo sütunlarından eşleyen sınıfı) şemasını kullanarak:

![](build-a-model-with-business-rule-validations/_static/image5.png)

"Veritabanı şemasını temel alan sınıflar oluşturduğunda varsayılan LINQ to SQL Tasarımcı otomatik olarak tablo ve sütun adları çoğullaştırır". Örneğin: Yukarıdaki örneğimizde "Azalma" Tablo "Akşam Yemeği" sınıfında sonuçlandı. Bu sınıf adlandırma bizim modelleri .NET adlandırma kuralları ile tutarlı hale getirir ve genellikle bu Tasarımcı düzeltme bu kullanışlı ayarlama (özellikle de tabloların çok sayıda eklerken) sahip bulabilirim. Bir sınıf ya da her zaman geçersiz kılmak ve istediğiniz bir adla değiştirin olsa, oluşturduğu Tasarımcı, özellik adını beğenmezseniz. Varlık/özellik adı satır içini Tasarımcı düzenleyerek veya özellik kılavuzunda değiştirerek bunu yapabilirsiniz.

LINQ to SQL Tasarımcı ayrıca tabloların birincil anahtarı/yabancı anahtar ilişkileri inceler ve bunları temel alan otomatik olarak varsayılan olarak, varsayılan "ilişkisi ilişkilendirmeleri" arasında farklı model sınıfları oluşturur oluşturur. Bir yabancı anahtar tablosuna azalma RSVP tablonuz dayanarak gerçeğine Örneğin, ne zaman azalma Sürüklemeyi tercih ve RSVP tabloları LINQ to SQL Tasarımcı üzerine bir-çok ilişkisi ilişkilendirmesi ikisi arasındaki çıkarılan (Bu oku simgesiyle belirtilir Tasarımcı):

![](build-a-model-with-business-rule-validations/_static/image6.png)

Yukarıdaki ilişkilendirme LINQ geliştiriciler ile belirli bir RSVP ilişkili Akşam Yemeği erişmek için kullanabileceğiniz RSVP sınıfı türü kesin belirlenmiş bir "Akşam Yemeği" özelliği eklemek için SQL neden olur. Ayrıca, geliştiricilerin almak ve belirli bir Akşam Yemeği ile ilişkili RSVP nesnelerini güncelleştirme sağlayan "RSVPs" bir koleksiyon özelliği Dinner sınıfı neden olur.

Aşağıda yeni RSVP nesne oluşturma ve Dinner'ın LCV'ler koleksiyona eklemek IntelliSense Visual Studio içinden bir örneğini görebilirsiniz. Nasıl LINQ to SQL otomatik olarak "LCV'ler" koleksiyonu Şimdi Akşam nesnede eklenen dikkat edin:

![](build-a-model-with-business-rule-validations/_static/image7.png)

Akşam Yemeği'nın LCV'ler koleksiyonuna RSVP nesne ekleyerek biz LINQ Akşam Yemeği veritabanımızdaki RSVP satır arasında bir yabancı anahtar ilişkisi ilişkilendirilecek SQL söylemiş olursunuz:

![](build-a-model-with-business-rule-validations/_static/image8.png)

Tasarımcı nasıl veya modellenmiş adlı bir tablo ilişkisi beğenmezseniz geçersiz kılabilirsiniz. Yalnızca Tasarımcı içinde ilişkilendirme oku tıklatın ve özelliklerini yeniden adlandırma, silme veya değiştirmek için özellik kılavuzunu aracılığıyla erişin. NerdDinner uygulamamız için yine de oluşturmakta olduğunuz da veri modeli sınıfları için varsayılan ilişkilendirme kuralları çalışır ve yalnızca varsayılan davranışı kullanabiliriz.

### <a name="nerddinnerdatacontext-class"></a>NerdDinnerDataContext sınıfı

Visual Studio, modelleri ve LINQ to SQL Tasarımcı kullanılarak tanımlanan veritabanı ilişkileri temsil eden .NET sınıfları otomatik olarak oluşturur. Bir LINQ to SQL Datacontext'e sınıf ayrıca her bir LINQ to SQL Tasarımcı dosyası çözüme oluşturulur. Size sunduğumuz LINQ to SQL sınıf öğesi "NerdDinner" kullandığından, oluşturulan DataContext sınıf "NerdDinnerDataContext" olarak adlandırılır. Bu NerdDinnerDataContext sınıf biz veritabanıyla etkileşim birincil yoludur.

Bizim NerdDinnerDataContext sınıfı iki özellikleri - biz veritabanı içinde modellenmiş iki tabloları temsil eden "Azalma" ve "RSVPs" - gösterir. C# LINQ sorguları bu özellikleri veritabanından sorgu ve alma Akşam Yemeği ve RSVP nesnelerine yazmak için kullanabiliriz.

Aşağıdaki kod bir NerdDinnerDataContext nesnesinin örneğini oluşturma ve bir dizi gelecekte ortaya azalma almak üzere bir LINQ sorgusu gerçekleştirmek nasıl gösterir. LINQ Sorgu yazarken, Visual Studio IntelliSense sağlar ve ondan döndürülen nesneleri türü kesin olarak belirtilmiş ve IntelliSense de destekler:

![](build-a-model-with-business-rule-validations/_static/image9.png)

Akşam Yemeği ve RSVP nesneleri sorgulamak bize yayımlamanızı bir NerdDinnerDataContext üzerinden alıyoruz Dinner ve RSVP nesnelere biz sonradan yaptığınız tüm değişiklikler otomatik olarak izler. Değişiklikleri kolayca kaydetmek veritabanına - açık SQL güncelleştirme kod yazmaya gerek kalmadan bu işlevselliği kullanabiliriz.

Örneğin, aşağıdaki kod bir LINQ Sorgu tek bir Akşam Yemeği nesne veritabanından, iki Dinner özelliklerini güncelleştirin ve değişiklikleri veritabanına geri kaydedin için nasıl kullanılacağını göstermektedir:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample1.cs)]

NerdDinnerDataContext nesne üzerindeki kodda biz alınan Dinner nesneye özellik değişikliklerinin otomatik olarak izlenir. Biz "SubmitChanges()" yöntemi çağrıldığında, "Güncelleştir" uygun SQL deyimi veritabanına güncelleştirilmiş değerleri geri kalıcı hale getirmek için çalıştırın.

### <a name="creating-a-dinnerrepository-class"></a>DinnerRepository sınıfı oluşturma

Küçük uygulamalar için bazen bir LINQ to SQL Datacontext'e sınıfı karşı doğrudan çalışma ve LINQ sorguları içinde denetleyicileri ekleme denetleyicileri uygundur. Ancak, uygulamaları daha büyük alma gibi bu yaklaşım sağlamak ve test etmek için zahmetli hale gelir. Bize birden çok yerde aynı LINQ sorguları çoğaltma da neden olabilir.

Uygulamaları sağlamak ve test etmek daha kolay hale getirebilirsiniz bir yaklaşım, "depo" deseni kullanmaktır. Bir depo sınıfına veri sorgulama ve Kalıcılık mantığı ve hemen özetleri uygulamadan veri kalıcılığı uygulama ayrıntılarını kapsüllemek yardımcı olur. Uygulama kodu temizleme yapmak, ek bir depo deseni kullanarak veri depolama uygulamaları değiştirmeye kolaylaştırabilir ve birim gerçek bir veritabanı gerek kalmadan uygulama testi kolaylaştırmak yardımcı olabilir.

NerdDinner uygulamamız için biz DinnerRepository sınıfıyla tanımlarsınız imza aşağıda:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample2.cs)]

*Not: Bu bölümde daha sonra Biz bu sınıftan bir IDinnerRepository Arabirimi Ayıkla ve onunla bağımlılık ekleme bizim denetleyicilerinde etkinleştirin. Başlangıç olarak, yine de başlayın ve yalnızca doğrudan DinnerRepository sınıfı ile çalışmak için kullanacağız.*

Biz bizim "Modelleri" klasörü sağ tıklatın ve seçin, bu sınıf uygulamak için **Add -&gt;yeni öğe** menü komutu. "Yeni Öğe Ekle" iletişim kutusunun içinden biz "Class" şablonu seçin ve "DinnerRepository.cs" dosya adı:

![](build-a-model-with-business-rule-validations/_static/image10.png)

Biz, ardından aşağıdaki kodu kullanarak bizim DinnerRespository sınıfı uygulayabilirsiniz:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample3.cs)]

### <a name="retrieving-updating-inserting-and-deleting-using-the-dinnerrepository-class"></a>Alma, güncelleştirme, ekleme ve silme DinnerRepository sınıfını kullanma

Bizim DinnerRepository sınıfı oluşturduk, ortak görevler ile yapabileceğimiz gösteren birkaç kod örnekleri göz atalım:

#### <a name="querying-examples"></a>Örnekleri sorgulama

Aşağıdaki kod DinnerID değerini kullanarak tek bir Akşam Yemeği alır:


[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample4.cs)]

Aşağıdaki kod, tüm gelecek azalma ve döngüler üzerlerine alır:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample5.cs)]

#### <a name="insert-and-update-examples"></a>INSERT ve Update örnekleri

Aşağıdaki kod, iki yeni azalma eklemeyi gösterir. "Save()" yöntemi çağrılana kadar depoya ekleme/değişiklik veritabanına taahhüt değildir. LINQ to SQL otomatik olarak tüm değişiklikleri bir veritabanı işlemindeki – tüm değişiklikleri ortaya ya da depomuzda kaydettiğinde bunların hiçbiri yapmak sarmalar:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample6.cs)]

Aşağıdaki kod Şimdi Akşam nesnelerinden alır ve iki özelliklerde değiştirir. Depomuzda "Save()" yöntemi çağrıldığında, değişiklikler veritabanına uygulanır:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample7.cs)]

Aşağıdaki kod, bir Akşam Yemeği alır ve bir RSVP ekler. (Olmadığı için birincil-anahtar/yabancı anahtar ilişkisi iki veritabanı arasında), LINQ to SQL bizim için oluşturulan Dinner nesnesindeki LCV'ler koleksiyonunu kullanarak bunu yapar. Havuzda "Save()" yöntem çağrıldığında bu değişiklik yeni RSVP tabloda satır veritabanına geri kalıcı hale getirilir:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample8.cs)]

#### <a name="delete-example"></a>Örnek delete

Aşağıdaki kod, Şimdi Akşam nesnelerinden alır ve sonra silinebilir işaretler. Havuzda "Save()" yöntemi çağrıldığında veritabanına geri silme işlemeniz:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample9.cs)]

### <a name="integrating-validation-and-business-rule-logic-with-model-classes"></a>Doğrulama ve iş kuralı mantığı ile Model sınıfları tümleştirme

Doğrulama ve iş kuralı mantığını verileriyle çalışan herhangi bir uygulamanın önemli bir parçasıdır tümleştirme.

#### <a name="schema-validation"></a>Şema doğrulaması

LINQ to SQL Tasarımcı kullanarak model sınıfları tanımlandığında, veri modeli sınıfları özelliklerin türleri veritabanı tablosu veri türleri için karşılık gelir. Örneğin: "EventDate" tablosundaki azalma "tarih" ise, LINQ to SQL tarafından oluşturulan veri modeli sınıfı türü "(yerleşik bir .NET veri türü olan) DateTime" olacaktır. Başka bir deyişle, bir tamsayı veya boolean koddan atayın dener ve örtük olarak geçerli olmayan bir dize türü için çalışma zamanında dönüştürmeye çalışırsanız, hata otomatik olarak oluşturur, derleme hatası alırsınız.

LINQ to SQL da otomatik olarak işler escaping SQL değerleri sizin için dizeleri - korunmasına yardımcı olan, SQL ekleme saldırılarına karşı kullanırken kullanırken.

#### <a name="validation-and-business-rule-logic"></a>Doğrulama ve iş kural mantığı

Şema doğrulaması ilk adım olarak kullanışlıdır ancak nadiren yeterlidir. Çoğu gerçek dünya senaryoları birden çok özellik span, kod yürütmek ve genellikle bir modelin durumunun tanıma sahip daha zengin Doğrulama mantığı belirtme olanağı gerektirir (örnek:, / güncelleştirilen/silinen, veya bir etki alanına özel durumu oluşturuluyor "arşivlenen gibi"). Çok sayıda farklı desenleri ve tanımlamak ve model sınıflarına doğrulama kuralları uygulamak için kullanılan çerçeve mevcuttur ve bu konuda yardımcı olmak için kullanılan birkaç temel .NET Framework burada vardır. Neredeyse herhangi biri ASP.NET MVC uygulamaları içinde kullanabilirsiniz.

NerdDinner uygulamamızın amacı doğrultusunda, görece basit ve rahatça desen burada IsValid özelliği ve GetRuleViolations() yöntemi bizim Dinner model nesnesi üzerinde kullanıma sunuyoruz kullanacağız. IsValid özelliği true veya false doğrulama ve iş kuralları geçerli olup olmadığına bağlı olarak döndürür. GetRuleViolations() yöntemi herhangi bir kural hatalarının listesini döndürür.

Biz IsValid ve GetRuleViolations() Dinner modelimiz bizim projeye "kısmi class" ekleyerek uygulayacaksınız. Kısmi sınıflar, yöntemler/özellikler/olaylar (örneğin, LINQ to SQL tasarımcı tarafından oluşturulan Dinner sınıfı) bir VS tasarımcısı tarafından tutulan sınıfları eklemek için kullanılabilir ve kodumuzla uðraþmaya gelen araç önlemeye yardımcı olmak. Biz \Models klasöre sağ tıklayarak yeni bir kısmi sınıf bizim projesine ekleyin ve ardından "Yeni Öğe Ekle" menü komutunu seçin. Biz daha sonra "Yeni Öğe Ekle" iletişim kutusunda "Sınıfı" şablonunu seçin ve Dinner.cs adlandırın.

![](build-a-model-with-business-rule-validations/_static/image11.png)

"Ekle" düğmesine tıklayarak sunduğumuz projesine Dinner.cs dosyası ekleme ve IDE içinde açın. Biz bir temel kural/doğrulama zorlama framework kullanarak ardından uygulayabilirsiniz kod aşağıda:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample10.cs)]

Yukarıdaki kodla ilgili birkaç Not:

- Akşam Yemeği sınıfı ile içerdiği kod LINQ to SQL tasarımcı tarafından oluşturulan ve tutulan sınıfıyla birleşik ve tek bir sınıfı derlenmiş anlamına gelir. bir "kısmi" anahtar sözcüğünü – başlar.
- Yardımcı bir sınıf kurmamızı kuralı ihlali hakkında daha fazla ayrıntı sağlamak proje ekleyeceğiz RuleViolation sınıftır.
- Değerlendirilecek bizim doğrulama ve iş kuralları Dinner.GetRuleViolations() yöntemi neden olur (biz bunları kısa bir süre içinde uygulama). Daha sonra geri kural hatalar hakkında daha fazla ayrıntı sağlayan RuleViolation nesneleri dizisi döndürür.
- Dinner.IsValid özelliği Şimdi Akşam nesne herhangi bir etkin RuleViolations olup olmadığını belirten bir kullanışlı yardımcı özelliği sağlar. Proaktif olarak Şimdi Akşam nesnesi dilediğiniz zaman kullanılarak bir geliştirici tarafından denetlenebilir (ve bir özel durum oluşturmaz).
- Veritabanı içinde yaklaşık sürdürülecek Dinner nesnedir herhangi bir zamanda size bildirilmesini sağlar bir LINQ to SQL sağlayan bir kanca Dinner.OnValidate() kısmi yöntem budur. OnValidate() kararlılığımızın yukarıdaki, kaydedilmeden önce Dinner hiçbir RuleViolations sahip olmasını sağlar. Geçersiz bir durumda ise LINQ SQL işlem iptal için neden olacak bir özel durum oluşturur.

Bu yaklaşım, biz doğrulama ve iş kuralları ile tümleştirebileceğiniz basit bir çerçeve sağlar. Şimdilik ekleyelim bizim GetRuleViolations() yöntemi kuralları aşağıda:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample11.cs)]

Tüm RuleViolations bir dizisini döndürmek için C# "yield return" özelliği kullanıyoruz. Yukarıdaki ilk altı kural denetimleri, yalnızca bizim Dinner dize özellikleri null veya boş olamaz uygular. Son kural biraz daha ilginçtir ve çağrıları biz doğrulamak için Projemizin ContactPhone ekleyebilirsiniz PhoneValidator.IsValidNumber() yardımcı yöntem sayı biçimi eşleşme Akşam Yemeği'nın ülke.

Kullanabiliriz. Bu telefon doğrulama desteği uygulamak için NET normal ifade desteği. Ekleyebiliriz basit bir PhoneValidator uygulama aşağıdadır Ülkeye özel normal ifade deseni denetimleri Ekle sağlıyor Projemizin için:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample12.cs)]

#### <a name="handling-validation-and-business-logic-violations"></a>Doğrulama ve iş mantığı ihlalleri işleme

Yukarıdaki doğrulama ve iş kuralı kod, oluşturulacak veya güncelleştirilecek bir Akşam Yemeği Çalıştığımızda dilediğiniz zaman ekledik, bizim Doğrulama mantığı kuralları değerlendirilir ve zorunlu tutulur.

Geliştiriciler gibi aşağıda proaktif olarak Dinner nesne geçerli olup olmadığını belirlemek için kod ve içindeki tüm ihlalleri herhangi bir özel durumlarını oluşturma olmadan listesini almak yazabilirsiniz:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample13.cs)]

Biz, geçersiz bir durumda bir Akşam Yemeği kaydetmeyi denerse, biz üzerinde DinnerRepository Save() yöntemi çağırdığınızda bir özel durum gerçekleştirilecektir. Bu durum, LINQ to SQL otomatik olarak Akşam Yemeği'nın değişiklikleri kaydeder ve Dinner.OnValidate() Akşam Yemeği içinde herhangi bir kural ihlallerini mevcut bir özel durum oluşturmak için kod ekledik önce bizim Dinner.OnValidate() kısmi yöntem çağrıları kaynaklanır. Bu özel durumu yakalar ve öngörülebiliyorsa ihlallerini düzeltmek için bir listesini alın:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample14.cs)]

Bizim doğrulama ve iş kuralları UI yerleşiminde içinde değil ve bizim modeli katmanı içinde uygulanan olduğundan, bunlar uygulanan ve uygulamamızı içindeki tüm senaryolarında kullanılan. Daha sonra değiştirmek veya iş kuralları ekleyebilir ve bizim Dinner nesneleri ile çalışır bunları dikkate tüm koda sahip.

İş kuralları tek bir yerde değiştirme esnekliğine sahip olmak, bu değişiklikler uygulama ve UI mantığı ripple zorunda kalmadan bir iyi yazılmış uygulama ve MVC çerçevesi teşvik yardımcı olur. bir avantajı işaretidir.

### <a name="next-step"></a>Sonraki adım

Artık sorgu hem bizim veritabanını güncellemek için kullanabileceğiniz bir model sorunumuz.

Şimdi bazı denetleyicileri ve görünümleri çevresinde bir HTML kullanıcı Arabirimi deneyimi oluşturmak için kullanabiliriz proje ekleyelim.

> [!div class="step-by-step"]
> [Önceki](create-a-database.md)
> [İleri](use-controllers-and-views-to-implement-a-listingdetails-ui.md)
