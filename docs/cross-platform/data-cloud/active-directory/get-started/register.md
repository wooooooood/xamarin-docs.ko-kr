---
title: '1단계: Azure Active Directory를 사용 하도록 앱을 등록 합니다.'
ms.prod: xamarin
ms.assetid: 0B17991A-4573-4F6C-9E86-D4B9D1A47E4D
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: c9a44a35e91e6368522f8632e185bd8a554d8593
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="step-1-register-an-app-to-use-azure-active-directory"></a>1단계: Azure Active Directory를 사용 하도록 앱을 등록 합니다.

1. 로 이동 [windowsazure.com](https://manage.windowsazure.com) Microsoft 계정 또는 Azure 포털에서 조직 계정으로 로그인 합니다. 평가판을 얻을 수 없다면 Azure 구독을 [azure.com](http://www.azure.com)

2. 로그인 한 후에 이동 된 **Active Directory** (1) 섹션 (2) 응용 프로그램 등록을 저장할 디렉터리 선택

  [ ![](register-images/01.-active-directory-in-azure-portal-sml.jpg "섹션 및 응용 프로그램 등록을 저장할 디렉터리 선택")](register-images/01.-active-directory-in-azure-portal.jpg#lightbox)

3. 클릭 **추가** 새 응용 프로그램을 만들려면 선택 **조직에서 개발 중인 응용 프로그램 추가**

  [ ![](register-images/02.-add-new-application-sml.jpg "조직에서 개발 중인 응용 프로그램 추가")](register-images/02.-add-new-application.jpg#lightbox)

4. 다음 화면에서 응용 프로그램에 이름을 지정 (예:. XAM-DEMO).
  선택 했는지 확인 **네이티브 클라이언트 응용 프로그램** 응용 프로그램의 형식입니다.

  ![](register-images/03.-app-name.jpg "응용 프로그램의 형식으로 네이티브 클라이언트 응용 프로그램을 선택 했는지 확인")

5. 마지막 화면에서 제공 된 **리디렉션 URI* 되에 응용 프로그램에 고유한 인증 완료 되 면이 URI를 반환 합니다.

  ![](register-images/04.-app-redirect.jpg "마지막 화면에서를 응용 프로그램에 고유한 인증 완료 되 면이 URI를 반환 합니다 리디렉션 URI를 제공 합니다.")

6. 응용 프로그램을 만든 후로 이동 된 **구성** 탭 합니다. 적어는 **클라이언트 ID** 나중에 응용 프로그램에서에서는입니다. 또한이 화면에서 Active Directory에 모바일 응용 프로그램 액세스를 부여 하거나 웹 API 또는 인증이 완료 되 면 모바일 응용 프로그램에서 사용할 수 있는 office 365와 같은 다른 응용 프로그램을 추가할 수 있습니다.

    ![](register-images/05.-configure.jpg "또한이 화면에서 Active Directory에 모바일 응용 프로그램 액세스를 부여 하거나 추가할 수 있습니다 웹 API 또는 office 365와 같은 다른 응용 프로그램")



## <a name="related-links"></a>관련 링크

- [Microsoft NativeClient 샘플](https://github.com/AzureADSamples/NativeClient-MultiTarget-DotNet)
