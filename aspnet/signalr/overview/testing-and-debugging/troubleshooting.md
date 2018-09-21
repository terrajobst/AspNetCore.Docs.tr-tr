---
uid: signalr/overview/testing-and-debugging/troubleshooting
title: SignalR sorunlarını giderme | Microsoft Docs
author: pfletcher
description: Bu makalede, SignalR uygulamaları geliştirme ile yaygın sorunları açıklar.
ms.author: riande
ms.date: 06/10/2014
ms.assetid: 4b559e6c-4fb0-4a04-9812-45cf08ae5779
msc.legacyurl: /signalr/overview/testing-and-debugging/troubleshooting
msc.type: authoredcontent
ms.openlocfilehash: 9ef69ece2558260f0409e9388751f77ac84a06f1
ms.sourcegitcommit: c12ebdab65853f27fbb418204646baf6ce69515e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/21/2018
ms.locfileid: "46523083"
---
<a name="signalr-troubleshooting"></a>SignalR sorunlarını giderme
====================
tarafından [Patrick Fletcher](https://github.com/pfletcher)

> Bu belge, SignalR ile ortak sorun giderme konularını açıklar.
> 
> ## <a name="software-versions-used-in-this-topic"></a>Bu konu başlığında kullanılan yazılım sürümleri
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR sürüm 2
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a>Bu konunun önceki sürümleri
> 
> SignalR eski sürümleri hakkında daha fazla bilgi için bkz: [SignalR eski sürümleri](../older-versions/index.md).
> 
> ## <a name="questions-and-comments"></a>Sorularınız ve yorumlarınız
> 
> Lütfen bu öğreticide sevmediğinizi nasıl ve ne sayfanın alt kısmındaki açıklamalarda geliştirebileceğimiz hakkında geri bildirim bırakın. Öğretici için doğrudan ilgili olmayan sorularınız varsa, bunları gönderebilir [ASP.NET SignalR Forumu](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) veya [StackOverflow.com](http://stackoverflow.com/).


Bu belge aşağıdaki bölümleri içerir.

- [Yöntemleri istemci ve sunucu arasındaki sessizce başarısız çağırma](#connection)
- [Etkin olmayan istemci algılamak için ping/pong için IIS websockets yapılandırma](#pong)
- [Diğer bağlantı sorunları](#other)
- [Derleme ve sunucu tarafı hataları](#server)
- [Visual Studio sorunları](#vs)
- [Internet Information Services sorunları](#iis)
- [Microsoft Azure sorunları](#azure)

<a id="connection"></a>

## <a name="calling-methods-between-the-client-and-server-silently-fails"></a>Yöntemleri istemci ve sunucu arasındaki sessizce başarısız çağırma

Bu bölümde, bir yöntem çağrısının başarısız anlamlı bir hata iletisi, istemci ve sunucu arasındaki olası nedenleri açıklanmaktadır. Bir SignalR uygulamasında, sunucunun istemci uygulayan yöntemler hakkında bir bilgi bulunmaz; Sunucunun istemci yöntemini çağırdığında, istemciye gönderilen yöntemi adı ve parametre veri ve yalnızca sunucu belirtilen biçimde varsa yöntemi yürütülür. Varsa eşleşen hiçbir yöntemi hiçbir şey olmaz, istemcide bulunan ve hiçbir hata iletisi, sunucu üzerinde oluşturulur.

İstemci yöntemleri çağrılmazsa daha fazla araştırmak için başlangıç yöntemin hangi çağrıları görmek için hub'ında, sunucudan geliyor çağırmadan önce günlüğü etkinleştirebilirsiniz. Bir JavaScript uygulamasında günlüğe kaydetmeyi etkinleştirmek için bkz: [istemci tarafı günlüğe kaydetme (JavaScript istemci sürümü) etkinleştirme](../guide-to-the-api/hubs-api-guide-javascript-client.md#logging). Bir .NET istemci uygulamasında günlüğe kaydetmeyi etkinleştirmek için bkz: [istemci tarafı günlüğe kaydetme (.NET istemci sürümü) etkinleştirme](../guide-to-the-api/hubs-api-guide-net-client.md#logging).

### <a name="misspelled-method-incorrect-method-signature-or-incorrect-hub-name"></a>Yanlış yazılmış yöntemi, hatalı yöntem imzası veya yanlış hub'ı adı

İstemci üzerinde uygun bir yöntem adı ya da çağrılan yöntemin imzası tam olarak eşleşmiyor, çağrı başarısız olur. Sunucu tarafından adlandırılan yöntem adını istemcideki yöntemin adı eşleştiğini doğrulayın. Ayrıca, SignalR hub proxy başlamalıdır yöntemlerle oluşturur JavaScript'te uygun olduğundan, bu nedenle bir yöntem adlı `SendMessage` sunucu üzerinde çağrılır `sendMessage` istemci proxy'de. Kullanırsanız `HubName` özniteliği sunucu tarafı kodunuzda, kullanılan adını istemcide hub'ı oluşturmak için kullanılan adı ile eşleştiğini doğrulayın. Kullanmıyorsanız, `HubName` öznitelik, bir JavaScript istemci hub'ı adını başlamalıdır, chatHub ChatHub yerine gibi olduğunu doğrulayın.

### <a name="duplicate-method-name-on-client"></a>İstemci üzerinde yinelenen yöntem adı

Yinelenen bir yöntem yalnızca büyük küçük harfle farklı istemci üzerinde olmadığını doğrulayın. İstemci uygulamanızın adlı yöntemi varsa `sendMessage`, adında bir yöntem de, hiç doğrulayın `SendMessage` de.

### <a name="missing-json-parser-on-the-client"></a>İstemci üzerinde eksik JSON ayrıştırıcı

SignalR, bir JSON ayrıştırıcı, sunucu ve istemci arasındaki çağrıların seri hale getirmek için mevcut olmasını gerektirir. İstemci (örneğin, Internet Explorer 7) yerleşik bir JSON Ayrıştırıcı yoksa, bir uygulama eklemek gerekir. JSON ayrıştırıcının indirebileceğiniz [burada](http://nuget.org/packages/json2).

### <a name="mixing-hub-and-persistentconnection-syntax"></a>Hub ve PersistentConnection sözdiziminin karışık kullanımına

SignalR iki iletişim modeli kullanır: hub'ları ve PersistentConnections. Bu iki iletişim modeller çağırma söz dizimi, istemci kodu farklıdır. Sunucu kodunuzdaki bir hub eklediyseniz, istemci kodunuzun tamamını kullandığını hub'ı uygun söz dizimini doğrulayın.

**JavaScript istemci olarak bir PersistentConnection oluşturan JavaScript istemci kodu**

[!code-javascript[Main](troubleshooting/samples/sample1.js)]

**Bir Javascript istemci bir Hub Proxy oluşturan JavaScript istemci kodu**

[!code-javascript[Main](troubleshooting/samples/sample2.js)]

**Bir rota eşleyen bir PersistentConnection için C# sunucu kodu**

[!code-csharp[Main](troubleshooting/samples/sample3.cs)]

**Birden çok uygulamalarınız varsa, Hub'ına veya birden çok hubs'a bir rotayı eşler C# sunucu kodu**

[!code-css[Main](troubleshooting/samples/sample4.css)]

### <a name="connection-started-before-subscriptions-are-added"></a>Abonelikleri eklemeden önce başlatılan bağlantı

Hub'ın bağlantı için proxy sunucudan çağrılabilir yöntemleri eklenmeden önce başlatılırsa, iletileri alınmaz. Aşağıdaki JavaScript kodunu hub düzgün bir şekilde başlar:

**Alınacak hub'ları iletileri izin vermez yanlış JavaScript istemci kodu**

[!code-javascript[Main](troubleshooting/samples/sample5.js)]

Bunun yerine, başlangıç çağırmadan önce yöntemi abonelik ekleyin:

**Doğru bir şekilde bir hub'ına abonelikleri ekleyen JavaScript istemci kodu**

[!code-javascript[Main](troubleshooting/samples/sample6.js)]

### <a name="missing-method-name-on-the-hub-proxy"></a>Hub proxy yöntemi adı eksik

Sunucuda tanımlanmış yöntemi istemcide abone olduğu olduğunu doğrulayın. Sunucu yöntemi tanımlar olsa bile, yine de için istemci proxy eklenmelidir. Yöntemleri eklenebilir istemci Ara sunucuya aşağıdaki yollarla (yöntemi eklenen Not `client` hub'ı değil hub doğrudan üyesi):

**Hub proxy için yöntemler ekleyen JavaScript istemci kodu**

[!code-javascript[Main](troubleshooting/samples/sample7.js)]

### <a name="hub-or-hub-methods-not-declared-as-public"></a>Hub veya hub yöntemlerine genel olarak bildirilmedi

İstemcide görünür olması için hub uygulama ve yöntemler olarak bildirilmelidir `public`.

### <a name="accessing-hub-from-a-different-application"></a>Hub'ı farklı bir uygulamadan erişme

SignalR hub'ları yalnızca SignalR istemciler kullanan uygulamalar erişilebilir. Diğer iletişim kitaplıklarını (örneğin, SOAP ve WCF web hizmetlerini.) ile SignalR işleyemez Hedef platform için kullanılabilir hiçbir SignalR istemcisi varsa, sunucu uç noktası doğrudan erişemez.

### <a name="manually-serializing-data"></a>El ile verileri seri hale getirme

SignalR otomatik olarak JSON yönteminizi seri hale getirmek için kendi başınıza parametreleri-burada ait gerek kullanır.

### <a name="remote-hub-method-not-executed-on-client-in-ondisconnected-function"></a>İstemci OnDisconnected işlevindeki yürütülmedi uzak Hub yöntemi

Bu davranış tasarım gereğidir. Zaman `OnDisconnected` olan çağrılır, hub'ı zaten geçtiğini `Disconnected` hub yöntemlerine yönelik daha fazla izin vermeyen durumu.

**Doğru OnDisconnected olayda kodu yürüten C# sunucu kodu**

[!code-csharp[Main](troubleshooting/samples/sample8.cs)]

### <a name="ondisconnect-not-firing-at-consistent-times"></a>OnDisconnect tutarlı zamanlarda tetikleme değil

Bu davranış tasarım gereğidir. Bir kullanıcı bir sayfa etkin bir SignalR bağlantısı ile uzakta gitmek çalıştığında, SignalR istemci server istemci bağlantısı durdurulacak bildirmek için mümkün olan en iyi girişim hale getirir. SignalR istemcisi mümkün olan en iyi denemesi başarısız olur tarafından sunucuya ulaşmak, yapılandırılabilir bir sonra bağlantının sunucu de dispose `DisconnectTimeout` daha sonra hangi zamanında `OnDisconnected` olayını ateşle. SignalR istemcisi mümkün olan en iyi girişim başarılı ise `OnDisconnected` olay yangın hemen.

Ayarı hakkında bilgi için `DisconnectTimeout` ayar bkz [bağlantı ömrü olaylarını işleme: DisconnectTimeout](../guide-to-the-api/handling-connection-lifetime-events.md#disconnecttimeout).

### <a name="connection-limit-reached"></a>Bağlantı sınırına ulaşıldı

Windows 7 gibi bir istemci işletim sisteminde IIS tam sürümünü kullanırken, 10 bağlantı sınırı uygulanmaktadır. Bir istemci işletim sistemi kullanırken, IIS Express yerine bu sınırı önlemek için kullanın.

### <a name="cross-domain-connection-not-set-up-properly"></a>Etki alanları arası bağlantı düzgün şekilde ayarlanmamış

Etki alanları arası bağlantı varsa (bir ağ bağlantısı için SignalR URL değil barındırma sayfası aynı etki alanında) doğru şekilde ayarlanmamışsa, bir hata iletisi bağlantı başarısız olabilir. Etki alanları arası iletişimi etkinleştirme hakkında daha fazla bilgi için bkz: [etki alanları arası bağlantı kurmak nasıl](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain).

### <a name="connection-using-ntlm-active-directory-not-working-in-net-client"></a>.NET istemci çalışmıyor NTLM (Active Directory) kullanarak sanal ağa bağlantı

Bağlantının düzgün şekilde yapılandırılmadıysa, etki alanı güvenlik kullanan bir .NET istemci uygulamasındaki bir bağlantı başarısız olabilir. SignalR, bir etki alanı ortamında kullanmak için gerekli bağlantı özelliği şu şekilde ayarlayın:

**Bağlantı kimlik bilgilerini uygulayan C# istemci kodu**

[!code-csharp[Main](troubleshooting/samples/sample9.cs)]

<a id="pong"></a>

## <a name="configuring-iis-websockets-to-pingpong-to-detect-a-dead-client"></a>Etkin olmayan istemci algılamak için ping/pong için IIS websockets yapılandırma

İstemci kullanılmayan olup ve şirketler bağlantı hataları için temel alınan websocket bildirimden diğer bir deyişle güvenmektedir SignalR sunucuları bilinmiyor, `OnClose` geri çağırma. Bu soruna bir çözüm ping/pong sizin için gerçekleştirmesini istemeniz IIS websockets yapılandırmaktır. Bu, beklenmedik bir şekilde keserse bağlantınızı kapatın sağlar. Daha fazla bilgi için [stackoverflow yazıya](http://stackoverflow.com/questions/19502755/websocket-clients-state-not-changing-on-network-loss).

<a id="other"></a>

## <a name="other-connection-issues"></a>Diğer bağlantı sorunları

Bu bölümde, nedenleri ve çözümleri belirli belirtileri veya bağlantı sırasında oluşan hata iletileri açıklanmaktadır.

### <a name="start-must-be-called-before-data-can-be-sent-error"></a>"Veri gönderilmeden önce başlangıç çağrılmalıdır" hatası

Kod SignalR nesneleri başvuruyorsa bağlantı başlatılmadan önce bu hatanın yaygın olarak görülür. İşleyicileri ve benzeri için iliştirdiği bağlantı tamamlandıktan sonra sunucuda tanımlı yöntemlerini eklenecek gerekir. Unutmayın çağrısı `Start` tamamlandıktan sonra önce çağrı çalıştırılabilir kod şekilde uyumsuzdur. Bir bağlantı tamamen başlatıldıktan sonra işleyicileri eklemek için en iyi başlangıç yöntemine bir parametre olarak geçirilen bir geri çağırma işlevini yerleştirmenin yoludur:

**SignalR nesnelerine başvuru olay işleyicileri doğru ekleyen JavaScript istemci kodu**

[!code-javascript[Main](troubleshooting/samples/sample10.js?highlight=1)]

Bu hata ayrıca SignalR nesneler hala başvurulmayan ederken bir bağlantı durduğunda görülür.

### <a name="301-moved-permanently-or-302-moved-temporarily-error"></a>"301 kalıcı olarak taşındı" veya "302 geçici olarak taşındı" hatası

Bu hata, proje ile otomatik olarak oluşturulan proxy müdahale SignalR, adlı bir klasör içeriyorsa görülebilir. Bu hatayı önlemek için adlı bir klasör kullanmayın `SignalR` , uygulama ya da devre dışı bırakma otomatik proxy oluşturma. Bkz: [oluşturulan Proxy ve sizin için yaptığı](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy) daha fazla ayrıntı için.

### <a name="403-forbidden-error-in-net-or-silverlight-client"></a>.NET veya Silverlight İstemcisi'nde "403 Yasak" hatası

Burada etki alanları arası iletişimin düzgün şekilde etkinleştirilmedi etki alanları arası ortamlarda bu hata oluşabilir. Etki alanları arası iletişimi etkinleştirme hakkında daha fazla bilgi için bkz: [etki alanları arası bağlantı kurmak nasıl](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain). Bir Silverlight İstemcisi'nde etki alanları arası bağlantı kurmak için bkz: [Silverlight istemcileri etki alanları arası bağlantılara](../guide-to-the-api/hubs-api-guide-net-client.md#slcrossdomain).

### <a name="404-not-found-error"></a>"404 bulunamadı" hatası

Bu sorunun çeşitli nedenleri vardır. Aşağıdakilerin tümü doğrulayın:

- **Hub proxy adresi başvurusu düzgün biçimlendirilmemiş:** oluşturulan hub proxy adresi başvurusu doğru biçimlendirilmemiş varsa bu hatanın yaygın olarak görülür. Hub adresine başvuru düzgün yapıldığını doğrulayın. Bkz: [nasıl dinamik olarak oluşturulan proxy başvuru](../guide-to-the-api/hubs-api-guide-javascript-client.md#dynamicproxy) Ayrıntılar için.
- **Hub rotasını eklemeden önce uygulama için yollar ekleme:** uygulamanız başka bir yolun kullanıyorsa, eklenen ilk rota çağrısı olduğunu doğrulayın `MapSignalR`.
- **IIS 7 ya da güncelleştirme olmadan 7.5 uzantısız URL'lerle ilgili kullanma:** kullanarak IIS 7 veya 7.5 gerektiren bir güncelleştirme uzantısız URL'lerle ilgili sunucu hub tanımlarını erişim sağlayabilmesi `/signalr/hubs`. Güncelleştirme bulunabilir [burada](https://support.microsoft.com/kb/980368).
- **IIS önbelleğe güncel değil veya bozuk:** önbellek içeriği güncel olmadığını doğrulamak için önbelleği temizlemek için bir PowerShell penceresinde aşağıdaki komutu girin:

    [!code-powershell[Main](troubleshooting/samples/sample11.ps1)]

### <a name="500-internal-server-error"></a>"500 İç sunucu hatası"

Çok çeşitli nedenleri olabilir çok genel bir hatadır. Hatanın ayrıntıları sunucunun olay günlüğüne görünmelidir veya sunucunun hata ayıklama aracılığıyla bulunabilir. Daha ayrıntılı hata bilgileri, sunucu üzerindeki ayrıntılı hatalar açarak alınabilir. Daha fazla bilgi için [Hub sınıfına hataları işlemek nasıl](../guide-to-the-api/hubs-api-guide-server.md#handleErrors).

Bu hata, bir güvenlik duvarı veya proxy düzgün bir şekilde yazılması istek üst bilgilerini neden yapılandırılmamışsa, yaygın olarak görülür. Çözüm, 80 numaralı bağlantı noktası güvenlik duvarı veya proxy etkin olduğundan emin olmaktır.

### <a name="unexpected-response-code-500"></a>"Beklenmeyen bir yanıt kodu: 500"

Uygulamada kullanılan .NET framework sürümü, Web.Config dosyasında belirtilen sürümü eşleşmiyor Bu hata oluşabilir. Çözüm, .NET 4.5, uygulama ayarları ve Web.Config dosyasını kullanıldığını doğrulamaktır.

### <a name="typeerror-lthubtypegt-is-undefined-error"></a>"TypeError: &lt;hubType&gt; tanımlanmamış" hatası

Bu hataya neden olur çağrısı `MapSignalR` düzgün yapılmaz. Bkz: [SignalR ara yazılım kaydetmek ve SignalR seçeneklerini yapılandırmak nasıl](../guide-to-the-api/hubs-api-guide-server.md#route) daha fazla bilgi için.

### <a name="jsonserializationexception-was-unhandled-by-user-code"></a>JsonSerializationException kullanıcı kodu ile işlenmedi

Doğrulamak için yöntemlerinizi gönderdiğiniz parametreleri seri hale getirilemeyen türleri (örneğin, dosya tanıtıcıları veya veritabanı bağlantıları) içermez. İstemci (veya güvenlik için serileştirme nedeniyle), kullanım gönderilmesini istemiyorsanız bir sunucu tarafı nesne üyeleri kullanmanız gerekiyorsa `JSONIgnore` özniteliği.

### <a name="protocol-error-unknown-transport-error"></a>"Protokol hatası: Bilinmeyen aktarım" hatası

İstemci SignalR kullanan taşımalar desteklemiyorsa bu hata oluşabilir. Bkz: [aktarım ve geri dönüşler](../getting-started/introduction-to-signalr.md#transports) hangi tarayıcılar kullanılabilir SignalR ile bilgi.

### <a name="javascript-hub-proxy-generation-has-been-disabled"></a>"JavaScript Hub proxy oluşturma devre dışı bırakıldı."

Bu hata oluşacaktır `DisableJavaScriptProxies` dinamik olarak oluşturulan proxy için bir başvuru da dahil olmak üzere sırasında ayarlanır `signalr/hubs`. Proxy el ile oluşturma hakkında daha fazla bilgi için bkz. [oluşturulan proxy ve sizin için yaptığı](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy).

### <a name="the-connection-id-is-in-the-incorrect-format-or-the-user-identity-cannot-change-during-an-active-signalr-connection-error"></a>"Bağlantı kimliği yanlış biçimde" veya "kullanıcı kimliği etkin bir SignalR bağlantısı sırasında değiştiremezsiniz" hatası

Bu hata, kimlik doğrulaması kullanılır ve istemci bağlantı durdurulmadan önce kaydedilir görülebilir. İstemci oturumu kapatmak önce SignalR bağlantı durdurma çözümdür.

### <a name="uncaught-error-signalr-jquery-not-found-please-ensure-jquery-is-referenced-before-the-signalrjs-file-error"></a>"Yakalanmayan hata: SignalR: jQuery nebyl nalezen. JQuery önce SignalR.js dosyasını başvurulduğundan emin olun"hatası

SignalR JavaScript istemci çalıştırmak için jQuery gerektirir. JQuery yönelik başvurunuz kullanılan yolun geçerli olduğunu ve jQuery başvuru SignalR başvurusu önce olduğunu doğru olduğunu doğrulayın.

### <a name="uncaught-typeerror-cannot-read-property-ltpropertygt-of-undefined-error"></a>"Yakalanmamış TypeError: özelliği okunamıyor '&lt;özelliği&gt;' undefined'ın" hatası

JQuery veya düzgün başvurulan hub proxy kalmamasını değil Bu hata oluşur. Başvurunuz jQuery ve hub proxy için kullanılan yolun geçerli olduğunu ve jQuery başvurusunu başvuru hub proxy için önce olduğunu doğru olduğundan emin olun. Hub proxy için varsayılan başvuru aşağıdaki gibi görünmelidir:

**HTML istemci tarafı hub proxy doğru başvuran bir kod**

[!code-html[Main](troubleshooting/samples/sample12.html)]

### <a name="runtimebinderexception-was-unhandled-by-user-code-error"></a>"RuntimeBinderException kullanıcı kodu ile işlenmedi" hatası

Bu hata oluşabilir, yanlış aşırı yüklemesini `Hub.On` kullanılır. Yöntemin dönüş değeri varsa, dönüş türü bir genel tür parametresi belirtilmesi gerekir:

**İstemci (olmadan oluşturulan proxy) üzerinde tanımlanan yöntemi**

[!code-html[Main](troubleshooting/samples/sample13.html?highlight=1)]

### <a name="connection-id-is-inconsistent-or-connection-breaks-between-page-loads"></a>Bağlantı kimliği tutarsız veya sayfa yüklemelerinin bağlantıyı keser

Bu davranış tasarım gereğidir. Hub nesnesi sayfa nesnesinde barındırıldığından, Sayfa yenilendiğinde hub yok edilir. Bunlar sayfa yüklemelerinin arasında tutarlı olması kullanıcıların bağlantı kimlikleri arasındaki ilişkiyi korumak çok sayfalı bir uygulamada gerekir. Bağlantı kimlikleri ya da sunucuda depolanan bir `ConcurrentDictionary` nesnesi veya bir veritabanı.

### <a name="value-cannot-be-null-error"></a>Hata "Değer null olamaz"

İsteğe bağlı parametreler içeren sunucu tarafı yöntemlerinin şu anda desteklenmiyor; İsteğe bağlı parametresi atlanırsa, yöntem başarısız olur. Daha fazla bilgi için [isteğe bağlı parametreler](https://github.com/SignalR/SignalR/issues/324).

### <a name="firefox-cant-establish-a-connection-to-the-server-at-ltaddressgt-error-in-firebug"></a>"Firefox sunucusuna bir bağlantı kuramıyor &lt;adresi&gt;" Firebug hatası

Anlaşma WebSocket taşıma başarısız olur ve bunun yerine başka bir aktarım kullanılıyorsa bu hata iletisini Firebug içinde görülebilir. Bu davranış tasarım gereğidir.

### <a name="the-remote-certificate-is-invalid-according-to-the-validation-procedure-error-in-net-client-application"></a>.NET istemci uygulamasındaki "doğrulama yordamına göre uzak sertifika geçersiz" hatası

Sunucunuz, özel bir istemci sertifikası gerektiriyorsa, isteği yapılmadan önce sonra bir x509certificate bağlantı ekleyebilirsiniz. Sertifikayı kullanarak bağlantı eklemek `Connection.AddClientCertificate`.

### <a name="connection-drops-after-authentication-times-out"></a>Bağlantı kimlik doğrulaması zaman aşımına sonra bırakır

Bu davranış tasarım gereğidir. Kimlik doğrulama kimlik bilgileri, bir bağlantı etkin durumdayken değiştirilemez; kimlik bilgilerini yenilemek için bağlantı durdurulup yeniden gerekir.

### <a name="onconnected-gets-called-twice-when-using-jquery-mobile"></a>OnConnected iki kez jQuery Mobile'ı kullanırken uğradığında çağrılır

jQuery Mobile's `initializePage` işlevi, böylece ikinci bir bağlantı oluşturma betikleri yeniden yürütülmesi için her bir sayfasında zorlar. Bu soruna yönelik çözümler içerir:

- JQuery Mobile JavaScript dosyanıza önce başvurusunu içerir.
- Devre dışı `initializePage` ayarlayarak işlevi `$.mobile.autoInitializePage = false`.
- Sayfa başlatma bağlantı başlatmadan önce tamamlanması için bekleyin.

### <a name="messages-are-delayed-in-silverlight-applications-using-server-sent-events"></a>İletileri sunucu gönderilen olayları kullanarak Silverlight uygulamalarında gecikiyor

Sunucu kullanarak olayları Silverlight'ın gönderilen iletileri gecikir. Uzun yoklama yerine kullanılacak zorlamak için aşağıdaki bağlantı başlatırken kullanın:

[!code-css[Main](troubleshooting/samples/sample14.css)]

### <a name="permission-denied-using-forever-frame-protocol"></a>"İzin reddedildi" kullanarak sürekli çerçeve Protokolü

Açıklanan bilinen bir sorun budur [burada](https://github.com/SignalR/SignalR/issues/1963). En son JQuery kitaplığını kullanarak bu belirti görülebilir; JQuery 1.8.2 uygulamanıza düşürmek çözüm olabilir.

### <a name="invalidoperationexception-not-a-valid-web-socket-request"></a>"InvalidOperationException: geçerli bir web yuvası isteğini.

Bu hata WebSocket protokolü kullanılır, ancak istek üstbilgilerini ağ proxy değiştiriyor ortaya çıkabilir. Proxy bağlantı noktası 80 üzerinde WebSocket izin verecek şekilde yapılandırmak için kullanılan çözümüdür.

### <a name="exception-ltmethod-namegt-method-could-not-be-resolved-when-client-calls-method-on-server"></a>"Özel durum: &lt;yöntem adı&gt; yöntemi çözümlenemedi" İstemci sunucuda yöntemi çağırdığında

Bu hata, dizi gibi bir JSON yükü bulunamıyorsa veri türleri kullanarak neden olabilir. Geçici çözüm, JSON, IList gibi tarafından keşfedilebilir olan bir veri türü kullanmaktır. Daha fazla bilgi için [.NET istemcinin dizisine parametreli hub yöntemlerini çağırma](https://github.com/SignalR/SignalR/issues/2672).

<a id="server"></a>

## <a name="compilation-and-server-side-errors"></a>Derleme ve sunucu tarafı hataları

 Aşağıdaki bölümde, derleyici ve sunucu tarafı çalışma zamanı hataları için olası çözümleri içerir. 

### <a name="reference-to-hub-instance-is-null"></a>Hub örneği null başvurudur

Her bağlantı için hub örneği oluşturulduğundan, hub örneği, kodunuzda oluşturma olamaz. Hub dışında aynı hub'da yöntemleri çağırmak için bkz: [istemci yöntemleri çağırmak ve Hub sınıfına dışındaki grupları yönetmek nasıl](../guide-to-the-api/hubs-api-guide-server.md#callfromoutsidehub) için hub bağlamını başvuru elde etme.

### <a name="httpcontextcurrentsession-is-null"></a>HTTPContext.Current.Session null

Bu davranış tasarım gereğidir. Oturum durumunu etkinleştirme çift yönlü Mesajlaşma kesmek beri SignalR ASP.NET oturum durumu desteklemez.

### <a name="no-suitable-method-to-override"></a>Geçersiz kılmak için uygun yöntem

Eski belgelere veya blog koddan kullanıyorsanız bu hatayı görebilirsiniz. Adları değiştirildi veya kullanım dışı yöntemler başvuran değil doğrulayın (gibi `OnConnectedAsync`).

### <a name="hostcontextextensionswebsocketserverurl-is-null"></a>HostContextExtensions.WebSocketServerUrl null

Bu davranış tasarım gereğidir. Bu üye, kullanım dışıdır ve kullanılmamalıdır.

### <a name="a-route-named-signalrhubs-is-already-in-the-route-collection-error"></a>"'Signalr.hubs' adlı bir rota rota koleksiyonu içinde zaten" hatası

Bu hata görülen `MapSignalR` iki kez uygulamanız tarafından çağrılır. Bazı örnek uygulamalar arama `MapSignalR` doğrudan başlangıç sınıfında; başkalarının yaptığı çağrısı içinde bir sarmalayıcı sınıftır. Uygulamanızı hem yapmaz emin olun.

### <a name="websocket-is-not-used"></a>WebSocket kullanılmaz

Sunucunuz ile istemcileriniz WebSocket gereksinimlerini olduğunu doğruladıysanız (listelenen [desteklenen platformlar](../getting-started/supported-platforms.md) belge), sunucunuzda WebSocket etkinleştirmeniz gerekir. Bunu yapmak için yönergeler bulunabilir [burada](https://www.iis.net/learn/get-started/whats-new-in-iis-8/iis-80-websocket-protocol-support).

### <a name="connection-is-undefined"></a>$.connection tanımsızdır

Bu hata, bir sayfa komut düzgün şekilde yüklenmemiş veya hub proxy erişilebilir değil veya yanlış erişiliyor olduğunu gösterir. Sayfanızda komut dosyası başvuruları projenize yüklenen betikleri için karşılık gelen ve sunucu çalışırken /signalr/hubs bir tarayıcıdan erişilebilir olduğunu doğrulayın.

### <a name="one-or-more-types-required-to-compile-a-dynamic-expression-cannot-be-found"></a>Dinamik bir ifadeyi derlemek için gereken bir veya daha fazla tür bulunamıyor

Bu hata `Microsoft.CSharp` kitaplığı eksik. Ekleyebilirsiniz **derlemeleri -&gt;Framework** sekmesi.

### <a name="caller-state-cannot-be-accessed-from-clientscaller-in-visual-basic-or-in-a-strongly-typed-hub-conversion-from-type-taskof-object-to-type-string-is-not-valid-error"></a>Arayan durumu Clients.Caller Visual Basic'te veya türü kesin belirlenmiş bir hub'ında erişilemez; "Dönüştürme türünü 'Task (nesne),' 'String' türü için geçersiz" hatası

Visual Basic'te veya türü kesin belirlenmiş bir hub'ında arayan durumu erişmek için `Clients.CallerState` (SignalR 2.1 içinde sunulmuştur) özelliği yerine `Clients.Caller`.

<a id="vs"></a>

## <a name="visual-studio-issues"></a>Visual Studio sorunları

Bu bölümde, Visual Studio'da karşılaşılan sorunları açıklar.

### <a name="script-documents-node-does-not-appear-in-solution-explorer"></a>Betik Belgeler düğümü, Çözüm Gezgini'nde görünmüyor.

Öğreticilerimizden bazıları, Çözüm Gezgininde "Betik belgelerini" hata ayıklama sırasında yönlendirin. Bu düğüm, JavaScript hata ayıklayıcı tarafından üretilen ve yalnızca tarayıcı istemcilerinin Internet Explorer'da hata ayıklama sırasında görünür; Chrome ya da Firefox kullandıysanız düğümünde görünmez. Başka bir istemci hata ayıklayıcı çalışıyorsa, Silverlight hata ayıklayıcı gibi JavaScript hata ayıklayıcı ayrıca çalışmaz.

### <a name="signalr-does-not-work-on-visual-studio-2008-or-earlier"></a>Visual Studio 2008 veya daha eski SignalR çalışmıyor

Bu davranış tasarım gereğidir. SignalR, .NET Framework 4 veya sonrasını gerektirir; Bu SignalR uygulamalarını Visual Studio 2010 veya üzeri geliştirilecek gerektirir. SignalR'in sunucu bileşenini, .NET Framework 4.5 gerektirir.

<a id="iis"></a>

## <a name="iis-issues"></a>IIS sorunları

Bu bölüm, Internet Information Services ile ilgili sorunları içerir.

### <a name="signalr-works-on-visual-studio-development-server-but-not-in-iis"></a>Visual Studio geliştirme sunucusu üzerinde ancak IIS SignalR çalışır

SignalR 7.5 ve IIS 7.0 desteklenir, ancak desteği uzantısız URL'leri eklenmesi gerekir. Uzantısız URL'ler için destek eklemek için bkz [https://support.microsoft.com/kb/980368](https://support.microsoft.com/kb/980368)

SignalR, ASP.NET (ASP.NET IIS'de varsayılan olarak yüklü değildir) sunucusunda yüklü olmasını gerektirir. Bkz: ASP.NET yüklemek için [ASP.NET indirir](https://www.asp.net/downloads).

<a id="azure"></a>

## <a name="microsoft-azure-issues"></a>Microsoft Azure sorunları

Bu bölüm, Microsoft Azure ile ilgili sorunlar içerir.

### <a name="fileloadexception-when-hosting-signalr-in-an-azure-worker-role"></a>Bir Azure çalışan rolünde SignalR barındırırken FileLoadException

Bir Azure çalışan rolünde SignalR barındırma özel duruma neden olabilir "dosyası veya bütünleştirilmiş kod yüklenemedi ' Microsoft.Owin, sürüm 2.0.0.0 =". Bu, NuGet ile bilinen bir sorundur; Bağlama yeniden yönlendirmeleri Azure çalışan rolü projeleri otomatik olarak eklenmez. Bu sorunu gidermek için bağlama yeniden yönlendirmelerini el ile ekleyebilirsiniz. Aşağıdaki satırları ekleyin `app.config` çalışan rolü projeniz için dosya.

[!code-xml[Main](troubleshooting/samples/sample15.xml)]

### <a name="messages-are-not-received-through-the-azure-backplane-after-altering-topic-names"></a>İletiler konu adları değiştirme sonra Azure devre kartına alınmıyor

Azure devre kartına tarafından kullanılan konuları dahili olarak tutulur; kullanıcı tarafından yapılandırılabilen olması amaçlanmamıştır.
