---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-validation-to-the-model
title: "(VB) Model için doğrulama ekleme | Microsoft Docs"
author: Rick-Anderson
description: "Bu öğretici Microsoft Visual Web Developer 2010 Express Service Pack olan 1, kullanarak bir ASP.NET MVC Web uygulaması oluşturmanın temellerini öğretmek..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 878f6c31-972d-45f4-8849-5c633b511409
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-validation-to-the-model
msc.type: authoredcontent
ms.openlocfilehash: a58b4a4893fca66800c012bebae4a8bbfedf7a6a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="adding-validation-to-the-model-vb"></a>(VB) Model için doğrulama ekleme
====================
Tarafından [Rick Anderson](https://github.com/Rick-Anderson)

> Bu öğretici Microsoft Visual Web Developer 2010 Express Service Pack ücretsiz Microsoft Visual Studio sürümünü olan 1, kullanarak bir ASP.NET MVC Web uygulaması oluşturmanın temellerini öğretmek. Başlamadan önce aşağıda listelenen önkoşulları kurduğunuzdan emin olun. Bunların tümünün aşağıdaki bağlantıyı tıklatarak yükleyin: [Web Platformu yükleyicisi](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternatif olarak, aşağıdaki bağlantıları kullanarak önkoşulları ayrı ayrı yükleyebilirsiniz:
> 
> - [Visual Studio Web Developer Express SP1 önkoşulları](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 araçları güncelleştirme](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(çalışma zamanı + araçları destekler)
> 
> Visual Web Developer 2010 yerine Visual Studio 2010 kullanıyorsanız, aşağıdaki bağlantıyı tıklatarak önkoşulları yükleyin: [Visual Studio 2010 önkoşulları](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> VB.NET kaynak kodu ile Visual Web Developer projesi bu konuya eşlik etmek kullanılabilir. [VB.NET Eki](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). C# tercih ederseniz, geçiş [C# sürümü](../cs/adding-validation-to-the-model.md) Bu öğreticinin.


Bu bölümde için doğrulama mantığını ekleyeceksiniz `Movie` modeli, emin olun ve kullanıcı deneyip oluşturmak veya uygulama kullanarak film düzenlemek için her zaman doğrulama kuralları zorunlu tutulmaz.

## <a name="keeping-things-dry"></a>Şeyler KURU tutma

ASP.NET MVC, çekirdek tasarım tenets KURU ("verme yineleyin kendiniz") biridir. ASP.NET MVC, işlev veya davranış yalnızca bir kez belirtin ve her yerde bir uygulamada yansıtılması sonra sağlamak için önerir. Bu kod yazmanız gereken miktarını azaltır ve çok daha kolay korumak için yazdığınız kodu sağlar.

ASP.NET MVC ve Entity Framework Code First tarafından sağlanan doğrulama desteği KURU ilkesini uygulamada harika bir örnektir. Tek bir yerde (modeli sınıfında) doğrulama kuralları bildirimli olarak belirleyebilir ve ardından bu kurallar uygulamada her yerde uygulanır.

Nasıl doğrulama desteğin film uygulamada yararlanabilirsiniz bakalım.

## <a name="adding-validation-rules-to-the-movie-model"></a>Film Model için doğrulama kuralları ekleme

Bazı doğrulama mantığını ekleyerek başlarsınız `Movie` sınıfı.

Açık *Movie.vb* dosya. Ekleme bir `Imports` deyimini dosyanın üst kısmındaki başvuran [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) ad alanı:

[!code-vb[Main](adding-validation-to-the-model/samples/sample1.vb)]

Ad alanı .NET Framework'ün parçasıdır. Yerleşik, herhangi bir sınıf veya özellik bildirimli olarak uygulanan doğrulama öznitelikleri kümesi sağlar.

Şimdi güncelleştirmek `Movie` yerleşik yararlanmak için sınıf [ `Required` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx), [ `StringLength` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx), ve [ `Range` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) doğrulama öznitelikleri . Aşağıdaki kod öznitelikleri uygulamak burada örnek olarak kullanın.

[!code-vb[Main](adding-validation-to-the-model/samples/sample2.vb)]

Doğrulama öznitelikleri için uygulanan model özellikleri zorlayan istediğiniz davranışı belirtin. `Required` Öznitelik, bir özelliği bir değere sahip olması gerektiğini gösterir; Bu örnekte, bir filmi değerlere sahip olmasını `Title`, `ReleaseDate`, `Genre`, ve `Price` geçerli olması için özellikler. `Range` Özniteliği için bir değer belirtilen aralıkta kısıtlar. `StringLength` Özniteliği bir dize özelliği en büyük uzunluğu ve isteğe bağlı olarak, minimum uzunluğu ayarlamanıza olanak tanır.

Kod, uygulamanın veritabanında değişiklikler kaydedilmeden önce bir model sınıfı belirttiğiniz doğrulama kuralları zorunlu tutulmaz ilk sağlar. Örneğin, aşağıdaki kod bir özel durum oluşturur, `SaveChanges` yöntemi çağrıldığında, birkaç gerektirdiğinden `Movie` özellik değerleri eksik ve Fiyat (Bu geçerli aralığın dışında) sıfırdır.

[!code-vb[Main](adding-validation-to-the-model/samples/sample3.vb)]

Doğrulama kuralları otomatik olarak .NET Framework tarafından zorlanan sahip yardımcı olur, uygulamanızın daha sağlam hale. Ayrıca, bir şey doğrulamak ve yanlışlıkla hatalı veri veritabanına izin unuttunuz olamaz sağlar.

Güncelleştirilmiş için listeleme tam bir kod işte *Movie.vb* dosyası:

[!code-vb[Main](adding-validation-to-the-model/samples/sample4.vb)]

## <a name="validation-error-ui-in-aspnet-mvc"></a>ASP.NET MVC kullanıcı Arabiriminde doğrulama hatası

Uygulamayı yeniden çalıştırın ve gidin */Movies* URL.

Tıklatın **oluşturma film** yeni film eklemek için bağlantı. Bazı geçersiz değerlerle formu doldurun ve ardından **oluşturma** düğmesi.

[![8_validationErrors](adding-validation-to-the-model/_static/image2.png)](adding-validation-to-the-model/_static/image1.png)

Form otomatik olarak bir arka plan rengi uygun doğrulama hata iletisi her birinin yanında gösterilen ve geçersiz veri içeren metin kutuları vurgulamak için nasıl kullanılacağını dikkat edin. Hata iletileri, belirttiğiniz zaman, açıklama hata dizeleri eşleşen `Movie` sınıfı. (Bir kullanıcının JavaScript devre dışı olduğundan durumda) hataları istemci-tarafı (JavaScript kullanarak) ve sunucu tarafı uygulanır.

Gerçek avantajdır kodda tek bir satırı değiştirmek ihtiyacım kalmadı `MoviesController` sınıfı veya *Create.vbhtml* bu doğrulama kullanıcı arabirimini etkinleştirmek için görünümü. Denetleyici ve görünümler otomatik olarak Bu öğreticide daha önce oluşturduğunuz doğrulama kuralları özniteliklerini kullanarak belirtilen yukarı çekilen `Movie` model sınıfı.

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a>Doğrulama oluşuyor nasıl oluşturma görüntülemek ve eylem yöntemi oluşturma

Denetleyici veya görünümler kodunda herhangi bir güncelleştirme olmadan UI doğrulama nasıl oluşturulan merak ediyor. Sonraki kod ne gösterir `Create` yöntemleri `MovieController` sınıfı görünümlü. Bunların nasıl bunları Bu öğreticide daha önce oluşturduğunuz değişmez.

[!code-vb[Main](adding-validation-to-the-model/samples/sample5.vb)]

İlk eylem yönteminin ilk form oluştur görüntüler. İkinci form post işler. İkinci `Create` yöntem çağrılarını `ModelState.IsValid` film herhangi bir doğrulama hatası olup olmadığını denetlemek için. Bu yöntemin çağrılması nesneye uygulanan tüm doğrulama öznitelikleri değerlendirir. Doğrulama hataları, nesne varsa, `Create` yöntemi form görüntüler. Herhangi bir hata varsa, yöntem yeni film veritabanına kaydeder.

Aşağıda *Create.vbhtml* öğreticinin önceki bölümlerinde iskele kurulmuş şablonu görüntüle. Bu hem gösterilen eylem yöntemleri tarafından ilk form görüntülemek ve bir hata durumunda yeniden görüntülemek için kullanılır.

[!code-vbhtml[Main](adding-validation-to-the-model/samples/sample6.vbhtml)]

Kodu nasıl kullandığını fark bir `Html.EditorFor` çıktısını almak için yardımcı `<input>` öğesini her `Movie` özelliği. Bu yardımcı yanındaki çağrıdır `Html.ValidationMessageFor` yardımcı yöntemi. Bu iki yardımcı yöntemler denetleyici tarafından görünüme iletilen model nesnesi ile çalışma (Bu durumda, bir `Movie` nesnesi). Bunlar otomatik olarak uygun şekilde modeli ve görüntü hata iletilerinde belirtilen doğrulama öznitelikleri arayın.

Bu yaklaşımı hakkında gerçekten iyi nedir denetleyicisi ne şablonu oluştur görüntüleme herhangi bir şey görüntülenen özel hata iletileri veya zorlanan gerçek doğrulama kuralları hakkında bildiği şeklinde değil. Doğrulama kuralları ve hata dizesi yalnızca belirtilen `Movie` sınıfı.

Doğrulama mantığını daha sonra değiştirmek istiyorsanız, tek bir yerde bunu yapabilirsiniz. Kurallar nasıl zorlanır ile tutarsız olan uygulamanın farklı bölümleri hakkında endişelenmeniz gerekmez; tüm doğrulama mantığını tek bir yerde tanımlanır ve her yerde kullanılır. Bu kod çok temiz tutar ve muhafaza etmek ve gelişmesi daha kolay hale getirir. Ve olması, anlamına gelir tam olarak KURU ilkesini uygularken.

## <a name="adding-formatting-to-the-movie-model"></a>Film modeline biçimlendirme ekleme

Açık *Movie.vb* dosya. [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) Ad alanı, yerleşik doğrulama öznitelikleri kümesi yanı sıra biçimlendirme öznitelikleri sağlar. Geçerli [ `DisplayFormat` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) özniteliğini ve bir [ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) numaralandırma değeri yayın tarihi ve fiyat alanları. Aşağıdaki kodda gösterildiği `ReleaseDate` ve `Price` uygun özelliklerle [ `DisplayFormat` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) özniteliği.

[!code-vb[Main](adding-validation-to-the-model/samples/sample7.vb)]

Alternatif olarak, açıkça ayarladığınız bir [ `DataFormatString` ](https://msdn.microsoft.com/library/system.string.format.aspx) değeri. Aşağıdaki kod bir tarih biçimi dizesi yayın tarihi özelliğiyle gösterir (yani, "d"). Bu yayın tarihi bir parçası olarak süresi istemediğinizi belirtmek için kullanırsınız.

[!code-vb[Main](adding-validation-to-the-model/samples/sample8.vb)]

Aşağıdaki kod biçimleri `Price` para birimi olarak özelliği.

[!code-vb[Main](adding-validation-to-the-model/samples/sample9.vb)]

Tam `Movie` sınıfı aşağıda gösterilmektedir.

[!code-vb[Main](adding-validation-to-the-model/samples/sample10.vb)]

Uygulamayı çalıştırın ve Gözat `Movies` denetleyicisi.

![8_format_SM](adding-validation-to-the-model/_static/image3.png)

Serinin sonraki bölümünde, biz uygulama gözden geçirin ve bazı geliştirmeler otomatik olarak oluşturulan yapmak `Details` ve `Delete` yöntemleri...

>[!div class="step-by-step"]
[Önceki](adding-a-new-field.md)
[sonraki](improving-the-details-and-delete-methods.md)
