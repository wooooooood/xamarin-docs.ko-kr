---
title: Java를 Xamarin.Android용 C#로 이식
description: Xamarin.Android 애플리케이션에서 Java를 사용하는 세 번째 옵션은 Java 소스 코드를 C#로 이식하는 것입니다.
ms.prod: xamarin
ms.assetid: 39E528BD-010F-47FC-BE48-8E7848E30454
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 04/05/2016
ms.openlocfilehash: 8f96fcc4aadcd8f082d55dc568b2517f048edaf2
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/13/2020
ms.locfileid: "73027189"
---
# <a name="porting-java-to-c-for-xamarinandroid"></a>Java를 Xamarin.Android용 C#로 이식

이 방법은 다음과 같은 조직에 유용할 수 있습니다.

- **기술 스택을 Java에서 C#로 전환 중인 조직.**
- **동일한 제품에 대해 C# 및 Java 버전을 유지해야 하는 조직.**
- **인기 있는 Java 라이브러리의 .NET 버전을 보유하기를 희망하는 조직.**

Java 코드를 C#로 이식하는 방법에는 두 가지가 있습니다. 첫 번째 방법은 수동으로 코드를 이식하는 것입니다. 여기에는 .NET과 Java를 모두 이해하고 각 언어에 적합한 관용구를 잘 알고 있는 숙련된 개발자가 필요합니다. 이 접근 방식은 코드가 소규모이거나 Java에서 C#로 완전히 이동하려는 조직에게 가장 적합합니다.

두 번째 이식 방법은 [Sharpen](https://github.com/mono/sharpen) 같은 코드 변환기를 사용하여 프로세스를 시도하고 자동화하는 것입니다. [Sharpen](https://github.com/mono/sharpen)은 원래 *db4o* 코드를 Java에서 C#로 이식하는 데 사용된 Versant의 오픈 소스 변환기입니다. db4o는 Versant가 Java로 개발했다가 .NET으로 이식한 개체 지향 데이터베이스입니다. 코드 변환기 사용은 두 언어 모두로 작성되고 둘 사이에 약간의 패리티가 필요한 프로젝트에 적합할 수 있습니다.

자동 코드 변환 도구의 사용이 적절한 예는 [ngit](https://github.com/mono/ngit) 프로젝트에서 찾을 수 있습니다.
Ngit은 Java 프로젝트 [jgit](https://eclipse.org/)의 한 포트입니다.
Jgit 자체는 [Git](https://git-scm.com/) 소스 코드 관리 시스템의 Java 구현입니다. Java에서 C# 코드를 생성하기 위해 ngit 프로그래머는 사용자 지정 자동 시스템을 사용하여 jgit에서 Java 코드를 추출하고, 변환 프로세스를 수용할 수 있도록 일부 패치를 적용한 다음, C# 코드를 생성하는 Sharpen을 실행합니다. 이렇게 하면 ngit 프로젝트가 jgit에서 지속되는 작업에서 혜택을 얻을 수 있습니다.

자동 코드 변환 도구를 부트스트랩하는 작업과 관련된 작업량이 적지 않은 경우가 자주 있으며 이는 하나의 장벽으로 작용할 수 있습니다. 대부분의 경우 수동으로 Java를 C#로 이식하는 것이 더 간단하고 쉬울 수 있습니다.

## <a name="related-links"></a>관련 링크

- [Sharpen 변환 도구](https://github.com/mono/sharpen)
