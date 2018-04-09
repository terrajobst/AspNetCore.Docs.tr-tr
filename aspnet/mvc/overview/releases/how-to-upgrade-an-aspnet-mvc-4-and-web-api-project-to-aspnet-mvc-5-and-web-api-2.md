---
uid: mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2
title: Bir ASP.NET MVC 4 yükseltme ve Web API projesi ASP.NET MVC 5 ve Web API 2 | Microsoft Docs
author: Rick-Anderson
description: ASP.NET MVC 5 ve Web API 2 yeni özelliklerin özniteliği yönlendirme, kimlik doğrulaması filtreleri ve çok daha fazlasını içeren bir ana bilgisayar duruma getirin.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: db0d02d9-58e8-4a0b-8d7d-b8df8ea97b88
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: f61502933a5ba92896ee97cef9cff915fe23831d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2"></a>Bir ASP.NET MVC 4 ve Web API projesi ASP.NET MVC 5 ve Web API 2'ye yükseltme yapma
====================
tarafından [Rick Anderson](https://github.com/Rick-Anderson)

> ASP.NET MVC 5 ve Web API 2 yeni özelliklerin özniteliği yönlendirme, kimlik doğrulaması filtreleri ve çok daha fazlasını içeren bir ana bilgisayar duruma getirin. Bkz: [ https://www.asp.net/vnext ](https://www.asp.net/core) daha fazla ayrıntı için.
> 
> Bu kılavuz, uygulamanızın en son sürüme yükseltmek için gerekli adımları size yol gösterecektir.  
> 
> > [!NOTE]
> > Lütfen bakın [ASP.NET ve Web Araçları Visual Studio 2013 sürüm notları için](../../../visual-studio/overview/2013/release-notes.md) MVC 4 ve Web API sonraki sürümü için önemli değişiklikler hakkında bilgi için.
> 
>   
> 
> Bu makalede Youngjune Hong ve Rick Anderson tarafından yazılan ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )


## <a name="upgrade-steps"></a>Yükseltme adımları

1. Projenizi yedekleme. Bu kılavuz, proje dosyasını, paket yapılandırmanızı ve web.config dosyalarında değişiklik yapmanızı gerektirir.
2. Web API Web API 2'ye, global.asax dosyasında, yükseltme için değiştirin:

    [!code-csharp[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample1.cs)]

   to

    [!code-csharp[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample2.cs)]
3. Projelerinizi kullanan tüm paketler MVC 5 ve Web API 2 ile uyumlu olduğundan emin olun. Aşağıdaki tabloda gösterildiği MVC 4 ve Web API paketleri değiştirilmesi gereken daha ilgili. Lütfen aşağıda listelenen paketlerden birini bağlı bir paket varsa, MVC 5 ve Web API 2 ile uyumlu olan daha yeni sürümlerini almak için yayımcılar başvurun. Bu paket için kaynak kodu varsa, MVC 5 ve Web API 2 yeni derlemeler ile derlemeniz.   

    | **Paket kimliği** | **Eski sürümü** | **Yeni sürüm** |
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
    | Microsoft.AspNet.Mvc.FixedDisplayModes | < o: >< / o: p > | Kaldırıldı |
    | Microsoft.AspNet.WebPages.Administration | < o: >< / o: p > | Kaldırıldı |
    | Microsoft-Web-Helpers | < o: >< / o: p > | Microsoft.AspNet.WebHelpers |

    > [!NOTE]
    > Microsoft Web Yardımcıları Microsoft.AspNet.WebHelpers ile değiştirilmiştir. Eski paketi kaldırın ve sonra yeni paketi yükleyin.   
    >   
    > Önde gelen ASP.NET paketler arasında hiçbir çapraz sürüm uyumluluk yoktur. Örneğin, MVC 5 yalnızca Razor 3 ve değil Razor 2 ile uyumludur.
4. Projenizi Visual Studio 2013'te açın.
5. Yüklü olan aşağıdaki ASP.NET NuGet paketlerinden birini kaldırın. Bu paket Yöneticisi Konsolu (PMC) kullanarak kaldırır. PMC açmak için seçin **Araçları** menüsüne ve ardından **kitaplık Paket Yöneticisi** seçip **Paket Yöneticisi Konsolu**. Projenizi bunların tümünü içermeyebilir.

    1. `Microsoft.AspNet.WebPages.Administration`  
   Bu paket, MVC 3 ile MVC 4'e yükseltme yaparken genellikle eklenir. Kaldırmak için PMC aşağıdaki komutu çalıştırın:  
        `Uninstall-Package -Id Microsoft.AspNet.WebPages.Administration`
    2. `Microsoft-Web-Helpers`   
   Bu paket olarak rebranded `Microsoft.AspNet.WebHelpers`. Kaldırmak için PMC aşağıdaki komutu çalıştırın:  
        `Uninstall-Package -Id Microsoft-Web-Helpers`
    3. `Microsoft.AspNet.Mvc.FixedDisplayMode`  
   Bu paket, MVC 5'te düzeltilen MVC 4'te bir hata için geçici bir çözüm içerir. Kaldırmak için PMC aşağıdaki komutu çalıştırın:  
        `Uninstall-Package -Id Microsoft.AspNet.Mvc.FixedDisplayModes`
6. PMC kullanarak tüm ASP.NET NuGet paketleri yükseltin. PMC içinde aşağıdaki komutu çalıştırın:  
    `Update-Package`  
   `Update-Package` Herhangi bir parametre her paketi güncelleştirir olmadan komutu. Paketleri kimliği bağımsız değişkeni kullanarak tek tek güncelleştirebilirsiniz. Güncelleştirme komut hakkında daha fazla bilgi için Çalıştır `get-help update-package` .

## <a name="update-the-application-webconfig-file"></a>Uygulama güncelleştirmesi *web.config* dosyası

Uygulamada bu değişiklikler yapmaya dikkat edin *web.config* dosyası değil, *web.config* dosyasını *görünümleri* klasör.

Bulun `<runtime>/<assemblyBinding>` bölümünde ve aşağıdaki değişiklikleri yapın:

1. Öğelerle name özniteliği "System.Web.Mvc", "4.0.0.0" "5.0.0.0" için sürüm numarasını değiştirin. (Bu öğesinde iki değişiklikleri.)
2. Ad özniteliği olan öğeleri &quot;System.Web.Helpers "ve &quot;System.Web.WebPages&quot; "2.0.0.0""3.0.0.0"için sürüm numarasını değiştirin. Dört değişiklikleri gerçekleşir, iki öğelerin her biri.

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample3.xml?highlight=6,10,14)]
3. Bulun `<appSettings>` bölümünde ve 2.0.0.0.0 gelen webpages:version 3.0.0.0 için aşağıda gösterildiği gibi güncelleştirin:

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample4.xml?highlight=2)]
4. Tam dışındaki tüm güven düzeyleri kaldırın. Örneğin:

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample5.xml?highlight=2)]

## <a name="update-the-webconfig-files-under-the-views-folder"></a>Güncelleştirme *web.config* görünümleri klasör altındaki dosyalar

Uygulamanızı alanları kullanıyorsa, ayrıca her güncelleştirmeniz gerekecektir *web.config* dosyasını *görünümleri* her alanı klasörünün alt klasörü.

1. "System.Web.Mvc" Sürüm "5.0.0.0" "4.0.0.0" sürümünden içeren tüm öğeleri güncelleştirin.  

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample6.xml?highlight=2)]

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample7.xml?highlight=4-6,8)]
2. "System.Web.WebPages.Razor" Sürüm "3.0.0.0" "2.0.0.0" sürümünden içeren tüm öğeleri güncelleştirin. Bu bölüm, "System.Web.WebPages" içerir, bu öğelerden 2.0.0.0 veya sonraki "" "3.0.0.0" sürümüne güncelleştirin.  

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample8.xml?highlight=3-5)]
3. Kaldırdıysanız `Microsoft-Web-Helpers` bir önceki adımda NuGet paket yüklemesi `Microsoft.AspNet.WebHelpers` ile PMC de aşağıdaki komutu çalıştırın:  
    `Install-Package -Id Microsoft.AspNet.WebHelpers`
4. Uygulamanız kullanıyorsa [User.IsInRole()](https://msdn.microsoft.com/en-us/library/system.web.security.roleprincipal.isinrole(v=vs.110).aspx) yöntemi, aşağıdakileri ekleyin *Web.config* dosya.

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample9.xml)]

## <a name="final-steps"></a>Son adımlar

Derleme ve uygulamayı test etme.

MVC 4 proje türü GUID proje dosyalarını kaldırın.

1. Çözüm Gezgini'nde proje adına sağ tıklayın ve ardından **projeyi**.
2. Projeye sağ tıklayın ve düzenlemek ProjectName.csproj seçin.
3. Bulun `ProjectTypeGuids` öğesini ve ardından Kaldır MVC 4 Proje GUID, `{E3E379DF-F4C6-4180-9B81-6769533ABE47}`.
4. Proje Aç dosyasını kaydedip kapatın.
5. Projeye sağ tıklayın ve seçin **projeyi yeniden yükle**.
