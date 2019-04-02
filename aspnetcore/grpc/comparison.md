---
title: gRPC hizmetlerini HTTP API’leriyle karşılaştırma
author: jamesnk
description: Nasıl gRPC karşılaştırır HTTP API'lerini ve ne sahip senaryolar önerilir öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 03/31/2019
uid: grpc/comparison
ms.openlocfilehash: 05bd0357ada2d9a2c876469c533605ee7cbab5b3
ms.sourcegitcommit: 5995f44e9e13d7e7aa8d193e2825381c42184e47
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2019
ms.locfileid: "58809230"
---
# <a name="comparing-grpc-services-with-http-apis"></a>gRPC hizmetlerini HTTP API’leriyle karşılaştırma

Tarafından [James Newton-King](https://twitter.com/jamesnk)

Bu makalede açıklanır nasıl [gRPC Hizmetleri](https://grpc.io/docs/guides/) HTTP API'lerini karşılaştırın. Uygulamanız için bir API sağlamak için kullanılan teknoloji önemli bir seçenektir ve gRPC HTTP API'lerini karşılaştırıldığında benzersiz avantajları sunar. Bu makalede, güçlü ve zayıf gRPC birini açıklar ve diğer teknolojileri gRPC kullanma senaryoları önerir.

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

* İkili çerçeveleme ve sıkıştırma. HTTP/2 protokolüne sıkıştırılmış ve verimli hem de gönderme ve alma.
* Birden çok HTTP/2 çağrıları tek bir TCP bağlantı üzerinden çoğullama. Çoğullama ortadan [satır baş engelleme](https://en.wikipedia.org/wiki/Head-of-line_blocking).

### <a name="code-generation"></a>Kod oluşturma

Tüm gRPC çerçeveleri kod oluşturma için birinci sınıf destek sağlar. GRPC geliştirme için çekirdek dosyasıdır [ `*.proto` dosya](https://developers.google.com/protocol-buffers/docs/proto3), sözleşmenin gRPC Hizmetleri ve iletilerin tanımlar. Bu dosya gRPC çerçevelerini hizmet temel sınıfı, iletileri ve eksiksiz bir istemci kodu oluşturur.

Paylaşımı tarafından `*.proto` dosya sunucu ve istemci, iletileri ve istemci kodu arasında oluşturulabilir uçtan uca. İstemci kod üretimi iletilerin istemci ve sunucu üzerinde çoğaltma ortadan kaldırır ve kesin tür belirtilmiş bir istemci sizin için oluşturur. Bir istemci yazmak zorunda değil önemli geliştirme süresini birçok hizmet uygulamalarla kaydeder.

### <a name="strict-specification"></a>Katı belirtimi

Bir resmi belirtimi JSON ile HTTP API'si için mevcut değil. Geliştiriciler debate URL'ler, en iyi biçimi HTTP fiilleri ve yanıt kodları.

[GRPC belirtimi](https://github.com/grpc/grpc/blob/master/doc/PROTOCOL-HTTP2.md) gRPC hizmet izlemelidir biçimi hakkında açıklayıcı olduğundan. gRPC tartışılması ortadan kaldırır ve gPRC platformlar ve uygulamalar arasında tutarlı olduğu için geliştirici zaman tasarrufu sağlar.

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

## <a name="grpc-recommended-scenarios"></a>gRPC senaryolar önerilir

gRPC aşağıdaki senaryolar için uygundur:

* **Mikro Hizmetler** &ndash; gRPC olan tasarlanmış düşük gecikme süresi ve yüksek hacimli iletişim. gRPC verimliliği kritik olduğu basit mikro hizmetler için idealdir.
* **Noktadan noktaya gerçek zamanlı iletişim** &ndash; gRPC yönlü akış için mükemmel destek vardır. gRPC Hizmetleri iletileri yoklama olmadan gerçek zamanlı gönderebilir.
* **Polygot ortamları** &ndash; gRPC araçları gRPC çok dilli ortamlarda iyi bir seçim yaparak, tüm popüler geliştirme dilleri destekler.
* **Kısıtlanmış ortamları ağ** &ndash; gRPC iletileri Protobuf, bir basit bir ileti biçimine serileştirilir. GRPC ileti her zaman eşdeğer bir JSON ileti küçüktür.

## <a name="grpc-weaknesses"></a>gRPC zayıf

### <a name="limited-browser-support"></a>Sınırlı tarayıcı desteği

Doğrudan gRPC hizmeti bir tarayıcıdan bugün çağırmak mümkün değildir. gRPC HTTP/2 özellikleri yoğun olarak kullanıyorsa ve hiçbir tarayıcı web istekleri gRPC istemciyi desteklemek için gereken denetim düzeyini sağlar. Örneğin, tarayıcılar değil HTTP/2 kullanılmasını gerektiren bir çağrıyı yapanın veya temel alınan HTTP/2 çerçeveler erişim sağlar.

[gRPC Web](https://grpc.io/docs/tutorials/basic/web.html) gRPC ekibinden tarayıcıda sınırlı gRPC desteği sağlayan ek bir teknolojidir. gRPC Web iki bölümden oluşur: tüm modern tarayıcılar ve gRPC Web proxy sunucusunda destekleyen bir JavaScript istemcisi. Proxy gRPC Web istemcisi çağırır ve proxy gRPC isteklerinde gRPC sunucusuna iletir.

GRPC ait özelliklerin tümünü gRPC Web tarafından desteklenir. İstemci ve çift yönlü akış desteklenmez ve akış sunucusu için destek sınırlıdır.

### <a name="not-human-readable"></a>Değil insan tarafından okunabilir

HTTP API isteklerinin metin olarak gönderilir ve okunabilir ve insanlar tarafından oluşturuldu.

gRPC iletileri Protobuf ile varsayılan olarak kodlanır. İkili biçimi Protobuf göndermek ve almak için etkili olsa da, İnsan olmayan okunabilir. Protobuf gerektirir belirtilen ileti arabirimi açıklama `*.proto` dosya düzgün bir şekilde seri durumdan çıkarılacak. Ek araçlar Protobuf yüklerini Kablodaki analiz etmek ve istekleri el ile oluşturmak için gereklidir.

Gibi özellikleri [sunucu yansıma](https://github.com/grpc/grpc/blob/master/doc/server-reflection.md) ve [gRPC komut satırı aracını](https://github.com/grpc/grpc/blob/master/doc/command_line_tool.md) ikili Protobuf iletilerle yardımcı olmak üzere mevcut. Ayrıca, destek Protobuf iletileri [dönüştürme json'a ve json'dan](https://developers.google.com/protocol-buffers/docs/proto3#json). Yerleşik JSON dönüştürme Protobuf iletileri insan tarafından okunabilir formda gelen ve hata ayıklama sırasında dönüştürmek için etkili bir yol sağlar.

## <a name="alternative-framework-scenarios"></a>Alternatif framework senaryoları

Aşağıdaki senaryolarda gRPC üzerinden diğer çerçeveler önerilir:

* **Tarayıcı erişilebilir API'leri** &ndash; gRPC tamamen tarayıcıda desteklenmiyor. gRPC Web tarayıcısı desteği sağlayabilir, ancak sınırlamaları vardır ve bir sunucu proxy tanıtır.
* **Gerçek zamanlı iletişim yayın** &ndash; gRPC akış üzerinden gerçek zamanlı iletişim destekler, ancak iletiye kayıtlı bağlantıları için yayın kavramı yok. Örneğin burada yeni sohbet iletileri sohbet odası tüm istemcilere gönderilmesi gereken bir sohbet odası senaryosunda, tek tek istemci için yeni sohbet iletileri akışını sağlamak için her gRPC çağrı gereklidir. [SignalR](xref:signalr/introduction) bu senaryo için kullanışlı bir çerçevedir. SignalR kalıcı bağlantılar ve ileti yayınlamak için yerleşik destek kavramı vardır.
* **Arası iletişimi işlem** &ndash; gelen gRPC çağrıları kabul etmek için bir HTTP/2 sunucu ana bilgisayar bir işlemi gerekir. Windows için arası iletişimi işlem [kanallar](/dotnet/standard/io/pipe-operations) iletişim hızlı ve basit yöntemdir.

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
