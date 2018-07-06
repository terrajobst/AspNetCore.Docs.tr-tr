---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started
title: 'Entity Framework 4.0 ve ObjectDataSource denetimi kullanarak, 1. Bölüm: Başlarken | Microsoft Docs'
author: tdykstra
description: Bu öğretici serisinde Contoso University web uygulaması ile çalışmaya başlama Öğreticisi dizisinin Entity Framework tarafından oluşturulan geliştirir. Yo varsa...
ms.author: aspnetcontent
ms.date: 01/26/2011
ms.assetid: 244278c1-fec8-4255-8a8a-13bde491c4f5
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started
msc.type: authoredcontent
ms.openlocfilehash: 9eb39d0cf57e114537c76f33e2f4647196b0ff30
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37837684"
---
<a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-1-getting-started"></a>Entity Framework 4.0 ve ObjectDataSource denetimi kullanarak, 1. Bölüm: Başlarken
====================
tarafından [Tom Dykstra](https://github.com/tdykstra)

> Bu öğretici serisinde Contoso University web uygulaması tarafından oluşturulan geliştirir [Entity Framework 4.0 ile çalışmaya başlama](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) öğretici serisi. Önceki öğreticilerde tamamlanmadıysa, Bu öğretici için bir başlangıç noktası olarak yapabilecekleriniz [uygulamayı karşıdan](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) oluşturmuş olduğunuz. Ayrıca [uygulamayı karşıdan](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) tam öğretici serisinin tarafından oluşturulur.
> 
> Contoso University örnek web uygulaması Entity Framework 4.0 ve Visual Studio 2010 kullanarak ASP.NET Web Forms uygulamalarının nasıl oluşturulacağını gösterir. Örnek, kurgusal Contoso üniversite için bir Web uygulamasıdır. Öğrenci giriş, kurs oluşturma ve Eğitmen atamaları gibi işlevleri içerir.
> 
> Öğreticide örnekler C# ' de gösterilmektedir. [İndirilebilir örnek](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) hem C# ve Visual Basic kodunu içerir.
> 
> ## <a name="database-first"></a>İlk veritabanı
> 
> Entity Framework verilerle çalışabilir üç yolu vardır: *veritabanı ilk*, *modeli ilk*, ve *Code First*. Bu öğretici için veritabanı ilk bölümüdür. Senaryonuz için en uygun olanı seçin konusunda bu iş akışları ve rehberlik arasındaki farklar hakkında bilgi için bkz. [Entity Framework geliştirme iş akışlarının](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).
> 
> ## <a name="web-forms"></a>Web Forms
> 
> Bu öğretici serisinin Başlarken serisinin gibi ASP.NET Web Forms modeli kullanır ve Visual Studio'da ASP.NET Web Forms ile nasıl çalışılacağını bildiğiniz varsayılır. Görmüyorsanız [ASP.NET 4.5 Web Forms ile çalışmaya başlama](../../getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). ASP.NET MVC çerçevesiyle çalışmak istiyorsanız, bkz. [ASP.NET MVC kullanarak Entity Framework ile çalışmaya başlama](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
> 
> ## <a name="software-versions"></a>Yazılım sürümleri
> 
> | **Öğreticide gösterilen** | **İle de çalışır.** |
> | --- | --- |
> | Windows 7 | Windows 8 |
> | Visual Studio 2010 | Web için Visual Studio 2010 Express. Öğretici Visual Studio'nun daha yeni sürümlerini test edilmemiştir. Menü seçimleri, iletişim kutuları ve şablonları pek çok fark vardır. |
> | .NET 4 | .NET 4.5 .NET 4 ile geriye dönük uyumludur, ancak öğreticide .NET 4.5 ile test edilmemiştir. |
> | Entity Framework 4 | Öğreticinin sonraki sürümlerinde Entity Framework ile test edilmemiştir. Entity Framework 5 ile başlayarak, varsayılan olarak EF kullanan `DbContext API` ile EF 4.1 tanıtılmıştır. EntityDataSource denetimi kullanmak için tasarlanmış `ObjectContext` API. EntityDataSource kullanma hakkında daha fazla bilgi için denetimini `DbContext` API bakın [bu blog gönderisini](https://blogs.msdn.com/b/webdev/archive/2012/09/13/how-to-use-the-entitydatasource-control-with-entity-framework-code-first.aspx). |
> 
> ## <a name="questions"></a>Sorular
> 
> Öğretici için doğrudan ilgili olmayan sorularınız varsa, bunları gönderebilir [ASP.NET Entity Framework Forumu](https://forums.asp.net/1227.aspx), [Entity Framework ve LINQ to Entities Forumu](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/), veya [ StackOverflow.com](http://stackoverflow.com/).


`EntityDataSource` Denetimi çok hızlı bir şekilde uygulama oluşturmanıza olanak sağlar, ancak genellikle önemli miktarda iş mantığı ve veri erişim mantığı tutmanızı gerektirir, *.aspx* sayfaları. Uygulamanız karmaşık hale gelmesi ve devam eden bakım gerektirecek şekilde bekliyorsanız, daha fazla geliştirme süresini Önden oluşturmak için yatırım yapabilir bir *n katmanlı* veya *katmanlı* uygulama yapısı daha sürdürülebilir olmasıdır. Bu mimariyi uygulamak için sunu katmanı (BLL) iş mantığı katmanı ve veri erişim katmanı (DAL) ayırın. Bu yapı uygulamak için bir tek yolu `ObjectDataSource` denetimi yerine `EntityDataSource` denetimi. Kullanırken `ObjectDataSource` denetimi, kendi veri erişim kodunu uygulamak ve onu çağırmak *.aspx* sayfalarını kullanarak aynı çoğunu içeren bir denetim özellikleri gibi diğer veri kaynağı denetimleri. Bu, veri erişimi için bir Web Forms denetimi kullanmanın avantajları bir n katmanlı yaklaşımın avantajları birlikte sağlar.

`ObjectDataSource` Denetimi, size daha fazla esneklik başka yöntemlerle de. Kendi veri erişim kodu yazmak, görevleri olan daha fazlasını okuyun, eklemek, güncelleştirmek veya bir özel varlık türünü silmek daha kolay olmasıdır, `EntityDataSource` denetimi gerçekleştirmek için tasarlanmıştır. Örneğin, varlığın her güncelleştirildiğinde günlüğe kaydetme gerçekleştirmek, veri varlığı silme veya otomatik olarak denetleyin ve güncelleştirmeyi yabancı anahtar değere sahip bir satır eklendiğinde gerektiği gibi ilgili verileri arşivleme.

## <a name="business-logic-and-repository-classes"></a>İş mantığı ve depo sınıfları

Bir `ObjectDataSource` denetim çalışır, oluşturduğunuz bir sınıfı çağırarak. Sınıfı, almak ve veri güncelleştirme yöntemleri içerir ve bu yöntemlere adlarını sağlamanız `ObjectDataSource` denetiminde biçimlendirme. İşleme veya geri gönderme işlemi sırasında `ObjectDataSource` belirlediğiniz yöntemleri çağırır.

Temel CRUD işlemleri ile kullanılacak oluşturma sınıfı yanı sıra `ObjectDataSource` denetimi, iş mantığı yürütmek gerekebilir, `ObjectDataSource` okur veya verileri güncelleştirir. Örneğin, bir departman güncelleştirdiğinizde, birden fazla bölüm Yöneticisi bir kişinin olamayacağı için hiçbir diğer bölümler aynı yönetici olduğunu doğrulamak gerekebilir.

Bazı `ObjectDataSource` belgeleri, gibi [ObjectDataSource sınıfına genel bakış](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.aspx), bir sınıf olarak adlandırılan Denetim çağrıları bir *iş nesnesi* hem iş mantığı ve veri erişim mantığı içerir . Bu öğreticide, veri erişim mantığını ve iş mantığı için ayrı sınıfları oluşturacaksınız. Veri erişim mantığı kapsülleyen sınıftır adlı bir *depo*. İş mantığı sınıfı, hem iş mantığı ve veri erişim yöntemleri içerir, ancak veri erişim görevleri gerçekleştirmek için depo veri erişim yöntemlerine çağırın.

BLL ve DAL otomatik birim kolaylaştıran arasında bir soyutlama katmanı da oluşturacağınız BLL test etme. Bu Soyutlama Katmanı, bir arabirim oluşturma ve iş mantığı sınıfı depoda başlattığınızda arabirimi kullanılarak uygulanır. Depo arabirimi uygulayan herhangi bir nesneye bir başvuru ile iş mantığı sınıfı sağlamanızı mümkün kılar. Normal işlem için Entity Framework ile birlikte çalışan bir depo nesnesi sağlayın. Test etmek için kolayca, koleksiyon olarak tanımlanan sınıfı değişkenleri gibi işleyebileceğiniz şekilde depolanan verilerle çalışan bir depo nesnesi sağlayın.

Aşağıdaki çizimde, bir depo veri erişim mantığını içeren bir iş mantığı sınıf ve bir depo kullanan bir arasındaki farkı gösterir.

[![Image05](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image1.png)

Web sayfaları, oluşturarak başlar `ObjectDataSource` denetim yalnızca temel veri erişim görevleri gerçekleştirdiğinden doğrudan bir depoya bağlı. Sonraki öğreticide Doğrulama mantığı ile iş mantığı sınıfı oluşturacak ve bağlama `ObjectDataSource` yerine bu sınıf bir depo sınıfına denetimi. Doğrulama mantığı için birim testleri de oluşturur. Bu serideki üçüncü öğreticide sıralama ve filtreleme işlevselliği uygulamaya ekleyeceksiniz.

Bu öğreticide oluşturduğunuz sayfaları çalışın `Departments` , oluşturduğunuz veri modeli varlık kümesini [başlangıç öğretici serisinin](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md).

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image3.png)

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image5.png)

## <a name="updating-the-database-and-the-data-model"></a>Veritabanı ve veri modelini güncelleştirme

İkisi de gerektiren oluşturduğunuz veri modeline karşılık gelen değişiklikleri veritabanına iki değişiklik yaparak Bu öğretici başlayacak [Entity Framework ve Web Forms ile çalışmaya başlama](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) öğreticiler. Bu öğreticiler her birinde, veri modeli veritabanı ile bir veritabanı değişiklikten sonra el ile eşitlemek için tasarımcıda yapılan değişiklikler. Bu öğreticide tasarımcının kullanacağınız **veritabanından modeli güncelleştirme** veri modeli otomatik olarak güncelleştirmek için aracı.

### <a name="adding-a-relationship-to-the-database"></a>Veritabanına ilişki ekleme

Visual Studio'da oluşturduğunuz Contoso University web uygulamasını açın [Entity Framework ve Web Forms ile çalışmaya başlama](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) açın ve öğretici serisinin `SchoolDiagram` veritabanı diyagramı.

Bakarsanız `Department` tablo veritabanı diyagramında sahip olduğunu göreceksiniz bir `Administrator` sütun. Bu sütun olduğundan yabancı anahtar `Person` tablosu, ancak herhangi bir yabancı anahtar ilişkisi veritabanında tanımlanır. İlişki oluşturma ve veri modelini güncelleştirme, Entity Framework otomatik olarak bu ilişkiyi işleyebilmesi gerekir.

Veritabanı diyagramında, sağ `Department` tablosuna sağ tıklayıp seçin **ilişkileri**.

[![Image80](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image7.png)

İçinde **yabancı anahtar ilişkilerini** kutusunu tıklatıp **Ekle**, sonra üç nokta simgesine tıklayın **tablolar ve sütunlar belirtimi**.

[![Image81](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image10.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image9.png)

İçinde **tablolar ve sütunlar** iletişim kutusunda, birincil anahtar tablosu ayarlama ve alan `Person` ve `PersonID`, yabancı anahtar tablosu ayarlama ve alan `Department` ve `Administrator`. (Bunu yaptığınızda, ilişki adı çıkacak `FK_Department_Department` için `FK_Department_Person`.)

[![Image82](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image12.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image11.png)

Tıklayın **Tamam** içinde **tablolar ve sütunlar** kutusunun **Kapat** içinde **yabancı anahtar ilişkilerini** kutusunda ve değişiklikleri kaydedin. Kaydetmek isteyip istemediğiniz sorulursa `Person` ve `Department` tablolar tıklayın **Evet**.

> [!NOTE]
> Silmiş olduğunuz varsa `Person` kullanılmakta olan verilere karşılık gelen satır `Administrator` sütun olmayacaktır bu değişikliği kaydetmek kullanabilirsiniz. Bu durumda, tablo Düzenleyicisi'nde kullanmak **Sunucu Gezgini** emin olmak için `Administrator` değerini her `Department` satır gerçekten var olan bir kayıt Kimliğini içeren `Person` tablo.
> 
> Değişiklikleri kaydettikten sonra bir satırdan silmek mümkün olmayacaktır `Person` söz konusu kişinin bir departman Yöneticisi ise tablo. Bir üretim uygulamasında belirli bir hata iletisi sağlandığından veritabanı kısıtlaması bir silme işlemi engeller ya da art arda silineceğini belirtmeniz gerekir. Art arda silineceğini belirtin ilişkin bir örnek için bkz: [Entity Framework ve ASP.NET – bölüm 2 kullanmaya başlama](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2.md).


### <a name="adding-a-view-to-the-database"></a>Veritabanına bir görünüm ekleme

Yeni *Departments.aspx* oluşturacağınız sayfasında, istediğiniz kullanıcıları departman yöneticilerinin seçebilmeniz için "Soyadı" biçiminde adlara sahip eğitmenler, aşağı açılan listesini sağlar. Bunu daha kolay hale getirmek için veritabanında bir görünüm oluşturur. Görünüm açılır listeyi gerekli verileri oluşur: (doğru şekilde biçimlendirildiğini) tam adı ve kayıt anahtarı.

İçinde **Sunucu Gezgini**, genişletin *School.mdf*, sağ **görünümleri** klasörü ve select **Yeni Görünüm Ekle**.

[![Image06](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image14.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image13.png)

Tıklayın **Kapat** olduğunda **Tablo Ekle** iletişim kutusu görünür ve aşağıdaki SQL deyimini SQL bölmesine yapıştırın:

[!code-sql[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample1.sql)]

Görünüm olarak kaydedebilirsiniz `vInstructorName`.

### <a name="updating-the-data-model"></a>Veri modeli güncelleştiriliyor

İçinde *DAL* açık klasör *SchoolModel.edmx* dosya, tasarım yüzeyine sağ tıklayın ve seçin **veritabanından bir güncelleştirme modeli**.

[![Image07](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image16.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image15.png)

İçinde **veritabanı nesnelerinizi seçin** iletişim kutusunda **Ekle** sekme ve yeni oluşturduğunuz görünümü seçin.

[![Image08](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image18.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image17.png)

**Son**'a tıklayın.

Tasarımcısı'nda, araç oluşturulan görürsünüz. bir `vInstructorName` varlık ve arasında yeni bir ilişki `Department` ve `Person` varlıklar.

[![Image13](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image20.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image19.png)

> [!NOTE]
> İçinde **çıkış** ve **hata listesi** windows araç birincil otomatik olarak oluşturulan bildiren bir uyarı iletisi görebilirsiniz ve yeni anahtar `vInstructorName` görünümü. Bu beklenen bir davranıştır.


Ne zaman başvuru yeni `vInstructorName` varlık kodda istemediğiniz bir küçük harf "v", önek, veritabanı kuralını kullanır. Bu nedenle, varlık ve varlık kümesi modelde adlandırır.

Açık **Model tarayıcı**. Gördüğünüz `vInstructorName` bir varlık türü ve bir görünüm listelenir.

[![image14](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image22.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image21.png)

Altında **SchoolModel** (değil **SchoolModel.Store**), sağ **vInstructorName** seçip **özellikleri**. İçinde **özellikleri** penceresinde değişiklik **adı** özelliğini "InstructorName" ve değişiklik **varlık kümesi adı** "InstructorNames" özelliği.

[![Image15](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image24.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image23.png)

Kaydet ve veri modelini kapatın ve ardından projeyi yeniden derleyin.

## <a name="using-a-repository-class-and-an-objectdatasource-control"></a>Bir depo sınıfına ve ObjectDataSource Denetimi kullanma

Yeni bir sınıf dosyası oluşturma *DAL* klasörünü adlandırın *SchoolRepository.cs*, mevcut kodu şu kodla değiştirin:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample2.cs)]

Bu kod bir tek sağlar `GetDepartments` tüm varlık döndüren yöntem `Departments` varlık kümesi. Erişecek olduğunu bildiğiniz `Person` her satır için gezinme özelliği döndürdü, belirttiğiniz için bu özelliği kullanarak yükleme istekli `Include` yöntemi. Sınıfı ayrıca uygulayan `IDisposable` nesne çıkarıldığından, veritabanı bağlantısını yayımlanan emin olmak için arabirim.

> [!NOTE]
> Her varlık türü için bir depo sınıfına oluşturma yaygın bir uygulamadır. Bu öğreticide, bir depo sınıfına birden çok varlık türleri için kullanılır. Gönderi depo düzeni hakkında daha fazla bilgi için bkz. [Entity Framework ekip blogu](https://blogs.msdn.com/b/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx) ve [Julie Lerman'ın blog](http://thedatafarm.com/blog/data-access/agile-ef4-repository-part-3-fine-tuning-the-repository/).


`GetDepartments` Yöntemi döndürür bir `IEnumerable` nesne yerine `IQueryable` bile depo nesne bırakıldıktan sonra döndürülen koleksiyon kullanılabilir olmasını sağlamak için nesne. Bir `IQueryable` nesne, erişilen, ancak veri bağlama denetimi çalışır verileri işlemek için zaman deposu nesnesi atıldı her veritabanı erişimi neden olabilir. Başka bir koleksiyon türü gibi döndürebilir bir `IList` yerine Nesne bir `IEnumerable` nesne. Ancak, döndüren bir `IEnumerable` nesne sağlar tipik salt okunur listesi işleme görevlerini gibi gerçekleştirebilirsiniz `foreach` döngüler ve LINQ sorguları ancak ekleyemez veya bu tür değişiklikleri İmparatoru olduğunu ima edecek koleksiyondaki öğeleri Kaldır veritabanına kalıcı.

Oluşturma bir *Departments.aspx* kullanan sayfa *Site.Master* ana sayfa ve ekleme aşağıdaki biçimlendirmede `Content` adlı Denetim `Content2`:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample3.aspx)]

Bu biçimlendirme oluşturur bir `ObjectDataSource` az önce oluşturduğunuz depo sınıfını kullanan denetimi ve bir `GridView` verileri görüntülemek için denetim. `GridView` Denetimi belirtir **Düzenle** ve **Sil** komutları, ancak henüz eklediğiniz henüz desteklemek için kod.

Çeşitli sütunları kullanın `DynamicField` otomatik veri biçimlendirme ve doğrulama işlevini yararlanabilir, böylece denetler. Bu iş çağrı gerekecektir `EnableDynamicData` yönteminde `Page_Init` olay işleyicisi. (`DynamicControl` denetimleri içinde kullanılmaz `Administrator` bunlar Gezinti özellikleri ile çalışmaz çünkü alan.)

`Vertical-Align="Top"` Öznitelikleri olacak önemli daha sonra iç içe bir sahip bir sütunu eklediğinizde `GridView` kılavuz denetimi.

Açık *Departments.aspx.cs* dosyasını açıp aşağıdaki `using` deyimi:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample4.cs)]

Sayfanın aşağıdaki işleyicisi eklersiniz `Init` olay:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample5.cs)]

İçinde *DAL* klasöründe adlı yeni bir sınıf dosyası oluşturma *Department.cs* ve varolan kodu aşağıdaki kodla değiştirin:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample6.cs)]

Bu kod, meta verileri veri modeline ekler. Belirtir `Budget` özelliği `Department` varlık temsil para veri türü olmasına rağmen `Decimal`, ve değeri 0 ile $1,000,000.00 arasında olması gerektiğini belirtir. Ayrıca, belirtir `StartDate` özelliği bir tarih biçimini aa/gg/yyyy olarak biçimlendirilmelidir.

Çalıştırma *Departments.aspx* sayfası.

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image26.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image25.png)

Bir biçim dizesinde belirtmedi rağmen dikkat *Departments.aspx* sayfa biçimlendirmesi için **bütçe** veya **başlangıç tarihi** sütunlar varsayılan para birimi ve tarih bunlara göre biçimlendirme uygulanmış `DynamicField` denetimleri içinde sağlanan meta verileri kullanarak *Department.cs* dosya.

## <a name="adding-insert-and-delete-functionality"></a>Ekleme INSERT ve Delete işlevleri

Açık *SchoolRepository.cs*, oluşturmak için aşağıdaki kodu ekleyin. bir `Insert` yöntemi ve bir `Delete` yöntemi. Kod ayrıca adlı bir yöntem içerir `GenerateDepartmentID` tarafından kullanılmak üzere sonraki kullanılabilir kayıt anahtar değerini hesaplar `Insert` yöntemi. Veritabanı bu otomatik olarak hesaplamak için yapılandırılmadığından bu gereklidir `Department` tablo.

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample7.cs)]

### <a name="the-attach-method"></a>Attach yöntemi

`DeleteDepartment` Yöntem çağrılarını `Attach` korunur bağlantıyı varlık bellek içinde ve veritabanı arasında nesne bağlamı'nın nesne durum Yöneticisi'nde yeniden oluşturmak için gereken yöntemini satır, temsil eder. Bu yöntem çağrıları önce gerçekleşmelidir `SaveChanges` yöntemi.

Terim *nesne bağlamı* türetildiği Entity Framework sınıfı başvurduğu `ObjectContext` varlık setleri ve varlıkları erişmek için kullandığınız sınıfı. Bu proje için kod içinde sınıf adlı `SchoolEntities`, ve her zaman bir örneğini adlı `context`. Nesne bağlamı'nın *nesne durum Yöneticisi* türetildiği bir sınıf olan `ObjectStateManager` sınıfı. Nesne kişi varlığı nesneleri depolamak için ve her birini karşılık gelen bir tablo satır veya veritabanındaki satırları eşit olup olmadığını izlemek için Nesne Durum Yöneticisi'ni kullanır.

Bir varlık okuduğunuzda nesne bağlamı nesne durum Yöneticisi'nde depolar ve o nesnenin gösterimini veritabanı ile eşitlenmiş olup olmadığını izler. Örneğin, bir özellik değeri değiştirirseniz, değiştirdiğiniz özelliği artık veritabanı ile eşit olduğunu belirtmek için bir bayrağı ayarlanır. Ardından çağırdığınızda `SaveChanges` yöntemi, nesne bağlamı bilen veritabanında nesne durum Yöneticisi tam olarak ne varlığın geçerli durumu ve veritabanının durumu arasında farklı olduğunu bildiğinden yapmanız gerekenler.

Ancak, kendi nesne durum Yöneticisi her şeyi birlikte bir varlığını okur nesne bağlam örneği bir sayfa oluşturulduğunda elden çünkü bu işlem bir web uygulamasında, genellikle çalışmaz. Bu değişiklikleri uygulamalısınız nesne bağlamı geri gönderme işlemi için oluşturulan yeni bir örneğidir. Durumunda, `DeleteDepartment` yöntemi `ObjectDataSource` denetimi yeniden oluşturur varlığın özgün sürümle sizin için değerleri görünüm durumu, ancak bu yeniden oluşturulan `Department` varlık nesnesi durum Yöneticisi'nde mevcut değil. Aradığınız varsa `DeleteObject` bu yeniden oluşturulan varlık yönteminde, çağrı nesne bağlamı varlık veritabanı ile eşitlenmiş olup olmadığını bilmediğinden gösterilebilir. Ancak, çağırma `Attach` yöntemi yeniden kurar yeniden oluşturulan varlığın ve değerleri arasında aynı izleme varlık nesne bağlamı bir önceki örnekte okuduğunuzda başlangıçta otomatik olarak yapıldığı veritabanında.

Ne zaman nesne durum Yöneticisi'nde varlıkları izlemek için nesne bağlamı istemediğiniz ve bayrakları, bunu önlemek için ayarlayabileceğiniz zamanlar vardır. Bu serideki sonraki öğreticilerde Bu örnekler gösterilmektedir.

### <a name="the-savechanges-method"></a>SaveChanges yöntemi

Bu basit bir depo sınıfına temel ilkelerini CRUD işlemlerini nasıl gerçekleştireceğinizi gösterir. Bu örnekte, `SaveChanges` yöntemi her güncelleştirmeden hemen sonra çağrılır. Bir üretim uygulamasında çağırmak isteyebilirsiniz `SaveChanges` veritabanı güncelleştirildiğinde üzerinde daha fazla denetim sağlamak için ayrı bir yöntem yöntemi. (Sonraki öğreticinin sonunda bir ilgili güncelleştirmeleri koordine bir yaklaşım olan çalışma deseni birimi incelemeyi bağlantısını bulabilirsiniz.) Örnekte ayrıca dikkat `DeleteDepartment` yöntemi eşzamanlılık çakışmalarını işleme kodunu içermez; bunun için kodu, bir sonraki Öğreticide bu serideki eklenir.

### <a name="retrieving-instructor-names-to-select-when-inserting"></a>Eklerken seçilecek Eğitmen adları alınıyor

Yeni bölümler oluşturulurken Eğitmenler aşağı açılan listesinde listesinden bir yönetici seçin olması gerekir. Bu nedenle, aşağıdaki kodu ekleyin *SchoolRepository.cs* daha önce oluşturduğunuz görünümünü kullanarak Eğitmenler listesini almak için bir yöntem oluşturmak için:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample8.cs)]

### <a name="creating-a-page-for-inserting-departments"></a>Departmanlar eklemek için bir sayfa oluşturma

Oluşturma bir *DepartmentsAdd.aspx* kullanan sayfa *Site.Master* sayfa ve ekleme aşağıdaki biçimlendirmede `Content` adlı Denetim `Content2`:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample9.aspx)]

Bu işaretleme iki oluşturur `ObjectDataSource` denetleyen yeni eklemek için bir tane `Department` varlıkları ve bir eğitmen adlarını almak için `DropDownList` departman yöneticilerinin seçmek için kullanılan bir denetim. Biçimlendirme oluşturur bir `DetailsView` yeni bölümler ve denetim için bir işleyici belirtir denetimi `ItemInserting` olay ayarlayabilirsiniz böylece `Administrator` yabancı anahtar değeri. Sonunda olduğu bir `ValidationSummary` hata iletileri görüntülemek için denetimi.

Açık *DepartmentsAdd.aspx.cs* ve aşağıdakileri ekleyin `using` deyimi:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample10.cs)]

Aşağıdaki sınıf değişkeni ve yöntemleri ekleyin:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample11.cs)]

`Page_Init` Yöntemi dinamik veri işlevini etkinleştirir. İşleyici için `DropDownList` denetimin `Init` olayı kaydeder denetim ve işleyici için bir başvuru `DetailsView` denetimin `Inserting` olay almak için bu başvuru kullanır `PersonID` değerini seçilen Eğitmen ve güncelleştirme `Administrator` yabancı anahtar özelliğine `Department` varlık.

Çalıştırırsanız, yeni bir bölüm bilgilerini ekleyin ve ardından **Ekle** bağlantı.

[![Image04](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image28.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image27.png)

Başka bir yeni bölümü için değerleri girin. İçinde 1,000,000.00'den büyük bir sayı girin **bütçe** alan ve sonraki alana için sekmesinde. Alanında bir yıldız işareti görünür ve üzerine fare işaretçisini tutarsanız, meta verilerde Bu alan için girdiğiniz hata iletisini görebilirsiniz.

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image30.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image29.png)

Tıklayın **Ekle**, ve tarafından görüntülenen hata iletisini gördüğünüz `ValidationSummary` sayfanın alt kısmındaki denetim.

[![image12](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image32.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image31.png)

Ardından, tarayıcıyı kapatın ve açın *Departments.aspx* sayfası. Silme yeteneği ekleme *Departments.aspx* sayfası ekleyerek bir `DeleteMethod` özniteliğini `ObjectDataSource` denetimi ve bir `DataKeyNames` özniteliğini `GridView` denetimi. Bu denetimler için açılış etiketleri, artık aşağıdaki örneğe benzer:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample12.aspx)]

Sayfayı çalıştırın.

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image34.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image33.png)

Çalıştırdığınızda eklediğiniz bölüm silme *DepartmentsAdd.aspx* sayfası.

## <a name="adding-update-functionality"></a>Güncelleştirme işlevsellik ekleme

Açık *SchoolRepository.cs* ve aşağıdakileri ekleyin `Update` yöntemi:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample13.cs)]

Tıkladığınızda **güncelleştirme** içinde *Departments.aspx* sayfasında `ObjectDataSource` denetimi oluşturur iki `Department` geçirmek için varlıklar `UpdateDepartment` yöntemi. Görünüm durumuna depolanan özgün değerleri içerir ve diğeri içindeki girilen yeni değerleri içeren `GridView` denetimi. Kodda `UpdateDepartment` yöntemi geçişleri `Department` özgün değerlerine olan varlık `Attach` varlık arasındaki veritabanında nedir izleme kurabilmek için yöntemi. Kod geçirmeden sonra `Department` için yeni değerleri olan varlık `ApplyCurrentValues` yöntemi. Nesne bağlamı, eski ve yeni değerlerini karşılaştırır. Yeni bir değer eski bir değerden farklı ise, nesne bağlamı özellik değerini değiştirir. `SaveChanges` Yöntemi ardından yalnızca değiştirilen sütun veritabanında güncelleştirir. (Güncelleştirme işlevi bu varlık için bir saklı yordam için eşleştirilmiş, ancak tüm bir satırda bağımsız olarak, sütunların değiştirildi güncelleştirilecek.)

Açık *Departments.aspx* dosyasını açıp aşağıdaki öznitelikleri eklemek `DepartmentsObjectDataSource` denetimi:

- `UpdateMethod="UpdateDepartment"`
- `ConflictDetection="CompareAllValues"`   
 Böylece yeni değerleri karşılaştırılabilir olması için bu neden eski değerleri görünüm durumu depolanan `Update` yöntemi.
- `OldValuesParameterFormatString="orig{0}"`   
 Bu özgün değer parametresi adını denetimi bildirir `origDepartment` .

Öğesinin açılış etiketinde için biçimlendirme `ObjectDataSource` denetimi artık aşağıdaki örnekte benzer:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample14.aspx)]

Ekleme bir `OnRowUpdating="DepartmentsGridView_RowUpdating"` özniteliğini `GridView` denetimi. Bunu ayarlamak için kullanacağınız `Administrator` aşağı açılan listede kullanıcının seçtiği satır temel özellik değeri. `GridView` Etiketiyle artık, aşağıdaki örnekte benzer şekilde görünür:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample15.aspx)]

Ekleme bir `EditItemTemplate` için Denetim `Administrator` sütuna `GridView` hemen sonra Denetim `ItemTemplate` denetim söz konusu sütun için:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample16.aspx)]

Bu `EditItemTemplate` denetim benzer `InsertItemTemplate` denetim *DepartmentsAdd.aspx* sayfası. Denetimin ilk değerini kullanarak ayarlandığını fark `SelectedValue` özniteliği.

Önce `GridView` ekleyin, denetimi bir `ValidationSummary` denetim yaptığınız gibi *DepartmentsAdd.aspx* sayfası.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample17.aspx)]

Açık *Departments.aspx.cs* ve parçalı sınıf bildiriminden hemen sonra başvurmak için özel bir alan oluşturmak için aşağıdaki kodu ekleyin `DropDownList` denetimi:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample18.cs)]

Ardından işleyicileri ekleyin `DropDownList` denetimin `Init` olay ve `GridView` denetimin `RowUpdating` olay:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample19.cs)]

İşleyici için `Init` olay başvuru kaydeder `DropDownList` sınıf alanı denetimi. İşleyici için `RowUpdating` olay başvurusu kullanıcının girdiği değer almak ve uygulamak için kullandığı `Administrator` özelliği `Department` varlık.

Kullanım *DepartmentsAdd.aspx* sayfasında yeni bir bölüm eklemek ve ardından çalıştırın *Departments.aspx* sayfasında ve tıklayın **Düzenle** eklediğiniz satırda.

> [!NOTE]
> Eklemediğiniz satırları düzenlemeniz mümkün olmayacak (diğer bir deyişle, olduğunu zaten veritabanında), veritabanında geçersiz veriler nedeniyle Öğrenciler veritabanı ile oluşturulmuş olan satırlar için yöneticilerdir. Bunlardan biri düzenlemeye çalışırsanız, bir hata sayfası gibi bir hata raporları alırsınız. `'InstructorsDropDownList' has a SelectedValue which is invalid because it does not exist in the list of items.`


[![Image10](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image36.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image35.png)

Geçersiz bir girerseniz **bütçe** tutar ve ardından **güncelleştirme**, aynı yıldız ve gördüğünüz hata iletisi görürsünüz *Departments.aspx* sayfası.

Bir alanın değerini değiştirin veya farklı bir yönetici seçip tıklayın **güncelleştirme**. Değişiklik görüntülenir.

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image38.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image37.png)

Bu kullanmaya giriş tamamlar `ObjectDataSource` denetimi için temel CRUD (oluşturma, okuma, güncelleştirme ve silme) Entity Framework ile işlemleri. Bir basit n katmanlı uygulama derlediğinize göre ancak iş mantığı katmanı otomatik birim testi karmaşıklaştırır veri erişim katmanına hala sıkı bir şekilde bağlı. Aşağıdaki öğreticide birim testi kolaylaştırmak için depo düzeni nasıl uygulayacağınıza karar görürsünüz.

> [!div class="step-by-step"]
> [Next](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
