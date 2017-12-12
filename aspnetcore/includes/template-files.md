* Haline: [başlangıç sınıfı](../fundamentals/startup.md) -sınıfı uygulamaya yapılan tüm istekleri işleyen istek ardışık düzenini yapılandırır.
* Program.cs: [Program sınıfı](../fundamentals/index.md) , uygulamanın ana giriş noktası içerir.
* firstapp.csproj: [proje dosyası](https://docs.microsoft.com/dotnet/articles/core/preview3/tools/csproj) ASP.NET Core uygulamaları için MSBuild proje dosyası biçimi. Proje için proje başvuruları içeren NuGet başvurularını ve diğer ilgili öğeler proje.
* appSettings.JSON / appsettings. Development.JSON: Ortam temel uygulama ayarlarını yapılandırma dosyası. [Bkz.](xref:fundamentals/configuration/index).
* bower.JSON: Bower Paket bağımlılıklarını projesi için.
* .bowerrc: Bower varlıklar indirdiğinde bileşenleri yükleneceği tanımlayan bower yapılandırma dosyası.
* bundleconfig.JSON: paketleme ve küçültme ön uç JavaScript ve CSS varlıklar için yapılandırma dosyaları.
* Görünümleri: Razor görünümleri içerir. Görünümler uygulamanın kullanıcı arabirimini (UI) görüntüleyen bileşenlerdir. Genellikle, bu UI model verileri görüntüler.
* Denetleyicileri: MVC denetleyicileri, başlangıçta içeren *HomeController.cs*. Denetleyicileri tarayıcı isteklerini işleyen sınıflarıdır.
* wwwroot: Web uygulama kök klasörü.

Daha fazla bilgi için bkz: [MVC örüntüsü](xref:mvc/overview).
