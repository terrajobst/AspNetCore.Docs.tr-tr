---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
title: 'Öğretici: Entity Framework 6 Code MVC 5 kullanarak First ile çalışmaya başlama | Microsoft Docs'
description: Bu öğretici serisinde, veri erişimi için Entity Framework 6 kullanan bir ASP.NET MVC 5 uygulaması derlemeyi öğrenin.
author: tdykstra
ms.author: riande
ms.date: 01/22/2019
ms.topic: tutorial
ms.assetid: 00bc8b51-32ed-4fd3-9745-be4c2a9c1eaf
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: a00a5a3aa295c2584b90abbf931c258c9e1b58ca
ms.sourcegitcommit: d5223cf6a2cf80b4f5dc54169b0e376d493d2d3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54889957"
---
# <a name="tutorial-get-started-with-entity-framework-6-code-first-using-mvc-5"></a>Öğretici: Entity Framework 6 Code MVC 5 kullanarak First ile çalışmaya başlama

> [!NOTE]
> Yeni geliştirme projeleri için öneririz [ASP.NET Core Razor sayfalar](/aspnet/core/razor-pages) ASP.NET MVC denetleyicileri ve görünümleri üzerinden. Aşağıdakine benzer bir öğretici serisinin için Razor sayfaları kullanarak bkz [Öğreticisi: ASP.NET Core Razor sayfaları kullanmaya başlama](/aspnet/core/tutorials/razor-pages/razor-pages-start). Yeni öğretici:
> * İzlemek daha kolay olur.
> * Daha fazla EF Core en iyi uygulamalar sağlanır.
> * Daha verimli sorgular kullanır.
> * En yeni API ile daha güncel değil.
> * Daha fazla özellikleri kapsar.
> * Yeni uygulama geliştirme için tercih edilen yaklaşımdır.

Bu öğretici serisinde, veri erişimi için Entity Framework 6 kullanan bir ASP.NET MVC 5 uygulaması derlemeyi öğrenin. Bu öğretici, Code First iş akışını kullanır. Code First, ilk veritabanı ve Model ilk arasında seçim yapma hakkında daha fazla bilgi için bkz. [model oluşturma](/ef/ef6/modeling/).

Bu öğretici serisinde Contoso University örnek uygulamanın nasıl oluşturulacağını açıklar. Örnek uygulama, bir basit university Web sitesidir. Bununla, görüntüleyebilir ve Öğrenci, kurs ve Eğitmen bilgileri güncelleştirin. İki oluşturduğunuz ekranlar şunlardır:

![Students_Index_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image1.png)

![Öğrenciyi Düzenle](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image2.png)

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Bir MVC web uygulaması oluşturma
> * Site stili Ayarla
> * Entity Framework 6 yükleyin
> * Veri modeli oluşturma
> * Veritabanı bağlamı oluşturur
> * DB test verileri ile başlatılamıyor
> * EF 6 ' Localdb'yi kullanmak üzere ayarla
> * Denetleyici ve görünümler oluşturma
> * Veritabanı görünümü

## <a name="prerequisites"></a>Önkoşullar

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)

## <a name="create-an-mvc-web-app"></a>Bir MVC web uygulaması oluşturma

1. Visual Studio'yu açın ve oluşturma bir C# web projesi kullanarak **ASP.NET Web uygulaması (.NET Framework)** şablonu. Projeyi adlandırın *ContosoUniversity* seçip **Tamam**.

   ![Visual Studio'da yeni proje iletişim kutusu](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/new-project-dialog.png)

1. İçinde **yeni ASP.NET Web uygulaması - ContosoUniversity**seçin **MVC**.

   ![Visual Studio'da yeni web uygulaması iletişim kutusu](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/new-web-app-dialog.png)

    > [!NOTE]
    > Varsayılan olarak, **kimlik doğrulaması** seçeneği **kimlik doğrulaması yok**. Bu öğreticide, web uygulaması oturum açmalarını gerektirmez. Ayrıca, bunu kimin imzalı dayalı olarak erişimi kısıtlamaz.

1. Seçin **Tamam** projeyi oluşturmak için.

## <a name="set-up-the-site-style"></a>Site stili Ayarla

Birkaç basit değişiklikler site menü, Düzen ve giriş sayfasına ayarlar.

1. Açık *görünümler/paylaşılan\\_Layout.cshtml*ve aşağıdaki değişiklikleri yapın:

   - "Contoso Üniversitesi" için "My ASP.NET Application" ve "Uygulama adı" her örneğini değiştirin.
   - Öğrenciler, kurslara, eğitmenler ve Departmanlar için menü girişler ekleyin ve ilgili girişi silebilirsiniz.

   Değişiklikler, aşağıdaki kod parçacığında vurgulanır:

   [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample1.cshtml?highlight=6,19,24-27,38)]

2. İçinde *Views\Home\Index.cshtml*, dosyanın içeriğini bu uygulamayla ilgili metin ile ASP.NET ve MVC hakkında metnin değiştirmek için aşağıdaki kodla değiştirin:

   [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample2.cshtml)]

3. Web sitesi çalıştırmak için CTRL + F5 tuşlarına basın. Ana menü ile giriş sayfası görürsünüz.

## <a name="install-entity-framework-6"></a>Entity Framework 6 yükleyin

1. Gelen **Araçları** menüsünde seçin **NuGet Paket Yöneticisi**ve ardından **Paket Yöneticisi Konsolu**.

2. İçinde **Paket Yöneticisi Konsolu** penceresinde aşağıdaki komutu girin:

   ```text
   Install-Package EntityFramework
   ```

Bu adım, Bu öğretici, el ile olan ancak, otomatik olarak ASP.NET MVC iskele kurma özelliği tarafından yapılmış, birkaç adımda biridir. Entity Framework (EF) kullanmak için gerekli adımları görebilmeniz için bunları el ile yaptığınız. MVC denetleyicisi ve görünümleri oluşturmak için yapı iskelesi daha sonra kullanacaksınız. Otomatik olarak EF NuGet paketini yüklemek, veritabanı bağlamı sınıfının oluşturma ve bağlantı dizesini oluşturmak yapı iskelesi izin vermek için kullanılan bir alternatiftir. Bu şekilde yapmak hazır olduğunuzda, yapmanız gereken tek şey bu adımları atlayın ve varlık sınıflarınızı oluşturduktan sonra MVC denetleyicisi iskelesini.

## <a name="create-the-data-model"></a>Veri modeli oluşturma

Sonraki varlık sınıfları Contoso University uygulaması oluşturacaksınız. Aşağıdaki üç varlıklarla başlayacaksınız:

**Kurs** <-> **kayıt** <-> **Öğrenci**

| Varlıklar | İlişki |
| -------- | ------------ |
| Kayıt için kurs | -Çok |
| Kayıt için Öğrenci | -Çok |

Arasında bir-çok ilişkisi `Student` ve `Enrollment` varlıkları ve bir-çok ilişkisi arasında `Course` ve `Enrollment` varlıklar. Diğer bir deyişle, bir öğrenci herhangi bir sayıda kursları kaydedilebilir ve bir kurs herhangi bir sayıda Öğrenciler içinde kayıtlı olabilir.

Aşağıdaki bölümlerde, bu varlıkların her biri için bir sınıf oluşturacaksınız.

> [!NOTE]
> Tüm bu varlık sınıfları oluşturma tamamlanmadan önce projeyi derlemeyi denerseniz derleyici hataları alırsınız.

### <a name="the-student-entity"></a>Öğrenci varlık

- İçinde *modelleri* klasöründe adlı bir sınıf dosyası oluşturma *Student.cs* klasöre sağ tıklayarak **Çözüm Gezgini** seçip **Ekle**  >  **Sınıfı**. Şablon kodunu aşağıdaki kodla değiştirin:

   [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample3.cs)]

`ID` Özelliği, bu sınıf için karşılık gelen veritabanı tablosunun birincil anahtar sütunu olacak. Varsayılan olarak Entity Framework adlı bir özellik yorumlar `ID` veya *classname* `ID` birincil anahtar olarak.

`Enrollments` Özelliği bir *gezinti özelliği*. Gezinti özellikleri bu varlıkla ilgili diğer varlıkların tutun. Bu durumda, `Enrollments` özelliği bir `Student` varlık tüm tutun `Enrollment` olarak ilişkili varlıkları `Student` varlık. Diğer bir deyişle, varsa bir verilen `Student` satır veritabanında ilgili iki sahip `Enrollment` satırları (Bu öğrencinin birincil anahtarını içeren satır değerini kendi `StudentID` yabancı anahtar sütunu), bu `Student` varlığın `Enrollments` gezinme özelliği Bu iki içerecek `Enrollment` varlıklar.

Gezinti özellikleri olarak tanımlanmış genellikle `virtual` bunlar belirli Entity Framework işlevleri gibi yararlanabilir, böylece *yavaş Yükleniyor*. (Gecikmeli yükleme verilecektir daha sonra [ilgili verileri okuma](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) bu serideki sonraki öğretici.)

Bir gezinme özelliği (bire çok veya tek-çok ilişkilerde) olduğu gibi birden çok varlık tutarsanız, girişleri eklenebilir, silindi ve gibi güncelleştirilmiş bir listesi türü olmalıdır `ICollection`.

### <a name="the-enrollment-entity"></a>Kayıt varlık

- İçinde *modelleri* klasör oluşturma *Enrollment.cs* ve varolan kodu aşağıdaki kodla değiştirin:

   [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample4.cs)]

`EnrollmentID` Birincil anahtar özelliği olacaktır; bu varlığı kullanan *classname* `ID` yerine desen `ID` gördüğünüz şekilde kendisi `Student` varlık. Genellikle bir düzen seçin ve veri modelinizi kullanılmakta. Burada, değişim ya da Desen kullanabilirsiniz gösterilmektedir. Bir sonraki öğreticide gördüğünüz kullanıldığında nasıl `ID` olmadan `classname` devralma veri modelinde uygulamak kolaylaştırır.

`Grade` Özelliği bir [enum](/ef/ef6/modeling/code-first/data-types/enums). Sonra soru işareti `Grade` türü bildirimi gösterir `Grade` özelliği [boş değer atanabilir](/dotnet/csharp/programming-guide/nullable-types/using-nullable-types). Boş bir sınıf bir sıfır sınıf farklıdır — bir sınıf bilinen değil veya henüz atanmamış null anlamına gelir.

`StudentID` Özelliği olduğundan yabancı anahtar ve karşılık gelen gezinme özelliğini `Student`. Bir `Enrollment` varlıktır biriyle ilişkili `Student` özelliği yalnızca tek bir içerebileceği için varlık `Student` varlık (aksine `Student.Enrollments` gezinti özelliği gördüğünüz önceki sürümlerinde, birden çok tutabilir `Enrollment` varlıklar).

`CourseID` Özelliği olduğundan yabancı anahtar ve karşılık gelen gezinme özelliğini `Course`. Bir `Enrollment` varlıktır biriyle ilişkili `Course` varlık.

Entity Framework adlandırılmışsa, bu özellik bir yabancı anahtar özellik olarak yorumlar *&lt;gezinme özelliği adı&gt;&lt;birincil anahtar özelliği adı&gt;* (örneğin, `StudentID`için `Student` gezinti özelliği bu yana `Student` varlığın birincil anahtarı `ID`). Yabancı anahtar özellikleri de adı aynı yalnızca *&lt;birincil anahtar özelliği adı&gt;* (örneğin, `CourseID` beri `Course` varlığın birincil anahtarı `CourseID`).

### <a name="the-course-entity"></a>Kurs varlık

- İçinde *modelleri* klasör oluşturma *Course.cs*, şablon kodunu aşağıdaki kodla değiştirin:

   [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample5.cs)]

`Enrollments` Özelliktir bir gezinme özelliği. A `Course` varlık dilediğiniz sayıda ilgili olabileceğini `Enrollment` varlıklar.

Daha fazla ilgili dediğimiz <xref:System.ComponentModel.DataAnnotations.Schema.DatabaseGeneratedAttribute> bu serinin sonraki bir eğitimde özniteliği. Temel olarak, bu öznitelik kursu yerine için oluşturmak veritabanı birincil anahtarı girmenize olanak tanır.

## <a name="create-the-database-context"></a>Veritabanı bağlamı oluşturur

Verilen veri modeli için Entity Framework işlevselliği koordine eden ana sınıftır *veritabanı bağlamı* sınıfı. Türeterek Bu sınıf oluşturduğunuz [System.Data.Entity.DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.113).aspx) sınıfı. Kodunuzda, hangi varlıkları veri modelinde yer alan belirtin. Ayrıca, belirli bir Entity Framework davranış özelleştirebilirsiniz. Bu projede adlı sınıfı `SchoolContext`.

- ContosoUniversity projeye bir klasör oluşturmak için projeye sağ **Çözüm Gezgini** tıklatıp **Ekle**ve ardından **yeni klasör**. Yeni klasör adı *DAL* (için veri erişim katmanı). Adlı yeni bir sınıf dosyası bu klasörde oluşturma *SchoolContext.cs*, şablonu kodu aşağıdaki kodla değiştirin:

   [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample6.cs)]

### <a name="specify-entity-sets"></a>Varlık kümesi belirtme

Bu kod oluşturur bir [olan DB](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.113).aspx) her varlık kümesi özelliği. Entity Framework terminolojisinde, bir *varlık kümesi* genellikle bir veritabanı tablosuna karşılık gelir ve bir *varlık* tablosunda bir satıra karşılık gelir.

> [!NOTE]
>
> Atlayabilirsiniz `DbSet<Enrollment>` ve `DbSet<Course>` deyimleri ve aynı şekilde çalışır. Entity Framework dahil bunları örtük olarak çünkü `Student` varlık başvuruları `Enrollment` varlık ve `Enrollment` varlık başvuruları `Course` varlık.

### <a name="specify-the-connection-string"></a>Bağlantı dizesini belirtin

(Bu, Web.config dosyasında daha sonra ekleyeceksiniz) bağlantı dizesinin adını oluşturucuya geçirilir.

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample7.cs?highlight=1)]

Bağlantı dizesindeki kendisini Web.config dosyasında depolanan bir adı yerine geçirebiliriz. Veritabanını belirtmek için kullanılacak seçenekleri hakkında daha fazla bilgi için bkz. [bağlantı dizelerini ve modelleri](/ef/ef6/fundamentals/configuring/connection-strings).

Bir bağlantı dizesi veya birisinin adını açıkça belirtmezseniz, Entity Framework bağlantı dizesi adı sınıfın adıyla aynı olduğu varsayılır. Bu örnekte varsayılan bağlantı dizesi adı ardından olacaktır `SchoolContext`, ne, açıkça belirtmekle aynı.

### <a name="specify-singular-table-names"></a>Tekil tablo adları belirtin

`modelBuilder.Conventions.Remove` Deyiminde [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx) yöntemi pluralized tablo adları engeller. Bunu yapmadıysanız, veritabanında oluşturulan tabloları sayfadayken `Students`, `Courses`, ve `Enrollments`. Bunun yerine, tablo adları olacaktır `Student`, `Course`, ve `Enrollment`. Geliştiriciler olup tablo adları veya pluralized hakkında katılmıyorum. Bu öğreticide tekil kullanır, ancak en önemli nokta dahil olmak üzere veya bu kod satırı atlama tercih hangi formu seçebilirsiniz.

## <a name="initialize-db-with-test-data"></a>DB test verileri ile başlatılamıyor

Entity Framework otomatik olarak oluşturabilir (veya drop yeniden oluşturmak için uygulama çalıştığında bir veritabanı ve). Bu, uygulama her çalıştırıldığında ya da model mevcut veritabanı ile eşitlenmemiş olduğunda yapılması gerektiğini belirtebilirsiniz. Ayrıca yazabileceğiniz bir `Seed` yöntemi, Entity Framework otomatik olarak çağıran test verileri ile doldurmak için veritabanını oluşturduktan sonra.

Yalnızca olmayan mevcut (ve modeli değişti ve veritabanı zaten mevcut değilse bir özel durum halinde) bir veritabanı oluşturmak için varsayılan davranıştır. Bu bölümde, veritabanı bırakılacak ve model değiştiğinde yeniden oluşturulması gerektiğini belirtirsiniz. Veritabanını silmek, tüm veri kaybına neden olur. Bu genellikle Tamam geliştirme sırasında çünkü `Seed` yöntemi çalıştırılacağı veritabanını yeniden oluşturulduğunda ve test verilerini yeniden oluşturun. Ancak, üretim ortamında genellikle veritabanı şemasını değiştirmeniz gereken her zaman tüm verilerinizi kaybetmek istemediğiniz. Daha sonra bırakarak ve veritabanını yeniden oluşturma yerine veritabanı şemasını değiştirmek için Code First Migrations'ı kullanarak model değişikliklerin üstesinden nasıl görürsünüz.

1. DAL klasöründe adlı yeni bir sınıf dosyası oluşturma *SchoolInitializer.cs* şablon kodunu gerektiğinde oluşturulması için bir veritabanı neden aşağıdaki kodla değiştirin ve yeni veritabanına yüklerini test verileri.

   [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample8.cs)]

   `Seed` Yöntemi giriş parametresi olarak veritabanı bağlam nesnesi alır ve yeni varlıklar eklemek için bu nesne yöntemindeki kodu kullanır. Her varlık türü için kodu yeni varlıklar koleksiyonu oluşturur, bunları uygun ekler `DbSet` özellik ve değişiklikleri veritabanına kaydeder. Çağrı için gerekli olmayan `SaveChanges` yöntemi her grubu varlıkların sonra olarak burada yapılır, ancak, bunu yardımcı olur, veritabanına kod yazarken bir özel durum oluşursa, bir sorunun kaynağını bulun.

2. Başlatıcı sınıfınıza kullanmak için Entity Framework bildirmek için bir öğeye eklediğiniz `entityFramework` uygulama öğesinde *Web.config* dosyasını (projenin kök klasöründe), aşağıdaki örnekte gösterildiği gibi:

   [!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample9.xml?highlight=2-6)]

   `context type` Bağlam tam sınıf adı ve içinde derleme belirtir ve `databaseinitializer type` Başlatıcı sınıfı ve içinde derleme tam olarak nitelenmiş adını belirtir. (EF Başlatıcı kullanmak istemediğinizde, üzerinde bir öznitelik ayarlayabilirsiniz `context` öğesi: `disableDatabaseInitialization="true"`.) Daha fazla bilgi için [yapılandırma dosyası ayarlarının](/ef/ef6/fundamentals/configuring/config-file).

   Başlatıcı ayarı alternatif *Web.config* dosyasıdır yapmak için kod ekleyerek bir `Database.SetInitializer` ifadesine `Application_Start` yönteminde *Global.asax.cs* dosya. Daha fazla bilgi için [anlama veritabanı başlatıcılar, Entity Framework Code First](http://www.codeguru.com/csharp/article.php/c19999/Understanding-Database-Initializers-in-Entity-Framework-Code-First.htm).

Böylece uygulamanın belirli bir çalıştırma ilk kez veritabanına eriştiğinizde, Entity Framework veritabanı modeline karşılaştırır. uygulamayı şimdi ayarlanır (sizin `SchoolContext` ve varlık sınıfları). Bir fark varsa uygulamanın bırakır ve veritabanını yeniden oluşturur.

> [!NOTE]
> Bir üretim web sunucusu için bir uygulama dağıttığınızda, kaldırın veya devre dışı bırakır ve veritabanını yeniden oluşturan kodu. Bu, bir sonraki Öğreticide bu serideki gerçekleştirirsiniz.

## <a name="set-up-ef-6-to-use-localdb"></a>EF 6 ' Localdb'yi kullanmak üzere ayarla

[LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb?view=sql-server-2017) SQL Server Express Veritabanı Altyapısı'nın basit bir sürümüdür. Yüklemek ve yapılandırmak kolay, isteğe bağlı olarak başlatılır ve kullanıcı modunda çalışır. Bir özel yürütme modu veritabanları ile çalışmanıza olanak tanır SQL Server Express LocalDB çalışan *.mdf* dosyaları. LocalDB veritabanı dosyaları koyabilirsiniz *uygulama\_veri* proje ile veritabanını kopyalamak isterseniz web projesinin klasörüne. SQL Server Express kullanıcı örneği özelliği de ile çalışmanıza olanak tanır *.mdf* dosyaları, ancak kullanıcı örneği özelliği kullanım dışıdır; bu nedenle, LocalDB ile çalışma için önerilir *.mdf* dosyaları. LocalDB, Visual Studio ile varsayılan olarak yüklenir.

Genellikle, SQL Server Express üretim web uygulamaları için kullanılmaz. IIS ile çalışmak için tasarlanmamıştır, çünkü LocalDB bir web uygulaması ile üretim kullanımı için özellikle önerilmez.

- Bu öğreticide, LocalDB ile çalışırsınız. Uygulamayı açmak *Web.config* dosya ve ekleme bir `connectionStrings` öğesi önceki `appSettings` öğesi, aşağıdaki örnekte gösterildiği gibi. (Güncelleştirdiğinizden emin olun *Web.config* kök proje klasöründeki dosya. Ayrıca bir *Web.config* dosyası *görünümleri* alt güncelleştirmeniz gerekmez.)

   [!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample10.xml?highlight=1-3)]

Eklediğiniz bağlantı dizesini varlık çerçevesi adlı bir LocalDB veritabanına kullanacağını belirtir. *ContosoUniversity1.mdf*. (EF oluşturur ancak veritabanı henüz mevcut değil.) Veritabanı oluşturmak istiyorsanız, *uygulama\_veri* klasör ekleyebilirsiniz `AttachDBFilename=|DataDirectory|\ContosoUniversity1.mdf` bağlantı dizesi. Bağlantı dizeleri hakkında daha fazla bilgi için bkz. [ASP.NET Web uygulamaları için SQL Server bağlantı dizelerini](/previous-versions/aspnet/jj653752(v=vs.110)).

Bağlantı dizesinde gerçekten ihtiyacınız yoksa *Web.config* dosya. Bir bağlantı dizesi sağlamazsanız, Entity Framework bağlam sınıfınıza dayalı bir varsayılan bağlantı dizesini kullanır. Daha fazla bilgi için [yeni veritabanına Code First](/ef/ef6/modeling/code-first/workflows/new-database).

## <a name="create-controller-and-views"></a>Denetleyici ve görünümler oluşturma

Artık verileri görüntülemek için bir web sayfası oluşturacaksınız. Veritabanı oluşturma verileri otomatik olarak isteme işlemini tetikler. Yeni bir denetleyici oluşturarak başlarsınız. Ancak bunu yapmadan önce modeli ve bağlam sınıfları MVC denetleyicisi yapı iskelesi kullanılabilir hale getirmek için projeyi derleyin.

1. Sağ **denetleyicileri** klasöründe **Çözüm Gezgini**seçin **Ekle**ve ardından **yeni iskele kurulmuş öğe**.
2. İçinde **İskele Ekle** iletişim kutusunda **MVC 5 denetleyici Entity Framework kullanarak görünümler ile**ve ardından **Ekle**.

     ![Visual Studio'da İskelesi iletişim Ekle](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/add-scaffold.png)

3. İçinde **denetleyici Ekle** iletişim kutusunda, aşağıdaki seçimleri yapın ve ardından **Ekle**:

   - Model sınıfı: **Öğrenci (ContosoUniversity.Models)**. (Aşağı açılan listede bu seçeneği görmüyorsanız, projeyi oluşturun ve yeniden deneyin.)
   - Veri bağlamı sınıfı: **SchoolContext (ContosoUniversity.DAL)**.
   - Denetleyici adı: **StudentController** (StudentsController değil).
   - Diğer alanları varsayılan değerleri bırakın.

     Tıkladığınızda **Ekle**, iskele kurucu oluşturur bir *StudentController.cs* dosyası ve bir görünüm kümesi (*.cshtml* dosyaları) Denetleyici ile çalışır. Gelecekte Entity Framework kullanan projeleri oluşturduğunuzda, aynı zamanda bazı ek işlevsellik iskele kurucu, yararlanabilirsiniz: ilk model sınıfınızın oluşturmak, bir bağlantı dizesi oluşturma ve ardından **DenetleyiciEkle** kutusunda belirtin **yeni veri bağlamı** seçerek **+** düğmesinin yanındaki **veri bağlamı sınıfının**. İskele kurucu oluşturacak, `DbContext` sınıfı, bağlantı dizesi denetleyici ve görünüm yanı sıra.
4. Visual Studio açılır *Controllers\StudentController.cs* dosya. Bir sınıf değişken bir veritabanı bağlam nesnesi başlatan oluşturulduğunu görürsünüz:

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

     `Index` Eylem yöntemine öğrencilerden listesini alır *Öğrenciler* varlık kümesi okuyarak `Students` veritabanı bağlam örneğinin özelliği:

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

     *Student\Index.cshtml* bu liste bir tabloda görüntüleyen:

     [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample13.cshtml)]
5. Projeyi çalıştırmak için CTRL + F5 tuşlarına basın. (Bir "Gölge kopyası oluşturulamıyor" hata alırsanız, tarayıcıyı kapatın ve yeniden deneyin.)

     Tıklayın **Öğrenciler** test verilerini görmek için sekmesinde, `Seed` eklenen yöntemi. Nasıl bağlı olarak tarayıcı pencerenizin darsa, ilk adres çubuğundaki Öğrenci sekmesini bağlantı görürsünüz veya bağlantıyı görmek için sağ üst köşedeki tıklamanız gerekir.

     ![Menü düğmesi](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image14.png)

## <a name="view-the-database"></a>Veritabanı görünümü

Öğrenciler sayfanın çalıştırdığınız ve uygulama veritabanına erişmeye EF olmadığını hiçbir veritabanı ve dağıtımd bulundu. EF sonra veritabanını verilerle doldurmak için seed yöntemi çalıştırdınız.

Kullanabilirsiniz **Sunucu Gezgini** veya **SQL Server Nesne Gezgini** (SSOX Visual Studio'daki veritabanını görüntülemek için). Bu öğretici için kullanacağınız **Sunucu Gezgini**.

1. Tarayıcıyı kapatın.
2. İçinde **Sunucu Gezgini**, genişletin **veri bağlantıları** (gerekebilir yenile düğmesine ilk seçin), genişletin **Okul bağlam (ContosoUniversity)** ve ardından genişletin **Tabloları** yeni veritabanınızdaki tablolar görmek için.

3. Sağ **Öğrenci** tıklayın ve tablo **tablo verilerini Göster** oluşturulan sütunları ve tabloya eklenen satırları görebilirsiniz.

4. Kapat **Sunucu Gezgini** bağlantı.

*ContosoUniversity1.mdf* ve *.ldf* veritabanı dosyalar, *% USERPROFILE %* klasör.

Kullanmakta olduğunuz çünkü `DropCreateDatabaseIfModelChanges` Başlatıcı artık bir değişiklik için `Student` sınıfı, uygulamayı yeniden çalıştırın ve veritabanı otomatik olarak değişikliğiniz eşleşecek şekilde yeniden oluşturulması. Örneğin, eklediğiniz bir `EmailAddress` özelliğini `Student` sınıf, Öğrenciler sayfayı yeniden çalıştırın ve ardından göz tablo yeniden, göreceksiniz yeni bir `EmailAddress` sütun.

## <a name="conventions"></a>Kurallar

Sizin için tam bir veritabanı oluşturmak Entity Framework için sırayla yazmak için olan kod nedeniyle daha kısadır *kuralları*, veya Entity Framework yapan varsayımlar. Bunlardan bazıları zaten belirtildiği veya bunları farkında olmadan kullanıldı:

- Varlık sınıfı adları pluralized formlar, tablo adları kullanılır.
- Varlık özellik adlarını sütun adları için kullanılır.
- Adlandırılmış varlık özellikleri `ID` veya *classname* `ID` birincil anahtar özellik olarak tanınır.
- Adlandırılmışsa, bu özellik bir yabancı anahtar özellik olarak yorumlanır *&lt;gezinme özelliği adı&gt;&lt;birincil anahtar özelliği adı&gt;* (örneğin, `StudentID` için`Student` gezinti özelliği bu yana `Student` varlığın birincil anahtarı `ID`). Yabancı anahtar özellikleri de adı aynı yalnızca &lt;birincil anahtar özelliği adı&gt; (örneğin, `EnrollmentID` beri `Enrollment` varlığın birincil anahtarı `EnrollmentID`).

Kuralları geçersiz kılınabilir gördünüz. Örneğin, tablo adları olmamalıdır pluralized ve daha sonra göreceğiniz belirtilen nasıl açıkça bir özelliği bir yabancı anahtar özellik olarak işaretleyin.

## <a name="get-the-code"></a>Kodu alma

[Projeyi yükle](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Ek kaynaklar

EF 6 hakkında daha fazla bilgi için şu makalelere bakın:

* [ASP.NET Veri Erişimi - Önerilen Kaynaklar](../../../../whitepapers/aspnet-data-access-content-map.md)

* [Kod öncelikli kurallar](/ef/ef6/modeling/code-first/conventions/built-in)

* [Daha Karmaşık Bir Veri Modeli Oluşturma](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Bir MVC web uygulaması oluşturuldu
> * Site stili Ayarla
> * Yüklü Entity Framework 6
> * Veri modeli oluşturduk
> * Veritabanı bağlamı oluşturuldu
> * Test verileri ile başlatılmış DB
> * EF 6 ' Localdb'yi kullanmak üzere ayarla
> * Oluşturulan denetleyici ve Görünüm
> * Veritabanı görüntülenebilir

Gözden geçirin ve oluşturma özelleştirme, okuma, güncelleştirme, silme (CRUD) kodda görünümleri ve denetleyicileri hakkında bilgi edinmek için sonraki makaleye ilerleyin.
> [!div class="nextstepaction"]
> [Temel CRUD işlevselliği uygulama](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)