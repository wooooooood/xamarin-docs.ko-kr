---
title: "C# Java 이식"
description: "Xamarin.Android 응용 프로그램에서 Java를 사용 하기 위한 세 번째 방법은 Java 소스 코드를 C# 이식 하 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 39E528BD-010F-47FC-BE48-8E7848E30454
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/05/2016
ms.openlocfilehash: bf881861eec0b28e59704253c3dab3f4e5dbae46
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="porting-java-to-c"></a>C# Java 이식

_Xamarin.Android 응용 프로그램에서 Java를 사용 하기 위한 세 번째 방법은 Java 소스 코드를 C# 이식 하 합니다._

## <a name="overview"></a>개요

이 방법을 조직에 도움이 될 수입니다.

-  **로 전환 하는 기술 스택을 Java에서 C#입니다.**
-  **C#과 같은 제품의 Java 버전을 유지 해야 합니다.**
-  **인기 있는 Java 라이브러리의.NET 버전을 두려는 합니다.**


Java 코드를 C# 이식 하는 방법은 두 가지가 있습니다. 첫 번째 방법은 수동으로 코드를 이식 하는 것입니다. .NET 및 Java를 이해 하 고 각 언어에 대 한 적절 한 관용구에 잘 알고 있는 숙련 된 개발자가 포함 됩니다. 이 이렇게 하면 적은 양의 코드에 또는 C#에서 Java 완전히 전환 하는 조직에 가장 적합 하 게 합니다.

두 번째 포팅 방법론 시도 하 고와 같은 코드 변환기를 사용 하 여 프로세스를 자동화 하는 것 [선명](https://github.com/mono/sharpen)합니다. [선명 하 게](https://github.com/mono/sharpen) 는 오픈 소스 변환기에 대 한 코드를 이식 하 데 사용 된 원래 Versant에서 *db4o* Java와 C#에서. db4o는 Versant java를 개발 하 고 다음.NET 이식 하는 개체 지향 데이터베이스를입니다. 코드 변환기를 사용 하면 언어 모두에 존재 해야 하는 고 둘 사이의 몇 가지 패리티를 해야 하는 프로젝트에 대 한 됩니다.

경우 자동화 된 코드 변환 도구는 것이 좋습니다의 예를 볼 수는 [ngit](https://github.com/mono/ngit) 프로젝트.
Ngit은 Java 프로젝트의 포트 [jgit](http://eclipse.org/)합니다.
Jgit 자체는의 Java로 구현 된 [Git](http://git-scm.com/) 소스 코드 관리 시스템입니다. Java에서 C# 코드를 생성 하려면 ngit 프로그래머 jgit에서 Java 코드를 추출, 변환 프로세스를 수용 하기 위해 일부 패치를 적용 한 다음 선명 C# 코드를 생성 하는 메시지를 실행 하는 사용자 지정 자동화 된 시스템을 사용 합니다. 이렇게 하면 ngit 프로젝트를 jgit에 의해 이루어진다는 continuous, 진행 중인 작업을 활용할 수 있습니다.

종종 특수 양의 작업 부트스트랩 하는 코드의 자동된 변환 도구와 관련 된 이며이 하는 데 장애가 될 증명할 수 있습니다. 대부분의 경우에서 수도 있습니다 간단 하 고 보다 쉽게 C# Java 포트 수를 직접.



## <a name="related-links"></a>관련 링크

- [변환 도구를 선명 하 게](https://github.com/mono/sharpen)
