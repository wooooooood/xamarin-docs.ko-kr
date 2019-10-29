---
title: 패키지 마모 앱
ms.prod: xamarin
ms.assetid: E32DD855-78DD-46F8-B234-4EAC0756BDA2
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/02/2018
ms.openlocfilehash: aa4a4f1ab3ae3024de2d969f9325c2efa4db48af
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028641"
---
# <a name="packaging-wear-apps"></a>패키지 마모 앱

Android 마모 된 앱은 Google Play에 배포할 수 있도록 전체 Android 앱으로 패키지 됩니다. 

## <a name="automatic-packaging"></a>자동 패키징

Xamarin Android 5.0부터 사용자가 앱을 휴대 하는 프로젝트에 대 한 프로젝트 참조를 만들 때 앱이 자동으로 핸드헬드 앱에 리소스로 패키지 됩니다. 다음 단계를 사용 하 여이 연결을 만들 수 있습니다. 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. 사용자의 앱이 핸드헬드 솔루션에 아직 포함 되지 않은 경우 솔루션 노드를 마우스 오른쪽 단추로 클릭 하 고 **추가 > 기존 프로젝트 추가**...를 선택 합니다.

2. 마모 된 앱의 **.csproj** 파일로 이동 하 여 선택 하 고 **열기**를 클릭 합니다. 이제 마모 된 앱 프로젝트가 핸드헬드 솔루션에 표시 됩니다.

3. **참조** 노드를 마우스 오른쪽 단추로 클릭 하 고 **참조 추가**를 선택 합니다.

4. **참조 관리자** 대화 상자에서 사용자의 마모 프로젝트 (확인 표시를 추가 하려면 클릭)를 사용 하도록 설정 하 고 **확인**을 클릭 합니다.

5. 사용자의 프로젝트에 대 한 패키지 이름이 핸드헬드 프로젝트의 패키지 이름과 일치 하도록 변경 합니다. 패키지 이름은 **속성 > Android Manifest**에서 변경할 수 있습니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. 사용자의 앱이 핸드헬드 솔루션에 아직 포함 되지 않은 경우 솔루션 노드를 마우스 오른쪽 단추로 클릭 하 고 **추가 > 기존 프로젝트 추가**...를 선택 합니다.

2. 마모 된 앱의 **.csproj** 파일로 이동 하 여 선택 하 고 **열기**를 클릭 합니다. 이제 마모 된 앱 프로젝트가 핸드헬드 솔루션에 표시 됩니다.

3. 솔루션에서 핸드헬드 프로젝트 노드를 마우스 오른쪽 단추로 클릭 하 고 **참조 편집**...을 클릭 합니다.

4. **참조 편집** 대화 상자에서 사용자의 마모 프로젝트를 사용 하도록 설정 합니다 (확인 표시를 추가 하려면 클릭). 그런 다음 **확인**을 클릭 합니다.

5. 사용자가 만든 프로젝트의 패키지 이름과 일치 하도록 사용자의 패키지 이름 변경 ( **Android 응용 프로그램 > 프로젝트 옵션**에서 패키지 이름 변경 가능)

-----

마모 된 앱의 패키지 이름이 핸드헬드 앱의 패키지 이름과 일치 하지 않는 경우 **XA5211** 오류가 발생 합니다. 예를 들면,

```shell
Error XA5211: Embedded wear app package name differs from handheld 
app package name (com.companyname.mywearapp != com.companyname.myapp). (XA5211)
```

이 오류를 해결 하려면 핸드헬드 앱의 패키지 이름과 일치 하도록 마모 된 앱의 패키지 이름을 변경 합니다.

**빌드 > 모두 빌드**를 클릭 하면이 연결은 기본 핸드헬드 (Phone) 프로젝트에 대 한 마모 프로젝트의 자동 패키징을 트리거합니다. 마모 된 앱은 자동으로 빌드되고 핸드헬드 앱에 리소스로 포함 됩니다.

마모 된 앱 프로젝트가 생성 하는 어셈블리는 핸드헬드 (Phone) 프로젝트에서 어셈블리 참조로 사용 되지 않습니다. 대신, 빌드 프로세스에서 다음을 수행 합니다.

- 패키지 이름이 일치 하는지 확인 합니다. 

- XML을 생성 하 고이를 휴대용 앱에 연결 하기 위해 핸드헬드 프로젝트에 추가 합니다. 예를 들면, 

    ```xml
    <!-- Handheld (Phone) Project.csproj -->
    <ProjectReference Include="..\MyWearApp\MyWearApp.csproj">
        <Project>{D80E1FEF-653B-448C-B2AA-609C74E88340}</Project>
        <Name>MyWearApp</Name>
        <IsAppExtension>True</IsAppExtension>
    </ProjectReference>
    ```

- 푸시 응용 프로그램을 Hpc 프로젝트에 **원시** 리소스로 추가 합니다. 

## <a name="manual-packaging"></a>수동 패키징

Android 용 앱은 버전 5.0 이전에 Xamarin.ios에서 작성할 수 있지만 앱을 배포 하려면 다음 수동 패키징 지침을 따라야 합니다. 

1. Wearable 프로젝트와 핸드헬드 (Phone) 프로젝트의 버전 번호와 패키지 이름이 동일한 지 확인 합니다.

2. Wearable 프로젝트를 **릴리스** 빌드로 수동으로 빌드합니다.

3. 수동으로 릴리스를 추가 **합니다.** (2)에서 핸드헬드 (전화) 프로젝트의 **리소스/원시** 디렉터리로 apk를 실행 합니다.

4. Wearable **Apk** 의 단계 (3)를 참조 하는 핸드헬드 프로젝트에서 새 Xml 리소스 **리소스/x m l/wearable_app_desc** 을 수동으로 추가 합니다.

    ```xml
    <wearableApp package="wearable.app.package.name">
        <versionCode>1</versionCode>
        <versionName>1.0</versionName>
        <rawPathResId>NAME_OF_APK_FROM_STEP_3</rawPathResId>
    </wearableApp>
    ```

5. 새 XML 리소스를 참조 하는 핸드헬드 프로젝트의 **Androidmanifest** `<application>` 요소에 `<meta-data />` 요소를 수동으로 추가 합니다.

    ```xml
    <meta-data android:name="com.google.android.wearable.beta.app"
        android:resource="@xml/wearable_app_desc"/>
    ```

Android 개발자 사이트의 [manual packging 지침](https://developer.android.com/training/wearables/apps/packaging.html#PackageManually)도 참조 하세요.
