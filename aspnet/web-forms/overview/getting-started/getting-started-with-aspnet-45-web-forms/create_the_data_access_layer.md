---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create_the_data_access_layer
title: Veri erişim katmanı oluşturun | Microsoft Docs
author: Erikre
description: Bu öğretici seri ASP.NET 4.5 ve Microsoft Visual Studio Express 2013 biz için kullanarak bir ASP.NET Web Forms uygulaması oluşturma temellerini öğretmek...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 0bbf7a6e-d7eb-4091-91e4-fff892777f32
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create_the_data_access_layer
msc.type: authoredcontent
ms.openlocfilehash: 671d1bbf661dfb3e56c6ccd67ce0d383990918d6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30881470"
---
<a name="create-the-data-access-layer"></a>Veri erişim katmanı oluşturma
====================
Tarafından [Erik Reitan](https://github.com/Erikre)

[Wingtip Toys örnek proje (C#) karşıdan](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) veya [karşıdan E-kitap (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Bu öğretici seri ASP.NET 4.5 ve Microsoft Visual Studio Express 2013 için Web kullanarak bir ASP.NET Web Forms uygulaması oluşturma temellerini öğretmek. Visual Studio 2013 [C# kaynak kodu projeyle](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) Bu öğretici seri eşlik etmek üzere hazırdır.


Bu öğretici, oluşturma, erişim ve ASP.NET Web Forms ve Entity Framework Code First kullanarak bir veritabanındaki verileri gözden açıklar. Bu öğretici önceki öğreticiyi "proje oluştur" oluşturur ve Wingtip Oyuncak deposu öğretici serinin bir parçasıdır. Bu öğreticiyi tamamladıktan sonra bir grubu olan veri erişimi sınıfların oluşturacaksınız *modelleri* projenin klasör.

## <a name="what-youll-learn"></a>Öğrenecekleriniz:

- Veri modelleri oluşturma
- Başlatmak ve veritabanı oluşturmak nasıl.
- Güncelleştirme ve veritabanı destekleyecek şekilde yapılandırmak nasıl.

### <a name="these-are-the-features-introduced-in-the-tutorial"></a>Bu öğreticide sunulan özellikler şunlardır:

- Entity Framework Code First
- Yerel Veritabanı
- Veri ek açıklamaları

## <a name="creating-the-data-models"></a>Veri modelleri oluşturma

[Entity Framework](https://msdn.microsoft.com/data/aa937723) bir nesne ilişkisel eşleme (ORM) çerçevedir. Genellikle yazma gerekir veri erişimi kod çoğunu ortadan nesneler olarak ilişkisel verilerle çalışmanıza olanak sağlar. Entity Framework kullanarak, kullanarak sorguları yayınlayabilirsiniz [LINQ](https://msdn.microsoft.com/library/bb397926.aspx), ardından almak ve veri güçlü şekilde yazılan nesnelerin olarak değiştirmek. LINQ sorgulama ve verileri güncelleştirme desenleri sağlar. Entity Framework kullanarak, rest, uygulamanızın oluşturma yerine verilere erişimi temelleri odaklanan odaklanmanıza olanak sağlar. Öğretici serisinde daha sonra bu verileri gezinti ve ürün sorguları doldurmak için nasıl kullanılacağını göstereceğiz.

Entity Framework destekleyen adlı bir geliştirme standardı *Code First*. Kod ilk sınıflarını kullanarak, veri modelleri tanımlamanıza olanak sağlar. Bir sınıf, kendi özel türler değişkenleri diğer türleri, yöntemleri ve olayları bir arada gruplandırma tarafından oluşturmanıza olanak sağlayan bir yapıdır. Map sınıfları varolan bir veritabanını veya bunları bir veritabanı oluşturmak için kullanın. Bu öğreticide, veri modeli sınıflarını yazarak veri modelleri oluşturacaksınız. Ardından, bu yeni sınıflardan kolay bir şekilde veritabanı oluşturma Entity Framework bildiririz.

Web Forms uygulama veri modellerini tanımlayan sınıflar oluşturarak başlar. Ardından varlık sınıflarını yönetir ve veritabanına veri erişimi sağlayan bir bağlam sınıfı oluşturur. Ayrıca, veritabanı doldurmak için kullanacağınız bir başlatıcı sınıfı oluşturun.

### <a name="entity-framework-and-references"></a>Entity Framework ve başvurular

Varsayılan olarak, yeni bir oluşturduğunuzda, Entity Framework bulunur **ASP.NET Web uygulaması** kullanarak **Web Forms** şablonu. Entity Framework yüklü, kaldırılır ve bir NuGet paketi olarak güncelleştirildi.

Bu NuGet paketi şunları içerir **çalışma zamanı** derlemeler projenizin içinde:

- EntityFramework.dll – Entity Framework tarafından kullanılan tüm ortak çalışma zamanı kodu
- EntityFramework.SqlServer.dll – Entity Framework için Microsoft SQL Server sağlayıcısı

### <a name="entity-classes"></a>Varlık sınıfı

Verilerin şemasını tanımlamak için oluşturduğunuz sınıfları sınıflar olarak adlandırılır. Veritabanı tasarım yeniyseniz, varlık sınıflarını bir veritabanı tablo tanımları düşünün. Sınıfın her bir özellik veritabanı tablosunda bir sütun belirtir. Bu sınıfları, nesne yönelimli kod ve veritabanının ilişkisel tablo yapısı arasında basit, nesne ilişkisel bir arabirim sağlar.

Bu öğreticide, ürünler ve kategoriler şemaları temsil eden basit sınıflar ekleyerek başlayacaksınız. Ürünler sınıfı her ürün için tanımları içerir. Her bir ürün sınıfı üyeleri adı `ProductID`, `ProductName`, `Description`, `ImagePath`, `UnitPrice`, `CategoryID`, ve `Category`. Kategori sınıfında bir ürün için otomobil, bot veya uçak gibi ait olabilir. her kategori için tanımları içerir. Her bir kategori sınıfı üyeleri adı `CategoryID`, `CategoryName`, `Description`, ve `Products`. Her ürün kategorileri birine ait. Bu varlık sınıflar projenin varolan eklenecek *modelleri* klasör.

1. İçinde **Çözüm Gezgini**, sağ *modelleri* klasörünü ve ardından **Ekle**  - &gt; **yeni öğe**. 

    ![Veri erişim katmanı - yeni öğe menü oluşturma](create_the_data_access_layer/_static/image1.png)

   **Yeni Öğe Ekle** iletişim kutusu görüntülenir.
2. Altında **Visual C#** gelen **yüklü** seçin sol bölmede **kod**. 

    ![Veri erişim katmanı - yeni öğe menü oluşturma](create_the_data_access_layer/_static/image2.png)
3. Seçin **sınıfı** Orta bölmesinden ve bu yeni sınıf adını *Product.cs*.
4. **Ekle**'yi tıklatın.  
   Yeni sınıf dosyası Düzenleyicisi'nde görüntülenir.
5. Varsayılan kodu aşağıdaki kodla değiştirin:   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample1.cs)]
6. Ancak, ad yeni sınıf 1 ile 4 arasındaki adımları yineleyerek başka bir sınıf oluşturun *Category.cs* ve varsayılan kodu aşağıdaki kodla değiştirin:  

    [!code-csharp[Main](create_the_data_access_layer/samples/sample2.cs)]

Daha önce belirtildiği gibi `Category` sınıf satmak için tasarlanmış uygulama ürün türünü temsil eder (gibi <a id="a"> </a> &quot;araba&quot;, &quot;Boats&quot;, &quot;Rockets&quot;, vb.) ve `Product` sınıfı, veritabanındaki (toys) tek tek ürünlerin temsil eder. Her bir örneğini bir `Product` nesne ilişkisel veritabanı tablo içindeki satır karşılık ve ilişkisel veritabanı tablodaki bir sütun için ürün sınıfın her bir özellik eşler. Daha sonra Bu öğreticide veritabanında bulunan ürün verileri gözden geçirin.

### <a name="data-annotations"></a>Veri ek açıklamaları

Belirli üyeleri sınıfların üye ayrıntılarını belirtme gibi özniteliklere sahip fark etmiş olabilirsiniz `[ScaffoldColumn(false)]`. Bunlar *veri ek açıklamaları*. Veri ek açıklaması öznitelikleri biçimlendirme belirtin ve veritabanı oluşturulduğunda nasıl modellenir belirtmek için bu üye için kullanıcı girişi doğrulamak nasıl açıklayabilirsiniz.

### <a name="context-class"></a>Bağlam Sınıfı

Veri erişimi için sınıfları kullanmaya başlamak için bir bağlam sınıfı tanımlamanız gerekir. Daha önce belirtildiği gibi bağlam sınıfını varlık sınıflarını yönetir (gibi `Product` sınıfı ve `Category` sınıfı) ve veritabanı veri erişim sağlar.

Bu yordam, yeni bir C# bağlamı sınıf için ekler *modelleri* klasör.

1. Sağ *modelleri* klasörünü ve ardından **Ekle**  - &gt; **yeni öğe**.   
   **Yeni Öğe Ekle** iletişim kutusu görüntülenir.
2. Seçin **sınıfı** Orta bölmesinden adlandırın *ProductContext.cs* tıklatıp **Ekle**.
3. Aşağıdaki kod ile sınıfı içinde yer alan varsayılan kod değiştirin:   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample3.cs)]

Bu kod ekler `System.Data.Entity` ad alanı böylece sorgu yeteneği içerir, Entity Framework'ün tüm çekirdek işlevlerin erişimi ekleme, güncelleştirme ve ile güçlü şekilde yazılan nesnelerin çalışarak verilerini sil.

`ProductContext` Sınıfı temsil eder getirme, depolama ve güncelleştirme işleme Entity Framework ürün veritabanı bağlamı `Product` sınıf örnekleri veritabanındaki. `ProductContext` Sınıfı türer `DbContext` temel Entity Framework tarafından sağlanan sınıfı.

### <a name="initializer-class"></a>Başlatıcı sınıfı

İçerik veritabanı ilk kullanıldığında başlatmak için bazı özel mantık çalıştırmanız gerekir. Bu ürünler ve kategoriler hemen görüntüleyebilmesi veritabanına eklenecek çekirdek veri olanak tanır.

Bu yordam, yeni bir C# Başlatıcı sınıf için ekler *modelleri* klasör.

1. Başka birini oluşturmak `Class` içinde *modelleri* klasörü ve adlandırın *ProductDatabaseInitializer.cs*.
2. Aşağıdaki kod ile sınıfı içinde yer alan varsayılan kod değiştirin:   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample4.cs)]

Veritabanı oluşturduğunuzda ve başlatılmadığı, yukarıdaki koddan gördüğünüz `Seed` özelliği geçersiz kılındı ve ayarlayın. Zaman `Seed` özelliği ayarlanmışsa, kategoriler ve ürünler değerleri veritabanı doldurmak için kullanılır. Yukarıdaki kod veritabanı oluşturulduktan sonra değiştirerek çekirdek verileri güncelleştirmek denerseniz, Web uygulamasını çalıştırdığınızda, herhangi bir güncelleştirme görmezsiniz. Yukarıdaki kod kullanan uygulaması nedeni `DropCreateDatabaseIfModelChanges` (şema) modeli çekirdek veri sıfırlamadan önce değişip değişmediğini bilmek sınıfı. Hiçbir değişiklik yapılırsa `Category` ve `Product` sınıflar, veritabanı yeniden çekirdek verilerle.

> [!NOTE] 
> 
> Her zaman yeniden oluşturulması için veritabanını istemenize uygulamayı çalıştırdığınız, kullanabilirsiniz `DropCreateDatabaseAlways` sınıfının yerine `DropCreateDatabaseIfModelChanges` sınıfı. Ancak bu öğretici serisi için kullanmak `DropCreateDatabaseIfModelChanges` sınıfı.


Bu öğreticide, bu noktada gerekir bir *modelleri* dört yeni sınıflar ve bir varsayılan sınıf klasörüyle:

![Veri erişim katmanı - modeller klasörü oluşturulamıyor](create_the_data_access_layer/_static/image3.png)

### <a name="configuring-the-application-to-use-the-data-model"></a>Veri modelini kullanmak için uygulamayı yapılandırma

Verileri temsil eden sınıflar oluşturduğunuza göre uygulamayı sınıfları kullanacak şekilde yapılandırmanız gerekir. İçinde *Global.asax* dosya, modelin başlatan kodu ekleyin. İçinde *Web.config* Dosya Ekle uygulama, veritabanı bildiren bilgiler yeni veri sınıfları tarafından temsil edilen veri depolamak için kullanır. *Global.asax* dosya, uygulama olayları veya yöntemlerini işlemek için kullanılabilir. *Web.config* dosyasının, ASP.NET web uygulamanızın yapılandırmasını denetlemenize olanak verir.

#### <a name="updating-the-globalasax-file"></a>Global.asax dosyası güncelleştiriliyor

Uygulama başladığında veri modelleri başlatmak için güncelleştirecektir `Application_Start` işleyicisinde *Global.asax.cs* dosya.

> [!NOTE] 
> 
> Çözüm Gezgini'nde, her ikisini seçebilirsiniz *Global.asax* dosya veya *Global.asax.cs* dosyayı düzenlemek için *Global.asax.cs* dosya.


1. Sarı vurgulanan aşağıdaki kodu ekleyin `Application_Start` yönteminde *Global.asax.cs* dosya.   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample5.cs?highlight=9-10,22-23)]

> [!NOTE] 
> 
> Tarayıcınız Bu öğretici seri bir tarayıcıda görüntülerken sarı ile vurgulanmış kodu görmek için HTML5 desteklemesi gerekir.


Uygulama başladığında yukarıdaki kodda gösterildiği gibi uygulamayı ilk kez sırasında veri çalışacak Başlatıcı erişilen belirtir. İki ek ad alanlarını erişim için gerekli olan `Database` nesne ve `ProductDatabaseInitializer` nesne.

 Web.Config dosyasını değiştirme 

Veritabanı çekirdek verilerle doldurulduğunda Entity Framework Code First bir veritabanı sizin için bir varsayılan konumda oluşturur ancak kendi bağlantı bilgilerini uygulamanıza eklemek, sağlar veritabanı konumunun denetim. Uygulamanın içinde bir bağlantı dizesi kullanarak bu veritabanı bağlantısı belirttiğiniz *Web.config* proje kökündeki dosya. Yeni bir bağlantı dizesi ekleyerek, veritabanının konumunu yönlendirebilirsiniz (*wingtiptoys.mdf*) uygulamanın veri dizininde oluşturulacak (*uygulama\_veri*), varsayılan yerine konum. Bu değişikliği yapmadan bulmak ve daha sonra Bu öğreticide veritabanı dosyasını incelemek olanak sağlar.

1. İçinde **Çözüm Gezgini**, bulma ve açma *Web.config* dosya.
2. Sarı vurgulanan bağlantı dizesi eklemek `<connectionStrings>` bölümünü *Web.config* gibi dosya:  

    [!code-xml[Main](create_the_data_access_layer/samples/sample6.xml?highlight=4-7)]

Uygulamayı ilk kez çalıştırdığınızda, veritabanı bağlantı dizesi tarafından belirtilen konumda oluşturacaksınız. Ancak uygulamayı çalıştırmadan önce şimdi onu ilk oluşturun.

## <a name="building-the-application"></a>Uygulama Oluşturma

Tüm sınıflar ve Web uygulamanızda değişiklik yapılması çalıştığından emin olmak için uygulamayı oluşturması gerekir.

1. Gelen **hata ayıklama** menüsünde, select **yapı WingtipToys**.  
 **Çıkış** penceresinde görüntülenir ve tüm iyi olursa gördüğünüz bir *başarılı* ileti.  

    ![Veri erişim katmanı - çıkış penceresine oluşturma](create_the_data_access_layer/_static/image4.png)

Bir hata çalıştırırsanız, yukarıdaki adımları yeniden denetleyin. Bilgileri **çıkış** hangi dosya bir sorun varsa ve dosyasında bir değişikliğin gerekli olduğu penceresi gösterilir. Bu bilgileri gözden geçirdikten ve projenizde sabit için yukarıdaki adımları hangi kısmının ihtiyacınız belirlemenize olanak sağlar.

## <a name="summary"></a>Özet

Serinin Bu öğreticide, veri modeli oluşturulan yanı, başlatmak ve veritabanı oluşturmak için kullanılacak bir kod ekledik. Ayrıca, uygulamayı çalıştırdığınızda, veri modelleri kullanmak için uygulamayı yapılandırdınız.

Sonraki öğreticide UI güncelleştirmesi, gezinti ekleyin ve veritabanından veri almak. Bu, bu öğreticide oluşturduğunuz varlık sınıflarını temel alınarak otomatik olarak oluşturulan veritabanı neden olur.

## <a name="additional-resources"></a>Ek Kaynaklar

[Entity Framework genel bakış](https://msdn.microsoft.com/library/bb399567.aspx)   
[ADO.NET Entity Framework Başlangıç Kılavuzu](https://msdn.microsoft.com/data/ee712907)   
[İlk geliştirme Entity Framework kod](http://www.msteched.com/2010/Europe/DEV212) (video)   
[Kod ilk ilişki Fluent API'si](https://msdn.microsoft.com/data/hh134698)   
[Kod ilk veri ek açıklamaları](https://msdn.microsoft.com/data/gg193958)  
[Entity Framework için üretkenlik iyileştirmeleri](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0)

> [!div class="step-by-step"]
> [Önceki](create-the-project.md)
> [sonraki](ui_and_navigation.md)
