---
uid: web-api/samples-list
title: "Web API örnekleri listesi | Microsoft Docs"
author: rick-anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/18/2012
ms.topic: article
ms.assetid: 8cbd9d7f-7027-4390-b098-cb81a63ecd6f
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/samples-list
msc.type: content
ms.openlocfilehash: 2f40cd4bebdd64c3a4b94cfc1e717fa4b304e57e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="web-api-samples-list"></a>Web API örnekleri listesi
====================
## <a name="httpclient-samples"></a>HttpClient örnekleri

**Bing Çevir örnek** | [VS 2012 kaynak](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fHttpClient%2fBingTranslateSample%2fReadMe.txt)

Nasıl çağrılacağını gösterir [Microsoft Translator hizmet](https://msdn.microsoft.com/en-us/library/ff512419.aspx) kullanarak **HttpClient** sınıfı. Microsoft Translator hizmeti API'si Azure belirteci sunucunun Çeviricisi hizmetine her istek için bir istek göndererek uygulama edinir OAuth belirteci gerektirir. Belirteç sunucunun sonucundan çeviri hizmete gönderilen istek uygulamasına gönderilir. Bu örneği çalıştırmadan önce edinmeniz gerekir bir [uygulama anahtarı Azure Marketi'nden](https://msdn.microsoft.com/en-us/library/hh454950.aspx) ve AccessTokenMessageHandler örnek sınıfındaki bilgileri doldurun.

**Google haritalar örneği** | [ayrıntılı açıklama](https://blogs.msdn.com/b/henrikn/archive/2012/02/17/downloading-a-google-map-to-local-file.aspx) | [VS 2012 kaynak](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fHttpClient%2fGoogleMapsSample%2fReadMe.txt)

Kullanan **HttpClient** Redmond, WA haritasını indirmek için [Google haritalar API'si](https://developers.google.com/maps/), yerel bir dosya olarak kaydeder ve varsayılan görüntü Görüntüleyicisi'ni açar.

**İstemci örnek twitter** | [ayrıntılı açıklama](https://blogs.msdn.com/b/henrikn/archive/2012/02/16/extending-httpclient-with-oauth-to-access-twitter.aspx) | [VS 2012 kaynak](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fHttpClient%2fTwitterSample%2fReadMe.txt)

Kullanarak basit bir Twitter istemcisini yazma gösterilmektedir **HttpClient**. Örnek kullanan bir **HttpMessageHandler** giden içine OAuth kimlik doğrulama bilgilerini eklemek için **HttpRequestMessage**. Twitter sonucundan JSON.NET kullanarak okunur. Bu örneği çalıştırmadan önce edinmeniz gerekir bir [Twitter uygulama anahtarından](https://dev.twitter.com/)ve OAuthMessageHandler örnek sınıfındaki bilgileri doldurun.

**Dünya Bank örnek** | [ayrıntılı açıklama](https://blogs.msdn.com/b/henrikn/archive/2012/02/16/httpclient-is-here.aspx) | [VS 2010 kaynak](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fHttpClient%2fWorldBankSample%2fReadMe.txt) | [VS 2012 kaynak](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fHttpClient%2fWorldBankSample%2fReadMe.txt)

Sonuç ayrıştırmak için JSON.NET kullanan World banka verileri site veritabanından veri almak nasıl gösterir.

## <a name="web-api-samples"></a>Web API'si örnekleri

**ASP.NET Web API'si ile çalışmaya başlama** | [VS 2012 kaynak](overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)

Temel bir web API HTTP GET isteklerini destekler oluşturulacağını gösterir. Öğretici için kaynak kodunu içerir [bilgisayarınızı ilk ASP.NET Web API](overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).

**ASP.NET Web API JavaScript senaryoları – açıklamaları** | [VS 2012 kaynak](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7)

ASP.NET Web API web tarayıcı istemcileri destekleyen ve jQuery kullanılarak kolayca çağrılabilir API'ları oluşturmak için nasıl kullanılacağını gösterir.

**İlgili Kişi Yöneticisi** | [VS 2010 kaynak](https://code.msdn.microsoft.com/Contact-Manager-Web-API-0e8e373d)

Bu örnek, bir basit kişi manager uygulaması oluşturmak için ASP.NET Web API kullanır. Uygulamayı görüntülemek ve kişilerinizin listesini yönetmek için bir ASP.NET MVC uygulaması ve bir Windows Phone uygulama tarafından kullanılan bir kişi yöneticisi web API oluşur.

**Toplu işlem örnek** | [ayrıntılı açıklama](http://trocolate.wordpress.com/2012/07/19/mitigate-issue-260-in-batching-scenario/) | [VS 2012 kaynak](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fHostedBatchSample%2fReadMe.txt)

HTTP ASP.NET içinde toplu işleme uygulamak gösterilmiştir. Toplu işleme sonra sunucuya bir HTTP POST olarak gönderilen tek bir MIME çok bölümlü Varlık gövdesi içinde birden çok HTTP isteklerini koyma oluşur. İstekleri ayrı ayrı işlenir ve yanıtları, istemciye döndürülen başka bir MIME çok bölümlü Varlık gövdesi içine konur.

**İçerik denetleyicisi örnek** | [ayrıntılı açıklama](https://blogs.msdn.com/b/henrikn/archive/2012/02/24/async-actions-in-asp-net-web-api.aspx) | [VS 2010 kaynak](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fContentControllerSample%2fReadMe.txt) | [VS 2012 kaynak](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fContentControllerSample%2fReadMe.txt)

Okuma ve yazma zaman uyumsuz akışlar kullanarak istek ve yanıt varlıkları gösterilmektedir. Örnek denetleyicisi iki eylem vardır: İstek Varlık gövdesi zaman uyumsuz olarak okur ve yerel bir dosyada depolar PUT eylemi ve yerel dosyanın içeriğini döndüren alma işlemi.

**Özel derleme Çözümleyicisi örnek** | [VS 2012 kaynak](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fCustomAssemblyResolverSample%2fReadMe.txt)

ASP.NET Web API denetleyicilerinin dinamik olarak yüklü olan kitaplık derlemesinden bulma destekleyecek şekilde değiştirmek gösterilmiştir. Özel bir örnek uygulayan **IAssembliesResolver** varsayılan uygulama çağırır ve sonra kitaplık derlemesinin varsayılan sonuçları ekler.

**Özel medya türü biçimlendiricisi örnek** | [ayrıntılı açıklama](https://blogs.msdn.com/b/henrikn/archive/2012/04/23/using-cookies-with-asp-net-web-api.aspx) | [VS 2010 kaynak](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fCustomMediaTypeFormatterSample%2fReadMe.txt)

Kullanarak bir özel medya türü biçimlendiricisini oluşturmayı gösteren **BufferedMediaTypeFormatter** temel sınıfı. Taban sınıfı öncelikle kullanmak eşzamanlı okuma ve yazma işlemleri biçimlendiricileri için tasarlanmıştır. Ek olarak gösteren medya türü biçimlendiricisi, örnek bir parçası olarak kaydederek takma gösterilmektedir **HttpConfiguration** uygulamanız için. Bu ayrıca kullanmak mümkün olduğunu unutmayın **MediaTypeFormatter** temel sınıfı doğrudan, öncelikle zaman uyumsuz kullanan biçimlendiricileri okuma ve yazma işlemleri için.

**Özel parametre bağlama örnek** | [ayrıntılı açıklama](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx) | [VS 2010 kaynak](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fCustomParameterBinding%2fReadMe.txt)

Bir istek bilgileri için eylem parametrelerini nasıl bağlı belirler işlemidir parametre bağlama işlemini nasıl özelleştireceğinizi gösterir. Bu örnekte, giriş denetleyicisi dört eylem vardır:

1. BindPrincipal nasıl IPrincipal parametresi bir HTTP GET iletisi değil, özel genel sorumlu bağlanacağını gösterir;
2. BindCustomComplexTypeFromUriOrBody nasıl ileti gövdesi ya da bir HTTP POST iletisi URI'sini isteğinden gelebilir bir karmaşık tür parametre bağlanacağını gösterir;
3. BindCustomComplexTypeFromUriWithRenamedProperty nasıl bir HTTP POST iletisi URI'sini istekten gelen yeniden adlandırılmış bir özelliği bir karmaşık türü parametresiyle bağlanacağını gösterir;
4. PostMultipleParametersFromBody nasıl gövdesinden bir posta iletisi için birden çok parametre bağlanacağını gösterir;

**Örnek karşıya dosya** | [ayrıntılı açıklama](https://blogs.msdn.com/b/henrikn/archive/2012/03/01/file-upload-and-asp-net-web-api.aspx) | [VS 2012 kaynak](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fFileUploadSample%2fReadMe.txt)

Dosyaları yüklemeyi gösteren bir **ApiController** MIME çok bölümlü dosya karşıya yükleme ve ilerleme bildirimlerle kurma kullanarak **HttpClient** kullanarak **ProgressNotificationHandler**. Denetleyici HTML dosya karşıya yükleme içeriğini zaman uyumsuz olarak okur ve bir veya daha fazla gövde bölümü yerel bir dosyaya yazar. Yanıt karşıya yüklenen dosya (veya dosyaları) hakkında bilgi içerir.

**Azure Blob Depolama örnek yüklenecek dosya** | [ayrıntılı açıklama](https://blogs.msdn.com/b/yaohuang1/archive/2012/07/02/asp-net-web-api-and-azure-blob-storage.aspx) | [VS 2012 kaynak](http://aspnet.codeplex.com/SourceControl/changeset/view/61dfed023e50#Samples%2fNet45%2fCS%2fWebApi%2fAzureBlobsFileUploadSample%2fReadMe.txt)

Bu örnek dosya karşıya yükleme örneğe benzer, ancak yerel diskteki karşıya yüklenen dosyaların kaydetme yerine, bu zaman uyumsuz olarak dosyaları karşıya yükleme [Azure Blob Depolama](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs) kullanarak [.NET için Windows Azure SDK](https://www.windowsazure.com/en-us/develop/net/). Ayrıca, mevcut BLOB listeleme için bir mekanizma sağlar bir [Azure Blob Depolama kapsayıcısını](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs). Karşı çalışan örnek deneyebilirsiniz **Azure Storage öykünücüsü** Azure SDK ile birlikte gelir. Varsa bir [Azure depolama hesabı](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs), karşı gerçek depolama hizmetini de çalıştırabilirsiniz.

**HTTP ileti işleyicisi ardışık örnek** | [ayrıntılı açıklama](https://blogs.msdn.com/b/henrikn/archive/2012/08/07/httpclient-httpclienthandler-and-httpwebrequesthandler.aspx) | [VS 2010 kaynak](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fHttpMessageHandlerPipelineSample%2fReadMe.txt)

Yukarı wire gösterilmektedir **HttpMessageHandler** hem de istemci örnekleri (**HttpClient**) ve sunucu (ASP.NET Web API). Aşağıdaki örnekte istemci ve sunucu üzerinde aynı işleyici kullanılır. Her iki yerde de tam aynı işleyici çalıştırılır ender olsa da, nesne modeli istemci ve sunucu tarafında aynıdır.

**JSON karşıya örnek** | [VS 2012 kaynak](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fJsonUploadSample%2fReadMe.txt)

Karşıya yükleme ve JSON için indirin gösterilmektedir bir **ApiController**. Örnek en az kullanır **ApiController** ve kullanarak erişir **HttpClient**.

**Karma örnek** | [ayrıntılı açıklama](https://blogs.msdn.com/b/henrikn/archive/2012/03/03/async-mashups-using-asp-net-web-api.aspx) | [VS 2012 kaynak](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fMashupSample%2fReadMe.txt)

Birden çok uzak siteleri zaman uyumsuz olarak içinden nasıl erişileceği gösterir bir **ApiController** eylem. Böylece iş parçacığı engellenen eylemi İsabeti, her zaman isteklerini zaman uyumsuz olarak gerçekleştirilir.

**Bellek izleme örnek** | [ayrıntılı açıklama](https://blogs.msdn.com/b/roncain/archive/2012/04/12/tracing-in-asp-net-web-api.aspx) | [VS 2010 kaynak](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fMemoryTracingSample%2fReadMe.txt)

Bu örnek proje özel bellek içi izleme yazıcısı ASP.NET Web API uygulamaları yükleyecek bir Nuget paketi oluşturur.

**MongoDB örnek** | [ayrıntılı açıklama](https://blogs.msdn.com/b/henrikn/archive/2012/02/19/using-web-api-with-mongodb.aspx) | [VS 2012 kaynak](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fMongoSample%2fReadMe.txt)

MongoDB için kalıcı deposu olarak kullanmak üzere nasıl gösterir bir **ApiController**, bir depo desen.

**Yanıt gövdesi işlemci örnek** | [VS 2012 kaynak](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fResponseEntityProcessorSample%2fReadMe.txt)

İstemciye iletilmeden önce yanıt varlık (diğer bir deyişle, bir HTTP yanıt gövdesi) yerel bir dosyaya kopyalayın ve ek bu dosyada zaman uyumsuz işlem gerçekleştirmek nasıl gösterir. Örnek uygulayan bir **HttpMessageHandler** her ikisi de yazdığını çıktısı normal olarak ve yerel bir dosya için kendisini yanıt varlık biriyle sarmalar.

**XDocument örnek karşıya** | [ayrıntılı açıklama](https://blogs.msdn.com/b/henrikn/archive/2012/02/17/push-and-pull-streams-using-httpclient.aspx) | [VS 2012 kaynak](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fUploadXDocumentSample%2fReadMe.txt)

Bir XDocument'e yüklemeyi gösteren bir **ApiController** kullanarak **PushStreamContent** ve **HttpClient**.

**Doğrulama örnek** | [VS 2010 kaynak](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fValidationSample%2fReadMe.txt)

Nasıl doğrulama öznitelikleri, ASP.NET Webapı modellerinde üzerinde HTTP isteğinin içeriğini doğrulamak için kullanabileceğiniz gösterir. Hem framework tanımlı kullanmayı ve özel doğrulama öznitelikleri modelinizde açıklama eklemek için gerektiği şekilde özellikleri işaretleyin ve hata yanıtları geçersiz model durumları için dönüş gösterir.

**Web formu örnek** | [ayrıntılı açıklama](https://blogs.msdn.com/b/henrikn/archive/2012/02/23/using-asp-net-web-api-with-asp-net-web-forms.aspx) | [VS 2010 kaynak](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fWebFormSample%2fReadMe.txt)

Web Forms projeye eklenen bir ApiController gösterir.

**[RestBugs örnek](https://github.com/howarddierking/RestBugs)**

RestBugs, ASP.NET Web API ve yeni HTTP istemci kitaplığı iletilir güdümlü bir sistem oluşturmak için nasıl kullanılacağını gösteren uygulama izleme basit bir hatadır. Örnek, ASP.NET Web API kullanarak, istemci ve sunucu uygulamaları içerir. Sunucu, kaynak ifadeleri oluşturmak için özel bir Razor biçimlendirici kullanır. Örnek de iletilir Tasarım'ı kullanarak istemcileri ve sunucuları ayırırsınız gelen avantajları göstermek için bir node.js sunucusu sağlar.

## <a name="web-api-extensions-preview-samples"></a>Web API Uzantıları Önizleme örnekleri

**OData sorgulanabilir örnek** | [ayrıntılı açıklama](https://blogs.msdn.com/b/alexj/archive/2012/08/15/odata-support-in-asp-net-web-api.aspx) | [VS 2010 kaynak](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fODataQueryableSample%2fReadMe.txt)

ASP.NET Web API kullanarak OData sorgularda tanıtmak gösterilmiştir `[Queryable]` kullanarak veya öznitelik **ODataQueryOptions** yürütülmekte olan önce sorgu el ile incelemek eylem sağlayan eylem parametresi.

[Queryable] özniteliğini kullanarak CustomerController gösterir ve OrderController ODataQueryOptions parametresinin nasıl kullanılacağını gösterir. ResponseController benzer CustomerController ancak döndürme alma işlemi yerine `IEnumerable<Customer>` döndürdüğü bir **httpresponsemessage öğesini**. Bu ek üstbilgi alanları eklemek için durum kodu, vb. hala sorgu işlevi kullanılırken işlemek için bize sağlar. Örnek, $orderby, $skip, $top, any(), all() ve $filter kullanarak sorguları gösterir.

**OData hizmet örneği** | [ayrıntılı açıklama](https://blogs.msdn.com/b/alexj/archive/2012/08/15/odata-support-in-asp-net-web-api.aspx) | [VS 2010 kaynak](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fODataServiceSample%2fReadMe.txt)

Bu örnek üç varlık ve üç Web API denetleyicisi oluşan bir OData hizmetin nasıl oluşturulacağını gösterir. Denetleyicileri işlevsellik ortaya OData işlevselliği açısından çeşitli düzeylerini sağlar:

SupplierController sorgu dahil olmak üzere işlevlerinin bir alt kümesini gösterir, bu istekleri işleyerek anahtarı ve oluşturma, tarafından alın:

- /Suppliers Al
- /Suppliers(KEY) Al
- GET /Suppliers? $filter =... &amp;$orderby =... &amp;$top =... &amp;$skip =...
- POST /Suppliers

ProductsController çıkarır Al koy sonrası SİLİN ve eylemi bu işlemlerin her biri için doğrudan uygulayarak düzeltme eki.

ProductFamilesController zengin bir OData hizmeti uygulamak için yararlı bir desen sunan EntitySetController temel sınıf yararlanır.

Ayrıca tüketilen veri sağlayan bir $metadata belgesini OData hizmeti sunan WCF veri hizmeti istemciler ve $metadata biçimi kabul diğer istemciler tarafından.
