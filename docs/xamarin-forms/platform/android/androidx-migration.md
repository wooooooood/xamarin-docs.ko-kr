---
title: Xamarin.ios의 AndroidX 마이그레이션
description: 이 문서에서는 AndroidX가 있는 이유와 Xamarin.ios 앱에서 AndroidX로 마이그레이션하는 방법에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: 98884003-E65A-4EB4-842D-66CFE27344A4
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 01/22/2020
ms.openlocfilehash: 13fb802dec326cdb82bac8825ca84343ef85b13e
ms.sourcegitcommit: 10b4d7952d78f20f753372c53af6feb16918555c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/26/2020
ms.locfileid: "77646655"
---
# <a name="androidx-migration-in-xamarinforms"></a>Xamarin.ios의 AndroidX 마이그레이션

AndroidX는 Android 지원 라이브러리를 대체 합니다. 이 문서에서는 AndroidX가 있는 이유와 Xamarin에 영향을 주는 방법 및 AndroidX 라이브러리를 사용 하도록 응용 프로그램을 마이그레이션하는 방법에 대해 설명 합니다.

## <a name="history-of-androidx"></a>AndroidX 기록

Android 지원 라이브러리는 이전 버전의 Android에서 최신 기능을 제공 하기 위해 만들어졌습니다. 이는 개발자가 모든 버전의 Android 운영 체제에 존재 하지 않을 수 있는 기능을 사용할 수 있도록 하 고 이전 버전을 대체 하는 데 사용할 수 있는 호환성 계층입니다. 지원 라이브러리에는 편리 하 고 도우미 클래스, 디버깅 및 유틸리티 도구, 기능을 제공 하는 다른 지원 라이브러리 클래스에 종속 된 정교한 클래스도 포함 되어 있습니다.

지원 라이브러리는 원래 단일 이진 이지만 최신 앱 개발에 거의 필요한 라이브러리 모음으로 성장 하 고 진화 하 고 있습니다. 지원 라이브러리에서 일반적으로 사용 되는 기능은 다음과 같습니다.

- `Fragment` 지원 클래스입니다.
- 긴 목록을 관리 하는 데 사용 되는 `RecyclerView`입니다.
- 65536 이상의 메서드를 사용 하는 앱에 대 한 Multidex 지원.
- `ActivityCompat` 클래스입니다.

AndroidX는 더 이상 유지 관리 되지 않는 지원 라이브러리를 대체 합니다. 모든 새 라이브러리 개발은 AndroidX 라이브러리에서 발생 합니다. AndroidX는 일반 응용 프로그램 아키텍처 패턴에 대 한 의미 체계 버전 관리, 더 명확한 패키지 이름 및 향상 된 지원 기능을 사용 하는 다시 디자인 된 라이브러리입니다. AndroidX 버전 1.0.0은 라이브러리 버전 28.0.0 지원에 해당 하는 이진 파일입니다. 지원 라이브러리에서 AndroidX로의 클래스 매핑 전체 목록은 developer.android.com에서 [라이브러리 클래스 매핑 지원](https://developer.android.com/jetpack/androidx/migrate/class-mappings) 을 참조 하세요.

Google은 AndroidX로 Jetifier 이라는 마이그레이션 프로세스를 만들었습니다. Jetifier은 빌드 프로세스 중에 jar 바이트 코드를 검사 하 고 앱 코드 및 종속성 모두에 대 한 지원 라이브러리 참조를 AndroidX에 해당 하는 값으로 다시 매핑합니다.

Android Java 앱에서와 마찬가지로 Xamarin Forms 앱에서 jar 종속성은 AndroidX로 마이그레이션해야 합니다. 그러나 Xamarin 바인딩만 올바른 기본 jar 파일을 가리키도록 마이그레이션해야 합니다. Xamarin.ios는 버전 4.5에서 자동 AndroidX 마이그레이션에 대 한 지원을 추가 했습니다.

AndroidX에 대 한 자세한 내용은 developer.android.com의 [androidx 개요](https://developer.android.com/jetpack/androidx) 를 참조 하세요.

## <a name="automatic-migration-in-xamarinforms"></a>Xamarin.ios에서 자동 마이그레이션

AndroidX로 자동으로 마이그레이션하려면 Xamarin.ios 프로젝트는 다음을 수행 해야 합니다.

- Android API 버전 29 이상을 대상으로 합니다.
- Xamarin.ios 버전 4.5 이상을 사용 합니다.

프로젝트에서 이러한 설정을 확인 한 후 Visual Studio 2019에서 Android 앱을 빌드합니다. 빌드 프로세스 중에 IL (중간 언어)이 검사 되 고 라이브러리 종속성이 지원 되며 바인딩은 AndroidX 종속성으로 교환 됩니다. 응용 프로그램에 빌드에 필요한 모든 AndroidX 종속성이 있는 경우 빌드 프로세스에 차이가 없다는 것을 알 수 있습니다.

> [!NOTE]
> 프로젝트에서 지원 라이브러리에 대 한 참조를 유지 해야 합니다. 이러한 기능은 마이그레이션 프로세스가 결과 IL을 검사 하 고 종속성을 변환 하기 전에 응용 프로그램을 컴파일하는 데 사용 됩니다.

프로젝트의 일부가 아닌 AndroidX 종속성이 검색 되 면 누락 된 AndroidX 패키지를 나타내는 빌드 오류가 보고 됩니다. 예제 빌드 오류는 다음과 같습니다.

```
Could not find 37 AndroidX assemblies, make sure to install the following NuGet packages:
- Xamarin.AndroidX.Lifecycle.LiveData
- Xamarin.AndroidX.Browser
- Xamarin.Google.Android.Material
- Xamarin.AndroidX.Legacy.Supportv4
You can also copy and paste the following snippit into your .csproj file:
 <PackageReference Include="Xamarin.AndroidX.Lifecycle.LiveData" Version="2.1.0-rc1" />
 <PackageReference Include="Xamarin.AndroidX.Browser" Version="1.0.0-rc1" />
 <PackageReference Include="Xamarin.Google.Android.Material" Version="1.0.0-rc1" />
 <PackageReference Include="Xamarin.AndroidX.Legacy.Support.V4" Version="1.0.0-rc1" />
```

누락 된 NuGet 패키지는 Visual Studio의 NuGet 패키지 관리자를 통해 설치 하거나, Android .csproj 파일을 편집 하 여 설치 하 여 오류에 나열 된 `PackageReference` XML 항목을 포함할 수 있습니다.

누락 된 패키지가 확인 되 면 프로젝트를 다시 빌드하면 누락 된 패키지가 로드 되 고 프로젝트는 라이브러리 종속성 지원 대신 AndroidX 종속성을 사용 하 여 컴파일됩니다.

> [!NOTE]
> 프로젝트 및 프로젝트 종속성이 Android 지원 라이브러리를 참조 하지 않는 경우 마이그레이션 프로세스는 아무 작업도 수행 하지 않으며 실행 되지 않습니다.

## <a name="related-links"></a>관련 링크

- Developer.android.com의 [Android 지원 라이브러리 개요](https://developer.android.com/topic/libraries/support-library/index)
- Developer.android.com의 [Androidx 개요](https://developer.android.com/jetpack/androidx)
