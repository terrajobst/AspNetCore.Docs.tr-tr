---
title: ASP.NET core'da gRPC giriş
author: juntaoluo
description: GRPC hizmetleriyle Kestrel sunucusu ve ASP.NET Core yığını hakkında bilgi edinin.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 02/26/2019
uid: grpc/index
ms.openlocfilehash: dd1c42744bfda965df91ea1fcc0b71814317b969
ms.sourcegitcommit: a467828b5e4eaae291d961ffe2279a571900de23
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/18/2019
ms.locfileid: "58142623"
---
# <a name="introduction-to-grpc-on-aspnet-core"></a>ASP.NET core'da gRPC giriş

Tarafından [John Luo](https://github.com/juntaoluo)

[gRPC](https://grpc.io/docs/guides/) dil belirsiz, yüksek performanslı uzak yordam çağrısı (RPC) çerçevesi. GRPC temelleri hakkında daha fazla bilgi için bkz. [gRPC belgeleri sayfasını](https://grpc.io/docs/).

GRPC başlıca avantajları şunlardır:
* Modern yüksek performanslı basit RPC çerçevesi.
* Önce anlaşma API geliştirme, varsayılan dil belirsiz uygulamalar için izin, protokol arabellekleri kullanarak.
* Kesin tür belirtilmiş sunucular ve istemciler oluşturmak birçok dili için kullanılabilen araçları.
* İstemci, sunucu ve iki yönlü akış çağrılarını destekler.
* Daha düşük ağ kullanım Protobuf ikili serileştirme ile.

Bu avantajlar gRPC için ideal bir çözüm oluşturur:
* Verimliliği kritik olduğu basit bir mikro hizmetler.
* Birden çok dilde geliştirme için gerekli olduğu polyglot sistemler.
* Akış istekler veya yanıtlar işlemeniz gereken noktadan noktaya gerçek zamanlı Hizmetleri.

Ancak bir C# uygulama şu anda üzerinde resmi [gRPC sayfa](https://grpc.io/docs/quickstart/csharp.html), geçerli yerel kitaplık C'de yazılmış kullanır (gRPC [C-çekirdek](https://grpc.io/blog/grpc-stacks)). İş şu anda Kestrel HTTP sunucusu ve tam olarak yönetilen bir ASP.NET Core yığın göre yeni bir uygulama sağlamak için devam ediyor. Aşağıdaki belgeler, gRPC hizmetler bu yeni bir uygulama oluşturmaya giriş bilgileri sağlar.

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:grpc/basics>
* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/aspnetcore>
* <xref:grpc/migration>