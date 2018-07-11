# <a name="custom-webhost-service-sample"></a><span data-ttu-id="57406-101">Özel bir Web barındırma hizmeti örneği</span><span class="sxs-lookup"><span data-stu-id="57406-101">Custom WebHost Service Sample</span></span>

<span data-ttu-id="57406-102">Bu örnek bir ASP.NET Core uygulaması IIS kullanmadan bir Windows hizmeti olarak barındırmak nasıl gösterir.</span><span class="sxs-lookup"><span data-stu-id="57406-102">This sample shows how to host an ASP.NET Core app as a Windows Service without using IIS.</span></span> <span data-ttu-id="57406-103">Bu örnek, açıklanan senaryoyu gösterir [ASP.NET Core uygulaması bir Windows hizmetinde barındırma](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span><span class="sxs-lookup"><span data-stu-id="57406-103">This sample demonstrates the scenario described in [Host an ASP.NET Core app in a Windows Service](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span></span>

## <a name="instructions"></a><span data-ttu-id="57406-104">Yönergeler</span><span class="sxs-lookup"><span data-stu-id="57406-104">Instructions</span></span>

<span data-ttu-id="57406-105">Yönergelere göre değiştiren bir Razor sayfaları web uygulaması örnek uygulamadır [ASP.NET Core uygulaması bir Windows hizmetinde barındırma](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span><span class="sxs-lookup"><span data-stu-id="57406-105">The sample app is a Razor Pages web app modified according to the instructions in [Host an ASP.NET Core app in a Windows Service](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span></span>

<span data-ttu-id="57406-106">Uygulamayı bir hizmet olarak çalıştırmak için aşağıdaki adımları gerçekleştirin:</span><span class="sxs-lookup"><span data-stu-id="57406-106">To run the app in a service, perform the following steps:</span></span>

1. <span data-ttu-id="57406-107">Bir klasör oluşturma *c:\svc*.</span><span class="sxs-lookup"><span data-stu-id="57406-107">Create a folder at *c:\svc*.</span></span>

1. <span data-ttu-id="57406-108">Uygulamayı içeren klasöre yayımlama `dotnet publish --configuration Release --output c:\\svc`.</span><span class="sxs-lookup"><span data-stu-id="57406-108">Publish the app to the folder with `dotnet publish --configuration Release --output c:\\svc`.</span></span> <span data-ttu-id="57406-109">Uygulamanın varlıklarına komutu taşır *svc* gerekli dahil olmak üzere, klasör `appsettings.json` dosya ve `wwwroot` klasör.</span><span class="sxs-lookup"><span data-stu-id="57406-109">The command moves the app's assets to the *svc* folder, including the required `appsettings.json` file and the `wwwroot` folder.</span></span>

1. <span data-ttu-id="57406-110">Açık bir **yönetici** komut istemi.</span><span class="sxs-lookup"><span data-stu-id="57406-110">Open an **administrator** command prompt.</span></span>

1. <span data-ttu-id="57406-111">Aşağıdaki komutları yürütün:</span><span class="sxs-lookup"><span data-stu-id="57406-111">Execute the following commands:</span></span>

   ```console
   sc create MyService binPath= "c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

  <span data-ttu-id="57406-112">*Eşittir işareti ve yol dizesini başlangıcı arasındaki boşluk gereklidir.*</span><span class="sxs-lookup"><span data-stu-id="57406-112">*The space between the equal sign and the start of the path string is required.*</span></span>

1. <span data-ttu-id="57406-113">Bir tarayıcıda gidin `http://localhost:5000` ve hizmetin çalıştığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="57406-113">In a browser, navigate to `http://localhost:5000` and verify that the service is running.</span></span> <span data-ttu-id="57406-114">Uygulama güvenli uç noktaya yönlendiren `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="57406-114">The app redirects to the secure endpoint `https://localhost:5001`.</span></span>

1. <span data-ttu-id="57406-115">Hizmeti durdurmak için komutu kullanın:</span><span class="sxs-lookup"><span data-stu-id="57406-115">To stop the service, use the command:</span></span>

   ```console
   sc stop MyService
   ```

<span data-ttu-id="57406-116">Uygulamanın beklendiği gibi başlatılmazsa, hata iletileri erişilebilir hale getirmek için hızlı bir şekilde bir oturum açma sağlayıcısı gibi eklemektir [Windows olay günlüğü sağlayıcısı](https://docs.microsoft.com/aspnet/core/fundamentals/logging/index#eventlog).</span><span class="sxs-lookup"><span data-stu-id="57406-116">If the app doesn't start as expected, a quick way to make error messages accessible is to add a logging provider, such as the [Windows EventLog provider](https://docs.microsoft.com/aspnet/core/fundamentals/logging/index#eventlog).</span></span> <span data-ttu-id="57406-117">Sistemde Olay Görüntüleyicisi'ni kullanarak uygulama olay günlüğünü denetleyerek başka bir seçenektir.</span><span class="sxs-lookup"><span data-stu-id="57406-117">Another option is to check the Application Event Log using the Event Viewer on the system.</span></span> <span data-ttu-id="57406-118">Örneğin, bir FileNotFound hatası uygulama olay günlüğü'ndeki için işlenmeyen bir özel durum aşağıdadır:</span><span class="sxs-lookup"><span data-stu-id="57406-118">For example, here's an unhandled exception for a FileNotFound error in the Application Event Log:</span></span>

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
