---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create_the_data_access_layer
title: Veri erişim katmanını oluşturma | Microsoft Docs
author: Erikre
description: Bu öğretici serisinin ASP.NET 4.5 ve Visual Studio 2013 Express için kullandığımız bir ASP.NET Web Forms uygulaması oluşturmaya yönelik temel bilgiler sağlanır...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 0bbf7a6e-d7eb-4091-91e4-fff892777f32
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create_the_data_access_layer
msc.type: authoredcontent
ms.openlocfilehash: 5b4bb5bc89938836bc37e8ebd385fa966bc6f511
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37390050"
---
<a name="create-the-data-access-layer"></a>Veri erişim katmanını oluşturma
====================
tarafından [Erik Reitan](https://github.com/Erikre)

[Wingtip Toys örnek projeyi (C#) indirin](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) veya [indirme E-kitabı (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Bu öğretici serisinin Web için ASP.NET 4.5 ve Visual Studio 2013 Express kullanarak bir ASP.NET Web Forms uygulaması oluşturmaya yönelik temel bilgiler sağlanır. Bir Visual Studio 2013'ün [C# kaynak kodu ile proje](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) Bu öğretici serisinin eşlik etmek üzere hazırdır.


Bu öğreticide, oluşturma, erişim ve ASP.NET Web Forms ve Entity Framework Code First kullanarak veritabanındaki verileri gözden açıklar. Bu öğreticide, önceki öğreticide "proje oluştur" oluşturur ve Wingtip çocuğunun Store öğretici serisinin bir parçasıdır. Bu öğreticiyi tamamladıktan sonra bulunan veri erişim sınıfları bir grup oluşturacaksınız *modelleri* proje klasörü.

## <a name="what-youll-learn"></a>Öğrenecekleriniz:

- Veri modelleri oluşturma
- Nasıl başlatmak ve veritabanının çekirdeğini oluşturma.
- Nasıl güncelleştirmek ve veritabanı destekleyecek şekilde yapılandırın.

### <a name="these-are-the-features-introduced-in-the-tutorial"></a>Bu öğreticide sunulan özellikler şunlardır:

- Entity Framework Code First
- Yerel Veritabanı
- Veri ek açıklamaları

## <a name="creating-the-data-models"></a>Veri modelleri oluşturma

[Entity Framework](https://msdn.microsoft.com/data/aa937723) bir nesne ilişkisel eşleme (ORM) çerçevesidir. Genellikle yazmanız gerekirdi veri erişim kodu çoğunu ortadan nesneler olarak, ilişkisel verilerle çalışacak olanak tanır. Entity Framework kullanarak kullanarak sorgu iletebilirsiniz [LINQ](https://msdn.microsoft.com/library/bb397926.aspx), ardından almak ve veri türü kesin olarak belirtilmiş nesneler olarak yönetmek. LINQ verileri sorgulama ve güncelleştirmeye yönelik Desenler sağlar. Entity Framework kullanarak uygulamanızı geri kalanını oluşturmak yerine verilere erişim ile ilgili temel bilgiler odaklanarak odaklanmanızı sağlar. Daha sonra Bu öğretici serisinde, veri gezinti ve ürün sorguları doldurmak için nasıl kullanılacağını göstereceğiz.

Entity Framework destekleyen adlı bir geliştirme paradigma *Code First*. Kod ilk veri modellerinize sınıfları tanımlamanıza olanak sağlar. Bir sınıf kendi özel türlerinizde diğer türleri, yöntemleri ve olayları değişkenleri birlikte gruplandırarak oluşturmanızı sağlayan bir yapıdır. Mevcut bir veritabanına sınıflarını eşlemek veya bir veritabanı oluşturmak için bunları kullanın. Bu öğreticide, veri modeli sınıfları yazarak veri modellerini oluşturacaksınız. Ardından, bu yeni sınıflardan hareket halindeyken veritabanı oluşturma Entity Framework değiştirebilmesini sağlayacaksınız.

Web Forms uygulaması için veri modellerini tanımlayan varlık sınıfları oluşturarak başlar. Ardından, varlık sınıflarının yönetir ve veritabanına veri erişimi sağlayan bir bağlam sınıf oluşturacaksınız. Ayrıca, veritabanını doldurmak için kullanacağınız bir başlatıcı sınıfı oluşturur.

### <a name="entity-framework-and-references"></a>Entity Framework ve başvurular

Yeni bir oluşturduğunuzda varsayılan olarak Entity Framework dahil **ASP.NET Web uygulaması** kullanarak **Web Forms** şablonu. Entity Framework yüklü, kaldırılır ve bir NuGet paketi olarak güncelleştirildi.

Bu NuGet paketi şunlardır **çalışma zamanı** derlemelerini projenizin içinde:

- EntityFramework.dll – Entity Framework tarafından kullanılan tüm ortak çalışma zamanı kodu
- EntityFramework.SqlServer.dll – Entity Framework için Microsoft SQL Server sağlayıcısı

### <a name="entity-classes"></a>Varlık sınıfları

Varlık sınıflarının veri şemasını tanımlamak için oluşturduğunuz sınıfları çağrılır. Veritabanı tasarımı yeniyseniz, varlık sınıflarının bir veritabanı tablosu tanımları düşünün. Sınıfı her bir özellik veritabanı tablosunda bir sütun belirtir. Bu sınıf, nesne yönelimli kodu ve veritabanı, ilişkisel tablo yapısı arasında hafif, nesne ilişkisel bir arabirim sağlar.

Bu öğreticide, ürünler ve kategoriler için şemalar gösteren basit bir varlık sınıfları ekleyerek başlar. Ürünleri sınıfı her ürün için tanımları içerir. Her bir ürün sınıf adı olacaktır `ProductID`, `ProductName`, `Description`, `ImagePath`, `UnitPrice`, `CategoryID`, ve `Category`. Kategori sınıfında bir ürün, araba, bot veya düzlemi gibi ait olabilir her kategori için tanımları içerir. Her kategori sınıf üyelerini bir adı olacaktır `CategoryID`, `CategoryName`, `Description`, ve `Products`. Her ürün kategorilerden birine ait olur. Bu varlık sınıfları, projenin varolan eklenecek *modelleri* klasör.

1. İçinde **Çözüm Gezgini**, sağ *modelleri* klasörünü ve ardından **Ekle**  - &gt; **yeni öğe**. 

    ![Veri erişim katmanı - yeni öğe menü oluşturma](create_the_data_access_layer/_static/image1.png)

   **Yeni Öğe Ekle** iletişim kutusu görüntülenir.
2. Altında **Visual C#** gelen **yüklü** seçin sol bölmede **kod**. 

    ![Veri erişim katmanı - yeni öğe menü oluşturma](create_the_data_access_layer/_static/image2.png)
3. Seçin **sınıfı** Orta bölmeden ve bu yeni bir sınıf adı *Product.cs*.
4. **Ekle**'yi tıklatın.  
   Yeni bir sınıf dosyası Düzenleyicisi'nde görüntülenir.
5. Varsayılan kodu aşağıdaki kodla değiştirin:   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample1.cs)]
6. Ancak ad yeni bir sınıf 1 ile 4 arasındaki adımları tekrarlayarak başka bir sınıf oluşturun *Category.cs* varsayılan kodu aşağıdaki kodla değiştirin:  

    [!code-csharp[Main](create_the_data_access_layer/samples/sample2.cs)]

Daha önce belirtildiği gibi `Category` sınıf uygulama ürün türü satmak üzere tasarlanan temsil eder (gibi <a id="a"> </a> &quot;otomobiller&quot;, &quot;Boats&quot;, &quot;Rockets&quot;, vb.) ve `Product` sınıf veritabanında tek tek ürünlerin (toys) temsil eder. Her bir örneği bir `Product` nesne ilişkisel veritabanı tablosu içinde bir satıra karşılık gelen ve ilişkisel veritabanı tablosunda bir sütun için ürün sınıfın her bir özellik eşler. Bu öğreticinin ilerleyen bölümlerinde veritabanında yer alan ürün verileri gözden geçirelim.

### <a name="data-annotations"></a>Veri ek açıklamaları

Belirli üyeleri sınıfların üye hakkındaki ayrıntıları gibi belirten öznitelikler olduğunu fark etmiş olabilirsiniz `[ScaffoldColumn(false)]`. Bunlar *veri ek açıklamaları*. Veri ek açıklama öznitelikleri biçimlendirme belirtin ve veritabanını oluştururken nasıl modellenmiştir belirtmek için bu üye için kullanıcı girişini doğrulama açıklayabilirsiniz.

### <a name="context-class"></a>Bağlam Sınıfı

İçin veri erişim sınıfları kullanmaya başlamak için bir bağlam sınıfı tanımlamanız gerekir. Daha önce belirtildiği gibi varlık sınıflarının bağlam sınıfını yönetir (gibi `Product` sınıfı ve `Category` sınıfı) ve veritabanı veri erişim sağlar.

Bu yordam, yeni C# bağlamı sınıfı için ekler *modelleri* klasör.

1. Sağ *modelleri* klasörünü ve ardından **Ekle**  - &gt; **yeni öğe**.   
   **Yeni Öğe Ekle** iletişim kutusu görüntülenir.
2. Seçin **sınıfı** Orta bölmesinden adlandırın *ProductContext.cs* tıklatıp **Ekle**.
3. Aşağıdaki kod ile bir sınıf içindeki varsayılan kodu değiştirin:   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample3.cs)]

Bu kod ekler `System.Data.Entity` ad alanı sorgu özelliği içerir, Entity Framework'ün tüm çekirdek işlevlerin erişiminiz olduğunu ekleme, güncelleştirme ve kesin olarak belirlenmiş nesneler ile çalışma ile verileri silin.

`ProductContext` Sınıfı temsil eder, alma, depolama ve güncelleştirme işleme Entity Framework ürün veritabanı bağlamı `Product` sınıfı veritabanında örnekleri. `ProductContext` Sınıf türetilir `DbContext` temel Entity Framework tarafından sağlanan sınıfı.

### <a name="initializer-class"></a>Başlatıcı sınıfı

İçerik veritabanı ilk kullanıldığında başlatmak için özel bir mantık çalıştırmanız gerekir. Bu ürünler ve kategoriler hemen görüntüleyebilmesi veritabanına eklenmesi çekirdek veri olanak tanır.

Bu yordam, yeni bir C# Başlatıcı sınıf için ekler *modelleri* klasör.

1. Hesaplanmış sütun oluşturabiliriz `Class` içinde *modelleri* klasörünü adlandırın *ProductDatabaseInitializer.cs*.
2. Aşağıdaki kod ile bir sınıf içindeki varsayılan kodu değiştirin:   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample4.cs)]

Veritabanı oluşturduğunuzda ve başlatılır, yukarıdaki koddan gördüğünüz gibi `Seed` özelliği geçersiz kılındı ve ayarlayın. Zaman `Seed` özelliği ayarlanmışsa, değerler kategorilerini ve ürünler veritabanını doldurmak için kullanılır. Veritabanı oluşturulduktan sonra yukarıdaki kod değiştirerek çekirdek verileri güncelleştirmek çalışırsanız, Web uygulamasını çalıştırdığınızda, herhangi bir güncelleştirme görmezsiniz. Yukarıdaki kodda uygulaması nedeni `DropCreateDatabaseIfModelChanges` (şema) modeli çekirdek veri sıfırlamadan değişip değişmediğini bilmek sınıfı. Herhangi bir değişiklik yapılırsa `Category` ve `Product` varlık sınıfları, veritabanı yeniden çekirdek verilerle.

> [!NOTE] 
> 
> Her zaman yeniden oluşturulması veritabanı istediyseniz, uygulamayı çalıştırdığınız, kullanabileceğinizi `DropCreateDatabaseAlways` sınıfı yerine `DropCreateDatabaseIfModelChanges` sınıfı. Ancak bu öğretici serisinin kullanmanız `DropCreateDatabaseIfModelChanges` sınıfı.


Bu öğreticide, bu noktada olması bir *modelleri* dört yeni sınıflar ve sınıf bir varsayılan klasör:

![Veri erişim katmanı - modeller klasörü oluşturma](create_the_data_access_layer/_static/image3.png)

### <a name="configuring-the-application-to-use-the-data-model"></a>Veri modelini kullanmak için uygulamayı yapılandırma

Verileri temsil eden sınıfları oluşturduğunuza göre uygulamayı sınıflarını kullanacak şekilde yapılandırmanız gerekir. İçinde *Global.asax* dosyası, model başlatan kodu ekleyin. İçinde *Web.config* eklediğiniz uygulama, veritabanı bildiren bilgiler, yeni veri sınıfları tarafından temsil edilen verilerini depolamak için kullanacağınız dosya. *Global.asax* dosya, uygulama olayları veya yöntemleri işlemek için kullanılabilir. *Web.config* dosya, ASP.NET web uygulamanızı yapılandırmasını denetlemenize olanak tanır.

#### <a name="updating-the-globalasax-file"></a>Global.asax dosyası güncelleştiriliyor

Uygulama başladığında veri modellerini başlatmak için güncelleştirecektir `Application_Start` işleyicisinde *Global.asax.cs* dosya.

> [!NOTE] 
> 
> Çözüm Gezgini'nde seçebilirsiniz *Global.asax* dosya veya *Global.asax.cs* dosyayı düzenlemek için *Global.asax.cs* dosya.


1. Sarı ile vurgulanmış aşağıdaki kodu ekleyin `Application_Start` yönteminde *Global.asax.cs* dosya.   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample5.cs?highlight=9-10,22-23)]

> [!NOTE] 
> 
> Tarayıcınız HTML5, Bu öğretici serisinin bir tarayıcıda görüntülerken, sarı ile vurgulanmış kodu görmek için desteklemesi gerekir.


Uygulama başladığında yukarıdaki kodda gösterildiği gibi uygulama, verileri ilk kez sırasında çalıştırılacak Başlatıcı erişilen belirtir. İki ek ad alanları erişim için gerekli olan `Database` nesne ve `ProductDatabaseInitializer` nesne.

 Web.Config dosyasını değiştirme 

Veritabanı çekirdek verilerle doldurulur, Entity Framework Code First bir veritabanı varsayılan konumunda oluşturacaktır olsa da, kendi bağlantı bilgilerini uygulamanıza ekleme size denetimi veritabanı konumu. Bu veritabanı bağlantısı içinde uygulamanın bir bağlantı dizesi kullanarak belirttiğiniz *Web.config* projenin köküne dosya. Yeni bir bağlantı dizesi ekleyerek, veritabanının konumunu yönlendirebilir (*wingtiptoys.mdf*) uygulamanın veri dizininde oluşturulacak (*uygulama\_veri*), varsayılan yerine konum. Bu değişiklik, bulun ve bu öğreticinin ilerleyen bölümlerinde veritabanı dosyasını incelemek izin verir.

1. İçinde **Çözüm Gezgini**, bulma ve açma *Web.config* dosya.
2. Sarı ile vurgulanmış bağlantı dizesi Ekle `<connectionStrings>` bölümünü *Web.config* aşağıdaki gibi:  

    [!code-xml[Main](create_the_data_access_layer/samples/sample6.xml?highlight=4-7)]

Uygulamayı ilk kez çalıştırdığınızda, veritabanı bağlantı dizesi tarafından belirtilen konumda oluşturacaksınız. Ancak, uygulamayı çalıştırmadan önce ilk oluşturalım.

## <a name="building-the-application"></a>Uygulama Oluşturma

Tüm sınıfları ve değişiklikleri Web uygulamanıza çalıştığından emin olmak için uygulamayı oluşturması gerekir.

1. Gelen **hata ayıklama** menüsünde **derleme WingtipToys**.  
 **Çıkış** penceresi görüntülenir ve tüm iyi gittiği varsa, bir *başarılı* ileti.  

    ![Veri erişim katmanı - çıkış Windows oluşturma](create_the_data_access_layer/_static/image4.png)

Bir hata çalıştırırsanız, yukarıdaki adımları yeniden kontrol edin. Bilgileri **çıkış** dosyasında bir değişiklik gerekli olduğu ve hangi dosyayı sorunlu penceresinde gösterecektir. Bu bilgiler yukarıdaki adımları hangi parçası gerekiyor gözden geçirilebilir ve projenizde sabit belirlemenize olanak sağlar.

## <a name="summary"></a>Özet

Serinin Bu öğreticide, veri modeli oluşturulan yanı, başlatmak ve veritabanının çekirdeğini oluşturma için kullanılacak bir kod ekledik. Ayrıca, uygulamayı uygulamayı çalıştırdığınızda veri modellerini kullanacak şekilde yapılandırdınız.

Sonraki öğreticide, kullanıcı arabirimini güncelleştirme, gezinti ekleyin ve veritabanından veri alınamıyor. Bu, bu öğreticide oluşturduğunuz varlık sınıflarının temel alınarak otomatik olarak oluşturulan veritabanı neden olur.

## <a name="additional-resources"></a>Ek Kaynaklar

[Entity Framework'e Genel Bakış](https://msdn.microsoft.com/library/bb399567.aspx)   
[ADO.NET Entity Framework için Başlangıç Kılavuzu](https://msdn.microsoft.com/data/ee712907)   
[Entity Framework ile ilk geliştirme kod](http://www.msteched.com/2010/Europe/DEV212) (video)   
[Kod ilk ilişkileri Fluent API'si](https://msdn.microsoft.com/data/hh134698)   
[Kod ilk veri ek açıklamaları](https://msdn.microsoft.com/data/gg193958)  
[Entity Framework için üretkenlik geliştirmeleri](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0)

> [!div class="step-by-step"]
> [Önceki](create-the-project.md)
> [İleri](ui_and_navigation.md)
