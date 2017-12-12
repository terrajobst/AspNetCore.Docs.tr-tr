---
uid: signalr/overview/older-versions/troubleshooting
title: SignalR sorun giderme (SignalR 1.x) | Microsoft Docs
author: pfletcher
description: "Bu makalede SignalR uygulamaları geliştirme ile ilgili genel sorunları açıklar."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/05/2013
ms.topic: article
ms.assetid: 347210ba-c452-4feb-886f-b51d89f58971
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/troubleshooting
msc.type: authoredcontent
ms.openlocfilehash: eb739f806eb57d09008fa0bf04b5a40721dab73d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="signalr-troubleshooting-signalr-1x"></a>SignalR sorun giderme (SignalR 1.x)
====================
tarafından [CAN Fletcher'dan](https://github.com/pfletcher)

> Bu belge SignalR ile ilgili genel sorun giderme sorunları açıklar.


Bu belge aşağıdaki bölümleri içerir.

- [İstemci ve sunucu arasında yöntemleri sessizce başarısız çağırma](#connection)
- [Diğer bağlantı sorunları](#other)
- [Derleme ve sunucu tarafı hataları](#server)
- [Visual Studio sorunları](#vs)
- [Internet Information Services sorunları](#iis)
- [Azure sorunları](#azure)

<a id="connection"></a>

## <a name="calling-methods-between-the-client-and-server-silently-fails"></a>İstemci ve sunucu arasında yöntemleri sessizce başarısız çağırma

Bu bölümde bir yöntem çağrısı anlamlı bir hata iletisi başarısız için istemci ve sunucu arasındaki olası nedenleri açıklanmaktadır. Bir SignalR uygulamada, sunucunun istemci uygulayan yöntemleri hakkında bir bilgi bulunmaz; sunucunun bir istemci yöntemini çağıran istemciye gönderilen yöntemi adı ve parametre veri ve yalnızca sunucu belirtilen biçimde varsa yöntemi yürütülür. Varsa eşleşen bir yöntem hiçbir şey olmaz, istemcide bulunan ve hiçbir hata iletisi sunucuda tetiklenir.

Daha fazla istemci yöntemleri çağrılmaz araştırmak için günlüğe kaydetme hub'ındaki hangi çağrıları görmek için başlangıç yöntemi sunucudan gelen çağırmadan önce etkinleştirebilirsiniz. Bir JavaScript uygulamasında günlüğe kaydetmeyi etkinleştirmek için bkz: [istemci-tarafı günlüğünün (JavaScript istemci sürümü) nasıl etkinleştirileceği](../guide-to-the-api/hubs-api-guide-javascript-client.md#logging). Bir .NET istemci uygulamasında günlüğe kaydetmeyi etkinleştirmek için bkz: [istemci-tarafı günlüğünün (.NET istemci sürümü) nasıl etkinleştirileceği](../guide-to-the-api/hubs-api-guide-net-client.md#logging).

### <a name="misspelled-method-incorrect-method-signature-or-incorrect-hub-name"></a>Yanlış yazılmış yöntemi, hatalı yöntem imzası veya yanlış hub adı

İstemci üzerinde uygun bir yöntem adı veya çağrılan yöntemin imzası tam olarak eşleşmiyor çağrı başarısız olur. Sunucu tarafından çağrılır yöntem adı istemcide yöntemin adını eşleştiğini doğrulayın. Ayrıca, SignalR başlamalıdır yöntemlerle hub proxy oluşturur JavaScript'te uygun olduğundan, bu nedenle çağırılan bir yöntem `SendMessage` sunucuda adlı `sendMessage` istemci proxy'de. Kullanırsanız `HubName` özniteliği, sunucu tarafı kodu, kullanılan adını istemcideki hub oluşturmak için kullanılan ad ile eşleştiğini doğrulayın. Kullanmıyorsanız, `HubName` öznitelik, bir JavaScript istemci hub adını başlamalıdır, ChatHub yerine chatHub gibi olduğunu doğrulayın.

### <a name="duplicate-method-name-on-client"></a>İstemcideki yinelenen yöntemi adı

Yinelenen bir yöntem yalnızca örneğe göre farklılık istemci üzerinde sahip olmadığını doğrulayın. İstemci uygulamanızı adlı bir yöntemi varsa `sendMessage`, adında bir yöntem de, hiç doğrulayın `SendMessage` de.

### <a name="missing-json-parser-on-the-client"></a>İstemci üzerinde eksik JSON ayrıştırıcı

SignalR bir JSON ayrıştırıcısı sunucu ve istemci arasındaki aramaları serileştirmek için mevcut olması gerekir. İstemciniz (örneğin, Internet Explorer 7) yerleşik bir JSON Ayrıştırıcı yoksa, uygulamanız birinde dahil gerekir. JSON ayrıştırıcı indirebilirsiniz [burada](http://nuget.org/packages/json2).

### <a name="mixing-hub-and-persistentconnection-syntax"></a>Hub ve PersistentConnection sözdizimi karıştırma

SignalR iki iletişim modeli kullanır: hub'ları ve PersistentConnections. Bu iki iletişim modellerini çağırma söz dizimi, istemci kodu farklıdır. Bir hub sunucusu kodunuzda eklediyseniz, istemci kodunuzun tamamını kullandığını uygun hub sözdizimini doğrulayın.

**Bir PersistentConnection bir JavaScript istemci oluşturur JavaScript istemci kodu**

[!code-javascript[Main](troubleshooting/samples/sample1.js)]

**Bir Javascript istemci bir Hub Proxy oluşturur JavaScript istemci kodu**

[!code-javascript[Main](troubleshooting/samples/sample2.js)]

**Bir rota için PersistentConnection eşleyen C# sunucu kodu**

[!code-csharp[Main](troubleshooting/samples/sample3.cs)]

**Birden çok uygulamalarınız varsa, Hub'ına veya içereceğini hub için bir yol eşleyen C# sunucu kodu**

[!code-csharp[Main](troubleshooting/samples/sample4.cs)]

### <a name="connection-started-before-subscriptions-are-added"></a>Bağlantı abonelikleri eklenmeden önce başlatıldı

Hub'ın bağlantı için proxy sunucudan çağrılabilir yöntemleri eklenmeden önce başlatılırsa, iletileri alınmaz. Şu JavaScript kodunu hub düzgün başlatılmaz:

**Alınacak hub iletileri izin vermez yanlış JavaScript istemci kodu**

[!code-javascript[Main](troubleshooting/samples/sample5.js)]

Bunun yerine, yöntemi abonelikleri başlangıç çağırmadan önce ekleyin:

**Doğru bir şekilde bir hub'ına abonelikleri ekler JavaScript istemci kodu**

[!code-javascript[Main](troubleshooting/samples/sample6.js)]

### <a name="missing-method-name-on-the-hub-proxy"></a>Hub proxy yöntemi adı eksik

Sunucu üzerinde tanımlı yöntemi istemcide abone olduğu olduğunu doğrulayın. Sunucunun yöntemi tanımlar olsa bile, yine istemci proxy'sine eklenmelidir. Yöntemleri eklenebilir istemci proxy'sine aşağıdaki yollarla (yöntemi eklenir Not `client` değil hub doğrudan hub üyesi):

**Hub proxy için yöntemler ekler JavaScript istemci kodu**

[!code-javascript[Main](troubleshooting/samples/sample7.js)]

### <a name="hub-or-hub-methods-not-declared-as-public"></a>Hub'ı veya hub yöntemlerine genel olarak bildirilmedi

İstemcide görünür olması için hub uygulama ve yöntemleri olarak bildirilmelidir `public`.

### <a name="accessing-hub-from-a-different-application"></a>Farklı bir uygulamadan hub erişme

SignalR hub'ları yalnızca SignalR istemcileri kullanan uygulamalar erişilebilir. SignalR diğer iletişim kitaplıkları (gibi SOAP veya WCF web hizmeti.) ile işleyemez Hedef platformunuz için kullanılabilir SignalR istemcisi yok ise sunucunun uç noktası doğrudan erişemez.

### <a name="manually-serializing-data"></a>El ile verileri seri hale getirme

SignalR otomatik olarak JSON yönteminizi serileştirmek için kendi başınıza parametreleri-orada ait gerek kullanır.

### <a name="remote-hub-method-not-executed-on-client-in-ondisconnected-function"></a>OnDisconnected işlevi içinde istemciye yürütülmedi uzak Hub yöntemi

Bu davranış tasarım gereğidir. Zaman `OnDisconnected` olduğu olarak adlandırılan, hub'ı zaten geçtiğini `Disconnected` daha fazla çağrılacak hub yöntemlerine izin verme durumu.

**Doğru bir şekilde kod içinde OnDisconnected olayını yürüten C# sunucu kodu**

[!code-csharp[Main](troubleshooting/samples/sample8.cs)]

### <a name="connection-limit-reached"></a>Bağlantı sınırına ulaşıldı

Windows 7 gibi bir istemci işletim sisteminde IIS'ın tam sürümünü kullanırken, 10-bağlantı sınırı sınırlamasıdır. Bir istemci işletim sistemi kullanırken, IIS Express yerine bu sınırı önlemek için kullanın.

### <a name="cross-domain-connection-not-set-up-properly"></a>Etki alanları arası bağlantı düzgün ayarlanmamış

Etki alanları arası bağlantı varsa (bir ağ bağlantısı için SignalR URL içinde olmayan barındırma sayfası aynı etki alanında) ayarlanmamışsa doğru bağlantı bir hata iletisi başarısız olabilir. Etki alanları arası iletişimi etkinleştirme hakkında daha fazla bilgi için bkz: [etki alanları arası bağlantı kurmak nasıl](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain).

### <a name="connection-using-ntlm-active-directory-not-working-in-net-client"></a>.NET istemci çalışmıyor NTLM (Active Directory) kullanarak bağlantı

Bağlantısı düzgün şekilde yapılandırılmamışsa, etki alanı güvenlik kullanan bir .NET istemci uygulamasındaki bağlantı başarısız olabilir. Bir etki alanı ortamında SignalR kullanmak için gerekli bağlantı özelliği aşağıdaki gibi ayarlayın:

**Bağlantı kimlik bilgilerini uygulayan C# istemci kodu**

[!code-csharp[Main](troubleshooting/samples/sample9.cs)]

<a id="other"></a>

## <a name="other-connection-issues"></a>Diğer bağlantı sorunları

Bu bölümde, nedenleri ve çözümleri belirli Belirtiler veya bağlantı sırasında oluşan hata iletileri açıklanmaktadır.

### <a name="start-must-be-called-before-data-can-be-sent-error"></a>"Veri gönderilmeden önce start çağırılmalıdır" hatası

Kod SignalR nesneleri başvuruyorsa bağlantı başlatmadan önce bu hata sık görülür. İşleyicileri ve benzeri için iliştirdiği bağlantı tamamlandıktan sonra sunucuda tanımlanan çağrı yöntemleri eklenecek gerekir. Unutmayın çağrısı `Start` böylelikle önce çağrı yürütülebilecek sonra kod tamamlanır uyumsuzdur. Bir bağlantı tamamen başlatıldıktan sonra işleyicileri eklemek için en iyi yolu bunları Başlangıç yönteme parametre olarak geçirilen bir geri çağırma işlevini yerleştirin şudur:

**SignalR nesnelerine başvuru olay işleyicilerini doğru ekler JavaScript istemci kodu**

[!code-javascript[Main](troubleshooting/samples/sample10.js?highlight=1)]

Bu hata, ayrıca SignalR nesneler hala başvurulan ederken bir bağlantı durduğunda görülür.

### <a name="301-moved-permanently-or-302-moved-temporarily-error"></a>"301 kalıcı olarak taşındı" veya "302 geçici olarak taşındı" hatası

Proje otomatik olarak oluşturulan proxy ile müdahale SignalR adlı bir klasör içeriyorsa, bu hata görülebilir. Bu hatayı önlemek için adlı bir klasör kullanmayın `SignalR` , uygulama ya da devre dışı bırakma otomatik proxy oluşturma. Bkz: [oluşturulan Proxy ve onu sizin için ne yaptığını](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy) daha fazla ayrıntı için.

### <a name="403-forbidden-error-in-net-or-silverlight-client"></a>.NET veya Silverlight istemci "403 Yasak" hatası

Bu hata, burada etki alanları arası iletişimin düzgün etkin değilse etki alanları arası ortamlarda ortaya çıkabilir. Etki alanları arası iletişimi etkinleştirme hakkında daha fazla bilgi için bkz: [etki alanları arası bağlantı kurmak nasıl](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain). Silverlight istemcisinde etki alanları arası bağlantı kurmak için bkz: [Silverlight istemcilerden etki alanları arası bağlantıları](../guide-to-the-api/hubs-api-guide-net-client.md#slcrossdomain).

### <a name="404-not-found-error"></a>"404 bulunamadı" hatası

Bu sorunu birkaç nedeni vardır. Aşağıdakilerin tümü doğrulayın:

- **Hub proxy adresi başvurusu düzgün şekilde biçimlendirilmemiş:** oluşturulan hub proxy adresi referansı doğru şekilde biçimlendirilmemiş değilse bu hata sık görülür. Hub adresi referansı düzgün şekilde yapıldığını doğrulayın. Bkz: [dinamik olarak üretilen proxy başvurmak nasıl](../guide-to-the-api/hubs-api-guide-javascript-client.md#dynamicproxy) Ayrıntılar için.
- **Hub rotasını eklemeden önce uygulama için yollar ekleme:** uygulamanız diğer yollar kullanıyorsa, eklediğiniz ilk rota çağrısı olduğunu doğrulayın `MapHubs`.

### <a name="500-internal-server-error"></a>"500 İç sunucu hatası"

Bu, çok çeşitli nedenleri olabilir çok genel bir hatadır. Hata ayrıntılarını sunucunun olay günlüğüne görünmelidir veya sunucunun hata ayıklama aracılığıyla bulunabilir. Daha ayrıntılı hata bilgileri sunucusundaki ayrıntılı hataları açarak alınabilir. Daha fazla bilgi için bkz: [Hub sınıfında hataların nasıl işleneceğini](../guide-to-the-api/hubs-api-guide-server.md#handleErrors).

### <a name="typeerror-lthubtypegt-is-undefined-error"></a>"TypeError: &lt;hubType&gt; tanımlı değil" hatası

Bu hataya neden olur çağrısı `MapHubs` düzgün bırakılmaz. Bkz: [SignalR rota kaydetmek ve SignalR seçeneklerini yapılandırmak nasıl](../guide-to-the-api/hubs-api-guide-server.md#route) daha fazla bilgi için.

### <a name="jsonserializationexception-was-unhandled-by-user-code"></a>JsonSerializationException kullanıcı kodu tarafından işlenmemiş

Yöntemlerinizi için gönderdiğiniz parametreleri seri hale getirilemeyen türleri (örneğin, dosya tanıtıcısı veya veritabanı bağlantılarını) eklemeyin doğrulayın. İstemci (veya güvenlik için seri hale getirme, nedeniyle), kullanım gönderilmek üzere istemediğiniz bir sunucu tarafı nesnesinde üyeleri kullanmanız gerekiyorsa `JSONIgnore` özniteliği.

### <a name="protocol-error-unknown-transport-error"></a>"Protokol hatası: Unknown transport" hatası

İstemci SignalR kullanan taşımalar desteklemiyorsa, bu hata oluşabilir. Bkz: [aktarımları ve geri dönüşler](../getting-started/introduction-to-signalr.md#transports) üzerinde tarayıcılar kullanılabilir SignalR ile bilgi.

### <a name="javascript-hub-proxy-generation-has-been-disabled"></a>"JavaScript hub'ı proxy oluşturması devre dışı."

Bu hata oluşacaktır `DisableJavaScriptProxies` dinamik olarak üretilen proxy başvuru de dahil olmak üzere sırasında ayarlanır `signalr/hubs`. Proxy el ile oluşturma hakkında daha fazla bilgi için bkz: [oluşturulan proxy ve onu sizin için ne yaptığını](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy).

### <a name="the-connection-id-is-in-the-incorrect-format-or-the-user-identity-cannot-change-during-an-active-signalr-connection-error"></a>"Bağlantı kimliği yanlış biçimde" veya "kullanıcı kimliği etkin bir SignalR bağlantısı sırasında değiştiremezsiniz" hatası

Bu hata, kimlik doğrulaması kullanılır ve istemci bağlantı durdurulmadan önce kaydedilir görülebilir. İstemci günlük önce SignalR bağlantısı durdurmak için çözümüdür.

### <a name="uncaught-error-signalr-jquery-not-found-please-ensure-jquery-is-referenced-before-the-signalrjs-file-error"></a>"Yakalanmamış hata: SignalR: jQuery bulunamadı. JQuery SignalR.js dosyanın önce başvurulan olun"hatası

SignalR JavaScript istemci çalıştırmak için jQuery gerektirir. JQuery, başvuru kullanılan yolun geçerli olduğundan ve jQuery referansı SignalR referansı önce olduğunu doğru olduğunu doğrulayın.

### <a name="uncaught-typeerror-cannot-read-property-ltpropertygt-of-undefined-error"></a>"Yakalanmamış TypeError: özelliği okunamıyor '&lt;özelliği&gt;' tanımsız," hatası

Bu hata, jQuery veya düzgün başvurulan hub proxy kalmaktan olmayan sonuçlanır. Başvuru jQuery ve hub proxy için kullanılan yolu geçerli olduğunu ve jQuery referansı hub proxy referansı önce olduğunu doğru olduğunu doğrulayın. Hub proxy için varsayılan başvuru aşağıdaki gibi görünmelidir:

**Hub proxy doğru başvuran HTML istemci-tarafı kodu**

[!code-html[Main](troubleshooting/samples/sample11.html)]

### <a name="runtimebinderexception-was-unhandled-by-user-code-error"></a>"RuntimeBinderException kullanıcı kodu tarafından işlenmemiş" hatası

Bu hata oluşabilir olduğunda yanlış aşırı yüklemesini `Hub.On` kullanılır. Yöntemin dönüş değeri varsa, dönüş türü genel tür parametresi olarak belirtilmelidir:

**İstemci (olmadan oluşturulan proxy) üzerinde tanımlı yöntemi**

[!code-html[Main](troubleshooting/samples/sample12.html?highlight=1)]

### <a name="connection-id-is-inconsistent-or-connection-breaks-between-page-loads"></a>Bağlantı kimliği tutarsız veya sayfa yüklerinin bağlantıyı keser

Bu davranış tasarım gereğidir. Sayfa nesnesinde barındırılan hub nesne olduğundan, Sayfa yenilendiğinde hub'ı yok. Sayfa yüklerinin arasında tutarlı olmaları böylece kullanıcılar ve bağlantı kimlikleri arasındaki ilişkiyi korumak çok sayfalı uygulama gerekir. Bağlantı kimlikleri ya da sunucusunda depolanan bir `ConcurrentDictionary` nesnesi veya bir veritabanı.

### <a name="value-cannot-be-null-error"></a>Hata "Değeri null olamaz"

Sunucu tarafı yöntemlerinin isteğe bağlı parametreler ile şu anda desteklenmiyor; İsteğe bağlı parametresi atlanırsa, yöntem başarısız olur. Daha fazla bilgi için bkz: [isteğe bağlı parametreler](https://github.com/SignalR/SignalR/issues/324).

### <a name="firefox-cant-establish-a-connection-to-the-server-at-ltaddressgt-error-in-firebug"></a>"Firefox sunucuda bağlantı kuramıyor &lt;adresi&gt;" Firebug hatası

Bu hata iletisi içinde Firebug anlaşma WebSocket taşıma başarısız olur ve bunun yerine başka bir aktarım kullanılıyorsa görülebilir. Bu davranış tasarım gereğidir.

### <a name="the-remote-certificate-is-invalid-according-to-the-validation-procedure-error-in-net-client-application"></a>.NET istemci uygulamasında "uzak sertifika doğrulama yordamına göre geçersiz" hatası

Sunucunuz özel istemci sertifikası gerektiriyorsa, istekte önce sonra bir x509certificate bağlantı ekleyebilirsiniz. Bağlantı kullanarak sertifika eklemeniz `Connection.AddClientCertificate`.

### <a name="connection-drops-after-authentication-times-out"></a>Kimlik doğrulama zaman aşımına uğruyor sonra bağlantı bırakır

Bu davranış tasarım gereğidir. Bağlantı etkin durumdayken, kimlik doğrulama kimlik bilgileri değiştirilemez; kimlik bilgilerini yenilemek için bağlantı durdurulup yeniden gerekir.

### <a name="onconnected-gets-called-twice-when-using-jquery-mobile"></a>OnConnected iki kez jQuery Mobile kullanırken adlı

jQuery Mobile's `initializePage` işlevi, böylece ikinci bir bağlantı oluşturma yeniden yürütülmesi için her bir sayfa komut zorlar. Bu soruna yönelik çözümler içerir:

- JQuery Mobile, JavaScript dosyası önce referansı içerir.
- Devre dışı `initializePage` ayarlayarak işlevi `$.mobile.autoInitializePage = false`.
- Sayfanın bağlantı başlatmadan önce başlatma bitmesini bekleyin.

### <a name="messages-are-delayed-in-silverlight-applications-using-server-sent-events"></a>İleti sunucusu gönderilen olayları kullanarak Silverlight uygulamalarında gecikti

Sunucu kullanarak olayları Silverlight'ın gönderilen iletileri gecikir. Bunun yerine kullanılacak yoklama uzun zorlamak için aşağıdaki bağlantıyı başlatırken kullanın:

[!code-css[Main](troubleshooting/samples/sample13.css)]

### <a name="permission-denied-using-forever-frame-protocol"></a>"İzni reddedildi" kullanarak sonsuza kadar çerçeve Protokolü

Bu açıklanan bilinen bir sorun olup [burada](https://github.com/SignalR/SignalR/issues/1963). Bu belirti son JQuery kitaplığını kullanarak görülebilir; JQuery 1.8.2 uygulamanıza düşürmek için geçici bir çözüm değildir.

<a id="server"></a>

## <a name="compilation-and-server-side-errors"></a>Derleme ve sunucu tarafı hataları

 Aşağıdaki bölümde, derleyici ve sunucu tarafı çalışma zamanı hataları için olası çözümleri içerir. 

### <a name="reference-to-hub-instance-is-null"></a>Hub örneği başvurusu null

Her bağlantı için hub örneği oluşturulduktan sonra bir hub örneği kodunuzda kendiniz oluşturamazsınız. Hub dışında aynı hub'da yöntemleri çağırmak için bkz: [istemci yöntemlerini çağırın ve Hub sınıfına dışında gruplarından yönetmek nasıl](../guide-to-the-api/hubs-api-guide-server.md#callfromoutsidehub) için hub bağlamını başvuru elde etme.

### <a name="httpcontextcurrentsession-is-null"></a>HTTPContext.Current.Session null

Bu davranış tasarım gereğidir. Oturum durumunu etkinleştirme çift yönlü Mesajlaşma ihlal edeceğinden bu yana SignalR ASP.NET oturum durumu desteklemez.

### <a name="no-suitable-method-to-override"></a>Geçersiz kılmak için uygun yöntem

Eski belgelere veya blog koddan kullanıyorsanız, bu hatayı görebilirsiniz. Değiştirilmiş veya kullanım dışı yöntemler adlarını başvuran değil doğrulayın (gibi `OnConnectedAsync`).

### <a name="hostcontextextensionswebsocketserverurl-is-null"></a>HostContextExtensions.WebSocketServerUrl null

Bu davranış tasarım gereğidir. Bu üye kullanım dışıdır ve kullanılmamalıdır.

### <a name="a-route-named-signalrhubs-is-already-in-the-route-collection-error"></a>"'Signalr.hubs' adlı bir rota rota koleksiyonunda zaten" hatası

Bu hata görülen `MapHubs` iki kez uygulamanız tarafından çağrılır. Bazı örnek uygulamaları çağrısı `MapHubs` doğrudan genel uygulama dosyasında; başkalarının yaptığı bir sarmalayıcı sınıfı çağrısında. Uygulamanızı her ikisi de yapmaz emin olun.

<a id="vs"></a>

## <a name="visual-studio-issues"></a>Visual Studio sorunları

Bu bölümde Visual Studio'da karşılaşılan sorunları açıklar.

### <a name="script-documents-node-does-not-appear-in-solution-explorer"></a>Komut dosyası Belgeler düğümü Çözüm Gezgini'nde görünmüyor

Öğreticilerimizi bazıları, Çözüm Gezgini'nde "Komut dosyası belgeler" düğümü hata ayıklama sırasında yönlendirin. Bu düğüm, JavaScript hata ayıklayıcı tarafından üretilen ve Internet Explorer'da tarayıcı istemcilerinin hata ayıklarken yalnızca görünür; Chrome ya da Firefox kullanılırsa düğüm görünmez. Başka bir istemci hata ayıklayıcı çalışıyorsa, Silverlight hata ayıklayıcı gibi JavaScript hata ayıklayıcısı olsa da çalışmaz.

### <a name="signalr-does-not-work-on-visual-studio-2008-or-earlier"></a>SignalR Visual Studio 2008 veya önceki sürümlerde çalışmaz

Bu davranış tasarım gereğidir. SignalR .NET Framework 4 veya üstünü gerektirir; Bu SignalR uygulamalarını Visual Studio 2010 veya üzeri geliştirilecek gerektirir.

<a id="iis"></a>

## <a name="iis-issues"></a>IIS sorunları

Bu bölüm, Internet Information Services ile ilgili sorunları içerir.

### <a name="web-site-crashes-after-maphubs-call"></a>Web sitesi MapHubs çağırdıktan sonra kilitleniyor

SignalR son sürümünde bu sorun düzeltilmiştir. NuGet kullanarak yüklemenizi güncelleştirerek SignalR ' en son yayımlanan sürümünü kullandığınızdan emin olun.

<a id="azure"></a>

## <a name="azure-issues"></a>Azure sorunları

Bu bölüm, Microsoft Azure ile ilgili sorunları içerir.

### <a name="messages-are-not-received-through-the-azure-backplane-after-altering-topic-names"></a>İletileri Azure devre kartı konu adlarının değiştirilmesine sonra alınmaz

Azure devre kartı tarafından kullanılan konuları kullanıcı yapılandırılabilir olması amaçlanmamıştır.
