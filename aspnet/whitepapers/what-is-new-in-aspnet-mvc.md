---
uid: whitepapers/what-is-new-in-aspnet-mvc
title: ASP.NET MVC 2 sürümündeki yenilikler | Microsoft Docs
author: rick-anderson
description: Bu belgede, yeni özellikler ve geliştirmeler, ASP.NET MVC 2'de sunulmuştur açıklanmaktadır. Bu belge ayrıca yükleme için kullanılabilir.
ms.author: aspnetcontent
ms.date: 04/20/2010
ms.assetid: 69a8d6f8-4b10-4602-8822-2d6c05fc432b
msc.legacyurl: /whitepapers/what-is-new-in-aspnet-mvc
msc.type: content
ms.openlocfilehash: 54f62b0f6bbde50f53d062eda422f5f58806cbe1
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37834170"
---
<a name="whats-new-in-aspnet-mvc-2"></a>ASP.NET MVC 2 sürümündeki yenilikler
====================
> Bu belgede, yeni özellikler ve geliştirmeler, ASP.NET MVC 2'de sunulmuştur açıklanmaktadır. Bu belge için de kullanılabilir olan [indirin](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/WhatIsNewInMVC_2.pdf)


[Giriş](#_TOC1)   
[ASP.NET MVC 2'için bir ASP.NET MVC 1.0 projesini yükseltme](#_TOC2)   
[Yeni Özellikler](#_TOC3)   
[Şablonlu Yardımcılar](#_TOC3_1)   
[Alanları](#_TOC3_2)   
[Zaman uyumsuz denetleyicileri için destek](#_TOC3_3)   
[DefaultValueAttribute eylem-yöntem parametreleri için destek](#_TOC3_4)   
[Model bağlayıcıları ile ikili veri bağlama desteği](#_TOC3_5)   
[ModelMetadata ve ModelMetadataProvider sınıfları](#_TOC3_6)   
[DataAnnotations öznitelikler için destek](#_TOC3_7)   
[Model Doğrulayıcı sağlayıcıları](#_TOC3_8)   
[İstemci tarafı doğrulama](#_TOC3_9)   
[Visual Studio 2010 için yeni kod parçacıkları](#_TOC3_10)   
[Yeni RequireHttpsAttribute eylem filtresi](#_TOC3_11)   
[HTTP yöntemi fiili geçersiz kılma](#_TOC3_12)   
[Şablonlu Yardımcılar için yeni HiddenInputAttribute sınıfı](#_TOC3_13)   
[Html.ValidationSummary yardımcı yöntem Model düzeyi hataları görüntüleyebilirsiniz.](#_TOC3_14)   
[T4 şablonları Visual Studio code'da oluşturmak özel olan .NET Framework hedef sürümü için](#_TOC3_15)[API geliştirmeleri](#_TOC4)  
[Bozucu değişiklikler](#_TOC5)  
[Sorumluluk reddi](#_TOC6)  

## <a id="_TOC1"></a>  Giriş

ASP.NET MVC 2, ASP.NET MVC 1.0 oluşturur ve çok sayıda geliştirmeleri ve üretkenliği artırmak odaklı özellikleri sunar. Bu sürüm, tüm Bilgi Bankası, yetenekler, kod ve ASP.NET MVC 1.0 için uzantıları uygulamaya devam etmek için ASP.NET MVC 1.0 ile uyumludur.

ASP.NET MVC hakkında daha fazla bilgi için aşağıdaki kaynakları ziyaret edin:

- [MSDN'de ASP.NET MVC belgeleri](https://go.microsoft.com/fwlink/?LinkId=159758)
- [ASP.NET MVC Web sitesi](https://asp.net/mvc/)
- [ASP.NET MVC forumları](https://forums.asp.net/1146.aspx)

## <a id="_TOC2"></a>  ASP.NET MVC 2'için bir ASP.NET MVC 1.0 projesini yükseltme

ASP.NET MVC 2, bir ASP.NET MVC 1.0 uygulamasını ASP.NET MVC 2 Yükseltme zamanı seçme içinde uygulama geliştiricilerin esneklik aynı sunucuda ASP.NET MVC 1.0 ile yan yana yüklenebilir. Yükseltme hakkında daha fazla bilgi için bkz [bir ASP.NET MVC 1.0 uygulamasını ASP.NET MVC 2 yükseltme](https://go.microsoft.com/fwlink/?LinkID=185459).

## <a id="_TOC3"></a>  Yeni Özellikler

Bu bölümde sunulan özellikler açıklanır MVC 2 sürümündeki.

### <a id="_TOC3_1"></a>  Şablonlu Yardımcılar

Şablonlu yardımcılar, düzenleme için otomatik olarak ilişkilendirme HTML öğeleri sağlar ve veri türleriyle görüntüler. Örneğin, tür System.DateTime Veri Görünümü'nde görüntülendiğinde, bir tarih seçici UI öğesi otomatik olarak oluşturulabilir. Bu, ASP.NET dinamik veri alanı şablonları nasıl çalıştığını için benzer. Daha fazla bilgi için [verileri görüntülemek için şablonlu Yardımcılar kullanarak](https://go.microsoft.com/fwlink/?LinkId=159062) MSDN Web sitesinde.

### <a id="_TOC3_2"></a>  Alanları

Alanları büyük bir Web uygulaması karmaşıklığını yönetmek için birden çok daha küçük bölümlere büyük bir projenin düzenlemenize olanak tanır. Her bölüm ("alanı"), genellikle büyük bir Web sitesi ayrı bir bölümünü temsil eder ve ilgili ayarlar denetleyicileri ve görünümlerinin gruplandırmak için kullanılır. Daha fazla bilgi için [izlenecek yol: bir ASP.NET MVC uygulaması alanları tarafından düzenleme](https://go.microsoft.com/fwlink/?LinkId=158978) MSDN Web sitesinde.

Çözüm Gezgini içinde yeni bir alan oluşturmak için projeye sağ tıklayın ve Ekle'yi tıklatın alan'ye tıklayın. Bu alan adı için soran bir iletişim kutusu görüntüler. Alan adı girdikten sonra Visual Studio projesine yeni bir alan ekler.

Aşağıdaki şekilde, yönetim ve bloglar iki alana sahip bir proje için bir örnek düzeni gösterilir.

![](what-is-new-in-aspnet-mvc/_static/image1.png)

Bir alanı oluşturduğunuzda, Visual Studio her alana AreaRegistration türeyen bir sınıf ekler. Bu sınıf, alan ve onun yollar kaydetmek için aşağıdaki örnekte gösterildiği gibi gereklidir:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample1.cs)]

ASP.NET MVC 2 için varsayılan proje şablonu Global.asax dosyası için kodu RegisterAllAreas yönteme bir çağrı içerir. Bu yöntem her alanı AreaRegistration sınıfından türetilen tüm türler için arayan, türün bir örneğini örnekleme ve RegisterArea çalışma metodunu örneğinde projede kaydeder. Aşağıdaki örnek nasıl yapıldığını gösterir.

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample2.cs)]

Ad alanı RegisterArea yöntemi bağlamı çağırarak belirtmezseniz. Namespaces.Add yöntemi, kayıt sınıfın ad alanı varsayılan olarak kullanılır.

### <a id="_TOC3_3"></a>  Zaman uyumsuz denetleyicileri için destek

ASP.NET MVC 2 denetleyicileri isteklerini zaman uyumsuz olarak işlemek artık izin verir. Bu performans kazancı elde edildi sık engelleyici olmayan ortaklarınıza yerine çağırmak için (örneğin, ağ istekleri) engelleme işlemleri çağırmak sunucuları vererek açabilir. Daha fazla bilgi için [ASP.NET MVC'de zaman uyumsuz bir denetleyiciyi kullanarak](https://msdn.microsoft.com/library/ee728598(v=VS.100).aspx) MSDN'de konu.

### <a id="_TOC3_4"></a>  DefaultValueAttribute eylem-yöntem parametreleri için destek

Bir eylem yöntemi için bağımsız değişken parametresi için sağlanan için varsayılan bir değer System.ComponentModel.DefaultValueAttribute sınıfı sağlar. Örneğin, aşağıdaki varsayılan rota tanımlandığını varsayar:

[!code-json[Main](what-is-new-in-aspnet-mvc/samples/sample3.json)]

Ayrıca aşağıdaki denetleyici ve eylem yöntemi tanımlandığını varsayar:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample4.cs)]

Aşağıdaki isteği birini URL'leri yukarıdaki örnekte tanımlanan görünüm eylem yöntemini çağırır.

- / Makale/görünüm/123
- / Makale/görünüm/123? sayfa = 1 (etkili bir şekilde önceki istek aynı)
- / Makale/görünüm/123? sayfa = 2

Sayfa değişkeni değeri sağlanmadı atanamayan değer türü olduğundan DefaultValueAttribute özniteliği olmadan önceki listede ilk URL, çalışmaz.

Kodunuzu Visual Basic 2010 veya Visual C# 2010'yazılmışsa, isteğe bağlı parametreler yerine DefaultValueAttribute öznitelik, aşağıdaki örnekte gösterildiği gibi kullanabilirsiniz:

[!code-vb[Main](what-is-new-in-aspnet-mvc/samples/sample5.vb)]

### <a id="_TOC3_5"></a>  Model bağlayıcıları ile ikili veri bağlama desteği

İkili değerleri base-64 kodlu dize olarak kodlama iki yeni aşırı Html.Hidden Yardımcısı'nın vardır:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample6.cs)]

Bir genel görünümünde bir nesne için bir zaman damgası eklemek için kullanılır. Örneğin, uygulamanız aşağıdaki ürün nesne içerebilir:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample7.cs)]

Düzenleme formu zaman damgası özelliği formunda aşağıdaki örnekte gösterildiği gibi işleyebilir:

[!code-aspx[Main](what-is-new-in-aspnet-mvc/samples/sample8.aspx)]

Bu işaretleme, aşağıdaki örneğe benzer base-64 kodlu bir dize olarak zaman damgası değere sahip gizli bir input öğesi oluşturur:

[!code-html[Main](what-is-new-in-aspnet-mvc/samples/sample9.html)]

Bu formu ürünü türünde bir bağımsız değişken olan bir eylem yöntemine aşağıdaki örnekte gösterildiği gibi gönderilen:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample10.cs)]

Eylem yöntemine gönderilen base-64 kodlanmış dizeyi bir bayt dizisine dönüştürülür çünkü zaman damgası özelliği doğru bir şekilde doldurulur.

### <a id="_TOC3_6"></a>  ModelMetadata ve ModelMetadataProvider sınıfları

ModelMetadataProvider sınıfı bir görünüm içindeki model için meta verileri almak için bir Özet sağlar. MVC 2 varsayılan sağlayıcı System.ComponentModel.DataAnnotations ad alanı öznitelikleri tarafından kullanıma sunulan meta verileri kullanıma sunduğu içerir. Veritabanları veya XML dosyaları gibi diğer veri depolarından meta verilerini sağlamak meta veri sağlayıcıları oluşturmak mümkündür.

ViewDataDictionary sınıfı modelden ModelMetadataProvider sınıfı tarafından ayıklanan meta veriler içeren bir ModelMetadata nesnesi gösterir. Bu, bu meta veriler kullanma ve çıktılarını uygun şekilde ayarlamak şablonlu Yardımcılar sağlar.

Daha fazla bilgi için belgelerine bakın [ModelMetadata](https://msdn.microsoft.com/library/system.web.mvc.modelmetadataprovider(VS.100).aspx) ve [ModelMetadataProvider](https://msdn.microsoft.com/library/system.web.mvc.modelmetadataprovider(VS.100).aspx) sınıfları.

### <a id="_TOC3_7"></a>  DataAnnotations öznitelikler için destek

ASP.NET MVC 2 destekler (System.ComponentModel.DataAnnotations ad alanında tanımlanan) RangeAttribute, RequiredAttribute StringLengthAttribute ve RegexAttribute doğrulama öznitelikleri kullanarak giriş sağlamak için bir model için bağladığınızda doğrulama.

Daha fazla bilgi için [nasıl yapılır: Model veri kullanarak DataAnnotations öznitelikleri doğrulama](https://go.microsoft.com/fwlink/?LinkId=159063) MSDN Web sitesinde. Bu öznitelikler kullanımını gösteren bir örnek proje adresten indirilebilir [ https://go.microsoft.com/fwlink/?LinkId=157753 ](https://go.microsoft.com/fwlink/?LinkId=157753).

### <a id="_TOC3_8"></a>  Model Doğrulayıcı sağlayıcıları

Model doğrulama sağlayıcı sınıf, model için doğrulama mantığı sağlar bir soyutlamayı temsil eder. ASP.NET MVC için varsayılan bir sağlayıcı System.ComponentModel.DataAnnotations ad alanında bulunan doğrulama öznitelikleri temel alan içerir. Modeline özel doğrulama kurallarını ve özel doğrulama kuralları eşlemelerini tanımlayan kendi doğrulama sağlayıcılarının da oluşturabilirsiniz. Daha fazla bilgi için belgelerine bakın [ModelValidatorProvider](https://msdn.microsoft.com/library/system.web.mvc.ModelValidatorProvider(VS.100).aspx) sınıfı.

### <a id="_TOC3_9"></a>  İstemci tarafı doğrulama

Model Doğrulayıcı sağlayıcı sınıfı, tarayıcının istemci tarafı doğrulama kitaplığı tarafından tüketilebilecek bir JSON ile seri hale getirilmiş veri biçiminde doğrulama meta verileri gösterir. ASP.NET MVC 2, istemci doğrulama kitaplığı ve daha önce not ettiğiniz DataAnnotations ad alanı doğrulama özniteliklerinin destekleyen bağdaştırıcısı içerir. Sağlayıcı sınıfı JSON verilerini ve diğer kitaplık çağrılarını işleyen bir bağdaştırıcı yazarak diğer istemci doğrulama kitaplıklarını kullanmanızı sağlar.

### <a id="_TOC3_10"></a>  Visual Studio 2010 için yeni kod parçacıkları

Bir HTML kod parçacıkları için ASP.NET MVC 2 ile Visual Studio 2010 yüklenir. Araçlar menüsünde bu kod parçacıklarının listesini görüntülemek için kod parçacıkları Yöneticisi'ni seçin. Dil için HTML seçin ve konum için ASP.NET MVC 2'yi seçin. Kod parçacıkları kullanma hakkında daha fazla bilgi için Visual Studio belgelerine bakın.

### <a id="_TOC3_11"></a>  Yeni RequireHttpsAttribute eylem filtresi

ASP.NET MVC 2, eylem yöntemleri ve denetleyicileri için uygulanan yeni bir RequireHttpsAttribute sınıfı içerir. Varsayılan olarak, filtre SSL etkin (HTTPS) eşdeğer bir SSL - HTTP isteği yönlendirir.

### <a id="_TOC3_12"></a>  HTTP yöntemi fiili geçersiz kılma

REST mimari stil kullanarak bir Web sitesi oluşturduğunuzda, HTTP fiilleri bir kaynak için gerçekleştirilecek eylemi belirlemek için kullanılır. REST uygulamaları dahil olmak üzere, ortak HTTP fiilleri tam aralığının Al koy, Gönder ve Sil desteği gerektirir.

ASP.NET MVC 2, eylem yöntemleri ve bu özellik kısaltılmış sözdizimi uygulanabilir yeni öznitelikler içerir. Bu öznitelikler, ASP.NET MVC, HTTP fiiline bağlı bir eylem yöntemini seçmek etkinleştirin. Aşağıdaki örnekte, bir POST isteği ilk eylem yöntemini çağırır ve bir PUT İsteği ikinci eylem yöntemini çağırır.

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample11.cs)]

ASP.NET MVC eski sürümleri, aşağıdaki örnekte gösterildiği gibi daha ayrıntılı sözdizimi, bu eylem yöntemleri gereklidir:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample12.cs)]

Tarayıcıları yalnızca GET ve sonrası HTTP fiilleri desteklediğinden, farklı bir eylem gerektiren bir eylem mümkün değildir. Bu nedenle tüm RESTful istekleri yerel olarak destekleyen mümkün değildir.

Ancak, RESTful desteklemek için istekleri gönderme işlemleri, ASP.NET MVC 2 sırasında tanıtır yeni HttpMethodOverride HTML yardımcı yöntemi. Bu yöntem herhangi bir HTTP yöntemini etkili bir şekilde benzetmek form neden olan gizli bir input öğesi oluşturur. Örneğin, HttpMethodOverride HTML yardımcı yöntemi kullanarak, bir form gönderimi görünür olabilir bir PUT ya da DELETE isteği. Aşağıdaki öznitelikler HttpMethodOverride davranışını etkiler:

- HttpPostAttribute
- HttpPutAttribute
- HttpGetAttribute
- HttpDeleteAttribute
- AcceptVerbsAttribute

Gizli bir input öğesi, X-HTTP-Method-Override adını ve değerini benzetmek için HTTP fiili için ayarlanmış ' var. Geçersiz kılma değeri de bir HTTP üst bilgisinde veya bir sorgu dizesi değeri bir ad/değer çifti olarak belirtilebilir.

Geçersiz kılma, yalnızca bir POST isteği gerçek istek olduğunda kullanılabilir. Geçersiz kılma değeri herhangi bir HTTP fiili kullanan istekleri yoksayılacak.

### <a id="_TOC3_13"></a>  Şablonlu Yardımcılar için yeni HiddenInputAttribute sınıfı

Gizli bir input öğesi modeli bir düzenleyici şablonunda görüntülenirken oluşturulması gerekip gerekmediğini belirtmek için bir model özelliğine yeni HiddenInputAttribute öznitelik uygulayabilirsiniz. (Öznitelik, bir örtük HiddenInput UIHint değeri ayarlar). Özniteliğin DisplayValue özellik değer Düzenleyicisi'nde görüntülenip görüntülenmeyeceğini belirtin ve görüntü modları sağlar. DisplayValue false olarak ayarlandığında, hiçbir şey görüntülenmez, normalde bir alanını çevreleyen HTML biçimlendirmeyi bile. DisplayValue için varsayılan değer doğrudur.

HiddenInputAttribute özniteliği aşağıdaki senaryolarda kullanabilirsiniz:

- Ne zaman bir görünüm kullanıcılar bir nesnenin Kimliğini düzenlemenize olanak tanır ve eski kimliği alır ve böylece denetleyiciye geri geçirilen gizli bir input öğesi sağlamak için değer de görüntülemek gereklidir.
- Bir görünüm sağlar, hiçbir zaman, bir zaman damgası özelliği gibi görüntülenmesi gereken bir ikili özellik kullanıcıları düzenleyebilirsiniz. Bu durumda, çevreleyen HTML biçimlendirmesi (örneğin, değer ve etiket) ve değer görüntülenmez.

Aşağıdaki örnek HiddenInputAttribute sınıfının nasıl kullanılacağını gösterir.

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample13.cs)]

Öznitelik ayarlandığında true (veya parametre belirtilmediğinde), aşağıdakiler gerçekleşir:

- Görüntüleme şablonları, bir etiket oluşturulur ve kullanıcıya değeri görüntülenir.
- Düzenleyici şablonları, bir etiket oluşturulur ve değerini gizli bir input öğesi içinde işlenir.

Öznitelik false olarak ayarlandığında, aşağıdakiler gerçekleşir:

- Bu alan için görüntüleme şablonları hiçbir şey işlenir.
- Düzenleyici şablonları herhangi bir etiket oluşturulur ve değerini gizli bir input öğesi içinde işlenir.

### <a id="_TOC3_14"></a>  Html.ValidationSummary yardımcı yöntem Model düzeyi hataları görüntüleyebilirsiniz.

Her zaman tüm doğrulama hatalarını yerine Html.ValidationSummary yardımcı yöntemi yalnızca model düzeyi hataları görüntülemek için yeni bir seçenek vardır. Bu, doğrulama özetinin görüntülenecek model düzeyi hataları ve her alanın yanındaki görüntülenecek alan özgü hataları sağlar.

### <a id="_TOC3_15"></a>  T4 şablonları Visual Studio code'da oluşturmak özel olan .NET Framework hedef sürümü için

Yeni bir özellik için T4 dosyaları uygulama tarafından kullanılan .NET Framework sürümünü belirten bir ASP.NET MVC T4 ana bilgisayardan kullanılabilir. Bu kod ve .NET Framework sürümüne özeldir biçimlendirme oluşturmak T4 şablonları sağlar. Visual Studio 2008'de her zaman .NET 3.5 değerdir. Visual Studio 2010'da .NET 3.5 ya da .NET 4 değerdir.

## <a id="_TOC4"></a>  API geliştirmeleri

Bu bölümde, mevcut bir ASP.NET MVC türleri ve üyeleri değişiklikler açıklanmaktadır.

- Korumalı bir sanal CreateActionInvoker yöntemi Controller sınıfında eklendi. Bu yöntem, denetleyici ActionInvoker özelliği tarafından çağrılır ve yavaş çağırıcı örneklemesi için hiçbir Başlatıcı zaten ayarlanmışsa sağlar.
- Korumalı bir sanal HandleUnauthorizedRequest yöntem AuthorizeAttribute sınıfı eklendi. Bu yetkilendirme başarısız olduğunda davranışını denetlemek için AuthorizeAttribute öğesinden türetilen filtreler sağlar.
- Add (dize anahtar, nesne değeri) metodu ValueProviderDictionary sınıfı eklendi. Bu sözlük Başlatıcısı sözdizimi ValueProviderDictionary için aşağıdaki örnekte olduğu gibi kullanmanıza olanak sağlar:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample14.cs)]

- Bir get eklenen\_nesne Sys.Mvc.AjaxContext sınıfında yöntemi. Get için benzer bir JavaScript yöntemini budur\_get veri yöntemi, yanıtın içerik türü application/json olup olmadığını ancak\_nesnesini JSON nesnesi döndürür.
- AuthorizationContext sınıfında ActionDescriptor'özelliği eklendi.
- Eklenen özellik eksik olduğunda, bir kimlik özelliği içerdiğinden bir model bağlama sırasında sorunlarına geçici çözümler bulmak için kullanılan bir UrlParameter.Optional belirteç formu gönderisinde. Daha fazla ayrıntı için bkz: Giriş [ASP.NET MVC 2 isteğe bağlı URL parametrelerini](http://haacked.com/archive/2010/02/12/asp-net-mvc-2-optional-url-parameters.aspx) Phil Haack'ın blogunda.

## <a id="_TOC5"></a>  Bozucu değişiklikler

Aşağıdaki değişiklikler, mevcut bir ASP.NET MVC 1.0 uygulamalarda hatalara neden olabilir.

#### <a name="change-in-property-validation-behavior-for-classes-that-implement-idataerrorinfo"></a>IDataErrorInfo uygulayan sınıflar için özellik doğrulama davranışını değiştir

IDataErrorInfo doğrulama gerçekleştirmek için kullandığınız model nesneleri için yeni bir değer kümesi bağımsız olarak her özelliğin doğrulanır. ASP.NET MVC 1.0, yeni değerleri olan özellikler doğrulandı. Tüm özellik doğrulayıcıları başarılı olursa ASP.NET MVC 2'de IDataErrorInfo hata özelliği adı verilir.

#### <a name="iis-script-mapping-script-is-no-longer-available-in-the-installer"></a>IIS komut dosyası eşleme komut dosyası yükleyici artık kullanılamıyor

IIS komut dosyası eşleme komut betik eşlemeleri Klasik modda IIS 6 ve IIS 7 için yapılandırmak için kullanılan bir komut satırı komut dosyasıdır. Visual Studio geliştirme sunucusu kullanıyorsanız veya tümleşik modda IIS 7 kullanıyorsanız, komut dosyası eşleme betiği gerekli değildir. Komut dosyalarını, desteklenmeyen ayrı bir indirme olarak kullanılabilir [ASP.NET WebStack](https://github.com/aspnet/AspNetWebStack).

#### <a name="the-htmlsubstitute-helper-method-in-mvc-futures-is-no-longer-available"></a>MVC vadeli Html.Substitute yardımcı yönteminin artık kullanılamıyor

MVC görünüm altyapıları işleme davranışını değişiklikleri nedeniyle Html.Substitute yardımcı yöntem çalışmaz ve kaldırıldı.

#### <a name="the-ivalueprovider-interface-replaces-all-uses-of-idictionary"></a>IDictionary tüm kullanımları IValueProvider arabirimi değiştirir

IDictionary MVC 1.0 artık kabul her bir özellik veya yöntem bağımsız IValueProvider kabul eder. Bu değişiklik, yalnızca özel değer sağlayıcıları ya da özel model bağlayıcılarını içeren uygulamaları etkiler. Özellikler ve yöntemler bu değişiklikten etkilenen örnekleri şunları içerir:

- ControllerBase ve ModelBindingContext sınıfları ValueProvider özelliği.
- Denetleyici sınıfının TryUpdateModel yöntemleri.

#### <a name="new-css-classes-were-added-in-the-sitecss-file"></a>Site.css dosyasında yeni CSS sınıfları eklendi

ASP.NET MVC proje şablonları Site.css dosyasında doğrulama işlevleri ve şablonlu Yardımcılar tarafından kullanılan yeni stiller içerecek şekilde güncelleştirildi.

#### <a name="helpers-now-return-an-mvchtmlstring-object"></a>Yardımcıları artık MvcHtmlString nesneyi döndürür

ASP.NET 4'te yeni HTML kodlaması ifade sözdizimi yararlanmak için dönüş türü için HTML Yardımcıları MvcHtmlString bir dize yerine sunulmuştur. ASP.NET MVC 2 ve yeni Yardımcıları ASP.NET 3.5 kullanırsanız, HTML kodlaması söz dizimi yararlanmak mümkün olmayacaktır; Yeni sözdizimi yalnızca, ASP.NET 4'te ASP.NET MVC 2'ı çalıştırdığınızda kullanılabilir.

#### <a name="jsonresult-now-responds-only-to-http-post-requests"></a>JsonResult artık yalnızca HTTP POST isteklerini yanıtlar

Varsayılan olarak, bilgilerin açıklanması, olası sahip saldırıları ele geçirme JSON azaltmak için JsonResult sınıfı yalnızca HTTP POST istekleri yanıtlar. AJAX almak bir JsonResult nesnesi döndüren eylem yöntemleri çağrısına POST kullanacak şekilde değiştirilmelidir. Gerekirse, yeni JsonResult JsonRequestBehavior özelliğini ayarlayarak bu davranışı geçersiz kılabilirsiniz. Olası yararlanma hakkında daha fazla bilgi için blog gönderisine bakın [JSON ele geçirme](http://haacked.com/archive/2009/06/25/json-hijacking.aspx) Phil Haack'ın blogunda.

#### <a name="model-and-modeltype-property-setters-on-modelbindingcontext-are-obsolete"></a>Model ve ModelType özellik ayarlayıcılarına ModelBindingContext üzerinde geçersizdir

Yeni bir ayarlanabilir ModelMetadata özelliği ModelBindingContext sınıfı eklendi. Yeni özellik, modelin hem ModelType özelliklerini kapsüller. Geriye dönük uyumluluk için Model ve ModelType özellikleri artık kullanılmıyor olsa da, özellik alıcıları hala çalışır; Bunlar ModelMetadata özelliğin değerini almak için temsilci.

#### <a name="changes-to-the-defaultcontrollerfactory-class-break-custom-controller-factories-that-derive-from-it"></a>Değişiklikleri DefaultControllerFactory sınıf türetmeniz özel denetleyici üreteçlerini Kes

RequestContext özelliği kaldırarak DefaultControllerFactory sınıfı düzeltildi. Bu özellik yerine istek bağlamı örneği sanal korumalı GetControllerInstance ve GetControllerType yöntemler geçirilir. Bu değişiklik DefaultControllerFactory türetilen özel denetleyici üreteçlerini etkiler.

Özel denetleyici üreteçlerini, genellikle bağımlılık ekleme için ASP.NET MVC uygulamaları sağlamak için kullanılır. ASP.NET MVC 2 desteklemek için özel denetleyici üreteçlerini güncelleştirmek için yöntem imzası veya imzaları yeni imzalar eşleşecek şekilde değiştirin ve isteği bağlam parametresi yerine özelliğini kullanın.

#### <a name="area-is-a-now-a-reserved-route-value-key"></a>"Alanı" artık ayrılmış yol değeri bir anahtardır

Rota değerleri dize "alanı", "controller" ve "action" yolla ASP.NET MVC'de özel anlamı artık sahiptir. Bir olduğu çıkarımında HTML Yardımcıları "alanı" içeren bir rota değeri sözlüğü ile sağlanırsa, Yardımcıları artık "alanı" sorgu dizesinde ekleyeceği emin olur.

Alanlar özelliğini kullanıyorsanız, {alan} rota URL'nizi bir parçası olarak kullanmayı emin olun.


## <a id="_TOC6"></a>  Sorumluluk reddi

Bu, Başlangıç belgesi ve burada açıklanan yazılım son ticari sürümünden önce önemli ölçüde değiştirilebilir.

Bu belgede yer alan bilgileri, Microsoft Corporation'ın geçerli görünümü tarih itibariyle doğrudur ele alınan sorunlar temsil eder. Microsoft'un değişen piyasa koşullarına yanıt vermesi gerekir çünkü Microsoft tarafında taahhüdü olarak yorumlanmamalıdır ve Microsoft yayın tarihinden sonra sunulan bilgilerin doğruluğunu garanti edemez.

Bu teknik incelemeyi yalnızca bilgilendirme amaçlıdır. MICROSOFT HİÇBİR EXPRESS, ZIMNİ VEYA YASAL BU BELGEDEKİ BİLGİLER GARANTİDE BULUNMAZ.

İlgili tüm telif hakkı yasalara uymak kullanıcının sorumluluğundadır. Telif Hakkı altındaki sınırlamaksızın, bu belgenin hiçbir bölümü çoğaltılamaz, depolanan veya bir sistemde saklanamaz ya da herhangi bir yolla (elektronik, mekanik, fotokopi, kaydı veya diğer) veya herhangi bir amaçla, Microsoft Corporation'ın izni hızlı yazılır.

Microsoft'un patentleri, patent başvuruları, ticari markaları, telif hakları veya diğer fikri mülkiyet hakları bu belgedeki konuyla ilgili olabilir. Microsoft'tan bir yazılı lisans anlaşmasında bu belgenin bulundurmak, herhangi bir lisans bu patentlere, ticari markaları, telif hakları veya diğer fikri mülkiyet sağlamazsa açıkça belirtilmediği sürece.

Aksi belirtilmediği sürece, örnek şirketler, kuruluşlar, ürünler, etki alanı adları, e-posta adresleri, logolar, kişiler, yerler ve olaylar burada olduğunu hayal ve tüm gerçek şirket, kuruluş, ürün, etki alanı adı, e-posta ile hiçbir ilişki adı geçen adresi, logo, kişi, yer veya olay amaçlanmamıştır veya çıkarılmamalıdır.

© 2010 Microsoft Corporation. Tüm hakları saklıdır.

Microsoft ve Windows tescilli veya Amerika Birleşik Devletleri ve/veya diğer ülkelerde Microsoft Corporation'ın ticari olan.

Burada sözü edilen gerçek şirketler ve ürünler ticari markalar kendi sahiplerinin olabilir.
