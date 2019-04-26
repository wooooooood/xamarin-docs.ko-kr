---
title: Java를 C#으로 이식
description: Java 소스 코드를 이식 하는 Java를 사용 하 여 Xamarin.Android 응용 프로그램에서 세 번째 옵션인 C#입니다.
ms.prod: xamarin
ms.assetid: 39E528BD-010F-47FC-BE48-8E7848E30454
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/05/2016
ms.openlocfilehash: 9beb6d59c9376a404c06af7f0cd1efd985929843
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61075148"
---
# <a name="porting-java-to-c"></a>Java를 C#으로 이식

_Java 소스 코드를 이식 하는 Java를 사용 하 여 Xamarin.Android 응용 프로그램에서 세 번째 옵션인 C#입니다._

## <a name="overview"></a>개요

이 방법은 조직에 도움이 될 수입니다.

-  **Java에서 기술 스택에 전환 하는 C#입니다.**
-  **유지 관리 해야 합니다는 C# 와 같은 제품의 Java 버전입니다.**
-  **인기 있는 Java 라이브러리의.NET 버전이 하려고 합니다.**


Java 코드를 이식 하는 방법은 두 가지 C#입니다. 첫 번째 방법은 수동으로 코드를 이식 하는 것입니다. 여기에.NET 및 Java 모두 이해 하 고 각 언어에 대 한 적절 한 코드를 잘 알고 있는 숙련 된 개발자가 포함 됩니다. 이 이렇게 하면 소량의 코드 또는 Java에서 완전히 탈피 하려는 조직에 가장 적합 하 게 C#입니다.

시도 같은 코드 변환기를 사용 하 여 프로세스를 자동화 하는 두 번째 이식 방법론 [선명 하 게](https://github.com/mono/sharpen)합니다. [단련](https://github.com/mono/sharpen) 는 오픈 소스 변환기에 대 한 코드를 이식 하려면 원래 사용 된 Versant에서 *db4o* Java에서 C#. db4o는 Versant java로 개발 하 고 다음.NET으로 이식 하는 개체 지향 데이터베이스를입니다. 코드 변환기를 사용 하 여 둘 간의 일부 패리티를 필요로 하는 두 언어 모두에 있어야 하는 프로젝트에 대 한 의미를 만들 수 있습니다.

경우는 자동화 된 코드 변환 도구는 것이 좋습니다 예가에서 볼 수 있습니다 합니다 [ngit](https://github.com/mono/ngit) 프로젝트입니다.
Java 프로젝트의 포트인 Ngit [jgit](http://eclipse.org/)합니다.
Jgit 자체는 Java로 구현 된 [Git](http://git-scm.com/) 소스 코드 관리 시스템입니다. 생성 C# 사용자 지정 자동화 jgit에서 Java 코드를 추출, 변환 프로세스를 수용 하기 위해 일부 패치를 적용 한 다음 선명 하 게를 생성 하는 실행 하는 시스템 ngit 프로그래머에 게 사용 하 여 Java에서 코드를 C# 코드입니다. 이렇게 하면 ngit 프로젝트 jgit에서 수행 되는 연속, 진행 중인 작업을 활용할 수 있습니다.

경우가 trivial이 아닌 양의 작업을 부트스트랩 하는 자동화 된 코드 변환 도구와 관련 된 및 사용에 대 한 장벽을 증명할 수 있습니다이 있습니다. 대부분의 경우에 것이 간단 하 고 포트 Java를 쉽게 C# 를 직접.



## <a name="related-links"></a>관련 링크

- [변환 도구 단련](https://github.com/mono/sharpen)
