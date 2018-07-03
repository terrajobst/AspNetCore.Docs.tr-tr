---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: Bir ASP.NET MVC uygulamasındaki (10 6) Entity Framework ile ilgili verileri güncelleştirme | Microsoft Docs
author: tdykstra
description: Contoso University örnek web uygulaması Entity Framework 5 Code First ve Visual Studio kullanarak ASP.NET MVC 4 uygulamalarının nasıl oluşturulacağını gösterir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 7871dc05-2750-470f-8b4c-3a52511949bc
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: dde9d5022823b252e59949144e3021a53c0bdd3a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371678"
---
<a name="updating-related-data-with-the-entity-framework-in-an-aspnet-mvc-application-6-of-10"></a>Bir ASP.NET MVC uygulamasındaki (10 6) Entity Framework ile ilgili verileri güncelleştirme
====================
tarafından [Tom Dykstra](https://github.com/tdykstra)

[Projeyi yükle](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso University örnek web uygulaması Entity Framework 5 Code First ve Visual Studio 2012 kullanarak ASP.NET MVC 4 uygulamalarının nasıl oluşturulacağını gösterir. Öğretici serisinin hakkında daha fazla bilgi için bkz. [serideki ilk öğreticide](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Öğretici serisinin en baştan başlayın veya [Bu bölüm için bir başlangıç projesi indirme](building-the-ef5-mvc4-chapter-downloads.md) ve buradan başlayın.
> 
> > [!NOTE] 
> > 
> > Çözümleyemiyor, bir sorunla karşılaştıysanız [tamamlanmış bölüm indirme](building-the-ef5-mvc4-chapter-downloads.md) ve sorununuzu yeniden oluşturmaya çalışın. Tamamlanan kodu kodunuza karşılaştırarak, sorunun çözümünü genellikle bulabilirsiniz. Bazı yaygın hatalar ve bunları çözmek nasıl için bkz: [hatalarını ve geçici çözümleri bulabilirsiniz.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


Önceki öğreticide ilgili veriler görüntülenecek; Bu öğreticide ilgili verileri güncelleştirin. Çoğu ilişki için bu uygun yabancı anahtar alanları güncelleştirerek yapılabilir. Siz açıkça ekleme ve kaldırma gelen uygun Gezinti özellikleri ve varlıkları için çoktan çoğa ilişkiler için Entity Framework birleşim tablosundan doğrudan ortaya çıkarmıyor.

Aşağıdaki çizimler ile çalışmak sayfaları göstermektedir.

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="customize-the-create-and-edit-pages-for-courses"></a>Kursları oluşturma ve düzenleme sayfalarını özelleştirme

Yeni bir kurs varlık oluşturulduğunda, var olan bir bölüm arasında bir ilişki olması gerekir. Bunu kolaylaştırmak için iskele kurulan kodu denetleyici metotları ve departman seçmek için aşağı açılan listede yer oluşturma ve düzenleme görünümleri içerir. Açılır listede kümeleri `Course.DepartmentID` yabancı anahtar özellik ve tüm yük için Entity Framework gereken `Department` uygun sahip gezinme özelliği `Department` varlık. İskele kurulan kodu kullanır ancak biraz hata işleme eklemek ve açılan listeyi sıralamak için değiştirmeniz.

İçinde *CourseController.cs*, dört Sil `Edit` ve `Create` yöntemleri ve bunları aşağıdaki kodla değiştirin:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,10,13-14,21-27,34,41,44-45,52-58,62-68)]

`PopulateDepartmentsDropDownList` Yöntemi tüm Departmanlar adına göre sıralanmış bir listesini alır, oluşturur bir `SelectList` koleksiyonu için bir açılan liste ve toplama görünümüne geçirir bir `ViewBag` özelliği. Yöntemi isteğe bağlı olarak kabul `selectedDepartment` açılır listede işlendiğinde seçilir öğeyi belirtmek çağıran kod veren parametresi. Görünüm adı geçer `DepartmentID` için [ `DropDownList` Yardımcısı](../working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc.md), aranacak yardımcı ardından bilir `ViewBag` nesnesi bir `SelectList` adlı `DepartmentID`.

`HttpGet` `Create` Yöntem çağrılarını `PopulateDepartmentsDropDownList` için yeni bir kurs departman henüz yapılandırılmadı çünkü seçilen öğe ayarlama olmadan yöntemi:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

`HttpGet` `Edit` Yöntemi düzenlenmekte olan kursa zaten atanmış bölüm Kimliğini temel öğeye ayarlar:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

`HttpPost` Hem de yöntemleri `Create` ve `Edit` de bunlar sayfanın bir hatanın ardından yeniden seçili öğe ayarlar kodu içerir:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

Bu kod, her departman seçilen hata mesajını göstermeye sayfası görüntülendiğinde, seçili kalmasını sağlar.

İçinde *Views\Course\Create.cshtml*, önce yeni bir kurs numarası alanı oluşturmak için vurgulanmış kodu ekleyin **başlık** alan. Bir önceki öğreticide açıklandığı şekilde, birincil anahtar alanları varsayılan olarak iskele kurulmuş değildir, ancak kullanıcının anahtar değeri girmeniz mümkün olmasını istediğiniz şekilde bu birincil anahtar anlamlıdır.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=17-23)]

İçinde *Views\Course\Edit.cshtml*, *Views\Course\Delete.cshtml*, ve *Views\Course\Details.cshtml*, önce bir kurs sayı alan eklemek **başlığı**  alan. Birincil anahtar olduğu için nerde gösterilirse, ancak değiştirilemez.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml)]

Çalıştırma **Oluştur** sayfa (kurs dizin sayfasını görüntüleyin ve tıklayın **Yeni Oluştur**) ve yeni bir kurs için veri girin:

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

**Oluştur**'u tıklatın. Listeye eklenen yeni kurs ile kurs dizin sayfası görüntülenir. Bölüm adı dizin sayfası listesinde ilişki doğru şekilde kurulduğundan gösteren navigation özelliğinden gelir.

![Course_Index_page_showing_new_course](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Çalıştırma **Düzenle** sayfa (kurs dizin sayfasını görüntüleyin ve tıklayın **Düzenle** kurs üzerinde).

![Course_edit_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Sayfadaki verileri değiştirip'ı **Kaydet**. Güncelleştirilmiş kurs verilerle kurs dizin sayfası görüntülenir.

## <a name="adding-an-edit-page-for-instructors"></a>Eğitmen için bir düzen sayfası ekleme

Bir eğitmen kaydı düzenlediğinizde, eğitmen ofis ataması güncelleştirilecek yönetebilmek istiyorsunuz. `Instructor` Varlığı ile bir sıfır-veya-bir ilişkisi vardır `OfficeAssignment` aşağıdaki durumlarda işlemek gerekir yani varlık:

- Kullanıcı ofis ataması temizler ve ilk olarak bir değer vardı, Sil kaldırın ve gereken `OfficeAssignment` varlık.
- Kullanıcı bir office atama değer girer ve başlangıçta boş ise, yeni bir oluşturmalısınız `OfficeAssignment` varlık.
- Kullanıcı bir ofis ataması değeri değişirse, mevcut değerin değiştirmeli `OfficeAssignment` varlık.

Açık *InstructorController.cs* bakın `HttpGet` `Edit` yöntemi:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

İskele kurulan kodu buraya istediklerinizi değildir. Bu verileri ayarı bir açılan liste, ancak için bir metin kutusu ihtiyacınız olduğu. Bu yöntem, aşağıdaki kodla değiştirin:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Bu kod bıraktığı `ViewBag` deyimi ve ilişkili istekli yükleme ekler `OfficeAssignment` varlık. İle istekli yükleme gerçekleştiremezsiniz `Find` yöntemi, bu nedenle `Where` ve `Single` yöntemleri yerine Eğitmen seçmek için kullanılır.

Değiştirin `HttpPost` `Edit` yöntemini aşağıdaki kod ile. hangi office atama güncelleştirmelerini işler:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Kod şunları yapar:

- Geçerli alır `Instructor` istekli yükleme için kullanarak veritabanını bir varlıktan `OfficeAssignment` gezinme özelliği. Bu, yaptığınız aynıdır `HttpGet` `Edit` yöntemi.
- Alınan güncelleştirmeler `Instructor` varlık değerlerle model bağlayıcı. [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.108).aspx) aşırı kullanılan olanak tanır *beyaz liste* dahil etmek istediğiniz özellikler. Bu aşırı posta açıklandığı şekilde engeller [ikinci öğreticide](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md).

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]
- Ofis konumu boş ise, ayarlar `Instructor.OfficeAssignment` özelliği null böylece ilgili satırdaki `OfficeAssignment` tablo silinecek.

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]
- Değişiklikleri veritabanına kaydeder.

İçinde *Views\Instructor\Edit.cshtml*, sonra `div` için öğeleri **işe giriş tarihi** alan, ofis konumu düzenlemek için yeni bir alan ekleyin:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml)]

Sayfayı çalıştırın (seçin **Eğitmenler** sekmesine ve ardından **Düzenle** bir eğitmen üzerinde). Değişiklik **ofis konumu** tıklatıp **Kaydet**.

![Changing_the_office_location](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

## <a name="adding-course-assignments-to-the-instructor-edit-page"></a>Eğitmen ekleme kurs atamaları sayfayı Düzenle

Eğitmenler kursları herhangi bir sayıda öğretin. Artık aşağıdaki ekran görüntüsünde gösterildiği gibi bir grup onay kutularını kullanarak kurs atamalarını değiştirme olanağı ekleyerek Eğitmen Düzenle sayfasında geliştirmek:

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

Arasındaki ilişkiyi `Course` ve `Instructor` varlıklar olduğunu çoktan çoğa, doğrudan erişim birleştirme tablosuna sahip olmadığınız anlamına gelir. Bunun yerine, ekleyip varlıkları ve ondan `Instructor.Courses` gezinme özelliği.

Hangi kursları değiştirmenize olanak sağlayan bir eğitmen UI'dir atanan onay kutularını grubudur. Her kurs sonunda verilen veritabanında bir onay kutusu görüntülenir ve Eğitmen şu anda atandığı olanları seçilir. Kullanıcı seçin veya kurs atamaları değiştirmek için onay kutularının işaretini kaldırın. Kursları sayısı çok büyük, görünüm verileri sunmak için farklı bir yöntem kullanmak istiyorsunuz, ancak oluşturmak veya ilişkileri silmek için Gezinti özelliklerini düzenleme aynı yöntemini kullanmanız gerekir.

Verileri görüntülemek için onay kutusu listesi sağlamak için bir görünüm modeli sınıfı kullanacaksınız. Oluşturma *AssignedCourseData.cs* içinde *Viewmodel'lar* klasörü ve varolan kodu aşağıdaki kodla değiştirin:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

İçinde *InstructorController.cs*, değiştirin `HttpGet` `Edit` yöntemini aşağıdaki kod ile. Değişiklikler vurgulanır.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs?highlight=5,8,12-27)]

İstekli yükleme için kod ekler `Courses` gezinme özelliğini ve yeni çağırır `PopulateAssignedCourseData` dizi onay kutusunu kullanma hakkında bilgi sağlamak için yöntemi `AssignedCourseData` model sınıfı görüntüleyin.

Kodda `PopulateAssignedCourseData` yöntemi okur ile tüm `Course` varlıklar listesi görünümünü kullanarak kursları yüklemek için model sınıfı. Her kurs için kod kursu Eğitmen içinde mevcut olup olmadığını denetler `Courses` gezinme özelliği. Bir kurs eğitmen için atanmış olup olmadığını denetlerken verimli arama oluşturmak için eğitmen için atanan kursları içine yerleştirilir bir [HashSet](https://msdn.microsoft.com/library/bb359438.aspx) koleksiyonu. `Assigned` Özelliği `true` kursları Eğitmen atanır. Görünüm, bu özellik, hangi onay kutuları olarak görüntülenmelidir seçili belirlemek için kullanır. Son olarak, listenin Görünümü'nde geçirilecek bir `ViewBag` özelliği.

Ardından, kullanıcı tıkladığında yürütülen kod ekleme **Kaydet**. Değiştirin `HttpPost` `Edit` yöntemini aşağıdaki kodla güncelleştirmeleri yeni bir yöntemi çağıran `Courses` gezinti özelliği `Instructor` varlık. Değişiklikler vurgulanır.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs?highlight=3,7,20,33,37-65)]

Görünüm koleksiyonunu olmadığı `Course` varlıklar, model bağlayıcı güncelleştiremiyor otomatik olarak `Courses` gezinme özelliği. Kursları gezinme özelliğini güncelleştirmek için model bağlayıcı kullanmak yerine, bu yeni gerçekleştirirsiniz `UpdateInstructorCourses` yöntemi. Bu nedenle hariç tutmak ihtiyacınız `Courses` model bağlama özelliği. Bunu çağıran kodu herhangi bir değişiklik gerektirmez [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.98).aspx) kullanmakta olduğunuz çünkü *beyaz listeye ekleme* aşırı yükleme ve `Courses` ekleme listesinde değil.

Onay kutuları seçilmedi, kodda `UpdateInstructorCourses` başlatır `Courses` sahip boş bir koleksiyon gezinme özelliği:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs)]

Kod sonra veritabanındaki tüm kurslara döngü ve her kurs sonunda verilen görünümünde seçilen olanları karşı Eğitmen atanmış olanlar karşı denetler. Verimli aramaları kolaylaştırmak için ikinci iki koleksiyon depolanır `HashSet` nesneleri.

Bir kurs için onay kutusu seçili ancak kursu değil `Instructor.Courses` gezinti özelliği, kurs gezinme özelliğini koleksiyona eklenir.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs)]

Bir kurs için onay kutusu seçili değildi, ancak kursu bulunduğu `Instructor.Courses` Gezinti özelliğine, kurs navigation özelliğinden kaldırılır.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

İçinde *Views\Instructor\Edit.cshtml*, ekleme bir **kursları** onay kutularını'ı vurgulanmış aşağıdakileri ekleyerek bir alanı hemen sonra kod `div` için öğeleri `OfficeAssignment` alan:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml?highlight=51-73)]

Bu kod, üç sütuna sahip bir HTML tablosu oluşturur. Her kurs numarası ve başlığı oluşan bir resim yazısı tarafından izlenen bir onay kutusu sütunundadır. Tüm onay kutularını oldukları bir grup olarak kabul edilmesi için model bağlayıcı bildirir aynı adı ("selectedCourses") sahip. `value` Her onay kutusunun öznitelik değeri olarak ayarlanır `CourseID.` sayfa gönderildiğinde, model bağlayıcı dizisi içeren denetleyicisine geçirir. `CourseID` yalnızca, seçilen onay kutularına için değerler.

Onay kutularını başlangıçta işlendiğinde, eğitmen için atanan kurslara olanlar sahip `checked` öznitelikleri, bunları (bunları kullanıma gösterir) seçer.

Kurs atamaları değiştirdikten sonra site döndüğünde değişiklikleri doğrulamak için isteyebilirsiniz `Index` sayfası. Bu nedenle, bu sayfa tabloda bir sütun eklemeniz gerekir. Bu durumda kullanmanız gerekmez `ViewBag` görüntülemek istediğiniz bilgileri zaten kullanımda olduğundan, nesne `Courses` gezinti özelliği `Instructor` sayfasına model olarak geçirerek varlık.

İçinde *Views\Instructor\Index.cshtml*, ekleme bir **kursları** hemen başlık **Office** aşağıdaki örnekte gösterildiği gibi başlığı:

[!code-html[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.html?highlight=7)]

Ardından office konumu ayrıntı hücre hemen ardından, yeni bir ayrıntı hücresi ekleyin:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml?highlight=19,50-57)]

Çalıştırma **Eğitmen dizin** her eğitmen için atanan kursları görmek için sayfayı:

![Instructor_index_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

Tıklayın **Düzenle** üzerinde bir eğitmen Düzen sayfasına bakın.

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

Bazı kurs atamalarını değiştirip'ı **Kaydet**. Dizin sayfasında, yaptığınız değişiklikler yansıtılır.

 Not: Eğitmen kurs verileri düzenlemek için uygulanan yaklaşıma de sınırlı sayıda kursları olduğunda çalışır. Farklı bir kullanıcı Arabirimi ve farklı bir güncelleştirme yöntemi, daha büyük olan koleksiyonları için gerekli olacaktır.  
 

## <a name="update-the-delete-method"></a>Güncelleştirme Delete yöntemi

Office atama kaydı (varsa), eğitmen silindiğinde silinir şekilde HttpPost Delete yöntemini kodu değiştirin:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs?highlight=6,10)]


Yönetici olarak bir bölüme atanan bir eğitmen silmeye çalışırsanız, bir başvuru bütünlüğü hatası alırsınız. Bkz: [geçerli sürümü bu öğreticinin](../../getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) Eğitmen Eğitmen yönetici olarak atanan burada departmanı otomatik olarak kaldırır ek kod için.

## <a name="summary"></a>Özet

Bu giriş, ilgili verilerle çalışmaya tamamladınız. Şu ana kadar bu öğreticilerde tam CRUD işlemleri yaptığınızdan, ancak eşzamanlılık sorunları ele henüz. Sonraki öğreticiye eşzamanlılık konu tanıtmak, bu işleme için seçenekleri açıklar ve bir varlık türü için zaten yazdığınız CRUD kodu işleme eşzamanlılık ekleyin.

Diğer Entity Framework kaynakların bağlantılarını sonunda bulunabilir [bu serinin son öğreticide](advanced-entity-framework-scenarios-for-an-mvc-web-application.md).

> [!div class="step-by-step"]
> [Önceki](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [İleri](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
