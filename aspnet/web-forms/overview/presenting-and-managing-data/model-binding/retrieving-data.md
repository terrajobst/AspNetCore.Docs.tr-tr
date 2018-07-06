---
uid: web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
title: Model bağlama ve web forms ile verileri alma ve görüntüleme | Microsoft Docs
author: tfitzmac
description: Bu öğretici serisinde, model bağlama kullanarak bir ASP.NET Web formları projesi ile temel yönlerini gösterir. Model bağlama veri etkileşimi daha fazla düz - sağlar...
ms.author: aspnetcontent
ms.date: 02/27/2014
ms.assetid: 9f24fb82-c7ac-48da-b8e2-51b3da17e365
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
msc.type: authoredcontent
ms.openlocfilehash: 8153642762cf7032f335d21c8c67eac9b52ed2f8
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37823929"
---
<a name="retrieving-and-displaying-data-with-model-binding-and-web-forms"></a>Model bağlama ve web forms ile verileri alma ve görüntüleme
====================
tarafından [Tom FitzMacken](https://github.com/tfitzmac)

> Bu öğretici serisinde, model bağlama kullanarak bir ASP.NET Web formları projesi ile temel yönlerini gösterir. Model bağlama, daha doğru verilerle ilgili kaynak nesne (örneğin, ObjectDataSource veya SqlDataSource) daha veri etkileşim sağlar. Bu seri, tanıtım malzemeleri ile başlar ve sonraki öğreticilerde için daha gelişmiş kavramlar taşır.
> 
> Model bağlama deseni, tüm veri erişim teknolojisi ile çalışır. Bu öğreticide, Entity Framework kullanır, ancak en tanıdık veri erişim teknolojisi kullanabilirsiniz. GridView, ListView, DetailsView veya FormView denetimi gibi bir verilere bağlı sunucu denetimden seçme, güncelleştirme, silme ve veri oluşturmak için kullanabileceğiniz yöntemler adlarını belirtin. Bu öğreticide, SelectMethod için bir değer belirtmeniz.
> 
> Bu yöntem içinde veri almak için mantığı sağlar. Sonraki öğreticide UpdateMethod, DeleteMethod ve InsertMethod değerleri ayarlanır.
> 
> Yapabilecekleriniz [indirme](https://go.microsoft.com/fwlink/?LinkId=286116) tam projeyi C# veya vb İndirilebilir kod, Visual Studio 2012 veya Visual Studio 2013 ile çalışır. Bu öğreticide gösterilen Visual Studio 2013 şablonundan biraz farklıdır Visual Studio 2012 şablonu kullanır.
> 
> Öğreticide uygulamayı Visual Studio'da çalıştırın. Ayrıca uygulama kullanılabilir Internet üzerinden bir barındırma sağlayıcısına dağıtarak yapabilirsiniz. Microsoft'un sunduğu en fazla 10 web siteleri için ücretsiz bir web barındırma bir  
>  [Ücretsiz Azure deneme hesabı](https://azure.microsoft.com/free/?WT.mc_id=A443DD604). Visual Studio web projesini Azure App Service Web Apps'e dağıtma hakkında daha fazla bilgi için bkz: [Visual Studio kullanarak ASP.NET Web dağıtımı](../../deployment/visual-studio-web-deployment/introduction.md) serisi. Bu öğretici ayrıca SQL Server veritabanınızı Azure SQL veritabanı'na dağıtmak için Entity Framework Code First Migrations kullanmayı gösterir.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Bu öğreticide kullanılan yazılım sürümleri
> 
> 
> - Microsoft Visual Studio 2013 veya Microsoft Visual Studio Web için Express 2013
>   
> 
> Bu öğreticide Visual Studio 2012 ile de çalışır ancak kullanıcı arabirimi ve proje şablonu bazı farklılıklar olacaktır.


## <a name="what-youll-build"></a>Derleme

Bu öğreticide, gerekir:

1. Üniversite öğrencileri eğitim kayıtlı yansıtan veri nesneleri oluşturma
2. Derleme nesnelerden veritabanı tabloları
3. Veritabanı test verileri ile doldurma
4. Bir web formunda veri görüntüleme

## <a name="set-up-project"></a>Projesi kurun

Visual Studio 2013'te yeni bir oluşturma **ASP.NET Web uygulaması** adlı **ContosoUniversityModelBinding**.

![Proje oluşturma](retrieving-data/_static/image2.png)

Web Forms şablonu seçin ve diğer varsayılan seçenekleri bırakın. Kurulum projesi için Tamam'a tıklayın.

![Web formları seçin](retrieving-data/_static/image3.png)

İlk olarak birkaç site görünümünü özelleştirmek için küçük değişiklik yapar. Açık **Site.Master** dosya ve başlığı Contoso University My ASP.NET Application yerine içerecek şekilde değiştirin.

[!code-aspx[Main](retrieving-data/samples/sample1.aspx?highlight=1)]

Ardından, üst bilgi metni değiştirme **uygulama adı** için **Contoso University**.

[!code-aspx[Main](retrieving-data/samples/sample2.aspx?highlight=7)]

Ayrıca bu site için uygun olan sayfaları yansıtacak şekilde üstbilgi görünür Gezinti bağlantıları Site.Master değiştirin. Ya da ihtiyacınız olacak değil **hakkında** sayfası veya **kişi** bu bağlantıları kaldırılabilmesi için sayfa. Bunun yerine, adlı bir sayfaya bağlantı gerekir **Öğrenciler**. Bu sayfa henüz oluşturulmamış.

[!code-aspx[Main](retrieving-data/samples/sample3.aspx)]

Kaydedip Site.Master kapatın.

Şimdi, Öğrenci verilerinin görüntülemek için web formu oluşturacaksınız. Projenize sağ tıklayın ve **Ekle** bir **yeni öğe**. Seçin **ana sayfa ile Web formu** şablonunu ve adlandırın **Students.aspx**.

![sayfası oluşturma](retrieving-data/_static/image5.png)

Seçin **Site.Master** yeni bir web formu için ana sayfa olarak.

## <a name="create-the-data-models-and-database"></a>Veritabanı ve veri modelleri oluşturma

Nesneler ve ilgili veritabanı tablolarını oluşturmak için Code First Migrations'ı kullanır. Bu tablolar, Öğrenciler ve bunların kurslar ilgili bilgileri saklar.

Modelleri klasöründe adlı yeni bir sınıf ekleyin **UniversityModels.cs**.

![Model sınıfı oluşturma](retrieving-data/_static/image7.png)

Bu dosyada SchoolContext, Öğrenci, kayıt ve kursu sınıfları aşağıdaki gibi tanımlayın:

[!code-csharp[Main](retrieving-data/samples/sample4.cs)]

Veritabanı bağlantısı ve verilerdeki değişikliklerin yöneten DbContext SchoolContext sınıfın türetildiği.

Öğrenci sınıfında uygulanmış öznitelikleri fark **FirstName**, **LastName**, ve **yıl** özellikleri. Bu öznitelikler, bu öğreticideki veri doğrulama için kullanılır. Bu sunum proje kodu basitleştirmek için yalnızca bu özellikler veri doğrulama özniteliklerle işaretlenmiş. Gerçek bir projede kayıt ve kursu sınıflarının özellikleri gibi doğrulanmış veriler gereken tüm özellikler için doğrulama öznitelikleri uygulanacak.

UniversityModels.cs kaydedin.

Bu sınıflara göre bir veritabanı ayarlamak için araçları için Code First Migrations'ı kullanır.

İçinde **Paket Yöneticisi Konsolu**, komutu çalıştırın:  
`enable-migrations -ContextTypeName ContosoUniversityModelBinding.Models.SchoolContext`

Komut başarıyla tamamlarsa, geçişler etkin olmadığını bildiren bir ileti alırsınız,

![migrations'ı etkinleştirme](retrieving-data/_static/image8.png)

Adlı yeni bir dosya bildirim **Configuration.cs** oluşturuldu. Visual Studio'da oluşturulduktan sonra bu dosyayı otomatik olarak açılır. Yapılandırma sınıfı içeren bir **çekirdek** bu sayede veritabanı tabloları test verilerini önceden doldurmak yöntemi.

Seed yöntemi için aşağıdaki kodu ekleyin. Eklemeniz gerekecektir bir **kullanarak** bildirimi **ContosoUniversityModelBinding.Models** ad alanı.

[!code-csharp[Main](retrieving-data/samples/sample5.cs)]

Configuration.cs kaydedin.

Paket Yöneticisi Konsolu'nda komutu Çalıştır `add-migration initial`.

Ardından komutu çalıştırın `update-database`.

Bu komutu çalıştırırken özel durum iletisini alırsanız StudentID ve CourseID değerlerini Seed yöntemi değerlerinden değiştirilen mümkündür. Veritabanındaki tabloların açın ve mevcut değerleri StudentID ve CourseID bulun. Bu değerleri kayıtları tablo dengeli dağıtım için kod ekleyin.

Veritabanı dosyası eklendi, ancak şu anda projede gizlenir. Tıklayın **tüm dosyaları göster** dosya görüntülenecek.

![tüm dosyaları göster](retrieving-data/_static/image10.png)

.Mdf dosyasını şimdi uygulamada göründüğüne dikkat edin\_veri klasörü.

![Veritabanı dosyası](retrieving-data/_static/image12.png)

.Mdf dosyasını çift tıklayın ve sunucu Gezgini'ni açın. Tablo artık mevcut ve verilerle doldurulur.

![veritabanı tabloları](retrieving-data/_static/image14.png)

## <a name="display-data-from-students-and-related-tables"></a>Öğrenciler ve ilgili tablo verilerini görüntüleme

Veritabanındaki verilerle artık bu verileri almak ve bir web sayfasında görüntülemek hazır olursunuz. Kullanacağınız bir **GridView** sütunlar ve satırlarda verileri görüntülemek için denetim.

Students.aspx açarak **MainContent** yer tutucu. Bu yer tutucu içinde ekleyin bir **GridView** aşağıdaki kodu içeren bir denetim.

[!code-aspx[Main](retrieving-data/samples/sample6.aspx)]

Birkaç bu fark size işaretleme kodda önemli kavramı vardır. İlk olarak, bir değer için ayarlandığını fark **SelectMethod** GridView öğesinde bulunan özellik. Bu değer için GridView veri almak için kullanılan yöntemin adını belirtir. Sonraki adımda, bu yöntem oluşturacaksınız. İkinci olarak, dikkat **Itemtype** özelliği, daha önce oluşturduğunuz Öğrenci sınıfa ayarlayın. Bu bir değere ayarlayarak biçimlendirme koddaki söz konusu sınıfın özelliklerine başvurabilirsiniz. Örneğin, Öğrenci sınıf kayıtları adlı bir koleksiyon içerir. Kullanabileceğiniz **Item.Enrollments** o koleksiyon almak ve her öğrencinin için kayıtlı kredi toplamını almak için LINQ söz dizimi kullanın.

Arka plan kod dosyasında, belirtilen yöntemi eklemeniz gereken **SelectMethod** değeri. Açık **Students.aspx.cs**ve ekleme **kullanarak** deyimleri için **ContosoUniversityModelBinding.Models** ve **System.Data.Entity**ad alanları.

[!code-csharp[Main](retrieving-data/samples/sample7.cs)]

Ardından, aşağıdaki yöntemi ekleyin. Bu yöntemin adını SelectMethod için sağlanan adı eşleştiğine dikkat edin.

[!code-csharp[Main](retrieving-data/samples/sample8.cs)]

**INCLUDE** yan tümcesi bu sorgu performansını artırır ancak sorgu çalışması gerekli değildir. Kullanılarak veriler alınır her veritabanı için ayrı bir sorgu ilgili gönderme içerir yavaş yükleniyor, INCLUDE yan tümcesi olmadan veri alınması. INCLUDE yan tümcesi sağlayarak, tüm ilgili verileri tek bir veritabanı sorgusunu alınır yani istekli yükleme kullanarak verileri alınır. İlgili verilerin çoğu olduğu olmaması durumlarda daha fazla veri alınması için kullanılan, istekli yükleme daha az verimli olabilir. Her kayıt için ilgili verileri görüntülendiğinden ancak bu durumda, istekli yükleme en iyi performansı sunar.

İlgili verileri yüklenirken performans artışı hakkında daha fazla bilgi için başlıklı bölüme bakın **Lazy Eager ve açık, ilgili veri yükleme** içinde [bir ASP Entity Framework ile ilgili verileri okuma .NET MVC uygulamasında](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) konu.

Varsayılan olarak, veri anahtarı olarak işaretlenmiş özelliğinin değerlere göre sıralanır. Sıralama için farklı bir değer belirtmek için bir OrderBy yan tümcesi ekleyebilirsiniz. Bu örnekte, varsayılan StudentID özellik, sıralama için kullanılır. İçinde [sıralama, sayfalama ve filtreleme veri](sorting-paging-and-filtering-data.md) konu, kullanıcının sıralamak için sütun seçmek etkinleştirilir.

Web uygulamanızı çalıştırın ve öğrenciler sayfasına gidin. Öğrenciler sayfa aşağıdaki Öğrenci bilgi görüntüler.

![verileri göster](retrieving-data/_static/image16.png)

## <a name="automatic-generation-of-model-binding-methods"></a>Model bağlama yöntemleri otomatik olarak oluşturulmasını

Bu öğretici serisinin ile çalışırken, projenize öğreticinin kodu yalnızca kopyalayabilirsiniz. Ancak, bu yaklaşımın bir dezavantajı, model bağlama yöntemleri için kod otomatik olarak oluşturmak için Visual Studio tarafından sağlanan özellik, uyumlu olmayan bir duruma gelebilir emin olur. Otomatik kod oluşturma kendi projeleri üzerinde çalışıyorsanız, zaman ve nasıl bir işlem uygulanacağı bir fikir elde Yardım kaydedebilirsiniz. Otomatik kod oluşturma özelliği bu bölümde açıklanmaktadır. Bu bölümde, yalnızca bilgi amaçlıdır ve projenizde uygulamak için gereken herhangi bir kod içermiyor.

İçin bir değer ayarlarken **SelectMethod**, **UpdateMethod**, **InsertMethod**, veya **DeleteMethod** biçimlendirme kod özellikleri seçebileceğiniz **yeni yöntem oluşturma** seçeneği.

![Yeni metot oluştur](retrieving-data/_static/image18.png)

Visual Studio yalnızca arka plan kod uygun imzaya sahip bir yöntem oluşturur, ancak aynı zamanda işlemi gerçekleştirme ile yardımcı olmak için uygulama kodu oluşturur. İlk ayarlarsanız **Itemtype** otomatik kod oluşturma özelliği, oluşturulan kod kullanmadan önce özelliği bu tür işlemler için kullanır. Örneğin, UpdateMethod özelliği ayarlanırken aşağıdaki kodu otomatik olarak oluşturulur:

[!code-csharp[Main](retrieving-data/samples/sample9.cs)]

Yine, yukarıdaki kod projenize eklenmesi gerekmez. Sonraki öğreticide, güncelleştirme, silme ve yeni veri eklemek için yöntemleri uygular.

## <a name="conclusion"></a>Sonuç

Bu öğreticide, oluşturulan veri modeli sınıfları ve bu sınıflardan bir veritabanı oluşturulur. Veritabanı tablolarını test verileri ile doldurulur. Model bağlama veritabanından veri almak için kullanılır ve ardından verileri GridView içinde görüntülenir.

Sonraki [öğretici](updating-deleting-and-creating-data.md) bu dizide, güncelleştirme, silme ve verileri oluşturma sağlayacaktır.

> [!div class="step-by-step"]
> [Next](updating-deleting-and-creating-data.md)
