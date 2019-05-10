---
title: Veri koruma anahtar yönetimi ve ASP.NET Core yaşam süresi
author: rick-anderson
description: Veri koruma anahtar yönetimi ve ASP.NET Core yaşam süresi hakkında bilgi edinin.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/configuration/default-settings
ms.openlocfilehash: 2f022a4c7519485fe629ce47c27d214c8c27d5bc
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64902984"
---
# <a name="data-protection-key-management-and-lifetime-in-aspnet-core"></a>Veri koruma anahtar yönetimi ve ASP.NET Core yaşam süresi

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="key-management"></a>Anahtar yönetimi

Uygulama, işletimsel ortamı algılamak ve kendi anahtar yapılandırması işlemek çalışır.

1. Uygulama içinde barındırılıyorsa [Azure uygulamaları](https://azure.microsoft.com/services/app-service/), anahtarları kaldı *%HOME%\ASP.NET\DataProtection-Keys* klasör. Bu klasör, ağ depolaması tarafından desteklenir ve uygulamayı barındıran tüm makinelerde eşitlenir.
   * Anahtarlar, bekleme sırasında korunmayan.
   * *DataProtection-Keys* klasör anahtar halkası tek dağıtım yuvasındaki bir uygulamanın tüm örnekleri sağlar.
   * Hazırlama ve üretim gibi farklı dağıtım yuvaları, anahtar halkası paylaşmayın. Örneğin hazırlık-üretim değiştirmeyi veya kullanan bir dağıtım yuvası arasında taktığınızda / B testi, veri korumayı kullanarak herhangi bir uygulama anahtarı halka önceki yuvasına kullanarak depolanan verilerin şifresini çözmek mümkün olmayacaktır. Bu, veri koruma, tanımlama bilgilerini korumak için kullandığı standart ASP.NET Core tanımlama bilgisi kimlik doğrulaması kullanan bir uygulamanın oturumunu günlüğe kaydedilmesini kullanıcılara yol açar. Yuva bağımsız anahtar halkaları isterse, Azure Blob Depolama, Azure Key Vault, bir SQL depolama gibi bir dış anahtar halkası sağlayıcısı kullanma veya Redis önbelleği.

1. Anahtarları kalıcı kullanıcı profili varsa, *%LOCALAPPDATA%\ASP.NET\DataProtection-Keys* klasör. Windows işletim sistemi olması halinde, anahtarları DPAPI kullanılarak, bekleme sırasında şifrelenir.

   Uygulama havuzunun [setProfileEnvironment özniteliği](/iis/configuration/system.applicationhost/applicationpools/add/processmodel#configuration) etkinleştirilmiş olması da gerekir. Varsayılan değer olan `setProfileEnvironment` olduğu `true`. (Örneğin, Windows işletim sistemi), bazı senaryolarda `setProfileEnvironment` ayarlanır `false`. Kullanıcı profili dizinde anahtarları depolanmaz, beklenen:

   1. Gidin *%windir%/system32/inetsrv/config* klasör.
   1. Açık *applicationHost.config* dosya.
   1. Bulun `<system.applicationHost><applicationPools><applicationPoolDefaults><processModel>` öğesi.
   1. Onaylayın `setProfileEnvironment` özniteliği mevcut olmadığında, bunun varsayılan değeri için `true`, veya özniteliğin değerini açık olarak `true`.

1. Uygulama IIS'de barındırılıyorsa, anahtarları ACLed yalnızca çalışan işlem hesabı için bir özel kayıt defteri anahtarı ' HKLM Kayıt defterine kalıcıdır. Anahtarları DPAPI kullanılarak, bekleme sırasında şifrelenir.

1. Bu koşulların hiçbiri eşleşirse, anahtarları geçerli işlemin dışında kalıcı değildir. İşlem oluşturulan tüm aşağı kapattığında anahtarları kaybolur.

Geliştirici her zaman tam denetimi ve nasıl ve anahtarları depolandığı geçersiz kılabilirsiniz. Yukarıdaki ilk üç seçenekten nasıl benzer uygulamaların çoğu için varsayılan sağlamalıdır ASP.NET  **\<machineKey >** otomatik üretimi yordamları çalışan geçmiş. Son, geri dönüş seçeneği belirlemek Geliştirici gerektiren yalnızca senaryodur [yapılandırma](xref:security/data-protection/configuration/overview) ön anahtar kalıcılığı istediği, ancak bu geri dönüş yalnızca nadir durumlarda oluşur.

Bir Docker kapsayıcısında barındırırken, anahtarları bir Docker birim (bir paylaşılan veya kapsayıcının ömründen uzun kalıcı bir ana bilgisayar bağlı biriminde) bir klasörde kalıcı veya bir dış sağlayıcı gibi [Azure anahtar kasası](https://azure.microsoft.com/services/key-vault/) veya [Redis](https://redis.io/). Bir dış sağlayıcı ayrıca uygulamaları bir paylaşılan ağ birime erişememesi durumunda web grubu senaryolarda yararlı olur. (bkz [PersistKeysToFileSystem](xref:security/data-protection/configuration/overview#persistkeystofilesystem) daha fazla bilgi için).

> [!WARNING]
> Geliştirici yukarıda özetlenen kuralları geçersiz kılar ve veri koruma sisteminde belirli bir anahtar deposunda işaret otomatik şifreleme anahtarlarının bekleyen devre dışı bırakılır. Bekleyen koruma aracılığıyla yeniden etkin olabilir [yapılandırma](xref:security/data-protection/configuration/overview).

## <a name="key-lifetime"></a>Anahtar yaşam süresi

Anahtarları, varsayılan olarak 90 günlük bir ömrü vardır. Bir anahtar geçerlilik süresi dolduğunda, uygulama, otomatik olarak yeni bir anahtar oluşturur ve yeni anahtar etkin anahtar ayarlar. Devre dışı bırakılan anahtarları sistemde kaldığı sürece, uygulamanız ile korunan tüm verileri şifresini çözebilir. Bkz: [anahtar yönetimi](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling) daha fazla bilgi için.

## <a name="default-algorithms"></a>Varsayılan algoritmaları

Kullanılan varsayılan yükü koruma algoritması AES-256-CBC, gizliliği ve HMACSHA256 kimlik doğrulaması için içindir. Her 90 günde değiştirilmiş bir 512 bit ana anahtar, bu algoritmalar yükü başına temelinde kullanılan iki alt anahtar türetme kullanılır. Bkz: [alt anahtarını türetme](xref:security/data-protection/implementation/subkeyderivation#additional-authenticated-data-and-subkey-derivation) daha fazla bilgi için.

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:security/data-protection/extensibility/key-management>
* <xref:host-and-deploy/web-farm>
