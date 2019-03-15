---
ms.openlocfilehash: 1f8d3913c83aaf5fe6ec2cec482a30f0f066c16b
ms.sourcegitcommit: 34bf9fc6ea814c039401fca174642f0acb14be3c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/14/2019
ms.locfileid: "57841699"
---

> [!NOTE]
> Bu öğretici için Entity Framework Core kullanan *geçişler* mümkün olduğunca özelliği. Geçişler, veri modelindeki değişikliklerle eşleştirmek için veritabanı şemasını güncelleştirir. Ancak, geçişler yalnızca tür EF Core sağlayıcının desteklediği değişiklikleri yapabilirsiniz ve SQLite sağlayıcısının özellikleri sınırlıdır. Örneğin, sütun ekleme desteklenir, ancak kaldırmayı veya sütunu değiştirmek desteklenmiyor. Bir sütunu değiştirme veya kaldırmak için bir geçiş oluşturduysanız `ef migrations add` komut başarılı ancak `ef database update` komutu başarısız oluyor. Bu sınırlamaları nedeniyle, Bu öğretici için SQLite şema değişiklikleri geçişleri kullanmaz. Bunun yerine, şema değiştiğinde, bırak ve veritabanını yeniden oluşturun.
>
>SQLite sınırlamaları için geçici bir tablo yeniden oluşturma şeyler olduğunda tablo değişiklikleri gerçekleştirmek için geçişler kod el ile yazmaktır. Bir tablo yeniden oluşturma içerir:
>
>* Varolan bir tabloyu yeniden adlandırma.
>* Yeni bir tablo oluşturuluyor.
>* Verileri yeni tabloya eski tablodan kopyalama.
>* Eski tablosu bırakılıyor.
>
>Daha fazla bilgi için aşağıdaki kaynaklara bakın:
>
> * [SQLite EF Core veritabanı sağlayıcısı sınırlamaları](/ef/core/providers/sqlite/limitations)
> * [Geçiş kodu özelleştirme](/ef/core/managing-schemas/migrations/#customize-migration-code)
> * [Veri çekirdeği oluşturma](/ef/core/modeling/data-seeding)