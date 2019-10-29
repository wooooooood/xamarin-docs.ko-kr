---
title: .NET 포함
description: .Net 포함을 사용 하면 다른 프로그래밍 언어로C#작성 F#된 코드에서 기존 .net 코드 (, 및 기타)를 사용할 수 있습니다.
ms.prod: xamarin
ms.assetid: 617C38CA-B921-4A76-8DFC-B0A3DF90E48A
author: davidortinau
ms.author: daortin
ms.date: 11/14/2017
ms.openlocfilehash: 5a0e7eeaee9b3189de63d0b82a3822cc68023505
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73006940"
---
# <a name="net-embedding"></a>.NET 포함

![미리 보기](~/media/shared/preview.png)

.Net 포함을 사용 하면 기존 .net 코드C#( F#, 및 기타)를 다른 프로그래밍 언어 및 여러 다른 환경에서 사용할 수 있습니다.

즉, 기존 iOS 앱에서 사용 하려는 .NET 라이브러리가 있는 경우이 작업을 수행할 수 있습니다.   또는 네이티브 C++ 라이브러리를 사용 하 여 연결 하려는 경우에도이 작업을 수행할 수 있습니다.   또는 Java의 .NET 코드를 사용 합니다.

.NET 포함은 [Embeddinator-4000](https://github.com/mono/Embeddinator-4000) 오픈 소스 프로젝트를 기반으로 합니다.

## <a name="environments-and-languages"></a>환경 및 언어

도구는 사용 하는 환경 뿐만 아니라이 도구를 사용 하는 언어도 인식 합니다.   예를 들어 iOS 플랫폼은 JIT (just-in-time) 컴파일을 허용 하지 않으므로 .NET 포함은 .NET 코드를 iOS에서 사용할 수 있는 네이티브 코드로 정적으로 컴파일합니다.  다른 환경에서는 JIT 컴파일을 허용 하 고, 이러한 환경에서는 JIT 컴파일을 옵트인 합니다.

다양 한 언어 소비자를 지원 하므로 .NET 코드를 대상 언어로 자연 스러운 코드로 표시 합니다.   현재 지원 되는 언어 목록입니다.

- [**목적-c**](objective-c/index.md) – 자연 스러운 목적과 api에 대 한 .net 매핑
- [**Java**](android/index.md) – 자연 스러운 Java api로 .net 매핑
- [**C**](get-started/c.md) – 개체 지향 c api와 같은 .net 매핑

추가 언어는 나중에 제공 될 예정입니다.

## <a name="getting-started"></a>시작

시작 하려면 현재 지원 되는 각 언어에 대 한 가이드 중 하나를 확인 하세요.

- [**목적-C**](get-started/objective-c/index.md) – macos 및 iOS를 다룹니다.
- [**Java**](get-started/java/index.md) – macos 및 Android 커버
- [**C**](get-started/c.md) – 데스크톱 플랫폼의 c 언어에 대해 설명 합니다.

## <a name="related-links"></a>관련 링크

- [GitHub의 Embeddinator-4000](https://github.com/mono/Embeddinator-4000)
