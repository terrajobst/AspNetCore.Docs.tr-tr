---
title: SignalR API tasarım konuları
author: anurse
description: SignalR API'leri, uygulamanıza sürümleri arasında uyumluluk için tasarlamayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 11/06/2018
uid: signalr/api-design
ms.openlocfilehash: 3f17bf055b793e8fc91fbcc15f668928ca261f77
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64903182"
---
# <a name="signalr-api-design-considerations"></a>SignalR API tasarım konuları

Tarafından [Andrew Stanton-Nurse](https://twitter.com/anurse)

Bu makalede, SignalR tabanlı API'leri oluşturmaya yönelik rehberlik sağlar.

## <a name="use-custom-object-parameters-to-ensure-backwards-compatibility"></a>Geriye dönük uyumluluk sağlamak için özel nesne parametreleri kullanma

Bir SignalR hub'yöntemini (istemci veya sunucu) parametreleri ekleyerek olan bir *değişiklik*. Başka bir deyişle, eski istemciler/sunucuları parametreleri uygun sayıda olmayan bir yöntem çağırmak çalıştığınızda bir hata almazsınız. Ancak, bir özel nesne parametresi için Özellikler ekleme olan **değil** bir değişiklik. Bu, istemci veya Sunucu'da değişikliklere dayanıklı uyumlu API'leri tasarlamak için kullanılabilir.

Örneğin, aşağıdaki gibi bir sunucu tarafı API göz önünde bulundurun:

[!code-csharp[ParameterBasedOldVersion](api-design/sample/Samples.cs?name=ParameterBasedOldVersion)]

JavaScript istemcisi kullanarak bu yöntemi çağıran `invoke` gibi:

[!code-typescript[CallWithOneParameter](api-design/sample/Samples.ts?name=CallWithOneParameter)]

Eski istemciler, sunucu yöntemi daha sonra ikinci bir parametre ekleyin, bu parametre değeri sağlamaz. Örneğin:

[!code-csharp[ParameterBasedNewVersion](api-design/sample/Samples.cs?name=ParameterBasedNewVersion)]

Eski istemci, bu yöntem çağırmak çalıştığında, şunun gibi bir hata alırsınız:

```
Microsoft.AspNetCore.SignalR.HubException: Failed to invoke 'GetTotalLength' due to an error on the server.
```

Sunucuda böyle bir günlük iletisi görürsünüz:

```
System.IO.InvalidDataException: Invocation provides 1 argument(s) but target expects 2.
```

Eski istemci yalnızca bir parametreye gönderilen, ancak yeni sunucu API iki parametre gerekli. Parametre olarak özel nesneler kullanarak daha fazla esneklik sağlar. Şimdi yeniden tasarlamanız özgün API'yi özel bir nesne kullanın:

[!code-csharp[ObjectBasedOldVersion](api-design/sample/Samples.cs?name=ObjectBasedOldVersion)]

Şimdi, istemci yöntemini çağırmak için bir nesne kullanır:

[!code-typescript[CallWithObject](api-design/sample/Samples.ts?name=CallWithObject)]

Bir parametre eklemek yerine, özellik eklemek `TotalLengthRequest` nesnesi:

[!code-csharp[ObjectBasedNewVersion](api-design/sample/Samples.cs?name=ObjectBasedNewVersion&highlight=4,9-13)]

Eski istemci, tek bir parametre, ek gönderdiğinde `Param2` özellik sol `null`. Kontrol ederek daha eski bir istemci tarafından gönderilen bir iletinin algılayabilir `Param2` için `null` ve varsayılan değer geçerlidir. Yeni bir istemci, her iki parametre gönderebilirsiniz.

[!code-typescript[CallWithObjectNew](api-design/sample/Samples.ts?name=CallWithObjectNew)]

İstemci üzerinde tanımlanan yöntemleri için aynı tekniği çalışır. Sunucu tarafındaki özel bir nesne gönderebilirsiniz:

[!code-csharp[ClientSideObjectBasedOld](api-design/sample/Samples.cs?name=ClientSideObjectBasedOld)]

İstemci tarafında size erişim `Message` bir parametre kullanmak yerine özelliği:

[!code-typescript[OnWithObjectOld](api-design/sample/Samples.ts?name=OnWithObjectOld)]

İletiyi gönderen yüküne eklemek üzere daha sonra karar verirseniz, nesnenin bir özellik ekleyin:

[!code-csharp[ClientSideObjectBasedNew](api-design/sample/Samples.cs?name=ClientSideObjectBasedNew&highlight=5)]

Eski istemciler bekleniyor olmaz `Sender` bunu yoksaymak şekilde değeri. Yeni bir istemci yeni özelliği okunamıyor güncelleştirerek kabul edebilirsiniz:

[!code-typescript[OnWithObjectNew](api-design/sample/Samples.ts?name=OnWithObjectNew&highlight=2-5)]

Bu durumda, yeni istemci de sağlamaz eski bir sunucunun dayanıklıdır `Sender` değeri. Eski sunucu sağlamayacak beri `Sender` değeri, istemci erişmeden önce olup olmadığını görmek için denetler.
