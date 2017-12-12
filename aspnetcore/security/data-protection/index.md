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
ms.openlocfilehash: 8b42e65bb6121355120a6f4fbe8cd4d1fea153de
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="data-protection-in-aspnet-core-consumer-apis-configuration-extensibility-apis-and-implementation"></a>ASP.NET Çekirdeği'nde veri koruma: tüketici API'leri, yapılandırma, genişletilebilirlik API'leri ve uygulama

* [Veri koruma giriş](introduction.md)

* [Veri Koruma API'ları ile çalışmaya başlama](using-data-protection.md)

* [Tüketici API'leri](consumer-apis/index.md)

  * [Tüketici API'leri genel bakış](consumer-apis/overview.md)

  * [Amaç dizeleri](consumer-apis/purpose-strings.md)

  * [Amaç hiyerarşi ve çoklu kiracı](consumer-apis/purpose-strings-multitenancy.md)

  * [Parola karma](consumer-apis/password-hashing.md)

  * [Korumalı yüklerini ömrü sınırlama](consumer-apis/limited-lifetime-payloads.md)

  * [Kaldırmayı yükü, anahtarları iptal edildi](consumer-apis/dangerous-unprotect.md)

* [Yapılandırma](configuration/index.md)

  * [Veri korumasını yapılandırma](configuration/overview.md)

  * [Varsayılan ayarları](configuration/default-settings.md)

  * [Makine geniş İlkesi](configuration/machine-wide-policy.md)

  * [Olmayan dı kullanan senaryolar](configuration/non-di-scenarios.md)

* [Genişletilebilirlik API](extensibility/index.md)

  * [Çekirdek şifreleme genişletilebilirliği](extensibility/core-crypto.md)

  * [Anahtar Yönetimi genişletilebilirliği](extensibility/key-management.md)

  * [Çeşitli API'ler](extensibility/misc-apis.md)

* [Uygulama](implementation/index.md)

  * [Kimliği doğrulanmış şifreleme ayrıntıları](implementation/authenticated-encryption-details.md)

  * [Alt anahtar türetme ve kimliği doğrulanmış şifreleme](implementation/subkeyderivation.md)

  * [İçerik üstbilgileri](implementation/context-headers.md)

  * [Anahtar Yönetimi](implementation/key-management.md)

  * [Anahtar depolama sağlayıcıları](implementation/key-storage-providers.md)

  * [REST anahtar şifrelemesi](implementation/key-encryption-at-rest.md)

  * [Anahtar girişi ve ayarlarını değiştirme](implementation/key-immutability.md)

  * [Anahtar depolama biçimi](implementation/key-storage-format.md)

  * [Kısa ömürlü veri koruma sağlayıcısı](implementation/key-storage-ephemeral.md)

* [Uyumluluk](compatibility/index.md)

  * [Tanımlama bilgilerini uygulamalar arasında paylaşma](compatibility/cookie-sharing.md)

  * [Değiştirme <machineKey> ASP.NET](compatibility/replacing-machinekey.md)
