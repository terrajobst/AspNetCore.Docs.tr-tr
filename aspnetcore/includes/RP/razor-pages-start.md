Varsayılan şablon oluşturur **RazorPagesMovie**, **giriş**, **hakkında** ve **kişi** bağlantılar ve sayfaları. Tarayıcı pencerenizin boyutuna bağlı olarak, bağlantıları göstermek için Gezinti simgesini gerekebilir.

![Giriş veya dizin sayfası](../../tutorials/razor-pages/razor-pages-start/_static/home2.png)

Bağlantıları sınayın. **RazorPagesMovie** ve **giriş** bağlantılar dizin sayfasına gider. **Hakkında** ve **kişi** bağlantılar Git `About` ve `Contact` sayfaları, sırasıyla.

## <a name="project-files-and-folders"></a>Proje dosyaları ve klasörleri

Aşağıdaki tabloda, dosya ve klasörleri projesinde listeler. Bu öğretici için *haline* dosyasıdır anlamak en önemli. Aşağıda sağlanan her bir bağlantının gözden gerek yoktur. Bir dosya veya klasör projesinde hakkında daha fazla bilgi gerektiğinde bağlantılar bir başvuru olarak sağlanır.

| Dosya veya klasör              | Amaç |
| ----------------- | ------------ | 
| wwwroot | Statik dosyaları içerir. Bkz: [statik dosyaları](xref:fundamentals/static-files). |
| Sayfaları | Klasör için [Razor sayfalarının](xref:mvc/razor-pages/index). | 
| *appsettings.json* | [Yapılandırma](xref:fundamentals/configuration/index) |
| *Program.cs* | [Ana bilgisayar](xref:fundamentals/hosting) ASP.NET Core uygulama.|
| *Haline* | Hizmetler ve istek ardışık düzenini yapılandırır. Bkz: [başlangıç](xref:fundamentals/startup).|

### <a name="the-pages-folder"></a>Sayfaları klasörü

*_Layout.cshtml* dosyası ortak HTML öğeleri (komut dosyalarını ve stil sayfalarını) içerir ve uygulama için bir düzen ayarlar. Örneğin, tıkladığınızda **RazorPagesMovie**, **giriş**, **hakkında** veya **kişi**, aynı öğelere bakın. Ortak öğeler, üst ve penceresinin alt kısmındaki başlığındaki Gezinti menüsünde içerir. Bkz: [düzeni](xref:mvc/views/layout) daha fazla bilgi için.

*_ViewStart.cshtml* Razor sayfalarının ayarlar `Layout` kullanmak için özelliği *_Layout.cshtml* dosya. Bkz: [düzeni](xref:mvc/views/layout) daha fazla bilgi için.

*_Viewımports.cshtml* dosyası her Razor sayfasına içe Razor yönergeleri içerir. Bkz: [paylaşılan yönergeleri alma](xref:mvc/views/layout#importing-shared-directives) daha fazla bilgi için.

*_ValidationScriptsPartial.cshtml* dosyası için bir başvuru sağlar [jQuery](https://jquery.com/) doğrulama komut dosyaları. Biz eklediğinizde `Create` ve `Edit` daha sonra öğreticide sayfaları *_ValidationScriptsPartial.cshtml* dosya kullanılır.

`About`, `Contact` Ve `Index` sayfalarıdır temel sayfaların bir uygulamayı başlatmak için kullanabilirsiniz. `Error` Sayfa hata bilgilerini görüntülemek için kullanılır.