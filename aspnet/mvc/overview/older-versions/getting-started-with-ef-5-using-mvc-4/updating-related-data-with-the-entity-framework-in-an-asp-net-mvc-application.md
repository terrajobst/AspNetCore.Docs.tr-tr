---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: Bir ASP.NET MVC uygulamasındaki (6 10) Entity Framework ile ilgili verileri güncelleştirme | Microsoft Docs
author: tdykstra
description: Contoso University örnek web uygulaması Entity Framework 5 Code First ve Visual Studio kullanarak ASP.NET MVC 4 uygulamaları oluşturmak nasıl gösteren...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 7871dc05-2750-470f-8b4c-3a52511949bc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 227a7fed0ced884db591f0375454d6d0a62518f5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30875090"
---
<a name="updating-related-data-with-the-entity-framework-in-an-aspnet-mvc-application-6-of-10"></a>Bir ASP.NET MVC uygulamasındaki (6 10) Entity Framework ile ilgili verileri güncelleştirme
====================
by [Tom Dykstra](https://github.com/tdykstra)

[Tamamlanan projenizi indirin](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso University örnek web uygulaması Entity Framework 5 Code First ve Visual Studio 2012 kullanarak ASP.NET MVC 4 uygulamalarının nasıl oluşturulacağını gösterir. Eğitmen serisi hakkında daha fazla bilgi için bkz: [serideki ilk öğreticide](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Eğitmen serisi baştan başlayabilirsiniz veya [Bu bölüm için bir başlangıç projesi indirme](building-the-ef5-mvc4-chapter-downloads.md) ve buradan başlayın.
> 
> > [!NOTE] 
> > 
> > Olamaz gidermek, bir sorunla çalıştırırsanız [tamamlanmış bölüm karşıdan](building-the-ef5-mvc4-chapter-downloads.md) ve sorunu yeniden deneyin. Tamamlanan kodu kodunuzu karşılaştırarak genellikle soruna çözüm bulabilirsiniz. Bazı yaygın hatalar ve bunları çözmek nasıl için bkz: [hatalarını ve geçici çözümler.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


Önceki öğreticide ilgili verileri görüntülenir; Bu öğreticide ilgili verileri güncelleştirin. Çoğu ilişkiler için bu uygun yabancı anahtar alanlarını güncelleştirerek yapılabilir. Böylece, açıkça ekleyin ve varlıklar için ve uygun Gezinti özellikleri Kaldır çok-çok ilişkileri için Entity Framework birleşim tablosundan doğrudan açığa çıkarmıyor.

Aşağıdaki çizimler ile karşılaşmayacağınızı sayfalarında gösterilir.

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="customize-the-create-and-edit-pages-for-courses"></a>Oluşturma ve düzenleme sayfaları kursları özelleştirme

Yeni bir indirmelere varlık oluşturulduğunda, var olan bir bölüm için bir ilişki olması gerekir. Bu kolaylaştırmak için kurulmuş kodu denetleyicisi yöntemleri ve departman seçmek için aşağı açılan listede yer oluşturma ve düzenleme görünümleri içerir. Aşağı açılan liste kümeleri `Course.DepartmentID` yabancı anahtar özellik ve tüm Entity Framework gereken yüklemek için olan `Department` uygun sahip gezinme özelliği `Department` varlık. Kurulmuş kodu kullanın ancak biraz hata işleme ekleme ve açılan listeyi sıralayın değiştirmeniz.

İçinde *CourseController.cs*, dört silme `Edit` ve `Create` yöntemleri ve bunları aşağıdaki kodla değiştirin:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,10,13-14,21-27,34,41,44-45,52-58,62-68)]

`PopulateDepartmentsDropDownList` Yöntemi adına göre sıralanmış tüm bölümleri listesini alır, oluşturur bir `SelectList` açılır listesi, koleksiyon ve koleksiyon görünümüne geçirir bir `ViewBag` özelliği. Yöntemi isteğe bağlı kabul `selectedDepartment` aşağı açılan liste işlendiğinde seçilir öğeyi seçmek arama kodu verir parametresi. Görünüm adı geçer `DepartmentID` için [ `DropDownList` yardımcı](../working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc.md), ve yardımcı sonra konum bilir `ViewBag` için nesne bir `SelectList` adlı `DepartmentID`.

`HttpGet` `Create` Yöntem çağrılarını `PopulateDepartmentsDropDownList` için yeni bir indirmelere departman henüz yapılandırılmadı olmadığından, seçili öğe ayarlama olmadan yöntemi:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

`HttpGet` `Edit` Yöntemi, seçili öğe düzenlenmekte indirmelere zaten atanmış bölüm Kimliğini temel ayarlar:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

`HttpPost` Yöntemleri her ikisi için de `Create` ve `Edit` da bunlar sayfa bir hatanın ardından yeniden görüntüleyin, seçili öğe ayarlar kodu ekleyin:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

Bu kod hata iletisini görüntülemek için sayfanın görüntülendiğinde için hangi departmanı seçilmedi seçili kalmasını sağlar.

İçinde *Views\Course\Create.cshtml*, önce yeni bir indirmelere numarası alanı oluşturmak için vurgulanmış kodu ekleyin **başlık** alan. Bir önceki öğreticide açıklandığı gibi birincil anahtar alanları varsayılan olarak iskele kurulmuş olmayan, ancak anahtar değeri girmek kullanıcı istediğiniz şekilde bu birincil anahtar anlamlıdır.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=17-23)]

İçinde *Views\Course\Edit.cshtml*, *Views\Course\Delete.cshtml*, ve *Views\Course\Details.cshtml*, önce bir indirmelere numarası alanı ekleme **başlığı**  alan. Birincil anahtar olduğu için bu görüntülenir, ancak değiştirilemez.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml)]

Çalıştırma **oluşturma** sayfa (indirmelere dizin sayfasını görüntüleyin ve tıklayın **Yeni Oluştur**) ve veri için yeni bir indirmelere girin:

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

**Oluştur**'u tıklatın. Listeye eklenen yeni indirmelere indirmelere dizin sayfası görüntülenir. Bölüm adı dizin sayfası listesinde ilişki doğru şekilde kurulduğundan gösteren Gezinti özelliğinden gelir.

![Course_Index_page_showing_new_course](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Çalıştırma **Düzenle** sayfa (indirmelere dizin sayfasını görüntüleyin ve tıklatın **Düzenle** bir indirmelere üzerinde).

![Course_edit_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Sayfadaki verileri değiştirip'ı **kaydetmek**. Güncelleştirilmiş indirmelere verilerle indirmelere dizin sayfası görüntülenir.

## <a name="adding-an-edit-page-for-instructors"></a>Eğitmen için bir düzen sayfası ekleme

Eğitmen kaydı düzenlediğinizde, eğitmen office atama güncelleştirmek kullanabilmek ister. `Instructor` Varlık bir tane sıfır-veya-bire ilişkisine sahip olan `OfficeAssignment` aşağıdaki durumlarda işlemek gerekir anlamına gelir, varlık:

- Kullanıcı office atama temizler ve ilk olarak bir değere sahip değilse, silme kaldırın ve gereken `OfficeAssignment` varlık.
- Kullanıcı bir office atama değeri girer ve başlangıçta boş değilse, yeni bir oluşturmalısınız `OfficeAssignment` varlık.
- Kullanıcı bir office atama değeri değiştirirse, varolan bir değer değiştirmelisiniz `OfficeAssignment` varlık.

Açık *InstructorController.cs* ve bakmak `HttpGet` `Edit` yöntemi:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

İskele kurulmuş kodu buraya istediğinizi değil. Verileri ayarlamak açılan listesinden, ancak için gerekenler metin kutusudur. Bu yöntem, aşağıdaki kod ile değiştirin:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Bu kod bırakır `ViewBag` deyimi ve ilişkili istekli yükleme ekler `OfficeAssignment` varlık. İle istekli yükleme gerçekleştirilemiyor `Find` yöntemi, böylece `Where` ve `Single` yöntemleri bunun yerine Eğitmen seçmek için kullanılır.

Değiştir `HttpPost` `Edit` aşağıdaki kod ile yöntemi. hangi office atama güncelleştirmelerini işler:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Kod şunları yapar:

- Geçerli alır `Instructor` istekli yükleme için kullanarak veritabanını varlıktan `OfficeAssignment` gezinti özelliği. Bu yaptıklarınızın aynıdır `HttpGet` `Edit` yöntemi.
- Alınan güncelleştirmeleri `Instructor` model bağlayıcı değerlerle varlık. [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.108).aspx) aşırı kullanılan olanak tanır *beyaz liste* dahil etmek istediğiniz özellikleri. Bu açıklandığı gibi atlayarak nakil engeller [ikinci Öğreticisi](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md).

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]
- Ofis konumu boş ise, ayarlar `Instructor.OfficeAssignment` özelliği null böylece ilgili satırda `OfficeAssignment` tablo silinecek.

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]
- Değişiklikleri veritabanına kaydeder.

İçinde *Views\Instructor\Edit.cshtml*, sonra `div` için öğeleri **işe alma tarihi** alan, ofis konumu düzenlemek için yeni bir alan ekleyin:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml)]

Sayfayı çalıştırın (seçin **Eğitmen** sekmesini ve sonra **Düzenle** bir eğitmen üzerinde). Değişiklik **ofis konumu** tıklatıp **kaydetmek**.

![Changing_the_office_location](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

## <a name="adding-course-assignments-to-the-instructor-edit-page"></a>Eğitmen ekleme indirmelere atamalar sayfasını Düzenle

Eğitmen kurslar herhangi bir sayıda öğretir. Şimdi aşağıdaki ekran görüntüsünde gösterildiği gibi onay kutuları, bir grup kullanarak indirmelere atamalarını değiştirme yeteneğini ekleyerek sayfasını Eğitmen Düzenle geliştirmek:

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

Arasındaki ilişkiyi `Course` ve `Instructor` varlıklar olan çok-çok, birleşim tablosundan doğrudan erişim sahip olmadığınız anlamına gelir. Bunun yerine, ekleyip varlıkları ve ondan `Instructor.Courses` gezinti özelliği.

Hangi kurslar değiştirmenize olanak sağlayan bir eğitmen UI'dir atanan onay kutularını grubudur. Her indirmelere veritabanındaki bir onay kutusu görüntülenir ve Eğitmen şu anda atanmış olanları seçilir. Kullanıcı seçin veya indirmelere atamalarını değiştirmek için onay kutularını temizleyin. Kurslar sayısı çok büyük, büyük olasılıkla veri görünümü sunan, farklı bir yöntem kullanmak isteyebilirsiniz, ancak ilişki oluşturma veya silme için Gezinti özellikleri düzenleme, aynı yöntemini kullanırsınız.

Onay kutuları listesi için veri görünümüne sağlamak için bir görünüm model sınıfı kullanacaksınız. Oluşturma *AssignedCourseData.cs* içinde *ViewModels* klasörü ve var olan kodu aşağıdaki kodla değiştirin:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

İçinde *InstructorController.cs*, yerine `HttpGet` `Edit` aşağıdaki kod ile yöntemi. Değişiklikler vurgulanır.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs?highlight=5,8,12-27)]

İstekli yükleme için kod ekler `Courses` gezinti özelliği ve yeni çağırır `PopulateAssignedCourseData` onay kutusunu dizi kullanma hakkında bilgi sağlamak için yöntemi `AssignedCourseData` görüntülemek model sınıfı.

Kodda `PopulateAssignedCourseData` yöntemi okur tüm `Course` görünümü kullanarak kurslar yüklenmesi için varlıklar model sınıfı. Her Elbette, kod indirmelere Eğitmen içinde var olup olmadığını denetler `Courses` gezinti özelliği. Bir indirmelere Eğitmen atanmış olup olmadığını denetlerken verimli arama oluşturmak için Eğitmen atanan kurslar içine konur bir [Hashset'i](https://msdn.microsoft.com/library/bb359438.aspx) koleksiyonu. `Assigned` Özelliği ayarlanmış `true` kursları Eğitmen atanır. Görünüm, hangi onay kutuları olarak görüntülenmelidir seçili belirlemek için bu özelliği kullanır. Son olarak, liste görünümünde geçirilecek bir `ViewBag` özelliği.

Ardından, kullanıcı tıklattığında yürütülen kod ekleme **kaydetmek**. Değiştir `HttpPost` `Edit` güncelleştirmeleri yeni bir yöntemi çağırır aşağıdaki kod ile yöntemi `Courses` Gezinti özelliğinin `Instructor` varlık. Değişiklikler vurgulanır.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs?highlight=3,7,20,33,37-65)]

Görünüm koleksiyonu olmadığından `Course` varlıklar, model bağlayıcı güncelleştiremiyor otomatik olarak `Courses` gezinti özelliği. Kurslar gezinti özelliği güncelleştirmek için model bağlayıcı kullanmak yerine, bu yeni gerçekleştirirsiniz `UpdateInstructorCourses` yöntemi. Bu nedenle hariç gerek `Courses` model bağlama özelliğinden. Bu çağıran kodu herhangi bir değişiklik gerektirmez [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.98).aspx) kullanmakta olduğunuz çünkü *uygulamaları güvenilir listeye almayı* aşırı yükleme ve `Courses` ekleme listesinde değil.

Hiçbir onay kutuları seçilmedi, varsa kodda `UpdateInstructorCourses` başlatır `Courses` boş bir koleksiyon gezinti özelliği:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs)]

Kod sonra veritabanındaki tüm kursları aracılığıyla döngüler ve her indirmelere görünümünde seçilen dosyalardan karşı Eğitmen atanmış olanları karşı denetler. Verimli aramalar kolaylaştırmak için sonraki iki koleksiyon depolanmış `HashSet` nesneleri.

Bir indirmelere onay kutusunu seçildi ancak indirmelere değil `Instructor.Courses` gezinti özelliği, koleksiyon gezinti özelliği içinde indirmelere eklenir.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs)]

Bir indirmelere onay kutusunun seçili edilmedi ancak indirmelere bulunduğu `Instructor.Courses` gezinti özelliği, indirmelere Gezinti özelliğinden kaldırılır.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

İçinde *Views\Instructor\Edit.cshtml*, ekleme bir **kurslar** vurgulanmış aşağıdakileri ekleyerek onay kutularını dizisi ile alan hemen sonra kod `div` için öğeleri `OfficeAssignment` alan:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml?highlight=51-73)]

Bu kod, üç sütun içeren bir HTML tablosu oluşturur. Her sütunda, indirmelere numarası ve başlığı oluşan bir resim yazısı tarafından izlenen bir onay kutusudur. Tüm onay kutularını grup olarak kabul edilmesi için oldukları model Bağlayıcısı bilgi veren aynı adı ("selectedCourses") sahip. `value` Her onay kutusunun özniteliği değerine ayarlanmış `CourseID.` sayfa gönderildiğinde, model bağlayıcı bir dizi oluşan denetleyicisine geçirir. `CourseID` yalnızca, seçilen onay kutuları için değerleri.

Onay kutularını başlangıçta işlendiğinde, eğitmen atanan kurslara olanlar sahip `checked` bunları (bunları denetlenen gösterir) seçer öznitelikleri.

İndirmelere atamaları değiştirdikten sonra site için döndürdüğünde değişiklikler doğrulayamazsınız istersiniz `Index` sayfası. Bu nedenle, bu sayfanın tablosunda bir sütun eklemeniz gerekir. Bu durumda kullanmanız gerekmez `ViewBag` görüntülemek istediğiniz bilgileri zaten kullanımda olduğundan, nesne `Courses` Gezinti özelliğinin `Instructor` modeli olarak sayfasına geçirme varlık.

İçinde *Views\Instructor\Index.cshtml*, ekleme bir **kurslar** hemen ardından başlık **Office** , aşağıdaki örnekte gösterildiği gibi Başlık:

[!code-html[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.html?highlight=7)]

Daha sonra office konumu ayrıntı hücre hemen ardından yeni bir ayrıntı hücre ekleyin:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml?highlight=19,50-57)]

Çalıştırma **Eğitmen dizin** her Eğitmen atanan kurslar görmek için sayfayı:

![Instructor_index_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

Tıklatın **Düzenle** düzenleme sayfasını görmek için bir eğitmen üzerinde.

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

Bazı indirmelere atamalarını değiştirip'ı **kaydetmek**. Dizin sayfasında yaptığınız değişiklikleri yansıtır.

 Not: Eğitmen indirmelere verileri düzenlemek için uygulanan yaklaşıma iyi kurslar sınırlı sayıda olduğunda çalışır. Farklı bir kullanıcı Arabirimi ve farklı bir güncelleştirme yöntemi, çok daha büyük olan koleksiyonları için gerekli olacak.  
 

## <a name="update-the-delete-method"></a>Güncelleştirme Delete yöntemi

Eğitmen silindiğinde (varsa) office atama kayıt silinir şekilde HttpPost Delete yöntemini kodu değiştirin:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs?highlight=6,10)]


Yönetici olarak bir birime atanmış bir eğitmen silmeye çalışırsanız, bir başvuru bütünlüğü hata iletisi alırsınız. Bkz: [geçerli sürümü Bu öğretici,](../../getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) Eğitmen Eğitmen yönetici olarak atanmış olduğu herhangi bir departmanı otomatik olarak kaldıracak ek kod için.

## <a name="summary"></a>Özet

İlgili verilerle çalışmak için bu giriş tamamladınız. Şu ana kadar bu öğreticileri tam CRUD işlemleri yaptığınızı ancak eşzamanlılık sorunları ele henüz. Sonraki öğretici eşzamanlılık konu tanıtmak, bunu işlemek için seçenekleri açıklar ve bir varlık türü için zaten yazdıktan CRUD kod işleme eşzamanlılık ekleyin.

Diğer Entity Framework kaynaklarına bağlantılar sonunda bulunabilir [bu serideki son Öğreticisi](advanced-entity-framework-scenarios-for-an-mvc-web-application.md).

> [!div class="step-by-step"]
> [Önceki](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [sonraki](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
