---
title: Objective-C 개발자용 Xamarin
description: 이 문서에서는 Objective-C 개발자용 Xamarin.iOS의 설명을 제공합니다. Objective-C에서 C#으로 전환하는 방법, C#에서 사용을 위해 Objective-C 라이브러리를 바인딩하는 방법 및 플랫폼 간 모바일 응용 프로그램을 빌드하는 방법을 설명하는 설명서에 연결합니다.
ms.prod: xamarin
ms.assetid: 9F3C86A3-403E-4025-99CA-99FCA86DC828
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/05/2017
ms.openlocfilehash: 415a888a61b4c4ca9164166aadb1983df2d828dd
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50107341"
---
# <a name="xamarin-for-objective-c-developers"></a>Objective-C 개발자용 Xamarin

Xamarin은 iOS를 대상으로 하는 개발자가 비 사용자 인터페이스 코드를 플랫폼 제약 없는 C#으로 이동할 수 있는 경로를 제공하며, 여기에는 Xamarin.Android 통한 Android 및 Windows의 다양한 버전을 포함하여 C#을 사용할 수 있는 모든 곳이 포함됩니다. 그러나 Xamarin에 C#을 사용한다고 해서 기존 기술과 Objective-C 코드를 활용할 수 없는 것은 아닙니다. 사실, Xamarin은 UIKit, Core Animation, Core Foundation, Core Graphics를 포함하여 개발자들이 잘 알고 있고 애용하는 모든 네이티브 iOS 및 OS X 플랫폼을 노출하므로 Objective-C를 알고 있으면 Xamarin.iOS를 개발하는 데 많은 도움이 됩니다. 뿐만 아니라 LINQ 및 제네릭 같은 기능을 비롯한 C# 언어와 풍부한 .NET 기본 클래스 라이브러리의 강력한 성능을 기본 응용 프로그램에 사용할 수 있습니다.

또한 Xamarin에서는 바인딩으로 알려진 기술을 통해 기존 Objective-C 자산을 활용할 수 있습니다. 다음 다이어그램에 보이는 것처럼, 간단하게 Objective-C에서 정적 라이브러리를 만들고 바인딩을 통해 C#에 노출하면 됩니다.

 [![](images/01-bindings.png "바인딩을 통해 C#에 노출된 Objective-C의 정적 라이브러리")](images/01-bindings.png#lightbox)

이것은 비 UI 코드로 제한할 필요가 없습니다. Objective-C에서 개발된 사용자 인터페이스 코드도 바인딩을 통해 노출할 수 있습니다.

## <a name="transitioning-from-objective-c"></a>Objective-C에서 전환

당사의 설명서 사이트에는 Xamarin으로 쉽게 전환할 수 있도록 도와주는 다양한 정보가 있으며, C# 코드를 여러분이 이미 알고 있는 내용과 통합하는 방법을 보여줍니다. 시작할 때 다음과 같은 사항이 중요합니다.

-   [Objective-C 개발자용 C# 입문서](primer.md) - 이 문서는 Xamarin 및 C# 언어로 이동하려는 Objective-C 개발자를 위한 간단한 입문서입니다. 
-   [연습: Objective-C 라이브러리 바인딩](~/ios/platform/binding-objective-c/walkthrough.md) - Xamarin.iOS 응용 프로그램에서 기존 Objective-C 코드를 다시 사용하는 방법을 단계별로 연습합니다. 


## <a name="binding-objective-c"></a>Objective-C 바인딩

C#과 Objective-C의 차이점을 이해하고 위의 바인딩 연습을 모두 마쳤다면 Xamarin 플랫폼으로 전환할 준비가 완료된 것입니다. 후속 작업으로, [Objective-C 바인딩](~/ios/platform/binding-objective-c/index.md) 섹션에서 포괄적인 바인딩 참조를 포함하여 Xamarin.iOS 바인딩 기술에 대한 좀 더 구체적인 정보를 확인하세요.

## <a name="cross-platform-development"></a>크로스 플랫폼 개발

마지막으로, Xamarin.iOS로 전환한 후에는 현재 개발된 참조 응용 프로그램 사례 연구를 포함하여 크로스 플랫폼 지침을 확인하고 [크로스 플랫폼 응용 프로그램 빌드 섹션](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)에 포함된 재사용 가능한 크로스 플랫폼 코드 만들기 모범 사례를 확인합니다.
