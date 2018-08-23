---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-validation-to-the-model
title: Doğrulama ekleme (VB) modeline | Microsoft Docs
author: Rick-Anderson
description: Bu öğreticide, Microsoft Visual Web Developer 2010 Express Service Pack, 1, kullanarak bir ASP.NET MVC Web uygulaması oluşturmaya yönelik temel bilgiler sağlanır...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 878f6c31-972d-45f4-8849-5c633b511409
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-validation-to-the-model
msc.type: authoredcontent
ms.openlocfilehash: d2b4fb9d8f673f223c923ba0705b1ea27d430f43
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41757238"
---
<a name="adding-validation-to-the-model-vb"></a>(VB) modele doğrulama ekleme
====================
Tarafından [Rick Anderson](https://github.com/Rick-Anderson)

> Bu öğreticide, Microsoft Visual Web Developer 2010 Express Service Pack ücretsiz bir Microsoft Visual Studio sürümü olan 1, kullanarak bir ASP.NET MVC Web uygulaması oluşturmaya yönelik temel bilgiler sağlanır. Başlamadan önce aşağıda listelenen ön yüklediğiniz emin olun. Aşağıdaki bağlantıya tıklayarak bunların tümünü yükleyebilirsiniz: [Web Platformu yükleyicisi](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternatif olarak, aşağıdaki bağlantıları kullanarak önkoşulları ayrı ayrı yükleyebilirsiniz:
> 
> - [Visual Studio Web Developer Express SP1 önkoşulları](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 araçları güncelleştirme](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(çalışma zamanı + araçları desteği)
> 
> Visual Web Developer 2010 yerine Visual Studio 2010 kullanıyorsanız, aşağıdaki bağlantıyı tıklatarak önkoşulları yükleyin: [Visual Studio 2010 önkoşulları](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Bu konuya eşlik etmek üzere bir Visual Web Developer proje VB.NET kaynak koduyla birlikte kullanılabilir. [VB.NET Eki](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). C# tercih ederseniz, geçiş [C# sürümü](../cs/adding-validation-to-the-model.md) Bu öğreticinin.


Bu bölümde, doğrulama mantığını ekleyeceksiniz `Movie` modeli ve olun doğrulama kuralları çalışır bir kullanıcı oluşturmak ve uygulamayı kullanarak bir filmi düzenlemek için dilediğiniz zaman uygulanır.

## <a name="keeping-things-dry"></a>Şeyler KURU tutma

ASP.NET MVC core tasarım İlkesi KURU ("yoksa yineleyin kendiniz") biridir. ASP.NET MVC işlevselliği veya davranış yalnızca bir kez belirtin ve ardından sahip, bir uygulamada her yerde yansıtılması için teşvik eder. Bu yazmanız gereken kod miktarını azaltır ve daha kolay bir şekilde korumak için yazdığınız kod yapar.

ASP.NET MVC ve Entity Framework Code First tarafından sağlanan doğrulama desteği, uygulamada KURU ilkesini harika bir örneğidir. Tek bir yerde (model sınıfında) doğrulama kuralları bildirimli olarak belirleyebilir ve ardından bu kurallar uygulamada her yerde uygulanır.

Nasıl bu doğrulama desteği film uygulamada yararlanabilirsiniz bakalım.

## <a name="adding-validation-rules-to-the-movie-model"></a>Film modeli doğrulama kuralları ekleme

Bazı doğrulama mantığını ekleyerek başlarsınız `Movie` sınıfı.

Açık *Movie.vb* dosya. Ekleme bir `Imports` başvuran dosyasının en üstüne bildirimine [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) ad alanı:

[!code-vb[Main](adding-validation-to-the-model/samples/sample1.vb)]

Ad alanı .NET Framework'ün bir parçasıdır. Bu yerleşik uygulayabileceğiniz bildirimli olarak herhangi bir sınıf veya özellik için doğrulama öznitelikleri kümesi sağlar.

Şimdi Güncelleştir `Movie` yerleşik yararlanmak için sınıf [ `Required` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx), [ `StringLength` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx), ve [ `Range` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) doğrulama öznitelikleri . Öznitelikleri nerde örnek olarak aşağıdaki kodu kullanın.

[!code-vb[Main](adding-validation-to-the-model/samples/sample2.vb)]

Doğrulama özniteliklerinin uygulanacak olan model özellikleri uygulamak istediğiniz davranışı belirtin. `Required` Öznitelik, bir özellik bir değere sahip olması gerektiğini gösterir; Bu örnekte, değerlerini sağlamak bir filmi var. `Title`, `ReleaseDate`, `Genre`, ve `Price` geçerli olması için özellikleri. `Range` Öznitelik değerine belirtilen bir aralıktaki kısıtlar. `StringLength` Özniteliği bir dize özelliğini en fazla uzunluğu ve isteğe bağlı olarak, minimum uzunluk ayarlamanızı sağlar.

Kod, uygulamanın veritabanında değişiklikler kaydedilmeden önce bir model sınıfında belirttiğiniz doğrulama kuralları zorunlu tutulmaz ilk sağlar. Örneğin, aşağıdaki kod bir özel durum oluşturur, `SaveChanges` yöntemi çağrıldığında, birkaç gerektirdiğinden `Movie` özellik değerleri eksik ve Fiyat (Bu geçerli aralığın) sıfırdır.

[!code-vb[Main](adding-validation-to-the-model/samples/sample3.vb)]

Doğrulama kuralları otomatik olarak .NET Framework tarafından zorlanan olması, uygulamanızın daha sağlam hale getirmeye yardımcı olur. Ayrıca, bir şey doğrulamak ve yanlışlıkla veritabanına bozuk veri unutursanız olamaz sağlar.

Güncelleştirilmiş listeleme tam kod işte *Movie.vb* dosyası:

[!code-vb[Main](adding-validation-to-the-model/samples/sample4.vb)]

## <a name="validation-error-ui-in-aspnet-mvc"></a>ASP.NET MVC kullanıcı Arabiriminde doğrulama hatası

Uygulamayı yeniden çalıştırın ve gidin */Movies* URL'si.

Tıklayın **oluşturma film** yeni bir film eklenecek bağlantı. Bazı geçersiz değerlerle formunu doldurun ve ardından **Oluştur** düğmesi.

[![8_validationErrors](adding-validation-to-the-model/_static/image2.png)](adding-validation-to-the-model/_static/image1.png)

Form otomatik olarak bir arka plan rengi geçersiz veri içeriyor ve her birinin yanında uygun doğrulama hata iletisi yayılan metin kutularını vurgulamak için nasıl kullanılacağını olduğuna dikkat edin. Hata iletileri, belirttiğiniz zaman, ek açıklamalı hata dizeleri eşleştir `Movie` sınıfı. (Bir kullanıcı JavaScript devre dışı olması durumunda) hataları hem istemci-tarafı (JavaScript kullanarak) hem de sunucu tarafı uygulanır.

Gerçek bir avantajı, tek satırlık bir kod içinde değiştirmek ihtiyacım kalmadı olan `MoviesController` sınıfı veya *Create.vbhtml* bu doğrulama kullanıcı arabirimini etkinleştirmek için görünümü. Yukarı doğrulama kuralları özniteliklerini kullanarak belirtilen denetleyici ve otomatik olarak Bu öğreticide daha önce oluşturduğunuz görünümleri çekilen `Movie` model sınıfı.

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a>Doğrulama oluşuyor nasıl oluşturma görüntüleyin ve eylem yöntemi oluşturma

Denetleyici veya görünümleri kodda herhangi bir güncelleştirme olmadan UI doğrulama nasıl oluşturulduğunu merak ediyor. Sonraki kod ne gösterir `Create` yöntemleri `MovieController` sınıfı görünür. Bunlar nasıl bunları bu öğreticinin önceki bölümlerinde oluşturduğunuz değişmez.

[!code-vb[Main](adding-validation-to-the-model/samples/sample5.vb)]

İlk eylem yönteminin ilk oluşturma formu görüntüler. İkinci form post işler. İkinci `Create` yöntem çağrılarını `ModelState.IsValid` film doğrulama hataları olup olmadığını denetlemek için. Bu yöntemi çağırmadan nesneye uygulanan herhangi bir doğrulama özniteliği değerlendirir. Nesne doğrulama hataları varsa `Create` yöntemi form görüntüler. Herhangi bir hata varsa, yöntemin yeni filmden veritabanına kaydeder.

Aşağıdaki *Create.vbhtml* öğreticinin önceki bölümlerinde iskelesi oluşturulmuş şablonu görüntüle. Bu hem gösterilen eylem yöntemlerine göre ilk formu görüntülemek ve bir hata olması durumunda görüntülemek için kullanılır.

[!code-vbhtml[Main](adding-validation-to-the-model/samples/sample6.vbhtml)]

Kodun nasıl kullandığına dikkat edin bir `Html.EditorFor` çıkış Yardımcısı `<input>` her öğe `Movie` özelliği. Bu yardımcı yanında bir çağrıdır `Html.ValidationMessageFor` yardımcı yöntemi. Bu iki yardımcı yöntemler denetleyicisi tarafından görünüm iletilen model nesnesinin çalışmak (Bu durumda, bir `Movie` nesne). Bunlar otomatik olarak uygun şekilde modeli ve görünen hata iletilerinde belirtilen doğrulama öznitelikler arayın.

Bu yaklaşımı hakkında geliştiriciyiz nedir denetleyici ne Oluştur görünüm şablonu herhangi bir şey görüntülenen özel hata iletileri veya zorlanmasını gerçek doğrulama kuralları hakkında biliyor olmasıdır. Doğrulama kuralları ve hata dizelerini yalnızca belirtilen `Movie` sınıfı.

Doğrulama mantığını daha sonra değiştirmek istiyorsanız, tam olarak tek bir yerde bunu yapabilirsiniz. Kurallar nasıl zorlanır ile tutarsız olan bir uygulamanın farklı kısımlarını hakkında endişelenmenize gerek kalmaz; tek bir yerde tanımlanmış ve her yerde kullanılan tüm doğrulama mantığı. Bu kodu çok temiz kalmasını sağlar ve korur ve evrim Geçiren daha kolay hale getirir. Ve bu, tam olarak KURU ilkesini uygularken, anlamına gelir.

## <a name="adding-formatting-to-the-movie-model"></a>Film modeli için biçimlendirme ekleme

Açık *Movie.vb* dosya. [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) Yerleşik doğrulama öznitelikleri kümesi yanı sıra biçimlendirme öznitelikleri ad alanı sağlar. Uygulayacağınız [ `DisplayFormat` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) özniteliğini ve bir [ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) yayın tarihi ve fiyat alanları için numaralandırma değeri. Aşağıdaki kodda gösterildiği `ReleaseDate` ve `Price` uygun özelliklerle [ `DisplayFormat` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) özniteliği.

[!code-vb[Main](adding-validation-to-the-model/samples/sample7.vb)]

Alternatif olarak, açıkça ayarladığınız bir [ `DataFormatString` ](https://msdn.microsoft.com/library/system.string.format.aspx) değeri. Aşağıdaki kod bir tarih biçimi dizesi yayın tarihi özelliğiyle gösterir (yani, "d"). Yayın tarihi bir parçası olarak süresi istemediğinizi belirtmek için kullanırsınız.

[!code-vb[Main](adding-validation-to-the-model/samples/sample8.vb)]

Aşağıdaki kod biçimleri `Price` para birimi olarak özelliği.

[!code-vb[Main](adding-validation-to-the-model/samples/sample9.vb)]

Tam `Movie` sınıfı aşağıda gösterilmektedir.

[!code-vb[Main](adding-validation-to-the-model/samples/sample10.vb)]

Uygulamayı çalıştırmak ve göz atın `Movies` denetleyicisi.

![8_format_SM](adding-validation-to-the-model/_static/image3.png)

Serinin sonraki bölümünde, biz uygulama gözden geçirin ve bazı iyileştirmeler otomatik olarak oluşturulan yapın `Details` ve `Delete` yöntemleri...

> [!div class="step-by-step"]
> [Önceki](adding-a-new-field.md)
> [İleri](improving-the-details-and-delete-methods.md)
