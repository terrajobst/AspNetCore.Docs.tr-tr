---
title: GRPC Hizmetleri HTTP API ile karşılaştırma
author: jamesnk
description: Nasıl gRPC karşılaştırır HTTP API'lerini ve ne sahip senaryolar önerilir öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 03/26/2019
uid: grpc/comparison
ms.openlocfilehash: fbe1647ab6f5e890700eccf43f920e0ef2b37ce7
ms.sourcegitcommit: 687ffb15ebe65379f75c84739ea851d5a0d788b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58489013"
---
# <a name="comparing-grpc-and-http-apis"></a>GRPC ve HTTP API ile karşılaştırma

Tarafından [James Newton-King](https://twitter.com/jamesnk)

Bu makalede arasında bir karşılaştırma sağlanmaktadır [gRPC](https://grpc.io/docs/guides/) ve HTTP API'lerini ve gRPC diğer teknolojileri kullanarak senaryoları önerir.

#### <a name="overview"></a>Genel Bakış

|    Özellik             |    gRPC                                                 |    JSON ile HTTP API'leri                       |
|------------------------|---------------------------------------------------------|----------------------------------------------|
|    Sözleşme            |    Gerekli (`*.proto`)                                 |    İsteğe bağlı (Openapı)                        |
|    Taşıma           |    HTTP/2                                               |    HTTP                                      |
|    Yükü             |    [Protobuf (küçük, ikili)](#performance)             |    JSON (büyük, insan tarafından okunabilir)              |
|    Prescriptiveness    |    [Katı belirtimi](#strict-specification)        |    Kaybedilir. Tüm HTTP geçerlidir                  |
|    Akış           |    [İstemci, sunucu, çift yönlü](#streaming)         |    İstemci, sunucu                            |
|    Tarayıcı desteği     |    [Hayır (grpc web gerektirir)](#limited-browser-support)   |    Evet                                       |
|    Güvenlik            |    Aktarım (HTTPS)                                    |    Aktarım (HTTPS)                         |
|    İstemci kod üretimi     |    [Evet](#code-generation)                              |    Openapı + üçüncü taraf araçları             |

## <a name="grpc-strengths"></a>gRPC güçlü

### <a name="performance"></a>Performans

gRPC iletileri kullanarak serileştirilir [Protobuf](https://developers.google.com/protocol-buffers/docs/overview), verimli ikili ileti biçimi. Protobuf çok hızlı bir şekilde sunucu ve istemci serileştirir. Protobuf serileştirme sonuçlarda küçük ileti yüklerini, sınırlı bant genişliği senaryolarda önemli mobil uygulamalar gibi çalışır.

gRPC, HTTP/2, HTTP üzerinden önemli performans avantajlarının sağlayan HTTP büyük bir düzeltme için tasarlanmıştır 1.x:

* İkili çerçeveleme ve sıkıştırma. HTTP/2 protokolüne sıkıştırılmış ve verimli gönderme ve alma.
* Birden çok HTTP/2 çağrıları tek bir TCP bağlantı üzerinden çoğullama. Çoğullama ortadan [satır baş engelleme](https://en.wikipedia.org/wiki/Head-of-line_blocking).

### <a name="code-generation"></a>Kod Üretimi

Tüm gRPC çerçeveleri kod oluşturma için birinci sınıf destek sağlar. GRPC geliştirme için çekirdek dosyasıdır [ `*.proto` dosya](https://developers.google.com/protocol-buffers/docs/proto3), ilgili kişinin gRPC Hizmetleri ve iletileri tanımlar. Bu dosya gRPC çerçevelerini hizmet temel sınıfı, iletileri ve eksiksiz bir istemci kodu oluşturur.

Paylaşımı tarafından `*.proto` dosya sunucu ve istemci, iletileri ve istemci kodu arasında oluşturulabilir uçtan uca. İstemci kod üretimi iletilerin istemci ve sunucu üzerinde çoğaltma ortadan kaldırır ve kesin tür belirtilmiş bir istemci sizin için oluşturur. Bir istemci yazmak zorunda değil önemli geliştirme süresini birçok hizmet uygulamalarla kaydeder.

### <a name="strict-specification"></a>Katı belirtimi

gRPC, Basitlik aracılığıyla Geliştirici zaman tasarrufu sağlar. [GRPC belirtimi](https://github.com/grpc/grpc/blob/master/doc/PROTOCOL-HTTP2.md) gRPC hizmet yöntemi nasıl göründüğünü açıklayıcı olduğundan. Var olan herhangi bir biçimsel sözleşmesi JSON ile bir HTTP API'sinin ne gibi görünmelidir. Bir anlaşma eksikliği tartışılması URL biçimi üzerinde oluşturur. HTTP fiilleri ve yanıt kodları. gRPC tartışılması gRPC yöntemi gibi benzemelidir bildiren belirtilerek ortadan kaldırır.

### <a name="streaming"></a>Akış

HTTP/2 uzun süreli, gerçek zamanlı iletişim akışlar için bir temel sağlar. gRPC, HTTP/2'den akış için birinci sınıf destek sağlar.

GRPC hizmeti, tüm akış birleşimleri destekler:

* Birli (akış yok)
* Sunucudan istemciye akış
* Akış sunucusu için istemci
* Yönlü akış

### <a name="deadlinetimeouts-and-cancellation"></a>Son tarih/zaman aşımları ve iptal etme

gRPC ne kadar bir RPC tamamlanması beklenecek iradeye sahip oldukları belirtin etmesine olanak tanır. [Son](https://grpc.io/blog/deadlines) sunucuya gönderilir ve sunucunun hangi eylemin son aşarsa karar verebilirsiniz. Örneğin, sunucu zaman aşımı üzerinde devam eden gRPC/HTTP/veritabanı istekleri iptal et.

Son tarih ve alt gRPC aracılığıyla iptal yayma çağrıları yardımcı olan kaynak kullanımı sınırlarını zorunlu.

## <a name="grpc-recommended-scenarios"></a>gRPC önerilen senaryolar

gRPC aşağıdaki senaryolar için uygundur:

* **Mikro Hizmetler** -gRPC olan tasarlanmış düşük gecikme süresi ve yüksek hacimli iletişim. gRPC verimliliği kritik olduğu basit mikro hizmetler için idealdir.
* **Noktadan noktaya gerçek zamanlı iletişim** -gRPC yönlü akış için mükemmel destek vardır. gRPC Hizmetleri iletileri yoklama olmadan gerçek zamanlı gönderebilir.
* **Polygot ortamları** -gRPC araçları gRPC çok dilli ortamlarda iyi bir seçim yaparak, tüm popüler geliştirme dilleri destekler.
* **Kısıtlanmış ortamları ağ** -gRPC iletileri Protobuf, bir basit bir ileti biçimine serileştirilir. Her zaman bir gRPC ileti eşdeğer bir JSON ileti daha küçük olur.

## <a name="grpc-weaknesses"></a>gRPC zayıf

### <a name="limited-browser-support"></a>Sınırlı tarayıcı desteği

Doğrudan gRPC hizmeti bir tarayıcıdan bugün çağırmak mümkün değildir. gRPC HTTP/2 özellikleri yoğun olarak kullanıyorsa ve hiçbir tarayıcı web istekleri gRPC istemciyi desteklemek için gereken denetim düzeyini sağlar. Örneğin, tarayıcılar değil HTTP/2 kullanılmasını gerektiren bir çağrıyı yapanın veya temel alınan HTTP/2 çerçeveler erişim sağlar.

[gRPC Web](https://grpc.io/docs/tutorials/basic/web.html) gRPC ekibinden tarayıcıda sınırlı gRPC desteği sağlayan ek bir teknolojidir. gRPC Web iki bölümden oluşur: tüm modern tarayıcılar ve gRPC Web proxy sunucusunda destekleyen bir JavaScript istemcisi. Proxy gRPC Web istemcisi çağırır ve proxy gRPC isteklerinde gRPC sunucusuna iletir.

GRPC ait özelliklerin tümünü gRPC Web tarafından desteklenir. İstemci ve çift yönlü akış desteklenmez ve akış sunucusu için destek sınırlıdır.

### <a name="not-human-readable"></a>Değil insan tarafından okunabilir

HTTP API isteklerinin metin olarak gönderilir ve okunabilir ve insanlar tarafından oluşturuldu.

gRPC iletileri Protobuf ile varsayılan olarak kodlanır. Protobuf göndermek ve almak için etkili olsa da, ikili biçimi İnsan; okunabilir. Protobuf gerektirir belirtilen ileti arabirimi açıklama `*.proto` dosya düzgün bir şekilde seri durumdan çıkarılacak. Bu sorunun geçici çözümü Protobuf iletileri [destek dönüştürme json'a ve json'dan](https://developers.google.com/protocol-buffers/docs/proto3#json). Bu özellik, üretim ortamlarında verimli ikili ileti geçiş geliştirme sırasında insanlar tarafından okunabilen ileti gönderme sağlar.

## <a name="alternative-framework-scenarios"></a>Alternatif Framework senaryoları

Aşağıdaki senaryolarda gRPC üzerinden diğer çerçeveler önerilir:

* **Tarayıcı erişilebilir API'leri** -gRPC tamamen tarayıcıda desteklenmiyor. gRPC Web tarayıcısı desteği sağlayabilir, ancak sınırlamaları vardır ve bir sunucu proxy tanıtır.
* **Gerçek zamanlı iletişim yayın** - gRPC akış üzerinden gerçek zamanlı iletişim destekler, ancak kayıtlı bağlantılara bir ileti yayın kavramı yok. Örneğin, burada yeni sohbet iletileri sohbet odası tüm istemcilere gönderilmesi gereken bir sohbet odası senaryosunda, tek tek istemci için yeni sohbet iletileri akışını her gRPC çağrı gerekir. [SignalR](xref:signalr/introduction) bu senaryo için iyi bir çerçevedir. Bu kavramı kalıcı bağlantılar ve ileti yayınlamak için yerleşik desteği vardır.
* **Arası iletişimi işlem** -gelen çağrıları kabul etmek için bir HTTP/2 gRPC sunucusunu barındırmak bir işlem gerekir. Windows için arası iletişimi işlem [adlandırılmış kanallar WCF ile](/dotnet/framework/wcf/feature-details/choosing-a-transport#when-to-use-the-named-pipe-transport) iletişim hızlı ve basit yöntemdir.

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
