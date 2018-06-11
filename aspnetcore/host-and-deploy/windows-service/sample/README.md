# <a name="custom-webhost-service-sample"></a><span data-ttu-id="42354-101">Özel WebHost hizmet örneği</span><span class="sxs-lookup"><span data-stu-id="42354-101">Custom WebHost Service Sample</span></span>

<span data-ttu-id="42354-102">Bu örnek, bir ASP.NET Core uygulama IIS kullanmadan bir Windows hizmeti olarak barındırmak gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="42354-102">This sample shows how to host an ASP.NET Core app as a Windows Service without using IIS.</span></span> <span data-ttu-id="42354-103">Bu örnek açıklanan senaryoyu göstermektedir [bir Windows hizmetinde bir ASP.NET Core uygulama ana bilgisayar](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span><span class="sxs-lookup"><span data-stu-id="42354-103">This sample demonstrates the scenario described in [Host an ASP.NET Core app in a Windows Service](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span></span>

## <a name="instructions"></a><span data-ttu-id="42354-104">Yönergeler</span><span class="sxs-lookup"><span data-stu-id="42354-104">Instructions</span></span>

<span data-ttu-id="42354-105">Örnek uygulaması yönergelerine göre değiştiren bir Razor sayfalarının web uygulaması olduğundan [bir Windows hizmetinde bir ASP.NET Core uygulama ana bilgisayar](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span><span class="sxs-lookup"><span data-stu-id="42354-105">The sample app is a Razor Pages web app modified according to the instructions in [Host an ASP.NET Core app in a Windows Service](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span></span>

<span data-ttu-id="42354-106">Uygulamayı bir hizmet olarak çalıştırmak için aşağıdaki adımları gerçekleştirin:</span><span class="sxs-lookup"><span data-stu-id="42354-106">To run the app in a service, perform the following steps:</span></span>

1. <span data-ttu-id="42354-107">Konumunda bir klasör oluşturun *c:\svc*.</span><span class="sxs-lookup"><span data-stu-id="42354-107">Create a folder at *c:\svc*.</span></span>

1. <span data-ttu-id="42354-108">Uygulamayı içeren klasöre yayımlama `dotnet publish --configuration Release --output c:\\svc`.</span><span class="sxs-lookup"><span data-stu-id="42354-108">Publish the app to the folder with `dotnet publish --configuration Release --output c:\\svc`.</span></span> <span data-ttu-id="42354-109">Uygulamanın varlıklarına komutu taşır *svc* gerekli dahil olmak üzere, klasör `appsettings.json` dosya ve `wwwroot` klasör.</span><span class="sxs-lookup"><span data-stu-id="42354-109">The command moves the app's assets to the *svc* folder, including the required `appsettings.json` file and the `wwwroot` folder.</span></span>

1. <span data-ttu-id="42354-110">Açık bir **yönetici** komut istemi.</span><span class="sxs-lookup"><span data-stu-id="42354-110">Open an **administrator** command prompt.</span></span>

1. <span data-ttu-id="42354-111">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="42354-111">Execute the following commands:</span></span>

   ```console
   sc create MyService binPath= "c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

  <span data-ttu-id="42354-112">*Eşittir işareti ve yol dizesi başlangıcı arasındaki boşluğu gereklidir.*</span><span class="sxs-lookup"><span data-stu-id="42354-112">*The space between the equal sign and the start of the path string is required.*</span></span>

1. <span data-ttu-id="42354-113">Bir tarayıcıda gidin `http://localhost:5000` ve hizmetin çalıştığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="42354-113">In a browser, navigate to `http://localhost:5000` and verify that the service is running.</span></span> <span data-ttu-id="42354-114">Uygulama güvenli uç noktasına yönlendirir `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="42354-114">The app redirects to the secure endpoint `https://localhost:5001`.</span></span>

1. <span data-ttu-id="42354-115">Hizmeti durdurmak için komutu kullanın:</span><span class="sxs-lookup"><span data-stu-id="42354-115">To stop the service, use the command:</span></span>

   ```console
   sc stop MyService
   ```

<span data-ttu-id="42354-116">Uygulamanın beklendiği gibi başlamazsa, hata iletileri erişilebilir hale getirmek için hızlı bir şekilde bir oturum açma sağlayıcısı gibi eklemektir [Windows olay günlüğü sağlayıcısı](https://docs.microsoft.com/aspnet/core/fundamentals/logging/index#eventlog).</span><span class="sxs-lookup"><span data-stu-id="42354-116">If the app doesn't start as expected, a quick way to make error messages accessible is to add a logging provider, such as the [Windows EventLog provider](https://docs.microsoft.com/aspnet/core/fundamentals/logging/index#eventlog).</span></span> <span data-ttu-id="42354-117">Sistemde Olay Görüntüleyicisi'ni kullanarak uygulama olay günlüğünü denetleyin başka bir seçenektir.</span><span class="sxs-lookup"><span data-stu-id="42354-117">Another option is to check the Application Event Log using the Event Viewer on the system.</span></span> <span data-ttu-id="42354-118">Örneğin, işlenmeyen bir özel durum FileNotFound hata uygulama olay günlüğü'ndeki şöyledir:</span><span class="sxs-lookup"><span data-stu-id="42354-118">For example, here's an unhandled exception for a FileNotFound error in the Application Event Log:</span></span>

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
