---
title: "ASP.NET Çekirdeği'nde veri koruma"
author: rick-anderson
description: "Bu belge, çeşitli ASP.NET Core veri koruma konular için içindekiler tablosu olarak görev yapar."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/index
ms.openlocfilehash: 7bbd203a67b32032ba2ab82448a5fc9a495b52aa
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="data-protection-in-aspnet-core-consumer-apis-configuration-extensibility-apis-and-implementation"></a>ASP.NET Çekirdeği'nde veri koruma: tüketici API'leri, yapılandırma, genişletilebilirlik API'leri ve uygulama

* [Veri korumaya giriş](introduction.md)

* [Veri Koruma API’lerini kullanmaya başlama](using-data-protection.md)

* [Tüketici API'leri](consumer-apis/index.md)

  * [Tüketici API’lerine genel bakış](consumer-apis/overview.md)

  * [Amaç dizeleri](consumer-apis/purpose-strings.md)

  * [Amaç hiyerarşisi ve çok kiracılılık](consumer-apis/purpose-strings-multitenancy.md)

  * [Parola karması](consumer-apis/password-hashing.md)

  * [Korumalı yüklerin ömrünü sınırlama](consumer-apis/limited-lifetime-payloads.md)

  * [Anahtarları iptal edilen yüklerin korumasını kaldırma](consumer-apis/dangerous-unprotect.md)

* [Yapılandırma](configuration/index.md)

  * [Veri korumasını yapılandırma](configuration/overview.md)

  * [Varsayılan ayarlar](configuration/default-settings.md)

  * [Makine geneli ilke](configuration/machine-wide-policy.md)

  * [DI kullanan olmayan senaryolar](configuration/non-di-scenarios.md)

* [Genişletilebilirlik API’leri](extensibility/index.md)

  * [Çekirdek şifreleme genişletilebilirliği](extensibility/core-crypto.md)

  * [Anahtar yönetimi genişletilebilirliği](extensibility/key-management.md)

  * [Çeşitli API'ler](extensibility/misc-apis.md)

* [Uygulama](implementation/index.md)

  * [Kimliği doğrulanmış şifreleme ayrıntıları](implementation/authenticated-encryption-details.md)

  * [Alt anahtar türetme ve kimliği doğrulanmış şifreleme](implementation/subkeyderivation.md)

  * [Bağlam üst bilgileri](implementation/context-headers.md)

  * [Anahtar yönetimi](implementation/key-management.md)

  * [Anahtar depolama sağlayıcıları](implementation/key-storage-providers.md)

  * [Bekleme durumunda anahtar şifreleme](implementation/key-encryption-at-rest.md)

  * [Anahtar değiştirilemezliği ve ayarları değiştirme](implementation/key-immutability.md)

  * [Anahtar depolama biçimi](implementation/key-storage-format.md)

  * [Kısa ömürlü veri koruma sağlayıcıları](implementation/key-storage-ephemeral.md)

* [Uyumluluk](compatibility/index.md)

  * [Tanımlama bilgilerini uygulamalar arasında paylaşma](compatibility/cookie-sharing.md)

  * [ASP.NET’te <machineKey>‘i değiştirme](compatibility/replacing-machinekey.md)
