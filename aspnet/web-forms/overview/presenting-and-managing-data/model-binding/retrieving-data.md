---
uid: web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
title: "Model bağlama ve web forms alma ve verilerle görüntüleme | Microsoft Docs"
author: tfitzmac
description: "Bu öğretici seri model bağlama kullanarak bir ASP.NET Web Forms projesi ile temel yönlerini gösterir. Model bağlama verileri etkileşim daha fazla düz - sağlar..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 9f24fb82-c7ac-48da-b8e2-51b3da17e365
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
msc.type: authoredcontent
ms.openlocfilehash: e750250285fcb0088da284588d721ac21e73820c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="retrieving-and-displaying-data-with-model-binding-and-web-forms"></a>Model bağlama ve web forms ile verilerini görüntüleme ve alma
====================
tarafından [zel FitzMacken](https://github.com/tfitzmac)

> Bu öğretici seri model bağlama kullanarak bir ASP.NET Web Forms projesi ile temel yönlerini gösterir. Model bağlama verileri etkileşim daha düz iletme ile veri kaynağı nesneleri (örneğin, ObjectDataSource veya SqlDataSource) ilgilenme daha kolaylaştırır. Bu seri tanıtım malzemeleri ile başlar ve sonraki öğreticileri, daha gelişmiş kavramları taşır.
> 
> Model bağlama düzeni tüm veri erişim teknolojisi ile çalışır. Bu öğreticide Entity Framework kullanır, ancak size en alışkın olduğu veri erişim teknolojisi kullanabilirsiniz. GridView, ListView, DetailsView veya FormView denetimi gibi bir veriye bağlı sunucu denetiminden seçerek, güncelleştirme, silme ve veri oluşturma için kullanabileceğiniz yöntemler adlarını belirtin. Bu öğreticide, SelectMethod için bir değer belirtmeniz gerekecektir.
> 
> Bu yöntem içinde veri almak için mantığı sağlar. Sonraki öğreticide UpdateMethod, DeleteMethod ve InsertMethod değerlerini ayarlayın.
> 
> Yapabilecekleriniz [karşıdan](https://go.microsoft.com/fwlink/?LinkId=286116) C# veya vb tamamlanmış projede İndirilebilir kod, Visual Studio 2012 veya Visual Studio 2013 ile çalışır. Bu öğreticide gösterilen Visual Studio 2013 şablon biraz farklıdır Visual Studio 2012 şablonu kullanır.
> 
> Öğreticide uygulamayı Visual Studio'da çalıştırın. Ayrıca uygulama kullanılabilir Internet üzerinden bir barındırma sağlayıcısına dağıtarak yapabilirsiniz. Microsoft, 10 web siteleri için ücretsiz bir web barındırma sunar bir  
>  [Ücretsiz Azure deneme hesabı](https://azure.microsoft.com/free/?WT.mc_id=A443DD604). Azure App Service Web Apps için Visual Studio web projesi dağıtma hakkında daha fazla bilgi için bkz: [Visual Studio kullanarak ASP.NET Web dağıtımı](../../deployment/visual-studio-web-deployment/introduction.md) serisi. Bu öğretici, ayrıca, SQL Server veritabanını Azure SQL veritabanına dağıtmak için Entity Framework Code First Migrations kullanmayı gösterir.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Öğreticide kullanılan yazılım sürümleri
> 
> 
> - Microsoft Visual Studio 2013 veya Microsoft Visual Studio Express 2013 için Web
>   
> 
> Bu öğreticide Visual Studio 2012 ile de çalışır, ancak kullanıcı arabirimi ve proje şablonu bazı farklılıklar olacaktır.


## <a name="what-youll-build"></a>Yapı

Bu öğreticide, gerekir:

1. Öğrenciler kurslar içinde kayıtlı olan bir üniversite yansıtacak veri nesneleri oluşturma
2. Derleme nesnelerinden veritabanı tabloları
3. Veritabanını test verilerle doldurma
4. Bir web formunda veri görüntüleme

## <a name="set-up-project"></a>Projesi ayarlayın

Visual Studio 2013'te yeni bir oluşturma **ASP.NET Web uygulaması** adlı **ContosoUniversityModelBinding**.

![Proje oluşturma](retrieving-data/_static/image2.png)

Web Forms şablonu seçin ve diğer varsayılan seçenekleri bırakın. Kurulum projesi için Tamam'ı tıklatın.

![Web formları seçin](retrieving-data/_static/image3.png)

İlk olarak, birkaç küçük site görünümünü özelleştirmek için değişiklik yapar. Açık **Site.Master** dosya ve Contoso University My ASP.NET Application yerine eklenecek başlık değiştirin.

[!code-aspx[Main](retrieving-data/samples/sample1.aspx?highlight=1)]

Ardından, üstbilgi metni değiştirme **uygulama adı** için **Contoso University**.

[!code-aspx[Main](retrieving-data/samples/sample2.aspx?highlight=7)]

Ayrıca bu siteye ilgili sayfaları yansıtacak şekilde üstbilgisinde görünen Gezinti bağlantıları Site.Master değiştirin. Ya da gerekmez **hakkında** sayfa veya **kişi** bu bağlantıları kaldırılabilmesi için sayfa. Bunun yerine, adlı bir sayfaya bir bağlantı gerekir **Öğrenciler**. Bu sayfa henüz oluşturulmadı.

[!code-aspx[Main](retrieving-data/samples/sample3.aspx)]

Kaydedin ve Site.Master kapatın.

Şimdi, Öğrenci verileri görüntülemek için web formu oluşturacaksınız. Projenize sağ tıklayın ve **Ekle** bir **yeni öğe**. Seçin **ana sayfa ile Web Form** şablonu ve adlandırın **Students.aspx**.

![Sayfa oluşturma](retrieving-data/_static/image5.png)

Seçin **Site.Master** yeni bir web formu için ana sayfa olarak.

## <a name="create-the-data-models-and-database"></a>Veritabanı ve veri modelleri oluşturma

Nesneleri ve ilgili veritabanı tablolarını oluşturmak için Code First geçişleri kullanır. Bu tablolar Öğrenciler ve bunların kurslar hakkında bilgi depolar.

Modeller klasörü adlı yeni bir sınıf ekleyin **UniversityModels.cs**.

![Model sınıfı oluşturma](retrieving-data/_static/image7.png)

Bu dosyada, SchoolContext, Öğrenci, kayıt ve indirmelere sınıfları aşağıdaki gibi tanımlayın:

[!code-csharp[Main](retrieving-data/samples/sample4.cs)]

Veritabanı bağlantısı ve verilerde yapılan değişiklikler yönetir DbContext SchoolContext sınıfı türetir.

Öğrenci sınıfında uygulanmış öznitelikleri fark **FirstName**, **LastName**, ve **yıl** özellikleri. Bu öznitelikler, bu öğreticide veri doğrulaması için kullanılır. Bu sunum proje kodunu basitleştirmek için yalnızca bu özellikler veri doğrulama özniteliklerle işaretlenmiş. Gerçek bir proje ile kayıt ve indirmelere sınıfları özelliklerinde gibi doğrulanmış veriler gereken tüm özellikler için doğrulama öznitelikleri geçerli olur.

UniversityModels.cs kaydedin.

Bu sınıflara göre bir veritabanını ayarlamak için Code First geçişleri için araçları kullanır.

İçinde **Paket Yöneticisi Konsolu**, komutu çalıştırın:  
`enable-migrations -ContextTypeName ContosoUniversityModelBinding.Models.SchoolContext`

Komut başarıyla tamamlarsa geçişler etkin bildiren bir ileti alırsınız,

![geçişleri etkinleştir](retrieving-data/_static/image8.png)

Yeni bir dosya adlı bildirim **Configuration.cs** oluşturuldu. Visual Studio'da oluşturulduktan sonra bu dosyayı otomatik olarak açılır. Yapılandırma sınıfı içeren bir **çekirdek** test verileri veritabanı tablolarıyla önceden doldurmak sağlayan yöntemi.

Çekirdek yöntemine aşağıdaki kodu ekleyin. Eklemeniz gerekir bir **kullanarak** bildirimi **ContosoUniversityModelBinding.Models** ad alanı.

[!code-csharp[Main](retrieving-data/samples/sample5.cs)]

Configuration.cs kaydedin.

Paket Yöneticisi konsolunda komutu çalıştırmak `add-migration initial`.

Komutu çalıştırın `update-database`.

Bu komutu çalıştırırken özel durum almaya devam ederseniz, StudentID ve CourseID değerlerini Seed yöntemi değerlerinden farklılık mümkündür. Veritabanında bu tablolar açın ve StudentID ve CourseID için var olan değerleri bulur. Bu değerleri kayıtları tablo dengeli kodunu ekleyin.

Veritabanı dosyası eklendi ancak şu anda projede gizlenir. Tıklatın **tüm dosyaları göster** dosyayı görüntüleyemiyor.

![Tüm dosyaları göster](retrieving-data/_static/image10.png)

.Mdf dosyasını şimdi uygulamada göründüğüne dikkat edin\_veri klasörü.

![Veritabanı dosyası](retrieving-data/_static/image12.png)

.Mdf dosyasını çift tıklatın ve sunucu Gezgini'ni açın. Tablo artık mevcut ve verilerle doldurulur.

![veritabanı tabloları](retrieving-data/_static/image14.png)

## <a name="display-data-from-students-and-related-tables"></a>Öğrenciler ve ilgili tablolardaki verileri görüntüleme

Veritabanındaki verileri ile artık bu verileri almak ve bir web sayfasında görüntülemek hazırsınız. Kullanacağınız bir **GridView** sütunları ve satırları verileri görüntülemek için denetim.

Students.aspx açarak **MainContent** yer tutucu. Bu yer tutucu içinde eklemek bir **GridView** aşağıdaki kodu içeren denetim.

[!code-aspx[Main](retrieving-data/samples/sample6.aspx)]

Birkaç önemli kavramlar, fark bu biçimlendirme kodda vardır. İlk olarak, bir değer için ayarlanmış fark **SelectMethod** GridView öğesindeki özellik. Bu değer için GridView veri almak için kullanılan yöntemin adını belirtir. Bu yöntem sonraki adımda oluşturur. İkinci olarak, dikkat **ItemType** özelliği, daha önce oluşturduğunuz Öğrenci sınıfı ayarlanır. Bu değer ayarlanarak biçimlendirme kodu o sınıfın özelliklerine başvurabilirsiniz. Örneğin, Öğrenci sınıfı kayıtları adlı bir koleksiyonu içerir. Kullanabileceğiniz **Item.Enrollments** o koleksiyona almak ve her Öğrenci için kayıtlı kredi toplamını almak için LINQ sözdizimini kullanın.

Arka plan kod dosyasına için belirtilen yöntemi eklemeniz gerekir **SelectMethod** değeri. Açık **Students.aspx.cs**ve ekleme **kullanarak** deyimleri için **ContosoUniversityModelBinding.Models** ve **System.Data.Entity**ad alanları.

[!code-csharp[Main](retrieving-data/samples/sample7.cs)]

Ardından, aşağıdaki yöntemi ekleyin. Bu yöntemin adını SelectMethod için sağlanan adıyla eşleşen dikkat edin.

[!code-csharp[Main](retrieving-data/samples/sample8.cs)]

**INCLUDE** yan tümcesi bu sorgu performansını artırır ancak sorgu çalışması gerekli değildir. INCLUDE yan tümcesi olmadan verileri veri alındığını her veritabanı için ayrı bir sorgu ilgili gönderme içerir geç yükleme kullanılarak alınması. INCLUDE yan tümcesini sağlayarak, ilgili verilerin tümü tek bir veritabanı sorgu alınır anlamına gelir istekli yükleme kullanarak verileri alınır. İlgili verilerin çoğu olduğu olmaması durumlarda daha fazla veri alındığından kullanılan, istekli yüklenmesi daha az verimli olabilir. İlgili verileri her kayıt için görüntülendiğinden ancak, bu durumda, istekli yüklenirken en iyi performans sağlar.

İlgili verileri yüklenirken performans açıklamasında hakkında daha fazla bilgi için başlıklı bölüme bakın **Lazy, Eager ve açık, ilgili veri yükleme** içinde [bir ASP Entity Framework ile ilgili verileri okuma .NET MVC uygulaması](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) konu.

Varsayılan olarak, veriler anahtar olarak işaretlenmiş özelliği değerlere göre sıralanır. OrderBy yan tümcesinin sıralama için farklı bir değer belirtmek için ekleyebilirsiniz. Bu örnekte, varsayılan StudentID özellik, sıralama için kullanılır. İçinde [sıralama, disk belleği ve filtreleme verilerini](sorting-paging-and-filtering-data.md) konu, sıralamak için sütun seçmek kullanıcı etkinleştirecek.

Web uygulamanızı çalıştırın ve öğrenciler sayfasına gidin. Öğrenciler sayfası aşağıdaki Öğrenci bilgileri görüntüler.

![Verileri göster](retrieving-data/_static/image16.png)

## <a name="automatic-generation-of-model-binding-methods"></a>Model bağlama yöntemlerinin otomatik oluşturma

Bu öğretici seri ile çalışırken, projenize tamamen kod yalnızca kopyalayabilirsiniz. Ancak, bu yaklaşımın bir dezavantajı, model bağlama yöntemleri için kodu otomatik olarak oluşturmak için Visual Studio tarafından sağlanan özellik farkında olmaktan emin olur. Otomatik kod oluşturma kendi projelerde çalışırken, zaman ve nasıl bir işlem uygulanacağı bir fikir edinmek Yardım kaydedebilirsiniz. Bu bölümde otomatik kod oluşturma özelliğini açıklar. Bu bölüm, yalnızca bilgi amaçlıdır ve projenizde uygulamak için gereken herhangi bir kod içermiyor.

İçin bir değer ayarlarken **SelectMethod**, **UpdateMethod**, **InsertMethod**, veya **DeleteMethod** biçimlendirme kodda özellikleri seçebileceğiniz **yeni yöntem oluşturma** seçeneği.

![Yeni bir yöntem oluşturma](retrieving-data/_static/image18.png)

Visual Studio yalnızca arka plan kod uygun imzalı bir yöntem oluşturur, ancak ayrıca işlemi gerçekleştiren ile yardımcı olmak üzere uygulama kodu oluşturur. İlk ayarlarsanız **ItemType** otomatik kod oluşturma özelliği, oluşturulan kodu kullanmadan önce özelliği bu tür işlemler için kullanır. Örneğin, UpdateMethod özelliğini ayarlarken, aşağıdaki kodu otomatik olarak oluşturulur:

[!code-csharp[Main](retrieving-data/samples/sample9.cs)]

Yeniden projenize eklemek Yukarıdaki kod gerekmez. Sonraki öğreticide, güncelleştirme, silme ve yeni veri eklemek için yöntemleri gerçekleştireceksiniz.

## <a name="conclusion"></a>Sonuç

Bu öğreticide, veri modeli sınıflarını oluşturulur ve bu sınıflardan bir veritabanı oluşturulur. Veritabanı tablolarını test verilerle doldurulur. Model bağlama veritabanından veri almak için kullanılır ve ardından verileri bir GridView görüntülenir.

Sonraki [öğretici](updating-deleting-and-creating-data.md) bu dizide, güncelleştirme, silme ve veri oluşturma olanak tanır.

>[!div class="step-by-step"]
[Sonraki](updating-deleting-and-creating-data.md)
