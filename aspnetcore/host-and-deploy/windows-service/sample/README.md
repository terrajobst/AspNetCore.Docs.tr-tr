# <a name="custom-webhost-service-sample"></a>Özel WebHost hizmet örneği

Bu örnek, bir ASP.NET Core uygulama IIS kullanmadan bir Windows hizmeti olarak barındırmak gösterilmiştir. Bu örnek açıklanan senaryoyu göstermektedir [bir Windows hizmetinde bir ASP.NET Core uygulama ana bilgisayar](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).

## <a name="instructions"></a>Yönergeler

Örnek uygulaması yönergelerine göre değiştiren bir Razor sayfalarının web uygulaması olduğundan [bir Windows hizmetinde bir ASP.NET Core uygulama ana bilgisayar](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).

Uygulamayı bir hizmet olarak çalıştırmak için aşağıdaki adımları gerçekleştirin:

1. Konumunda bir klasör oluşturun *c:\svc*.

1. Uygulamayı içeren klasöre yayımlama `dotnet publish --configuration Release --output c:\\svc`. Uygulamanın varlıklarına komutu taşır *svc* gerekli dahil olmak üzere, klasör `appsettings.json` dosya ve `wwwroot` klasör.

1. Açık bir **yönetici** komut istemi.

1. Aşağıdaki komutları çalıştırın:

   ```console
   sc create MyService binPath= "c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

  *Eşittir işareti ve yol dizesi başlangıcı arasındaki boşluğu gereklidir.*

1. Bir tarayıcıda gidin `http://localhost:5000` ve hizmetin çalıştığını doğrulayın. Uygulama güvenli uç noktasına yönlendirir `https://localhost:5001`.

1. Hizmeti durdurmak için komutu kullanın:

   ```console
   sc stop MyService
   ```

Uygulamanın beklendiği gibi başlamazsa, hata iletileri erişilebilir hale getirmek için hızlı bir şekilde bir oturum açma sağlayıcısı gibi eklemektir [Windows olay günlüğü sağlayıcısı](https://docs.microsoft.com/aspnet/core/fundamentals/logging/index#eventlog). Sistemde Olay Görüntüleyicisi'ni kullanarak uygulama olay günlüğünü denetleyin başka bir seçenektir. Örneğin, işlenmeyen bir özel durum FileNotFound hata uygulama olay günlüğü'ndeki şöyledir:

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
