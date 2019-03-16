---
title: Xamarin 사용 하 여 연속 통합 소개
description: '이 문서는 Xamarin 사용 하 여 연속 통합을 설명 하는 지침에 연결 합니다. 연결 된 콘텐츠는 연속 통합의 개요를 제공 하 고 App Center 빌드, TeamCity, 및 Jenkins를 설명 합니다.'
ms.prod: xamarin
ms.assetid: 99484E96-DC69-4697-8BBB-1B44C5CBB5ED
author: lobrien
ms.author: laobri
ms.date: 10/23/2018
---

# <a name="continuous-integration-with-xamarin"></a>Xamarin 사용 하 여 연속 통합

> [!Video https://youtube.com/embed/wXgnh2Q7Uv8]

## <a name="introduction-to-continuous-integrationtoolsciintro-to-cimd"></a>[연속 통합 소개](~/tools/ci/intro-to-ci.md)

이 섹션에서는 지속적인 통합 및 해당 관계와 관련 된 다양 한 구성 요소에 설명 합니다. 이 특정 섹션 아래에서 설명 하는 연속 통합 환경을 간략하게 설명 합니다.

## <a name="devops-with-xamarintoolscidevopsmd"></a>[Xamarin 사용한 DevOps](~/tools/ci/devops.md)

이 섹션에서는 Azure 및 Visual Studio는 Xamarin 프로젝트에서 작동 하도록 원하는 수의 DevOps 기능 식별 합니다.

## <a name="working-with-continuous-integration-environments"></a>연속 통합 환경 사용

### <a name="build-xamarin-apps-with-azure-pipelineshttpsdocsmicrosoftcomazuredevopspipelineslanguagesxamarin"></a>[Azure 파이프라인을 사용 하 여 Xamarin 앱 빌드](https://docs.microsoft.com/azure/devops/pipelines/languages/xamarin/)

자동으로 Android 및 iOS 용 Xamarin 앱을 빌드하려면 Azure 파이프라인을 사용 합니다.

### <a name="build-xamarin-apps-using-app-centerhttpsdocsmicrosoftcomappcenterbuildxamarin"></a>[App Center를 사용 하 여 Xamarin 앱 빌드](https://docs.microsoft.com/appcenter/build/xamarin/)

Azure DevOps, GitHub, Bitbucket에서 바로 App Center를 사용 하 여 Xamarin.iOS 및 Xamarin.Android 솔루션을 빌드하십시오.

### <a name="build-xamarin-apps-with-teamcitytoolsciteamcitymd"></a>[TeamCity 사용 하 여 Xamarin 앱 빌드](~/tools/ci/teamcity.md)

이 가이드에서는 모바일 앱을 컴파일 및 App Center test 제출해 TeamCity 사용과 관련 된 단계를 설명 합니다.

### <a name="build-xamarin-apps-with-jenkinstoolscijenkins-walkthroughmd"></a>[Jenkins 사용 하 여 Xamarin 앱 빌드](~/tools/ci/jenkins-walkthrough.md)

이 가이드에서는 Jenkins 연속 통합 서버로 설정 및 Xamarin을 사용 하 여 만든 모바일 앱을 컴파일 자동화 하는 방법을 보여 줍니다. OS X에서 Jenkins를 설치, 구성 변경 내용을 버전 제어 시스템에 커밋된 경우 Xamarin.iOS 및 Xamarin.Android 앱을 컴파일하는 데 작업을 설정 하는 방법을 설명 합니다.
