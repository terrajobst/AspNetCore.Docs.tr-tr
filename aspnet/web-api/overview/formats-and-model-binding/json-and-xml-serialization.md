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
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/json-and-xml-serialization
msc.type: authoredcontent
ms.openlocfilehash: b1fcaf70cc38d73da0a454764520197b97f34b26
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="json-and-xml-serialization-in-aspnet-web-api"></a>JSON ve XML serileştirme ASP.NET Web API
====================
tarafından [CAN Wasson](https://github.com/MikeWasson)

Bu makalede, ASP.NET Web API içinde JSON ve XML biçimlendiricileri açıklanmaktadır.

ASP.NET Web API içinde bir *medya türü biçimlendiricisi* olabilir bir nesnedir:

- Gövde okuma CLR nesneleri bir HTTP iletisi
- Bir HTTP ileti gövdesini yazma CLR nesneleri

Web API, JSON ve XML için medya türü biçimlendiricilerini sağlar. Framework bu biçimlendiricileri varsayılan olarak ardışık düzenine ekler. İstemciler HTTP isteği kabul et üstbilgisinde JSON veya XML isteyebilir.

## <a name="contents"></a>İçindekiler

- [JSON medya türü biçimlendiricisi](#json_media_type_formatter)

    - [Salt okunur özellikler](#json_readonly)
    - [Tarihleri](#json_dates)
    - [Girintileme](#json_indenting)
    - [Ortası büyük/küçük harf](#json_camelcasing)
    - [Anonim ve zayıf yazılmış nesneleri](#json_anon)
- [XML medya türü biçimlendiricisi](#xml_media_type_formatter)

    - [Salt okunur özellikler](#xml_readonly)
    - [Tarihleri](#xml_dates)
    - [Girintileme](#xml_indenting)
    - [Tür başına XML serileştiricileri ayarlama](#xml_pertype)
- [JSON veya XML biçimlendiricisi kaldırma](#removing_the_json_or_xml_formatter)
- [İşleme döngüsel nesne başvuruları](#handling_circular_object_references)
- [Sınama nesnesi seri hale getirme](#testing_object_serialization)

<a id="json_media_type_formatter"></a>
## <a name="json-media-type-formatter"></a>JSON medya türü biçimlendiricisi

JSON biçimlendirme tarafından sağlanır **JsonMediaTypeFormatter** sınıfı. Varsayılan olarak, **JsonMediaTypeFormatter** kullanan [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) serileştirme gerçekleştirmek için kitaplık. Json.NET üçüncü taraf açık kaynaklı bir projedir.

İsterseniz, yapılandırabileceğiniz **JsonMediaTypeFormatter** kullanmak için sınıf **DataContractJsonSerializer** Json.NET yerine. Bunu yapmak için ayarlanmış **UseDataContractJsonSerializer** özelliğine **true**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample1.cs)]

### <a name="json-serialization"></a>JSON Seri Hale Getirme

Bu bölümde varsayılan kullanılarak JSON biçimlendirici belirli bazı davranışlarını anlatır [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) seri hale getirici. Json.NET kitaplığının kapsamlı belgeler olması düşünülmemiştir; Daha fazla bilgi için bkz: [Json.NET belgelerine](http://james.newtonking.com/projects/json/help/).

#### <a name="what-gets-serialized"></a>Serileştirilmiş?

Varsayılan olarak, tüm genel özellikleri ve alanları serileştirilmiş JSON'de dahil edilir. Bir özellik veya alan atlamak için kendisiyle tasarlamanız **JsonIgnore** özniteliği.

[!code-csharp[Main](json-and-xml-serialization/samples/sample2.cs)]

İsterseniz bir &quot;katılımı&quot; yaklaşımını, sınıfıyla tasarlamanız **DataContract** özniteliği. Bu öznitelik varsa, sahip oldukları sürece üyeleri yoksayılır **DataMember**. Aynı zamanda **DataMember** özel üyeler seri hale getirilemedi.

[!code-csharp[Main](json-and-xml-serialization/samples/sample3.cs)]

<a id="json_readonly"></a>
### <a name="read-only-properties"></a>Salt okunur özellikler

Salt okunur özellikler varsayılan olarak serileştirilir.

<a id="json_dates"></a>
### <a name="dates"></a>tarihleri

Varsayılan olarak, Json.NET tarihleri Yazar [ISO 8601](http://www.w3.org/TR/NOTE-datetime) biçimi. Tarihler, UTC (Eşgüdümlü Evrensel Saat) "Z" soneki ile yazılır. Yerel saat tarihleri bir saat dilimi farkı içerir. Örneğin:

[!code-console[Main](json-and-xml-serialization/samples/sample4.cmd)]

Varsayılan olarak, saat dilimi Json.NET korur. DateTimeZoneHandling özelliğini ayarlayarak bunu geçersiz kılabilirsiniz:

[!code-csharp[Main](json-and-xml-serialization/samples/sample5.cs)]

Kullanmayı tercih ederseniz, [Microsoft JSON tarih biçimi](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) ISO 8601 yerine ayarlamak **DateFormatHandling** özelliği seri hale getirici ayarları:

[!code-csharp[Main](json-and-xml-serialization/samples/sample6.cs)]

<a id="json_indenting"></a>
### <a name="indenting"></a>Girintileme

Girintili JSON yazmak için ayarlanmış **biçimlendirme** ayarını **Formatting.Intended**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample7.cs)]

<a id="json_camelcasing"></a>
### <a name="camel-casing"></a>Ortası büyük/küçük harf

JSON özellik adlarını ortası büyük/küçük harf, veri modelinizi değiştirmeden yazmak için ayarlanmış **CamelCasePropertyNamesContractResolver** seri hale getirici üzerinde:

[!code-csharp[Main](json-and-xml-serialization/samples/sample8.cs)]

<a id="json_anon"></a>
### <a name="anonymous-and-weakly-typed-objects"></a>Anonim ve zayıf yazılmış nesneleri

Bir eylem yöntemi, bir anonim nesnenin dönün ve JSON için seri hale. Örneğin:

[!code-csharp[Main](json-and-xml-serialization/samples/sample9.cs)]

Yanıt ileti gövdesi aşağıdaki JSON içerir:

[!code-json[Main](json-and-xml-serialization/samples/sample10.json)]

JSON nesnelerinin istemcilerinden API alır geniş web yapılandırılmış, istek gövdesi seri durumdan çıkarabiliyorsa bir **Newtonsoft.Json.Linq.JObject** türü.

[!code-csharp[Main](json-and-xml-serialization/samples/sample11.cs)]

Ancak, kesin türü belirtilmiş veri nesneleri kullanmak daha iyi olur. Ardından verileri kendiniz ayrıştırma gerekmez ve model doğrulama avantajları elde edin.

XML serileştiriciyi anonim türler desteklemiyor veya **JObject** örnekleri. Bu özellikleri JSON verileriniz için kullanırsanız, XML biçimlendiricisi ardışık düzen tarafından bu makalenin sonraki bölümlerinde açıklandığı gibi kaldırmanız gerekir.

<a id="xml_media_type_formatter"></a>
## <a name="xml-media-type-formatter"></a>XML medya türü biçimlendiricisi

XML Biçimlendirme tarafından sağlanır **XmlMediaTypeFormatter** sınıfı. Varsayılan olarak, **XmlMediaTypeFormatter** kullanan **DataContractSerializer** serileştirme gerçekleştirmek için sınıf.

İsterseniz, yapılandırabileceğiniz **XmlMediaTypeFormatter** kullanmak için **XmlSerializer** yerine **DataContractSerializer**. Bunu yapmak için ayarlanmış **UseXmlSerializer** özelliğine **true**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample12.cs)]

**XmlSerializer** sınıfı türlerinden daha dar bir kümesini destekler **DataContractSerializer**, ancak, sonuçta elde edilen XML üzerinde daha fazla denetim sağlar. Kullanmayı **XmlSerializer** var olan bir XML şeması ile eşleşmesi gerekiyorsa.

### <a name="xml-serialization"></a>XML seri hale getirme

Bu bölümde XML biçimlendiricisi varsayılan kullanılarak, belirli bazı davranışlarını anlatır **DataContractSerializer**.

Varsayılan olarak, DataContractSerializer aşağıdaki gibi davranır:

- Tüm ortak okuma/yazma özellikleri ve alanları serileştirilir. Bir özellik veya alan atlamak için kendisiyle tasarlamanız **IgnoreDataMember** özniteliği.
- Özel ve korumalı üyeleri seri değildir.
- Salt okunur özellikler seri değildir. (Ancak, bir salt okunur koleksiyonun özelliği içeriğini serileştirilir.)
- Sınıf bildiriminde tam olarak göründükleri gibi sınıfı ve üye adları XML'de yazılır.
- Varsayılan bir XML ad alanı kullanılır.

Seri hale getirme hakkında daha fazla denetime ihtiyacınız varsa, sınıfıyla tasarlamanız **DataContract** özniteliği. Bu öznitelik mevcut olduğunda sınıfı şu şekilde seri değildir:

- &quot;Kabul&quot; yaklaşım: özellikleri ve alanları değil serileştirilir varsayılan olarak. Bir özellik veya alan serileştirmek için ile işaretleme **DataMember** özniteliği.
- Özel veya korumalı bir üye serileştirmek için ile işaretleme **DataMember** özniteliği.
- Salt okunur özellikler seri değildir.
- Sınıf adı XML'de görünme biçimini değiştirmek için ayarlayın *adı* parametresinde **DataContract** özniteliği.
- Üye adı XML'de görünme biçimini değiştirmek için ayarlayın *adı* parametresinde **DataMember** özniteliği.
- XML ad alanı değiştirmek için ayarlayın *Namespace* parametresinde **DataContract** sınıfı.

<a id="xml_readonly"></a>
### <a name="read-only-properties"></a>Salt okunur özellikler

Salt okunur özellikler seri değildir. Bir yedekleme özel alan salt okunur bir özellik varsa, özel alanıyla işaretleyebilirsiniz **DataMember** özniteliği. Bu yaklaşım gerektirir **DataContract** Sınıf özniteliği.

[!code-csharp[Main](json-and-xml-serialization/samples/sample13.cs)]

<a id="xml_dates"></a>
### <a name="dates"></a>tarihleri

Tarihleri ISO 8601 biçiminde yazılır. Örneğin, &quot;2012-05-23T20:21:37.9116538Z&quot;.

<a id="xml_indenting"></a>
### <a name="indenting"></a>Girintileme

Girintili XML yazmak için ayarlanmış **girinti** özelliğine **true**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample14.cs)]

<a id="xml_pertype"></a>
## <a name="setting-per-type-xml-serializers"></a>Tür başına XML serileştiricileri ayarlama

Farklı CLR türleri için farklı XML serileştiricileri ayarlayabilirsiniz. Örneğin, gerektiren belirli veri nesnesi olabilir **XmlSerializer** geriye dönük uyumluluk için. Kullanabileceğiniz **XmlSerializer** bu nesne ve kullanmaya devam **DataContractSerializer** diğer türleri.

Belirli bir tür için bir XML serileştiricisi ayarlamak için arama **SetSerializer**.

[!code-csharp[Main](json-and-xml-serialization/samples/sample15.cs)]

Belirleyebileceğiniz bir **XmlSerializer** veya türetilen herhangi bir nesne **XmlObjectSerializer**.

<a id="removing_the_json_or_xml_formatter"></a>
## <a name="removing-the-json-or-xml-formatter"></a>JSON veya XML biçimlendiricisi kaldırma

Bunları kullanmak istemiyorsanız, JSON biçimlendirici veya XML biçimlendiricisi biçimlendiricileri, listeden kaldırabilirsiniz. Bunu yapmak için nedenler şunlardır:

- Web kısıtlamak için belirli bir ortam API yanıtlarını yazın. Örneğin, yalnızca JSON yanıtları desteklemek ve XML biçimlendiricisi kaldırmak karar verebilirsiniz.
- Varsayılan biçimlendirici ile özel bir biçimlendirici değiştirmek için. Örneğin, kendi özel JSON biçimlendirici uygulamasıyla JSON biçimlendirici değiştirebilirsiniz.

Aşağıdaki kod, varsayılan biçimlendiricileri kaldırmak gösterilmiştir. Bu çağrı, **uygulama\_Başlat** yöntemi, Global.asax dosyasında tanımlanmış.

[!code-csharp[Main](json-and-xml-serialization/samples/sample16.cs)]

<a id="handling_circular_object_references"></a>
## <a name="handling-circular-object-references"></a>İşleme döngüsel nesne başvuruları

Varsayılan olarak, JSON ve XML biçimlendiricileri değerleri olarak tüm nesneler yazma. İki özellik de aynı nesneye başvuruda bulunuyorsa veya aynı nesne iki kez koleksiyonunda görüntülenirse, biçimlendirici nesne iki kez serileştirir. Nesne grafiği döngüsünü içeriyorsa grafiğinde bir döngü algıladığında seri hale getirici bir özel durum atar belirli bir sorunu olmasıdır.

Aşağıdaki nesne modelleri ve denetleyici göz önünde bulundurun.

[!code-csharp[Main](json-and-xml-serialization/samples/sample17.cs)]

Bu eylemi çağırma biçimlendirici için bir durum kodu 500 (Dahili Sunucu hatası) yanıt istemciye çeviren bir özel durum neden olur.

JSON içinde nesne başvuruları korumak için aşağıdaki kodu ekleyin **uygulama\_Başlat** yöntemi Global.asax dosyasında:

[!code-csharp[Main](json-and-xml-serialization/samples/sample18.cs)]

Denetleyici eylemini şöyle JSON şimdi döndürür:

[!code-json[Main](json-and-xml-serialization/samples/sample19.json)]

Seri hale getirici ekler bildirimi bir &quot;$id&quot; hem nesnelerini özelliğine. Ayrıca, değeri bir nesne başvurusu yerini şekilde Employee.Department özelliği'nın bir döngü oluşturuyor algılar: {&quot;$ref&quot;:&quot;1&quot;}.

> [!NOTE]
> Nesne başvuruları JSON'de standart değildir. Bu özelliği kullanmadan önce istemcilerinizin sonuçları ayrıştırabiliyor olup olmayacağını göz önünde bulundurun. Yalnızca döngüleri Grafikten kaldırmak daha iyi olabilir. Örneğin, çalışan bağlantıdan departmanı dön gerçekten bu örnekte, gerekli değildir.


Nesne başvuruları XML korumak için iki seçeneğiniz vardır. Eklemek için daha basit seçenektir `[DataContract(IsReference=true)]` modeli sınıfınıza. *IsReference* parametresi nesne başvuruları sağlar. Unutmayın **DataContract** eklemeniz gerekir böylece katılımı, serileştirme yapar **DataMember** öznitelikleri özellikler:

[!code-csharp[Main](json-and-xml-serialization/samples/sample20.cs)]

Biçimlendirici XML benzer üretecektir artık aşağıdaki:

[!code-xml[Main](json-and-xml-serialization/samples/sample21.xml)]

Model sınıfı özniteliklerinde önlemek istiyorsanız, başka bir seçenek yoktur: yeni bir tür özel Oluştur **DataContractSerializer** örneği ve ayarlama *preserveObjectReferences* için **true**  oluşturucuda. Ardından bu örnek XML medya türü biçimlendirici başına türü seri hale getirici ayarlayın. Aşağıdaki kod bunun nasıl yapılacağı gösterilmektedir:

[!code-csharp[Main](json-and-xml-serialization/samples/sample22.cs?highlight=3)]

<a id="testing_object_serialization"></a>
## <a name="testing-object-serialization"></a>Sınama nesnesi seri hale getirme

Web API tasarlarken, veri nesnelerinizi serileştirilmiş nasıl test etmek kullanışlıdır. Bir denetleyici oluşturma veya bir denetleyici eylemi çağırma bunu yapabilirsiniz.

[!code-csharp[Main](json-and-xml-serialization/samples/sample23.cs)]
