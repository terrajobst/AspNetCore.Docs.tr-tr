# <a name="custom-webhost-service-sample"></a>Özel bir Web barındırma hizmeti örneği

Bu örnek bir ASP.NET Core uygulaması IIS kullanmadan bir Windows hizmeti olarak barındırmak nasıl gösterir. Bu örnek, açıklanan senaryoyu gösterir [ASP.NET Core uygulaması bir Windows hizmetinde barındırma](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).

## <a name="instructions"></a>Yönergeler

Yönergelere göre değiştiren bir Razor sayfaları web uygulaması örnek uygulamadır [ASP.NET Core uygulaması bir Windows hizmetinde barındırma](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).

Uygulamayı bir hizmet olarak çalıştırmak için aşağıdaki adımları gerçekleştirin:

1. Bir klasör oluşturma *c:\svc*.

1. Uygulamayı içeren klasöre yayımlama `dotnet publish --configuration Release --output c:\\svc`. Uygulamanın varlıklarına komutu taşır *svc* gerekli dahil olmak üzere, klasör `appsettings.json` dosya ve `wwwroot` klasör.

1. Açık bir **yönetici** komut istemi.

1. Aşağıdaki komutları yürütün:

   ```console
   sc create MyService binPath= "c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

  *Eşittir işareti ve yol dizesini başlangıcı arasındaki boşluk gereklidir.*

1. Bir tarayıcıda gidin `http://localhost:5000` ve hizmetin çalıştığını doğrulayın. Uygulama güvenli uç noktaya yönlendiren `https://localhost:5001`.

1. Hizmeti durdurmak için komutu kullanın:

   ```console
   sc stop MyService
   ```

Uygulamanın beklendiği gibi başlatılmazsa, hata iletileri erişilebilir hale getirmek için hızlı bir şekilde bir oturum açma sağlayıcısı gibi eklemektir [Windows olay günlüğü sağlayıcısı](https://docs.microsoft.com/aspnet/core/fundamentals/logging/index#eventlog). Sistemde Olay Görüntüleyicisi'ni kullanarak uygulama olay günlüğünü denetleyerek başka bir seçenektir. Örneğin, bir FileNotFound hatası uygulama olay günlüğü'ndeki için işlenmeyen bir özel durum aşağıdadır:

```console
Application: AspNetCoreService.exe
Framework Version: v4.0.30319
Description: The process was terminated due to an unhandled exception.
Exception Info: System.IO.FileNotFoundException
   at Microsoft.Extensions.Configuration.FileConfigurationProvider.Load(Boolean)
   at Microsoft.Extensions.Configuration.ConfigurationRoot..ctor(System.Collections.Generic.IList`1<Microsoft.Extensions.Configuration.IConfigurationProvider>)
   at Microsoft.Extensions.Configuration.ConfigurationBuilder.Build()
   ...
```
