---
ms.openlocfilehash: 07abb12af390c0f2a50e98fc5e53545b6635f968
ms.sourcegitcommit: 191d21c1e37b56f0df0187e795d9a56388bbf4c7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57665515"
---
# <a name="aspnet-core-web-api-controller-sample"></a><span data-ttu-id="8f0c6-101">ASP.NET Core Web API denetleyicisi örneği</span><span class="sxs-lookup"><span data-stu-id="8f0c6-101">ASP.NET Core Web API Controller Sample</span></span>

<span data-ttu-id="8f0c6-102">Bu örnek uygulama aşağıdaki projeleri oluşur:</span><span class="sxs-lookup"><span data-stu-id="8f0c6-102">This sample app consists of the following projects:</span></span>

- <span data-ttu-id="8f0c6-103">**WebApiSample.Api.22**: .NET Core 2.2 hedefleyen bir ASP.NET Core 2.2 projesi.</span><span class="sxs-lookup"><span data-stu-id="8f0c6-103">**WebApiSample.Api.22**: An ASP.NET Core 2.2 project targeting .NET Core 2.2.</span></span>
- <span data-ttu-id="8f0c6-104">**WebApiSample.Api.21**: .NET Core 2.1 hedefleyen bir ASP.NET Core 2.1 projesi.</span><span class="sxs-lookup"><span data-stu-id="8f0c6-104">**WebApiSample.Api.21**: An ASP.NET Core 2.1 project targeting .NET Core 2.1.</span></span>
- <span data-ttu-id="8f0c6-105">**WebApiSample.Api.Pre21**: .NET Core 2.0 hedefleyen bir ASP.NET Core 2.0 projesi.</span><span class="sxs-lookup"><span data-stu-id="8f0c6-105">**WebApiSample.Api.Pre21**: An ASP.NET Core 2.0 project targeting .NET Core 2.0.</span></span>
- <span data-ttu-id="8f0c6-106">**WebApiSample.DataAccess**: 2 Web API projeleri için bir veri erişim katmanı olarak hizmet veren bir .NET Standard 2.0 sınıf kitaplığı.</span><span class="sxs-lookup"><span data-stu-id="8f0c6-106">**WebApiSample.DataAccess**: A .NET Standard 2.0 class library serving as a data access tier for the 2 Web API projects.</span></span>

<span data-ttu-id="8f0c6-107">Bu örnek, Web API denetleyicisi oluşturma çeşidi gösterir:</span><span class="sxs-lookup"><span data-stu-id="8f0c6-107">This sample illustrates variations of Web API controller creation:</span></span>

- [<span data-ttu-id="8f0c6-108">Sınıf ControllerBase türetin.</span><span class="sxs-lookup"><span data-stu-id="8f0c6-108">Derive class from ControllerBase</span></span>](https://docs.microsoft.com/aspnet/core/web-api#derive-class-from-controllerbase)
- [<span data-ttu-id="8f0c6-109">Sınıf ApiControllerAttribute ile Not Ekle</span><span class="sxs-lookup"><span data-stu-id="8f0c6-109">Annotate class with ApiControllerAttribute</span></span>](https://docs.microsoft.com/aspnet/core/web-api#annotate-class-with-apicontrollerattribute)
