---
title: ASP.NET Core Blazor için Içerik Güvenlik Ilkesi zorlama
author: guardrex
description: Siteler arası komut dosyası (XSS) saldırılarına karşı korumaya yardımcı olmak için ASP.NET Core Blazor uygulamalarıyla bir Içerik Güvenlik Ilkesi (CSP) kullanmayı öğrenin.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/02/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/content-security-policy
ms.openlocfilehash: 1cfebf7b3d3bbb98a671b6f2db7c6518cda74b65
ms.sourcegitcommit: 51c86c003ab5436598dbc42f26ea4a83a795fd6e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/10/2020
ms.locfileid: "78964552"
---
# <a name="enforce-a-content-security-policy-for-aspnet-core-opno-locblazor"></a>ASP.NET Core Blazor için Içerik Güvenlik Ilkesi zorlama

, [Javier Calvarro Nelson](https://github.com/javiercn) ve [Luke Latham](https://github.com/guardrex) 'e göre

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[Siteler arası betik oluşturma (XSS)](xref:security/cross-site-scripting) , bir saldırganın bir veya daha fazla kötü amaçlı istemci tarafı komut dosyasını uygulamanın işlenmiş içeriğine yerleştirdiği bir güvenlik açığıdır. Bir Içerik Güvenlik Ilkesi (CSP), geçerli bir tarayıcıya bildirerek XSS saldırılarına karşı korunmaya yardımcı olur:

* Betikler, stil sayfaları ve görüntüler dahil olmak üzere yüklenen içerik kaynakları.
* Formun izin verilen URL hedeflerini belirten bir sayfa tarafından gerçekleştirilen eylemler.
* Yüklenebilen eklentiler.

Bir CSP 'yi bir uygulamaya uygulamak için, geliştirici bir veya daha fazla `Content-Security-Policy` üst bilgi veya `<meta>` etiketlerinde çeşitli CSP içerik güvenliği *yönergeleri* belirler.

İlkeler, bir sayfa yüklenirken tarayıcı tarafından değerlendirilir. Tarayıcı, sayfanın kaynaklarını inceler ve içerik güvenliği yönergelerinin gereksinimlerini karşılayıp karşılamadığını belirler. Bir kaynak için ilke yönergeleri karşılanmazsa, tarayıcı kaynağı yüklemez. Örneğin, üçüncü taraf betiklerine izin veren bir ilkeyi göz önünde bulundurun. Bir sayfa, `src` özniteliğinde üçüncü taraf kaynağına sahip bir `<script>` etiketi içerdiğinde, tarayıcı betiğin yüklenmesini engeller.

CSP, Chrome, Edge, Firefox, Opera ve Safari dahil olmak üzere çoğu modern masaüstü ve mobil tarayıcılarda desteklenir. CSP Blazor uygulamalar için önerilir.

## <a name="policy-directives"></a>İlke yönergeleri

En düşük düzeyde, Blazor uygulamalar için aşağıdaki yönergeleri ve kaynakları belirtin. Gerektiğinde ek yönergeler ve kaynaklar ekleyin. Aşağıdaki yönergeler, bu makalenin [Ilkeyi Uygula](#apply-the-policy) bölümünde kullanılır; burada, Blazor WebAssembly ve Blazor sunucusu için güvenlik ilkeleri sağlanır:

* [taban urı](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/base-uri) &ndash; sayfanın `<base>` etiketinin URL 'lerini kısıtlar. Uygulamanın kaynağının, düzen ve bağlantı noktası numarası dahil olmak üzere geçerli bir kaynak olduğunu göstermek için `self` belirtin.
* [Engelle-tümünü karışık içerikli](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/block-all-mixed-content) &ndash;, karışık http ve https Içeriğini yüklemeyi engeller.
* [varsayılan-src](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/default-src) &ndash;, ilke tarafından açıkça belirtilmeyen kaynak yönergeleri için bir geri dönüş gösterir. Uygulamanın kaynağının, düzen ve bağlantı noktası numarası dahil olmak üzere geçerli bir kaynak olduğunu göstermek için `self` belirtin.
* [img-src](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/img-src) &ndash; görüntüler için geçerli kaynakları gösterir.
  * `data:` URL 'lerden görüntü yüklemeye izin vermek için `data:` belirtin.
  * HTTPS uç noktalarından görüntü yüklemeye izin vermek için `https:` belirtin.
* [nesne-src](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/object-src) &ndash; `<object>`, `<embed>`ve `<applet>` etiketleri için geçerli kaynakları gösterir. Tüm URL kaynaklarını engellemek için `none` belirtin.
* [Script-src](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/script-src) &ndash; betikler için geçerli kaynakları gösterir.
  * Önyükleme betikleri için `https://stackpath.bootstrapcdn.com/` konak kaynağını belirtin.
  * Uygulamanın kaynağının, düzen ve bağlantı noktası numarası dahil olmak üzere geçerli bir kaynak olduğunu göstermek için `self` belirtin.
  * Blazor WebAssembly uygulamasında:
    * Gerekli Blazor WebAssembly satır içi betiklerinin yüklenmesine izin vermek için aşağıdaki karmaları belirtin:
      * `sha256-v8ZC9OgMhcnEQ/Me77/R9TlJfzOBqrMTW8e1KuqLaqc=`
      * `sha256-If//FtbPc03afjLezvWHnC3Nbu4fDM04IIzkPaf3pH0=`
      * `sha256-v8v3RKRPmN4odZ1CWM5gw80QKPCCWMcpNeOmimNL2AA=`
    * `eval()` kullanılacak `unsafe-eval` ve dizelerdeki kodu oluşturmak için yöntemleri belirtin.
  * Blazor sunucu uygulamasında, stil sayfaları için geri dönüş algılamayı gerçekleştiren satır içi betik için `sha256-34WLX60Tw3aG6hylk0plKbZZFXCuepeQ6Hu7OqRf8PI=` karmasını belirtin.
* [Style-src](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/style-src) &ndash; stil sayfaları için geçerli kaynakları gösterir.
  * Önyükleme stil sayfaları için `https://stackpath.bootstrapcdn.com/` konak kaynağını belirtin.
  * Uygulamanın kaynağının, düzen ve bağlantı noktası numarası dahil olmak üzere geçerli bir kaynak olduğunu göstermek için `self` belirtin.
  * Satır içi stillerin kullanılmasına izin vermek için `unsafe-inline` belirtin. İlk istekten sonra istemciyi ve sunucuyu yeniden bağlamaya yönelik Blazor Server uygulamalarındaki Kullanıcı arabirimi için satır içi bildirimi gerekir. Gelecekteki bir sürümde, `unsafe-inline` artık gerekli olmaması için satır içi stil kaldırılabilir.
* [yükseltme-güvenli olmayan-istekler](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/upgrade-insecure-requests) &ndash;, güvenli olmayan (http) kaynaklardaki Içerik URL 'lerinin https üzerinden güvenli bir şekilde alınması gerektiğini gösterir.

Yukarıdaki yönergeler, Microsoft Internet Explorer hariç tüm tarayıcılar tarafından desteklenir.

Ek satır içi betikler için SHA karmaları almak için:

* [Ilkeyi Uygula](#apply-the-policy) bölümünde gösterilen CSP 'yi uygulayın.
* Uygulamayı yerel olarak çalıştırırken tarayıcının geliştirici araçları konsoluna erişin. Tarayıcı, bir CSP üst bilgisi veya `meta` etiketi mevcut olduğunda engellenen betikler için karmaları hesaplar ve görüntüler.
* Tarayıcı tarafından sunulan karmaları `script-src` kaynaklarına kopyalayın. Her karmaya ait tek tırnakları kullanın.

Içerik Güvenlik Ilkesi düzey 2 tarayıcı desteği matrisi için bkz. [Içerik Güvenlik Ilkesi düzeyi 2 ' yi kullanabilir miyim](https://www.caniuse.com/#feat=contentsecuritypolicy2).

## <a name="apply-the-policy"></a>İlkeyi Uygula

İlkeyi uygulamak için bir `<meta>` etiketi kullanın:

* `http-equiv` özniteliğinin değerini `Content-Security-Policy`olarak ayarlayın.
* Yönergeleri `content` öznitelik değerine yerleştirin. Yönergeleri noktalı virgülle ayırın (`;`).
* `meta` etiketini her zaman `<head>` içeriğine yerleştirin.

Aşağıdaki bölümlerde, Blazor WebAssembly ve Blazor sunucusu için örnek ilkeler gösterilmektedir. Bu örnekler, her Blazorsürümü için bu makalede sürümlüdür. Sürümünüze uygun bir sürümü kullanmak için bu Web sayfasında **Sürüm** açılan Seçicisi seçiciyle birlikte belge sürümü ' nü seçin.

### <a name="opno-locblazor-webassembly"></a>Blazor WebAssembly

*Wwwroot/index.html* ana bilgisayarının `<head>` içeriği sayfasında, [ilke yönergeleri](#policy-directives) bölümünde açıklanan yönergeleri uygulayın:

```html
<meta http-equiv="Content-Security-Policy" 
      content="base-uri 'self';
               block-all-mixed-content;
               default-src 'self';
               img-src data: https:;
               object-src 'none';
               script-src https://stackpath.bootstrapcdn.com/ 
                          'self' 
                          'sha256-v8ZC9OgMhcnEQ/Me77/R9TlJfzOBqrMTW8e1KuqLaqc=' 
                          'sha256-If//FtbPc03afjLezvWHnC3Nbu4fDM04IIzkPaf3pH0=' 
                          'sha256-v8v3RKRPmN4odZ1CWM5gw80QKPCCWMcpNeOmimNL2AA=' 
                          'unsafe-eval';
               style-src https://stackpath.bootstrapcdn.com/
                         'self'
                         'unsafe-inline';
               upgrade-insecure-requests;">
```

### <a name="opno-locblazor-server"></a>Blazor sunucusu

*Pages/_Host. cshtml* ana bilgisayarının `<head>` içeriği sayfasında, [ilke yönergeleri](#policy-directives) bölümünde açıklanan yönergeleri uygulayın:

```cshtml
<meta http-equiv="Content-Security-Policy" 
      content="base-uri 'self';
               block-all-mixed-content;
               default-src 'self';
               img-src data: https:;
               object-src 'none';
               script-src https://stackpath.bootstrapcdn.com/ 
                          'self' 
                          'sha256-34WLX60Tw3aG6hylk0plKbZZFXCuepeQ6Hu7OqRf8PI=';
               style-src https://stackpath.bootstrapcdn.com/
                         'self' 
                         'unsafe-inline';
               upgrade-insecure-requests;">
```

## <a name="meta-tag-limitations"></a>Meta etiketi sınırlamaları

`<meta>` Tag ilkesi aşağıdaki yönergeleri desteklemez:

* [çerçeve-üst öğeleri](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/frame-ancestors)
* [raporla](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/report-to)
* [rapor-URI](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/report-uri)
* [alanda](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/sandbox)

Önceki yönergeleri desteklemek için `Content-Security-Policy`adlı bir üst bilgi kullanın. Yönerge dizesi üst bilginin değeridir.

## <a name="test-a-policy-and-receive-violation-reports"></a>İlkeyi test etme ve ihlal raporları alma

Test, ilk ilke oluşturulurken üçüncü taraf betiklerin yanlışlıkla engellenmediğinden emin olmaya yardımcı olur.

İlke yönergelerini uygulamadan bir süre boyunca bir ilkeyi test etmek için, üst bilgi tabanlı bir ilkenin `<meta>` etiketinin `http-equiv` özniteliğini veya üstbilgi adını `Content-Security-Policy-Report-Only`olarak ayarlayın. Hata raporları, belirtilen bir URL 'ye JSON belgeleri olarak gönderilir. Daha fazla bilgi için bkz. [MDN Web belgeleri: içerik-güvenlik-ilke-yalnızca rapor](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy-Report-Only).

Bir ilke etkinken ihlal hakkında raporlamak için aşağıdaki makalelere bakın:

* [raporla](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/report-to)
* [rapor-URI](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/report-uri)

`report-uri` artık kullanım için önerilmese de, tüm ana tarayıcıların `report-to` desteklenene kadar her iki yönergelerin de kullanılması gerekir. `report-uri` desteği tarayıcılardan *herhangi bir zamanda* bırakılmakta olduğundan `report-uri` özel olarak kullanmayın. `report-to` tam olarak desteklenmiş olduğunda ilkelerinizdeki `report-uri` desteğini kaldırın. `report-to`benimseme durumunu izlemek için, bkz. [Raporlama-to](https://caniuse.com/#feat=mdn-http_headers_csp_content-security-policy_report-to).

Uygulamanın ilkesini her sürümde test edin ve güncelleştirin.

## <a name="troubleshoot"></a>Sorun giderme

* Hatalar tarayıcının geliştirici araçları konsolunda görünür. Tarayıcılar hakkında bilgi sağlar:
  * İlkeyle uyumlu olmayan öğeler.
  * İlkeyi engellenen bir öğe için izin verecek şekilde değiştirme.
* Bir ilke, istemci tarayıcısı tüm dahil edilen yönergeleri destekliyorsa tamamen etkilidir. Geçerli bir tarayıcı desteği matrisi için, bkz [: Content-Security-Policy](https://caniuse.com/#search=Content-Security-Policy).

## <a name="additional-resources"></a>Ek kaynaklar

* [MDN Web belgeleri: Içerik-güvenlik-Ilke](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy)
* [İçerik Güvenlik Ilkesi düzeyi 2](https://www.w3.org/TR/CSP2/)
* [Google CSP değerlendiricisi](https://csp-evaluator.withgoogle.com/)
