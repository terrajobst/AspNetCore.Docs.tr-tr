---
uid: web-forms/videos/how-do-i/how-do-i-send-email-asynchronously-with-aspnet
title: '[Nasıl stop yaparım] ASP.NET ile zaman uyumsuz olarak e-posta gönderme | Microsoft Docs'
author: rick-anderson
description: Bu videoda, Chris Pels System.Net.Mail sınıflarının ASP.NET bir zaman uyumsuz e-posta iletisi göndermek için nasıl kullanılacağını gösterir. İlk olarak, bir web si yapılandırmak bkz....
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/24/2008
ms.topic: article
ms.assetid: 77a5c8fa-ebb2-426d-b56b-a5a98a46b516
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-send-email-asynchronously-with-aspnet
msc.type: video
ms.openlocfilehash: a9e35a8fe3a6918da712e5f12c75937ef7f6e76d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
ms.locfileid: "26572067"
---
<a name="how-do-i-send-email-asynchronously-with-aspnet"></a><span data-ttu-id="3f0fa-104">[Nasıl stop yaparım] ASP.NET ile zaman uyumsuz olarak e-posta Gönder</span><span class="sxs-lookup"><span data-stu-id="3f0fa-104">[How Do I:] Send Email Asynchronously with ASP.NET</span></span>
====================
<span data-ttu-id="3f0fa-105">tarafından [Chris Pels](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="3f0fa-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="3f0fa-106">Bu videoda, Chris Pels System.Net.Mail sınıflarının ASP.NET bir zaman uyumsuz e-posta iletisi göndermek için nasıl kullanılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="3f0fa-106">In this video, Chris Pels shows how to use the System.Net.Mail classes in ASP.NET to send an asynchronous email message.</span></span> <span data-ttu-id="3f0fa-107">İlk olarak, e-posta kullanarak göndermek için bir web sitesinin nasıl yapılandırılacağı bkz &lt;mailSettings&gt; web.config dosyasında öğesi.</span><span class="sxs-lookup"><span data-stu-id="3f0fa-107">First, see how to configure a web site to send email using the &lt;mailSettings&gt; element in the web.config file.</span></span> <span data-ttu-id="3f0fa-108">Ardından, e-posta bilgi girmek için basit bir kullanıcı arabirimi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="3f0fa-108">Next, create a simple user interface for entering email information.</span></span> <span data-ttu-id="3f0fa-109">Nasıl oluşturulacağını öğrenin posta iletisi sınıfı arka plan kod sayfası için bir e-posta iletisi oluşturmak için kullanın.</span><span class="sxs-lookup"><span data-stu-id="3f0fa-109">Then learn how to create use the MailMessage class to create an email message in the code behind for the page.</span></span> <span data-ttu-id="3f0fa-110">Bu işlemin bir parçası olarak e-posta gönderme aşağıdaki zaman uyumsuz geri çağırma için bir olay işleyicisi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="3f0fa-110">As part of that process create an event handler for the asynchronous callback following the sending of the email.</span></span> <span data-ttu-id="3f0fa-111">Olay işleyici işlemi gönderme e-posta hakkında bilgi sağlayan AsynchCompletedEventArgs sınıfı örneğini kullanacak şekilde nasıl bakın.</span><span class="sxs-lookup"><span data-stu-id="3f0fa-111">In the event handler see how to use the instance of the AsynchCompletedEventArgs class which provides information about the email sending process.</span></span> <span data-ttu-id="3f0fa-112">Son olarak, aşağıdaki adımları hata ayıklama modunda bir test e-posta zaman uyumsuz olarak gönderme ve işleminden alınan gerçek e-posta görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="3f0fa-112">Finally, send a test email asynchronously, following the steps in the debug mode, and view the actual email received from the process.</span></span>

[<span data-ttu-id="3f0fa-113">&#9654; (18 dakika) videoyu izleyin</span><span class="sxs-lookup"><span data-stu-id="3f0fa-113">&#9654; Watch video (18 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-send-email-asynchronously-with-aspnet)
