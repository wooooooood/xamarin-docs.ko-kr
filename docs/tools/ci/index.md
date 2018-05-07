---
title: Xamarin 사용한 연속 통합 소개
description: 연속 통합은 자동화 된 빌드 컴파일 및 코드를 추가 하거나 프로젝트의 버전 제어 리포지토리에 개발자가 변경한 경우 필요에 따라 앱을 테스트 하는 소프트웨어 엔지니어링 좋습니다. 이 문서는 연속 통합의 일반적인 개념 및 옵션 중 일부는 사용할 수 있는 연속 통합에 대 한 Xamarin 프로젝트와 함께 설명 합니다.
ms.prod: xamarin
ms.assetid: 99484E96-DC69-4697-8BBB-1B44C5CBB5ED
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 05/04/2017
ms.openlocfilehash: 54f3d3c475e506e7d451af5125e90a0f51aa7374
ms.sourcegitcommit: c9ebf456e1c6924956bedb13f4ea78ff09f7b1a0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/04/2018
---
# <a name="introduction-to-continuous-integration-with-xamarin"></a>Xamarin 사용한 연속 통합 소개

_연속 통합은 자동화 된 빌드 컴파일 및 코드를 추가 하거나 프로젝트의 버전 제어 리포지토리에 개발자가 변경한 경우 필요에 따라 앱을 테스트 하는 소프트웨어 엔지니어링 좋습니다. 이 문서는 연속 통합의 일반적인 개념 및 옵션 중 일부는 사용할 수 있는 연속 통합에 대 한 Xamarin 프로젝트와 함께 설명 합니다._

> [!Video https://youtube.com/embed/wXgnh2Q7Uv8]


##  <a name="introduction-to-continuous-integrationtoolsciintro-to-cimd"></a>[연속 통합 소개](~/tools/ci/intro-to-ci.md)

이 섹션에서는 연속 통합 및 테이블 간의 관계와 관련 된 서로 다른 구성 요소에 설명 합니다. 아래 특정 섹션에서 설명 하는 연속 통합 환경을 설명 합니다.

[!include[](~/tools/ci/includes/firewall-information.md)]

## <a name="working-with-continuous-integration-environments"></a>연속 통합 환경 작업


### <a name="using-app-center-build-with-xamarinappcenterbuildxamarin"></a>[Xamarin에서 App Center 빌드 사용](/appcenter/build/xamarin/)

GitHub, VSTS, 또는 Bitbucket에서 직접 앱 센터를 사용 하 여 Xamarin.iOS 및 Xamarin.Android 솔루션을 빌드하십시오.

### <a name="using-teamcity-with-xamarintoolsciteamcitymd"></a>[Xamarin에서 TeamCity 사용](~/tools/ci/teamcity.md)

이 가이드를 모바일 응용 프로그램을 컴파일하고 다음 센터 테스트 응용 프로그램에 전송할 TeamCity 사용과 관련 된 단계를 설명 합니다.

### <a name="using-jenkins-with-xamarintoolscijenkins-walkthroughmd"></a>[Xamarin에서 Jenkins 사용](~/tools/ci/jenkins-walkthrough.md)

이 가이드에는 Jenkins 연속 통합 서버를 설정 하 고 컴파일 Xamarin을 사용 하 여 만든 모바일 응용 프로그램을 자동화 하는 방법을 보여 줍니다. Jenkins OS x 설치, 구성, 및 버전 제어 시스템에 변경 내용이 커밋됩니다 때 Xamarin.iOS 및 Xamarin.Android 응용 프로그램을 컴파일할 수 작업을 설정 하는 방법을 설명 합니다.
