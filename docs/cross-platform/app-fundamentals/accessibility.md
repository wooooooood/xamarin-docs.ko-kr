---
title: Xamarin 앱의 내게 필요한 옵션
description: 이 문서에서는 액세스할 수 있는 앱 만들기에 대 한 다양 한 팁을 제공 합니다. 예를 들어 큰 글꼴, 고대비, 자체 설명적 인터페이스 등에 대 한 권장 사항에 포함 됩니다.
ms.prod: xamarin
ms.assetid: E587F0CF-7C1D-41F8-B5A8-DA3E738EDA81
author: asb3993
ms.author: amburns
ms.date: 03/22/2017
ms.openlocfilehash: 97cd3655ac47a017d9590e1890b93d74f10a9c34
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34780306"
---
# <a name="accessibility-in-xamarin-apps"></a>Xamarin 앱의 내게 필요한 옵션

_앱 상당히 폭넓은 대상에서 사용할 수 있는지 확인_

액세스 가능성 큰 형식, 고대비 등 확대/축소와 같은 운영 체제 표시 및 입력 지원 기능이 제대로 작동 하는 응용 프로그램 사용자 인터페이스를 디자인, 화면 읽기 (텍스트 음성 변환), visual 또는 haptic 피드백 큐의 개념을 참조 하 고 입력된 메서드를 대체 합니다.

IOS, Android 및 Windows 등의 데스크톱 및 모바일 플랫폼 제공 개발자가 같은 액세스할 수 있는 앱을 빌드하는 데 도움이 되는 Api에서 기본적으로 제공 [Google TalkBack](https://play.google.com/store/apps/details?id=com.google.android.marvin.talkback) 및 [Apple의 음성](http://www.apple.com/accessibility/ios/voiceover/)합니다.

## <a name="platform-specific-apis"></a>플랫폼 관련 Api

이 문서의 지침을 구현 하려면 각 플랫폼에서 제공 된 Api를 사용 합니다.

- [**Android 내게 필요한 옵션**](~/android/app-fundamentals/accessibility.md)
- [**iOS 내게 필요한 옵션**](~/ios/app-fundamentals/accessibility.md)
- [**OS X 내게 필요한 옵션**](~/mac/app-fundamentals/accessibility.md)
- [**Xamarin.Forms**](~/xamarin-forms/app-fundamentals/accessibility/index.md)

<a name="checklist" />

## <a name="accessibility-checklist"></a>내게 필요한 옵션 검사 목록

앱에 사용자에 맞게 너비가 가장 넓은 가능한 액세스할 수 있는지 확인 하려면 다음이 팁을 따르십시오. 체크 아웃 된 [Android 접근성 테스트 검사 목록](http://developer.android.com/training/accessibility/testing.html) 및 [Apple의 내게 필요한 옵션 페이지](http://www.apple.com/accessibility/) 추가 정보에 대 한 합니다.

### <a name="support-large-fonts-and-high-contrast"></a>고대비 및 큰 글꼴 지원

컨트롤 차원을 하드 코딩 하지 않도록 하 고 대신 큰 글꼴 크기에 맞게 크기를 조정할 수 있는 레이아웃을 선호 합니다.
읽을 수 있는 인지 확인 하 고대비 모드에서 색 구성표를 테스트 합니다.

### <a name="make-the-user-interface-self-describing"></a>사용자 인터페이스 자체 설명 만들기

설명 텍스트는 각 플랫폼에서 Api를 읽는 화면와 호환 되는 힌트와 프로그램 사용자 인터페이스의 모든 요소 태그를 지정 합니다.

### <a name="ensure-that-images-and-icons-have-an-alternate-text-description"></a>이미지 및 아이콘 대체 텍스트 설명 되었는지 확인

이미지와 응용 프로그램 사용자 인터페이스 (예: 단추 또는의 예제에 대 한 상태 표시기)의 일부인 아이콘 액세스 가능한 설명으로 태그가 지정 되어야 합니다.

### <a name="design-the-visual-tree-with-accessible-navigation-in-mind"></a>디자인에에서 액세스할 수 있는 탐색이 있는 시각적 트리

적절 한 레이아웃 컨트롤을 사용 하거나 Api 대체 입력된 메서드를 사용 하 여 컨트롤 간을 탐색할 수 있도록 뒤에 오는 터치 스크린을 사용 하 여 동일한 논리 흐름.

화면 판독기 (장식 이미지 또는 이미 액세스할 수 있는, 예를 들어 필드에 대 한 레이블을)에서 불필요 한 요소를 제외 합니다.

### <a name="dont-rely-on-audio-or-color-cues-alone"></a>오디오 또는 색 신호만 의존 하지 마십시오

진행 중, 완료 되 면 또는 다른 상태에 대 한 유일한 표시는 소리 또는 색 변경 하는 상황을 방지 합니다. 경우 소리 및 색을 강화한만) (으로 명확한 시각적 신호를 포함 하거나 특정 내게 필요한 옵션 표시기 추가 사용자 인터페이스를 디자인 합니다.

색을 선택할 때는 색맹와 사용자에 대 한 구분 하기가 어렵습니다 색상표 않도록 하십시오.

### <a name="captioning-for-video-text-for-audio"></a>오디오에 대 한 비디오, 텍스트에 대 한 캡션

콘텐츠 비디오 및 오디오 콘텐츠에 대 한 읽을 수 있는 스크립트에 대 한 캡션을 제공 합니다. 오디오 또는 비디오 콘텐츠의 속도 조정 하는 컨트롤을 제공 하 고 해당 볼륨을 확인에 유용 하 고 재생/일시 중지 단추는 쉽게 찾아서 사용할 수 있습니다.

### <a name="localize"></a>Localize

내게 필요한 옵션 설명 수 (및 해야) 지역화 응용 프로그램이 다국어를 지원 합니다.



## <a name="related-links"></a>관련 링크

- [Android 내게 필요한 옵션](~/android/app-fundamentals/accessibility.md)
- [iOS 내게 필요한 옵션](~/ios/app-fundamentals/accessibility.md)
- [OS X 내게 필요한 옵션](~/mac/app-fundamentals/accessibility.md)
- [Xamarin.Forms 내게 필요한 옵션](~/xamarin-forms/app-fundamentals/accessibility/index.md)
