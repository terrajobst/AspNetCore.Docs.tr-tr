Varsayılan şablon oluşturur **RazorPagesMovie**, **giriş**, **hakkında** ve **kişi** bağlantılar ve sayfaları. Tarayıcı pencerenizin boyutuna bağlı olarak, bağlantıları göstermek için Gezinti simgesini gerekebilir.

![Giriş veya dizin sayfası](~/tutorials/razor-pages/razor-pages-start/_static/home2.png)

Bağlantıları sınayın. **RazorPagesMovie** ve **giriş** bağlantılar dizin sayfasına gider. **Hakkında** ve **kişi** bağlantılar Git `About` ve `Contact` sayfaları, sırasıyla.

## <a name="project-files-and-folders"></a>Proje dosyaları ve klasörleri

Aşağıdaki tabloda, dosya ve klasörleri projesinde listeler. Bu öğretici için *haline* dosyasıdır anlamak en önemli. Aşağıda sağlanan her bir bağlantının gözden gerek yoktur. Bir dosya veya klasör projesinde hakkında daha fazla bilgi gerektiğinde bağlantılar bir başvuru olarak sağlanır.

| Dosya veya klasör | Amaç |
| -------------- | ------- |
| *wwwroot* | Statik varlıklarını içerir. Bkz: [statik dosyaları](xref:fundamentals/static-files). |
| *Sayfalar* | Klasör için [Razor sayfalarının](xref:razor-pages/index). |
| *appsettings.json* | [Yapılandırma](xref:fundamentals/configuration/index) |
| *Program.cs* | Yapılandırır [konak](xref:fundamentals/host/index) ASP.NET Core uygulamanın. |
| *Haline* | Hizmetler ve istek ardışık düzenini yapılandırır. Bkz: [başlangıç](xref:fundamentals/startup). |

### <a name="the-pagesshared-folder"></a>Sayfa/paylaşılan klasör

*_Layout.cshtml* dosyası ortak HTML öğeleri (komut dosyalarını ve stil sayfası bağlantıları) içerir ve uygulama için bir düzen ayarlar. Örneğin seçtiğinizde, **RazorPagesMovie**, **giriş**, **hakkında** veya **kişi**, öğeleri ortak bir dizi Web sayfasında görüntülenir. Ortak öğeler en üstündeki ve üstbilgi pencerenin altındaki gezinti menüsü içerir. Daha fazla bilgi için bkz: [düzeni](xref:mvc/views/layout).

*_ValidationScriptsPartial.cshtml* dosyası için bir başvuru sağlar [jQuery](https://jquery.com/) doğrulama komut dosyaları. Zaman `Create` ve `Edit` sayfaları öğreticide daha sonra eklenen *_ValidationScriptsPartial.cshtml* dosyası kullanılır.

*_CookieConsentPartial.cshtml* dosya gezinti çubuğu sağlar ve içerik özetlemek gizlilik ve tanımlama bilgisi ilkesini kullanın. Projeye dahil GDPR varlıklar hakkında daha fazla bilgi için bkz: [ASP.NET Core AB genel veri koruma düzenleme (GDPR) desteği)](xref:security/gdpr).

### <a name="the-pages-folder"></a>Sayfaları klasörü

*_ViewStart.cshtml* Razor sayfalarının ayarlar `Layout` kullanmak için özelliği *_Layout.cshtml* dosya. Bkz: [düzeni](xref:mvc/views/layout) daha fazla bilgi için.

*_Viewımports.cshtml* dosyası her Razor sayfasına içe Razor yönergeleri içerir. Bkz: [paylaşılan yönergeleri alma](xref:mvc/views/layout#importing-shared-directives) daha fazla bilgi için.

`About`, `Contact` Ve `Index` sayfalarıdır temel sayfaların bir uygulamayı başlatmak için kullanabilirsiniz. `Error` Sayfa hata bilgilerini görüntülemek için kullanılır.
