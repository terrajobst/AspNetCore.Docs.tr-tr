---
uid: web-api/overview/formats-and-model-binding/json-and-xml-serialization
title: JSON ve XML serileştirme ASP.NET Web API'de | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/30/2012
ms.topic: article
ms.assetid: 1cd7525d-de5e-4ab6-94f0-51480d3255d1
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/formats-and-model-binding/json-and-xml-serialization
msc.type: authoredcontent
ms.openlocfilehash: e7fbcd41d64651255763c7629f0232788dcb3d30
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37400927"
---
<a name="json-and-xml-serialization-in-aspnet-web-api"></a>JSON ve XML serileştirme ASP.NET Web API
====================
tarafından [Mike Wasson](https://github.com/MikeWasson)

Bu makalede, ASP.NET Web API'de JSON ve XML biçimlendiricileri açıklanır.

ASP.NET Web API, bir *medya türü biçimlendiricisi* olabilir bir nesnedir:

- Bir HTTP nesnelerinden okuma CLR ileti gövdesi
- Bir HTTP ileti gövdesine CLR nesnelerine yazma

Web API'si, JSON ve XML için medya türü biçimlendiricileri sağlar. Framework, varsayılan olarak bu biçimlendiricileri ardışık düzene ekler. İstemciler HTTP isteği Accept üst bilgisi, JSON veya XML isteyebilir.

## <a name="contents"></a>İçindekiler

- [JSON medya türü biçimlendiricisi](#json_media_type_formatter)

    - [Salt okunur özellikler](#json_readonly)
    - [Tarihleri](#json_dates)
    - [Girintileme](#json_indenting)
    - [Camel Casing](#json_camelcasing)
    - [Anonim ve zayıf yazılmış nesneler](#json_anon)
- [XML medya türü biçimlendiricisi](#xml_media_type_formatter)

    - [Salt okunur özellikler](#xml_readonly)
    - [Tarihleri](#xml_dates)
    - [Girintileme](#xml_indenting)
    - [Ayar türü başına XML seri hale getiricileri genişletme](#xml_pertype)
- [JSON veya XML biçimlendiricisi kaldırılıyor](#removing_the_json_or_xml_formatter)
- [İşleme döngüsel nesne başvuruları](#handling_circular_object_references)
- [Test nesne seri hale getirme](#testing_object_serialization)

<a id="json_media_type_formatter"></a>
## <a name="json-media-type-formatter"></a>JSON medya türü biçimlendiricisi

JSON biçimlendirme tarafından sağlanır **JsonMediaTypeFormatter** sınıfı. Varsayılan olarak, **JsonMediaTypeFormatter** kullanan [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) serileştirme gerçekleştirmek için kitaplığı. Json.NET bir üçüncü taraf açık kaynak projesidir.

Tercih ederseniz, yapılandırabileceğiniz **JsonMediaTypeFormatter** kullanılacak sınıfı **DataContractJsonSerializer** Json.NET yerine. Bunu yapmak için ayarlanmış **UseDataContractJsonSerializer** özelliğini **true**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample1.cs)]

### <a name="json-serialization"></a>JSON Seri Hale Getirme

Bu bölümde, JSON Biçimlendiricinin varsayılan kullanarak belirli bazı davranışları açıklanmaktadır [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) serileştirici. Bu Json.NET kitaplığının kapsamlı belgeler olarak tasarlanmamıştır; Daha fazla bilgi için [Json.NET belgeleri](http://james.newtonking.com/projects/json/help/).

#### <a name="what-gets-serialized"></a>Serileştirilmiş?

Varsayılan olarak, tüm ortak özellikler ve alanları seri hale getirilmiş JSON biçiminde dahil edilir. Bir özellik veya alan atlamak için kendisiyle süslemek **JsonIgnore** özniteliği.

[!code-csharp[Main](json-and-xml-serialization/samples/sample2.cs)]

İsterseniz bir &quot;katılımı&quot; yaklaşımı, sınıfıyla süslemek **DataContract** özniteliği. Bu öznitelik varsa, sahip oldukları sürece üyeleri yoksayılır **DataMember**. Ayrıca **DataMember** özel üyeler serileştirmek için.

[!code-csharp[Main](json-and-xml-serialization/samples/sample3.cs)]

<a id="json_readonly"></a>
### <a name="read-only-properties"></a>Salt okunur özellikler

Salt okunur özellikler varsayılan olarak serileştirilir.

<a id="json_dates"></a>
### <a name="dates"></a>Tarihleri

Varsayılan olarak Json.NET tarihleri Yazar [ISO 8601](http://www.w3.org/TR/NOTE-datetime) biçimi. Tarihler UTC (Eşgüdümlü Evrensel Saat), "Z" soneki ile yazılır. Bir saat dilimi farkı yerel saat tarihleri içerir. Örneğin:

[!code-console[Main](json-and-xml-serialization/samples/sample4.cmd)]

Varsayılan olarak, saat dilimini Json.NET korur. DateTimeZoneHandling özelliğini ayarlayarak bunu geçersiz kılabilirsiniz:

[!code-csharp[Main](json-and-xml-serialization/samples/sample5.cs)]

Kullanmayı tercih ederseniz [Microsoft JSON tarih biçimi](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) ISO 8601 yerine ayarlamak **DateFormatHandling** serileştirici ayarlarını özelliği:

[!code-csharp[Main](json-and-xml-serialization/samples/sample6.cs)]

<a id="json_indenting"></a>
### <a name="indenting"></a>Girintileme

Girintili JSON yazmak için ayarlanmış **biçimlendirme** ayarını **Formatting.Intended**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample7.cs)]

<a id="json_camelcasing"></a>
### <a name="camel-casing"></a>Camel Casing

JSON özellik adları camel casing ile veri modelinizi değiştirmeden yazmak için ayarlanmış **CamelCasePropertyNamesContractResolver** seri hale getirici üzerinde:

[!code-csharp[Main](json-and-xml-serialization/samples/sample8.cs)]

<a id="json_anon"></a>
### <a name="anonymous-and-weakly-typed-objects"></a>Anonim ve zayıf yazılmış nesneler

Bir eylem yöntemi, anonim bir nesne döndürür ve JSON için seri hale getirme. Örneğin:

[!code-csharp[Main](json-and-xml-serialization/samples/sample9.cs)]

Aşağıdaki JSON yanıt iletisi gövdesi içerir:

[!code-json[Main](json-and-xml-serialization/samples/sample10.json)]

JSON nesneleri istemcilerden gelen web API alır gevşek yapılandırılmış, için istek gövdesi seri durumdan çıkarabiliyorsa bir **Newtonsoft.Json.Linq.JObject** türü.

[!code-csharp[Main](json-and-xml-serialization/samples/sample11.cs)]

Ancak, kesin türü belirtilmiş veri nesnelerini kullanmak daha iyi olur. Ardından verileri kendiniz ayrıştırmak gerekmez ve model doğrulama avantajlarından yararlanın.

XML seri hale getirici anonim türler desteklemiyor veya **JObject** örnekleri. JSON verilerinizi bu özellikleri kullanırsanız, XML biçimlendiricisi ardışık düzen tarafından bu makalenin sonraki bölümlerinde açıklandığı kaldırmalısınız.

<a id="xml_media_type_formatter"></a>
## <a name="xml-media-type-formatter"></a>XML medya türü biçimlendiricisi

XML Biçimlendirme tarafından sağlanır **XmlMediaTypeFormatter** sınıfı. Varsayılan olarak, **XmlMediaTypeFormatter** kullanan **DataContractSerializer** serileştirme gerçekleştirmek için sınıf.

Tercih ederseniz, yapılandırabileceğiniz **XmlMediaTypeFormatter** kullanılacak **XmlSerializer** yerine **DataContractSerializer**. Bunu yapmak için ayarlanmış **UseXmlSerializer** özelliğini **true**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample12.cs)]

**XmlSerializer** sınıf türleri daha dar bir kümesini destekler **DataContractSerializer**, ancak, elde edilen XML üzerinde daha fazla denetim sağlar. Kullanmayı **XmlSerializer** varolan bir XML şemasına uyan gerekiyorsa.

### <a name="xml-serialization"></a>XML seri hale getirme

Bu bölümde, XML biçimlendiricisi varsayılan kullanarak, belirli bazı davranışları açıklanmaktadır **DataContractSerializer**.

Varsayılan olarak, DataContractSerializer aşağıdaki gibi davranır:

- Tüm ortak okuma/yazma özellikleri ve alanları serileştirilir. Bir özellik veya alan atlamak için kendisiyle süslemek **IgnoreDataMember** özniteliği.
- Özel ve korumalı üyeler seri duruma getirilmez.
- Salt okunur özelliklerini seri duruma getirilmez. (Ancak, bir salt okunur koleksiyon özelliği içeriğini serileştirilir.)
- Sınıf bildirimi içinde tam olarak göründükleri gibi sınıf ve üye adlarını XML'de yazılır.
- Varsayılan XML ad alanı kullanılır.

Seri hale getirme hakkında daha fazla denetime ihtiyacınız varsa, sınıf ile donatmak **DataContract** özniteliği. Bu öznitelik mevcut olduğunda, sınıf şu şekilde serileştirilmiş:

- &quot;Kabul et&quot; yaklaşım: özellikler ve alanları seri varsayılan olarak. Bir özellik veya alan seri hale getirmek için onunla süslemek **DataMember** özniteliği.
- Özel veya korumalı bir üye serileştirmek için onunla süslemek **DataMember** özniteliği.
- Salt okunur özelliklerini seri duruma getirilmez.
- Sınıf adı XML'de görüntülenme şeklini değiştirmek için Ayarla *adı* parametresinde **DataContract** özniteliği.
- Üye adı XML'de görüntülenme şeklini değiştirmek için Ayarla *adı* parametresinde **DataMember** özniteliği.
- XML ad alanı değiştirmek için Ayarla *Namespace* parametresinde **DataContract** sınıfı.

<a id="xml_readonly"></a>
### <a name="read-only-properties"></a>Salt okunur özellikler

Salt okunur özelliklerini seri duruma getirilmez. Destek özel alanı salt okunur bir özellik varsa özel bir alanla işaretleyebilirsiniz **DataMember** özniteliği. Bu yaklaşım gerektirir **DataContract** sınıfı özniteliği.

[!code-csharp[Main](json-and-xml-serialization/samples/sample13.cs)]

<a id="xml_dates"></a>
### <a name="dates"></a>Tarihleri

Tarihleri ISO 8601 biçiminde yazılır. Örneğin, &quot;2012-05-23T20:21:37.9116538Z&quot;.

<a id="xml_indenting"></a>
### <a name="indenting"></a>Girintileme

Girintili XML yazmak için ayarlanmış **Girintile** özelliğini **true**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample14.cs)]

<a id="xml_pertype"></a>
## <a name="setting-per-type-xml-serializers"></a>Ayar türü başına XML seri hale getiricileri genişletme

Farklı CLR türleri için farklı XML seri hale getiricileri genişletme ayarlayabilirsiniz. Örneğin, gerektiren belirli bir veri nesnesi olabilir **XmlSerializer** geriye dönük uyumluluk için. Kullanabileceğiniz **XmlSerializer** bu nesne ve kullanmaya devam **DataContractSerializer** diğer türleri için.

Belirli bir tür için bir XML serileştiricisi ayarlamak için çağrı **SetSerializer**.

[!code-csharp[Main](json-and-xml-serialization/samples/sample15.cs)]

Belirtebileceğiniz bir **XmlSerializer** veya türetilen herhangi bir nesne **XmlObjectSerializer**.

<a id="removing_the_json_or_xml_formatter"></a>
## <a name="removing-the-json-or-xml-formatter"></a>JSON veya XML biçimlendiricisi kaldırılıyor

Bunları kullanmak istemiyorsanız, biçimlendirici JSON veya XML biçimlendiricisi biçimlendiricileri, listeden kaldırabilirsiniz. Bunu yapmak için temel neden şunlardır:

- Web sınırlamak için belirli bir ortama API yanıtları yazın. Örneğin, yalnızca JSON yanıtları desteği ve XML biçimlendiricisi kaldırmak karar verebilirsiniz.
- Özel bir Biçimlendiricinin varsayılan biçimlendirici değiştirilecek. Örneğin, kendi özel uygulanışı JSON biçimlendirici ile JSON biçimlendirici yerini alabilir.

Aşağıdaki kod, varsayılan biçimlendiricileri kaldırılacağı gösterilmektedir. Bu çağrı, **uygulama\_Başlat** yöntemi, Global.asax dosyasında tanımlanmış.

[!code-csharp[Main](json-and-xml-serialization/samples/sample16.cs)]

<a id="handling_circular_object_references"></a>
## <a name="handling-circular-object-references"></a>İşleme döngüsel nesne başvuruları

Varsayılan olarak, JSON ve XML biçimlendiricileri tüm nesneleri değerleri yazın. İki özellik de aynı nesneye başvuruyorsa ya da aynı nesne iki kez bir koleksiyonda görünürse, biçimlendirici nesne iki kez serileştirir. Nesne grafınızı döngüsünü içeriyorsa seri hale getirici bir özel durum oluşturmaz, bu grafikte bir döngü algıladığında, belirli bir sorunu olmasıdır.

Aşağıdaki nesne modelleri ve denetleyici göz önünde bulundurun.

[!code-csharp[Main](json-and-xml-serialization/samples/sample17.cs)]

Bu eylemi çağırma biçimlendirici için bir durum kodunu 500 (iç sunucu hatası) yanıtı istemciye izlendiği bir özel durum neden olur.

Nesne başvuruları json'da korumak için aşağıdaki kodu ekleyin. **uygulama\_Başlat** Global.asax dosyasındaki yöntemi:

[!code-csharp[Main](json-and-xml-serialization/samples/sample18.cs)]

Denetleyici eylemini şimdi şuna benzer bir JSON döndürür:

[!code-json[Main](json-and-xml-serialization/samples/sample19.json)]

Seri hale getirici ekler bildirimi bir &quot;$id&quot; iki nesne için özellik. Ayrıca, değeri bir nesne başvurusu ile değiştirir Employee.Department özelliği bir döngü oluşturur, bu nedenle, algıladığı: {&quot;$ref&quot;:&quot;1&quot;}.

> [!NOTE]
> Nesne başvuruları JSON'da standart değildir. Bu özelliği kullanmadan önce istemcilerinize sonuçları ayrıştırabiliyor olup olmayacağını göz önünde bulundurun. Graftan döngüleri kaldırmanız daha iyi olabilir. Örneğin, çalışan bağlantıdan departmanı dön gerçekten bu örnekte gerekli değildir.


Nesne başvuruları XML'de korumak için iki seçeneğiniz vardır. Daha basit bir seçenek eklemektir `[DataContract(IsReference=true)]` modeli sınıfınıza. *IsReference* parametresi nesne başvuruları sağlar. Unutmayın **DataContract** eklemeniz gerekecektir şekilde kabul etme, serileştirme yapar **DataMember** özelliklerine öznitelikleri:

[!code-csharp[Main](json-and-xml-serialization/samples/sample20.cs)]

Biçimlendirici XML benzer üretecektir artık aşağıdaki:

[!code-xml[Main](json-and-xml-serialization/samples/sample21.xml)]

Model sınıfınızın özniteliklerinde önlemek istiyorsanız, başka bir seçenek yoktur: yeni bir tür özel oluşturma **DataContractSerializer** ayarlayın ve örnek *preserveObjectReferences* için **true**  oluşturucuda. Ardından bu örneği üzerinde XML medya türü biçimlendiricisi başına türü seri hale getirici ayarlayın. Aşağıdaki kod, bunun nasıl yapılacağını göster:

[!code-csharp[Main](json-and-xml-serialization/samples/sample22.cs?highlight=3)]

<a id="testing_object_serialization"></a>
## <a name="testing-object-serialization"></a>Test nesne seri hale getirme

Web API'nizi tasarlarken, veri nesnelerinizi nasıl serileştirilecek test etmek kullanışlıdır. Denetleyici oluşturma veya bir denetleyici eylemi çağrılırken bunu yapabilirsiniz.

[!code-csharp[Main](json-and-xml-serialization/samples/sample23.cs)]
