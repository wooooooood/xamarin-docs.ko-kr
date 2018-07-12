---
title: TvOS 12 소개
description: 이 문서는 Xamarin의 미리 보기 릴리스에 대 한 12 tvOS에서 새롭고 업데이트 된 기능의 개요는 고급 C# 바인딩을 현재 제공을 제공 합니다.
ms.prod: xamarin
ms.assetid: 037F7FFF-2155-4017-B99A-839CE7EC5C9C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/25/2018
ms.openlocfilehash: 03841306ba54e511dbf2f2b86a7c17e9f4669bcd
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38847563"
---
# <a name="introduction-to-tvos-12"></a>TvOS 12 소개

![미리 보기](~/media/shared/preview.png)

> [!WARNING]
> Xamarin의 tvOS 12 지원은 현재 버그를 포함할 수 있음을 의미를 없는 완벽 한 기능이 미리 보기로 제공 되며 변경 될 수 있습니다. 실험 용도로 사용 합니다.

> [!NOTE]
> - 검토 합니다 [시작](~/ios/platform/introduction-to-ios12/get-started.md) 12 iOS 및 Xamarin 사용 하 여 12 tvOS 앱 구축을 시작 하는 방법에 대 한 지침은 가이드입니다.
> - 자세한 내용은 합니다 [릴리스](https://releases.xamarin.com/preview-release-xcode-10-beta/) Xamarin에 대 한 미리 보기 릴리스 합니다.

이 문서는 Xamarin의 preview 릴리스 현재 C# 바인딩을 제공 하는 12 기능 새롭고 업데이트 된 tvOS의 대략적인 개요를 제공 합니다.

## <a name="password-autofill"></a>암호 자동 채우기

TvOS 12 사용 하 여 사용자가 한 번의 탭을 사용 하 여 tvOS 앱에 로그인 할 iOS 장치를 사용할 수 있습니다. 결합을 통해 활성화 됩니다 `UITextContentType` iOS 앱 및 tvOS 앱을 기본 포커스 포커스를 받을 사용자 후 항목을 선택 하는 환경 간의 관계를 설정 하려면 연결 된 도메인 사용자 이름 및 암호를 지정 하는 사용량 필드 사용자 이름 및 암호를 제공합니다.

## <a name="focus-engine-enhancements"></a>포커스 엔진 향상 된 기능

tvOS 12 어떻게 렌더링 되며, 포커스 엔진과 상호 작용에 관계 없이 모든 앱을 허용 합니다. Siri 원격을 사용 하 여 사용자의 상호 작용을 통해 항목을 선택 가능한 포커스 변경 힌트, 포커스를 자연스럽 게 업데이트를 포커스 엔진 모든 앱을 사용 하 여 사용할 수 있습니다. UIKit의를 통해 사용자 지정 응용 프로그램에서 사용 됩니다 `IUIFocusItemContainer` 인터페이스를 `UIFocusMovementHint` 클래스는 `IUIFocusItemScrollableContainer` 인터페이스, 기타 관련된 클래스 및 메서드.

## <a name="related-links"></a>관련 링크

- [tvOS 샘플](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS – Apple Developer (Apple)](https://developer.apple.com/tvos/)
- [TvOS 12 (Apple) (비디오)의 새로운 기능](https://developer.apple.com/videos/play/wwdc2018/208/)
- [TV (Apple)](https://www.apple.com/tv/)
- Xamarin 미리 보기 [릴리스 정보](https://releases.xamarin.com/preview-release-xcode-10-beta/)
