Varsayılan şablon oluşturur **RazorPagesMovie**, **giriş**, **hakkında** ve **kişi** bağlantılar ve sayfalar. Tarayıcı pencerenizin boyutuna bağlı olarak, bağlantıları göstermek için Gezinti simgesi tıklamanız gerekebilir.

![Giriş ya da dizin sayfası](~/tutorials/razor-pages/razor-pages-start/_static/home2.png)

Bağlantıları test edin. **RazorPagesMovie** ve **giriş** bağlantılar dizin sayfasına gider. **Hakkında** ve **kişi** bağlantıları Git `About` ve `Contact` sayfaları, sırasıyla.

## <a name="project-files-and-folders"></a>Proje dosyaları ve klasörleri

Aşağıdaki tabloda, proje klasörleri ve dosyaları listeler. Bu öğretici için *Startup.cs* dosyasıdır anlamak en önemli. Aşağıda sağlanan her bir bağlantısını incelemeniz gerek yoktur. Bir dosya veya klasör proje hakkında daha fazla bilgi gerektiğinde bağlantılar bir başvuru olarak sağlanır.

| Dosya veya klasör | Amaç |
| -------------- | ------- |
| *wwwroot* | Statik varlıkları içerir. Bkz: [statik dosyalar](xref:fundamentals/static-files). |
| *Sayfalar* | Klasör [Razor sayfaları](xref:razor-pages/index). |
| *appsettings.json* | [Yapılandırma](xref:fundamentals/configuration/index) |
| *Program.cs* | Yapılandırır [konak](xref:fundamentals/index#host) ASP.NET Core uygulaması. |
| *Startup.cs* | Hizmetler ve istek ardışık düzenini yapılandırır. Bkz: [başlangıç](xref:fundamentals/startup). |

### <a name="the-pagesshared-folder"></a>Sayfa/paylaşılan klasör

*_Layout.cshtml* dosya ortak HTML öğeleri (komut dosyası ve stil sayfası bağlantılar) içerir ve uygulama düzenini ayarlar. Örneğin seçtiğinizde **RazorPagesMovie**, **giriş**, **hakkında** veya **kişi**, öğeler ortak bir dizi Web sayfasında görünür. Sık kullanılan öğeleri üstte ve pencerenin alt kısmındaki üst gezinti menüsünde içerir. Daha fazla bilgi için [Düzen](xref:mvc/views/layout).

*_ValidationScriptsPartial.cshtml* dosyası bir başvuru sağlar [jQuery](https://jquery.com/) doğrulama komut. Zaman `Create` ve `Edit` sayfaları öğreticide daha sonra eklenen *_ValidationScriptsPartial.cshtml* dosyası kullanılır.

*_CookieConsentPartial.cshtml* dosyası bir gezinti çubuğu sağlar ve içerik özetlemek gizlilik ve tanımlama bilgisi İlkesi'ni kullanın. Projeye dahil GDPR varlıkları hakkında daha fazla bilgi için bkz. [ASP.NET Core AB genel veri koruma yönetmeliği (GDPR) desteği)](xref:security/gdpr).

### <a name="the-pages-folder"></a>Sayfalar klasöründe

*_ViewStart.cshtml* Razor sayfaları ayarlar `Layout` kullanılacak özellik *_Layout.cshtml* dosya. Bkz: [Düzen](xref:mvc/views/layout) daha fazla bilgi için.

*_Viewımports.cshtml* dosyası her bir Razor sayfası alınan Razor yönergeleri içerir. Bkz: [paylaşılan yönergeleri alma](xref:mvc/views/layout#importing-shared-directives) daha fazla bilgi için.

`About`, `Contact` Ve `Index` sayfalarıdır temel sayfaları bir uygulamayı başlatmak için kullanabilirsiniz. `Error` Sayfası, hata bilgilerini görüntülemek için kullanılır.
