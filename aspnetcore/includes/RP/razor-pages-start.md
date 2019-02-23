Varsayılan şablon oluşturur **RazorPagesMovie**, **giriş**, **hakkında** ve **kişi** bağlantılar ve sayfalar. Tarayıcı pencerenizin boyutuna bağlı olarak, bağlantıları göstermek için Gezinti simgesi tıklamanız gerekebilir.

![Giriş ya da dizin sayfası](../../tutorials/razor-pages/razor-pages-start/_static/home2.png)

Bağlantıları test edin. **RazorPagesMovie** ve **giriş** bağlantılar dizin sayfasına gider. **Hakkında** ve **kişi** bağlantıları Git `About` ve `Contact` sayfaları, sırasıyla.

## <a name="project-files-and-folders"></a>Proje dosyaları ve klasörleri

Aşağıdaki tabloda, proje klasörleri ve dosyaları listeler. Bu öğretici için *Startup.cs* dosyasıdır anlamak en önemli. Aşağıda sağlanan her bir bağlantısını incelemeniz gerek yoktur. Bir dosya veya klasör proje hakkında daha fazla bilgi gerektiğinde bağlantılar bir başvuru olarak sağlanır.

| Dosya veya klasör              | Amaç |
| ----------------- | ------------ |
| wwwroot | Statik dosyaları içerir. Bkz: [statik dosyalar](xref:fundamentals/static-files). |
| Sayfaları | Klasör [Razor sayfaları](xref:razor-pages/index). |
| *appsettings.json* | [Yapılandırma](xref:fundamentals/configuration/index) |
| *Program.cs* | [Konaklar](xref:fundamentals/index#host) ASP.NET Core uygulaması.|
| *Startup.cs* | Hizmetler ve istek ardışık düzenini yapılandırır. Bkz: [başlangıç](xref:fundamentals/startup).|

### <a name="the-pages-folder"></a>Sayfalar klasöründe

*_Layout.cshtml* dosya ortak HTML öğeleri (betikleri ve stil sayfalarını) içerir ve uygulama düzenini ayarlar. Örneğin, tıkladığınızda **RazorPagesMovie**, **giriş**, **hakkında** veya **kişi**, aynı öğelere bakın. Ortak öğeler, üst ve alt pencerenin üst gezinti menüsünde içerir. Bkz: [Düzen](xref:mvc/views/layout) daha fazla bilgi için.

*_Viewımports.cshtml* dosyası her bir Razor sayfası alınan Razor yönergeleri içerir. Bkz: [paylaşılan yönergeleri alma](xref:mvc/views/layout#importing-shared-directives) daha fazla bilgi için.

*_ViewStart.cshtml* Razor sayfaları ayarlar `Layout` kullanılacak özellik *_Layout.cshtml* dosya. Bkz: [Düzen](xref:mvc/views/layout) daha fazla bilgi için.

*_ValidationScriptsPartial.cshtml* dosyası bir başvuru sağlar [jQuery](https://jquery.com/) doğrulama komut. Biz eklediğinizde `Create` ve `Edit` öğreticinin ilerleyen bölümlerinde sayfaları *_ValidationScriptsPartial.cshtml* dosya kullanılır.

`About`, `Contact` Ve `Index` sayfalarıdır temel sayfaları bir uygulamayı başlatmak için kullanabilirsiniz. `Error` Sayfası, hata bilgilerini görüntülemek için kullanılır. `Privacy` Sayfası, sitenizin gizlilik ilkesiyle ilgili ayrıntıları belirtmenize olanak sağlar.
