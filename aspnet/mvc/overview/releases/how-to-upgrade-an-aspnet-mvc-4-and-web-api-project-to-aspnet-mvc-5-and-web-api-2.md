---
uid: mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2
title: Nasıl bir ASP.NET MVC 4 yükseltin ve Web API projesini ASP.NET MVC 5 ve Web API 2 | Microsoft Docs
author: Rick-Anderson
description: ASP.NET MVC 5 ve Web API 2'öznitelik yönlendirme, kimlik doğrulama filtreleri ve çok daha fazlası dahil olmak üzere yeni özelliklerin bir ana bilgisayarınızı getirin.
ms.author: riande
ms.date: 10/17/2013
ms.assetid: db0d02d9-58e8-4a0b-8d7d-b8df8ea97b88
msc.legacyurl: /mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: e313fc2229026f1b11d2f6a046a8b550c08ba96b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753362"
---
<a name="how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2"></a>Bir ASP.NET MVC 4 ve Web API projelerini ASP.NET MVC 5 ve Web API 2 sürümüne yükseltme yapmayı
====================
Tarafından [Rick Anderson](https://github.com/Rick-Anderson)

> ASP.NET MVC 5 ve Web API 2'öznitelik yönlendirme, kimlik doğrulama filtreleri ve çok daha fazlası dahil olmak üzere yeni özelliklerin bir ana bilgisayarınızı getirin. Bkz: [ https://www.asp.net/vnext ](https://www.asp.net/core) daha fazla ayrıntı için.
> 
> Bu izlenecek yol, uygulamanızın en son sürüme yükseltmek için gerekli adımlarla size yol gösterir.  
> 
> > [!NOTE]
> > Lütfen [için ASP.NET and Web Tools Visual Studio 2013 sürüm notları](../../../visual-studio/overview/2013/release-notes.md) MVC 4 ve Web API sonraki sürümü için önemli değişiklikler hakkında bilgi.
> 
>   
> 
> Bu makale Youngjune Hong Rick Anderson ile yazılmış ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )


## <a name="upgrade-steps"></a>Yükseltme adımları

1. Projenizi yedekleyin. Bu izlenecek yol, proje dosyasını, paket yapılandırmasını ve web.config dosyalarından değişiklikler yapmanızı gerektirir.
2. Web API'si, global.asax dosyasında Web API 2'ye yükseltme için değiştirin:

    [!code-csharp[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample1.cs)]

   to

    [!code-csharp[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample2.cs)]
3. Projelerinizi kullanan tüm paketleri Web API 2 ve MVC 5 ile uyumlu olduğundan emin olun. Aşağıdaki tabloda gösterilen MVC 4 ve Web API paketleri değiştirilmesi gereken daha ilgili. Aşağıdaki paketlerden birini bağımlı olduğu bir paketiniz varsa, Web API 2 ve MVC 5 ile uyumlu olan daha yeni sürümlerini almak için yayımcılar'ne başvurun. Bu paket için kaynak kodu varsa, yeni derlemeler Web API 2 ve MVC 5 ile derlemeniz.   

    | **Paket kimliği** | **Eski sürüm** | **Yeni sürüm** |
    | --- | --- | --- |
    | Microsoft.AspNet.Razor | 2.0.x.x | 3.0.0 |
    | Microsoft.AspNet.WebPages | 2.0.x.x | 3.0.0 |
    | Microsoft.AspNet.WebPages.WebData | 2.0.x.x | 3.0.0 |
    | Microsoft.AspNet.WebPages.OAuth | 2.0.x.x | 3.0.0 |
    | Microsoft.AspNet.Mvc | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.Mvc.Facebook | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.Core | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.SelfHost | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.Client | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.OData | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.WebHost | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.Tracing | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.HelpPage | 4.0.x.x | 5.0.0 |
    | Microsoft.Net.Http | 2.0.x. | 2.2.x. |
    | Microsoft.Data.OData | 5.2.x | 5.6.x |
    | System.Spatial | 5.2.x | 5.6.x |
    | Microsoft.Data.Edm | 5.2.x | 5.6.x |
    | Microsoft.AspNet.Mvc.FixedDisplayModes | < o:p >< / o:p > | Kaldırıldı |
    | Microsoft.AspNet.WebPages.Administration | < o:p >< / o:p > | Kaldırıldı |
    | Microsoft Web Yardımcıları | < o:p >< / o:p > | Microsoft.AspNet.WebHelpers |

    > [!NOTE]
    > Microsoft Web Yardımcıları Microsoft.AspNet.WebHelpers ile değiştirilmiştir. İlk olarak eski paketi kaldırın ve sonra yeni paketi yükleyin.   
    >   
    > Önemli ASP.NET paketleri arasında hiçbir çapraz sürüm uyumluluğu yoktur. Örneğin, MVC 5 yalnızca Razor 3 ve değil Razor 2 ile uyumludur.
4. Projenizi Visual Studio 2013'te açın.
5. Yüklenen aşağıdaki ASP.NET NuGet paketlerinden birini kaldırın. Bu paket Yöneticisi Konsolu (PMC'yi) kullanarak kaldırır. PMC'yi açın, seçin **Araçları** menüsünü ve ardından **kitaplık Paket Yöneticisi** seçip **Paket Yöneticisi Konsolu**. Projenizi bunların hepsini içermeyebilir.

    1. `Microsoft.AspNet.WebPages.Administration`  
   Bu paket, MVC 3'ten MVC 4'e yükseltirken genellikle eklenir. Kaldırmak için PMC'de aşağıdaki komutu çalıştırın:  
        `Uninstall-Package -Id Microsoft.AspNet.WebPages.Administration`
    2. `Microsoft-Web-Helpers`   
   Bu paketi olarak yeni marka adları verilmiştir `Microsoft.AspNet.WebHelpers`. Kaldırmak için PMC'de aşağıdaki komutu çalıştırın:  
        `Uninstall-Package -Id Microsoft-Web-Helpers`
    3. `Microsoft.AspNet.Mvc.FixedDisplayMode`  
   Bu paket, geçici bir çözüm için MVC 4, MVC 5'te düzeltilen bir hata içeriyor. Kaldırmak için PMC'de aşağıdaki komutu çalıştırın:  
        `Uninstall-Package -Id Microsoft.AspNet.Mvc.FixedDisplayModes`
6. PMC kullanarak tüm ASP.NET NuGet paketlerini yükseltin. PMC'de, aşağıdaki komutu çalıştırın:  
    `Update-Package`  
   `Update-Package` Herhangi bir parametre, her paketi güncelleştirir olmadan komutu. Paket kimliği bağımsız değişkenini kullanarak ayrı ayrı güncelleştirebilirsiniz. Güncelleştirme komut hakkında daha fazla bilgi için `get-help update-package` .

## <a name="update-the-application-webconfig-file"></a>Uygulama güncelleştirmesi *web.config* dosyası

Uygulamada bu değişiklikleri yapmak mutlaka *web.config* dosya değil *web.config* dosyası *görünümleri* klasör.

Bulun `<runtime>/<assemblyBinding>` bölümünde ve aşağıdaki değişiklikleri yapın:

1. Öğesinde ad özniteliği "System.Web.Mvc" ile "5.0.0.0" için "4.0.0.0" sürüm numarasını değiştirin. (Bu öğe içinde iki değişiklik.)
2. Ad özniteliği ile öğelerinde &quot;System.Web.Helpers "ve &quot;System.Web.WebPages&quot; "3.0.0.0"için"2.0.0.0"sürüm numarasını değiştirin. Dört değişiklikleri oluşur, her iki.

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample3.xml?highlight=6,10,14)]
3. Bulun `<appSettings>` bölümünde ve 2.0.0.0.0 gelen webpages:version 3.0.0.0 için aşağıda gösterildiği gibi güncelleştirin:

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample4.xml?highlight=2)]
4. Tam dışındaki herhangi bir güven düzeyleri kaldırın. Örneğin:

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample5.xml?highlight=2)]

## <a name="update-the-webconfig-files-under-the-views-folder"></a>Güncelleştirme *web.config* görünümleri klasörü altındaki dosyaları

Uygulamanızı alanları kullanıyorsa, ayrıca her güncelleştirmeniz gerekecektir *web.config* dosyası *görünümleri* her alanı klasörünün alt klasörü.

1. "4.0.0.0" sürümünden "5.0.0.0" Sürüm "System.Web.Mvc" içeren tüm öğeleri güncelleştirin.  

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample6.xml?highlight=2)]

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample7.xml?highlight=4-6,8)]
2. "System.Web.WebPages.Razor" sürümü "2.0.0.0" Sürüm "3.0.0.0" içeren tüm öğeleri güncelleştirin. Bu bölümde "System.Web.WebPages" içeriyorsa, bu öğelerden "2.0.0.0" Sürüm "3.0.0.0" sürümüne güncelleştirin  

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample8.xml?highlight=3-5)]
3. Kaldırdıysanız `Microsoft-Web-Helpers` önceki bir adımda NuGet paketini yüklemek `Microsoft.AspNet.WebHelpers` PMC'yi içinde aşağıdaki komutla:  
    `Install-Package -Id Microsoft.AspNet.WebHelpers`
4. Uygulamanız kullanıyorsa [User.IsInRole()](https://msdn.microsoft.com/en-us/library/system.web.security.roleprincipal.isinrole(v=vs.110).aspx) yöntemi ekleyin *Web.config* dosya.

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample9.xml)]

## <a name="final-steps"></a>Son adımlar

Derleme ve uygulamayı test edin.

MVC 4 proje türü GUID konumundan proje dosyaları kaldırın.

1. Çözüm Gezgini'nde proje adına sağ tıklayın ve ardından **projeyi**.
2. Projeye sağ tıklayıp Düzen ProjectName.csproj seçin.
3. Bulun `ProjectTypeGuids` öğesini ve ardından Kaldır MVC 4 Proje GUID, `{E3E379DF-F4C6-4180-9B81-6769533ABE47}`.
4. Açık proje dosyasını kaydedip kapatın.
5. Projeye sağ tıklayıp **projeyi**.
