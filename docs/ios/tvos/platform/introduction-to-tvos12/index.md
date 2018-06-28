---
title: 12 tvOS 소개
description: 이 문서에서는의 새로운 기능과 업데이트 tvOS 12는 Xamarin 미리 보기 릴리스에 대 한 기능 개요에 대 한 높은 수준의 현재 C#에서는 바인딩을 제공 합니다.
ms.prod: xamarin
ms.assetid: 037F7FFF-2155-4017-B99A-839CE7EC5C9C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/25/2018
ms.openlocfilehash: 03841306ba54e511dbf2f2b86a7c17e9f4669bcd
ms.sourcegitcommit: 3f2737f8abf9b855edf060474aa222e973abda3f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37067319"
---
# <a name="introduction-to-tvos-12"></a>12 tvOS 소개

![미리 보기](~/media/shared/preview.png)

> [!WARNING]
> Xamarin의 tvOS 12 지원 버그를 포함할 수 있음을 의미 하는 기능 완료 없으면, 미리 보기에서 현재 이며 변경 될 수 있습니다. 실험용만 사용 합니다.

> [!NOTE]
> - 검토는 [시작](~/ios/platform/introduction-to-ios12/get-started.md) iOS 12 및 Xamarin 사용한 tvOS 12 앱을 구축 하는 방법에 대 한 지침은 가이드입니다.
> - 자세한 내용은 참조는 [릴리스 정보](https://releases.xamarin.com/preview-release-xcode-10-beta/) 미리 보기 릴리스에 Xamarin에 대 한 합니다.

이 문서는 Xamarin 미리 보기 릴리스 현재 C# 바인딩을 제공 하는 12 기능도 새로운 기능과 업데이트 tvOS에 대 한 고급 개요를 제공 합니다.

## <a name="password-autofill"></a>암호 자동 채우기

12 tvOS 사용자 단일 탭이 있는 tvOS 앱에 로그인 하 여 iOS 장치를 사용할 수 있습니다. 이 기능은의 결합을 통해 사용 `UITextContentType` iOS 앱 및 tvOS 앱 및 포커스를 받을 사용자 후 항목을 선택 하 기본 포커스 환경 간의 관계를 설정 하려면 연결 된 도메인 사용자 이름 및 암호를 지정 하는 사용 필드 사용자 이름 및 암호를 제공합니다.

## <a name="focus-engine-enhancements"></a>포커스 엔진 향상 된 기능

12 tvOS 어떻게 렌더링 되기, 포커스 엔진과 상호 작용 없이 모든 응용 프로그램을 허용 합니다. Siri 원격 사용자의 상호 작용을 통해 포커스가 엔진 사용 하 여 모든 앱 항목을 선택, 가능한 포커스 변경 사항에 대 한 힌트 및 자연스럽 게 포커스를 업데이트 합니다. UIKit의를 통해 사용자 지정 응용 프로그램에서이 사용 `IUIFocusItemContainer` 인터페이스는 `UIFocusMovementHint` 클래스는 `IUIFocusItemScrollableContainer` 인터페이스 및 기타 관련된 클래스 및 메서드.

## <a name="related-links"></a>관련 링크

- [tvOS 샘플](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS – Apple 개발자 (Apple)](https://developer.apple.com/tvos/)
- [TvOS 12 (Apple) (비디오)의 새로운 기능](https://developer.apple.com/videos/play/wwdc2018/208/)
- [TV (Apple)](https://www.apple.com/tv/)
- Xamarin 미리 보기 [릴리스 정보](https://releases.xamarin.com/preview-release-xcode-10-beta/)
