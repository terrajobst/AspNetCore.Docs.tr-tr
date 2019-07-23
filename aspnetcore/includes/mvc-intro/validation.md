# <a name="add-validation-to-an-aspnet-core-mvc-app"></a>ASP.NET Core MVC uygulamasına doğrulama ekleme

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Bu bölümde, `Movie` modele doğrulama mantığı ekleyeceksiniz ve bir Kullanıcı bir filmi oluştururken veya düzenleişinizde doğrulama kurallarının uygulanmasını sağlayabilirsiniz.

## <a name="keeping-things-dry"></a>İşleri güncel tutma

MVC 'nin tasarımdan [biri ("](https://wikipedia.org/wiki/Don%27t_repeat_yourself) kendini tekrarlama"). ASP.NET Core MVC, işlevselliği veya davranışı yalnızca bir kez belirtmenizi ve bir uygulamada her yerde yansıtıldığını önerir. Bu, yazmanız gereken kod miktarını azaltır ve daha az hata yazmanızı, daha kolay test yapmayı ve bakımını daha kolay hale getirir.

MVC ve Entity Framework Core Code First tarafından sunulan doğrulama desteği, işlem içindeki kuru ilkeye uygun bir örnektir. Doğrulama kurallarını tek bir yerde (model sınıfında) bildirimli olarak belirtebilir ve kurallar uygulamada her yerde zorlanır.

## <a name="adding-validation-rules-to-the-movie-model"></a>Film modeline doğrulama kuralları ekleme

*Movie.cs* dosyasını açın. Veri açıklamaları, herhangi bir sınıfa veya özelliğe bildirimli olarak uygulayabileceğiniz yerleşik bir doğrulama öznitelikleri kümesi sağlar. (Ayrıca biçimlendirme ile ilgili yardım ve `DataType` herhangi bir doğrulama sağlamayan gibi biçimlendirme özniteliklerini de içerir.)

Yerleşik`Required`, ,`RegularExpression`vedoğrulama özniteliklerinden yararlanmak için sınıfıgüncelleştirin.`Movie` `StringLength` `Range`

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc//sample/MvcMovie21/Models/MovieDateRatingDA.cs?name=snippet1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Models/MovieDateRatingDA.cs?name=snippet1)]

::: moniker-end

Doğrulama öznitelikleri, uygulandığı model özellikleri üzerinde zorlamak istediğiniz davranışı belirtir. `Required` Ve`MinimumLength` öznitelikleri bir özelliğin bir değere sahip olması gerektiğini belirtir; ancak hiçbir şey, bir kullanıcının bu doğrulamayı karşılamak için boşluk girmesini engeller. `RegularExpression` Öznitelik, hangi karakterlerin girişi yapabileceğini sınırlamak için kullanılır. `Genre` Yukarıdaki`Rating` kodda yalnızca harfler kullanılmalıdır (ilk harfi büyük harf, boşluk, sayı ve özel karakterlere izin verilmez). `Range` Özniteliği bir değeri belirtilen bir Aralık içinde kısıtlar. `StringLength` Özniteliği, bir dize özelliğinin en büyük uzunluğunu ve isteğe bağlı olarak en düşük uzunluğunu ayarlamanıza olanak sağlar. Değer türleri (örneğin, `decimal`, `int`, `float` `DateTime`), doğal olarak gereklidir ve `[Required]` özniteliğe gerek kalmaz.

Doğrulama kurallarının otomatik olarak uygulanmasını ASP.NET Core uygulamanızın daha sağlam olmasına yardımcı olur. Ayrıca, bir şeyi doğrulamayı unutmanızı ve veritabanına yanlışlıkla veri vermemesini de sağlar.

## <a name="validation-error-ui-in-mvc"></a>MVC 'de doğrulama hatası Kullanıcı arabirimi

Uygulamayı çalıştırın ve filmler denetleyicisine gidin.

Yeni bir film eklemek için **Yeni oluştur** bağlantısına dokunun. Formu, bazı geçersiz değerlerle doldurun. JQuery istemci tarafı doğrulaması hatayı algıladıktan hemen sonra bir hata iletisi görüntüler.

![Birden çok jQuery istemci tarafı doğrulama hatası içeren film görünümü formu](~/tutorials/first-mvc-app/validation/_static/val.png)

[!INCLUDE[](~/includes/currency.md)]

Formun, geçersiz bir değer içeren her bir alanda uygun bir doğrulama hata iletisini nasıl otomatik olarak oluşturduğuna dikkat edin. Hatalar hem istemci tarafında (JavaScript ve jQuery kullanılarak) hem de sunucu tarafında (kullanıcının JavaScript devre dışı bırakılmış olması durumunda) zorlanır.

Önemli bir avantaj, bu doğrulama kullanıcı arabirimini etkinleştirmek için `MoviesController` sınıfında veya *Create. cshtml* görünümündeki tek bir kod satırını değiştirmeniz gerekmez. Bu öğreticide daha önce oluşturduğunuz denetleyici ve görünümler, `Movie` model sınıfının özelliklerinde doğrulama özniteliklerini kullanarak belirttiğiniz doğrulama kurallarını otomatik olarak çekti. `Edit` Eylem yöntemini kullanarak test doğrulama ve aynı doğrulama uygulandı.

Form verileri, istemci tarafı doğrulama hatası kalmayana kadar sunucuya gönderilmez. Bunu, `HTTP Post` [Fiddler aracını](https://www.telerik.com/fiddler) ya da [F12 geliştirici araçlarını](/microsoft-edge/devtools-guide)kullanarak yöntemine bir kesme noktası koyarak doğrulayabilirsiniz.

## <a name="how-validation-works"></a>Doğrulamanın çalışması

Doğrulama Kullanıcı arabiriminin denetleyici veya görünümlerde kodda herhangi bir güncelleştirme yapmadan nasıl oluşturulduğunu merak edebilirsiniz. Aşağıdaki kod iki `Create` yöntemi gösterir.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Controllers/MoviesController.cs?name=snippetCreate)]

İlk (http get) `Create` eylem yöntemi ilk oluşturma formunu görüntüler. İkinci (`[HttpPost]`) sürüm, form gönderisini işler. İkinci `Create` Yöntem `ModelState.IsValid` (sürüm), filmin herhangi bir doğrulama hatası olup olmadığını denetlemek için çağırır. `[HttpPost]` Bu yöntemi çağırmak, nesnesine uygulanmış olan tüm doğrulama özniteliklerini değerlendirir. Nesne doğrulama hatalarıyla karşılaşırsanız, `Create` yöntemi formu yeniden görüntüler. Hata yoksa, yöntemi yeni filmi veritabanına kaydeder. Film örneğimizde, istemci tarafında algılanan doğrulama hataları olduğunda form sunucuya nakledilmez; İkinci `Create` Yöntem, istemci tarafı doğrulama hataları olduğunda hiçbir zaman çağrılmaz. Tarayıcınızda JavaScript 'i devre dışı bırakırsanız, istemci doğrulaması devre dışıdır ve herhangi bir doğrulama hatasını tespit etmek için `Create` http `ModelState.IsValid` Post yöntemini test edebilirsiniz.

`[HttpPost] Create` Yönteminde bir kesme noktası ayarlayabilir ve yöntemin hiçbir zaman çağrılmadığını doğrulayabilirsiniz, istemci tarafı doğrulama, doğrulama hataları algılandığında form verilerini göndermez. Tarayıcınızda JavaScript 'i devre dışı bırakır, ardından formu hatalarla gönderirseniz, kesme noktası isabet eder. JavaScript olmadan tam doğrulama almaya devam edersiniz. 

Aşağıdaki görüntüde, FireFox tarayıcısında JavaScript 'In nasıl devre dışı bırakılacağı gösterilmektedir.

!['U Seçeneklerin Içerik sekmesinde, JavaScript 'i etkinleştir onay kutusunun işaretini kaldırın.](~/tutorials/first-mvc-app/validation/_static/ff.png)

Aşağıdaki görüntüde, Chrome tarayıcısında JavaScript 'In nasıl devre dışı bırakılacağı gösterilmektedir.

![Google Chrome: Içerik ayarlarının JavaScript bölümünde herhangi bir sitenin JavaScript çalıştırmasına izin verme ' yi seçin.](~/tutorials/first-mvc-app/validation/_static/chrome.png)

JavaScript 'i devre dışı bıraktıktan sonra, geçersiz veri gönderin ve hata ayıklayıcıda adım adım ilerleyin.

![Geçersiz verilerin bir göndermasında hata ayıklarken, ModelState. IsValid üzerinde IntelliSense, değerin false olduğunu gösterir.](~/tutorials/first-mvc-app/validation/_static/ms.png)

Öğreticide daha önce iskele aldığınız *Create. cshtml* görünüm şablonunun bir parçası aşağıda verilmiştir. İlk formu görüntülemek ve bir hata durumunda onu yeniden görüntülemek için yukarıda gösterilen eylem yöntemleri tarafından kullanılır.

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Views/Movies/CreateRatingBrevity.cshtml)]

[Giriş etiketi Yardımcısı](xref:mvc/views/working-with-forms) , [dataaçıklamaların](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) özniteliklerini kullanır ve istemci tarafında jQuery doğrulaması için gerekli HTML özniteliklerini üretir. [Doğrulama etiketi Yardımcısı](xref:mvc/views/working-with-forms#the-validation-tag-helpers) doğrulama hatalarını görüntüler. Daha fazla bilgi için bkz. [doğrulama](xref:mvc/models/validation) .

Bu yaklaşım ne kadar iyi bir şeydir, denetleyicinin ne de `Create` görünüm şablonu zorlanmakta olan gerçek doğrulama kuralları veya görüntülenen belirli hata iletileri hakkında herhangi bir şeyi bilmez. Doğrulama kuralları ve hata dizeleri yalnızca `Movie` sınıfında belirtilmiştir. Bu aynı doğrulama kuralları, `Edit` bir görünüme ve modelinizi düzenleyebilecek oluşturabileceğiniz diğer tüm görünümler şablonlarına otomatik olarak uygulanır.

Doğrulama mantığını değiştirmeniz gerektiğinde, modele doğrulama öznitelikleri ekleyerek tam olarak bir yerde bunu yapabilirsiniz (Bu örnekte, `Movie` sınıfı). Kuralların nasıl zorlandığından, uygulamanın farklı bölümlerinin tutarsız olması konusunda endişelenmeniz gerekmez; tüm doğrulama mantığı tek bir yerde tanımlanır ve her yerde kullanılır. Bu, kodun temiz kalmasını sağlar ve bakımını ve gelişmesini kolaylaştırır. Ayrıca, KURULAMA ilkesini tam olarak sunabileceksiniz anlamına gelir.

## <a name="using-datatype-attributes"></a>DataType özniteliklerini kullanma

*Movie.cs* dosyasını açın ve `Movie` sınıfını inceleyin. Ad `System.ComponentModel.DataAnnotations` alanı, yerleşik doğrulama öznitelikleri kümesine ek olarak biçimlendirme öznitelikleri sağlar. Yayın tarihine ve fiyat alanlarına `DataType` bir numaralandırma değeri zaten uyguladık. Aşağıdaki kod, `ReleaseDate` uygun `DataType` özniteliğiyle ve `Price` özelliklerini gösterir.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Models/MovieDateRatingDA.cs?highlight=2,6&name=snippet2)]

Öznitelikler yalnızca görünüm altyapısının verileri biçimlendirmek için ipuçları sağlar (ve URL 'ler ve `<a href="mailto:EmailAddress.com">` e-posta için gibi `<a>` öğeleri/öznitelikleri sağlar. `DataType` Veri biçimini doğrulamak için `RegularExpression` özniteliğini kullanabilirsiniz. `DataType` Özniteliği, veritabanı iç türünden daha belirgin bir veri türü belirtmek için kullanılır, bunlar doğrulama öznitelikleri değildir. Bu durumda, zamanı değil yalnızca tarihi izlemek istiyoruz. `DataType` Sabit listesi, tarih, saat, PhoneNumber, para birimi, emaadresi ve daha fazlası gibi birçok veri türünü sağlar. `DataType` Özniteliği Ayrıca uygulamanın türe özgü özellikleri otomatik olarak sağlamasını da sağlayabilir. Örneğin, için `DataType.EmailAddress`bir `mailto:` bağlantı oluşturulabilir ve HTML5 'i destekleyen tarayıcılarda için `DataType.Date` bir tarih seçici sağlaneklenebilir. Öznitelikler HTML 5 tarayıcılarının anlayabilmesi için HTML 5 `data-` (bir veri Dash) öznitelikleri yayar. `DataType` Öznitelikler herhangi bir **doğrulama sağlamaz.** `DataType`

`DataType.Date`görüntülenen tarihin biçimini belirtmez. Varsayılan olarak, veri alanı sunucu `CultureInfo`' a göre varsayılan biçimlere göre görüntülenir.

`DisplayFormat` Öznitelik, tarih biçimini açıkça belirtmek için kullanılır:

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
public DateTime ReleaseDate { get; set; }
```

`ApplyFormatInEditMode` Ayar, bir metin kutusunda değer görüntülenmek üzere görüntülendiğinde biçimlendirmenin de uygulanacağını belirtir. (Örneğin, para birimi değerleri için, büyük olasılıkla, metin kutusundaki para birimi sembolünü, bazı alanlar için istemiyor olabilirsiniz.)

`DisplayFormat` Özniteliğini kendi başına kullanabilirsiniz, ancak genellikle `DataType` özniteliği kullanmak iyi bir fikirdir. `DataType` Özniteliği, bir ekranda nasıl işlenirim aksine, verilerin semantiğini alır ve DisplayFormat ile elde olmadığınız avantajları sağlar:

* Tarayıcı HTML5 özelliklerini etkinleştirebilir (örneğin, bir Takvim denetimini, yerel ayara uygun para birimi sembolünü, e-posta bağlantılarını vb. göstermek için)

* Varsayılan olarak tarayıcı, verileri yerel ayarınızı temel alarak doğru biçimi kullanarak işleyebilir.

* Özniteliği, verileri işlemek için doğru alan şablonunu seçmek üzere MVC 'yi etkinleştirebilir (kendisi tarafından kullanılıyorsa `DisplayFormat` , dize şablonunu kullanıyorsa). `DataType`

> [!NOTE]
> jQuery doğrulaması, `Range` ve `DateTime`özniteliğiyle çalışmaz. Örneğin, aşağıdaki kod, tarih belirtilen aralıkta olduğunda bile her zaman bir istemci tarafı doğrulama hatası görüntüler:

```csharp
[Range(typeof(DateTime), "1/1/1966", "1/1/2020")]
   ```

`Range` Özniteliğini ile`DateTime`kullanmak için jQuery Tarih doğrulamasını devre dışı bırakmanız gerekir. Genellikle, modellerinizde sabit tarihleri derlemek iyi bir uygulamadır, bu nedenle `Range` özniteliğini kullanarak ve `DateTime` önerilmez.

Aşağıdaki kod, öznitelikleri tek bir satırda birleştirmeyi gösterir:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc//sample/MvcMovie21/Models/MovieDateRatingDAmult.cs?name=snippet1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Models/MovieDateRatingDAmult.cs?name=snippet1)]

::: moniker-end

Serinin bir sonraki bölümünde, uygulamayı gözden geçireceğiz ve otomatik olarak oluşturulan `Details` ve `Delete` metotlarda bazı geliştirmeler yaparsınız.

## <a name="additional-resources"></a>Ek kaynaklar

* [Formlarla Çalışma](xref:mvc/views/working-with-forms)
* [Genelleştirme ve yerelleştirme](xref:fundamentals/localization)
* [Etiket yardımcılarına giriş](xref:mvc/views/tag-helpers/intro)
* [Yazar etiketi yardımcıları](xref:mvc/views/tag-helpers/authoring)
