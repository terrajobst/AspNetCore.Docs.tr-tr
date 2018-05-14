## <a name="call-the-web-api-with-jquery"></a>JQuery ile Web API çağrısı

Bu bölümde, Web API'sini çağırmak için jQuery kullanan bir HTML sayfası eklenir. jQuery isteği başlatır ve sayfa API'nin yanıt ayrıntıları ile güncelleştirir.

Projenin statik dosyaları işleme ve varsayılan dosya eşlemesini etkinleştirmek üzere yapılandırın. Bu çağırarak gerçekleştirilir [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) ve [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) uzantı yöntemleri *Startup.Configure*. Daha fazla bilgi için bkz: [statik dosyaları](xref:fundamentals/static-files).

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Startup2.cs?name=snippet_Configure&highlight=3-4)]

Adlı bir HTML dosya ekleme *index.html*, projenin *wwwroot* dizin. İçeriği aşağıdaki biçimlendirme ile değiştirin:

[!code-html[](../../tutorials/first-web-api/samples/2.0/TodoApi/wwwroot/index.html)]

Adlı bir JavaScript dosyası ekleme *site.js*, projenin *wwwroot* dizin. İçeriğini aşağıdaki kodla değiştirin:

[!code-javascript[](../../tutorials/first-web-api/samples/2.0/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

ASP.NET Core projenin başlatma ayarlarında bir değişiklik, HTML sayfası yerel olarak test etmek için gerekli. Açık *launchSettings.json* içinde *özellikleri* projenin dizin. Kaldırma `launchUrl` adresindeki açmak için uygulama zorlamak için özellik *index.html*&mdash;projenin varsayılan dosya.

JQuery almak için birkaç yolu vardır. Önceki parçacığında, bir CDN kitaplığı yüklenir. Bu örnek, jQuery ile API'yi çağıran bir tam CRUD örnektir. Bu örnek daha zengin bir deneyime için ek özellikler vardır. API çağrıları geçici açıklamalar aşağıda verilmiştir.

### <a name="get-a-list-of-to-do-items"></a>Yapılacaklar öğelerini bir listesini alma

Yapılacaklar öğelerini bir listesini almak için bir HTTP GET isteği Gönder */api/todo*.

JQuery [ajax](https://api.jquery.com/jquery.ajax/) işlevi bir nesne ya da dizi temsil eden JSON döndürür API AJAX isteği gönderir. Bu işlev bir HTTP isteğinin belirtilen gönderirken HTTP etkileşiminin tüm formları işleyebilir `url`. `GET` olarak kullanılan `type`. `success` Geri çağırma işlevi istek başarılı olursa çağrılır. Geri arama, DOM Yapılacaklar bilgilerle güncelleştirilir.

[!code-javascript[](../../tutorials/first-web-api/samples/2.0/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a>Bir Yapılacaklar öğesi ekleyin

Yapılacaklar öğesi eklemek için bir HTTP POST isteği Gönder */api/todo/*. İstek gövdesi bir Yapılacaklar nesnesi içermelidir. [Ajax](https://api.jquery.com/jquery.ajax/) işlevi kullanarak `POST` API'yi çağırmak için. İçin `POST` ve `PUT` istekleri, istek gövdesini API için gönderilen verileri temsil eder. API JSON istek gövdesini bekliyor. `accepts` Ve `contentType` seçeneklerini ayarlamak `application/json` sınıflandırmak için medya alındı ve sırasıyla gönderilen türü. Kullanarak bir JSON nesnesi dönüştürülen veriler [ `JSON.stringify` ](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify). API başarılı durum kodu döndürdüğünde `getData` işlevi HTML tablosu güncelleştirmek için çağrılır.

[!code-javascript[](../../tutorials/first-web-api/samples/2.0/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a>Güncelleştirme Yapılacaklar öğesi

Yapılacaklar öğesi güncelleştirme, her ikisi de bir istek gövdesi kullanır bu yana bir ekleme ile çok benzer. İkisi arasındaki yalnızca gerçek fark bu durumda olan `url` öğesinin benzersiz tanıtıcısı eklemek üzere değişiklikler ve `type` olan `PUT`.

[!code-javascript[](../../tutorials/first-web-api/samples/2.0/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a>Yapılacaklar öğesi silme

Yapılacaklar öğesi silme gerçekleştirilir ayarlayarak `type` AJAX çağrısı üzerinde `DELETE` ve belirttiğinizden öğeyi benzersiz tanımlayıcı URL'si.

[!code-javascript[](../../tutorials/first-web-api/samples/2.0/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]
