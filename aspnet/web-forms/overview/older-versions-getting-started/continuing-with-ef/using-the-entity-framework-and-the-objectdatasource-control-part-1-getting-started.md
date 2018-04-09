---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started
title: 'Entity Framework 4.0 ve ObjectDataSource denetimi kullanarak, bölüm 1: Başlarken | Microsoft Docs'
author: tdykstra
description: Bu öğretici seri ile çalışmaya başlama Entity Framework öğretici serisi tarafından oluşturulan Contoso University web uygulaması üzerinde oluşturur. Yo varsa...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: 244278c1-fec8-4255-8a8a-13bde491c4f5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started
msc.type: authoredcontent
ms.openlocfilehash: 6584767418c898913777b3b1549a816679c8430d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-1-getting-started"></a>Entity Framework 4.0 ve ObjectDataSource denetimi kullanarak, bölüm 1: Başlarken
====================
by [Tom Dykstra](https://github.com/tdykstra)

> Bu öğretici seri tarafından oluşturulan Contoso University web uygulaması üzerinde derlemeler [Entity Framework 4.0 ile çalışmaya başlama](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) öğretici serisi. Önceki öğreticileri tamamlanmadı, Bu öğretici için bir başlangıç noktası olarak yapabilecekleriniz [uygulamayı karşıdan](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) oluşturduğunuz. Ayrıca [uygulamayı karşıdan](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) tam öğretici seri tarafından oluşturulur.
> 
> Contoso University örnek web uygulaması Entity Framework 4.0 ve Visual Studio 2010 kullanarak ASP.NET Web Forms uygulamalarının nasıl oluşturulacağını gösterir. Örnek uygulama kurgusal bir Contoso üniversite için bir Web sitesidir. Öğrenci giriş, indirmelere oluşturma ve Eğitmen atamaları gibi işlevselliği içerir.
> 
> Öğretici örnekler C# dilinde gösterir. [İndirilebilir örnek](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) hem C# ve Visual Basic kodu içerir.
> 
> ## <a name="database-first"></a>İlk veritabanı
> 
> Entity Framework verilerle çalışma üç yolu vardır: *veritabanı ilk*, *Model First*, ve *Code First*. Bu öğretici ilk veritabanı için ' dir. Senaryonuz için en uygun olanı seçmeniz konusunda bu iş akışları ve Kılavuzu arasındaki farklar hakkında bilgi için bkz: [Entity Framework geliştirme iş akışları](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).
> 
> ## <a name="web-forms"></a>Web Forms
> 
> Başlarken serisinin gibi Bu öğretici seri ASP.NET Web Forms modeli kullanır ve ASP.NET Web Forms Visual Studio ile çalışmaya nasıl bildiğiniz varsayar. Görmüyorsanız [ASP.NET 4.5 Web formları ile çalışmaya başlama](../../getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). ASP.NET MVC çerçevesi ile çalışmayı tercih ederseniz bkz [ASP.NET MVC kullanarak Entity Framework ile çalışmaya başlama](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
> 
> ## <a name="software-versions"></a>Yazılım sürümleri
> 
> | **Öğreticide gösterilen** | **İle de çalışır** |
> | --- | --- |
> | Windows 7 | Windows 8 |
> | Visual Studio 2010 | Web için Visual Studio 2010 Express. Öğreticinin sonraki sürümlerini Visual Studio test edilmemiştir. Menü seçimleri, iletişim kutuları ve şablonları birçok farklılıklar vardır. |
> | .NET 4 | .NET 4.5, .NET 4 geriye dönük olarak uyumludur, ancak öğreticinin .NET 4.5 ile test edilmemiştir. |
> | Entity Framework 4 | Öğreticinin sonraki sürümlerini Entity Framework test edilmemiştir. Entity Framework 5 ile başlayarak, varsayılan olarak EF kullanan `DbContext API` EF 4.1 ile sunulmuştur. EntityDataSource denetimi kullanmak için tasarlanmış `ObjectContext` API. İle EntityDataSource kullanma hakkında bilgi için Denetim `DbContext` API, bkz: [bu blog gönderisine](https://blogs.msdn.com/b/webdev/archive/2012/09/13/how-to-use-the-entitydatasource-control-with-entity-framework-code-first.aspx). |
> 
> ## <a name="questions"></a>Sorular
> 
> Öğretici için doğrudan ilgili olmayan sorularınız varsa, bunları nakledebilirsiniz [ASP.NET Entity Framework Forumu](https://forums.asp.net/1227.aspx), [Entity Framework ve LINQ to Entities Forumu](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/), veya [ StackOverflow.com](http://stackoverflow.com/).


`EntityDataSource` Denetimi, bir uygulama çok hızlı bir şekilde oluşturmanızı sağlar, ancak genellikle iş mantığı ve veri erişimi mantığında önemli miktarda tutmanızı gerektirir, *.aspx* sayfaları. Uygulamanızı karmaşıklığı büyümeye ve devam eden bakım gerektirecek şekilde düşünüyorsanız, daha fazla geliştirme zamanı Önden oluşturmak için yatırım yapabilir bir *n katmanlı* veya *katmanlı* uygulama yapısı daha rahat olmasıdır. Bu mimarisi uygulama için sunu katmanı iş mantığı katmanı (BLL) ve veri erişim katmanı'nı (DAL) ayırın. Bu yapı uygulamak için bir yolu `ObjectDataSource` yerine kontrol `EntityDataSource` denetim. Kullandığınızda `ObjectDataSource` denetim, kendi veri erişimi kodunuzu uygulamak ve içinde çağırma *.aspx* birçok aynı olan bir denetimi kullanma sayfaları özellikleri diğer veri kaynağı denetimler. Bu, veri erişimi için bir Web Forms denetimi kullanmanın avantajları n katmanlı yaklaşımın avantajları birleştirin sağlar.

`ObjectDataSource` Denetimi size daha fazla esneklik diğer yollarla. Kendi veri erişimi kod yazmak için bu görevleri olduğu daha fazlasını okuyun, Ekle, Güncelleştir veya belirli bir varlık türünü silmek kolaydır, `EntityDataSource` denetimi gerçekleştirmek için tasarlanmıştır. Örneğin, bir varlık her güncelleştirildiğinde günlüğü gerçekleştirmek, her bir varlık silinmiş veya otomatik onay ve güncelleştirme veri yabancı anahtar değerine sahip bir satır ekleme yaparken gerektiğinde ilgili verileri arşivlemek.

## <a name="business-logic-and-repository-classes"></a>İş mantığı ve depo sınıfları

Bir `ObjectDataSource` denetim works oluşturduğunuz bir sınıfı çağırarak. Sınıfı, almak ve veri güncelleştirme yöntemleri içerir ve bu yöntemlere adlarını sağlamanız `ObjectDataSource` denetiminde biçimlendirme. İşleme veya geri gönderme işleme sırasında `ObjectDataSource` belirlediğiniz yöntemleri çağırır.

Temel CRUD işlemleri, ile kullanmak için oluşturduğunuz sınıfı yanı sıra `ObjectDataSource` denetim iş mantığını yürütme gerekebilir zaman `ObjectDataSource` okur veya verileri güncelleştirir. Örneğin, bir departman güncelleştirdiğinizde, tek bir kişi birden fazla bölüm Yöneticisi olamayacağı için başka bir Departmanlar aynı yönetici sahip olduğunu doğrulayın gerekebilir.

Bazı `ObjectDataSource` belgeleri gibi [ObjectDataSource sınıfına genel bakış](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.aspx), denetimi olarak adlandırılan bir sınıf çağıran bir *iş nesnesi* iş mantığı ve veri erişimi mantığı içerir . Bu öğreticide iş mantığı ve veri erişimi mantığı için ayrı sınıfları oluşturur. Veri erişimi mantığı yalıtır sınıfı olarak adlandırılan bir *depo*. İş mantığı sınıfı hem iş mantığı ve veri erişim yöntemleri içerir, ancak veri erişim görevleri gerçekleştirmek için depo veri erişim yöntemlerini çağırın.

Ayrıca bir Soyutlama Katmanı BLL ve otomatikleştirilmiş birim kolaylaştıran DAL arasında oluşturulur BLL test etme. Bu Soyutlama Katmanı, bir arabirim oluşturma ve iş mantığı sınıfı depoya örneği olduğunda arabirimi kullanılarak uygulanır. Depo arabirimini uygulayan herhangi bir nesneye bir başvurusu olan iş mantığı sınıfı sağlamak mümkün kılar. Normal işlem için Entity Framework ile çalışan bir depo nesnesi sağlayın. Test etmek için kolayca, koleksiyon olarak tanımlanan sınıf değişkenleri gibi işleyebileceğiniz şekilde depolanan verilerle çalışır bir depo nesnesi sağlayın.

Aşağıdaki çizimde bir depo olmadan veri erişimi mantığı içeren bir iş mantığı sınıf ve depo kullanan arasındaki farkı gösterir.

[![Image05](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image1.png)

Web sayfaları, oluşturarak başlar `ObjectDataSource` denetim, doğrudan bir depo bağlıysa, yalnızca temel veri erişim görevleri gerçekleştirdiğinden. Sonraki öğreticide Doğrulama mantığı ile bir iş mantığı sınıf oluşturacak ve bağlama `ObjectDataSource` depo sınıfını yerine o sınıfın denetimine. Doğrulama mantığı için birim testleri de oluşturur. Bu serideki üçüncü öğreticideki sıralama ve filtreleme uygulamaya işlevselliği ekleyeceksiniz.

Bu öğreticide oluşturduğunuz sayfaları çalışabilirsiniz `Departments` oluşturduğunuz veri modeli varlık kümesini [Başlarken Öğreticisi serisi](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md).

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image3.png)

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image5.png)

## <a name="updating-the-database-and-the-data-model"></a>Veritabanı ve veri modelinin güncelleştiriliyor

Bu öğreticide oluşturduğunuz veri modeline karşılık gelen değişiklikleri her ikisi de gerektirir veritabanına iki değişiklik yaparak başlayacak [Web Forms ve Entity Framework ile çalışmaya başlama](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) öğreticileri. Bu öğreticiler her birinde, veri modeli ile veritabanı bir veritabanı değişikliğinden sonra el ile eşitlemek için Tasarımcısı'nda değişiklikleri. Bu öğreticide tasarımcının kullanacağı **modeli veritabanından Güncelleştir** veri modeli otomatik olarak güncelleştirmek için aracı.

### <a name="adding-a-relationship-to-the-database"></a>Veritabanına ilişki ekleme

Oluşturduğunuz Contoso University web uygulamasını Visual Studio'da açın [Web Forms ve Entity Framework ile çalışmaya başlama](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) öğretici serisi ve ardından açın `SchoolDiagram` veritabanı diyagramı.

Bakarsanız `Department` tablo veritabanında olduğunu görürsünüz bir `Administrator` sütun. Bu sütun bir yabancı anahtar olan `Person` tablosu, ancak hiçbir yabancı anahtar ilişkisi veritabanında tanımlanır. Bir ilişki oluşturmak ve böylece Entity Framework otomatik olarak bu ilişkiyi işleyebilir veri modeli güncelleştirmek gerekir.

Veritabanı diyagramı sağ `Department` tablo ve seçin **ilişkileri**.

[![Image80](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image7.png)

İçinde **yabancı anahtar ilişkilerini** kutusunda **Ekle**, ardından için üç nokta düğmesine **tablolar ve sütunlar belirtimi**.

[![Image81](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image10.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image9.png)

İçinde **tablolar ve sütunlar** iletişim kutusunda, birincil anahtar tablosu ayarlayabilir ve alanı `Person` ve `PersonID`, yabancı anahtar tablosu ayarlayın ve alanı `Department` ve `Administrator`. (Bunu yaptığınızda, ilişki adı değiştirilecek `FK_Department_Department` için `FK_Department_Person`.)

[![Image82](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image12.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image11.png)

Tıklatın **Tamam** içinde **tablolar ve sütunlar** kutusunda, **Kapat** içinde **yabancı anahtar ilişkilerini** kutusunda ve değişiklikleri kaydedin. Kaydetmek isteyip istemediğiniz sorulursa `Person` ve `Department` tabloları, tıklatın **Evet**.

> [!NOTE]
> Sildiğiniz varsa `Person` kullanılmakta olan verilere karşılık gelen satırları `Administrator` sütun, edemeyecek bu değişikliği kaydetmek. Bu durumda, tablo Düzenleyicisi'nde kullanmak **Sunucu Gezgini** emin olmak için `Administrator` değeri her `Department` satır gerçekten var bir kayıt Kimliğini içeren `Person` tablo.
> 
> Değişiklikleri kaydettikten sonra bir satırı silmek mümkün olmaz `Person` bu kişi bir bölüm yöneticisi ise tablo. Bir üretim uygulamasında, belirli bir hata iletisi silme işlemini bir veritabanı kısıtlaması engeller ya da bir art arda silme belirtirsiniz sağlar. Art arda delete belirleme konusunda bir örnek için bkz: [Entity Framework ve ASP.NET – başlatılan Kısım 2 alma](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2.md).


### <a name="adding-a-view-to-the-database"></a>Veritabanına bir görünümü ekleme

Yeni *Departments.aspx* , oluşturma sayfasında, istediğiniz kullanıcılar departman yöneticilerinin seçebilmeniz için "Soyadı" biçiminde adlara sahip eğitmen, aşağı açılan listesi temin etmek. Bunu yapmak daha kolay hale getirmek için veritabanında bir görünüm oluşturur. Görünüm yalnızca aşağı açılan listeyi gereken verileri oluşur: (düzgün biçimlendirilmiş) tam adı ve kayıt anahtarı.

İçinde **Sunucu Gezgini**, genişletin *School.mdf*, sağ tıklatın **görünümleri** klasörü ve select **Yeni Görünüm Ekle**.

[![Image06](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image14.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image13.png)

Tıklatın **Kapat** zaman **Tablo Ekle** iletişim kutusu belirir ve aşağıdaki SQL deyimini SQL bölmesine yapıştırın:

[!code-sql[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample1.sql)]

Görünüm olarak kaydedebilirsiniz `vInstructorName`.

### <a name="updating-the-data-model"></a>Veri modeli güncelleştiriliyor

İçinde *DAL* klasörü, açık *SchoolModel.edmx* dosya, tasarım yüzeyine sağ tıklatın ve seçin **güncelleştirme modeli veritabanından**.

[![Image07](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image16.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image15.png)

İçinde **veritabanı nesnelerinizi** iletişim kutusunda **Ekle** sekmesinde ve yeni oluşturduğunuz görünümü seçin.

[![Image08](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image18.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image17.png)

**Son**'a tıklayın.

Aracı oluşturduğunuz bkz Tasarımcısı'nda bir `vInstructorName` varlık ve arasında yeni bir ilişki `Department` ve `Person` varlıklar.

[![Image13](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image20.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image19.png)

> [!NOTE]
> İçinde **çıkış** ve **hata listesi** windows aracı birincil otomatik olarak oluşturulan bildiren bir uyarı iletisi görebileceği ve yeni anahtar `vInstructorName` görünümü. Bu beklenen bir davranıştır.


Ne zaman, başvuran yeni `vInstructorName` varlık kodda istemediğiniz bir küçük harf "v" ona önek olarak veritabanı kuralı kullanmak. Bu nedenle, varlık ve varlık kümesi modelde adlandırır.

Açık **Model tarayıcı**. Gördüğünüz `vInstructorName` bir varlık türü ve bir görünüm listelenir.

[![Image14](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image22.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image21.png)

Altında **SchoolModel** (değil **SchoolModel.Store**), sağ **vInstructorName** seçip **özellikleri**. İçinde **özellikleri** penceresinde, değişiklik **adı** özelliğini "InstructorName" ve değişiklik **varlık kümesi adı** "InstructorNames" özelliğine.

[![Image15](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image24.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image23.png)

Kaydet ve veri modelinin kapatın ve projeyi yeniden derleyin.

## <a name="using-a-repository-class-and-an-objectdatasource-control"></a>Bir depo sınıfına ve ObjectDataSource Denetimi kullanma

Yeni bir sınıf dosyasını içinde oluşturmak *DAL* klasörü adlandırın *SchoolRepository.cs*ve var olan kodu aşağıdaki kodla değiştirin:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample2.cs)]

Bu kod tek bir sağlar `GetDepartments` tüm varlıkları döndürür yöntemi `Departments` varlık kümesi. Erişecek olduğunu bildiğiniz için `Person` gezinti özelliği her satır için döndürülen, belirlediğiniz için bu özelliği kullanarak yükleme istekli `Include` yöntemi. Sınıf ayrıca uygulayan `IDisposable` nesne çıkarıldığından, veritabanı bağlantısını yayımlanan emin olmak için arabirim.

> [!NOTE]
> Her bir varlık türü için bir depo sınıfına oluşturmak yaygın bir uygulamadır. Bu öğreticide, bir depo sınıfına birden çok varlık türleri için kullanılır. Gönderilerde havuz deseni hakkında daha fazla bilgi için bkz: [Entity Framework ekibinin blog](https://blogs.msdn.com/b/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx) ve [Julie Lerman'ın blogu](http://thedatafarm.com/blog/data-access/agile-ef4-repository-part-3-fine-tuning-the-repository/).


`GetDepartments` Yöntemi döndürür bir `IEnumerable` nesne yerine bir `IQueryable` bile deposu nesne kullanıldıktan sonra döndürülen koleksiyon kullanılabilir olduğundan emin olmak için nesne. Bir `IQueryable` nesne erişilmeden ancak havuz nesne denetim çalışır verileri işlemek için zaman atıldı her veritabanı erişimi neden olabilir. Başka bir koleksiyon türü gibi döndürebilirsiniz bir `IList` yerine Nesne bir `IEnumerable` nesnesi. Ancak, döndüren bir `IEnumerable` nesne sağlar tipik salt okunur listede işleme görevlerini gibi gerçekleştirebilirsiniz `foreach` döngüler ve LINQ sorgularını ancak için ekleyemez veya bu değişiklikleri olacaktır kapsıyor koleksiyonunda öğeleri kaldırma veritabanına kalıcı.

Oluşturma bir *Departments.aspx* kullanan sayfasını *Site.Master* ana sayfa ve aşağıdaki biçimlendirmede ekleyin `Content` adlı Denetim `Content2`:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample3.aspx)]

Bu biçimlendirme oluşturur bir `ObjectDataSource` az önce oluşturduğunuz depo sınıfını kullanan denetim ve `GridView` verileri görüntülemek için denetim. `GridView` Denetim belirtir **Düzenle** ve **silmek** komutları, ancak henüz destekleyecek kodu eklenen henüz.

Birden fazla sütun kullan `DynamicField` , otomatik veri biçimlendirmeyi ve doğrulama işlevselliğinden yararlanabilmeniz denetler. Bunlar çalışmaya çağrı gerekecek `EnableDynamicData` yönteminde `Page_Init` olay işleyicisi. (`DynamicControl` içinde denetimleri kullanılmaz `Administrator` Gezinti özellikleriyle çalışmanıza yoktur çünkü alan.)

`Vertical-Align="Top"` Öznitelikleri olacak önemli daha sonra iç içe bir sahip bir sütunu eklediğinizde `GridView` denetimi kılavuza.

Açık *Departments.aspx.cs* dosya ve aşağıdakileri ekleyin `using` deyimi:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample4.cs)]

Sayfanın aşağıdaki işleyicisi ekleme `Init` olay:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample5.cs)]

İçinde *DAL* klasörünü adlı yeni bir sınıf dosyası oluşturma *Department.cs* ve var olan kodu aşağıdaki kodla değiştirin:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample6.cs)]

Bu kod meta verileri veri modeline ekler. Gerektiğini belirtir `Budget` özelliği `Department` varlığı gerçekten temsil eden para birimi veri türüne olmasına rağmen `Decimal`, ve değeri 0 ile $1,000,000.00 arasında olması gerektiğini belirtir. Ayrıca, belirtir `StartDate` özelliği bir tarih biçimini aa/gg/yyyy olarak biçimlendirilmelidir.

Çalıştırma *Departments.aspx* sayfası.

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image26.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image25.png)

Bir biçim dizesi belirtmedi rağmen dikkat *Departments.aspx* için sayfası biçimlendirmesini **bütçe** veya **başlangıç tarihi** sütunları, varsayılan para birimi ve tarih Bunlar için biçimlendirme uygulanmış `DynamicField` denetimleri, içinde sağlanan meta verileri kullanarak *Department.cs* dosya.

## <a name="adding-insert-and-delete-functionality"></a>Ekleme INSERT ve Delete işlevi

Açık *SchoolRepository.cs*, oluşturmak için aşağıdaki kodu ekleyin bir `Insert` yöntemi ve `Delete` yöntemi. Kod ayrıca adlı bir yöntem içerir `GenerateDepartmentID` sonraki kullanılabilir kayıt anahtarı değeri tarafından kullanılmak üzere hesaplar `Insert` yöntemi. Veritabanı bu için otomatik olarak hesaplamak için yapılandırılmadığı için bu gereklidir `Department` tablo.

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample7.cs)]

### <a name="the-attach-method"></a>Attach yöntemi

`DeleteDepartment` Yöntem çağrılarını `Attach` korunur bağlantı varlığı bellekte ve veritabanı arasındaki nesne bağlamın nesne durum Yöneticisi'nde yeniden oluşturmak için yöntem satır onu temsil eder. Bu yöntem çağrılarını önce gelmelidir `SaveChanges` yöntemi.

Terim *nesne bağlamı* türetilen Entity Framework sınıfına başvuruyor `ObjectContext` varlık kümeleri ve varlıkları erişmek için kullandığınız sınıfı. Bu proje için kodda adlı sınıfı `SchoolEntities`, ve bir örneği her zaman adlı `context`. Nesne bağlamın *nesne durumu Yöneticisi* türeyen bir sınıf `ObjectStateManager` sınıfı. Nesne kişi varlık nesneleri depolamak için ve her biri karşılık gelen bir tablo satırının veya veritabanındaki satırları eşit olup olmadığını izlemek için Nesne Durum Yöneticisi'ni kullanır.

Bir varlık okurken nesne bağlamı nesnesi durum Yöneticisi'nde saklar ve o nesnenin gösterimi veritabanı ile eşit olup olmadığını izler. Örneğin, bir özellik değeri değiştirirseniz, değiştirdiğiniz özelliği artık veritabanı ile eşit olduğunu belirtmek için bir bayrak ayarlanır. Çağırdığınızda sonra `SaveChanges` yöntemi, nesne bağlamına veritabanında nesne durumu Yöneticisi tam olarak ne varlık geçerli durumunu ve veritabanının durumu arasında farklı bildiği için yapmanız gerekenler bilir.

Ancak, bir sayfa oluşturulduğunda, nesne durumu Yöneticisi'nde, her şeyi birlikte bir varlık okur nesne bağlamı örneği atıldı olmadığından bu işlem bir web uygulamasında genellikle çalışmaz. Değişiklikleri uygulamanız gerekir nesne bağlamı geri gönderme işlenmek üzere örneği yeni bir örneğidir. Durumunda `DeleteDepartment` yöntemi, `ObjectDataSource` denetim yeniden oluşturur varlığın özgün sürümü sizin için değerleri görünümde durum, ancak bu yeniden oluşturulan `Department` varlık nesnesi durum Yöneticisi'nde mevcut değil. Aradığınız varsa `DeleteObject` yöntemi bu yeniden oluşturulan varlığı çağrısı başarısız nesne bağlamı varlık veritabanı ile eşit olup olmadığını bilmediğinden olurdu. Ancak, çağırma `Attach` yöntemi yeniden kurar yeniden oluşturulan varlığı ve değerleri arasında aynı izleme varlık bir nesne bağlamına önceki örnekte okurken başlangıçta otomatik olarak yapılıyordu veritabanında.

Ne zaman nesne durumu Yöneticisi varlıklarda izlemek için nesne bağlamı istemediğiniz ve bayrakları, bunu önlemek için ayarlayabileceğiniz zamanları vardır. Bu örnekler, bu serideki sonraki öğreticileri gösterilmektedir.

### <a name="the-savechanges-method"></a>SaveChanges yöntemi

Bu basit havuz sınıf temel CRUD işlemleri gerçekleştirmek nasıl ilkeleri gösterilmektedir. Bu örnekte, `SaveChanges` yöntemi her güncelleştirmeden hemen sonra çağrılır. Bir üretim uygulamasında çağrı isteyebilirsiniz `SaveChanges` veritabanı güncelleştirildiğinde üzerinde daha fazla denetim sağlamak için ayrı bir yöntem yönteminden. (Sonraki öğretici sonunda ilgili güncelleştirmeler Eşgüdümleme bir yaklaşım olan iş düzeni birim ele incelemeyi bağlantısını bulabilirsiniz.) Ayrıca örnek dikkat `DeleteDepartment` yöntemi eşzamanlılık çakışmalarını işleme için kodu içermez; bunun için kodu, bu serideki sonraki öğretici olarak eklenir.

### <a name="retrieving-instructor-names-to-select-when-inserting"></a>Eklerken seçmek için Eğitmen adları alınıyor

Yeni bölümler oluştururken bir açılan listedeki Eğitmen listesinden bir yönetici seçin olması gerekir. Bu nedenle, aşağıdaki kodu ekleyin *SchoolRepository.cs* daha önce oluşturduğunuz görünümünü kullanarak Eğitmen listesini almak için bir yöntem oluşturmak için:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample8.cs)]

### <a name="creating-a-page-for-inserting-departments"></a>Departmanlar eklemek için bir sayfa oluşturma

Oluşturma bir *DepartmentsAdd.aspx* kullanan sayfasını *Site.Master* sayfasında ve aşağıdaki biçimlendirmede eklemek `Content` adlı Denetim `Content2`:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample9.aspx)]

Bu biçimlendirme iki oluşturur `ObjectDataSource` denetimleri, yeni eklemek için bir tane `Department` varlıkları ve Eğitmen adları almak için bir tane `DropDownList` departmanı Yöneticiler seçmek için kullanılan denetim. Biçimlendirme oluşturur bir `DetailsView` yeni bölümler ve onu girme belirtir ve denetim için bir işleyici denetimi `ItemInserting` olay ayarlayabileceğiniz böylece `Administrator` yabancı anahtar değeri. Sonunda olan bir `ValidationSummary` hata iletileri görüntülemek için denetimi.

Açık *DepartmentsAdd.aspx.cs* ve aşağıdakileri ekleyin `using` deyimi:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample10.cs)]

Aşağıdaki sınıf değişkeni ve yöntemleri ekleyin:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample11.cs)]

`Page_Init` Yöntemi dinamik veri işlevselliği sağlar. İşleyicisi `DropDownList` denetimin `Init` olayı kaydeder denetim ve işleyicisi için bir başvuru `DetailsView` denetimin `Inserting` olayını almak için bu başvuru kullanır `PersonID` değeri seçili Eğitmen ve güncelleştirme `Administrator` yabancı anahtar özelliği `Department` varlık.

Sayfayı çalıştırın, yeni bir bölüm için bilgileri ekleyin ve ardından **Ekle** bağlantı.

[![Image04](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image28.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image27.png)

Değerleri için başka bir yeni bölüm girin. İçinde 1,000,000.00'den büyük bir sayı girin **bütçe** alan ve sonraki alana sekmesi. Bir yıldız işareti alanında görüntülenir ve fare işaretçisini üzerine tutarsanız, bu alan için meta verilerde girdiğiniz hata iletisi görebilirsiniz.

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image30.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image29.png)

Tıklatın **Ekle**, ve tarafından görüntülenen hata iletisini görmek `ValidationSummary` sayfanın sonundaki denetim.

[![Image12](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image32.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image31.png)

Ardından, tarayıcıyı kapatıp açın *Departments.aspx* sayfası. Silme yeteneği eklemek *Departments.aspx* ekleyerek sayfa bir `DeleteMethod` özniteliğini `ObjectDataSource` denetimi ve `DataKeyNames` özniteliğini `GridView` denetim. Bu denetimler için açılış etiketler şimdi aşağıdaki örnekte benzeyecektir:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample12.aspx)]

Sayfayı çalıştırın.

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image34.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image33.png)

Çalıştırdığınızda eklediğiniz bölüm silme *DepartmentsAdd.aspx* sayfası.

## <a name="adding-update-functionality"></a>Güncelleştirme işlevsellik ekleme

Açık *SchoolRepository.cs* ve aşağıdakileri ekleyin `Update` yöntemi:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample13.cs)]

Tıkladığınızda **güncelleştirme** içinde *Departments.aspx* sayfasında, `ObjectDataSource` denetimi oluşturur iki `Department` geçirilecek varlıklar `UpdateDepartment` yöntemi. Görünüm durumuna depolanan özgün değerler içeriyor ve diğer içindeki girilen yeni değerleri içeren `GridView` denetim. Kodda `UpdateDepartment` yöntemi geçişleri `Department` özgün değerlere sahip varlık `Attach` varlık arasındaki veritabanında nedir izleme oluşturmak için yöntem. Kod geçirmeden sonra `Department` yeni değerleri olan varlık `ApplyCurrentValues` yöntemi. Nesne bağlamı eski ve yeni değerlerini karşılaştırır. Eski değerden yeni bir değer farklıysa, nesne bağlamına özellik değerini değiştirir. `SaveChanges` Yöntemi ardından yalnızca değiştirilen sütun veritabanındaki güncelleştirir. (Güncelleştirme işlevi bu varlık için bir saklı yordam eşleştirilmiş, ancak tüm satır, bağımsız olarak sütunları değiştirilen güncelleştirilmesi.)

Açık *Departments.aspx* dosya ve aşağıdaki öznitelikleri eklemek `DepartmentsObjectDataSource` denetimi:

- `UpdateMethod="UpdateDepartment"`
- `ConflictDetection="CompareAllValues"`   
 Böylece yeni değerleri karşılaştırılabilir olması için bu nedenler eski değerleri görünümde durumu depolanan `Update` yöntemi.
- `OldValuesParameterFormatString="orig{0}"`   
 Bu özgün değer parametresi adı denetimi bildirir `origDepartment` .

Açılış etiketinde için biçimlendirme `ObjectDataSource` denetim şimdi aşağıdaki örnekte benzer:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample14.aspx)]

Ekleme bir `OnRowUpdating="DepartmentsGridView_RowUpdating"` özniteliğini `GridView` denetim. Bunu ayarlamak için kullanacağınız `Administrator` kullanıcının seçtiği bir açılır liste satır temel özellik değeri. `GridView` Etiketi şimdi açma, aşağıdaki örnekte benzer:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample15.aspx)]

Ekleme bir `EditItemTemplate` için Denetim `Administrator` sütuna `GridView` hemen sonra Denetim `ItemTemplate` denetimi bu sütun için:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample16.aspx)]

Bu `EditItemTemplate` denetim benzer `InsertItemTemplate` denetim *DepartmentsAdd.aspx* sayfası. Denetimin ilk değeri kullanılarak ayarlanan farktır `SelectedValue` özniteliği.

Önce `GridView` denetlemek, ekleme bir `ValidationSummary` denetlemek de yaptığınız gibi *DepartmentsAdd.aspx* sayfası.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample17.aspx)]

Açık *Departments.aspx.cs* ve kısmi sınıf bildiriminden hemen sonra başvurmak için özel bir alan oluşturmak için aşağıdaki kodu ekleyin `DropDownList` denetimi:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample18.cs)]

İşleyicileri eklemek `DropDownList` denetimin `Init` olay ve `GridView` denetimin `RowUpdating` olay:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample19.cs)]

İşleyicisi `Init` olayı kaydeder başvuru `DropDownList` sınıf alanı denetiminde. İşleyicisi `RowUpdating` olay başvurusu kullanıcının girdiği değer almak ve uygulamak için kullandığı `Administrator` özelliği `Department` varlık.

Kullanım *DepartmentsAdd.aspx* yeni bir bölüm eklemek için sayfa, ardından Çalıştır *Departments.aspx* sayfasında ve tıklayın **Düzenle** eklediğiniz satırda.

> [!NOTE]
> Eklemediğiniz satırları düzenlemeniz mümkün olmaz (diğer bir deyişle, olduğunu zaten veritabanında), veritabanındaki; geçersiz veriler nedeniyle veritabanı ile oluşturulmuş satırların Öğrenciler yöneticilerdir. Bunlardan birini düzenlemeye çalışırsanız, benzer bir hata raporları bir hata sayfası alırsınız `'InstructorsDropDownList' has a SelectedValue which is invalid because it does not exist in the list of items.`


[![Image10](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image36.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image35.png)

Geçersiz bir girerseniz **bütçe** tutar ve ardından **güncelleştirme**, aynı yıldız ve içinde gördüğünüz hata iletisini görmek *Departments.aspx* sayfası.

Bir alanın değerini değiştirin veya farklı bir yönetici seçin ve tıklatın **güncelleştirme**. Değişiklik görüntülenir.

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image38.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image37.png)

Bu kullanmaya giriş tamamlar `ObjectDataSource` denetimi için temel CRUD (Oluştur, oku, Güncelleştir, Sil) Entity Framework işlemleriyle. Basit bir n katmanlı uygulama oluşturduğunuza, ancak iş mantığı katmanı otomatik birim testi karmaşıklaştırır veri erişim katmanı için hala sıkı şekilde bağlı. Aşağıdaki öğreticide birim testi kolaylaştırmak için havuz deseni uygulamak nasıl görürsünüz.

> [!div class="step-by-step"]
> [Next](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
