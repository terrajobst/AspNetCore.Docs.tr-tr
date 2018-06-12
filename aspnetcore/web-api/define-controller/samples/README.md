# <a name="aspnet-core-web-api-controller-sample"></a><span data-ttu-id="1d008-101">ASP.NET Core Web API denetleyicisi örneği</span><span class="sxs-lookup"><span data-stu-id="1d008-101">ASP.NET Core Web API Controller Sample</span></span>

<span data-ttu-id="1d008-102">Bu örnek uygulama aşağıdaki projeleri oluşur:</span><span class="sxs-lookup"><span data-stu-id="1d008-102">This sample app consists of the following projects:</span></span>

- <span data-ttu-id="1d008-103">**WebApiSample.Api**: bir ASP.NET Core 2.1 proje .NET Core 2.1 hedefleme.</span><span class="sxs-lookup"><span data-stu-id="1d008-103">**WebApiSample.Api**: An ASP.NET Core 2.1 project targeting .NET Core 2.1.</span></span>
- <span data-ttu-id="1d008-104">**WebApiSample.Api.Pre21**: bir ASP.NET Core 2.0 proje .NET Core 2.0 hedefleme.</span><span class="sxs-lookup"><span data-stu-id="1d008-104">**WebApiSample.Api.Pre21**: An ASP.NET Core 2.0 project targeting .NET Core 2.0.</span></span>
- <span data-ttu-id="1d008-105">**WebApiSample.DataAccess**: 2 Web API projeleri için bir veri erişim katmanı olarak hizmet veren bir .NET standart 2.0 sınıf kitaplığı.</span><span class="sxs-lookup"><span data-stu-id="1d008-105">**WebApiSample.DataAccess**: A .NET Standard 2.0 class library serving as a data access tier for the 2 Web API projects.</span></span>

<span data-ttu-id="1d008-106">Bu örnek Web API denetleyicisi oluşturmanın varyasyonları gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="1d008-106">This sample illustrates variations of Web API controller creation:</span></span>

- [<span data-ttu-id="1d008-107">ControllerBase sınıf türetin</span><span class="sxs-lookup"><span data-stu-id="1d008-107">Derive class from ControllerBase</span></span>](https://docs.microsoft.com/en-us/aspnet/core/web-api/define-controller#derive-class-from-controllerbase)
- [<span data-ttu-id="1d008-108">ApiControllerAttribute sınıfıyla açıklama ekleme</span><span class="sxs-lookup"><span data-stu-id="1d008-108">Annotate class with ApiControllerAttribute</span></span>](https://docs.microsoft.com/en-us/aspnet/core/web-api/define-controller#annotate-class-with-apicontrollerattribute)