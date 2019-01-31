---
title: Android 빌드 오류 – The LinkAssemblies 작업이 예기치 않게 실패 했습니다
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: EB3BE685-CB72-48E3-89D7-C845E76B9FA2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
ms.openlocfilehash: b027dd23b9144a865bc16b55ebac71855bae0725
ms.sourcegitcommit: 817d26585093cd180a36b28179eb354b0eb900b3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55292040"
---
# <a name="android-build-error--the-linkassemblies-task-failed-unexpectedly"></a>Android 빌드 오류 – The LinkAssemblies 작업이 예기치 않게 실패 했습니다

오류 메시지가 표시 될 수 있습니다 `The "LinkAssemblies" task failed unexpectedly` Forms를 사용 하는 Xamarin.Android 프로젝트를 빌드할 경우. 링커 활성화 되어 있을 때 이런 (에서 일반적으로 *릴리스* 앱 패키지의 크기를 줄이기 위해 빌드); Android 대상을 최신 프레임 워크에 업데이트 되지 않은 때문에 발생 하 고 합니다. (자세한 정보: [Android 요구 사항에 대 한 Xamarin.Forms](~/get-started/installation.md#android))

이 문제를 해결 하 고 최신 지원 되는 Android SDK 버전을 설정 해야 하는 것은 **대상 프레임 워크** 하 **사용 하 여 가장 최근에 설치 된 플랫폼**합니다. 설정 하는 것이 좋습니다는 **대상 Android 버전** 에 **사용 하 여 대상 프레임 워크 버전** 하며 **최소 Android 버전** API 15 이상. 이것은 지원 되는 구성 합니다.

## <a name="setting-in-visual-studio-for-mac"></a>Mac 용 Visual Studio에서 설정

1.  Android 프로젝트를 마우스 오른쪽 단추로 클릭 합니다.
2.  로 이동 **빌드 > 일반 > 대상 프레임 워크**합니다.
3.  설정 된 **Framework를 대상으로 합니다. 사용 하 여 가장 최근에 설치 된 플랫폼**합니다.
4.  이동할 프로젝트 옵션에서 여전히 **빌드 > Android 응용 프로그램**합니다.
5.  설정 합니다 **최소 Android 버전** API 수준 15 이상으로 및 **대상 Android 버전** 하 **자동으로 사용 하 여 대상 프레임 워크 버전**합니다.

## <a name="setting-in-visual-studio"></a>Visual Studio에서 설정

1.  Android 프로젝트를 마우스 오른쪽 단추로 클릭 합니다.
2.  로 이동 **응용 프로그램** 프로젝트 옵션에서 합니다.
3.  설정 합니다 **Android 버전을 사용 하 여 컴파일하고** & **대상 Android 버전** 설정을 **사용 하 여 최신 플랫폼** / **사용 SDK 버전을 사용 하 여 컴파일**합니다.
4.  설정 된 **최소 Android 대상** API 19 이상 설정 합니다.

이러한 설정을 업데이트 한 후 정리 하 고 변경 내용을 선택 하도록 프로젝트를 다시 작성 하십시오.
