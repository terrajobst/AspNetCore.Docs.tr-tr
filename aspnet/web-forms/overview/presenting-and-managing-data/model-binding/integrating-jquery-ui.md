---
uid: web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
title: JQuery UI Datepicker model bağlama ve web forms ile tümleştirme | Microsoft Docs
author: Rick-Anderson
description: Bu öğretici serisinde, model bağlama kullanarak bir ASP.NET Web formları projesi ile temel yönlerini gösterir. Model bağlama veri etkileşimi daha fazla düz - sağlar...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 3cbab37b-fb0f-4751-9ec4-74e068c3f380
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: ff1b17295c58d40d55bdcd4346b83121b579bb4c
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51020565"
---
<a name="integrating-jquery-ui-datepicker-with-model-binding-and-web-forms"></a>JQuery UI Datepicker model bağlama ve web forms ile tümleştirme
====================
tarafından [Tom FitzMacken](https://github.com/tfitzmac)

> Bu öğretici serisinde, model bağlama kullanarak bir ASP.NET Web formları projesi ile temel yönlerini gösterir. Model bağlama, daha doğru verilerle ilgili kaynak nesne (örneğin, ObjectDataSource veya SqlDataSource) daha veri etkileşim sağlar. Bu seri, tanıtım malzemeleri ile başlar ve sonraki öğreticilerde için daha gelişmiş kavramlar taşır.
> 
> Bu öğreticide, JQuery kullanıcı Arabirimi ekleme işlemi açıklanır [Datepicker pencere öğesi](http://jqueryui.com/datepicker/) bir Web formu ve kullanım modeli ile seçilen değeri veritabanını güncellemek için bağlama.
> 
> Bu öğreticide oluşturulan proje geliştirir [ilk](retrieving-data.md) ve [ikinci](updating-deleting-and-creating-data.md) dizisinin bölümleri.
> 
> Yapabilecekleriniz [indirme](https://go.microsoft.com/fwlink/?LinkId=286116) tam projeyi C# veya vb İndirilebilir kod, Visual Studio 2012 veya Visual Studio 2013 ile çalışır. Bu öğreticide gösterilen Visual Studio 2013 şablonundan biraz farklıdır Visual Studio 2012 şablonu kullanır.


## <a name="what-youll-build"></a>Derleme

Bu öğreticide, gerekir:

1. Öğrenci kayıt tarihi kaydetmek için model bir özelliği Ekle
2. JQuery UI Datepicker pencere öğesi kullanarak kayıt tarih seçmesini etkinleştirin
3. Doğrulama kuralları uygulamak için kayıt tarihi

JQuery UI Datepicker pencere öğesi, kullanıcıların kolayca kullanıcı alanı ile etkileşim kurduğunda görüntülenen iletideki bir takvimden bir tarih seçmek üzere sağlar. Bu pencere öğesini kullanarak bir tarih el ile yazmak daha kullanıcılar için daha kullanışlı olabilir. Datepicker pencere öğesi veri işlemleri için model bağlama kullanan bir sayfa ile tümleştirme, yalnızca az miktarda ek iş gerektirir.

## <a name="add-a-new-property-to-the-model"></a>Yeni bir özellik modele eklemek

İlk olarak ekleyecek bir **Datetime** Tutkusu özelliğini model ve bu değişiklik veritabanına geçirin. Açık **UniversityModels.cs**, Öğrenci modele vurgulanmış kodu ekleyin.

[!code-csharp[Main](integrating-jquery-ui/samples/sample1.cs?highlight=16-18)]

**RangeAttribute** özelliği için doğrulama kuralları uygulamak dahil edilir. Bu öğreticide, Contoso University 1 Ocak 2013'ün üzerinde kurulmuştur ve bu nedenle önceki kayıt tarihleri geçerli olmayan varsayacağız.

Komutunu çalıştırarak paket Yönetimi penceresinde bir geçiş ekleyin **ekleme geçiş AddEnrollmentDate**. Geçiş kodu Öğrenci tabloya yeni bir Datetime sütunu eklediğine dikkat edin. RangeAttribute içinde belirtilen değerle eşleşecek şekilde aşağıdaki vurgulanmış kodu gösterildiği gibi yeni bir sütun için varsayılan bir değer ekleyin.

[!code-csharp[Main](integrating-jquery-ui/samples/sample2.cs?highlight=11)]

Değişikliğinizi geçiş dosyaya kaydedin.

Verileri yeniden dağıtılması gerekmez. Bu nedenle, açık **Configuration.cs** geçişler klasöründe kaldırın ya da kod açıklama **çekirdek** yöntemi. Dosyayı kaydedin ve kapatın.

Şimdi komutu çalıştırın **veritabanını Güncelleştir**. Sütun artık veritabanında bulunmuyor ve var olan kayıtların tümünün EnrollmentDate için varsayılan değere sahip olduğuna dikkat edin.

## <a name="add-dynamic-controls-for-enrollment-date"></a>Kayıt tarihi için Dinamik denetimler ekleme

Şimdi, görüntülemek ve kayıt tarihi düzenleme denetimleri de ekleyeceksiniz. Bu noktada, değerin bir metin kutusu düzenlenmiştir. Öğreticinin sonraki bölümlerinde JQuery Datepicker pencere öğesi için metin kutusu değişecektir.

İlk olarak, herhangi bir değişiklik yapmanız gerekmez unutmayın **AddStudent.aspx** dosya. DynamicEntity denetimi otomatik olarak yeni bir özellik olarak işlenir.

Açık **Students.aspx**, aşağıdaki vurgulanmış kodu ekleyin.

[!code-aspx[Main](integrating-jquery-ui/samples/sample3.aspx?highlight=13)]

Uygulamayı çalıştırın ve kayıt tarih değerini bir tarih yazarak ayarlayabilirsiniz dikkat edin. Yeni bir öğrenci eklendiği zaman:

![tarihi ayarlama](integrating-jquery-ui/_static/image1.png)

Veya var olan bir değer düzenleme:

![tarihi Düzenle](integrating-jquery-ui/_static/image2.png)

Müşteri deneyimini sağlamak istediğiniz tarih çalışır, ancak yazmayı olmayabilir. Sonraki bölümde, bir takvim aracılığıyla bir tarih seçerek olanak sağlar.

## <a name="install-nuget-package-to-work-with-jquery-ui"></a>JQuery kullanıcı Arabirimi ile çalışmak için NuGet paketini yükle

**Suyu UI** NuGet paketi, web uygulamanıza JQuery kullanıcı Arabirimi pencere öğeleri kolay tümleştirme sağlar. Bu paketi kullanmak için NuGet yükleyin.

![suyu kullanıcı Arabirimi ekleme](integrating-jquery-ui/_static/image3.png)

Yüklediğiniz suyu UI sürümü, uygulamanızdaki JQuery sürümüyle çakışabilir. Bu öğretici ile devam etmeden önce uygulamanızı çalıştırmayı deneyin. Bir JavaScript hatayla karşılaşırsanız, JQuery sürüm mutabakat sağlamak gerekir. JQuery beklenen sürümü betikleri klasörünüze (Bu öğreticide yazma zamanında sürüm 1.8.2) ekleyin veya Site.master JQuery dosyanın yolunu belirtin.

[!code-aspx[Main](integrating-jquery-ui/samples/sample4.aspx)]

## <a name="customize-datetime-template-to-include-datepicker-widget"></a>DatePicker pencere öğesi eklemek için DateTime şablonunu özelleştirme

Datepicker pencere öğesi, bir datetime değeri düzenlemek için dinamik veri şablona ekleyeceksiniz. Şablona pencere öğesi ekleyerek bunu otomatik olarak yeni bir öğrenci eklemek için her iki formu ve düzenleme Öğrenciler için kılavuz görünümünde oluşturulur. Açık **DateTime\_Edit.ascx**, aşağıdaki vurgulanmış kodu ekleyin.

[!code-aspx[Main](integrating-jquery-ui/samples/sample5.aspx?highlight=3)]

Arka plan kod dosyasında, minimum ve maksimum tarih tarih seçici için ayarlanır. Bu değerleri ayarlayarak, kullanıcıların geçersiz tarihlere gitmesini engeller. Minimum ve maksimum değerleri alır **RangeAttribute** sağlanmazsa, bir DateTime özellikte. Açık **DateTime\_Edit.ascx.cs**, sayfaya aşağıdaki vurgulanmış kodu ekleyin\_yükleme yöntemi.

[!code-csharp[Main](integrating-jquery-ui/samples/sample6.cs?highlight=9-14)]

Web uygulamasını çalıştırmak ve AddStudent sayfasına gidin. Alanlar için değerler sağlayın ve kayıt tarihi için metin kutusunu tıkladığınızda, Takvim görüntülendiğini dikkat edin.

![Tarih Seçici](integrating-jquery-ui/_static/image4.png)

Bir tarih seçin ve tıklayın **Ekle**. Sunucu doğrulamasını RangeAttribute zorlar. Datepicker üzerinde minDate özelliğini ayarlayarak, ayrıca doğrulama istemcide uygulayın. Takvime bir tarihten önce minDate değerini gidin kullanıcı izin vermez.

Izgara görünümünde bir kayıt düzenlediğinizde, Takvim de görüntülenir.

![DatePicker GridView içinde](integrating-jquery-ui/_static/image5.png)

## <a name="conclusion"></a>Sonuç

Bu öğreticide, model bağlama kullanan bir web formu JQuery pencere öğesi eklemek öğrendiniz.

Sonraki [öğretici](using-query-string-values-to-retrieve-data.md), verileri seçerken bir sorgu dizesi değerini kullanın.

> [!div class="step-by-step"]
> [Önceki](sorting-paging-and-filtering-data.md)
> [İleri](using-query-string-values-to-retrieve-data.md)
