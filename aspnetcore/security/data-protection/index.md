---
title: "ASP.NET Çekirdeği'nde veri koruma"
author: rick-anderson
description: "Bu belge, çeşitli ASP.NET Core veri koruma konular için içindekiler tablosu olarak görev yapar."
keywords: ASP.NET Core, veri koruma
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 1f402da8-1052-4970-9835-9f9f16a02dbc
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/index
ms.openlocfilehash: cbf18c6ec867fefec22980f3e3493562594ef72d
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/11/2018
---
# <a name="data-protection-in-aspnet-core-consumer-apis-configuration-extensibility-apis-and-implementation"></a>ASP.NET Çekirdeği'nde veri koruma: tüketici API'leri, yapılandırma, genişletilebilirlik API'leri ve uygulama

* [Veri koruma giriş](introduction.md)

* [Veri Koruma API'ları ile çalışmaya başlama](using-data-protection.md)

* [Tüketici API'leri](consumer-apis/index.md)

  * [Tüketici API'leri genel bakış](consumer-apis/overview.md)

  * [Amaç dizeleri](consumer-apis/purpose-strings.md)

  * [Amaç hiyerarşisi ve çok kiracılılık](consumer-apis/purpose-strings-multitenancy.md)

  * [Parola karma](consumer-apis/password-hashing.md)

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

  * [Anahtar Yönetimi](implementation/key-management.md)

  * [Anahtar depolama sağlayıcıları](implementation/key-storage-providers.md)

  * [REST anahtar şifrelemesi](implementation/key-encryption-at-rest.md)

  * [Anahtar girişi ve ayarlarını değiştirme](implementation/key-immutability.md)

  * [Anahtar depolama biçimi](implementation/key-storage-format.md)

  * [Kısa ömürlü veri koruma sağlayıcıları](implementation/key-storage-ephemeral.md)

* [Uyumluluk](compatibility/index.md)

  * [Tanımlama bilgilerini uygulamalar arasında paylaşma](compatibility/cookie-sharing.md)

  * [ASP.NET’te <machineKey>‘i değiştirme](compatibility/replacing-machinekey.md)
