---
title: IOS 12 소개
description: 이 문서는 Xamarin의 미리 보기 릴리스는 C# 바인딩을 제공 하는 일부 12 iOS api 자세히 설명 합니다.
ms.prod: xamarin
ms.assetid: 99EA7090-315D-493C-87D3-26AB73D9E1A9
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/08/2018
ms.openlocfilehash: 4e1249b7a9c1e9797cbc758c3bd1b83f87d47431
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615150"
---
# <a name="introduction-to-ios-12"></a>IOS 12 소개

![미리 보기](~/media/shared/preview.png)

> [!WARNING]
> Xamarin iOS 12 지원에서 현재 버그를 포함할 수 있음을 의미를 없는 완벽 한 기능이 미리 보기 이며 변경 될 수 있습니다. 실험 용도로 사용 합니다.

이 문서는 Xamarin의 미리 보기 릴리스는 C# 바인딩을 제공 하는 일부 12 iOS api 자세히 설명 합니다.

시작 하려면 Xamarin 사용한 iOS 12 앱을 빌드하는 확인해 보세요.

- [시작 가이드](get-started.md)
- Xamarin 미리 보기 [릴리스 블로그 게시물](https://releases.xamarin.com/preview-release-xcode-10-beta-5/)

## <a name="arkit-2"></a>ARKit 2

ARKit는 iOS를 사용 하 여 포함 된 보강 된 현실 프레임 워크입니다. ARKit 2는 확대 된 현실 장면에서 서로 상호 작용 하는 여러 사용자가 공간에서 개체를 유지 하 고 나중에로 돌아올 수 있도록 제공 2D 이미지 인식 및 추적 및 3D 개체 인식 합니다. 또한 iOS 12는 AR 빠른 보기를 usdz AR 앱 모델을 렌더링 하는 방법을 제공 합니다.

## <a name="siri-shortcuts"></a>Siri 바로 가기

Siri 바로 가기 Siri를 사용 하 여 해당 응용 프로그램을 보다 강력 하 게 통합 하는 개발자를 수 있습니다. Siri 바로 가기를 사용 하 여 사용자 콘텐츠를 열거나 해당 앱에서 작업을 시작 음성 명령을 사용할 수 있습니다. Siri가 특정 바로 가기 가능성이 사용할 및 알림을 통해 사용자에 게 제안 하는 경우에 대해 알아봅니다.

## <a name="core-ml-2"></a>코어 ML 2

2 코어 ML 모델 양자화 및 유연한 모델을 통해 응용 프로그램 크기를 줄입니다, 그리고 새 일괄 처리 예측 API 사용 하 여 응용 프로그램 성능이 향상 됩니다 및 machine learning의 고급 기능을 지원 하기 위해 사용자 지정 모델을 사용 합니다.

## <a name="notification-improvements"></a>알림 기능 향상

Ios 12에서 그룹화 된 알림 수 있도록 앱 또는 스레드 관련 그룹에 사용자 알림을 표시 합니다. 추가 알림 그룹에 대 한 정보를 제공 하 여 요약 텍스트를 사용할 수 있습니다.

알림 콘텐츠 확장 12 ios에서 사용자 지정 사용자 인터페이스 및 동적 작업에 대 한 허용합니다. 이러한 기능을 사용자 알림에 관련성이 더 풍부한 환경을 사용 합니다.

## <a name="natural-language-framework"></a>자연 언어 프레임 워크

자연 언어 프레임 워크에는 응용 프로그램을을 다양 한 유형의 언어 분석을 수행할 수 있습니다. 예를 들어, 요소를 확인 하 고 텍스트 블록을 나타내는 언어를 결정 하려면 사용할 수 있습니다.

## <a name="carplay"></a>CarPlay

12 iOS에서 타사 앱에에서 제공할 수 맵 및 설정에 설정 하 여 탐색 지침이 CarPlay 새로운 CarPlay 프레임 워크를 사용 하 여.

## <a name="automatic-strong-passwords"></a>자동 강력한 암호

iOS 12 제안 하 고 사용자 이름 및 계정 만들기 화면을 포함 하는 응용 프로그램에 대 한 강력한 암호를 저장 합니다. 제안 된 암호를 생성할 수 있습니다 또는 기본적으로 20 문자 형식에 따라 개발자가 지정한 암호 규칙을 기반으로 합니다. 이 기능을 사용 하면 연결 된 도메인의 사용 및 새 사용자 이름 및 새 암호 필드에 지정 된 콘텐츠 형식입니다.

## <a name="autofill-credential-provider-extensions"></a>자동 채우기 자격 증명 공급자 확장

IOS 12 사용 하 여 제 3 자 암호 관리자 응용 프로그램 로그인 필드에 사용자 이름 및 암호 값을 제공 하는 확장을 제공할 수 있습니다.

## <a name="healthkit-updates"></a>HealthKit 업데이트

iOS 11.3 도입 [건강 기록](https://www.apple.com/healthcare/health-records/), 다양 한 의료 기관에서 정보를 기록 하 고 iOS 장치에서 대시보드를 볼 사용자가 해당 상태를 다운로드할 수 있습니다. 타사 응용 프로그램에서이 데이터를 안전 하 게 액세스할 수 있도록 Api를 추가 하는 iOS 12입니다.

## <a name="imessage-app-presentation-contexts"></a>iMessage 앱 프레젠테이션 컨텍스트

IOS 12, iMessage 앱 일반 iMessage 앱으로 또는 사진 또는 비디오 효과의 컨텍스트에서 실행 되도록 앱을 허용 하는 프레젠테이션 컨텍스트를 지원 합니다.

## <a name="vision-framework"></a>비전 프레임 워크

비전 프레임 워크에 다양 한 방향에서 얼굴을 검색할 수 있는 향상 된 얼굴 탐지기를 포함 합니다. 또한 요청 수정 이제 용도 특정 비전 framework 알고리즘 수정 버전을 선택 합니다.

## <a name="related-links"></a>관련 링크

- [IOS (Apple) 12에 대 한 준비](https://developer.apple.com/ios/)
- [12 iOS (Apple) 미리 보기](https://www.apple.com/ios/ios-12-preview/)
- Xamarin 미리 보기 [릴리스 블로그 게시물](https://releases.xamarin.com/preview-release-xcode-10-beta-5/)
