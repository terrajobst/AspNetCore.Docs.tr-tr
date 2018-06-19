---
uid: mvc/overview/getting-started/introduction/adding-validation
title: Doğrulama ekleme | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 9f35ca15-e216-4db6-9ebf-24380b0f31b4
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-validation
msc.type: authoredcontent
ms.openlocfilehash: 946d4d5e5a506fb437232f9f4440c98e33a1a9b3
ms.sourcegitcommit: 74be78285ea88772e7dad112f80146b6ed00e53e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
ms.locfileid: "33966565"
---
<a name="adding-validation"></a>Doğrulama ekleme
====================
tarafından [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

Bu bölümde için doğrulama mantığını ekleyeceksiniz `Movie` modeli, emin olun ve kullanıcı deneyip oluşturmak veya uygulama kullanarak film düzenlemek için her zaman doğrulama kuralları zorunlu tutulmaz.

## <a name="keeping-things-dry"></a>Şeyler KURU tutma

ASP.NET MVC, çekirdek tasarım tenets biri [KURU](http://en.wikipedia.org/wiki/Don't_repeat_yourself) (&quot;yok yineleyin kendiniz&quot;). ASP.NET MVC, işlev veya davranış yalnızca bir kez belirtin ve her yerde bir uygulamada yansıtılması sonra sağlamak için önerir. Bu kod yazmanız gereken miktarını azaltır ve daha az hata potansiyeli ve sürdürmek daha kolay yazma kod yapar.

ASP.NET MVC ve Entity Framework Code First tarafından sağlanan doğrulama desteği KURU ilkesini uygulamada harika bir örnektir. Tek bir yerde (modeli sınıfında) doğrulama kuralları bildirimli olarak belirtebilirsiniz ve kuralların her yerde uygulamada uygulanır.

Nasıl doğrulama desteğin film uygulamada yararlanabilirsiniz bakalım.

## <a name="adding-validation-rules-to-the-movie-model"></a>Film Model için doğrulama kuralları ekleme

Bazı doğrulama mantığını ekleyerek başlarsınız `Movie` sınıfı.

Açık *Movie.cs* dosya. Bildirim [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) ad alanı içermiyor `System.Web`. DataAnnotations, herhangi bir sınıf veya özellik bildirimli olarak uygulanan doğrulama öznitelikleri yerleşik bir dizi sağlar. (Ayrıca gibi biçimlendirme özniteliklerini içeren [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) Yardım biçimlendirmesiyle ve tüm doğrulama sağlamıyorsa.)

Şimdi güncelleştirmek `Movie` yerleşik yararlanmak için sınıf [ `Required` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx), [ `StringLength` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx), [yanıtta normal ifade](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx), ve [ `Range` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) doğrulama öznitelikleri. Değiştir `Movie` aşağıdaki sınıfı:

[!code-csharp[Main](adding-validation/samples/sample1.cs?highlight=5,13-15,18-19,22-23)]

[ `StringLength` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) Öznitelik dizesinin uzunluğu en fazla ayarlar ve veritabanı üzerinde bu sınırlamaya ayarlar, bu nedenle veritabanı şeması değişir. Sağ tıklayın **filmler** tablosundaki **Sunucu Gezgini** tıklatıp **açık tablo tanımı**:

![](adding-validation/_static/image1.png)

Yukarıdaki resimde tüm için ayarlanmış olan dize alanları görebilirsiniz [NVARCHAR (Maks)](https://technet.microsoft.com/library/ms186939.aspx). Şemayı güncelleştirmenin geçişler kullanacağız. Çözümü derlemek ve ardından açın **Paket Yöneticisi Konsolu** penceresi ve aşağıdaki komutları girin:

[!code-console[Main](adding-validation/samples/sample2.cmd)]

Bu komut sona erdiğinde, Visual Studio yeni tanımlar sınıfı dosyasını açar `DbMIgration` türetilmiş sınıf belirtilen adla (`DataAnnotations`) ve `Up` yöntemi şema kısıtlamalarını güncelleştirmeleri kod görebilirsiniz:

[!code-csharp[Main](adding-validation/samples/sample3.cs)]

`Genre` Alanı artık boş değer atanabilir olmayan (diğer bir deyişle, bir değer girmelisiniz). `Rating` Alan en fazla 5 sahip ve `Title` 60 maksimum uzunluğunu aşıyor. En az 3'te uzunluğu `Title` ve aralıkta `Price` şema değişiklikleri oluşturmadı.

Film şema inceleyin:

![](adding-validation/_static/image2.png)

Dize alanları yeni uzunluğu sınırları gösterir ve `Genre` artık boş değer atanabilir olarak denetlenir.

Doğrulama öznitelikleri için uygulanan model özellikleri zorlayan istediğiniz davranışı belirtin. `Required` Ve `MinimumLength` öznitelikleri gösteren bir özelliği bir değer; olması gerekir, ancak hiçbir şey bu doğrulama karşılamak için boşluk girişini kullanıcı engeller. [Yanıtta normal ifade](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) özniteliği ne karakter olabilir sınırlamak için kullanılır giriş. Yukarıdaki kod `Genre` ve `Rating` yalnızca harf (beyaz alan, sayı ve özel karakterler kullanılamaz) kullanmanız gerekir. [ `Range` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) Özniteliği için bir değer belirtilen aralıkta kısıtlar. `StringLength` Özniteliği bir dize özelliği en büyük uzunluğu ve isteğe bağlı olarak, minimum uzunluğu ayarlamanıza olanak tanır. Değer türleri (gibi `decimal, int, float, DateTime`) kendiliğinden gereklidir ve gerekmeyen `Required` özniteliği.

Kod, uygulamanın veritabanında değişiklikler kaydedilmeden önce bir model sınıfı belirttiğiniz doğrulama kuralları zorunlu tutulmaz ilk sağlar. Örneğin, aşağıdaki kodu özel durum oluşturacak bir [DbEntityValidationException](https://msdn.microsoft.com/library/system.data.entity.validation.dbentityvalidationexception(v=vs.103).aspx) özel durumu zaman `SaveChanges` yöntemi çağrıldığında, birkaç gerektirdiğinden `Movie` özellik değerleri eksik:

[!code-csharp[Main](adding-validation/samples/sample4.cs)]

Yukarıdaki kod aşağıdaki özel durum oluşturur:

*Bir veya daha fazla varlıklar için doğrulanamadı. Daha fazla ayrıntı için 'EntityValidationErrors' özelliğine bakın.*

Doğrulama kuralları otomatik olarak .NET Framework tarafından zorlanan sahip yardımcı olur, uygulamanızın daha sağlam hale. Ayrıca, bir şey doğrulamak ve yanlışlıkla hatalı veri veritabanına izin unuttunuz olamaz sağlar.

## <a name="validation-error-ui-in-aspnet-mvc"></a>ASP.NET MVC kullanıcı Arabiriminde doğrulama hatası

Uygulamayı çalıştırın ve gidin */Movies* URL.

Tıklatın **Yeni Oluştur** yeni film eklemek için bağlantı. Bazı geçersiz değerlerle formu doldurun. JQuery istemci tarafı doğrulama hatası algılarsa hemen bir hata iletisi görüntüler.

![8_validationErrors](adding-validation/_static/image3.png)

> [!NOTE]
> virgül İngilizce dışındaki yerel ayarlar için jQuery doğrulamasına desteklemek için (",") için bir ondalık noktası, NuGet içermelidir Bu öğreticide daha önce açıklandığı gibi globalize.


Form otomatik olarak kırmızı kenarlık rengi uygun doğrulama hata iletisi her birinin yanında gösterilen ve geçersiz veri içeren metin kutuları vurgulamak için nasıl kullanılacağını dikkat edin. (Bir kullanıcının JavaScript devre dışı olduğundan durumda) hataları istemci-tarafı (JavaScript ve Jquery'de kullanarak) ve sunucu tarafı uygulanır.

Gerçek avantajdır kodda tek bir satırı değiştirmek ihtiyacım kalmadı `MoviesController` sınıfı veya *Create.cshtml* bu doğrulama kullanıcı arabirimini etkinleştirmek için görünümü. Denetleyici ve görünümler otomatik olarak Bu öğreticide daha önce oluşturduğunuz doğrulama kuralları özellikleri üzerinde doğrulama özniteliklerini kullanarak belirtilen yukarı çekilen `Movie` model sınıfı. Doğrulama testi kullanılarak `Edit` eylem yöntemi ve aynı doğrulama uygulanır.

İstemci tarafı doğrulama hataları kadar form verilerini sunucuya gönderilmez. Bunu kullanarak HTTP Post yönteminde kesme noktası koyarak doğrulamak [fiddler aracı](http://fiddler2.com/fiddler2/), veya IE [F12 Geliştirici Araçları](https://msdn.microsoft.com/ie/aa740478).

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a>Doğrulama oluşuyor nasıl oluşturma görüntülemek ve eylem yöntemi oluşturma

Denetleyici veya görünümler kodunda herhangi bir güncelleştirme olmadan UI doğrulama nasıl oluşturulan merak ediyor. Sonraki kod ne gösterir `Create` yöntemleri `MovieController` sınıfı görünümlü. Bunların nasıl bunları Bu öğreticide daha önce oluşturduğunuz değişmez.

[!code-csharp[Main](adding-validation/samples/sample5.cs)]

İlk (HTTP GET) `Create` eylem yönteminin ilk form oluştur görüntüler. İkinci (`[HttpPost]`) sürüm form post işler. İkinci `Create` yöntemi ( `HttpPost` sürüm) çağrıları `ModelState.IsValid` film herhangi bir doğrulama hatası olup olmadığını denetlemek için. Bu yöntemin çağrılması nesneye uygulanan tüm doğrulama öznitelikleri değerlendirir. Doğrulama hataları, nesne varsa, `Create` yöntemi, formu yeniden görüntüler. Herhangi bir hata varsa, yöntem yeni film veritabanına kaydeder. Film örneğimizde **istemci tarafında; algılanan doğrulama hataları olduğunda formun sunucuya gönderilen değil ikinci** `Create` **yöntemi çağrıldığında hiçbir zaman**. Tarayıcınızda JavaScript devre dışı bırakırsanız, istemci doğrulaması devre dışı bırakıldı ve HTTP POST `Create` yöntem çağrılarını `ModelState.IsValid` film herhangi bir doğrulama hatası olup olmadığını denetlemek için.

Bir kesme noktası ayarlayabilirsiniz `HttpPost Create` yöntemi ve doğrulama hiçbir zaman bu yöntem çağrıldığında, istemci tarafı doğrulama değil gönderme form verileri doğrulama hatalar algılandığında. JavaScript tarayıcınızda devre dışı bırakın ve ardından hatalarla form gönderme, kesme noktası karşılaşır. Hala JavaScript olmadan tam doğrulama Al Aşağıdaki resimde, Internet Explorer'da JavaScript devre dışı bırakmak gösterilmiştir.

![](adding-validation/_static/image5.png)

![](adding-validation/_static/image6.png)

Aşağıdaki resimde, FireFox tarayıcısında JavaScript devre dışı bırakmak gösterilmiştir.

![](adding-validation/_static/image7.png)

Aşağıdaki resimde, Chrome tarayıcıda JavaScript devre dışı bırakmak gösterilmiştir.

![](adding-validation/_static/image8.png)

Aşağıda *Create.cshtml* öğreticinin önceki bölümlerinde iskele kurulmuş şablonu görüntüle. Bu hem gösterilen eylem yöntemleri tarafından ilk form görüntülemek ve bir hata durumunda yeniden görüntülemek için kullanılır.

[!code-cshtml[Main](adding-validation/samples/sample6.cshtml?highlight=16-17)]

Kodu nasıl kullandığını fark bir `Html.EditorFor` çıktısını almak için yardımcı `<input>` öğesini her `Movie` özelliği. Bu yardımcı yanındaki çağrıdır `Html.ValidationMessageFor` yardımcı yöntemi. Bu iki yardımcı yöntemler denetleyici tarafından görünüme iletilen model nesnesi ile çalışma (Bu durumda, bir `Movie` nesnesi). Bunlar otomatik olarak uygun şekilde modeli ve görüntü hata iletilerinde belirtilen doğrulama öznitelikleri arayın.

Bu yaklaşımı hakkında gerçekten iyi nedir hiçbiri denetleyicisi olan veya `Create` şablonu görüntüleme bilir, herhangi bir şey, görüntülenen özel hata iletileri veya zorlanan gerçek doğrulama kuralları hakkında. Doğrulama kuralları ve hata dizesi yalnızca belirtilen `Movie` sınıfı. Bu aynı doğrulama kuralları otomatik olarak uygulanır `Edit` görünümü ve modelinizi düzenleme oluşturduğunuz diğer görünümleri şablonlar.

Doğrulama mantığını daha sonra değiştirmek istiyorsanız, tek bir yerde model için doğrulama öznitelikleri ekleyerek bunu yapabilirsiniz (Bu örnekte, `movie` sınıfı). Kurallar nasıl zorlanır ile tutarsız olan uygulamanın farklı bölümleri hakkında endişelenmeniz gerekmez; tüm doğrulama mantığını tek bir yerde tanımlanır ve her yerde kullanılır. Bu kod çok temiz tutar ve muhafaza etmek ve gelişmesi daha kolay hale getirir. Ve, tam olarak uygularken, anlamına gelir *KURU* ilkesi.

## <a name="using-datatype-attributes"></a>Veri türü öznitelikleri kullanma

Açık *Movie.cs* dosya ve inceleyin `Movie` sınıfı. [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) Ad alanı, yerleşik doğrulama öznitelikleri kümesi yanı sıra biçimlendirme öznitelikleri sağlar. Zaten uyguladık bir [ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) numaralandırma değeri yayın tarihi ve fiyat alanları. Aşağıdaki kodda gösterildiği `ReleaseDate` ve `Price` uygun özelliklerle [ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) özniteliği.

[!code-csharp[Main](adding-validation/samples/sample7.cs)]

[DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) öznitelikler verilerin biçimlendirilmesi görünüm altyapısı için ipuçları yalnızca sağlar (ve öznitelikler gibi tedarik `<a>` URL'SİNİN için ve `<a href="mailto:EmailAddress.com">` e-posta için. Kullanabileceğiniz [yanıtta normal ifade](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) veri biçimi doğrulamak için öznitelik. [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) özniteliği veritabanı geçerli bir tür daha fazla belirli bir veri türünü belirtmek için kullanılır, bunlar ***değil*** doğrulama öznitelikleri. Bu durumda yalnızca tarihi, tarih ve saat değil izlemek istiyoruz. [DataType numaralandırma](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) birçok veri türleri gibi sağlar *tarih, saat, PhoneNumber, para birimi, EmailAddress* ve daha fazlası. `DataType` Özniteliği de otomatik olarak türüne özgü özellikleri sağlamak uygulama etkinleştir. Örneğin, bir `mailto:` bağlantı için oluşturulabilir [DataType.EmailAddress](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx), ve bir tarih seçici için sağlanan [DataType.Date](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) destekleyen tarayıcılarda [HTML5](http://html5.org/). [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) öznitelikleri yayar HTML 5 [veri](http://ejohn.org/blog/html-5-data-attributes/) (belirgin *veri tire*) HTML 5 tarayıcılar anlayabileceği öznitelikleri. [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) öznitelikleri tüm doğrulama sağlamaz.

`DataType.Date` Görüntülenen tarih biçimi belirtmiyor. Varsayılan olarak, sunucu üzerinde temel alan varsayılan biçimler göre veri alanı görüntülenir [CultureInfo](https://msdn.microsoft.com/library/vstudio/system.globalization.cultureinfo(v=vs.110).aspx).

`DisplayFormat` Özniteliği açıkça tarih biçimini belirtmek için kullanılır:


[!code-csharp[Main](adding-validation/samples/sample8.cs)]


`ApplyFormatInEditMode` Ayar değeri düzenlemek için bir metin kutusu görüntülendiğinde belirtilen biçimlendirmeyi de uygulanması gerektiğini belirtir. (, Bazı alanlar için istemeyebilirsiniz — Örneğin, para birimi değerleri için metin kutusuna para birimi simgesini düzenleme için istemeyebilirsiniz.)

Kullanabileceğiniz [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) kendisi, ancak tarafından özniteliktir genellikle kullanmak iyi bir fikir [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) de özniteliği. `DataType` Özniteliği ileten *semantiği* verilerin olarak ekranda işlemek nasıl değil ve ile elde etmezsiniz aşağıdaki yararları sağlar `DisplayFormat`:

- Tarayıcı HTML5 özellikleri (örneğin. bir Takvim denetimi, yerel ayar uygun para birimi simgesini, e-posta bağlantılar, vb. göstermek) etkinleştirebilirsiniz.
- Varsayılan olarak, tarayıcı göre doğru biçimi kullanarak bir veri oluşturmaz, [yerel ayar](https://msdn.microsoft.com/library/vstudio/wyzd2bce.aspx).
- [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) özniteliği verileri işlemek için sağ alan şablon seçmek MVC etkinleştirebilir ( [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) tarafından kullanılan kendisini dize şablonu kullanıyorsa). Daha fazla bilgi için Brad Wilson'ın bkz [ASP.NET MVC 2 şablonları](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html). (MVC 2 için yazılmış olsa, bu makalede hala ASP.NET MVC geçerli sürümü için geçerlidir.)

Kullanırsanız `DataType` özniteliği belirtmek zorunda tarih alanıyla `DisplayFormat` ayrıca alanın doğru Chrome tarayıcılarda işler sağlamak için öznitelik. Daha fazla bilgi için bkz: [bu StackOverflow iş parçacığı](http://stackoverflow.com/questions/12633471/mvc4-datatype-date-editorfor-wont-display-date-value-in-chrome-fine-in-ie).

> [!NOTE]
> jQuery doğrulama ile çalışmıyor [aralığı](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) özniteliği ve [DateTime](https://msdn.microsoft.com/library/system.datetime.aspx). Örneğin, tarih belirtilen aralıkta olduğunda bile aşağıdaki kodu her zaman bir istemci tarafı doğrulama hatası görüntülenir:
> 
> [!code-csharp[Main](adding-validation/samples/sample9.cs)]
> 
> JQuery tarih doğrulama kullanmak için devre dışı bırakmanız gerekir [aralığı](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) ile öznitelik [DateTime](https://msdn.microsoft.com/library/system.datetime.aspx). Bu genellikle bunu kullanarak Modellerinizi sabit tarihler derlemek için iyi bir uygulama değil [aralığı](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) özniteliği ve [DateTime](https://msdn.microsoft.com/library/system.datetime.aspx) önerilmez.


Aşağıdaki kod, tek bir satırda birleştirme öznitelikleri gösterir:

[!code-csharp[Main](adding-validation/samples/sample10.cs?highlight=6,10)]

Serinin sonraki bölümünde, biz uygulama gözden geçirin ve bazı geliştirmeler otomatik olarak oluşturulan yapmak `Details` ve `Delete` yöntemleri.

> [!div class="step-by-step"]
> [Önceki](adding-a-new-field.md)
> [sonraki](examining-the-details-and-delete-methods.md)
