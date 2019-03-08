---
ms.openlocfilehash: b1f7c306533e70f84e73a020c74756b91cae7ea7
ms.sourcegitcommit: 191d21c1e37b56f0df0187e795d9a56388bbf4c7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57665740"
---
# <a name="error-handling-sample-application"></a><span data-ttu-id="aef22-101">Hata işleme örnek uygulaması</span><span class="sxs-lookup"><span data-stu-id="aef22-101">Error Handling Sample Application</span></span>

<span data-ttu-id="aef22-102">Bu örnek uygulama açıklanan senaryoları gösterir [ASP.NET Core hatalarını işlemek](https://docs.microsoft.com/aspnet/core/fundamentals/error-handling) konu.</span><span class="sxs-lookup"><span data-stu-id="aef22-102">This sample app demonstrates the scenarios described in the [Handle errors in ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/error-handling) topic.</span></span>

## <a name="configure-a-custom-exception-handling-page"></a><span data-ttu-id="aef22-103">Sayfa işleme özel bir özel durum yapılandırın</span><span class="sxs-lookup"><span data-stu-id="aef22-103">Configure a custom exception handling page</span></span>

<span data-ttu-id="aef22-104">Uygulama özel durum işleme ara yazılım geliştirme ortamında çalıştırırken değil:</span><span class="sxs-lookup"><span data-stu-id="aef22-104">When the app isn't running in the Development environment, Exception Handling Middleware:</span></span>

* <span data-ttu-id="aef22-105">Özel durumu yakalar.</span><span class="sxs-lookup"><span data-stu-id="aef22-105">Catches exceptions.</span></span>
* <span data-ttu-id="aef22-106">Özel durumları günlüğe kaydeder.</span><span class="sxs-lookup"><span data-stu-id="aef22-106">Logs exceptions.</span></span>
* <span data-ttu-id="aef22-107">İstekte sağlanan yolda başka bir işlem hattı yeniden yürütür.</span><span class="sxs-lookup"><span data-stu-id="aef22-107">Re-executes the request in an alternate pipeline at the path provided.</span></span>

## <a name="configure-custom-exception-handling-code"></a><span data-ttu-id="aef22-108">Özel durum işleme kodunu yapılandırın</span><span class="sxs-lookup"><span data-stu-id="aef22-108">Configure custom exception handling code</span></span>

<span data-ttu-id="aef22-109">Hatalar için bir uç nokta hizmet veren bir özel durum işleme sayfayla alternatif lambda sağlamaktır `UseExceptionHandler`.</span><span class="sxs-lookup"><span data-stu-id="aef22-109">An alternative to serving an endpoint for errors with a custom exception handling page is to provide a lambda to `UseExceptionHandler`.</span></span> <span data-ttu-id="aef22-110">Bir lambda ile kullanarak `UseExceptionHandler` yanıt döndürmeden önce hata erişim sağlar.</span><span class="sxs-lookup"><span data-stu-id="aef22-110">Using a lambda with `UseExceptionHandler` allows access to the error before returning the response.</span></span>

<span data-ttu-id="aef22-111">Özel durum işleme kodunu, örnek uygulamayı gösterir `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="aef22-111">The sample app demonstrates custom exception handling code in `Startup.Configure`.</span></span> <span data-ttu-id="aef22-112">Üst kısmındaki yönergeleri *Startup.cs* dosyası (`LambdaErrorHandler`).</span><span class="sxs-lookup"><span data-stu-id="aef22-112">Follow the instructions at the top of the *Startup.cs* file (`LambdaErrorHandler`).</span></span> <span data-ttu-id="aef22-113">Uygulama başlatıldıktan sonra bir özel durum ile tetikleme **özel durum Throw** bağlantısı dizin sayfası.</span><span class="sxs-lookup"><span data-stu-id="aef22-113">After the app starts, trigger an exception with the **Throw Exception** link on the Index page.</span></span>
