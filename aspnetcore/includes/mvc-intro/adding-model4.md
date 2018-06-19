Vurgulanan gösterildiği eklenmesini film veritabanı bağlamı kod [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcı (içinde *haline* dosyası). `services.AddDbContext<MvcMovieContext>(options =>` Kullanım ve bağlantı dizesi veritabanına belirtir. `=>` olan bir [lambda işleci](/dotnet/articles/csharp/language-reference/operators/lambda-operator).

Açık *Controllers/MoviesController.cs* dosya ve Oluşturucusu inceleyin:

<!-- l.. Make copy of Movies controller because we comment out the initial index method and update it later  -->

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_1)] 

Oluşturucusu kullanan [bağımlılık ekleme](xref:fundamentals/dependency-injection) veritabanı bağlamı eklemesine (`MvcMovieContext `) denetleyicisi içine. Veritabanı bağlamı her biri kullanılan [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) denetleyicisi yöntemleri.

<a name="strongly-typed-models-keyword-label"></a>
<a name="strongly-typed-models-and-the--keyword"></a>

## <a name="strongly-typed-models-and-the-model-keyword"></a>Modelleri'kesin türü belirtilmiş ve @model anahtar sözcüğü

Bu öğreticide daha önce nasıl bir denetleyici veri veya nesneler kullanarak bir görünüm geçirebilirsiniz gördüğünüzü `ViewData` sözlük. `ViewData` Sözlük olan bir görünüme bilgi geçirmek için kullanışlı bir geç bağlama yol sağlayan dinamik bir nesne.

MVC, model nesneleri bir görünüme kesin geçirme özelliği yazılan de sağlar. Bu yaklaşım etkinleştirir daha iyi kodunuzu denetleme zamanı kesin türü belirtilmiş. Yapı iskelesi mekanizması (kesin türü belirtilmiş bir model geçen) Bu yaklaşım kullanılan `MoviesController` sınıfı ve yöntemleri ve görünümler oluşturduğunuzda görünümleri.

Oluşturulan inceleyin `Details` yönteminde *Controllers/MoviesController.cs* dosyası:

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Controllers/MoviesController.cs?name=snippet_details)]
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_details)]
::: moniker-end


`id` Parametre genellikle rota verileri olarak geçirilir. Örneğin `http://localhost:5000/movies/details/1` ayarlar:

* Denetleyiciye `movies` denetleyici (ilk URL kesimi).
* Eyleme `details` (ikinci URL kesimi).
* 1 (son URL kesimi) kimliği.

İçinde geçebilen `id` içeren bir sorgu dizesi şu şekilde:

`http://localhost:1234/movies/details?id=1`

`id` Parametre olarak tanımlanmış bir [boş değer atanabilir tür](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) durumda bir kimlik değeri sağlanmadı.



::: moniker range=">= aspnetcore-2.1"

A [lambda ifadesi](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) için geçirilen `FirstOrDefaultAsync` rota verileri veya sorgu dize değerinin eşleşmesi film varlıkları seçin.

```csharp
var movie = await _context.Movie
    .FirstOrDefaultAsync(m => m.ID == id);
```
::: moniker-end

::: moniker range="<= aspnetcore-2.0"

A [lambda ifadesi](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) için geçirilen `SingleOrDefaultAsync` rota verileri veya sorgu dize değerinin eşleşmesi film varlıkları seçin.

```csharp
var movie = await _context.Movie
    .SingleOrDefaultAsync(m => m.ID == id);
```
::: moniker-end



Bir filmi bulunursa örneği `Movie` modeli iletilir `Details` görünümü:

```csharp
return View(movie);
   ```

İçeriğini incelemek *Views/Movies/Details.cshtml* dosyası:

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/DetailsOriginal.cshtml)]

Ekleyerek bir `@model` deyimini dosyanın üst kısmındaki görünümü Görünüm bekliyor nesne türünü belirtebilirsiniz. Film denetleyicisini oluşturduğunuzda, Visual Studio otomatik olarak aşağıdaki dahil `@model` deyimi en üstündeki *Details.cshtml* dosyası:

```HTML
@model MvcMovie.Models.Movie
   ```

Bu `@model` yönergesi kullanarak denetleyici görünüm tarafından geçirilen film erişmenize olanak sağlayan bir `Model` kesin türü belirtilmiş nesnesi. Örneğin, *Details.cshtml* görünümü, kodu her film alanına geçirir `DisplayNameFor` ve `DisplayFor` HTML Yardımcıları ile güçlü şekilde yazılan `Model` nesnesi. `Create` Ve `Edit` yöntemlere ve görünümleri de geçirmek bir `Movie` model nesnesi.

İncelemek *Index.cshtml* Görünüm ve `Index` filmler denetleyicisi yöntemi. Kodu nasıl oluşturduğunu fark bir `List` nesne çağırdığında `View` yöntemi. Bu kod iletir `Movies` dan listesinde `Index` eylem yöntemi görüntülemek için:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_index)]

Film denetleyicisini oluşturduğunuzda, otomatik olarak iskele aşağıdaki dahil `@model` deyimi en üstündeki *Index.cshtml* dosyası:

<!-- Copy Index.cshtml to IndexOriginal.cshtml -->

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexOriginal.cshtml?range=1)]

`@model` Yönergesi kullanarak denetleyici görünüm tarafından geçirilen filmler listesi erişmenize olanak sağlayan bir `Model` kesin türü belirtilmiş nesnesi. Örneğin, *Index.cshtml* görüntülemek, kod döngüler ile filmler aracılığıyla bir `foreach` kesin türü belirtilmiş üzerinden deyimi `Model` nesnesi:

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexOriginal.cshtml?highlight=1,31,34,37,40,43,46-48)]

Çünkü `Model` nesne kesin türü belirtilmiş (olarak bir `IEnumerable<Movie>` nesnesi), döngünün her öğe olarak yazılan `Movie`. Diğer avantajlar arasında bu derleme zamanı elde anlamına gelir kodunu denetleme:
