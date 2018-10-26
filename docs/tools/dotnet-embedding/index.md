---
title: .NET 포함
description: '.NET 포함 기존.NET 코드 (C#, F # 및 다른) 다른 프로그래밍 언어로 작성 된 코드에서 사용할 수 있습니다.'
ms.prod: xamarin
ms.assetid: 617C38CA-B921-4A76-8DFC-B0A3DF90E48A
author: lobrien
ms.author: laobri
ms.date: 11/14/2017
ms.openlocfilehash: 23233ea8b06e0db580ba99edf2705e3dae5b931f
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50107146"
---
# <a name="net-embedding"></a>.NET 포함

![미리 보기](~/media/shared/preview.png)

.NET 포함 기존.NET 코드 (C#, F # 및 기타) 다양 한 다른 환경 및 다른 프로그래밍 언어에서 사용할 수 있습니다.

즉,이 기존 iOS 앱에서 사용 하려는.NET 라이브러리를 사용 하는 경우 이렇게 할 수 있습니다.   또는 네이티브 c + + 라이브러리와 연결 하려는 경우 이렇게 할 수 있습니다도 합니다.   또는 Java에서.NET 코드를 사용 합니다.

.NET 포함 기반으로 합니다 [Embeddinator 4000](https://github.com/mono/Embeddinator-4000) 오픈 소스 프로젝트입니다.

## <a name="environments-and-languages"></a>환경 및 언어

이 도구는 둘 다를 사용 하는 환경 뿐만 아니라는 해당 설명을 사용 하는 언어를 인식 합니다.   예를 들어 iOS 플랫폼에서는.NET 포함 정적으로 컴파일되지.NET 코드를 iOS에서 사용할 수 있는 네이티브 코드에 있으므로 (JIT) 컴파일, 수 없습니다.  다른 환경에 JIT 컴파일을 허용지 않습니다 및 이러한 환경에서 JIT 컴파일 하도록 선택 했습니다.

대상 언어의 관용구 코드와.NET 코드를 표시 합니다.이 있으므로 다양 한 언어 소비자를 지원 합니다.   현재 지원 되는 언어 목록입니다.

- [**Objective-c** ](objective-c/index.md) –.NET 자연 스러운 Objective C Api에 매핑
- [**Java** ](android/index.md) –.NET 자연 스러운 Java Api에 매핑
- [**C** ](get-started/c.md) – C Api와 같은 개체 지향.NET 매핑

더 많은 언어 나중에 제공 됩니다.

## <a name="getting-started"></a>시작

시작 하려면이 가이드는 현재 지원 되는 언어에 대 한 중 하나를 확인 합니다.

- [**Objective-c** ](get-started/objective-c/index.md) – macOS 및 iOS에 설명
- [**Java** ](get-started/java/index.md) – macOS 및 Android에 설명
- [**C** ](get-started/c.md) – 데스크톱 플랫폼에서 C 언어를 설명 합니다.

## <a name="related-links"></a>관련 링크

- [GitHub에서 Embeddinator 4000](https://github.com/mono/Embeddinator-4000)
