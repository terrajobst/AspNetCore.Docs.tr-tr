---
uid: whitepapers/what-is-new-in-aspnet-mvc
title: ASP.NET MVC 2 yenilikler | Microsoft Docs
author: rick-anderson
description: "Bu belgede, yeni özellikler ve geliştirmeler ASP.NET MVC 2'de tanıtılan açıklanmaktadır. Bu belge da indirme için kullanılabilir."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/20/2010
ms.topic: article
ms.assetid: 69a8d6f8-4b10-4602-8822-2d6c05fc432b
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/what-is-new-in-aspnet-mvc
msc.type: content
ms.openlocfilehash: 808f51b48b31e21848d76e7ded436ca1b17901d2
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2018
---
<a name="whats-new-in-aspnet-mvc-2"></a>ASP.NET MVC 2 yenilikler nelerdir?
====================
> Bu belgede, yeni özellikler ve geliştirmeler ASP.NET MVC 2'de tanıtılan açıklanmaktadır. Bu belgede de kullanılabilir [indirin](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/WhatIsNewInMVC_2.pdf)


[Giriş](#_TOC1)   
[ASP.NET MVC 2 bir ASP.NET MVC 1,0 proje yükseltme](#_TOC2)   
[Yeni Özellikler](#_TOC3)   
[Şablonlu Yardımcılar](#_TOC3_1)   
[Alanları](#_TOC3_2)   
[Zaman uyumsuz denetleyicileri için destek](#_TOC3_3)   
[Eylem-yöntem parametrelerini DefaultValueAttribute desteği](#_TOC3_4)   
[Model bağlayıcıları ile ikili veri bağlama için destek](#_TOC3_5)   
[ModelMetadata ve ModelMetadataProvider sınıfları](#_TOC3_6)   
[DataAnnotations öznitelikleri için destek](#_TOC3_7)   
[Model Doğrulayıcı sağlayıcıları](#_TOC3_8)   
[İstemci tarafı doğrulama](#_TOC3_9)   
[Visual Studio 2010 için yeni kod parçacıkları](#_TOC3_10)   
[Yeni RequireHttpsAttribute eylem filtresi](#_TOC3_11)   
[HTTP yöntemini fiil geçersiz kılma](#_TOC3_12)   
[Şablonlu Yardımcılar için yeni HiddenInputAttribute sınıfı](#_TOC3_13)   
[Model düzeyi hataları görüntüleme Html.ValidationSummary yardımcı yöntemi](#_TOC3_14)   
[Visual Studio özel olan kodu oluştur T4 şablonlarında .NET Framework'ün hedef sürümüne](#_TOC3_15)[API geliştirmeleri](#_TOC4)  
[Yeni değişiklikler](#_TOC5)  
[Sorumluluk reddi](#_TOC6)  

## <a id="_TOC1"></a>Giriş

ASP.NET MVC 2 ASP.NET MVC 1.0 oluşturur ve çok sayıda geliştirmeleri ve verimliliği artırma odaklı özellikleri sunar. Tüm Bilgi Bankası, becerileri, kod ve ASP.NET MVC 1.0 için Uzantılar uygulamaya devam etmek için bu sürümde ASP.NET MVC 1.0 ile uyumludur.

ASP.NET MVC hakkında daha fazla bilgi için aşağıdaki kaynaklara ziyaret edin:

- [MSDN'deki ASP.NET MVC belgelere](https://go.microsoft.com/fwlink/?LinkId=159758)
- [ASP.NET MVC Web sitesi](https://asp.net/mvc/)
- [ASP.NET MVC forumları](https://forums.asp.net/1146.aspx)

## <a id="_TOC2"></a>ASP.NET MVC 2 bir ASP.NET MVC 1,0 proje yükseltme

ASP.NET MVC 2 ASP.NET MVC 2 ASP.NET MVC 1.0 uygulamaya yükseltme zamanı seçerken uygulama geliştiricilerin esneklik aynı sunucuda ASP.NET MVC 1. 0'yan yana yüklenebilir. Belge yükseltme hakkında daha fazla bilgi için bkz [bir ASP.NET MVC 1.0 uygulaması için ASP.NET MVC 2 yükseltme](https://go.microsoft.com/fwlink/?LinkID=185459).

## <a id="_TOC3"></a>Yeni Özellikler

Bu bölümde tanıtılmıştır özellikleri açıklanmıştır MVC 2 sürümünde.

### <a id="_TOC3_1"></a>Şablonlu Yardımcılar

Şablonlu Yardımcılar veri türleriyle görüntüleme ve düzenleme için otomatik olarak ilişkilendirmek HTML öğeleri sağlar. Örneğin, veri türü System.DateTime bir görünümde görüntülendiğinde, tarih seçici UI öğesi otomatik olarak oluşturulabilir. Bu alan şablonları ASP.NET dinamik veri nasıl benzer. Daha fazla bilgi için bkz: [kullanarak verileri görüntülemek için şablonlu Yardımcılar](https://go.microsoft.com/fwlink/?LinkId=159062) MSDN Web sitesinde.

### <a id="_TOC3_2"></a>Alanları

Alanları büyük bir Web uygulaması karmaşıklığını yönetmek için birden çok daha küçük bölümlere büyük bir proje düzenlemenize olanak tanır. Her bölümde ("alanı") genellikle büyük bir Web sitesi ayrı bir bölümünü temsil eder ve ilgili ayarlar denetleyicilerinin ve görünümlerin gruplandırmak için kullanılır. Daha fazla bilgi için bkz: [izlenecek yol: bir ASP.NET MVC uygulaması tarafından alanları düzenleme](https://go.microsoft.com/fwlink/?LinkId=158978) MSDN Web sitesinde.

Çözüm Gezgini'nde yeni bir alan oluşturmak için projeye sağ tıklayın, Ekle ve alan'ı tıklatın. Bu alan adını isteyen bir iletişim kutusu görüntüler. Alan adı girdikten sonra Visual Studio projesi için yeni bir alan ekler.

Aşağıdaki şekilde, yönetici ve bloglar iki alana sahip bir proje için bir örnek düzeni gösterilir.

![](what-is-new-in-aspnet-mvc/_static/image1.png)

Bir alanı oluşturduğunuzda, Visual Studio için her alan AreaRegistration türeyen bir sınıf ekler. Bu sınıf, alan ve onun yollar kaydetmek için aşağıdaki örnekte gösterildiği gibi gereklidir:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample1.cs)]

ASP.NET MVC 2 için varsayılan proje şablonu Global.asax dosyası için kod RegisterAllAreas yöntemine bir çağrı içerir. Bu yöntem her bir alan AreaRegistration sınıfından türetilen tüm türler için arayan, türünün bir örneğini oluşturarak ve ardından RegisterArea yöntemi örneğinde çağırarak tarafından projede kaydeder. Aşağıdaki örnekte, bunun nasıl yapılacağı gösterilmektedir.

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample2.cs)]

Ad alanı RegisterArea yönteminde bağlam çağırarak belirtmezseniz. Namespaces.Add yöntemi, kayıt sınıfı ad alanı, varsayılan olarak kullanılır.

### <a id="_TOC3_3"></a>Zaman uyumsuz denetleyicileri için destek

ASP.NET MVC 2 isteklerini zaman uyumsuz olarak işlemek denetleyicileri artık izin verir. Sık engellemeyen ortaklarınıza yerine çağırmak için (örneğin, ağ istekleri) engelleme işlemlerini çağırma sunucuları izin vererek bu performans artışları için yol açabilir. Daha fazla bilgi için bkz: [ASP.NET MVC uygulamasında zaman uyumsuz bir denetleyiciyi kullanarak](https://msdn.microsoft.com/library/ee728598(v=VS.100).aspx) MSDN'de konu.

### <a id="_TOC3_4"></a>Eylem-yöntem parametrelerini DefaultValueAttribute desteği

The System.ComponentModel.DefaultValueAttribute class allows a default value to be supplied for the argument parameter to an action method. Örneğin, aşağıdaki varsayılan yol tanımlanır varsayın:

[!code-json[Main](what-is-new-in-aspnet-mvc/samples/sample3.json)]

Ayrıca aşağıdaki denetleyici ve eylem yöntemi tanımlanır varsayın:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample4.cs)]

Aşağıdaki isteği hiçbirini URL'leri yukarıdaki örnekte tanımlanan görünüm eylem yöntemi çağırır.

- / Makale/görünüm/123
- / Makale/görünüm/123? sayfa = 1 (etkin önceki istek aynı)
- /Article/View/123?page=2

Sayfa bağımsız değişken değeri sağlanmamış bir null değer türü olduğundan DefaultValueAttribute öznitelik olmadan, yukarıdaki listede ilk URL, işe.

Kodunuzu Visual Basic 2010 veya Visual C# 2010 yazılmışsa, aşağıdaki örnekte gösterildiği gibi isteğe bağlı parametreler DefaultValueAttribute özniteliği yerine kullanabilirsiniz:

[!code-vb[Main](what-is-new-in-aspnet-mvc/samples/sample5.vb)]

### <a id="_TOC3_5"></a>Model bağlayıcıları ile ikili veri bağlama için destek

İkili değerler base-64 olarak kodlanmış dize olarak kodlamak iki yeni aşırı Html.Hidden Yardımcısı, vardır:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample6.cs)]

Bir genel görünümünde bir nesne için bir zaman damgası katıştırmak için kullanılır. Örneğin, uygulamanızın aşağıdaki ürün nesnesi şunlar olabilir:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample7.cs)]

Bir düzen formu zaman damgası özelliği aşağıdaki örnekte gösterildiği gibi bu formda işleyebilir:

[!code-aspx[Main](what-is-new-in-aspnet-mvc/samples/sample8.aspx)]

Bu biçimlendirme, aşağıdaki örneğe benzer base-64 olarak kodlanmış bir dize olarak zaman damgası değere sahip gizli bir input öğesi oluşturur:

[!code-html[Main](what-is-new-in-aspnet-mvc/samples/sample9.html)]

Bu form ürün türünde bir bağımsız değişken sahip bir eylem yöntemi aşağıdaki örnekte gösterildiği gibi gönderilen:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample10.cs)]

Gönderilen base-64 olarak kodlanmış dize bir bayt dizisine dönüştürülür çünkü eylem yönteminde zaman damgası özelliği doğru olarak doldurulur.

### <a id="_TOC3_6"></a>ModelMetadata ve ModelMetadataProvider sınıfları

ModelMetadataProvider sınıfı bir görünüm içindeki model meta verilerini almak için bir Özet sağlar. MVC 2 System.ComponentModel.DataAnnotations ad alanındaki öznitelikler tarafından kullanıma sunulan meta veriler kullanılabilir hale getirir ve varsayılan bir sağlayıcı içerir. Veritabanları veya XML dosyaları gibi diğer veri depolarına meta verilerini sağlamak meta veri sağlayıcılar oluşturmak mümkündür.

ViewDataDictionary sınıfı modelden ModelMetadataProvider sınıfı tarafından ayıklanan meta verileri içeren bir ModelMetadata nesnesi kullanıma sunar. Bu meta verileri kullanabilir ve bunların çıktısı buna uygun olarak ayarlamak şablonlu Yardımcılar sağlar.

Daha fazla bilgi için belgelerine bakın [ModelMetadata](https://msdn.microsoft.com/library/system.web.mvc.modelmetadataprovider(VS.100).aspx) ve [ModelMetadataProvider](https://msdn.microsoft.com/library/system.web.mvc.modelmetadataprovider(VS.100).aspx) sınıfları.

### <a id="_TOC3_7"></a>DataAnnotations öznitelikleri için destek

ASP.NET MVC 2 destekler (System.ComponentModel.DataAnnotations ad alanında tanımlı) RangeAttribute, RequiredAttribute, StringLengthAttribute ve RegexAttribute doğrulama öznitelikleri kullanarak giriş sağlamak için bir model bağladığınızda doğrulama.

Daha fazla bilgi için bkz: [nasıl yapılır: Model verileri kullanarak DataAnnotations özniteliklerini doğrula](https://go.microsoft.com/fwlink/?LinkId=159063) MSDN Web sitesinde. Bu öznitelikler kullanımını gösteren bir örnek proje adresten indirilebilir [https://go.microsoft.com/fwlink/?LinkId=157753](https://go.microsoft.com/fwlink/?LinkId=157753).

### <a id="_TOC3_8"></a>Model Doğrulayıcı sağlayıcıları

Model doğrulama sağlayıcısı sınıfı model için doğrulama mantığını sağlayan bir Özet gösterir. ASP.NET MVC System.ComponentModel.DataAnnotations ad alanında yer alan doğrulama özniteliklerini temel alarak bir varsayılan sağlayıcı içerir. Özel doğrulama kuralları ve özel doğrulama kuralları eşlemelerini modele tanımlayın kendi doğrulama sağlayıcılarının de oluşturabilirsiniz. Daha fazla bilgi için belgelerine bakın [ModelValidatorProvider](https://msdn.microsoft.com/library/system.web.mvc.ModelValidatorProvider(VS.100).aspx) sınıfı.

### <a id="_TOC3_9"></a>İstemci tarafı doğrulama

Model Doğrulayıcı sağlayıcı sınıfı doğrulama meta verileri JSON seri hale getirilmiş ve istemci tarafı doğrulama kitaplığı tarafından tüketilen veri biçiminde tarayıcıya gösterir. ASP.NET MVC 2 bir istemci doğrulama kitaplığını ve daha önce not ettiğiniz DataAnnotations ad alanı doğrulama öznitelikleri destekleyen bağdaştırıcısı içerir. Sağlayıcı sınıfı JSON verilerini ve alternatif kitaplığı çağrılarını işleyen bir bağdaştırıcı yazarak diğer istemci doğrulama kitaplıklarını kullanmanızı sağlar.

### <a id="_TOC3_10"></a>Visual Studio 2010 için yeni kod parçacıkları

HTML kod parçacıkları ASP.NET MVC 2 için bir dizi Visual Studio 2010 ile yüklenir. Araçlar menüsünde bu parçacıkları listesini görüntülemek için kod parçacıkları Yöneticisi'ni seçin. Dil için HTML seçin ve konum için ASP.NET MVC 2'yi seçin. Kod parçacıkları kullanma hakkında daha fazla bilgi için Visual Studio belgelerine bakın.

### <a id="_TOC3_11"></a>Yeni RequireHttpsAttribute eylem filtresi

ASP.NET MVC 2 eylem yöntemleri ve denetleyicileri uygulanan yeni bir RequireHttpsAttribute sınıfı içerir. Varsayılan olarak, filtre SSL - HTTP isteği için SSL etkin (HTTPS) eşdeğer yönlendirir.

### <a id="_TOC3_12"></a>HTTP yöntemini fiil geçersiz kılma

REST mimari stil kullanarak bir Web sitesi oluşturduğunuzda, HTTP fiilleri bir kaynak için gerçekleştirilecek eylemi belirlemek için kullanılır. REST dahil olmak üzere, ortak HTTP fiilleri tam aralığını Al koy, POST ve silme uygulamaları destekleyen gerektirir.

ASP.NET MVC 2 eylem yöntemleri ve bu özellik kısaltılmış sözdizimi uygulayabilirsiniz yeni öznitelikler içerir. Bu öznitelikler HTTP fiiline bağlı bir eylem yöntemini seçmek ASP.NET MVC etkinleştirin. Aşağıdaki örnekte, bir POST isteği ilk eylem yöntemini çağırır ve bir PUT İsteği ikinci eylem yöntemini çağırır.

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample11.cs)]

ASP.NET MVC önceki sürümlerinde, bu eylem yöntemleri aşağıdaki örnekte gösterildiği gibi daha ayrıntılı sözdizimi gereklidir:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample12.cs)]

Yalnızca GET ve POST HTTP fiilleri tarayıcılar desteklediği için farklı bir fiil gerektiren bir eylem göndermek mümkün değildir. Bu nedenle tüm RESTful istekleri yerel olarak desteklemesi olası değil.

Ancak, RESTful destek istekleri gönderme işlemleri, ASP.NET MVC 2 sırasında tanıtır yeni bir HttpMethodOverride HTML yardımcı yöntemi. Bu yöntem herhangi bir HTTP yöntemini etkili bir şekilde benzetmek form neden olan gizli bir input öğesi oluşturur. Örneğin, HttpMethodOverride HTML yardımcı yöntemi kullanarak, bir form gönderme görünür olabilir PUT veya silme isteği olabilir. HttpMethodOverride davranışını aşağıdaki öznitelikleri etkiler:

- HttpPostAttribute
- HttpPutAttribute
- HttpGetAttribute
- HttpDeleteAttribute
- AcceptVerbsAttribute

Gizli bir input öğesi, X-HTTP-Method-Override adını ve değerini benzetmek için HTTP fiili ayarlamak sahiptir. Geçersiz kılma değeri da bir HTTP üstbilgisi veya bir sorgu dizesi değeri bir ad/değer çifti olarak belirtilebilir.

Geçersiz kılma, yalnızca bir POST isteği gerçek istek olduğunda kullanılabilir. Geçersiz kılma değeri başka bir HTTP fiili kullanan istekleri için yoksayılacak.

### <a id="_TOC3_13"></a>Şablonlu Yardımcılar için yeni HiddenInputAttribute sınıfı

Gizli bir input öğesi model Düzenleyicisi şablonunda görüntülenirken oluşturulması gerekip gerekmediğini belirtmek için bir model özelliğine yeni HiddenInputAttribute öznitelik uygulayabilirsiniz. (Öznitelik HiddenInput örtük bir UIHint değerini ayarlar). Özniteliğin DisplayValue özellik değeri düzenleyicisinde görüntülenip görüntülenmeyeceğini belirtin ve görüntü modları olanak sağlar. DisplayValue false olarak ayarlandığında, hiçbir şey görünmez, normalde bir alanını çevreleyen HTML biçimlendirmesi bile. DisplayValue için varsayılan değer doğrudur.

HiddenInputAttribute özniteliği aşağıdaki senaryolarda kullanabilirsiniz:

- Ne zaman kullanıcıları bir nesnenin Kimliğini Düzenle bir görünüm sağlar ve eski kimliği içerir ve böylece denetleyiciye geri geçirilen gizli bir input öğesi sağlamak için değer de görüntülemek gereklidir.
- Bir görünüm izin verdiğinde kullanıcıların hiçbir zaman, zaman damgası özelliği gibi görüntülenmesi gereken ikili bir özelliğini düzenleyin. Bu durumda (örneğin, değer ve etiket) çevresindeki HTML biçimlendirmesi ve değer görüntülenmez.

Aşağıdaki örnek HiddenInputAttribute sınıfının nasıl kullanılacağını gösterir.

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample13.cs)]

Öznitelik ayarlandığında true (veya hiçbir parametre belirtilen), aşağıdakiler gerçekleşir:

- Görüntüleme şablonları, bir etiket oluşturulur ve değeri kullanıcıya görüntülenir.
- Düzenleyici şablonlarındaki bir etiket oluşturulur ve değeri gizli bir input öğesi içinde işlenir.

Özniteliği false olarak ayarlandığında, aşağıdakiler gerçekleşir:

- Görüntü şablonlarında hiçbir şey bu alan için işlenir.
- Düzenleyici şablonlarındaki hiçbir etiket oluşturulur ve değeri gizli bir input öğesi içinde işlenir.

### <a id="_TOC3_14"></a>Model düzeyi hataları görüntüleme Html.ValidationSummary yardımcı yöntemi

Her zaman tüm doğrulama hataları yerine Html.ValidationSummary yardımcı yöntemi yalnızca model düzeyi hataları görüntülemek için yeni bir seçenek vardır. Bu doğrulama özetinin görüntülenecek model düzeyi hataları ve her alanın yanındaki görüntülenecek alan özgü hataları sağlar.

### <a id="_TOC3_15"></a>Visual Studio özel olan kodu oluştur T4 şablonlarında .NET Framework'ün hedef sürüm

Yeni bir özellik için T4 dosyalarını uygulama tarafından kullanılan .NET Framework sürümünü belirten bir ASP.NET MVC T4 ana bilgisayardan kullanılabilir. Bu kod ve .NET Framework sürümü için belirli biçimlendirme oluşturmak T4 şablonları sağlar. Visual Studio 2008'de her zaman .NET 3.5 değeridir. Visual Studio 2010'da, .NET 3.5 veya .NET 4 değerdir.

## <a id="_TOC4"></a>API geliştirmeleri

Bu bölümde, mevcut ASP.NET MVC türleri ve üyeleri değişiklikler açıklanmaktadır.

- Korumalı bir sanal CreateActionInvoker yöntemi Controller sınıfında eklendi. Bu yöntem denetleyicisi ActionInvoker özelliği tarafından çağrılır ve çağırıcı yavaş örnek oluşturma için hiç çağırıcı zaten ayarladıysanız sağlar.
- Korumalı bir sanal HandleUnauthorizedRequest yöntem AuthorizeAttribute sınıfında eklendi. Yetkilendirme başarısız olduğunda davranışını denetlemek için AuthorizeAttribute öğesinden türetilen filtreler sağlar.
- Add (dize anahtar, nesne değeri) yöntemini ValueProviderDictionary sınıfında eklendi. Bu sözlük Başlatıcısı sözdizimi ValueProviderDictionary için aşağıdaki örnekte olduğu gibi kullanmanıza olanak sağlar:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample14.cs)]

- Get eklenen\_nesne Sys.Mvc.AjaxContext sınıfında yöntemi. Get için benzer bir JavaScript yöntemini budur\_alma yanıtın içerik türü application/json, şeklindedir, ancak veri yöntemi\_nesne JSON nesnesi döndürür.
- Bir ActionDescriptor'özelliği yetkilendirme bağlamı sınıfında eklendi.
- Özelliği mevcut olduğunda bir kimlik özelliği içerdiğinden model bağlama sırasında sorunları çözmek çalışmak için kullanılan bir UrlParameter.Optional belirteci eklenen da form post. Daha ayrıntılı bilgi için bkz: Giriş [ASP.NET MVC 2 isteğe bağlı URL parametreleri](http://haacked.com/archive/2010/02/12/asp-net-mvc-2-optional-url-parameters.aspx) Phil Haack'ın blogunda.

## <a id="_TOC5"></a>Yeni değişiklikler

Aşağıdaki değişiklikler mevcut ASP.NET MVC 1.0 uygulamalarda hatalara neden olabilir.

#### <a name="change-in-property-validation-behavior-for-classes-that-implement-idataerrorinfo"></a>IDataErrorInfo uygulayan sınıflar için özellik doğrulama davranışını değiştirme

IDataErrorInfo doğrulama gerçekleştirmek için kullandığınız model nesneleri için yeni bir değer kümesi bağımsız olarak her özellik doğrulanır. ASP.NET MVC 1. 0'yeni değerleri olan özellikler doğrulandı. Tüm özellik doğrulayıcıları başarılı olursa ASP.NET MVC 2'de IDataErrorInfo hata özelliği adı verilir.

#### <a name="iis-script-mapping-script-is-no-longer-available-in-the-installer"></a>IIS komut dosyası eşleme komut dosyası yükleyici artık kullanılamıyor

IIS komut dosyası eşleme komut Klasik modda IIS 6 ve IIS 7 için betik eşlemeleri yapılandırmak için kullanılan bir komut satırı komut dosyasıdır. Komut dosyası eşleme betik Visual Studio geliştirme sunucusu kullanıyorsanız veya IIS 7 tümleşik modunda kullanırsanız, gerekli değildir. Komut dosyaları desteklenmeyen ayrı bir yükleme olarak kullanılabilir [ASP.NET WebStack](https://github.com/aspnet/AspNetWebStack).

#### <a name="the-htmlsubstitute-helper-method-in-mvc-futures-is-no-longer-available"></a>MVC vadeli Html.Substitute yardımcı yönteminin artık kullanılamıyor

MVC görünüm altyapıları işleme davranışlarındaki değişiklikler, nedeniyle Html.Substitute yardımcı yöntem çalışmaz ve kaldırıldı.

#### <a name="the-ivalueprovider-interface-replaces-all-uses-of-idictionary"></a>IDictionary tüm kullanımlarını IValueProvider arabirimi değiştirir

IDictionary MVC 1.0 artık kabul her özellik veya yöntem bağımsız IValueProvider kabul eder. Bu değişiklik, özel değer sağlayıcıları veya özel model bağlayıcıları içeren uygulamaları etkiler. Özellikleri ve bu değişiklikten etkilenen yöntemleri örnekleri şunlardır:

- ValueProvider özelliği ControllerBase ve ModelBindingContext sınıflarının.
- Denetleyici sınıfı TryUpdateModel yöntemleri.

#### <a name="new-css-classes-were-added-in-the-sitecss-file"></a>Yeni CSS sınıfları Site.css dosyasında eklendi

ASP.NET MVC proje şablonları Site.css dosyasında doğrulama işlevselliği ve şablonlu Yardımcılar tarafından kullanılan yeni stiller içerecek şekilde güncelleştirildi.

#### <a name="helpers-now-return-an-mvchtmlstring-object"></a>Yardımcıları şimdi MvcHtmlString nesneyi geri dön

ASP.NET 4'te yeni HTML kodlaması ifade sözdizimini yararlanmak için dönüş türü için HTML Yardımcıları MvcHtmlString yerine bir dize sunulmuştur. ASP.NET 3.5 ASP.NET MVC 2 ve yeni Yardımcıları kullanırsanız, HTML kodlaması sözdizimi yararlanmak mümkün olmaz; Yeni sözdizimi, ASP.NET 4'te ASP.NET MVC 2 çalıştırdığınızda kullanılabilir.

#### <a name="jsonresult-now-responds-only-to-http-post-requests"></a>JsonResult artık yalnızca HTTP POST isteklerini yanıtlar

Varsayılan olarak, bilgilerin açıklanması olası sahip saldırılar ele geçirme JSON azaltmak için JsonResult sınıfı yalnızca HTTP POST isteklerini yanıtlar. AJAX GET çağrıları bir JsonResult nesnesi döndüren eylem yöntemleri için POST kullanmayı değiştirilmelidir. Gerekirse, yeni JsonResult JsonRequestBehavior özelliğini ayarlayarak bu davranışı geçersiz kılabilirsiniz. Olası yararlanma hakkında daha fazla bilgi için blog gönderisine bakın [JSON ele geçirme](http://haacked.com/archive/2009/06/25/json-hijacking.aspx) Phil Haack'ın blogunda.

#### <a name="model-and-modeltype-property-setters-on-modelbindingcontext-are-obsolete"></a>Model ve ModelType özellik ayarlayıcıları ModelBindingContext üzerinde geçersiz

Yeni bir ayarlanabilir ModelMetadata özelliği ModelBindingContext sınıfı eklendi. Yeni özellik modeli ve ModelType özellikleri yalıtır. Model ve ModelType özellikleri artık kullanılmıyor olsa da, geriye dönük uyumluluk için özellik alıcıları çalışmaya; Bunlar, ModelMetadata özellik değerini almak için temsilci.

#### <a name="changes-to-the-defaultcontrollerfactory-class-break-custom-controller-factories-that-derive-from-it"></a>Bu sınıftan türetilen özel denetleyici üreteçlerini DefaultControllerFactory sınıfı değişiklikler bölün

DefaultControllerFactory sınıfı bir RequestContext özelliği kaldırarak giderilmiştir. Bu özellik yerine istek bağlamı örneği sanal korumalı GetControllerInstance ve GetControllerType yöntemleri geçirilir. Bu değişiklik DefaultControllerFactory türetilen özel denetleyici üreteçlerini etkiler.

Özel denetleyici üreteçlerini, genellikle ASP.NET MVC için bağımlılık ekleme uygulamaları sağlamak için kullanılır. ASP.NET MVC 2 desteklemek için özel denetleyici üreteçlerini güncelleştirmek için yöntem imzası veya imzaları yeni imzalar eşleşecek şekilde değiştirin ve isteği bağlam parametresi yerine özelliğini kullanın.

#### <a name="area-is-a-now-a-reserved-route-value-key"></a>"Alanı" artık ayrılan rota değeri anahtarıdır

Rota değerleri dizesi "alanı", "denetleyici" ve "eylem" biçiminde ASP.NET MVC uygulamasında özel bir anlamı artık sahiptir. HTML Yardımcıları "alanı" içeren bir rota değeri sözlüğü sağlandıysa Yardımcıları artık "alanı" sorgu dizesinde ekleyeceği olduğunu olduğu bir çıkarımında bulunulur.

Alanları özelliğini kullanıyorsanız, {alanı} yönlendirme URL'si bir parçası olarak kullanmayacak şekilde emin olun.


## <a id="_TOC6"></a>Sorumluluk reddi

Bu ön belge ve burada açıklanan yazılım son ticari sürümünden önce önemli ölçüde değişebilir.

Bu belgede yer alan bilgileri yayımlandığı tarih itibariyle açıklanan sorunları, Microsoft Corporation'ın geçerli görünümde temsil eder. Microsoft değişen piyasa koşullarına yanıt vermesi gerektiğinden Microsoft tarafında taahhüdü olarak yorumlanmamalıdır ve Microsoft belgenin yayımlanma tarihinden sonra sunulan bilgilerin doğruluğu garanti edemez.

Bu teknik incelemede yalnızca bilgilendirme amaçlıdır. MICROSOFT HİÇBİR AÇIK, ZIMNİ VEYA NİZAMİ BU BELGEDEKİ BİLGİLER GARANTİDE BULUNMAZ.

İlgili tüm telif hakkı yasalarına uymak kullanıcının sorumluluğundadır. Telif Hakkı altındaki sınırlamaksızın, bu belgenin hiçbir bölümü çoğaltılamaz, depolanan ya da bir geri alma sisteminde sunulan ya da herhangi bir yöntem (elektronik, mekanik, fotokopi, kayıt veya diğer) veya herhangi bir amaçla aktarılan, Microsoft Corporation'ın izni hızlı yazılır.

Microsoft, patentler, patent uygulamaları, ticari markalar, telif hakkı veya bu belgedeki konuyla ilgili diğer fikri mülkiyet hakları sahip olabilir. Microsoft'tan herhangi yazılı Lisans sözleşmesindeki, bu belgeyi bulundurmak, herhangi bir lisans bu patentler, ticari markaları, telif hakkı veya diğer fikri mülkiyet hakkı sağlamaz açıkça belirtilmediği sürece.

Aksi belirtilmediği sürece, örnek şirketler, kuruluşlar, ürünler, etki alanı adları, e-posta adresleri, logolar, kişiler, yerler ve olaylar burada olan kurgusal ve tüm gerçek şirket, kuruluş, ürün, etki alanı adı, e-posta ile ilişki gösterilen adresi, logo, kişi, yer veya olay amaçlanmamıştır veya çıkarılmamalıdır.

© 2010 Microsoft Corporation. Tüm hakları saklıdır.

Microsoft ve Windows kayıtlı ticari markaları ya da Microsoft Corporation'ın ABD'de ve/veya diğer ülkelerdeki ticari markalarıdır.

Burada sözü edilen gerçek şirketler ve ürünler adları, ticari markalar ilgili sahiplerinin olabilir.
