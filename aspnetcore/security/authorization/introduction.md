---
title: ASP.NET Core yetkilendirme giriş
author: rick-anderson
description: Yetkilendirme ve yetkilendirme ASP.NET Core uygulamalarında nasıl çalıştığı hakkındaki temel bilgileri öğrenin.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/introduction
ms.openlocfilehash: 5465eb7875ebecd77b628376ef886db0ddd05025
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64902846"
---
# <a name="introduction-to-authorization-in-aspnet-core"></a>ASP.NET Core yetkilendirme giriş

<a name="security-authorization-introduction"></a>

Yetkilendirme ne belirleyen işleme başvuran bir kullanıcı işlemleri gerçekleştirebilirsiniz. Örneğin, bir belge kitaplığı oluşturma, belgelere ekleme, belgeleri düzenlemeye ve silmek için bir yönetici kullanıcı izin. Kitaplıkla çalışırken yönetici olmayan bir kullanıcı yalnızca belgeleri okumak için yetkili.

Yetkilendirme, dikey ve kimlik doğrulaması'dan bağımsız gerçekleşir. Ancak, bir kimlik doğrulama mekanizması yetkilendirme gerektirir. Kimlik doğrulaması kullanan bir kullanıcıdır ascertaining işlemidir. Kimlik doğrulaması, geçerli kullanıcı için bir veya daha fazla kimlikleri oluşturabilir.

## <a name="authorization-types"></a>Yetkilendirme türleri

ASP.NET Core yetkilendirme sağlayan basit, bildirim temelli [rol](xref:security/authorization/roles) ve zengin [ilke tabanlı](xref:security/authorization/policies) modeli. Yetkilendirme gereksinimleri ifade edilir ve işleyicileri bir kullanıcının talebi gereksinimleriyle karşılaştırarak değerlendirmek. Kesinlik temelli denetimleri basit ilkeleri veya kullanıcı kimliği ve kullanıcının erişmeye çalıştığı kaynak özelliklerini değerlendirmek ilkeleri temel alabilir.

## <a name="namespaces"></a>Ad Alanları

Yetkilendirme bileşenlerine dahil `AuthorizeAttribute` ve `AllowAnonymousAttribute` öznitelikleri içinde bulunan `Microsoft.AspNetCore.Authorization` ad alanı.

Üzerinde belgelerine [basit yetkilendirme](xref:security/authorization/simple).
