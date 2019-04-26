---
title: '1단계: Azure Active Directory를 사용 하도록 앱 등록'
description: 이 문서에는 모바일 클라이언트에서 안전 하 게 액세스할 수 있도록 Azure Active Directory를 사용 하 여 Azure 응용 프로그램을 등록 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 0B17991A-4573-4F6C-9E86-D4B9D1A47E4D
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 7f1e743eea81edc0aa45b49f6acb6a9fd461bc80
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61188252"
---
# <a name="step-1-register-an-app-to-use-azure-active-directory"></a>1단계: Azure Active Directory를 사용 하도록 앱 등록

1. 이동할 [windowsazure.com](https://manage.windowsazure.com) Microsoft 계정 또는 Azure 포털에서 조직 계정으로 로그인 합니다. Azure 구독이 없으면에서 평가판을 얻을 수 있습니다 [azure.com](http://www.azure.com)

2. 로그인 한 후로 이동 합니다 **Active Directory** (1) 섹션 (2) 응용 프로그램을 등록 하려는 디렉터리를 선택 합니다.

  [ ![](register-images/01.-active-directory-in-azure-portal-sml.jpg "섹션 및 응용 프로그램을 등록 하려는 디렉터리를 선택 합니다.")](register-images/01.-active-directory-in-azure-portal.jpg#lightbox)

3. 클릭 **추가** 새 응용 프로그램을 만들려면 선택한 **내 조직에서 개발 중인 응용 프로그램 추가**

  [ ![](register-images/02.-add-new-application-sml.jpg "내 조직에서 개발 중인 응용 프로그램 추가")](register-images/02.-add-new-application.jpg#lightbox)

4. 다음 화면에서 앱 이름을 지정 (예: XAM-DEMO).
  선택 했는지 **네이티브 클라이언트 응용 프로그램** 응용 프로그램의 형식으로 합니다.

  ![](register-images/03.-app-name.jpg "응용 프로그램의 형식으로 네이티브 클라이언트 응용 프로그램을 선택 했는지 확인")

5. 최종 화면에서 제공 된 **리디렉션 URI* 는 응용 프로그램에 고유한 인증이 완료 되 면이 URI로 반환 됩니다.

  ![](register-images/04.-app-redirect.jpg "마지막 화면에서이 URI로 반환 됩니다 인증이 완료 되 면 응용 프로그램에 고유한는 리디렉션 URI 제공")

6. 앱이 만들어지면로 이동 합니다 **구성** 탭 합니다. 적어 합니다 **클라이언트 ID** 나중에 응용 프로그램에서 사용 됩니다. 또한이 화면에서 Active Directory에 모바일 응용 프로그램 액세스를 제공 하거나 웹 API 또는 인증이 완료 되 면 모바일 응용 프로그램에서 사용할 수 있는 office 365와 같은 다른 응용 프로그램을 추가할 수 있습니다.

    ![](register-images/05.-configure.jpg "또한이 화면에서 Active Directory에 모바일 응용 프로그램 액세스를 제공 하거나 추가할 수 있습니다 웹 API 또는 office 365와 같은 다른 응용 프로그램")



## <a name="related-links"></a>관련 링크

- [Microsoft NativeClient 샘플](https://github.com/AzureADSamples/NativeClient-MultiTarget-DotNet)
