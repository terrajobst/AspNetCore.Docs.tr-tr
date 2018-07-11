Öğesinin içeriğini değiştirin *Controllers/HelloWorldController.cs* aşağıdaki:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_1)]

Her `public` bir denetleyici yöntemi bir HTTP uç noktası olarak çağrılabilir. Yukarıdaki örnekte her iki yöntem bir dize döndürür.  Her bir yöntemin önceki yorumlar unutmayın.

Web uygulamasındaki hedeflenebilir bir URL bir HTTP uç noktası olduğu gibi `http://localhost:1234/HelloWorld`ve kullanılan protokol birleştirir: `HTTP`, (TCP bağlantı noktası dahil) web sunucusu ağ konumu: `localhost:1234` ve hedef URI `HelloWorld`.

İlk yorum durumları bu bir [HTTP GET](https://www.w3schools.com/tags/ref_httpmethods.asp) temel URL'si "/HelloWorld/" ekleyerek çağrılan yöntem. İkinci yorumu belirtir bir [HTTP GET](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) ekleyerek çağrılan yöntem "/ HelloWorld/Hoş Geldiniz /" URL'si. Daha sonra Bu öğreticide, yapı iskelesi altyapısı oluşturmak için kullanacağınız `HTTP POST` yöntemleri.

Uygulamayı hata ayıklama olmayan modda çalıştırın ve adres çubuğuna yoluna "HelloWorld" ekleyin. `Index` Yöntemi bir dize döndürür.

![Bu bir uygulama yanıtı gösteren tarayıcı penceresinde my varsayılan eylemi değildir](~/tutorials/first-mvc-app/adding-controller/_static/hell1.png)

MVC denetleyici sınıflarına (ve içlerindeki eylem yöntemleri) gelen URL bağlı olarak çağırır. Varsayılan [URL yönlendirme mantığı](xref:mvc/controllers/routing) MVC tarafından bu gibi bir biçim ne çağırmak için kod belirlemek için kullanır:

`/[Controller]/[ActionName]/[Parameters]`

Yönlendirme için biçimi ayarlayın `Configure` yönteminde *Startup.cs* dosya.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=snippet_1&highlight=5)]

Uygulamayı çalıştırın ve herhangi bir URL kesimleri kaynağı yok, "Home" denetleyiciye varsayılanlar ve "Index" yöntem yukarıda vurgulanmış olan şablon satırında belirtilen.

İlk URL kesimini çalıştırmak için denetleyici sınıfını belirler. Bu nedenle `localhost:xxxx/HelloWorld` eşlendiği `HelloWorldController` sınıfı. URL kesimi ikinci bölümü sınıfı eylem yöntemine belirler. Bu nedenle `localhost:xxxx/HelloWorld/Index` neden `Index` yöntemi `HelloWorldController` çalıştırmak için sınıf. Yalnızca gözatmak için olan bildirim `localhost:xxxx/HelloWorld` ve `Index` yöntemi varsayılan olarak çağrıldı. Bunun nedeni, `Index` bir yöntem adı açıkça belirtilmezse, bir denetleyicisinde çağrılacak için varsayılan yöntemdir. URL kesimi üçüncü bölümü ( `id`) için rota veri. Bu öğreticide daha sonra rota verileri görürsünüz.

Gözat `http://localhost:xxxx/HelloWorld/Welcome`. `Welcome` Yöntemi çalışır ve "Bu bir Hoş Geldiniz eylem yönteminin..." dizesini döndürür. Bu URL için denetleyicisidir `HelloWorld` ve `Welcome` eylem yöntemi. Kullanmadığınız `[Parameters]` henüz URL parçası.

![Bu bir uygulama yanıtı gösteren tarayıcı penceresi olan Hoş Geldiniz eylem yöntemi](~/tutorials/first-mvc-app/adding-controller/_static/welcome.png)

Bazı parametre bilgileri URL'den denetleyiciye geçirmek için kodu değiştirin. Örneğin, `/HelloWorld/Welcome?name=Rick&numtimes=4`. Değişiklik `Welcome` aşağıdaki kodda gösterildiği gibi iki parametre eklemek için yöntemi. 

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_2)]

Yukarıdaki kod:

* C# isteğe bağlı parametre özelliği belirtmek için kullanılan `numTimes` parametre varsayılan olarak 1 için bu parametre için değer iletilmezse.
* Kullanan`HtmlEncoder.Default.Encode` kötü amaçlı giriş (yani, JavaScript) uygulama korumak için. 
* Kullanan [Ara değerli dizeler](/dotnet/articles/csharp/language-reference/keywords/interpolated-strings).

Uygulamanızı çalıştırın ve göz atın:

   `http://localhost:xxxx/HelloWorld/Welcome?name=Rick&numtimes=4`

(Xxxx, bağlantı noktası numarasıyla değiştirin.) İçin farklı değerler deneyebilirsiniz `name` ve `numtimes` URL. MVC [model bağlama](xref:mvc/models/model-binding) sistem yönteminizi parametrelerinde adres çubuğundaki Sorgu dizesinden adlandırılmış parametreleri otomatik olarak eşlenir. Bkz: [Model bağlama](xref:mvc/models/model-binding) daha fazla bilgi için.

![Tarayıcı penceresinde Hello Rick bir uygulama yanıtı NumTimes gösteren: 4](~/tutorials/first-mvc-app/adding-controller/_static/rick4.png)

URL kesimi yukarıdaki görüntüde (`Parameters`) kullanılmaz, `name` ve `numTimes` parametreleri olarak geçirilir [sorgu dizeleri](https://wikipedia.org/wiki/Query_string). `?` (Soru işareti) ayırıcı yukarıdaki URL'ye olduğu ve sorgu dizelerini izleyin. `&` Karakter sorgu dizeleri ayırır.

Değiştirin `Welcome` yöntemini aşağıdaki kod ile:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_3)]

Uygulamayı çalıştırın ve aşağıdaki URL'yi girin:  `http://localhost:xxx/HelloWorld/Welcome/3?name=Rick`

![Tarayıcı penceresinde Hello Rick, kimliği bir uygulama yanıtı gösteren: 3](~/tutorials/first-mvc-app/adding-controller/_static/rick_routedata.png)

Bu süre üçüncü URL kesimini eşleşen bir rota parametresini `id`. `Welcome` Yöntemi içeren bir parametre `id` URL şablonda eşleşen `MapRoute` yöntemi. Sondaki `?` (içinde `id?`) gösteren `id` parametresi isteğe bağlıdır.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=snippet_1&highlight=5)]

Bu örneklerde denetleyici MVC "VC" kısmını yapmak - diğer bir deyişle, denetleyici ve görünüm iş. Denetleyici HTML doğrudan döndürüyor. Genellikle, kod ve sürdürmek için çok kullanışsız olur beri HTML doğrudan döndürerek denetleyicileri istemezsiniz. Bunun yerine, genellikle ayrı bir Razor görünüm şablon dosyası HTML yanıtı oluşturmak için kullanın. Sonraki öğreticide bunu.
