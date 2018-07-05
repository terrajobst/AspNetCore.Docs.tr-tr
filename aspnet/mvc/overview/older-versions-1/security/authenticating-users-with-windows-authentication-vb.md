---
uid: mvc/overview/older-versions-1/security/authenticating-users-with-windows-authentication-vb
title: Windows kimlik doğrulama (VB) ile kullanıcıların kimliğini doğrulama | Microsoft Docs
author: microsoft
description: Bir MVC uygulaması bağlamında Windows kimlik doğrulaması kullanmayı öğrenin. Uygulamanızın web ortak içinde Windows kimlik doğrulamasını etkinleştirmek öğrenin...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 532fa051-7d5c-4d6d-87f6-339ce4b84c44
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/security/authenticating-users-with-windows-authentication-vb
msc.type: authoredcontent
ms.openlocfilehash: fd14ae32c286fb67cf75cc103f6a7969c4b5a731
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37396695"
---
<a name="authenticating-users-with-windows-authentication-vb"></a>Windows kimlik doğrulama (VB) ile kullanıcıların kimliğini doğrulama
====================
tarafından [Microsoft](https://github.com/microsoft)

> Bir MVC uygulaması bağlamında Windows kimlik doğrulaması kullanmayı öğrenin. Uygulamanızın web yapılandırma dosyası içinde Windows kimlik doğrulamasının nasıl etkinleştirileceği ve IIS ile kimlik doğrulaması yapılandırma konusunda bilgi edinin. Son olarak, belirli Windows kullanıcıları veya grupları için denetleyici eylemleri için erişimi kısıtlamak için [Authorize] özniteliği kullanmayı öğrenin.


Nasıl yararlanabileceğinizi açıklayan Bu öğreticinin amacı olan MVC uygulamalarınızı görünümlerde parola için Internet Information Services içinde yerleşik özellikler güvenliğinin korunmasına. Denetleyici eylemleri yalnızca belirli Windows kullanıcıları veya belirli Windows gruplarının üyeleri olan kullanıcılar tarafından çağrılmasına izin öğrenin.

Windows kimlik doğrulaması kullanarak bir şirket içi Web sitesinde (bir intranet siteye) oluşturuyorsanız ve standart Windows kullanıcı adlarını ve parolaları Web sitesi erişirken kullanılacak, kullanıcıların istediğinizde mantıklıdır. Web sitesi (bir Internet Web sitesi)'e yönelik bir outwards oluşturuyorsanız form kimlik doğrulaması kullanmayı düşünün.

#### <a name="enabling-windows-authentication"></a>Windows kimlik doğrulamasını etkinleştirme

Yeni bir ASP.NET MVC uygulaması oluşturduğunuzda, Windows kimlik doğrulaması varsayılan olarak etkin değil. Form kimlik doğrulaması etkin MVC uygulamaları için varsayılan kimlik doğrulaması türüdür. MVC uygulamanızda web yapılandırma (web.config) dosyasını değiştirerek, Windows kimlik doğrulamasını etkinleştirmeniz gerekir. Bulma &lt;kimlik doğrulaması&gt; bölümünde ve bunun gibi form kimlik doğrulaması yerine Windows kullanacak şekilde değiştirin:

[!code-xml[Main](authenticating-users-with-windows-authentication-vb/samples/sample1.xml)]

Windows kimlik doğrulamasını etkinleştirdiğinizde, web sunucunuza kullanıcıların kimliklerinin doğrulanması için sorumlu olur. Genellikle, iki farklı türde oluşturma ve bir ASP.NET MVC uygulamasını dağıtırken kullandığınız web sunucuları vardır.

İlk olarak, bir MVC Uygulama geliştirirken, Visual Studio'ya dahil edildi ASP.NET Geliştirme Web sunucusunu kullanın. Varsayılan olarak, ASP.NET Geliştirme Web sunucusu (Windows oturum açmak için kullandığınız hangi hesabı) geçerli Windows hesabı bağlamında tüm sayfaları yürütür.

ASP.NET Geliştirme Web sunucusu, NTLM kimlik doğrulamasını da destekler. Çözüm Gezgini penceresinde projenizin adını sağ tıklatıp Özellikler'i seçerek, NTLM kimlik doğrulamasını etkinleştirebilirsiniz. Ardından, Web sekmesini seçin ve NTLM onay (bkz. Şekil 1).

**Şekil 1: etkinleştirme ASP.NET Geliştirme Web sunucusu için NTLM kimlik doğrulaması**

![clip_image002](authenticating-users-with-windows-authentication-vb/_static/image1.jpg)

Diğer yandan, bir üretim web uygulaması için size, web sunucusu olarak IIS kullanın. IIS kimlik doğrulaması dahil olmak üzere çeşitli türlerini destekler:

- Temel kimlik doğrulaması – HTTP 1.0 protokolünün bir parçası tanımlanır. Kullanıcı adları ve parolalar düz metin (şifreli Base64) Internet üzerinden gönderir. -Özet kimlik doğrulaması – Internet üzerinden parola kendisi yerine bir parola karmasını gönderir. -Tümleşik Windows (NTLM) kimlik doğrulaması – kimlik doğrulaması, windows kullanarak intranet ortamlarında kullanmak için en iyi türü. -Sertifika kimlik doğrulaması – bir istemci-tarafı sertifikasını kullanarak kimlik doğrulamayı etkinleştirir. Sertifika, bir Windows kullanıcı hesabına eşlenir.

> [!NOTE] 
> 
> Bu farklı kimlik doğrulama türleri daha ayrıntılı bir genel bakış için bkz [ https://msdn.microsoft.com/library/aa292114(VS.71).aspx ](https://msdn.microsoft.com/library/aa292114(VS.71).aspx).


Belirli bir kimlik doğrulama türü etkinleştirmek için Internet Information Services Manager'ı kullanabilirsiniz. Tüm kimlik doğrulama türlerinin her işletim sistemi söz konusu olduğunda mevcut olmadığına dikkat edin. Ayrıca, IIS 7.0, Windows Vista ile kullanıyorsanız, Internet Information Services Manager'da göründükleri önce Windows kimlik doğrulaması farklı türde etkinleştirmeniz gerekir. Açık **Denetim Masası, programlar, programlar ve özellikler, kapatma Windows özelliklerini aç veya Kapat**, Internet Information Services düğümünü genişletin (bkz: Şekil 2).

**Şekil 2 – etkinleştirme Windows IIS özellikleri**

![clip_image004](authenticating-users-with-windows-authentication-vb/_static/image2.jpg)

Internet Information Services'ı kullanarak, etkinleştirebilir veya farklı türde kimlik doğrulaması devre dışı bırakın. Örneğin, Şekil 3 IIS 7.0 kullanırken devre dışı bırakma anonim kimlik doğrulaması ve etkinleştirme tümleşik Windows (NTLM) kimlik doğrulaması göstermektedir.

**Şekil 3: tümleşik Windows kimlik doğrulamasını etkinleştirme**

![clip_image006](authenticating-users-with-windows-authentication-vb/_static/image3.jpg)

#### <a name="authorizing-windows-users-and-groups"></a>Windows yetkilendirme kullanıcılar ve gruplar

Windows kimlik doğrulaması etkinleştirdikten sonra kullanabileceğiniz &lt;Authorize&gt; denetleyicileri veya denetleyici eylemleri erişimi denetlemek için özniteliği. Bu öznitelik tüm MVC denetleyicisi veya belirli bir denetleyici eylemi için uygulanabilir.

Örneğin, 1 listeleme giriş denetleyicisine İNDİS() CompanySecrets() ve StephenSecrets() adlı üç eylem kullanıma sunar. Herkes İNDİS() eylemini çağırabilirsiniz. Ancak, yalnızca Windows yerel Yöneticiler grubunun üyeleri CompanySecrets() eylemini çağırabilirsiniz. Son olarak, yalnızca Windows etki alanı kullanıcı (etki alanı Redmond), Stephen adlı StephenSecrets() eylemini çağırabilirsiniz.

**1 – Controllers\HomeController.vb listeleme**

[!code-vb[Main](authenticating-users-with-windows-authentication-vb/samples/sample2.vb)]

> [!NOTE]
> Windows kullanıcı hesabı denetimi (Windows Vista veya Windows Server 2008 ile çalışırken UAC nedeniyle), yerel Yöneticiler grubu diğer gruplara farklı davranır. &lt;Authorize&gt; özniteliği olmaz doğru bir şekilde tanımak yerel Yöneticiler grubunun bir üyesi, bilgisayarınızın UAC ayarları değiştirmediğiniz sürece.


Tam olarak ne olur, doğru izinler olmaksızın bir denetleyici eylemi çağırmak istediğinizde, etkin kimlik doğrulama türüne bağlıdır. Varsayılan ASP.NET Geliştirme Sunucusu kullanırken, sadece boş bir sayfa alın. Sayfa ile sunulan bir **401 yetkilendirilmedi** HTTP yanıt durumu.

Öte yandan, anonim kimlik doğrulamasını devre dışı ve temel kimlik doğrulaması etkin ile IIS kullanarak ve ardından korumalı sayfanın istek her zaman bir oturum açma iletişim kutusu metni almaya devam etmek, (bkz: Şekil 4).

**Şekil 4 – temel kimlik doğrulaması oturum açma iletişim kutusu**

![clip_image008](authenticating-users-with-windows-authentication-vb/_static/image4.jpg)

#### <a name="summary"></a>Özet

Bu öğretici, Windows kimlik doğrulaması bir ASP.NET MVC uygulaması bağlamında nasıl kullanabileceğinizi açıklanmıştır. Uygulamanızın web yapılandırma dosyası içinde Windows kimlik doğrulamasının nasıl etkinleştirileceği ve IIS ile kimlik doğrulamasını yapılandırma öğrendiniz. Son olarak, size nasıl kullanacağınızı öğrendiniz &lt;Authorize&gt; belirli Windows kullanıcıları veya grupları için denetleyici eylemleri için erişimi kısıtlamak için özniteliği.

> [!div class="step-by-step"]
> [Önceki](authenticating-users-with-forms-authentication-vb.md)
> [İleri](preventing-javascript-injection-attacks-vb.md)
