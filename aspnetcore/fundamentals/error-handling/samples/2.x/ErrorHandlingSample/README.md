---
ms.openlocfilehash: b1f7c306533e70f84e73a020c74756b91cae7ea7
ms.sourcegitcommit: 191d21c1e37b56f0df0187e795d9a56388bbf4c7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57665740"
---
# <a name="error-handling-sample-application"></a>Hata işleme örnek uygulaması

Bu örnek uygulama açıklanan senaryoları gösterir [ASP.NET Core hatalarını işlemek](https://docs.microsoft.com/aspnet/core/fundamentals/error-handling) konu.

## <a name="configure-a-custom-exception-handling-page"></a>Sayfa işleme özel bir özel durum yapılandırın

Uygulama özel durum işleme ara yazılım geliştirme ortamında çalıştırırken değil:

* Özel durumu yakalar.
* Özel durumları günlüğe kaydeder.
* İstekte sağlanan yolda başka bir işlem hattı yeniden yürütür.

## <a name="configure-custom-exception-handling-code"></a>Özel durum işleme kodunu yapılandırın

Hatalar için bir uç nokta hizmet veren bir özel durum işleme sayfayla alternatif lambda sağlamaktır `UseExceptionHandler`. Bir lambda ile kullanarak `UseExceptionHandler` yanıt döndürmeden önce hata erişim sağlar.

Özel durum işleme kodunu, örnek uygulamayı gösterir `Startup.Configure`. Üst kısmındaki yönergeleri *Startup.cs* dosyası (`LambdaErrorHandler`). Uygulama başlatıldıktan sonra bir özel durum ile tetikleme **özel durum Throw** bağlantısı dizin sayfası.
