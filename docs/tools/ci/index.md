---
title: Xamarin을 사용한 연속 통합 소개
description: 이 문서는 Xamarin과의 연속 통합을 설명 하는 가이드로 연결 됩니다. 연결 된 콘텐츠는 연속 통합에 대 한 개요를 제공 하 고 App Center 빌드, TeamCity 및 Jenkins에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: 99484E96-DC69-4697-8BBB-1B44C5CBB5ED
author: conceptdev
ms.author: crdun
ms.date: 10/23/2018
ms.openlocfilehash: 6e1d90152fa47fef0638c93777f1e7179e97e387
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70292548"
---
# <a name="continuous-integration-with-xamarin"></a>Xamarin을 사용한 연속 통합

> [!Video https://youtube.com/embed/wXgnh2Q7Uv8]

## <a name="introduction-to-continuous-integrationtoolsciintro-to-cimd"></a>[연속 통합 소개](~/tools/ci/intro-to-ci.md)

이 섹션에서는 연속 통합 및 해당 관계와 관련 된 다양 한 구성 요소에 대해 설명 합니다. 아래 특정 섹션에서 설명 하는 연속 통합 환경을 간략하게 설명 합니다.

## <a name="devops-with-xamarintoolscidevopsmd"></a>[Xamarin을 사용 하는 DevOps](~/tools/ci/devops.md)

이 섹션에서는 Xamarin 프로젝트에서 제대로 작동 하는 것으로 예측할 수 있는 Azure 및 Visual Studio의 DevOps 기능을 식별 합니다.

## <a name="working-with-continuous-integration-environments"></a>연속 통합 환경 사용

### <a name="build-xamarin-apps-with-azure-pipelineshttpsdocsmicrosoftcomazuredevopspipelineslanguagesxamarin"></a>[Azure Pipelines를 사용 하 여 Xamarin 앱 빌드](https://docs.microsoft.com/azure/devops/pipelines/languages/xamarin/)

Azure Pipelines를 사용 하 여 Android 및 iOS 용 Xamarin 앱을 자동으로 빌드합니다.

### <a name="build-xamarin-apps-using-app-centerhttpsdocsmicrosoftcomappcenterbuildxamarin"></a>[App Center를 사용 하 여 Xamarin 앱 빌드](https://docs.microsoft.com/appcenter/build/xamarin/)

App Center를 사용 하 여 GitHub, Azure DevOps 또는 Bitbucket를 사용 하 여 Xamarin.ios 및 Xamarin Android 솔루션을 빌드합니다.

### <a name="build-xamarin-apps-with-teamcitytoolsciteamcitymd"></a>[TeamCity를 사용 하 여 Xamarin 앱 빌드](~/tools/ci/teamcity.md)

이 가이드에서는 TeamCity를 사용 하 여 모바일 앱을 컴파일한 다음 App Center 테스트에 제출 하는 단계를 설명 합니다.

### <a name="build-xamarin-apps-with-jenkinstoolscijenkins-walkthroughmd"></a>[Jenkins를 사용 하 여 Xamarin 앱 빌드](~/tools/ci/jenkins-walkthrough.md)

이 가이드에서는 Jenkins를 연속 통합 서버로 설정 하 고 Xamarin을 사용 하 여 만든 모바일 앱 컴파일을 자동화 하는 방법을 보여 줍니다. OS X에 Jenkins을 설치 하 고, 구성 하 고, 변경 내용이 버전 제어 시스템으로 커밋될 때 Xamarin.ios 및 Xamarin Android 앱을 컴파일하도록 작업을 설정 하는 방법을 설명 합니다.
