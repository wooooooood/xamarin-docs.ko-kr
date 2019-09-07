---
title: Xamarin 앱의 접근성
description: 이 문서에서는 액세스 가능한 앱을 만들기 위한 다양 한 팁을 제공 합니다. 예를 들어 큰 글꼴, 고대비, 자체 기술 인터페이스 등에 대 한 권장 사항을 포함 합니다.
ms.prod: xamarin
ms.assetid: E587F0CF-7C1D-41F8-B5A8-DA3E738EDA81
author: conceptdev
ms.author: crdun
ms.date: 03/22/2017
ms.openlocfilehash: 55d531036336cdd6c3ac7efa1c5ba21b09a7be9e
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70758130"
---
# <a name="accessibility-in-xamarin-apps"></a>Xamarin 앱의 접근성

_최대한 광범위 한 대상 그룹이 앱을 사용할 수 있는지 확인_

접근성은 큰 형식, 고대비, 확대, 화면 읽기 (텍스트 음성 변환), 시각적 또는 햅 피드백 큐와 같은 잘 운영 체제 표시 및 입력 지원 기능을 수행 하는 응용 프로그램 사용자 인터페이스를 디자인 하는 개념을 나타냅니다. 대체 입력 방법

IOS, Android, Windows 등의 데스크톱 및 모바일 플랫폼은 개발자가 [Google TalkBack](https://play.google.com/store/apps/details?id=com.google.android.marvin.talkback) 및 [Apple의 VoiceOver](http://www.apple.com/accessibility/ios/voiceover/)와 같은 액세스 가능한 앱을 빌드하는 데 도움이 되는 기본 제공 api를 제공 합니다.

## <a name="platform-specific-apis"></a>플랫폼별 Api

이 문서의 지침을 구현 하려면 각 플랫폼에서 제공 하는 Api를 사용 합니다.

- [**Android 접근성**](~/android/app-fundamentals/accessibility.md)
- [**iOS 접근성**](~/ios/app-fundamentals/accessibility.md)
- [**OS X 접근성**](~/mac/app-fundamentals/accessibility.md)
- [**Xamarin.Forms**](~/xamarin-forms/app-fundamentals/accessibility/index.md)

<a name="checklist" />

## <a name="accessibility-checklist"></a>내게 필요한 옵션 검사 목록

이러한 팁을 따라 가장 광범위 한 사용자가 앱에 액세스할 수 있도록 합니다. 자세한 내용은 [Android 접근성 테스트 검사 목록](https://developer.android.com/training/accessibility/testing.html) 및 [Apple의 내게 필요한 옵션 페이지](http://www.apple.com/accessibility/) 를 참조 하세요.

### <a name="support-large-fonts-and-high-contrast"></a>큰 글꼴 및 고대비 지원

컨트롤 크기를 하드 코딩 하지 않고 크기를 조정 하 여 더 큰 글꼴 크기를 수용 하는 레이아웃을 선호 합니다.
고대비 모드에서 색 구성표를 테스트 하 여 읽을 수 있도록 합니다.

### <a name="make-the-user-interface-self-describing"></a>사용자 인터페이스를 자체 설명으로 만들기

각 플랫폼에서 Api를 읽는 화면과 호환 되는 설명 텍스트 및 힌트를 사용 하 여 사용자 인터페이스의 모든 요소에 태그를 설정 합니다.

### <a name="ensure-that-images-and-icons-have-an-alternate-text-description"></a>이미지와 아이콘에 대체 텍스트 설명이 있는지 확인

응용 프로그램 사용자 인터페이스에 포함 된 이미지와 아이콘 (예: 단추 또는 상태 표시기)은 액세스 가능한 설명으로 태그를 지정 해야 합니다.

### <a name="design-the-visual-tree-with-accessible-navigation-in-mind"></a>액세스 가능한 탐색을 염두에 두면 시각적 트리 디자인

대체 입력 메서드를 사용 하는 컨트롤 간의 탐색이 터치 스크린을 사용 하는 것과 동일한 논리 흐름을 따르는 적절 한 레이아웃 컨트롤 또는 Api를 사용 합니다.

화면 판독기에서 불필요 한 요소를 제외 합니다 (예: 이미 액세스할 수 있는 필드의 경우 장식용 이미지 또는 레이블).

### <a name="dont-rely-on-audio-or-color-cues-alone"></a>오디오 또는 색 큐만 사용 하지 않음

진행률, 완료 또는 기타 상태를 유일한 표시로 소리나 색 변경 하는 상황을 방지 합니다. 명확한 시각적 표시를 포함 하도록 사용자 인터페이스를 디자인 하거나 (보충에만 해당 하는 소리 및 색 사용) 특정 접근성 표시기를 추가 합니다.

색을 선택 하는 경우 색이 시각 인 사용자를 구분 하기 어려운 팔레트를 사용 하지 마십시오.

### <a name="captioning-for-video-text-for-audio"></a>비디오에 대 한 캡션, 오디오 텍스트

비디오 콘텐츠의 캡션과 오디오 콘텐츠에 대 한 읽을 수 있는 스크립트를 제공 합니다. 또한 오디오 또는 비디오 콘텐츠의 속도를 조정 하는 컨트롤을 제공 하 고 볼륨 및 재생/일시 중지 단추를 쉽게 찾아서 사용할 수 있도록 하는 것이 유용 합니다.

### <a name="localize"></a>Localize

응용 프로그램에서 여러 언어를 지 원하는 경우 접근성 설명 (및)을 지역화할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [Android 접근성](~/android/app-fundamentals/accessibility.md)
- [iOS 접근성](~/ios/app-fundamentals/accessibility.md)
- [OS X 접근성](~/mac/app-fundamentals/accessibility.md)
- [Xamarin 양식 접근성](~/xamarin-forms/app-fundamentals/accessibility/index.md)
