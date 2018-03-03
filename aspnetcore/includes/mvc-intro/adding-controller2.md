Değiştir *Controllers/HelloWorldController.cs* aşağıdaki:

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_1)]

Her `public` bir denetleyici yöntemidir HTTP uç noktası olarak çağrılabilir. Yukarıdaki örnekte, her iki yöntem bir dize döndürür.  Her yöntem önceki açıklamaları unutmayın.

Web uygulaması'nda targetable bir URL bir HTTP uç noktası olduğu gibi `http://localhost:1234/HelloWorld`ve kullanılan protokol birleştirir: `HTTP`, ağ konumu (TCP bağlantı noktası dahil) web sunucusunun: `localhost:1234` ve hedef URI `HelloWorld`.

İlk yorumu durumları bu bir [HTTP GET](https://www.w3schools.com/tags/ref_httpmethods.asp) için temel URL "/HelloWorld/" ekleyerek çağrılan yöntem. İkinci açıklamayı belirtir bir [HTTP GET](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) ekleyerek çağrılan yöntemi "/ HelloWorld/Hoş Geldiniz /" için URL. Daha sonra öğreticide yapı iskelesi altyapısı oluşturmak için kullanacağınız `HTTP POST` yöntemleri.

Uygulama olmayan hata ayıklama modunda çalıştırılması ve adres çubuğundaki yoluna "HelloWorld" ekleyin. `Index` Yöntemi bir dize döndürür.

![Bu bir uygulama yanıt gösteren bir tarayıcı penceresi my varsayılan eylemdir](../../tutorials/first-mvc-app/adding-controller/_static/hell1.png)

MVC denetleyicisi sınıfları (ve bunların içindeki eylem yöntemleri) gelen URL bağlı olarak çağırır. Varsayılan [URL yönlendirme mantığı](../../mvc/controllers/routing.md) MVC tarafından bu gibi bir biçim ne çağırmak için kodu belirlemek için kullanır:

`/[Controller]/[ActionName]/[Parameters]`

Yönlendirme için biçimini ayarlama `Configure` yönteminde *haline* dosya.

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=snippet_1&highlight=5)]

Uygulamayı çalıştırın ve tüm URL kesimleri kaynağı yok, "Home" denetleyiciye Varsayılanları ve yukarıdaki vurgulanmış şablonu satırı "Index" yöntemi belirtilen.

İlk URL kesimi çalıştırmak için denetleyici sınıfını belirler. Bu nedenle `localhost:xxxx/HelloWorld` eşlendiği `HelloWorldController` sınıfı. URL kesimi ikinci bölümü sınıfı eylem yöntemine belirler. Bu nedenle `localhost:xxxx/HelloWorld/Index` neden olacağından `Index` yöntemi `HelloWorldController` çalıştırmak için sınıf. Yalnızca Gözat gerekiyordu bildirimi `localhost:xxxx/HelloWorld` ve `Index` yöntemi, varsayılan olarak çağrıldı. Bunun nedeni, `Index` bir yöntem adı açıkça belirtilmezse, bir denetleyicisinde çağrılacak varsayılan yöntemdir. URL kesimi üçüncü parçası ( `id`) için rota veri. Bu öğreticide daha sonra rota verilerini görürsünüz.

Gözat `http://localhost:xxxx/HelloWorld/Welcome`. `Welcome` Yöntemi çalışır ve "... Hoş Geldiniz eylem yöntemi olduğu" dizesini döndürür. Bu URL için denetleyicisidir `HelloWorld` ve `Welcome` eylem yöntemidir. Kullandığınız parolalardan `[Parameters]` URL henüz parçası.

![Bu bir uygulama yanıt gösteren bir tarayıcı penceresi Hoş Geldiniz eylem yöntemidir](../../tutorials/first-mvc-app/adding-controller/_static/welcome.png)

Bazı parametre bilgilerini URL'den denetleyiciye geçirmek için kodu değiştirin. Örneğin, `/HelloWorld/Welcome?name=Rick&numtimes=4`. Değişiklik `Welcome` yöntemi iki parametre aşağıdaki kodda gösterildiği gibi ekleyin. 

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_2)]

Önceki kod:

* C# isteğe bağlı parametre özelliği belirtmek için kullanılan `numTimes` parametresi varsayılan olarak 1'e bu parametre için değer iletilmezse.
* Kullanan`HtmlEncoder.Default.Encode` kötü amaçlı giriş (yani JavaScript) uygulama korumak için. 
* Kullanan [Ara değerli dizeler](https://docs.microsoft.com/dotnet/articles/csharp/language-reference/keywords/interpolated-strings).

Uygulamanızı çalıştırın ve göz atın:

   `http://localhost:xxxx/HelloWorld/Welcome?name=Rick&numtimes=4`

(Xxxx, bağlantı noktası numarasını değiştirin.) İçin farklı değerler deneyin `name` ve `numtimes` URL. MVC [model bağlama](../../mvc/models/model-binding.md) sistem adres çubuğundaki sorgu dizesi yönteminizi parametrelerinde adlandırılmış parametreleri otomatik olarak eşlenir. Bkz: [Model bağlama](../../mvc/models/model-binding.md) daha fazla bilgi için.

![Merhaba Rick uygulama yanıtın NumTimes gösteren bir tarayıcı penceresi: 4](../../tutorials/first-mvc-app/adding-controller/_static/rick4.png)

Yukarıdaki URL kesimini görüntüsündeki (`Parameters`) kullanılmaz, `name` ve `numTimes` parametre olarak geçirilir [sorgu dizeleri](https://wikipedia.org/wiki/Query_string). `?` (Soru işareti) yukarıdaki URL'de bir ayırıcı olduğu ve sorgu dizeleri izleyin. `&` Karakter sorgu dizeleri ayırır.

Değiştir `Welcome` aşağıdaki kod ile yöntemi:

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_3)]

Uygulamayı çalıştırın ve aşağıdaki URL'yi girin:  `http://localhost:xxx/HelloWorld/Welcome/3?name=Rick`

![Merhaba Rick, kimliği, bir uygulama yanıt gösteren bir tarayıcı penceresi: 3](../../tutorials/first-mvc-app/adding-controller/_static/rick_routedata.png)

Bu süre üçüncü URL kesimi eşleşen rota parametresi `id`. `Welcome` Yöntemini içeren bir parametre `id` URL şablonda eşleşen `MapRoute` yöntemi. Sondaki `?` (içinde `id?`) gösteren `id` parametresi isteğe bağlıdır.

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=snippet_1&highlight=5)]

Bu örneklerde denetleyicisi MVC "VC" bölümünü yapılması - diğer bir deyişle, iş Görünüm ve denetleyici. Denetleyici HTML doğrudan döndürüyor. Genellikle, kod ve korumak için çok kullanışsız hale beri HTML doğrudan döndürerek denetleyicileri istemezsiniz. Bunun yerine, genellikle ayrı bir Razor görünüm şablon dosyası HTML yanıtı oluşturmak amacıyla kullanın. Sonraki öğreticide bunu.
