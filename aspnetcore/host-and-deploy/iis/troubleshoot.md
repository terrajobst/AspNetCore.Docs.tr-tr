---
title: "IIS üzerinde ASP.NET Core sorun giderme"
author: guardrex
description: "ASP.NET Core uygulamaların IIS dağıtımlarını sorunları tanılamak öğrenin."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/troubleshoot
ms.openlocfilehash: c68070a9cba5667504d1ad4927b02b73f83e6573
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2018
---
# <a name="troubleshoot-aspnet-core-on-iis"></a>IIS üzerinde ASP.NET Core sorun giderme

Tarafından [Luke Latham](https://github.com/guardrex)

IIS dağıtımları ile ilgili sorunları tanılamak için:

* Tarayıcı çıktı üzerinde çalışın.
* Sistemin inceleyin **uygulama** aracılığıyla oturum **Olay Görüntüleyicisi'ni**.
* Etkinleştirme `stdout` günlüğü. **ASP.NET Core Modülü** günlük sağlanan yolunda bulunduğunda *stdoutLogFile* özniteliği `<aspNetCore>` öğesinde *web.config*. Öznitelik değerinde sağlanan yol üzerindeki herhangi bir klasörde dağıtımda mevcut olması gerekir. Ayarlama *stdoutLogEnabled* için `true`. Kullanan uygulamalar `Microsoft.NET.Sdk.Web` oluşturmak için SDK *web.config* dosya varsayılan *stdoutLogEnabled* ayarını `false`, böylece el ile sağlamak *web.config* dosya veya etkinleştirmek için dosyasını değiştirmek `stdout` günlüğü.

İle üç bu kaynaklardan bilgi kullanmak [sık karşılaşılan başvuru konu](xref:host-and-deploy/azure-iis-errors-reference) sorunu belirlemek için. Bu sorunu çözmek için sağlanan sorun giderme önerileri izleyin.

Sık karşılaşılan çeşitli tarayıcı, uygulama günlüğüne ve ASP.NET Core modülü günlük modülü kadar görünmüyor *startupTimeLimit* (varsayılan: 120 saniye) ve *startupRetryCount* (varsayılan: 2) geçti. Bu nedenle, bir tam altı önce deducing modülü uygulama için bir işlemi başlatmak için başarısız olan dakika bekleyin.

Uygulamanın düzgün çalıştığını belirlemek için bir hızlı doğrudan Kestrel üzerinde uygulamayı çalıştırmak için yoludur. Uygulama olarak yayımlandıysa bir [framework bağımlı dağıtım](/dotnet/core/deploying/#framework-dependent-deployments-fdd), yürütme `dotnet <assembly_name>.dll` dağıtım klasöründe olan uygulamanın IIS fiziksel yolu. Uygulama olarak yayımlandıysa bir [müstakil dağıtım](/dotnet/core/deploying/#self-contained-deployments-scd)çalıştırın uygulamayı doğrudan bir komut isteminden, yürütülebilir `<assembly_name>.exe`, dağıtım klasörü. Kestrel 5000 varsayılan bağlantı noktasında dinleme, uygulama kullanılabilir olmalıdır `http://localhost:5000/`. Uygulama Kestrel uç nokta adresinde normalde yanıt verirse, sorun ilişkili ters proxy yapılandırmasını ve büyük olasılıkla daha az uygulama içinde daha yüksektir.

Ters proxy düzgün çalışıp çalışmadığını belirlemek için bir yoldur stil sayfası, betik veya uygulamanın statik dosyaları görüntüden basit statik dosya isteği gerçekleştirmek için *wwwroot* kullanarak [statik dosya ara yazılımlarını](xref:fundamentals/static-files). Uygulama statik dosyaları görebilecek ancak MVC görünümleri ve diğer uç noktaları başarısız oluyor, sorun büyük olasılıkla daha az ters proxy yapılandırma ile ilgili ve büyük olasılıkla uygulama (örneğin, MVC yönlendirme veya için 500 İç sunucu hatası) içinde ' dir.

Kestrel başlatır normalde IIS ancak uygulama başarıyla yerel olarak çalıştırdıktan sonra sistem üzerinde çalışmadığından, bir ortam değişkeni geçici olarak eklenebilir *web.config* ayarlamak için `ASPNETCORE_ENVIRONMENT` için `Development`. Uygulama başlangıç ortamı kılmadığınız sürece, ortam değişkeni ayarlamayı sağlar [Geliştirici özel durum sayfasında](xref:fundamentals/error-handling) görünmesi uygulama çalıştırıldığında. Ortam değişkeni ayarı `ASPNETCORE_ENVIRONMENT` bu şekilde yalnızca Internet'e açık olmayan hazırlama ve test sunucuları için önerilir. Ortam değişkenini kaldırdığınızdan emin olun *web.config* dosya bitirdikten sonra. Aracılığıyla ortam değişkenlerini ayarlama hakkında bilgi için *web.config*, bkz: [aspNetCore environmentVariables alt öğesi](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).

Çoğu durumda, Uygulama günlüğünü etkinleştirme uygulama veya ters proxy sorun gidermeye yardımcı olur. Bkz: [günlüğü](xref:fundamentals/logging/index) daha fazla bilgi için.

Son sorun giderme ipucu ya da yükselttikten sonra çalışamayan uygulamalar için .NET Core SDK geliştirme makine ya da paketi sürümlerinde uygulama içinde ilgilidir. Bazı durumlarda, önemli yükseltme yaparken, bir uygulama tutarsız paketleri kesilebilir. Bu sorunların çoğunu tarafından çözülebilir:

* Silme `bin` ve `obj` projesinde klasörler.
* Temizleme paketi önbelleğe adresindeki `%UserProfile%\.nuget\packages\` ve `%LocalAppData%\Nuget\v3-cache`.
* Geri yükleme ve projeyi yeniden derlemeyi.
* Önceki dağıtım sunucu üzerinde tamamen uygulamayı yeniden dağıtmadan önce silinmiş olduğunu onaylayan.

> [!TIP]
> Yürütülecek paket önbellekleri temizlemek için kolay bir yol olduğundan `dotnet nuget locals all --clear` bir komut isteminden.
> 
> Paket önbelleklerini temizleme de gerçekleştirilebilir kullanarak [nuget.exe](https://www.nuget.org/downloads) aracı ve komutu yürütülürken `nuget locals all -clear`. *nuget.exe* Windows 10 ile birlikte gelen yükleme değildir ve ayrı olarak NuGet Web sitesinden alınması gerekir.
<!--
> [!TIP]
> A convenient way to clear package caches is to:
>
> * Obtain the *NuGet.exe* tool from [NuGet.org](https://www.nuget.org/).
> * Add the path to *NuGet.exe* to the system PATH.
> * Execute `nuget locals all -clear` from a command prompt.
>
> Alternatively, execute `dotnet nuget locals all --clear` from a command prompt without obtaining *NuGet.exe*. -->

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure App Service ve ASP.NET Core IIS için ortak hataları başvurusu](xref:host-and-deploy/azure-iis-errors-reference)
* [ASP.NET Core Module yapılandırma başvurusu](xref:host-and-deploy/aspnet-core-module)
