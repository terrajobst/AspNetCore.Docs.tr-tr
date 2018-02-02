# <a name="custom-webhost-service-sample"></a><span data-ttu-id="5cca1-101">Özel WebHost hizmet örneği</span><span class="sxs-lookup"><span data-stu-id="5cca1-101">Custom WebHost Service Sample</span></span>

<span data-ttu-id="5cca1-102">Bu örnek bir Windows hizmeti olarak IIS kullanmadan Windows üzerinde bir ASP.NET Core uygulama barındırmak için önerilen yol göstermektedir.</span><span class="sxs-lookup"><span data-stu-id="5cca1-102">This sample shows the recommended way to host an ASP.NET Core app on Windows without using IIS as a Windows Service.</span></span> <span data-ttu-id="5cca1-103">Bu örnek açıklanan özelliklerini gösterir [bir Windows hizmetinde bir ASP.NET Core uygulama ana bilgisayar](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span><span class="sxs-lookup"><span data-stu-id="5cca1-103">This sample demonstrates the features described in [Host an ASP.NET Core app in a Windows Service](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span></span>

## <a name="instructions"></a><span data-ttu-id="5cca1-104">Yönergeler</span><span class="sxs-lookup"><span data-stu-id="5cca1-104">Instructions</span></span>

<span data-ttu-id="5cca1-105">Örnek uygulaması yönergelerine göre değiştiren basit bir MVC web uygulaması olduğundan [bir Windows hizmetinde bir ASP.NET Core uygulama ana bilgisayar](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span><span class="sxs-lookup"><span data-stu-id="5cca1-105">The sample app is a simple MVC web app modified according to the instructions in [Host an ASP.NET Core app in a Windows Service](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span></span>

<span data-ttu-id="5cca1-106">Uygulamayı bir hizmet olarak çalıştırmak için aşağıdaki adımları gerçekleştirin:</span><span class="sxs-lookup"><span data-stu-id="5cca1-106">To run the app in a service, perform the following steps:</span></span>

1. <span data-ttu-id="5cca1-107">Konumunda bir klasör oluşturun *c:\svc*.</span><span class="sxs-lookup"><span data-stu-id="5cca1-107">Create a folder at *c:\svc*.</span></span>

1. <span data-ttu-id="5cca1-108">Uygulamayı içeren klasöre yayımlama `dotnet publish --configuration Release --output c:\\svc`.</span><span class="sxs-lookup"><span data-stu-id="5cca1-108">Publish the app to the folder with `dotnet publish --configuration Release --output c:\\svc`.</span></span> <span data-ttu-id="5cca1-109">Komutu uygulamanın varlıklar gerekli dahil olmak üzere klasörüne taşımak `appsettings.json` dosya ve `wwwroot` içeriğiyle ilgili klasör.</span><span class="sxs-lookup"><span data-stu-id="5cca1-109">The command will move the app's assets to the folder, including the required `appsettings.json` file and the `wwwroot` folder with its contents.</span></span>

1. <span data-ttu-id="5cca1-110">Açık bir **yönetici** komut kabuğunda.</span><span class="sxs-lookup"><span data-stu-id="5cca1-110">Open an **administrator** command shell.</span></span>

1. <span data-ttu-id="5cca1-111">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="5cca1-111">Execute the following commands:</span></span>

   ```console
   sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

1. <span data-ttu-id="5cca1-112">Bir tarayıcıda Git `http://localhost:5000` hizmetinin çalıştığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="5cca1-112">In a browser, go to `http://localhost:5000` to verify that the service is running.</span></span>

1. <span data-ttu-id="5cca1-113">Hizmeti durdurmak için komutu kullanın:</span><span class="sxs-lookup"><span data-stu-id="5cca1-113">To stop the service, use the command:</span></span>

   ```console
   sc stop MyService
   ```

<span data-ttu-id="5cca1-114">Uygulamayı bir hizmet olarak çalıştırırken istendiği şekilde başlamazsa, hata iletileri erişilebilir hale getirmek için hızlı bir şekilde bir oturum açma sağlayıcısı gibi eklemektir [Windows olay günlüğü sağlayıcısı](https://docs.microsoft.com/aspnet/core/fundamentals/logging/index#eventlog).</span><span class="sxs-lookup"><span data-stu-id="5cca1-114">If the app doesn't start up as expected when running in a service, a quick way to make error messages accessible is to add a logging provider, such as the [Windows EventLog provider](https://docs.microsoft.com/aspnet/core/fundamentals/logging/index#eventlog).</span></span> <span data-ttu-id="5cca1-115">Sistemde Olay Görüntüleyicisi'ni kullanarak uygulama olay günlüğünü denetleyin başka bir seçenektir.</span><span class="sxs-lookup"><span data-stu-id="5cca1-115">Another option is to check the Application Event Log using the Event Viewer on the system.</span></span> <span data-ttu-id="5cca1-116">Örneğin, işlenmeyen bir özel durum FileNotFound hata uygulama olay günlüğü'ndeki şöyledir:</span><span class="sxs-lookup"><span data-stu-id="5cca1-116">For example, here's an unhandled exception for a FileNotFound error in the Application Event Log:</span></span>

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
