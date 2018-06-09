---
uid: web-api/overview/security/working-with-ssl-in-web-api
title: Web API'de SSL ile çalışma | Microsoft Docs
author: MikeWasson
description: SSL istemci sertifikalarını kullanan dahil olmak üzere ASP.NET Web API ile SSL kullanmayı gösterir.
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/12/2012
ms.topic: article
ms.assetid: 97f6164f-59cf-45c0-b820-e4aa29b45396
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/working-with-ssl-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 127b336cb628e55bd59481ecb1c4df83960dc25b
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "28036170"
---
<a name="working-with-ssl-in-web-api"></a>Web API'de SSL ile çalışma
====================
tarafından [CAN Wasson](https://github.com/MikeWasson)

Birçok ortak kimlik doğrulama şemasını düz HTTP üzerinden güvenli değildir. Özellikle, temel kimlik doğrulaması ve forms kimlik doğrulaması şifrelenmemiş kimlik bilgilerini gönderir. Güvenli olması için bu kimlik doğrulama şemasını *gerekir* SSL kullanın. Buna ek olarak, SSL istemci sertifikaları, istemcilerin kimliğini doğrulamak için kullanılabilir.

## <a name="enabling-ssl-on-the-server"></a>Sunucuda SSL etkinleştirme

IIS 7 veya sonraki sürümlerde SSL kurma için:

- Oluşturun veya bir sertifika alın. Test etmek için otomatik olarak imzalanan sertifika oluşturabilirsiniz.
- Bir HTTPS bağlaması ekleyin.

Ayrıntılar için bkz [SSL ayarlamak IIS 7 üzerinde nasıl](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis).

Yerel test etmek için IIS Express Visual Studio'dan SSL etkinleştirebilirsiniz. Özellikler penceresinde ayarlayın **SSL etkin** için **doğru**. Değeri Not **SSL URL**; HTTPS bağlantıları test etmek için bu URL'yi kullanın.

![](working-with-ssl-in-web-api/_static/image1.png)

### <a name="enforcing-ssl-in-a-web-api-controller"></a>Bir Web API denetleyicisi SSL zorlama

Bir HTTPS ve HTTP bağlaması varsa, istemciler siteye erişmek için HTTP hala kullanabilirsiniz. Diğer kaynaklar SSL gerektirirken HTTP kullanılabilir olması bazı kaynaklar sağlayabilir. Bu durumda, korunan kaynaklar için SSL gerektirmek için bir eylem filtresi kullanın. Aşağıdaki kod, SSL için denetler bir Web API kimlik doğrulama filtre gösterir:

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample1.cs)]

Bu filtre SSL gereken tüm Web API eylemler ekleyin:

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample2.cs)]

## <a name="ssl-client-certificates"></a>SSL istemci sertifikaları

SSL, ortak anahtar altyapısı sertifikalarını kullanarak kimlik doğrulaması sağlar. Sunucu istemciye kimlik doğrulamasını bir sertifika sağlamanız gerekir. İstemci sunucuya bir sertifika sağlamak daha az yaygın bir durumdur, ancak istemcilerin kimliğini doğrulamak için bir seçenek budur. SSL ile istemci sertifikaları kullanmak için kullanıcılarınız için imzalanmış sertifikaları dağıtmak için bir yönteme ihtiyacınız vardır. Birçok uygulama türü için bu iyi bir kullanıcı deneyimi olmayacak, ancak bazı ortamlarda (örneğin, Kurumsal) uygun olabilir.

| Yararları | Dezavantajlar |
| --- | --- |
| -Kullanıcı adı/parola güçlü sertifika kimlik bilgileridir. -SSL kimlik doğrulaması, ileti bütünlüğü ve ileti şifreleme ile tam bir güvenli kanal sağlar. | -Edinmeli ve PKI sertifikaları yönetin. -İstemci Platformu SSL istemci sertifikalarını desteklemesi gerekir. |

İstemci sertifikalarını kabul etmek üzere IIS'yi yapılandırmak için IIS Yöneticisi'ni açın ve aşağıdaki adımları gerçekleştirin:

1. Ağaç görünümünde site düğümünü tıklatın.
2. Çift **SSL ayarları** Orta bölmede özelliği.
3. Altında **istemci sertifikalarını**, aşağıdaki seçeneklerden birini seçin: 

    - **Kabul**: IIS istemci sertifikası kabul eder, ancak bir gerektirmez.
    - **Gerektiren**: bir istemci sertifikası gerektirir. (Bu seçeneği etkinleştirmek için de "SSL iste" seçmeniz gerekir)

Bu gibi durumlarda, bu seçenekler ayrıca ApplicationHost.config dosyasında ayarlayabilirsiniz:

[!code-xml[Main](working-with-ssl-in-web-api/samples/sample3.xml)]

**SslNegotiateCert** bayrağı anlamına gelir IIS istemci sertifikası kabul eder, ancak bir ("Kabul et" seçeneğini IIS Yöneticisi'nde eşdeğer) gerektirmez. Bir sertifika istemek için ayarlayın **sıklıkla** bayrağı. Test etmek için Ayrıca bu seçenekler IIS Express'te yerel applicationhost ayarlayabilirsiniz. "Documents\IISExpress\config" içinde bulunan yapılandırma dosyası.

### <a name="creating-a-client-certificate-for-testing"></a>Test etmek için bir istemci sertifikası oluşturma

Test amacıyla kullanabilirsiniz [MakeCert.exe](https://msdn.microsoft.com/library/bfsktky3.aspx) bir istemci sertifikası oluşturmak için. İlk olarak, bir test kök yetkilisi oluşturun:

[!code-console[Main](working-with-ssl-in-web-api/samples/sample4.cmd)]

MakeCert, özel anahtarı bir parola girmenizi ister.

Ardından, sertifika sunucunun "güvenilen kök sertifika yetkilileri" depolamak, aşağıdaki gibi test ekleyin:

1. MMC'yi açın.
2. Altında **dosya**seçin **Ekle/Kaldır ek bileşenini**.
3. Seçin **bilgisayar hesabı**.
4. Seçin **yerel bilgisayar** ve Sihirbazı tamamlayın.
5. Gezinti bölmesinin altında "Güvenilen kök sertifika yetkilileri" düğümünü genişletin.
6. Üzerinde **eylem** menüsündeki **tüm görevler**ve ardından **alma** Sertifika Alma Sihirbazı'nı başlatın.
7. TempCA.cer sertifika dosyasına göz atın.
8. Tıklatın **açık**, ardından **sonraki** ve Sihirbazı tamamlayın. (Parola girmeniz istenir.)

Artık ilk sertifikası tarafından imzalanmış bir istemci sertifikası oluşturun:

[!code-console[Main](working-with-ssl-in-web-api/samples/sample5.cmd)]

### <a name="using-client-certificates-in-web-api"></a>İstemci sertifikalarını Web API kullanma

Sunucu tarafında çağırarak istemci sertifikasını alabilirsiniz [GetClientCertificate](https://msdn.microsoft.com/library/system.net.http.httprequestmessageextensions.getclientcertificate.aspx) üzerinde istek iletisi. Yöntemi istemci sertifikası ise null döndürür. Aksi takdirde, döndürür bir **X509Certificate2** örneği. Bu nesne, sertifika veren ve konu gibi bilgi almak için kullanın. Ardından kimlik doğrulama ve/veya yetkilendirme için bu bilgileri kullanabilirsiniz.

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample6.cs)]
