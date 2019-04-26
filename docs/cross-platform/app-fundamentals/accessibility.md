---
title: Xamarin 앱의 내게 필요한 옵션
description: 이 문서는 액세스 가능한 앱 만들기에 대 한 다양 한 팁을 제공합니다. 예를 들어, 큰 글꼴, 고대비, 자체 설명적 인터페이스 등에 대 한 권장 사항을 포함합니다.
ms.prod: xamarin
ms.assetid: E587F0CF-7C1D-41F8-B5A8-DA3E738EDA81
author: asb3993
ms.author: amburns
ms.date: 03/22/2017
ms.openlocfilehash: 0ec264e0f3d381fdac46c79dd479da2bc768954f
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61282191"
---
# <a name="accessibility-in-xamarin-apps"></a>Xamarin 앱의 내게 필요한 옵션

_앱 광범위 한 대상에서 사용할 수 있는지 확인_

내게 필요한 옵션 큰 형식, 고대비 등 확대/축소와 같은 운영 체제 표시 및 입력 지원 기능을 잘 작동 하는 앱 사용자 인터페이스 디자인, 화면 읽기 (텍스트 음성 변환), visual 또는 햅 틱 피드백 큐 개념 참조 및 대체 입력된 메서드의 입력 합니다.

IOS, Android 및 Windows 등의 데스크톱 및 모바일 플랫폼에서는 개발자가 같은 액세스 가능한 앱을 빌드하는 데 도움이 되는 Api에서 작성 [Google TalkBack](https://play.google.com/store/apps/details?id=com.google.android.marvin.talkback) 하 고 [Apple의 VoiceOver](http://www.apple.com/accessibility/ios/voiceover/)합니다.

## <a name="platform-specific-apis"></a>플랫폼 관련 Api

이 문서의 지침을 구현 하려면 각 플랫폼에서 제공 하는 Api를 사용 합니다.

- [**Android 내게 필요한 옵션**](~/android/app-fundamentals/accessibility.md)
- [**iOS 내게 필요한 옵션**](~/ios/app-fundamentals/accessibility.md)
- [**OS X 내게 필요한 옵션**](~/mac/app-fundamentals/accessibility.md)
- [**Xamarin.Forms**](~/xamarin-forms/app-fundamentals/accessibility/index.md)

<a name="checklist" />

## <a name="accessibility-checklist"></a>액세스 가능성 검사 목록

다음과 같은이 팁을 앱에 가능한 결과 대상에 게 액세스할 수 있는지 확인 합니다. 체크 아웃 합니다 [Android 접근성 테스트 검사 목록](https://developer.android.com/training/accessibility/testing.html) 하 고 [Apple의 내게 필요한 옵션 페이지](http://www.apple.com/accessibility/) 추가 정보에 대 한 합니다.

### <a name="support-large-fonts-and-high-contrast"></a>큰 글꼴 및 고대비를 지원 합니다.

하드 코딩 컨트롤 차원을 사용 하지 마십시오 및 대신 큰 글꼴 크기에 맞게 크기를 조정할 수 있는 레이아웃을 선호 합니다.
읽기 가능한 지 확인 하기 위해 고대비 모드로 색 구성표를 테스트 합니다.

### <a name="make-the-user-interface-self-describing"></a>사용자 인터페이스 자체 설명 확인

설명이 포함 된 텍스트 및 각 플랫폼에서 Api를 읽는 화면와 호환 되는 힌트를 사용 하 여 프로그램 사용자 인터페이스의 모든 요소 태그를 지정 합니다.

### <a name="ensure-that-images-and-icons-have-an-alternate-text-description"></a>이미지 및 아이콘에 대 한 대체 텍스트 설명이 있는지 확인

이미지 및 아이콘 (예: 단추 또는 예를 들어 상태 표시기) 응용 프로그램 사용자 인터페이스의 일부인 태그를 지정 해야 액세스할 수 있는 설명과 함께 합니다.

### <a name="design-the-visual-tree-with-accessible-navigation-in-mind"></a>디자인에에서 액세스할 수 있는 탐색을 사용 하 여 시각적 트리

적절 한 레이아웃 컨트롤을 사용 하거나 Api 대체 입력된 메서드를 사용 하 여 컨트롤 간에 이동 다음과 같이 나타나도록 터치 스크린을 사용 하 여 동일한 논리 흐름입니다.

(장식 이미지 또는 이미 액세스할 수 있는, 예를 들어 필드에 대 한 레이블을) 화면 판독기에서 불필요 한 요소를 제외 합니다.

### <a name="dont-rely-on-audio-or-color-cues-alone"></a>단독으로 오디오 또는 색 신호에 의존 하지 마세요

여기서 진행 중, 완료 또는 일부 다른 상태에 대 한 유일한 표시는 사운드 또는 색 변경 하는 상황을 방지 합니다. 중 하나 (및 사용 하 여 소리 보충만 색)를 지우기 시각 신호를 포함 하거나 특정 접근성 표시기를 추가 하는 사용자 인터페이스를 디자인 합니다.

색을 선택할 때 색상표 색맹을 사용 하 여 사용자에 대 한 구분 하기 어려울 것을 방지 하려고 합니다.

### <a name="captioning-for-video-text-for-audio"></a>오디오에 대 한 비디오, 텍스트에 대 한 선택 자막

비디오 콘텐츠 및 오디오 콘텐츠에 대 한 읽을 수 있는 스크립트에 대 한 캡션을 제공 합니다. 또한 오디오 또는 비디오 콘텐츠의 속도 조정 하는 컨트롤을 제공 하 고 해당 볼륨을 확인 하는 데 도움이 및 재생/일시 중지 단추는 쉽게 찾아서 사용 합니다.

### <a name="localize"></a>Localize

내게 필요한 옵션 설명 있습니다을 지역화할 응용 프로그램은 여러 언어를 지원 합니다.



## <a name="related-links"></a>관련 링크

- [Android 내게 필요한 옵션](~/android/app-fundamentals/accessibility.md)
- [iOS 내게 필요한 옵션](~/ios/app-fundamentals/accessibility.md)
- [OS X 내게 필요한 옵션](~/mac/app-fundamentals/accessibility.md)
- [Xamarin.Forms 내게 필요한 옵션](~/xamarin-forms/app-fundamentals/accessibility/index.md)
