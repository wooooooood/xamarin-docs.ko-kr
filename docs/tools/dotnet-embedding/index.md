---
title: ".NET 포함"
description: "사용 (C#, F # 등과) 다른 프로그래밍 언어에서 사용할 수 있도록 기존.NET 코드를 포함 하는.NET"
ms.topic: article
ms.prod: xamarin
ms.assetid: 617C38CA-B921-4A76-8DFC-B0A3DF90E48A
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 29c34ede0b59620b8951109f8571a08a960182d1
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="net-embedding"></a>.NET 포함

![미리 보기](~/media/shared/preview.png)

포함 하는.NET 기존.NET 코드 (C#, F # 및 다른 사용자)를 다른 프로그래밍 언어 및 다른 다양 한 환경에서 사용할 수 있습니다.

즉,이 기존 iOS 앱에서 사용 하려는.NET 라이브러리를 사용 하도록 설정한 경우 할 수 있습니다.   또는 네이티브 c + + 라이브러리와 연결 하려는 경우 수행할 수도 있습니다는 합니다.   또는.NET 코드 Java에서 사용 합니다.

## <a name="environments-and-languages"></a>환경 및 언어

이 도구는 둘 다를 사용 하는 환경 뿐만 아니라을 사용 하는 언어를 인식 합니다.   예를 들어 iOS 플랫폼은 embeddinator iOS에서 사용할 수 있는 네이티브 코드를.NET 코드를 정적으로 컴파일할 됩니다 있도록 (JIT) 컴파일을 허용 하지 않습니다.  다른 환경에 JIT 컴파일 못하게 하 고 해당 enviroments에서 JIT 컴파일 하도록 선택 했습니다.

대상 언어의 관용구 코드로.NET 코드를 표시 하므로 다양 한 언어 소비자를 지원 합니다.   이것은 현재 지원 되는 언어 목록입니다.

- [**Objective C** ](objective-c/index.md) –.NET 관용구 Objective-c Api에 매핑.
- [**Java** ](android/index.md) –.NET 관용구 Java Api에 매핑.
- **C**: 하는 개체 지향 C Api와 같이.NET 매핑.

더 많은 언어를 나중에 옵니다.

## <a name="getting-started"></a>시작

시작 하려면 현재 지원 되는 언어에 대 한 가이드 중 하나를 확인 합니다.

- [**Objective C** ](get-started/objective-c/index.md) – macOS 및 iOS에 설명 합니다.
- [**Java** ](get-started/java/index.md) – macOS 및 Android에 설명 합니다.
- [**C** ](get-started/c.md) – 데스크톱 플랫폼에서 C 언어에 설명 합니다.


## <a name="related-links"></a>관련 링크

- [GitHub의 Embeddinator 4000](https://github.com/mono/Embeddinator-4000)
