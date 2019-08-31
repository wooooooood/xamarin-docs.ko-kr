---
title: 1단계. Azure Active Directory 사용할 앱 등록
description: 이 문서에서는 모바일 클라이언트에서 안전 하 게 액세스할 수 있도록 Azure Active Directory에 Azure 응용 프로그램을 등록 하는 방법에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: 0B17991A-4573-4F6C-9E86-D4B9D1A47E4D
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 732c1ee241b39a4bb1422b8c27820631bf8c6b0c
ms.sourcegitcommit: 1e3a0d853669dcc57d5dee0894d325d40c7d8009
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2019
ms.locfileid: "70199056"
---
# <a name="step-1-register-an-app-to-use-azure-active-directory"></a>1단계. Azure Active Directory 사용할 앱 등록

1. [Windowsazure.com](https://manage.windowsazure.com) 으로 이동 하 여 Azure Portal에서 Microsoft 계정 또는 조직 계정으로 로그인 합니다. Azure 구독이 없는 경우 [azure.com](https://www.azure.com) 에서 평가판을 받을 수 있습니다.

2. 로그인 한 후 **Active Directory** (1) 섹션으로 이동 하 고 응용 프로그램을 등록할 디렉터리 (2)를 선택 합니다.

   [![](register-images/01.-active-directory-in-azure-portal-sml.jpg "섹션을 열고 응용 프로그램을 등록할 디렉터리를 선택 합니다.")](register-images/01.-active-directory-in-azure-portal.jpg#lightbox)

3. **추가** 를 클릭 하 여 새 응용 프로그램을 만든 다음, **내 조직에서 개발 중인 응용 프로그램 추가** 를 선택 합니다.

   [![](register-images/02.-add-new-application-sml.jpg "내 조직에서 개발 중인 응용 프로그램 추가")](register-images/02.-add-new-application.jpg#lightbox)

4. 다음 화면에서 앱에 이름을 지정 합니다 (예: XAM-DEMO).
   응용 프로그램 유형으로 **네이티브 클라이언트 응용 프로그램** 을 선택 했는지 확인 합니다.

   ![](register-images/03.-app-name.jpg "응용 프로그램 유형으로 네이티브 클라이언트 응용 프로그램을 선택 했는지 확인 합니다.")

5. 최종 화면에서 인증이 완료 되 면이 URI로 반환 되는 응용 프로그램에 고유한 **리디렉션 URI* 를 제공 합니다.

   ![](register-images/04.-app-redirect.jpg "최종 화면에서 인증이 완료 되 면이 URI로 반환 되는 응용 프로그램에 고유한 리디렉션 URI를 제공 합니다.")

6. 앱이 만들어지면 **구성** 탭으로 이동 합니다. 나중에 응용 프로그램에서 사용할 **클라이언트 ID** 를 적어 보세요. 또한이 화면에서 Active Directory에 대 한 모바일 응용 프로그램 액세스 권한을 부여 하거나, 인증이 완료 되 면 모바일 응용 프로그램에서 사용할 수 있는 Web API 또는 Office365 같은 다른 응용 프로그램을 추가할 수 있습니다.

   ![](register-images/05.-configure.jpg "또한이 화면에서 Active Directory에 대 한 모바일 응용 프로그램 액세스 권한을 부여 하거나 Web API 또는 Office365 같은 다른 응용 프로그램을 추가할 수 있습니다.")



## <a name="related-links"></a>관련 링크

- [Microsoft NativeClient 샘플](https://github.com/AzureADSamples/NativeClient-MultiTarget-DotNet)
