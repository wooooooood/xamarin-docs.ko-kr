---
title: Mac 용 Visual Studio의 서비스 연결
description: Mac 용 Visual Studio 내에서 모바일 앱에 Azure 데이터 저장소, 인증 및 푸시 알림 추가
ms.prod: xamarin
ms.assetid: ADDFB3A5-DB6A-417C-8ACE-33D107FB75C2
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: b9aaa197ccf01c59d6e4bbb0476d10295a108f89
ms.sourcegitcommit: 66807f8927d472fbfd0ff8bc77cea9b37e7b9a4f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/05/2018
---
# <a name="connected-services-walkthrough"></a>연결 된 서비스 연습

연결 된 서비스 워크플로 통해 Azure 포털 워크플로 Visual Studio로, Mac 용 하므로 서비스를 추가 하려면 프로젝트를 두고 필요가 없습니다.

이 연습에서는 클라우드 데이터 저장소, 인증 및 플랫폼 Xamarin.Forms 이식 가능한 클래스 라이브러리 (PCL) 응용 프로그램에 푸시 알림을 표시 하는 Azure의 백 엔드 서비스를 추가 하는 방법을 보여 줍니다.


1.  두 번 클릭 하 여 시작는 **연결 된 서비스** 나타납니다 솔루션에 있는 노드는 **서비스 갤러리**합니다.
  이 응용 프로그램 종류에 대 한 모든 사용 가능한 서비스 목록입니다. 서비스를 선택 (예: **Azure 앱 서비스와 모바일 백 엔드**) 클릭 하 여 합니다.

  [![](connected-services-images/image001-sml.png "Mac 용 Visual Studio에서 서비스 노드를 연결")](connected-services-images/image001.png#lightbox)

2. 서비스 세부 정보 페이지에 대 한 설명이 서비스 및 종속성을 설치할 수 있습니다.
  클릭는 **추가** 응용 프로그램에 종속성을 추가 하는 단추:

  [![](connected-services-images/image002-sml.png "Azure 모바일 백 엔드")](connected-services-images/image002.png#lightbox)

3. 종속성에서 실행 되도록 하는 PCL와 플랫폼별 프로젝트에 추가 해야 합니다.
  참조 하는 것 (직접 또는 간접적으로) 모든 프로젝트에 서비스를 추가 하려면 확인란을 선택 합니다.

  [![](connected-services-images/image003-sml.png "서비스를 참조 해야 하는 모든 프로젝트를 확인 합니다.")](connected-services-images/image003.png#lightbox)

4. 선택 **Accept** 에 **라이선스 승인** NuGet 패키지에 대 한 대화 상자.
  두 대화 상자에 동의할 수,는 MobileClient 및 종속성에 대 한 관리를 위한 SQLiteStore, 오프 라인 데이터 동기화에 필요한 수 있습니다.

  [![](connected-services-images/image004-sml.png "사용권 계약에 동의")](connected-services-images/image004.png#lightbox)

  ![](connected-services-images/image005.png "라이선스 승인 창")

5. 종속성을 추가 했으면 되 면 Azure와 통신 하는 데 사용할 계정으로 로그인 해야 합니다.
  이미 Microsoft ID로 로그인 하 고, Mac 용 Visual Studio는 Azure 구독 및 관련 된 모든 응용 프로그램 서비스를 인출 하려고 합니다. 구독이 없는 경우 Azure 포털에서 구독 계획을 구입 하거나 무료 평가판에 등록 하 여 하나를 추가할 수 있습니다.

6. 목록에서 응용 프로그램 서비스를 선택 합니다. 이 대 한 템플릿 코드를 채울는 `MobileServiceClient` Azure에서 응용 프로그램 서비스의 해당 URL 가진 개체:

  [![](connected-services-images/image006-sml.png "목록에서 응용 프로그램 서비스를 선택 합니다.")](connected-services-images/image006.png#lightbox)

  나열 된 서비스가 있는 경우 클릭는 **새로** 단추 (9 단계 참조).

7. 에 대 한 템플릿 코드를 복사 하는 `MobileServiceClient` PCL에 있습니다. 파일 위치 응용 프로그램에 하나의 인스턴스가으로 중요 하지 않은 합니다.
  권장된 방법은 만드는 것는 `AzureService` Azure의 모든 상호 작용을 처리 하 고 사용 하는 클래스는 `MobileServiceClient`:

  ![](connected-services-images/image007.png "응용 프로그램에 구성 코드를 복사 합니다.")

8. 에 대 한 설명서에 따라 **다음 단계** 데이터, 오프 라인 동기화, 인증을 추가 하 여 응용 프로그램에 대 한 푸시 알림:

  [![](connected-services-images/image008-sml.png "다음 단계 지침을 검토")](connected-services-images/image008.png#lightbox)

10. 모든 기존 앱 서비스를 설정 하지 않은 경우 Mac.에 대 한 Visual Studio 내에서 새 서비스를 만들 수 있습니다.
  클릭는 **새로** 열려는 서비스 목록의 왼쪽 하단에서에서 단추는 **새 앱 서비스** 대화 상자:

  [![](connected-services-images/image009-sml.png "Mac 용 Visual Studio에서 새 응용 프로그램 서비스 만들기")](connected-services-images/image009.png#lightbox)

새 서비스는 다음 매개 변수가 필요합니다.

-   **응용 프로그램 서비스 이름** – 계획에 대 한 고유 이름/i d
-   **구독** – 서비스에 대 한 비용을 지불 하는 데 원하는 구독
-   **리소스 그룹** –는 방식으로 또는 프로젝트에 대 한 모든 Azure 리소스를 구성 합니다. 기존 항목 사용 하거나 새로 만들 옵션입니다. 첫 번째 Azure 서비스의 경우 새로 만듭니다.
-   **서비스 계획** – 위치 및을 사용 하는 모든 리소스의 비용을 결정 합니다. 기존 항목 사용 하거나 새로 만들 옵션입니다. 첫 번째 Azure 서비스의 경우 기본 하나를 사용 하거나 무료 계층 (F1)에서 새 인증서를 만듭니다.

방문는 [Azure 앱 서비스 설명서](https://docs.microsoft.com/azure/app-service/) 에 대 한 자세한 내용은


## <a name="related-links"></a>관련 링크

- [Azure App Service](https://docs.microsoft.com/en-us/azure/app-service/)
- [릴리스 정보](https://developer.xamarin.com/releases/studio/xamarin.studio_6.2/xamarin.studio_6.2/#Connected_Services)
