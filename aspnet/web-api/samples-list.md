---
uid: web-api/samples-list
title: Web API örnekleri listesi | Microsoft Docs
author: rick-anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/18/2012
ms.topic: article
ms.assetid: 8cbd9d7f-7027-4390-b098-cb81a63ecd6f
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/samples-list
msc.type: content
ms.openlocfilehash: 6c9fff024df6c485b8d904d39cb0a4e5b5bf7e44
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362570"
---
<a name="web-api-samples-list"></a>Web API örnekleri listesi
====================
## <a name="httpclient-samples"></a>HttpClient örnekleri

**Bing Çevir örnek** | [VS 2012 kaynak](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fHttpClient%2fBingTranslateSample%2fReadMe.txt)

Nasıl çağrılacağını gösterir [Microsoft Translator hizmeti](https://msdn.microsoft.com/library/ff512419.aspx) kullanarak **HttpClient** sınıfı. Microsoft Translator hizmeti API translator hizmeti için her istek için belirteci Azure sunucusuna bir istek göndererek uygulamayı alır. bir OAuth belirteci gerektirir. Belirteç sunucudan sonuç uygulamasına çevirisini hizmete gönderilen istek gönderilir. Bu örneği çalıştırmadan önce edinmeniz gerekir bir [Azure Market'ten uygulama anahtarı](https://msdn.microsoft.com/library/hh454950.aspx) ve AccessTokenMessageHandler örnek sınıf bilgileri doldurun.

**Google haritalar örnek** | [ayrıntılı açıklama](https://blogs.msdn.com/b/henrikn/archive/2012/02/17/downloading-a-google-map-to-local-file.aspx) | [VS 2012 kaynak](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fHttpClient%2fGoogleMapsSample%2fReadMe.txt)

Kullanan **HttpClient** Redmond, WA'dan haritasını indirmek için [Google haritalar API'si](https://developers.google.com/maps/), yerel dosya olarak kaydeder ve varsayılan resim görüntüleyici açar.

**İstemci örneği twitter** | [ayrıntılı açıklama](https://blogs.msdn.com/b/henrikn/archive/2012/02/16/extending-httpclient-with-oauth-to-access-twitter.aspx) | [VS 2012 kaynak](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fHttpClient%2fTwitterSample%2fReadMe.txt)

Kullanarak basit bir Twitter istemcisini yazma işlemi gösterilmektedir **HttpClient**. Örnek kullanan bir **HttpMessageHandler** içine giden OAuth kimlik doğrulama bilgilerini eklemek için **HttpRequestMessage**. JSON.NET kullanarak Twitter sonuç okunur. Bu örneği çalıştırmadan önce edinmeniz gerekir bir [uygulama anahtarı twitter'dan](https://dev.twitter.com/)ve OAuthMessageHandler örnek sınıf bilgileri doldurun.

**Dünya banka örnek** | [ayrıntılı açıklama](https://blogs.msdn.com/b/henrikn/archive/2012/02/16/httpclient-is-here.aspx) | [VS 2010 kaynak](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fHttpClient%2fWorldBankSample%2fReadMe.txt) | [VS 2012 kaynak](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fHttpClient%2fWorldBankSample%2fReadMe.txt)

İstemcinin sonucu ayrıştırması için JSON.NET kullanan dünya banka veri siteden veri alma işlemi gösterilmektedir.

## <a name="web-api-samples"></a>Web API örnekleri

**ASP.NET Web API'si ile çalışmaya başlama** | [VS 2012 kaynak](overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)

Temel bir web API HTTP GET isteklerini destekleyen oluşturma işlemi gösterilmektedir. Öğretici için kaynak kodunu içeren [bilgisayarınızı ilk ASP.NET Web API](overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).

**ASP.NET Web API JavaScript senaryoları açıklamaları** | [VS 2012 kaynak](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7)

Web tarayıcı istemcileri desteklemek ve jQuery kullanılarak kolayca çağrılabilir API'leri oluşturmak için ASP.NET Web API'sini kullanma işlemi gösterilmektedir.

**Kişi Yöneticisi** | [VS 2010 kaynak](https://code.msdn.microsoft.com/Contact-Manager-Web-API-0e8e373d)

Bu örnek, bir basit Kişi Yöneticisi uygulaması oluşturmak için ASP.NET Web API kullanır. Uygulamayı görüntülemek ve ilgili kişi listesini yönetmek için bir ASP.NET MVC uygulaması ve Windows Phone uygulama tarafından kullanılan bir kişi yöneticisi web API'den oluşur.

**Örnek toplu işleme** | [ayrıntılı açıklama](http://trocolate.wordpress.com/2012/07/19/mitigate-issue-260-in-batching-scenario/) | [VS 2012 kaynak](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fHostedBatchSample%2fReadMe.txt)

HTTP ASP.NET içinde toplu işleme uygulanması gösterilmektedir. Toplu işleme, birden çok HTTP istekleri sunucuya bir HTTP POST olarak gönderilmesi tek bir MIME çok bölümlü bir varlık gövdesi içinde koymak oluşur. İstekleri ayrı ayrı işlenir ve yanıt istemciye döndürülen başka bir MIME çok bölümlü varlık gövdesini içine yerleştirilir.

**Denetleyici örneği içerik** | [ayrıntılı açıklama](https://blogs.msdn.com/b/henrikn/archive/2012/02/24/async-actions-in-asp-net-web-api.aspx) | [VS 2010 kaynak](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fContentControllerSample%2fReadMe.txt) | [VS 2012 kaynak](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fContentControllerSample%2fReadMe.txt)

Okuma ve zaman uyumsuz olarak akış'ı kullanarak istek ve yanıt varlıkları yazma işlemi gösterilmektedir. Örnek denetleyicisi iki eylem vardır: İstek Varlık gövdesi zaman uyumsuz olarak okur ve yerel bir dosyada depolayan bir PUT eylem ve yerel dosya içeriğini döndürür bir alma işlemi.

**Özel derleme Çözümleyicisi örnek** | [VS 2012 kaynak](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fCustomAssemblyResolverSample%2fReadMe.txt)

Dinamik olarak yüklenen kitaplık derlemesine denetleyicilerinden bulunmasını desteklemek için ASP.NET Web API değiştirme işlemi gösterilmektedir. Örnek bir özel uygulayan **IAssembliesResolver** varsayılan uygulama çağırır ve ardından Kitaplık derlemesine varsayılan sonuçlara ekler.

**Özel medya türü biçimlendiricisi örneği** | [ayrıntılı açıklama](https://blogs.msdn.com/b/henrikn/archive/2012/04/23/using-cookies-with-asp-net-web-api.aspx) | [VS 2010 kaynak](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fCustomMediaTypeFormatterSample%2fReadMe.txt)

Kullanarak bir özel medya türü biçimlendiricisini oluşturulacağını gösterir **BufferedMediaTypeFormatter** temel sınıfı. Bu temel sınıf için öncelikle kullanmak zaman uyumlu okuma ve yazma işlemleri biçimlendiricileri yöneliktir. Medya türü biçimlendiricisi gösteren ek olarak, örnek bir parçası olarak kaydederek bağlama işlemi gösterilmektedir **HttpConfiguration** uygulamanız için. Ayrıca kullanmak mümkün unutmayın **MediaTypeFormatter** temel sınıfından doğrudan, öncelikli olarak zaman uyumsuz kullanan biçimlendiricileri okuma ve yazma işlemleri için.

**Özel parametre bağlama örnek** | [ayrıntılı açıklama](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx) | [VS 2010 kaynak](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fCustomParameterBinding%2fReadMe.txt)

Bir istekten daha fazla bilgi için eylem parametrelerini nasıl bağlı belirleyen işlem parametresi bağlama işleminin nasıl özelleştirileceğini gösterir. Bu örnekte, giriş denetleyicisine dört eylem vardır:

1. BindPrincipal nasıl IPrincipal parametresi bir HTTP GET iletisi yerine özel bir genel kural bağlanacağını gösterir.
2. BindCustomComplexTypeFromUriOrBody, ileti gövdesinin veya istek URI'SİNDEN bir HTTP POST iletisi gelebilir bir karmaşık tür parametre bağlama işlemi gösterilmektedir;
3. BindCustomComplexTypeFromUriWithRenamedProperty shows how to bind a complex-type parameter with a renamed property which comes from the request URI of an HTTP POST message;
4. PostMultipleParametersFromBody POST iletinin gövdesinden birden çok parametre bağlama gösterilmektedir;

**Dosya yükleme örneği** | [ayrıntılı açıklama](https://blogs.msdn.com/b/henrikn/archive/2012/03/01/file-upload-and-asp-net-web-api.aspx) | [VS 2012 kaynak](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fFileUploadSample%2fReadMe.txt)

Dosyaları karşıya yükleme işlemini gösteren bir **ApiController** MIME çok bölümlü dosya karşıya yükleme ve ilerleme durumunu bildirimler ile kurma kullanarak **HttpClient** kullanarak **ProgressNotificationHandler**. Denetleyici bir HTML dosyası karşıya yükleme içeriğini zaman uyumsuz olarak okur ve bir veya daha fazla gövde bölümü yerel bir dosyaya yazar. Yanıt, karşıya yüklenen dosya (veya dosyaları) hakkında bilgi içerir.

**Azure Blob Store Sample karşıya dosya** | [ayrıntılı açıklama](https://blogs.msdn.com/b/yaohuang1/archive/2012/07/02/asp-net-web-api-and-azure-blob-storage.aspx) | [VS 2012 kaynak](http://aspnet.codeplex.com/SourceControl/changeset/view/61dfed023e50#Samples%2fNet45%2fCS%2fWebApi%2fAzureBlobsFileUploadSample%2fReadMe.txt)

Bu örnek için dosya yükleme örneği benzer, ancak yerel diskte karşıya yüklenen dosyaları kaydetmek yerine, bu zaman uyumsuz olarak dosyaları karşıya yükler [Azure Blob Store](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs) kullanarak [.NET için Windows Azure SDK'sı](https://www.windowsazure.com/develop/net/). Ayrıca şu anda mevcut blobları listelemek için bir mekanizma sağlar bir [Azure Blob Depolama kapsayıcısına](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs). Çalışan örnek deneyebilirsiniz **Azure Storage öykünücüsü** Azure SDK'sı ile birlikte gelir. Varsa bir [Azure depolama hesabı](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs), karşı gerçek depolama hizmetini de çalıştırabilirsiniz.

**HTTP ileti işleyicisi işlem hattı örneği** | [ayrıntılı açıklama](https://blogs.msdn.com/b/henrikn/archive/2012/08/07/httpclient-httpclienthandler-and-httpwebrequesthandler.aspx) | [VS 2010 kaynak](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fHttpMessageHandlerPipelineSample%2fReadMe.txt)

Yukarı wire gösterilmektedir **HttpMessageHandler** hem de istemci örnekleri (**HttpClient**) ve sunucu (ASP.NET Web API). Bu örnekte, aynı işleyici hem istemci hem de sunucu kullanılır. Tam aynı işleyici her iki yerde de çalışır nadirdir, ancak nesne modelini istemci ve sunucu tarafında aynıdır.

**JSON yükleme örneği** | [VS 2012 kaynak](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fJsonUploadSample%2fReadMe.txt)

Karşıya yükleme ve JSON ve ondan indirme işlemini gösterir bir **ApiController**. En az bir örnek kullanır **ApiController** ve kullanarak eriştiği **HttpClient**.

**Mashup örnek** | [ayrıntılı açıklama](https://blogs.msdn.com/b/henrikn/archive/2012/03/03/async-mashups-using-asp-net-web-api.aspx) | [VS 2012 kaynak](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fMashupSample%2fReadMe.txt)

Zaman uyumsuz olarak birden çok uzak siteleri içinden erişmek nasıl gösterir bir **ApiController** eylem. Böylece iş parçacığı engellenir eylemi tıklıyorum ve her zaman isteklerin zaman uyumsuz olarak gerçekleştirilir.

**Bellek izleme örnek** | [ayrıntılı açıklama](https://blogs.msdn.com/b/roncain/archive/2012/04/12/tracing-in-asp-net-web-api.aspx) | [VS 2010 kaynak](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fMemoryTracingSample%2fReadMe.txt)

Bu örnek proje bir özel bellek içi izleme yazıcısı ASP.NET Web API uygulamaları yükleyecek bir Nuget paketi oluşturur.

**MongoDB örnek** | [ayrıntılı açıklama](https://blogs.msdn.com/b/henrikn/archive/2012/02/19/using-web-api-with-mongodb.aspx) | [VS 2012 kaynak](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fMongoSample%2fReadMe.txt)

MongoDB için kalıcı depolama olarak kullanma işlemini gösterir bir **ApiController**, bir depo deseni kullanılarak.

**Yanıt gövdesi işlemci örnek** | [VS 2012 kaynak](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fResponseEntityProcessorSample%2fReadMe.txt)

İstemciye aktarılmadan önce yanıt varlık (diğer bir deyişle, bir HTTP yanıt gövdesinde) yerel bir dosyaya kopyalayın ve ek söz konusu dosya üzerinde zaman uyumsuz işleme gerçekleştirmek nasıl gösterir. Örnek uygulayan bir **HttpMessageHandler** hem yazdığını normal olarak çıktı ve yerel bir dosyaya kendisini yanıt varlık biriyle sonuna geldik.

**XDocument örnek karşıya** | [ayrıntılı açıklama](https://blogs.msdn.com/b/henrikn/archive/2012/02/17/push-and-pull-streams-using-httpclient.aspx) | [VS 2012 kaynak](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fUploadXDocumentSample%2fReadMe.txt)

Bir XDocument için yüklemeyi gösteren bir **ApiController** kullanarak **PushStreamContent** ve **HttpClient**.

**Doğrulama örnek** | [VS 2010 kaynak](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fValidationSample%2fReadMe.txt)

Nasıl doğrulama özniteliklerinin, ASP.NET Webapı modellerinde üzerinde HTTP isteğinin içeriği doğrulamak için kullanabileceğiniz gösterilmektedir. Framework tarafından tanımlanan her ikisi de kullanmayı ve özel doğrulama öznitelikleri, model öğesine açıklama eklemek için gerektiği gibi özellikleri işaretleyin ve hata yanıtları geçersiz model durumu için döndürülecek gösterir.

**Web formu örnek** | [ayrıntılı açıklama](https://blogs.msdn.com/b/henrikn/archive/2012/02/23/using-asp-net-web-api-with-asp-net-web-forms.aspx) | [VS 2010 kaynak](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fWebFormSample%2fReadMe.txt)

Bir Web Forms projesine eklenen bir ApiController gösterir.

**[RestBugs örnek](https://github.com/howarddierking/RestBugs)**

RestBugs ASP.NET Web API ve yeni HTTP istemci kitaplığı gösterimde temelli bir sistem oluşturmak için nasıl kullanılacağını gösteren uygulama izleme, basit bir hatadır. Örnek ASP.NET Web API'sini kullanarak, istemci ve sunucu uygulamaları içerir. Sunucu, kaynak ifadeleri oluşturmak için özel bir Razor biçimlendirici kullanır. Örnek ayrıca istemcileri ve sunucuları ayrıştırmak için bir hiper medya tasarım kullanarak gelen avantajların göstermek için bir node.js sunucusu sağlar.

## <a name="web-api-extensions-preview-samples"></a>Web API'si uzantıları Önizleme örnekleri

**OData sorgulanabilir örnek** | [ayrıntılı açıklama](https://blogs.msdn.com/b/alexj/archive/2012/08/15/odata-support-in-asp-net-web-api.aspx) | [VS 2010 kaynak](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fODataQueryableSample%2fReadMe.txt)

Kullanarak ASP.NET Web API OData sorgu konusunda gösterir `[Queryable]` kullanarak veya özniteliği **ODataQueryOptions** eylem parametresi, bir sorgu yürütülmekte olan önce el ile denetlemek bir eylem sağlar.

[Queryable] özniteliğini kullanarak CustomerController gösterir ve OrderController ODataQueryOptions parametrenin nasıl kullanılacağını gösterir. ResponseController benzer CustomerController ancak döndüren GET eylemi yerine `IEnumerable<Customer>` döndürür bir **HttpResponseMessage**. Bu ek üstbilgi alanlarını ekleyin, sorgu işlevselliği kullanmaya devam ederken durum kodu, vb. işleme olanak tanır. Örnek, $orderby, $skip, $top, any(), all() ve $filter kullanarak sorguları gösterir.

**OData hizmet örneği** | [ayrıntılı açıklama](https://blogs.msdn.com/b/alexj/archive/2012/08/15/odata-support-in-asp-net-web-api.aspx) | [VS 2010 kaynak](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fODataServiceSample%2fReadMe.txt)

Bu örnek, üç varlıkları ve üç Web APİ'si denetleyicilerinin oluşan bir OData hizmetinin nasıl oluşturulduğunu gösterir. Denetleyicileri çeşitli düzeylerde ortaya OData işlevsellik bakımından işlevleri sağlar:

SupplierController sorgu dahil olmak üzere işlevlerinin bir alt kümesini sunar, bu istekleri işleyerek anahtarı ile oluşturma, alma:

- /Suppliers Al
- /Suppliers(KEY) Al
- GET /Suppliers? $filter =... &amp;$orderby =... &amp;$top =... &amp;$skip =...
- POST /Suppliers

ProductsController kullanıma sunan Al koy, gönderin, silme ve eylem bu işlemlerin her biri için doğrudan uygulayarak düzeltme eki.

Zengin bir OData hizmeti uygulamak için kullanışlı bir desen sunan EntitySetController temel sınıf ProductFamilesController yararlanır.

Ayrıca tüketilen veri sağlayan bir $metadata belge OData hizmeti sunan WCF veri hizmeti istemcileri ve $metadata biçimi kabul diğer istemciler tarafından.
