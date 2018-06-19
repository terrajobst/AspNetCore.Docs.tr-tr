---
uid: web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
title: Model bağlama ve web forms ile JQuery UI Datepicker tümleştirme | Microsoft Docs
author: tfitzmac
description: Bu öğretici seri model bağlama kullanarak bir ASP.NET Web Forms projesi ile temel yönlerini gösterir. Model bağlama verileri etkileşim daha fazla düz - sağlar...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 3cbab37b-fb0f-4751-9ec4-74e068c3f380
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: 126262b440f3e914a7fac3f0b7eeadb4f648d2bb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30887918"
---
<a name="integrating-jquery-ui-datepicker-with-model-binding-and-web-forms"></a>JQuery UI Datepicker model bağlama ve web forms ile tümleştirme
====================
tarafından [zel FitzMacken](https://github.com/tfitzmac)

> Bu öğretici seri model bağlama kullanarak bir ASP.NET Web Forms projesi ile temel yönlerini gösterir. Model bağlama verileri etkileşim daha düz iletme ile veri kaynağı nesneleri (örneğin, ObjectDataSource veya SqlDataSource) ilgilenme daha kolaylaştırır. Bu seri tanıtım malzemeleri ile başlar ve sonraki öğreticileri, daha gelişmiş kavramları taşır.
> 
> Bu öğretici JQuery UI eklemeyi gösterir [Datepicker pencere öğesi](http://jqueryui.com/datepicker/) bir Web formu ve seçilen değerle veritabanını güncelleştirmek için bağlama kullanım modeli.
> 
> Bu öğreticide oluşturduğunuz proje inşa edilmiştir [ilk](retrieving-data.md) ve [ikinci](updating-deleting-and-creating-data.md) seri bölümlerini.
> 
> Yapabilecekleriniz [karşıdan](https://go.microsoft.com/fwlink/?LinkId=286116) C# veya vb tamamlanmış projede İndirilebilir kod, Visual Studio 2012 veya Visual Studio 2013 ile çalışır. Bu öğreticide gösterilen Visual Studio 2013 şablon biraz farklıdır Visual Studio 2012 şablonu kullanır.


## <a name="what-youll-build"></a>Yapı

Bu öğreticide, gerekir:

1. Modelinize öğrencinin kayıt tarihi kaydetmek için özellik ekleme
2. JQuery UI Datepicker pencere öğesini kullanarak kayıt tarihi seçmek kullanıcı etkinleştir
3. Doğrulama kurallarını zorunlu tutmak için kayıt tarihi

JQuery UI Datepicker pencere öğesi kolayca bir tarih kullanıcı alanı ile etkileşim kurarken, açılır takvimden kullanıcıların seçmesine olanak sağlar. Bu pencere öğesini kullanarak bir tarih el ile yazmak daha kullanıcılar için daha kullanışlı olabilir. Model bağlama için veri işlemleri kullanan bir sayfasına Datepicker pencere öğesini tümleştirmek az miktarda ek iş gerektirir.

## <a name="add-a-new-property-to-the-model"></a>Yeni bir özellik için model ekleme

İlk olarak, ekleyecek bir **Datetime** , Öğrenci özelliğine model ve bu değişikliği veritabanına geçirme. Açık **UniversityModels.cs**ve Öğrenci modeline vurgulanmış kodu ekleyin.

[!code-csharp[Main](integrating-jquery-ui/samples/sample1.cs?highlight=16-18)]

**RangeAttribute** özelliği için doğrulama kurallarını zorunlu tutmak için eklenmiştir. Bu öğreticide, Contoso University 1 Ocak 2013'ün kurulmuştur ve bu nedenle önceki kayıt tarihleri geçerli olmayan varsayacağız.

Paket Yönetimi penceresinde, bir geçiş komutunu çalıştırarak ekleyin **add-migration AddEnrollmentDate**. Geçiş kodu Öğrenci tabloya yeni bir Datetime sütun eklediğine dikkat edin. RangeAttribute içinde belirtilen değerle eşleşecek şekilde aşağıda vurgulanan kodda gösterildiği gibi yeni bir sütun için varsayılan bir değer ekleyin.

[!code-csharp[Main](integrating-jquery-ui/samples/sample2.cs?highlight=11)]

Değişikliğiniz geçiş dosyasına kaydedin.

Verileri yeniden çekirdek gerekmez. Bu nedenle, açık **Configuration.cs** geçişler klasöründe ve kaldırın veya açıklama kod çıkışı **çekirdek** yöntemi. Dosyayı kaydedin ve kapatın.

Şimdi, komutu çalıştırmak **update-database**. Sütun artık veritabanında var ve tüm var olan kayıtlar için EnrollmentDate varsayılan değere sahip dikkat edin.

## <a name="add-dynamic-controls-for-enrollment-date"></a>Kayıt tarihi dinamik denetimleri ekleme

Şimdi görüntülemek ve kayıt tarihi düzenleme denetimleri ekleyeceksiniz. Bu noktada, değeri metin kutusu düzenlenmiştir. Daha sonra öğreticide JQuery Datepicker pencere öğesi için metin kutusunu değiştirir.

İlk olarak, herhangi bir değişiklik yapmak gerekmez unutmamalısınız **AddStudent.aspx** dosya. DynamicEntity denetimi otomatik olarak yeni özellik işlemez.

Açık **Students.aspx**ve aşağıdaki vurgulanmış kodu ekleyin.

[!code-aspx[Main](integrating-jquery-ui/samples/sample3.aspx?highlight=13)]

Uygulamayı çalıştırın ve bir tarih yazarak kayıt tarih değerini ayarlayabilir dikkat edin. Yeni bir öğrenci eklerken:

![tarihi ayarlama](integrating-jquery-ui/_static/image1.png)

Ya da var olan bir değerle düzenleme:

![düzenleme tarihi](integrating-jquery-ui/_static/image2.png)

Tarih çalışır, ancak yazarak sağlamak istediğiniz müşteri deneyimi olmayabilir. Sonraki bölümde, bir takvim aracılığıyla bir tarih seçme olanak sağlar.

## <a name="install-nuget-package-to-work-with-jquery-ui"></a>JQuery UI ile çalışmak için NuGet paket yüklemesi

**Suyu UI** NuGet paketi web uygulamanıza JQuery UI pencere öğeleri kolay tümleştirme sağlar. Bu paket kullanmak için NuGet yükleyin.

![suyu UI ekleme](integrating-jquery-ui/_static/image3.png)

Yüklediğiniz suyu UI sürümü, uygulamanızda JQuery sürümüyle çakışabilir. Bu öğretici ile devam etmeden önce uygulamayı çalıştırmayı deneyin. JavaScript hatayla karşılaşırsanız, JQuery sürüm mutabık kılmak gerekir. Komut dosyaları klasörünüze (sürüm 1.8.2 Bu öğretici yazma zamanında) JQuery beklenen sürümü eklemek veya Site.master içinde JQuery dosyasının yolunu belirtin.

[!code-aspx[Main](integrating-jquery-ui/samples/sample4.aspx)]

## <a name="customize-datetime-template-to-include-datepicker-widget"></a>DatePicker pencere öğesi eklemek için DateTime şablonunu özelleştirme

Bir tarih saat değeri düzenlemek için dinamik veri şablonu Datepicker pencere öğesi ekleyeceksiniz. Şablona pencere öğesi ekleyerek onu otomatik olarak yeni bir öğrenci eklemek için her iki formun ve ızgara görünümü düzenleme Öğrenciler için işlenir. Açık **DateTime\_Edit.ascx**ve aşağıdaki vurgulanmış kodu ekleyin.

[!code-aspx[Main](integrating-jquery-ui/samples/sample5.aspx?highlight=3)]

Arka plan kod dosyasına DatePicker için minimum ve maksimum tarihleri ayarlamak. Bu değerleri ayarlayarak, kullanıcıların geçersiz tarihleri gezinme engeller. Minimum ve maksimum değerleri alacak **RangeAttribute** bir tane belirtilmişse DateTime özelliği üzerinde. Açık **DateTime\_Edit.ascx.cs**ve aşağıdaki vurgulanmış kodu sayfasına ekleme\_yükleme yöntemi.

[!code-csharp[Main](integrating-jquery-ui/samples/sample6.cs?highlight=9-14)]

Web uygulamasını çalıştırmak ve AddStudent sayfasına gidin. Alanlar için değerler sağlayın ve kayıt tarihini metin kutusunu tıklattığınızda, takvim görüntülenir dikkat edin.

![Tarih Seçici](integrating-jquery-ui/_static/image4.png)

Bir tarihi seçin ve'ı tıklatın **Ekle**. RangeAttribute doğrulama sunucusundaki zorlar. Üzerinde Datepicker minDate özelliği ayarlanarak, ayrıca doğrulama istemcide uygulayın. Takvim tarihi minDate değerini önce gidin kullanıcı izin vermez.

Izgara görünümünde bir kaydı düzenlediğinizde, Takvim de görüntülenir.

![GridView DatePicker](integrating-jquery-ui/_static/image5.png)

## <a name="conclusion"></a>Sonuç

Bu öğreticide, model bağlama kullanan bir web formu JQuery pencere öğesi dahil öğrendiniz.

Sonraki [öğretici](using-query-string-values-to-retrieve-data.md), verileri seçerken bir sorgu dizesi değeri kullanır.

> [!div class="step-by-step"]
> [Önceki](sorting-paging-and-filtering-data.md)
> [sonraki](using-query-string-values-to-retrieve-data.md)
