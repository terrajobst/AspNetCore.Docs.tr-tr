---
title: ASP.NET Core Web API 'Lerinde hataları işleme
author: pranavkm
description: ASP.NET Core Web API 'Leri ile hata işleme hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.1'
ms.author: prkrishn
ms.custom: mvc
ms.date: 09/25/2019
uid: web-api/handle-errors
ms.openlocfilehash: 9c5dd2f89e7351f386d1f0633c831952dc58e568
ms.sourcegitcommit: 994da92edb0abf856b1655c18880028b15a28897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2019
ms.locfileid: "71278728"
---
# <a name="handle-errors-in-aspnet-core-web-apis"></a>ASP.NET Core Web API 'Lerinde hataları işleme

Bu makalede, ASP.NET Core Web API 'Leriyle hata işlemenin nasıl işleneceği ve özelleştirileceği açıklanır.

[Örnek kodu görüntüle veya indir](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/handle-errors/samples) ([İndirme](xref:index#how-to-download-a-sample))

## <a name="developer-exception-page"></a>Geliştirici özel durum sayfası

[Geliştirici özel durum sayfası](xref:fundamentals/error-handling) , sunucu hataları için ayrıntılı yığın izlemeleri almak için kullanışlı bir araçtır.

İstemci HTML biçimli çıktıyı kabul etmezse, geliştirici özel durum sayfasında düz metin yanıtı görüntülenir. Örneğin:

```
> curl https://localhost:5001/weatherforecast
System.ArgumentException: count
   at errorhandling.Controllers.WeatherForecastController.Get(Int32 x) in D:\work\Samples\samples\aspnetcore\mvc\errorhandling\Controllers\WeatherForecastController.cs:line 35
   at lambda_method(Closure , Object , Object[] )
   at Microsoft.Extensions.Internal.ObjectMethodExecutor.Execute(Object target, Object[] parameters)
...
```

> [!WARNING]
> Geliştirici özel durum sayfasını **yalnızca uygulama geliştirme ortamında çalışırken**etkinleştirin. Uygulama üretimde çalıştırıldığında ayrıntılı özel durum bilgilerini herkese açık bir şekilde paylaşmak istemezsiniz. Ortamları yapılandırma hakkında daha fazla bilgi için bkz <xref:fundamentals/environments>.

## <a name="exception-handler"></a>Özel durum işleyicisi

Geliştirme dışı ortamlarda, [özel durum Işleme ara yazılımı](xref:fundamentals/error-handling) bir hata yükü oluşturmak için kullanılabilir:

1. ' `Startup.Configure`De, <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> ara yazılımı kullanmak için çağırın:

    ::: moniker range=">= aspnetcore-3.0"

    [!code-csharp[](handle-errors/samples/3.x/Startup.cs?name=snippet_UseExceptionHandler&highlight=9)]

    ::: moniker-end

    ::: moniker range="<= aspnetcore-2.2"

    [!code-csharp[](handle-errors/samples/2.x/2.2/Startup.cs?name=snippet_UseExceptionHandler&highlight=9)]

    ::: moniker-end

1. `/error` Yola yanıt vermek için bir denetleyici eylemi yapılandırın:

    ::: moniker range=">= aspnetcore-3.0"

    [!code-csharp[](handle-errors/samples/3.x/Controllers/ErrorController.cs?name=snippet_ErrorController)]

    ::: moniker-end

    ::: moniker range="<= aspnetcore-2.2"

    [!code-csharp[](handle-errors/samples/2.x/2.2/Controllers/ErrorController.cs?name=snippet_ErrorController)]

    ::: moniker-end

Önceki `Error` eylem istemciye [RFC7807](https://tools.ietf.org/html/rfc7807)uyumlu bir yük gönderir.

Özel durum Işleme ara yazılımı, yerel geliştirme ortamında daha ayrıntılı içerik üzerinde anlaşılan çıkış de sağlayabilir. Geliştirme ve üretim ortamları genelinde tutarlı bir yük biçimi oluşturmak için aşağıdaki adımları kullanın:

1. ' `Startup.Configure`De, ortama özgü özel durum işleme ara yazılım örneklerini kaydedin:

    ::: moniker range=">= aspnetcore-3.0"

    ```csharp
    public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
    {
        if (env.IsDevelopment())
        {
            app.UseExceptionHandler("/error-local-development");
        }
        else
        {
            app.UseExceptionHandler("/error");
        }
    }
    ```

    ::: moniker-end

    ::: moniker range="<= aspnetcore-2.2"

    ```csharp
    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        if (env.IsDevelopment())
        {
            app.UseExceptionHandler("/error-local-development");
        }
        else
        {
            app.UseExceptionHandler("/error");
        }
    }
    ```

    ::: moniker-end

    Yukarıdaki kodda, ara yazılım ile kaydedilir:

    * Geliştirme ortamında bir `/error-local-development` yol.
    * Geliştirme olmayan ortamlarda `/error` bir yolu.
    
1. Denetleyici eylemlerine öznitelik yönlendirmeyi Uygula:

    ::: moniker range=">= aspnetcore-3.0"

    [!code-csharp[](handle-errors/samples/3.x/Controllers/ErrorController.cs?name=snippet_ErrorControllerEnvironmentSpecific)]

    ::: moniker-end

    ::: moniker range="<= aspnetcore-2.2"

    [!code-csharp[](handle-errors/samples/2.x/2.2/Controllers/ErrorController.cs?name=snippet_ErrorControllerEnvironmentSpecific)]

    ::: moniker-end

## <a name="use-exceptions-to-modify-the-response"></a>Yanıtı değiştirmek için özel durumları kullanın

Yanıtın içeriği, denetleyicinin dışından değiştirilebilir. ASP.NET 4. x Web API 'sinde, bunu yapmanın bir yolu <xref:System.Web.Http.HttpResponseException> türünü kullanmaktır. ASP.NET Core eşdeğer bir tür içermez. İçin `HttpResponseException` destek aşağıdaki adımlarla eklenebilir:

1. Adında `HttpResponseException`iyi bilinen bir özel durum türü oluşturun:

    [!code-csharp[](handle-errors/samples/3.x/Exceptions/HttpResponseException.cs?name=snippet_HttpResponseException)]

1. Adlı `HttpResponseExceptionFilter`bir eylem filtresi oluşturun:

    [!code-csharp[](handle-errors/samples/3.x/Filters/HttpResponseExceptionFilter.cs?name=snippet_HttpResponseExceptionFilter)]

1. İçinde `Startup.ConfigureServices`, filtre koleksiyonuna eylem filtresini ekleyin:

    ::: moniker range=">= aspnetcore-3.0"

    [!code-csharp[](handle-errors/samples/3.x/Startup.cs?name=snippet_AddExceptionFilter)]

    ::: moniker-end

    ::: moniker range="= aspnetcore-2.2"
    
    [!code-csharp[](handle-errors/samples/2.x/2.2/Startup.cs?name=snippet_AddExceptionFilter)]

    ::: moniker-end

    ::: moniker range="= aspnetcore-2.1"

    [!code-csharp[](handle-errors/samples/2.x/2.1/Startup.cs?name=snippet_AddExceptionFilter)]

    ::: moniker-end

## <a name="validation-failure-error-response"></a>Doğrulama hatası hata yanıtı

Web API denetleyicileri için, bir model doğrulaması başarısız <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails> olduğunda MVC bir yanıt türüyle yanıt verir. MVC, doğrulama hatasına yönelik <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory> hata yanıtını oluşturmak için sonuçlarını kullanır. Aşağıdaki örnek, varsayılan yanıt türünü <xref:Microsoft.AspNetCore.Mvc.SerializableError> içinde `Startup.ConfigureServices`olarak değiştirmek için fabrikası kullanır:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](handle-errors/samples/3.x/Startup.cs?name=snippet_DisableProblemDetailsInvalidModelStateResponseFactory&highlight=4-13)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

[!code-csharp[](handle-errors/samples/2.x/2.2/Startup.cs?name=snippet_DisableProblemDetailsInvalidModelStateResponseFactory&highlight=5-14)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-csharp[](handle-errors/samples/2.x/2.1/Startup.cs?name=snippet_DisableProblemDetailsInvalidModelStateResponseFactory&highlight=6-15)]

::: moniker-end

## <a name="client-error-response"></a>İstemci hata yanıtı

Bir *hata sonucu* , 400 veya ÜZERI bir http durum kodu ile sonuç olarak tanımlanır. Web API denetleyicileri için, MVC bir hata sonucunu ile <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>sonucuyla dönüştürür.

::: moniker range=">= aspnetcore-3.0"

Hata yanıtı aşağıdaki yollarla yapılandırılabilir:

1. [Problemayrıntılar Fabrikası Uygulama](#implement-problemdetailsfactory)
1. [ApiBehaviorOptions. ClientErrorMapping kullanın](#use-apibehavioroptionsclienterrormapping)

### <a name="implement-problemdetailsfactory"></a>Problemayrıntılar Fabrikası Uygulama

MVC, `Microsoft.AspNetCore.Mvc.ProblemDetailsFactory` ve <xref:Microsoft.AspNetCore.Mvc.ProblemDetails> öğesinin<xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>tüm örneklerini üretmek için kullanır. Buna istemci hata yanıtları, doğrulama hatası hata yanıtları ve `Microsoft.AspNetCore.Mvc.ControllerBase.Problem` ve <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ValidationProblem> yardımcı yöntemler dahildir.

Sorun ayrıntıları yanıtını özelleştirmek için, `ProblemDetailsFactory` içinde `Startup.ConfigureServices`özel bir uygulamasını kaydedin:

```csharp
public void ConfigureServices(IServiceCollection serviceCollection)
{
    services.AddControllers();
    services.AddTransient<ProblemDetailsFactory, CustomProblemDetailsFactory>();
}
```

::: moniker-end

::: moniker range="= aspnetcore-2.2"

Hata yanıtı, [Use ApiBehaviorOptions. ClientErrorMapping](#use-apibehavioroptionsclienterrormapping) bölümünde özetlenen şekilde yapılandırılabilir.

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

### <a name="use-apibehavioroptionsclienterrormapping"></a>ApiBehaviorOptions. ClientErrorMapping kullanın

Yanıtıniçeriğini`ProblemDetails` yapılandırmak için <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.ClientErrorMapping*> özelliğini kullanın. Örneğin, aşağıdaki kod `Startup.ConfigureServices` 404 yanıtlarının `type` özelliğini güncelleştirir:

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=8-9)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=9-10)]

::: moniker-end
