# <a name="adding-validation"></a>Doğrulama ekleme

tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Bu bölümde için doğrulama mantığını ekleyeceksiniz `Movie` modeli, emin olun ve bir kullanıcı oluşturur veya bir filmi düzenler dilediğiniz zaman doğrulama kuralları zorunlu tutulmaz.

## <a name="keeping-things-dry"></a>Şeyler KURU tutma

MVC, tasarım tenets biri [KURU](https://wikipedia.org/wiki/Don%27t_repeat_yourself) ("verme yineleyin kendiniz"). ASP.NET MVC, işlev veya davranış yalnızca bir kez belirtin ve her yerde bir uygulamada yansıtılması sonra sağlamak için önerir. Bu kod yazmanız gereken miktarını azaltır ve daha az hata eğilimindedir, test etmek daha kolay ve sürdürmek daha kolay yazma kod yapar.

MVC ve Entity Framework Çekirdek Code First tarafından sağlanan doğrulama desteği, uygulamada KURU İlkesi iyi bir örnektir. Tek bir yerde (modeli sınıfında) doğrulama kuralları bildirimli olarak belirtebilirsiniz ve kuralların her yerde uygulamada uygulanır.

## <a name="adding-validation-rules-to-the-movie-model"></a>Film model için doğrulama kuralları ekleme

Açık *Movie.cs* dosya. DataAnnotations, herhangi bir sınıf veya özellik bildirimli olarak uygulanan doğrulama öznitelikleri yerleşik bir dizi sağlar. (Ayrıca gibi biçimlendirme özniteliklerini içeren `DataType` Yardım biçimlendirmesiyle ve tüm doğrulama sağlaması gerekmez.)

Güncelleştirme `Movie` yerleşik yararlanmak için sınıf `Required`, `StringLength`, `RegularExpression`, ve `Range` doğrulama öznitelikleri.

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc//sample/MvcMovie21/Models/MovieDateRatingDA.cs?name=snippet1)]
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Models/MovieDateRatingDA.cs?name=snippet1)]
::: moniker-end

Doğrulama öznitelikleri uygulanan model özellikleri zorlayan istediğiniz davranışı belirtin. `Required` Ve `MinimumLength` öznitelikleri gösteren bir özelliği bir değer; olması gerekir, ancak hiçbir şey bu doğrulama karşılamak için boşluk girişini kullanıcı engeller. `RegularExpression` Özniteliği ne karakter olabilir sınırlamak için kullanılır giriş. Yukarıdaki kod `Genre` ve `Rating` yalnızca harf (beyaz alan, sayı ve özel karakterler kullanılamaz) kullanmanız gerekir. `Range` Özniteliği için bir değer belirtilen aralıkta kısıtlar. `StringLength` Özniteliği bir dize özelliği en büyük uzunluğu ve isteğe bağlı olarak, minimum uzunluğu ayarlamanıza olanak tanır. Değer türleri (gibi `decimal`, `int`, `float`, `DateTime`) kendiliğinden gereklidir ve gerekmeyen `[Required]` özniteliği.

Doğrulama kuralları otomatik olarak sahip uygulamanızı daha güçlü ASP.NET yardımcı olun tarafından zorunlu tutulur. Ayrıca, bir şey doğrulamak ve yanlışlıkla hatalı veri veritabanına izin unuttunuz olamaz sağlar.

## <a name="validation-error-ui-in-mvc"></a>MVC kullanıcı Arabiriminde doğrulama hatası

Uygulamayı çalıştırın ve filmler denetleyiciye gidin.

Dokunun **Yeni Oluştur** yeni film eklemek için bağlantı. Bazı geçersiz değerlerle formu doldurun. JQuery istemci tarafı doğrulama hatası algılarsa hemen bir hata iletisi görüntüler.

![Film birden çok jQuery istemci tarafı doğrulama hataları olan formu görüntüle](~/tutorials/first-mvc-app/validation/_static/val.png)

> [!NOTE]
> Ondalık virgül kullanımı girmeniz mümkün olmayabilir `Price` alan. Desteklemek için [jQuery doğrulama](https://jqueryvalidation.org/) virgül İngilizce dışındaki yerel ayarlar için (",") ondalık ve ABD İngilizcesi dışındaki tarih biçimleri, uygulamanızı globalize için adımlar atmanız gerekir. Bu [GitHub sorunu 4076](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420) ondalık virgülle ekleme hakkında yönergeler için. 

Form otomatik olarak oluşturulmasını uygun doğrulama hata iletisine geçersiz bir değer içeren her bir alan nasıl dikkat edin. (Bir kullanıcının JavaScript devre dışı olduğundan durumda) hataları istemci-tarafı (JavaScript ve Jquery'de kullanarak) ve sunucu tarafı uygulanır.

Önemli avantajdır kodda tek bir satırı değiştirmek ihtiyacım kalmadı `MoviesController` sınıfı veya *Create.cshtml* bu doğrulama kullanıcı arabirimini etkinleştirmek için görünümü. Denetleyici ve görünümler otomatik olarak Bu öğreticide daha önce oluşturduğunuz doğrulama kuralları özellikleri üzerinde doğrulama özniteliklerini kullanarak belirtilen yukarı çekilen `Movie` model sınıfı. Doğrulama testi kullanılarak `Edit` eylem yöntemi ve aynı doğrulama uygulanır.

İstemci tarafı doğrulama hataları kadar form verilerini ve sunucuya gönderilen değil. Bu bir kesme noktası koyarak kullanılabilir doğrulayabilirsiniz `HTTP Post` kullanarak yöntemi [Fiddler aracı](http://www.telerik.com/fiddler) , veya [F12 Geliştirici Araçları](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/).

## <a name="how-validation-works"></a>Doğrulama nasıl çalışır?

Denetleyici veya görünümler kodunda herhangi bir güncelleştirme olmadan UI doğrulama nasıl oluşturulan merak ediyor. Aşağıdaki kod iki gösterir `Create` yöntemleri.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Controllers/MoviesController.cs?name=snippetCreate)]

İlk (HTTP GET) `Create` eylem yönteminin ilk form oluştur görüntüler. İkinci (`[HttpPost]`) sürüm form post işler. İkinci `Create` yöntemi ( `[HttpPost]` sürüm) çağrıları `ModelState.IsValid` film herhangi bir doğrulama hatası olup olmadığını denetlemek için. Bu yöntemin çağrılması nesneye uygulanan tüm doğrulama öznitelikleri değerlendirir. Doğrulama hataları, nesne varsa, `Create` yöntemi, formu yeniden görüntüler. Herhangi bir hata varsa, yöntem yeni film veritabanına kaydeder. İstemci tarafında algılanan doğrulama hataları olduğunda film Örneğimizde, formun sunucusuna gönderilen değil; İkinci `Create` yöntemi istemci tarafı doğrulama hataları olduğunda hiçbir zaman çağrılır. Tarayıcınızda JavaScript devre dışı bırakırsanız, istemci doğrulama devre dışıysa ve HTTP POST sınayabilirsiniz `Create` yöntemi `ModelState.IsValid` tüm doğrulama hatalarını algılama.

Bir kesme noktası ayarlayabilirsiniz `[HttpPost] Create` yöntemi ve doğrulama yöntemi hiçbir zaman çağrılır, doğrulama hataları algılandığında, istemci tarafı doğrulama form verileri gönderme olmaz. JavaScript tarayıcınızda devre dışı bırakın ve ardından hatalarla form gönderme, kesme noktası karşılaşır. Hala JavaScript olmadan tam doğrulama Al 

Aşağıdaki resimde, FireFox tarayıcısında JavaScript devre dışı bırakmak gösterilmiştir.

![Firefox: seçenekleri İçerik sekmesinde Enable Javascript onay kutusunun işaretini kaldırın.](~/tutorials/first-mvc-app/validation/_static/ff.png)

Aşağıdaki resimde, Chrome tarayıcıda JavaScript devre dışı bırakmak gösterilmiştir.

![Google Chrome: İçerik ayarları bölümüne, Javascript, select JavaScript çalıştırmak herhangi bir sitede izin vermez.](~/tutorials/first-mvc-app/validation/_static/chrome.png)

JavaScript devre dışı bıraktıktan sonra geçersiz veri ve hata ayıklayıcısı ile adım gönderin.

![Geçersiz veri post üzerinde hata ayıklama sırasında IntelliSense ModelState.IsValid üzerinde değeri yanlış olduğunu gösterir.](~/tutorials/first-mvc-app/validation/_static/ms.png)

Aşağıda bölümüdür *Create.cshtml* öğreticinin önceki bölümlerinde iskele kurulmuş şablonu görüntüle. Bu hem gösterilen eylem yöntemleri tarafından ilk form görüntülemek ve bir hata durumunda yeniden görüntülemek için kullanılır.

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Views/Movies/CreateRatingBrevity.cshtml)]

[Giriş etiketi yardımcı](xref:mvc/views/working-with-forms) kullanan [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) öznitelikleri ve istemci tarafında jQuery doğrulama için gereken HTML özniteliklerini üretir. [Doğrulama etiket Yardımcısı](xref:mvc/views/working-with-forms#the-validation-tag-helpers) doğrulama hataları görüntüler. Bkz: [doğrulama](xref:mvc/models/validation) daha fazla bilgi için.

Bu yaklaşımı hakkında gerçekten iyi nedir hiçbiri denetleyicisi olan veya `Create` şablonu görüntüleme bilir, herhangi bir şey, görüntülenen özel hata iletileri veya zorlanan gerçek doğrulama kuralları hakkında. Doğrulama kuralları ve hata dizesi yalnızca belirtilen `Movie` sınıfı. Bu aynı doğrulama kuralları otomatik olarak uygulanır `Edit` görünümü ve modelinizi düzenleme oluşturduğunuz diğer görünümleri şablonlar.

Doğrulama mantığını değiştirmeniz gerektiğinde, tek bir yerde model için doğrulama öznitelikleri ekleyerek yapabilirsiniz (Bu örnekte, `Movie` sınıfı). Kurallar nasıl zorlanır ile tutarsız olan uygulamanın farklı bölümleri hakkında endişelenmeniz gerekmez; tüm doğrulama mantığını tek bir yerde tanımlanır ve her yerde kullanılır. Bu kod çok temiz tutar ve muhafaza etmek ve gelişmesi daha kolay hale getirir. Ve, tam olarak KURU ilkesini uygularken, anlamına gelir.

## <a name="using-datatype-attributes"></a>Veri türü öznitelikleri kullanma

Açık *Movie.cs* dosya ve inceleyin `Movie` sınıfı. `System.ComponentModel.DataAnnotations` Ad alanı, yerleşik doğrulama öznitelikleri kümesi yanı sıra biçimlendirme öznitelikleri sağlar. Zaten uyguladık bir `DataType` numaralandırma değeri yayın tarihi ve fiyat alanları. Aşağıdaki kodda gösterildiği `ReleaseDate` ve `Price` uygun özelliklerle `DataType` özniteliği.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Models/MovieDateRatingDA.cs?highlight=2,6&name=snippet2)]

`DataType` Öznitelikler verilerin biçimlendirilmesi görünüm altyapısı için ipuçları yalnızca sağlar (ve öğeleri/öznitelikleri gibi kaynakları `<a>` URL'SİNİN için ve `<a href="mailto:EmailAddress.com">` e-posta için. Kullanabileceğiniz `RegularExpression` veri biçimi doğrulamak için öznitelik. `DataType` Özniteliği veritabanı geçerli bir tür daha fazla belirli bir veri türünü belirtmek için kullanılır, olmadıklarını doğrulama öznitelikleri. Bu durumda yalnızca tarih, saat izlemek istiyoruz. `DataType` Numaralandırması sağlar tarihi gibi birçok veri türleri için zaman PhoneNumber, para birimi, EmailAddress ve daha fazlası. `DataType` Özniteliği de otomatik olarak türüne özgü özellikleri sağlamak uygulama etkinleştir. Örneğin, bir `mailto:` bağlantı için oluşturulabilir `DataType.EmailAddress`, ve bir tarih seçici için sağlanan `DataType.Date` HTML5 destekleyen tarayıcılarda. `DataType` Öznitelikleri yayar HTML 5 `data-` HTML 5 tarayıcılar anlayabilirsiniz (okunur veri tire) öznitelikler. `DataType` Öznitelikleri yapmak **değil** tüm doğrulama sağlar.

`DataType.Date` Görüntülenen tarih biçimi belirtmiyor. Varsayılan olarak, sunucu üzerinde temel alan varsayılan biçimler göre veri alanı görüntülenir `CultureInfo`.

`DisplayFormat` Özniteliği açıkça tarih biçimini belirtmek için kullanılır:

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
public DateTime ReleaseDate { get; set; }
```

`ApplyFormatInEditMode` Ayar değeri düzenlemek için bir metin kutusu görüntülendiğinde biçimlendirmeyi de uygulanması gerektiğini belirtir. (, Bazı alanlar için istemeyebilirsiniz — Örneğin, para birimi değerleri için büyük olasılıkla metin kutusundaki para birimi simgesini düzenleme için istemediğiniz.)

Kullanabileceğiniz `DisplayFormat` kendisi, ancak tarafından özniteliktir genellikle kullanmak iyi bir fikir `DataType` özniteliği. `DataType` Özniteliği bir ekranda işlemesini nasıl aksine veri semantiği iletir ve DisplayFormat ile elde etmezsiniz aşağıdaki avantajları sağlar:

* Tarayıcı HTML5 özellikleri (örneğin. bir Takvim denetimi, yerel ayar uygun para birimi simgesini, e-posta bağlantılar, vb. göstermek) etkinleştirebilirsiniz

* Varsayılan olarak, tarayıcı, bölgesel ayarına göre doğru biçimi kullanarak bir veri oluşturmaz.

* `DataType` Özniteliği verileri işlemek için sağ alan şablon seçmek MVC etkinleştirebilir ( `DisplayFormat` tarafından kullanılan kendisini dize şablonu kullanıyorsa).

> [!NOTE]
> jQuery doğrulama işe yaramazsa `Range` özniteliği ve `DateTime`. Örneğin, tarih belirtilen aralıkta olduğunda bile aşağıdaki kodu her zaman bir istemci tarafı doğrulama hatası görüntülenir:

```csharp
[Range(typeof(DateTime), "1/1/1966", "1/1/2020")]
   ```

JQuery tarih doğrulama kullanmak için devre dışı bırakmanız gerekir `Range` ile öznitelik `DateTime`. Bu genellikle bunu kullanarak Modellerinizi sabit tarihler derlemek için iyi bir uygulama değil `Range` özniteliği ve `DateTime` önerilmez.

Aşağıdaki kod, tek bir satırda birleştirme öznitelikleri gösterir:

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc//sample/MvcMovie21/Models/MovieDateRatingDAmult.cs?name=snippet1)]

::: moniker-end
::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Models/MovieDateRatingDAmult.cs?name=snippet1)]

::: moniker-end

Serinin sonraki bölümünde, biz uygulama gözden geçirin ve bazı geliştirmeler otomatik olarak oluşturulan yapmak `Details` ve `Delete` yöntemleri.

## <a name="additional-resources"></a>Ek kaynaklar

* [Formlarla Çalışma](xref:mvc/views/working-with-forms)
* [Genelleştirme ve yerelleştirme](xref:fundamentals/localization)
* [Etiket Yardımcıları giriş](xref:mvc/views/tag-helpers/intro)
* [Yazar etiket Yardımcıları](xref:mvc/views/tag-helpers/authoring)
