---
title: Bir ASP.NET Core Razor sayfasına doğrulama ekleme
author: rick-anderson
description: ASP.NET Core bir Razor sayfasına doğrulama ekleme bulur.
manager: wpickett
ms.author: riande
ms.date: 08/07/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/validation
ms.openlocfilehash: 271a5ce517ae550845d96e3969b39b1eda6ae51b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="add-validation-to-an-aspnet-core-razor-page"></a>Bir ASP.NET Core Razor sayfasına doğrulama ekleme

tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Bu bölümde, doğrulama mantığını eklenen `Movie` modeli. Doğrulama kuralları, bir kullanıcı oluşturur veya bir filmi düzenler dilediğiniz zaman uygulanır.

## <a name="validation"></a>Doğrulama

Yazılım geliştirme anahtar tenet adlı [KURU](https://wikipedia.org/wiki/Don%27t_repeat_yourself) ("**D**aşağıdakilerden **R**epeat **Y**ourself"). Razor sayfalarının burada işlevselliği bir kez belirtildiğinden ve uygulama boyunca yansıtılır geliştirme önerir. KURU uygulama kodunda sayısını azaltmanıza yardımcı. KURU kod daha az hata eğilimindedir ve test ve sürdürmek daha kolay yapar.

Razor sayfalarının ve Entity Framework tarafından sağlanan doğrulama desteği KURU İlkesi iyi bir örnektir. Doğrulama kuralları bildirimli olarak tek bir yerde (modeli sınıfı) belirtilir ve kuralların her yerde uygulamada uygulanır.

### <a name="adding-validation-rules-to-the-movie-model"></a>Film model için doğrulama kuralları ekleme

Açık *Movie.cs* dosya. [DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) yerleşik bir sınıf veya özellik bildirimli olarak uygulanan doğrulama öznitelikleri kümesi sağlar. DataAnnotations de içeren biçimlendirme öznitelikleri gibi `DataType` Yardım biçimlendirmesiyle ve doğrulama sağlaması gerekmez.

Güncelleştirme `Movie` yararlanmak için sınıf `Required`, `StringLength`, `RegularExpression`, ve `Range` doğrulama öznitelikleri.

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Models/MovieDateRatingDA.cs?name=snippet1)]

Doğrulama öznitelikleri zorlanır davranışı model özellikleri belirtin:

* `Required` Ve `MinimumLength` öznitelikleri gösteren bir özelliği bir değere sahip olmalıdır. Ancak, hiçbir şey kullanıcı null atanabilir bir tür için doğrulama kısıtlamasını boşluk geçmesini engeller. Olamayan [değer türleri](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/value-types) (gibi `decimal`, `int`, `float`, ve `DateTime`) kendiliğinden gereklidir ve gerekmeyen `Required` özniteliği.
* `RegularExpression` Öznitelik, kullanıcının girebileceği karakterleri sınırlar. Önceki kod `Genre` ve `Rating` yalnızca harf kullanmanız gerekir (boşluk, rakam ve özel karakterler izin verilmiyor).
* `Range` Özniteliği belirli bir aralık için bir değer kısıtlar.
* `StringLength` Öznitelik, bir dize uzunluğu en fazla ve isteğe bağlı olarak en az uzunluk ayarlar. 

Doğrulama kuralları otomatik olarak ASP.NET Core tarafından zorlanan sahip yardımcı olan bir uygulama daha sağlam hale. Otomatik doğrulama modellerinde yeni kod eklendiğinde, bunları uygulamak olmadığından uygulama korunmasına yardımcı olur.

### <a name="validation-error-ui-in-razor-pages"></a>Doğrulama hatası Razor sayfalarında kullanıcı Arabirimi

Uygulamayı çalıştırın ve sayfaları/filmlere gidin.

Seçin **Yeni Oluştur** bağlantı. Bazı geçersiz değerlerle formu doldurun. JQuery istemci tarafı doğrulama hatası algıladığında, bir hata iletisi görüntüler.

![Film birden çok jQuery istemci tarafı doğrulama hataları olan formu görüntüle](validation/_static/val.png)

> [!NOTE]
> Ondalık ayırıcıların veya virgül kullanımı girmeniz mümkün olmayabilir `Price` alan. Desteklemek için [jQuery doğrulama](https://jqueryvalidation.org/) virgül İngilizce dışındaki yerel ayarlarda (",") ondalık ve ABD İngilizcesi dışındaki tarih biçimleri için uygulamanızı globalize için adımlar atmanız gerekir. Bkz: [ek kaynaklar](#additional-resources) daha fazla bilgi için. Şimdilik, yalnızca tam sayılar 10 gibi girin.

Form otomatik olarak işlenen geçersiz bir değer içeren her bir alan bir doğrulama hata iletisine nasıl dikkat edin. (Bir kullanıcının JavaScript devre dışı olduğunda) hataları istemci-tarafı (JavaScript ve Jquery'de kullanarak) ve sunucu tarafı uygulanır.

Önemli faydası ise **hiçbir** kod değişiklikleri oluşturma veya düzenleme sayfalarında gerekli. DataAnnotations modeline uygulanan sonra UI doğrulama etkinleştirildi. Bu öğreticide otomatik olarak oluşturulan Razor sayfalarının çekilen doğrulama kurallarını (doğrulama öznitelikleri özelliklerini kullanarak `Movie` model sınıfı). Sınama doğrulaması aynı doğrulama düzen sayfası kullanılarak uygulanır.

İstemci tarafı doğrulama hataları kadar form verilerini sunucuya gönderilen değil. Form verileri bir veya daha fazla aşağıdaki yaklaşımlardan tarafından gönderilen değil doğrulayın:

* Bir kesme noktası koyun `OnPostAsync` yöntemi. Form gönderme (seçin **oluşturma** veya **kaydetmek**). Kesme noktası hiçbir zaman ulaştı.
* Kullanım [Fiddler aracı](http://www.telerik.com/fiddler).
* Tarayıcının geliştirici araçları, ağ trafiğini izlemek için kullanın.

### <a name="server-side-validation"></a>Sunucu tarafında doğrulama

JavaScript tarayıcıda devre dışı bırakıldığında, hatalarla form gönderme sunucuya gönderin.

İsteğe bağlı, test sunucu tarafında doğrulama:

* JavaScript tarayıcıda devre dışı bırakın. Tarayıcıda JavaScript devre dışı bırakamazsınız, başka bir tarayıcı deneyin.
* Bir kesme noktası kümesinde `OnPostAsync` oluşturma veya düzenleme sayfasının yöntemi.
* Doğrulama hataları olan bir form gönderme.
* Model durumu geçersiz doğrulayın:

  ```csharp
   if (!ModelState.IsValid)
   {
      return Page();
   }
  ```

Aşağıdaki kod bir kısmı gösterir *Create.cshtml* öğreticinin önceki bölümlerinde iskele kurulmuş sayfası. İlk form görüntülemek ve bir hata durumunda formu yeniden görüntüleyin, oluşturma ve düzenleme sayfaları tarafından kullanılır.

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=14-20)]

[Giriş etiketi yardımcı](xref:mvc/views/working-with-forms) kullanan [DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) öznitelikleri ve istemci tarafında jQuery doğrulama için gereken HTML özniteliklerini üretir. [Doğrulama etiket Yardımcısı](xref:mvc/views/working-with-forms#the-validation-tag-helpers) doğrulama hataları görüntüler. Bkz: [doğrulama](xref:mvc/models/validation) daha fazla bilgi için.

Oluşturma ve düzenleme sayfaları hiçbir doğrulama kuralları olması. Doğrulama kuralları ve hata dizesi yalnızca belirtilen `Movie` sınıfı. Bu doğrulama kuralları otomatik olarak Düzenle Razor sayfalara uygulanır `Movie` modeli.

Doğrulama mantığını değiştirmek gerektiğinde, model yalnızca yapılır. Doğrulama uygulama genelinde tutarlı olarak uygulandığı (tek bir yerde doğrulama mantığını tanımlanmış). Tek bir yerde doğrulama kodu temiz tutmaya yardımcı olur ve bakımını yapmak ve güncelleştirmek kolaylaştırır.

## <a name="using-datatype-attributes"></a>Veri türü öznitelikleri kullanma

İncelemek `Movie` sınıfı. `System.ComponentModel.DataAnnotations` Ad alanı, yerleşik doğrulama öznitelikleri kümesi yanı sıra biçimlendirme öznitelikleri sağlar. `DataType` Özniteliği uygulanan `ReleaseDate` ve `Price` özellikleri.

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRatingDA.cs?highlight=2,6&name=snippet2)]

`DataType` Öznitelikler verilerin biçimlendirilmesi görünüm altyapısı için ipuçları yalnızca sağlar (ve öznitelikler gibi kaynakları `<a>` URL'SİNİN için ve `<a href="mailto:EmailAddress.com">` e-posta için). Kullanım `RegularExpression` veri biçimi doğrulamak için öznitelik. `DataType` Özniteliği veritabanı geçerli bir tür daha fazla belirli bir veri türünü belirtmek için kullanılır. `DataType` öznitelikler doğrulama öznitelikleri değildir. Örnek uygulama, yalnızca tarih, saat görüntülenir.

`DataType` Tarih, saat, PhoneNumber, para birimi, EmailAddress ve daha fazla gibi birçok veri türleri için numaralandırma sağlar. `DataType` Özniteliği de otomatik olarak türüne özgü özellikleri sağlamak uygulama etkinleştir. Örneğin, bir `mailto:` bağlantı için oluşturulabilir `DataType.EmailAddress`. Bir tarih seçici için sağlanan `DataType.Date` HTML5 destekleyen tarayıcılarda. `DataType` Öznitelikleri yayar HTML 5 `data-` HTML 5 tarayıcılar tüketebilir (okunur veri tire) öznitelikler. `DataType` Öznitelikleri yapmak **değil** tüm doğrulama sağlar.

`DataType.Date` Görüntülenen tarih biçimi belirtmiyor. Varsayılan olarak, sunucu üzerinde temel alan varsayılan biçimler göre veri alanı görüntülenir `CultureInfo`.

`DisplayFormat` Özniteliği açıkça tarih biçimini belirtmek için kullanılır:

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
public DateTime ReleaseDate { get; set; }
```

`ApplyFormatInEditMode` Ayarı, düzenleme için değer görüntülendiğinde biçimlendirme uygulanması gerektiğini belirtir. Bazı alanları için bu davranışı istemeyebilirsiniz. Örneğin, para birimi değerleri düzenleme UI para birimi simgesini büyük olasılıkla istemezsiniz.

`DisplayFormat` Özniteliği tek başına kullanılabilir, ancak genellikle kullanmak iyi bir fikir değil `DataType` özniteliği. `DataType` Özniteliği bir ekranda işlemesini nasıl aksine veri semantiği iletir ve DisplayFormat ile elde etmezsiniz aşağıdaki avantajları sağlar:

* Tarayıcı HTML5 özellikleri (örneğin. bir Takvim denetimi, yerel ayar uygun para birimi simgesini, e-posta bağlantılar, vb. göstermek) etkinleştirebilirsiniz
* Varsayılan olarak, tarayıcı, bölgesel ayarına göre doğru biçimi kullanarak bir veri oluşturmaz.
* `DataType` Özniteliği verileri işlemek için sağ alan şablon seçmek ASP.NET Core framework etkinleştirebilir. `DisplayFormat` Tarafından kullanılan kendisini dize şablonu kullanır.

Not: jQuery doğrulama ile işe yaramazsa `Range` özniteliği ve `DateTime`. Örneğin, tarih belirtilen aralıkta olduğunda bile aşağıdaki kodu her zaman bir istemci tarafı doğrulama hatası görüntülenir:

```csharp
[Range(typeof(DateTime), "1/1/1966", "1/1/2020")]
   ```

Bu genellikle bunu kullanarak Modellerinizi sabit tarihler derlemek için iyi bir uygulama değil `Range` özniteliği ve `DateTime` önerilmez.

Aşağıdaki kod, tek bir satırda birleştirme öznitelikleri gösterir:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRatingDAmult.cs?name=snippet1)]

[Razor sayfalarının ve EF çekirdek kullanmaya başlama](xref:data/ef-rp/intro) Razor sayfalarının EF çekirdek işlemleriyle daha gelişmiş gösterir.

### <a name="publish-to-azure"></a>Azure'a yayımlama

Bkz: [Visual Studio kullanarak Azure App Service için ASP.NET Core web uygulama yayımlama](xref:tutorials/publish-to-azure-webapp-using-vs) bu uygulama için Azure yayımlama hakkında yönergeler için.

## <a name="additional-resources"></a>Ek kaynaklar

* [Formlarla Çalışma](xref:mvc/views/working-with-forms)
* [Genelleştirme ve yerelleştirme](xref:fundamentals/localization)
* [Etiket Yardımcıları giriş](xref:mvc/views/tag-helpers/intro)
* [Yazar etiket Yardımcıları](xref:mvc/views/tag-helpers/authoring)

> [!div class="step-by-step"]
> [Önceki: yeni bir alan ekleyerek](xref:tutorials/razor-pages/new-field)
> [sonraki: dosyaları karşıya yükleme](xref:tutorials/razor-pages/uploading-files)
