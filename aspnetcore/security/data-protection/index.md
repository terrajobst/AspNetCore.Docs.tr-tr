---
title: ASP.NET Çekirdeği'nde veri koruma
author: rick-anderson
description: Bu belge, çeşitli ASP.NET Core veri koruma konular için içindekiler tablosu olarak görev yapar.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/index
ms.openlocfilehash: 83b5bb1e6a4942a4d3e5ec0d445fa6e5a21fb533
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2018
---
# <a name="data-protection-in-aspnet-core"></a><span data-ttu-id="fce25-103">ASP.NET Çekirdeği'nde veri koruma</span><span class="sxs-lookup"><span data-stu-id="fce25-103">Data Protection in ASP.NET Core</span></span>

* [<span data-ttu-id="fce25-104">Veri korumaya giriş</span><span class="sxs-lookup"><span data-stu-id="fce25-104">Introduction to data protection</span></span>](xref:security/data-protection/introduction)

* [<span data-ttu-id="fce25-105">Veri Koruma API’lerini kullanmaya başlama</span><span class="sxs-lookup"><span data-stu-id="fce25-105">Get started with the Data Protection APIs</span></span>](xref:security/data-protection/using-data-protection)

* [<span data-ttu-id="fce25-106">Tüketici API'leri</span><span class="sxs-lookup"><span data-stu-id="fce25-106">Consumer APIs</span></span>](xref:security/data-protection/consumer-apis/index)

  * [<span data-ttu-id="fce25-107">Tüketici API’lerine genel bakış</span><span class="sxs-lookup"><span data-stu-id="fce25-107">Consumer APIs overview</span></span>](xref:security/data-protection/consumer-apis/overview)

  * [<span data-ttu-id="fce25-108">Amaç dizeleri</span><span class="sxs-lookup"><span data-stu-id="fce25-108">Purpose strings</span></span>](xref:security/data-protection/consumer-apis/purpose-strings)

  * [<span data-ttu-id="fce25-109">Amaç hiyerarşisi ve çok kiracılılık</span><span class="sxs-lookup"><span data-stu-id="fce25-109">Purpose hierarchy and multi-tenancy</span></span>](xref:security/data-protection/consumer-apis/purpose-strings-multitenancy)

  * [<span data-ttu-id="fce25-110">Karma parolalar</span><span class="sxs-lookup"><span data-stu-id="fce25-110">Hash passwords</span></span>](xref:security/data-protection/consumer-apis/password-hashing)

  * [<span data-ttu-id="fce25-111">Korumalı yüklerin ömrünü sınırlama</span><span class="sxs-lookup"><span data-stu-id="fce25-111">Limit the lifetime of protected payloads</span></span>](xref:security/data-protection/consumer-apis/limited-lifetime-payloads)

  * [<span data-ttu-id="fce25-112">Anahtarları iptal edilen yüklerin korumasını kaldırma</span><span class="sxs-lookup"><span data-stu-id="fce25-112">Unprotect payloads whose keys have been revoked</span></span>](xref:security/data-protection/consumer-apis/dangerous-unprotect)

* [<span data-ttu-id="fce25-113">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="fce25-113">Configuration</span></span>](xref:security/data-protection/configuration/index)

  * [<span data-ttu-id="fce25-114">ASP.NET Core veri korumasını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="fce25-114">Configure ASP.NET Core Data Protection</span></span>](xref:security/data-protection/configuration/overview)

  * [<span data-ttu-id="fce25-115">Varsayılan ayarlar</span><span class="sxs-lookup"><span data-stu-id="fce25-115">Default settings</span></span>](xref:security/data-protection/configuration/default-settings)

  * [<span data-ttu-id="fce25-116">Makine geneli ilke</span><span class="sxs-lookup"><span data-stu-id="fce25-116">Machine-wide policy</span></span>](xref:security/data-protection/configuration/machine-wide-policy)

  * [<span data-ttu-id="fce25-117">DI kullanan olmayan senaryolar</span><span class="sxs-lookup"><span data-stu-id="fce25-117">Non DI-aware scenarios</span></span>](xref:security/data-protection/configuration/non-di-scenarios)

* [<span data-ttu-id="fce25-118">Genişletilebilirlik API’leri</span><span class="sxs-lookup"><span data-stu-id="fce25-118">Extensibility APIs</span></span>](xref:security/data-protection/extensibility/index)

  * [<span data-ttu-id="fce25-119">Çekirdek şifreleme genişletilebilirliği</span><span class="sxs-lookup"><span data-stu-id="fce25-119">Core cryptography extensibility</span></span>](xref:security/data-protection/extensibility/core-crypto)

  * [<span data-ttu-id="fce25-120">Anahtar yönetimi genişletilebilirliği</span><span class="sxs-lookup"><span data-stu-id="fce25-120">Key management extensibility</span></span>](xref:security/data-protection/extensibility/key-management)

  * [<span data-ttu-id="fce25-121">Çeşitli API'ler</span><span class="sxs-lookup"><span data-stu-id="fce25-121">Miscellaneous APIs</span></span>](xref:security/data-protection/extensibility/misc-apis)

* [<span data-ttu-id="fce25-122">Uygulama</span><span class="sxs-lookup"><span data-stu-id="fce25-122">Implementation</span></span>](xref:security/data-protection/implementation/index)

  * [<span data-ttu-id="fce25-123">Kimliği doğrulanmış şifreleme ayrıntıları</span><span class="sxs-lookup"><span data-stu-id="fce25-123">Authenticated encryption details</span></span>](xref:security/data-protection/implementation/authenticated-encryption-details)

  * [<span data-ttu-id="fce25-124">Alt anahtar türetme ve kimliği doğrulanmış şifreleme</span><span class="sxs-lookup"><span data-stu-id="fce25-124">Subkey derivation and authenticated encryption</span></span>](xref:security/data-protection/implementation/subkeyderivation)

  * [<span data-ttu-id="fce25-125">Bağlam üst bilgileri</span><span class="sxs-lookup"><span data-stu-id="fce25-125">Context headers</span></span>](xref:security/data-protection/implementation/context-headers)

  * [<span data-ttu-id="fce25-126">Anahtar yönetimi</span><span class="sxs-lookup"><span data-stu-id="fce25-126">Key management</span></span>](xref:security/data-protection/implementation/key-management)

  * [<span data-ttu-id="fce25-127">Anahtar depolama sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="fce25-127">Key storage providers</span></span>](xref:security/data-protection/implementation/key-storage-providers)

  * [<span data-ttu-id="fce25-128">Bekleme durumunda anahtar şifreleme</span><span class="sxs-lookup"><span data-stu-id="fce25-128">Key encryption at rest</span></span>](xref:security/data-protection/implementation/key-encryption-at-rest)

  * [<span data-ttu-id="fce25-129">Anahtar girişi ve ayarları</span><span class="sxs-lookup"><span data-stu-id="fce25-129">Key immutability and settings</span></span>](xref:security/data-protection/implementation/key-immutability)

  * [<span data-ttu-id="fce25-130">Anahtar depolama biçimi</span><span class="sxs-lookup"><span data-stu-id="fce25-130">Key storage format</span></span>](xref:security/data-protection/implementation/key-storage-format)

  * [<span data-ttu-id="fce25-131">Kısa ömürlü veri koruma sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="fce25-131">Ephemeral data protection providers</span></span>](xref:security/data-protection/implementation/key-storage-ephemeral)

* [<span data-ttu-id="fce25-132">Uyumluluk</span><span class="sxs-lookup"><span data-stu-id="fce25-132">Compatibility</span></span>](xref:security/data-protection/compatibility/index)

  * [<span data-ttu-id="fce25-133">ASP.NET değiştirme <machineKey> ASP.NET Core içinde</span><span class="sxs-lookup"><span data-stu-id="fce25-133">Replacing ASP.NET <machineKey> in ASP.NET Core</span></span>](xref:security/data-protection/compatibility/replacing-machinekey)
