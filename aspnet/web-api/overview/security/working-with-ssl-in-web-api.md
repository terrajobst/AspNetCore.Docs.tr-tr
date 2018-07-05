---
uid: web-api/overview/security/working-with-ssl-in-web-api
title: Web API'de SSL ile çalışma | Microsoft Docs
author: MikeWasson
description: SSL SSL istemci sertifikaları kullanma dahil olmak üzere ASP.NET Web API ile kullanma işlemi gösterilmektedir.
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/12/2012
ms.topic: article
ms.assetid: 97f6164f-59cf-45c0-b820-e4aa29b45396
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/security/working-with-ssl-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 4e14607f11fcd376b4ceca4bf7862259abe015e6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37396636"
---
<a name="working-with-ssl-in-web-api"></a>Web API'de SSL ile çalışma
====================
tarafından [Mike Wasson](https://github.com/MikeWasson)

Birçok yaygın kimlik doğrulama düzenleri düz HTTP üzerinden güvenli değildir. Özellikle temel kimlik doğrulaması ve form kimlik doğrulaması şifrelenmemiş kimlik bilgileri gönderin. Güvenli olması için bu kimlik doğrulama düzenleri *gerekir* SSL kullanın. Ayrıca, SSL istemci sertifikaları, istemcilerin kimliğini doğrulamak için kullanılabilir.

## <a name="enabling-ssl-on-the-server"></a>Sunucuda SSL etkinleştirme

SSL kurma IIS 7 veya üzeri için:

- Oluşturun veya bir sertifika alın. Test için otomatik olarak imzalanan bir sertifika oluşturabilirsiniz.
- Bir HTTPS bağlaması ekleyin.

Ayrıntılar için bkz [SSL ayarlama IIS 7 üzerinde nasıl](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis).

Yerel test etmek için Visual Studio'dan IIS Express SSL etkinleştirebilirsiniz. Özellikler penceresinde ayarlayın **SSL etkin** için **True**. Değerini not edin **SSL URL**; HTTPS bağlantıları test etmek için bu URL'yi kullanın.

![](working-with-ssl-in-web-api/_static/image1.png)

### <a name="enforcing-ssl-in-a-web-api-controller"></a>Bir Web API denetleyicisi, SSL'yi zorunlu tutma

Bir HTTPS hem bir HTTP bağlaması varsa, istemciler siteye erişmek için HTTP yine de kullanabilirsiniz. Bazı kaynaklar diğer kaynaklara SSL gerektirirken HTTP kullanılabilir olmasını sağlayabilir. Bu durumda, bir eyleme eylem filtresi korumalı kaynaklar için SSL gerektirmek için kullanın. Aşağıdaki kod, SSL için denetleyen bir Web API kimlik doğrulaması filtre gösterir:

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample1.cs)]

Bu filtre, SSL iste herhangi bir Web API eylemlerini ekleyin:

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample2.cs)]

## <a name="ssl-client-certificates"></a>SSL istemci sertifikaları

SSL, ortak anahtar altyapısı sertifikalarını kullanarak kimlik doğrulaması sağlar. Sunucu, sunucuda istemci kimliğini doğrulayan bir sertifika sağlamanız gerekir. Sunucu için bir sertifika sağlamak için istemcinin daha az yaygın bir durumdur, ancak istemcilerin kimliğini doğrulamak için bir seçenek budur. İstemci sertifikaları, SSL ile kullanmak için kullanıcılarınız için otomatik imzalı sertifikaları dağıtmak için bir yol gerekir. Birçok uygulama türü, bu iyi bir kullanıcı deneyimi olmayacak, ancak bazı ortamlarda (örneğin, Kurumsal) uygun olabilir.

| Yararları | Dezavantajlar |
| --- | --- |
| -Kullanıcı adı/parola daha güçlü sertifika kimlik bilgileridir. -Kimlik doğrulama, ileti bütünlüğü ve ileti şifreleme tam güvenli kanalı SSL sağlar. | -Edinmeli ve PKI sertifikalarını yönetin. -İstemci Platformu SSL istemci sertifikaları desteklemesi gerekir. |

İstemci sertifikalarını kabul etmek üzere IIS'yi yapılandırmak için IIS Yöneticisi'ni açın ve aşağıdaki adımları gerçekleştirin:

1. Ağaç görünümünde site düğümünü tıklatın.
2. Çift **SSL ayarları** Orta bölmede özelliği.
3. Altında **istemci sertifikaları**, aşağıdaki seçeneklerden birini seçin: 

    - **Kabul**: IIS istemci sertifika kabul eder, ancak bir gerektirmez.
    - **Gerekli**: bir istemci sertifikası gerektirir. (Bu seçeneği etkinleştirmek için de "SSL iste" seçmeniz gerekir)

Bu gibi durumlarda, bu seçenekler ayrıca ApplicationHost.config dosyasında ayarlayabilirsiniz:

[!code-xml[Main](working-with-ssl-in-web-api/samples/sample3.xml)]

**SslNegotiateCert** bayrağı anlamına gelir IIS istemci sertifika kabul eder, ancak biri ("Kabul et" seçeneğini IIS Yöneticisi'nde eşdeğer) gerektirmez. Bir sertifika gerektirecek şekilde **SslRequireCert** bayrağı. Test etmek için de bu seçenekler IIS Express'te URL'i yerel applicationhost içinde ayarlayabilirsiniz. "Documents\IISExpress\config" içinde bulunan yapılandırma dosyası.

### <a name="creating-a-client-certificate-for-testing"></a>Test etmek için bir istemci sertifikası oluşturma

Test amacıyla kullanabilirsiniz [MakeCert.exe](https://msdn.microsoft.com/library/bfsktky3.aspx) bir istemci sertifikası oluşturmak için. İlk olarak, bir test kök yetkilisi oluşturun:

[!code-console[Main](working-with-ssl-in-web-api/samples/sample4.cmd)]

MakeCert özel anahtarı için bir parola girmenizi ister.

Ardından, sertifika sunucunun "güvenilen kök sertifika yetkilileri" mağazasından, aşağıdaki şekilde test ekleyin:

1. MMC'yi açın.
2. Altında **dosya**seçin **Ekle/Kaldır ek bileşenini**.
3. Seçin **bilgisayar hesabı**.
4. Seçin **yerel bilgisayar** ve Sihirbazı tamamlayın.
5. Altındaki gezinme bölmesinde, "Güvenilen kök sertifika yetkilileri" düğümünü genişletin.
6. Üzerinde **eylem** menüsünde **tüm görevler**ve ardından **alma** Sertifika Alma Sihirbazı'nı başlatın.
7. TempCA.cer sertifika dosyasına göz atın.
8. Tıklayın **açık**, ardından **sonraki** ve Sihirbazı tamamlayın. (, Parolayı yeniden girmeniz istenir.)

Şimdi ilk sertifika tarafından imzalanmış bir istemci sertifikası oluşturun:

[!code-console[Main](working-with-ssl-in-web-api/samples/sample5.cmd)]

### <a name="using-client-certificates-in-web-api"></a>İstemci sertifikaları Web API'si kullanma

Sunucu tarafında çağırarak istemci sertifikası edinebilirsiniz [GetClientCertificate](https://msdn.microsoft.com/library/system.net.http.httprequestmessageextensions.getclientcertificate.aspx) istek iletisinde. Yöntemi, istemci sertifikası yok ise null döndürür. Aksi halde bir **X509Certificate2** örneği. Sertifikadan konu ve veren gibi bilgileri almak için bu nesneyi kullanırsınız. Ardından bu bilgileri kimlik doğrulaması ve/veya yetkilendirme için kullanabilirsiniz.

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample6.cs)]
