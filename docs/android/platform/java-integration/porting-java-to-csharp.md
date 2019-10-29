---
title: Xamarin Android 용 C# 으로 Java 포팅
description: Xamarin Android 응용 프로그램에서 Java를 사용 하는 세 번째 옵션은 Java 소스 코드를로 C#이식 하는 것입니다.
ms.prod: xamarin
ms.assetid: 39E528BD-010F-47FC-BE48-8E7848E30454
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 04/05/2016
ms.openlocfilehash: 8f96fcc4aadcd8f082d55dc568b2517f048edaf2
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73027189"
---
# <a name="porting-java-to-c-for-xamarinandroid"></a>Xamarin Android 용 C# 으로 Java 포팅

이 방법은 다음과 같은 조직에 유용할 수 있습니다.

- **기술 스택을 Java에서로 전환 하 C#고 있습니다.**
- **는 C# 동일한 제품의 및 Java 버전을 유지 관리 해야 합니다.**
- **인기 있는 Java 라이브러리의 .NET 버전을 사용 하고자 합니다.**

Java 코드를로 C#이식 하는 방법에는 두 가지가 있습니다. 첫 번째 방법은 수동으로 코드를 이식 하는 것입니다. 여기에는 .NET과 Java를 모두 이해 하는 숙련 된 개발자가 포함 되며, 각 언어에 적합 한 관용구 잘 알고 있습니다. 이 접근 방식은 적은 양의 코드 또는 Java에서로 C#완전히 이동 하려는 조직의 경우 가장 적합 합니다.

두 번째 포팅 방법은 [선명](https://github.com/mono/sharpen)하 고 같은 코드 변환기를 사용 하 여 프로세스를 자동화 하는 것입니다. [선명 효과](https://github.com/mono/sharpen) 는 원래 *Db4o* 코드를 Java에서로 C#이식 하는 데 사용 된 Versant의 오픈 소스 변환기입니다. db4o은 Java에서 Versant 개발 된 다음 .NET으로 이식 되는 개체 지향 데이터베이스입니다. 코드 변환기를 사용 하면 두 언어 모두에 있어야 하 고 둘 사이에 약간의 패리티가 필요한 프로젝트에 적합할 수 있습니다.

자동 코드 변환 도구를 사용 하는 경우의 예는 [ngit](https://github.com/mono/ngit) 프로젝트에서 볼 수 있습니다.
Ngit은 Java 프로젝트 [jgit](https://eclipse.org/)의 포트입니다.
Jgit 자체는 [Git](https://git-scm.com/) 소스 코드 관리 시스템의 Java 구현입니다. Java에서 C# 코드를 생성 하기 위해 ngit 프로그래머는 사용자 지정 자동화 된 시스템을 사용 하 여 Jgit에서 java 코드를 추출 하 고, 변환 프로세스를 수용할 수 있도록 일부 패치를 적용 한 C# 다음, 코드를 생성 하는 선명 효과를 실행 합니다. 이렇게 하면 ngit 프로젝트가 jgit에서 수행 되는 지속적인 지속적인 작업을 누릴 수 있습니다.

자동화 된 코드 변환 도구를 부트스트랩 하는 작업과 관련 된 작업의 양이 많지 않으며,이는 사용할 장벽 임을 입증할 수 있습니다. 대부분의 경우에 C# 는 Java를 직접 이식 하는 것이 더 간단 하 고 더 쉬울 수 있습니다.

## <a name="related-links"></a>관련 링크

- [선명 변환 도구](https://github.com/mono/sharpen)
