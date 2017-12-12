---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: "ASP.NET MVC uygulamasındaki Entity Framework ile ilgili verileri güncelleştirme | Microsoft Docs"
author: tdykstra
description: "Contoso University örnek web uygulaması Entity Framework 6 Code First ve Visual Studio kullanarak ASP.NET MVC 5 uygulamalarının nasıl oluşturulacağını gösterir..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/01/2015
ms.topic: article
ms.assetid: 7ba88418-5d0a-437d-b6dc-7c3816d4ec07
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 348940748e3c33ace03d1b8f41615e9814cf6b40
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="updating-related-data-with-the-entity-framework-in-an-aspnet-mvc-application"></a>ASP.NET MVC uygulamasındaki Entity Framework ile ilgili verileri güncelleştirme
====================
tarafından [zel Dykstra](https://github.com/tdykstra)

[Tamamlanan projenizi indirin](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) veya [PDF indirin](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Contoso University örnek web uygulaması Entity Framework 6 Code First ve Visual Studio 2013 kullanarak ASP.NET MVC 5 uygulamalarının nasıl oluşturulacağını gösterir. Eğitmen serisi hakkında daha fazla bilgi için bkz: [serideki ilk öğreticide](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


Önceki öğreticide ilgili verileri görüntülenir; Bu öğreticide ilgili verileri güncelleştirin. Çoğu ilişkiler için bu yabancı anahtar alanları veya gezinti özellikleri güncelleştirerek yapılabilir. Ekleme ve varlıkları için ve uygun Gezinti özelliklerinden kaldırmak için çok-çok ilişkileri için Entity Framework birleşim tablosundan doğrudan açığa çıkarmıyor.

Aşağıdaki çizimler ile çalışma sayfaları bazıları göstermektedir.

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

![Kurslar Eğitmen düzenleme](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="customize-the-create-and-edit-pages-for-courses"></a>Oluşturma ve düzenleme sayfaları kursları özelleştirme

Yeni bir indirmelere varlık oluşturulduğunda, var olan bir bölüm için bir ilişki olması gerekir. Bu kolaylaştırmak için kurulmuş kodu denetleyicisi yöntemleri ve departman seçmek için aşağı açılan listede yer oluşturma ve düzenleme görünümleri içerir. Aşağı açılan liste kümeleri `Course.DepartmentID` yabancı anahtar özellik ve tüm Entity Framework gereken yüklemek için olan `Department` uygun sahip gezinme özelliği `Department` varlık. Kurulmuş kodu kullanın ancak biraz hata işleme ekleme ve açılan listeyi sıralayın değiştirmeniz.

İçinde *CourseController.cs*, dört silme `Create` ve `Edit` yöntemleri ve bunları aşağıdaki kodla değiştirin:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,11-12,19-25,40,44,46,48-78)]

Aşağıdakileri ekleyin `using` dosyasının başında deyimi:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

`PopulateDepartmentsDropDownList` Yöntemi adına göre sıralanmış tüm bölümleri listesini alır, oluşturur bir `SelectList` açılır listesi, koleksiyon ve koleksiyon görünümüne geçirir bir `ViewBag` özelliği. Yöntemi isteğe bağlı kabul `selectedDepartment` aşağı açılan liste işlendiğinde seçilir öğeyi seçmek arama kodu verir parametresi. Görünüm adı geçer `DepartmentID` için [DropDownList](../../older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc.md) Yardımcısı ve yardımcı sonra bilir Bakılacak `ViewBag` için nesne bir `SelectList` adlı `DepartmentID`.

`HttpGet` `Create` Yöntem çağrılarını `PopulateDepartmentsDropDownList` için yeni bir indirmelere departman henüz yapılandırılmadı olmadığından, seçili öğe ayarlama olmadan yöntemi:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

`HttpGet` `Edit` Yöntemi, seçili öğe düzenlenmekte indirmelere zaten atanmış bölüm Kimliğini temel ayarlar:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=12)]

`HttpPost` Yöntemleri her ikisi için de `Create` ve `Edit` da bunlar sayfa bir hatanın ardından yeniden görüntüleyin, seçili öğe ayarlar kodu ekleyin:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=6)]

Bu kod hata iletisini görüntülemek için sayfanın görüntülendiğinde için hangi departmanı seçilmedi seçili kalmasını sağlar.

İndirmelere görünümleri zaten departmanı alanı için aşağı açılır listeler ile iskele kurulmuş, ancak aşağıda vurgulanan yapma değiştirmek için bu alan için DepartmentID resim yazısı istemediğiniz *Views\Course\Create.cshtml* dosya Resim yazısını değiştirin.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml?highlight=43)]

Aynı değişikliği yapmak *Views\Course\Edit.cshtml*.

Anahtar değeri veritabanı tarafından oluşturulan ve değiştirilemez ve kullanıcılara görüntülenecek anlamlı bir değer değil normalde iskele kurucu bir birincil anahtar iskelesi değil. İndirmelere varlıklar için bir metin kutusu için iskele kurucu içermez `CourseID` bunun bilincindedir çünkü alan `DatabaseGeneratedOption.None` özniteliği anlamına gelir kullanıcı mümkün birincil anahtar değeri girin. Ancak anlamlı olduğundan el ile eklemeniz gerekir böylece diğer görünümlere görmek istediğinizi anlamıyor.

İçinde *Views\Course\Edit.cshtml*, önce bir indirmelere numarası alanı ekleme **başlık** alan. Birincil anahtar olduğu için bu görüntülenir, ancak değiştirilemez.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cshtml)]

Gizli bir alan zaten var. (`Html.HiddenFor` yardımcı) düzenleme görünümü indirmelere numaralı. Ekleme bir *Html.LabelFor* Yardımcısı kullanıcı tıklattığında gönderilen veriler dahil edilecek indirmelere numarası neden değil çünkü gizli alan gereksinimini ortadan değil **kaydetmek** düzenleme sayfasında.

İçinde *Views\Course\Delete.cshtml* ve *Views\Course\Details.cshtml*, "Bölüm" için "Ad" bölüm adı başlığını değiştirme ve indirmelere numarası alanı önce eklemek **başlığı**  alan.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cshtml?highlight=2,9-15)]

Çalıştırma **oluşturma** sayfa (indirmelere dizin sayfasını görüntüleyin ve tıklayın **Yeni Oluştur**) ve veri için yeni bir indirmelere girin:

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)


              **Oluştur**'u tıklatın. Listeye eklenen yeni indirmelere indirmelere dizin sayfası görüntülenir. Bölüm adı dizin sayfası listesinde ilişki doğru şekilde kurulduğundan gösteren Gezinti özelliğinden gelir.

![Course_Index_page_showing_new_course](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Çalıştırma **Düzenle** sayfa (indirmelere dizin sayfasını görüntüleyin ve tıklatın **Düzenle** bir indirmelere üzerinde).

![Course_edit_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Sayfadaki verileri değiştirip'ı **kaydetmek**. Güncelleştirilmiş indirmelere verilerle indirmelere dizin sayfası görüntülenir.

## <a name="adding-an-edit-page-for-instructors"></a>Eğitmen için bir düzen sayfası ekleme

Eğitmen kaydı düzenlediğinizde, eğitmen office atama güncelleştirmek kullanabilmek ister. `Instructor` Varlık bir tane sıfır-veya-bire ilişkisine sahip olan `OfficeAssignment` aşağıdaki durumlarda işlemek gerekir anlamına gelir, varlık:

- Kullanıcı office atama temizler ve ilk olarak bir değere sahip değilse, silme kaldırın ve gereken `OfficeAssignment` varlık.
- Kullanıcı bir office atama değeri girer ve başlangıçta boş değilse, yeni bir oluşturmalısınız `OfficeAssignment` varlık.
- Kullanıcı bir office atama değeri değiştirirse, varolan bir değer değiştirmelisiniz `OfficeAssignment` varlık.

Açık *InstructorController.cs* ve bakmak `HttpGet` `Edit` yöntemi:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

İskele kurulmuş kodu buraya istediğinizi değil. Verileri ayarlamak açılan listesinden, ancak için gerekenler metin kutusudur. Bu yöntem, aşağıdaki kod ile değiştirin:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs?highlight=7-10)]

Bu kod bırakır `ViewBag` deyimi ve ilişkili istekli yükleme ekler `OfficeAssignment` varlık. İle istekli yükleme gerçekleştirilemiyor `Find` yöntemi, böylece `Where` ve `Single` yöntemleri bunun yerine Eğitmen seçmek için kullanılır.

Değiştir `HttpPost` `Edit` aşağıdaki kod ile yöntemi. hangi office atama güncelleştirmelerini işler:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

Başvuru `RetryLimitExceededException` gerektiren bir `using` eklemek için sağ deyimi; `RetryLimitExceededException`ve ardından **gidermek** - **System.Data.Entity.Infrastructurekullanarak**.

![Yeniden deneme özel durumu gidermek](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

Kod şunları yapar:

- Yöntem adı değiştirir `EditPost` imza artık aynı olduğundan `HttpGet` yöntemi ( `ActionName` özniteliği belirtir /Edit/ URL'si hala kullanılır).
- Geçerli alır `Instructor` istekli yükleme için kullanarak veritabanını varlıktan `OfficeAssignment` gezinti özelliği. Bu yaptıklarınızın aynıdır `HttpGet` `Edit` yöntemi.
- Alınan güncelleştirmeleri `Instructor` model bağlayıcı değerlerle varlık. [TryUpdateModel](https://msdn.microsoft.com/en-us/library/dd470908(v=vs.108).aspx) aşırı kullanılan olanak tanır *beyaz liste* dahil etmek istediğiniz özellikleri. Bu açıklandığı gibi atlayarak nakil engeller [ikinci Öğreticisi](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md).

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs)]
- Ofis konumu boş ise, ayarlar `Instructor.OfficeAssignment` özelliği null böylece ilgili satırda `OfficeAssignment` tablo silinecek.

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]
- Değişiklikleri veritabanına kaydeder.

İçinde *Views\Instructor\Edit.cshtml*, sonra `div` için öğeleri **işe alma tarihi** alan, ofis konumu düzenlemek için yeni bir alan ekleyin:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cshtml)]

Sayfayı çalıştırın (seçin **Eğitmen** sekmesini ve sonra **Düzenle** bir eğitmen üzerinde). Değişiklik **ofis konumu** tıklatıp **kaydetmek**.

![Changing_the_office_location](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

## <a name="adding-course-assignments-to-the-instructor-edit-page"></a>Eğitmen ekleme indirmelere atamalar sayfasını Düzenle

Eğitmen kurslar herhangi bir sayıda öğretir. Şimdi aşağıdaki ekran görüntüsünde gösterildiği gibi onay kutuları, bir grup kullanarak indirmelere atamalarını değiştirme yeteneğini ekleyerek sayfasını Eğitmen Düzenle geliştirmek:

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

Arasındaki ilişkiyi `Course` ve `Instructor` varlıklar olan çok-çok, birleştirme tabloda yer alan yabancı anahtar özelliklerini doğrudan erişim sahip olmadığınız anlamına gelir. Bunun yerine, ekleme ve varlıkları ve ondan kaldırma `Instructor.Courses` gezinti özelliği.

Hangi kurslar değiştirmenize olanak sağlayan bir eğitmen UI'dir atanan onay kutularını grubudur. Her indirmelere veritabanındaki bir onay kutusu görüntülenir ve Eğitmen şu anda atanmış olanları seçilir. Kullanıcı seçin veya indirmelere atamalarını değiştirmek için onay kutularını temizleyin. Kurslar sayısı çok büyük, büyük olasılıkla veri görünümü sunan, farklı bir yöntem kullanmak isteyebilirsiniz, ancak ilişki oluşturma veya silme için Gezinti özellikleri düzenleme, aynı yöntemini kullanırsınız.

Onay kutuları listesi için veri görünümüne sağlamak için bir görünüm model sınıfı kullanacaksınız. Oluşturma *AssignedCourseData.cs* içinde *ViewModels* klasörü ve var olan kodu aşağıdaki kodla değiştirin:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

İçinde *InstructorController.cs*, yerine `HttpGet` `Edit` aşağıdaki kod ile yöntemi. Değişiklikler vurgulanır.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs?highlight=9,12,20-35)]

İstekli yükleme için kod ekler `Courses` gezinti özelliği ve yeni çağırır `PopulateAssignedCourseData` onay kutusunu dizi kullanma hakkında bilgi sağlamak için yöntemi `AssignedCourseData` görüntülemek model sınıfı.

Kodda `PopulateAssignedCourseData` yöntemi okur tüm `Course` görünümü kullanarak kurslar yüklenmesi için varlıklar model sınıfı. Her Elbette, kod indirmelere Eğitmen içinde var olup olmadığını denetler `Courses` gezinti özelliği. Bir indirmelere Eğitmen atanmış olup olmadığını denetlerken verimli arama oluşturmak için Eğitmen atanan kurslar içine konur bir [Hashset'i](https://msdn.microsoft.com/en-us/library/bb359438.aspx) koleksiyonu. `Assigned` Özelliği ayarlanmış `true` kursları Eğitmen atanır. Görünüm, hangi onay kutuları olarak görüntülenmelidir seçili belirlemek için bu özelliği kullanır. Son olarak, liste görünümünde geçirilecek bir `ViewBag` özelliği.

Ardından, kullanıcı tıklattığında yürütülen kod ekleme **kaydetmek**. Değiştir `EditPost` güncelleştirmeleri yeni bir yöntemi çağırır aşağıdaki kod ile yöntemi `Courses` Gezinti özelliğinin `Instructor` varlık. Değişiklikler vurgulanır.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs?highlight=3,11,25,37,40-68)]

Yöntem imzası şimdi farklıdır `HttpGet` `Edit` yöntem adını değişiklikleri şekilde yöntemi `EditPost` geri `Edit`.

Görünüm koleksiyonu olmadığından `Course` varlıklar, model bağlayıcı güncelleştiremiyor otomatik olarak `Courses` gezinti özelliği. Güncelleştirilecek model bağlayıcısını kullanmak yerine `Courses` gezinti özelliği, bu yeni gerçekleştirirsiniz `UpdateInstructorCourses` yöntemi. Bu nedenle hariç gerek `Courses` model bağlama özelliğinden. Bu çağıran kodu herhangi bir değişiklik gerektirmez [TryUpdateModel](https://msdn.microsoft.com/en-us/library/dd470908(v=vs.98).aspx) kullanmakta olduğunuz çünkü *uygulamaları güvenilir listeye almayı* aşırı yükleme ve `Courses` ekleme listesinde değil.

Hiçbir onay kutuları seçilmedi, varsa kodda `UpdateInstructorCourses` başlatır `Courses` boş bir koleksiyon gezinti özelliği:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

Kod sonra veritabanındaki tüm kursları aracılığıyla döngüler ve her indirmelere görünümünde seçilen dosyalardan karşı Eğitmen atanmış olanları karşı denetler. Verimli aramalar kolaylaştırmak için sonraki iki koleksiyon depolanmış `HashSet` nesneleri.

Bir indirmelere onay kutusunu seçildi ancak indirmelere değil `Instructor.Courses` gezinti özelliği, koleksiyon gezinti özelliği içinde indirmelere eklenir.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

Bir indirmelere onay kutusunun seçili edilmedi ancak indirmelere bulunduğu `Instructor.Courses` gezinti özelliği, indirmelere Gezinti özelliğinden kaldırılır.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs)]

İçinde *Views\Instructor\Edit.cshtml*, ekleme bir **kurslar** aşağıdakileri ekleyerek onay kutularını dizisi ile alan hemen sonra kod `div` için öğeleri `OfficeAssignment` alan ve önce `div` öğesi için **kaydetmek** düğmesi:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml)]

Yapıştırdığınız sonra bunlar burada yaptığınız gibi satır sonları, kodu ve girinti görünmüyor, burada gördüğünüz gibi görünüyor el ile her şeyi düzeltin. Girinti kusursuz, olmak zorunda değildir ancak `@</tr><tr>`, `@:<td>`, `@:</td>`, ve `@</tr>` satırlarını her tek bir satıra gösterildiği gibi veya olmalıdır, bir çalışma zamanı hatasıyla karşılaşırsınız.

Bu kod, üç sütun içeren bir HTML tablosu oluşturur. Her sütunda, indirmelere numarası ve başlığı oluşan bir resim yazısı tarafından izlenen bir onay kutusudur. Tüm onay kutularını grup olarak kabul edilmesi için oldukları model Bağlayıcısı bilgi veren aynı adı ("selectedCourses") sahip. `value` Her onay kutusunun özniteliği değerine ayarlanmış `CourseID.` sayfa gönderildiğinde, model bağlayıcı bir dizi oluşan denetleyicisine geçirir. `CourseID` yalnızca, seçilen onay kutuları için değerleri.

Onay kutularını başlangıçta işlendiğinde, eğitmen atanan kurslara olanlar sahip `checked` bunları (bunları denetlenen gösterir) seçer öznitelikleri.

İndirmelere atamaları değiştirdikten sonra site için döndürdüğünde değişiklikler doğrulayamazsınız istersiniz `Index` sayfası. Bu nedenle, bu sayfanın tablosunda bir sütun eklemeniz gerekir. Bu durumda kullanmanız gerekmez `ViewBag` görüntülemek istediğiniz bilgileri zaten kullanımda olduğundan, nesne `Courses` Gezinti özelliğinin `Instructor` modeli olarak sayfasına geçirme varlık.

İçinde *Views\Instructor\Index.cshtml*, ekleme bir **kurslar** hemen ardından başlık **Office** , aşağıdaki örnekte gösterildiği gibi Başlık:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cshtml?highlight=6)]

Daha sonra office konumu ayrıntı hücre hemen ardından yeni bir ayrıntı hücre ekleyin:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml?highlight=7-14)]

Çalıştırma **Eğitmen dizin** her Eğitmen atanan kurslar görmek için sayfayı:

![Instructor_index_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

Tıklatın **Düzenle** düzenleme sayfasını görmek için bir eğitmen üzerinde.

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)

Bazı indirmelere atamalarını değiştirip'ı **kaydetmek**. Dizin sayfasında yaptığınız değişiklikleri yansıtır.

 Not: Eğitmen indirmelere verileri düzenlemek için burada uygulanan yaklaşıma iyi kurslar sınırlı sayıda olduğunda çalışır. Farklı bir kullanıcı Arabirimi ve farklı bir güncelleştirme yöntemi, çok daha büyük olan koleksiyonları için gerekli olacak.  
 

## <a name="update-the-deleteconfirmed-method"></a>Güncelleştirme DeleteConfirmed yöntemi

İçinde *InstructorController.cs*, silme `DeleteConfirmed` aşağıdaki kodu yerine yöntemi ve Ekle.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample24.cs?highlight=5-8,12-18)]

Bu kod, aşağıdaki değişiklik yapar:

- Eğitmen departmanı yönetici olarak atanmış ise bu departmanından Eğitmen atama kaldırır. Bir bölüm için yönetici olarak atanan bir eğitmen silmek çalıştıysanız, bu kodu olmadan bir başvuru bütünlüğü hatası almalıdır.

Bu kod, birden çok Departmanlar için yönetici olarak atanmış bir eğitmen senaryo işleyemez. Son Öğreticide bu senaryonun oluşmasını engeller kod ekleyeceksiniz.

## <a name="add-office-location-and-courses-to-the-create-page"></a>Ofis konumu ve kurslar Oluştur sayfası ekleme

İçinde *InstructorController.cs*, silme `HttpGet` ve `HttpPost` `Create` yöntemleri ve ardından yerine aşağıdaki kodu ekleyin:


[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample25.cs)]

Bu kod, hiçbir kurslar seçilmemesini dışında ne için düzenleme yöntemleri gördüğünüz için benzer. `HttpGet` `Create` Yöntem çağrılarını `PopulateAssignedCourseData` değil olabileceğinden ancak seçilen kurslar yöntemi sipariş için boş bir koleksiyon sağlamak için `foreach` (Aksi halde görünümü kodu throw bir null başvuru özel durumu görünümünde döngüsü ).

Yöntem HttpPost oluşturma her seçili indirmelere önce doğrulama hatalarını denetler ve yeni Eğitmen veritabanına ekler şablon kodu kurslar gezinme özelliği ekler. Model hatalar olsa bile kurslar eklenir böylece (örneğin bir, geçersiz bir tarih anahtarlı kullanıcı) modeli hata olduğunda sayfaya bir hata iletisi ile ne zaman yeniden böylece yapılan indirmelere seçimleri otomatik olarak geri yüklenir.

Kurslara eklemek için dikkat `Courses` sahip özellik boş bir koleksiyon olarak başlatmak için gezinti özelliği:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample26.cs)]

Denetleyici kodda bunu alternatif olarak, eğitmen modelinde yoksa, otomatik olarak koleksiyonu aşağıdaki örnekte gösterildiği gibi oluşturmak için özellik alıcısı değiştirerek bunu:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample27.cs)]

Değiştirirseniz `Courses` özelliği bu şekilde, denetleyici açık özellik başlatma kodda kaldırabilirsiniz.

İçinde *Views\Instructor\Create.cshtml*, bir office konumu metin kutusu ekleyin ve işe alma tarihten sonra alan ve önce onay kutularını kurs **gönderme** düğmesi.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample28.cshtml)]

Kod yapıştırıldıktan sonra düzenleme sayfasını için daha önce yaptığınız gibi satır sonu ve girinti düzeltin.

Oluştur sayfası çalıştırın ve bir eğitmen ekleyin.

![Eğitmen kurslar ile oluşturma](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

<a id="transactions"></a>
## <a name="handling-transactions"></a>İşleme işlemleri

İçinde anlatıldığı gibi [temel CRUD işlevlerini öğretici](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md), varsayılan olarak Entity Framework örtük olarak işlemleri gerçekleştirir. Burada, daha fazla denetim--Örneğin, bir işlem içinde--Entity Framework dışında yapılan işlemleri dahil etmek istiyorsanız senaryolar görmek için [hareketlerle çalışma](https://msdn.microsoft.com/en-US/data/dn456843) MSDN'de.

## <a name="summary"></a>Özet

İlgili verilerle çalışmak için bu giriş tamamladınız. Şu ana kadar bu öğreticileri zaman uyumlu g/ç mu koduyla çalıştığınız. Zaman uyumsuz kod uygulayarak web sunucusunun kaynaklarını daha verimli bir şekilde kullanmak uygulama yapabilir ve sonraki öğreticide gerçekleştirirsiniz.

Lütfen geri bildirim, Bu öğretici beğendiğinizi nasıl ve ne biz artabileceğini bırakın. En yeni konular da isteğinde bulunabilirsiniz [Göster bana nasıl kodu ile](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Diğer Entity Framework kaynaklarına bağlantılar bulunabilir [ASP.NET Data Access - kaynakları önerilen](../../../../whitepapers/aspnet-data-access-content-map.md).

>[!div class="step-by-step"]
[Önceki](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[sonraki](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application.md)
