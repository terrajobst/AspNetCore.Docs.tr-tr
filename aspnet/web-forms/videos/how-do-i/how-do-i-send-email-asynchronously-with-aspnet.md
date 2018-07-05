---
uid: web-forms/videos/how-do-i/how-do-i-send-email-asynchronously-with-aspnet
title: '[Bunu nasıl yaparım:] ASP.NET ile zaman uyumsuz olarak e-posta gönderin | Microsoft Docs'
author: rick-anderson
description: Bu videoda, Chris piksel System.Net.Mail sınıflarının ASP.NET'te bir zaman uyumsuz bir e-posta iletisi göndermek için nasıl kullanılacağını gösterir. İlk olarak, bir web sitesi yapılandırma gör...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/24/2008
ms.topic: article
ms.assetid: 77a5c8fa-ebb2-426d-b56b-a5a98a46b516
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-send-email-asynchronously-with-aspnet
msc.type: video
ms.openlocfilehash: 69810f4c25b6b449168ca31af5df584c77d92e07
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37395934"
---
<a name="how-do-i-send-email-asynchronously-with-aspnet"></a><span data-ttu-id="119bf-104">[Bunu nasıl yaparım:] ASP.NET ile zaman uyumsuz olarak e-posta Gönder</span><span class="sxs-lookup"><span data-stu-id="119bf-104">[How Do I:] Send Email Asynchronously with ASP.NET</span></span>
====================
<span data-ttu-id="119bf-105">tarafından [Chris piksel](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="119bf-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="119bf-106">Bu videoda, Chris piksel System.Net.Mail sınıflarının ASP.NET'te bir zaman uyumsuz bir e-posta iletisi göndermek için nasıl kullanılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="119bf-106">In this video, Chris Pels shows how to use the System.Net.Mail classes in ASP.NET to send an asynchronous email message.</span></span> <span data-ttu-id="119bf-107">İlk olarak, e-posta ile göndermek için bir web sitesinin nasıl yapılandırılacağı bkz &lt;mailSettings&gt; web.config dosyasında öğesi.</span><span class="sxs-lookup"><span data-stu-id="119bf-107">First, see how to configure a web site to send email using the &lt;mailSettings&gt; element in the web.config file.</span></span> <span data-ttu-id="119bf-108">Ardından, e-posta bilgi girmek için basit bir kullanıcı arabirimi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="119bf-108">Next, create a simple user interface for entering email information.</span></span> <span data-ttu-id="119bf-109">Daha sonra nasıl oluşturulacağını öğrenin arka plan kod sayfası için bir e-posta iletisi oluşturmak için MailMessage sınıfını kullanın.</span><span class="sxs-lookup"><span data-stu-id="119bf-109">Then learn how to create use the MailMessage class to create an email message in the code behind for the page.</span></span> <span data-ttu-id="119bf-110">Bu işlemin bir parçası olarak e-postası gönderme aşağıdaki zaman uyumsuz geri çağırma için bir olay işleyicisi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="119bf-110">As part of that process create an event handler for the asynchronous callback following the sending of the email.</span></span> <span data-ttu-id="119bf-111">Olay işleyicisi, e-posta gönderme hakkında bilgi sağlayan AsynchCompletedEventArgs sınıf örneği kullanma işlemini bakın.</span><span class="sxs-lookup"><span data-stu-id="119bf-111">In the event handler see how to use the instance of the AsynchCompletedEventArgs class which provides information about the email sending process.</span></span> <span data-ttu-id="119bf-112">Son olarak, hata ayıklama modunda adımları izleyerek zaman uyumsuz olarak bir test e-posta gönderin ve işlemden alınan gerçek e-posta görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="119bf-112">Finally, send a test email asynchronously, following the steps in the debug mode, and view the actual email received from the process.</span></span>

[<span data-ttu-id="119bf-113">&#9654;Videoyu (18 dakika)</span><span class="sxs-lookup"><span data-stu-id="119bf-113">&#9654; Watch video (18 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-send-email-asynchronously-with-aspnet)
