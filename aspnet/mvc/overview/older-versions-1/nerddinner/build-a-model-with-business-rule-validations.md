---
uid: mvc/overview/older-versions-1/nerddinner/build-a-model-with-business-rule-validations
title: İş kuralı doğrulamaları ile bir Model oluşturma | Microsoft Docs
author: microsoft
description: Adım 3 biz için her iki sorgusunu kullanın ve böylelikle NerdDinner uygulamamız için veritabanını güncelleştirmek bir model oluşturmak nasıl gösterir.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 0bc191b2-4311-479a-a83a-7f1b1c32e6fe
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/build-a-model-with-business-rule-validations
msc.type: authoredcontent
ms.openlocfilehash: c5a482474fd2f41836f70952306ada5cd9136455
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="build-a-model-with-business-rule-validations"></a>İş kuralı doğrulamaları ile bir Model oluşturma
====================
tarafından [Microsoft](https://github.com/microsoft)

[PDF indirin](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Adım 3 / ücretsiz budur ["NerdDinner" uygulaması Öğreticisi](introducing-the-nerddinner-tutorial.md) , yetenekte küçük bir yapı ancak tamamlandı, ASP.NET MVC 1 kullanarak web uygulamasına nasıl aracılığıyla.
> 
> Adım 3 biz için her iki sorgusunu kullanın ve böylelikle NerdDinner uygulamamız için veritabanını güncelleştirmek bir model oluşturmak nasıl gösterir.
> 
> ASP.NET MVC 3 kullanıyorsanız, izlemeniz önerilir [MVC 3 ile çalışmaya başlama](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) veya [MVC müzik deposu](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) öğreticileri.


## <a name="nerddinner-step-3-building-the-model"></a>NerdDinner 3. adım: Model oluşturma

Bir model-view-controller framework "modeli" terimi, doğrulama ve iş kuralları ile tümleşir karşılık gelen etki alanı mantığı yanı sıra uygulama verilerini temsil eden nesneler anlamına gelir. Modeli birçok yolla MVC tabanlı uygulamanın "Kalp" olduğunu ve daha sonra temelde anlatıldığı gibi davranışını sürücüler.

Tüm veri erişim teknolojisini kullanarak ASP.NET MVC çerçevesi destekler ve geliştiriciler, çeşitli zengin .NET veri seçenekleri de dahil olmak üzere modellerini uygulamak için seçebilirsiniz: LINQ to Entities, LINQ to SQL, NHibernate, LLBLGen Pro, SubSonic, WilsonORM veya yalnızca ham ADO. NET DataReader veya veri kümeleri.

NerdDinner uygulamamız için biz LINQ-SQL oldukça yakından bizim veritabanı tasarımına karşılık gelir ve bazı özel doğrulama mantığının ve iş kuralları ekleyen basit bir model oluşturmak için kullanacağınız. Özet hemen yardımcı olan bir depo sınıfına sonra uygulama ve bize kolayca birim testi etkinleştirir kalan veri kalıcılığı uygulamasından gerçekleştireceksiniz.

### <a name="linq-to-sql"></a>LINQ - SQL

LINQ-SQL .NET 3.5 bir parçası olarak gönderilen bir ORM (nesne ilişkisel Eşleyici) olur.

LINQ-SQL veritabanı tabloları karşı kodlayabilirsiniz .NET sınıfları eşlemek için kolay bir yol sağlar. NerdDinner uygulamamız için bunu Veritabanımıza Yemeği ve RSVP sınıfları içinde azalma ve RSVP tabloları eşlemek için kullanacağız. Azalma ve RSVP tablo sütunlarını Yemeği ve RSVP sınıfları özellikleri karşılık gelir. Her nesne Yemeği ve RSVP veritabanındaki azalma veya RSVP tabloların içinde ayrı bir satır temsil eder.

LINQ-SQL el ile almak ve Yemeği ve RSVP güncelleştirmek için SQL deyimlerini oluşturmak zorunda kalmamak verir veritabanı verileri olan nesneler. Bunun yerine, biz Yemeği ve RSVP sınıfları tanımlarsınız bunlar Eşle nasıl / veritabanı ve aralarındaki ilişkiler. LINQ-SQL sonra etkileşim ve bunları çalışma zamanında kullanılacak uygun SQL yürütme mantığı oluşturmanın dikkatli sürer.

VB ve C# içinde LINQ dil desteği biz Yemeği ve RSVP almak açıklayıcı sorgular yazmak için kullanabileceğiniz nesnelerini veritabanından. Bu veri kod yazmak için ihtiyacımız miktarını azaltır ve bize gerçekten temiz uygulamalar oluşturmanızı sağlar.

### <a name="adding-linq-to-sql-classes-to-our-project"></a>LINQ SQL'e sınıflarını bizim projesine ekleniyor

Biz Projemizin içinde "Modeller" klasörü üzerinde sağ tıklayarak başlayın ve seçin **Ekle -&gt;yeni öğe** menü komutu:

![](build-a-model-with-business-rule-validations/_static/image1.png)

Bu, "Yeni Öğe Ekle" iletişim kutusunu getirir. Biz "Data" kategoriye göre filtre uygulamak ve içindeki "SQL sınıflar LINQ" şablonunu seçin:

![](build-a-model-with-business-rule-validations/_static/image2.png)

Biz "NerdDinner" öğe adı ve "Ekle" düğmesini tıklatın. Visual Studio bizim \Models dizini altında NerdDinner.dbml dosya ekleyin ve ardından LINQ to SQL Nesne İlişkisel Tasarımcısı açın:

![](build-a-model-with-business-rule-validations/_static/image3.png)

### <a name="creating-data-model-classes-with-linq-to-sql"></a>LINQ-SQL ile oluşturma veri modeli sınıfları

LINQ-SQL bize veri modeli sınıflarını varolan veritabanı şemadan hızlı bir şekilde oluşturmak etkinleştirir. Yapılacaklar bu biz NerdDinner veritabanı Server Explorer'da açın ve tabloları seçin içinde model istiyoruz:

![](build-a-model-with-business-rule-validations/_static/image4.png)

Biz sonra tabloları LINQ SQL Tasarımcı yüzeyine sürükleyin. Biz SQL bu LINQ yaptığınızda Yemeği otomatik olarak oluşturur ve RSVP sınıfları tablolarla (veritabanı tablo sütunlarına eşleme sınıf özelliklerini) şeması'nı kullanarak:

![](build-a-model-with-business-rule-validations/_static/image5.png)

"Veritabanı şemasını temel alan sınıfları oluşturduğunda, varsayılan LINQ-SQL Tasarımcısı otomatik olarak tablo ve sütun adlarını pluralizes". Örneğin: örneğimizde yukarıdaki "Azalma" Tablo "Yemeği" sınıfında sonuçlandı. Bu sınıf adlandırma bizim modelleri .NET adlandırma kuralları ile tutarlı hale getirir ve genellikle o Tasarımcı düzeltme bu kullanışlı Yukarı (özellikle çok sayıda tablo eklerken) sahip bulabilirim. Bir sınıf ya da her zaman geçersiz kılmak ve istediğiniz bir adla değiştirin olsa oluşturduğu Tasarımcı, özellik adı sevmiyorsanız. Bu varlık/özelliği adı satır içi Tasarımcısı'nda düzenleyerek veya özellik Kılavuzu değiştirerek yapabilirsiniz.

LINQ-SQL Tasarımcısı da tablonun birincil anahtarı/yabancı anahtar ilişkileri inceler ve bunlar üzerinde temel otomatik olarak varsayılan olarak varsayılan "ilişki ilişkilendirmeleri" oluşturduğu farklı model sınıflar arasında oluşturur. Bir yabancı anahtar azalma tabloya RSVP tabloda olduğunu dayanarak açık nedeni, örneğin, biz azalma sürüklenen ve RSVP tabloları LINQ-SQL Tasarımcısı üzerine bir-çok ilişkisi ilişkisi ikisi arasındaki zaman çıkarımı yapılan (Bu oku simgesiyle belirtilir Tasarımcı):

![](build-a-model-with-business-rule-validations/_static/image6.png)

Yukarıdaki ilişkilendirme LINQ geliştiriciler verilen RSVP ile ilişkili Yemeği erişmek için kullanabileceğiniz RSVP sınıfı kesin türü belirtilmiş bir "Yemeği" özelliği eklemek için SQL neden olur. Ayrıca, geliştiricilerin almak ve belirli bir Yemeği ile ilişkili RSVP nesneleri güncelleştirme olanak veren "RSVPs" bir koleksiyon özelliği Yemeği sınıfı neden olur.

Aşağıda yeni bir RSVP nesnesi oluşturma ve Yemeği 's LCV'ler koleksiyona eklemek, Visual Studio içinde IntelliSense örneği görebilirsiniz. Nasıl LINQ-SQL otomatik olarak bir "LCV'ler" koleksiyon Yemeği nesnesinde eklenen dikkat edin:

![](build-a-model-with-business-rule-validations/_static/image7.png)

Yemeği 's LCV'ler koleksiyonuna RSVP nesnesini ekleyerek biz LINQ-SQL Yemeği Veritabanımıza RSVP satırda arasında bir yabancı anahtar ilişkisi ilişkilendirmek için söylemiş olursunuz:

![](build-a-model-with-business-rule-validations/_static/image8.png)

Tasarımcı nasıl modellenir veya bir tablo ilişkisi adlı hoşlanmıyorsanız, geçersiz kılabilirsiniz. Yalnızca Tasarımcısı'nda ilişkilendirme oku tıklatın ve özelliklerini yeniden adlandırmak, silmek veya değiştirmek için özellik kılavuzunu aracılığıyla erişebilirsiniz. NerdDinner uygulamamız için yine de iyi veri modeli sınıflarını oluşturmakta olduğunuz varsayılan ilişkilendirme kuralları çalışır ve biz yalnızca varsayılan davranışı kullanabilirsiniz.

### <a name="nerddinnerdatacontext-class"></a>NerdDinnerDataContext Class

Visual Studio modelleri ve LINQ-SQL Tasarımcısı kullanılarak tanımlanan veritabanı ilişkiler temsil eden .NET sınıfları otomatik olarak oluşturur. Bir LINQ to SQL DataContext sınıfı, ayrıca her LINQ-SQL Tasarımcı dosya çözüme eklenmesi için oluşturulur. Biz bizim LINQ-SQL sınıfı öğesi "NerdDinner" adlı çünkü oluşturulan DataContext sınıfı "NerdDinnerDataContext" adı verilir. Bu NerdDinnerDataContext sınıfı veritabanı ile etkileşime gireceğini birincil yoludur.

Bizim NerdDinnerDataContext sınıfı iki özellikleri - biz veritabanı içinde Modellenen iki tablo temsil eden "Azalma" ve "RSVPs" - gösterir. Biz C# bu özellikleri LINQ sorguları sorgu ve alma Yemeği ve RSVP nesnelere veritabanından yazmak için kullanabilirsiniz.

Aşağıdaki kod, bir NerdDinnerDataContext nesnesi ve gelecekte meydana azalma dizisini almak üzere bir LINQ Sorgu gerçekleştirme gösterilmiştir. LINQ sorgu yazılırken Visual Studio IntelliSense sağlar ve ondan döndürülen nesnelerin kesin türü belirtilmiş ve IntelliSense de destekler:

![](build-a-model-with-business-rule-validations/_static/image9.png)

Bize Yemeği ve RSVP nesneler için sorgulamaya izin ek olarak, bir NerdDinnerDataContext üzerinden alıyoruz Yemeği ve RSVP nesnelere biz sonradan yaptığınız tüm değişiklikler de otomatik olarak izler. Kolayca değişiklikleri veritabanına - herhangi bir açık SQL güncelleştirme kod yazmak zorunda kalmadan kaydetmeye Biz bu işlevi kullanabilirsiniz.

Örneğin, aşağıdaki kodu bir LINQ Sorgu tek bir Yemeği nesne veritabanından, iki Yemeği özelliklerini güncelleştirmek ve ardından değişiklikleri veritabanına kaydetmek için nasıl kullanılacağını gösterir:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample1.cs)]

Yukarıdaki kodu NerdDinnerDataContext nesnesinde biz ondan alınan Yemeği nesnesine özelliği değişikliklerinin otomatik olarak izlenir. Biz "SubmitChanges()" yöntemi çağrıldığında, "Güncelleştir" uygun SQL deyimi geri güncelleştirilmiş değerleri kalıcı hale getirmek için veritabanına yürütülür.

### <a name="creating-a-dinnerrepository-class"></a>DinnerRepository sınıfı oluşturma

Küçük uygulamalar için bazen bir LINQ to SQL DataContext sınıfı karşı doğrudan çalışmak ve LINQ sorgularını denetleyicileri içinde embed denetleyicileri daha iyi olur. Ancak, uygulamaları daha büyük aldıkça, bu yaklaşım korumak ve test için sıkıcı olur. Ayrıca, bize aynı LINQ sorgularını birden çok yerde çoğaltma açabilir.

Uygulamaları korumak ve test etmek daha kolay hale getirebilirsiniz bir yaklaşım "repository" deseni kullanmaktır. Bir depo sınıfına veri sorgulama ve kalıcılığı mantığı ve hemen özetleri uygulamadan veri kalıcılığını uygulama ayrıntılarını kapsülleyen yardımcı olur. Uygulama kodu temizleyici yapmak, yanı sıra bir havuz deseni kullanarak veri depolama uygulamaları gelecekte değiştirmek kolaylaştırabilir ve birim gerçek bir veritabanı gerek kalmadan uygulama testi kolaylaştırmak yardımcı olabilir.

NerdDinner uygulamamız için biz DinnerRepository sınıfıyla tanımlarsınız imza altında:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample2.cs)]

*Not: Bu bölümde daha sonra Biz bu sınıftan bir IDinnerRepository arabirimi ayıklamak ve onunla bağımlılık ekleme bizim denetleyicilerinde etkinleştirin. Başından itibaren ancak biz basit başlatmak ve yalnızca doğrudan DinnerRepository sınıfıyla çalışmak için adımıdır.*

Bu sınıf biz bizim "Modelleri" klasörü sağ tıklatın ve seçin uygulamak için **Ekle -&gt;yeni öğe** menü komutu. "Yeni Öğe Ekle" iletişim biz "Sınıf" şablonunu seçin ve "DinnerRepository.cs" dosya adı:

![](build-a-model-with-business-rule-validations/_static/image10.png)

Biz, ardından aşağıdaki kodu kullanarak bizim DinnerRespository sınıfı uygulayabilirsiniz:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample3.cs)]

### <a name="retrieving-updating-inserting-and-deleting-using-the-dinnerrepository-class"></a>Alma, güncelleştirme, ekleme ve DinnerRepository sınıfı kullanarak silme

Bizim DinnerRepository sınıfı oluşturduk, ortak görevler ile yapabiliriz gösteren birkaç kod örnekleri bakalım:

#### <a name="querying-examples"></a>Örnekler sorgulama

Aşağıdaki kodu DinnerID değerini kullanarak tek bir Yemeği alır:


[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample4.cs)]

Aşağıdaki kod, tüm yaklaşan azalma ve döngüler üzerlerine alır:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample5.cs)]

#### <a name="insert-and-update-examples"></a>Ekleme ve güncelleştirme örnekleri

Aşağıdaki kodu, iki yeni azalma eklemeyi gösterir. "Save()" yöntemi, üzerinde çağrılıncaya kadar eklemeleri/değişiklikleri deponuza veritabanına işlendiği değil. LINQ-SQL otomatik olarak tüm değişiklikleri veritabanına işlemde – tüm değişiklikleri durum ya da bizim deposu kaydettiğinde hiçbirinin yapmak sarmalar:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample6.cs)]

Aşağıdaki kodu varolan Yemeği nesnesini alır ve bunu iki özelliklerini değiştirir. Bizim havuzda "Save()" yöntemi çağrıldığında değişiklikler veritabanına geri uygulanır:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample7.cs)]

Aşağıdaki kod bir Yemeği alır ve bir RSVP ekler. (Bir birincil-anahtar/yabancı anahtar ilişkisi iki veritabanı arasında) olduğundan LINQ-SQL bize için oluşturulan Yemeği nesnesinde LCV'ler koleksiyonunu kullanarak bunu yapar. Havuzda "Save()" yöntemi çağrıldığında bu değişiklik yeni RSVP tabloda satır olarak veritabanına kalıcı hale getirilir:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample8.cs)]

#### <a name="delete-example"></a>Örnek Sil

Aşağıdaki kod, varolan Yemeği nesnesini alır ve tarafından silinmek üzere işaretler. Havuzda "Save()" yöntemi çağrıldığında veritabanına geri silme yürütme:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample9.cs)]

### <a name="integrating-validation-and-business-rule-logic-with-model-classes"></a>Doğrulama ve iş kuralı mantığı modeli sınıfları ile tümleştirme

Doğrulama ve iş kuralı mantığı verileriyle çalışan herhangi bir uygulama, önemli bir parçasıdır tümleştirme.

#### <a name="schema-validation"></a>Şema doğrulaması

Model sınıfları LINQ-SQL Tasarımcısı kullanarak tanımlandığında, veri modeli sınıflarını özelliklerinde türleri veritabanı tablosu veri türleri için karşılık gelir. Örneğin: azalma tablonun "EventDate" sütununda "datetime" ise, LINQ to SQL tarafından oluşturulan veri modeli sınıfının türü "(Bu bir yerleşik .NET datatype) DateTime" olacaktır. Başka bir deyişle, bir tamsayı veya boolean koddan atamak dener ve örtük olarak geçerli olmayan bir dize türü çalışma zamanında dönüştürebilir çalışırsanız, bir hata otomatik olarak yükseltirsiniz, derleme hataları alırsınız.

LINQ-SQL da otomatik olarak işler kaçış SQL değerleri sizin yerinize korunmasına yardımcı olan, SQL ekleme saldırıları, kullanırken dizeleri - kullanırken.

#### <a name="validation-and-business-rule-logic"></a>Doğrulama ve iş kuralı mantığı

Şema doğrulaması ilk adım olarak yararlıdır, ancak nadiren yeterlidir. Çoğu gerçek dünya senaryoları, birden çok özellik span, kod yürütmek ve genellikle bir modelin durumu tanıma sahip daha zengin doğrulama mantığını belirtme olanağı gerektirir (örneğin: bunu/güncelleştirme/silindiğinde veya farklı bir etki alanına özel durumu içinde oluşturuluyor "arşivlenmiş gibi"). Çeşitli farklı desenleri ve tanımlamak ve model sınıflarına doğrulama kuralları uygulamak için kullanılan çerçeveler vardır ve bu konuda yardımcı olmak için kullanılan birkaç temel .NET Framework ölçeklendiriyor vardır. Neredeyse hiçbirini ASP.NET MVC uygulamaları içinde kullanabilirsiniz.

NerdDinner uygulamamız amaçları doğrultusunda, nispeten basit ve düz İleri düzeni burada size bir IsValid özelliğini ve GetRuleViolations() yöntemi bizim Yemeği model nesnesi kullanıma kullanacağız. IsValid özelliği true veya false doğrulama ve iş kuralları geçerli olup olmadığına bağlı olarak döndürür. GetRuleViolations() yöntemi kural hataları listesini döndürür.

Biz IsValid ve GetRuleViolations() Yemeği modelimiz için bizim projeye "kısmi sınıf" ekleyerek uygulaması. Kısmi sınıflar özellikleri/yöntemleri/olaylar VS Tasarımcısı (ör. LINQ to SQL tasarımcısı tarafından oluşturulan Yemeği sınıfı) tarafından korunan sınıfları eklemek için kullanılabilir ve bizim koduyla uðraþmaya öğesinden aracı korunmanıza yardımcı olun. Biz \Models klasöre sağ tıklayarak yeni bir parçalı sınıf bizim projeye ekleyin ve ardından "Yeni Öğe Ekle" menü komutu seçin. Biz daha sonra "Yeni Öğe Ekle" iletişim içinde "Sınıfının" şablonunu seçin ve Dinner.cs olarak adlandırın.

![](build-a-model-with-business-rule-validations/_static/image11.png)

"Ekle" düğmesini tıklatarak Dinner.cs dosya bizim projeye ekleyin ve IDE içinde açın. Biz bir temel kural/doğrulama zorlama framework kullanarak ardından uygulayabilirsiniz kod aşağıda:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample10.cs)]

Yukarıdaki kod ilgili birkaç Notlar:

- İçerdiği kodu LINQ to SQL tasarımcısı tarafından oluşturulan ve tutulan sınıfı ile birleştirilmiş ve tek bir sınıf halinde derlenmiş anlamına gelen "kısmi" anahtar sözcük – Yemeği sınıfı başında.
- RuleViolation sınıfı, bir kural ihlali hakkında daha fazla ayrıntı sağlamamız veren projeye ekleyeceğiz yardımcı bir sınıftır.
- Değerlendirilecek bizim doğrulama hem de iş kurallarını Dinner.GetRuleViolations() yöntemi neden olur (size kısa süre içinde uygulamadan). Daha sonra geri kural hatalar hakkında daha fazla bilgi sağlayan RuleViolation nesnelerinin bir dizisi döndürür.
- Dinner.IsValid özelliği Yemeği nesne herhangi etkin RuleViolations olup olmadığını belirten bir kullanışlı yardımcı özelliği sağlar. İstediğiniz zaman Yemeği nesnesi kullanılarak bir geliştirici tarafından önceden denetlenebilir (ve bir özel durum oluşturmaz).
- Dinner.OnValidate() kısmi hakkında veritabanı içinde kalıcı olmasını Yemeği nesnedir zaman bildirilmesini kurmamızı sağlayan bir LINQ-SQL sağlayan bir kanca yöntemidir. Yukarıdaki bizim OnValidate() uygulama kaydedilmeden önce Yemeği hiçbir RuleViolations sahip olmasını sağlar. Geçersiz bir durumda ise LINQ işlem iptal etmek için SQL neden olacak bir özel durum oluşturur.

Bu yaklaşım, biz doğrulama ve iş kurallarını içine tümleştirebilir basit bir çerçeve sağlar. Şu an için ekleyelim bizim GetRuleViolations() yöntemi kuralları aşağıda:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample11.cs)]

Tüm RuleViolations bir dizi döndürmek için C# "return verim" özelliği kullanıyoruz. Yukarıdaki ilk altı kural denetimleri yalnızca dize özellikleri bizim Yemeği null veya boş olamaz uygulayın. Son kural biraz daha ilginç ve çağrıları sizi Projemizin doğrulamak için ContactPhone ekleyebilirsiniz PhoneValidator.IsValidNumber() yardımcı yöntemi numara biçimi eşleşmeleri Yemeği'nın ülke.

Biz kullanabilirsiniz. Bu telefon doğrulama desteği uygulamak için NET normal ifade desteği. Ekleyebiliriz basit bir PhoneValidator uygulaması aşağıdadır bizim projeye bize Ülkeye özel Regex düzeni denetimleri eklemek sağlar:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample12.cs)]

#### <a name="handling-validation-and-business-logic-violations"></a>Doğrulama ve iş mantığı ihlalleri işleme

Yukarıdaki doğrulama ve iş kuralı kodu, biz oluşturmak veya bir Yemeği güncelleştirmek için denemenizde ekledik, bizim Doğrulama mantığı kuralları değerlendirilir ve zorunlu tutulur.

Geliştiriciler gibi aşağıda proaktif olarak Yemeği nesne geçerli olup olmadığını belirlemek için kod ve içindeki tüm ihlalleri özel durumlar oluşturma olmadan listesini almak yazabilirsiniz:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample13.cs)]

Biz geçersiz bir durumda bir Yemeği kaydetmeyi denediğinizde, biz üzerinde DinnerRepository Save() yöntemini çağırdığınızda bir özel durum oluşturulur. LINQ-SQL otomatik olarak Yemeği 's değişiklikleri kaydeder ve bir özel durum kuralı ihlallerini Yemeği varsa yükseltmek için Dinner.OnValidate() kodu eklediğimiz önce bizim Dinner.OnValidate() kısmi yöntemini çağıran oluşur. Bu özel durumu yakalayın ve Tepkisel düzeltmek için ihlallerinin bir listesini alın:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample14.cs)]

Bizim doğrulama hem de iş kurallarını bizim modeli katman içinde ve UI katmanında içinde değil uygulanan olduğundan, bunlar uygulanan ve olması uygulamamız içindeki tüm senaryolar arasında kullanılır. Daha sonra değiştirmek veya iş kuralları ekleyebilir ve works bizim Yemeği nesnelerle bunları dikkate tüm koda sahip.

Tek bir yerde iş kuralları değiştirme esnekliğine sahip olmak, bu değişiklikler uygulama ve UI mantığı boyunca ripple gerek kalmadan bir iyi yazılmış bir uygulama ve bir MVC çerçevesi teşvik yardımcı olan bir avantajı işaretidir.

### <a name="next-step"></a>Sonraki adım

Biz şimdi biz sorgu hem bizim veritabanını güncelleştirmek için kullanabileceğiniz bir model açıyor.

Şimdi bazı denetleyicilerinin ve görünümlerin bir HTML UI deneyimi çevresinde yapı kullanırız projesine ekleyelim.

> [!div class="step-by-step"]
> [Önceki](create-a-database.md)
> [sonraki](use-controllers-and-views-to-implement-a-listingdetails-ui.md)
