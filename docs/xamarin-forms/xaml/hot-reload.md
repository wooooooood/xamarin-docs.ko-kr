---
title: Xamarin에 대 한 XAML 핫 다시 로드
description: 실행 중인 응용 프로그램에서 XAML 파일에 대 한 변경 내용을 즉시 다시 로드 하므로 모든 XAML 변경 후에 Xamarin.ios 프로젝트를 빌드할 필요가 없습니다.
ms.prod: xamarin
ms.assetid: E220F054-32EE-424C-A7E5-6156BE271519
ms.technology: xamarin-forms
author: maddyleger1
ms.author: maleger
ms.date: 08/13/2019
ms.openlocfilehash: 4f6a0b45d37252c141b2741dd0b37a980c958a51
ms.sourcegitcommit: 41a029c69925e3a9d2de883751ebfd649e8747cd
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/13/2019
ms.locfileid: "68984535"
---
# <a name="xaml-hot-reload-for-xamarinforms-preview"></a>Xamarin.ios에 대 한 XAML 핫 다시 로드 (미리 보기)

![미리 보기 기능](~/media/shared/preview.png)

XAML 핫 다시 로드는 기존 워크플로에 연결 하 여 생산성을 높이고 시간을 절약 합니다. XAML 핫 다시 로드를 사용 하지 않으면 XAML 변경 내용을 확인할 때마다 앱을 빌드하고 배포 해야 합니다. 핫 다시 로드를 사용 하 여 XAML 파일을 저장 하면 변경 내용이 실행 중인 앱에 라이브 반영 됩니다. 또한 탐색 상태와 데이터가 유지 되므로 앱에서 사용자의 자리를 잃지 않고도 UI를 신속 하 게 반복할 수 있습니다. 따라서 XAML 핫 다시 로드를 사용 하 여 UI 변경의 유효성을 검사 하기 위해 앱을 다시 작성 하 고 배포 하는 시간을 줄일 수 있습니다.

## <a name="system-requirements"></a>시스템 요구 사항

| IDE/프레임 워크 | 버전 필요 |
|------|------------------|
|Visual Studio 2019 | 16.3 이상
Mac용 Visual Studio 2019 | 8.3 이상
Xamarin.Forms | 4.1 이상

## <a name="use-xaml-hot-reload-for-xamarinforms"></a>Xamarin에 대해 XAML 핫 다시 로드 사용

XAML 핫 다시 로드를 사용 하려면 추가 설치 또는 설정이 필요 하지 않습니다. Visual Studio에 기본 제공 되며 IDE 설정에서 사용 하도록 설정할 수 있습니다. 사용 하도록 설정 되 면 에뮬레이터, 시뮬레이터 또는 물리적 장치에서 앱을 디버그 하 여 XAML 핫 다시 로드 사용을 시작할 수 있습니다.

Windows에서는 **도구** > **옵션** **xamarin 핫 다시 로드**에서 **xamarin 핫 다시 로드 사용** 확인란을 선택 하 여 XAML 핫 다시 로드를 사용 하도록 설정할 수 있습니다. >  > 

Mac에서 **Visual Studio** > **기본 설정** > **프로젝트** xamarin핫다시로드에서xamarin핫다시로드사용확인란을선택하여XAML핫다시로드를사용하도록 > 설정할 수 있습니다.

## <a name="resilient-reloading"></a>복원 력 다시 로드

XAML 핫 다시 로드에서 다시 로드할 수 없는 변경 작업을 수행 하면 IntelliSense를 사용 하 여 오류 메시지가 표시 됩니다. 이러한 변경 내용에는 잘못 된 편집이 포함 되어 있으며,이는 XAML을 잘못 삽입 하거나 컨트롤을 존재 하지 않는 이벤트 처리기에 연결 합니다. 잘못 된 편집 기능을 사용 하는 경우에도 앱을 다시 시작 하지 않고 계속 다시 로드할 수 있습니다. XAML 파일의 다른 위치에서 다른 변경을 수행 하 고 저장을 누릅니다. 강제 편집은 다시 로드 되지 않지만 다른 변경 내용은 계속 적용 됩니다.

## <a name="known-limitations"></a>알려진 제한 사항

- XAML 핫 다시 로드 세션 중에는 파일 또는 NuGet 패키지를 추가, 제거 또는 이름을 변경할 수 없습니다. 파일이 나 NuGet 패키지를 추가 하거나 제거 하는 경우 XAML 핫 다시 로드를 계속 사용 하도록 앱을 다시 빌드하고 다시 배포 합니다.
- 최상의 환경을 위해 링커를 **없음 링크** 로 설정 합니다. **링크 SDK only 설정만** 대부분의 시간 동안 작동 하지만 특정 한 경우에는 실패할 수 있습니다.
- 실제 iPhone에서 디버깅 하려면 인터프리터에서 XAML 핫 다시 로드를 사용 해야 합니다. XAML 핫 다시 로드를 사용 하려면 **--인터프리터** 를 iOS 빌드 설정의 **추가 mtouch 인수** 필드에 추가 합니다.
- 컨트롤을 해당 `x:Name` 값을 사용 하 여 다른 필드나 속성에 할당 하 여 만든 모든 참조는 다시 로드 되지 않습니다.
- **Appshell** 에서 셸 응용 프로그램의 시각적 계층 구조를 업데이트 하면 응용 프로그램의 상태를 유지 관리 하는 문제가 발생할 수 있습니다. 다시 로드를 계속 하려면 앱을 다시 빌드합니다.
- XAML 핫 다시 로드는 C# 이벤트 처리기, 사용자 지정 컨트롤, 페이지 코드 및 추가 클래스를 포함 하 여 코드를 다시 로드할 수 없습니다.

## <a name="migrate-from-the-private-preview"></a>비공개 미리 보기에서 마이그레이션

비공개 미리 보기의 일부인 경우에는 Visual Studio가 업데이트 될 때 XAML 핫 다시 로드 확장이 자동으로 업데이트 됩니다. Visual Studio를 업데이트 하지 않도록 선택한 경우 XAML 핫 재 로드의 현재 버전을 계속 사용할 수 있지만 비공개 미리 보기 확장 피드를 통해 추가 업데이트를 받지 못합니다.

## <a name="troubleshooting"></a>문제 해결

- XAML 핫 재 로드를 초기화 하지 못한 경우:
  - Xamarin.ios 버전을 업데이트 합니다.
  - 최신 버전의 IDE를 사용할 수 있는지 확인 합니다.
  - Android 또는 iOS 링커 설정을 프로젝트의 빌드 설정에서 **링크 안 함** 으로 설정 합니다.
- XAML 파일을 저장할 때 아무 작업도 수행 되지 않으면 IDE에서 핫 다시 로드를 사용 하도록 설정 해야 합니다.
- 실제 iPhone에서 디버깅 하 고 앱이 응답 하지 않는 경우 인터프리터를 사용 하도록 설정 되었는지 확인 합니다. 이 기능을 설정 하려면 **--인터프리터** 를 iOS 빌드 설정의 **추가 mtouch 인수** 필드에 추가 합니다.

버그를 보고 하려면 Windows의**사용자 의견** > 보내기**문제 보고** 메뉴와 Mac > 의**문제 보고** **도움말** > 메뉴에서 피드백 도구를 사용 합니다.
